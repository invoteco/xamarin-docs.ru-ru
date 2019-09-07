---
title: Можно ли подключиться с виртуальной машины Windows к эмуляторам Android под управлением Mac?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 7B6752BB-8E4C-4690-B275-7E425A051F45
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/21/2018
ms.openlocfilehash: af32f4af3951eff3b8b5412908e35c4cdef09ae4
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2019
ms.locfileid: "70757259"
---
# <a name="is-it-possible-to-connect-to-android-emulators-running-on-a-mac-from-a-windows-vm"></a>Можно ли подключиться с виртуальной машины Windows к эмуляторам Android под управлением Mac?

Чтобы подключиться к Android Emulator, работающему на компьютере Mac, с виртуальной машины Windows, выполните следующие действия.

1. Запустите эмулятор на компьютере Mac.

2. Завершить работу `adb` сервера на компьютере Mac:

    ```bash
    adb kill-server
    ```

3. Обратите внимание, что эмулятор прослушивает 2 порта TCP в интерфейсе замыкания на себя:

    ```bash
    lsof -iTCP -sTCP:LISTEN -P | grep 'emulator\|qemu'

    emulator6 94105 macuser   20u  IPv4 0xa8dacfb1d4a1b51f      0t0  TCP localhost:5555 (LISTEN)
    emulator6 94105 macuser   21u  IPv4 0xa8dacfb1d845a51f      0t0  TCP localhost:5554 (LISTEN)
    ```

    Порт с нечетным номером используется для подключения `adb`к. См. [https://developer.android.com/tools/devices/emulator.html#emulatornetworking](https://developer.android.com/tools/devices/emulator.html#emulatornetworking)также.

4. _Вариант 1_. Использует[`nc`](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man1/nc.1.html)
    чтобы пересылать входящие пакеты TCP, полученные извне через порт 5555 (или любой другой порт), на порт с нечетным номером в интерфейсе замыкания на себя (**127.0.0.1 5555** в этом примере) и пересылать исходящие пакеты другим способом:

    ```bash
    cd /tmp
    mkfifo backpipe
    nc -kl 5555 0<backpipe | nc 127.0.0.1 5555 > backpipe
    ```

    `nc` Пока команды продолжают работать в окне терминала, пакеты пересылаются должным образом. Вы можете ввести в окне терминала элемент управления-C, чтобы выйти `nc` из команд после завершения работы с эмулятором.

    (Вариант 1 обычно проще, чем вариант 2, особенно если **Системные настройки > безопасность & конфиденциальность > брандмауэр** включен.) 

    _Вариант 2_. Использует[`pfctl`](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man8/pfctl.8.html)
    для перенаправления пакетов TCP с `5555` порта (или любого другого порта) в интерфейсе [общего доступа](http://kb.parallels.com/en/4948) к порту с нечетным номером в интерфейсе замыкания на себя`127.0.0.1:5555` (в этом примере):

    ```bash
    sed '/rdr-anchor/a rdr pass on vmnet8 inet proto tcp from any to any port 5555 -> 127.0.0.1 port 5555' /etc/pf.conf | sudo pfctl -ef -
    ```

    Эта команда настраивает перенаправление портов с `pf packet filter` помощью системной службы. Разрывы строк важны. Не забудьте обеспечить их неизменность при копировании. Кроме того, при использовании параллельных вычислений необходимо изменить имя интерфейса с *VMnet8* . `vmnet8`— Это имя Специального *устройства NAT* для режима *общей сетевой сети* в слиянии VMware. Вполне вероятно, что соответствующий сетевой интерфейс в параллелях [vnic0](http://download.parallels.com/doc/psbm/en/Parallels_Server_Bare_Metal_Users_Guide/29258.htm).

5. Подключитесь к эмулятору с компьютера Windows:

    ```cmd
    C:\> adb connect ip-address-of-the-mac:5555
    ```

    Замените IP-address-of-Mac на IP-адрес компьютера Mac, например, в списке `ifconfig vmnet8 | grep 'inet '`. При необходимости замените `5555` другим портом, который вам нравится на шаге 4.\. (Примечание. один из способов получения доступа к командной строке — `adb` с помощью [**средств > Android > Android ADB Командная**](~/cross-platform/troubleshooting/questions/version-logs.md#adb-logcat) строка в Visual Studio.)

### <a name="alternate-technique-using-ssh"></a>Альтернативный способ использования`ssh`

Если вы включили _Удаленный вход_ на компьютере Mac, для подключения к эмулятору можно использовать `ssh` перенаправление портов.

1. Установите SSH-клиент в Windows. Один из вариантов — установить [Git для Windows](https://git-for-windows.github.io/). Затем команда будет доступна в командной строке **Git bash.** `ssh`

2. Выполните шаги 1-3 (см. выше), чтобы запустить эмулятор, `adb` завершить работу сервера на компьютере Mac и найти порты эмулятора.

3. Запустите `ssh` в Windows, чтобы настроить двустороннюю переадресацию портов между локальным портом в Windows (`localhost:15555` в этом примере) и портом неограниченного эмулятора на интерфейсе замыкания на себя Mac`127.0.0.1:5555` (в этом примере):

    ```cmd 
    C:\> ssh -L localhost:15555:127.0.0.1:5555 mac-username@ip-address-of-the-mac
    ```

    Замените `mac-username` именем пользователя Mac, `whoami`указанным в списке. Замените `ip-address-of-the-mac` IP-адресом компьютера Mac.

4. Подключитесь к эмулятору с помощью локального порта в Windows:

    ```cmd
    C:\> adb connect localhost:15555
    ```

    (Примечание. один простой способ получить доступ к командной строке `adb` — это использовать [ **средства > Android > Android ADB командной** строки в Visual Studio](~/cross-platform/troubleshooting/questions/version-logs.md#adb-logcat).)

Небольшое внимание! при использовании порта `5555` для локального `adb` порта будет считать, что эмулятор выполняется локально в Windows. Это не вызывает проблем в Visual Studio, но в Visual Studio для Mac это приводит к завершению работы приложения сразу после запуска.

### <a name="alternate-technique-using-adb--h-is-not-yet-supported"></a>Альтернативный способ `adb -H` использования еще не поддерживается

Теоретически другим подходом является использование `adb`встроенных возможностей для подключения `adb` к серверу, работающему на удаленном компьютере (например [https://stackoverflow.com/a/18551325](https://stackoverflow.com/a/18551325),).
Но расширения IDE Xamarin. Android в настоящее время не предоставляют способа настройки этого параметра.

## <a name="contact-information"></a>Контактные данные

В этом документе рассматривается текущее поведение по состоянию на март 2016. Методика, описанная в этом документе, не является частью стабильного набора тестов для Xamarin, поэтому она может прерываться в будущем.

Если вы заметили, что методика больше не работает, или если вы заметили другие ошибки в документе, вы можете добавить обсуждение в следующий поток форума: [http://forums.xamarin.com/discussion/33702/android-emulator-from-host-device-inside-windows-vm](http://forums.xamarin.com/discussion/33702/android-emulator-from-host-device-inside-windows-vm).
Спасибо!
