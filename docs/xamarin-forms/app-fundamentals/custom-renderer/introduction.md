---
title: Общие сведения о настраиваемых отрисовщиках
description: В этой статье содержатся общие сведения о пользовательских отрисовщиках и описывается процесс создания настраиваемого отрисовщика.
ms.prod: xamarin
ms.assetid: 264314BE-1C5C-4727-A14E-F6F98151CDBD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/19/2016
ms.openlocfilehash: ad2868a82f662f45066a6111a1dd3bd2aacad671
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/13/2020
ms.locfileid: "70771882"
---
# <a name="introduction-to-custom-renderers"></a>Общие сведения о настраиваемых отрисовщиках

_Настраиваемые отрисовщики обеспечивают удобный подход к настройке внешнего вида и поведения элементов управления Xamarin.Forms. Их можно использовать для незначительных изменений стиля или сложной настройки макетов и поведения в зависимости от платформы. В этой статье содержатся общие сведения о настраиваемых отрисовщиках и описывается процесс создания настраиваемого отрисовщика._

[Страницы, макеты и элементы управления](~/xamarin-forms/user-interface/controls/index.md) Xamarin.Forms представляют общий интерфейс API для описания кроссплатформенных пользовательских интерфейсов на мобильных устройствах. Каждая страница, макет и элемент управления отрисовываются по-разному на разных платформах с помощью класса `Renderer`, который создает собственный элемент управления (соответствующий представлению Xamarin.Forms), размещает его на экране и реализует поведение, определенное в общем коде.

Разработчики могут реализовывать пользовательские классы `Renderer` для настройки внешнего вида или работы элемента управления. Настраиваемые отрисовщики заданного типа можно добавить в проект одного приложения, чтобы настроить элемент управления в одном месте, обеспечив поведение по умолчанию на других платформах. Либо можно добавить разные настраиваемые отрисовщики в каждый проект приложения, чтобы обеспечить разный внешний вид в iOS, Android и универсальной платформе Windows (UWP). Однако реализация класса настраиваемого отрисовщика для выполнения простой настройки элемента управления чаще всего является довольно трудоемкой. Эффекты упрощают этот процесс и обычно используются для небольшого изменения стилей. Дополнительные сведения см. в статье [Эффекты](~/xamarin-forms/app-fundamentals/effects/index.md).

## <a name="examining-why-custom-renderers-are-necessary"></a>Анализ того, почему настраиваемые отрисовщики необходимы

Изменение внешнего вида элемента управления Xamarin.Forms без использования настраиваемого отрисовщика представляет собой двухэтапный процесс, включающий в себя создание пользовательского элемента управления посредством выделения подкласса и последующее использование этого элемента управления вместо исходного. Следующий пример кода показывает пример выделения подкласса для элемента управления `Entry`:

```csharp
public class MyEntry : Entry
{
  public MyEntry ()
  {
    BackgroundColor = Color.Gray;
  }
}
```

Элемент управления `MyEntry` — это элемент управления `Entry`, где для `BackgroundColor` задан серый цвет, и на него можно ссылаться в XAML, объявив пространство имен для его расположения и используя префикс пространства имен в элементе управления. Следующий пример кода показывает, как пользовательский элемент управления `MyEntry` может быть использован `ContentPage`:

```xaml
<ContentPage
    ...
    xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer"
    ...>
    ...
    <local:MyEntry Text="In Shared Code" />
    ...
</ContentPage>
```

Префикс пространства имен `local` может быть любым. Тем не менее значения `namespace` и `assembly` должны соответствовать данным пользовательского элемента управления. После объявления пространства имен префикс используется для ссылки на пользовательский элемент управления.

> [!NOTE]
> Определение `xmlns` выполняется гораздо проще в проектах библиотеки .NET Standard, чем в общих проектах. Библиотека .NET Standard компилируется в сборку, поэтому легко определить, какое значение `assembly=CustomRenderer` должно использоваться. При использовании общих проектов все общие ресурсы (включая XAML) компилируются в каждый из ссылающихся проектов, поэтому, если проекты iOS, Android и UWP имеют собственные *имена сборок*, написать объявление `xmlns` невозможно, так как значение должно быть разным для каждого приложения. Пользовательские элементы управления в XAML для общих проектов потребуют настройки одного и того же имени сборки в каждом проекте приложения.

После этого пользовательский элемент управления `MyEntry` отрисовывается на каждой платформе на сером фоне, как показано на следующих снимках экрана:

![](introduction-images/screenshots.png "MyEntry Custom Control on each Platform")

Изменение цвета фона элемента управления на каждой платформе было достигнуто исключительно посредством создания подкласса элемента управления. Однако возможности этого метода ограничены, так как он не позволяет воспользоваться преимуществами усовершенствования и настройками конкретной платформы. Если в них есть потребность, нужно реализовать настраиваемые отрисовщики.

## <a name="creating-a-custom-renderer-class"></a>Создание класса настраиваемого отрисовщика

Процесс создания класса пользовательского отрисовщика выглядит следующим образом:

1. Создайте подкласс класса отрисовщика, отвечающего за отрисовку собственного элемента управления.
1. Переопределите метод, который отрисовывает собственный элемент управления, и напишите логику для настройки элемента управления. Часто метод `OnElementChanged` используется для отрисовки собственного элемента управления, который вызывается при создании соответствующего элемента управления Xamarin.Forms.
1. Добавьте атрибут `ExportRenderer` в класс пользовательского отрисовщика, чтобы указать, что он будет использоваться для отрисовки пользовательского элемента управления Xamarin.Forms. Этот атрибут используется для регистрации пользовательского отрисовщика в Xamarin.Forms.

> [!NOTE]
> Для большинства элементов Xamarin.Forms предоставлять пользовательский отрисовщик в проекте для каждой платформы необязательно. Если пользовательский отрисовщик не зарегистрирован, будет использоваться отрисовщик по умолчанию для базового класса элемента управления. Однако настраиваемые отрисовщики необходимы в проекте каждом платформы при отрисовке элемента [View](xref:Xamarin.Forms.View) или [ViewCell](xref:Xamarin.Forms.ViewCell).

Статьи из этой серии содержат демонстрации и пояснения по данному процессу для различных элементов Xamarin.Forms.

## <a name="troubleshooting"></a>Устранение неполадок

Если пользовательский элемент управления находится в проекте библиотеки .NET Standard, добавляемом в решение (а, например, не библиотеки .NET Standard, создаваемой шаблоном проекта приложения Visual Studio для Mac/Visual Studio Xamarin.Forms), исключение может произойти в iOS при попытке доступа к пользовательскому элементу управления. При возникновении этой проблемы ее можно решить, создав ссылку на пользовательский элемент управления из класса `AppDelegate`:

```csharp
var temp = new ClassInPCL(); // in AppDelegate, but temp not used anywhere
```

Это принуждает компилятор распознать тип `ClassInPCL`, разрешая его. Кроме того, можно добавить атрибут `Preserve` в класс `AppDelegate` для достижения такого же результата:

```csharp
[assembly: Preserve (typeof (ClassInPCL))]
```

При этом создается ссылка на тип `ClassInPCL`, указывая, что он требуется во время выполнения. Дополнительные сведения см. в разделе [Сохранение кода](~/ios/deploy-test/linker.md).

## <a name="summary"></a>Сводка

В этой статье представлены общие сведения о пользовательских отрисовщиках, а также описан процесс создания настраиваемого отрисовщика. Пользовательские отрисовщики обеспечивают эффективный подход к настройке внешнего вида и поведения элементов управления Xamarin.Forms. Их можно использовать для незначительных изменений стиля или сложной настройки макетов и поведения в зависимости от платформы.

## <a name="related-links"></a>Связанные ссылки

- [Эффекты](~/xamarin-forms/app-fundamentals/effects/index.md)
