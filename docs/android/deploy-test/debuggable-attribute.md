---
title: Атрибут Debuggable
ms.prod: xamarin
ms.assetid: 1ABF90F1-6A70-45AE-9271-D90DC42807D0
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/05/2018
ms.openlocfilehash: 55de652363cb761e730d2d92f66a14a941899ca2
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028114"
---
# <a name="debuggable-attribute"></a>Атрибут Debuggable

Для отладки Android поддерживает протокол JDWP (Java Debug Wire Protocol). Эта технология позволяет некоторым средствам, например ADB, взаимодействовать с виртуальной машиной Java. Протокол JDWP очень важен на этапе разработки, но не забывайте отключить его перед публикацией приложения.

JDWP можно задать значением атрибута `android:debuggable` в приложении Android. Xamarin.Android предоставляет следующие методы для установки этого атрибута.

1. Создайте файл `AndroidManifext.xml` и задайте в нем атрибут `android:debuggable`.
2. Включите `ApplicationAttribute` в файл `.CS`, например так: `[assembly: Application(Debuggable=false)]` .

Если присутствуют одновременно `AndroidManifest.xml` и `ApplicationAttribute`, содержимое `AndroidManifest.xml` имеет более высокий приоритет, чем `ApplicationAttribute`.

Если не указано ни `AndroidManifest.xml`, ни `ApplicationAttribute`, то значение по умолчанию для атрибута `android:debuggable` зависит от того, создаются ли отладочные символы. Если символы отладки присутствуют, Xamarin.Android устанавливает для атрибута `android:debuggable` значение `true`.

Обратите внимание, что значение атрибута `android:debuggable` НЕ ВСЕГДА зависит от конфигурации сборки. Может случиться так, что для сборки выпуска атрибут `android:debuggable` имеет значение true.

## <a name="related-links"></a>Связанные ссылки

- [Приложения с атрибутом Debuggable на Android Market](https://labs.f-secure.com/archive/debuggable-apps-in-android-market/)
