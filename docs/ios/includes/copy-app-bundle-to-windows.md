---
ms.openlocfilehash: 22a8542c0e829db8b889abc2c7fe5f5ab9d19ed4
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/13/2020
ms.locfileid: "69527640"
---

При создании приложений iOS в Visual Studio и агенте сборки Mac пакет APP не копируется обратно на компьютер Windows. В инструменты Xamarin для Visual Studio 7.4 добавлено новое свойство `CopyAppBundle`, позволяющее сборкам CI копировать пакеты APP обратно в Windows.

Чтобы использовать эту функцию, добавьте свойство `CopyAppBundle` в CSPROJ-файл в группе свойств, к которой следует применить эту функцию. Например, в приведенном ниже примере показано копирование пакета APP обратно на компьютер Windows для сборки **Отладка**, предназначенной для **iPhoneSimulator**:

```xml
<PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Debug|iPhoneSimulator' ">
    <CopyAppBundle>true</CopyAppBundle>
</PropertyGroup>
```
