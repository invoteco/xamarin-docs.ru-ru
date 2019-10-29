---
title: начало работы с помощью пакета SDK для Xamarin Workbooks
description: В этом документе описывается, как приступить к работе с пакетом SDK для Xamarin Workbooks, который можно использовать для разработки интеграции для Xamarin Workbooks.
ms.prod: xamarin
ms.assetid: FAED4445-9F37-46D8-B408-E694060969B9
author: davidortinau
ms.author: daortin
ms.date: 03/30/2017
ms.openlocfilehash: e4a9e9113f83dd89b622de3e1f74f458efd4f07f
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73018690"
---
# <a name="getting-started-with-the-xamarin-workbooks-sdk"></a>начало работы с помощью пакета SDK для Xamarin Workbooks

Этот документ содержит краткое руководство по началу разработки интеграции для Xamarin Workbooks. Большая часть этого будет работать с стабильными Xamarin Workbooks, но **Загрузка интеграции через пакеты NuGet поддерживается только в книгах 1,3**, в альфа-канале на момент написания статьи.

## <a name="general-overview"></a>Общие сведения

Интеграция Xamarin Workbooks — это небольшие библиотеки, использующие пакет SDK для [NuGet`Xamarin.Workbooks.Integrations`][nuget] для интеграции с агентами Xamarin Workbooks и инспектора для предоставления расширенных возможностей.

Есть три основных шага по началу разработки интеграции — мы будем структурировать их здесь.

## <a name="creating-the-integration-project"></a>Создание проекта интеграции

Библиотеки интеграции лучше разрабатывать как многоплатформенные библиотеки. Так как вы хотите обеспечить лучшую интеграцию со всеми доступными агентами, в прошлом и будущем, вам нужно выбрать обширный поддерживаемый набор библиотек. Для широкой поддержки рекомендуется использовать шаблон "Переносимая библиотека":

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio для Mac](#tab/macos)

[Шаблон![переносимой библиотеки Visual Studio для Mac](images/xamarin-studio-pcl.png)](images/xamarin-studio-pcl.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[Шаблон переносимой библиотеки![Visual Studio](images/visual-studio-pcl.png)](images/visual-studio-pcl.png#lightbox)

В Visual Studio необходимо выбрать следующие целевые платформы для переносимой библиотеки:

[Платформы переносимых библиотек![Visual Studio](images/visual-studio-pcl-platforms.png)](images/visual-studio-pcl-platforms.png#lightbox)

-----

После создания проекта библиотеки добавьте ссылку на нашу `Xamarin.Workbooks.Integration` библиотеку NuGet с помощью диспетчера пакетов NuGet.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio для Mac](#tab/macos)

[![NuGet Visual Studio для Mac](images/xamarin-studio-nuget.png)](images/xamarin-studio-nuget.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![NuGet Visual Studio](images/visual-studio-nuget.png)](images/visual-studio-nuget.png#lightbox)

-----

Вы хотите удалить пустой класс, который будет создан в рамках проекта — для этого вам не понадобится. После выполнения этих действий можно приступать к созданию интеграции.

## <a name="building-an-integration"></a>Создание интеграции

Мы создадим простую интеграцию. Мы очень рады зеленому цвету, поэтому мы добавим зеленый цвет в виде представления для каждого объекта. Сначала создайте новый класс с именем `SampleIntegration`и реализуем наш интерфейс `IAgentIntegration`:

```csharp
using Xamarin.Interactive;

public class SampleIntegration : IAgentIntegration
{
    public void IntegrateWith (IAgent agent)
    {
    }
}
```

Мы хотим добавить [представление](~/tools/workbooks/sdk/representations.md) для каждого объекта, который является зеленым цветом. Мы выполним это с помощью поставщика представлений. Поставщики наследуют от класса `RepresentationProvider`. для нас нужно просто переопределить `ProvideRepresentations`:

```csharp
using Xamarin.Interactive.Representations;

class SampleRepresentationProvider : RepresentationProvider
{
    public override IEnumerable<object> ProvideRepresentations (object obj)
    {
        // This corresponds to Pantone 2250 XGC, our favorite color.
        yield return new Color (0.0, 0.702, 0.4314);
    }
}
```

Мы возвращаем `Color`, предварительно созданный тип представления в пакете SDK.
Вы заметите, что возвращаемый тип здесь является `IEnumerable<object>`&mdash;один поставщик представлений может возвращать множество представлений для объекта! Все поставщики представлений вызываются для каждого объекта, поэтому важно не делать никаких предположений относительно того, какие объекты передаются вам.

Заключительным этапом является факт регистрации поставщика с помощью агента и указания книг, где найти наш тип интеграции. Чтобы зарегистрировать поставщик, добавьте этот код в метод `IntegrateWith` в созданном ранее классе `SampleIntegration`:

```csharp
agent.RepresentationManager.AddProvider (new SampleRepresentationProvider ());
```

Настройка типа интеграции выполняется через атрибут на уровне сборки. Это можно разместить в AssemblyInfo.cs или в том же классе, что и тип интеграции для удобства:

```csharp
[assembly: AgentIntegration (typeof (SampleIntegration))]
````

Во время разработки может оказаться удобнее использовать перегрузки `AddProvider` на `RepresentationManager`, которые позволяют зарегистрировать простой обратный вызов для представления в книге, а затем переместить этот код в реализацию `RepresentationProvider` после завершения работы. Пример визуализации [`OxyPlot`][oxyplot] `PlotModel` может выглядеть следующим образом:

```csharp
InteractiveAgent.RepresentationManager.AddProvider<PlotModel> (
  plotModel => Image (new SvgExporter {
      Width = 300,
      Height = 250
    }.ExportToString (plotModel)));
```

> [!NOTE]
> Эти API позволяют быстро приступить к работе, но мы не рекомендуем предоставлять полную интеграцию только с помощью их&mdash;они обеспечивают очень небольшой контроль над тем, как ваши типы обрабатываются клиентом.

После регистрации представления ваша интеграция готова к поставке.

## <a name="shipping-your-integration"></a>Доставка интеграции

Чтобы отправить интеграцию, необходимо добавить ее в пакет NuGet.
Вы можете отправить его с помощью NuGet существующей библиотеки или, если вы создаете новый пакет, вы можете использовать этот файл Template. nuspec в качестве отправной точки.
Вам потребуется заполнить разделы, относящиеся к вашей интеграции. Наиболее важной частью является то, что все файлы для интеграции должны находиться в каталоге `xamarin.interactive` в корне пакета. Это позволяет нам легко найти все необходимые файлы для интеграции, независимо от того, используется ли существующий пакет или создается новый.

```xml
<?xml version="1.0"?>
<package xmlns="http://schemas.microsoft.com/packaging/2010/07/nuspec.xsd">
    <metadata>
      <id>$YourNuGetPackage$</id>
      <version>$YourVersion$</version>
      <authors>$YourNameHere$</authors>
      <projectUrl>$YourProjectPage$</projectUrl>
      <description>A short description of your library.</description>
    </metadata>
    <files>
      <file src="Path\To\Your\Integration.dll" target="xamarin.interactive" />
    </files>
</package>
```

После создания файла nuspec можно упаковать NuGet следующим образом:

```csharp
nuget pack MyIntegration.nuspec
```

а затем опубликовать его в [NuGet][nugetorg]. После этого вы сможете ссылаться на него из любой книги и видеть ее в действии. На снимке экрана ниже мы создали пример интеграции, который мы создали в этом документе и установили пакет NuGet в книге:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio для Mac](#tab/macos)

[![книги с интеграцией](images/mac-workbooks-integrated.png)](images/mac-workbooks-integrated.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![книги с интеграцией](images/windows-workbooks-integrated.png)](images/windows-workbooks-integrated.png#lightbox)

-----

Обратите внимание, что не отображаются директивы `#r` или что-либо для инициализации интеграции — книги потратили все это на все это за кулисами.

## <a name="next-steps"></a>Следующие шаги

Ознакомьтесь с нашей другой документацией, чтобы получить дополнительные сведения о перемещенных элементах, составляющих пакет SDK, и наших [примерах интеграции](~/tools/workbooks/samples/index.md) для выполнения дополнительных задач, которые можно выполнить с помощью интеграции, например предоставления пользовательского кода JavaScript, который выполняется в клиенте книги.

[nugetorg]: https://nuget.org
[nuget]: https://nuget.org/packages/Xamarin.Workbooks.Integration
[oxyplot]: http://www.oxyplot.org/
