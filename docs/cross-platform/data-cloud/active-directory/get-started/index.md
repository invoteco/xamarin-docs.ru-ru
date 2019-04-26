---
title: Azure Active Directory
description: В этом документе описаны шаги, которые должны учитываться, чтобы разрешить мобильное приложение для проверки подлинности в Azure Active Directory.
ms.prod: xamarin
ms.assetid: 70B3C2AB-CB4D-420C-9CFA-20CCFA0E3C78
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: ca33422817f19dbb0a04e8870800d3f5efa8af2a
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "61318302"
---
# <a name="azure-active-directory"></a>Azure Active Directory

_Регистрация приложения для использования Azure Active Directory_

Azure Active Directory позволяет разработчикам защищенным ресурсам, таких как файлы, ссылки и веб-API, используя учетную запись организации, которая сотрудники используют для входа в систему, в их системах или проверьте своей электронной почты.

Разработка мобильных приложений, которые могут проходить проверку подлинности с помощью Azure Active Directory состоит из трех этапов.
Первые два шага одинаковы как правило, независимо от того, какие службы вы планируете использовать. Третий шаг отличается для каждого типа службы:

  1. [Регистрация в Azure Active Directory](~/cross-platform/data-cloud/active-directory/get-started/register.md) на *windowsazure.com* портала, затем
  2. [Настройка служб](~/cross-platform/data-cloud/active-directory/get-started/configure.md).
  3. Разработка мобильных приложений с помощью служб.

Примеры различных служб, которые можно использовать.

- [Graph API](~/cross-platform/data-cloud/active-directory/graph.md)
- Веб-интерфейс API
- Office 365


## <a name="conclusion"></a>Заключение

Используя приведенные выше вы действия может проверить подлинность ваших мобильных приложений с помощью Azure Active Directory. Библиотеки аутентификации Active Directory (ADAL) значительно упрощает меньшее число строк кода, сохраняя при этом большая часть кода те же и тем самым делая его совместного использования на платформах.



## <a name="related-links"></a>Связанные ссылки

- [Пример Microsoft NativeClient](https://github.com/AzureADSamples/NativeClient-MultiTarget-DotNet)
