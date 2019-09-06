---
title: Доступные сборки
description: В этом документе перечислены сборки, доступные для использования в Xamarin. iOS, Xamarin. Android и Xamarin. Mac. Он также содержит ссылки на документацию по библиотекам .NET Standard и переносимым библиотекам классов.
ms.prod: xamarin
ms.assetid: AEF4ED0E-391F-4FA4-9F18-842BC24C272D
author: conceptdev
ms.author: crdun
ms.date: 03/13/2018
ms.openlocfilehash: 3d3524a0f99d78fc356dc7c4bb3e3aca1940a718
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/04/2019
ms.locfileid: "70278451"
---
# <a name="available-assemblies"></a>Доступные сборки

Все устройства Xamarin. iOS, Xamarin. Android и Xamarin. Mac поставляются с более чем десятком сборок. Так же, как Silverlight — это расширенное подмножество сборок для настольных систем .NET, платформы Xamarin также являются расширенным подмножеством нескольких сборок Silverlight и настольных систем .NET.

Платформы Xamarin не совместимы с интерфейсом ABI с существующими сборками, скомпилированными для другого профиля. Необходимо перекомпилировать исходный код, чтобы создать сборки, предназначенные для правильного профиля (так же, как необходимо повторно скомпилировать исходный код для Silverlight и .NET 3,5 отдельно).

Приложения Xamarin. Mac могут быть скомпилированы в трех режимах: в одном из которых используется проверенный мобильный профиль Xamarin, платформа Xamarin. Mac .NET 4,5, позволяющая ориентироваться на существующие сборки с полным рабочим столом, а также неподдерживаемый, использующий API-интерфейс .NET, находящийся в системе Mono. систему. Дополнительные сведения см. в документации по [целевым платформам](~/mac/platform/target-framework.md) .

## <a name="net-standard-libraries"></a>Библиотеки .NET Standard

Помимо привязок iOS, Android и Mac, проекты Xamarin могут использовать [библиотеки .NET Standard](~/cross-platform/app-fundamentals/net-standard.md).

## <a name="portable-class-libraries"></a>Переносимые библиотеки классов

Проекты Xamarin также могут использовать [переносимые библиотеки классов .NET](~/cross-platform/app-fundamentals/pcl.md), хотя эта технология является устаревшей в пользу .NET Standard.

## <a name="supported-assemblies"></a>Поддерживаемые сборки

Это сборки, доступные в **диспетчере ссылок > сборки > Framework** (Visual Studio 2017) и **ссылки Edit > packages** (Visual Studio для Mac) и их совместимость с платформами Xamarin.

> [!div class="mx-tdCol2BreakAll"]
> |Assembly|Совместимость API|Xamarin iOS|Xamarin Android|Xamarin Mac|
> |--------|-----------------|-----------|---------------|-----------|
> |FSharp.Core.dll| |✓|✓|✓|
> |l18N. dll|Включает ККЯ, Ближний Восток, другие, редкие, Западная|✓|✓|✓|
> |Microsoft.CSharp.dll| |✓|✓|✓|
> |Mono.CSharp.dll| |✓|✓|✓|
> |Mono.Data.Sqlite.dll|Поставщик ADO.NET для SQLite; см. раздел ограничения.|✓|✓|✓|
> |Mono.Data.Tds.dll|Поддержка протокола TDS; используется для поддержки [System. Data. SqlClient](xref:System.Data.SqlClient) в [System. Data](xref:System.Data).|✓|✓|✓|
> |Моно. Dynamic. &#8203;Интерпретатор. dll| |✓| | |
> |Mono.Security.dll|Криптографические API.|✓|✓|✓|
> |monotouch.dll|Эта сборка содержит C# привязку к API кокоатауч. Она доступна только в классических проектах iOS.|✓| | |
> |MonoTouch. &#8203;Dialog-1. dll| |✓| | |
> |MonoTouch.&#8203;NUnitLite.dll| |✓| | |
> |mscorlib.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |OpenTK-1.0.dll|API-интерфейсы OpenGL/Open Object, расширенные для обеспечения поддержки устройств iPhone.|✓|✓|✓|
> |System.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx), а также типы из следующих пространств имен:<br />System.Collections.Specialized<br />Системой. &#8203;ComponentModel<br />System. ComponentModel. Design<br />System.Diagnostics<br />System.IO<br />System.IO.Compression<br />System. IO. Compression. FileSystem<br />System.Net<br />System .NET. Cache<br />Система .NET. mail<br />System .NET. MIME<br />System.Net. &#8203;NetworkInformation<br />System.Net.Security<br />System.Net.Sockets<br />System. Runtime. &#8203;InteropServices<br />System.Runtime.Versioning<br />System. Security. &#8203;AccessControl<br />System.Security.Authentication<br />System. Security. &#8203;Шифрование<br />System. Security. Permissions<br />System.Threading<br />Системные. Timers|✓|✓|✓|
> |Системой. &#8203;ComponentModel. &#8203;Композиция. dll| |✓|✓|✓|
> |Системой. &#8203;ComponentModel. &#8203;Аннотации. dll| |✓|✓|✓|
> |System.Core.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |System.Data.dll|[.Net 3,5](https://msdn.microsoft.com/library/ms229335.aspx) с [удаленными функциями](~/ios/data-cloud/system.data.md).|✓|✓|✓|
> |System. Data. &#8203;Службы. &#8203;Client. dll|Полный клиент oData.|✓|✓|✓|
> |System.IO. &#8203;Сжатие данных| |✓|✓|✓|
> |System.IO. &#8203;Сжатие. &#8203;Файловая система| |✓|✓|✓|
> |System.Json.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |System.Net. &#8203;HTTP. dll| |✓|✓|✓|
> |Системой. &#8203;Numerics. dll| |✓|✓|✓|
> |System. Runtime. &#8203;Библиотека сериализации. dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |Системой. &#8203;ServiceModel. dll|Стек WCF, имеющийся в [Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |Системой. &#8203;ServiceModel. &#8203;Внутренняя библиотека. dll| |✓|✓|✓|
> |Системой. &#8203;ServiceModel. &#8203;Web. dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx), а также типы из следующих пространств имен: <br />Система<br />System.ServiceModel.Channels<br />System.ServiceModel.Description<br />System.ServiceModel.Web|✓|✓|✓|
> |Системой. &#8203;Transactions. dll|[.Net 3,5](https://msdn.microsoft.com/library/ms229335.aspx); часть поддержки [System. Data](~/ios/data-cloud/system.data.md) .|✓|✓|✓|
> |System. Web. &#8203;Services. dll|Основные веб-службы из профиля .NET 3,5 с удаленными компонентами сервера.|✓|✓|✓|
> |Системой. &#8203;Windows. dll| |✓|✓|✓|
> |System.&#8203;Xml.dll|[.NET 3.5](https://msdn.microsoft.com/library/ms229335.aspx)|✓|✓|✓|
> |System.Xml.&#8203;Linq.dll|[.NET 3.5](https://msdn.microsoft.com/library/ms229335.aspx)|✓|✓|✓|
> |System.Xml.Serialization.dll| |✓|✓|✓|
> |Xamarin.iOS.dll|Эта сборка содержит C# привязку к API кокоатауч. Это используется только в Объединенных проектах iOS.|✓| | |
> |Java.Interop.dll| | |✓| |
> |Mono.Android.dll| | |✓| |
> |Моно. Android. &#8203;Export. dll| | |✓| |
> |Mono.Posix.dll| | |✓| |
> |System.&#8203;EnterpriseServices.dll| | |✓| |
> |Xamarin. Android. &#8203;NUnitLite. dll| | |✓| |
> |Моно. CompilerServices. &#8203;Симболвритер. dll|Для модулей записи компилятора.| | |✓|
> |Xamarin.Mac.dll| | | |✓|
> |Системой. &#8203;Drawing. dll|System. Drawing не поддерживается в Unified API для платформ Xamarin. Mac, .NET 4,5 или Mobile. Поддержку System. Drawing можно добавить в iOS и macOS с помощью библиотеки [сисдравинг-кореграфикс](https://github.com/mono/sysdrawing-coregraphics) .|✓| |✓|
