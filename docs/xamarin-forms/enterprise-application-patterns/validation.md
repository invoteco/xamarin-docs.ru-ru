---
title: Проверка в корпоративных приложениях
description: В этой главе объясняется, как мобильное приложение eShopOnContainers выполняет проверку вводимых пользователем данных. Это включает в себя указание правил проверки, запуск проверки и отображение ошибок проверки.
ms.prod: xamarin
ms.assetid: 56e4f0fc-48d9-4033-91ec-173bb46a5e4d
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: de5728710a408b8e0c7c68dc89c7e6484cbcc3ce
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/13/2020
ms.locfileid: "79306395"
---
# <a name="validation-in-enterprise-apps"></a>Проверка в корпоративных приложениях

Любое приложение, принимающее входные данные от пользователей, должно обеспечить допустимость входных данных. Приложение может, например, проверять наличие входных данных, содержащих только символы в определенном диапазоне, имеет определенную длину или соответствует определенному формату. Без проверки пользователь может предоставить данные, которые приведут к сбою приложения. Проверка применяет бизнес-правила и предотвращает внедрение вредоносных данных злоумышленником.

В контексте шаблона Model-View-ViewModel (MVVM) для проверки данных часто требуется модель или модель представления, чтобы пользователь мог исправить все ошибки проверки. Мобильное приложение eShopOnContainers выполняет синхронную проверку свойств модели View на стороне клиента и уведомляет пользователя о любых ошибках проверки, выделяя элемент управления, содержащий недопустимые данные, и отображая сообщения об ошибках, которые сообщают пользователю. причины недопустимых данных. На рис. 6-1 показаны классы, участвующие в выполнении проверки в мобильном приложении eShopOnContainers.

[![](validation-images/validation.png "Validation classes in the eShopOnContainers mobile app")](validation-images/validation-large.png#lightbox "Validation classes in the eShopOnContainers mobile app")

**Рис. 6-1**. классы проверки в мобильном приложении eShopOnContainers

Просмотр свойств модели, требующих проверки, имеет тип `ValidatableObject<T>`, и каждый экземпляр `ValidatableObject<T>` имеет правила проверки, добавленные в свойство `Validations`. Проверка вызывается из модели представления путем вызова метода `Validate` экземпляра `ValidatableObject<T>`, который извлекает правила проверки и выполняет их для свойства `ValidatableObject<T>` `Value`. Любые ошибки проверки помещаются в свойство `Errors` экземпляра `ValidatableObject<T>`, а свойство `IsValid` экземпляра `ValidatableObject<T>` обновляется, чтобы указать, успешно ли выполнена проверка.

Уведомление об изменении свойства предоставляется классом `ExtendedBindableObject`, поэтому элемент управления [`Entry`](xref:Xamarin.Forms.Entry) может выполнить привязку к свойству `IsValid` экземпляра `ValidatableObject<T>` в классе модели представления, чтобы получать уведомления о том, являются ли введенные данные допустимыми.

## <a name="specifying-validation-rules"></a>Указание правил проверки

Правила проверки задаются путем создания класса, производного от интерфейса `IValidationRule<T>`, который показан в следующем примере кода:

```csharp
public interface IValidationRule<T>  
{  
    string ValidationMessage { get; set; }  
    bool Check(T value);  
}
```

Этот интерфейс указывает, что класс правила проверки должен предоставлять метод `boolean` `Check`, который используется для выполнения необходимой проверки, и свойство `ValidationMessage`, значение которого является сообщением об ошибке проверки, которое будет отображаться при сбое проверки.

В следующем примере кода показано правило проверки `IsNotNullOrEmptyRule<T>`, которое используется для проверки имени пользователя и пароля, вводимых пользователем на `LoginView` при использовании служб макетирования в мобильном приложении eShopOnContainers:

```csharp
public class IsNotNullOrEmptyRule<T> : IValidationRule<T>  
{  
    public string ValidationMessage { get; set; }  

    public bool Check(T value)  
    {  
        if (value == null)  
        {  
            return false;  
        }  

        var str = value as string;  
        return !string.IsNullOrWhiteSpace(str);  
    }  
}
```

Метод `Check` возвращает `boolean`, указывающий, является ли аргумент значения `null`, пустым или состоит только из пробелов.

В следующем примере кода, который не используется мобильным приложением eShopOnContainers, показано правило проверки для проверки адресов электронной почты.

```csharp
public class EmailRule<T> : IValidationRule<T>  
{  
    public string ValidationMessage { get; set; }  

    public bool Check(T value)  
    {  
        if (value == null)  
        {  
            return false;  
        }  

        var str = value as string;  
        Regex regex = new Regex(@"^([\w\.\-]+)@([\w\-]+)((\.(\w){2,3})+)$");  
        Match match = regex.Match(str);  

        return match.Success;  
    }  
}
```

Метод `Check` возвращает `boolean`, указывающий, является ли аргумент value допустимым адресом электронной почты. Это достигается путем поиска аргумента value для первого вхождения шаблона регулярного выражения, указанного в конструкторе `Regex`. Можно ли определить шаблон регулярного выражения во входной строке, проверив значение свойства `Success` объекта `Match`.

> [!NOTE]
> Иногда проверка свойства может содержать зависимые свойства. Примером зависимых свойств является то, что набор допустимых значений свойства A зависит от конкретного значения, установленного в свойстве B. Чтобы проверить, что значение свойства A является одним из допустимых значений, будет требовать получения значения свойства б. Кроме того, при изменении значения свойства б необходимо будет повторно проверить свойство A.

## <a name="adding-validation-rules-to-a-property"></a>Добавление правил проверки к свойству

В мобильном приложении eShopOnContainers свойства модели, требующие проверки, объявляются с типом `ValidatableObject<T>`, где `T` — это тип проверяемых данных. В следующем примере кода показан пример двух таких свойств:

```csharp
public ValidatableObject<string> UserName  
{  
    get  
    {  
        return _userName;  
    }  
    set  
    {  
        _userName = value;  
        RaisePropertyChanged(() => UserName);  
    }  
}  

public ValidatableObject<string> Password  
{  
    get  
    {  
        return _password;  
    }  
    set  
    {  
        _password = value;  
        RaisePropertyChanged(() => Password);  
    }  
}
```

Для выполнения проверки необходимо добавить правила проверки в коллекцию `Validations` каждого экземпляра `ValidatableObject<T>`, как показано в следующем примере кода:

```csharp
private void AddValidations()  
{  
    _userName.Validations.Add(new IsNotNullOrEmptyRule<string>   
    {   
        ValidationMessage = "A username is required."   
    });  
    _password.Validations.Add(new IsNotNullOrEmptyRule<string>   
    {   
        ValidationMessage = "A password is required."   
    });  
}
```

Этот метод добавляет правило проверки `IsNotNullOrEmptyRule<T>` в коллекцию `Validations` каждого экземпляра `ValidatableObject<T>`, задавая значения для свойства `ValidationMessage` правила проверки, которое указывает сообщение об ошибке проверки, которое будет отображаться при сбое проверки.

## <a name="triggering-validation"></a>Активация проверки

Метод проверки, используемый в мобильном приложении eShopOnContainers, может вручную активировать проверку свойства и автоматически запускать проверку при изменении свойства.

### <a name="triggering-validation-manually"></a>Активация проверки вручную

Проверку можно запустить вручную для свойства модели представления. Например, это происходит в мобильном приложении eShopOnContainers, когда пользователь нажмет кнопку **входа** на `LoginView`при использовании служб макетирования. Делегат команды вызывает метод `MockSignInAsync` в `LoginViewModel`, который вызывает проверку, выполняя метод `Validate`, который показан в следующем примере кода:

```csharp
private bool Validate()  
{  
    bool isValidUser = ValidateUserName();  
    bool isValidPassword = ValidatePassword();  
    return isValidUser && isValidPassword;  
}  

private bool ValidateUserName()  
{  
    return _userName.Validate();  
}  

private bool ValidatePassword()  
{  
    return _password.Validate();  
}
```

Метод `Validate` выполняет проверку имени пользователя и пароля, вводимых пользователем на `LoginView`, путем вызова метода Validate для каждого экземпляра `ValidatableObject<T>`. В следующем примере кода показан метод Validate из класса `ValidatableObject<T>`:

```csharp
public bool Validate()  
{  
    Errors.Clear();  

    IEnumerable<string> errors = _validations  
        .Where(v => !v.Check(Value))  
        .Select(v => v.ValidationMessage);  

    Errors = errors.ToList();  
    IsValid = !Errors.Any();  

    return this.IsValid;  
}
```

Этот метод очищает коллекцию `Errors`, а затем извлекает все правила проверки, добавленные в коллекцию `Validations` объекта. Выполняется метод `Check` для каждого полученного правила проверки, и значение свойства `ValidationMessage` для любого правила проверки, которое не удается проверить данные, добавляется в коллекцию `Errors` экземпляра `ValidatableObject<T>`. Наконец, задается свойство `IsValid`, и его значение возвращается вызывающему методу, который указывает, завершилась ли проверка успешно или неудачно.

### <a name="triggering-validation-when-properties-change"></a>Активация проверки при изменении свойств

Проверка может также запускаться при изменении привязанного свойства. Например, если двусторонняя привязка в `LoginView` задает свойство `UserName` или `Password`, то проверка активируется. В следующем примере кода показано, как это происходит:

```xaml
<Entry Text="{Binding UserName.Value, Mode=TwoWay}">  
    <Entry.Behaviors>  
        <behaviors:EventToCommandBehavior  
            EventName="TextChanged"  
            Command="{Binding ValidateUserNameCommand}" />  
    </Entry.Behaviors>  
    ...  
</Entry>
```

[`Entry`](xref:Xamarin.Forms.Entry) элемент управления привязывается к свойству `UserName.Value` экземпляра `ValidatableObject<T>`, а в коллекции `Behaviors` элемента управления добавляется экземпляр `EventToCommandBehavior`. Это поведение выполняет `ValidateUserNameCommand` в ответ на событие [`TextChanged`], возникающее на `Entry`, которое вызывается при изменении текста в `Entry`. В свою очередь, `ValidateUserNameCommand` делегат выполняет метод `ValidateUserName`, который выполняет метод `Validate` на экземпляре `ValidatableObject<T>`. Таким образом, каждый раз, когда пользователь вводит символ в элемент управления `Entry` для имени пользователя, выполняется проверка введенных данных.

Дополнительные сведения о поведении см. в разделе [Реализация поведения](~/xamarin-forms/enterprise-application-patterns/mvvm.md#implementing_behaviors).

<a name="displaying_validation_errors" />

## <a name="displaying-validation-errors"></a>Отображение ошибок проверки

Мобильное приложение eShopOnContainers уведомляет пользователя о любых ошибках проверки, выделяя элемент управления, содержащий недопустимые данные, с красной линией и отображая сообщение об ошибке, сообщающее пользователю, почему данные являются недопустимыми ниже элемента управления, содержащего недопустимые данные. При исправлении недопустимых данных строка изменяется на черный, а сообщение об ошибке удаляется. На рис. 6-2 показано представление LoginView в мобильном приложении eShopOnContainers при наличии ошибок проверки.

![](validation-images/validation-login.png "Displaying validation errors during login")

**Рис. 6-2.** Отображение ошибок проверки во время входа

### <a name="highlighting-a-control-that-contains-invalid-data"></a>Выделение элемента управления, содержащего недопустимые данные

`LineColorBehavior` присоединенное поведение используется для выделения [`Entry`](xref:Xamarin.Forms.Entry) элементов управления, в которых произошли ошибки проверки. В следующем примере кода показано, как присоединенное к `LineColorBehavior`е поведение прикрепляется к элементу управления `Entry`.

```xaml
<Entry Text="{Binding UserName.Value, Mode=TwoWay}">
    <Entry.Style>
        <OnPlatform x:TypeArguments="Style">
            <On Platform="iOS, Android" Value="{StaticResource EntryStyle}" />
            <On Platform="UWP" Value="{StaticResource UwpEntryStyle}" />
        </OnPlatform>
    </Entry.Style>
    ...
</Entry>
```

Элемент управления [`Entry`](xref:Xamarin.Forms.Entry) использует явный стиль, который показан в следующем примере кода:

```xaml
<Style x:Key="EntryStyle"  
       TargetType="{x:Type Entry}">  
    ...  
    <Setter Property="behaviors:LineColorBehavior.ApplyLineColor"  
            Value="True" />  
    <Setter Property="behaviors:LineColorBehavior.LineColor"  
            Value="{StaticResource BlackColor}" />  
    ...  
</Style>
```

Этот стиль задает `ApplyLineColor` и `LineColor` присоединенные свойства связанного с `LineColorBehavior` поведения в элементе управления [`Entry`](xref:Xamarin.Forms.Entry) . Дополнительные сведения о стилях см. в статье [Стили](~/xamarin-forms/user-interface/styles/index.md).

Если значение присоединенного свойства `ApplyLineColor` установлено или изменяется, то `LineColorBehavior` присоединенное поведение выполняет метод `OnApplyLineColorChanged`, который показан в следующем примере кода:

```csharp
public static class LineColorBehavior  
{  
    ...  
    private static void OnApplyLineColorChanged(  
                BindableObject bindable, object oldValue, object newValue)  
    {  
        var view = bindable as View;  
        if (view == null)  
        {  
            return;  
        }  

        bool hasLine = (bool)newValue;  
        if (hasLine)  
        {  
            view.Effects.Add(new EntryLineColorEffect());  
        }  
        else  
        {  
            var entryLineColorEffectToRemove =   
                    view.Effects.FirstOrDefault(e => e is EntryLineColorEffect);  
            if (entryLineColorEffectToRemove != null)  
            {  
                view.Effects.Remove(entryLineColorEffectToRemove);  
            }  
        }  
    }  
}
```

Параметры этого метода предоставляют экземпляр элемента управления, к которому присоединено поведение, а также старое и новое значения присоединенного свойства `ApplyLineColor`. `EntryLineColorEffect` класс добавляется в коллекцию [`Effects`](xref:Xamarin.Forms.Element.Effects) элемента управления, если `ApplyLineColor` присоединенное свойство `true`, в противном случае оно удаляется из коллекции `Effects` элемента управления. Дополнительные сведения о поведении см. в разделе [Реализация поведения](~/xamarin-forms/enterprise-application-patterns/mvvm.md#implementing_behaviors).

`EntryLineColorEffect` подклассы класса [`RoutingEffect`](xref:Xamarin.Forms.RoutingEffect) и показан в следующем примере кода:

```csharp
public class EntryLineColorEffect : RoutingEffect  
{  
    public EntryLineColorEffect() : base("eShopOnContainers.EntryLineColorEffect")  
    {  
    }  
}
```

Класс [`RoutingEffect`](xref:Xamarin.Forms.RoutingEffect) представляет независимый от платформы результат, который заключает в оболочку внутренний результат, зависящий от платформы. Это упрощает процесс удаления эффекта, так как отсутствует доступ времени компиляции к сведениям о типе для определяемого платформой эффекта. `EntryLineColorEffect` вызывает конструктор базового класса, передавая параметр, состоящий из объединения имени группы разрешения, и уникального идентификатора, указанного в каждом классе влияния для конкретной платформы.

В следующем примере кода показана реализация `eShopOnContainers.EntryLineColorEffect` для iOS:

```csharp
[assembly: ResolutionGroupName("eShopOnContainers")]  
[assembly: ExportEffect(typeof(EntryLineColorEffect), "EntryLineColorEffect")]  
namespace eShopOnContainers.iOS.Effects  
{  
    public class EntryLineColorEffect : PlatformEffect  
    {  
        UITextField control;  

        protected override void OnAttached()  
        {  
            try  
            {  
                control = Control as UITextField;  
                UpdateLineColor();  
            }  
            catch (Exception ex)  
            {  
                Console.WriteLine("Can't set property on attached control. Error: ", ex.Message);  
            }  
        }  

        protected override void OnDetached()  
        {  
            control = null;  
        }  

        protected override void OnElementPropertyChanged(PropertyChangedEventArgs args)  
        {  
            base.OnElementPropertyChanged(args);  

            if (args.PropertyName == LineColorBehavior.LineColorProperty.PropertyName ||  
                args.PropertyName == "Height")  
            {  
                Initialize();  
                UpdateLineColor();  
            }  
        }  

        private void Initialize()  
        {  
            var entry = Element as Entry;  
            if (entry != null)  
            {  
                Control.Bounds = new CGRect(0, 0, entry.Width, entry.Height);  
            }  
        }  

        private void UpdateLineColor()  
        {  
            BorderLineLayer lineLayer = control.Layer.Sublayers.OfType<BorderLineLayer>()  
                                                             .FirstOrDefault();  

            if (lineLayer == null)  
            {  
                lineLayer = new BorderLineLayer();  
                lineLayer.MasksToBounds = true;  
                lineLayer.BorderWidth = 1.0f;  
                control.Layer.AddSublayer(lineLayer);  
                control.BorderStyle = UITextBorderStyle.None;  
            }  

            lineLayer.Frame = new CGRect(0f, Control.Frame.Height-1f, Control.Bounds.Width, 1f);  
            lineLayer.BorderColor = LineColorBehavior.GetLineColor(Element).ToCGColor();  
            control.TintColor = control.TextColor;  
        }  

        private class BorderLineLayer : CALayer  
        {  
        }  
    }  
}
```

Метод `OnAttached` извлекает собственный элемент управления для элемента управления Xamarin. Forms [`Entry`](xref:Xamarin.Forms.Entry) и обновляет цвет линии путем вызова метода `UpdateLineColor`. Переопределение `OnElementPropertyChanged` реагирует на изменения свойств, которые можно привязать к элементу управления `Entry`, путем обновления цвета линии при изменении присоединенного свойства `LineColor` или при изменении свойства [`Height`](xref:Xamarin.Forms.VisualElement.Height) `Entry`. Дополнительные сведения об эффектах см. в статье [Эффекты](~/xamarin-forms/app-fundamentals/effects/index.md).

Если в элементе управления [`Entry`](xref:Xamarin.Forms.Entry) указаны допустимые данные, то в нижней части элемента управления будет применена черная линия, указывающая, что нет ошибки проверки. На рисунке 6-3 показан пример этого.

![](validation-images/validation-blackline.png "Black line indicating no validation error")

**Рис. 6-3**. Черная линия, указывающая на отсутствие ошибки проверки

Элемент управления [`Entry`](xref:Xamarin.Forms.Entry) также имеет [`DataTrigger`](xref:Xamarin.Forms.DataTrigger) добавлен в коллекцию [`Triggers`](xref:Xamarin.Forms.VisualElement.Triggers) . В следующем примере кода показано `DataTrigger`:

```xaml
<Entry Text="{Binding UserName.Value, Mode=TwoWay}">  
    ...  
    <Entry.Triggers>  
        <DataTrigger   
            TargetType="Entry"  
            Binding="{Binding UserName.IsValid}"  
            Value="False">  
            <Setter Property="behaviors:LineColorBehavior.LineColor"   
                    Value="{StaticResource ErrorColor}" />  
        </DataTrigger>  
    </Entry.Triggers>  
</Entry>
```

Этот [`DataTrigger`](xref:Xamarin.Forms.DataTrigger) наблюдает за свойством `UserName.IsValid` и, если он имеет значение `false`, он выполняет [`Setter`](xref:Xamarin.Forms.Setter), что изменяет присоединенное свойство `LineColor` присоединенного поведения `LineColorBehavior` на красный. На рисунке 6-4 показан пример этого.

![](validation-images/validation-redline.png "Red line indicating validation error")

**Рис. 6-4**. Красная линия, указывающая на ошибку проверки

Строка в элементе управления [`Entry`](xref:Xamarin.Forms.Entry) останется красным, тогда как входные данные станут недопустимыми, в противном случае изменится на черный, чтобы указать, что входные данные являются допустимыми.

Дополнительные сведения о триггерах см. в разделе [Triggers](~/xamarin-forms/app-fundamentals/triggers.md).

### <a name="displaying-error-messages"></a>Отображение сообщений об ошибках

Пользовательский интерфейс отображает сообщения об ошибках проверки в элементах управления Label под каждым элементом управления, чьи данные не прошли проверку. В следующем примере кода показан [`Label`](xref:Xamarin.Forms.Label) , отображающий сообщение об ошибке проверки, если пользователь не указал допустимое имя пользователя:

```xaml
<Label Text="{Binding UserName.Errors, Converter={StaticResource FirstValidationErrorConverter}}"  
       Style="{StaticResource ValidationErrorLabelStyle}" />
```

Каждый [`Label`](xref:Xamarin.Forms.Label) привязывается к свойству `Errors` проверяемого объекта модели представления. Свойство `Errors` предоставляется классом `ValidatableObject<T>` и имеет тип `List<string>`. Поскольку свойство `Errors` может содержать несколько ошибок проверки, экземпляр `FirstValidationErrorConverter` используется для получения первой ошибки из коллекции для вывода.

## <a name="summary"></a>Сводка

Мобильное приложение eShopOnContainers выполняет синхронную проверку свойств модели View на стороне клиента и уведомляет пользователя о любых ошибках проверки, выделяя элемент управления, содержащий недопустимые данные, и отображая сообщения об ошибках, которые сообщают пользователю. Почему данные недопустимы.

Просмотр свойств модели, требующих проверки, имеет тип `ValidatableObject<T>`, и каждый экземпляр `ValidatableObject<T>` имеет правила проверки, добавленные в свойство `Validations`. Проверка вызывается из модели представления путем вызова метода `Validate` экземпляра `ValidatableObject<T>`, который извлекает правила проверки и выполняет их для свойства `ValidatableObject<T>` `Value`. Любые ошибки проверки помещаются в свойство `Errors` экземпляра `ValidatableObject<T>`, а свойство `IsValid` экземпляра `ValidatableObject<T>` обновляется, чтобы указать, успешно ли выполнена проверка.

## <a name="related-links"></a>Связанные ссылки

- [Скачать электронную книгу (2 МБ в формате PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (пример)](https://github.com/dotnet-architecture/eShopOnContainers)
