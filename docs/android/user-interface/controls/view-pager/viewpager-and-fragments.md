---
title: ViewPager с фрагментами
description: ViewPager — это Диспетчер макетов, позволяющий реализовать навигацию жестурал. Жестуралная навигация позволяет пользователю прокручивать влево и вправо для пошагового просмотра страниц данных. В этом руководство объясняется, как реализовать прокрутку пользовательского интерфейса с помощью ViewPager, используя фрагменты в качестве страниц данных.
ms.prod: xamarin
ms.assetid: 62B6286F-3680-48F3-B91B-453692E457E5
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/01/2018
ms.openlocfilehash: d0fdfa44ce6caa0c5f0e0aa2eed6406e606eacc4
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029035"
---
# <a name="viewpager-with-fragments"></a>ViewPager с фрагментами

_ViewPager — это Диспетчер макетов, позволяющий реализовать навигацию жестурал. Жестуралная навигация позволяет пользователю прокручивать влево и вправо для пошагового просмотра страниц данных. В этом руководство объясняется, как реализовать прокрутку пользовательского интерфейса с помощью ViewPager, используя фрагменты в качестве страниц данных._

## <a name="overview"></a>Обзор

`ViewPager` часто используется вместе с фрагментами, что упрощает управление жизненным циклом каждой страницы в `ViewPager`. В этом пошаговом руководстве `ViewPager` используется для создания приложения с именем **флашкардпажер** , которое представляет ряд математических проблем на Flash-картах. Каждая флэш-карта реализуется как фрагмент. Пользователь выполняет прокрутку влево и вправо через карты флэш-памяти и применяет математическую проблему, чтобы показать свой ответ. Это приложение создает `Fragment` экземпляр для каждой флэш-карты и реализует адаптер, производный от `FragmentPagerAdapter`. В [Viewpager и представлениях](~/android/user-interface/controls/view-pager/viewpager-and-views.md)большая часть работы выполнялась в `MainActivity` методы жизненного цикла. В **флашкардпажер**большая часть работы выполняется `Fragment` в одном из методов жизненного цикла. 

В этом руководство не рассматриваются основы фрагментов &ndash; если вы еще не знакомы с фрагментами в Xamarin. Android, см. [фрагменты](~/android/platform/fragments/index.md) , которые помогут начать работу с фрагментами. 

## <a name="start-an-app-project"></a>Запуск проекта приложения

Создайте новый проект Android с именем **флашкардпажер**. Затем запустите диспетчер пакетов NuGet (Дополнительные сведения об установке пакетов NuGet см. [в разделе Пошаговое руководство. Включение NuGet в проект](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)). Найдите и установите пакет **Xamarin. Android. support. v4** , как описано в [Viewpager и представлениях](~/android/user-interface/controls/view-pager/viewpager-and-views.md). 

## <a name="add-an-example-data-source"></a>Добавление примера источника данных

В **флашкардпажер**источник данных — это колода флэш-карт, представленная классом `FlashCardDeck`. Этот источник данных предоставляет `ViewPager` с содержимым элемента. `FlashCardDeck` содержит готовый набор математических проблем и ответов на них. Для конструктора `FlashCardDeck` не требуются аргументы: 

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

Поскольку сведения о реализации `FlashCardDeck` не важны для понимания `ViewPager`, код `FlashCardDeck` не указан здесь.
Исходный код для `FlashCardDeck` доступен по адресу [FlashCardDeck.CS](https://github.com/xamarin/monodroid-samples/blob/master/UserInterface/FlashCardPager/FlashCardPager/FlashCardDeck.cs).
Скачайте этот исходный файл (или скопируйте и вставьте код в новый файл **FlashCardDeck.CS** ) и добавьте его в проект.

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
```

Этот XML-код определяет `ViewPager`, который занимает весь экран. Обратите внимание, что необходимо использовать полное имя **Android. support. v4. View. ViewPager** , так как `ViewPager` упаковывается в библиотеку поддержки. `ViewPager` доступен только в [библиотеке поддержки Android v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/); он недоступен в пакет SDK для Android.

## <a name="set-up-viewpager"></a>Настройка ViewPager

Измените **MainActivity.CS** и добавьте следующие `using` операторы:

```csharp
using Android.Support.V4.View;
using Android.Support.V4.App;
```

Измените объявление класса `MainActivity` так, чтобы он был производным от `FragmentActivity`:

```csharp
public class MainActivity : FragmentActivity
```

`MainActivity` является производным от`FragmentActivity` (а не `Activity`), так как `FragmentActivity` знает, как управлять поддержкой фрагментов. Замените метод `OnCreate` следующим кодом: 

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

2. Извлекает ссылку на `ViewPager` из макета.

3. Создает новый `FlashCardDeck` в качестве источника данных.

При сборке и запуске этого кода вы увидите экран, который напоминает следующий снимок экрана: 

[![снимок экрана приложения Флашкардпажер с пустым ViewPager](viewpager-and-fragments-images/01-initial-screen-sml.png)](viewpager-and-fragments-images/01-initial-screen.png#lightbox)

На этом этапе `ViewPager` пуст, так как в нем отсутствуют фрагменты, используемые для заполнения `ViewPager`, и в нем отсутствует адаптер для создания этих фрагментов из данных в **флашкарддекк**. 

В следующих разделах `FlashCardFragment` создается для реализации функциональности каждой флэш-карты, и создается `FragmentPagerAdapter` для подключения `ViewPager` к фрагментам, созданным на основе данных в `FlashCardDeck`. 

## <a name="create-the-fragment"></a>Создание фрагмента

Каждая флэш-карта будет управляться фрагментом пользовательского интерфейса, который называется `FlashCardFragment`. в представлении `FlashCardFragment`отображаются сведения, содержащиеся в одной флэш-карте. Каждый экземпляр `FlashCardFragment` будет размещен в `ViewPager`. 
представление `FlashCardFragment`будет состоять из `TextView`, в котором отображается текст проблемы флэш-карты. В этом представлении будет реализован обработчик событий, использующий `Toast` для отображения ответа, когда пользователь касается вопроса из флэш-карты. 

### <a name="create-the-flashcardfragment-layout"></a>Создание макета Флашкардфрагмент

Прежде чем `FlashCardFragment` можно будет реализовать, необходимо определить его макет. Этот макет является макетом контейнера фрагментов для одного фрагмента. Добавьте новый макет Android в **ресурсы и макет** с именем **flashcard_layout. axml**. Откройте **Resources/Layout/flashcard_layout. axml** и замените его содержимое следующим кодом: 

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

Этот макет определяет один фрагмент флэш-карты. Каждый фрагмент состоит из `TextView`, который отображает математическую проблему, используя крупный (100SP) шрифт. Этот текст выравнивается по центру на флэш-карте по вертикали и по горизонтали. 

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

Этот код заглушки использует базовое определение `Fragment`, которое будет использоваться для показа флэш-карты. Обратите внимание, что `FlashCardFragment` является производной от версии библиотеки поддержки `Fragment`, определенной в `Android.Support.V4.App.Fragment`. Конструктор пуст, поэтому `newInstance` метод фабрики используется для создания нового `FlashCardFragment` вместо конструктора. 

Метод жизненного цикла `OnCreateView` создает и настраивает `TextView`. Он увеличивает макет для `TextView` фрагмента и возвращает неизменный `TextView` вызывающему объекту. `LayoutInflater` и `ViewGroup` передаются `OnCreateView`, чтобы они могли сделать макет неизменным. Пакет `savedInstanceState` содержит данные, которые `OnCreateView` использует для повторного создания `TextView` из сохраненного состояния. 

Представление фрагмента явно расширяется путем вызова метода `inflater.Inflate`. Аргумент `container` — это родительский элемент представления, а флаг `false` указывает невозможность добавления несведенного представления к родительскому элементу представления (он будет добавлен, когда `ViewPager` вызов метода `GetItem` адаптера далее в этом пошаговом руководстве). 

### <a name="add-state-code-to-flashcardfragment"></a>Добавить код состояния в Флашкардфрагмент

Как и действие, фрагмент имеет `Bundle`, который он использует для сохранения и извлечения его состояния. В **флашкардпажер**этот `Bundle` используется для сохранения текста вопроса и ответа для связанной флэш-карты. В **FlashCardFragment.CS**добавьте в начало определения класса `FlashCardFragment` следующие ключи `Bundle`: 

```csharp
private static string FLASH_CARD_QUESTION = "card_question";
private static string FLASH_CARD_ANSWER = "card_answer";
```

Измените метод фабрики `newInstance` так, чтобы он создавал `Bundle` объект и использовал указанные выше ключи для сохранения переданного текста вопроса и ответа в фрагменте после его создания. 

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

Измените метод жизненного цикла фрагмента `OnCreateView`, чтобы получить эти сведения из переданного пакета и загрузить текст вопроса в `TextBox`: 

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

Переменная `answer` не используется здесь, но будет использоваться позже при добавлении кода обработчика событий в этот файл. 

## <a name="create-the-adapter"></a>Создание адаптера

`ViewPager` использует объект контроллера адаптера, расположенный между `ViewPager` и источником данных (см. иллюстрацию в статье ViewPager [Adapter](~/android/user-interface/controls/view-pager/index.md#adapter) ). Для доступа к этим данным `ViewPager` требует предоставления пользовательского адаптера, производного от `PagerAdapter`. Поскольку в этом примере используются фрагменты, используется `FragmentPagerAdapter` &ndash; `FragmentPagerAdapter` производным от `PagerAdapter`. 
`FragmentPagerAdapter` представляет каждую страницу как `Fragment`, которая постоянно сохраняется в диспетчере фрагментов до тех пор, пока пользователь может вернуться на страницу. По мере того, как пользователь просматривает страницы `ViewPager`, `FragmentPagerAdapter` извлекает данные из источника данных и использует их для создания `Fragment`s для `ViewPager` для просмотра. 

При реализации `FragmentPagerAdapter`необходимо переопределить следующее:

- **Count** &ndash; свойство только для чтения, которое возвращает количество доступных представлений (страниц).

- &ndash;- **элемент** Возвращает фрагмент, отображаемый для указанной страницы.

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

Этот код заглушки является основной реализацией `FragmentPagerAdapter`. В следующих разделах каждый из этих методов заменяется рабочим кодом. Назначение конструктора заключается в передаче диспетчера фрагментов конструктору базового класса `FlashCardDeckAdapter`. 

### <a name="implement-the-adapter-constructor"></a>Реализация конструктора адаптера

Когда приложение создает экземпляр `FlashCardDeckAdapter`, оно предоставляет ссылку на диспетчер фрагментов и экземпляр `FlashCardDeck`. Добавьте следующую переменную члена в начало класса `FlashCardDeckAdapter` в **FlashCardDeckAdapter.CS**: 

```csharp
public FlashCardDeck flashCardDeck;
```

Добавьте следующую строку кода в конструктор `FlashCardDeckAdapter`: 

```csharp
this.flashCardDeck = flashCards;
```

В этой строке кода хранится экземпляр `FlashCardDeck`, который будет использоваться `FlashCardDeckAdapter`. 

### <a name="implement-count"></a>Число реализаций

`Count`ная реализация относительно проста: она возвращает число видеоадаптеров в колоде флэш-карты. Замените `Count` следующим кодом: 

```csharp
public override int Count
{
    get { return flashCardDeck.NumCards; }
}
```

Свойство `NumCards` объекта `FlashCardDeck` возвращает количество карт Flash (число фрагментов) в наборе данных. 

### <a name="implement-getitem"></a>Реализация элемента Item

Метод `GetItem` Возвращает фрагмент, связанный с заданной позицией. Когда для позиции в колоде Flash-карты вызывается `GetItem`, он возвращает `FlashCardFragment`, настроенный на отображение проблемы с картой флэш-памяти в этой позиции. Замените метод `GetItem` следующим кодом: 

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

3. Вызывает метод фабрики `FlashCardFragment` `newInstance`, передавая проблемы с флэш-картой и строки ответов. 

4. Создает и возвращает новую флэш-карту `Fragment`, содержащую текст вопроса и ответа для этой должности. 

Когда `ViewPager` визуализирует `Fragment` в `position`, он отображает `TextBox`, содержащую строку математической проблемы, находящуюся в `position` в колоде Flash-карт. 

## <a name="add-the-adapter-to-the-viewpager"></a>Добавление адаптера в ViewPager

Теперь, когда `FlashCardDeckAdapter` реализована, пришло время добавить его в `ViewPager`. В **MainActivity.CS**добавьте следующую строку кода в конец метода `OnCreate`:

```csharp
FlashCardDeckAdapter adapter =
    new FlashCardDeckAdapter(SupportFragmentManager, flashCards);
viewPager.Adapter = adapter;
```

Этот код создает экземпляр `FlashCardDeckAdapter`, передавая `SupportFragmentManager` в первом аргументе. (Свойство `SupportFragmentManager` Фрагментактивити используется для получения ссылки на `FragmentManager` &ndash; дополнительные сведения о `FragmentManager`см. в разделе [Управление фрагментами](~/android/platform/fragments/managing-fragments.md).) 

Основная реализация теперь завершена &ndash; сборки и запуска приложения.
На экране появится первое изображение колоды флэш-карты, как показано на рисунке слева на следующем снимке экрана. Проведите влево, чтобы увидеть больше видеоадаптеров, а затем проведите вправо, чтобы перейти назад через колоду флэш-карты:

[![пример снимков экрана приложения Флашкардпажер без индикаторов страничного навигатора](viewpager-and-fragments-images/02-example-views-sml.png)](viewpager-and-fragments-images/02-example-views.png#lightbox)

## <a name="add-a-pager-indicator"></a>Добавление индикатора страничного навигатора

Эта минимальная реализация `ViewPager` отображает каждую флэш-карту в колоде, но не указывает, где находится пользователь в колоде. Следующим шагом является добавление `PagerTabStrip`. `PagerTabStrip` информирует пользователя о том, какой номер проблемы отображается, и предоставляет контекст навигации, отображая указание предыдущей и следующей флэш-карты. 

Откройте **ресурсы/макет/Main. axml** и добавьте в макет `PagerTabStrip`:

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

При сборке и запуске приложения вы увидите пустое `PagerTabStrip`, отображаемое в верхней части каждой флэш-карты: 

[![крупный план Пажертабстрип без текста](viewpager-and-fragments-images/03-empty-pagetabstrip-sml.png)](viewpager-and-fragments-images/03-empty-pagetabstrip.png#lightbox)

### <a name="display-a-title"></a>Отображение заголовка

Чтобы добавить заголовок на каждую вкладку страницы, реализуйте метод `GetPageTitleFormatted` в адаптере. `ViewPager` вызывает метод `GetPageTitleFormatted` (Если реализован), чтобы получить строку заголовка, описывающую страницу в указанной позиции. Добавьте следующий метод в класс `FlashCardDeckAdapter` в **FlashCardDeckAdapter.CS**: 

```csharp
public override Java.Lang.ICharSequence GetPageTitleFormatted(int position)
{
    return new Java.Lang.String("Problem " + (position + 1));
}
```

Этот код преобразует расположение в колоде флэш-карты в номер проблемы. Полученная строка преобразуется в `String` Java, которая возвращается в `ViewPager`. При запуске приложения с помощью этого нового метода на каждой странице отображается номер проблемы в `PagerTabStrip`: 

[![снимки экрана Флашкардпажер с номером проблемы, отображаемым над каждой страницей](viewpager-and-fragments-images/04-pagetabstrip-sml.png)](viewpager-and-fragments-images/04-pagetabstrip.png#lightbox)

Вы можете прокрутить экран, чтобы увидеть номер проблемы в колоде Flash-карт, который отображается в верхней части каждой флэш-карты. 

## <a name="handle-user-input"></a>Обработку входных данных пользователя

**Флашкардпажер** представляет серию видеоадаптеров, основанных на фрагментах, в `ViewPager`, но пока нет способа обнаружить ответ для каждой проблемы. В этом разделе к `FlashCardFragment`у добавляется обработчик событий, который отображает ответ, когда пользователь отменяет текст проблемы с флэш-картой. 

Откройте **FlashCardFragment.CS** и добавьте следующий код в конец метода `OnCreateView` непосредственно перед возвратом представления вызывающему объекту: 

```csharp
questionBox.Click += delegate
{
    Toast.MakeText(Activity.ApplicationContext,
            "Answer: " + answer, ToastLength.Short).Show();
};
```

Этот `Click` обработчик событий отображает ответ в виде всплывающего уведомления, которое появляется, когда пользователь касается `TextBox`. Переменная `answer` была инициализирована ранее, когда сведения о состоянии были считаны из пакета, переданного в `OnCreateView`. Выполните сборку и запуск приложения, а затем коснитесь текста проблемы на каждой флэш-карте, чтобы увидеть ответ: 

[![снимков экрана с всплывающими уведомлениями о приложении Флашкардпажер при касании математической задачи](viewpager-and-fragments-images/05-answer-sml.png)](viewpager-and-fragments-images/05-answer.png#lightbox)

**Флашкардпажер** , представленный в этом пошаговом руководстве, использует `MainActivity`, производный от `FragmentActivity`, но можно также наследовать `MainActivity` от `AppCompatActivity` (что также обеспечивает поддержку управления фрагментами). Пример `AppCompatActivity` см. в разделе [флашкардпажер](https://docs.microsoft.com/samples/xamarin/monodroid-samples/userinterface-flashcardpager) в коллекции примеров.

## <a name="summary"></a>Сводка

В этом пошаговом руководстве представлен пошаговый пример создания базового приложения на основе `ViewPager`с помощью `Fragment`s. В нем представлен пример источника данных, содержащего вопросы и ответы по Flash Card, `ViewPager`ный макет для отображения видеоадаптеров, а также подкласс `FragmentPagerAdapter`, соединяющий `ViewPager` с источником данных. Чтобы помочь пользователю перемещаться по картам флэш-памяти, были добавлены инструкции, объясняющие, как добавить `PagerTabStrip` для отображения номера проблемы в верхней части каждой страницы. Наконец, добавлен код обработки событий для вывода ответа, когда пользователь касается проблемы с флэш-картой. 

## <a name="related-links"></a>Связанные ссылки

- [Флашкардпажер (пример)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/userinterface-flashcardpager)
