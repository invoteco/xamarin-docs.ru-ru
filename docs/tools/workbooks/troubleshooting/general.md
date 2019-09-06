---
title: Известные проблемы & решения
description: В этом документе описаны известные проблемы и способы их решения для Xamarin Workbooks. В нем обсуждаются проблемы CultureInfo, ошибки JSON и многое другое.
ms.prod: xamarin
ms.assetid: 495958BA-C9C2-4910-9BAD-F48A425208CF
author: conceptdev
ms.author: crdun
ms.date: 03/30/2017
ms.openlocfilehash: b7b73e214af6a5a45426b4e2d2d7e01a436b379e
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/04/2019
ms.locfileid: "70292783"
---
# <a name="known-issues--workarounds"></a>Известные проблемы & решения

## <a name="persistence-of-cultureinfo-across-cells"></a>Сохраняемость CultureInfo в ячейках

Параметр `System.Threading.CurrentThread.CurrentCulture` или`System.Globalization.CultureInfo.CurrentCulture` не сохраняется в ячейках книг на целевых объектах книг на основе Mono (Mac, iOS и Android) из-за [ `AppContext.SetSwitch` ошибки в реализации Mono][appcontext-bug] .

### <a name="workarounds"></a>Методы обхода проблемы

- Задайте локальное `DefaultThreadCurrentCulture`доменное приложение:

```csharp
using System.Globalization;
CultureInfo.DefaultThreadCurrentCulture = new CultureInfo("de-DE")
```

- Или обновите книги до версии 1.2.1 или более поздней, которая будет перезаписывать `System.Threading.CurrentThread.CurrentCulture` назначения `System.Globalization.CultureInfo.CurrentCulture` и предоставлять требуемый режим работы (обходит ошибку Mono).

## <a name="unable-to-use-newtonsoftjson"></a>Не удается использовать Newtonsoft. JSON

### <a name="workaround"></a>Возможное решение

- Обновите книгу до 1.2.1, которая установит Newtonsoft. JSON 9.0.1.
  Книги 1,3, в настоящее время в альфа-канале, поддерживают версии 10 и более поздние.

### <a name="details"></a>Подробные сведения

Newtonsoft. JSON 10 был освобожден, что его зависимость от Microsoft. CSharp, которая конфликтует с книгами версий, поддерживает `dynamic`. Это решено в выпуске книги 1,3 Preview, но сейчас мы обработали его, закрепите Newtonsoft. JSON специально для версии 9.0.1.

Пакеты NuGet, явно зависящие от Newtonsoft. JSON 10 или более поздней версии, поддерживаются только в книгах 1,3, в настоящее время в альфа-канале.

## <a name="code-tooltips-are-blank"></a>Подсказки кода пусты

В [редакторе Монако][monaco-bug] в Safari или WebKit есть ошибка, которая используется в приложении "книги Mac", что приводит к отображению всплывающих подсказок кода без текста.

![](general-images/monaco-signature-help-bug.png)

### <a name="workaround"></a>Возможное решение

- Если щелкнуть подсказку после ее отображения, текст будет отображаться.

- Или обновите книги до версии 1.2.1 или более поздней

[appcontext-bug]: https://bugzilla.xamarin.com/show_bug.cgi?id=54448
[monaco-bug]: https://github.com/Microsoft/monaco-editor/issues/408

## <a name="skiasharp-renderers-are-missing-in-workbooks-13"></a>Модули подготовки отчетов SkiaSharp отсутствуют в книгах 1,3

Начиная с книг 1,3, мы удалили модули подготовки отчетов SkiaSharp, которые мы поставляли в книгах 0.99.0, в пользу SkiaSharp, предоставляющей сами средства подготовки отчетов, с помощью нашего [пакета SDK](~/tools/workbooks/sdk/index.md).

### <a name="workaround"></a>Возможное решение

- Обновите SkiaSharp до последней версии в NuGet. На момент написания статьи это 1.57.1.

## <a name="related-links"></a>Связанные ссылки

- [Сообщения об ошибках](~/tools/workbooks/install.md#reporting-bugs)
