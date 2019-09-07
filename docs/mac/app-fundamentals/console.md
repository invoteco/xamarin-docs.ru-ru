---
title: Использование привязок Xamarin. Mac для консольных приложений
description: Создайте консольное приложение без монитора с помощью Xamarin. Mac для доступа к собственным интерфейсам API macOS.
ms.prod: xamarin
ms.assetid: 9E52B4CC-F530-4B3E-984A-7F5719A6D528
ms.technology: xamarin-mac
author: migueldeicaza
ms.author: miguel
ms.date: 07/27/2018
ms.openlocfilehash: a38543d575f655a3b1b70ff94eece7fef1bf2d40
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2019
ms.locfileid: "70769898"
---
# <a name="xamarinmac-bindings-in-console-apps"></a>Привязки Xamarin. Mac в консольных приложениях

Существует несколько сценариев, в которых необходимо использовать некоторые собственные C# API Apple в для создания приложения &ndash; без монитора, которое не имеет пользовательского интерфейса &ndash; с помощью. C#

Шаблоны проектов для приложений Mac включают вызов `NSApplication.Init()` `NSApplication.Main(args)`, за которым следует вызов, как правило, выглядит следующим образом:

```csharp
static class MainClass {
    static void Main (string [] args)
    {
        NSApplication.Init ();
        NSApplication.Main (args);
    }
}
```

Вызов для `Init` подготовки среды выполнения Xamarin. Mac `Main(args)` вызывает запуск главного цикла приложения Cocoa, который готовит приложение к получению событий клавиатуры и мыши и отображает главное окно приложения.   Вызов `Main` также попытается разместить ресурсы Cocoa, подготовить TopLevel окно и предполагает, что программа будет частью пакета приложений (программ, распространяемых в каталоге `.app` с расширением и очень специфическим макетом).

Приложениям без монитора не требуется пользовательский интерфейс и не требуется запускать их как часть пакета приложений.

## <a name="creating-the-console-app"></a>Создание консольного приложения

Поэтому лучше начать с обычного типа проекта консоли .NET.

Необходимо выполнить несколько действий:

- Создайте пустой проект.
- Ссылка на библиотеку Xamarin. Mac. dll.
- Приведите неуправляемую зависимость к проекту.

Эти шаги более подробно описаны ниже.

### <a name="create-an-empty-console-project"></a>Создание пустого консольного проекта

Создайте новый проект консоли .NET, убедитесь, что он является .NET, а не .NET Core, так как Xamarin. Mac. dll не выполняется в среде выполнения .NET Core, он выполняется только в среде выполнения Mono.

### <a name="reference-the-xamarinmac-library"></a>Ссылка на библиотеку Xamarin. Mac

Чтобы скомпилировать код, необходимо сослаться `Xamarin.Mac.dll` на сборку из этого каталога:`/Library/Frameworks/Xamarin.Mac.framework/Versions/Current//lib/x86_64/full`

Для этого перейдите в раздел ссылки на проект, перейдите на вкладку **Сборка .NET** и нажмите кнопку **Обзор** , чтобы найти файл в файловой системе.  Перейдите по указанному выше пути, а затем выберите **Xamarin. Mac. dll** из этого каталога.

Это обеспечит доступ к API-интерфейсам Cocoa во время компиляции.   На этом этапе можно добавить `using AppKit` в начало файла и `NSApplication.Init()` вызвать метод.   Перед запуском приложения можно выполнить только еще один шаг.

### <a name="bring-the-unmanaged-support-library-into-your-project"></a>Перевод неуправляемой библиотеки поддержки в проект

Прежде чем приложение будет запущено, необходимо перенести `Xamarin.Mac` библиотеку поддержки в проект.   Для этого добавьте в проект новый файл (в параметрах проекта выберите **Добавить**, а затем **Добавить существующий файл**) и перейдите к этому каталогу:

`/Library/Frameworks/Xamarin.Mac.framework/Versions/Current/lib`

Выберите файл **либксаммак. dylib**.   Вам будет предложено выбрать копирование, компоновку или перемещение.   Мне нравится компоновка, но копирование также работает.    Затем необходимо выбрать файл и на панели свойств (выберите **View > pad > свойства** , если панель свойств не видна), перейдите к разделу **Build** и задайте параметру **Копировать в выходной каталог** значение **Копировать, если новее**.

Теперь вы можете запустить приложение Xamarin. Mac.

Результат в каталоге bin будет выглядеть следующим образом:

```
Xamarin.Mac.dll
Xamarin.Mac.pdb
consoleapp.exe
consoleapp.pdb
libxammac.dylib
```

Для запуска этого приложения все эти файлы потребуются в одном каталоге.

## <a name="building-a-standalone-application-for-distribution"></a>Создание автономного приложения для распространения

Вам может потребоваться распространить один исполняемый файл среди пользователей.  Для этого можно использовать `mkbundle` средство для преобразования различных файлов в автономный исполняемый файл.

Сначала убедитесь, что приложение компилируется и выполняется.   Когда результаты будут удовлетворены, можно выполнить в командной строке следующую команду:

```
$ mkbundle --simple -o /tmp/consoleapp consoleapp.exe --library libxammac.dylib --config /Library/Frameworks/Mono.framework/Versions/Current/etc/mono/config --machine-config /Library/Frameworks/Mono.framework/Versions/Current//etc/mono/4.5/machine.config
[Output from the bundling tool]
$ _
```

В приведенном выше вызове командной строки параметр `-o` используется для указания создаваемых выходных данных, в данном случае переданного. `/tmp/consoleapp`   Теперь это автономное приложение, которое можно распространять и не имеет внешних зависимостей от mono или Xamarin. Mac. это полностью автономный исполняемый файл.

Командная строка вручную указала используемый файл **Machine. config** и файл конфигурации сопоставления библиотеки на уровне системы.   Они не являются обязательными для всех приложений, но удобно объединять их, так как они используются при использовании дополнительных возможностей .NET.

## <a name="project-less-builds"></a>Сборки без проектов

Для создания автономного приложения Xamarin. Mac не требуется полный проект. Вы также можете использовать простые файлы Makefile UNIX, чтобы завершить задание.   В следующем примере показано, как можно настроить файл makefile для простого приложения командной строки.

```
XAMMAC_PATH=/Library/Frameworks/Xamarin.Mac.framework/Versions/Current//lib/x86_64/full/
DYLD=/Library/Frameworks/Xamarin.Mac.framework/Versions/Current//lib
MONODIR=/Library/Frameworks/Mono.framework/Versions/Current/etc/mono

all: consoleapp.exe

consoelapp.exe: consoleapp.cs Makefile
    mcs -g -r:$(XAMMAC_PATH)/Xamarin.Mac.dll consoleapp.cs
    
run: consoleapp.exe
    MONO_PATH=$(XAMMAC_PATH) DYLD_LIBRARY_PATH=$(DYLD) mono --debug consoleapp.exe $(COMMAND)

bundle: consoleapp.exe
    mkbundle --simple consoleapp.exe -o ncsharp -L $(XAMMAC_PATH) --library $(DYLD)/libxammac.dylib --config $(MONODIR)/config --machine-config $(MONODIR)/4.5/machine.config
```

Приведенный `Makefile` выше объект предоставляет три цели:

- `make`будет создавать программу
- `make run`будет выполнять сборку и запуск программы в текущем каталоге
- `make bundle`создаст автономный исполняемый файл
