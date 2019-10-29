---
title: Изменения WebKit и Safari в iOS 11
description: В этом документе обсуждаются изменения, вносимые в WebKit и платформу служб Safari в iOS 11. В нем описывается работа с обновлениями стилей в Сфсафаривиевконтроллер и новые функции в Вквебвиев.
ms.prod: xamarin
ms.assetid: C74B2E94-177C-43D4-8D6C-9B528773C120
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/12/2017
ms.openlocfilehash: ef9577aad756ae67ac9fed685d7e40faea33c316
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032054"
---
# <a name="webkit-and-safari-changes-in-ios-11"></a>Изменения WebKit и Safari в iOS 11

в iOS 11 появилась новая версия веб-браузера Safari — Safari 11,0, который включает изменения в WebKit и Сафарисервицес. В этом руководством рассматриваются эти изменения.

## <a name="safariservices"></a>сафарисервицес

`SFSafariViewController` была введена в iOS 9 в качестве варианта отображения веб-содержимого или проверки подлинности пользователей в приложении. Дополнительные сведения о его возможностях см. в разделе "Обзор [веб-представлений](~/ios/user-interface/controls/uiwebview.md#safariviewcontroller) ".

в iOS 11 появились обновления стиля для контроллера представления Safari, что позволяет пользователям более эффективно работать между приложением и Интернетом. Например, удаление адресной строки теперь дает контроллеру представления Safari поведение обозревателя в приложении, а не мини-браузер. Можно также настроить цветовую схему так, чтобы она соответствовала цветовой схеме приложения, задав свойства `preferredBarTintColor` и `PreferredControlTintColor`.

```csharp
sfViewController.PreferredControlTintColor = UIColor.White;
sfViewController.PreferredBarTintColor = UIColor.Purple;
```

Следующий фрагмент кода отображает столбцы в виде фиолетового и белого столбцов, как показано на следующем рисунке:

![Сфсафаривиевконтроллер панели, отображаемые фиолетовым и белым цветом](web-images/image1.png)

Кнопку Закрыть, представленную в контроллере представления Safari, можно также изменить, задав для свойства `DismissButtonStyle` значение `Done`, `Close`или `Cancel`.

```csharp
sfViewController.DismissButtonStyle = SFSafariViewControllerDismissButtonStyle.Close;
```

![Текст кнопки "Закрыть" изменен](web-images/image2.png)

Это значение можно изменить, когда будет представлено `SFSafariViewController`.

В зависимости от содержимого, отображаемого в контроллере представления Safari, может потребоваться не сворачивать строки меню при прокрутке пользователем. Это можно настроить, задав для нового свойства `BarCollapsedEnabled` значение `false`.

```csharp
var config = new SFSafariViewControllerConfiguration();
config.BarCollapsingEnabled = false;

var sfViewController = new SFSafariViewController(url, config);
```

![Свертывание строк отключено](web-images/image3.png)

Компания Apple также внесла изменения в сведения о конфиденциальности в контроллере представления Safari в iOS 11. Теперь просмотр данных, таких как файлы cookie и локальное хранилище, доступен только для каждого приложения, а не для всех экземпляров контроллера представления Safari. Это позволяет сохранить частное действие обзора пользователей в приложении.

Дополнительные функции, такие как поддержка перетаскивания для URL-адресов и поддержка `window.open()`, также добавлены в `SFSafariViewController` в iOS 11. Дополнительные сведения об этих новых функциях см. в [документации Apple сфсафаривиевконтроллер](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller?changes=latest_minor).

## <a name="webkit"></a>WebKit

`WKWebView` было представлено в составе WebKit в iOS 8 как средство отображения веб-содержимого для пользователя. Это гораздо более настраиваемый, чем `SFSafariViewController`, что позволяет создавать собственные возможности навигации и пользовательский интерфейс.

В Apple появились три основных улучшения `WKWebView` с iOS 11: 

- Возможность управления файлами cookie
- Фильтрация содержимого
- Пользовательская загрузка ресурсов. 

Управление файлами cookie осуществляется с помощью нового класса [`WKHttpCookieStore`](https://developer.apple.com/documentation/webkit/wkhttpcookiestore) , который позволяет добавлять и удалять файлы cookie, получать все файлы cookie, хранящиеся в вквебвиев, и отслеживать изменения в хранилище файлов cookie.

Фильтрация содержимого позволяет управлять типом содержимого, которое будет видеть пользователь, что позволяет обеспечить его безопасность, удобство работы в семействе и, при необходимости, доступ только к выбранным группам пользователей. Это реализуется с помощью нового класса [`WKContentRuleList`](https://developer.apple.com/documentation/webkit/wkcontentrulelist) , предоставляя пары триггеров и действий в JSON. Дополнительные сведения об этих триггерах и действиях можно найти в статье о [правилах блокировки содержимого](https://developer.apple.com/library/content/documentation/Extensions/Conceptual/ContentBlockingRules/Introduction/Introduction.html) Apple.

Теперь iOS 11 позволяет настраивать `WKWebView` с пользовательской загрузкой ресурсов для веб-содержимого. Это реализуется с помощью интерфейса `IWKUrlSchemeHandler`, который позволяет управлять схемами URL-адресов, которые не являются собственными для веб-набора. Этот интерфейс имеет метод Start и останавливаться, который должен быть реализован:

```csharp
public class MyHandler : NSObject, IWKUrlSchemeHandler {

    [Export("webView:startURLSchemeTask:")]
    public void StartUrlSchemeTask(WKWebView webView, IWKUrlSchemeTask urlSchemeTask){
        
        // Implement a IWKUrlSchemeTask here
        var response = new NSUrlResponse(urlSchemeTask.Request.Url, "text/html", ContentLength, null);
        urlSchemeTask.DidReceiveResponse(response);
        urlSchemeTask.DidReceiveData(someData);
        urlSchemeTask.DidFinish();
    }

    [Export("webView:stopURLSchemeTask:")]
    public void StopUrlSchemeTask(WKWebView webView, IWKUrlSchemeTask urlSchemeTask){
        throw new NotImplementedException();
    }

}
``` 

После реализации обработчика используйте его для задания свойства `SetUrlSchemeHandler` в `WKWebViewConfiguration`. Затем загрузите URL-адрес объекта, который использует пользовательскую схему:

```csharp
var config = new WKWebViewConfiguration();
config.SetUrlSchemeHandler(new MyHandler(), "xamarin-asset");

webView = new WKWebView (View.Frame, config);
webView.LoadRequest (new NSUrlRequest("xamarin-asset://xamarin.com"));
```
