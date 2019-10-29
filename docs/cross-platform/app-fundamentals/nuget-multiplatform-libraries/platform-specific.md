---
title: Создание новых проектов библиотеки для платформы для NuGet
description: В этом документе описывается создание одного пакета NuGet, содержащего код, зависящий от платформы, для нескольких платформ.
ms.prod: xamarin
ms.assetid: D8BC4906-805F-4AFB-8D1A-88B7BF87E17F
author: davidortinau
ms.author: daortin
ms.date: 03/23/2017
ms.openlocfilehash: 925e08c600c695640c927ada26df376a252b3927
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73016724"
---
# <a name="creating-new-platform-specific-library-projects-for-nuget"></a>Создание новых проектов библиотеки для платформы для NuGet

Проекты многоплатформенных библиотек, предназначенные для конкретных платформ, таких как iOS и Android, лучше работают с общими проектами.

NuGet может содержать код, относящийся к iOS и Android, а также код .NET для обоих типов.

Несколько сборок создаются и встроены в один пакет NuGet. Стандарты NuGet гарантируют, что пакет можно добавить ко всем поддерживаемым типам проектов, таким как проекты Xamarin. iOS и Android.

## <a name="steps-to-create-a-cross-platform-library-nuget"></a>Шаги для создания кросс-платформенной библиотеки NuGet

1. Выберите **файл > создать решение** (или щелкните правой кнопкой мыши существующее решение и выберите **Добавить > новый проект**).

2. Выберите **многоплатформенную библиотеку** из раздела **многоплатформенная > Библиотека** :

    [![](platform-specific-images/mulitplatform-library-sml.png "Configure multi-platform library for a single code base")](platform-specific-images/multiplatform-library.png#lightbox)

3. Введите **имя** и **Описание**и выберите значение для **конкретной платформы**:

    [![](platform-specific-images/specific-configure-sml.png "Configure platform-specific library for iOS and Android")](platform-specific-images/specific-configure.png#lightbox)

4. Завершите работу мастера. В решение добавляются следующие проекты:

    - **Проект Android** — код, зависящий от Android, при необходимости можно добавить в этот проект.
    - **проект iOS** — код, относящийся к iOS, при необходимости можно добавить в этот проект.
    - **Проект NuGet** — код не добавляется в этот проект. Он ссылается на другие проекты и содержит конфигурацию метаданных для выходных данных пакета NuGet.
    - **Общий проект** — общий код должен быть добавлен в этот проект, включая код, зависящий от платформы, в `#if` директивах компилятора.

5. Щелкните правой кнопкой мыши проект NuGet и выберите **Параметры**, затем откройте раздел **метаданных пакета NuGet > метаданные** и введите [необходимые метаданные](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md) (а также любые необязательные метаданные):

    [![](platform-specific-images/specific-metadata-sml.png "Enter required metadata")](platform-specific-images/specific-metadata.png#lightbox)

6. Кроме того, в окне **Параметры проекта** откройте раздел **эталонные сборки** и выберите профили PCL, которые будут поддерживаться общей библиотекой через "Bait and Switch":

    ![](platform-specific-images/specific-reference-assemblies.png "Also in the Project Options window, open the Reference Assemblies section and choose   which PCL profiles the shared library will support via bait and switch")

    > [!NOTE]
    > "Bait and Switch" означает, что сборки PCL будут содержать только API, предоставляемый библиотекой (он не может содержать код, зависящий от платформы). При добавлении NuGet в проект Xamarin общие библиотеки будут скомпилированы для PCL, но сборки для конкретной платформы содержат код, который фактически используется проектом iOS или Android.

7. Щелкните проект правой кнопкой мыши и выберите **создать пакет NuGet** (или выполните сборку или развертывание решения), а файл пакета NuGet **. nupkg** будет сохранен в папке **/bin/** (Отладка или выпуск, в зависимости от конфигурации).

    ![](platform-specific-images/create-nuget-package.png "NuGet package file will be saved in the bin folder either Debug or Release, depending on configuration")

## <a name="verifying-the-output"></a>Проверка выходных данных

Пакеты NuGet также являются ZIP-файлами, поэтому можно проверить внутреннюю структуру созданного пакета.

На этом снимке экрана показано содержимое зависимой от платформы NuGet, которая поддерживает iOS и Android и в ней были выбраны две эталонные сборки:

![](platform-specific-images/nuget-output.png "Files contained in the NuGet package")

## <a name="related-links"></a>Связанные ссылки

- [Руководство по метаданным](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)
