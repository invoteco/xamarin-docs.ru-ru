---
title: Веб-представления в Xamarin. iOS
description: В этом документе описаны различные способы, с помощью которых приложение Xamarin. iOS может отображать веб-содержимое. В нем обсуждаются Уивебвиев, Вквебвиев, Сфсафаривиевконтроллер, Safari и безопасность транспорта приложений.
ms.prod: xamarin
ms.assetid: 84886CF4-2B2B-4540-AD92-7F0B791952D1
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/22/2017
ms.openlocfilehash: 210e59239957d3963a3d3275315a0eac14748ff8
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73021789"
---
# <a name="web-views-in-xamarinios"></a>Веб-представления в Xamarin. iOS

В течение времени существования iOS Apple было выпущено несколько способов реализации функциональных возможностей веб-представлений в приложениях для разработчиков приложений. Большинство пользователей использует встроенный веб-браузер Safari на устройстве iOS и, следовательно, предполагаю, что функциональность веб-представлений других приложений согласуется с этим интерфейсом. Они предполагают работу одних и тех же жестов, а также производительность по номиналу и функциональность.

В этой статье рассматриваются все три веб-представления, предоставляемые Apple: `UIWebView`, `WKWebview`и `SFSafariViewController`, их сходства и различия, а также способы их использования. 

в iOS 11 появились новые изменения как в `WKWebView`, так и в `SFSafariViewController`. Дополнительные сведения см. в статье об [изменениях в Интернете в программе](~/ios/platform/introduction-to-ios11/web.md) "Пошаговое руководством по iOS 11".

## <a name="uiwebview"></a>UIWebView

`UIWebView` является устаревшим способом Apple для предоставления веб-содержимого в приложении. Она была выпущена в iOS 2,0 и является устаревшей по отношению к 8,0.

Если вы планируете поддерживать версии iOS более ранние, чем 8,0, потребуется использовать `UIWebView`. Из-за того, что `UIWebView` менее оптимизирована для производительности, чем альтернативные варианты, рекомендуется проверить версию iOS пользователя. Если он 8,0 или более поздней версии, использование любого из параметров, описанных ниже, поможет улучшить взаимодействие с пользователем.

Чтобы добавить Уивебвиев в приложение Xamarin. iOS, используйте следующий код:

```
webView = new UIWebView (View.Bounds);
View.AddSubview(webView);

var url = "https://xamarin.com"; // NOTE: https secure request
webView.LoadRequest(new NSUrlRequest(new NSUrl(url)));
```

В результате будет создано следующее веб-представление:

[![](uiwebview-images/webview.png "The effect of ScalesPagesToFit")](uiwebview-images/webview.png#lightbox)

Дополнительные сведения об использовании `UIWebView`см. в следующих рецептах:

- [Загрузка веб-страницы](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/web_view/load_a_web_page)
- [Загрузка локального содержимого](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/web_view/load_local_content)
- [Загрузка документов, не относящихся к веб-сайтам](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/web_view/load_non-web_documents)

## <a name="wkwebview"></a>вквебвиев

`WKWebView` появился в iOS 8, позволяя разработчикам приложений реализовать интерфейс веб-браузера, аналогичный мобильному Safari. Это связано с тем, что `WKWebView` использует подсистему JavaScript нитро, ту же подсистему, которая используется мобильным обозревателем Safari. `WKWebView` должны всегда использоваться по сравнению с Уивебвиев, так как это было возможно благодаря [повышению производительности](http://blog.initlabs.com/post/100113463211/wkwebview-vs-uiwebview), встроенным жестам, удобным для пользователя, и простоте взаимодействия между веб-страницей и приложением.
  
`WKWebView` можно добавить в приложение практически аналогичным образом, так как разработчик имеет гораздо больший контроль над пользовательским интерфейсом, UX и функциональностью. При создании и отображении объекта веб-представления отображается запрошенная страница, однако вы можете управлять представлением представления, тем, как пользователь может перемещаться и как пользователь выходит из представления.  

Приведенный ниже код можно использовать для запуска `WKWebView` в приложении Xamarin. iOS:

```csharp
    WKWebView webView = new WKWebView(View.Frame, new WKWebViewConfiguration());
    View.AddSubview(webView);

    var url = new NSUrl("https://xamarin.com");
    var request = new NSUrlRequest(url);
    webView.LoadRequest(request);
```

В результате будет создано следующее веб-представление:

[![](uiwebview-images/wkwebview.png "An example web view without ScalesPagesToFit")](uiwebview-images/wkwebview.png#lightbox)

Важно отметить, что `WKWebView` находится в пространстве имен WebKit, поэтому вам придется добавить эту директиву using в начало класса.

`WKWebView` также можно использовать в приложениях Xamarin. Mac, поэтому при создании кросс-платформенного приложения Mac/iOS может потребоваться использовать его.

В инструкции по [обработке оповещений JavaScript](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/web_view/handle_javascript_alerts) также содержатся сведения об использовании Вквебвиев с JavaScript.

<a name="safariviewcontroller" />

## <a name="sfsafariviewcontroller"></a>сфсафаривиевконтроллер

 `SFSafariViewController` — это последний способ предоставления веб-содержимого из приложения и доступен в iOS 9 и более поздних версиях. В отличие от `UIWebView` или `WKWebView`, `SFSafariViewController` является контроллером представления и поэтому не может использоваться с другими представлениями. Вы должны представлять `SFSafariViewController` как новый контроллер представления, точно так же, как и любой контроллер представления.

 `SFSafariViewController` по сути является мини-обозревателем Safari, который можно внедрить в приложение. Как и Вквебвиев, он использует тот же механизм JavaScript нитро, но также предоставляет ряд дополнительных функций Safari, таких как автозаполнение, читатель и возможность совместного использования файлов cookie и данных с помощью мобильного обозревателя Safari. Взаимодействие между пользователем и `SFSafariViewController` недоступно для вашего приложения. Приложение не будет иметь доступ к каким бы то ни было функциям Safari по умолчанию.

Кроме того, по умолчанию реализуется кнопка **done (Готово** ), позволяющая пользователю легко вернуться в приложение, а также кнопки перехода и обратно, позволяющие пользователю перемещаться по стеку веб-страниц. Кроме того, он также предоставляет пользователю, которому необходимо предоставить уверенность в том, что они находятся на ожидаемой веб-странице. Адресная строка не разрешает пользователю изменять URL-адрес. 

Эти реализации не могут быть изменены, поэтому `SFSafariViewController` идеально подходит для использования в качестве браузера по умолчанию, если приложение хочет предоставить веб-страницу без настройки.

Приведенный ниже код можно использовать для запуска `SFSafariViewController` в приложении Xamarin. iOS:

```csharp
var sfViewController = new SFSafariViewController(url);

PresentViewController(sfViewController, true, null);
```

В результате будет создано следующее веб-представление:

[![](uiwebview-images/sfsafariviewcontroller.png "An example web view with SFSafariViewController")](uiwebview-images/sfsafariviewcontroller.png#lightbox)

## <a name="safari"></a>Safari

Вы также можете открыть мобильное приложение Safari в приложении, используя приведенный ниже код.

```csharp
var url = new NSUrl("https://xamarin.com");

UIApplication.SharedApplication.OpenUrl(url);

```

В результате будет создано следующее веб-представление:

[![](uiwebview-images/safari.png "A web page presented in Safari")](uiwebview-images/safari.png#lightbox)

В большинстве случаев следует избегать переходов пользователей из приложения в Safari. Большинство пользователей не будут получать переходы за пределы приложения, поэтому при переходе от приложения пользователи могут никогда не возвращать его, по сути, выбросить задействование.

улучшения iOS 9 позволяют пользователю легко вернуться в приложение с помощью кнопки назад, представленной в левом верхнем углу страницы Safari.

## <a name="app-transport-security"></a>Защита транспорта приложения

Защита транспорта приложений или *ATS* была введена компанией Apple в iOS 9 для обеспечения соответствия всем Интернет – рекомендациям по обеспечению безопасного подключения.

Дополнительные сведения о ATS, в том числе о том, как ее реализовать в приложении, см. в руководстве по [безопасности транспорта приложений](~/ios/app-fundamentals/ats.md) .

## <a name="related-links"></a>Связанные ссылки

- [Представления (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/webview)
