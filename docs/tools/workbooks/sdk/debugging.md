---
title: Отладка интеграции
description: В этом документе описывается, как выполнять отладку Xamarin Workbooks интеграции, как на стороне агента, так и на стороне клиента в Windows и Mac.
ms.prod: xamarin
ms.assetid: 90143544-084D-49BF-B44D-7AF943668F6C
author: conceptdev
ms.author: crdun
ms.date: 06/19/2018
ms.openlocfilehash: fbb5673a70328ad6edde78af1b35d2801fe65ca8
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/04/2019
ms.locfileid: "70283926"
---
# <a name="debugging-integrations"></a>Отладка интеграции

## <a name="debugging-agent-side-integrations"></a>Отладка интеграции на стороне агента

Отладку интеграции на стороне агента лучше выполнить с помощью методов ведения журнала, предоставляемых `Log` классом в. `Xamarin.Interactive.Logging`

В macOS сообщения журнала отображаются в меню средства просмотра журналов (**окно > Log Viewer**) и в журнале клиента. В Windows сообщения появляются только в журнале клиента, так как в средстве просмотра журнала нет.

Журнал клиента находится в следующих расположениях в macOS и Windows:

- Mac: `~/Library/Logs/Xamarin/Workbooks/Xamarin Workbooks {date}.log`
- Windows: `%LOCALAPPDATA%\Xamarin\Workbooks\logs\Xamarin Workbooks {date}.log`

Следует помнить, что при загрузке интеграции через обычный `#r` механизм во время разработки сборка интеграции будет выбрана как _зависимость_ книги и упакована в нее, если не используется абсолютный путь. Это может привести к тому, что изменения не будут распространяться, как если бы при перестроении интеграции ничего не происходило.

## <a name="debugging-client-side-integrations"></a>Отладка интеграции на стороне клиента

Так как интеграция на стороне клиента написана на языке JavaScript и загружается в веб-обозреватель ( см. документацию по [архитектуре](~/tools/workbooks/sdk/architecture.md)), лучшим способом для их отладки является использование средств разработчика WebKit на Mac или использование F12 Chooser в Windows.

Оба набора средств позволяют просматривать источник JavaScript и TypeScript, устанавливать точки останова, просматривать выходные данные консоли, а также проверять и изменять модель DOM.

### <a name="mac"></a>Mac

Чтобы включить средства разработчика для Xamarin Workbooks на Mac, выполните в терминале следующую команду:

```shell
defaults write com.xamarin.Workbooks WebKitDeveloperExtras -bool true
```

а затем перезапустите Xamarin Workbooks. После этого в контекстном меню щелчка правой кнопкой мыши появится **элемент проверить** , а в настройках книги будет доступна новая панель **разработчика** . Этот параметр позволяет выбрать, следует ли открывать средства разработчика при запуске.

[![Область разработчика](debugging-images/developer-pane-small.png)](debugging-images/developer-pane.png#lightbox)

Этот параметр также доступен только для перезагрузки. чтобы изменения в новых книгах вступили в силу, необходимо перезапустить клиент книги. При активации средств разработчика с помощью контекстного меню или настроек будет показан привычный пользовательский интерфейс Safari:

[![Средства разработки Safari](debugging-images/mac-dev-tools.png)](debugging-images/mac-dev-tools.png#lightbox)

Сведения об использовании средств разработчика Safari см. в документации по [инспектору WebKit][webkit-docs].

### <a name="windows"></a>Windows

В Windows группа IE предоставляет средство, называемое "F12 Chooser", которое является удаленным отладчиком для внедренных экземпляров Internet Explorer. Средство можно найти в следующем расположении:

```shell
C:\Windows\System32\F12\F12Chooser.exe
```

Выполните команду F12 Chooser, и вы увидите встроенный экземпляр, который запускает рабочую область книги в списке. Выберите его, после чего появится знакомое средство отладки F12 из Internet Explorer, подключенное к клиенту:

[![Средства F12](debugging-images/windows-dev-tools.png)](debugging-images/windows-dev-tools.png#lightbox)

[webkit-docs]: https://trac.webkit.org/wiki/WebInspector
