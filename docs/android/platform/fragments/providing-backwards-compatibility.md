---
title: Обеспечение обратной совместимости с пакетом поддержки Android
ms.prod: xamarin
ms.assetid: 7511D2F8-2B4F-4200-C74E-E967153B2E8D
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/12/2017
ms.openlocfilehash: c32d666da1347b947c55209ed7c7ec31a97a70e0
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73027299"
---
# <a name="providing-backwards-compatibility-with-the-android-support-package"></a>Обеспечение обратной совместимости с пакетом поддержки Android

Полезность фрагментов будет ограничена без обратной совместимости с устройствами предварительного Android 3,0 (API уровня 11). Чтобы предоставить эту возможность, в Google появилась [Библиотека поддержки](https://developer.android.com/sdk/compatibility-library.html) (изначально называлась *библиотекой совместимости Android* при ее освобождении), которая удаляет некоторые API из более новых версий Android в более старые версии Android. Это пакет поддержки Android, который позволяет устройствам под управлением Android 1,6 (API уровня 4) в Android 2.3.3. (API уровня 10).

> [!NOTE]
> Только `ListFragment` и `DialogFragment` доступны через пакет поддержки Android. Ни один из других подклассов фрагментов, таких как `PreferenceFragment,`, не поддерживается в пакете поддержки Android. Они не будут работать в приложениях до Android 3,0. 

## <a name="adding-the-support-package"></a>Добавление пакета поддержки

Пакет поддержки Android не добавляется автоматически в приложение Xamarin. Android. Xamarin предоставляет [пакет NuGet библиотеки поддержки Android версии v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) для упрощения добавления библиотек поддержки в приложение Xamarin. Android. Чтобы включить пакеты поддержки в приложение Xamarin. Android, включите компонент [библиотеки поддержки Android версии 4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) в проект Xamarin. Android, как показано на следующем снимке экрана: 

[![снимок экрана пакета библиотеки поддержки Android версии 4, добавляемого в проект](providing-backwards-compatibility-images/02-sml.png)](providing-backwards-compatibility-images/02.png#lightbox)

После выполнения этих действий в более ранних версиях Android можно будет использовать фрагменты. API-интерфейсы фрагмента будут работать в предыдущих версиях, за исключением следующих: 

- **Измените минимальную версию android** &ndash; приложение больше не должно быть предназначено для Android 3,0 или более поздней версии, как показано ниже: 

    [![снимок экрана минимального целевого значения Android в манифесте Android](providing-backwards-compatibility-images/03-sml.png)](providing-backwards-compatibility-images/03.png#lightbox)

- **Расширение фрагментактивити** &ndash; действия, в которых размещены фрагменты, теперь должны наследовать от `Android.Support.V4.App.FragmentActivity`, а не от `Android.App.Activity`. 

- **Обновление пространств имен** &ndash; классы, которые наследуют от `Android.App.Fragment`, теперь должны наследовать от `Android.Support.V4.App.Fragment`. Удалите оператор using "`using Android.App;`" в верхней части файла исходного кода и замените его на "`using Android.Support.V4.App`". 

- **Используйте суппортфрагментманажер** &ndash; `Android.Support.V4.App.FragmentActivity` предоставляет свойство `SupportingFragmentManager`, которое должно использоваться для получения ссылки на `FragmentManager`. Пример: 

```csharp
FragmentTransaction fragmentTx = this.SupportingFragmentManager.BeginTransaction();
DetailsFragment detailsFrag = new DetailsFragment();
fragmentTx.Add(Resource.Id.fragment_container, detailsFrag);
fragmentTx.Commit();
```

После внесения этих изменений можно будет запустить приложение на основе фрагмента на Android 1,6 или 2. x, а также на Хонэйкомб и Южные Сандвичевы. 

## <a name="related-links"></a>Связанные ссылки

- [Библиотека поддержки Android v4, NuGet](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)
