---
title: Жизненный цикл оболочки Xamarin.Forms
description: Приложения оболочки учитывают жизненный цикл Xamarin.Forms, и событие Appearing возникает, когда страница собирается отображаться на экране, а событие Disappearing возникает, когда страница собирается исчезнуть с экрана.
ms.prod: xamarin
ms.assetid: 4E4EE50E-3BB4-441D-8355-CD9CD26ED1D0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/25/2019
ms.openlocfilehash: 2ed51763b5866c15e91d88a6a1a58c7285fb5973
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/13/2020
ms.locfileid: "72749768"
---
# <a name="xamarinforms-shell-lifecycle"></a>Жизненный цикл оболочки Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)

Приложения оболочки учитывают жизненный цикл Xamarin.Forms, и событие `Appearing` возникает, когда страница собирается отображаться на экране, а событие `Disappearing` возникает, когда страница собирается исчезнуть с экрана. Эти события распространяются на страницы и могут быть обработаны путем переопределения методов [`OnAppearing`](xref:Xamarin.Forms.Page.OnAppearing) или [`OnDisappearing`](xref:Xamarin.Forms.Page.OnDisappearing) на странице.

> [!NOTE]
> В приложении оболочки события `Appearing` и `Disappearing` вызываются из кросс-платформенного кода до того, как код платформы делает страницу видимой или удаляет страницу с экрана.

Дополнительные сведения о жизненном цикле приложения Xamarin.Forms см. в статье [Жизненный цикл приложения Xamarin.Forms](~/xamarin-forms/app-fundamentals/app-lifecycle.md).

## <a name="hierarchical-navigation"></a>Иерархическая навигация

В приложении оболочки принудительная отправка страницы в стек навигации приведет к тому, что текущий видимый объект `ShellContent` и его содержимое страницы порождает событие `Disappearing`. Аналогичным образом, появление последней страницы из стека навигации приведет к тому, что новый видимый объект `ShellContent` и его содержимое страницы порождает событие `Appearing`.

Дополнительные сведения об иерархической навигации см. в статье [Иерархическая навигация в Xamarin.Forms](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md).

## <a name="modal-navigation"></a>Модальная навигация

В приложении оболочки принудительная отправка модальной страницы в модальный стек навигации приведет к тому, что все видимые объекты оболочки будут вызывать событие `Disappearing`. Аналогично, появление последней модальной страницы из модального стека навигации приведет к тому, что все видимые объекты оболочки будут вызывать событие `Appearing`.

Дополнительные сведения о модальной навигации см. в статье [Модальные страницы в Xamarin.Forms](~/xamarin-forms/app-fundamentals/navigation/modal.md).

## <a name="related-links"></a>Связанные ссылки

- [Xaminals (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)
- [Жизненный цикл приложения Xamarin.Forms](~/xamarin-forms/app-fundamentals/app-lifecycle.md)
- [Модальные страницы Xamarin.Forms](~/xamarin-forms/app-fundamentals/navigation/modal.md)
