---
title: Android Wear
description: Создание приложений для устройств Android носимого пользователем.
ms.prod: xamarin
ms.assetid: 3BE4A128-2D88-4500-9E48-20375EA99A49
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/16/2018
ms.openlocfilehash: 5768a8fb402304d399e619ddb111aea24d975daf
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2019
ms.locfileid: "70758333"
---
# <a name="android-wear"></a>Android Wear

"Износ Android" — это версия Android, предназначенная для устройств носимого пользователем, таких как интеллектуальные контрольные значения. В этом разделе приводятся инструкции по установке и настройке средств, необходимых для выполнения разработки, пошаговое пошаговое руководство по созданию первого устройства, а также список примеров, на которые можно ознакомиться для создания собственных приложений для износа.

## <a name="getting-startedandroidwearget-startedindexmd"></a>[Начало работы](~/android/wear/get-started/index.md)

В этой статье представлены сведения об износе Android, описывается, как установить и настроить компьютер для обеспечения действительной разработки, а также пошаговые инструкции по созданию и запуску первого приложения "износа Android" на эмуляторе или устройстве износа.

## <a name="user-interfaceandroidwearuser-interfaceindexmd"></a>[Пользовательский интерфейс](~/android/wear/user-interface/index.md)

Описание элементов управления, относящихся к износу Android, и ссылки на примеры, демонстрирующие использование этих элементов управления.

## <a name="platform-featuresandroidwearplatformindexmd"></a>[Функции платформы](~/android/wear/platform/index.md)

Документы в этом разделе охватывают функции, относящиеся к износу Android. Здесь вы найдете раздел, в котором описано, как создать Ватчфаце.

## <a name="screen-sizesandroidwearscreen-sizesmd"></a>[Размеры экрана](~/android/wear/screen-sizes.md)

Предварительный просмотр и оптимизация пользовательского интерфейса для доступных размеров экрана.

## <a name="deployment--testingandroidweardeploy-testindexmd"></a>[Тестирование и развертывание](~/android/wear/deploy-test/index.md)

Объясняется, как развернуть приложение "износ Android" на устройстве "износ Android" или в эмуляторе Android, настроенном для износа. В нем также содержатся советы по отладке и сведения о настройке подключения Bluetooth между компьютером разработчика и устройством Android.

## <a name="wear-apishttpsdeveloperandroidcomreferenceandroidsupportwearable"></a>[API-интерфейсы износа](https://developer.android.com/reference/android/support/wearable)

Сайт для разработчиков Android предоставляет подробные сведения об основных API-интерфейсах износа, [таких](https://developer.android.com/reference/com/google/android/wearable/intent/package-summary.html)как [действия носимого пользователем](https://developer.android.com/reference/android/support/wearable/activity/package-summary.html), намерения, [Проверка подлинности](https://developer.android.com/reference/android/support/wearable/authentication/package-summary.html), [сложности](https://developer.android.com/reference/android/support/wearable/complications/package-summary.html), [визуализация](https://developer.android.com/reference/android/support/wearable/complications/rendering/package-summary.html), [уведомления](https://developer.android.com/reference/android/support/wearable/notifications/package-summary.html), [ Представления](https://developer.android.com/reference/android/support/wearable/view/package-summary.html)и [ватчфаце](https://developer.android.com/reference/android/support/wearable/watchface/package-summary.html).

## <a name="samples"></a>Примеры

Вы можете найти несколько [примеров](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Android+wear) , используя износ Android (или перейти непосредственно в [GitHub](https://github.com/xamarin/monodroid-samples/tree/master/wear)).

|Пример|Описание|Снимок экрана|
|--- |--- |--- |
|[скелетонвеар](https://docs.microsoft.com/samples/xamarin/monodroid-samples/wear-skeletonwear)|Простой пример основ проектов носимого пользователем, включая Гридвиевпажер и интерактивные уведомления.|![Снимок экрана Скелетонвеар](images/skeleton.png)|
|[ватчвиевстуб](https://docs.microsoft.com/samples/xamarin/monodroid-samples/wear-watchviewstub)|Простая демонстрация элемента управления Ватчвиевстуб, который обнаруживает форму экрана и автоматически загружает правильный макет. Узнайте, как работает Ватчвиевстуб в макете **Resources/Layout/main_activity. XML** .|![Снимок экрана Ватчвиевстуб](images/watchview.png)|
|[реЦипеассистант](https://docs.microsoft.com/samples/xamarin/monodroid-samples/wear-recipeassistant)|Демонстрация страниц с нестандартными уведомлениями в виде шагов рецепта. Уведомления создаются в RecipeService.cs.|![Снимок экрана РеЦипеассистант](images/recipeassist.png)|
|[елизачат](https://docs.microsoft.com/samples/xamarin/monodroid-samples/wear-elizachat)|Забавный пример взаимодействия с "личным помощником", именуемый Елиза, с использованием стандартных интерактивных уведомлений для создания диалога с использованием предустановленных ответов.|![Снимок экрана Елизачат](images/eliza.png)|
|[GridViewPager](https://docs.microsoft.com/samples/xamarin/monodroid-samples/wear-gridviewpager)|Гридвиевпажер реализует плоский навигационный шаблон, где пользователь выполняет вертикальное перемещение, а затем по горизонтали для навигации по параметрам и содержимому.|![Снимок экрана Гридвиевпажер](images/gridviewpager.png)|
|[ватчфаце](https://docs.microsoft.com/samples/xamarin/monodroid-samples/wear-watchface)|Ватчфаце — это пользовательское лицо с контрольными данными с использованием часов, минут и секунд с аналоговым стилем. В этом примере показано, как создать службу отслеживания лиц, которая рисует текущее время и обрабатывает события изменения внешнего режима и видимости. Он включает широковещательный приемник, который прослушивает изменения часового пояса и автоматически обновляет соответствующее время.|![Снимок экрана Ватчфаце](images/gridviewpager.png)|

## <a name="videos"></a>Видеоролики

Ознакомьтесь с этими видеоматериалами, посвященными Xamarin. Android с поддержкой износа:

|Описание|Снимок экрана|
|--- |--- |
|[Android L и многое другое](https://blog.xamarin.com/webinar-recording-android-l-and-so-much-more/) &ndash; В предварительной версии Android L для разработчиков появились новые API-интерфейсы, которые позволяют разработчикам воспользоваться преимуществами, включая проектирование материалов, уведомления и новые анимации.|![Снимок экрана презентации](images/video-android-l.png)|
|[C#в моей Ушкие и в моих глаза: Носимого пользователем вычисления с помощью Google](https://www.youtube.com/watch?v=80H8tXByZQc) Glass и Android могут показаться что-нибудь из будущего (или мини-приложения инспектора), но многие люди уже используют будущее сегодня! &ndash; C#разработчики узнают это и уже имеют средства и навыки для использования возможностей носимого пользователем устройств (от развития до 2014).|![Снимок экрана презентации](images/video-eyes-ears.png)|
|[Новые возможности Xamarin. Android](https://www.youtube.com/watch?v=Gpqc2XZIQfU) &ndash; Android L, Android Light, Android TV, автозапуск для Android, проектирование материалов и искусство; что это означает в качестве разработчика Xamarin? от развития 2014.|![Снимок экрана презентации](Images/video-whats-new.png)|

<!--

March 18
https://blog.xamarin.com/android-wear/

August 14
https://blog.xamarin.com/android-l-developer-preview-android-wear-support/

August 27
https://blog.xamarin.com/tips-for-your-first-android-wear-app/

Watch Face
https://github.com/Redth/Xamarin.Wear.WatchFace
-->
