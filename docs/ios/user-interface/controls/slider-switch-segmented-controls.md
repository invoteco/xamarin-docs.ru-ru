---
title: Ползунки, переключатели и сегментированные элементы управления в Xamarin. iOS
description: В этом документе обсуждаются слайды, переключатели и сегментированные элементы управления в Xamarin. iOS, описание способов работы с ними как программно, так и в конструкторе iOS.
ms.prod: xamarin
ms.assetid: 85BF0EC8-E581-49CD-B9E7-98BE4C5A0F6B
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: 763165f1e09f847745b820987f8dbbae8f834fd7
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73021952"
---
# <a name="sliders-switches-and-segmented-controls-in-xamarinios"></a>Ползунки, переключатели и сегментированные элементы управления в Xamarin. iOS

<a name="Sliders" />

## <a name="sliders"></a>Ползунки

Элемент управления "ползунок" позволяет просто выбрать числовое значение в диапазоне. Элемент управления по умолчанию имеет значение от 0 до 1, но эти ограничения можно настроить.

 [![](slider-switch-segmented-controls-images/image25a.png "Slider")](slider-switch-segmented-controls-images/image25a.png#lightbox)

На следующем снимке экрана показаны свойства, которые могут быть изменены в конструкторе:

 [![](slider-switch-segmented-controls-images/image26a.png "Slider Properties")](slider-switch-segmented-controls-images/image25a.png#lightbox)

Эти значения можно задать в коде, как показано ниже, включая подключение обработчика для отображения выбранного значения в `UILabel` элементе управления:

```csharp
slider1.MinValue = -1;
slider1.MaxValue = 2;
slider1.Value = 0.5f; // the current value
slider1.ValueChanged += (sender,e) => label1.Text = ((UISlider)sender).Value.ToString ();
```

Внешний вид ползунка можно также настроить, установив

```csharp
slider1.ThumbTintColor = UIColor.Blue;
slider1.MinimumTrackTintColor = UIColor.Gray;
slider1.MaximumTrackTintColor = UIColor.Green;
```

Настраиваемый ползунок выглядит следующим образом:

 [![](slider-switch-segmented-controls-images/image27a.png "Custom Slider")](slider-switch-segmented-controls-images/image28a.png#lightbox)

> [!IMPORTANT]
> В настоящее время возникает [Ошибка](https://stackoverflow.com/a/19496179) , вызывающая невозможность отображения `ThumbTint` во время выполнения, как ожидалось. В качестве обходного пути можно добавить следующую строку кода **перед** приведенным выше кодом. [[Источник](https://stackoverflow.com/a/21396794)]:
>
> `slider1.SetThumbImage(UIImage.FromBundle("thumb.png"),UIControlState.Normal);`
> 
> Вы можете использовать любое изображение, так как оно будет переопределено, но убедитесь, что оно помещено _в_ каталог ресурсов и вызывается в коде.

<a name="Switch" />

## <a name="switch"></a>Параметр

iOS использует `UISwitch` в качестве логического входа, который может быть представлен переключателем на других платформах. Пользователь может управлять этим элементом управления, перемещая *бегунок* между положениями **On/Off** .

 [![](slider-switch-segmented-controls-images/image28a.png "Switch")](slider-switch-segmented-controls-images/image28a.png#lightbox)

Внешний вид переключателя можно настроить в **панель свойств** конструктора, который позволит управлять состоянием оттенок по умолчанию, **вкл./выкл** . и **изображением включения/выключения**. Это показано на рисунке ниже.

 [![](slider-switch-segmented-controls-images/image29a.png "Switch Properties")](slider-switch-segmented-controls-images/image29a.png#lightbox)

Свойства переключателя также можно задать в коде, например в приведенном ниже коде будет показан параметр со значением по умолчанию `On`:

```csharp
switch1.On = true;
```

 <a name="Segmented_Controls" />

## <a name="segmented-controls"></a>Сегментированные элементы управления

Сегментированный элемент управления — это организованный способ, позволяющий пользователям взаимодействовать с небольшим количеством параметров. Он располагается горизонтально, и каждый сегмент функционирует как отдельная кнопка. При использовании конструктора сегментированный элемент управления можно найти в области **элементов > элементы управления**и должен выглядеть, как на следующем рисунке:

 [![](slider-switch-segmented-controls-images/segmentedcontrol.png "Segmented Control")](slider-switch-segmented-controls-images/segmentedcontrol.png#lightbox)

Уникальная функция конструктора позволяет выбирать каждый сегмент отдельно в области конструктора, как показано ниже:

 [![](slider-switch-segmented-controls-images/segmentedcontrolselection.png "Segmented Control")](slider-switch-segmented-controls-images/segmentedcontrolselection.png#lightbox)

Это позволяет использовать Панель свойств для более точного управления свойствами каждого сегмента. Редактируемые свойства можно увидеть на снимке экрана ниже:

 [![](slider-switch-segmented-controls-images/segmentedcontrolproperties.png "Segmented Control")](slider-switch-segmented-controls-images/segmentedcontrolproperties.png#lightbox)

Следует отметить, что стиль сегментированного элемента управления в iOS7 устарел, поэтому настройка параметров для этого в приложении iOS7 не будет действовать.

## <a name="related-links"></a>Связанные ссылки

- [Элементы управления (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/controls)
- [Контроллер предупреждений](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/alertcontroller)
