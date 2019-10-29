---
title: Ручные элементы управления камерой в Xamarin. iOS
description: В этом документе описывается, как можно использовать платформу iOS Авфаундатион Framework с Xamarin. iOS для включения элементов управления камерой вручную. Ручные элементы управления камерой позволяют пользователю управлять фокусом, белым балансом и параметрами экспозиции.
ms.prod: xamarin
ms.assetid: 56340225-5F3C-4BFC-9A79-61496D7FE5B5
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/22/2017
ms.openlocfilehash: 1ae7e4c39a07ccfcb472f7add580bf5109166c3d
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73022062"
---
# <a name="manual-camera-controls-in-xamarinios"></a>Ручные элементы управления камерой в Xamarin. iOS

Ручная камера, предоставляемая `AVFoundation Framework` в iOS 8, позволяет мобильному приложению получить полный контроль над камерой устройства iOS. Такой детализированный уровень контроля можно использовать для создания высококачественных приложений камеры и предоставления самокомпозиций путем настройки параметров камеры с сохранением изображения или видео.

Эти элементы управления также могут быть полезны при разработке научных или промышленных приложений, где результаты менее приспособлены к правильности или характеристикам изображения, и в них более подробно описаны некоторые функции или элементы создаваемого образа.

## <a name="avfoundation-capture-objects"></a>Объекты отслеживания Авфаундатион

Независимо от того, используете ли видеоизображение или изображение на устройстве iOS, процесс, используемый для записи этих образов, практически одинаков. Это справедливо для приложений, использующих автоматизированные элементы управления камерой по умолчанию или использующие преимущества новых ручных элементов управления камеры:

 [![](intro-to-manual-camera-controls-images/image1.png "AVFoundation Capture Objects overview")](intro-to-manual-camera-controls-images/image1.png#lightbox)

Входные данные берутся из `AVCaptureDeviceInput` в `AVCaptureSession` с помощью `AVCaptureConnection`. Результат выводится как изображение по-прежнему или как поток видео. Весь процесс управляется `AVCaptureDevice`.

## <a name="manual-controls-provided"></a>Предоставлены ручные элементы управления

С помощью новых API, предоставляемых iOS 8, приложение может контролировать следующие возможности камеры:

- **Ручная фокусировка** — благодаря тому, что конечный пользователь может управлять фокусом напрямую, приложение предоставляет больший контроль над созданным образом.
- **Ручная раскрытие** — предоставление возможности ручного управления раскрытием позволяет приложению предоставить больше возможностей для пользователей и позволить им добиться стилизации.
- **Ручной баланс белого** — белый баланс используется для корректировки цвета в изображении — часто, чтобы сделать его реалистичным. Различные источники освещения имеют разные цветовые температуры, а параметры камеры, используемые для записи образа, корректируются для компенсации этих различий. Опять же, разрешая пользователю управлять балансом белого, пользователи могут вносить корректировки, которые не могут быть выполнены автоматически.

iOS 8 предоставляет расширения и усовершенствования существующих интерфейсов API iOS, чтобы обеспечить точный контроль над процессом отслеживания изображений.

## <a name="bracketed-capture"></a>Захват в квадратных скобках

Захват в квадратных скобках основан на параметрах из элементов управления ручной камеры, представленных выше, и позволяет приложению захватывать мгновенные данные в течение определенного времени различными способами.

Как и было сказано, захват в квадратных скобках — это более неподвижные изображения с различными параметрами изображения.

## <a name="requirements"></a>Требования

Для выполнения действий, описанных в этой статье, необходимо выполнить следующие действия.

- **Xcode 7 + и iOS 8 или более поздней версии** — на компьютере разработчика должны быть установлены и настроены API Apple Xcode 7 и iOS 8 или более поздней версии.
- **Visual Studio для Mac** — на устройстве пользователя должна быть установлена и настроена последняя версия Visual Studio для Mac.
- **устройство iOS 8** — устройство iOS под управлением последней версии iOS 8. Не удается протестировать функции камеры в симуляторе iOS.

## <a name="general-av-capture-setup"></a>Общая Настройка записи вирусов

При записи видео на устройстве iOS необходимо всегда иметь общий код настройки. В этом разделе рассматривается минимальная настройка, необходимая для записи видео с камеры устройства iOS и отображение видео в режиме реального времени в `UIImageView`.

### <a name="output-sample-buffer-delegate"></a>Выходные данные делегата образца буфера

Одним из первых вещей является делегат для отслеживания примера выходного буфера и отображения изображения, извлеченного из буфера, в `UIImageView` в пользовательском интерфейсе приложения.

Следующая подпрограммы будет отслеживать буфер выборки и обновлять пользовательский интерфейс:

```csharp
using System;
using Foundation;
using UIKit;
using System.CodeDom.Compiler;
using System.Collections.Generic;
using System.Linq;
using AVFoundation;
using CoreVideo;
using CoreMedia;
using CoreGraphics;

namespace ManualCameraControls
{
    public class OutputRecorder : AVCaptureVideoDataOutputSampleBufferDelegate
    {
        #region Computed Properties
        public UIImageView DisplayView { get; set; }
        #endregion

        #region Constructors
        public OutputRecorder ()
        {

        }
        #endregion

        #region Private Methods
        private UIImage GetImageFromSampleBuffer(CMSampleBuffer sampleBuffer) {

            // Get a pixel buffer from the sample buffer
            using (var pixelBuffer = sampleBuffer.GetImageBuffer () as CVPixelBuffer) {
                // Lock the base address
                pixelBuffer.Lock (0);

                // Prepare to decode buffer
                var flags = CGBitmapFlags.PremultipliedFirst | CGBitmapFlags.ByteOrder32Little;

                // Decode buffer - Create a new colorspace
                using (var cs = CGColorSpace.CreateDeviceRGB ()) {

                    // Create new context from buffer
                    using (var context = new CGBitmapContext (pixelBuffer.BaseAddress,
                        pixelBuffer.Width,
                        pixelBuffer.Height,
                        8,
                        pixelBuffer.BytesPerRow,
                        cs,
                        (CGImageAlphaInfo)flags)) {

                        // Get the image from the context
                        using (var cgImage = context.ToImage ()) {

                            // Unlock and return image
                            pixelBuffer.Unlock (0);
                            return UIImage.FromImage (cgImage);
                        }
                    }
                }
            }
        }
        #endregion

        #region Override Methods
        public override void DidOutputSampleBuffer (AVCaptureOutput captureOutput, CMSampleBuffer sampleBuffer, AVCaptureConnection connection)
        {
            // Trap all errors
            try {
                // Grab an image from the buffer
                var image = GetImageFromSampleBuffer(sampleBuffer);

                // Display the image
                if (DisplayView !=null) {
                    DisplayView.BeginInvokeOnMainThread(() => {
                        // Set the image
                        if (DisplayView.Image != null) DisplayView.Image.Dispose();
                        DisplayView.Image = image;

                        // Rotate image to the correct display orientation
                        DisplayView.Transform = CGAffineTransform.MakeRotation((float)Math.PI/2);
                    });
                }

                // IMPORTANT: You must release the buffer because AVFoundation has a fixed number
                // of buffers and will stop delivering frames if it runs out.
                sampleBuffer.Dispose();
            }
            catch(Exception e) {
                // Report error
                Console.WriteLine ("Error sampling buffer: {0}", e.Message);
            }
        }
        #endregion
    }
}
```

С помощью этой подпрограммы можно изменить `AppDelegate`, чтобы открыть сеанс записи AV для записи канала видео в реальном времени.

### <a name="creating-an-av-capture-session"></a>Создание сеанса записи вирусов

Сеанс записи вирусов-кадров используется для управления записью в реальном времени с камеры устройства iOS и требуется для получения видео в приложение iOS. Так как пример приложения `ManualCameraControl` использует сеанс записи в нескольких разных местах, он будет настроен в `AppDelegate` и станет доступным для всего приложения.

Чтобы изменить `AppDelegate` приложения и добавить необходимый код, выполните следующие действия.

1. Дважды щелкните файл `AppDelegate.cs` в обозреватель решений, чтобы открыть его для редактирования.
1. Добавьте следующие операторы using в начало файла:

    ```csharp
    using System;
    using Foundation;
    using UIKit;
    using System.CodeDom.Compiler;
    using System.Collections.Generic;
    using System.Linq;
    using AVFoundation;
    using CoreVideo;
    using CoreMedia;
    using CoreGraphics;
    using CoreFoundation;
    ```

1. Добавьте следующие частные переменные и вычисленные свойства в класс `AppDelegate`:

    ```csharp
    #region Private Variables
    private NSError Error;
    #endregion

    #region Computed Properties
    public override UIWindow Window {get;set;}
    public bool CameraAvailable { get; set; }
    public AVCaptureSession Session { get; set; }
    public AVCaptureDevice CaptureDevice { get; set; }
    public OutputRecorder Recorder { get; set; }
    public DispatchQueue Queue { get; set; }
    public AVCaptureDeviceInput Input { get; set; }
    #endregion
    ```

1. Переопределите метод завершения и измените его на:

    ```csharp
    public override void FinishedLaunching (UIApplication application)
    {
        // Create a new capture session
        Session = new AVCaptureSession ();
        Session.SessionPreset = AVCaptureSession.PresetMedium;

        // Create a device input
        CaptureDevice = AVCaptureDevice.DefaultDeviceWithMediaType (AVMediaType.Video);
        if (CaptureDevice == null) {
            // Video capture not supported, abort
            Console.WriteLine ("Video recording not supported on this device");
            CameraAvailable = false;
            return;
        }

        // Prepare device for configuration
        CaptureDevice.LockForConfiguration (out Error);
        if (Error != null) {
            // There has been an issue, abort
            Console.WriteLine ("Error: {0}", Error.LocalizedDescription);
            CaptureDevice.UnlockForConfiguration ();
            return;
        }

        // Configure stream for 15 frames per second (fps)
        CaptureDevice.ActiveVideoMinFrameDuration = new CMTime (1, 15);

        // Unlock configuration
        CaptureDevice.UnlockForConfiguration ();

        // Get input from capture device
        Input = AVCaptureDeviceInput.FromDevice (CaptureDevice);
        if (Input == null) {
            // Error, report and abort
            Console.WriteLine ("Unable to gain input from capture device.");
            CameraAvailable = false;
            return;
        }

        // Attach input to session
        Session.AddInput (Input);

        // Create a new output
        var output = new AVCaptureVideoDataOutput ();
        var settings = new AVVideoSettingsUncompressed ();
        settings.PixelFormatType = CVPixelFormatType.CV32BGRA;
        output.WeakVideoSettings = settings.Dictionary;

        // Configure and attach to the output to the session
        Queue = new DispatchQueue ("ManCamQueue");
        Recorder = new OutputRecorder ();
        output.SetSampleBufferDelegate (Recorder, Queue);
        Session.AddOutput (output);

        // Let tabs know that a camera is available
        CameraAvailable = true;
    }
    ```

1. Сохраните изменения в файле.

С помощью этого кода можно легко реализовать ручное управление камерой для экспериментов и тестирования.

## <a name="manual-focus"></a>Ручной фокус

Позволяя конечному пользователю напрямую получать элементы управления фокусом, приложение может предоставить более художественный контроль над создаваемым изображением.

Например, профессиональный фотографом может смягчить фокусировку изображения для достижения [бокеного воздействия](https://en.wikipedia.org/wiki/Bokeh):

[![](intro-to-manual-camera-controls-images/image2.png "A Bokeh Effect")](intro-to-manual-camera-controls-images/image2.png#lightbox)

Или создайте фокус на [броскую фокусировку](http://www.mediacollege.com/video/camera/focus/pull.html), например:

[![](intro-to-manual-camera-controls-images/image3.png "The Focus Pull Effect")](intro-to-manual-camera-controls-images/image3.png#lightbox)

Для специалистов или средств записи медицинских приложений приложению может потребоваться Программное перемещение линзы для экспериментов. В любом случае новый API позволяет конечному пользователю или приложению получить контроль над фокусом во время создания образа.

### <a name="how-focus-works"></a>Как работает фокус

Прежде чем обсуждать сведения об управлении фокусом в приложении IOS 8. Давайте кратко рассмотрим, как работает фокус на устройстве iOS:

[![](intro-to-manual-camera-controls-images/image4.png "How focus works in an iOS device")](intro-to-manual-camera-controls-images/image4.png#lightbox)

Лампочка входит в объектив камеры на устройстве iOS и помещается на датчике изображения. Расстояние от элемента управления датчиков, где точка фокуса (область, в которой изображение будет выглядеть самым самым четким) — связь с датчиком. Чем дальше линза от датчика, объекты расстояния кажутся более четкими, а ближе близко к объектам кажутся наиболее четкими.

На устройстве iOS линза перемещается ближе к датчику или к нему с помощью магнитов и пружины. В результате точное позиционирование линзы невозможно, так как оно будет меняться от устройства к устройству и может быть затронуто такими параметрами, как ориентация устройства или возраст устройства и пружина.

### <a name="important-focus-terms"></a>Важные условия фокусировки

При работе с фокусом существует несколько терминов, с которыми разработчик должен быть знаком.

- **Глубина поля** — расстояние между ближайшим и крайним числом объектов, которые находятся в фокусе.
- **Макрос** — это приближается к концу спектра фокуса и представляет собой ближайшее расстояние, на которое может сосредоточиться объектив.
- **Бесконечность** — это дальний конец спектра фокуса и является самым крайним расстоянием, на которое может сосредоточиться объектив.
- **Расстояние между** фокусами — это точка в спектре, где самый крайний объект в кадре находится на самом конце фокуса. Иными словами, это фокусное расположение, которое увеличивает глубину поля.
- **Позицией линзы** — это то, что управляет всеми другими условиями. Это расстояние линзы от датчика и, следовательно, контроллер фокусировки.

С учетом этих условий и знаний новые элементы управления фокусом ввода вручную могут быть успешно реализованы в приложении iOS 8.

### <a name="existing-focus-controls"></a>Существующие элементы управления фокусом

iOS 7 и более ранние версии предоставили существующие элементы управления фокусом через свойство `FocusMode`как:

- `AVCaptureFocusModeLocked` — фокус заблокирован в одной точке фокусировки.
- `AVCaptureFocusModeAutoFocus` — Камера выводит объектив на все фокальные точки, пока не найдет четкий фокус и не останется там.
- `AVCaptureFocusModeContinuousAutoFocus` — Камера подписывается при обнаружении условия отсутствия фокуса.

Существующие элементы управления также предоставили настраиваемую точку интересов с помощью свойства`FocusPointOfInterest`, чтобы пользователь мог сосредоточиться на определенной области. Приложение также может отслеживать перемещение линзы, отслеживая свойство `IsAdjustingFocus`.

Кроме того, ограничение диапазона было предоставлено свойством `AutoFocusRangeRestriction` следующим образом:

- `AVCaptureAutoFocusRangeRestrictionNear` — позволяет ограничивать автофокус до ближайших глубин. Полезен в таких ситуациях, как сканирование QR-кода или штрихкода.
- `AVCaptureAutoFocusRangeRestrictionFar` — ограничивают автофокус до удаленных глубин. Полезен в ситуациях, когда объекты, которые являются ненужными, находятся в поле представления (например, фрейм окна).

Наконец, существует свойство `SmoothAutoFocus`, которое замедляет алгоритм автоматического фокуса и помещает его с меньшим шагом, чтобы избежать перемещения артефактов при записи видео.

### <a name="new-focus-controls-in-ios-8"></a>Новые элементы управления фокусом в iOS 8

Помимо функций, уже предоставляемых iOS 7 и более поздних версий, теперь для управления фокусом в iOS 8 доступны следующие функции:

- Полное ручное управление положением линзы при блокировке фокуса.
- Наблюдение за ключами линзы в любом режиме фокусировки.

Чтобы реализовать описанные выше функции, `AVCaptureDevice` класс был изменен так, чтобы он включал в себя свойство `LensPosition` только для чтения, используемое для получения текущей позицией линзы.

Чтобы управлять позицией линза вручную, устройство записи должно находиться в режиме фокуса блокировки. Пример

 `CaptureDevice.FocusMode = AVCaptureFocusMode.Locked;`

Метод `SetFocusModeLocked` устройства записи используется для регулировки расположения линзы камеры. Необязательная подпрограммы обратного вызова можно использовать для получения уведомлений о том, что изменение вступает в силу. Пример

```csharp
ThisApp.CaptureDevice.LockForConfiguration(out Error);
ThisApp.CaptureDevice.SetFocusModeLocked(Position.Value,null);
ThisApp.CaptureDevice.UnlockForConfiguration();
```

Как видно из приведенного выше кода, устройство записи должно быть заблокировано для настройки, прежде чем можно будет изменить расположение лупы. Допустимые значения позиций линзы находятся в диапазоне от 0,0 до 1,0.

### <a name="manual-focus-example"></a>Пример ручного фокуса

Используя общий код настройки записи вирусов, `UIViewController` можно добавить в раскадровку приложения и настроить следующим образом:

[![](intro-to-manual-camera-controls-images/image5.png "A UIViewController can be added to the applications Storyboard and configured as shown here")](intro-to-manual-camera-controls-images/image5.png#lightbox)

Представление содержит следующие основные элементы:

- `UIImageView`, который будет отображать видеоканал.
- `UISegmentedControl`, который изменит режим фокусировки с автоматически на заблокировано.
- `UISlider`, которая будет показывать и обновлять текущую позицией линзы.

Выполните следующие действия, чтобы подключить контроллер представления для управления фокусом вручную:

1. Добавьте следующие операторы using:

    ```csharp
    using System;
    using Foundation;
    using UIKit;
    using System.CodeDom.Compiler;
    using System.Collections.Generic;
    using System.Linq;
    using AVFoundation;
    using CoreVideo;
    using CoreMedia;
    using CoreGraphics;
    using CoreFoundation;
    using System.Timers;
    ```

1. Добавьте следующие частные переменные:

    ```csharp
    #region Private Variables
    private NSError Error;
    private bool Automatic = true;
    #endregion
    ```

1. Добавьте следующие вычисленные свойства:

    ```csharp
    #region Computed Properties
    public AppDelegate ThisApp {
        get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
    }
    public Timer SampleTimer { get; set; }
    #endregion
    ```

1. Переопределите метод `ViewDidLoad` и добавьте следующий код:

    ```csharp
    public override void ViewDidLoad ()
    {
        base.ViewDidLoad ();

        // Hide no camera label
        NoCamera.Hidden = ThisApp.CameraAvailable;

        // Attach to camera view
        ThisApp.Recorder.DisplayView = CameraView;

        // Create a timer to monitor and update the UI
        SampleTimer = new Timer (5000);
        SampleTimer.Elapsed += (sender, e) => {
            // Update position slider
            Position.BeginInvokeOnMainThread(() =>{
                Position.Value = ThisApp.Input.Device.LensPosition;
            });
        };

        // Watch for value changes
        Segments.ValueChanged += (object sender, EventArgs e) => {

            // Lock device for change
            ThisApp.CaptureDevice.LockForConfiguration(out Error);

            // Take action based on the segment selected
            switch(Segments.SelectedSegment) {
            case 0:
                // Activate auto focus and start monitoring position
                Position.Enabled = false;
                ThisApp.CaptureDevice.FocusMode = AVCaptureFocusMode.ContinuousAutoFocus;
                SampleTimer.Start();
                Automatic = true;
                break;
            case 1:
                // Stop auto focus and allow the user to control the camera
                SampleTimer.Stop();
                ThisApp.CaptureDevice.FocusMode = AVCaptureFocusMode.Locked;
                Automatic = false;
                Position.Enabled = true;
                break;
            }

            // Unlock device
            ThisApp.CaptureDevice.UnlockForConfiguration();
        };

        // Monitor position changes
        Position.ValueChanged += (object sender, EventArgs e) => {

            // If we are in the automatic mode, ignore changes
            if (Automatic) return;

            // Update Focus position
            ThisApp.CaptureDevice.LockForConfiguration(out Error);
            ThisApp.CaptureDevice.SetFocusModeLocked(Position.Value,null);
            ThisApp.CaptureDevice.UnlockForConfiguration();
        };
    }
    ```

1. Переопределите метод `ViewDidAppear` и добавьте следующий параметр, чтобы начать запись при загрузке представления:

    ```csharp
    public override void ViewDidAppear (bool animated)
    {
        base.ViewDidAppear (animated);

        // Start udating the display
        if (ThisApp.CameraAvailable) {
            // Remap to this camera view
            ThisApp.Recorder.DisplayView = CameraView;

            ThisApp.Session.StartRunning ();
            SampleTimer.Start ();
        }
    }
    ```

1. Если камера находится в режиме Auto, ползунок будет перемещаться автоматически по мере регулировки фокуса камерой:

    [![](intro-to-manual-camera-controls-images/image6.png "The slider will move automatically as the camera adjusts focus in this sample app")](intro-to-manual-camera-controls-images/image6.png#lightbox)
1. Коснитесь заблокированного сегмента и перетащите ползунок положение, чтобы изменить положение линзы вручную:

    [![](intro-to-manual-camera-controls-images/image7.png "Manually adjusting the lens position")](intro-to-manual-camera-controls-images/image7.png#lightbox)
1. Завершите работу приложения.

Приведенный выше код показывает, как отслеживать положение линзы, когда камера находится в автоматическом режиме, или использовать ползунок для управления положением линзы в режиме блокировки.

## <a name="manual-exposure"></a>Раскрытие вручную

Экспозиция относится к яркости изображения относительно яркости источника и определяется тем, насколько интенсивность попадания в датчик, как долго и на уровне получения датчика (сопоставление ISO). Предоставляя возможность ручного управления раскрытием, приложение может предоставить больше возможностей для конечного пользователя и позволить им получить стилизованный взгляд.

Используя ручные элементы управления экспозицией, пользователь может получить изображение с нереалистичного яркого на темное и Муди:

[![](intro-to-manual-camera-controls-images/image8.png "A sample an image showing exposure from unrealistically bright to dark and moody")](intro-to-manual-camera-controls-images/image8.png#lightbox)

Опять же, это можно сделать автоматически с помощью программного управления для научных приложений или с помощью ручных элементов управления, предоставляемых пользовательским интерфейсом приложений. В любом случае новые интерфейсы API экспозиции iOS 8 обеспечивают детальный контроль над параметрами экспозиции камеры.

### <a name="how-exposure-works"></a>Как работает экспозиция

Прежде чем обсуждать сведения об управлении экспозицией в приложении IOS 8. Давайте кратко рассмотрим, как работает экспозиция.

[![](intro-to-manual-camera-controls-images/image9.png "How exposure works")](intro-to-manual-camera-controls-images/image9.png#lightbox)

Ниже перечислены три основных элемента, которые связаны с выдержки.

- **Скорость затвора** — это продолжительность времени, в течение которого назатвор открыт, чтобы дать лампочку на датчик камеры. Чем короче время открытия затвора, тем менее светлое изображение имеет в себе и четкость изображения (меньше размытого движения). Чем дольше открыта выдержка, тем более светлой становится то же самое, что и более размытое движение.
- **Сопоставление ISO** — это термин, взятый из фотопленки и означающий чувствительность химических веществ на пленке к светлой. Низкое значение ISO на пленке имеет меньшую детализацию и более точное воспроизведение цвета; низкие значения ISO на цифровых датчиках имеют меньше шума от датчика, но меньше яркости. Чем выше значение ISO, тем ярче изображение, но с большим шум датчика. "ISO" на цифровом датчике — это мера [электронного дохода](https://en.wikipedia.org/wiki/Gain), а не физического компонента.
- **Апертура линзы** — это размер проёма линзы. На всех устройствах iOS Исправлена Апертура линза, поэтому единственными значениями, которые можно использовать для корректировки экспозиции, являются скорость выдержки и ISO.

### <a name="how-continuous-auto-exposure-works"></a>Принцип непрерывной автоматической экспозиции

Прежде чем изучать, как работает раскрытие вручную, рекомендуется понимать, как работает постоянная автоматическая экспозиция на устройстве iOS.

[![](intro-to-manual-camera-controls-images/image10.png "How continuous auto exposure works in an iOS device")](intro-to-manual-camera-controls-images/image10.png#lightbox)

Во-первых, это блок автоматической экспозиции, он имеет задание по вычислению идеальной экспозиции и непрерывно отслеживает статистику контроля использования. Эта информация используется для вычисления оптимального сочетания ISO и скорости затвора, чтобы получить индикатор сцены. Этот цикл называется циклом СЗ.

### <a name="how-locked-exposure-works"></a>Как работает заблокированная экспозиция

Теперь давайте рассмотрим, как работает заблокированная экспозиция на устройствах iOS.

[![](intro-to-manual-camera-controls-images/image11.png "How locked exposure works in iOS devices")](intro-to-manual-camera-controls-images/image11.png#lightbox)

Опять же, у вас есть блок Auto экспозиции, который пытается вычислить оптимальные значения iOS и Duration. Однако в этом режиме блок AE отключается от обработчика статистики отслеживания использования.

### <a name="existing-exposure-controls"></a>Существующие элементы управления экспозицией

iOS 7 и более поздних версий, предоставьте следующие существующие элементы управления экспозицией с помощью свойства `ExposureMode`:

- `AVCaptureExposureModeLocked` — выбирает сцену один раз и использует эти значения во всей сцене.
- `AVCaptureExposureModeContinuousAutoExposure` — пример сцены непрерывно, чтобы убедиться в том, что он хорошо горит.

`ExposurePointOfInterest` можно использовать для предоставления сцены, выбрав целевой объект для предоставления, и приложение может отслеживать свойство `AdjustingExposure`, чтобы определить, когда раскрытие настраивается.

### <a name="new-exposure-controls-in-ios-8"></a>Новые элементы управления экспозицией в iOS 8

В дополнение к функциям, уже предоставляемым iOS 7 и более поздних версий, теперь доступны следующие функции для управления уязвимостью в iOS 8:

- Полная пользовательская Выдержка вручную.
- Get, Set и значение ключа — Следите за скоростью IOS и выдержкой (длительность).

Для реализации описанных выше функций был добавлен новый режим `AVCaptureExposureModeCustom`. Если камера находится в пользовательском режиме, можно использовать следующий код для настройки длительности экспозиции и ISO:

```csharp
CaptureDevice.LockForConfiguration(out Error);
CaptureDevice.LockExposure(DurationValue,ISOValue,null);
CaptureDevice.UnlockForConfiguration();
```

В режимах Auto и locked приложение может настроить сдвиг автоматической подпрограммы выдержки, используя следующий код:

```csharp
CaptureDevice.LockForConfiguration(out Error);
CaptureDevice.SetExposureTargetBias(Value,null);
CaptureDevice.UnlockForConfiguration();
```

Минимальный и максимальный диапазоны зависят от устройства, на котором работает приложение, поэтому они никогда не должны быть жестко закодированы. Вместо этого используйте следующие свойства для получения минимального и максимального диапазонов значений:

- `CaptureDevice.MinExposureTargetBias`
- `CaptureDevice.MaxExposureTargetBias`
- `CaptureDevice.ActiveFormat.MinISO`
- `CaptureDevice.ActiveFormat.MaxISO`
- `CaptureDevice.ActiveFormat.MinExposureDuration`
- `CaptureDevice.ActiveFormat.MaxExposureDuration`

Как видно из приведенного выше кода, устройство записи должно быть заблокировано для настройки, прежде чем можно будет внести изменение в уязвимость.

### <a name="manual-exposure-example"></a>Пример раскрытия вручную

Используя общий код настройки записи вирусов, `UIViewController` можно добавить в раскадровку приложения и настроить следующим образом:

[![](intro-to-manual-camera-controls-images/image12.png "A UIViewController can be added to the applications Storyboard and configured as shown here")](intro-to-manual-camera-controls-images/image12.png#lightbox)

Представление содержит следующие основные элементы:

- `UIImageView`, который будет отображать видеоканал.
- `UISegmentedControl`, который изменит режим фокусировки с автоматически на заблокировано.
- Четыре элемента управления `UISlider`, которые будут показывать и обновлять смещение, длительность, ISO и смещение.

Выполните следующие действия, чтобы подключить контроллер представления для ручного управления экспозицией:

1. Добавьте следующие операторы using:

    ```csharp
    using System;
    using Foundation;
    using UIKit;
    using System.CodeDom.Compiler;
    using System.Collections.Generic;
    using System.Linq;
    using AVFoundation;
    using CoreVideo;
    using CoreMedia;
    using CoreGraphics;
    using CoreFoundation;
    using System.Timers;
    ```

1. Добавьте следующие частные переменные:

    ```csharp
    #region Private Variables
    private NSError Error;
    private bool Automatic = true;
    private nfloat ExposureDurationPower = 5;
    private nfloat ExposureMinimumDuration = 1.0f/1000.0f;
    #endregion
    ```

1. Добавьте следующие вычисленные свойства:

    ```csharp
    #region Computed Properties
    public AppDelegate ThisApp {
        get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
    }
    public Timer SampleTimer { get; set; }
    #endregion
    ```

1. Переопределите метод `ViewDidLoad` и добавьте следующий код:

    ```csharp
    public override void ViewDidLoad ()
    {
        base.ViewDidLoad ();

        // Hide no camera label
        NoCamera.Hidden = ThisApp.CameraAvailable;

        // Attach to camera view
        ThisApp.Recorder.DisplayView = CameraView;

        // Set min and max values
        Offset.MinValue = ThisApp.CaptureDevice.MinExposureTargetBias;
        Offset.MaxValue = ThisApp.CaptureDevice.MaxExposureTargetBias;

        Duration.MinValue = 0.0f;
        Duration.MaxValue = 1.0f;

        ISO.MinValue = ThisApp.CaptureDevice.ActiveFormat.MinISO;
        ISO.MaxValue = ThisApp.CaptureDevice.ActiveFormat.MaxISO;

        Bias.MinValue = ThisApp.CaptureDevice.MinExposureTargetBias;
        Bias.MaxValue = ThisApp.CaptureDevice.MaxExposureTargetBias;

        // Create a timer to monitor and update the UI
        SampleTimer = new Timer (5000);
        SampleTimer.Elapsed += (sender, e) => {
            // Update position slider
            Offset.BeginInvokeOnMainThread(() =>{
                Offset.Value = ThisApp.Input.Device.ExposureTargetOffset;
            });

            Duration.BeginInvokeOnMainThread(() =>{
                var newDurationSeconds = CMTimeGetSeconds(ThisApp.Input.Device.ExposureDuration);
                var minDurationSeconds = Math.Max(CMTimeGetSeconds(ThisApp.CaptureDevice.ActiveFormat.MinExposureDuration), ExposureMinimumDuration);
                var maxDurationSeconds = CMTimeGetSeconds(ThisApp.CaptureDevice.ActiveFormat.MaxExposureDuration);
                var p = (newDurationSeconds - minDurationSeconds) / (maxDurationSeconds - minDurationSeconds);
                Duration.Value = (float)Math.Pow(p, 1.0f/ExposureDurationPower);
            });

            ISO.BeginInvokeOnMainThread(() => {
                ISO.Value = ThisApp.Input.Device.ISO;
            });

            Bias.BeginInvokeOnMainThread(() => {
                Bias.Value = ThisApp.Input.Device.ExposureTargetBias;
            });
        };

        // Watch for value changes
        Segments.ValueChanged += (object sender, EventArgs e) => {

            // Lock device for change
            ThisApp.CaptureDevice.LockForConfiguration(out Error);

            // Take action based on the segment selected
            switch(Segments.SelectedSegment) {
            case 0:
                // Activate auto exposure and start monitoring position
                Duration.Enabled = false;
                ISO.Enabled = false;
                ThisApp.CaptureDevice.ExposureMode = AVCaptureExposureMode.ContinuousAutoExposure;
                SampleTimer.Start();
                Automatic = true;
                break;
            case 1:
                // Lock exposure and allow the user to control the camera
                SampleTimer.Stop();
                ThisApp.CaptureDevice.ExposureMode = AVCaptureExposureMode.Locked;
                Automatic = false;
                Duration.Enabled = false;
                ISO.Enabled = false;
                break;
            case 2:
                // Custom exposure and allow the user to control the camera
                SampleTimer.Stop();
                ThisApp.CaptureDevice.ExposureMode = AVCaptureExposureMode.Custom;
                Automatic = false;
                Duration.Enabled = true;
                ISO.Enabled = true;
                break;
            }

            // Unlock device
            ThisApp.CaptureDevice.UnlockForConfiguration();
        };

        // Monitor position changes
        Duration.ValueChanged += (object sender, EventArgs e) => {

            // If we are in the automatic mode, ignore changes
            if (Automatic) return;

            // Calculate value
            var p = Math.Pow(Duration.Value,ExposureDurationPower);
            var minDurationSeconds = Math.Max(CMTimeGetSeconds(ThisApp.CaptureDevice.ActiveFormat.MinExposureDuration),ExposureMinimumDuration);
            var maxDurationSeconds = CMTimeGetSeconds(ThisApp.CaptureDevice.ActiveFormat.MaxExposureDuration);
            var newDurationSeconds = p * (maxDurationSeconds - minDurationSeconds) +minDurationSeconds;

            // Update Focus position
            ThisApp.CaptureDevice.LockForConfiguration(out Error);
            ThisApp.CaptureDevice.LockExposure(CMTime.FromSeconds(p,1000*1000*1000),ThisApp.CaptureDevice.ISO,null);
            ThisApp.CaptureDevice.UnlockForConfiguration();
        };

        ISO.ValueChanged += (object sender, EventArgs e) => {

            // If we are in the automatic mode, ignore changes
            if (Automatic) return;

            // Update Focus position
            ThisApp.CaptureDevice.LockForConfiguration(out Error);
            ThisApp.CaptureDevice.LockExposure(ThisApp.CaptureDevice.ExposureDuration,ISO.Value,null);
            ThisApp.CaptureDevice.UnlockForConfiguration();
        };

        Bias.ValueChanged += (object sender, EventArgs e) => {

            // If we are in the automatic mode, ignore changes
            // if (Automatic) return;

            // Update Focus position
            ThisApp.CaptureDevice.LockForConfiguration(out Error);
            ThisApp.CaptureDevice.SetExposureTargetBias(Bias.Value,null);
            ThisApp.CaptureDevice.UnlockForConfiguration();
        };
    }
    ```

1. Переопределите метод `ViewDidAppear` и добавьте следующий параметр, чтобы начать запись при загрузке представления:

    ```csharp
    public override void ViewDidAppear (bool animated)
    {
        base.ViewDidAppear (animated);

        // Start udating the display
        if (ThisApp.CameraAvailable) {
            // Remap to this camera view
            ThisApp.Recorder.DisplayView = CameraView;

            ThisApp.Session.StartRunning ();
            SampleTimer.Start ();
        }
    }
    ```

1. Если камера находится в режиме Auto, ползунки будут перемещаться автоматически по мере регулировки экспозиции.

    [![](intro-to-manual-camera-controls-images/image13.png "The sliders will move automatically as the camera adjusts exposure")](intro-to-manual-camera-controls-images/image13.png#lightbox)
1. Коснитесь заблокированного сегмента и перетащите ползунок сдвига, чтобы настроить сдвиг автоматической экспозиции вручную:

    [![](intro-to-manual-camera-controls-images/image14.png "Adjusting the bias of the automatic exposure manually")](intro-to-manual-camera-controls-images/image14.png#lightbox)
1. Коснитесь пользовательского сегмента и перетащите ползунки длительность и ISO, чтобы вручную контролировать раскрытие:

    [![](intro-to-manual-camera-controls-images/image15.png "Drag the Duration and ISO sliders to manually control exposure")](intro-to-manual-camera-controls-images/image15.png#lightbox)
1. Завершите работу приложения.

В приведенном выше коде показано, как отслеживать параметры экспозиции, когда камера находится в автоматическом режиме, и как использовать ползунки для управления экспозицией в заблокированных или пользовательских режимах.

## <a name="manual-white-balance"></a>Ручной баланс белого

Элементы управления "белый баланс" позволяют пользователям регулировать баланс колоср в изображении, чтобы сделать их более реалистичными. Различные источники освещения имеют разные цветовые температуры, и параметры камеры, используемые для записи образа, должны быть скорректированы для компенсации этих различий. Опять же, разрешая пользователю управлять балансом белого, они могут вносить профессиональные изменения, которые не способны реализовать автоматические подпрограммы для достижения художественных эффектов.

[![](intro-to-manual-camera-controls-images/image16.png "A sample image showing Manual White Balance adjustments")](intro-to-manual-camera-controls-images/image16.png#lightbox)

Например, летнее время имеет синюю пересылку, тогда как индикаторы тунгстен инкандесцент имеют тепло-оранжевый тон. (В заблуждении, "крутое" цвета имеют более высокие цветовые температуры, чем "горячий" цвет. Цветовая температура является физической мерой, а не искусственного.)

Человеческим расделом является компенсация за различия в цветовой температуре, но это не так. Камера работает путем повышения цвета в противоположном спектре для настройки цветовых различий.

Новый API экспозиции iOS 8 позволяет приложению получить контроль над процессом и обеспечить точный контроль над настройками баланса белого по камере.

### <a name="how-white-balance-works"></a>Как работает баланс белого цвета

Прежде чем обсуждать подробные сведения об управлении балансом белого приложения для IOS 8. Давайте кратко рассмотрим, как работает баланс белого:

В процессе восприятия цветов цветовое [пространство RGB Цие 1931 и цветовое пространство Цие 1931 XYZ](https://en.wikipedia.org/wiki/CIE_1931_color_space) представляют собой первые математически определенные цветовые пространства. Они были созданы Международной комиссией по освещения (ЦИЕ) в 1931.

[![](intro-to-manual-camera-controls-images/image17.png "The CIE 1931 RGB color space and CIE 1931 XYZ color space")](intro-to-manual-camera-controls-images/image17.png#lightbox)

На приведенной выше диаграмме показаны все цвета, видимые для человеческого глаз, от глубокого синего до светлого зеленого до светлого красного цвета. Любую точку на схеме можно отобразить с помощью значения X и Y, как показано на приведенном выше графе.

Как видно на диаграмме, есть значения X и Y, которые могут быть отображены на диаграмме за пределами диапазона человеческих концепций, и в результате эти цвета не могут быть воспроизведены камерой.

Уменьшенная кривая на приведенной выше диаграмме называется [Планккиан очага](https://en.wikipedia.org/wiki/Planckian_locus), которая выражает цветовую температуру (в градусах Кельвина) с большим числом на синей стороне (температура здания выше) и меньшими числами на красном (другом). Они полезны для типичных ситуаций освещения.

В условиях смешанного освещения корректировки белого баланса должны отклоняться от очага Планккиан для внесения необходимых изменений. В таких ситуациях коррекцию необходимо сдвинуть на зеленую или красную/пурпурную сторону шкалы ЦИЕ.

Устройства iOS компенсируют цветовые преобразования, повышая противоположную усиление цвета. Например, если сцена имеет слишком большой синий цвет, то для компенсации будет увеличено красное усиление. Эти значения повышения будут откалиброваны для конкретных устройств, чтобы они были зависимыми от устройства.

### <a name="existing-white-balance-controls"></a>Существующие элементы управления балансом белого

iOS 7 и более поздние версии предоставили следующие существующие элементы управления балансом белого через свойство `WhiteBalanceMode`:

- `AVCapture WhiteBalance ModeLocked` — выборка сцены и использование этих значений во всей сцене.
- `AVCapture WhiteBalance ModeContinuousAutoExposure` — пример сцены непрерывно, чтобы убедиться в том, что он хорошо сбалансирован.

И приложение может отслеживать свойство `AdjustingWhiteBalance`, чтобы определить, когда раскрытие будет скорректировано.

### <a name="new-white-balance-controls-in-ios-8"></a>Новые элементы управления балансом белого цвета в iOS 8

Помимо функций, уже предоставляемых iOS 7 и более поздних версий, теперь доступны следующие функции для управления балансом белого цвета в iOS 8:

- Полное управление увеличением объема RGB для устройства вручную.
- Get, Set и ключевое значение Обратите внимание на увеличение RGB устройства.
- Поддержка белого баланса с помощью серой карты.
- Подпрограммы преобразования в независимые от устройства цветовые пространства.

Для реализации описанных выше функций была добавлена структура `AVCaptureWhiteBalanceGain` со следующими элементами:

- `RedGain`
- `GreenGain`
- `BlueGain`

Максимальный коэффициент белого баланса сейчас равен четырем (4) и может быть готов к использованию в свойстве `MaxWhiteBalanceGain`. Так что допустимый диапазон — от одного (1) до `MaxWhiteBalanceGain` (4) в настоящее время.

Для отслеживания текущих значений можно использовать свойство `DeviceWhiteBalanceGains`. Используйте `SetWhiteBalanceModeLockedWithDeviceWhiteBalanceGains` для корректировки баланса, когда камера находится в режиме блокировки белого баланса.

#### <a name="conversion-routines"></a>Подпрограммы преобразования

В iOS 8 добавлены подпрограммы преобразования, помогающие выполнять преобразование в ненезависимые от устройства цветовые пространства. Для реализации подпрограмм преобразования структура `AVCaptureWhiteBalanceChromaticityValues` добавлена со следующими элементами:

- `X` — значение от 0 до 1.
- `Y` — значение от 0 до 1.

Также добавлена структура `AVCaptureWhiteBalanceTemperatureAndTintValues` со следующими элементами:

- `Temperature` — это значение с плавающей запятой в градусах Кельвина.
- `Tint` — это смещение от зеленого или пурпурного от 0 до 150 с положительными значениями по направлению зеленого и отрицательного в пурпурном.

Используйте `CaptureDevice.GetTemperatureAndTintValues`и `CaptureDevice.GetDeviceWhiteBalanceGains`методы для преобразования между цветовыми пространствами температуры и оттенка, цветностью и усилением RGB.

> [!NOTE]
> Подпрограммы преобразования более точны. чем ближе значение для преобразования, тем Планккиан очага.

#### <a name="gray-card-support"></a>Поддержка серых карт

Apple использует термин серый мир для обозначения встроенной в iOS 8 поддержки карт серого цвета. Он позволяет пользователю сосредоточиться на физической серой карте, охватывающей по крайней мере 50% центра фрейма, и использует его для коррекции белого баланса. Эта серая карта предназначена для того, чтобы получить белый цвет, который выглядит нейтральным.

Это может быть реализовано в приложении с запросом на размещение физической серой карты перед камерой, наблюдение за свойством `GrayWorldDeviceWhiteBalanceGains` и ожидание сопоставления значений.

Затем приложение будет блокировать прибыли белого баланса для метода `SetWhiteBalanceModeLockedWithDeviceWhiteBalanceGains`, используя значения свойства `GrayWorldDeviceWhiteBalanceGains`, чтобы применить изменения.

Устройство записи должно быть заблокировано для настройки, прежде чем можно будет изменить баланс белого.

### <a name="manual-white-balance-example"></a>Пример ручного баланса белого

Используя общий код настройки записи вирусов, `UIViewController` можно добавить в раскадровку приложения и настроить следующим образом:

[![](intro-to-manual-camera-controls-images/image18.png "A UIViewController can be added to the applications Storyboard and configured as shown here")](intro-to-manual-camera-controls-images/image18.png#lightbox)

Представление содержит следующие основные элементы:

- `UIImageView`, который будет отображать видеоканал.
- `UISegmentedControl`, который изменит режим фокусировки с автоматически на заблокировано.
- Два элемента управления `UISlider`, которые будут показывать и обновлять температуру и оттенок.
- `UIButton`, используемое для выборки серого (серого) пространства и установки баланса белого по этим значениям.

Выполните следующие действия, чтобы подключить контроллер представления для контроля "баланс белого баланса" вручную:

1. Добавьте следующие операторы using:

    ```csharp
    using System;
    using Foundation;
    using UIKit;
    using System.CodeDom.Compiler;
    using System.Collections.Generic;
    using System.Linq;
    using AVFoundation;
    using CoreVideo;
    using CoreMedia;
    using CoreGraphics;
    using CoreFoundation;
    using System.Timers;
    ```

1. Добавьте следующие частные переменные:

    ```csharp
    #region Private Variables
    private NSError Error;
    private bool Automatic = true;
    #endregion
    ```

1. Добавьте следующие вычисленные свойства:

    ```csharp
    #region Computed Properties
    public AppDelegate ThisApp {
        get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
    }
    public Timer SampleTimer { get; set; }
    #endregion
    ```

1. Добавьте следующий частный метод, чтобы задать новую температуру и оттенок белого баланса:

    ```csharp
    #region Private Methods
    void SetTemperatureAndTint() {
        // Grab current temp and tint
        var TempAndTint = new AVCaptureWhiteBalanceTemperatureAndTintValues (Temperature.Value, Tint.Value);

        // Convert Color space
        var gains = ThisApp.CaptureDevice.GetDeviceWhiteBalanceGains (TempAndTint);

        // Set the new values
        if (ThisApp.CaptureDevice.LockForConfiguration (out Error)) {
            gains = NomralizeGains (gains);
            ThisApp.CaptureDevice.SetWhiteBalanceModeLockedWithDeviceWhiteBalanceGains (gains, null);
            ThisApp.CaptureDevice.UnlockForConfiguration ();
        }
    }

    AVCaptureWhiteBalanceGains NomralizeGains (AVCaptureWhiteBalanceGains gains)
    {
        gains.RedGain = Math.Max (1, gains.RedGain);
        gains.BlueGain = Math.Max (1, gains.BlueGain);
        gains.GreenGain = Math.Max (1, gains.GreenGain);

        float maxGain = ThisApp.CaptureDevice.MaxWhiteBalanceGain;
        gains.RedGain = Math.Min (maxGain, gains.RedGain);
        gains.BlueGain = Math.Min (maxGain, gains.BlueGain);
        gains.GreenGain = Math.Min (maxGain, gains.GreenGain);

        return gains;
    }
    #endregion
    ```

1. Переопределите метод `ViewDidLoad` и добавьте следующий код:

    ```csharp
    public override void ViewDidLoad ()
    {
        base.ViewDidLoad ();

        // Hide no camera label
        NoCamera.Hidden = ThisApp.CameraAvailable;

        // Attach to camera view
        ThisApp.Recorder.DisplayView = CameraView;

        // Set min and max values
        Temperature.MinValue = 1000f;
        Temperature.MaxValue = 10000f;

        Tint.MinValue = -150f;
        Tint.MaxValue = 150f;

        // Create a timer to monitor and update the UI
        SampleTimer = new Timer (5000);
        SampleTimer.Elapsed += (sender, e) => {
            // Convert color space
            var TempAndTint = ThisApp.CaptureDevice.GetTemperatureAndTintValues (ThisApp.CaptureDevice.DeviceWhiteBalanceGains);

            // Update slider positions
            Temperature.BeginInvokeOnMainThread (() => {
                Temperature.Value = TempAndTint.Temperature;
            });

            Tint.BeginInvokeOnMainThread (() => {
                Tint.Value = TempAndTint.Tint;
            });
        };

        // Watch for value changes
        Segments.ValueChanged += (sender, e) => {
            // Lock device for change
            if (ThisApp.CaptureDevice.LockForConfiguration (out Error)) {

                // Take action based on the segment selected
                switch (Segments.SelectedSegment) {
                case 0:
                // Activate auto focus and start monitoring position
                    Temperature.Enabled = false;
                    Tint.Enabled = false;
                    ThisApp.CaptureDevice.WhiteBalanceMode = AVCaptureWhiteBalanceMode.ContinuousAutoWhiteBalance;
                    SampleTimer.Start ();
                    Automatic = true;
                    break;
                case 1:
                // Stop auto focus and allow the user to control the camera
                    SampleTimer.Stop ();
                    ThisApp.CaptureDevice.WhiteBalanceMode = AVCaptureWhiteBalanceMode.Locked;
                    Automatic = false;
                    Temperature.Enabled = true;
                    Tint.Enabled = true;
                    break;
                }

                // Unlock device
                ThisApp.CaptureDevice.UnlockForConfiguration ();
            }
        };

        // Monitor position changes
        Temperature.TouchUpInside += (sender, e) => {

            // If we are in the automatic mode, ignore changes
            if (Automatic)
                return;

            // Update white balance
            SetTemperatureAndTint ();
        };

        Tint.TouchUpInside += (sender, e) => {

            // If we are in the automatic mode, ignore changes
            if (Automatic)
                return;

            // Update white balance
            SetTemperatureAndTint ();
        };

        GrayCardButton.TouchUpInside += (sender, e) => {

            // If we are in the automatic mode, ignore changes
            if (Automatic)
                return;

            // Get gray card values
            var gains = ThisApp.CaptureDevice.GrayWorldDeviceWhiteBalanceGains;

            // Set the new values
            if (ThisApp.CaptureDevice.LockForConfiguration (out Error)) {
                ThisApp.CaptureDevice.SetWhiteBalanceModeLockedWithDeviceWhiteBalanceGains (gains, null);
                ThisApp.CaptureDevice.UnlockForConfiguration ();
            }
        };
    }
    ```

1. Переопределите метод `ViewDidAppear` и добавьте следующий параметр, чтобы начать запись при загрузке представления:

    ```csharp
    public override void ViewDidAppear (bool animated)
    {
        base.ViewDidAppear (animated);

        // Start udating the display
        if (ThisApp.CameraAvailable) {
            // Remap to this camera view
            ThisApp.Recorder.DisplayView = CameraView;

            ThisApp.Session.StartRunning ();
            SampleTimer.Start ();
        }
    }
    ```

1. Сохраните изменения в коде и запустите приложение.
1. При использовании камеры в режиме Auto ползунки автоматически перемещаются, когда камера корректирует баланс белого:

    [![](intro-to-manual-camera-controls-images/image19.png "The sliders will move automatically as the camera adjusts white balance")](intro-to-manual-camera-controls-images/image19.png#lightbox)
1. Коснитесь заблокированного сегмента и перетащите ползунки временные и оттенки, чтобы изменить баланс белого вручную:

    [![](intro-to-manual-camera-controls-images/image20.png "Drag the Temp and Tint sliders to adjust the white balance manually")](intro-to-manual-camera-controls-images/image20.png#lightbox)
1. Выбрав заблокированный сегмент, поместите физическую серую карту перед камерой и коснитесь кнопки с серой картой, чтобы настроить баланс белого цвета на серый мир:

    [![](intro-to-manual-camera-controls-images/image21.png "Tap the Gray Card button to adjust white balance to the Gray World")](intro-to-manual-camera-controls-images/image21.png#lightbox)
1. Завершите работу приложения.

Приведенный выше код показывает, как отслеживать настройки белого баланса, когда камера находится в автоматическом режиме, или использовать ползунки для управления белым балансом в режиме блокировки.

## <a name="bracketed-capture"></a>Захват в квадратных скобках

Захват в квадратных скобках основан на параметрах из элементов управления ручной камеры, представленных выше, и позволяет приложению захватывать мгновенные данные в течение определенного времени различными способами.

Как и было сказано, захват в квадратных скобках — это более неподвижные изображения с различными параметрами изображения.

[![](intro-to-manual-camera-controls-images/image22.png "How Bracketed Capture works")](intro-to-manual-camera-controls-images/image22.png#lightbox)

С помощью записи в квадратных скобках в iOS 8 приложение может предустановить ряд ручных элементов управления камеры, выполнить одну команду и использовать текущую сцену для получения набора изображений для каждой предустановки вручную.

### <a name="bracketed-capture-basics"></a>Основы записи в квадратные скобки

Опять же, захват в квадратных скобках — это более неподвижные изображения, взятые с различными параметрами изображения на изображение. Доступны следующие типы записи в квадратных скобках:

- **Автоматическая раскрытие скобок** , где все изображения имеют различные величины смещения.
- **Ручная закрывающая скобка** , где все образы имеют различные значения скорости затвора (продолжительность) и ISO-объем.
- **Простая многоуровневая скобка** — серия всех изображений, которые потребовались для быстрого успеха.

### <a name="new-bracketed-capture-controls-in-ios-8"></a>Новые элементы управления записью в квадратных скобках в iOS 8

Все команды записи в квадратных скобках реализуются в классе `AVCaptureStillImageOutput`. Используйте метод `CaptureStillImageBracket`, чтобы получить ряд изображений с заданным массивом параметров.

Для работы с параметрами были реализованы два новых класса:

- `AVCaptureAutoExposureBracketedStillImageSettings` — у него есть одно свойство, `ExposureTargetBias`, используемое для задания смещения для автоматической скобки экспозиции.
- `AVCaptureManual` @ no__t_1_ — у него есть два свойства: `ExposureDuration` и `ISO`, которые используются для установки скорости затвора и ISO для ручной скобки.

### <a name="bracketed-capture-controls-dos-and-donts"></a>Элементы управления для захвата в квадратных скобках

#### <a name="dos"></a>Все

Ниже приведен список действий, которые следует выполнить при использовании элементов управления для записи в квадратных скобках в iOS 8.

- Подготовьте приложение к ситуации записи с наихудшим вариантом, вызвав метод `PrepareToCaptureStillImageBracket`.
- Предположим, что буферы образцов будут поступать из одного общего пула.
- Чтобы освободить память, выделенную предыдущим вызовом prepare, вызовите `PrepareToCaptureStillImageBracket` еще раз и отправьте ей массив из одного объекта.

#### <a name="donts"></a>Запрет

Ниже приведен список вещей, которые не должны выполняться при использовании элементов управления для записи в квадратных скобках в iOS 8.

- Не смешивать типы параметров захвата в квадратных скобках в одной записи.
- Не запрашивать более `MaxBracketedCaptureStillImageCount` образов в одной записи.

### <a name="bracketed-capture-details"></a>Сведения о захвате в квадратных скобках

При работе с захватом в квадратных скобках в iOS 8 следует учитывать следующие сведения.

- Параметры в квадратных скобках временно переопределяют параметры `AVCaptureDevice`.
- Параметры стабилизациии Flash и Image не обрабатываются.
- Все изображения должны использовать один и тот же формат вывода (JPEG, PNG и т. д.).
- Предварительный просмотр видео может удалять кадры.
- Запись в квадратных скобках поддерживается на всех устройствах, совместимых с iOS 8.

Учитывая эти сведения, давайте рассмотрим пример использования записи в квадратных скобках в iOS 8.

### <a name="bracket-capture-example"></a>Пример записи скобок

Используя общий код настройки записи вирусов, `UIViewController` можно добавить в раскадровку приложения и настроить следующим образом:

[![](intro-to-manual-camera-controls-images/image23.png "A UIViewController can be added to the applications Storyboard and configured as shown here")](intro-to-manual-camera-controls-images/image23.png#lightbox)

Представление содержит следующие основные элементы:

- `UIImageView`, который будет отображать видеоканал.
- Три `UIImageViews`, которые будут отображать результаты записи.
- `UIScrollView` для размещения видео-канала и представлений результатов.
- `UIButton`, используемая для выделения скобок с предустановленными параметрами.

Выполните следующие действия, чтобы подключить контроллер представления для записи в квадратных скобках:

1. Добавьте следующие операторы using:

    ```csharp
    using System;
    using System.Drawing;
    using Foundation;
    using UIKit;
    using System.CodeDom.Compiler;
    using System.Collections.Generic;
    using System.Linq;
    using AVFoundation;
    using CoreVideo;
    using CoreMedia;
    using CoreGraphics;
    using CoreFoundation;
    using CoreImage;
    ```

1. Добавьте следующие частные переменные:

    ```csharp
    #region Private Variables
    private NSError Error;
    private List<UIImageView> Output = new List<UIImageView>();
    private nint OutputIndex = 0;
    #endregion
    ```

1. Добавьте следующие вычисленные свойства:

    ```csharp
    #region Computed Properties
    public AppDelegate ThisApp {
        get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
    }
    #endregion
    ```

1. Добавьте следующий частный метод, чтобы создать требуемые представления выходных изображений:

    ```csharp
    #region Private Methods
    private UIImageView BuildOutputView(nint n) {

        // Create a new image view controller
        var imageView = new UIImageView (new CGRect (CameraView.Frame.Width * n, 0, CameraView.Frame.Width, CameraView.Frame.Height));

        // Load a temp image
        imageView.Image = UIImage.FromFile ("Default-568h@2x.png");

        // Add a label
        UILabel label = new UILabel (new CGRect (0, 20, CameraView.Frame.Width, 24));
        label.TextColor = UIColor.White;
        label.Text = string.Format ("Bracketed Image {0}", n);
        imageView.AddSubview (label);

        // Add to scrolling view
        ScrollView.AddSubview (imageView);

        // Return new image view
        return imageView;
    }
    #endregion
    ```

1. Переопределите метод `ViewDidLoad` и добавьте следующий код:

    ```csharp
    public override void ViewDidLoad ()
    {
        base.ViewDidLoad ();

        // Hide no camera label
        NoCamera.Hidden = ThisApp.CameraAvailable;

        // Attach to camera view
        ThisApp.Recorder.DisplayView = CameraView;

        // Setup scrolling area
        ScrollView.ContentSize = new SizeF (CameraView.Frame.Width * 4, CameraView.Frame.Height);

        // Add output views
        Output.Add (BuildOutputView (1));
        Output.Add (BuildOutputView (2));
        Output.Add (BuildOutputView (3));

        // Create preset settings
        var Settings = new AVCaptureBracketedStillImageSettings[] {
            AVCaptureAutoExposureBracketedStillImageSettings.Create(-2.0f),
            AVCaptureAutoExposureBracketedStillImageSettings.Create(0.0f),
            AVCaptureAutoExposureBracketedStillImageSettings.Create(2.0f)
        };

        // Wireup capture button
        CaptureButton.TouchUpInside += (sender, e) => {
            // Reset output index
            OutputIndex = 0;

            // Tell the camera that we are getting ready to do a bracketed capture
            ThisApp.StillImageOutput.PrepareToCaptureStillImageBracket(ThisApp.StillImageOutput.Connections[0],Settings,async (bool ready, NSError err) => {
                // Was there an error, if so report it
                if (err!=null) {
                    Console.WriteLine("Error: {0}",err.LocalizedDescription);
                }
            });

            // Ask the camera to snap a bracketed capture
            ThisApp.StillImageOutput.CaptureStillImageBracket(ThisApp.StillImageOutput.Connections[0],Settings, (sampleBuffer, settings, err) =>{
                // Convert raw image stream into a Core Image Image
                var imageData = AVCaptureStillImageOutput.JpegStillToNSData(sampleBuffer);
                var image = CIImage.FromData(imageData);

                // Display the resulting image
                Output[OutputIndex++].Image = UIImage.FromImage(image);

                // IMPORTANT: You must release the buffer because AVFoundation has a fixed number
                // of buffers and will stop delivering frames if it runs out.
                sampleBuffer.Dispose();
            });
        };
    }
    ```

1. Переопределите метод `ViewDidAppear` и добавьте следующий код:

    ```csharp
    public override void ViewDidAppear (bool animated)
    {
        base.ViewDidAppear (animated);

        // Start udating the display
        if (ThisApp.CameraAvailable) {
            // Remap to this camera view
            ThisApp.Recorder.DisplayView = CameraView;

            ThisApp.Session.StartRunning ();
        }
    }

    ```

1. Сохраните изменения в коде и запустите приложение.
1. Кадрировать сцену и нажать кнопку "записать скобку":

    [![](intro-to-manual-camera-controls-images/image24.png "Frame a scene and tap the Capture Bracket button")](intro-to-manual-camera-controls-images/image24.png#lightbox)
1. Проведите вправо влево, чтобы увидеть три изображения, взятые с помощью записи в квадратных скобках:

    [![](intro-to-manual-camera-controls-images/image25.png "Swipe right to left to see the three images taken by the Bracketed Capture")](intro-to-manual-camera-controls-images/image25.png#lightbox)
1. Завершите работу приложения.

В приведенном выше коде показано, как настроить и принять автоматическую запись в квадратных скобках в iOS 8.

## <a name="summary"></a>Сводка

В этой статье мы рассмотрели общие сведения о новых ручных элементах управления камерой, предоставляемых iOS 8, и как они работают. Мы предоставили примеры фокуса вручную, раскрытия вручную и ручного баланса белого цвета. Наконец, мы предоставили пример с записью в квадратные скобки, используя ранее обсуждаемые элементы управления камеры вручную.

## <a name="related-links"></a>Связанные ссылки

- [Мануалкамераконтролс (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/manualcameracontrols)
- [Введение в iOS 8](~/ios/platform/introduction-to-ios8.md)
