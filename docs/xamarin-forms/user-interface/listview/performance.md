---
title: Производительность элемента управления ListView
description: Несмотря на то, что ListView является мощным представление для отображения данных, он имеет некоторые ограничения. В этой статье объясняется, как обеспечить высокую производительность с ListView Xamarin.Forms в приложении.
ms.prod: xamarin
ms.assetid: 1B085639-652C-4862-86EB-5D55D32B9395
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/11/2017
ms.openlocfilehash: ed920db129d2af203046a648c0069580f99a295e
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/13/2020
ms.locfileid: "79306443"
---
# <a name="listview-performance"></a>Производительность ListView

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistviewnative)

При написании приложений для мобильных устройств, производительность имеет значение. Пользователи привыкли плавную прокрутку и время быстрой загрузки. Несоответствие требованиям ожиданиям пользователей будет стоить вам оценки в хранилище приложения или в случае с бизнес-приложением издержек время и деньги.

[`ListView`](xref:Xamarin.Forms.ListView) Xamarin. Forms является мощным представлением для отображения данных, но имеет некоторые ограничения. Производительность прокрутки может снизиться при использовании пользовательских ячеек, особенно если они содержат глубоко вложенные иерархии представлений или используют определенные макеты, требующие сложного измерения. К счастью существуют методы, которые можно использовать, чтобы избежать снижения производительности.

## <a name="caching-strategy"></a>Стратегия кэширования

ListView часто используется для отображения гораздо большего объема данных, чем на экране. Например, музыкальное приложение может иметь библиотеку песен с тысячами записей. Создание элемента для каждой записи приведет к лишнему потреблению ценной памяти и будет работать плохо. Для постоянного создания и уничтожения строк приложение должно постоянно создавать и очищать объекты, что также будет работать плохо.

Для экономии памяти собственные [`ListView`](xref:Xamarin.Forms.ListView) эквиваленты для каждой платформы имеют встроенные функции для повторного использования строк. Только ячейки, видимые на экране, загружаются в память, а **содержимое** загружается в существующие ячейки. Этот шаблон запрещает приложению создавать экземпляры тысяч объектов, экономя время и память.

Xamarin. Forms позволяет повторно использовать [`ListView`](xref:Xamarin.Forms.ListView) ячейки через перечисление [`ListViewCachingStrategy`](xref:Xamarin.Forms.ListViewCachingStrategy) , которое имеет следующие значения:

```csharp
public enum ListViewCachingStrategy
{
    RetainElement,   // the default value
    RecycleElement,
    RecycleElementAndDataTemplate
}
```

> [!NOTE]
> Универсальная платформа Windows (UWP) игнорирует стратегию кэширования [`RetainElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RetainElement) , поскольку она всегда использует кэширование для повышения производительности. Таким образом, по умолчанию он ведет себя так, как если бы стратегия кэширования [`RecycleElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) была применена.

### <a name="retainelement"></a>RetainElement

Стратегия кэширования [`RetainElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RetainElement) указывает, что [`ListView`](xref:Xamarin.Forms.ListView) создаст ячейку для каждого элемента в списке и является поведением `ListView` по умолчанию. Его следует использовать в следующих случаях:

- Каждая ячейка имеет большое количество привязок (20 – 30 +).
- Шаблон ячейки часто меняется.
- Тестирование показывает, что стратегия кэширования `RecycleElement` приводит к снижению скорости выполнения.

Важно понимать последствия стратегии кэширования [`RetainElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RetainElement) при работе с пользовательскими ячейками. Код для инициализации ячейки будет нужно запускать для создания каждой ячейки, которое может иметь несколько раз в секунду. В этом случае приемы макета, которые хорошо были на странице, например использование нескольких вложенных экземпляров [`StackLayout`](xref:Xamarin.Forms.StackLayout) , становятся узким местом производительности при настройке и уничтожении в режиме реального времени по мере прокрутки пользователем.

### <a name="recycleelement"></a>RecycleElement

Стратегия кэширования [`RecycleElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) указывает, что [`ListView`](xref:Xamarin.Forms.ListView) будет пытаться уменьшить объем памяти и скорость выполнения, перезапуская ячейки списка. Этот режим не всегда обеспечивает повышение производительности, поэтому для определения каких-либо улучшений необходимо выполнить тестирование. Однако это предпочтительный вариант, и его следует использовать в следующих случаях.

- Каждая ячейка имеет небольшое и умеренное количество привязок.
- [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) каждой ячейки определяет все данные ячейки.
- Каждая ячейка почти похожа, при этом шаблон ячейки не изменяется.

Во время виртуализации ячейка будет иметь контекст привязки обновлены и поэтому если приложение использует этот режим его необходимо убедиться, правильно обрабатывать обновления контекста привязки. Все данные о ячейке должны поступать из контекста привязки, или, возможно возникновение ошибок согласованности. Эту проблему можно избежать с помощью привязки данных для вывода данных ячейки. Кроме того, данные ячеек должны быть заданы в `OnBindingContextChanged` переопределении, а не в конструкторе пользовательской ячейки, как показано в следующем примере кода:

```csharp
public class CustomCell : ViewCell
{
    Image image = null;
    
    public CustomCell ()
    {
        image = new Image();
        View = image;
    }
    
    protected override void OnBindingContextChanged ()
    {
        base.OnBindingContextChanged ();
        
        var item = BindingContext as ImageItem;
        if (item != null) {
            image.Source = item.ImageUrl;
        }
    }
}
```

Дополнительные сведения см. в разделе [изменение контекста привязки](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#binding-context-changes).

В iOS и Android Если ячейки использовать пользовательские модули подготовки отчетов, они необходимо убедиться, что правильно реализуется уведомления об изменении свойства. При повторном использовании ячеек значения их свойств изменятся при обновлении контекста привязки до значения доступной ячейки с `PropertyChanged` событиями. Дополнительные сведения см. [в разделе Настройка ViewCell](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md).

#### <a name="recycleelement-with-a-datatemplateselector"></a>RecycleElement с DataTemplateSelector

Если [`ListView`](xref:Xamarin.Forms.ListView) использует [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) для выбора [`DataTemplate`](xref:Xamarin.Forms.DataTemplate), стратегия кэширования [`RecycleElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) не кэширует `DataTemplate`s. Вместо этого для каждого элемента данных в списке выбирается `DataTemplate`.

> [!NOTE]
> Стратегия кэширования [`RecycleElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) имеет предварительные требования, появившиеся в Xamarin. forms 2,4, что при запросе [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) выбора [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) , каждый `DataTemplate` должен возвращать один и тот же тип [`ViewCell`](xref:Xamarin.Forms.ViewCell) . Например, при наличии [`ListView`](xref:Xamarin.Forms.ListView) с `DataTemplateSelector`, который может возвращать либо `MyDataTemplateA` (где `MyDataTemplateA` возвращает `ViewCell` типа `MyViewCellA`), либо `MyDataTemplateB` (где `MyDataTemplateB` возвращает `ViewCell` типа `MyViewCellB`), когда возвращается `MyDataTemplateA`, он должен возвращать `MyViewCellA`, иначе будет вызвано исключение.

### <a name="recycleelementanddatatemplate"></a>RecycleElementAndDataTemplate

Стратегия кэширования [`RecycleElementAndDataTemplate`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElementAndDataTemplate) основана на стратегии кэширования [`RecycleElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) , а также гарантирует, что если [`ListView`](xref:Xamarin.Forms.ListView) использует [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) для выбора [`DataTemplate`](xref:Xamarin.Forms.DataTemplate), `DataTemplate`s кэшируется типом элемента в списке. Таким образом, `DataTemplate`s выбираются один раз для каждого типа элемента, а не один раз для каждого экземпляра элемента.

> [!NOTE]
> Стратегия кэширования [`RecycleElementAndDataTemplate`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElementAndDataTemplate) имеет предварительные требования, которые `DataTemplate`s, возвращаемые [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) , должны использовать конструктор [`DataTemplate`](xref:Xamarin.Forms.DataTemplate.%23ctor(System.Type)) , который принимает `Type`.

### <a name="set-the-caching-strategy"></a>Настройка стратегии кэширования

Значение перечисления [`ListViewCachingStrategy`](xref:Xamarin.Forms.ListViewCachingStrategy) задается с помощью перегрузки конструктора [`ListView`](xref:Xamarin.Forms.ListView) , как показано в следующем примере кода:

```csharp
var listView = new ListView(ListViewCachingStrategy.RecycleElement);
```

В XAML задайте атрибут `CachingStrategy`, как показано в XAML ниже:

```xaml
<ListView CachingStrategy="RecycleElement">
    <ListView.ItemTemplate>
        <DataTemplate>
            <ViewCell>
              ...
            </ViewCell>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

Этот метод оказывает тот же результат, что и установка аргумента стратегии кэширования в C#конструкторе в.

#### <a name="set-the-caching-strategy-in-a-subclassed-listview"></a>Настройка стратегии кэширования в ListView с подклассом

Установка атрибута `CachingStrategy` из XAML в подклассе [`ListView`](xref:Xamarin.Forms.ListView) не приведет к желаемому поведению, так как для `ListView`отсутствует свойство `CachingStrategy`. Кроме того, если [XAMLC](~/xamarin-forms/xaml/xamlc.md) включен, будет выдаваться следующее сообщение об ошибке: **для "качингстратеги" не найдено свойство, связываемое свойство или событие** .

Решением этой проблемы является указание конструктора в подклассе [`ListView`](xref:Xamarin.Forms.ListView) , который принимает параметр [`ListViewCachingStrategy`](xref:Xamarin.Forms.ListViewCachingStrategy) и передает его в базовый класс:

```csharp
public class CustomListView : ListView
{
    public CustomListView (ListViewCachingStrategy strategy) : base (strategy)
    {
    }
    ...
}
```

Затем значение перечисления [`ListViewCachingStrategy`](xref:Xamarin.Forms.ListViewCachingStrategy) можно указать из XAML с помощью синтаксиса `x:Arguments`:

```xaml
<local:CustomListView>
    <x:Arguments>
        <ListViewCachingStrategy>RecycleElement</ListViewCachingStrategy>
    </x:Arguments>
</local:CustomListView>
```

## <a name="listview-performance-suggestions"></a>Предложения по повышению производительности ListView

Существует множество методов повышения производительности `ListView`. Следующие рекомендации могут повысить производительность ListView

- Привяжите свойство `ItemsSource` к коллекции `IList<T>`, а не к коллекции `IEnumerable<T>`, так как коллекции `IEnumerable<T>` не поддерживают произвольный доступ.
- Используйте встроенные ячейки (например, `TextCell` / `SwitchCell`), а не `ViewCell` каждый раз, когда это возможно.
- Используйте меньшее число элементов. Например, можно использовать одну `FormattedString` метку вместо нескольких меток.
- Замените `ListView` `TableView` при отображении неоднородных данных, то есть данных различных типов.
- Ограничьте использование метода [`Cell.ForceUpdateSize`](xref:Xamarin.Forms.Cell.ForceUpdateSize) . Поскольку вызовет перегрузку, он снизится производительность.
- В Android Избегайте установки `ListView`видимости разделителя строк или цвета после создания экземпляра, так как это приводит к значительному снижению производительности.
- Избегайте изменения макета ячейки на основе [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext). Изменение макета влечет за собой большие затраты на измерение и инициализацию.
- Избегайте иерархий глубоко вложенных макетов. Чтобы сократить число вложений, используйте `AbsoluteLayout` или `Grid`.
- Старайтесь не использовать конкретные `LayoutOptions`, кроме `Fill` (`Fill` является самым выгодным для вычислений).
- Старайтесь не размещать `ListView` в `ScrollView` по следующим причинам.
  - `ListView` реализует собственную прокрутку.
  - `ListView` не будут получать никакие жесты, так как они будут обрабатываться родительским `ScrollView`.
  - `ListView` может представлять настраиваемый верхний и нижний колонтитулы, которые прокручивается с элементами списка, потенциально предлагая функциональные возможности, для которых используется `ScrollView`. Дополнительные сведения см. в разделе [верхние и нижние колонтитулы](~/xamarin-forms/user-interface/listview/customizing-list-appearance.md#headers-and-footers).
- Рассмотрите пользовательский модуль подготовки отчетов, если вам нужна конкретная, сложная структура, представленная в ячейках.

`AbsoluteLayout` может выполнять макеты без единичного вызова меры, что делает ее высокопроизводительной. Если `AbsoluteLayout` нельзя использовать, рассмотрите возможность [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout). При использовании `RelativeLayout`непосредственное передаваемые ограничения будут выполняться значительно быстрее, чем с помощью API Expression. Этот метод выполняется быстрее, поскольку API выражений использует JIT, а в iOS дерево необходимо интерпретировать, что является медленным. API выражений подходит для макетов страниц, где он необходим только для первоначального макета и вращения, но в `ListView`, где он постоянно запускается во время прокрутки, он вредит производительность.

Создание пользовательского модуля подготовки отчетов для [`ListView`](xref:Xamarin.Forms.ListView) или его ячеек является одним из подходов к уменьшению влияния вычисления макета на производительность при прокрутке. Дополнительные сведения см. в разделе [Настройка ListView](~/xamarin-forms/app-fundamentals/custom-renderer/listview.md) и [Настройка ViewCell](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md).

## <a name="related-links"></a>Связанные ссылки

- [Настраиваемое представление модуля подготовки отчетов (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistviewnative)
- [Пользовательский модуль подготовки к просмотру ViewCell (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-viewcell)
- [листвиевкачингстратеги](xref:Xamarin.Forms.ListViewCachingStrategy)
