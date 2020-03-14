---
title: Xamarin.Forms ImageButton
description: ImageButton отображает изображение и реагирует на касание или щелчок, который направляет приложению выполнения определенной задачи.
ms.prod: xamarin
ms.assetid: B5906AB6-3F79-4FCB-8C78-1F0AF18AB39E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/04/2019
ms.openlocfilehash: 7c6647a0299b5ece3caaaa1d322ec1a0efac3557
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/13/2020
ms.locfileid: "79305717"
---
# <a name="xamarinforms-imagebutton"></a>Xamarin.Forms ImageButton

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)

_Элемент ImageButton отображает изображение и реагирует на касание или щелчок, который направляет приложение для выполнения определенной задачи._

Представление `ImageButton` объединяет представление [`Button`](xref:Xamarin.Forms.Button) и представление [`Image`](xref:Xamarin.Forms.Image) для создания кнопки, содержимое которой является изображением. Пользователь нажимает `ImageButton` с помощью пальца или щелкает его с помощью мыши, чтобы направить приложение на выполнение определенной задачи. Однако, в отличие от представления `Button`, представление `ImageButton` не имеет концепции текста и текста.

> [!NOTE]
> [`Button`](xref:Xamarin.Forms.Button) представление определяет свойство [`Image`](xref:Xamarin.Forms.Button.Image) , которое позволяет отображать изображение на `Button`, это свойство предназначено для использования при отображении маленького значка рядом с текстом `Button`.

Примеры кода из этого руководств взяты из [примера формсгаллери](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery).

## <a name="setting-the-image-source"></a>Установка источника изображения

`ImageButton` определяет свойство `Source`, которое должно быть задано для изображения, отображаемого на кнопке, при этом источником изображения будет либо файл, URI, ресурс или поток. Дополнительные сведения о загрузке изображений из разных источников см. [в разделе изображения в Xamarin. Forms](images.md).

В следующем примере показано, как создать экземпляр `ImageButton` в XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="FormsGallery.XamlExamples.ImageButtonDemoPage"
             Title="ImageButton Demo">
    <StackLayout>
        <Label Text="ImageButton"
               FontSize="50"
               FontAttributes="Bold"
               HorizontalOptions="Center" />

       <ImageButton Source="XamarinLogo.png"
                    HorizontalOptions="Center"
                    VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

Свойство `Source` указывает изображение, которое отображается в `ImageButton`. В этом примере задается в локальный файл, которые будут загружены из каждого проекта платформы, что на следующих снимках экрана:

[![Базовая ImageButton](imagebutton-images/BasicImageButton.png "Базовая ImageButton")](imagebutton-images/BasicImageButton-Large.png#lightbox "Базовая ImageButton")

По умолчанию `ImageButton` является прямоугольной, но можно передать скругленные углы с помощью свойства `CornerRadius`. Дополнительные сведения о `ImageButton` внешний вид см. в разделе [представление ImageButton](#imagebutton-appearance).

> [!NOTE]
> Хотя `ImageButton` может загрузить анимированный GIF-файл, он будет отображать только первый кадр GIF-файла.

В следующем примере показано, как создать страницу, которая является функциональным эквивалентом предыдущего примера XAML, но полностью в C#:

```csharp
public class ImageButtonDemoPage : ContentPage
{
    public ImageButtonDemoPage()
    {
        Label header = new Label
        {
            Text = "ImageButton",
            FontSize = 50,
            FontAttributes = FontAttributes.Bold,
            HorizontalOptions = LayoutOptions.Center
        };

        ImageButton imageButton = new ImageButton
        {
            Source = "XamarinLogo.png",
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.CenterAndExpand
        };

        // Build the page.
        Title = "ImageButton Demo";
        Content = new StackLayout
        {
            Children = { header, imageButton }
        };
    }
}
```

## <a name="handling-imagebutton-clicks"></a>Обработка ImageButton нажимает кнопку

`ImageButton` определяет событие `Clicked`, которое возникает, когда пользователь отменяет `ImageButton` с помощью пальца или указателя мыши. Событие возникает при отпускании кнопки пальца или мыши с поверхности `ImageButton`. Свойство `IsEnabled` для `ImageButton` должно иметь значение `true`, чтобы реагировать на касания.

В следующем примере показано, как создать экземпляр `ImageButton` в XAML и обработать его событие `Clicked`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="FormsGallery.XamlExamples.ImageButtonDemoPage"
             Title="ImageButton Demo">
    <StackLayout>
        <Label Text="ImageButton"
               FontSize="50"
               FontAttributes="Bold"
               HorizontalOptions="Center" />

       <ImageButton Source="XamarinLogo.png"
                    HorizontalOptions="Center"
                    VerticalOptions="CenterAndExpand"
                    Clicked="OnImageButtonClicked" />

        <Label x:Name="label"
               Text="0 ImageButton clicks"
               FontSize="Large"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

Для события `Clicked` задается обработчик событий с именем `OnImageButtonClicked`, расположенный в файле кода программной части:

```csharp
public partial class ImageButtonDemoPage : ContentPage
{
    int clickTotal;

    public ImageButtonDemoPage()
    {
        InitializeComponent();
    }

    void OnImageButtonClicked(object sender, EventArgs e)
    {
        clickTotal += 1;
        label.Text = $"{clickTotal} ImageButton click{(clickTotal == 1 ? "" : "s")}";
    }
}
```

При нажатии `ImageButton` выполняется метод `OnImageButtonClicked`. Аргумент `sender` является `ImageButton`, ответственным за это событие. Его можно использовать для доступа к объекту `ImageButton` или для различения нескольких объектов `ImageButton`, совместно использующих одно и то же событие `Clicked`.

Этот конкретный обработчик `Clicked` увеличивает счетчик и отображает значение счетчика в [`Label`](xref:Xamarin.Forms.Label):

[![Базовая кнопка ImageButton Click](imagebutton-images/ImageButton.png "Базовая кнопка ImageButton Click")](imagebutton-images/ImageButton-Large.png#lightbox "Базовая кнопка ImageButton Click")

В следующем примере показано, как создать страницу, которая является функциональным эквивалентом предыдущего примера XAML, но полностью в C#:

```csharp
public class ImageButtonDemoPage : ContentPage
{
    Label label;
    int clickTotal = 0;

    public ImageButtonDemoPage()
    {
        Label header = new Label
        {
            Text = "ImageButton",
            FontSize = 50,
            FontAttributes = FontAttributes.Bold,
            HorizontalOptions = LayoutOptions.Center
        };

        ImageButton imageButton = new ImageButton
        {
            Source = "XamarinLogo.png",
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.CenterAndExpand
        };
        imageButton.Clicked += OnImageButtonClicked;

        label = new Label
        {
            Text = "0 ImageButton clicks",
            FontSize = Device.GetNamedSize(NamedSize.Large, typeof(Label)),
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.CenterAndExpand
        };

        // Build the page.
        Title = "ImageButton Demo";
        Content = new StackLayout
        {
            Children =
            {
                header,
                imageButton,
                label
            }
        };
    }

    void OnImageButtonClicked(object sender, EventArgs e)
    {
        clickTotal += 1;
        label.Text = $"{clickTotal} ImageButton click{(clickTotal == 1 ? "" : "s")}";
    }
}
```

## <a name="disabling-the-imagebutton"></a>Отключение ImageButton

Иногда приложение находится в определенном состоянии, в котором определенное `ImageButton` щелчком не является допустимой операцией. В таких случаях `ImageButton` следует отключить, задав свойству `IsEnabled` значение `false`.

## <a name="using-the-command-interface"></a>С помощью интерфейса командной

Приложение может реагировать на `ImageButton` касаний без обработки события `Clicked`. `ImageButton` реализует альтернативный механизм уведомления, называемый _командой_ или интерфейсом _команд_ . Он состоит из двух свойств:

- `Command` типа [`ICommand`](xref:System.Windows.Input.ICommand), интерфейс, определенный в пространстве имен [`System.Windows.Input`](xref:System.Windows.Input) .
- Свойство `CommandParameter` типа [`Object`](xref:System.Object).

Этот подход используется в связи с привязкой к данным и особенно в том случае, если реализация архитектуры Model-View-ViewModel (MVVM).

Дополнительные сведения об использовании интерфейса командной строки см [. в разделе](button.md) [Использование интерфейса команды](button.md#using-the-command-interface) в этой панели.

## <a name="pressing-and-releasing-the-imagebutton"></a>Нажатие и отпускание ImageButton

Помимо `Clicked` события `ImageButton` также определяет `Pressed` и `Released` события. Событие `Pressed` возникает, когда палец нажимает на `ImageButton`или нажата кнопка мыши с указателем, расположенным на `ImageButton`. Событие `Released` возникает при отпускании кнопки мыши или пальца. Как правило, событие `Clicked` также срабатывает в то же время, что и событие `Released`, но если палец или указатель мыши выходит за пределы поверхности `ImageButton` до выпуска, событие `Clicked` может не произойти.

Дополнительные сведения об этих событиях см. в разделе, посвященном [нажатию и отпустите кнопку](button.md#pressing-and-releasing-the-button) [в данной панели](button.md) .

## <a name="imagebutton-appearance"></a>Внешний вид ImageButton

Помимо свойств, которые `ImageButton` наследуют от класса [`View`](xref:Xamarin.Forms.View) , `ImageButton` также определяет несколько свойств, влияющих на его внешний вид:

- `Aspect`, как изображение будет масштабироваться в соответствии с отображаемой областью.
- `BorderColor` — это цвет области, окружающей `ImageButton`.
- `BorderWidth` — это ширина границы.
- `CornerRadius` является радиусом угла `ImageButton`.

Свойству `Aspect` может быть присвоено значение одного из членов перечисления [`Aspect`](xref:Xamarin.Forms.Aspect) :

- [`Fill`](xref:Xamarin.Forms.Aspect.Fill) — растягивает изображение до полного и точного заполнения `ImageButton`. Это может привести в образе искажен.
- [`AspectFill`](xref:Xamarin.Forms.Aspect.AspectFill) — обрезает изображение таким образом, чтобы оно заполнило `ImageButton` с сохранением пропорций.
- [`AspectFit`](xref:Xamarin.Forms.Aspect.AspectFit) — леттербоксес изображение (при необходимости), чтобы весь образ поместился в `ImageButton`, с добавлением пробела в верхнюю или нижнюю часть или в зависимости от того, является ли изображение широким или максимальным. Это значение по умолчанию для перечисления [`Aspect`](xref:Xamarin.Forms.Aspect) .

> [!NOTE]
> Класс `ImageButton` также имеет [`Margin`](xref:Xamarin.Forms.View.Margin) и `Padding` свойства, управляющие поведением макета `ImageButton`. Дополнительные сведения см. в статье [Поля и заполнение](~/xamarin-forms/user-interface/layouts/margin-and-padding.md).

## <a name="imagebutton-visual-states"></a>Визуальные состояния ImageButton

`ImageButton` имеет `Pressed` [`VisualState`](xref:Xamarin.Forms.VisualState) , который можно использовать для инициации визуального изменения `ImageButton` при нажатии пользователем при условии, что он включен.

В следующем примере XAML показано, как определить визуальное состояние для состояния `Pressed`.

```xaml
<ImageButton Source="XamarinLogo.png"
             ...>
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="Normal">
                <VisualState.Setters>
                    <Setter Property="Scale"
                            Value="1" />
                </VisualState.Setters>
            </VisualState>

            <VisualState x:Name="Pressed">
                <VisualState.Setters>
                    <Setter Property="Scale"
                            Value="0.8" />
                </VisualState.Setters>
            </VisualState>

        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</ImageButton>
```

`Pressed` [`VisualState`](xref:Xamarin.Forms.VisualState) указывает, что при нажатии `ImageButton` его свойство [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) будет заменено значением по умолчанию от 1 до 0,8. `Normal` `VisualState` указывает, что если `ImageButton` находится в нормальном состоянии, его свойство `Scale` будет установлено в значение 1. Таким образом, общий результат заключается в том, что при нажатии `ImageButton` его масштаб уменьшается, и при освобождении `ImageButton` его размер изменяется до размера по умолчанию.

Дополнительные сведения о визуальных состояниях см. [в разделе Диспетчер визуальных состояний Xamarin. Forms](~/xamarin-forms/user-interface/visual-state-manager.md).

## <a name="related-links"></a>Связанные ссылки

- [Пример Формсгаллери](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)
