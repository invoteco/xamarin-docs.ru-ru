---
title: Унифицированные раскадровки в Xamarin. iOS
description: В этом документе описываются унифицированные раскадровки в Xamarin. iOS. Унифицированные раскадровки позволяют разработчикам поддерживать несколько размеров экрана с одним определением интерфейса.
ms.prod: xamarin
ms.assetid: F6F70374-FC2A-4401-A712-A16D0F9B340F
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/20/2017
ms.openlocfilehash: 13891100d3571f9e847243172aa974072f46e7fe
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/13/2020
ms.locfileid: "79306257"
---
# <a name="unified-storyboards-in-xamarinios"></a>Унифицированные раскадровки в Xamarin. iOS

iOS 8 включает новый, более простой в использовании механизм создания пользовательского интерфейса — унифицированную раскадровку. С помощью одной раскадровки для охвата всех различных размеров аппаратного экрана, быстрого и быстро реагирующего представления можно создать в стиле "один раз," использовать "многие".

Поскольку разработчику больше не требуется создавать отдельную и специальную раскадровку для устройств iPhone и iPad, они имеют гибкость в проектировании приложений с помощью общего интерфейса, а затем настраивают этот интерфейс для различных классов размеров. Таким образом, приложение может адаптироваться к преимуществам каждого конструктивного фактора, и каждый пользовательский интерфейс может быть настроен для обеспечения лучшей работы.

<a name="size-classes" />

## <a name="size-classes"></a>Размер классов

До iOS 8 разработчик использовал `UIInterfaceOrientation` и `UIInterfaceIdiom` для различения книжных и альбомных режимов, а также между устройствами iPhone и iPad. В iOS8 ориентация и устройство определяются с помощью *классов размера*.

Устройства определяются классами размера, как вертикальными, так и горизонтальными осями, и в iOS 8 существует два типа классов размеров:

- **Regular** — это для большого размера экрана (например, iPad) или мини-приложения, которое дает впечатление большого размера (например, `UIScrollView`
- **Compact** — для устройств меньшего размера (например, iPhone). Этот размер учитывает ориентацию устройства.

Если два понятия используются совместно, то результатом будет сетка 2 x 2, которая определяет различные возможные размеры, которые можно использовать как в различных ориентациях, так и на схеме ниже.

 [![](unified-storyboards-images/sizeclassgrid.png "A 2 x 2 grid that defines the different possible sizes that can be used in Regular and Compact orientations")](unified-storyboards-images/sizeclassgrid.png#lightbox)

Разработчик может создать контроллер представления, использующий любую из четырех возможностей, которые могут привести к различным макетам (как показано на рисунке выше).

### <a name="ipad-size-classes"></a>Классы размера iPad

Для iPad в связи с размером существует **регулярный** размер класса для обеих ориентаций.

 [![](unified-storyboards-images/image1.png "iPad Size Classes")](unified-storyboards-images/image1.png#lightbox)

### <a name="iphone-size-classes"></a>Классы размера iPhone

У iPhone разные классы размеров в зависимости от ориентации устройства:

 [![](unified-storyboards-images/iphonesizeclasses.png "iPhone Size Classes")](unified-storyboards-images/iphonesizeclasses.png#lightbox)

- Когда устройство находится в книжной ориентации **, горизонтальный и** вертикальный экран содержит класс **Compact**
- Когда устройство находится в альбомном режиме, классы экрана отменяются от книжного режима.

### <a name="iphone-6-plus-size-classes"></a>Классы размера iPhone 6 Plus

Размеры совпадают со старыми iPhone, когда в книжной ориентации используется разная ориентация:

[![](unified-storyboards-images/iphone6sizeclasses.png "iPhone 6 Plus Size Classes")](unified-storyboards-images/iphone6sizeclasses.png#lightbox)

Так как iPhone 6 Plus имеет достаточно большой экран, он может иметь обычный класс размера ширины в альбомном режиме.

### <a name="support-for-a-new-screen-scale"></a>Поддержка нового масштаба экрана

В iPhone 6 Plus используется новый экран Retina HD с коэффициентом масштабирования экрана 3,0 (в три раза больше исходного разрешения экрана iPhone). Чтобы обеспечить оптимальную работу на этих устройствах, включите новую иллюстрацию, предназначенную для этого масштаба экрана. В Xcode 6 и более поздних версиях каталоги активов могут содержать изображения в размере 1x, 2x и 3 раза. просто добавьте новые ресурсы изображений и iOS выберет правильные активы при запуске на iPhone 6 Plus.

Поведение загрузки изображений в iOS также распознает суффикс `@3x` для файлов изображений. Например, если разработчик включает ресурс изображения (с разными разрешениями) в пакете приложения со следующими именами файлов: `MonkeyIcon.png`, `MonkeyIcon@2x.png`и `MonkeyIcon@3x.png`. На iPhone 6, а образ `MonkeyIcon@3x.png` будет использоваться автоматически, если разработчик загрузит образ с помощью следующего кода:

```csharp
UIImage icon = UIImage.FromFile("MonkeyImage.png");
```

Или если назначить изображение элементу пользовательского интерфейса с помощью конструктора iOS как `MonkeyIcon.png`, `MonkeyIcon@3x.png` будет использоваться автоматически, на iPhone 6 Plus.

<a name="dynamic-launch-screens" />

### <a name="dynamic-launch-screens"></a>Экраны динамического запуска

Файл экрана запуска отображается как экран-заставка во время запуска приложения iOS для предоставления отзыва пользователю о том, что приложение фактически запускается. До iOS 8 разработчику пришлось включать несколько ресурсов `Default.png` изображений для каждого типа устройства, ориентации и разрешения экрана, на которых будет выполняться приложение.

В iOS 8 разработчик может создать один атомарный файл `.xib` в Xcode, использующий классы автоматического макета и размера для создания *динамического экрана запуска* , который будет работать для каждого устройства, разрешения и ориентации. Это не только сокращает объем работы, необходимой разработчику для создания и обслуживания всех необходимых ресурсов изображений, но уменьшает размер установленного пакета приложения.

## <a name="traits"></a>Признаки

Признаки — это свойства, которые можно использовать для определения способа изменения макета при изменении его среды. Они состоят из набора свойств (`HorizontalSizeClass` и `VerticalSizeClass` на основе `UIUserInterfaceSizeClass`), а также идиомы интерфейса (`UIUserInterfaceIdiom`) и шкалы экрана.

Все перечисленные выше состояния упаковываются в контейнер, на который Apple ссылается как коллекция признаков (`UITraitCollection`), которая содержит не только свойства, но и их значения.

## <a name="trait-environment"></a>Среда признаков

Среды признаков являются новым интерфейсом в iOS 8 и могут возвращать коллекцию признаков для следующих объектов:

- Экраны (`UIScreens`).
- Windows (`UIWindows`).
- Контроллеры представления (`UIViewController`).
- Представления (`UIView`).
- Контроллер представления (`UIPresentationController`).

Разработчик использует коллекцию признаков, возвращаемую средой признаков, чтобы определить, как должен быть размещен пользовательский интерфейс.

Все среды признаков представляют иерархию, как показано на следующей схеме:

 [![](unified-storyboards-images/viewhierarchy.png "The Trait Environments hierarchy diagram")](unified-storyboards-images/viewhierarchy.png#lightbox)

Коллекция признаков, которую по умолчанию передаются каждой из вышеперечисленных окружений из родительской среды в дочернюю.

Помимо получения текущей коллекции признаков, в среде имеется метод `TraitCollectionDidChange`, который можно переопределить в подклассах View или View контроллера. Разработчик может использовать этот метод для изменения любого из элементов пользовательского интерфейса, зависящих от признаков, при изменении этих характеристик.

## <a name="typical-trait-collections"></a>Типичные коллекции признаков

В этом разделе рассматриваются типичные типы коллекций признаков, которые пользователь будет испытывать при работе с iOS 8.

Ниже приведена типичная коллекция признаков, которую разработчик может видеть на iPhone:

|Свойство|Значение|
|--- |--- |
|`HorizontalSizeClass`|Сжатие|
|`VerticalSizeClass`|Обычный|
|`UserInterfaceIdom`|Номер телефона|
|`DisplayScale`|2.0|

Приведенный выше набор будет представлять собой полную коллекцию признаков, так как она содержит значения для всех свойств признаков.

Также возможно наличие коллекции признаков, в которой отсутствуют некоторые из ее значений (которые Apple ссылается как *неопределенное*):

|Свойство|Значение|
|--- |--- |
|`HorizontalSizeClass`|Сжатие|
|`VerticalSizeClass`|Не указано|
|`UserInterfaceIdom`|Не указано|
|`DisplayScale`|Не указано|

Однако, как правило, когда разработчик запрашивает среду признаков для коллекции признаков, она возвращает полную коллекцию, как показано в примере выше.

Если среда признаков (например, представление или контроллер представления) не находится внутри текущей иерархии представления, разработчик может получить неуказанные значения для одного или нескольких свойств признаки.

Разработчик также будет получать частично квалифицированную коллекцию признаков, если они используют один из методов создания, предоставляемых Apple, например `UITraitCollection.FromHorizontalSizeClass`, для создания новой коллекции.

Одна операция, которая может быть выполнена над несколькими коллекциями признаков, сравнивает их друг с другом, что включает в себя запрос одной коллекции признаков, если она содержит другой набор характеристик. *Это означает* , что для всех характеристик, указанных во второй коллекции, значение должно точно совпадать со значением в первой коллекции.

Чтобы проверить две признаки, используйте метод `Contains` `UITraitCollection` передав значение проверяемой характеристики.

Разработчик может вручную выполнять сравнения в коде, чтобы определить, как разметка представлений или контроллеров представлений. Однако `UIKit` использует этот метод для внутренних целей, как в случае с прокси-интерфейсом внешнего вида, например.

## <a name="appearance-proxy"></a>Прокси-сервер внешнего вида

Прокси-сервер внешнего вида появился в более ранних версиях iOS, что позволяет разработчикам настраивать свойства их представлений. Он был расширен в iOS 8 для поддержки коллекций признаков.

Прокси-серверы оформления теперь включают новый метод, `AppearanceForTraitCollection`, который возвращает новый прокси-сервер внешнего вида для заданной передаваемой коллекции признаков. Любые настройки, выполняемые разработчиком на этом прокси-сервере, вступят в силу только для представлений, которые соответствуют заданной коллекции признаков.

Как правило, разработчик передает частично определенную коллекцию признаков методу `AppearanceForTraitCollection`, например, только что задали класс горизонтального размера Compact, чтобы они могли настраивать любое представление в приложении, которое выполняется горизонтально.

## <a name="uiimage"></a>уиимаже

Другой класс, к которому добавлена коллекция признаков Apple, — `UIImage`. В прошлом разработчику пришлось указать @1X и @2x версию любого ресурса точечного рисунка, который будет включен в приложение (например, значок).

iOS 8 расширена, чтобы разработчик включил в каталог образов несколько версий образа на основе коллекции признаков. Например, разработчик может добавить небольшое изображение для работы с классом признаков сжатия и изображением полного размера для любой другой коллекции.

Если одно из изображений используется внутри класса `UIImageView`, представление изображений автоматически отображает правильную версию изображения для коллекции признаков. Если среда признаков изменяется (например, если пользователь переключает устройство с книжной на альбомную), представление изображений автоматически выбирает новый размер изображения в соответствии с новой коллекцией признаков и изменяет его размер в соответствии с текущей версией изображения. отображаем.

## <a name="uiimageasset"></a>уиимажеассет

Компания Apple добавила в iOS 8 новый класс, именуемый `UIImageAsset`, чтобы предоставить разработчику еще больший контроль над выбором изображения.

Ресурс изображения создает оболочку для всех различных версий образа и позволяет разработчику запрашивать конкретный образ, соответствующий переданной коллекции признаков. Изображения можно добавлять или удалять из ресурса изображения на лету.

Дополнительные сведения о ресурсах изображений см. в документации Apple [уиимажеассет](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIImageAsset_Ref/index.html#//apple_ref/occ/cl/UIImageAsset) .

## <a name="combining-trait-collections"></a>Объединение коллекций признаков

Еще одна функция, которую разработчик может выполнять с коллекциями признаков, — добавить два объединения, которые будут приводить к объединенной коллекции, где неуказанные значения из одной коллекции заменяются указанными значениями во второй. Это делается с помощью метода `FromTraitsFromCollections` класса `UITraitCollection`.

Как упоминалось выше, если какая-либо из признаков не указана в одной из коллекций признаков и указана в другом, значение будет задано для указанной версии. Однако если указано несколько версий заданного значения, то значение из последней коллекции признаков будет использоваться в качестве значения.

## <a name="adaptive-view-controllers"></a>Адаптивные контроллеры представлений

В этом разделе рассматриваются сведения о том, как представление iOS и контроллеры представлений применяют основные понятия характеристик и классы размеров для автоматического адаптивного использования в приложениях разработчика.

### <a name="split-view-controller"></a>Контроллер разделенного представления

Одним из классов контроллеров представлений, которые изменились в iOS 8, является класс `UISplitViewController`. В прошлом разработчику часто приходилось использовать контроллер разделенного представления в версии приложения для iPad, а затем ему пришлось бы предоставить совершенно другую версию иерархии представлений для iPhone-версии приложения.

В iOS 8 класс `UISplitViewController` доступен на обеих платформах (iPad и iPhone), что позволяет разработчику создать одну иерархию контроллеров представлений, которая будет работать как для iPhone, так и для iPad.

Если устройство iPhone находится в альбомной ориентации, контроллеры с разделением представлений параллельно представляют свои представления, так же как и при отображении на iPad.

### <a name="overriding-traits"></a>Переопределяющие признаки

Окружения признака каскадом из родительского контейнера вниз к дочерним контейнерам, как показано на следующем рисунке, где показан контроллер с разделенным представлением на iPad в альбомной ориентации:

 [![](unified-storyboards-images/cascadingclasses01.png "A Split View Controller on an iPad in the landscape orientation")](unified-storyboards-images/cascadingclasses01.png#lightbox)

Так как у iPad есть обычный класс размера как для горизонтальной, так и для вертикальной ориентации, в представлении с разделением будут отображаться как главное, так и подробное представления.

На iPhone, где класс Size сжимается в обеих направлениях, контроллер с разделением представлений отображает только подробное представление, как показано ниже:

 [![](unified-storyboards-images/cascadingclasses02.png "The Split View Controller only displays the detail view")](unified-storyboards-images/cascadingclasses02.png#lightbox)

В приложении, в котором разработчику нужно отобразить как главное представление, так и подробное описание на устройстве iPhone в альбомной ориентации, разработчик должен вставить родительский контейнер для контроллера разделенного представления и переопределить коллекцию признаков. Как показано на рисунке ниже:

 [![](unified-storyboards-images/cascadingclasses03.png "The developer must insert a parent container for the Split View Controller and override the Trait Collection")](unified-storyboards-images/cascadingclasses03.png#lightbox)

`UIView` задается в качестве родителя контроллера разделенного представления, а метод `SetOverrideTraitCollection` вызывается в представлении, которое передается в новой коллекции признаков и предназначено для контроллера разделенного представления. Новая коллекция признаков переопределяет `HorizontalSizeClass`, присвоив ей значение `Regular`, чтобы контроллер с разделением представлений отображал как главное представление, так и представление сведений на устройстве iPhone в альбомной ориентации.

Обратите внимание, что `VerticalSizeClass` был установлен в `unspecified`, что позволяет добавлять новую коллекцию признаков в коллекцию признаков в родительском объекте, что приводит к `Compact VerticalSizeClass` для контроллера дочернего представления с разделением.

### <a name="trait-changes"></a>Изменения характеристик

В этом разделе подробно рассматривается, как переходят коллекции признаков при изменении среды признаков. Например, при повороте устройства с книжной на альбомную.

 [![](unified-storyboards-images/traittransitions01.png "The portrait to landscape Trait Changes overview")](unified-storyboards-images/traittransitions01.png#lightbox)

Во первых, iOS 8 выполняет некоторые настройки для подготовки к переходу на выполнение перехода. Затем система выполняет анимацию состояния перехода. Наконец, iOS 8 очищает временные состояния, необходимые во время перехода.

iOS 8 предоставляет несколько обратных вызовов, которые разработчик может использовать для участия в изменении характеристик, как показано в следующей таблице.

|Этап|Обратный вызов|Description|
|--- |--- |--- |
|Настройка|<ul><li>`WillTransitionToTraitCollection`</li><li>`TraitCollectionDidChange`</li></ul>|<ul><li>Этот метод вызывается в начале изменения признака до того, как для коллекции признаков задается новое значение.</li><li>Метод вызывается, когда значение коллекции признаков изменилось, но до того, как будет выполнена анимация.</li></ul>|
|Анимация|`WillTransitionToTraitCollection`|Координатор переходов, который передается в этот метод, имеет свойство `AnimateAlongside`, позволяющее разработчику добавлять анимации, которые будут выполняться вместе с анимацией по умолчанию.|
|Очистка|`WillTransitionToTraitCollection`|Предоставляет разработчикам способ включения собственного кода очистки после перехода.|

Метод `WillTransitionToTraitCollection` отлично подходит для анимации контроллеров представлений вместе с изменениями коллекции признаков. Метод `WillTransitionToTraitCollection` доступен только на контроллерах представления (`UIViewController`), а не в других средах признаков, таких как `UIViews`.

`TraitCollectionDidChange` отлично подходит для работы с классом `UIView`, где разработчик хочет обновить пользовательский интерфейс при изменении характеристик.

### <a name="collapsing-the-split-view-controllers"></a>Сворачивание контроллеров разделенного представления

Теперь рассмотрим более подробное рассмотрение того, что происходит, когда контроллер с разделением представления сворачивается из двух столбцов в представление одного столбца. В рамках этого изменения необходимо выполнить два процесса:

- По умолчанию контроллер с разделением представлений будет использовать первичный контроллер представления в качестве представления после сворачивания. Разработчик может переопределить это поведение, переопределив метод `GetPrimaryViewControllerForCollapsingSplitViewController` `UISplitViewControllerDelegate` и предоставив любой контроллер представления, который должен отображаться в свернутом состоянии.
- Вторичный контроллер представления должен быть объединен в первичный контроллер представления. Как правило, разработчику не нужно предпринимать никаких действий для этого шага; Контроллер разделенного представления включает автоматическую обработку этого этапа на основе аппаратного устройства. Однако могут возникнуть некоторые особые случаи, когда разработчику нужно будет взаимодействовать с этим изменением. Вызов метода `CollapseSecondViewController` `UISplitViewControllerDelegate` позволяет отображать контроллер главного представления при свертывании, а не в подробном представлении.

### <a name="expanding-the-split-view-controller"></a>Развертывание контроллера разделенного представления

Теперь рассмотрим более подробное рассмотрение того, что происходит, когда контроллер с разделенным представлением развернут из свернутого состояния. Опять же, необходимо выполнить два этапа:

- Сначала определите новый первичный контроллер представления. По умолчанию контроллер с разделением представлений будет автоматически использовать первичный контроллер представления из свернутого представления. Опять же, разработчик может переопределить это поведение с помощью метода `GetPrimaryViewControllerForExpandingSplitViewController` `UISplitViewControllerDelegate`.
- После выбора первичного контроллера представления его необходимо создать заново. Опять же, контроллер разделенного представления включает автоматическую обработку этого этапа на основе аппаратного устройства. Разработчик может переопределить это поведение, вызвав метод `SeparateSecondaryViewController` `UISplitViewControllerDelegate`.

В контроллере с разделенным представлением первичный контроллер представления играет часть как в развертывании, так и в свертывании представлений, реализуя методы `CollapseSecondViewController` и `SeparateSecondaryViewController` `UISplitViewControllerDelegate`. `UINavigationController` реализует эти методы, чтобы автоматически отправлять и открывать дополнительные контроллеры представлений.

### <a name="showing-view-controllers"></a>Отображение контроллеров представления

Еще одно изменение, внесенное Apple в iOS 8, — это способ, которым разработчик показывает контроллеры представления. В прошлом, если в приложении имелся контроллер конечного представления (например, контроллер представления таблицы), а разработчик показал другое (например, в ответ на нажатие пользователем ячейки), приложение будет обращаться к контроллеру представления навигации и вызвать метод `PushViewController` для него, чтобы отобразить новое представление.

Это представляло собой очень тесное связывание между контроллером навигации и средой, в которой он выполнялся. В iOS 8 компания Apple разделяет это, предоставляя два новых метода:

- `ShowViewController` — адаптируется для отображения нового контроллера представления на основе его среды. Например, в `UINavigationController` он просто отправляет новое представление в стек. В контроллере разделенного представления новый контроллер представления будет представлен слева в качестве нового первичного контроллера представления. Если контроллер представления контейнера отсутствует, новое представление будет отображаться как модальный контроллер представления.
- `ShowDetailViewController` — работает аналогично `ShowViewController`, но реализуется на контроллере разделенного представления, чтобы заменить представление Details новым передаваемым контроллером представления. Если контроллер разделенного представления свернут (как можно увидеть в приложении iPhone), вызов будет перенаправлен к методу `ShowViewController`, а новое представление будет отображаться как первичный контроллер представления. Опять же, если контроллер представления контейнера отсутствует, новое представление будет отображаться как модальный контроллер представления.

Эти методы работают путем запуска на контроллере конечного представления и проходят по иерархии представлений до тех пор, пока не найдут подходящий контроллер представления контейнера для отображения нового представления.

Разработчики могут реализовать `ShowViewController` и `ShowDetailViewController` в собственных настраиваемых контроллерах представлений, чтобы получить те же автоматизированные функциональные возможности, которые предоставляет `UINavigationController` и `UISplitViewController`.

### <a name="how-it-works"></a>Принцип работы

В этом разделе мы рассмотрим, как эти методы фактически реализуются в iOS 8. Сначала давайте рассмотрим новый метод `GetTargetForAction`:

 [![](unified-storyboards-images/gettargetforaction.png "The new GetTargetForAction method")](unified-storyboards-images/gettargetforaction.png#lightbox)

Этот метод просматривает цепочку иерархии до тех пор, пока не будет найден правильный контроллер представления контейнера. Пример:

1. Если вызывается метод `ShowViewController`, первый контроллер представления в цепочке, реализующий этот метод, является контроллером навигации, поэтому он используется в качестве родителя нового представления.
1. Если вместо этого был вызван метод `ShowDetailViewController`, контроллер с разделением представлений является первым контроллером представления для его реализации, поэтому он используется в качестве родительского.

Метод `GetTargetForAction` работает путем обнаружения контроллера представления, который реализует данное действие, и последующего запроса этого контроллера представления, если он хочет получить это действие. Поскольку этот метод является открытым, разработчики могут создавать собственные пользовательские методы, которые работают так же, как и встроенные методы `ShowViewController` и `ShowDetailViewController`.

## <a name="adaptive-presentation"></a>Адаптивная презентация

В iOS 8 компания Apple сделала контекстном меню Action презентации (`UIPopoverPresentationController`) адаптивно. Так что контроллер представления представления контекстном меню Action автоматически предложит обычное представление контекстном меню Action в обычном классе размера, но будет отображать его во весь экран в классе горизонтального размера (например, на iPhone).

Чтобы внести изменения в единую систему раскадровки, создан новый объект контроллера для управления представленными контроллерами представления — `UIPresentationController`. Этот контроллер создается с момента представления контроллера представления до его закрытия. Так как это управляемый класс, он может рассматриваться как супер-класс на контроллере представления, так как он реагирует на изменения устройства, влияющие на контроллер представления (например, ориентацию), который затем возвращается в контроллер представления элементов управления контроллера представления.

Когда разработчик представляет контроллер представления с помощью метода `PresentViewController`, управление процессом презентации выполняется `UIKit`. UIKit обрабатывает (помимо прочего) правильный контроллер для создаваемого стиля с единственным исключением, если для контроллера представления задан стиль `UIModalPresentationCustom`. Здесь приложение может предоставить собственный Пресентатионконтроллер, а не использовать контроллер `UIKit`.

### <a name="custom-presentation-styles"></a>Пользовательские стили представления

При использовании пользовательского стиля представления разработчики могут использовать пользовательский контроллер представления. Этот настраиваемый контроллер можно использовать для изменения внешнего вида и поведения представления, на которое он allied.

<a name="size-classes"/>

## <a name="working-with-size-classes"></a>Работа с классами размера

Проект Xamarin Photo фото, включенный в эту статью, содержит рабочий пример использования классов размера и адаптивных контроллеров представлений в приложении единого интерфейса iOS 8.

Хотя приложение полностью создает пользовательский интерфейс из кода, в отличие от использования конструктора IOS и создания унифицированной раскадровки, применяются те же методы. Далее в этой статье мы покажем, как использовать классы размеров с унифицированной раскадровкой и конструктором iOS в приложении Xamarin.

Теперь подробнее рассмотрим, как проект адаптивных фотографий реализует несколько функций класса Size в iOS 8 для создания адаптивного приложения.

### <a name="adapting-to-trait-environment-changes"></a>Адаптация к изменениям среды

При запуске приложения адаптивных фотографий на iPhone, когда пользователь поворачивает устройство с книжной на альбомную, контроллер разделенного представления будет отображать как главное, так и подробное представление:

 [![](unified-storyboards-images/rotation.png "The Split View Controller will display both the master and details view as seen here")](unified-storyboards-images/rotation.png#lightbox)

Это достигается путем переопределения метода `UpdateConstraintsForTraitCollection` контроллера представления и настройки ограничений на основе значения `VerticalSizeClass`. Пример:

```csharp
public void UpdateConstraintsForTraitCollection (UITraitCollection collection)
{
    var views = NSDictionary.FromObjectsAndKeys (
        new object[] { TopLayoutGuide, ImageView, NameLabel, ConversationsLabel, PhotosLabel },
        new object[] { "topLayoutGuide", "imageView", "nameLabel", "conversationsLabel", "photosLabel" }
    );

    var newConstraints = new List<NSLayoutConstraint> ();
    if (collection.VerticalSizeClass == UIUserInterfaceSizeClass.Compact) {
        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("|[imageView]-[nameLabel]-|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("[imageView]-[conversationsLabel]-|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("[imageView]-[photosLabel]-|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("V:|[topLayoutGuide]-[nameLabel]-[conversationsLabel]-[photosLabel]",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("V:|[topLayoutGuide][imageView]|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.Add (NSLayoutConstraint.Create (ImageView, NSLayoutAttribute.Width, NSLayoutRelation.Equal,
            View, NSLayoutAttribute.Width, 0.5f, 0.0f));
    } else {
        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("|[imageView]|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("|-[nameLabel]-|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("|-[conversationsLabel]-|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("|-[photosLabel]-|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("V:[topLayoutGuide]-[nameLabel]-[conversationsLabel]-[photosLabel]-20-[imageView]|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));
    }

    if (constraints != null)
        View.RemoveConstraints (constraints.ToArray ());

    constraints = newConstraints;
    View.AddConstraints (constraints.ToArray ());
}
```

### <a name="adding-transition-animations"></a>Добавление анимации переходов

Когда контроллер разделенного представления в приложении адаптивных фотографий переходит из свернутого в развернутый, в анимацию по умолчанию добавляются анимации путем переопределения метода `WillTransitionToTraitCollection` контроллера представления. Пример:

```csharp
public override void WillTransitionToTraitCollection (UITraitCollection traitCollection, IUIViewControllerTransitionCoordinator coordinator)
{
    base.WillTransitionToTraitCollection (traitCollection, coordinator);
    coordinator.AnimateAlongsideTransition ((UIViewControllerTransitionCoordinatorContext) => {
        UpdateConstraintsForTraitCollection (traitCollection);
        View.SetNeedsLayout ();
    }, (UIViewControllerTransitionCoordinatorContext) => {
    });
}
```

### <a name="overriding-the-trait-environment"></a>Переопределение среды признаков

Как показано выше, приложение адаптивных фотографий заставляет контроллер разделенного представления отображать как сведения, так и основные представления, если устройство iPhone находится в альбомном представлении.

Это было выполнено с помощью следующего кода в контроллере представления:

```csharp
private UITraitCollection forcedTraitCollection = new UITraitCollection ();
...

public UITraitCollection ForcedTraitCollection {
    get {
        return forcedTraitCollection;
    }

    set {
        if (value != forcedTraitCollection) {
            forcedTraitCollection = value;
            UpdateForcedTraitCollection ();
        }
    }
}
...

public override void ViewWillTransitionToSize (SizeF toSize, IUIViewControllerTransitionCoordinator coordinator)
{
    ForcedTraitCollection = toSize.Width > 320.0f ?
         UITraitCollection.FromHorizontalSizeClass (UIUserInterfaceSizeClass.Regular) :
         new UITraitCollection ();

    base.ViewWillTransitionToSize (toSize, coordinator);
}

public void UpdateForcedTraitCollection ()
{
    SetOverrideTraitCollection (forcedTraitCollection, viewController);
}
```

### <a name="expanding-and-collapsing-the-split-view-controller"></a>Развертывание и свертывание контроллера разделенного представления

Далее давайте рассмотрим, как развернутое и сворачивание поведение контроллера разделенного представления было реализовано в Xamarin. В `AppDelegate`при создании контроллера разделенного представления ему назначается его делегат для выполнения этих изменений:

```csharp
public class SplitViewControllerDelegate : UISplitViewControllerDelegate
{
    public override bool CollapseSecondViewController (UISplitViewController splitViewController,
        UIViewController secondaryViewController, UIViewController primaryViewController)
    {
        AAPLPhoto photo = ((CustomViewController)secondaryViewController).Aapl_containedPhoto (null);
        if (photo == null) {
            return true;
        }

        if (primaryViewController.GetType () == typeof(CustomNavigationController)) {
            var viewControllers = new List<UIViewController> ();
            foreach (var controller in ((UINavigationController)primaryViewController).ViewControllers) {
                var type = controller.GetType ();
                MethodInfo method = type.GetMethod ("Aapl_containsPhoto");

                if ((bool)method.Invoke (controller, new object[] { null })) {
                    viewControllers.Add (controller);
                }
            }

            ((UINavigationController)primaryViewController).ViewControllers = viewControllers.ToArray<UIViewController> ();
        }

        return false;
    }

    public override UIViewController SeparateSecondaryViewController (UISplitViewController splitViewController,
        UIViewController primaryViewController)
    {
        if (primaryViewController.GetType () == typeof(CustomNavigationController)) {
            foreach (var controller in ((CustomNavigationController)primaryViewController).ViewControllers) {
                var type = controller.GetType ();
                MethodInfo method = type.GetMethod ("Aapl_containedPhoto");

                if (method.Invoke (controller, new object[] { null }) != null) {
                    return null;
                }
            }
        }

        return new AAPLEmptyViewController ();
    }
}
```

Метод `SeparateSecondaryViewController` проверяет, отображается ли фотография, и принимает действия на основе этого состояния. Если фотография не отображается, она сворачивает дополнительный контроллер представления, чтобы отображался контроллер главного представления.

Метод `CollapseSecondViewController` используется при разворачивании контроллера разделенного представления, чтобы узнать, существуют ли фотографии в стеке, если это так, и она сворачивается обратно в это представление.

### <a name="moving-between-view-controllers"></a>Перемещение между контроллерами представления

Теперь давайте посмотрим, как приложение адаптивных фотографий перемещается между контроллерами представления. В классе `AAPLConversationViewController`, когда пользователь выбирает ячейку из таблицы, вызывается метод `ShowDetailViewController` для отображения подробного представления:

```csharp
public override void RowSelected (UITableView tableView, NSIndexPath indexPath)
{
    var photo = PhotoForIndexPath (indexPath);
    var controller = new AAPLPhotoViewController ();
    controller.Photo = photo;

    int photoNumber = indexPath.Row + 1;
    int photoCount = (int)Conversation.Photos.Count;
    controller.Title = string.Format ("{0} of {1}", photoNumber, photoCount);
    ShowDetailViewController (controller, this);
}
```

### <a name="displaying-disclosure-indicators"></a>Отображение индикаторов раскрытия

В приложении адаптивного фото есть несколько мест, где индикаторы раскрытия скрыты или показаны на основе изменений в среде признаков. Это обрабатывается с помощью следующего кода:

```csharp
public bool Aapl_willShowingViewControllerPushWithSender ()
{
    var selector = new Selector ("Aapl_willShowingViewControllerPushWithSender");
    var target = this.GetTargetViewControllerForAction (selector, this);

    if (target != null) {
        var type = target.GetType ();
        MethodInfo method = type.GetMethod ("Aapl_willShowingDetailViewControllerPushWithSender");
        return (bool)method.Invoke (target, new object[] { });
    } else {
        return false;
    }
}

public bool Aapl_willShowingDetailViewControllerPushWithSender ()
{
    var selector = new Selector ("Aapl_willShowingDetailViewControllerPushWithSender");
    var target = this.GetTargetViewControllerForAction (selector, this);

    if (target != null) {
        var type = target.GetType ();
        MethodInfo method = type.GetMethod ("Aapl_willShowingDetailViewControllerPushWithSender");
        return (bool)method.Invoke (target, new object[] { });
    } else {
        return false;
    }
}
```

Они реализуются с помощью метода `GetTargetViewControllerForAction`, рассмотренного выше.

Когда контроллер представления таблицы отображает данные, он использует методы, реализованные выше, чтобы определить, происходит ли принудительная отправка, а также следует ли отображать или скрывать индикатор раскрытия соответствующим образом:

```csharp
public override void WillDisplay (UITableView tableView, UITableViewCell cell, NSIndexPath indexPath)
{
    bool pushes = ShouldShowConversationViewForIndexPath (indexPath) ?
         Aapl_willShowingViewControllerPushWithSender () :
         Aapl_willShowingDetailViewControllerPushWithSender ();

    cell.Accessory = pushes ? UITableViewCellAccessory.DisclosureIndicator : UITableViewCellAccessory.None;
    var conversation = ConversationForIndexPath (indexPath);
    cell.TextLabel.Text = conversation.Name;
}
```

### <a name="new-showdetailtargetdidchangenotification-type"></a>Новый тип `ShowDetailTargetDidChangeNotification`

Компания Apple добавила новый тип уведомлений для работы с классами размеров и характеристиками из контроллера разделенного представления `ShowDetailTargetDidChangeNotification`. Это уведомление отправляется при каждом изменении целевого представления для контроллера разделенного представления, например при развертывании или сворачивании контроллера.

Приложение адаптивных фотографий использует это уведомление для обновления состояния индикатора раскрытия при изменении контроллера подробного представления:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();
    TableView.RegisterClassForCellReuse (typeof(UITableViewCell), AAPLListTableViewControllerCellIdentifier);
    NSNotificationCenter.DefaultCenter.AddObserver (this, new Selector ("showDetailTargetDidChange:"),
        UIViewController.ShowDetailTargetDidChangeNotification, null);
    ClearsSelectionOnViewWillAppear = false;
}
```

Внимательно просмотрите приложение адаптивных фотографий, чтобы увидеть все способы, с помощью которых классы размеров, коллекции признаков и адаптивные контроллеры представлений можно использовать для простого создания унифицированного приложения в Xamarin. iOS.

## <a name="unified-storyboards"></a>Унифицированные раскадровки

В iOS 8 унифицированные раскадровки позволяют разработчику создавать один унифицированный файл раскадровки, который может отображаться на устройствах iPhone и iPad, нацеливание на несколько классов размеров. Используя унифицированные раскадровки, разработчик записывает меньше кода, специфичного для пользовательского интерфейса, и имеет только одну конструкцию интерфейса для создания и обслуживания.

Основные преимущества Объединенных раскадровок:

- Используйте один и тот же файл раскадровки для iPhone и iPad.
- Развертывайте обратно в iOS 6 и iOS 7.
- Просмотрите макет для различных устройств, ориентации и версий ОС в конструкторе Xamarin iOS.

Эта функция полностью поддерживается в Visual Studio для Mac

<a name="enabling-size-classes" />

### <a name="enabling-size-classes"></a>Включение классов размера

По умолчанию каждый новый проект Xamarin. iOS будет изменять размер классов. Чтобы использовать классы размера и адаптивные переходов внутри раскадровки из старого проекта, сначала необходимо преобразовать его в формат унифицированной раскадровки Xcode 6 из конструктора iOS.

Для этого откройте раскадровку для преобразования в конструкторе iOS и установите флажок " **использовать классы размера** ":

 [![](unified-storyboards-images/sizeclass01.png "The Use Size Classes check box")](unified-storyboards-images/sizeclass01.png#lightbox)

Конструктор iOS подтверждает, что разработчик хочет преобразовать формат раскадровки для использования классов размера:

 [![](unified-storyboards-images/sizeclass02.png "The use Size Classes alert")](unified-storyboards-images/sizeclass02.png#lightbox)

> [!IMPORTANT]
> Для правильной работы классы размеров также должны быть проверены на наличие автоматического макета.

### <a name="generic-device-types"></a>Универсальные типы устройств

После преобразования раскадровки для использования классов размера она будет отображаться в область конструктора и **представление как** устройство будет универсальным:

 [![](unified-storyboards-images/sizeclass03.png "View as a Generic device type")](unified-storyboards-images/sizeclass03.png#lightbox)

При выборе универсального типа устройства размер всех контроллеров представления изменится на квадрат 600 x 600. Этот квадрат представляет размеры любой ширины и любой высоты. Если конструктор iOS находится в этом режиме, все изменения будут применены ко всем классам размера.

Разработчик также имеет возможность просматривать область конструктора как iPhone:

 [![](unified-storyboards-images/sizeclass04.png "Viewing the design surface as an iPhone")](unified-storyboards-images/sizeclass04.png#lightbox)

Или просмотреть его как iPad:

 [![](unified-storyboards-images/sizeclass05.png "Viewing the design surface as an iPad")](unified-storyboards-images/sizeclass05.png#lightbox)

### <a name="select-a-size-class"></a>Выберите класс размера

Кнопка выбора класса Size находится в левом верхнем углу область конструктора (рядом с представлением в виде раскрывающегося списка). Он позволяет разработчику выбрать, какие классы размеров в данный момент редактируются:

 [![](unified-storyboards-images/sizeclass06.png "Select a Size Class")](unified-storyboards-images/sizeclass06.png#lightbox)

Селектор отображает выбранный класс размера как сетку 3 x 3. Каждый из квадратов в сетке представляет сочетание класса Width и класса Height. Центральный квадрат выбирает любой класс размера ширины или высоты (который является представлением по умолчанию для унифицированной раскадровки). Если этот квадрат выбран, разработчик редактирует макет по умолчанию, который наследуется всеми другими конфигурациями.

Квадрат в левом верхнем углу сетки представляет класс размера Compact Width/Compact высота:

 [![](unified-storyboards-images/sizeclass07.png "The Compact Width/Compact Height Size Class")](unified-storyboards-images/sizeclass07.png#lightbox)

Этот режим соответствует устройству iPhone в альбомной ориентации. Квадрат в правом нижнем углу сетки представляет собой класс обычной ширины или размера обычной высоты, представляющий iPad:

 [![](unified-storyboards-images/sizeclass08.png "The Regular Width/Regular Height Size Class")](unified-storyboards-images/sizeclass08.png#lightbox)

Чтобы изменить макет для iPhone в книжной ориентации, выберите квадрат в левом нижнем углу. Это класс, представляющий размер компакт-и обычного размера высоты:

 [![](unified-storyboards-images/sizeclass09.png "The Compact Width/Regular Height Size Class")](unified-storyboards-images/sizeclass09.png#lightbox)

Щелкните квадрат, чтобы выделить его, и область конструктора изменит размер контроллеров представления в соответствии с новым выбором:

 [![](unified-storyboards-images/sizeclass10.png "The Design Surface will change the size of the View Controllers to match the new selection as shown")](unified-storyboards-images/sizeclass10.png#lightbox)

Дополнительные сведения о классах размера и о том, как они влияют на макеты для iPhone и iPad, см. в разделе "размер класса" этой статьи.

### <a name="adaptive-segue-types"></a>Адаптивные типы перехода

Если разработчик использовал раскадровки раньше, они будут знакомы с существующими типами перехода **push-уведомлений**, **модальными** и **контекстном меню Action**. Если для унифицированного файла раскадровки включены классы размера, становятся доступными следующие адаптивные типы перехода (которые соответствуют новому API контроллера представления, описанному выше): **Показать** и **Показать подробности**.

> [!IMPORTANT]
> Если включены классы размера, все существующие переходов будут преобразованы в новые типы.

Возьмем пример приложения iOS 8, которое использует унифицированную раскадровку с контроллером разделенного представления с простым меню навигации по игре в главном представлении. Если пользователь нажимает кнопку меню, контроллер представления выбранного элемента должен отображаться в разделе сведений контроллера разделенного представления при работе на iPad. На iPhone контроллер представления элемента должен быть помещен в стек навигации.

Чтобы добиться этого результата, в элементе управления iOS Designer щелкните кнопку и перетащите линию в отображаемый контроллер представления. Когда кнопка мыши будет отпущена, выберите `Show Detail` из всплывающего меню типа перехода:

 [![](unified-storyboards-images/segue01.png "Select Show Detail from the Segue Type Popup menu")](unified-storyboards-images/segue01.png#lightbox)

Новый перехода будет создан между кнопкой и контроллером представления. Теперь запустите приложение в симуляторе iPhone, и в главном меню отобразится следующее:

 [![](unified-storyboards-images/segue02.png "The Main Menu")](unified-storyboards-images/segue02.png#lightbox)

Нажмите кнопку **SELECT Game (выбрать игру** ), и контроллер представления элемента будет помещен в стек навигации:

 [![](unified-storyboards-images/segue03.png "The items View Controller will be pushed onto the Navigation Stack as shown")](unified-storyboards-images/segue03.png#lightbox)

Останавливает имитатор iPhone и запускает приложение в симуляторе iPad. Переключитесь на альбомную ориентацию, и главное меню снова отобразится:

 [![](unified-storyboards-images/segue04.png "The main menu displayed")](unified-storyboards-images/segue04.png#lightbox)

Опять же, нажмите кнопку **выбрать игру** , и контроллер представления элемента отобразится в разделе сведений контроллера разделенного представления:

 [![](unified-storyboards-images/segue05.png "The items View Controller shown in the Details section of the Split View Controller")](unified-storyboards-images/segue05.png#lightbox)

### <a name="excluding-an-element-from-a-size-class"></a>Исключение элемента из класса размера

Бывают случаи, когда данный элемент (например, представление, элемент управления или ограничение) не требуется в определенном классе размера. Чтобы исключить элемент из класса Size, выберите нужный элемент для исключения в **область конструктора**. Прокрутите окно вниз **обозревателя свойств** и перейдите в раскрывающееся меню **шестеренки** . Выберите сочетание **ширины** и **высоты** , чтобы исключить элемент из:

[![](unified-storyboards-images/exclude-a.png "Select the combination of Width and Height")](unified-storyboards-images/exclude-a.png#lightbox)

Новый *случай исключения* будет добавлен к элементу в нижней части **обозревателя свойств**. Затем снимите флажок **установлено** для данного класса размера:

[![](unified-storyboards-images/exclude-b.png "Uncheck the Installed checkbox")](unified-storyboards-images/exclude-b.png#lightbox)

Переключить область конструктора на ширину и высоту, из которой элемент был исключен из, он был удален из данного класса размера, но не весь дизайн пользовательского интерфейса:

 [![](unified-storyboards-images/exclude02.png "Switch the Design Surface to the Width and Height that the item was excluded from")](unified-storyboards-images/exclude02.png#lightbox)

Переключитесь обратно на любой из классов ширины или высоты, и элемент по-прежнему имеет место:

 [![](unified-storyboards-images/exclude03.png "Switching back to the Any Width/Any Height size class")](unified-storyboards-images/exclude03.png#lightbox)

Когда приложение запускается в симуляторе iPad, появляется элемент:

 [![](unified-storyboards-images/exclude04.png "The element shown when the running app in the iPad Simulator")](unified-storyboards-images/exclude04.png#lightbox)

И когда приложение запускается в симуляторе iPhone, элемент отсутствует:

 [![](unified-storyboards-images/exclude05.png "The element missing when the running app in the iPhone Simulator")](unified-storyboards-images/exclude05.png#lightbox)

Чтобы удалить исключение из элемента, просто выберите элемент в **область конструктора**, прокрутите его вниз до нижней части **обозревателя свойств** и нажмите кнопку **-** рядом с удаляемым вариантом.

Чтобы просмотреть реализацию унифицированных раскадровок, просмотрите `UnifiedStoryboard` пример приложения Xamarin iOS 8, прикрепленного к этому документу.

## <a name="dynamic-launch-screens"></a>Экраны динамического запуска

Файл экрана запуска отображается как экран-заставка во время запуска приложения iOS для предоставления отзыва пользователю о том, что приложение фактически запускается. До iOS 8 разработчику пришлось включать несколько ресурсов `Default.png` изображений для каждого типа устройства, ориентации и разрешения экрана, на которых будет выполняться приложение. Например, `Default@2x.png`, `Default-Landscape@2x~ipad.png`, `Default-Portrait@2x~ipad.png`и т. д.

В новых устройствах iPhone и iPhone 6 Plus (а также в предстоящем Apple Watch) со всеми существующими устройствами iPhone и iPad это представляет собой большой массив различных размеров, ориентации и разрешений `Default.png` ресурсов изображения экрана запуска, которые должны быть созданы и сохранены. Кроме того, эти файлы могут быть достаточно большими, что приведет к чрезмерному увеличению количества времени, необходимого для загрузки приложения из магазина iTunes (возможно, его можно будет доставить через сотовую сеть). и увеличение объема хранилища, необходимого для устройства конечного пользователя.

В iOS 8 разработчик может создать один атомарный файл `.xib` в Xcode, использующий классы автоматического макета и размера для создания *динамического экрана запуска* , который будет работать для каждого устройства, разрешения и ориентации. Это не только сокращает объем работы, необходимой разработчику для создания и обслуживания всех необходимых ресурсов изображений, но значительно сокращает размер установленного пакета приложения.

Экраны динамического запуска имеют следующие ограничения и рекомендации.

- Используйте только `UIKit` классы.
- Используйте одно корневое представление, которое является объектом `UIView` или `UIViewController`.
- Не делайте никаких подключений к коду приложения (не добавляйте **действия** или **розетки**).
- Не добавляйте `UIWebView` объекты.
- Не используйте пользовательские классы.
- Не используйте атрибуты среды выполнения.

Учитывая приведенные выше рекомендации, рассмотрим добавление динамического экрана запуска в существующий проект Xamarin iOS 8.

Выполните следующие действия.

1. Откройте **Visual Studio для Mac** и загрузите **решение** , чтобы добавить экран динамического запуска в.
2. В **Обозреватель решений**щелкните правой кнопкой мыши файл `MainStoryboard.storyboard` и выберите **открыть с помощью** > **Xcode Interface Builder**:

    [![](unified-storyboards-images/dls01.png "Open With Xcode Interface Builder")](unified-storyboards-images/dls01.png#lightbox)
3. В Xcode выберите **файл** > **Новый** > **файл...** :

    [![](unified-storyboards-images/dls02.png "Select File / New")](unified-storyboards-images/dls02.png#lightbox)
4. Выберите **интерфейс пользователя** **iOS** >  > **запустить экран** и нажмите кнопку **Далее** :

    [![](unified-storyboards-images/dls03.png "Select iOS / User Interface / Launch Screen")](unified-storyboards-images/dls03.png#lightbox)
5. Присвойте файлу имя `LaunchScreen.xib` и нажмите кнопку **создать** :

    [![](unified-storyboards-images/dls04.png "Name the file LaunchScreen.xib")](unified-storyboards-images/dls04.png#lightbox)
6. Измените структуру экрана запуска, добавив графические элементы и используя ограничения макета, чтобы разместить их для заданных устройств, ориентации и размеров экрана:

    [![](unified-storyboards-images/dls05.png "Editing the design of the launch screen")](unified-storyboards-images/dls05.png#lightbox)
7. Сохраните изменения в `LaunchScreen.xib`.
8. Выберите **целевые приложения** и вкладку **Общие** :

    [![](unified-storyboards-images/dls06.png "Select the Applications Target and the General tab")](unified-storyboards-images/dls06.png#lightbox)
9. Нажмите кнопку " **выбрать info. plist** ", выберите `Info.plist` для приложения Xamarin и нажмите кнопку " **выбрать** ":

    [![](unified-storyboards-images/dls07.png "Select the Info.plist for the Xamarin app")](unified-storyboards-images/dls07.png#lightbox)
10. В разделе **значки приложений и образы запуска** откройте раскрывающееся меню **файл экрана запуска** и выберите созданную ранее `LaunchScreen.xib`:

    [![](unified-storyboards-images/dls08.png "Choose the LaunchScreen.xib")](unified-storyboards-images/dls08.png#lightbox)
11. Сохраните изменения в файле и вернитесь в Visual Studio для Mac.
12. Дождитесь, пока Visual Studio для Mac завершит синхронизацию изменений с Xcode.
13. В **Обозреватель решений**щелкните правой кнопкой мыши папку **ресурсов** и выберите команду **добавить** > **Добавить файлы...** :

    [![](unified-storyboards-images/dls09.png "Select Add / Add Files...")](unified-storyboards-images/dls09.png#lightbox)
14. Выберите созданный выше файл `LaunchScreen.xib` и нажмите кнопку **Открыть** :

    [![](unified-storyboards-images/dls10.png "Select the LaunchScreen.xib file")](unified-storyboards-images/dls10.png#lightbox)
15. Создайте приложение.

### <a name="testing-the-dynamic-launch-screen"></a>Тестирование экрана динамического запуска

В Visual Studio для Mac выберите симулятор iPhone 4 Retina и запустите приложение. Экран динамического запуска будет отображаться в правильном формате и ориентации:

[![](unified-storyboards-images/dls11.png "The Dynamic Launch Screen displayed in the vertical orientation")](unified-storyboards-images/dls11.png#lightbox)

Закройте приложение в Visual Studio для Mac и выберите устройство iPad iOS 8. Запустите приложение, и экран запуска будет правильно отформатирован для этого устройства и ориентации:

[![](unified-storyboards-images/dls12.png "The Dynamic Launch Screen displayed in the horizontal orientation")](unified-storyboards-images/dls12.png#lightbox)

Вернитесь к Visual Studio для Mac и завершите работу приложения.

### <a name="working-with-ios-7"></a>Работа с iOS 7

Чтобы обеспечить обратную совместимость с iOS 7, просто включите в приложение iOS 8 обычные `Default.png` образов ресурсов как обычно. iOS вернется к предыдущему поведению и будет использовать эти файлы в качестве начального экрана при запуске на устройстве iOS 7.

Чтобы увидеть реализацию динамического экрана запуска в Xamarin, посмотрите на [экран динамического запуска](https://docs.microsoft.com/samples/xamarin/ios-samples/ios8-dynamiclaunchscreen) пример приложения iOS 8, присоединенного к этому документу.

## <a name="summary"></a>Сводка

В этой статье мы потратили краткий обзор классов размеров и их влияния на макет на устройствах iPhone и iPad. В нем обсуждалось, как признаки, среды признаков и коллекции характеристик работают с классами размера для создания Объединенных интерфейсов. Он потратил краткое представление о адаптивных контроллерах представления и о том, как они работают с классами размеров в Объединенных интерфейсах. Мы рассмотрели реализацию классов размера и унифицированных интерфейсов полностью из C# кода внутри приложения Xamarin iOS 8.

Наконец, в этой статье были рассмотрены основы создания унифицированных раскадровок с помощью конструктора Xamarin iOS, который будет работать на устройствах iOS и создать отдельный экран динамического запуска, который будет отображаться в качестве экрана запуска на каждом устройстве iOS 8.

## <a name="related-links"></a>Связанные ссылки

- [Адаптивные фотографии (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios8-adaptivephotos)
- [Экраны динамического запуска (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios8-dynamiclaunchscreen)
- [Введение в iOS 8](~/ios/platform/introduction-to-ios8.md)
- [Динамические макеты в iOS8 — развиваются 2014 (видео)](https://youtu.be/f3mMGlS-lM4)
- [уипресентатионконтроллер](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIPresentationController_class/)
- [уиимажеассет](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIImageAsset_Ref/index.html#//apple_ref/occ/cl/UIImageAsset)
