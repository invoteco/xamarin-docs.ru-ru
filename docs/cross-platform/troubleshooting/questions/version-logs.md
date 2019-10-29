---
title: Где я могу найти информацию о версии и журналы
description: В этом документе описывается, где искать сведения о версии Xamarin и журналах. Эти сведения полезны при диагностике проблем, отправке ошибок или получении поддержки.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: CF386485-EAB0-4B9E-AA17-CB1B6462E505
author: davidortinau
ms.author: daortin
ms.date: 03/29/2017
ms.openlocfilehash: 68de58f499788d803aa0af6c68f20e2265b1d6b5
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73013175"
---
# <a name="where-can-i-find-my-version-information-and-logs"></a>Где я могу найти информацию о версии и журналы

## <a name="outline"></a>Контур

- [Сведения о версии](#version-information)
  - Сведения о версии Windows
  - Сведения о версии Mac
  - Android SDK Tools, Platform-Tools, Build-Tools
- [Журналы IDE и установщика](#ide-and-installer-logs)
  - [Журналы Windows](#windows-logs)
    - Xamarin Studio
    - Xamarin для Visual Studio
    - Универсальный установщик Xamarin
    - Отдельные `.msi` установщики, подробные журналы
    - Запуск Visual Studio, подробные журналы
  - [Журналы Mac](#mac-logs)
    - Узел сборки
  - Visual Studio для Mac
    - Xamarin Studio
    - Установщик Xamarin
- [Подробные выходные данные сборки](#verbose-build-output-logs)
- [Журналы отладки для приложений Xamarin. Android и Xamarin. iOS](#debug-logs-for-xamarin-apps)
  - Журналы logcat для Android `adb`
  - журналы симуляторов iOS (на Mac)
  - журналы устройств iOS (на Mac)

## <a name="a-idversion-information-nameversion-information-version-information"></a>сведения о версии <a id="version-information" name="version-information" />

Обычно лучше всего отправить обратно все сведения из кнопок **Копировать информацию** . В противном случае часто требуется запросить дополнительную информацию. Например, при устранении неполадок могут быть важны версии операционной системы, версия Xcode, установленные уровни API Android и версия .NET.

### <a name="a-idwindows-version-information-namewindows-version-information-windows-version-information"></a><a id="windows-version-information" name="windows-version-information" />сведения о версии Windows

#### <a name="xamarin-studio"></a>Xamarin Studio

**Справка > о > показывать сведения > копирование сведений [кнопка]**

#### <a name="visual-studio"></a>Visual Studio

**Справка > о Microsoft Visual Studio > Копировать сведения [кнопка]**

### <a name="a-idmac-version-information-namemac-version-information-mac-version-information"></a>сведения о версии <a id="mac-version-information" name="mac-version-information" />Mac

#### <a name="visual-studio-for-mac"></a>Visual Studio для Mac

**Visual Studio > о Visual Studio > Отображение подробных сведений > копирование информации [кнопка]**

### <a name="a-idandroid-sdk-tools-versions-nameandroid-sdk-tools-versions-android-sdk-tools-platform-tools-build-tools"></a><a id="android-sdk-tools-versions" name="android-sdk-tools-versions" />Android SDK Tools, Platform-Tools, Build-Tools

Откройте диспетчер пакет SDK для Android и Создайте снимок экрана с разделом "лучшие **средства** ".

#### <a name="visual-studio-for-mac"></a>Visual Studio для Mac

**Средства > открыть пакет SDK для Android Manager**

#### <a name="visual-studio"></a>Visual Studio

**Средства > Android > открыть диспетчер пакет SDK для Android...**

## <a name="a-idide-and-installer-logs-nameide-and-installer-logs-ide-and-installer-logs"></a>журналы <a id="ide-and-installer-logs" name="ide-and-installer-logs" />IDE и установщика

Для каждого расположения журнала обязательно заархивируйте и прикрепите всю папку журнала.

### <a name="a-idwindows-logs-namewindows-logs-windows-logs"></a><a id="windows-logs" name="windows-logs" />журналов Windows

#### <a name="a-idwindows-logs-xamarin-vs-namewindows-logs-xamarin-vs--visual-studio-tools-for-xamarin"></a>Инструменты Visual Studio <a id="windows-logs-xamarin-vs" name="windows-logs-xamarin-vs" /> для Xamarin

`%LOCALAPPDATA%\Xamarin\Logs`

#### <a name="a-idvs-2017-namevs-2017--visual-studio-2017"></a><a id="vs-2017" name="vs-2017" /> Visual Studio 2017

[Как получить журналы установки Visual Studio](https://docs.microsoft.com/visualstudio/install/troubleshooting-installation-issues#how-to-get-the-visual-studio-installation-logs)

#### <a name="a-idvs-2015-namevs-2015--visual-studio-2015"></a><a id="vs-2015" name="vs-2015" /> Visual Studio 2015

#### <a name="a-idwindows-universal-installer-namewindows-universal-installer--xamarin-universal-installer"></a><a id="windows-universal-installer" name="windows-universal-installer" /> универсальный установщик Xamarin

`%LOCALAPPDATA%\Xamarin\Universal`

Это журналы из установщика `XamarinInstaller.exe`.

#### <a name="a-idindividual-msi-installers-verbose-logs-nameindividual-msi-installers-verbose-logs-individual-msi-installers-verbose-logs"></a><a id="individual-msi-installers-verbose-logs" name="individual-msi-installers-verbose-logs" />отдельных `.msi` установщиков, подробных журналов

```csharp
msiexec /i Xamarin.msi /l*vx "%USERPROFILE%\Desktop\Xamarin.log"
```

Ссылка: [Параметры командной строки](https://msdn.microsoft.com/library/aa367988.aspx)

#### <a name="a-idvisual-studio-startup-verbose-logs-namevisual-studio-startup-verbose-logs-visual-studio-startup-verbose-logs"></a><a id="visual-studio-startup-verbose-logs" name="visual-studio-startup-verbose-logs" />запуска Visual Studio, подробные журналы

```csharp
devenv.exe /log "%USERPROFILE%\Desktop\VisualStudio.log"
```

Ссылка: [/log (devenv. exe)](https://msdn.microsoft.com/library/ms241272.aspx)

### <a name="a-idmac-logs-namemac-logs-mac-logs"></a>журналы <a id="mac-logs" name="mac-logs" />Mac

Можно выбрать пункт меню **переход > переход к папке** в Finder, а затем скопировать и вставить любой из этих путей в диалоговое окно.

#### <a name="a-idmac-logs-visual-studio-namemac-logs-visual-studio-visual-studio-for-mac"></a><a id="mac-logs-visual-studio" name="mac-logs-visual-studio" />Visual Studio для Mac

`~/Library/Logs/VisualStudio/7.0` (это значение может изменяться в зависимости от используемой версии).

Эту папку также можно открыть с помощью "Help-> открыть каталог журналов".

#### <a name="a-idmac-logs-xamarin-studio-namemac-logs-xamarin-studio-xamarin-studio"></a><a id="mac-logs-xamarin-studio" name="mac-logs-xamarin-studio" />Xamarin Studio

`~/Library/Logs/XamarinStudio-6.0` (это значение может изменяться в зависимости от используемой версии).

Эту папку также можно открыть с помощью "Help-> открыть каталог журналов".

#### <a name="a-idmac-universal-installer-namemac-universal-installer-xamarin-universal-installer"></a><a id="mac-universal-installer" name="mac-universal-installer" />универсальный установщик Xamarin

`~/Library/Logs/XamarinInstaller/Universal`

Это журналы из установщика `XamarinInstaller.dmg`.

#### <a name="a-idmac-build-host-namemac-build-host-xamarin-build-host"></a><a id="mac-build-host" name="mac-build-host" />узла сборки Xamarin

`~/Library/Logs/Xamarin-[MAJOR.MINOR]`

## <a name="a-idverbose-build-output-logs-nameverbose-build-output-logs-verbose-build-output"></a><a id="verbose-build-output-logs" name="verbose-build-output-logs" />подробные выходные данные сборки

1. Включите [выходные данные диагностики MSBuild](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output).

2. Для приложений iOS также включите **Вывод подробных выходных данных mtouch** , добавив `-v -v -v -v` в **Свойства проекта > iOS Build > General (TAB) > Дополнительные параметры > Дополнительные аргументы mtouch**.

3. Очистите и перестройте проект.

4. Скопируйте и вставьте выходные данные сборки из IDE в текстовый файл.
     - Visual Studio (Windows): **просмотр > выходных данных > Показать выходные данные: сборка**
     - Visual Studio для Mac: **просмотр > pad > ошибках > выходные данные сборки (вкладка)**

## <a name="a-iddebug-logs-for-xamarin-apps-namedebug-logs-for-xamarin-apps-debug-logs-for-xamarinandroid-and-xamarinios-apps"></a><a id="debug-logs-for-xamarin-apps" name="debug-logs-for-xamarin-apps" />журналов отладки для приложений Xamarin. Android и Xamarin. iOS

### <a name="visual-studio-for-mac"></a>Visual Studio для Mac

**Просмотр выходных данных > приложения > Pad**

(Обратите внимание, что этот пункт меню будет отображаться только после запуска приложения.)

### <a name="visual-studio"></a>Visual Studio

**Просмотр выходных данных > > Отображение выходных данных: Отладка**

### <a name="a-idadb-logcat-nameadb-logcat-android-adbhttpsdeveloperandroidcomtoolshelpadbhtml-logcat-logs"></a><a id="adb-logcat" name="adb-logcat" />журналов logcat [`adb`](https://developer.android.com/tools/help/adb.html) Android

После выполнения команды `adb` присоедините файл **android_logcat. txt** к рабочему столу. В этих инструкциях предполагается, что подключено только одно устройство.

См. также страницу [журнала отладки Android](~/android/deploy-test/debugging/android-debug-log.md) .

#### <a name="visual-studio"></a>Visual Studio

1. **Средства > Android > запустить командную строку ADB для Android**
2. Очистка журнала: `adb logcat -c`
3. Воспроизведите ошибку.
4. Вывод журнала: `adb logcat -vtime -d > "%USERPROFILE%\Desktop\android_logcat.txt"`

#### <a name="visual-studio-for-mac"></a>Visual Studio для Mac

1. **Средства > открыть пакет SDK для Android командной строки**
2. Очистка журнала: `adb logcat -c`
3. Воспроизведите ошибку.
4. Вывод журнала: `adb logcat -vtime -d > ~/Desktop/android_logcat.txt`

### <a name="a-idios-simulator-logs-nameios-simulator-logs-ios-simulator-logs-on-mac"></a>журналы <a id="ios-simulator-logs" name="ios-simulator-logs" />iOS симуляторов (на Mac)

- Чтобы получить доступ к системному журналу, выберите **отладка > открыть системный журнал...** в приложении для симуляторов iOS.

- Чтобы просмотреть отчеты о сбоях в симуляторе, откройте консоль. app и перейдите к `~/Library/Logs > DiagnosticReports`.

### <a name="a-idios-device-logs-nameios-device-logs-ios-device-logs-on-mac"></a>журналы устройств iOS <a id="ios-device-logs" name="ios-device-logs" />(на Mac)

#### <a name="visual-studio-for-mac"></a>Visual Studio для Mac

**Просмотр > Pad > Журнал устройств iOS**

#### <a name="xcode"></a>Xcode

**Окна > устройств > $ {DeviceName}**

Отчеты о сбоях доступны при нажатии кнопки **Просмотр журналов устройств** . Системный журнал для устройства отображается в нижней части окна со стрелкой раскрытия.

#### <a name="xcode-5"></a>Xcode 5

**Окно > Организатор > устройств (вкладка) > $ {DeviceName}**
