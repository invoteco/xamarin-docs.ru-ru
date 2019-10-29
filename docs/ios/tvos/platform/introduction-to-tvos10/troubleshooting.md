---
title: Устранение неполадок приложений tvOS 10, созданных с помощью Xamarin
description: Эта статья содержит несколько советов по устранению неполадок при работе с tvOS 10 в приложениях Xamarin. Здесь описываются проблемы, связанные с магазином приложений, двоичной совместимостью, CFNetwork Хттппротокол, CloudKit, основным образом, Нсусерактивити и UIKit.
ms.prod: xamarin
ms.assetid: EA5564BB-C415-49A2-B70C-3DBF5E0F3FAB
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/16/2017
ms.openlocfilehash: a6588dee675aee3e2580b70dfdea2920c6235775
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73030602"
---
# <a name="troubleshooting-tvos-10-apps-built-with-xamarin"></a>Устранение неполадок приложений tvOS 10, созданных с помощью Xamarin

В следующих разделах перечислены некоторые известные проблемы, которые могут возникнуть при использовании tvOS 10 с Xamarin, и решение этих проблем.

- [Магазин приложений](#App-Store)
- [Двоичная совместимость](#Binary-Compatibility)
- [Протокол HTTP CFNetwork](#CFNetwork-HTTP-Protocol)
- [CloudKit](#CloudKit)
- [Основное изображение](#CoreImage)
- [NSUserActivity](#NSUserActivity)
- [UIKit](#UIKit)

<a name="App-Store" />

## <a name="app-store"></a>App Store

Известные проблемы:

- При тестировании покупок в приложении в изолированной среде диалоговое окно проверки подлинности может появиться дважды.
- При тестировании покупок в приложении с размещенным содержимым в изолированной среде диалоговое окно пароля появляется при каждом переходе приложения на передний план до завершения загрузки содержимого.

<a name="Binary-Compatibility" />

## <a name="binary-compatibility"></a>Двоичная совместимость

Известные проблемы:

- Вызов `NSObject.ValueForKey` приведет к возникновению исключения `null`ого ключа.
- Ссылка на шрифт по имени при вызове `UIFont.WithName` приведет к сбою.
- `NSURLSession` и `NSURLConnection` больше не являются комплектами шифров RC4 во время подтверждения TLS для `http://` URL-адресов.
- Приложения могут зависнуть при изменении геометрического представления в методах `ViewWillLayoutSubviews` или `LayoutSubviews`.
- Для всех подключений SSL/TLS симметричный Шифр RC4 теперь отключен по умолчанию. Кроме того, интерфейс API защищенного транспорта больше не поддерживает SSLv3, поэтому рекомендуется как можно быстрее приостанавливаться к работе приложения с шифрованием SHA-1 и 3DES.

<a name="CFNetwork-HTTP-Protocol" />

## <a name="cfnetwork-http-protocol"></a>Протокол HTTP CFNetwork

Для свойства `HTTPBodyStream` класса `NSMutableURLRequest` необходимо задать неоткрытый поток, так как `NSURLConnection` и `NSURLSession` теперь строго обеспечивают это требование.

<a name="CloudKit" />

## <a name="cloudkit"></a>CloudKit

Длительные операции будут возвращать _«у вас нет разрешения на сохранение файла»._ План.

<a name="CoreImage" />

## <a name="core-image"></a>Основное изображение

API `CIImageProcessor` теперь поддерживает произвольное число образов ввода. `CIImageProcessor` API, который был добавлен в tvOS 10 Beta 1, будет удален.

<a name="NSUserActivity" />

## <a name="nsuseractivity"></a>NSUserActivity

После операции передачи свойство `UserInfo` объекта `NSUserActivity` может быть пустым. Явным образом вызовите объект `BecomeCurrent` Нсусерактивити "в качестве текущего обходного пути.

<a name="UIKit" />

## <a name="uikit"></a>UIKit

Известные проблемы:

- Изменение внешнего вида `UINavigationBar`, `UITabBar` или `UIToolBar` может привести к передаче макета для разрешения нового внешнего вида. Попытка изменить эти представления внутри события `LayoutSubviews`, `UpdateConstraints`, `WillLayoutSubviews` или `DidUpdateSubviews` может привести к бесконечному циклу макета.
- В tvOS 10 вызов метода `RemoveGestureRecognizer` объекта `UIView` явным образом отменяет любой распознаватель выполняемых жестов.
- Представленные контроллеры представления теперь могут повлиять на внешний вид строки состояния.
- tvOS 10 требует, чтобы разработчик вызывал `base.AwakeFromNib`, когда подкласс `UIViewController` и переопределяет метод `AwakeFromNib`.
- Приложения с настраиваемыми `UIView` подклассами, переопределяющие `LayoutSubviews` и Dirty макет перед вызовом `base.LayoutSubviews` могут вызвать бесконечный цикл макета в tvOS 10.
- Ресурсы, связанные с направлением или перевернутым образами, не перемещаются при назначении `UIButton` объектам.

## <a name="related-links"></a>Связанные ссылки

- [Примеры tvOS](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [Новые возможности в tvOS 10](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewinTVOS/Articles/tvOS10.html#//apple_ref/doc/uid/TP40017259-SW1)
