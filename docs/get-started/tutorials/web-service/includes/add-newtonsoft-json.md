---
ms.openlocfilehash: aee7c7eb494af76bb450038453c19ee1ed83f2d3
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "61388903"
---
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Запустите Visual Studio и создайте пустое приложение Xamarin.Forms **WebServiceTutorial**. Убедитесь, что в качестве механизма общего кода в приложении используется .NET Standard.

    > [!IMPORTANT]
    > Фрагменты кода на C# и XAML из этого руководства предполагают, что решение называется **WebServiceTutorial**. Выбор другого имени приведет к ошибкам сборки при копировании кода из этого руководства в решение.

    Дополнительные сведения о создаваемой библиотеке .NET Standard см. в разделе [Структура приложения Xamarin.Forms](~/get-started/first-app/index.md) статьи [Краткое руководство по Xamarin.Forms: глубокое погружение в обработку](~/get-started/first-app/index.md).

1. В **обозревателе решений** выберите проект **WebServiceTutorial**, щелкните правой кнопкой мыши и выберите **Управление пакетами NuGet...**:

    ![Снимок экрана: пункт меню "Добавить пакеты NuGet"](../images/vs/add-nuget-packages.png "Пункт меню \"Добавить пакеты NuGet\"")

1. В разделе **Диспетчер пакетов NuGet** выберите вкладку **Обзор**, выполните поиск пакета NuGet **Newtonsoft.Json**, выберите его и нажмите кнопку **Установить**, чтобы добавить его в проект:

    ![Снимок экрана: пакет NuGet Newtonsoft.Json в диспетчере пакетов NuGet](../images/vs/add-package.png "Пакет NuGet Newtonsoft.Json")

    Этот пакет будет использоваться для включения десериализации JSON в приложение.

1. Соберите решение, чтобы убедиться в отсутствии ошибок.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

1. Запустите Visual Studio для Mac и создайте пустое приложение Xamarin.Forms **WebServiceTutorial**. Убедитесь, что в качестве механизма общего кода в приложении используется .NET Standard.

    > [!IMPORTANT]
    > Фрагменты кода на C# и XAML из этого руководства предполагают, что решение называется **WebServiceTutorial**. Выбор другого имени приведет к ошибкам сборки при копировании кода из этого руководства в решение.

    Дополнительные сведения о создаваемой библиотеке .NET Standard см. в разделе [Структура приложения Xamarin.Forms](~/get-started/first-app/index.md) статьи [Краткое руководство по Xamarin.Forms: глубокое погружение в обработку](~/get-started/first-app/index.md).

1. На **панели решений** выберите проект **WebServiceTutorial**, щелкните правой кнопкой мыши и выберите **Добавить > Добавить пакеты NuGet...**:

    ![Снимок экрана: пункт меню "Добавить пакеты NuGet"](../images/vsmac/add-nuget-packages.png "Пункт меню \"Добавить пакеты NuGet\"")

1. В окне **Добавление пакетов** выполните поиск пакета NuGet **Newtonsoft.Json**, выберите его и нажмите кнопку **Добавить пакет**, чтобы добавить его в проект:

    ![Снимок экрана: пакет NuGet Newtonsoft.Json в диспетчере пакетов NuGet](../images/vsmac/add-package.png "Пакет NuGet Newtonsoft.Json")

    Этот пакет будет использоваться для включения десериализации JSON в приложение.

1. Соберите решение, чтобы убедиться в отсутствии ошибок.
