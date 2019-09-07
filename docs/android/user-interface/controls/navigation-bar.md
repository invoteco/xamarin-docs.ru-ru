---
title: Панель навигации Xamarin. Android
ms.prod: xamarin
ms.assetid: 6023DB7E-9E72-4B90-A96A-11BC297B8A3D
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/01/2017
ms.openlocfilehash: cf57142f0896b42c5c8ba726db723527e0e61452
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2019
ms.locfileid: "70762389"
---
# <a name="xamarinandroid-navigation-bar"></a>Панель навигации Xamarin. Android

В Android 4 появилась новая функция пользовательского интерфейса системы, называемая *панелью навигации*, которая предоставляет элементы управления навигацией на устройствах, которые не включают аппаратные кнопки для **домашних**, **задних**и **меню**.
На следующем снимке экрана показана панель навигации из устройства для создания хранилища:

 [![Пример панели навигации Android](navigation-bar-images/19-navbar.png)](navigation-bar-images/19-navbar.png#lightbox)

Доступно несколько новых флагов, контролирующих видимость панели навигации и ее элементов управления, а также видимость системной панели, представленной в Android 3. Флаги определены в `Android.View.View` классе и перечислены ниже:

- `SystemUiFlagVisible`&ndash; Делает панель навигации видимой. 
- `SystemUiFlagLowProfile`&ndash; Затемнение элементов управления на панели навигации. 
- `SystemUiFlagHideNavigation`&ndash; Скрывает панель навигации. 

Эти флаги можно применить к любому представлению в иерархии представлений, задав `SystemUiVisibility` свойство. Если для нескольких представлений задано это свойство, система объединяет их с операцией или и применяет их до тех пор, пока в окне, в котором установлены флаги, остается фокус. При удалении представления все установленные флаги также будут удалены.

В следующем примере показано простое приложение, `SystemUiVisibility`при щелчке на любой из кнопок изменяется:

 [![Снимки экрана, демонстрирующие видимые, низкие профили и скрытые Системуивисибилити](navigation-bar-images/18-systemuivisibility.png)](navigation-bar-images/18-systemuivisibility.png#lightbox)

Код для изменения `SystemUiVisibility` задает свойство `TextView` для обработчика событий нажатия для каждой кнопки, как показано ниже:

```csharp
var tv = FindViewById<TextView> (Resource.Id.systemUiFlagTextView);
var lowProfileButton = FindViewById<Button>(Resource.Id.lowProfileButton);
var hideNavButton = FindViewById<Button> (Resource.Id.hideNavigation);
var visibleButton = FindViewById<Button> (Resource.Id.visibleButton);
           
lowProfileButton.Click += delegate {
    tv.SystemUiVisibility =
        (StatusBarVisibility)View.SystemUiFlagLowProfile;
};
           
hideNavButton.Click += delegate {
    tv.SystemUiVisibility =
       (StatusBarVisibility)View.SystemUiFlagHideNavigation;        
};
           
visibleButton.Click += delegate {
    tv.SystemUiVisibility = (StatusBarVisibility)View.SystemUiFlagVisible;
}
```

Кроме того, `SystemUiVisibility` изменение `SystemUiVisibilityChange` вызывает событие. Как и при задании `SystemUiVisibility` свойства, обработчик `SystemUiVisibilityChange` события может быть зарегистрирован для любого представления в иерархии. Например, приведенный ниже код использует `TextView` экземпляр для регистрации события:

```csharp
tv.SystemUiVisibilityChange +=
  delegate(object sender, View.SystemUiVisibilityChangeEventArgs e) {
        tv.Text = String.Format ("Visibility = {0}", e.Visibility);
  };
```

## <a name="related-links"></a>Связанные ссылки

- [Системуивисибилитидемо (пример)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/systemuivisibilitydemo)
- [Знакомство со Южным Сандвичевым](http://www.android.com/about/ice-cream-sandwich/)
- [Платформа Android 4,0](https://developer.android.com/sdk/android-4.0.html)
