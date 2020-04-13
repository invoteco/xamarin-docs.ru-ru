---
title: Обнаружение темного режима в приложениях Xamarin.Forms
description: Темный режим может быть поддержан в любом приложении Xamarin.Forms, используя комбинацию resourceDictionaries, DynamicResources и знаний платформы.
ms.prod: xamarin
ms.assetid: D10506DD-BAA0-437F-A4AD-882D16E7B60D
ms.technology: xamarin-forms
author: davidortinau
ms.author: daortin
ms.date: 03/13/2020
ms.openlocfilehash: 7fe1a98e6a497a5791f26df2fc96d41781b71ef6
ms.sourcegitcommit: b93754b220fca3d6e3d131341e3cfbe233d10f84
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/03/2020
ms.locfileid: "80628314"
---
# <a name="detect-dark-mode-in-xamarinforms-applications"></a>Обнаружение темного режима в приложениях Xamarin.Forms

Приложения Xamarin.Forms могут реагировать на изменения темы операционной системы, используя ту же стратегию, которая позволяет поддерживать [тематику.](theming.md) Основное различие заключается в том, как срабатывает смена темы. Темный режим относится к более широкому набору предпочтений внешнего вида, которые могут быть установлены на уровне операционной системы, и какие приложения должны уважать и реагировать на немедленно.

Минимальные требования к использованию темного режима в приложениях Xamarin.Forms:

- iOS 13 или больше.
- Android 9 или больше (Android 9 поддерживает режимы внешнего вида, которые вы можете заделать).
- Android 10 или больше (Android 10 уведомляет приложения об изменениях режима).
- UWP v10.0.10240.0 или больше.
- Xamarin.Forms (любая версия).

Процесс поддержки режимов внешнего вида, включая светлый и темный, заключается в следующем:

1. Определите ресурсы, совместно [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)распределенные по всему приложению в .
2. Определите тему словаря ресурсов для каждого режима внешнего вида, который предоставляет переопределения, специфичные для каждого стиля, который вы хотите изменить.
3. Установите тему режима внешнего вида по умолчанию в файле **App.xaml** приложения.
4. Стиль приложения с `DynamicResource` помощью расширения разметки, где вы хотите, чтобы стили реагировали при изменении режимов внешнего вида.
5. Добавьте код для прослушивания изменений темы ОС и загрузите соответствующую тему приложения.

Следующие скриншоты показывают тематические страницы, для светлого и темного режима:

[![Скриншот главной страницы тематизированного приложения, на iOS и Android](theming-images/main-page-both-themes.png "Главная страница тематизированного приложения")](theming-images/main-page-both-themes-large.png#lightbox "Главная страница тематизированного приложения")
[![Скриншот подробной страницы тематизированного приложения, на iOS и Android](theming-images/detail-page-both-themes.png "Подробная страница тематизированного приложения")](theming-images/detail-page-both-themes-large.png#lightbox "Подробная страница тематизированного приложения")

## <a name="define-themes"></a>Определить темы

Следуйте [тематические руководства](theming.md) для шаг за шагом подробности о создании темных и светлых тем.

Вы можете легко установить новую тему для вашего приложения в соответствующем месте кода платформы. Чтобы загрузить новую тему, просто замените текущий словарь ресурсов приложения тематическим словарем ресурсов по вашему выбору:

```csharp
App.Current.Resources = new YourDarkTheme();
```

## <a name="detect-and-apply-theme"></a>Обнаружение и применение темы

Обнаружение в настоящее время запущенной [`RequestedTheme`](~/essentials/app-theme.md) темы может быть достигнуто с помощью функции [Xamarin.Essentials](~/essentials/index.md) (версия 1.4.0 или новее). Затем можно создать метод помощника в новом классе `App` или в классе для настройки темы:

```csharp
public partial class App : Application
{
    public static void ApplyTheme()
    {
        if (AppInfo.RequestedTheme == AppTheme.Dark)
        {
            // Change to dark theme
            // e.g. App.Current.Resources = new YourDarkTheme();
        }
        else
        {
            // Change to light theme
            // e.g. App.Current.Resources = new YourLightTheme();
        }
    }
}
```

## <a name="react-to-appearance-mode-changes"></a>Реагировать на изменения режима видимости

Режим внешнего вида на устройстве может меняться по целому ряду причин в зависимости от того, как пользователь настроил свои предпочтения, включая явное выбор режима, времени суток или факторов окружающей среды, таких как низкая освещенность. Вам придется добавить код платформы, чтобы убедиться, что приложение может реагировать на эти изменения, и следующие разделы обсуждают это более подробно.

### <a name="android"></a>Android

Для поддержки темного режима в приложении необходимо обновить тему вашего приложения, которая может быть найдена в, `Resources/values/styles.xml`чтобы унаследовать от темы: `DayNight`

```xml
<style name="MainTheme.Base" parent="Theme.AppCompat.DayNight">
```

Если вы обновили [компоненты](https://www.nuget.org/packages/Xamarin.Google.Android.Material/) AndroidX (1.1.0-rc2) или новее, вы можете использовать:

```xml
<style name="MainTheme.Base" parent="Theme.MaterialComponents.DayNight">
```

В **MainActivity.cs** файле приложения добавьте `ConfigChanges.UiMode` поле `ConfigurationChanges` в `Activity` свойство в атрибуте, чтобы ваше приложение было уведомлено об изменениях режима uI:

```csharp
[Activity(
    // other settings
    ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation | ConfigChanges.UiMode)]
```

В том же **файле** `OnConfigurationChanged` MainActivity.cs переопределить метод:

```csharp
public override void OnConfigurationChanged(Configuration newConfig)
{
    base.OnConfigurationChanged(newConfig);
    App.ApplyTheme();
}
```

### <a name="ios"></a>iOS

На iOS изменения режима внешнего вида уведомляются на UIViewController, `ContentPage`который в Xamarin.Forms является . Для того, чтобы переопределить этот метод, создайте пользовательский рендерватор в проекте iOS под названием: `PageRenderer.cs`

```csharp
using System;
using UIKit;
using Xamarin.Forms;
using Xamarin.Forms.Platform.iOS;

[assembly: ExportRenderer(typeof(ContentPage), typeof(YourApp.iOS.Renderers.PageRenderer))]
namespace YourApp.iOS.Renderers
{
    public class PageRenderer : Xamarin.Forms.Platform.iOS.PageRenderer
    {
        protected override void OnElementChanged(VisualElementChangedEventArgs e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null || Element == null)
            {
                return;
            }

            try
            {
                App.ApplyTheme();
            }
            catch (Exception ex)
            {
                System.Diagnostics.Debug.WriteLine($"\t\t\tERROR: {ex.Message}");
            }
        }

        public override void TraitCollectionDidChange(UITraitCollection previousTraitCollection)
        {
            base.TraitCollectionDidChange(previousTraitCollection);

            App.ApplyTheme();
        }
    }
}
```

Преодоление `TraitCollectionDidChange` метода позволяет действовать `UserInterfaceStyle` на изменение.

### <a name="uwp"></a>UWP

На UWP добавьте следующий код в **MainPage.xaml.cs** файл приложения:

```csharp
public sealed partial class MainPage
{
    UISettings uiSettings;

    public MainPage()
    {
        this.InitializeComponent();

        LoadApplication(new YourApp.App());

        uiSettings = new UISettings();
        uiSettings.ColorValuesChanged += ColorValuesChanged;
    }

    private void ColorValuesChanged(UISettings sender, object args)
    {
        Xamarin.Essentials.MainThread.BeginInvokeOnMainThread(() =>
        {
            App.ApplyTheme();
        });
    }
}
```

## <a name="related-links"></a>Связанные ссылки

- [Тематика (образец)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-theming/)
- [Словари ресурсов](~/xamarin-forms/xaml/resource-dictionaries.md)
- [Динамические стили в Xamarin.Forms](~/xamarin-forms/user-interface/styles/xaml/dynamic.md)
- [Задание стиля приложений Xamarin.Forms с помощью стилей XAML](~/xamarin-forms/user-interface/styles/xaml/index.md)
