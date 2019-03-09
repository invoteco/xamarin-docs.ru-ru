---
title: Устранить проблемы, связанные с System.Diagnostics.Tracing и поток данных TPL
description: Практический пример PCL. Как устранять проблемы, связанные с System.Diagnostics.Tracing для пакета NuGet потока данных библиотеки параллельных задач Майкрософт
ms.prod: xamarin
ms.assetid: 7986A556-382D-4D00-ACCF-3589B4029DE8
ms.date: 04/17/2018
author: asb3993
ms.author: amburns
ms.openlocfilehash: d9aa85b946f20addb7d69c559bff68c6b1f75429
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/08/2019
ms.locfileid: "57669847"
---
# <a name="pcl-case-study-how-can-i-resolve-problems-related-to-systemdiagnosticstracing-for-the-microsoft-tpl-dataflow-nuget-package"></a>Практический пример PCL. Как устранять проблемы, связанные с System.Diagnostics.Tracing для пакета NuGet потока данных библиотеки параллельных задач Майкрософт

> [!IMPORTANT]
> Данном конкретном примере из `System.Diagnostic.Tracing` больше не создает ошибок по умолчанию в последних версиях Xamarin. Хотя предлагаемые обходной путь, по-прежнему будет работать, обратите внимание, что некоторые из ошибок, перечисленных в разделе «уровни ошибок» были исправлены.
> Кроме того следует отметить, что .NET Standard стала предпочтительным способом реализации кроссплатформенные API-интерфейсы .NET.

## <a name="summary"></a>Сводка

Xamarin.iOS и Xamarin.Android не следует реализовывать каждый профиль PCL, они позволяют как ссылки на 100%. Для практического удобства в Visual Studio для Mac, Visual Studio и диспетчер пакетов NuGet, проекты Xamarin позволяют использовать несколько профилей, которые имеют только _неполные_ реализаций. Например, ни Xamarin.iOS и Xamarin.Android в настоящее время включает в себя полную реализацию типы в «System.Diagnostics.Tracing» переносимой библиотеки Классов пространства имен. Это ограничение приводит к три уровня ошибок, при попытке использовать значение по умолчанию `portable-net45+win8+wpa81` версию пакета NuGet потоков данных TPL Microsoft.

## <a name="workaround-switch-the-app-project-to-reference-the-portable-net45win8wp8wpa81-version-of-the-tpl-dataflow-library"></a>Решение Переключить проект приложения для ссылки на `portable-net45+win8+wp8+wpa81` версию библиотеки потоков данных TPL

(Это позволяет избежать все три уровня ошибок и работает для всех последних версиях Xamarin.)

1. Откройте проект приложения **.csproj** файл в текстовом редакторе.

2. Найдите строку, которая выглядит примерно так:

    ```xml
    <HintPath>..\packages\Microsoft.Tpl.Dataflow.4.5.24\lib\portable-net45+win8+wpa81\System.Threading.Tasks.Dataflow.dll</HintPath>
    ```

3. Изменение `portable-net45+win8+wpa81` для `portable-net45+win8+wp8+wpa81` (`+wp8` добавляется):

    ```xml
    <HintPath>..\packages\System.Threading.Tasks.Dataflow.4.5.25\lib\portable-net45+win8+wp8+wpa81\System.Threading.Tasks.Dataflow.dll</HintPath>
    ```

### <a name="explanation"></a>Объяснение

`portable-net45+win8+wp8+wpa81` Не ссылается на версию библиотеки **System.Diagnostics.Tracing.dll** _вообще_, поэтому она полностью позволяет избежать все три уровня проблем.

### <a name="limitations"></a>Ограничения

- `portable-net45+win8+wp8+wpa81` Версии библиотеки могут не включать 100% функциональные возможности `portable-net45+win8+wpa81` версии.

- Диспетчер пакетов NuGet устанавливает `portable-net45+win8+wpa81` версию пакета PCL NuGet по умолчанию, поэтому необходимо вручную изменить ссылки.

## <a name="details-about-the-three-layers-of-errors"></a>Сведения о трех уровнях ошибок

1. **System.Diagnostics.Tracing.dll** видимой сборкой является в настоящее время отсутствуют все версии Mac Xamarin.Android (закрытые ошибки 34888) и исключаются из всех версий Xamarin.iOS ниже, чем 9.0 (или ниже, чем XamarinVS 3.11.1443 в Windows) (устранена в [ошибки 32388](https://bugzilla.xamarin.com/show_bug.cgi?id=32388)). Эту проблему вызывает один из следующих ошибок в зависимости от цели развертывания и компоновщик параметры:

    - Xamarin.Android.Common.targets: Ошибка: Возникло исключение при загрузке сборки: System.IO.FileNotFoundException: Не удалось загрузить сборку "System.Diagnostics.Tracing, Version = 4.0.0.0, Culture = neutral, PublicKeyToken = b03f5f7f11d50a3a". Возможно она не существует в Mono для Android профиля?

    - Не удалось загрузить файл или сборку «System.Diagnostics.Tracing» или одну из ее зависимостей. Не удается найти указанный файл. (System.IO.FileNotFoundException)

    - MTOUCH: ошибка MT3001: Может не сборки AOT "/ Users/macuser/Projects/TPLDataflow/UnifiedSingleViewIphone1/obj/iPhone/Debug/mtouch-cache/64/Build/System.Threading.Tasks.Dataflow.dll"

    - MTOUCH: ошибка MT2002: Не удалось разрешить сборку: 'System.Diagnostics.Tracing, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a'

2. Текущий [реализацию Mono среды типы в «System.Diagnostics.Tracing»](https://github.com/mono/mono/blob/master/mcs/class/corlib/System.Diagnostics.Tracing/EventSource.cs) отсутствуют некоторые перегрузки метода ([ошибки 27337](https://bugzilla.xamarin.com/show_bug.cgi?id=27337)). Эта проблема приведет к одной из следующих ошибок компоновщика, при создании приложения Xamarin:

    - / Library/Frameworks/Mono.framework/External/xbuild/Xamarin/Android/Xamarin.Android.Common.targets: ошибка: Ошибка при выполнении задачи LinkAssemblies: ошибка XA2006: Ссылка на метаданные элементов «System.Void System.Diagnostics.Tracing.EventSource::WriteEvent(System.Int32,System.Object[])» (определенный в "System.Threading.Tasks.Dataflow, Version = 4.5.24.0, язык и региональные параметры = neutral, PublicKeyToken = b03f5f7f11d50a3a") из "System.Threading.Tasks.Dataflow, Version = 4.5.24.0, язык и региональные параметры = neutral, PublicKeyToken = b03f5f7f11d50a3a" не удалось разрешить.

    - MTOUCH: ошибка MT2002: Не удалось разрешить ссылку «System.Void System.Diagnostics.Tracing.EventSource::WriteEvent(System.Int32,System.Object[])» из «System.Diagnostics.Tracing, Version = 4.0.0.0, язык и региональные параметры = neutral, PublicKeyToken = b03f5f7f11d50a3a»

3. Текущий [реализацию Mono среды типы в «System.Diagnostics.Tracing»](https://github.com/mono/mono/blob/master/mcs/class/corlib/System.Diagnostics.Tracing/EventSource.cs) настоящий момент также _пустой_ «пустого» реализация ([ошибки 34890](https://bugzilla.xamarin.com/show_bug.cgi?id=34890)). Таким образом, любая попытка использовать эти методы во время выполнения может дать непредвиденные результаты. Для _определенного_ случай библиотеки потоков данных TPL Microsoft, кажется, вызовы `WriteEvent(System.Int32,System.Object[])` не нужны для большей части библиотеки поведение, поэтому исправление «уровня 2» ([ошибки 27337](https://bugzilla.xamarin.com/show_bug.cgi?id=27337), Добавление пустой реализации), скорее всего будет достаточно для большинства случаев использования потоков данных TPL Microsoft.

## <a name="questions--answers"></a>Вопросы и ответы

### <a name="i-was-able-to-leave-linking-enabled-with-the-codeportable-net45win8wpa81code-version-of-the-library-on-older-versions-of-xamarinios-or-on-xamarinandroid-how-did-that-work"></a>Я смог оставьте связывания, снабженный <code>portable-net45+win8+wpa81</code> версии библиотеки на более старых версиях Xamarin.iOS или Xamarin.Android. Как сработало?

#### <a name="answer"></a>Ответ

Это _возможных_ по сборки для завершения «успешно» (с компоновки включено) в более старых версиях Xamarin.iOS или Xamarin.Android на компьютере Mac при включении ссылку `System.Diagnostics.Tracing.dll` _ссылку на сборку_ \[1\] вместо _видимой сборкой_ \[2], но увы это делается не «правильный». Ссылочные сборки предназначены только для использования при построении _переносимых библиотек_, код не специфические для платформы, такими как приложения. Попытка _запуска_ кода, содержащегося в ссылочные сборки (а не только сборки, для ее), вероятнее всего будут давать непредвиденные результаты. Правильным решением будет Mono группой Добавьте отсутствующее `WriteEvent(System.Int32,System.Object[])` перегруженная версия будет [ `EventSource` ](https://github.com/mono/mono/blob/master/mcs/class/corlib/System.Diagnostics.Tracing/EventSource.cs) тип ([ошибки 27337](https://bugzilla.xamarin.com/show_bug.cgi?id=27337)). Для теперь лучше переключиться на `portable-net45+win8+wp8+wpa81` версию библиотеки потоков данных TPL Microsoft, как описано в предыдущем разделе инструкции по решению.

(Для всех, кто может прочитать эту статью, увидев связанных более старые, более компактно ответ в StackOverflow (<https://stackoverflow.com/a/23591322/2561894>), обратите внимание, что был различие между ссылочных сборок и видимой сборкой _не_ упоминалось существует.)

**\[1\] расположения «Ссылочные сборки»**

Windows: `C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETPortable\v4.5\System.Diagnostics.Tracing.dll`

Mac (Mono): `/Library/Frameworks/Mono.framework/Versions/Current/lib/mono/xbuild-frameworks/.NETPortable/v4.5/System.Diagnostics.Tracing.dll`

**\[2\] расположения «Facade сборок»**

Windows: `C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETFramework\v4.5\Facades\System.Diagnostics.Tracing.dll`

Mac (Mono): `/Library/Frameworks/Mono.framework/Versions/Current/lib/mono/4.5/Facades/System.Diagnostics.Tracing.dll`


### <a name="will-it-help-if-i-manually-add-a-reference-to-the-systemdiagnosticstracing-facade-assembly"></a>Он поможет, если вручную добавить ссылку на сборку фасадной «System.Diagnostics.Tracing»?

_В частности можно устранить проблему, используя 2 шага?_

1. _Копировать `System.Diagnostics.Tracing.dll` видимой сборкой в папке проекта приложения из одного из следующих расположений:_

    Windows: `C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETFramework\v4.5\Facades\System.Diagnostics.Tracing.dll`

    Mac (Mono): `/Library/Frameworks/Mono.framework/Versions/Current/lib/mono/4.5/Facades/System.Diagnostics.Tracing.dll`

2. _Добавьте ссылку на сборку фасадной в проекте приложения Xamarin.iOS или Xamarin.Android._

#### <a name="answer"></a>Ответ

Нет, это не поможет.

- Для Xamarin.iOS 9.0 или любой текущей версии Xamarin.Android в Windows этот обходной путь является строго избыточным и может привести к ошибкам компиляции, аналогичную «сборка «System.Diagnostics.Tracing» с одинаковыми идентификаторами уже была импортирована.».

- Для Xamarin.iOS 8.10 или ниже или Xamarin.Android на компьютере Mac, поможет этого обходного пути, но _только_ для сборки проблемы отсутствующих «уровень 1». Он будет _не_ решить ошибки компоновщика «уровень 2», поэтому не полное решение.

### <a name="can-i-use-the-systemdiagnosticstracing-nuget-packagehttpswwwnugetorgpackagessystemdiagnosticstracing-to-solve-the-problem"></a>Могу ли я использовать [пакет System.Diagnostics.Tracing NuGet](https://www.nuget.org/packages/System.Diagnostics.Tracing/) для решения проблемы?

#### <a name="answer"></a>Ответ

Нет, пакет NuGet 3.0 «System.Diagnostics.Tracing» включает только реализации платформы «DNXCore50» и «netcore50». Он явно _опускает_ реализации для Xamarin.Android («MonoAndroid») и Xamarin.iOS («MonoTouch» и «xamarinios»). Это означает, что установка пакета будет иметь _не влияет_ для проектов Xamarin.Android и Xamarin.iOS. Пакет NuGet предполагает, что оба этих платформ предоставляют свои _собственные_ реализации типов. Это предположение «правильный», в том смысле, что у _моно_ реализации пространства имен, а также как описано в разделе точек \#2 и \#3 из «Сведения о трех уровней ошибок» выше, Реализация в настоящее время неполон. Поэтому для Mono команды для разрешения будут внесены соответствующие исправления [ошибки 27337](https://bugzilla.xamarin.com/show_bug.cgi?id=27337) и [ошибки 34890](https://bugzilla.xamarin.com/show_bug.cgi?id=34890).

## <a name="next-steps"></a>Следующие шаги

Дополнительная помощь, свяжитесь с нами, или если эта проблема остается даже после использования указанные выше сведения, ознакомьтесь с разделом [какие варианты поддержки доступны для Xamarin?](~/cross-platform/troubleshooting/support-options.md) сведения о параметрах связи, предложения, а также как файл новую ошибку, при необходимости.
