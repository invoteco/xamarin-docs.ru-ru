---
title: Xamarin. Android Текстуревиев
ms.prod: xamarin
ms.assetid: DD1F3D68-5DD8-4644-8A13-08AE7719DE30
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 05/30/2017
ms.openlocfilehash: 5d6b1b01cf9597a1d7ae9de762eff1514b494663
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029122"
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

- [Текстуревиевдемо (пример)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/textureviewdemo)
- [Знакомство со Южным Сандвичевым](https://www.android.com/about/ice-cream-sandwich/)
- [Платформа Android 4,0](https://developer.android.com/sdk/android-4.0.html)
