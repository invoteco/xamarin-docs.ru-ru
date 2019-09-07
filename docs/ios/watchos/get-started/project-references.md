---
title: Ссылки на проекты watchOS в Xamarin
description: В этом документе описывается связь между приложением iOS, приложением наблюдения и расширением приложения Watch. В нем рассматриваются ссылки на проекты и идентификаторы пакетов.
ms.prod: xamarin
ms.assetid: C366E062-C33D-406A-B3FF-CBE82E5D1E7E
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 09/13/2016
ms.openlocfilehash: dcadb5146df39aa4887e28b65078acc9454f3d34
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2019
ms.locfileid: "70767984"
---
# <a name="watchos-project-references-in-xamarin"></a>Ссылки на проекты watchOS в Xamarin

_Описание связи между приложением iOS, просмотром приложения и расширением контрольных значений._

Три проекта в решении watchOS *автоматически настраиваются* для ссылки друг на друга, чтобы обеспечить правильное построение и пакет watchOS 3 приложений. Для справки эти ссылки проекта и параметры идентификатора пакета описаны ниже.

## <a name="project-references"></a>Ссылки проекта

Чтобы просмотреть ссылки, дважды щелкните узлы ссылки для каждого проекта:

- Приложение для **iPhone** . ссылки приложения **Watch**

  ![](project-references-images/catalog-reference1.png "Приложение для iPhone. ссылки приложения Watch")

- **Просмотр** ссылок на приложения **Просмотр расширения приложения**

  ![](project-references-images/catalog-reference2.png "Приложение для iPhone. ссылки приложения Watch")

- **Расширение Watch App** не ссылается ни на один из других проектов

  ![](project-references-images/catalog-reference3.png "Контрольное расширение приложения не ссылается на другие проекты")

## <a name="bundle-identifiers"></a>Идентификаторы пакета

Также необходимо убедиться в правильности **идентификаторов пакета** .
Все три проекта должны иметь *один и тот же* префикс идентификатора, при этом два проекта наблюдения имеют предопределенные `watchkitapp`расширения `watchkitextension` и, как показано ниже (для примера **ватчкиткаталог** ):

- Унифицированный проект Xamarin. iOS —`com.xamarin.WatchKitCatalog`

- Проект расширения WatchKit —`com.xamarin.WatchKitCatalog.watchkitextension`

- Просмотр проекта приложения-`com.xamarin.WatchKitCatalog.watchkitapp`

Также убедитесь, что эти параметры **info. plist** верны:

- Проект Watch App `WKCompanionAppBundleIdentifier` соответствует идентификатору пакета родительского приложения или контейнера (IE. тот, который работает на iPhone);

- **Идентификатор пакета WKApp** проекта расширения набора контрольных пакетов соответствует идентификатору пакета проекта приложения Watch.

Идентификаторы можно изменить, дважды щелкнув файл **info. plist** в каждом проекте.

На этом снимке экрана показан файл info. plist **расширения Watch** , где также отображается идентификатор **приложения для просмотра** .

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio для Mac](#tab/macos)

![](project-references-images/infoplist-extension.png "На этом снимке экрана показан файл info. plist расширения Watch.")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![](project-references-images/infoplist-extension-vs.png "На этом снимке экрана показан файл info. plist расширения Watch.")

-----

На этом снимке экрана показан файл info. plist **приложения Watch** .
Текущая версия **ОС контрольных значений** — 8,2, поэтому **целевой объект развертывания** для приложения Watch должен быть равен **8,2**. Обратите внимание, что при наличии установленного Xcode 6,3 это значение может быть равно 8,3. Вы должны изменить его на 8,2.

![](project-references-images/infoplist-watchapp.png "Файл Watch info. plist")

Целевой объект развертывания для приложения Watch может отличаться от расширения Watch и приложения iOS.
