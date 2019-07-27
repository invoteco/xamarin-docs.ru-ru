---
title: Веб-представление
ms.prod: xamarin
ms.assetid: 807F214A-166D-B342-0BBA-525517577F6B
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: 2718953c9e5628374c45fa3741d1ad3be3125dd9
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/26/2019
ms.locfileid: "68510148"
---
# <a name="xamarinandroid-web-view"></a>Веб-представление Xamarin. Android

[`WebView`](xref:Android.Webkit.WebView)позволяет создать собственное окно для просмотра веб-страниц (или даже разработать полный браузер). В этом учебнике вы создадите простую[`Activity`](xref:Android.App.Activity)
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

Так как это приложение будет иметь доступ к Интернету, необходимо добавить соответствующие разрешения в файл манифеста Android. Откройте свойства проекта, чтобы указать, какие разрешения требуются для работы приложения. `INTERNET` Включите разрешение, как показано ниже.

![Настройка разрешения Интернета в манифесте Android](web-view-images/01-set-internet-permissions.png)

Теперь откройте **MainActivity.CS** и добавьте директиву using для WebKit:

```csharp
using Android.Webkit;
```

В верхней `MainActivity` части класса [`WebView`](xref:Android.Webkit.WebView) объявите объект:

```csharp
WebView web_view;
```

Когда **WebView** запрашивает загрузку URL-адреса, он по умолчанию делегирует запрос браузеру по умолчанию. Чтобы **WebView** загружал URL-адрес (а не браузер по умолчанию), необходимо создать подкласс `Android.Webkit.WebViewClient` и переопределить `ShouldOverriderUrlLoading` метод. Экземпляр этого пользовательского `WebViewClient` класса предоставляется `WebView`для. Для этого добавьте следующий вложенный `HelloWebViewClient` `MainActivity`класс в:

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

При `ShouldOverrideUrlLoading` возврате `false`он передает сигналу Android о том `WebView` , что текущий экземпляр обработал запрос и что дальнейшие действия не требуются. 

Если вы нацелены на уровень API 24 или более поздней версии, используйте `ShouldOverrideUrlLoading` перегрузку `IWebResourceRequest` , которая принимает для `string`второго аргумента, а не:

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

Затем используйте следующий код для [`OnCreate()`](xref:Android.App.Activity.OnCreate*)метода):

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

При [`WebView`](xref:Android.Webkit.WebView) этом член инициализируется с помощью [`Activity`](xref:Android.App.Activity) из макета [`WebView`](xref:Android.Webkit.WebView) и включается в JavaScript для с помощью [`JavaScriptEnabled`](xref:Android.Webkit.WebSettings.JavaScriptEnabled) 
 `= true` (см. [вызов C\# из JavaScript](https://github.com/xamarin/recipes/tree/master/Recipes/android/controls/webview/call_csharp_from_javascript) ). рецепт для получения сведений о вызове функций\# C из JavaScript. Наконец, загружается начальная веб- [`LoadUrl(String)`](xref:Android.Webkit.WebView)страница с.

Выполните сборку и запуск приложения. Вы увидите простое приложение просмотра веб-страниц, как показано на следующем снимке экрана:

[![Пример приложения, отображающего WebView](web-view-images/02-simple-webview-app-sml.png)](web-view-images/02-simple-webview-app.png#lightbox)

Чтобы выполнить нажатие клавиши " **назад** ", добавьте следующую инструкцию using:

```csharp
using Android.Views;
```

Затем добавьте в `HelloWebView` действие следующий метод:

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

Настоящего[`OnKeyDown(int, KeyEvent)`](xref:Android.App.Activity.OnKeyDown*)
метод обратного вызова будет вызываться при каждом нажатии кнопки во время выполнения действия. Условие в использует [`KeyEvent`](xref:Android.Views.KeyEvent) для проверки того, является ли нажатой клавишей « **назад** », и [`WebView`](xref:Android.Webkit.WebView) может ли объект, который может перемещаться назад (если у него есть журнал), был в действительности. Если оба значения имеют значение true, [`GoBack()`](xref:Android.Webkit.WebView.GoBack) вызывается метод, который будет переходить назад [`WebView`](xref:Android.Webkit.WebView) на один шаг в журнале. Возврат `true` указывает, что событие было обработано. Если это условие не выполнено, событие отправляется обратно в систему.

Снова запустите приложение. Теперь вы сможете следовать ссылкам и переходить назад к журналу страниц:

[![Пример снимков экрана кнопки "назад" в действии](web-view-images/03-back-button-sml.png)](web-view-images/03-back-button.png#lightbox)

*Части этой страницы являются изменениями на основе работы, созданной и совместно используемой проектом Android с открытым кодом, и используются в соответствии с терминами,* 
описанными в[*лицензии Creative Commons Attribution 2,5*](http://creativecommons.org/licenses/by/2.5/).

## <a name="related-links"></a>Связанные ссылки

- [Вызов C# из JavaScript](https://github.com/xamarin/recipes/tree/master/Recipes/android/controls/webview/call_csharp_from_javascript)
- [Android. WebKit. WebView](xref:Android.Webkit.WebView)
- [KeyEvent](xref:Android.Webkit.WebView)
