---
title: Направление извлечения Рефрешвиев в Windows
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать конкретную платформу Windows, которая позволяет изменить направление извлечения Рефрешвиев.
ms.prod: xamarin
ms.assetid: 407A862B-281E-4384-9696-C0655830B84D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2019
ms.openlocfilehash: cf2ab38bed7b45a48fcf0b5f86add49c0d4cc21f
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/21/2019
ms.locfileid: "72697653"
---
# <a name="refreshview-pull-direction-on-windows"></a>Направление извлечения Рефрешвиев в Windows

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Это универсальная платформа Windows, зависящее от платформы, позволяет изменить направление извлечения `RefreshView` в соответствии с ориентацией прокручиваемого элемента управления, отображающего данные. Он используется в XAML путем установки для свойства `RefreshView.RefreshPullDirection` BIND значения `RefreshPullDirection` перечисления:

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <RefreshView windows:RefreshView.RefreshPullDirection="LeftToRight"
                 IsRefreshing="{Binding IsRefreshing}"
                 Command="{Binding RefreshCommand}">
        <ScrollView>
            ...
        </ScrollView>
    </RefreshView>
 </ContentPage>
```

Кроме того, его можно использовать с C# помощью API Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...
refreshView.On<Windows>().SetRefreshPullDirection(RefreshPullDirection.LeftToRight);
```

Метод `RefreshView.On<Windows>` указывает, что эта платформа будет запускаться только на универсальная платформа Windows. Метод `RefreshView.SetRefreshPullDirection` в пространстве имен [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) используется для задания направления извлечения `RefreshView`, при этом перечисление `RefreshPullDirection` предоставляет четыре возможных значения:

- `LeftToRight` указывает, что операция извлечения слева направо инициирует обновление.
- `TopToBottom` указывает, что операция извлечения сверху вниз инициирует обновление, а является направлением по умолчанию для `RefreshView`.
- `RightToLeft` указывает, что операция извлечения справа налево инициирует обновление.
- `BottomToTop` указывает, что операция извлечения снизу вверх инициирует обновление.

Кроме того, можно использовать метод `GetRefreshPullDirection` для возврата текущего `RefreshPullDirection` `RefreshView`.

В результате заданная `RefreshPullDirection` применяется к `RefreshView`, чтобы задать направление извлечения, совпадающее с ориентацией прокручиваемого элемента управления, отображающего данные. На следующем снимке экрана показан `RefreshView` с `LeftToRight`ным направлением извлечения:

[![Снимок экрана Рефрешвиев с направлением извлечения слева направо в UWP](refreshview-pulldirection-images/refreshview-pulldirection.png "Рефрешвиев с направлением извлечения слева направо")](refreshview-pulldirection-images/refreshview-pulldirection-large.png#lightbox "Рефрешвиев с направлением извлечения слева направо")

> [!NOTE]
> При изменении направления запроса начальное расположение круга хода выполнения автоматически поворачивается таким образом, чтобы стрелка начиналась в соответствующем положении для направления запроса.

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ВиндовсспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)
