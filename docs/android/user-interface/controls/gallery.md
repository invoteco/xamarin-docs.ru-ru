---
title: Элемент управления коллекции Android
ms.prod: xamarin
ms.assetid: 3112E68A-7853-B147-90A6-6295CA2C4CB5
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/15/2018
ms.openlocfilehash: 93eb7f98da6f3fe06f288eae5823f7173e58585f
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029244"
---
# <a name="xamarinandroid-gallery-control"></a>Элемент управления коллекции Xamarin. Android

[`Gallery`](xref:Android.Widget.Gallery) — это мини-приложение макета, используемое для отображения элементов в списке с горизонтальной прокруткой и помещает текущее выделение в центре представления.

> [!IMPORTANT]
> Это мини-приложение было признано устаревшим в Android 4,1 (API уровня 16). 

В этом руководстве вы создадите галерею фотографий, а затем выводите всплывающее сообщение при каждом выборе элемента коллекции.

После того как для представления содержимого будет задан макет `Main.axml`, `Gallery` захватывается в макете с помощью [`FindViewById`](xref:Android.App.Activity.FindViewById*).
[`Adapter`](xref:Android.Widget.AdapterView.RawAdapter)
Затем свойство используется для задания пользовательского адаптера (`ImageAdapter`) в качестве источника для всех элементов, отображаемых в даллери. На следующем шаге создается `ImageAdapter`.

Чтобы сделать что-то при щелчке элемента в коллекции, на [`ItemClick`](xref:Android.Widget.AdapterView.ItemClick) будет подписан анонимный делегат.
. В нем отображается [`Toast`](xref:Android.Widget.Toast)
, отображающий позицию индекса (с отсчетом от нуля) элемента сеселектед (в реальном сценарии это положение можно использовать для получения изображения полного размера для некоторой другой задачи).

Во-первых, существует несколько переменных-членов, включая массив идентификаторов, которые ссылаются на изображения, сохраненные в каталоге нарисованных ресурсов (**ресурсы и нарисованные**).

Далее следует конструктор класса, где [`Context`](xref:Android.Content.Context)
для экземпляра `ImageAdapter` определяется и сохраняется в локальном поле.
Далее реализуются некоторые обязательные методы, унаследованные от [`BaseAdapter`](xref:Android.Widget.BaseAdapter).
Конструктор и [`Count`](xref:Android.Widget.BaseAdapter.Count)
свойство является самим пояснением. Обычно [`GetItem(int)`](xref:Android.Widget.BaseAdapter.GetItem*)
должен возвращать фактический объект в указанной позиции в адаптере, но он игнорируется в этом примере. Аналогичным образом [`GetItemId(int)`](xref:Android.Widget.BaseAdapter.GetItemId*)
должен возвращать идентификатор строки элемента, но здесь это не требуется.

Метод выполняет работу по применению изображения к [`ImageView`](xref:Android.Widget.ImageView)
который будет внедрен в [`Gallery`](xref:Android.Widget.Gallery)
В этом методе член [`Context`](xref:Android.Content.Context)
используется для создания нового [`ImageView`](xref:Android.Widget.ImageView).
[`ImageView`](xref:Android.Widget.ImageView)
подготавливается путем применения изображения из локального массива рисуемых ресурсов с указанием [`Gallery.LayoutParams`](xref:Android.Widget.Gallery.LayoutParams)
Высота и ширина изображения, Установка масштаба в соответствии с [`ImageView`](xref:Android.Widget.ImageView)
и, наконец, задавая для фона атрибут, полученный в конструкторе.

Дополнительные параметры масштабирования изображения см. в разделе [`ImageView.ScaleType`](xref:Android.Widget.ImageView.ScaleType) .

## <a name="walkthrough"></a>Пошаговое руководство

Запустите новый проект с именем *хеллогаллери*.

[![снимок экрана нового проекта Android в диалоговом окне "Создание решения"](gallery-images/hellogallery1-sml.png)](gallery-images/hellogallery1.png#lightbox)

Найдите фотографии, которые вы хотели бы использовать, или [Скачайте эти образцы изображений](https://developer.android.com/shareables/sample_images.zip).
Добавьте файлы изображений в каталог **ресурсов** проекта. В окне **Свойства** задайте для параметра Действие сборки значение **AndroidResource**.

Откройте **ресурсы/макет/Main. axml** и вставьте следующее:

```xml
<?xml version="1.0" encoding="utf-8"?>
<Gallery xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/gallery"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
/>
```

Откройте `MainActivity.cs` и вставьте следующий код для [`OnCreate()`](xref:Android.App.Activity.OnCreate*)
Method

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    // Set our view from the "main" layout resource
    SetContentView (Resource.Layout.Main);

    Gallery gallery = (Gallery) FindViewById<Gallery>(Resource.Id.gallery);

    gallery.Adapter = new ImageAdapter (this);

    gallery.ItemClick += delegate (object sender, Android.Widget.AdapterView.ItemClickEventArgs args) {
        Toast.MakeText (this, args.Position.ToString (), ToastLength.Short).Show ();
    };
}
```

Создайте новый класс с именем `ImageAdapter`, который подклассировать [`BaseAdapter`](xref:Android.Widget.BaseAdapter):

```csharp
public class ImageAdapter : BaseAdapter
{
    Context context;

    public ImageAdapter (Context c)
    {
          context = c;
    }

    public override int Count { get { return thumbIds.Length; } }

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
          ImageView i = new ImageView (context);

          i.SetImageResource (thumbIds[position]);
          i.LayoutParameters = new Gallery.LayoutParams (150, 100);
          i.SetScaleType (ImageView.ScaleType.FitXy);

          return i;
    }

    // references to our images
    int[] thumbIds = {
            Resource.Drawable.sample_1,
            Resource.Drawable.sample_2,
            Resource.Drawable.sample_3,
            Resource.Drawable.sample_4,
            Resource.Drawable.sample_5,
            Resource.Drawable.sample_6,
            Resource.Drawable.sample_7
     };
}

```

Запустите приложение. Он должен выглядеть, как показано на снимке экрана ниже:

![Снимок экрана Хеллогаллери, отображающий образцы изображений](gallery-images/hellogallery3.png)

## <a name="references"></a>Ссылки

- [`BaseAdapter`](xref:Android.Widget.BaseAdapter)
- [`Gallery`](xref:Android.Widget.Gallery)
- [`ImageView`](xref:Android.Widget.ImageView)

_Части этой страницы являются изменениями на основе работы, созданной и совместно используемой проектом Android с открытым кодом, и используются в соответствии с терминами, описанными в [лицензии Creative commons attribution 2,5](https://creativecommons.org/licenses/by/2.5/)._
