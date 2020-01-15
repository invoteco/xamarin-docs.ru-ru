---
title: Триггеры Xamarin.Forms
description: В этой статье содержатся сведения об использовании триггеров Xamarin.Forms для реагирования на изменения пользовательского интерфейса с помощью XAML. Триггеры позволяют декларативно задавать действия в XAML, которые изменяют внешний вид элементов управления при изменении событий или свойств.
ms.prod: xamarin
ms.assetid: 60460F57-63C6-4916-BBB5-A870F1DF53D7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/01/2016
ms.openlocfilehash: 056bb16c76887661f054422b2c682a91e6bfa466
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/25/2019
ms.locfileid: "75489899"
---
# <a name="xamarinforms-triggers"></a>Триггеры Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithtriggers)

Триггеры позволяют декларативно задавать действия в XAML, которые изменяют внешний вид элементов управления при изменении событий или свойств.

Триггер можно напрямую назначить элементу управления или добавить его в словарь ресурсов уровня страницы или приложения для применения к нескольким элементам управления.

Существует четыре типа триггеров.

- [Триггер свойства](#property) — срабатывает, если свойству элемента управления задается определенное значение.

- [Триггер данных](#data) — использует привязку данных к триггеру на основе свойств другого элемента управления.

- [Триггер события](#event) — срабатывает при возникновении события в элементе управления.

- [Мультитриггер](#multi) — позволяет задавать несколько условий, которые должны выполняться перед возникновением действия.

<a name="property" />

## <a name="property-triggers"></a>Триггеры свойств

Простой триггер может быть выражен чисто в XAML — в коллекцию триггеров элемента управления добавляется элемент `Trigger`.
В этом примере показан триггер, который изменяет цвет фона элемента управления `Entry` при получении им фокуса:

```xaml
<Entry Placeholder="enter name">
    <Entry.Triggers>
        <Trigger TargetType="Entry"
                 Property="IsFocused" Value="True">
            <Setter Property="BackgroundColor" Value="Yellow" />
            <!-- multiple Setters elements are allowed -->
        </Trigger>
    </Entry.Triggers>
</Entry>
```

Ниже приводятся важные части объявления триггера.

- **TargetType** — тип элемента управления, к которому применяется триггер.

- **Property** — отслеживаемое свойство элемента управления.

- **Value** — значение отслеживаемого свойства, при котором срабатывает триггер.

- **Setter** — возможность добавления коллекции элементов `Setter` при выполнении условия триггера. Необходимо указать `Property` и `Value`.

- **EnterActions и ExitActions** (не показаны) пишутся в коде и могут использоваться наряду с элементами `Setter` (или вместо них). Они [описаны ниже](#enterexit).

### <a name="applying-a-trigger-using-a-style"></a>Применение триггера с помощью стиля

Триггеры также можно добавлять в объявление `Style` в элементе управления в `ResourceDictionary` на странице или в приложении. В этом примере объявляется неявный стиль (т. е. `Key` не задан). Это значит, что он будет применяться ко всем элементам управления `Entry` на странице.

```xaml
<ContentPage.Resources>
    <ResourceDictionary>
        <Style TargetType="Entry">
                        <Style.Triggers>
                <Trigger TargetType="Entry"
                         Property="IsFocused" Value="True">
                    <Setter Property="BackgroundColor" Value="Yellow" />
                    <!-- multiple Setters elements are allowed -->
                </Trigger>
            </Style.Triggers>
        </Style>
    </ResourceDictionary>
</ContentPage.Resources>
```

<a name="data" />

## <a name="data-triggers"></a>Триггеры данных

Триггеры данных используют привязку данных для отслеживания другого элемента управления в целях вызова `Setter`. Для отслеживания указанного значения задайте в триггере свойства атрибут `Binding` вместо атрибута `Property`.

В примере ниже используется синтаксис привязки данных `{Binding Source={x:Reference entry}, Path=Text.Length}`,
который показывает, как осуществляется ссылка на свойства другого элемента управления. Если длина `entry` равна нулю, сработает триггер. В этом примере триггер отключает кнопку, если входные данные отсутствуют.

```xaml
<!-- the x:Name is referenced below in DataTrigger-->
<!-- tip: make sure to set the Text="" (or some other default) -->
<Entry x:Name="entry"
       Text=""
       Placeholder="required field" />

<Button x:Name="button" Text="Save"
        FontSize="Large"
        HorizontalOptions="Center">
    <Button.Triggers>
        <DataTrigger TargetType="Button"
                     Binding="{Binding Source={x:Reference entry},
                                       Path=Text.Length}"
                     Value="0">
            <Setter Property="IsEnabled" Value="False" />
            <!-- multiple Setters elements are allowed -->
        </DataTrigger>
    </Button.Triggers>
</Button>
```

> [!TIP]
> При определении `Path=Text.Length` всегда следует указывать значение по умолчанию для целевого свойства (например, `Text=""`), так как в противном случае будет использоваться значение `null` и триггер не будет работать надлежащим образом.

Помимо указания `Setter`, можно задать коллекции [`EnterActions` и `ExitActions`](#enterexit).

<a name="event" />

## <a name="event-triggers"></a>Триггеры событий

Элементу `EventTrigger` требуется только свойство `Event`, такое как `"Clicked"` в примере ниже.

```xaml
<EventTrigger Event="Clicked">
    <local:NumericValidationTriggerAction />
</EventTrigger>
```

Обратите внимание, что здесь нет элементов `Setter`, а есть ссылка на класс, определяемый элементом `local:NumericValidationTriggerAction`, для которого требуется объявить пространство имен `xmlns:local` в XAML страницы.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:WorkingWithTriggers;assembly=WorkingWithTriggers"
```

Сам класс реализует класс `TriggerAction`, то есть он должен предоставлять переопределение для метода `Invoke`, вызываемого при возникновении события триггера.

В ходе реализации действия триггера должны выполняться следующие задачи.

- Реализация универсального класса `TriggerAction<T>` с универсальным параметром, соответствующим типу элемента управления, к которому будет применяться триггер. Можно использовать суперклассы, например `VisualElement`, для записи действий триггера, которые работают с различными элементами управления, или указать тип элемента управления, например `Entry`.

- Переопределение метода `Invoke`, который вызывается при каждом выполнении условий триггера.

- Необязательное предоставление свойств, которые можно задать в XAML. Пример см. в описании `VisualElementPopTriggerAction` класса в прилагаемом примере приложения.

```csharp
public class NumericValidationTriggerAction : TriggerAction<Entry>
{
    protected override void Invoke (Entry entry)
    {
        double result;
        bool isValid = Double.TryParse (entry.Text, out result);
        entry.TextColor = isValid ? Color.Default : Color.Red;
    }
}
```

Затем триггер события можно использовать из XAML:

```xaml
<EventTrigger Event="TextChanged">
    <local:NumericValidationTriggerAction />
</EventTrigger>
```

Следует соблюдать осторожность при совместном использовании триггеров в классе `ResourceDictionary`: один экземпляр будет общим для элементов управления, поэтому любое однократно настроенное состояние будет применяться ко всем элементам.

Обратите внимание, что триггеры событий не поддерживают коллекции `EnterActions` и `ExitActions`, [описанные ниже](#enterexit).

<a name="multi" />

## <a name="multi-triggers"></a>Мультитриггеры

`MultiTrigger` аналогичен `Trigger` или `DataTrigger`, за исключением того, что для него может существовать несколько условий. `Setter` срабатывают при выполнении всех условий.

Ниже приведен пример триггера для кнопки, которая привязывается к двум различным записям (`email` и `phone`).

```xaml
<MultiTrigger TargetType="Button">
    <MultiTrigger.Conditions>
        <BindingCondition Binding="{Binding Source={x:Reference email},
                                   Path=Text.Length}"
                               Value="0" />
        <BindingCondition Binding="{Binding Source={x:Reference phone},
                                   Path=Text.Length}"
                               Value="0" />
    </MultiTrigger.Conditions>
    <Setter Property="IsEnabled" Value="False" />
    <!-- multiple Setter elements are allowed -->
</MultiTrigger>
```

Коллекция `Conditions` может также содержать элементы `PropertyCondition`, аналогичные приведенным ниже.

```xaml
<PropertyCondition Property="Text" Value="OK" />
```

### <a name="building-a-require-all-multi-trigger"></a>Создание мультитриггера, требующего выполнения всех условий

Мультитриггер обновляет свой элемент управления только в том случае, если выполняются все условия. Тестировать условие "Все значения длины поля равны нулю" (например, для страницы входа, где должны быть указаны все входные данные) достаточно сложно, поскольку требуется условие "где Text.Length > 0", которое невозможно выразить в XAML.

Это можно сделать с помощью интерфейса `IValueConverter`. Приведенный ниже код преобразует привязку `Text.Length` в значение типа `bool`, указывающее, является поле пустым или нет.

```csharp
public class MultiTriggerConverter : IValueConverter
{
    public object Convert(object value, Type targetType,
        object parameter, CultureInfo culture)
    {
        if ((int)value > 0) // length > 0 ?
            return true;            // some data has been entered
        else
            return false;            // input is empty
    }

    public object ConvertBack(object value, Type targetType,
        object parameter, CultureInfo culture)
    {
        throw new NotSupportedException ();
    }
}
```

Для использования этого конвертера в мультитриггере сначала добавьте его в словарь ресурсов страницы (вместе с пользовательским определением пространства имен `xmlns:local`).

```xaml
<ResourceDictionary>
   <local:MultiTriggerConverter x:Key="dataHasBeenEntered" />
</ResourceDictionary>
```

XAML приведен ниже. Обратите внимание на указанные далее отличия от первого примера мультитриггера.

- Для кнопки задан параметр `IsEnabled="false"` по умолчанию.
- Условия мультитриггера используют преобразователь для преобразования значения `Text.Length` в тип `boolean`.
- Если все условия имеют значение `true`, метод задания устанавливает для свойства `IsEnabled` кнопки значение `true`.

```xaml
<Entry x:Name="user" Text="" Placeholder="user name" />

<Entry x:Name="pwd" Text="" Placeholder="password" />

<Button x:Name="loginButton" Text="Login"
        FontSize="Large"
        HorizontalOptions="Center"
        IsEnabled="false">
  <Button.Triggers>
    <MultiTrigger TargetType="Button">
      <MultiTrigger.Conditions>
        <BindingCondition Binding="{Binding Source={x:Reference user},
                              Path=Text.Length,
                              Converter={StaticResource dataHasBeenEntered}}"
                          Value="true" />
        <BindingCondition Binding="{Binding Source={x:Reference pwd},
                              Path=Text.Length,
                              Converter={StaticResource dataHasBeenEntered}}"
                          Value="true" />
      </MultiTrigger.Conditions>
      <Setter Property="IsEnabled" Value="True" />
    </MultiTrigger>
  </Button.Triggers>
</Button>
```

На этих снимках экрана показаны различия между двумя приведенными выше примерами мультитриггеров. В верхней части экранов достаточно ввести текст только в один элемент управления `Entry`, чтобы включить кнопку **Сохранить**.
В нижней части экранов кнопка **Вход** остается неактивной до тех пор, пока не будут заполнены оба поля.

![](triggers-images/multi-requireall.png "MultiTrigger Examples")

<a name="enterexit" />

## <a name="enteractions-and-exitactions"></a>EnterActions и ExitActions

Другим способом реализации изменений при срабатывании триггера является добавление коллекций `EnterActions` и `ExitActions` и указание реализаций класса `TriggerAction<T>`.

Коллекция [`EnterActions`](xref:Xamarin.Forms.TriggerBase.EnterActions) служит для определения списка `IList` объектов [`TriggerAction`](xref:Xamarin.Forms.TriggerAction), которые будут вызываться при соблюдении условия триггера. Коллекция [`ExitActions`](xref:Xamarin.Forms.TriggerBase.ExitActions) служит для определения списка `IList` объектов `TriggerAction`, которые будут вызываться, когда условие триггера больше не соблюдается.

> [!NOTE]
> Объекты [`TriggerAction`](xref:Xamarin.Forms.TriggerAction), определенные в коллекциях `EnterActions` и `ExitActions`, игнорируются классом [`EventTrigger`](xref:Xamarin.Forms.EventTrigger).    

В триггер можно добавить *обе* коллекции `EnterActions` и `ExitActions`, а также `Setter`, но учтите, что `Setter` вызываются мгновенно (они не ожидают завершения `EnterAction` или `ExitAction`). Кроме того, можно выполнить все действия в коде и вообще не использовать `Setter`.

```xaml
<Entry Placeholder="enter job title">
    <Entry.Triggers>
        <Trigger TargetType="Entry"
                 Property="Entry.IsFocused" Value="True">
            <Trigger.EnterActions>
                <local:FadeTriggerAction StartsFrom="0"" />
            </Trigger.EnterActions>

            <Trigger.ExitActions>
                <local:FadeTriggerAction StartsFrom="1" />
            </Trigger.ExitActions>
            <!-- You can use both Enter/Exit and Setter together if required -->
        </Trigger>
    </Entry.Triggers>
</Entry>
```

Как обычно, если класс указывается в XAML, необходимо объявить пространство имен, например `xmlns:local`, следующим образом.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:WorkingWithTriggers;assembly=WorkingWithTriggers"
```

Ниже приведен код для класса `FadeTriggerAction`.

```csharp
public class FadeTriggerAction : TriggerAction<VisualElement>
{
    public int StartsFrom { set; get; }

    protected override void Invoke(VisualElement sender)
    {
        sender.Animate("FadeTriggerAction", new Animation((d) =>
        {
            var val = StartsFrom == 1 ? d : 1 - d;
            // so i was aiming for a different color, but then i liked the pink :)
            sender.BackgroundColor = Color.FromRgb(1, val, 1);
        }),
        length: 1000, // milliseconds
        easing: Easing.Linear);
    }
}
```

## <a name="related-links"></a>Связанные ссылки

- [Пример триггеров](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithtriggers)
- [Документация по API Xamarin.Forms](xref:Xamarin.Forms.TriggerAction`1)
