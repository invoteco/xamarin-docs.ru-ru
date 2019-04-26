---
title: ApiDefinitions & StructsAndEnums Files
description: В этом документе описаны ApiDefinitions.cs и StructsAndEnums.cs файлы, которые создает Sharpie цели. Эти файлы затем используются для доступа к коду Objective-C из C#.
ms.prod: xamarin
ms.assetid: AC2087C0-BA54-46D8-B70C-6972941C8F73
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: df8d4508db14116a5b36e893f161ac891d58dc46
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "61266374"
---
# <a name="apidefinitions--structsandenums-files"></a>ApiDefinitions & StructsAndEnums Files

Когда цель Sharpie выполнится успешно, он создает `Binding/ApiDefinitions.cs` и `Binding/StructsAndEnums.cs` файлы.
Эти файлы добавляются в проект привязки в Visual Studio для Mac или переданные непосредственно в директиву `btouch` или `bmac` средства для создания окончательного привязки.

В *некоторые* случаи, созданные файлы может быть все, что нужно, однако чаще разработчику потребуется вручную изменять эти созданные файлы для устранения проблем, которые не удалось обработать автоматически с помощью средства (например, эти помеченные с помощью [ `Verify` атрибут](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md)).

Ниже перечислены некоторые из следующих шагов.

- **Настройка имен**: Иногда требуется настроить имена методов и классов в соответствии с рекомендации по разработке .NET Framework.
- **Методы или свойства**: Эвристические методы, используемые Sharpie цели иногда выберет метод для включения в свойство. На этом этапе вы решите, является ли это предполагаемое поведение, или нет.
- **Обработка событий**: Можно связать классов с классами в делегат и автоматически создавать события для тех.
- **Подключить уведомления**: Он уже не сможете извлечь контракта API-интерфейса уведомлений из файлов заголовка чистые, для этого потребуется поездку на документацию по API. Если требуется строго типизированных уведомлений, необходимо будет обновить результат.
- **API проверки**: На этом этапе вы можете предоставить дополнительные конструкторы, добавьте методы (для обеспечения C# синтаксиса инициализации на создания), оператор перегрузке и реализовать свои собственные интерфейсы для файла дополнительных определений.

См. в разделе [привязки API](~/cross-platform/macios/binding/objective-c-libraries.md) описание, чтобы увидеть, как эти файлы помещаются в процесса привязки, как показано на следующей схеме:

![](apidefinitions-structsandenums-images/binding-flowchart.png "На этой схеме показан процесс привязки")

Ссылаться на [Справочник по типам привязки](~/cross-platform/macios/binding/binding-types-reference.md) Дополнительные сведения о содержимое этих файлов.

## <a name="related-links"></a>Связанные ссылки

- [Курс Xamarin University. Создание библиотеки привязки Objective-C](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Курс Xamarin University. Создание библиотеки привязки Objective-C с помощью цели Sharpie](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)
