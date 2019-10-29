---
title: Специальные возможности в Android
ms.prod: xamarin
ms.assetid: 157F0899-4E3E-4538-90AF-B59B8A871204
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/28/2018
ms.openlocfilehash: 6e86663be0bb06697fbfe0e8c360d733bca18da0
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73017018"
---
# <a name="accessibility-on-android"></a>Специальные возможности в Android

На этой странице описывается, как использовать интерфейсы API специальных возможностей Android для создания приложений в соответствии с [контрольным списком специальных возможностей](~/cross-platform/app-fundamentals/accessibility.md).
См. страницы специальных [возможностей iOS](~/ios/app-fundamentals/accessibility.md) и [доступность OS X](~/mac/app-fundamentals/accessibility.md) для других API-интерфейсов платформы.

## <a name="describing-ui-elements"></a>Описание элементов пользовательского интерфейса

Android предоставляет свойство `ContentDescription`, которое используется API чтения экрана для предоставления доступного описания назначения элемента управления.

Описание содержимого можно задать в файле макета AXML C# или.

**C#**

Описание можно задать в коде для любой строки (или строкового ресурса):

```csharp
saveButton.ContentDescription = "Save data";
```

**Макет AXML**

В макетах XML используйте атрибут `android:contentDescription`:

```xml
<ImageButton
    android:id=@+id/saveButton"
    android:src="@drawable/save_image"
    android:contentDescription="Save data" />
```

### <a name="use-hint-for-textview"></a>Использовать указание для TextView

Для `EditText` и `TextView` элементов управления для ввода данных используйте свойство `Hint` для предоставления описания ожидаемого ввода (вместо `ContentDescription`).
При введении какого бы то ни было текста текст будет «Read» вместо подсказки.

**C#**

Задайте свойство `Hint` в коде:

```csharp
someText.Hint = "Enter some text"; // displays (and is "read") when control is empty
```

**Макет AXML**

В XML-файлах макета используется атрибут `android:hint`:

```xml
<EditText
    android:id="@+id/someText"
    android:hint="Enter some text" />
```

### <a name="labelfor-links-input-fields-with-labels"></a>Лабелфор ссылки на поля ввода с метками

Чтобы связать метку с элементом управления вводом данных, используйте свойство `LabelFor` для

**C#**

В C#задайте для свойства `LabelFor` идентификатор ресурса элемента управления, описанного в этом содержимом (обычно это свойство задано для метки и ссылается на другой элемент управления вводом):

```csharp
EditText edit = FindViewById<EditText> (Resource.Id.editFirstName);
TextView tv = FindViewById<TextView> (Resource.Id.labelFirstName);
tv.LabelFor = Resource.Id.editFirstName;
```

**Макет AXML**

В XML-макете используйте свойство `android:labelFor` для ссылки на идентификатор другого элемента управления:

```xml
<TextView
    android:id="@+id/labelFirstName"
    android:hint="Enter some text"
    android:labelFor="@+id/editFirstName" />
<EditText
    android:id="@+id/editFirstName"
    android:hint="Enter some text" />
```

### <a name="announce-for-accessibility"></a>Объявить о специальных возможностях

Используйте метод `AnnounceForAccessibility` для любого элемента управления представления, чтобы сообщить о событии или изменении состояния пользователям при включении специальных возможностей. Этот метод не требуется для большинства операций, в которых Встроенный речевой комментарий предоставляет достаточное количество отзывов, но следует использовать там, где дополнительная информация будет полезной для пользователя.

В приведенном ниже коде показан простой пример вызова `AnnounceForAccessibility`:

```csharp
button.Click += delegate {
  button.Text = string.Format ("{0} clicks!", count++);
  button.AnnounceForAccessibility (button.Text);
};
```

## <a name="changing-focus-settings"></a>Изменение параметров фокуса

Доступная Навигация полагается на элементы управления, которые имеют фокус, чтобы помочь пользователю понять, какие операции доступны. Android предоставляет свойство `Focusable`, которое может помечать элементы управления как, в частности, возможность получать фокус во время навигации.

**C#**

Чтобы предотвратить получение фокуса элементом управления C#, задайте для свойства `Focusable` значение `false`.

```csharp
label.Focusable = false;
```

**Макет AXML**

В XML-файлах макета задайте атрибут `android:focusable`:

```xml
<android:focusable="false" />
```

Можно также управлять порядком фокуса с помощью атрибутов `nextFocusDown`, `nextFocusLeft`, `nextFocusRight` `nextFocusUp`, обычно заданных в макете AXML. Используйте эти атрибуты, чтобы убедиться, что пользователь может легко перемещаться по элементам управления на экране.

## <a name="accessibility-and-localization"></a>Специальные возможности и локализация

В примерах выше указание и описание содержимого задаются непосредственно в отображаемое значение. Предпочтительнее использовать значения в файле **Strings. XML** , например:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="enter_info">Enter some text</string>
    <string name="save_info">Save data</string>
</resources>
```

Использование текста из файла со строками показано ниже в C# файлах макета и AXML:

**C#**

Вместо использования строковых литералов в коде найдите переведенные значения из файлов строк с помощью `Resources.GetText`:

```csharp
someText.Hint = Resources.GetText (Resource.String.enter_info);
saveButton.ContentDescription = Resources.GetText (Resource.String.save_info);
```

**AXML**

В атрибутах доступа XML макета, таких как `hint` и `contentDescription`, можно задать строковый идентификатор:

```xml
<TextView
    android:id="@+id/someText"
    android:hint="@string/enter_info" />
<ImageButton
    android:id=@+id/saveButton"
    android:src="@drawable/save_image"
    android:contentDescription="@string/save_info" />
```

Преимущество хранения текста в отдельном файле заключается в том, что в приложении может быть предоставлено несколько языковых переводов файла. Дополнительные сведения о добавлении локализованных строковых файлов в проект приложения см. в статье [руководство по локализации Android](~/android/app-fundamentals/localization.md) .

## <a name="testing-accessibility"></a>Тестирование специальных возможностей

Выполните следующие [действия](https://developer.android.com/training/accessibility/testing.html#how-to) , чтобы включить Талкбакк и исследовать с помощью сенсорного ввода для проверки специальных возможностей на устройствах Android.

Может потребоваться установить [талкбакк](https://play.google.com/store/apps/details?id=com.google.android.marvin.talkback) из Google Play, если он не отображается в окне **Параметры > Специальные возможности**.

## <a name="related-links"></a>Связанные ссылки

- [Кросс-платформенные специальные возможности](~/cross-platform/app-fundamentals/accessibility.md)
- [Интерфейсы API специальных возможностей для Android](https://developer.android.com/guide/topics/ui/accessibility/index.html)
