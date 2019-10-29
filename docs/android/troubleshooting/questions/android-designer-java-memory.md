---
title: Настройка параметров памяти Java для конструктора Android
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 62FAF21C-8090-4AF3-9D88-05A4CFCAFFDC
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/02/2018
ms.openlocfilehash: 9c9b9f5a205a2eef7db9f27e8d09b10ce65a4318
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73027045"
---
# <a name="adjusting-java-memory-parameters-for-the-android-designer"></a>Настройка параметров памяти Java для конструктора Android

Параметры памяти по умолчанию, используемые при запуске `java` процесса для конструктора Android, могут быть несовместимы с некоторыми конфигурациями системы.

Начиная с Xamarin Studio 5.7.2.7 (и более поздних Visual Studio для Mac) и Инструменты Visual Studio для Xamarin 3.9.344 эти параметры можно настроить отдельно для каждого проекта.

## <a name="new-android-designer-properties-and-corresponding-java-options"></a>Новые свойства конструктора Android и соответствующие параметры Java

Следующие имена свойств соответствуют указанному [параметру командной строки](https://docs.oracle.com/javase/7/docs/technotes/tools/windows/java.html) Java.

- **Андроиддесигнержаварендерерминмемори** -XMS

- **Андроиддесигнержаварендерермаксмемори** — Xmx

- **Андроиддесигнержаварендерерпермсизе** -XX: макспермсизе

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Откройте решение в Visual Studio.

2. Выберите каждый проект Android один на один в обозреватель решений и дважды щелкните [отобразить все файлы](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2008/4afxey9h(v=vs.90)) в каждом проекте. Можно пропустить проекты, которые не содержат файлы макета `.axml`. Этот шаг гарантирует, что каждый каталог проекта будет содержать файл `.csproj.user`.

3. Выйдите из Visual Studio.

4. Поиск файла `.csproj.user` для каждого из проектов, выполненных на шаге 2.

5. Измените каждый файл `.csproj.user` в текстовом редакторе.

6. Добавьте все или все новые свойства памяти конструктора Android в элемент `<PropertyGroup>`. Можно использовать существующий `<PropertyGroup>` или создать новый. Ниже приведен полный пример `.csproj.user` файла, который содержит все 3 атрибута, для которых заданы значения по умолчанию:

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <Project ToolsVersion="12.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
       <PropertyGroup>
         <ProjectView>ProjectFiles</ProjectView>
       </PropertyGroup>
       <PropertyGroup>
         <AndroidDesignerJavaRendererMinMemory>128m</AndroidDesignerJavaRendererMinMemory>
         <AndroidDesignerJavaRendererMaxMemory>750m</AndroidDesignerJavaRendererMaxMemory>
         <AndroidDesignerJavaRendererPermSize>350m</AndroidDesignerJavaRendererPermSize>
       </PropertyGroup>
    </Project>
    ```

7. Сохраните и закройте все обновленные файлы `.csproj.user`.

8. Перезапустите Visual Studio и повторно откройте решение.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio для Mac](#tab/macos)

1. Откройте решение в Visual Studio для Mac, чтобы каталог решения содержал файл `.userprefs`.

2. Закройте Visual Studio для Mac.

3. В каталоге решения щелкните файл `.userprefs`.

4. Измените файл `.userprefs` в текстовом редакторе.

5. Нахождение существующего XML-элемента в следующем формате. Последняя часть имени элемента будет соответствовать имени проекта: "AndroidApplication1" в этом примере:

    ```xml
    <MonoDevelop.Ide.ItemProperties.AndroidApplication1 ... >
    ```

6. Если элемент `<MonoDevelop.Ide.ItemProperties.AndroidApplication1 ... >` не существует, создайте его в любом месте внутри вложенного элемента `<Properties>`. Не забудьте заменить "AndroidApplication1" именем своего проекта.

7. Добавьте все или все новые свойства памяти конструктора Android в качестве атрибутов элемента. Ниже приведен полный пример `.userprefs` файла, который содержит все 3 атрибута, для которых заданы значения по умолчанию:

    ```xml
    <Properties StartupItem="AndroidApplication1\AndroidApplication1.csproj">
      <MonoDevelop.Ide.Workspace ActiveConfiguration="Debug" PreferredExecutionTarget="Android.SelectDevice" />
      <MonoDevelop.Ide.Workbench />
      <MonoDevelop.Ide.DebuggingService.Breakpoints>
        <BreakpointStore />
      </MonoDevelop.Ide.DebuggingService.Breakpoints>
      <MonoDevelop.Ide.DebuggingService.PinnedWatches />
      <MonoDevelop.Ide.ItemProperties.AndroidApplication1 AndroidDesignerJavaRendererMinMemory="128m" AndroidDesignerJavaRendererMaxMemory="750m" AndroidDesignerJavaRendererPermSize="350m" />
    </Properties>
    ```

8. Повторите шаги 5-7 для каждого проекта Android в решении, содержащем файлы макета `.axml`. (То есть добавьте один элемент `<MonoDevelop.Ide.ItemProperties.ProjectName>` для каждого проекта.)

9. Сохраните и закройте файл `.userprefs`.

10. Перезапустите Visual Studio для Mac и снова откройте решение.

-----
