---
title: Почему корпорация Майкрософт не поддерживает Jenkins?
description: В этом документе на высоком уровне описывается взаимодействие Xamarin с системой Jenkins CI. В нем также рассматриваются некоторые распространенные проблемы, которые возникают при работе с Jenkins.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 9951F980-2C6C-47C0-8A35-A78F06C20BEB
author: conceptdev
ms.author: crdun
ms.date: 06/05/2018
ms.openlocfilehash: a8dc27574dc9959cc375a98fc0d7a18aac8bd6b7
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2019
ms.locfileid: "70756965"
---
# <a name="why-isnt-jenkins-supported-by-microsoft"></a>Почему корпорация Майкрософт не поддерживает Jenkins?

## <a name="jenkins-support-explanation"></a>Описание поддержки Jenkins

Jenkins — это набор элементов конфигурации с открытым исходным кодом; из-за этого многие проблемы, непосредственно вызванные *самой* Jenkins, должны быть зарегистрированы как проблемы, связанные с тем, что вы получили код. Например, [главный репозиторий Jenkins](https://github.com/jenkinsci/jenkins)или репозиторий для [Jenkins. app](https://github.com/stisti/jenkins-app).

Исключением из этого является проблема, которая может быть изолирована с конкретными ошибками в средствах Xamarin. Если вы подозреваете, что можете проверить [варианты поддержки](~/cross-platform/troubleshooting/support-options.md), то проблема может быть вызвана тем, что Группа поддержки Xamarin может *напрямую* помочь.

## <a name="setup-jenkins-with-xamarin"></a>Настройка Jenkins с помощью Xamarin

Хотя, как отмечалось выше, Jenkinsные проблемы не поддерживаются непосредственно нашей командой; [Использование Jenkins с](~/tools/ci/jenkins-walkthrough.md) руководством по Xamarin можно использовать для настройки сервера Jenkins CI, интегрированного с Xamarin. 

## <a name="fixes-for-common-issues"></a>Исправления распространенных проблем

### <a name="jenkins-is-unable-to-find-the-android-sdk"></a>Jenkins не удалось найти пакет SDK для Android

Сообщение об ошибке для этой проблемы выглядит примерно так:

> Ошибка XA5205: Не удалось найти пакет SDK для Android каталог. Задайте с помощью параметра/p: Андроидсдкдиректори

Параметры для настройки расположения пакета SDK могут различаться в зависимости от используемого подключаемого модуля Android Jenkins. в разделе Руководство по подключаемому модулю можно найти сведения о том, как это сделать. Например, [подключаемый модуль Android Emulator](https://wiki.jenkins-ci.org/display/JENKINS/Android+Emulator+Plugin#AndroidEmulatorPlugin-Systemconfiguration) автоматически ищет пакет SDK, но если его не удается найти, расположение также можно задать с помощью страницы конфигурации системы Jenkins для этого подключаемого модуля. 

## <a name="deprecated-errors"></a>Устаревшие ошибки

> [!IMPORTANT]
> Эта проблема решена в последних версиях Xamarin. Однако если проблема возникает в последней версии программного обеспечения, запишите [новую ошибку](~/cross-platform/troubleshooting/questions/howto-file-bug.md) с полными сведениями о версии и полным выходным файлом журнала сборки.

### <a name="jenkins-reports-an-invalid-xamarin-license"></a>Jenkins сообщает о недопустимой лицензии Xamarin
Сообщения об ошибках для этой проблемы обычно выглядят примерно так:

> Ошибка XA9008: Для сборки из командной строки требуется бизнес-лицензия

или диспетчер конфигурации служб

> Ошибка: Выпуск Starter. iOS не поддерживает сборку за пределами Xamarin Studio 

Наиболее распространенной причиной этого сценария является использование Jenkins путем входа в систему с учетной записью пользователя, не связанной с лицензией Xamarin. Самый простой способ решения этой проблемы — установить Jenkins как приложение непосредственно через учетную запись пользователя. Этот процесс и некоторые дополнительные вопросы описаны здесь:[https://forums.xamarin.com/discussion/comment/99397/#Comment_99397](https://forums.xamarin.com/discussion/comment/99397/#Comment_99397)
