---
title: Настройка платформы WPF
description: Xamarin.Forms теперь имеет предварительную поддержку платформы WPF
ms.prod: xamarin
ms.assetid: 650723F2-4279-4B7B-B0A1-D7F8FF26BF1E
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 04/09/2020
ms.openlocfilehash: c9ec9fec2391d7d7a24f97f2ec20208a7d69dbc1
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/09/2020
ms.locfileid: "80992384"
---
# <a name="wpf-platform-setup"></a>Настройка платформы WPF

![Preview (Предварительный просмотр)](~/media/shared/preview.png)

Xamarin.Forms теперь имеет поддержку предварительного просмотра для Windows Презентация фонда (WPF). В этой статье показано, как добавить проект WPF в решение Xamarin.Forms.

> [!IMPORTANT]
> Поддержка WPF Xamarin.Forms предоставляется сообществом. Для получения дополнительной [Xamarin.Forms Platform Support](https://github.com/xamarin/Xamarin.Forms/wiki/Platform-Support)информации см.

Прежде чем начать, создайте новое решение Xamarin.Forms в Visual Studio 2019 или используйте существующее решение Xamarin.Forms, [**например, BoxViewClock.**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-boxviewclock) Приложения WPF можно добавлять только в решение Xamarin.Forms в Windows.

## <a name="add-a-wpf-application"></a>Добавление приложения WPF

Следуйте этим инструкциям, чтобы добавить приложение WPF, которое будет работать на Windows 7, 8 и 10 настольных компьютерах:

1. В Visual Studio 2019, право нажмите на имя решения в **Solution Explorer** и выбрать **Добавить > новый проект ...**.

2. В **добавлении нового** окна проекта, выберите **C-в** **Языках** падение вниз, выберите **Windows** в **платформах** падение, и выбрать **рабочий стол** в **типе проекта** падение вниз. В списке типов проектов выберите **приложение WPF (.NET Framework)**:

    ![Добавить новый проект WPF](wpf-images/add-project.png "Добавить новый проект WPF")

    Нажмите кнопку **«Следующая».**

3. В новом окне **проекта введите** название проекта с расширением **WPF,** например, **BoxViewClock.WPF.** Нажмите кнопку **«Просмотр»,** выберите папку **BoxViewClock** и нажмите **Select Folder,** чтобы поместить проект WPF в тот же каталог, что и другие проекты в решении:

    ![Добавить новый проект WPF](wpf-images/configure-project.png "Добавить новый проект WPF")

    Нажмите кнопку **«Создание»** для создания проекта.

4. В **Solution Explorer**, право нажмите на новый проект **BoxViewClock.WPF** и выберите **Управление NuGet пакеты ...**. Выберите вкладку **«Просмотр»** и ищите **Xamarin.Forms.Platform.WPF:**

    ![Выберите пакет NuGet](wpf-images/select-nuget-package.png "Выберите пакет NuGet")

    Выберите пакет и нажмите кнопку **«Установка».**

5. Право нажмите имя решения в **Solution Explorer** и выберите **Управление NuGet пакеты для решения ...**. Выберите вкладку **Обновления,** а затем выберите пакет **Xamarin.Forms.** Выберите все проекты и обновите их в одну и ту же версию Xamarin.Forms:

    ![Обновление пакета NuGet](wpf-images/update-nuget-package.png "Обновление пакета NuGet")

6. В проекте WPF, право ездуна на **ссылки** и выберите **Добавить Справка ...**. В диалоге **справочного менеджера** выберите **Проекты** слева и проверьте флажок, примыкающий к проекту **BoxViewClock:**

    ![Ссылка на общий проект](wpf-images/reference-shared-project.png "Ссылка на общий проект")

    Нажмите кнопку **OK.**

7. Оторите файл **MainWindow.xaml** проекта WPF. В `Window` теге добавьте декларацию XML namespace для сборки **Xamarin.Forms.Platform.WPF** и пространства имен:

    ```xaml
    xmlns:wpf="clr-namespace:Xamarin.Forms.Platform.WPF;assembly=Xamarin.Forms.Platform.WPF"
    ```

    Теперь `Window` измените `wpf:FormsApplicationPage`тег на . Измените `Title` настройку на имя приложения, например, **BoxViewClock.** Завершенный файл XAML должен выглядеть следующим образом:

    ```xaml
    <wpf:FormsApplicationPage x:Class="BoxViewClock.WPF.MainWindow"
            xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
            xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
            xmlns:local="clr-namespace:BoxViewClock.WPF"
            xmlns:wpf="clr-namespace:Xamarin.Forms.Platform.WPF;assembly=Xamarin.Forms.Platform.WPF"            
            mc:Ignorable="d"
            Title="BoxViewClock" Height="450" Width="800">
        <Grid>

        </Grid>
    </wpf:FormsApplicationPage>
    ```

8. Отсваивай **MainWindow.xaml.cs** файл проекта WPF. Добавьте `using` две новые директивы:

    ```csharp
    using Xamarin.Forms;
    using Xamarin.Forms.Platform.WPF;
    ```

    Изменение базового `MainWindow` класса `Window` `FormsApplicationPage`от . После `InitializeComponent` вызова добавьте следующие два заявления:

    ```csharp
    Forms.Init();
    LoadApplication(new BoxViewClock.App());
    ```

    За исключением комментариев и неиспользованных `using` директив, полный **MainWindows.xaml.cs** файл должен выглядеть следующим образом:

    ```csharp
    using Xamarin.Forms;
    using Xamarin.Forms.Platform.WPF;

    namespace BoxViewClock.WPF
    {
        public partial class MainWindow : FormsApplicationPage
        {
            public MainWindow()
            {
                InitializeComponent();

                Forms.Init();
                LoadApplication(new BoxViewClock.App());
            }
        }
    }
    ```

9. Нажмите правой кнопкой мыши на проект WPF в **Solution Explorer** и выберите Set в **качестве стартап-проекта.** Нажмите F5 для запуска программы с отладчиком Visual Studio на рабочем столе Windows:

    ![Часы WPF BoxView](wpf-images/wpf-boxviewclock.png "Часы WPF BoxView" )

## <a name="platform-specifics"></a>Специфика платформы

Вы можете определить, на какой платформе работает приложение Xamarin.Forms с помощью кода или XAML. Это позволяет изменять характеристики программы при запуске на WPF. В коде сравните `Device.RuntimePlatform` значение `Device.WPF` с константой (что равно строке "WPF"). Если есть совпадение, приложение работает на WPF.

В XAML можно использовать `OnPlatform` тег для выбора значения свойства, характерного для платформы:

```xaml
<Button.TextColor>
    <OnPlatform x:TypeArguments="Color">
        <On Platform="iOS" Value="White" />
        <On Platform="macOS" Value="White" />
        <On Platform="Android" Value="Black" />
        <On Platform="WPF" Value="Blue" />
    </OnPlatform>
</Button.TextColor>
```

## <a name="window-size"></a>Размер окна

Исходный размер окна можно настроить в файле WPF **MainWindow.xaml:**

```xaml
Title="BoxViewClock" Height="450" Width="800"
```

## <a name="issues"></a>Проблемы

Это предварительный просмотр, так что вы должны ожидать, что не все готово к производству. Не все пакеты NuGet для Xamarin.Forms готовы к WPF, и некоторые функции могут не работать в полной мере.

## <a name="related-video"></a>Связанные видео

> [!VIDEO https://youtube.com/embed/Fy9N6OSxK64]

**Видео поддержки Xamarin.Forms 3.0 WPF**
