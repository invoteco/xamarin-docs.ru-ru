---
title: 'Ошибка Ибтул: не удалось завершить операцию.'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: A804EBC4-2BBF-4A98-A4E8-A455DB2E8A17
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 04/03/2018
ms.openlocfilehash: 4ff7b88ad63870246acbf2b29d01775f6711a8ce
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031192"
---
# <a name="ibtool-error-the-operation-couldnt-be-completed"></a>Ошибка Ибтул: не удалось завершить операцию.

## <a name="fixed-in-xcode-611"></a>Исправлено в Xcode 6.1.1

Компания Apple [устранила](https://developer.apple.com/library/content/documentation/Xcode/Conceptual/RN-Xcode-Archive/Chapters/xc6_release_notes.html#//apple_ref/doc/uid/TP40016994-CH4-SW1) эту `ibtool` ошибку в Xcode 6.1.1, поэтому обновление до Xcode 6.1.1 или более поздней версии является самым простым решением.

* * *

## <a name="description-of-the-problem"></a>Описание проблемы

В команде `ibtool` в Xcode 6,0 возникла ошибка в OS X 10,10 Yosemite. Xamarin. iOS использует `ibtool` Xcode для компиляции раскадровок и `XIB` файлов.

Дополнительные сведения об ошибке в отношении Xcode можно найти в следующей Stack Overflow записи: [https://stackoverflow.com/questions/25754763/cant-open-storyboard](https://stackoverflow.com/questions/25754763/cant-open-storyboard)

### <a name="error-message"></a>Сообщение об ошибке

> Не удалось открыть документ "файл mainstoryboard. Storyboard". не удалось завершить операцию. (ошибка com. Apple. Интерфацебуилдер-1.)

## <a name="workarounds-for-xcode-60"></a>Обходные пути (для Xcode 6,0)

### <a name="option-1-manage-all-uiimageviewimage-properties-in-code"></a>Вариант 1. Управление всеми `UIImageView.Image` свойствами в коде

Вместо того, чтобы задавать свойство `Image` `UIImageView` в файле раскадровки или `.xib`, можно задать свойство в одном из методов переопределения жизненного цикла представления в контроллере представления (например, в `ViewDidLoad()`). Советы по использованию `UIImage.FromBundle()` и `UIImage.FromFile()`см. [в разделе Работа с образами](~/ios/app-fundamentals/images-icons/index.md) .

### <a name="option-2-move-all-of-the-image-resources-to-the-top-level-resources-folder"></a>Вариант 2. Перемещение всех ресурсов образа в папку `Resources` верхнего уровня

После перемещения образов в папку `Resources` верхнего уровня необходимо обновить файлы Storyboard и `.xib`, чтобы использовать новые пути к изображениям.

### <a name="option-3-set-the-logicalname-for-any-problematic-image-assets-so-they-are-copied-to-the-top-level-of-theapp-bundle"></a>Вариант 3. Установка `LogicalName` для любых проблемных ресурсов изображений, чтобы они копировались на верхний уровень пакета`.app`

Например, предположим, что исходный файл `.csproj` содержит следующую запись:

`<BundleResource Include="Resources\Images\image.png" />`

Вы можете изменить этот элемент и добавить `LogicalName`, чтобы изображение вместо него копировалось на верхний уровень `.app` пакета:

```xml
<BundleResource Include="Resources\Images\image.png">
    <LogicalName>image.png</LogicalName>
</BundleResource>
```

В Visual Studio для Mac `LogicalName` можно также задать с помощью поля `Resource ID` для изображения в области **вид > pad > свойства**. (См. также: [https://stackoverflow.com/questions/16938250/xamarin-studio-folder-structure-issue-in-ios-project/16951545#16951545](https://stackoverflow.com/questions/16938250/xamarin-studio-folder-structure-issue-in-ios-project/16951545#16951545))

После этого изменения необходимо будет обновить файлы раскадровки и `.xib`, чтобы использовать новые пути к изображениям верхнего уровня. Visual Studio для Mac будет автоматически обновлять список автозаполнения для свойства `Image` в конструкторе iOS. В Visual Studio необходимо изменить путь вручную. В конструкторе iOS это изображение будет отображаться как отсутствующее, но проект будет построен и запущен правильно.

### <a name="next-steps"></a>Следующие шаги

Для получения дополнительной помощи, для связи с нами или если проблема остается даже после использования приведенных выше сведений, ознакомьтесь с возможностями [поддержки Xamarin?](~/cross-platform/troubleshooting/support-options.md) дополнительные сведения о вариантах контактов, предложениях и о том, как создать новую ошибку при необходимости . 
