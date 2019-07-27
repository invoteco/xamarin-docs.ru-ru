---
title: Службы с намерением в Xamarin. Android
ms.prod: xamarin
ms.assetid: A5B86FE4-C8E2-4B0A-84CA-EF8F5119E31B
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 4c868623ae08ac1366c1c9ea55c8d635f0a6a061
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/26/2019
ms.locfileid: "68509138"
---
# <a name="intent-services-in-xamarinandroid"></a>Службы с намерением в Xamarin. Android

## <a name="intent-services-overview"></a>Общие сведения о службах намерения

Запущенные и привязанные службы выполняются в основном потоке, что означает, что для обеспечения плавности производительности службе необходимо асинхронно выполнять работу. Одним из самых простых способов решения этой проблемы является _шаблон обработчика рабочей очереди_, где работа, которую необходимо выполнить, помещается в очередь, обслуживаемую одним потоком.

`Service` Класс [`IntentService`](xref:Android.App.IntentService) является подклассом класса, который предоставляет реализацию этого шаблона для Android. Он будет управлять работой очередей, запускать рабочий поток для обслуживания очереди и выдавать запросы из очереди для выполнения в рабочем потоке. Компонент `IntentService` будет беззвучно останавливаться и удалять рабочий поток, если в очереди больше нет работы.

Работа отправляется в очередь путем создания `Intent` и последующей `Intent` передачи в `StartService` метод.

Невозможно остановить или прервать `OnHandleIntent` выполнение метода `IntentService` во время его работы. В `IntentService` связи с этим проект должен храниться без &ndash; отслеживания состояния, он не должен полагаться на активное соединение или связь с остальной частью приложения. `IntentService` Стателессли предназначен для обработки рабочих запросов.

Для подклассов `IntentService`существует два требования.

1. Новый тип (созданный подклассом `IntentService`) `OnHandleIntent` переопределяет только метод.
2. Конструктору для нового типа требуется строка, которая используется для именования рабочего потока, который будет выполнять запросы. Имя этого рабочего потока в основном используется при отладке приложения.

В следующем коде показана `IntentService` реализация с переопределенным `OnHandleIntent` методом:

```csharp
[Service]
public class DemoIntentService: IntentService
{
    public DemoIntentService () : base("DemoIntentService")
    {
    }

    protected override void OnHandleIntent (Android.Content.Intent intent)
    {
        Console.WriteLine ("perform some long running work");
        ...
        Console.WriteLine ("work complete");
    }
}
```

Работа отправляется в объект `IntentService` путем создания экземпляра `Intent` и последующего вызова [`StartService`](xref:Android.Content.Context.StartService*) метода с этим намерением в качестве параметра. Цель будет передаваться в службу в качестве параметра в `OnHandleIntent` методе. Этот фрагмент кода является примером отправки рабочего запроса в цель: 

```csharp
// This code might be called from within an Activity, for example in an event
// handler for a button click.
Intent downloadIntent = new Intent(this, typeof(DemoIntentService));

// This is just one example of passing some values to an IntentService via the Intent:
downloadIntent.Put
("file_to_download", "http://www.somewhere.com/file/to/download.zip");

StartService(downloadIntent);
```

`IntentService` Может извлекать значения из цели, как показано в следующем фрагменте кода:  

```csharp
protected override void OnHandleIntent (Android.Content.Intent intent)
{
    string fileToDownload = intent.GetStringExtra("file_to_download");

    Log.Debug("DemoIntentService", $"File to download: {fileToDownload}.");
}
```

## <a name="related-links"></a>Связанные ссылки

- [IntentService](xref:Android.App.IntentService)
- [StartService](xref:Android.Content.Context.StartService*)
