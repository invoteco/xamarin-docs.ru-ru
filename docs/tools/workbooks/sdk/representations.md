---
title: Представления в Xamarin Workbooks
description: В этом документе описывается конвейер представления Xamarin Workbooks, который позволяет отображать сложные результаты для любого кода, возвращающего значение.
ms.prod: xamarin
ms.assetid: 5C7A60E3-1427-47C9-A022-720F25ECB031
author: davidortinau
ms.author: daortin
ms.date: 03/30/2017
ms.openlocfilehash: c1afba6943faa03ee07a3ec70624f668748041cb
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73018580"
---
# <a name="representations-in-xamarin-workbooks"></a>Представления в Xamarin Workbooks

## <a name="representations"></a>Заверен

В рамках сеанса книги или инспектора код, который выполняется и возвращает результат (например, метод, возвращающий значение или результат выражения), обрабатывается через конвейер представлений в агенте. Все объекты, за исключением примитивов, таких как целые числа, будут отражены для создания интерактивных графов-элементов и будут проходить через процесс предоставления дополнительных представлений, которые клиент может визуализировать более богато. Объекты любого размера и глубины безопасно поддерживаются (включая циклы и бесконечные перечислимые числа) из-за отложенного и интерактивного отражения и удаленного взаимодействия.

Xamarin Workbooks предоставляет несколько типов, общих для всех агентов и клиентов, которые позволяют выполнять расширенную визуализацию результатов. `Color` является одним из примеров такого типа, где, например, в iOS, агент отвечает за преобразование `CGColor` или `UIColor` объектов в объект `Xamarin.Interactive.Representations.Color`.

Помимо распространенных представлений, пакет SDK для интеграции предоставляет API для сериализации пользовательских представлений в агенте и отрисовки представлений в клиенте.

## <a name="external-representations"></a>Внешние представления

`Xamarin.Interactive.IAgent.RepresentationManager` предоставляет возможность регистрировать `RepresentationProvider`, которую интеграция должна реализовать для преобразования из произвольного объекта в независимую форму для отображения. Эти независимые формы должны реализовывать интерфейс `ISerializableObject`.

Реализация интерфейса `ISerializableObject` добавляет метод сериализации, точно определяющий способ сериализации объектов. Метод `Serialize` предложит разработчику точно указать, какие свойства должны быть сериализованы, и какое конечное имя будет. Просматривая объект `Person` в нашем [`KitchenSink` примере] [Sample], можно увидеть, как это работает:

```csharp
public sealed class Person : ISerializableObject
{
  public string Name { get; }

  // Rest of the code is omitted…

  void ISerializableObject.Serialize (ObjectSerializer serializer)
    => serializer.Property (nameof (Name), Name);
}
```

Если бы нам хотелось предоставить надмножество или подмножество свойств из исходного объекта, это можно сделать с помощью `Serialize`. Например, можно сделать что-то вроде этого, чтобы предоставить предварительно вычисленное свойство `Age` для `Person`:

```csharp
public sealed class Person : ISerializableObject
{
  public string Name { get; set; }
  public DateTime DateOfBirth { get; set; }

  // <snip>

  void ISerializableObject.Serialize (ObjectSerializer serializer)
  {
    serializer.Property (nameof (Name), Name);
    serializer.Property (nameof (DateOfBirth), DateOfBirth);

    // Let's pre-compute an Age property that's the person's age in years,
    // so we don't have to compute it in the renderer.
    var age = (DateTime.MinValue + (DateTime.Now - DateOfBirth)).Year - 1;
    serializer.Property ("Age", age)
  }
}
```

> [!NOTE]
> Интерфейсы API, которые создают объекты `ISerializableObject` напрямую, не должны обрабатываться `RepresentationProvider`. Если объект, который требуется отобразить, **не** является `ISerializableObject`, необходимо обменять его оболочкой в `RepresentationProvider`.

### <a name="rendering-a-representation"></a>Подготовка представления

Модули подготовки отчетов реализуются в JavaScript и имеют доступ к версии объекта JavaScript, представленного с помощью `ISerializableObject`. Копия JavaScript также будет иметь свойство `$type` String, указывающее имя типа .NET.

Мы рекомендуем использовать TypeScript для кода интеграции клиента, который, безусловно, компилируется в обычный JavaScript. В любом случае пакет SDK предоставляет [вводы][typings] , на которые можно ссылаться непосредственно с помощью TypeScript, или просто называть их вручную, если предпочтительно написание JavaScript обычный.

Главная точка интеграции для подготовки к просмотру — `xamarin.interactive.RendererRegistry`:

```js
xamarin.interactive.RendererRegistry.registerRenderer(
  function (source) {
    if (source.$type === "SampleExternalIntegration.Person")
      return new PersonRenderer;
    return undefined;
  }
);
```

Здесь `PersonRenderer` реализует интерфейс `Renderer`. Дополнительные сведения см. в разделе [Ввод][typings] .

[typings]: https://github.com/xamarin/Workbooks/blob/master/SDK/typings/xamarin-interactive.d.ts
