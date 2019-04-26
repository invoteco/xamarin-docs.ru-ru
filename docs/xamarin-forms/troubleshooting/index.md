---
title: Устранение неполадок
description: Распространенные ошибки и способы их устранения
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 63291951-7375-4CBF-BCC3-2E4AD157A2C8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: fbe4fb6fce52636b59a9637ee0150c4c19fcc9da
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60850451"
---
# <a name="troubleshooting"></a>Устранение неполадок

_Распространенные ошибки и способы их устранения_

## <a name="error-unable-to-find-a-version-of-xamarinforms-compatible-with"></a>Ошибка: «Не удается найти версию Xamarin.Forms, совместимый с в...»

Могут отображаться следующие ошибки в **консоли пакета** окно при обновлении все пакеты Nuget в решение Xamarin.Forms, или в проекте приложения Xamarin.Forms Android:

```csharp
Attempting to resolve dependency 'Xamarin.Android.Support.v7.AppCompat (= 23.3.0.0)'.
Attempting to resolve dependency 'Xamarin.Android.Support.v4 (= 23.3.0.0)'.
Looking for updates for 'Xamarin.Android.Support.v7.MediaRouter'...
Updating 'Xamarin.Android.Support.v7.MediaRouter' from version '23.3.0.0' to '23.3.1.0' in project 'Todo.Droid'.
Updating 'Xamarin.Android.Support.v7.MediaRouter 23.3.0.0' to 'Xamarin.Android.Support.v7.MediaRouter 23.3.1.0' failed.
Unable to find a version of 'Xamarin.Forms' that is compatible with 'Xamarin.Android.Support.v7.MediaRouter 23.3.0.0'.
```

### <a name="what-causes-this-error"></a>Что вызывает эту ошибку?

Visual Studio для Mac (или Visual Studio) может означать, что доступны обновления для пакета Xamarin.Forms Nuget *и всех его зависимостей*. В Xamarin Studio, решения **пакетов** узел может выглядеть следующим образом (номера версий могут отличаться):

![](images/updates-available.png "Папка пакетов проекта Android")

Эта ошибка может возникать при попытке обновить _все_ пакеты.

Это обусловлено с Android проекты на целевой объект и скомпилировать версию Android 6.0 (API 23) или ниже, Xamarin.Forms жесткую зависимость *конкретных* версии Android поддерживают пакеты. Несмотря на то, что обновленные версии, эти пакеты могут быть доступны, Xamarin.Forms не всегда совместим с ними.

В этом случае необходимо обновить _только_ **Xamarin.Forms** пакета как это гарантирует, что еще зависимостей осталось на совместимые версии. Другие пакеты, которые были добавлены в проект также будут обновляться по отдельности до тех пор, пока они не приводят к пакеты поддержки Android для обновления.


> [!NOTE]
> Если вы используете Xamarin.Forms 2.3.4 или более поздней версии **и** задана версия целевой и скомпилировать проект Android, Android 7.0 (API 24) или выше, а затем жестких зависимостей, упомянутых выше, больше не применяются и вы можете обновить поддержки пакеты, независимо от пакет Xamarin.Forms.


### <a name="fix-remove-all-packages-and-re-add-xamarinforms"></a>Исправление: Удалите все пакеты и повторно добавьте Xamarin.Forms

Если **Xamarin.Android.Support** пакеты были обновлены для несовместимые версии, самый простой способ исправления заключается в:

1. Затем вручную удалите все пакеты Nuget в проект Android,
2. Повторное добавление **Xamarin.Forms** пакета.

Это будет автоматически загружать *правильный* версий других пакетов.

Чтобы просмотреть видео этого процесса, ознакомьтесь с этой [форумы post](https://forums.xamarin.com/discussion/comment/170012/#Comment_170012).
