---
title: Диагностика макета Android
description: Описание диагностики макета Android и начало работы
ms.prod: xamarin
ms.assetid: BD252EA7-7E69-4DB4-96AB-D52CC0510C8F
ms.technology: xamarin-android
author: decriptor
ms.author: stepsha
ms.date: 03/24/2020
ms.openlocfilehash: 5c29a1a80d8c1f599f0bbc750d22d8334ddb3494
ms.sourcegitcommit: d83c6af42ed26947aa7c0ecfce00b9ef60f33319
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/25/2020
ms.locfileid: "80247818"
---
# <a name="android-layout-diagnostics"></a>Диагностика макета Android

Диагностика макета Android призвана помочь улучшить качество файлов макета Android, выделяя распространенные проблемы с качеством и полезные оптимизации. Эта функция доступна для Visual Studio 16.5 + и Visual Studio для Mac 8.5 +.

Набор анализаторов по умолчанию предоставляется для широкого спектра проблем, и каждый из них можно настроить для охвата конкретных потребностей проекта. Анализаторы слабо основаны на системе Android linting.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

## <a name="enable-android-layout-diagnostics-on-visual-studio-2019"></a>Включение диагностики макета Android в Visual Studio 2019

Убедитесь, что параметр диагностики макета включен, включена **Диагностика макета**. Чтобы открыть эту страницу параметров, выберите **сервис** > **Параметры**, а затем выберите **текстовый редактор** > **Android XML** > **Дополнительно**:

![Диалоговое окно параметров, в котором показано, как включить параметр диагностики](diagnostics-images/AndroidDiagnosticsEnableOption.png)

После включения в редакторе макета Android будут отображаться проблемы:

![Диагностика Android включена в Visual Studio 2019](diagnostics-images/AndroidDiagnosticsEnabled.png)

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

## <a name="enable-android-layout-diagnostics-on-visual-studio-for-mac"></a>Включить диагностику макета Android на Visual Studio для Mac

Убедитесь, что параметр диагностики макета включен, включена **Диагностика макета**. Чтобы открыть эту страницу параметров, выберите **Visual Studio** > **предпочтения...** , а затем выберите **текстовый редактор** > **Android XML**:

![Диалоговое окно "настройки", показывающее, как включить параметр "Диагностика"](diagnostics-images/AndroidDiagnosticsEnableOptionVSmac.png)

После включения в редакторе макета Android будут отображаться проблемы:

![Диагностика Android включена на Visual Studio для Mac](diagnostics-images/AndroidDiagnosticsEnabledVSmac.png)

-----

## <a name="features"></a>Компоненты

В следующих разделах описаны доступные функции диагностики макета Android.

### <a name="analyzers"></a>Анализаторы

Анализаторы используются для обнаружения проблем в файлах макета. Некоторые способы сокращения закодированных значений, повышения производительности и пометки ошибок.

### <a name="diagnostic-configuration"></a>Конфигурация диагностики

Анализаторы могут быть настроены с помощью XML-файла, что позволяет изменять уровень серьезности по умолчанию, игнорировать определенные файлы и передавать переменные.

Вы можете использовать базовый файл, если у вас есть набор конфигураций, которые вы хотите использовать в нескольких приложениях Android. Чтобы использовать эту функцию, создайте новый файл конфигурации и добавьте `-baseline` к имени файла. Сначала применяются базовые конфигурации, а затем оставшиеся файлы конфигурации.

> [!TIP]
> Это может быть полезно, если вы хотите пропустить набор проблем в новом или существующем приложении Android.

Формат будет следующим:

```xml
<?xml version="1.0" encoding="utf-8" ?> 
<configuration>
    <issue id="DuplicateIDs" severity="warning">
        <ignore path="Resources/layout/layout1.xml" />
    </issue>
    <issue id="HardcodedText" severity="informational">
        <ignore path="Resources/layout/layout1.xml" />
        <ignore path="Resource/layout/layout2.xml" />
    </issue>
    <issue id="TooManyViews">
        <variable name="MAX_VIEW_COUNT" value="12" />
    </issue>
    <issue id="TooDeepLayout">
        <variable name="MAX_DEPTH" value="12" />
    </issue>
</configuration>
```

> [!NOTE]
> В настоящее время единственными переменными являются `MAX_VIEW_COUNT` (по умолчанию: 80) и `MAX_DEPTH` (по умолчанию: 10) для `TooManyViews` и `TooDeepLayout` соответственно.

Ниже перечислены уровни серьезности.

- Предложение
- Сведения
- Предупреждение
- Ошибка
- Игнорирование

### <a name="add-a-configuration-file"></a>Добавление файла конфигурации

Создайте новый XML-файл в корне проекта приложения Android. Имя файла не имеет значения, но в этом примере используется `AndroidLayoutDiagnostics.xml`:

![Добавление нового элемента](diagnostics-images/AndroidDiagnosticsNewFileDialog.png)

После добавления нового XML-файла он должен появиться в дереве проекта приложения Android:

![Дерево проекта приложения Android](diagnostics-images/AndroidDiagnosticsFileAddToTree.png)

Убедитесь, что для действия сборки задано значение **андроидресаурцеаналисисконфиг** на панели Свойства.
Самый простой способ извлечь панель свойств для нового файла — щелкнуть правой кнопкой мыши файл и выбрать пункт Свойства. После отображения панели свойств следует изменить **действие сборки** на **андроидресаурцеаналисисконфиг**:

![Задание действия сборки в свойствах элемента](diagnostics-images/AndroidDiagnosticsSetBuildAction.png)

Теперь, когда у вас есть пустой XML-файл, необходимо добавить `<configuration>` корневой элемент. На этом этапе можно настроить поведение по умолчанию для любых поддерживаемых проблем.
Если вы хотите убедиться, что жестко запрограммированные строки обрабатываются как ошибки, добавьте:

```xml
<issue="HardcodedText" severity="error">
</issue>
```

![Файл конфигурации диагностики](diagnostics-images/AndroidDiagnosticsConfigurationFileExample.png)

Теперь, когда жестко кодированный текст считается ошибкой, он теперь отмечен красной волнистой линией в редакторе макета:

![Макет с использованием конфигурации диагностики](diagnostics-images/AndroidDiagnosticsUsingConfiguration.png)

> [!NOTE]
> Чтобы новые изменения в файле конфигурации вступили в силу, необходимо повторно открыть все открытые в настоящий момент файлы макета.
>

## <a name="troubleshooting"></a>Диагностика

Ниже приведены некоторые возможные распространенные проблемы.

- Убедитесь в отсутствии ошибок формата XML.
- Действие сборки правильно настроено для **андроидресаурцеаналисисконфиг**.

## <a name="known-issues"></a>Известные проблемы

- Панель ошибок не заполняется до тех пор, пока файл не будет изменен в первый раз.

## <a name="related-links"></a>Связанные ссылки

- [Проверки Lint Android](http://tools.android.com/tips/lint-checks)
- [Улучшение кода с помощью проверок Lint](https://developer.android.com/studio/write/lint)
