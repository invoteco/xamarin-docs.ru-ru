---
title: Представления в Xamarin Workbooks
description: В этом документе описывается конвейер представления Xamarin Workbooks, который позволяет отображать сложные результаты для любого кода, возвращающего значение.
ms.prod: xamarin
ms.assetid: 5C7A60E3-1427-47C9-A022-720F25ECB031
author: lobrien
ms.author: laobri
ms.date: 03/30/2017
ms.openlocfilehash: b61452fc21d81f427249825decee4f119c50abf0
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/26/2019
ms.locfileid: "68511501"
---
# <a name="representations-in-xamarin-workbooks"></a>Представления в Xamarin Workbooks

## <a name="representations"></a>Заверен

В рамках сеанса книги или инспектора код, который выполняется и возвращает результат (например, метод, возвращающий значение или результат выражения), обрабатывается через конвейер представлений в агенте. Все объекты, за исключением примитивов, таких как целые числа, будут отражены для создания интерактивных графов-элементов и будут проходить через процесс предоставления дополнительных представлений, которые клиент может визуализировать более богато. Объекты любого размера и глубины безопасно поддерживаются (включая циклы и бесконечные перечислимые числа) из-за отложенного и интерактивного отражения и удаленного взаимодействия.

Xamarin Workbooks предоставляет несколько типов, общих для всех агентов и клиентов, которые позволяют выполнять расширенную визуализацию результатов. `Color`— Это один из примеров такого типа, где, например, в iOS, агент отвечает за преобразование `CGColor` объектов или `UIColor` в `Xamarin.Interactive.Representations.Color` объект.

Помимо распространенных представлений, пакет SDK для интеграции предоставляет API для сериализации пользовательских представлений в агенте и отрисовки представлений в клиенте.

## <a name="external-representations"></a>Внешние представления

`Xamarin.Interactive.IAgent.RepresentationManager`предоставляет возможность зарегистрировать объект `RepresentationProvider`, который должен быть реализован интеграцией для преобразования из произвольного объекта в независимую форму для подготовки к просмотру. Эти независимые формы должны реализовывать `ISerializableObject` интерфейс.

При реализации `ISerializableObject` интерфейса добавляется метод Serialize, точно определяющий способ сериализации объектов. `Serialize` Метод предложит разработчику точно указать, какие свойства должны быть сериализованы, и какое конечное имя будет. Взглянув`KitchenSink` на `Person` объект в нашем [примере] [Sample], можно увидеть, как это работает:

```csharp
public sealed class Person : ISerializableObject
{
  public string Name { get; }

  // Rest of the code is omitted…

  void ISerializableObject.Serialize (ObjectSerializer serializer)
    => serializer.Property (nameof (Name), Name);
}
```

Если бы нам хотелось предоставить надмножество или подмножество свойств из исходного объекта, мы можем сделать это с помощью `Serialize`. Например, для предоставления предварительно вычисленного `Age` `Person`свойства можно использовать нечто вроде этого:

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
> Интерфейсы API, `ISerializableObject` которые непосредственно создают объекты, не должны обрабатываться. `RepresentationProvider` Если объект `ISerializableObject`, который требуется отобразить, **не** является, необходимо обменять его `RepresentationProvider`оболочкой в.

### <a name="rendering-a-representation"></a>Подготовка представления

Модули подготовки отчетов реализуются в JavaScript и имеют доступ к версии JavaScript объекта, представленного с помощью `ISerializableObject`. Копия JavaScript также будет иметь `$type` строковое свойство, указывающее имя типа .NET.

Мы рекомендуем использовать TypeScript для кода интеграции клиента, который, безусловно, компилируется в обычный JavaScript. В любом случае пакет SDK предоставляет [вводы][typings] , на которые можно ссылаться непосредственно с помощью TypeScript, или просто называть их вручную, если предпочтительно написание JavaScript обычный.

Основная точка интеграции для подготовки к просмотру `xamarin.interactive.RendererRegistry`:

```js
xamarin.interactive.RendererRegistry.registerRenderer(
  function (source) {
    if (source.$type === "SampleExternalIntegration.Person")
      return new PersonRenderer;
    return undefined;
  }
);
```

`PersonRenderer` Здесь`Renderer` реализует интерфейс. Дополнительные сведения см. в разделе [Ввод][typings] .

[typings]: https://github.com/xamarin/Workbooks/blob/master/SDK/typings/xamarin-interactive.d.ts
