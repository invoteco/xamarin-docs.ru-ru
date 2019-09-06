---
title: Возвращено исключение System.Exception AMDeviceNotificationSubscribe…
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 7E4ACC7E-F4FB-46C1-8837-C7FBAAFB2DC7
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/21/2017
ms.openlocfilehash: 8ae465c98dee25cd0f1fe635da45f4d399b42ee3
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/04/2019
ms.locfileid: "70284485"
---
# <a name="systemexception-amdevicenotificationsubscribe-returned-"></a>Возвращено исключение System.Exception AMDeviceNotificationSubscribe…

> [!IMPORTANT]
> Эта проблема решена в последних версиях Xamarin. Однако если проблема возникает в последней версии программного обеспечения, запишите [новую ошибку](~/cross-platform/troubleshooting/questions/howto-file-bug.md) с полными сведениями о версии и полным выходным файлом журнала сборки.


## <a name="fix"></a>Fix

1. Завершите `usbmuxd` процесс, чтобы система перезапустила его:

    ```csharp
    sudo killall -QUIT usbmuxd
    ```

2. Если проблема не устранена, перезагрузите компьютер Mac.

## <a name="error-message"></a>Сообщение об ошибке

```csharp
Exception: Exception type: System.Exception
AMDeviceNotificationSubscribe returned: 3892314211
  at Xamarin.MacDev.IPhoneDeviceManager.SubscribeToNotifications () [0x00000] in <filename unknown="">:0
  at Xamarin.MacDev.IPhoneDeviceManager.StartWatching (Boolean useOwnRunloop) [0x00000] in <filename unknown="">:0
  at Mtb.Server.DeviceListener.Start () [0x00000] in <filename unknown="">:0
  at Mtb.Application.MainClass.RunDeviceListener () [0x00000] in <filename unknown="">:0
  at Mtb.Application.MainClass.Main (System.String[] args) [0x00000] in <filename unknown="">:0
```

Это сообщение может появиться в диалоговом окне об ошибке при первом запуске Visual Studio для Mac или в `mtbserver.log` файле в приложении сборки Xamarin. iOS (**узел сборки Xamarin. IOS > Просмотр журнала узла сборки**).

Обратите внимание, что это нетипичная проблема. Если Visual Studio не удается подключиться к узлу сборки Mac, существуют другие ошибки, которые скорее всего появятся в `mtbserver.log` файле.

### <a name="errors-in-systemlog"></a>Ошибки в System. log

В некоторых случаях следующие два сообщения об ошибках также могут многократно `/var/log/system.log`появляться в:

```csharp
17:17:11.369 usbmuxd[55040]: dnssd_clientstub ConnectToServer: socket failed 24 Too many open files
17:17:11.369 com.apple.usbmuxd[55040]: _BrowseReplyReceivedCallback Error doing DNSServiceResolve(): -65539
```

## <a name="additional-information"></a>Дополнительные сведения

Одно из предположения, вызвавшее ошибку, заключается в том, что системные службы OS X, ответственные за сведения об устройствах и симуляторах iOS, могут в редких случаях вводить непредвиденное состояние. Xamarin не может правильно взаимодействовать с системными службами в этом состоянии. Перезагрузка компьютера перезапускает системные службы и решает проблему.

В зависимости от `system.log` показанных здесь ошибок эта проблема может быть связана с Bonjour (`mDNSResponder`). Изменение между различными сетями Wi-Fi, кажется, приводит к увеличению шансов на проблему.

## <a name="references"></a>Ссылки

* [Ошибка 11789-Мобиледевице. Мобиледевицеексцептион: Амдевиценотификатионсубскрибе вернул: 0xe8000063 [РАЗРЕШЕНный ответ]](https://bugzilla.xamarin.com/show_bug.cgi?id=11789)
