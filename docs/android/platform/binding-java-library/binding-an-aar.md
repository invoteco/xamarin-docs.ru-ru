---
title: Привязка AAR-файла
description: Это пошаговое руководство содержит пошаговые инструкции по созданию библиотеки привязок Java для Xamarin. Android из Android. Файл AAR.
ms.prod: xamarin
ms.assetid: 380413B8-6A99-4BB8-B64C-3EAF9F359C22
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/11/2018
ms.openlocfilehash: 4c612faca90d4b2b4e44cfbb53ff65ec07fcdfd1
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2019
ms.locfileid: "69524836"
---
# <a name="binding-an-aar"></a>Привязка AAR-файла

_Это пошаговое руководство содержит пошаговые инструкции по созданию библиотеки привязок Java для Xamarin. Android из Android. Файл AAR._


## <a name="overview"></a>Обзор

*Архив Android (. AAR)* — это формат файла для библиотек Android.
Ранне. Файл AAR — это. ZIP-архив, содержащий следующие сведения:

- Скомпилированный код Java
- Идентификаторы ресурсов
- Ресурсы
- Метаданные (например, объявления действий, разрешения)

В этом пошаговом руководством мы рассмотрим основы создания библиотеки привязок для одного. Файл AAR. Общие сведения о привязке библиотеки Java в целом (с примером базового кода) см. в разделе [Привязка библиотеки Java](~/android/platform/binding-java-library/index.md).


> [!IMPORTANT]
> Проект привязки может включать только один. Файл AAR. Если. AAR зависимости от другого. AAR, эти зависимости должны содержаться в своем собственном проекте привязки, а затем ссылаться на них. См. [ошибку 44573](https://bugzilla.xamarin.com/show_bug.cgi?id=44573).


## <a name="walkthrough"></a>Пошаговое руководство

Мы создадим библиотеку привязок для примера файла архива Android, созданного в Android Studio, [текстанализер. AAR](https://github.com/xamarin/monodroid-samples/blob/master/JavaIntegration/AarBinding/Resources/textanalyzer.aar?raw=true). Настоящего. AAR содержит `TextCounter` класс со статическими методами, которые подсчитывает количество гласных и согласных строк. Кроме того, **текстанализер. AAR** содержит ресурс изображения, помогающий отображать результаты инвентаризации.

Чтобы создать библиотеку привязок из, мы будем использовать следующие шаги. Файл AAR:

1. Создайте новый проект библиотеки привязок Java.

2. Добавьте один объект. AAR файл в проект. Проект привязки может содержать только один объект. AAR.

3. Задайте соответствующее действие сборки для. Файл AAR.

4. Выберите целевую платформу, которую. AAR поддерживает.

5. Создайте библиотеку привязок.

После создания библиотеки привязок мы разработаем небольшое приложение Android, которое предлагает пользователю ввести текстовую строку, вызывает. Методы AAR для анализа текста извлекают изображение из. AAR и отображает результаты вместе с изображением.

Пример приложения будет обращаться к `TextCounter` классу класса **текстанализер. AAR**:

```java
package com.xamarin.textcounter;

public class TextCounter
{
    ...
    public static int numVowels (String text) { ... };
    ...
    public static int numConsonants (String text) { ... };
    ...
}
```

Кроме того, этот пример приложения будет получать и отображать ресурс изображения, упакованный в **текстанализер. AAR**:

[![Образ обезьяны Xamarin](binding-an-aar-images/00-monkey-sml.png)](binding-an-aar-images/00-monkey.png#lightbox)

Этот ресурс изображения находится в **RES/Draw/обезьян. png** в **текстанализер. AAR**.



### <a name="creating-the-bindings-library"></a>Создание библиотеки привязок

Прежде чем приступить к началом, скачайте файл архива Android [текстанализер. AAR](https://github.com/xamarin/monodroid-samples/blob/master/JavaIntegration/AarBinding/Resources/textanalyzer.aar?raw=true) :

1. Создайте новый проект библиотеки привязок, начиная с шаблона библиотеки привязок Android. Можно использовать либо Visual Studio для Mac, либо Visual Studio (на снимках экрана ниже показана Visual Studio, но Visual Studio для Mac очень похожа). Назовите решение **аарбиндинг**:

    [![Создание проекта Аарбиндингс](binding-an-aar-images/01-new-bindings-library-vs-sml.w157.png)](binding-an-aar-images/01-new-bindings-library-vs.w157.png#lightbox)

2. Шаблон содержит папку **JAR** , в которую вы добавляете. AAR в проект библиотеки привязок. Щелкните правой кнопкой мыши папку **JAR** и выберите **Добавить > существующий элемент**:

    [![Добавить существующий элемент](binding-an-aar-images/02-add-existing-item-vs-sml.png)](binding-an-aar-images/02-add-existing-item-vs.png#lightbox)


3. Перейдите к файлу **текстанализер. AAR** , скачанному ранее, выберите его и нажмите кнопку **Добавить**:

    [![Добавление текстаналайзер. AAR](binding-an-aar-images/03-select-aar-file-vs-sml.png)](binding-an-aar-images/03-select-aar-file-vs.png#lightbox)


4. Убедитесь, что файл **текстанализер. AAR** был успешно добавлен в проект:

    [![Файл текстанализер. AAR был добавлен](binding-an-aar-images/04-aar-added-vs-sml.png)](binding-an-aar-images/04-aar-added-vs.png#lightbox)

5. Задайте для действия сборки **текстанализер. AAR** `LibraryProjectZip`значение. В Visual Studio для Mac щелкните правой кнопкой мыши **текстанализер. AAR** , чтобы задать действие сборки. В Visual Studio действие сборки можно задать на панели **Свойства** .

    [![Установка для действия сборки текстанализер. AAR значения Либрарипрожектзип](binding-an-aar-images/05-embedded-aar-vs-sml.png)](binding-an-aar-images/05-embedded-aar-vs.png#lightbox)

6. Откройте свойства проекта, чтобы настроить *требуемую платформу*. Если. AAR использует любые API-интерфейсы Android, установите целевую платформу на уровне API, который. AAR ожидания. (Дополнительные сведения о параметрах целевой платформы и уровнях API Android см. в разделе [Основные сведения об уровнях API Android](~/android/app-fundamentals/android-api-levels.md).)

    Задайте целевой уровень API для библиотеки привязок. В этом примере мы бесплатно используем Новейший уровень API платформы (API уровня 23), так как наш **текстанализер** не зависит от интерфейсов API Android:

    [![Установка для целевого уровня API 23](binding-an-aar-images/06-set-target-framework-vs-sml.png)](binding-an-aar-images/06-set-target-framework-vs.png#lightbox)

7. Создайте библиотеку привязок. Проект библиотеки привязок должен быть успешно построен и выдавать выходные данные. DLL по следующему адресу: **Аарбиндинг/bin/debug/Аарбиндинг. dll**



### <a name="using-the-bindings-library"></a>Использование библиотеки привязок

Для использования. DLL в приложении Xamarin. Android, необходимо сначала добавить ссылку на библиотеку привязок. Выполните следующие действия.

1. Мы создаем это приложение в том же решении, что и библиотека привязок для упрощения этого пошагового руководства. (Приложение, использующее библиотеку привязок, также может находиться в другом решении.) Создайте новое приложение Xamarin. Android. Щелкните решение правой кнопкой мыши и выберите **Добавить новый проект**. Назовите новый проект **биндингтест**:

    [![Создать новый проект Биндингтест](binding-an-aar-images/07-add-new-project-vs-sml.w157.png)](binding-an-aar-images/07-add-new-project-vs.w157.png#lightbox)

2. Щелкните правой кнопкой мыши узел **ссылки** проекта **биндингтест** и выберите команду **Добавить ссылку...** :

    [![Нажмите кнопку Добавить ссылку.](binding-an-aar-images/08-add-reference-vs-sml.png)](binding-an-aar-images/08-add-reference-vs.png#lightbox)

3. Выберите проект **аарбиндинг** , созданный ранее, и нажмите кнопку **ОК**.

    [![Проверка проекта привязки AAR](binding-an-aar-images/09-choose-aar-binding-vs-sml.png)](binding-an-aar-images/09-choose-aar-binding-vs.png#lightbox)

4. Откройте узел **References** проекта **биндингтест** , чтобы убедиться в наличии ссылки **аарбиндинг** :

    [![Аарбиндинг отображается в списке ссылок](binding-an-aar-images/10-references-shows-aarbinding-vs-sml.png)](binding-an-aar-images/10-references-shows-aarbinding-vs.png#lightbox)


Если вы хотите просмотреть содержимое проекта библиотеки привязки, можно дважды щелкнуть ссылку, чтобы открыть ее в **обозревателе объектов**. Вы увидите сопоставленное содержимое `Com.Xamarin.Textcounter` пространства имен (сопоставленное из пакета Java `com.xamarin.textanalyzezr` ), и вы можете `TextCounter` просмотреть члены класса:

[![Просмотр обозревателя объектов](binding-an-aar-images/11-object-browser-vs-sml.png)](binding-an-aar-images/11-object-browser-vs.png#lightbox)

На приведенном выше снимке `TextAnalyzer` экрана показаны два метода, которые будут вызываться в примере приложения: `NumConsonants` (оболочка для базового метода `NumVowels` Java `numConsonants` ) и (который создает оболочку для базового метода Java `numVowels` ).



### <a name="accessing-aar-types"></a>Данному. Типы AAR

После добавления ссылки на приложение, которое указывает на библиотеку привязки, можно получить доступ к типам Java в. AAR так же, как C# и доступ к типам ( C# благодаря оболочкам). C#код приложения может вызывать `TextAnalyzer` методы, как показано в следующем примере:

```csharp
using Com.Xamarin.Textcounter;
...
int numVowels = TextCounter.NumVowels (myText);
int numConsonants = TextCounter.NumConsonants (myText);
```

В приведенном выше примере мы вызываем статические методы в `TextCounter` классе. Однако можно также создать экземпляры классов и вызвать методы экземпляра. Например, если. AAR заключает класс с именем `Employee` , который имеет метод `buildFullName`экземпляра. Вы можете создать экземпляр `MyClass` и использовать его, как показано здесь:

```csharp
var employee = new Com.MyCompany.MyProject.Employee();
var name = employee.BuildFullName ();
```

Следующие шаги позволяют добавить код в приложение, чтобы он запрашивает у пользователя текст, использовался `TextCounter` для анализа текста, а затем выводит результаты.

Замените макет **биндингтест** (**Main. axml**) следующим XML-кодом. Этот макет содержит `EditText` текст для ввода текста и две кнопки для инициации счетчиков гласных и согласных:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation             ="vertical"
    android:layout_width            ="fill_parent"
    android:layout_height           ="fill_parent" >
    <TextView
        android:text                ="Text to analyze:"
        android:textSize            ="24dp"
        android:layout_marginTop    ="30dp"
        android:layout_gravity      ="center"
        android:layout_width        ="wrap_content"
        android:layout_height       ="wrap_content" />
    <EditText
        android:id                  ="@+id/input"
        android:text                ="I can use my .AAR file from C#!"
        android:layout_marginTop    ="10dp"
        android:layout_gravity      ="center"
        android:layout_width        ="300dp"
        android:layout_height       ="wrap_content"/>
    <Button
        android:id                  ="@+id/vowels"
        android:layout_marginTop    ="30dp"
        android:layout_width        ="240dp"
        android:layout_height       ="wrap_content"
        android:layout_gravity      ="center"
        android:text                ="Count Vowels" />
    <Button
        android:id                  ="@+id/consonants"
        android:layout_width        ="240dp"
        android:layout_height       ="wrap_content"
        android:layout_gravity      ="center"
        android:text                ="Count Consonants" />
</LinearLayout>
```

Замените содержимое **MainActivity.CS** следующим кодом. Как показано в этом примере, обработчики событий кнопки вызывают Упакованные `TextCounter` методы, которые находятся в. AAR и используйте всплывающие уведомления для вывода результатов. Обратите `using` внимание на инструкцию для пространства имен привязанной библиотеки (в данном `Com.Xamarin.Textcounter`случае):

```csharp
using System;
using Android.App;
using Android.Content;
using Android.Runtime;
using Android.Views;
using Android.Widget;
using Android.OS;
using Android.Views.InputMethods;
using Com.Xamarin.Textcounter;

namespace BindingTest
{
    [Activity(Label = "BindingTest", MainLauncher = true, Icon = "@drawable/icon")]
    public class MainActivity : Activity
    {
        InputMethodManager imm;

        protected override void OnCreate(Bundle bundle)
        {
            base.OnCreate(bundle);

            SetContentView(Resource.Layout.Main);

            imm = (InputMethodManager)GetSystemService(Context.InputMethodService);

            var vowelsBtn = FindViewById<Button>(Resource.Id.vowels);
            var consonBtn = FindViewById<Button>(Resource.Id.consonants);
            var edittext = FindViewById<EditText>(Resource.Id.input);
            edittext.InputType = Android.Text.InputTypes.TextVariationPassword;

            edittext.KeyPress += (sender, e) =>
            {
                imm.HideSoftInputFromWindow(edittext.WindowToken, HideSoftInputFlags.NotAlways);
                e.Handled = true;
            };

            vowelsBtn.Click += (sender, e) =>
            {
                int count = TextCounter.NumVowels(edittext.Text);
                string msg = count + " vowels found.";
                Toast.MakeText (this, msg, ToastLength.Short).Show ();
            };

            consonBtn.Click += (sender, e) =>
            {
                int count = TextCounter.NumConsonants(edittext.Text);
                string msg = count + " consonants found.";
                Toast.MakeText (this, msg, ToastLength.Short).Show ();
            };

        }
    }
}
```

Скомпилируйте и запустите проект **биндингтест** . Приложение запустится и отобразит снимок экрана слева (объект `EditText` инициализируется каким-либо текстом, но его можно коснуться, чтобы изменить его). При нажатиикнопки ПОДсчитать гласные отображается число гласных, как показано справа:

[![Снимки экрана от запуска Биндингтест](binding-an-aar-images/12-count-vowels.png)](binding-an-aar-images/12-count-vowels.png#lightbox)

Попробуйте нажать кнопку **подсчет согласных** . Кроме того, можно изменить строку текста и снова коснуться этих кнопок, чтобы проверить различные гласные и согласные.



### <a name="accessing-aar-resources"></a>Данному. Ресурсы AAR

Средства Xamarin объединяют данные **R** из. AAR в класс **ресурсов** приложения. В результате вы можете получить доступ к. AAR ресурсы из макета (и из кода программной части) так же, как и к ресурсам, которые находятся в пути к **ресурсам** проекта.

Для доступа к ресурсу изображения используется имя **ресурса.** изображение, упакованное в. AAR. Например, можно создать ссылку на **Image. png** в. AAR файл с помощью `@drawable/image`:

```xml
<ImageView android:src="@drawable/image" ... />
```

Можно также получить доступ к макетам ресурсов, которые находятся в. AAR. Для этого используется имя **ресурса. макет** для макета, упакованного в. AAR. Например:

```csharp
var a = new ArrayAdapter<string>(this, Resource.Layout.row_layout, ...);
```

Пример **текстанализер. AAR** содержит файл изображения, который находится в **RES/Draw/обезьян. png**. Давайте выполним доступ к этому ресурсу изображения и используем его в нашем примере приложения:

Измените макет **биндингтест** (**Main. axml**) `ImageView` и добавьте в конец `LinearLayout` контейнера. Отображается изображение, найденное в **@drawable/monkey** . Этот образ будет загружен из раздела ресурсов в **текстанализер. AAR:** `ImageView`

```xml
    ...
    <ImageView
        android:src                 ="@drawable/monkey"
        android:layout_marginTop    ="40dp"
        android:layout_width        ="200dp"
        android:layout_height       ="200dp"
        android:layout_gravity      ="center" />

</LinearLayout>
```

Скомпилируйте и запустите проект **биндингтест** . Приложение запустится и отобразит снимок экрана слева &ndash; , когда вы нажмете **число согласных**, результаты будут отображаться, как показано справа:

[![Биндингтест отображение числа согласных](binding-an-aar-images/13-count-consonants.png)](binding-an-aar-images/13-count-consonants.png#lightbox)


Поздравляем! Вы успешно привязываете библиотеку Java. AAR!



## <a name="summary"></a>Сводка

В этом пошаговом руководстве мы создали библиотеку привязок для. Файл AAR, добавлена библиотека привязок в тестовое приложение с минимальными затратами и запустила приложение, C# чтобы убедиться, что наш код может вызывать код Java, находящийся в. Файл AAR.
Кроме того, мы расширили приложение для доступа к ресурсу изображения, который находится в. Файл AAR.


## <a name="related-links"></a>Связанные ссылки

- [Создание библиотеки привязок Java (видео)](https://university.xamarin.com/classes#10090)
- [Привязка JAR-файла](~/android/platform/binding-java-library/binding-a-jar.md)
- [Привязка библиотеки Java](~/android/platform/binding-java-library/index.md)
- [Аарбиндинг (пример)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/javaintegration-aarbinding)
- [Ошибка 44573 — один проект не может привязать несколько AAR-файлов](https://bugzilla.xamarin.com/show_bug.cgi?id=44573)
