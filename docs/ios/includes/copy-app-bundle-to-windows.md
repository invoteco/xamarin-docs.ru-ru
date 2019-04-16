---
ms.openlocfilehash: 5bcf548c0ff907df0913e77a1def2fe27f3eecfd
ms.sourcegitcommit: e7f27ba75cae5099ef053b819b84132a77d4f9e7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/16/2019
ms.locfileid: "52295057"
---

При создании приложений iOS в Visual Studio и агенте сборки Mac пакет APP не копируется обратно на компьютер Windows. В инструменты Xamarin для Visual Studio 7.4 добавлено новое свойство `CopyAppBundle`, позволяющее сборкам CI копировать пакеты APP обратно в Windows.

Чтобы использовать эту функцию, добавьте свойство `CopyAppBundle` в CSPROJ-файл в группе свойств, к которой следует применить эту функцию. Например, в приведенном ниже примере показано копирование пакета APP обратно на компьютер Windows для сборки **Отладка**, предназначенной для **iPhoneSimulator**:

    <PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Debug|iPhoneSimulator' ">
        <CopyAppBundle>true</CopyAppBundle>
    </PropertyGroup>

