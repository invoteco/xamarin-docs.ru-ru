---
title: Макет для планшетных и настольных приложений
description: В этой статье объясняется, как оптимизировать макеты приложений Xamarin. Forms для планшетов, а не телефонов.
ms.prod: xamarin
ms.assetid: D62F472B-4345-4983-8403-659A538B591F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/01/2016
ms.openlocfilehash: f91d0127d0f2ffe37e3e0ff016dee551a679ad84
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/21/2019
ms.locfileid: "72273113"
---
# <a name="layout-for-tablet-and-desktop-apps"></a>Макет для планшетных и настольных приложений

Xamarin. Forms поддерживает все типы устройств, доступные на поддерживаемых платформах, поэтому в дополнение к телефонам приложения также могут работать в:

- iPad
- Планшеты с Android,
- Планшетные и настольные компьютеры Windows (под управлением Windows 10).

На этой странице кратко рассматриваются следующие вопросы:

- Поддерживаемые [типы устройств](#Device_Types)и
- [Оптимизация](#optimize) макетов для планшетов и телефонов.

<a name="Device_Types" />

## <a name="device-types"></a>Типы устройств

Для всех платформ, поддерживаемых Xamarin. Forms, доступны более крупные экранные устройства.

### <a name="ipads-ios"></a>iPad (iOS)

Шаблон Xamarin. Forms автоматически включает поддержку iPad, настроив для параметра **Devices. plist > устройства** значение **Universal** (это означает, что поддерживаются iPhone и iPad).

Чтобы обеспечить приятный запуска и убедиться, что на всех устройствах используется полноэкранное разрешение, следует убедиться в том, что предоставлен [экран запуска iPad](~/ios/app-fundamentals/images-icons/launch-screens.md) (с использованием раскадровки). Это обеспечит правильную визуализацию приложения на устройствах iPad Mini, iPad и iPad Pro.

До версии iOS 9 все приложения заняли весь экран на устройстве, но некоторые iPad теперь могут выполнять [многозадачные разбиение экрана](~/ios/platform/multitasking.md).
Это означает, что ваше приложение может занимать только тонкий столбец на стороне экрана, 50% от ширины экрана или всего экрана.

[![](tablet-images/ipad-sml.png "iPad Split Screen Example")](tablet-images/ipad.png#lightbox "iPad Split Screen Example")

Работа с разделением экрана означает, что вы должны проектировать приложение так, чтобы оно работало с шириной в 320 пикселей в ширину или в ширину 1366 пикселов.

### <a name="android-tablets"></a>Планшеты с Android

Экосистема Android имеет множество поддерживаемых размеров экрана — от небольших телефонов до крупных планшетов. Xamarin. Forms поддерживает все размеры экрана, но, как и другие платформы, может потребоваться настроить пользовательский интерфейс для более крупных устройств.

При поддержке множества различных разрешений экрана можно предоставить ресурсы машинного образа в различных размерах, чтобы оптимизировать взаимодействие с пользователем.
Дополнительные сведения о том, как структурировать папки и имена файлов в проекте приложения Android для включения оптимизированных ресурсов изображений в, см. в документации по [ресурсам Android](~/android/app-fundamentals/resources-in-android/index.md) (и в конкретном руководстве по [созданию ресурсов для различных размеров экрана](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md)). Ваше приложение.

### <a name="windows-tablets-and-desktops"></a>Планшеты и рабочие столы Windows

Для поддержки планшетных и настольных компьютеров под управлением Windows необходимо использовать [поддержку Windows UWP](~/xamarin-forms/platform/windows/installation/index.md), которая создает универсальные приложения, работающие в Windows 10.

В дополнение к полноэкранному режиму приложения, работающие на планшетах и настольных компьютерах под управлением Windows, можно изменять размеры до произвольных измерений.

[![](tablet-images/splitscreen-sml.png "Windows Split Screen Example")](tablet-images/splitscreen.png#lightbox "Windows Split Screen Example")

<a name="optimize" />

## <a name="optimizing-for-tablet-and-desktop"></a>Оптимизация для планшетов и настольных компьютеров

Вы можете настроить пользовательский интерфейс Xamarin. Forms в зависимости от того, используется ли телефонное или планшетное или настольное устройство. Это означает, что вы можете оптимизировать взаимодействие с пользователем для больших экранных устройств, например планшетов и настольных компьютеров.

### <a name="deviceidiom"></a>Device. идиом

Для изменения поведения приложения или пользовательского интерфейса можно использовать класс [`Device`](~/xamarin-forms/platform/device.md) . Используя перечисление `Device.Idiom`, можно

```csharp
if (Device.Idiom == TargetIdiom.Phone)
{
    HeroImage.Source = ImageSource.FromFile("hero.jpg");
} else {
    HeroImage.Source = ImageSource.FromFile("herotablet.jpg");
}
```

Этот подход можно расширить для внесения значительных изменений в отдельные макеты страниц или даже для отображения совершенно разных страниц на более крупных экранах.

### <a name="leveraging-masterdetailpage"></a>Использование Мастердетаилпаже

[@No__t_1](xref:Xamarin.Forms.MasterDetailPage) идеально подходит для более крупных экранов, особенно на iPad, где он использует [`UISplitViewController`](xref:UIKit.UISplitViewController) для предоставления собственного интерфейса iOS.

Просмотрите [эту запись блога Xamarin](https://devblogs.microsoft.com/xamarin/bringing-xamarin-forms-apps-to-tablets/) , чтобы увидеть, как можно адаптировать пользовательский интерфейс, чтобы на телефонах использовался один макет, а более крупные экраны могли использовать другое (с `MasterDetailPage`).

## <a name="related-links"></a>Связанные ссылки

- [Блог Xamarin](https://devblogs.microsoft.com/xamarin/bringing-xamarin-forms-apps-to-tablets/)
- [Пример Мишоппе](https://github.com/jamesmontemagno/myshoppe)
