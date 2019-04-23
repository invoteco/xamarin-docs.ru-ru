---
ms.openlocfilehash: 8a4fbed10fa52c08a63f3aed306fe5cde21804b4
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "61384575"
---
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. В **MainPage.xaml** измените объявление [`Image`](xref:Xamarin.Forms.Image), чтобы настроить его внешний вид.

    ```xaml
    <Image Source="http://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/Papio_anubis_%28Serengeti%2C_2009%29.jpg/200px-Papio_anubis_%28Serengeti%2C_2009%29.jpg"
           Aspect="Fill"
           HeightRequest="{OnPlatform iOS=300, Android=250}"
           WidthRequest="{OnPlatform iOS=300, Android=250}"
           HorizontalOptions="Center" />
    ```

    Этот код задает свойство [`Aspect`](xref:Xamarin.Forms.Image.Aspect), которое определяет режим масштабирования изображения, для [`Fill`](xref:Xamarin.Forms.Aspect.Fill). Элемент `Fill` определен в перечислении [`Aspect`](xref:Xamarin.Forms.Aspect) и растягивает изображение, чтобы полностью заполнить вид, независимо от того, является ли изображение искажено. Дополнительные сведения о масштабировании изображения см. в разделе [Displaying images](~/xamarin-forms/user-interface/images.md#displaying-images) (Отображение изображений) в руководстве [Images in Xamarin.Forms](~/xamarin-forms/user-interface/images.md) (Изображения в Xamarin.Forms).

    Расширение разметки `OnPlatform` позволяет настраивать внешний вид пользовательского интерфейса для каждой платформы. В этом примере расширение разметки используется для установки свойств [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) и [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) равным 300 независимым от устройства единицам на iOS и 250 независимым от устройства единицам на Android. Дополнительные сведения о расширении разметки `OnPlatform` см. в разделе [Расширение разметки OnPlatform](~/xamarin-forms/xaml/markup-extensions/consuming.md#onplatform) в руководстве [Использование расширений разметки XAML](~/xamarin-forms/xaml/markup-extensions/consuming.md).

    Кроме того, свойство [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) указывает, что изображение будет отцентрировано по горизонтали.

1. На панели инструментов Visual Studio нажмите кнопку **Пуск** (треугольная кнопка, похожая на кнопку воспроизведения) для запуска приложения в выбранном удаленном симуляторе iOS или эмуляторе Android.

    [![Снимок экрана с изображением, имеющим разные размеры в iOS и Android](../images/customize-appearance.png "Размер изображения для каждой платформы")](../images/customize-appearance-large.png#lightbox "Размер изображения для каждой платформы")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

1. В **MainPage.xaml** измените объявление [`Image`](xref:Xamarin.Forms.Image), чтобы настроить его внешний вид.

    ```xaml
    <Image Source="http://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/Papio_anubis_%28Serengeti%2C_2009%29.jpg/200px-Papio_anubis_%28Serengeti%2C_2009%29.jpg"
           Aspect="Fill"
           HeightRequest="{OnPlatform iOS=300, Android=250}"
           WidthRequest="{OnPlatform iOS=300, Android=250}"
           HorizontalOptions="Center" />
    ```

    Этот код задает свойство [`Aspect`](xref:Xamarin.Forms.Image.Aspect), которое определяет режим масштабирования изображения, для [`Fill`](xref:Xamarin.Forms.Aspect.Fill). Элемент `Fill` определен в перечислении [`Aspect`](xref:Xamarin.Forms.Aspect) и растягивает изображение, чтобы полностью заполнить вид, независимо от того, является ли изображение искажено. Дополнительные сведения о масштабировании изображения см. в разделе [Displaying images](~/xamarin-forms/user-interface/images.md#displaying-images) (Отображение изображений) в руководстве [Images in Xamarin.Forms](~/xamarin-forms/user-interface/images.md) (Изображения в Xamarin.Forms).

    Расширение разметки `OnPlatform` позволяет настраивать внешний вид пользовательского интерфейса для каждой платформы. В этом примере расширение разметки используется для установки свойств [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) и [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) равными 300 в iOS и 250 в Android. Дополнительные сведения о расширении разметки `OnPlatform` см. в разделе [Расширение разметки OnPlatform](~/xamarin-forms/xaml/markup-extensions/consuming.md#onplatform) в руководстве [Использование расширений разметки XAML](~/xamarin-forms/xaml/markup-extensions/consuming.md).

    Кроме того, свойство [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) указывает, что изображение будет отцентрировано по горизонтали.

1. На панели инструментов Visual Studio для Mac нажмите кнопку **Пуск** (треугольная кнопка, похожая на кнопку воспроизведения) для запуска приложения в выбранном симуляторе iOS или эмуляторе Android.

    [![Снимок экрана с изображением, имеющим разные размеры в iOS и Android](../images/customize-appearance.png "Размер изображения для каждой платформы")](../images/customize-appearance-large.png#lightbox "Размер изображения для каждой платформы")
