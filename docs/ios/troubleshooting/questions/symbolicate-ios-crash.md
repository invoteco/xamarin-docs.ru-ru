---
title: Где найти dSYM-файл, чтобы выразить символами журналы с данными об аварийном завершении iOS?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: CB8607B9-FFDA-4617-8210-8E43EC512588
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 05/09/2018
ms.openlocfilehash: 418a0196849099da03983085aca9ceed2077207b
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73030919"
---
# <a name="where-can-i-find-the-dsym-file-to-symbolicate-ios-crash-logs"></a>Где найти dSYM-файл, чтобы выразить символами журналы с данными об аварийном завершении iOS?

При создании приложения iOS с Visual Studio для Mac или Visual Studio 2017 файл. dSYM, необходимый для симболикате отчетов о сбоях, будет помещен в ту же иерархию каталогов, что и файл проекта приложения (. csproj). Точное расположение зависит от параметров сборки проекта:

- Если вы включили сборки для конкретных устройств, dSYM можно найти в следующем каталоге:

    **&lt;каталог проекта&gt;/bin/&lt;платформа&gt;/&lt;конфигурация&gt;/девице-буилдс/&lt;устройство&gt;-&lt;версия ОС&gt;/**

    Пример:
  
    **TestApp/bin/iPhone/Release/Device-Builds/iPhone 8.4-11.3.1/**

- Если вы не включили сборки для конкретных устройств, dSYM можно найти в следующем каталоге:

    **&lt;каталог проекта&gt;/bin/&lt;платформа&gt;/&lt;Configuration&gt;/**

    Пример:

    **TestApp/bin/iPhone/Release/**

> [!NOTE]
> В рамках процесса сборки Visual Studio 2017 копирует файл dSYM из узла сборки Mac в Windows. Если dSYM-файл не отображается в Windows, убедитесь, что вы настроили параметры сборки приложения, чтобы [создать IPA-файл](~/ios/deploy-test/app-distribution/ipa-support.md).

## <a name="see-also"></a>См. также

- [Файлы аварийного завершения iOS симболикатинг (Xamarin. iOS)](https://www.jmillerdev.net/symbolicating-ios-crash-files-xamarin-ios/)
- [Пояснение журналов аварийного завершения работы приложений iOS](https://www.raywenderlich.com/23704/demystifying-ios-application-crash-logs)
