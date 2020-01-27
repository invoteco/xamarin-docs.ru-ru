---
title: Начало работы с macOS
description: В этом документе описывается, как приступить к использованию внедрения .NET с помощью macOS. В нем обсуждаются требования и представлен пример приложения для демонстрации привязки управляемой сборки и использования созданных выходных данных в проекте Xcode.
ms.prod: xamarin
ms.assetid: AE51F523-74F4-4EC0-B531-30B71C4D36DF
author: davidortinau
ms.author: daortin
ms.date: 11/14/2017
ms.openlocfilehash: f0e2128bca5d2965395647353cd5a95a4030439f
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725282"
---
# <a name="getting-started-with-macos"></a>Начало работы с macOS

## <a name="what-you-will-need"></a>Что вам потребуется

* Следуйте инструкциям в разделе [Приступая к работе с целевым](~/tools/dotnet-embedding/get-started/objective-c/index.md) руководством.

## <a name="hello-world"></a>Здравствуй, мир

Сначала создайте простой пример Hello World в C#.

### <a name="create-c-sample"></a>Создание C# образца

Откройте Visual Studio для Mac, создайте новый проект библиотеки классов Mac с именем **Hello-from-CSharp**и сохраните его в **~/прожектс/Хелло-Фром-кшарп**.

Замените код в файле **MyClass.CS** следующим фрагментом кода:

```csharp
using AppKit;
public class MyNSView : NSTextView
{
    public MyNSView ()
    {
        Value = "Hello from C#";
    }
}
```

Постройте проект. Полученная сборка будет сохранена как **~/прожектс/Хелло-Фром-кшарп/Хелло-Фром-кшарп/бин/дебуг/Хелло-Фром-кшарп.длл**.

### <a name="bind-the-managed-assembly"></a>Привязка управляемой сборки

После создания управляемой сборки привяжите ее, вызвав внедрение .NET.

Как описано в разделе Руководство по [установке](~/tools/dotnet-embedding/get-started/install/install.md) , это можно сделать в качестве шага после сборки в проекте, с помощью настраиваемого целевого объекта MSBuild или вручную:

```shell
cd ~/Projects/hello-from-csharp
objcgen ~/Projects/hello-from-csharp/hello-from-csharp/bin/Debug/hello-from-csharp.dll --target=framework --platform=macOS-modern --abi=x86_64 --outdir=output -c --debug
```

Платформа будет помещена в **~/прожектс/Хелло-Фром-кшарп/аутпут/Хелло-Фром-кшарп.фрамеворк**.

### <a name="use-the-generated-output-in-an-xcode-project"></a>Использование созданных выходных данных в проекте Xcode

Откройте Xcode и создайте новое приложение Cocoa. Назовите его **Hello-from-CSharp** и выберите язык **цели-C** .

Откройте каталог **~/прожектс/Хелло-Фром-кшарп/аутпут** в Finder, выберите **Hello-from-CSharp. Framework**, перетащите его в проект Xcode и поместите непосредственно над папкой **Hello-from-CSharp** в проекте.

![Среда перетаскивания](macos-images/hello-from-csharp-mac-drag-drop-framework.png)

Убедитесь, что в появившемся диалоговом окне установлен флажок **Копировать элементы, если это необходимо** , и нажмите кнопку **Готово**.

![Копировать элементы при необходимости](macos-images/hello-from-csharp-mac-copy-items-if-needed.png)

Выберите проект **Hello-from-CSharp** и перейдите на вкладку **Общие** целевого объекта **Hello-from-CSharp** . В разделе **внедренные двоичные файлы** добавьте **Hello-from-CSharp. Framework**.

![Внедренные двоичные файлы](macos-images/hello-from-csharp-mac-embedded-binaries.png)

Откройте **ViewController. m**и замените содержимое следующим:

```objc
#import "ViewController.h"

#include "hello-from-csharp/hello-from-csharp.h"

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];

    MyNSView *view = [[MyNSView alloc] init];
    view.frame = CGRectMake(0, 200, 200, 200);
    [self.view addSubview: view];
}

@end
```

Наконец, запустите проект Xcode, и примерно так же будет видно следующее:

![Приветствие C# из примера, выполняемого в симуляторе](macos-images/hello-from-csharp-mac.png)
