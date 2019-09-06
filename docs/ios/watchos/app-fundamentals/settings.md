---
title: Работа с параметрами watchOS в Xamarin
description: В этом документе описывается работа с параметрами watchOS в Xamarin. В нем обсуждается добавление параметров в решение для наблюдения за приложениями, использование этих параметров в приложении и Apple Watchное приложение на устройстве iPhone.
ms.prod: xamarin
ms.assetid: 4B2EB192-F0A2-4010-B141-0431520594C0
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/17/2017
ms.openlocfilehash: bcb719451529cd5a9ca829b8693c425d752cc93b
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/04/2019
ms.locfileid: "70283224"
---
# <a name="working-with-watchos-settings-in-xamarin"></a>Работа с параметрами watchOS в Xamarin

Apple Watch приложения могут использовать те же функции параметров, что и приложения iOS. Пользовательский интерфейс параметров отображается в **Apple Watchном** приложении iPhone, но значения доступны в приложении iPhone, а также в расширении.

![](settings-images/intro.png "Apple Watch приложения могут использовать те же функции параметров, что и приложения iOS")

Параметры будут храниться в общем расположении файлов, доступном как для приложения iOS, так и для расширения приложения для наблюдения, определенного **группой приложений**. Перед добавлением параметров необходимо [настроить группу приложений](~/ios/watchos/app-fundamentals/app-groups.md) , выполнив приведенные ниже инструкции.

## <a name="add-settings-in-a-watch-solution"></a>Добавление параметров в решение "контрольные значения"

В **приложении iPhone** в решении (а*не* на контрольном приложении или расширении):

1. Щелкните правой кнопкой мыши **добавить > новый файл...** и выберите **Settings. пучок** (имя нельзя изменить в диалоговом окне " **Создание файла** "):

   [![](settings-images/settings-add-sml.png "Добавить новый пакет параметров")](settings-images/settings-add.png#lightbox)

2. Измените имя на **Settings-Watch. пучок** (выберите и введите **Command + R** для переименования):

   ![](settings-images/settings-rename.png "Переименование пакета")

3. Добавьте новый ключ `ApplicationGroupContainerIdentifier` в **корневую папку. plist** со значением, заданным для настроенной вами группы приложений (например, `group.com.xamarin.WatchSettings`в примере):

   [![](settings-images/settings-appgroup-sml.png "Добавьте ключ Аппликатионграупконтаинеридентифиер в корневую папку. plist")](settings-images/settings-appgroup.png#lightbox)

4. Измените **Сеттингс-ватч. пучок/root. plist** , чтобы они содержали параметры, которые вы хотите использовать. файл шаблона содержит группу.
  TextField, выключатель и ползунок по умолчанию (которые можно удалить и заменить собственными параметрами):

  [![](settings-images/rootplist-sml.png "Измените Сеттингс-ватч. пучок/root. plist")](settings-images/rootplist.png#lightbox)


## <a name="use-settings-in-the-watch-app"></a>Использование параметров в приложении для просмотра контрольных значений

Чтобы получить доступ к значениям, выбранным пользователем, `NSUserDefaults` создайте экземпляр с помощью группы приложений и `NSUserDefaultsType.SuiteName`укажите:

```csharp
NSUserDefaults shared = new NSUserDefaults(
    "group.com.xamarin.WatchSettings",
    NSUserDefaultsType.SuiteName);

var isEnabled = shared.BoolForKey ("enabled_preference");
var userName = shared.StringForKey ("name_preference");
```

## <a name="apple-watch-app"></a>Приложение Apple Watch

[![](settings-images/settings-app-sml.png "Новое приложение Apple Watch на устройстве iPhone")](settings-images/settings-app.png#lightbox)

Пользователи будут взаимодействовать с параметрами через новое приложение **Apple Watch** на iPhone. Это приложение позволяет пользователю отображать или скрывать приложения в контрольном списке, а также изменять параметры, предоставляемые с помощью **параметров-Watch. пучок**.

![](settings-images/applewatch-1.png "Пример параметров приложения") ![](settings-images/applewatch-2.png "пример параметров приложения")



## <a name="related-links"></a>Связанные ссылки

- [Документ параметров Apple](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/Settings.html#//apple_ref/doc/uid/TP40014969-CH22-SW1)
