---
title: Общие часто задаваемые вопросы
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: C7E6E54D-3957-407D-BB87-22B095148C6B
author: davidortinau
ms.author: daortin
ms.date: 05/08/2018
ms.openlocfilehash: 30827292e59c70395fce17bb520f48b2f19c7d1e
ms.sourcegitcommit: 4691b48f14b166afcec69d1350b769ff5bf8c9f6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/08/2020
ms.locfileid: "75728180"
---
# <a name="general-frequently-asked-questions"></a>Общие часто задаваемые вопросы

## <a name="portable-class-libraries"></a>Переносимые библиотеки классов

### <a name="how-can-i-view-what-libraries-are-supported-in-a-pclpcl-support-librariesmd"></a>[Как можно просмотреть, какие библиотеки поддерживаются в PCL](pcl-support-libraries.md)
В этом разделе перечислены ресурсы и методы, позволяющие определить, поддерживается ли имеющаяся библиотека различными целевыми платформами PCL, или же их можно преобразовать в профиль PCL.

### <a name="pcl-reflection-apipcl-reflectionmd"></a>[API отражения PCL](pcl-reflection.md)
Корпорация Майкрософт разработала новый API отражения для использования в переносимых библиотеках классов. Если имеется какой-либо существующий код отражения, который вы хотите переместить в PCL, он может не работать.

### <a name="pcl-case-study-how-can-i-resolve-problems-related-to-systemdiagnosticstracing-for-the-microsoft-tpl-dataflow-nuget-packagepcl-case-studymd"></a>[Практический пример PCL. Как устранять проблемы, связанные с System.Diagnostics.Tracing для пакета NuGet потока данных библиотеки параллельных задач Майкрософт](pcl-case-study.md)
Xamarin. iOS и Xamarin. Android не реализуют 100% всех профилей PCL, которые они могут использовать в качестве ссылок. Для практического удобства в Visual Studio для Mac, Visual Studio и диспетчере пакетов NuGet проекты Xamarin позволяют использовать несколько профилей, которые имеют только неполные реализации. Например, ни Xamarin. iOS, ни Xamarin. Android в настоящее время не включают полную реализацию типов в `System.Diagnostics.Tracing` пространстве имен PCL. Это можно обойти, переключив проект приложения, чтобы он ссылался на версию библиотеки потоков данных TPL Portable-net45 + Win8 + WP8 + wpa81.

## <a name="nuget-packages--xamarin-components"></a>Пакеты NuGet & компоненты Xamarin
### <a name="how-can-i-update-nugetnuget-updatemd"></a>[Как обновить NuGet](nuget-update.md)
Обновления NuGet, расширения и надстройки можно найти на вкладке **обновления** в **диспетчере пакетов NuGet**. Подробное перемещение по поиску обновлений в Visual Studio для Mac & Visual Studio см. в этом руководством.

### <a name="how-do-i-downgrade-a-nuget-packagenuget-package-downgrademd"></a>[Как перейти на использование более ранней версии пакета NuGet](nuget-package-downgrade.md)
Visual Studio для Mac & Visual Studio имеют функции для выбора старых версий пакетов и их автоматической установки. Аналогично тому, как работает обновление пакетов.

### <a name="missing-packages-error-after-updating-nuget-packagesnuget-packages-missingmd"></a>[Ошибка отсутствующих пакетов после обновления пакетов NuGet](nuget-packages-missing.md)
Эта проблема в основном была указана в примерах решений Xamarin. Forms, но потенциальная проблема может возникнуть в любом проекте, использующем пакеты NuGet.

### <a name="unifying-google-play-services-components-and-nugetgps-components-nugetmd"></a>[Объединение компонентов Сервисов Google Play и NuGet](gps-components-nuget.md)
Использовалось несколько Сервисы Google Play компонентов и пакетов NuGet, но чтобы упростить работу с разработчиками, мы теперь объединены компоненты и пакеты NuGet в два. Почти во всех случаях следует использовать Сервисы Google Play. Единственная причина использования пакета (Фройо) — только в том случае, если вы активно нацеливание на Фройо.

### <a name="where-are-the-components-stored-on-my-machinecomponent-storagemd"></a>[Где на компьютере хранятся компоненты](component-storage.md)
При установке компонента Xamarin в проект приложения он помещается в два расположения, перечисленные в этом разделе.

## <a name="troubleshooting"></a>Диагностика
### <a name="where-can-i-find-my-version-information-and-logsversion-logsmd"></a>[Где я могу найти информацию о версии и журналы](version-logs.md)
В этом разделе содержатся сведения о поиске большинства диагностических сведений, которые можно использовать для устранения проблем с Xamarin.

### <a name="when-and-how-should-i-file-a-bug-reporthowto-file-bugmd"></a>[Когда и как следует указывать отчет об ошибке](howto-file-bug.md)
В этом разделе приводятся советы по созданию отчетов об ошибках высокого качества, чтобы наши инженеры могли определить причину (и любые возможные исправления) для решения проблемы более эффективно.

### <a name="why-isnt-jenkins-supported-by-xamarinxamarin-jenkinsmd"></a>[Почему Xamarin не поддерживает Jenkins](xamarin-jenkins.md)
Jenkins — это набор элементов конфигурации с открытым исходным кодом; из-за этого многие проблемы, непосредственно вызванные *самой* Jenkins, должны быть зарегистрированы как проблемы, связанные с тем, что вы получили код. Например, [главный репозиторий Jenkins](https://github.com/jenkinsci/jenkins)или репозиторий для [Jenkins. app](https://github.com/stisti/jenkins-app).

### <a name="what-project-settings-are-required-for-the-debuggerdebugger-settingsmd"></a>[Какие параметры проекта нужны отладчику](debugger-settings.md)
Чтобы отладчик работал должным образом (точки останова, отображать журналы отладки и т. д.), должны быть включены средства инструментирования и отладочной информации для разработчика. В этом руководство подробно описано, как найти и активировать эти параметры.
