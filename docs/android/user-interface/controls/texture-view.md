---
title: Xamarin. Android Текстуревиев
ms.prod: xamarin
ms.assetid: DD1F3D68-5DD8-4644-8A13-08AE7719DE30
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/30/2017
ms.openlocfilehash: 799e117e3a4cb6e3071680da90e6432f389c6682
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/30/2019
ms.locfileid: "68642560"
---
# <a name="xamarinandroid-textureview"></a>Xamarin. Android Текстуревиев

`TextureView` Класс представляет собой представление, которое использует двухмерную отрисовку с аппаратным ускорением для отображения потока содержимого видео или OpenGL. Например, на следующем снимке экрана показано `TextureView` , как отобразить динамический канал с камеры устройства:

[![Пример снимка экрана динамического изображения с камеры устройства](texture-view-images/22-textureviewcamera.png)](texture-view-images/22-textureviewcamera.png#lightbox)

В отличие `SurfaceView` от класса, который также можно использовать для отображения содержимого OpenGL или видео, текстуревиев не подготавливается к просмотру в отдельном окне.
Таким образом `TextureView` , может поддерживать преобразования представлений, как и любое другое представление. Например, поворот `TextureView` можно выполнить, просто задав его `Rotation` свойство, его прозрачность, установив его `Alpha` свойство и т. д.

Таким образом, `TextureView` теперь мы можем выполнять такие действия, как отображение активного потока из камеры и преобразование его, как показано в следующем коде:

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

Приведенный выше код создает `TextureView` экземпляр в `OnCreate` методе действия и `TextureView`задает действие в качестве `SurfaceTextureListener`. Для этого действие `TextureView.ISurfaceTextureListener` реализует интерфейс. `SurfaceTextureListener` Система будет вызывать `OnSurfaceTextAvailable` метод, когда объект `SurfaceTexture` готов к использованию. В этом методе передается `SurfaceTexture` переданный объект и задается текстура предварительной версии камеры. Затем мы можем выполнять обычные операции на основе представлений, такие как установка `Rotation` и `Alpha`, как в примере выше. Полученное приложение, выполняемое на устройстве, показано ниже:

[![Пример приложения, выполняемого на устройстве с отображением изображения](texture-view-images/17-textureviewdemo.png)](texture-view-images/17-textureviewdemo.png#lightbox)

Чтобы использовать `TextureView`, необходимо включить аппаратное ускорение, которое по умолчанию будет иметь уровень API 14. Кроме того, поскольку в этом примере используется камера, как `android.permission.CAMERA` разрешение, так `android.hardware.camera` и функция должны быть установлены в **AndroidManifest. XML**.



## <a name="related-links"></a>Связанные ссылки

- [Текстуревиевдемо (пример)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/textureviewdemo)
- [Знакомство со Южным Сандвичевым](http://www.android.com/about/ice-cream-sandwich/)
- [Платформа Android 4,0](https://developer.android.com/sdk/android-4.0.html)
