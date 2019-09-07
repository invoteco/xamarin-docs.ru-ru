---
title: Устранение ошибок установки библиотеки
description: В некоторых случаях при установке библиотек поддержки Android могут возникать ошибки. В этом разделе приводятся обходные пути для некоторых распространенных ошибок.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 2AE68ACE-8496-445D-BF17-5E4097D4AE35
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/14/2018
ms.openlocfilehash: 8107a26e090aa920d71146d5f2af8b8365697d6b
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2019
ms.locfileid: "70757209"
---
# <a name="resolving-library-installation-errors"></a>Устранение ошибок установки библиотеки

_В некоторых случаях при установке библиотек поддержки Android могут возникать ошибки. В этом разделе приводятся обходные пути для некоторых распространенных ошибок._

## <a name="overview"></a>Обзор

При построении проекта приложения Xamarin. Android могут возникнуть ошибки сборки, когда Visual Studio или Visual Studio для Mac пытаются загрузить и установить библиотеки зависимостей. Многие из этих ошибок вызваны проблемами с сетевым подключением, повреждением файлов или проблемами управления версиями. В этом руководстве описаны наиболее распространенные ошибки установки библиотеки поддержки и приведены инструкции по решению этих проблем и повторному созданию проекта приложения. 

## <a name="errors-while-downloading-m2repository"></a>Ошибки при скачивании m2Repository

При ссылке на пакет NuGet библиотек поддержки Android или служб Google Play можно столкнуться с ошибками **m2repository** . Сообщение об ошибке выглядит следующим образом:

```shell
Download failed. Please download https://dl-ssl.google.com/android/repository/android_m2repository_r16.zip and extract it to the C:\Users\mgm\AppData\Local\Xamarin\Android.Support.v4\22.2.1\content directory.
```

Этот пример предназначен для **Android\_m2repository\_R16**, но вы можете увидеть такое же сообщение об ошибке для другой версии, **например\_Android\_m2repository R18** или **Android\_ m2repository\_R25**. 

### <a name="automatic-recovery-from-m2repository-errors"></a>Автоматическое восстановление из m2repository ошибок 

Часто эту проблему можно исправлено, удалив библиотеку проблем и перестроив ее в соответствии с приведенными ниже шагами. 

1. Перейдите в каталог библиотеки поддержки на своем компьютере:

    - В Windows библиотеки поддержки находятся в папке **C\\: Users\\_username_\\AppData\\Local\\Xamarin**. 

    - В Mac OS X библиотеки поддержки расположены по адресу **/Users/_username_/.локал/шаре/ксамарин**. 

2. Определение папки библиотеки и версии, соответствующей сообщению об ошибке. Например, папка библиотеки и версии для приведенного выше сообщения об ошибке находится в **Android. support. v4\\22.2.1**:

    [![Пример расположения папки для библиотеки поддержки 22.2.1](resolving-library-installation-errors-images/01-example-location.png)](resolving-library-installation-errors-images/01-example-location.png#lightbox)

3. Удалите содержимое папки версии. Не забудьте удалить **ZIP** -файл, а также **содержимое** и **вложенные** подкаталоги в этой папке. В примере сообщения об ошибке, приведенном выше, файлы и подкаталоги, показанные на этом снимке экрана (**содержимое**, **внедренные**и **android_m2repository_r16. zip**), будут удалены:

    [![Пример содержимого папки библиотеки поддержки 22.2.1](resolving-library-installation-errors-images/02-example-folder-vs.png)](resolving-library-installation-errors-images/02-example-folder-vs.png#lightbox)

   Обратите внимание, что важно удалить *все* содержимое этой папки. Хотя эта папка изначально может содержать файл "Missing **"\_Android\_m2repository R16. zip** , этот файл может быть частично скачан или поврежден.

4. Перестроение проекта &ndash; . это приведет к повторной загрузке отсутствующей библиотеки в процессе сборки.

В большинстве случаев эти действия позволят устранить ошибку сборки и продолжить работу. Если удаление этой библиотеки не приводит к устранению ошибки сборки, необходимо вручную скачать и установить файл **Android\_m2repository\_r_nn_. zip** , как описано в следующем разделе. 

### <a name="manually-downloading-m2repository"></a>Загрузка m2repository вручную

Если вы попытались использовать описанные выше шаги автоматического восстановления и все еще имели ошибки сборки, можно вручную скачать файл **Android\_m2repository\_r_nn_. zip** (с помощью веб-браузера) и установить его в соответствии со следующими шагами. . Эта процедура также полезна, если у вас нет доступа к Интернету на компьютере разработчика, но вы можете скачать архив с другого компьютера. 

1. Скачайте файл **Android\_m2repository\_r_nn_. zip** , который соответствует ссылкам на сообщения &ndash; об ошибках, приведен в следующем списке (вместе с соответствующим хэшем MD5 для URL-адреса ссылки):

    - [Android\_m2repository\_R33. zip](https://dl-ssl.google.com/android/repository/android_m2repository_r33.zip) &ndash; 5FB756A25962361D17BBE99C3B3FCC44

    - [Android\_m2repository\_R32. zip](https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip) &ndash; F16A3455987DBAE5783F058F19F7FCDF

    - [Android\_m2repository\_R31. zip](https://dl-ssl.google.com/android/repository/android_m2repository_r31.zip) &ndash; 99A8907CE2324316E754A95E4C2D786E

    - [Android\_m2repository\_R30. zip](https://dl-ssl.google.com/android/repository/android_m2repository_r30.zip) &ndash; 05AD180B8BDC7C21D6BCB94DDE7F2C8F

    - [Android\_m2repository\_R29. zip](https://dl-ssl.google.com/android/repository/android_m2repository_r29.zip) &ndash; 2A3A8A6D6826EF6CC653030E7D695C41

    - [Android\_m2repository\_R28. zip](https://dl-ssl.google.com/android/repository/android_m2repository_r28.zip) &ndash; 17BE247580748F1EDB72E9F374AA0223

    - [Android\_m2repository\_R27. zip](https://dl-ssl.google.com/android/repository/android_m2repository_r27.zip) &ndash; C9FD4FCD69D7D12B1D9DF076B7BE4E1C

    - [Android\_m2repository\_R26. zip](https://dl-ssl.google.com/android/repository/android_m2repository_r26.zip) &ndash; 8157FC1C311BB36420C1D8992AF54A4D

    - [Android\_m2repository\_R25. zip](https://dl-ssl.google.com/android/repository/android_m2repository_r25.zip) &ndash; 0B3F1796C97C707339FB13AE8507AF50

    - [Android\_m2repository\_R24. zip](https://dl-ssl.google.com/android/repository/android_m2repository_r24.zip) &ndash; 8E3C9EC713781EDFE1EFBC5974136BEA

    - [android\_m2repository\_r23.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r23.zip) &ndash; D5BB66B3640FD9B9C6362C9DB5AB0FE7

    - [Android\_m2repository\_R22. zip](https://dl-ssl.google.com/android/repository/android_m2repository_r22.zip) &ndash; 96659D653BDE0FAEDB818170891F2BB0

    - [Android\_m2repository\_R21. zip](https://dl-ssl.google.com/android/repository/android_m2repository_r21.zip) &ndash; CD3223F2EFE068A26682B9E9C4B6FBB5

    - [Android\_m2repository\_R20. zip](https://dl-ssl.google.com/android/repository/android_m2repository_r20.zip) &ndash; 650E58DF02DB1A832386FA4A2DE46B1A

    - [Android\_m2repository\_R19. zip](https://dl-ssl.google.com/android/repository/android_m2repository_r19.zip) &ndash; 263B062D6EFAA8AEE39E9460B8A5851A

    - [Android\_m2repository\_R18. zip](https://dl-ssl.google.com/android/repository/android_m2repository_r18.zip) &ndash; 25947AD38DCB4865ABEB61522FAFDA0E

    - [Android\_m2repository\_R17. zip](https://dl-ssl.google.com/android/repository/android_m2repository_r17.zip) &ndash; 49054774F44AE5F35A6BA9D3C117EFD8

    - [Android\_m2repository\_R16. zip](https://dl-ssl.google.com/android/repository/android_m2repository_r16.zip) &ndash; 0595E577D19D31708195A83087881EE6

    Если архив **m2repository** не показан в этой таблице, можно создать URL-адрес для скачивания, добавив **https://dl-ssl.google.com/android/repository/** в него имя **m2repository** для скачивания. Например, используйте **https://dl-ssl.google.com/android/repository/android\_m2repository\_r10.zip** для загрузки **Android\_ m2repository\_ R10. zip**.

2. Переименуйте файл в соответствующий хэш MD5 URL-адреса скачивания, как показано в приведенной выше таблице. Например, если вы скачали **файл\_Android\_m2repository R25. zip**, переименуйте его в **0B3F1796C97C707339FB13AE8507AF50. zip**. Если хэш MD5 для URL-адреса скачивания скачанного файла не показан в таблице, можно использовать [Генератор MD5 в Интернете](http://www.webconfs.com/online-md5-generator.php) для преобразования URL-адреса в хэш-строку MD5. 

3. Скопируйте файл в папку Xamarin **zips** : 

    - В Windows эта папка находится в папке **C:\\Users\\***username***\\AppData\\Local\\Xamarin\\zips**. 

    - На Mac OS X эта папка находится по адресу **/Users/***имя_пользователя***/.локал/шаре/ксамарин/зипс**. 

    Например, на следующем снимке экрана показан результат загрузки **Android\_m2repository\_R16. zip** и его переименования в хэш MD5 своего URL-адреса скачивания в Windows:

    [![Пример переименования репозитория R16. zip в 0595E577D19D31708195A83087881EE6. zip](resolving-library-installation-errors-images/03-md5-rename-vs.png)](resolving-library-installation-errors-images/03-md5-rename-vs.png#lightbox)

Если эта процедура не позволяет устранить ошибку сборки, необходимо вручную скачать файл **Android\_m2repository\_r_nn_. zip** , распаковать его и установить его содержимое, как описано в следующем разделе. 

### <a name="manually-downloading-and-installing-m2repository-files"></a>Загрузка и установка файлов m2repository вручную

Полная ручная процедура восстановления после ошибок **m2repository** влечет за собой загрузку файла **Android\_m2repository\_r_nn_. zip** (с помощью веб-браузера), его распаковку и копирование содержимого в службу поддержки Каталог библиотеки на компьютере. В следующем примере будет восстановлено следующее сообщение об ошибке: 

```shell
Unzipping failed. Please download https://dl-ssl.google.com/android/repository/android_m2repository_r25.zip and extract it to the C:\Users\mgm\AppData\Local\Xamarin\Android.Support.v4\23.1.1\content directory.
```

Чтобы скачать **m2repository** и установить его содержимое, выполните следующие действия:

1. Удалите содержимое папки Library, соответствующей сообщению об ошибке. Например, в приведенном выше сообщении об ошибке будет удалено содержимое файла **C\\:\\Users\\***username***\\AppData\\Local\\Xamarin Android. support. v423.1.1.0\\** . 
    Как было описано ранее, необходимо удалить все содержимое этого каталога:

    [![Удаление содержимого, внедренных и android_m2repository папок из папки 23.1.1.0](resolving-library-installation-errors-images/04-delete-contents-vs.png)](resolving-library-installation-errors-images/04-delete-contents-vs.png#lightbox)

2. Скачайте файл **Android\_m2repository\_r_nn_. zip** из Google, соответствующий сообщению об ошибке (см. таблицу в предыдущем разделе для ссылок).

3. Извлеките этот **ZIP** -Архив в любое расположение (например, на Рабочий стол). При этом следует создать каталог, соответствующий имени **ZIP** -архива. В этом каталоге должен находиться подкаталог с именем **m2repository**: 

    [![Папка m2repository найдена в извлеченном ZIP-архиве](resolving-library-installation-errors-images/05-m2repository-vs.png)](resolving-library-installation-errors-images/05-m2repository-vs.png#lightbox)

4. В каталоге библиотеки с версией, очищенном на шаге 1, повторно создайте **содержимое** и **внедренные** подкаталоги. Например, на следующем снимке экрана показано **содержимое** и **внедренные** подкаталоги, создаваемые в папке **23.1.1.0** для **Android\_m2repository\_R25. zip**: 

    [![Создание содержимого и внедренных папок в папке 23.1.1.0](resolving-library-installation-errors-images/06-recreate-folders-vs.png)](resolving-library-installation-errors-images/06-recreate-folders-vs.png#lightbox)

5. Скопируйте **m2repository** из извлеченного **ZIP** -файла в каталог **содержимого** , созданный на предыдущем шаге. 

    [![Снимок экрана m2repository, скопированный в папку 23.1.1.0/Content](resolving-library-installation-errors-images/07-copied-m2repository-vs.png)](resolving-library-installation-errors-images/07-copied-m2repository-vs.png#lightbox)

6. В извлеченном **ZIP** -каталоге перейдите к **m2repository\\com\\поддержка поддержки\\Android\\— v4** и откройте папку, соответствующую номеру версии, созданному выше (в этом примере — **23.1.1**):

    [![Пример списка файлов, содержащихся в папке Support-v4/23.1.1](resolving-library-installation-errors-images/08-zip-contents-vs.png)](resolving-library-installation-errors-images/08-zip-contents-vs.png#lightbox)

7. Скопируйте все файлы из этой папки в **внедренный** каталог, созданный на шаге 4.

    [![Пример файлов, скопированных в папку 23.1.1.0/Embedded](resolving-library-installation-errors-images/09-copied-vs.png)](resolving-library-installation-errors-images/09-copied-vs.png#lightbox)

8. Убедитесь, что все файлы скопированы. **Внедренный** каталог теперь должен содержать такие файлы, как **. jar**, **. AAR**и **. POM**.

9. Распакуйте содержимое всех извлеченных файлов **. AAR** во **внедренный** каталог. В Windows добавьте расширение **ZIP** в файл **AAR** , откройте его и скопируйте содержимое в **внедренный** каталог.
    В macOS Распакуйте файл **AAR** с помощью команды **unzip** в терминале (например, **unzip File. AAR**).

На этом этапе вы вручную установили недостающие компоненты, и проект должен быть построен без ошибок. Если это не так, убедитесь, что вы скачали версию архива **m2repository** **. zip** , соответствующую версии в сообщении об ошибке, и убедитесь, что ее содержимое установлено в нужных расположениях, как описано в описанных выше шагах. 

## <a name="summary"></a>Сводка 

В этой статье объясняется, как устранить распространенные ошибки, которые могут происходить во время автоматической загрузки и установки библиотек зависимостей. В нем описано, как удалить библиотеку проблем и перестроить проект в качестве способа повторной загрузки и повторной установки библиотеки. В нем описано, как скачать библиотеку и установить ее в папку **zips** . В нем также описана более обязательная процедура загрузки и установки необходимых файлов вручную в качестве способа устранения проблем, которые не могут быть разрешены с помощью автоматических средств. 
