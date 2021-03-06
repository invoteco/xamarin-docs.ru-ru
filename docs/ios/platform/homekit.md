---
title: HomeKit в Xamarin. iOS
description: HomeKit — это платформа Apple для управления устройствами службы автоматизации дома. В этой статье рассказывается о HomeKit и рассказывается о настройке аксессуаров для тестирования в симуляторе аксессуаров HomeKit и написании простого приложения Xamarin. iOS для взаимодействия с этими аксессуарами.
ms.prod: xamarin
ms.assetid: 90C0C553-916B-46B1-AD52-1E7332792283
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/22/2017
ms.openlocfilehash: e0f6d9ed1028cd825d9fd8e40fe99c4ea250612f
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032419"
---
# <a name="homekit-in-xamarinios"></a>HomeKit в Xamarin. iOS

_HomeKit — это платформа Apple для управления устройствами службы автоматизации дома. В этой статье рассказывается о HomeKit и рассказывается о настройке аксессуаров для тестирования в симуляторе аксессуаров HomeKit и написании простого приложения Xamarin. iOS для взаимодействия с этими аксессуарами._

[![](homekit-images/accessory01.png "An example HomeKit enabled App")](homekit-images/accessory01.png#lightbox)

Компания Apple представила HomeKit в iOS 8 в качестве способа беспрепятственной интеграции нескольких устройств домашней службы автоматизации из различных поставщиков в единую, согласованную единицу. Получив общий протокол для обнаружения, настройки и управления устройствами главной автоматизации, HomeKit позволяет устройствам, не связанным с поставщиками, работать вместе, не имея индивидуальных поставщиков, которым приходится координировать усилия.

С помощью HomeKit вы можете создать приложение Xamarin. iOS, которое управляет любым устройством с поддержкой HomeKit, не используя предоставляемые поставщиком API или приложения. С помощью HomeKit можно выполнять следующие действия.

- Обнаружение новых устройств HomeKit с поддержкой главной автоматизации и добавление их в базу данных, которая будет храниться на всех устройствах iOS пользователя.
- Установка, Настройка, отображение и управление любыми устройствами в _базе данных конфигурации HomeKit Home_.
- Обмен данными с предварительно настроенным устройством HomeKit и их командам для выполнения отдельных действий или совместной работы, например для включения всех индикаторов на кухни.

Помимо передачи устройств в главной базе данных конфигурации в HomeKit включенные приложения, HomeKit предоставляет доступ к голосовым командам Siri. При наличии соответствующей настроенной установки HomeKit пользователь может выдавать голосовые команды, такие как «Siri», включать индикаторы в гостиной.

<a name="Home-Configuration-Database" />

## <a name="the-home-configuration-database"></a>Корневая база данных конфигурации

HomeKit организует все устройства автоматизации в заданном расположении в домашней коллекции. Эта коллекция предоставляет пользователю способ группирования своих домашних устройств автоматизации в логически упорядоченные блоки с осмысленными и удобочитаемыми метками.

Коллекция Home хранится в главной базе данных конфигурации, которая будет автоматически создана и синхронизирована на всех устройствах iOS пользователя. HomeKit предоставляет следующие классы для работы с базой данных конфигурации Home:

- `HMHome` — это контейнер верхнего уровня, содержащий все сведения и конфигурации для всех устройств главной автоматизации в одном физическом расположении (например, один дом для семьи). У пользователя может быть несколько домов, например Главная и Отпускная. Или они могут иметь разные "дома" для одного и того же свойства, например основной дом и гостевой дом по нештатной области. В любом случае по крайней мере один объект `HMHome` _должен_ быть настроен и сохранен до того, как можно будет указать другие данные HomeKit.
- `HMRoom` — хотя это необязательно, `HMRoom` позволяет пользователю определять определенные комнаты в домашних (`HMHome`ах) местах, таких как кухни, ванной, зал или гостиной. Пользователь может сгруппировать все устройства "Домашняя автоматизация" в определенном расположении в своем регионе в `HMRoom` и работать с ними как с единым целым. Например, вы запрашиваете Siri для отключения освещения.
- `HMAccessory` — это отдельное, физическое устройство автоматизации с поддержкой HomeKit, установленное в вашем компьютере пользователя (например, Smart термостата). Каждый `HMAccessory` назначается `HMRoom`. Если пользователь не настроил какие-либо комнаты, HomeKit назначает аксессуары специальной комнате по умолчанию.
- `HMService` — представляет службу, предоставляемую заданным `HMAccessory`, например состояние включения/выключения освещения или его цвет (если поддерживается изменение цвета). Каждый `HMAccessory` может иметь несколько служб, например дверцу несредство открытияной двери, которая также включает источник. Кроме того, у данного `HMAccessory` могут быть службы, такие как обновление встроенного по, которые выходят за пределы пользовательского элемента управления.
- `HMZone` — позволяет пользователю группировать коллекцию объектов `HMRoom` в логические зоны, такие как семьи, Довнстаирс или подвала. Хотя это и необязательно, это позволяет выполнять такие действия, как запрос Siri к выключению всех легких довнстаирс.

<a name="Provisioning-a-HomeKit-App" />

## <a name="provisioning-a-homekit-app"></a>Подготовка приложения HomeKit

Из-за требований безопасности, накладываемых HomeKit, приложение Xamarin. iOS, использующее платформу HomeKit, должно быть правильно настроено как на портале разработчика Apple, так и в файле проекта Xamarin. iOS.

Выполните следующие действия:

1. Войдите на [портал разработчика Apple](https://developer.apple.com).
2. Щелкните **сертификаты, идентификаторы & профили**.
3. Если вы еще не сделали этого, щелкните **идентификаторы** и создайте идентификатор приложения (например, `com.company.appname`), а затем измените существующий идентификатор.
4. Убедитесь, что служба **HomeKit** была проверена на наличие указанного идентификатора: 

    [![](homekit-images/provision01.png "Enable the HomeKit service for the given ID")](homekit-images/provision01.png#lightbox)
5. Сохраните изменения.
6. Щелкните **профили подготовки** > **разработки** и создайте новый профиль подготовки разработки для своего приложения: 

    [![](homekit-images/provision02.png "Create a new development provisioning profile for the app")](homekit-images/provision02.png#lightbox)
7. Скачайте и установите новый профиль подготовки или используйте Xcode для скачивания и установки профиля.
8. Измените параметры проекта Xamarin. iOS и убедитесь, что вы используете только что созданный профиль подготовки: 

    [![](homekit-images/provision03.png "Select provisioning profile just created")](homekit-images/provision03.png#lightbox)
9. Затем измените файл **info. plist** и убедитесь, что вы используете идентификатор приложения, который использовался для создания профиля подготовки: 

    [![](homekit-images/provision04.png "Set the App ID ")](homekit-images/provision04.png#lightbox)
10. Наконец, измените файл прав **. plist** и убедитесь, что выбрано право **HomeKit** . 

    [![](homekit-images/provision05.png "Enable the HomeKit entitlement")](homekit-images/provision05.png#lightbox)
11. Сохраните изменения во всех файлах.

После этих параметров приложение теперь готово к доступу к API-интерфейсам HomeKit Framework. Подробные сведения о подготовке см. в руководствах по [подготовке](~/ios/get-started/installation/device-provisioning/index.md) и [подготовке устройств к](~/ios/get-started/installation/device-provisioning/index.md) использованию.

> [!IMPORTANT]
> Для тестирования приложения с поддержкой HomeKit требуется реальное устройство iOS, которое было правильно подготовлено для разработки. HomeKit невозможно протестировать из симулятора iOS.

## <a name="the-homekit-accessory-simulator"></a>Симулятор аксессуаров HomeKit

Чтобы протестировать все возможные устройства и службы службы "Домашняя служба", не имея физического устройства, компания Apple создала _симулятор аксессуаров HomeKit_. С помощью этого симулятора можно установить и настроить виртуальные устройства HomeKit.

### <a name="installing-the-simulator"></a>Установка симулятора

Apple предоставляет симулятор аксессуаров HomeKit как отдельную загрузку из Xcode, поэтому ее необходимо установить перед продолжением.

Выполните следующие действия:

1. В веб-браузере посетите страницу [Downloads for Apple Developers](https://developer.apple.com/download/more/?name=for%20Xcode)
2. Скачайте **дополнительные средства для Xcode XXX** (где XXX — версия Xcode, которую вы установили): 

    [![](homekit-images/simulator01.png "Download the Additional Tools for Xcode")](homekit-images/simulator01.png#lightbox)
3. Откройте образ диска и установите средства в каталоге **приложений** .

После установки симулятора аксессуаров HomeKit виртуальные аксессуары можно создавать для тестирования.

### <a name="creating-virtual-accessories"></a>Создание виртуальных аксессуаров

Чтобы запустить симулятор аксессуаров HomeKit и создать несколько виртуальных аксессуаров, выполните следующие действия.

1. В папке приложения запустите симулятор аксессуаров HomeKit: 

    [![](homekit-images/simulator02.png "The HomeKit Accessory Simulator")](homekit-images/simulator02.png#lightbox)
2. Нажмите кнопку **+** и выберите **создать периферию...** : 

    [![](homekit-images/simulator03.png "Add a new accessory")](homekit-images/simulator03.png#lightbox)
3. Заполните сведения о новом аксессуаре и нажмите кнопку **"Готово** ": 

    [![](homekit-images/simulator04.png "Fill out the information about the new accessory")](homekit-images/simulator04.png#lightbox)
4. Щелкните **Добавить службу.** и выберите тип службы из раскрывающегося списка: 

    [![](homekit-images/simulator05.png "Select a service type from the dropdown")](homekit-images/simulator05.png#lightbox)
5. Укажите **имя** службы и нажмите кнопку **"Готово"** : 

    [![](homekit-images/simulator06.png "Enter a Name for the service")](homekit-images/simulator06.png#lightbox)
6. Можно указать дополнительные характеристики для службы, нажав кнопку **Добавить характеристику** и настроив необходимые параметры: 

    [![](homekit-images/simulator07.png "Configuring the required settings")](homekit-images/simulator07.png#lightbox)
7. Повторите описанные выше действия, чтобы создать один из типов виртуальных устройств автоматизации, поддерживаемых HomeKit.

После создания и настройки некоторых примеров виртуальных HomeKit вы можете использовать эти устройства и управлять этими устройствами из приложения Xamarin. iOS.

## <a name="configuring-the-infoplist-file"></a>Настройка файла INFO. plist

Для iOS 10 (и более поздних версий) разработчику потребуется добавить ключ `NSHomeKitUsageDescription` в файл `Info.plist` приложения и предоставить строку, объявляющая, почему приложению требуется доступ к базе данных HomeKit пользователя. Эта строка будет представлена пользователю при первом запуске приложения:

[![](homekit-images/info01.png "The HomeKit permission dialog")](homekit-images/info01.png#lightbox)

Чтобы задать этот ключ, выполните следующие действия.

1. Дважды щелкните файл `Info.plist` в **Обозреватель решений** , чтобы открыть его для редактирования.
2. В нижней части экрана переключитесь в представление **исходного кода** .
3. Добавление новой **записи** в список.
4. В раскрывающемся списке выберите **Конфиденциальность — HomeKit Usage описание**: 

    [![](homekit-images/info02.png "Select Privacy - HomeKit Usage Description")](homekit-images/info02.png#lightbox)
5. Введите описание, по которому приложение хочет получить доступ к базе данных HomeKit пользователя: 

    [![](homekit-images/info03.png "Enter a description")](homekit-images/info03.png#lightbox)
6. Сохраните изменения в файле.

> [!IMPORTANT]
> Если не задать ключ `NSHomeKitUsageDescription` в файле `Info.plist`, приложение будет _автоматически завершаться сбоем_ (закрывается системой во время выполнения) без ошибок при выполнении в iOS 10 (или более поздней версии).

## <a name="connecting-to-homekit"></a>Подключение к HomeKit

Для взаимодействия с HomeKit приложение Xamarin. iOS сначала должно создать экземпляр класса `HMHomeManager`. Главный менеджер является центральной точкой входа в HomeKit и отвечает за предоставление списка доступных домов, обновление и обслуживание этого списка и возврат _основного домашнего_пользователя.

Объект `HMHome` содержит все сведения о домашнем объекте, включая все комнаты, группы или зоны, которые могут содержаться в нем, а также все установленные стандартные средства автоматизации. Прежде чем выполнять любые операции в HomeKit, необходимо создать по крайней мере один `HMHome` и назначить его в качестве первичной домашней.

Приложение отвечает за проверку существования первичного дома, а также создание и назначение его в случае отсутствия.

### <a name="adding-a-home-manager"></a>Добавление диспетчера Home

Чтобы добавить HomeKit осведомленности в приложение Xamarin. iOS, измените файл **AppDelegate.CS** , чтобы изменить его, и сделайте его следующим:

```csharp
using HomeKit;
...

public HMHomeManager HomeManager { get; set; }
...

public override void FinishedLaunching (UIApplication application)
{
    // Attach to the Home Manager
    HomeManager = new HMHomeManager ();
    Console.WriteLine ("{0} Home(s) defined in the Home Manager", HomeManager.Homes.Count());

    // Wire-up Home Manager Events
    HomeManager.DidAddHome += (sender, e) => {
        Console.WriteLine("Manager Added Home: {0}",e.Home);
    };

    HomeManager.DidRemoveHome += (sender, e) => {
        Console.WriteLine("Manager Removed Home: {0}",e.Home);
    };
    HomeManager.DidUpdateHomes += (sender, e) => {
        Console.WriteLine("Manager Updated Homes");
    };
    HomeManager.DidUpdatePrimaryHome += (sender, e) => {
        Console.WriteLine("Manager Updated Primary Home");
    };
}
```

При первом запуске приложения пользователю будет предложено разрешить ему доступ к сведениям HomeKit:

[![](homekit-images/home01.png "The user will be asked if they want to allow it to access their HomeKit information")](homekit-images/home01.png#lightbox)

Если пользователь ответит на " **ОК**", приложение сможет работать со своими HomeKit, иначе в противном случае вызовы HomeKit будут завершаться ошибкой.

После того, как диспетчер домашнего размещения заполнится, в приложении потребуется определить, настроена ли основная домашняя страница, и если нет, предоставьте пользователю возможность создать и назначить ее.

### <a name="accessing-the-primary-home"></a>Доступ к основной домашней странице

Как упоминалось выше, необходимо создать и настроить первичную домашнюю сеть, прежде чем HomeKit станет доступной, и ответственность за предоставление пользователям возможности создавать и назначать основную домашнюю страницу, если она еще не существует.

Когда приложение впервые запускается или возвращается из фона, ему необходимо отслеживать событие `DidUpdateHomes` класса `HMHomeManager`, чтобы проверить наличие первичной домашней страницы. Если такой объект не существует, он должен предоставить пользователю интерфейс, чтобы создать его.

Следующий код можно добавить в контроллер представления для проверки первичной домашней страницы:

```csharp
using HomeKit;
...

public AppDelegate ThisApp {
    get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
}
...

// Wireup events
ThisApp.HomeManager.DidUpdateHomes += (sender, e) => {

    // Was a primary home found?
    if (ThisApp.HomeManager.PrimaryHome == null) {
        // Ask user to add a home
        PerformSegue("AddHomeSegue",this);
    }
};
```

Когда диспетчер дома устанавливает подключение к HomeKit, будет запущено событие `DidUpdateHomes`, все существующие дома будут загружены в коллекцию домов, а также будет загружена Главная Домашняя страница, если она доступна.

### <a name="adding-a-primary-home"></a>Добавление первичной домашней страницы

Если свойство `PrimaryHome` `HMHomeManager` `null` после события `DidUpdateHomes`, необходимо предоставить пользователю способ создания и назначения первичной домашней страницы перед продолжением.

Обычно приложение предоставляет пользователю форму для имени новой домашней страницы, которая затем передается главному диспетчеру для настройки в качестве основной домашней страницы. Для примера приложения **хомекитинтро** модальное представление было создано в конструкторе iOS и вызвано `AddHomeSegue` перехода из основного интерфейса приложения.

Он предоставляет текстовое поле для ввода имени новой домашней страницы и кнопки для добавления домашней страницы. Когда пользователь нажмет кнопку " **Добавить домашнюю** ", следующий код вызывает диспетчер Home для добавления домашней страницы:

```csharp
// Add new home to HomeKit
ThisApp.HomeManager.AddHome(HomeName.Text,(home,error) =>{
    // Did an error occur
    if (error!=null) {
        // Yes, inform user
        AlertView.PresentOKAlert("Add Home Error",string.Format("Error adding {0}: {1}",HomeName.Text,error.LocalizedDescription),this);
        return;
    }

    // Make the primary house
    ThisApp.HomeManager.UpdatePrimaryHome(home,(err) => {
        // Error?
        if (err!=null) {
            // Inform user of error
            AlertView.PresentOKAlert("Add Home Error",string.Format("Unable to make this the primary home: {0}",err.LocalizedDescription),this);
            return ;
        }
    });

    // Close the window when the home is created
    DismissViewController(true,null);
});
```

Метод `AddHome` попытается создать новую домашнюю страницу и вернуть ее в указанную подпрограммы обратного вызова. Если свойство `error` не `null`, то произошла ошибка и она должна быть представлена пользователю. Наиболее распространенные ошибки вызваны неуникальными домашним именем или диспетчером домашнего диспетчера, который не может взаимодействовать с HomeKit.

Если домашняя страница создана успешно, необходимо вызвать метод `UpdatePrimaryHome`, чтобы задать новую домашнюю домашнюю страницу. Опять же, если свойство `error` не `null`, произошла ошибка и она должна быть представлена пользователю.

Кроме того, следует наблюдать за `DidAddHome` и `DidRemoveHome` событиями домашнего диспетчера, а также при необходимости обновлять пользовательский интерфейс приложения.

> [!IMPORTANT]
> Метод `AlertView.PresentOKAlert`, используемый в приведенном выше примере кода, является вспомогательным классом в приложении Хомекитинтро, который упрощает работу с оповещениями iOS.

## <a name="finding-new-accessories"></a>Поиск новых аксессуаров

После того как основная домашняя страница будет определена или загружена из диспетчера Home, приложение Xamarin. iOS может вызвать `HMAccessoryBrowser`, чтобы найти новые стандартные аксессуары и добавить их в домашнюю страницу.

Вызовите метод `StartSearchingForNewAccessories`, чтобы начать поиск новых аксессуаров и метод `StopSearchingForNewAccessories` по завершении.

> [!IMPORTANT]
> `StartSearchingForNewAccessories` не должны выполняться в течение длительного периода времени, так как это отрицательно повлияет на время работы батареи и производительность устройства iOS. Компания Apple предлагает вызвать `StopSearchingForNewAccessories` через минуту или только после того, как будет представлен пользовательский интерфейс поиска аксессуаров.

Событие `DidFindNewAccessory` будет вызвано при обнаружении новых аксессуаров, и они будут добавлены в список `DiscoveredAccessories` в браузере аксессуаров.

Список `DiscoveredAccessories` будет содержать коллекцию объектов `HMAccessory`, которые определяют устройство HomeKit с поддержкой домашней автоматизации и доступные службы, такие как индикаторы или элемент управления "задвижок".

После обнаружения нового аксессуара оно должно быть представлено пользователю, чтобы его можно было выбрать и добавить в домашнюю страницу. Пример

[![](homekit-images/accessory01.png "Finding a new accessory")](homekit-images/accessory01.png#lightbox)

Вызовите метод `AddAccessory`, чтобы добавить выбранное украшение в коллекцию Home. Пример:

```csharp
// Add the requested accessory to the home
ThisApp.HomeManager.PrimaryHome.AddAccessory (_controller.AccessoryBrowser.DiscoveredAccessories [indexPath.Row], (err) => {
    // Did an error occur
    if (err !=null) {
        // Inform user of error
        AlertView.PresentOKAlert("Add Accessory Error",err.LocalizedDescription,_controller);
    }
});
```

Если свойство `err` не `null`, то произошла ошибка и она должна быть представлена пользователю. В противном случае пользователю будет предложено ввести код установки добавляемого устройства:

[![](homekit-images/accessory02.png "Enter the setup code for the device to add")](homekit-images/accessory02.png#lightbox)

В симуляторе аксессуаров HomeKit этот номер можно найти в поле **код установки** :

[![](homekit-images/accessory03.png "The Setup Code field in the HomeKit Accessory Simulator")](homekit-images/accessory03.png#lightbox)

Для реальных аксессуаров HomeKit код установки будет либо напечатан на метке на самом устройстве, либо в поле продукт, либо в руководстве пользователя по установке.

Вы должны наблюдать за событием `DidRemoveNewAccessory` в браузере аксессуаров и обновлять пользовательский интерфейс, чтобы удалить принадлежность из доступного списка после того, как пользователь добавит его в коллекцию Home.

## <a name="working-with-accessories"></a>Работа с аксессуарами

После того как основная домашняя и установленная и в нее добавлены аксессуары, вы можете просмотреть список аксессуаров (и, возможно, комнат) для работы пользователя.

Объект `HMRoom` содержит все сведения об заданной комнате и всех аксессуаров, принадлежащих ей. Комнаты при необходимости можно объединить в одну или несколько зон. `HMZone` содержит все сведения об определенной зоне и всех ее комнатах.

В этом примере мы будем выполнять простоту и работать с домашними аксессуарами напрямую, не организуя их в комнаты или зоны.

Объект `HMHome` содержит список назначенных аксессуаров, которые могут быть представлены пользователю в его свойстве `Accessories`. Пример:

[![](homekit-images/accessory04.png "An example accessory")](homekit-images/accessory04.png#lightbox)

Здесь пользователь может выбрать конкретное украшение и работать с предоставляемыми им службами.

## <a name="working-with-services"></a>Работа со службами

Когда пользователь взаимодействует с данным устройством главной автоматизации HomeKit с поддержкой, он обычно предоставляется через предоставляемые им службы. Свойство `Services` класса `HMAccessory` содержит коллекцию объектов `HMService`, определяющих службы, которые предлагает устройство.

Службы — это такие вещи, как источники света, термостатов, открытые дверцы, переключатели или блокировки. Некоторые устройства (например, дверца несредство открытияной области) предоставляют несколько служб, таких как легкая и возможность открытия или закрытия дверцы.

В дополнение к определенным службам, предоставляемым заданным аксессуаром, каждый аксессуар содержит `Information Service`, определяющий такие свойства, как имя, производитель, модель и серийный номер.

### <a name="accessory-service-types"></a>Типы услуг принадлежности

Следующие типы служб доступны через перечисление `HMServiceType`:

- **Акцессоринформатион** — содержит сведения об указанном устройстве службы автоматизации (аксессуаров).
- **Аиркуалитисенсор** — определяет Датчик качества воздуха.
- **Батарея** — определяет состояние аккумулятора принадлежности.
- **Карбондиоксидесенсор** — определяет датчик углекислого газа.
- **Карбонмоноксидесенсор** — определяет датчик углекислого угарногоа.
- **Контактсенсор** — определяет датчик контактов (например, открытие или закрытие окна).
- **Дверь** — определяет датчик состояния двери (например, Открытый или закрытый).
- **Вентилятор** — определяет удаленный вентилятор.
- **Гаражедуропенер** — определяет средство открытия дверь.
- **Хумидитисенсор** — определяет датчик влажности.
- **Леаксенсор** — определяет датчик утечки (например, для "горячей" хеатер или Вашинг машины).
- **Лампочка** определяет изолированный источник или освещение, которое является частью другого аксессуара (например, дверца средство открытия).
- **LightSensor** — определяет датчик освещения.
- **Локкманажемент** — определяет службу, которая управляет автоматической блокировкой двери.
- **Локкмечанисм** — определяет удаленную управляемую блокировку (например, блокировку двери).
- **Мотионсенсор** — определяет датчик движения.
- **Оккупанцисенсор** — определяет датчик заполнения.
- **Розетка** — определяет удаленно управляемую розетку.
- **Секуритисистем** — определяет домашнюю систему безопасности.
- **Статефулпрограммаблесвитч** — определяет программируемый коммутатор, который остается в состоянии предоставления после активации (например, переключатель перелистывания).
- **Стателесспрограммаблесвитч** — определяет программируемый коммутатор, который возвращается в исходное состояние после активации (например, кнопка "Отправить").
- **Смокесенсор** — определяет датчик состояния.
- **Параметр** — определяет параметр ON/OFF, например стандартный коммутатор стены.
- **Датчик температуры** — определяет датчик температуры.
- **Термостата** — определяет смарт-термостата, используемый для управления системой кондиционирования.
- **Окно** — определяет автоматизированное окно, которое карамели удаленно или закрыто.
- **Виндовковеринг** — определяет окно, управляемое удаленно, как слепые, которые могут быть открыты или закрыты.

### <a name="displaying-service-information"></a>Отображение сведений о службе

После загрузки `HMAccessory` можно запросить отдельные предоставляемые им объекты `HNService` и отобразить эти сведения пользователю:

[![](homekit-images/accessory05.png "Displaying Service Information")](homekit-images/accessory05.png#lightbox)

Прежде чем пытаться работать с ним, всегда следует проверять свойство `Reachable` `HMAccessory`. Принадлежность может быть недостижима, если пользователь не входит в диапазон устройства или если он был отсоединен.

После выбора службы пользователь может просмотреть или изменить одно или несколько характеристик этой службы, чтобы отслеживать или контролировать конкретное устройство главной автоматизации.

<a name="Working-with-Characteristics" />

## <a name="working-with-characteristics"></a>Работа с характеристиками

Каждый объект `HMService` может содержать коллекцию объектов `HMCharacteristic`, которые могут предоставить сведения о состоянии службы (например, открытую или закрытую дверцу) или разрешить пользователю настраивать состояние (например, задать цвет освещения).

`HMCharacteristic` не только предоставляет сведения о характеристике и ее состоянии, но также предоставляет методы для работы с состоянием через _метаданные характеристики_ (`HMCharacteristisMetadata`). Эти метаданные могут предоставлять свойства (например, минимальные и максимальные диапазоны значений), которые удобно использовать при отображении сведений для пользователя или при изменении состояния.

Перечисление `HMCharacteristicType` предоставляет набор значений метаданных характеристик, которые могут быть определены или изменены следующим образом:

- админонлякцесс
- аирпартикулатеденсити
- аирпартикулатесизе
- аиркуалити
- аудиофидбакк
- баттерилевел
- Яркость
- карбондиоксидедетектед
- карбондиоксиделевел
- карбондиоксидепеаклевел
- карбонмоноксидедетектед
- карбонмоноксиделевел
- карбонмоноксидепеаклевел
- чаргингстате
- ContactState
- кулингсрешолд
- куррентдурстате
- курренсеатингкулинг
- курренсоризонталтилт
- куррентлигхтлевел
- куррентлоккмечанисмстате
- CurrentPosition
- куррентрелативехумидити
- куррентсекуритисистемстате
- курренттемпературе
- куррентвертикалтилт
- фирмвареверсион
- хардвареверсион
- хеатингкулингстатус
- хеатингсрешолд
- холдпоситион
- Цветовой тон
- Identify
- инпутевент
- леакдетектед
- локкманажементаутосекуретимеаут
- локкманажементконтролпоинт
- локкмечанисмласткновнактион
- Журналы
- Изготовитель
- Модель
- мотиондетектед
- Название
- обструктиондетектед
- оккупанцидетектед
- аутлетинусе
- аутпутстате
- поситионстате
- Установлен
- ротатиондиректион
- ротатионспид
- Насыщенность
- Номер
- смокедетектед
- SoftwareVersion
- статусактиве
- статусфаулт
- статусжаммед
- статусловбаттери
- статустамперед
- таржетдурстате
- таржесеатингкулинг
- таржесоризонталтилт
- таржетлоккмечанисмстате
- таржетпоситион
- таржетрелативехумидити
- таржетсекуритисистемстате
- таржеттемпературе
- таржетвертикалтилт
- температуреунитс
- Version

### <a name="working-with-a-characteristics-value"></a>Работа со значением характеристик

Чтобы убедиться, что приложение имеет Последнее состояние данной характеристики, вызовите метод `ReadValue` класса `HMCharacteristic`. Если свойство `err` не `null`, то произошла ошибка, и она может быть недоступна пользователю.

Свойство `Value` характеристик содержит текущее состояние данной характеристики как `NSObject`, и поэтому не может работать непосредственно в C#.

Для считывания значения в пример приложения **хомекитинтро** был добавлен следующий вспомогательный класс:

```csharp
using System;
using Foundation;
using System.Globalization;
using CoreGraphics;

namespace HomeKitIntro
{
    /// <summary>
    /// NS object converter is a helper class that helps to convert NSObjects into
    /// C# objects
    /// </summary>
    public static class NSObjectConverter
    {
        #region Static Methods
        /// <summary>
        /// Converts to an object.
        /// </summary>
        /// <returns>The object.</returns>
        /// <param name="nsO">Ns o.</param>
        /// <param name="targetType">Target type.</param>
        public static Object ToObject (NSObject nsO, Type targetType)
        {
            if (nsO is NSString) {
                return nsO.ToString ();
            }

            if (nsO is NSDate) {
                var nsDate = (NSDate)nsO;
                return DateTime.SpecifyKind ((DateTime)nsDate, DateTimeKind.Unspecified);
            }

            if (nsO is NSDecimalNumber) {
                return decimal.Parse (nsO.ToString (), CultureInfo.InvariantCulture);
            }

            if (nsO is NSNumber) {
                var x = (NSNumber)nsO;

                switch (Type.GetTypeCode (targetType)) {
                case TypeCode.Boolean:
                    return x.BoolValue;
                case TypeCode.Char:
                    return Convert.ToChar (x.ByteValue);
                case TypeCode.SByte:
                    return x.SByteValue;
                case TypeCode.Byte:
                    return x.ByteValue;
                case TypeCode.Int16:
                    return x.Int16Value;
                case TypeCode.UInt16:
                    return x.UInt16Value;
                case TypeCode.Int32:
                    return x.Int32Value;
                case TypeCode.UInt32:
                    return x.UInt32Value;
                case TypeCode.Int64:
                    return x.Int64Value;
                case TypeCode.UInt64:
                    return x.UInt64Value;
                case TypeCode.Single:
                    return x.FloatValue;
                case TypeCode.Double:
                    return x.DoubleValue;
                }
            }

            if (nsO is NSValue) {
                var v = (NSValue)nsO;

                if (targetType == typeof(IntPtr)) {
                    return v.PointerValue;
                }

                if (targetType == typeof(CGSize)) {
                    return v.SizeFValue;
                }

                if (targetType == typeof(CGRect)) {
                    return v.RectangleFValue;
                }

                if (targetType == typeof(CGPoint)) {
                    return v.PointFValue;
                }
            }

            return nsO;
        }

        /// <summary>
        /// Convert to string
        /// </summary>
        /// <returns>The string.</returns>
        /// <param name="nsO">Ns o.</param>
        public static string ToString(NSObject nsO) {
            return (string)ToObject (nsO, typeof(string));
        }

        /// <summary>
        /// Convert to date time
        /// </summary>
        /// <returns>The date time.</returns>
        /// <param name="nsO">Ns o.</param>
        public static DateTime ToDateTime(NSObject nsO){
            return (DateTime)ToObject (nsO, typeof(DateTime));
        }

        /// <summary>
        /// Convert to decimal number
        /// </summary>
        /// <returns>The decimal.</returns>
        /// <param name="nsO">Ns o.</param>
        public static decimal ToDecimal(NSObject nsO){
            return (decimal)ToObject (nsO, typeof(decimal));
        }

        /// <summary>
        /// Convert to boolean
        /// </summary>
        /// <returns><c>true</c>, if bool was toed, <c>false</c> otherwise.</returns>
        /// <param name="nsO">Ns o.</param>
        public static bool ToBool(NSObject nsO){
            return (bool)ToObject (nsO, typeof(bool));
        }

        /// <summary>
        /// Convert to character
        /// </summary>
        /// <returns>The char.</returns>
        /// <param name="nsO">Ns o.</param>
        public static char ToChar(NSObject nsO){
            return (char)ToObject (nsO, typeof(char));
        }

        /// <summary>
        /// Convert to integer
        /// </summary>
        /// <returns>The int.</returns>
        /// <param name="nsO">Ns o.</param>
        public static int ToInt(NSObject nsO){
            return (int)ToObject (nsO, typeof(int));
        }

        /// <summary>
        /// Convert to float
        /// </summary>
        /// <returns>The float.</returns>
        /// <param name="nsO">Ns o.</param>
        public static float ToFloat(NSObject nsO){
            return (float)ToObject (nsO, typeof(float));
        }

        /// <summary>
        /// Converts to double
        /// </summary>
        /// <returns>The double.</returns>
        /// <param name="nsO">Ns o.</param>
        public static double ToDouble(NSObject nsO){
            return (double)ToObject (nsO, typeof(double));
        }
        #endregion
    }
}
```

`NSObjectConverter` используется каждый раз, когда приложению требуется прочитать текущее состояние характеристики. Пример.

```csharp
var value = NSObjectConverter.ToFloat (characteristic.Value);
```

Приведенная выше строка преобразует значение в `float`, которое затем можно использовать в коде Xamarin C# .

Чтобы изменить `HMCharacteristic`, вызовите его метод `WriteValue` и заключите новое значение в `NSObject.FromObject` вызов. Пример.

```csharp
Characteristic.WriteValue(NSObject.FromObject(value),(err) =>{
    // Was there an error?
    if (err!=null) {
        // Yes, inform user
        AlertView.PresentOKAlert("Update Error",err.LocalizedDescription,Controller);
    }
});
```

Если свойство `err` не `null`, произошла ошибка и она должна быть представлена пользователю.

### <a name="testing-characteristic-value-changes"></a>Проверка изменений значений характеристик

При работе с `HMCharacteristics` и смоделированными аксессуарами изменения свойства `Value` можно отслеживать внутри симулятора аксессуаров HomeKit.

Если приложение **хомекитинтро** работает на устройстве реального времени iOS, изменения в значении характеристик должны сразу же увидеть в симуляторе аксессуаров HomeKit. Например, изменение состояния освещения в приложении iOS:

[![](homekit-images/test01.png "Changing the state of a light in an iOS app")](homekit-images/test01.png#lightbox)

Следует изменить состояние освещения в симуляторе аксессуаров HomeKit. Если значение не меняется, проверьте состояние сообщения об ошибке при записи новых значений характеристик и убедитесь, что периферия по-прежнему достижима.

## <a name="advanced-homekit-features"></a>Расширенные функции HomeKit

В этой статье описаны основные функции, необходимые для работы с HomeKitными аксессуарами в приложении Xamarin. iOS. Однако существует несколько дополнительных функций HomeKit, которые не рассматриваются в этом введении:

- **Комнаты** — аксессуары, включенные в HomeKit, могут быть организованы в комнаты конечным пользователем. Это позволяет HomeKit представлять аксессуары таким образом, чтобы пользователь легко понимал и работал с. Дополнительные сведения о создании и обслуживании комнат см. в документации Apple [хмрум](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMRoom_Class/index.html#//apple_ref/occ/cl/HMRoom) .
- **Зоны** — комнаты при необходимости могут быть организованы конечным пользователем в зоны. Зона ссылается на коллекцию комнат, которые пользователь может обрабатывать как единое целое. Например: семьи, Довнстаирс или подвала. Опять же, это позволяет HomeKit предоставлять аксессуары и работать с ними способом, который имеет смысл для конечного пользователя. Дополнительные сведения о создании и обслуживании зон см. в документации Apple [хмзоне](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMZone_Class/index.html#//apple_ref/occ/cl/HMZone) .
- **Действия и наборы действий** — действия изменяют характеристики службы аксессуаров и могут быть сгруппированы в наборы. Набор действий действует как скрипты для управления группой аксессуаров и координации их действий. Например, сценарий просмотра ТЕЛЕПЕРЕДАЧ может закрывать слепые, затемнять индикаторы и включать телевизор и его звуковую систему. Дополнительные сведения о создании и обслуживании действий и наборов действий см. в документации по [хмактион](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMAction_Class/index.html#//apple_ref/occ/cl/HMAction) и [хмактионсет](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMActionSet_Class/index.html#//apple_ref/occ/cl/HMActionSet) компании Apple.
- **Триггеры** . триггер может активировать один или несколько наборов действий при выполнении определенного набора условий. Например, включите портч Light и заблокируйте все внешние двери, когда она становится темной. Дополнительные сведения о создании и обслуживании триггеров см. в документации Apple [хмтригжер](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMTrigger_Class/index.html#//apple_ref/occ/cl/HMTrigger) .

Поскольку эти функции используют те же методы, которые описаны выше, их следует легко реализовать, следуя рекомендациям Apple [Хомекитдевелопер Guide](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/HomeKitDeveloperGuide/Introduction/Introduction.html), [правилам интерфейса пользователя HomeKit](https://developer.apple.com/homekit/ui-guidelines/) и [Справочнику по HomeKit Framework](https://developer.apple.com/library/ios/home_kit_framework_ref).

## <a name="homekit-app-review-guidelines"></a>Рекомендации по проверке приложения HomeKit

Перед отправкой приложения Xamarin. iOS HomeKit в iTunes Connect для выпуска в магазине приложений iTunes обязательно следуйте рекомендациям Apple для приложений с поддержкой HomeKit.

- Основное назначение приложения _должно_ быть главной автоматизацией при использовании платформы HomeKit.
- Маркетинговый текст приложения должен уведомлять пользователей о том, что HomeKit используется и должен предоставить политику конфиденциальности.
- Сбор сведений о пользователях или использование HomeKit для рекламы строго запрещено.

Рекомендации по полному анализу см. в разделе [рекомендации по проверке в магазине приложений](https://developer.apple.com/app-store/review/guidelines/)Apple.

## <a name="whats-new-in-ios-9"></a>Новые возможности iOS 9

Компания Apple внесла следующие изменения и дополнения в HomeKit для iOS 9:

- **Обслуживание существующих объектов** . Если изменилось существующее украшение, диспетчер дома (`HMHomeManager`) сообщит об изменении конкретного элемента.
- **Постоянные идентификаторы** . все соответствующие классы HomeKit теперь включают свойство `UniqueIdentifier` для уникальной идентификации данного элемента в приложениях с поддержкой HomeKit (или экземплярах того же приложения).
- **Управление пользователями** — добавлен встроенный контроллер представления для управления пользователями, которые имеют доступ к устройствам HomeKit в домашней сети основного пользователя.
- **Возможности пользователей** . Пользователи HomeKit теперь имеют набор привилегий, которые определяют, какие функции они могут использовать в стандартных аксессуарах HomeKit и HomeKit. Приложение должно отображать только те возможности, которые относятся к текущему пользователю. Например, только администраторы должны иметь возможность поддерживать других пользователей.
- **Предопределенные** сцены — это стандартные сцены для четырех распространенных событий, возникающих для среднего пользователя HomeKit: получение, выход, возврат, переход в "испытательный". Эти предопределенные сцены нельзя удалить из дома.
- **Сцены и Siri** -Siri имеют более глубокую поддержку сцен в iOS 9 и могут распознать имя любой сцены, определенной в HomeKit. Пользователь может выполнить сцену, просто говорите его имя на Siri.
- **Категории аксессуаров** — набор предопределенных категорий был добавлен ко всем аксессуарам и помогает определить тип принадлежности, добавляемого в домашнюю или работающую из приложения. Эти новые категории доступны во время установки аксессуаров.
- **Apple Watch support** -HomeKit теперь доступен для watchOS, и Apple Watch сможет управлять устройствами, поддерживающими HomeKit, без устройства iPhone, близкого к просмотру. HomeKit for watchOS поддерживает следующие возможности: Просмотр домов, управление аксессуарами и выполнение сцен.
- **Новый тип триггера события** . Помимо триггеров типа таймера, поддерживаемых в iOS 8, iOS 9 теперь поддерживает триггеры событий на основе состояния принадлежности (например, данных датчика) или географического расположения. Триггеры событий используют `NSPredicates` для задания условий выполнения.
- **Удаленный доступ** . Благодаря удаленному доступу пользователь может управлять своими стандартными аксессуарами по автоматизации HomeKit, когда они находятся далеко от дома в удаленном расположении. В iOS 8 это поддерживалось, только если у пользователя есть третье телевидение Apple TV в домашней сети. В iOS 9 это ограничение ликвидируется, и удаленный доступ поддерживается через iCloud и протокол HomeKit (хап).
- **Новые возможности Bluetooth с низким энергопотреблением (BLE)** — HomeKit теперь поддерживает больше типов аксессуаров, которые могут взаимодействовать через протокол Bluetooth с низким энергопотреблением (BLE). Используя безопасное туннелирование хап, HomeKitное устройство может предоставлять другое устройство Bluetooth через Wi-Fi (если оно выходит за пределы диапазона Bluetooth). В iOS 9 аксессуары BLE имеют полную поддержку уведомлений и метаданных.
- **Новые категории аксессуаров** — Apple добавила в iOS 9 следующие новые категории аксессуаров: окна, двигательные двери и окна, системы сигнализации, датчики и программируемые коммутаторы.

Дополнительные сведения о новых возможностях HomeKit в iOS 9 см. в статье об [индексе Apple HomeKit](https://developer.apple.com/homekit/) и новых возможностях [в HomeKit](https://developer.apple.com/videos/wwdc/2015/?id=210) Video.

## <a name="summary"></a>Сводка

В этой статье мы предоставили платформу Apple HomeKit Home. В нем было показано, как установить и настроить тестовые устройства с помощью симулятора аксессуаров HomeKit, а также как создать простое приложение Xamarin. iOS для обнаружения, взаимодействия с устройствами службы автоматизации и управления им с помощью HomeKit.

## <a name="related-links"></a>Связанные ссылки

- [Примеры для iOS 9](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS9)
- [iOS 9 для разработчиков](https://developer.apple.com/ios/pre-release/)
- [Новые возможности iOS 9,0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Хомекитдевелопер Guide](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/HomeKitDeveloperGuide/Introduction/Introduction.html)
- [Рекомендации по пользовательскому интерфейсу HomeKit](https://developer.apple.com/homekit/ui-guidelines/)
- [Справочник по HomeKit Framework](https://developer.apple.com/library/ios/home_kit_framework_ref)
