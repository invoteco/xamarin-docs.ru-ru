---
title: RecyclerView
description: RecyclerView — это группа представлений для отображения коллекций; Она обеспечивает более гибкую замену для старых групп представлений, таких как ListView и GridView.  В этом руководство объясняется, как использовать и настраивать RecyclerView в приложениях Xamarin. Android.
ms.prod: xamarin
ms.assetid: 91EF0BD2-3306-47E1-9B39-627A1787762F
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 01/03/2018
ms.openlocfilehash: bce89be8bec512ac70ca40015521c7d56f3460d3
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028811"
---
# <a name="recyclerview"></a>RecyclerView

_RecyclerView — это группа представлений для отображения коллекций; Она обеспечивает более гибкую замену для старых групп представлений, таких как ListView и GridView.  В этом руководство объясняется, как использовать и настраивать RecyclerView в приложениях Xamarin. Android._

## <a name="recyclerview"></a>RecyclerView

Многие приложения должны отображать коллекции одного типа (например, сообщения, контакты, изображения или песни). часто эта коллекция слишком велика, чтобы уместиться на экране, поэтому коллекция представлена в маленьком окне, позволяющем плавно прокручивать все элементы коллекции.
`RecyclerView` — это мини-приложение Android, которое отображает коллекцию элементов в списке или сетке, позволяя пользователю прокручивать коллекцию. Ниже приведен снимок экрана примера приложения, которое использует `RecyclerView` для отображения содержимого папки "Входящие" электронной почты в прокручиваемом вертикальном списке:

[![пример приложения с использованием RecyclerView для вывода списка сообщений в папке "Входящие"](images/01-recyclerview-example-sml.png)](images/01-recyclerview-example.png#lightbox)

`RecyclerView` предлагает две интересные функции:

- Она имеет гибкую архитектуру, позволяющую изменять ее поведение путем подключения предпочитаемых компонентов.

- Она работает эффективно с большими коллекциями, так как она повторно использует представления элементов и требует использования *владельцев представления* для ссылок на представления кэша.

В этом руководство объясняется, как использовать `RecyclerView` в приложениях Xamarin. Android. в нем объясняется, как добавить пакет `RecyclerView` в проект Xamarin. Android, и в нем описано, как `RecyclerView` функции в типичном приложении. Реальные примеры кода содержат сведения о том, как интегрировать `RecyclerView` в приложение, как реализовать нажатие кнопки "Просмотр элементов" и как обновить `RecyclerView` при изменении базовых данных. В этом учебнике предполагается, что вы знакомы с разработкой Xamarin. Android.

### <a name="requirements"></a>Требования

Хотя `RecyclerView` часто связывается с интерфейсом загрузки Android 5,0, она предоставляется в виде библиотеки поддержки &ndash; `RecyclerView` работает с приложениями, предназначенными для API уровня 7 (Android 2,1) и более поздних версий. Для использования `RecyclerView` в приложениях на основе Xamarin необходимо следующее:

- **Xamarin. android** &ndash; Xamarin. Android 4,20 или более поздней версии должен быть установлен и настроен с помощью Visual Studio или Visual Studio для Mac.

- Проект приложения должен содержать пакет **Xamarin. Android. support. версии 7. RecyclerView** . Дополнительные сведения об установке пакетов NuGet см. [в разделе Пошаговое руководство. Включение NuGet в проект](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough).

### <a name="overview"></a>Обзор

`RecyclerView` можно рассматривать как замену для мини-приложений `ListView` и `GridView` в Android. Как и предшественники, `RecyclerView` предназначен для отображения большого набора данных в маленьком окне, но `RecyclerView` предлагает больше вариантов макета и лучше оптимизирован для отображения больших коллекций. Если вы знакомы с `ListView`, существует несколько важных различий между `ListView` и `RecyclerView`:

- `RecyclerView` немного сложнее в использовании: необходимо написать дополнительный код для использования `RecyclerView` по сравнению с `ListView`.

- `RecyclerView` не предоставляет предопределенный адаптер; необходимо реализовать код адаптера, который обращается к источнику данных. Однако Android включает несколько стандартных адаптеров, которые работают с `ListView` и `GridView`.

- `RecyclerView` не предлагает событие щелчка элемента, когда пользователь касается элемента; Вместо этого события щелчка элемента обрабатываются вспомогательными классами. Напротив, `ListView` предлагает событие щелчка элемента.

- `RecyclerView` повышает производительность, перезапуская представления и применяя шаблон представления, который устраняет ненужные Просмотры ресурсов макета. Использование шаблона представления-заполнителя является необязательным в `ListView`.

- `RecyclerView` основан на модульной конструкции, которая упрощает настройку. Например, можно подключить другую политику макета без значительных изменений кода в приложении.
    Напротив, `ListView` является относительно монолитной структурой.

- `RecyclerView` включает встроенные анимации для элемента добавить и удалить. для анимации `ListView` требуются некоторые дополнительные усилия в части разработчика приложения.

### <a name="sections"></a>Разделы

#### <a name="recyclerview-parts-and-functionalityandroiduser-interfacelayoutsrecycler-viewparts-and-functionalitymd"></a>[RecyclerView части и функциональные возможности](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md)

В этом разделе объясняется, как `Adapter`, `LayoutManager`и `ViewHolder` работают вместе в качестве вспомогательных классов для поддержки `RecyclerView`.
Он предоставляет общий обзор каждого из этих вспомогательных классов и объясняет, как использовать их в приложении.

#### <a name="a-basic-recyclerview-exampleandroiduser-interfacelayoutsrecycler-viewrecyclerview-examplemd"></a>[Пример базового RecyclerView](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)

В этом разделе приводятся реальные примеры кода, демонстрирующие реализацию различных элементов `RecyclerView` для создания приложения для [работы](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md) с фотографиями в реальном мире.

#### <a name="extending-the-recyclerview-exampleandroiduser-interfacelayoutsrecycler-viewextending-the-examplemd"></a>[Расширение примера RecyclerView](~/android/user-interface/layouts/recycler-view/extending-the-example.md)

В этом разделе добавляется дополнительный код в пример приложения, представленный в [базовом примере RecyclerView](~/android/user-interface/layouts/recycler-view/recyclerview-example.md) , чтобы продемонстрировать, как обрабатывает события щелчка элемента и обновляет `RecyclerView` при изменении базового источника данных.

### <a name="summary"></a>Сводка

В этом руководством появился мини-приложение `RecyclerView` Android. в нем было объяснено, как добавить библиотеку поддержки `RecyclerView` в проекты Xamarin. Android, как `RecyclerView` перезапускать представления, как он обеспечивает эффективность шаблона представления и как различные вспомогательные классы, которые составляют `RecyclerView` совместной работы для отображения коллекций. В нем приведен пример кода для демонстрации того, как `RecyclerView` интегрирована в приложение, было объяснено, как настроить политику макета `RecyclerView`, подключив различные диспетчеры макетов и описывали обработку событий щелчка элемента и уведомления `RecyclerView` данных. исходные изменения.

Дополнительные сведения о `RecyclerView`см. в [справочнике по классам RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html).

## <a name="related-links"></a>Связанные ссылки

- [Рециклервиевер (пример)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-recyclerviewer)
- [Введение в интерфейс без описания операций](~/android/platform/lollipop.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
