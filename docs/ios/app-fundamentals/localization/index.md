---
title: Локализация в Xamarin. iOS
description: В этом документе описываются возможности локализации iOS и способы использования этих функций в приложениях Xamarin. iOS. В нем обсуждаются язык, языковой стандарт, файлы строк, образы запуска и многое другое.
ms.prod: xamarin
ms.assetid: DFD9EB4A-E536-18E4-C8FD-679BA9C836D8
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 04/28/2017
ms.openlocfilehash: fc67c7f683b6c55d3b4f552c2f8c113ee721be61
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73009808"
---
# <a name="localization-in-xamarinios"></a>Локализация в Xamarin. iOS

_В этом документе рассматриваются функции локализации пакета SDK для iOS и способы доступа к ним с помощью Xamarin._

Инструкции по включению наборов символов и кодовых страниц в приложениях, которые должны обрабатывать данные не в Юникоде, см. в разделе о [кодировках интернационализации](encodings.md) .

## <a name="ios-platform-features"></a>возможности платформы iOS

В этом разделе описываются некоторые возможности локализации в iOS. Перейдите к [следующему разделу](#localization-basics-in-ios) , чтобы просмотреть конкретный код и примеры.

### <a name="language"></a>Язык

Пользователи выбирают язык в приложении " **Параметры** ". Этот параметр влияет на строки и изображения языка, отображаемые операционной системой и приложениями.

Чтобы определить язык, используемый в приложении, получите первый элемент `NSBundle.MainBundle.PreferredLocalizations`:

```csharp
var lang = NSBundle.MainBundle.PreferredLocalizations[0];
```

Это значение будет представлять собой код языка, например `en` для английского, `es` для испанского, `ja` для японского и т. д. Возвращаемое значение ограничено одной из локализаций, поддерживаемых приложением (для определения наилучшего соответствия используются правила резервирования).

Код приложения не всегда должен проверять это значение — Xamarin и iOS предоставляют функции, которые помогают автоматически предоставлять правильную строку или ресурс для языка пользователя. Эти функции описаны в оставшейся части этого документа.

> [!NOTE]
> Используйте `NSLocale.PreferredLanguages`, чтобы определить языковые параметры пользователя, независимо от локализации, поддерживаемых приложением. Значения, возвращаемые этим методом, изменились в iOS 9; Дополнительные сведения см. в [техническом примечании TN2418](https://developer.apple.com/library/content/technotes/tn2418/_index.html) .

### <a name="locale"></a>Языковой стандарт

Пользователи выбирают свой языковой стандарт в приложении " **Параметры** ". Этот параметр влияет на способ форматирования дат, времени, чисел и денежных единиц.

Это позволяет пользователям выбирать, будут ли они просматривать форматы времени в 12-часовом или 24-часовом формате, является ли их десятичный разделитель запятой или точкой, а также порядком времени суток, месяца и года в поле Дата.

Благодаря Xamarin у вас есть доступ к классам iOS Apple (`NSNumberFormatter`), а также к классам .NET в System. Globalization. Разработчики должны оценить, какие из них лучше подходят, так как в каждом из них доступны различные функции. В частности, при извлечении и отображении цен покупки в приложении с помощью StoreKit следует использовать классы форматирования Apple для получения сведений о ценах.

Текущий языковой стандарт может быть запрошен одним из двух способов:

- `NSLocale.CurrentLocale.LocaleIdentifier`
- `NSLocale.AutoUpdatingCurrentLocale.LocaleIdentifier`

Первое значение может кэшироваться операционной системой и поэтому может не всегда отражать выбранный в данный момент языковой стандарт пользователя. Используйте второе значение для получения текущего выбранного языкового стандарта.

> [!NOTE]
> Mono (среда выполнения .NET, на которой основано Xamarin. iOS), а API-интерфейсы Apple iOS не поддерживают идентичные наборы сочетаний языков и регионов.
> По этой причине можно выбрать сочетание языка и региона в приложении iOS **Settings** , которое не соответствует допустимому значению в Mono. Например, если задать для языка iPhone значение Английский, а в его регионе — Испания, то следующие API будут давать разные значения:
>
> - `CurrentThead.CurrentCulture`: en-US (моно API)
> - `CurrentThread.CurrentUICulture`: en-US (моно API)
> - `NSLocale.CurrentLocale.LocaleIdentifier`: en_ES (API Apple)
>
> Поскольку Mono использует `CurrentThread.CurrentUICulture` для выбора ресурсов и `CurrentThread.CurrentCulture` для форматирования дат и валют, локализация на основе Mono (например, с помощью RESX-файлов) может не дать ожидаемых результатов для этих сочетаний языков и регионов. В таких ситуациях для локализации необходимо использовать API Apple.

### <a name="nscurrentlocaledidchangenotification"></a>нскуррентлокаледидчанженотификатион

iOS создает `NSCurrentLocaleDidChangeNotification`, когда пользователь обновляет свой языковой стандарт. Приложения могут прослушивать это уведомление, пока они выполняются, и могут вносить соответствующие изменения в пользовательский интерфейс.

## <a name="localization-basics-in-ios"></a>Основы локализации в iOS

В Xamarin можно легко использовать следующие функции iOS для предоставления локализованных ресурсов для показа пользователю. Чтобы узнать, как реализовать эти идеи, ознакомьтесь с [примером TaskyL10n](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) .

### <a name="specifying-default-and-supported-languages-in-infoplist"></a>Указание поддерживаемых языков по умолчанию в info. plist

В [техническом соотношении & QA1828: как iOS определяет язык приложения](https://developer.apple.com/library/content/qa/qa1828/_index.html), компания Apple описывает, как iOS выбирает язык для использования в приложении. На отображение языка влияют следующие факторы.

- Предпочтительные языки пользователя (находятся в приложении " **Параметры** ")
- Локализации, Объединенные с приложением (лпрож Folders)
- `CFBundleDevelopmentRegion` (значение**info. plist** , указывающее язык по умолчанию для приложения)
- `CFBundleLocalizations` (массив**info. plist,** указывающий все поддерживаемые локализации)

Как указано в техническом Q & A, `CFBundleDevelopmentRegion` представляет регион и язык по умолчанию для приложения. Если приложение явно не поддерживает какие-либо предпочтительные языки пользователя, оно будет использовать язык, заданный этим полем.

> [!IMPORTANT]
> iOS 11 применяет этот механизм выбора языка более строго, чем предыдущие версии операционной системы. По этой причине любое приложение iOS 11, которое не объявляет в явном виде свои поддерживаемые локализации, либо путем включения. лпрож, либо установки значения для `CFBundleLocalizations` — может отображать другой язык в iOS 11, чем в iOS 10.

Если `CFBundleDevelopmentRegion` не указано в файле **info. plist** , в инструментах сборки Xamarin. iOS в настоящее время используется значение по умолчанию `en_US`. Хотя это может измениться в будущем выпуске, это означает, что языком по умолчанию является английский.

Чтобы убедиться, что приложение выбирает ожидаемый язык, выполните следующие действия.

- Укажите язык по умолчанию. Откройте **info. plist** и используйте представление **исходного кода** , чтобы задать значение для ключа `CFBundleDevelopmentRegion`. в XML он должен выглядеть следующим образом:

```xml
<key>CFBundleDevelopmentRegion</key>
<string>es</string>
```

В этом примере используется "ES", чтобы указать, что если ни один из предпочитаемых языков пользователя не поддерживается, по умолчанию принимается испанский язык.

- Объявите все поддерживаемые локализации. В файле **info. plist**используйте представление **исходного кода** , чтобы задать массив для ключа `CFBundleLocalizations`. в XML он должен выглядеть следующим образом:

```xml
<key>CFBundleLocalizations</key>
<array>
    <string>en</string>
    <string>es</string>
    ...
</array>
```

Приложения Xamarin. iOS, локализованные с помощью механизмов .NET, таких как RESX, должны также предоставлять эти значения **info. plist** .

Дополнительные сведения об этих ключах **info. plist** см. в [справочнике по ключам списка информационных свойств](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CoreFoundationKeys.html)Apple.

### <a name="getlocalizedstring-method"></a>Метод Жетлокализедстринг

Метод `NSBundle.MainBundle.GetLocalizedString` ищет локализованный текст, сохраненный в файлах **. strings** в проекте. Эти файлы упорядочены по языку, в специально именованных каталогах с суффиксом **. лпрож** (Обратите внимание, что первая буква расширения — это нижний регистр "L").

#### <a name="strings-file-locations"></a>расположения файловых строк

- **Base. лпрож** — это каталог, который содержит ресурсы для языка по умолчанию.
  Она часто находится в корневом каталоге проекта (но также может быть помещена в папку **Resources** ).
- для каждого поддерживаемого языка, как правило, в папке **Resources (ресурсы** ) создаются **&lt;языки&gt;. лпрож** .

В каталоге каждого языка может быть несколько различных файлов **. strings** :

- **Localizable. strings** — основной список локализованного текста.
- **Инфоплист. strings** — в этом файле разрешены определенные ключи для преобразования таких элементов, как имя приложения.
- **\<имя раскадровки >. strings** — необязательный файл, содержащий переводы для элементов пользовательского интерфейса в раскадровке.

**Действие сборки** для этих файлов должно быть **ресурсом пакета**.

#### <a name="strings-file-format"></a>формат файлов. strings

Синтаксис локализованных строковых значений:

```console
/* comment */
"key"="localized-value";
```

В строки должны быть экранированы следующие символы:

- `\"`ная цитата
- Обратная косая черта `\\`
- `\n` новой строки

Это пример **ES/Localizable. strings** (IE. Испанский) файл из примера:

```console
"<new task>" = "<new task>";
"Task Details" = "Detalles de la tarea";
"Name" = "Nombre";
"task name" = "nombre de la tarea";
"Notes" = "Notas";
"other task info"= "otra información de tarea";
"Done" = "Completo";
"Save" = "Guardar";
"Delete" = "Eliminar";
```

### <a name="images"></a>образы,

Локализация образа в iOS:

1. См. образ в коде, например:

    ```csharp
    UIImage.FromBundle("flag");
    ```

2. Установите флаг файла изображения по умолчанию **. png** в **base. лпрож** (каталог языка разработки машинного кода).

3. При необходимости разместите локализованные версии образа в папках **лпрож** для каждого языка (например, **ES. лпрож**, **ja. лпрож**). Используйте одно и то же имя файла **Flag. png** в каждом языковом каталоге.

Если образ отсутствует для определенного языка, iOS перейдет в папку на собственном языке по умолчанию и загрузит образ из него.

#### <a name="launch-images"></a>Образы запуска

Используйте стандартные соглашения об именовании для образов запуска (и XIB или раскадровку для моделей iPhone 6), если они помещены в каталоги **лпрож** для каждого языка.

```console
Default.png
Default@2x.png
Default-568h@2x.png
LaunchScreen.xib
```

### <a name="app-name"></a>Имя приложения

Размещение файла **инфоплист. strings** в каталоге **. лпрож** позволяет переопределить некоторые значения из **info. plist**приложения, включая имя приложения:

```console
"CFBundleDisplayName" = "LeónTodo";
```

Другие ключи, которые можно использовать для [локализации строк для конкретного приложения](https://developer.apple.com/library/ios/documentation/MacOSX/Conceptual/BPInternational/LocalizingYourApp/LocalizingYourApp.html#//apple_ref/doc/uid/10000171i-CH5-SW21) :

- кфбундленаме
- кфбундлешортверсионстринг
- ншуманреадаблекопиригхт

### <a name="dates-and-times"></a>Даты и время

Хотя можно использовать встроенные функции даты и времени .NET (вместе с текущей `CultureInfo`) для форматирования даты и времени для языкового стандарта, это будет игнорировать пользовательские параметры, зависящие от локали (которые могут быть установлены отдельно от языка).

Используйте `NSDateFormatter` iOS, чтобы получить выходные данные, соответствующие предпочтениям национальной настройки пользователя. В следующем образце кода показаны основные параметры форматирования даты и времени:

```csharp
var date = NSDate.Now;
var df = new NSDateFormatter ();
df.DateStyle = NSDateFormatterStyle.Full;
df.TimeStyle = NSDateFormatterStyle.Long;
Debug.WriteLine ("Full,Long: " + df.StringFor(date));
df.DateStyle = NSDateFormatterStyle.Short;
df.TimeStyle = NSDateFormatterStyle.Short;
Debug.WriteLine ("Short,Short: " + df.StringFor(date));
df.DateStyle = NSDateFormatterStyle.Medium;
df.TimeStyle = NSDateFormatterStyle.None;
Debug.WriteLine ("Medium,None: " + df.StringFor(date));
```

Результаты для английского языка в США:

```console
Full,Long: Friday, August 7, 2015 at 10:29:32 AM PDT
Short,Short: 8/7/15, 10:29 AM
Medium,None: Aug 7, 2015
```

Результаты для испанского языка в Испании:

```console
Full,Long: viernes, 7 de agosto de 2015, 10:26:58 GMT-7
Short,Short: 7/8/15 10:26
Medium,None: 7/8/2015
```

Дополнительные сведения см. в документации по средствам [форматирования дат](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/DataFormatting/Articles/dfDateFormatting10_4.html) Apple. При проверке форматирования даты и времени, зависящих от языкового стандарта, проверьте параметры **язык iPhone** и **регион** .

<a name="rtl" />

### <a name="right-to-left-rtl-layout"></a>Макет с НАПРАВЛЕНИЕм справа налево

iOS предоставляет ряд функций, которые помогут в создании приложений с поддержкой RTL:

- Используйте `leading` и `trailing` атрибуты автомакета для выравнивания элемента управления (который соответствует левому и правому для английского языка, но для языков с письмом справа налево).
  Элемент управления [`UIStackView`](~/ios/user-interface/controls/uistackview.md) особенно полезен для размещения элементов управления с поддержкой RTL.
- Используйте `TextAlignment = UITextAlignment.Natural` для выравнивания текста (который будет оставлен для большинства языков, но справа налево).
- `UINavigationController` автоматически переворачивает кнопку «назад» и меняет направление считывания.

На следующих снимках экрана показан [локализованный пример задачи](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) на арабском языке и иврите (хотя в полях был введен английский язык):

[![](images/rtl-ar-sml.png "Localization in Arabic")](images/rtl-ar.png#lightbox "Arabic")

[![](images/rtl-he-sml.png "Localization in Hebrew")](images/rtl-he.png#lightbox "Hebrew")

iOS автоматически отменяет `UINavigationController`, а другие элементы управления помещаются в `UIStackView` или согласованы с автоматическим макетом.
Текст справа налево локализуется с помощью файлов **. strings** так же, как и текст слева направо.

<a name="code"/>

## <a name="localizing-the-ui-in-code"></a>Локализация пользовательского интерфейса в коде

В примере [задачи (локализована в коде)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) показано, как локализовать приложение, в котором пользовательский интерфейс построен в коде (а не XIB или раскадровки).

### <a name="project-structure"></a>Структура проекта

![](images/solution-code.png "Resources tree")

### <a name="localizablestrings-file"></a>Локализуемый файл. strings

Как описано выше, формат **локализуемого файла. strings** состоит из пар "ключ-значение". Ключ описывает назначение строки, а значение — переведенный текст, который будет использоваться в приложении.

Ниже показаны испанские (**ES**) локализации для примера.

```console
"<new task>" = "<new task>";
"Task Details" = "Detalles de la tarea";
"Name" = "Nombre";
"task name" = "nombre de la tarea";
"Notes" = "Notas";
"other task info"= "otra información de tarea";
"Done" = "Completo";
"Save" = "Guardar";
"Delete" = "Eliminar";
```

### <a name="performing-the-localization"></a>Выполнение локализации

В коде приложения, когда задается отображаемый текст пользовательского интерфейса (текст метки или заполнитель ввода и т. д.), код использует функцию iOS `GetLocalizedString` для получения правильного отображаемого перевода:

```csharp
var localizedString = NSBundle.MainBundle.GetLocalizedString ("key", "optional");
someControl.Text = localizedString;
```

<a name="storyboard"/>

## <a name="localizing-storyboard-uis"></a>Локализация интерфейсов Storyboard

В примере [задачи (локализованная Раскадровка)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10nStoryboard) показано, как локализовать текст на элементах управления в раскадровке.

### <a name="project-structure"></a>Структура проекта

Каталог **base. лпрож** содержит раскадровку и также должен содержать все изображения, используемые в приложении.

Каталоги других языков содержат локализуемый файл **. strings** для любых строковых ресурсов, упоминаемых в коде, а также файл **файл mainstoryboard. strings** , содержащий переводы для текста в раскадровке.

![](images/solution-storyboard.png "Resources tree")

Каталоги языков должны содержать копию всех локализованных образов, чтобы переопределить ту, которая имеется в **базе данных Base. лпрож**.

### <a name="object-id--localization-id"></a>Идентификатор объекта/ИД локализации

При создании и изменении элементов управления в раскадровке выберите каждый элемент управления и проверьте идентификатор, используемый для локализации:

- В Visual Studio для Mac он находится в **панель свойств** и называется **идентификатором локализации**.
- В Xcode это называется **Object ID**.

Это строковое значение часто имеет форму, например "NF3-H8-КСМР", как показано на следующем снимке экрана:

![](images/xs-designer-localization-id.png "Xcode view of Storyboard localization")

Это значение используется в файле **. strings** для автоматического присвоения переведенного текста каждому элементу управления.

### <a name="mainstoryboardstrings"></a>Файл mainstoryboard. strings

Формат файла перевода раскадровки похож на **Локализуемый файл. strings** , за исключением того, что ключ (значение слева) не может быть определен пользователем, а должен иметь специфический формат: `ObjectID.property`.

В примере **файл mainstoryboard. strings** ниже можно увидеть, `UITextField`s имеют свойство `placeholder` Text, которое можно локализовать; `UILabel`s имеют свойство `text`; и `UIButton`s текст по умолчанию задается с помощью `normalTitle`:

```console
"SXg-TT-IwM.placeholder" = "nombre de la tarea";
"Pqa-aa-ury.placeholder"= "otra información de tarea";
"zwR-D9-hM1.text" = "Detalles de la tarea";
"bAM-2j-Rzw.text" = "Notas";           /* Notes */
"NF3-h8-xmR.text" = "Completo";        /* Done */
"MWt-Ya-pMf.normalTitle" = "Guardar";  /* Save */
"IGr-pR-05L.normalTitle" = "Eliminar"; /* Delete */
```

> [!IMPORTANT]
> Использование раскадровки с классами Size может привести к переводам, которые не отображаются в приложении. [Заметки о выпуске Apple Xcode](https://developer.apple.com/library/content/releasenotes/DeveloperTools/RN-Xcode/Chapters/Introduction.html) указывают на то, что раскадровка или XIB не будет правильно локализоваться, если три вещи имеют значение true: используются классы размера, базовая локализация и целевой объект сборки установлены в значение Universal, а сборка предназначена для iOS 7,0. Исправление заключается в копировании строк раскадровки в два идентичных файла: **файл mainstoryboard ~ iPhone. strings** и **файл mainstoryboard ~ iPad. strings**, как показано на следующем снимке экрана:
>
> ![](images/xs-dup-strings.png "Strings files")

<a name="appstore" />

## <a name="app-store-listing"></a>Список App Store

В разделе часто задаваемых вопросов Apple о [локализации магазина приложений](https://itunespartner.apple.com/en/apps/faq/App%20Store_Localization) введите переводы для каждой страны, на которой приложение находится в продаже. Обратите внимание на предупреждение о том, что переводы будут отображаться только в том случае, если приложение содержит локализованный каталог **лпрож** для данного языка.

## <a name="summary"></a>Сводка

В этой статье рассматриваются основы локализации приложений iOS с помощью встроенных функций обработки ресурсов и раскадровки.

Дополнительные сведения о i18n и l10n для iOS, Android и кросс-платформенных приложений (включая Xamarin. Forms) см. в [этом межплатформенном руководством](~/cross-platform/app-fundamentals/localization.md).

## <a name="related-links"></a>Связанные ссылки

- [Задача (локализованная в коде) (пример)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n)
- [Задача (локализованная Раскадровка) (пример)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10nStoryboard)
- [Руководством по локализации Apple](https://developer.apple.com/library/ios/documentation/MacOSX/Conceptual/BPInternational/InternationalizingYourUserInterface/InternationalizingYourUserInterface.html)
- [Общие сведения о локализации на разных платформах](~/cross-platform/app-fundamentals/localization.md)
- [Локализация Xamarin. Forms](~/xamarin-forms/app-fundamentals/localization/index.md)
- [Локализация в Android](~/android/app-fundamentals/localization.md)
