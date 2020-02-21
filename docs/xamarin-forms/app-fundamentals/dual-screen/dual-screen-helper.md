---
title: DualScreenHelper в Xamarin.Forms
description: В этом руководстве рассматривается использование класса DualScreenHelper из Xamarin.Forms для оптимизации интерфейса приложения на двухэкранных устройствах, таких как Surface Duo и Surface Neo.
ms.prod: xamarin
ms.assetid: 5aa184c2-5611-427d-85c7-1c56486c3e1b
ms.technology: xamarin-forms
author: davidortinau
ms.author: daortin
ms.date: 02/08/2020
ms.openlocfilehash: 26e6389dc7effd52fad8307e1e3191fc4b760fe6
ms.sourcegitcommit: 524fc148bad17272bda83c50775771daa45bfd7e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/20/2020
ms.locfileid: "77480570"
---
# <a name="xamarinforms-dualscreenhelper"></a>DualScreenHelper в Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://github.com/xamarin/xamarin-forms-samples/UserInterface/DualScreenDemos)

Класс `DualScreenHelper` позволяет определить, поддерживает ли устройство режим "картинка в картинке", и открыть `ContentPage` в окне, использующем такой режим. Если вы используете устройство Neo, то в режиме создания страница откроется на панели WonderBar.

## <a name="properties"></a>Свойства

- `HasCompactModeSupport` используется для проверки того, поддерживает ли платформа открытие `ContentPage` в режиме CompactMode.
- `OpenCompactMode` открывает `ContentPage` в режиме CompactMode, если поддерживается платформой.

## <a name="example"></a>Пример

```csharp
async void OpenCompactWindowClicked(object sender, EventArgs e)
{
    if(!DualScreenHelper.HasCompactModeSupport())
    {
        await DisplayAlert("Unsupported", "This platform doesn't support this feature", "Ok");
        return;
    }

    ContentPage page = new ContentPage() { BackgroundColor = Color.Purple };

    var button = new Button()
    {
        Text = "Close this Window"
    };

    var layout = new StackLayout()
    {
        Children =
        {
            new Label(){ Text = "Welcome to your Compact Mode Window" },
            button
        },
        BackgroundColor = Color.Yellow,
        HorizontalOptions = LayoutOptions.Fill,
        VerticalOptions = LayoutOptions.Fill
    };

    page.Content = new ScrollView() { Content = layout };
    var args = await DualScreenHelper.OpenCompactMode(page);

    button.Command = new Command(async () =>
    {
        await args.CloseAsync();
    });
}
```
