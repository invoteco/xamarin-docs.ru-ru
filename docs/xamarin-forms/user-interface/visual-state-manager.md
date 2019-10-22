---
title: Диспетчер визуальных состояний Xamarin. Forms
description: Используйте Диспетчер визуальных состояний для внесения изменений в элементы XAML на основе визуальных состояний, заданных из кода.
ms.prod: xamarin
ms.assetid: 17296F14-640D-484B-A24C-A4E9B7013E4F
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 05/07/2018
ms.openlocfilehash: 228501172ede71204c64e1efe1673ce92be424ea
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/21/2019
ms.locfileid: "68656059"
---
# <a name="the-xamarinforms-visual-state-manager"></a>Диспетчер визуальных состояний Xamarin. Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)

_Используйте Диспетчер визуальных состояний для внесения изменений в элементы XAML на основе визуальных состояний, заданных из кода._

Диспетчер визуальных состояний (VSM) является новым в Xamarin. Forms 3,0. VSM предоставляет структурированный способ внесения визуальных изменений в пользовательский интерфейс из кода. В большинстве случаев пользовательский интерфейс приложения определен в XAML, а этот XAML включает в себя разметку, описывающую влияние диспетчера визуального состояния на визуальные элементы пользовательского интерфейса.

В VSM введена концепция _визуальных состояний_. Представление Xamarin. Forms, например `Button`, может иметь несколько различных визуальных представлений в зависимости от его базового состояния &mdash;, отключено ли оно или нажато или имеет фокус ввода. Это состояния кнопки.

Визуальные состояния собираются в _группах визуальных состояний_. Все визуальные состояния в пределах визуальной группы состояний являются взаимоисключающими. Визуальные состояния и группы визуальных состояний идентифицируются простыми текстовыми строками.

Диспетчер визуальных состояний Xamarin. Forms определяет одну группу визуальных состояний с именем "Коммонстатес" с тремя визуальными состояниями:

- "Normal"
- Доступ
- Указанной

Эта группа визуальных состояний поддерживается для всех классов, производных от [`VisualElement`](xref:Xamarin.Forms.VisualElement), которые являются базовым классом для [`View`](xref:Xamarin.Forms.View) и [`Page`](xref:Xamarin.Forms.Page). 

Можно также определить собственные визуальные группы состояний и визуальные состояния, как показано в этой статье.

> [!NOTE]
> Разработчики Xamarin. Forms, знакомые с [триггерами](~/xamarin-forms/app-fundamentals/triggers.md) , знают, что триггеры также могут вносить изменения в визуальные элементы в пользовательском интерфейсе на основе изменений в свойствах представления или при срабатывании событий. Однако использование триггеров для работы с различными сочетаниями этих изменений может стать весьма запутанным. Исторически, диспетчер визуального состояния появился в средах на основе XAML в Windows, чтобы сократить путаницу, полученную из-за сочетаний визуальных состояний. В VSM визуальные состояния в пределах визуальной группы состояний всегда являются взаимоисключающими. В любой момент только одно состояние в каждой группе — Текущее состояние.

## <a name="the-common-states"></a>Общие состояния

Диспетчер визуальных состояний позволяет включить разделы в файл XAML, который может изменить внешний вид представления, если оно является нормальным или отключенным, или имеет фокус ввода. Они называются _распространенными состояниями_.

Например, предположим, что на странице имеется `Entry`ное представление, и необходимо изменить внешний вид `Entry` следующим образом:

- Если `Entry` отключен, `Entry` должен иметь розовый фон.
- @No__t_0 должен иметь травяной фон в обычном режиме.
- При наличии фокуса ввода `Entry` должен расширяться в два раза больше его стандартной высоты.

Разметку VSM можно прикрепить к отдельному представлению или можно определить в стиле, если она применяется к нескольким представлениям. Эти подходы описаны в следующих двух разделах.

### <a name="vsm-markup-on-a-view"></a>Разметка VSM в представлении

Чтобы присоединить разметку VSM к `Entry`ному представлению, сначала разделите `Entry` в открывающие и закрывающие теги:

```xaml
<Entry FontSize="18">

</Entry>
```

Он имеет явный размер шрифта, так как одно из состояний будет использовать свойство `FontSize`, чтобы удвоить размер текста в `Entry`.

Затем вставьте `VisualStateManager.VisualStateGroups` теги между этими тегами:

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>

    </VisualStateManager.VisualStateGroups>
</Entry>
```

[`VisualStateGroups`](xref:Xamarin.Forms.VisualStateManager.VisualStateGroupsProperty) является присоединенным свойством привязки, определенным классом [`VisualStateManager`](xref:Xamarin.Forms.VisualStateManager) . (Дополнительные сведения о присоединенных свойствах привязки см. в статье [присоединенные свойства](~/xamarin-forms/xaml/attached-properties.md).) Таким способом к объекту `Entry` прикрепляется свойство `VisualStateGroups`.

Свойство `VisualStateGroups` имеет тип [`VisualStateGroupList`](xref:Xamarin.Forms.VisualStateGroupList), который представляет собой коллекцию объектов [`VisualStateGroup`](xref:Xamarin.Forms.VisualStateGroup) . В тегах `VisualStateManager.VisualStateGroups` вставьте пару `VisualStateGroup` тегов для каждой группы визуальных состояний, которые вы хотите включить:

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">

        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Entry>
```

Обратите внимание, что тег `VisualStateGroup` имеет атрибут `x:Name`, указывающий имя группы. Класс `VisualStateGroup` определяет свойство `Name`, которое можно использовать вместо него:

```xaml
<VisualStateGroup Name="CommonStates">
```

Можно использовать либо `x:Name`, либо `Name`, но не оба одновременно в одном элементе.

Класс `VisualStateGroup` определяет свойство с именем [`States`](xref:Xamarin.Forms.VisualStateGroup.States), которое представляет собой коллекцию объектов [`VisualState`](xref:Xamarin.Forms.VisualState) . `States` является _свойством содержимого_ `VisualStateGroups`, поэтому теги `VisualState` можно включать непосредственно между тегами `VisualStateGroup`. (Свойства содержимого обсуждаются в статье [важный синтаксис XAML](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md#content-properties).)

Следующим шагом является включение пары тегов для каждого визуального состояния в этой группе. Их также можно определить с помощью `x:Name` или `Name`.

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="Normal">

            </VisualState>

            <VisualState x:Name="Focused">

            </VisualState>

            <VisualState x:Name="Disabled">

            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Entry>
```

`VisualState` определяет свойство с именем [`Setters`](xref:Xamarin.Forms.VisualState.Setters), которое является коллекцией объектов [`Setter`](xref:Xamarin.Forms.Setter) . Это те же `Setter` объекты, которые используются в объекте [`Style`](xref:Xamarin.Forms.Style) .

`Setters` _не_ является свойством Content `VisualState`, поэтому необходимо включить теги элементов свойств для свойства `Setters`:

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="Normal">
                <VisualState.Setters>

                </VisualState.Setters>
            </VisualState>

            <VisualState x:Name="Focused">
                <VisualState.Setters>
    
                </VisualState.Setters>
            </VisualState>

            <VisualState x:Name="Disabled">
                <VisualState.Setters>

                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Entry>
```

Теперь можно вставить один или несколько объектов `Setter` между каждой парой тегов `Setters`. Это `Setter` объекты, определяющие визуальные состояния, описанные выше:

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="Normal">
                <VisualState.Setters>
                    <Setter Property="BackgroundColor" Value="Lime" />
                </VisualState.Setters>
            </VisualState>

            <VisualState x:Name="Focused">
                <VisualState.Setters>
                    <Setter Property="FontSize" Value="36" />
                </VisualState.Setters>
            </VisualState>

            <VisualState x:Name="Disabled">
                <VisualState.Setters>
                    <Setter Property="BackgroundColor" Value="Pink" />
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Entry>
```

Каждый тег `Setter` указывает значение конкретного свойства, когда это состояние является текущим. Любое свойство, на которое ссылается объект `Setter`, должно поддерживаться связываемым свойством.

Разметка, аналогичная этой, является основанием страницы **VSM on View** в образце программы **[всмдемос](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)** . Страница содержит три представления `Entry`, но к ней присоединена разметка VSM только во второй.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:VsmDemos"
             x:Class="VsmDemos.MainPage"
             Title="VSM Demos">

    <StackLayout>
        <StackLayout.Resources>
            <Style TargetType="Entry">
                <Setter Property="Margin" Value="20, 0" />
                <Setter Property="FontSize" Value="18" />
            </Style>

            <Style TargetType="Label">
                <Setter Property="Margin" Value="20, 30, 20, 0" />
                <Setter Property="FontSize" Value="Large" />
            </Style>
        </StackLayout.Resources>

        <Label Text="Normal Entry:" />

        <Entry />

        <Label Text="Entry with VSM: " />

        <Entry>
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup x:Name="CommonStates">
                    
                    <VisualState x:Name="Normal">
                        <VisualState.Setters>
                            <Setter Property="BackgroundColor" Value="Lime" />
                        </VisualState.Setters>
                    </VisualState>

                    <VisualState x:Name="Focused">
                        <VisualState.Setters>
                            <Setter Property="FontSize" Value="36" />
                        </VisualState.Setters>
                    </VisualState>

                    <VisualState x:Name="Disabled">
                        <VisualState.Setters>
                            <Setter Property="BackgroundColor" Value="Pink" />
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>

            <Entry.Triggers>
                <DataTrigger TargetType="Entry"
                             Binding="{Binding Source={x:Reference entry3},
                                               Path=Text.Length}"
                             Value="0">
                    <Setter Property="IsEnabled" Value="False" />
                </DataTrigger>
            </Entry.Triggers>
        </Entry>

        <Label Text="Entry to enable 2nd Entry:" />

        <Entry x:Name="entry3"
               Text=""
               Placeholder="Type something to enable 2nd Entry" />
    </StackLayout>
</ContentPage>
```

Обратите внимание, что во втором `Entry` также имеется `DataTrigger` в составе `Trigger` коллекции. Это приводит к отключению `Entry`, пока что что-то не будет введено в третий `Entry`. Ниже приведена страница при запуске в iOS, Android и универсальная платформа Windows (UWP):

[![VSM в представлении: отключено](vsm-images/VsmOnViewDisabled.png "VSM при просмотре — отключено")](vsm-images/VsmOnViewDisabled-Large.png#lightbox)

Текущее визуальное состояние — "отключено", поэтому фон второго `Entry` является розовым на экранах iOS и Android. Реализация `Entry` UWP не позволяет задать цвет фона при отключении `Entry`. 

При вводе какого-либо текста в третий `Entry` второй `Entry` переключается в "нормальное" состояние, а фон теперь является травяным:

[![VSM в представлении: обычная](vsm-images/VsmOnViewNormal.png "VSM в представлении — обычная")](vsm-images/VsmOnViewNormal-Large.png#lightbox)

При касании второго `Entry` он получает фокус ввода. Он переключается в состояние "назначено" и увеличивает его высоту вдвое:

[![VSM в представлении: с упором](vsm-images/VsmOnViewFocused.png "VSM для просмотра — ориентированный на представление")](vsm-images/VsmOnViewFocused-Large.png#lightbox)

Обратите внимание, что `Entry` не удерживает травяной фон, когда он получает фокус ввода. Когда диспетчер визуального состояния переключается между визуальными состояниями, свойства, заданные предыдущим состоянием, задаются неопределенными. Помните, что визуальные состояния являются взаимоисключающими. "Нормальное" состояние не означает, что `Entry` включен. Это означает, что `Entry` включен и не имеет фокуса ввода. 

Если вы хотите, чтобы `Entry` имело травяное изображение в состоянии "назначено", добавьте еще один `Setter` в это визуальное состояние:

```xaml
<VisualState x:Name="Focused">
    <VisualState.Setters>
        <Setter Property="FontSize" Value="36" />
        <Setter Property="BackgroundColor" Value="Lime" />
    </VisualState.Setters>
</VisualState>
```

Чтобы эти `Setter` объекты работали правильно, `VisualStateGroup` должны содержать `VisualState` объекты для всех состояний в этой группе. Если имеется визуальное состояние, в котором нет объектов `Setter`, включите его в все равно как пустой тег:

```xaml
<VisualState x:Name="Normal" />
``` 

### <a name="visual-state-manager-markup-in-a-style"></a>Разметка диспетчера визуального состояния в стиле

Часто возникает необходимость совместно использовать ту же разметку диспетчера визуального состояния для двух или более представлений. В этом случае необходимо поместить разметку в определение `Style`.

Ниже приведена существующая неявная `Style` для элементов `Entry` на странице **представления VSM on View** :

```xaml
<Style TargetType="Entry">
    <Setter Property="Margin" Value="20, 0" />
    <Setter Property="FontSize" Value="18" />
</Style> 
```

Добавьте теги `Setter` для привязанного к `VisualStateManager.VisualStateGroups` свойства привязки:

```xaml
<Style TargetType="Entry">
    <Setter Property="Margin" Value="20, 0" />
    <Setter Property="FontSize" Value="18" />
    <Setter Property="VisualStateManager.VisualStateGroups">

    </Setter>
</Style> 
```

Свойство Content для `Setter` `Value`, поэтому значение свойства `Value` можно указать непосредственно в этих тегах. Это свойство имеет тип `VisualStateGroupList`:

```xaml
<Style TargetType="Entry">
    <Setter Property="Margin" Value="20, 0" />
    <Setter Property="FontSize" Value="18" />
    <Setter Property="VisualStateManager.VisualStateGroups">
        <VisualStateGroupList>

        </VisualStateGroupList>
    </Setter>
</Style> 
```

Внутри этих тегов можно включить один или несколько объектов `VisualStateGroup`:

```xaml
<Style TargetType="Entry">
    <Setter Property="Margin" Value="20, 0" />
    <Setter Property="FontSize" Value="18" />
    <Setter Property="VisualStateManager.VisualStateGroups">
        <VisualStateGroupList>
            <VisualStateGroup x:Name="CommonStates">

            </VisualStateGroup>
        </VisualStateGroupList>
    </Setter>
</Style> 
```

Оставшаяся часть разметки VSM та же, что и раньше.

Ниже приведена страница **VSM в стиле** , на которой показана полная разметка VSM:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="VsmDemos.VsmInStylePage"
             Title="VSM in Style">
    <StackLayout>
        <StackLayout.Resources>
            <Style TargetType="Entry">
                <Setter Property="Margin" Value="20, 0" />
                <Setter Property="FontSize" Value="18" />
                <Setter Property="VisualStateManager.VisualStateGroups">
                    <VisualStateGroupList>
                        <VisualStateGroup x:Name="CommonStates">

                            <VisualState x:Name="Normal">
                                <VisualState.Setters>
                                    <Setter Property="BackgroundColor" Value="Lime" />
                                </VisualState.Setters>
                            </VisualState>

                            <VisualState x:Name="Focused">
                                <VisualState.Setters>
                                    <Setter Property="FontSize" Value="36" />
                                    <Setter Property="BackgroundColor" Value="Lime" />
                                </VisualState.Setters>
                            </VisualState>

                            <VisualState x:Name="Disabled">
                                <VisualState.Setters>
                                    <Setter Property="BackgroundColor" Value="Pink" />
                                </VisualState.Setters>
                            </VisualState>
                        </VisualStateGroup>
                    </VisualStateGroupList>
                </Setter>
            </Style>

            <Style TargetType="Label">
                <Setter Property="Margin" Value="20, 30, 20, 0" />
                <Setter Property="FontSize" Value="Large" />
            </Style>
        </StackLayout.Resources>

        <Label Text="Normal Entry:" />

        <Entry />
        
        <Label Text="Entry with VSM: " />

        <Entry>
            <Entry.Triggers>
                <DataTrigger TargetType="Entry"
                             Binding="{Binding Source={x:Reference entry3},
                                               Path=Text.Length}"
                             Value="0">
                    <Setter Property="IsEnabled" Value="False" />
                </DataTrigger>
            </Entry.Triggers>
        </Entry>

        <Label Text="Entry to enable 2nd Entry:" />

        <Entry x:Name="entry3"
               Text=""
               Placeholder="Type something to enable 2nd Entry" />
    </StackLayout>
</ContentPage>
```

Теперь все представления `Entry` на этой странице одинаково реагируют на визуальные состояния. Также обратите внимание, что состояние "Фокус" теперь включает вторую `Setter`, которая дает каждому `Entry` травяной фон, а также когда он имеет фокус ввода:

[![VSM в стиле](vsm-images/VsmInStyle.png "VSM в стиле")](vsm-images/VsmInStyle-Large.png#lightbox)

## <a name="defining-your-own-visual-states"></a>Определение собственных визуальных состояний

Каждый класс, производный от `VisualElement`, поддерживает три стандартных состояния: "обычный", "с сортировкой" и "отключен". На внутреннем уровне класс [`VisualElement`](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Core/VisualElement.cs) обнаруживает, когда он становится включенным или отключенным, и вызывает статический метод [`VisualStateManager.GoToState`](xref:Xamarin.Forms.VisualStateManager.GoToState(Xamarin.Forms.VisualElement,System.String)) :

```csharp
VisualStateManager.GoToState(this, "Focused");
```

Это единственный код диспетчера визуального состояния, который можно найти в классе `VisualElement`. Поскольку `GoToState` вызывается для каждого объекта, основанного на каждом классе, производном от `VisualElement`, можно использовать Диспетчер визуальных состояний с любым объектом `VisualElement` для реагирования на эти изменения.

Интересно, что имя группы визуального состояния "Коммонстатес" не упоминается явно в `VisualElement`. Имя группы не входит в API для диспетчера визуальных состояний. В одном из двух примеров программы, показанных на данный момент, можно изменить имя группы с «Коммонстатес» на любое другое, и программа по-прежнему будет работать. Имя группы — это просто общее описание состояний в этой группе. Неявное понимание того, что визуальные состояния в любой группе являются взаимоисключающими: одно состояние и только одно состояние является текущим в любое время.

Если вы хотите реализовать собственные визуальные состояния, необходимо вызвать `VisualStateManager.GoToState` из кода. Чаще всего этот вызов будет осуществляться из файла кода программной части класса Page.

На странице **проверки VSM** в образце **[всмдемос](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)** показано, как использовать Диспетчер визуальных состояний в связи с проверкой входных данных. Файл XAML состоит из двух элементов `Label`, `Entry` и `Button`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="VsmDemos.VsmValidationPage"
             Title="VSM Validation">
    <StackLayout Padding="10, 10">
        
        <Label Text="Enter a U.S. phone number:"
               FontSize="Large" />

        <Entry Placeholder="555-555-5555"
               FontSize="Large"
               Margin="30, 0, 0, 0"
               TextChanged="OnTextChanged" />

        <Label x:Name="helpLabel"
               Text="Phone number must be of the form 555-555-5555, and not begin with a 0 or 1">
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup Name="ValidityStates">
                    
                    <VisualState Name="Valid">
                        <VisualState.Setters>
                            <Setter Property="TextColor" Value="Transparent" />
                        </VisualState.Setters>
                    </VisualState>

                    <VisualState Name="Invalid" />
                    
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>
        </Label>

        <Button x:Name="submitButton"
                Text="Submit"
                FontSize="Large"
                Margin="0, 20"
                VerticalOptions="Center"
                HorizontalOptions="Center">
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup Name="ValidityStates">
                    
                    <VisualState Name="Valid" />

                    <VisualState Name="Invalid">
                        <VisualState.Setters>
                            <Setter Property="IsEnabled" Value="False" />
                        </VisualState.Setters>
                    </VisualState>
                    
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>
        </Button>
    </StackLayout>
</ContentPage>
```

Разметка VSM прикрепляется ко второму `Label` (с именем `helpLabel`) и `Button` (с именем `submitButton`). Существует два взаимоисключающих состояния с именами "Valid" и "Invalid". Обратите внимание, что каждая из двух групп "Валидатионстате" содержит теги `VisualState` как для "допустимого", так и для "недопустимого", хотя один из них пуст в каждом случае. 

Если `Entry` не содержит допустимый номер телефона, то текущим состоянием является "Invalid", поэтому второй `Label` является видимым, а `Button` отключена:

[![Проверка VSM: недопустимое состояние](vsm-images/VsmValidationInvalid.png "Проверка VSM-недопустимо")](vsm-images/VsmValidationInvalid-Large.png#lightbox)

При указании допустимого номера телефона текущее состояние становится допустимым. Второй `Entry` исчезает, и теперь `Button` включена.

[![Проверка VSM: допустимое состояние](vsm-images/VsmValidationValid.png "Проверка VSM — допустимая")](vsm-images/VsmValidationValid-Large.png#lightbox)

Файл кода программной части — это репонсибле для обработки события `TextChanged` из `Entry`. Обработчик использует регулярное выражение для определения допустимости входной строки. Метод в файле кода программной части с именем `GoToState` вызывает статический метод `VisualStateManager.GoToState` как для `helpLabel`, так и для `submitButton`:

```csharp
public partial class VsmValidationPage : ContentPage
{
    public VsmValidationPage ()
    {
        InitializeComponent ();

        GoToState(false);
    }

    void OnTextChanged(object sender, TextChangedEventArgs args)
    {
        bool isValid = Regex.IsMatch(args.NewTextValue, @"^[2-9]\d{2}-\d{3}-\d{4}$");
        GoToState(isValid);
    }

    void GoToState(bool isValid)
    {
        string visualState = isValid ? "Valid" : "Invalid";
        VisualStateManager.GoToState(helpLabel, visualState);
        VisualStateManager.GoToState(submitButton, visualState);
    }
}
```

Кроме того, обратите внимание, что метод `GoToState` вызывается из конструктора для инициализации состояния. Всегда должно быть текущее состояние. Но нигде в коде нет какой-либо ссылки на имя группы визуальных состояний, хотя для ясности на него имеется ссылка в XAML как «Валидатионстатес». 

Обратите внимание, что файл кода программной части должен принимать учетные записи каждого объекта на странице, на который влияют эти визуальные состояния, а также вызывать `VisualStateManager.GoToState` для каждого из этих объектов. В этом примере это только два объекта (`Label` и `Button`), но это может быть еще несколько.

Может возникнуть вопрос: Если файл кода программной части должен ссылаться на каждый объект на странице, на который влияют эти визуальные состояния, почему файл кода программной части просто напрямую обращается к объектам? Конечно, это может быть. Однако преимущество использования VSM заключается в том, что вы можете контролировать, как визуальные элементы реагируют на разные состояния в XAML, что позволяет хранить всю структуру пользовательского интерфейса в одном расположении. Это позволяет избежать настройки визуального отображения путем доступа к визуальным элементам непосредственно из кода программной части.

Может возникнуть желание рассмотреть возможность наследования класса от `Entry` и, возможно, определения свойства, которое можно задать для внешней функции проверки. Класс, производный от `Entry`, может затем вызвать метод `VisualStateManager.GoToState`. Эта схема будет работать нормально, но только в том случае, если `Entry` единственным объектом, затронутым различными визуальными состояниями. В этом примере также затрагиваются `Label` и `Button`. Невозможно прикрепить разметку VSM к `Entry` для управления другими объектами на странице, а разметка VSM, прикрепленная к этим объектам, должна ссылаться на изменение визуального состояния из другого объекта.

<a name="adaptive-layout" />

## <a name="using-the-visual-state-manager-for-adaptive-layout"></a>Использование диспетчера визуальных состояний для адаптивного макета

Как правило, приложение Xamarin. Forms, работающее на телефоне, обычно просматривается в книжной или альбомной пропорции, и для программы Xamarin. Forms, выполняющейся на рабочем столе, можно изменить размер, чтобы иметь много различных размеров и пропорций. Хорошо спроектированное приложение может отображать свое содержимое по-разному для различных форм и конструктивных параметров формы. 

Этот метод иногда называют _адаптивным макетом_. Поскольку адаптивный макет включает только визуальные элементы программы, это идеальное приложение диспетчера визуального состояния.

Простой пример — это приложение, которое отображает небольшую коллекцию кнопок, влияющих на содержимое приложения. В портретном режиме эти кнопки могут отображаться в горизонтальной строке в верхней части страницы:

[![Адаптивный макет VSM: Книжная](vsm-images/VsmAdaptiveLayoutPortrait.png "Адаптивный макет VSM — книжная ориентация")](vsm-images/VsmAdaptiveLayoutPortrait-Large.png#lightbox)

В альбомном режиме массив кнопок может быть перемещен на одну сторону и отображаться в столбце:

[![Адаптивный макет VSM: альбомная ориентация](vsm-images/VsmAdaptiveLayoutLandscape.png "Адаптивный макет VSM — альбомная ориентация")](vsm-images/VsmAdaptiveLayoutLandscape-Large.png#lightbox)

В верхней части окна программа работает на универсальная платформа Windows, Android и iOS.

На странице **адаптивного макета VSM** в образце [всмдемос](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos) определена группа с именем "ориентатионстатес" с двумя визуальными состояниями "Книжная" и "Альбомная". (Более сложный подход может основываться на нескольких разных ширинах страницы или окна.) 

Разметка VSM происходит в четырех местах файла XAML. @No__t_0 с именем `mainStack` содержит как меню, так и содержимое, которое является элементом `Image`. Эта `StackLayout` должна иметь вертикальную ориентацию в книжной и горизонтальной ориентации в альбомном режиме:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="VsmDemos.VsmAdaptiveLayoutPage"
             Title="VSM Adaptive Layout">

    <StackLayout x:Name="mainStack">
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup Name="OrientationStates">

                <VisualState Name="Portrait">
                    <VisualState.Setters>
                        <Setter Property="Orientation" Value="Vertical" />
                    </VisualState.Setters>
                </VisualState>

                <VisualState Name="Landscape">
                    <VisualState.Setters>
                        <Setter Property="Orientation" Value="Horizontal" />
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>
        
        <ScrollView x:Name="menuScroll">
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup Name="OrientationStates">
                    
                    <VisualState Name="Portrait">
                        <VisualState.Setters>
                            <Setter Property="Orientation" Value="Horizontal" />
                        </VisualState.Setters>
                    </VisualState>

                    <VisualState Name="Landscape">
                        <VisualState.Setters>
                            <Setter Property="Orientation" Value="Vertical" />
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>
            
            <StackLayout x:Name="menuStack">
                <VisualStateManager.VisualStateGroups>
                    <VisualStateGroup Name="OrientationStates">

                        <VisualState Name="Portrait">
                            <VisualState.Setters>
                                <Setter Property="Orientation" Value="Horizontal" />
                            </VisualState.Setters>
                        </VisualState>

                        <VisualState Name="Landscape">
                            <VisualState.Setters>
                                <Setter Property="Orientation" Value="Vertical" />
                            </VisualState.Setters>
                        </VisualState>
                    </VisualStateGroup>
                </VisualStateManager.VisualStateGroups>

                <StackLayout.Resources>
                    <Style TargetType="Button">
                        <Setter Property="VisualStateManager.VisualStateGroups">
                            <VisualStateGroupList>
                                <VisualStateGroup Name="OrientationStates">

                                    <VisualState Name="Portrait">
                                        <VisualState.Setters>
                                            <Setter Property="HorizontalOptions" Value="CenterAndExpand" />
                                            <Setter Property="Margin" Value="10, 5" />
                                        </VisualState.Setters>
                                    </VisualState>

                                    <VisualState Name="Landscape">
                                        <VisualState.Setters>
                                            <Setter Property="VerticalOptions" Value="CenterAndExpand" />
                                            <Setter Property="HorizontalOptions" Value="Center" />
                                            <Setter Property="Margin" Value="10" />
                                        </VisualState.Setters>
                                    </VisualState>
                                </VisualStateGroup>
                            </VisualStateGroupList>
                        </Setter>
                    </Style>
                </StackLayout.Resources>

                <Button Text="Banana"
                        Command="{Binding SelectedCommand}"
                        CommandParameter="Banana.jpg" />
                
                <Button Text="Face Palm"
                        Command="{Binding SelectedCommand}"
                        CommandParameter="FacePalm.jpg" />
                
                <Button Text="Monkey"
                        Command="{Binding SelectedCommand}"
                        CommandParameter="monkey.png" />
                
                <Button Text="Seated Monkey"
                        Command="{Binding SelectedCommand}"
                        CommandParameter="SeatedMonkey.jpg" />
            </StackLayout>
        </ScrollView>

        <Image x:Name="image"
               VerticalOptions="FillAndExpand"
               HorizontalOptions="FillAndExpand" />
    </StackLayout>
</ContentPage>
```

Внутреннее `ScrollView` с именем `menuScroll` и `StackLayout`, именуемое `menuStack`, реализуют меню кнопок. Ориентация этих макетов противоположна `mainStack`. Меню должно располагаться горизонтально в книжной ориентации и вертикально в альбомном режиме.

Четвертый раздел разметки VSM имеет неявный стиль для самих кнопок. Этот разметка задает свойства `VerticalOptions`, `HorizontalOptions` и `Margin`, относящиеся к портаит и альбомным ориентациям.

Файл кода программной части задает свойство `BindingContext` `menuStack` для реализации `Button` команд, а также присоединяет обработчик к событию `SizeChanged` страницы:

```csharp
public partial class VsmAdaptiveLayoutPage : ContentPage
{
    public VsmAdaptiveLayoutPage ()
    {
        InitializeComponent ();

        SizeChanged += (sender, args) =>
        {
            string visualState = Width > Height ? "Landscape" : "Portrait";
            VisualStateManager.GoToState(mainStack, visualState);
            VisualStateManager.GoToState(menuScroll, visualState);
            VisualStateManager.GoToState(menuStack, visualState);

            foreach (View child in menuStack.Children)
            {
                VisualStateManager.GoToState(child, visualState);
            }
        };

        SelectedCommand = new Command<string>((filename) =>
        {
            image.Source = ImageSource.FromResource("VsmDemos.Images." + filename);
        });

        menuStack.BindingContext = this;
    }

    public ICommand SelectedCommand { private set; get; }
}
```

Обработчик `SizeChanged` вызывает `VisualStateManager.GoToState` для двух элементов `StackLayout` и `ScrollView`, а затем просматривает дочерние элементы `menuStack`, чтобы вызвать `VisualStateManager.GoToState` для элементов `Button`.

Может показаться, что файл кода программной части может более точно управлять изменением ориентации, устанавливая свойства элементов в файле XAML, но диспетчер визуального состояния является определенно более структурированным подходом. Все визуальные элементы хранятся в файле XAML, где они становятся проще исследовать, обслуживать и изменять.

## <a name="visual-state-manager-with-xamarinuniversity"></a>Диспетчер визуальных состояний с Xamarin. Университет

> [!VIDEO https://youtube.com/embed/qhUHbVP5mIQ]

**Видео по диспетчеру визуального состояния Xamarin. Forms 3,0**

## <a name="related-links"></a>Связанные ссылки

- [всмдемос](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)
