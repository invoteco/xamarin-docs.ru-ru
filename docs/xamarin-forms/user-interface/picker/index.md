---
title: Средство выбора Xamarin. Forms
description: В средстве выбора Xamarin. Forms отображается короткий список элементов, из которого пользователь может выбрать элемент. В этой статье объясняется, как использовать класс средства выбора для выбора текстового элемента из списка данных.
ms.prod: xamarin
ms.assetid: D4815A4B-104B-4294-951B-BD8F2EC33C86
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/26/2019
ms.openlocfilehash: c8246f8316a2a579bc890935dc4f9beecccb8dcc
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/21/2019
ms.locfileid: "72696003"
---
# <a name="xamarinforms-picker"></a>Средство выбора Xamarin. Forms

_Представление выбора — это элемент управления для выбора текстового элемента из списка данных._

В [`Picker`](xref:Xamarin.Forms.Picker) Xamarin. Forms отображается короткий список элементов, из которого пользователь может выбрать элемент. `Picker` определяет следующие свойства:

- [`Title`](xref:Xamarin.Forms.Picker.Title) типа `string`, значение по умолчанию `null`.
- `TitleColor` типа [`Color`](xref:Xamarin.Forms.Color)— цвет, используемый для вывода `Title` текста.
- [`ItemsSource`](xref:Xamarin.Forms.Picker.ItemsSource) типа `IList`, исходный список элементов для вывода, который по умолчанию имеет значение `null`.
- [`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex) типа `int`, индекс выбранного элемента, который по умолчанию имеет значение-1.
- [`SelectedItem`](xref:Xamarin.Forms.Picker.SelectedItem) типа `object` выбранный элемент, который по умолчанию имеет значение `null`.
- [`TextColor`](xref:Xamarin.Forms.Picker.TextColor) типа [`Color`](xref:Xamarin.Forms.Color), цвет, используемый для вывода текста, по умолчанию — [`Color.Default`](xref:Xamarin.Forms.Color.Default).
- [`FontAttributes`](xref:Xamarin.Forms.Picker.FontAttributes) типа [`FontAttributes`](xref:Xamarin.Forms.FontAttributes), значение по умолчанию [`FontAtributes.None`](xref:Xamarin.Forms.FontAttributes.None).
- [`FontFamily`](xref:Xamarin.Forms.Picker.FontFamily) типа `string`, значение по умолчанию `null`.
- [`FontSize`](xref:Xamarin.Forms.Picker.FontSize) типа `double`, значение по умолчанию — 1,0.
- `CharacterSpacing` типа `double` — интервал между символами элемента, отображаемого `Picker`.

Все свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами. Это означает, что их можно использовать в качестве стиля, а свойства могут быть целями привязок данных. Свойства [`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex) и [`SelectedItem`](xref:Xamarin.Forms.Picker.SelectedItem) имеют режим привязки по умолчанию [`BindingMode.TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay), что означает, что они могут быть целевыми объектами привязок данных в приложении, использующем архитектуру [Model-View-ViewModel (MVVM)](~/xamarin-forms/enterprise-application-patterns/mvvm.md) . Дополнительные сведения о настройке свойств шрифта см. в разделе [шрифты](~/xamarin-forms/user-interface/text/fonts.md).

[@No__t_1](xref:Xamarin.Forms.Picker) не отображает данные при первом отображении. Вместо этого значение его свойства [`Title`](xref:Xamarin.Forms.Picker.Title) отображается в виде заполнителя на платформах iOS и Android:

[![](images/picker-initial.png "Initial Picker Display")](images/picker-initial-large.png#lightbox "Initial Picker Display")

Когда [`Picker`](xref:Xamarin.Forms.Picker) получает фокус, отображаются его данные, и пользователь может выбрать элемент:

[![](images/picker-selection.png "Picker Selecting an Item")](images/picker-selection-large.png#lightbox "Picker Selecting an Item")

[@No__t_1](xref:Xamarin.Forms.Picker) запускает событие [`SelectedIndexChanged`](xref:Xamarin.Forms.Picker.SelectedIndexChanged) , когда пользователь выбирает элемент. После выбора выбранный элемент отображается `Picker`:

![](images/picker-after-selection.png "Picker after Selection")

Существует два метода заполнения [`Picker`](xref:Xamarin.Forms.Picker) данными:

- Установка для свойства [`ItemsSource`](xref:Xamarin.Forms.Picker.ItemsSource) отображаемых данных. Это рекомендуемая методика. Дополнительные сведения см. [в разделе Задание свойства ItemsSource элемента управления](populating-itemssource.md).
- Добавление данных, отображаемых в коллекцию [`Items`](xref:Xamarin.Forms.Picker.Items) . Этот прием был исходным процессом заполнения [`Picker`](xref:Xamarin.Forms.Picker) данными. Дополнительные сведения см. в разделе [Добавление данных в коллекцию элементов средства выбора](populating-items.md).

## <a name="related-links"></a>Связанные ссылки

- [Средство выбора](xref:Xamarin.Forms.Picker)
