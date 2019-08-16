---
title: Основы ресурсов Android
ms.prod: xamarin
ms.assetid: ED32E7B5-D552-284B-6385-C3EDDCC30A4B
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/01/2018
ms.openlocfilehash: 1df10c4b8eaa30ce417feb2abae7f52b2494edf6
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2019
ms.locfileid: "69526366"
---
# <a name="android-resource-basics"></a>Основы ресурсов Android

Почти все приложения Android будут иметь некоторые ресурсы. как минимум они часто имеют макеты пользовательского интерфейса в виде XML-файлов. При первом создании приложения Xamarin. Android ресурсы по умолчанию устанавливаются с помощью шаблона проекта Xamarin. Android:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Файлы ресурсов](android-resource-basics-images/01-resource-files-vs.png)
 
# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio для Mac](#tab/macos)

![Файлы ресурсов](android-resource-basics-images/01-resource-files-xs.png)
 
-----

В папке resources были созданы пять файлов, составляющих ресурсы по умолчанию:

- **Icon. png** &ndash; значок по умолчанию для приложения

- **Main. axml** &ndash; файл макета пользовательского интерфейса по умолчанию для приложения. Обратите внимание, что хотя Android использует расширение **XML-** файла, Xamarin. Android использует расширение файла **. axml** .

- **Strings. XML** &ndash; таблица строк для упрощения локализации приложения

- **Абаутресаурцес. txt** &ndash; это необязательно и может быть безопасно удалено. Он просто предоставляет общий обзор папки Resources и ее файлов.

- Resource.Designer.CS&ndash; . Этот файл автоматически создается и обслуживается с помощью Xamarin. Android и содержит уникальный идентификатор, назначенный каждому ресурсу. Это очень похоже и идентично назначению файлу R. Java, который будет иметь приложение Android, написанное на Java. Оно автоматически создается средствами Xamarin. Android и будет повторно создано с течением времени.


## <a name="creating-and-accessing-resources"></a>Создание ресурсов и доступ к ним

Создание ресурсов — это просто добавление файлов в каталог для рассматриваемого типа ресурса. На снимке экрана ниже показаны строковые ресурсы для немецких языков, добавленных в проект. При добавлении **Strings. XML** в файл **действие сборки** автоматически задается **AndroidResource** инструментами Xamarin. Android:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Действие сборки для String. XML, установленное в AndroidResource](android-resource-basics-images/02-build-action-vs.png)
 
# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio для Mac](#tab/macos)

![Действие сборки для String. XML, установленное в AndroidResource](android-resource-basics-images/02-build-action-xs.png)
 
-----
 

Это позволяет средствам Xamarin. Android правильно компилировать и внедрять ресурсы в файл APK. Если по какой-либо причине для **действия сборки** не задано значение **ресурса Android**, файлы будут исключены из APK, а любая попытка загрузить или получить доступ к ресурсам приведет к ошибке во время выполнения и произойдет сбой приложения.

Кроме того, важно отметить, что хотя Android поддерживает только имена файлов нижнего регистра для элементов ресурсов, Xamarin. Android немного более терпим отношению; Он будет поддерживать имена файлов в верхнем и нижнем регистре. Для имен образов используется строчная буква с символами подчеркивания в качестве разделителей (например, **My\_Image\_Name. png**). Обратите внимание, что имена ресурсов не могут быть обработаны, если в качестве разделителей используются дефисы или пробелы.

После добавления ресурсов в проект существует два способа использования их в приложении &ndash; программным способом (внутри кода) или из XML-файлов.


## <a name="referencing-resources-programmatically"></a>Программное создание ссылок на ресурсы

Для программного доступа к этим файлам им назначается уникальный идентификатор ресурса. Этот идентификатор ресурса является целым числом, определенным в специальном `Resource`классе с именем, который находится в файле **Resource.Designer.CS**, и выглядит примерно так:

```csharp
public partial class Resource
{
    public partial class Attribute
    {
    }
    public partial class Drawable {
        public const int Icon=0x7f020000;
    }
    public partial class Id
    {
        public const int Textview=0x7f050000;
    }
    public partial class Layout
    {
        public const int Main=0x7f030000;
    }
    public partial class String
    {
        public const int App_Name=0x7f040001;
        public const int Hello=0x7f040000;
    }
}
```

Каждый идентификатор ресурса содержится внутри вложенного класса, соответствующего типу ресурса. Например, когда файл **Icon. png** был добавлен в проект, Xamarin. Android обновил `Resource` класс, создавая вложенный класс `Drawable` с константой внутри именованного `Icon`.
Это позволяет называть файл **Icon. png** в коде как `Resource.Drawable.Icon`. `Resource` Класс не следует изменять вручную, так как любые изменения, внесенные в него, будут перезаписаны Xamarin. Android.

При ссылке на ресурсы программным способом (в коде) доступ к ним можно получить с помощью иерархии классов Resources, которая использует следующий синтаксис:

```csharp
[<PackageName>.]Resource.<ResourceType>.<ResourceName>
```

- **PackageName** &ndash; Пакет, предоставляющий ресурс и необходимый только при использовании ресурсов из других пакетов.

- **ResourceType** &ndash; Это тип вложенного ресурса, который находится в классе ресурсов, описанном выше.

- **Имя ресурса** &ndash; это имя файла ресурса (без расширения) или значение атрибута Android: Name для ресурсов, которые находятся в элементе XML.


## <a name="referencing-resources-from-xml"></a>Ссылки на ресурсы из XML

Доступ к ресурсам в XML-файле осуществляется с помощью специального синтаксиса:

```xml
@[<PackageName>:]<ResourceType>/<ResourceName>
```

- **PackageName** &ndash; пакет, предоставляющий ресурс и необходимый только при использовании ресурсов из других пакетов.

- **ResourceType** &ndash; Это тип вложенного ресурса, который находится в классе ресурсов.

- **Имя ресурса** Это имя файла ресурса (*без* расширения типа файла) или значение `android:name` атрибута для ресурсов, которые находятся в элементе XML. &ndash;

Например, содержимое файла макета **Main. axml**имеет следующий вид:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:orientation="vertical"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent">
    <ImageView android:id="@+id/myImage"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:src="@drawable/flag" />
</LinearLayout>
```

В [`ImageView`](https://github.com/xamarin/recipes/tree/master/Recipes/android/controls/imageview) этом примере имеется, для которого требуется рисуемый ресурс с именем **Flag**. Атрибут`src` имеет значение`@drawable/flag`. `ImageView` Когда действие начинается, Android будет искать в ресурсе каталога **или** для файла с именем **Flag. png** (расширение файла может быть другим форматом изображения, например **Flag. jpg**) и загружать этот файл `ImageView`и отображать его в.
При запуске этого приложения он будет выглядеть примерно так, как на следующем рисунке:

![Локализованная Имажевиев](android-resource-basics-images/03-localized-screenshot.png)
