---
title: Введение в iOS 6
description: В этом документе содержатся ссылки на руководства, в которых описываются функции iOS 6. Обсуждаются представления коллекций, PassKit, социальные платформы и изменения в StoreKit.
ms.prod: xamarin
ms.assetid: 242DA7E3-8FD8-5F20-285D-603259CA622D
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/19/2017
ms.openlocfilehash: d30789a33eb8023f4ce77b4a7c2445fabca0f2db
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031839"
---
# <a name="introduction-to-ios-6"></a>Введение в iOS 6

_в iOS 6 реализовано множество новых технологий для разработки приложений, которые Xamarin. iOS 6 предоставляет C# разработчикам._

[![](images/ios6-large.jpg "The iOS 6 logo")](images/ios6-large.jpg#lightbox)

В iOS 6 и Xamarin. iOS 6 разработчики теперь имеют множество возможностей при их реализации для создания приложений iOS, включая те, которые нацелены на iPhone 5.
В этом документе перечислены некоторые из наиболее интересных новых функций и ссылки на статьи по каждому разделу. Кроме того, он касается нескольких изменений, которые будут важны, когда разработчики переходят на iOS 6 и новое разрешение iPhone 5.

## <a name="introduction-to-collection-viewsiosuser-interfacecontrolsuicollectionviewmd"></a>[Общие сведения о представлениях коллекций](~/ios/user-interface/controls/uicollectionview.md)

Представления коллекций позволяют отображать содержимое с помощью произвольных макетов. Они позволяют легко создавать макеты, основанные на сетке, при поддержке пользовательских макетов. Дополнительные сведения см. в разделе [Введение в представления](~/ios/user-interface/controls/uicollectionview.md) [](~/ios/user-interface/controls/uicollectionview.md)коллекции.

## <a name="introduction-to-passkitiosplatformpasskitmd"></a>[Введение в PassKit](~/ios/platform/passkit.md)

Платформа PassKit позволяет приложениям взаимодействовать с цифровыми проходами, управляемыми в приложении расчетной книжки. Дополнительные сведения см. в статье [Введение в проход по пакету](~/ios/platform/passkit.md).

## <a name="introduction-to-eventkitiosplatformeventkitmd"></a>[Введение в EventKit](~/ios/platform/eventkit.md)

Платформа EventKit предоставляет способ доступа к календарям, событиям календаря и данным напоминаний, которые хранятся в базе данных календаря. Доступ к календарям и событиям календаря был доступен с iOS 4, но теперь iOS 6 предоставляет доступ к данным напоминаний. Дополнительные сведения см [. в разделе](~/ios/platform/eventkit.md) [нтродуктион to EventKit](~/ios/platform/eventkit.md) Guide.

## <a name="introduction-to-the-social-frameworkiosplatformsocial-frameworkmd"></a>[Введение в социальные платформы](~/ios/platform/social-framework.md)

Социальные платформы предоставляют унифицированный API для взаимодействия с социальными сетями, включая Twitter и Facebook, а также Синавеибо для пользователей в Китае. Дополнительные сведения см. в разделе Введение в руководством по [социальной инфраструктуре](~/ios/platform/social-framework.md) .

## <a name="changes-to-storekitchanges-to-storekitmd"></a>[Изменения в StoreKit](changes-to-storekit.md)

Компания Apple предоставила две новые функции в магазине Store Kit: приобретение и скачивание содержимого iTunes или App Store из приложения, а также размещение файлов содержимого для покупок в приложении!. Дополнительные сведения см. в разделе [изменения для Store Kit](changes-to-storekit.md) Guide.

## <a name="other-changes"></a>Другие изменения

### <a name="viewwillunload-and-viewdidunload-deprecated"></a>Виеввиллунлоад и Виевдидунлоад устарели

Методы `ViewWillUnload` и `ViewDidUnload` `UIViewController` больше не вызываются в iOS 6. В предыдущих версиях iOS эти методы могли использоваться приложениями для сохранения состояния перед выгрузкой представления и кодом очистки соответственно.

Например, Visual Studio для Mac создаст метод с именем `ReleaseDesignerOutlets`, показанный ниже, который затем будет вызываться из `ViewDidUnload`:

```csharp
void ReleaseDesignerOutlets ()
{
    if (myOutlet != null) {
        myOutlet.Dispose ();
        myOutlet = null;
    }
}
```

Однако в iOS 6 больше не требуется вызывать `ReleaseDesignerOutlets`.   

Для кода очистки приложения iOS 6 должны использовать `DidReceiveMemoryWarning`. Однако код, вызывающий `Dispose`, следует использовать экономно и только для объектов, интенсивно использующих память, как показано ниже:

```csharp
if (myImageView != null){
    if (myImageView.Superview == null){
        myImageView.Dispose();
        myImageView = null;
    }
}
```

Опять же, вызов `Dispose`, как и выше, не должен требоваться редко. Как правило, большинство приложений должны удалить обработчики событий.

В случае сохранения состояния приложения могут выполнять эту действия в `ViewWillDisappear` и `ViewDidDisappear` вместо `ViewWillUnload`.

### <a name="iphone-5-resolution"></a>Разрешение iPhone 5

устройства iPhone 5 имеют разрешение 640x1136. Приложения, нацеленные на предыдущие версии iOS, будут отображаться леттербоксед при запуске на iPhone 5, как показано ниже:

 [![](images/01-letterboxed.png "Applications that targeted previous versions of iOS will appear letterboxed when run on an iPhone 5")](images/01-letterboxed.png#lightbox)

Чтобы приложение отображалось на устройстве iPhone 5 в полноэкранном режиме, просто добавьте образ с именем `Default-568h@2x.png` с разрешением 640x1136. На следующем снимке экрана показано приложение, запущенное после включения этого образа:

 [![](images/02-fullscreen.png "This screenshot shows the application running after this image has been included")](images/02-fullscreen.png#lightbox)

### <a name="subclassing-uinavigationbar"></a>Подклассировать Уинавигатионбар

В iOS 6 `UINavigationBar` могут быть подклассами. Это позволяет дополнительно контролировать внешний вид `UINavigationBar`. Например, приложения могут подклассировать для добавления подпредставлений, анимировать эти представления и изменить границы `UINavigationBar`.

В приведенном ниже коде показан пример подкласса `UINavigationBar`, добавляющего `UIImageView`:

```csharp
public class CustomNavBar : UINavigationBar
{
    UIImageView iv;
    public CustomNavBar (IntPtr h) : base(h)
    {
        iv = new UIImageView (UIImage.FromFile ("monkey.png"));
        iv.Frame = new CGRect (75, 0, 30, 39);
    }
    public override void Draw (RectangleF rect)
    {
        base.Draw (rect);
        TintColor = UIColor.Purple;
        AddSubview (iv);
    }
}
```

Чтобы добавить в `UINavigationController`подкласс `UINavigationBar`, используйте конструктор `UINavigationController`, который принимает тип `UINavigationBar` и `UIToolbar`, как показано ниже:

```csharp
navController = new UINavigationController (typeof(CustomNavBar), typeof(UIToolbar));
```

Использование этого подкласса `UINavigationBar` приводит к отображению представления изображения, как показано на следующем снимке экрана:

 [![](images/03-navbar.png "Using this UINavigationBar subclass results in the image view being displayed as shown in this screenshot")](images/03-navbar.png#lightbox)

### <a name="interface-orientation"></a>Ориентация интерфейса

До появления приложений iOS 6 можно было переопределить `ShouldAutorotateToInterfaceOrientation`, возвращая значение true для любой ориентации, поддерживаемой определенным контроллером. Например, следующий код будет использоваться для поддержки только книжной ориентации:

```csharp
public override bool ShouldAutorotateToInterfaceOrientation (UIInterfaceOrientation toInterfaceOrientation)
    {
        return (toInterfaceOrientation == UIInterfaceOrientation.Portrait);
    }
```

В `ShouldAutorotateToInterfaceOrientation` iOS 6 не рекомендуется использовать.
Вместо этого приложения могут переопределять `GetSupportedInterfaceOrientations` на контроллере корневого представления, как показано ниже:

```csharp
public override UIInterfaceOrientationMask GetSupportedInterfaceOrientations ()
    {
        return UIInterfaceOrientationMask.Portrait;
    }
```

На iPad это значение по умолчанию равно всем четырем ориентация, если `GetSupportedInterfaceOrientation` не реализована. На iPhone и iPod touch по умолчанию заданы все ориентации, кроме `PortraitUpsideDown`.
