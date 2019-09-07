---
title: Фрагменты Пошаговое руководство. Часть 2
ms.prod: xamarin
ms.topic: tutorial
ms.assetid: 444A894D-5197-4726-934F-79BA80A71CB0
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/26/2018
ms.openlocfilehash: 0363213d76d9a67b559614741edf37d296848075
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2019
ms.locfileid: "70761658"
---
# <a name="fragments-walkthrough-ndash-landscape"></a>Пошаговое руководство &ndash; . альбомная ориентация

В [пошаговом руководстве &ndash; «фрагменты» в разделе 1](./walkthrough.md) показано, как создавать и использовать фрагменты в приложении Android, предназначенном для небольших экранов на телефоне. Следующим шагом в этом пошаговом руководстве является изменение приложения, чтобы воспользоваться преимуществами дополнительного пространства на планшете &ndash; , в котором всегда будет список воспроизведения `TitlesFragment`() и `PlayQuoteFragment` будет динамически добавляться к действию в ответ на выбор, сделанный пользователем:

[![Приложение, работающее на планшете](./walkthrough-landscape-images/01-tablet-screenshot-sml.png)](./walkthrough-landscape-images/01-tablet-screenshot.png#lightbox)

На телефонах, работающих в альбомном режиме, также будет выгодно это улучшение:

[![Приложение, работающее на телефоне Android в альбомном режиме](./images/intro-screenshot-phone-land-sml.png)](./images/intro-screenshot-phone-land.png#lightbox)

## <a name="updating-the-app-to-handle-landscape-orientation"></a>Идет обновление приложения для работы с альбомной ориентацией

Следующие изменения будут созданы на основе работы, выполненной в [пошаговом руководстве по телефону](./walkthrough.md) .

1. Создайте альтернативный макет для вывода `TitlesFragment` и. `PlayQuoteFragment`
1. Обновите `TitlesFragment` , чтобы определить, одновременно ли на устройстве отображаются оба фрагмента, и измените поведение соответствующим образом.
1. Обновите `PlayQuoteActivity` , чтобы закрыть, когда устройство находится в альбомном режиме.

## <a name="1-create-an-alternate-layout"></a>1. Создание альтернативного макета

Когда основное действие создается на устройстве Android, Android выбирает макет для загрузки в зависимости от ориентации устройства. По умолчанию Android предоставит файл макета **Resources/Layout/activity_main. axml** . Для устройств, загружаемых в альбомном режиме Android, будет предоставлен файл макета **Resources/лайаут-Ланд/activity_main. axml** . Руководство по [ресурсам Android](/xamarin/android/app-fundamentals/resources-in-android) содержит дополнительные сведения о том, как Android решает, какие файлы ресурсов загружать для приложения.

Создайте альтернативный макет, ориентированный на **альбомную** ориентацию, выполнив действия, описанные в разделе [Альтернативное руководство по разметке](/xamarin/android/user-interface/android-designer/alternative-layout-views) . При этом необходимо добавить новый файл ресурсов макета в проект, **Resources/Layout/activity_main. axml**:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Альтернативный макет в обозреватель решений](./walkthrough-landscape-images/02-alternate-layout.w157-sml.png)](./walkthrough-landscape-images/02-alternate-layout.w157.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio для Mac](#tab/macos)

[![Альтернативный макет в Панель решения](./walkthrough-landscape-images/02-alternate-layout.m743-sml.png)](./walkthrough-landscape-images/02-alternate-layout.m743.png#lightbox)

-----

После создания альтернативного макета измените источник файла **Resources/лайаут-Ланд/activity_main. axml** , чтобы он совпадал с этим XML-кодом:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/two_fragments_layout"
    android:orientation="horizontal"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <fragment android:name="FragmentSample.TitlesFragment"
        android:id="@+id/titles"
        android:layout_weight="1"
        android:layout_width="0px"
        android:layout_height="match_parent" />

    <FrameLayout android:id="@+id/playquote_container"
            android:layout_weight="1"
            android:layout_width="0px"
            android:layout_height="match_parent"
             />
</LinearLayout>
```

Корневое представление действия получает идентификатор `two_fragments_layout` ресурса и имеет два подпредставления `fragment` , а и `FrameLayout`. Хотя статически загружены, объект выступает в `FrameLayout` качестве заполнителя, который будет заменен во время `PlayQuoteFragment`выполнения. `fragment` Каждый раз при выборе нового воспроизведения в `TitlesFragment` `playquote_container` будет обновлен новый экземпляр `PlayQuoteFragment`.

Каждое из подчиненных представлений будет занимать полную высоту их родителя. Ширина каждого подпредставления определяется `android:layout_weight` атрибутами и. `android:layout_width` В этом примере каждое Подпредставление будет занимать 50% ширины, предоставляемой родителем. Дополнительные сведения о _весе макета_см. [в документе Google на элемент LinearLayout](https://developer.android.com/guide/topics/ui/layout/linear.html) .

## <a name="2-changes-to-titlesfragment"></a>2. Изменения в Титлесфрагмент

После создания альтернативного макета необходимо обновить `TitlesFragment`. Когда приложение отображает два фрагмента одного действия, `TitlesFragment` следует `PlayQuoteFragment` загрузить в родительское действие. В противном случае `PlayQuoteActivity` `PlayQuoteFragment`должен запустить узел, на котором размещается. `TitlesFragment` Логический флаг поможет `TitlesFragment` определить, какое поведение следует использовать. Этот флаг будет инициализирован в `OnActivityCreated` методе.

Сначала добавьте переменную экземпляра в начало `TitlesFragment` класса:

```csharp
bool showingTwoFragments;
```

Затем добавьте следующий фрагмент кода для `OnActivityCreated` инициализации переменной: 

```csharp
var quoteContainer = Activity.FindViewById(Resource.Id.playquote_container);
showingTwoFragments = quoteContainer != null &&
                      quoteContainer.Visibility == ViewStates.Visible;
if (showingTwoFragments)
{
    ListView.ChoiceMode = ChoiceMode.Single;
    ShowPlayQuote(selectedPlayId);
}
```

Если устройство `FrameLayout` работает в альбомном режиме, на экране будет отображаться идентификатор `playquote_container` ресурса, поэтому `showingTwoFragments` он будет инициализирован в `true`. Если устройство работает в книжной ориентации, `playquote_container` оно не будет отображаться на экране, поэтому `showingTwoFragments` будет `false`иметь значение.

Метод должен изменить способ отображения квоты &ndash; в фрагменте или запуске нового действия. `ShowPlayQuote`  Обновите `ShowPlayQuote` метод, чтобы загрузить фрагмент при отображении двух фрагментов, иначе он должен запустить действие:

```csharp
void ShowPlayQuote(int playId)
{
    selectedPlayId = playId;
    if (showingTwoFragments)
    {
        ListView.SetItemChecked(selectedPlayId, true);

        var playQuoteFragment = FragmentManager.FindFragmentById(Resource.Id.playquote_container) as PlayQuoteFragment;

        if (playQuoteFragment == null || playQuoteFragment.PlayId != playId)
        {
            var container = Activity.FindViewById(Resource.Id.playquote_container);
            var quoteFrag = PlayQuoteFragment.NewInstance(selectedPlayId);

            FragmentTransaction ft = FragmentManager.BeginTransaction();
            ft.Replace(Resource.Id.playquote_container, quoteFrag);
            ft.Commit();
        }
    }
    else
    {
        var intent = new Intent(Activity, typeof(PlayQuoteActivity));
        intent.PutExtra("current_play_id", playId);
        StartActivity(intent);
    }
}
```

Если пользователь выбрал воспроизведение, отличное от того, в котором в данный момент отображается `PlayQuoteFragment`в, то создается новый `PlayQuoteFragment` объект, который `playquote_container` заменит содержимое в контексте `FragmentTransaction`.

### <a name="complete-code-for-titlesfragment"></a>Полный код для Титлесфрагмент

После завершения всех предыдущих изменений в `TitlesFragment`полный класс должен соответствовать этому коду:

```csharp
public class TitlesFragment : ListFragment
{
    int selectedPlayId;
    bool showingTwoFragments;

    public override void OnActivityCreated(Bundle savedInstanceState)
    {
        base.OnActivityCreated(savedInstanceState);
        ListAdapter = new ArrayAdapter<string>(Activity, Android.Resource.Layout.SimpleListItemActivated1, Shakespeare.Titles);

        if (savedInstanceState != null)
        {
            selectedPlayId = savedInstanceState.GetInt("current_play_id", 0);
        }

        var quoteContainer = Activity.FindViewById(Resource.Id.playquote_container);
        showingTwoFragments = quoteContainer != null &&
                                quoteContainer.Visibility == ViewStates.Visible;
        if (showingTwoFragments)
        {
            ListView.ChoiceMode = ChoiceMode.Single;
            ShowPlayQuote(selectedPlayId);
        }
    }

    public override void OnSaveInstanceState(Bundle outState)
    {
        base.OnSaveInstanceState(outState);
        outState.PutInt("current_play_id", selectedPlayId);
    }

    public override void OnListItemClick(ListView l, View v, int position, long id)
    {
        ShowPlayQuote(position);
    }

    void ShowPlayQuote(int playId)
    {
        selectedPlayId = playId;
        if (showingTwoFragments)
        {
            ListView.SetItemChecked(selectedPlayId, true);

            var playQuoteFragment = FragmentManager.FindFragmentById(Resource.Id.playquote_container) as PlayQuoteFragment;

            if (playQuoteFragment == null || playQuoteFragment.PlayId != playId)
            {
                var container = Activity.FindViewById(Resource.Id.playquote_container);
                var quoteFrag = PlayQuoteFragment.NewInstance(selectedPlayId);

                FragmentTransaction ft = FragmentManager.BeginTransaction();
                ft.Replace(Resource.Id.playquote_container, quoteFrag);
                ft.AddToBackStack(null);
                ft.SetTransition(FragmentTransit.FragmentFade);
                ft.Commit();
            }
        }
        else
        {
            var intent = new Intent(Activity, typeof(PlayQuoteActivity));
            intent.PutExtra("current_play_id", playId);
            StartActivity(intent);
        }
    }
}
```

## <a name="3-changes-to-playquoteactivity"></a>3. Изменения в Плайкуотеактивити

Есть одна последняя информация, которую следует предпринять: `PlayQuoteActivity` не требуется, если устройство находится в альбомном режиме. Если устройство находится в альбомном режиме, `PlayQuoteActivity` оно не должно быть видимым. Обновите `OnCreate` `PlayQuoteActivity` метод таким образом, чтобы он закрыл себя. Этот код является окончательной версией `PlayQuoteActivity.OnCreate`:

```csharp
protected override void OnCreate(Bundle savedInstanceState)
{
    base.OnCreate(savedInstanceState);

    if (Resources.Configuration.Orientation == Android.Content.Res.Orientation.Landscape)
    {
        Finish();
    }

    var playId = Intent.Extras.GetInt("current_play_id", 0);
    var playQuoteFrag = PlayQuoteFragment.NewInstance(playId);
    FragmentManager.BeginTransaction()
                    .Add(Android.Resource.Id.Content, playQuoteFrag)
                    .Commit();
}
```

Это изменение добавляет проверку ориентации устройства. Если он находится в альбомном режиме, `PlayQuoteActivity` он закроется.

## <a name="4-run-the-application"></a>4. Запуск приложения

После завершения этих изменений запустите приложение, переворачивайте устройство в альбомный режим (при необходимости), а затем выберите воспроизведение. Цитата должна отображаться на том же экране, что и список воспроизведения:

[![Приложение, работающее на телефоне Android в альбомном режиме](./images/intro-screenshot-phone-land-sml.png)](./images/intro-screenshot-phone-land.png#lightbox)

[![Приложение, работающее на планшете Android](./images/intro-screenshot-tablet-sml.png)](./images/intro-screenshot-tablet.png#lightbox)
