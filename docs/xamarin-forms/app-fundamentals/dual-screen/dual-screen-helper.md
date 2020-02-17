---
title: DualScreenHelper в Xamarin.Forms
description: В этом руководстве рассматривается использование элемента DualScreenHelper из Xamarin.Forms для оптимизации интерфейса приложения на двухэкранных устройствах, таких как Surface Duo и Surface Neo.
ms.prod: xamarin
ms.assetid: 5aa184c2-5611-427d-85c7-1c56486c3e1b
ms.technology: xamarin-forms
author: davidortinau
ms.author: daortin
ms.date: 02/08/2020
ms.openlocfilehash: b06e105560de635a21b42e8366bb47842372cf6a
ms.sourcegitcommit: 07941cf9704ff88cf4087de5ebdea623ff54edb1
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/11/2020
ms.locfileid: "77145548"
---
# <a name="xamarinforms-dualscreenhelper"></a>DualScreenHelper в Xamarin.Forms
[![Скачать пример](~/media/shared/download.png) Скачать пример](https://github.com/xamarin/xamarin-forms-samples/UserInterface/DualScreenDemos)
_DualScreenHelper позволяет определить, поддерживает ли устройство изображение в режиме "картинка в картинке", а затем открыть ContentPage в окне, использующем такой режим. Если вы используете устройство Neo, то в режиме создания страница откроется на панели WonderBar._
## <a name="properties"></a>Свойства
- `HasCompactModeSupport` используется для проверки, поддерживает ли платформа открытие ContentPage в режиме CompactMode.
- `OpenCompactMode` открывает ContentPage в режиме CompactMode, если платформа это поддерживает.
## <a name="example-usage"></a>Пример использования
```c#
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