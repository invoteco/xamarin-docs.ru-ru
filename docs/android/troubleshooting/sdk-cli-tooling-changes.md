---
title: Изменения в Android SDK Tools
description: Изменения в способе управления установленными уровнями API и AVD в пакете SDK для Android.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5AC61C00-0FF6-4C2D-80E7-D67A3EE30A5A
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/21/2018
ms.openlocfilehash: 4c559a76d7354fd957d065717ef14d91591d1be0
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/10/2020
ms.locfileid: "73019500"
---
# <a name="changes-to-the-android-sdk-tooling"></a>Изменения в Android SDK Tools

_Изменения в способе управления установленными уровнями API и AVD в пакете SDK для Android._

## <a name="changes-to-android-sdk-tooling"></a>Изменения в инструментарии пакета SDK для Android

В последних версиях SDK Tools для Android компания Google исключила существующие диспетчеры AVD и SDK, заменив их новыми средствами интерфейса командной строки (CLI). Программа **Android** была удалена, а диспетчеры графического пользовательского интерфейса Google в Visual Studio для Mac и более ранних версиях службы Инструменты Visual Studio для Xamarin больше не будет работать после версии Android SDK Tools 25.2.5. Например, попытка использования программы **Android** с помощью командной строки приведет к появлению следующего сообщения об ошибке:

```shell
The "android" command is deprecated.
For manual SDK, AVD, and project management, please use Android Studio.
For command-line tools, use tools\bin\sdkmanager.bat
and tools\bin\avdmanager.bat
```

В следующих разделах объясняется, как управлять пакетом SDK для Android и виртуальными устройствами Android с помощью пакета SDK для Android 25.3.0 и более поздних версий.

### <a name="ui-tools"></a>Инструменты пользовательского интерфейса

Visual Studio и Visual Studio для Mac теперь обеспечивают замену Xamarin для неподдерживаемых диспетчеров на основе графического пользовательского интерфейса Google:

- Чтобы скачать средства пакета SDK для Android, платформы и другие компоненты, необходимые для разработки приложений Xamarin.Android, используйте [Диспетчер SDK Xamarin Android](~/android/get-started/installation/android-sdk.md) вместо устаревшего диспетчера пакетов SDK для Google.

- Чтобы создать и настроить виртуальные устройства Android, используйте [Диспетчер устройств Android](~/android/get-started/installation/android-emulator/device-manager.md) вместо устаревшего Диспетчера эмуляторов Google.

Эти инструменты функционально эквивалентны менеджерам с графическим пользовательским интерфейсом Google, которых они заменяют.

### <a name="cli-tools"></a>Средства CLI

С другой стороны, вы можете использовать инструменты CLI для управления эмуляторами и пакетом SDK для Android и для их обновления. Следующие программы теперь составляют интерфейс командной строки для средств пакета SDK для Android:

#### <a name="sdkmanager"></a>sdkmanager

**Добавлено в:** Android SDK Tools версии 25.2.3 (ноябрь 2016 года) и более поздние версии.

В папке **tools/bin** вашего пакета SDK для Android появилась новая программа под названием **sdkmanager**. Это средство используется для поддержки пакета SDK для Android в командной строке. Дополнительные сведения об использовании этого инструмента см. в [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html).

#### <a name="avdmanager"></a>avdmanager

**Добавлено в:** Android SDK Tools 25.3.0 (март 2017 года) и более поздние версии.

В папке **tools/bin** вашего пакета SDK для Android появилась новая программа под названием **avdmanager**. Это средство используется для поддержки AVD для Android Emulator. Дополнительные сведения об использовании этого инструмента см. в [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html).

### <a name="downgrading"></a>Переход на использование более ранней версии

Вы можете перейти на более раннюю версию **Android SDK Tools**, установив предыдущую версию пакета SDK для Android с сайта [Android Developer](https://developer.android.com/studio/index.html).

### <a name="using-the-old-gui"></a>Использование старого графического пользовательского интерфейса

Вы по-прежнему можете использовать исходный графический пользовательский интерфейс, запустив программу **Аndroid** в папке **средств** при условии, что **Android SDK Tools** имеет версию **25.2.5** или более раннюю.

## <a name="related-links"></a>Связанные ссылки

- [Установка пакета SDK для Android](~/android/get-started/installation/android-sdk.md)
- [Диспетчер устройств Android](~/android/get-started/installation/android-emulator/device-manager.md)
- [Общие сведения об уровнях API Android](~/android/app-fundamentals/android-api-levels.md)
- [Заметки о выпуске пакета SDK Tools (Google)](https://developer.android.com/studio/releases/sdk-tools.html)
- [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)
- [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html)
