---
title: Флажок Xamarin. Forms
description: Флажок Xamarin. Forms является типом кнопки, которая может быть либо установлена, либо пустой. Если флажок установлен, он считается включенным. Если флажок пуст, он считается отключенным.
ms.prod: xamarin
ms.assetid: B8B9268B-BCB8-42B9-B08C-C0F22C137238
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/11/2019
ms.openlocfilehash: f78ca9d2cf7a9e57b81c5d923c64b36a7982c4b0
ms.sourcegitcommit: 699de58432b7da300ddc2c85842e5d9e129b0dc5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/25/2019
ms.locfileid: "68739145"
---
# <a name="xamarinforms-checkbox"></a>Флажок Xamarin. Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-checkboxdemos/)

Xamarin. Forms `CheckBox` — это тип кнопки, которая может быть либо установлена, либо пустой. Если флажок установлен, он считается включенным. Если флажок пуст, он считается отключенным.

`CheckBox`Определяет свойство с `IsChecked` именем`CheckBox` , которое указывает, установлен ли флажок. `bool` Это свойство также [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) поддерживается объектом, то есть его можно присвоить стилю и быть целевым объектом привязок данных.

> [!NOTE]
> Свойство BIND имеет режим привязки по умолчанию [`BindingMode.TwoWay`.](xref:Xamarin.Forms.BindingMode.TwoWay) `IsChecked`

`CheckBox`Определяет событие, которое возникает `IsChecked` при изменении свойства с помощью манипуляции пользователя или `IsChecked` когда приложение задает свойство. `CheckedChanged` Объект, `Value`сопровождающий событие, имеет одно свойство с именем типа `bool`. `CheckedChanged` `CheckedChangedEventArgs` При срабатывании события в качестве значения `Value` свойства задается новое значение `IsChecked` свойства.

## <a name="create-a-checkbox"></a>Создание флажка

В следующем примере показано, как создать экземпляр `CheckBox` в XAML:

```xaml
<CheckBox />
```

Этот XAML приводит к отображению на следующих снимках экрана:

![Снимок экрана пустого флажка в iOS и Android](checkbox-images/checkbox-empty.png "Пустой флажок")

По умолчанию `CheckBox` параметр пуст. Можно проверить с помощью манипуляции с пользователем или `IsChecked` задав для `true`свойства значение: `CheckBox`

```xaml
<CheckBox IsChecked="true" />
```

Этот XAML приводит к отображению на следующих снимках экрана:

![Снимок экрана флажка "отмечено" в iOS и Android](checkbox-images/checkbox-checked.png "Флажок") флажка

Кроме того, `CheckBox` можно создать в коде:

```csharp
CheckBox checkBox = new CheckBox { IsChecked = true };
```

## <a name="respond-to-a-checkbox-changing-state"></a>Реагирование на изменение состояния флажка

Когда свойство изменяется либо с помощью пользовательской манипуляции, либо когда приложение `IsChecked` задает свойство, `CheckedChanged` возникает событие. `IsChecked` Для реагирования на изменение можно зарегистрировать обработчик событий для этого события:

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

Кроме того, обработчик событий для `CheckedChanged` события можно зарегистрировать в коде:

```csharp
CheckBox checkBox = new CheckBox { ... };
checkBox.CheckedChanged += (sender, e) =>
{
    // Perform required operation after examining e.Value
};
```

## <a name="data-bind-a-checkbox"></a>Флажок привязки данных

Обработчик событий можно исключить с помощью привязки данных и триггеров, чтобы реагировать `CheckBox` на проверяемый или пустой: `CheckedChanged`

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

В этом примере [`Label`](xref:Xamarin.Forms.Label) компонент использует выражение привязки в триггере данных для `IsChecked` отслеживания свойства объекта `CheckBox`. Когда это свойство примет `true`значение `FontAttributes` , свойства `FontSize` и для `Label` изменения. `FontAttributes` `FontSize` Когда свойство возвращает значение `false`, свойства и объекта`Label` сбрасываются в исходное состояние. `IsChecked`

На приведенных ниже снимках экрана в iOS отображается [`Label`](xref:Xamarin.Forms.Label) форматирование, `CheckBox` когда пусто, а `Label` на снимке экрана Android `CheckBox` отображается форматирование при проверке.

[![Снимок экрана: флажок привязки данных в iOS и Android](checkbox-images/checkbox-databinding.png "Флажок привязки данных")](checkbox-images/checkbox-databinding-large.png#lightbox "Флажок привязки данных")

Дополнительные сведения о триггерах см. в разделе [триггеры Xamarin. Forms](~/xamarin-forms/app-fundamentals/triggers.md).

## <a name="disable-a-checkbox"></a>Отключение флажка

Иногда приложение переходит в состояние, в котором `CheckBox` проверяемое значение не является допустимой операцией. В таких случаях `CheckBox` можно отключить, `IsEnabled` задав свойству `false`значение.

## <a name="checkbox-appearance"></a>Вид флажка

В дополнение к свойствам, `CheckBox` [`View`](xref:Xamarin.Forms.View) унаследованным от `Color` класса, `CheckBox` также определяет свойство, которое задает для его цвета значение [`Color`](xref:Xamarin.Forms.Color).

```xaml
<CheckBox Color="Red" />
```

На следующих снимках экрана показан ряд проверенных `CheckBox` объектов, для каждого `Color` из которых задано другое [`Color`](xref:Xamarin.Forms.Color)свойство:

![Снимок экрана с цветными флажками в iOS и Android](checkbox-images/checkbox-colors.png "Цветовой флажок")

## <a name="checkbox-visual-states"></a>Визуальные состояния флажка

`CheckBox`имеет объект `IsChecked` [`VisualState`](xref:Xamarin.Forms.VisualState) , который можно использовать для инициации визуального изменения `CheckBox` в при проверке.

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

`IsChecked` В этом примере параметр [`VisualState`](xref:Xamarin.Forms.VisualState) указывает, что если `CheckBox` флажок установлен, его `Color` свойство будет иметь значение Green. Указывает, `Color` что когда `CheckBox` находится в нормальном состоянии, его свойство будет установлено в значение Red. `Normal` `VisualState` Таким образом, общий результат заключается в `CheckBox` том, что красный, когда он пуст, и зеленый при проверке.

Дополнительные сведения о визуальных состояниях см. в разделе [Диспетчер визуальных состояний Xamarin. Forms](~/xamarin-forms/user-interface/visual-state-manager.md).

## <a name="related-links"></a>Связанные ссылки

- [Демонстрации CheckBox (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-checkboxdemos/)
- [Триггеры Xamarin. Forms](~/xamarin-forms/app-fundamentals/triggers.md)
- [Диспетчер визуальных состояний Xamarin. Forms](~/xamarin-forms/user-interface/visual-state-manager.md)
