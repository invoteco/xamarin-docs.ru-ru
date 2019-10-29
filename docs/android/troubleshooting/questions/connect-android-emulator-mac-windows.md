---
title: Можно ли подключиться с виртуальной машины Windows к эмуляторам Android под управлением Mac?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 7B6752BB-8E4C-4690-B275-7E425A051F45
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/21/2018
ms.openlocfilehash: 2c1f571efb9ec3fb726912eb1e30496bc51fe26e
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73026988"
---
# <a name="is-it-possible-to-connect-to-android-emulators-running-on-a-mac-from-a-windows-vm"></a>Можно ли подключиться с виртуальной машины Windows к эмуляторам Android под управлением Mac?

Чтобы подключиться к Android Emulator, работающему на компьютере Mac, с виртуальной машины Windows, выполните следующие действия.

1. Запустите эмулятор на компьютере Mac.

2. Прервать `adb` сервер на компьютере Mac:

    ```bash
    adb kill-server
    ```

3. Обратите внимание, что эмулятор прослушивает 2 порта TCP в интерфейсе замыкания на себя:

    ```bash
    lsof -iTCP -sTCP:LISTEN -P | grep 'emulator\|qemu'

    emulator6 94105 macuser   20u  IPv4 0xa8dacfb1d4a1b51f      0t0  TCP localhost:5555 (LISTEN)
    emulator6 94105 macuser   21u  IPv4 0xa8dacfb1d845a51f      0t0  TCP localhost:5554 (LISTEN)
    ```

    Порт с нечетным номером используется для подключения к `adb`. См. также [https://developer.android.com/tools/devices/emulator.html#emulatornetworking](https://developer.android.com/tools/devices/emulator.html#emulatornetworking).

4. _Вариант 1_. Использование [`nc`](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man1/nc.1.html)
    чтобы пересылать входящие пакеты TCP, полученные извне через порт 5555 (или любой другой порт), на порт с нечетным номером в интерфейсе замыкания на себя (**127.0.0.1 5555** в этом примере) и пересылать исходящие пакеты другим способом:

    ```bash
    cd /tmp
    mkfifo backpipe
    nc -kl 5555 0<backpipe | nc 127.0.0.1 5555 > backpipe
    ```

    При условии, что команды `nc` остаются в окне терминала, пакеты будут перенаправлены должным образом. Вы можете ввести в окне терминала элемент управления-C, чтобы выйти из `nc` команд после завершения работы с эмулятором.

    (Вариант 1 обычно проще, чем вариант 2, особенно если **Системные настройки > безопасность & конфиденциальность > брандмауэр** включен.) 

    _Вариант 2_. Использование [`pfctl`](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man8/pfctl.8.html)
    чтобы перенаправить пакеты TCP с порта `5555` (или любого другого порта, который вам нравится) на [общем сетевом](https://kb.parallels.com/en/4948) интерфейсе для неограниченного порта в интерфейсе замыкания на себя (`127.0.0.1:5555` в этом примере):

    ```bash
    sed '/rdr-anchor/a rdr pass on vmnet8 inet proto tcp from any to any port 5555 -> 127.0.0.1 port 5555' /etc/pf.conf | sudo pfctl -ef -
    ```

    Эта команда настраивает перенаправление портов с помощью системной службы `pf packet filter`. Разрывы строк важны. Не забудьте обеспечить их неизменность при копировании. Кроме того, при использовании параллельных вычислений необходимо изменить имя интерфейса с *VMnet8* . `vmnet8` — это имя Специального *устройства NAT* для режима *общей сетевой сети* в слиянии VMware. Вполне вероятно, что соответствующий сетевой интерфейс в параллелях [vnic0](https://download.parallels.com/doc/psbm/en/Parallels_Server_Bare_Metal_Users_Guide/29258.htm).

5. Подключитесь к эмулятору с компьютера Windows:

    ```cmd
    C:\> adb connect ip-address-of-the-mac:5555
    ```

    Замените IP-address-of-Mac на IP-адрес компьютера Mac, например в списке `ifconfig vmnet8 | grep 'inet '`. При необходимости замените `5555` другим портом, который вы хотите на шаге 4\. (Примечание. один из способов получения доступа из командной строки к `adb` — с помощью [**инструментов > android > Android ADB Командная**](~/cross-platform/troubleshooting/questions/version-logs.md#adb-logcat) строка в Visual Studio.)

### <a name="alternate-technique-using-ssh"></a>Альтернативный способ использования `ssh`

Если вы включили _Удаленный вход_ на компьютере Mac, для подключения к эмулятору можно использовать `ssh` перенаправление портов.

1. Установите SSH-клиент в Windows. Один из вариантов — установить [Git для Windows](https://git-for-windows.github.io/). Затем команда `ssh` будет доступна в командной строке **Git Bash** .

2. Выполните шаги 1-3 (см. выше), чтобы запустить эмулятор, завершить работу сервера `adb` на компьютере Mac и задать порты эмулятора.

3. Запустите `ssh` в Windows, чтобы настроить двустороннюю переадресацию портов между локальным портом в Windows (`localhost:15555` в этом примере) и портом нестандартного эмулятора на интерфейсе замыкания на себя Mac (`127.0.0.1:5555` в этом примере):

    ```cmd 
    C:\> ssh -L localhost:15555:127.0.0.1:5555 mac-username@ip-address-of-the-mac
    ```

    Замените `mac-username` именем пользователя Mac, как указано в `whoami`. Замените `ip-address-of-the-mac` IP-адресом компьютера Mac.

4. Подключитесь к эмулятору с помощью локального порта в Windows:

    ```cmd
    C:\> adb connect localhost:15555
    ```

    (Примечание. простой способ получить доступ к `adb` из командной строки с помощью [ **средств > Android > Android ADB** в Visual Studio](~/cross-platform/troubleshooting/questions/version-logs.md#adb-logcat).)

Небольшое внимание! если для локального порта используется порт `5555`, `adb` считает, что эмулятор выполняется локально в Windows. Это не вызывает проблем в Visual Studio, но в Visual Studio для Mac это приводит к завершению работы приложения сразу после запуска.

### <a name="alternate-technique-using-adb--h-is-not-yet-supported"></a>Альтернативный метод, использующий `adb -H`, пока не поддерживается

Теоретически другой подход заключается в использовании встроенной возможности `adb`для подключения к `adb` серверу, работающему на удаленном компьютере (см. пример [https://stackoverflow.com/a/18551325](https://stackoverflow.com/a/18551325)).
Но расширения IDE Xamarin. Android в настоящее время не предоставляют способа настройки этого параметра.

## <a name="contact-information"></a>Контактные данные

В этом документе рассматривается текущее поведение по состоянию на март 2016. Методика, описанная в этом документе, не является частью стабильного набора тестов для Xamarin, поэтому она может прерываться в будущем.

Если вы заметили, что методика больше не работает, или если вы заметили другие ошибки в документе, вы можете добавить обсуждение в следующий поток форума: [http://forums.xamarin.com/discussion/33702/android-emulator-from-host-device-inside-windows-vm](https://forums.xamarin.com/discussion/33702/android-emulator-from-host-device-inside-windows-vm).
Спасибо!
