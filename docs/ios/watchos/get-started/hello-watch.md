---
title: Привет, watchOS — пошаговое руководство
description: В этом документе представлено пошаговое руководство по созданию простого приложения watchOS с помощью Xamarin. В нем описывается работа в Visual Studio и Visual Studio для Mac, работа с раскадровкой и реагирование на события в коде.
ms.prod: xamarin
ms.assetid: AD1DA488-51AB-420A-A0B7-3AE69A964A40
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 12/14/2016
ms.openlocfilehash: 2d8b48892a5a1106b03778ac30eca4b18f049f4d
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725327"
---
# <a name="hello-watchos--walkthrough"></a>Привет, watchOS — пошаговое руководство

После создания решения, описанного в разделе [Установка и установка](~/ios/watchos/get-started/installation.md), у вас будет три проекта:

- Родительское приложение iOS, которое используется для установки или других административных задач на устройстве. (Другие типы модулей iOS часто называют "контейнерным приложением".) С помощью контрольных приложений пользователи могут **начать работу с** приложением Watch, не запуская родительское приложение.
- Расширение Watch, содержащее программный код для приложения Watch; перетаскивани
- Приложение Watch, которое содержит ресурсы Storyboard и Image, отображаемые в контрольных значениях.

Проверьте [правильность ссылок](~/ios/watchos/get-started/project-references.md): у родительского приложения есть ссылка на расширение и что у расширения есть ссылка на приложение Watch.

Убедитесь, что идентификаторы пакета соответствуют соглашению \*. ватчкитекстенсион \*. ватчкитапп, а файл info. plist вашего расширения имеет значение идентификатора пакета **WKApp** , которому задан идентификатор пакета приложения Watch.

Теперь вы можете запустить приложение наблюдения, но так как файл раскадровки в приложении для просмотра контрольных значений пуст, вы не сможете понять.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio для Mac](#tab/macos)

![](hello-watch-images/projectstructure.png "The Solution Explorer")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![](hello-watch-images/vs-projectstructure.png "The Solution Explorer")

-----

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio для Mac](#tab/macos)

Дважды щелкните интерфейс. Раскадровка в приложении Watch, чтобы запустить конструктор Xamarin iOS (если вы используете компьютер Mac, можно также щелкнуть правой кнопкой мыши и **Открыть с помощью > Xcode Interface Builder**).

1. Убедитесь, что **панели элементов** и **свойства** отображаются.
1. Щелкните, чтобы выбрать контроллер интерфейса,
1. Задайте для идентификатора и заголовка контроллера интерфейса значение **интерфацеконтроллер** и **Hi Watch**,
1. Убедитесь, что для **класса** задано значение **интерфацеконтроллер** .

    ![](hello-watch-images/interfacecontrollerattributes.png "Set the Identifier and Title of the Interface Controller to interfaceController and Hi Watch")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Дважды щелкните интерфейс. Раскадровка в приложении Watch, чтобы изменить его с помощью конструктора Xamarin iOS в Visual Studio:

1. Откройте панель свойств.
1. Измените класс на **интерфацеконтроллер**.
1. Щелкните контроллер интерфейса; перетаскивани
1. Задайте для идентификатора и заголовка контроллера интерфейса значение **интерфацеконтроллер** и **Hi Watch**.

    ![](hello-watch-images/vs-interfacecontrollerattributes.png "Set the Identifier and Title of the Interface Controller to interfaceController and Hi Watch")

-----

Создайте пользовательский интерфейс:

1. Из панели **элементов** ,
1. Перетащите **кнопку** и **метку** на сцену и
1. Задайте текст и атрибуты элементов управления, как показано ниже.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio для Mac](#tab/macos)

![](hello-watch-images/draganddrop.png "Set the text and attributes of the controls as shown")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![](hello-watch-images/vs-draganddrop.png "Set the text and attributes of the controls as shown")

-----

1. Задайте **имя** на панели **свойств** для каждого элемента управления. В этом примере мы использовали `myButton` и `myLabel`.

1. Нажмите кнопку раскадровки и перейдите в список **событий** панели **свойств** , а затем

1. Создайте новое **действие** , введя `OnButtonPress` и нажав клавишу **Ввод**.
  Действие появится в списке, и разделяемый метод будет автоматически создан в C#.

![](hello-watch-images/buttonaction.png "The OnButtonPress Action added to a button")

После сохранения раскадровки **InterfaceController.Designer.CS** обновляется с именами и действиями элементов управления. Если открыть этот файл после его обновления, можно увидеть, как `RegisterAttribute` соответствует контроллеру и каким образом элементы управления ИП соответствуют переменным C# экземпляра, помеченным `OutletAttribute`, и как действия сопоставляются с разделяемыми методами, помеченными `ActionAttribute`:

```csharp
// WARNING
//
// This file has been generated automatically by Visual Studio for Mac from the outlets and
// actions declared in your storyboard file.
// Manual changes to this file will not be maintained.
//
[Register ("InterfaceController")]
partial class InterfaceController
{
    [Outlet]
    [GeneratedCode ("iOS Designer", "1.0")]
    WatchKit.WKInterfaceButton myButton { get; set; }

    [Outlet]
    [GeneratedCode ("iOS Designer", "1.0")]
    WatchKit.WKInterfaceLabel myLabel { get; set; }

    [Action ("OnButtonPress:")]
    [GeneratedCode ("iOS Designer", "1.0")]
    partial void OnButtonPress (WatchKit.WKInterfaceButton sender);

    void ReleaseDesignerOutlets ()
    {
        if (myButton != null) {
            myButton.Dispose ();
            myButton = null;
        }
        if (myLabel != null) {
            myLabel.Dispose ();
            myLabel = null;
        }
    }
}
```

Теперь откройте **InterfaceController.CS** (*не* InterfaceController.Designer.cs) и добавьте следующий код:

```csharp
int clickCount = 0;
partial void OnButtonPress (WatchKit.WKInterfaceButton sender)
{
  var msg = String.Format("Clicked {0} times", ++clickCount);
  myLabel.SetText(msg);
}
```

Этот код должен быть довольно прозрачным: переменная экземпляра `clickCount` увеличивается при каждом вызове функции `OnButtonPress`. Текст `myLabel` изменяется в соответствии с этим счетчиком; `myLabel`, само собой, является именем одного из розеток, созданных в XCode. Функция `partial` является реализацией функции, связанной с именем указанного действия.

Если он еще не является запускаемым проектом,

1. Щелкните проект расширения контрольных значений правой кнопкой мыши и выберите пункт **Назначить запускаемым проектом**.

1. Задайте в качестве цели развертывания образ симулятора, совместимый с набором (например, iPhone 6 iOS 8,2).

1. Нажмите кнопку " **Отладка** ", чтобы активировать запуск сборки и симулятора.

    [![](hello-watch-images/readytodebug-sml.png "The Visual Studio interface elements")](hello-watch-images/readytodebug.png#lightbox)

При запуске симулятора нажмите кнопку, чтобы увеличить метку.
Поздравляем, у вас есть приложение для просмотра контрольных значений!

![](hello-watch-images/running.png "The app running in the Simulator")

## <a name="related-links"></a>Связанные ссылки

- [Настройка и установка](~/ios/watchos/get-started/installation.md)
- [Первое видео о приложении](https://blog.xamarin.com/your-first-watch-kit-app/)
