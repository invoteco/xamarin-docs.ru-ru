---
title: Настраиваемые значки документов в Xamarin. iOS
description: В этой статье рассматривается включение ресурса изображения и управление им в приложении Xamarin. iOS, которое будет использоваться в качестве пользовательского значка типа документа.
ms.prod: xamarin
ms.assetid: 7A3F3C94-2578-4F53-9B8E-25714F48BDD6
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 05/23/2017
ms.openlocfilehash: 683587e4857ede20096be731b3cfa3b88b3a668d
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/04/2019
ms.locfileid: "70282519"
---
# <a name="custom-document-icons-in-xamarinios"></a>Настраиваемые значки документов в Xamarin. iOS

_В этой статье рассматривается включение ресурса изображения и управление им в приложении Xamarin. iOS, которое будет использоваться в качестве пользовательского значка типа документа._

Если приложение Xamarin. iOS поддерживает загрузку определенного типа документа, разработчик может предоставить значки, которые система будет использовать при обнаружении этого типа документа, например, когда пользователь удерживает вложение в *почтовом приложении* , как показано ниже:

 [![](custom-document-types-images/17.png "Пример значков типа документа")](custom-document-types-images/17.png#lightbox)

Разработчик может добавить сведения о типе документа для формата файла, который приложение может открывать, включая записи словаря для `CFBundleTypeName` строки и `LSItemContentTypes` `Info.plist`массива в приложении. Значки для типа документа попадают в `CFBundleTypeIconFiles` массив. Если значок документа не указан, iOS будет наследоваться от значка приложения.
Значки можно указывать для нескольких размеров, оптимизированных для различных способов разрешения устройств. 

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio для Mac](#tab/macos)

Чтобы назначить эти значения в Visual Studio для Mac, используйте раздел **типы документов** на `Info.plist` вкладке **Дополнительно** в редакторе, чтобы добавить тип документа и назначить ему значки изображений. Например, ниже приведен снимок экрана, показывающий регистрацию для поддержки PDF:

 [![](custom-document-types-images/18.png "Раздел \"типы документов\" на вкладке \"Дополнительно\" редактора \"info. plist\"")](custom-document-types-images/18.png#lightbox)
 
# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Чтобы назначить эти значения в Visual Studio, используйте раздел **типы документов** на `Info.plist`вкладке **Дополнительно** на странице:

 ![](custom-document-types-images/doc01w.png "Откройте раздел типы документов на вкладке Дополнительно.")

Нажмите кнопку **Добавить тип документа** и заполните обязательные поля:

![](custom-document-types-images/doc02w.png "Форма «Добавление типа документа»")

-----


Дополнительные сведения о типах документов см. в разделах [Справочник по единообразным идентификаторам типов](https://developer.apple.com/library/ios/#documentation/Miscellaneous/Reference/UTIRef/Articles/System-DeclaredUniformTypeIdentifiers.html) Apple и [разделы по программированию документов для iOS](https://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Introduction/Introduction.html).


## <a name="related-links"></a>Связанные ссылки

- [Работа с изображениями (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/workingwithimages)
- [Hello, iPhone](~/ios/get-started/hello-ios/index.md)
- [Пользовательские рекомендации по созданию значков и изображений](https://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html)
