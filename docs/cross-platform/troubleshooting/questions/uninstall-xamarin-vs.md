---
title: Как полностью удалить Xamarin для Visual Studio?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: c1742239-05ea-449d-9c99-611e5e5a90e4
author: conceptdev
ms.author: crdun
ms.date: 12/02/2016
ms.openlocfilehash: 1596e7ed7b3f6d71e13f19a64d111873efb7445c
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2019
ms.locfileid: "70764937"
---
# <a name="how-do-i-perform-a-thorough-uninstall-for-xamarin-for-visual-studio"></a>Как полностью удалить Xamarin для Visual Studio?

1. На панели управления Windows удалите следующие элементы:

    - Xamarin
    - Xamarin для Windows
    - Xamarin.Android
    - Xamarin.iOS
    - Xamarin для Visual Studio

2. В обозревателе удалите все оставшиеся файлы из папок расширения Xamarin Visual Studio (все версии, включая _программные файлы_ и _программные файлы (x86)_ ):

    _C:\\Program Files\\MicrosoftVisualStudio1\\0 Common7IDE\\Extensions\\Xamarin\*\*\\_

3. Удалите также каталог кэша компонента MEF Visual Studio:

    _% LocalAppData%\\0\\\*\\компонентмоделкаче\\Microsoft VisualStudio 1_

    На самом деле этот шаг часто достаточно для разрешения таких ошибок, как:

    - "Пакет" Ксамариншеллпаккаже "не был правильно загружен"

    - "Файл проекта... не удается открыть. Отсутствует подтип проекта "

    - "Ссылка на объект не задана для экземпляра объекта.  в Xamarin. VisualStudio. IOS. Ксамариниоспаккаже. Initialize () "

    - "Сбой SetSite для пакета" (в Visual Studio _ActivityLog. XML_)

    - "Сбой Легациситепаккаже для пакета" (в Visual Studio _ActivityLog. XML_)

    (См. также расширение " [Очистка кэша компонентов MEF](https://visualstudiogallery.msdn.microsoft.com/22b94661-70c7-4a93-9ca3-8b6dd45f47cd) " для Visual Studio.  См. [ошибку 40781, комментарий 19,](https://bugzilla.xamarin.com/show_bug.cgi?id=40781#c19) чтобы получить более подробные сведения о вышестоящей неполадке в Visual Studio, которая может вызвать эти ошибки.)

4. Также проверьте каталог _виртуалсторе_ , чтобы узнать, _\\_ могли ли Windows хранить файлы оверлея для каталогов Xamarin или _компонентмоделкаче_ .

    _% LocalAppData%\\виртуалсторе_

5. Откройте редактор реестра (`regedit`).

6. Найдите следующий раздел:

    _HKey\_\_Локальное\\программное\\обеспечениеWOW6432NodeMicrosoft\\Windows CurrentVersionSharedDlls\\\\\\_

7. Найдите и удалите все записи, соответствующие этому шаблону.

    _C:\\Program Files\\MicrosoftVisualStudio1\\0 Common7IDE\\Extensions\\Xamarin\*\*\\_

8. Найдите следующий раздел.

    _HKey\_текущее\\пользовательское\\программное\\обеспечение Microsoft VisualStudio1\\0екстенсионманажер\\пендингделетионс\*\_\\_

9. Удалите все записи, которые могут иметь отношение к Xamarin.  Вот пример, который используется для того, чтобы вызвать проблемы в старых версиях Xamarin:

    _Моно. VisualStudio. Shell, 1.0_

10. Откройте командную `cmd.exe` строку администратора и `devenv /setup` выполните команды и `devenv /updateconfiguration` для каждой установленной версии Visual Studio.  Например, для Visual Studio 2015:

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
    - **В Regedit** _—\_hKey\_Current\\User Software\\Microsoft\\VisualStudio\*1.0\\_
    - **В Regedit** — _\_hKey\_Current\\user\\Software программа Microsoft\\VisualStudio1\\0\_config\*_

4. Если эти сохраненные параметры действительно выглядят как проблема, можно попытаться выполнить резервное копирование и удалить их.
