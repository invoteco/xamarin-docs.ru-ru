---
title: Общие сведения о главе 28. Расположение и карты
description: 'Создание мобильных приложений с помощью Xamarin. Forms: сводка в главе 28. Расположение и карты'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F6E20077-687C-45C4-A375-31D4F49BBFA4
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
ms.openlocfilehash: 5dcd84536cc6d80deb753fc6fe57f9090f6b2dad
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/21/2019
ms.locfileid: "72697079"
---
# <a name="summary-of-chapter-28-location-and-maps"></a>Общие сведения о главе 28. Расположение и карты

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28)

> [!NOTE]
> Примечания на этой странице указывают области, в которых Xamarin. Forms рассходится от материала, представленного в книге.

Xamarin. Forms поддерживает элемент [`Map`](xref:Xamarin.Forms.Maps.Map) , производный от `View`. Из-за специальных требований к платформе, связанных с использованием карт, они реализуются в отдельной сборке, **Xamarin. Forms. Maps**и используют другое пространство имен: `Xamarin.Forms.Maps`.

## <a name="the-geographic-coordinate-system"></a>Географическая система координат

Географическая система координат определяет позиции в сферном (или почти сферическим) объекте, таком как земля. Координата состоит из *широты* и *долготы* , выраженных в угловых углами.

Большой круг, именуемый `equator`, находится в середине между двумя полюсам, через которые ось Земли концептуально расширяется.

### <a name="parallels-and-latitude"></a>Параллели и Широта

Угол, измеряемый Севером или Юг от деления от центра земли, отмечает строки одинаковой широты, называемые *параллельными*. Это диапазон от 0 градусов на равенство до 90 градусов в Северной и Южной полюсам. В соответствии с соглашением, северы в ко483 Азии являются положительными значениями, а эти значения являются отрицательными.

### <a name="longitude-and-meridians"></a>Долгота и меридианы

Половины больших кругов от Северного полюса до Южной полюса — это строки равной долготе, также известные как *меридианы*. Они зависят от самого простого Меридианова по Гринвичу, Англия. По соглашению, Долгота восточной части Меридианова — положительные значения от 0 до 180 градусов, а Долгота — Западная часть самого простого меридианов — отрицательные значения от 0 градусов до &ndash;180 градусов.

### <a name="the-equirectangular-projection"></a>Проекция равнопромежуточная

Любая неструктурированная схема земли вводит искажения. Если все строки широты и долготы являются прямыми, и если одинаковые различия в столбцах широты и долготы соответствуют равным расстояниям на карте, то результатом будет *равнопромежуточная проекция*. Эта схема искажает области ближе к полюсам, так как они растягиваются горизонтально.

### <a name="the-mercator-projection"></a>Проекция Меркатора

Популярная *проекция Меркатора* пытается компенсировать Горизонтальное растяжение, также растягивая эти области по вертикали. Это приводит к отображению на карте, где области, близкие к полюсам, выглядят гораздо больше, чем на самом деле, но все локальные области очень тесно соответствуют реальной области.

### <a name="map-services-and-tiles"></a>Сопоставленные службы и плитки

Службы Map используют разновидность проекции Меркатора, именуемую `Web Mercator`. Службы Map доставляют плитки точечного рисунка клиенту на основе расположения и масштаба.

## <a name="getting-the-users-location"></a>Получение расположения пользователя

Классы `Map` Xamarin. Forms не включают средство для получения географического расположения пользователя, но это часто желательно при работе с картами, поэтому служба зависимостей должна ее справиться.

> [!NOTE]
> Вместо этого приложения Xamarin. Forms могут использовать класс [`Geolocation`](~/essentials/geolocation.md) , входящий в Xamarin. Essentials.

### <a name="the-location-tracker-api"></a>API инспектора расположения

Решение [**Xamarin. формсбук. Platform**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform) содержит код для API-интерфейса Tracker Location. Структура [`GeographicLocation`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/GeographicLocation.cs) инкапсулирует широту и долготу. Интерфейс [`ILocationTracker`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/ILocationTracker.cs) определяет два метода запуска и приостановки инспектора расположения и событие, когда доступно новое расположение.

#### <a name="the-ios-location-manager"></a>Диспетчер расположения iOS

Реализация `ILocationTracker` для iOS — это [`LocationTracker`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/LocationTracker.cs) класс, который использует [`CLLocationManager`](xref:CoreLocation.CLLocationManager)iOS.

#### <a name="the-android-location-manager"></a>Диспетчер расположения Android

Реализация `ILocationTracker` Android — это [`LocationTracker`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/LocationTracker.cs) класс, который использует класс [`LocationManager`](xref:Android.Locations.LocationManager) Android.

#### <a name="the-uwp-geo-locator"></a>Географический локатор UWP

Универсальная платформа Windows реализация `ILocationTracker` является классом [`LocationTracker`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/LocationTracker.cs) , который использует [`Geolocator`](/uwp/api/Windows.Devices.Geolocation.Geolocator)UWP.

### <a name="display-the-phones-location"></a>Отображение расположения телефона

В примере [**вхереами**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28/WhereAmI) используется средство прослушивания местоположения для отображения расположения телефона как в тексте, так и на карте Равнопромежуточная.

### <a name="the-required-overhead"></a>Требуемые издержки

Для использования инспектора местоположения **вхереами** требуется дополнительная нагрузка. Во первых, все проекты в решении **вхереами** должны иметь ссылки на соответствующие проекты в **Xamarin. формсбук. Platform**, а каждый проект **вхереами** должен вызывать метод `Toolkit.Init`.

Некоторые дополнительные издержки, связанные с конкретной платформой, являются обязательными в форме разрешений расположения.

#### <a name="location-permission-for-ios"></a>Разрешение расположения для iOS

Для iOS файл **info. plist** должен содержать элементы, содержащие текст вопроса, предлагающего пользователю разрешить получение расположения этого пользователя.

#### <a name="location-permissions-for-android"></a>Разрешения расположения для Android

Приложения Android, которые получают расположение пользователя, должны иметь разрешение ACCESS_FILE_LOCATION в файле AndroidManifest. XML.

#### <a name="location-permissions-for-the-uwp"></a>Разрешения расположения для UWP

Универсальная платформа Windows приложение должно иметь возможность `location` устройства, помеченная в файле Package. appxmanifest.

## <a name="working-with-xamarinformsmaps"></a>Работа с Xamarin. Forms. Maps

Использование класса `Map` участвует в нескольких требованиях.

### <a name="the-nuget-package"></a>Пакет NuGet

Библиотека NuGet **Xamarin. Forms. Maps** должна быть добавлена в решение приложения. Номер версии должен быть таким же, как и установленный в настоящее время пакет **Xamarin. Forms** .

### <a name="initializing-the-maps-package"></a>Инициализация пакета Maps

Проекты приложения должны вызывать метод `Xamarin.FormsMaps.Init` после совершения вызова `Xamarin.Forms.Forms.Init`.

### <a name="enabling-map-services"></a>Включение служб Map

Поскольку `Map` может получить расположение пользователя, приложение должно получить разрешение для пользователя в соответствии с описанным выше образом в этой главе:

#### <a name="enabling-ios-maps"></a>Включение карт iOS

Приложению iOS, использующему `Map`, требуются две строки в файле info. plist.

#### <a name="enabling-android-maps"></a>Включение карт Android

Ключ авторизации необходим для использования служб Google Map Services. Этот ключ вставляется в файл **AndroidManifest. XML** . Кроме того, файл **AndroidManifest. XML** требует тегов `manifest`, участвующих в получении расположения пользователя.

#### <a name="enabling-uwp-maps"></a>Включение карт UWP

Для приложения универсальная платформа Windows требуется ключ авторизации для использования карт Bing. Этот ключ передается в качестве аргумента в метод `Xamarin.FormsMaps.Init`. Приложение также должно быть включено для служб определения местоположения.

### <a name="the-unadorned-map"></a>Неоформленная схема

Пример [**мапдемос**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28/MapDemos) состоит из файла [мапсдемохомепаже. XAML](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapDemosHomePage.xaml) и [MapsDemoHomePage.XAML.CS](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapDemosHomePage.xaml.cs) файла кода программной части, который позволяет переходить к различным демонстрационным программам.

В файле [басикмаппаже. XAML](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/BasicMapPage.xaml) показано, как отобразить представление [`Map`](xref:Xamarin.Forms.Maps.Map) . По умолчанию отображается город Рим, но пользователь может управлять картой.

Чтобы отключить горизонтальную и вертикальную прокрутку, задайте для свойства [`HasScrollEnabled`](xref:Xamarin.Forms.Maps.Map.HasScrollEnabled) значение `false`. Чтобы отключить масштаб, задайте для параметра [`HasZoomEnabled`](xref:Xamarin.Forms.Maps.Map.HasZoomEnabled) значение `false`. Эти свойства могут не работать на всех платформах.

### <a name="streets-and-terrain"></a>Улиц и ландшафта

Можно отобразить различные типы карт, задав свойство `Map` [`MapType`](xref:Xamarin.Forms.Maps.Map.MapType) типа [`MapType`](xref:Xamarin.Forms.Maps.MapType), перечисление с тремя элементами:

- [`Street`](xref:Xamarin.Forms.Maps.MapType.Street), значение по умолчанию
- [`Satellite`](xref:Xamarin.Forms.Maps.MapType.Satellite)
- [`Hybrid`](xref:Xamarin.Forms.Maps.MapType.Hybrid)

В файле [маптипеспаже. XAML](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapTypesPage.xaml) показано, как использовать переключатель для выбора типа схемы. Он использует класс [`RadioButtonManager`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/RadioButtonManager.cs) в библиотеке [**Xamarin. формсбук. Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) и класс, основанный на файле [маптиперадиобуттон. XAML](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapTypeRadioButton.xaml) .

### <a name="map-coordinates"></a>Координаты карт

Программа может получить текущую область, которую `Map` отобразить с помощью свойства [`VisibleRegion`](xref:Xamarin.Forms.Maps.Map.VisibleRegion) . Это свойство *не* поддерживается связываемым свойством и не существует механизма уведомления, указывающего, когда он изменился, поэтому программа, желающая отслеживать свойство, может использовать таймер для этой цели.

`VisibleRegion` имеет тип [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan), класс с четырьмя свойствами только для чтения:

- [`Center`](xref:Xamarin.Forms.Maps.MapSpan.Center) типа [`Position`](xref:Xamarin.Forms.Maps.Position).
- [`LatitudeDegrees`](xref:Xamarin.Forms.Maps.MapSpan.LatitudeDegrees) типа `double`, указывающее высоту отображаемой области на карте
- [`LongitudeDegrees`](xref:Xamarin.Forms.Maps.MapSpan.LongitudeDegrees) типа `double`, указывающее ширину отображаемой области на карте
- [`Radius`](xref:Xamarin.Forms.Maps.MapSpan.Radius) типа [`Distance`](xref:Xamarin.Forms.Maps.Distance), указывающее размер самой крупной круглой области, видимой на карте

`Position` и `Distance` являются структурами. `Position` определяет два свойства только для чтения, заданные с помощью [конструктора `Position`](xref:Xamarin.Forms.Maps.Position.%23ctor(System.Double,System.Double)):

- [`Latitude`](xref:Xamarin.Forms.Maps.Position.Latitude)
- [`Longitude`](xref:Xamarin.Forms.Maps.Position.Longitude)

`Distance` предназначено для обеспечения независимого от единицы расстояния путем преобразования между единицами измерения и английского уровня. Значение `Distance` можно создать несколькими способами.

- [конструктор `Distance`](xref:Xamarin.Forms.Maps.Distance.%23ctor(System.Double)) с расстоянием в метрах
- [`Distance.FromMeters`](xref:Xamarin.Forms.Maps.Distance.FromMeters(System.Double)) статический метод
- [`Distance.FromKilometers`](xref:Xamarin.Forms.Maps.Distance.FromKilometers(System.Double)) статический метод
- [`Distance.FromMiles`](xref:Xamarin.Forms.Maps.Distance.FromMiles(System.Double)) статический метод

Значение доступно из трех свойств:

- [`Meters`](xref:Xamarin.Forms.Maps.Distance.Meters) типа `double`
- [`Kilometers`](xref:Xamarin.Forms.Maps.Distance.Kilometers) типа `double`
- [`Miles`](xref:Xamarin.Forms.Maps.Distance.Miles) типа `double`

Файл [мапкурдинатеспаже. XAML](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapCoordinatesPage.xaml) содержит несколько элементов `Label` для отображения сведений о `MapSpan`. Файл кода программной части [MapCoordinatesPage.XAML.CS](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapCoordinatesPage.xaml.cs) использует таймер, чтобы обновлять информацию по мере того, как пользователь управляет картой.

### <a name="position-extensions"></a>Расширения позиций

Новая библиотека для этой книги с именем [**Xamarin. формсбук. Toolkit. Maps**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit.Maps) содержит сведения, связанные с картой, но не зависящими от платформы типами. Класс [`PositionExtensions`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit.Maps/Xamarin.FormsBook.Toolkit.Maps/PositionExtensions.cs) содержит метод `ToString` для `Position` и метод для вычисления расстояния между двумя `Position` значениями.

### <a name="setting-an-initial-location"></a>Задание начального расположения

Можно вызвать метод [`MoveToRegion`](xref:Xamarin.Forms.Maps.Map.MoveToRegion(Xamarin.Forms.Maps.MapSpan)) `Map`, чтобы программно задать расположение и уровень масштабирования на карте. Аргумент имеет тип `MapSpan`. Объект `MapSpan` можно создать одним из следующих:

- [`MapSpan` конструктор](xref:Xamarin.Forms.Maps.MapSpan.%23ctor(Xamarin.Forms.Maps.Position,System.Double,System.Double)) с `Position`, а также диапазон широты и долготы
- [`MapSpan.FromCenterAndRadius`](xref:Xamarin.Forms.Maps.MapSpan.FromCenterAndRadius(Xamarin.Forms.Maps.Position,Xamarin.Forms.Maps.Distance)) с `Position` и RADIUS

Можно также создать новый `MapSpan` из существующего, используя методы [`ClampLatitude`](xref:Xamarin.Forms.Maps.MapSpan.ClampLatitude(System.Double,System.Double)) или [`WithZoom`](xref:Xamarin.Forms.Maps.MapSpan.WithZoom(System.Double)).

Файл [вйомингпаже. XAML](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/WyomingPage.xaml) и файл кода программной части [WyomingPage.XAML.CS](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/WyomingPage.xaml.cs) демонстрирует использование метода `MoveToRegion` для отображения состояния штат Вайоминг.

Можно также использовать [конструктор `Map`](xref:Xamarin.Forms.Maps.Map.%23ctor(Xamarin.Forms.Maps.MapSpan)) с объектом `MapSpan` для инициализации расположения схемы. В файле [ксамаринхкпаже. XAML](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/XamarinHQPage.xaml) показано, как полностью сделать это в XAML для отображения центрального офиса Xamarin в Сан-Франциско.

### <a name="dynamic-zooming"></a>Динамическое масштаб

Для динамического масштабирования карт можно использовать `Slider`. В файле [радиусзумпаже. XAML](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/RadiusZoomPage.xaml) и файле кода программной части [RadiusZoomPage.XAML.CS](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/RadiusZoomPage.xaml.cs) показано, как изменить радиус на карте на основе значения `Slider`.

Файл [лонгитудезумпаже. XAML](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LongitudeZoomPage.xaml) и файл кода программной части [LongitudeZoomPage.XAML.CS](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LongitudeZoomPage.xaml.cs) демонстрируют альтернативный подход, который лучше работает в Android, но ни один из подходов отлично работает на платформах Windows.

### <a name="the-phones-location"></a>Расположение телефона

Свойство [`IsShowingUser`](xref:Xamarin.Forms.Maps.Map.IsShowingUser) `Map` работает несколько по-разному на каждой платформе, как показано в файле [шовлокатионпаже. XAML](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/ShowLocationPage.xaml) :

- В iOS синяя точка обозначает расположение телефона, но необходимо вручную перемещаться
- В Android отображается значок, по которому при нажатии этой схемы перемещается расположение на телефоне.
- UWP аналогичен iOS, но иногда автоматически переходит в расположение

Проект **мапдемос** пытается имитировать подход Android, сначала определяя кнопку на основе значков на основе файла [милокатионбуттон. XAML](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MyLocationButton.xaml) и файла кода программной части [MyLocationButton.XAML.CS](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MyLocationButton.xaml.cs) .

Файл [готолокатионпаже. XAML](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GoToLocationPage.xaml) и файл кода программной части [GoToLocationPage.XAML.CS](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GoToLocationPage.xaml.cs) используйте эту кнопку для перехода к расположению телефона.

### <a name="pins-and-science-museums"></a>ПИН-коды и научные музеи

Наконец, класс `Map` определяет свойство [`Pins`](xref:Xamarin.Forms.Maps.Map.Pins) типа `IList<Pin>`. Класс [`Pin`](xref:Xamarin.Forms.Maps.Pin) определяет четыре свойства:

- [`Label`](xref:Xamarin.Forms.Maps.Pin.Label) типа `string`, обязательное свойство
- [`Address`](xref:Xamarin.Forms.Maps.Pin.Address) типа `string`, необязательный непонятный пользователю адрес
- [`Position`](xref:Xamarin.Forms.Maps.Pin.Position) типа `Position`, показывающего, где отображается ПИН-код на карте
- [`Type`](xref:Xamarin.Forms.Maps.Pin.Type) типа [`PinType`](xref:Xamarin.Forms.Maps.PinType)— перечисление, которое не используется.

Проект **мапдемос** содержит файл [сЦиенцемусеумс. XML](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/Data/ScienceMuseums.xml), в котором перечислены научные музеи в США, а также классы [`Locations`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/Locations.cs) и [`Site`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/Site.cs) для десериализации этих данных.

Файл [сЦиенцемусеумспаже. XAML](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/ScienceMuseumsPage.xaml) и файл кода программной части [ScienceMuseumsPage.XAML.CS](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/ScienceMuseumsPage.xaml.cs) отображают ПИН-коды для этих научных музеи на карте. Когда пользователь касается ПИН-кода, он отображает адрес и веб-сайт для музей.

### <a name="the-distance-between-two-points"></a>Расстояние между двумя точками

Класс [`PositionExtensions`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit.Maps/Xamarin.FormsBook.Toolkit.Maps/PositionExtensions.cs) содержит метод [`DistanceTo`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit.Maps/Xamarin.FormsBook.Toolkit.Maps/PositionExtensions.cs#L88) с упрощенным вычислением расстояния между двумя географическими расположениями.

Он используется в файле [локалмусеумспаже. XAML](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LocalMuseumsPage.xaml) и файле кода программной части [LocalMuseumsPage.XAML.CS](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LocalMuseumsPage.xaml.cs) для просмотра расстояния до музей из расположения пользователя:

[![Тройной снимок экрана локальной страницы музеи](images/ch28fg28-small.png "Расстояние до расположения")](images/ch28fg28-large.png#lightbox "Расстояние до расположения")

Кроме того, в программе показано, как динамически ограничить количество ПИН-кодов в зависимости от расположения схемы.

## <a name="geocoding-and-back-again"></a>Геокодирование и обратная кодировка

Сборка [**Xamarin. Forms. Maps**](xref:Xamarin.Forms.Maps) также содержит класс [`Geocoder`](xref:Xamarin.Forms.Maps.Geocoder) с методом [`GetPositionsForAddressAsync`](xref:Xamarin.Forms.Maps.Geocoder.GetPositionsForAddressAsync(System.String)) , который преобразует текстовый адрес в ноль или несколько возможных географических позиций, а другой метод [`GetAddressesForPositionAsync`](xref:Xamarin.Forms.Maps.Geocoder.GetAddressesForPositionAsync(Xamarin.Forms.Maps.Position)) , который преобразует в другой двух.

Это средство демонстрирует файл [жеокодерраундтрип. XAML](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GeocoderRoundTripPage.xaml) и файл кода программной части [GeocoderRoundTrip.XAML.CS](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GeocoderRoundTripPage.xaml.cs) .

## <a name="related-links"></a>Связанные ссылки

- [Глава 28 Full Text (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch28-Aug2016.pdf)
- [Глава 28 примеров](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28)
- [Схема Xamarin. Forms](~/xamarin-forms/user-interface/map/index.md)
