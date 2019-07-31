---
title: Xamarin.Forms карты
description: В этой статье описывается использование класса Xamarin.Forms Map использовать собственный API карты на каждой платформе для предоставления знакомой карт для пользователей.
ms.prod: xamarin
ms.assetid: 59CD1344-8248-406C-9144-0C8A67141E5B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/13/2019
ms.openlocfilehash: 51555382e38412b7fa2c5c821b252984758bb1ec
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/30/2019
ms.locfileid: "68656970"
---
# <a name="xamarinforms-map"></a>Xamarin.Forms карты

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

_Xamarin.Forms использует карту собственного API-интерфейсы на каждой платформе._

Xamarin.Forms.Maps использует карту собственного API-интерфейсы на каждой платформе. Это обеспечивает быстрый, знакомый интерфейс карт для пользователей, но означает, что некоторые действия по настройке необходимы для соблюдения требований к API для каждой платформы.
После настройки конфигурации `Map` работает так же, как и любой другой элемент Xamarin.Forms в общем коде контроль.

Элемент управления map был использован в [MapsSample](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps) пример, как показано ниже.

 [![Карты в образце MobileCRM](map-images/maps-zoom-sml.png "пример элемента управления Map")](map-images/maps-zoom.png#lightbox "пример элемента управления Map")

Функциональностью карты можно улучшить путем создания [сопоставить пользовательское средство отрисовки](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md).

<a name="Maps_Initialization" />

## <a name="map-initialization"></a>Инициализация карт

При добавлении карты в приложении Xamarin.Forms **Xamarin.Forms.Maps** — отдельный пакет NuGet, который следует добавить в каждый проект в решении.
В Android это также имеет зависимость от GooglePlayServices (другой NuGet), который загружается автоматически при добавлении Xamarin.Forms.Maps.

После установки пакета NuGet, требуется код инициализации в каждый проект приложения *после* `Xamarin.Forms.Forms.Init` вызова метода. Для устройств iOS используйте следующий код:

```csharp
Xamarin.FormsMaps.Init();
```

На устройстве Android необходимо передать те же параметры, что `Forms.Init`:

```csharp
Xamarin.FormsMaps.Init(this, bundle);
```

Для универсальной платформы Windows (UWP), используйте следующий код:

```csharp
Xamarin.FormsMaps.Init("INSERT_AUTHENTICATION_TOKEN_HERE");
```

Добавьте этот вызов в следующих файлах для каждой платформы:

-  **iOS** -файл AppDelegate.cs, в `FinishedLaunching` метод.
-  **Android** -MainActivity.cs файле `OnCreate` метод.
-  **UWP** -файл MainPage.xaml.cs, в `MainPage` конструктор.

После добавления пакета NuGet и метода инициализации, вызываемого в каждом приложении, `Xamarin.Forms.Maps` интерфейсы API можно использовать в общем проекте .NET Standard библиотеки или в общем коде проекта.

<a name="Platform_Configuration" />

## <a name="platform-configuration"></a>Конфигурация платформы

Дополнительные действия по настройке необходимы на некоторых платформах, прежде чем будет отображать карту.

### <a name="ios"></a>iOS

Чтобы открыть окно службы определения местоположения в iOS, необходимо задать следующие ключи в **Info.plist**:

- iOS 11
    - [`NSLocationWhenInUseUsageDescription`](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26) — для использования службы определения местоположения, когда приложение используется
    - [`NSLocationAlwaysAndWhenInUseUsageDescription`](https://developer.apple.com/documentation/corelocation/choosing_the_authorization_level_for_location_services/requesting_always_authorization?language=objc) — для использования службы определения местоположения все время
- iOS 10 и более ранних версий
    - [`NSLocationWhenInUseUsageDescription`](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26) — для использования службы определения местоположения, когда приложение используется
    - [`NSLocationAlwaysUsageDescription`](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW18) — для использования службы определения местоположения все время    

Для поддержки iOS 11 и более ранних версий, можно включить все три ключа: `NSLocationWhenInUseUsageDescription`, `NSLocationAlwaysAndWhenInUseUsageDescription`, и `NSLocationAlwaysUsageDescription`.

XML-представление для этих ключей в **Info.plist** показан ниже. Необходимо обновить `string` значения в том, как приложение использует сведения о расположении:

```xml
<key>NSLocationAlwaysUsageDescription</key>
<string>Can we use your location at all times?</string>
<key>NSLocationWhenInUseUsageDescription</key>
<string>Can we use your location when your app is being used?</string>
<key>NSLocationAlwaysAndWhenInUseUsageDescription</key>
<string>Can we use your location at all times?</string>
```

**Info.plist** записи также может быть добавлено в **источника** представления во время редактирования **Info.plist** файла:

![Info.plist для iOS 8](map-images/ios8-map-permissions.png "записи Info.plist требуется iOS 8")

### <a name="android"></a>Android

Чтобы использовать [v2 Google Maps API](https://developers.google.com/maps/documentation/android/) на устройстве Android необходимо создать ключ API и добавить его в проект Android.
Следуйте инструкциям в документе Xamarin на [Получение ключа Google Maps API v2](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md).
После выполнения этих инструкций, вставьте ключ API в **Properties/AndroidManifest.xml** файл (Просмотр исходного кода и поиска или обновить следующий элемент):

```xml
<application ...>
    <meta-data android:name="com.google.android.maps.v2.API_KEY" android:value="YOUR_API_KEY" />
</application>
```

Без допустимого ключа API элемент управления Maps будет отображаться как серый прямоугольник в Android.

> [!NOTE]
> Обратите внимание, что, чтобы пакет APK для доступа к Google карты, необходимо включить отпечатки пальцев SHA-1 и упаковать имена для каждого хранилища ключей (отладочную и окончательную), которые используются для входа пакет APK. Например если вы используете один компьютер для отладки и другой компьютер для создания выпуска APK, должно содержать отпечаток SHA-1 сертификата из хранилища ключей отладки первого компьютера и отпечаток SHA-1 сертификата из хранилища ключей выпуска из второй компьютер. Также не забудьте изменить учетные данные ключа, если приложения **имя пакета** изменения. См. в разделе [Получение ключа Google Maps API v2](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md).

Также необходимо включить соответствующие разрешения, щелкнув правой кнопкой мыши на проект Android и выбрав **параметры > Создать > приложение Android** и отсчет следующие:

* `AccessCoarseLocation`
* `AccessFineLocation`
* `AccessLocationExtraCommands`
* `AccessMockLocation`
* `AccessNetworkState`
* `AccessWifiState`
* `Internet`

На следующем снимке экрана показаны некоторые из них:

![Разрешения, необходимые для Android](map-images/android-map-permissions.png "необходимые разрешения для Android")

Последние два необходимы, так как приложения требуют подключения к сети для загрузки данных карты. Ознакомьтесь с Android [разрешения](https://developer.android.com/reference/android/Manifest.permission.html) для получения дополнительных сведений.

Кроме того, Android 9 удалил клиентскую библиотеку Apache HTTP из буткласспас, и поэтому она недоступна для приложений, предназначенных для API 28 или более поздней версии. Чтобы продолжить использование HTTP-клиента Apache в `application` приложениях, предназначенных для API 28 или более поздней версии, необходимо добавить следующую строку в узел файла **AndroidManifest. XML** .

```xml
<application ...>
    ...
    <uses-library android:name="org.apache.http.legacy" android:required="false" />    
</application>
```

### <a name="universal-windows-platform"></a>Универсальная платформа Windows

Для работы с картами на универсальной платформе Windows необходимо создать маркер авторизации. Дополнительные сведения см. в разделе [запроса ключа проверки подлинности карты](https://msdn.microsoft.com/library/windows/apps/mt219694.aspx) на сайте MSDN.

Затем маркер проверки подлинности следует указывать в `FormsMaps.Init("AUTHORIZATION_TOKEN")` вызов метода, для проверки подлинности приложения с помощью Bing Maps.

<a name="Using_Maps" />

## <a name="map-configuration"></a>Конфигурация схемы

См. в разделе [MapPage.cs](https://github.com/xamarin/xamarin-forms-samples/blob/master/MobileCRM/MobileCRM.Shared/Pages/MapPage.cs) в образце MobileCRM пример использования элемента управления картой в коде. Простой `MapPage` класс может выглядеть как - уведомления, новый `MapSpan` создается для размещения представления карты:

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

Также можно изменить, задав содержимого карты `MapType` свойства, чтобы показать карту регулярных улицы (по умолчанию), вспомогательных изображений или их сочетание.

```csharp
map.MapType == MapType.Street;
```

Допустимые `MapType` значения:

-  Гибридные
-  Вспомогательные
-  Улица (по умолчанию)

### <a name="map-region-and-mapspan"></a>Регион и Мапспан на карте

Как показано в приведенном выше фрагменте кода, указав `MapSpan` экземпляр конструктора карты задает первоначального представления (центральную точку и масштаб) сопоставления при его загрузке. `MoveToRegion` Затем метод класса map можно использовать для изменения положения или масштабирования уровня карты. Существует два способа для создания нового `MapSpan` экземпляр:

-  **MapSpan.FromCenterAndRadius()** -статический метод, чтобы создать диапазон из `Position` и указав `Distance` .
-  **New () MapSpan** -конструктор, использующий `Position` и градусах широты и долготы для отображения.


Чтобы изменить уровень масштаба карты, не изменяя расположение, создайте новый `MapSpan` с использованием текущего расположения из `VisibleRegion.Center` свойство элемента управления картой. Объект `Slider` может использоваться для управления масштабом карты следующим образом (Однако масштабирование непосредственно в элементе управления картой не может обновить сейчас значение ползунка):

```csharp
var slider = new Slider (1, 18, 1);
slider.ValueChanged += (sender, e) => {
    var zoomLevel = e.NewValue; // between 1 and 18
    var latlongdegrees = 360 / (Math.Pow(2, zoomLevel));
    map.MoveToRegion(new MapSpan (map.VisibleRegion.Center, latlongdegrees, latlongdegrees));
};
```

 [![Карты с zoom](map-images/maps-zoom-sml.png "масштаб элемента управления Map")](map-images/maps-zoom.png#lightbox "масштаб элемента управления Map")

### <a name="map-pins"></a>Контакты карт

Расположений можно пометить на карте с `Pin` объектов.

```csharp
var position = new Position(37,-122); // Latitude, Longitude
var pin = new Pin {
            Type = PinType.Place,
            Position = position,
            Label = "custom pin",
            Address = "custom detail info"
        };
map.Pins.Add(pin);
```

`PinType`можно задать одно из следующих значений, которое может повлиять на способ подготовки к просмотру ПИН-кода (в зависимости от платформы):

-  Универсальный
-  Место
-  SavedPin
-  SearchResult

### <a name="map-clicks"></a>Кнопки карт

`Map``MapClicked` определяет событие, которое возникает при нажатии на карту. Объект, `Position`сопровождающий событие, имеет одно свойство с именем типа `Position`. `MapClicked` `MapClickedEventArgs` При срабатывании события в качестве значения `Position` свойства задается расположение, которое было касанием.

В следующем примере кода показан обработчик событий для `MapClicked` события:

```csharp
map.MapClicked += OnMapClicked;

void OnMapClicked(object sender, MapClickedEventArgs e)
{
    System.Diagnostics.Debug.WriteLine($"MapClick: {e.Position.Latitude}, {e.Position.Longitude}");
}
```

В этом примере `OnMapClicked` обработчик событий выводит широту и долготу, представляющую расположение касания.

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
> Для ссылки `xmlns` на элементы управления Xamarin. Forms. Maps требуется дополнительное определение пространства имен.

И могут быть заданы в коде с помощью именованной ссылки для `Map`: `MapRegion` `Pins`

```csharp
MyMap.MoveToRegion(
    MapSpan.FromCenterAndRadius(
        new Position(37,-122), Distance.FromMiles(1)));
```

## <a name="populate-a-map-with-data-using-data-binding"></a>Заполнение схемы данными с помощью привязки данных

[`Map`](xref:Xamarin.Forms.Maps.Map) Класс также предоставляет следующие свойства:

- `ItemsSource`— Указывает коллекцию `IEnumerable` элементов для отображения.
- `ItemTemplate`— Задает объект [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) , применяемый к каждому элементу в коллекции отображаемых элементов.
- `ItemTemplateSelector`— Указывает [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) , который будет использоваться для [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) выбора элемента во время выполнения.

> [!NOTE]
> Свойство имеет приоритет, если `ItemTemplate` заданы свойства `ItemTemplateSelector`и. `ItemTemplate`

Можно заполнить данными с помощью привязки данных, чтобы привязать его `ItemsSource` свойство к `IEnumerable` коллекции: [`Map`](xref:Xamarin.Forms.Maps.Map)

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps"
             x:Class="WorkingWithMaps.PinItemsSourcePage">
    <Grid>
        ...
        <maps:Map x:Name="map"
                  ItemsSource="{Binding Locations}">
            <maps:Map.ItemTemplate>
                <DataTemplate>
                    <maps:Pin Position="{Binding Position}"
                              Address="{Binding Address}"
                              Label="{Binding Description}" />
                </DataTemplate>
            </maps:Map.ItemTemplate>
        </maps:Map>
        ...
    </Grid>
</ContentPage>
```

Данные свойства привязываются `Locations` к свойству подключенной `ObservableCollection` модели представления, которая возвращает коллекцию `Location` объектов, которая является пользовательским типом. `ItemsSource` Каждый `Location` объект определяет `Address` и `Description` свойства типа `string` и`Position` свойства типа [.`Position`](xref:Xamarin.Forms.Maps.Position)

Внешний `IEnumerable` вид каждого элемента в коллекции определяется путем `ItemTemplate` присвоения свойству [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) значения, содержащего [`Pin`](xref:Xamarin.Forms.Maps.Pin) объект, который привязывает данные к соответствующим свойствам.

На следующих снимках экрана [`Map`](xref:Xamarin.Forms.Maps.Map) показано, [`Pin`](xref:Xamarin.Forms.Maps.Pin) как отобразить коллекцию с помощью привязки данных:

[ ![Снимок экрана с привязками к данным на карте iOS и Android](map-images/pins-itemssource.png "с закрепленными данными") ] (map-images/pins-itemssource-large.png#lightbox "Сопоставьте с закрепленными данными")

### <a name="choose-item-appearance-at-runtime"></a>Выбор внешнего вида элемента во время выполнения

Внешний вид каждого элемента в `IEnumerable` коллекции можно выбрать во время выполнения на основе значения элемента, `ItemTemplateSelector` задав для [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector)свойства значение.

```xaml
<ContentPage ...
             xmlns:local="clr-namespace:WorkingWithMaps"
             xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps">
    <ContentPage.Resources>
        <local:MapItemTemplateSelector x:Key="MapItemTemplateSelector">
            <local:MapItemTemplateSelector.DefaultTemplate>
                <DataTemplate>
                    <maps:Pin Position="{Binding Position}"
                              Address="{Binding Address}"
                              Label="{Binding Description}" />
                </DataTemplate>
            </local:MapItemTemplateSelector.DefaultTemplate>
            <local:MapItemTemplateSelector.XamarinTemplate>
                <DataTemplate>
                    <maps:Pin Position="{Binding Position}"
                              Address="{Binding Address}"
                              Label="Xamarin!" />
                </DataTemplate>
            </local:MapItemTemplateSelector.XamarinTemplate>    
        </local:MapItemTemplateSelector>
    </ContentPage.Resources>

    <Grid>
        ...
        <maps:Map x:Name="map"
                  ItemsSource="{Binding Locations}"
                  ItemTemplateSelector="{StaticResource MapItemTemplateSelector}" />
        ...
    </Grid>
</ContentPage>
```

В следующем примере показан `MapItemTemplateSelector` класс:

```csharp
public class MapItemTemplateSelector : DataTemplateSelector
{
    public DataTemplate DefaultTemplate { get; set; }
    public DataTemplate XamarinTemplate { get; set; }

    protected override DataTemplate OnSelectTemplate(object item, BindableObject container)
    {
        return ((Location)item).Address.Contains("San Francisco") ? XamarinTemplate : DefaultTemplate;
    }
}
```

Класс определяет `DefaultTemplate` свойства и`XamarinTemplate` , [длякоторыхустановленыразныешаблоныданных.`DataTemplate`](xref:Xamarin.Forms.DataTemplate) `MapItemTemplateSelector` Метод возвращает, который отображает Xamarin в `Pin` качестве метки при касании, если элемент имеет адрес, содержащий "Сан-Франциско". `XamarinTemplate` `OnSelectTemplate` Если у элемента нет адреса, содержащего "Сан Франциско", `OnSelectTemplate` метод `DefaultTemplate`возвращает.

Дополнительные сведения о селекторах шаблонов данных см. [в разделе Создание DataTemplateSelector Xamarin. Forms](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md).

## <a name="related-links"></a>Связанные ссылки

- [MapsSample](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)
- [Пользовательское средство отрисовки карты](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)
- [Примеры Xamarin.Forms](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Forms)
- [Создание DataTemplateSelector Xamarin. Forms](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
- [API карт](xref:Xamarin.Forms.Maps)
