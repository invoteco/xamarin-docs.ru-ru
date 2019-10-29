---
title: Пример базового RecyclerView
description: Пример приложения, демонстрирующий использование RecyclerView.
ms.prod: xamarin
ms.assetid: A50520D2-1214-40E1-9B27-B0891FE11584
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 07/30/2018
ms.openlocfilehash: 7266d13136dbfc858bc3c882cacb802d83c92f52
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028843"
---
# <a name="a-basic-recyclerview-example"></a>Пример базового RecyclerView

Чтобы понять, как работает `RecyclerView` в типичном приложении, в этом разделе рассматривается пример приложения [рециклервиевер](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-recyclerviewer) , простой пример кода, который использует `RecyclerView` для отображения большой коллекции фотографий: 

[![двух снимков экрана приложения RecyclerView, использующего Кардвиевс для показа фотографий](recyclerview-example-images/01-recyclerviewer-sml.png)](recyclerview-example-images/01-recyclerviewer.png#lightbox)

**Рециклервиевер** использует [кардвиев](~/android/user-interface/controls/card-view.md) для реализации каждого элемента фотографии в макете `RecyclerView`. Из-за преимуществ производительности `RecyclerView`этот пример приложения может быстро прокручивать большую коллекцию фотографий без заметных задержек.

### <a name="an-example-data-source"></a>Пример источника данных

В этом примере приложения источник данных "Фотоальбом" (представленный классом `PhotoAlbum`) предоставляет `RecyclerView` с содержимым элемента.
`PhotoAlbum` — это коллекция фотографий с заголовками; При создании экземпляра вы получаете готовый набор из 32 фотографий:

```csharp
PhotoAlbum mPhotoAlbum = new PhotoAlbum ();
```

Каждый экземпляр Photo в `PhotoAlbum` предоставляет свойства, позволяющие считывать идентификатор ресурса изображения, `PhotoID`и строку заголовка `Caption`. Коллекция фотографий организована таким же путем, что индексатор может получить доступ к каждой фотографии. Например, следующие строки кода обращаются к ИДЕНТИФИКАТОРу ресурса изображения и заголовку для десятой фотографии в коллекции:

```csharp
int imageId = mPhotoAlbum[9].ImageId;
string caption = mPhotoAlbum[9].Caption;
```

`PhotoAlbum` также предоставляет метод `RandomSwap`, который можно вызвать для переключения первого фото в коллекции с помощью случайного выбора фотографии в другой части коллекции:

```csharp
mPhotoAlbum.RandomSwap ();
```

Поскольку сведения о реализации `PhotoAlbum` не важны для понимания `RecyclerView`, исходный код `PhotoAlbum` не представлен здесь. Исходный код для `PhotoAlbum` доступен по адресу [PhotoAlbum.CS](https://github.com/xamarin/monodroid-samples/blob/master/android5.0/RecyclerViewer/RecyclerViewer/PhotoAlbum.cs) в примере приложения [рециклервиевер](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-recyclerviewer) .

### <a name="layout-and-initialization"></a>Макет и инициализация

Файл макета **Main. axml**состоит из одного `RecyclerView` в `LinearLayout`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent">
    <android.support.v7.widget.RecyclerView
        android:id="@+id/recyclerView"
        android:scrollbars="vertical"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent" />
</LinearLayout>
```

Обратите внимание, что необходимо использовать полное имя **Android. support. версии 7. widget. RecyclerView** , так как `RecyclerView` упаковывается в библиотеку поддержки. Метод `OnCreate` `MainActivity` Инициализирует этот макет, создает экземпляр адаптера и подготавливает базовый источник данных:

```csharp
public class MainActivity : Activity
{
    RecyclerView mRecyclerView;
    RecyclerView.LayoutManager mLayoutManager;
    PhotoAlbumAdapter mAdapter;
    PhotoAlbum mPhotoAlbum;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        // Prepare the data source:
        mPhotoAlbum = new PhotoAlbum ();

        // Instantiate the adapter and pass in its data source:
        mAdapter = new PhotoAlbumAdapter (mPhotoAlbum);

        // Set our view from the "main" layout resource:
        SetContentView (Resource.Layout.Main);

        // Get our RecyclerView layout:
        mRecyclerView = FindViewById<RecyclerView> (Resource.Id.recyclerView);

        // Plug the adapter into the RecyclerView:
        mRecyclerView.SetAdapter (mAdapter);
```

Этот код выполняет следующие действия:

1. Создает экземпляр источника данных `PhotoAlbum`.

2. Передает источник данных фотоальбома в конструктор адаптера, `PhotoAlbumAdapter` (который определяется далее в этом разделе). 
   Обратите внимание, что рекомендуется передать источник данных в качестве параметра в конструктор адаптера. 

3. Возвращает `RecyclerView` из макета.

4. Подключает адаптер к экземпляру `RecyclerView`, вызвав метод `RecyclerView` `SetAdapter`, как показано выше.

### <a name="layout-manager"></a>Диспетчер макетов

Каждый элемент в `RecyclerView` состоит из `CardView`, который содержит фото-изображение и подпись фотографии (подробные сведения см. в разделе « [держатель»](#view-holder) ниже). Предопределенный `LinearLayoutManager` используется для размещения каждого `CardView` в вертикальной прокрутке:

```csharp
mLayoutManager = new LinearLayoutManager (this);
mRecyclerView.SetLayoutManager (mLayoutManager);
```

Этот код находится в методе `OnCreate` основного действия. Конструктору диспетчера макетов требуется *контекст*, поэтому `MainActivity` передается с помощью `this` как показано выше.

Вместо использования предопределенной `LinearLayoutManager`можно подключить пользовательский диспетчер макетов, отображающий два элемента `CardView` рядом друг с другом, реализуя эффект анимации для переворачивания страниц для прохода по коллекции фотографий. Далее в этом пошаговом окне вы увидите пример изменения макета, переключаясь в другой диспетчер макетов.

<a name="view-holder" />

### <a name="view-holder"></a>Просмотреть держатель

Класс-держатель представления называется `PhotoViewHolder`. Каждый экземпляр `PhotoViewHolder` содержит ссылки на `ImageView` и `TextView` связанного элемента строки, который располагается в `CardView` как пояснены здесь:

[![схема Кардвиев, содержащая Имажевиев и TextView](recyclerview-example-images/02-cardview-layout-sml.png)](recyclerview-example-images/02-cardview-layout.png#lightbox)

`PhotoViewHolder` является производным от `RecyclerView.ViewHolder` и содержит свойства для хранения ссылок на `ImageView` и `TextView`, показанные в приведенном выше макете.
`PhotoViewHolder` состоит из двух свойств и одного конструктора:

```csharp
public class PhotoViewHolder : RecyclerView.ViewHolder
{
    public ImageView Image { get; private set; }
    public TextView Caption { get; private set; }

    public PhotoViewHolder (View itemView) : base (itemView)
    {
        // Locate and cache view references:
        Image = itemView.FindViewById<ImageView> (Resource.Id.imageView);
        Caption = itemView.FindViewById<TextView> (Resource.Id.textView);
    }
}
```

В этом примере кода конструктору `PhotoViewHolder` передается ссылка на представление родительского элемента (`CardView`), которое `PhotoViewHolder` переносится в оболочку. Обратите внимание, что вы всегда пересылаете представление родительского элемента в базовый конструктор. Конструктор `PhotoViewHolder` вызывает `FindViewById` в представлении родительского элемента для поиска всех ссылок на дочерний вид, `ImageView` и `TextView`, сохраняя результаты в свойствах `Image` и `Caption` соответственно. Адаптер позже извлекает ссылки на представления из этих свойств при обновлении дочерних представлений этого `CardView`с новыми данными.

Дополнительные сведения о `RecyclerView.ViewHolder`см. в [справочнике по классам RecyclerView. виевхолдер](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ViewHolder.html).

### <a name="adapter"></a>Адаптер

Адаптер загружает каждую строку `RecyclerView` с данными для конкретной фотографии. Например, для данной фотографии в позиции строки *p*адаптер находит связанные данные в позиции *p* в источнике данных и копирует эти данные в элемент строки в позиции *p* в коллекции `RecyclerView`. Адаптер использует держатель представления для поиска ссылок на `ImageView` и `TextView` в этой позиции, поэтому ему не нужно многократно вызывать `FindViewById` для этих представлений, так как пользователь прокручивается по коллекции фотографий и повторно использует представления.

В **рециклервиевер**класс адаптера является производным от `RecyclerView.Adapter` для создания `PhotoAlbumAdapter`:

```csharp
public class PhotoAlbumAdapter : RecyclerView.Adapter
{
    public PhotoAlbum mPhotoAlbum;

    public PhotoAlbumAdapter (PhotoAlbum photoAlbum)
    {
        mPhotoAlbum = photoAlbum;
    }
    ...
}
```

Элемент `mPhotoAlbum` содержит источник данных (фотоальбом), который передается в конструктор; конструктор копирует фотоальбом в эту переменную члена. Реализуются следующие обязательные `RecyclerView.Adapter` методы:

- **`OnCreateViewHolder`** &ndash; создает экземпляр файла макета элемента и владельца представления.

- **`OnBindViewHolder`** &ndash; загружает данные в указанной позиции в представления, ссылки на которые хранятся в заданном заполнителье представления.

- **`ItemCount`** &ndash; возвращает число элементов в источнике данных.

Диспетчер макетов вызывает эти методы при размещении элементов в `RecyclerView`. Реализация этих методов рассматривается в следующих разделах.

#### <a name="oncreateviewholder"></a>онкреатевиевхолдер

Диспетчер макетов вызывает `OnCreateViewHolder`, когда `RecyclerView`у требуется новый владелец представления для представления элемента. `OnCreateViewHolder` увеличивает представление элемента из файла макета представления и заключает его в новый экземпляр `PhotoViewHolder`. Конструктор `PhotoViewHolder` находит и сохраняет ссылки на дочерние представления в макете, как описано ранее в поле [Владелец представления](#view-holder).

Каждый элемент строки представлен `CardView`, который содержит `ImageView` (для фотографии) и `TextView` (для заголовка). Этот макет находится в файле **фотокардвиев. axml**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:card_view="http://schemas.android.com/apk/res-auto"
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content">
    <android.support.v7.widget.CardView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        card_view:cardElevation="4dp"
        card_view:cardUseCompatPadding="true"
        card_view:cardCornerRadius="5dp">
        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:orientation="vertical"
            android:padding="8dp">
            <ImageView
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:id="@+id/imageView"
                android:scaleType="centerCrop" />
            <TextView
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:textAppearance="?android:attr/textAppearanceMedium"
                android:textColor="#333333"
                android:text="Caption"
                android:id="@+id/textView"
                android:layout_gravity="center_horizontal"
                android:layout_marginLeft="4dp" />
        </LinearLayout>
    </android.support.v7.widget.CardView>
</FrameLayout>
```

Этот макет представляет один элемент строки в `RecyclerView`. Метод `OnBindViewHolder` (описанный ниже) копирует данные из источника данных в `ImageView` и `TextView` этого макета.
`OnCreateViewHolder` расширяет этот макет для заданного расположения фотографий в `RecyclerView` и создает новый экземпляр `PhotoViewHolder` (который находит и кэширует ссылки на `ImageView` и `TextView` дочерних представлений в связанном макете `CardView`). :

```csharp
public override RecyclerView.ViewHolder
    OnCreateViewHolder (ViewGroup parent, int viewType)
{
    // Inflate the CardView for the photo:
    View itemView = LayoutInflater.From (parent.Context).
                Inflate (Resource.Layout.PhotoCardView, parent, false);

    // Create a ViewHolder to hold view references inside the CardView:
    PhotoViewHolder vh = new PhotoViewHolder (itemView);
    return vh;
}

```

Полученный экземпляр заполнителя представления, `vh`, возвращается к вызывающему объекту (диспетчеру макетов).

#### <a name="onbindviewholder"></a>онбиндвиевхолдер

Когда Диспетчер макетов готов к отображению определенного представления в видимой области экрана `RecyclerView`, он вызывает метод `OnBindViewHolder` адаптера для заполнения элемента в указанной позиции строки содержимым из источника данных. `OnBindViewHolder` получает сведения о фотографии для указанной позицией строки (ресурса изображения фотографии и строки для подписи фотографии) и копирует эти данные в связанные представления. Представления размещаются через ссылки, хранящиеся в объекте-владельцу представления (который передается с помощью параметра `holder`):

```csharp
public override void
    OnBindViewHolder (RecyclerView.ViewHolder holder, int position)
{
    PhotoViewHolder vh = holder as PhotoViewHolder;

    // Load the photo image resource from the photo album:
    vh.Image.SetImageResource (mPhotoAlbum[position].PhotoID);

    // Load the photo caption from the photo album:
    vh.Caption.Text = mPhotoAlbum[position].Caption;
}
```

Переданный объект-владелец представления сначала должен быть приведен к типу заполнителя производного представления (в данном случае `PhotoViewHolder`) перед использованием.
Адаптер загружает ресурс изображения в представление, на которое ссылается свойство `Image` владельца представления, и копирует текст заголовка в представление, на которое ссылается свойство `Caption` владельца представления. Это *привязывает* связанное представление к его данным.

Обратите внимание, что `OnBindViewHolder` — это код, который напрямую работает со структурой данных. В этом случае `OnBindViewHolder` распознает, как сопоставить позицию `RecyclerView` элемента со связанным элементом данных в источнике данных. В этом случае сопоставление является простым, так как позиция может использоваться в качестве индекса массива в фотоальбоме. Однако более сложные источники данных могут потребовать дополнительного кода для установления такого сопоставления.

#### <a name="itemcount"></a>ItemCount

Метод `ItemCount` возвращает количество элементов в коллекции данных. В примере приложения Photo Viewer число элементов равно числу фотографий в фотоальбоме:

```csharp
public override int ItemCount
{
    get { return mPhotoAlbum.NumPhotos; }
}
```

Дополнительные сведения о `RecyclerView.Adapter`см. в [справочнике по классам RecyclerView. Adapter](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.Adapter.html).

### <a name="putting-it-all-together"></a>Совместное размещение

Итоговая `RecyclerView` реализация в примере Photo App состоит из `MainActivity` кода, который создает источник данных, Диспетчер макетов и адаптер. в `MainActivity` создается экземпляр `mRecyclerView`, создается экземпляр источника данных и адаптер и подключается к диспетчеру макета и адаптеру:

```csharp
public class MainActivity : Activity
{
    RecyclerView mRecyclerView;
    RecyclerView.LayoutManager mLayoutManager;
    PhotoAlbumAdapter mAdapter;
    PhotoAlbum mPhotoAlbum;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);
        mPhotoAlbum = new PhotoAlbum();
        SetContentView (Resource.Layout.Main);
        mRecyclerView = FindViewById<RecyclerView> (Resource.Id.recyclerView);

        // Plug in the linear layout manager:
        mLayoutManager = new LinearLayoutManager (this);
        mRecyclerView.SetLayoutManager (mLayoutManager);

        // Plug in my adapter:
        mAdapter = new PhotoAlbumAdapter (mPhotoAlbum);
        mRecyclerView.SetAdapter (mAdapter);
    }
}

```

`PhotoViewHolder` находит и кэширует ссылки на представление:

```csharp
public class PhotoViewHolder : RecyclerView.ViewHolder
{
    public ImageView Image { get; private set; }
    public TextView Caption { get; private set; }

    public PhotoViewHolder (View itemView) : base (itemView)
    {
        // Locate and cache view references:
        Image = itemView.FindViewById<ImageView> (Resource.Id.imageView);
        Caption = itemView.FindViewById<TextView> (Resource.Id.textView);
    }
}
```

`PhotoAlbumAdapter` реализует три переопределенных метода.

```csharp
public class PhotoAlbumAdapter : RecyclerView.Adapter
{
    public PhotoAlbum mPhotoAlbum;
    public PhotoAlbumAdapter (PhotoAlbum photoAlbum)
    {
        mPhotoAlbum = photoAlbum;
    }

    public override RecyclerView.ViewHolder
        OnCreateViewHolder (ViewGroup parent, int viewType)
    {
        View itemView = LayoutInflater.From (parent.Context).
                    Inflate (Resource.Layout.PhotoCardView, parent, false);
        PhotoViewHolder vh = new PhotoViewHolder (itemView);
        return vh;
    }

    public override void
        OnBindViewHolder (RecyclerView.ViewHolder holder, int position)
    {
        PhotoViewHolder vh = holder as PhotoViewHolder;
        vh.Image.SetImageResource (mPhotoAlbum[position].PhotoID);
        vh.Caption.Text = mPhotoAlbum[position].Caption;
    }

    public override int ItemCount
    {
        get { return mPhotoAlbum.NumPhotos; }
    }
}
```

При компиляции и выполнении этого кода создается основное приложение для просмотра фотографий, как показано на следующих снимках экрана:

[![двух снимков экрана приложения для просмотра фотографий с вертикальной прокруткой карт фото](recyclerview-example-images/03-recyclerviewer-basic-sml.png)](recyclerview-example-images/03-recyclerviewer-basic.png#lightbox)

Если тени не рисуются (как показано на снимке экрана выше), измените **Свойства/AndroidManifest. XML** и добавьте следующий параметр атрибута в элемент `<application>`:

```xml
android:hardwareAccelerated="true"
```

Это базовое приложение поддерживает только обзор фотоальбома. Он не реагирует на события касания элемента и не обрабатывает изменения в базовых данных. Эта функция добавлена в [примере расширения RecyclerView](~/android/user-interface/layouts/recycler-view/extending-the-example.md).

### <a name="changing-the-layoutmanager"></a>Изменение Лайаутманажер

Из-за гибкости `RecyclerView`можно легко изменить приложение, чтобы использовать другой диспетчер макетов. В следующем примере он изменяется для отображения фотоальбома с макетом сетки, который прокручивается горизонтально, а не с вертикальным линейным макетом. Для этого создание экземпляра диспетчера макетов изменяется для использования `GridLayoutManager` следующим образом.

```csharp
mLayoutManager = new GridLayoutManager(this, 2, GridLayoutManager.Horizontal, false);
```

Это изменение кода заменяет вертикальный `LinearLayoutManager` на `GridLayoutManager`, представляющий сетку, которая состоит из двух строк, прокрутхся по горизонтали. При повторной компиляции и запуске приложения вы увидите, что фотографии отображаются в сетке, а прокрутка — горизонтальная, а не вертикальная:

[![Пример снимка экрана приложения с горизонтальной прокруткой фотографий в сетке](recyclerview-example-images/04-gridlayoutmanager-sml.png)](recyclerview-example-images/04-gridlayoutmanager.png#lightbox)

Изменяя только одну строку кода, можно изменить приложение для просмотра фотографий, чтобы использовать другой макет с другим поведением.
Обратите внимание, что ни код адаптера, ни XML макета не были изменены для изменения стиля макета. 

В следующем разделе, посвященном расширению [примера RecyclerView](~/android/user-interface/layouts/recycler-view/extending-the-example.md), этот базовый пример приложения расширяется для обработки событий щелчка элемента и обновления `RecyclerView` при изменении базового источника данных.

## <a name="related-links"></a>Связанные ссылки

- [Рециклервиевер (пример)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-recyclerviewer)
- [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)
- [RecyclerView части и функциональные возможности](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md)
- [Расширение примера RecyclerView](~/android/user-interface/layouts/recycler-view/extending-the-example.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
