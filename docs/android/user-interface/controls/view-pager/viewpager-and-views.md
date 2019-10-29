---
title: ViewPager с представлениями
description: ViewPager — это Диспетчер макетов, позволяющий реализовать навигацию жестурал. Жестуралная навигация позволяет пользователю прокручивать влево и вправо для пошагового просмотра страниц данных. В этом руководстве объясняется, как реализовать прокрутку пользовательского интерфейса с помощью ViewPager и Пажертабстрип, используя представления в качестве страниц данных (в следующем руководстве описывается использование фрагментов для страниц).
ms.prod: xamarin
ms.assetid: 42E5379F-B0F4-4B87-A314-BF3DE405B0C8
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/01/2018
ms.openlocfilehash: 65a613f229f04a4ab01ca73a9c53c026add49f84
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029049"
---
# <a name="viewpager-with-views"></a>ViewPager с представлениями

_ViewPager — это Диспетчер макетов, позволяющий реализовать навигацию жестурал. Жестуралная навигация позволяет пользователю прокручивать влево и вправо для пошагового просмотра страниц данных. В этом руководстве объясняется, как реализовать прокрутку пользовательского интерфейса с помощью ViewPager и Пажертабстрип, используя представления в качестве страниц данных (в следующем руководстве описывается использование фрагментов для страниц)._

## <a name="overview"></a>Обзор

Это руководство содержит пошаговую демонстрацию того, как использовать `ViewPager` для реализации коллекции изображений листопадное и популярная деревьев. В этом приложении пользователь просматривает левое и правое по «каталогу дерева» для просмотра изображений дерева. В верхней части каждой страницы каталога отображается имя дерева в`PagerTabStrip`, а изображение дерева выводится в `ImageView`. Адаптер используется для взаимодействия `ViewPager` с базовой моделью данных. Это приложение реализует адаптер, производный от `PagerAdapter`. 

Несмотря на то, что приложения, основанные на `ViewPager`, часто реализуются с помощью `Fragment`s, существуют некоторые сравнительно простые случаи использования, в которых не требуется сложность `Fragment`s. Например, в базовом приложении Коллекция образов, показанном в этом пошаговом руководстве, не требуется использование `Fragment`s. Так как содержимое является статическим и пользователь просматривает только между разными образами, реализацию можно упростить, используя стандартные представления и макеты Android. 

## <a name="start-an-app-project"></a>Запуск проекта приложения

Создайте проект Android с именем **трипажер** (Дополнительные сведения о создании проектов Android см. в статье [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md) ). Затем запустите диспетчер пакетов NuGet. (Дополнительные сведения об установке пакетов NuGet см. [в разделе Пошаговое руководство. Включение NuGet в проект](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)). Найдите и установите **библиотеку поддержки Android v4**: 

[![снимок экрана поддержки версии 4, выбранного в диспетчере пакетов NuGet](viewpager-and-views-images/01-install-support-lib-sml.png)](viewpager-and-views-images/01-install-support-lib.png#lightbox)

При этом также будут установлены дополнительные пакеты, реакуиред с помощью **библиотеки поддержки Android версии 4**.

## <a name="add-an-example-data-source"></a>Добавление примера источника данных

В этом примере источник данных каталога дерева (представленный классом `TreeCatalog`) предоставляет `ViewPager` с содержимым элемента. 
`TreeCatalog` содержит готовый набор изображений дерева и заголовков деревьев, которые адаптер будет использовать для создания `View`s. Для конструктора `TreeCatalog` не требуются аргументы:

```csharp
TreeCatalog treeCatalog = new TreeCatalog();
```

Коллекция изображений в `TreeCatalog` организована таким образом, что индексатор может получить доступ к каждому изображению. Например, следующая строка кода извлекает идентификатор ресурса изображения для третьего изображения в коллекции: 

```csharp
int imageId = treeCatalog[2].imageId;
```

Поскольку сведения о реализации `TreeCatalog` не важны для понимания `ViewPager`, код `TreeCatalog` не указан здесь. Исходный код для `TreeCatalog` доступен по адресу [TreeCatalog.CS](https://github.com/xamarin/monodroid-samples/blob/master/UserInterface/TreePager/TreePager/TreeCatalog.cs). Скачайте этот исходный файл (или скопируйте и вставьте код в новый файл **TreeCatalog.CS** ) и добавьте его в проект. Кроме того, скачайте и распакуйте [файлы образа](https://github.com/xamarin/monodroid-samples/blob/master/UserInterface/TreePager/Resources/tree-images.zip?raw=true) в папку **Resources/Draw** и включите их в проект. 

## <a name="create-a-viewpager-layout"></a>Создание макета ViewPager

Откройте файл **Resources/Layout/Main. axml** и замените его содержимое следующим XML-кодом:

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.v4.view.ViewPager
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/viewpager"
    android:layout_width="match_parent"
    android:layout_height="match_parent" >

</android.support.v4.view.ViewPager>

```csharp
This XML defines a `ViewPager` that occupies the entire screen. Note that
you must use the fully-qualified name **android.support.v4.view.ViewPager**
because `ViewPager` is packaged in a support library. `ViewPager` is
available only from 
[Android Support Library v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/);
it is not available in the Android SDK. 

## Set up ViewPager

Edit **MainActivity.cs** and add the following `using` statement:

```csharp
using Android.Support.V4.View;
```

Замените метод `OnCreate` следующим кодом:

```csharp
protected override void OnCreate(Bundle bundle)
{
    base.OnCreate(bundle);
    SetContentView(Resource.Layout.Main);
    ViewPager viewPager = FindViewById<ViewPager>(Resource.Id.viewpager);
    TreeCatalog treeCatalog = new TreeCatalog();
}
```

Этот код выполняет следующие действия:

1. Задает представление из ресурса макета **Main. axml** .

2. Извлекает ссылку на `ViewPager` из макета.

3. Создает новый `TreeCatalog` в качестве источника данных.

При сборке и запуске этого кода вы увидите экран, который напоминает следующий снимок экрана: 

[![снимок экрана приложения, отображающего пустой ViewPager](viewpager-and-views-images/02-initial-screen-sml.png)](viewpager-and-views-images/02-initial-screen.png#lightbox)

На этом этапе `ViewPager` пуст, так как в нем отсутствует адаптер для доступа к содержимому в **трикаталог**. В следующем разделе создается **пажерадаптер** для подключения `ViewPager` к **трикаталог**. 

## <a name="create-the-adapter"></a>Создание адаптера

`ViewPager` использует объект контроллера адаптера, расположенный между `ViewPager` и источником данных (см. рисунок в разделе [адаптер](~/android/user-interface/controls/view-pager/index.md#adapter)). Для доступа к этим данным `ViewPager` требует предоставления пользовательского адаптера, производного от `PagerAdapter`. Этот адаптер заполняет каждую страницу `ViewPager` содержимым из источника данных. Поскольку этот источник данных зависит от конкретного приложения, Пользовательский адаптер является кодом, который понимает, как получить доступ к данным. По мере того, как пользователь просматривает страницы `ViewPager`, адаптер извлекает сведения из источника данных и загружает их на страницы для отображаемого `ViewPager`. 

При реализации `PagerAdapter`необходимо переопределить следующее:

- **Инстантиатеитем** &ndash; создает страницу (`View`) для заданной должности и добавляет ее в коллекцию представлений `ViewPager`. 

- **Дестройитем** &ndash; Удаляет страницу из заданной должности.

- **Count** &ndash; свойство только для чтения, которое возвращает количество доступных представлений (страниц). 

- **Исвиевфромобжект** &ndash; определяет, связана ли страница с конкретным объектом ключа. (Этот объект создается методом `InstantiateItem`.) В этом примере объект ключа является объектом данных `TreeCatalog`.

Добавьте новый файл с именем **TreePagerAdapter.CS** и замените его содержимое следующим кодом: 

```csharp
using System;
using Android.App;
using Android.Runtime;
using Android.Content;
using Android.Views;
using Android.Widget;
using Android.Support.V4.View;
using Java.Lang;

namespace TreePager
{
    class TreePagerAdapter : PagerAdapter
    {
        public override int Count
        {
            get { throw new NotImplementedException(); }
        }

        public override bool IsViewFromObject(View view, Java.Lang.Object obj)
        {
            throw new NotImplementedException();
        }

        public override Java.Lang.Object InstantiateItem (View container, int position)
        {
            throw new NotImplementedException();
        }

        public override void DestroyItem(View container, int position, Java.Lang.Object view)
        {
            throw new NotImplementedException();
        }
    }
}
```

Этот код заглушки является основной реализацией `PagerAdapter`. В следующих разделах каждый из этих методов заменяется рабочим кодом. 

### <a name="implement-the-constructor"></a>Реализация конструктора

Когда приложение создает экземпляр `TreePagerAdapter`, оно предоставляет контекст (`MainActivity`) и экземпляр `TreeCatalog`. Добавьте следующие переменные члена и конструктор в начало класса `TreePagerAdapter` в **TreePagerAdapter.CS**: 

```csharp
Context context;
TreeCatalog treeCatalog;

public TreePagerAdapter (Context context, TreeCatalog treeCatalog)
{
    this.context = context;
    this.treeCatalog = treeCatalog;
}
```

Этот конструктор предназначен для хранения контекста и `TreeCatalog` экземпляра, который будет использоваться `TreePagerAdapter`. 

### <a name="implement-count"></a>Число реализаций

`Count`ная реализация относительно проста: она возвращает количество деревьев в каталоге дерева. Замените `Count` следующим кодом:

```csharp
public override int Count
{
    get { return treeCatalog.NumTrees; }
}
```

Свойство `NumTrees` объекта `TreeCatalog` возвращает количество деревьев (число страниц) в наборе данных.

### <a name="implement-instantiateitem"></a>Реализация Инстантиатеитем

Метод `InstantiateItem` создает страницу для заданной должности. Он также должен добавить вновь созданное представление в коллекцию представлений `ViewPager`. Чтобы сделать это возможным, `ViewPager` передается в качестве параметра контейнера. 

Замените метод `InstantiateItem` следующим кодом:

```csharp
public override Java.Lang.Object InstantiateItem (View container, int position)
{
    var imageView = new ImageView (context);
    imageView.SetImageResource (treeCatalog[position].imageId);
    var viewPager = container.JavaCast<ViewPager>();
    viewPager.AddView (imageView);
    return imageView;
}
```

Этот код выполняет следующие действия:

1. Создает новый `ImageView` для вывода изображения дерева в указанной позиции. `MainActivity` приложения — это контекст, который будет передан конструктору `ImageView`.

2. Задает `ImageView` ресурсу `TreeCatalog` идентификатор ресурса изображения в указанной позиции.

3. Приводит переданный `View` контейнера к `ViewPager` ссылке.
    Обратите внимание, что для правильного выполнения этого приведения необходимо использовать `JavaCast<ViewPager>()` (это необходимо для того, чтобы Android выполнял преобразование типов, проверяемое во время выполнения).

4. Добавляет экземпляр `ImageView` в `ViewPager` и возвращает `ImageView` вызывающему объекту.

Когда `ViewPager` отображает образ в `position`, он отображает этот `ImageView`. Изначально `InstantiateItem` вызывается дважды для заполнения первых двух страниц с представлениями. По мере прокрутки пользователь вызывается снова, чтобы поддерживать представления сразу за пределами текущего отображаемого элемента. 

### <a name="implement-destroyitem"></a>Реализация Дестройитем

Метод `DestroyItem` Удаляет страницу из заданной должности. В приложениях, в которых можно изменить представление в любой заданной позиции, `ViewPager` должен иметь какой-либо способ удаления устаревшего представления в этой позиции перед его заменой новым представлением. В `TreeCatalog` примере представление в каждой позиции не изменяется, поэтому представление, удаленное с помощью `DestroyItem`, будет просто повторно Добавлено при вызове `InstantiateItem` для этой позиции. (Для повышения эффективности можно реализовать пул для перезапуска `View`s, которые будут повторно отображаться в той же позиции.) 

Замените метод `DestroyItem` следующим кодом: 

```csharp
public override void DestroyItem(View container, int position, Java.Lang.Object view)
{
    var viewPager = container.JavaCast<ViewPager>();
    viewPager.RemoveView(view as View);
}
```

Этот код выполняет следующие действия:

1. Приводит переданный `View` контейнера в ссылку на `ViewPager`.

2. Приводит переданный объект Java (`view`) в C#`View`(`view as View`);

3. Удаляет представление из `ViewPager`. 

### <a name="implement-isviewfromobject"></a>Реализация Исвиевфромобжект

По мере того как пользователь просматривает слайды влево и вправо по страницам содержимого, `ViewPager` вызывает `IsViewFromObject`, чтобы убедиться, что дочерний `View` в заданной позиции связан с объектом адаптера для той же позиции (следовательно, объект адаптера называется *ключом объекта* ). Для относительно простых приложений Ассоциация является одним из идентификаторов, &ndash; ключ объекта адаптера на этом экземпляре — это представление, которое ранее возвращалось в `ViewPager` с помощью `InstantiateItem`. Однако для других приложений ключ объекта может быть другим экземпляром класса адаптера, связанным с (но не аналогичным) дочерним представлением, которое `ViewPager` отображается в этой позиции. Только адаптер знает, связаны ли переданные представления и ключ объекта. 

для правильной работы `PagerAdapter` необходимо реализовать `IsViewFromObject`. Если `IsViewFromObject` возвращает `false` для заданной позиции, `ViewPager` не будет отображать представление в этой позиции. В приложении `TreePager` ключ объекта, возвращаемый `InstantiateItem`, *является* страницей `View` дерева, поэтому код должен проверять только удостоверение (т. е. ключ объекта и представление являются одним и тем же). Замените `IsViewFromObject` следующим кодом: 

```csharp
public override bool IsViewFromObject(View view, Java.Lang.Object obj)
{
    return view == obj;
}
```

## <a name="add-the-adapter-to-the-viewpager"></a>Добавление адаптера в ViewPager

Теперь, когда `TreePagerAdapter` реализована, пришло время добавить его в `ViewPager`. В **MainActivity.CS**добавьте следующую строку кода в конец метода `OnCreate`:

```csharp
viewPager.Adapter = new TreePagerAdapter(this, treeCatalog);
```

Этот код создает экземпляр `TreePagerAdapter`, передавая `MainActivity` в качестве контекста (`this`). Экземпляр `TreeCatalog` передается в второй аргумент конструктора. Свойству `Adapter` `ViewPager`присваивается экземпляр объекта `TreePagerAdapter`. в этом случае `TreePagerAdapter` будет вставлен в `ViewPager`. 

Основная реализация теперь завершена &ndash; сборки и запуска приложения. На экране появится первое изображение каталога дерева, как показано на рисунке слева на следующем снимке экрана. Проведите влево, чтобы увидеть другие представления в виде дерева, а затем проведите вправо для перемещения назад по каталогу дерева: 

[![снимки экрана прокрутки приложений Трипажер с помощью изображений дерева](viewpager-and-views-images/03-example-views-sml.png)](viewpager-and-views-images/03-example-views.png#lightbox)

## <a name="add-a-pager-indicator"></a>Добавление индикатора страничного навигатора

Эта минимальная `ViewPager` реализация отображает изображения каталога дерева, но не указывает, где находится пользователь в каталоге. Следующим шагом является добавление `PagerTabStrip`. `PagerTabStrip` информирует пользователя о том, какая страница отображается, и предоставляет контекст навигации, отображая указание предыдущей и следующей страниц. `PagerTabStrip` предназначено для использования в качестве индикатора для текущей страницы `ViewPager`; он прокручивается и обновляется по мере того, как пользователь просматривает каждую страницу. 

Откройте **ресурсы/макет/Main. axml** и добавьте в макет `PagerTabStrip`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.v4.view.ViewPager
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/viewpager"
    android:layout_width="match_parent"
    android:layout_height="match_parent" >

    <android.support.v4.view.PagerTabStrip
          android:layout_width="match_parent"
          android:layout_height="wrap_content"
          android:layout_gravity="top"
          android:paddingBottom="10dp"
          android:paddingTop="10dp"
          android:textColor="#fff" />

</android.support.v4.view.ViewPager>
```

`ViewPager` и `PagerTabStrip` предназначены для совместной работы. При объявлении `PagerTabStrip` в макете `ViewPager` `ViewPager` автоматически найдет `PagerTabStrip` и подключится к адаптеру. При сборке и запуске приложения в верхней части каждого экрана должно отобразиться пустое `PagerTabStrip`: 

[снимок экрана![крупный план пустого Пажертабстрип](viewpager-and-views-images/04-empty-pagetabstrip-cap-sml.png)](viewpager-and-views-images/04-empty-pagetabstrip-cap.png#lightbox)

### <a name="display-a-title"></a>Отображение заголовка

Чтобы добавить заголовок на каждую вкладку страницы, реализуйте метод `GetPageTitleFormatted` в классе, производном от `PagerAdapter`. `ViewPager` вызывает метод `GetPageTitleFormatted` (Если реализован), чтобы получить строку заголовка, описывающую страницу в указанной позиции. Добавьте следующий метод в класс `TreePagerAdapter` в **TreePagerAdapter.CS**: 

```csharp
public override Java.Lang.ICharSequence GetPageTitleFormatted(int position)
{
    return new Java.Lang.String(treeCatalog[position].caption);
}
```

Этот код извлекает строку заголовка дерева из указанной страницы (расположения) в каталоге дерева, преобразует ее в `String`Java и возвращает в `ViewPager`. При запуске приложения с помощью этого нового метода на каждой странице отображается заголовок дерева в `PagerTabStrip`. Вы увидите имя дерева в верхней части экрана без подчеркивания: 

[![снимки экрана страниц с заполненными текстом Пажертабстрип табуляции](viewpager-and-views-images/05-final-pagetabstrip-sml.png)](viewpager-and-views-images/05-final-pagetabstrip.png#lightbox)

Для просмотра каждого изображения в виде дерева в каталоге можно прокрутить вперед и назад. 

### <a name="pagertitlestrip-variation"></a>Пажертитлестрип

`PagerTitleStrip` очень похожа на `PagerTabStrip` за исключением того, что `PagerTabStrip` добавляет подчеркивание для текущей выбранной вкладки. Вы можете заменить `PagerTabStrip` `PagerTitleStrip` в приведенном выше макете и запустить приложение еще раз, чтобы увидеть, как оно выглядит с `PagerTitleStrip`. 

[![Пажертитлестрип с подчеркиванием, удаленным из текста](viewpager-and-views-images/06-pagetitlestrip-example-sml.png)](viewpager-and-views-images/06-pagetitlestrip-example.png#lightbox)

Обратите внимание, что подчеркивание удаляется при преобразовании в `PagerTitleStrip`. 

## <a name="summary"></a>Сводка

В этом пошаговом руководстве представлен пошаговый пример создания базового приложения на основе `ViewPager`без использования `Fragment`s. В нем представлен пример источника данных, содержащий изображения и строки заголовков, макет `ViewPager` для отображения изображений, а также подкласс `PagerAdapter`, соединяющий `ViewPager` с источником данных. Чтобы помочь пользователю перемещаться по набору данных, были добавлены инструкции, объясняющие, как добавить `PagerTabStrip` или `PagerTitleStrip` для отображения заголовка изображения в верхней части каждой страницы. 

## <a name="related-links"></a>Связанные ссылки

- [Трипажер (пример)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/userinterface-treepager)
