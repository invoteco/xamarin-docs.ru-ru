---
title: Вручную созданию пакетов NuGet для Xamarin
description: Этот документ содержит советы помогут вам создавать пакеты NuGet, нацеленных на платформу Xamarin. Он описывает профили Xamarin пакета NuGet, пакеты NuGet переносимой библиотеки Классов с зависимостями платформа и ссылки на различные примеры с открытым исходным кодом.
ms.prod: xamarin
ms.assetid: a5964686-5fc6-4280-b087-7ba27cc1c8bf
author: asb3993
ms.author: amburns
ms.date: 03/22/2017
ms.openlocfilehash: 4f4ca327479a7f4eb4a7dc7feafdd71291c1b7fe
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/08/2019
ms.locfileid: "57671693"
---
# <a name="manually-creating-nuget-packages-for-xamarin"></a>Вручную созданию пакетов NuGet для Xamarin

_Эта страница содержит некоторые советы помогут вам создавать пакеты NuGet, нацеленных на платформу Xamarin._

> [!NOTE]
> Xamarin Studio 6.2 (и Visual Studio для Mac) включает в себя возможность _автоматически_ создание пакетов NuGet из библиотеки PCL .NET Standard и проектах общих. Ссылаться на [многоплатформенные библиотеки для совместного использования кода](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/index.md) руководство для получения дополнительных сведений.

## <a name="nuget-package-xamarin-profiles"></a>Профили Xamarin пакета NuGet

Веб-сайте NuGet [поддержка нескольких версий платформы .NET и профили](https://docs.nuget.org/create/enforced-package-conventions) описывает, как для поддержки различных платформ Microsoft и профили, но не содержит имен целевой платформы, используемые Xamarin.

Сегодня являются основной целевые платформы Xamarin, используется:

* **MonoAndroid** -Xamarin.Android
* **Xamarin.iOS** -Xamarin.iOS [единый API](~/cross-platform/macios/unified/index.md) (поддерживает 64-разрядных)
* **Xamarin.Mac** -мобильных профиля в Xamarin.Mac, что эквивалентно области Xamarin.iOS и Xamarin.Android API.

Имеется также целевой объект для более старых iOS [классический API](~/cross-platform/macios/unified/index.md):

* **MonoTouch** -iOS классический API

Объект **файлу nuspec** файл, который все это в качестве целевого будет выглядеть следующим образом:

```xml
<files>
    <file src="Mac\bin\Release\*.dll" target="lib\Xamarin.Mac20" />
    <file src="iOS\bin\Release\*.dll" target="lib\Xamarin.iOS10" />
    <file src="Android\bin\Release\*.dll" target="lib\MonoAndroid10" />
    <file src="iOSClassic\bin\Release\*.dll" target="lib\MonoTouch10" />
</files>
```

Выше игнорирует все переносимые библиотеки классов.

Большинство **файлу nuspec** файлах указывается номер версии целевой платформы, но это необязательно, если сборка работает со всеми версиями этой целевой платформы. Таким образом, если на целевом сервере была **lib\MonoAndroid** это будет означать, она работает с любой версией Xamarin.Android.

Можно указать версию с набором номера без десятичной запятой, или это можно сделать с помощью десятичных запятых. Без десятичной запятой NuGet просто принимает каждое число и включить его в версию, вставив "." между каждая цифра.

В приведенном выше «MonoAndroid10» означает «Android 1.0". Это означает лишь то проекта [требуемой версии .NET framework](~/android/app-fundamentals/android-api-levels.md) должен быть MonoAndroid 1.0 или более поздней версии. Версия, указанная в `<TargetFrameworkVersion>` элемент в файле проекта.

Для уточнения:

- **MonoAndroid403** соответствует Android 4.0.3 и более поздних версий (т. е API уровня 15)
- **Xamarin.iOS10** соответствует Xamarin.iOS 1.0 и новее
- **Xamarin.iOS1.0** также соответствует Xamarin.iOS 1.0 и новее

## <a name="pcl-nugets-with-platform-dependencies"></a>Пакеты NuGet переносимой библиотеки Классов с зависимости платформы

Профили PCL ограничены в какую платформу .NET API-интерфейсов, они могут получить доступ, и они определенно не может получить доступ к кода для конкретных платформ. Эти ссылки 3rd-party обсуждаются различные подходы к созданию пакетов NuGet, использующие PCL и собственные API-интерфейсы для обеспечения совместимости для Xamarin и других платформ:

- [Как исправить работу библиотеки переносимый класс для вас](http://blogs.msdn.com/b/dsplaisted/archive/2012/08/27/how-to-make-portable-class-libraries-work-for-you.aspx)
- [Прием с подменой переносимой библиотеки Классов](http://log.paulbetts.org/the-bait-and-switch-pcl-trick/)
- [Создание NuGet переносимую библиотеку Классов, которая работает с помощью Xamarin.iOS](http://www.jimbobbennett.io/creating-a-nuget-pcl-that-works-with-xamarin-ios/)

Этот внешний [список профилей PCL с их именами целевой NuGet](http://embed.plnkr.co/03ck2dCtnJogBKHJ9EjY) также является полезным справочником.

## <a name="examples"></a>Примеры

Некоторые примеры открытым исходным кодом, который можно использовать для:

- [**ModernHttpClient** ](https://www.nuget.org/packages/modernhttpclient/) — создавать приложения с помощью System.Net.Http, но удалить эту библиотеку в и осуществляется значительно быстрее (представление [источника](https://github.com/paulcbetts/ModernHttpClient)).
- [**Сплаттинга** ](https://www.nuget.org/packages/Splat/) — библиотеку, чтобы упростить платформ, должно быть (представление [источника](https://github.com/paulcbetts/Splat)).
- [**NGraphics** ](https://www.nuget.org/packages/NGraphics/) -межплатформенной библиотеки для отрисовки векторной графики на платформе .NET (представление [источника](https://github.com/praeclarum/NGraphics/blob/master/NGraphics.nuspec)).

## <a name="related-links"></a>Связанные ссылки

- [Nugetizer 3000 автоматическое создание Nuget](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/index.md)
- [Обновление пакеты NuGet для операций ввода-вывода 64-разрядная](https://blog.xamarin.com/how-to-update-nuget-packages-for-64-bit/)
- [Включение NuGet в проект](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)
