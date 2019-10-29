---
title: Использование библиотек .NET Standard для совместного использования кода
description: В этом документе описано, как использовать библиотеки .NET Standard для совместного использования кода. В нем обсуждается создание библиотеки .NET Standard, изменение ее параметров и использование в приложении.
ms.prod: xamarin
ms.assetid: 8C30F8D3-1920-453E-9E8B-D40696736FF2
author: davidortinau
ms.author: daortin
ms.custom: video
ms.date: 07/18/2018
ms.openlocfilehash: cae59053374f673a56d02e86cd59fb85f313c41b
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73016815"
---
# <a name="net-standard-library-code-sharing"></a>Совместное использование кода библиотеки .NET Standard

Библиотеки .NET Standard имеют унифицированный API для всех платформ .NET, включая Xamarin и .NET Core. Создайте единую библиотеку .NET Standard и используйте ее из любой среды выполнения, поддерживающей .NET Standardную платформу. Дополнительные сведения о поддерживаемых платформах см. в [этой диаграмме](https://docs.microsoft.com/dotnet/standard/net-standard#net-implementation-support) .

Хотя .NET Standard версии 1,0 – 1,6 предоставляют инкрементно большие подмножества .NET Framework, .NET Standard 2,0 обеспечивает лучший уровень поддержки для приложений Xamarin и для переноса существующих переносимых библиотек классов.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio для Mac](#tab/macos)

## <a name="visual-studio-for-mac"></a>Visual Studio для Mac

В этом разделе описывается создание и использование библиотеки .NET Standard с помощью Visual Studio для Mac.

### <a name="creating-a-net-standard-library"></a>Создание библиотеки .NET Standard

Вы можете добавить в решение библиотеку .NET Standard, выполнив следующие действия.

1. В диалоговом окне **Добавление нового проекта** выберите категорию **.NET Core** , а затем выберите **.NET Standard библиотека**:

    ![Создание библиотеки .NET Standard](net-standard-images/vsm01-m157.png "Создание новой библиотеки .NET Standard")

2. На следующем экране выберите целевую платформу — **.NET Standard 2,0** рекомендуется:

    [![выберите .NET Standard 2,0](net-standard-images/vsm01a-m157-sml.png)](net-standard-images/vsm01a-m157.png#lightbox)

3. На последнем экране введите имя проекта и нажмите кнопку **создать**.

4. Проект библиотеки .NET Standard будет выглядеть так, как показано на обозреватель решений. Узел зависимости будет указывать, что библиотека использует библиотеку [NETStandard. Library](https://www.nuget.org/packages/NETStandard.Library/).

    ![Узел зависимостей в решении указывает .NET Standard](net-standard-images/vsm02-m157.png)

#### <a name="editing-net-standard-library-settings"></a>Изменение параметров библиотеки .NET Standard

Параметры библиотеки .NET Standard можно просмотреть и изменить, щелкнув проект правой кнопкой мыши и выбрав `Options`, как показано на следующем снимке экрана:

![Изменение .NET Standard целевой платформы в параметрах проекта](net-standard-images/vsm03-m157.png "Изменение версии .NET Standard целевой платформы в параметрах проекта")

Внутри можно изменить версию `netstandard`, изменив значение раскрывающегося списка `Target Framework`.

**Кроме того:** Вы можете изменить `.csproj` напрямую, чтобы изменить это значение.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="visual-studio-2017-windows"></a>Visual Studio 2017 (Windows)

В этом разделе описывается создание и использование библиотеки .NET Standard с помощью Visual Studio.

### <a name="creating-a-net-standard-library"></a>Создание библиотеки .NET Standard

Добавление библиотеки .NET Standard в решение довольно проста.

1. В диалоговом окне **Новый проект** выберите категорию **.NET Standard** , а затем выберите пункт **Библиотека классов (.NET Standard)** .

    ![Создание новой .NET Standard библиотеки классов](net-standard-images/vs01-w157.png "Создать новую библиотеку классов .NET Standard")

2. Проект библиотеки .NET Standard будет выглядеть так, как показано на обозреватель решений. Узел зависимости будет указывать, что библиотека использует библиотеку [NETStandard. Library](https://www.nuget.org/packages/NETStandard.Library/).

    ![NETStandard. Library в папке проекта](net-standard-images/vs02-w157.png "Проект .NET Standard в решении")

### <a name="editing-net-standard-library-settings"></a>Изменение параметров библиотеки .NET Standard

Параметры библиотеки .NET Standard можно просмотреть и изменить, щелкнув проект правой кнопкой мыши и выбрав **Свойства** , как показано на следующем снимке экрана:

![Изменение целевых платформ .NET Standard в свойствах проекта](net-standard-images/vs03-w157.png "Ссылка на библиотеку .NET Standard так же, как другие проекты")

**Кроме того:** Можно изменить `.csproj` непосредственно, чтобы изменить элемент `TargetFramework` и изменить целевую версию (например, `<TargetFramework>netstandard2.0</TargetFramework>`).

### <a name="using-a-net-standard-library-project"></a>Использование проекта библиотеки .NET Standard

После создания библиотеки .NET Standard можно добавить ссылку на нее из любого совместимого приложения или проекта библиотеки так же, как обычно добавляются ссылки. В Visual Studio щелкните правой кнопкой мыши узел ссылки и выберите команду **Добавить ссылку...** затем перейдите на вкладку **проекты > решение** , как показано ниже.

![Ссылка на библиотеку .NET Standard](net-standard-images/vs04.png "В Visual Studio щелкните правой кнопкой мыши узел ссылки и выберите команду Добавить ссылку... затем перейдите на вкладку проекты решений, как показано ниже.")

-----

## <a name="net-standard-and-xamarinforms-for-the-net-developer-video"></a>.NET Standard и Xamarin. Forms для разработчика .NET (видео)

> [!Video https://channel9.msdn.com/Shows/XamarinShow/NET-Standard-and-XamarinForms-for-the-NET-Developer/player]

## <a name="related-links"></a>Связанные ссылки

* [.NET Standard](https://docs.microsoft.com/dotnet/standard/net-standard) — подробные сведения и сравнение с PCL.
