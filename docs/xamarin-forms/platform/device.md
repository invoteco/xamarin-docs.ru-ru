---
title: Класс устройств Xamarin.Forms
description: В этой статье объясняется, как использовать класс устройств Xamarin.Forms, возможность точного управления функциональные возможности и макеты на каждой платформы.
ms.prod: xamarin
ms.assetid: 2F304AEC-8612-4833-81E5-B2F3F469B2DF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/12/2019
ms.openlocfilehash: 25ddbea75d0fd6858f848499281da5d5f0b68171
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/13/2020
ms.locfileid: "79306431"
---
# <a name="xamarinforms-device-class"></a>Класс устройств Xamarin.Forms

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithdevice)

Класс [`Device`](xref:Xamarin.Forms.Device) содержит ряд свойств и методов, помогающих разработчикам настраивать макет и функциональность для отдельных платформ.

Помимо методов и свойств, предназначенных для целевого кода на конкретных типах и размерах оборудования, класс `Device` содержит методы, которые можно использовать для взаимодействия с элементами управления пользовательского интерфейса из фоновых потоков. Дополнительные сведения см. в разделе [взаимодействие с интерфейсом пользователя из фоновых потоков](#interact-with-the-ui-from-background-threads).

## <a name="providing-platform-specific-values"></a>Предоставление значений для конкретной платформы

До Xamarin. Forms 2.3.4 платформа, в которой выполнялось приложение, можно получить, изучив свойство [`Device.OS`](xref:Xamarin.Forms.Device.OS) и сравнив его со значениями перечисления [`TargetPlatform.iOS`](xref:Xamarin.Forms.TargetPlatform.iOS), [`TargetPlatform.Android`](xref:Xamarin.Forms.TargetPlatform.Android), [`TargetPlatform.WinPhone`](xref:Xamarin.Forms.TargetPlatform.WinPhone)и [`TargetPlatform.Windows`](xref:Xamarin.Forms.TargetPlatform.Windows) . Аналогичным образом один из [`Device.OnPlatform`](xref:Xamarin.Forms.Device.OnPlatform(System.Action,System.Action,System.Action,System.Action)) перегрузок можно использовать для предоставления для элемента управления значений, зависящих от платформы.

Тем не менее поскольку Xamarin.Forms 2.3.4 эти API устарели и заменены. Класс [`Device`](xref:Xamarin.Forms.Device) теперь содержит константы открытых строк, которые обозначают платформы: [`Device.iOS`](xref:Xamarin.Forms.Device.iOS), [`Device.Android`](xref:Xamarin.Forms.Device.Android), `Device.WinPhone`(устарело), `Device.WinRT` (не рекомендуется), [`Device.UWP`](xref:Xamarin.Forms.Device.UWP)и [`Device.macOS`](xref:Xamarin.Forms.Device.macOS). Аналогичным образом перегрузки [`Device.OnPlatform`](xref:Xamarin.Forms.Device.OnPlatform(System.Action,System.Action,System.Action,System.Action)) были заменены [`OnPlatform`](xref:Xamarin.Forms.OnPlatform`1) и [`On`](xref:Xamarin.Forms.On) API.

В C#значения, зависящие от платформы, можно предоставить, создав инструкцию `switch` в свойстве [`Device.RuntimePlatform`](xref:Xamarin.Forms.Device.RuntimePlatform) , а затем предоставив инструкции `case` для требуемых платформ:

```csharp
double top;
switch (Device.RuntimePlatform)
{
  case Device.iOS:
    top = 20;
    break;
  case Device.Android:
  case Device.UWP:
  default:
    top = 0;
    break;
}
layout.Margin = new Thickness(5, top, 5, 0);
```

Классы [`OnPlatform`](xref:Xamarin.Forms.OnPlatform`1) и [`On`](xref:Xamarin.Forms.On) предоставляют те же функции в XAML:

```xaml
<StackLayout>
  <StackLayout.Margin>
    <OnPlatform x:TypeArguments="Thickness">
      <On Platform="iOS" Value="0,20,0,0" />
      <On Platform="Android, UWP" Value="0,0,0,0" />
    </OnPlatform>
  </StackLayout.Margin>
  ...
</StackLayout>
```

Класс [`OnPlatform`](xref:Xamarin.Forms.OnPlatform`1) является универсальным классом, который должен быть создан с помощью атрибута `x:TypeArguments`, соответствующего целевому типу. В классе [`On`](xref:Xamarin.Forms.On) атрибут [`Platform`](xref:Xamarin.Forms.On.Platform) может принимать одно значение `string` или несколько `string` значений с разделителями-запятыми.

> [!IMPORTANT]
> Указание неверного значения `Platform` атрибута в классе `On` не приведет к ошибке. Вместо этого код будет выполняться без значения платформы, которые применяются.

Кроме того, расширение разметки `OnPlatform` можно использовать в XAML для настройки внешнего вида пользовательского интерфейса на уровне платформы. Дополнительные сведения см. в разделе [расширение разметки для платформы](~/xamarin-forms/xaml/markup-extensions/consuming.md#onplatform).

## <a name="deviceidiom"></a>Device.Idiom

Свойство `Device.Idiom` можно использовать для изменения макетов или функций в зависимости от устройства, на котором работает приложение. Перечисление [`TargetIdiom`](xref:Xamarin.Forms.TargetIdiom) содержит следующие значения:

- **Телефон** — устройства iPhone, iPod Touch и Android с узким пределом, чем 600 DIP. ^
- **Планшет** — iPad, устройства Windows и устройства Android, ширина которых превышает 600 DIP ^
- **Настольный** компьютер — возвращается только в [приложениях UWP](~/xamarin-forms/platform/windows/installation/index.md) на настольных компьютерах с windows 10 (Возвращает `Phone` на мобильных устройствах Windows, в том числе в сценариях с Continuum).
- **ТВ** – Tizen телевизионные устройства
- **Watch** — Tizen Watch Devices
- **Не поддерживается** — не используется

*значение ^ DIP не обязательно является физическим числом пикселей*

Свойство `Idiom` особенно полезно для создания макетов, которые используют преимущества более крупных экранов, например:

```csharp
if (Device.Idiom == TargetIdiom.Phone) {
    // layout views vertically
} else {
    // layout views horizontally for a larger display (tablet or desktop)
}
```

Класс [`OnIdiom`](xref:Xamarin.Forms.OnIdiom`1) предоставляет те же функции в XAML:

```xaml
<StackLayout>
    <StackLayout.Margin>
        <OnIdiom x:TypeArguments="Thickness">
            <OnIdiom.Phone>0,20,0,0</OnIdiom.Phone>
            <OnIdiom.Tablet>0,40,0,0</OnIdiom.Tablet>
            <OnIdiom.Desktop>0,60,0,0</OnIdiom.Desktop>
        </OnIdiom>
    </StackLayout.Margin>
    ...
</StackLayout>
```

Класс [`OnIdiom`](xref:Xamarin.Forms.OnPlatform`1) является универсальным классом, который должен быть создан с помощью атрибута `x:TypeArguments`, соответствующего целевому типу.

Кроме того, расширение разметки `OnIdiom` можно использовать в XAML для настройки внешнего вида пользовательского интерфейса на основе идиомы устройства, на котором выполняется приложение. Дополнительные сведения см. в разделе [расширение разметки onidiom](~/xamarin-forms/xaml/markup-extensions/consuming.md#onidiom).

## <a name="deviceflowdirection"></a>Device.FlowDirection

Значение [`Device.FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) извлекает значение перечисления [`FlowDirection`](xref:Xamarin.Forms.FlowDirection) , представляющее текущее направление потока, используемое устройством. Направление потока — это направление, в котором глаз человека перемещается по элементам пользовательского интерфейса на странице. Перечисление имеет следующие значения.

- [`LeftToRight`](xref:Xamarin.Forms.FlowDirection.LeftToRight)
- [`RightToLeft`](xref:Xamarin.Forms.FlowDirection.RightToLeft)
- [`MatchParent`](xref:Xamarin.Forms.FlowDirection.MatchParent)

В XAML [`Device.FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) значение можно получить с помощью расширения разметки `x:Static`.

```xaml
<ContentPage ... FlowDirection="{x:Static Device.FlowDirection}"> />
```

Эквивалентный код на C# является:

```csharp
this.FlowDirection = Device.FlowDirection;
```

Дополнительные сведения о направлении потока см. [в разделе локализация справа налево](~/xamarin-forms/app-fundamentals/localization/right-to-left.md).

## <a name="devicestyles"></a>Device.Styles

[Свойство`Styles`](~/xamarin-forms/user-interface/styles/index.md) содержит встроенные определения стилей, которые можно применить к некоторым элементам управления (например, `Label`) `Style` свойства. Ниже приведены доступные стили.

- BodyStyle
- CaptionStyle
- ListItemDetailTextStyle
- ListItemTextStyle
- SubtitleStyle
- TitleStyle

## <a name="devicegetnamedsize"></a>Device.GetNamedSize

`GetNamedSize` можно использовать при настройке [`FontSize`](~/xamarin-forms/user-interface/text/fonts.md) в C# коде:

```csharp
myLabel.FontSize = Device.GetNamedSize (NamedSize.Small, myLabel);
someLabel.FontSize = Device.OnPlatform (
      24,         // hardcoded size
      Device.GetNamedSize (NamedSize.Medium, someLabel),
      Device.GetNamedSize (NamedSize.Large, someLabel)
);
```

## <a name="devicestarttimer"></a>Device.StartTimer

Класс `Device` также содержит метод `StartTimer`, который предоставляет простой способ запуска зависимых от времени задач, которые работают в общем коде Xamarin. Forms, включая библиотеку .NET Standard. Передайте `TimeSpan`, чтобы задать интервал и вернуть `true`, чтобы таймер выполнялся или `false` для его завершения после текущего вызова.

```csharp
Device.StartTimer (new TimeSpan (0, 0, 60), () =>
{
    // do something every 60 seconds
    return true; // runs again, or false to stop
});
```

Если код внутри таймера взаимодействует с пользовательским интерфейсом (например, задание текста `Label` или отображение предупреждения), его следует выполнять в `BeginInvokeOnMainThread` выражении (см. ниже).

> [!NOTE]
> Классы `System.Timers.Timer` и `System.Threading.Timer` являются .NET Standard альтернативами использования метода `Device.StartTimer`.

## <a name="interact-with-the-ui-from-background-threads"></a>Взаимодействие с пользовательским интерфейсом из фоновых потоков

Большинство операционных систем, в том числе iOS, Android и универсальная платформа Windows, используют однопотоковую модель для кода, включающего пользовательский интерфейс. Этот поток часто называют *основным потоком* или *потоком пользовательского интерфейса*. Следствием этой модели является то, что весь код, обращающийся к элементам пользовательского интерфейса, должен выполняться в основном потоке приложения.

Приложения иногда используют фоновые потоки для выполнения потенциально длительных операций, таких как извлечение данных из веб-службы. Если код, выполняемый в фоновом потоке, должен иметь доступ к элементам пользовательского интерфейса, он должен запустить этот код в основном потоке.

Класс `Device` включает следующие методы `static`, которые можно использовать для взаимодействия с элементами пользовательского интерфейса из фоновых потоков.

| Метод | Аргументы | Результаты | Назначение |
|---|---|---|---|
| `BeginInvokeOnMainThread` | `Action` | `void` | Вызывает `Action` в основном потоке и не ожидает его завершения. |
| `InvokeOnMainThreadAsync<T>` | `Func<T>` | `Task<T>` | Вызывает объект `Func<T>` в основном потоке и ожидает его завершения. |
| `InvokeOnMainThreadAsync` | `Action` | `Task` | Вызывает объект `Action` в основном потоке и ожидает его завершения. |
| `InvokeOnMainThreadAsync<T>`| `Func<Task<T>>` | `Task<T>` | Вызывает объект `Func<Task<T>>` в основном потоке и ожидает его завершения. |
| `InvokeOnMainThreadAsync` | `Func<Task>` | `Task` | Вызывает объект `Func<Task>` в основном потоке и ожидает его завершения. |
| `GetMainThreadSynchronizationContextAsync` | | `Task<SynchronizationContext>` | Возвращает `SynchronizationContext` для основного потока. |

В следующем коде показан пример использования метода `BeginInvokeOnMainThread`:

```csharp
Device.BeginInvokeOnMainThread (() =>
{
    // interact with UI elements
});
```

## <a name="related-links"></a>Связанные ссылки

- [Пример устройства](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithdevice)
- [Пример стилей](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithstyles)
- [Устройство](xref:Xamarin.Forms.Device)
