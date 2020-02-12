---
title: Сводка Глава 24. Переход по страницам
description: 'Создание мобильных приложений с помощью Xamarin.Forms: Сводка Глава 24. Переход по страницам'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: DDCDB49C-6008-4F72-B095-463EE21D7C23
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: fd8e4fc77917fcba9bc61e59ced714ac1cd6fbe9
ms.sourcegitcommit: ccbf914615c0ce6b3f308d930f7a77418aeb4dbc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/11/2020
ms.locfileid: "77130845"
---
# <a name="summary-of-chapter-24-page-navigation"></a>Сводка Глава 24. Переход по страницам

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24)

Многие приложения состоят из нескольких страниц, между которыми переходит пользователь. У приложения всегда есть Главная *страница или* *Домашняя* страница, и из нее пользователь переходит на другие страницы, которые хранятся в стеке для перехода назад. Дополнительные параметры навигации описаны в [**главе 25. Вида страниц**](chapter25.md).

## <a name="modal-pages-and-modeless-pages"></a>Модальные страницы и страницы немодального

`VisualElement` определяет свойство [`Navigation`](xref:Xamarin.Forms.NavigableElement.Navigation) типа [`INavigation`](xref:Xamarin.Forms.INavigation), которое включает следующие два метода перехода на новую страницу:

- [`PushAsync`](xref:Xamarin.Forms.INavigation.PushAsync(Xamarin.Forms.Page))
- [`PushModalAsync`](xref:Xamarin.Forms.INavigation.PushModalAsync(Xamarin.Forms.Page))

Оба метода принимают экземпляр `Page` в качестве аргумента и возвращают объект `Task`. Следующие два метода перейдите обратно к предыдущей странице:

- [`PopAsync`](xref:Xamarin.Forms.INavigation.PopAsync)
- [`PopModalAsync`](xref:Xamarin.Forms.INavigation.PopModalAsync)

Если пользовательский интерфейс имеет собственную кнопку « **назад** » (как для Android, так и для Windows Phone), то приложению не нужно вызывать эти методы.

Хотя эти методы доступны из любого `VisualElement`, обычно они вызываются из свойства `Navigation` текущего экземпляра `Page`.

Приложения обычно используют модальные страницы, когда пользователь должен предоставить некоторые сведения на странице, прежде чем возвращать на предыдущую страницу. Страницы, которые не являются модальными, иногда называют немодальными или *иерархическими*. Ничего не на самой странице отличает его как модальное или немодальное; он управляется вместо этого метод, используемый для перехода к нему. Для работы на всех платформах, модальные страницы необходимо предоставить собственный пользовательский интерфейс для перехода к предыдущей странице.

Образец [**моделессандмодал**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ModelessAndModal) позволяет исследовать разницу между немодальными и модальными страницами. Любое приложение, использующее навигацию по страницам, должно передавать свою домашнюю страницу конструктору [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) , как правило, в классе `App` программы. Одна премия заключается в том, что вам больше не нужно задавать `Padding` на странице для iOS.

Вы обнаружите, что для немодальных страниц отображается свойство [`Title`](xref:Xamarin.Forms.Page.Title) страницы. IOS, Android и платформы планшетных и рабочий стол Windows укажите элемент пользовательского интерфейса для обратного перехода на предыдущую страницу. На устройствах Android и Windows Phone есть стандартная кнопка **назад** для возврата.

Для модальных страниц страница `Title` не отображается, и не предоставляется элемент пользовательского интерфейса для возврата на предыдущую страницу. Хотя вы можете **использовать стандартную** кнопку "Android и Windows Phone", чтобы вернуться на предыдущую страницу, модальная страница на других платформах должна предоставить собственный механизм для возврата.

### <a name="animated-page-transitions"></a>Анимированные переходы

Альтернативные версии различных методов навигации предоставляются с помощью второго логического аргумента, который имеет значение `true`, если требуется, чтобы переход страницы включал анимацию:

- [PushAsync](xref:Xamarin.Forms.INavigation.PushAsync(Xamarin.Forms.Page,System.Boolean))
- [пушмодаласинк](xref:Xamarin.Forms.INavigation.PushModalAsync(Xamarin.Forms.Page,System.Boolean))
- [попасинк](xref:Xamarin.Forms.INavigation.PopAsync(System.Boolean))
- [попмодаласинк](xref:Xamarin.Forms.INavigation.PopModalAsync(System.Boolean))

Однако стандартные методы навигации по страницам включают анимацию по умолчанию, поэтому они полезны только при переходе на определенную страницу при запуске (как описано в конце этой главы) или при предоставлении собственной анимации входа (как описано в [**Chapter22). Анимация**](chapter22.md)).

### <a name="visual-and-functional-variations"></a>Визуальные и функциональные вариантов

`NavigationPage` содержит два свойства, которые можно задать при создании экземпляра класса в методе `App`:

- [`BarBackgroundColor`](xref:Xamarin.Forms.NavigationPage.BarBackgroundColor)
- [`BarTextColor`](xref:Xamarin.Forms.NavigationPage.BarTextColor)

`NavigationPage` также включает четыре присоединенных присоединяемых свойства, влияющие на определенную страницу, на которой они установлены:

- [`SetHasBackButton`](xref:Xamarin.Forms.NavigationPage.SetHasBackButton(Xamarin.Forms.Page,System.Boolean)) и [`GetHasBackButton`](xref:Xamarin.Forms.NavigationPage.GetHasBackButton(Xamarin.Forms.Page))
- [`SetHasNavigationBar`](xref:Xamarin.Forms.NavigationPage.SetHasNavigationBar(Xamarin.Forms.BindableObject,System.Boolean)) и [`GetHasNavigationBar`](xref:Xamarin.Forms.NavigationPage.GetHasNavigationBar(Xamarin.Forms.BindableObject))
- [`SetBackButtonTitle`](xref:Xamarin.Forms.NavigationPage.SetBackButtonTitle(Xamarin.Forms.BindableObject,System.String)) и [`GetBackButtonTitle`](xref:Xamarin.Forms.NavigationPage.GetBackButtonTitle(Xamarin.Forms.BindableObject)) работают только в iOS
- [`SetTitleIcon`](xref:Xamarin.Forms.NavigationPage.SetTitleIcon(Xamarin.Forms.BindableObject,Xamarin.Forms.FileImageSource)) и [`GetTitleIcon`](xref:Xamarin.Forms.NavigationPage.GetTitleIcon(Xamarin.Forms.BindableObject)) работают только в iOS и Android

### <a name="exploring-the-mechanics"></a>Изучение механизм

Методы навигации по страницам являются асинхронными и должны использоваться с `await`. Завершение не указывает, что Навигация по страницам завершен, но только безопасность для просмотра стека навигации по страницам.

Когда одна страница переходит к другой, первая страница обычно получает вызов метода [`OnDisappearing`](xref:Xamarin.Forms.Page.OnDisappearing) , а вторая страница получает вызов его метода [`OnAppearing`](xref:Xamarin.Forms.Page.OnAppearing) . Аналогично, когда одна страница возвращается к другой, первая страница получает вызов метода `OnDisappearing`, а вторая страница обычно получает вызов метода `OnAppearing`. Порядок этих вызовов (и завершения асинхронных методов, который вызывает переход) — зависит от платформы. Слово «обычно» в две предыдущие инструкции используется из-за Android модальное окно навигации, в котором не возникают эти вызовы методов.

Кроме того, вызовы методов `OnAppearing` и `OnDisappearing` не обязательно указывают на навигацию по страницам.

Интерфейс `INavigation` содержит два свойства коллекции, которые позволяют проверить стек навигации:

- [`NavigationStack`](xref:Xamarin.Forms.INavigation.NavigationStack) типа `IReadOnlyList<Page>` для немодального стека
- [`ModalStack`](xref:Xamarin.Forms.INavigation.ModalStack) типа `IReadOnlyList<Page>` для модального стека

Можно обеспечить безопасность доступа к этим стекам из свойства `Navigation` `NavigationPage` (которое должно быть свойством [`MainPage`](xref:Xamarin.Forms.Application.MainPage) класса `App`). Безопасным является только для просмотра этих стеков после выполнения методов асинхронной Навигация по страницам. Свойство [`CurrentPage`](xref:Xamarin.Forms.NavigationPage.CurrentPage) `NavigationPage` не указывает на текущую страницу, если текущая страница является модальной страницей, а указывает на последнюю немодальную страницу.

Образец [**синглепаженавигатион**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/SinglePageNavigation) позволяет исследовать навигацию по страницам и стеки, а также юридические типы переходов по страницам:

- Страницы немодального можно перейти к другой немодальные или модальные страницы
- Модальная страница поддерживает переход только к другой модальные страницы

### <a name="enforcing-modality"></a>Применение модальность

Приложение использует модальной страницы, когда это необходимо для получения некоторых сведений от пользователя. Пользователь должен быть запрещен от возврата на предыдущую страницу, пока не будет предоставлена эту информацию. В iOS можно легко предоставить кнопку " **назад** " и включить ее только после завершения работы пользователя со страницей. Но для устройств Android и Windows Phone приложение должно переопределять метод [`OnBackButtonPressed`](xref:Xamarin.Forms.Page.OnBackButtonPressed) и возвращать `true`, если программа обрабатывала саму кнопку **назад** , как показано в примере [**модаленфорцемент**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ModalEnforcement) .

В примере [**мввменфорцемент**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/MvvmEnforcement) показано, как это работает в сценарии MVVM.

## <a name="navigation-variations"></a>Варианты навигации

Если это был переход определенной модальные страницы может быть несколько раз, он должен сохранить сведения, таким образом, чтобы пользователь мог изменить данные, а не вводить его на еще раз. Это можно обрабатывать за счет сохранения определенным экземпляром модальные страницы, но сохраняет данные в модель представления более эффективный подход (особенно в iOS).

### <a name="making-a-navigation-menu"></a>Создание меню навигации

В примере [**виевгаллеритипе**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ViewGalleryType) демонстрируется использование `TableView` для вывода списка пунктов меню. Каждый элемент связан с объектом `Type` для определенной страницы. При выборе этого элемента, программа создает экземпляр страницы и переходу на нее.

[![Тройной снимок экрана: тип коллекции представлений](images/ch24fg21-small.png "Пункты меню "список Таблевиев"")](images/ch24fg21-large.png#lightbox "Пункты меню "список Таблевиев"")

Пример [**виевгаллеринст**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ViewGalleryInst) немного отличается тем, что меню содержит экземпляры каждой страницы, а не типы. Это позволяет сохранять сведения о каждой странице, но все страницы должен быть создан при запуске программы.

### <a name="manipulating-the-navigation-stack"></a>Управление стек навигации

[**Стаккманипулатион**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/StackManipulation) демонстрирует несколько функций, определенных `INavigation`, которые позволяют управлять стеком навигации структурированным способом:

- [`RemovePage`](xref:Xamarin.Forms.INavigation.RemovePage(Xamarin.Forms.Page))
- [`InsertPageBefore`](xref:Xamarin.Forms.INavigation.InsertPageBefore(Xamarin.Forms.Page,Xamarin.Forms.Page))
- [`PopToRootAsync`](xref:Xamarin.Forms.INavigation.PopToRootAsync) и [`PopToRootAsync`](xref:Xamarin.Forms.INavigation.PopToRootAsync(System.Boolean)) с необязательной анимацией

### <a name="dynamic-page-generation"></a>Создание динамической страницы

В примере [**буилдапаже**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/BuildAPage) демонстрируется создание страницы во время выполнения на основе вводимых пользователем данных.

## <a name="patterns-of-data-transfer"></a>Шаблоны передачи данных

Часто возникает необходимость в обмене данными между страницами &mdash; для передачи данных на страницу навигации, а для страницы — для возврата данных на страницу, вызвавшую ее. Существует несколько методов для этого.

### <a name="constructor-arguments"></a>Аргументы конструктора

При переходе на новую страницу, можно создать экземпляр класса page с аргументом конструктора, позволяющий страницы, чтобы инициализировать самого себя. Это показано в образце [**счуландстудентс**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/SchoolAndStudents) . Также возможно, что страница, на которой выполняется переход, имеет `BindingContext`, заданную страницей, которая выполняет переход к ней.

### <a name="properties-and-method-calls"></a>Свойства и вызовы методов

В остальных примерах передачи данных исследовать проблемы передачи сведений о страниц, когда одна страница переходит на другую страницу и обратно. В этих обсуждениях *Домашняя* страница переходит на страницу *сведений* и должна передавать инициализированные сведения на страницу *сведений* . Страница *сведений* получает дополнительные сведения от пользователя и передает их на *домашнюю* страницу.

*Домашняя* страница может легко получить доступ к открытым методам и свойствам на странице *сведений* сразу после создания экземпляра этой страницы. Страница *сведений* также может получить доступ к открытым методам и свойствам на *домашней* странице, но выбор хорошего времени для этого может быть непростой задачей. Пример [**DateTransfer1**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer1) делает это в переопределении `OnDisappearing`. Недостаток состоит в том, что страница *сведений* должна знать тип *домашней* страницы.

### <a name="messagingcenter"></a>MessagingCenter

Класс Xamarin. Forms [`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) предоставляет другой способ связи между двумя страницами. Сообщения идентифицируются по текстовой строки и могут быть дополнены любого объекта.

Программа, желающая получить сообщения от определенного типа, должна подписываться на них с помощью [`MessagingCenter.Subscribe`](xref:Xamarin.Forms.MessagingCenter.Subscribe*) и указать функцию обратного вызова. Позже он может отменить подписывание, вызвав [`MessagingCenter.Unsubscribe`](xref:Xamarin.Forms.MessagingCenter.Unsubscribe*). Функция обратного вызова получает любое сообщение, отправленное из указанного типа, с указанным именем, отправленным с помощью метода [`Send`](xref:Xamarin.Forms.MessagingCenter.Send*) .

Программа [**DateTransfer2**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer2) показывает, как передавать данные с помощью центра обмена сообщениями, но опять же это требует, чтобы *информационная* страница знала тип *домашней* страницы.

### <a name="events"></a>События

Событие проверенные временем подход для одного класса отправлять сведения с другим классом, не зная тип этого класса. В образце [**DateTransfer3**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer3) класс *info* определяет событие, которое оно вызывает, когда информация готова. Однако на *домашней* странице нет удобного места для отключения обработчика событий.

### <a name="the-app-class-intermediary"></a>Класс посредника приложения

В примере [**DateTransfer4**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer4) показано, как получить доступ к свойствам, определенным в классе `App`, на *домашней* странице и на странице *сведений* . Это является хорошим решением, но в следующем разделе описывается, что-нибудь получше.

### <a name="switching-to-a-viewmodel"></a>Переключение на модель представления

Использование ViewModel для получения сведений позволяет *домашней* странице и странице *сведений* совместно использовать экземпляр класса Information. Это продемонстрировано в примере [**DateTransfer5**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer5) .

### <a name="saving-and-restoring-page-state"></a>Сохранение и восстановление состояния страницы

Промежуточный класс `App` класса или подход ViewModel идеально подходит, когда приложение должно сохранять информацию, если программа переходит в спящий режим, когда страница *сведений* активна. Это показано в образце [**DateTransfer6**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer6) .

## <a name="saving-and-restoring-the-navigation-stack"></a>Сохранение и восстановление стек навигации

В общем случае многостраничных программу, которая переходит в спящий режим должен перейти к той же странице, при восстановлении. Это означает, что такой программе следует сохранить содержимое стека навигации. В этом разделе показано, как автоматизировать этот процесс в классе специально для этой цели. Этот класс также вызывает отдельных страниц, чтобы они могли сохранять и восстанавливать их состояния страницы.

Библиотека [**Xamarin. формсбук. Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) определяет интерфейс с именем [`IPersistantPage`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/IPersistentPage.cs) , который классы могут реализовать для сохранения и восстановления элементов в словаре `Properties`.

Класс [`MultiPageRestorableApp`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MultiPageRestorableApp.cs) в библиотеке **Xamarin. формсбук. Toolkit** является производным от `Application`. Затем можно получить класс `App` от `MultiPageRestorableApp` и выполнить некоторые действия по обслуживанию.

В [**стаккресторедемо**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/StackRestoreDemo) демонстрируется использование `MultiPageRestorableApp`.

### <a name="something-like-a-real-life-app"></a>Что-то наподобие приложения реальной жизни

Пример [**нотетакер**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/NoteTaker) также использует `MultiPageRestorableApp` и позволяет вводить и изменять примечания, сохраненные в словаре `Properties`.

## <a name="related-links"></a>Связанные ссылки

- [Глава 24 Full Text (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch24-Apr2016.pdf)
- [Глава 24 примеры](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24)
- [Иерархическая навигация](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)
- [Модальные страницы](~/xamarin-forms/app-fundamentals/navigation/modal.md)
