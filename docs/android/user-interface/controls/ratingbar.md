---
title: Xamarin. Android Ратингбар
description: Добавление мини-приложения Ратингбар в действие Android.
ms.prod: xamarin
ms.assetid: d7a1f9bb-926d-4f93-9e8e-0fa933e330e7
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 08/29/2018
ms.openlocfilehash: 529fecb4e24e83ef7b783815843e132347d99262
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029143"
---
# <a name="xamarinandroid-ratingbar"></a>Xamarin. Android Ратингбар

Ратингбар — это мини-приложение пользовательского интерфейса, которое отображает рейтинг от одной до пяти звезд. Пользователь может выбрать оценку, съемки на звездочку в этом разделе. вы создадите мини-приложение, позволяющее пользователю предоставить рейтинг с помощью мини-приложения [`RatingBar`](xref:Android.Widget.RatingBar) .

![Пример Ратингбар](ratingbar-images/01-ratingbar.png)

## <a name="creating-a-ratingbar"></a>Создание Ратингбар

1. Откройте файл **Resource/Layout/Main. axml** и добавьте [`RatingBar`](xref:Android.Widget.RatingBar)
   элемент (внутри [`LinearLayout`](xref:Android.Widget.LinearLayout)):

   ```xml
   <RatingBar android:id="@+id/ratingbar"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:numStars="5"
            android:stepSize="1.0"/>
   ```

   Атрибут `android:numStars` определяет количество звезд для отображения в строке рейтинга. Атрибут `android:stepSize` определяет гранулярность для каждой звезды (например, значение `0.5` будет допускать оценку в половину звезды).

2. Чтобы сделать что-то при установке новой оценки, добавьте следующий код в конец [`OnCreate()`](xref:Android.App.Activity.OnCreate*)
   Method

    ```csharp
    RatingBar ratingbar = FindViewById<RatingBar>(Resource.Id.ratingbar);

    ratingbar.RatingBarChange += (o, e) => {
            Toast.MakeText(this, "New Rating: " + ratingbar.Rating.ToString (), ToastLength.Short).Show ();
    };
    ```

    Это захватывает мини-приложение [`RatingBar`](xref:Android.Widget.RatingBar) из макета с [`FindViewById`](xref:Android.App.Activity.FindViewById*) , а затем задает метод события, который затем определяет действие, выполняемое, когда пользователь задает оценку. В этом случае в простом [`Toast`](xref:Android.Widget.Toast) сообщении отображается новая оценка.

3. Запустите приложение.
