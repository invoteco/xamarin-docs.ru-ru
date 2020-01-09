---
title: Создание первого приложения Xamarin.Forms
description: Видеоруководство, показывающее, как создать первое приложение Xamarin.Forms в Visual Studio.
zone_pivot_groups: platform-dev16
ms.prod: xamarin
ms.assetid: 72B6AF82-4D98-47E5-AB54-0A35B3253468
ms.technology: xamarin-forms
ms.custom: video
author: conceptdev
ms.author: crdun
ms.date: 05/23/2019
ms.openlocfilehash: fd2fcf6ebe11df27444f2ecc1d89955debf56cb4
ms.sourcegitcommit: c4f72221a6dce1276a90f2b52282b8145f8e0f1c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502804"
---
# <a name="build-your-first-xamarinforms-app"></a>Создание первого приложения Xamarin.Forms

_Просмотрите это видео и следуйте инструкциям, чтобы создать свое первое приложение для мобильных устройств с помощью Xamarin.Forms._

::: zone pivot="windows"

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Build-Your-First-Android-App-with-Visual-Studio-2019-and-Xamarin/player]

## <a name="step-by-step-instructions-for-windows"></a>Пошаговые инструкции для Windows

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-firstapp/)

Выполните следующие действия, как показано в видео.

1. Выберите **файл > создать > проект...** или нажмите кнопку **создать новый проект..** .:

    [![создать новый проект](images/win-2019/01-sml.png)](images/win-2019/01.png#lightbox)

2. Выполните поиск по фразе Xamarin или выберите **Mobile** в меню **тип проекта** . Выберите тип проекта " **мобильное приложение (Xamarin. Forms)** ":

    [Фильтр ![для проектов Xamarin](images/win-2019/02-sml.png)](images/win-2019/02.png#lightbox)

3. Выберите имя проекта &ndash; в примере используется "Авесомеапп":

    [![выбрать имя проекта](images/win-2019/03-sml.png)](images/win-2019/03.png#lightbox)

4. Щелкните **пустой** тип проекта и убедитесь, что выбраны **Android** и **iOS** .

    [![Android и iOS с .NET Standard](images/win-2019/04-sml.png)](images/win-2019/04.png#lightbox)

5. Подождите, пока восстанавливаются пакеты NuGet (в строке состояния появится сообщение "Восстановление завершено").

6. В новых установках Visual Studio 2019 не будет настроен эмулятор Android. Щелкните стрелку раскрывающегося списка на кнопке **Отладка** и выберите **создать Android Emulator** , чтобы запустить экран создания эмулятора:

    ![Раскрывающийся список создания Android Emulator](images/win-2019/debug-dropdown.png)

7. На экране создания эмулятора используйте параметры по умолчанию и нажмите кнопку **создать** :

    [экран создания эмулятора ![Android](images/win-2019/create-emulator-sml.png)](images/win-2019/create-emulator.png#lightbox)

8. Создание эмулятора вернет вас в окно Device Manager. Нажмите кнопку " **Пуск** ", чтобы запустить новый эмулятор:

    ![Эмулятор Android в Device Manager](images/win-2019/start-emulator.png)

9. Visual Studio 2019 теперь должен показывать имя нового эмулятора на кнопке " **Отладка** ":

    ![Имя эмулятора Android на кнопке "Отладка"](images/win-2019/debug-emulator-name.png)

10. Нажмите кнопку " **Отладка** ", чтобы создать и развернуть приложение в эмуляторе Android:

    ![Эмулятор Android, отображающий приложение](images/win-2019/android-emulator.png)

## <a name="customize-the-application"></a>Настройка приложения

Приложение можно настроить для добавления интерактивных функций. Чтобы добавить взаимодействие с пользователем в приложение, выполните следующие действия.

1. Измените **MainPage.xaml**, добавив этот XAML до конца `</StackLayout>`:

    ```xaml
    <Button Text="Click Me" Clicked="Button_Clicked" />
    ```

2. Измените **MainPage.xaml**, добавив этот код до конца класса:

    ```csharp
    int count = 0;
    void Button_Clicked(object sender, System.EventArgs e)
    {
        count++;
        ((Button)sender).Text = $"You clicked {count} times.";
    }
    ```

3. Отладка приложения на Android:

    ![Приложение Android](images/win/07-sml.png)

> [!NOTE]
> Пример приложения включает дополнительные интерактивные функции, которые не рассматриваются в видео.

## <a name="build-an-ios-app-in-visual-studio-2019"></a>Создание приложения iOS в Visual Studio 2019

Можно создать и отладить приложение iOS из Visual Studio с помощью сетевого компьютера Mac. Дополнительные сведения см. в [инструкциях по установке](~/ios/get-started/installation/windows/index.md).

В этом видеоролике рассматривается процесс создания и тестирования приложения iOS с помощью Visual Studio 2019 в Windows.

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Build-Your-First-iOS-App-with-Visual-Studio-2019-and-Xamarin/player]

::: zone-end
::: zone pivot="win-vs2017"

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Building-Your-First-Android--iOS-App-in-Visual-Studio-2017/player]

## <a name="step-by-step-instructions-for-windows"></a>Пошаговые инструкции для Windows

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-firstapp/)

Выполните следующие действия, как показано в видео.

1. Выберите **Файл > Создать > Проект...** или нажмите кнопку **Создать проект...** , а затем выберите **Visual C# > Кроссплатформенные > Мобильное приложение (Xamarin.Forms)** :

    [![Мобильное приложение (Xamarin.Forms)](images/win/01-sml.png)](images/win/01.png#lightbox)

2. Убедитесь, что выбраны **Android** и **iOS** с совместным использованием кода **.NET Standard**:

    [![Android и iOS с .NET Standard](images/win/02-sml.png)](images/win/02.png#lightbox)

3. Подождите, пока восстанавливаются пакеты NuGet (в строке состояния появится сообщение "Восстановление завершено").

4. Запустите эмулятор Android, нажав кнопку отладки (или пункт меню **Отладка > Начать отладку**).

5. Измените **MainPage.xaml**, добавив этот XAML до конца `</StackLayout>`:

    ```xaml
    <Button Text="Click Me" Clicked="Button_Clicked" />
    ```

6. Измените **MainPage.xaml**, добавив этот код до конца класса:

    ```csharp
    int count = 0;
    void Button_Clicked(object sender, System.EventArgs e)
    {
        count++;
        ((Button)sender).Text = $"You clicked {count} times.";
    }
    ```

7. Отладка приложения на Android:

    ![Приложение Android](images/win/07-sml.png)

    > [!TIP]
    > Можно создавать и отлаживать приложения iOS из Visual Studio с компьютером Mac, подключенным к сети. Дополнительные сведения см. в [инструкциях по установке](~/ios/get-started/installation/windows/index.md).

::: zone-end
::: zone pivot="macos"

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Building-Your-First-iOS--Android-App-in-Visual-Studio-for-Mac/player]

## <a name="step-by-step-instructions-for-mac"></a>Пошаговые инструкции для Mac

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-firstapp/)

Выполните следующие действия, как показано в видео.

1. Выберите **Файл > Создать решение...** или нажмите кнопку **Создать проект...** и выберите **Многоплатформенность > Приложение > Приложение с пустыми формами**:

    [![Приложение с пустыми формами](images/01-sml.png)](images/01.png#lightbox)

2. Убедитесь, что выбраны **Android** и **iOS** с совместным использованием кода **.NET Standard**:

    [![Android и iOS с .NET Standard](images/02-sml.png)](images/02.png#lightbox)

3. Восстановите пакеты NuGet, щелкнув правой кнопкой решение:

    ![Приложение Android](images/03-sml.png)

4. Запустите эмулятор Android, нажав кнопку отладки (или **Запуск > Начать отладку**).

5. Измените **MainPage.xaml**, добавив этот XAML до конца `</StackLayout>`:

    ```xaml
    <Button Text="Click Me" Clicked="Handle_Clicked" />
    ```

6. Измените **MainPage.xaml**, добавив этот код до конца класса:

    ```csharp
    int count = 0;
    void Handle_Clicked(object sender, System.EventArgs e)
    {
        count++;
        ((Button)sender).Text = $"You clicked {count} times.";
    }
    ```

7. Отладка приложения на Android:

    ![Приложение Android](images/07-sml.png)

8. Щелкните правой кнопкой мыши, чтобы настроить iOS на **Запускаемый проект**:

    [![Задание запускаемого проекта в iOS](images/08-sml.png)](images/08.png#lightbox)

9. Отладка приложения в iOS:

    ![Приложение iOS](images/09-sml.png)

::: zone-end

Загрузите полный код из [коллекции примеров](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-firstapp/) или просмотрите его на [GitHub](https://github.com/xamarin/xamarin-forms-samples/tree/master/GetStarted/FirstApp).

## <a name="next-steps"></a>Дальнейшие действия

- [Краткое руководство по отдельным страницам](~/get-started/quickstarts/single-page.md) &ndash; создания более функционального приложения.
- [Примеры Xamarin. forms](~/xamarin-forms/samples/index.md) &ndash; Скачайте и запустите примеры кода и примеры приложений.
- [Создание электронной документации по мобильным приложениям](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md) &ndash; подробные главы, посвященные разработке Xamarin. Forms, доступной в формате PDF и включающей сотни дополнительных примеров.
