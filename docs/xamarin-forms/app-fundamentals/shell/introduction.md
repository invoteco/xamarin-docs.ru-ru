---
title: Введение в оболочку Xamarin.Forms
description: Оболочка Xamarin.Forms предоставляет набор основных возможностей, которые требуются для большинства приложений, таких как стандартизированная навигация для пользователя, схема навигации на основе URI и интегрированный обработчик поиска.
ms.prod: xamarin
ms.assetid: 4604DCB5-83DA-458A-8B02-6508A740BE0E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: f6662f20485c6671edcb5a1654569cdd8498607e
ms.sourcegitcommit: 0596004d4a0e599c1da1ddd75a6ac928f21191c2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/22/2019
ms.locfileid: "66005211"
---
# <a name="xamarinforms-shell"></a>Оболочка Xamarin.Forms

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/Xaminals/)

Оболочка Xamarin.Forms упрощает разработку мобильных приложений, предоставляя основные возможности, которые необходимы для большинства мобильных приложений, такие как:

- единое место для описания визуальной структуры приложения;
- единый пользовательский интерфейс навигации;
- схема навигации на основе URI, которая позволяет переходить на любую страницу в приложении;
- Обработчик интегрированного поиска.

Кроме того, приложения оболочки получают более высокую скорость отрисовки и снижение потребления памяти.

> [!IMPORTANT]
> Оболочка Xamarin.Forms доступна только в iOS и Android. Оболочку можно внедрить в уже существующие приложения для iOS и Android, сразу же получив преимущества улучшения навигации, производительности и расширяемости.

## <a name="shell-navigation-experience"></a>Возможности навигации оболочки

Оболочка предоставляет специализированный пользовательский интерфейс навигации на основе всплывающих элементов и вкладок. Верхний уровень навигации в приложении оболочки — всплывающее окно:

[![Снимок экрана для всплывающего элемента оболочки для iOS и Android](introduction-images/flyout.png "Всплывающий элемент оболочки")](introduction-images/flyout-large.png#lightbox "Всплывающий элемент оболочки")

Выбор элемента приводит к отображению нижней вкладки, которая представляет выбранный элемент:

[![Снимок экрана с нижними вкладками оболочки для iOS и Android](introduction-images/monkeys.png "Нижние вкладки оболочки")](introduction-images/monkeys-large.png#lightbox "Нижние вкладки оболочки")

> [!NOTE]
> Если всплывающий элемент не открыт, вкладки на нижней панели считаются верхним уровнем навигации в приложении.

Каждая вкладка отображает [`ContentPage`](xref:Xamarin.Forms.ContentPage). Но если вкладка содержит более одной страницы, перемещение по страницам осуществляется с помощью верхней панели вкладок.

[![Снимок экрана с верхними вкладками оболочки для iOS и Android](introduction-images/cats.png "Верхние вкладки оболочки")](introduction-images/cats-large.png#lightbox "Верхние вкладки оболочки")

В каждой вкладке можно перейти на дополнительные объекты [`ContentPage`](xref:Xamarin.Forms.ContentPage):

[![Снимок экрана со страницами навигации оболочки для iOS и Android](introduction-images/cat-details.png "Навигация оболочки для приложения")](introduction-images/cat-details-large.png#lightbox "Навигация оболочки для приложения")

## <a name="subclassing-the-shell-class"></a>Создание производного класса Shell

Производный объект `Shell` описывает визуальную иерархию приложения оболочки. Он содержит три основных объекта иерархии:

- `FlyoutItem` представляет один или несколько элементов всплывающего элемента. Каждый объект `FlyoutItem` является дочерним для объекта `Shell`.
- `Tab` представляет сгруппированное содержимое с навигацией по нижним вкладкам. Каждый объект `Tab` является дочерним для объекта `FlyoutItem`.
- `ShellContent` представляет объекты `ContentPage` в приложении. Каждый объект `ShellContent` является дочерним для объекта `Tab`. Если `Tab` содержит более одного объекта `ShellContent`, перемещение по ним осуществляется с помощью верхней панели вкладок.

Ни один из этих объектов не представляет какую-либо часть пользовательского интерфейса, они служат лишь для организации визуальной структуры приложения. На основе этих объектов оболочка создает пользовательский интерфейс навигации по содержимому.

> [!NOTE]
> Класс `FlyoutItem` является псевдонимом для класса `ShellItem`, а класс `Tab` — для класса `ShellSection`. Эти псевдонимы нужны, чтобы упростить работу с API.

Ниже представлен простой пример XAML для производного объекта `Shell`:

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell">
    ...
    <FlyoutItem Title="Animals"
                FlyoutDisplayOptions="AsMultipleItems">
        <Tab Title="Domestic"
             Icon="paw.png">
            <ShellContent Title="Cats"
                          Icon="cat.png">
                <views:CatsPage />
            </ShellContent>
            <ShellContent Title="Dogs"
                          Icon="dog.png">
                <views:DogsPage />
            </ShellContent>
        </Tab>
        <ShellContent Title="Monkeys"
                      Icon="monkey.png">
            <views:MonkeysPage />
        </ShellContent>
        <ShellContent Title="Elephants"
                      Icon="elephant.png">  
            <views:ElephantsPage />
        </ShellContent>
        <ShellContent Title="Bears"
                      Icon="bear.png">
            <views:BearsPage />
        </ShellContent>
    </FlyoutItem>
    ...
</Shell>
```

Этот XAML при выполнении отображает `CatsPage`, то есть первый элемент содержимого, объявленный в производном объекте `Shell`:

[![Снимок экрана с приложением оболочки для iOS и Android](introduction-images/cats.png "Приложение оболочки")](introduction-images/cats-large.png#lightbox "Приложение оболочки")

Если нажать значок из трех полосок или провести пальцем от левого края, отобразится всплывающий элемент:

[![Снимок экрана для всплывающего элемента оболочки для iOS и Android](introduction-images/flyout-reduced.png "Всплывающий элемент оболочки")](introduction-images/flyout-reduced-large.png#lightbox "Всплывающий элемент оболочки")

> [!IMPORTANT]
> В приложении оболочки каждый [`ContentPage`](xref:Xamarin.Forms.ContentPage), который является дочерним элементом объекта `ShellContent`, создается во время запуска приложения. Такой метод добавления объектов `ShellContent` приводит к созданию дополнительных страниц во время запуска приложения, что может замедлять запуск. Но оболочка может создавать страницы и по требованию, реагируя на переходы. Дополнительные сведения см. в разделе [об эффективности загрузки страниц](tabs.md#efficient-page-loading) в [руководстве по вкладкам оболочки Xamarin.Forms](tabs.md).

## <a name="bootstrapping-a-shell-application"></a>Начальная загрузка приложения оболочки

Приложение оболочки загружается путем присваивания свойству [`MainPage`](xref:Xamarin.Forms.Application.MainPage) класса `App` нового производного объекта `Shell`:

```csharp
namespace Xaminals
{
    public partial class App : Application
    {
        public App()
        {
            InitializeComponent();
            MainPage = new AppShell();
        }
        ...
    }
}
```

В нашем примере класс `AppShell` представляет собой XAML-файл, который наследуется от класса `Shell` и описывает визуальную иерархию приложения.

## <a name="shell-appearance"></a>Внешний вид оболочки

Класс `Shell` определяет следующие свойства, которые определяют внешний вид приложения оболочки:

- Присоединенное свойство `BackgroundColor` с типом `Color` определяет цвет фона для хрома оболочки. Этот цвет не применяется под содержимым оболочки.
- Присоединенное свойство `DisabledColor` с типом `Color` определяет цвет затененного текста и отключенных значков.
- Присоединенное свойство `ForegroundColor` с типом `Color` определяет цвет теней для текста и значков.
- Присоединенное свойство `TitleColor` с типом `Color` определяет цвет для заголовка активной страницы.
- Присоединенное свойство `UnselectedColor` с типом `Color` определяет цвет невыделенного текста и значков для хрома оболочки.

Все эти свойства поддерживаются объектами [`BindableProperty`](xref:Xamarin.Forms.BindableProperty), то есть их можно указывать в качестве целевых для привязки данных.

Кроме того, эти свойства можно задать с помощью каскадных таблиц стилей (CSS). Подробные сведения см. в статье [об особых свойствах оболочки Xamarin.Forms](~/xamarin-forms/user-interface/styles/css/index.md#xamarinforms-shell-specific-properties).

## <a name="shell-content-layout"></a>Макет содержимого оболочки

Класс `Shell` определяет следующие свойства, которые формируют макет содержимого для приложения оболочки:

- Присоединенное свойство `NavBarIsVisible` с типом `boolean`определяет, будет ли видна панель навигации при отображении страницы. Это свойство задается отдельно для каждой страницы, а по умолчанию используется значение `true`.
- Присоединенное свойство `TabBarIsVisible` с типом `bool`определяет, будет ли видна панель вкладок при отображении страницы. Это свойство задается отдельно для каждой страницы, а по умолчанию используется значение `true`.
- Присоединенное свойство `TitleView` с типом `View`определяет `TitleView` для страницы. Это свойство задается отдельно для каждой страницы.

Все эти свойства поддерживаются объектами [`BindableProperty`](xref:Xamarin.Forms.BindableProperty), то есть их можно указывать в качестве целевых для привязки данных.

## <a name="related-links"></a>Связанные ссылки

- [Xaminals (пример)](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/Xaminals/)
- [Особые свойства оболочки Xamarin.Forms](~/xamarin-forms/user-interface/styles/css/index.md#xamarinforms-shell-specific-properties)
