---
title: Xamarin.Mac - macOS Sierra Устранение неполадок
description: Этот документ содержит несколько советов по устранению неполадок для работы с macOS Sierra в приложений Xamarin.Mac. Советы относятся к Mac App Store, Apple Pay, совместимость двоичных файлов, CFNetwork, CloudKit и многое другое.
ms.prod: xamarin
ms.assetid: 323DD5EE-87CE-48E4-B234-1CF61B45A019
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 09/22/2016
ms.openlocfilehash: 322acff3279d0513266c7d9883726cac726334f7
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2019
ms.locfileid: "67830559"
---
# <a name="xamarinmac---macos-sierra-troubleshooting"></a>Xamarin.Mac - macOS Sierra Устранение неполадок

_В этой статье предоставляет некоторые советы по устранению неполадок для работы с macOS Sierra в приложений Xamarin.Mac._

В следующих разделах перечислены некоторые известные проблемы, которые могут возникнуть при использовании macOS Sierra с Xamarin.mac и решение этих проблем:

- [Магазин приложений](#App-Store)
- [Apple Pay](#Apple-Pay)
- [Совместимость двоичных файлов](#Binary-Compatibility)
- [CFNetwork протокола HTTP](#CFNetwork-HTTP-Protocol)
- [CloudKit](#CloudKit)
- [Образ основных компонентов](#CoreImage)
- [Уведомления](#Notifications)
- [NSUserActivity](#NSUserActivity)
- [Safari](#Safari)

<a name="App-Store" />

## <a name="app-store"></a>App Store

Известные проблемы:

- При тестировании покупки из приложений в изолированной среде, может появиться диалоговое окно проверки подлинности дважды.
- При тестировании покупки из приложений с помощью размещенное содержимое в изолированной среде, появится диалоговое окно пароля каждый раз, приложение будет выведен на передний план, пока не завершит загрузку содержимого.

<a name="Apple-Pay" />

## <a name="apple-pay"></a>Apple Pay

Если неверное истечения срока действия даты или безопасности код (CW) вводится при добавлении новой карточки оплаты для Apple Pay, карты, процесс подготовки будет завершен.

<a name="Binary-Compatibility" />

## <a name="binary-compatibility"></a>Совместимость двоичных файлов

Известные проблемы:

- Вызов `NSObject.ValueForKey` будет `null` ключа приведет к возникновению исключения.
- Оба `NSURLSession` и `NSURLConnection` RC4 комплекты шифров в процессе подтверждения TLS для больше не `http://` URL-адреса.
- Приложения может зависнуть, если они изменяют суперпредставления geometry в любом `ViewWillLayoutSubviews` или `LayoutSubviews` методы.
- Для всех подключений SSL/TLS симметричного шифрования RC4 теперь отключено по умолчанию. Кроме того Защита транспорта API больше не поддерживает SSLv3, и рекомендуется, что приложение остановить использование шифрования SHA-1 и 3DES как можно скорее.

<a name="CFNetwork-HTTP-Protocol" />

## <a name="cfnetwork-http-protocol"></a>CFNetwork протокола HTTP

`HTTPBodyStream` Свойство `NSMutableURLRequest` класса должно быть присвоено поток неоткрытый с момента `NSURLConnection` и `NSURLSession` теперь строго соблюсти это требование.

<a name="CloudKit" />

## <a name="cloudkit"></a>CloudKit

Долго выполняющиеся операции, будет возвращать _«У вас нет разрешения для сохранения файла.»_ Ошибка.

<a name="CoreImage" />

## <a name="core-image"></a>Образ основных компонентов

`CIImageProcessor` API теперь поддерживает счетчиком произвольные входные изображения. `CIImageProcessor` Удаляется API, который был включен в macOS Sierra бета-версии 1.

<a name="Notifications" />

## <a name="notifications"></a>Уведомления

При работе с расширениями содержимого уведомлений, контроллеры представлений не выходят в правильно и может привести к ошибкам при достижении ограничений памяти расширение.

<a name="NSUserActivity" />

## <a name="nsuseractivity"></a>NSUserActivity

После выполнения операции передачи `UserInfo` свойство `NSUserActivity` объект может быть пустым. Явным образом вызвать `BecomeCurrent` `NSUserActivity` объект в качестве текущего решения.

<a name="Safari" />

## <a name="safari"></a>Safari

WebGeolocation требуется защищенный канал (`https://`) URL-адрес для работы с iOS 10 и macOS Sierra, чтобы предотвратить злонамеренное использование данных расположения.







## <a name="related-links"></a>Связанные ссылки

- [Примеры Mac](https://developer.xamarin.com/samples/mac/)
- [Новые возможности в OS X 10.12](https://developer.apple.com/library/prerelease/content/releasenotes/MacOSX/WhatsNewInOSX/Articles/OSXv10.html#//apple_ref/doc/uid/TP40017145-SW1)
