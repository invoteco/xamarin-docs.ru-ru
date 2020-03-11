---
title: Расширенный (ручной) пример реального мира
description: В этом документе описывается использование выходных данных xcodebuild в качестве входных данных для цели Шарпие, которая позволяет понять, какое целевое Шарпие делает.
ms.prod: xamarin
ms.assetid: 044FF669-0B81-4186-97A5-148C8B56EE9C
author: davidortinau
ms.author: daortin
ms.date: 03/29/2017
ms.openlocfilehash: 2dea16633181d6b1120a5f9a90da685df66e5451
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/10/2020
ms.locfileid: "79031015"
---
# <a name="advanced-manual-real-world-example"></a>Расширенный (ручной) пример реального мира

**В этом примере используется [Библиотека POP из Facebook](https://github.com/facebook/pop).**

В этом разделе рассматривается более сложный подход к привязке, где мы будем использовать `xcodebuild`ное средство Apple, чтобы сначала построить проект POP, а затем вручную вывести входные данные для цели Шарпие. По сути, это посвящено цели, Шарпие в предыдущем разделе.

```bash
 $ git clone https://github.com/facebook/pop.git
Cloning into 'pop'...
   _(more git clone output)_

$ cd pop
```

Так как библиотека POP содержит проект Xcode (`pop.xcodeproj`), для сборки POP можно просто использовать `xcodebuild`. Этот процесс, в свою очередь, может создавать файлы заголовков, для которых может потребоваться выполнить анализ Шарпие. Именно поэтому сборка перед привязкой важна. При построении с помощью `xcodebuild` убедитесь, что вы передали тот же идентификатор и архитектуру SDK, которые вы планируете передать в целевую Шарпие (и помните, что цель Шарпие 3,0 может сделать это за вас):

```bash
$ xcodebuild -sdk iphoneos9.0 -arch arm64

Build settings from command line:
    ARCHS = arm64
    SDKROOT = iphoneos8.1

=== BUILD TARGET pop OF PROJECT pop WITH THE DEFAULT CONFIGURATION (Release) ===

...

CpHeader pop/POPAnimationTracer.h build/Headers/POP/POPAnimationTracer.h
    cd /Users/aaron/src/sharpie/pop
    export PATH="/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/usr/bin:/Applications/Xcode.app/Contents/Developer/usr/bin:/Users/aaron/bin::/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/opt/X11/bin:/usr/local/git/bin:/Users/aaron/.rvm/bin"
    builtin-copy -exclude .DS_Store -exclude CVS -exclude .svn -exclude .git -exclude .hg -strip-debug-symbols -strip-tool /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/strip -resolve-src-symlinks /Users/aaron/src/sharpie/pop/pop/POPAnimationTracer.h /Users/aaron/src/sharpie/pop/build/Headers/POP

...

** BUILD SUCCEEDED **
```

В консоли в качестве части `xcodebuild`будет выводиться множество сведений о сборке. Как показано выше, можно увидеть, что целевой объект "Кфеадер" был запущен, где файлы заголовков были скопированы в выходной каталог сборки. Это часто и упрощает привязку: как часть сборки собственной библиотеки файлы заголовков часто копируются в "общедоступное", которое может упростить синтаксический анализ для привязки. В этом случае мы понимаем, что файлы заголовков POP находятся в каталоге `build/Headers`.

Теперь все готово для привязки POP. Мы понимаем, что мы хотим создать пакет SDK `iphoneos8.1` с архитектурой `arm64`, а файлы заголовков, которые нам интересуют, находятся в `build/Headers` в разделе Извлечение Git. Если взглянуть на каталог `build/Headers`, мы увидим несколько файлов заголовков:

```bash
$ ls build/Headers/POP/
POP.h                    POPAnimationTracer.h     POPDefines.h
POPAnimatableProperty.h  POPAnimator.h            POPGeometry.h
POPAnimation.h           POPAnimatorPrivate.h     POPLayerExtras.h
POPAnimationEvent.h      POPBasicAnimation.h      POPPropertyAnimation.h
POPAnimationExtras.h     POPCustomAnimation.h     POPSpringAnimation.h
POPAnimationPrivate.h    POPDecayAnimation.h
```

Если взглянуть на `POP.h`, можно увидеть, что это основной файл верхнего уровня библиотеки, который `#import`другие файлы. По этой причине нам нужно только передать `POP.h` цели Шарпие, и Clang будет выполнять оставшиеся действия в фоновом режиме:

```bash
$ sharpie bind -output Binding -sdk iphoneos8.1 \
    -scope build/Headers build/Headers/POP/POP.h \
    -c -Ibuild/Headers -arch arm64

Parsing Native Code...

Binding...
  [write] ApiDefinitions.cs
  [write] StructsAndEnums.cs

Binding Analysis:
  Automated binding is complete, but there are a few APIs which have
  been flagged with [Verify] attributes. While the entire binding
  should be audited for best API design practices, look more closely
  at APIs with the following Verify attribute hints:

  ConstantsInterfaceAssociation (1 instance):
    There's no fool-proof way to determine with which Objective-C
    interface an extern variable declaration may be associated.
    Instances of these are bound as [Field] properties in a partial
    interface into a near-by concrete interface to produce a more
    intuitive API, possibly eliminating the 'Constants' interface
    altogether.

  StronglyTypedNSArray (4 instances):
    A native NSArray* was bound as NSObject[]. It might be possible
    to more strongly type the array in the binding based on
    expectations set through API documentation (e.g. comments in the
    header file) or by examining the array contents through testing.
    For example, an NSArray* containing only NSNumber* instances can
    be bound as NSNumber[] instead of NSObject[].

  MethodToProperty (2 instances):
    An Objective-C method was bound as a C# property due to
    convention such as taking no parameters and returning a value (
    non-void return). Often methods like these should be bound as
    properties to surface a nicer API, but sometimes false-positives
    can occur and the binding should actually be a method.

  Once you have verified a Verify attribute, you should remove it
  from the binding source code. The presence of Verify attributes
  intentionally cause build failures.

  For more information about the Verify attribute hints above,
  consult the Objective Sharpie documentation by running 'sharpie
  docs' or visiting the following URL:

    http://xmn.io/sharpie-docs

Submitting usage data to Xamarin...
  Submitted - thank you for helping to improve Objective Sharpie!

Done.
```

Обратите внимание, что мы передали аргумент `-scope build/Headers` для цели Шарпие. Так как библиотеки C и цели-C должны `#import` или `#include` другие файлы заголовков, которые являются подробными сведениями о реализации библиотеки, а не API, который требуется привязать, аргумент `-scope` указывает цели Шарпие игнорировать любой API, который не определен в файле где-либо в каталоге `-scope`.

Аргумент `-scope` часто является необязательным для чистых библиотек, но нет ничего негативного предоставления. 

> [!TIP]
> Если в заголовках библиотеки импортируются все заголовки пакета SDK для iOS, например `#import <Foundation.h>`, то необходимо будет задать область, в противном случае Шарпие будет создавать определения привязки для импортированного заголовка пакета SDK iOS, что привело бы к созданию огромных привязок, которые, скорее всего, будут формировать ошибки при компиляции проекта привязки. 

Кроме того, мы указали `-c -Ibuild/headers`. Во первых, аргумент `-c` указывает цели Шарпие на то, что необходимо отключить интерпретацию аргументов командной строки и передать последующие аргументы _непосредственно в компилятор Clang_. Таким образом, `-Ibuild/Headers` является аргументом компилятора Clang, который указывает Clang искать включаемые сообщения в `build/Headers`, где находятся заголовки POP. Без этого аргумента Clang не сможет понять, где искать файлы, `POP.h` `#import`. _Почти все "проблемы" с использованием цели Шарпие сводится к выяснению того, что нужно передать в Clang_.

### <a name="completing-the-binding"></a>Завершение привязки

Целевая Шарпие теперь создала `Binding/ApiDefinitions.cs` и `Binding/StructsAndEnums.cs` файлы.

Они являются базовым первым проходом Шарпие в привязке, и в некоторых случаях это может быть достаточно. Как упоминалось выше, разработчику, как правило, потребуется вручную изменить созданные файлы после завершения цели Шарпие, чтобы [устранить проблемы](~/cross-platform/macios/binding/objective-sharpie/platform/apidefinitions-structsandenums.md) , которые не удалось автоматически обработать с помощью средства.

После завершения обновления эти два файла теперь можно добавить в проект привязки в Visual Studio для Mac или передать непосредственно в `btouch` или `bmac` средствам для создания окончательной привязки.

Подробное описание процесса привязки см. в [полном пошаговом руководстве](~/ios/platform/binding-objective-c/walkthrough.md).
