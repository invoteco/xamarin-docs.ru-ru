---
title: Макет оболочки Xamarin.Forms
description: Следующим уровнем навигации после всплывающего меню в приложении оболочки является нижняя панель вкладок. Если вкладка содержит более одной страницы, перемещение по ним осуществляется с помощью верхней панели вкладок.
ms.prod: xamarin
ms.assetid: 318D81DB-E456-4E44-B083-36A27DBD9523
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: a8da1e96bbdf51899b1780265933402da791a03e
ms.sourcegitcommit: 0596004d4a0e599c1da1ddd75a6ac928f21191c2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/22/2019
ms.locfileid: "66005162"
---
# <a name="xamarinforms-shell-tabs"></a>Вкладки оболочки Xamarin.Forms

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/Xaminals/)

Следующим уровнем навигации после всплывающего меню в приложении оболочки является нижняя панель вкладок. Если же всплывающий элемент закрыт, верхним уровнем навигации в приложении считаются вкладки на нижней панели.

Каждый объект `FlyoutItem` может содержать один или несколько объектов `Tab`, где каждый объект `Tab` представляет вкладку на нижней панели. Каждый объект `Tab` может содержать один или несколько объектов `ShellContent`, где каждый объект `ShellContent` отображает один объект [`ContentPage`](xref:Xamarin.Forms.ContentPage). Если `Tab` содержит более одного объекта `ShellContent`, перемещение по объектам `ContentPage` осуществляется с помощью верхней панели вкладок.

В каждом объекте `ContentPage` можно перейти к дополнительным объектам `ContentPage`. Дополнительные сведения о навигации, см. в статье [о навигации в оболочке Xamarin.Forms](navigation.md).

## <a name="single-page-application"></a>Одностраничное приложение

Простейшее приложение оболочки содержит одну страницу, и для его создания достаточно добавить один объект `Tab` в объект `FlyoutItem`. В объекте `Tab` следует присвоить объекту `ShellContent` значение объекта [`ContentPage`](xref:Xamarin.Forms.ContentPage):

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell"
       FlyoutBehavior="Disabled">
    <FlyoutItem>
        <Tab>
            <ShellContent>
                <views:CatsPage />
            </ShellContent>
        </Tab>
    </FlyoutItem>
</Shell>
```

Этот код создаст такую структуру одностраничного приложения:

[![Снимок экрана с одностраничным приложением оболочки для iOS и Android](tabs-images/single-page-app.png "Одностраничное приложение оболочки")](tabs-images/single-page-app-large.png#lightbox "Одностраничное приложение оболочки")

В одностраничном приложении всплывающий элемент не нужен, поэтому свойство `Shell.FlyoutBehavior` получает значение `Disabled`.

> [!NOTE]
> При необходимости панель навигации можно спрятать, присвоив присоединенному свойству `Shell.NavBarIsVisible` в объекте [`ContentPage`](xref:Xamarin.Forms.ContentPage) значение `false`.

Оболочка содержит операторы неявного преобразования, которые позволяют упростить визуальную иерархию оболочки без добавления новых представлений в визуальное дерево. Это возможно благодаря тому, что производный объект `Shell` может содержать только объекты `FlyoutItem`, которые могут содержать только объекты `Tab`, которые могут содержать только объекты `ShellContent`. Эти операторы неявного преобразования позволяют удалить из предыдущего примера объекты `FlyoutItem`, `Tab` и `ShellContent`:

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell"
       FlyoutBehavior="Disabled">
    <views:CatsPage />
</Shell>
```

Это неявное преобразование автоматически заключает объект [`ContentPage`](xref:Xamarin.Forms.ContentPage) в объект `ShellContent`, который заключается в объект `Tab`, который заключается в объект `FlyoutItem`.

> [!IMPORTANT]
> В приложении оболочки каждый [`ContentPage`](xref:Xamarin.Forms.ContentPage), который является дочерним элементом объекта `ShellContent`, создается во время запуска приложения. Такой метод добавления объектов `ShellContent` приводит к созданию дополнительных страниц во время запуска приложения, что может замедлять запуск. Но оболочка может создавать страницы и по требованию, реагируя на переходы. Подробные сведения см. в разделе [об эффективной загрузке страниц](tabs.md#efficient-page-loading).

## <a name="bottom-tabs"></a>Нижние вкладки

Объекты `Tab` отображаются в виде нижних вкладок, если существует несколько объектов `Tab` в одном объекте `FlyoutItem`:

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell"
       FlyoutBehavior="Disabled">
    <FlyoutItem>
        <Tab Title="Cats"
             Icon="cat.png">
            <ShellContent>
                <views:CatsPage />
            </ShellContent>
        </Tab>
        <Tab Title="Dogs"
             Icon="dog.png">
            <ShellContent>
                <views:DogsPage />
            </ShellContent>
        </Tab>
    </FlyoutItem>
</Shell>
```

Заголовки и значки для вкладок задаются отдельно для каждого объекта `Tab` и отображаются на нижних вкладках:

[![Снимок экрана двухстраничного приложения оболочки с нижними вкладками для iOS и Android](tabs-images/two-page-app-bottom-tabs.png "Двухстраничное приложение оболочки с нижними вкладками")](tabs-images/two-page-app-bottom-tabs-large.png#lightbox "Двухстраничное приложение оболочки с нижними вкладками")

Вы также можете использовать операторы неявного преобразования оболочки для удаления объектов `ShellContent` и `Tab` из предыдущего примера:

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell"
       FlyoutBehavior="Disabled">
    <FlyoutItem>
        <views:CatsPage IconImageSource="cat.png" />
        <views:DogsPage IconImageSource="dog.png" />
    </FlyoutItem>
</Shell>
```

Это неявное преобразование автоматически заключает объект [`ContentPage`](xref:Xamarin.Forms.ContentPage) в объект `ShellContent`, а затем оба этих объекта — в объект `Tab`.

> [!IMPORTANT]
> В приложении оболочки каждый [`ContentPage`](xref:Xamarin.Forms.ContentPage), который является дочерним элементом объекта `ShellContent`, создается во время запуска приложения. Такой метод добавления объектов `ShellContent` приводит к созданию дополнительных страниц во время запуска приложения, что может замедлять запуск. Но оболочка может создавать страницы и по требованию, реагируя на переходы. Подробные сведения см. в разделе [об эффективной загрузке страниц](tabs.md#efficient-page-loading).

### <a name="tab-class"></a>Класс Tab

Класс `Tab` включает следующие свойства, которые определяют внешний вид и поведение вкладки:

- `CurrentItem` с типом `ShellContent` обозначает выбранный элемент.
- `FlyoutDisplayOptions` с типом `FlyoutDisplayOptions` определяет, как этот элемент и его дочерние элементы отображаются во всплывающем меню. Значение по умолчанию — `AsSingleItem`.
- `FlyoutIcon` с типом `ImageSource` определяет значок, который отображается во всплывающем элементе.
- `Icon` с типом `ImageSource` определяет значок, который отображается в частях хрома, не являющихся всплывающими элементами.
- `IsChecked` с типом `boolean` определяет, выделен ли этот элемент во всплывающем элементе в настоящий момент.
- `IsEnabled` с типом `boolean` определяет, можно ли выбрать элемент в хроме.
- `Items` с типом `ShellContentCollection` определяет все содержимое в `Tab`.
- `Title` с типом `string` определяет заголовок, отображаемый на вкладке в пользовательском интерфейсе.

## <a name="shell-content"></a>Содержимое оболочки

Дочерним элементом каждого объекта `Tab` является объект `ShellContent`, для свойства `Content` которого задается значение [`ContentPage`](xref:Xamarin.Forms.ContentPage):

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell"
       FlyoutBehavior="Disabled">
    <FlyoutItem>
        <Tab Title="Cats"
             Icon="cat.png">
            <ShellContent>
                <views:CatsPage />
            </ShellContent>
        </Tab>
        <Tab Title="Dogs"
             Icon="dog.png">
            <ShellContent>
                <views:DogsPage />
            </ShellContent>
        </Tab>
    </FlyoutItem>
</Shell>
```

В каждом объекте `ContentPage` можно перейти к дополнительным объектам `ContentPage`. Дополнительные сведения о навигации, см. в статье [о навигации в оболочке Xamarin.Forms](navigation.md).

### <a name="shellcontent-class"></a>Класс ShellContent

Класс `ShellContent` включает следующие свойства, которые определяют внешний вид и поведение содержимого вкладки:

- `Content` с типом `object` определяет содержимое из `ShellContent`.
- `ContentTemplate` с типом `DataTemplate` определяет шаблон, на основе которого динамически оформляется содержимое `ShellContent`.
- `FlyoutIcon` с типом `ImageSource` определяет значок, который отображается во всплывающем элементе.
- `Icon` с типом `ImageSource` определяет значок, который отображается в частях хрома, не являющихся всплывающими элементами.
- `IsChecked` с типом `boolean` определяет, выделен ли этот элемент во всплывающем элементе в настоящий момент.
- `IsEnabled` с типом `boolean` определяет, можно ли выбрать элемент в хроме.
- `MenuItems` с типом `MenuItemCollection` обозначает элементы меню, которые отображаются во всплывающем элементе, если этот `ShellContent` является отображаемой страницей.
- `Title` с типом `string` определяет заголовок, отображаемый на вкладке в пользовательском интерфейсе.

Все эти свойства поддерживаются объектами [`BindableProperty`](xref:Xamarin.Forms.BindableProperty), то есть их можно указывать в качестве целевых для привязки данных.

## <a name="bottom-and-top-tabs"></a>Нижние и верхние вкладки

Если `Tab` содержит более одного объекта `ShellContent`, вверху добавляется еще одна панель вкладок, с помощью которой осуществляется перемещение по объектам `ContentPage`:

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell"
       FlyoutBehavior="Disabled">
    <FlyoutItem>
        <Tab Title="Domestic"
             Icon="domestic.png">
            <ShellContent>
                <views:CatsPage />
            </ShellContent>
            <ShellContent>
                <views:DogsPage />
            </ShellContent>
        </Tab>
        <Tab Title="Monkeys"
             Icon="monkey.png">
            <ShellContent>
                <views:MonkeysPage />
            </ShellContent>
        </Tab>
    </FlyoutItem>
</Shell>
```

В результате создается такой макет, как показано на следующих снимках экрана:

[![Снимок экрана двухстраничного приложения оболочки с верхними и нижними вкладками для iOS и Android](tabs-images/two-page-app-top-tabs.png "Двухстраничное приложение оболочки с верхними и нижними вкладками")](tabs-images/two-page-app-top-tabs-large.png#lightbox "Двухстраничное приложение оболочки с верхними и нижними вкладками")

Также вы можете использовать операторы неявного преобразования оболочки для удаления объектов `ShellContent` и второго объекта `Tab` из предыдущего примера:

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell"
       FlyoutBehavior="Disabled">
    <FlyoutItem>
        <Tab Title="Domestic"
             Icon="domestic.png">
            <views:CatsPage />
            <views:DogsPage />
        </Tab>
        <views:MonkeysPage IconImageSource="monkey.png" />
    </FlyoutItem>
</Shell>
```

Это неявное преобразование автоматически заключает `MonkeysPage` в объект `ShellContent`, а его — в объект `Tab`. Кроме того, `CatsPage` и `DogsPage` неявным образом заключаются в объекты `ShellContent`.

## <a name="efficient-page-loading"></a>Эффективная загрузка страниц

В приложении оболочки каждый объект [`ContentPage`](xref:Xamarin.Forms.ContentPage) в объекте `ShellContent` создается во время запуска приложения, что может замедлять запуск. Но оболочка позволяет создавать страницы по требованию, реагируя на переходы. Для этого следует применить расширение разметки `DataTemplate`, чтобы преобразовать каждый `ContentPage` в [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) и сохранить полученный результат в свойстве `ShellContent.ContentTemplate`:

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell">    
    <FlyoutItem Title="Animals"
                FlyoutDisplayOptions="AsMultipleItems">
        <Tab Title="Domestic"
             Icon="paw.png">
            <ShellContent Title="Cats"
                          Icon="cat.png"
                          ContentTemplate="{DataTemplate views:CatsPage}" />
            <ShellContent Title="Dogs"
                          Icon="dog.png"
                          ContentTemplate="{DataTemplate views:DogsPage}" />
        </Tab>
        <ShellContent Title="Monkeys"
                      Icon="monkey.png"
                      ContentTemplate="{DataTemplate views:MonkeysPage}" />
    </FlyoutItem>    
</Shell>
```

Этот XAML создает и отображает `CatsPage`, то есть первый элемент содержимого, объявленный в производном объекте `Shell`. Переход между `CatsPage` и `MonkeysPage` осуществляется через нижние вкладки, и эти страницы создаются только тогда, когда пользователь переходит к ним. Преимуществом этого подхода является ускорение запуска приложения, ведь теперь страницы создаются только по запросу в ответ на действия навигации, а не сразу при запуске приложения.

## <a name="tab-appearance"></a>Внешний вид вкладок

Класс `Shell` предоставляет следующие свойства, которые определяют внешний вид вкладок:

- Присоединенное свойство `TabBarBackgroundColor` с типом `Color`определяет цвет фона для панели вкладок. Если это свойство не задано, используется значение свойства `BackgroundColor`.
- Присоединенное свойство `TabBarDisabledColor` с типом `Color`определяет цвет для отключенных вкладок на панели. Если это свойство не задано, используется значение свойства `DisabledColor`.
- Присоединенное свойство `TabBarForegroundColor` с типом `Color`определяет цвет переднего плана для панели вкладок. Если это свойство не задано, используется значение свойства `ForegroundColor`.
- Присоединенное свойство `TabBarTitleColor` с типом `Color`определяет цвет заголовков для панели вкладок. Если это свойство не задано, используется значение свойства `TitleColor`.
- Присоединенное свойство `TabBarUnselectedColor` с типом `Color`определяет цвет невыбранных вкладок на панели. Если это свойство не задано, используется значение свойства `UnselectedColor`.

Все эти свойства поддерживаются объектами [`BindableProperty`](xref:Xamarin.Forms.BindableProperty), то есть их можно указывать в качестве целевых для привязки данных.

Это означает, что ко вкладкам можно применить стили XAML. В следующем примере показан стиль XAML, который задает разные свойства для цветов вкладки:

```xaml
<Style x:Key="BaseStyle"
       TargetType="Element">
    <Setter Property="Shell.TabBarBackgroundColor"
            Value="#3498DB" />
    <Setter Property="Shell.TabBarTitleColor"
            Value="White" />
    <Setter Property="Shell.TabBarUnselectedColor"
            Value="#B4FFFFFF" />
</Style>
```

Кроме того, стиль вкладок можно задать с помощью каскадных таблиц стилей (CSS). Подробные сведения см. в статье [об особых свойствах оболочки Xamarin.Forms](~/xamarin-forms/user-interface/styles/css/index.md#xamarinforms-shell-specific-properties).

## <a name="related-links"></a>Связанные ссылки

- [Xaminals (пример)](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/Xaminals/)
- [Навигация в оболочке Xamarin.Forms](navigation.md)
- [Особые свойства оболочки Xamarin.Forms](~/xamarin-forms/user-interface/styles/css/index.md#xamarinforms-shell-specific-properties)
