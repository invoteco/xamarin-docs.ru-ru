---
title: Основы приложений Xamarin. Mac
description: В этом документе содержатся ссылки на руководства, в которых описываются различные понятия, необходимые для понимания разработки приложений Xamarin. Mac.
ms.prod: xamarin
ms.assetid: 5A36B3A7-F197-4AC3-A40D-B2C49362FF06
ms.technology: xamarin-mac
author: conceptdev
ms.author: crdun
ms.date: 12/17/2015
ms.openlocfilehash: 73ec847b697c2d588d0c217bcbf12d4f0b6aa817
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/04/2019
ms.locfileid: "70291336"
---
# <a name="xamarinmac-application-fundamentals"></a>Основы приложений Xamarin. Mac

## <a name="common-patterns-and-idiomsmacapp-fundamentalspatternsmd"></a>[Общие шаблоны и идиомы](~/mac/app-fundamentals/patterns.md)

В интерфейсах API Apple, C#предоставляемых через, некоторые идиомы и закономерности снова возходят и снова. Если у вас есть опыт программирования с помощью Xamarin. iOS, они могут показаться знакомыми. Документация часто относится к этим шаблонам и идиомам многократно, поэтому знание этих шаблонов поможет понять, как вы найдете документацию.

## <a name="understanding-mac-apismacapp-fundamentalsmac-apismd"></a>[Основные сведения об API Mac](~/mac/app-fundamentals/mac-apis.md)

В C# значительной мере время разработки с помощью Xamarin. Mac вы можете думать, читать и писать, не заботясь о базовых API-интерфейсах целевой платформы. Однако иногда вам потребуется прочитать документацию по API от Apple, преобразовать ответ из Stack Overflow в решение проблемы или сравнить с существующим примером.

## <a name="console-appsmacapp-fundamentalsconsolemd"></a>[Консольные приложения](~/mac/app-fundamentals/console.md)

Вы также можете создавать "автономные" консольные приложения, обращающиеся к собственным интерфейсам API macOS с помощью Xamarin. Mac.

## <a name="working-with-xib-filesmacapp-fundamentalsxibmd"></a>[Работа с файлами. XIB](~/mac/app-fundamentals/xib.md)

В этой статье рассматривается работа с XIB-файлами, созданными в Interface Builder Xcode для создания и обслуживания пользовательских интерфейсов для приложения Xamarin. Mac.

## <a name="storyboardxib-less-user-interface-designmacapp-fundamentalsxibless-uimd"></a>[Структура пользовательского интерфейса. Storyboard/. XIB меньше](~/mac/app-fundamentals/xibless-ui.md)

В этой статье рассматривается создание пользовательского интерфейса приложения Xamarin. Mac непосредственно из C# кода без использования Interface Builder Xcode с файлами. Storyboard или. XIB.

## <a name="working-with-imagesmacapp-fundamentalsimagemd"></a>[Работа с изображениями](~/mac/app-fundamentals/image.md)

В этой статье рассматривается работа с изображениями и значками в приложении Xamarin. Mac. Здесь рассматривается создание и обслуживание образов, необходимых для создания значка приложения и использование изображений в Interface Builder C# кода и в Xcode.

## <a name="data-binding-and-key-value-codingmacapp-fundamentalsdatabindingmd"></a>[Привязка данных и кодирование значений ключа](~/mac/app-fundamentals/databinding.md)

В этой статье рассматривается использование кодирования и значения ключа для обеспечения привязки данных к элементам пользовательского интерфейса в Interface Builder Xcode. С помощью этого метода можно значительно сократить объем C# кода, который необходимо записать для приложения Xamarin. Mac. 

## <a name="working-with-databasesmacapp-fundamentalsdatabasesmd"></a>[Работа с базами данных](~/mac/app-fundamentals/databases.md)

В этой статье рассматривается использование кодирования и значения ключа-значения для обеспечения привязки данных с прямым доступом к базам данных SQLite к элементам пользовательского интерфейса в Interface Builder Xcode. В нем также рассматривается использование ORM SQLite.NET для предоставления доступа к данным SQLite.

## <a name="working-with-copy-and-pastemacapp-fundamentalscopy-pastemd"></a>[Работа с копированием и вставкой](~/mac/app-fundamentals/copy-paste.md)

В этой статье рассматривается работа с монтажным столом для копирования и вставки в приложение Xamarin. Mac. В нем показано, как работать со стандартными типами данных, которые могут совместно использоваться несколькими приложениями, а также как поддерживать пользовательские данные в приложении "предоставление".

## <a name="sandboxing-a-xamarinmac-appmacapp-fundamentalssandboxingmd"></a>[Изолирование приложения Xamarin. Mac](~/mac/app-fundamentals/sandboxing.md)

В этой статье рассматривается изолированное приложение Xamarin. Mac для выпуска в App Store. В нем рассматриваются все элементы, которые переходят в "песочницу": каталоги контейнеров, права, определяемые пользователем разрешения, разделение привилегий и принудительное применение ядра.

## <a name="playing-sound-with-avaudioplayermacapp-fundamentalssoundsmd"></a>[Воспроизведение звука с помощью помощью avaudioplayer](~/mac/app-fundamentals/sounds.md)

В этой статье показано, как использовать вспомогательный класс для управления воспроизведением звука с помощью помощью avaudioplayer.

## <a name="reporting-bugsmacapp-fundamentalstroubleshootingmd"></a>[Сообщения об ошибках](~/mac/app-fundamentals/troubleshooting.md)

Иногда мы работаем во время работы над проектом, как при невозможности получить API для работы с нужным образом, так и при попытке обойти ошибку. Нашей целью в Xamarin является успешная разработка мобильных и настольных приложений, и мы предоставили некоторые ресурсы для помощи.
