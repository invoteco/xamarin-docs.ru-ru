---
title: Материал Xamarin.Forms Visual
description: Visual материал Xamarin.Forms можно использовать для создания приложений Xamarin.Forms, которые выглядят идентичными или мало чем отличается в iOS и Android.
ms.prod: xamarin
ms.assetid: B774F68C-EF9E-49E1-B738-CDC64879ADA2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/12/2019
ms.openlocfilehash: cf6ab8266b0798ccbf29078313bbc7454125a1af
ms.sourcegitcommit: 97dca3face7c4ad5555dfaca88f5b45a70ca556d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/14/2019
ms.locfileid: "57972628"
---
# <a name="xamarinforms-material-visual"></a>Материал Xamarin.Forms Visual

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/VisualDemos/)

[Материальное](https://material.io) — это система признанная проектирования, разработанную компанией Google, которое предписывает размер, цвет, интервалы и другие аспекты как представления и макеты вид и поведение.

Visual материал Xamarin.Forms можно применять правила материальному дизайну для приложений Xamarin.Forms, создание приложений, которые выглядят идентичными или мало чем отличается в iOS и Android. При включении Visual материал, поддерживаемых представлений принимают разработки различных платформах, создание единой внешнего вида и поведения. Это достигается с помощью материалов модули подготовки отчетов, применить правила материальному дизайну.

Процесс включения Xamarin.Forms материал Visual в приложении является:

1. Добавить [Xamarin.Forms.Visual.Material](https://www.nuget.org/packages/Xamarin.Forms.Visual.Material/) пакет NuGet для iOS и Android платформы проектов. Этот пакет NuGet предоставляет оптимизированные материальному дизайну модулей подготовки отчетов в iOS и Android. В iOS, пакет предоставляет транзитивной зависимости для [Xamarin.iOS.MaterialComponents](https://www.nuget.org/packages/Xamarin.iOS.MaterialComponents), который является C# привязка к Google [компоненты материал для iOS](https://material.io/develop/ios/). В Android пакет предоставляет целевые объекты сборки, чтобы убедиться, что ваш TargetFramework правильно настроена.
1. Инициализируйте материала модулей подготовки отчетов в каждом проекте платформы. Дополнительные сведения см. в разделе [инициализировать материала модулей подготовки отчетов](#initialize-material-renderers).
1. Использовать материалы модулей подготовки отчетов, задав [ `Visual` ](xref:Xamarin.Forms.VisualElement.Visual) свойства `Material` на все страницы, рекомендуется применять правила материальному дизайну. Дополнительные сведения см. в разделе [потреблять материала модулей подготовки отчетов](#consume-material-renderers).
1. [необязательно] Настройка материала модулей подготовки отчетов. Дополнительные сведения см. в разделе [настроить материала модулей подготовки отчетов](#customize-material-renderers).

> [!IMPORTANT]
> В Android материала модулей подготовки отчетов требуется Минимальная версия 5.0 (API 21) или более поздней версии и TargetFramework версии 9.0 (API 28). Кроме того, платформа проекта необходим библиотеки поддержки Android 28.0.0 или более поздней версии, и его темы должен наследовать от темы материал компонентов или наследовать от AppCompat темы по-прежнему. Дополнительные сведения см. в разделе [Приступая к работе с компонентами материал для Android](https://github.com/material-components/material-components-android/blob/master/docs/getting-started.md).

Материала модулей подготовки отчетов в настоящее время включены в [Xamarin.Forms.Visual.Material](https://www.nuget.org/packages/Xamarin.Forms.Visual.Material/) пакет NuGet для следующих представлений:

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

## <a name="initialize-material-renderers"></a>Инициализировать материала модулей подготовки отчетов

После установки [Xamarin.Forms.Visual.Material](https://www.nuget.org/packages/Xamarin.Forms.Visual.Material/) пакетов NuGet, материал, модули подготовки отчетов, которые должны быть инициализированы в каждый проект платформы.

В iOS, это должно произойти через **AppDelegate.cs** путем вызова `FormsMaterial.Init` метод *после* `Xamarin.Forms.Forms.Init` метод:

```csharp
global::Xamarin.Forms.Forms.Init();
FormsMaterial.Init();
```

В Android, это должно произойти через **MainActivity.cs** путем вызова `FormsMaterial.Init` метод *после* `Xamarin.Forms.Forms.Init` метод:

```csharp
global::Xamarin.Forms.Forms.Init(this, savedInstanceState);
FormsMaterial.Init(this, savedInstanceState);
```

## <a name="consume-material-renderers"></a>Использовать материалы модулей подготовки отчетов

Приложения, можно выбрать использование материала модулей подготовки отчетов, установив [ `VisualElement.Visual` ](xref:Xamarin.Forms.VisualElement.Visual) свойство на странице, макета или представления, чтобы `Material`:

```xaml
<ContentPage Visual="Material"
             ...>
    ...
</ContentPage>
```

Ниже приведен аналогичный код C#:

```csharp
ContentPage contentPage = new ContentPage();
contentPage.Visual = VisualMarker.Material;
```

[ `Visual` ](xref:Xamarin.Forms.VisualElement.Visual) Свойству можно присвоить любой тип, реализующий `IVisual`, с помощью [ `VisualMarker` ](xref:Xamarin.Forms.VisualMarker) класс, предоставляющий следующие `IVisual` свойства:

- `Default` — Указывает, что представление должны отображаться с помощью модуля подготовки отчетов по умолчанию.
- `MatchParent` — Указывает, что представление следует использовать же модуля подготовки отчетов с родительским direct.
- `Material` — Указывает, что представление должны отображаться с помощью материалов модуля подготовки отчетов.

> [!IMPORTANT]
> [ `Visual` ](xref:Xamarin.Forms.VisualElement.Visual) Свойство определено в [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) класс с представлениями наследование `Visual` значение свойства от их родительских объектов. Таким образом, задание `Visual` свойство [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) гарантирует, что все поддерживаемые представления на странице будет использовать этот визуальный. Кроме того `Visual` свойство можно переопределить для представления.

На следующих снимках экрана показано пользовательского интерфейса к просмотру с использованием модулей подготовки отчетов по умолчанию:

[![Снимок экрана модулей подготовки отчетов по умолчанию, в iOS и Android](material-visual-images/default-renderers.png "представлениями с использованием модулей подготовки отчетов по умолчанию")](material-visual-images/default-renderers-large.png#lightbox)

На следующих снимках экрана показано тот же пользовательский интерфейс, с помощью материалов модулей подготовки отчетов к просмотру:

[![Снимок экрана материала модулей подготовки отчетов, в iOS и Android](material-visual-images/material-renderers.png "представлениями с использованием материала модулей подготовки отчетов")](material-visual-images/material-renderers-large.png#lightbox)

Перечислены основные различия видны между модулями подготовки отчетов по умолчанию и материала модулей подготовки отчетов, показанный здесь, что материала визуализаторов начинайте с прописной буквы [ `Button` ](xref:Xamarin.Forms.Button) текст и скругления углов [ `Frame` ](xref:Xamarin.Forms.Frame)границы. Однако материала модулей подготовки отчетов использует собственные элементы управления, и таким образом могут по-прежнему существовать отличия в пользовательском интерфейсе между платформами сферах, таких как шрифты, теней, цвета и повышение прав.

> [!NOTE]
> Компоненты материала близко соответствуют рекомендациям по от Google. Таким образом модули подготовки отчетов материальному дизайну смещаются в сторону, размера и поведение. Когда требуется больший контроль стили или поведение, можно создавать собственные [эффект](~/xamarin-forms/app-fundamentals/effects/index.md), [поведение](~/xamarin-forms/app-fundamentals/behaviors/index.md), или [пользовательского средства визуализации](~/xamarin-forms/app-fundamentals/custom-renderer/index.md) для достижения детализации, вам требуется.

## <a name="customize-material-renderers"></a>Настройка материала модулей подготовки отчетов

Материала модулей подготовки отчетов можно при необходимости настроить, так же, как модули подготовки отчетов по умолчанию посредством следующих базовых классов:

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

В этом примере `ExportRendererAttribute` указывает, что `CustomMaterialProgressBarRenderer` класс будет использоваться для подготовки к просмотру [ `ProgressBar` ](xref:Xamarin.Forms.ProgressBar) представление, с помощью `IVisual` тип зарегистрирован в качестве третьего аргумента.

> [!NOTE]
> Модуль подготовки отчетов, который указывает `IVisual` тип как часть его `ExportRendererAttribute`, будет использоваться для подготовки к просмотру, использующим представления, а не стандартный модуль подготовки отчетов. Во время выбора модуля подготовки отчетов `Visual` свойства представления проверен и включены в процесс выбора модуля подготовки отчетов.

Дополнительные сведения о настраиваемых отрисовщиках см. в разделе [пользовательские Отрисовщики](~/xamarin-forms/app-fundamentals/custom-renderer/index.md).

## <a name="related-links"></a>Связанные ссылки

- [Материалы Visual (пример)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/VisualDemos/)
- [Создайте в модуль подготовки Xamarin.Forms Visual](create.md)
- [Пользовательские отрисовщики](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
