---
title: Работа с потоком пользовательского интерфейса в Xamarin. iOS
description: В этом документе описывается работа с потоком пользовательского интерфейса в Xamarin. iOS. Он обсуждает выполнение потока пользовательского интерфейса, предоставляет пример фонового потока и проверяет async/await.
ms.prod: xamarin
ms.assetid: 98762ACA-AD5A-4E1E-A536-7AF3BE36D77E
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: ee7ab7c5d0503cffd2c12a493f314f191d912e92
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73002833"
---
# <a name="working-with-the-ui-thread-in-xamarinios"></a>Работа с потоком пользовательского интерфейса в Xamarin. iOS

Пользовательские интерфейсы приложения всегда являются однопотоковыми, даже на многопоточных устройствах — существует только одно представление экрана, а все изменения, отображаемые для отображения, необходимо координировать с помощью одной точки доступа. Это предотвращает попытку одновременного обновления одного и того же пикселя несколькими потоками (например,).

Код должен вносить изменения только в элементы управления пользовательского интерфейса из основного потока (или пользовательского интерфейса). Любые обновления пользовательского интерфейса, происходящие в другом потоке (например, обратный вызов или фоновый поток), могут не отображаться на экране или даже вызывать сбой.

## <a name="ui-thread-execution"></a>Выполнение потока пользовательского интерфейса

При создании элементов управления в представлении или обработке инициированного пользователем события, такого как касание, код уже выполняется в контексте потока пользовательского интерфейса.

Если код выполняется в фоновом потоке, в задаче или обратном вызове, скорее всего, он не выполняется в основном потоке пользовательского интерфейса. В этом случае код следует заключить в вызов `InvokeOnMainThread` или `BeginInvokeOnMainThread` следующим образом:

```csharp
InvokeOnMainThread ( () => {
    // manipulate UI controls
});
```

Метод `InvokeOnMainThread` определяется на `NSObject`, поэтому его можно вызывать из методов, определенных для любого объекта UIKit (например, представления или контроллера представления).

При отладке приложений Xamarin. iOS возникнет ошибка, если код пытается получить доступ к элементу управления пользовательского интерфейса из неправильного потока. Это поможет вам отвестись и устранить эти проблемы с помощью метода Инвокеонмаинсреад. Это происходит только при отладке и не вызывает ошибку в сборках выпуска. Сообщение об ошибке будет выглядеть следующим образом:

 ![](ui-thread-images/image10.png "UI Thread Execution")

 <a name="Background_Thread_Example" />

## <a name="background-thread-example"></a>Пример фонового потока

Ниже приведен пример, который пытается получить доступ к элементу управления пользовательского интерфейса (`UILabel`) из фонового потока с помощью простого потока:

```csharp
new System.Threading.Thread(new System.Threading.ThreadStart(() => {
    label1.Text = "updated in thread"; // should NOT reference UILabel on background thread!
})).Start();
```

Этот код вызовет `UIKitThreadAccessException` во время отладки. Чтобы устранить проблему (и обеспечить доступ к элементу управления пользовательского интерфейса только из основного потока пользовательского интерфейса), заключите код, который ссылается на элементы управления пользовательского интерфейса в выражении `InvokeOnMainThread`, следующим образом:

```csharp
new System.Threading.Thread(new System.Threading.ThreadStart(() => {
    InvokeOnMainThread (() => {
        label1.Text = "updated in thread"; // this works!
    });
})).Start();
```

Это не потребуется для оставшейся части примеров в этом документе, но важно помнить, когда приложение выполняет сетевые запросы, использует Центр уведомлений или другие методы, требующие обработчика завершения, который будет выполняться на другом поток.

 <a name="Async_Await_Example" />

## <a name="asyncawait-example"></a>Пример async/await

При использовании C# 5 ключевых слов async/await`InvokeOnMainThread`не требуется, поскольку при завершении ожидающей задачи метод продолжит работу в вызывающем потоке.

В этом примере кода (который ожидает вызова метода Delay, исключительно для демонстрационных целей) показан асинхронный метод, который вызывается в потоке пользовательского интерфейса (это обработчик Таучупинсиде). Поскольку содержащийся метод вызывается в потоке пользовательского интерфейса, операции пользовательского интерфейса, такие как установка текста на `UILabel` или отображение `UIAlertView` можно безопасно вызывать после завершения асинхронных операций в фоновых потоках.

```csharp
async partial void button2_TouchUpInside (UIButton sender)
{
    textfield1.ResignFirstResponder ();
    textfield2.ResignFirstResponder ();
    textview1.ResignFirstResponder ();
    label1.Text = "async method started";
    await Task.Delay(1000); // example purpose only
    label1.Text = "1 second passed";
    await Task.Delay(2000);
    label1.Text = "2 more seconds passed";
    await Task.Delay(1000);
    new UIAlertView("Async method complete", "This method", 
               null, "Cancel", null)
        .Show();
    label1.Text = "async method completed";
}
```

Если асинхронный метод вызывается из фонового потока (не основного потока пользовательского интерфейса), то `InvokeOnMainThread` по-прежнему потребуется.

## <a name="related-links"></a>Связанные ссылки

- [Элементы управления (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/controls)
- [Работа с потоками](~/ios/app-fundamentals/threading.md)
