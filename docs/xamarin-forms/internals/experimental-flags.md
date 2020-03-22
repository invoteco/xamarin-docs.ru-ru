---
title: Экспериментальные флаги Xamarin. Forms
description: Экспериментальные флаги Xamarin. Forms позволяют группе инженеров-разработчиков поставлять новые функции пользователям быстрее, в то же время сохраняя возможность изменять API функций до того, как они переходят в стабильный выпуск.
ms.prod: xamarin
ms.assetid: AF4BDD27-89F6-48AE-A8CD-D7E4DDA2CCA2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/20/2020
ms.openlocfilehash: cebb996da992058616f9cf96ef3212c9ce27022a
ms.sourcegitcommit: 6c60914b380ff679bbffd7790edd4d5e18005d0a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/21/2020
ms.locfileid: "80112603"
---
# <a name="xamarinforms-experimental-flags"></a>Экспериментальные флаги Xamarin. Forms

Когда реализована новая функция Xamarin. Forms, она иногда помещается за экспериментальный флаг. Это позволяет группе разработчиков предоставлять новые функции быстрее, в то же время сохраняя возможность изменения API-интерфейсов функций до того, как они переходят в стабильный выпуск. Затем экспериментальный флаг удаляется после того, как функция переместится в стабильный выпуск.

Xamarin. Forms включает следующие экспериментальные флаги:

- `CarouselView_Experimental`
- `IndicatorView_Experimental`
- `Markup_Experimental`
- `MediaElement_Experimental`
- `Shell_UWP_Experimental`
- `StateTriggers_Experimental`
- `SwipeView_Experimental`

Использование функций, которые находятся за экспериментальным флагом, требует включения флага или флагов в приложении. Существует два подхода к включению экспериментальных флагов.

- Включите экспериментальный флаг или флаги в проектах платформы.
- Включите экспериментальный флаг или флаги в классе `App`.

> [!WARNING]
> Использование функциональных возможностей, которые находятся за экспериментальным флагом, без включения флага, приведет к тому, что приложение создаст исключение, указывающее, какой флаг должен быть включен.

## <a name="enable-flags-in-platform-projects"></a>Включение флагов в проектах платформы

Метод `Xamarin.Forms.Forms.SetFlags` можно использовать для включения экспериментального флага в проектах платформы:

```csharp
Xamarin.Forms.Forms.SetFlags("CarouselView_Experimental");
```

Метод `SetFlags` должен быть вызван в классе `AppDelegate` в iOS, в классе `MainActivity` в Android и в классе `App` в UWP.

> [!IMPORTANT]
> Включение экспериментального флага в проектах платформы должно происходить до вызова метода `Forms.Init`.

Метод `Xamarin.Forms.Forms.SetFlags` принимает аргумент массива `string`, что делает возможным включение нескольких экспериментальных флагов в одном вызове метода:

```csharp
Xamarin.Forms.Forms.SetFlags(new string[] { "CarouselView_Experimental", "IndicatorView_Experimental", "SwipeView_Experimental" });
```

> [!WARNING]
> Никогда не вызывайте метод `SetFlags` более одного раза, так как последующие вызовы будут перезаписывать результат предыдущих вызовов.

## <a name="enable-flags-in-your-app-class"></a>Включение флагов в классе приложения

Метод `Device.SetFlags` можно использовать для включения экспериментального флага в классе `App` в проекте общего кода:

```csharp
Device.SetFlags(new string[]{ "MediaElement_Experimental" });
```

Метод `Device.SetFlags` принимает аргумент `IReadOnlyList<string>`, что позволяет включить несколько экспериментальных флагов в одном вызове метода:

```csharp
Device.SetFlags(new string[]{ "CarouselView_Experimental", "MediaElement_Experimental", "SwipeView_Experimental" });
```

> [!WARNING]
> Никогда не вызывайте метод `SetFlags` более одного раза, так как последующие вызовы будут перезаписывать результат предыдущих вызовов.
