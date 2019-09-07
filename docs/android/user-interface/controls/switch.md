---
title: Параметр Xamarin. Android
description: Использование мини-приложения коммутатора в приложении Xamarin. Android
ms.prod: xamarin
ms.assetid: 6E1F3324-EC41-454A-AEC0-0208813C7E50
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/29/2018
ms.openlocfilehash: 82271711864363bbaf593e8c44e31632048399dc
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2019
ms.locfileid: "70764877"
---
# <a name="xamarinandroid-switch"></a>Параметр Xamarin. Android

`Switch` Мини-приложение (показанное ниже) позволяет пользователю переключаться между двумя состояниями, например включать или выключать. Значение `Switch` по умолчанию — OFF. Мини-приложение отображается в обоих состояниях:

[![Снимки экрана мини-приложения переключателя в состоянии OFF и ON](switch-images/16-switch-onoff.png)](switch-images/16-switch-onoff.png#lightbox)

## <a name="creating-a-switch"></a>Создание переключателя

Чтобы создать переключатель, просто объявите `Switch` элемент в XML следующим образом:

```xml
<Switch android:layout_width="wrap_content"
        android:layout_height="wrap_content" />
```

При этом создается базовый коммутатор, как показано ниже.

[![Снимок экрана демонстрационного приложения, отображающего параметр в состоянии OFF](switch-images/07-switch.png)](switch-images/07-switch.png#lightbox)

## <a name="changing-default-values"></a>Изменение значений по умолчанию

Текст, отображаемый элементом управления для состояний включения и ВЫКЛЮЧЕНия, и значение по умолчанию можно настроить. Например, чтобы параметру по умолчанию было присвоено значение ON, а для свойства Read/Yes — значение OFF/ `checked`On `textOn`, можно `textOff` задать атрибуты, и в следующем коде XML.

```xml
<Switch android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:checked="true"
        android:textOn="YES"
        android:textOff="NO" />
```

## <a name="providing-a-title"></a>Указание заголовка

Мини-приложение также поддерживает включение текстовой метки, `text` настроив атрибут следующим образом: `Switch`

```xml
<Switch android:text="Is Xamarin.Android great?"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:checked="true"
        android:textOn="YES"
        android:textOff="NO" />
```

Эта разметка создает следующий снимок экрана во время выполнения:

[![Снимок экрана демонстрационного приложения с текстом по горизонтали перед мини-приложением-переключателем](switch-images/08-switch.png)](switch-images/08-switch.png#lightbox)

При изменении значения `CheckedChange` возникает событие. `Switch`
Например, в следующем коде мы Сообразим это событие и представим `Toast` мини-приложение с сообщением на `isChecked` основе значения `Switch`, которое передается в обработчик событий как часть `CompoundButton.CheckedChangeEventArg` аргумента.

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
