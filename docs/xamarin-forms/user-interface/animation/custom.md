---
title: Пользовательской анимации в Xamarin.Forms
description: В этой статье показано, как использовать класс анимации Xamarin.FOrms для создания и Отмена анимации, синхронизации нескольких анимаций и создание анимации, анимации свойств, не анимировано в существующие методы анимации.
ms.prod: xamarin
ms.assetid: 03B2E3FC-E720-4D45-B9A0-711081FC1907
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/10/2019
ms.openlocfilehash: 405d7990b622b890aa3d66bd632662f086441666
ms.sourcegitcommit: 10b4d7952d78f20f753372c53af6feb16918555c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/26/2020
ms.locfileid: "77635761"
---
# <a name="custom-animations-in-xamarinforms"></a>Пользовательской анимации в Xamarin.Forms

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-animation-custom)

_Класс анимации — это стандартный блок всех анимаций Xamarin. Forms с методами расширения в классе Виевекстенсионс, создающими один или несколько объектов анимации. В этой статье показано, как использовать класс анимации для создания и отмены анимации, синхронизации нескольких анимаций и создания пользовательских анимаций, которые анимировать свойства, которые не анимированы с помощью существующих методов анимации._

При создании объекта `Animation` необходимо указать ряд параметров, включая начальное и конечное значения анимируемого свойства, а также обратный вызов, который изменяет значение свойства. Объект `Animation` может также поддерживать коллекцию дочерних анимаций, которые могут быть запущены и синхронизированы. Дополнительные сведения см. в разделе [дочерние анимации](#child).

Выполнение анимации, созданной с помощью класса [`Animation`](xref:Xamarin.Forms.Animation) , который может или не может включать дочерние анимации, достигается путем вызова метода [`Commit`](xref:Xamarin.Forms.Animation.Commit(Xamarin.Forms.IAnimatable,System.String,System.UInt32,System.UInt32,Xamarin.Forms.Easing,System.Action{System.Double,System.Boolean},System.Func{System.Boolean})) . Этот метод задает длительность анимации и среди других элементов, обратный вызов, который определяет, следует ли повторять анимацию.

Кроме того, класс [`Animation`](xref:Xamarin.Forms.Animation) имеет свойство `IsEnabled`, которое можно проверить, чтобы определить, была ли анимация отключена операционной системой, например при активации режима энергосбережения.

## <a name="create-an-animation"></a>Создание анимации

При создании объекта [`Animation`](xref:Xamarin.Forms.Animation) , как правило, требуется не менее трех параметров, как показано в следующем примере кода:

```csharp
var animation = new Animation (v => image.Scale = v, 1, 2);
```

Этот код определяет анимацию свойства [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) экземпляра [`Image`](xref:Xamarin.Forms.Image) от значения 1 до значения 2. Анимированное значение, производное от Xamarin. Forms, передается обратному вызову, указанному в качестве первого аргумента, где он используется для изменения значения свойства `Scale`.

Анимация начинается с вызова метода [`Commit`](xref:Xamarin.Forms.Animation.Commit(Xamarin.Forms.IAnimatable,System.String,System.UInt32,System.UInt32,Xamarin.Forms.Easing,System.Action{System.Double,System.Boolean},System.Func{System.Boolean})) , как показано в следующем примере кода:

```csharp
animation.Commit (this, "SimpleAnimation", 16, 2000, Easing.Linear, (v, c) => image.Scale = 1, () => true);
```

Обратите внимание, что метод [`Commit`](xref:Xamarin.Forms.Animation.Commit(Xamarin.Forms.IAnimatable,System.String,System.UInt32,System.UInt32,Xamarin.Forms.Easing,System.Action{System.Double,System.Boolean},System.Func{System.Boolean})) не возвращает объект `Task`. Вместо этого уведомления предоставляются через методы обратного вызова.

В методе `Commit` указаны следующие аргументы:

- Первый аргумент (*owner*) определяет владельца анимации. Это может быть визуальный элемент, к которому применяется анимация, или другой визуальный элемент, например с помощью страницы.
- Второй аргумент (*Name*) определяет анимацию с именем. Имя объединяется с владельцем для уникальной идентификации анимации. С помощью этой уникальной идентификации можно определить, выполняется ли анимация ([`AnimationIsRunning`](xref:Xamarin.Forms.AnimationExtensions.AnimationIsRunning(Xamarin.Forms.IAnimatable,System.String))), или отменить ее ([`AbortAnimation`](xref:Xamarin.Forms.AnimationExtensions.AbortAnimation(Xamarin.Forms.IAnimatable,System.String))).
- Третий аргумент (*Rate*) указывает количество миллисекунд между вызовами метода обратного вызова, определенного в конструкторе [`Animation`](xref:Xamarin.Forms.Animation)
- Четвертый аргумент (*Длина*) указывает длительность анимации в миллисекундах.
- Пятый аргумент (*замедление*) определяет функцию плавности, используемую в анимации. Кроме того, функция плавности может быть указана в качестве аргумента для конструктора [`Animation`](xref:Xamarin.Forms.Animation) . Дополнительные сведения о функциях плавности см. в разделе [функции плавности](~/xamarin-forms/user-interface/animation/easing.md).
- Шестой аргумент (*Finished*) — это обратный вызов, который будет выполнен после завершения анимации. Этот обратный вызов принимает два аргумента с первым аргументом, указывающим конечное значение, а второй аргумент — `bool`, для которого задано `true`, если анимация была отменена. Кроме того, *завершенный* обратный вызов можно указать в качестве аргумента для конструктора [`Animation`](xref:Xamarin.Forms.Animation) . Однако при использовании одной анимации, если обратные вызовы задаются как в конструкторе `Animation`, так и в методе `Commit`, будет выполнен только *обратный вызов,* указанный в методе `Commit`.
- Седьмой аргумент (*Repeat*) — это обратный вызов, позволяющий повторять анимацию. Он вызывается в конце анимации и возвращает `true` указывает, что анимация должна повторяться.

Общий эффект заключается в создании анимации, которая увеличивает свойство [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) [`Image`](xref:Xamarin.Forms.Image) от 1 до 2, более 2 секунд (2000 миллисекунд) с помощью функции плавности [`Linear`](xref:Xamarin.Forms.Easing.Linear) . Каждый раз, когда анимация завершается, ее свойство `Scale` сбрасывается в 1, а анимация повторяется.

> [!NOTE]
> Параллельная анимация, которая выполняется независимо друг от друга, может быть создана путем создания объекта `Animation` для каждой анимации, а затем вызова метода `Commit` для каждой анимации.

<a name="child" />

### <a name="child-animations"></a>Дочерних анимациях

Класс [`Animation`](xref:Xamarin.Forms.Animation) также поддерживает дочерние анимации, включающие создание объекта `Animation`, к которому добавляются другие `Animation`ные объекты. Это позволяет ряд анимации должен быть запущен и синхронизированы. В следующем примере кода показано создание и запуск дочерних анимациях:

```csharp
var parentAnimation = new Animation ();
var scaleUpAnimation = new Animation (v => image.Scale = v, 1, 2, Easing.SpringIn);
var rotateAnimation = new Animation (v => image.Rotation = v, 0, 360);
var scaleDownAnimation = new Animation (v => image.Scale = v, 2, 1, Easing.SpringOut);

parentAnimation.Add (0, 0.5, scaleUpAnimation);
parentAnimation.Add (0, 1, rotateAnimation);
parentAnimation.Add (0.5, 1, scaleDownAnimation);

parentAnimation.Commit (this, "ChildAnimations", 16, 4000, null, (v, c) => SetIsEnabledButtonState (true, false));
```

Кроме того в примере кода можно написать более кратко, как показано в следующем примере кода:

```csharp
new Animation {
    { 0, 0.5, new Animation (v => image.Scale = v, 1, 2) },
    { 0, 1, new Animation (v => image.Rotation = v, 0, 360) },
    { 0.5, 1, new Animation (v => image.Scale = v, 2, 1) }
    }.Commit (this, "ChildAnimations", 16, 4000, null, (v, c) => SetIsEnabledButtonState (true, false));
```

В обоих примерах кода создается родительский объект [`Animation`](xref:Xamarin.Forms.Animation) , в который затем добавляются дополнительные `Animation` объекты. Первые два аргумента метода [`Add`](xref:Xamarin.Forms.Animation.Add(System.Double,System.Double,Xamarin.Forms.Animation)) указывают, когда следует начинать и завершать дочернюю анимацию. Значения аргументов должен находиться в диапазоне от 0 до 1 и представляют относительный период в пределах продолжительности анимации родительского указанного дочернего анимация будет активна. Таким образом, в этом примере `scaleUpAnimation` будет активна для первой половины анимации, `scaleDownAnimation` будет активна во второй половине анимации, и `rotateAnimation` будет активна в течение всего времени.

Общий эффект является то, что анимация возникает более чем 4 секунды (4000 миллисекунд). `scaleUpAnimation` анимируется свойство [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) от 1 до 2, более 2 секунд. `scaleDownAnimation` затем анимируется свойство `Scale` от 2 до 1, более 2 секунд. Хотя обе анимации происходят, `rotateAnimation` анимируется свойство [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) от 0 до 360, более 4 секунд. Обратите внимание на то, что анимаций также использовать функции плавности. Функция [`SpringIn`](xref:Xamarin.Forms.Easing.SpringIn) плавности приводит к тому, что [`Image`](xref:Xamarin.Forms.Image) изначально сжимается перед увеличением, а функция [`SpringOut`](xref:Xamarin.Forms.Easing.SpringOut) плавности приводит к тому, что `Image` становится меньше, чем его фактический размер в конце всей анимации.

Существует ряд различий между объектом [`Animation`](xref:Xamarin.Forms.Animation) , использующим дочерние анимации, и одним из которых не является:

- При использовании дочерних анимаций функция *завершения* обратного вызова в дочерней анимации показывает, что дочерний элемент завершен, а обратный вызов, переданный методу `Commit`, *указывает, что* вся анимация завершена.
- При использовании дочерних анимаций возврат `true` из обратного вызова *Repeat* в методе `Commit` не приведет к повторению анимации, но анимация продолжит выполняться без новых значений.
- Если включить функцию плавности в метод `Commit`, а функция плавности возвращает значение больше 1, анимация будет прервана. Если функция плавности возвращает значение меньше 0, значение присваивается значение 0. Чтобы использовать функцию плавности, которая возвращает значение меньше 0 или больше 1, оно должно быть указано в одной из дочерних анимаций, а не в методе `Commit`.

Класс [`Animation`](xref:Xamarin.Forms.Animation) также включает методы [`WithConcurrent`](xref:Xamarin.Forms.Animation.WithConcurrent(Xamarin.Forms.Animation,System.Double,System.Double)) , которые можно использовать для добавления дочерних анимаций в родительский объект `Animation`. Однако значения аргументов *Begin* и *Finish* не ограничиваются 0 и 1, но только эта часть дочерней анимации, соответствующая диапазону от 0 до 1, будет активной. Например, если вызов метода `WithConcurrent` определяет дочернюю анимацию, предназначенную для свойства [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) от 1 до 6, но со значениями *Begin* и *Finish* , равными-2 и 3, *начальное* значение-2 соответствует значению `Scale`, равному 1, а значение *окончания* 3 соответствует значению `Scale`, равному 6. Поскольку значения за пределами диапазона 0 и 1 не играют никакой части анимации, свойство `Scale` будет анимироваться только от 3 до 6.

## <a name="cancel-an-animation"></a>Отмена анимации

Приложение может отменить анимацию с помощью вызова метода расширения [`AbortAnimation`](xref:Xamarin.Forms.AnimationExtensions.AbortAnimation(Xamarin.Forms.IAnimatable,System.String)) , как показано в следующем примере кода:

```csharp
this.AbortAnimation ("SimpleAnimation");
```

Обратите внимание на то, что анимации однозначно идентифицируются сочетанием анимации владельца и имя анимации. Таким образом владельца и имя указано, когда выполнение анимации должен быть указан для отмены анимации. Поэтому в примере кода немедленно отменяется анимация с именем `SimpleAnimation`, которой владеет страница.

## <a name="create-a-custom-animation"></a>Создание пользовательской анимации

Приведенные здесь примеры демонстрируют анимацию, которая может быть так же достигнута с помощью методов класса [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) . Однако преимущество класса [`Animation`](xref:Xamarin.Forms.Animation) заключается в том, что он имеет доступ к методу обратного вызова, который выполняется при изменении анимированного значения. Это позволяет обратному вызову реализации любой требуемой анимации. Например, в следующем примере кода анимируется свойство [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) страницы, присвоив ему значение [`Color`](xref:Xamarin.Forms.Color) значения, созданные методом [`Color.FromHsla`](xref:Xamarin.Forms.Color.FromHsla(System.Double,System.Double,System.Double,System.Double)) , со значениями оттенка от 0 до 1:

```csharp
new Animation (callback: v => BackgroundColor = Color.FromHsla (v, 1, 0.5),
  start: 0,
  end: 1).Commit (this, "Animation", 16, 4000, Easing.Linear, (v, c) => BackgroundColor = Color.Default);
```

Результирующая анимация предоставляет внешний вид смещения фона страницы через цвета радуги.

Дополнительные примеры создания сложных анимаций, включая анимацию кривой Безье, см. в [главе 22 статьи](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch22-Apr2016.pdf) [Создание мобильных приложений с помощью Xamarin. Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md).

## <a name="create-a-custom-animation-extension-method"></a>Создание пользовательского метода расширения анимации

Методы расширения в классе [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) анимировать свойство от текущего значения до указанного значения. Это усложняет создание, например, `ColorTo` метода анимации, который можно использовать для анимации цвета из одного значения в другое, поскольку:

- Единственным свойством [`Color`](xref:Xamarin.Forms.Color) , определенным классом [`VisualElement`](xref:Xamarin.Forms.VisualElement) , является [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor), которое не всегда является необходимым для анимации свойством `Color`.
- Часто текущее значение свойства [`Color`](xref:Xamarin.Forms.Color) [`Color.Default`](xref:Xamarin.Forms.Color.Default), которое не является реальным цветом и не может использоваться в вычислениях интерполяции.

Решением этой проблемы является отсутствие метода `ColorTo`, предназначенного для конкретного свойства [`Color`](xref:Xamarin.Forms.Color) . Вместо этого он может быть написан с помощью метода обратного вызова, который передает интерполяцию `Color` значение обратно вызывающему объекту. Кроме того, метод будет принимать начальные и конечные `Color` аргументов.

Метод `ColorTo` можно реализовать как метод расширения, использующий метод [`Animate`](xref:Xamarin.Forms.AnimationExtensions.Animate*) в классе [`AnimationExtensions`](xref:Xamarin.Forms.AnimationExtensions) для предоставления его функциональных возможностей. Это обусловлено тем, что метод `Animate` можно использовать для назначения свойств, которые не имеют тип `double`, как показано в следующем примере кода:

```csharp
public static class ViewExtensions
{
  public static Task<bool> ColorTo(this VisualElement self, Color fromColor, Color toColor, Action<Color> callback, uint length = 250, Easing easing = null)
  {
    Func<double, Color> transform = (t) =>
      Color.FromRgba(fromColor.R + t * (toColor.R - fromColor.R),
                     fromColor.G + t * (toColor.G - fromColor.G),
                     fromColor.B + t * (toColor.B - fromColor.B),
                     fromColor.A + t * (toColor.A - fromColor.A));
    return ColorAnimation(self, "ColorTo", transform, callback, length, easing);
  }

  public static void CancelAnimation(this VisualElement self)
  {
    self.AbortAnimation("ColorTo");
  }

  static Task<bool> ColorAnimation(VisualElement element, string name, Func<double, Color> transform, Action<Color> callback, uint length, Easing easing)
  {
    easing = easing ?? Easing.Linear;
    var taskCompletionSource = new TaskCompletionSource<bool>();

    element.Animate<Color>(name, transform, callback, 16, length, easing, (v, c) => taskCompletionSource.SetResult(c));
    return taskCompletionSource.Task;
  }
}
```

Для метода [`Animate`](xref:Xamarin.Forms.AnimationExtensions.Animate*) требуется аргумент *Transform* , который является методом обратного вызова. Входные данные для этого обратного вызова всегда являются `double` в диапазоне от 0 до 1. Таким образом, метод `ColorTo` определяет собственный `Func` преобразования, который принимает `double` в диапазоне от 0 до 1 и возвращает значение [`Color`](xref:Xamarin.Forms.Color) , соответствующее этому значению. Значение `Color` вычисляется путем интерполяции [`R`](xref:Xamarin.Forms.Color.R), [`G`](xref:Xamarin.Forms.Color.G), [`B`](xref:Xamarin.Forms.Color.B)и [`A`](xref:Xamarin.Forms.Color.A) значений двух предоставленных `Color` аргументов. Затем `Color` значение передается в метод обратного вызова для приложения в определенное свойство.

Такой подход позволяет методу `ColorTo` анимировать любое [`Color`](xref:Xamarin.Forms.Color) свойство, как показано в следующем примере кода:

```csharp
await Task.WhenAll(
  label.ColorTo(Color.Red, Color.Blue, c => label.TextColor = c, 5000),
  label.ColorTo(Color.Blue, Color.Red, c => label.BackgroundColor = c, 5000));
await this.ColorTo(Color.FromRgb(0, 0, 0), Color.FromRgb(255, 255, 255), c => BackgroundColor = c, 5000);
await boxView.ColorTo(Color.Blue, Color.Red, c => boxView.Color = c, 4000);
```

В этом примере кода метод `ColorTo` анимирует свойства [`TextColor`](xref:Xamarin.Forms.Label.TextColor) и [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) [`Label`](xref:Xamarin.Forms.Label), свойство `BackgroundColor` страницы и свойство [`Color`](xref:Xamarin.Forms.BoxView.Color) объекта [`BoxView`](xref:Xamarin.Forms.BoxView).

## <a name="related-links"></a>Связанные ссылки

- [Пользовательские анимации (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-animation-custom)
- [API анимации](xref:Xamarin.Forms.Animation)
- [API AnimationExtensions](xref:Xamarin.Forms.AnimationExtensions)
