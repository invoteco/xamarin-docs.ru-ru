---
title: Локализация приложения и строковые ресурсы
ms.prod: xamarin
ms.assetid: 374A9DA6-1853-8B98-6954-7FE3F591C07C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/30/2017
ms.openlocfilehash: 45fe5c783e737fb913730082841e0dfafc555684
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2019
ms.locfileid: "70755136"
---
# <a name="application-localization-and-string-resources"></a>Локализация приложения и строковые ресурсы

Локализация приложений — это процесс предоставления альтернативных ресурсов для определенного региона или локали. Например, можно предоставить строки локализованных языков для различных стран или изменить цвета или макет для соответствия определенным культурам. Android будет загружать и использовать ресурсы, подходящие для локали устройства во время выполнения, без внесения изменений в исходный код.

Например, на рисунке ниже показано приложение, работающее на трех разных национальных настройках, но текст, отображаемый в каждой кнопке, зависит от локали, для каждого устройства установлено значение:

[![Примеры трех разных языковых стандартов](application-localization-images/01-click-me-sml.png)](application-localization-images/01-click-me.png#lightbox)

В этом примере содержимое файла макета **Main. axml** выглядит примерно так:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
   android:orientation="vertical"
   android:layout_width="fill_parent"
   android:layout_height="fill_parent"
   >
<Button  
   android:id="@+id/myButton"
   android:layout_width="fill_parent"
   android:layout_height="wrap_content"
android:text="@string/hello"
   />
</LinearLayout>
```

В приведенном выше примере строка для кнопки была загружена из ресурсов путем предоставления идентификатора ресурса для строки:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Строки ресурсов для трех языков](application-localization-images/02-resource-strings-vs.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio для Mac](#tab/macos)

![Строки ресурсов для трех языков](application-localization-images/02-resource-strings-xs.png)

-----

## <a name="localizing-android-apps"></a>Локализация приложений Android

Ознакомьтесь с [введением в локализацию](~/cross-platform/app-fundamentals/localization.md) для получения советов и рекомендаций по локализации мобильных приложений.

Руководство по [локализации приложений Android](~/android/app-fundamentals/localization.md) содержит более конкретные примеры преобразования строк и локализации образов с помощью Xamarin. Android.

## <a name="related-links"></a>Связанные ссылки

- [Локализация приложений Android](~/android/app-fundamentals/localization.md)
- [Общие сведения о локализации на разных платформах](~/cross-platform/app-fundamentals/localization.md)
