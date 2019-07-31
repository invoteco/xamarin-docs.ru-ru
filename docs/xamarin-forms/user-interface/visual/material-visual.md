---
title: Визуальный элемент материалов Xamarin. Forms
description: Визуальный элемент "материалы Xamarin. Forms" можно использовать для создания приложений Xamarin. Forms, которые выглядят одинаково или в значительной степени идентичны в iOS и Android.
ms.prod: xamarin
ms.assetid: B774F68C-EF9E-49E1-B738-CDC64879ADA2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/12/2019
ms.openlocfilehash: 6c529c8df7ef1e4372285a157f489941d795d7f6
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/30/2019
ms.locfileid: "68655225"
---
# <a name="xamarinforms-material-visual"></a>Визуальный элемент материалов Xamarin. Forms

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-visualdemos)

[Проектирование материалов](https://material.io) — это упрямого система разработки, созданная Google, которая предписывает размер, цвет, расстояния и другие аспекты того, как должны выглядеть и работать представления и макеты.

Визуальный элемент "материалы по Xamarin. Forms" можно использовать для применения правил проектирования материалов к приложениям Xamarin. Forms, создавая приложения, которые выглядят одинаково или в значительной степени идентичны в iOS и Android. Если визуальный элемент "материалы" включен, Поддерживаемые представления применяют один и тот же проект кросс-платформенный режим, создавая единообразный внешний вид. Это достигается с помощью модулей подготовки материалов, которые применяют правила проектирования материалов.

Процесс включения визуального элемента "материалы" в Xamarin. Forms в приложении:

1. Добавьте пакет NuGet [Xamarin. Forms. Visual. Material](https://www.nuget.org/packages/Xamarin.Forms.Visual.Material/) в проекты iOS и Android Platform. Этот пакет NuGet предоставляет оптимизированные для разработки материалов модули подготовки отчетов в iOS и Android. В iOS пакет предоставляет транзитивное зависимость для [Xamarin. iOS. материалкомпонентс](https://www.nuget.org/packages/Xamarin.iOS.MaterialComponents), которая является C# привязкой к [компонентам материалов Google для iOS](https://material.io/develop/ios/). В Android пакет предоставляет целевые объекты сборки, чтобы обеспечить правильную настройку TargetFramework.
1. Инициализируйте модули подготовки материалов в каждом проекте платформы. Дополнительные сведения см. в разделе Инициализация модулей [подготовки](#initialize-material-renderers)отчетов.
1. [`Visual`](xref:Xamarin.Forms.VisualElement.Visual) Чтобы`Material` использовать модули подготовки материалов, присвоив свойству значение на всех страницах, которые должны применять правила дизайна материалов. Дополнительные сведения см. в разделе [Использование модулей подготовки](#consume-material-renderers)отчетов к просмотру.
1. используемых Настройте модули подготовки материалов. Дополнительные сведения см. в разделе [Настройка модулей подготовки](#customize-material-renderers)отчетов к просмотру.

> [!IMPORTANT]
> В Android для модулей подготовки материалов требуется минимальная версия 5,0 (API 21) или более поздней версии, а также TargetFramework версии 9,0 (API 28). Кроме того, проект платформы требует наличия библиотек поддержки Android 28.0.0 или более поздней версии, и его тема должна наследовать от темы "компоненты материала" или продолжить наследовать от темы AppCompat. Дополнительные сведения см. в разделе [Приступая к работе с компонентами материал для Android](https://github.com/material-components/material-components-android/blob/master/docs/getting-started.md).

Модули подготовки материалов в настоящее время включены в пакет [Xamarin. Forms. Visual. Material](https://www.nuget.org/packages/Xamarin.Forms.Visual.Material/) NuGet для следующих представлений:

- [`Button`](xref:Xamarin.Forms.Button)
- `CheckBox`
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

## <a name="initialize-material-renderers"></a>Инициализация модулей подготовки материалов

После установки пакета NuGet для [Xamarin. Forms. Visual. Material](https://www.nuget.org/packages/Xamarin.Forms.Visual.Material/) модули подготовки материалов должны быть инициализированы в каждом проекте платформы.

В iOS это должно произойти в **AppDelegate.CS** путем вызова `FormsMaterial.Init` метода *после* `Xamarin.Forms.Forms.Init` метода:

```csharp
global::Xamarin.Forms.Forms.Init();
FormsMaterial.Init();
```

В Android это должно произойти в **MainActivity.CS** путем вызова `FormsMaterial.Init` метода *после* `Xamarin.Forms.Forms.Init` метода:

```csharp
global::Xamarin.Forms.Forms.Init(this, savedInstanceState);
FormsMaterial.Init(this, savedInstanceState);
```

## <a name="consume-material-renderers"></a>Использование модулей подготовки материалов

Приложения могут отказаться от использования модулей подготовки материалов, установив [`VisualElement.Visual`](xref:Xamarin.Forms.VisualElement.Visual) свойство на странице, макете или представлении, чтобы: `Material`

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

Свойству можно задать любой тип, реализующий `IVisual`, с [`VisualMarker`](xref:Xamarin.Forms.VisualMarker) классом, который предоставляет следующие `IVisual` свойства: [`Visual`](xref:Xamarin.Forms.VisualElement.Visual)

- `Default`— Указывает, что представление должно отображаться с помощью модуля подготовки отчетов по умолчанию.
- `MatchParent`— Указывает, что представление должно использовать тот же модуль подготовки отчетов, что и его непосредственного родителя.
- `Material`— Указывает, что представление должно быть отображено с помощью модуля подготовки материалов.

> [!IMPORTANT]
> Свойство определяется в классе с представлениями, которые наследуют значение свойстваотсвоихродителей.`Visual` [`VisualElement`](xref:Xamarin.Forms.VisualElement) [`Visual`](xref:Xamarin.Forms.VisualElement.Visual) Таким образом, установка `Visual` свойства для гарантирует [`ContentPage`](xref:Xamarin.Forms.ContentPage) , что все поддерживаемые представления на странице будут использовать этот визуальный элемент. Кроме того `Visual` свойство можно переопределить для представления.

На следующих снимках экрана показан пользовательский интерфейс, отображаемый с помощью модулей подготовки отчетов по умолчанию:

[![Снимок экрана модулей подготовки отчетов по умолчанию в iOS и Android](material-visual-images/default-renderers.png "Представления, использующие модули подготовки отчетов по умолчанию")](material-visual-images/default-renderers-large.png#lightbox)

На следующих снимках экрана показано тот же пользовательский интерфейс, с помощью материалов модулей подготовки отчетов к просмотру:

[![Снимок экрана модулей подготовки материалов в iOS и Android](material-visual-images/material-renderers.png "Представления, использующие модули подготовки материалов")](material-visual-images/material-renderers-large.png#lightbox)

Основные видимые различия между модулями подготовки отчетов по умолчанию и модулями подготовки материалов, показанными здесь, заключаются [`Button`](xref:Xamarin.Forms.Button) в том, что модули подготовки отчетов [`Frame`](xref:Xamarin.Forms.Frame) записывают текст и округляют углы границ. Однако модули подготовки материалов используют собственные элементы управления, поэтому для таких областей, как шрифты, тени, цвета и повышения прав, могут существовать различия в пользовательском интерфейсе.

> [!NOTE]
> Компоненты проектирования материалов тесно соответствуют рекомендациям Google. В результате визуализаторы дизайна материалов перемещаются в сторону изменения размера и поведения. Если требуется больший контроль над стилями или поведением, можно по-прежнему создать собственный [результат](~/xamarin-forms/app-fundamentals/effects/index.md), [поведение](~/xamarin-forms/app-fundamentals/behaviors/index.md)или [пользовательский модуль подготовки](~/xamarin-forms/app-fundamentals/custom-renderer/index.md) отчетов для получения необходимых сведений.

## <a name="customize-material-renderers"></a>Настройка модулей подготовки материалов

Модули подготовки материалов, как и модули подготовки отчетов по умолчанию, могут быть настроены с помощью следующих базовых классов:

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

В следующем примере кода показан пример настройки `MaterialProgressBarRenderer` класса:

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

В этом примере `ExportRendererAttribute` объект указывает `CustomMaterialProgressBarRenderer` , что класс будет использоваться для отрисовки [`ProgressBar`](xref:Xamarin.Forms.ProgressBar) представления с `IVisual` типом, зарегистрированным в качестве третьего аргумента.

> [!NOTE]
> Модуль подготовки отчетов, указывающий `IVisual` тип (как часть его `ExportRendererAttribute`), будет использоваться для визуализации входящих в представления, а не для модуля подготовки к просмотру по умолчанию. Во время выбора модуля подготовки отчетов `Visual` свойства представления проверен и включены в процесс выбора модуля подготовки отчетов.

Дополнительные сведения о пользовательских модулях подготовки отчетов см. в разделе [пользовательские модули подготовки](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)отчетов.

## <a name="related-links"></a>Связанные ссылки

- [Визуальный элемент "материал" (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-visualdemos)
- [Создание визуального модуля подготовки Xamarin. Forms](create.md)
- [Пользовательские отрисовщики](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
