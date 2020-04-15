---
title: Обеспечение обратной совместимости с пакетом поддержки Android
ms.prod: xamarin
ms.assetid: 7511D2F8-2B4F-4200-C74E-E967153B2E8D
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/12/2017
ms.openlocfilehash: c32d666da1347b947c55209ed7c7ec31a97a70e0
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/13/2020
ms.locfileid: "73027299"
---
# <a name="providing-backwards-compatibility-with-the-android-support-package"></a>Обеспечение обратной совместимости с пакетом поддержки Android

Полезность фрагментов будет ограничена без обратной совместимости с устройствами предварительной версии Android 3.0 (API уровня 11). Чтобы предоставить эту возможность, в Google появилась [Библиотека поддержки](https://developer.android.com/sdk/compatibility-library.html) (изначально, после выпуска именуемая *библиотекой совместимости Android*), которая применяет некоторые API из более новых версий Android в более старые версии Android. Это пакет поддержки Android, который позволяет устройствам под управлением Android 1.6 (API уровня 4) работать в Android 2.3.3. (API уровня 10).

> [!NOTE]
> При использовании пакета поддержки Android доступны только `ListFragment` и `DialogFragment`. Ни один из других подклассов фрагментов, таких как `PreferenceFragment,`, не поддерживается в пакете поддержки Android. Они не будут работать в приложениях до версии Android 3.0. 

## <a name="adding-the-support-package"></a>Добавление пакета поддержки

Пакет поддержки Android не добавляется автоматически в приложение Xamarin.Android. Xamarin предоставляет [пакет NuGet библиотеки поддержки Android версии 4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/), чтобы упростить добавление библиотек поддержки в приложение Xamarin.Android. Чтобы включить пакеты поддержки в приложение Xamarin.Android, включите компонент [Библиотека поддержки Android версии 4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) в проект Xamarin.Android, как показано на следующем снимке экрана: 

[![Снимок экрана пакета библиотеки поддержки Android версии 4, добавляемого в проект](providing-backwards-compatibility-images/02-sml.png)](providing-backwards-compatibility-images/02.png#lightbox)

После выполнения этих действий вы сможете использовать Фрагменты в более ранних версиях Android. API-интерфейсы Фрагмента будут работать в предыдущих версиях, за исключением следующих: 

- **Измените минимальную версию Android** &ndash; приложение больше не должно быть предназначено для Android 3.0 или более поздней версии, как показано ниже: 

    [![Снимок экрана с минимальным целевым значением Android в манифесте Android](providing-backwards-compatibility-images/03-sml.png)](providing-backwards-compatibility-images/03.png#lightbox)

- **Расширьте FragmentActivity** &ndash; действия, в которых размещены фрагменты, теперь должны наследовать из `Android.Support.V4.App.FragmentActivity`, а не из `Android.App.Activity`. 

- **Обновление пространств имен** &ndash; классы, которые наследуют из `Android.App.Fragment`, теперь должны наследовать из `Android.Support.V4.App.Fragment`. Удалите оператор использования "`using Android.App;`"в верхней части файла с исходным кодом и замените его на "`using Android.Support.V4.App`". 

- **Используйте SupportFragmentManager** &ndash; `Android.Support.V4.App.FragmentActivity` предоставляет свойство `SupportingFragmentManager`, которое должно использоваться для получения ссылки на `FragmentManager`. Пример: 

```csharp
FragmentTransaction fragmentTx = this.SupportingFragmentManager.BeginTransaction();
DetailsFragment detailsFrag = new DetailsFragment();
fragmentTx.Add(Resource.Id.fragment_container, detailsFrag);
fragmentTx.Commit();
```

После внесения этих изменений приложение на основе Фрагмента можно будет запустить на Android 1.6 или 2.x, а также на Honeycomb и Ice Cream Sandwich. 

## <a name="related-links"></a>Связанные ссылки

- [Библиотека поддержки Android версии 4 NuGet](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)
