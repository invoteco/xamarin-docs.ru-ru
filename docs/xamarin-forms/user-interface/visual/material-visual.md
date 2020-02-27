---
title: Визуальный элемент материалов Xamarin. Forms
description: Визуальный элемент "материалы Xamarin. Forms" можно использовать для создания приложений Xamarin. Forms, которые выглядят примерно так же, как в iOS и Android.
ms.prod: xamarin
ms.assetid: B774F68C-EF9E-49E1-B738-CDC64879ADA2
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 11/25/2019
ms.openlocfilehash: 81ef3c44d6eb8aaf4dd0ec467e11ef04adb02a76
ms.sourcegitcommit: 5a6124271a679b8961fa9430bd738fcb18544e92
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/26/2020
ms.locfileid: "77618406"
---
# <a name="xamarinforms-material-visual"></a>Визуальный элемент материалов Xamarin. Forms

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-visualdemos)

[Проектирование материалов](https://material.io) — это упрямого система разработки, созданная Google, которая предписывает размер, цвет, расстояния и другие аспекты того, как должны выглядеть и работать представления и макеты.

Визуальный элемент "материалы по Xamarin. Forms" можно использовать для применения правил проектирования материалов к приложениям Xamarin. Forms, создавая приложения, которые выглядят примерно одинаково в iOS и Android. Если визуальный элемент "материалы" включен, Поддерживаемые представления применяют один и тот же проект кросс-платформенный режим, создавая единообразный внешний вид.

[Визуальные снимки экрана ![материалов](material-visual-images/material-visual-cropped.png)](material-visual-images/material-visual.png#lightbox)

Процесс включения визуального элемента "материалы" в Xamarin. Forms в приложении:

1. Добавьте пакет NuGet [Xamarin. Forms. Visual. Material](https://www.nuget.org/packages/Xamarin.Forms.Visual.Material/) в проекты iOS и Android Platform. Этот пакет NuGet предоставляет оптимизированные для разработки материалов модули подготовки отчетов в iOS и Android. В iOS пакет предоставляет транзитивное зависимость для [Xamarin. iOS. материалкомпонентс](https://www.nuget.org/packages/Xamarin.iOS.MaterialComponents), которая является C# привязкой к [компонентам материалов Google для iOS](https://material.io/develop/ios/). В Android пакет предоставляет целевые объекты сборки, чтобы обеспечить правильную настройку TargetFramework.
1. Инициализация визуального элемента "материалы" в каждом проекте платформы. Дополнительные сведения см. в разделе [Инициализация визуального элемента Material](#initialize-material-visual).
1. Создайте визуальные элементы управления "материал", задав для свойства [`Visual`](xref:Xamarin.Forms.VisualElement.Visual) значение `Material` на всех страницах, которые должны применять правила дизайна материалов. Дополнительные сведения см. в разделе Использование модулей подготовки отчетов к [просмотру](#apply-material-visual).
1. используемых Настройка элементов управления материалами. Дополнительные сведения см. в разделе [Настройка элементов управления материалами](#customize-material-visual).

> [!IMPORTANT]
> В Android визуальный элемент "материал" требует минимальную версию 5,0 (API 21) или более TargetFramework, а также версию 9,0 (API 28). Кроме того, проект платформы требует наличия библиотек поддержки Android 28.0.0 или более поздней версии, и его тема должна наследовать от темы "компоненты материала" или продолжить наследовать от темы AppCompat. Дополнительные сведения см. в статье [Начало работы с материальными компонентами для Android](https://github.com/material-components/material-components-android/blob/master/docs/getting-started.md).

Визуальный элемент "материалы" в настоящее время поддерживает следующие элементы управления:

- [`ActivityIndicator`](xref:Xamarin.Forms.ActivityIndicator)
- [`Button`](xref:Xamarin.Forms.Button)
- [`CheckBox`](xref:Xamarin.Forms.CheckBox)
- [`DatePicker`](xref:Xamarin.Forms.DatePicker)
- [`Editor`](xref:Xamarin.Forms.Editor)
- [`Entry`](xref:Xamarin.Forms.Entry)
- [`Frame`](xref:Xamarin.Forms.Frame)
- [`Picker`](xref:Xamarin.Forms.Picker)
- [`ProgressBar`](xref:Xamarin.Forms.ProgressBar)
- [`Slider`](xref:Xamarin.Forms.Slider)
- [`Stepper`](xref:Xamarin.Forms.Stepper)
- [`TimePicker`](xref:Xamarin.Forms.TimePicker)

Элементы управления материалами реализуются модулями подготовки материалов, которые применяют правила проектирования материалов. Функционально, модули подготовки материалов не отличаются от модулей подготовки отчетов по умолчанию. Дополнительные сведения см. в разделе [Настройка визуального элемента Material](#customize-material-visual).

## <a name="initialize-material-visual"></a>Инициализация визуального элемента материалов

После установки пакета NuGet для [Xamarin. Forms. Visual. Material](https://www.nuget.org/packages/Xamarin.Forms.Visual.Material/) модули подготовки материалов должны быть инициализированы в каждом проекте платформы.

В iOS это должно произойти в **AppDelegate.CS** путем вызова метода `Xamarin.Forms.FormsMaterial.Init` *после* метода `Xamarin.Forms.Forms.Init`:

```csharp
global::Xamarin.Forms.Forms.Init();
global::Xamarin.Forms.FormsMaterial.Init();
```

В Android это должно произойти в **MainActivity.CS** путем вызова метода `Xamarin.Forms.FormsMaterial.Init` *после* метода `Xamarin.Forms.Forms.Init`:

```csharp
global::Xamarin.Forms.Forms.Init(this, savedInstanceState);
global::Xamarin.Forms.FormsMaterial.Init(this, savedInstanceState);
```

## <a name="apply-material-visual"></a>Применение визуального элемента "материалы"

Приложения могут включить визуальный элемент "материалы", задав для свойства [`VisualElement.Visual`](xref:Xamarin.Forms.VisualElement.Visual) на странице, макете или представлении `Material`:

```xaml
<ContentPage Visual="Material"
             ...>
    ...
</ContentPage>
```

Эквивалентный код на C# выглядит так:

```csharp
ContentPage contentPage = new ContentPage();
contentPage.Visual = VisualMarker.Material;
```

Если задать для свойства `VisualElement.Visual` значение `Material`, приложение будет использовать визуальные модули подготовки материалов вместо модулей подготовки отчетов по умолчанию. Свойству [`Visual`](xref:Xamarin.Forms.VisualElement.Visual) можно присвоить любой тип, реализующий `IVisual`, с классом [`VisualMarker`](xref:Xamarin.Forms.VisualMarker) , который предоставляет следующие свойства `IVisual`:

- `Default` — указывает, что представление должно отображаться с помощью модуля подготовки отчетов по умолчанию.
- `MatchParent` — указывает, что представление должно использовать тот же модуль подготовки отчетов, что и его непосредственного родителя.
- `Material` — указывает, что представление должно отображаться с помощью модуля подготовки материалов.

> [!IMPORTANT]
> Свойство [`Visual`](xref:Xamarin.Forms.VisualElement.Visual) определено в классе [`VisualElement`](xref:Xamarin.Forms.VisualElement) , в котором представления наследуют значение свойства `Visual` от своих родителей. Поэтому установка свойства `Visual` в [`ContentPage`](xref:Xamarin.Forms.ContentPage) гарантирует, что все поддерживаемые представления на странице будут использовать этот визуальный элемент. Кроме того, свойство `Visual` может быть переопределено в представлении.

На следующих снимках экрана показан пользовательский интерфейс, отображаемый с помощью модулей подготовки отчетов по умолчанию:

[![Снимок экрана модулей подготовки отчетов по умолчанию в iOS и Android](material-visual-images/default-renderers.png "Представления, использующие модули подготовки отчетов по умолчанию")](material-visual-images/default-renderers-large.png#lightbox)

На следующих снимках экрана показан один и тот же пользовательский интерфейс, отображаемый с помощью модулей подготовки материалов:

[![Снимок экрана модулей подготовки материалов в iOS и Android](material-visual-images/material-renderers.png "Представления, использующие модули подготовки материалов")](material-visual-images/material-renderers-large.png#lightbox)

Основные видимые различия между модулями подготовки отчетов по умолчанию и модулями подготовки материалов, показанными здесь, означают, что модули подготовки материалов записывают [`Button`](xref:Xamarin.Forms.Button) текста и округляют углы [`Frame`](xref:Xamarin.Forms.Frame) границ. Однако модули подготовки материалов используют собственные элементы управления, поэтому для таких областей, как шрифты, тени, цвета и повышения прав, могут существовать различия в пользовательском интерфейсе.

> [!NOTE]
> Компоненты проектирования материалов тесно соответствуют рекомендациям Google. В результате визуализаторы дизайна материалов перемещаются в сторону изменения размера и поведения. Если требуется больший контроль над стилями или поведением, можно по-прежнему создать собственный [результат](~/xamarin-forms/app-fundamentals/effects/index.md), [поведение](~/xamarin-forms/app-fundamentals/behaviors/index.md)или [пользовательский модуль подготовки](~/xamarin-forms/app-fundamentals/custom-renderer/index.md) отчетов для получения необходимых сведений.

## <a name="customize-material-visual"></a>Настройка визуального элемента "материалы"

Пакет для визуального элемента NuGet — это коллекция модулей подготовки отчетов, которые реализуют элементы управления Xamarin. Forms. Настройка визуальных элементов управления "материал" идентична настройке элементов управления по умолчанию.

Эффекты являются рекомендуемым методом, когда целью является Настройка существующего элемента управления. Если существует визуальный модуль подготовки материалов, то для настройки элемента управления с применением этого средства будет меньше работы, чем подкласс модуля подготовки отчетов. Дополнительные сведения о влиянии см. в статье [эффекты Xamarin. Forms](~/xamarin-forms/app-fundamentals/effects/index.md).

Пользовательские модули подготовки отчетов являются рекомендуемым методом, если модуль подготовки материалов не существует. В визуальный элемент Material включены следующие классы модуля подготовки отчетов:

- `MaterialButtonRenderer`
- `MaterialCheckBoxRenderer`
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

Подкласс модуля подготовки материалов практически идентичен модулям визуализации, не связанным с материалами. Однако при экспорте модуля подготовки отчетов, подклассов модуля подготовки отчетов, необходимо предоставить третий аргумент для атрибута `ExportRenderer`, который указывает тип `VisualMarker.MaterialVisual`:

```csharp
using Xamarin.Forms.Material.Android;

[assembly: ExportRenderer(typeof(ProgressBar), typeof(CustomMaterialProgressBarRenderer), new[] { typeof(VisualMarker.MaterialVisual) })]
namespace MyApp.Android
{
    public class CustomMaterialProgressBarRenderer : MaterialProgressBarRenderer
    {
        //...
    }
}
```

В этом примере `ExportRendererAttribute` указывает, что класс `CustomMaterialProgressBarRenderer` будет использоваться для отрисовки представления [`ProgressBar`](xref:Xamarin.Forms.ProgressBar) с типом `IVisual`, зарегистрированным в качестве третьего аргумента.

> [!NOTE]
> Модуль подготовки отчетов, указывающий тип `IVisual`, в составе его `ExportRendererAttribute`, будет использоваться для отрисовки включенных в представления, а не для модуля подготовки по умолчанию. Во время выбора модуля визуализации свойство `Visual` представления проверяется и включается в процесс выбора модуля подготовки отчетов.

Дополнительные сведения о пользовательских модулях подготовки отчетов см. в разделе [пользовательские модули подготовки](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)отчетов.

## <a name="related-links"></a>Связанные ссылки

- [Визуальный элемент "материал" (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-visualdemos)
- [Создание визуального модуля подготовки Xamarin. Forms](create.md)
- [Эффекты Xamarin. Forms](~/xamarin-forms/app-fundamentals/effects/index.md)
- [Пользовательские отрисовщики](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
