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
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/21/2019
ms.locfileid: "68739145"
---
# <a name="xamarinforms-checkbox"></a>Флажок Xamarin. Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-checkboxdemos/)

@No__t_0 Xamarin. Forms является типом кнопки, которая может быть либо установлена, либо пустой. Если флажок установлен, он считается включенным. Если флажок пуст, он считается отключенным.

`CheckBox` определяет свойство `bool` с именем `IsChecked`, которое указывает, установлен ли `CheckBox`. Это свойство также поддерживается объектом [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) , что означает, что его можно присвоить стилю и быть целевым объектом привязок данных.

> [!NOTE]
> Свойство `IsChecked` BIND имеет режим привязки по умолчанию [`BindingMode.TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay).

`CheckBox` определяет событие `CheckedChanged`, которое возникает при изменении свойства `IsChecked` либо с помощью манипуляции пользователя, либо когда приложение задает свойство `IsChecked`. Объект `CheckedChangedEventArgs`, сопровождающий событие `CheckedChanged`, имеет одно свойство с именем `Value` типа `bool`. При срабатывании события в качестве значения свойства `Value` устанавливается новое значение свойства `IsChecked`.

## <a name="create-a-checkbox"></a>Создание флажка

В следующем примере показано, как создать экземпляр `CheckBox` в XAML:

```xaml
<CheckBox />
```

Этот XAML приводит к отображению на следующих снимках экрана:

![Снимок экрана пустого флажка в iOS и Android](checkbox-images/checkbox-empty.png "Пустой флажок")

По умолчанию `CheckBox` пуст. @No__t_0 можно проверить с помощью манипуляции с пользователем или присвоив свойству `IsChecked` значение `true`.

```xaml
<CheckBox IsChecked="true" />
```

Этот XAML приводит к отображению на следующих снимках экрана:

![Снимок экрана флажка "отмечено" в iOS и Android](checkbox-images/checkbox-checked.png "Флажок флажка")

В качестве альтернативы можно создать `CheckBox` в коде:

```csharp
CheckBox checkBox = new CheckBox { IsChecked = true };
```

## <a name="respond-to-a-checkbox-changing-state"></a>Реагирование на изменение состояния флажка

При изменении свойства `IsChecked` либо с помощью пользовательской манипуляции, либо когда приложение задает свойство `IsChecked`, срабатывает событие `CheckedChanged`. Для реагирования на изменение можно зарегистрировать обработчик событий для этого события:

```xaml
<CheckBox CheckedChanged="OnCheckBoxCheckedChanged" />
```

Файл кода программной части содержит обработчик события `CheckedChanged`:

```csharp
void OnCheckBoxCheckedChanged(object sender, CheckedChangedEventArgs e)
{
    // Perform required operation after examining e.Value
}
```

Аргумент `sender` является `CheckBox`, ответственным за это событие. Его можно использовать для доступа к объекту `CheckBox` или для различения нескольких объектов `CheckBox`, совместно использующих одно и то же событие `CheckedChanged`.

Кроме того, обработчик событий для события `CheckedChanged` может быть зарегистрирован в коде:

```csharp
CheckBox checkBox = new CheckBox { ... };
checkBox.CheckedChanged += (sender, e) =>
{
    // Perform required operation after examining e.Value
};
```

## <a name="data-bind-a-checkbox"></a>Флажок привязки данных

Обработчик событий `CheckedChanged` можно исключить с помощью привязки данных и триггеров, чтобы отреагировать на проверяемое или пустое `CheckBox`:

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

В этом примере [`Label`](xref:Xamarin.Forms.Label) использует выражение привязки в триггере данных для отслеживания свойства `IsChecked` `CheckBox`. Когда это свойство переходит `true`, свойства `FontAttributes` и `FontSize` `Label` изменяются. Когда свойство `IsChecked` возвращает значение в `false`, свойства `FontAttributes` и `FontSize` `Label` сбрасываются в исходное состояние.

На следующих снимках экрана в iOS отображается [`Label`](xref:Xamarin.Forms.Label) форматирование, если `CheckBox` пусто, а на снимке экрана Android отображается `Label` форматирование при проверке `CheckBox`:

[![Снимок экрана: флажок привязки данных в iOS и Android](checkbox-images/checkbox-databinding.png "Флажок привязки данных")](checkbox-images/checkbox-databinding-large.png#lightbox "Флажок привязки данных")

Дополнительные сведения о триггерах см. в разделе [триггеры Xamarin. Forms](~/xamarin-forms/app-fundamentals/triggers.md).

## <a name="disable-a-checkbox"></a>Отключение флажка

Иногда приложение переходит в состояние, в котором проверяемый `CheckBox` не является допустимой операцией. В таких случаях `CheckBox` можно отключить, задав свойству `IsEnabled` значение `false`.

## <a name="checkbox-appearance"></a>Вид флажка

В дополнение к свойствам, которые `CheckBox` наследуют от класса [`View`](xref:Xamarin.Forms.View) , `CheckBox` также определяет свойство `Color`, которое задает для его цвета значение [`Color`](xref:Xamarin.Forms.Color).

```xaml
<CheckBox Color="Red" />
```

На следующих снимках экрана показан ряд проверенных `CheckBox` объектов, для каждого из которых в свойстве `Color` задано другое [`Color`](xref:Xamarin.Forms.Color):

![Снимок экрана с цветными флажками в iOS и Android](checkbox-images/checkbox-colors.png "Цветовой флажок")

## <a name="checkbox-visual-states"></a>Визуальные состояния флажка

`CheckBox` имеет `IsChecked` [`VisualState`](xref:Xamarin.Forms.VisualState) , который можно использовать для запуска визуального изменения `CheckBox`, когда оно станет отмеченным.

В следующем примере XAML показано, как определить визуальное состояние для состояния `IsChecked`.

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

В этом примере `IsChecked` [`VisualState`](xref:Xamarin.Forms.VisualState) указывает, что при проверке `CheckBox` его свойство `Color` будет иметь значение Green. @No__t_0 `VisualState` указывает, что если `CheckBox` находится в нормальном состоянии, его свойство `Color` будет установлено в значение Red. Таким образом, общий результат заключается в том, что `CheckBox` красный, когда он пуст, и зеленый при проверке.

Дополнительные сведения о визуальных состояниях см. в разделе [Диспетчер визуальных состояний Xamarin. Forms](~/xamarin-forms/user-interface/visual-state-manager.md).

## <a name="related-links"></a>Связанные ссылки

- [Демонстрации CheckBox (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-checkboxdemos/)
- [Триггеры Xamarin. Forms](~/xamarin-forms/app-fundamentals/triggers.md)
- [Диспетчер визуальных состояний Xamarin. Forms](~/xamarin-forms/user-interface/visual-state-manager.md)
