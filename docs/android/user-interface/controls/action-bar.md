---
title: Актионбар для Xamarin. Android
ms.prod: xamarin
ms.assetid: 84A79F1F-9E73-4E3E-80FA-B72E5686900B
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: 4a0d0e46147a37da4787224e797d403ab7b1097e
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/30/2019
ms.locfileid: "68643039"
---
# <a name="actionbar-for-xamarinandroid"></a>Актионбар для Xamarin. Android

При использовании `TabActivity`код для создания значков вкладок не влияет на работу с платформой Android 4,0. Хотя функционально работает так же, как в версиях Android до 2,3, `TabActivity` сам класс был признан устаревшим в 4,0. Появился новый способ создания интерфейса с вкладками, использующий панель действий, которую мы обсудим далее.


## <a name="action-bar-tabs"></a>Вкладки панели действий

Панель действий включает поддержку добавления интерфейсов с вкладками в Android 4,0.
На следующем снимке экрана показан пример такого интерфейса.

[![Снимок экрана приложения, работающего в эмуляторе; показаны две вкладки](action-bar-images/25-actionbartabs.png)](action-bar-images/25-actionbartabs.png#lightbox)

Чтобы создать вкладки на панели действий, сначала необходимо установить `NavigationMode` свойство для поддержки вкладок. В Android 4 `ActionBar` свойство доступно в классе Activity, который можно использовать для `NavigationMode` установки следующего вида:

```csharp
this.ActionBar.NavigationMode = ActionBarNavigationMode.Tabs;
```

После этого можно создать вкладку, вызвав `NewTab` метод на панели действий. С помощью этого экземпляра вкладки можно вызвать `SetText` методы и `SetIcon` для задания текста и значка метки вкладки; эти вызовы выполняются по порядку в коде, показанном ниже:

```csharp
var tab = this.ActionBar.NewTab ();
tab.SetText (tabText);
tab.SetIcon (Resource.Drawable.ic_tab_white);
```

Однако, прежде чем можно будет добавить вкладку, необходимо выполнить обработку `TabSelected` события. В этом обработчике мы можем создать содержимое для вкладки. Вкладки панели действий предназначены для работы с *фрагментами*, которые представляют собой классы, представляющие часть пользовательского интерфейса в действии. В этом примере представление фрагмента содержит единственный `TextView`объект, который мы делаем в нашем `Fragment` подклассе следующим образом:

```csharp
class SampleTabFragment: Fragment
{           
    public override View OnCreateView (LayoutInflater inflater,
        ViewGroup container, Bundle savedInstanceState)
    {
        base.OnCreateView (inflater, container, savedInstanceState);

        var view = inflater.Inflate (
            Resource.Layout.Tab, container, false);

        var sampleTextView =
            view.FindViewById<TextView> (Resource.Id.sampleTextView);            
        sampleTextView.Text = "sample fragment text";


        return view;
    }
}
```

Аргумент события, переданный в `TabSelected` событии, имеет `TabEventArgs`тип, который включает `FragmentTransaction` свойство, которое можно использовать для добавления фрагмента, как показано ниже:

```csharp
tab.TabSelected += delegate(object sender, ActionBar.TabEventArgs e) {             
    e.FragmentTransaction.Add (Resource.Id.fragmentContainer,
        new SampleTabFragment ());
};
```

Наконец, можно добавить вкладку на панель действий, вызвав `AddTab` метод, как показано в следующем коде:

```csharp
this.ActionBar.AddTab (tab);
```

Полный пример см. в описании проекта *хеллотабсикс* в образце кода для этого документа.


## <a name="shareactionprovider"></a>шареактионпровидер

`ShareActionProvider` Класс позволяет выполнить действие совместного доступа из панели действий. Он создает представление действий со списком приложений, которые могут справиться с намерением совместного использования, и сохраняет историю ранее использовавшихся приложений для быстрого доступа к ним в дальнейшем с панели действий. Это позволяет приложениям обмениваться данными с помощью взаимодействия с пользователем, которые согласованы на всем устройстве Android.


### <a name="image-sharing-example"></a>Пример общего доступа к изображению

Например, ниже приведен снимок экрана панели действий с пунктом меню для совместного использования изображения (взято из примера [шареактионпровидер](https://docs.microsoft.com/samples/xamarin/monodroid-samples/shareactionproviderdemo) ). Когда пользователь отменяет пункт меню на панели действий, Шареактионпровидер загружает приложение для решения намерений, связанных с `ShareActionProvider`. В этом примере приложение для обмена сообщениями использовалось ранее, поэтому оно отображается на панели действий.

[![Снимок экрана: значок приложения обмена сообщениями на панели действий](action-bar-images/09-shareactionprovider.png)](action-bar-images/09-shareactionprovider.png#lightbox)


Когда пользователь щелкает элемент на панели действий, запускается приложение для обмена сообщениями, содержащее общий образ, как показано ниже:

[![Снимок экрана: приложение для обмена сообщениями, отображающее изображение обезьяны](action-bar-images/10-messagewithimage.png)](action-bar-images/10-messagewithimage.png#lightbox)


### <a name="specifying-the-action-provider-class"></a>Указание класса поставщика действий

Чтобы использовать `ShareActionProvider`, `android:actionProviderClass` установите атрибут для пункта меню в XML-коде для меню панели действий следующим образом:

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
  <item android:id="@+id/shareMenuItem"
      android:showAsAction="always"
      android:title="@string/sharePicture"
      android:actionProviderClass="android.widget.ShareActionProvider" />
</menu>
```


### <a name="inflating-the-menu"></a>При увеличении меню

Чтобы увеличить это меню, мы переопределяем `OnCreateOptionsMenu` в подклассе действия. После получения ссылки на меню можно получить `ShareActionProvider` `ActionProvider` из свойства элемента меню, а затем использовать метод `ShareActionProvider`сетшареинтент для установки цели, как показано ниже:

```csharp
public override bool OnCreateOptionsMenu (IMenu menu)
{
    MenuInflater.Inflate (Resource.Menu.ActionBarMenu, menu);       

    var shareMenuItem = menu.FindItem (Resource.Id.shareMenuItem);           
    var shareActionProvider =
       (ShareActionProvider)shareMenuItem.ActionProvider;
    shareActionProvider.SetShareIntent (CreateIntent ());
}
```


### <a name="creating-the-intent"></a>Создание цели

Будет использовать намерение, передаваемое `SetShareIntent` методу в приведенном выше коде, для запуска соответствующего действия. `ShareActionProvider` В этом случае мы создаем намерение отправить изображение с помощью следующего кода:

```csharp
Intent CreateIntent ()
{  
    var sendPictureIntent = new Intent (Intent.ActionSend);
    sendPictureIntent.SetType ("image/*");
    var uri = Android.Net.Uri.FromFile (GetFileStreamPath ("monkey.png"));          
    sendPictureIntent.PutExtra (Intent.ExtraStream, uri);
    return sendPictureIntent;
}
```

Изображение в приведенном выше примере кода включается в качестве ресурса в приложение и копируется в общедоступное расположение при создании действия, поэтому оно будет доступно другим приложениям, таким как приложение для обмена сообщениями. Пример кода, прилагаемого к этой статье, содержит полный исходный код этого примера, иллюстрирующий его использование.



## <a name="related-links"></a>Связанные ссылки

- [Общий доступ к вкладкам Hello (пример)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/hellotabsics)
- [Демонстрация Шареактионпровидер (пример)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/shareactionproviderdemo)
- [Знакомство со Южным Сандвичевым](http://www.android.com/about/ice-cream-sandwich/)
- [Платформа Android 4,0](https://developer.android.com/sdk/android-4.0.html)
