---
title: Xamarin. Android Текстуревиев
ms.prod: xamarin
ms.assetid: DD1F3D68-5DD8-4644-8A13-08AE7719DE30
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 05/30/2017
ms.openlocfilehash: 2ffa544789e0d605a241c8e038c790650a7fc6a3
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2020
ms.locfileid: "76724980"
---
# <a name="xamarinandroid-textureview"></a>Xamarin. Android Текстуревиев

Класс `TextureView` — это представление, которое использует двухмерную отрисовку с аппаратным ускорением для отображения видео или потока содержимого OpenGL. Например, на следующем снимке экрана показан `TextureView`, отображающий динамический канал с камеры устройства:

[![Пример снимка экрана в реальном времени с камеры устройства](texture-view-images/22-textureviewcamera.png)](texture-view-images/22-textureviewcamera.png#lightbox)

В отличие от класса `SurfaceView`, который также можно использовать для отображения содержимого OpenGL или видео, Текстуревиев не подготавливается к просмотру в отдельном окне.
Таким образом, `TextureView` может поддерживать преобразования представлений, как и любое другое представление. Например, поворот `TextureView` можно выполнить, просто задав свойство `Rotation`, его прозрачность, установив свойство `Alpha` и т. д.

Таким образом, с `TextureView` мы можем выполнять такие действия, как отображение живого потока из камеры и преобразование его, как показано в следующем коде:

```csharp
public class TextureViewActivity : Activity,
    TextureView.ISurfaceTextureListener
{
    Camera _camera;
    TextureView _textureView;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);
        _textureView = new TextureView (this);
        _textureView.SurfaceTextureListener = this;

        SetContentView (_textureView);
    }

    public void OnSurfaceTextureAvailable (
        Android.Graphics.SurfaceTexture surface,
        int width, int height)
    {
        _camera = Camera.Open ();
        var previewSize = _camera.GetParameters ().PreviewSize;
        _textureView.LayoutParameters =
            new FrameLayout.LayoutParams (previewSize.Width,
                previewSize.Height, (int)GravityFlags.Center);

        try {
            _camera.SetPreviewTexture (surface);
            _camera.StartPreview ();
        } catch (Java.IO.IOException ex) {
            Console.WriteLine (ex.Message);
        }

        // this is the sort of thing TextureView enables
        _textureView.Rotation = 45.0f;
        _textureView.Alpha = 0.5f;
    }
    …
}
```

Приведенный выше код создает экземпляр `TextureView` в методе `OnCreate` действия и задает действие в качестве `SurfaceTextureListener``TextureView`. Чтобы быть `SurfaceTextureListener`, действие реализует интерфейс `TextureView.ISurfaceTextureListener`. Система будет вызывать метод `OnSurfaceTextAvailable`, когда `SurfaceTexture` готов к использованию. В этом методе передается `SurfaceTexture`, переданный в, и задается текстура предварительной версии камеры. Затем мы можем выполнять обычные операции на основе представлений, например задавать `Rotation` и `Alpha`, как показано в примере выше. Полученное приложение, выполняемое на устройстве, показано ниже:

[![пример приложения, выполняемого на устройстве, в котором отображается изображение](texture-view-images/17-textureviewdemo.png)](texture-view-images/17-textureviewdemo.png#lightbox)

Чтобы использовать `TextureView`, необходимо включить аппаратное ускорение, которое по умолчанию будет иметь уровень API 14. Кроме того, поскольку в этом примере используется камера, как разрешение `android.permission.CAMERA`, так и функция `android.hardware.camera` должны быть установлены в **AndroidManifest. XML**.

## <a name="related-links"></a>Связанные ссылки

- [Текстуревиевдемо (пример)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/textureviewdemo)/)
