---
title: Использование Xamarin. Android Designer
description: Эта статья представляет собой пошаговое руководство по Xamarin. Android Designer. Здесь показано, как создать пользовательский интерфейс для небольшого приложения браузера цвета. Этот пользовательский интерфейс полностью создан в конструкторе.
ms.prod: xamarin
ms.assetid: 70FF2F9A-71BD-317E-C881-A44D82DF1BD8
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 07/25/2018
ms.openlocfilehash: df83bdfcc847b07754a349060c9be1613efd0b08
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029527"
---
# <a name="using-the-xamarinandroid-designer"></a>Использование Xamarin. Android Designer

_Эта статья представляет собой пошаговое руководство по Xamarin. Android Designer. Здесь показано, как создать пользовательский интерфейс для небольшого приложения браузера цвета. Этот пользовательский интерфейс полностью создан в конструкторе._

## <a name="overview"></a>Обзор

Пользовательские интерфейсы Android можно создавать декларативно с помощью XML-файлов или программно, путем написания кода. Xamarin. Android Designer позволяет разработчикам создавать и изменять декларативные макеты визуально, не требуя ручного редактирования XML-файлов. Конструктор также предоставляет отзыв в режиме реального времени, позволяющий разработчикам оценивать изменения пользовательского интерфейса без необходимости повторного развертывания приложения на устройстве или в эмуляторе. Эти функции конструктора могут ускорить разработку пользовательского интерфейса Android невероятно.
В этой статье показано, как использовать Xamarin. Android Designer для визуального создания пользовательского интерфейса.

> [!TIP]
> Новые выпуски Visual Studio поддерживают открытие XML-файлов в Android Designer.
>
> Android Designer поддерживает как файлы AXML, так и XML.

## <a name="walkthrough"></a>Пошаговое руководство

Цель этого пошагового руководства — использовать Android Designer для создания пользовательского интерфейса для примера приложения браузера цветов. Приложение браузера цветов отображает список цветов, их имена и значения RGB. Вы узнаете, как добавлять мини-приложения в **область конструктора** , а также как визуально размещать эти мини-приложения. После этого вы узнаете, как изменить мини-приложения в интерактивном режиме на **область конструктора** или с помощью панели **свойств** конструктора. Наконец, вы увидите, как выглядит дизайн при запуске приложения на устройстве или в эмуляторе.

<!-- markdownlint-disable MD001 -->

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

### <a name="creating-a-new-project"></a>Создание нового проекта

Первым шагом является создание проекта Xamarin. Android. Запустите Visual Studio, щелкните **создать проект...** и выберите шаблон **Visual C\# > Android > Android (Xamarin)** .
Назовите новое приложение **десигнервалксраугх** и нажмите кнопку **ОК**.

[![пустое приложение Android](designer-walkthrough-images/vs/01-android-app-w158-sml.png)](designer-walkthrough-images/vs/01-android-app-w158.png#lightbox)

В диалоговом окне **новое приложение Android** выберите **пустое приложение** и нажмите кнопку **ОК**.

[![выборе шаблона Пустое приложение Android](designer-walkthrough-images/vs/02-blank-app-w158-sml.png)](designer-walkthrough-images/vs/02-blank-app-w158.png#lightbox)

### <a name="adding-a-layout"></a>Добавление макета

Следующим шагом является создание **элемент LinearLayout** , который будет содержать элементы пользовательского интерфейса. Щелкните правой кнопкой мыши **ресурсы и макет** в **Обозреватель решений** и выберите **Добавить > новый элемент...** . В диалоговом окне **Добавление нового элемента** выберите **Макет Android**. Назовите файл **list_item** и нажмите кнопку **Добавить**:

[![новый макет](designer-walkthrough-images/vs/03-new-layout-w158-sml.png)](designer-walkthrough-images/vs/03-new-layout-w158.png#lightbox)

В конструкторе отобразится новый макет **list_item** . Обратите внимание, что две панели отображаются, &ndash; *область конструктора* для **list_item** видна на левой панели, пока его источник XML отображается на правой панели. Можно поменять местами **область конструктора** и **исходных** областей, щелкнув значок **подкачки панелей** , расположенный между двумя панелями:

[представление конструктора![](designer-walkthrough-images/vs/04-designer-view-w158-sml.png)](designer-walkthrough-images/vs/04-designer-view-w158.png#lightbox)

В меню **вид** выберите пункт **другие окна > Структура документа** , чтобы открыть **структуру документа**. В **структуре документа** показано, что в настоящее время макет содержит одно мини-приложение **элемент LinearLayout** :

[![структура документа](designer-walkthrough-images/vs/06-document-outline-w158-sml.png)](designer-walkthrough-images/vs/06-document-outline-w158.png#lightbox)

Следующим шагом является создание пользовательского интерфейса для приложения браузера цветов в рамках этого `LinearLayout`.

### <a name="creating-the-list-item-user-interface"></a>Создание пользовательского интерфейса элемента списка

Если панель **элементов** не отображается, щелкните вкладку **область элементов** слева. В **области элементов**прокрутите вниз до раздела **Images & Media (изображения** ) и прокрутите вниз, пока не обнаружите `ImageView`:

[![нахождение Имажевиев](designer-walkthrough-images/vs/07-locate-imageview-w158-sml.png)](designer-walkthrough-images/vs/07-locate-imageview-w158.png#lightbox)

Кроме того, можно ввести *имажевиев* в строку поиска, чтобы найти `ImageView`:

[![поиска Имажевиев](designer-walkthrough-images/vs/08-imageview-search-w158-sml.png)](designer-walkthrough-images/vs/08-imageview-search-w158.png#lightbox)

Перетащите этот `ImageView` на область конструктора (этот `ImageView` будет использоваться для вывода образца цвета в приложении браузера цветов):

[![Имажевиев на холсте](designer-walkthrough-images/vs/09-imageview-on-canvas-w158-sml.png)](designer-walkthrough-images/vs/09-imageview-on-canvas-w158.png#lightbox)

Затем перетащите `LinearLayout (Vertical)` мини-приложение из **области элементов** в конструктор. Обратите внимание, что синий контур указывает границы добавленного `LinearLayout`. **Структура документа** показывает, что это дочерний элемент `LinearLayout`, расположенный в разделе `imageView1 (ImageView)`:

[Синий контур![](designer-walkthrough-images/vs/10-blue-outline-w158-sml.png)](designer-walkthrough-images/vs/10-blue-outline-w158.png#lightbox)

При выборе `ImageView` в конструкторе синяя структура перемещается вокруг `ImageView`. Кроме того, выделение перемещается в `imageView1 (ImageView)` в **структуре документа**:

[![SELECT Имажевиев](designer-walkthrough-images/vs/11-select-imageview-w158-sml.png)](designer-walkthrough-images/vs/11-select-imageview-w158.png#lightbox)

Затем перетащите `Text (Large)` мини-приложение из **панели элементов** в недавно добавленные `LinearLayout`. Обратите внимание, что в конструкторе используются зеленые выделения, указывающие, куда будет вставлен новый мини-приложение:

[![зеленые выделения](designer-walkthrough-images/vs/12-green-highlight-w158-sml.png)](designer-walkthrough-images/vs/12-green-highlight-w158.png#lightbox)

Затем добавьте мини-приложение `Text (Small)`, расположенное под `Text (Large)` мини-приложение:

[![добавить небольшое мини-приложение текста](designer-walkthrough-images/vs/13-add-small-text-w158-sml.png)](designer-walkthrough-images/vs/13-add-small-text-w158.png#lightbox)

На этом этапе область конструктора должна выглядеть как на следующем снимке экрана:

[макет конструктора![](designer-walkthrough-images/vs/14-raw-layout-w158-sml.png)](designer-walkthrough-images/vs/14-raw-layout-w158.png#lightbox)

Если два `textView` мини-приложений находятся не в `linearLayout1`, их можно перетащить в `linearLayout1` в **структуре документа** , чтобы они отображались так, как показано на предыдущем снимке экрана (с отступом в разделе `linearLayout1`).

### <a name="arranging-the-user-interface"></a>Упорядочение пользовательского интерфейса

Следующим шагом является изменение пользовательского интерфейса для показа `ImageView` слева, при этом два `TextView` мини-приложения располагаются справа от `ImageView`.

1. Выберите `ImageView`.

2. В **окно свойств**введите *Width* в поле поиска и найдите **ширину макета**.

3. Измените значение **ширины макета** на `wrap_content`:

![Задать содержимое для переноса](designer-walkthrough-images/vs/15-wrap-content-w158.png)

Другим способом изменения параметра `Width` является щелчок треугольника в правой части мини-приложения для переключения ширины значения `wrap_content`:

![Перетащите, чтобы задать ширину](designer-walkthrough-images/vs/15b-width-arrow-w158.png)

Если щелкнуть треугольник еще раз, параметр `Width` будет возвращен `match_parent`. Затем перейдите на панель **Структура документа** и выберите корневой `LinearLayout`:

[![выбрать корневой элемент LinearLayout](designer-walkthrough-images/vs/16-root-linearlayout-w158-sml.png)](designer-walkthrough-images/vs/16-root-linearlayout-w158.png#lightbox)

Выбрав корневой `LinearLayout`, вернитесь на панель **свойств** , введите *Orientation* в поле поиска и найдите параметр **ориентации** . Измените **ориентацию** на `horizontal`:

![Выбрать ориентацию по горизонтали](designer-walkthrough-images/vs/17-horizontal-orientation-w158.png)

На этом этапе область конструктора должна выглядеть примерно так, как показано на снимке экрана ниже.
Обратите внимание, что `TextView` мини-приложений были перемещены справа от `ImageView`:

[макет конструктора![](designer-walkthrough-images/vs/18-designer-layout-w158-sml.png)](designer-walkthrough-images/vs/18-designer-layout-w158.png#lightbox)

### <a name="modifying-the-spacing"></a>Изменение расстояния

Следующим шагом является изменение параметров заполнения и полей в пользовательском интерфейсе, чтобы предоставить больше пространства между мини-приложениями. Выберите `ImageView` в области конструктора. В области **Свойства** введите `min` в поле поиска. Введите `70dp` для **минимальной высоты** и `50dp` для **минимальной ширины**:

[![задать высоту и ширину](designer-walkthrough-images/vs/18b-set-height-width-sml.png)](designer-walkthrough-images/vs/18b-set-height-width.png#lightbox)

В области **Свойства** введите `padding` в поле поиска и введите `10dp` для **заполнения**. Эти `minHeight`, `minWidth` и `padding` параметры добавляют отступ вокруг всех сторон `ImageView` и елонгате их по вертикали. Обратите внимание, что XML макета изменяется при вводе этих значений:

[Заполнение набора![](designer-walkthrough-images/vs/19-padding-widths-w158-sml.png)](designer-walkthrough-images/vs/19-padding-widths-w158.png#lightbox)

Параметры "снизу", "слева", "справа" и "верхнее заполнение" можно задавать независимо, вводя значения в поля " **отступ снизу**", " **Отступ слева**", " **справа**" и " **Заполнение" верхних** полей соответственно.
Например, задайте для поля **левое** поле значение `5dp`, а для полей Отступ **снизу**, **Отступ справа**и **Заполнение верхние** поля — `10dp`.

[![настраиваемые параметры заполнения](designer-walkthrough-images/vs/20-custom-padding-w158-sml.png)](designer-walkthrough-images/vs/20-custom-padding-w158.png#lightbox)

Затем измените расположение мини-приложения `LinearLayout`, которое содержит два мини-приложения `TextView`. В области **Структура документа**выберите `linearLayout1`. В окне **Свойства** введите `margin` в поле поиска. Задайте для **поля макета нижний край**, **левое поле макета**и **верхнее поле макета** `5dp`. Задайте для **поля макета право** `0dp`.

[![заданных полей](designer-walkthrough-images/vs/21-margins-w158-sml.png)](designer-walkthrough-images/vs/21-margins-w158.png#lightbox)

### <a name="removing-the-default-image"></a>Удаление изображения по умолчанию

Так как `ImageView` используется для показа цветов (а не изображений), следующим шагом является удаление источника изображения по умолчанию, добавленного шаблоном.

1. Выберите `ImageView` в **области конструктора**.

2. В окне **Свойства**введите *src* в поле поиска.

3. Щелкните маленький квадрат справа от параметра свойства **src** и выберите **сбросить**.

[![очистить параметр Имажевиев src](designer-walkthrough-images/vs/22-clear-img-src-w158-sml.png)](designer-walkthrough-images/vs/22-clear-img-src-w158.png#lightbox)

Это приведет к удалению `android:src="@android:drawable/ic_menu_gallery"` из исходного XML-файла для этого `ImageView`.

### <a name="adding-a-listview-container"></a>Добавление контейнера ListView

Теперь, когда макет **list_item** определен, следующий шаг заключается в добавлении `ListView` в основной макет. Этот `ListView` будет содержать список **list_item**. 

В **Обозреватель решений**откройте **Resources/Layout/activity_main. axml**. На **панели элементов**выберите мини-приложение `ListView` и перетащите его на **область конструктора**. `ListView` в конструкторе будет пустым, за исключением синих линий, в которых отображается граница, если она выбрана. Можно просмотреть **структуру документа** , чтобы убедиться, что **ListView** добавлен правильно:

[![нового ListView](designer-walkthrough-images/vs/23-new-listview-w158-sml.png)](designer-walkthrough-images/vs/23-new-listview-w158.png#lightbox)

По умолчанию `ListView` получает `Id` значение `@+id/listView1`.
Пока `listView1` по-прежнему выбран в **структуре документа**, откройте панель **свойства** , щелкните **упорядочение по**и выберите **Категория**.
Откройте **главное**, выберите свойство **ID** и измените его значение на `@+id/myListView`:

[![переименовать идентификатор в Милиствиев](designer-walkthrough-images/vs/24-change-id-w158-sml.png)](designer-walkthrough-images/vs/24-change-id-w158.png#lightbox)

На этом этапе пользовательский интерфейс готов к использованию.

### <a name="running-the-application"></a>Запуск приложения

Откройте **MainActivity.CS** и замените его код следующим:

```csharp
using Android.App;
using Android.Widget;
using Android.Views;
using Android.OS;
using Android.Support.V7.App;
using System.Collections.Generic;

namespace DesignerWalkthrough
{
    [Activity(Label = "@string/app_name", Theme = "@style/AppTheme", MainLauncher = true)]
    public class MainActivity : AppCompatActivity
    {
        List<ColorItem> colorItems = new List<ColorItem>();
        ListView listView;

        protected override void OnCreate(Bundle savedInstanceState)
        {
            base.OnCreate(savedInstanceState);

            // Set our view from the "main" layout resource
            SetContentView(Resource.Layout.activity_main);
            listView = FindViewById<ListView>(Resource.Id.myListView);

            colorItems.Add(new ColorItem()
            {
                Color = Android.Graphics.Color.DarkRed,
                ColorName = "Dark Red",
                Code = "8B0000"
            });
            colorItems.Add(new ColorItem()
            {
                Color = Android.Graphics.Color.SlateBlue,
                ColorName = "Slate Blue",
                Code = "6A5ACD"
            });
            colorItems.Add(new ColorItem()
            {
                Color = Android.Graphics.Color.ForestGreen,
                ColorName = "Forest Green",
                Code = "228B22"
            });

            listView.Adapter = new ColorAdapter(this, colorItems);
        }
    }

    public class ColorAdapter : BaseAdapter<ColorItem>
    {
        List<ColorItem> items;
        Activity context;
        public ColorAdapter(Activity context, List<ColorItem> items)
            : base()
        {
            this.context = context;
            this.items = items;
        }
        public override long GetItemId(int position)
        {
            return position;
        }
        public override ColorItem this[int position]
        {
            get { return items[position]; }
        }
        public override int Count
        {
            get { return items.Count; }
        }
        public override View GetView(int position, View convertView, ViewGroup parent)
        {
            var item = items[position];

            View view = convertView;
            if (view == null) // no view to re-use, create new
                view = context.LayoutInflater.Inflate(Resource.Layout.list_item, null);
            view.FindViewById<TextView>(Resource.Id.textView1).Text = item.ColorName;
            view.FindViewById<TextView>(Resource.Id.textView2).Text = item.Code;
            view.FindViewById<ImageView>(Resource.Id.imageView1).SetBackgroundColor(item.Color);

            return view;
        }
    }

    public class ColorItem
    {
        public string ColorName { get; set; }
        public string Code { get; set; }
        public Android.Graphics.Color Color { get; set; }
    }
}

```

Этот код использует пользовательский адаптер `ListView` для загрузки сведений о цвете и для вывода этих данных в пользовательском интерфейсе, который был только что создан. Чтобы не усложнять этот пример, сведения о цвете жестко запрограммированы в списке, но адаптер можно изменить, чтобы извлечь сведения о цвете из источника данных или вычислить его на лету. Дополнительные сведения о `ListView` адаптерах см. в разделе [ListView](~/android/user-interface/layouts/list-view/index.md).

Выполните сборку и запуск приложения. На следующем снимке экрана представлен пример того, как приложение отображается при запуске на устройстве.

[снимок экрана![](designer-walkthrough-images/vs/25-final-screenshot-sml.png)](designer-walkthrough-images/vs/25-final-screenshot.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio для Mac](#tab/macos)

### <a name="creating-a-new-project"></a>Создание нового проекта

Первым шагом является создание проекта Xamarin. Android.

Запустите Visual Studio для Mac и нажмите кнопку **создать проект...** . Выберите шаблон **приложения Android** и нажмите кнопку **Далее**.

[![пустое приложение Android](designer-walkthrough-images/xs/01-android-app-m75-sml.png)](designer-walkthrough-images/xs/01-android-app-m75.png#lightbox)

Назовите новое приложение **десигнервалксраугх**. В разделе **целевые платформы**выберите **Последняя и наибольшая** и нажмите кнопку **Далее**.

[Имя приложения![](designer-walkthrough-images/xs/02-designer-walkthrough-m75-sml.png)](designer-walkthrough-images/xs/02-designer-walkthrough-m75.png#lightbox)

На следующем экране диалогового окна нажмите кнопку **создать**.

### <a name="adding-a-layout"></a>Добавление макета

Следующим шагом является создание **элемент LinearLayout** , который будет содержать элементы пользовательского интерфейса.

В Visual Studio для Mac щелкните правой кнопкой мыши **ресурсы и макет** на панели **решения** и выберите **Добавить > новый файл...** . В диалоговом окне **новый файл** выберите **> макет Android**. Назовите файл **list_item** и нажмите кнопку **создать**:

[![новый макет](designer-walkthrough-images/xs/03-new-layout-m75-sml.png)](designer-walkthrough-images/xs/03-new-layout-m75.png#lightbox)

После добавления этого файла на **область конструктора** отображается новый макет **list_item** (если отображается сообщение, *этот проект содержит ресурсы, которые не были успешно скомпилированы, может быть затронуто подготовка к просмотру*, щелкните **Сборка > Собрать все** для сборки проекта):

[представление конструктора![](designer-walkthrough-images/xs/04-designer-view-m75-sml.png)](designer-walkthrough-images/xs/04-designer-view-m75.png#lightbox)

Щелкните вкладку **источник** в нижней части конструктора, чтобы просмотреть источник XML для этого макета. Если щелкнуть вкладку **Структура документа** справа, будет показано, что в данный момент макет содержит одно мини-приложение **элемент LinearLayout** :

[XML-код конструктора![](designer-walkthrough-images/xs/05-designer-xml-m75-sml.png)](designer-walkthrough-images/xs/05-designer-xml-m75.png#lightbox)

Следующим шагом является создание пользовательского интерфейса для приложения браузера цветов.

### <a name="creating-the-list-item-user-interface"></a>Создание пользовательского интерфейса элемента списка

Щелкните вкладку **конструктор** в нижней части экрана, чтобы вернуться в **область конструктора**. В области **элементов** справа перейдите к разделу **Images & Media** и найдите `ImageView`:

[![нахождение Имажевиев](designer-walkthrough-images/xs/06-locate-imageview-m75-sml.png)](designer-walkthrough-images/xs/06-locate-imageview-m75.png#lightbox)

Кроме того, можно ввести *имажевиев* в строку поиска, чтобы найти `ImageView`:

[![поиска Имажевиев](designer-walkthrough-images/xs/07-imageview-search-m75-sml.png)](designer-walkthrough-images/xs/07-imageview-search-m75.png#lightbox)

Перетащите этот `ImageView` на **область конструктора** (этот `ImageView` будет использоваться для вывода образца цвета в приложении браузера цветов):

[![Имажевиев на холсте](designer-walkthrough-images/xs/08-imageview-on-canvas-m75-sml.png)](designer-walkthrough-images/xs/08-imageview-on-canvas-m75.png#lightbox)

Затем перетащите `LinearLayout (Vertical)` мини-приложение из **панели элементов** в **область конструктора**. Обратите внимание, что синий контур указывает границы добавленного `LinearLayout`. В **структуре документа** показано, что это дочерний элемент `LinearLayout`, расположенный ниже `imageView1 (ImageView)`.

[Синий контур![](designer-walkthrough-images/xs/10-blue-outline-m75-sml.png)](designer-walkthrough-images/xs/10-blue-outline-m75.png#lightbox)

При выборе `ImageView` в конструкторе синяя структура перемещается вокруг `ImageView`. Кроме того, выделение перемещается в `imageView1 (ImageView)` в **структуре документа**:

[![SELECT Имажевиев](designer-walkthrough-images/xs/11-select-imageview-m75-sml.png)](designer-walkthrough-images/xs/11-select-imageview-m75.png#lightbox)

Затем перетащите `Text (Large)` мини-приложение из **панели элементов** в недавно добавленные `LinearLayout`. Обратите внимание, что при перетаскивании указателя мыши на **область конструктора**он выделяет, куда будет вставлено новое мини-приложение.
Мини-приложение `Text (Large)` должно находиться в `linearLayout1`, как показано ниже:

[![Добавление мини-приложения крупного текста](designer-walkthrough-images/xs/12-green-highlight-m75-sml.png)](designer-walkthrough-images/xs/12-green-highlight-m75.png#lightbox)

Затем добавьте мини-приложение `Text (Small)` под мини – приложением `Text (Large)`. На этом этапе **область конструктора** должен выглядеть примерно следующим снимком экрана:

[![добавить небольшое мини-приложение текста](designer-walkthrough-images/xs/13-add-small-text-m75-sml.png)](designer-walkthrough-images/xs/13-add-small-text-m75.png#lightbox)

Если два `textView` мини-приложений находятся вне `linearLayout1`, их можно перетащить в `linearLayout1` в **структуре документа** , чтобы они отображались так, как показано на предыдущем снимке экрана (с отступом в разделе `linearLayout1`).

### <a name="arranging-the-user-interface"></a>Упорядочение пользовательского интерфейса

Следующим шагом является изменение пользовательского интерфейса для показа `ImageView` слева, при этом два `TextView` мини-приложения располагаются справа от `ImageView`.

1. Выбрав `ImageView`, перейдите на вкладку **Свойства** .

2. Сразу под вкладкой **Свойства** нажмите кнопку **Макет**.

3. Прокрутите вниз до раздела **виевграуп** и измените значение параметра `Width` на `wrap_content`:

[![задать содержимое для переноса](designer-walkthrough-images/xs/15-wrap-content-m75-sml.png)](designer-walkthrough-images/xs/15-wrap-content-m75.png#lightbox)

Другим способом изменения параметра `Width` является щелчок треугольника в правой части мини-приложения для переключения ширины значения `wrap_content`:

[![перетащите, чтобы задать ширину](designer-walkthrough-images/xs/16-width-arrow-m75-sml.png)](designer-walkthrough-images/xs/16-width-arrow-m75.png#lightbox)

Если щелкнуть треугольник еще раз, параметр `Width` будет возвращен `match_parent`. Затем перейдите на панель **Структура документа** и выберите корневой `LinearLayout`:

[![выбрать корневой элемент LinearLayout](designer-walkthrough-images/xs/17-root-linearlayout-m75-sml.png)](designer-walkthrough-images/xs/17-root-linearlayout-m75.png#lightbox)

Выбрав корневой `LinearLayout`, вернитесь на вкладку **Свойства** и щелкните **мини**-приложение. Измените значение параметра `Orientation` на `horizontal`, как показано ниже. На этом этапе **область конструктора** должен выглядеть примерно так, как показано на снимке экрана ниже. Обратите внимание, что `TextView` мини-приложений были перемещены справа от `ImageView`:

[![выбрать ориентацию по горизонтали](designer-walkthrough-images/xs/18-horizontal-orientation-m75-sml.png)](designer-walkthrough-images/xs/18-horizontal-orientation-m75.png#lightbox)

### <a name="modifying-the-spacing"></a>Изменение расстояния

Следующим шагом является изменение настроек заполнения и полей в пользовательском интерфейсе, чтобы предоставить больше пространства между мини-приложениями. Выберите `ImageView` и перейдите на вкладку **Макет** в разделе **свойства**. Измените `Min Width` на `50dp`, `Min Height` `70dp`, а `Padding` — на `10dp`.
Это применяет заполнение вокруг всех сторон `ImageView` и елонгатес его по вертикали:

[Заполнение набора![](designer-walkthrough-images/xs/20-padding-widths-m75-sml.png)](designer-walkthrough-images/xs/20-padding-widths-m75.png#lightbox)

Параметры верхнего, правого, нижнего и левого отступов можно задать независимо, введя значения в поля `Top`, `Right`, `Bottom`и `Left` заполнения соответственно. Например, задайте для параметра `Left` заполнение значение `5dp` и `Top`, `Right`и `Bottom` значения заполнения `10dp`. Обратите внимание, что параметр `Padding` изменяется на список следующих значений с разделителями-запятыми:

[![настраиваемые параметры заполнения](designer-walkthrough-images/xs/21-custom-padding-m75-sml.png)](designer-walkthrough-images/xs/21-custom-padding-m75.png#lightbox)

Затем измените расположение мини-приложения `LinearLayout`, которое содержит два мини-приложения `TextView`. В области **Структура документа**выберите `linearLayout1`. На панели **Свойства** перейдите на вкладку **Макет** . Прокрутите вниз до раздела **виевграуп** и задайте для полей `Left`, `Top`, `Right`и `Bottom` поля значение `5dp`, `5dp`, `0dp`и `5dp` соответственно. :

[![заданных полей](designer-walkthrough-images/xs/22-margins-m75-sml.png)](designer-walkthrough-images/xs/22-margins-m75.png#lightbox)

### <a name="removing-the-default-image"></a>Удаление изображения по умолчанию

Так как `ImageView` используется для показа цветов (а не изображений), следующим шагом является удаление источника изображения по умолчанию, добавленного шаблоном.

1. Выберите `ImageView`.

2. Перейдите на вкладку **мини** -приложение в разделе **свойства**.

3. Очистите параметр `Src`, чтобы он был пустым:

[![очистить параметр Имажевиев src](designer-walkthrough-images/xs/23-clear-src-m75-sml.png)](designer-walkthrough-images/xs/23-clear-src-m75.png#lightbox)

Это приведет к удалению `android:src="@android:drawable/ic_menu_gallery"` из исходного XML-файла для этого `ImageView`.

### <a name="adding-a-listview-container"></a>Добавление контейнера ListView

Теперь, когда макет **list_item** определен, следующий шаг заключается в добавлении `ListView` в основной макет. Этот `ListView` будет содержать список **list_item**. 

В **Обозреватель решений**откройте **ресурсы/макет/Main. axml**.
Щелкните мини-приложение `Button` (если оно есть) и удалите его. На **панели элементов**выберите мини-приложение `ListView` и перетащите его на **область конструктора**.
`ListView` в конструкторе будет пустым, за исключением синих линий, в которых отображается граница, если она выбрана. Можно просмотреть **структуру документа** , чтобы убедиться, что **ListView** добавлен правильно:

[![нового ListView](designer-walkthrough-images/xs/24-new-listview-m75-sml.png)](designer-walkthrough-images/xs/24-new-listview-m75.png#lightbox)

По умолчанию `ListView` получает `Id` значение `@+id/listView1`.
Пока `listView1` по-прежнему выбран в **структуре документа**, откройте панель **свойства** , щелкните **упорядочение по**и выберите **Категория**.
Откройте **главное**, выберите свойство **ID** и измените его значение на `@+id/myListView`:

[![переименовать идентификатор в Милиствиев](designer-walkthrough-images/xs/25-change-id-m75-sml.png)](designer-walkthrough-images/xs/25-change-id-m75.png#lightbox)

На этом этапе пользовательский интерфейс готов к использованию.

### <a name="running-the-application"></a>Запуск приложения

Откройте **MainActivity.CS** и замените его код следующим:

```csharp
using Android.App;
using Android.Widget;
using Android.Views;
using Android.OS;
using System.Collections.Generic;

namespace DesignerWalkthrough
{
    [Activity(Label = "@string/app_name", MainLauncher = true)]
    public class MainActivity : Activity
    {
        List<ColorItem> colorItems = new List<ColorItem>();
        ListView listView;

        protected override void OnCreate(Bundle savedInstanceState)
        {
            base.OnCreate(savedInstanceState);

            // Set our view from the "main" layout resource
            SetContentView(Resource.Layout.Main);
            listView = FindViewById<ListView>(Resource.Id.myListView);

            colorItems.Add(new ColorItem()
            {
                Color = Android.Graphics.Color.DarkRed,
                ColorName = "Dark Red",
                Code = "8B0000"
            });
            colorItems.Add(new ColorItem()
            {
                Color = Android.Graphics.Color.SlateBlue,
                ColorName = "Slate Blue",
                Code = "6A5ACD"
            });
            colorItems.Add(new ColorItem()
            {
                Color = Android.Graphics.Color.ForestGreen,
                ColorName = "Forest Green",
                Code = "228B22"
            });

            listView.Adapter = new ColorAdapter(this, colorItems);
        }
    }

    public class ColorAdapter : BaseAdapter<ColorItem>
    {
        List<ColorItem> items;
        Activity context;
        public ColorAdapter(Activity context, List<ColorItem> items)
            : base()
        {
            this.context = context;
            this.items = items;
        }
        public override long GetItemId(int position)
        {
            return position;
        }
        public override ColorItem this[int position]
        {
            get { return items[position]; }
        }
        public override int Count
        {
            get { return items.Count; }
        }
        public override View GetView(int position, View convertView, ViewGroup parent)
        {
            var item = items[position];

            View view = convertView;
            if (view == null) // no view to re-use, create new
                view = context.LayoutInflater.Inflate(Resource.Layout.list_item, null);
            view.FindViewById<TextView>(Resource.Id.textView1).Text = item.ColorName;
            view.FindViewById<TextView>(Resource.Id.textView2).Text = item.Code;
            view.FindViewById<ImageView>(Resource.Id.imageView1).SetBackgroundColor(item.Color);

            return view;
        }
    }

    public class ColorItem
    {
        public string ColorName { get; set; }
        public string Code { get; set; }
        public Android.Graphics.Color Color { get; set; }
    }
}
```

Этот код использует пользовательский адаптер `ListView` для загрузки сведений о цвете и для вывода этих данных в пользовательском интерфейсе, который был только что создан. Чтобы не усложнять этот пример, сведения о цвете жестко запрограммированы в списке, но адаптер можно изменить, чтобы извлечь сведения о цвете из источника данных или вычислить его на лету. Дополнительные сведения о `ListView` адаптерах см. в разделе [ListView](~/android/user-interface/layouts/list-view/index.md).

Выполните сборку и запуск приложения. На следующем снимке экрана представлен пример того, как приложение отображается при запуске на устройстве.

[снимок экрана![](designer-walkthrough-images/xs/26-final-screenshot-sml.png)](designer-walkthrough-images/xs/26-final-screenshot.png#lightbox)

-----

## <a name="summary"></a>Сводка

В этой статье описывается процесс использования Xamarin. Android Designer в Visual Studio для создания пользовательского интерфейса для базового приложения.
Было продемонстрировано, как создать интерфейс для одного элемента в списке и продемонстрировать, как добавлять мини-приложения и размещать их визуально.
Также было объяснено, как назначать ресурсы, а затем задавать различные свойства для этих мини-приложений.
