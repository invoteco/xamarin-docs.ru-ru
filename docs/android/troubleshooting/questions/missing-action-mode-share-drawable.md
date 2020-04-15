---
title: "Android.Support.v7.AppCompat — не удалось найти ресурс, который соответствует указанному имени: attr 'android:actionModeShareDrawable'"
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5814069C-FC43-41DE-B5A5-024D05E59929
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/09/2018
ms.openlocfilehash: e688bd27d1116b2a77a12ccd6da29ea582053581
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/13/2020
ms.locfileid: "75728113"
---
# <a name="androidsupportv7appcompat---no-resource-found-that-matches-the-given-name-attr-androidactionmodesharedrawable"></a>Android.Support.v7.AppCompat — не удалось найти ресурс, который соответствует указанному имени: attr 'android:actionModeShareDrawable'

1. Обязательно скачайте новейшие дополнительные возможности, а также пакет SDK для Android 5.0 (API 21) с помощью Диспетчера SDK Android.

2. Убедитесь, что при компиляции приложения для compileSdkVersion установлено значение 21. Кроме того, при необходимости можно установить значение 21 для targetSdkVersion.

3. Если вам нужна предыдущая версия, например API 19, скачайте ее со страницы NuGet:

[https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)

> [!NOTE]
> Если вы вручную устанавливаете нужную версию в консоли диспетчера пакетов, убедитесь, что установлена та же версия Xamarin.Android.Support.v4.

[https://www.nuget.org/packages/Xamarin.Android.Support.v4/](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)

Ссылка на Stack Overflow: [https://stackoverflow.com/questions/26431676/appcompat-v721-0-0-no-resource-found-that-matches-the-given-name-attr-andro](https://stackoverflow.com/questions/26431676/appcompat-v721-0-0-no-resource-found-that-matches-the-given-name-attr-andro)

## <a name="see-also"></a>См. также

- [Какие пакеты SDK Android следует установить?](~/android/troubleshooting/questions/install-android-sdk-packages.md)
