---
title: Элементы управления Android (мини-приложения)
description: Стандартные блоки для создания пользовательских интерфейсов Xamarin. Android
ms.prod: xamarin
ms.assetid: B7A82166-B920-4672-B7A2-20DD5E0B5AEF
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 08/29/2018
ms.openlocfilehash: 25e33ee2d77501b3913a3598ef7855714f0b9bc1
ms.sourcegitcommit: 52fb214c0e0243587d4e9ad9306b75e92a8cc8b7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/01/2020
ms.locfileid: "76940899"
---
# <a name="xamarinandroid-controls-widgets"></a>Элементы управления Xamarin. Android (мини-приложения)

Xamarin. Android предоставляет все собственные элементы управления пользовательского интерфейса (мини-приложения), предоставляемые Android. Эти элементы управления можно легко добавить в приложения Xamarin. Android, используя Android Designer или программно с помощью XML-файлов макета. Независимо от выбранного метода Xamarin. Android предоставляет все свойства и методы объекта пользовательского интерфейса в C#. В следующих разделах представлены наиболее распространенные элементы управления пользовательского интерфейса Android и объясняется, как их внедрять в приложения Xamarin. Android.

## <a name="action-barandroiduser-interfacecontrolsaction-barmd"></a>[Панель действий](~/android/user-interface/controls/action-bar.md) 

`ActionBar` — это панель инструментов, на которой отображаются заголовок действия, интерфейсы навигации и другие интерактивные элементы. Как правило, панель действий отображается в верхней части окна действия.

![Пример Актионбар](images/action-bar.png)

## <a name="auto-completeandroiduser-interfacecontrolsauto-completemd"></a>[Автозаполнение](~/android/user-interface/controls/auto-complete.md)

`AutoCompleteTextView` является редактируемым элементом текста представления, который автоматически показывает варианты завершения при вводе пользователем. Список предложений отображается в раскрывающемся меню, из которого пользователь может выбрать элемент для замены содержимого поля ввода на.

![Пример автоматического завершения](images/auto-complete.png)

## <a name="buttonsandroiduser-interfacecontrolsbuttonsindexmd"></a>[Кнопки](~/android/user-interface/controls/buttons/index.md)

Кнопки — это элементы пользовательского интерфейса, которые пользователь откасается для выполнения действия.

![Примеры кнопок](images/buttons.png)

## <a name="calendarandroiduser-interfacecontrolscalendarmd"></a>[Calendar](~/android/user-interface/controls/calendar.md)

Класс `Calendar` используется для преобразования определенного экземпляра во времени (значение миллисекунд, которое смещается от эпохи) к значениям, например year, month, Hour, Day месяца, и Дата следующей недели.
`Calendar` поддерживает множество вариантов взаимодействия с данными календаря, включая возможность чтения и записи событий, участников и напоминаний. Используя поставщик календаря в приложении, данные, добавляемые через API, будут отображаться в встроенном приложении календаря, поставляемом с Android.

![Пример календаря](images/calendar.png)

## <a name="cardviewandroiduser-interfacecontrolscard-viewmd"></a>[CardView](~/android/user-interface/controls/card-view.md)

`CardView` — это компонент пользовательского интерфейса, который представляет содержимое текста и изображения в представлениях, напоминающих карты. `CardView` реализуется как мини-приложение `FrameLayout` со скругленными углами и тенью. Как правило, `CardView` используется для представления одного элемента строки в группе представлений `ListView` или `GridView`.

![Пример представления карточки](images/cardview.png)

## <a name="edit-textandroiduser-interfacecontrolsedit-textmd"></a>[Изменить текст](~/android/user-interface/controls/edit-text.md)

`EditText` — это элемент пользовательского интерфейса, который используется для ввода и изменения текста.

![Пример правки текста](images/edit-text.png)

## <a name="galleryandroiduser-interfacecontrolsgallerymd"></a>[Коллекция](~/android/user-interface/controls/gallery.md)

`Gallery` — это мини-приложение макета, которое используется для отображения элементов в списке с горизонтальной прокруткой; Он позиционирует текущее выделение в центре представления.

![Галерея примеров](images/gallery.png)

## <a name="navigation-barandroiduser-interfacecontrolsnavigation-barmd"></a>[Панель навигации](~/android/user-interface/controls/navigation-bar.md)

*Панель навигации* предоставляет элементы управления навигацией на устройствах, которые не включают аппаратные кнопки для **домашних**, **задних**и **меню**.

![Пример панели навигации](images/navigation-bar.png)

## <a name="pickersandroiduser-interfacecontrolspickersindexmd"></a>[Средства выбора](~/android/user-interface/controls/pickers/index.md)

Средства *выбора* — это элементы пользовательского интерфейса, которые позволяют пользователю выбрать дату или время с помощью диалоговых окон, предоставляемых Android.

![Пример средства выбора](images/picker.png)

## <a name="popup-menuandroiduser-interfacecontrolspopup-menumd"></a>[Всплывающее меню](~/android/user-interface/controls/popup-menu.md)

`PopupMenu` используется для отображения всплывающих меню, присоединенных к определенному представлению.

![Пример всплывающего меню](images/popup-menu.png)

## <a name="ratingbarandroiduser-interfacecontrolsratingbarmd"></a>[ратингбар](~/android/user-interface/controls/ratingbar.md)

`RatingBar` — это элемент пользовательского интерфейса, который отображает рейтинг в звездах.

![Пример Ратингбар](ratingbar-images/01-ratingbar.png)

## <a name="spinnerandroiduser-interfacecontrolsspinnermd"></a>[Вертушка](~/android/user-interface/controls/spinner.md)

`Spinner` — это элемент пользовательского интерфейса, предоставляющий быстрый способ выбора одного значения из набора. Он похож на раскрывающийся список. 

![Пример счетчика](images/spinner.png)

## <a name="switchandroiduser-interfacecontrolsswitchmd"></a>[Коммутатор](~/android/user-interface/controls/switch.md)

`Switch` — это элемент пользовательского интерфейса, который позволяет пользователю переключаться между двумя состояниями, например включать или выключать. Значение `Switch` по умолчанию — OFF.

![Пример переключения](images/switch.png)

## <a name="textureviewandroiduser-interfacecontrolstexture-viewmd"></a>[TextureView](~/android/user-interface/controls/texture-view.md)

`TextureView` — это представление, которое использует двухмерную отрисовку с аппаратным ускорением для отображения потока содержимого видео или OpenGL.

![Пример представления текстуры](images/texture-view.png)

## <a name="toolbarandroiduser-interfacecontrolstool-barindexmd"></a>[ToolBar](~/android/user-interface/controls/tool-bar/index.md)

Мини-приложение `Toolbar` (введенное в ОС Android 5,0 без описания операций) можно рассматривать как обобщение интерфейса панели действий &ndash; оно предназначено для замены панели действий. `Toolbar` можно использовать в любом месте макета приложения, и это гораздо более настраиваемая, чем панель действий.

![Пример панели инструментов](images/toolbar.png)

## <a name="viewpagerandroiduser-interfacecontrolsview-pagerindexmd"></a>[ViewPager](~/android/user-interface/controls/view-pager/index.md) 

`ViewPager` — это Диспетчер макетов, позволяющий пользователю отражать левые и правые страницы данных.

![Пример ViewPager](images/viewpager.png)

## <a name="webviewandroiduser-interfacecontrolsweb-viewmd"></a>[WebView](~/android/user-interface/controls/web-view.md)

`WebView` — это элемент пользовательского интерфейса, который позволяет создать собственное окно для просмотра веб-страниц (или даже разработать полный браузер).

![Пример веб-представления](images/web-view.png)
