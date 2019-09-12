---
title: Шаблоны данных Xamarin.Forms
description: Шаблон данных используется для определения внешнего вида данных в поддерживаемых элементах управления и обычно привязывается к данным для отображения.
ms.prod: xamarin
ms.assetid: 838F4BDB-B719-457F-8633-27E9B267A2A0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/11/2017
ms.openlocfilehash: 5d130a6644af4e5831263c6de137513c021e0b6a
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2019
ms.locfileid: "70760805"
---
# <a name="xamarinforms-data-templates"></a>Шаблоны данных Xamarin.Forms

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/templates-datatemplates)

_Шаблон данных используется для определения внешнего вида данных в поддерживаемых элементах управления и обычно привязывается к данным для отображения._

## <a name="introductionintroductionmd"></a>[Введение](introduction.md)

Шаблоны данных Xamarin.Forms дают возможность настраивать представление данных в поддерживаемых элементах управления. В этой статье описываются шаблоны данных и объясняется их необходимость.

## <a name="creating-a-datatemplatecreatingmd"></a>[Создание шаблона данных](creating.md)

Шаблоны данных могут создаваться как встроенные, в [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary), или из пользовательского типа или соответствующего типа ячейки Xamarin.Forms. Встроенный шаблон следует использовать, если нет необходимости повторно использовать шаблон данных в другом месте. Шаблон данных можно использовать повторно, определив его как пользовательский тип или как ресурс на уровне элемента управления, страницы или приложения.

## <a name="creating-a-datatemplateselectorselectormd"></a>[Создание DataTemplateSelector](selector.md)

Объект [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) можно использовать для выбора [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) в среде выполнения на основе значения свойства, привязанного к данным. Это позволяет применять несколько экземпляров `DataTemplate` к одному типу объектов, чтобы настроить внешний вид определенных объектов. В этой статье демонстрируется, как создать и использовать `DataTemplateSelector`.

## <a name="related-links"></a>Связанные ссылки

- [Шаблоны данных (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/templates-datatemplates)
