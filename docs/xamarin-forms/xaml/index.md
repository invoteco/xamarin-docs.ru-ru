---
title: Расширяемый язык разметки приложений (XAML)
description: XAML — это декларативная разметка язык, который может использоваться для определения пользовательских интерфейсов. Пользовательский интерфейс определяется в XML-файл с помощью синтаксиса XAML, хотя поведение во время выполнения определяется в отдельном вспомогательном файле.
ms.prod: xamarin
ms.assetid: CD30EECC-8AC1-4CF5-A4FE-348420A6231E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/03/2019
ms.openlocfilehash: 1f6bdfb1aed87f8a2dabdc0efcea3a1b8bf2ecf2
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "61075272"
---
# <a name="extensible-application-markup-language-xaml"></a>Расширяемый язык разметки приложений (XAML)

_XAML — это декларативная разметка язык, который может использоваться для определения пользовательских интерфейсов. Пользовательский интерфейс определяется в XML-файл с помощью синтаксиса XAML, хотя поведение во время выполнения определяется в отдельном вспомогательном файле._

> [!VIDEO https://youtube.com/embed/H6UOrSyhTEE]

**Усовершенствовано в 2016 г. Как стать шаблона XAML**

> [!NOTE]
> Испытайте [предварительной версии стандартных XAML](standard/index.md)

## <a name="xaml-controlsxaml-controlsmd"></a>[Элементы управления XAML](xaml-controls.md)

Все представления, которые определены в Xamarin.Forms можно ссылаться из файлов XAML.

<a name="xaml" />

## <a name="xaml-basicsxaml-basicsindexmd"></a>[Основы XAML](xaml-basics/index.md)

XAML позволяет разработчикам определить пользовательские интерфейсы приложений Xamarin.Forms с помощью разметки вместо кода. В приложении Xamarin.Forms XAML никогда не требуется, но он оснащен инструментами и часто визуально согласованными и короче, чем эквивалентный код. XAML особенно хорошо подходит для использования с популярными архитектуры Model-View-ViewModel (MVVM) приложения: XAML определяет представление, связанного с ViewModel кода через привязки данных на основе XAML.

## <a name="xaml-compilationxamlcmd"></a>[Компиляция XAML](xamlc.md)

При необходимости можно воспользоваться компилятором XAML (XAMLC) и скомпилировать XAML напрямую в промежуточный язык (IL). В этой статье описывается использование XAMLC и ее преимущества.

## <a name="xaml-previewerxaml-previewerindexmd"></a>[Средство предварительного просмотра XAML](xaml-previewer/index.md)

[Средства предварительного просмотра XAML](~/xamarin-forms/xaml/xaml-previewer/index.md) отрисовывает динамический просмотр страницы side-by-side с разметкой XAML, позволяя см. в разделе пользовательского интерфейса к просмотру при вводе.

## <a name="xaml-namespacesnamespacesmd"></a>[Пространства имен языка XAML](namespaces.md)

XAML использует `xmlns` XML-атрибута для объявления пространств имен. В этой статье синтаксис пространства имен XAML и показано, как объявление пространства имен XAML для доступа к типу.

## <a name="xaml-custom-namespace-schemascustom-namespace-schemasmd"></a>[Схемы настраиваемых пространств имен XAML](custom-namespace-schemas.md)

Схема пользовательского пространства имен XAML может быть определена с `XmlnsDefinitionAttribute` класс, который задает сопоставление между пользовательский URL-адрес и один или несколько пространств имен CLR. Затем схемы пользовательского пространства имен можно использовать в объявлениях пространств имен XAML.

## <a name="xaml-namespace-recommended-prefixescustom-prefixmd"></a>[Рекомендуемые префиксы пространства имен XAML](custom-prefix.md)

`XmlnsPrefixAttribute` Класс может использоваться авторами элемента управления, чтобы указать рекомендуемый префикс, связываемый с пространством имен XAML, для использования XAML.

## <a name="xaml-markup-extensionsmarkup-extensionsindexmd"></a>[Расширения разметки XAML](markup-extensions/index.md)

XAML включает расширения разметки для настройки атрибутов в значения или объекты за пределами нельзя выразить с помощью простых строк. К ним относятся, ссылающиеся на константы, статические свойства и поля, словари ресурсов и привязки данных.

## <a name="field-modifiersfield-modifiersmd"></a>[Модификаторы полей](field-modifiers.md)

`x:FieldModifier` Атрибут пространства имен указывает уровень доступа для созданных полей для именованных элементов XAML.

## <a name="passing-argumentspassing-argumentsmd"></a>[Передача аргументов](passing-arguments.md)

XAML можно использовать для передачи аргументов конструктора не по умолчанию или к методам фабрики. В этой статье демонстрируется использование атрибутов XAML, которые могут использоваться для передачи аргументов к конструкторам для вызова методов фабрики, а также указать тип универсального аргумента.

## <a name="bindable-propertiesbindable-propertiesmd"></a>[Привязываемые свойства](bindable-properties.md)

В Xamarin.Forms функциональные возможности параметров среды выполнения (CLR) расширен с помощью свойства связывания. Может быть привязано — это специальный тип свойства, где значение свойства отслеживается системой свойств Xamarin.Forms. В этой статье содержатся вводные привязываемые свойства, а также показано, как создавать и использовать их.

## <a name="attached-propertiesattached-propertiesmd"></a>[Вложенные свойства](attached-properties.md)

Присоединенное свойство — это специальный тип привязки свойства, определенные в одном классе, но присоединен к другим объектам и распознается в XAML как атрибут, содержащий класс и имя свойства, разделенные точкой. В этой статье содержатся вводные присоединенных свойств и показано, как создавать и использовать их.

## <a name="resource-dictionariesresource-dictionariesmd"></a>[Словари ресурсов](resource-dictionaries.md)

Ресурсы XAML, определения объектов, которые могут использоваться более одного раза. Объект [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) позволяет ресурсы, определенные в одном месте, и повторно использоваться на протяжении всего приложения Xamarin.Forms. В этой статье показано, как создавать и использовать `ResourceDictionary`и способы слияния одного `ResourceDictionary` в другой.

## <a name="loading-xaml-at-runtimeruntime-loadmd"></a>[Загрузка XAML во время выполнения](runtime-load.md)

XAML можно загружать и проводится во время выполнения с помощью [ `LoadFromXaml` ](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) методы расширения.
