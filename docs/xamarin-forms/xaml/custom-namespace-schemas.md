---
title: Схемы пользовательских пространств имен XAML в Xamarin. Forms
description: Пользовательскую схему пространства имен XAML можно определить с помощью класса XmlnsDefinitionAttribute, который указывает сопоставление между пользовательским URL-адресом и одним или несколькими пространствами имен CLR. Пользовательская схема пространства имен может быть использована в объявлениях пространств имен XAML.
ms.prod: xamarin
ms.assetid: FDF201A1-8C35-4569-A728-F9B0A0C5B31A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/21/2018
ms.openlocfilehash: d76b5eefcaf0edeb12f128c60e9b8fffff8bcf3b
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/21/2019
ms.locfileid: "68644709"
---
# <a name="xaml-custom-namespace-schemas-in-xamarinforms"></a>Схемы пользовательских пространств имен XAML в Xamarin. Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-customnamespaceschemas)

На типы в библиотеке можно ссылаться в XAML, объявляя пространство имен XAML для библиотеки, с помощью объявления namespace, указывающего имя пространства имен среды CLR и имя сборки:

```xaml
<ContentPage ...
             xmlns:controls="clr-namespace:MyCompany.Controls;assembly="MyCompany.Controls">
    ...
</ContentPage>
```

Однако указание пространства имен CLR и имени сборки в определении `xmlns` может быть непростой и подвержено ошибкам. Кроме того, может потребоваться несколько объявлений пространства имен XAML, если библиотека содержит типы в нескольких пространствах имен.

Альтернативный подход заключается в определении пользовательской схемы пространства имен, например `http://mycompany.com/schemas/controls`, которая сопоставляется с одним или несколькими пространствами имен среды CLR. Это позволяет использовать одно объявление пространства имен XAML для ссылки на все типы в сборке, даже если они находятся в разных пространствах имен. Он также позволяет использовать одно объявление пространства имен XAML для ссылки на типы в нескольких сборках.

Дополнительные сведения о пространствах имен XAML см. [в разделе пространства имен XAML в Xamarin. Forms](namespaces.md).

## <a name="defining-a-custom-namespace-schema"></a>Определение пользовательской схемы пространства имен

Пример приложения содержит библиотеку, предоставляющую некоторые простые элементы управления, например `CircleButton`:

```csharp
using Xamarin.Forms;

namespace MyCompany.Controls
{
    public class CircleButton : Button
    {
        ...
    }
}
```

Все элементы управления в библиотеке находятся в пространстве имен `MyCompany.Controls`. Эти элементы управления могут быть предоставлены вызывающей сборке через пользовательскую схему пространства имен.

Схема пользовательского пространства имен определяется с помощью класса `XmlnsDefinitionAttribute`, который задает сопоставление между пространством имен XAML и одним или несколькими пространствами имен CLR. @No__t_0 принимает два аргумента: имя пространства имен XAML и имя пространства имен CLR. Имя пространства имен XAML хранится в свойстве `XmlnsDefinitionAttribute.XmlNamespace`, а имя пространства имен CLR хранится в свойстве `XmlnsDefinitionAttribute.ClrNamespace`.

> [!NOTE]
> Класс `XmlnsDefinitionAttribute` также имеет свойство с именем `AssemblyName`, которое можно дополнительно задать для имени сборки. Это необходимо только в том случае, если пространство имен CLR, на которое ссылается `XmlnsDefinitionAttribute`, находится во внешней сборке.

@No__t_0 должны быть определены на уровне сборки в проекте, содержащем пространства имен CLR, которые будут сопоставлены в схеме пользовательского пространства имен. В следующем примере показан файл **AssemblyInfo.CS** из примера приложения:

```csharp
using Xamarin.Forms;
using MyCompany.Controls;

[assembly: Preserve]
[assembly: XmlnsDefinition("http://mycompany.com/schemas/controls", "MyCompany.Controls")]
```

Этот код создает пользовательскую схему пространства имен, которая сопоставляет URL-адрес `http://mycompany.com/schemas/controls` с пространством имен `MyCompany.Controls` CLR. Кроме того, для сборки задается атрибут `Preserve`, чтобы компоновщик сохранял все типы в сборке.

> [!IMPORTANT]
> Атрибут `Preserve` должен применяться к классам в сборке, которые сопоставляются с пользовательской схемой пространства имен или применены ко всей сборке.

Затем пользовательская схема пространства имен может использоваться для разрешения типов в файлах XAML.

## <a name="consuming-a-custom-namespace-schema"></a>Использование пользовательской схемы пространства имен

Для использования типов из схемы пользовательского пространства имен компилятор XAML требует, чтобы в сборке, определяющей типы, была ссылка на код из сборки, использующей типы. Это можно сделать, добавив класс, содержащий метод `Init`, в сборку, определяющую типы, которые будут использоваться в XAML:

```csharp
namespace MyCompany.Controls
{
    public static class Controls
    {
        public static void Init()
        {
        }
    }
}
```

Затем можно вызвать метод `Init` из сборки, которая использует типы из схемы пользовательского пространства имен:

```csharp
using Xamarin.Forms;
using MyCompany.Controls;

namespace CustomNamespaceSchemaDemo
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            Controls.Init();
            InitializeComponent();
        }
    }
}
```

> [!WARNING]
> Если не включить такую ссылку на код, компилятору XAML не удастся разместить сборку, содержащую пользовательские типы схемы пространства имен.

Для использования элемента управления `CircleButton` объявляется пространство имен XAML с объявлением пространства имен, определяющим URL-адрес схемы пользовательского пространства имен:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:controls="http://mycompany.com/schemas/controls"
             x:Class="CustomNamespaceSchemaDemo.MainPage">
    <StackLayout Margin="20,35,20,20">
        ...
        <controls:CircleButton Text="+"
                               BackgroundColor="Fuchsia"
                               BorderColor="Black"
                               CircleDiameter="100" />
        <controls:CircleButton Text="-"
                               BackgroundColor="Teal"
                               BorderColor="Silver"
                               CircleDiameter="70" />
        ...
    </StackLayout>
</ContentPage>
```

`CircleButton` экземпляры можно добавить в [`ContentPage`](xref:Xamarin.Forms.ContentPage) , объявляя их с префиксом пространства имен `controls`.

Чтобы найти пользовательские типы схем пространства имен, Xamarin. Forms будет искать экземпляры `XmlnsDefinitionAttribute` в ссылках на сборки. Если атрибут `xmlns` элемента в файле XAML совпадает со значением свойства `XmlNamespace` в `XmlnsDefinitionAttribute`, Xamarin. Forms будет пытаться использовать значение свойства `XmlnsDefinitionAttribute.ClrNamespace` для разрешения типа. Если разрешение типа завершается неудачей, Xamarin. Forms продолжит выполнять разрешение типов на основе любого дополнительного соответствующего экземпляра `XmlnsDefinitionAttribute`.

В результате отображаются два экземпляра `CircleButton`:

![Круг кнопок](custom-namespace-schemas-images/circle-buttons.png "Круг кнопок")

## <a name="related-links"></a>Связанные ссылки

- [Схемы пользовательских пространств имен (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-customnamespaceschemas)
- [Рекомендуемые префиксы пространства имен XAML](custom-prefix.md)
- [Пространства имен XAML в Xamarin. Forms](namespaces.md)
