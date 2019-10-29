---
title: Параметр Xamarin. Android
description: Использование мини-приложения коммутатора в приложении Xamarin. Android
ms.prod: xamarin
ms.assetid: 6E1F3324-EC41-454A-AEC0-0208813C7E50
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/29/2018
ms.openlocfilehash: 73becb5e4424854c9be6cdc3554f6cf93507b9a9
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029136"
---
# <a name="xamarinandroid-switch"></a>Параметр Xamarin. Android

Мини-приложение `Switch` (показанное ниже) позволяет пользователю переключаться между двумя состояниями, например включать или выключать. Значение `Switch` по умолчанию — OFF. Мини-приложение отображается в обоих состояниях:

[![снимки экрана мини-приложения коммутатора в состоянии OFF и ON](switch-images/16-switch-onoff.png)](switch-images/16-switch-onoff.png#lightbox)

## <a name="creating-a-switch"></a>Создание переключателя

Чтобы создать переключатель, просто объявите элемент `Switch` в XML следующим образом:

```xml
<Switch android:layout_width="wrap_content"
        android:layout_height="wrap_content" />
```

При этом создается базовый коммутатор, как показано ниже.

[Снимок экрана демонстрационного приложения, отображающий параметр в состоянии "Выкл."![](switch-images/07-switch.png)](switch-images/07-switch.png#lightbox)

## <a name="changing-default-values"></a>Изменение значений по умолчанию

Текст, отображаемый элементом управления для состояний включения и ВЫКЛЮЧЕНия, и значение по умолчанию можно настроить. Например, чтобы параметру по умолчанию было присвоено значение ON, а для параметра считать значение Нет/Да, можно задать атрибуты `checked`, `textOn`и `textOff` в следующем коде XML.

```xml
<Switch android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:checked="true"
        android:textOn="YES"
        android:textOff="NO" />
```

## <a name="providing-a-title"></a>Указание заголовка

Мини-приложение `Switch` также поддерживает включение метки текста путем установки атрибута `text` следующим образом:

```xml
<Switch android:text="Is Xamarin.Android great?"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:checked="true"
        android:textOn="YES"
        android:textOff="NO" />
```

Эта разметка создает следующий снимок экрана во время выполнения:

[![снимок экрана демонстрационного приложения с текстом по горизонтали перед мини-приложением-переключателем](switch-images/08-switch.png)](switch-images/08-switch.png#lightbox)

При изменении значения `Switch`возникает событие `CheckedChange`.
Например, в следующем коде мы заносим это событие и представиме мини-приложение `Toast` с сообщением на основе `isChecked` значения `Switch`, которое передается в обработчик событий как часть аргумента `CompoundButton.CheckedChangeEventArg`.

```csharp
Switch s = FindViewById<Switch> (Resource.Id.monitored_switch);
           
s.CheckedChange += delegate(object sender, CompoundButton.CheckedChangeEventArgs e) {
    var toast = Toast.MakeText (this, "Your answer is " +
        (e.IsChecked ?  "correct" : "incorrect"), ToastLength.Short);
    toast.Show ();
};
```

## <a name="related-links"></a>Связанные ссылки

- [Свитчдемо (пример)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/switchdemo)
- [Учебник по макету вкладок](~/android/user-interface/layouts/tab-layout/index.md)
