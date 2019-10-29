---
title: Кнопки в Xamarin. iOS
description: Класс Уибуттон используется для представления различных стилей кнопок на экранах iOS. В этом руководством представлены различные варианты работы с кнопками в iOS.
ms.prod: xamarin
ms.assetid: 304229E5-8FA8-41BD-8563-D19E1D2A0296
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 07/11/2018
ms.openlocfilehash: a8dfd267fe9f5f838927fc216d53c2475398ed16
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73022112"
---
# <a name="buttons-in-xamarinios"></a>Кнопки в Xamarin. iOS

В iOS класс `UIButton` представляет элемент управления Button.

Свойства кнопки можно изменить программно или с помощью **панель свойств** конструктора iOS:

![Панель свойств конструктора iOS](buttons-images/properties.png "Панель свойств конструктора iOS")

## <a name="creating-a-button-programmatically"></a>Создание кнопки программным способом

`UIButton` можно создать только с помощью нескольких строк кода.

- Создайте экземпляр кнопки и укажите ее тип:

  ```csharp
  UIButton myButton = new UIButton(UIButtonType.System);
  ```

  Тип кнопки задается `UIButtonType`:

  - `UIButtonType.System` — кнопка общего назначения
  - `UIButtonType.DetailDisclosure` — указывает доступность подробных сведений, обычно о конкретном элементе в таблице.
  - `UIButtonType.InfoDark` — указывает доступность сведений о конфигурации; темно-цветной
  - `UIButtonType.InfoLight` — указывает доступность сведений о конфигурации; светло-цветной
  - `UIButtonType..AddContact` — указывает, что можно добавить контакт
  - Настраиваемая кнопка `UIButtonType.Custom`

  Дополнительные сведения о различных типах кнопок см. в следующих статьях:
  
  - Раздел ["пользовательские типы кнопок"](#custom-button-types) этого документа
  - Рецепт [типов кнопок](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/buttons/create_different_types_of_buttons)
  - [Рекомендации по работе с человеческим интерфейсом iOS](https://developer.apple.com/design/human-interface-guidelines/ios/controls/buttons/)в Apple.

- Определите размер и расположение кнопки:

  ```csharp
  myButton.Frame = new CGRect(25, 25, 300, 150);
  ```

- Задайте текст кнопки. Используйте метод `SetTitle`, для которого требуется текст и значение `UIControlState`:

  ```csharp
  myButton.SetTitle("Hello, World!", UIControlState.Normal);
  ```

  Дополнительные сведения о оформлении кнопки и задании ее текста см. в следующих источниках:

  - Стиль раздела для [кнопки](#styling-a-button) в этом документе
  - Рецепт [ввода для кнопки Set](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/buttons/set_button_text) .

## <a name="handling-a-button-tap"></a>Обработка касания кнопки

Чтобы ответить на касание кнопки, укажите обработчик для события `TouchUpInside` кнопки:

```csharp
button.TouchUpInside += (sender, e) => {
    DoSomething();
};
```

> [!NOTE]
> `TouchUpInside` не является единственным доступным событием кнопки. `UIButton` является дочерним классом `UIControl`, который определяет [множество различных событий](xref:UIKit.UIControlEvent).

### <a name="using-the-ios-designer-to-specify-button-event-handlers"></a>Использование конструктора iOS для задания обработчиков событий кнопок

Используйте вкладку **события** **панель свойств** , чтобы указать обработчики событий для различных событий кнопки.

Для соответствующего события либо введите имя нового обработчика событий, либо выберите его из списка. Это приведет к созданию обработчика событий в коде для контроллера представления кнопки.

![Вкладка "события" Панель свойств](buttons-images/image1.png "Вкладка "события" Панель свойств")

## <a name="styling-a-button"></a>Стилизация кнопки

элементы управления `UIButton` могут существовать в разных состояниях, каждое из которых задается значением `UIControlState` — `Normal`, `Disabled`, `Focused`, `Highlighted`и т. д. Каждому состоянию можно присвоить уникальный стиль, заданный программно или с помощью конструктора iOS.

> [!NOTE]
> Полный список всех `UIControlState` значений см. в [`UIKit.UIControlState enumeration`](xref:UIKit.UIControlState)
> по.

Например, чтобы задать цвет заголовка и цвет тени для `UIControlState.Normal`:

```csharp
button.SetTitleColor(UIColor.White, UIControlState.Normal);
button.SetTitleShadowColor(UIColor.Black, UIControlState.Normal);
```

Следующий код задает в качестве заголовка кнопки строку с атрибутом (стилизованную) для `UIControlState.Normal` и `UIControlState.Highlighted`:

```csharp
var normalAttributedTitle = new NSAttributedString(buttonTitle, foregroundColor: UIColor.Blue, strikethroughStyle: NSUnderlineStyle.Single);
myButton.SetAttributedTitle(normalAttributedTitle, UIControlState.Normal);

var highlightedAttributedTitle = new NSAttributedString(buttonTitle, foregroundColor: UIColor.Green, strikethroughStyle: NSUnderlineStyle.Thick);
myButton.SetAttributedTitle(highlightedAttributedTitle, UIControlState.Highlighted);
```

## <a name="custom-button-types"></a>Типы настраиваемых кнопок

Кнопки с `UIButtonType` `Custom` не имеют стилей по умолчанию. Однако можно настроить внешний вид кнопки, задав для нее изображение в различных состояниях.

```csharp
button4.SetImage (UIImage.FromBundle ("Buttons/MagicWand.png"), UIControlState.Normal);
button4.SetImage (UIImage.FromBundle ("Buttons/MagicWand_Highlight.png"), UIControlState.Highlighted);
button4.SetImage (UIImage.FromBundle ("Buttons/MagicWand_On.png"), UIControlState.Selected);
```

В зависимости от того, касается ли пользователь кнопки, она будет отображаться как одно из следующих изображений (`UIControlState.Normal`, `UIControlState.Highlighted` и `UIControlState.Selected` состояния соответственно):

![Уиконтролстате. обычная](buttons-images/image22.png "Уиконтролстате. Обычная")
![Уиконтролстате. выделено](buttons-images/image23.png "Уиконтролстате. выделенный")
![уиконтролстате. Selected](buttons-images/image24.png "Уиконтролстате. выбрано")

Дополнительные сведения о работе с пользовательскими кнопками см. в разделе [использование изображения для](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/buttons/use_an_image_for_a_button) рецепта кнопки.
