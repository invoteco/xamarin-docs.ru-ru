---
title: Передача аргументов в XAML
description: В этой статье демонстрируется использование атрибутов XAML, которые могут использоваться для передачи аргументов конструктора не по умолчанию, для вызова методов фабрики, а также указать тип универсального аргумента.
ms.prod: xamarin
ms.assetid: 8F3B267F-499E-4D79-9193-FCA99F199519
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/25/2016
ms.openlocfilehash: 80f332e45d6c46ad49543923e85cbb2eceadb378
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/13/2020
ms.locfileid: "79306563"
---
# <a name="passing-arguments-in-xaml"></a>Передача аргументов в XAML

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-passingconstructorarguments)

_В этой статье показано использование атрибутов XAML, которые можно использовать для передачи аргументов в конструкторы, отличные от по умолчанию, для вызова методов фабрики и для указания типа универсального аргумента._

## <a name="overview"></a>Обзор

Часто бывает необходимо создавать экземпляры объектов с помощью конструкторов, которые требуют аргументов или путем вызова метода статического создания. Это можно сделать в XAML с помощью атрибутов `x:Arguments` и `x:FactoryMethod`:

- Атрибут `x:Arguments` используется для указания аргументов конструктора для конструктора, не являющегося конструктором по умолчанию, или для объявления объекта фабричного метода. Дополнительные сведения см. в разделе [Передача аргументов конструктора](#constructor_arguments).
- Атрибут `x:FactoryMethod` используется для указания фабричного метода, который можно использовать для инициализации объекта. Дополнительные сведения см. в разделе [вызов фабричных методов](#factory_methods).

Кроме того, можно использовать атрибут `x:TypeArguments`, чтобы указать аргументы универсального типа для конструктора универсального типа. Дополнительные сведения см. [в разделе Указание аргумента универсального типа](#generic_type_arguments).

<a name="constructor_arguments" />

## <a name="passing-constructor-arguments"></a>Передача аргументов конструктора

Аргументы можно передать в конструктор, не используемый по умолчанию, с помощью атрибута `x:Arguments`. Каждый аргумент конструктора должны быть заключены в XML-элемент, представляющий тип аргумента. Xamarin.Forms поддерживает следующие элементы для основных типов:

- `x:Object`
- `x:Boolean`
- `x:Byte`
- `x:Int16`
- `x:Int32`
- `x:Int64`
- `x:Single`
- `x:Double`
- `x:Decimal`
- `x:Char`
- `x:String`
- `x:TimeSpan`
- `x:Array`
- `x:DateTime`

В следующем примере кода показано использование атрибута `x:Arguments` с тремя конструкторами [`Color`](xref:Xamarin.Forms.Color) :

```xaml
<BoxView HeightRequest="150" WidthRequest="150" HorizontalOptions="Center">
  <BoxView.Color>
    <Color>
      <x:Arguments>
        <x:Double>0.9</x:Double>
      </x:Arguments>
    </Color>
  </BoxView.Color>
</BoxView>
<BoxView HeightRequest="150" WidthRequest="150" HorizontalOptions="Center">
  <BoxView.Color>
    <Color>
      <x:Arguments>
        <x:Double>0.25</x:Double>
        <x:Double>0.5</x:Double>
        <x:Double>0.75</x:Double>
      </x:Arguments>
    </Color>
  </BoxView.Color>
</BoxView>
<BoxView HeightRequest="150" WidthRequest="150" HorizontalOptions="Center">
  <BoxView.Color>
    <Color>
      <x:Arguments>
        <x:Double>0.8</x:Double>
        <x:Double>0.5</x:Double>
        <x:Double>0.2</x:Double>
        <x:Double>0.5</x:Double>
      </x:Arguments>
    </Color>
  </BoxView.Color>
</BoxView>
```

Число элементов в теге `x:Arguments` и типы этих элементов должны соответствовать одному из конструкторов [`Color`](xref:Xamarin.Forms.Color) . Для [конструктора](xref:Xamarin.Forms.Color.%23ctor(System.Double)) `Color` с одним параметром требуется значение оттенка серого от 0 (черный) до 1 (белый). Для [конструктора](xref:Xamarin.Forms.Color.%23ctor(System.Double,System.Double,System.Double)) `Color` с тремя параметрами требуется красный, зеленый и синий значения в диапазоне от 0 до 1. [Конструктор](xref:Xamarin.Forms.Color.%23ctor(System.Double,System.Double,System.Double,System.Double)) `Color` с четырьмя параметрами добавляет альфа-канал в качестве четвертого параметра.

На следующих снимках экрана показан результат вызова каждого конструктора [`Color`](xref:Xamarin.Forms.Color) с указанными значениями аргумента:

![Боксвиев. Color, заданный с помощью x:Arguments](passing-arguments-images/passing-arguments.png)

<a name="factory_methods" />

## <a name="calling-factory-methods"></a>Обращение к методам фабрики

Фабричные методы могут быть вызваны в XAML путем указания имени метода с помощью атрибута `x:FactoryMethod` и его аргументов, использующих атрибут `x:Arguments`. Фабричный метод — это `public static` метод, который возвращает объекты или значения того же типа, что и класс или структура, определяющие методы.

Структура [`Color`](xref:Xamarin.Forms.Color) определяет ряд фабричных методов, и в следующем примере кода демонстрируется вызов трех из них:

```xaml
<BoxView HeightRequest="150" WidthRequest="150" HorizontalOptions="Center">
  <BoxView.Color>
    <Color x:FactoryMethod="FromRgba">
      <x:Arguments>
        <x:Int32>192</x:Int32>
        <x:Int32>75</x:Int32>
        <x:Int32>150</x:Int32>                        
        <x:Int32>128</x:Int32>
      </x:Arguments>
    </Color>
  </BoxView.Color>
</BoxView>
<BoxView HeightRequest="150" WidthRequest="150" HorizontalOptions="Center">
  <BoxView.Color>
    <Color x:FactoryMethod="FromHsla">
      <x:Arguments>
        <x:Double>0.23</x:Double>
        <x:Double>0.42</x:Double>
        <x:Double>0.69</x:Double>
        <x:Double>0.7</x:Double>
      </x:Arguments>
    </Color>
  </BoxView.Color>
</BoxView>
<BoxView HeightRequest="150" WidthRequest="150" HorizontalOptions="Center">
  <BoxView.Color>
    <Color x:FactoryMethod="FromHex">
      <x:Arguments>
        <x:String>#FF048B9A</x:String>
      </x:Arguments>
    </Color>
  </BoxView.Color>
</BoxView>
```

Число элементов в теге `x:Arguments` и типы этих элементов должны соответствовать аргументам вызываемого метода фабрики. Для метода фабрики [`FromRgba`](xref:Xamarin.Forms.Color.FromRgba(System.Int32,System.Int32,System.Int32,System.Int32)) требуется четыре параметра [`Int32`](https://docs.microsoft.com/dotnet/api/system.int32) , представляющие красные, зеленые, синие и альфа-значения в диапазоне от 0 до 255 соответственно. Для метода фабрики [`FromHsla`](xref:Xamarin.Forms.Color.FromHsla(System.Double,System.Double,System.Double,System.Double)) требуется четыре параметра [`Double`](https://docs.microsoft.com/dotnet/api/system.double) , представляющие оттенок, насыщенность, яркость и альфа-значения в диапазоне от 0 до 1 соответственно. Для метода фабрики [`FromHex`](xref:Xamarin.Forms.Color.FromHex(System.String)) требуется [`String`](https://docs.microsoft.com/dotnet/api/system.string) , представляющий шестнадцатеричный цвет (a) RGB.

На следующих снимках экрана показан результат вызова каждого метода фабрики [`Color`](xref:Xamarin.Forms.Color) с указанными значениями аргументов:

![Боксвиев. Color, заданный с помощью x:FactoryMethod и x:Arguments](passing-arguments-images/factory-methods.png)

<a name="generic_type_arguments" />

## <a name="specifying-a-generic-type-argument"></a>Указание аргумента универсального типа

Аргументы универсального типа для конструктора универсального типа можно указать с помощью атрибута `x:TypeArguments`, как показано в следующем примере кода:

```xaml
<ContentPage ...>
  <StackLayout>
    <StackLayout.Margin>
      <OnPlatform x:TypeArguments="Thickness">
        <On Platform="iOS" Value="0,20,0,0" />
        <On Platform="Android" Value="5, 10" />
        <On Platform="UWP" Value="10" />
      </OnPlatform>
    </StackLayout.Margin>
  </StackLayout>
</ContentPage>
```

Класс [`OnPlatform`](xref:Xamarin.Forms.OnPlatform`1) является универсальным классом и должен быть создан с помощью атрибута `x:TypeArguments`, соответствующего целевому типу. В классе [`On`](xref:Xamarin.Forms.On) атрибут [`Platform`](xref:Xamarin.Forms.On.Platform) может принимать одно значение `string` или несколько `string` значений с разделителями-запятыми. В этом примере свойству [`StackLayout.Margin`](xref:Xamarin.Forms.View.Margin) присвоено [`Thickness`](xref:Xamarin.Forms.Thickness), зависящее от платформы.

## <a name="summary"></a>Сводка

В этой статье показано, с помощью атрибутов XAML, которые могут использоваться для передачи аргументов конструктора не по умолчанию, для вызова методов фабрики, а также указать тип универсального аргумента.

## <a name="related-links"></a>Связанные ссылки

- [Пространства имен языка XAML](~/xamarin-forms/xaml/namespaces.md)
- [Передача аргументов конструктора (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-passingconstructorarguments)
- [Методы вызова фабрики (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-callingfactorymethods)
