---
title: Известные проблемы & решения
description: В этом документе описаны известные проблемы и способы их решения для Xamarin Workbooks. В нем обсуждаются проблемы CultureInfo, ошибки JSON и многое другое.
ms.prod: xamarin
ms.assetid: 495958BA-C9C2-4910-9BAD-F48A425208CF
author: davidortinau
ms.author: daortin
ms.date: 03/30/2017
ms.openlocfilehash: c7b9f93c2d6339ba1fd26b27742ecfc0f438c5de
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73018164"
---
# <a name="known-issues--workarounds"></a>Известные проблемы & решения

## <a name="persistence-of-cultureinfo-across-cells"></a>Сохраняемость CultureInfo в ячейках

Установка `System.Threading.CurrentThread.CurrentCulture` или `System.Globalization.CultureInfo.CurrentCulture` не сохраняется в ячейках книг на целевых объектах книг на основе Mono (Mac, iOS и Android) из-за [ошибки в реализации `AppContext.SetSwitch`Mono][appcontext-bug] .

### <a name="workarounds"></a>Методы обхода проблемы

- Задайте `DefaultThreadCurrentCulture`приложения — локальный домен:

```csharp
using System.Globalization;
CultureInfo.DefaultThreadCurrentCulture = new CultureInfo("de-DE")
```

- Или обновите книги до версии 1.2.1 или более поздней, которая будет перезаписывать назначения для `System.Threading.CurrentThread.CurrentCulture` и `System.Globalization.CultureInfo.CurrentCulture`, чтобы обеспечить требуемое поведение (обойти ошибку Mono).

## <a name="unable-to-use-newtonsoftjson"></a>Не удается использовать Newtonsoft. JSON

### <a name="workaround"></a>Обходной путь

- Обновите книгу до 1.2.1, которая установит Newtonsoft. JSON 9.0.1.
  Книги 1,3, в настоящее время в альфа-канале, поддерживают версии 10 и более поздние.

### <a name="details"></a>Подробнее

Newtonsoft. JSON 10 был освобожден, что противоречит его зависимости от Microsoft. CSharp, который конфликтует с книгами версий, которые доставляются для поддержки `dynamic`. Это решено в выпуске книги 1,3 Preview, но сейчас мы обработали его, закрепите Newtonsoft. JSON специально для версии 9.0.1.

Пакеты NuGet, явно зависящие от Newtonsoft. JSON 10 или более поздней версии, поддерживаются только в книгах 1,3, в настоящее время в альфа-канале.

## <a name="code-tooltips-are-blank"></a>Подсказки кода пусты

В [редакторе Монако][monaco-bug] в Safari или WebKit есть ошибка, которая используется в приложении "книги Mac", что приводит к отображению всплывающих подсказок кода без текста.

![](general-images/monaco-signature-help-bug.png)

### <a name="workaround"></a>Обходной путь

- Если щелкнуть подсказку после ее отображения, текст будет отображаться.

- Или обновите книги до версии 1.2.1 или более поздней

[appcontext-bug]: https://bugzilla.xamarin.com/show_bug.cgi?id=54448
[monaco-bug]: https://github.com/Microsoft/monaco-editor/issues/408

## <a name="skiasharp-renderers-are-missing-in-workbooks-13"></a>Модули подготовки отчетов SkiaSharp отсутствуют в книгах 1,3

Начиная с книг 1,3, мы удалили модули подготовки отчетов SkiaSharp, которые мы поставляли в книгах 0.99.0, в пользу SkiaSharp, предоставляющей сами средства подготовки отчетов, с помощью нашего [пакета SDK](~/tools/workbooks/sdk/index.md).

### <a name="workaround"></a>Обходной путь

- Обновите SkiaSharp до последней версии в NuGet. На момент написания статьи это 1.57.1.

## <a name="related-links"></a>Связанные ссылки

- [Сообщения об ошибках](~/tools/workbooks/install.md#reporting-bugs)
