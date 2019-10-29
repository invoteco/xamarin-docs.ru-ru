---
title: Специальные возможности в macOS
description: В этом документе описано, как работать со специальными возможностями macOS в приложении Xamarin. Mac. В нем обсуждается описание элементов пользовательского интерфейса в раскадровках и коде, пользовательские элементы управления и доступность для тестирования.
ms.prod: xamarin
ms.assetid: D7F4892B-501A-4271-A7E0-BDD1586B63AD
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: 3f3b9c84fad0bce8939187fcd0c91d18314ce8ab
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032639"
---
# <a name="accessibility-on-macos"></a>Специальные возможности в macOS

На этой странице описывается, как использовать интерфейсы API специальных возможностей macOS для создания приложений в соответствии с [контрольным списком специальных возможностей](~/cross-platform/app-fundamentals/accessibility.md).
См. страницы [специальных возможностей Android](~/android/app-fundamentals/accessibility.md) и [Специальные возможности iOS](~/ios/app-fundamentals/accessibility.md) для других API-интерфейсов платформы.

Чтобы понять, как интерфейсы API специальных возможностей работают в macOS (ранее именуемой OS X), сначала проверьте [модель специальных возможностей OS x](https://developer.apple.com/library/mac/documentation/Accessibility/Conceptual/AccessibilityMacOSX/OSXAXmodel.html).

## <a name="describing-ui-elements"></a>Описание элементов пользовательского интерфейса

AppKit использует протокол `NSAccessibility` для предоставления интерфейсов API, которые помогают сделать пользовательский интерфейс доступным. Сюда входит поведение по умолчанию, которое пытается задать осмысленные значения для свойств специальных возможностей, таких как установка `AccessibilityLabel` кнопки. Обычно метка является одним словом или короткой фразой, описывающей элемент управления или представление.

### <a name="storyboard-files"></a>Файлы раскадровки

Xamarin. Mac использует Interface Builder Xcode для изменения файлов раскадровки.
Сведения о специальных возможностях могут быть изменены в **инспекторе удостоверений** при выборе элемента управления в области конструктора (как показано на снимке экрана ниже):

[![Добавление специальных возможностей в Interface Builder Xcode](accessibility-images/xcode.png "Добавление специальных возможностей в Interface Builder Xcode")](accessibility-images/xcode-large.png#lightbox)

### <a name="code"></a>Код

В настоящее время Xamarin. Mac не предоставляется как метод задания `AccessibilityLabel`.  Добавьте следующий вспомогательный метод, чтобы задать метку доступности:

```csharp
public static class AccessibilityHelper
{
    [System.Runtime.InteropServices.DllImport (ObjCRuntime.Constants.ObjectiveCLibrary)]
    extern static void objc_msgSend (IntPtr handle, IntPtr selector, IntPtr label);

    static public void SetAccessibilityLabel (this NSView view, string value)
    {
        objc_msgSend (view.Handle, new ObjCRuntime.Selector ("setAccessibilityLabel:").Handle, new NSString (value).Handle);
    }
}
```

Этот метод можно затем использовать в коде, как показано ниже.

```csharp
AccessibilityHelper.SetAccessibilityLabel (someButton, "New Accessible Description");
```

Свойство `AccessibilityHelp` предназначено для объяснения того, что делает элемент управления или представление, и следует добавлять только в том случае, если метка не предоставляет достаточных сведений. Текст справки по-прежнему должен быть как можно более коротким, например «удаляет документ».

Некоторые элементы пользовательского интерфейса несущественны для доступного доступа (например, метки рядом с входными данными, имеющими собственную метку доступности и справку).
В этих случаях задайте `AccessibilityElement = false`, чтобы эти элементы управления или представления пропускались средствами чтения с экрана или другими средствами специальных возможностей.

Apple предоставляет рекомендации по [специальным возможностям](https://developer.apple.com/library/mac/documentation/Accessibility/Conceptual/AccessibilityMacOSX/EnhancingtheAccessibilityofStandardAppKitControls.html) , которые объясняют рекомендации по меткам специальных возможностей и тексту справки.

## <a name="custom-controls"></a>Пользовательские элементы управления

Дополнительные сведения о дополнительных шагах см. в руководстве [по Apple для доступных настраиваемых элементов управления](https://developer.apple.com/library/mac/documentation/Accessibility/Conceptual/AccessibilityMacOSX/ImplementingAccessibilityforCustomControls.html) .

## <a name="testing-accessibility"></a>Специальные возможности тестирования

macOS предоставляет **инспектор специальных** возможностей, который помогает тестировать специальные возможности. Инспектор включен в Xcode.

При первом запуске **инспектор специальных возможностей** должен иметь разрешение на управление компьютером через специальные возможности:

![Инспектор специальных возможностей, запрашивающий разрешение на запуск](accessibility-images/accessibility-inspector-1.png "Инспектор специальных возможностей, запрашивающий разрешение на запуск")

Разблокируйте экран параметров (если это необходимо в левом нижнем углу) и установите флажок **инспектор специальных возможностей**:

![Экран параметров для включения инспектора специальных возможностей](accessibility-images/accessibility-inspector-2.png "Экран параметров для включения инспектора специальных возможностей")

После включения инспектор отображается как плавающее окно, которое может быть перемещено вокруг экрана. На снимке экрана ниже показан инспектор, работающий рядом с примером приложения Mac. По мере перемещения курсора по окну в инспекторе отображаются все доступные свойства каждого элемента управления:

[![Пример выполняющейся инспектора специальных возможностей](accessibility-images/accessibility-example.png "Пример выполняющейся инспектора специальных возможностей")](accessibility-images/accessibility-example-large.png#lightbox)

Дополнительные сведения см. в разделе о [специальных возможностях тестирования для OS X](https://developer.apple.com/library/mac/documentation/Accessibility/Conceptual/AccessibilityMacOSX/OSXAXTestingApps.html).

## <a name="related-links"></a>Связанные ссылки

- [Специальные возможности для разных платформ](~/cross-platform/app-fundamentals/accessibility.md)
- [Специальные возможности Mac](https://www.apple.com/accessibility/mac/)
