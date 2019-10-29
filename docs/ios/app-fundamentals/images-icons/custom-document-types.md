---
title: Настраиваемые значки документов в Xamarin. iOS
description: В этой статье рассматривается включение ресурса изображения и управление им в приложении Xamarin. iOS, которое будет использоваться в качестве пользовательского значка типа документа.
ms.prod: xamarin
ms.assetid: 7A3F3C94-2578-4F53-9B8E-25714F48BDD6
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 05/23/2017
ms.openlocfilehash: ac8ee96d6183f9a62233d217c75b03da15605bd2
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73004221"
---
# <a name="custom-document-icons-in-xamarinios"></a>Настраиваемые значки документов в Xamarin. iOS

_В этой статье рассматривается включение ресурса изображения и управление им в приложении Xamarin. iOS, которое будет использоваться в качестве пользовательского значка типа документа._

Если приложение Xamarin. iOS поддерживает загрузку определенного типа документа, разработчик может предоставить значки, которые система будет использовать при обнаружении этого типа документа, например, когда пользователь удерживает вложение в *почтовом приложении* , как показано ниже:

 [![](custom-document-types-images/17.png "An example of document type icons")](custom-document-types-images/17.png#lightbox)

Разработчик может добавлять сведения о типе документа для формата файлов, которые приложение может открывать, включая записи словаря для строки `CFBundleTypeName` и `LSItemContentTypes` массив в `Info.plist`е приложения. Значки для типа документа попадают в массив `CFBundleTypeIconFiles`. Если значок документа не указан, iOS будет наследоваться от значка приложения.
Значки можно указывать для нескольких размеров, оптимизированных для различных способов разрешения устройств. 

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio для Mac](#tab/macos)

Чтобы назначить эти значения в Visual Studio для Mac, используйте раздел **типы документов** на вкладке **дополнительно** в редакторе `Info.plist`, чтобы добавить тип документа и назначить ему значки изображений. Например, ниже приведен снимок экрана, показывающий регистрацию для поддержки PDF:

 [![](custom-document-types-images/18.png "The Document Types section under the Advanced tab on the `Info.plist` editor")](custom-document-types-images/18.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Чтобы назначить эти значения в Visual Studio, используйте раздел **типы документов** на вкладке **Дополнительно** на `Info.plist`:

 ![](custom-document-types-images/doc01w.png "Open the Document Types section under the Advanced tab")

Нажмите кнопку **Добавить тип документа** и заполните обязательные поля:

![](custom-document-types-images/doc02w.png "The Add Document Type form")

-----

Дополнительные сведения о типах документов см. в разделах [Справочник по единообразным идентификаторам типов](https://developer.apple.com/library/ios/#documentation/Miscellaneous/Reference/UTIRef/Articles/System-DeclaredUniformTypeIdentifiers.html) Apple и [разделы по программированию документов для iOS](https://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Introduction/Introduction.html).

## <a name="related-links"></a>Связанные ссылки

- [Работа с изображениями (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/workingwithimages)
- [Hello, iPhone](~/ios/get-started/hello-ios/index.md)
- [Пользовательские рекомендации по созданию значков и изображений](https://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html)
