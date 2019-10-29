---
title: Почему сборка iOS завершается ошибкой "В цепочке ключей не удалось найти допустимые ключи подписывания кода iPhone"?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 9DF24C46-D521-4112-9B21-52EA4E8D90D0
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 04/03/2018
ms.openlocfilehash: 0c777b8d5326963e959d8bb13d81d7058caa6bde
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73030939"
---
# <a name="why-does-my-ios-build-fail-with-no-valid-iphone-code-signing-keys-found-in-keychain"></a>Почему сборка iOS завершается ошибкой "В цепочке ключей не удалось найти допустимые ключи подписывания кода iPhone"?

## <a name="cause-of-the-error"></a>Причина ошибки

Это сообщение об ошибке возникает, если рассматриваемый проект ищет действительные учетные данные подписывания кода, но их не удается найти. Для тестирования и развертывания на физических устройствах iOS требуется подписывание кода. а также ad-hoc & сборки магазина приложений.

### <a name="provisioning-devices"></a>Подготовка устройств

Если вы еще не подготовили устройство iOS, в следующем разделе вы узнаете, как выполнить полный пошаговый процесс: [руководство по подготовке устройств](~/ios/get-started/installation/device-provisioning/index.md) .

## <a name="bug-when-using-ios-simulator"></a>Ошибка при использовании симулятора iOS

> [!NOTE]
> Эта проблема решена в последних версиях Xamarin для Visual Studio. Однако если проблема возникает в последней версии программного обеспечения, запишите [новую ошибку](~/cross-platform/troubleshooting/questions/howto-file-bug.md) с полными сведениями о версии и полным выходным файлом журнала сборки.

Произошла ошибка в Xamarin. Visual Studio 3,11, которая привела к проекту iOS в шаблоне Xamarin. Forms, чтобы добавить соplistные права на сборку симуляторов; эффективное блокирование тестирования с помощью симулятора.

### <a name="how-to-fix"></a>Как исправить

Проблему можно устранить, удалив флаг `<CodesignEntitlements>` из отладочных сборок в файле CSPROJ. Это можно сделать следующим образом:

> [!WARNING]
> Ошибки в CSPROJ-файлах могут нарушить работу проекта, поэтому рекомендуется создать резервную копию файлов перед попыткой.

1. Щелкните правой кнопкой мыши проект iOS в области решение и выберите команду **Выгрузить проект** .
2. Еще раз щелкните проект правой кнопкой мыши и выберите **изменить [имя_проекта]. csproj.**
3. Найдите группы PropertyGroup отладки. они должны начинаться с флагов, которые выглядят следующим образом:
   - Отладка: `<PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Debug|iPhoneSimulator' ">`
   - Выпуск: `<PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Release|iPhoneSimulator' ">`
4. В каждой из сборок, использующих симулятор, удалите или закомментируйте следующее свойство: `<CodesignEntitlements>Entitlements.plist</CodesignEntitlements>`
5. Перезагрузите проект, и вы сможете развернуть его в симуляторе.

### <a name="next-steps"></a>Следующие шаги
Для получения дополнительной помощи, для связи с нами или если проблема остается даже после использования приведенных выше сведений, ознакомьтесь с возможностями [поддержки Xamarin?](~/cross-platform/troubleshooting/support-options.md) дополнительные сведения о вариантах контактов, предложениях и о том, как создать новую ошибку при необходимости .
