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
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/30/2019
ms.locfileid: "68644709"
---
# <a name="xaml-custom-namespace-schemas-in-xamarinforms"></a>Схемы пользовательских пространств имен XAML в Xamarin. Forms

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-customnamespaceschemas)

На типы в библиотеке можно ссылаться в XAML, объявляя пространство имен XAML для библиотеки, с помощью объявления namespace, указывающего имя пространства имен среды CLR и имя сборки:

```xaml
<ContentPage ...
             xmlns:controls="clr-namespace:MyCompany.Controls;assembly="MyCompany.Controls">
    ...
</ContentPage>
```

Однако указание пространства имен CLR и имени сборки в `xmlns` определении может быть непростой и подвержено ошибкам. Кроме того, может потребоваться несколько объявлений пространства имен XAML, если библиотека содержит типы в нескольких пространствах имен.

Альтернативный подход заключается в определении пользовательской схемы пространства имен, такой как `http://mycompany.com/schemas/controls`, которая сопоставляется с одним или несколькими пространствами имен среды CLR. Это позволяет использовать одно объявление пространства имен XAML для ссылки на все типы в сборке, даже если они находятся в разных пространствах имен. Он также позволяет использовать одно объявление пространства имен XAML для ссылки на типы в нескольких сборках.

Дополнительные сведения о пространствах имен XAML см. [в разделе пространства имен XAML в Xamarin. Forms](namespaces.md).

## <a name="defining-a-custom-namespace-schema"></a>Определение пользовательской схемы пространства имен

Пример приложения содержит библиотеку, предоставляющую некоторые простые элементы управления, такие `CircleButton`как:

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

Все элементы управления в библиотеке находятся в `MyCompany.Controls` пространстве имен. Эти элементы управления могут быть предоставлены вызывающей сборке через пользовательскую схему пространства имен.

Схема пользовательского пространства имен определяется `XmlnsDefinitionAttribute` классом, который задает сопоставление между пространством имен XAML и одним или несколькими пространствами имен CLR. `XmlnsDefinitionAttribute` Принимает два аргумента: имя пространства имен XAML и имя пространства имен CLR. Имя пространства имен XAML хранится в `XmlnsDefinitionAttribute.XmlNamespace` свойстве, а имя пространства имен CLR хранится `XmlnsDefinitionAttribute.ClrNamespace` в свойстве.

> [!NOTE]
> Класс также имеет свойство с именем `AssemblyName`, которое можно дополнительно задать для имени сборки. `XmlnsDefinitionAttribute` Это необходимо только в том случае, если пространство имен CLR `XmlnsDefinitionAttribute` , на которое ссылается, находится во внешней сборке.

`XmlnsDefinitionAttribute` Должен быть определен на уровне сборки в проекте, содержащем пространства имен CLR, которые будут сопоставлены в схеме пользовательского пространства имен. В следующем примере показан файл **AssemblyInfo.CS** из примера приложения:

```csharp
using Xamarin.Forms;
using MyCompany.Controls;

[assembly: Preserve]
[assembly: XmlnsDefinition("http://mycompany.com/schemas/controls", "MyCompany.Controls")]
```

Этот код создает пользовательскую схему пространства имен, которая `http://mycompany.com/schemas/controls` сопоставляет URL- `MyCompany.Controls` адрес с пространством имен CLR. Кроме того, `Preserve` атрибут задается для сборки, чтобы обеспечить сохранение компоновщиком всех типов в сборке.

> [!IMPORTANT]
> `Preserve` Атрибут следует применять к классам в сборке, которые сопоставляются с пользовательской схемой пространства имен или применены ко всей сборке.

Затем пользовательская схема пространства имен может использоваться для разрешения типов в файлах XAML.

## <a name="consuming-a-custom-namespace-schema"></a>Использование пользовательской схемы пространства имен

Для использования типов из схемы пользовательского пространства имен компилятор XAML требует, чтобы в сборке, определяющей типы, была ссылка на код из сборки, использующей типы. Это можно сделать, добавив в сборку класс, `Init` содержащий метод, определяющий типы, которые будут использоваться в XAML:

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

Затем `Init` метод можно вызвать из сборки, которая использует типы из схемы пользовательского пространства имен:

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

Для использования `CircleButton` элемента управления объявляется пространство имен XAML с объявлением пространства имен, которое указывает URL-адрес схемы пользовательского пространства имен:

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

`CircleButton`затем экземпляры можно добавить в, [`ContentPage`](xref:Xamarin.Forms.ContentPage) объявляя их `controls` с префиксом пространства имен.

Чтобы найти пользовательские типы схем пространства имен, Xamarin. Forms будет искать `XmlnsDefinitionAttribute` экземпляры, на которые имеются ссылки. Если атрибут элемента в файле XAML `XmlNamespace` соответствует значению свойства в `XmlnsDefinitionAttribute`, то `XmlnsDefinitionAttribute.ClrNamespace` Xamarin. Forms попытается использовать значение свойства для разрешения типа. `xmlns` Если разрешение типа завершается неудачей, Xamarin. Forms продолжит выполнять разрешение типов на основе `XmlnsDefinitionAttribute` любых дополнительных соответствующих экземпляров.

В результате отображаются два `CircleButton` экземпляра:

![Круг кнопок](custom-namespace-schemas-images/circle-buttons.png "Круг кнопок")

## <a name="related-links"></a>Связанные ссылки

- [Схемы пользовательских пространств имен (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-customnamespaceschemas)
- [Рекомендуемые префиксы пространства имен XAML](custom-prefix.md)
- [Пространства имен XAML в Xamarin. Forms](namespaces.md)
