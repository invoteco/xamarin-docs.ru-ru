---
title: Советы по обновлению кода в Unified API
description: В этом документе обсуждаются распространенные ошибки и советы, полезные при обновлении приложения для использования Unified API Xamarin.
ms.prod: xamarin
ms.assetid: 8DD34D21-342C-48E9-97AA-1B649DD8B61F
ms.date: 03/29/2017
author: davidortinau
ms.author: daortin
ms.openlocfilehash: ee207ffc83f887e9c86c650b6f93fc2ff9f5b69c
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73014995"
---
# <a name="tips-for-updating-code-to-the-unified-api"></a>Советы по обновлению кода в Unified API

При обновлении старых решений Xamarin до Unified API могут возникать следующие ошибки.

## <a name="nsinvalidargumentexception-could-not-find-storyboard-error"></a>Нсинвалидаргументексцептион не удалось найти ошибку раскадровки

В текущей версии Visual Studio для Mac есть [Ошибка](https://bugzilla.xamarin.com/show_bug.cgi?id=25569) , которая может возникнуть после использования средства автоматической миграции для преобразования проекта в единые API. Если после обновления появится сообщение об ошибке в форме:

```console
Objective-C exception thrown. Name: NSInvalidArgumentException Reason: Could not find a storyboard named 'xxx' in bundle NSBundle...
```

Чтобы решить эту проблему, можно выполнить следующие действия. для этого нужно найти следующий целевой файл сборки:

```console
/Library/Frameworks/Xamarin.iOS.framework/Versions/Current/lib/mono/2.1/Xamarin.iOS.Common.targets
```

В этом файле необходимо найти следующее объявление целевого объекта:

```xml
<Target Name="_CopyContentToBundle"
        Inputs = "@(_BundleResourceWithLogicalName)"
        Outputs = "@(_BundleResourceWithLogicalName -> '$(_AppBundlePath)%(LogicalName)')" >
```

И добавьте к нему атрибут `DependsOnTargets="_CollectBundleResources"`. Пример:

```xml
<Target Name="_CopyContentToBundle"
        DependsOnTargets="_CollectBundleResources"
        Inputs = "@(_BundleResourceWithLogicalName)"
        Outputs = "@(_BundleResourceWithLogicalName -> '$(_AppBundlePath)%(LogicalName)')" >
```

Сохраните файл, перезагрузите Visual Studio для Mac и выполните очистку & перестроения проекта. Исправление для этой проблемы должно быть выпущено Xamarin вскоре.

## <a name="useful-tips"></a>Полезные советы

После использования средства миграции могут возникать некоторые ошибки компилятора, требующие вмешательства вручную.
Некоторые вещи, которые может потребоваться исправить вручную, включают:

- Для сравнения `enum`может потребоваться приведение `(int)`.

- `NSDictionary.IntValue` теперь возвращает `nint`, вместо него можно использовать `Int32Value`.

- типы `nfloat` и `nint` не могут быть помечены как `const`; `static readonly nint` является разумной альтернативой.

- Элементы, которые были непосредственно использованы в пространстве имен `MonoTouch.`, теперь обычно находятся в пространстве имен `ObjCRuntime.` (например, `MonoTouch.Constants.Version` теперь `ObjCRuntime.Constants.Version`).

- Код, который сериализует объекты, может прерываться при попытке сериализовать типы `nint` и `nfloat`. Убедитесь, что код сериализации работает правильно после миграции.

- Иногда автоматизированное средство пропустило код внутри `#if #else` условных директив компилятора. В этом случае необходимо внести исправления вручную (см. типичные ошибки ниже).

- Экспортированные вручную методы, использующие `[Export]`, не могут быть автоматически исправлены средством миграции, например в этом коде снипперт необходимо вручную обновить тип возвращаемого значения на `nfloat`:

  ```csharp
  [Export("tableView:heightForRowAtIndexPath:")]
  public nfloat HeightForRow(UITableView tableView, NSIndexPath indexPath)
  ```

- Unified API не обеспечивает неявное преобразование между Нсдате и .NET DateTime, так как оно не является преобразованием без потерь. Чтобы предотвратить ошибки, связанные с `DateTimeKind.Unspecified` преобразуйте `DateTime` .NET в локальную или UTC перед приведением к `NSDate`.

- Методы категории цели-C теперь создаются как методы расширения в Unified API. Например, код, использовавшийся ранее `UIView.DrawString`, теперь будет ссылаться `NSString.DrawString` в Unified API.

- Код, использующий классы Авфаундатион с `VideoSettings`, должен измениться на использование свойства `WeakVideoSettings`. Для этого требуется `Dictionary`, который доступен как свойство в классах параметров, например:

  ```csharp
  vidrec.WeakVideoSettings = new AVVideoSettings() { ... }.Dictionary;
  ```

- Конструктор `.ctor(IntPtr)` Нсобжект был изменен с общедоступного на защищенный ([чтобы предотвратить неправильное использование](~/cross-platform/macios/unified/overview.md#NSObject_ctor)).

- `NSAction` [заменена](~/cross-platform/macios/unified/overview.md#NSAction) стандартными `Action`ами .NET. Некоторые простые делегаты (один параметр) также были заменены `Action<T>`.

Наконец, ознакомьтесь с [классическими отличиями v Unified API](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/api_changes/classic-vs-unified-8.6.0/index.md) , чтобы найти изменения в интерфейсах API в коде. Поиск на [этой странице](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/api_changes/classic-vs-unified-8.6.0/index.md) поможет найти классические API и то, до чего они были обновлены.

> [!NOTE]
> Пространство имен `MonoTouch.Dialog` остается неизменным после миграции. Если код использует **Однокасание. диалоговое окно** следует продолжать использовать это пространство имен — *не* изменяйте `MonoTouch.Dialog` на `Dialog`!

## <a name="common-compiler-errors"></a>Распространенные ошибки компилятора

Ниже приведены другие примеры распространенных ошибок, а также решение.

**Ошибка CS0012: тип "with Touch. UIKit. UIView" определен в сборке, на которую нет ссылок.**

Исправление. обычно это означает, что проект ссылается на компонент или пакет NuGet, который не был построен с Unified API. Следует удалить и повторно добавить все компоненты и пакеты NuGet. Если это не устраняет ошибку, возможно, внешняя библиотека пока не поддерживает Unified API.

**Ошибка MT0034: не может одновременно включать "неtouch. dll" и "Xamarin. iOS. dll" в один и тот же проект Xamarin. iOS — "Xamarin. iOS. dll" ссылается явно, а "Xamarin. Mobile" Version = 0.6.3.0, Culture = Neutral, PublicKeyToken = null '.**

Исправление: удалите компонент, который вызывает эту ошибку, и повторно добавьте его в проект.

**Ошибка CS0234: тип или имя пространства имен "Foundation" не существует в пространстве имен "with Touch". Пропущена ли ссылка на сборку?**

Исправление. Средство автоматической миграции в Visual Studio для Mac *должно* обновить все `MonoTouch.Foundation` ссылки на `Foundation`, однако в некоторых случаях их потребуется обновить вручную. Аналогичные ошибки могут отображаться для других пространств имен, ранее содержащихся в `MonoTouch`, например `UIKit`.

**Ошибка CS0266: невозможно неявно преобразовать тип "double" в "System. float"**

Исправление. Измените тип и приведите к `nfloat`. Эта ошибка также может возникать для других типов с 64-разрядными эквивалентами (например, `nint`,).

```csharp
nfloat scale = (nfloat)Math.Min(rect.Width, rect.Height);
```

**Ошибка CS0266: невозможно неявно преобразовать тип "Кореграфикс. Кгрект" в "System. Drawing. Ректанглеф". Существует явное преобразование (возможно, отсутствует приведение?)**

Исправление. Измените экземпляры на `RectangleF` на `CGRect`, `SizeF` `CGSize`и `PointF` в `CGPoint`. `using System.Drawing;` пространства имен следует заменить `using CoreGraphics;` (если оно еще не указано).

**ошибка CS1502: наиболее подходящий перегруженный метод для "Кореграфикс. Кгконтекст. Сетлинедаш (System. нфлоат, System. нфлоат [])" имеет несколько недопустимых аргументов**

Исправление. Измените тип массива на `nfloat[]` и выполните явное приведение `Math.PI`.

```csharp
grphc.SetLineDash (0, new nfloat[] { 0, 3 * (nfloat)Math.PI });
```

**Ошибка CS0115: "Вордстаблесаурце. Ровсинсектион (UIKit. Уитаблевиев, int)" помечен как переопределение, но не найден подходящий метод для переопределения**

Исправление: Измените возвращаемое значение и типы параметров на `nint`. Обычно это происходит в переопределениях методов, таких как `UITableViewSource`, включая `RowsInSection`, `NumberOfSections`, `GetHeightForRow`, `TitleForHeader`, `GetViewForHeader`и т. д.

```csharp
public override nint RowsInSection (UITableView tableview, nint section) {
```

**Ошибка CS0508: `WordsTableSource.NumberOfSections(UIKit.UITableView)`: возвращаемый тип должен быть "System. НИНТ", чтобы соответствовать переопределенному члену `UIKit.UITableViewSource.NumberOfSections(UIKit.UITableView)`**

Исправление: когда тип возвращаемого значения изменяется на `nint`, приведите возвращаемое значение к `nint`.

```csharp
public override nint NumberOfSections (UITableView tableView)
{
    return (nint)navItems.Count;
}
```

**Ошибка CS1061: тип "Кореграфикс. Кгпас" не содержит определения для "Адделипсеинрект"**

Исправление: исправьте орфографию для `AddEllipseInRect`. Другие изменения имен включают:

- Измените цвет. Black на `NSColor.Black`.
- Измените Мапкит "Адданнотатион" на "`AddAnnotations`".
- Измените Авфаундатион "Датаусинженкодинг" на "`Encode`".
- Измените Авфаундатион "Авметадатаобжект. Типекркоде" на `AVMetadataObjectType.QRCode`.
- Измените Авфаундатион "Видеосеттингс" на "`WeakVideoSettings`".
- Измените Попвиевконтроллераниматед на `PopViewController`.
- Измените Кореграфикс "Кгбитмапконтекст. Сетргбфиллколор" на `SetFillColor`.

**Ошибка CS0546: не удается переопределить, так как "Мапкит. Мканнотатион. координировать" не имеет метода доступа set, доступного для переопределения (CS0546)**

При создании пользовательской заметки с помощью подкласса Мканнотатион поле координаты не имеет метода задания, а только метода получения.

[Исправление](https://forums.xamarin.com/discussion/comment/109505/#Comment_109505):

- Добавление поля для отслеживания координат
- возвратить это поле в методе получения свойства координаты
- Переопределите метод Сеткурдинате и задайте поле
- Вызовите Сеткурдинате в ctor с помощью переданного параметра координаты.

Он должен выглядеть следующим образом:

```csharp
class BasicPinAnnotation : MKAnnotation
{
    private CLLocationCoordinate2D _coordinate;

    public override CLLocationCoordinate2D Coordinate
    {
        get
        {
            return _coordinate;
        }
    }

    public override void SetCoordinate(CLLocationCoordinate2D value)
    {
        _coordinate = value;
    }

    public BasicPinAnnotation (CLLocationCoordinate2D coordinate)
    {
        SetCoordinate(coordinate);
    }
}
```

## <a name="related-links"></a>Связанные ссылки

- [Обновление приложений](~/cross-platform/macios/unified/updating-apps.md)
- [Обновление приложений iOS](~/cross-platform/macios/unified/updating-ios-apps.md)
- [Обновление приложений Mac](~/cross-platform/macios/unified/updating-mac-apps.md)
- [Обновление приложений Xamarin. Forms](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md)
- [Обновление привязок](~/cross-platform/macios/unified/update-binding.md)
- [Работа с собственными типами в кроссплатформенных приложениях](~/cross-platform/macios/native-types-cross-platform.md)
- [Различия между классическими и Unified APIми](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/api_changes/classic-vs-unified-8.6.0/index.md)
