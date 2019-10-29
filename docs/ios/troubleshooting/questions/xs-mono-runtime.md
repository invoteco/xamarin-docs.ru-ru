---
title: Как задать переменные среды выполнения Mono для проектов iOS в Xamarin Studio?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 1176CEA9-C7F1-411B-8F1A-99374E8AFF33
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/31/2017
ms.openlocfilehash: fdd208122934b6fa8194a644592d1e23c4000d57
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73030892"
---
# <a name="how-do-i-set-mono-runtime-environment-variables-for-ios-projects-in-xamarin-studio"></a>Как задать переменные среды выполнения Mono для проектов iOS в Xamarin Studio?

Если необходимо задать переменные среды выполнения для Mono, их можно задать в **параметрах проекта > запуска > страница Общие** .

Примечание. переменные среды сборки мусора для SGen (параметры MONO\_GC\_) заданы таким образом, только при запуске из Xamarin Studio. При запуске приложения с устройства параметры SGen будут игнорироваться. 

Чтобы окончательно задать переменную среды для приложения, необходимо добавить ее в качестве дополнительного аргумента mtouch (для всех соответствующих конфигураций):

```csharp
   --setenv=NAME=VALUE
```

Чтобы просмотреть переменные среды, которые могут быть заданы, перейдите на страницу с именем человека Mono: [http://docs.go-mono.com/?link=man%3amono(1)](http://docs.go-mono.com/?link=man%3amono(1)) см. раздел: `ENVIRONMENT VARIABLES`

![](xs-mono-runtime-images/environment-variables.jpg "Setting environment variables for a project")
