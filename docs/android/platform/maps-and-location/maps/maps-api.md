---
title: Использование API-интерфейса Google Maps в приложении
description: Как реализовать функции API-интерфейса Google Maps v2 в приложении Xamarin. Android.
ms.prod: xamarin
ms.assetid: C0589878-2D04-180E-A5B9-BB41D5AF6E02
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 09/07/2018
ms.openlocfilehash: adcfb1457742d343f87a602885566107cf327e2d
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73027155"
---
# <a name="using-the-google-maps-api-in-your-application"></a>Использование API-интерфейса Google Maps в приложении

Использование приложения Maps отлично, но иногда требуется включить карты непосредственно в приложение. Помимо встроенного приложения Maps, Google также предлагает [собственный интерфейс API сопоставления для Android](https://developers.google.com/maps/documentation/android-sdk/intro).
API Maps подходит для случаев, когда требуется обеспечить больший контроль над процессом сопоставления. К API Maps относятся следующие вещи.

- Программное изменение точки зрения на карте.
- Добавление и настройка маркеров.
- Аннотирование схемы с наложенными слоями.

В отличие от устаревшего интерфейса Google Maps Android API версии v1, Google Maps Android API v2 является частью [сервисы Google Play](https://developers.google.com/android/guides/overview).
Приложение Xamarin. Android должно соответствовать некоторым обязательным требованиям, прежде чем можно будет использовать API-интерфейс Google Maps Android.

## <a name="google-maps-api-prerequisites"></a>Необходимые условия для API-интерфейса Google Maps

Прежде чем можно будет использовать API Maps, необходимо выполнить несколько шагов, в том числе:

- [Получение ключа API Maps](#obtain-maps-key)
- [Установка пакета SDK для Сервисы Google Play](#install-gps-sdk)
- [Установка пакета Xamarin. Гуглеплайсервицес. Maps из NuGet](#install-gpsmaps-nuget)
- [Укажите необходимые разрешения](#declare-permissions)
- [При необходимости создайте эмулятор с помощью API Google.](#create-emulator-with-google-api)

### <a name="a-nameobtain-maps-key-obtain-a-google-maps-api-key"></a><a name="obtain-maps-key" />получить ключ API Google Maps

Первым шагом является получение ключа API Google Maps (Обратите внимание, что вы не можете повторно использовать ключ API из устаревшей версии API Google Maps v1). Сведения о том, как получить и использовать ключ API с помощью Xamarin. Android, см. [в статье Получение ключа API Google Maps](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md).

### <a name="a-nameinstall-gps-sdk--install-the-google-play-services-sdk"></a><a name="install-gps-sdk" /> установить пакет SDK для Сервисы Google Play

Сервисы Google Play — это технология Google, которая позволяет приложениям Android использовать преимущества различных функций Google, таких как Google +, In-App Billing и Maps. Эти функции доступны на устройствах Android в качестве фоновых служб, которые содержатся в [сервисы Google Play apk](https://play.google.com/store/apps/details?id=com.google.android.gms&hl=en).

Приложения Android взаимодействуют с Сервисы Google Play через клиентскую библиотеку Сервисы Google Play. Эта библиотека содержит интерфейсы и классы для отдельных служб, таких как Maps. На следующей схеме показана связь между приложением Android и Сервисы Google Play:

![Схема, иллюстрирующая Google Play Маркет обновления Сервисы Google Play APK](maps-api-images/play-services-diagram.png)

API карт Android поставляется в составе Сервисы Google Play.
Прежде чем приложение Xamarin. Android сможет использовать API Maps, необходимо установить пакет SDK для Сервисы Google Play с помощью [диспетчера пакет SDK для Android](~/android/get-started/installation/android-sdk.md). На следующем снимке экрана показано, где можно найти клиент Google Play Services пакет SDK для Android Manager:

![Сервисы Google Play отображается в разделе "дополнительные" в диспетчере пакет SDK для Android](maps-api-images/image01.png)

> [!NOTE]
> Google Play Services APK — это лицензированный продукт, который может отсутствовать на всех устройствах. Если оно не установлено, Google Maps не будет работать на устройстве.

### <a name="a-nameinstall-gpsmaps-nuget--install-the-xamaringoogleplayservicesmaps-package-from-nuget"></a><a name="install-gpsmaps-nuget" /> установить пакет Xamarin. Гуглеплайсервицес. Maps из NuGet

[Пакет Xamarin. гуглеплайсервицес. Maps](https://www.nuget.org/packages/Xamarin.GooglePlayServices.Maps) содержит привязки Xamarin. Android для API карт сервисы Google Play.
Чтобы добавить пакет Сервисы Google Play Map, щелкните правой кнопкой мыши папку **ссылки** проекта в Обозреватель решений и выберите пункт **Управление пакетами NuGet...** :

![обозреватель решений отображение пункта меню "Управление пакетами NuGet" в разделе "ссылки"](maps-api-images/image02.png)

Откроется **Диспетчер пакетов NuGet**. Нажмите кнопку **Обзор** и введите в поле поиска элемент **Xamarin сервисы Google Play Maps** . Выберите **Xamarin. гуглеплайсервицес. Maps** и нажмите кнопку **установить**. (Если этот пакет был установлен ранее, нажмите кнопку **Обновить**.):

[![диспетчер пакетов NuGet с выбранным пакетом Xamarin. Гуглеплайсервицес. Maps](maps-api-images/image03-sml.png)](maps-api-images/image03.png#lightbox)

Обратите внимание, что также установлены следующие пакеты зависимостей:

- **Xamarin.GooglePlayServices.Base**
- **Xamarin.GooglePlayServices.Basement**
- **Xamarin.GooglePlayServices.Tasks**

### <a name="a-namedeclare-permissions--specify-the-required-permissions"></a><a name="declare-permissions" /> указать необходимые разрешения

Приложения должны указать требования к оборудованию и разрешениям для использования API-интерфейса Google Maps.  Некоторые разрешения автоматически предоставляются пакетом SDK для Сервисы Google Play, и разработчику не нужно явно добавлять их в **андроидманфест. XML**:

- **Доступ к сетевому состоянию** &ndash; API Maps должен иметь возможность проверить, можно ли загрузить плитки карты.

- **Доступ к интернету** &ndash; доступ к Интернету необходим для загрузки плиток карт и взаимодействия с серверами Google Play для доступа к API.

Следующие разрешения и компоненты должны быть указаны в **файле AndroidManifest. XML** для API Google Maps Android:

- **OpenGL ES v2** &ndash; приложение должно объявлять требование для OpenGL ES v2.

- **Ключ API Google Maps** &ndash; ключ API используется для подтверждения того, что приложение зарегистрировано и разрешено использовать сервисы Google Play. Дополнительные сведения об этом ключе см. [в разделе Получение ключа API Google Maps](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md) .

- **Запросите устаревшие клиентские &ndash; Apache HTTP** , предназначенные для Android 9,0 (уровень API 28) или выше, должны указывать, что устаревший клиент HTTP Apache является необязательной библиотекой для использования.

- **Доступ к службам Google Web-based** &ndash; приложению требуются разрешения на доступ к веб-службам Google, поддерживающим API карт Android.

- **Разрешения для уведомлений Сервисы Google Play** &ndash; приложении должно быть предоставлено разрешение на получение удаленных уведомлений от сервисы Google Play.

- **Доступ к поставщикам расположения** &ndash; они являются необязательными разрешениями.
   Они позволяют классу `GoogleMap` отображать расположение устройства на карте.

Кроме того, Android 9 удалил клиентскую библиотеку Apache HTTP из буткласспас, и поэтому она недоступна для приложений, предназначенных для API 28 или более поздней версии. Чтобы продолжить использование HTTP-клиента Apache в приложениях, предназначенных для API 28 или более поздней версии, необходимо добавить следующую строку в `application` узел файла **AndroidManifest. XML** .

```xml
<application ...>
   ...
   <uses-library android:name="org.apache.http.legacy" android:required="false" />    
</application>
```

> [!NOTE]
> Самые старые версии пакета SDK для Google Play требовали, чтобы приложение запрашивало разрешение `WRITE_EXTERNAL_STORAGE`. Это требование больше не требуется для последних привязок Xamarin для Сервисы Google Play.

В следующем фрагменте кода приведен пример параметров, которые необходимо добавить в **AndroidManifest. XML**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android" android:versionName="4.5" package="com.xamarin.docs.android.mapsandlocationdemo2" android:versionCode="6">
    <uses-sdk android:minSdkVersion="23" android:targetSdkVersion="28" />

    <!-- Google Maps for Android v2 requires OpenGL ES v2 -->
    <uses-feature android:glEsVersion="0x00020000" android:required="true" />

    <!-- Necessary for apps that target Android 9.0 or higher -->
    <uses-library android:name="org.apache.http.legacy" android:required="false" />

    <!-- Permission to receive remote notifications from Google Play Services -->
    <!-- Notice here that we have the package name of our application as a prefix on the permissions. -->
    <uses-permission android:name="<PACKAGE NAME>.permission.MAPS_RECEIVE" />
    <permission android:name="<PACKAGE NAME>.permission.MAPS_RECEIVE" android:protectionLevel="signature" />

    <!-- These are optional, but recommended. They will allow Maps to use the My Location provider. -->
    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />

    <application android:label="@string/app_name">
        <!-- Put your Google Maps V2 API Key here. -->
        <meta-data android:name="com.google.android.maps.v2.API_KEY" android:value="YOUR_API_KEY" />
        <meta-data android:name="com.google.android.gms.version" android:value="@integer/google_play_services_version" />
        <!-- Necessary for apps that target Android 9.0 or higher -->
        <uses-library android:name="org.apache.http.legacy" android:required="false" />
    </application>
</manifest>
```

Помимо запроса разрешений **AndroidManifest. XML**, приложение также должно выполнять проверку разрешений среды выполнения для `ACCESS_COARSE_LOCATION` и разрешений `ACCESS_FINE_LOCATION`. Дополнительные сведения о выполнении проверок разрешений во время выполнения см. в разделе "рекомендации по [разрешениям Xamarin. Android](~/android/app-fundamentals/permissions.md) ".

### <a name="a-namecreate-emulator-with-google-api-create-an-emulator-with-google-apis"></a><a name="create-emulator-with-google-api" />создание эмулятора с помощью API Google

В случае, если физическое устройство Android с Google Playными службами не установлено, можно создать образ эмулятора для разработки. Дополнительные сведения см. в [Device Manager](~/android/get-started/installation/android-emulator/device-manager.md).

## <a name="the-googlemap-class"></a>Класс Гуглемап

После выполнения необходимых условий пора приступить к разработке приложения и использовать API карт Android. Класс [гуглемап](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap) — это основной интерфейс API, который приложение Xamarin. Android будет использовать для показа и взаимодействия с Google Maps для Android. Этот класс имеет следующие обязанности:

- Взаимодействие со службами Google Play для авторизации приложения с помощью веб-службы Google.

- Загрузка, кэширование и отображение плиток карт.

- Отображение элементов управления пользовательского интерфейса, таких как Панорама и масштаб, для пользователя.

- Рисование маркеров и геометрических фигур на картах.

`GoogleMap` добавляется в действие одним из двух способов:

- **Мапфрагмент** — [мапфрагмент](https://developers.google.com/android/reference/com/google/android/gms/maps/MapFragment) — это специализированный фрагмент, который выступает в качестве узла для объекта `GoogleMap`. Для `MapFragment` требуется API Android уровня 12 или выше.
   Более старые версии Android могут использовать [суппортмапфрагмент](https://developers.google.com/android/reference/com/google/android/gms/maps/SupportMapFragment).  В этом руководством основное внимание уделяется использованию класса `MapFragment`.

- **MapView** — [MapView](https://developers.google.com/android/reference/com/google/android/gms/maps/MapView) является специализированным подклассом представления, который может служить узлом для `GoogleMap` объекта. Пользователи этого класса должны пересылать все методы жизненного цикла действия в класс `MapView`.

Каждый из этих контейнеров предоставляет свойство `Map`, которое возвращает экземпляр `GoogleMap`. Необходимо предоставить предпочтение классу [мапфрагмент](https://developers.google.com/android/reference/com/google/android/gms/maps/MapFragment) , так как это более простой API, который сокращает объем стандартного кода, который разработчик должен реализовывать вручную.

### <a name="adding-a-mapfragment-to-an-activity"></a>Добавление Мапфрагмент к действию

На следующем снимке экрана показан пример простого `MapFragment`:

[![снимок экрана устройства, отображающего фрагмент Google Map](maps-api-images/image05-sml.png)](maps-api-images/image05.png#lightbox)

Как и в случае с другими классами фрагментов, существует два способа добавления `MapFragment` к действию:

- **Декларативно** — `MapFragment` можно добавить с помощью XML-файла макета для действия. В следующем фрагменте кода XML показан пример использования элемента `fragment`:

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <fragment xmlns:android="http://schemas.android.com/apk/res/android"
              android:id="@+id/map"
              android:layout_width="match_parent"
              android:layout_height="match_parent"
              class="com.google.android.gms.maps.MapFragment" />
    ```

- **Программно** — `MapFragment` можно создать программно с помощью метода [`MapFragment.NewInstance`](https://developers.google.com/android/reference/com/google/android/gms/maps/MapFragment.html#newInstance()) , а затем добавить в действие. В этом фрагменте кода показан самый простой способ создания объекта `MapFragment` и добавления в действие:

    ```csharp
        var mapFrag = MapFragment.NewInstance();
        activity.FragmentManager.BeginTransaction()
                                .Add(Resource.Id.map_container, mapFrag, "map_fragment")
                                .Commit();

    ```

    Можно настроить объект `MapFragment`, передав [`GoogleMapOptions`](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMapOptions) объект в `NewInstance`. Это описано в разделе [Свойства гуглемап](#googlemap_object) , которые появятся далее в этом пошаговом окне.

Метод `MapFragment.GetMapAsync` используется для инициализации [`GoogleMap`](#googlemap_object) , размещенного в фрагменте, и получения ссылки на объект Map, размещенный в `MapFragment`. Этот метод принимает объект, реализующий интерфейс `IOnMapReadyCallback`.

Этот интерфейс содержит один метод, `IMapReadyCallback.OnMapReady(MapFragment map)`, который будет вызываться, когда приложение может взаимодействовать с объектом `GoogleMap`. В следующем фрагменте кода показано, как действие Android может инициализировать `MapFragment` и реализовать интерфейс `IOnMapReadyCallback`:

```csharp
public class MapWithMarkersActivity : AppCompatActivity, IOnMapReadyCallback
{
    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);
        SetContentView(Resource.Layout.MapLayout);

        var mapFragment = (MapFragment) FragmentManager.FindFragmentById(Resource.Id.map);
        mapFragment.GetMapAsync(this);

        // remainder of code omitted
    }

    public void OnMapReady(GoogleMap map)
    {
        // Do something with the map, i.e. add markers, move to a specific location, etc.
    }
}
```

### <a name="map-types"></a>Типы карт

В API-интерфейсе Google Maps доступно пять различных типов карт:

- **Обычная** — это тип отображения по умолчанию. Здесь показаны дороги и важные функции, а также некоторые искусственные точки интересов (например, здания и мосты).

- **Вспомогательная** . Эта схема показывает вспомогательную фотографию.

- **Гибридная** . Эта карта показывает вспомогательные фотографии и карты дорог.

- **Ландшафта** — в этом основном показаны функции топографикал с некоторыми дорогыми компонентами.

- **Нет** — эта схема не загружает плитки, она отображается как пустая сетка.

На рисунке ниже показаны три различных типа карт: слева направо (обычная, гибридная, ландшафта).

[![три экрана с примерами карт: обычные, гибридные и ландшафта](maps-api-images/map-types-sml.png)](maps-api-images/map-types.png#lightbox)

Свойство `GoogleMap.MapType` используется для задания или изменения отображаемого типа отображения. В следующем фрагменте кода показано, как отобразить вспомогательную карту.

```csharp
public void OnMapReady(GoogleMap map)
{
    map.MapType = GoogleMap.MapTypeHybrid;
}
```

### <a name="a-namegooglemap_object-googlemap-properties"></a>свойства <a name="googlemap_object" />Гуглемап

`GoogleMap` определяет несколько свойств, которые могут управлять функциональностью и внешним видом схемы. Одним из способов настройки начального состояния `GoogleMap` является передача объекта [гуглемапоптионс](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMapOptions) при создании `MapFragment`. Следующий фрагмент кода является одним из примеров использования объекта `GoogleMapOptions` при создании `MapFragment`:

```csharp
GoogleMapOptions mapOptions = new GoogleMapOptions()
    .InvokeMapType(GoogleMap.MapTypeSatellite)
    .InvokeZoomControlsEnabled(false)
    .InvokeCompassEnabled(true);

FragmentTransaction fragTx = FragmentManager.BeginTransaction();
mapFragment = MapFragment.NewInstance(mapOptions);
fragTx.Add(Resource.Id.map, mapFragment, "map");
fragTx.Commit();
```

Другим способом настройки `GoogleMap` является Управление свойствами [уисеттингс](https://developers.google.com/android/reference/com/google/android/gms/maps/UiSettings) объекта Map. В следующем примере кода показано, как настроить `GoogleMap` для отображения элементов управления масштабом и компаса:

```csharp
public void OnMapReady(GoogleMap map)
{
    map.UiSettings.ZoomControlsEnabled = true;
    map.UiSettings.CompassEnabled = true;
}
```

## <a name="interacting-with-the-googlemap"></a>Взаимодействие с Гуглемап

API карт Android предоставляет интерфейсы API, позволяющие действию изменять точку зрения, добавлять маркеры, размещать пользовательские наложения или рисовать геометрические фигуры. В этом разделе рассказывается, как выполнить некоторые из этих задач в Xamarin. Android.

### <a name="changing-the-viewpoint"></a>Изменение точки зрения

Карты моделируются в виде плоской плоскости на экране на основе Меркатора проекции. Представление "схема" — это то, что *Камера* смотрит на этой плоскости. Положение камеры можно контролировать, изменяя расположение, масштаб, наклон и коэффициент прохождения. Класс [камераупдате](https://developers.google.com/android/reference/com/google/android/gms/maps/CameraUpdate) используется для перемещения расположения камеры. объекты `CameraUpdate` не создаются напрямую, а API Maps предоставляет класс [камераупдатефактори](https://developers.google.com/android/reference/com/google/android/gms/maps/CameraUpdateFactory) .

После создания объекта `CameraUpdate` он передается в качестве параметра методам [GoogleMap.MoveCamera](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap#moveCamera(com.google.android.gms.maps.CameraUpdate)) или [GoogleMap.AnimateCamera](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap#animateCamera(com.google.android.gms.maps.CameraUpdate)). Метод `MoveCamera` обновляет карту мгновенно, в то время как метод `AnimateCamera` предоставляет плавный анимированный переход.

Этот фрагмент кода является простым примером того, как использовать `CameraUpdateFactory` для создания `CameraUpdate`, увеличивающего масштаб масштабирования на один уровень масштаба:

```csharp
MapFragment mapFrag = (MapFragment) FragmentManager.FindFragmentById(Resource.Id.my_mapfragment_container);
mapFrag.GetMapAsync(this);
...

public void OnMapReady(GoogleMap map)
{   
    map.MoveCamera(CameraUpdateFactory.ZoomIn());
}
```

API Maps предоставляет [камерапоситион](https://developer.android.com/reference/com/google/android/gms/maps/model/CameraPosition.html) , который будет объединять все возможные значения для расположения камеры. Экземпляр этого класса может быть предоставлен методу [камераупдатефактори. невкамерапоситион](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/CameraUpdateFactory#newCameraPosition%28com.google.android.gms.maps.model.CameraPosition%29) , который возвращает объект `CameraUpdate`. API Maps также включает класс [камерапоситион. Builder](https://developer.android.com/reference/com/google/android/gms/maps/model/CameraPosition.Builder.html) , предоставляющий API Fluent для создания объектов `CameraPosition`.
В следующем фрагменте кода показан пример создания `CameraUpdate` из `CameraPosition` и его использование для изменения расположения камеры на `GoogleMap`:

```csharp
public void OnMapReady(GoogleMap map)
{
    LatLng location = new LatLng(50.897778, 3.013333);

    CameraPosition.Builder builder = CameraPosition.InvokeBuilder();
    builder.Target(location);
    builder.Zoom(18);
    builder.Bearing(155);
    builder.Tilt(65);

    CameraPosition cameraPosition = builder.Build();

    CameraUpdate cameraUpdate = CameraUpdateFactory.NewCameraPosition(cameraPosition);

    map.MoveCamera(cameraUpdate);
}
```

В предыдущем фрагменте кода определенное расположение на карте представлено классом [латлнг](https://developers.google.com/android/reference/com/google/android/gms/maps/model/LatLng) . Уровень масштаба равен 18, что является произвольной мерой масштабирования, используемой в Google Maps. Это измерение компаса по часовой стрелке от Севера. Свойство «наклон» определяет угол обзора и задает угол в 25 градусов от вертикального. На следующем снимке экрана показан `GoogleMap` после выполнения предыдущего кода:

[![пример на карте Google, показывающей указанное расположение с наклонным углом обзора](maps-api-images/image06-sml.png)](maps-api-images/image06.png#lightbox)

### <a name="drawing-on-the-map"></a>Рисование на карте

API карт Android предоставляет API для рисования следующих элементов на карте:

- **Маркеры** — это специальные значки, используемые для определения одного расположения на карте.

- **Наложенные** — это изображение, которое можно использовать для задания коллекции расположений или области на карте.

- **Линии, многоугольники и круги** — это интерфейсы API, позволяющие действиям добавлять фигуры в карту.

#### <a name="markers"></a>Markers

API Maps предоставляет класс [маркера](https://developers.google.com/android/reference/com/google/android/gms/maps/model/Marker) , который инкапсулирует все данные об одном расположении на карте. По умолчанию класс маркера использует стандартный значок, предоставляемый Google Maps. Можно настроить внешний вид маркера и реагировать на нажатия пользователем.

##### <a name="adding-a-marker"></a>Добавление маркера

Чтобы добавить маркер к карте, необходимо создать новый объект [маркероптионс](https://developers.google.com/android/reference/com/google/android/gms/maps/model/MarkerOptions) , а затем вызвать метод [аддмаркер](https://developer.android.com/reference/com/google/android/gms/maps/GoogleMap.html#addMarker%28com.google.android.gms.maps.model.MarkerOptions%29) для экземпляра `GoogleMap`. Этот метод вернет объект [маркера](https://developers.google.com/android/reference/com/google/android/gms/maps/model/Marker) .

```csharp
public void OnMapReady(GoogleMap map)
{
    MarkerOptions markerOpt1 = new MarkerOptions();
    markerOpt1.SetPosition(new LatLng(50.379444, 2.773611));
    markerOpt1.SetTitle("Vimy Ridge");

    map.AddMarker(markerOpt1);
}
```

Заголовок маркера будет отображаться в *окне сведений* , когда пользователь нажмет маркер. На следующем снимке экрана показано, как выглядит этот маркер:

[![примере Google Map с маркером и окном сведений для Виминого](maps-api-images/image07-sml.png)](maps-api-images/image07.png#lightbox)

##### <a name="customizing-a-marker"></a>Настройка маркера

Можно настроить значок, используемый маркером, вызвав метод `MarkerOptions.InvokeIcon` при добавлении маркера на карту.
Этот метод принимает объект [битмапдескриптор](https://developers.google.com/android/reference/com/google/android/gms/maps/model/BitmapDescriptor) , содержащий данные, необходимые для отрисовки значка. Класс [битмапдескрипторфактори](https://developers.google.com/android/reference/com/google/android/gms/maps/model/BitmapDescriptorFactory) предоставляет некоторые вспомогательные методы для упрощения создания `BitmapDescriptor`. В следующем списке представлены некоторые из этих методов.

- `DefaultMarker(float colour)` &ndash; использовать маркер Google Maps по умолчанию, но изменить цвет.

- `FromAsset(string assetName)` &ndash; использовать пользовательский значок из указанного файла в папке Assets.

- `FromBitmap(Bitmap image)` &ndash; использовать указанное растровое изображение в качестве значка.

- `FromFile(string fileName)` &ndash; создать пользовательский значок из файла по указанному пути.

- `FromResource(int resourceId)` &ndash; создать пользовательский значок из указанного ресурса.

В следующем фрагменте кода показан пример создания маркера по умолчанию голубого цвета:

```csharp
public void OnMapReady(GoogleMap map)
{
    MarkerOptions markerOpt1 = new MarkerOptions();
    markerOpt1.SetPosition(new LatLng(50.379444, 2.773611));
    markerOpt1.SetTitle("Vimy Ridge");

    var bmDescriptor = BitmapDescriptorFactory.DefaultMarker (BitmapDescriptorFactory.HueCyan);
    markerOpt1.InvokeIcon(bmDescriptor);

    map.AddMarker(markerOpt1);
}
```

#### <a name="info-windows"></a>Окна сведений

*Окна сведений* — это специальные окна, которые отображаются в виде всплывающих окон для вывода сведений пользователю при касании определенного маркера. По умолчанию в окне info отображается содержимое заголовка маркера. Если заголовок не назначен, окно сведений не отображается. За раз может отображаться только одно окно сведений.

Окно сведений можно настроить, реализовав интерфейс [гуглемап. иинфовиндовадаптер](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap.InfoWindowAdapter) . В этом интерфейсе есть два важных метода:

- `public View GetInfoWindow(Marker marker)` &ndash; вызывается этот метод для получения настраиваемого окна сведений для маркера. Если он возвращает `null`, будет использоваться отрисовка окна по умолчанию. Если этот метод возвращает представление, то это представление будет помещено в рамку окна сведений.

- `public View GetInfoContents(Marker marker)` &ndash; этот метод будет вызван только в том случае, если Жетинфовиндов возвращает `null`. Этот метод может возвращать значение `null`, если будет использоваться отрисовка содержимого окна сведений по умолчанию. В противном случае этот метод должен возвращать представление с содержимым окна сведений.

Окно сведений не является активным представлением. вместо этого Android преобразует представление в статическое растровое изображение и отображает его на изображении. Это означает, что окно сведений не может реагировать на любые события касания или жесты, а также не будет автоматически обновляться. Чтобы обновить окно сведений, необходимо вызвать метод [гуглемап. шовинфовиндов](https://developers.google.com/android/reference/com/google/android/gms/maps/model/Marker.html#showInfoWindow()) .

На следующем рисунке показаны некоторые примеры некоторых настраиваемых окон сведений. Изображение в левой части имеет настроенное содержимое, а изображение справа — окно и его содержимое, настроенное с помощью скругленных углов:

![Примеры окон с маркерами для Мельбурн, включая значки и заполнение. В правом окне есть скругленные углы.](maps-api-images/marker-infowindows.png)

#### <a name="groundoverlays"></a>граундоверлайс

В отличие от маркеров, определяющих определенное расположение на карте, [граундоверлай](https://developers.google.com/android/reference/com/google/android/gms/maps/model/GroundOverlay) — это изображение, которое используется для определения коллекции расположений или области на карте.

##### <a name="adding-a-groundoverlay"></a>Добавление Граундоверлай

Добавление наложения заземления на карту аналогично добавлению маркера к карте. Сначала создается объект [граундоверлайоптионс](https://developers.google.com/android/reference/com/google/android/gms/maps/model/GroundOverlayOptions) . Затем этот объект передается в качестве параметра методу [`GoogleMap.AddGroundOverlay`](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap.html#addGroundOverlay(com.google.android.gms.maps.model.GroundOverlayOptions)) , который возвращает объект `GroundOverlay`. Этот фрагмент кода является примером добавления наложения заземления на карту.

```csharp
BitmapDescriptor image = BitmapDescriptorFactory.FromResource(Resource.Drawable.polarbear);
GroundOverlayOptions groundOverlayOptions = new GroundOverlayOptions()
    .Position(position, 150, 200)
    .InvokeImage(image);
GroundOverlay myOverlay = googleMap.AddGroundOverlay(groundOverlayOptions);
```

На следующем снимке экрана показан этот наложенный элемент на карте:

[![пример схемы с наложенным изображением полярного носителя](maps-api-images/image09-sml.png)](maps-api-images/image09.png#lightbox)

#### <a name="lines-circles-and-polygons"></a>Линии, круги и многоугольники

Существует три простых типа геометрических значений, которые можно добавить к карте:

- **Ломаная** линия — это ряд Соединенных сегментов. Он может пометить путь на карте или создать геометрическую форму.

- **Кружок** — рисуется окружность на карте.

- **Многоугольник** — это замкнутая фигура для пометки областей на карте.

##### <a name="polylines"></a>Ломаных линий

[Ломаная](https://developers.google.com/android/reference/com/google/android/gms/maps/model/Polyline) линия — это список последовательных `LatLng` объектов, которые указывают вершины для каждого сегмента линии. Чтобы создать ломаную линию, сначала нужно создать объект `PolylineOptions` и добавить в него точки. Затем объект `PolylineOption` передается `GoogleMap` объекту путем вызова метода `AddPolyline`.

```csharp
PolylineOption rectOptions = new PolylineOption();
rectOptions.Add(new LatLng(37.35, -122.0));
rectOptions.Add(new LatLng(37.45, -122.0));
rectOptions.Add(new LatLng(37.45, -122.2));
rectOptions.Add(new LatLng(37.35, -122.2));
rectOptions.Add(new LatLng(37.35, -122.0)); // close the polyline - this makes a rectangle.

googleMap.AddPolyline(rectOptions);
```

##### <a name="circles"></a>Мелких

Круги создаются путем создания объекта [Цирклеоптион](https://developers.google.com/android/reference/com/google/android/gms/maps/model/CircleOptions) , который будет указывать центр и радиус окружности в метрес. Окружность рисуется на карте путем вызова [гуглемап. аддЦиркле](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap.html#addCircle(com.google.android.gms.maps.model.CircleOptions)).
В следующем фрагменте кода показано, как нарисовать круг:

```csharp
CircleOptions circleOptions = new CircleOptions ();
circleOptions.InvokeCenter (new LatLng(37.4, -122.1));
circleOptions.InvokeRadius (1000);

googleMap.AddCircle (circleOptions);
```

##### <a name="polygons"></a>Многоугольники

`Polygon`s похожи на `Polyline`s, но они не открыты. `Polygon`s — это замкнутый цикл, который заполняется внутренним.
`Polygon`создаются точно так же, как `Polyline`, за исключением того, что вызывается метод [гуглемап. аддполигон](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap.html#addPolygon(com.google.android.gms.maps.model.PolygonOptions)) .

В отличие от `Polyline`, `Polygon` является самозакрывающимся. Многоугольник будет закрыт с помощью `AddPolygon` метода путем рисования линии, соединяющей первую и последнюю точки. В следующем фрагменте кода будет создан закрашенный прямоугольник в той же области, что и предыдущий фрагмент кода, в `Polyline` примере.

```csharp
PolygonOptions rectOptions = new PolygonOptions();
rectOptions.Add(new LatLng(37.35, -122.0));
rectOptions.Add(new LatLng(37.45, -122.0));
rectOptions.Add(new LatLng(37.45, -122.2));
rectOptions.Add(new LatLng(37.35, -122.2));
// notice we don't need to close off the polygon

googleMap.AddPolygon(rectOptions);
```

## <a name="responding-to-user-events"></a>Реагирование на события пользователя

Существует три типа взаимодействий, которые может иметь пользователь с картой:

- **Щелчок маркера** — пользователь щелкает маркер.

- **Перетаскивание маркера** — пользователь долго щелкнул мпаржер

- **Окно сведений щелкните** , чтобы пользователь щелкнул окно сведений.

Каждое из этих событий будет обсуждаться более подробно ниже.

### <a name="marker-click-events"></a>События щелчка маркера

Событие `MarkerClicked` возникает при касании пользователем маркера. Это событие принимает объект `GoogleMap.MarkerClickEventArgs` в качестве параметра. Этот класс содержит два свойства:

- `GoogleMap.MarkerClickEventArgs.Handled` &ndash; это свойство должно иметь значение `true`, чтобы указать, что обработчик событий использовал событие. Если задано значение `false`, то в дополнение к пользовательскому поведению обработчика событий будет возникать поведение по умолчанию.

- `Marker` &ndash; это свойство является ссылкой на маркер, который вызвал событие `MarkerClick`.

В этом фрагменте кода показан пример `MarkerClick`, который изменит положение камеры на новое место на карте:

```csharp
void MapOnMarkerClick(object sender, GoogleMap.MarkerClickEventArgs markerClickEventArgs)
{
    markerClickEventArgs.Handled = true;

    var marker = markerClickEventArgs.Marker;
    if (marker.Id.Equals(gotMauiMarkerId))
    {
        LatLng InMaui = new LatLng(20.72110, -156.44776);

        // Move the camera to look at Maui.
        PositionPolarBearGroundOverlay(InMaui);
        googleMap.AnimateCamera(CameraUpdateFactory.NewLatLngZoom(InMaui, 13));
        gotMauiMarkerId = null;
        polarBearMarker.Remove();
        polarBearMarker = null;
    }
    else
    {
        Toast.MakeText(this, $"You clicked on Marker ID {marker.Id}", ToastLength.Short).Show();
    }
}
```

### <a name="marker-drag-events"></a>События перетаскивания маркеров

Это событие возникает, когда пользователь хочет перетащить маркер. По умолчанию маркеры не могут быть перенесены. Маркер можно задать как перетаскиваемый, присвоив свойству `Marker.Draggable` значение `true` или вызвав метод `MarkerOptions.Draggable` с `true` в качестве параметра.

Чтобы перетащить маркер, пользователь должен сначала щелкнуть маркер, а затем его палец должен остаться на карте. Когда палец пользователя перетаскивается на экран, маркер перемещается. Когда палец пользователя выходит за пределы экрана, маркер останется на месте.

В следующем списке описаны различные события, которые будут вызываться для перетаскиваемого маркера:

- `GoogleMap.MarkerDragStart(object sender, GoogleMap.MarkerDragStartEventArgs e)` &ndash; это событие возникает при первом перетаскивании маркера пользователем.

- `GoogleMap.MarkerDrag(object sender, GoogleMap.MarkerDragEventArgs e)` &ndash; это событие возникает при перетаскивании маркера.

- `GoogleMap.MarkerDragEnd(object sender, GoogleMap.MarkerDragEndEventArgs e)` &ndash; это событие возникает, когда пользователь завершает перетаскивание маркера.

Каждый `EventArgs` содержит одно свойство с именем `P0`, которое является ссылкой на перетаскиваемый объект `Marker`.

### <a name="info-window-click-events"></a>События щелчка окна сведений

Одновременно может отображаться только одно окно сведений. Когда пользователь щелкает окно сведений на карте, объект Map создает событие `InfoWindowClick`. В следующем фрагменте кода показано, как подключить обработчик к событию:

```csharp
public void OnMapReady(GoogleMap map)
{
    map.InfoWindowClick += MapOnInfoWindowClick;
}

private void MapOnInfoWindowClick (object sender, GoogleMap.InfoWindowClickEventArgs e)
{
    Marker myMarker = e.Marker;
    // Do something with marker.
}
```

Помните, что окно сведений является статическим `View`, отображаемым в виде изображения на карте. Любые мини-приложения, такие как кнопки, флажки или текстовые представления, помещенные в окно сведений, будут инерт и не смогут отвечать на какие-либо их целочисленные события.

## <a name="related-links"></a>Связанные ссылки

- [симплемапдемо](https://github.com/xamarin/monodroid-samples/tree/master/MapsAndLocationDemo_v3/SimpleMapDemo)
- [Сервисы Google Play](https://developers.google.com/android/guides/overview)
- [Google Maps Android API v2](https://developers.google.com/maps/documentation/android-sdk/intro)
- [Сервисы Google Play APK](https://play.google.com/store/apps/details?id=com.google.android.gms&hl=en)
- [Получение ключа API Google Maps](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)
- [использует-Library](https://developer.android.com/guide/topics/manifest/uses-library-element)
- [Использование функции](https://developer.android.com/guide/topics/manifest/uses-feature-element)
