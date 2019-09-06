---
title: Xamarin. Mac — macOS Sierra устранение неполадок
description: Этот документ содержит несколько советов по устранению неполадок при работе с macOS Sierra в приложениях Xamarin. Mac. Советы относятся к Mac App Store, Apple Pay, двоичной совместимости, CFNetwork, CloudKit и др.
ms.prod: xamarin
ms.assetid: 323DD5EE-87CE-48E4-B234-1CF61B45A019
ms.technology: xamarin-mac
author: conceptdev
ms.author: crdun
ms.date: 09/22/2016
ms.openlocfilehash: 51276a7682599c6480c637fac385992feaf06e49
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/04/2019
ms.locfileid: "70278899"
---
# <a name="xamarinmac---macos-sierra-troubleshooting"></a>Xamarin. Mac — macOS Sierra устранение неполадок

_В этой статье приведены некоторые советы по устранению неполадок при работе с macOS Sierra в приложениях Xamarin. Mac._

В следующих разделах перечислены некоторые известные проблемы, которые могут возникнуть при использовании macOS Sierra с Xamarin. Mac и решения для этих проблем.

- [Магазин приложений](#App-Store)
- [Apple Pay](#Apple-Pay)
- [Двоичная совместимость](#Binary-Compatibility)
- [Протокол HTTP CFNetwork](#CFNetwork-HTTP-Protocol)
- [CloudKit](#CloudKit)
- [Основное изображение](#CoreImage)
- [Уведомления](#Notifications)
- [NSUserActivity](#NSUserActivity)
- [Обозревателе](#Safari)

<a name="App-Store" />

## <a name="app-store"></a>App Store

Известные проблемы:

- При тестировании покупок в приложении в изолированной среде диалоговое окно проверки подлинности может появиться дважды.
- При тестировании покупок в приложении с размещенным содержимым в изолированной среде диалоговое окно пароля появляется при каждом переходе приложения на передний план до завершения загрузки содержимого.

<a name="Apple-Pay" />

## <a name="apple-pay"></a>Apple Pay

Если при добавлении новой платежной карты в Apple Pay введена неправильная дата истечения срока действия или код безопасности (во вт.), процесс подготовки карт будет прерван.

<a name="Binary-Compatibility" />

## <a name="binary-compatibility"></a>Двоичная совместимость

Известные проблемы:

- Вызов `NSObject.ValueForKey` будет приводить к `null` исключению.
- `http://` И `NSURLSession` большенеявляютсякомплектамишифровRC4вовремяподтвержденияTLSдляURL-адресов.`NSURLConnection`
- Приложения могут зависнуть при изменении геометрического представления в `ViewWillLayoutSubviews` методах или. `LayoutSubviews`
- Для всех подключений SSL/TLS симметричный Шифр RC4 теперь отключен по умолчанию. Кроме того, интерфейс API защищенного транспорта больше не поддерживает SSLv3, поэтому рекомендуется как можно быстрее приостанавливаться к работе приложения с шифрованием SHA-1 и 3DES.

<a name="CFNetwork-HTTP-Protocol" />

## <a name="cfnetwork-http-protocol"></a>Протокол HTTP CFNetwork

Свойство класса должно быть установлено в неоткрытый поток, `NSURLSession` так как `NSURLConnection` теперь строго применяет это требование. `NSMutableURLRequest` `HTTPBodyStream`

<a name="CloudKit" />

## <a name="cloudkit"></a>CloudKit

Длительные операции будут возвращать _«у вас нет разрешения на сохранение файла»._ план.

<a name="CoreImage" />

## <a name="core-image"></a>Основное изображение

`CIImageProcessor` API теперь поддерживает произвольное число образов ввода. `CIImageProcessor`API, который был добавлен в macOS Sierra бета-версии 1, будет удален.

<a name="Notifications" />

## <a name="notifications"></a>Уведомления

При работе с расширениями содержимого уведомлений контроллеры представлений не освобождаются должным образом и могут привести к сбою при достижении предела памяти расширения.

<a name="NSUserActivity" />

## <a name="nsuseractivity"></a>NSUserActivity

После операции `UserInfo` передачи свойство `NSUserActivity` объекта может быть пустым. Явный вызов `BecomeCurrent` `NSUserActivity` объекта в качестве текущего обходного пути.

<a name="Safari" />

## <a name="safari"></a>Safari

Вебжеолокатион требует наличия безопасного (`https://`) URL-адреса для работы с iOS 10 и macOS Sierra, чтобы предотвратить вредоносное использование данных о расположении.

## <a name="related-links"></a>Связанные ссылки

- [Примеры Mac](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Mac)
- [Новые возможности в macOS 10,12](https://developer.apple.com/library/prerelease/content/releasenotes/MacOSX/WhatsNewInOSX/Articles/OSXv10.html#//apple_ref/doc/uid/TP40017145-SW1)
