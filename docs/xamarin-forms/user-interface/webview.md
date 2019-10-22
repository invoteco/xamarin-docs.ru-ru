---
title: WebView Xamarin. Forms
description: В этой статье объясняется, как использовать класс Xamarin. Forms WebView для представления локальных или сетевых веб-содержимого и документов пользователям.
ms.prod: xamarin
ms.assetid: E44F5D0F-DB8E-46C7-8789-114F1652A6C5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2019
ms.openlocfilehash: cdb2a9f1a37dc7bca458a4291ce6fe5ac9c120aa
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/21/2019
ms.locfileid: "72696044"
---
# <a name="xamarinforms-webview"></a>WebView Xamarin. Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithwebview)

[`WebView`](xref:Xamarin.Forms.WebView) — это представление для отображения содержимого веб-и HTML в приложении:

![В браузере приложений](webview-images/in-app-browser.png)

## <a name="content"></a>Content

`WebView` поддерживает следующие типы содержимого:

- Веб-сайты HTML & CSS &ndash; WebView полностью поддерживают веб-сайты, написанные с помощью HTML & CSS, включая поддержку JavaScript.
- Документы &ndash;, поскольку WebView реализуется с помощью собственных компонентов на каждой платформе, WebView может отображать документы, доступные для просмотра на каждой платформе. Это означает, что PDF-файлы работают в iOS и Android.
- Строки HTML &ndash; WebView могут показывать строки HTML из памяти.
- Локальные файлы &ndash; WebView могут представлять любой из типов содержимого, приведенных выше, внедренных в приложение.

> [!NOTE]
> `WebView` в Windows не поддерживает Silverlight, Flash или любые элементы управления ActiveX, даже если они поддерживаются Internet Explorer на этой платформе.

### <a name="websites"></a>Веб-сайты

Чтобы отобразить веб-сайт из Интернета, задайте для свойства [`Source`](xref:Xamarin.Forms.WebViewSource) `WebView` строку URL-адрес:

```csharp
var browser = new WebView
{
  Source = "http://xamarin.com"
};
```

> [!NOTE]
> URL-адреса должны быть полностью сформированы с указанным Протоколом (т. е. он должен иметь в начале префикса « http://» или « https://»).

#### <a name="ios-and-ats"></a>iOS и ATS

Начиная с версии 9, iOS позволяет приложению взаимодействовать только с серверами, которые по умолчанию реализуют оптимальную безопасность. Для обеспечения связи с небезопасными серверами необходимо задать значения в `Info.plist`.

> [!NOTE]
> Если приложению требуется подключение к незащищенному веб-сайту, следует всегда вводить домен как исключение с помощью `NSExceptionDomains` вместо того, чтобы полностью ATS с помощью `NSAllowsArbitraryLoads`. `NSAllowsArbitraryLoads` следует использовать только в чрезвычайных ситуациях.

Ниже показано, как включить конкретный домен (в данном случае xamarin.com) для обхода требований ATS:

```xml
<key>NSAppTransportSecurity</key>
    <dict>
        <key>NSExceptionDomains</key>
        <dict>
            <key>xamarin.com</key>
            <dict>
                <key>NSIncludesSubdomains</key>
                <true/>
                <key>NSTemporaryExceptionAllowsInsecureHTTPLoads</key>
                <true/>
                <key>NSTemporaryExceptionMinimumTLSVersion</key>
                <string>TLSv1.1</string>
            </dict>
        </dict>
    </dict>
    ...
</key>
```

Рекомендуется только разрешить некоторым доменам обходить ATS, что позволяет использовать Доверенные сайты, одновременно выигрыша от дополнительной безопасности в недоверенных доменах. Ниже показан менее безопасный метод отключения ATS для приложения.

```xml
<key>NSAppTransportSecurity</key>
    <dict>
        <key>NSAllowsArbitraryLoads </key>
        <true/>
    </dict>
    ...
</key>
```

Дополнительные сведения об этой новой функции в iOS 9 см. в разделе [Безопасность транспорта приложений](~/ios/app-fundamentals/ats.md) .

### <a name="html-strings"></a>Строки HTML

Если требуется предоставить в коде строку HTML, определенную динамически, необходимо создать экземпляр [`HtmlWebViewSource`](xref:Xamarin.Forms.HtmlWebViewSource):

```csharp
var browser = new WebView();
var htmlSource = new HtmlWebViewSource();
htmlSource.Html = @"<html><body>
  <h1>Xamarin.Forms</h1>
  <p>Welcome to WebView.</p>
  </body></html>";
browser.Source = htmlSource;
```

![WebView отображение HTML-строки](webview-images/html-string.png)

В приведенном выше коде `@` используется для пометки HTML как строкового литерала, что означает, что все обычные escape-символы игнорируются.

> [!NOTE]
> Может потребоваться установить свойства `WidthRequest` и `HeightRequest` [`WebView`](xref:Xamarin.Forms.WebView) для просмотра содержимого HTML в зависимости от макета, который `WebView` является дочерним элементом. Например, это необходимо в [`StackLayout`](xref:Xamarin.Forms.StackLayout).

### <a name="local-html-content"></a>Локальное содержимое HTML

WebView может отображать содержимое из HTML, CSS и JavaScript Embedded в приложении. Пример:

```html
<html>
  <head>
    <title>Xamarin Forms</title>
  </head>
  <body>
    <h1>Xamrin.Forms</h1>
    <p>This is an iOS web page.</p>
    <img src="XamarinLogo.png" />
  </body>
</html>
```

Каскад

```css
html,body {
  margin:0;
  padding:10;
}
body,p,h1 {
  font-family: Chalkduster;
}
```

Обратите внимание, что шрифты, указанные в приведенном выше CSS, должны быть настроены для каждой платформы, поскольку не все платформы имеют одинаковые шрифты.

Чтобы отобразить локальное содержимое с помощью `WebView`, необходимо открыть HTML-файл, как и любой другой, а затем загрузить содержимое в виде строки в свойство `Html` `HtmlWebViewSource`. Дополнительные сведения об открытии файлов см. в разделе [Работа с файлами](~/xamarin-forms/data-cloud/data/files.md).

На следующих снимках экрана показан результат отображения локального содержимого на каждой платформе:

![WebView Отображение локального содержимого](webview-images/local-content.png)

Хотя первая страница загружена, `WebView` не имеет сведений о том, откуда поступил HTML-код. Это проблема при работе со страницами, которые ссылаются на локальные ресурсы. Например, если локальные страницы связаны друг с другом, страница использует отдельный файл JavaScript или ссылку на страницу таблицы стилей CSS.  

Чтобы решить эту проблему, необходимо сообщить `WebView`, где искать файлы в файловой системе. Это можно сделать, задав свойство `BaseUrl` на `HtmlWebViewSource`, используемом `WebView`.

Так как файловая система в каждой операционной системе отличается, необходимо определить этот URL-адрес на каждой платформе. Xamarin. Forms предоставляет `DependencyService` для разрешения зависимостей во время выполнения на каждой платформе.

Чтобы использовать `DependencyService`, сначала определите интерфейс, который можно реализовать на каждой платформе:

```csharp
public interface IBaseUrl { string Get(); }
```

Обратите внимание, что пока интерфейс не будет реализован на каждой платформе, приложение не запустится. В общем проекте убедитесь, что вы не захотите задать `BaseUrl` с помощью `DependencyService`:

```csharp
var source = new HtmlWebViewSource();
source.BaseUrl = DependencyService.Get<IBaseUrl>().Get();
```

После этого необходимо предоставить реализации интерфейса для каждой платформы.

#### <a name="ios"></a>iOS

В iOS веб-содержимое должно находиться в корневом каталоге проекта или каталоге **ресурсов** с действием сборки *BundleResource*, как показано ниже:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Локальные файлы в iOS](webview-images/ios-vs.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio для Mac](#tab/macos)

![Локальные файлы в iOS](webview-images/ios-xs.png)

-----

В качестве `BaseUrl` должен быть указан путь к основному пакету:

```csharp
[assembly: Dependency (typeof (BaseUrl_iOS))]
namespace WorkingWithWebview.iOS
{
  public class BaseUrl_iOS : IBaseUrl
  {
    public string Get()
    {
      return NSBundle.MainBundle.BundlePath;
    }
  }
}
```

#### <a name="android"></a>Android

В Android поместите HTML, CSS и изображения в папку Assets с действием Build *AndroidAsset* , как показано ниже:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Локальные файлы на Android](webview-images/android-vs.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio для Mac](#tab/macos)

![Локальные файлы на Android](webview-images/android-xs.png)

-----

В Android для `BaseUrl` должно быть установлено значение `"file:///android_asset/"`:

```csharp
[assembly: Dependency (typeof(BaseUrl_Android))]
namespace WorkingWithWebview.Android
{
  public class BaseUrl_Android : IBaseUrl
  {
    public string Get()
    {
      return "file:///android_asset/";
    }
  }
}
```

В Android файлы в папке **Assets** также могут быть доступны через текущий контекст Android, который предоставляется свойством `MainActivity.Instance`:

```csharp
var assetManager = MainActivity.Instance.Assets;
using (var streamReader = new StreamReader (assetManager.Open ("local.html")))
{
  var html = streamReader.ReadToEnd ();
}
```

#### <a name="universal-windows-platform"></a>Универсальная платформа Windows

В проектах универсальная платформа Windows (UWP) размещайте HTML, CSS и изображения в корне проекта с действием Build, имеющим значение *Content*.

Для `BaseUrl` должно быть задано значение `"ms-appx-web:///"`:

```csharp
[assembly: Dependency(typeof(BaseUrl))]
namespace WorkingWithWebview.UWP
{
    public class BaseUrl : IBaseUrl
    {
        public string Get()
        {
            return "ms-appx-web:///";
        }
    }
}
```

## <a name="navigation"></a>Навигация

WebView поддерживает навигацию по нескольким методам и свойствам, которые он делает доступными:

- **GoForward ()** &ndash; если `CanGoForward` имеет значение true, вызов `GoForward` переходит вперед к следующей посещаемой странице.
- **GoBack ()** &ndash; если `CanGoBack` имеет значение true, вызов `GoBack` будет переходить на последнюю посещенную страницу.
- **CanGoBack** &ndash; `true`, если есть страницы для обратного перехода, `false`, если браузер находится на начальном URL-адресе.
- **Кангофорвард** &ndash; `true`, если пользователь перешел назад и может перейти вперед к уже посещенной странице.

На страницах `WebView` не поддерживает жесты с несколькими касаниями. Важно убедиться, что содержимое является оптимизированным для мобильных устройств и отображается без необходимости масштабирования.

Обычно для приложений отображается ссылка в `WebView`, а не в браузере устройства. В таких случаях полезно разрешить нормальную навигацию, но когда пользователь обращается к начальной ссылке, приложение должно вернуться к обычному представлению приложения.

Чтобы включить этот сценарий, используйте встроенные методы навигации и свойства.

Начните с создания страницы для представления браузера:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="WebViewSample.InAppBrowserXaml"
             Title="Browser">
    <StackLayout Margin="20">
        <StackLayout Orientation="Horizontal">
            <Button Text="Back" HorizontalOptions="StartAndExpand" Clicked="OnBackButtonClicked" />
            <Button Text="Forward" HorizontalOptions="EndAndExpand" Clicked="OnForwardButtonClicked" />
        </StackLayout>
        <!-- WebView needs to be given height and width request within layouts to render. -->
        <WebView x:Name="webView" WidthRequest="1000" HeightRequest="1000" />
    </StackLayout>
</ContentPage>
```

В коде программной части:

```csharp
public partial class InAppBrowserXaml : ContentPage
{
    public InAppBrowserXaml(string URL)
    {
        InitializeComponent();
        webView.Source = URL;
    }

    async void OnBackButtonClicked(object sender, EventArgs e)
    {
        if (webView.CanGoBack)
        {
            webView.GoBack();
        }
        else
        {
            await Navigation.PopAsync();
        }
    }

    void OnForwardButtonClicked(object sender, EventArgs e)
    {
        if (webView.CanGoForward)
        {
            webView.GoForward();
        }
    }
}
```

Вот и все!

![Кнопки навигации WebView](webview-images/in-app-browser.png)

## <a name="events"></a>события

WebView создает следующие события, помогающие реагировать на изменения в состоянии:

- [`Navigating`](xref:Xamarin.Forms.WebView.Navigating) — событие возникает, когда WebView начинает загрузку новой страницы.
- [`Navigated`](xref:Xamarin.Forms.WebView.Navigated) — событие, возникающее при загрузке страницы и остановке навигации.
- [`ReloadRequested`](xref:Xamarin.Forms.WebView.ReloadRequested) — событие, создаваемое при запросе на перезагрузку текущего содержимого.

Объект [`WebNavigatingEventArgs`](xref:Xamarin.Forms.WebNavigatingEventArgs) , сопровождающий событие [`Navigating`](xref:Xamarin.Forms.WebView.Navigating) , имеет четыре свойства:

- `Cancel` — указывает, следует ли отменить навигацию.
- `NavigationEvent` — событие навигации, которое было вызвано.
- `Source` — элемент, который выполнил навигацию.
- `Url` — назначение навигации.

Объект [`WebNavigatedEventArgs`](xref:Xamarin.Forms.WebNavigatedEventArgs) , сопровождающий событие [`Navigated`](xref:Xamarin.Forms.WebView.Navigated) , имеет четыре свойства:

- `NavigationEvent` — событие навигации, которое было вызвано.
- `Result` — описывает результат навигации с помощью члена перечисления [`WebNavigationResult`](xref:Xamarin.Forms.WebNavigationResult) . Допустимые значения: `Cancel`, `Failure`, `Success` и `Timeout`.
- `Source` — элемент, который выполнил навигацию.
- `Url` — назначение навигации.

Если предполагается использование веб-страниц, загрузка которых занимает много времени, рассмотрите возможность использования событий [`Navigating`](xref:Xamarin.Forms.WebView.Navigating) и [`Navigated`](xref:Xamarin.Forms.WebView.Navigated) для реализации индикатора состояния. Пример:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="WebViewSample.LoadingLabelXaml"
             Title="Loading Demo">
    <StackLayout>
        <!--Loading label should not render by default.-->
        <Label x:Name="labelLoading" Text="Loading..." IsVisible="false" />
        <WebView HeightRequest="1000" WidthRequest="1000" Source="http://www.xamarin.com" Navigated="webviewNavigated" Navigating="webviewNavigating" />
    </StackLayout>
</ContentPage>
```

Два обработчика событий:

```csharp
void webviewNavigating(object sender, WebNavigatingEventArgs e)
{
    labelLoading.IsVisible = true;
}

void webviewNavigated(object sender, WebNavigatedEventArgs e)
{
    labelLoading.IsVisible = false;
}
```

В результате получается следующий результат (Загрузка):

![Пример события навигации WebView](webview-images/loading-start.png)

Загрузка завершена:

![Пример события навигации WebView](webview-images/loading-end.png)

## <a name="reloading-content"></a>Перезагрузка содержимого

[`WebView`](xref:Xamarin.Forms.WebView) имеет метод `Reload`, который можно использовать для повторной загрузки текущего содержимого:

```csharp
var webView = new WebView();
...
webView.Reload();
```

При вызове метода `Reload` запускается событие `ReloadRequested`, указывающее на то, что был выполнен запрос на перезагрузку текущего содержимого.

## <a name="performance"></a>Производительность

Популярные веб-браузеры теперь принимают такие технологии, как визуализация с аппаратным ускорением и компиляция JavaScript. В iOS по умолчанию `WebView` Xamarin. Forms реализуется классом `UIWebView`, и многие из этих технологий недоступны в этой реализации. Однако приложение может явно использовать класс iOS `WkWebView` для реализации `WebView` Xamarin. Forms, который поддерживает более быстрый просмотр. Это можно сделать, добавив следующий код в файл **AssemblyInfo.CS** в проекте платформы iOS для приложения:

```csharp
// Opt-in to using WkWebView instead of UIWebView.
[assembly: ExportRenderer(typeof(WebView), typeof(Xamarin.Forms.Platform.iOS.WkWebViewRenderer))]
```

> [!NOTE]
> В iOS `WkWebViewRenderer` имеет перегрузку конструктора, которая принимает аргумент `WkWebViewConfiguration`. Это позволяет настроить модуль подготовки отчетов при создании.

`WebView` в Android по умолчанию — это так же быстро, как и встроенный браузер.

[WebView UWP](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/web-view) использует механизм визуализации Microsoft ребра. Для настольных и планшетных устройств должна отображаться такая же производительность, как и при использовании браузера Microsoft ребра.

## <a name="permissions"></a>Разрешения

Чтобы `WebView` работать, необходимо убедиться, что для каждой платформы заданы разрешения. Обратите внимание, что на некоторых платформах `WebView` будет работать в режиме отладки, но не при построении для выпуска. Это связано с тем, что некоторые разрешения, например для доступа к Интернету на Android, по умолчанию устанавливаются по Visual Studio для Mac в режиме отладки.

- Для &ndash; **UWP** требуется возможность Интернет (клиент & сервер) при отображении сетевого содержимого.
- @No__t_1 **Android** требует `INTERNET` только при отображении содержимого из сети. Для локального содержимого не требуются специальные разрешения.
- &ndash; **iOS** не требует специальных разрешений.

## <a name="layout"></a>Макет

В отличие от большинства других представлений Xamarin. Forms, `WebView` требует, чтобы `HeightRequest` и `WidthRequest` были указаны в StackLayout или RelativeLayout. Если не указать эти свойства, `WebView` не будет отображаться.

В следующих примерах демонстрируются макеты, которые приводят к работе, отрисовка `WebView`s:

StackLayout с Видсрекуест & Хеигхтрекуест:

```xaml
<StackLayout>
    <Label Text="test" />
    <WebView Source="http://www.xamarin.com/"
        HeightRequest="1000"
        WidthRequest="1000" />
</StackLayout>
```

RelativeLayout с Видсрекуест & Хеигхтрекуест:

```xaml
<RelativeLayout>
    <Label Text="test"
        RelativeLayout.XConstraint= "{ConstraintExpression
                                      Type=Constant, Constant=10}"
        RelativeLayout.YConstraint= "{ConstraintExpression
                                      Type=Constant, Constant=20}" />
    <WebView Source="http://www.xamarin.com/"
        RelativeLayout.XConstraint="{ConstraintExpression Type=Constant,
                                     Constant=10}"
        RelativeLayout.YConstraint="{ConstraintExpression Type=Constant,
                                     Constant=50}"
        WidthRequest="1000" HeightRequest="1000" />
</RelativeLayout>
```

Абсолутелайаут *без* Видсрекуест & хеигхтрекуест:

```xaml
<AbsoluteLayout>
    <Label Text="test" AbsoluteLayout.LayoutBounds="0,0,100,100" />
    <WebView Source="http://www.xamarin.com/"
      AbsoluteLayout.LayoutBounds="0,150,500,500" />
</AbsoluteLayout>
```

Сетка *без* Видсрекуест & хеигхтрекуест. Grid является одной из нескольких макетов, не требующих указания требуемых значений высоты и ширины.

```xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="100" />
        <RowDefinition Height="*" />
    </Grid.RowDefinitions>
    <Label Text="test" Grid.Row="0" />
    <WebView Source="http://www.xamarin.com/" Grid.Row="1" />
</Grid>
```

## <a name="invoking-javascript"></a>Вызов JavaScript

[`WebView`](xref:Xamarin.Forms.WebView) включает возможность вызывать функцию JavaScript из C#и возвращать результат в вызывающий C# код. Это достигается с помощью метода [`WebView.EvaluateJavaScriptAsync`](xref:Xamarin.Forms.WebView.EvaluateJavaScriptAsync*) , который показан в следующем примере из примера [WebView](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-webview) :

```csharp
var numberEntry = new Entry { Text = "5" };
var resultLabel = new Label();
var webView = new WebView();
...

int number = int.Parse(numberEntry.Text);
string result = await webView.EvaluateJavaScriptAsync($"factorial({number})");
resultLabel.Text = $"Factorial of {number} is {result}.";
```

Метод [`WebView.EvaluateJavaScriptAsync`](xref:Xamarin.Forms.WebView.EvaluateJavaScriptAsync*) оценивает JavaScript, указанный в качестве аргумента, и возвращает любой результат в виде `string`. В этом примере вызывается `factorial` функция JavaScript, которая возвращает факториал `number` в результате. Эта функция JavaScript определена в локальном HTML-файле, который загружается [`WebView`](xref:Xamarin.Forms.WebView) и показан в следующем примере:

```html
<html>
<body>
<script type="text/javascript">
function factorial(num) {
        if (num === 0 || num === 1)
            return 1;
        for (var i = num - 1; i >= 1; i--) {
            num *= i;
        }
        return num;
}
</script>
</body>
</html>
```

## <a name="related-links"></a>Связанные ссылки

- [Работа с WebView (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithwebview)
- [WebView (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-webview)
