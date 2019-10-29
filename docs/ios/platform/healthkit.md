---
title: HealthKit в Xamarin. iOS
description: В этом документе описывается HealthKit, платформа, появившаяся в iOS 8, которая предоставляет централизованное, координированное и безопасное хранилище данных для информации, связанной с работоспособностью. В нем обсуждается, как подготовить приложение HealthKit и как написать код, использующий платформу HealthKit.
ms.prod: xamarin
ms.assetid: E3927A21-507C-43BA-A2AD-957716BA9B52
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/19/2017
ms.openlocfilehash: 21f10c7771e1c30eabb3f42a161c6d563a5327f3
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032387"
---
# <a name="healthkit-in-xamarinios"></a>HealthKit в Xamarin. iOS

Пакет Health Kit предоставляет безопасное хранилище данных для сведений, связанных с работоспособностью пользователя. Приложения пакета работоспособности могут с явно указанными разрешениями пользователя читать и записывать данные в это хранилище данных и получать уведомления при добавлении соответствующих данных. Приложения могут представлять данные, или пользователь может использовать предоставленное вами приложение работоспособности Apple для просмотра панели мониторинга всех своих данных.

Так как данные о работоспособности настолько важны и являются критически важными, набор работоспособности строго типизирован с единицами измерения и явной связью с типом записываемой информации (например, в крови глюкометры уровня или частоте сердца). Кроме того, приложения набора работоспособности должны использовать явные права доступа, должны запрашивать доступ к определенным типам информации, и пользователь должен явно предоставить приложению доступ к этим типам данных.

В этой статье мы рассмотрим следующее:

- Требования к безопасности набора исправности, включая подготовку приложений и запрос пользователя на доступ к базе данных набора работоспособности;
- Система типов набора исправности, которая позволяет максимально сокращать вероятность неправильного применения или неправильной интерпретации данных;
- Запись в общее хранилище данных комплекта работоспособности на уровне системы.

В этой статье не рассматриваются более сложные темы, такие как запрос к базе данных, преобразование единиц измерения или получение уведомлений о новых данных.

В этой статье мы создадим пример приложения для записи темпа сердца пользователя:

[![](healthkit-images/image01.png "A sample application to record the users heart rate")](healthkit-images/image01.png#lightbox)

## <a name="requirements"></a>Требования

Для выполнения действий, описанных в этой статье, необходимо выполнить следующие действия.

- **Xcode 7 и iOS 8 (или более поздней версии)** . на компьютере разработчика должны быть установлены и настроены последние API-интерфейсы Apple Xcode и iOS.
- **Visual Studio для Mac или Visual Studio** — последняя версия Visual Studio для Mac должна быть установлена и настроена на компьютере разработчика.
- **устройство iOS 8 (или более поздней версии)** — устройство iOS с последней версией iOS 8 или более поздней для тестирования.

> [!IMPORTANT]
> Пакет работоспособности появился в iOS 8. В настоящее время комплект работоспособности недоступен в симуляторе iOS, а для отладки требуется подключение к физическому устройству iOS.

## <a name="creating-and-provisioning-a-health-kit-app"></a>Создание и подготовка приложения набора работоспособности
Прежде чем приложение Xamarin iOS 8 сможет использовать API HealthKit, оно должно быть правильно настроено и подготовлено. В этом разделе рассматриваются шаги, необходимые для правильной настройки приложения Xamarin.

Для приложений набора работоспособности требуется:

- Явный **идентификатор приложения**.
- **Профиль подготовки** , связанный с этим явным **идентификатором приложения** и с разрешениями **Health Kit** .
- `Entitlements.plist` со свойством `com.apple.developer.healthkit` типа `Boolean` установленным в значение `Yes`.
- `Info.plist`, ключ `UIRequiredDeviceCapabilities` которого содержит запись со значением `String` `healthkit`.
- `Info.plist` также должны иметь соответствующие записи с пояснениями о конфиденциальности: `String` пояснения для ключа `NSHealthUpdateUsageDescription`, если приложение будет записывать данные, и `String` пояснения для ключа `NSHealthShareUsageDescription`, если приложение собирается считать данные набора работоспособности.

Чтобы узнать больше о подготовке приложения iOS, в статье [Подготовка устройства](~/ios/get-started/installation/device-provisioning/index.md) в серии **Начало работы** Xamarin описывается связь между сертификатами разработчика, идентификаторами приложений, профилями подготовки и назначениями приложений.

<a name="explicit-appid" />

### <a name="explicit-app-id-and-provisioning-profile"></a>Явный идентификатор приложения и профиль подготовки

Создание явного **идентификатора приложения** и соответствующего **профиля подготовки** выполняются в [центре разработки iOS](https://developer.apple.com/devcenter/ios/index.action)Apple. 

Текущие **Идентификаторы приложений** перечислены в разделе [сертификаты, идентификаторы & профилей](https://developer.apple.com/account/ios/identifiers/bundle/bundleList.action) в центре разработки. Часто в этом списке отображаются значения **идентификаторов** `*`, указывающие, что **идентификатор приложения** - **имя** может использоваться с любым количеством суффиксов. Такие *идентификаторы приложения с подстановочными знаками* нельзя использовать с набором работоспособности.

Чтобы создать явный **идентификатор приложения**, нажмите кнопку **+** в правом верхнем углу, чтобы перейти на страницу **Регистрация идентификатора приложения для iOS** .

[![](healthkit-images/image02.png "Registering an app on the Apple Developer Portal")](healthkit-images/image02.png#lightbox)

Как показано на рисунке выше, после создания описания приложения используйте раздел **ЯВНЫЙ идентификатор приложения** , чтобы создать идентификатор для приложения. В разделе **службы приложений** проверьте **набор работоспособности** в разделе **Включение служб** .

По завершении нажмите кнопку Continue ( **продолжить** ), чтобы зарегистрировать **идентификатор приложения** в вашей учетной записи. Будет возвращена страница **сертификаты, идентификаторы и профили** . Щелкните **профили подготовки** , чтобы перейти к списку текущих профилей подготовки, и нажмите кнопку **+** в правом верхнем углу, чтобы перейти на страницу **Добавление профиля подготовки iOS** . Выберите параметр **Разработка приложений iOS** и нажмите кнопку **продолжить** , чтобы перейти на страницу **Выбор идентификатора приложения** . Выберите явно указанный **идентификатор приложения** , который вы указали ранее:

[![](healthkit-images/image03.png "Select the explicit App ID")](healthkit-images/image03.png#lightbox)

Нажмите кнопку **продолжить** и пройдите по оставшимся экранам, где вы укажете **сертификаты разработчика**, **устройства**и **имя** для этого **профиля подготовки**:

[![](healthkit-images/image04.png "Generating the Provisioning Profile")](healthkit-images/image04.png#lightbox)

Щелкните **создать** и дождаться создания профиля. Скачайте файл и дважды щелкните его, чтобы установить в Xcode. Вы можете подтвердить установку в разделе **Xcode > Preferences > accounts > View Details...** Вы должны увидеть только что установленный профиль подготовки, и он должен иметь значок для набора работоспособности и других специальных служб **в строке прав** :

[![](healthkit-images/image05.png "Viewing the profile in Xcode")](healthkit-images/image05.png#lightbox)

<a name="associating-appid" />

### <a name="associating-the-app-id-and-provisioning-profile-with-your-xamarinios-app"></a>Сопоставление идентификатора приложения и профиля подготовки с приложением Xamarin. iOS

После создания и установки соответствующего **профиля подготовки** , как описано выше, обычно пришло время создать решение в Visual Studio для Mac или Visual Studio. Доступ к набору работоспособности доступен для C# любого F# проекта или iOS.

Вместо того чтобы пополнить процесс создания проекта Xamarin iOS 8 вручную, откройте пример приложения, присоединенного к этой статье (в том числе предварительно построенную раскадровку и код). Чтобы связать пример приложения с **профилем подготовки**с включенным набором работоспособности, в **панель решения**щелкните правой кнопкой мыши проект и откройте диалоговое окно " **Параметры** ". Перейдите на панель **приложения iOS** и введите явный **идентификатор приложения** , созданный ранее в качестве **идентификатора пакета**приложения:

[![](healthkit-images/image06.png "Enter the explicit App ID")](healthkit-images/image06.png#lightbox)

Теперь переключитесь на панель **подписывания пакета iOS** . Недавно установленный **профиль подготовки**, имеющий связь с явным **идентификатором приложения**, теперь будет доступен в качестве **профиля подготовки**:

[![](healthkit-images/image07.png "Select the Provisioning Profile")](healthkit-images/image07.png#lightbox)

Если **профиль подготовки** недоступен, дважды проверьте **идентификатор пакета** на панели **приложений iOS** , в противном случае — в **центре разработки iOS** , а также в том, что **профиль подготовки** установлен (**Xcode > Предпочтения > учетные записи > Просмотр сведений...** ).

Если выбран **профиль подготовки** с включенным набором работоспособности, нажмите кнопку **ОК** , чтобы закрыть диалоговое окно Параметры проекта.

### <a name="entitlementsplist-and-infoplist-values"></a>Значения прав. plist и info. plist

Пример приложения включает файл `Entitlements.plist` (который необходим для приложений с поддержкой пакета работоспособности) и не включается в каждый шаблон проекта. Если проект не содержит прав, щелкните правой кнопкой мыши проект и выберите **файл > создать файл... > iOS > прав. plist** добавить его вручную.

В конечном счете, ваша `Entitlements.plist` должна иметь следующую пару "ключ — значение":

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>com.apple.developer.HealthKit</key>
    <true/>
</dict>
</plist>

```

Аналогичным образом `Info.plist` для приложения должно иметь значение `healthkit` связанное с ключом `UIRequiredDeviceCapabilities`:

```xml
<key>UIRequiredDeviceCapabilities</key>
<array>
<string>armv7</string>
    <string>healthkit</string>
</array>

```

Пример приложения, приведенный в этой статье, включает предварительно настроенные `Entitlements.plist`, включающие все необходимые ключи.

<a name="programming" />

## <a name="programming-health-kit"></a>Набор средств для определения работоспособности программ

Хранилище данных набора работоспособности — это закрытое хранилище данных конкретного пользователя, которое является общим для различных приложений. Так как сведения о работоспособности конфиденциальны, пользователь должен принять положительные действия, чтобы разрешить доступ к данным. Этот доступ может быть частичным (запись, но не чтение, доступ к некоторым типам данных, но не другим и т. д.) и может быть отозван в любое время. Приложения комплекта исправности должны быть написаны с учетом того, что многие пользователи будут сомневаются о хранении информации, связанной с их работоспособностью.

Данные набора работоспособности ограничены указанными в Apple типами. Эти типы строго определены: некоторые, например тип крови, ограничиваются определенными значениями перечисления, предоставляемого компанией Apple, а другие сочетают величину с единицей измерения (например, граммы, калориес и литры). Даже данные, совместно использующие совместимую единицу измерения, различаются их `HKObjectType`; Например, система типов будет перехватывать ошибочную попытку сохранить `HKQuantityTypeIdentifier.NumberOfTimesFallen` значение в поле, ожидающем `HKQuantityTypeIdentifier.FlightsClimbed` несмотря на то, что в обоих случаях используется `HKUnit.Count` единица измерения.

Типы который можно поместить в хранилище данных набора работоспособности — это все подклассы `HKObjectType`. `HKCharacteristicType` объекты хранят биологических пол, тип крови и дату рождения. Чаще всего используются `HKSampleType` объекты, представляющие данные, которые вычисляются в определенное время или в течение определенного периода времени. 

[![](healthkit-images/image08.png "HKSampleType objects chart")](healthkit-images/image08.png#lightbox)

`HKSampleType` является абстрактным и имеет четыре конкретных подкласса. В настоящее время существует только один тип `HKCategoryType` данных, который является анализом спящего режима. Большая часть данных в пакете работоспособности имеет тип `HKQuantityType` и хранить их в `HKQuantitySample` объектах, созданных с помощью привычного шаблона проектирования фабрики:

[![](healthkit-images/image09.png "The large majority of data in Health Kit are of type HKQuantityType and store their data in HKQuantitySample objects")](healthkit-images/image09.png#lightbox)

`HKQuantityType` типы в диапазоне от `HKQuantityTypeIdentifier.ActiveEnergyBurned` до `HKQuantityTypeIdentifier.StepCount`. 

<a name="requesting-permission" />

### <a name="requesting-permission-from-the-user"></a>Запрос разрешения у пользователя

Конечные пользователи должны предпринять положительные действия, чтобы разрешить приложению читать или записывать данные набора работоспособности. Это выполняется с помощью приложения работоспособности, которое предварительно установлено на устройствах iOS 8. При первом запуске приложения набора работоспособности пользователю предоставляется диалоговое окно для **доступа к работоспособности** , управляемое системой:

[![](healthkit-images/image10.png "The user is presented with a system-controlled Health Access dialog")](healthkit-images/image10.png#lightbox)

Позже пользователь может изменить разрешения с помощью диалогового окна **источники** приложения работоспособности:

[![](healthkit-images/image11.png "The user can change permissions using Health apps Sources dialog")](healthkit-images/image11.png#lightbox)

Поскольку информация о работоспособности чрезвычайно важна, разработчики приложений должны обеспечить защитную программу, предоставляя возможность отказа и изменения разрешений во время работы приложения. Наиболее распространенной идиомой является запрос разрешений в методе `UIApplicationDelegate.OnActivated`, а затем изменение пользовательского интерфейса соответствующим образом.

### <a name="permissions-walkthrough"></a>Руководство по разрешениям

В проекте, подготовленном для набора работоспособности, откройте файл `AppDelegate.cs`. Обратите внимание на инструкцию, используя `HealthKit`; в верхней части файла.

Следующий код относится к разрешениям для набора прав:

```csharp
private HKHealthStore healthKitStore = new HKHealthStore ();

public override void OnActivated (UIApplication application)
{
        ValidateAuthorization ();
}

private void ValidateAuthorization ()
{
        var heartRateId = HKQuantityTypeIdentifierKey.HeartRate;
        var heartRateType = HKObjectType.GetQuantityType (heartRateId);
        var typesToWrite = new NSSet (new [] { heartRateType });
        var typesToRead = new NSSet ();
        healthKitStore.RequestAuthorizationToShare (
                typesToWrite, 
                typesToRead, 
                ReactToHealthCarePermissions);
}

void ReactToHealthCarePermissions (bool success, NSError error)
{
        var access = healthKitStore.GetAuthorizationStatus (HKObjectType.GetQuantityType (HKQuantityTypeIdentifierKey.HeartRate));
        if (access.HasFlag (HKAuthorizationStatus.SharingAuthorized)) {
                HeartRateModel.Instance.Enabled = true;
        } else {
                HeartRateModel.Instance.Enabled = false;
        }
}

```

Весь код в этих методах можно сделать встроенным в `OnActivated`, но пример приложения использует отдельные методы, чтобы сделать их более четкими: `ValidateAuthorization()` содержит шаги, необходимые для запроса доступа к заданным типам (и для чтения, если требуется приложение) и `ReactToHealthCarePermissions()` — Это обратный вызов, который активируется после взаимодействия пользователя с диалоговым окном разрешений в Health. app.

Задача `ValidateAuthorization()` состоит в том, чтобы создать набор `HKObjectTypes`, который будет записываться приложением и запросить авторизацию для обновления этих данных. В примере приложения `HKObjectType` для ключа `KHQuantityTypeIdentifierKey.HeartRate`. Этот тип добавляется в набор `typesToWrite`, в то время как набор `typesToRead` остается пустым. Эти наборы и ссылка на обратный вызов `ReactToHealthCarePermissions()` передается в `HKHealthStore.RequestAuthorizationToShare()`.

Обратный вызов `ReactToHealthCarePermissions()` будет вызван после того, как пользователь взаимодействовал с диалоговым окном разрешений и передавал два фрагмента информации: значение `bool`, которое будет `true`, если пользователь взаимодействовал с диалоговым окном разрешений и `NSError` Если значение, отличное от NULL, указывает на ошибку, связанную с представлением диалогового окна разрешений.

> [!IMPORTANT]
> Чтобы быть ясным о аргументах этой функции: параметры _Success_ и _Error_ не указывают, предоставил ли пользователь разрешение на доступ к данным набора работоспособности. Они только указывают, что пользователю предоставлена возможность разрешения доступа к данным.

Чтобы проверить, имеет ли приложение доступ к данным, используется `HKHealthStore.GetAuthorizationStatus()`, передав `HKQuantityTypeIdentifierKey.HeartRate`. В зависимости от возвращенного состояния приложение включает или отключает возможность ввода данных. Нет стандартного интерфейса пользователя для работы с отказом в доступе, и существует множество возможных вариантов. В примере приложения состояние задается для `HeartRateModel` одноэлементного объекта, который, в свою очередь, вызывает соответствующие события.

## <a name="model-view-and-controller"></a>Модель, представление и контроллер

Чтобы проверить объект `HeartRateModel` Singleton, откройте файл `HeartRateModel.cs`:

```csharp
using System;
using HealthKit;
using Foundation;

namespace HKWork
{
        public class GenericEventArgs<T> : EventArgs
        {
                public T Value { get; protected set; }
                public DateTime Time { get; protected set; }

                public GenericEventArgs (T value)
                {
                        this.Value = value;
                        Time = DateTime.Now;
                }
        }

        public delegate void GenericEventHandler<T> (object sender,GenericEventArgs<T> args);

        public sealed class HeartRateModel : NSObject
        {
                private static volatile HeartRateModel singleton;
                private static object syncRoot = new Object ();

                private HeartRateModel ()
                {
                }

                public static HeartRateModel Instance {
                        get {
                                //Double-check lazy initialization
                                if (singleton == null) {
                                        lock (syncRoot) {
                                                if (singleton == null) {
                                                        singleton = new HeartRateModel ();
                                                }
                                        }
                                }

                                return singleton;
                        }
                }

                private bool enabled = false;

                public event GenericEventHandler<bool> EnabledChanged;
                public event GenericEventHandler<String> ErrorMessageChanged;
                public event GenericEventHandler<Double> HeartRateStored;

                public bool Enabled { 
                        get { return enabled; }
                        set {
                                if (enabled != value) {
                                        enabled = value;
                                        InvokeOnMainThread(() => EnabledChanged (this, new GenericEventArgs<bool>(value)));
                                }
                        }
                }

                public void PermissionsError(string msg)
                {
                        Enabled = false;
                        InvokeOnMainThread(() => ErrorMessageChanged (this, new GenericEventArgs<string>(msg)));
                }

                //Converts its argument into a strongly-typed quantity representing the value in beats-per-minute
                public HKQuantity HeartRateInBeatsPerMinute(ushort beatsPerMinute)
                {
                        var heartRateUnitType = HKUnit.Count.UnitDividedBy (HKUnit.Minute);
                        var quantity = HKQuantity.FromQuantity (heartRateUnitType, beatsPerMinute);

                        return quantity;
                }
                        
                public void StoreHeartRate(HKQuantity quantity)
                {
                        var bpm = HKUnit.Count.UnitDividedBy (HKUnit.Minute);
                        //Confirm that the value passed in is of a valid type (can be converted to beats-per-minute)
                        if (! quantity.IsCompatible(bpm))
                        {
                                InvokeOnMainThread(() => ErrorMessageChanged(this, new GenericEventArgs<string> ("Units must be compatible with BPM")));
                        }

                        var heartRateId = HKQuantityTypeIdentifierKey.HeartRate;
                        var heartRateQuantityType = HKQuantityType.GetQuantityType (heartRateId);
                        var heartRateSample = HKQuantitySample.FromType (heartRateQuantityType, quantity, new NSDate (), new NSDate (), new HKMetadata());

                        using (var healthKitStore = new HKHealthStore ()) {
                                healthKitStore.SaveObject (heartRateSample, (success, error) => {
                                        InvokeOnMainThread (() => {
                                                if (success) {
                                                        HeartRateStored(this, new GenericEventArgs<Double>(quantity.GetDoubleValue(bpm)));
                                                } else {
                                                        ErrorMessageChanged(this, new GenericEventArgs<string>("Save failed"));
                                                }
                                                if (error != null) {
                                                        //If there's some kind of error, disable 
                                                        Enabled = false;
                                                        ErrorMessageChanged (this, new GenericEventArgs<string>(error.ToString()));
                                                }
                                        });
                                });
                        }
                }
        }
}

```

Первый раздел — это стандартный код для создания универсальных событий и обработчиков. Начальная часть класса `HeartRateModel` также является шаблоном для создания потокобезопасного одноэлементного объекта Singleton.

Затем `HeartRateModel` предоставляет 3 события: 

- `EnabledChanged` — указывает, что хранилище с частотой сердца включено или отключено (Обратите внимание, что хранилище изначально отключено). 
- `ErrorMessageChanged` — для этого примера приложения у нас есть очень простая модель обработки ошибок: строка с последней ошибкой. 
- `HeartRateStored` — возникает при хранении темпа в базе данных набора работоспособности.

Обратите внимание, что при каждом запуске этих событий выполняется с помощью `NSObject.InvokeOnMainThread()`, что позволяет подписчикам обновлять пользовательский интерфейс. Кроме того, события могут задокументированы в фоновых потоках, и ответственность за обеспечение совместимости может быть оставлена для их обработчиков. Вопросы о потоках важны в приложениях пакета работоспособности, так как многие из функций, таких как запрос разрешений, являются асинхронными и выполняют обратные вызовы для потоков, не являющихся основными.

Специфичный для набора работоспособности код в `HeartRateModel` состоит из двух функций `HeartRateInBeatsPerMinute()` и `StoreHeartRate()`. 

`HeartRateInBeatsPerMinute()` преобразует свой аргумент в строго типизированный набор работоспособности `HKQuantity`. Тип количества определяется `HKQuantityTypeIdentifierKey.HeartRate`, а единицы количества `HKUnit.Count` делятся на `HKUnit.Minute` (иными словами, единица *перебивается в минуту*). 

Функция `StoreHeartRate()` принимает `HKQuantity` (в примере приложения, созданном `HeartRateInBeatsPerMinute()`). Для проверки данных используется метод `HKQuantity.IsCompatible()`, который возвращает `true`, если единицы объекта можно преобразовать в единицы в аргументе. Если количество было создано с помощью `HeartRateInBeatsPerMinute()` это, очевидно, будет возвращать `true`, но оно также будет возвращать `true`, если количество было создано как, например, в *час*. Чаще `HKQuantity.IsCompatible()` можно использовать для проверки массы, расстояния и энергии, которые пользователь или устройство могут вводить или отображать в одной системе измерения (например, германской Units), но они могут храниться в другой системе (например, в единицах метрик). 

После проверки совместимости количества используется метод `HKQuantitySample.FromType()` фабрики для создания строго типизированного `heartRateSample` объекта. `HKSample` объекты имеют дату начала и окончания; для мгновенного считывания эти значения должны быть одинаковыми, как в примере. В этом примере также не устанавливаются никакие данные ключа-значения в аргументе `HKMetadata`, но для указания расположения датчика можно использовать следующий код:

```csharp
var hkm = new HKMetadata();
hkm.HeartRateSensorLocation = HKHeartRateSensorLocation.Chest;

```

После создания `heartRateSample` код создает новое соединение с базой данных с помощью блока using. В рамках этого блока метод `HKHealthStore.SaveObject()` пытается выполнить асинхронную запись в базу данных. Результирующий вызов лямбда-выражения вызывает соответствующие события: `HeartRateStored` или `ErrorMessageChanged`.

Теперь, когда модель была запрограммирована, пришло время увидеть, как контроллер отражает состояние модели. Откройте файл `HKWorkViewController.cs`. Конструктор просто подсоединяет `HeartRateModel` Singleton к методам обработки событий (опять же, это можно сделать с помощью лямбда-выражений, но отдельные методы делают намерение более очевидным):

```csharp
public HKWorkViewController (IntPtr handle) : base (handle)
{
     HeartRateModel.Instance.EnabledChanged += OnEnabledChanged;
     HeartRateModel.Instance.ErrorMessageChanged += OnErrorMessageChanged;
     HeartRateModel.Instance.HeartRateStored += OnHeartBeatStored;
}

```

Ниже приведены соответствующие обработчики:

```csharp
void OnEnabledChanged (object sender, GenericEventArgs<bool> args)
{
        StoreData.Enabled = args.Value;
        PermissionsLabel.Text = args.Value ? "Ready to record" : "Not authorized to store data.";
        PermissionsLabel.SizeToFit ();
}

void OnErrorMessageChanged (object sender, GenericEventArgs<string> args)
{
        PermissionsLabel.Text = args.Value;
}

void OnHeartBeatStored (object sender, GenericEventArgs<double> args)
{
        PermissionsLabel.Text = String.Format ("Stored {0} BPM", args.Value);
}

```

Очевидно, что в приложении с одним контроллером было бы возможным избежать создания отдельного объекта модели и использования событий для потока управления, но использование объектов модели более подходит для реальных приложений.

## <a name="running-the-sample-app"></a>Запуск примера приложения

Симулятор iOS не поддерживает пакет работоспособности. Отладка должна выполняться на физическом устройстве под управлением iOS 8.

Подключите правильно подготовленное устройство разработки iOS 8 к системе. Выберите его в качестве целевого объекта развертывания в Visual Studio для Mac а затем в меню выберите **выполнить > отладки**.

> [!IMPORTANT]
> На этом этапе будут отображаться ошибки, связанные с подготовкой. Чтобы устранить ошибки, ознакомьтесь с разделом Создание и подготовка приложения для набора работоспособности выше. Компоненты: 
>
> - **центр разработки iOS** — явный идентификатор приложения, в & включен профиль подготовки. 
> - **Параметры проекта** — идентификатор пакета (явный идентификатор приложения) & профиль подготовки.
> - **Исходный код** — права. plist & info. plist

При условии, что подготовка настроена правильно, приложение запустится. При достижении метода `OnActivated` будет запрошена авторизация для набора прав. В первый раз, когда эта операционная система обнаруживает, пользователь будет представлен в следующем диалоговом окне:

[![](healthkit-images/image12.png "The user will be presented with this dialog")](healthkit-images/image12.png#lightbox)

Разрешите приложению обновлять данные о частоте сердца, и ваше приложение появится снова. Обратный вызов `ReactToHealthCarePermissions` будет активирован асинхронно. Это приведет к изменению свойства `HeartRateModel’s` `Enabled`, которое вызовет событие `EnabledChanged`, которое приведет к запуску обработчика событий `HKPermissionsViewController.OnEnabledChanged()`, что активирует кнопку `StoreData`. Последовательность показана на следующей схеме:

[![](healthkit-images/image13.png "This diagram shows the sequence of events")](healthkit-images/image13.png#lightbox)

Нажмите кнопку **запись** . Это приведет к запуску обработчика `StoreData_TouchUpInside()`, который будет пытаться проанализировать значение текстового поля `heartRate`, преобразовать в `HKQuantity` с помощью ранее обсуждаемой функции `HeartRateModel.HeartRateInBeatsPerMinute()` и передать это количество в `HeartRateModel.StoreHeartRate()`. Как обсуждалось ранее, будет предпринята попытка сохранить данные и будет вызвано событие `HeartRateStored` или `ErrorMessageChanged`.

Дважды щелкните кнопку " **Главная** " на устройстве и откройте приложение "работоспособность". Перейдите на вкладку **источники** , и вы увидите пример приложения в списке. Выберите его и запретите разрешение на обновление данных частоты сердца. Дважды щелкните кнопку **Главная** и вернитесь в приложение. Опять же, `ReactToHealthCarePermissions()` будет вызываться, но на этот раз, так как в доступе отказано, кнопка **storeData** будет отключена (Обратите внимание, что это происходит асинхронно, и изменение пользовательского интерфейса может быть видимо конечному пользователю).

## <a name="advanced-topics"></a>Дополнительные разделы

Чтение данных из базы данных комплекта работоспособности очень похоже на запись данных: одна из них указывает типы данных, к которым пытается получить доступ, запросы авторизации и, если авторизация предоставлена, данные доступны, с автоматическим преобразованием в совместимые единицы мер.

Существует ряд более сложных функций запросов, которые позволяют выполнять запросы и запросы на основе предикатов, которые выполняют обновления при обновлении актуальных данных. 

Разработчикам приложений из комплекта работоспособности следует ознакомиться с разделом «пакет работоспособности» [руководства по проверке приложений](https://developer.apple.com/app-store/review/guidelines/#healthkit)Apple.

После понимания моделей безопасности и системных типов хранение и чтение данных в базе данных общего набора работоспособности выполняется довольно просто. Многие функции в наборе работоспособности работают асинхронно, и разработчики приложений должны правильно писать свои программы.

На момент написания этой статьи в Android или Windows Phone нет эквивалента комплекта Health.

## <a name="summary"></a>Сводка

В этой статье мы рассмотрели, как пакет работоспособности позволяет приложениям хранить, извлекать и совместно использовать информацию о работоспособности, а также предоставляет стандартное приложение работоспособности, которое позволяет пользователям получать доступ к этим данным и управлять ими. 

Мы также показали, как конфиденциальность, безопасность и целостность данных переопределяют проблемы, связанные со сведениями о работоспособности, и приложения, использующие пакет работоспособности, должны справляться с увеличением сложности в аспектах управления приложениями (подготовка), написании кода (тип набора исправности System) и взаимодействие с пользователем (пользовательский контроль разрешений через системные диалоговые окна и приложение работоспособности). 

Наконец, мы рассмотрим простую реализацию комплекта исправности с помощью прилагаемого примера приложения, записывающего данные пульса в хранилище комплекта работоспособности и имеющего асинхронно-ориентированный проект.

## <a name="related-links"></a>Связанные ссылки

- [Хкворк (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios8-introtohealthkit)
- [Введение в iOS 8](~/ios/platform/introduction-to-ios8.md)
