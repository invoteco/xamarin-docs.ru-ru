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
ms.sourcegitcommit: a5ef4497db04dfa016865bc7454b3de6ff088554
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/15/2019
ms.locfileid: "70998182"
---
# <a name="listview-performance"></a>Производительность ListView

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistviewnative)

При написании приложений для мобильных устройств, производительность имеет значение. Пользователи привыкли плавную прокрутку и время быстрой загрузки. Несоответствие требованиям ожиданиям пользователей будет стоить вам оценки в хранилище приложения или в случае с бизнес-приложением издержек время и деньги.

Xamarin. Forms [`ListView`](xref:Xamarin.Forms.ListView) — это мощное представление для отображения данных, но есть некоторые ограничения. Производительность прокрутки может снизиться при использовании пользовательских ячеек, особенно если они содержат глубоко вложенные иерархии представлений или используют определенные макеты, требующие сложного измерения. К счастью существуют методы, которые можно использовать, чтобы избежать снижения производительности.

## <a name="caching-strategy"></a>Стратегия кэширования

ListView часто используется для отображения гораздо большего объема данных, чем на экране. Например, музыкальное приложение может иметь библиотеку песен с тысячами записей. Создание элемента для каждой записи приведет к лишнему потреблению ценной памяти и будет работать плохо. Для постоянного создания и уничтожения строк приложение должно постоянно создавать и очищать объекты, что также будет работать плохо.

Для экономии памяти собственные [`ListView`](xref:Xamarin.Forms.ListView) эквиваленты для каждой платформы имеют встроенные функции для повторного использования строк. Только ячейки, которые отображаются на экране, загруженные в память и **содержимого** загружается в существующие ячейки. Этот шаблон запрещает приложению создавать экземпляры тысяч объектов, экономя время и память.

Xamarin. Forms [`ListView`](xref:Xamarin.Forms.ListView) позволяет повторно использовать ячейки [`ListViewCachingStrategy`](xref:Xamarin.Forms.ListViewCachingStrategy) через перечисление, которое имеет следующие значения:

```csharp
public enum ListViewCachingStrategy
{
    RetainElement,   // the default value
    RecycleElement,
    RecycleElementAndDataTemplate
}
```

> [!NOTE]
> Игнорирует универсальной платформы Windows (UWP) [ `RetainElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RetainElement) стратегию, кэширования, так как он всегда использует кэширование для повышения производительности. Таким образом, по умолчанию он ведет себя так, как если [ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) применяется стратегию кэширования.

### <a name="retainelement"></a>RetainElement

[ `RetainElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RetainElement) Стратегию кэширования указывает, что [ `ListView` ](xref:Xamarin.Forms.ListView) создаст ячейку для каждого элемента в списке, и значение по умолчанию `ListView` поведение. Его следует использовать в следующих случаях:

- Каждая ячейка имеет большое количество привязок (20 – 30 +).
- Шаблон ячейки часто меняется.
- Тестирование показывает, что `RecycleElement` стратегия кэширования приводит к снижению скорости выполнения.

Очень важно для распознавания последствия [ `RetainElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RetainElement) стратегию кэширования, при работе с пользовательские ячейки. Код для инициализации ячейки будет нужно запускать для создания каждой ячейки, которое может иметь несколько раз в секунду. В этом случае приемы макета, которые хорошо были на странице, например использование нескольких вложенных [`StackLayout`](xref:Xamarin.Forms.StackLayout) экземпляров, становятся узким местом производительности, когда они настраиваются и уничтожаются в режиме реального времени при прокрутке пользователем.

### <a name="recycleelement"></a>RecycleElement

[ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) Стратегию кэширования указывает, что [ `ListView` ](xref:Xamarin.Forms.ListView) будет старайтесь свести к минимуму его памяти объема и скорости выполнения путем перезапуска ячейки списка. Этот режим не всегда обеспечивает повышение производительности, поэтому для определения каких-либо улучшений необходимо выполнить тестирование. Однако это предпочтительный вариант, и его следует использовать в следующих случаях.

- Каждая ячейка имеет небольшое и умеренное количество привязок.
- Каждая ячейка [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) определяет все данные ячейки.
- Каждая ячейка почти похожа, при этом шаблон ячейки не изменяется.

Во время виртуализации ячейка будет иметь контекст привязки обновлены и поэтому если приложение использует этот режим его необходимо убедиться, правильно обрабатывать обновления контекста привязки. Все данные о ячейке должны поступать из контекста привязки, или, возможно возникновение ошибок согласованности. Эту проблему можно избежать с помощью привязки данных для вывода данных ячейки. Кроме того, данные ячейки следует задавать в `OnBindingContextChanged` переопределить, а не в конструктор пользовательской ячейки, как показано в следующем примере кода:

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

Дополнительные сведения см. в разделе [изменений контекста привязки](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#binding-context-changes).

В iOS и Android Если ячейки использовать пользовательские модули подготовки отчетов, они необходимо убедиться, что правильно реализуется уведомления об изменении свойства. При повторно ячеек используются значения свойств изменится при обновлении контекста привязки, доступные ячейки, с помощью `PropertyChanged` вызванных событий. Дополнительные сведения см. в разделе [Настройка ViewCell](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md).

#### <a name="recycleelement-with-a-datatemplateselector"></a>RecycleElement с DataTemplateSelector

Когда [ `ListView` ](xref:Xamarin.Forms.ListView) использует [ `DataTemplateSelector` ](xref:Xamarin.Forms.DataTemplateSelector) для выбора [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate), [ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) кэширования стратегия не кэширует `DataTemplate`s. Вместо этого `DataTemplate` выбран для каждого элемента данных в списке.

> [!NOTE]
> [ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) Стратегию кэширования имеет обязательной, представленные в Xamarin.Forms 2.4, что при [ `DataTemplateSelector` ](xref:Xamarin.Forms.DataTemplateSelector) предлагается выбрать [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate), в которых `DataTemplate` должен возвращать же [ `ViewCell` ](xref:Xamarin.Forms.ViewCell) типа. Например, если [ `ListView` ](xref:Xamarin.Forms.ListView) с `DataTemplateSelector` , могут возвращать либо `MyDataTemplateA` (где `MyDataTemplateA` возвращает `ViewCell` типа `MyViewCellA`), или `MyDataTemplateB` (где `MyDataTemplateB`возвращает `ViewCell` типа `MyViewCellB`), когда `MyDataTemplateA` возвращается, он должен возвращать `MyViewCellA` или будет вызвано исключение.

### <a name="recycleelementanddatatemplate"></a>RecycleElementAndDataTemplate

[ `RecycleElementAndDataTemplate` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElementAndDataTemplate) Стратегию кэширования лежит [ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) стратегию кэширования, гарантируя, кроме того, что при [ `ListView` ](xref:Xamarin.Forms.ListView) использует [ `DataTemplateSelector` ](xref:Xamarin.Forms.DataTemplateSelector) для выбора [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate), `DataTemplate`s кэшируемых тип элемента в списке. Таким образом `DataTemplate`s выбраны один раз на тип элемента, а не один раз для каждого экземпляра элемента.

> [!NOTE]
> [ `RecycleElementAndDataTemplate` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElementAndDataTemplate) Стратегию кэширования имеет необходимым условием, `DataTemplate`, возвращаемый [ `DataTemplateSelector` ](xref:Xamarin.Forms.DataTemplateSelector) необходимо использовать [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate.%23ctor(System.Type)) конструктор, принимающий `Type`.

### <a name="set-the-caching-strategy"></a>Настройка стратегии кэширования

[ `ListViewCachingStrategy` ](xref:Xamarin.Forms.ListViewCachingStrategy) Указано значение перечисления с [ `ListView` ](xref:Xamarin.Forms.ListView) перегрузку конструктора, как показано в следующем примере кода:

```csharp
var listView = new ListView(ListViewCachingStrategy.RecycleElement);
```

В XAML задайте `CachingStrategy` атрибут, как показано в XAML ниже:

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

Установка атрибута из XAML в [`ListView`](xref:Xamarin.Forms.ListView) подклассе не приведет к желаемому поведению `CachingStrategy` , так как отсутствует свойство в `ListView`. `CachingStrategy` Кроме того, если [XAMLC](~/xamarin-forms/xaml/xamlc.md) включен, будет создано следующее сообщение об ошибке: **Для "Качингстратеги" не найдено свойство, связываемое свойство или событие**

Решение этой проблемы является указание конструктор для подкласса [ `ListView` ](xref:Xamarin.Forms.ListView) , принимающий [ `ListViewCachingStrategy` ](xref:Xamarin.Forms.ListViewCachingStrategy) параметра и передает его в базовый класс:

```csharp
public class CustomListView : ListView
{
    public CustomListView (ListViewCachingStrategy strategy) : base (strategy)
    {
    }
    ...
}
```

Затем [ `ListViewCachingStrategy` ](xref:Xamarin.Forms.ListViewCachingStrategy) можно указать значение перечисления из XAML с помощью `x:Arguments` синтаксис:

```xaml
<local:CustomListView>
    <x:Arguments>
        <ListViewCachingStrategy>RecycleElement</ListViewCachingStrategy>
    </x:Arguments>
</local:CustomListView>
```

## <a name="listview-performance-suggestions"></a>Предложения по повышению производительности ListView

Существует множество методов повышения производительности `ListView`. Следующие рекомендации могут повысить производительность ListView

- Привязать `ItemsSource` свойства `IList<T>` , а не `IEnumerable<T>` коллекции, так как `IEnumerable<T>` коллекции, которые не поддерживают произвольного доступа.
- Используйте встроенные ячейки `TextCell` (например  /  `SwitchCell` , `ViewCell` ), а не всякий раз, когда это возможно.
- Используйте меньшее число элементов. Например, можно использовать одну `FormattedString` метку, а не несколько меток.
- Замените `ListView` с `TableView` при отображении неоднородные данные – то есть разные типы данных.
- Ограничить использование [ `Cell.ForceUpdateSize` ](xref:Xamarin.Forms.Cell.ForceUpdateSize) метод. Поскольку вызовет перегрузку, он снизится производительность.
- В Android, старайтесь не использовать параметр `ListView`в строку разделителя видимость или цвет после он был создан экземпляр, поскольку приводит к снижению производительности.
- Не следует изменять макет ячейки, на основе [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext). Изменение макета влечет за собой большие затраты на измерение и инициализацию.
- Избегайте иерархий глубоко вложенных макетов. Используйте `AbsoluteLayout` или `Grid` с целью сокращения вложения.
- Старайтесь не `LayoutOptions` использовать не `Fill` только`Fill` (самый дешевый для вычислений).
- Избегайте размещения `ListView` внутри `ScrollView` по следующим причинам:
  - `ListView` Реализует свой собственный прокрутки.
  - `ListView` Не получит все жесты, как будут обрабатываться в родительском `ScrollView`.
  - `ListView` Может создать настраиваемый верхний и нижний колонтитул, прокручивает элементы списка, потенциально, предоставляющей функциональность `ScrollView` был использован для. Дополнительные сведения см. в разделе [верхние и нижние колонтитулы](~/xamarin-forms/user-interface/listview/customizing-list-appearance.md#headers-and-footers).
- Рассмотрите пользовательский модуль подготовки отчетов, если вам нужна конкретная, сложная структура, представленная в ячейках.

`AbsoluteLayout`имеет возможность выполнять макеты без единичного вызова меры, делая ее очень производительной. Если `AbsoluteLayout` нельзя использовать, рассмотрите возможность [ `RelativeLayout` ](xref:Xamarin.Forms.RelativeLayout). При использовании `RelativeLayout`, непосредственно передачи ограничения будут значительно быстрее, чем использование выражения API. Этот метод выполняется быстрее, поскольку API выражений использует JIT, а в iOS дерево необходимо интерпретировать, что является медленным. Выражение API подходит для макетов страниц, в котором его требуется только на исходный макет и поворота, но в `ListView`, где он выполняется постоянно во время прокрутки, ее производительность может снижаться.

Создание пользовательского средства визуализации для [ `ListView` ](xref:Xamarin.Forms.ListView) или ячейках является одним из подходов к уменьшению влияния вычисления макета на производительность прокрутки. Дополнительные сведения см. в разделе [Настройка ListView](~/xamarin-forms/app-fundamentals/custom-renderer/listview.md) и [Настройка ViewCell](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md).

## <a name="related-links"></a>Связанные ссылки

- [Пользовательское представление модуля подготовки отчетов (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistviewnative)
- [ViewCell пользовательского модуля подготовки отчетов (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-viewcell)
- [ListViewCachingStrategy](xref:Xamarin.Forms.ListViewCachingStrategy)
