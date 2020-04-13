---
title: Диагностика Android Layout
description: Объясняет диагностику макета Android и как начать работу
ms.prod: xamarin
ms.assetid: BD252EA7-7E69-4DB4-96AB-D52CC0510C8F
ms.technology: xamarin-android
author: decriptor
ms.author: stepsha
ms.date: 03/24/2020
ms.openlocfilehash: 746f74e68fa4816f1f7979980af9506dc0173542
ms.sourcegitcommit: 765b69ed451a0f48625ea597c3f39de95f3ae693
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/09/2020
ms.locfileid: "80987586"
---
# <a name="android-layout-diagnostics"></a>Диагностика макета Android

Диагностика макета Android разработана, чтобы помочь улучшить качество файлов макета Android, выделяя общие проблемы качества и полезные оптимизации. Эта функция доступна как для Visual Studio 16,5 "и Visual Studio для Mac 8.5".

Набор анализаторов по умолчанию предоставляется для широкого круга проблем, и каждый из них может быть настроен для удовлетворения конкретных потребностей проекта. Анализаторы свободно основаны на системе Android linting.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

## <a name="enable-android-layout-diagnostics-on-visual-studio-2019"></a>Включить диагностику макета Android на Visual Studio 2019

Убедитесь, что настройка диагностики макета, **Включить диагностику макета,** включена. Чтобы получить доступ к этой странице опций, выберите **Параметры инструментов,** > **Options**а затем выберите **текстовый редактор** > **Android XML** > **Расширенный:**

![Диалог опций, показывающий, как включить опцию диагностики](diagnostics-images/AndroidDiagnosticsEnableOption.png)

После включения редактор макета Android будет отображать проблемы:

![Android диагностика включена на Visual Studio 2019](diagnostics-images/AndroidDiagnosticsEnabled.png)

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

## <a name="enable-android-layout-diagnostics-on-visual-studio-for-mac"></a>Включить диагностику макета Android на Visual Studio для Mac

Убедитесь, что настройка диагностики макета, **Включить диагностику макета,** включена. Чтобы получить доступ к этой странице опций, выберите **Visual Studio** > **Предпочтения ...**, а затем выберите **текстовый редактор** > **Android XML:**

![Диалог предпочтений, показывающий, как включить опцию диагностики](diagnostics-images/AndroidDiagnosticsEnableOptionVSmac.png)

После включения редактор макета Android будет отображать проблемы:

![Android диагностики включен на Visual Studio для Mac](diagnostics-images/AndroidDiagnosticsEnabledVSmac.png)

-----

## <a name="features"></a>Компоненты

В следующих разделах описаны доступные функции в диагностике макета Android.

### <a name="analyzers"></a>Анализаторы

Анализаторы используются для обнаружения проблем в файлах макетов, снижения значений с жестким кодом, повышения производительности и ошибок флага. Список анализаторов можно ознакомьтесь с [диагностическими анализаторами конструкторских диагностических аналитиков Android](diagnostic-analyzers.md)

### <a name="diagnostic-configuration"></a>Диагностическая конфигурация

Анализаторы могут быть настроены с помощью файла XML, что позволяет изменить уровень серьезности по умолчанию, игнорировать определенные файлы и передавать переменные.

Вы можете использовать базовый файл, если у вас есть набор конфигураций, которыми вы хотите поделиться в нескольких приложениях Android. Чтобы использовать эту функцию, создайте `-baseline` новый файл конфигурации и придайте представление к имени файла. Базовые конфигурации сначала применяются, а затем остальные файлы конфигурации.

> [!TIP]
> Это может быть полезно, если вы хотите игнорировать ряд проблем на новом или существующем приложении Для Android.

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
> В настоящее время `MAX_VIEW_COUNT` единственными переменными `MAX_DEPTH` являются (по умолчанию: 80) и (по умолчанию: 10) для `TooManyViews` и `TooDeepLayout` соответственно.

Ниже перечислены уровни серьезности.

- Предложение
- Сведения
- Предупреждение
- Error
- Игнорировать

### <a name="add-a-configuration-file"></a>Добавление файла конфигурации

Создайте новый файл XML в корне проекта приложения Для Android. Имя файла не важно, но этот `AndroidLayoutDiagnostics.xml`пример использует:

![Добавление нового элемента](diagnostics-images/AndroidDiagnosticsNewFileDialog.png)

После добавления нового файла XML он должен появиться в дереве проекта приложения Android:

![Android App Project Tree](diagnostics-images/AndroidDiagnosticsFileAddToTree.png)

Убедитесь, что действие сборки настроено на **AndroidResourceAnalysisConfig** в панели свойств.
Самый простой способ подтянуть панель свойств для нового файла — нажать правой кнопкой мыши на файл и выбрать свойства. После того, как панель свойств показывает, вы должны изменить **действие сборки** на **AndroidResourceAnalysisConfig:**

![Установка действий по сборке в свойствах элементов](diagnostics-images/AndroidDiagnosticsSetBuildAction.png)

Теперь, когда у вас есть пустой `<configuration>` файл XML, вам нужно добавить корневой элемент. На этом этапе можно настроить поведение по умолчанию любых поддерживаемых проблем.
Если вы хотите убедиться, что жесткие строки рассматриваются как ошибки добавить:

```xml
<issue="HardcodedText" severity="error">
</issue>
```

![Файл конфигурации диагностики](diagnostics-images/AndroidDiagnosticsConfigurationFileExample.png)

Теперь, когда жесткий текст считается ошибкой, он теперь помечен красным завихрением в редакторе макета:

![Расположение с использованием конфигурации диагностики](diagnostics-images/AndroidDiagnosticsUsingConfiguration.png)

> [!NOTE]
> Для того чтобы все новые изменения файла конфигурации вступили в силу, любые открытые файлы макета, которые в настоящее время открыты, должны быть вновь открыты.
>

## <a name="troubleshooting"></a>Устранение неполадок

Вот некоторые возможные общие проблемы.

- Убедитесь, что нет ошибки формата XML.
- Действие сборки настроено правильно на **AndroidResourceAnalysisConfig.**

## <a name="known-issues"></a>Известные проблемы

- Панель ошибок не заседает до тех пор, пока файл не будет изменен в первый раз.

## <a name="related-links"></a>Связанные ссылки

- [Android Линт Проверки](http://tools.android.com/tips/lint-checks)
- [Улучшение кода с помощью проверок ворса](https://developer.android.com/studio/write/lint)
