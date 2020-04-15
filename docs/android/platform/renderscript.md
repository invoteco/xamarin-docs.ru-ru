---
title: Введение в Renderscript
description: В этом руководстве вы познакомитесь с Renderscript и узнаете, как использовать встроенный API Renderscript в приложении Xamarin.Android, нацеленном на API уровня 17 или выше.
ms.prod: xamarin
ms.assetid: 378793C7-5E3E-40E6-ABEE-BEAEF64E6A47
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/06/2018
ms.openlocfilehash: 884b69b0cdecf4f979cec314b6440974c5bac97d
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/13/2020
ms.locfileid: "73019798"
---
# <a name="an-introduction-to-renderscript"></a>Введение в Renderscript

_В этом руководстве вы познакомитесь с Renderscript и узнаете, как использовать встроенный API Renderscript в приложении Xamarin.Android, нацеленном на API уровня 17 или выше._

## <a name="overview"></a>Обзор

Платформа программирования Renderscript создана корпорацией Google в целях повышения производительности приложений Android, которые требуют обширных вычислительных ресурсов. Это высокопроизводительный низкоуровневый API, основанный на [C99](https://en.wikipedia.org/wiki/C99). Так как это интерфейс API низкого уровня, который может работать на ЦП, GPU или DSP, Renderscript хорошо подходит для приложений Android, выполняющих любые из следующих задач:

- Графика
- обработка изображений;
- Шифрование
- обработка сигналов;
- математические функции.

Renderscript использует `clang` и компилирует скрипты в байтовый код LLVM, упакованный в формат APK. При первом запуске приложения байтовый код LLVM компилируется в машинный код для конкретных процессоров на устройстве. Такая архитектура позволяет приложению Android использовать все преимущества машинного кода, не вынуждая разработчиков самостоятельно писать код для каждого процессора на используемых устройствах.

Подпрограмма Renderscript содержит два компонента.

1. **Среда выполнения Renderscript** — это собственные API, которые отвечают за выполнение Renderscript. Сюда входят все скрипты Renderscript, написанные для приложения.

2. **Управляемые оболочки из платформы Android** — это управляемые классы, которые позволяют приложению Android управлять средой выполнения и скриптами Renderscript и взаимодействовать с ними. В дополнение к классам, предоставленным платформой для управления средой выполнения Renderscript, цепочка инструментов Android создаст управляемые классы-оболочки для использования в приложении Android, изучив исходный код Renderscript.

На схеме ниже показана взаимосвязь этих компонентов.

![Схема взаимодействия платформы Android со средой выполнения Renderscript](renderscript-images/renderscript-01.png)

Существуют три важных понятия, связанных с использованием скриптов Renderscript в приложении Android.

1. **Контекст** — управляемый API, предоставленный пакетом SDK для Android, который выделяет ресурсы для Renderscript и позволяет приложению Android передавать и принимать данные Renderscript.

2. **_Ядро вычислений_** , _корневое ядро_ или просто _ядро_ — обозначает подпрограмму, которая выполняет работу. Ядро схоже с функцией в C; это подпрограммы с возможностью параллельного выполнения, которая вызывается для всех данных в выделенной памяти.

3. **Распределенная память** — данные передаются в ядро и из него через _[распределение](xref:Android.Renderscripts.Allocation)_ . Ядро может иметь одно входное и (или) одно выходное распределение.

Пространство имен [Android.Renderscripts](xref:Android.Renderscripts) содержит классы для взаимодействия со средой выполнения Renderscript. В частности, класс [`Renderscript`](xref:Android.Renderscripts.RenderScript) управляет жизненным циклом и ресурсами подсистемы Renderscript. Приложение Android должно инициализировать один или несколько объектов [`Android.Renderscripts.Allocation`](xref:Android.Renderscripts.Allocation)
. Управляемый API Распределения отвечает за выделение памяти и доступ к ней для совместного использования приложением Android и средой выполнения Renderscript. Как правило, создается одно распределение для входных данных и при необходимости еще одно для хранения выходных данных ядра. Подсистема среды выполнения Renderscript и связанные управляемые классы-оболочки самостоятельно управляют доступом к памяти распределений, и разработчику приложения Android не нужно ничего с этим делать.

Распределение может содержать один или несколько элементов [Android.Renderscripts.Elements](xref:Android.Renderscripts.Element).
Элементами называют специализированный тип, который описывает данные в каждом распределении.
Типы элементов для выходного распределения должны соответствовать типам входного элемента. При выполнении Renderscript параллельно циклически перебирает все элементы во входном распределении и сохраняет результаты в выходное распределение. Существуют два типа элементов:

- **простой тип**, который концептуально аналогичен типу данных `float` или `char` в C;

- **сложный тип**, который похож на `struct` в C.

Подсистема Renderscript выполнит проверку среды выполнения, чтобы убедиться, что элементы в каждом распределении совместимы с требованиями ядра. Если тип данных для элементов в распределении не соответствует ожидаемому ядром типу, будет создано исключение.

Все ядра Renderscript помещаются в оболочку типа, который наследует от класса [`Android.Renderscripts.Script`](xref:Android.Renderscripts.Script)
. Класс `Script` используется для задания параметров для Renderscript, установки соответствующих `Allocations` и запуска Renderscript. В пакет SDK для Android входят два подкласса `Script`:

- **`Android.Renderscripts.ScriptIntrinsic`** . Некоторые из распространенных задач Renderscript объединены в пакет SDK для Android и доступны в подклассе класса [ScriptIntrinsic](xref:Android.Renderscripts.ScriptIntrinsic). Разработчику не нужно предпринимать дополнительных действий, чтобы использовать эти скрипты в своем приложении, так как они уже предоставлены.

- **`ScriptC_XXXXX`** . Это сценарии, написанные разработчиками и упакованные в APK, также известны как _пользовательские скрипты_. Во время компиляции цепочка инструментов Android создаст управляемые классы-оболочки, которые позволят использовать эти скрипты в приложении Android.
  Имена для создаваемых классов составляются из имени файла Renderscript с префиксом `ScriptC_`. Написание и внедрение пользовательских скриптов официально не поддерживается в Xamarin.Android и не входит в задачи этого руководства.

Из этих двух типов в Xamarin.Android поддерживается только `StringIntrinsic`. В этом руководстве рассказывается, как использовать встроенные скрипты в приложении Xamarin.Android.

## <a name="requirements"></a>Требования

Это руководство применимо к приложениям Xamarin.Android, нацеленным на API уровня 17 или выше. Использование _пользовательских скриптов_ в этом руководстве не рассматривается.

[Библиотека поддержки Xamarin.Android версии 8](https://www.nuget.org/packages/Xamarin.Android.Support.v8.RenderScript/) обеспечивает обратную совместимость API Renderscript с приложениями, которые нацеливаются на более старые версии пакета SDK для Android. Добавление этого пакета в проект Xamarin.Android позволит разрешить приложениям, нацеленным на более старый пакет SDK для Android, использовать встроенные скрипты.

## <a name="using-intrinsic-renderscripts-in-xamarinandroid"></a>Использование встроенных скриптов Renderscript в Xamarin.Android

Встроенные скрипты отлично подходят для выполнения задач с высокой вычислительной нагрузкой, позволяя почти не создавать новый код. Они уже оптимизированы для оптимальной производительности на большом числе разнообразных устройств.
Вполне нормально, что встроенный скрипт выполняется в 10 раз быстрее, чем управляемый код, и примерно в 2–3 раза быстрее пользовательской реализации на C. Встроенные скрипты покрывают широкий спектр типичных вычислительных сценариев. Вот список встроенных скриптов в текущей версии Xamarin.Android:

- [ScriptIntrinsic3DLUT](xref:Android.Renderscripts.ScriptIntrinsic3DLUT) — преобразует RGB в RGBA по трехмерной таблице подстановки. 

- [ScriptIntrinsicBLAS](https://developer.android.com/reference/android/renderscript/ScriptIntrinsicBLAS.html) — предоставляет высокопроизводительные API Renderscript для [BLAS](http://www.netlib.org/blas/). Программы BLAS (Basic Linear Algebra Subprograms) предоставляют стандартные строительные блоки для базовых векторных и матричных операций. 

- [ScriptIntrinsicBlend](xref:Android.Renderscripts.ScriptIntrinsicBlend) — объединяет два распределения.

- [ScriptIntrinsicBlur](xref:Android.Renderscripts.ScriptIntrinsicBlur) — применяет к распределению гауссово размытие.

- [ScriptIntrinsicColorMatrix](xref:Android.Renderscripts.ScriptIntrinsicColorMatrix) — применяет к распределению цветовую матрицу (например, для изменения оттенка или цвета).

- [ScriptIntrinsicConvolve3x3](xref:Android.Renderscripts.ScriptIntrinsicConvolve3x3) — применяет к распределению цветовую матрицу 3×3.

- [ScriptIntrinsicConvolve5x5](xref:Android.Renderscripts.ScriptIntrinsicConvolve5x5) — применяет к распределению цветовую матрицу 5×5.

- [ScriptIntrinsicHistogram](xref:Android.Renderscripts.ScriptIntrinsicHistogram) — встроенный фильтр гистограммы.

- [ScriptIntrinsicLUT](xref:Android.Renderscripts.ScriptIntrinsicLUT) — применяет к буферу таблицу подстановки по каналам.

- [ScriptIntrinsicResize](xref:Android.Renderscripts.ScriptIntrinsicResize) — скрипт для изменения размера двухмерного распределения.

- [ScriptIntrinsicYuvToRGB](xref:Android.Renderscripts.ScriptIntrinsicYuvToRGB) — преобразует буфер YUV в RGB.

Подробные сведения о каждом из встроенных скриптов вы найдете в документации по API.

Далее описаны базовые процедуры для применение Renderscript  в приложении Android.

**Создание контекста выполнения для Renderscript.** Класс [`Renderscript`](xref:Android.Renderscripts.RenderScript)
представляет собой управляемую программу-оболочку для контекста Renderscript, которая берет на себя инициализацию элементов управления, управление ресурсами и очистку. Объект Renderscript создается с помощью фабричного метода `RenderScript.Create`, который принимает в качестве параметра контекст выполнения Android (например, действие). Следующая строка кода демонстрирует инициализацию контекста выполнения Renderscript:

```csharp
Android.Renderscripts.RenderScript renderScript = RenderScript.Create(this);
```

**Создание распределений.** В зависимости от типа встроенного скрипта могут потребоваться одно или два `Allocation`. Класс [`Android.Renderscripts.Allocation`](xref:Android.Renderscripts.Allocation)
предоставляет несколько фабричных методов для помощи в создании экземпляров распределений для встроенных скриптов. В следующем примере кода показано, как создать распределение для растровых изображений.

```csharp
Android.Graphics.Bitmap originalBitmap;
Android.Renderscripts.Allocation inputAllocation = Allocation.CreateFromBitmap(renderScript,
                                                     originalBitmap,
                                                     Allocation.MipmapControl.MipmapFull,
                                                     AllocationUsage.Script);
```

Часто приходится создавать `Allocation` для хранения выходных данных скрипта. Следующий фрагмент кода демонстрирует создание вспомогательного метода `Allocation.CreateTyped` для создания второго экземпляра `Allocation` с тем же типом, что у исходного.

```csharp
Android.Renderscripts.Allocation outputAllocation = Allocation.CreateTyped(renderScript, inputAllocation.Type);
```

**Создание экземпляра программы-оболочки для скрипта.** Каждый из классов-оболочек для встроенных скриптов должен определять вспомогательные методы (обычно их называют `Create`) для создания экземпляра объекта-оболочки для этого скрипта. В следующем фрагменте кода приведен пример создания экземпляра для объекта размытия `ScriptIntrinsicBlur`. Вспомогательный метод `Element.U8_4` создает элемент, который описывает тип данных с 4 8-битовыми полями для хранения целых чисел без знака, что соответствует данным для объекта `Bitmap`:

```csharp
Android.Renderscripts.ScriptIntrinsicBlur blurScript = ScriptIntrinsicBlur.Create(renderScript, Element.U8_4(renderScript));
```

**Назначение распределений, настройка параметров и выполнение скрипта.** Класс `Script` предоставляет метод `ForEach` для фактического выполнения скрипта Renderscript. Этот метод последовательно перебирает все `Element` в `Allocation`, где хранятся входные данные. В некоторых случаях может потребоваться создать еще один `Allocation` для выходных данных.
`ForEach` перезаписывает содержимое выходного распределения. Продолжая работу с фрагментами кода из предыдущих шагов, этот пример демонстрирует назначение входного распределения, настройку параметра и выполнение скрипта (который копирует результаты в выходное распределение).

```csharp
blurScript.SetInput(inputAllocation);
blurScript.SetRadius(25);  // Set a pamaeter
blurScript.ForEach(outputAllocation);
```

Возможно, вас заинтересует рецепт [размытия изображения с помощью Renderscript](https://github.com/xamarin/recipes/tree/master/Recipes/android/other_ux/drawing/blur_an_image_with_renderscript), который содержит полный пример программы с использованием встроенного скрипта Xamarin.Android.

## <a name="summary"></a>Сводка

В этом руководстве представлены базовые сведения о Renderscript и показано, как использовать его в приложении Xamarin.Android. Здесь кратко описана суть Renderscript и взаимодействие с приложением Android. Также здесь описаны некоторые важные компоненты Renderscript и важные различия между _пользовательскими скриптами_ и _встроенными скриптами_. И наконец, обсуждается процесс применения встроенных скриптов в приложении Xamarin.Android.

## <a name="related-links"></a>Связанные ссылки

- [Пространство имен Android.Renderscripts](xref:Android.Renderscripts)
- [Размытие изображения с помощью Renderscript](https://github.com/xamarin/recipes/tree/master/Recipes/android/other_ux/drawing/blur_an_image_with_renderscript)
- [Renderscript](https://developer.android.com/guide/topics/renderscript/compute.html)
- [Учебник. Начало работы с Renderscript](https://software.intel.com/en-us/articles/renderscript-basic-sample-for-android-os)
