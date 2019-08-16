---
title: Xamarin. Android Ратингбар
description: Добавление мини-приложения Ратингбар в действие Android.
ms.prod: xamarin
ms.assetid: d7a1f9bb-926d-4f93-9e8e-0fa933e330e7
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/29/2018
ms.openlocfilehash: 729daef1a7a003613bebc4f82067bc267dab8e00
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2019
ms.locfileid: "69522844"
---
# <a name="xamarinandroid-ratingbar"></a>Xamarin. Android Ратингбар

Ратингбар — это мини-приложение пользовательского интерфейса, которое отображает рейтинг от одной до пяти звезд. Пользователь может выбрать оценку, съемки на звездочку в этом разделе. вы создадите мини-приложение, позволяющее пользователю предоставить рейтинг с помощью [`RatingBar`](xref:Android.Widget.RatingBar) мини – приложения.

![Пример Ратингбар](ratingbar-images/01-ratingbar.png)


## <a name="creating-a-ratingbar"></a>Создание Ратингбар

1. Откройте файл **Resource/Layout/Main. axml** и добавьте[`RatingBar`](xref:Android.Widget.RatingBar)
   элемент (внутри [`LinearLayout`](xref:Android.Widget.LinearLayout)):

    ```xml
    <RatingBar android:id="@+id/ratingbar"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:numStars="5"
            android:stepSize="1.0"/>
    ```
   `android:numStars` Атрибут определяет количество звезд для отображения в строке рейтинга. Атрибут определяет гранулярность для каждой звезды (например, `0.5` значение, которое позволило бы получить оценку в половину звезды). `android:stepSize`

2. Чтобы сделать что-то при установке новой оценки, добавьте следующий код в конец элемента[`OnCreate()`](xref:Android.App.Activity.OnCreate*)
   Method

    ```csharp
    RatingBar ratingbar = FindViewById<RatingBar>(Resource.Id.ratingbar);

    ratingbar.RatingBarChange += (o, e) => {
            Toast.MakeText(this, "New Rating: " + ratingbar.Rating.ToString (), ToastLength.Short).Show ();
    };
    ```

    Это захватывает [`RatingBar`](xref:Android.Widget.RatingBar) мини-приложение из макета с [`FindViewById`](xref:Android.App.Activity.FindViewById*) помощью, а затем задает метод события, который определяет действие, выполняемое, когда пользователь устанавливает рейтинг. В этом случае в простом [`Toast`](xref:Android.Widget.Toast) сообщении отображается новая оценка.

3. Запустите приложение.

