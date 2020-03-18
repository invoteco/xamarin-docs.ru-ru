---
title: Устранение ошибок установки библиотеки
description: В некоторых случаях при установке вспомогательных библиотек Android могут возникать ошибки. В этом руководстве представлены обходные пути для устранения некоторых типичных ошибок.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 2AE68ACE-8496-445D-BF17-5E4097D4AE35
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/14/2018
ms.openlocfilehash: e99e12aa73374cc8145fad0eb5aad7f3259e0ca2
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/10/2020
ms.locfileid: "76724303"
---
# <a name="resolving-library-installation-errors"></a>Устранение ошибок установки библиотеки

_В некоторых случаях при установке вспомогательных библиотек Android могут возникать ошибки. В этом руководстве представлены обходные пути для устранения некоторых типичных ошибок._

## <a name="overview"></a>Обзор

При сборке проекта приложения Xamarin.Android могут возникать ошибки сборки, когда Visual Studio или Visual Studio для Mac пытаются скачать и установить библиотеки зависимостей. Многие из этих ошибок вызваны проблемами с сетевым подключением, повреждением файлов или проблемами с управлением версиями. В этом руководстве описаны несколько распространенных ошибок при установке вспомогательных библиотек и приведены обходные решения для этих проблем, которые позволят продолжить сборку проекта приложения.

## <a name="errors-while-downloading-m2repository"></a>Ошибки при скачивании из m2Repository

Указав ссылку на пакет NuGet для вспомогательных библиотек Android или службы Google Play, вы можете увидеть ошибки с репозиторием **m2repository**. Сообщение об ошибке выглядит следующим образом:

```shell
Download failed. Please download https://dl-ssl.google.com/android/repository/android_m2repository_r16.zip and extract it to the C:\Users\mgm\AppData\Local\Xamarin\Android.Support.v4\22.2.1\content directory.
```

Этот пример описывает использование **Android\_m2repository\_r16**, но аналогичное сообщение об ошибке может появляться и для другой версии, например **Android\_m2repository\_r18** или **Android\_m2repository\_r25**.

### <a name="automatic-recovery-from-m2repository-errors"></a>Автоматические восстановление после ошибок с m2repository

Часто эту проблему можно исправить, удалив библиотеку, с которой возникли проблемы, и повторив сборку по следующей процедуре.

1. Перейдите в каталог вспомогательной библиотеки на локальном компьютере.

    - В Windows вспомогательные библиотеки расположены в папке **C:\\Users\\_имя_пользователя_\\AppData\\Local\\Xamarin**.

    - В Mac OS X вспомогательные библиотеки расположены в папке **/Users/_имя_пользователя_/.local/share/Xamarin**.

2. Найдите папку, соответствующую библиотеке и версии, которые указаны в сообщении об ошибке. Например, для библиотеки и версии из приведенного выше сообщения об ошибке это будет папка **Android.Support.v4\\22.2.1**:

    [![Пример расположения папки для вспомогательной библиотеки 22.2.1](resolving-library-installation-errors-images/01-example-location.png)](resolving-library-installation-errors-images/01-example-location.png#lightbox)

3. Удалите содержимое папки этой версии. Не забудьте удалить **ZIP-файл**, а также вложенные каталоги **content** и **embedded** из этой папки. Для приведенного выше сообщения об ошибке нужно удалить файлы и подкаталоги, показанные на этом снимке экрана (**content**, **embedded** и **android_m2repository_r16.zip**):

    [![Пример содержимого папки вспомогательной библиотеки 22.2.1](resolving-library-installation-errors-images/02-example-folder-vs.png)](resolving-library-installation-errors-images/02-example-folder-vs.png#lightbox)

   Здесь очень важно *полностью* удалить все содержимое папки. Даже если в папке есть "недостающий" файл **android\_m2repository\_r16.zip**, он может быть скачан частично или поврежден.

4. Повторная сборка проекта &ndash; приведет к повторному скачиванию отсутствующей библиотеки.

В большинстве случаев эти действия позволят устранить ошибку сборки и продолжить работу. Если удаление библиотеки не устранит ошибку сборки, следует вручную скачать и установить файл **android\_m2repository\_r_nn_.zip**, как описано в следующем разделе.

### <a name="manually-downloading-m2repository"></a>Скачивание из m2repository вручную

Если вы уже применили описанные выше шаги автоматического восстановления, но по-прежнему получаете ошибки сборки, можно вручную скачать файл **android\_m2repository\_r_nn_.zip** (через веб-браузер) и установить его по приведенной ниже инструкции. Эта процедура также полезна, если у вас нет доступа к Интернету на компьютере разработчика, но вы можете скачать архив с другого компьютера.

1. Скачайте файл **android\_m2repository\_r_nn_.zip**, соответствующий сообщению об ошибке. В следующем списке представлены ссылки с соответствующим хэшем MD5 для каждого URL-адреса:

    - [android\_m2repository\_r33.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r33.zip) &ndash; 5FB756A25962361D17BBE99C3B3FCC44

    - [android\_m2repository\_r32.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip) &ndash; F16A3455987DBAE5783F058F19F7FCDF

    - [android\_m2repository\_r31.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r31.zip) &ndash; 99A8907CE2324316E754A95E4C2D786E

    - [android\_m2repository\_r30.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r30.zip) &ndash; 05AD180B8BDC7C21D6BCB94DDE7F2C8F

    - [android\_m2repository\_r29.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r29.zip) &ndash; 2A3A8A6D6826EF6CC653030E7D695C41

    - [android\_m2repository\_r28.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r28.zip) &ndash; 17BE247580748F1EDB72E9F374AA0223

    - [android\_m2repository\_r27.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r27.zip) &ndash; C9FD4FCD69D7D12B1D9DF076B7BE4E1C

    - [android\_m2repository\_r26.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r26.zip) &ndash; 8157FC1C311BB36420C1D8992AF54A4D

    - [android\_m2repository\_r25.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r25.zip) &ndash; 0B3F1796C97C707339FB13AE8507AF50

    - [android\_m2repository\_r24.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r24.zip) &ndash; 8E3C9EC713781EDFE1EFBC5974136BEA

    - [android\_m2repository\_r23.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r23.zip) &ndash; D5BB66B3640FD9B9C6362C9DB5AB0FE7

    - [android\_m2repository\_r22.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r22.zip) &ndash; 96659D653BDE0FAEDB818170891F2BB0

    - [android\_m2repository\_r21.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r21.zip) &ndash; CD3223F2EFE068A26682B9E9C4B6FBB5

    - [android\_m2repository\_r20.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r20.zip) &ndash; 650E58DF02DB1A832386FA4A2DE46B1A

    - [android\_m2repository\_r19.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r19.zip) &ndash; 263B062D6EFAA8AEE39E9460B8A5851A

    - [android\_m2repository\_r18.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r18.zip) &ndash; 25947AD38DCB4865ABEB61522FAFDA0E

    - [android\_m2repository\_r17.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r17.zip) &ndash; 49054774F44AE5F35A6BA9D3C117EFD8

    - [android\_m2repository\_r16.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r16.zip) &ndash; 0595E577D19D31708195A83087881EE6

    Если в этой таблице нет нужного репозитория **m2repository**, вы можете создать нужный URL-адрес для скачивания самостоятельно, добавив префикс `https://dl-ssl.google.com/android/repository/` к имени нужного репозитория **m2repository**. Например, адрес **https://dl-ssl.google.com/android/repository/android\_m2repository\_r10.zip** для скачивания **android\_ m2repository\_ r10.zip**.

2. Переименуйте файл в соответствующий хэш MD5 для URL-адреса скачивания, как показано в приведенной выше таблице. Например, если вы скачали **android\_m2repository\_r25.zip**, переименуйте его в **0B3F1796C97C707339FB13AE8507AF50.zip**. Если хэш MD5 для нужного URL-адреса не представлен в этой таблице, можно преобразовать URL-адрес в хэш-строку MD5 с помощью [онлайн-генератора MD5](http://www.webconfs.com/online-md5-generator.php).

3. Скопируйте этот файл в папку **zips** для Xamarin.

    - В Windows эта папка расположена по адресу **C:\\Users\\***имя_пользователя***\\AppData\\Local\\Xamarin\\zips**.

    - В Mac OS X эта папка расположена по адресу **/Users/***имя_пользователя***/.local/share/Xamarin/zips**.

    Например, на следующем снимке экрана показано состояние после скачивания **android\_m2repository\_r16.zip** и переименования в соответствующий хэш MD5 на платформе Windows:

    [![Пример репозитория r16.zip, переименованного в 0595E577D19D31708195A83087881EE6.zip](resolving-library-installation-errors-images/03-md5-rename-vs.png)](resolving-library-installation-errors-images/03-md5-rename-vs.png#lightbox)

Если эта процедура не устранит ошибку сборки, следует вручную скачать и установить файл **android\_m2repository\_r_nn_.zip**, распаковать его и установить содержимое, как описано в следующем разделе.

### <a name="manually-downloading-and-installing-m2repository-files"></a>Скачивание и установка файлов m2repository вручную

Чтобы выполнить процедуру устранения ошибок с **m2repositor** полностью вручную, скачайте файл **android\_m2repository\_r_nn_.zip** (через веб-браузер), распакуйте его и скопируйте содержимое в каталог вспомогательной библиотеки на локальном компьютере. В следующем примере мы устраняем такое сообщение об ошибке:

```shell
Unzipping failed. Please download https://dl-ssl.google.com/android/repository/android_m2repository_r25.zip and extract it to the C:\Users\mgm\AppData\Local\Xamarin\Android.Support.v4\23.1.1\content directory.
```

Чтобы скачать репозиторий **m2repository** и установить его содержимое, выполните следующие действия.

1. Удалите содержимое папки, которая соответствует библиотеке и версии, указанным в сообщении об ошибке. Например, для приведенного выше сообщения об ошибке следует удалить содержимое **C:\\Users\\***имя_пользователя***\\AppData\\Local\\Xamarin\\Android.Support.v4\\23.1.1.0**.
    Как описано ранее, важно полностью удалить все содержимое этого каталога:

    [![Удаление папок content, embedded и android_m2repository из папки 23.1.1.0](resolving-library-installation-errors-images/04-delete-contents-vs.png)](resolving-library-installation-errors-images/04-delete-contents-vs.png#lightbox)

2. Скачайте из Google файл **android\_m2repository\_r_nn_.zip**, соответствующий сообщению об ошибке (ссылки представлены в таблице в предыдущем разделе).

3. Извлеките **ZIP**-архив в любое расположение (например, на рабочий стол). При этом будет создан каталог, соответствующий имени **ZIP**-архива. В этом каталоге найдите подкаталог с именем **m2repository**:

    [![Папка m2repository в извлеченном ZIP-архиве](resolving-library-installation-errors-images/05-m2repository-vs.png)](resolving-library-installation-errors-images/05-m2repository-vs.png#lightbox)

4. В каталоге библиотеки с версией, которую вы удаляли на шаге 1, повторно создайте содержимое подкаталогов **content** и **embedded**. Например, на следующем снимке экрана представлены подкаталоги **content** и **embedded**, созданные в папке **23.1.1.0** для файла **android\_m2repository\_r25.zip**:

    [![Создание папок content и embedded в папке 23.1.1.0](resolving-library-installation-errors-images/06-recreate-folders-vs.png)](resolving-library-installation-errors-images/06-recreate-folders-vs.png#lightbox)

5. Скопируйте **m2repository** из извлеченного **ZIP**-архива в каталог **content**, созданный на предыдущем шаге:

    [![Снимок экрана с m2repository, скопированным в папку 23.1.1.0/content](resolving-library-installation-errors-images/07-copied-m2repository-vs.png)](resolving-library-installation-errors-images/07-copied-m2repository-vs.png#lightbox)

6. В извлеченном каталоге с расширением **.zip** перейдите к папке **m2repository\\com\\android\\support\\support-v4** и откройте папку, соответствующую созданному выше номеру версии (в нашем примере это **23.1.1**):

    [![Пример списка файлов, содержащихся в папке support-v4/23.1.1](resolving-library-installation-errors-images/08-zip-contents-vs.png)](resolving-library-installation-errors-images/08-zip-contents-vs.png#lightbox)

7. Скопируйте все файлы из этой папки в каталог **embedded**, созданный на шаге 4.

    [![Пример файлов, скопированных в папку 23.1.1.0/embedded](resolving-library-installation-errors-images/09-copied-vs.png)](resolving-library-installation-errors-images/09-copied-vs.png#lightbox)

8. Убедитесь, что все файлы скопированы. Каталог **embedded** теперь должен содержать файлы с расширениями **.jar**, **.aar** и **.pom**.

9. Распакуйте содержимое всех извлеченных **AAR-файлов** в каталог **embedded**. В Windows добавьте расширение **.zip** к файлу **AAR-файлу**, откройте его и скопируйте содержимое в каталог **embedded**.
    В macOS распакуйте **AAR**-файл с помощью команды **unzip** в окне терминала (например, **unzip file.aar**).

Вы вручную установили недостающие компоненты и теперь проект должен компилироваться без ошибок. Если это не так, убедитесь, что вы скачали из **m2repository** правильную версию **ZIP**-файла, которая соответствует версии в сообщении об ошибке, и убедитесь, что ее содержимое установлено в нужных расположениях, как описано в предыдущих шагах.

## <a name="summary"></a>Сводка

В этой статье объясняется, как устранить распространенные ошибки, которые могут возникать при автоматической загрузке и установке библиотек зависимостей. Здесь описано, как удалить проблемную библиотеку и перестроить проект для повторного скачивания и повторной установки библиотеки.
Здесь описано, как скачать библиотеку и установить ее в папку **zips**. Здесь также описана более трудоемкая процедура скачивания и установки необходимых файлов вручную, которая поможет устранить проблемы, которые не могут быть разрешены с помощью автоматических средств.
