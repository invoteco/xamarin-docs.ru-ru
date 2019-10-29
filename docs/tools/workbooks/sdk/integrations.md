---
title: Разделы о расширенной интеграции
description: В этом документе описываются дополнительные темы, связанные с интеграцией Xamarin Workbooks. В нем обсуждается пакет NuGet Xamarin. Workbook. Integrations и интерфейс API в книге Xamarin.
ms.prod: xamarin
ms.assetid: 002CE0B1-96CC-4AD7-97B7-43B233EF57A6
author: davidortinau
ms.author: daortin
ms.date: 03/30/2017
ms.openlocfilehash: 159fecbe1385c091c56a6ececb61bf7d020dfc1b
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029600"
---
# <a name="advanced-integration-topics"></a>Разделы о расширенной интеграции

Сборки интеграции должны ссылаться на [`Xamarin.Workbooks.Integrations` NuGet][nuget]. Дополнительные сведения о начале работы с пакетом NuGet см. в [документации по быстрому запуску](~/tools/workbooks/sdk/index.md) .

Также поддерживаются интеграции клиентов, и они инициируются путем размещения файлов JavaScript или CSS с тем же именем, что и у сборки интеграции агента, в одном каталоге. Например, если сборка интеграции агента (которая ссылается на NuGet) имеет имя `SampleExternalIntegration.dll`, `SampleExternalIntegration.js` и `SampleExternalIntegration.css` будут интегрированы в клиент и, если они существуют. Интеграция с клиентами не является обязательной.

Внешняя интеграция может быть упакована как NuGet, предоставлена и указана непосредственно в приложении, где размещается агент, или просто помещено вместе с `.workbook` файлом, который хочет его использовать.

Внешние интеграции (агент и клиент) в пакетах NuGet будут автоматически загружаться при обращении к пакету, как и в случае с кратким руководством, в то время как сборки интеграции, поставляющиеся вместе с книгой, должны ссылаться на нее следующим образом:

```csharp
#r "SampleExternalIntegration.dll"
```

При ссылке на интеграцию таким образом, клиент не будет загружаться немедленно,&mdash;потребуется вызвать код из интеграции, чтобы загрузить его. Мы будем устранять эту ошибку в будущем.

`Xamarin.Interactive` PCL предоставляет несколько важных API-интерфейсов интеграции. Каждая интеграция должна по крайней мере предоставлять точку входа интеграции:

```csharp
using Xamarin.Interactive;

[assembly: AgentIntegration (typeof (AgentIntegration))]

class AgentIntegration : IAgentIntegration
{
    const string TAG = nameof (AgentIntegration);

    public void IntegrateWith (IAgent agent)
    {
        // hook into IAgent APIs
    }
}
```

На этом этапе после ссылки на сборку интеграции клиент будет неявно загружать файлы интеграции JavaScript и CSS.

## <a name="apis"></a>API - интерфейсы

Как и для любой сборки, на которую ссылается книга или сеанс интерактивного анализа, любой из его общедоступных API доступен для этого сеанса. Поэтому важно иметь надежную и понятную поверхность API для изучения пользователями.

Сборка интеграции — это фактический мост между приложением или пакетом SDK и сеансом. Он может предоставлять новые API, которые имеют смысл в контексте книги или сеанса интерактивного анализа или не предоставляют общедоступных API и просто выполняют «фоновые» задачи, такие как выдают [представления](~/tools/workbooks/sdk/representations.md)объектов.

> [!NOTE]
> API, которые должны быть общедоступными, но не должны отображаться с помощью IntelliSense, могут быть помечены обычным `[EditorBrowsable (EditorBrowsableState.Never)]` атрибутом.

[nuget]: https://nuget.org/packages/Xamarin.Workbooks.Integration
