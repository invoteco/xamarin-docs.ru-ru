---
title: Элемент управления Menu watchOS (Force Touch) в Xamarin
description: В этом документе описывается использование жеста принудительного касания watchOS в Xamarin. В нем рассматривается реакция на принудительное касание, Добавление меню и изменение пунктов меню.
ms.prod: xamarin
ms.assetid: 5A7F83FB-9BC4-4812-92C5-CEC8DAE8211E
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: f848433a5a668e247142aa4f47374c2c6531b55d
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032697"
---
# <a name="watchos-menu-control-force-touch-in-xamarin"></a>Элемент управления Menu watchOS (Force Touch) в Xamarin

Контрольный комплект предоставляет Force Touch жест, который запускает меню при реализации на экране контрольных приложений.

![](menu-images/menu.png "Apple Watch showing a menu")
<!-- watch image courtesy of http://infinitapps.com/bezel/ -->

## <a name="responding-to-force-touch"></a>Реагирование на Force Touch

Если `Menu` был реализован для контроллера интерфейса, то при выполнении пользователем Force Touch появится меню. Если меню не было реализовано, экран кратко анимируется без каких-либо других действий.

Принудительное касание не связано с каким бы то ни было определенным элементом на экране; к контроллеру интерфейса можно присоединить только одно меню, которое будет отображаться независимо от того, где на экране появляется Force Touch нажатии.

Можно представить между одним и четырьмя параметрами меню.

## <a name="adding-a-menu"></a>Добавление меню

`Menu` необходимо добавить в `InterfaceController` раскадровки во время разработки. При перетаскивании элемента управления Menu на контроллер интерфейса отсутствует визуальное указание на просмотр раскадровки, но **меню** появляется на панели **структуры документа** :

![](menu-images/menu-action.png "Editing a menu at design time")

В элемент управления Menu можно добавить до четырех пунктов меню. Их можно настроить на панели **свойств** . Можно задать следующие атрибуты:

- Заголовок и
- Пользовательский образ или
- Образ системы: принятие, добавление, блокировка, отклонение, информация, возможно, больше, Выкл., пауза, воспроизведение, повторение, возобновление, поделиться, передвинуть, докладчик, корзина.

Создайте `Action`, выбрав раздел **события** на панели **свойства** и введя имя метода действия. Разделяемый метод будет создан в коде, который можно реализовать в классе контроллера интерфейса следующим образом:

```csharp
partial void MenuItemTapped ()
{
    Console.WriteLine ("A menu item was tapped.");
}
```

### <a name="custom-images"></a>Пользовательские образы

Как и в изображениях вкладок в iOS, для изображений пунктов меню требуется непрозрачный шаблон с альфа-каналом, позволяющий отображать фон.

Для лучшей производительности следует добавить изображения, используемые для меню, в проект Watch App (а не проект расширения приложения).

## <a name="changing-the-menu-items"></a>Изменение пунктов меню

<!--
### Design Time Items

Menu items added the storyboard can be shown and hidden programmatically.
-->

### <a name="adding-at-runtime"></a>Добавление во время выполнения

Невозможно добавить `Menu` в контроллер интерфейса во время выполнения, хотя коллекцию `MenuItem`s *можно* изменить программным способом.
Используйте метод `AddMenuItem`, как показано ниже.

```csharp
AddMenuItem (WKMenuItemIcon.Accept, "Yes", new ObjCRuntime.Selector ("tapped"));
```

API комплекта отслеживания Xamarin. iOS в настоящее время требует `selector` для метода `AdMenuItem`, который должен быть объявлен следующим образом:

```csharp
[Export("tapped")]
void MenuItemTapped ()
{
    Console.WriteLine ("The dynamically added 'Yes' menu item was tapped.");
}
```

### <a name="removing-at-runtime"></a>Удаление во время выполнения

Чтобы удалить все *программно добавленные* элементы меню, можно вызвать метод `ClearAllMenuItems`.

Не удается очистить элементы меню, настроенные в раскадровке.

## <a name="related-links"></a>Связанные ссылки

- [Ватчкиткаталог (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [Документ меню Apple](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/Menus.html)
