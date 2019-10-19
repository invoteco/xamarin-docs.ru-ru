---
title: Кадр Xamarin. Forms
description: Класс фрейма Xamarin. Forms — это макет, используемый для создания оболочки представления или макета с границей, которая может быть настроена с помощью цвета, тени и других параметров.
ms.prod: xamarin
ms.assetId: 4E074714-0928-41C8-A468-B60E23236A8C
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 08/06/2019
ms.openlocfilehash: 619b29a9d65594b1badd805c3361fe1a174d7174
ms.sourcegitcommit: dad4dfcd194b63ec9e903363351b6d9e543d4888
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/18/2019
ms.locfileid: "69976502"
---
# <a name="xamarinforms-frame"></a>Кадр Xamarin. Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-frame/)

Класс [`Frame`](xref:Xamarin.Forms.Frame) Xamarin. Forms — это макет, используемый для создания оболочки представления с границей, которые можно настроить с помощью цвета, тени и других параметров. Фреймы обычно используются для создания границ вокруг элементов управления, но их можно использовать для создания более сложного пользовательского интерфейса. Дополнительные сведения см. в разделе [Расширенное использование кадров](#advanced-frame-usage).

На следующем снимке экрана показаны `Frame` элементы управления в iOS и Android:

[![ "примеры кадров в iOS и Android"](frame-images/frame-cropped.png)](frame-images/frame-full.png#lightbox "Примеры кадров в iOS и Android")

Класс `Frame` определяет следующие свойства:

* [`BorderColor`](xref:Xamarin.Forms.Frame.BorderColor) — это `Color` значение, определяющее цвет границы `Frame`.
* [`CornerRadius`](xref:Xamarin.Forms.Frame.CornerRadius) — это `float` значение, определяющее закругленный радиус угла.
* [`HasShadow`](xref:Xamarin.Forms.Frame.HasShadow) — это `bool` значение, которое определяет, содержит ли кадр тень.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами, что означает, что `Frame` может быть целевым объектом привязок данных.

> [!NOTE]
> Поведение свойства `HasShadow` зависит от платформы. Значение по умолчанию — `true` на всех платформах. Однако на тенях тени UWP не отображаются. Тени отображаются в Android и iOS, но тени на iOS темнее и занимают больше места.

## <a name="create-a-frame"></a>Создание рамки

В XAML можно создать экземпляр `Frame`. Объект `Frame` по умолчанию имеет белый фон, тень тени и без границы. Объект `Frame` обычно заключает в оболочку другой элемент управления. В следующем примере показано `Frame` по умолчанию оболочкой для объекта `Label`.

```xaml
<Frame>
  <Label Text="Example" />
</Frame>
```

@No__t_0 также можно создать в коде:

```csharp
Frame defaultFrame = new Frame
{
    Content = new Label { Text = "Example" }
};
```

`Frame` объекты можно настраивать с помощью скругленных углов, цветных границ и теней, задавая свойства в XAML. В следующем примере показан настраиваемый объект `Frame`.

```xaml
<Frame BorderColor="Orange"
       CornerRadius="10"
       HasShadow="True">
  <Label Text="Example" />
</Frame>
```

Эти свойства экземпляра также можно задать в коде:

```csharp
Frame frame = new Frame
{
    BorderColor = Color.Orange,
    CornerRadius = 10,
    HasShadow = true,
    Content = new Label { Text = "Example" }
};
```

## <a name="advanced-frame-usage"></a>Расширенное использование кадров

Класс `Frame` наследует от `ContentView`, что означает, что он может содержать любой тип объекта `View`, включая `Layout` объекты. Эта возможность позволяет использовать `Frame` для создания сложных объектов пользовательского интерфейса, таких как карты.

### <a name="create-a-card-with-a-frame"></a>Создание карточки с рамкой

Объединение объекта `Frame` с объектом `Layout`, таким как объект `StackLayout`, позволяет создавать более сложный пользовательский интерфейс. На следующем снимке экрана показан пример карточки, созданной с помощью объекта `Frame`:

[Снимок экрана с картой, созданной с помощью кадра "!["](frame-images/frame-card-cropped.png)](frame-images/frame-full.png#lightbox "Снимок экрана Карты, созданной с помощью кадра")

В следующем коде XAML показано, как создать карточку с помощью класса `Frame`:

```xaml
<Frame BorderColor="Gray"
       CornerRadius="5"
       Padding="8">
  <StackLayout>
    <Label Text="Card Example"
           FontSize="Medium"
           FontAttributes="Bold" />
    <BoxView Color="Gray"
             HeightRequest="2"
             HorizontalOptions="Fill" />
    <Label Text="Frames can wrap more complex layouts to create more complex UI components, such as this card!"/>
  </StackLayout>
</Frame>
```

Карту также можно создать в коде:

```csharp
Frame cardFrame = new Frame
{
    BorderColor = Color.Gray,
    CornerRadius = 5,
    Padding = 8,
    Content = new StackLayout
    {
        Children =
        {
            new Label
            {
                Text = "Card Example",
                FontSize = Device.GetNamedSize(NamedSize.Medium, typeof(Label)),
                FontAttributes = FontAttributes.Bold
            },
            new BoxView
            {
                Color = Color.Gray,
                HeightRequest = 2,
                HorizontalOptions = LayoutOptions.Fill
            },
            new Label
            {
                Text = "Frames can wrap more complex layouts to create more complex UI components, such as this card!"
            }
        }
    }
};
```

### <a name="round-elements"></a>Элементы Round

Свойство `CornerRadius` элемента управления `Frame` можно использовать для создания кругового изображения. На следующем снимке экрана показан пример кругового изображения, созданного с помощью объекта `Frame`:

[Снимок экрана с изображением круга, созданным с помощью кадра "!["](frame-images/circle-image-cropped.png)](frame-images/frame-full.png#lightbox "Снимок экрана с изображением круга, созданным с помощью рамки")

В следующем коде XAML показано, как создать изображение круга на языке XAML:

```xaml
<Frame Margin="10"
       BorderColor="Black"
       CornerRadius="50"
       HeightRequest="60"
       WidthRequest="60"
       IsClippedToBounds="True"
       HorizontalOptions="Center"
       VerticalOptions="Center">
  <Image Source="outdoors.jpg"
         Aspect="AspectFill"
         Margin="-20"
         HeightRequest="100"
         WidthRequest="100" />
</Frame>
```

Изображение в виде круга также можно создать в коде:

```csharp
Frame circleImageFrame = new Frame
{
    Margin = 10,
    BorderColor = Color.Black,
    CornerRadius = 50,
    HeightRequest = 60,
    WidthRequest = 60,
    IsClippedToBounds = true,
    HorizontalOptions = LayoutOptions.Center,
    VerticalOptions = LayoutOptions.Center,
    Content = new Image
    {
        Source = ImageSource.FromFile("outdoors.jpg"),
        Aspect = Aspect.AspectFill,
        Margin = -20,
        HeightRequest = 100,
        WidthRequest = 100
    }
};
```

Образ **туризм. jpg** должен быть добавлен в каждый проект платформы, и как это достигается в зависимости от платформы. Дополнительные сведения см. [в разделе изображения в Xamarin. Forms](~/xamarin-forms/user-interface/images.md).

> [!NOTE]
> Скругленные углы ведут себя немного иначе на разных платформах. @No__t_1 объекта `Image` должен быть половиной разницы между шириной изображения и шириной родительского фрейма и должно быть отрицательным для центрирования изображения равномерно в пределах объекта `Frame`. Однако запрошенные ширина и высота не гарантированы, поэтому `Margin`, `HeightRequest` и `WidthRequest` свойства могут быть изменены в зависимости от размера изображения и других вариантов расположения.

## <a name="related-links"></a>Связанные ссылки

* [Демонстрации кадров](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-frame/)
* [Изображения в Xamarin. Forms](~/xamarin-forms/user-interface/images.md)
