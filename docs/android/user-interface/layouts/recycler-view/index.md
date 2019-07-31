---
title: RecyclerView
description: RecyclerView — это группа представлений для отображения коллекций; Она обеспечивает более гибкую замену для старых групп представлений, таких как ListView и GridView.  В этом руководство объясняется, как использовать и настраивать RecyclerView в приложениях Xamarin. Android.
ms.prod: xamarin
ms.assetid: 91EF0BD2-3306-47E1-9B39-627A1787762F
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 01/03/2018
ms.openlocfilehash: 95d71beff2bd5219712494deb43f1f9fb4b082ec
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/30/2019
ms.locfileid: "68646308"
---
# <a name="recyclerview"></a>RecyclerView

_RecyclerView — это группа представлений для отображения коллекций; Она обеспечивает более гибкую замену для старых групп представлений, таких как ListView и GridView.  В этом руководство объясняется, как использовать и настраивать RecyclerView в приложениях Xamarin. Android._

## <a name="recyclerview"></a>RecyclerView

Многие приложения должны отображать коллекции одного типа (например, сообщения, контакты, изображения или песни). часто эта коллекция слишком велика, чтобы уместиться на экране, поэтому коллекция представлена в маленьком окне, позволяющем плавно прокручивать все элементы коллекции.
`RecyclerView`— Это мини-приложение Android, которое отображает коллекцию элементов в списке или сетке, позволяя пользователю прокручивать коллекцию. Ниже приведен снимок экрана примера приложения, которое использует `RecyclerView` для отображения содержимого папки "Входящие" электронной почты в вертикальном списке прокрутки:

[![Пример приложения, использующего RecyclerView для вывода списка входящих сообщений](images/01-recyclerview-example-sml.png)](images/01-recyclerview-example.png#lightbox)

`RecyclerView`предлагает две интересные функции:

-  Она имеет гибкую архитектуру, позволяющую изменять ее поведение путем подключения предпочитаемых компонентов.

-  Она работает эффективно с большими коллекциями, так как она повторно использует представления элементов и требует использования *владельцев представления* для ссылок на представления кэша.

В этом руководство объясняется, `RecyclerView` как использовать в приложениях Xamarin. Android. в нем объясняется `RecyclerView` `RecyclerView` , как добавить пакет в проект Xamarin. Android, и в нем описываются функции типичного приложения. Реальные примеры кода содержат сведения о том, как интегрироваться `RecyclerView` в приложение, как реализовать нажатие кнопки "Просмотр элементов" и как обновляться `RecyclerView` при изменении базовых данных. В этом учебнике предполагается, что вы знакомы с разработкой Xamarin. Android.


### <a name="requirements"></a>Требования

Несмотря `RecyclerView` на то, что часто связывается с интерфейсом загрузки Android 5,0, оно &ndash; предоставляется в виде библиотеки `RecyclerView` поддержки для приложений, предназначенных для API уровня 7 (Android 2,1) и более поздних версий. Для использования `RecyclerView` в приложениях на основе Xamarin необходимо следующее:

-  **Xamarin. Android** &ndash; Xamarin. Android 4,20 или более поздней версии должен быть установлен и настроен с помощью Visual Studio или Visual Studio для Mac.

-  Проект приложения должен содержать пакет **Xamarin. Android. support. версии 7. RecyclerView** . Дополнительные сведения об установке пакетов NuGet см. в [разделе Пошаговое руководство. Включение NuGet в проект](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough).


### <a name="overview"></a>Обзор

`RecyclerView`можно рассматривать как замену для `ListView` мини-приложений и `GridView` в Android. Как и его предшественники `RecyclerView` , предназначен для отображения большого набора данных в маленьком окне, но `RecyclerView` предлагает больше вариантов макета и лучше оптимизирован для отображения больших коллекций. Если вы знакомы с `ListView`, существует несколько важных различий между `ListView` и `RecyclerView`:

-   `RecyclerView`немного сложнее в использовании: необходимо написать дополнительный код для использования `RecyclerView` по сравнению с. `ListView`

-   `RecyclerView`не предоставляет предопределенный адаптер; необходимо реализовать код адаптера, который обращается к источнику данных. Однако Android включает несколько стандартных адаптеров, которые работают `ListView` с `GridView`и.

-   `RecyclerView`не предлагает событие щелчка элемента, когда пользователь касается элемента. Вместо этого события щелчка элемента обрабатываются вспомогательными классами. В отличие от `ListView` этого, предлагает событие щелчка элемента.

-   `RecyclerView`повышает производительность, перезапуская представления и применяя шаблон представления, который устраняет ненужные Просмотры ресурсов макета. Использование шаблона представления-заполнителя является необязательным в `ListView`.

-   `RecyclerView`основан на модульной конструкции, которая упрощает настройку. Например, можно подключить другую политику макета без значительных изменений кода в приложении.
    Напротив, `ListView` является относительно монолитным в структуре.

-   `RecyclerView`включает встроенные анимации для элемента добавить и удалить. `ListView`Анимация требует некоторых дополнительных усилий в части разработчика приложения.


### <a name="sections"></a>Разделы

#### <a name="recyclerview-parts-and-functionalityandroiduser-interfacelayoutsrecycler-viewparts-and-functionalitymd"></a>[RecyclerView части и функциональные возможности](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md)

В этом разделе объясняется `Adapter`, `LayoutManager`как, `ViewHolder` и работают вместе как вспомогательные классы для `RecyclerView`поддержки.
Он предоставляет общий обзор каждого из этих вспомогательных классов и объясняет, как использовать их в приложении.

#### <a name="a-basic-recyclerview-exampleandroiduser-interfacelayoutsrecycler-viewrecyclerview-examplemd"></a>[Пример базового RecyclerView](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)

Этот раздел основан на информации, представленной в [RecyclerView частях и функциональных](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md) возможностях, предоставляя реальные `RecyclerView` примеры кода реализации различных элементов для создания приложения для работы с фотографиями в реальном мире.

#### <a name="extending-the-recyclerview-exampleandroiduser-interfacelayoutsrecycler-viewextending-the-examplemd"></a>[Расширение примера RecyclerView](~/android/user-interface/layouts/recycler-view/extending-the-example.md)

В этом разделе добавляется дополнительный код для примера приложения, представленного в [базовом примере RecyclerView](~/android/user-interface/layouts/recycler-view/recyclerview-example.md) , чтобы продемонстрировать, как обрабатывает события щелчка элемента и обновляются `RecyclerView` при изменении базового источника данных.


### <a name="summary"></a>Сводка

В этом руководство было представлено мини-приложение Android `RecyclerView` . в нем было объяснено, как `RecyclerView` добавить библиотеку поддержки в `RecyclerView` проекты Xamarin. Android, как перезапускать представления, как она применяет шаблон представления для повышения эффективности и как различные вспомогательные классы, которые составляют `RecyclerView` совместную работу для вывода коллекций. В нем представлен пример кода для демонстрации `RecyclerView` того, как интегрирована в приложение, было объяснено `RecyclerView`, как настроить политику макета в соответствии с разделом в различных диспетчерах макета, и было описано, как управлять событиями щелчка элемента и уведомлять `RecyclerView`изменений в источнике данных.

Дополнительные сведения о `RecyclerView`см. в справочнике по [классам RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html).


## <a name="related-links"></a>Связанные ссылки

- [Рециклервиевер (пример)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-recyclerviewer)
- [Введение в интерфейс без описания операций](~/android/platform/lollipop.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
