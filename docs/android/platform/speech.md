---
title: Распознавание речи для Android
description: В этой статье рассматриваются основы использования очень мощного пространства имен Android. Speech. С момента его создания Android может распознать речь и вывести его в виде текста. Это относительно простой процесс. Тем не менее, преобразование текста в речь является более сложной процедурой, так как не только необходимо учитывать механизм распознавания речи, но и языки, доступные и установленные в системе преобразования текста в речь (TTS).
ms.prod: xamarin
ms.assetid: FA3B8EC4-34D2-47E3-ACEA-BD34B28115B9
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 04/02/2018
ms.openlocfilehash: e8c7d1a4fb3537644ed3b7737158a5e50abcdae5
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73019767"
---
# <a name="android-speech"></a>Распознавание речи для Android

_В этой статье рассматриваются основы использования очень мощного пространства имен Android. Speech. С момента его создания Android может распознать речь и вывести его в виде текста. Это относительно простой процесс. Тем не менее, преобразование текста в речь является более сложной процедурой, так как не только необходимо учитывать механизм распознавания речи, но и языки, доступные и установленные в системе преобразования текста в речь (TTS)._

## <a name="speech-overview"></a>Обзор речи

Наличие системы, «распознает» человеческий голос и енунЦиатес, что вводимо — речь по тексту, а Преобразование текста в речь — это постоянно развивающаяся область в рамках разработки мобильных приложений, так как потребность в естественном взаимодействии с нашими устройствами растет. Существует множество экземпляров, в которых имеется функция, преобразующая текст в речь, или наоборот, является очень полезным инструментом для внедрения в приложение Android.

Например, при использовании на мобильном телефоне с помощью этого зажима пользователи хотят получить бесплатный способ работы с устройствами. Множество различных форм Android, таких как износ Android, и постоянно расширяющееся включение этих устройств Android (например, планшеты и панели заметок), значительно посвящены большим приложениям TTS.

Google предоставляет разработчику обширный набор API-интерфейсов в пространстве имен Android. Speech, чтобы охватить большинство экземпляров, выполняющих распознавание речи устройства (например, программное обеспечение, разработанное для слепых).  Пространство имен включает в себя средство, позволяющее переводить текст в речь с помощью `Android.Speech.Tts`, управлять подсистемой, используемой для преобразования, а также числом `RecognizerIntent`s, которые позволяют преобразовать речь в текст.

Несмотря на то, что средства для распознавания речи будут понятны, существуют ограничения, основанные на используемом оборудовании. Маловероятно, что устройство будет успешно интерпретировать все, что вы направите в него, на каждом из доступных языков.

## <a name="requirements"></a>Требования

Никаких особых требований к этому руководству, кроме устройства с микрофоном и докладчиком, нет.

Ядро устройства Android, обрабатывающее речь, использует `Intent` с соответствующим `OnActivityResult`.
Однако важно понимать, что речь не понимается, но интерпретируется как текст. Разница важна.

### <a name="the-difference-between-understanding-and-interpreting"></a>Разница между пониманием и интерпретацией

Простое определение понимания состоит в том, что вы можете определить по тона и контексту реальное значение того, что говорят. Для интерпретации простоев, чтобы взять слова и вывести их в другую форму.

Рассмотрим следующий простой пример, который используется в повседневном диалоге.

<kbd>Привет, как дела?</kbd>

Без перегиба (выделение для конкретных слов или частей слов) — это простой вопрос. Тем не менее, если к строке применяется более интенсивный темп, лицо, слушающее, обнаружит, что Аскер не слишком счастливо и, возможно, не потребует, чтобы Аскер нехорошо. Если акцент помещается на «—», то пользователь, который запрашивается, обычно больше заинтересован в ответе.

Без довольно мощной обработки звука для использования перегиба и степени искусственного интеллекта (ии) для понимания контекста, программное обеспечение не может даже начать понимание того, что было сказано, а лучший способ — преобразовать речь в текст.

## <a name="setting-up"></a>Настройка

Перед использованием речевой системы всегда разумно проверить, есть ли у устройства микрофон. Попытка запустить приложение на Kindle или Google ноте без установленного микрофона будет незначительной.

В приведенном ниже примере кода демонстрируется, когда доступен микрофон, и, если нет, создается предупреждение. Если на этом этапе нет доступных микрофонов, вы можете прервать действие или отключить возможность записи речи.

```csharp
string rec = Android.Content.PM.PackageManager.FeatureMicrophone;
if (rec != "android.hardware.microphone")
{
    var alert = new AlertDialog.Builder(recButton.Context);
    alert.SetTitle("You don't seem to have a microphone to record with");
    alert.SetPositiveButton("OK", (sender, e) =>
    {
        return;
    });
    alert.Show();
}
```

### <a name="creating-the-intent"></a>Создание цели

Намерение для речевой системы использует определенный тип намерения, называемый `RecognizerIntent`. Это позволяет управлять большим количеством параметров, включая время ожидания бездействия, пока не будет считаться запись, любые дополнительные языки для распознавания и вывода, а также любой текст, включаемый в модальное диалоговое окно `Intent`в качестве способа работы с инструкциями. В этом фрагменте кода `VOICE` — это `readonly int`, используемый для распознавания в `OnActivityResult`.

```csharp
var voiceIntent = new Intent(RecognizerIntent.ActionRecognizeSpeech);
voiceIntent.PutExtra(RecognizerIntent.ExtraLanguageModel, RecognizerIntent.LanguageModelFreeForm);
voiceIntent.PutExtra(RecognizerIntent.ExtraPrompt, Application.Context.GetString(Resource.String.messageSpeakNow));
voiceIntent.PutExtra(RecognizerIntent.ExtraSpeechInputCompleteSilenceLengthMillis, 1500);
voiceIntent.PutExtra(RecognizerIntent.ExtraSpeechInputPossiblyCompleteSilenceLengthMillis, 1500);
voiceIntent.PutExtra(RecognizerIntent.ExtraSpeechInputMinimumLengthMillis, 15000);
voiceIntent.PutExtra(RecognizerIntent.ExtraMaxResults, 1);
voiceIntent.PutExtra(RecognizerIntent.ExtraLanguage, Java.Util.Locale.Default);
StartActivityForResult(voiceIntent, VOICE);
```

### <a name="conversion-of-the-speech"></a>Преобразование речи

Текст, интерпретируемый от речи, будет доставляться в `Intent`, который возвращается после завершения действия и доступа к нему через `GetStringArrayListExtra(RecognizerIntent.ExtraResults)`. Это приведет к возврату `IList<string>`, в котором индекс может использоваться и отображаться в зависимости от числа языков, запрошенных в намерении вызывающего объекта (и указанного в `RecognizerIntent.ExtraMaxResults`). Как и в случае с любым списком, стоит проверить, есть ли данные для отображения.

При прослушивании возвращаемого значения `StartActivityForResult`необходимо указать метод `OnActivityResult`.

В приведенном ниже примере `textBox` — это `TextBox`, используемый для вывода того, что было продиктовано. Это же можно использовать для передачи текста в некоторую форму интерпретатора и из него, если приложение может сравнить текст и ветвь с другой частью приложения.

```csharp
protected override void OnActivityResult(int requestCode, Result resultVal, Intent data)
{
    if (requestCode == VOICE)
    {
        if (resultVal == Result.Ok)
        {
            var matches = data.GetStringArrayListExtra(RecognizerIntent.ExtraResults);
            if (matches.Count != 0)
            {
                string textInput = textBox.Text + matches[0];
                textBox.Text = textInput;
                switch (matches[0].Substring(0, 5).ToLower())
                {
                    case "north":
                        MovePlayer(0);
                        break;
                    case "south":
                        MovePlayer(1);
                        break;
                }
            }
            else
            {
                textBox.Text = "No speech was recognised";
            }
        }
        base.OnActivityResult(requestCode, resultVal, data);
    }
}
```

## <a name="text-to-speech"></a>Преобразование текста в речь

Преобразование текста в речь не является обратным голосом к тексту и зависит от двух ключевых компонентов. модуль преобразования текста в речь, устанавливаемый на устройстве, и устанавливаемый язык.

В основном устройства Android устанавливаются с установленной по умолчанию службой Google TTS и по крайней мере одним языком. Это устанавливается при первой установке устройства и зависит от того, где находится устройство в данный момент (например, Телефон, настроенный в Германии, установит немецкий язык, а в Америке — Американский английский).

### <a name="step-1---instantiating-texttospeech"></a>Шаг 1. Создание экземпляра Тексттоспич

`TextToSpeech` может принимать до 3 параметров, первые два являются обязательными, а третье — необязательным (`AppContext`, `IOnInitListener`, `engine`). Прослушиватель используется для привязки к службе и тестирования сбоя, если подсистема имеет любое количество доступного текста Android для обработчиков речи. Как минимум, устройство будет иметь собственный модуль Google.

### <a name="step-2---finding-the-languages-available"></a>Шаг 2. Поиск доступных языков

Класс `Java.Util.Locale` содержит полезный метод, именуемый `GetAvailableLocales()`. Этот список языков, поддерживаемых модулем распознавания речи, можно протестировать на установленных языках.

Довольно просто создать список понятных языков. Всегда будет использоваться язык по умолчанию (язык, устанавливаемый пользователем при первой установке устройства), поэтому в этом примере `List<string>` имеет значение "по умолчанию" в качестве первого параметра, оставшаяся часть списка будет заполнена в зависимости от результата `textToSpeech.IsLanguageAvailable(locale)`.

```csharp
var langAvailable = new List<string>{ "Default" };
var localesAvailable = Java.Util.Locale.GetAvailableLocales().ToList();
foreach (var locale in localesAvailable)
{
    var res = textToSpeech.IsLanguageAvailable(locale);
    switch (res)
    {
        case LanguageAvailableResult.Available:
          langAvailable.Add(locale.DisplayLanguage);
          break;
        case LanguageAvailableResult.CountryAvailable:
          langAvailable.Add(locale.DisplayLanguage);
          break;
        case LanguageAvailableResult.CountryVarAvailable:
          langAvailable.Add(locale.DisplayLanguage);
          break;
    }
}
langAvailable = langAvailable.OrderBy(t => t).Distinct().ToList();
```

Этот код вызывает [тексттоспич. ислангуажеаваилабле](xref:Android.Speech.Tts.TextToSpeech.IsLanguageAvailable*) для проверки, если языковой пакет для данного языкового стандарта уже существует на устройстве.
Этот метод возвращает [лангуажеаваилаблересулт](xref:Android.Speech.Tts.LanguageAvailableResult), который указывает, доступен ли язык для переданной национальной настройки. Если `LanguageAvailableResult` указывает, что язык `NotSupported`, то для этого языка нет доступных голосовых пакетов (даже для загрузки). Если `LanguageAvailableResult` имеет значение `MissingData`, можно скачать новый языковой пакет, как описано ниже в шаге 4.

### <a name="step-3---setting-the-speed-and-pitch"></a>Шаг 3. задание скорости и высоты

Android позволяет пользователю изменять звук речи, изменяя `SpeechRate` и `Pitch` (скорость и тон речи). Это значение передается от 0 до 1, а "Обычная" речь идет 1 для обоих.

### <a name="step-4---testing-and-loading-new-languages"></a>Шаг 4. Тестирование и загрузка новых языков

Загрузка нового языка выполняется с помощью `Intent`. Результат этой цели приводит к вызову метода [онактивитиресулт](xref:Android.App.Activity.OnActivityResult*) . В отличие от примера преобразования речи в текст (который использовал [рекогнизеринтент](xref:Android.Speech.RecognizerIntent) в качестве параметра `PutExtra` `Intent`), тестирование и загрузка `Intent`s `Action`.

- [Тексттоспич. Engine. актиончеккттсдата](xref:Android.Speech.Tts.TextToSpeech.Engine.ActionCheckTtsData) &ndash; запускает действие из механизма `TextToSpeech` платформы для проверки правильности установки и доступности языковых ресурсов на устройстве.

- [Тексттоспич. Engine. актионинсталлттсдата](xref:Android.Speech.Tts.TextToSpeech.Engine.ActionInstallTtsData) &ndash; запускает действие, предлагающее пользователю загрузить необходимые языки.

В следующем примере кода показано, как использовать эти действия для проверки языковых ресурсов и загрузки нового языка:

```csharp
var checkTTSIntent = new Intent();
checkTTSIntent.SetAction(TextToSpeech.Engine.ActionCheckTtsData);
StartActivityForResult(checkTTSIntent, NeedLang);
//
protected override void OnActivityResult(int req, Result res, Intent data)
{
    if (req == NeedLang)
    {
        var installTTS = new Intent();
        installTTS.SetAction(TextToSpeech.Engine.ActionInstallTtsData);
        StartActivity(installTTS);
    }
}
```

`TextToSpeech.Engine.ActionCheckTtsData` проверяет доступность языковых ресурсов. `OnActivityResult` вызывается при завершении этого теста. Если необходимо загрузить языковые ресурсы, `OnActivityResult` выпустит действие `TextToSpeech.Engine.ActionInstallTtsData`, чтобы запустить действие, позволяющее пользователю загрузить необходимые языки. Обратите внимание, что эта `OnActivityResult` реализация не проверяет код `Result`, поскольку в этом упрощенном примере уже было сделано определение того, что необходимо загрузить языковой пакет.

Действие `TextToSpeech.Engine.ActionInstallTtsData` приводит к предоставлению пользователю действия по **голосовым данным Google TTS** для выбора языков для загрузки:

![Действие с голосовыми данными Google TTS](speech-images/01-google-tts-voice-data.png)

Например, пользователь может выбрать французский язык и щелкнуть значок скачивания, чтобы скачать французские голоса:

![Пример скачивания французского языка](speech-images/02-selecting-french.png)

Установка этих данных происходит автоматически после завершения загрузки.

### <a name="step-5---the-ioninitlistener"></a>Шаг 5. Ионинитлистенер

Чтобы действие могло преобразовать текст в речь, метод интерфейса `OnInit` должен быть реализован (это второй параметр, указанный для создания экземпляра класса `TextToSpeech`). Инициализирует прослушиватель и проверяет результат.

Прослушиватель должен проверять как минимум `OperationResult.Success` и `OperationResult.Failure`.
В следующем примере показано, как это происходит:

```csharp
void TextToSpeech.IOnInitListener.OnInit(OperationResult status)
{
    // if we get an error, default to the default language
    if (status == OperationResult.Error)
        textToSpeech.SetLanguage(Java.Util.Locale.Default);
    // if the listener is ok, set the lang
    if (status == OperationResult.Success)
        textToSpeech.SetLanguage(lang);
}
```

## <a name="summary"></a>Сводка

В этом руководство мы рассмотрели основы преобразования текста в речь и речи в текст и возможные способы их включения в свои приложения. Хотя они не охватывают каждый конкретный случай, вы должны иметь базовое представление о способе интерпретации речи, установке новых языков и увеличении инклусивити приложений.

## <a name="related-links"></a>Связанные ссылки

- [DependencyService Xamarin. Forms](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/dependencyservice//)
- [Преобразование текста в речь (пример)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/platformfeatures-texttospeech)
- [Преобразование речи в текст (пример)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/platformfeatures-speechtotext)
- [Пространство имен Android. Speech](xref:Android.Speech)
- [Пространство имен Android. Speech. TTS](xref:Android.Speech.Tts)
