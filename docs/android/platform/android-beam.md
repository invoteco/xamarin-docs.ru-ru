---
title: Android Beam
ms.prod: xamarin
ms.assetid: 4172A798-89EC-444D-BC0C-0A7DD67EF98C
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/06/2017
ms.openlocfilehash: 11f1d38eb10421d4ecef3e2039688b2d9a6d9e6e
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73027832"
---
# <a name="android-beam"></a>Android Beam

Устройства с Android — это технология NFC, появившаяся в Android 4,0, которая позволяет приложениям обмениваться информацией через NFC при близком близком расположении.

[Диаграмма![, иллюстрирующая два устройства в окне сведений о близком доступе](android-beam-images/androidbeam.png)](android-beam-images/androidbeam.png#lightbox)

Технология Android поддерживает передачу сообщений через NFC, если два устройства находятся в радиусе действия. Устройства, связанные с 4cm, могут обмениваться данными с помощью устройств с Android. Действие на одном устройстве создает сообщение и указывает действие (или действия), которое может выполнять его принудительную отправку. Если указанное действие находится на переднем плане, а устройства находятся в диапазоне, устройство Android отправляет сообщение на второе. На принимающем устройстве вызывается намерение, содержащее данные сообщения.

Android поддерживает два способа настройки сообщений с помощью устройства Android:

- `SetNdefPushMessage`-до инициации устройства Android, приложение может вызвать Сетндефпушмессаже, чтобы указать Ндефмессаже для отправки через NFC, и действие, которое отправляет его. Этот механизм лучше использовать, если сообщение не изменяется во время использования приложения.

- `SetNdefPushMessageCallback` — когда инициируется образ Android, приложение может реализовать обратный вызов для создания Ндефмессаже. Этот механизм позволяет отложить создание сообщений до тех пор, пока устройства не будут находиться в диапазоне. Он поддерживает сценарии, в которых сообщение может изменяться в зависимости от того, что происходит в приложении.

В любом случае, чтобы отправлять данные с помощью устройства Android, приложение отправляет `NdefMessage`, упаковывая данные в несколько `NdefRecords`. Давайте рассмотрим ключевые моменты, которые необходимо устранить, прежде чем можно будет активировать образ Android. Во первых, мы будем работать с стилем обратного вызова создания `NdefMessage`.

## <a name="creating-a-message"></a>Создание сообщения

Можно зарегистрировать обратные вызовы с помощью `NfcAdapter` в методе `OnCreate` действия. Например, если `NfcAdapter` с именем `mNfcAdapter` объявляется как переменная класса в действии, можно написать следующий код для создания обратного вызова, который будет формировать сообщение:

```csharp
mNfcAdapter = NfcAdapter.GetDefaultAdapter (this);
mNfcAdapter.SetNdefPushMessageCallback (this, this);
```

Действие, которое реализует `NfcAdapter.ICreateNdefMessageCallback`, передается методу `SetNdefPushMessageCallback` выше. Когда инициируется образ Android, система вызывает `CreateNdefMessage`, из которой действие может создать `NdefMessage`, как показано ниже:

```csharp
public NdefMessage CreateNdefMessage (NfcEvent evt)
{
    DateTime time = DateTime.Now;
    var text = ("Beam me up!\n\n" + "Beam Time: " +
        time.ToString ("HH:mm:ss"));
    NdefMessage msg = new NdefMessage (
        new NdefRecord[]{ CreateMimeRecord (
            "application/com.example.android.beam",
            Encoding.UTF8.GetBytes (text)) });
        } };
    return msg;
}

public NdefRecord CreateMimeRecord (String mimeType, byte [] payload)
{
    byte [] mimeBytes = Encoding.UTF8.GetBytes (mimeType);
    NdefRecord mimeRecord = new NdefRecord (
        NdefRecord.TnfMimeMedia, mimeBytes, new byte [0], payload);
    return mimeRecord;
}
```

## <a name="receiving-a-message"></a>Получение сообщения

На принимающей стороне система вызывает намерение с действием `ActionNdefDiscovered`, из которого можно извлечь Ндефмессаже следующим образом:

```csharp
IParcelable [] rawMsgs = intent.GetParcelableArrayExtra (NfcAdapter.ExtraNdefMessages);
NdefMessage msg = (NdefMessage) rawMsgs [0];
```

Полный пример кода, в котором используется образ Android, показанный на снимке экрана ниже, см. в [демонстрационном ролике по Android](https://docs.microsoft.com/samples/xamarin/monodroid-samples/androidbeamdemo) в галерее примеров.

[![пример снимков экрана с демонстрационной версией Android](android-beam-images/24.png)](android-beam-images/24.png#lightbox)

## <a name="related-links"></a>Связанные ссылки

- [Демонстрация для Android (пример)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/androidbeamdemo)
- [Знакомство со Южным Сандвичевым](https://www.android.com/about/ice-cream-sandwich/)
- [Платформа Android 4,0](https://developer.android.com/sdk/android-4.0.html)
