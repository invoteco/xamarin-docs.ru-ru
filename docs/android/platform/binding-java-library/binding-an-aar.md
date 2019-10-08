---
title: Привязка AAR-файла
description: Это пошаговое руководство содержит пошаговые инструкции по созданию библиотеки привязок Java для Xamarin. Android из Android. Файл AAR.
ms.prod: xamarin
ms.assetid: 380413B8-6A99-4BB8-B64C-3EAF9F359C22
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/11/2018
ms.openlocfilehash: cdd68def895fea362d9ad3147e3d622471d73a63
ms.sourcegitcommit: 4ff181101d76f048b949c9613b2c72cf02618f8b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/07/2019
ms.locfileid: "71994897"
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

Мы создадим библиотеку привязок для примера файла архива Android, созданного в Android Studio, [текстанализер. AAR](https://github.com/xamarin/monodroid-samples/blob/master/JavaIntegration/AarBinding/Resources/textanalyzer.aar?raw=true). Настоящего. AAR содержит класс `TextCounter` со статическими методами, которые подсчитывают число гласных и согласных строк. Кроме того, **текстанализер. AAR** содержит ресурс изображения, помогающий отображать результаты инвентаризации.

Чтобы создать библиотеку привязок из, мы будем использовать следующие шаги. Файл AAR:

1. Создайте новый проект библиотеки привязок Java.

2. Добавьте один объект. AAR файл в проект. Проект привязки может содержать только один объект. AAR.

3. Задайте соответствующее действие сборки для. Файл AAR.

4. Выберите целевую платформу, которую. AAR поддерживает.

5. Создайте библиотеку привязок.

После создания библиотеки привязок мы разработаем небольшое приложение Android, которое предлагает пользователю ввести текстовую строку, вызывает. Методы AAR для анализа текста извлекают изображение из. AAR и отображает результаты вместе с изображением.

Пример приложения будет обращаться к классу `TextCounter` класса **текстанализер. AAR**:

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

[Образ обезьяны @no__t 1Xamarin](binding-an-aar-images/00-monkey-sml.png)](binding-an-aar-images/00-monkey.png#lightbox)

Этот ресурс изображения находится в **RES/Draw/обезьян. png** в **текстанализер. AAR**.

### <a name="creating-the-bindings-library"></a>Создание библиотеки привязок

Прежде чем приступить к началом, скачайте файл архива Android [текстанализер. AAR](https://github.com/xamarin/monodroid-samples/blob/master/JavaIntegration/AarBinding/Resources/textanalyzer.aar?raw=true) :

1. Создайте новый проект библиотеки привязок, начиная с шаблона библиотеки привязок Android. Можно использовать либо Visual Studio для Mac, либо Visual Studio (на снимках экрана ниже показана Visual Studio, но Visual Studio для Mac очень похожа). Назовите решение **аарбиндинг**:

    [проект @no__t 1Create Аарбиндингс](binding-an-aar-images/01-new-bindings-library-vs-sml.w160.png)](binding-an-aar-images/01-new-bindings-library-vs.w160.png#lightbox)

2. Шаблон содержит папку **JAR** , в которую вы добавляете. AAR в проект библиотеки привязок. Щелкните правой кнопкой мыши папку **JAR** и выберите **Добавить > существующий элемент**:

    [![Add существующий элемент](binding-an-aar-images/02-add-existing-item-vs-sml.png)](binding-an-aar-images/02-add-existing-item-vs.png#lightbox)

3. Перейдите к файлу **текстанализер. AAR** , скачанному ранее, выберите его и нажмите кнопку **Добавить**:

    [@no__t 1Add текстаналайзер. AAR](binding-an-aar-images/03-select-aar-file-vs-sml.png)](binding-an-aar-images/03-select-aar-file-vs.png#lightbox)

4. Убедитесь, что файл **текстанализер. AAR** был успешно добавлен в проект:

    [добавлен файл ![The текстанализер. AAR](binding-an-aar-images/04-aar-added-vs-sml.png)](binding-an-aar-images/04-aar-added-vs.png#lightbox)

5. Задайте для действия сборки **текстанализер. AAR** значение `LibraryProjectZip`. В Visual Studio для Mac щелкните правой кнопкой мыши **текстанализер. AAR** , чтобы задать действие сборки. В Visual Studio действие сборки можно задать на панели **Свойства** .

    [![Setting действие сборки текстанализер. AAR в Либрарипрожектзип](binding-an-aar-images/05-embedded-aar-vs-sml.png)](binding-an-aar-images/05-embedded-aar-vs.png#lightbox)

6. Откройте свойства проекта, чтобы настроить *требуемую платформу*. Если. AAR использует любые API-интерфейсы Android, установите целевую платформу на уровне API, который. AAR ожидания. (Дополнительные сведения о параметрах целевой платформы и уровнях API Android см. в разделе [Основные сведения об уровнях API Android](~/android/app-fundamentals/android-api-levels.md).)

    Задайте целевой уровень API для библиотеки привязок. В этом примере мы бесплатно используем Новейший уровень API платформы (API уровня 23), так как наш **текстанализер** не зависит от интерфейсов API Android:

    [@no__t 1Setting целевой уровень API 23](binding-an-aar-images/06-set-target-framework-vs-sml.png)](binding-an-aar-images/06-set-target-framework-vs.png#lightbox)

7. Создайте библиотеку привязок. Проект библиотеки привязок должен быть успешно построен и выдавать выходные данные. DLL по следующему адресу: **Аарбиндинг/bin/debug/Аарбиндинг. dll**

### <a name="using-the-bindings-library"></a>Использование библиотеки привязок

Для использования. DLL в приложении Xamarin. Android, необходимо сначала добавить ссылку на библиотеку привязок. Выполните следующие действия.

1. Мы создаем это приложение в том же решении, что и библиотека привязок для упрощения этого пошагового руководства. (Приложение, использующее библиотеку привязок, также может находиться в другом решении.) Создайте новое приложение Xamarin. Android. Щелкните решение правой кнопкой мыши и выберите **Добавить новый проект**. Назовите новый проект **биндингтест**:

    [![Create новый проект Биндингтест](binding-an-aar-images/07-add-new-project-vs-sml.w157.png)](binding-an-aar-images/07-add-new-project-vs.w157.png#lightbox)

2. Щелкните правой кнопкой мыши узел **ссылки** проекта **биндингтест** и выберите команду **Добавить ссылку...** :

    [![Click добавить ссылку](binding-an-aar-images/08-add-reference-vs-sml.png)](binding-an-aar-images/08-add-reference-vs.png#lightbox)

3. Выберите проект **аарбиндинг** , созданный ранее, и нажмите кнопку **ОК**.

    [Проект привязки AAR @no__t 1Check](binding-an-aar-images/09-choose-aar-binding-vs-sml.png)](binding-an-aar-images/09-choose-aar-binding-vs.png#lightbox)

4. Откройте узел **References** проекта **биндингтест** , чтобы убедиться в наличии ссылки **аарбиндинг** :

    [![AarBinding отображается в разделе ссылки.](binding-an-aar-images/10-references-shows-aarbinding-vs-sml.png)](binding-an-aar-images/10-references-shows-aarbinding-vs.png#lightbox)

Если вы хотите просмотреть содержимое проекта библиотеки привязки, можно дважды щелкнуть ссылку, чтобы открыть ее в **обозревателе объектов**. Вы увидите сопоставленное содержимое пространства имен `Com.Xamarin.Textcounter` (сопоставленное из пакета Java `com.xamarin.textanalyzezr`), и вы можете просмотреть элементы класса `TextCounter`:

[@no__t — 1Viewing обозреватель объектов](binding-an-aar-images/11-object-browser-vs-sml.png)](binding-an-aar-images/11-object-browser-vs.png#lightbox)

На приведенном выше снимке экрана показаны два метода `TextAnalyzer`, которые будут вызываться в примере приложения: `NumConsonants` (который заключает в оболочку базовый метод Java `numConsonants`) и `NumVowels` (который заключает в оболочку базовый метод Java `numVowels`).

### <a name="accessing-aar-types"></a>Данному. Типы AAR

После добавления ссылки на приложение, которое указывает на библиотеку привязки, можно получить доступ к типам Java в. AAR так же, как C# и доступ к типам ( C# благодаря оболочкам). C#код приложения может вызывать методы `TextAnalyzer`, как показано в следующем примере:

```csharp
using Com.Xamarin.Textcounter;
...
int numVowels = TextCounter.NumVowels (myText);
int numConsonants = TextCounter.NumConsonants (myText);
```

В приведенном выше примере мы вызываем статические методы в классе `TextCounter`. Однако можно также создать экземпляры классов и вызвать методы экземпляра. Например, если. AAR заключает класс с именем `Employee`, имеющий метод экземпляра `buildFullName`, можно создать экземпляр `MyClass` и использовать его, как показано ниже:

```csharp
var employee = new Com.MyCompany.MyProject.Employee();
var name = employee.BuildFullName ();
```

Следующие шаги позволяют добавить код в приложение, чтобы он запрашивает у пользователя текст, использовал `TextCounter` для анализа текста, а затем выводит результаты.

Замените макет **биндингтест** (**Main. axml**) следующим XML-кодом. Этот макет имеет `EditText` для ввода текста и две кнопки для инициации счетчиков гласных и согласных:

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

Замените содержимое **MainActivity.CS** следующим кодом. Как показано в этом примере, обработчики событий кнопки вызывают Упакованные методы `TextCounter`, которые находятся в. AAR и используйте всплывающие уведомления для вывода результатов. Обратите внимание на инструкцию `using` для пространства имен привязанной библиотеки (в данном случае `Com.Xamarin.Textcounter`):

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

Скомпилируйте и запустите проект **биндингтест** . Приложение запустится и отобразит снимок экрана слева (`EditText` инициализируется некоторым текстом, но вы можете коснуться его, чтобы изменить его). При нажатии кнопки **подсчитать гласные**отображается число гласных, как показано справа:

[@no__t 1Screenshots от запуска Биндингтест](binding-an-aar-images/12-count-vowels.png)](binding-an-aar-images/12-count-vowels.png#lightbox)

Попробуйте нажать кнопку **подсчет согласных** . Кроме того, можно изменить строку текста и снова коснуться этих кнопок, чтобы проверить различные гласные и согласные.

### <a name="accessing-aar-resources"></a>Данному. Ресурсы AAR

Средства Xamarin объединяют данные **R** из. AAR в класс **ресурсов** приложения. В результате вы можете получить доступ к. AAR ресурсы из макета (и из кода программной части) так же, как и к ресурсам, которые находятся в пути к **ресурсам** проекта.

Для доступа к ресурсу изображения используется имя **ресурса.** изображение, упакованное в. AAR. Например, можно создать ссылку на **Image. png** в. AAR файл с помощью `@drawable/image`:

```xml
<ImageView android:src="@drawable/image" ... />
```

Можно также получить доступ к макетам ресурсов, которые находятся в. AAR. Для этого используется имя **ресурса. макет** для макета, упакованного в. AAR. Пример:

```csharp
var a = new ArrayAdapter<string>(this, Resource.Layout.row_layout, ...);
```

Пример **текстанализер. AAR** содержит файл изображения, который находится в **RES/Draw/обезьян. png**. Давайте выполним доступ к этому ресурсу изображения и используем его в нашем примере приложения:

Измените макет **биндингтест** (**Main. axml**) и добавьте `ImageView` в конец контейнера `LinearLayout`. Этот `ImageView` отображает образ, найденный в **\@drawable/обезьяне**; Этот образ будет загружен из раздела ресурсов **текстанализер. AAR**:

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

Скомпилируйте и запустите проект **биндингтест** . Приложение запустится и отобразит снимок экрана слева &ndash; при касании **числа согласных**результаты отображаются, как показано справа:

[@no__t 1BindingTest отображение числа согласных](binding-an-aar-images/13-count-consonants.png)](binding-an-aar-images/13-count-consonants.png#lightbox)

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
