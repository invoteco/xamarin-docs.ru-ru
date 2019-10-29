---
title: Таблевиев Xamarin. Forms
description: В этой статье объясняется, как использовать класс Xamarin. Forms Таблевиев для представления прокручиваемых меню, настроек и форм ввода в приложениях.
ms.prod: xamarin
ms.assetid: D1619D19-A74F-40DF-8E53-B1B7DFF7A3FB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/25/2019
ms.openlocfilehash: 67625aa413880023cce6d3e5e21e4d3bd0ec8e4c
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/21/2019
ms.locfileid: "72695986"
---
# <a name="xamarinforms-tableview"></a>Таблевиев Xamarin. Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-tableview)

[`TableView`](xref:Xamarin.Forms.TableView) — это представление для отображения прокручиваемых списков данных или вариантов выбора, в которых есть строки, которые не используют один шаблон. В отличие от [ListView](~/xamarin-forms/user-interface/listview/index.md) `TableView` не имеет концепции `ItemsSource`, поэтому элементы должны добавляться вручную в качестве дочерних элементов.

![Пример Таблевиев](tableview-images/tableview-all-sml.png)

<a name="Use_Cases" />

## <a name="use-cases"></a>Варианты использования

[`TableView`](xref:Xamarin.Forms.TableView) удобно использовать в следующих случаях:

- представление списка параметров
- сбор данных в форме или
- Отображение данных, которые представлены по-разному из строки в строку (например, числа, проценты и изображения).

[`TableView`](xref:Xamarin.Forms.TableView) выполняет прокрутку и размещение строк в привлекательных разделах, что часто требуется для приведенных выше сценариев. Элемент управления `TableView` использует собственное эквивалентное представление каждой платформы, если оно доступно, создавая собственный внешний вид для каждой платформы.

<a name="TableView_Structure" />

## <a name="structure"></a>structure

Элементы в [`TableView`](xref:Xamarin.Forms.TableView) организованы по разделам. В корне `TableView` находится [`TableRoot`](xref:Xamarin.Forms.TableRoot), который является родительским для одного или нескольких экземпляров [`TableSection`](xref:Xamarin.Forms.TableSection) . Каждый [`TableSection`](xref:Xamarin.Forms.TableSection) состоит из заголовка и одного или нескольких [`ViewCell`](xref:Xamarin.Forms.ViewCell) экземпляров:

```xaml
<TableView Intent="Settings">
    <TableRoot>
        <TableSection Title="Ring">
            <SwitchCell Text="New Voice Mail" />
            <SwitchCell Text="New Mail" On="true" />
        </TableSection>
    </TableRoot>
</TableView>
```

Эквивалентный код на C# выглядит так:

```csharp
Content = new TableView
{
    Root = new TableRoot
    {
        new TableSection("Ring")
        {
          // TableSection constructor takes title as an optional parameter
          new SwitchCell { Text = "New Voice Mail" },
          new SwitchCell { Text = "New Mail", On = true }
        }
    },
    Intent = TableIntent.Settings
};
```

<a name="TableView_Appearance" />

## <a name="appearance"></a>Вид

[`TableView`](xref:Xamarin.Forms.TableView) предоставляет свойство [`Intent`](xref:Xamarin.Forms.TableView.Intent) , для которого можно задать любой из [`TableIntent`](xref:Xamarin.Forms.TableIntent) членов перечисления:

- `Data` — для использования при отображении записей данных. Обратите внимание, что [ListView](~/xamarin-forms/user-interface/listview/index.md) может быть лучшим вариантом для прокрутки списков данных.
- `Form` — используется, если Таблевиев выступает в качестве формы.
- `Menu` — для использования при показе меню выбора.
- `Settings` — для использования при отображении списка параметров конфигурации.

Выбранное значение [`TableIntent`](xref:Xamarin.Forms.TableIntent) может влиять на отображение [`TableView`](xref:Xamarin.Forms.TableView) на каждой платформе. Даже если различий нет, рекомендуется выбрать `TableIntent`, который наиболее полно соответствует предполагаемому использованию таблицы.

Кроме того, цвет текста, отображаемого для каждого [`TableSection`](xref:Xamarin.Forms.TableSection) , можно изменить, задав свойству `TextColor` значение [`Color`](xref:Xamarin.Forms.Color).

<a name="Built-In_Cells" />

## <a name="built-in-cells"></a>Встроенные ячейки

Xamarin. Forms поставляется со встроенными ячейками для сбора и отображения информации. Хотя [`ListView`](xref:Xamarin.Forms.ListView) и [`TableView`](xref:Xamarin.Forms.TableView) могут использовать все одни и те же ячейки, [`SwitchCell`](xref:Xamarin.Forms.SwitchCell) и [`EntryCell`](xref:Xamarin.Forms.EntryCell) наиболее актуальны для сценария `TableView`.

Подробное описание [текстцелл](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#textcell) и [имажецелл](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#imagecell)см. в разделе [представление ячеек ListView](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md) .

<a name="switchcell" />

### <a name="switchcell"></a>свитчцелл

[`SwitchCell`](xref:Xamarin.Forms.SwitchCell) — это элемент управления, используемый для показа и записи данных о состоянии или `true`/`false`. Он определяет следующие свойства:

- `Text` — текст, отображаемый рядом с параметром.
- `On` — указывает, отображается ли переключатель как on или OFF.
- `OnColor` — [`Color`](xref:Xamarin.Forms.Color) параметра, если он находится в положении ON.

Все эти свойства могут быть связаны.

[`SwitchCell`](xref:Xamarin.Forms.SwitchCell) также предоставляет событие `OnChanged`, что позволяет реагировать на изменения состояния ячейки.

![Пример Свитчцелл](tableview-images/switch-cell.png)

<a name="entrycell" />

### <a name="entrycell"></a>ентрицелл

[`EntryCell`](xref:Xamarin.Forms.EntryCell) удобно использовать в тех случаях, когда необходимо отображать текстовые данные, которые пользователь может изменять. Он определяет следующие свойства:

- `Keyboard` — клавиатура, отображаемая во время редактирования. Существуют такие параметры, как числовые значения, электронная почта, Номера телефонов и т. д. [Документация по API см. в разделе](xref:Xamarin.Forms.Keyboard).
- `Label` — текст метки, отображаемый слева от поля ввода текста.
- `LabelColor` — цвет текста метки.
- `Placeholder` — текст, отображаемый в поле ввода, если он имеет значение null или пуст. Этот текст исчезает при начале ввода текста.
- `Text` — текст в поле ввода.
- `HorizontalTextAlignment` — горизонтальное выравнивание текста. Значения выравниваются по центру, по левому или по правому краю. [Документация по API см. в разделе](xref:Xamarin.Forms.TextAlignment).
- `VerticalTextAlignment` — вертикальное выравнивание текста. Значения: `Start`, `Center`или `End`.

[`EntryCell`](xref:Xamarin.Forms.EntryCell) также предоставляет событие `Completed`, которое срабатывает, когда пользователь нажимает кнопку "Готово" на клавиатуре при редактировании текста.

![Пример Ентрицелл](tableview-images/entry-cell.png)

<a name="Custom_Cells" />

## <a name="custom-cells"></a>Пользовательские ячейки

Если встроенные ячейки недостаточно, можно использовать пользовательские ячейки для представления и записи данных способом, который имеет смысл для вашего приложения. Например, может потребоваться Показать ползунок, чтобы позволить пользователю выбрать непрозрачность изображения.

Все пользовательские ячейки должны быть производными от [`ViewCell`](xref:Xamarin.Forms.ViewCell)— того же базового класса, который используется всеми встроенными типами ячеек.

Пример пользовательской ячейки:

![Пример пользовательской ячейки](tableview-images/custom-cell.png)

В следующем примере показан XAML, используемый для создания [`TableView`](xref:Xamarin.Forms.TableView) на снимках экрана выше:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DemoTableView.TablePage"
             Title="TableView">
      <TableView Intent="Settings">
          <TableRoot>
              <TableSection Title="Getting Started">
                  <ViewCell>
                      <StackLayout Orientation="Horizontal">
                          <Image Source="bulb.png" />
                          <Label Text="left"
                                 TextColor="#f35e20" />
                          <Label Text="right"
                                 HorizontalOptions="EndAndExpand"
                                 TextColor="#503026" />
                      </StackLayout>
                  </ViewCell>
              </TableSection>
          </TableRoot>
      </TableView>
</ContentPage>
```

Эквивалентный код на C# выглядит так:

```csharp
var table = new TableView();
table.Intent = TableIntent.Settings;
var layout = new StackLayout() { Orientation = StackOrientation.Horizontal };
layout.Children.Add (new Image() { Source = "bulb.png"});
layout.Children.Add (new Label()
{
    Text = "left",
    TextColor = Color.FromHex("#f35e20"),
    VerticalOptions = LayoutOptions.Center
});
layout.Children.Add (new Label ()
{
    Text = "right",
    TextColor = Color.FromHex ("#503026"),
    VerticalOptions = LayoutOptions.Center,
    HorizontalOptions = LayoutOptions.EndAndExpand
});
table.Root = new TableRoot ()
{
    new TableSection("Getting Started")
    {
        new ViewCell() {View = layout}
    }
};
Content = table;
```

Корневым элементом в [`TableView`](xref:Xamarin.Forms.TableView) является [`TableRoot`](xref:Xamarin.Forms.TableRoot), и [`TableSection`](xref:Xamarin.Forms.TableSection) непосредственно под `TableRoot`. [`ViewCell`](xref:Xamarin.Forms.ViewCell) определяется непосредственно в `TableSection`, а [`StackLayout`](xref:Xamarin.Forms.StackLayout) используется для управления макетом пользовательской ячейки, хотя здесь можно использовать любой макет.

> [!NOTE]
> В отличие [`ListView`](xref:Xamarin.Forms.ListView) от [`TableView`](xref:Xamarin.Forms.TableView) не требует, чтобы пользовательские (или любые) ячейки были определены в `ItemTemplate`.

## <a name="row-height"></a>Высота строки

Класс [`TableView`](xref:Xamarin.Forms.TableView) имеет два свойства, которые можно использовать для изменения высоты строки ячеек:

- [`RowHeight`](xref:Xamarin.Forms.TableView.RowHeight) — задает высоту каждой строки в `int`.
- [`HasUnevenRows`](xref:Xamarin.Forms.TableView.HasUnevenRows) — строки имеют различные значения высоты, если задано значение `true`. Обратите внимание, что при установке этого свойства в значение `true`высота строк будет автоматически вычисляться и применяться в Xamarin. Forms.

При изменении высоты содержимого ячейки в [`TableView`](xref:Xamarin.Forms.TableView) ее высота неявным образом обновляется в Android и универсальная платформа Windows (UWP). Однако в iOS необходимо принудительно обновить, задав для свойства [`HasUnevenRows`](xref:Xamarin.Forms.TableView.HasUnevenRows) значение `true` и вызвав метод [`Cell.ForceUpdateSize`](xref:Xamarin.Forms.Cell.ForceUpdateSize) .

В следующем примере XAML показан [`TableView`](xref:Xamarin.Forms.TableView) , содержащий [`ViewCell`](xref:Xamarin.Forms.ViewCell):

```xaml
<ContentPage ...>
    <TableView ...
               HasUnevenRows="true">
        <TableRoot>
            ...
            <TableSection ...>
                ...
                <ViewCell x:Name="_viewCell"
                          Tapped="OnViewCellTapped">
                    <Grid Margin="15,0">
                        <Grid.RowDefinitions>
                            <RowDefinition Height="Auto" />
                            <RowDefinition Height="Auto" />
                        </Grid.RowDefinitions>
                        <Label Text="Tap this cell." />
                        <Label x:Name="_target"
                               Grid.Row="1"
                               Text="The cell has changed size."
                               IsVisible="false" />
                    </Grid>
                </ViewCell>
            </TableSection>
        </TableRoot>
    </TableView>
</ContentPage>
```

При нажатии [`ViewCell`](xref:Xamarin.Forms.ViewCell) выполняется обработчик `OnViewCellTapped` событий:

```csharp
void OnViewCellTapped(object sender, EventArgs e)
{
    _target.IsVisible = !_target.IsVisible;
    _viewCell.ForceUpdateSize();
}
```

Обработчик событий `OnViewCellTapped` показывает или скрывает второй [`Label`](xref:Xamarin.Forms.Label) в [`ViewCell`](xref:Xamarin.Forms.ViewCell)и явно обновляет размер ячейки путем вызова метода [`Cell.ForceUpdateSize`](xref:Xamarin.Forms.Cell.ForceUpdateSize) .

На следующих снимках экрана показана ячейка перед тем, как коснуться:

![ViewCell до изменения размера](tableview-images/cell-beforeresize.png)

На следующих снимках экрана показана ячейка после нажатия на:

![ViewCell после изменения размера](tableview-images/cell-afterresize.png)

> [!IMPORTANT]
> Если эта функция используется слишком сильно, снижение производительности может снизить производительность.

## <a name="related-links"></a>Связанные ссылки

- [Таблевиев (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-tableview)
