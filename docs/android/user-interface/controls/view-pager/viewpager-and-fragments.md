---
title: ViewPager с фрагментами
description: ViewPager — это Диспетчер макетов, позволяющий реализовать навигацию жестурал. Жестуралная навигация позволяет пользователю прокручивать влево и вправо для пошагового просмотра страниц данных. В этом руководство объясняется, как реализовать прокрутку пользовательского интерфейса с помощью ViewPager, используя фрагменты в качестве страниц данных.
ms.prod: xamarin
ms.assetid: 62B6286F-3680-48F3-B91B-453692E457E5
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: 0657e89e3026cbe2d146d538ca0158ed1d1806d2
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2019
ms.locfileid: "69522738"
---
# <a name="viewpager-with-fragments"></a>ViewPager с фрагментами

_ViewPager — это Диспетчер макетов, позволяющий реализовать навигацию жестурал. Жестуралная навигация позволяет пользователю прокручивать влево и вправо для пошагового просмотра страниц данных. В этом руководство объясняется, как реализовать прокрутку пользовательского интерфейса с помощью ViewPager, используя фрагменты в качестве страниц данных._

 
## <a name="overview"></a>Обзор

`ViewPager`часто используется в сочетании с фрагментами, что упрощает управление жизненным циклом каждой страницы в `ViewPager`. В этом пошаговом руководстве `ViewPager` используется для создания приложения с именем **флашкардпажер** , которое представляет ряд математических проблем на Flash-картах. Каждая флэш-карта реализуется как фрагмент. Пользователь выполняет прокрутку влево и вправо через карты флэш-памяти и применяет математическую проблему, чтобы показать свой ответ. Это приложение создает `Fragment` экземпляр для каждой флэш-карты и реализует адаптер, производный `FragmentPagerAdapter`от. В [Viewpager и представлениях](~/android/user-interface/controls/view-pager/viewpager-and-views.md)большая часть работы выполнялась в `MainActivity` методах жизненного цикла. В **флашкардпажер**большая часть работы `Fragment` выполняется в одном из методов жизненного цикла. 

В этом руководство не рассматриваются основы фрагментов &ndash; , если вы еще не знакомы с фрагментами в Xamarin. Android. Дополнительные сведения см. в разделе [фрагменты](~/android/platform/fragments/index.md) , которые помогут начать работу с фрагментами. 



## <a name="start-an-app-project"></a>Запуск проекта приложения

Создайте новый проект Android с именем **флашкардпажер**. Затем запустите диспетчер пакетов NuGet (Дополнительные сведения об установке пакетов NuGet см. в разделе [пошаговое руководство. Включение NuGet в проект](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)). Найдите и установите пакет **Xamarin. Android. support. v4** , как описано в [Viewpager и представлениях](~/android/user-interface/controls/view-pager/viewpager-and-views.md). 



## <a name="add-an-example-data-source"></a>Добавление примера источника данных

В **флашкардпажер**источник данных — это колода флэш-карт, представленная `FlashCardDeck` классом. Этот источник `ViewPager` данных предоставляет содержимое элемента. `FlashCardDeck`содержит готовый набор математических проблем и ответов. `FlashCardDeck` Конструктор не требует аргументов: 

```csharp
FlashCardDeck flashCards = new FlashCardDeck();
```

Коллекция карт Flash в `FlashCardDeck` организована таким, что индексатор может получить доступ к каждой флэш-карте. Например, следующая строка кода извлекает проблему с четвертой картой флэш-памяти в колоде: 

```csharp
string problem = flashCardDeck[3].Problem;
```

Эта строка кода извлекает соответствующий ответ на предыдущую проблему:

```csharp
string answer = flashCardDeck[3].Answer;
```

Поскольку сведения о `FlashCardDeck` реализации не важны для понимания `ViewPager`, `FlashCardDeck` код не указан здесь.
Исходный код `FlashCardDeck` доступен по адресу [FlashCardDeck.CS](https://github.com/xamarin/monodroid-samples/blob/master/UserInterface/FlashCardPager/FlashCardPager/FlashCardDeck.cs).
Скачайте этот исходный файл (или скопируйте и вставьте код в новый файл **FlashCardDeck.CS** ) и добавьте его в проект.



## <a name="create-a-viewpager-layout"></a>Создание макета ViewPager

Откройте файл Resources **/Layout/Main. axml** и замените его содержимое следующим XML-кодом:

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.v4.view.ViewPager
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/viewpager"
    android:layout_width="match_parent"
    android:layout_height="match_parent" >

    </android.support.v4.view.ViewPager>
```

Этот XML-код `ViewPager` определяет, который занимает весь экран. Обратите внимание, что необходимо использовать полное имя **Android. support. v4. View. ViewPager** , так `ViewPager` как оно упаковано в библиотеку поддержки. `ViewPager`доступен только в [библиотеке поддержки Android v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/); он недоступен в пакет SDK для Android.


## <a name="set-up-viewpager"></a>Настройка ViewPager

Измените **MainActivity.CS** и добавьте следующие `using` инструкции:

```csharp
using Android.Support.V4.View;
using Android.Support.V4.App;
```

Измените объявление `FragmentActivity`класса так, чтобы он был производным от: `MainActivity`

```csharp
public class MainActivity : FragmentActivity
```

`MainActivity`является производным`FragmentActivity` от ( `Activity`а не) `FragmentActivity` , так как знает, как управлять поддержкой фрагментов. Замените метод `OnCreate` следующим кодом: 

```csharp
protected override void OnCreate(Bundle bundle)
{
    base.OnCreate(bundle);
    SetContentView(Resource.Layout.Main);
    ViewPager viewPager = FindViewById<ViewPager>(Resource.Id.viewpager);
    FlashCardDeck flashCards = new FlashCardDeck();
}
```

Этот код выполняет следующие действия:

1. Задает представление из ресурса макета **Main. axml** .

2. Извлекает ссылку `ViewPager` на из макета.

3. Создает новый `FlashCardDeck` объект в качестве источника данных.

При сборке и запуске этого кода вы увидите экран, который напоминает следующий снимок экрана: 

[![Снимок экрана приложения Флашкардпажер с пустым ViewPager](viewpager-and-fragments-images/01-initial-screen-sml.png)](viewpager-and-fragments-images/01-initial-screen.png#lightbox)

На этом этапе элемент `ViewPager` пуст, поскольку в нем отсутствуют фрагменты, используемые для заполнения `ViewPager`, и у него отсутствует адаптер для создания этих фрагментов из данных в **флашкарддекк**. 

В следующих разделах `FlashCardFragment` создается, чтобы реализовать функциональные возможности каждой флэш-карты, `FragmentPagerAdapter` и создается для подключения `ViewPager` к фрагментам, созданным на основе данных в `FlashCardDeck`. 



## <a name="create-the-fragment"></a>Создание фрагмента

Каждая флэш-карта будет управляться фрагментом пользовательского интерфейса `FlashCardFragment`с именем. `FlashCardFragment`будет отображать сведения, содержащиеся в одной флэш-карте. Каждый экземпляр `FlashCardFragment` будет размещен в `ViewPager`. 
`FlashCardFragment`будет состоять из `TextView` , который отображает текст проблемы с флэш-картой. `Toast` В этом представлении будет реализован обработчик событий, использующий для отображения ответа, когда пользователь касается вопроса из флэш-карты. 



### <a name="create-the-flashcardfragment-layout"></a>Создание макета Флашкардфрагмент

Прежде `FlashCardFragment` чем можно будет реализовать, необходимо определить его макет. Этот макет является макетом контейнера фрагментов для одного фрагмента. Добавьте новый макет Android в **ресурсы и макет** с именем **flashcard_layout. axml**. Откройте **Resources/Layout/flashcard_layout. axml** и замените его содержимое следующим кодом: 

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <TextView
        android:id="@+id/flash_card_question"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:gravity="center"
            android:textAppearance="@android:style/TextAppearance.Large"
            android:textSize="100sp"
            android:layout_centerHorizontal="true"
            android:layout_centerVertical="true"
            android:text="Question goes here" />
    </RelativeLayout>
```

Этот макет определяет один фрагмент флэш-карты. Каждый фрагмент состоит из `TextView` , который отображает математическую проблему, используя крупный (100SP) шрифт. Этот текст выравнивается по центру на флэш-карте по вертикали и по горизонтали. 



### <a name="create-the-initial-flashcardfragment-class"></a>Создание исходного класса Флашкардфрагмент

Добавьте новый файл с именем **FlashCardFragment.CS** и замените его содержимое следующим кодом:

```csharp
using System;
using Android.OS;
using Android.Views;
using Android.Widget;
using Android.Support.V4.App;

namespace FlashCardPager
{
    public class FlashCardFragment : Android.Support.V4.App.Fragment
    {
        public FlashCardFragment() { }

        public static FlashCardFragment newInstance(String question, String answer)
        {
            FlashCardFragment fragment = new FlashCardFragment();
            return fragment;
        }
        public override View OnCreateView (
            LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState)
        {
            View view = inflater.Inflate (Resource.Layout.flashcard_layout, container, false);
            TextView questionBox = (TextView)view.FindViewById (Resource.Id.flash_card_question);
            return view;
        }
    }
}
```

Этот код заглушки является основным `Fragment` определением, которое будет использоваться для показа флэш-карты. Обратите `FlashCardFragment` внимание, что является производным от `Fragment` версии библиотеки поддержки `Android.Support.V4.App.Fragment`, определенной в. Конструктор пуст, поэтому `newInstance` для создания нового `FlashCardFragment` вместо конструктора используется метод фабрики. 

Метод Lifecycle создает и настраивает `TextView`. `OnCreateView` Он расширяет макет для фрагмента `TextView` и возвращает его `TextView` в вызывающий объект. `LayoutInflater`и `ViewGroup` передаются `OnCreateView` в, чтобы можно было сделать макет неизменным. Пакет содержит данные, которые `OnCreateView` `TextView` используют для повторного создания из сохраненного состояния. `savedInstanceState` 

Представление фрагмента явно размещается путем вызова метода `inflater.Inflate`. Аргумент — это родительский элемент представления, `false` а флаг указывает, что неструктурированное представление нужно добавить к родительскому элементу представления (оно будет добавлено при `ViewPager` последующем вызове `GetItem` метода адаптера). `container` Пошаговое руководство). 



### <a name="add-state-code-to-flashcardfragment"></a>Добавить код состояния в Флашкардфрагмент

Как и действие, фрагмент имеет объект `Bundle` , который используется для сохранения и извлечения его состояния. В **флашкардпажер** `Bundle` используется для сохранения текста вопроса и ответа для связанной флэш-карты. В **FlashCardFragment.CS**добавьте следующие `Bundle` ключи в начало `FlashCardFragment` определения класса: 

```csharp
private static string FLASH_CARD_QUESTION = "card_question";
private static string FLASH_CARD_ANSWER = "card_answer";
```

Измените метод `Bundle` фабрики таким образом, чтобы он создавал объект и использовал приведенные выше ключи для сохранения переданного текста вопроса и ответа в фрагменте после его создания. `newInstance` 

```csharp
public static FlashCardFragment newInstance(String question, String answer)
{
    FlashCardFragment fragment = new FlashCardFragment();

    Bundle args = new Bundle();
    args.PutString(FLASH_CARD_QUESTION, question);
    args.PutString(FLASH_CARD_ANSWER, answer);
    fragment.Arguments = args;

    return fragment;
}
```

Измените метод `OnCreateView` жизненного цикла фрагмента, чтобы получить эти сведения из переданного пакета и загрузить текст `TextBox`вопроса в: 

```csharp
public override View OnCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState)
{
    string question = Arguments.GetString(FLASH_CARD_QUESTION, "");
    string answer = Arguments.GetString(FLASH_CARD_ANSWER, "");

    View view = inflater.Inflate(Resource.Layout.flashcard_layout, container, false);
    TextView questionBox = (TextView)view.FindViewById(Resource.Id.flash_card_question);
    questionBox.Text = question;

    return view;
}
```

`answer` Переменная не используется здесь, но будет использоваться позже, когда код обработчика событий будет добавлен в этот файл. 


## <a name="create-the-adapter"></a>Создание адаптера

`ViewPager`использует объект контроллера адаптера, расположенный между `ViewPager` объектом и источником данных (см. иллюстрацию в статье [адаптера](~/android/user-interface/controls/view-pager/index.md#adapter) ViewPager). Для доступа к этим данным `ViewPager` необходимо предоставить пользовательский адаптер, производный от. `PagerAdapter` `FragmentPagerAdapter` Поскольку в &ndash; `PagerAdapter`этом примере используются фрагменты, то используется производный от. `FragmentPagerAdapter` 
`FragmentPagerAdapter`представляет каждую страницу в виде `Fragment` , которая постоянно сохраняется в диспетчере фрагментов до тех пор, пока пользователь может вернуться на страницу. По `ViewPager`мере того `FragmentPagerAdapter` , как пользователь просматривает страницы, компонент извлекает данные из источника данных и использует их для создания `Fragment`для отображаемого объекта `ViewPager` . 

При реализации класса `FragmentPagerAdapter`необходимо переопределить следующее:

- **Количество** &ndash; Свойство только для чтения, возвращающее число доступных представлений (страниц).

- **Элемент** &ndash; Возвращает фрагмент, отображаемый для указанной страницы.

Добавьте новый файл с именем **FlashCardDeckAdapter.CS** и замените его содержимое следующим кодом:

```csharp
using System;
using Android.Views;
using Android.Widget;
using Android.Support.V4.App;

namespace FlashCardPager
{
    class FlashCardDeckAdapter : FragmentPagerAdapter
    {
        public FlashCardDeckAdapter (Android.Support.V4.App.FragmentManager fm, FlashCardDeck flashCards)
            : base(fm)
        {
        }

        public override int Count
        {
            get { throw new NotImplementedException(); }
        }

        public override Android.Support.V4.App.Fragment GetItem(int position)
        {
            throw new NotImplementedException();
        }
    }
}
```

Этот код является заглушкой для `FragmentPagerAdapter` основной реализации. В следующих разделах каждый из этих методов заменяется рабочим кодом. Целью конструктора является передача диспетчера `FlashCardDeckAdapter`фрагментов в конструктор базового класса. 



### <a name="implement-the-adapter-constructor"></a>Реализация конструктора адаптера

Когда приложение создает экземпляр `FlashCardDeckAdapter`, оно предоставляет ссылку на диспетчер фрагментов и `FlashCardDeck`экземпляр. Добавьте следующую переменную члена в начало `FlashCardDeckAdapter` класса в **FlashCardDeckAdapter.CS**: 

```csharp
public FlashCardDeck flashCardDeck;
```

Добавьте в `FlashCardDeckAdapter` конструктор следующую строку кода: 

```csharp
this.flashCardDeck = flashCards;
```

`FlashCardDeck` В`FlashCardDeckAdapter` этой строке кода хранится экземпляр, который будет использоваться. 



### <a name="implement-count"></a>Число реализаций

`Count` Реализация относительно проста: она возвращает число карт Flash в колоде флэш-карты. Замените `Count` следующим кодом: 

```csharp
public override int Count
{
    get { return flashCardDeck.NumCards; }
}
```


`NumCards` СвойствовозвращаетколичествокартFlash(числофрагментов)`FlashCardDeck` в наборе данных. 



### <a name="implement-getitem"></a>Реализация элемента Item

`GetItem` Метод возвращает фрагмент, связанный с заданной позицией. Когда `GetItem` вызывается для позиции в колоде флэш-карты, она возвращает значение `FlashCardFragment` , настроенное для вывода проблемы с картой флэш-памяти в этой позиции. Замените метод `GetItem` следующим кодом: 

```csharp
public override Android.Support.V4.App.Fragment GetItem(int position)
{
    return (Android.Support.V4.App.Fragment)
        FlashCardFragment.newInstance (
            flashCardDeck[position].Problem, flashCardDeck[position].Answer);
}
```

Этот код выполняет следующие действия:

1. Ищет строку математической проблемы в `FlashCardDeck` колоде для указанной должности. 

2. Ищет строку ответов в `FlashCardDeck` колоде для указанной должности. 

3. Вызывает метод `FlashCardFragment` `newInstance`фабрики, передавая строки проблем и ответов на флэш-карту. 

4. Создает и возвращает новую флэш-карту `Fragment` , содержащую текст вопроса и ответа для этой должности. 

Когда объект `ViewPager` готовится к `Fragment` `position`просмотру в, он `TextBox` отображает содержащуюся в нем строку математической `position` проблемы, находящуюся в колоде флэш-карты. 



## <a name="add-the-adapter-to-the-viewpager"></a>Добавление адаптера в ViewPager

Теперь, `FlashCardDeckAdapter` когда реализуется, пришло время добавить его `ViewPager`в. В **MainActivity.CS**добавьте следующую строку кода в конец `OnCreate` метода:

```csharp
FlashCardDeckAdapter adapter =
    new FlashCardDeckAdapter(SupportFragmentManager, flashCards);
viewPager.Adapter = adapter;
```

Этот код создает экземпляр `FlashCardDeckAdapter`, передавая `SupportFragmentManager` в первом аргументе. (Свойство `SupportFragmentManager` объекта FragmentActivity используется для получения ссылки на `FragmentManager` &ndash; дополнительные сведения о `FragmentManager`, см. раздел [Управление фрагментами](~/android/platform/fragments/managing-fragments.md).) 

Основная реализация теперь завершает &ndash; сборку и запускает приложение.
На экране появится первое изображение колоды флэш-карты, как показано на рисунке слева на следующем снимке экрана. Проведите влево, чтобы увидеть больше видеоадаптеров, а затем проведите вправо, чтобы перейти назад через колоду флэш-карты:

[![Примеры снимков экрана приложения Флашкардпажер без индикаторов страничного навигатора](viewpager-and-fragments-images/02-example-views-sml.png)](viewpager-and-fragments-images/02-example-views.png#lightbox)



## <a name="add-a-pager-indicator"></a>Добавление индикатора страничного навигатора

Эта минимальная `ViewPager` реализация отображает каждую карту флэш-памяти в колоде, но не указывает, где находится пользователь в колоде. Следующим шагом является добавление `PagerTabStrip`. `PagerTabStrip` Информирует пользователя о том, какой номер проблемы отображается, и предоставляет контекст навигации, отображая указание предыдущей и следующей флэш-карты. 

Откройте **ресурсы/макет/Main. axml** и добавьте `PagerTabStrip` в макет:

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.v4.view.ViewPager xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/pager"
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

При сборке и запуске приложения в верхней части каждой флэш-карты должно `PagerTabStrip` отобразиться пустое значение. 

[![Крупный план Пажертабстрип без текста](viewpager-and-fragments-images/03-empty-pagetabstrip-sml.png)](viewpager-and-fragments-images/03-empty-pagetabstrip.png#lightbox)



### <a name="display-a-title"></a>Отображение заголовка

Чтобы добавить заголовок на каждую вкладку страницы, реализуйте `GetPageTitleFormatted` метод в адаптере. `ViewPager`вызывает `GetPageTitleFormatted` метод (Если реализован), чтобы получить строку заголовка, описывающую страницу в указанной позиции. Добавьте следующий метод `FlashCardDeckAdapter` в класс в **FlashCardDeckAdapter.CS**: 

```csharp
public override Java.Lang.ICharSequence GetPageTitleFormatted(int position)
{
    return new Java.Lang.String("Problem " + (position + 1));
}
```

Этот код преобразует расположение в колоде флэш-карты в номер проблемы. Результирующая строка преобразуется в Java `String` , возвращаемое `ViewPager`в. При запуске приложения с помощью этого нового метода на каждой странице отображается номер проблемы в `PagerTabStrip`: 

[![Снимки экрана Флашкардпажер с номером проблемы, отображаемым над каждой страницей](viewpager-and-fragments-images/04-pagetabstrip-sml.png)](viewpager-and-fragments-images/04-pagetabstrip.png#lightbox)

Вы можете прокрутить экран, чтобы увидеть номер проблемы в колоде Flash-карт, который отображается в верхней части каждой флэш-карты. 



## <a name="handle-user-input"></a>Обработку входных данных пользователя

**Флашкардпажер** представляет серию видеоадаптеров `ViewPager`, основанных на фрагментах, но еще не имеет способа обнаружить ответ для каждой проблемы. В этом разделе обработчик событий добавляется в, `FlashCardFragment` чтобы отображать ответ, когда пользователь отменяет текст проблемы с флэш-картой. 

Откройте **FlashCardFragment.CS** и добавьте следующий код в конец `OnCreateView` метода непосредственно перед возвратом представления вызывающему объекту: 

```csharp
questionBox.Click += delegate
{
    Toast.MakeText(Activity.ApplicationContext,
            "Answer: " + answer, ToastLength.Short).Show();
};
```

Этот `Click` обработчик событий отображает ответ в виде всплывающего уведомления, которое появляется при `TextBox`касании пользователем. Переменная была инициализирована ранее при считывании сведений о состоянии из пакета, переданного в `OnCreateView`. `answer` Выполните сборку и запуск приложения, а затем коснитесь текста проблемы на каждой флэш-карте, чтобы увидеть ответ: 

[![Снимки экрана с всплывающими уведомлениями о приложении Флашкардпажер при касании математической задачи](viewpager-and-fragments-images/05-answer-sml.png)](viewpager-and-fragments-images/05-answer.png#lightbox)

`FragmentActivity` **Флашкардпажер** `MainActivity` ,представленныйв`AppCompatActivity` этом пошаговом руководстве, использует производную от, но можно также создать производную от(котораятакжеобеспечиваетподдержкудляуправленияфрагментами).`MainActivity` Пример `AppCompatActivity` см. в разделе [FlashCardPager](https://docs.microsoft.com/samples/xamarin/monodroid-samples/userinterface-flashcardpager) в коллекции примеров.



## <a name="summary"></a>Сводка

В этом пошаговом руководстве представлен пошаговый пример создания базового `ViewPager`приложения с помощью `Fragment`s. В нем представлен пример источника данных, который содержит вопросы и ответы по флэш `ViewPager` -картам, макет для отображения карт флэш `FragmentPagerAdapter` -памяти и подкласс `ViewPager` , который подключает к источнику данных. Чтобы помочь пользователю перемещаться по картам флэш-памяти, были добавлены инструкции, объясняющие, `PagerTabStrip` как добавить, чтобы отобразить номер проблемы в верхней части каждой страницы. Наконец, добавлен код обработки событий для вывода ответа, когда пользователь касается проблемы с флэш-картой. 



## <a name="related-links"></a>Связанные ссылки

- [Флашкардпажер (пример)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/userinterface-flashcardpager)
