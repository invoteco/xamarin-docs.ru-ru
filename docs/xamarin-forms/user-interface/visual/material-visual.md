---
title: Материал Xamarin.Forms Visual
description: Visual материал Xamarin.Forms можно использовать для создания приложений Xamarin.Forms, которые выглядят идентичными или мало чем отличается в iOS и Android.
ms.prod: xamarin
ms.assetid: B774F68C-EF9E-49E1-B738-CDC64879ADA2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/05/2019
ms.openlocfilehash: 46ffe29f898e2f7bd8fcbe759f5a2f54e3dd60e1
ms.sourcegitcommit: 00744f754527e5b55154365f89691caaf1c9d929
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/07/2019
ms.locfileid: "57557506"
---
# <a name="xamarinforms-material-visual"></a>Материал Xamarin.Forms Visual

Visual материал Xamarin.Forms можно использовать для создания приложений Xamarin.Forms, которые выглядят идентичными или мало чем отличается в iOS и Android. Это достигается с помощью дополнительных модулей подготовки отчетов, которые реализуют материала внешний вид, с приложениями, переключиться на внешний через `Visual` свойство:

```xaml
<ContentPage ...
             Visual="Material">
    ...
</ContentPage>
```

Ниже приведен аналогичный код C#:

```csharp
ContentPage contentPage = new ContentPage();
contentPage.Visual = VisualMarker.Material;
```

> [!IMPORTANT]
> `Visual` Свойство определено в `VisualElement` класс с представлениями наследование `Visual` значение свойства от их родительских объектов. Таким образом, задание `Visual` свойство `ContentPage` гарантирует, что все поддерживаемые представления на странице будет использовать этот внешний вид. Кроме того `Visual` свойство можно переопределить для представления.

Материала модулей подготовки отчетов в настоящее время включены для следующих представлений на iOS и Android:

- [`Button`](xref:Xamarin.Forms.Button)
- [`Entry`](xref:Xamarin.Forms.Entry)
- [`Frame`](xref:Xamarin.Forms.Frame)
- [`ProgressBar`](xref:Xamarin.Forms.ProgressBar)
- [`DatePicker`](xref:Xamarin.Forms.DatePicker)
- [`TimePicker`](xref:Xamarin.Forms.TimePicker)
- [`Picker`](xref:Xamarin.Forms.Picker)
- [`ActivityIndicator`](xref:Xamarin.Forms.ActivityIndicator)
- [`Editor`](xref:Xamarin.Forms.Editor)
- [`Slider`](xref:Xamarin.Forms.Slider)
- [`Stepper`](xref:Xamarin.Forms.Stepper)

С функциональной точки зрения материала визуализаторов ничем не отличаются, для модулей подготовки отчетов по умолчанию.

В iOS и Android для проекта платформы должен быть [Xamarin.Forms.Visual.Material](https://www.nuget.org/packages/ https://www.nuget.org/packages/Xamarin.Forms.Visual.Material/) установленный пакет NuGet. После установки пакета NuGet, требуется код инициализации в каждом проекте платформы *после* `Xamarin.Forms.Forms.Init` вызова метода. Для устройств iOS используйте следующий код:

```csharp
global::Xamarin.Forms.Forms.Init();
FormsMaterial.Init();
```

В Android, необходимо передать те же параметры, передаваемые `Forms.Init`:

```csharp
global::Xamarin.Forms.Forms.Init(this, bundle);
FormsMaterial.Init(this, bundle);
```

> [!IMPORTANT]
> В Android материал работает только с TargetFramework 9.0 (API 28). Кроме того платформа проекта необходимо использовать v28 библиотек поддержки, и тема должен наследовать от темы материал компонентов или наследовать от AppCompat темы по-прежнему. Дополнительные сведения см. в разделе [Приступая к работе с компонентами материал для Android](https://github.com/material-components/material-components-android/blob/master/docs/getting-started.md).

На следующих снимках экрана показано пользовательский интерфейс, который включает в себя четыре представления, какие материалы существует модулей подготовки отчетов, но отображаются с использованием модулей подготовки отчетов по умолчанию:

[![Снимок экрана модулей подготовки отчетов по умолчанию, в iOS и Android](material-visual-images/default-renderers.png "представлениями с использованием модулей подготовки отчетов по умолчанию")](material-visual-images/default-renderers-large.png#lightbox)

На следующих снимках экрана показано тот же пользовательский интерфейс, с помощью материалов модулей подготовки отчетов к просмотру:

[![Снимок экрана материала модулей подготовки отчетов, в iOS и Android](material-visual-images/material-renderers.png "представлениями с использованием материала модулей подготовки отчетов")](material-visual-images/material-renderers-large.png#lightbox)

> [!NOTE]
> Готовый для просмотра элементы управления остаются собственные элементы управления материала модулей подготовки отчетов и таким образом будет по-прежнему существовать отличия между платформами сферах, таких как шрифты, теней, цвета и повышение прав в пользовательском интерфейсе.

## <a name="material-renderers"></a>Материала модулей подготовки отчетов

Материала модулей подготовки отчетов можно настраивать, так же, как модули подготовки отчетов по умолчанию, с помощью следующих базовых классов:

- `MaterialButtonRenderer`
- `MaterialEntryRenderer`
- `MaterialFrameRenderer`
- `MaterialProgressBarRenderer`
- `MaterialDatePickerRenderer`
- `MaterialTimePickerRenderer`
- `MaterialPickerRenderer`
- `MaterialActivityIndicatorRenderer`
- `MaterialEditorRenderer`
- `MaterialSliderRenderer`
- `MaterialStepperRenderer`

Ниже приведен пример настройки `MaterialProgressBarRenderer` класса:

```csharp
using Xamarin.Forms.Material.Android;

[assembly: ExportRenderer(typeof(ProgressBar), typeof(CustomMaterialProgressBarRenderer), new[] { typeof(VisualMarker.MaterialVisual) })]
namespace MyApp.Android
{
    public class CustomMaterialProgressBarRenderer : MaterialProgressBarRenderer
    {
        ...
    }
}
```

## <a name="related-links"></a>Связанные ссылки

- [Пользовательские отрисовщики](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
