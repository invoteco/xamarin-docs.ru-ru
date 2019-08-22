---
title: Переключатель Xamarin. Forms
description: Параметр Xamarin. Forms — это тип кнопки, которая может управляться пользователем для переключения между состояниями. В этой статье объясняется, как использовать класс Switch для отображения переключаемого элемента пользовательского интерфейса.
ms.prod: xamarin
ms.assetId: B2F9CC65-481B-4323-8E77-C6BE29C90DE9
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 07/18/2019
ms.openlocfilehash: 1f2ef838287e32df5df42f73e4b43816d618552d
ms.sourcegitcommit: 5f972a757030a1f17f99177127b4b853816a1173
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/21/2019
ms.locfileid: "69887888"
---
# <a name="xamarinforms-switch"></a>Переключатель Xamarin. Forms

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-switchdemos/)

Элемент управления Xamarin. [`Switch`](xref:Xamarin.Forms.Switch) Forms является горизонтальной выключателем, с помощью которого пользователь может переключаться между состояниями включения и выключения, которые представлены `boolean` значением. Класс наследует от [`View`.](xref:Xamarin.Forms.View) `Switch`

На следующих снимках экрана `Switch` показан элемент управления в состояниях переключения и **отключения** в iOS и Android:

![Снимок экрана параметров включения и отключения состояний, в iOS и Android](switch-images/switch-states-default.png "Параметры в iOS и Android")

`Switch` Элемент управления определяет два свойства:

* [`IsToggled`](xref:Xamarin.Forms.Switch.IsToggled)значение, указывающее, включено ли в. `Switch` `boolean`
* [`OnColor`](xref:Xamarin.Forms.Switch.OnColor)параметр, который влияет на то `Switch` , как объект отображается в переключенном или **включенном**состоянии. `Color`
* `ThumbColor`значение `Color` параметра бегунка Switched.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектом. Это означает, `Switch` что можно использовать стиль и цель привязок данных.

Элемент управления определяет событие, которое `IsToggled` возникает при изменении свойстваспомощьюманипуляциипользователяиликогдаприложениезадаетсвойство.`IsToggled` `Toggled` `Switch` Объект, `Value`сопровождающий событие, имеет одно свойство с именем типа `bool`. `Toggled` `ToggledEventArgs` При срабатывании события значение `Value` свойства отражает новое значение `IsToggled` свойства.

## <a name="create-a-switch"></a>Создание переключателя

Экземпляр `Switch` можно создать в XAML. Его `IsToggled` свойство можно задать для `Switch`переключения. По умолчанию `IsToggled` свойство имеет `false`значение. В следующем примере показано, как создать экземпляр `Switch` в XAML с помощью необязательного `IsToggled` набора свойств:

```xaml
<Switch IsToggled="true"/>
```

Также `Switch` можно создать в коде:

```csharp
Switch switchControl = new Switch { IsToggled = true };
```

## <a name="switch-appearance"></a>Переключить внешний вид

В дополнение к свойствам, [`Switch`](xref:Xamarin.Forms.Switch) [`View`](xref:Xamarin.Forms.View) унаследованным от класса, `Switch` также определяет `OnColor` и `ThumbColor` свойства. `ThumbColor` `Switch` `Color` Свойство может быть задано для определения цвета, когда он переключается в состояние ON, а свойство может быть задано для определения типа бегунка переключателя. `OnColor` В следующем примере показано, как создать экземпляр `Switch` в XAML со следующими наборами свойств:

```xaml
<Switch OnColor="Orange"
        ThumbColor="Green" />
```

Свойства также можно задать при создании `Switch` в коде:

```csharp
Switch switch = new Switch { OnColor = Color.Orange, ThumbColor = Color.Green };
```

На следующем снимке экрана `Switch` показаны состояния переключателя **On** и **Off** , для которых `OnColor` заданы свойства и `ThumbColor` .

![Снимок экрана параметров включения и отключения состояний, в iOS и Android](switch-images/switch-states-colors.png "Параметры в iOS и Android")

## <a name="respond-to-a-switch-state-change"></a>Ответ на изменение состояния переключения

Когда свойство изменяется либо с помощью пользовательской манипуляции, либо когда приложение `IsToggled` задает свойство, `Toggled` возникает событие. `IsToggled` Для реагирования на изменение можно зарегистрировать обработчик событий для этого события:

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

Аргумент в обработчике событий `Switch` является ответственным за срабатывание этого события. `sender` `sender` Свойство можно использовать для `Switch` доступа к объекту или для различения нескольких `Switch` объектов, совместно использующих один `Toggled` и тот же обработчик событий.

Обработчик `Toggled` событий можно также назначить в коде:

```csharp
Switch switchControl = new Switch {...};
switchControl.Toggled += (sender, e) =>
{
    // Perform an action after examining e.Value
}
```

## <a name="data-bind-a-switch"></a>Привязка данных коммутатора

Обработчик событий можно исключить с помощью привязки данных и триггеров, чтобы реагировать `Switch` на изменяющиеся состояния переключения. `Toggled`

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

В [`Label`](xref:Xamarin.Forms.Label) этом примере компонент использует выражение привязки `IsToggled` `DataTrigger` в для `Switch` отслеживания свойства с именем `styleSwitch`. Когда это свойство примет `true`значение `FontAttributes` , изменяются свойства `Label` и `FontSize` объекта. `FontAttributes` `FontSize` Когда свойство возвращает значение `false`, свойства и объекта`Label` сбрасываются в исходное состояние. `IsToggled`

Дополнительные сведения о триггерах см. в разделе [триггеры Xamarin. Forms](~/xamarin-forms/app-fundamentals/triggers.md).

## <a name="disable-a-switch"></a>Отключение переключателя

Приложение может перейти в состояние, в котором `Switch` переключаемый объект не является допустимой операцией. В таких случаях `Switch` можно отключить, `IsEnabled` задав свойству `false`значение. Это предотвратит возможность пользователей управлять `Switch`.

## <a name="related-links"></a>Связанные ссылки

* [Переключить демонстрации](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-switchdemos/)
* [Триггеры Xamarin. Forms](~/xamarin-forms/app-fundamentals/triggers.md)
