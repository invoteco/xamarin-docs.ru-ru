---
title: Коммутатор Xamarin.Forms
description: Параметр Xamarin.Forms — это тип кнопки, которые могут управляться пользователю переключаться между, включение и отключение состояния. В этой статье описывается использование класса Switch для отображения переключение видимости элемента пользовательского интерфейса.
ms.prod: xamarin
ms.assetId: B2F9CC65-481B-4323-8E77-C6BE29C90DE9
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 07/03/2019
ms.openlocfilehash: 22a17f9a916d94a3a0f44a451512de43c943e95a
ms.sourcegitcommit: 58d8bbc19ead3eb535fb8248710d93ba0892e05d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/09/2019
ms.locfileid: "67675031"
---
# <a name="xamarinforms-switch"></a>Коммутатор Xamarin.Forms

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/SwitchDemos)

Xamarin.Forms [ `Switch` ](xref:Xamarin.Forms.Switch) является горизонтальной выключатель, который может управляться пользователю переключаться между Включение и отключение состояния, которые представлены `boolean` значение. `Switch` Класс наследует от [ `View` ](xref:Xamarin.Forms.View).

На следующем снимке экрана показан `Switch` контролировать его **на** и **off** переключения состояний в iOS и Android:

![Снимок экрана переключений в Включение и отключение состояния, в iOS и Android](switch-images/switch-states-default.png "переключается на iOS и Android")

`Switch` Управления определяет два свойства:

* [`OnColor`](xref:Xamarin.Forms.Switch.OnColor) — `Color` , влияющее на способ `Switch` — к просмотру включена, или **на**, состояние.
* [`IsToggled`](xref:Xamarin.Forms.Switch.IsToggled) — `boolean` значение, указывающее, является ли `Switch` — **на**.

Эти свойства поддерживаются [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) объектом, то есть `Switch` можно применить различные стили и быть целевым объектом привязки данных.

`Switch` Управления определяет `Toggled` событие, возникающее, когда `IsToggled` изменения свойств, либо через операции пользователя, или когда приложение задает `IsToggled` свойства. `ToggledEventArgs` Объект, который прилагается к `Toggled` событие имеет одно свойство с именем `Value`, типа `bool`. При возникновении этого события значение `Value` свойство отражает новое значение `IsToggled` свойства.

## <a name="create-a-switch"></a>Создайте коммутатор

Объект `Switch` может быть создано в XAML. Его `IsToggled` может быть установлено для переключения `Switch`. По умолчанию `IsToggled` свойство `false`. В следующем примере показано, как создать экземпляр `Switch` в XAML с помощью необязательного `IsToggled` набор свойств:

```xaml
<Switch IsToggled="true"/>
```

Объект `Switch` также могут создаваться в коде:

```csharp
Switch switch = new Switch { IsToggled = true };
```

### <a name="switch-style-properties"></a>Свойства стиля коммутатора

`OnColor` Для определения можно задать свойство `Switch` цвета, когда оно заменяется его **на** состояние. В следующем примере показано, как создать экземпляр `Switch` в XAML с `OnColor` набор свойств:

```xaml
<Switch OnColor="Orange" />
```

`OnColor` Также можно задать свойство, при создании `Switch` в коде:

```csharp
Switch switch = new Switch { OnColor = Color.Orange };
```

На следующем снимке экрана показан `Switch` в его **на** и **off** переключаться с состояния, `OnColor` свойство значение `Color.Orange` в iOS и Android:

![Снимок экрана переключений в Включение и отключение состояния, в iOS и Android](switch-images/switch-states-oncolor.png "переключается на iOS и Android")

## <a name="respond-to-a-switch-state-change"></a>Ответ на изменение состояния коммутатора

Когда `IsToggled` изменения свойств, либо через операции пользователя, или когда приложение задает `IsToggled` свойства `Toggled` вызывает событие. Обработчик событий для этого события могут быть зарегистрированы в ответ на изменение:

```xaml
<Switch Toggled="OnToggled" />
```

Файл кода содержит обработчик `Toggled` событий:

```csharp
void OnToggled(object sender, ToggledEventArgs e)
{
    // Perform an action after examining e.Value
}
```

`sender` Аргумент в обработчике событий является `Switch` отвечающий за это событие. Можно использовать `sender` свойство для доступа к `Switch` объекта, или чтобы различать несколько `Switch` объектов с одинаковым `Toggled` обработчик событий.

`Toggled` Обработчик событий также могут быть назначены в коде:

```csharp
Switch switch = new Switch {...};
switch.Toggled += (sender, e) =>
{
    // Perform an action after examining e.Value
}
```

## <a name="data-bind-a-switch"></a>Параметр привязки данных

`Toggled` Обработчик событий, которые можно устранить с помощью привязки данных и триггеров реагировать на `Switch` изменения состояния переключателя.

```xaml
<Switch x:Name="styleSwitch" />
<Label Text="Lorem ipsum dolor sit amet, elit rutrum, enim hendrerit augue vitae praesent sed non, lorem aenean quis praesent pede.">
    <Label.Triggers>
        <DataTrigger TargetType="Label"
                     Binding="{Binding Source={x:Reference styleSwitch}, Path=IsToggled}"
                     Value="true">
            <Setter Property="FontAttributes"
                    Value="Italic, Bold" />
            <Setter Property="FontSize"
                    Value="Large" />
        </DataTrigger>
    </Label.Triggers>
</Label>
```

В этом примере [ `Label` ](xref:Xamarin.Forms.Label) используется выражение привязки в `DataTrigger` для наблюдения за `IsToggled` свойство `Switch` с именем `styleSwitch`. Когда это свойство становится `true`, `FontAttributes` и `FontSize` свойства `Label` изменяются. Когда `IsToggled` свойство возвращается `false`, `FontAttributes` и `FontSize` свойства `Label` сбрасываются в изначальное состояние.

Сведения о триггерах см. в разделе [Xamarin.Forms триггеры](~/xamarin-forms/app-fundamentals/triggers.md).

## <a name="disable-a-switch"></a>Отключить параметр

Приложение может переходить в состояние где `Switch` соседней с ним не является допустимой операцией. В таких случаях `Switch` можно отключить, задав его `IsEnabled` свойства `false`. Это позволит пользователям возможность управлять `Switch`.

## <a name="related-links"></a>Связанные ссылки

* [Демонстрации коммутатора](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/SwitchDemos)
* [Триггеры Xamarin.Forms](~/xamarin-forms/app-fundamentals/triggers.md)
