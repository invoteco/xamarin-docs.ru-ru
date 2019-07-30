---
title: 'Xamarin.Essentials: Адрес эл. почты'
description: Класс Email в Xamarin.Essentials позволяет приложению открывать приложение электронной почты по умолчанию с указанной информацией, включая тему, текст и получателей (TO, CC, BCC).
ms.assetid: 5FBB6FF0-0E7B-4C29-8F06-91642AF12629
author: jamesmontemagno
ms.author: jamont
ms.date: 04/02/2019
ms.openlocfilehash: 73c398a7dbc2b8b7b22b9b8e38177efe2ff48735
ms.sourcegitcommit: 8fe8d163cb9927917f6a83204b4c387fc50181c2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/23/2019
ms.locfileid: "68388500"
---
# <a name="xamarinessentials-email"></a>Xamarin.Essentials: Адрес эл. почты

Класс **Email** позволяет приложению открывать приложение электронной почты по умолчанию с указанной информацией, включая тему, текст и получателей (TO, CC, BCC).

## <a name="get-started"></a>Начало работы

[!include[](~/essentials/includes/get-started.md)]

> [!TIP]
> Чтобы использовать API электронной почты в iOS, запустите его на физическом устройстве, в противном случае будет создано исключение.

## <a name="using-email"></a>Использование Email

Добавьте в свой класс ссылку на Xamarin.Essentials:

```csharp
using Xamarin.Essentials;
```

Функция Email выполняется путем вызова метода `ComposeAsync` и `EmailMessage`, содержащего сведения о сообщении:

```csharp
public class EmailTest
{
    public async Task SendEmail(string subject, string body, List<string> recipients)
    {
        try
        {
            var message = new EmailMessage
            {
                Subject = subject,
                Body = body,
                To = recipients,
                //Cc = ccRecipients,
                //Bcc = bccRecipients
            };
            await Email.ComposeAsync(message);
        }
        catch (FeatureNotSupportedException fbsEx)
        {
            // Email is not supported on this device
        }
        catch (Exception ex)
        {
            // Some other exception occurred
        }
    }
}
```


## <a name="file-attachments"></a>Вложения файлов

![Предварительная версия](~/media/shared/preview.png)

Отправка файлов по электронной почте доступна как экспериментальная предварительная версия в Xamarin.Essentials версии 1.1.0. Эта функция позволяет приложению вкладывать файлы в сообщения электронной почты из почтовых клиентов на устройстве. Чтобы включить эту функцию, задайте следующее свойство в коде запуска приложения.

```csharp
ExperimentalFeatures.Enable(ExperimentalFeatures.EmailAttachments);
```

После включения этой функции любой файл можно отправить по электронной почте. Xamarin.Essentials автоматически обнаруживает тип файла (MIME) и запрашивает его добавление в качестве вложения. Все почтовые клиенты отличаются в том, что поддерживают только определенные расширения файлов или не поддерживают их вообще.

Ниже приведен пример записи текста на диск и добавления его в качестве вложения электронной почты.

```csharp
var message = new EmailMessage
{
    Subject = "Hello",
    Body = "World",
};

var fn = "Attachment.txt";
var file = Path.Combine(FileSystem.CacheDirectory, fn);
File.WriteAllText(file, "Hello World");

message.Attachments.Add(new EmailAttachment(file));

await Email.ComposeAsync(message);
```

## <a name="platform-differences"></a>Различия платформ

# <a name="androidtabandroid"></a>[Android](#tab/android)

Не все почтовые клиенты для Android поддерживают `Html`. Так как не существует способа определить это, при отправке писем рекомендуем использовать `PlainText`.

# <a name="iostabios"></a>[iOS](#tab/ios)

Различия платформ отсутствуют.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Поддерживается только `PlainText`, так как если `BodyFormat` попытается отправить `Html`, возникнет исключение `FeatureNotSupportedException`.

Не все почтовые клиенты поддерживают отправку вложений. См. подробнее в [документации](https://docs.microsoft.com/windows/uwp/contacts-and-calendar/sending-email).

-----

## <a name="api"></a>API

- [Исходный код Email](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Email)
- [Документация по API Email](xref:Xamarin.Essentials.Email)
