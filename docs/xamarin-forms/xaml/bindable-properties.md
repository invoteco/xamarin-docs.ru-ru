---
title: Свойства, связываемые Xamarin. Forms
description: В этой статье содержатся вводные привязываемые свойства, а также показано, как создавать и использовать их.
ms.prod: xamarin
ms.assetid: 1EE869D8-6FE1-45CA-A0AD-26EC7D032AD7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/16/2020
ms.openlocfilehash: 56583aa0df676ae55e1b283f1a8e151b69a13d28
ms.sourcegitcommit: 10b4d7952d78f20f753372c53af6feb16918555c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/26/2020
ms.locfileid: "77635753"
---
# <a name="xamarinforms-bindable-properties"></a>Свойства, связываемые Xamarin. Forms

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/behaviors-eventtocommandbehavior)

Привязываемые свойства расширяют функциональные возможности свойства CLR путем резервного копирования свойства с типом [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) , а не для резервного копирования свойства с полем. Привязываемые свойства служит для предоставления системы свойств, который поддерживает привязку данных, стили, шаблоны и значения, заданные через родительско дочерних отношений. Кроме того привязываемые свойства можно указать значения по умолчанию, проверку значений свойств и обратных вызовов, отслеживающих изменения свойств.

Свойства должны быть реализованы как привязываемые свойства должно поддерживать одну или несколько из следующих компонентов:

- Действует как допустимое *целевое* свойство для привязки данных.
- Задание свойства с помощью [стиля](~/xamarin-forms/user-interface/styles/index.md).
- Предоставление значением свойства по умолчанию, который отличается от по умолчанию для типа свойства.
- Проверка значения свойства.
- Мониторинг изменений свойств.

Примеры связываемых свойств Xamarin. Forms включают [`Label.Text`](xref:Xamarin.Forms.Label.Text), [`Button.BorderRadius`](xref:Xamarin.Forms.Button.BorderRadius)и [`StackLayout.Orientation`](xref:Xamarin.Forms.StackLayout.Orientation). Каждое свойство, доступное для привязки, имеет соответствующее свойство `public static readonly` типа [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) , которое предоставляется в том же классе и является идентификатором привязываемого свойства. Например, соответствующий идентификатор привязываемого свойства для свойства `Label.Text` [`Label.TextProperty`](xref:Xamarin.Forms.Label.TextProperty).

## <a name="create-a-bindable-property"></a>Создание привязываемого свойства

Процесс создания может быть привязано выглядит следующим образом:

1. Создайте экземпляр [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) с одной из перегрузок метода [`BindableProperty.Create`](xref:Xamarin.Forms.BindableProperty.Create*) .
1. Определите методы доступа к свойствам для экземпляра [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) .

Все экземпляры [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) должны быть созданы в ПОТОКЕ пользовательского интерфейса. Это означает, что только код, который выполняется в потоке пользовательского интерфейса можно получить или задать значение может быть привязано. Однако доступ к `BindableProperty` экземплярам можно осуществлять из других потоков путем маршалирования в поток пользовательского интерфейса с помощью метода [`Device.BeginInvokeOnMainThread`](xref:Xamarin.Forms.Device.BeginInvokeOnMainThread(System.Action)) .

### <a name="create-a-property"></a>Создание свойства

Чтобы создать `BindableProperty` экземпляр, содержащий класс должен быть производным от класса [`BindableObject`](xref:Xamarin.Forms.BindableObject) . Однако класс `BindableObject` является высоким в иерархии классов, поэтому большинство классов, используемых для функциональности пользовательского интерфейса, поддерживают связываемые свойства.

Свойство, доступное для привязки, может быть создано путем объявления свойства `public static readonly` типа [`BindableProperty`](xref:Xamarin.Forms.BindableProperty). Свойству BIND должно быть присвоено возвращаемое значение одной из перегрузок [`BindableProperty.Create`](xref:Xamarin.Forms.BindableProperty.Create(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate)) метода. Объявление должно находиться в теле [`BindableObject`](xref:Xamarin.Forms.BindableObject) производного класса, но за пределами определений элементов.

При создании [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)необходимо указать как минимум идентификатор, а также следующие параметры:

- Имя [`BindableProperty`](xref:Xamarin.Forms.BindableProperty).
- Тип свойства.
- Тип объекта-владельца.
- Значение по умолчанию для свойства. Это гарантирует, что свойство всегда возвращает значение определенного по умолчанию, если оно не установлено, и он может отличаться от значения по умолчанию для типа свойства. Значение по умолчанию будет восстановлено при вызове метода [`ClearValue`](xref:Xamarin.Forms.BindableObject.ClearValue(Xamarin.Forms.BindableProperty)) для свойства, доступного для привязки.

Ниже приведен пример привязки свойства, с идентификатором и значения четыре обязательных параметров:

```csharp
public static readonly BindableProperty EventNameProperty =
  BindableProperty.Create ("EventName", typeof(string), typeof(EventToCommandBehavior), null);
```

При этом создается экземпляр [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) с именем `EventName`типа `string`. Свойство принадлежит классу `EventToCommandBehavior` и имеет значение по умолчанию `null`. Соглашение об именовании для свойств, допускающих привязку, заключается в том, что идентификатор связываемого свойства должен совпадать с именем свойства, указанным в методе `Create` с добавлением к нему "Property". Таким образом, в приведенном выше примере идентификатор привязываемого свойства — `EventNameProperty`.

При необходимости при создании [`BindableProperty`ного](xref:Xamarin.Forms.BindableProperty) экземпляра можно указать следующие параметры.

- Режим привязки. Используется для указания направления, в котором будут распространены изменения значений свойств. В режиме привязки по умолчанию изменения будут распространяться от *источника* к *целевому объекту*.
- Делегат проверки, который будет вызываться, когда свойство имеет значение. Дополнительные сведения см. в разделе [обратные вызовы проверки](#validation-callbacks).
- Делегат, который будет вызываться при изменении значения свойства изменено свойство. Дополнительные сведения см. в разделе [Обнаружение изменений свойств](#detect-property-changes).
- Свойство, изменение делегата, который будет вызываться, когда изменится значение свойства. Этот делегат имеет ту же сигнатуру, что делегат изменения свойства.
- Делегат значение принудительного, который будет вызываться при изменении значения свойства. Дополнительные сведения см. в разделе [приведение обратных вызовов к значению](#coerce-value-callbacks).
- `Func`, используемый для инициализации значения свойства по умолчанию. Дополнительные сведения см. в разделе [Создание значения по умолчанию с помощью Func](#create-a-default-value-with-a-func).

### <a name="create-accessors"></a>Создание методов доступа

К свойствам должны использовать синтаксис свойства для доступа к может быть привязано. Метод доступа `Get` должен возвращать значение, которое содержится в соответствующем связываемом свойстве. Это можно сделать, вызвав метод [`GetValue`](xref:Xamarin.Forms.BindableObject.GetValue(Xamarin.Forms.BindableProperty)) , передав идентификатор привязываемого свойства для получения значения, а затем приведя результат к требуемому типу. Метод доступа `Set` должен задавать значение соответствующего привязываемого свойства. Это можно сделать, вызвав метод [`SetValue`](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindableProperty,System.Object)) , передав идентификатор привязываемого свойства, для которого задается значение, и задаваемый значение.

В следующем примере кода показаны методы доступа для свойства с возможностью привязки `EventName`.

```csharp
public string EventName
{
  get { return (string)GetValue (EventNameProperty); }
  set { SetValue (EventNameProperty, value); }
}
```

## <a name="consume-a-bindable-property"></a>Использование привязываемого свойства

После создания может быть привязано, его можно будет использовать из XAML или кода. В XAML это достигается путем объявления пространства имен с префиксом, с объявлением пространства имен, указывающее имя пространства имен CLR и при необходимости, имя сборки. Дополнительные сведения см. в разделе [пространства имен XAML](~/xamarin-forms/xaml/namespaces.md).

В следующем примере кода показано пространство имен XAML для пользовательского типа, который содержит свойство привязки, который определен в сборке, в качестве код приложения, который ссылается на пользовательский тип:

```xaml
<ContentPage ... xmlns:local="clr-namespace:EventToCommandBehavior" ...>
  ...
</ContentPage>
```

Объявление пространства имен используется при задании свойства с возможностью привязки `EventName`, как показано в следующем примере кода XAML:

```xaml
<ListView ...>
  <ListView.Behaviors>
    <local:EventToCommandBehavior EventName="ItemSelected" ... />
  </ListView.Behaviors>
</ListView>
```

Эквивалентный код на языке C# показан в следующем примере:

```csharp
var listView = new ListView ();
listView.Behaviors.Add (new EventToCommandBehavior
{
  EventName = "ItemSelected",
  ...
});
```

## <a name="advanced-scenarios"></a>Сложные сценарии

При создании экземпляра [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) существует ряд необязательных параметров, которые можно задать для включения расширенных сценариев свойств с возможностью привязки. В данном разделе изучаются этих сценариев.

### <a name="detect-property-changes"></a>Обнаружение изменений свойств

Метод обратного вызова с измененным свойством `static` можно зарегистрировать с помощью свойства, доступного для привязки, указав параметр `propertyChanged` для метода [`BindableProperty.Create`](xref:Xamarin.Forms.BindableProperty.Create(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate)) . Указанный метод обратного вызова будет вызываться при изменении значения свойства привязки.

В следующем примере кода показано, как свойство с возможностью привязки `EventName` регистрирует метод `OnEventNameChanged` в виде метода обратного вызова, измененного свойством:

```csharp
public static readonly BindableProperty EventNameProperty =
  BindableProperty.Create (
    "EventName", typeof(string), typeof(EventToCommandBehavior), null, propertyChanged: OnEventNameChanged);
...

static void OnEventNameChanged (BindableObject bindable, object oldValue, object newValue)
{
  // Property changed implementation goes here
}
```

В методе обратного вызова, измененном свойством, параметр [`BindableObject`](xref:Xamarin.Forms.BindableObject) используется для обозначения того, какой экземпляр класса-владельца сообщил об изменении, а значения двух параметров `object` представляют старое и новое значения привязываемого свойства.

### <a name="validation-callbacks"></a>Обратные вызовы проверки

Метод обратного вызова проверки `static` можно зарегистрировать с помощью свойства, доступного для привязки, указав параметр `validateValue` для метода [`BindableProperty.Create`](xref:Xamarin.Forms.BindableProperty.Create(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate)) . Указанный метод обратного вызова будет вызываться, если задано значение, связываемые свойства.

В следующем примере кода показано, как свойство с возможностью привязки `Angle` регистрирует метод `IsValidValue` в качестве метода обратного вызова проверки.

```csharp
public static readonly BindableProperty AngleProperty =
  BindableProperty.Create ("Angle", typeof(double), typeof(HomePage), 0.0, validateValue: IsValidValue);
...

static bool IsValidValue (BindableObject view, object value)
{
  double result;
  bool isDouble = double.TryParse (value.ToString (), out result);
  return (result >= 0 && result <= 360);
}
```

Обратные вызовы проверки предоставляются со значением и должны возвращать `true`, если значение является допустимым для свойства, в противном случае `false`. Если обратный вызов проверки возвращает `false`, который должен обрабатываться разработчиком, возникнет исключение. Типичное использование метода обратного вызова проверки ограничения значений целых чисел, если задано свойство, используемое. Например, метод `IsValidValue` проверяет, является ли значение свойства `double` в диапазоне от 0 до 360.

### <a name="coerce-value-callbacks"></a>Приведение обратных вызовов значения

Метод обратного вызова `static` приведения значения можно зарегистрировать с помощью свойства, доступного для привязки, указав параметр `coerceValue` для метода [`BindableProperty.Create`](xref:Xamarin.Forms.BindableProperty.Create(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate)) . Указанный метод обратного вызова будет вызываться при изменении значения свойства привязки.

> [!IMPORTANT]
> Тип `BindableObject` содержит метод `CoerceValue`, который можно вызвать для принудительной повторной оценки значения его аргумента `BindableProperty`, путем вызова обратного вызова приводимого значения.

Приведение значения обратные вызовы используются для принудительного переоценки свойство, используемое при изменении значения свойства. Например чтобы убедиться, что значение одного свойства привязки не больше, чем значение другого привязываемые свойства можно использовать значение принудительного обратного вызова.

В следующем примере кода показано, как свойство с возможностью привязки `Angle` регистрирует метод `CoerceAngle` в качестве метода обратного вызова для приведения значения:

```csharp
public static readonly BindableProperty AngleProperty = BindableProperty.Create (
  "Angle", typeof(double), typeof(HomePage), 0.0, coerceValue: CoerceAngle);
public static readonly BindableProperty MaximumAngleProperty = BindableProperty.Create (
  "MaximumAngle", typeof(double), typeof(HomePage), 360.0, propertyChanged: ForceCoerceValue);
...

static object CoerceAngle (BindableObject bindable, object value)
{
  var homePage = bindable as HomePage;
  double input = (double)value;

  if (input > homePage.MaximumAngle)
  {
    input = homePage.MaximumAngle;
  }
  return input;
}

static void ForceCoerceValue(BindableObject bindable, object oldValue, object newValue)
{
  bindable.CoerceValue(AngleProperty);
}
```

Метод `CoerceAngle` проверяет значение свойства `MaximumAngle`, и если значение свойства `Angle` больше, то оно приводит к значению свойства `MaximumAngle`. Кроме того, когда свойство `MaximumAngle` изменяет обратный вызов приводимого значения, вызывается для свойства `Angle` путем вызова метода `CoerceValue`.

### <a name="create-a-default-value-with-a-func"></a>Создание значения по умолчанию с помощью Func

`Func` можно использовать для инициализации значения свойства, доступного для привязки, по умолчанию, как показано в следующем примере кода:

```csharp
public static readonly BindableProperty SizeProperty =
  BindableProperty.Create ("Size", typeof(double), typeof(HomePage), 0.0,
  defaultValueCreator: bindable => Device.GetNamedSize (NamedSize.Large, (Label)bindable));
```

Параметр `defaultValueCreator` имеет значение `Func`, которое вызывает метод [`Device.GetNamedSize`](xref:Xamarin.Forms.Device.GetNamedSize(Xamarin.Forms.NamedSize,System.Type)) для возврата `double`, представляющего именованный размер шрифта, используемого в [`Label`](xref:Xamarin.Forms.Label) на собственной платформе.

## <a name="related-links"></a>Связанные ссылки

- [Пространства имен языка XAML](~/xamarin-forms/xaml/namespaces.md)
- [Поведение события для команды (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/behaviors-eventtocommandbehavior)
- [Обратный вызов проверки (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-validationcallback)
- [Обратный вызов значения приведения (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-coercevaluecallback)
- [API Биндаблепроперти](xref:Xamarin.Forms.BindableProperty)
- [API BindableObject](xref:Xamarin.Forms.BindableObject)
