---
title: Советы по устранению неполадок
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 56137ACA-4811-B312-6860-E16D0FA123F7
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/15/2018
ms.openlocfilehash: 6a5c06d64a06d8b1a7bcbf32e99905494cd06180
ms.sourcegitcommit: c9651cad80c2865bc628349d30e82721c01ddb4a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/03/2019
ms.locfileid: "70225492"
---
# <a name="troubleshooting-tips"></a>Советы по устранению неполадок


## <a name="getting-diagnostic-information"></a>Получение диагностических сведений

В Xamarin. Android есть несколько мест, которые следует рассмотреть при отслеживании различных ошибок.
Сюда входит следующее.

1. Выходные данные диагностики MSBuild.
2. Журналы развертывания устройств.
3. Выходные данные журнала отладки Android.


<a name="Diagnostic_MSBuild_Output" />

## <a name="diagnostic-msbuild-output"></a>Выходные данные диагностики MSBuild

Диагностическая программа MSBuild может содержать дополнительные сведения, относящиеся к сборке пакетов, и может содержать некоторые сведения о развертывании пакета.

Чтобы включить вывод диагностики MSBuild в Visual Studio, выполните следующие действия:

1. Щелкните **сервис > параметры...**
2. В представлении дерева слева выберите **проекты и решения > сборка и запуск** .
3. На правой панели задайте для раскрывающегося списка детализации выходных данных сборки MSBuild значение диагностика.
4. Нажмите кнопку **ОК**.
5. Выполните очистку и перестройку пакета.
6. Диагностические данные отображаются на панели вывода.


Чтобы включить диагностические выходные данные MSBuild в Visual Studio для Mac/OS X:

1. Щелкните **Visual Studio для Mac > предпочтения...**
2. В представлении дерева слева выберите **проекты > сборка** .
3. На правой панели задайте для раскрывающегося списка подробный журнал значение диагностика.
4. Нажмите кнопку **ОК**.
5. Перезапустите Visual Studio для Mac.
6. Выполните очистку и перестройку пакета.
7. Диагностические данные отображаются на панели ошибок (**просмотрите > pad > ошибки** ), нажав кнопку выходные данные построения.




## <a name="device-deployment-logs"></a>Журналы развертывания устройств

Чтобы включить ведение журнала развертывания устройств в Visual Studio, выполните следующие действия.

1. **Сервис > Параметры...** >
2. В представлении дерева слева выберите **Параметры Xamarin > Android** .
3. На панели справа включите **ведение журнала отладки расширения [X] (запись monodroid. log на Рабочий стол)** .
4. Сообщения журнала записываются в файл monodroid. log на рабочем столе.


Visual Studio для Mac всегда записывает журналы развертывания устройств. Их поиск немного сложнее; файл журнала *андроидутилс* создается для каждого дня и времени, когда происходит развертывание, например: **Андроидтулс-2012-10 -24 _12-35 -45. log**.

- В Windows файлы журнала записываются в `%LOCALAPPDATA%\XamarinStudio-{VERSION}\Logs`.
- В OS X файлы журнала записываются в `$HOME/Library/Logs/XamarinStudio-{VERSION}`.




## <a name="android-debug-log-output"></a>Выходные данные журнала отладки Android

Android запишет много сообщений в [журнал отладки Android](~/android/deploy-test/debugging/android-debug-log.md).
Xamarin. Android использует свойства системы Android для управления созданием дополнительных сообщений в журнале отладки Android. Свойства системы Android можно задать с помощью команды *сбой setprop* в [Android Debug Bridge (ADB)](https://developer.android.com/guide/developing/tools/adb.html):

```shell
adb shell setprop PROPERTY_NAME PROPERTY_VALUE
```

Свойства системы считываются во время запуска процесса и поэтому должны быть установлены перед запуском приложения или после изменения системных свойств.



### <a name="xamarinandroid-system-properties"></a>Системные свойства Xamarin.Android

Xamarin. Android поддерживает следующие системные свойства:

- *Отладка. Mono. Debug*: Если не пустая строка, это эквивалентно значению `*mono-debug*`.

- *Debug. Mono. env*: Разделенный *|* вертикальным каналом список переменных среды для экспорта во время запуска приложения *перед* инициализацией Mono. Это позволяет задать переменные среды, управляющие ведением журнала Mono.

  - *Примечание*. *|* Так как значение разделено, оно должно иметь дополнительный уровень заключения в кавычки \`, так как команда *оболочки* \` ADB удалит набор кавычек.

  - *Примечание*. Длина значений системных свойств Android не может превышать 92 символов.

  - Пример

    ```
    adb shell setprop debug.mono.env "'MONO_LOG_LEVEL=info|MONO_LOG_MASK=asm'"
    ```

- *Отладка. Mono. log*: Список компонентов, разделенных запятыми (" *,* "), которые должны выводить дополнительные сообщения в журнал отладки Android. По умолчанию ничего не задано. Компоненты включают:

  - *все*: Печать всех сообщений
  - *GC*: Печать сообщений, связанных с GC.
  - *Греф*: Печать (слабые, глобальные) ссылки на выделение и освобождение сообщений.
  - *лреф*: Печать сообщений о выделении и освобождении локальных ссылок.

  *Примечание*. это *чрезвычайно* подробная информация. Не включайте, если вам действительно не требуется.

- *Отладка. Mono. Trace*: Позволяет задать параметр [Mono--Trace](http://docs.go-mono.com/?link=man%3amono(1)) `=PROPERTY_VALUE` .

## <a name="deleting-bin-and-obj"></a>Удаление `bin` и`obj`

Xamarin. Android в прошлом пострадал от такой ситуации:

- Возникает Странная ошибка сборки или времени выполнения.
- Вы `Clean` `bin` `obj` , `Rebuild`или вручную удалите каталоги и.
- Проблема исчезает.

Мы сильно инвестированы в устранение таких проблем, как это связано с их влиянием на производительность разработчиков.

Если проблема возникает следующим образом:

1. Обратите внимание на оценку. Что было последним действием, которое получило проект в этом состоянии?
1. Сохраните текущий журнал сборки. Повторите попытку сборки и запишите [журнал диагностической сборки](#diagnostic-msbuild-output).
1. Отправка [отчета об ошибке][bug].

Прежде чем удалять `bin` каталоги `obj` и, заархивируйте их в архив и сохраните их для последующей диагностики при необходимости. Вы, вероятно, `Clean` просто можете воспользоваться проектом приложения Xamarin. Android, чтобы снова работать.

[bug]: https://github.com/xamarin/xamarin-android/wiki/Submitting-Bugs,-Feature-Requests,-and-Pull-Requests

## <a name="xamarinandroid-cannot-resolve-systemvaluetuple"></a>Xamarin. Android не удается разрешить System. ValueTuple

Эта ошибка возникает из-за несовместимости с Visual Studio.

- **Обновление 1 для Visual Studio 2017** (версия 15,1 или более старая) совместима только с **System. ValueTuple NuGet 4.3.0** (или более ранней версии).

- **Visual Studio 2017 с обновлением 2** (версия 15,2 или более поздняя) совместима только с **System. ValueTuple NuGet 4.3.1** (или более поздней версии).

Выберите правильный System. ValueTuple NuGet, соответствующий установке Visual Studio 2017.


## <a name="gc-messages"></a>Сообщения GC

Сообщения компонента GC можно просмотреть, задав для системного свойства Debug. моно. log значение, содержащее GC.

Сообщения GC создаются всякий раз, когда выполняется GC, и предоставляет сведения о том, сколько работы выполняла GC:

```shell
I/monodroid-gc(12331): GC cleanup summary: 81 objects tested - resurrecting 21.
```

Дополнительные сведения о GC, такие как сведения о времени, можно создать `MONO_LOG_LEVEL` , задав для `debug`переменной среды следующее значение:

```shell
adb shell setprop debug.mono.env MONO_LOG_LEVEL=debug
```

Это приведет к появлению (множество) дополнительных сообщений Mono, включая следующие три последствия:

```shell
D/Mono (15723): GC_BRIDGE num-objects 1 num_hash_entries 81226 sccs size 81223 init 0.00ms df1 285.36ms sort 38.56ms dfs2 50.04ms setup-cb 9.95ms free-data 106.54ms user-cb 20.12ms clenanup 0.05ms links 5523436/5523436/5523096/1 dfs passes 1104 6883/11046605
D/Mono (15723): GC_MINOR: (Nursery full) pause 2.01ms, total 287.45ms, bridge 225.60 promoted 0K major 325184K los 1816K
D/Mono ( 2073): GC_MAJOR: (user request) pause 2.17ms, total 2.47ms, bridge 28.77 major 576K/576K los 0K/16K
```

`GC_BRIDGE` В `num_hash_entries` сообщении — число объектов моста, рассматриваемых в ходе этого прохода, а — количество объектов, обработанных в ходе этого вызова кода моста. `num-objects`

`GC_MAJOR` `bridge` В сообщениях `GC_MINOR` и — это период времени, в течение которого работа в мире приостановлена (потоки не выполняются), а — время, затраченное на код обработки моста (который работает с виртуальной машиной Java). `total` Во время обработки моста этот мир *не* приостанавливается.

 *В целом*, чем больше значение `num_hash_entries`, тем `bridge` больше времени займет коллекция, тем больше `total` времени, затраченного на сбор, будет.



## <a name="global-reference-messages"></a>Глобальные справочные сообщения

Чтобы включить ведение журнала Global Reference логгиг (Греф), системное свойство *Debug. моно. log* должно содержать *Греф*, например:

```shell
adb shell setprop debug.mono.log gref
```

Xamarin. Android использует глобальные ссылки Android для предоставления сопоставлений между экземплярами Java и связанными управляемыми экземплярами, как при вызове метода Java. экземпляр Java необходимо предоставить Java.

К сожалению, Эмуляторы Android допускают наличие только 2000 глобальных ссылок за раз. На оборудовании существует более высокий предел в 52000 глобальных ссылок. Нижняя граница может быть проблематичной при запуске приложений в эмуляторе, поэтому знание того, *откуда* поступил экземпляр, может оказаться очень полезным.

 *Примечание*. глобальный счетчик ссылок является внутренним по отношению к Xamarin. Android, а не (и не может) включать глобальные ссылки, взятые другими собственными библиотеками, загруженными в процесс. Использование глобального счетчика ссылок в качестве оценки.

```shell
I/monodroid-gref(12405): +g+ grefc 108 gwrefc 0 obj-handle 0x40517468/L -> new-handle 0x40517468/L from    at Java.Lang.Object.RegisterInstance(IJavaObject instance, IntPtr value, JniHandleOwnership transfer)
I/monodroid-gref(12405):    at Java.Lang.Object.SetHandle(IntPtr value, JniHandleOwnership transfer)
I/monodroid-gref(12405):    at Java.Lang.Object..ctor(IntPtr handle, JniHandleOwnership transfer)
I/monodroid-gref(12405):    at Java.Lang.Thread+RunnableImplementor..ctor(System.Action handler, Boolean removable)
I/monodroid-gref(12405):    at Java.Lang.Thread+RunnableImplementor..ctor(System.Action handler)
I/monodroid-gref(12405):    at Android.App.Activity.RunOnUiThread(System.Action action)
I/monodroid-gref(12405):    at Mono.Samples.Hello.HelloActivity.UseLotsOfMemory(Android.Widget.TextView textview)
I/monodroid-gref(12405):    at Mono.Samples.Hello.HelloActivity.<OnCreate>m__3(System.Object o)
I/monodroid-gref(12405): handle 0x40517468; key_handle 0x40517468: Java Type: `mono/java/lang/RunnableImplementor`; MCW type: `Java.Lang.Thread+RunnableImplementor`
I/monodroid-gref(12405): Disposing handle 0x40517468
I/monodroid-gref(12405): -g- grefc 107 gwrefc 0 handle 0x40517468/L from    at Java.Lang.Object.Dispose(System.Object instance, IntPtr handle, IntPtr key_handle, JObjectRefType handle_type)
I/monodroid-gref(12405):    at Java.Lang.Object.Dispose()
I/monodroid-gref(12405):    at Java.Lang.Thread+RunnableImplementor.Run()
I/monodroid-gref(12405):    at Java.Lang.IRunnableInvoker.n_Run(IntPtr jnienv, IntPtr native__this)
I/monodroid-gref(12405):    at System.Object.c200fe6f-ac33-441b-a3a0-47659e3f6750(IntPtr , IntPtr )
I/monodroid-gref(27679): +w+ grefc 1916 gwrefc 296 obj-handle 0x406b2b98/G -> new-handle 0xde68f4bf/W from take_weak_global_ref_jni
I/monodroid-gref(27679): -w- grefc 1915 gwrefc 294 handle 0xde691aaf/W from take_global_ref_jni
```

Существует четыре сообщения о последствиях.

- Создание глобальной ссылки: это строки, начинающиеся с *+ g +* , которые обеспечивают трассировку стека для создания пути кода.
- Уничтожение глобальной ссылки. это строки, которые начинаются с *-g-* и могут предоставлять трассировку стека для удаления пути кода глобальной ссылки. Если сборщик мусора удаляет Греф, трассировка стека не предоставляется.
- Создание слабой глобальной ссылки. это строки, начинающиеся с *+ w +* .
- Уничтожение ненадежной глобальной ссылки: это строки, начинающиеся с *-w-* .


Во всех сообщениях значение *грефк* — это число глобальных ссылок, созданных Xamarin. Android, а значение *грефвк* — число слабых глобальных ссылок, созданных Xamarin. Android. Значение обработчика или *obj-Handle* является значением обработчика JNI, а символ после " */* " является типом значения Handle: */l* для локальной ссылки, */g* для глобальных ссылок и */w* для слабых глобальных ссылок.

В рамках процесса сборки мусора глобальные ссылки (+ g +) преобразуются в слабые глобальные ссылки (что вызывает + w + и-g-), выполняется сборка мусора на стороне Java, а затем проверяется наличие слабой глобальной ссылки, чтобы определить, была ли она собрана. Если это все еще активно, то для слабой ссылки (+ g +,-w-) создается новый Греф, в противном случае слабая ссылка уничтожается (-w).

## <a name="java-instance-is-created-and-wrapped-by-a-mcw"></a>Экземпляр Java создается и упаковывается с помощью MCW

```shell
I/monodroid-gref(27679): +g+ grefc 2211 gwrefc 0 obj-handle 0x4066df10/L -> new-handle 0x4066df10/L from ...
I/monodroid-gref(27679): handle 0x4066df10; key_handle 0x4066df10: Java Type: `android/graphics/drawable/TransitionDrawable`; MCW type: `Android.Graphics.Drawables.TransitionDrawable`
```

## <a name="a-gc-is-being-performed"></a>Выполняется сборка мусора...

```shell
I/monodroid-gref(27679): +w+ grefc 1953 gwrefc 259 obj-handle 0x4066df10/G -> new-handle 0xde68f95f/W from take_weak_global_ref_jni
I/monodroid-gref(27679): -g- grefc 1952 gwrefc 259 handle 0x4066df10/G from take_weak_global_ref_jni
```

## <a name="object-is-still-alive-as-handle--null"></a>Объект по-прежнему активен, как Handle! = null
## <a name="wref-turned-back-into-a-gref"></a>ВРЕФ был возвращен в Греф

```shell
I/monodroid-gref(27679): *try_take_global obj=0x4976f080 -> wref=0xde68f95f handle=0x4066df10
I/monodroid-gref(27679): +g+ grefc 1930 gwrefc 39 obj-handle 0xde68f95f/W -> new-handle 0x4066df10/G from take_global_ref_jni
I/monodroid-gref(27679): -w- grefc 1930 gwrefc 38 handle 0xde68f95f/W from take_global_ref_jni
```

## <a name="object-is-dead-as-handle--null"></a>Объект не существует, так как Handle = = null
## <a name="wref-is-freed-no-new-gref-created"></a>ВРЕФ освобожден, новый Греф не создан

```shell
I/monodroid-gref(27679): *try_take_global obj=0x4976f080 -> wref=0xde68f95f handle=0x0
I/monodroid-gref(27679): -w- grefc 1914 gwrefc 296 handle 0xde68f95f/W from take_global_ref_jni
```

Здесь есть один интересный вринкле: на целевых объектах под управлением Android до 4,0 значение Греф равно адресу объекта Java в памяти среды выполнения Android. (Т. е. сборщик мусора является не перемещающимся, консервативным, сборщиком и передает прямые ссылки на эти объекты.) Таким образом, после + g +, + w +,-g-, + g +,-w-Sequence результирующий Греф будет иметь то же значение, что и исходное значение Греф. Это делает греппинг через журналы довольно простыми.

В Android 4,0 есть перемещаемый сборщик, который больше не передает прямые ссылки на объекты виртуальных машин среды выполнения Android. Следовательно, после + g +, + w +,-g-, + g +,-w-Sequence значение Греф *будет другим*. Если объект выходит из нескольких сборок мусора, он помещается в несколько Греф значений, что усложняет определение места, из которого фактически был выделен экземпляр.

### <a name="querying-programmatically"></a>Выполнение запросов программным способом

Вы можете запросить счетчики Греф и ВРЕФ, выполнив запрос к `JniRuntime` объекту.

`Java.Interop.JniRuntime.CurrentRuntime.GlobalReferenceCount`— Глобальное число ссылок

`Java.Interop.JniRuntime.CurrentRuntime.WeakGlobalReferenceCount`-Счетчик слабых ссылок



## <a name="android-debug-logs"></a>Журналы отладки Android

[Журналы отладки Android](~/android/deploy-test/debugging/android-debug-log.md) могут предоставлять дополнительный контекст в зависимости от наблюдаемых ошибок времени выполнения.



## <a name="floating-point-performance-is-terrible"></a>Производительность операций с плавающей точкой — плохой!

Кроме того, "мое приложение работает 10 раз быстрее с отладочной сборкой, чем с построением выпуска!"

Xamarin. Android поддерживает несколько Abiов устройств: *ARMEABI*, *ARMEABI-V7A*и *x86*. ABI устройства можно указать в **свойствах проекта > вкладке "приложение" > поддерживаемых архитектур**.

В отладочных сборках используется пакет Android, который предоставляет все ABI, поэтому для целевого устройства будет использоваться самый быстрый ABI.

Сборки выпуска будут включать только ABI, выбранные на вкладке Свойства проекта. Можно выбрать более одного.

*ARMEABI* является интерфейсом ABI по умолчанию и обладает широкими возможностями поддержки устройств. *Однако*ARMEABI не поддерживает многопроцессорные устройства и аппаратные операции с плавающей запятой, амонт другие вещи. Следовательно, приложения, использующие среду выполнения ARMEABI Release, будут привязаны к одному ядру и будут использовать реализацию Soft-float. Оба они могут значительно снизить производительность приложения.

Если приложению требуется пользовательская производительность с плавающей запятой (например, игры), следует включить ABI *ARMEABI-V7A* . Может потребоваться поддержка только среды выполнения *ARMEABI-V7A* , хотя это означает, что старые устройства, которые поддерживают только *ARMEABI* , не смогут запустить приложение.



## <a name="could-not-locate-android-sdk"></a>Не удалось выполнить обнаружение пакет SDK для Android

Для пакет SDK для Android Windows доступно 2 скачивания из Google.
При выборе установщика exe будут записаны разделы реестра, указывающие Xamarin. Android на место установки. Если вы выберете ZIP-файл и распакуйте его самостоятельно, Xamarin. Android не узнает, где искать пакет SDK. Вы можете сообщить Xamarin. Android, где находится пакет SDK, в Visual Studio, перейдя в **меню сервис > параметры > Xamarin > Android Settings**:

[![пакет SDK для Android расположение в параметрах Xamarin Android](troubleshooting-images/01.png)](troubleshooting-images/01.png#lightbox)



## <a name="ide-does-not-display-target-device"></a>В интегрированной среде разработки не отображается целевое устройство

Иногда вы попытаетесь развернуть приложение на устройстве, но устройство, которое вы хотите развернуть, не отображается в диалоговом окне Выбор устройства. Это может произойти, если Android Debug Bridge решил продвигаться на отпуск.

Чтобы диагностировать эту проблему, найдите [программу ADB](~/android/deploy-test/debugging/android-debug-log.md), а затем выполните следующую команду:

```shell
adb devices
```

Если устройство отсутствует, необходимо перезапустить сервер Android Debug Bridge, чтобы можно было найти устройство:

```shell
adb kill-server
adb start-server
```

Программное обеспечение для синхронизации HTC может препятствовать правильной работе **Start-Server ADB** . Если команда " **Start-Server" ADB** не выводит порт, на котором он запускается, выйдите из программы синхронизации HTC и попробуйте перезапустить сервер ADB.


## <a name="the-specified-task-executable-keytool-could-not-be-run"></a>Не удалось запустить указанную задачу "keytool"

Это означает, что путь не содержит каталог, в котором находится каталог bin пакета SDK для Java. Проверьте, выполнены ли эти действия из руководства по [установке](~/android/get-started/installation/index.md) .


## <a name="monodroidexe-or-aresgenexe-exited-with-code-1"></a>monodroid. exe или аресжен. exe завершил работу с кодом 1

Чтобы помочь вам в отладке этой проблемы, перейдите в Visual Studio и измените уровень детализации MSBuild. для этого выберите: **Средства > параметры > проект** и **решения > сборки** и **запуска > подробные сведения о выводе построения проекта MSBuild** и присвойте этому значению значение **Обычная**.

Перестройте и проверьте область вывода Visual Studio, которая должна содержать полную ошибку.

## <a name="there-is-not-enough-storage-space-on-the-device-to-deploy-the-package"></a>На устройстве недостаточно дискового пространства для развертывания пакета

Это происходит, когда вы не запускаете эмулятор из среды Visual Studio. При запуске эмулятора вне среды Visual Studio необходимо передать `-partition-size 512` параметры, например

```shell
emulator -partition-size 512 -avd MonoDroid
```

Убедитесь, что вы используете правильное имя симулятора, т. е. [имя, которое использовалось при настройке симулятора](~/android/get-started/installation/windows.md#device).


## <a name="install_failed_invalid_apk-when-installing-a-package"></a>При\_установке\_пакета\_не удалось установить недопустимый apk

Имена пакетов Android *должны* содержать точку (" *.* "). Измените имя пакета, чтобы оно содержало точку.

- В Visual Studio:
  - Щелкните правой кнопкой мыши проект > Свойства
  - Щелкните вкладку манифест Android в левой части экрана.
  - Обновите поле имя пакета.
    - Если отображается сообщение &ldquo;No AndroidManifest. XML не найдено. Щелкните, чтобы добавить его. &rdquo;, щелкните ссылку, а затем обновите поле имя пакета.
- В пределах Visual Studio для Mac:
  - Щелкните проект правой кнопкой мыши > Параметры.
  - Перейдите к разделу сборка или приложение Android.
  - Измените значение в поле имя пакета, чтобы оно содержало ".".




## <a name="install_failed_missing_shared_library-when-installing-a-package"></a>При\_установке\_пакета\_не удалось удалить общую библиотеку\_

"Общая библиотека" в этом контексте *не* является файлом собственной общей библиотеки (*libfoo.so*); Вместо этого используется библиотека, которая должна быть отдельно установлена на целевом устройстве, например в Google Maps.

Пакет Android определяет, какие общие библиотеки требуются для `<uses-library/>` элемента. Если на целевом устройстве отсутствует требуемая библиотека `//uses-library/@android:required` (например, *true*, то есть значение по умолчанию), установка пакета завершится с ошибкой " *не\_удалось\_\_установить общий доступ".\_ Библиотека*.

Чтобы определить, какие общие библиотеки являются обязательными, просмотрите *созданный*
файл**AndroidManifest. XML** (например, " **obj\\\\Debug\\Android AndroidManifest. XML**") и найдите элемент `<uses-library/>` элементы. `<uses-library/>`элементы можно добавить вручную в файл **свойств\\AndroidManifest. XML** проекта и с помощью [настраиваемого атрибута усеслибраряттрибуте](xref:Android.App.UsesLibraryAttribute).

Например, Добавление ссылки на сборку в *Mono. Android. гуглемапс. dll* приведет к неявному добавлению `<uses-library/>` для общей библиотеки Google Maps.



## <a name="install_failed_update_incompatible-when-installing-a-package"></a>Не\_удалось\_установитьОбновление\_, несовместимое с установкой пакета

У пакетов Android есть три требования:

- Они должны содержать "." (см. предыдущий элемент)
- Они должны иметь уникальное имя строкового пакета (поэтому в именах приложений Android, например, com. Android. Chrome для приложения Chrome, рассматривается соглашение о обратных доменах)
- При обновлении пакетов пакет должен иметь один и тот же ключ подписывания.

Таким образом, представьте себе этот сценарий:

1. Вы создаете & развернуть приложение в качестве отладочного приложения.
2. Вы изменяете ключ подписывания, например, для использования в качестве приложения выпуска (или, если вы не хотите использовать ключ подписывания по умолчанию).
3. Вы устанавливаете приложение, не удаляя его сначала, например, Отладка > Запуск без отладки в Visual Studio.


В этом случае установка пакета завершится ошибкой, если не\_удалось\_установить\_несовместимое с обновлением сообщение об ошибке, так как имя пакета не изменилось во время выполнения ключа подписывания. [Журнал отладки Android](~/android/deploy-test/debugging/android-debug-log.md) также будет содержать примерно следующее сообщение:

```shell
E/PackageManager(  146): Package [PackageName] signatures do not match the previously installed version; ignoring!
```

Чтобы устранить эту ошибку, полностью удалите приложение с устройства, прежде чем выполнять повторную установку.


## <a name="install_failed_uid_changed-when-installing-a-package"></a>При установке\_пакета\_изменилсянеудачныйUID\_

При установке пакета Android ему назначается *идентификатор пользователя* (UID).
*Иногда*, по неизвестным причинам, при установке в уже установленном приложении установка завершается с `INSTALL_FAILED_UID_CHANGED`ошибкой:

```shell
ERROR [2015-03-23 11:19:01Z]: ANDROID: Deployment failed
Mono.AndroidTools.InstallFailedException: Failure [INSTALL_FAILED_UID_CHANGED]
   at Mono.AndroidTools.Internal.AdbOutputParsing.CheckInstallSuccess(String output, String packageName)
   at Mono.AndroidTools.AndroidDevice.<>c__DisplayClass2c.<InstallPackage>b__2b(Task`1 t)
   at System.Threading.Tasks.ContinuationTaskFromResultTask`1.InnerInvoke()
   at System.Threading.Tasks.Task.Execute()
```

Чтобы обойти эту ошибку, *полностью удалите* пакет Android, установив приложение из графического пользовательского интерфейса целевого устройства Android или используя `adb`:

```shell
$ adb uninstall @PACKAGE_NAME@
```

**не используйте** , так как это сохранит данные приложения и, таким образом, сохранит конфликтующий идентификатор UID на целевом устройстве. `adb uninstall -k`



## <a name="release-apps-fail-to-launch-on-device"></a>Не удается запустить приложения выпуска на устройстве

Будет ли вывод журнала отладки Android содержать примерно следующее сообщение:

```shell
D/AndroidRuntime( 1710): Shutting down VM
W/dalvikvm( 1710): threadid=1: thread exiting with uncaught exception (group=0xb412f180)
E/AndroidRuntime( 1710): FATAL EXCEPTION: main
E/AndroidRuntime( 1710): java.lang.UnsatisfiedLinkError: Couldn't load monodroid: findLibrary returned null
E/AndroidRuntime( 1710):        at java.lang.Runtime.loadLibrary(Runtime.java:365)
```

В этом случае возможны две причины.

1. APK не предоставляет ABI, поддерживаемый целевым устройством.
    Например, APK содержит только двоичные файлы ARMEABI-V7A, а целевое устройство поддерживает только ARMEABI.

2. [Ошибка Android](http://code.google.com/p/android/issues/detail?id=21670). В этом случае удалите приложение, перекрестно пальцы и переустановите приложение.

Чтобы исправить (1), измените параметры или свойства проекта, а также [добавьте поддержку требуемого интерфейса ABI в список поддерживаемых ABI](~/android/app-fundamentals/cpu-architectures.md). Чтобы определить, какой ABI необходимо добавить, выполните следующую команду ADB на целевом устройстве:

```shell
adb shell getprop ro.product.cpu.abi
adb shell getprop ro.product.cpu.abi2
```

Выходные данные будут содержать первичный (и необязательный дополнительный) ABI.

```shell
$ adb shell getprop | grep ro.product.cpu
[ro.product.cpu.abi2]: [armeabi]
[ro.product.cpu.abi]: [armeabi-v7a]
```

## <a name="the-outpath-property-is-not-set-for-project-ldquomyappcsprojrdquo"></a>Свойство свойства Path не задано для проекта &ldquo;MyApp. csproj&rdquo;

Обычно это означает, что у вас есть компьютер HP, а &ldquo;для&rdquo; платформы переменных среды задано нечто вроде MCD или ХПД. &ldquo;Это противоречит свойству платформы MSBuild, которое обычно имеет значение любой ЦП&rdquo; или &ldquo;x86&rdquo;. Необходимо удалить эту переменную среды с компьютера, прежде чем MSBuild сможет работать:

- Панель управления > Системные > Дополнительные переменные среды >

Перезапустите Visual Studio или Visual Studio для Mac и попытайтесь выполнить перестроение. Теперь все должно работать правильно.

## <a name="javalangclasscastexception-monoandroidruntimejavaobject-cannot-be-cast-to"></a>Java. lang. Класскастексцептион: Mono. Android. Runtime. Жаваобжект не может быть приведен к...

Xamarin. Android 4. x неправильно маршалирует вложенные универсальные типы должным образом. Например, рассмотрим следующий код на языке\# C, использующий [симпликспандаблелистадаптер](xref:Android.Widget.SimpleExpandableListAdapter):


```csharp
// BAD CODE; DO NOT USE
var groupData = new List<IDictionary<string, object>> () {
        new Dictionary<string, object> {
                { "NAME", "Group 1" },
                { "IS_EVEN", "This group is odd" },
        },
};
var childData = new List<IList<IDictionary<string, object>>> () {
        new List<IDictionary<string, object>> {
                new Dictionary<string, object> {
                        { "NAME", "Child 1" },
                        { "IS_EVEN", "This group is odd" },
                },
        },
};
mAdapter = new SimpleExpandableListAdapter (
        this,
        groupData,
        Android.Resource.Layout.SimpleExpandableListItem1,
        new string[] { "NAME", "IS_EVEN" },
        new int[] { Android.Resource.Id.Text1, Android.Resource.Id.Text2 },
        childData,
        Android.Resource.Layout.SimpleExpandableListItem2,
        new string[] { "NAME", "IS_EVEN" },
        new int[] { Android.Resource.Id.Text1, Android.Resource.Id.Text2 }
);
```


Проблема заключается в том, что Xamarin. Android неправильно маршалирует вложенные универсальные типы. Объект `List<IDictionary<string, object>>` упаковывается в [Java. lang. арррайлист](xref:Java.Util.ArrayList), но `ArrayList` содержит `mono.android.runtime.JavaObject` экземпляры (которые ссылаются на `Dictionary<string, object>` экземпляры) вместо того, что реализует [Java. util. Map](xref:Java.Util.IMap), Это приводит к возникновению следующего исключения:

```shell
E/AndroidRuntime( 2991): FATAL EXCEPTION: main
E/AndroidRuntime( 2991): java.lang.ClassCastException: mono.android.runtime.JavaObject cannot be cast to java.util.Map
E/AndroidRuntime( 2991):        at android.widget.SimpleExpandableListAdapter.getGroupView(SimpleExpandableListAdapter.java:278)
E/AndroidRuntime( 2991):        at android.widget.ExpandableListConnector.getView(ExpandableListConnector.java:446)
E/AndroidRuntime( 2991):        at android.widget.AbsListView.obtainView(AbsListView.java:2271)
E/AndroidRuntime( 2991):        at android.widget.ListView.makeAndAddView(ListView.java:1769)
E/AndroidRuntime( 2991):        at android.widget.ListView.fillDown(ListView.java:672)
E/AndroidRuntime( 2991):        at android.widget.ListView.fillFromTop(ListView.java:733)
E/AndroidRuntime( 2991):        at android.widget.ListView.layoutChildren(ListView.java:1622)
```

Обходной путь заключается в использовании предоставленных [типов коллекций Java](~/android/internals/api-design.md) вместо `System.Collections.Generic` типов &ldquo;внутренних&rdquo; типов. Это приведет к появлению соответствующих типов Java при упаковке экземпляров. (Следующий код является более сложным, чем требуется для сокращения времени жизни Греф. Можно упростить изменение исходного кода с помощью `s/List/JavaList/g` , а `s/Dictionary/JavaDictionary/g` также если Греф время жизни не беспокойтесь.)

```csharp
// insert good code here
using (var groupData = new JavaList<IDictionary<string, object>> ()) {
    using (var groupEntry = new JavaDictionary<string, object> ()) {
        groupEntry.Add ("NAME", "Group 1");
        groupEntry.Add ("IS_EVEN", "This group is odd");
        groupData.Add (groupEntry);
    }
    using (var childData = new JavaList<IList<IDictionary<string, object>>> ()) {
        using (var childEntry = new JavaList<IDictionary<string, object>> ())
        using (var childEntryDict = new JavaDictionary<string, object> ()) {
            childEntryDict.Add ("NAME", "Child 1");
            childEntryDict.Add ("IS_EVEN", "This child is odd.");
            childEntry.Add (childEntryDict);
            childData.Add (childEntry);
        }
        mAdapter = new SimpleExpandableListAdapter (
            this,
            groupData,
            Android.Resource.Layout.SimpleExpandableListItem1,
            new string[] { "NAME", "IS_EVEN" },
            new int[] { Android.Resource.Id.Text1, Android.Resource.Id.Text2 },
            childData,
            Android.Resource.Layout.SimpleExpandableListItem2,
            new string[] { "NAME", "IS_EVEN" },
            new int[] { Android.Resource.Id.Text1, Android.Resource.Id.Text2 }
        );
    }
}
```

[Это будет исправлено в следующем](https://bugzilla.xamarin.com/show_bug.cgi?id=5401)выпуске.


## <a name="unexpected-nullreferenceexceptions"></a>Непредвиденный исключений NullReferenceException

Время от времени в [журнале отладки Android](~/android/deploy-test/debugging/android-debug-log.md) будет упоминаться исключений NullReferenceException,&rdquo; который &ldquo;не может произойти, или поступил из Mono для кода среды выполнения Android вскоре до выхода из приложения:

```shell
E/mono(15202): Unhandled Exception: System.NullReferenceException: Object reference not set to an instance of an object
E/mono(15202):   at Java.Lang.Object.GetObject (IntPtr handle, System.Type type, Boolean owned)
E/mono(15202):   at Java.Lang.Object._GetObject[IOnTouchListener] (IntPtr handle, Boolean owned)
E/mono(15202):   at Java.Lang.Object.GetObject[IOnTouchListener] (IntPtr handle, Boolean owned)
E/mono(15202):   at Android.Views.View+IOnTouchListenerAdapter.n_OnTouch_Landroid_view_View_Landroid_view_MotionEvent_(IntPtr jnienv, IntPtr native__this, IntPtr native_v, IntPtr native_e)
E/mono(15202):   at (wrapper dynamic-method) object:b039cbb0-15e9-4f47-87ce-442060701362 (intptr,intptr,intptr,intptr)
```

или диспетчер конфигурации служб

```shell
E/mono    ( 4176): Unhandled Exception:
E/mono    ( 4176): System.NullReferenceException: Object reference not set to an instance of an object
E/mono    ( 4176): at Android.Runtime.JNIEnv.NewString (string)
E/mono    ( 4176): at Android.Util.Log.Info (string,string)
```

Это может произойти, когда среда выполнения Android решает прервать процесс, что может происходить по любому ряду причин, включая ограничение Греф целевого объекта или &ldquo;неправильное&rdquo; выполнение в JNI.

Чтобы проверить это, проверьте в журнале отладки Android сообщение от процесса, аналогичное следующему:

```shell
E/dalvikvm(  123): VM aborting
```


## <a name="abort-due-to-global-reference-exhaustion"></a>Прервать из-за нехватки глобальной ссылки

Слой JNI среды выполнения Android поддерживает только ограниченное число ссылок на объекты JNI, которые должны быть действительными в любой заданный момент времени. При превышении этого ограничения происходит прерывание операции.

Ограничение Греф (*Global Reference*) — 2000 ссылок в эмуляторе, а ~ 52000 ссылается на оборудование.

Вы знаете, что вы начинаете создавать слишком много Грефс при появлении таких сообщений в журнале отладки Android:

```shell
D/dalvikvm(  602): GREF has increased to 1801
```

При достижении предельного значения Греф печатается следующее сообщение:

```shell
D/dalvikvm(  602): GREF has increased to 2001
W/dalvikvm(  602): Last 10 entries in JNI global reference table:
W/dalvikvm(  602):  1991: 0x4057eff8 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602):  1992: 0x4057f010 cls=Landroid/graphics/Point; (28 bytes)
W/dalvikvm(  602):  1993: 0x40698e70 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602):  1994: 0x40698e88 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602):  1995: 0x40698ea0 cls=Landroid/graphics/Point; (28 bytes)
W/dalvikvm(  602):  1996: 0x406981f0 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602):  1997: 0x40698208 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602):  1998: 0x40698220 cls=Landroid/graphics/Point; (28 bytes)
W/dalvikvm(  602):  1999: 0x406956a8 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602):  2000: 0x406956c0 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602): JNI global reference table summary (2001 entries):
W/dalvikvm(  602):    51 of Ljava/lang/Class; 164B (41 unique)
W/dalvikvm(  602):    46 of Ljava/lang/Class; 188B (17 unique)
W/dalvikvm(  602):     6 of Ljava/lang/Class; 212B (6 unique)
W/dalvikvm(  602):    11 of Ljava/lang/Class; 236B (7 unique)
W/dalvikvm(  602):     3 of Ljava/lang/Class; 260B (3 unique)
W/dalvikvm(  602):     4 of Ljava/lang/Class; 284B (2 unique)
W/dalvikvm(  602):     8 of Ljava/lang/Class; 308B (6 unique)
W/dalvikvm(  602):     1 of Ljava/lang/Class; 316B
W/dalvikvm(  602):     4 of Ljava/lang/Class; 332B (3 unique)
W/dalvikvm(  602):     1 of Ljava/lang/Class; 356B
W/dalvikvm(  602):     2 of Ljava/lang/Class; 380B (1 unique)
W/dalvikvm(  602):     1 of Ljava/lang/Class; 428B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 452B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 476B
W/dalvikvm(  602):     2 of Ljava/lang/Class; 500B (1 unique)
W/dalvikvm(  602):     1 of Ljava/lang/Class; 548B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 572B
W/dalvikvm(  602):     2 of Ljava/lang/Class; 596B (2 unique)
W/dalvikvm(  602):     1 of Ljava/lang/Class; 692B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 956B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 1004B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 1148B
W/dalvikvm(  602):     2 of Ljava/lang/Class; 1172B (1 unique)
W/dalvikvm(  602):     1 of Ljava/lang/Class; 1316B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 3428B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 3452B
W/dalvikvm(  602):     1 of Ljava/lang/String; 28B
W/dalvikvm(  602):     2 of Ldalvik/system/VMRuntime; 12B (1 unique)
W/dalvikvm(  602):    10 of Ljava/lang/ref/WeakReference; 28B (10 unique)
W/dalvikvm(  602):     1 of Ldalvik/system/PathClassLoader; 44B
W/dalvikvm(  602):  1553 of Landroid/graphics/Point; 20B (1553 unique)
W/dalvikvm(  602):   261 of Landroid/graphics/Point; 28B (261 unique)
W/dalvikvm(  602):     1 of Landroid/view/MotionEvent; 100B
W/dalvikvm(  602):     1 of Landroid/app/ActivityThread$ApplicationThread; 28B
W/dalvikvm(  602):     1 of Landroid/content/ContentProvider$Transport; 28B
W/dalvikvm(  602):     1 of Landroid/view/Surface$CompatibleCanvas; 44B
W/dalvikvm(  602):     1 of Landroid/view/inputmethod/InputMethodManager$ControlledInputConnectionWrapper; 36B
W/dalvikvm(  602):     1 of Landroid/view/ViewRoot$1; 12B
W/dalvikvm(  602):     1 of Landroid/view/ViewRoot$W; 28B
W/dalvikvm(  602):     1 of Landroid/view/inputmethod/InputMethodManager$1; 28B
W/dalvikvm(  602):     1 of Landroid/view/accessibility/AccessibilityManager$1; 28B
W/dalvikvm(  602):     1 of Landroid/widget/LinearLayout$LayoutParams; 44B
W/dalvikvm(  602):     1 of Landroid/widget/LinearLayout; 332B
W/dalvikvm(  602):     2 of Lorg/apache/harmony/xnet/provider/jsse/TrustManagerImpl; 28B (1 unique)
W/dalvikvm(  602):     1 of Landroid/view/SurfaceView$MyWindow; 36B
W/dalvikvm(  602):     1 of Ltouchtest/RenderThread; 92B
W/dalvikvm(  602):     1 of Landroid/view/SurfaceView$3; 12B
W/dalvikvm(  602):     1 of Ltouchtest/DrawingView; 412B
W/dalvikvm(  602):     1 of Ltouchtest/Activity1; 180B
W/dalvikvm(  602): Memory held directly by tracked refs is 75624 bytes
E/dalvikvm(  602): Excessive JNI global references (2001)
E/dalvikvm(  602): VM aborting
```


В приведенном выше примере (который, кстати, поступает из [ошибки 685215](https://bugzilla.novell.com/show_bug.cgi?id=685215)) проблема заключается в том, что слишком много экземпляров Android. Graphics. Point создаются. Список исправлений для этой конкретной ошибки см. в [комментарии \#2](https://bugzilla.novell.com/show_bug.cgi?id=685215#c2) .

Как правило, полезное решение заключается в том, чтобы найти, в каком &ndash; типе слишком много экземпляров выделили Android. &ndash; Graphics. Point в приведенном выше дампе, а затем найдет место, где они создаются в исходном коде, и избавиться от них соответствующим образом (чтобы их Время жизни объекта Java сокращено. Это не всегда уместно (\#685215 является многопоточным, поэтому тривиальное решение позволяет избежать вызова Dispose), но это первое, что следует учитывать.

Вы можете включить [ведение журнала Греф](~/android/troubleshooting/index.md) , чтобы увидеть время создания грефс и количество существующих.


## <a name="abort-due-to-jni-type-mismatch"></a>Прервано из-за несоответствия типов JNI

Если вы вручную JNI код, возможно, типы не будут согласованы правильно, например при попытке вызова `java.lang.Runnable.run` для типа, который не реализует. `java.lang.Runnable` В этом случае в журнале отладки Android будет примерно следующее сообщение:

```shell
W/dalvikvm( 123): JNI WARNING: can't call Ljava/Type;;.method on instance of Lanother/java/Type;
W/dalvikvm( 123):              in Lmono/java/lang/RunnableImplementor;.n_run:()V (CallVoidMethodA)
...
E/dalvikvm( 123): VM aborting
```

## <a name="dynamic-code-support"></a>Поддержка динамического кода

### <a name="dynamic-code-does-not-compile"></a>Динамический код не компилируется

Чтобы использовать в\# приложении или библиотеке динамическую платформу C, необходимо добавить в проект файл System. Core. dll, Microsoft. CSharp. dll и Mono. CSharp. dll.

### <a name="in-release-build-missingmethodexception-occurs-for-dynamic-code-at-run-time"></a>В сборке Release MissingMethodException выполняется для динамического кода во время выполнения.

- Скорее всего, проект приложения не имеет ссылок на System. Core. dll, Microsoft. CSharp. dll или Mono. CSharp. dll. Убедитесь, что имеются ссылки на эти сборки.

  - Помните, что динамический код всегда имеет затраты. Если требуется эффективный код, рекомендуется не использовать динамический код.

- В первой предварительной версии эти сборки были исключены, если в коде приложения явно не используются типы в каждой сборке. Для решения этой проблемы см. следующие сведения.[http://lists.ximian.com/pipermail/mo...il/009798.html](http://lists.ximian.com/pipermail/monodroid/2012-April/009798.html)


## <a name="projects-built-with-aotllvm-crash-on-x86-devices"></a>Проекты, созданные с помощью AOT + LLVM сбой на устройствах x86

При развертывании приложения, созданного с помощью [AOT + LLVM](~/android/deploy-test/release-prep/index.md) на устройствах на базе x86, может появиться сообщение об ошибке, похожее на следующее:

```shell
Assertion: should not be reached at /Users/.../external/mono/mono/mini/tramp-x86.c:124
Fatal signal 6 (SIGABRT), code -6 in tid 4051 (amarin.bug56111)
```

Это известная проблема, о которой сообщается в [56111](https://bugzilla.xamarin.com/show_bug.cgi?id=56111). Обходной путь заключается в отключении LLVM.
