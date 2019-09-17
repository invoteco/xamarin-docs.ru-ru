---
title: "Android.Support.v7.AppCompat — не удалось найти ресурс, который соответствует указанному имени: attr 'android:actionModeShareDrawable'"
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5814069C-FC43-41DE-B5A5-024D05E59929
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/09/2018
ms.openlocfilehash: 3bce50169919df83ba1ab76c29d1ecc5c1da0bd6
ms.sourcegitcommit: 13e43f510da37ad55f1c2f5de1913fb0aede6362
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/16/2019
ms.locfileid: "71021121"
---
# <a name="androidsupportv7appcompat---no-resource-found-that-matches-the-given-name-attr-androidactionmodesharedrawable"></a>Android.Support.v7.AppCompat — не удалось найти ресурс, который соответствует указанному имени: attr 'android:actionModeShareDrawable'

1. Обязательно Скачайте последние дополнительные возможности, а также пакет SDK для Android 5,0 (API 21) с помощью диспетчера пакет SDK для Android.

2. Убедитесь, что вы компилируете приложение с параметров compilesdkversion, для которого задано значение 21. При необходимости можно также установить targetSdkVersion в значение 21.

3. Если вам нужна Предыдущая версия, например API 19, скачайте соответствующую версию, найденную на странице NuGet:

[https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)

> [!NOTE]
> Если вы вручную установите это через консоль диспетчера пакетов, убедитесь, что также установлена та же версия Xamarin. Android. support. v4

[https://www.nuget.org/packages/Xamarin.Android.Support.v4/](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)

Ссылка на Stack Overflow:[https://stackoverflow.com/questions/26431676/appcompat-v721-0-0-no-resource-found-that-matches-the-given-name-attr-andro](https://stackoverflow.com/questions/26431676/appcompat-v721-0-0-no-resource-found-that-matches-the-given-name-attr-andro)

## <a name="see-also"></a>См. также

- [Какие пакеты SDK Android следует установить?](~/android/troubleshooting/questions/install-android-sdk-packages.md)
