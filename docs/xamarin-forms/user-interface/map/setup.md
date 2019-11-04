---
title: Инициализация и Настройка карт Xamarin. Forms
description: Для использования функций карт в приложении требуется пакет NuGet Xamarin. Forms. Maps. Кроме того, для доступа к расположению пользователя требуются разрешения на расположение, предоставленные приложению.
ms.prod: xamarin
ms.assetid: 59CD1344-8248-406C-9144-0C8A67141E5B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/31/2019
ms.openlocfilehash: dd451ae1acd233c1d3de675357bb172f25716f59
ms.sourcegitcommit: 3ea19e3a51515b30349d03c70a5b3acd7eca7fe7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/01/2019
ms.locfileid: "73426291"
---
# <a name="xamarinforms-map-initialization-and-configuration"></a>Инициализация и Настройка карт Xamarin. Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

Элемент управления [`Map`](xref:Xamarin.Forms.Maps.Map) использует собственный элемент управления картой на каждой платформе. Это обеспечивает быстрый, знакомый интерфейс карт для пользователей, но означает, что некоторые действия по настройке необходимы для соблюдения требований к API для каждой платформы.

## <a name="map-initialization"></a>Инициализация карт

Элемент управления [`Map`](xref:Xamarin.Forms.Maps.Map) предоставляется пакетом NuGet [Xamarin. Forms. Maps](https://www.nuget.org/packages/Xamarin.Forms.Maps/) , который должен быть добавлен в каждый проект решения.

После установки пакета NuGet [Xamarin. Forms. Maps](https://www.nuget.org/packages/Xamarin.Forms.Maps/) он должен быть инициализирован в каждом проекте платформы.

В iOS это должно произойти в **AppDelegate.CS** путем вызова метода `Xamarin.FormsMaps.Init` *после* метода `Xamarin.Forms.Forms.Init`:

```csharp
Xamarin.FormsMaps.Init();
```

В Android это должно произойти в **MainActivity.CS** путем вызова метода `Xamarin.FormsMaps.Init` *после* метода `Xamarin.Forms.Forms.Init`:

```csharp
Xamarin.FormsMaps.Init(this, savedInstanceState);
```

В универсальная платформа Windows (UWP) это должно произойти в **MainPage.XAML.CS** путем вызова метода `Xamarin.FormsMaps.Init` из конструктора `MainPage`:

```csharp
Xamarin.FormsMaps.Init("INSERT_AUTHENTICATION_TOKEN_HERE");
```

После добавления пакета NuGet и метода инициализации, вызываемого внутри каждого приложения, `Xamarin.Forms.Maps` API можно использовать в проекте с общим кодом.

## <a name="platform-configuration"></a>Конфигурация платформы

Перед отображением этой схемы требуется дополнительная настройка для Android и универсальная платформа Windows (UWP). Кроме того, в iOS, Android и UWP для доступа к расположению пользователя требуются разрешения на размещение, предоставленные приложению.

### <a name="ios"></a>iOS

Для отображения и взаимодействия с картой в iOS не требуется дополнительная настройка. Однако для доступа к службам обнаружения необходимо задать следующие ключи в **info. plist**:

- iOS 11 и более поздние версии
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
<string>Can we use your location when your application is being used?</string>
<key>NSLocationAlwaysAndWhenInUseUsageDescription</key>
<string>Can we use your location at all times?</string>
```

Записи **info. plist** также можно добавить в представление **исходного кода** при редактировании файла **info. plist** :

![Info. plist для iOS 8](setup-images/ios8-map-permissions.png "Обязательные записи info. plist для iOS 8")

После того, как приложение попытается получить доступ к расположению пользователя, будет выведено приглашение, запрашивающее доступ:

[![Снимок экрана запроса на разрешение расположения в iOS](setup-images/permission-ios.png "запрос разрешения iOS")](setup-images/permission-ios-large.png#lightbox "запрос разрешения iOS")

### <a name="android"></a>Android

Процесс настройки отображения и взаимодействия с картой в Android:

1. Получите ключ API Google Maps и добавьте его в манифест.
1. Укажите номер версии Google Play Services в манифесте.
1. Укажите требование для устаревшей библиотеки Apache HTTP в манифесте.
1. используемых Укажите разрешение WRITE_EXTERNAL_STORAGE в манифесте.
1. используемых Укажите разрешения расположения в манифесте.
1. используемых Запросите разрешения расположения среды выполнения в классе `MainActivity`.

Пример правильно настроенного файла манифеста см. в разделе [AndroidManifest. XML](https://github.com/xamarin/xamarin-forms-samples/blob/master/WorkingWithMaps/WorkingWithMaps/WorkingWithMaps.Android/Properties/AndroidManifest.xml) из примера приложения.

#### <a name="get-a-google-maps-api-key"></a>Получение ключа API Google Maps

Чтобы использовать [API Google Maps](https://developers.google.com/maps/documentation/android/) в Android, необходимо создать ключ API. Для этого следуйте инструкциям в статье [Получение ключа API Google Maps](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md).

После получения ключа API его необходимо добавить в элемент `<application>` файла **Properties/AndroidManifest. XML** :

```xml
<application ...>
    <meta-data android:name="com.google.android.maps.v2.API_KEY" android:value="PASTE-YOUR-API-KEY-HERE" />
</application>
```

При этом ключ API внедряется в манифест. Без допустимого ключа API элемент управления [`Map`](xref:Xamarin.Forms.Maps.Map) будет отображать пустую сетку.

> [!NOTE]
> Обратите внимание, что для APK доступа к Google Maps необходимо включить отпечатки SHA-1 и имена пакетов для каждого хранилища ключей (Отладка и выпуск), которое используется для подписания APK. Например, если вы используете один компьютер для отладки и другой компьютер для создания APK выпуска, следует включить отпечаток сертификата SHA-1 из хранилища ключей отладки первого компьютера и отпечаток сертификата SHA-1 из хранилища ключей выпуска второй компьютер. Также не забудьте изменить ключевые учетные данные при изменении **имени пакета** приложения. См. [раздел Получение ключа API Google Maps](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md).

#### <a name="specify-the-google-play-services-version-number"></a>Укажите номер версии служб Google Play Services

Добавьте следующее объявление в элемент `<application>` **AndroidManifest. XML**:

```xml
<meta-data android:name="com.google.android.gms.version" android:value="@integer/google_play_services_version" />
```

В манифест будет внедрена версия служб Google Play Services, с помощью которой было скомпилировано приложение.

#### <a name="specify-the-requirement-for-the-apache-http-legacy-library"></a>Укажите требование для устаревшей библиотеки Apache HTTP

Если приложение Xamarin. Forms предназначено для API 28 или более поздней версии, необходимо добавить следующее объявление в элемент `<application>` **AndroidManifest. XML**:

```xml
<uses-library android:name="org.apache.http.legacy" android:required="false" />    
```

Это указывает приложению использовать клиентскую библиотеку Apache HTTP, которая была удалена из `bootclasspath` в Android 9.

#### <a name="specify-the-write_external_storage-permission"></a>Указание разрешения WRITE_EXTERNAL_STORAGE

Если приложение предназначено для API 22 или ниже, может потребоваться добавить разрешение `WRITE_EXTERNAL_STORAGE` в манифест, как дочерний элемент `<manifest>`.

```xml
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```

Это не требуется, если приложение предназначено для API 23 или более поздней версии.

#### <a name="specify-location-permissions"></a>Укажите разрешения на расположение

Если приложению требуется доступ к расположению пользователя, необходимо запросить разрешение, добавив `ACCESS_COARSE_LOCATION` или `ACCESS_FINE_LOCATION` разрешения в манифест (или оба) как дочерний элемент элемента `<manifest>`:

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android" android:versionCode="1" android:versionName="1.0" package="com.companyname.myapp">
  ...
  <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
  <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
</manifest>
```

Разрешение `ACCESS_COARSE_LOCATION` позволяет API использовать Wi-Fi или мобильные данные или оба значения для определения расположения устройства. Разрешения `ACCESS_FINE_LOCATION` позволяют API использовать систему глобальной позиционирования (GPS), Wi-Fi или мобильные данные для определения точного расположения по возможности.

Кроме того, эти разрешения можно включить с помощью редактора манифестов, чтобы добавить следующие разрешения.

- `AccessCoarseLocation`
- `AccessFineLocation`

Они показаны на снимке экрана ниже:

![Необходимые разрешения для Android](setup-images/android-map-permissions.png "Необходимые разрешения для Android")

#### <a name="request-runtime-location-permissions"></a>Запросить разрешения расположения среды выполнения

Если приложение предназначено для API 23 или более поздней версии и должно иметь доступ к расположению пользователя, оно должно проверить наличие необходимого разрешения во время выполнения и запросить его в случае его отсутствия. Это можно обеспечить, выполнив следующие действия.

1. В классе `MainActivity` добавьте следующие поля:

    ```csharp
    const int RequestLocationId = 0;

    readonly string[] LocationPermissions =
    {
        Manifest.Permission.AccessCoarseLocation,
        Manifest.Permission.AccessFineLocation
    };
    ```

1. В классе `MainActivity` добавьте следующее переопределение `OnStart`.

    ```csharp
    protected override void OnStart()
    {
        base.OnStart();

        if ((int)Build.VERSION.SdkInt >= 23)
        {
            if (CheckSelfPermission(Manifest.Permission.AccessFineLocation) != Permission.Granted)
            {
                RequestPermissions(LocationPermissions, RequestLocationId);
            }
            else
            {
                // Permissions already granted - display a message.
            }
        }
    }
    ```

    При условии, что приложение предназначено для API 23 или более поздней версии, этот код выполняет проверку разрешения на выполнение `AccessFineLocation` для разрешения времени выполнения. Если разрешение не предоставлено, запрос разрешения выполняется путем вызова метода `RequestPermissions`.

1. В классе `MainActivity` добавьте следующее переопределение `OnRequestPermissionsResult`.

    ```csharp
    public override void OnRequestPermissionsResult(int requestCode, string[] permissions, [GeneratedEnum] Permission[] grantResults)
    {
        if (requestCode == RequestLocationId)
        {
            if ((grantResults.Length == 1) && (grantResults[0] == (int)Permission.Granted))
                // Permissions granted - display a message.
            else
                // Permissions denied - display a message.
        }
        else
        {
            base.OnRequestPermissionsResult(requestCode, permissions, grantResults);
        }
    }
    ```

    Это переопределение обрабатывает результат запроса разрешения.

Общий результат этого кода заключается в том, что когда приложение запрашивает расположение пользователя, отображается следующее диалоговое окно, в котором запрашиваются разрешения:

[![Снимок экрана запроса на разрешение расположения на устройстве Android](setup-images/permission-android.png "Запрос разрешения Android")](setup-images/permission-android-large.png#lightbox "Запрос разрешения Android")

### <a name="universal-windows-platform"></a>Универсальная платформа Windows

В UWP приложение должно пройти проверку подлинности, прежде чем оно сможет отобразить карту и использовать службы Map Services. Для проверки подлинности приложения необходимо указать ключ проверки подлинности карты. Дополнительные сведения см. [в разделе запрос на сопоставление ключа проверки подлинности](/windows/uwp/maps-and-location/authentication-key). После этого маркер проверки подлинности должен быть указан в вызове метода `FormsMaps.Init("AUTHORIZATION_TOKEN")` для проверки подлинности приложения с помощью карт Bing.

Кроме того, если приложению требуется доступ к расположению пользователя, необходимо включить возможность расположения в манифесте пакета. Это можно обеспечить, выполнив следующие действия.

1. В **Обозреватель решений**дважды щелкните **Package. appxmanifest** и перейдите на вкладку **возможности** .
1. В списке **возможностей** установите флажок **Расположение**. Это позволит добавить возможность `location` устройства в файл манифеста пакета.

    ```xml
    <Capabilities>
      <!-- DeviceCapability elements must follow Capability elements (if present) -->
      <DeviceCapability Name="location"/>
    </Capabilities>
    ```

## <a name="related-links"></a>Связанные ссылки

- [Пример Maps](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)
- [Контакты Xamarin. Forms. Maps](~/xamarin-forms/user-interface/map/pins.md).
- [API карт](xref:Xamarin.Forms.Maps)
- [Преобразование пользовательского модуля подготовки отчетов](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)
