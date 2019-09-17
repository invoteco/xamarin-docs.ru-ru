---
title: Обзор Unified API
description: Unified API Xamarin позволяет обмениваться кодом между Mac и iOS, а также поддерживать 32 и 64-разрядные приложения с одним и тем же двоичным файлом.
ms.prod: xamarin
ms.assetid: 5F0CEC18-5EF6-4A99-9DCF-1A3B57EA157C
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: cc27b3162c03f0292d3910f86fac9ed89b117f6f
ms.sourcegitcommit: 61a35d0643eb3bf5adb8f8831da54771d8dde626
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/17/2019
ms.locfileid: "71033325"
---
# <a name="unified-api-overview"></a>Обзор Unified API

Unified API Xamarin позволяет обмениваться кодом между Mac и iOS, а также поддерживать 32 и 64-разрядные приложения с одним и тем же двоичным файлом. По умолчанию Unified API используется в новых проектах Xamarin. iOS и Xamarin. Mac.

> [!IMPORTANT]
> Classic API Xamarin, который предшествует Unified API, является устаревшим. 
>
> - Последняя версия Xamarin. iOS для поддержки Classic API (с помощью файла. dll) была Xamarin. iOS 9,10.
> - Xamarin. Mac по-прежнему поддерживает Classic API, но больше не обновляется. Поскольку это не рекомендуется, разработчики должны переместить свои приложения на Unified API.

## <a name="updating-classic-api-based-apps"></a>Обновление приложений на основе Classic API

Выполните соответствующие инструкции для своей платформы:

- [Обновление имеющихся приложений](updating-apps.md)
- [Обновление имеющихся приложений iOS](updating-ios-apps.md)
- [Обновление имеющихся приложений Mac](updating-mac-apps.md)
- [Обновление имеющихся приложений Xamarin.Forms](updating-xamarin-forms-apps.md)
- [Миграция привязки в Unified API](update-binding.md)

## <a name="tips-for-updating-code-to-the-unified-apiupdating-tipsmd"></a>[Советы по обновлению кода в Unified API](updating-tips.md)

Независимо от того, какие приложения вы переносите, ознакомьтесь со [следующими советами](updating-tips.md) , которые помогут успешно выполнить обновление до Unified API.

## <a name="library-split"></a>Разделение библиотеки

С этого момента наши API будут выдаваться двумя способами:

- **Classic API:** Ограничено до 32-бит (только) и предоставляется в `monotouch.dll` сборках и `XamMac.dll` .
- **Unified API:** Поддержка разработок 32 и 64 в разрядах с одним API `Xamarin.iOS.dll` , `Xamarin.Mac.dll` доступным в сборках и.

Это означает, что для корпоративных разработчиков (не предназначенных в магазине приложений) можно продолжать использовать существующие классические API, так как мы будем поддерживать их непрерывно или можете обновить до новых API.

<a name="namespace-changes" />

## <a name="namespace-changes"></a>Изменения пространства имен

Чтобы уменьшить трение для обмена кодом между продуктами Mac и iOS, мы изменим пространства имен для API-интерфейсов в продуктах.

Мы относим префикс «нестандартно» из нашего продукта iOS и «MonoMac» из нашего продукта Mac на основе типов данных.

Это упрощает совместное использование кода на платформах Mac и iOS, не прибегая к условной компиляции и уменьшая шум в верхней части файлов исходного кода.

- **Classic API:** Пространства имен используют `MonoTouch.` префикс `MonoMac.` или.
- **Unified API:** Нет префикса пространства имен

## <a name="runtime-defaults"></a>Значения по умолчанию среды выполнения

По умолчанию Unified API использует сборщик мусора **SGen** и новую систему [подсчета ссылок](~/ios/internals/newrefcount.md) для отслеживания владения объектом. Эта же функция перенесена в Xamarin. Mac.

Это решает ряд проблем, с которыми сталкиваются разработчики со старой системой, а также простотой [управления памятью](~/cross-platform/deploy-test/memory-perf-best-practices.md).

Обратите внимание, что можно включить новое значение refcount даже для Classic API, но значение по умолчанию — консервативное и не требует от пользователей внесения каких-либо изменений. С Unified API мы предприняли возможность изменить значение по умолчанию и предоставить разработчикам все улучшения в то же время, когда они рефакторингируют и повторно проверяют свой код.

## <a name="api-changes"></a>Изменения API

Unified API удаляет устаревшие методы, и существует несколько экземпляров, в которых в именах API были опечатки, когда они были привязаны к исходным пространствам имен с ограничением на касание и MonoMac в классических интерфейсах API. Эти экземпляры были исправлены в новых Объединенных интерфейсах API и должны быть обновлены в приложениях компонентов, iOS и Mac. Ниже приведен список наиболее распространенных типов, которые можно использовать в:

|Имя метода Classic API|Имя метода Unified API|
|--- |--- |
|`UINavigationController.PushViewControllerAnimated()`|`UINavigationController.PushViewController()`|
|`UINavigationController.PopViewControllerAnimated()`|`UINavigationController.PopViewController()`|
|`CGContext.SetRGBFillColor()`|`CGContext.SetFillColor()`|
|`NetworkReachability.SetCallback()`|`NetworkReachability.SetNotification()`|
|`CGContext.SetShadowWithColor`|`CGContext.SetShadow`|
|`UIView.StringSize`|`UIKit.UIStringDrawing.StringSize`|

Полный список изменений при переключении с классической версии на Unified API см. в классической документации по [различиям API (Xamarin. iOS. dll](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/api_changes/classic-vs-unified-8.6.0/index.md) ).

## <a name="updating-to-unified"></a>Обновление до единой системы

В **классическом** API не доступны несколько старых, неиспользуемых или устаревших API- **интерфейсов** . Это может быть проще исправить `CS0616` до начала обновления (ручного или автоматического), так как `[Obsolete]` у вас будет сообщение с атрибутом (часть предупреждения), которое поможет вам правильно использовать API.

Обратите внимание, что мы публикуем отличия [*от изменений в КЛАССИЧЕСКОМ*](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/api_changes/classic-vs-unified-8.6.0/index.md) API VS, которые можно использовать до или после обновления проекта. Устранение устаревших вызовов в классической модели зачастую будет зафиксировано во времени (меньшее количество поисков в документации).

Выполните эти инструкции, чтобы [обновить существующие приложения iOS](~/cross-platform/macios/unified/updating-ios-apps.md)или [приложения Mac](~/cross-platform/macios/unified/updating-mac-apps.md) до Unified API.
Ознакомьтесь с оставшейся частью этой страницы и [этими советами](~/cross-platform/macios/unified/updating-tips.md) , чтобы получить дополнительные сведения о переносе кода.

### <a name="nuget"></a>NuGet

Пакеты NuGet, которые ранее поддерживали Xamarin. iOS через Classic API опубликовали свои сборки с помощью моникера платформы **Monotouch10** .

В Unified API введен новый идентификатор платформы для совместимых пакетов — **Xamarin. iOS10**. Необходимо обновить существующие пакеты NuGet, чтобы добавить поддержку для этой платформы, создав на основе Unified API.

> [!IMPORTANT]
> Если у вас есть ошибка в форме _"Ошибка 3 не может одновременно включать" и ". dll" и "Xamarin. iOS. dll" в одном проекте Xamarin. iOS, на "Xamarin. iOS. dll" имеется явная ссылка, а "0.0.000" — "XXX, Version =, Culture = нейтральный, PublicKeyToken = null ""_ после преобразования приложения в унифицированные API, обычно это происходит из-за наличия в проекте компонента или пакета NuGet, который не был обновлен до Unified API. Необходимо удалить существующий компонент или NuGet, обновить версию, поддерживающую унифицированные API-интерфейсы, и выполнить чистую сборку.

### <a name="the-road-to-64-bits"></a>Путь к 64 бит

Общие сведения о поддержке 32 и 64 разрядов приложений и информация о платформах см. в статье [рекомендации по платформе 32 и 64](~/cross-platform/macios/32-and-64/index.md).

 <a name="new-data-types" />

#### <a name="new-data-types"></a>Новые типы данных

В основе различий оба интерфейса API Mac и iOS используют специфические для архитектуры типы данных, которые всегда 32 бит на 32 разрядных платформ и 64 бит на платформах 64 bit.

Например, цель-C сопоставляет `NSInteger` `int32_t` тип данных с 32-разрядными системами и с `int64_t` 64-разрядными системами.

Чтобы соответствовать этому поведению, на нашем Unified API мы заменяем предыдущее применение `int` (которое в .NET определено как `System.Int32`всегда) к новому типу данных: `System.nint`.  "N" можно представить как "машинный код", поэтому собственный целочисленный тип платформы.

Мы представляем `nint`, а `nuint` `nfloat` также предоставляя типы данных, построенные поверх них, когда это необходимо.

Дополнительные сведения об этих изменениях типа данных см. в документе [native Types](~/cross-platform/macios/nativetypes.md) .

### <a name="how-to-detect-the-architecture-of-ios-apps"></a>Как определить архитектуру приложений iOS

Могут возникнуть ситуации, когда приложению необходимо выяснить, работает ли оно в 32-разрядной или 64-разрядной системе iOS. Для проверки архитектуры можно использовать следующий код:

```csharp
if (IntPtr.Size == 4) {
    Console.WriteLine ("32-bit App");
} else if (IntPtr.Size == 8) {
    Console.WriteLine ("64-bit App");
}
```

<a name="deprecated-apis" />

### <a name="arrays-and-systemcollectionsgeneric"></a>Массивы и System. Collections. Generic

Поскольку C# индексаторы предполагают тип `int`, необходимо явно привести `nint` значения в `int` для доступа к элементам в коллекции или массиве. Например:

```csharp
public List<string> Names = new List<string>();
...

public string GetName(nint index) {
    return Names[(int)index];
}

```

Это ожидаемое поведение, поскольку приведение FROM `int` к `nint` является потерей в 64 бит, неявное преобразование не выполняется.

### <a name="converting-datetime-to-nsdate"></a>Преобразование DateTime в Нсдате

При использовании унифицированных интерфейсов API неявное `DateTime` преобразование `NSDate` значений в значения больше не выполняется. Эти значения потребуется явно преобразовать из одного типа в другой. Для автоматизации этого процесса можно использовать следующие методы расширения:

```csharp
public static DateTime NSDateToDateTime(this NSDate date)
{
    // NSDate has a wider range than DateTime, so clip
    // the converted date to DateTime.Min|MaxValue.
    double secs = date.SecondsSinceReferenceDate;
    if (secs < -63113904000)
        return DateTime.MinValue;
    if (secs > 252423993599)
        return DateTime.MaxValue;
    return (DateTime) date;
}

public static NSDate DateTimeToNSDate(this DateTime date)
{
    if (date.Kind == DateTimeKind.Unspecified)
        date = DateTime.SpecifyKind (date, /* DateTimeKind.Local or DateTimeKind.Utc, this depends on each app */)
    return (NSDate) date;
}

```

<a name="deprecated-typos" />

### <a name="deprecated-apis-and-typos"></a>Устаревшие API и опечатки

В классическом API-интерфейсе Xamarin. iOS (очень Touch. `[Obsolete]` DLL) атрибут использовался двумя разными способами:

- **Устаревший API iOS:** Это объясняется тем, что вы можете запретить использование API с помощью указания Apple, так как он заменяется более новым. Classic API по-прежнему хорошо и часто требуется (если поддерживается более старая версия iOS).
 Такой API (и `[Obsolete]` атрибут) включаются в новые сборки Xamarin. iOS.
- **Неверный API** Некоторые API имеют опечатки в своих именах.

Для исходных сборок (с нестрогими и Ксаммак. dll) мы сохранили старый код для обеспечения совместимости, но они были удалены из Unified API сборок (Xamarin. iOS. dll и Xamarin. Mac).

<a name="NSObject_ctor" />

### <a name="nsobject-subclasses-ctorintptr"></a>Подклассы Нсобжект. ctor (IntPtr)

Каждый `NSObject` подкласс имеет конструктор, `IntPtr`принимающий. Таким способом можно создать экземпляр нового управляемого экземпляра из собственного обработчика ObjC.

Класс Classic является `public` конструктором. Однако было бы легко неправильно расставить эту функцию в пользовательский код, например создать несколько управляемых экземпляров для одного экземпляра ObjC *или* создать управляемый экземпляр, в котором отсутствует ожидаемое управляемое состояние (для подклассов).

Чтобы избежать проблем `IntPtr` такого рода, конструкторы теперь `protected` находятся в **унифицированном** API, чтобы их можно было использовать только для создания подклассов. Это обеспечит использование правильного и безопасного API для создания управляемого экземпляра из дескрипторов, т. е.

```csharp
var label = Runtime.GetNSObject<UILabel> (handle);
```

Этот API вернет существующий управляемый экземпляр (если он уже существует) или создаст новый (при необходимости). Он уже доступен как в классическом, так и в унифицированном API.

Обратите внимание `.ctor(NSObjectFlag)` , что теперь `protected` объект также используется редко за пределами подклассов.

<a name="NSAction" />

### <a name="nsaction-replaced-with-action"></a>Нсактион заменено действием

При использовании унифицированных API `NSAction` -интерфейсов был удален в пользу стандартного .NET `Action`. Это значительное улучшение, так как `Action` является распространенным типом .NET, `NSAction` тогда как это характерно для Xamarin. iOS. Они выполняют точно одно и то же действие, но они были разными и несовместимыми типами и привели к тому, что для достижения того же результата потребуется написать код.

Например, если существующее приложение Xamarin включало следующий код:

```csharp
UITapGestureRecognizer singleTap = new UITapGestureRecognizer (new NSAction (delegate() {
    ShowDropDownAnimated (tblDataView);
}));
```

Теперь его можно заменить простой лямбда-выражением:

```csharp
UITapGestureRecognizer singleTap = new UITapGestureRecognizer (() => ShowDropDownAnimated(tblDataView));
```

Раньше это было бы `Action` ошибкой компилятора `NSAction`, поскольку нельзя присвоить значение, `Action` но так `UITapGestureRecognizer` как теперь в унифицированных API `NSAction` -интерфейсах действует вместо него.

### <a name="custom-delegates-replaced-with-actiont"></a>Пользовательские делегаты, замененные на действие\<T >

В **унифицированных** простых (например, один параметр) делегаты .NET были заменены `Action<T>`на. Например,

```csharp
public delegate void NSNotificationHandler (NSNotification notification);
```

Теперь можно использовать в качестве `Action<NSNotification>`. Это способствует повторному использованию кода и сокращению дублирования кода внутри Xamarin. iOS и ваших собственных приложений.

### <a name="taskbool-replaced-with-taskbooleannserror"></a>Логическое\<> задачи заменено <ой задачи Boolean, нсеррор > >

В **классической** модели были возвращены `Task<bool>`некоторые асинхронные API. Однако некоторые из них используются, когда `NSError` является частью сигнатуры, т `bool` . е. уже `true` существовала и `NSError`пришлось бы перехватить исключение, чтобы получить.

Так как некоторые ошибки очень распространены и возвращаемое значение не было полезным, этот шаблон был изменен в **унифицированном** виде `Task<Tuple<Boolean,NSError>>`для возврата. Это позволяет проверить как успешность, так и любые ошибки, которые могли произойти во время асинхронного вызова.

### <a name="nsstring-vs-string"></a>NSString и строка

В некоторых случаях требуется изменить некоторые константы с `string` на `NSString`, например`UITableViewCell`

**Классический**

```csharp
public virtual string ReuseIdentifier { get; }
```

**Объединенная**

```csharp
public virtual NSString ReuseIdentifier { get; }
```

В общем случае предпочтительнее тип `System.String` .NET. Однако, несмотря на рекомендации Apple, некоторый собственный API сравнивает постоянные указатели (а не саму строку), и это может работать только при предоставлении констант как `NSString`.

 <a name="protocols" />

### <a name="objective-c-protocols"></a>Протоколы цели-C

Первоначальная котушь не поддерживала полную поддержку протоколов ObjC, а некоторые неоптимальные API были добавлены для поддержки наиболее распространенного сценария. Это ограничение больше не существует, но для обеспечения обратной совместимости несколько API хранятся внутри `monotouch.dll` и. `XamMac.dll`

Эти ограничения были удалены и очищены в единых интерфейсах API. Большинство изменений будут выглядеть следующим образом:

**Классический**

```csharp
public virtual AVAssetResourceLoaderDelegate Delegate { get; }
```

**Объединенная**

```csharp
public virtual IAVAssetResourceLoaderDelegate Delegate { get; }
```

Префикс означает, что для протокола ObjC вместо конкретного типа предоставляется интерфейс, а не конкретный тип. `I` Это упростит случаи, когда вам не нужно подделать подкласс конкретного типа, предоставленного Xamarin. iOS.

Кроме того, было разрешено, что некоторые API были более точными и простыми в использовании, например:

**Классический**

```csharp
public virtual void SelectionDidChange (NSObject uiTextInput);
```

**Объединенная**

```csharp
public virtual void SelectionDidChange (IUITextInput uiTextInput);
```

Такой API теперь проще, без ссылки на документацию, и завершение кода интегрированной среды разработки предоставит вам более полезные предложения на основе протокола или интерфейса.

#### <a name="nscoding-protocol"></a>Протокол Нскодинг

Исходная привязка включала. ctor (нскодер) для каждого типа, даже если он не поддерживал `NSCoding` протокол.  Для кодирования `Encode(NSCoder)` объекта имеется один метод. `NSObject`
Но этот метод будет работать только в том случае, если экземпляр соответствует протоколу Нскодинг.

На Unified API мы устранили это.  Новые сборки будут иметь только, `.ctor(NSCoder)` если тип `NSCoding`соответствует. Кроме того, такие типы теперь `Encode(NSCoder)` имеют метод, который соответствует `INSCoding` интерфейсу.

Низкая степень влияния: В большинстве случаев это изменение не повлияет на приложения, так как старые, удаленные конструкторы не могут использоваться.

## <a name="further-tips"></a>Дополнительные советы

Дополнительные изменения, о которых следует помнить, перечислены в [подсказках по обновлению приложений до Unified API](~/cross-platform/macios/unified/updating-tips.md).

## <a name="sample-code"></a>Пример кода

По состоянию на 31 июля мы опубликовали порты образцов iOS для этого нового API в `magic-types` ветви в виде [примеров](https://github.com/xamarin/monotouch-samples/commits/magic-types).

Для Mac мы проверяя примеры в репозитории [примеров Mac](https://github.com/xamarin/mac-samples) (в которых показаны новые API-интерфейсы в Mavericks/Yosemite), а также 32/64 разрядных примеров в подразделениях "волшебных типов" в [Mac-Samples](https://github.com/xamarin/monotouch-samples/commits/magic-types).

## <a name="related-links"></a>Связанные ссылки

- [Обновление приложений iOS](updating-ios-apps.md)
- [Обновление приложений Mac](updating-mac-apps.md)
- [Обновление приложений Xamarin. Forms](updating-xamarin-forms-apps.md)
- [Обновление привязок](update-binding.md)
- [Обновление советов](updating-tips.md)
- [Различия между классическими и Unified APIми](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/api_changes/classic-vs-unified-8.6.0/index.md)
- [Работа с собственными типами в кроссплатформенных приложениях](~/cross-platform/macios/native-types-cross-platform.md)
