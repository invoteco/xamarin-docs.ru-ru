---
title: Добавление данных в коллекцию элементов средства выбора
description: Представление выбора является элемент управления для выбора элемента из списка данных. В этой статье объясняется, как заполнить управляющий элемент выбора с данными, добавив его в коллекцию элементов и как реагировать на выбор элемента пользователем.
ms.prod: xamarin
ms.assetid: 3C840F64-A430-457D-A4B2-3D7AF46F9DBE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/26/2019
ms.openlocfilehash: 3bbea036efef44077ccbd28a16af06c97cd7026b
ms.sourcegitcommit: 00744f754527e5b55154365f89691caaf1c9d929
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/07/2019
ms.locfileid: "57557234"
---
# <a name="adding-data-to-a-pickers-items-collection"></a>Добавление данных в коллекцию элементов средства выбора

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PickerDemo/)

_Представление выбора является элемент управления для выбора элемента из списка данных. В этой статье объясняется, как заполнить управляющий элемент выбора с данными, добавив его в коллекцию элементов и как реагировать на выбор элемента пользователем._

## <a name="populating-a-picker-with-data"></a>Заполнение управляющий элемент выбора с данными

До Xamarin.Forms 2.3.4, процесс заполнения [ `Picker` ](xref:Xamarin.Forms.Picker) с данными было Добавление данных для отображения только для чтения [ `Items` ](xref:Xamarin.Forms.Picker.Items) коллекции, которая имеет тип `IList<string>`. Каждый элемент в коллекции должен быть типа `string`. Элементы могут быть добавлены в XAML путем инициализации `Items` свойства со списком `x:String` элементов:

```xaml
<Picker Title="Select a monkey"
        TitleColor="Red">
  <Picker.Items>
    <x:String>Baboon</x:String>
    <x:String>Capuchin Monkey</x:String>
    <x:String>Blue Monkey</x:String>
    <x:String>Squirrel Monkey</x:String>
    <x:String>Golden Lion Tamarin</x:String>
    <x:String>Howler Monkey</x:String>
    <x:String>Japanese Macaque</x:String>
  </Picker.Items>
</Picker>
```

Ниже приведен эквивалентный код на C#:

```csharp
var picker = new Picker { Title = "Select a monkey", TitleColor = Color.Red };
picker.Items.Add("Baboon");
picker.Items.Add("Capuchin Monkey");
picker.Items.Add("Blue Monkey");
picker.Items.Add("Squirrel Monkey");
picker.Items.Add("Golden Lion Tamarin");
picker.Items.Add("Howler Monkey");
picker.Items.Add("Japanese Macaque");
```

Помимо добавления данных с помощью `Items.Add` метода данных также могут быть вставлены в коллекцию с помощью `Items.Insert` метод.

## <a name="responding-to-item-selection"></a>Реагирование на выбор элементов

Объект [ `Picker` ](xref:Xamarin.Forms.Picker) поддерживает выбор одного элемента за раз. Когда пользователь выбирает элемент, [ `SelectedIndexChanged` ](xref:Xamarin.Forms.Picker.SelectedIndexChanged) вызывает событие и [ `SelectedIndex` ](xref:Xamarin.Forms.Picker.SelectedIndex) свойство обновляется в целое число, представляющее индекс выбранного элемента в списке. `SelectedIndex` Свойство — отсчитываемый от нуля номер, указывающий элемент, выбранный пользователем. Если элемент не выбран, что происходит при `Picker` сначала создается и инициализируется, `SelectedIndex` будет равно -1.

> [!NOTE]
> Элемент поведение выделения в [ `Picker` ](xref:Xamarin.Forms.Picker) можно настраивать на iOS с конкретной платформы. Дополнительные сведения см. в разделе [управление Выбор элемента](~/xamarin-forms/platform/ios/picker-selection.md).

В следующем коде показано в примере `OnPickerSelectedIndexChanged` метод обработчика событий, который выполняется при [ `SelectedIndexChanged` ](xref:Xamarin.Forms.Picker.SelectedIndexChanged) вызывает событие:

```csharp
void OnPickerSelectedIndexChanged(object sender, EventArgs e)
{
  var picker = (Picker)sender;
  int selectedIndex = picker.SelectedIndex;

  if (selectedIndex != -1)
  {
    monkeyNameLabel.Text = picker.Items[selectedIndex];
  }
}
```

Этот метод получает [ `SelectedIndex` ](xref:Xamarin.Forms.Picker.SelectedIndex) значение свойства и использует его для получения выбранного элемента из [ `Items` ](xref:Xamarin.Forms.Picker.Items) коллекции. Так как каждый элемент в `Items` коллекция `string`, они могут отображаться путем [ `Label` ](xref:Xamarin.Forms.Label) не требуя приведения к типу.

> [!NOTE]
> Объект [ `Picker` ](xref:Xamarin.Forms.Picker) может быть инициализировано для отображения указанного элемента, задав [ `SelectedIndex` ](xref:Xamarin.Forms.Picker.SelectedIndex) свойство. Тем не менее `SelectedIndex` свойство должно быть задано после инициализации [ `Items` ](xref:Xamarin.Forms.Picker.Items) коллекции.

## <a name="related-links"></a>Связанные ссылки

- [Средство выбора Demo (пример)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PickerDemo/)
- [Средство выбора](xref:Xamarin.Forms.Picker)
