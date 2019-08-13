---
title: Класс MessagingCenter в Xamarin.Forms
description: Класс Xamarin.Forms MessagingCenter реализует шаблон "публикация-подписка", который обеспечивает взаимодействие на основе сообщений между компонентами, которые неудобно связывать по ссылкам объектов и типов.
ms.prod: xamarin
ms.assetid: EDFE7B19-C5FD-40D5-816C-FAE56532E885
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/30/2019
ms.openlocfilehash: a4d246419c7449c2395759cf5a8b04469e7a2309
ms.sourcegitcommit: 266e75fa6893d3732e4e2c0c8e79c62be2804468
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/06/2019
ms.locfileid: "68821007"
---
# <a name="xamarinforms-messagingcenter"></a>Класс MessagingCenter в Xamarin.Forms

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/usingmessagingcenter)

Шаблон "публикация-подписка" — это шаблон обмена сообщениями, в котором издатели отправляют сообщение без знания о получателях, известных как подписчики. Аналогичным образом подписчики прослушивают определенные сообщения, не зная издателей.

Класс Xamarin.Forms [`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) реализует шаблон "публикация-подписка", который обеспечивает взаимодействие на основе сообщений между компонентами, которые неудобно связывать по ссылкам объектов и типов. Этот механизм позволяет издателям и подписчикам взаимодействовать без ссылки друг на друга, помогая уменьшить зависимости между ними.

Класс [`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) предоставляет функцию многоадресной публикации и подписки. Это означает, что может существовать несколько издателей, которые публикуют одно сообщение, и может быть несколько подписчиков, прослушивающих одно и то же сообщение:

![](messaging-center-images/messaging-center.png "Функция многоадресной публикации и подписки")

Издатели отправляют сообщения с помощью метода [`MessagingCenter.Send`](xref:Xamarin.Forms.MessagingCenter.Send*), а подписчики прослушивают сообщения с помощью метода [`MessagingCenter.Subscribe`](xref:Xamarin.Forms.MessagingCenter.Subscribe*). Кроме того, подписчики могут также отменять подписку на сообщения, если это необходимо, с помощью метода [`MessagingCenter.Unsubscribe`](xref:Xamarin.Forms.MessagingCenter.Unsubscribe*).

> [!IMPORTANT]
> На внутреннем уровне класс [`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) использует слабые ссылки. Это означает, что он не будет поддерживать объекты в активном состоянии и позволит им собирать мусор. Поэтому необходимо отменять подписку на сообщения только в том случае, если классу больше не требуется получать сообщения.

## <a name="publish-a-message"></a>Публикация сообщения

Сообщения [`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) являются строками. Издатели уведомляют подписчиков о сообщении с помощью одной из перегрузок [`MessagingCenter.Send`](xref:Xamarin.Forms.MessagingCenter.Send*). Следующий код публикует события `Hi`.

```csharp
MessagingCenter.Send<MainPage>(this, "Hi");
```

В этом примере метод [`Send`](xref:Xamarin.Forms.MessagingCenter.Send*) задает универсальный аргумент, представляющий отправителя. Чтобы получить сообщение, подписчик также должен указать тот же универсальный аргумент, указывающий, что он прослушивает сообщение от этого отправителя. Кроме того, в этом примере указываются два аргумента метода:

- Первый аргумент указывает экземпляр отправителя.
- Второй аргумент указывает само сообщение.

Полезные данные также могут быть отправлены с сообщением:

```csharp
MessagingCenter.Send<MainPage, string>(this, "Hi", "John");
```

В этом примере метод [`Send`](xref:Xamarin.Forms.MessagingCenter.Send*) задает два универсальных аргумента. Первый — это тип, который отправляет сообщение, а второй — тип отправляемых полезных данных. Чтобы получить сообщение, подписчик также должен указать те же универсальные аргументы. Это позволяет разным подписчикам получать несколько сообщений с одним удостоверением сообщения, но различными типами полезных данных. Кроме того, в этом примере задается третий аргумент метода, представляющий полезные данные для отправки подписчику. В этом случае данные — это `string`.

Метод [`Send`](xref:Xamarin.Forms.MessagingCenter.Send*) опубликует сообщение и все полезные данные в стиле "отправил и забыл". Поэтому сообщение отправляется, даже если отсутствуют подписчики, зарегистрированные для получения сообщения. В этом случае отправленное сообщение игнорируется.

## <a name="subscribe-to-a-message"></a>Оформление подписки на сообщение

Подписчики могут зарегистрироваться для получения сообщения с помощью одной из перегрузок [`MessagingCenter.Subscribe`](xref:Xamarin.Forms.MessagingCenter.Subscribe*). В следующем коде показан пример такого действия:

```csharp
MessagingCenter.Subscribe<MainPage> (this, "Hi", (sender) =>
{
    // Do something whenever the "Hi" message is received
});
```

В этом примере метод [`Subscribe`](xref:Xamarin.Forms.MessagingCenter.Subscribe*) подписывает объект `this` на сообщения `Hi`, отправляемые типом `MainPage`, и выполняет делегат обратного вызова в ответ на получение сообщения. Делегат обратного вызова, указанный в качестве лямбда-выражения, может быть кодом, который обновляет пользовательский интерфейс, сохраняет данные или запускает другую операцию.

> [!NOTE]
> Подписчику может не потребоваться обрабатывать каждый экземпляр опубликованного сообщения, и это можно контролировать с помощью аргументов универсального типа, указанных в методе [`Subscribe`](xref:Xamarin.Forms.MessagingCenter.Subscribe*).

Следующий пример демонстрирует, как подписаться на сообщение, содержащее полезные данные.

```csharp
MessagingCenter.Subscribe<MainPage, string>(this, "Hi", async (sender, arg) =>
{
    await DisplayAlert("Message received", "arg=" + arg, "OK");
});
```

В этом примере метод [`Subscribe`](xref:Xamarin.Forms.MessagingCenter.Subscribe*) подписывается на сообщения `Hi`, отправляемые типом `MainPage`, полезные данные которого — `string`. В ответ на получение такого сообщения выполняется делегат обратного вызова, который отображает полезные данные в оповещении.

## <a name="unsubscribe-from-a-message"></a>Отмена подписки на сообщение

Если подписчики больше не должны получать сообщения, можно отменить подписку на них. Для этого используется одна из перегрузок [`MessagingCenter.Unsubscribe`](xref:Xamarin.Forms.MessagingCenter.Unsubscribe*).

```csharp
MessagingCenter.Unsubscribe<MainPage>(this, "Hi");
```

В этом примере метод [`Unsubscribe`](xref:Xamarin.Forms.MessagingCenter.Unsubscribe*) отменяет подписывание объекта `this` на сообщения `Hi`, отправляемые типом `MainPage`.

Подписку на сообщения, содержащие полезные данные, необходимо отменять с помощью перегрузки [`Unsubscribe`](xref:Xamarin.Forms.MessagingCenter.Unsubscribe*), указывающей два универсальных аргумента:

```csharp
MessagingCenter.Unsubscribe<MainPage, string>(this, "Hi");
```

В этом примере метод [`Unsubscribe`](xref:Xamarin.Forms.MessagingCenter.Unsubscribe*) отменяет подписывание объекта `this` на сообщения `Hi`, отправляемые типом `MainPage`, полезные данные которого — `string`.

## <a name="related-links"></a>Связанные ссылки

- [MessagingCenterSample](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/usingmessagingcenter)
