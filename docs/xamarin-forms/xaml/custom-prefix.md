---
title: Пространство имен XAML, рекомендуется использовать префиксы в Xamarin.Forms
description: Класс XmlnsPrefixAttribute используется авторами элемента управления, для указания рекомендуемый префикс, связываемый с пространством имен XAML, для использования XAML.
ms.prod: xamarin
ms.assetid: 7B315BEC-7A35-48F4-A9C7-EF40255E95FF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/28/2019
ms.openlocfilehash: 33f18b3f9c9ddb6ab31ca92e2f192ffad783ec0c
ms.sourcegitcommit: 00744f754527e5b55154365f89691caaf1c9d929
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/07/2019
ms.locfileid: "57557476"
---
# <a name="xaml-namespace-recommended-prefixes-in-xamarinforms"></a>Пространство имен XAML, рекомендуется использовать префиксы в Xamarin.Forms

`XmlnsPrefixAttribute` Класс может использоваться авторами элемента управления, чтобы указать рекомендуемый префикс, связываемый с пространством имен XAML, для использования XAML. Префикс полезен при поддержке сериализация дерева объектов для XAML, или при взаимодействии со средой разработки с возможности редактирования XAML. Пример:

- Использовать XAML текстовые редакторы `XmlnsPrefixAttribute` как указание для начального пространства имен XAML `xmlns` сопоставления.
- Среда разработки XAML может использовать `XmlnsPrefixAttribute` добавить сопоставления в XAML, при перетаскивании объектов из панели инструментов, а также на визуальную область конструктора.

Рекомендуется использовать префиксы пространства имен должны быть определены на уровне сборки с `XmlnsPrefixAttribute` конструктор, который принимает два аргумента: строка, задающая идентификатор пространства имен XAML и строка, указывающая рекомендуемый префикс:

```csharp
[assembly: XmlnsPrefix("http://xamarin.com/schemas/2014/forms", "xf")]
```

Префиксы следует использовать короткие строки, так как префикс обычно применяется ко всем элементам сериализованный, полученные из пространства имен XAML. Таким образом длина строки префикса может иметь заметно влияет на размер сериализованных выходных данных XAML.

> [!NOTE]
> Более одного `XmlnsPrefixAttribute` могут применяться к сборке. Например если у вас есть сборки, которая определяет типы для нескольких пространств имен XAML, можно определить различные значения префиксов для каждого пространства имен XAML.

## <a name="related-links"></a>Связанные ссылки

- [Схемы настраиваемых пространств имен XAML](custom-namespace-schemas.md)
- [Пространства имен XAML в Xamarin.Forms](namespaces.md)