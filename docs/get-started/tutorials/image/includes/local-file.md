---
ms.openlocfilehash: 93ee0681adcc63fe05b4be88ff67f0aeee3e03ca
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "61384587"
---
Файлы изображений могут быть добавлены в проекты платформы и использоваться в общем коде Xamarin.Forms. Этот метод распространения является обязательным для изображений, специфических для платформы, например при использовании разных разрешений на различных платформах или немного разных вариантах изображения.

В этом упражнении вы измените решение **ImageTutorial** для отображения локального изображения, не скачивая его по URI. Локальное изображение — это логотип Xamarin, который следует скачать, нажав кнопку ниже.

> [!div class="nextstepaction"]
> [Скачать XamarinLogo.png](https://raw.githubusercontent.com/xamarin/xamarin-forms-samples/master/UserInterface/PlatformSpecifics/Droid/Resources/drawable/XamarinLogo.png)

> [!IMPORTANT]
> Для использования одного изображения на всех платформах *следует использовать одно и то же имя файла на всех платформах*, и оно должно представлять собой допустимое имя ресурса Android (т. е. только строчные буквы, цифры, подчеркивания и точки).

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. В **обозревателе решений**в проекте **ImageTutorial.iOS** разверните **Каталог активов** и дважды щелкните **Активы**, чтобы открыть этот раздел. Затем в **Assets.xcassets** нажмите кнопку **Плюс** и выберите команду **Добавить набор изображений**:

    ![Снимок экрана создания набора изображений в каталоге активов в Visual Studio](../images/vs/new-image-set.png "Новый набор изображений в каталоге активов")

1. В **Assets.xcassets** выберите новый набор изображений; отобразится редактор:

    ![Снимок экрана с набором изображений в каталоге активов в Visual Studio](../images/vs/new-image-set-editor.png "Редактор набора изображений в каталоге активов")

1. Перетащите **XamarinLogo.png** из файловой системы в поле **1x** для **универсальной** категории:

    ![Снимок экрана с набором изображений, содержащим изображение, в Visual Studio](../images/vs/image-set-with-image.png "Набор изображений, содержащий изображение")

1. На вкладке **Assets.xcassets** щелкните правой кнопкой мыши имя нового набора изображений и переименуйте его в **XamarinLogo**:

    ![Снимок экрана переименованного набора изображений в Visual Studio](../images/vs/rename-image-set.png "Переименованный набор изображений")

    Сохраните и закройте и вкладку **Assets.xcassets**.

1. В **обозревателе решений** в проекте **ImageTutorial.Android** разверните папку **Ресурсы**. Затем перетащите **XamarinLogo.png** из файловой системы в папку **drawable**:

    ![Снимок экрана файла изображения в качестве ресурса Android в Visual Studio](../images/vs/android-resource.png "Локальный файл изображения в папке ресурсов Android")

    > [!NOTE]
    > Visual Studio автоматически задаст действие построения для изображения как **AndroidResource**.

1. В проекте **ImageTutorial** в файле **MainPage.xaml** измените объявление [`Image`](xref:Xamarin.Forms.Editor) для отображения локального файла **XamarinLogo**:

    ```xaml
    <Image Source="XamarinLogo"
           WidthRequest="{OnPlatform iOS=300, Android=250}"
           HorizontalOptions="Center" />
    ```

    Этот код задает в свойстве [`Source`](xref:Xamarin.Forms.Image.Source) имя локального файла для отображения. Свойство [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) имеет значение 300 аппаратно-независимых единиц в iOS и 250 аппаратно-независимых единиц в Android. Кроме того, свойство [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) указывает, что изображение будет отцентрировано по горизонтали.

    > [!NOTE]
    > Для PNG-изображений в iOS расширение **.png** в имени файла в свойстве [`Source`](xref:Xamarin.Forms.Image.Source) можно опустить. Для других форматов расширение является обязательным.

1. На панели инструментов Visual Studio для Mac нажмите кнопку **Запуск** (треугольная кнопка, похожая на кнопку воспроизведения) для запуска приложения в выбранном симуляторе iOS или эмуляторе Android:

    [![Снимок представления экрана с локальным изображением в iOS и Android](../images/local-file.png "Представление изображения с локальным файлом")](../images/local-file-large.png#lightbox "Представление изображения с локальным файлом")

    Дополнительные сведения о локальных изображениях см. в разделе [Вывод изображений](~/xamarin-forms/user-interface/images.md#local-images) в руководстве [Изображения в Xamarin.Forms](~/xamarin-forms/user-interface/images.md).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

1. На **Панели решения** в проекте **ImageTutorial.iOS** дважды щелкните **Assets.xcassets**, чтобы открыть его. Затем в **списке ресурсов** щелкните правой кнопкой мыши и выберите команду **Новый набор изображений**:

    ![Снимок экрана создания набора изображений в каталоге активов в Visual Studio для Mac](../images/vsmac/new-image-set.png "Новый набор изображений в каталоге активов")

1. В **списке активов** выберите новый набор изображений; отобразится редактор:

    ![Снимок экрана с набором изображений в каталоге активов в Visual Studio для Mac](../images/vsmac/new-image-set-editor.png "Редактор набора изображений в каталоге активов")

1. Перетащите **XamarinLogo.png** из файловой системы в поле **1x** для **универсальной** категории:

    ![Снимок экрана с набором изображений, содержащим изображение, в Visual Studio для Mac](../images/vsmac/image-set-with-image.png "Набор изображений, содержащий изображение")

1. В **списке активов** щелкните правой кнопкой мыши имя нового набора изображений и переименуйте его в **XamarinLogo**:

    ![Снимок экрана переименованного набора изображений в Visual Studio для Mac](../images/vsmac/rename-image-set.png "Переименованный набор изображений")

1. На **Панели решения** в проекте **ImageTutorial.Android** разверните папку **Ресурсы**. Затем перетащите **XamarinLogo.png** из файловой системы в папку **drawable**.

1. В диалоговом окне **Добавление файла в папку** нажмите кнопку **ОК**.

    ![Снимок экрана файла изображения в качестве ресурса Android в Visual Studio для Mac](../images/vsmac/android-resource.png "Локальный файл изображения в папке ресурсов Android")

    > [!NOTE]
    > Visual Studio для Mac автоматически задаст действие построения для изображения как **AndroidResource**.

1. В проекте **ImageTutorial** в файле **MainPage.xaml** измените объявление [`Image`](xref:Xamarin.Forms.Editor) для отображения локального файла **XamarinLogo**:

    ```xaml
    <Image Source="XamarinLogo"
           WidthRequest="{OnPlatform iOS=300, Android=250}"
           HorizontalOptions="Center" />
    ```

    Этот код задает в свойстве [`Source`](xref:Xamarin.Forms.Image.Source) имя локального файла для отображения. Свойство [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) имеет значение 300 аппаратно-независимых единиц в iOS и 250 аппаратно-независимых единиц в Android. Кроме того, свойство [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) указывает, что изображение будет отцентрировано по горизонтали.

    > [!NOTE]
    > Для PNG-изображений в iOS расширение **.png** в имени файла в свойстве [`Source`](xref:Xamarin.Forms.Image.Source) можно опустить. Для других форматов расширение является обязательным.

1. На панели инструментов Visual Studio для Mac нажмите кнопку **Запуск** (треугольная кнопка, похожая на кнопку воспроизведения) для запуска приложения в выбранном симуляторе iOS или эмуляторе Android:

    [![Снимок представления экрана с локальным изображением в iOS и Android](../images/local-file.png "Представление изображения с локальным файлом")](../images/local-file-large.png#lightbox "Представление изображения с локальным файлом")

    Дополнительные сведения о локальных изображениях см. в разделе [Вывод изображений](~/xamarin-forms/user-interface/images.md#local-images) в руководстве [Изображения в Xamarin.Forms](~/xamarin-forms/user-interface/images.md).
