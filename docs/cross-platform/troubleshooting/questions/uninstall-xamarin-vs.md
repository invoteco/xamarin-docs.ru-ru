---
title: Как полностью удалить Xamarin для Visual Studio?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: c1742239-05ea-449d-9c99-611e5e5a90e4
author: davidortinau
ms.author: daortin
ms.date: 12/02/2016
ms.openlocfilehash: ed0171c2b6bd98e5b29ec100d0235131d36acb05
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73013337"
---
# <a name="how-do-i-perform-a-thorough-uninstall-for-xamarin-for-visual-studio"></a>Как полностью удалить Xamarin для Visual Studio?

1. На панели управления Windows удалите следующие элементы:

    - Xamarin
    - Xamarin для Windows
    - Xamarin.Android
    - Xamarin.iOS
    - Xamarin для Visual Studio

2. В обозревателе удалите все оставшиеся файлы из папок расширения Xamarin Visual Studio (все версии, включая _программные файлы_ и _программные файлы (x86)_ ):

    _В.\\Program Files\*\\Microsoft Visual Studio 1\*0\\Common7\\IDE\\Extensions\\Xamarin_

3. Удалите также каталог кэша компонента MEF Visual Studio:

    _% LOCALAPPDATA%\\Microsoft\\VisualStudio\\1\*. 0\\Компонентмоделкаче_

    На самом деле этот шаг часто достаточно для разрешения таких ошибок, как:

    - "Пакет" Ксамариншеллпаккаже "не был правильно загружен"

    - "Файл проекта... не удается открыть. Отсутствует подтип проекта "

    - "Ссылка на объект не задана для экземпляра объекта.  в Xamarin. VisualStudio. IOS. Ксамариниоспаккаже. Initialize () "

    - "Сбой SetSite для пакета" (в Visual Studio _ActivityLog. XML_)

    - "Сбой Легациситепаккаже для пакета" (в Visual Studio _ActivityLog. XML_)

    (См. также расширение " [Очистка кэша компонентов MEF](https://visualstudiogallery.msdn.microsoft.com/22b94661-70c7-4a93-9ca3-8b6dd45f47cd) " для Visual Studio.  См. [ошибку 40781, комментарий 19,](https://bugzilla.xamarin.com/show_bug.cgi?id=40781#c19) чтобы получить более подробные сведения о вышестоящей неполадке в Visual Studio, которая может вызвать эти ошибки.)

4. Также проверьте каталог _виртуалсторе_ , чтобы узнать, могли ли Windows хранить файлы оверлея для _расширений\\каталогов Xamarin_ или _компонентмоделкаче_ .

    _% LOCALAPPDATA%\\Виртуалсторе_

5. Откройте редактор реестра (`regedit`).

6. Найдите следующий раздел:

    _HKEY\_локальный\_компьютер\\программное обеспечение\\Wow6432Node\\Microsoft\\Windows\\CurrentVersion\\SharedDlls_

7. Найдите и удалите все записи, соответствующие этому шаблону.

    _В.\\Program Files\*\\Microsoft Visual Studio 1\*0\\Common7\\IDE\\Extensions\\Xamarin_

8. Найдите следующий раздел.

    _HKEY\_текущий\_пользователь\\программное обеспечение\\Microsoft\\VisualStudio\\1\*0\\Екстенсионманажер\\Пендингделетионс_

9. Удалите все записи, которые могут иметь отношение к Xamarin.  Вот пример, который используется для того, чтобы вызвать проблемы в старых версиях Xamarin:

    _Моно. VisualStudio. Shell, 1.0_

10. Откройте `cmd.exe` командной строки администратора, а затем выполните команды `devenv /setup` и `devenv /updateconfiguration` для каждой установленной версии Visual Studio.  Например, для Visual Studio 2015:

    ```
    "%ProgramFiles(x86)%\Microsoft Visual Studio 14.0\Common7\IDE\devenv.exe" /setup
    "%ProgramFiles(x86)%\Microsoft Visual Studio 14.0\Common7\IDE\devenv.exe" /updateconfiguration
    ```

11. Перезагрузите систему.

12. Переустановите текущую стабильную версию Xamarin с помощью из [VisualStudio.com](https://visualstudio.com/xamarin/).

## <a name="additional-troubleshooting-steps-for-package-did-not-load-correctly"></a>Дополнительные действия по устранению неполадок при неправильной загрузке пакета

В тех случаях, когда описанные выше действия не позволяют устранить ошибку «пакет не был правильно загружен», попробуйте выполнить еще несколько действий.

1. Создайте новую учетную запись пользователя Windows.

2. Проверьте, загружены ли расширения Xamarin Visual Studio без ошибок для нового пользователя.

3. Если расширения загружены правильно, проблема, скорее всего, вызвана некоторыми из сохраненных параметров для исходного пользователя:

    - **В обозревателе** — _% LocalAppData%\\Microsoft\\VisualStudio\\1\*. 0_
    - **В редакторе реестра** — _HKEY\_CURRENT\_пользователь\\Software\\Microsoft\\VisualStudio\\1\*. 0_
    - **В редакторе реестра** — _HKEY\_CURRENT\_пользователь\\Software\\Microsoft\\VisualStudio\\1\*. 0\_config_

4. Если эти сохраненные параметры действительно выглядят как проблема, можно попытаться выполнить резервное копирование и удалить их.
