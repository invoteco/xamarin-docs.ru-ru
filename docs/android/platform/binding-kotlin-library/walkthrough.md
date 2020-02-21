---
title: Пошаговое руководство. привязка библиотеки Котлин для Android
description: В этой статье приводятся пошаговые инструкции по созданию привязки Xamarin. Android для существующей библиотеки Котлин, Бубблепиккер. В нем рассматриваются такие темы, как Компиляция библиотеки Котлин, ее привязка и использование привязки в приложении Xamarin. Android.
ms.prod: xamarin
ms.assetid: 5E45451F-514F-4F2B-A6E8-599ED2EFDA2C
ms.technology: xamarin-android
author: alexeystrakh
ms.author: alstrakh
ms.date: 02/11/2020
ms.openlocfilehash: cbd7c796cd13aa45dc107bddf06ca44d6adbdf9d
ms.sourcegitcommit: fec87846fcb262fc8b79774a395908c8c8fc8f5b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/20/2020
ms.locfileid: "77519686"
---
# <a name="walkthrough-bind-an-android-kotlin-library"></a>Пошаговое руководство. привязка библиотеки Котлин для Android

Xamarin позволяет разработчикам мобильных приложений создавать кросс-платформенные собственные мобильные приложения с помощью C#Visual Studio и. Вы можете использовать компоненты пакета SDK для платформы Android, но во многих случаях вы также хотите использовать сторонние пакеты SDK, написанные для этой платформы, и Xamarin позволяет сделать это с помощью привязок. Чтобы внедрить сторонние платформы Android в приложение Xamarin. Android, необходимо создать для него привязку Xamarin. Android, прежде чем ее можно будет использовать в приложениях.

Платформа Android, а также ее собственные языки и средства постоянно развиваются, включая Последнее введение в язык Котлин, который, в конечном итоге, заменяет Java. Существует ряд пакетов SDK для трехмерных сторон, которые уже были перенесены с Java в Котлин и представляют собой новые проблемы. Несмотря на то, что процесс привязки Котлин аналогичен Java, он требует дополнительных шагов и параметров конфигурации для успешной сборки и запуска в рамках приложения Xamarin. Android.  

Цель этого документа — описать подход высокого уровня для решения этой ситуации и предоставить подробные пошаговые инструкции с простым примером.

## <a name="background"></a>Историческая справка

Котлин был выпущен в феврале 2016 и был помещен в качестве альтернативы стандартному компилятору Java в Android Studio на 2017. Позднее в 2019 Google объявил о том, что язык программирования Котлин стал предпочтительным языком для разработчиков приложений Android. Высокоуровневый подход к связыванию аналогичен [процессу привязки регулярных библиотек Java](https://docs.microsoft.com/xamarin/android/platform/binding-java-library/) с несколькими важными Котлин действиями.

## <a name="prerequisites"></a>предварительные требования

Для выполнения задач этого руководства необходимы:

- [Android Studio](https://developer.android.com/studio)
- [Visual Studio для Mac](https://visualstudio.microsoft.com/downloads)
- [Декомпилятор Java](http://java-decompiler.github.io/)

## <a name="build-a-native-library"></a>Создание собственной библиотеки

Первым шагом является создание собственной библиотеки Котлин с помощью Android Studio. Библиотека обычно предоставляется сторонним разработчиком или доступна в [репозитории Maven Google](https://maven.google.com/web/index.html) и других удаленных репозиториях. Например, в этом руководстве создается привязка для библиотеки Котлин инструмента пузырьковой выборки:

![Демонстрация Бубблепиккер GitHub](walkthrough-images/github-bubblepicker-demo.png)

1. Скачайте [Исходный код](https://github.com/igalata/Bubble-Picker/archive/develop.zip) из GitHub для библиотеки и распакуйте его в **пузырьковую программу выбора**локальных папок.

1. Запустите Android Studio и выберите пункт **Открыть существующий Android Studio проект** , выбрав локальную папку всплывающего окна выбора:

    ![Android Studio открыть проект](walkthrough-images/android-studio-open-project.png)

1. Убедитесь, что Android Studio не устарела, включая Gradle. Исходный код можно успешно построить на Android Studio v 3.5.3, Gradle v 5.4.1. Инструкции по обновлению Gradle до последней версии Gradle [можно найти здесь](https://gradle.org/install/).

1. Убедитесь, что установлен обязательный пакет SDK для Android. Исходный код требует пакет SDK для Android V25. Откройте меню **сервис > диспетчер пакетов SDK** , чтобы установить компоненты пакета SDK.

1. Обновите и синхронизируйте главный файл конфигурации **Build. gradle** , расположенный в корне папки проекта:

    - Задайте для параметра версия Котлин значение 1.3.10.

        ```gradle
        buildscript {
            ext.kotlin_version = '1.3.10'
        }
        ```

    - Зарегистрируйте репозиторий Maven для Google по умолчанию, чтобы можно было разрешить зависимость библиотеки поддержки:

        ```gradle
        allprojects {
            repositories {
                jcenter()
                maven {
                    url "https://maven.google.com"
                }
            }
        }
        ```

    - После обновления файла конфигурации он не синхронизирован, и Gradle отображает кнопку **Синхронизировать сейчас** , нажмите ее и дождитесь завершения процесса синхронизации:

        ![Android Studio синхронизировать Gradle сейчас](walkthrough-images/android-studio-gradle-syncnow.png)

        > [!TIP]
        > Кэш зависимостей Gradle может быть поврежден, это иногда происходит после истечения времени ожидания сетевого подключения. Повторно Скачайте зависимости и синхронизируйте проект (требуется сеть).

        > [!TIP]
        > Состояние процесса сборки Gradle (управляющая программа) может быть повреждено. Остановка всех управляющих программ Gradle может решить эту проблему. Останавливает процессы сборки Gradle (требуется перезапуск). В случае повреждения процессов Gradle можно также закрыть интегрированную среду разработки, а затем выполнить все процессы Java.

        > [!TIP]
        > Возможно, ваш проект использует сторонний подключаемый модуль, который несовместим с другими подключаемыми модулями в проекте или версией Gradle, запрошенной проектом.

1. Откройте меню Gradle справа, перейдите в меню **бубблепиккер > задачи** , выполните задачу **сборки** , дважды коснувшись ее и дождитесь завершения процесса сборки:

    ![Android Studio выполнения задачи Gradle](walkthrough-images/android-studio-gradle-execute-task.png)

1. Откройте браузер файлов корневой папки и перейдите в папку сборки: **пузырьковая выборка-> бубблепиккер-> Build-> Outputs-> AAR**, сохраните файл **бубблепиккер-релеасе. AAR** как **бубблепиккер-в 1.0. AAR**, этот файл будет использоваться в процессе привязки позже:

    ![Android Studio выходных данных AAR](walkthrough-images/android-studio-aar-output.png)

Файл AAR — это Архив Android, который содержит скомпилированный исходный код и ресурсы Котлин, необходимые Android для запуска приложения с помощью этого пакета SDK.  

## <a name="prepare-metadata"></a>Подготовка метаданных

Вторым шагом является подготовка файла преобразования метаданных, который используется Xamarin. Android для создания соответствующих C# классов. Проект привязки Xamarin. Android будет обнаруживать все собственные классы и члены из заданного архива Android, последовательно создавая XML-файл с соответствующими метаданными. Затем созданный вручную файл преобразования метаданных применяется к ранее созданному базовому плану для создания окончательного XML-файла определения, используемого C# для создания кода.

Метаданные используют синтаксис [XPath](https://www.w3.org/TR/xpath/) и используются генератором привязок для повлиять на создание сборки привязки. В статье о [метаданных привязки Java](https://docs.microsoft.com/xamarin/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata) приведены дополнительные сведения о преобразованиях, которые могут быть применены:

1. Создайте пустой файл **Metadata. XML** :

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <metadata>
    </metadata>
    ```

1. Определение XML-преобразований:

- Собственная библиотека Котлин имеет две зависимости, которые вы не хотите предоставлять в C# мир, определите два преобразования, чтобы полностью игнорировать их. Важно отметить, что собственные члены не будут удалены из результирующего двоичного файла, но C# только классы создаваться не будут. [Декомпилятор Java](http://java-decompiler.github.io/) можно использовать для обнаружения зависимостей. Запустите средство и откройте созданный ранее файл AAR, в результате чего будет показана структура архива Android, отражающая все зависимости, значения, ресурсы, манифесты и классы:  

    ![Зависимости декомпиляторов Java](walkthrough-images/java-decompiler-dependencies.png)

    Преобразования для пропуска обработки этих пакетов определяются с помощью инструкций XPath:

    ```xml
    <remove-node path="/api/package[starts-with(@name,'org.jbox2d')]" />
    <remove-node path="/api/package[starts-with(@name,'org.slf4j')]" />
    ```

- Класс Native `BubblePicker` имеет два метода `getBackgroundColor` и `setBackgroundColor`, а следующее преобразование изменит его на свойство C# `BackgroundColor`:

    ```xml
    <attr path="/api/package[@name='com.igalata.bubblepicker.rendering']/class[@name='BubblePicker']/method[@name='getBackground' and count(parameter)=0]" name="propertyName">BackgroundColor</attr>
    <attr path="/api/package[@name='com.igalata.bubblepicker.rendering']/class[@name='BubblePicker']/method[@name='setBackground' and count(parameter)=1 and parameter[1][@type='int']]" name="propertyName">BackgroundColor</attr>
    ```

- Для неподписанных типов `UInt, UShort, ULong, UByte` требуется специальная обработка. Для этих типов Котлин изменяет имена методов и типы параметров автоматически, что отражается в созданном коде:

    ```Kotlin
    public open fun fooUIntMethod(value: UInt) : String {
        return "fooUIntMethod${value}"
    }
    ```

    Этот код компилируется в следующий байтовый код Java:

    ```Java byte code
    @NotNull
    public String fooUIntMethod-WZ4Q5Ns(int value) {
    return "fooUIntMethod" + UInt.toString-impl(value);
    }
    ```

    Более того, связанные типы, такие как `UIntArray, UShortArray, ULongArray, UByteArray`, также затрагиваются Котлин. Имя метода изменяется для включения дополнительного суффикса, а параметры изменяются на массив элементов подписанных версий тех же типов. В примере ниже параметр типа `UIntArray` преобразуется автоматически в `int[]`, а имя метода изменяется с `fooUIntArrayMethod` на `fooUIntArrayMethod--ajY-9A`. Второй метод обнаруживается средствами Xamarin. Android и формируется как допустимое имя метода:

    ```Kotlin
    public open fun fooUIntArrayMethod(value: UIntArray) : String {
        return "fooUIntArrayMethod${value.size}"
    }
    ```

    Этот код компилируется в следующий байтовый код Java:

    ```Java byte code
    @NotNull
    public String fooUIntArrayMethod--ajY-9A(@NotNull int[] value) {
        Intrinsics.checkParameterIsNotNull(value, "value");
        return "fooUIntArrayMethod" + UIntArray.getSize-impl(value);
    }
    ```

    Чтобы дать ей понятное имя, в **файл metadata. XML**можно добавить следующие метаданные, которые будут обновлены до первоначального определения в коде Котлин:

    ```xml
    <attr path="/api/package[@name='com.microsoft.simplekotlinlib']/class[@name='FooClass']/method[@name='fooUIntArrayMethod--ajY-9A']" name="managedName">fooUIntArrayMethod</attr>
    ```

    В примере Бубблепиккер нет членов, использующих неподписанные типы, поэтому дополнительные изменения не требуются.

- Члены Котлин с универсальными параметрами по умолчанию преобразуются в параметры Java.`Lang.Object` Тип. Например, метод Котлин имеет универсальный параметр \<T >:

    ```Kotlin
    public open fun <T>fooGenericMethod(value: T) : String {
    return "fooGenericMethod${value}"
    }
    ```

    После создания привязки Xamarin. Android метод предоставляется C# следующим образом:

    ```csharp
    [Register ("fooGenericMethod", "(Ljava/lang/Object;)Ljava/lang/String;", "GetFooGenericMethod_Ljava_lang_Object_Handler")]
    [JavaTypeParameters (new string[] {
        "T"
    })]

    public virtual string FooGenericMethod (Java.Lang.Object value);
    ```

    Универсальные шаблоны Java и Котлин не поддерживаются привязками Xamarin. Android, поэтому создается обобщенный C# метод доступа к универсальному API. В качестве обходного решения можно создать библиотеку Котлин-оболочки и предоставить необходимые API строго типизированным способом без универсальных шаблонов. Кроме того, можно создавать вспомогательные методы C# для решения проблемы таким же образом с помощью API-интерфейсов со строгим типом.

    > [!TIP]
    > При преобразовании метаданных к созданной привязке можно применить любые изменения. В статье о [привязке библиотеки Java](https://docs.microsoft.com/xamarin/android/platform/binding-java-library/) объясняется, как создаются и обрабатываются метаданные.

## <a name="build-a-binding-library"></a>Создание библиотеки привязки

Следующим шагом является создание проекта привязки Xamarin. Android с помощью шаблона привязки Visual Studio, добавление необходимых метаданных, встроенных ссылок и последующая сборка проекта для создания используемой библиотеки:

1. Откройте Visual Studio для Mac и создайте новый проект библиотеки привязки Xamarin. Android, присвойте ему имя, в данном случае **тестбубблепиккер. Binding** , и завершите работу мастера. Шаблон привязки Xamarin. Android расположен по следующему пути: **Библиотека привязок для android > library >** :

    ![Создание привязки в Visual Studio](walkthrough-images/visual-studio-create-binding.png)

    В папке Transforms есть три основных файла преобразования:

    - **Metadata. XML** — позволяет вносить изменения в окончательный API, например изменять пространство имен созданной привязки.
    - **EnumFields. XML** — содержит сопоставление между константами и C# перечислениями Java int.
    - **Enummethods-. XML** — разрешает изменение параметров метода и возвращаемых типов из констант Java int C# в перечисления.

    Сохраните пустые файлы **EnumFields. XML** и **enummethods-. XML** и обновите файл **Metadata. XML** , чтобы определить преобразования.

1. Замените существующий файл **TRANSFORMS/Metadata. XML** файлом **Metadata. XML** , созданным на предыдущем шаге. В окне "Свойства" убедитесь, что для **действия сборки** файла задано значение **трансформатионфиле**:

    ![Метаданные Visual Studio](walkthrough-images/visual-studio-metadata.png)

1. Добавьте файл **бубблепиккер-в 1.0. AAR** , созданный в шаге 1, в проект привязки в качестве собственной ссылки. Чтобы добавить ссылки на собственную библиотеку, откройте окно Finder и перейдите в папку с архивом Android. Перетащите Архив в папку JAR в обозреватель решений. Кроме того, можно использовать пункт **Добавить** контекстное меню в папке JAR и выбрать **существующие файлы.** . Выберите, чтобы скопировать файл в каталог для целей данного пошагового руководства. Убедитесь, что для **действия сборки** задано значение **либрарипрожектзип**:

    ![Собственная ссылка на Visual Studio](walkthrough-images/visual-studio-native-reference.png)

1. Добавьте ссылку на пакет [NuGet Xamarin. Котлин. StdLib](https://www.nuget.org/packages/Xamarin.Kotlin.StdLib/) . Этот пакет является привязкой для стандартной библиотеки Котлин. Без этого пакета привязка будет работать только в том случае, если библиотека Котлин не использует специфические типы Котлин, иначе все эти члены не будут предоставлены C# , а любое приложение, пытающееся использовать привязку, приведет к сбою во время выполнения.

    > [!TIP]
    > Из-за ограничения Xamarin. Android средства привязки могут быть добавлены только в один архив Android (AAR) для каждого проекта привязки. Если необходимо добавить несколько файлов AAR, требуется несколько проектов Xamarin. Android, по одному на каждый AAR. Если это было так в этом пошаговом руководстве, то предыдущие четыре действия этого шага придется повторить для каждого архива. В качестве альтернативного варианта можно вручную объединить несколько архивов Android в один архив, и в результате можно будет использовать один проект привязки Xamarin. Android.

1. Завершающим действием является создание библиотеки и отсутствие ошибок компиляции. В случае ошибок компиляции они могут быть решены и обработаны с помощью файла metadata. XML, созданного ранее путем добавления метаданных преобразования XML, которые будут добавлять, удалять или переименовывать элементы библиотеки.

## <a name="consume-the-binding-library"></a>Использование библиотеки привязки

Последним шагом является использование библиотеки привязки Xamarin. Android в приложении Xamarin. Android. Создайте новый проект Xamarin. Android, добавьте ссылку на библиотеку привязки и пользовательский интерфейс средства отображения пузырьковой подшивки:

1. Создайте проект Xamarin. Android. Используйте приложение **android > > Android** в качестве отправной точки и выберите **последний и наибольший** , как целевые платформы, чтобы избежать проблем совместимости. Этот проект предназначен для следующих шагов:

    ![Создание приложения в Visual Studio](walkthrough-images/visual-studio-create-app.png)

1. Добавьте ссылку на проект в проект привязки или добавьте ссылку на созданную ранее библиотеку DLL:

    ![Добавление ссылки привязки в Visual Studio. png](walkthrough-images/visual-studio-add-binding-reference.png)

1. Добавьте ссылку на пакет [NuGet Xamarin. Котлин. StdLib](https://www.nuget.org/packages/Xamarin.Kotlin.StdLib/) , который вы добавили ранее в проект привязки Xamarin. Android. Он добавляет поддержку для любых типов Котлин, которые должны обрабатываться в среде выполнения.  Без этого пакета приложение может быть скомпилировано, но произойдет сбой во время выполнения:

    ![Добавление NuGet StdLib в Visual Studio](walkthrough-images/visual-studio-add-stdlib-nuget.png)

1. Добавьте `BubblePicker` элемент управления в макет Android для `MainActivity`. Откройте файл **тестбубблепиккер/RESOURCES/Layout/content_main. XML** и добавьте узел элемента управления бубблепиккер в качестве последнего элемента корневого элемента управления RelativeLayout:

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <RelativeLayout …>
        …
        <com.igalata.bubblepicker.rendering.BubblePicker
            android:id="@+id/picker"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            app:backgroundColor="@android:color/white" />
    </RelativeLayout>
    ```

1. Обновите исходный код приложения и добавьте логику инициализации в `MainActivity`, которая активирует пакет SDK для пузырьковой программы выбора:

    ```csharp
    protected override void OnCreate(Bundle savedInstanceState)
    {
        ...
        var picker = FindViewById<BubblePicker>(Resource.Id.picker);
        picker.BubbleSize = 20;
        picker.Adapter = new BubblePickerAdapter();
        picker.Listener = new BubblePickerListener(picker);
        ...
    }
    ```

    `BubblePickerAdapter` и `BubblePickerListener` — это два класса, создаваемые с нуля, которые обрабатывают данные пузырьков и взаимодействуют с элементом управления:

    ```csharp
    public class BubblePickerAdapter : Java.Lang.Object, IBubblePickerAdapter
    {
        private List<string> _bubbles = new List<string>();
        public int TotalCount => _bubbles.Count;
        public BubblePickerAdapter()
        {
            for (int i = 0; i < 10; i++)
            {
                _bubbles.Add($"Item {i}");
            }
        }

        public PickerItem GetItem(int itemIndex)
        {
            if (itemIndex < 0 || itemIndex >= _bubbles.Count)
                return null;

            var result = _bubbles[itemIndex];
            var item = new PickerItem(result);
            return item;
        }
    }

    public class BubblePickerListener : Java.Lang.Object, IBubblePickerListener
    {
        public View Picker { get; }
        public BubblePickerListener(View picker)
        {
            Picker = picker;
        }

        public void OnBubbleDeselected(PickerItem item)
        {
            Snackbar.Make(Picker, $"Deselected: {item.Title}", Snackbar.LengthLong)
                .SetAction("Action", (Android.Views.View.IOnClickListener)null)
                .Show();
        }

        public void OnBubbleSelected(PickerItem item)
        {
            Snackbar.Make(Picker, $"Selected: {item.Title}", Snackbar.LengthLong)
            .SetAction("Action", (Android.Views.View.IOnClickListener)null)
            .Show();
        }
    }
    ```

1. Запустите приложение, которое должно визуализировать пользовательский интерфейс средства выбора пузырьков.

    ![Демонстрация Бубблепиккер](walkthrough-images/bubble-picker-demo.png)

    Для примера требуется дополнительный код для отрисовки стиля элементов и обработки взаимодействий, но элемент управления `BubblePicker` был успешно создан и активирован.

Поздравляем! Вы успешно создали приложение Xamarin. Android и библиотеку привязок, которая использует библиотеку Котлин.  

Теперь у вас должно быть базовое приложение Xamarin. Android, которое использует собственную библиотеку Котлин через библиотеку привязки Xamarin. Android. В этом пошаговом руководстве намеренно используется простой пример для лучшего привлечения ключевых концепций. В реальных сценариях, скорее всего, потребуется предоставить большее количество API-интерфейсов и применить к ним преобразования метаданных.

## <a name="related-links"></a>Связанные ссылки

- [Android Studio](https://developer.android.com/studio)
- [Установка Gradle](https://gradle.org/install/)
- [Visual Studio для Mac](https://visualstudio.microsoft.com/downloads)
- [Декомпилятор Java](http://java-decompiler.github.io/)
- [Библиотека Котлин Бубблепиккер](https://github.com/igalata/Bubble-Picker)
- [Привязка библиотеки Java](https://docs.microsoft.com/xamarin/android/platform/binding-java-library/)
- [Формате](https://www.w3.org/TR/xpath/)
- [Метаданные привязки Java](https://docs.microsoft.com/xamarin/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata)
- [Xamarin. Котлин. StdLib NuGet](https://www.nuget.org/packages/Xamarin.Kotlin.StdLib/)
- [Пример репозитория проекта](https://github.com/alexeystrakh/xamarin-binding-kotlin-framework)
