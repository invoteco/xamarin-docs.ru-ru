---
title: Особенности платформы
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать и создавать зависящие от платформы.
ms.prod: xamarin
ms.assetid: 4729DB9C-8800-4E29-9D66-3BE13C5F8C94
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/01/2018
ms.openlocfilehash: f6190b9c0d29d57d6d509bdff25e2ce3572e3a3c
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/13/2020
ms.locfileid: "79305735"
---
# <a name="platform-specifics"></a>Особенности платформы

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

_Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов._

Процесс для использования платформы через XAML или через API текучего кода выглядит следующим образом:

1. Добавьте объявление `xmlns` или директиву `using` для пространства имен [`Xamarin.Forms.PlatformConfiguration`](xref:Xamarin.Forms.PlatformConfiguration) .
1. Добавьте `xmlns` объявление или директиву `using` для пространства имен, содержащего функциональные возможности платформы:
    1. В iOS это пространство имен [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) .
    1. В Android это пространство имен [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) . Для Android AppCompat это пространство имен [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat) .
    1. На универсальная платформа Windows это пространство имен [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) .
1. Примените конкретную платформу на основе XAML или из кода с помощью API-интерфейса `On<T>` Fluent. Значением `T` может быть [`iOS`](xref:Xamarin.Forms.PlatformConfiguration.iOS), [`Android`](xref:Xamarin.Forms.PlatformConfiguration.Android)или [`Windows`](xref:Xamarin.Forms.PlatformConfiguration.Windows) типы из пространства имен [`Xamarin.Forms.PlatformConfiguration`](xref:Xamarin.Forms.PlatformConfiguration) .

> [!NOTE]
> Обратите внимание на то, что попытка использовать платформы на платформе, где он недоступен не приведет ошибку. Вместо этого код будет выполняться без применения конкретной платформы.

Зависящие от платформы функции, используемые с помощью API кода `On<T>` Fluent, возвращают [`IPlatformElementConfiguration`](xref:Xamarin.Forms.IPlatformElementConfiguration`2) объектов. Благодаря этому несколько особенностей платформы должен быть вызван на один и тот же объект с каскадным метод.

Дополнительные сведения о конкретных платформах, предоставляемых Xamarin. Forms, см. в разделе [особенности платформы iOS](~/xamarin-forms/platform/ios/index.md), [особенности платформы Android](~/xamarin-forms/platform/android/index.md)и [особенности платформы Windows](~/xamarin-forms/platform/windows/index.md).

## <a name="creating-platform-specifics"></a>Создание особенностей платформы

Поставщики могут создавать собственные платформы с применением эффектов. Эффект предоставляет определенных функциональных возможностей, которые затем передаются через конкретную платформу. Результатом является эффект, можно было более легко через XAML и текучего кода API.

Процесс создания конкретной платформы выглядит следующим образом:

1. Реализуйте определенные функции, как эффект. Дополнительные сведения см. [в разделе Создание влияния](~/xamarin-forms/app-fundamentals/effects/creating.md).
1. Создание класса платформы, который будет предоставить эффект. Дополнительные сведения см. [в разделе Создание класса, зависящего от платформы](#creating-a-platform-specific-class).
1. В классе платформы реализации присоединенного свойства, чтобы разрешить конкретную платформу быть востребован XAML. Дополнительные сведения см. в разделе [Добавление присоединенного свойства](#adding-an-attached-property).
1. В классе платформы реализации методов расширения, чтобы разрешить конкретную платформу для использования через API текучего кода. Дополнительные сведения см. в разделе [Добавление методов расширения](#adding-extension-methods).
1. Измените реализацию эффект, эффект применяется только в том случае, если был вызван с платформой на той же платформе, в результате. Дополнительные сведения см. [в разделе Создание результата](#creating-the-effect).

Предоставление эффект как от платформы образом, результат можно было более легко через XAML и текучего кода API.

> [!NOTE]
> Он является предусмотрено, что этот способ будет использоваться поставщиков для создания собственных особенностей платформы, для упрощения потребления пользователями. Хотя пользователи могут начать создавать свои собственные особенности платформы, следует отметить, что он требует больше кода, чем создание и использование эффекта.

[Пример приложения](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shadowplatformspecific) демонстрирует `Shadow` конкретной платформы, которая добавляет тень к тексту, отображаемому элементом управления [`Label`](xref:Xamarin.Forms.Label) :

![](images/screenshots.png "Shadow Platform-Specific")

[Пример приложения](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shadowplatformspecific) реализует `Shadow`, зависящие от платформы, для простоты понимания. Тем не менее помимо Каждая реализация эффект платформой, реализация класса теневой копии мало чем отличается для каждой платформы. Таким образом это руководство посвящено реализации класса теневой копии и связанные влияет на единую платформу.

Дополнительные сведения о влиянии см. [в разделе Настройка элементов управления с помощью эффектов](~/xamarin-forms/app-fundamentals/effects/index.md).

### <a name="creating-a-platform-specific-class"></a>Создание класса, зависящего от платформы

Конкретная платформа создается как класс `public static`:

```csharp
namespace MyCompany.Forms.PlatformConfiguration.iOS
{
  public static Shadow
  {
    ...
  }
}
```

В следующих разделах обсуждается реализация `Shadow`ного и связанного с платформой действия.

#### <a name="adding-an-attached-property"></a>Добавление присоединенного свойства

Присоединенное свойство необходимо добавить в `Shadow`, зависящее от платформы, чтобы разрешить использование с помощью XAML:

```csharp
namespace MyCompany.Forms.PlatformConfiguration.iOS
{
    using System.Linq;
    using Xamarin.Forms;
    using Xamarin.Forms.PlatformConfiguration;
    using FormsElement = Xamarin.Forms.Label;

    public static class Shadow
    {
        const string EffectName = "MyCompany.LabelShadowEffect";

        public static readonly BindableProperty IsShadowedProperty =
            BindableProperty.CreateAttached("IsShadowed",
                                            typeof(bool),
                                            typeof(Shadow),
                                            false,
                                            propertyChanged: OnIsShadowedPropertyChanged);

        public static bool GetIsShadowed(BindableObject element)
        {
            return (bool)element.GetValue(IsShadowedProperty);
        }

        public static void SetIsShadowed(BindableObject element, bool value)
        {
            element.SetValue(IsShadowedProperty, value);
        }

        ...

        static void OnIsShadowedPropertyChanged(BindableObject element, object oldValue, object newValue)
        {
            if ((bool)newValue)
            {
                AttachEffect(element as FormsElement);
            }
            else
            {
                DetachEffect(element as FormsElement);
            }
        }

        static void AttachEffect(FormsElement element)
        {
            IElementController controller = element;
            if (controller == null || controller.EffectIsAttached(EffectName))
            {
                return;
            }
            element.Effects.Add(Effect.Resolve(EffectName));
        }

        static void DetachEffect(FormsElement element)
        {
            IElementController controller = element;
            if (controller == null || !controller.EffectIsAttached(EffectName))
            {
                return;
            }

            var toRemove = element.Effects.FirstOrDefault(e => e.ResolveId == Effect.Resolve(EffectName).ResolveId);
            if (toRemove != null)
            {
                element.Effects.Remove(toRemove);
            }
        }
    }
}
```

Присоединенное свойство `IsShadowed` используется для добавления `MyCompany.LabelShadowEffect`ного результата к элементу управления, к которому присоединен класс `Shadow`, и удаления его из него. Это присоединенное свойство регистрирует метод `OnIsShadowedPropertyChanged`, который будет выполняться при изменении значения свойства. В свою очередь, этот метод вызывает метод `AttachEffect` или `DetachEffect`, чтобы добавить или удалить результат на основе значения присоединенного свойства `IsShadowed`. Этот результат добавляется в элемент управления или удаляется из него путем изменения коллекции [`Effects`](xref:Xamarin.Forms.Element.Effects) элемента управления.

> [!NOTE]
> Обратите внимание, что эффект разрешается, указав значение, которое представляет собой объединение имени группы разрешения и уникальный идентификатор, который указан в реализации эффект. Дополнительные сведения см. [в разделе Создание влияния](~/xamarin-forms/app-fundamentals/effects/creating.md).

Дополнительные сведения о присоединенных свойствах см. в разделе [Присоединенные свойства](~/xamarin-forms/xaml/attached-properties.md).

#### <a name="adding-extension-methods"></a>Добавления методов расширения

Методы расширения необходимо добавить в `Shadow`, зависящие от платформы, чтобы разрешить использование через API кода Fluent:

```csharp
namespace MyCompany.Forms.PlatformConfiguration.iOS
{
    using System.Linq;
    using Xamarin.Forms;
    using Xamarin.Forms.PlatformConfiguration;
    using FormsElement = Xamarin.Forms.Label;

    public static class Shadow
    {
        ...
        public static bool IsShadowed(this IPlatformElementConfiguration<iOS, FormsElement> config)
        {
            return GetIsShadowed(config.Element);
        }

        public static IPlatformElementConfiguration<iOS, FormsElement> SetIsShadowed(this IPlatformElementConfiguration<iOS, FormsElement> config, bool value)
        {
            SetIsShadowed(config.Element, value);
            return config;
        }
        ...
    }
}
```

Методы расширения `IsShadowed` и `SetIsShadowed` вызывают метод доступа get и Set для `IsShadowed` присоединенного свойства соответственно. Каждый метод расширения работает с типом `IPlatformElementConfiguration<iOS, FormsElement>`, который указывает, что конкретная платформа может быть вызвана на экземплярах [`Label`](xref:Xamarin.Forms.Label) из iOS.

#### <a name="creating-the-effect"></a>Создание результата

`Shadow`, зависящая от платформы, добавляет `MyCompany.LabelShadowEffect` в [`Label`](xref:Xamarin.Forms.Label)и удаляет ее. В следующем примере кода показана реализация класса `LabelShadowEffect` для проекта на платформе iOS.

```csharp
[assembly: ResolutionGroupName("MyCompany")]
[assembly: ExportEffect(typeof(LabelShadowEffect), "LabelShadowEffect")]
namespace ShadowPlatformSpecific.iOS
{
    public class LabelShadowEffect : PlatformEffect
    {
        protected override void OnAttached()
        {
            UpdateShadow();
        }

        protected override void OnDetached()
        {
        }

        protected override void OnElementPropertyChanged(PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged(args);

            if (args.PropertyName == Shadow.IsShadowedProperty.PropertyName)
            {
                UpdateShadow();
            }
        }

        void UpdateShadow()
        {
            try
            {
                if (((Label)Element).OnThisPlatform().IsShadowed())
                {
                    Control.Layer.CornerRadius = 5;
                    Control.Layer.ShadowColor = UIColor.Black.CGColor;
                    Control.Layer.ShadowOffset = new CGSize(5, 5);
                    Control.Layer.ShadowOpacity = 1.0f;
                }
                else if (!((Label)Element).OnThisPlatform().IsShadowed())
                {
                    Control.Layer.ShadowOpacity = 0;
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine("Cannot set property on attached control. Error: ", ex.Message);
            }
        }
    }
}
```

Метод `UpdateShadow` задает `Control.Layer` свойства для создания тени, при условии, что присоединяемое свойство `IsShadowed` имеет значение `true`, и при условии, что на той же платформе была вызвана конкретная платформа `Shadow`, для которой реализуется эффект. Эта проверка выполняется с помощью метода `OnThisPlatform`.

Если значение присоединенного свойства `Shadow.IsShadowed` изменяется во время выполнения, то эффект должен ответить, удалив тень. Таким образом, переопределенная версия метода `OnElementPropertyChanged` используется для реагирования на изменение свойства, доступного для привязки, путем вызова метода `UpdateShadow`.

Дополнительные сведения о создании эффектов см. в разделе [Создание эффектов](~/xamarin-forms/app-fundamentals/effects/creating.md) и [Передача параметров эффектов в качестве вложенных свойств](~/xamarin-forms/app-fundamentals/effects/passing-parameters/attached-properties.md).

### <a name="consuming-the-platform-specific"></a>Использование конкретной платформы

`Shadow`, зависящая от платформы, используется в XAML путем задания для присоединенного свойства `Shadow.IsShadowed` значения `boolean`.

```xaml
<ContentPage xmlns:ios="clr-namespace:MyCompany.Forms.PlatformConfiguration.iOS" ...>
  ...
  <Label Text="Label Shadow Effect" ios:Shadow.IsShadowed="true" ... />
  ...
</ContentPage>
```

Кроме того его можно будет использовать с помощью C# с помощью текучего API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using MyCompany.Forms.PlatformConfiguration.iOS;

...

shadowLabel.On<iOS>().SetIsShadowed(true);
```

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [ШадовплатформспеЦифик (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shadowplatformspecific)
- [Особенности платформы iOS](~/xamarin-forms/platform/ios/index.md)
- [Особенности платформы Android](~/xamarin-forms/platform/android/index.md)
- [Особенности платформы Windows](~/xamarin-forms/platform/windows/index.md)
- [Настройка элементов управления с помощью эффектов](~/xamarin-forms/app-fundamentals/effects/index.md)
- [Вложенные свойства](~/xamarin-forms/xaml/attached-properties.md)
- [API Платформконфигуратион](xref:Xamarin.Forms.PlatformConfiguration)
