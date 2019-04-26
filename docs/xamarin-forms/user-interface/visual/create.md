---
title: Создайте в модуль подготовки Xamarin.Forms Visual
description: Создание визуальных элементов Xamarin.Forms, чтобы выборочно применять к объектам VisualElement без необходимости подкласс представления Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 80BF9C72-AC28-4AAF-9DDD-B60CBDD1CD59
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/12/2019
ms.openlocfilehash: a11c2045fa6119d0689834c35794bc8913c80bd6
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "61023778"
---
# <a name="create-a-xamarinforms-visual-renderer"></a>Создайте в модуль подготовки Xamarin.Forms Visual

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/VisualDemos/)

Xamarin.Forms Visual включает модули подготовки отчетов, будет создан и выборочно применять к [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) объектов без необходимости подкласс представления Xamarin.Forms. Модуль подготовки отчетов, который указывает `IVisual` тип как часть его `ExportRendererAttribute`, будет использоваться для подготовки к просмотру, использующим представления, а не стандартный модуль подготовки отчетов. Во время выбора модуля подготовки отчетов `Visual` свойства представления проверен и включены в процесс выбора модуля подготовки отчетов.

> [!IMPORTANT]
> В настоящее время [ `Visual` ](xref:Xamarin.Forms.VisualElement.Visual) свойство нельзя изменить после завершения отображения представления, но это приведет к изменению в будущих выпусках.

Процесс создания и использования в модуль подготовки Xamarin.Forms Visual не:

1. Создание платформы модули подготовки отчетов для необходимого представления. Дополнительные сведения см. в разделе [создание модулей подготовки отчетов](#create-platform-renderers).
1. Создать тип, который является производным от `IVisual`. Дополнительные сведения см. в разделе [создать тип IVisual](#create-an-ivisual-type).
1. Зарегистрировать `IVisual` тип как часть `ExportRendererAttribute` который оформляет модулей подготовки отчетов. Дополнительные сведения см. в разделе [зарегистрировать тип IVisual](#register-the-ivisual-type).
1. Использовать Visual модуля подготовки отчетов, задав [ `Visual` ](xref:Xamarin.Forms.VisualElement.Visual) свойства в представлении, чтобы `IVisual` имя. Дополнительные сведения см. в разделе [использовать средства визуализации компоненту Visual](#consume-the-visual-renderer).
1. [необязательно] Регистрация имени для `IVisual` типа. Дополнительные сведения см. в разделе [регистрация имени для типа IVisual](#register-a-name-for-the-ivisual-type).

## <a name="create-platform-renderers"></a>Создание платформы модули подготовки отчетов

Сведения о создании класса визуализации, см. в разделе [пользовательские Отрисовщики](~/xamarin-forms/app-fundamentals/custom-renderer/index.md). Тем не менее Обратите внимание на то, что в модуль подготовки Xamarin.Forms Visual применяется к представлению без необходимости подкласс представления.

Создание пользовательского модуля подготовки отчетов классов, описанную в этой статье [ `Button` ](xref:Xamarin.Forms.Button) , отображающий текст с тенью.

### <a name="ios"></a>iOS

В следующем примере кода показано, модуль подготовки отчетов кнопки для iOS:

```csharp
public class CustomButtonRenderer : ButtonRenderer
{
    protected override void OnElementChanged(ElementChangedEventArgs<Button> e)
    {
        base.OnElementChanged(e);

        if (e.OldElement != null)
        {
            // Cleanup
        }

        if (e.NewElement != null)
        {
            Control.TitleShadowOffset = new CoreGraphics.CGSize(1, 1);
            Control.SetTitleShadowColor(Color.Black.ToUIColor(), UIKit.UIControlState.Normal);
        }
    }
}
```

### <a name="android"></a>Android

В следующем примере кода показано, модуль подготовки отчетов кнопки для Android:

```csharp
public class CustomButtonRenderer : Xamarin.Forms.Platform.Android.AppCompat.ButtonRenderer
{
    public CustomButtonRenderer(Context context) : base(context)
    {
    }

    protected override void OnElementChanged(ElementChangedEventArgs<Button> e)
    {
        base.OnElementChanged(e);

        if (e.OldElement != null)
        {
            // Cleanup
        }

        if (e.NewElement != null)
        {
            Control.SetShadowLayer(5, 3, 3, Color.Black.ToAndroid());
        }
    }
}
```

## <a name="create-an-ivisual-type"></a>Создание типа IVisual

В library кроссплатформенная создать тип, который является производным от `IVisual`:

```csharp
public class CustomVisual : IVisual
{
}
```

`CustomVisual` Типа, можно зарегистрировать для классов отрисовки, разрешая [ `Button` ](xref:Xamarin.Forms.Button) объектов можно согласиться на использование модулей подготовки отчетов.

## <a name="register-the-ivisual-type"></a>Зарегистрировать тип IVisual

В проектах платформы, снабдить классов отрисовки с `ExportRendererAttribute`:

```csharp
[assembly: ExportRenderer(typeof(Xamarin.Forms.Button), typeof(CustomButtonRenderer), new[] { typeof(CustomVisual) })]
public class CustomButtonRenderer : ButtonRenderer
{
    protected override void OnElementChanged(ElementChangedEventArgs<Button> e)
    {
        ...
    }
}
```

В этом примере `ExportRendererAttribute` указывает, что `CustomButtonRenderer` класс будет использоваться для подготовки к просмотру использование [ `Button` ](xref:Xamarin.Forms.Button) объектов, с помощью `IVisual` тип зарегистрирован в качестве третьего аргумента. Модуль подготовки отчетов, который указывает `IVisual` тип как часть его `ExportRendererAttribute`, будет использоваться для подготовки к просмотру, использующим представления, а не стандартный модуль подготовки отчетов.

## <a name="consume-the-visual-renderer"></a>Использование Visual модуля подготовки отчетов

Объект [ `Button` ](xref:Xamarin.Forms.Button) объекта, можно выбрать использование классов отрисовки, установив его [ `Visual` ](xref:Xamarin.Forms.VisualElement.Visual) свойства `Custom`:

```xaml
<Button Visual="Custom"
        Text="CUSTOM BUTTON"
        BackgroundColor="{StaticResource PrimaryColor}"
        TextColor="{StaticResource SecondaryTextColor}"
        HorizontalOptions="FillAndExpand" />
```

> [!NOTE]
> Преобразователь типов в XAML, избавляет от необходимости включать «Visual» суффикс в [ `Visual` ](xref:Xamarin.Forms.VisualElement.Visual) значение свойства. Тем не менее можно также указать полное имя типа.

Ниже приведен аналогичный код C#:

```csharp
Button button = new Button { Text = "CUSTOM BUTTON", ... };
button.Visual = new CustomVisual();
```

Во время выбора модуля подготовки отчетов [ `Visual` ](xref:Xamarin.Forms.VisualElement.Visual) свойство [ `Button` ](xref:Xamarin.Forms.Button) проверен и включены в процесс выбора модуля подготовки отчетов. Если модуль подготовки отчетов не находится, будет использоваться модуль подготовки отчетов по умолчанию Xamarin.Forms.

Далее на снимках экрана показано создаваемые [ `Button` ](xref:Xamarin.Forms.Button), который отображает текст с тенью:

[![Снимок экрана настраиваемой кнопки с текстом тени, iOS и Android](material-visual-images/custom-button.png "кнопка с текстовой тени")](material-visual-images/custom-button-large.png#lightbox)

## <a name="register-a-name-for-the-ivisual-type"></a>Регистрация имени для типа IVisual

[ `VisualAttribute` ](xref:Xamarin.Forms.VisualAttribute) Можно использовать при необходимости зарегистрировать другое имя для `IVisual` типа. Этот подход используется для разрешения конфликтов имен между различных библиотеках Visual или в ситуациях, где нужно ссылаться на визуальный элемент с именем, отличным от имени типа.

[ `VisualAttribute` ](xref:Xamarin.Forms.VisualAttribute) Должны быть определены на уровне сборки в кросс платформенной библиотекой или в проект платформы:

```csharp
[assembly: Visual("MyVisual", typeof(CustomVisual))]
```

`IVisual` Тип затем может быть востребован зарегистрированного имени:

```xaml
<Button Visual="MyVisual"
        ... />
```

> [!NOTE]
> При использовании визуального помощью зарегистрированного имени, любые суффиксы «Visual» должен быть включен.

## <a name="related-links"></a>Связанные ссылки

- [Материалы Visual (пример)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/VisualDemos/)
- [Материал Xamarin.Forms Visual](material-visual.md)
- [Пользовательские отрисовщики](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
