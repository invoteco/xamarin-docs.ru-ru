---
title: Xamarin.Forms TableView
description: В этой статье объясняется, как использовать класс Xamarin.Forms TableView для представления прокручивающиеся меню, параметры и формы ввода в приложениях.
ms.prod: xamarin
ms.assetid: D1619D19-A74F-40DF-8E53-B1B7DFF7A3FB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/14/2018
ms.openlocfilehash: 195ed82325ed681f31f9963d5ff0ca0a7fef48ab
ms.sourcegitcommit: c9651cad80c2865bc628349d30e82721c01ddb4a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/03/2019
ms.locfileid: "70228139"
---
# <a name="xamarinforms-tableview"></a>Xamarin.Forms TableView

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-tableview)

[`TableView`](xref:Xamarin.Forms.TableView)— Это представление для отображения прокручиваемых списков данных или вариантов выбора, где имеются строки, которые не используют один и тот же шаблон. В отличие от [ListView](~/xamarin-forms/user-interface/listview/index.md) `TableView` не имеет концепции `ItemsSource`, поэтому элементы должны добавляться вручную в качестве дочерних элементов.

![Пример Таблевиев](tableview-images/tableview-all-sml.png)

<a name="Use_Cases" />

## <a name="use-cases"></a>Варианты использования

[`TableView`](xref:Xamarin.Forms.TableView)полезно в следующих случаях:

- представления списка параметров,
- Сбор данных в форме, или
- Отображение данных, который представляется по-разному из строки строки (например, номера, проценты и изображения).

[`TableView`](xref:Xamarin.Forms.TableView)обрабатывает прокрутку и размещение строк в привлекательных разделах, что часто требуется для приведенных выше сценариев. `TableView` Элемент управления использует каждой платформы базовый эквивалентного представления, если она доступна, создание собственного вида для каждой платформы.

<a name="TableView_Structure" />

## <a name="structure"></a>Структура

Элементы в разделе [`TableView`](xref:Xamarin.Forms.TableView) упорядочены по разделам. В корне объекта `TableView` [`TableRoot`](xref:Xamarin.Forms.TableRoot)—, который является родительским для одного или нескольких [`TableSection`](xref:Xamarin.Forms.TableSection) экземпляров. Каждый [`TableSection`](xref:Xamarin.Forms.TableSection) из них состоит из заголовка и одного или [`ViewCell`](xref:Xamarin.Forms.ViewCell) нескольких экземпляров:

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

[`TableView`](xref:Xamarin.Forms.TableView)предоставляет свойство, которое можно задать любому [`TableIntent`](xref:Xamarin.Forms.TableIntent) из членов перечисления: [`Intent`](xref:Xamarin.Forms.TableView.Intent)

- `Data`— для использования при отображении записей данных. Обратите внимание, что [ListView](~/xamarin-forms/user-interface/listview/index.md) может быть лучшим вариантом для прокрутки списков данных.
- `Form`— используется, если Таблевиев выступает в качестве формы.
- `Menu`— для использования при показе меню выбора.
- `Settings`— для использования при отображении списка параметров конфигурации.

Выбранное [`TableView`](xref:Xamarin.Forms.TableView) значение может повлиять на то, как отображается на каждой платформе. [`TableIntent`](xref:Xamarin.Forms.TableIntent) Даже если не очистить различия, рекомендуется выбрать `TableIntent` , наиболее точно соответствующий, как вы планируете использовать в таблице.

Кроме того, цвет текста, отображаемого для каждого из [`TableSection`](xref:Xamarin.Forms.TableSection) них, можно изменить, `TextColor` задав для [`Color`](xref:Xamarin.Forms.Color)свойства значение.

<a name="Built-In_Cells" />

## <a name="built-in-cells"></a>Встроенные ячейки

Xamarin.Forms в состав встроенных ячеек для сбора и отображения сведений. Хотя [`ListView`](xref:Xamarin.Forms.ListView) [`SwitchCell`](xref:Xamarin.Forms.SwitchCell) и [`TableView`](xref:Xamarin.Forms.TableView) могут использовать все одни и те же ячейки и [`EntryCell`](xref:Xamarin.Forms.EntryCell) наиболее актуальны для `TableView` сценария.

См. в разделе [внешний вид ячейки ListView](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md) подробное описание [TextCell](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#TextCell) и [ImageCell](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#ImageCell).

<a name="switchcell" />

### <a name="switchcell"></a>SwitchCell

[`SwitchCell`](xref:Xamarin.Forms.SwitchCell) Представляет элемент управления, используемый для представления и запись Вкл/Выкл или `true` / `false` состояние. Он определяет следующие свойства:

- `Text`— текст, отображаемый рядом с параметром.
- `On`— Указывает, отображается ли переключатель как on или OFF.
- `OnColor`[`Color`](xref:Xamarin.Forms.Color) — параметр параметра, если он находится в положении ON.

Все эти свойства могут быть связаны.

[`SwitchCell`](xref:Xamarin.Forms.SwitchCell)также предоставляет `OnChanged` событие, позволяющее реагировать на изменения в состоянии ячейки.

![Пример Свитчцелл](tableview-images/switch-cell.png)

<a name="entrycell" />

### <a name="entrycell"></a>EntryCell

[`EntryCell`](xref:Xamarin.Forms.EntryCell) полезно при необходимости отображать текстовые данные, пользователь может редактировать. Он определяет следующие свойства:

- `Keyboard`— Клавиатура, отображаемая во время редактирования. Существуют параметры для таких вещей, как числовые значения, электронной почты, номера телефонов, и т.д. [Документация по API см. в разделе](xref:Xamarin.Forms.Keyboard).
- `Label`— Текст метки, отображаемый слева от поля ввода текста.
- `LabelColor`— Цвет текста метки.
- `Placeholder`— Текст, отображаемый в поле ввода, если он имеет значение null или пуст. Этот текст исчезает, когда начинается запись текста.
- `Text`— Текст в поле ввода.
- `HorizontalTextAlignment`— Горизонтальное выравнивание текста. Может быть, влево или вправо центру. [Документация по API см. в разделе](xref:Xamarin.Forms.TextAlignment).

[`EntryCell`](xref:Xamarin.Forms.EntryCell)также предоставляет `Completed` событие, которое срабатывает, когда пользователь нажимает кнопку "Готово" на клавиатуре при редактировании текста.

![Пример Ентрицелл](tableview-images/entry-cell.png)

<a name="Custom_Cells" />

## <a name="custom-cells"></a>Пользовательские ячейки

При встроенной ячейки недостаточно, пользовательские ячейки можно использовать для представления и записать данные способом, который лучше всего подходит для вашего приложения. Например можно представить ползунок, чтобы разрешить пользователю выбирать прозрачность изображения.

Все пользовательские ячейки должен быть производным от [ `ViewCell` ](xref:Xamarin.Forms.ViewCell), тот же базовый класс, что все ячейки, встроенные типы использования.

Ниже приведен пример пользовательского ячейки:

![Пример пользовательской ячейки](tableview-images/custom-cell.png)

В следующем примере показан XAML, используемый для создания на [`TableView`](xref:Xamarin.Forms.TableView) снимках экрана выше:

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

Корневым элементом [`TableView`](xref:Xamarin.Forms.TableView) в [`TableRoot`](xref:Xamarin.Forms.TableRoot)является [`TableSection`](xref:Xamarin.Forms.TableSection) , а сразу под ним `TableRoot`находится. Определяется непосредственно в, а [`StackLayout`](xref:Xamarin.Forms.StackLayout) используется для управления макетом пользовательской ячейки, хотя здесь можно использовать любой макет. `TableSection` [`ViewCell`](xref:Xamarin.Forms.ViewCell)

> [!NOTE]
> В отличие [`ListView`](xref:Xamarin.Forms.ListView)от [`TableView`](xref:Xamarin.Forms.TableView) , не требует, чтобы пользовательские (или любые) `ItemTemplate`ячейки были определены в.

## <a name="row-height"></a>Высота строки

[ `TableView` ](xref:Xamarin.Forms.TableView) Класс имеет два свойства, которые могут использоваться для изменения высоты строки ячеек:

- [`RowHeight`](xref:Xamarin.Forms.TableView.RowHeight) — Задает высоту каждой строки `int`.
- [`HasUnevenRows`](xref:Xamarin.Forms.TableView.HasUnevenRows) — строки имеют различные значения высоты, если значение `true`. Обратите внимание, что при задании этого свойства `true`, высоты строк автоматически вычисляются и применены с Xamarin.Forms.

При высоту содержимого ячейки в [ `TableView` ](xref:Xamarin.Forms.TableView) изменяется строка высота неявно обновится на Android и универсальной платформы Windows (UWP). Тем не менее, в iOS он должен иметь возможность для обновления, задав [ `HasUnevenRows` ](xref:Xamarin.Forms.TableView.HasUnevenRows) свойства `true` и вызвав [ `Cell.ForceUpdateSize` ](xref:Xamarin.Forms.Cell.ForceUpdateSize) метод.

В следующем примере показан XAML [ `TableView` ](xref:Xamarin.Forms.TableView) , содержащий [ `ViewCell` ](xref:Xamarin.Forms.ViewCell):

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

Когда [ `ViewCell` ](xref:Xamarin.Forms.ViewCell) шифрованию, `OnViewCellTapped` выполняется обработчик событий:

```csharp
void OnViewCellTapped(object sender, EventArgs e)
{
    _target.IsVisible = !_target.IsVisible;
    _viewCell.ForceUpdateSize();
}
```

`OnViewCellTapped` Обработчик событий показывает или скрывает второй [ `Label` ](xref:Xamarin.Forms.Label) в [ `ViewCell` ](xref:Xamarin.Forms.ViewCell)и явным образом обновляет размер ячейки, вызвав [ `Cell.ForceUpdateSize` ](xref:Xamarin.Forms.Cell.ForceUpdateSize) метод.

На следующих снимках экрана показано ячейки до касание после:

![ViewCell до изменения размера](tableview-images/cell-beforeresize.png)

Далее на снимках экрана Показать ячейки после касание после:

![ViewCell после изменения размера](tableview-images/cell-afterresize.png)

> [!IMPORTANT]
> Если эта функция является чрезмерным, есть вероятность, strong снижения производительности.

## <a name="related-links"></a>Связанные ссылки

- [TableView (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-tableview)
