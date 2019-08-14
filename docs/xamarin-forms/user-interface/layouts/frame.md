---
title: Кадр Xamarin. Forms
description: Класс фрейма Xamarin. Forms — это макет, используемый для создания оболочки представления или макета с границей, которая может быть настроена с помощью цвета, тени и других параметров.
ms.prod: xamarin
ms.assetId: 4E074714-0928-41C8-A468-B60E23236A8C
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 08/06/2019
ms.openlocfilehash: 329edf455a4778efa5f6fbd1ec88cfb274c5cb49
ms.sourcegitcommit: 41a029c69925e3a9d2de883751ebfd649e8747cd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/13/2019
ms.locfileid: "68984426"
---
# <a name="xamarinforms-frame"></a>Кадр Xamarin. Forms

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/en-us/samples/xamarin/xamarin-forms-samples/userinterface-frame/)

Класс Xamarin. Forms [`Frame`](xref:Xamarin.Forms.Frame) — это макет, используемый для создания оболочки представления с границей, которые можно настроить с помощью цвета, тени и других параметров. Фреймы обычно используются для создания границ вокруг элементов управления, но их можно использовать для создания более сложного пользовательского интерфейса. Дополнительные сведения см. в разделе [Расширенное использование кадров](#advanced-frame-usage).

На следующем снимке `Frame` экрана показаны элементы управления iOS и Android.

Примеры кадров ["примеры кадров в iOS и Android" в iOS и Android ![](frame-images/frame-cropped.png)](frame-images/frame-full.png#lightbox "")

`Frame` Класс определяет следующие свойства:

* [`BorderColor`](xref:Xamarin.Forms.Frame.BorderColor)значение, определяющее цвет `Frame` границы. `Color`
* [`CornerRadius`](xref:Xamarin.Forms.Frame.CornerRadius)`float` значение, определяющее закругленный радиус угла.
* [`HasShadow`](xref:Xamarin.Forms.Frame.HasShadow)`bool` значение, определяющее, содержит ли кадр тень.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами, что означает, `Frame` что может быть целевым объектом привязок данных.

> [!NOTE]
> Поведение `HasShadow` свойства зависит от платформы. Значение по умолчанию `true` — на всех платформах. Однако на тенях тени UWP не отображаются. Тени отображаются в Android и iOS, но тени на iOS темнее и занимают больше места.

## <a name="create-a-frame"></a>Создание рамки

Экземпляр `Frame` можно создать в XAML. Объект по `Frame` умолчанию имеет белый фон, тень и без границы. Объект `Frame` обычно заключает в оболочку другой элемент управления. В следующем примере показан `Frame` `Label` перенос объекта по умолчанию.

```xaml
<Frame>
  <Label Text="Example" />
</Frame>
```

Также `Frame` можно создать в коде:

```csharp
Frame defaultFrame = new Frame
{
    Content = new Label { Text = "Example" }
};
```

`Frame`объекты можно настраивать с помощью скругленных углов, цветных границ и теней, задавая свойства в XAML. В следующем примере показан настраиваемый `Frame` объект:

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

Класс наследует от `ContentView`, что означает, что он `View` может содержать любой тип объекта, `Layout` включая объекты. `Frame` Эта возможность позволяет `Frame` использовать для создания сложных объектов пользовательского интерфейса, таких как карты.

### <a name="create-a-card-with-a-frame"></a>Создание карточки с рамкой

Объединение объекта с объектом, таким как `StackLayout` объект, позволяет создать более сложный пользовательский интерфейс. `Layout` `Frame` На следующем снимке экрана показан пример карточки, созданной `Frame` с помощью объекта:

Снимок экрана [с картой, созданной на снимке экрана, созданной с помощью кадра ![](frame-images/frame-card-cropped.png)](frame-images/frame-full.png#lightbox "")

В следующем коде XAML показано, как создать карточку с `Frame` классом:

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

`CornerRadius` Свойство`Frame` элемента управления можно использовать для создания кругового изображения. На следующем снимке экрана показан пример кругового изображения, созданного с `Frame` помощью объекта:

Снимок экрана с изображением круга, [созданным с помощью кадра "изображение круга, созданного с помощью рамки ![](frame-images/circle-image-cropped.png)](frame-images/frame-full.png#lightbox "")

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
> Скругленные углы ведут себя немного иначе на разных платформах. Объект должен быть половиной разницы между шириной изображения и шириной родительского фрейма и должен быть отрицательным для центрирования `Frame` изображения равномерно в пределах объекта. `Margin` `Image` Однако запрошенные ширина и высота не гарантированы, поэтому свойства `Margin` `HeightRequest` и `WidthRequest` могут потребоваться изменить в зависимости от размера изображения и других вариантов макета.

## <a name="related-links"></a>Связанные ссылки

* [Демонстрации кадров](https://docs.microsoft.com/en-us/samples/xamarin/xamarin-forms-samples/userinterface-frame/)
* [Изображения в Xamarin. Forms](~/xamarin-forms/user-interface/images.md)
