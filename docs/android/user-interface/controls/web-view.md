---
title: Веб-представление
ms.prod: xamarin
ms.assetid: 807F214A-166D-B342-0BBA-525517577F6B
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/06/2018
ms.openlocfilehash: c1fcb16bd40b818b27b57b877534e051a789a6c9
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029024"
---
# <a name="xamarinandroid-web-view"></a>Веб-представление Xamarin. Android

[`WebView`](xref:Android.Webkit.WebView) позволяет создать собственное окно для просмотра веб-страниц (или даже разработать полный браузер). В этом учебнике вы создадите простой [`Activity`](xref:Android.App.Activity)
для просмотра и навигации по веб-страницам.

Создайте новый проект с именем **хелловебвиев**.

Откройте **ресурсы/макет/Main. axml** и вставьте следующее:

```xml
<?xml version="1.0" encoding="utf-8"?>
<WebView  xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/webview"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent" />
```

Так как это приложение будет иметь доступ к Интернету, необходимо добавить соответствующие разрешения в файл манифеста Android. Откройте свойства проекта, чтобы указать, какие разрешения требуются для работы приложения. Включите разрешение `INTERNET`, как показано ниже.

![Настройка разрешения Интернета в манифесте Android](web-view-images/01-set-internet-permissions.png)

Теперь откройте **MainActivity.CS** и добавьте директиву using для WebKit:

```csharp
using Android.Webkit;
```

В верхней части класса `MainActivity` объявите объект [`WebView`](xref:Android.Webkit.WebView) :

```csharp
WebView web_view;
```

Когда **WebView** запрашивает загрузку URL-адреса, он по умолчанию делегирует запрос браузеру по умолчанию. Чтобы **WebView** загружал URL-адрес (а не браузер по умолчанию), необходимо создать подкласс `Android.Webkit.WebViewClient` и переопределить метод `ShouldOverriderUrlLoading`. Экземпляр этого пользовательского `WebViewClient` предоставляется для `WebView`. Для этого добавьте следующий вложенный класс `HelloWebViewClient` в `MainActivity`:

```csharp
public class HelloWebViewClient : WebViewClient
{
    public override bool ShouldOverrideUrlLoading (WebView view, string url)
    {
        view.LoadUrl(url);
        return false;
    }
}
```

Когда `ShouldOverrideUrlLoading` возвращает `false`, он сообщает Android, что текущий экземпляр `WebView` обработал запрос и не требует дальнейших действий. 

Если вы нацелены на уровень API 24 или более поздней версии, используйте перегрузку `ShouldOverrideUrlLoading`, которая принимает `IWebResourceRequest` для второго аргумента вместо `string`:

```csharp
public class HelloWebViewClient : WebViewClient
{
    // For API level 24 and later
    public override bool ShouldOverrideUrlLoading (WebView view, IWebResourceRequest request)
    {
        view.LoadUrl(request.Url.ToString());
        return false;
    }
}
```

Затем используйте следующий код для метода [`OnCreate()`](xref:Android.App.Activity.OnCreate*)):

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    // Set our view from the "main" layout resource
    SetContentView (Resource.Layout.Main);

    web_view = FindViewById<WebView> (Resource.Id.webview);
    web_view.Settings.JavaScriptEnabled = true;
    web_view.SetWebViewClient(new HelloWebViewClient());
    web_view.LoadUrl ("https://www.xamarin.com/university");
}
```

Этот элемент инициализирует член [`WebView`](xref:Android.Webkit.WebView) с помощью макета [`Activity`](xref:Android.App.Activity) и включает JavaScript для [`WebView`](xref:Android.Webkit.WebView) с [`JavaScriptEnabled`](xref:Android.Webkit.WebSettings.JavaScriptEnabled)
`= true` (см. [вызов\# из набора JavaScript](https://github.com/xamarin/recipes/tree/master/Recipes/android/controls/webview/call_csharp_from_javascript) для сведения о вызове функций C\# из JavaScript). Наконец, загружается начальная веб-страница с [`LoadUrl(String)`](xref:Android.Webkit.WebView).

Выполните сборку и запуск приложения. Вы увидите простое приложение просмотра веб-страниц, как показано на следующем снимке экрана:

[![пример приложения, отображающего WebView](web-view-images/02-simple-webview-app-sml.png)](web-view-images/02-simple-webview-app.png#lightbox)

Чтобы выполнить нажатие клавиши " **назад** ", добавьте следующую инструкцию using:

```csharp
using Android.Views;
```

Затем добавьте следующий метод в действие `HelloWebView`:

```csharp
public override bool OnKeyDown (Android.Views.Keycode keyCode, Android.Views.KeyEvent e)
{
    if (keyCode == Keycode.Back && web_view.CanGoBack ())
    {
        web_view.GoBack ();
        return true;
    }
    return base.OnKeyDown (keyCode, e);
}
```

Этот [`OnKeyDown(int, KeyEvent)`](xref:Android.App.Activity.OnKeyDown*)
метод обратного вызова будет вызываться при каждом нажатии кнопки во время выполнения действия. Условие в использует [`KeyEvent`](xref:Android.Views.KeyEvent) , чтобы проверить, является ли нажатой клавишей кнопку **назад** , а также определить, может ли [`WebView`](xref:Android.Webkit.WebView) на самом деле переходить назад (если у него есть журнал). Если оба значения имеют значение true, вызывается метод [`GoBack()`](xref:Android.Webkit.WebView.GoBack) , который будет переходить назад на один шаг в журнале [`WebView`](xref:Android.Webkit.WebView) . Возврат `true` указывает, что событие было обработано. Если это условие не выполнено, событие отправляется обратно в систему.

Снова запустите приложение. Теперь вы сможете следовать ссылкам и переходить назад к журналу страниц:

[![пример снимков экрана кнопки "назад" в действии](web-view-images/03-back-button-sml.png)](web-view-images/03-back-button.png#lightbox)

*Части этой страницы являются изменениями на основе работы, созданной и совместно используемой проектом Android с открытым кодом, и используются в соответствии с терминами, описанными в* [*лицензии
Creative Commons Attribution 2,5*](https://creativecommons.org/licenses/by/2.5/).

## <a name="related-links"></a>Связанные ссылки

- [Вызов C# из JavaScript](https://github.com/xamarin/recipes/tree/master/Recipes/android/controls/webview/call_csharp_from_javascript)
- [Android. WebKit. WebView](xref:Android.Webkit.WebView)
- [кэйевент](xref:Android.Webkit.WebView)
