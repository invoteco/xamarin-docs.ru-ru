---
title: Создание NuGet из имеющиеся проекты библиотеки
description: В этом документе описывается создание пакета NuGet из существующего проекта библиотеки, что позволяет коду, совместно с другими разработчиками.
ms.prod: xamarin
ms.assetid: EDAC3E5E-DB7D-40A9-AE28-45C52ADA854E
author: asb3993
ms.author: amburns
ms.date: 04/20/2017
ms.openlocfilehash: 7f407b22d1793d585ae40aeae8c2d9b7616784e6
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "61267832"
---
# <a name="creating-a-nuget-from-existing-library-projects"></a>Создание NuGet из имеющиеся проекты библиотеки

Существующей библиотекой PCL или .NET Standard библиотек, которые можно преобразовать в пакеты NuGet через **параметры проекта** окна:

1. Щелкните правой кнопкой мыши проект библиотеки в **панели решения** и выберите **параметры**.

2. Перейдите к **пакетов NuGet > метаданных** раздела и ввести все [необходимые сведения](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md) в **Общие** вкладку:

  [![](existing-library-images/existing-metadata-sml.png "Введите необходимые метаданные")](existing-library-images/existing-metadata.png#lightbox)

3. При необходимости [добавления дополнительных метаданных](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md) в **сведения** вкладки.

4. После настройки метаданных можно правой кнопкой мыши проект и выберите **создать пакет NuGet** и **.nupkg** будет сохранен файл пакета NuGet в **/bin/** папка (отладки или выпуска, в зависимости от конфигурации).

  ![](existing-library-images/create-nuget-package.png "Выберите создать пакет NuGet из контекстного меню")

5. Создание пакета NuGet на _каждые_ сборки или развертывания, перейдите к **пакет NuGet > построения** раздел и их **Создание пакета NuGet при сборке проекта**:

    [![](existing-library-images/existing-tickbox-sml.png "Установка флажка для создания пакета NuGet")](existing-library-images/existing-tickbox.png#lightbox)

> [!NOTE]
> Создание пакета NuGet пакета может замедлить работу процесса сборки. Если этот флажок не установлен, можно по-прежнему создать пакет NuGet вручную в любое время, в контекстном меню проекта (как показано на шаге 4 выше).

## <a name="verifying-the-output"></a>Проверка выходных данных

Пакеты NuGet также являются ZIP-файлы, поэтому имеется возможность проверить внутреннюю структуру создаваемый пакет.

На этом снимке экрана показано содержимое NuGet на основе переносимой библиотеки Классов — включается только в одну сборку переносимой библиотеки Классов:

![](existing-library-images/nuget-output.png "Файлы, содержащиеся в пакете NuGet")


## <a name="related-links"></a>Связанные ссылки

- [Руководство по метаданным](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)
