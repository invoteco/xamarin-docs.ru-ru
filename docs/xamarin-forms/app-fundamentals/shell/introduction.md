---
title: Введение в оболочку Xamarin.Forms
description: Оболочка Xamarin.Forms предоставляет набор основных возможностей, которые требуются для большинства приложений, таких как стандартизированная навигация для пользователя, схема навигации на основе URI и интегрированный обработчик поиска.
ms.prod: xamarin
ms.assetid: 4604DCB5-83DA-458A-8B02-6508A740BE0E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2019
ms.openlocfilehash: cb2ae3afe9db86d4db603d499ef0e75e7cbbf552
ms.sourcegitcommit: 52fb214c0e0243587d4e9ad9306b75e92a8cc8b7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/01/2020
ms.locfileid: "76940407"
---
# <a name="xamarinforms-shell-introduction"></a>Введение в оболочку Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)

Оболочка Xamarin.Forms упрощает разработку мобильных приложений, предоставляя основные возможности, которые необходимы для большинства мобильных приложений, такие как:

- единое место для описания визуальной структуры приложения;
- единый пользовательский интерфейс навигации;
- схема навигации на основе URI, которая позволяет переходить на любую страницу в приложении;
- Обработчик интегрированного поиска.

Кроме того, приложения оболочки получают более высокую скорость отрисовки и снижение потребления памяти.

> [!IMPORTANT]
> Оболочку можно внедрить в уже существующие приложения, сразу же получив преимущества улучшения навигации, производительности и расширяемости.

## <a name="platform-support"></a>Поддержка платформ

Оболочка Xamarin.Forms полностью доступна в iOS и Android, но лишь частично доступна на универсальной платформе Windows (UWP). Кроме того, сейчас оболочка в UWP является экспериментальной и может использоваться только посредством добавления следующей строки кода в класс `App` в проекте UWP перед вызовом `Forms.Init`:

```csharp
global::Xamarin.Forms.Forms.SetFlags("Shell_UWP_Experimental");
```

Сведения о том, как добавить проект UWP в решение Xamarin.Forms, см. в статье о [настройке проектов Windows](~/xamarin-forms/platform/windows/installation/index.md).

## <a name="shell-navigation-experience"></a>Возможности навигации оболочки

Оболочка предоставляет специализированный пользовательский интерфейс навигации на основе всплывающих элементов и вкладок. Верхний уровень навигации в приложении оболочки — это всплывающее меню или нижняя панель вкладок, выбор которых обусловлен требованиями навигации в приложении. В следующем примере показано приложение, в котором для верхнего уровня навигации используется всплывающее меню:

[![Снимок экрана: всплывающий элемент оболочки в iOS и Android](introduction-images/flyout.png "Всплывающий элемент оболочки")](introduction-images/flyout-large.png#lightbox "Всплывающий элемент оболочки")

Выбор элемента приводит к отображению нижней вкладки, которая представляет выбранный элемент:

[![Снимок экрана: нижние вкладки оболочки в iOS и Android](introduction-images/monkeys.png "Нижние вкладки оболочки")](introduction-images/monkeys-large.png#lightbox "Нижние вкладки оболочки")

> [!NOTE]
> Если всплывающее меню отсутствует, нижняя панель вкладок считается верхним уровнем навигации в приложении.

Каждая вкладка отображает [`ContentPage`](xref:Xamarin.Forms.ContentPage). Но если вкладка содержит более одной страницы, перемещение по страницам осуществляется с помощью верхней панели вкладок.

[![Снимок экрана: верхние вкладки оболочки в iOS и Android](introduction-images/cats.png "Верхние вкладки оболочки")](introduction-images/cats-large.png#lightbox "Верхние вкладки оболочки")

В каждой вкладке можно перейти на дополнительные объекты [`ContentPage`](xref:Xamarin.Forms.ContentPage):

[![Снимок экрана: перемещение по страницам оболочки в iOS и Android](introduction-images/cat-details.png "Навигация в приложении оболочки")](introduction-images/cat-details-large.png#lightbox "Навигация в приложении оболочки")

## <a name="related-links"></a>Связанные ссылки

- [Xaminals (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)
