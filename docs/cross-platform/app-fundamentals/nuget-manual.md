---
title: Создание пакетов NuGet для Xamarin вручную
description: В этом документе содержатся советы по созданию пакетов NuGet, предназначенных для платформы Xamarin. Здесь описываются профили Xamarin для пакетов NuGet, PCL NuGet с зависимостями платформы и ссылки на различные примеры с открытым исходным кодом.
ms.prod: xamarin
ms.assetid: a5964686-5fc6-4280-b087-7ba27cc1c8bf
author: conceptdev
ms.author: crdun
ms.date: 03/22/2017
ms.openlocfilehash: 88feea4c0a638da99a0316d802f04f08f0fea689
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/04/2019
ms.locfileid: "70289191"
---
# <a name="manually-creating-nuget-packages-for-xamarin"></a>Создание пакетов NuGet для Xamarin вручную

_На этой странице содержатся некоторые советы по созданию пакетов NuGet, предназначенных для платформы Xamarin._

> [!NOTE]
> Xamarin Studio 6,2 (и Visual Studio для Mac) включает возможность _автоматического_ создания пакетов NuGet с помощью PCL, .NET Standard или общих проектов. Дополнительные сведения см. в разделе [многоплатформенные библиотеки для совместного использования кода](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/index.md) .

## <a name="nuget-package-xamarin-profiles"></a>Профили Xamarin пакета NuGet

Веб-сайт NuGet, [поддерживающий несколько версий и профилей .NET Framework](https://docs.nuget.org/create/enforced-package-conventions) , описывает поддержку различных платформ и профилей Майкрософт, но не включает имена целевых платформ, используемых Xamarin.

Основные целевые платформы Xamarin, используемые сегодня:

- Для **Android** — Xamarin. Android
- **Xamarin. iOS** — [Unified API](~/cross-platform/macios/unified/index.md) Xamarin. iOS (поддерживает 64-разрядный)
- **Xamarin. Mac** — профиль мобильного устройства Xamarin. Mac, эквивалентный поверхности API Xamarin. iOS и Xamarin. Android.

Существует также целевой объект для более старых [Classic API](~/cross-platform/macios/unified/index.md)iOS:

- Classic API iOS с поддержкой **технологии Touch**

**Nuspec** -файл, предназначенный для всех, будет выглядеть следующим образом:

```xml
<files>
    <file src="Mac\bin\Release\*.dll" target="lib\Xamarin.Mac20" />
    <file src="iOS\bin\Release\*.dll" target="lib\Xamarin.iOS10" />
    <file src="Android\bin\Release\*.dll" target="lib\MonoAndroid10" />
    <file src="iOSClassic\bin\Release\*.dll" target="lib\MonoTouch10" />
</files>
```

Приведенный выше параметр игнорирует все переносимые библиотеки классов.

Большинство файлов **. nuspec** указывают номер версии целевой платформы, но это необязательно, если сборка работает со всеми версиями этой целевой платформы. Таким образом, если целевой объект был **либ\моноандроид** , это означает, что он работает с любой версией Xamarin. Android.

Можно указать версию с набором чисел без десятичной запятой или указать ее с помощью десятичных разделителей. Без десятичной запятой NuGet просто принимает каждое число и преобразует его в версию, вставляя символ "." между каждой цифрой.

В приведенном выше "MonoAndroid10" означает "Android 1,0". Это означает, что [Целевая платформа](~/android/app-fundamentals/android-api-levels.md) проекта должна иметь версию Android 1,0 или более позднюю. Версия указывается в `<TargetFrameworkVersion>` элементе в файле проекта.

Для уточнения:

- **MonoAndroid403** соответствует Android 4.0.3 и более поздней версии (уровень API IE 15)
- **Xamarin. iOS10** соответствует Xamarin. iOS 1,0 и более поздним версиям
- **Xamarin. iOS 1.0** также соответствует Xamarin. iOS 1,0 и более поздним версиям

## <a name="pcl-nugets-with-platform-dependencies"></a>PCL NuGet с зависимостями платформы

Профили PCL ограничены тем, какие API-интерфейсы .NET Framework имеют доступ, и, конечно, не могут получить доступ к коду, зависящему от платформы. Эти сторонние ссылки обсуждают различные подходы к созданию пакетов NuGet, использующих PCL и собственные API для обеспечения совместимости с Xamarin и другими платформами:

- [Как сделать переносимые библиотеки классов пригодными для вас](http://blogs.msdn.com/b/dsplaisted/archive/2012/08/27/how-to-make-portable-class-libraries-work-for-you.aspx)
- [Прием Bait и переключение PCL](http://log.paulbetts.org/the-bait-and-switch-pcl-trick/)
- [Создание пакета NuGet PCL, который работает с Xamarin. iOS](http://www.jimbobbennett.io/creating-a-nuget-pcl-that-works-with-xamarin-ios/)

Этот внешний [список профилей PCL с именем целевого объекта NuGet](http://embed.plnkr.co/03ck2dCtnJogBKHJ9EjY) также является полезной ссылкой.

## <a name="examples"></a>Примеры

Некоторые примеры с открытым кодом, на которые можно ссылаться:

- [**Модернхттпклиент**](https://www.nuget.org/packages/modernhttpclient/) — напишите свое приложение с помощью System .NET. http, но удалите эту библиотеку в, и она будет работать значительно быстрее (Просмотр [исходного кода](https://github.com/paulcbetts/ModernHttpClient)).
- [**Со звездочкой**](https://www.nuget.org/packages/Splat/) — Библиотека, позволяющая выполнять операции между различными платформами (Просмотр [исходного кода](https://github.com/paulcbetts/Splat)).
- [**Нграфикс**](https://www.nuget.org/packages/NGraphics/) — межплатформенная библиотека для отрисовки векторной графики на платформе .NET (Просмотр [исходного кода](https://github.com/praeclarum/NGraphics/blob/master/NGraphics.nuspec)).

## <a name="related-links"></a>Связанные ссылки

- [Нужетизер-3000 автоматическое создание NuGet](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/index.md)       
- [Включение NuGet в проект](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)
