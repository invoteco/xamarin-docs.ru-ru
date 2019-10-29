---
title: Изменения в Android SDK Tools
description: Изменения в том, как пакет SDK для Android управляет установленными уровнями API и AVD.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5AC61C00-0FF6-4C2D-80E7-D67A3EE30A5A
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/21/2018
ms.openlocfilehash: 4c559a76d7354fd957d065717ef14d91591d1be0
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73019500"
---
# <a name="changes-to-the-android-sdk-tooling"></a>Изменения в Android SDK Tools

_Изменения в том, как пакет SDK для Android управляет установленными уровнями API и AVD._

## <a name="changes-to-android-sdk-tooling"></a>Изменения в средствах пакет SDK для Android

В последних версиях SDK Tools для Android компания Google удалила существующие диспетчеры AVD и пакетов SDK в пользу новых средств интерфейса командной строки (CLI). Программа **Android** была удалена, а диспетчер Google GUI (графический пользовательский интерфейс) в Visual Studio для Mac и более ранних версиях инструменты Visual Studio для Xamarin больше не будет работать с 25.2.5 версии Android SDK Tools. Например, попытка использовать программу **Android** через командную строку приведет к появлению сообщения об ошибке следующего вида:

```shell
The "android" command is deprecated.
For manual SDK, AVD, and project management, please use Android Studio.
For command-line tools, use tools\bin\sdkmanager.bat
and tools\bin\avdmanager.bat
```

В следующих разделах объясняется, как управлять пакет SDK для Android и виртуальными устройствами Android с помощью пакет SDK для Android 25.3.0 и более поздних версий.

### <a name="ui-tools"></a>Средства пользовательского интерфейса

Visual Studio и Visual Studio для Mac теперь обеспечивают замену Xamarin для неподдерживаемых диспетчеров на основе Google GUI:

- Чтобы скачать средства пакет SDK для Android, платформы и другие компоненты, необходимые для разработки приложений Xamarin. Android, используйте [Диспетчер Xamarin пакет SDK для Android Manager](~/android/get-started/installation/android-sdk.md) вместо устаревшего ДИСПЕТЧЕРА пакетов SDK Google.

- Чтобы создать и настроить виртуальные устройства Android, используйте [Android Device Manager](~/android/get-started/installation/android-emulator/device-manager.md) вместо устаревшего диспетчера эмуляторов Google.

Эти средства функционально эквивалентны управляющим руководителям на основе Google GUI, которые они заменяют.

### <a name="cli-tools"></a>Средства CLI

Кроме того, можно использовать средства CLI для управления и обновления эмуляторов и пакет SDK для Android. Следующие программы теперь составляют интерфейс командной строки для средств пакет SDK для Android:

#### <a name="sdkmanager"></a>sdkmanager

**Добавлено в:** Android SDK Tools 25.2.3 (Ноябрь, 2016) и более поздних версий.

В папке **Tools/bin** пакет SDK для Android имеется новая программа с именем **сдкманажер** . Это средство используется для поддержки пакет SDK для Android в командной строке. Дополнительные сведения об использовании этого средства см. в разделе [сдкманажер](https://developer.android.com/studio/command-line/sdkmanager.html).

#### <a name="avdmanager"></a>avdmanager

**Добавлено в:** Android SDK Tools 25.3.0 (Март, 2017) и выше.

В папке **Tools/bin** пакет SDK для Android имеется новая программа с именем **авдманажер** . Это средство используется для поддержки AVD для Android Emulator. Дополнительные сведения об использовании этого средства см. в разделе [авдманажер](https://developer.android.com/studio/command-line/avdmanager.html).

### <a name="downgrading"></a>Переход

Вы можете понизить версию **Android SDK Tools** , установив предыдущую версию пакет SDK для Android с [веб-сайта разработчика Android](https://developer.android.com/studio/index.html).

### <a name="using-the-old-gui"></a>Использование старого графического пользовательского интерфейса

Вы по-прежнему можете использовать исходный графический пользовательский интерфейс, запустив программу **Android** в папке **Tools** , если используется **Android SDK Tools** версии **25.2.5** или ниже.

## <a name="related-links"></a>Связанные ссылки

- [Установка пакета SDK для Android](~/android/get-started/installation/android-sdk.md)
- [Android Device Manager](~/android/get-started/installation/android-emulator/device-manager.md)
- [Общие сведения об уровнях API Android](~/android/app-fundamentals/android-api-levels.md)
- [Заметки о выпуске пакета SDK Tools (Google)](https://developer.android.com/studio/releases/sdk-tools.html)
- [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)
- [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html)
