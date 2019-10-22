---
title: Рефрешвиев Xamarin. Forms
description: Рефрешвиев Xamarin. Forms — это контейнерный элемент управления, который предоставляет функции обновления для прокручиваемого содержимого.
ms.prod: xamarin
ms.assetId: 58DBD23B-ADB9-40DA-B331-4DDB6E698990
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/19/2019
ms.openlocfilehash: b53c58a5e859bf7752855c3954666a062261599d
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/21/2019
ms.locfileid: "72697743"
---
# <a name="xamarinforms-refreshview"></a>Рефрешвиев Xamarin. Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-refreshview/)

@No__t_0 — это контейнерный элемент управления, предоставляющий функции обновления для прокручиваемого содержимого. Таким образом, дочерний элемент `RefreshView` должен быть прокручиваемым элементом управления, таким как [`ScrollView`](xref:Xamarin.Forms.ScrollView), [`CollectionView`](xref:Xamarin.Forms.CollectionView)или [`ListView`](xref:Xamarin.Forms.ListView).

`RefreshView` определяет следующие свойства:

- `Command`, типа `ICommand`, который выполняется при активации обновления.
- `CommandParameter` с типом `object`, который передается как параметр в `Command`.
- `IsRefreshing`, типа `bool`, который указывает текущее состояние `RefreshView`.
- `RefreshColor`, тип `Color`, цвет круга хода выполнения, отображаемый во время обновления.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами, что означает, что они могут быть целевыми объектами привязки данных и стилями.

> [!NOTE]
> В универсальная платформа Windows направление извлечения `RefreshView` можно задать с учетом конкретной платформы. Дополнительные сведения см. в статье [направление извлечения рефрешвиев](~/xamarin-forms/platform/windows/refreshview-pulldirection.md).

## <a name="create-a-refreshview"></a>Создание Рефрешвиев

В следующем примере показано, как создать экземпляр `RefreshView` в XAML:

```xaml
<RefreshView IsRefreshing="{Binding IsRefreshing}"
             Command="{Binding RefreshCommand}">
    <ScrollView>
        <FlexLayout Direction="Row"
                    Wrap="Wrap"
                    AlignItems="Center"
                    AlignContent="Center"
                    BindableLayout.ItemsSource="{Binding Items}"
                    BindableLayout.ItemTemplate="{StaticResource ColorItemTemplate}" />
    </ScrollView>
</RefreshView>
```

@No__t_0 также можно создать в коде:

```csharp
RefreshView refreshView = new RefreshView();
ICommand refreshCommand = new Command(() =>
{
    // IsRefreshing is true
    // Refresh data here
    refreshView.IsRefreshing = false;
});
refreshView.Command = refreshCommand;

ScrollView scrollView = new ScrollView();
FlexLayout flexLayout = new FlexLayout { ... };
scrollView.Content = flexLayout;
refreshView.Content = scrollView;
```

В этом примере `RefreshView` предоставляет функции обновления для [`ScrollView`](xref:Xamarin.Forms.ScrollView) , дочерним элементом которого является [`FlexLayout`](xref:Xamarin.Forms.FlexLayout). @No__t_0 использует связываемый макет для создания его содержимого путем привязки к коллекции элементов и задает внешний вид каждого элемента с [`DataTemplate`](xref:Xamarin.Forms.DataTemplate). Дополнительные сведения о макетах с возможностью привязки см. [в разделе макеты с возможностью привязки в Xamarin. Forms](~/xamarin-forms/user-interface/layouts/bindable-layouts.md).

Значение свойства `RefreshView.IsRefreshing` указывает текущее состояние `RefreshView`. При активации обновления пользователем это свойство автоматически переходит в `true`. После завершения обновления следует сбросить свойство на `false`.

Когда пользователь инициирует обновление, выполняется `ICommand`, определяемое свойством `Command`, которое должно обновлять отображаемые элементы. Во время обновления отображается визуализация обновления, которая состоит из анимированного круга хода выполнения:

[![Снимок экрана Рефрешвиев обновления данных в iOS и Android](refreshview-images/default-progress-circle.png "Рефрешвиев обновление данных")](refreshview-images/default-progress-circle-large.png#lightbox "Рефрешвиев обновление данных")

> [!NOTE]
> Ручная установка свойства `IsRefreshing` на `true` вызовет визуализацию обновления и выполнит `ICommand`, определяемую свойством `Command`.

## <a name="refreshview-appearance"></a>Внешний вид Рефрешвиев

Помимо свойств, которые `RefreshView` наследуют от класса [`VisualElement`](xref:Xamarin.Forms.VisualElement) , `RefreshView` также определяет свойство `RefreshColor`. Это свойство можно задать для определения цвета круга хода выполнения, отображаемого во время обновления:

```xaml
<RefreshView RefreshColor="Teal"
             ... />
```

На следующем снимке экрана показан `RefreshView` с набором свойств `RefreshColor`.

[![Снимок экрана Рефрешвиев с синей окружностью хода выполнения на iOS и Android](refreshview-images/teal-progress-circle.png "Рефрешвиев с синей окружностью хода выполнения")](refreshview-images/teal-progress-circle-large.png#lightbox "Рефрешвиев с синей окружностью хода выполнения")

Кроме того, свойству `BackgroundColor` может быть присвоено значение [`Color`](xref:Xamarin.Forms.Color) , представляющее цвет фона круга хода выполнения.

> [!NOTE]
> В iOS свойство `BackgroundColor` задает цвет фона `UIView`, который содержит круг хода выполнения.

## <a name="disable-a-refreshview"></a>Отключение Рефрешвиев

Приложение может ввести состояние, при котором операция Pull для обновления не является допустимой операцией. В таких случаях `RefreshView` можно отключить, задав свойству `IsEnabled` значение `false`. Это предотвратит возможность запуска запроса Pull для обновления пользователями.

Кроме того, при определении свойства `Command` можно указать `CanExecute` делегата `ICommand`, чтобы включить или отключить команду.

## <a name="related-links"></a>Связанные ссылки

- [Рефрешвиев (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-refreshview/)
- [Связываемые макеты в Xamarin. Forms](~/xamarin-forms/user-interface/layouts/bindable-layouts.md)
- [Рефрешвиев направление извлечения для конкретной платформы](~/xamarin-forms/platform/windows/refreshview-pulldirection.md)
