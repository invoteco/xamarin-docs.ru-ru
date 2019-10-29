---
title: Службы определения местоположения на Android
description: В этом руководство рассматривается отслеживание расположения в приложениях Android и показано, как получить расположение пользователя с помощью API службы расположения Android, а также поставщика расположения с плавким предохранителем, доступного в Google API служб расположения.
ms.prod: xamarin
ms.assetid: 0008682B-6CEF-0C1D-3200-56ECF58F5D3C
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 05/22/2018
ms.openlocfilehash: e027d41e98c26ef1659c27ab05df3052e19cc670
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73027138"
---
# <a name="location-services-on-android"></a>Службы определения местоположения на Android

_В этом руководство рассматривается отслеживание расположения в приложениях Android и показано, как получить расположение пользователя с помощью API службы расположения Android, а также поставщика расположения с плавким предохранителем, доступного в Google API служб расположения._

Android предоставляет доступ к различным технологиям расположения, таким как расположение ячеек Tower, Wi-Fi и GPS. Сведения о каждой технологии расположения абстрактны с помощью *поставщиков расположений*, что позволяет приложениям получать расположения одинаково независимо от используемого поставщика. В этом руководство рассматривается поставщик расположения с плавким предохранителем, часть Сервисы Google Play, которая Intelligent определяет лучший способ получения расположения устройств в зависимости от доступных поставщиков и способа использования устройства. API службы расположения Android и показывает, как взаимодействовать со службой расположения системы с помощью `LocationManager`. Во второй части этого руководством рассматривается API служб расположения Android с помощью `LocationManager`.

Как правило, приложения должны использовать поставщик расположения с плавким предохранителем, выменяя Старый API службы расположения Android, только если это необходимо.

## <a name="location-fundamentals"></a>Основные принципы расположения

В Android независимо от того, какой API вы выбрали для работы с данными о расположении, некоторые концепции останутся неизменными. В этом разделе описываются поставщики расположения и разрешения, связанные с расположением.

### <a name="location-providers"></a>Поставщики расположения

Для определения местоположения пользователя используются различные технологии. Используемое оборудование зависит от типа *поставщика расположения* , выбранного для задания сбора данных. Android использует три поставщика расположения:

- **Поставщик gps** &ndash; GPS обеспечивает наиболее точное расположение, использует максимальную мощь и лучше всего туризм. Этот поставщик использует сочетание GPS и помощи GPS ([агпс](https://en.wikipedia.org/wiki/Assisted_GPS)), которое ВОЗВРАЩАЕТ данные GPS, собранные сотовой сетью Towers.

- &ndash; **поставщика сети** предоставляет сочетание Wi-Fi и сотовой связи, включая данные агпс, собираемые ячейкой Towers. Он использует меньше энергии, чем поставщик GPS, но возвращает данные о местоположении различной точности.

- **Пассивный поставщик** &ndash; параметр "проникновения" с помощью поставщиков, запрашиваемых другими приложениями или службами для создания данных расположения в приложении. Это менее надежный вариант, но режим энергосбережения идеально подходит для приложений, которые не нуждаются в обновлениях постоянного расположения для работы.

Поставщики расположений не всегда доступны. Например, может потребоваться использовать GPS для нашего приложения, но может быть отключено GPS в параметрах, или устройство может вообще не иметь GPS. Если конкретный поставщик недоступен, выбор этого поставщика может вернуть `null`.

### <a name="location-permissions"></a>Разрешения расположения

Приложению с поддержкой местоположения требуется доступ к аппаратным датчикам устройства для получения данных GPS, Wi-Fi и сотовой сети. Управление доступом осуществляется с помощью соответствующих разрешений в манифесте Android приложения.
Существует два доступных разрешения &ndash; в зависимости от требований вашего приложения и выбора API, необходимо разрешить одно из следующих действий:

- `ACCESS_FINE_LOCATION` &ndash; позволяет приложению получить доступ к GPS.
    Требуется для *поставщика GPS* и параметров *пассивного поставщика* (*пассивный поставщик должен иметь разрешение на доступ к данным GPS, собранным другим приложением или службой*). Необязательное разрешение для *поставщика сети*.

- `ACCESS_COARSE_LOCATION` &ndash; позволяет приложению получать доступ к сотовой сети и расположению Wi-Fi. Требуется для *поставщика сети* , если параметр `ACCESS_FINE_LOCATION` не задан.

Для приложений, предназначенных для API версии 21 (программная поддержка Android 5,0) или выше, можно включить `ACCESS_FINE_LOCATION` и по-прежнему работать на устройствах, на которых отсутствует оборудование GPS. Если для приложения требуется оборудование GPS, необходимо явным образом добавить элемент `uses-feature` `android.hardware.location.gps` в манифест Android. Дополнительные сведения см. в разделе Справочник по элементу " [Использование компонентов](https://developer.android.com/guide/topics/manifest/uses-feature-element.html) Android".

Чтобы задать разрешения, разверните папку **Свойства** в **панель решения** и дважды щелкните **файл AndroidManifest. XML**. Разрешения будут перечислены в разделе **необходимые разрешения**:

[Снимок экрана:![параметры необходимых разрешений манифеста Android](location-images/location-01-xs.png)](location-images/location-01-xs.png#lightbox)

Установка любого из этих разрешений означает Android, что приложению требуется разрешение у пользователя для доступа к поставщикам расположения. Устройства, на которых выполняется API уровня 22 (Android 5,1) или ниже, будут предлагать пользователю предоставить эти разрешения при каждом установке приложения. На устройствах с уровнем API 23 (Android 6,0) или более поздней версии приложение должно выполнить проверку разрешений во время выполнения перед выполнением запроса к поставщику расположения. 

> [!NOTE]
>Примечание. параметр `ACCESS_FINE_LOCATION` подразумевает доступ к данным грубого и точного расположения. Вам никогда не нужно задавать оба разрешения, но только *минимальное* разрешение, необходимое для работы приложения.

В этом фрагменте кода приведен пример того, как проверить, имеет ли приложение разрешение на `ACCESS_FINE_LOCATION`.

```csharp
 if (ContextCompat.CheckSelfPermission(this, Manifest.Permission.AccessFineLocation) == Permission.Granted)
{
    StartRequestingLocationUpdates();
    isRequestingLocationUpdates = true;
}
else
{
    // The app does not have permission ACCESS_FINE_LOCATION 
}
```

Приложения должны быть отказоустойчивыми в ситуации, когда пользователь не предоставит разрешение (или отменяет разрешение) и может правильно справиться с этой ситуацией. Дополнительные сведения о реализации проверок разрешений во время выполнения в Xamarin. Android см. в разделе " [рекомендации по разрешениям](~/android/app-fundamentals/permissions.md) ".

## <a name="using-the-fused-location-provider"></a>Использование поставщика расположения с плавким предохранителем

Поставщик расположения с плавким предохранителем является предпочтительным способом для приложений Android, которые получают обновления расположения с устройства, так как он эффективно выбирает поставщик расположения во время выполнения, чтобы обеспечить оптимальную информацию о расположении в режиме экономного аккумулятора. Например, пользователь, проходящего по туризм, получает лучшее место чтения с помощью GPS. Если пользователь попытается получить расдвижки, где GPS работает плохо (если вообще), поставщик расположения с плавким предохранителем может автоматически переключиться на Wi-Fi, который работает лучше.

API поставщика расположения с плавким предохранителем предоставляет множество других средств для расширения возможностей, учитывающих местоположение приложений, включая геозоны и мониторинг активности. В этом разделе основное внимание уделяется основам настройки `LocationClient`, установлению поставщиков и получению расположения пользователя.

Поставщик расположения с плавким предохранителем является частью [сервисы Google Play](https://developer.android.com/google/play-services/index.html).
Пакет Сервисы Google Play должен быть правильно установлен и настроен в приложении для работы API поставщика расположения с плавким предохранителем, а на устройстве должно быть установлено Сервисы Google Play APK.

Прежде чем приложение Xamarin. Android сможет использовать поставщик расположения с плавким предохранителем, он должен добавить в проект пакет **Xamarin. гуглеплайсервицес. Maps** . Кроме того, в любые исходные файлы, которые ссылаются на классы, описанные ниже, должны быть добавлены следующие инструкции `using`.

```csharp
using Android.Gms.Common;
using Android.Gms.Location;
```

### <a name="checking-if-google-play-services-is-installed"></a>Проверка установки Сервисы Google Play

Xamarin. Android будет аварийно завершить работу, если при попытке использовать поставщик расположения с плавким предохранителем, когда Сервисы Google Play не установлен (или устарел), возникнет исключение времени выполнения.  Если Сервисы Google Play не установлен, приложение должно вернуться к службе расположения Android, о которой говорилось выше. Если Сервисы Google Play устарел, приложение может отобразить пользователю сообщение с просьбой обновить установленную версию Сервисы Google Play.

В этом фрагменте кода приведен пример того, как действие Android может программно проверить, установлено ли Сервисы Google Play:

```csharp
bool IsGooglePlayServicesInstalled()
{
    var queryResult = GoogleApiAvailability.Instance.IsGooglePlayServicesAvailable(this);
    if (queryResult == ConnectionResult.Success)
    {
        Log.Info("MainActivity", "Google Play Services is installed on this device.");
        return true;
    }

    if (GoogleApiAvailability.Instance.IsUserResolvableError(queryResult))
    {
        // Check if there is a way the user can resolve the issue
        var errorString = GoogleApiAvailability.Instance.GetErrorString(queryResult);
        Log.Error("MainActivity", "There is a problem with Google Play Services on this device: {0} - {1}",
                  queryResult, errorString);

        // Alternately, display the error to the user.
    }

    return false;
}
```

### <a name="fusedlocationproviderclient"></a>фуседлокатионпровидерклиент

Для взаимодействия с поставщиком расположения с плавким предохранителем приложение Xamarin. Android должно иметь экземпляр `FusedLocationProviderClient`. Этот класс предоставляет необходимые методы для подписки на обновления расположения и получения последнего известного расположения устройства.

Метод `OnCreate` действия является подходящим местом для получения ссылки на `FusedLocationProviderClient`, как показано в следующем фрагменте кода:

```csharp
public class MainActivity: AppCompatActivity
{
    FusedLocationProviderClient fusedLocationProviderClient;

    protected override void OnCreate(Bundle bundle) 
    {
        fusedLocationProviderClient = LocationServices.GetFusedLocationProviderClient(this);
    }
}
```

### <a name="getting-the-last-known-location"></a>Получение последнего известного расположения

Метод `FusedLocationProviderClient.GetLastLocationAsync()` предоставляет простой и неблокирующий способ для приложения Xamarin. Android, чтобы быстро получить Последнее известное расположение устройства с минимальными затратами на написание кода.

В этом фрагменте кода показано, как использовать метод `GetLastLocationAsync` для получения расположения устройства:

```csharp
async Task GetLastLocationFromDevice()
{
    // This method assumes that the necessary run-time permission checks have succeeded.
    getLastLocationButton.SetText(Resource.String.getting_last_location);
    Android.Locations.Location location = await fusedLocationProviderClient.GetLastLocationAsync();

    if (location == null)
    {
        // Seldom happens, but should code that handles this scenario
    }
    else
    {
        // Do something with the location 
        Log.Debug("Sample", "The latitude is " + location.Latitude);
    }
}
```

### <a name="subscribing-to-location-updates"></a>Подписка на обновления расположения

Приложение Xamarin. Android также может подписываться на обновления расположения из поставщика расположений с плавким предохранителем с помощью метода `FusedLocationProviderClient.RequestLocationUpdatesAsync`, как показано в следующем фрагменте кода:

```csharp
await fusedLocationProviderClient.RequestLocationUpdatesAsync(locationRequest, locationCallback);
```

Этот метод принимает два параметра:

- **`Android.Gms.Location.LocationRequest`** &ndash; объект `LocationRequest` — это то, как приложение Xamarin. Android передает параметры, как именно должен работать поставщик расположения с предохранителем. `LocationRequest` содержит такие сведения, как часто выполняемые запросы или важность обновления расположения. Например, важный запрос расположения приведет к тому, что устройство будет использовать GPS и, следовательно, больше энергии при определении расположения. В этом фрагменте кода показано, как создать `LocationRequest` для местоположения с высокой точностью, проверяя примерно каждые пять минут на наличие обновления расположения (но не более чем за две минуты между запросами). Поставщик расположения с плавким предохранителем будет использовать `LocationRequest` в качестве руководства для того, какой поставщик расположения использовать при попытке определить расположение устройства:

    ```csharp
    LocationRequest locationRequest = new LocationRequest()
                                      .SetPriority(LocationRequest.PriorityHighAccuracy)
                                      .SetInterval(60 * 1000 * 5)
                                      .SetFastestInterval(60 * 1000 * 2);
    ```

- **`Android.Gms.Location.LocationCallback`** &ndash; чтобы получить обновления расположения, приложение Xamarin. Android должно подделать подкласс абстрактного класса `LocationProvider`. Этот класс предоставлял два метода, которые могут быть вызваны поставщиком расположения плавким предохранителем для обновления приложения с информацией о расположении. Это будет рассмотрено более подробно ниже.

Чтобы уведомить приложение Xamarin. Android об обновлении расположения, поставщик расположения с предохранителем будет вызывать `LocationCallBack.OnLocationResult(LocationResult result)`. Параметр `Android.Gms.Location.LocationResult` будет содержать сведения о расположении обновления.

Когда поставщик расположения с плавким предохранителем обнаруживает изменение доступности данных расположения, он вызывает метод `LocationProvider.OnLocationAvailability(LocationAvailability
locationAvailability)`. Если свойство `LocationAvailability.IsLocationAvailable` возвращает `true`, то можно предположить, что результаты расположения устройства, сообщаемые `OnLocationResult`, являются точными и в соответствии с требованиями `LocationRequest`. Если `IsLocationAvailable` имеет значение false, то результаты поиска не будут возвращены `OnLocationResult`.

Этот фрагмент кода является примером реализации объекта `LocationCallback`:

```csharp
public class FusedLocationProviderCallback : LocationCallback
{
    readonly MainActivity activity;

    public FusedLocationProviderCallback(MainActivity activity)
    {
        this.activity = activity;
    }

    public override void OnLocationAvailability(LocationAvailability locationAvailability)
    {
        Log.Debug("FusedLocationProviderSample", "IsLocationAvailable: {0}",locationAvailability.IsLocationAvailable);
    }

    public override void OnLocationResult(LocationResult result)
    {
        if (result.Locations.Any())
        {
            var location = result.Locations.First();
            Log.Debug("Sample", "The latitude is :" + location.Latitude);
        }
        else
        {
            // No locations to work with.
        }
    }
}
```

## <a name="using-the-android-location-service-api"></a>Использование API службы расположения Android

Служба расположения Android — это более старый API для использования сведений о расположении в Android. Данные о расположении собираются аппаратными датчиками и собираются системной службой, которую можно получить в приложении с помощью класса `LocationManager` и `ILocationListener`.

Служба расположения лучше всего подходит для приложений, которые должны выполняться на устройствах, на которых не установлена Сервисы Google Play.

Служба расположения — это особый тип [службы](https://developer.android.com/guide/components/services.html) , управляемый системой. Системная служба взаимодействует с оборудованием устройства и всегда работает. Чтобы выбрать обновления расположения в нашем приложении, мы будем подписываться на обновления расположения из службы расположения системы, используя `LocationManager` и вызов `RequestLocationUpdates`.

Чтобы получить расположение пользователя с помощью службы расположения Android, требуется выполнить несколько действий:

1. Получите ссылку на службу `LocationManager`.
2. Реализуйте интерфейс `ILocationListener` и обрабатывайте события при изменении расположения.
3. Используйте `LocationManager`, чтобы запросить обновления расположения для указанного поставщика. `ILocationListener` из предыдущего шага будет использоваться для получения обратных вызовов от `LocationManager`.
4. Останавливает обновления расположения, когда приложение больше не подходит для получения обновлений.

### <a name="location-manager"></a>Диспетчер расположения

Можно получить доступ к службе расположения системы с помощью экземпляра класса `LocationManager`. `LocationManager` — это специальный класс, который позволяет взаимодействовать со службой расположения системы и вызывать методы. Приложение может получить ссылку на `LocationManager`, вызвав `GetSystemService` и передав тип службы, как показано ниже:

```csharp
LocationManager locationManager = (LocationManager) GetSystemService(Context.LocationService);
```

`OnCreate` является хорошим местом для получения ссылки на `LocationManager`.
Рекомендуется, чтобы `LocationManager` в качестве переменной класса, чтобы мы могли вызвать ее в различных точках жизненного цикла действия.

### <a name="request-location-updates-from-the-locationmanager"></a>Запрос обновлений расположения из Локатионманажер

Когда приложение содержит ссылку на `LocationManager`, ему нужно указать `LocationManager`, какой тип сведений о расположении требуется, и частоту обновления данных. Для этого вызовите `RequestLocationUpdates` объекта `LocationManager` и передайте в него некоторые критерии для обновлений и обратный вызов, который получит обновления расположения. Этот обратный вызов является типом, который должен реализовывать интерфейс `ILocationListener` (более подробно далее в этом разделе).

Метод `RequestLocationUpdates` сообщает службе расположения системы, что приложение должно начать получать обновления расположения. Этот метод позволяет указать поставщик, а также порог времени и расстояния для управления частотой обновления. Например, приведенный ниже метод запрашивает обновление расположения от поставщика расположения GPS каждые 2000 миллисекунд и только при изменении расположения более 1 метре:

```csharp
// For this example, this method is part of a class that implements ILocationListener, described below
locationManager.RequestLocationUpdates(LocationManager.GpsProvider, 2000, 1, this);
```

Приложение должно запрашивать обновления расположения только так часто, как это необходимо для выполнения приложения. Это сохраняет время работы от аккумулятора и повышает удобство работы пользователя.

### <a name="responding-to-updates-from-the-locationmanager"></a>Реагирование на обновления от Локатионманажер

После того как приложение запросило обновления из `LocationManager`, оно может получить сведения от службы, реализовав интерфейс [`ILocationListener`](xref:Android.Locations.ILocationListener) . Этот интерфейс предоставляет четыре метода для прослушивания службы расположения и поставщика расположения, `OnLocationChanged`. Система будет вызывать `OnLocationChanged`, когда расположение пользователя изменяется достаточно, чтобы оно было изменено в соответствии с критериями, заданными при запросе обновлений расположения. 

В следующем коде показаны методы в интерфейсе `ILocationListener`:

```csharp
public class MainActivity : AppCompatActivity, ILocationListener
{
    TextView latitude;
    TextView longitude;
    
    public void OnLocationChanged (Location location)
    {
        // called when the location has been updated.
    }
    
    public OnProviderDisabled(string locationProvider)
    {
        // called when the user disables the provider
    }
    
    public OnProviderEnabled(string locationProvider)
    {
        // called when the user enables the provider
    }
    
    public OnStatusChanged(string locationProvider, Availability status, Bundle extras)
    {
        // called when the status of the provider changes (there are a variety of reasons for this)
    }
}
```

### <a name="unsubscribing-to-locationmanager-updates"></a>Отмена подписки на обновления Локатионманажер

Чтобы обеспечить экономию системных ресурсов, приложение должно как можно скорее отменить подписываться на обновления расположения. Метод `RemoveUpdates` указывает `LocationManager`у отказаться от отправки обновлений в наше приложение.  Например, действие может вызвать `RemoveUpdates` в методе `OnPause`, чтобы можно было экономить электроэнергию, если приложение не нуждается в обновлениях расположения, пока его действие не находится на экране.

```csharp
protected override void OnPause ()
{
    base.OnPause ();
    locationManager.RemoveUpdates (this);
}
```

Если приложению необходимо получить обновления расположения в фоновом режиме, необходимо создать пользовательскую службу, которая подписывается на службу определения местоположения системы. Дополнительные сведения см. в разделе " [фоновый режим" в руководстве по службам Android](~/android/app-fundamentals/services/index.md) .

### <a name="determining-the-best-location-provider-for-the-locationmanager"></a>Определение лучшего поставщика расположения для Локатионманажер

Указанное выше приложение устанавливает GPS в качестве поставщика расположения. Однако GPS может быть недоступен во всех случаях, например если устройство находится в дверцах или у него нет приемника GPS. В этом случае результатом является `null` возврата для поставщика.

Чтобы приложение работало, когда GPS недоступен, используйте метод `GetBestProvider`, чтобы запросить наилучший доступный поставщик расположений (поддерживаемых устройствами и пользователей) при запуске приложения. Вместо передачи конкретного поставщика можно указать `GetBestProvider` требования к поставщику, такие как точность и энергопотребление, с помощью [объекта`Criteria`](xref:Android.Locations.Criteria). `GetBestProvider` возвращает лучший поставщик для заданных критериев.

В следующем коде показано, как получить наилучший доступный поставщик и использовать его при запросе обновлений расположения:

```csharp
Criteria locationCriteria = new Criteria();   
locationCriteria.Accuracy = Accuracy.Coarse;
locationCriteria.PowerRequirement = Power.Medium;

locationProvider = locationManager.GetBestProvider(locationCriteria, true);

if(locationProvider != null)
{
    locationManager.RequestLocationUpdates (locationProvider, 2000, 1, this);
}
else
{
    Log.Info(tag, "No location providers available");
}
```

> [!NOTE]
> Если пользователь отключил всех поставщиков расположения, `GetBestProvider` возвратит `null`. Чтобы увидеть, как этот код работает на реальном устройстве, обязательно включите GPS, Wi-Fi и сотовые сети в **параметрах Google > расположение >** , как показано на следующем снимке экрана:
>
> [экран режима расположения параметров![на телефоне с Android](location-images/location-02.png)](location-images/location-02.png#lightbox)
>
> На следующем снимке экрана показано приложение расположения, выполняемое с помощью `GetBestProvider`.
>
> [![приложение Жетбестпровидер, отображающее широту, долготу и поставщика](location-images/location-03.png)](location-images/location-03.png#lightbox)
>
> Помните, что `GetBestProvider` не изменяет этот поставщик динамически. Вместо этого он определяет наилучший доступный поставщик один раз во время жизненного цикла действия. Если состояние поставщика изменится после его установки, приложению потребуется дополнительный код в `ILocationListener` методы &ndash; `OnProviderEnabled`, `OnProviderDisabled`и `OnStatusChanged` &ndash;, чтобы все возможности, связанные с коммутатором поставщика, обрабатывались.

## <a name="summary"></a>Сводка

В этом разделе описано получение расположения пользователя с помощью службы расположения Android и поставщика расположений с плавким предохранителем из Google API служб расположения.

## <a name="related-links"></a>Связанные ссылки

- [Расположение (пример)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/location)
- [Фуседлокатионпровидер (пример)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/fusedlocationprovider)
- [Сервисы Google Play](https://developer.android.com/google/play-services/index.html)
- [Класс критериев](xref:Android.Locations.Criteria)
- [Класс Локатионманажер](xref:Android.Locations.LocationManager)
- [Класс Локатионлистенер](xref:Android.Locations.ILocationListener)
- [API Локатионклиент](https://developer.android.com/reference/com/google/android/gms/location/LocationClient.html)
- [API Локатионлистенер](https://developer.android.com/reference/com/google/android/gms/location/LocationListener.html)
- [API Локатионрекуест](https://developer.android.com/reference/com/google/android/gms/location/LocationRequest.html)
