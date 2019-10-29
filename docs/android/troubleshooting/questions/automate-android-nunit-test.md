---
title: Как автоматизировать тестовый проект Android NUnit?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: EA3CFCC4-2D2E-49D6-A26C-8C0706ACA045
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/29/2018
ms.openlocfilehash: 1246eeac63a0ae232396d4c2fd69d8bf516f5e3e
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73027005"
---
# <a name="how-do-i-automate-an-android-nunit-test-project"></a>Как автоматизировать тестовый проект Android NUnit?

> [!NOTE]
> В этом руководство объясняется, как автоматизировать проект тестирования для Android на NUnit, а не проект Xamarin. UITest. Руководства по Xamarin. UITest можно найти [здесь](https://docs.microsoft.com/appcenter/test-cloud/preparing-for-upload/xamarin-android-uitest).

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

    В этом файле `Xamarin.Android.NUnitLite.TestSuiteInstrumentation` (из **Xamarin. Android. NUnitLite. dll**) подклассировать для создания `TestInstrumentation`.

2. Реализуйте конструктор `TestInstrumentation` и метод `AddTests`. Метод `AddTests` определяет, какие тесты фактически выполняются.

3. Измените файл `.csproj`, добавив **TestInstrumentation.CS**. Пример:

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

4. Используйте следующую команду для запуска модульных тестов. Замените `PACKAGE_NAME` именем пакета приложения (имя пакета можно найти в атрибуте `/manifest/@package` приложения, расположенном в **файле AndroidManifest. XML**):

    ```shell
    adb shell am instrument -w PACKAGE_NAME/app.tests.TestInstrumentation
    ```

5. При необходимости можно изменить файл `.csproj`, добавив `RunTests` целевой объект MSBuild. Это дает возможность вызывать модульные тесты с помощью команды, как в следующем примере:

    ```shell
    msbuild /t:RunTests Project.csproj
    ```

    (Обратите внимание, что использование этого нового целевого объекта не является обязательным. можно использовать предыдущую `adb` команду вместо `msbuild`.)

Дополнительные сведения об использовании команды `adb shell am instrument` для выполнения модульных тестов см. в разделе Android Developer [Tests with ADB](https://developer.android.com/studio/test/command-line.html#RunTestsDevice) .

> [!NOTE]
> В версии [Xamarin. Android 5,0](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/android/xamarin.android_5/xamarin.android_5.1/index.md#Android_Callable_Wrapper_Naming) имена пакетов по умолчанию для вызываемых оболочек Android будут основываться на md5sum имени экспортируемого типа с указанием сборки. Это позволяет предоставлять одно и то же полное имя из двух разных сборок и не возвращать ошибку упаковки. Поэтому убедитесь, что вы используете свойство `Name` атрибута `Instrumentation`, чтобы создать имя класса для чтения или АКВ.

В _приведенной выше команде `adb` необходимо использовать имя АКВ_.
Для переименования и рефакторинга C# класса потребуется изменить команду`RunTests`, чтобы она использовала правильное имя АКВ.
