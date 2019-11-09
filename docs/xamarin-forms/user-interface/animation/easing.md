---
title: Ускорение функций в Xamarin. Forms
description: Xamarin. Forms включает класс замедления, который позволяет указать функцию передачи, которая управляет скоростью анимации или замедляется при их выполнении. В этой статье показано, как использовать предварительно определенные функции плавности и как создавать пользовательские функции плавности.
ms.prod: xamarin
ms.assetid: E6F124C7-A161-4C1F-AF40-52F0935E54DE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/14/2016
ms.openlocfilehash: 56ea31d1e1be8bbad4a27dd7ffd844aa03f75bbb
ms.sourcegitcommit: efbc69acf4ea484d8815311b058114379c9db8a2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/08/2019
ms.locfileid: "73842835"
---
# <a name="easing-functions-in-xamarinforms"></a>Ускорение функций в Xamarin. Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-animation-easing)

_Xamarin. Forms включает класс замедления, который позволяет указать функцию передачи, которая управляет скоростью анимации или замедляется при их выполнении. В этой статье показано, как использовать предварительно определенные функции плавности и как создавать пользовательские функции плавности._

Класс [`Easing`](xref:Xamarin.Forms.Easing) определяет ряд функций плавности, которые могут использоваться анимациями:

- Функция плавности [`BounceIn`](xref:Xamarin.Forms.Easing.BounceIn) посылает анимацию в начале.
- Функция плавности [`BounceOut`](xref:Xamarin.Forms.Easing.BounceOut) посылает анимацию в конце.
- Функция плавности [`CubicIn`](xref:Xamarin.Forms.Easing.CubicIn) медленно ускоряет анимацию.
- Функция плавности [`CubicInOut`](xref:Xamarin.Forms.Easing.CubicInOut) ускоряет анимацию в начале и замедляет анимацию в конце.
- Функция плавности [`CubicOut`](xref:Xamarin.Forms.Easing.CubicOut) быстро замедляет анимацию.
- Функция плавности [`Linear`](xref:Xamarin.Forms.Easing.Linear) использует постоянную скорость, а — функцию плавности по умолчанию.
- Функция плавности [`SinIn`](xref:Xamarin.Forms.Easing.SinIn) плавно ускоряет анимацию.
- Функция плавности [`SinInOut`](xref:Xamarin.Forms.Easing.SinInOut) плавно ускоряет анимацию в начале и плавно замедляет анимацию в конце.
- Функция плавной [`SinOut`](xref:Xamarin.Forms.Easing.SinOut) плавно замедляет анимацию.
- Функция плавности [`SpringIn`](xref:Xamarin.Forms.Easing.SpringIn) приводит к быстрому ускорению анимации в сторону конца.
- Функция плавности [`SpringOut`](xref:Xamarin.Forms.Easing.SpringOut) приводит к быстрому замедлению анимации в направлении конца.

Суффиксы `In` и `Out` указывают, является ли эффект, предоставляемый функцией плавности, заметным в начале анимации, в конце или в обоих.

Кроме того, можно создавать пользовательские функции плавности. Дополнительные сведения см. в разделе [пользовательские функции плавности](#customeasing).

## <a name="consuming-an-easing-function"></a>Использование функции плавности

Методы расширения анимации в классе [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) позволяют указать функцию плавности в качестве завершающего параметра метода, как показано в следующем примере кода:

```csharp
await image.TranslateTo(0, 200, 2000, Easing.BounceIn);
await image.ScaleTo(2, 2000, Easing.CubicIn);
await image.RotateTo(360, 2000, Easing.SinInOut);
await image.ScaleTo(1, 2000, Easing.CubicOut);
await image.TranslateTo(0, -200, 2000, Easing.BounceOut);
```

Задавая функцию плавности анимации, скорость анимации преобразуется в нелинейную и выдает эффект, предоставляемый функцией плавности. Пропуск функции плавности при создании анимации приводит к тому, что анимация использует [`Linear`](xref:Xamarin.Forms.Easing.Linear) функцию плавности по умолчанию, которая создает линейную скорость.

Дополнительные сведения об использовании методов расширения анимации в классе [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) см. в разделе [простые анимации](~/xamarin-forms/user-interface/animation/simple.md). Функции плавности можно также использовать с помощью класса [`Animation`](xref:Xamarin.Forms.Animation) . Дополнительные сведения см. в разделе [пользовательские анимации](~/xamarin-forms/user-interface/animation/custom.md).

<a name="customeasing" />

## <a name="custom-easing-functions"></a>Пользовательские функции плавности

Существует три основных подхода к созданию пользовательской функции плавности:

1. Создайте метод, принимающий аргумент `double` и возвращающий результат `double`.
1. Создайте таблицу `Func<double, double>`.
1. Укажите функцию плавности в качестве аргумента для конструктора [`Easing`](xref:Xamarin.Forms.Easing) .

Во всех трех случаях пользовательская функция плавности должна возвращать значение 0 для аргумента 0 и значение 1 для аргумента, равного 1. Однако любое значение может возвращаться между значениями аргументов 0 и 1. Теперь каждый подход будет рассмотрен в свою очередь.

### <a name="custom-easing-method"></a>Пользовательский метод плавности

Пользовательскую функцию плавности можно определить как метод, который принимает `double` аргумент и возвращает результат `double`, как показано в следующем примере кода:

```csharp
double CustomEase (double t)
{
  return t == 0 || t == 1 ? t : (int)(5 * t) / 5.0;
}

await image.TranslateTo(0, 200, 2000, (Easing)CustomEase);
```

Метод `CustomEase` усекает входящее значение до значений 0, 0,2, 0,4, 0,6, 0,8 и 1. Таким образом, экземпляр [`Image`](xref:Xamarin.Forms.Image) преобразуется в отдельные переходы, а не плавно.

### <a name="custom-easing-func"></a>Пользовательская замедление Func

Пользовательская функция плавности также может быть определена как `Func<double, double>`, как показано в следующем примере кода:

```csharp
Func<double, double> CustomEaseFunc = t => 9 * t * t * t - 13.5 * t * t + 5.5 * t;
await image.TranslateTo(0, 200, 2000, CustomEaseFunc);
```

`CustomEaseFunc` представляет функцию плавности, которая начинается быстро, замедляется и отменяет курс, а затем снова обращается к концу. Таким образом, в то время как общее перемещение [`Image`](xref:Xamarin.Forms.Image) экземпляра направлено вниз, оно также временно обращается к курсу на посередине.

### <a name="custom-easing-constructor"></a>Пользовательский конструктор плавности

Пользовательская функция плавности также может быть определена в качестве аргумента для конструктора [`Easing`](xref:Xamarin.Forms.Easing) , как показано в следующем примере кода:

```csharp
await image.TranslateTo (0, 200, 2000, new Easing (t => 1 - Math.Cos (10 * Math.PI * t) * Math.Exp (-5 * t)));
```

Пользовательская функция плавности указывается как аргумент лямбда-функции для конструктора [`Easing`](xref:Xamarin.Forms.Easing) и использует метод `Math.Cos` для создания медленных эффектов перетаскивания, которые допускают метод `Math.Exp`. Таким образом, экземпляр [`Image`](xref:Xamarin.Forms.Image) преобразуется так, чтобы он переводился в окончательное место размещения.

## <a name="summary"></a>Сводка

В этой статье показано, как использовать предварительно определенные функции плавности и как создавать пользовательские функции плавности. Xamarin. Forms включает класс [`Easing`](xref:Xamarin.Forms.Easing) , который позволяет указать функцию передачи, которая управляет скоростью анимации или замедляется при их выполнении.

## <a name="related-links"></a>Связанные ссылки

- [Общие сведения о поддержке асинхронного выполнения](~/cross-platform/platform/async.md)
- [Функции плавности (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-animation-easing)
- [Медлен](xref:Xamarin.Forms.Easing)
- [виевекстенсионс](xref:Xamarin.Forms.ViewExtensions)
