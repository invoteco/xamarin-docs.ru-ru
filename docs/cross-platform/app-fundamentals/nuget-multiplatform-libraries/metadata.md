---
title: Редактирование метаданных NuGet
description: В этом документе описывается, как использовать параметры проекта для изменения метаданных NuGet многоплатформенного библиотек. Здесь рассматриваются обязательные и необязательные метаданные.
ms.prod: xamarin
ms.assetid: 147BA370-67A7-4E6C-BF17-AA7C536C0A48
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 3680b02003a844668b0b5c97e5d4c0d296ae3500
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "61266886"
---
# <a name="editing-nuget-metadata"></a>Редактирование метаданных NuGet

_Используйте параметры проекта, чтобы изменить метаданные NuGet многоплатформенного библиотек_

Типы проектов библиотеки (например PCL или .NET Standard или новый тип проекта NuGet) имеют **пакет NuGet** статьи **параметры проекта** окна.

**Метаданных** раздел настраивает значения, используемые в [ **файлу nuspec** файл манифеста пакета NuGet](https://docs.microsoft.com/nuget/create-packages/creating-a-package#the-role-and-structure-of-the-nuspec-file).

## <a name="required-information"></a>Необходимые сведения

**Общие** вкладка содержит четыре поля, которые должны быть введены для создания пакета NuGet:

[![](metadata-images/metadata-general-sml.png "Окно необходимые метаданные пакета NuGet")](metadata-images/metadata-general.png#lightbox)

- **Идентификатор** — идентификатор пакета, который должен быть уникальным в пределах Nuget.org (или везде, где этот пакет будет распространяться). Выполните это [рекомендации](https://docs.microsoft.com/nuget/create-packages/creating-a-package#choosing-a-unique-package-identifier-and-setting-the-version-number) и использовать только символы, допустимые в URL-адрес (без пробелов и избежать большинство специальных символов).
- **Версия** — выберите номер версии, согласованные с [правила управления версиями NuGet](https://docs.microsoft.com/nuget/create-packages/dependency-versions).
- **Авторы** — разделенный запятыми список имен.
- **Описание** — Обзор функций пакета, который отображается, когда пользователь выбирает пакет.

> [!NOTE]
> Не забудьте увеличить номер версии, при создании новых версий для распространения NuGet или других пользователей.

Дополнительные сведения см. в разделе [требуется Справочник по элементам](https://docs.microsoft.com/nuget/schema/nuspec#required-metadata-elements) Дополнительные сведения, а также как эти подробные инструкции по [Выбор уникального идентификатора пакета и задание номера версии](https://docs.microsoft.com/nuget/create-packages/creating-a-package#choosing-a-unique-package-identifier-and-setting-the-version-number) и [ Указание типа пакета](https://docs.microsoft.com/nuget/create-packages/creating-a-package#setting-a-package-type).

> [!IMPORTANT]
> Все поля на этой вкладке, должны быть введены; в противном случае появится сообщение об ошибке: _«Проект не поддерживает метаданных NuGet, не будет создан пакет NuGet. Метаданные пакета NuGet можно указать в разделе "метаданные" в параметрах проекта»_

## <a name="optional-metadata"></a>Необязательные метаданные

**Сведения** вкладка содержит необязательные поля для включения в файле манифеста пакета NuGet.

[![](metadata-images/metadata-detail-sml.png "Окно необязательные метаданные пакета NuGet")](metadata-images/metadata-detail.png#lightbox)

Ссылаться на [Справочник по дополнительным элементы](https://docs.microsoft.com/nuget/schema/nuspec#optional-metadata-elements) Дополнительные сведения об обязательных и необязательных полях.

> [!NOTE]
> Если пакет NuGet распространяется на [NuGet.org](https://www.nuget.org) рекомендуется указать столько информации, как можно.


## <a name="related-links"></a>Связанные ссылки

- [Справочник по файлу NUSPEC](https://docs.microsoft.com/nuget/schema/nuspec#general-form-and-schema)
