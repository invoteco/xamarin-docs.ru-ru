---
title: Инструкции по настройке брандмауэра Xamarin
description: Этот документ содержит список узлов, которые нужно включить в список разрешенных в брандмауэре, чтобы обеспечить работу Xamarin в корпоративной среде.
ms.prod: xamarin
ms.assetid: 658f699b-8cca-48f7-ae54-fa956384b6d6
author: conceptdev
ms.author: crdun
ms.date: 07/17/2019
ms.openlocfilehash: 2b52dfd55194ec076f28f8c33e758a39d14f5943
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/13/2020
ms.locfileid: "70291334"
---
# <a name="xamarin-firewall-configuration-instructions"></a>Инструкции по настройке брандмауэра Xamarin

_В этой статье приводится перечень узлов, которые нужно включить в список разрешенных для брандмауэра, чтобы использовать платформу Xamarin в организации._

Для надлежащей установки и работы продуктов Xamarin должны быть доступны определенные конечные точки для скачивания необходимых средств и обновлений программного обеспечения. Если у вас или в вашей организации заданы строгие параметры брандмауэра, могут возникать проблемы с установкой, лицензированием, работой компонентов и т. д. В этом документе описываются некоторые конечные точки, которые следует включить в список разрешенных для брандмауэра, чтобы обеспечить функционирование Xamarin. В этом списке отсутствуют конечные точки, необходимые для сторонних средств, включенных в скачиваемые файлы. Если при использовании приведенных в этом списке адресов у вас по-прежнему возникают проблемы, см. руководства по устранению неполадок установки Apple или Android.

## <a name="endpoints-to-allow"></a>Конечные точки, которые требуется разрешить

### <a name="xamarin-installer"></a>Установщик Xamarin

Для правильной установки программного обеспечения с помощью последнего выпуска установщика Xamarin необходимо добавить следующие известные адреса:

- xamarin.com (манифесты установщика)
- dl.xamarin.com (расположение для скачивания пакета)
- dl.google.com (для скачивания пакета SDK для Android)
- download.oracle.com (JDK)
- visualstudio.com (расположение для скачивания пакетов установки)
- go.Microsoft.com (разрешение URL-адреса установки)
- aka.ms (разрешение URL-адреса установки)

Если вы используете компьютер Mac и сталкиваетесь с проблемами установки Xamarin.Android, проверьте, что ваша система macOS может скачивать Java.

### <a name="nuget-including-xamarinforms"></a>NuGet (включая Xamarin.Forms)

Для доступа к NuGet (набор Xamarin.Forms упакован в виде NuGet) потребуется добавить следующие адреса:

- www.nuget.org (для доступа к NuGet)
- globalcdn.nuget.org (скачиваемые компоненты NuGet)
- dl-ssl.google.com (компоненты Google для Android и Xamarin.Forms)

### <a name="software-updates"></a>Обновления программного обеспечения

Чтобы обеспечить правильное скачивание обновлений программного обеспечения, необходимо добавить следующие адреса:

- software.xamarin.com (служба средства обновления)
- download.visualstudio.microsoft.com
- dl.xamarin.com

## <a name="xamarin-mac-agent"></a>Xamarin Mac Agent

Чтобы подключить Visual Studio к узлу сборки Mac с помощью Xamarin Mac Agent, требуется открытый порт SSH. По умолчанию используется **порт 22**.
