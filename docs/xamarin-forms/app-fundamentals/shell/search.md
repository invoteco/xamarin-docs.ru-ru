---
title: Поиск в оболочке Xamarin.Forms
description: Приложения оболочки Xamarin.Forms могут использовать интегрированную функцию поиска, которая реализована в виде поля поиска в верхней части каждой страницы.
ms.prod: xamarin
ms.assetid: F8F9471D-6771-4D23-96C0-2B79473A06D4
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: e97dbb993cb108245636ae459a572e18b13d6817
ms.sourcegitcommit: 0596004d4a0e599c1da1ddd75a6ac928f21191c2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/22/2019
ms.locfileid: "66005200"
---
# <a name="xamarinforms-shell"></a>Оболочка Xamarin.Forms

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/Xaminals/)

Оболочка Xamarin.Forms включает встроенные функции поиска, предоставляемые классом `SearchHandler`. Возможность поиска можно добавить на страницу, указав для присоединенного свойства `Shell.SearchHandler` в качестве значения объект подкласса `SearchHandler`. После этого в верхней части страницы появится поле поиска:

[![Снимок экрана с SearchHandler в оболочке для iOS и Android](search-images/searchhandler.png "SearchHandler в оболочке")](search-images/searchhandler-large.png#lightbox "SearchHandler в оболочке")

Когда в такое поле поиска вводится запрос, область поисковых подсказок может заполняться данными:

[![Снимок экрана с результатами поиска SearchHandler в оболочке для iOS и Android](search-images/search-suggestions.png "Результаты поиска SearchHandler в оболочке")](search-images/search-suggestions-large.png#lightbox "Результаты поиска SearchHandler в оболочке")

Приложение может реагировать на выбор результатов поиска, например переходить на другую страницу.

## <a name="searchhandler-class"></a>Класс SearchHandler

Класс `SearchHandler` предоставляет следующие свойства, которые определяют его внешний вид и поведение.

- `ClearIcon` с типом [`ImageSource`](xref:Xamarin.Forms.ImageSource) обозначает отображаемый значок очистки поля поиска.
- `ClearIconHelpText` с типом `string` — доступный текст справки для значка очистки.
- `ClearIconName` с типом `string` — имя значка очистки, которое используется со средствами чтения с экрана.
- `ClearPlaceholderCommand` с типом `ICommand`, который выполняется при касании `ClearPlaceholderIcon`.
- `ClearPlaceholderCommandParameter` с типом `object`, который передается как параметр в `ClearPlaceholderCommand`.
- `ClearPlaceholderEnabled` с типом `bool`, который определяет возможность выполнять `ClearPlaceholderCommand`. Значение по умолчанию — `true`.
- `ClearPlaceholderHelpText` с типом `string` — доступный текст справки возле значка заполнителя очистки.
- `ClearPlaceholderIcon` с типом [`ImageSource`](xref:Xamarin.Forms.ImageSource) — значок заполнителя очистки, отображаемый при пустом поле поиска.
- `ClearPlaceholderName` с типом `string` — имя значка заполнителя очистки для использования со средствами чтения с экрана.
- `Command` с типом `ICommand`, который выполняется при подтверждении поискового запроса.
- `CommandParameter` с типом `object`, который передается как параметр в `Command`.
- `DisplayMemberName` с типом `string` обозначает имя или путь к свойству, которое отображается для каждого элемента данных в коллекции `ItemsSource`.
- `IsSearchEnabled` с типом `bool`, который представляет состояние активности поля поиска. Значение по умолчанию — `true`.
- `ItemsSource` с типом `IEnumerable` определяет коллекцию элементов, которые отображаются в области поисковых подсказок. По умолчанию имеет значение `null`.
- `ItemTemplate` с типом [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) определяет шаблон, применяемый к каждому элементу в коллекции элементов, которые отображаются в области поисковых подсказок.
- `Placeholder` с типом `string` — текст для отображения при пустом поле поиска.
- `Query` с типом `string` — введенный пользователем текст в поле поиска.
- `QueryIcon` с типом [`ImageSource`](xref:Xamarin.Forms.ImageSource) — значок, напоминающий пользователю о том, что доступна функция поиска.
- `QueryIconHelpText` с типом `string` — доступный текст справки для значка запроса.
- `QueryIconName` с типом `string` — имя значка запроса, которое используется со средствами чтения с экрана.
- `SearchBoxVisibility` с типом `SearchBoxVisibility` определяет видимость поля поиска. По умолчанию поле поиска является видимым и полностью развернутым.
- `SelectedItem` с типом `object` — выбранный элемент в результатах поиска. Это свойство доступно только для чтения и по умолчанию имеет значение `null`.
- `ShowsResults` с типом `bool` обозначает, нужно ли ожидать результаты поиска в области поисковых подсказок при вводе текста. Значение по умолчанию — `false`.

Все эти свойства поддерживаются объектами [`BindableProperty`](xref:Xamarin.Forms.BindableProperty), то есть их можно указывать в качестве целевых для привязки данных.

Кроме того, класс `SearchHandler` также предоставляет следующие переопределяемые методы:

- `OnClearPlaceholderClicked` вызывается каждый раз при касании `ClearPlaceholderIcon`;
- `OnItemSelected` вызывается каждый раз, когда пользователь выбирает результат поиска;
- `OnQueryChanged` вызывается при изменении свойства `Query`;
- `OnQueryConfirmed` вызывается каждый раз, когда пользователь нажимает клавишу ВВОД или подтверждает запрос в поле поиска.

Когда пользователь вводит запрос в поле поиска, обновляется свойство `Query`, и при каждом обновлении выполняется метод `OnQueryChanged`. Этот метод можно использовать для обновления области поисковых подсказок, которая отображается под полем поиска. Когда пользователь выбирает результат из области поисковых подсказок, выполняется метод `OnItemSelected`.

## <a name="create-a-searchhandler"></a>Создание SearchHandler

Чтобы добавить функцию поиска в приложение оболочки, создайте класс, производный от класса `SearchHandler`, и переопределите методы `OnQueryChanged` и `OnItemSelected`.

```csharp
public class MonkeySearchHandler : SearchHandler
{
    protected override void OnQueryChanged(string oldValue, string newValue)
    {
        base.OnQueryChanged(oldValue, newValue);

        if (string.IsNullOrWhiteSpace(newValue))
        {
            ItemsSource = null;
        }
        else
        {
            ItemsSource = MonkeyData.Monkeys
                .Where(monkey => monkey.Name.ToLower().Contains(newValue.ToLower()))
                .ToList<Animal>();
        }
    }

    protected override async void OnItemSelected(object item)
    {
        base.OnItemSelected(item);

        // Note: strings will be URL encoded for navigation (e.g. "Blue Monkey" becomes "Blue%20Monkey"). Therefore, decode at the receiver.
        await (App.Current.MainPage as Xamarin.Forms.Shell).GoToAsync($"monkeydetails?name={((Animal)item).Name}");
    }
}
```

Переопределение `OnQueryChanged` имеет два аргумента: `oldValue` содержит предыдущий поисковый запрос, а `newValue` — текущий поисковый запрос. Область поисковых подсказок можно обновлять, задавая свойству `SearchHandler.ItemsSource` значение коллекции `IEnumerable` с элементами, которые соответствуют текущему поисковому запросу.

Когда пользователь выбирает результат поиска, выполняется переопределение метода `OnItemSelected` и присваивается значение свойству `SelectedItem`. В нашем примере этот метод переходит на другую страницу, которая отображает данные о выбранных `Animal`. Дополнительные сведения о навигации, см. в статье [о навигации в оболочке Xamarin.Forms](navigation.md).

> [!NOTE]
> Дополнительные свойства `SearchHandler` позволяют управлять внешним видом поля поиска.

## <a name="consume-a-searchhandler"></a>Использование SearchHandler

Подкласс `SearchHandler` можно использовать, присваивая присоединенному свойству `Shell.SearchHandler` значение объекта соответствующего типа:

```xaml
<ContentPage ...
             xmlns:controls="clr-namespace:Xaminals.Controls">
    <Shell.SearchHandler>
        <controls:MonkeySearchHandler Placeholder="Enter search term"
                                      ShowsResults="true"
                                      DisplayMemberName="Name" />
    </Shell.SearchHandler>
    ...
</ContentPage>
```

Эквивалентный код на C# выглядит так:

```csharp
Shell.SetSearchHandler(this, new MonkeySearchHandler
{
    Placeholder = "Enter search term",
    ShowsResults = true,
    DisplayMemberName = "Name"
});
```

Метод `MonkeySearchHandler.OnQueryChanged` возвращает `List` объектов `Animal`. Свойству `DisplayMemberName` присваивается свойство `Name` каждого объекта `Animal`, а значит отображаемые в области поисковых подсказок данные будут содержать названия каждого животного.

Свойству `ShowsResults` присваивается значение `true`, чтобы поисковые подсказки отображались по мере ввода поискового запроса пользователем:

[![Снимок экрана с результатами поиска SearchHandler в оболочке для iOS и Android](search-images/search-results.png "Результаты поиска SearchHandler в оболочке")](search-images/search-results-large.png#lightbox "Результаты поиска SearchHandler в оболочке")

По мере изменения поискового запроса данные в области поисковых подсказок обновляются:

[![Снимок экрана с результатами поиска SearchHandler в оболочке для iOS и Android](search-images/search-results-change.png "Результаты поиска SearchHandler в оболочке")](search-images/search-results-change-large.png#lightbox "Результаты поиска SearchHandler в оболочке")

При выборе результата поиска происходит переход к `MonkeyDetailPage` и отображаются данные о выбранной обезьяне:

[![Снимок экрана с данными об обезьяне для iOS и Android](search-images/detailpage.png "Сведения об обезьяне")](search-images/detailpage-large.png#lightbox "Сведения об обезьяне")

## <a name="define-search-results-item-appearance"></a>Определения внешнего вида элементов в результатах поиска

Помимо отображения данных `string` в результатах поиска, вы можете изменить внешний вид каждого элемента результатов поиска, присвоив свойству `SearchHandler.ItemTemplate` значение [`DataTemplate`](xref:Xamarin.Forms.DataTemplate):

```xaml
<ContentPage ...
             xmlns:controls="clr-namespace:Xaminals.Controls">    
    <Shell.SearchHandler>
        <controls:MonkeySearchHandler Placeholder="Enter search term"
                                      ShowsResults="true">
            <controls:MonkeySearchHandler.ItemTemplate>
                <DataTemplate>
                    <Grid Padding="10">
                        <Grid.ColumnDefinitions>
                            <ColumnDefinition Width="0.15*" />
                            <ColumnDefinition Width="0.85*" />
                        </Grid.ColumnDefinitions>
                        <Image Source="{Binding ImageUrl}"
                               Aspect="AspectFill"
                               HeightRequest="40"
                               WidthRequest="40" />
                        <Label Grid.Column="1"
                               Text="{Binding Name}"
                               FontAttributes="Bold" />
                    </Grid>
                </DataTemplate>
            </controls:MonkeySearchHandler.ItemTemplate>
       </controls:MonkeySearchHandler>
    </Shell.SearchHandler>
    ...
</ContentPage>
```

Эквивалентный код на C# выглядит так:

```csharp
Shell.SetSearchHandler(this, new MonkeySearchHandler
{
    Placeholder = "Enter search term",
    ShowsResults = true,
    DisplayMemberName = "Name",
    ItemTemplate = new DataTemplate(() =>
    {
        Grid grid = new Grid { Padding = 10 };
        grid.ColumnDefinitions.Add(new ColumnDefinition { Width = new GridLength(0.15, GridUnitType.Star) });
        grid.ColumnDefinitions.Add(new ColumnDefinition { Width = new GridLength(0.85, GridUnitType.Star) });

        Image image = new Image { Aspect = Aspect.AspectFill, HeightRequest = 40, WidthRequest = 40 };
        image.SetBinding(Image.SourceProperty, "ImageUrl");
        Label nameLabel = new Label { FontAttributes = FontAttributes.Bold };
        nameLabel.SetBinding(Label.TextProperty, "Name");

        grid.Children.Add(image);
        grid.Children.Add(nameLabel, 1, 0);
        return grid;
    })
});
```

Указанные в [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) элементы определяют внешний вид каждого элемента в области поисковых подсказок. В нашем примере компоновка внутри `DataTemplate` определяется значением [`Grid`](xref:Xamarin.Forms.Grid). `Grid` содержит объект [`Image`](xref:Xamarin.Forms.Image) и объект [`Label`](xref:Xamarin.Forms.Label), оба из которых содержат привязки к свойствам каждого объекта `Monkey`.

На следующих снимках экрана представлены результаты применения шаблона для всех элементов в области поисковых подсказок:

[![Снимок экрана с шаблонными результатами поиска SearchHandler в оболочке для iOS и Android](search-images/search-results-template.png "Шаблонные результаты поиска SearchHandler в оболочке")](search-images/search-results-template-large.png#lightbox "Шаблонные результаты поиска SearchHandler в оболочке")

Дополнительные сведения о шаблонах данных см. в разделе [Шаблоны данных Xamarin.Forms](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md).

## <a name="searchbox-visibility"></a>Видимость SearchBox

Когда в верхней части страницы добавляется поле поиска, оно по умолчанию является видимым и полностью развернутым. Тем не менее, это поведение можно изменить, задав в свойстве `SearchHandler.SearchBoxVisibility` один из членов перечисления `SearchBoxVisibility`:

- `Hidden` — поле поиска не отображается и недоступно;
- `Collapsible` — поле поиска является скрытым, пока пользователь не выполнит действие, открывающее его;
- `Expanded` — поле поиска является видимым и полностью развернутым.

В следующем примере показано, как скрыть поле поиска:

```xaml
<ContentPage ...
             xmlns:controls="clr-namespace:Xaminals.Controls">
    <Shell.SearchHandler>
        <controls:MonkeySearchHandler SearchBoxVisibility="Hidden"
                                      ... />
    </Shell.SearchHandler>
    ...
</ContentPage>
```

## <a name="related-links"></a>Связанные ссылки

- [Xaminals (пример)](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/Xaminals/)
- [Навигация в оболочке Xamarin.Forms](navigation.md)
