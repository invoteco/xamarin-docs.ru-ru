---
title: Как задать переменные среды выполнения Mono для проектов iOS в Xamarin Studio?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 1176CEA9-C7F1-411B-8F1A-99374E8AFF33
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/31/2017
ms.openlocfilehash: ba74e316f706e5bf22f973de4dad38d94ccd0db9
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "61417675"
---
# <a name="how-do-i-set-mono-runtime-environment-variables-for-ios-projects-in-xamarin-studio"></a>Как задать переменные среды выполнения Mono для проектов iOS в Xamarin Studio?

Если вам нужно задать переменные среды любой среде выполнения для Mono, их можно задать **параметры проекта > выполнить > Общие** страницы.

Примечание. Переменные среды сбора мусора для SGen (MONO\_GC\_PARAMS) набора, таким образом будет использоваться только при запуске из Xamarin Studio. Если вы запустите приложение с устройства, будет игнорироваться параметры Sgen. 

Чтобы окончательно задать переменную среды для приложения, необходимо добавить в качестве аргумента дополнительных mtouch (для всех конфигураций, связанных с):

```csharp
   --setenv=NAME=VALUE
```

Чтобы просмотреть переменные среды, которые могут быть заданы, обратитесь к странице Mono man:  [http://docs.go-mono.com/?link=man%3amono(1)](http://docs.go-mono.com/?link=man%3amono(1)) См. в разделе: `ENVIRONMENT VARIABLES`

![](xs-mono-runtime-images/environment-variables.jpg "Задание переменных среды для проекта")
