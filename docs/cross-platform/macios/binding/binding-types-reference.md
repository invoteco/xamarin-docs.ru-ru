---
title: Справочное руководство по типам привязки
description: В этом справочном руководстве описываются различные атрибуты и понятия, которые необходимо учитывать при C# создании привязок к библиотекам цели-C.
ms.prod: xamarin
ms.assetid: C6618E9D-07FA-4C84-D014-10DAC989E48D
author: davidortinau
ms.author: daortin
ms.date: 03/06/2018
ms.openlocfilehash: e89cbf98dbaf5a96fdfa51069f580b914ba5ff76
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73016305"
---
# <a name="binding-types-reference-guide"></a>Справочное руководство по типам привязки

В этом документе описывается список атрибутов, которые можно использовать для добавления аннотаций к файлам контрактов API для привязки и создания кода.

Контракты API Xamarin. iOS и Xamarin. Mac записываются C# в основном как определения интерфейсов, которые определяют способ отображения кода цели-C C#. Процесс включает в себя сочетание объявлений интерфейса и некоторых базовых определений типов, которые может потребоваться контракту API. Общие сведения о типах привязок см. в нашем прилагаемом справочнике [библиотеки цели-C](~/cross-platform/macios/binding/objective-c-libraries.md).

## <a name="type-definitions"></a>Определения типов

Синтаксис:

```csharp
[BaseType (typeof (BTYPE))
interface MyType : [Protocol1, Protocol2] {
     IntPtr Constructor (string foo);
}
```

Каждый интерфейс в определении контракта имеет атрибут [`[BaseType]`](#BaseTypeAttribute) , объявляющий базовый тип для созданного объекта. В приведенном выше объявлении будет C# создан `MyType` тип класса, который привязывается к типу цели-C с именем`MyType`.

Если вы укажете какие-либо типы после имени TypeName (в примере выше `Protocol1` и `Protocol2`), используя синтаксис наследования интерфейса, содержимое этих интерфейсов будет встроенным, как если бы они были частью контракта для `MyType`.
Способ, с помощью которого Xamarin. iOS предоставляет тип протокола, состоит из встраивания всех методов и свойств, объявленных в протоколе, в сам тип.

Ниже показано, как объявление цели-C для `UITextField` будет определено в контракте Xamarin. iOS:

```objc
@interface UITextField : UIControl <UITextInput> {

}
```

Будет записан подобным образом как контракт C# API:

```csharp
[BaseType (typeof (UIControl))]
interface UITextField : UITextInput {
}
```

Вы можете управлять многими другими аспектами создания кода, применяя другие атрибуты к интерфейсу, а также настроив атрибут [`[BaseType]`](#BaseTypeAttribute) .

### <a name="generating-events"></a>Создание событий

Одной из функций конструкции API Xamarin. iOS и Xamarin. Mac является то, что мы сопоставлены классы делегата C# цели-C как события и обратные вызовы. Пользователи могут выбрать один экземпляр для каждого экземпляра, следует ли применять шаблон программирования "цель-C", назначив свойства, такие как `Delegate` экземпляр класса, который реализует различные методы, вызываемые средой выполнения цели "цель-C", или выбрав C#— события и свойства стиля.

Рассмотрим один пример использования модели цели-C:

```csharp
bool MakeDecision ()
{
    return true;
}

void Setup ()
{
     var scrollView = new UIScrollView (myRect);
     scrollView.Delegate = new MyScrollViewDelegate ();
     ...
}

class MyScrollViewDelegate : UIScrollViewDelegate {
    public override void Scrolled (UIScrollView scrollView)
    {
        Console.WriteLine ("Scrolled");
    }

    public override bool ShouldScrollToTop (UIScrollView scrollView)
    {
        return MakeDecision ();
    }
}
```

В приведенном выше примере можно увидеть, что мы решили переписать два метода, одно уведомление о том, что было выполнено событие прокрутки, а второе — обратный вызов, который должен возвращать логическое значение, указывающее, `scrollView` ли он должен прокручиваться вверх или n наблюдения.

C# Модель позволяет пользователю библиотеки прослушивать уведомления с помощью синтаксиса C# событий или синтаксиса свойств для подключения обратных вызовов, которые должны возвращать значения.

Так выглядит C# код для той же функции с помощью лямбда-выражений:

```csharp
void Setup ()
{
    var scrollview = new UIScrollView (myRect);
    // Event connection, use += and multiple events can be connected
    scrollView.Scrolled += (sender, eventArgs) { Console.WriteLine ("Scrolled"); }

    // Property connection, use = only a single callback can be used
    scrollView.ShouldScrollToTop = (sv) => MakeDecision ();
}
```

Так как события не возвращают значения (они имеют тип возврата void), можно соединить несколько копий. `ShouldScrollToTop` не является событием, вместо него используется свойство с типом `UIScrollViewCondition` с этой сигнатурой:

```csharp
public delegate bool UIScrollViewCondition (UIScrollView scrollView);
```

Он возвращает значение `bool`, в данном случае лямбда-синтаксис позволяет нам просто возвращать значение из функции `MakeDecision`.

Генератор привязок поддерживает создание событий и свойств, связывающих класс, например `UIScrollView`, с его `UIScrollViewDelegate` (назовите этот класс модели). это делается путем аннотирования определения [`[BaseType]`](#BaseTypeAttribute) с помощью параметров `Events` и `Delegates` ( описано ниже). Помимо аннотации [`[BaseType]`](#BaseTypeAttribute) с этими параметрами, необходимо сообщить генератору о нескольких дополнительных компонентах.

Для событий, которые принимают более одного параметра (в определенном случае-C — первый параметр в классе делегата является экземпляром объекта sender), необходимо указать имя, которое будет использоваться для созданного класса `EventArgs`. Это делается с помощью атрибута [`[EventArgs]`](#EventArgsAttribute) в объявлении метода в классе модели. Пример:

```csharp
[BaseType (typeof (UINavigationControllerDelegate))]
[Model][Protocol]
public interface UIImagePickerControllerDelegate {
    [Export ("imagePickerController:didFinishPickingImage:editingInfo:"), EventArgs ("UIImagePickerImagePicked")]
    void FinishedPickingImage (UIImagePickerController picker, UIImage image, NSDictionary editingInfo);
}
```

В приведенном выше объявлении создается класс `UIImagePickerImagePickedEventArgs`, производный от `EventArgs` и Packs обоих параметров, `UIImage` и `NSDictionary`. Генератор создает следующее:

```csharp
public partial class UIImagePickerImagePickedEventArgs : EventArgs {
    public UIImagePickerImagePickedEventArgs (UIImage image, NSDictionary editingInfo);
    public UIImage Image { get; set; }
    public NSDictionary EditingInfo { get; set; }
}
```

Затем он предоставляет следующие классы в классе `UIImagePickerController`:

```csharp
public event EventHandler<UIImagePickerImagePickedEventArgs> FinishedPickingImage { add; remove; }
```

Методы модели, возвращающие значение, привязаны по-разному. Для них требуется как имя созданного C# делегата (сигнатура метода), так и значение по умолчанию, возвращаемое в случае, если пользователь не предоставляет самому реализацию. Например, определение `ShouldScrollToTop`:

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface UIScrollViewDelegate {
    [Export ("scrollViewShouldScrollToTop:"), DelegateName ("UIScrollViewCondition"), DefaultValue ("true")]
    bool ShouldScrollToTop (UIScrollView scrollView);
}
```

Приведенный выше объект создает `UIScrollViewCondition` делегат с сигнатурой, показанной выше, и если пользователь не предоставляет реализацию, возвращаемое значение будет равно true.

Помимо атрибута [`[DefaultValue]`](#DefaultValueAttribute) можно также использовать атрибут [`[DefaultValueFromArgument]`](#DefaultValueFromArgumentAttribute) , который направляет генератор для возврата значения указанного параметра в вызове или параметра [`[NoDefaultValue]`](#NoDefaultValueAttribute) , который указывает генератору на наличие нет значения по умолчанию.

<a name="BaseTypeAttribute" />

### <a name="basetypeattribute"></a>басетипеаттрибуте

Синтаксис:

```csharp
public class BaseTypeAttribute : Attribute {
        public BaseTypeAttribute (Type t);

        // Properties
        public Type BaseType { get; set; }
        public string Name { get; set; }
        public Type [] Events { get; set; }
        public string [] Delegates { get; set; }
        public string KeepRefUntil { get; set; }
}
```

#### <a name="basetypename"></a>BaseType.Name

Свойство `Name` используется для управления именем, к которому будет привязан этот тип в мире цели-C. Обычно это используется для присвоения C# типу имени, которое соответствует рекомендациям по проектированию .NET Framework, но сопоставляется с именем в цели-C, которое не соответствует этому соглашению.

Например, в следующем примере тип `NSURLConnection` цели-C сопоставляется с `NSUrlConnection`, так как в .NET Framework рекомендации по проектированию используют URL-адрес вместо URL-адреса:

```csharp
[BaseType (typeof (NSObject), Name="NSURLConnection")]
interface NSUrlConnection {
}
```

Указанное имя используется в качестве значения для созданного атрибута `[Register]` в привязке. Если параметр `Name` не указан, используется короткое имя типа в качестве значения атрибута `[Register]` в создаваемом выводе.

#### <a name="basetypeevents-and-basetypedelegates"></a>BaseType. Events и BaseType. delegates

Эти свойства используются для того, чтобы создавать события C#типа создания в создаваемых классах. Они используются для связывания заданного класса с его классом делегата объектив-C. В большинстве случаев, когда класс использует класс делегата для отправки уведомлений и событий. Например, `BarcodeScanner` будет иметь сопутствующий `BardodeScannerDelegate` класс. Класс `BarcodeScanner` обычно имеет свойство `Delegate`, которое назначает экземпляру `BarcodeScannerDelegate`, тогда как это работает, может потребоваться предоставить пользователям C#интерфейс событий стиля, аналогичный, и в этих случаях использовать`Events`и`Delegates`свойства атрибута [`[BaseType]`](#BaseTypeAttribute) .

Эти свойства всегда устанавливаются вместе и должны иметь одинаковое количество элементов и храниться в синхронизации. Массив `Delegates` содержит одну строку для каждого слабо типизированного делегата, который требуется перенести, а массив `Events` содержит по одному типу для каждого типа, который необходимо связать с ним.

```csharp
[BaseType (typeof (NSObject),
           Delegates=new string [] { "WeakDelegate" },
           Events=new Type [] {typeof(UIAccelerometerDelegate)})]
public interface UIAccelerometer {
}

[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface UIAccelerometerDelegate {
}
```

#### <a name="basetypekeeprefuntil"></a>BaseType. Кипрефунтил

Если этот атрибут применяется при создании новых экземпляров этого класса, то экземпляр этого объекта будет храниться до вызова метода, на который ссылается `KeepRefUntil`. Это полезно для повышения удобства использования API-интерфейсов, когда нежелательно, чтобы пользователь оставался ссылкой на объект для использования вашего кода. Значением этого свойства является имя метода в классе `Delegate`, поэтому его необходимо использовать в сочетании со свойствами `Events` и `Delegates`.

В следующем примере показано, как это используется `UIActionSheet` в Xamarin. iOS:

```csharp
[BaseType (typeof (NSObject), KeepRefUntil="Dismissed")]
[BaseType (typeof (UIView),
           KeepRefUntil="Dismissed",
           Delegates=new string [] { "WeakDelegate" },
           Events=new Type [] {typeof(UIActionSheetDelegate)})]
public interface UIActionSheet {
}

[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface UIActionSheetDelegate {
    [Export ("actionSheet:didDismissWithButtonIndex:"), EventArgs ("UIButton")]
    void Dismissed (UIActionSheet actionSheet, nint buttonIndex);
}
```

<a name="DesignatedDefaultCtorAttribute" />

### <a name="designateddefaultctorattribute"></a>десигнатеддефаултктораттрибуте

Если этот атрибут применяется к определению интерфейса, он создаст атрибут `[DesignatedInitializer]` для конструктора по умолчанию (созданного), который сопоставляется с селектором `init`.

<a name="DisableDefaultCtorAttribute" />

### <a name="disabledefaultctorattribute"></a>дисабледефаултктораттрибуте

Если этот атрибут применяется к определению интерфейса, он не позволит генератору создавать конструктор по умолчанию.

Используйте этот атрибут, если требуется инициализировать объект с помощью одного из других конструкторов в классе.

<a name="PrivateDefaultCtorAttribute" />

### <a name="privatedefaultctorattribute"></a>приватедефаултктораттрибуте

Если этот атрибут применяется к определению интерфейса, конструктор по умолчанию будет помечен как частный. Это означает, что вы все равно можете создать экземпляр объекта этого класса на внутреннем уровне из файла расширения, но он просто не будет доступен пользователям вашего класса.

<a name="CategoryAttribute" />

### <a name="categoryattribute"></a>категоряттрибуте

Используйте этот атрибут для определения типа, чтобы привязать категории цели-C и предоставить эти методы C# расширения для отражения того, как цель-c предоставляет функциональные возможности.

Категории — это механизм цели-C, используемый для расширения набора методов и свойств, доступных в классе.   На практике они используются для расширения функциональных возможностей базового класса (например `NSObject`), когда определена связанная платформа (например, `UIKit`), делая их метод доступными, но только если новая платформа связана.   В некоторых других случаях они используются для упорядочения функций в классе по функциональным возможностям.   Они похожи на C# методы расширения.

Вот как будет выглядеть Категория в цели-C:

```objc
@interface UIView (MyUIViewExtension)
-(void) makeBackgroundRed;
@end
```

Приведенный выше пример находится в библиотеке, которая может расширить экземпляры `UIView` с помощью метода `makeBackgroundRed`.

Чтобы привязать их, можно использовать атрибут [`[Category]`](#CategoryAttribute) в определении интерфейса.   При использовании атрибута [`[Category]`](#CategoryAttribute) значение атрибута [`[BaseType]`](#BaseTypeAttribute) меняется с используемого, чтобы указать расширяемый базовый класс, чтобы тип был расширен.

Ниже показано, как привязываются расширения `UIView` и включены в C# методы расширения:

```csharp
[BaseType (typeof (UIView))]
[Category]
interface MyUIViewExtension {
    [Export ("makeBackgroundRed")]
    void MakeBackgroundRed ();
}
```

Выше будет создан `MyUIViewExtension` класс, содержащий метод расширения `MakeBackgroundRed`.   Это означает, что теперь можно вызывать `MakeBackgroundRed` для любого подкласса `UIView`, обеспечивая те же функциональные возможности, что и на уровне цели-C.

В некоторых случаях **статические** члены можно найти в категориях, как в следующем примере:

```objc
@interface FooObject (MyFooObjectExtension)
+ (BOOL)boolMethod:(NSRange *)range;
@end
```

Это приводит к **неправильному** определению интерфейса категории C# :

```csharp
[Category]
[BaseType (typeof (FooObject))]
interface FooObject_Extensions {

    // Incorrect Interface definition
    [Static]
    [Export ("boolMethod:")]
    bool BoolMethod (NSRange range);
}
```

Это неверно, поскольку для использования расширения `BoolMethod` требуется экземпляр `FooObject` но привязка **статического** расширения ObjC является побочным действием из-за того, как C# реализуются методы расширения.

Единственный способ использовать приведенные выше определения — это следующий некрасивой код:

```csharp
(null as FooObject).BoolMethod (range);
```

Эта рекомендация заключается в встраивании определения `BoolMethod` в само определение интерфейса `FooObject`, что позволяет вызывать это расширение так, как оно предназначено `FooObject.BoolMethod (range)`.

```csharp
[BaseType (typeof (NSObject))]
interface FooObject {

    [Static]
    [Export ("boolMethod:")]
    bool BoolMethod (NSRange range);
}
```

При обнаружении элемента [`[Static]`](#StaticAttribute) в определении [`[Category]`](#CategoryAttribute) будет выдаваться предупреждение (BI1117). Если вы действительно хотите иметь [`[Static]`](#StaticAttribute) членов в определениях [`[Category]`](#CategoryAttribute) , вы можете выдавать предупреждение с помощью `[Category (allowStaticMembers: true)]` или путем оформления определения интерфейса или [`[Category]`](#CategoryAttribute) с [`[Internal]`](#InternalAttribute).

<a name="StaticAttribute_Class" />

### <a name="staticattribute"></a>статикаттрибуте

Если этот атрибут применяется к классу, он просто создает статический класс, который не является производным от `NSObject`, поэтому атрибут [`[BaseType]`](#BaseTypeAttribute) игнорируется. Статические классы используются для размещения открытых переменных C, которые необходимо предоставить.

Пример:

```csharp
[Static]
interface CBAdvertisement {
    [Field ("CBAdvertisementDataServiceUUIDsKey")]
    NSString DataServiceUUIDsKey { get; }
```

Создаст C# класс со следующим API:

```csharp
public partial class CBAdvertisement  {
    public static NSString DataServiceUUIDsKey { get; }
}
```

## <a name="protocolmodel-definitions"></a>Определения протоколов и моделей

Модели обычно используются реализацией протокола.
Они отличаются тем, что среда выполнения будет зарегистрирована только с целью — с, фактически перезаписанные методы.
В противном случае метод не будет зарегистрирован.

Это в общем случае означает, что при подкласс класса, помеченного `ModelAttribute`, не следует вызывать базовый метод.   При вызове этого метода будет выдано исключение, поэтому необходимо реализовать все поведение подкласса для всех переопределяемых методов.

<a name="AbstractAttribute" />

### <a name="abstractattribute"></a>абстрактаттрибуте

По умолчанию члены, являющиеся частью протокола, не являются обязательными. Это позволяет пользователям создавать подкласс объекта `Model`, просто производя от класса в C# и переопределяя только те методы, которые им интересуют. Иногда контракт цели-C требует, чтобы пользователь выдает реализацию для этого метода (они помечаются директивой `@required` в цели-C). В таких случаях следует помечать эти методы атрибутом `[Abstract]`.

Атрибут `[Abstract]` может применяться к методам или свойствам и приводит к тому, что генератор помечает созданный член как абстрактный, а класс — абстрактным классом.

Из Xamarin. iOS берется следующее:

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface UITableViewDataSource {
    [Export ("tableView:numberOfRowsInSection:")]
    [Abstract]
    nint RowsInSection (UITableView tableView, nint section);
}
```

<a name="DefaultValueAttribute" />

### <a name="defaultvalueattribute"></a>DefaultValueAttribute

Указывает значение по умолчанию, возвращаемое методом модели, если пользователь не предоставляет метод для этого конкретного метода в объекте Model.

Синтаксис:

```csharp
public class DefaultValueAttribute : Attribute {
        public DefaultValueAttribute (object o);
        public object Default { get; set; }
}
```

Например, в следующем мнимом классе делегата для `Camera` класса мы предоставляем `ShouldUploadToServer`, который будет представлен как свойство в классе `Camera`. Если пользователь класса `Camera` явно не устанавливает значение лямбда-выражения, которое может принимать значение true или false, в этом случае возвращается значение false, а значение, указанное в атрибуте `DefaultValue` :

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
interface CameraDelegate {
    [Export ("camera:shouldPromptForAction:"), DefaultValue (false)]
    bool ShouldUploadToServer (Camera camera, CameraAction action);
}
```

Если пользователь задает обработчик в мнимом классе, это значение будет пропущено:

```csharp
var camera = new Camera ();
camera.ShouldUploadToServer = (camera, action) => return SomeDecision ();
```

См. также: [`[NoDefaultValue]`](#NoDefaultValueAttribute), [`[DefaultValueFromArgument]`](#DefaultValueFromArgumentAttribute).

<a name="DefaultValueFromArgumentAttribute" />

### <a name="defaultvaluefromargumentattribute"></a>дефаултвалуефромаргументаттрибуте

Синтаксис:

```csharp
public class DefaultValueFromArgumentAttribute : Attribute {
    public DefaultValueFromArgumentAttribute (string argument);
    public string Argument { get; }
}
```

Этот атрибут, если он указан в методе, возвращающем значение класса Model, даст генератору инструкцию вернуть значение указанного параметра, если пользователь не предоставил собственный метод или лямбда-выражение.

Пример

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface NSAnimationDelegate {
    [Export ("animation:valueForProgress:"), DelegateName ("NSAnimationProgress"), DefaultValueFromArgumentAttribute ("progress")]
    float ComputeAnimationCurve (NSAnimation animation, nfloat progress);
}
```

В приведенном выше случае, если пользователь класса `NSAnimation` выбрал использование любого из C# событий или свойств и не задал`NSAnimation.ComputeAnimationCurve`для метода или лямбда-выражения, возвращаемое значение будет значением, переданным в параметре Progress.

См. также: [`[NoDefaultValue]`](#NoDefaultValueAttribute), [`[DefaultValue]`](#DefaultValueAttribute)

### <a name="ignoredindelegateattribute"></a>игнорединделегатеаттрибуте

Иногда имеет смысл не предоставлять свойство события или делегата из класса Model в класс Host, поэтому добавление этого атрибута повлечет за собой указание генератору избегать создания любого метода, декорированного с помощью.

```csharp
[BaseType (typeof (UINavigationControllerDelegate))]
[Model][Protocol]
public interface UIImagePickerControllerDelegate {
    [Export ("imagePickerController:didFinishPickingImage:editingInfo:"), EventArgs ("UIImagePickerImagePicked")]
    void FinishedPickingImage (UIImagePickerController picker, UIImage image, NSDictionary editingInfo);

    [Export ("imagePickerController:didFinishPickingImage:"), IgnoredInDelegate)] // No event generated for this method
    void FinishedPickingImage (UIImagePickerController picker, UIImage image);
}
```

### <a name="delegatenameattribute"></a>делегатенамеаттрибуте

Этот атрибут используется в методах модели, возвращающих значения, чтобы задать имя сигнатуры делегата для использования.

Пример

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface NSAnimationDelegate {
    [Export ("animation:valueForProgress:"), DelegateName ("NSAnimationProgress"), DefaultValueFromArgumentAttribute ("progress")]
    float ComputeAnimationCurve (NSAnimation animation, float progress);
}
```

В приведенном выше определении генератор выдаст следующее общедоступное объявление:

```csharp
public delegate float NSAnimationProgress (MonoMac.AppKit.NSAnimation animation, float progress);
```

### <a name="delegateapinameattribute"></a>делегатеапинамеаттрибуте

Этот атрибут используется, чтобы позволить генератору изменять имя свойства, созданного в классе Host. Иногда бывает полезно, когда имя метода класса Фуделегате имеет смысл для класса делегата, но оно будет выглядеть нечетным в классе Host как свойство.

Кроме того, это действительно полезно (и требуется) при наличии двух или более методов перегрузки, которые имеют смысл обеспечить их имена как есть в классе Фуделегате, но вы хотите предоставить их в классе Host с более подходящим именем.

Пример

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface NSAnimationDelegate {
    [Export ("animation:valueForProgress:"), DelegateApiName ("ComputeAnimationCurve"), DelegateName ("Func<NSAnimation, float, float>"), DefaultValueFromArgument ("progress")]
    float GetValueForProgress (NSAnimation animation, float progress);
}
```

В приведенном выше определении генератор выдает следующее общедоступное объявление в классе Host:

```csharp
public Func<NSAnimation, float, float> ComputeAnimationCurve { get; set; }
```

<a name="EventArgsAttribute" />

### <a name="eventargsattribute"></a>евентаргсаттрибуте

Для событий, которые принимают более одного параметра (в определенном случае-C — первый параметр в классе делегата является экземпляром объекта отправителя), необходимо предоставить имя, которое будет использоваться для созданного класса EventArgs. Это делается с помощью атрибута `[EventArgs]` в объявлении метода в классе `Model`.

Пример:

```csharp
[BaseType (typeof (UINavigationControllerDelegate))]
[Model][Protocol]
public interface UIImagePickerControllerDelegate {
    [Export ("imagePickerController:didFinishPickingImage:editingInfo:"), EventArgs ("UIImagePickerImagePicked")]
    void FinishedPickingImage (UIImagePickerController picker, UIImage image, NSDictionary editingInfo);
}
```

В приведенном выше объявлении создается класс `UIImagePickerImagePickedEventArgs`, производный от EventArgs и упаковывает оба параметра, `UIImage` и `NSDictionary`. Генератор создает следующее:

```csharp
public partial class UIImagePickerImagePickedEventArgs : EventArgs {
    public UIImagePickerImagePickedEventArgs (UIImage image, NSDictionary editingInfo);
    public UIImage Image { get; set; }
    public NSDictionary EditingInfo { get; set; }
}
```

Затем он предоставляет следующие классы в классе `UIImagePickerController`:

```csharp
public event EventHandler<UIImagePickerImagePickedEventArgs> FinishedPickingImage { add; remove; }
```

### <a name="eventnameattribute"></a>евентнамеаттрибуте

Этот атрибут используется, чтобы позволить генератору изменять имя события или свойства, сформированного в классе. Иногда бывает полезно, когда имя метода класса модели имеет смысл для класса Model, но будет выглядеть нечетным в исходном классе как событие или свойство.

Например, `UIWebView` использует следующий бит из `UIWebViewDelegate`:

```csharp
[Export ("webViewDidFinishLoad:"), EventArgs ("UIWebView"), EventName ("LoadFinished")]
void LoadingFinished (UIWebView webView);
```

Приведенный выше объект предоставляет `LoadingFinished` в качестве метода в `UIWebViewDelegate`, но `LoadFinished` как событие для подключения к в `UIWebView`:

```csharp
var webView = new UIWebView (...);
webView.LoadFinished += delegate { Console.WriteLine ("done!"); }
```

<a name="ModelAttribute" />

### <a name="modelattribute"></a>ModelAttribute

При применении атрибута `[Model]` к определению типа в API контракта среда выполнения создаст специальный код, который будет отображать только вызовы к методам в классе, если пользователь перезаписал метод в классе. Этот атрибут обычно применяется ко всем интерфейсам API, которые создают оболочку для класса делегата цели-C.

<a name="NoDefaultValueAttribute" />

### <a name="nodefaultvalueattribute"></a>нодефаултвалуеаттрибуте

Указывает, что метод в модели не предоставляет возвращаемое значение по умолчанию.

Это работает с исполняющей средой цели-C, реагируя `false` в запрос выполнения целевой-C, чтобы определить, реализован ли указанный селектор в этом классе.

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
interface CameraDelegate {
    [Export ("shouldDisplayPopup"), NoDefaultValue]
    bool ShouldUploadToServer ();
}
```

См. также: [`[DefaultValue]`](#DefaultValueAttribute), [`[DefaultValueFromArgument]`](#DefaultValueFromArgumentAttribute)  

<a name="ProtocolAttribute" />

## <a name="protocols"></a>Протоколы

Концепция протокола цели-C на самом деле не существует в C#. Протоколы похожи на C# интерфейсы, но они отличаются тем, что не все методы и свойства, объявленные в протоколе, должны быть реализованы классом, который его использует. Вместо этого некоторые методы и свойства являются необязательными.

Некоторые протоколы обычно используются в качестве классов модели, они должны быть привязаны с помощью атрибута [`[Model]`](#ModelAttribute) .

```csharp
[BaseType (typeof (NSObject))]
[Model, Protocol]
interface MyProtocol {
    // Use [Abstract] when the method is defined in the @required section
    // of the protocol definition in Objective-C
    [Abstract]
    [Export ("say:")]
    void Say (string msg);

    [Export ("listen")]
    void Listen ();
}
```

Начиная с Xamarin. iOS 7,0, включена новая и улучшенная функциональность привязки протоколов.  Любое определение, содержащее атрибут `[Protocol]`, на самом деле создает три поддерживающих класса, что значительно улучшает способ использования протоколов:

```csharp
// Full method implementation, contains all methods
class MyProtocol : IMyProtocol {
    public void Say (string msg);
    public void Listen (string msg);
}

// Interface that contains only the required methods
interface IMyProtocol: INativeObject, IDisposable {
    [Export ("say:")]
    void Say (string msg);
}

// Extension methods
static class IMyProtocol_Extensions {
    public static void Optional (this IMyProtocol this, string msg);
    }
}
```

**Реализация класса** предоставляет полный абстрактный класс, который позволяет переопределять отдельные методы и получать полную безопасность типов. Но из- C# за отсутствия поддержки множественного наследования существуют сценарии, в которых может потребоваться другой базовый класс, но по-прежнему требуется реализовать интерфейс.

Именно здесь появляется созданное **Определение интерфейса** .  Это интерфейс, который имеет все необходимые методы из протокола.  Это позволяет разработчикам, которые хотят реализовать ваш протокол, просто реализовать интерфейс.  Среда выполнения автоматически зарегистрирует тип в качестве внедрения протокола.

Обратите внимание, что интерфейс содержит только необходимые методы и предоставляет необязательные методы.   Это означает, что классы, применяющие протокол, получат полную проверку типа для требуемых методов, но придется прибегнуть к нестрогой типизации (вручную с помощью атрибутов экспорта и соответствующей сигнатуре) для необязательных методов протокола.

Для удобства использования API, использующего протоколы, средство привязки также создаст класс метода Extensions, который предоставляет все необязательные методы.   Это означает, что пока вы используете API, вы сможете рассматривать протоколы как все методы.

Если вы хотите использовать определения протоколов в API, вам потребуется написать скелет пустых интерфейсов в определении API.   Если вы хотите использовать Мипротокол в API, необходимо сделать следующее:

```csharp
[BaseType (typeof (NSObject))]
[Model, Protocol]
interface MyProtocol {
    // Use [Abstract] when the method is defined in the @required section
    // of the protocol definition in Objective-C
    [Abstract]
    [Export ("say:")]
    void Say (string msg);

    [Export ("listen")]
    void Listen ();
}

interface IMyProtocol {}

[BaseType (typeof(NSObject))]
interface MyTool {
    [Export ("getProtocol")]
    IMyProtocol GetProtocol ();
}
```

Выше требуется, так как во время привязки `IMyProtocol` не существовало, поэтому необходимо предоставить пустой интерфейс.

### <a name="adopting-protocol-generated-interfaces"></a>Внедрение интерфейсов, созданных протоколом

Каждый раз при реализации одного из интерфейсов, созданных для протоколов, например:

```csharp
class MyDelegate : NSObject, IUITableViewDelegate {
    nint IUITableViewDelegate.GetRowHeight (nint row) {
        return 1;
    }
}
```

Реализация методов интерфейса автоматически экспортируется с соответствующим именем, поэтому она эквивалентна следующей:

```csharp
class MyDelegate : NSObject, IUITableViewDelegate {
    [Export ("getRowHeight:")]
    nint IUITableViewDelegate.GetRowHeight (nint row) {
        return 1;
    }
}
```

Не имеет значения, реализован ли интерфейс неявно или явно.

### <a name="protocol-inlining"></a>Встраивание протокола

При привязке существующих типов цели-C, объявленных как внедрение протокола, необходимо напрямую подставляемь протокол. Для этого просто объявите свой протокол как интерфейс без атрибута [`[BaseType]`](#BaseTypeAttribute) и перечислите протокол в списке базовых интерфейсов для интерфейса.

Пример

```csharp
interface SpeakProtocol {
    [Export ("say:")]
    void Say (string msg);
}

[BaseType (typeof (NSObject))]
interface Robot : SpeakProtocol {
    [Export ("awake")]
    bool Awake { get; set; }
}
```

## <a name="member-definitions"></a>Определения элементов

Атрибуты в этом разделе применяются к отдельным элементам типа: свойства и объявления методов.

### <a name="alignattribute"></a>алигнаттрибуте

Используется для указания значения выравнивания для возвращаемых типов свойств. Некоторые свойства принимают указатели на адреса, которые должны быть согласованы по определенным границам (в Xamarin. iOS это происходит, например, с некоторыми `GLKBaseEffect` свойствами, которые должны быть выделены 16 байтами). Это свойство можно использовать для украшения метода получения и использования значения выравнивания. Обычно это используется с типами `OpenTK.Vector4` и `OpenTK.Matrix4` при интеграции с API-интерфейсами цели.

Пример

```csharp
public interface GLKBaseEffect {
    [Export ("constantColor")]
    Vector4 ConstantColor { [Align (16)] get; set;  }
}
```

### <a name="appearanceattribute"></a>аппеаранцеаттрибуте

Атрибут `[Appearance]` ограничивается iOS 5, где появился диспетчер оформления.

Атрибут `[Appearance]` можно применить к любому методу или свойству, участвующим в инфраструктуре `UIAppearance`. Если этот атрибут применяется к методу или свойству в классе, он направляет генератору привязки запрос на создание строго типизированного класса внешнего вида, который используется для стиля всех экземпляров этого класса, или экземпляров, соответствующих определенным критериям.

Пример

```csharp
public interface UIToolbar {
    [Since (5,0)]
    [Export ("setBackgroundImage:forToolbarPosition:barMetrics:")]
    [Appearance]
    void SetBackgroundImage (UIImage backgroundImage, UIToolbarPosition position, UIBarMetrics barMetrics);

    [Since (5,0)]
    [Export ("backgroundImageForToolbarPosition:barMetrics:")]
    [Appearance]
    UIImage GetBackgroundImage (UIToolbarPosition position, UIBarMetrics barMetrics);
}
```

В приведенном выше коде в Уитулбар будет создан следующий код:

```csharp
public partial class UIToolbar {
    public partial class UIToolbarAppearance : UIView.UIViewAppearance {
        public virtual void SetBackgroundImage (UIImage backgroundImage, UIToolbarPosition position, UIBarMetrics barMetrics);
        public virtual UIImage GetBackgroundImage (UIToolbarPosition position, UIBarMetrics barMetrics)
    }
    public static new UIToolbarAppearance Appearance { get; }
    public static new UIToolbarAppearance AppearanceWhenContainedIn (params Type [] containers);
}
```

### <a name="autoreleaseattribute-xamarinios-54"></a>Ауторелеасеаттрибуте (Xamarin. iOS 5,4)

Используйте `[AutoReleaseAttribute]` для методов и свойств, чтобы заключить вызов метода в метод в `NSAutoReleasePool`.

В цели-C есть некоторые методы, возвращающие значения, добавляемые к `NSAutoReleasePool`по умолчанию. По умолчанию они переходят в поток `NSAutoReleasePool`, но, так как Xamarin. iOS также сохраняет ссылку на объекты, пока находится управляемый объект, может не потребоваться хранить в `NSAutoReleasePool` дополнительные ссылки, которые будут остановлены только до возвращения потока на следующий поток или вернитесь к главному циклу.

Этот атрибут применяется к примерам с большими свойствами (например `UIImage.FromFile`), которые возвращают объекты, добавленные в `NSAutoReleasePool`по умолчанию. Без этого атрибута изображения будут храниться, пока поток не вернет управление в главный цикл. УФ ваш поток — это какой-то фоновый загрузчик, который всегда активен и ожидает работы, изображения никогда не освобождаются.

### <a name="forcedtypeattribute"></a>форцедтипеаттрибуте

`[ForcedTypeAttribute]` используется для принудительного создания управляемого типа, даже если возвращаемый неуправляемый объект не соответствует типу, описанному в определении привязки.

Это полезно, если тип, описанный в заголовке, не соответствует возвращаемому типу собственного метода, например, принимать следующее определение цели-C из `NSURLSession`:

`- (NSURLSessionDownloadTask *)downloadTaskWithRequest:(NSURLRequest *)request`

Ясно, что он возвращает экземпляр `NSURLSessionDownloadTask`, но в нем **возвращается** `NSURLSessionTask`, который является суперклассом и, таким образом, не преобразуется в `NSURLSessionDownloadTask`. Так как мы работаем с типобезопасным контекстом, `InvalidCastException` произойдет.

Чтобы обеспечить соответствие описанию заголовка и избежать `InvalidCastException`, используется `[ForcedTypeAttribute]`.

```csharp
[BaseType (typeof (NSObject), Name="NSURLSession")]
interface NSUrlSession {

    [Export ("downloadTaskWithRequest:")]
    [return: ForcedType]
    NSUrlSessionDownloadTask CreateDownloadTask (NSUrlRequest request);
}
```

`[ForcedTypeAttribute]` также принимает логическое значение с именем `Owns`, которое `false` `[ForcedType (owns: true)]`по умолчанию. Владелец параметра используется для отслеживания [политики владения](https://developer.apple.com/library/content/documentation/CoreFoundation/Conceptual/CFMemoryMgmt/Concepts/Ownership.html) объектами **Core Foundation** .

`[ForcedTypeAttribute]` допустимы только для параметров, свойств и возвращаемого значения.

<a name="BindAsAttribute" />

### <a name="bindasattribute"></a>биндасаттрибуте

`[BindAsAttribute]` допускает привязку `NSNumber`, `NSValue` и `NSString`(enums) в более C# точные типы. Атрибут можно использовать для создания лучшего, более точного, API .NET через собственный API.

Можно оформлять методы (в возвращаемом значении), параметры и свойства с помощью `BindAs`. Единственное ограничение заключается в том, что член **не должен** находиться внутри `[Protocol]` или интерфейса [`[Model]`](#ModelAttribute) .

Пример:

```csharp
[return: BindAs (typeof (bool?))]
[Export ("shouldDrawAt:")]
NSNumber ShouldDraw ([BindAs (typeof (CGRect))] NSValue rect);
```

Выводит:

```csharp
[Export ("shouldDrawAt:")]
bool? ShouldDraw (CGRect rect) { ... }
```

На внутреннем уровне выполняется преобразование `bool?` <-> `NSNumber` и `CGRect` <-> `NSValue`.

Текущие поддерживаемые типы инкапсуляции:

* `NSValue`
* `NSNumber`
* `NSString`

#### <a name="nsvalue"></a>нсвалуе

Следующие C# типы данных поддерживаются для инкапсуляции из`NSValue`:

* кгаффинетрансформ
* нсранже
* кгвектор
* SCNMatrix4
* CLLocationCoordinate2D
* SCNVector3
* SCNVector4
* Кгпоинт/PointF
* Кгрект/Ректанглеф
* Кгсизе/SizeF
* уиеджеинсетс
* уиоффсет
* мккурдинатеспан
* кмтимеранже
* кмтиме
* кмтимемаппинг
* CATransform3D

#### <a name="nsnumber"></a>нснумбер

Следующие C# типы данных поддерживаются для инкапсуляции из`NSNumber`:

* bool
* byte
* двойная
* плавающее
* short
* int
* long
* sbyte
* ushort
* uint
* ulong
* нфлоат
* нинт
* нуинт
* перечислениям;

#### <a name="nsstring"></a>NSString

[`[BindAs]`](#BindAsAttribute) работает в конжунтион с [перечислениями, поддерживающими константу NSString](#enum-attributes) , чтобы можно было создать более эффективный API .NET, например:

```csharp
[BindAs (typeof (CAScroll))]
[Export ("supportedScrollMode")]
NSString SupportedScrollMode { get; set; }
```

Выводит:

```csharp
[Export ("supportedScrollMode")]
CAScroll SupportedScrollMode { get; set; }
```

Мы обработаем `enum` <-> `NSString` преобразование, только если предоставленный тип перечисления [`[BindAs]`](#BindAsAttribute) поддерживается [константой NSString](#enum-attributes).

#### <a name="arrays"></a>Массивы

[`[BindAs]`](#BindAsAttribute) также поддерживает массивы любого из поддерживаемых типов, в качестве примера можно использовать следующее определение API:

```csharp
[return: BindAs (typeof (CAScroll []))]
[Export ("getScrollModesAt:")]
NSString [] GetScrollModes ([BindAs (typeof (CGRect []))] NSValue [] rects);
```

Выводит:

```csharp
[Export ("getScrollModesAt:")]
CAScroll? [] GetScrollModes (CGRect [] rects) { ... }
```

Параметр `rects` будет инкапсулирован в `NSArray`, который содержит `NSValue` для каждого `CGRect` и в Return будет получен массив `CAScroll?`, который был создан с использованием значений возвращаемого `NSArray`, содержащего `NSStrings`.

<a name="BindAttribute" />

### <a name="bindattribute"></a>биндаттрибуте

Атрибут `[Bind]` имеет два применения, если он применяется к объявлению метода или свойства, а другой — к отдельному методу получения или задания в свойстве.

При использовании для метода или свойства результатом атрибута `[Bind]` является создание метода, который вызывает указанный селектор. Но созданный метод не снабжен атрибутом [`[Export]`](#ExportAttribute) , что означает, что он не может участвовать в переопределении метода. Обычно это используется в сочетании с атрибутом `[Target]` для реализации методов расширения цели-C.

Пример:

```csharp
public interface UIView {
    [Bind ("drawAtPoint:withFont:")]
    SizeF DrawString ([Target] string str, CGPoint point, UIFont font);
}
```

При использовании в методах считывания или Setter атрибут `[Bind]` используется для изменения значений по умолчанию, выводимых генератором кода при создании имен селектора и задания Setter-C для свойства. По умолчанию при пометке свойства с именем `fooBar`генератор создаст `fooBar` экспорта для метода получения и `setFooBar:` для метода задания. В некоторых случаях цель-C не соответствует этому соглашению, обычно они меняют имя метода получения на `isFooBar`.
Этот атрибут используется для информирования генератора об этом.

Пример:

```csharp
// Default behavior
[Export ("active")]
bool Active { get; set; }

// Custom naming with the Bind attribute
[Export ("visible")]
bool Visible { [Bind ("isVisible")] get; set; }
```

<a name="AsyncAttribute" />

### <a name="asyncattribute"></a>асинкаттрибуте

Доступно только в Xamarin. iOS 6,3 и более поздних версиях.

Этот атрибут может применяться к методам, принимающим обработчик завершения в качестве последнего аргумента.

Атрибут `[Async]` можно использовать для методов, последний аргумент которых является обратным вызовом.  При применении этого метода к методу генератор привязок создаст версию этого метода с суффиксом `Async`.  Если обратный вызов не принимает параметров, возвращаемое значение будет `Task`, если обратный вызов принимает параметр, то результатом будет `Task<T>`.

```csharp
[Export ("upload:complete:")]
[Async]
void LoadFile (string file, NSAction complete)
```

Следующий асинхронный метод будет создан следующим образом:

```csharp
Task LoadFileAsync (string file);
```

Если обратный вызов принимает несколько параметров, следует задать `ResultType` или `ResultTypeName`, чтобы указать нужное имя созданного типа, который будет содержать все свойства.

```csharp
delegate void OnComplete (string [] files, nint byteCount);

[Export ("upload:complete:")]
[Async (ResultTypeName="FileLoading")]
void LoadFiles (string file, OnComplete complete)
```

Далее будет создан этот асинхронный метод, где `FileLoading` содержит свойства для доступа к `files` и `byteCount`:

```csharp
Task<FileLoading> LoadFile (string file);
```

Если последний параметр обратного вызова является `NSError`, то созданный метод `Async` будет проверять, не является ли значение null, и если это так, то созданный асинхронный метод будет задавать исключение для задачи.

```csharp
[Export ("upload:onComplete:")]
[Async]
void Upload (string file, Action<string,NSError> onComplete);
```

В приведенном выше примере создается следующий асинхронный метод:

```csharp
Task<string> UploadAsync (string file);
```

И при ошибке в результирующей задаче будет задано исключение `NSErrorException`, которое создает оболочку для результирующего `NSError`.

#### <a name="asyncattributeresulttype"></a>Асинкаттрибуте. ResultType

Это свойство используется для указания значения для возвращаемого объекта `Task`.   Этот параметр принимает существующий тип, поэтому его необходимо определить в одном из базовых определений API.

#### <a name="asyncattributeresulttypename"></a>Асинкаттрибуте. Ресулттипенаме

Это свойство используется для указания значения для возвращаемого объекта `Task`.   Этот параметр принимает имя нужного типа, генератор создаст ряд свойств, по одному для каждого параметра, который принимает обратный вызов.

#### <a name="asyncattributemethodname"></a>Асинкаттрибуте. имя_метода

Это свойство используется для настройки имен созданных асинхронных методов.   По умолчанию используется имя метода и добавляется текст "Async". его можно использовать для изменения значения по умолчанию.

<a name="DesignatedInitializerAttribute" />

### <a name="designatedinitializerattribute"></a>десигнатединитиализераттрибуте

Когда этот атрибут применяется к конструктору, он создает тот же `[DesignatedInitializer]` в окончательной сборке платформы. Это помогает интегрированной среде разработки указать, какой конструктор следует использовать в подклассах.

Он должен соответствовать цели-C/Clang использование `__attribute__((objc_designated_initializer))`.

<a name="DisableZeroCopyAttribute" />

### <a name="disablezerocopyattribute"></a>дисаблезерокопяттрибуте

Этот атрибут применяется к строковым параметрам или строковым свойствам и инструктирует генератору кода не использовать для этого параметра Маршалирование строк нулевого копирования, а вместо этого создать новый экземпляр NSString из C# строки.
Этот атрибут требуется только в строках, если вы указываете генератору использовать Маршалирование строк без копирования, используя либо параметр командной строки `--zero-copy` либо установив атрибут уровня сборки `ZeroCopyStringsAttribute`.

Это необходимо в тех случаях, когда свойство объявлено в цели-C как `retain` или `assign` свойство вместо свойства `copy`. Обычно они происходят в сторонних библиотеках, которые были неправильно "оптимизированы" разработчиками. В общем случае свойства `retain` или `assign` `NSString` неверны, так как `NSMutableString` или классы, производные от пользователя `NSString`, могут изменять содержимое строк без знания кода библиотеки, что незначительно нарушает работу приложения. Обычно это происходит из-за преждевременной оптимизации.

Ниже показаны два таких свойства в цели-C:

```csharp
@property(nonatomic,retain) NSString *name;
@property(nonatomic,assign) NSString *name2;
```

<a name="DisposeAttribute" />

### <a name="disposeattribute"></a>диспосеаттрибуте

При применении `[DisposeAttribute]` к классу вы предоставляете фрагмент кода, который будет добавлен в реализацию метода `Dispose()` класса.

Поскольку метод `Dispose` автоматически создается средствами `bmac-native` и `btouch-native`, необходимо использовать атрибут `[Dispose]`, чтобы внедрить некоторый код в созданную реализацию метода `Dispose`.

Пример:

```csharp
[BaseType (typeof (NSObject))]
[Dispose ("if (OpenConnections > 0) CloseAllConnections ();")]
interface DatabaseConnection {
}
```

<a name="ExportAttribute" />

### <a name="exportattribute"></a>ExportAttribute

Атрибут `[Export]` используется для отметки метода или свойства, предоставляемого исполняющей среде цели-C. Этот атрибут является общим для средства привязки и фактических сред выполнения Xamarin. iOS и Xamarin. Mac. Для методов метод передается в сформированный код в соответствии со свойствами, экспорты методов получения и задания создаются на основе базового объявления (Дополнительные сведения о том, как изменить поведение средства привязки см. в разделе [`[BindAttribute]`](#BindAttribute) ).

Синтаксис:

```csharp
public enum ArgumentSemantic {
    None, Assign, Copy, Retain.
}

[AttributeUsage (AttributeTargets.Method | AttributeTargets.Constructor | AttributeTargets.Property)]
public class ExportAttribute : Attribute {
    public ExportAttribute();
    public ExportAttribute (string selector);
    public ExportAttribute (string selector, ArgumentSemantic semantic);
    public string Selector { get; set; }
    public ArgumentSemantic ArgumentSemantic { get; set; }
}
```

[Селектор](https://developer.apple.com/library/content/documentation/General/Conceptual/DevPedia-CocoaCore/Selector.html) представляет имя базового метода цели или свойства, к которому выполняется привязка.

#### <a name="exportattributeargumentsemantic"></a>Експортаттрибуте. Аргументсемантик

<a name="FieldAttribute" />

### <a name="fieldattribute"></a>фиелдаттрибуте

Этот атрибут используется для предоставления глобальной переменной C в качестве поля, которое загружается по запросу и предоставляется C# коду. Обычно это необходимо для получения значений констант, определенных в C или цели-C, которые могут быть либо токенами, используемыми в некоторых API, либо со значениями, которые являются непрозрачными и должны использоваться как есть в пользовательском коде.

Синтаксис:

```csharp
public class FieldAttribute : Attribute {
    public FieldAttribute (string symbolName);
    public FieldAttribute (string symbolName, string libraryName);
    public string SymbolName { get; set; }
    public string LibraryName { get; set; }
}
```

`symbolName` — это символ C для связывания с. По умолчанию он загружается из библиотеки, имя которой выводится из пространства имен, в котором определен тип. Если это не Библиотека, в которой выполняется поиск символа, следует передать параметр `libraryName`. При связывании статической библиотеки используйте `__Internal` в качестве параметра `libraryName`.

Создаваемые свойства всегда являются статическими.

Свойства, помеченные атрибутом field, могут иметь следующие типы:

* `NSString`
* `NSArray`
* `nint` / `int` / `long`
* `nuint` / `uint` / `ulong`
* `nfloat` / `float`
* `double`
* `CGSize`
* `System.IntPtr`
* перечислениям;

Методы задания не поддерживаются для [перечислений, поддерживающих константы NSString](#enum-attributes), но при необходимости их можно привязать вручную.

Пример

```csharp
[Static]
interface CameraEffects {
     [Field ("kCameraEffectsZoomFactorKey", "CameraLibrary")]
     NSString ZoomFactorKey { get; }
}
```

<a name="InternalAttribute" />

### <a name="internalattribute"></a>интерналаттрибуте

Атрибут `[Internal]` может применяться к методам или свойствам и влияет на пометку созданного кода с помощью ключевого слова `internal` C# , делая код доступным только для кода в создаваемой сборке. Обычно это используется для скрытия интерфейсов API, которые являются слишком низким уровнем, или предоставляют неоптимальный открытый API, который необходимо улучшить, или для API-интерфейсов, которые не поддерживаются генератором и для которых требуется написать код вручную.

При проектировании привязки обычно можно скрыть метод или свойство, используя этот атрибут, и указать другое имя для метода или свойства, а затем в C# вспомогательном файле поддержки добавить обертку со строгой типизацией, предоставляющую Базовая функциональность.

Пример:

```csharp
[Internal]
[Export ("setValue:forKey:")]
void _SetValueForKey (NSObject value, NSObject key);

[Internal]
[Export ("getValueForKey:")]
NSObject _GetValueForKey (NSObject key);
```

Затем в вспомогательном файле можно было бы использовать следующий код:

```csharp
public NSObject this [NSObject idx] {
    get {
        return _GetValueForKey (idx);
    }
    set {
        _SetValueForKey (value, idx);
    }
}
```

<a name="IsThreadStaticAttribute" />

### <a name="isthreadstaticattribute"></a>иссреадстатикаттрибуте

Этот атрибут помечает резервное поле для свойства, которое будет отмечено атрибутом .NET `[ThreadStatic]`. Это полезно, если поле является статической переменной потока.

### <a name="marshalnativeexceptions-xamarinios-606"></a>Маршалнативиксцептионс (Xamarin. iOS 6.0.6)

Этот атрибут сделает метод поддержкой собственных исключений (цель-C).
Вместо вызова `objc_msgSend` напрямую, вызов будет проходить через пользовательский трамполине, который перехватывает исключения ObjectiveC и маршалирует их в управляемые исключения.

В настоящее время поддерживаются только несколько подписей `objc_msgSend` (вы обнаружите, что сигнатура не поддерживается, когда собственная компоновка приложения, использующего привязку, завершается ошибкой с отсутствующим символом monotouch_ *_objc_msgSend* ), но больше можно добавить по запросу.

### <a name="newattribute"></a>неваттрибуте

Этот атрибут применяется к методам и свойствам, чтобы генератор создавал ключевое слово `new` перед объявлением.

Он используется, чтобы избежать предупреждений компилятора, если один и тот же метод или имя свойства введены в подкласс, который уже существовал в базовом классе.

<a name="NotificationAttribute" />

### <a name="notificationattribute"></a>нотификатионаттрибуте

Этот атрибут можно применить к полям, чтобы генератор создал строго типизированный вспомогательный класс уведомлений.

Этот атрибут можно использовать без аргументов для уведомлений, которые не содержат полезных данных, или можно указать `System.Type`, ссылающийся на другой интерфейс в определении API, обычно с именем, которое заканчивается на EventArgs. Генератор преобразует интерфейс в класс, который подклассировать `EventArgs` и будет включать все свойства, перечисленные здесь. Атрибут [`[Export]`](#ExportAttribute) должен использоваться в классе `EventArgs` для перечисления имени ключа, используемого для поиска словаря цели-C для выборки значения.

Пример:

```csharp
interface MyClass {
    [Notification]
    [Field ("MyClassDidStartNotification")]
    NSString DidStartNotification { get; }
}
```

Приведенный выше код создаст вложенный класс `MyClass.Notifications` со следующими методами:

```csharp
public class MyClass {
   [..]
   public Notifications {
      public static NSObject ObserveDidStart (EventHandler<NSNotificationEventArgs> handler)
      public static NSObject ObserveDidStart (NSObject objectToObserve, EventHandler<NSNotificationEventArgs> handler)
   }
}
```

Пользователи кода могут легко подписываться на уведомления, опубликованные в [нсдефаултцентер](xref:Foundation.NSNotificationCenter.DefaultCenter) , используя следующий код:

```csharp
var token = MyClass.Notifications.ObserverDidStart ((notification) => {
    Console.WriteLine ("Observed the 'DidStart' event!");
});
```

Или, чтобы задать конкретный объект для наблюдения. При передаче `null` в `objectToObserve` этот метод будет вести себя так же, как и его другой узел.

```csharp
var token = MyClass.Notifications.ObserverDidStart (objectToObserve, (notification) => {
    Console.WriteLine ("Observed the 'DidStart' event on objectToObserve!");
});
```

Возвращаемое значение из `ObserveDidStart` можно использовать для простой отмены получения уведомлений, например:

```csharp
token.Dispose ();
```

Также можно вызвать [нснотификатион. дефаултцентер. RemoveObserver](xref:Foundation.NSNotificationCenter.RemoveObserver(Foundation.NSObject)) и передать токен. Если уведомление содержит параметры, следует указать вспомогательный интерфейс `EventArgs`, как это выглядит так:

```csharp
interface MyClass {
    [Notification (typeof (MyScreenChangedEventArgs)]
    [Field ("MyClassScreenChangedNotification")]
    NSString ScreenChangedNotification { get; }
}

// The helper EventArgs declaration
interface MyScreenChangedEventArgs {
    [Export ("ScreenXKey")]
    nint ScreenX { get; set; }

    [Export ("ScreenYKey")]
    nint ScreenY { get; set; }

    [Export ("DidGoOffKey")]
    [ProbePresence]
    bool DidGoOff { get; }
}
```

В приведенном выше примере создается класс `MyScreenChangedEventArgs` со свойствами `ScreenX` и `ScreenY`, которые будут получать данные из словаря [нснотификатион. UserInfo](xref:Foundation.NSNotification.UserInfo) , используя ключи `ScreenXKey` и `ScreenYKey` соответственно и применяя соответствующие преобразования. Атрибут `[ProbePresence]` используется, чтобы генератор проверит, задан ли ключ в `UserInfo`, вместо того, чтобы пытаться извлечь значение. Используется в тех случаях, когда присутствие ключа является значением (обычно для логических значений).

Это позволяет написать следующий код:

```csharp
var token = MyClass.NotificationsObserveScreenChanged ((notification) => {
    Console.WriteLine ("The new screen dimensions are {0},{1}", notification.ScreenX, notification.ScreenY);
});
```

В некоторых случаях нет константы, связанной со значением, передаваемым в словаре.  Apple иногда использует константы открытых символов и иногда использует строковые константы.  По умолчанию атрибут [`[Export]`](#ExportAttribute) в предоставленном `EventArgs` классе будет использовать указанное имя как открытый символ для поиска во время выполнения.  Если это не так, а вместо этого следует выполнять поиск в виде строковой константы, передайте значение `ArgumentSemantic.Assign` в атрибут Export.

**Новые в Xamarin. iOS 8,4**

Иногда уведомления начнутся без аргументов, поэтому использование [`[Notification]`](#NotificationAttribute) без аргументов является приемлемым.  Но иногда будут введены параметры уведомления.  Для поддержки этого сценария атрибут может быть применен несколько раз.

Если вы разрабатываете привязку и хотите избежать разрушения существующего пользовательского кода, можно превратить существующее уведомление из:

```csharp
interface MyClass {
    [Notification]
    [Field ("MyClassScreenChangedNotification")]
    NSString ScreenChangedNotification { get; }
}
```

В версию, которая содержит атрибут уведомления дважды, например:

```csharp
interface MyClass {
    [Notification]
    [Notification (typeof (MyScreenChangedEventArgs)]
    [Field ("MyClassScreenChangedNotification")]
    NSString ScreenChangedNotification { get; }
}
```

<a name="NullAllowedAttribute" />

### <a name="nullallowedattribute"></a>нуллалловедаттрибуте

При применении к свойству оно помечает свойство как разрешающее присвоение значения `null`. Это допустимо только для ссылочных типов.

При применении к параметру в сигнатуре метода он указывает, что указанный параметр может иметь значение NULL и что проверка не должна выполняться для передачи `null` значений.

Если ссылочный тип не имеет этого атрибута, средство привязки создаст проверку значения, присваиваемого перед передачей цели в цель-C, и создаст проверку, которая выдаст `ArgumentNullException`, если присваиваемое значение `null`.

Пример:

```csharp
// In properties

[NullAllowed]
UIImage IconFile { get; set; }

// In methods
void SetImage ([NullAllowed] UIImage image, State forState);
```

<a name="OverrideAttribute" />

### <a name="overrideattribute"></a>оверридеаттрибуте

Используйте этот атрибут, чтобы указать генератору привязки, что привязка для этого конкретного метода должна помечаться ключевым словом `override`.

### <a name="presnippetattribute"></a>пресниппетаттрибуте

Этот атрибут можно использовать для вставки кода, который должен быть вставлен после проверки входных параметров, но до того, как код вызовет цель-C.

Пример

```csharp
[Export ("demo")]
[PreSnippet ("var old = ViewController;")]
void Demo ();
```

### <a name="prologuesnippetattribute"></a>прологуесниппетаттрибуте

Этот атрибут можно использовать для вставки кода, который должен быть вставлен перед проверкой любого из параметров в созданном методе.

Пример

```csharp
[Export ("demo")]
[Prologue ("Trace.Entry ();")]
void Demo ();
```

### <a name="postgetattribute"></a>постжетаттрибуте

Указывает генератору привязки вызвать указанное свойство из этого класса для выборки из него значения.

Это свойство обычно используется для обновления кэша, указывающего на ссылочные объекты, которые сохраняют граф объектов. Обычно он отображается в коде с такими операциями, как добавление или удаление. Этот метод используется, чтобы элементы после добавления или удаления были обновлены во внутреннем кэше, чтобы обеспечить хранение управляемых ссылок на объекты, которые фактически используются. Это возможно, поскольку средство привязки создает резервное поле для всех ссылочных объектов в заданной привязке.

Пример

```csharp
[BaseType (typeof (NSObject))]
[Since (4,0)]
public interface NSOperation {
    [Export ("addDependency:")][PostGet ("Dependencies")]
    void AddDependency (NSOperation op);

    [Export ("removeDependency:")][PostGet ("Dependencies")]
    void RemoveDependency (NSOperation op);

    [Export ("dependencies")]
    NSOperation [] Dependencies { get; }
}
```

В этом случае свойство `Dependencies` будет вызываться после добавления или удаления зависимостей из объекта `NSOperation`, гарантируя, что у нас есть граф, представляющий фактически загруженные объекты, предотвращая утечку памяти, а также повреждение памяти.

### <a name="postsnippetattribute"></a>постсниппетаттрибуте

Этот атрибут можно использовать для внедрения некоторого C# исходного кода, который будет вставлен после того, как код вызвал базовый метод цели-C

Пример

```csharp
[Export ("demo")]
[PostSnippet ("if (old != null) old.DemoComplete ();")]
void Demo ();
```

### <a name="proxyattribute"></a>проксяттрибуте

Этот атрибут применяется к возвращаемым значениям, чтобы помечать их как прокси-объекты. Некоторые API-интерфейсы цели на языке C возвращают прокси-объекты, которые не могут отличаться от пользовательских привязок. Результатом этого атрибута является Пометка объекта как объекта `DirectBinding`. Для сценария в Xamarin. Mac можно просмотреть [обсуждение этой ошибки](https://bugzilla.novell.com/show_bug.cgi?id=670844).

### <a name="retainlistattribute"></a>ретаинлистаттрибуте

Указывает генератору на необходимость сохранения управляемой ссылки на параметр или удаления внутренней ссылки на параметр. Используется для сохранения ссылок на объекты.

Синтаксис:

```csharp
public class RetainListAttribute: Attribute {
     public RetainListAttribute (bool doAdd, string listName);
}
```

Если значение `doAdd` равно true, то параметр добавляется в `__mt_{0}_var List<NSObject>;`. Где `{0}` заменяется заданным `listName`ом. Необходимо объявить это резервное поле в дополном разделяемом классе для API.

Пример см. в разделе [Foundation.CS](https://github.com/mono/maccore/blob/master/src/foundation.cs) and [NSNotificationCenter.CS](https://github.com/mono/maccore/blob/master/src/Foundation/NSNotificationCenter.cs)

### <a name="releaseattribute-xamarinios-60"></a>Релеасеаттрибуте (Xamarin. iOS 6,0)

Это можно применить к возвращаемым типам, чтобы указать, что генератор должен вызвать `Release` для объекта перед его возвратом. Это необходимо только в том случае, если метод предоставляет Сохраняемый объект (в отличие от автоосвобожденного объекта, который является наиболее распространенным сценарием).

Пример

```csharp
[Export ("getAndRetainObject")]
[return: Release ()]
NSObject GetAndRetainObject ();
```

Кроме того, этот атрибут распространяется на созданный код, чтобы среда выполнения Xamarin. iOS знала, что объект должен оставаться объектом при возврате цели-C из такой функции.

### <a name="sealedattribute"></a>сеаледаттрибуте

Указывает генератору на то, что созданный метод помечается как запечатанный. Если этот атрибут не задан, по умолчанию создается виртуальный метод (виртуальный метод, абстрактный метод или переопределение в зависимости от того, как используются другие атрибуты).

<a name="StaticAttribute" />

### <a name="staticattribute"></a>статикаттрибуте

При применении атрибута `[Static]` к методу или свойству создается статический метод или свойство. Если этот атрибут не указан, генератор создает метод или свойство экземпляра.

### <a name="transientattribute"></a>трансиентаттрибуте

Используйте этот атрибут, чтобы пометить свойства, значения которых являются временными, то есть объекты, которые были временно созданы iOS, но не являются долговременными. Если этот атрибут применяется к свойству, генератор не создает резервное поле для этого свойства, что означает, что управляемый класс не сохраняет ссылку на объект.

<a name="WrapAttribute" />

### <a name="wrapattribute"></a>врапаттрибуте

В структуре привязок Xamarin. iOS/Xamarin. Mac атрибут `[Wrap]` используется для упаковки слабо типизированного объекта в строго типизированный объект. Это происходит в основном с объектами делегата цели-C, которые обычно объявляются как тип `id` или `NSObject`. Соглашение, используемое Xamarin. iOS и Xamarin. Mac, позволяет предоставлять эти делегаты или источники данных как тип `NSObject` и называться с помощью соглашения "слабое" + имя, которое предоставляется. Свойство `id delegate` из цели-C будет предоставлено как свойство `NSObject WeakDelegate { get; set; }` в файле контракта API.

Но обычно значение, присвоенное этому делегату, имеет строгий тип, поэтому мы выберем строгий тип и применяете атрибут `[Wrap]`, это означает, что пользователи могут использовать слабые типы, если им требуется какой-либо тонкий контроль, или если им нужно прибегнуть к самым низким советам. или они могут использовать строго типизированное свойство для большей части их работы.

Пример

```csharp
[BaseType (typeof (NSObject))]
interface Demo {
     [Export ("delegate"), NullAllowed]
     NSObject WeakDelegate { get; set; }

     [Wrap ("WeakDelegate")]
     DemoDelegate Delegate { get; set; }
}

[BaseType (typeof (NSObject))]
[Model][Protocol]
interface DemoDelegate {
    [Export ("doDemo")]
    void DoDemo ();
}
```

Таким образом, пользователь будет использовать нестрого типизированную версию делегата:

```csharp
// The weak case, user has to roll his own
class SomeObject : NSObject {
    [Export ("doDemo")]
    void CallbackForDoDemo () {}

}

var demo = new Demo ();
demo.WeakDelegate = new SomeObject ();
```

Так как пользователь будет использовать строго типизированную версию, обратите внимание, что пользователь использует преимущества C#системы типов и использует ключевое слово override, чтобы объявить свое намерение, и что ему не нужно вручную снабдить метод `[Export]`, так как мы сделали это в привязке для пользователя:

```csharp
// This is the strong case,
class MyDelegate : DemoDelegate {
   override void Demo DoDemo () {}
}

var strongDemo = new Demo ();
demo.Delegate = new MyDelegate ();
```

Другим применением атрибута `[Wrap]` является поддержка строго типизированной версии методов.  Пример:

```csharp
[BaseType (typeof (NSObject))]
interface XyzPanel {
    [Export ("playback:withOptions:")]
    void Playback (string fileName, [NullAllowed] NSDictionary options);

    [Wrap ("Playback (fileName, options == null ? null : options.Dictionary")]
    void Playback (string fileName, XyzOptions options);
}
```

Если атрибут `[Wrap]` применяется к методу внутри типа, дополненного атрибутом `[Category]`, необходимо включить `This` в качестве первого аргумента, так как создается метод расширения. Пример:

```csharp
[Wrap ("Write (This, image, options?.Dictionary, out error)")]
bool Write (CIImage image, CIImageRepresentationOptions options, out NSError error);
```

Элементы, созданные `[Wrap]`, не `virtual` по умолчанию, если требуется элемент `virtual`, для которого можно задать `true` необязательный параметр `isVirtual`.

```csharp
[BaseType (typeof (NSObject))]
interface FooExplorer {
    [Export ("fooWithContentsOfURL:")]
    void FromUrl (NSUrl url);

    [Wrap ("FromUrl (NSUrl.FromString (url))", isVirtual: true)]
    void FromUrl (string url);
}
```

`[Wrap]` также можно использовать непосредственно в методах получения и заданиях свойств.
Это позволяет получить полный контроль над ними и при необходимости изменить код.
Например, рассмотрим следующее определение API, использующее интеллектуальные перечисления:

```csharp
// Smart enum.
enum PersonRelationship {
        [Field (null)]
        None,

        [Field ("FMFather", "__Internal")]
        Father,

        [Field ("FMMother", "__Internal")]
        Mother
}
```

Определение интерфейса:

```csharp
// Property definition.

[Export ("presenceType")]
NSString _PresenceType { get; set; }

PersonRelationship PresenceType {
    [Wrap ("PersonRelationshipExtensions.GetValue (_PresenceType)")]
    get;
    [Wrap ("_PresenceType = value.GetConstant ()")]
    set;
}
```

## <a name="parameter-attributes"></a>Атрибуты параметра

В этом разделе описываются атрибуты, которые можно применить к параметрам в определении метода, а также `[NullAttribute]`, применяемые к свойству в целом.

<a name="BlockCallback" />

### <a name="blockcallback"></a>блокккаллбакк

Этот атрибут применяется к типам параметров в C# объявлениях делегатов для уведомления связывателя о том, что рассматриваемый параметр соответствует соглашению о вызове блока "цель-C" и должен маршалировать его таким образом.

Обычно это используется для обратных вызовов, которые определяются подобным образом в цели-C:

```objc
typedef returnType (^SomeTypeDefinition) (int parameter1, NSString *parameter2);
```

См. также: [ккаллбакк](#CCallback).

<a name="CCallback" />

### <a name="ccallback"></a>ккаллбакк

Этот атрибут применяется к типам параметров в C# объявлениях делегатов для уведомления связывателя о том, что рассматриваемый параметр соответствует соглашению о вызовах указателей функции ABI, и должен маршалировать его таким образом.

Обычно это используется для обратных вызовов, которые определяются подобным образом в цели-C:

```objc
typedef returnType (*SomeTypeDefinition) (int parameter1, NSString *parameter2);
```

См. также: [блокккаллбакк](#BlockCallback).

### <a name="params"></a>params

Можно использовать атрибут `[Params]` в последнем параметре массива определения метода, чтобы генератор вставил в определение "params".   Это позволит привязке легко разрешить необязательные параметры.

Например, следующее определение:

```csharp
[Export ("loadFiles:")]
void LoadFiles ([Params]NSUrl [] files);
```

Позволяет написать следующий код:

```csharp
foo.LoadFiles (new NSUrl (url));
foo.LoadFiles (new NSUrl (url1), new NSUrl (url2), new NSUrl (url3));
```

Это имеет дополнительное преимущество, которое не требует от пользователей создавать массив исключительно для передачи элементов.

<a name="plainstring" />

### <a name="plainstring"></a>плаинстринг

Можно использовать атрибут `[PlainString]` перед строковыми параметрами, чтобы указать генератору привязки передать строку в виде строки C вместо передачи параметра в качестве `NSString`.

Большинство API-интерфейсов цели используют `NSString` параметры, но несколько API предоставляют `char *` API для передачи строк, а не `NSString` вариации.
В таких случаях используйте `[PlainString]`.

Например, следующие объявления цели-C:

```csharp
- (void) setText: (NSString *) theText;
- (void) logMessage: (char *) message;
```

Должен быть привязан следующим образом:

```csharp
[Export ("setText:")]
void SetText (string theText);

[Export ("logMessage:")]
void LogMessage ([PlainString] string theText);
```

### <a name="retainattribute"></a>ретаинаттрибуте

Указывает генератору на необходимость сохранения ссылки на указанный параметр. Генератор предоставит резервное хранилище для этого поля. можно также указать имя (`WrapName`), в котором будет храниться значение. Это полезно для хранения ссылки на управляемый объект, который передается в качестве параметра цели-C, и когда известно, что цель-C сохранит только эту копию объекта. Например, API, такой как `SetDisplay (SomeObject)`, будет использовать этот атрибут, так как вполне вероятно, что Сетдисплай может отображать только один объект за раз. Если необходимо отследить более чем один объект (например, для API, похожего на стек), используйте атрибут `[RetainList]`.

Синтаксис:

```csharp
public class RetainAttribute {
    public RetainAttribute ();
    public RetainAttribute (string wrapName);
    public string WrapName { get; }
}
```

### <a name="retainlistattribute"></a>ретаинлистаттрибуте

Указывает генератору на необходимость сохранения управляемой ссылки на параметр или удаления внутренней ссылки на параметр. Используется для сохранения ссылок на объекты.

Синтаксис:

```csharp
public class RetainListAttribute: Attribute {
     public RetainListAttribute (bool doAdd, string listName);
}
```

Если значение `doAdd` равно true, то параметр добавляется в `__mt_{0}_var List<NSObject>`. Где `{0}` заменяется заданным `listName`ом. Необходимо объявить это резервное поле в дополном разделяемом классе для API.

Пример см. в разделе [Foundation.CS](https://github.com/mono/maccore/blob/master/src/foundation.cs) and [NSNotificationCenter.CS](https://github.com/mono/maccore/blob/master/src/Foundation/NSNotificationCenter.cs)

### <a name="transientattribute"></a>трансиентаттрибуте

Этот атрибут применяется к параметрам и используется только при переходе с цели-C на C#.  Во время этих переходов различные параметры цели-C `NSObject` упаковываются в управляемое представление объекта.

Среда выполнения принимает ссылку на собственный объект и сохраняет ссылку до тех пор, пока не будет удалена последняя управляемая ссылка на объект, и сборщик мусора сможет запуститься.

В некоторых случаях важно, чтобы C# среда выполнения не оставалась ссылкой на собственный объект.  Это иногда происходит, когда базовый машинный код прикрепляет особое поведение к жизненному циклу параметра.  Например: деструктор для параметра выполняет некоторое действие по очистке или освобождает некоторый ценный ресурс.

Этот атрибут информирует среду выполнения о необходимости удаления объекта, если это возможно, при возврате к цели-C из перезаписанного метода.

Правило является простым: Если среде выполнения пришлось бы создать новое управляемое представление из собственного объекта, то в конце функции будет удалено число сохранений для собственного объекта, а свойство Handle управляемого объекта будет удалено.   Это означает, что если вы сохранили ссылку на управляемый объект, эта ссылка станет бесполезными (при вызове методов в ней будет создано исключение).

Если переданный объект не был создан или если уже имеется ожидающее управляемое представление объекта, принудительное удаление не выполняется. 

## <a name="property-attributes"></a>Атрибуты свойств

<a name="NotImplementedAttribute" />

### <a name="notimplementedattribute"></a>нотимплементедаттрибуте

Этот атрибут используется для поддержки идиомы цели-C, когда свойство с методом получения введено в базовый класс, а изменяемый подкласс вводит метод задания.

Поскольку C# не поддерживает эту модель, базовый класс должен иметь и метод задания, и метод получения, а подкласс может использовать [оверридеаттрибуте](#OverrideAttribute).

Этот атрибут используется только в методах задания свойств и используется для поддержки изменяемого идиома в цели-C.

Пример

```csharp
[BaseType (typeof (NSObject))]
interface MyString {
    [Export ("initWithValue:")]
    IntPtr Constructor (string value);

    [Export ("value")]
    string Value {
        get;

    [NotImplemented ("Not available on MyString, use MyMutableString to set")]
        set;
    }
}

[BaseType (typeof (MyString))]
interface MyMutableString {
    [Export ("value")]
    [Override]
    string Value { get; set; }
}
```

<a name="enum-attributes" />

## <a name="enum-attributes"></a>Атрибуты перечисления

Сопоставление констант `NSString` с перечисляемыми значениями является простым способом создания лучшего API .NET. Им

* позволяет получить более полезные сведения о завершении кода, отображая **только** правильные значения API;
* Добавляет безопасность типов, нельзя использовать другую `NSString`ую константу в неправильном контексте; перетаскивани
* позволяет скрыть некоторые константы, делая завершение кода более коротким списком API без потери функциональности.

Пример

```csharp
enum NSRunLoopMode {

    [DefaultEnumValue]
    [Field ("NSDefaultRunLoopMode")]
    Default,

    [Field ("NSRunLoopCommonModes")]
    Common,

    [Field (null)]
    Other = 1000
}
```

Из приведенного выше определения привязки генератор создаст сам `enum` и создаст `*Extensions` статический тип, включающий два способа преобразования между значениями перечисления и `NSString` константами. Это означает, что константы остаются доступными для разработчиков даже в том случае, если они не являются частью API.

Примеры

```csharp
// using the NSString constant in a different API / framework / 3rd party code
CallApiRequiringAnNSString (NSRunLoopMode.Default.GetConstant ());
```

```csharp
// converting the constants from a different API / framework / 3rd party code
var constant = CallApiReturningAnNSString ();
// back into an enum value
CallApiWithEnum (NSRunLoopModeExtensions.GetValue (constant));
```

### <a name="defaultenumvalueattribute"></a>дефаултенумвалуеаттрибуте

С этим атрибутом можно снабдить **одно** значение перечисления. Если значение перечисления неизвестно, это станет возвращаемой константой.

Из приведенного выше примера:

```csharp
var x = (NSRunLoopMode) 99;
Call (x.GetConstant ()); // NSDefaultRunLoopMode will be used
```

Если значение enum не декорировано, будет выдано исключение `NotSupportedException`.

### <a name="errordomainattribute"></a>еррордомаинаттрибуте

Коды ошибок привязываются как значения перечисления. Обычно для них существует домен с ошибками, и не всегда легко найти, какой из них применяется (или если он даже существует).

С помощью этого атрибута можно связать домен ошибок с самим перечислением.

Пример

```csharp
[Native]
[ErrorDomain ("AVKitErrorDomain")]
public enum AVKitError : nint {
    None = 0,
    Unknown = -1000,
    PictureInPictureStartFailed = -1001
}
```

Затем можно вызвать метод расширения `GetDomain`, чтобы получить константу домена для любой ошибки.

### <a name="fieldattribute"></a>фиелдаттрибуте

Это тот же атрибут `[Field]`, который используется для констант внутри типа. Он также может использоваться внутри перечислений для отображения значения с определенной константой.

Значение `null` можно использовать, чтобы указать, какое значение перечисления должно возвращаться, если указана `null` `NSString` константа.

Из приведенного выше примера:

```csharp
var constant = NSRunLoopMode.NewInWatchOS3; // will be null in watchOS 2.x
Call (NSRunLoopModeExtensions.GetValue (constant)); // will return 1000
```

Если `null` значение отсутствует, будет выдано исключение `ArgumentNullException`.

## <a name="global-attributes"></a>Глобальные атрибуты

Глобальные атрибуты либо применяются с помощью модификатора `[assembly:]` Attribute, например [`[LinkWithAttribute]`](#LinkWithAttribute) , либо могут использоваться где угодно, как атрибуты [`[Lion]`](#SinceAndLionAttributes) и [`[Since]`](#SinceAndLionAttributes) .

<a name="LinkWithAttribute" />

### <a name="linkwithattribute"></a>линквисаттрибуте

Это атрибут уровня сборки, позволяющий разработчикам указывать флаги связывания, необходимые для повторного использования связанной библиотеки без принудительной настройки gcc_flags и лишних аргументов mtouch, передаваемых в библиотеку вручную.

Синтаксис:

```csharp
// In properties
[Flags]
public enum LinkTarget {
    Simulator    = 1,
    ArmV6    = 2,
    ArmV7    = 4,
    Thumb    = 8,
}

[AttributeUsage(AttributeTargets.Assembly, AllowMultiple=true)]
public class LinkWithAttribute : Attribute {
    public LinkWithAttribute ();
    public LinkWithAttribute (string libraryName);
    public LinkWithAttribute (string libraryName, LinkTarget target);
    public LinkWithAttribute (string libraryName, LinkTarget target, string linkerFlags);
    public bool ForceLoad { get; set; }
    public string Frameworks { get; set; }
    public bool IsCxx { get; set;  }
    public string LibraryName { get; }
    public string LinkerFlags { get; set; }
    public LinkTarget LinkTarget { get; set; }
    public bool NeedsGccExceptionHandling { get; set; }
    public bool SmartLink { get; set; }
    public string WeakFrameworks { get; set; }
}
```

Этот атрибут применяется на уровне сборки, например при использовании [привязок кореплот](https://github.com/mono/monotouch-bindings/tree/master/CorePlot) :

```csharp
[assembly: LinkWith ("libCorePlot-CocoaTouch.a", LinkTarget.ArmV7 | LinkTarget.ArmV7s | LinkTarget.Simulator, Frameworks = "CoreGraphics QuartzCore", ForceLoad = true)]
```

При использовании атрибута `[LinkWith]` указанный `libraryName` внедряется в результирующую сборку, что позволяет пользователям поставлять одну библиотеку DLL, которая содержит как неуправляемые, так и все флаги командной строки, необходимые для правильного использования библиотеки из Xamarin. iOS.

Также можно не предоставлять `libraryName`, в этом случае атрибут `LinkWith` может быть использован только для указания дополнительных флагов компоновщика:

``` csharp
[assembly: LinkWith (LinkerFlags = "-lsqlite3")]
```

#### <a name="linkwithattribute-constructors"></a>Конструкторы Линквисаттрибуте

Эти конструкторы позволяют указать библиотеку для связывания и внедрения в результирующую сборку, Поддерживаемые целевые объекты, которые поддерживает Библиотека, и все необязательные флаги библиотеки, необходимые для связи с библиотекой.

Обратите внимание, что аргумент `LinkTarget` выводится Xamarin. iOS и не требует установки.

Примеры

```csharp
// Specify additional linker:
[assembly: LinkWith (LinkerFlags = "-sqlite3")]

// Specify library name for the constructor:
[assembly: LinkWith ("libDemo.a");

// Specify library name, and link target for the constructor:
[assembly: LinkWith ("libDemo.a", LinkTarget.Thumb | LinkTarget.Simulator);

// Specify only the library name, link target and linker flags for the constructor:
[assembly: LinkWith ("libDemo.a", LinkTarget.Thumb | LinkTarget.Simulator, SmartLink = true, ForceLoad = true, IsCxx = true);
```

#### <a name="linkwithattributeforceload"></a>Линквисаттрибуте. Форцелоад

Свойство `ForceLoad` используется для определения того, используется ли флаг связи `-force_load` для связывания собственной библиотеки. Сейчас это всегда должно быть истинно.

#### <a name="linkwithattributeframeworks"></a>Линквисаттрибуте. Frameworks

Если привязанная библиотека имеет жесткое требование для всех платформ (кроме `Foundation` и `UIKit`), следует задать свойство `Frameworks` со строкой, содержащей разделенный пробелами список требуемых платформ платформы. Например, при привязке библиотеки, для которой требуется `CoreGraphics` и `CoreText`, необходимо задать для свойства `Frameworks` значение `"CoreGraphics CoreText"`.

#### <a name="linkwithattributeiscxx"></a>Линквисаттрибуте. Исккскс

Присвойте этому свойству значение true, если полученный исполняемый файл необходимо C++ скомпилировать с помощью компилятора, а не по умолчанию, который является компилятором на языке C. Используйте этот параметр, если библиотека, которую вы привязываете C++, была написана в.

#### <a name="linkwithattributelibraryname"></a>Линквисаттрибуте. ИмяБиблиотеки

Имя неуправляемой библиотеки в наборе. Это файл с расширением ". a", который может содержать объектный код для нескольких платформ (например, ARM и x86 для симулятора).

Более ранние версии Xamarin. iOS проверяли свойство `LinkTarget`, чтобы определить платформу, поддерживаемую библиотекой, но теперь она обнаруживается автоопределением, а свойство `LinkTarget` игнорируется.

#### <a name="linkwithattributelinkerflags"></a>Линквисаттрибуте. Линкерфлагс

Строка `LinkerFlags` предоставляет авторам привязки возможность указывать любые дополнительные флаги компоновщика, необходимые при связывании собственной библиотеки с приложением.

Например, если для собственной библиотеки требуются libxml2 и zlib, можно задать для строки `LinkerFlags` значение `"-lxml2 -lz"`.

#### <a name="linkwithattributelinktarget"></a>Линквисаттрибуте. LinkTarget

Более ранние версии Xamarin. iOS проверяли свойство `LinkTarget`, чтобы определить платформу, поддерживаемую библиотекой, но теперь она обнаруживается автоопределением, а свойство `LinkTarget` игнорируется.

#### <a name="linkwithattributeneedsgccexceptionhandling"></a>Линквисаттрибуте. Нидсгкцексцептионхандлинг

Присвойте этому свойству значение true, если связываемая библиотека требует наличия библиотеки обработки исключений GCC (gcc_eh)

#### <a name="linkwithattributesmartlink"></a>Линквисаттрибуте. Смартлинк

Свойству `SmartLink` должно быть присвоено значение true, чтобы разрешить Xamarin. iOS определять, является ли `ForceLoad` обязательным.

#### <a name="linkwithattributeweakframeworks"></a>Линквисаттрибуте. Веакфрамеворкс

Свойство `WeakFrameworks` работает так же, как свойство `Frameworks`, за исключением того, что во время компоновки описатель `-weak_framework` передается GCC для каждой из перечисленных платформ.

`WeakFrameworks` позволяет библиотекам и приложениям слабо связываться с платформами платформы, чтобы они могли использовать их, если они доступны, но не имеют жесткой зависимости от них, что полезно, если ваша библиотека предназначена для добавления дополнительных функций в более новые версии iOS. Дополнительные сведения о слабой компоновке см. в документации Apple о [слабой компоновке](https://developer.apple.com/library/mac/#documentation/MacOSX/Conceptual/BPFrameworks/Concepts/WeakLinking.html).

Хорошим кандидатом для слабой компоновки `Frameworks`, например учетные записи, `CoreBluetooth`, `CoreImage`, `GLKit`, `NewsstandKit` и `Twitter`, так как они доступны только в iOS 5.

<a name="SinceAndLionAttributes" />

### <a name="sinceattribute-ios-and-lionattribute-macos"></a>Синцеаттрибуте (iOS) и Лионаттрибуте (macOS)

Используйте атрибут `[Since]`, чтобы пометить API как появившиеся в определенный момент времени. Атрибут следует использовать только для отметки типов и методов, которые могут вызвать проблему во время выполнения, если базовый класс, метод или свойство недоступны.

Синтаксис:

```csharp
public SinceAttribute : Attribute {
     public SinceAttribute (byte major, byte minor);
     public byte Major, Minor;
}
```

Он не должен применяться к перечислениям, ограничениям или новым структурам, так как они не будут вызывать ошибку времени выполнения, если они выполняются на устройстве с более ранней версией операционной системы.

Пример применения к типу:

```csharp
// Type introduced with iOS 4.2
[Since (4,2)]
[BaseType (typeof (UIPrintFormatter))]
interface UIViewPrintFormatter {
    [Export ("view")]
    UIView View { get; }
}
```

Пример применения к новому элементу:

```csharp
[BaseType (typeof (UIViewController))]
public interface UITableViewController {
    [Export ("tableView", ArgumentSemantic.Retain)]
    UITableView TableView { get; set; }

    [Since (3,2)]
    [Export ("clearsSelectionOnViewWillAppear")]
    bool ClearsSelectionOnViewWillAppear { get; set; }
```

Атрибут `[Lion]` применяется тем же способом, но для типов, представленных с помощью Lion. Причина использования `[Lion]` по сравнению с более конкретным номером версии, используемым в iOS, заключается в том, что iOS перемещается очень часто, в то время как основные выпуски OS X происходят редко, и проще запомнить операционную систему по его кодовому идентификатору, чем по номеру версии

### <a name="adviceattribute"></a>адвицеаттрибуте

Используйте этот атрибут, чтобы дать разработчикам указание о других интерфейсах API, которые более удобны для их использования.   Например, при предоставлении строго типизированной версии API можно использовать этот атрибут для слабо типизированного атрибута, чтобы направить разработчика на Улучшенный API.

Сведения из этого атрибута показаны в документации и средствах, которые могут быть разработаны для предоставления пользователю предложений по улучшению

### <a name="requiressuperattribute"></a>рекуирессуператтрибуте

Это специализированный подкласс атрибута `[Advice]`, который можно использовать для указания разработчику, что переопределение метода **требует** вызова базового метода (переопределен).

Соответствует `clang` [`__attribute__((objc_requires_super))`](https://clang.llvm.org/docs/AttributeReference.html#objc-requires-super)

### <a name="zerocopystringsattribute"></a>зерокопистрингсаттрибуте

Доступно только в Xamarin. iOS 5,4 и более поздних версиях.

Этот атрибут указывает генератору, что привязка для данной конкретной библиотеки (если она применяется с `[assembly:]`) или тип должна использовать быстрое Маршалирование строк нулевого копирования. Этот атрибут эквивалентен передаче параметра командной строки `--zero-copy` генератору.

При использовании нулевого копирования для строк генератор эффективно использует ту же C# строку, что и строка, которую использует цель-C, без создания нового объекта`NSString`и предотвращения копирования данных из C# строк в цель-c Строка. Единственным недостатком использования строк нулевого копирования является то, что необходимо убедиться, что любое строковое свойство, которое будет помечено как `retain` или `copy`, имеет набор атрибутов `[DisableZeroCopy]`. Это необходимо потому, что в стеке для строк нулевого копирования выделяется маркер и он является недопустимым при возврате функции.

Пример

```csharp
[ZeroCopyStrings]
[BaseType (typeof (NSObject))]
interface MyBinding {
    [Export ("name")]
    string Name { get; set; }

    [Export ("domain"), NullAllowed]
    string Domain { get; set; }

    [DisablZeroCopy]
    [Export ("someRetainedNSString")]
    string RetainedProperty { get; set; }
}

```

Атрибут также можно применить на уровне сборки, и он будет применяться ко всем типам сборки:

```csharp
[assembly:ZeroCopyStrings]
```

## <a name="strongly-typed-dictionaries"></a>Словари со строгой типизацией

С помощью Xamarin. iOS 8,0 мы предоставили поддержку простого создания строго типизированных классов, которые переносят `NSDictionaries`.

Хотя вы всегда можете использовать тип данных [диктионариконтаинер](xref:Foundation.DictionaryContainer) вместе с ручным API, гораздо проще сделать это.  Дополнительные сведения см. в разделе [отображая strong Types](~/cross-platform/macios/binding/objective-c-libraries.md#Surfacing_Strong_Types).

<a name="StrongDictionary" />

### <a name="strongdictionary"></a>стронгдиктионари

Когда этот атрибут применяется к интерфейсу, генератор создает класс с тем же именем, что и у интерфейса, производного от [диктионариконтаинер](xref:Foundation.DictionaryContainer) , и преобразует каждое свойство, определенное в интерфейсе, в строго типизированный метод получения и задания для элемента словаря.

При этом автоматически создается класс, который можно создать на основе существующего `NSDictionary` или созданного нового.

Этот атрибут принимает один параметр — имя класса, содержащего ключи, которые используются для доступа к элементам словаря.   По умолчанию каждое свойство в интерфейсе с атрибутом выполняет поиск элемента в указанном типе для имени с суффиксом "Key".

Пример:

```csharp
[StrongDictionary ("MyOptionKeys")]
interface MyOption {
    string Name { get; set; }
    nint    Age  { get; set; }
}

[Static]
interface MyOptionKeys {
    // In Objective-C this is "NSString *MYOptionNameKey;"
    [Field ("MYOptionNameKey")]
    NSString NameKey { get; }

    // In Objective-C this is "NSString *MYOptionAgeKey;"
    [Field ("MYOptionAgeKey")]
    NSString AgeKey { get; }
}

```

В приведенном выше случае класс `MyOption` создаст строковое свойство для `Name`, которое будет использовать `MyOptionKeys.NameKey` в качестве ключа в словаре для получения строки.   И будет использовать `MyOptionKeys.AgeKey` в качестве ключа в словаре для получения `NSNumber`, содержащего целое число.

Если вы хотите использовать другой ключ, можно использовать атрибут Export в свойстве, например:

```csharp
[StrongDictionary ("MyColoringKeys")]
interface MyColoringOptions {
    [Export ("TheName")]  // Override the default which would be NameKey
    string Name { get; set; }

    [Export ("TheAge")] // Override the default which would be AgeKey
    nint    Age  { get; set; }
}

[Static]
interface MyColoringKeys {
    // In Objective-C this is "NSString *MYColoringNameKey"
    [Field ("MYColoringNameKey")]
    NSString TheName { get; }

    // In Objective-C this is "NSString *MYColoringAgeKey"
    [Field ("MYColoringAgeKey")]
    NSString TheAge { get; }
}
```

#### <a name="strong-dictionary-types"></a>Строгие типы словарей

В определении `StrongDictionary` поддерживаются следующие типы данных:

|C#Тип интерфейса|Тип хранилища `NSDictionary`|
|---|---|
|`bool`|`Boolean`, хранящиеся в `NSNumber`|
|Значения перечисления|Целочисленное значение, хранящееся в `NSNumber`|
|`int`|32-разрядное целое число, хранящееся в `NSNumber`|
|`uint`|32-битовое целое число без знака, хранящееся в `NSNumber`|
|`nint`|`NSInteger`, хранящиеся в `NSNumber`|
|`nuint`|`NSUInteger`, хранящиеся в `NSNumber`|
|`long`|64-разрядное целое число, хранящееся в `NSNumber`|
|`float`|32-разрядное целое число, хранящееся в виде `NSNumber`|
|`double`|64-разрядное целое число, хранящееся в виде `NSNumber`|
|`NSObject` и подклассы|`NSObject`|
|`NSDictionary`|`NSDictionary`|
|`string`|`NSString`|
|`NSString`|`NSString`|
|C#`Array``NSObject`|`NSArray`|
|C#`Array`перечислений|`NSArray`, содержащие значения `NSNumber`|
