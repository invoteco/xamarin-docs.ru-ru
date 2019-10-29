---
title: Расширение примера RecyclerView
description: Добавление обработчиков событий Click в приложение RecyclerView example.
ms.prod: xamarin
ms.assetid: 707EE1CE-C164-485B-944C-82C6795E8A24
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 07/13/2018
ms.openlocfilehash: d50971c1ef0064463e576a895729ad84577e1788
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028871"
---
# <a name="extending-the-recyclerview-example"></a>Расширение примера RecyclerView

Основное приложение, описанное в [базовом RecyclerView примере,](~/android/user-interface/layouts/recycler-view/recyclerview-example.md) на самом деле не требует много &ndash; просто прокручивает и отображает фиксированный список элементов фотографий для упрощения просмотра. В реальных приложениях пользователи хотят иметь возможность взаимодействовать с приложением, коснувшись элементов на экране. Кроме того, базовый источник данных может измениться (или измениться приложением), а содержимое дисплея должно соответствовать этим изменениям. В следующих разделах вы узнаете, как управлять событиями щелчка элемента и обновлять `RecyclerView` при изменении базового источника данных.

### <a name="handling-item-click-events"></a>Обработка событий щелчка элемента

Когда пользователь касается элемента в `RecyclerView`, создается событие нажатия кнопки для уведомления приложения о том, какой элемент был затронут. Это событие не создается `RecyclerView` &ndash; вместо этого представление элемента (которое заключено в заполнитель представления) обнаруживает касания и сообщает об этом касаниях как о событиях щелчка.

Чтобы продемонстрировать, как можно обработать события нажатия кнопки, выполните следующие действия, чтобы узнать, как будет изменено базовое приложение для просмотра фотографий, чтобы сообщить, какая фотография была затронуты пользователем. При возникновении события щелчка элемента в примере приложения происходит следующая последовательность:

1. `CardView` фотографии обнаруживает событие щелчка элемента и уведомляет адаптер.

2. Адаптер перенаправляет событие (с информацией о позиции элемента) в обработчик элемента события Click.

3. Обработчик нажатия элемента действия реагирует на событие щелчка элемента.

Во-первых, в определение класса `PhotoAlbumAdapter` добавляется член обработчика событий с именем `ItemClick`.

```csharp
public event EventHandler<int> ItemClick;
```

Затем в `MainActivity`добавляется метод обработчика событий Click для элемента.
Этот обработчик кратко отображает всплывающее уведомление, указывающее, какой элемент фотографии был затронуты:

```csharp
void OnItemClick (object sender, int position)
{
    int photoNum = position + 1;
    Toast.MakeText(this, "This is photo number " + photoNum, ToastLength.Short).Show();
}

```

Далее требуется строка кода для регистрации обработчика `OnItemClick` с `PhotoAlbumAdapter`. Лучше всего сделать это сразу после создания `PhotoAlbumAdapter`: 

```csharp
mAdapter = new PhotoAlbumAdapter (mPhotoAlbum);
mAdapter.ItemClick += OnItemClick;

```

В этом базовом примере регистрация обработчика выполняется в методе `OnCreate` основного действия, но в рабочем приложении можно зарегистрировать обработчик в `OnResume` и отменить его регистрацию в `OnPause` &ndash; см. Дополнительные сведения в разделе [жизненный цикл действия](~/android/app-fundamentals/activity-lifecycle/index.md) .

`PhotoAlbumAdapter` теперь будет вызывать `OnItemClick` при получении события щелчка элемента. Следующим шагом является создание обработчика в адаптере, который вызывает это событие `ItemClick`. Следующий метод, `OnClick`, добавляется сразу после метода `ItemCount` адаптера:

```csharp
void OnClick (int position)
{
    if (ItemClick != null)
        ItemClick (this, position);
}
```

Этот `OnClick` метод является *прослушивателем* адаптера для событий щелчка элемента из представлений элементов. Прежде чем прослушиватель можно будет зарегистрировать в представлении элемента (через заполнитель представления элемента), конструктор `PhotoViewHolder` должен быть изменен, чтобы принять этот метод в качестве дополнительного аргумента, и зарегистрировать `OnClick` в представлении элемента `Click` события.
Ниже приведен измененный конструктор `PhotoViewHolder`.

```csharp
public PhotoViewHolder (View itemView, Action<int> listener)
    : base (itemView)
{
    Image = itemView.FindViewById<ImageView> (Resource.Id.imageView);
    Caption = itemView.FindViewById<TextView> (Resource.Id.textView);

    itemView.Click += (sender, e) => listener (base.LayoutPosition);
}

```

Параметр `itemView` содержит ссылку на `CardView`, которые были затронуты этим пользователем. Обратите внимание, что базовый класс-владелец представления знает положение элемента (`CardView`), которое он представляет (через свойство `LayoutPosition`), и эта позиция передается методу `OnClick` адаптера, когда происходит событие щелчка элемента. Метод `OnCreateViewHolder` адаптера изменяется для передачи метода `OnClick` адаптера в конструктор представления.

```csharp
PhotoViewHolder vh = new PhotoViewHolder (itemView, OnClick);
```

Теперь, когда вы создаете и запускаете пример приложения для просмотра фотографий, при нажатии фотографии на экране появится всплывающее уведомление, в котором сообщается об этой фотографии:

[![пример всплывающего уведомления, отображаемого при касании фотокарты](extending-the-example-images/01-photo-selected-sml.png)](extending-the-example-images/01-photo-selected.png#lightbox)

В этом примере демонстрируется только один подход к реализации обработчиков событий с `RecyclerView`. Другой подход, который можно использовать здесь, — поместить события в держатель представления и подписать адаптер на эти события. Если в примере Photo App была предоставлена возможность редактирования фотографий, то для `ImageView` и `TextView` в каждом `CardView`потребуются отдельные события. в `TextView` запустится диалоговое окно `EditView`, позволяющее пользователю редактировать заголовок. и касается `ImageView` запустит средство редактирования фотографий, позволяющее пользователю обрезать или вращать фотографию. В зависимости от потребностей приложения необходимо разработать лучший подход к обработке и реагированию на события касания.

Чтобы продемонстрировать, как можно обновить `RecyclerView` при изменении набора данных, можно изменить пример приложения для просмотра фотографий, чтобы случайно выбрать фотографию в источнике данных и заменить ее первой фотографией. Во-первых, в пример макета **Main. axml** для фотографии добавляется кнопка **случайного выбора** :

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent">
    <Button
        android:id="@+id/randPickButton"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:gravity="center_horizontal"
        android:textAppearance="?android:attr/textAppearanceLarge"
        android:text="Random Pick" />
    <android.support.v7.widget.RecyclerView
        android:id="@+id/recyclerView"
        android:scrollbars="vertical"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent" />
</LinearLayout>
```

Затем код добавляется в конце метода `OnCreate` основного действия для нахождение кнопки `Random Pick` в макете и присоединения к ней обработчика:

```csharp
Button randomPickBtn = FindViewById<Button>(Resource.Id.randPickButton);

randomPickBtn.Click += delegate
{
    if (mPhotoAlbum != null)
    {
        // Randomly swap a photo with the first photo:
        int idx = mPhotoAlbum.RandomSwap();
    }
};

```

Этот обработчик вызывает метод `RandomSwap` для фотоальбома при нажатии кнопки **случайного выбора** . Метод `RandomSwap` случайным образом меняет местами фотографию с первой фотографией в источнике данных, а затем возвращает индекс фотографии с произвольным переключением. При компиляции и запуске примера приложения с помощью этого кода нажатие кнопки **случайного выбора** не приводит к изменению экрана, так как `RecyclerView` не знает об изменении источника данных.

Чтобы обновление `RecyclerView` обновлялось после изменения источника данных, необходимо изменить обработчик щелчка **случайного выбора** , чтобы вызвать метод `NotifyItemChanged` адаптера для каждого элемента в коллекции, который был изменен (в данном случае изменились два элемента: первая фотография и пересъемная фотография). Это приведет к тому, что `RecyclerView` обновит свое отображение, чтобы оно соответствовало новому состоянию источника данных:

```csharp
Button randomPickBtn = FindViewById<Button>(Resource.Id.randPickButton);

randomPickBtn.Click += delegate
{
    if (mPhotoAlbum != null)
    {
        int idx = mPhotoAlbum.RandomSwap();

        // First photo has changed:
        mAdapter.NotifyItemChanged(0);

        // Swapped photo has changed:
        mAdapter.NotifyItemChanged(idx);
    }
};

```

Теперь, когда нажата кнопка **случайного выбора** , `RecyclerView` обновляет экран, чтобы показать, что фото, расположенное в коллекции, заменено первым фото в коллекции:

[![первый снимок экрана перед переключением, второй снимок экрана после переключения](extending-the-example-images/02-random-pick-sml.png)](extending-the-example-images/02-random-pick.png#lightbox)

Разумеется, `NotifyDataSetChanged` мог быть вызван вместо того, чтобы выполнить два вызова `NotifyItemChanged`, но при этом будет принудительно `RecyclerView` обновить всю коллекцию, несмотря на то, что были изменены только два элемента в коллекции. Вызов `NotifyItemChanged` значительно более эффективен, чем вызов `NotifyDataSetChanged`.

## <a name="related-links"></a>Связанные ссылки

- [Рециклервиевер (пример)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-recyclerviewer)
- [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)
- [RecyclerView части и функциональные возможности](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md)
- [Пример базового RecyclerView](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
