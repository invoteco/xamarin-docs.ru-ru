---
title: Службы с намерением в Xamarin. Android
ms.prod: xamarin
ms.assetid: A5B86FE4-C8E2-4B0A-84CA-EF8F5119E31B
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: c58787a051bfc965cb7493138ed6114ac23ed04d
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73024850"
---
# <a name="intent-services-in-xamarinandroid"></a>Службы с намерением в Xamarin. Android

## <a name="intent-services-overview"></a>Общие сведения о службах намерения

Запущенные и привязанные службы выполняются в основном потоке, что означает, что для обеспечения плавности производительности службе необходимо асинхронно выполнять работу. Одним из самых простых способов решения этой проблемы является _шаблон обработчика рабочей очереди_, где работа, которую необходимо выполнить, помещается в очередь, обслуживаемую одним потоком.

[`IntentService`](xref:Android.App.IntentService) является подклассом класса `Service`, который предоставляет реализацию этого шаблона, относящуюся к Android. Он будет управлять работой очередей, запускать рабочий поток для обслуживания очереди и выдавать запросы из очереди для выполнения в рабочем потоке. `IntentService` будет беззвучно останавливаться и удалять рабочий поток, если в очереди больше нет работы.

Работа отправляется в очередь путем создания `Intent` и последующей передачи `Intent` методу `StartService`.

Невозможно остановить или прервать метод `OnHandleIntent` `IntentService` во время работы. Из-за такой структуры `IntentService` следует сохранять без отслеживания состояния &ndash; он не должен полагаться на активное подключение или связь с остальной частью приложения. `IntentService` предназначен для стателессли обработки рабочих запросов.

Существует два требования к подклассам `IntentService`.

1. Новый тип (созданный подклассом `IntentService`) переопределяет только метод `OnHandleIntent`.
2. Конструктору для нового типа требуется строка, которая используется для именования рабочего потока, который будет выполнять запросы. Имя этого рабочего потока в основном используется при отладке приложения.

В следующем коде показана реализация `IntentService` с переопределенным методом `OnHandleIntent`:

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

Работа отправляется в `IntentService` путем создания экземпляра `Intent` и последующего вызова метода [`StartService`](xref:Android.Content.Context.StartService*) с этим намерением в качестве параметра. Цель будет передаваться в службу в качестве параметра в методе `OnHandleIntent`. Этот фрагмент кода является примером отправки рабочего запроса в цель: 

```csharp
// This code might be called from within an Activity, for example in an event
// handler for a button click.
Intent downloadIntent = new Intent(this, typeof(DemoIntentService));

// This is just one example of passing some values to an IntentService via the Intent:
downloadIntent.Put
("file_to_download", "http://www.somewhere.com/file/to/download.zip");

StartService(downloadIntent);
```

`IntentService` может извлекать значения из цели, как показано в следующем фрагменте кода:  

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
