---
title: Управление конфигурацией
description: В этой главе объясняется, как мобильное приложение eShopOnContainers реализует управление конфигурацией для предоставления параметров приложения и параметров пользователя.
ms.prod: xamarin
ms.assetid: 50d6e780-e768-47f8-9361-3af11e56b87b
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: cc83a18cd8c391c6228cf9d813ecf8bca795caba
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2019
ms.locfileid: "70760296"
---
# <a name="configuration-management"></a>Управление конфигурацией

Параметры позволяют разделить данные, которые настраивают поведение приложения из кода, позволяя изменять поведение без перестроения приложения. Существует два типа параметров: параметры приложения и параметры пользователя.

Параметры приложения — это данные, создаваемые и управляемые приложением. Сюда могут входить такие данные, как фиксированные конечные точки веб-служб, ключи API и состояние среды выполнения. Параметры приложения привязаны к существованию приложения и имеют смысл только для этого приложения.

Пользовательские параметры — это настраиваемые параметры приложения, которые влияют на поведение приложения и не нуждаются в частой повторной настройке. Например, приложение может позволить пользователю указать, откуда получать данные, и как отобразить их на экране.

Xamarin. Forms включает постоянный словарь, который можно использовать для хранения данных параметров. Доступ к этому словарю можно получить [`Application.Current.Properties`](xref:Xamarin.Forms.Application.Properties) с помощью свойства, и все данные, помещаемые в него, сохраняются при переходе приложения в спящий режим и восстанавливаются при возобновлении работы приложения или повторном запуске. Кроме того, [`Application`](xref:Xamarin.Forms.Application) класс также [`SavePropertiesAsync`](xref:Xamarin.Forms.Application.SavePropertiesAsync) имеет метод, который позволяет приложению сохранять параметры при необходимости. Дополнительные сведения об этом словаре см. в разделе [словарь свойств](~/xamarin-forms/app-fundamentals/application-class.md#Properties_Dictionary).

Недостаток хранения данных с помощью постоянного словаря Xamarin. Forms заключается в том, что данные не привязаны к данным. Таким образом, мобильное приложение eShopOnContainers использует библиотеку Ксам. plugins. Settings, доступную в [NuGet](https://www.nuget.org/packages/Xam.Plugins.Settings/). Эта библиотека предоставляет единообразный, строго типизированный и кросс-платформенный подход к сохранению и извлечению параметров приложений и пользователей, а также использование управления собственными параметрами, предоставляемых каждой платформой. Кроме того, можно легко использовать привязку данных для доступа к данным параметров, предоставляемым библиотекой.

> [!NOTE]
> Хотя в библиотеке Ксам. plugin. Settings можно хранить параметры приложения и пользователя, они не имеют различий между ними.

## <a name="creating-a-settings-class"></a>Создание класса параметров

При использовании библиотеки Ксам. plugins. Settings необходимо создать один статический класс, который будет содержать приложения и пользовательские параметры, необходимые для приложения. В следующем примере кода показан класс Settings в мобильном приложении eShopOnContainers:

```csharp
public static class Settings  
{  
    private static ISettings AppSettings  
    {  
        get  
        {  
            return CrossSettings.Current;  
        }  
    }  
    ...  
}
```

Параметры можно считывать и записывать с помощью `ISettings` API, который предоставляется библиотекой ксам. plugins. Settings. Эта библиотека предоставляет одноэлементный экземпляр, который можно использовать для доступа к `CrossSettings.Current`API, и класс параметров приложения должен предоставлять этот Singleton `ISettings` через свойство.

> [!NOTE]
> Директивы using для пространств имен plugin. Settings и plugin. Settings. абстракции следует добавлять в класс, который требует доступа к типам библиотек Ксам. plugins. Settings.

## <a name="adding-a-setting"></a>Добавление параметра

Каждый параметр состоит из ключа, значения по умолчанию и свойства. В следующем примере кода показаны все три элемента для параметра пользователя, представляющего базовый URL-адрес для веб-службы, к которому подключается мобильное приложение eShopOnContainers.

```csharp
public static class Settings  
{  
    ...  
    private const string IdUrlBase = "url_base";  
    private static readonly string UrlBaseDefault = GlobalSetting.Instance.BaseEndpoint;  
    ...  

    public static string UrlBase  
    {  
        get  
        {  
            return AppSettings.GetValueOrDefault<string>(IdUrlBase, UrlBaseDefault);  
        }  
        set  
        {  
            AppSettings.AddOrUpdateValue<string>(IdUrlBase, value);  
        }  
    }  
}
```

Ключ всегда является строкой константы, определяющей имя ключа, и значением по умолчанию для параметра, которое является статическим значением ReadOnly требуемого типа. Предоставление значения по умолчанию гарантирует доступность допустимого значения при извлечении неопределенного параметра.

Свойство static использует два метода `ISettings` из API для чтения или записи значения параметра. `UrlBase` `ISettings.GetValueOrDefault` Метод используется для получения значения параметра из хранилища, зависящего от платформы. Если для параметра не определено значение, вместо него извлекается значение по умолчанию. Аналогичным образом `ISettings.AddOrUpdateValue` метод используется для сохранения значения параметра в хранилище, зависящее от платформы.

Вместо того, чтобы определить значение по умолчанию внутри `Settings` класса `UrlBaseDefault` , строка получает свое значение из `GlobalSetting` класса. В следующем примере кода показано `BaseEndpoint` свойство и `UpdateEndpoint` метод в этом классе:

```csharp
public class GlobalSetting  
{  
    ...  
    public string BaseEndpoint  
    {  
        get { return _baseEndpoint; }  
        set  
        {  
            _baseEndpoint = value;  
            UpdateEndpoint(_baseEndpoint);  
        }  
    }  
    ...  

    private void UpdateEndpoint(string baseEndpoint)  
    {  
        RegisterWebsite = string.Format("{0}:5105/Account/Register", baseEndpoint);  
        CatalogEndpoint = string.Format("{0}:5101", baseEndpoint);  
        OrdersEndpoint = string.Format("{0}:5102", baseEndpoint);  
        BasketEndpoint = string.Format("{0}:5103", baseEndpoint);  
        IdentityEndpoint = string.Format("{0}:5105/connect/authorize", baseEndpoint);  
        UserInfoEndpoint = string.Format("{0}:5105/connect/userinfo", baseEndpoint);  
        TokenEndpoint = string.Format("{0}:5105/connect/token", baseEndpoint);  
        LogoutEndpoint = string.Format("{0}:5105/connect/endsession", baseEndpoint);  
        IdentityCallback = string.Format("{0}:5105/xamarincallback", baseEndpoint);  
        LogoutCallback = string.Format("{0}:5105/Account/Redirecting", baseEndpoint);  
    }  
}
```

При каждом задании `UpdateEndpoint` свойства вызывается метод. `BaseEndpoint` Этот метод обновляет ряд свойств, все из которых основаны на `UrlBase` пользовательском параметре, предоставляемом `Settings` классом, который представляет различные конечные точки, к которым подключается мобильное приложение eShopOnContainers.

## <a name="data-binding-to-user-settings"></a>Привязка данных к параметрам пользователя

В мобильном приложении `SettingsView` eShopOnContainers предоставляет два пользовательских параметра. Эти параметры позволяют настроить, следует ли приложению получать данные из микрослужб, развернутых в виде контейнеров DOCKER, или должно ли приложение получать данные из служб макетирования, не требующих подключения к Интернету. При выборе извлечения данных из контейнерных микрослужб необходимо указать URL-адрес базовой конечной точки для микрослужб. На рис. 7-1 `SettingsView` показано, когда пользователь выбрал извлечение данных из контейнерных микрослужб.

![](configuration-management-images/settings-endpoint.png "Параметры пользователя, предоставляемые мобильным приложением eShopOnContainers")

**Рис. 7-1**. Параметры пользователя, предоставляемые мобильным приложением eShopOnContainers

Привязку данных можно использовать для получения и установки параметров, `Settings` предоставляемых классом. Это достигается с помощью элементов управления в привязке представления для просмотра свойств модели, которые, в свою `Settings` очередь, имеют доступ к свойствам в классе, и создания уведомления об изменении свойства, если значение параметров изменилось. Сведения о том, как конструкция eShopOnContainers Mobile App конструирует модели и связывает их с представлениями, см. в разделе [Автоматическое создание модели представления с указателем модели представления](~/xamarin-forms/enterprise-application-patterns/mvvm.md#automatically_creating_a_view_model_with_a_view_model_locator).

В следующем примере кода показан [`Entry`](xref:Xamarin.Forms.Entry) элемент управления `SettingsView` из, который позволяет пользователю ввести URL-адрес базовой конечной точки для контейнерных микрослужб:

```xaml
<Entry Text="{Binding Endpoint, Mode=TwoWay}" />
```

Этот [`Entry`](xref:Xamarin.Forms.Entry) элемент управления привязывается `Endpoint` к свойству `SettingsViewModel` класса с помощью двусторонней привязки. В следующем примере кода показано свойство Endpoint:

```csharp
public string Endpoint  
{  
    get { return _endpoint; }  
    set  
    {  
        _endpoint = value;  

        if(!string.IsNullOrEmpty(_endpoint))  
        {  
            UpdateEndpoint(_endpoint);  
        }  

        RaisePropertyChanged(() => Endpoint);  
    }  
}
```

Если свойство задано, вызывается метод при условии, что предоставленное значение является допустимым, и вызывается уведомление об изменении свойства. `UpdateEndpoint` `Endpoint` Метод `UpdateEndpoint` показан в следующем примере кода:

```csharp
private void UpdateEndpoint(string endpoint)  
{  
    Settings.UrlBase = endpoint;  
}
```

Этот метод обновляет `UrlBase` свойство `Settings` в классе, используя значение URL-адреса базовой конечной точки, указанное пользователем, что приводит к его сохранению в хранилище, зависящем от платформы.

При переходе к `SettingsViewModel` `InitializeAsync` `SettingsView` выполняется метод в классе. Этот метод показан в следующем примере кода:

```csharp
public override Task InitializeAsync(object navigationData)  
{  
    ...  
    Endpoint = Settings.UrlBase;  
    ...  
}
```

Метод задает `Endpoint` для свойства значение `UrlBase` свойства в `Settings` классе. Обращение к `UrlBase` свойству приводит к тому, что библиотека ксам. plugins. Settings извлекает значение параметров из хранилища, зависящего от платформы. Сведения о том, как `InitializeAsync` вызывается метод, см. в разделе [Передача параметров во время навигации](~/xamarin-forms/enterprise-application-patterns/navigation.md#passing_parameters_during_navigation).

Этот механизм гарантирует, что каждый раз, когда пользователь переходит в Сеттингсвиев, пользовательские параметры извлекаются из хранилища, зависящего от платформы, и представляются с помощью привязки данных. Затем, если пользователь изменяет значения параметров, привязка данных гарантирует, что они будут немедленно сохранены в хранилище для конкретной платформы.

## <a name="summary"></a>Сводка

Параметры позволяют разделить данные, которые настраивают поведение приложения из кода, позволяя изменять поведение без перестроения приложения. Параметры приложения — это данные, создаваемые и управляемые приложением, а пользовательские параметры — это настраиваемые параметры приложения, которые влияют на поведение приложения и не нуждаются в частой повторной настройке.

Библиотека Ксам. plugins. Settings предоставляет единообразный, строго типизированный и кросс-платформенный подход к сохранению и извлечению параметров приложений и пользователей, а привязка данных может использоваться для доступа к параметрам, созданным с помощью библиотеки.

## <a name="related-links"></a>Связанные ссылки

- [Скачать электронную книгу (2 МБ в формате PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (пример)](https://github.com/dotnet-architecture/eShopOnContainers)
