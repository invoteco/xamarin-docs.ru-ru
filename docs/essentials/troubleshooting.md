---
title: 'Xamarin.Essentials: Устранение неполадок'
description: В этом документе описывается, как устранять неполадки, возникающие при разработке с помощью библиотеки Xamarin.Essentials.
ms.assetid: 2E474FAF-F841-4E3C-B815-F7ABD8EE3361
author: jamesmontemagno
ms.author: jamont
ms.date: 01/06/2020
ms.openlocfilehash: 2bd537a782b7090207b09ca02c5dfe5c4422a9ad
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/13/2020
ms.locfileid: "77545140"
---
# <a name="xamarinessentials-troubleshooting"></a>Xamarin.Essentials: Устранение неполадок

## <a name="error-version-conflict-detected-for-xamarinandroidsupportcompat"></a>Ошибка: для Xamarin.Android.Support.Compat обнаружен конфликт версий

При обновлении пакетов NuGet (или добавлении нового пакета) с помощью проекта Xamarin.Forms, использующего Xamarin.Essentials, может возникнуть следующая ошибка:

```error
NU1107: Version conflict detected for Xamarin.Android.Support.Compat. Reference the package directly from the project to resolve this issue. 
 MyApp -> Xamarin.Essentials 1.3.1 -> Xamarin.Android.Support.CustomTabs 28.0.0.3 -> Xamarin.Android.Support.Compat (= 28.0.0.3) 
 MyApp -> Xamarin.Forms 3.1.0.583944 -> Xamarin.Android.Support.v4 25.4.0.2 -> Xamarin.Android.Support.Compat (= 25.4.0.2).
```

Проблема заключается в несоответствии зависимостей для двух пакетов NuGet. Это можно устранить, добавив конкретную версию зависимости вручную (в данном случае **Xamarin.Android.Support.Compat**), которая поддерживает оба пакета.

Чтобы сделать это, добавьте пакет NuGet, который является источником конфликта, вручную, выбрав конкретную версию в списке **Версия**. Устранить эту ошибку можно с помощью пакетов NuGet Xamarin.Android.Support.Compat и Xamarin.Android.Support.Core.Util версии 28.0.0.3 (текущая версия).

Дополнительные сведения см. в [этой записи блога](https://redth.codes/how-to-fix-the-dreaded-version-conflict-nuget-error-in-your-xamarin-android-projects/), содержащей видео о том, как устранить проблему.

Если у вас возникли проблемы или вы обнаружили ошибки, сообщите о них в [репозиторий Xamarin.Essentials GitHub](https://github.com/xamarin/Essentials).
