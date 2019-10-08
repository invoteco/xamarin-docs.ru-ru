---
title: Установка платформы WPF
description: Xamarin. Forms теперь имеет поддержку предварительной версии для платформы WPF
ms.prod: xamarin
ms.assetid: 650723F2-4279-4B7B-B0A1-D7F8FF26BF1E
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 04/05/2018
ms.openlocfilehash: 38d9b42b3a29ea46d05a1d1cc4e38641d2445786
ms.sourcegitcommit: 4cf434b126eb7df6b2fd9bb1d71613bf2b6aac0e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/07/2019
ms.locfileid: "71997174"
---
# <a name="wpf-platform-setup"></a>Установка платформы WPF

![Предварительный просмотр](~/media/shared/preview.png)

Xamarin. Forms теперь имеет поддержку предварительной версии для Windows Presentation Foundation (WPF). В этой статье показано, как добавить проект WPF в решение Xamarin. Forms.

> [!IMPORTANT]
> Поддержка Xamarin. Forms для WPF предоставляется сообществом. Дополнительные сведения см. в статье [Поддержка платформы в Xamarin. Forms](https://github.com/xamarin/Xamarin.Forms/wiki/Platform-Support).

Прежде чем начать, создайте новое решение Xamarin. Forms в Visual Studio 2019 или используйте существующее решение Xamarin. Forms, например [**боксвиевклокк**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-boxviewclock). Приложения WPF можно добавлять только в решение Xamarin. Forms в Windows.

## <a name="add-a-wpf-project-to-a-xamarinforms-app-with-xamarinuniversity"></a>Добавление проекта WPF в приложение Xamarin. Forms с помощью Xamarin. Университет

> [!VIDEO https://youtube.com/embed/Fy9N6OSxK64]

**Видео о поддержке WPF Xamarin. Forms 3,0**

## <a name="adding-a-wpf-app"></a>Добавление приложения WPF

Выполните эти инструкции, чтобы добавить приложение WPF, которое будет выполняться на настольных компьютерах под управлением Windows 7, 8 и 10:

1. В Visual Studio 2019 щелкните правой кнопкой мыши имя решения в **Обозреватель решений** и выберите **Добавить > новый проект...** .

2. В окне **Новый проект** слева выберите **Visual C#**  и **классический рабочий стол Windows**. В списке типов проектов выберите **приложение WPF (.NET Framework)** .

3. Введите имя проекта с расширением **WPF** , например **боксвиевклокк. WPF**. Нажмите кнопку **Обзор** , выберите папку **боксвиевклокк** и нажмите кнопку **выбрать папку**. Проект WPF будет размещен в том же каталоге, что и другие проекты в решении.

    ![Добавление нового проекта WPF](wpf-images/add-new-project.png "Добавление нового проекта WPF")

    Нажмите кнопку ОК, чтобы создать проект.

4. В **Обозреватель решений**щелкните правой кнопкой мыши новый проект **боксвиевклокк. WPF** и выберите пункт **Управление пакетами NuGet**. Перейдите на вкладку **Обзор** , установите флажок **включить предварительные выпуски** и найдите **Xamarin. Forms**.

    ![Выбор пакета NuGet](wpf-images/select-nuget-package.png "Выбор пакета NuGet")

    Выберите этот пакет и нажмите кнопку " **установить** ".

5. Теперь найдите пакет **Xamarin. Forms. Platform. WPF** и установите его. Убедитесь, что пакет находится в корпорации Майкрософт.

6. Щелкните правой кнопкой мыши имя решения в **Обозреватель решений** и выберите пункт **Управление пакетами NuGet для решения**. Выберите вкладку **Обновление** и пакет **Xamarin. Forms** . Выберите все проекты и обновите их до одной версии Xamarin. Forms:

    ![Обновление пакета]NuGet(wpf-images/update-nuget-package.png "Обновление пакета NuGet")

7. В проекте WPF щелкните правой кнопкой мыши **ссылки**. В диалоговом окне **Диспетчер ссылок** выберите **проекты** слева и установите флажок рядом с проектом **боксвиевклокк** :

    ![Ссылка на общий проект](wpf-images/reference-shared-project.png "ссылки") на общий проект

8. Измените файл **MainWindow. XAML** проекта WPF. В теге `Window` добавьте объявление пространства имен XML для сборки и пространства имен **Xamarin. Forms. Platform. WPF** :

    ```xaml
    xmlns:wpf="clr-namespace:Xamarin.Forms.Platform.WPF;assembly=Xamarin.Forms.Platform.WPF"
    ```

    Теперь измените тег `Window` на `wpf:FormsApplicationPage`. Измените значение параметра `Title` на имя приложения, например **боксвиевклокк**. Завершенный файл XAML должен выглядеть следующим образом:

    ```xaml
    <wpf:FormsApplicationPage x:Class="BoxViewClock.WPF.MainWindow"
            xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
            xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
            xmlns:wpf="clr-namespace:Xamarin.Forms.Platform.WPF;assembly=Xamarin.Forms.Platform.WPF"
            xmlns:local="clr-namespace:BoxViewClock.WPF"
            mc:Ignorable="d"
            Title="BoxViewClock" Height="450" Width="800">
        <Grid>

        </Grid>
    </wpf:FormsApplicationPage>
    ```

9. Измените файл **MainWindow.XAML.CS** проекта WPF. Добавьте две новые директивы `using`:

    ```csharp
    using Xamarin.Forms;
    using Xamarin.Forms.Platform.WPF;
    ```

    Измените базовый класс `MainWindow` с `Window` на `FormsApplicationPage`. После вызова `InitializeComponent` добавьте следующие две инструкции:

    ```csharp
    Forms.Init();
    LoadApplication(new BoxViewClock.App());
    ```

    За исключением комментариев и неиспользуемых директив `using`, полный файл **MainWindows.XAML.CS** должен выглядеть следующим образом:

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

10. Щелкните правой кнопкой мыши проект WPF в **Обозреватель решений** и выберите **Назначить запускаемым проектом**. Нажмите клавишу F5, чтобы запустить программу с помощью отладчика Visual Studio на рабочем столе Windows:

    Часы ![боксвиев]WPF(wpf-images/wpf-boxviewclock.png "боксвиев часов" )

## <a name="next-steps"></a>Следующие шаги

### <a name="platform-specifics"></a>Особенности платформы

Вы можете определить, на какой платформе выполняется приложение Xamarin. Forms, из кода или XAML. Это позволяет изменять характеристики программы при выполнении в WPF. В коде Сравните значение `Device.RuntimePlatform` с константой `Device.WPF` (которая равна строке "WPF"). Если найдено совпадение, приложение работает в WPF.

В XAML можно использовать тег `OnPlatform`, чтобы выбрать значение свойства, относящееся к платформе:

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

### <a name="window-size"></a>Размер окна

Начальный размер окна можно скорректировать в файле WPF **MainWindow. XAML** :

```xaml
Title="BoxViewClock" Height="450" Width="800"
```

## <a name="issues"></a>Вопросы

Это предварительная версия, поэтому следует рассчитывать на то, что все готово к производству. Не все пакеты NuGet для Xamarin. Forms готовы для WPF, а некоторые функции могут быть не полностью работоспособными.
