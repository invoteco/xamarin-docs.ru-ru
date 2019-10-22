---
title: Инициализация и Настройка карт Xamarin. Forms
description: В этой статье объясняется, как использовать класс Map (карта Xamarin. Forms) для использования собственных интерфейсов API Map на каждой платформе для предоставления пользователям привычных возможностей для работы с картами.
ms.prod: xamarin
ms.assetid: 59CD1344-8248-406C-9144-0C8A67141E5B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/07/2019
ms.openlocfilehash: d9b1b93b0667415281bb04e2c4264f03be19bd83
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/21/2019
ms.locfileid: "72697713"
---
# <a name="xamarinforms-map-initialization-and-configuration"></a>Инициализация и Настройка карт Xamarin. Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

_Xamarin. Forms использует собственные API-интерфейсы Map на каждой платформе._

Xamarin. Forms. Maps использует собственные API карты на каждой платформе. Это обеспечивает быстрый, знакомый интерфейс карт для пользователей, но означает, что некоторые действия по настройке необходимы для соблюдения требований к API для каждой платформы.
После настройки элемент управления `Map` работает так же, как любой другой элемент Xamarin. Forms в общем коде.

Элемент управления картой использовался в образце [мапссампле](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps) , как показано ниже.

 [![Карты в образце Мобилекрм](map-images/maps-zoom-sml.png "Пример Map Control")](map-images/maps-zoom.png#lightbox "Пример Map Control")

Функциональность Map можно расширить, создав [пользовательский модуль подготовки](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)отчетов.

<a name="Maps_Initialization" />

## <a name="map-initialization"></a>Инициализация карт

При добавлении сопоставлений в приложение Xamarin. Forms **Xamarin. Forms. Maps** — это отдельный пакет NuGet, который следует добавить в каждый проект в решении.
На Android это также зависит от Гуглеплайсервицес (другой NuGet), которая автоматически загружается при добавлении Xamarin. Forms. Maps.

После установки пакета NuGet в каждом проекте приложения требуется некоторый код инициализации *после* вызова метода `Xamarin.Forms.Forms.Init`. Для iOS используйте следующий код:

```csharp
Xamarin.FormsMaps.Init();
```

В Android необходимо передать те же параметры, что и `Forms.Init`:

```csharp
Xamarin.FormsMaps.Init(this, bundle);
```

Для универсальная платформа Windows (UWP) используйте следующий код:

```csharp
Xamarin.FormsMaps.Init("INSERT_AUTHENTICATION_TOKEN_HERE");
```

Добавьте этот вызов в следующие файлы для каждой платформы:

- файл **iOS** -AppDelegate.cs в методе `FinishedLaunching`.
- Файл **Android** -MainActivity.cs в методе `OnCreate`.
- Файл **UWP** -MainPage.XAML.cs в конструкторе `MainPage`.

После добавления пакета NuGet и метода инициализации, вызываемого внутри каждого приложения, `Xamarin.Forms.Maps` API-интерфейсы можно использовать в общем проекте библиотеки .NET Standard или в общем коде проекта.

<a name="Platform_Configuration" />

## <a name="platform-configuration"></a>Конфигурация платформы

На некоторых платформах необходимо выполнить дополнительные действия по настройке, прежде чем будет отображена схема.

### <a name="ios"></a>iOS

Чтобы получить доступ к службам расположения в iOS, необходимо задать следующие ключи в файле **info. plist**:

- iOS 11
  - [`NSLocationWhenInUseUsageDescription`](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26) — для использования служб определения местоположения при использовании приложения
  - [`NSLocationAlwaysAndWhenInUseUsageDescription`](https://developer.apple.com/documentation/corelocation/choosing_the_authorization_level_for_location_services/requesting_always_authorization?language=objc) — для использования служб определения местоположения в любое время
- iOS 10 и более ранние версии
  - [`NSLocationWhenInUseUsageDescription`](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26) — для использования служб определения местоположения при использовании приложения
  - [`NSLocationAlwaysUsageDescription`](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW18) — для использования служб определения местоположения в любое время    

Для поддержки iOS 11 и более ранних версий можно включить все три ключа: `NSLocationWhenInUseUsageDescription`, `NSLocationAlwaysAndWhenInUseUsageDescription` и `NSLocationAlwaysUsageDescription`.

Ниже приведено представление XML для этих разделов в **info. plist** . Необходимо обновить значения `string`, чтобы отразить, как ваше приложение использует сведения о расположении:

```xml
<key>NSLocationAlwaysUsageDescription</key>
<string>Can we use your location at all times?</string>
<key>NSLocationWhenInUseUsageDescription</key>
<string>Can we use your location when your app is being used?</string>
<key>NSLocationAlwaysAndWhenInUseUsageDescription</key>
<string>Can we use your location at all times?</string>
```

Записи **info. plist** также можно добавить в представление **исходного кода** при редактировании файла **info. plist** :

![Info. plist для iOS 8](map-images/ios8-map-permissions.png "Обязательные записи info. plist для iOS 8")

### <a name="android"></a>Android

Чтобы использовать [API-интерфейс Google Maps v2](https://developers.google.com/maps/documentation/android/) в Android, необходимо создать ключ API и добавить его в проект Android.
Следуйте инструкциям в документе Xamarin, [Получив ключ API Google Maps v2](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md).
После выполнения этих инструкций вставьте ключ API в файл **Properties/AndroidManifest. XML** (Просмотр источника и найдите или обновите следующий элемент):

```xml
<application ...>
    <meta-data android:name="com.google.android.maps.v2.API_KEY" android:value="YOUR_API_KEY" />
</application>
```

Без допустимого ключа API элемент управления Maps будет отображаться как серый прямоугольник в Android.

> [!NOTE]
> Обратите внимание, что для APK доступа к Google Maps необходимо включить отпечатки SHA-1 и имена пакетов для каждого хранилища ключей (Отладка и выпуск), которое используется для подписания APK. Например, если вы используете один компьютер для отладки и другой компьютер для создания APK выпуска, следует включить отпечаток сертификата SHA-1 из хранилища ключей отладки первого компьютера и отпечаток сертификата SHA-1 из хранилища ключей выпуска второй компьютер. Также не забудьте изменить ключевые учетные данные при изменении **имени пакета** приложения. См. [раздел Получение ключа API Google Maps](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md).

Кроме того, необходимо включить соответствующие разрешения, щелкнув правой кнопкой мыши проект Android и выбрав пункт **параметры > сборки > приложение Android** и поделенное на следующее:

- `AccessCoarseLocation`
- `AccessFineLocation`
- `AccessLocationExtraCommands`
- `AccessMockLocation`
- `AccessNetworkState`
- `AccessWifiState`
- `Internet`

Некоторые из них показаны на снимке экрана ниже:

![Необходимые разрешения для Android](map-images/android-map-permissions.png "Необходимые разрешения для Android")

Последние два являются обязательными, так как приложениям требуется сетевое подключение для загрузки данных карт. Дополнительные сведения см. в статье о [разрешениях](https://developer.android.com/reference/android/Manifest.permission.html) Android.

Кроме того, Android 9 удалил клиентскую библиотеку Apache HTTP из буткласспас, и поэтому она недоступна для приложений, предназначенных для API 28 или более поздней версии. Чтобы продолжить использование HTTP-клиента Apache в приложениях, предназначенных для API 28 или более поздней версии, необходимо добавить следующую строку в `application` узел файла **AndroidManifest. XML** .

```xml
<application ...>
    ...
    <uses-library android:name="org.apache.http.legacy" android:required="false" />    
</application>
```

### <a name="universal-windows-platform"></a>Универсальная платформа Windows

Чтобы использовать карты в универсальная платформа Windows необходимо создать маркер авторизации. Дополнительные сведения см. в разделе [запрос сопоставления ключа проверки подлинности](https://msdn.microsoft.com/library/windows/apps/mt219694.aspx) на сайте MSDN.

После этого маркер проверки подлинности должен быть указан в вызове метода `FormsMaps.Init("AUTHORIZATION_TOKEN")` для проверки подлинности приложения с помощью карт Bing.

<a name="Using_Maps" />

## <a name="map-configuration"></a>Конфигурация схемы

Пример того, как элемент управления картой можно использовать в коде, см. в разделе [MapPage.CS](https://github.com/xamarin/xamarin-forms-samples/blob/master/MobileCRM/MobileCRM.Shared/Pages/MapPage.cs) в примере мобилекрм. Простой класс `MapPage` может выглядеть так: Обратите внимание, что для размещения представления отображения создается новый `MapSpan`.

```csharp
public class MapPage : ContentPage {
    public MapPage() {
        var map = new Map(
            MapSpan.FromCenterAndRadius(
                    new Position(37,-122), Distance.FromMiles(0.3))) {
                IsShowingUser = true,
                HeightRequest = 100,
                WidthRequest = 960,
                VerticalOptions = LayoutOptions.FillAndExpand
            };
        var stack = new StackLayout { Spacing = 0 };
        stack.Children.Add(map);
        Content = stack;
    }
}
```

### <a name="map-type"></a>Тип схемы

Содержимое схемы также можно изменить, задав свойство `MapType`, чтобы отобразить обычную карту (по умолчанию), вспомогательное изображение или их сочетание.

```csharp
map.MapType = MapType.Street;
```

Допустимые значения `MapType`:

- `Hybrid`
- `Satellite`
- `Street` (по умолчанию)

### <a name="map-region-and-mapspan"></a>Регион и Мапспан на карте

Как показано в приведенном выше фрагменте кода, при передаче экземпляра `MapSpan` в Конструктор Map задается начальное представление (Центральная точка и масштаб) на карте при загрузке. Создать новый экземпляр `MapSpan` можно двумя способами:

- **Мапспан. фромцентерандрадиус ()** — статический метод для создания диапазона из `Position` и указания `Distance`.
- **New мапспан ()** — конструктор, который использует `Position` и градусы широты и долготы для вывода.

Затем можно использовать метод `MoveToRegion` класса Map для изменения расположения или масштаба на карте. Чтобы изменить уровень масштабирования на карте без изменения расположения, создайте новую `MapSpan` используя текущее расположение из свойства `VisibleRegion.Center` элемента управления картой. Для управления масштабом карт можно использовать `Slider` (Однако при изменении масштаба непосредственно в элементе управления картой невозможно обновить значение ползунка):

```csharp
Slider slider = new Slider (1, 18, 1);
slider.ValueChanged += (sender, e) =>
{
    var zoomLevel = e.NewValue; // between 1 and 18
    var latlongdegrees = 360 / (Math.Pow(2, zoomLevel));
    map.MoveToRegion(new MapSpan (map.VisibleRegion.Center, latlongdegrees, latlongdegrees));
};
```

[![Карты с масштабом](map-images/maps-zoom-sml.png "Масштаб Map Control")](map-images/maps-zoom.png#lightbox "Масштаб Map Control")

Кроме того, класс [`Map`](xref:Xamarin.Forms.Maps.Map) имеет свойство `MoveToLastRegionOnLayoutChange` типа `bool`, которое поддерживается связываемым свойством. По умолчанию это свойство имеет значение `true`, которое указывает, что отображаемая область отображения будет перемещена из текущей области в ее ранее заданную область при изменении макета, например при смене устройства. Если для этого свойства задано значение `false`, отображаемая область отображения останется в центре при изменении макета. В следующем примере показано задание этого свойства:

```csharp
map.MoveToLastRegionOnLayoutChange = false;
```

### <a name="map-clicks"></a>Кнопки карт

`Map` определяет событие `MapClicked`, которое возникает при нажатии на карту. Объект `MapClickedEventArgs`, сопровождающий событие `MapClicked`, имеет одно свойство с именем `Position` типа `Position`. При срабатывании события в качестве значения свойства `Position` устанавливается выбранное расположение на карте.

В следующем примере кода показан обработчик событий для события `MapClicked`:

```csharp
map.MapClicked += OnMapClicked;

void OnMapClicked(object sender, MapClickedEventArgs e)
{
    System.Diagnostics.Debug.WriteLine($"MapClick: {e.Position.Latitude}, {e.Position.Longitude}");
}
```

В этом примере обработчик событий `OnMapClicked` выводит широту и долготу, представляющую расположение касания.

<a name="Using_Xaml" />

### <a name="create-a-map-in-xaml"></a>Создание схемы в XAML

Карты также можно создавать на языке XAML, как показано в следующем примере:

```xaml
<?xml version="1.0" encoding="UTF-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps"
             x:Class="MapDemo.MapPage">
    <StackLayout VerticalOptions="StartAndExpand" Padding="30">
        <maps:Map x:Name="MyMap"
                  Clicked="OnMapClicked"
                  WidthRequest="320"
                  HeightRequest="200"                  
                  IsShowingUser="true"
                  MapType="Hybrid" />
    </StackLayout>
</ContentPage>
```

> [!NOTE]
> Для ссылки на элементы управления Xamarin. Forms. Maps требуется дополнительное определение пространства имен `xmlns`. В предыдущем примере ссылка на пространство имен `Xamarin.Forms.Maps` указывается с помощью ключевого слова `maps`.

@No__t_0 можно задать в коде, используя именованную ссылку для `Map`:

```csharp
MyMap.MoveToRegion(
    MapSpan.FromCenterAndRadius(
        new Position(37,-122), Distance.FromMiles(1)));
```

## <a name="related-links"></a>Связанные ссылки

- [Пример Maps](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)
- [Контакты Xamarin. Forms. Maps](~/xamarin-forms/user-interface/map/pins.md).
- [API карт](xref:Xamarin.Forms.Maps)
- [Преобразование пользовательского модуля подготовки отчетов](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)
