---
title: GridView
ms.prod: xamarin
ms.assetid: 6992C4FF-ECBB-3493-AEE6-3E063C1A8C54
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: a1bcb83d6057cb7d4a43c510d7b5805b574812e6
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/26/2019
ms.locfileid: "68510061"
---
# <a name="xamarinandroid-gridview"></a>Элемент управления GridView. Android

[`GridView`](xref:Android.Widget.GridView)является[`ViewGroup`](xref:Android.Views.ViewGroup)
, отображающий элементы в двумерной, прокручиваемой сетке. Элементы сетки автоматически вставляются в макет с помощью [`ListAdapter`](xref:Android.App.ListActivity.ListAdapter).

В этом учебнике вы создадите сетку эскизов изображений. При выборе элемента всплывающее сообщение будет отображать позицию изображения.

Запустите новый проект с именем **хеллогридвиев**.

Найдите фотографии, которые вы хотели бы использовать, или [Скачайте эти образцы изображений](https://developer.android.com/shareables/sample_images.zip). Добавьте файлы изображений в каталог **ресурсов** проекта. В окне **Свойства** задайте для параметра Действие сборки значение **AndroidResource**.

Откройте файл **Resources/Layout/Main. axml** и вставьте в него следующее:

```xml
<?xml version="1.0" encoding="utf-8"?>
<GridView xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/gridview"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:columnWidth="90dp"
    android:numColumns="auto_fit"
    android:verticalSpacing="10dp"
    android:horizontalSpacing="10dp"
    android:stretchMode="columnWidth"
    android:gravity="center"
/>
```

Это [`GridView`](xref:Android.Widget.GridView) приведет к заполнению всего экрана. Атрибуты, скорее всего, говорят сами за себя. Дополнительные сведения о допустимых атрибутах см. [`GridView`](xref:Android.Widget.GridView) в справочнике.

Откройте `HelloGridView.cs` и вставьте следующий код для[`OnCreate()`](xref:Android.App.Activity.OnCreate*)
Method

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    SetContentView (Resource.Layout.Main);

    var gridview = FindViewById<GridView> (Resource.Id.gridview);
    gridview.Adapter = new ImageAdapter (this);

    gridview.ItemClick += delegate (object sender, AdapterView.ItemClickEventArgs args) {
        Toast.MakeText (this, args.Position.ToString (), ToastLength.Short).Show ();
    };
}
```

После того как для представления содержимого будет задан макет **Main. axml** , объект [`GridView`](xref:Android.Widget.GridView) будет захвачен из макета с помощью [`FindViewById`](xref:Android.App.Activity.FindViewById*). Тот[`Adapter`](xref:Android.Widget.AdapterView.RawAdapter)
Затем свойство используется для задания пользовательского адаптера (`ImageAdapter`) в качестве источника для всех элементов, отображаемых в сетке. Объект `ImageAdapter` создается на следующем шаге.

Чтобы сделать что-то при щелчке элемента в сетке, на [`ItemClick`](xref:Android.Widget.AdapterView.ItemClick) событие подписывается анонимный делегат.
Он показывает [`Toast`](xref:Android.Widget.Toast) , что показывает позицию индекса (от нуля) выбранного элемента (в реальном сценарии это положение можно использовать для получения изображения полного размера для некоторой другой задачи). Обратите внимание, что вместо событий .NET можно использовать классы прослушивателей в стиле Java.

Создайте новый класс с именем `ImageAdapter` , [`BaseAdapter`](xref:Android.Widget.BaseAdapter)подклассом:

```csharp
public class ImageAdapter : BaseAdapter
{
    Context context;

    public ImageAdapter (Context c)
    {
        context = c;
    }

    public override int Count {
        get { return thumbIds.Length; }
    }

    public override Java.Lang.Object GetItem (int position)
    {
        return null;
    }

    public override long GetItemId (int position)
    {
        return 0;
    }

    // create a new ImageView for each item referenced by the Adapter
    public override View GetView (int position, View convertView, ViewGroup parent)
    {
        ImageView imageView;

        if (convertView == null) {  // if it's not recycled, initialize some attributes
            imageView = new ImageView (context);
            imageView.LayoutParameters = new GridView.LayoutParams (85, 85);
            imageView.SetScaleType (ImageView.ScaleType.CenterCrop);
            imageView.SetPadding (8, 8, 8, 8);
        } else {
            imageView = (ImageView)convertView;
        }

        imageView.SetImageResource (thumbIds[position]);
        return imageView;
    }

    // references to our images
    int[] thumbIds = {
        Resource.Drawable.sample_2, Resource.Drawable.sample_3,
        Resource.Drawable.sample_4, Resource.Drawable.sample_5,
        Resource.Drawable.sample_6, Resource.Drawable.sample_7,
        Resource.Drawable.sample_0, Resource.Drawable.sample_1,
        Resource.Drawable.sample_2, Resource.Drawable.sample_3,
        Resource.Drawable.sample_4, Resource.Drawable.sample_5,
        Resource.Drawable.sample_6, Resource.Drawable.sample_7,
        Resource.Drawable.sample_0, Resource.Drawable.sample_1,
        Resource.Drawable.sample_2, Resource.Drawable.sample_3,
        Resource.Drawable.sample_4, Resource.Drawable.sample_5,
        Resource.Drawable.sample_6, Resource.Drawable.sample_7
    };
}
```

Во первых, он реализует некоторые обязательные методы [`BaseAdapter`](xref:Android.Widget.BaseAdapter), наследуемые от. Конструктор и [`Count`](xref:Android.Widget.BaseAdapter.Count) свойство являются самими пояснениями. Возмож[`GetItem(int)`](xref:Android.Widget.BaseAdapter.GetItem*)
должен возвращать фактический объект в указанной позиции в адаптере, но он игнорируется в этом примере. Подоб[`GetItemId(int)`](xref:Android.Widget.BaseAdapter.GetItemId*)
должен возвращать идентификатор строки элемента, но здесь это не требуется.

Первый из необходимых методов — [`GetView()`](xref:Android.Widget.BaseAdapter.GetView*).
Этот метод создает новый элемент[`View`](xref:Android.Views.View)
для каждого изображения, `ImageAdapter`добавленного в. При вызове метода[`View`](xref:Android.Views.View)
передается в, который обычно является перезапущенным объектом (по крайней мере после этого вызывался один раз), поэтому проверяется, имеет ли объект значение null. Если значение *равно* null,[`ImageView`](xref:Android.Widget.ImageView)
создается и настраивается с помощью требуемых свойств для представления изображения:

- [`LayoutParams`](xref:Android.Views.View.LayoutParameters)Задает высоту и ширину для представления&mdash;. Это гарантирует, что независимо от размера рисунка размер изображения изменяется и обрезается в соответствии с соответствующими размерами.

- [`SetScaleType()`](xref:Android.Widget.ImageView.SetScaleType*)объявляет, что изображения следует обрезать в центре (при необходимости).

- [`SetPadding(int, int, int, int)`](xref:Android.Views.View.SetPadding*)Определяет заполнение для всех сторон. (Обратите внимание, что если изображения имеют разные пропорции, то меньшее заполнение приведет к более обрезке изображения, если оно не соответствует измерениям, заданным для Имажевиев.)

[`View`](xref:Android.Views.View) Если  переданное [значениенеравноnull,`GetView()`](xref:Android.Widget.BaseAdapter.GetView*) то локальный[`ImageView`](xref:Android.Widget.ImageView)
инициализируется с помощью перезапущенного [`View`](xref:Android.Views.View) объекта.

В конце[`GetView()`](xref:Android.Widget.BaseAdapter.GetView*)
, `position` целое число, передаваемое в метод, используется для выбора изображения `thumbIds` из массива, установленного в качестве ресурса изображения для [`ImageView`](xref:Android.Widget.ImageView).

Все, что осталось, — это определить `thumbIds` массив рисуемых ресурсов.

Запустите приложение. Макет сетки должен выглядеть примерно так:

[![Пример снимка экрана GridView, отображающий 15 изображений](grid-view-images/helloviews4.png)](grid-view-images/helloviews4.png#lightbox)

Попробуйте поэкспериментировать с поведением [`GridView`](xref:Android.Widget.GridView) и[`ImageView`](xref:Android.Widget.ImageView)
путем настройки их свойств. Например, вместо использования функции [`LayoutParams`](xref:Android.Views.View.LayoutParameters) try используйте. [`SetAdjustViewBounds()`](xref:Android.Widget.ImageView.SetAdjustViewBounds*)

## <a name="references"></a>Ссылки

- [`GridView`](xref:Android.Widget.GridView)
- [`ImageView`](xref:Android.Widget.ImageView)
- [`BaseAdapter`](xref:Android.Widget.BaseAdapter)

*Части этой страницы являются изменениями на основе работы, созданной и совместно используемой проектом Android с открытым кодом, и используются в соответствии с терминами,* 
описанными в[*лицензии Creative Commons Attribution 2,5*](http://creativecommons.org/licenses/by/2.5/).
