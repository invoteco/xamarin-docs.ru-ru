---
title: Создание визуального модуля подготовки Xamarin. Forms
description: Создание визуальных элементов Xamarin. Forms для выборочного применения к объектам Висуалелемент без необходимости подклассировать представления Xamarin. Forms.
ms.prod: xamarin
ms.assetid: 80BF9C72-AC28-4AAF-9DDD-B60CBDD1CD59
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/12/2019
ms.openlocfilehash: bc95b9be0605c353ee9f914cb065f79711b9f92b
ms.sourcegitcommit: 41a029c69925e3a9d2de883751ebfd649e8747cd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/13/2019
ms.locfileid: "68978278"
---
# <a name="create-a-xamarinforms-visual-renderer"></a>Создание визуального модуля подготовки Xamarin. Forms

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-visualdemos)

С помощью Xamarin. Forms визуальные модули подготовки отчетов создаются и выборочно [`VisualElement`](xref:Xamarin.Forms.VisualElement) применяются к объектам без необходимости подклассировать представления Xamarin. Forms. Модуль подготовки отчетов, указывающий `IVisual` тип (как часть его `ExportRendererAttribute`), будет использоваться для визуализации входящих в представления, а не для модуля подготовки к просмотру по умолчанию. Во время выбора модуля подготовки отчетов `Visual` свойства представления проверен и включены в процесс выбора модуля подготовки отчетов.

> [!IMPORTANT]
> В настоящее время свойство не может быть изменено после подготовки представления, но оно будет изменено в следующем выпуске. [`Visual`](xref:Xamarin.Forms.VisualElement.Visual)

Процесс создания и использования визуального модуля подготовки Xamarin. Forms:

1. Создайте модули подготовки платформы для требуемого представления. Дополнительные сведения см. в разделе [Создание модулей подготовки](#create-platform-renderers)отчетов.
1. Создайте тип, производный от `IVisual`. Дополнительные сведения см. в разделе [Создание типа IVisual](#create-an-ivisual-type).
1. `ExportRendererAttribute` Зарегистрируйте `IVisual` тип как часть объекта, который будет оформлять модули подготовки отчетов. Дополнительные сведения см. [в статье регистрация типа IVisual](#register-the-ivisual-type).
1. Использовать визуальный модуль визуализации, задав [`Visual`](xref:Xamarin.Forms.VisualElement.Visual) `IVisual` для свойства представления имя. Дополнительные сведения см. [в разделе использование визуального модуля подготовки](#consume-the-visual-renderer)отчетов.
1. используемых Зарегистрируйте имя для `IVisual` типа. Дополнительные сведения см. [в статье регистрация имени для типа IVisual](#register-a-name-for-the-ivisual-type).

## <a name="create-platform-renderers"></a>Создание модулей подготовки платформы

Дополнительные сведения о создании класса модуля подготовки отчетов см. в разделе [пользовательские модули подготовки](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)отчетов. Однако обратите внимание, что визуальный модуль обработки Xamarin. Forms применяется к представлению без необходимости создавать подкласс представления.

Перечисленные здесь классы модуля подготовки отчетов реализуют пользовательский [`Button`](xref:Xamarin.Forms.Button) интерфейс, который отображает текст с тенью.

### <a name="ios"></a>iOS

В следующем примере кода показано средство визуализации кнопок для iOS:

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

В следующем примере кода показано средство визуализации кнопок для Android:

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

В межплатформенной библиотеке Создайте тип, производный от `IVisual`:

```csharp
public class CustomVisual : IVisual
{
}
```

Затем тип можно зарегистрировать в классах модуля подготовки отчетов, [`Button`](xref:Xamarin.Forms.Button) позволяя объектам использовать модули подготовки отчетов. `CustomVisual`

## <a name="register-the-ivisual-type"></a>Регистрация типа IVisual

В проектах платформы добавьте на `ExportRendererAttribute` уровне сборки:

```csharp
[assembly: ExportRenderer(typeof(Xamarin.Forms.Button), typeof(CustomButtonRenderer), new[] { typeof(CustomVisual) })]
namespace VisualDemos.iOS
{
    public class CustomButtonRenderer : ButtonRenderer
    {
        protected override void OnElementChanged(ElementChangedEventArgs<Button> e)
        {
            // ...
        }
    }
}
```

В этом примере для проекта платформы iOS `ExportRendererAttribute` параметр указывает `CustomButtonRenderer` , что класс будет [`Button`](xref:Xamarin.Forms.Button) использоваться для визуализации используемых объектов с `IVisual` типом, зарегистрированным в качестве третьего аргумента. Модуль подготовки отчетов, указывающий `IVisual` тип (как часть его `ExportRendererAttribute`), будет использоваться для визуализации входящих в представления, а не для модуля подготовки к просмотру по умолчанию.

## <a name="consume-the-visual-renderer"></a>Использование визуального модуля подготовки отчетов

Объект может использовать классы модуля подготовки отчетов, установив для `Custom`его [`Visual`](xref:Xamarin.Forms.VisualElement.Visual) свойства значение: [`Button`](xref:Xamarin.Forms.Button)

```xaml
<Button Visual="Custom"
        Text="CUSTOM BUTTON"
        BackgroundColor="{StaticResource PrimaryColor}"
        TextColor="{StaticResource SecondaryTextColor}"
        HorizontalOptions="FillAndExpand" />
```

> [!NOTE]
> В XAML преобразователь типов устраняет необходимость включения "визуального" суффикса в [`Visual`](xref:Xamarin.Forms.VisualElement.Visual) значение свойства. Однако можно также указать полное имя типа.

Эквивалентный код на C# выглядит так:

```csharp
Button button = new Button { Text = "CUSTOM BUTTON", ... };
button.Visual = new CustomVisual();
```

Во время [`Visual`](xref:Xamarin.Forms.VisualElement.Visual) выбора модуля визуализации свойство [`Button`](xref:Xamarin.Forms.Button) объекта проверяется и включается в процесс выбора модуля подготовки отчетов. Если модуль подготовки отчетов не размещается, будет использоваться модуль подготовки Xamarin. Forms по умолчанию.

На следующих снимках экрана показан [`Button`](xref:Xamarin.Forms.Button)отображаемый объект, который отображает текст с тенью:

[![Снимок экрана пользовательской кнопки с теневым текстом в iOS и Android](material-visual-images/custom-button.png "Кнопка с тенью текста")](material-visual-images/custom-button-large.png#lightbox)

## <a name="register-a-name-for-the-ivisual-type"></a>Регистрация имени для типа IVisual

Можно использовать для необязательной регистрации другого имени `IVisual` для типа. [`VisualAttribute`](xref:Xamarin.Forms.VisualAttribute) Этот подход можно использовать для разрешения конфликтов имен между различными визуальными библиотеками или в ситуациях, когда нужно просто ссылаться на визуальный элемент по имени, отличному от имени его типа.

[`VisualAttribute`](xref:Xamarin.Forms.VisualAttribute) Должен быть определен на уровне сборки в межплатформенной библиотеке или в проекте платформы:

```csharp
[assembly: Visual("MyVisual", typeof(CustomVisual))]
```

Затем `IVisual` тип можно использовать с помощью зарегистрированного имени:

```xaml
<Button Visual="MyVisual"
        ... />
```

> [!NOTE]
> При использовании визуального элемента с помощью его зарегистрированного имени необходимо добавить любой "визуальный" суффикс.

## <a name="related-links"></a>Связанные ссылки

- [Визуальный элемент "материал" (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-visualdemos)
- [Визуальный элемент материалов Xamarin. Forms](material-visual.md)
- [Пользовательские отрисовщики](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
