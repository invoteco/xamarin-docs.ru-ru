---
title: Как задать переменные среды выполнения Mono для проектов iOS в Xamarin Studio?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 1176CEA9-C7F1-411B-8F1A-99374E8AFF33
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/31/2017
ms.openlocfilehash: f8e3855b10a20bd4312420f8faf6c68dedde0c67
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/04/2019
ms.locfileid: "70292091"
---
# <a name="how-do-i-set-mono-runtime-environment-variables-for-ios-projects-in-xamarin-studio"></a>Как задать переменные среды выполнения Mono для проектов iOS в Xamarin Studio?

Если необходимо задать переменные среды выполнения для Mono, их можно задать в **параметрах проекта > запуска > страница Общие** .

Примечание. Переменные среды сборки мусора для SGen (параметры\_Mono\_GC), установленные таким образом, будут использоваться только при запуске из Xamarin Studio. При запуске приложения с устройства параметры SGen будут игнорироваться. 

Чтобы окончательно задать переменную среды для приложения, необходимо добавить ее в качестве дополнительного аргумента mtouch (для всех соответствующих конфигураций):

```csharp
   --setenv=NAME=VALUE
```

Чтобы просмотреть переменные среды, которые можно задать, перейдите на страницу man Mono:  [http://docs.go-mono.com/?link=man%3amono(1)](http://docs.go-mono.com/?link=man%3amono(1))См. раздел:`ENVIRONMENT VARIABLES`

![](xs-mono-runtime-images/environment-variables.jpg "Задание переменных среды для проекта")
