---
title: Звук Android
description: ОС Android обеспечивает расширенную поддержку мультимедиа, охватывающую как аудио, так и видео. В этом руководстве рассказывается о звуках в Android и рассматривается воспроизведение и запись звука с помощью встроенных классов аудио Player и записывающего класса, а также низкоуровневого аудио API. Кроме того, в нем рассматривается работа с звуковыми событиями в других приложениях, что позволяет разработчикам создавать хорошо работающие приложения.
ms.prod: xamarin
ms.assetid: 646ED563-C34E-256D-4B56-29EE99881C27
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/28/2018
ms.openlocfilehash: ac59bcbb062dc9dae784d18054048f50094c2145
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73018067"
---
# <a name="android-audio"></a>Звук Android

_ОС Android обеспечивает расширенную поддержку мультимедиа, охватывающую как аудио, так и видео. В этом руководстве рассказывается о звуках в Android и рассматривается воспроизведение и запись звука с помощью встроенных классов аудио Player и записывающего класса, а также низкоуровневого аудио API. Кроме того, в нем рассматривается работа с звуковыми событиями в других приложениях, что позволяет разработчикам создавать хорошо работающие приложения._

## <a name="overview"></a>Обзор

Современные мобильные устройства обладают функциональными возможностями, которые раньше использовали выделенные компоненты оборудования &ndash; камер, музыкальные проигрыватели и средства записи видео. По этой причине мультимедийные платформы стали функцией первого класса в мобильных интерфейсах API.

Android обеспечивает расширенную поддержку мультимедиа. В этой статье рассматривается работа с аудио в Android и рассматриваются следующие темы.

1. **Воспроизведение аудио с помощью MediaPlayer** &ndash; использование встроенного класса `MediaPlayer` для воспроизведения звуковых файлов, включая локальные аудиофайлы и потоковые аудиофайлы с помощью класса `AudioTrack`.

2. Запись **аудио** &ndash; с помощью встроенного класса `MediaRecorder` для записи звука.

3. **Работа с звуковыми уведомлениями** &ndash; использование звуковых уведомлений для создания правильно настроенных приложений, которые правильно реагируют на события (например, входящие телефонные вызовы) путем приостановки или отмены звуковых выходов.

4. **Работа с аудио &ndash; низкого уровня** воспроизведение звука с помощью класса `AudioTrack` путем записи непосредственно в буферы памяти. Запись звука с помощью класса `AudioRecord` и чтение непосредственно из буферов памяти.

## <a name="requirements"></a>Требования

Для работы с этим руководством требуется Android 2,0 (API уровня 5) или более поздней версии. Обратите внимание, что отладка звука на Android должна выполняться на устройстве.

Необходимо запросить разрешения `RECORD_AUDIO` в **AndroidManifest. XML**:

![Раздел "необходимые разрешения" манифеста Android с включенной ЗАПИСЬю\_AUDIO](android-audio-images/image01.png)

## <a name="playing-audio-with-the-mediaplayer-class"></a>Воспроизведение звука с помощью класса MediaPlayer

Самым простым способом воспроизведения звука в Android является встроенный класс [MediaPlayer](xref:Android.Media.MediaPlayer) .
`MediaPlayer` могут играть локальные или удаленные файлы путем передачи пути к файлу. Однако `MediaPlayer` является очень чувствительным к состоянию и вызов одного из его методов в неправильном состоянии приведет к возникновению исключения. Важно взаимодействовать с `MediaPlayer` в порядке, описанном ниже, чтобы избежать ошибок.

### <a name="initializing-and-playing"></a>Инициализация и воспроизведение

Для воспроизведения звука с `MediaPlayer` требуется следующая последовательность:

1. Создайте экземпляр нового объекта [MediaPlayer](xref:Android.Media.MediaPlayer) .

1. Настройте файл для воспроизведения с помощью метода [сетдатасаурце](xref:Android.Media.MediaPlayer.SetDataSource*) .

1. Вызовите метод [Prepare](xref:Android.Media.MediaPlayer.Prepare) , чтобы инициализировать проигрыватель.

1. Вызовите метод [Start](xref:Android.Media.MediaPlayer.Start) , чтобы начать воспроизведение звука.

Это использование показано в приведенном ниже примере кода.

```csharp
protected MediaPlayer player;
public void StartPlayer(String  filePath)
{
  if (player == null) {
    player = new MediaPlayer();
  } else {
    player.Reset();
    player.SetDataSource(filePath);
    player.Prepare();
    player.Start();
  }
}
```

### <a name="suspending-and-resuming-playback"></a>Приостановка и возобновление воспроизведения

Воспроизведение можно приостановить, вызвав метод [Pause](xref:Android.Media.MediaPlayer.Pause) :

```csharp
player.Pause();
```

Чтобы возобновить приостановленное воспроизведение, вызовите метод [Start](xref:Android.Media.MediaPlayer.Start) .
Это будет возобновлено из приостановленного места воспроизведения:

```csharp
player.Start();
```

Вызов метода [останавливает](xref:Android.Media.MediaPlayer.Stop) на проигрывателе завершает текущее воспроизведение:

```csharp
player.Stop();
```

Если проигрыватель больше не нужен, необходимо освободить ресурсы, вызвав метод [Release](xref:Android.Media.MediaPlayer.Release) :

```csharp
player.Release();
```

## <a name="using-the-mediarecorder-class-to-record-audio"></a>Использование класса Медиарекордер для записи звука

Чтобы `MediaPlayer` для записи звука в Android, используется класс [Медиарекордер](xref:Android.Media.MediaRecorder) . Как и `MediaPlayer`, оно чувствительно к состоянию и переходит по нескольким состояниям, чтобы получить точку, с которой он может начать запись. Для записи звука необходимо задать разрешение `RECORD_AUDIO`. Инструкции по настройке разрешений приложений см. в статье [Работа с AndroidManifest. XML](~/android/platform/android-manifest.md).

### <a name="initializing-and-recording"></a>Инициализация и запись

Для записи звука с `MediaRecorder` необходимо выполнить следующие действия.

1. Создайте экземпляр нового объекта [Медиарекордер](xref:Android.Media.MediaRecorder) .

2. Укажите, какое аппаратное устройство следует использовать для записи звуковых данных с помощью метода [сетаудиосаурце](xref:Android.Media.MediaRecorder.SetAudioSource*) .

3. Задайте звуковой формат выходного файла с помощью метода [сетаутпутформат](xref:Android.Media.MediaRecorder.SetOutputFormat*) . Список поддерживаемых типов аудио см. в статье [форматы мультимедиа, поддерживаемые Android](https://developer.android.com/guide/appendix/media-formats.html).

4. Вызовите метод [сетаудиоенкодер](xref:Android.Media.MediaRecorder.SetAudioEncoder*) , чтобы задать тип кодирования звука.

5. Вызовите метод [сетаутпутфиле](xref:Android.Media.MediaRecorder.SetOutputFile*) , чтобы указать имя выходного файла, в который записываются звуковые данные.

6. Вызовите метод [Prepare](xref:Android.Media.MediaRecorder.Prepare) , чтобы инициализировать средство записи.

7. Чтобы начать запись, вызовите метод [Start](xref:Android.Media.MediaRecorder.Start) .

Эта последовательность показана в следующем образце кода:

```csharp
protected MediaRecorder recorder;
void RecordAudio (String filePath)
{
  try {
    if (File.Exists (filePath)) {
      File.Delete (filePath);
    }
    if (recorder == null) {
      recorder = new MediaRecorder (); // Initial state.
    } else {
      recorder.Reset ();
      recorder.SetAudioSource (AudioSource.Mic);
      recorder.SetOutputFormat (OutputFormat.ThreeGpp);
      recorder.SetAudioEncoder (AudioEncoder.AmrNb);
      // Initialized state.
      recorder.SetOutputFile (filePath);
      // DataSourceConfigured state.
      recorder.Prepare (); // Prepared state
      recorder.Start (); // Recording state.
    }
  } catch (Exception ex) {
    Console.Out.WriteLine( ex.StackTrace);
  }
}
```

### <a name="stopping-recording"></a>Остановка записи

Чтобы прерывать запись, вызовите метод `Stop` на `MediaRecorder`:

```csharp
recorder.Stop();
```

### <a name="cleaning-up"></a>Очистка

После остановки `MediaRecorder` вызовите метод [Reset](xref:Android.Media.MediaRecorder.Reset) , чтобы вернуть его в состояние простоя:

```csharp
recorder.Reset();
```

Если `MediaRecorder` больше не требуется, его ресурсы необходимо освободить, вызвав метод [Release](xref:Android.Media.MediaRecorder.Release) :

```csharp
recorder.Release();
```

## <a name="managing-audio-notifications"></a>Управление звуковыми уведомлениями

### <a name="the-audiomanager-class"></a>Класс Аудиоманажер

Класс [аудиоманажер](xref:Android.Media.AudioManager) предоставляет доступ к звуковым уведомлениям, которые позволяют приложениям узнавать о возникновении звуковых событий. Эта служба также предоставляет доступ к другим звуковым функциям, таким как управление режимами громкости и звонка. `AudioManager` позволяет приложению работать с звуковыми уведомлениями для управления воспроизведением звука.

### <a name="managing-audio-focus"></a>Управление фокусом звука

Звуковые ресурсы устройства (встроенные проигрыватель и устройство записи) совместно используются всеми работающими приложениями.

По сути, это похоже на приложения на настольном компьютере, где только одно приложение имеет фокус клавиатуры. После выбора одного из выполняющихся приложений при щелчке мышью, ввод с клавиатуры поступает только в это приложение.

Фокус звука является похожей идеей и предотвращает одновременное воспроизведение или запись звука несколькими приложениями. Это сложнее, чем фокус клавиатуры, так как оно является добровольным &ndash; приложение может игнорировать тот факт, что в настоящее время он не имеет фокуса и воспроизводится независимо от &ndash; и из-за наличия различных типов звуковых фокусов, которые могут быть запрошены. Например, если ожидается, что запрашивающий должен воспроизводить звук в течение очень короткого времени, он может запросить временный фокус.

Фокус звука может быть предоставлен немедленно или изначально отклонен и предоставлен позже. Например, если приложение запрашивает фокус ввода во время звонка, оно будет отклонено, но после завершения звонка фокус может быть предоставлен. В этом случае прослушиватель регистрируется, чтобы ответить соответствующим образом при отсутствии звука. Запрос на запись звука используется для определения того, можно ли воспроизвести или записать звук.

Дополнительные сведения о звуковых фокусах см. в разделе [Управление звуковым фокусом](https://developer.android.com/training/managing-audio/audio-focus.html).

#### <a name="registering-the-callback-for-audio-focus"></a>Регистрация обратного вызова для звукового фокуса

Регистрация обратного вызова `FocusChangeListener` из `IOnAudioChangeListener` является важной частью получения и снятия фокуса звука. Это связано с тем, что предоставление фокуса звука может быть отложено до последующего времени. Например, приложение может запрашивать воспроизведение музыки, пока выполняется телефонный звонок. Фокус звука не будет предоставляться до завершения звонка.

По этой причине объект обратного вызова передается как параметр в метод `GetAudioFocus` `AudioManager`, и это вызов, регистрирующий обратный вызов. Если фокус звука изначально был отклонен, но позже предоставлен, приложение уведомляется, вызывая `OnAudioFocusChange` для обратного вызова. Этот же метод используется для информирования приложения о том, что он находится в процессе записи.

Когда приложение завершит использование звуковых ресурсов, оно вызывает метод `AbandonFocus` `AudioManager`и снова передает обратный вызов. Это отменит обратный вызов и освободит звуковые ресурсы, чтобы другие приложения могли получить фокус звука.

#### <a name="requesting-audio-focus"></a>Запрос фокуса звука

Ниже приведены шаги, необходимые для запроса звуковых ресурсов устройства.

1. Получение маркера для системной службы `AudioManager`.

2. Создайте экземпляр класса обратного вызова.

3. Запросите звуковые ресурсы устройства, вызвав метод `RequestAudioFocus` на `AudioManager`. Параметры — это объект обратного вызова, тип потока (музыка, голосовой вызов, кольцо и т. д.) и тип запрашиваемого права доступа (например, звуковые ресурсы можно запросить мгновенно или в течение неопределенного периода времени).

4. Если запрос предоставлен, метод `playMusic` вызывается немедленно, и начнется воспроизведение звука.

5. Если запрос отклоняется, дальнейшие действия не выполняются. В этом случае звук будет воспроизводиться, только если запрос будет предоставлен позже.

Следующие шаги показаны в примере кода ниже.

```csharp
Boolean RequestAudioResources(INotificationReceiver parent)
{
  AudioManager audioMan = (AudioManager) GetSystemService(Context.AudioService);
  AudioManager.IOnAudioFocusChangeListener listener  = new MyAudioListener(this);
  var ret = audioMan.RequestAudioFocus (listener, Stream.Music, AudioFocus.Gain );
  if (ret == AudioFocusRequest.Granted) {
    playMusic();
    return (true);
  } else if (ret == AudioFocusRequest.Failed) {
    return (false);
  }
  return (false);
}
```

#### <a name="releasing-audio-focus"></a>Освобождение фокуса звука

После завершения воспроизведения записи вызывается метод `AbandonFocus` в `AudioManager`. Это позволяет другому приложению получить звуковые ресурсы устройства. Другие приложения получат уведомление об изменении фокуса звука, если они зарегистрировали свои собственные прослушиватели.

## <a name="low-level-audio-api"></a>Аудио API низкого уровня

Низкоуровневые API низкого уровня обеспечивают больший контроль над воспроизведением и записью звука, так как они взаимодействуют непосредственно с буферами памяти вместо использования URI файлов. Существует несколько сценариев, в которых предпочтительнее использовать такой подход. К таким сценариям относятся:

1. При воспроизведении из зашифрованных звуковых файлов.

2. При воспроизведении коротких клипов.

3. Потоковая передача звука.

### <a name="audiotrack-class"></a>Класс AudioTrack

Класс [AudioTrack](xref:Android.Media.AudioTrack) использует низкоуровневые API низкого уровня для записи, а является аналогом класса `MediaPlayer`.

#### <a name="initializing-and-playing"></a>Инициализация и воспроизведение

Для воспроизведения звука должен быть создан новый экземпляр `AudioTrack`. В списке аргументов, переданном в [конструктор](xref:Android.Media.AudioTrack) , указывается способ воспроизведения звукового примера, содержащегося в буфере. Аргументы:

1. Тип потока &ndash; голоса, мелодии звонка, музыки, системы или оповещения.

2. Частота, &ndash;ая частотой выборки, выраженная в Гц.

3. Конфигурация канала &ndash; Mono или стерео.

4. Формат аудио &ndash; 8 бит или 16 бит кодирования.

5. Размер буфера &ndash; в байтах.

6. Режим буфера &ndash; потоковой или статической.

После создания вызывается метод [Play](xref:Android.Media.AudioTrack.Play) `AudioTrack`, чтобы настроить его для начала воспроизведения. Запись аудио буфера в `AudioTrack` запускает воспроизведение:

```csharp
void PlayAudioTrack(byte[] audioBuffer)
{
  AudioTrack audioTrack = new AudioTrack(
    // Stream type
    Stream.Music,
    // Frequency
    11025,
    // Mono or stereo
    ChannelOut.Mono,
    // Audio encoding
    Android.Media.Encoding.Pcm16bit,
    // Length of the audio clip.
    audioBuffer.Length,
    // Mode. Stream or static.
    AudioTrackMode.Stream);

    audioTrack.Play();
    audioTrack.Write(audioBuffer, 0, audioBuffer.Length);
}
```

#### <a name="pausing-and-stopping-the-playback"></a>Приостановка и остановка воспроизведения

Чтобы приостановить воспроизведение, вызовите метод [Pause](xref:Android.Media.AudioTrack.Pause) :

```csharp
audioTrack.Pause();
```

Вызов метода [остановить](xref:Android.Media.AudioTrack.Stop) приведет к окончательному завершению воспроизведения:

```csharp
audioTrack.Stop();
```

#### <a name="cleanup"></a>Чистку

Если `AudioTrack` больше не требуется, его ресурсы должны быть освобождены вызовом [Release](xref:Android.Media.AudioTrack.Release):

```csharp
audioTrack.Release();
```

### <a name="the-audiorecord-class"></a>Класс Аудиорекорд

Класс [аудиорекорд](xref:Android.Media.AudioRecord) эквивалентен `AudioTrack` на стороне записи. Как и `AudioTrack`, он использует буферы памяти непосредственно вместо файлов и URI. Для этого требуется, чтобы в манифесте было задано разрешение `RECORD_AUDIO`.

#### <a name="initializing-and-recording"></a>Инициализация и запись

Первым шагом является создание нового объекта [аудиорекорд](xref:Android.Media.AudioRecord) . Список аргументов, переданный в [конструктор](xref:Android.Media.AudioRecord) , предоставляет всю информацию, необходимую для записи. В отличие от `AudioTrack`, где аргументы являются главными перечислениями, эквивалентными аргументами в `AudioRecord` являются целые числа. Сюда входит следующее.

1. Аппаратный источник входных данных, например микрофон.

2. Тип потока &ndash; голоса, мелодии звонка, музыки, системы или оповещения.

3. Частота, &ndash;ая частотой выборки, выраженная в Гц.

4. Конфигурация канала &ndash; Mono или стерео.

5. Формат аудио &ndash; 8 бит или 16 бит кодирования.

6. Размер буфера — в байтах

После создания `AudioRecord` вызывается его метод [стартрекординг](xref:Android.Media.AudioRecord.StartRecording) . Теперь она готова начать запись. `AudioRecord` постоянно считывает аудио буфер для ввода и записывает эти данные в звуковой файл.

```csharp
void RecordAudio()
{
  byte[] audioBuffer = new byte[100000];
  var audRecorder = new AudioRecord(
    // Hardware source of recording.
    AudioSource.Mic,
    // Frequency
    11025,
    // Mono or stereo
    ChannelIn.Mono,
    // Audio encoding
    Android.Media.Encoding.Pcm16bit,
    // Length of the audio clip.
    audioBuffer.Length
  );
  audRecorder.StartRecording();
  while (true) {
    try
    {
      // Keep reading the buffer while there is audio input.
      audRecorder.Read(audioBuffer, 0, audioBuffer.Length);
      // Write out the audio file.
    } catch (Exception ex) {
      Console.Out.WriteLine(ex.Message);
      break;
    }
  }
}
```

#### <a name="stopping-the-recording"></a>Остановка записи

Вызов метода [остановить](xref:Android.Media.AudioRecord.Stop) завершает запись:

```csharp
audRecorder.Stop();
```

#### <a name="cleanup"></a>Чистку

Если объект `AudioRecord` больше не нужен, вызов метода [выпуска](xref:Android.Media.AudioRecord.Release) освобождает все связанные с ним ресурсы:

```csharp
audRecorder.Release();
```

## <a name="summary"></a>Сводка

ОС Android предоставляет мощную платформу для воспроизведения, записи и управления аудио. В этой статье мы рассмотрели, как воспроизвести и записать звук с помощью классов `MediaPlayer` и `MediaRecorder` высокого уровня. Далее рассматривается использование звуковых уведомлений для совместного использования звуковых ресурсов устройства различными приложениями. Наконец, она посвящена тому, как воспроизводить и записывать аудио с помощью низкоуровневых интерфейсов API, интерфейс которых напрямую использует буферы памяти.

## <a name="related-links"></a>Связанные ссылки

- [Работа с аудио (пример)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/example-workingwithaudio)
- [Проигрыватель мультимедиа](xref:Android.Media.MediaPlayer)
- [Устройство записи мультимедиа](xref:Android.Media.MediaRecorder)
- [Диспетчер аудио](xref:Android.Media.AudioManager)
- [Звуковая дорожка](xref:Android.Media.AudioTrack)
- [Устройство записи звука](xref:Android.Media.AudioRecord)
