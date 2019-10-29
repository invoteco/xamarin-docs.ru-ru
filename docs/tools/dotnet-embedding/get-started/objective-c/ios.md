---
title: Начало работы с iOS
description: В этом документе описывается, как приступить к использованию внедрения .NET с iOS. В нем обсуждаются требования и представлен пример приложения для демонстрации привязки управляемой сборки и использования выходных данных в проекте Xcode.
ms.prod: xamarin
ms.assetid: D5453695-69C9-44BC-B226-5B86950956E2
author: davidortinau
ms.author: daortin
ms.date: 11/14/2017
ms.openlocfilehash: 5697d20077b746d9d33db985111c2d04908d5d01
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029764"
---
# <a name="getting-started-with-ios"></a>Начало работы с iOS

## <a name="requirements"></a>Требования

Помимо требований, описанных в руководствах [Приступая к работе с цели-C](~/tools/dotnet-embedding/get-started/objective-c/index.md) , вам также потребуется:

* [Xamarin. iOS 10,11](https://visualstudio.microsoft.com/xamarin/) или более поздней версии

## <a name="hello-world"></a>Здравствуй, мир

Сначала создайте простой пример Hello World в C#.

### <a name="create-c-sample"></a>Создание C# образца

Откройте Visual Studio для Mac, создайте новый проект библиотеки классов iOS, присвойте ему имя **Hello-from-CSharp**и сохраните его в **~/прожектс/Хелло-Фром-кшарп**.

Замените код в файле **MyClass.CS** следующим фрагментом кода:

```csharp
using UIKit;
public class MyUIView : UITextView
{
    public MyUIView ()
    {
        Text = "Hello from C#";
    }
}
```

Постройте проект, и полученная сборка будет сохранена как **~/прожектс/Хелло-Фром-кшарп/Хелло-Фром-кшарп/бин/дебуг/Хелло-Фром-кшарп.длл**.

### <a name="bind-the-managed-assembly"></a>Привязка управляемой сборки

После создания управляемой сборки привяжите ее, вызвав внедрение .NET.

Как описано в разделе Руководство по [установке](~/tools/dotnet-embedding/get-started/install/install.md) , это можно сделать в качестве шага после сборки в проекте, с помощью настраиваемого целевого объекта MSBuild или вручную:

```shell
cd ~/Projects/hello-from-csharp
objcgen ~/Projects/hello-from-csharp/hello-from-csharp/bin/Debug/hello-from-csharp.dll --target=framework --platform=iOS --outdir=output -c --debug
```

Платформа будет помещена в **~/прожектс/Хелло-Фром-кшарп/аутпут/Хелло-Фром-кшарп.фрамеворк**.

### <a name="use-the-generated-output-in-an-xcode-project"></a>Использование созданных выходных данных в проекте Xcode

Откройте Xcode, создайте новое приложение iOS с одним представлением, назовите его **Hello-from-CSharp**и выберите язык **цели-C** .

Откройте каталог **~/прожектс/Хелло-Фром-кшарп/аутпут** в Finder, выберите **Hello-from-CSharp. Framework**, перетащите его в проект Xcode и поместите непосредственно над папкой **Hello-from-CSharp** в проекте.

![Среда перетаскивания](ios-images/hello-from-csharp-ios-drag-drop-framework.png)

Убедитесь, что в появившемся диалоговом окне установлен флажок **Копировать элементы, если это необходимо** , и нажмите кнопку **Готово**.

![Копировать элементы при необходимости](ios-images/hello-from-csharp-ios-copy-items-if-needed.png)

Выберите проект **Hello-from-CSharp** и перейдите на **вкладку Общие**целевого объекта **Hello-from-CSharp** . В разделе **внедренные двоичные файлы** добавьте **Hello-from-CSharp. Framework**.

![Внедренные двоичные файлы](ios-images/hello-from-csharp-ios-embedded-binaries.png)

Откройте **ViewController. m**и замените содержимое следующим:

```objective-c
#import "ViewController.h"
#include "hello-from-csharp/hello-from-csharp.h"

@interface ViewController ()
@end

@implementation ViewController
- (void)viewDidLoad {
    [super viewDidLoad];

    MyUIView *view = [[MyUIView alloc] init];
    view.frame = CGRectMake(0, 200, 200, 200);
    [self.view addSubview: view];
}
@end
```

Внедрение .NET в настоящее время не поддерживает bitcode в iOS, что включено для некоторых шаблонов проектов Xcode. 

Отключите его в параметрах проекта:

![Bitcode, параметр](../../images/ios-bitcode-option.png)

Наконец, запустите проект Xcode, и примерно так же будет видно следующее:

![Приветствие C# из примера, выполняемого в симуляторе](ios-images/hello-from-csharp-ios.png)
