---
ms.openlocfilehash: 90f3f9ff5ed29a1ae2c93e355fc15bc6550d78dd
ms.sourcegitcommit: ccbf914615c0ce6b3f308d930f7a77418aeb4dbc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/11/2020
ms.locfileid: "77135068"
---
В этом упражнении вы создадите пользовательский интерфейс для использования класса `RestService`, который, в свою очередь, получает данные из веб-API [OpenWeatherMap](https://openweathermap.org/).

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. В **обозревателе решений** в проекте **WebServiceTutorial** дважды щелкните файл **MainPage.xaml**, чтобы открыть его. В **MainPage.xaml** удалите весь код шаблона и замените его приведенным ниже кодом:

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="WebServiceTutorial.MainPage">
        <Grid Margin="20,35,20,20">
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="0.4*" />
                <ColumnDefinition Width="0.6*" />
            </Grid.ColumnDefinitions>
            <Grid.RowDefinitions>
                <RowDefinition Height="40" />
                <RowDefinition Height="40" />
                <RowDefinition Height="40" />
                <RowDefinition Height="40" />
                <RowDefinition Height="40" />
                <RowDefinition Height="40" />
                <RowDefinition Height="40" />
            </Grid.RowDefinitions>
            <Entry x:Name="cityEntry"
                   Grid.ColumnSpan="2"
                   Text="Seattle" />
            <Button Grid.ColumnSpan="2"
                    Grid.Row="1"
                    Text="Get Weather"
                    Clicked="OnButtonClicked" />                
            <Label Grid.Row="2"
                   Text="Location:" />
            <Label Grid.Row="2"
                   Grid.Column="1"
                   Text="{Binding Title}" />            
            <Label Grid.Row="3"
                   Text="Temperature:" />
            <Label Grid.Row="3"
                   Grid.Column="1"
                   Text="{Binding Main.Temperature}" />            
            <Label Grid.Row="4"
                   Text="Wind Speed:" />
            <Label Grid.Row="4"
                   Grid.Column="1"
                   Text="{Binding Wind.Speed}" />            
            <Label Grid.Row="5"
                   Text="Humidity:" />
            <Label Grid.Row="5"
                   Grid.Column="1"
                   Text="{Binding Main.Humidity}" />            
            <Label Grid.Row="6"
                   Text="Visibility:" />
            <Label Grid.Row="6"
                   Grid.Column="1"
                   Text="{Binding Weather[0].Visibility}" />
        </Grid>
    </ContentPage>
    ```

    Этот код декларативно определяет пользовательский интерфейс для страницы, который состоит из [`Entry`](xref:Xamarin.Forms.Entry), [`Button`](xref:Xamarin.Forms.Button) и набора экземпляров [`Label`](xref:Xamarin.Forms.Label) в [`Grid`](xref:Xamarin.Forms.Grid). `Entry` предварительно заполняется значением "Seattle" через свойство [`Text`](xref:Xamarin.Forms.InputView.Text). `Button` устанавливает событие [`Clicked`](xref:Xamarin.Forms.Button.Clicked) для обработчика событий с именем `OnButtonClicked`, который будет создан на следующем шаге. Половина экземпляров `Label` отображает статический текст, а оставшиеся экземпляры привязаны к свойствам `WeatherData`. В среде выполнения экземпляры `Label`, которые используют привязки данных, будут использовать свои соответствующие свойства [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) для объекта `WeatherData`, чтобы использовать выражения привязки. Дополнительные сведения о привязке данных см. в разделе [Привязки данных в Xamarin.Forms](~/xamarin-forms/app-fundamentals/data-binding/index.md).

    Кроме того, [`Entry`](xref:Xamarin.Forms.Entry) имеет имя, указанное с помощью атрибута `x:Name`. Это позволяет файлу с выделенным кодом получать доступ к объекту с помощью назначенного имени.

1. В **обозревателе решений** в проекте **WebServiceTutorial** разверните узел **MainPage.xaml** и дважды щелкните файл **MainPage.xaml.cs**, чтобы открыть его. Затем удалите из **MainPage.xaml.cs** весь шаблонный код и замените его приведенным ниже:

    ```csharp
    using System;
    using Xamarin.Forms;

    namespace WebServiceTutorial
    {
        public partial class MainPage : ContentPage
        {
            RestService _restService;

            public MainPage()
            {
                InitializeComponent();
                _restService = new RestService();
            }

            async void OnButtonClicked(object sender, EventArgs e)
            {
                if (!string.IsNullOrWhiteSpace(cityEntry.Text))
                {
                    WeatherData weatherData = await _restService.GetWeatherDataAsync(GenerateRequestUri(Constants.OpenWeatherMapEndpoint));
                    BindingContext = weatherData;
                }
            }

            string GenerateRequestUri(string endpoint)
            {
                string requestUri = endpoint;
                requestUri += $"?q={cityEntry.Text}";
                requestUri += "&units=imperial"; // or units=metric
                requestUri += $"&APPID={Constants.OpenWeatherMapAPIKey}";
                return requestUri;
            }
        }
    }
    ```

    Метод `OnButtonClicked`, который выполняется при касании [`Button`](xref:Xamarin.Forms.Button), вызывает метод `RestService.GetWeatherDataAsync` для извлечения данных о погоде для города, имя которого введено в [`Entry`](xref:Xamarin.Forms.Entry). Метод `GetWeatherDataAsync` требует аргумент `string`, представляющий URI вызываемого веб-API, и это осуществляется методом `GenerateRequestUri`. Этот метод принимает адрес конечной точки, хранящийся в константе `OpenWeatherMapEndpoint`, и добавляет к нему параметры запроса, которые указывают:

    - Город, для которого запрашиваются данные о погоде.
    - Единицы измерения данных о погоде.
    - Ваш личный ключ API.

    После получения запрошенных данных о погоде свойство [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) страницы устанавливается для объекта `WeatherData`. Дополнительные сведения о свойстве `BindingContext` см. в разделе [Привязки с контекстом привязки](~/xamarin-forms/app-fundamentals/data-binding/basic-bindings.md#bindings-with-a-binding-context) в руководстве по [привязкам данных в Xamarin.Forms](~/xamarin-forms/app-fundamentals/data-binding/index.md).

    > [!IMPORTANT]
    > Свойство [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) наследуется через визуальное дерево. Так как оно задано для объекта [`ContentPage`](xref:Xamarin.Forms.ContentPage), дочерние объекты `ContentPage` наследуют свое значение, включая экземпляры [`Label`](xref:Xamarin.Forms.Label).

1. На панели инструментов Visual Studio нажмите кнопку **Запуск** (треугольная кнопка, похожая на кнопку воспроизведения), чтобы запустить приложение в выбранном удаленном симуляторе iOS или эмуляторе Android. Коснитесь [`Button`](xref:Xamarin.Forms.Button) для получения текущих данных о погоде для Сиэтла:

    [![Снимок экрана с данными о погоде в Сиэтле в iOS и Android](../images/consume-web-service.png "Данные о погоде в Сиэтле")](../images/consume-web-service-large.png#lightbox "Данные о погоде в Сиэтле")

    > [!IMPORTANT]
    > Ваш личный ключ API OpenWeatherMap должен быть задан как значение константы `OpenWeatherMapAPIKey` в классе `Constants`.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

1. На **панели решений** в проекте **WebServiceTutorial** дважды щелкните файл **MainPage.xaml**, чтобы открыть его. В **MainPage.xaml** удалите весь код шаблона и замените его приведенным ниже кодом:

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="WebServiceTutorial.MainPage">
        <Grid Margin="20,35,20,20">
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="0.4*" />
                <ColumnDefinition Width="0.6*" />
            </Grid.ColumnDefinitions>
            <Grid.RowDefinitions>
                <RowDefinition Height="40" />
                <RowDefinition Height="40" />
                <RowDefinition Height="40" />
                <RowDefinition Height="40" />
                <RowDefinition Height="40" />
                <RowDefinition Height="40" />
                <RowDefinition Height="40" />
            </Grid.RowDefinitions>
            <Entry x:Name="cityEntry"
                   Grid.ColumnSpan="2"
                   Text="Seattle" />
            <Button Grid.ColumnSpan="2"
                    Grid.Row="1"
                    Text="Get Weather"
                    Clicked="OnButtonClicked" />                
            <Label Grid.Row="2"
                   Text="Location:" />
            <Label Grid.Row="2"
                   Grid.Column="1"
                   Text="{Binding Title}" />            
            <Label Grid.Row="3"
                   Text="Temperature:" />
            <Label Grid.Row="3"
                   Grid.Column="1"
                   Text="{Binding Main.Temperature}" />            
            <Label Grid.Row="4"
                   Text="Wind Speed:" />
            <Label Grid.Row="4"
                   Grid.Column="1"
                   Text="{Binding Wind.Speed}" />            
            <Label Grid.Row="5"
                   Text="Humidity:" />
            <Label Grid.Row="5"
                   Grid.Column="1"
                   Text="{Binding Main.Humidity}" />            
            <Label Grid.Row="6"
                   Text="Visibility:" />
            <Label Grid.Row="6"
                   Grid.Column="1"
                   Text="{Binding Weather[0].Visibility}" />
        </Grid>
    </ContentPage>
    ```

    Этот код декларативно определяет пользовательский интерфейс для страницы, который состоит из [`Entry`](xref:Xamarin.Forms.Entry), [`Button`](xref:Xamarin.Forms.Button) и набора экземпляров [`Label`](xref:Xamarin.Forms.Label) в [`Grid`](xref:Xamarin.Forms.Grid). `Entry` предварительно заполняется значением "Seattle" через свойство [`Text`](xref:Xamarin.Forms.InputView.Text). `Button` устанавливает событие [`Clicked`](xref:Xamarin.Forms.Button.Clicked) для обработчика событий с именем `OnButtonClicked`, который будет создан на следующем шаге. Половина экземпляров `Label` отображает статический текст, а оставшиеся экземпляры привязаны к свойствам `WeatherData`. В среде выполнения экземпляры `Label`, которые используют привязки данных, будут использовать свои соответствующие свойства [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) для объекта `WeatherData`, чтобы использовать выражения привязки. Дополнительные сведения о привязке данных см. в разделе [Привязки данных в Xamarin.Forms](~/xamarin-forms/app-fundamentals/data-binding/index.md).

    Кроме того, [`Entry`](xref:Xamarin.Forms.Entry) имеет имя, указанное с помощью атрибута `x:Name`. Это позволяет файлу с выделенным кодом получать доступ к объекту с помощью назначенного имени.

    Дополнительные сведения об использовании веб-служб на базе REST в Xamarin.Forms см. в разделе [Использование веб-службы RESTful (руководство)](~/xamarin-forms/data-cloud/web-services/rest.md).

1. На **панели решений** в проекте **WebServiceTutorial** разверните узел **MainPage.xaml** и дважды щелкните файл **MainPage.xaml.cs**, чтобы открыть его. Затем удалите из **MainPage.xaml.cs** весь шаблонный код и замените его приведенным ниже:

    ```csharp
    using System;
    using Xamarin.Forms;

    namespace WebServiceTutorial
    {
        public partial class MainPage : ContentPage
        {
            RestService _restService;

            public MainPage()
            {
                InitializeComponent();
                _restService = new RestService();
            }

            async void OnButtonClicked(object sender, EventArgs e)
            {
                if (!string.IsNullOrWhiteSpace(cityEntry.Text))
                {
                    WeatherData weatherData = await _restService.GetWeatherDataAsync(GenerateRequestUri(Constants.OpenWeatherMapEndpoint));
                    BindingContext = weatherData;
                }
            }

            string GenerateRequestUri(string endpoint)
            {
                string requestUri = endpoint;
                requestUri += $"?q={cityEntry.Text}";
                requestUri += "&units=imperial"; // or units=metric
                requestUri += $"&APPID={Constants.OpenWeatherMapAPIKey}";
                return requestUri;
            }
        }
    }
    ```

    Метод `OnButtonClicked`, который выполняется при касании [`Button`](xref:Xamarin.Forms.Button), вызывает метод `RestService.GetWeatherDataAsync` для извлечения данных о погоде для города, имя которого введено в [`Entry`](xref:Xamarin.Forms.Entry). Метод `GetWeatherDataAsync` требует аргумент `string`, представляющий URI вызываемого веб-API, и это осуществляется методом `GenerateRequestUri`. Этот метод принимает адрес конечной точки, хранящийся в константе `OpenWeatherMapEndpoint`, и добавляет к нему параметры запроса, которые указывают:

    - Город, для которого запрашиваются данные о погоде.
    - Единицы измерения данных о погоде.
    - Ваш личный ключ API.

    После получения запрошенных данных о погоде свойство [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) страницы устанавливается для объекта `WeatherData`. Дополнительные сведения о свойстве `BindingContext` см. в разделе [Привязки с контекстом привязки](~/xamarin-forms/app-fundamentals/data-binding/basic-bindings.md#bindings-with-a-binding-context) в руководстве по [привязкам данных в Xamarin.Forms](~/xamarin-forms/app-fundamentals/data-binding/index.md).

    > [!IMPORTANT]
    > Свойство [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) наследуется через визуальное дерево. Так как оно задано для объекта [`ContentPage`](xref:Xamarin.Forms.ContentPage), дочерние объекты `ContentPage` наследуют свое значение, включая экземпляры [`Label`](xref:Xamarin.Forms.Label).

1. На панели инструментов Visual Studio для Mac нажмите клавишу **Запуск** (треугольная кнопка, похожая на кнопку воспроизведения) для запуска приложения в выбранном симуляторе iOS или эмуляторе Android. Коснитесь [`Button`](xref:Xamarin.Forms.Button) для получения текущих данных о погоде для Сиэтла:

    [![Снимок экрана с данными о погоде в Сиэтле в iOS и Android](../images/consume-web-service.png "Данные о погоде в Сиэтле")](../images/consume-web-service-large.png#lightbox "Данные о погоде в Сиэтле")

    > [!IMPORTANT]
    > Ваш личный ключ API OpenWeatherMap должен быть задан как значение константы `OpenWeatherMapAPIKey` в классе `Constants`.

    Дополнительные сведения об использовании веб-служб на базе REST в Xamarin.Forms см. в разделе [Использование веб-службы RESTful (руководство)](~/xamarin-forms/data-cloud/web-services/rest.md).
