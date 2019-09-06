---
title: Устранение проблем, связанных с потоком данных System. Diagnostics. Tracing и TPL
description: Практический пример PCL. Как устранять проблемы, связанные с System.Diagnostics.Tracing для пакета NuGet потока данных библиотеки параллельных задач Майкрософт
ms.prod: xamarin
ms.assetid: 7986A556-382D-4D00-ACCF-3589B4029DE8
ms.date: 04/17/2018
author: conceptdev
ms.author: crdun
ms.openlocfilehash: e3ced3c989a88c6e759a5bc497147128b0a79868
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/04/2019
ms.locfileid: "70288195"
---
# <a name="pcl-case-study-how-can-i-resolve-problems-related-to-systemdiagnosticstracing-for-the-microsoft-tpl-dataflow-nuget-package"></a>Практический пример PCL. Как устранять проблемы, связанные с System.Diagnostics.Tracing для пакета NuGet потока данных библиотеки параллельных задач Майкрософт

> [!IMPORTANT]
> Этот конкретный пример `System.Diagnostic.Tracing` больше не выдает ошибок по умолчанию в последних версиях Xamarin. Хотя предложенное решение по-прежнему будет работать, обратите внимание, что некоторые ошибки, упомянутые в разделе "уровни ошибок", были устранены.
> Кроме того, следует отметить, что .NET Standard теперь является предпочтительным способом реализации межплатформенных API-интерфейсов .NET.

## <a name="summary"></a>Сводка

Xamarin. iOS и Xamarin. Android не реализуют 100% всех профилей PCL, которые они могут использовать в качестве ссылок. Для практического удобства в Visual Studio для Mac, Visual Studio и диспетчере пакетов NuGet проекты Xamarin позволяют использовать несколько профилей, которые имеют только _неполные_ реализации. Например, ни Xamarin. iOS, ни Xamarin. Android в настоящее время не включают полную реализацию типов в пространстве имен PCL "System. Diagnostics. Tracing". Это ограничение приводит к трем уровням ошибок при попытке использовать версию пакета `portable-net45+win8+wpa81` NuGet потока данных Microsoft TPL по умолчанию.

## <a name="workaround-switch-the-app-project-to-reference-the-portable-net45win8wp8wpa81-version-of-the-tpl-dataflow-library"></a>Решение Переключить проект приложения для ссылки `portable-net45+win8+wp8+wpa81` на версию библиотеки потоков данных TPL

(Это позволяет избежать всех трех уровней ошибок и работать с последними версиями Xamarin.)

1. Откройте файл приложения Project **. csproj** в текстовом редакторе.

2. Найдите строку, которая выглядит примерно так:

    ```xml
    <HintPath>..\packages\Microsoft.Tpl.Dataflow.4.5.24\lib\portable-net45+win8+wpa81\System.Threading.Tasks.Dataflow.dll</HintPath>
    ```

3. Измените `portable-net45+win8+wpa81` на `portable-net45+win8+wp8+wpa81` (`+wp8` добавляется):

    ```xml
    <HintPath>..\packages\System.Threading.Tasks.Dataflow.4.5.25\lib\portable-net45+win8+wp8+wpa81\System.Threading.Tasks.Dataflow.dll</HintPath>
    ```

### <a name="explanation"></a>Объяснение

Версия библиотеки не ссылается на **System. Diagnostics. Tracing. dll** , поэтому она полностью предотвращает все три уровня проблем. `portable-net45+win8+wp8+wpa81`

### <a name="limitations"></a>Ограничения

- Версия библиотеки может не включать 100% функций `portable-net45+win8+wpa81` версии. `portable-net45+win8+wp8+wpa81`

- Диспетчер пакетов NuGet устанавливает `portable-net45+win8+wpa81` версию пакета NuGet PCL по умолчанию, поэтому необходимо настроить ссылку вручную.

## <a name="details-about-the-three-layers-of-errors"></a>Сведения об этих трех уровнях ошибок

1. Сборка **System. Diagnostics. Tracing. dll** фасадной в настоящее время отсутствует во всех версиях Windows Xamarin. Android (не общедоступной ошибки 34888) и отсутствует во всех версиях Xamarin. iOS ниже 9,0 (или ниже, чем ксамаринвс 3.11.1443 в Windows) (исправлено в [Ошибка 32388](https://bugzilla.xamarin.com/show_bug.cgi?id=32388)). Эта проблема приведет к одной из следующих ошибок в зависимости от целевого объекта развертывания и параметров компоновщика:

    - Xamarin. Android. Common. targets: Ошибка: Исключение при загрузке сборок: System. IO. FileNotFoundException: Не удалось загрузить сборку System. Diagnostics. Tracing, версию = 4.0.0.0, культуру = Neutral, PublicKeyToken = b03f5f7f11d50a3a '. Возможно, он не существует в профиле Mono для Android?

    - Не удалось загрузить файл или сборку "System. Diagnostics. Tracing" или одну из ее зависимостей. Не удается найти указанный файл. (System. IO. FileNotFoundException)

    - MTOUCH: ошибка MT3001: Не удалось AOT сборку "/Users/macuser/Projects/TPLDataflow/UnifiedSingleViewIphone1/obj/iPhone/Debug/mtouch-cache/64/Build/System.Threading.Tasks.Dataflow.dll"

    - MTOUCH: ошибка MT2002: Не удалось разрешить сборку: "System. Диагностика. Tracing, версия = 4.0.0.0, культура = нейтральная, PublicKeyToken = b03f5f7f11d50a3a"

2. В текущей [реализации Mono типов в "System. Diagnostics. Tracing"](https://github.com/mono/mono/blob/master/mcs/class/corlib/System.Diagnostics.Tracing/EventSource.cs) отсутствуют некоторые перегрузки метода ([Ошибка 27337](https://bugzilla.xamarin.com/show_bug.cgi?id=27337)). Эта проблема вызовет одну из следующих ошибок компоновщика при создании приложения Xamarin:

    - /Либрари/фрамеворкс/моно.фрамеворк/екстернал/ксбуилд/ксамарин/андроид/ксамарин.андроид.Коммон.таржетс: ошибка: Ошибка при исполнении задачи Линкассемблиес: Error XA2006: Ссылка на элемент метаданных System. void System. Diagnostics. Tracing. EventSource:: WriteEvent (System. Int32, System. Object []) "(определен в" System. Threading. Tasks. DataFlow, Version = 4.5.24.0, Culture = Neutral, PublicKeyToken = b03f5f7f11d50a3a ") не удалось разрешить из "System. Threading. Tasks. DataFlow, Version = 4.5.24.0, Culture = Neutral, PublicKeyToken = b03f5f7f11d50a3a".

    - MTOUCH: ошибка MT2002: Не удалось разрешить ссылку "System. void System. Diagnostics. Tracing. EventSource:: WriteEvent (System. Int32; System. Object [])" из "System. Диагностика. Tracing, версия = 4.0.0.0, язык и региональные параметры = нейтральный, PublicKeyToken = b03f5f7f11d50a3a"

3. Текущая [Реализация Mono типов в "System. Diagnostics. Tracing"](https://github.com/mono/mono/blob/master/mcs/class/corlib/System.Diagnostics.Tracing/EventSource.cs) в настоящее время также является _пустой_ "фиктивной" реализацией ([Ошибка 34890](https://bugzilla.xamarin.com/show_bug.cgi?id=34890)). Любая попытка использовать эти методы во время выполнения может привести к непредвиденным результатам. Для _конкретного_ случая библиотеки потоков данных Microsoft TPL кажется, что вызовы `WriteEvent(System.Int32,System.Object[])` не являются обязательными для большей части поведения библиотеки, поэтому исправление "уровень 2" ([Ошибка 27337](https://bugzilla.xamarin.com/show_bug.cgi?id=27337), Добавление пустых реализаций), скорее всего, будет вполне достаточным. для большинства вариантов использования потоков данных TPL (Майкрософт).

## <a name="questions--answers"></a>Вопросы & ответы

### <a name="i-was-able-to-leave-linking-enabled-with-the-portable-net45win8wpa81-version-of-the-library-on-older-versions-of-xamarinios-or-on-xamarinandroid-how-did-that-work"></a>Я смог оставить компоновку доступной `portable-net45+win8+wpa81` для версии библиотеки в более старых версиях Xamarin. iOS или Xamarin. Android. Как это работало?

#### <a name="answer"></a>Клик

Сборка может завершиться успешно (с включенной компоновкой) в более старых версиях Xamarin. iOS или в Xamarin. Android на Mac, если включить ссылку на `System.Diagnostics.Tracing.dll` _сборку_ \[ссылок 1\]а не _фасадной сборка_ \[2], но, к сожалению, это не «правильный» обходной путь. Ссылочные сборки предназначены только для использования при создании _переносимых библиотек_, а не для кода, зависящего от платформы, например приложений. Попытка _запустить_ код, содержащийся в ссылочных сборках (а не только для сборки), скорее всего, приведет к непредвиденным результатам. Чтобы команда Mono добавит недостающую `WriteEvent(System.Int32,System.Object[])` перегрузку [`EventSource`](https://github.com/mono/mono/blob/master/mcs/class/corlib/System.Diagnostics.Tracing/EventSource.cs) к типу ([Ошибка 27337](https://bugzilla.xamarin.com/show_bug.cgi?id=27337)), необходимо исправить проблему. В настоящее же самое лучшее решение — переключиться `portable-net45+win8+wp8+wpa81` на версию библиотеки потоков данных Microsoft TPL, как описано в разделе обходной путь выше.

(Для всех, кто может прочитать эту статью после просмотра соответствующего более раннего, краткого ответа от StackOverflow (<https://stackoverflow.com/a/23591322/2561894>), обратите внимание, что различие между эталонными сборками и сборкой фасадной _не_ упомянуто там.)

**\[1\] "эталонная сборка"**

Windows: `C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETPortable\v4.5\System.Diagnostics.Tracing.dll`

Mac (моно):`/Library/Frameworks/Mono.framework/Versions/Current/lib/mono/xbuild-frameworks/.NETPortable/v4.5/System.Diagnostics.Tracing.dll`

**\[2\] расположения "фасад Assembly"**

Windows: `C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETFramework\v4.5\Facades\System.Diagnostics.Tracing.dll`

Mac (моно):`/Library/Frameworks/Mono.framework/Versions/Current/lib/mono/4.5/Facades/System.Diagnostics.Tracing.dll`


### <a name="will-it-help-if-i-manually-add-a-reference-to-the-systemdiagnosticstracing-facade-assembly"></a>Будет ли это помочь, если я вручную добавлю ссылку на сборку фасадной "System. Diagnostics. Tracing"?

_В частности, можно ли решить проблему с помощью этих двух шагов?_

1. _Скопируйте сборку `System.Diagnostics.Tracing.dll` фасадной в папку проекта приложения из одного из следующих расположений:_

    Windows: `C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETFramework\v4.5\Facades\System.Diagnostics.Tracing.dll`

    Mac (моно):`/Library/Frameworks/Mono.framework/Versions/Current/lib/mono/4.5/Facades/System.Diagnostics.Tracing.dll`

2. _Добавьте ссылку на сборку фасадной в проект приложения Xamarin. iOS или Xamarin. Android._

#### <a name="answer"></a>Клик

Нет. это не поможет.

- Для Xamarin. iOS 9,0 или любой последней версии Xamarin. Android в Windows это решение является строго избыточным и может вызвать ошибки компиляции, аналогичные "сборка System. Diagnostics. Tracing" с тем же удостоверением, уже импортирована. ".

- Для Xamarin. iOS 8,10 или более низкого уровня или для Xamarin. Android на Mac этот обходной путь будет полезен, но _только_ для проблемы отсутствия сборки уровня 1. Это _не_ позволит устранить ошибки компоновщика уровня 2, поэтому это решение не является полным.

### <a name="can-i-use-the-systemdiagnosticstracing-nuget-packagehttpswwwnugetorgpackagessystemdiagnosticstracing-to-solve-the-problem"></a>Можно ли использовать [пакет NuGet System. Diagnostics. Tracing](https://www.nuget.org/packages/System.Diagnostics.Tracing/) для решения проблемы?

#### <a name="answer"></a>Клик

Нет, пакет NuGet 3,0 "System. Diagnostics. Tracing" включает только реализации, зависящие от платформы, для "DNXCore50" и "netcore50". В нем явно _опущены_ реализации Xamarin. Android ("iOS") и Xamarin. iOS ("коtouch" и "ксамариниос"). Это означает, что установка пакета не будет _оказывать влияния_ на проекты Xamarin. Android и Xamarin. iOS. Пакет NuGet предполагает, что обе платформы предоставляют _собственную_ реализацию типов. Это предположение «правильный», в том смысле, что у _моно_ реализации пространства имен, а также как описано в разделе точек \#2 и \#3 из «Сведения о трех уровней ошибок» выше, Реализация в настоящее время неполон. Чтобы команда Mono могла устранить [ошибку 27337](https://bugzilla.xamarin.com/show_bug.cgi?id=27337) и [ошибку 34890](https://bugzilla.xamarin.com/show_bug.cgi?id=34890), нужно исправить проблему.

## <a name="next-steps"></a>Следующие шаги

Для получения дополнительной помощи, для связи с нами или если проблема остается даже после использования приведенных выше сведений, ознакомьтесь с возможностями [поддержки Xamarin?](~/cross-platform/troubleshooting/support-options.md) дополнительные сведения о вариантах контактов, предложениях и о том, как создать новую ошибку при необходимости .
