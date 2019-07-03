---
title: Флажок Xamarin.Forms
description: Флажок Xamarin.Forms — это тип кнопки, которая может быть проверено, или пустой. Если флажок установлен, он рассматривается как на. Если флажок пуст, он рассматривается как.
ms.prod: xamarin
ms.assetid: B8B9268B-BCB8-42B9-B08C-C0F22C137238
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/11/2019
ms.openlocfilehash: 42631b1b67dc1d342e9f8666916604e68ee158d8
ms.sourcegitcommit: 0fd04ea3af7d6a6d6086525306523a5296eec0df
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/02/2019
ms.locfileid: "67517923"
---
# <a name="xamarinforms-checkbox"></a>Флажок Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/CheckBoxDemos)

Xamarin.Forms `CheckBox` — это тип кнопки, которая может быть установлен или пустой. Если флажок установлен, он рассматривается как на. Если флажок пуст, он рассматривается как.

`CheckBox` Определяет `bool` свойство с именем `IsChecked`, которое указывает ли `CheckBox` проверяется. Это свойство также поддерживаемый [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) объект, который означает, что его можно применить различные стили и быть целевым объектом привязки данных.

> [!NOTE]
> `IsChecked` Режим привязки по умолчанию имеет свойство, используемое [ `BindingMode.TwoWay` ](xref:Xamarin.Forms.BindingMode.TwoWay).

`CheckBox` Определяет `CheckedChanged` событие, которое возникло при `IsChecked` изменения свойств, либо через операции пользователя, или когда приложение задает `IsChecked` свойства. `CheckedChangedEventArgs` Объект, который прилагается к `CheckedChanged` событие имеет одно свойство с именем `Value`, типа `bool`. При возникновении этого события значение `Value` свойству присваивается новое значение `IsChecked` свойства.

## <a name="create-a-checkbox"></a>Создание флажка

В следующем примере показано, как создать экземпляр `CheckBox` в XAML:

```xaml
<CheckBox />
```

Этот XAML приводит к появлению, показано на следующем снимке экрана:

![Снимок экрана пустой флажок, в iOS и Android](checkbox-images/checkbox-empty.png "пустой флажок")

По умолчанию `CheckBox` пуст. `CheckBox` Можно проверить путем управления пользователя, или установив `IsChecked` свойства `true`:

```xaml
<CheckBox IsChecked="true" />
```

Этот XAML приводит к появлению, показано на следующем снимке экрана:

![Снимок экрана установленный флажок, в iOS и Android](checkbox-images/checkbox-checked.png "установлен флажок")

Кроме того `CheckBox` могут создаваться в коде:

```csharp
CheckBox checkBox = new CheckBox { IsChecked = true };
```

## <a name="respond-to-a-checkbox-changing-state"></a>Ответ на изменение состояния флажка

Когда `IsChecked` изменения свойств, либо через операции пользователя, или когда приложение задает `IsChecked` свойства `CheckedChanged` вызывает событие. Обработчик событий для этого события могут быть зарегистрированы в ответ на изменение:

```xaml
<CheckBox CheckedChanged="OnCheckBoxCheckedChanged" />
```

Файл кода содержит обработчик `CheckedChanged` событий:

```csharp
void OnCheckBoxCheckedChanged(object sender, CheckedChangedEventArgs e)
{
    // Perform required operation after examining e.Value
}
```

`sender` Аргумент `CheckBox` инициатором этого события. Это можно использовать для доступа к `CheckBox` объекта, или чтобы различать несколько `CheckBox` объектов с одинаковым `CheckedChanged` событий.

Кроме того, обработчик событий для `CheckedChanged` событие может быть зарегистрировано в коде:

```csharp
CheckBox checkBox = new CheckBox { ... };
checkBox.CheckedChanged += (sender, e) =>
{
    // Perform required operation after examining e.Value
};
```

## <a name="data-bind-a-checkbox"></a>Привязка данных флажок

`CheckedChanged` Обработчик событий, которые можно устранить с помощью привязки данных и триггеров реагировать на `CheckBox` , помечен или пустой:

```xaml
<CheckBox x:Name="checkBox" />
<Label Text="Lorem ipsum dolor sit amet, elit rutrum, enim hendrerit augue vitae praesent sed non, lorem aenean quis praesent pede.">
    <Label.Triggers>
        <DataTrigger TargetType="Label"
                     Binding="{Binding Source={x:Reference checkBox}, Path=IsChecked}"
                     Value="true">
            <Setter Property="FontAttributes"
                    Value="Italic, Bold" />
            <Setter Property="FontSize"
                    Value="Large" />
        </DataTrigger>
    </Label.Triggers>
</Label>
```

В этом примере [ `Label` ](xref:Xamarin.Forms.Label) использует выражение привязки в триггер для отслеживания `IsChecked` свойство `CheckBox`. Когда это свойство становится `true`, `FontAttributes` и `FontSize` свойства `Label` изменить. Когда `IsChecked` свойство возвращается `false`, `FontAttributes` и `FontSize` свойства `Label` сбрасываются в изначальное состояние.

На снимках экрана ниже, показано на снимке экрана iOS [ `Label` ](xref:Xamarin.Forms.Label) форматирование при `CheckBox` пуст, хотя на Android снимке экрана показаны `Label` форматирование при `CheckBox` проверяется:

[![Снимок экрана данных привязано флажок, iOS и Android](checkbox-images/checkbox-databinding.png "флажок с привязкой к данным")](checkbox-images/checkbox-databinding-large.png#lightbox "флажок с привязкой к данным")

Дополнительные сведения о триггерах см. в разделе [Xamarin.Forms триггеры](~/xamarin-forms/app-fundamentals/triggers.md).

## <a name="disable-a-checkbox"></a>Снимите флажок

Иногда приложение переходит в состояние где `CheckBox` проверяемой в данный момент не является допустимой операцией. В таких случаях `CheckBox` можно отключить, задав его `IsEnabled` свойства `false`.

## <a name="checkbox-appearance"></a>Внешний вид флажка

Помимо свойств, `CheckBox` наследует от [ `View` ](xref:Xamarin.Forms.View) класс, `CheckBox` также определяет `Color` свойство, которое задает ее цвет [ `Color` ](xref:Xamarin.Forms.Color):

```xaml
<CheckBox Color="Red" />
```

На следующих снимках экрана показано ряд checked `CheckBox` объектов, где каждый объект имеет его `Color` свойство присвоено другой [ `Color` ](xref:Xamarin.Forms.Color):

![Снимок экрана цветной флажки, в iOS и Android](checkbox-images/checkbox-colors.png "цветной флажок")

## <a name="checkbox-visual-states"></a>Визуальные состояния CheckBox

`CheckBox` имеет `IsChecked` [ `VisualState` ](xref:Xamarin.Forms.VisualState) , можно использовать для запуска Визуальное изменение в `CheckBox` когда становится отмеченным.

В следующем примере XAML показано, как определить визуальное состояние для `IsChecked` состояния:

```xaml
<CheckBox ...>
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="Normal">
                <VisualState.Setters>
                    <Setter Property="Color"
                            Value="Red" />
                </VisualState.Setters>
            </VisualState>

            <VisualState x:Name="IsChecked">
                <VisualState.Setters>
                    <Setter Property="Color"
                            Value="Green" />
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</CheckBox>
```

В этом примере `IsChecked` [ `VisualState` ](xref:Xamarin.Forms.VisualState) указывает, что при `CheckBox` установлен, его `Color` будет установлено на зеленый. `Normal` `VisualState` Указывает, что при `CheckBox` находится в обычном состоянии, его `Color` будет установлено на красный. Таким образом, общий эффект является `CheckBox` отображается красным цветом, если это пустой и зеленого при его выборе.

Дополнительные сведения о визуальных состояниях см. в разделе [Диспетчер визуальных состояний Xamarin.Forms](~/xamarin-forms/user-interface/visual-state-manager.md).

## <a name="related-links"></a>Связанные ссылки

- [Флажок демонстрации (пример)](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/CheckBoxDemos)
- [Триггеры Xamarin.Forms](~/xamarin-forms/app-fundamentals/triggers.md)
- [Диспетчер визуальных состояний Xamarin.Forms](~/xamarin-forms/user-interface/visual-state-manager.md)
