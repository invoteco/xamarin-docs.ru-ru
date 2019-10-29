---
title: Метки в Xamarin. iOS
description: В этом документе описывается использование меток в Xamarin. iOS. В нем описывается создание меток программным способом и с помощью конструктора iOS.
ms.prod: xamarin
ms.assetid: 54DA1221-13E4-4D45-B263-5F22A0AC7B53
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 07/11/2017
ms.openlocfilehash: 04d33d986d76daf29fc7392206c62f77d34dd969
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73022024"
---
# <a name="labels-in-xamarinios"></a>Метки в Xamarin. iOS

Элемент управления `UILabel` используется для отображения одного и нескольких строк, текста только для чтения.

## <a name="implementing-a-label"></a>Реализация метки

Новая метка создается путем создания экземпляра [`UILabel`](xref:UIKit.UILabel):

```csharp
UILabel label = new UILabel();
```

### <a name="labels-and-storyboards"></a>Метки и раскадровки

Вы также можете добавить метку в пользовательский интерфейс при использовании конструктора iOS. Выполните поиск **метки** на **панели элементов** и перетащите ее в представление:

![Метка в панели элементов](labels-images/image3.png)

На панели свойств можно настроить следующие свойства:

![Панель свойств метки](labels-images/image2.png)

- **Текстовый контекст** — обычный или с атрибутами. Обычный текст позволяет задать [атрибуты форматирования](#Formatting_Text_and_Label) для всей строки. Тексты с атрибутами позволяют задавать форматирование для различных символов или слов в строке.
- **Цвет, шрифт, выравнивание** — атрибуты форматирования, которые могут быть применены к метке.
- **Строки** — задает количество линий, которые может занимать метка. Установите значение 0, чтобы разрешить метке использовать столько строк, сколько необходимо.
- **Поведение** — можно задать значение "включено" или "выделено". Enabled установлено по умолчанию, отключенный текст будет отображаться в более светлом сером цвете. Выделенное по умолчанию отключено и позволяет перерисовать метку с выделенным состоянием, если оно выбрано пользователем.
- **Баселане и разрыв строки** —
  - Превышает показателей определяет, как будет размещаться текст, если размер шрифта отличается от указанного.
  - Разрывы строк определяют, как строка будет упакована или обрезана, если она длиннее одной строки.
- **Автосжатие** — определяет, как размер шрифта будет уменьшаться в метке при необходимости.
- **Выделение, тень, смещение** — позволяет задать цвет подсвечено и тени, а также смещение тени.

## <a name="truncating-and-wrapping"></a>Усечение и перенос

Сведения об использовании разрывов строк в iOS см. в разделе [Truncate and Wrap Text](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/labels/uilabel-truncate-wrap-text) рецепт.

<a name="Formatting_Text_and_Label"/>

## <a name="formatting-text-and-label"></a>Форматирование текста и метки

Чтобы отформатировать строку, используемую в метке, можно либо задать атрибуты форматирования для всей строки, либо использовать строки с атрибутами. В следующих примерах показано, как реализовать эти методы:

```csharp
label = new UILabel(){
                Text = "Hello, this is a string",
                Font = UIFont.FromName("Papyrus", 20f),
                TextColor = UIColor.Magenta,
                TextAlignment = UITextAlignment.Center
            };
```

```csharp
label.AttributedText = new NSAttributedString(
                "This is some formatted text",
                font: UIFont.FromName("GillSans", 16.0f),
                foregroundColor: UIColor.Blue,
                backgroundColor: UIColor.White
            );
```

Дополнительные сведения о стилизации текста с помощью `NSAttributedString` см. в статье рецепт [стиля текста](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/text_field/style_text) .

По умолчанию для `Enabled` меток задано значение true, но можно установить значение Disabled (отключено), чтобы дать пользователю указание о том, что определенный элемент управления отключен:

```csharp
label.Enabled = false;
```

При этом для метки устанавливается светло-серый цвет, как показано в следующем примере изображения на экране ограничений в iOS:

![Отключенная кнопка в iOS](labels-images/image1.png)

Можно также задать цвета выделения и тени для текста метки, чтобы получить дополнительные эффекты:

```csharp
label.Highlighted = true;
label.HighlightedTextColor = UIColor.Cyan;

label.ShadowColor = UIColor.Black;
label.ShadowOffset = new CoreGraphics.CGSize(1.0f, 1.0f);
```

Который отображает текст следующим образом:

![Выделение и затемнение набора текста](labels-images/image4.png)

Дополнительные сведения об изменении шрифта Уилабел см. в разделе изменение рецепта [шрифта](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/labels/change_the_font) .
