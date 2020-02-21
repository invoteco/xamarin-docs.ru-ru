---
title: Переключатель Xamarin. Forms
description: Параметр Xamarin. Forms — это тип кнопки, которая может управляться пользователем для переключения между состояниями. В этой статье объясняется, как использовать класс Switch для отображения переключаемого элемента пользовательского интерфейса.
ms.prod: xamarin
ms.assetId: B2F9CC65-481B-4323-8E77-C6BE29C90DE9
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 07/18/2019
ms.openlocfilehash: 88655aabdbd32db63aaf3330a18b0ad8105ea26c
ms.sourcegitcommit: b751605179bef8eee2df92cb484011a7dceb6fda
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/20/2020
ms.locfileid: "77506535"
---
# <a name="xamarinforms-switch"></a>Переключатель Xamarin. Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-switchdemos/)

Элемент управления Xamarin. Forms [`Switch`](xref:Xamarin.Forms.Switch) является горизонтальной выключателем, с помощью которого пользователь может переключаться между состояниями включения и выключения, которые представлены `boolean`ным значением. Класс `Switch` наследует от [`View`](xref:Xamarin.Forms.View).

На следующих снимках экрана показан элемент управления `Switch` в состояниях переключателя **On** и **Off** в iOS и Android:

![Снимок экрана параметров включения и отключения состояний, в iOS и Android](switch-images/switch-states-default.png "Параметры в iOS и Android")

Элемент управления `Switch` определяет следующие свойства:

* [`IsToggled`](xref:Xamarin.Forms.Switch.IsToggled) — это `boolean` значение, указывающее **, включено ли `Switch`.**
* [`OnColor`](xref:Xamarin.Forms.Switch.OnColor) — это `Color`, который влияет на отображение `Switch` в переключенном или **включенном**состоянии.
* `ThumbColor` является `Color`ом бегунка переключателя.

Эти свойства поддерживаются объектом [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) , что означает, что `Switch` можно присвоить стилю и быть целевым объектом привязок данных.

Элемент управления `Switch` определяет `Toggled` событие, которое возникает при изменении свойства `IsToggled` либо с помощью манипуляции пользователя, либо когда приложение задает свойство `IsToggled`. Объект `ToggledEventArgs`, сопровождающий событие `Toggled`, имеет одно свойство с именем `Value`типа `bool`. При срабатывании события значение свойства `Value` отражает новое значение свойства `IsToggled`.

## <a name="create-a-switch"></a>Создание переключателя

В XAML можно создать экземпляр `Switch`. Его свойство `IsToggled` можно задать для переключения `Switch`. По умолчанию свойство `IsToggled` `false`. В следующем примере показано, как создать экземпляр `Switch` в XAML с помощью необязательного набора свойств `IsToggled`:

```xaml
<Switch IsToggled="true"/>
```

`Switch` также можно создать в коде:

```csharp
Switch switchControl = new Switch { IsToggled = true };
```

## <a name="switch-appearance"></a>Переключить внешний вид

В дополнение к свойствам, которые [`Switch`](xref:Xamarin.Forms.Switch) наследуют от класса [`View`](xref:Xamarin.Forms.View) , `Switch` также определяет свойства `OnColor` и `ThumbColor`. Свойство `OnColor` может быть задано, чтобы определить цвет `Switch`, когда он переключается в состояние **On** , а свойство `ThumbColor` можно задать, чтобы определить `Color` ползунка переключателя. В следующем примере показано, как создать экземпляр `Switch` в XAML со следующими наборами свойств:

```xaml
<Switch OnColor="Orange"
        ThumbColor="Green" />
```

Свойства также можно задать при создании `Switch` в коде:

```csharp
Switch switch = new Switch { OnColor = Color.Orange, ThumbColor = Color.Green };
```

На следующем снимке экрана показано `Switch` в состояниях переключателя **вкл** . и **откл** . свойства `OnColor` и `ThumbColor` задаются следующим образом:

![Снимок экрана параметров включения и отключения состояний, в iOS и Android](switch-images/switch-states-colors.png "Параметры в iOS и Android")

## <a name="respond-to-a-switch-state-change"></a>Ответ на изменение состояния переключения

При изменении свойства `IsToggled` либо с помощью пользовательской манипуляции, либо когда приложение задает свойство `IsToggled`, срабатывает событие `Toggled`. Для реагирования на изменение можно зарегистрировать обработчик событий для этого события:

```xaml
<Switch Toggled="OnToggled" />
```

Файл кода программной части содержит обработчик события `Toggled`:

```csharp
void OnToggled(object sender, ToggledEventArgs e)
{
    // Perform an action after examining e.Value
}
```

Аргумент `sender` в обработчике событий является `Switch`, ответственным за срабатывание этого события. Можно использовать свойство `sender` для доступа к объекту `Switch` или для различения нескольких объектов `Switch`, совместно использующих один и тот же обработчик событий `Toggled`.

Обработчик событий `Toggled` можно также назначить в коде:

```csharp
Switch switchControl = new Switch {...};
switchControl.Toggled += (sender, e) =>
{
    // Perform an action after examining e.Value
}
```

## <a name="data-bind-a-switch"></a>Привязка данных коммутатора

Обработчик событий `Toggled` можно исключить с помощью привязки данных и триггеров для реагирования на `Switch` изменение состояния переключения.

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

В этом примере [`Label`](xref:Xamarin.Forms.Label) использует выражение привязки в `DataTrigger` для отслеживания свойства `IsToggled` `Switch` с именем `styleSwitch`. Когда это свойство переходит `true`, изменяются свойства `FontAttributes` и `FontSize` `Label`. Когда свойство `IsToggled` возвращает значение в `false`, свойства `FontAttributes` и `FontSize` `Label` сбрасываются в исходное состояние.

Дополнительные сведения о триггерах см. в разделе [триггеры Xamarin. Forms](~/xamarin-forms/app-fundamentals/triggers.md).

## <a name="disable-a-switch"></a>Отключение переключателя

Приложение может перейти в состояние, в котором `Switch`, для которого выполняется переключение, не является допустимой операцией. В таких случаях `Switch` можно отключить, задав свойству `IsEnabled` значение `false`. Это не позволит пользователям работать с `Switch`.

## <a name="related-links"></a>Связанные ссылки

* [Переключить демонстрации](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-switchdemos/)
* [Триггеры Xamarin. Forms](~/xamarin-forms/app-fundamentals/triggers.md)
