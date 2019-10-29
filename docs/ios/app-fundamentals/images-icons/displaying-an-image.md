---
title: Отображение изображения в Xamarin. iOS
description: В этой статье описывается включение ресурса изображения в приложение Xamarin. iOS и отображение этого изображения либо с помощью C# кода, либо путем присвоения его элементу управления в конструкторе iOS.
ms.prod: xamarin
ms.assetid: 60288B12-49E3-4E87-8690-D04A5EC7A664
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 04/24/2018
ms.openlocfilehash: cda45f01dae2dc17c2517a7f013acacde7906a4b
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73004485"
---
# <a name="displaying-an-image-in-xamarinios"></a>Отображение изображения в Xamarin. iOS

_В этой статье описывается включение ресурса изображения в приложение Xamarin. iOS и отображение этого изображения либо с помощью C# кода, либо путем присвоения его элементу управления в конструкторе iOS._

## <a name="adding-and-organizing-images-in-a-xamarinios-app"></a>Добавление и упорядочение изображений в приложении Xamarin. iOS

При добавлении образа для использования в приложении Xamarin. iOS разработчик будет использовать _Каталог активов_ для поддержки каждого устройства iOS и разрешения, требуемого для приложения.

Добавленные в iOS 7 **наборы образов для каталогов активов** содержат все версии или представления образа, необходимые для поддержки различных устройств и коэффициентов масштабирования для приложения. Вместо того чтобы полагаться на имя файла ресурсов образа, **наборы образов** используют JSON-файл, чтобы указать, какой образ принадлежит устройству или разрешению. Это предпочтительный способ управления и поддержки изображений в iOS (с iOS 9 или более поздней версии).

## <a name="adding-images-to-an-asset-catalog-image-set"></a>Добавление изображений в набор изображений каталога активов

Как указано выше, **наборы изображений каталога активов** содержат все версии или представления образа, необходимые для поддержки различных устройств и коэффициентов масштабирования для приложения. Вместо того чтобы полагаться на имя файла ресурсов образа, **наборы образов** используют JSON-файл, чтобы указать, какой образ принадлежит устройству или разрешению.

Чтобы создать новый набор изображений и добавить в него изображения, выполните следующие действия.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio для Mac](#tab/macos)

1. В **Обозреватель решений**дважды щелкните файл `Assets.xcassets`, чтобы открыть его для редактирования:

    ![](displaying-an-image-images/imageset01.png "The Assets.xcassets in the Solution Explorer")
2. Щелкните правой кнопкой мыши **список активы** и выберите **создать набор изображений**:

    ![](displaying-an-image-images/imageset02.png "Adding a New Image Set")
3. Выберите новый набор изображений, и откроется редактор:

    ![](displaying-an-image-images/imageset03.png "The Image Set editor")
4. Здесь можно перетащить изображения для каждого из различных устройств и необходимых разрешений.
5. Дважды щелкните **имя** нового набора изображений в **списке ресурсов** , чтобы изменить его:![](displaying-an-image-images/imageset04.png "Изменение имени нового набора изображений")

При использовании **набора изображений** в конструкторе iOS просто выберите имя набора из раскрывающегося списка в редакторе свойств:

![](displaying-an-image-images/imageset06.png "Select an image set's name from the dropdown list")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Откройте каталог активов из **Обозреватель решений**и в левом верхнем углу нажмите кнопку **со знаком «плюс** »:

    ![](displaying-an-image-images/asset5.png "Click the Plus button")

2. Выберите **Добавить набор изображений** , и откроется редактор набора изображений для нового набора изображений. Здесь можно перетащить изображения для каждого из различных устройств и необходимых разрешений.

    ![](displaying-an-image-images/asset7.png "The image set editor")

### <a name="renaming-an-image-set"></a>Переименование набора изображений

Чтобы переименовать набор изображений, выполните следующие действия.

1. В **Обозреватель решений**дважды щелкните файл **каталога активов** , чтобы открыть его для редактирования:

    ![](displaying-an-image-images/rename01.png "The Asset Catalog in the Solution Explorer")
2. Выберите **набор изображений** для переименования:

    ![](displaying-an-image-images/rename02.png "Select the Image Set to rename")
3. В **обозревателе свойств**прокрутите вниз и выберите **имя**(в разделе **Прочие** ):

    ![](displaying-an-image-images/rename03.png "Select Name under the Misc section")
4. Введите новое **имя** **набора изображений** и сохраните изменения.

-----

При использовании **набора изображений** в коде сослаться на него по имени, вызвав метод `FromBundle` класса `UIImage`. Пример.

```csharp
MonkeyImage.Image = UIImage.FromBundle ("PurpleMonkey");
```

> [!IMPORTANT]
> Если изображения, назначенные набору изображений, отображаются неправильно, убедитесь, что используется правильное имя файла с методом `FromBundle` ( **набор изображений** , а не имя родительского **каталога активов** ). Для изображений PNG можно опустить расширение `.png`. Для других форматов изображений требуется расширение (например, `PurpleMonkey.jpg`).

### <a name="using-vector-images-in-asset-catalogs"></a>Использование векторных изображений в каталогах активов

Начиная с iOS 8 в **наборы изображений** добавлен специальный класс **vector** , позволяющий разработчику включать в кассету изображение в формате **PDF** в качестве, в том числе отдельные файлы точечных рисунков с разными разрешениями. Используя этот метод, предоставьте один векторный файл для разрешения `@1x` (в формате векторного PDF-файла), а `@2x` и `@3x` версии файла будут создаваться во время компиляции и включаться в пакет приложения.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio для Mac](#tab/macos)

![](displaying-an-image-images/imageset05.png "Vector Images in the Asset Catalogs editor")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![](displaying-an-image-images/asset8.png "Vector Images in the Asset Catalogs editor")

-----

Например, если разработчик включает файл `MonkeyIcon.pdf` в качестве вектора каталога активов с разрешением 150px x 150px, в окончательный набор приложений при его компиляции будут включены следующие ресурсы растрового изображения:

- разрешение `MonkeyIcon@1x.png` 150px x 150px.
- разрешение `MonkeyIcon@2x.png` 300 пикселей x 300 пикселей.
- разрешение `MonkeyIcon@3x.png` 450px x 450px.

При использовании векторных изображений PDF в каталогах активов следует учитывать следующее:

- Это не полная поддержка вектора, так как PDF-файл будет располагаться в растровом изображении во время компиляции и поставляются в окончательном приложении.
- Размер образа нельзя изменить после его настройки в каталоге активов. Если разработчик пытается изменить размер изображения (в коде или с помощью классов автоматической разметки и размера), изображение будет искажено так же, как и любой другой точечный рисунок.
- Каталоги активов совместимы только с iOS 7 и более поздних версий. Если приложению требуется поддержка iOS 6 или более низкого уровня, он не сможет использовать каталоги активов.

## <a name="working-with-template-images"></a>Работа с образами шаблонов

В зависимости от структуры приложения для iOS иногда разработчику нужно настроить значок или изображение в пользовательском интерфейсе в соответствии с изменениями цветовой схемы (например, в зависимости от настроек пользователя).

Чтобы легко достичь этого результата, переключите _режим рендеринга_ ресурса изображения на **образ шаблона**:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio для Mac](#tab/macos)

[![](displaying-an-image-images/templateimage01.png "The Render Mode set to Template Image")](displaying-an-image-images/templateimage01.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](displaying-an-image-images/templateimage01vs.png "The Render Mode set to Template")](displaying-an-image-images/templateimage01vs.png#lightbox)

-----

В конструкторе iOS назначьте ресурс изображения элементу управления пользовательского интерфейса, а затем установите **оттенок** для выделения изображения.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio для Mac](#tab/macos)

[![](displaying-an-image-images/templateimage03.png "Set the Tint to colorize the image")](displaying-an-image-images/templateimage03.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](displaying-an-image-images/templateimage03vs.png "Set the Tint to colorize the image")](displaying-an-image-images/templateimage03vs.png#lightbox)

-----

Кроме того, ресурс изображения и оттенок можно задать непосредственно в коде:

```csharp
MyIcon.Image = UIImage.FromBundle ("MessageIcon");
MyIcon.TintColor = UIColor.Red;
```

Чтобы полностью использовать образ шаблона из кода, выполните следующие действия.

```csharp
if (MyIcon.Image != null) {
    var mutableImage = MyIcon.Image.ImageWithRenderingMode (UIImageRenderingMode.AlwaysTemplate);
    MyIcon.Image = mutableImage;
    MyIcon.TintColor = UIColor.Red;
}
```

Поскольку свойство `RenderMode` `UIImage` доступно только для чтения, используйте метод `ImageWithRenderingMode`, чтобы создать новый экземпляр изображения с требуемым параметром режима рендеринга.

Существует три возможных параметра для `UIImage.RenderMode` с помощью перечисления `UIImageRenderingMode`.

- `AlwaysOriginal` — принудительное отображение изображения в качестве исходного файла исходного изображения без каких-либо изменений.
- `AlwaysTemplate` — принудительное отображение изображения в виде шаблона путем выделения цветом пикселов указанного цвета `Tint`.
- `Automatic` — отображает изображение в виде шаблона или исходного файла в зависимости от среды, в которой он используется. Например, если образ используется в `UIToolBar`, `UINavigationBar`, `UITabBar` или `UISegmentControl` он будет рассматриваться как шаблон.

## <a name="adding-new-assets-collections"></a>Добавление новых коллекций активов

При работе с изображениями в каталогах активов могут возникнуть ситуации, когда потребуется новая коллекция, а не все образы приложения в коллекцию `Assets.xcassets`. Например, при проектировании ресурсов по запросу.

Чтобы добавить новый каталог ресурсов в проект, выполните следующие действия.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio для Mac](#tab/macos)

1. Щелкните правой кнопкой мыши **имя проекта** в **Обозреватель решений** и выберите **добавить** > **новый файл...**
2. Выберите **iOS** > **Каталог ресурсов**, введите **имя** коллекции и нажмите кнопку **создать** :

    ![](displaying-an-image-images/asset01.png "Creating a new Asset Catalog")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. В обозреватель решений щелкните правой кнопкой мыши папку **каталоги активов** и выберите **Добавить > новый каталог ресурсов**.
2. Присвойте ему имя и нажмите кнопку **Добавить**:

    ![](displaying-an-image-images/asset1.png "Creating a new Asset Catalog")

-----

Отсюда коллекцию можно обрабатывать таким же образом, как и коллекция по умолчанию `Assets.xcassets`, автоматически включаемая в проект.

## <a name="using-images-with-controls"></a>Использование изображений с элементами управления

Кроме использования образов для поддержки приложения, iOS также использует изображения с такими типами элементов управления приложений, как панели вкладок, панели инструментов, панели навигации, таблицы и кнопки. Простым способом создания изображения на элементе управления является назначение `UIImage` экземпляра свойству `Image` элемента управления.

### <a name="frombundle"></a>фромбундле

Вызов метода `FromBundle` является синхронным (блокирующий) вызовом с несколькими встроенными функциями загрузки и управления изображениями, такими как поддержка кэширования и автоматическая обработка файлов изображений для различных решений.

В следующем примере показано, как задать изображение `UITabBarItem` на `UITabBar`.

```csharp
TabBarItem.Image = UIImage.FromBundle ("MyImage");
```

Предполагая, что `MyImage` — имя ресурса изображения, добавляемого в каталог активов выше. При работе с образами каталога активов просто укажите имя набора изображений в методе `FromBundle` для изображений в формате **PNG** :

```csharp
TabBarItem.Image = UIImage.FromBundle ("MyImage");
```

Для любого другого формата изображения Включите расширение с именем. Пример:

```csharp
TabBarItem.Image = UIImage.FromBundle ("MyImage.jpg");
```

Дополнительные сведения о значках и изображениях см. в документации Apple о [пользовательских значках и руководстве по созданию изображений](https://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html).

## <a name="displaying-an-image-in-a-storyboards"></a>Отображение изображения в раскадровках

После добавления образа в проект Xamarin. iOS с помощью каталогов активов его можно легко отобразить в раскадровке с помощью `UIImageView` в конструкторе iOS. Например, если был добавлен следующий ресурс изображения:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio для Mac](#tab/macos)

![](displaying-an-image-images/display01.png "A sample Image Asset has been added")

Чтобы отобразить его в раскадровке, выполните следующие действия:

1. Дважды щелкните файл `Main.storyboard` в **Обозреватель решений** , чтобы открыть его для редактирования в конструкторе iOS.
2. Выберите **представление изображения** из **области элементов**:

     ![](displaying-an-image-images/display02.png "Select an Image View from the Toolbox")
3. Перетащите представление изображения в область конструктора и установите его расположение и размер по мере необходимости:

    ![](displaying-an-image-images/display03.png "A new Image View on the Design Surface")
4. В разделе **мини** -приложения в **обозревателе свойств** выберите нужный ресурс **изображения** .

    ![](displaying-an-image-images/display04.png "Select the desired Image asset to be displayed")
5. В разделе " **представление** " используйте **режим** , чтобы управлять изменением размера изображения при изменении размера **представления изображения** .
6. Выбрав **представление изображения** , щелкните его еще раз, чтобы добавить **ограничения**:

    ![](displaying-an-image-images/display05.png "Adding Constraints")
7. Перетащите маркер "T" на каждом крае **представления изображения** на соответствующую сторону экрана, чтобы "закрепить" изображение на сторонах. Таким образом, **представление изображений** будет сжиматься и увеличиваться при изменении размера экрана.
8. Сохраните изменения в раскадровке.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![](displaying-an-image-images/display01vs.png "A sample Image Asset has been added")

Чтобы отобразить его в раскадровке, выполните следующие действия:

1. Дважды щелкните файл `Main.storyboard` в **Обозреватель решений** , чтобы открыть его для редактирования в конструкторе iOS.
2. Выберите **представление изображения** из **области элементов**:

     ![](displaying-an-image-images/display02vs.png "Select an Image View from the Toolbox")
3. Перетащите представление изображения в область конструктора и установите его расположение и размер по мере необходимости:

    ![](displaying-an-image-images/display03vs.png "A new Image View on the Design Surface")
4. В разделе **мини** -приложения в **обозревателе свойств** выберите нужный ресурс **изображения** .

    ![](displaying-an-image-images/display04vs.png "Select the desired Image asset to be displayed")
5. В разделе " **представление** " используйте **режим** , чтобы управлять изменением размера изображения при изменении размера **представления изображения** .
6. Выбрав **представление изображения** , щелкните его еще раз, чтобы добавить **ограничения**:

    ![](displaying-an-image-images/display05vs.png "Adding Constraints")
7. Перетащите маркер "T" на каждом крае **представления изображения** на соответствующую сторону экрана, чтобы "закрепить" изображение на сторонах. Таким образом, **представление изображений** будет сжиматься и увеличиваться при изменении размера экрана.
8. Сохраните изменения в раскадровке.

-----

## <a name="displaying-an-image-in-code"></a>Отображение изображения в коде

Как и при отображении изображения в раскадровке, после добавления изображения в проект Xamarin. iOS с помощью каталогов активов его можно легко отобразить с помощью C# кода.

Рассмотрим следующий пример:

```csharp
// Create an image view that will fill the
// parent image view and set the image from
// an Image Asset

var imageView = new UIImageView (View.Frame);
imageView.Image = UIImage.FromBundle ("Kemah");

// Add the Image View to the parent view
View.AddSubview (imageView);
```

Этот код создает новый `UIImageView` и присваивает ему исходный размер и расположение. Затем он загружает изображение из ресурса изображения, добавленного в проект, и добавляет `UIImageView` к родительскому `UIView`, чтобы отобразить его.

## <a name="related-links"></a>Связанные ссылки

- [Работа с изображениями (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/workingwithimages)
- [Hello, iPhone](~/ios/get-started/hello-ios/index.md)
- [Размер и разрешение изображения (Apple)](https://developer.apple.com/design/human-interface-guidelines/ios/icons-and-images/image-size-and-resolution/)
