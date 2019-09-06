---
title: Использование основной графической и основной анимации в Xamarin. iOS
description: В этой статье описывается пошаговое руководство по созданию приложения, использующего основные графические и основные анимации. В нем показано, как рисовать на экране в ответ на касание пользователя, а также как анимировать изображение, чтобы перемещаться по пути.
ms.prod: xamarin
ms.assetid: 4B96D5CD-1BF5-4520-AAA6-2B857C83815C
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/18/2017
ms.openlocfilehash: cae0196fc29944ba8ae87fe43d256da090942f3a
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/04/2019
ms.locfileid: "70286121"
---
# <a name="using-core-graphics-and-core-animation-in-xamarinios"></a>Использование основной графической и основной анимации в Xamarin. iOS

В этом пошаговом руководстве мы будем нарисовать контур, используя основные графические объекты, в ответ на сенсорный ввод. Затем мы добавим объект `CALayer` , содержащий изображение, которое будет анимировано вдоль пути.

На следующем снимке экрана показано готовое приложение:

![](graphics-animation-walkthrough-images/00-final-app.png "Готовое приложение")

Перед началом загрузки образца *графиксдемо* , прилагаемого к этому руководству. Его можно скачать [здесь](https://docs.microsoft.com/samples/xamarin/ios-samples/graphicsandanimation) и найти в каталоге **графиксвалксраугх** . Запустите проект с именем **GraphicsDemo_starter** , дважды щелкнув `DemoView` его, и откройте класс.

## <a name="drawing-a-path"></a>Рисование контура


1. `DemoView` В`CGPath` добавьте переменную в класс и создайте ее экземпляр в конструкторе. Также объявите `CGPoint` две `initialPoint` переменные и `latestPoint`, которые будут использоваться для захвата сенсорной точки, из которой строится путь:

    ```csharp
    public class DemoView : UIView
    {
        CGPath path;
        CGPoint initialPoint;
        CGPoint latestPoint;

        public DemoView ()
        {
            BackgroundColor = UIColor.White;

            path = new CGPath ();
        }
    }
    ```

2. Добавьте следующие директивы using:

    ```csharp
    using CoreGraphics;
    using CoreAnimation;
    using Foundation;
    ```

3. Затем переопределите `TouchesBegan` и `TouchesMoved,` добавьте следующие реализации для захвата начальной сенсорной точки и каждой последующей сенсорной точки соответственно:

    ```csharp
    public override void TouchesBegan (NSSet touches, UIEvent evt){

        base.TouchesBegan (touches, evt);

        UITouch touch = touches.AnyObject as UITouch;

        if (touch != null) {
            initialPoint = touch.LocationInView (this);
        }
    }

    public override void TouchesMoved (NSSet touches, UIEvent evt){

        base.TouchesMoved (touches, evt);

        UITouch touch = touches.AnyObject as UITouch;

        if (touch != null) {
            latestPoint = touch.LocationInView (this);
            SetNeedsDisplay ();
        }
    }
    ```

    `SetNeedsDisplay`будет вызываться каждый раз при перемещении `Draw` , чтобы их можно было вызывать при следующем передаче цикла выполнения.

4. Мы будем добавлять строки к пути в `Draw` методе и использовать красную пунктирную линию для рисования. [Реализуйте `Draw` ](~/ios/platform/graphics-animation-ios/core-graphics.md) с кодом, показанным ниже:

    ```csharp
    public override void Draw (CGRect rect){

        base.Draw (rect);

        if (!initialPoint.IsEmpty) {

            //get graphics context
            using(CGContext g = UIGraphics.GetCurrentContext ()){

                //set up drawing attributes
                g.SetLineWidth (2);
                UIColor.Red.SetStroke ();

                //add lines to the touch points
                if (path.IsEmpty) {
                    path.AddLines (new CGPoint[]{initialPoint, latestPoint});
                } else {
                    path.AddLineToPoint (latestPoint);
                }

                //use a dashed line
                g.SetLineDash (0, new nfloat[] { 5, 2 * (nfloat)Math.PI });

                //add geometry to graphics context and draw it
                g.AddPath (path);
                g.DrawPath (CGPathDrawingMode.Stroke);
            }
        }
    }
    ```

Если запустить приложение сейчас, можно нарисовать экран на экране, как показано на следующем снимке экрана:

![](graphics-animation-walkthrough-images/01-path.png "Рисование на экране")

## <a name="animating-along-a-path"></a>Анимация вдоль пути

Теперь, когда мы реализовали код, чтобы пользователи могли нарисовать контур, добавим код для анимации слоя вдоль рисуемого контура.

1. Сначала добавьте [`CALayer`](~/ios/platform/graphics-animation-ios/core-animation.md) переменную в класс и создайте ее в конструкторе:

    ```csharp
    public class DemoView : UIView
        {
            …

            CALayer layer;

            public DemoView (){
                …

                //create layer
                layer = new CALayer ();
                layer.Bounds = new CGRect (0, 0, 50, 50);
                layer.Position = new CGPoint (50, 50);
                layer.Contents = UIImage.FromFile ("monkey.png").CGImage;
                layer.ContentsGravity = CALayer.GravityResizeAspect;
                layer.BorderWidth = 1.5f;
                layer.CornerRadius = 5;
                layer.BorderColor = UIColor.Blue.CGColor;
                layer.BackgroundColor = UIColor.Purple.CGColor;
            }
    ```

2. Далее мы добавим слой в качестве подслоя слоя представления, когда пользователь отрывает свой палец с экрана. Затем мы создадим анимацию опорного кадра с помощью пути, и анимировать слой `Position`.

    Для этого необходимо переопределить `TouchesEnded` и добавить следующий код:

    ```csharp
    public override void TouchesEnded (NSSet touches, UIEvent evt)
        {
            base.TouchesEnded (touches, evt);

            //add layer with image and animate along path

            if (layer.SuperLayer == null)
                Layer.AddSublayer (layer);

            // create a keyframe animation for the position using the path
            layer.Position = latestPoint;
            CAKeyFrameAnimation animPosition = (CAKeyFrameAnimation)CAKeyFrameAnimation.FromKeyPath ("position");
            animPosition.Path = path;
            animPosition.Duration = 3;
            layer.AddAnimation (animPosition, "position");
        }
    ```

3. Запустить приложение сейчас и после рисования. слой с изображением добавляется и перемещается по изображенному пути:

![](graphics-animation-walkthrough-images/00-final-app.png "Слой с изображением добавляется и перемещается по изображенному контуру")

## <a name="summary"></a>Сводка

В этой статье мы пошаговым примером того, как вместе связаны концепции графики и анимации. Во- `UIView` первых, мы показали, как использовать основные графические изображения для рисования пути в в ответ на касание пользователя. Затем мы показали, как использовать основную анимацию для перемещения изображения по этому пути.


## <a name="related-links"></a>Связанные ссылки

- [Core Animation](~/ios/platform/graphics-animation-ios/core-animation.md)
- [Core Graphics](~/ios/platform/graphics-animation-ios/core-graphics.md)
- [Основные рецепты анимации](https://github.com/xamarin/recipes/tree/master/Recipes/ios/animation/coreanimation)
