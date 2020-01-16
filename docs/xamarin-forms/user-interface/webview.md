---
title: Веб-представления Xamarin.Forms
description: В этой статье объясняется, как использовать класс Xamarin.Forms WebView для представления локального или сетевого веб-содержимого и документы пользователям.
ms.prod: xamarin
ms.assetid: E44F5D0F-DB8E-46C7-8789-114F1652A6C5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/04/2019
ms.openlocfilehash: dedce45d0c09f807aaf2ecbf540b8c9f319a4f16
ms.sourcegitcommit: 3e94c6d2b6d6a70c94601e7bf922d62c4a6c7308
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/15/2020
ms.locfileid: "76031379"
---
# <a name="xamarinforms-webview"></a>Веб-представления Xamarin.Forms

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithwebview)

[`WebView`](xref:Xamarin.Forms.WebView) — это представление для отображения содержимого веб-и HTML в приложении:

![В браузере приложений](webview-images/in-app-browser.png)

## <a name="content"></a>Content

`WebView` поддерживает следующие типы содержимого:

- Веб-сайтов HTML и CSS &ndash; WebView имеет полную поддержку для веб-сайтов, написанные с использованием HTML и CSS, включая поддержку JavaScript.
- Документы &ndash; так, как веб-представление реализуется с помощью собственных компонентов на каждой платформе, WebView способен отображать документы, которые можно просмотреть на каждой платформе. Это означает, что PDF-файлов работать в iOS и Android.
- Строк HTML &ndash; WebView можно показать строки HTML из памяти.
- Локальные файлы &ndash; веб-представление может представлять любой из типов содержимого, перечисленных выше внедренных в приложение.

> [!NOTE]
> `WebView` в Windows не поддерживает Silverlight, Flash или все элементы управления ActiveX, даже если они поддерживаются обозревателем Internet Explorer на этой платформе.

### <a name="websites"></a>Веб-сайты

Для отображения веб-сайта из Интернета, задайте `WebView` [ `Source` ](xref:Xamarin.Forms.WebViewSource) свойство на строку URL-адрес:

```csharp
var browser = new WebView
{
  Source = "http://xamarin.com"
};
```

> [!NOTE]
> URL-адреса должен быть полностью сформирован с протоколом, заданным (т. е. он должен иметь « http://» или « https://», прикрепленный к нему).

#### <a name="ios-and-ats"></a>iOS и ATS

Начиная с версии 9 iOS только позволит приложению для взаимодействия с серверами, реализующие рекомендации безопасности по умолчанию. Значения следует передавать `Info.plist` для обеспечения взаимодействия с серверами небезопасным.

> [!NOTE]
> Если приложению требуется подключение на небезопасной веб-сайте, следует всегда введите домен как исключений с помощью `NSExceptionDomains` вместо отключение ATS полностью с помощью `NSAllowsArbitraryLoads`. `NSAllowsArbitraryLoads` должен использоваться только в чрезвычайных ситуациях аварийного.

Ниже показано, как включить определенного домена (в этом вариантов xamarin.com), чтобы обойти требования к ATS:

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

Его рекомендуется включать только некоторые домены для обхода ATS, позволяя использовать надежные узлы при пользоваться преимуществами дополнительную надежность в недоверенных доменах. Следующая команда демонстрирует метод менее безопасно отключить ATS для приложения:

```xml
<key>NSAppTransportSecurity</key>
    <dict>
        <key>NSAllowsArbitraryLoads </key>
        <true/>
    </dict>
    ...
</key>
```

См. в разделе [безопасность транспорта приложения](~/ios/app-fundamentals/ats.md) Дополнительные сведения об этой новой функции в iOS 9.

### <a name="html-strings"></a>Строк HTML

Если вы хотите представить строка HTML, определенные динамически в коде, необходимо создать экземпляр [ `HtmlWebViewSource` ](xref:Xamarin.Forms.HtmlWebViewSource):

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

В приведенном выше коде `@` используется для пометки HTML как строка литерала, то есть все обычные escape-символы учитываются.

> [!NOTE]
> Может потребоваться установить свойства `WidthRequest` и `HeightRequest` [`WebView`](xref:Xamarin.Forms.WebView) для просмотра содержимого HTML в зависимости от макета, который `WebView` является дочерним элементом. Например, это необходимо в [`StackLayout`](xref:Xamarin.Forms.StackLayout).

### <a name="local-html-content"></a>Локальное содержимое HTML

Веб-представление может отображать содержимое HTML, CSS и Javascript, встроенным в приложении. Например:

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

CSS:

```css
html,body {
  margin:0;
  padding:10;
}
body,p,h1 {
  font-family: Chalkduster;
}
```

Обратите внимание на то, что шрифты, указанные в таблице выше CSS будет необходимо настроить для каждой платформы, как не каждая платформа имеет шрифты.

Для отображения локального содержимого с помощью `WebView`, вам потребуется открыть HTML-файл, как и любое другое, а затем загрузить содержимое в виде строки в `Html` свойство `HtmlWebViewSource`. Дополнительные сведения о открытия файлов, см. в разделе [работа с файлами](~/xamarin-forms/data-cloud/data/files.md).

На следующих снимках экрана показано отображение локального содержимого на каждой платформе результат:

![WebView Отображение локального содержимого](webview-images/local-content.png)

Несмотря на то, что первая страница загрузится, `WebView` не имеет сведений о происхождения HTML. Это проблема, при работе со страницами, которые ссылаются на локальные ресурсы. Когда это может случиться примеры при другом, страницы делает ссылку локальных страниц использовать отдельный файл JavaScript, или страница ссылается на таблицу стилей CSS.  

Чтобы решить эту проблему, необходимо указать `WebView` место для поиска файлов в файловой системе. Этого нужно задать `BaseUrl` свойство `HtmlWebViewSource` используемые `WebView`.

Поскольку разные файловой системы на каждом из операционных систем, необходимо определить этот URL-адрес, на каждой платформе. Xamarin.Forms предоставляет `DependencyService` для разрешения зависимостей во время выполнения на каждой платформе.

Чтобы использовать `DependencyService`, сначала Определите интерфейс, который может быть реализован на каждой платформе:

```csharp
public interface IBaseUrl { string Get(); }
```

Обратите внимание на то, что до этого интерфейса возлагается на каждой платформе, приложение не запустится. В проекте common, убедитесь, что не забудьте установить для `BaseUrl` с помощью `DependencyService`:

```csharp
var source = new HtmlWebViewSource();
source.BaseUrl = DependencyService.Get<IBaseUrl>().Get();
```

Затем необходимо предоставить реализации интерфейса для каждой платформы.

#### <a name="ios"></a>iOS

В iOS, веб-содержимого должен быть расположен в корневом каталоге проекта или **ресурсы** каталог с действием сборки *BundleResource*, как показано ниже:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Локальные файлы в iOS](webview-images/ios-vs.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio для Mac](#tab/macos)

![Локальные файлы в iOS](webview-images/ios-xs.png)

-----

`BaseUrl` Должно быть присвоено путь основного пакета:

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

В Android, поместите HTML, CSS и изображений в папку средств с действием сборки *AndroidAsset* как показано ниже:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Локальные файлы на Android](webview-images/android-vs.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio для Mac](#tab/macos)

![Локальные файлы на Android](webview-images/android-xs.png)

-----

В Android `BaseUrl` должно быть присвоено `"file:///android_asset/"`:

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

В Android, файлы в **активы** папка может также осуществляться через контекст текущего Android, который предоставляется командлетом `MainActivity.Instance` свойство:

```csharp
var assetManager = MainActivity.Instance.Assets;
using (var streamReader = new StreamReader (assetManager.Open ("local.html")))
{
  var html = streamReader.ReadToEnd ();
}
```

#### <a name="universal-windows-platform"></a>Универсальная платформа Windows

В проектах универсальной платформы Windows (UWP), поместите HTML, CSS и изображений в корневую папку проекта с действием сборки *содержимого*.

`BaseUrl` Должно быть присвоено `"ms-appx-web:///"`:

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

## <a name="navigation"></a>Навигация в

Веб-представления поддерживает навигацию по несколько методов и свойств, которые делает доступными:

- **GoForward()** &ndash; Если `CanGoForward` имеет значение true, вызвав `GoForward` переходит вперед на следующую страницу посещенных.
- **GoBack()** &ndash; Если `CanGoBack` имеет значение true, вызвав `GoBack` будет перейти к последней страницы.
- **CanGoBack** &ndash; `true` при наличии страниц для обратного перехода, `false` Если браузер находится в начальный URL-адрес.
- **Значение свойства CanGoForward** &ndash; `true` Если пользователь переходит назад и перейдите к странице, который уже был посещен.

На страницах `WebView` не поддерживает мультисенсорные жесты. Это важно, чтобы убедиться в том что содержимое, оптимизированный для мобильных устройств и отображается без необходимости для масштабирования.

Обычно для приложений отобразить ссылку в `WebView`, вместо того чтобы браузера устройства. В этих случаях полезно разрешить нормальный переход, но при попаданий пользователя создать резервную копию, а на начальную ссылку, приложение должно вернуть представление обычное приложение.

Используйте встроенные средства навигации методы и свойства, чтобы реализовать этот сценарий.

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

Веб-представления вызывает следующие события для реагирования на изменения в состоянии:

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

Если предполагается использование веб-страниц, загрузка которых занимает много времени, рассмотрите возможность использования событий [`Navigating`](xref:Xamarin.Forms.WebView.Navigating) и [`Navigated`](xref:Xamarin.Forms.WebView.Navigated) для реализации индикатора состояния. Например:

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

Это приводит к приведенным ниже (загрузка):

![Пример события навигации WebView](webview-images/loading-start.png)

По завершении загрузки:

![Пример события навигации WebView](webview-images/loading-end.png)

## <a name="reloading-content"></a>Повторная загрузка содержимого

[`WebView`](xref:Xamarin.Forms.WebView) имеет `Reload` метод, который может использоваться для перезагрузки текущее содержимое:

```csharp
var webView = new WebView();
...
webView.Reload();
```

Когда `Reload` вызывается метод `ReloadRequested` инициируется событие, указывающее на то, что запрос на перезагрузку текущего содержимого.

## <a name="performance"></a>Производительность

Популярные веб-браузеры принимают такие технологии, как визуализация с аппаратным ускорением и компиляция JavaScript. До Xamarin. Forms 4,4 `WebView` Xamarin. Forms был реализован в iOS классом `UIWebView`. Однако многие из этих технологий были недоступны в этой реализации. Таким образом, поскольку Xamarin. Forms 4,4, `WebView` Xamarin. Forms реализуется в iOS с помощью класса `WkWebView`, который поддерживает более быстрый просмотр.

> [!NOTE]
> В iOS `WkWebViewRenderer` имеет перегрузку конструктора, которая принимает аргумент `WkWebViewConfiguration`. Это позволяет настроить модуль подготовки отчетов при создании.

Приложение может вернуться к использованию класса `UIWebView` iOS для реализации `WebView`Xamarin. Forms в целях совместимости. Это можно сделать, добавив следующий код, чтобы **AssemblyInfo.cs** файл в iOS проект платформы для приложения:

```csharp
// Opt-in to using UIWebView instead of WkWebView.
[assembly: ExportRenderer(typeof(Xamarin.Forms.WebView), typeof(Xamarin.Forms.Platform.iOS.WebViewRenderer))]
```

`WebView` в Android по умолчанию почти так же быстро, как встроенного браузера.

[UWP WebView](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/web-view) использует механизм отрисовки Microsoft Edge. Настольных и планшетных устройств должны увидеть ту же производительность как с помощью сам браузер Edge.

## <a name="permissions"></a>Разрешения

Чтобы `WebView` для работы, необходимо убедиться в том что разрешения задаются для каждой платформы. Обратите внимание, что на некоторых платформах `WebView` работает в режиме отладки, но не в том случае, при построении для выпуска. Том, что некоторые разрешения, как и для доступа к Интернету на устройстве Android, устанавливаются по умолчанию с помощью Visual Studio для Mac при работе в режиме отладки.

- **UWP** &ndash; требуются возможности Интернет (клиент и сервер), при отображении содержимого сети.
- **Android** &ndash; требует `INTERNET` только в том случае, если отображение содержимого из сети. Локальное содержимое не требует специальных разрешений.
- **iOS** &ndash; не требует специальных разрешений.

## <a name="layout"></a>Макет

В отличие от большинства других представлений Xamarin.Forms `WebView` требует `HeightRequest` и `WidthRequest` указываются, если он содержится в StackLayout или RelativeLayout. Если не указать эти свойства `WebView` не будут отображаться.

В следующих примерах демонстрируется макеты, которые приводят к работе, подготовки к просмотру `WebView`s:

Макет StackLayout с WidthRequest & HeightRequest:

```xaml
<StackLayout>
    <Label Text="test" />
    <WebView Source="http://www.xamarin.com/"
        HeightRequest="1000"
        WidthRequest="1000" />
</StackLayout>
```

RelativeLayout с WidthRequest & HeightRequest:

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

AbsoluteLayout *без* WidthRequest & HeightRequest:

```xaml
<AbsoluteLayout>
    <Label Text="test" AbsoluteLayout.LayoutBounds="0,0,100,100" />
    <WebView Source="http://www.xamarin.com/"
      AbsoluteLayout.LayoutBounds="0,150,500,500" />
</AbsoluteLayout>
```

Сетка *без* WidthRequest & HeightRequest. Сетка является одним из нескольких макетов, не требует указания запрошенного значения высоты и ширины.:

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

[`WebView`](xref:Xamarin.Forms.WebView) включает в себя возможность вызвать функцию JavaScript из C#и возвращает никакого результата для вызова C# кода. Это осуществляется с помощью [ `WebView.EvaluateJavaScriptAsync` ](xref:Xamarin.Forms.WebView.EvaluateJavaScriptAsync*) метод, который показан в следующем примере из [WebView](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-webview) пример:

```csharp
var numberEntry = new Entry { Text = "5" };
var resultLabel = new Label();
var webView = new WebView();
...

int number = int.Parse(numberEntry.Text);
string result = await webView.EvaluateJavaScriptAsync($"factorial({number})");
resultLabel.Text = $"Factorial of {number} is {result}.";
```

[ `WebView.EvaluateJavaScriptAsync` ](xref:Xamarin.Forms.WebView.EvaluateJavaScriptAsync*) Метод оценивает код JavaScript, указанное в качестве аргумента и возвращает любой результат в виде `string`. В этом примере `factorial` вызывается функция JavaScript, которая возвращает факториал `number` в результате. Этот код JavaScript, функция определена в HTML-код, локальный файл, который [ `WebView` ](xref:Xamarin.Forms.WebView) загружает и показан в следующем примере:

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

## <a name="uiwebview-deprecation-and-app-store-rejection-itms-90809"></a>Уивебвиев устаревания и отклонение магазина приложений (ИТМС-90809)

Начиная с 2020 апреля [компания Apple будет отклонять приложения](https://developer.apple.com/news/?id=12232019b) , которые по-прежнему используют устаревшие API `UIWebView`. Хотя Xamarin. Forms переключен на `WKWebView` по умолчанию, по-прежнему существует ссылка на старый пакет SDK в двоичных файлах Xamarin. Forms. Текущее поведение [компоновщика iOS](~/ios/deploy-test/linker.md) не удаляет это, и в результате нерекомендуемый API `UIWebView` по-прежнему будет отображаться в приложении при отправке в App Store.

Для устранения этой проблемы доступна предварительная версия компоновщика. Чтобы включить предварительную версию, необходимо указать дополнительный аргумент, `--optimize=experimental-xforms-product-type` компоновщику. 

Ниже перечислены необходимые условия для работы.

- Можно использовать **Xamarin. forms 4,5 или более поздней** версии &ndash; предварительных версий Xamarin. forms 4,5.
- **Xamarin. iOS 13.10.0.17 или более поздней** версии &ndash; проверить версию Xamarin. iOS [в Visual Studio](~/cross-platform/troubleshooting/questions/version-logs.md#version-information). Эта версия Xamarin. iOS входит в состав Visual Studio для Mac 8.4.1 и Visual Studio 16.4.3.
- **Удалите ссылки на `UIWebView`** &ndash; код не должен содержать ссылки на `UIWebView` или классы, использующие `UIWebView`.

### <a name="configure-the-linker-preview"></a>Настройка предварительного просмотра компоновщика

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Выполните следующие действия, чтобы компоновщик удалил ссылки `UIWebView`:

1. **Откройте свойства проекта ios** &ndash; щелкните правой кнопкой мыши проект iOS и выберите пункт **свойства**.
1. **Перейдите к разделу Build ios** &ndash; выберите раздел **Build для iOS** .
1. **Обновите дополнительные аргументы mtouch** &ndash; в **дополнительных аргументах mtouch** добавьте этот флаг `--optimize=experimental-xforms-product-type` (в дополнение к любому значению, которое уже возможно в нем). 
1. **Обновите все конфигурации сборки** &ndash; используйте списки **Конфигурация** и **платформа** в верхней части окна для обновления всех конфигураций сборки. Наиболее важной конфигурацией для обновления является конфигурация **Release/iPhone** , так как она обычно используется для создания сборок для отправки в App Store.

Можно увидеть окно с новым флагом на этом снимке экрана:

[![установки флага в разделе сборки iOS](webview-images/iosbuildblade-vs-sml.png)](webview-images/iosbuildblade-vs.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio для Mac](#tab/macos)

Выполните следующие действия, чтобы компоновщик удалил ссылки `UIWebView`

1. **Откройте параметры проекта ios** &ndash; щелкните правой кнопкой мыши проект iOS и выберите **Параметры**.
1. **Перейдите к разделу Build ios** &ndash; выберите раздел **Build для iOS** .
1. **Обновите дополнительные аргументы _mtouch_**  &ndash; ИИН **Дополнительные аргументы _mtouch_**  добавьте этот флаг `--optimize=experimental-xforms-product-type` (в дополнение к любому значению, которое уже возможно в нем).
1. **Обновите все конфигурации сборки** &ndash; используйте списки **Конфигурация** и **платформа** в верхней части окна для обновления всех конфигураций сборки. Наиболее важной конфигурацией для обновления является конфигурация **Release/iPhone** , так как она обычно используется для создания сборок для отправки в App Store.

Можно увидеть окно с новым флагом на этом снимке экрана:

[![установки флага в разделе сборки iOS](webview-images/iosbuildblade-xs-sml.png)](webview-images/iosbuildblade-xs.png#lightbox)

-----

Теперь при создании новой сборки (выпуска) и ее отправке в App Store не должно быть предупреждений об устаревшем API.

## <a name="related-links"></a>Связанные ссылки

- [Работа с WebView (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithwebview)
- [Веб-представления (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-webview)
