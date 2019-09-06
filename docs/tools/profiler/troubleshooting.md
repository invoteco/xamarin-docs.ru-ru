---
title: Устранение неполадок Xamarin Profiler
description: В этом документе содержатся сведения об устранении неполадок, связанных с Xamarin Profiler. Здесь описываются проблемы, связанные с ведением журнала и диагностикой, интегрированной средой разработки и другими разделами.
ms.prod: xamarin
ms.assetid: 0060E9D1-C003-4E4C-ADE8-B406978FE891
author: conceptdev
ms.author: crdun
ms.date: 10/27/2017
ms.openlocfilehash: c6a05e332bf0c08f8c7ea328c2793f7d0bf00fb7
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/04/2019
ms.locfileid: "70285700"
---
# <a name="xamarin-profiler-troubleshooting"></a>Устранение неполадок Xamarin Profiler

## <a name="logging-and-diagnostics"></a>Ведение журналов и диагностика

Команда Xamarin может помочь в отслеживании проблем, если вы предоставляете нам информацию, включая:

- Демонстрационная презентация проблемы, сбоя или сбоя, а также рабочего процесса.
- Выходные данные журнала (см. ниже).
- **MLPD** , формируемый для сеанса профилирования (см. ниже).

### <a name="getting-log-outputs"></a>Получение выходных данных журнала

В журналы Mac сохраняются в `~/Library/Logs/Xamarin.Profiler/Profiler.<date>.log`.

В Windows эти данные сохраняются, `%appdata%Local//Xamarin/Log/Xamarin.Profiler/Profiler.<date>.log` и каждый раз при отправке проблемы следует включать последний журнал.

Мы добавим ведение журнала по мере того, как мы собираемся, чтобы эти выходные данные были увеличены и стали более полезными в течение времени.

<a name="gen_mlpd" />

### <a name="generating-mlpd-files"></a>Создание MLPD файлов

**MLPD** -файл — это сжатый выход профилировщика среды выполнения Mono. Xamarin Profiler графический пользовательский интерфейс считывает данные из **MLPD** и отображает их для пользователя. файлы **. MLPD** являются полезными средствами отладки для Xamarin, так как они помогают нашим инженерам диагностировать проблемы, с которыми может столкнуться профилировщик.

**MLPD** для текущего сеанса автоматически сохраняется в `/tmp` каталоге Mac и может быть идентифицирован по метке времени. Если включить ведение журнала, первый выход будет указывать путь к **MLPD** файлу. Файл **. MLPD** , как правило, сохраняется в каталоге, начиная с ~/Вар/фолдерс...

**MLPD** для текущего сеанса также можно сохранить, выбрав **Файл > Сохранить как...** . из меню профилировщика:

**Visual Studio для Mac**:

![](troubleshooting-images/image17.png "Сохранение файла. MLPD в Visual Studio для Mac")

**Visual Studio**:

![](troubleshooting-images/image17-vs.png "Сохранение файла. MLPD в Visual Studio")

Важно отметить, что **. MLPD** содержит много информации, и размер файла будет большим.

## <a name="troubleshooting"></a>Устранение неполадок

В списке ниже приведены распространенные проблемы, способы их обхода и советы по использованию профилировщика.

> [!NOTE]
> Чтобы разблокировать эту функцию в Visual Studio Enterprise в Windows или Visual Studio для Mac, необходимо быть подписчиком Visual Studio **Enterprise** .

#### <a name="i-cant-see-the-ios-profiler-option-or-it-is-greyed-out-visual-studio-and-visual-studio-for-mac"></a>Не удается увидеть параметр профилировщика iOS, или он неактивен [Visual Studio и Visual Studio для Mac]

Чтобы устранить эту проблему, проверьте следующие параметры:

- Убедитесь, что используется конфигурация отладки
- Убедитесь, что используется сборщик мусора SGen.
- Убедитесь, что платформа [поддерживается](~/tools/profiler/index.md#Profiler_Support).
- Убедитесь, что у вас есть правильная лицензия.
- Убедитесь, что вы вошли в систему и правильно прошли проверку подлинности.
- [Visual Studio] Необходимо использовать [Visual Studio Enterprise](https://visualstudio.microsoft.com/vs/enterprise/) и иметь действительную корпоративную лицензию.

#### <a name="i-get-an-error-when-i-try-to-launch-the-profiler"></a>При попытке запуска профилировщика появляется сообщение об ошибке

Если при использовании профилировщика в Visual Studio вы выпустили эту ошибку:

![](troubleshooting-images/error.png "Поле \"ошибка\" при использовании профилировщика в Visual Studio")

Обычно это связано с невозможностью запуска симулятора или эмулятора. Попробуйте и запустите приложение обычным образом, исправьте возникающие проблемы и попытайтесь снова использовать профилировщик.

#### <a name="to-watch-a-specific-thread"></a>Просмотр определенного потока

Если у вас есть какой-то поток, который вы хотели бы отслеживать, то лучше приступить к именованию потока в самом начале его создания, `ThreadName` чтобы получить `0x0`вместо. Например, чтобы задать имя потока как `UI`, можно использовать следующий код:

```csharp
RunOnUiThread (() => {
  Thread.CurrentThread.Name  = "UI";
});
```

## <a name="related-links"></a>Связанные ссылки

- [Пошаговое руководство. Использование Xamarin Profiler](~/tools/profiler/index.md)
- [Рекомендации по использованию памяти и производительности](~/cross-platform/deploy-test/memory-perf-best-practices.md)
- [Заметки о выпуске](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/profiler/preview/index.md)
