---
ms.openlocfilehash: 8ad5ca60a074cbdc6ff91134cc9c1276ed653b91
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/09/2020
ms.locfileid: "67277423"
---
# <a name="visual-studio"></a>[Visual Studio](#tab/vswin)

Для работы с этим руководством у вас должен быть последний выпуск Visual Studio 2019 с установленной рабочей нагрузкой **Разработка мобильных приложений на .NET**. Кроме того, вам потребуется компьютер Mac для сборки учебного приложения на iOS. Сведения об установке платформы Xamarin см. в статье [Установка Xamarin](~/get-started/installation/index.md). Сведения о подключении Visual Studio 2019 к узлу сборки Mac см. в статье [Связывание с Mac при разработке для Xamarin.iOS](~/ios/get-started/installation/windows/connecting-to-mac/index.md).

1. Запустите Visual Studio и создайте пустое приложение Xamarin.Forms **LocalDatabaseTutorial**. Убедитесь, что в качестве механизма общего кода в приложении используется .NET Standard.

    > [!IMPORTANT]
    > Для фрагментов кода на C# и XAML в этом руководстве необходимо решение с именем **LocalDatabaseTutorial**. Выбор другого имени приведет к ошибкам сборки при копировании кода из этого руководства в решение.

    Дополнительные сведения о создаваемой библиотеке .NET Standard см. в разделе [Структура приложения Xamarin.Forms](~/get-started/first-app/index.md) статьи [Краткое руководство по Xamarin.Forms глубокое погружение в обработку](~/get-started/first-app/index.md).

1. В **обозревателе решений** выберите проект **LocalDatabaseTutorial**, щелкните правой кнопкой мыши и выберите **Управление пакетами NuGet...** .

    ![Снимок экрана: выбранный пункт меню "Управление пакетами NuGet"](../images/vs/add-nuget-packages.png "Элемент меню "Добавление пакетов NuGet"")

1. В разделе **Диспетчер пакетов NuGet** выберите вкладку **Обзор**, выполните поиск пакета NuGet **sqlite-net-pcl**, выберите его и нажмите кнопку **Установить**, чтобы добавить его в проект.

    ![Снимок экрана: пакет NuGet для SQLite.NET в диспетчере пакетов NuGet](../images/vs/add-package.png "Пакет NuGet для SQLite.NET")

    > [!NOTE]
    > Существует ряд пакетов NuGet с похожими названиями. Правильный пакет имеет следующие атрибуты:
    > - **Автор(ы)** : Фрэнк А. Крюгер (Frank A. Krueger)
    > - **Идентификатор:** sqlite-net-pcl
    > - **Ссылка NuGet:** [sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)  
    >
    > Несмотря на название, этот пакет NuGet можно использовать в проектах .NET Standard.

    Этот пакет будет использоваться для включения операций базы данных в приложение.

1. Создайте решение, чтобы убедиться в отсутствии ошибок.

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/vsmac)

Для работы с этим руководством вам нужно установить Visual Studio для Mac (последний выпуск) с поддержкой платформ Android и iOS. Кроме того, вам потребуется Xcode (последний выпуск). Дополнительные сведения об установке платформы Xamarin см. в статье [Установка Xamarin](~/get-started/installation/index.md).

1. Запустите Visual Studio для Mac и создайте пустое приложение Xamarin.Forms **LocalDatabaseTutorial**. Убедитесь, что в качестве механизма общего кода в приложении используется .NET Standard.

    > [!IMPORTANT]
    > Для фрагментов кода на C# и XAML в этом руководстве необходимо решение с именем **LocalDatabaseTutorial**. Выбор другого имени приведет к ошибкам сборки при копировании кода из этого руководства в решение.

    Дополнительные сведения о создаваемой библиотеке .NET Standard см. в разделе [Структура приложения Xamarin.Forms](~/get-started/first-app/index.md) статьи [Краткое руководство по Xamarin.Forms глубокое погружение в обработку](~/get-started/first-app/index.md).

1. На **Панели решения** выберите проект **LocalDatabaseTutorial**, щелкните правой кнопкой мыши и выберите **Добавить > Добавить пакеты NuGet...** .

    ![Снимок экрана: выбранный пункт меню "Добавить пакеты NuGet"](../images/vsmac/add-nuget-packages.png "Элемент меню "Добавление пакетов NuGet"")

1. В окне **Добавление пакетов** выполните поиск пакета NuGet **sqlite-net-pcl**, выберите его и нажмите кнопку **Добавить пакет**, чтобы добавить его в проект.

    ![Снимок экрана: пакет NuGet для SQLite.NET в диспетчере пакетов NuGet](../images/vsmac/add-package.png "Пакет NuGet для SQLite.NET")

    > [!NOTE]
    > Существует ряд пакетов NuGet с похожими названиями. Правильный пакет имеет следующие атрибуты:
    > - **Автор**: Фрэнк А. Крюгер (Frank A. Krueger)
    > - **Идентификатор:** sqlite-net-pcl
    > - **Ссылка NuGet:** [sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)  
    >
    > Несмотря на название, этот пакет NuGet можно использовать в проектах .NET Standard.

    Этот пакет будет использоваться для включения операций базы данных в приложение.

1. Создайте решение, чтобы убедиться в отсутствии ошибок.
