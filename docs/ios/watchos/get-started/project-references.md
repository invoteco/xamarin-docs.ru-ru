---
title: Ссылки на проекты watchOS в Xamarin
description: В этом документе описывается связь между приложением iOS, приложением наблюдения и расширением приложения Watch. В нем рассматриваются ссылки на проекты и идентификаторы пакетов.
ms.prod: xamarin
ms.assetid: C366E062-C33D-406A-B3FF-CBE82E5D1E7E
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/13/2016
ms.openlocfilehash: 3dcd5f17b35b9829831adcf997d8bde97c0572e7
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73030158"
---
# <a name="watchos-project-references-in-xamarin"></a>Ссылки на проекты watchOS в Xamarin

_Описание связи между приложением iOS, просмотром приложения и расширением контрольных значений._

Три проекта в решении watchOS *автоматически настраиваются* для ссылки друг на друга, чтобы обеспечить правильное построение и пакет watchOS 3 приложений. Для справки эти ссылки проекта и параметры идентификатора пакета описаны ниже.

## <a name="project-references"></a>Ссылки проекта

Чтобы просмотреть ссылки, дважды щелкните узлы ссылки для каждого проекта:

- Приложение для **iPhone** . ссылки приложения **Watch**

  ![](project-references-images/catalog-reference1.png "iPhone app references Watch App")

- **Просмотр** ссылок на приложения **Просмотр расширения приложения**

  ![](project-references-images/catalog-reference2.png "iPhone app references Watch App")

- **Расширение Watch App** не ссылается ни на один из других проектов

  ![](project-references-images/catalog-reference3.png "Watch App Extension does not reference the other projects")

## <a name="bundle-identifiers"></a>Идентификаторы пакета

Также необходимо убедиться в правильности **идентификаторов пакета** .
Все три проекта должны иметь *одинаковый* префикс идентификатора, два из которых имеют предопределенные расширения `watchkitextension` и `watchkitapp`, как показано ниже (для примера **ватчкиткаталог** ):

- Унифицированный проект Xamarin. iOS — `com.xamarin.WatchKitCatalog`

- Проект расширения WatchKit — `com.xamarin.WatchKitCatalog.watchkitextension`

- Просмотр проекта приложения — `com.xamarin.WatchKitCatalog.watchkitapp`

Также убедитесь, что эти параметры **info. plist** верны:

- `WKCompanionAppBundleIdentifier` проекта контрольного приложения соответствует ИДЕНТИФИКАТОРу пакета родительского приложения или контейнера (Internet Explorer), который выполняется на iPhone.

- **Идентификатор пакета WKApp** проекта расширения набора контрольных пакетов соответствует идентификатору пакета проекта приложения Watch.

Идентификаторы можно изменить, дважды щелкнув файл **info. plist** в каждом проекте.

На этом снимке экрана показан файл info. plist **расширения Watch** , где также отображается идентификатор **приложения для просмотра** .

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio для Mac](#tab/macos)

![](project-references-images/infoplist-extension.png "This screenshot is the Watch Extension's Info.plist file")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![](project-references-images/infoplist-extension-vs.png "This screenshot is the Watch Extension's Info.plist file")

-----

На этом снимке экрана показан файл info. plist **приложения Watch** .
Текущая версия **ОС контрольных значений** — 8,2, поэтому **целевой объект развертывания** для приложения Watch должен быть равен **8,2**. Обратите внимание, что при наличии установленного Xcode 6,3 это значение может быть равно 8,3. Вы должны изменить его на 8,2.

![](project-references-images/infoplist-watchapp.png "The watch Info.plist file")

Целевой объект развертывания для приложения Watch может отличаться от расширения Watch и приложения iOS.
