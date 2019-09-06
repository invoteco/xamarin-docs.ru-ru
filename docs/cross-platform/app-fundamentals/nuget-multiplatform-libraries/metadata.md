---
title: Изменение метаданных NuGet
description: В этом документе описывается, как использовать параметры проекта для изменения метаданных NuGet для многоплатформенных библиотек. В нем обсуждаются обязательные и необязательные метаданные.
ms.prod: xamarin
ms.assetid: 147BA370-67A7-4E6C-BF17-AA7C536C0A48
author: conceptdev
ms.author: crdun
ms.date: 03/23/2017
ms.openlocfilehash: bf8efad28c7ec6acfd0e43403e8db14639a3c755
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/04/2019
ms.locfileid: "70289410"
---
# <a name="editing-nuget-metadata"></a>Изменение метаданных NuGet

_Использование параметров проекта для изменения метаданных NuGet для многоплатформенных библиотек_

Типы проектов библиотеки (например, PCL или .NET Standard или новый тип проекта NuGet) имеют раздел **пакета NuGet** в окне " **Параметры проекта** ".

Раздел **Metadata (метаданные** ) настраивает значения, используемые в [файле манифеста пакета NuGet для **nuspec** ](https://docs.microsoft.com/nuget/create-packages/creating-a-package#the-role-and-structure-of-the-nuspec-file).

## <a name="required-information"></a>Требуемая информация

На вкладке **Общие** содержатся четыре поля, которые необходимо указать для создания пакета NuGet.

[![](metadata-images/metadata-general-sml.png "Требуемое окно метаданных пакета NuGet")](metadata-images/metadata-general.png#lightbox)

- **ID** — идентификатор пакета, который должен быть уникальным в пределах NuGet.org (или в любом месте, где будет распространяться пакет). Следуйте этому [руководству](https://docs.microsoft.com/nuget/create-packages/creating-a-package#choosing-a-unique-package-identifier-and-setting-the-version-number) и используйте только символы, допустимые в URL-адресе (без пробелов, и не используйте большинство специальных символов).
- **Версия** — выберите номер версии, совместимый с [правилами управления версиями NuGet](https://docs.microsoft.com/nuget/create-packages/dependency-versions).
- **Авторы** — список имен с разделителями-запятыми.
- **Описание** — обзор компонентов пакета, которые отображаются, когда пользователи выбирают пакет.

> [!NOTE]
> Не забывайте увеличивать номер версии при создании новых версий для распространения в NuGet или других пользователей.

Дополнительные сведения см. в [справочнике по необходимым элементам](https://docs.microsoft.com/nuget/schema/nuspec#required-metadata-elements) для получения дополнительных сведений, а также в этих подробных инструкциях по [выбору уникального идентификатора пакета и заданию номера версии](https://docs.microsoft.com/nuget/create-packages/creating-a-package#choosing-a-unique-package-identifier-and-setting-the-version-number) и [заданию типа пакета](https://docs.microsoft.com/nuget/create-packages/creating-a-package#setting-a-package-type).

> [!IMPORTANT]
> Необходимо указать все поля на этой вкладке. в противном случае появится сообщение об ошибке: _"У проекта нет метаданных NuGet, поэтому пакет NuGet не будет создан. Метаданные пакета NuGet можно указать в разделе метаданных в параметрах проекта "._

## <a name="optional-metadata"></a>Необязательные метаданные

Вкладка **сведения** содержит необязательные поля, которые будут включены в файл манифеста пакета NuGet.

[![](metadata-images/metadata-detail-sml.png "Необязательное окно метаданных пакета NuGet")](metadata-images/metadata-detail.png#lightbox)

Дополнительные сведения о обязательных и необязательных полях см. в [справочнике по дополнительным элементам](https://docs.microsoft.com/nuget/schema/nuspec#optional-metadata-elements) .

> [!NOTE]
> Если пакет NuGet распространяется в [NuGet.org](https://www.nuget.org) , рекомендуется предоставить как можно больше информации.


## <a name="related-links"></a>Связанные ссылки

- [Справочник по файлу NUSPEC](https://docs.microsoft.com/nuget/schema/nuspec#general-form-and-schema)
