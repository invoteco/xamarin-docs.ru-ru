---
title: Создайте в модуль подготовки Xamarin.Forms Visual
description: Xamarin.Forms Visual включает модули подготовки отчетов, чтобы выборочно применять к объектам VisualElement без необходимости элементов управления Xamarin.Forms подкласс.
ms.prod: xamarin
ms.assetid: 80BF9C72-AC28-4AAF-9DDD-B60CBDD1CD59
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/05/2019
ms.openlocfilehash: 3c614bcd0044a0aa4a698c830d2f2af5aba6c65a
ms.sourcegitcommit: 00744f754527e5b55154365f89691caaf1c9d929
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/07/2019
ms.locfileid: "57557516"
---
# <a name="xamarinforms-visual"></a>Xamarin.Forms Visual

Xamarin.Forms Visual включает модули подготовки отчетов, чтобы выборочно применять к `VisualElement` объекты, не прибегая к элементам управления Xamarin.Forms подкласс. Все модули подготовки отчетов, укажите `VisualType` как часть `ExportRendererAttribute` будет использоваться для визуализации представления, а не модулями подготовки отчетов по умолчанию. Во время выбора модуля подготовки отчетов `Visual` свойства представления проверен и включены в процесс выбора модуля подготовки отчетов.

> [!IMPORTANT]
> В настоящее время `Visual` нельзя изменить после завершения отображения элемента управления, но это приведет к изменению в будущих выпусках.

Платформа Xamarin.Forms включает внешний вид, в зависимости от материального исполнения. Дополнительные сведения см. в разделе [Xamarin.Forms материал Visual](material-visual.md).

## <a name="create-a-visual"></a>Создание визуального элемента

В library кроссплатформенная создать тип, который является производным от `IVisual`:

```csharp
public class CustomVisual : IVisual
{
}
```

## <a name="register-the-visual-type"></a>Зарегистрировать тип визуального элемента

Создать визуализатор для необходимого представления и зарегистрировать тип визуального элемента как часть платформы `ExportRenderAttribute`:

```csharp
using Xamarin.Forms.Material.Android;

[assembly: ExportRenderer(typeof(ContentPage), typeof(CustomPageRenderer), new[] { typeof(CustomVisual) })]
namespace MyApp.Android
{
    public class CustomPageRenderer : PageRenderer
    {
        ...
    }
}
```

## <a name="consume-the-visual"></a>Использование визуального элемента

Приложения, можно выбрать с помощью пользовательского визуального элемента через `Visual` свойство:

```xaml
<ContentPage ...
             Visual="Custom">
    ...
</ContentPage>
```

Ниже приведен аналогичный код C#:

```csharp
ContentPage contentPage = new ContentPage();
contentPage.Visual = new CustomVisual();
```

## <a name="register-a-name-for-a-visual"></a>Регистрация имени для визуального элемента

Возможны конфликты между различных библиотеках Visual или возможно, необходимы для ссылки на визуальный элемент с другим именем. В этом случае можно использовать атрибут сборки для регистрации другое имя для данного визуального элемента:

```csharp
[assembly: Visual("MyMaterialName", typeof(VisualMarker.MaterialVisual))]
```

Пользовательский визуальный элемент может быть востребован зарегистрированного имени:

```xaml
<ContentPage ...
             Visual="MyMaterialName">
    ...
</ContentPage>
````

## <a name="related-links"></a>Связанные ссылки

- [Материал Xamarin.Forms Visual](material-visual.md)
- [Пользовательские отрисовщики](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
