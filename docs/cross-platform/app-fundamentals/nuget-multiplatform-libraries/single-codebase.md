---
title: Создание новой многоплатформенной библиотеки для NuGet
description: В этом документе описывается создание многоплатформенной библиотеки для использования с NuGet. Этот метод подходит для бизнес-логики и алгоритмов, которые могут быть полностью выражены в библиотеке базовых классов .NET и, таким образом, выполняться на всех целевых платформах без кода, зависящего от платформы.
ms.prod: xamarin
ms.assetid: E7B55354-9BBE-4122-BCE3-3506B79090DD
author: conceptdev
ms.author: crdun
ms.date: 03/23/2017
ms.openlocfilehash: 966d7a21da0112383c08e862a357c3c1d4fb9c22
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/04/2019
ms.locfileid: "70289725"
---
# <a name="creating-a-new-multiplatform-library-for-nuget"></a>Создание новой многоплатформенной библиотеки для NuGet

Создание проекта многоплатформенной библиотеки с использованием PCL или .NET Standard означает, что полученный NuGet можно добавить в любой проект .NET, поддерживающий целевой профиль, включая проекты ASP.NET или классические приложения с помощью WinForms, WPF или UWP.

Библиотека может содержать только код, поддерживаемый выбранным профилем PCL или .NET Standard, а также любые другие добавленные NuGet.
Это подходит для бизнес-логики и алгоритмов, которые могут быть полностью выражены в библиотеке базовых классов .NET.

Отдельная сборка создается и встроена в пакет NuGet.

Если в дальнейшем требуются функциональные возможности платформы, [можно добавлять проекты для конкретных платформ](#add-platforms).

## <a name="steps-to-create-a-multiplatform-library-nuget"></a>Действия по созданию многоплатформенной библиотеки NuGet

1. Выберите **файл > создать решение** (или щелкните правой кнопкой мыши существующее решение и выберите **Добавить > новый проект**).

2. Выберите **многоплатформенную библиотеку** из раздела **многоплатформенная > Библиотека** :

   [![](single-codebase-images/mulitplatform-library-sml.png "Настройка многоплатформенной библиотеки для одной базы кода")](single-codebase-images/mulitplatform-library.png#lightbox)

3. Введите **имя** и **Описание**и выберите **Single для всех платформ**:

   [![](single-codebase-images/single-configure-sml.png "Настройка многоплатформенной библиотеки для одной базы кода")](single-codebase-images/single-configure.png#lightbox)

4. Завершите работу мастера. В решении создается один проект библиотеки.

5. Щелкните правой кнопкой мыши новый проект библиотеки и выберите пункт **Параметры**. Раздел **Build > General** позволяет задать **целевую платформу** — выберите переносимый профиль PCL .NET или версию .NET Standard:

   [![](single-codebase-images/single-choose-type-sml.png "Выберите PCL или .NET Standard для типа библиотеки")](single-codebase-images/single-choose-type.png#lightbox)

6. Кроме того, в окне **Параметры проекта** откройте раздел **NuGet Package > метаданных** и введите [необходимые метаданные](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md) (а также любые необязательные метаданные):

   [![](single-codebase-images/single-metadata-sml.png "Введите необходимые метаданные")](single-codebase-images/single-metadata.png#lightbox)

7. Щелкните правой кнопкой мыши проект библиотеки и выберите команду **создать пакет NuGet** (или выполните сборку или развертывание решения), а файл пакета NuGet **. nupkg** будет сохранен в папке **/bin/** (Отладка или выпуск, в зависимости от конфигурации):

   ![](single-codebase-images/create-nuget-package.png "Файл пакета NuGet будет сохранен в папке bin как отладка, так и выпуск, в зависимости от конфигурации.")


## <a name="verifying-the-output"></a>Проверка выходных данных

Пакеты NuGet также являются ZIP-файлами, поэтому можно проверить внутреннюю структуру созданного пакета.

На этом снимке экрана показано содержимое NuGet на основе PCL — включена только одна сборка PCL:

![](single-codebase-images/nuget-output.png "Файлы, содержащиеся в пакете NuGet")

<a name="add-platforms" />

## <a name="adding-platform-specific-code"></a>Добавление кода для конкретной платформы

Проекты на основе PCL и проекты на основе .NET Standard не могут содержать ссылки для конкретных платформ (например, функции iOS или Android).

Если существующий проект PCL или проект .NET Standard необходимо расширить для включения кода, зависящего от платформы, это можно сделать, щелкнув правой кнопкой мыши проект и выбрав **добавить > добавить реализацию платформы...** :

[![](single-codebase-images/add-later-sml.png "Меню \"добавить реализацию платформы\"")](single-codebase-images/add-later.png#lightbox)

В решение можно добавить один или несколько проектов платформы, а при необходимости можно преобразовать существующую библиотеку PCL или .NET Standard в общий проект:

[![](single-codebase-images/add-later-platforms-sml.png "Добавление параметров платформы, таких как iOS, Android и Shared Project")](single-codebase-images/add-later-platforms-sml.png#lightbox)

После преобразования в общий проект перейдите в[раздел](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/platform-specific.md) **Параметры проекта > пакет NuGet > справочные сборки**
и убедитесь, что выбраны все необходимые профили (чтобы NuGet продолжал быть совместим с проектами. ранее он использовался в).


## <a name="related-links"></a>Связанные ссылки

- [Руководство по метаданным](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)
