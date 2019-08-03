---
title: Модификаторы полей XAML в Xamarin. Forms
description: Атрибут пространства имен КС:фиелдмодифиер определяет уровень доступа для созданных полей именованных элементов XAML.
ms.prod: xamarin
ms.assetid: 12357CE0-3C11-4B62-947F-72DB6DFC23A2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/02/2019
ms.openlocfilehash: 0f6050de943ca9878cf41b448d44bf222689be56
ms.sourcegitcommit: c6e56545eafd8ff9e540d56aba32aa6232c5315f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/02/2019
ms.locfileid: "68739450"
---
# <a name="xaml-field-modifiers-in-xamarinforms"></a>Модификаторы полей XAML в Xamarin. Forms

Атрибут `x:FieldModifier` Namespace задает уровень доступа для созданных полей именованных элементов XAML. Допустимые значения атрибута:

- `private`— Указывает, что созданное поле для элемента XAML доступно только в теле класса, в котором он объявлен.
- `public`— Указывает, что созданное поле для элемента XAML не имеет ограничений доступа.
- `protected`— Указывает, что созданное поле для элемента XAML доступно в своем классе и экземплярах производного класса.
- `internal`— Указывает, что созданное поле для элемента XAML доступно только в типах в той же сборке.
- `notpublic`— Указывает, что созданное поле для элемента XAML доступно только в типах в той же сборке.

По умолчанию, если значение атрибута не задано, то созданным полем для элемента будет `private`.

> [!NOTE]
> Значение атрибута может использовать любой регистр, так как он будет преобразован в нижний регистр с помощью Xamarin. Forms.

Для `x:FieldModifier` обработки атрибута должны выполняться следующие условия.

- Элемент XAML верхнего уровня должен быть допустимым `x:Class`.
- Текущий элемент XAML имеет указанный объект `x:Name` .

В следующем коде XAML показаны примеры установки атрибута:

```xaml
<Label x:Name="privateLabel" />
<Label x:Name="internalLabel" x:FieldModifier="internal" />
<Label x:Name="publicLabel" x:FieldModifier="public" />
```

> [!IMPORTANT]
> `x:FieldModifier` Атрибут нельзя использовать для указания уровня доступа класса XAML.
