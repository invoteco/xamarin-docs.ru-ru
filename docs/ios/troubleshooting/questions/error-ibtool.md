---
title: 'Ошибка IBTool: не удалось завершить операцию.'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: A804EBC4-2BBF-4A98-A4E8-A455DB2E8A17
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 04/03/2018
ms.openlocfilehash: 695410937e82b885e31827d02f5fefd138497523
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/04/2019
ms.locfileid: "70290273"
---
# <a name="ibtool-error-the-operation-couldnt-be-completed"></a>Ошибка IBTool: не удалось завершить операцию.

## <a name="fixed-in-xcode-611"></a>Исправлено в Xcode 6.1.1

Компания Apple [устранила](https://developer.apple.com/library/content/documentation/Xcode/Conceptual/RN-Xcode-Archive/Chapters/xc6_release_notes.html#//apple_ref/doc/uid/TP40016994-CH4-SW1) эту `ibtool` ошибку в Xcode 6.1.1, поэтому обновление до Xcode 6.1.1 или более поздней версии является самым простым исправлением.

* * *

## <a name="description-of-the-problem"></a>Описание проблемы

В `ibtool` команде Xcode 6,0 возникла ошибка в OS X 10,10 Yosemite. Xamarin. iOS использует Xcode `ibtool` для компиляции раскадровки и `XIB` файлов.

Дополнительные сведения об ошибке в отношении Xcode можно найти в следующей записи Stack Overflow:[https://stackoverflow.com/questions/25754763/cant-open-storyboard](https://stackoverflow.com/questions/25754763/cant-open-storyboard)

### <a name="error-message"></a>Сообщение об ошибке

> Не удалось открыть документ "файл mainstoryboard. Storyboard". не удалось завершить операцию. (ошибка com. Apple. Интерфацебуилдер-1.)

## <a name="workarounds-for-xcode-60"></a>Обходные пути (для Xcode 6,0)

### <a name="option-1-manage-all-uiimageviewimage-properties-in-code"></a>Вариант 1. Управление всеми `UIImageView.Image` свойствами в коде

Вместо того чтобы задавать `Image` свойство `UIImageView` объекта в раскадровке или `.xib` файле, можно задать свойство в одном из методов переопределения жизненного цикла представления в контроллере представления (например, в `ViewDidLoad()`). Советы по использованию `UIImage.FromBundle()` VS см. в разделе также [Работа с образами](~/ios/app-fundamentals/images-icons/index.md) . `UIImage.FromFile()`

### <a name="option-2-move-all-of-the-image-resources-to-the-top-level-resources-folder"></a>Вариант 2. Переместить все ресурсы образа в папку верхнего уровня `Resources`

После перемещения образов в папку верхнего уровня `Resources` потребуется обновить раскадровку и `.xib` файлы, чтобы использовать новые пути к изображениям.

### <a name="option-3-set-the-logicalname-for-any-problematic-image-assets-so-they-are-copied-to-the-top-level-of-theapp-bundle"></a>Вариант 3. Задайте для всех проблемных ресурсов изображений, чтобы они были скопированы на верхний уровень`.app` пакета. `LogicalName`

Например, предположим, что исходный `.csproj` файл содержит следующую запись:

`<BundleResource Include="Resources\Images\image.png" />`

Этот элемент можно изменить и добавить `LogicalName` , чтобы изображение было скопировано на верхний уровень `.app` пакета:

```xml
<BundleResource Include="Resources\Images\image.png">
    <LogicalName>image.png</LogicalName>
</BundleResource>
```

В Visual Studio для Mac `LogicalName` можно также задать, `Resource ID` используя поле для изображения в разделе **View > Pad > Properties**. (См. также [https://stackoverflow.com/questions/16938250/xamarin-studio-folder-structure-issue-in-ios-project/16951545#16951545](https://stackoverflow.com/questions/16938250/xamarin-studio-folder-structure-issue-in-ios-project/16951545#16951545):)

После этого изменения необходимо будет обновить раскадровку и `.xib` файлы, чтобы использовать новые пути к изображениям верхнего уровня. Visual Studio для Mac будет автоматически обновлять список автозаполнения для `Image` свойства в конструкторе iOS. В Visual Studio необходимо изменить путь вручную. В конструкторе iOS это изображение будет отображаться как отсутствующее, но проект будет построен и запущен правильно.

### <a name="next-steps"></a>Следующие шаги

Для получения дополнительной помощи, для связи с нами или если проблема остается даже после использования приведенных выше сведений, ознакомьтесь с возможностями [поддержки Xamarin?](~/cross-platform/troubleshooting/support-options.md) дополнительные сведения о вариантах контактов, предложениях и о том, как создать новую ошибку при необходимости . 

