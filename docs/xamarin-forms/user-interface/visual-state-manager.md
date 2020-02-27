---
title: Диспетчер визуальных состояний Xamarin. Forms
description: Используйте Диспетчер визуальных состояний вносить изменения в элементы XAML, визуальные состояния набора из кода.
ms.prod: xamarin
ms.assetid: 17296F14-640D-484B-A24C-A4E9B7013E4F
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 02/21/2020
ms.openlocfilehash: 086adee4dc6b921abe92f6486186023a3125695c
ms.sourcegitcommit: 10b4d7952d78f20f753372c53af6feb16918555c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/26/2020
ms.locfileid: "77636070"
---
# <a name="xamarinforms-visual-state-manager"></a>Диспетчер визуальных состояний Xamarin. Forms

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)

_Используйте Диспетчер визуальных состояний для внесения изменений в элементы XAML на основе визуальных состояний, заданных из кода._

Диспетчер визуальных состояний (VSM) предоставляет структурированный способ внесения визуальных изменений в пользовательский интерфейс из кода. В большинстве случаев в пользовательском интерфейсе приложения определяется в XAML, и этот XAML содержит разметку, описывающий влияние Диспетчер визуальных состояний визуальные элементы пользовательского интерфейса.

В VSM введена концепция _визуальных состояний_. Представление Xamarin. Forms, например `Button`, может иметь несколько различных визуальных представлений в зависимости от его базового состояния &mdash;, отключено ли оно или нажато или имеет фокус ввода. Это состояний кнопки.

Визуальные состояния собираются в _группах визуальных состояний_. Все визуальные состояния в группе визуальных состояний являются взаимоисключающими. Визуальные состояния и группы визуальных состояний идентифицируются по простые текстовые строки.

Диспетчер визуальных состояний Xamarin. Forms определяет одну группу визуальных состояний с именем "Коммонстатес" со следующими визуальными состояниями:

- "Normal"
- «Отключено»
- «С фокусом ввода»
- Выбрать

Эта группа визуальных состояний поддерживается для всех классов, производных от [`VisualElement`](xref:Xamarin.Forms.VisualElement), которые являются базовым классом для [`View`](xref:Xamarin.Forms.View) и [`Page`](xref:Xamarin.Forms.Page).

Можно также определить собственные группы визуальных состояний и визуальные состояния, как в этой статье мы рассмотрим.

> [!NOTE]
> Разработчики Xamarin. Forms, знакомые с [триггерами](~/xamarin-forms/app-fundamentals/triggers.md) , знают, что триггеры также могут вносить изменения в визуальные элементы в пользовательском интерфейсе на основе изменений в свойствах представления или при срабатывании событий. Тем не менее использование триггеров для работы с различными сочетаниями этих изменений может стать довольно запутанным. Исторически сложилось так, что Диспетчер визуальных состояний был введен в среду на основе XAML Windows для устранения путаницы, полученный в результате сочетания визуальных состояний. В VSM визуальные состояния в группе визуальных состояний всегда являются взаимоисключающими. В любое время только одно состояние в каждой группе является текущим состоянием.

## <a name="common-states"></a>Распространенные состояния

Диспетчер визуальных состояний позволяет включить в файл XAML разметку, которая может изменить внешний вид представления, если оно является нормальным или отключенным, или имеет фокус ввода. Они называются _распространенными состояниями_.

Например, предположим, что на странице имеется `Entry`ное представление, и необходимо изменить внешний вид `Entry` следующим образом:

- Если `Entry` отключен, `Entry` должен иметь розовый фон.
- `Entry` должен иметь травяной фон в обычном режиме.
- При наличии фокуса ввода `Entry` должен расширяться в два раза больше его стандартной высоты.

Можно подключить VSM разметки для отдельного представления, или если он применяется к нескольким представлениям можно определить в стиле. В следующих двух разделах описываются эти подходы.

### <a name="vsm-markup-on-a-view"></a>Этот режим разметки для представления

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

Следующим шагом является включение пару тегов для каждого визуального состояния в этой группе. Их также можно определить с помощью `x:Name` или `Name`.

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

Обратите внимание, что во втором `Entry` также имеется `DataTrigger` в составе `Trigger` коллекции. Это приводит к отключению `Entry`, пока что что-то не будет введено в третий `Entry`. Ниже приведен страницы при запуске под управлением iOS, Android и универсальной платформы Windows (UWP).

[![VSM в представлении: отключено](vsm-images/VsmOnViewDisabled.png "VSM при просмотре — отключено")](vsm-images/VsmOnViewDisabled-Large.png#lightbox)

Текущее визуальное состояние — "отключено", поэтому фон второго `Entry` является розовым на экранах iOS и Android. Реализация `Entry` UWP не позволяет задать цвет фона при отключении `Entry`.

При вводе какого-либо текста в третий `Entry`второй `Entry` переключается в "нормальное" состояние, а фон теперь является травяным:

[![VSM в представлении: обычная](vsm-images/VsmOnViewNormal.png "VSM в представлении — обычная")](vsm-images/VsmOnViewNormal-Large.png#lightbox)

При касании второго `Entry`он получает фокус ввода. Он переключается в состояние «Focused» и дважды его высота при развертывании:

[![VSM в представлении: с упором](vsm-images/VsmOnViewFocused.png "VSM для просмотра — ориентированный на представление")](vsm-images/VsmOnViewFocused-Large.png#lightbox)

Обратите внимание, что `Entry` не удерживает травяной фон, когда он получает фокус ввода. Как Диспетчер визуальных состояний переключается между визуальными состояниями, свойств, заданных в предыдущее состояние не задано. Имейте в виду, что визуальные состояния являются взаимоисключающими. "Нормальное" состояние не означает, что `Entry` включен. Это означает, что `Entry` включен и не имеет фокуса ввода.

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

### <a name="visual-state-manager-markup-in-a-style"></a>Разметка Диспетчер визуальных состояний в стиле

Часто бывает необходимо совместно использовать одну разметку Диспетчер визуальных состояний между два или несколько представлений. В этом случае необходимо поместить разметку в определение `Style`.

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

Остальная часть разметки VSM используется теми же самыми.

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

## <a name="visual-states-in-xamarinforms"></a>Визуальные состояния в Xamarin. Forms

В следующей таблице перечислены визуальные состояния, определенные в Xamarin. Forms:

| Class | Состояния | Дополнительные сведения |
| ----- | ------ | ---------------- |
| `Button` | `Pressed` | [Визуальные состояния кнопки](~/xamarin-forms/user-interface/button.md#button-visual-states) |
| `CarouselView` | `DefaultItem`, `CurrentItem`, `PreviousItem`, `NextItem` | [Визуальные состояния Карауселвиев](~/xamarin-forms/user-interface/carouselview/interaction.md#define-visual-states) |
| `CollectionView` | `Selected` | [Изменить цвет выбранного элемента](~/xamarin-forms/user-interface/collectionview/selection.md#change-selected-item-color) |
| `ImageButton` | `Pressed` | [Визуальные состояния ImageButton](~/xamarin-forms/user-interface/imagebutton.md#imagebutton-visual-states) |
| `VisualElement` | `Normal`, `Disabled`, `Focused`, `Selected` | [Распространенные состояния](#common-states) |

Доступ к каждому из этих состояний можно получить с помощью группы визуального состояния с именем `CommonStates`.

## <a name="set-state-on-multiple-elements"></a>Задание состояния для нескольких элементов

В предыдущих примерах визуальные состояния были присоединены к отдельным элементам и работали с ними. Однако можно также создать визуальные состояния, присоединенные к одному элементу, но задавайте свойства других элементов в той же области. Это позволяет избежать повторения визуальных состояний для каждого элемента, над которым работают состояния.

Тип [`Setter`](xref:Xamarin.Forms.Setter) имеет свойство `TargetName` типа `string`, представляющее целевой элемент, который будет обрабатывать `Setter` для визуального состояния. Если определено свойство `TargetName`, `Setter` задает `Property` элемента, определенного в `TargetName`, в `Value`:

```xaml
<Setter TargetName="label"
        Property="Label.TextColor"
        Value="Red" />
```

В этом примере `Label` с именем `label` будет иметь свойство `TextColor`, для которого задано значение `Red`. При задании свойства `TargetName` необходимо указать полный путь к свойству в `Property`. Таким образом, чтобы задать свойство `TextColor` для `Label`, `Property` задается как `Label.TextColor`.

> [!NOTE]
> Любое свойство, на которое ссылается объект `Setter`, должно поддерживаться связываемым свойством.

Страница **VSM with Setter (TargetName** ) в образце **[всмдемос](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)** показывает, как задать состояние для нескольких элементов из одной группы состояний визуализации. Файл XAML состоит из `StackLayout`, содержащего элемент `Label`, `Entry`и `Button`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="VsmDemos.VsmSetterTargetNamePage"
             Title="VSM with Setter TargetName">
    <StackLayout Margin="10">
        <Label Text="What is the capital of France?" />
        <Entry x:Name="entry"
               Placeholder="Enter answer" />
        <Button Text="Reveal answer">
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup x:Name="CommonStates">
                    <VisualState x:Name="Normal" />
                    <VisualState x:Name="Pressed">
                        <VisualState.Setters>
                            <Setter Property="Scale"
                                    Value="0.8" />
                            <Setter TargetName="entry"
                                    Property="Entry.Text"
                                    Value="Paris" />
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>
        </Button>
    </StackLayout>
</ContentPage>
```

Разметка VSM прикрепляется к `StackLayout`. Существует два взаимоисключающих состояния с именами "нормально" и "нажато", с каждым состоянием, содержащим теги `VisualState`.

"Нормальное" состояние активно, если `Button` не нажата, а ответ на вопрос можно указать:

[![Метод задания VSM TargetName: нормальное состояние](vsm-images/VsmSetterTargetNameNormal.png "Метод задания VSM, TargetName — нормальный")](vsm-images/VsmSetterTargetNameNormal-Large.png#lightbox)

"Нажатое" состояние становится активным при нажатии `Button`:

[![Метод задания VSM, TargetName: нажатое состояние](vsm-images/VsmSetterTargetNamePressed.png "Метод задания VSM, нажатие TargetName")](vsm-images/VsmSetterTargetNamePressed-Large.png#lightbox)

«Нажатый» `VisualState` указывает, что при нажатии `Button` его свойство `Scale` будет изменено со значения по умолчанию от 1 до 0,8. Кроме того, `Entry` с именем `entry` будет иметь свойство `Text`, установленное в Париж. Таким образом, в результате, когда `Button` нажата кнопка, масштабирование будет немного меньше, а `Entry` отображает Париж. Затем, когда `Button` освобождается, он повторно масштабируется до значения по умолчанию 1, а `Entry` отображает любой ранее введенный текст.

> [!IMPORTANT]
> Пути к свойствам в настоящее время не поддерживаются в элементах `Setter`, которые указывают свойство `TargetName`.

## <a name="define-your-own-visual-states"></a>Определение собственных визуальных состояний

Каждый класс, производный от `VisualElement`, поддерживает три стандартных состояния: "обычный", "с сортировкой" и "отключен". На внутреннем уровне класс [`VisualElement`](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Core/VisualElement.cs) обнаруживает, когда он становится включенным или отключенным, и вызывает статический метод [`VisualStateManager.GoToState`](xref:Xamarin.Forms.VisualStateManager.GoToState(Xamarin.Forms.VisualElement,System.String)) :

```csharp
VisualStateManager.GoToState(this, "Focused");
```

Это единственный код диспетчера визуального состояния, который можно найти в классе `VisualElement`. Поскольку `GoToState` вызывается для каждого объекта, основанного на каждом классе, производном от `VisualElement`, можно использовать Диспетчер визуальных состояний с любым объектом `VisualElement` для реагирования на эти изменения.

Интересно, что имя группы визуального состояния "Коммонстатес" не упоминается явно в `VisualElement`. Имя группы не является частью API-Интерфейс для диспетчера визуальных состояний. В одном из двух пример программы, приведенные выше, можно изменить имя группы из «CommonStates», на что-нибудь еще, и программа будет по-прежнему работать. Имя группы является просто общим описанием состояния из этой группы. Неявно понятно, что визуальные состояния в любой группе являются взаимоисключающими: одно состояние и только одно состояние является текущей в любое время.

Если вы хотите реализовать собственные визуальные состояния, необходимо вызвать `VisualStateManager.GoToState` из кода. Чаще всего будет вызывать этот метод из файла кода класса страницы.

На странице **проверки VSM** в образце **[всмдемос](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)** показано, как использовать Диспетчер визуальных состояний в связи с проверкой входных данных. XAML-файл состоит из `StackLayout`, содержащего два элемента `Label`, `Entry`и `Button`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="VsmDemos.VsmValidationPage"
             Title="VSM Validation">
    <StackLayout x:Name="stackLayout"
                 Padding="10, 10">
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup Name="ValidityStates">
                    <VisualState Name="Valid">
                        <VisualState.Setters>
                            <Setter TargetName="helpLabel"
                                    Property="Label.TextColor"
                                    Value="Transparent" />
                            <Setter TargetName="entry"
                                    Property="Entry.BackgroundColor"
                                    Value="Lime" />
                        </VisualState.Setters>
                    </VisualState>
                    <VisualState Name="Invalid">
                        <VisualState.Setters>
                            <Setter TargetName="entry"
                                    Property="Entry.BackgroundColor"
                                    Value="Pink" />
                            <Setter TargetName="submitButton"
                                    Property="Button.IsEnabled"
                                    Value="False" />
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>
        <Label Text="Enter a U.S. phone number:"
               FontSize="Large" />
        <Entry x:Name="entry"
               Placeholder="555-555-5555"
               FontSize="Large"
               Margin="30, 0, 0, 0"
               TextChanged="OnTextChanged" />
        <Label x:Name="helpLabel"
               Text="Phone number must be of the form 555-555-5555, and not begin with a 0 or 1" />
        <Button x:Name="submitButton"
                Text="Submit"
                FontSize="Large"
                Margin="0, 20"
                VerticalOptions="Center"
                HorizontalOptions="Center" />
    </StackLayout>
</ContentPage>
```

Разметка VSM прикрепляется к `StackLayout` (с именем `stackLayout`). Существует два взаимоисключающих состояния с именами "Valid" и "Invalid" с каждым состоянием, содержащим теги `VisualState`.

Если `Entry` не содержит допустимый номер телефона, текущее состояние — "Недопустимый", поэтому `Entry` имеет розовый фон, второй `Label` является видимым, а `Button` отключена.

[![Проверка VSM: недопустимое состояние](vsm-images/VsmValidationInvalid.png "Проверка VSM-недопустимо")](vsm-images/VsmValidationInvalid-Large.png#lightbox)

Если введен правильный номер телефона, текущее состояние становится «Действителен». `Entry` получает травяной фон, второй `Label` исчезает, а `Button` теперь включается:

[![Проверка VSM: допустимое состояние](vsm-images/VsmValidationValid.png "Проверка VSM — допустимая")](vsm-images/VsmValidationValid-Large.png#lightbox)

Файл кода программной части отвечает за обработку события `TextChanged` из `Entry`. Обработчик использует регулярное выражение для определения, если входная строка является допустимым, или нет. Метод в файле кода программной части с именем `GoToState` вызывает статический метод `VisualStateManager.GoToState` для `stackLayout`:

```csharp
public partial class VsmValidationPage : ContentPage
{
    public VsmValidationPage()
    {
        InitializeComponent();

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
        VisualStateManager.GoToState(stackLayout, visualState);
    }
}
```

Кроме того, обратите внимание, что метод `GoToState` вызывается из конструктора для инициализации состояния. Всегда должно быть текущее состояние. Но ничто в коде не существует любая ссылка на имя группы визуальных состояний, несмотря на то, что он указывается в XAML как «ValidationStates» в целях ясности.

Обратите внимание, что файл кода программной части должен иметь только учетную запись объекта на странице, определяющей визуальные состояния, и вызывать `VisualStateManager.GoToState` для этого объекта. Это обусловлено тем, что оба визуальных состояния нацелены на несколько объектов на странице.

Может возникнуть вопрос: Если файл кода программной части должен ссылаться на объект на странице, где определены визуальные состояния, то почему файл кода программной части просто напрямую обращается к этому и другим объектам? Конечно же удалось. Однако преимущество использования VSM — возможности контролировать то, как визуальные элементы реагировать на другое состояние полностью в XAML, который хранит все Дизайн пользовательского интерфейса в одном месте. Это позволяет избежать параметр внешний вид, обратившись к визуальные элементы непосредственно из кода.

## <a name="visual-state-triggers"></a>Триггеры визуального состояния

Визуальные состояния поддерживают триггеры состояния, которые являются специализированной группой триггеров, определяющих условия, при которых следует применять [`VisualState`](xref:Xamarin.Forms.VisualState) .

Триггеры состояния добавляются в коллекцию [`StateTriggers`](xref:Xamarin.Forms.VisualState.StateTriggers) [`VisualState`](xref:Xamarin.Forms.VisualState). Эта коллекция может содержать триггер одного состояния или несколько триггеров состояния. [`VisualState`](xref:Xamarin.Forms.VisualState) будет применен, когда в коллекции будут активны какие-либо триггеры состояния.

При использовании триггеров состояния для управления визуальными состояниями Xamarin. Forms использует следующие правила приоритета, чтобы определить, какой триггер (и соответствующие [`VisualState`](xref:Xamarin.Forms.VisualState)) будут активны:

1. Любой триггер, производный от [`StateTriggerBase`](xref:Xamarin.Forms.StateTriggerBase).
1. [`AdaptiveTrigger`](xref:Xamarin.Forms.AdaptiveTrigger) активируется из-за соблюдения условия [`MinWindowWidth`](xref:Xamarin.Forms.AdaptiveTrigger.MinWindowWidth) .
1. [`AdaptiveTrigger`](xref:Xamarin.Forms.AdaptiveTrigger) активируется из-за соблюдения условия [`MinWindowHeight`](xref:Xamarin.Forms.AdaptiveTrigger.MinWindowHeight) .

Если одновременно активно несколько триггеров (например, два пользовательских триггера), то первый триггер, объявленный в разметке, имеет приоритет.

Дополнительные сведения о триггерах состояния см. в разделе [триггеры состояния](~/xamarin-forms/app-fundamentals/triggers.md#state-triggers).

<a name="adaptive-layout" />

## <a name="use-the-visual-state-manager-for-adaptive-layout"></a>Использование диспетчера визуальных состояний для адаптивного макета

Xamarin.Forms, которое обычно приложения, работающего на телефоне можно просмотреть в книжной или Альбомная пропорции и Xamarin.Forms, выполняющихся на рабочем столе можно изменять принимать множество различных размеров и пропорций. Даже хорошо проработанное приложение может отображать его содержимое по-разному для этих различных странице или в окне форм-факторов.

Этот метод иногда называют _адаптивным макетом_. Поскольку адаптивный макет исключительно включает визуальные элементы программы, это приложение идеально Диспетчер визуальных состояний.

Простым примером является приложение, отображающее это небольшая совокупность кнопок, которые влияют на содержимое приложения. В книжной ориентации эти кнопки могут отображаться в верхней части страницы по горизонтали:

[![Адаптивный макет VSM: Книжная](vsm-images/VsmAdaptiveLayoutPortrait.png "Адаптивный макет VSM — книжная ориентация")](vsm-images/VsmAdaptiveLayoutPortrait-Large.png#lightbox)

В альбомной ориентации массив кнопки может быть перемещен к одной стороне и отображаемому в столбце:

[![Адаптивный макет VSM: альбомная ориентация](vsm-images/VsmAdaptiveLayoutLandscape.png "Адаптивный макет VSM — альбомная ориентация")](vsm-images/VsmAdaptiveLayoutLandscape-Large.png#lightbox)

Сверху вниз программа выполняется в универсальной платформы Windows, Android и iOS.

На странице **адаптивного макета VSM** в образце [всмдемос](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos) определена группа с именем "ориентатионстатес" с двумя визуальными состояниями "Книжная" и "Альбомная". (Более сложный подход может основываться на несколько различных значений ширины страницы или окно.)

Этот режим разметки происходит в четырех местах в файле XAML. `StackLayout` с именем `mainStack` содержит как меню, так и содержимое, которое является элементом `Image`. Эта `StackLayout` должна иметь вертикальную ориентацию в книжной и горизонтальной ориентации в альбомном режиме:

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

Внутреннее `ScrollView` с именем `menuScroll` и `StackLayout`, именуемое `menuStack`, реализуют меню кнопок. Ориентация этих макетов противоположна `mainStack`. Меню должно быть по горизонтали в книжной ориентации и по вертикали в альбомной ориентации.

В четвертой части разметки VSM находится в неявный стиль кнопок сами. Этот разметка задает свойства `VerticalOptions`, `HorizontalOptions`и `Margin`, относящиеся к книжной и альбомной ориентации.

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

Может показаться, как если бы файл с выделенным кодом может обрабатывать изменения ориентации более напрямую, задав свойства элементов в файле XAML, но Диспетчер визуальных состояний определенно более структурированный подход. Все, что визуальные элементы хранятся в файле XAML, где они становятся проще для изучения, обслуживании и изменении.

## <a name="visual-state-manager-with-xamarinuniversity"></a>Диспетчер визуальных состояний с Xamarin.University

> [!VIDEO https://youtube.com/embed/qhUHbVP5mIQ]

**Видео по диспетчеру визуального состояния Xamarin. Forms 3,0**

## <a name="related-links"></a>Связанные ссылки

- [всмдемос](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)
- [Триггеры состояния](~/xamarin-forms/app-fundamentals/triggers.md#state-triggers)
