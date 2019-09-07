---
title: Как автоматизировать тестовый проект Android NUnit?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: EA3CFCC4-2D2E-49D6-A26C-8C0706ACA045
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/29/2018
ms.openlocfilehash: e96f9a0ce4d1eec9bf853faceeb85a2acb4840af
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2019
ms.locfileid: "70761021"
---
# <a name="how-do-i-automate-an-android-nunit-test-project"></a>Как автоматизировать тестовый проект Android NUnit?

> [!NOTE]
> В этом руководство объясняется, как автоматизировать проект тестирования для Android на NUnit, а не проект Xamarin. UITest. Руководства по Xamarin. UITest можно найти [здесь](https://docs.microsoft.com/appcenter/test-cloud/preparing-for-upload/uitest).

При создании проекта **приложения модульного тестирования (Android)** в Visual Studio (или проекте **модульного теста Android** в Visual Studio для Mac) этот проект не будет автоматически выполнять тесты по умолчанию.
Для выполнения тестов NUnit на целевом устройстве можно создать подкласс [Android. app. Instrumentation](xref:Android.App.Instrumentation) , запущенный с помощью следующей команды: 

```shell
adb shell am instrument 
```

Этот процесс описывается в следующих шагах:

1. Создайте новый файл с именем **TestInstrumentation.CS**: 

    ```cs 
    using System;
    using System.Reflection;
    using Android.App;
    using Android.Content;
    using Android.Runtime;
    using Xamarin.Android.NUnitLite;

    namespace App.Tests {

        [Instrumentation(Name="app.tests.TestInstrumentation")]
        public class TestInstrumentation : TestSuiteInstrumentation {

            public TestInstrumentation (IntPtr handle, JniHandleOwnership transfer) : base (handle, transfer)
            {
            }

            protected override void AddTests ()
            {
                AddTest (Assembly.GetExecutingAssembly ());
            }
        }
    }
    ```

    В этом файле `Xamarin.Android.NUnitLite.TestSuiteInstrumentation` (из **Xamarin. Android. NUnitLite. dll**) создается подкласс для создания. `TestInstrumentation`

2. Реализуйте `AddTests` конструктор и метод. `TestInstrumentation` `AddTests` Метод определяет, какие тесты фактически выполняются.

3. Измените файл, добавив **TestInstrumentation.cs.** `.csproj` Например:

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <Project DefaultTargets="Build" ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
        ...
        <ItemGroup>
            <Compile Include="TestInstrumentation.cs" />
        </ItemGroup>
        <Target Name="RunTests" DependsOnTargets="_ValidateAndroidPackageProperties">
            <Exec Command="&quot;$(_AndroidPlatformToolsDirectory)adb&quot; $(AdbTarget) $(AdbOptions) shell am instrument -w $(_AndroidPackage)/app.tests.TestInstrumentation" />
        </Target>
        ...
    </Project>
    ```

4. Используйте следующую команду для запуска модульных тестов. Замените `PACKAGE_NAME` на имя пакета приложения (имя пакета можно найти в `/manifest/@package` атрибуте приложения, расположенном в **файле AndroidManifest. XML**):

    ```shell
    adb shell am instrument -w PACKAGE_NAME/app.tests.TestInstrumentation
    ```

5. При необходимости можно изменить `.csproj` файл, `RunTests` добавив целевой объект MSBuild. Это дает возможность вызывать модульные тесты с помощью команды, как в следующем примере:

    ```shell
    msbuild /t:RunTests Project.csproj
    ```

    (Обратите внимание, что использование этого нового целевого объекта не является `adb` обязательным. можно использовать более раннюю `msbuild`команду вместо.)

Дополнительные сведения об использовании `adb shell am instrument` команды для выполнения модульных тестов см. в разделе Android Developer [Tests with ADB](https://developer.android.com/studio/test/command-line.html#RunTestsDevice) .

> [!NOTE]
> В версии [Xamarin. Android 5,0](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/android/xamarin.android_5/xamarin.android_5.1/index.md#Android_Callable_Wrapper_Naming) имена пакетов по умолчанию для вызываемых оболочек Android будут основываться на md5sum имени экспортируемого типа с указанием сборки. Это позволяет предоставлять одно и то же полное имя из двух разных сборок и не возвращать ошибку упаковки. Поэтому убедитесь, что `Name` свойство `Instrumentation` атрибута используется для создания имени АКВ или класса, доступного для чтения.

_В`adb` приведенной выше команде необходимо использовать имя АКВ_.
Для переименования и рефакторинга C# класса потребуется изменить `RunTests` команду так, чтобы она использовала правильное имя АКВ.
