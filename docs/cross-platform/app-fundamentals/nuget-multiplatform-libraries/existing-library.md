---
title: Создание NuGet из существующих проектов библиотек
description: В этом документе описывается, как создать пакет NuGet на основе существующего проекта библиотеки, что позволяет совместно использовать код с другими разработчиками.
ms.prod: xamarin
ms.assetid: EDAC3E5E-DB7D-40A9-AE28-45C52ADA854E
author: conceptdev
ms.author: crdun
ms.date: 04/20/2017
ms.openlocfilehash: e70cf426b2357570585c1c8f1cf21715858cb6f6
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2019
ms.locfileid: "70758160"
---
# <a name="creating-a-nuget-from-existing-library-projects"></a>Создание NuGet из существующих проектов библиотек

Существующие библиотеки PCL или .NET Standard можно включить в NuGet через окно " **Параметры проекта** ":

1. Щелкните правой кнопкой мыши проект библиотеки в **панель решения** и выберите пункт **Параметры**.

2. Перейдите в раздел **метаданных пакета NuGet >** и введите все [необходимые сведения](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md) на вкладке **Общие** .

   [![](existing-library-images/existing-metadata-sml.png "Введите необходимые метаданные")](existing-library-images/existing-metadata.png#lightbox)

3. При необходимости [добавьте дополнительные метаданные](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md) на вкладке **сведения** .

4. После настройки метаданных можно щелкнуть правой кнопкой мыши проект и выбрать пункт **создать пакет NuGet** , а файл пакета NuGet **. nupkg** будет сохранен в папке **/bin/** (Отладка или выпуск, в зависимости от конфигурации).

   ![](existing-library-images/create-nuget-package.png "Выберите пункт Создать пакет NuGet в контекстном меню.")

5. Чтобы создать пакет NuGet при _каждой_ сборке или развертывании, перейдите в раздел **NuGet Package > Build** и Tick ( **создать пакет NuGet) при сборке проекта**:

    [![](existing-library-images/existing-tickbox-sml.png "Такт для создания пакета NuGet")](existing-library-images/existing-tickbox.png#lightbox)

> [!NOTE]
> Создание пакета NuGet может замедлить процесс сборки. Если это поле не является тактовым, вы по-прежнему можете создать пакет NuGet вручную в любое время из контекстного меню проекта (см. шаг 4 выше).

## <a name="verifying-the-output"></a>Проверка выходных данных

Пакеты NuGet также являются ZIP-файлами, поэтому можно проверить внутреннюю структуру созданного пакета.

На этом снимке экрана показано содержимое NuGet на основе PCL — включена только одна сборка PCL:

![](existing-library-images/nuget-output.png "Файлы, содержащиеся в пакете NuGet")

## <a name="related-links"></a>Связанные ссылки

- [Руководство по метаданным](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)
