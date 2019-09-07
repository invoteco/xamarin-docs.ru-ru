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
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2019
ms.locfileid: "70760167"
---
# <a name="validation-in-enterprise-apps"></a>Проверка в корпоративных приложениях

Любое приложение, принимающее входные данные от пользователей, должно обеспечить допустимость входных данных. Приложение может, например, проверять наличие входных данных, содержащих только символы в определенном диапазоне, имеет определенную длину или соответствует определенному формату. Без проверки пользователь может предоставить данные, которые приведут к сбою приложения. Проверка применяет бизнес-правила и предотвращает внедрение вредоносных данных злоумышленником.

В контексте шаблона Model-View-ViewModel (MVVM) для проверки данных часто требуется модель или модель представления, чтобы пользователь мог исправить все ошибки проверки. Мобильное приложение eShopOnContainers выполняет синхронную проверку свойств модели View на стороне клиента и уведомляет пользователя о любых ошибках проверки, выделяя элемент управления, содержащий недопустимые данные, и отображая сообщения об ошибках, которые сообщают пользователю. причины недопустимых данных. На рис. 6-1 показаны классы, участвующие в выполнении проверки в мобильном приложении eShopOnContainers.

[Классы проверки в мобильном приложении eShopOnContainers ![(validation-images/validation.png " ")]] (validation-images/validation-large.png#lightbox "Классы проверки в мобильном приложении eShopOnContainers")

**Рис. 6-1**. Классы проверки в мобильном приложении eShopOnContainers

Просмотр свойств модели, требующих проверки, имеет `ValidatableObject<T>`тип, и `ValidatableObject<T>` каждый экземпляр содержит правила проверки, добавленные в его `Validations` свойство. Проверка вызывается из `Validate` модели представления путем вызова метода `ValidatableObject<T>` экземпляра, который получает правила проверки и выполняет их в `ValidatableObject<T>` `Value` отношении свойства. Все ошибки `Errors` проверки помещаются в свойство `ValidatableObject<T>` экземпляра, `ValidatableObject<T>` и `IsValid` свойство экземпляра обновляется, чтобы указать, успешно ли выполнена проверка.

Уведомление об `ExtendedBindableObject` изменении свойства предоставляется классом, [`Entry`](xref:Xamarin.Forms.Entry) поэтому `IsValid` элемент управления может быть `ValidatableObject<T>` привязан к свойству экземпляра в классе модели представления, чтобы получать уведомления о том, являются ли введенные данные допустимыми.

## <a name="specifying-validation-rules"></a>Указание правил проверки

Правила проверки задаются путем создания класса, производного от `IValidationRule<T>` интерфейса, который показан в следующем примере кода:

```csharp
public interface IValidationRule<T>  
{  
    string ValidationMessage { get; set; }  
    bool Check(T value);  
}
```

Этот интерфейс указывает, что класс правила проверки должен предоставлять `boolean` `Check` метод, используемый для выполнения `ValidationMessage` необходимой проверки, и свойство, значение которого является сообщением об ошибке проверки, которое будет отображаться, если Проверка не пройдена.

В следующем примере кода показано `IsNotNullOrEmptyRule<T>` правило проверки, которое используется для проверки имени пользователя и пароля, вводимых пользователем `LoginView` на странице при использовании служб макетирования в мобильном приложении eShopOnContainers:

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

Метод возвращает значение типа `boolean` , указывающее, является `null`ли аргумент значения пустым или содержит только символы пробела. `Check`

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

Метод возвращает значение типа `boolean` , указывающее, является ли аргумент value допустимым адресом электронной почты. `Check` Это достигается путем поиска аргумента value для первого вхождения шаблона регулярного выражения, указанного в `Regex` конструкторе. Можно ли определить шаблон регулярного выражения во входной строке, проверив значение `Match` `Success` свойства объекта.

> [!NOTE]
> Иногда проверка свойства может содержать зависимые свойства. Примером зависимых свойств является то, что набор допустимых значений свойства A зависит от конкретного значения, установленного в свойстве B. Чтобы проверить, что значение свойства A является одним из допустимых значений, будет требовать получения значения свойства б. Кроме того, при изменении значения свойства б необходимо будет повторно проверить свойство A.

## <a name="adding-validation-rules-to-a-property"></a>Добавление правил проверки к свойству

В мобильном приложении eShopOnContainers свойства модели, требующие проверки, объявляются с типом `ValidatableObject<T>`, где `T` — тип проверяемых данных. В следующем примере кода показан пример двух таких свойств:

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

Для выполнения `Validations` проверки необходимо добавить правила проверки в коллекцию каждого `ValidatableObject<T>` экземпляра, как показано в следующем примере кода:

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

Этот метод добавляет `IsNotNullOrEmptyRule<T>` правило проверки `Validations` к коллекции каждого `ValidatableObject<T>` экземпляра `ValidationMessage` , указывая значения для свойства правила проверки, которое указывает сообщение об ошибке проверки, которое будет отображаться, если Проверка не пройдена.

## <a name="triggering-validation"></a>Активация проверки

Метод проверки, используемый в мобильном приложении eShopOnContainers, может вручную активировать проверку свойства и автоматически запускать проверку при изменении свойства.

### <a name="triggering-validation-manually"></a>Активация проверки вручную

Проверку можно запустить вручную для свойства модели представления. Например, это происходит в мобильном приложении eShopOnContainers `LoginView`, когда пользователь нажмет кнопку **входа** на, при использовании служб макетирования. Делегат команды вызывает `MockSignInAsync` метод `LoginViewModel`в, который вызывает проверку, выполняя `Validate` метод, который показан в следующем примере кода:

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

Метод выполняет проверку имени пользователя и пароля `LoginView`, вводимых пользователем на, путем вызова метода Validate для каждого `ValidatableObject<T>` экземпляра. `Validate` В следующем примере кода показан метод Validate из `ValidatableObject<T>` класса:

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

Этот метод очищает `Errors` коллекцию, а затем извлекает все правила проверки, добавленные в `Validations` коллекцию объекта. Выполняется метод для каждого полученного правила проверки, `ValidationMessage` и значение свойства для любого правила проверки, которое не удается проверить `Errors` данные, добавляется `ValidatableObject<T>` в коллекцию экземпляра. `Check` Наконец, `IsValid` задается свойство, и его значение возвращается вызывающему методу, который указывает, завершилась ли проверка успешно или неудачно.

### <a name="triggering-validation-when-properties-change"></a>Активация проверки при изменении свойств

Проверка может также запускаться при изменении привязанного свойства. Например, если двусторонняя привязка в `LoginView` `UserName` задает свойство или `Password` , то проверка активируется. В следующем примере кода показано, как это происходит:

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

`ValidatableObject<T>` `Behaviors` `EventToCommandBehavior` Элемент управления привязывается `UserName.Value` к свойству экземпляра, а в коллекции элемента управления добавляется экземпляр. [`Entry`](xref:Xamarin.Forms.Entry) Это поведение выполняет `ValidateUserNameCommand` в ответ на событие [`TextChanged`], возникающее на элементе `Entry`, `Entry` который создается при изменении текста. В свою очередь, `ValidateUserNameCommand` делегат выполняет `ValidateUserName` метод `Validate` , который выполняет метод в `ValidatableObject<T>` экземпляре. Таким образом, каждый раз, когда пользователь вводит символ в `Entry` элементе управления для имени пользователя, выполняется проверка введенных данных.

Дополнительные сведения о поведении см. в разделе [Реализация поведения](~/xamarin-forms/enterprise-application-patterns/mvvm.md#implementing_behaviors).

<a name="displaying_validation_errors" />

## <a name="displaying-validation-errors"></a>Отображение ошибок проверки

Мобильное приложение eShopOnContainers уведомляет пользователя о любых ошибках проверки, выделяя элемент управления, содержащий недопустимые данные, с красной линией и отображая сообщение об ошибке, сообщающее пользователю, почему данные являются недопустимыми ниже элемента управления, содержащего недопустимые данные. При исправлении недопустимых данных строка изменяется на черный, а сообщение об ошибке удаляется. На рис. 6-2 показано представление LoginView в мобильном приложении eShopOnContainers при наличии ошибок проверки.

![](validation-images/validation-login.png "Отображение ошибок проверки во время входа")

**Рис. 6-2.** Отображение ошибок проверки во время входа

### <a name="highlighting-a-control-that-contains-invalid-data"></a>Выделение элемента управления, содержащего недопустимые данные

Присоединенное поведение используется для выделения [`Entry`](xref:Xamarin.Forms.Entry) элементов управления, в которых произошли ошибки проверки. `LineColorBehavior` В следующем примере кода показано, `LineColorBehavior` как присоединенное поведение прикрепляется `Entry` к элементу управления:

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

[`Entry`](xref:Xamarin.Forms.Entry) Элемент управления использует явный стиль, который показан в следующем примере кода:

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

Этот стиль задает `ApplyLineColor` и `LineColor` присоединенные свойства `LineColorBehavior` вложенного поведения для [`Entry`](xref:Xamarin.Forms.Entry) элемента управления. Дополнительные сведения о стилях см. в статье [Стили](~/xamarin-forms/user-interface/styles/index.md).

Если значение `ApplyLineColor` присоединенного свойства установлено или изменяется, то `LineColorBehavior` `OnApplyLineColorChanged` присоединенное поведение выполняет метод, который показан в следующем примере кода:

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

Параметры этого метода предоставляют экземпляр элемента управления, к которому присоединено поведение, а также старое и новое значения `ApplyLineColor` присоединенного свойства. [`Effects`](xref:Xamarin.Forms.Element.Effects) `ApplyLineColor` `true`Класс добавляется в коллекцию элемента управления, если вложенное свойство имеет значение, в противном случае оно удаляется из `Effects` коллекции элемента управления. `EntryLineColorEffect` Дополнительные сведения о поведении см. в разделе [Реализация поведения](~/xamarin-forms/enterprise-application-patterns/mvvm.md#implementing_behaviors).

Подклассы [`RoutingEffect`](xref:Xamarin.Forms.RoutingEffect) класса и показаны в следующем примере кода: `EntryLineColorEffect`

```csharp
public class EntryLineColorEffect : RoutingEffect  
{  
    public EntryLineColorEffect() : base("eShopOnContainers.EntryLineColorEffect")  
    {  
    }  
}
```

[`RoutingEffect`](xref:Xamarin.Forms.RoutingEffect) Класс представляет независимый от платформы результат, который заключает в оболочку внутренний результат, зависящий от платформы. Это упрощает процесс удаления эффекта, так как отсутствует доступ времени компиляции к сведениям о типе для определяемого платформой эффекта. `EntryLineColorEffect` Метод вызывает конструктор базового класса, передавая параметр, состоящий из объединения имени группы разрешения, и уникального идентификатора, указанного в каждом классе влияния для конкретной платформы.

В следующем примере кода показана `eShopOnContainers.EntryLineColorEffect` реализация для iOS:

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

Метод получает собственный элемент управления для элемента управления Xamarin. Forms [`Entry`](xref:Xamarin.Forms.Entry) и обновляет цвет `UpdateLineColor` линии путем вызова метода. `OnAttached` `LineColor` `Entry` `Entry` [`Height`](xref:Xamarin.Forms.VisualElement.Height) Переопределение реагирует на изменения свойств, которые можно привязать к элементу управления, путем обновления цвета линии при изменении присоединенного свойства или изменения свойства. `OnElementPropertyChanged` Дополнительные сведения об эффектах см. в статье [Эффекты](~/xamarin-forms/app-fundamentals/effects/index.md).

Когда в [`Entry`](xref:Xamarin.Forms.Entry) элементе управления появятся допустимые данные, в нижней части элемента управления будет применена черная линия, указывающая, что нет ошибки проверки. На рисунке 6-3 показан пример этого.

![](validation-images/validation-blackline.png "Черная линия, указывающая на отсутствие ошибки проверки")

**Рис. 6-3**. Черная линия, указывающая на отсутствие ошибки проверки

Элемент управления также [`DataTrigger`](xref:Xamarin.Forms.DataTrigger) добавлен в [`Triggers`](xref:Xamarin.Forms.VisualElement.Triggers) коллекцию. [`Entry`](xref:Xamarin.Forms.Entry) В следующем примере кода показано `DataTrigger`:

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

При [`DataTrigger`](xref:Xamarin.Forms.DataTrigger) этом `LineColorBehavior` `LineColor` [`Setter`](xref:Xamarin.Forms.Setter) `false`отслеживается свойство,и,еслионопринимаетзначение,оновыполняет,чтоизменяетприсоединенноесвойствоприсоединенногоповедениянакрасный.`UserName.IsValid` На рисунке 6-4 показан пример этого.

![](validation-images/validation-redline.png "Красная линия, указывающая на ошибку проверки")

**Рис. 6-4**. Красная линия, указывающая на ошибку проверки

Строка в [`Entry`](xref:Xamarin.Forms.Entry) элементе управления останется красным, тогда как входные данные станут недопустимыми, в противном случае изменится на черный, чтобы указать, что входные данные являются допустимыми.

Дополнительные сведения о триггерах см. в разделе [Triggers](~/xamarin-forms/app-fundamentals/triggers.md).

### <a name="displaying-error-messages"></a>Отображение сообщений об ошибках

Пользовательский интерфейс отображает сообщения об ошибках проверки в элементах управления Label под каждым элементом управления, чьи данные не прошли проверку. В следующем примере кода показано [`Label`](xref:Xamarin.Forms.Label) , где отображается сообщение об ошибке проверки, если пользователь не указал допустимое имя пользователя:

```xaml
<Label Text="{Binding UserName.Errors, Converter={StaticResource FirstValidationErrorConverter}}"  
       Style="{StaticResource ValidationErrorLabelStyle}" />
```

Каждый [`Label`](xref:Xamarin.Forms.Label) привязывается `Errors` к свойству проверяемого объекта модели представления. Свойство предоставляется `ValidatableObject<T>` классом и имеет тип `List<string>`. `Errors` Так как `FirstValidationErrorConverter` свойство может содержать несколько ошибок проверки, экземпляр используется для получения первой ошибки из коллекции для вывода. `Errors`

## <a name="summary"></a>Сводка

Мобильное приложение eShopOnContainers выполняет синхронную проверку свойств модели View на стороне клиента и уведомляет пользователя о любых ошибках проверки, выделяя элемент управления, содержащий недопустимые данные, и отображая сообщения об ошибках, которые сообщают пользователю. Почему данные недопустимы.

Просмотр свойств модели, требующих проверки, имеет `ValidatableObject<T>`тип, и `ValidatableObject<T>` каждый экземпляр содержит правила проверки, добавленные в его `Validations` свойство. Проверка вызывается из `Validate` модели представления путем вызова метода `ValidatableObject<T>` экземпляра, который получает правила проверки и выполняет их в `ValidatableObject<T>` `Value` отношении свойства. Все ошибки `Errors` проверки помещаются в свойство `ValidatableObject<T>`экземпляра, `ValidatableObject<T>` и `IsValid` свойство экземпляра обновляется, чтобы указать, успешно ли выполнена проверка.

## <a name="related-links"></a>Связанные ссылки

- [Скачать электронную книгу (2 МБ в формате PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (пример)](https://github.com/dotnet-architecture/eShopOnContainers)
