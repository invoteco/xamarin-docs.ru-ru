---
title: Изменения WebKit и Safari в iOS 11
description: В этом документе обсуждаются изменения, вносимые в WebKit и платформу служб Safari в iOS 11. В нем описывается работа с обновлениями стилей в Сфсафаривиевконтроллер и новые функции в Вквебвиев.
ms.prod: xamarin
ms.assetid: C74B2E94-177C-43D4-8D6C-9B528773C120
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 09/12/2017
ms.openlocfilehash: b90673559d0b8a3728898b7d8dbc3207bb22520b
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/04/2019
ms.locfileid: "70280082"
---
# <a name="webkit-and-safari-changes-in-ios-11"></a>Изменения WebKit и Safari в iOS 11

в iOS 11 появилась новая версия веб-браузера Safari — Safari 11,0, который включает изменения в WebKit и Сафарисервицес. В этом руководством рассматриваются эти изменения.

## <a name="safariservices"></a>сафарисервицес

`SFSafariViewController`была введена в iOS 9 в качестве варианта отображения веб-содержимого или проверки подлинности пользователей из приложения. Дополнительные сведения о его возможностях см. в разделе "Обзор [веб-представлений](~/ios/user-interface/controls/uiwebview.md#safariviewcontroller) ".

в iOS 11 появились обновления стиля для контроллера представления Safari, что позволяет пользователям более эффективно работать между приложением и Интернетом. Например, удаление адресной строки теперь дает контроллеру представления Safari поведение обозревателя в приложении, а не мини-браузер. Можно также настроить цветовую схему так, чтобы она соответствовала цветовой схеме приложения, задав `preferredBarTintColor` свойства и. `PreferredControlTintColor`

```csharp
sfViewController.PreferredControlTintColor = UIColor.White;
sfViewController.PreferredBarTintColor = UIColor.Purple;
```

Следующий фрагмент кода отображает столбцы в виде фиолетового и белого столбцов, как показано на следующем рисунке:

![Сфсафаривиевконтроллер панели, отображаемые фиолетовым и белым цветом](web-images/image1.png)

Кнопку Закрыть `Done`, представленную в контроллере представления Safari, можно также изменить, задав `DismissButtonStyle` для свойства значение, `Close`или `Cancel`.

```csharp
sfViewController.DismissButtonStyle = SFSafariViewControllerDismissButtonStyle.Close;
```

![Текст кнопки "Закрыть" изменен](web-images/image2.png)

Это значение можно изменить, пока `SFSafariViewController` представляется.


В зависимости от содержимого, отображаемого в контроллере представления Safari, может потребоваться не сворачивать строки меню при прокрутке пользователем. Это можно настроить, задав для `BarCollapsedEnabled` `false`нового свойства значение:

```csharp
var config = new SFSafariViewControllerConfiguration();
config.BarCollapsingEnabled = false;

var sfViewController = new SFSafariViewController(url, config);
```

![Свертывание строк отключено](web-images/image3.png)

Компания Apple также внесла изменения в сведения о конфиденциальности в контроллере представления Safari в iOS 11. Теперь просмотр данных, таких как файлы cookie и локальное хранилище, доступен только для каждого приложения, а не для всех экземпляров контроллера представления Safari. Это позволяет сохранить частное действие обзора пользователей в приложении.

`SFSafariViewController` В iOS 11 также добавлены дополнительные функции, такие как поддержка перетаскивания `window.open()` для URL-адресов и поддержки для. Дополнительные сведения об этих новых функциях см. в [документации Apple сфсафаривиевконтроллер](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller?changes=latest_minor).


## <a name="webkit"></a>WebKit

`WKWebView`был представлен в составе WebKit в iOS 8 как средство отображения веб-содержимого для пользователя. Это гораздо более настраиваемое, чем `SFSafariViewController`, что позволяет создавать собственные возможности навигации и пользовательский интерфейс.

В Apple появились три основных улучшения для `WKWebView` iOS 11: 

- Возможность управления файлами cookie
- Фильтрация содержимого
- Пользовательская загрузка ресурсов. 

Управление файлами cookie осуществляется с помощью нового [`WKHttpCookieStore`](https://developer.apple.com/documentation/webkit/wkhttpcookiestore) класса, который позволяет добавлять и удалять файлы cookie, получать все файлы cookie, хранящиеся в вквебвиев, и отслеживать изменения в хранилище файлов cookie.

Фильтрация содержимого позволяет управлять типом содержимого, которое будет видеть пользователь, что позволяет обеспечить его безопасность, удобство работы в семействе и, при необходимости, доступ только к выбранным группам пользователей. Это реализуется с помощью нового [`WKContentRuleList`](https://developer.apple.com/documentation/webkit/wkcontentrulelist) класса путем предоставления пар триггеров и действий в JSON. Дополнительные сведения об этих триггерах и действиях можно найти в статье о [правилах блокировки содержимого](https://developer.apple.com/library/content/documentation/Extensions/Conceptual/ContentBlockingRules/Introduction/Introduction.html) Apple.

Теперь iOS 11 позволяет настраивать `WKWebView` пользовательскую загрузку ресурсов для веб-содержимого. Это реализуется с помощью `IWKUrlSchemeHandler` интерфейса, который позволяет управлять схемами URL-адресов, которые не являются собственными для веб-набора. Этот интерфейс имеет метод Start и останавливаться, который должен быть реализован:

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

После реализации обработчика используйте его для задания `SetUrlSchemeHandler` свойства `WKWebViewConfiguration`в. Затем загрузите URL-адрес объекта, который использует пользовательскую схему:

```csharp
var config = new WKWebViewConfiguration();
config.SetUrlSchemeHandler(new MyHandler(), "xamarin-asset");

webView = new WKWebView (View.Frame, config);
webView.LoadRequest (new NSUrlRequest("xamarin-asset://xamarin.com"));
```

