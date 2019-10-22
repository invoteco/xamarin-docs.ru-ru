---
title: Ссылка на источник с помощью Xamarin. Forms
description: В этой статье объясняется, как использовать ссылку на источник для отладки в Xamarin. Forms.
zone_pivot_groups: platform
ms.prod: xamarin
ms.assetId: 1E13FCD9-5607-46E8-80E4-87A58B389BEB
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 09/26/2019
ms.openlocfilehash: 7a3fe70c8ac29f9e84b5d071a0ba1ef73afbbe11
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/21/2019
ms.locfileid: "72697809"
---
# <a name="source-link-with-xamarinforms"></a>Ссылка на источник с помощью Xamarin. Forms

Пакеты NuGet Xamarin. Forms включают сопоставления исходных ссылок. Ссылка на источник сопоставляет скомпилированные библиотеки, содержащиеся в пакете NuGet, с репозиторием исходного кода. Visual Studio загрузит файлы исходного кода во время отладки и позволит разработчикам выполнять код по шагам, обеспечивая отладку пакетов без сборки из источника.

Дополнительные сведения об использовании исходной ссылки см. в [документации по исходной ссылке](/dotnet/standard/library-guidance/sourcelink).

::: zone pivot="windows"

> [!WARNING]
> Visual Studio 2019 поддерживает ссылку на исходный код для **отладчика .NET** , но в настоящее время не поддерживает ссылку на исходный код для **отладчика Mono**. Таким образом, исходную ссылку можно использовать для отладки приложений UWP, но не для приложений Android или iOS. При отладке приложений UWP необходимо убедиться, что PDB-файлы библиотек, которые вы хотите отладить, копируются в папку **appx** в каталоге **bin** , где выполняется компиляция приложения.

## <a name="enable-source-link"></a>Включить ссылку на источник

Для использования исходной ссылки требуется включить отладку для внешнего кода. в противном случае отладчик будет выполнять предшествующие вызовы кода, не содержащегося в текущем решении. В Visual Studio 2019 это можно найти в меню **Параметры** в разделе **Отладка** :

[Ссылка на ![Enable источник в Visual Studio 2019](sourcelink-images/sourcelink-enable-pc-cropped.png)](sourcelink-images/sourcelink-enable-pc.png#lightbox)

Убедитесь, что **параметр Включить только мой код** отключен и включен **параметр Включить поддержку ссылок на источники** .

::: zone-end
::: zone pivot="macos"

## <a name="enable-source-link"></a>Включить ссылку на источник

Для использования исходной ссылки требуется включить отладку для внешнего кода. в противном случае отладчик будет выполнять предшествующие вызовы кода, не содержащегося в текущем решении. Этот параметр можно найти в окне " **настройки** " в разделе " **отладчик** ":

[Ссылка на источник ![Enable в Visual Studio для Mac](sourcelink-images/sourcelink-enable-mac-cropped.png)](sourcelink-images/sourcelink-enable-mac.png#lightbox)

Убедитесь, что **Шаг с заходом в внешний код** включен.

::: zone-end

## <a name="debug-xamarinforms-using-source-link"></a>Отладка Xamarin. Forms с использованием ссылки на источник

Если отладка внешних пакетов включена, Visual Studio будет использовать сопоставления исходной ссылки, содержащиеся в пакете NuGet, для скачивания и пошагового использования внешнего исходного кода. Это можно проверить, установив точку останова в вызове метода, предоставляемого Xamarin. Forms:

[![Breakpoint задано в методе Xamarin. Forms](sourcelink-images/breakpoint-cropped.png)](sourcelink-images/external-code-available.png#lightbox)

В зависимости от параметров, указанных в параметрах **отладчика** , Visual Studio выдаст предупреждение о том, что выполняется скачивание исходных файлов:

[предупреждение внешнего кода ![Visual Studio](sourcelink-images/external-code-cropped.png)](sourcelink-images/external-code-available.png#lightbox)

После того как вы разрешите Visual Studio загрузить файлы, отладчик выполнит шаг с заходом в внешний код.

::: zone pivot="windows"

## <a name="source-link-caching"></a>Кэширование исходной ссылки

Ссылка на источник использует кэширование для повышения производительности. Каталог Caching для ссылки на источник определяется в меню **Параметры** в разделе **Отладка** в разделе **символы** :

[Кэширование исходной ссылки ![Visual Studio](sourcelink-images/sourcelink-caching-pc-cropped.png)](sourcelink-images/sourcelink-caching-pc.png#lightbox)

Это меню позволяет указать каталог кэширования для всех отладочных символов, а также очистить кэш при возникновении проблем с кэшированными символами.

::: zone-end
::: zone pivot="macos"

## <a name="source-link-caching"></a>Кэширование исходной ссылки

Ссылка на источник использует кэширование для повышения производительности. Каталог Caching для ссылки на источник в MacOS — `/Users/<username>/Library/Caches/VisualStudio/8.0/Symbols`. Эта папка содержит вложенные папки, в которых хранится репозиторий, используемый для загрузки исходных файлов. Если резервный репозиторий для пакета NuGet изменился, может потребоваться вручную удалить эти папки для обновления кэша.

::: zone-end

## <a name="related-links"></a>Связанные ссылки

- [Документация по исходной ссылке](/dotnet/standard/library-guidance/sourcelink)
- [Ссылка на источник на GitHub](https://github.com/dotnet/sourcelink)
