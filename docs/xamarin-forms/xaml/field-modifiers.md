---
title: Модификаторы поля XAML в Xamarin.Forms
description: 'Атрибут x: FieldModifier пространства имен указывает уровень доступа для созданных полей для именованных элементов XAML.'
ms.prod: xamarin
ms.assetid: 12357CE0-3C11-4B62-947F-72DB6DFC23A2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/18/2018
ms.openlocfilehash: 8be56524ec1c5331f30418fcc29a4bd2c26ccde1
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "61075302"
---
# <a name="xaml-field-modifiers-in-xamarinforms"></a>Модификаторы поля XAML в Xamarin.Forms

_`x:FieldModifier` Атрибут пространства имен указывает уровень доступа для созданных полей для именованных элементов XAML._

## <a name="overview"></a>Обзор

Ниже приведены допустимые значения атрибута.

- `Public` — Указывает, что поле, создаваемое для элемента XAML `public`.
- `NotPublic` — Указывает, что поле, создаваемое для элемента XAML `internal` на сборку.

Если значение атрибута не задано, будет поле, создаваемое для элемента `private`.

Должны быть выполнены следующие условия для `x:FieldModifier` атрибут для обработки:

- Элемент верхнего уровня XAML должен быть допустимым `x:Class`.
- Текущий элемент XAML имеет `x:Name` указанного.

Следующий XAML показаны примеры установки атрибута:

```xaml
<Label x:Name="privateLabel" />
<Label x:Name="internalLabel" x:FieldModifier="NotPublic" />
<Label x:Name="publicLabel" x:FieldModifier="Public" />
```

> [!NOTE]
> `x:FieldModifier` Атрибут не может использоваться для указания уровня доступа класса XAML.
