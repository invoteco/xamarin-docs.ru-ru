---
title: Как перейти на использование более ранней версии пакета NuGet
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 2375F833-A630-471E-B8E9-5AD2CB81F264
author: davidortinau
ms.author: daortin
ms.date: 05/08/2018
ms.openlocfilehash: 0c70859845915a821bb83b0f9d29528634b1a5de
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73013614"
---
# <a name="how-do-i-downgrade-a-nuget-package"></a>Как перейти на использование более ранней версии пакета NuGet

Visual Studio для Mac & Visual Studio имеют функции для выбора старых версий пакетов и их автоматической установки. Аналогично тому, как работает обновление пакетов. Эти действия описаны ниже.

## <a name="visual-studio"></a>Visual Studio

1. Последовательно выберите **инструменты > диспетчер пакетов NuGet > консоль диспетчера пакетов** .
2. Задание проекта в качестве **проекта по умолчанию**
3. Используйте следующий синтаксис:

    > Install-Package [PackageName] — версия [вкладка для меню "версия"]

Можно также скопировать и вставить точную команду из страницы NuGet пакета. Пример для Xamarin. Forms: [https://www.nuget.org/packages/Xamarin.Forms/](https://www.nuget.org/packages/Xamarin.Forms/)

## <a name="visual-studio-for-mac"></a>Visual Studio для Mac

1. В проекте щелкните правой кнопкой мыши папку пакеты & выберите команду **Добавить пакеты** .
2. В сеарчбар для поиска необходимых пакетов можно использовать следующий синтаксис:

    `[PackageName] version:*`

### <a name="examples"></a>Примеры 

- Список всех пакетов Xamarin. Forms: 

    `Xamarin.Forms version:`

- Список всех пакетов Xamarin. Forms 1.4. x: 

    `Xamarin.Forms version:1.4`

> [!NOTE]
> При добавлении пробела между `version:` & номера версии Поиск будет вести себя так, как если бы не была указана версия.
