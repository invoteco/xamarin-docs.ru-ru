---
title: Просмотр переходов контроллера в Xamarin. iOS
description: В этом документе описывается настройка анимированных переходов между контроллерами представлений в приложениях Xamarin. iOS.
ms.prod: xamarin
ms.assetid: CB3AC8E2-8A47-4839-AFA5-AE33047BB26C
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/14/2017
ms.openlocfilehash: f0886ac9d47e7ab08a6f74365bcc25163b803e11
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73002397"
---
# <a name="view-controller-transitions-in-xamarinios"></a>Просмотр переходов контроллера в Xamarin. iOS

UIKit добавляет поддержку для настройки анимированного перехода, возникающего при представлении контроллеров представления. Эта поддержка входит в состав встроенных контроллеров, а также пользовательских контроллеров, которые наследуются непосредственно от `UIViewController`. Кроме того, `UICollectionViewController` использует преимущества настройки перехода контроллера, чтобы использовать анимированные переходы в макетах представлений коллекций.

## <a name="custom-transitions"></a>Пользовательские переходы

Анимированный переход между контроллерами представлений в iOS 7 является полностью настраиваемым. `UIViewController` теперь включает свойство `TransitioningDelegate`, которое предоставляет системе пользовательский класс аниматор, когда происходит переход.

Чтобы использовать пользовательский переход с `PresentViewController`:

1. Задайте для `ModalPresentationStyle` значение `UIModalPresentationStyle.Custom` на отображаемом контроллере.
2. Реализуйте `UIViewControllerTransitioningDelegate`, чтобы создать класс аниматор, который является экземпляром `UIViewControllerAnimatedTransitioning`.
3. Задайте для свойства `TransitioningDelegate` экземпляр `UIViewControllerTransitioningDelegate`, а также отображаемый контроллер.
4. Представьте контроллер представления.

Например, следующий код представляет контроллер представления типа `ControllerTwo` — подкласс `UIViewController`:

```csharp
showTwo.TouchUpInside += (object sender, EventArgs e) => {

    controllerTwo = new ControllerTwo ();

    this.PresentViewController (controllerTwo, true, null);
};
```

Запуск приложения и нажатие кнопки приводит к тому, что анимация по умолчанию в представлении второго контроллера будет анимироваться в нижней части, как показано ниже:

 ![](transitions-images/no-custom-transition.png "Running the app and tapping the button causes the default animation of the second controllers view to animate in from the bottom")

Однако установка `ModalPresentationStyle` и `TransitioningDelegate` приведет к настраиваемой анимации для перехода:

```csharp
showTwo.TouchUpInside += (object sender, EventArgs e) => {

    controllerTwo = new ControllerTwo () {
        ModalPresentationStyle = UIModalPresentationStyle.Custom
        };

    transitioningDelegate = new TransitioningDelegate ();
    controllerTwo.TransitioningDelegate = transitioningDelegate;

    this.PresentViewController (controllerTwo, true, null);
};
```

`TransitioningDelegate` отвечает за создание экземпляра класса `UIViewControllerAnimatedTransitioning`, который называется `CustomAnimator` в следующем примере:

```csharp
public class TransitioningDelegate : UIViewControllerTransitioningDelegate
{
    CustomTransitionAnimator animator;

    public override IUIViewControllerAnimatedTransitioning GetAnimationControllerForPresentedController (UIViewController presented, UIViewController presenting, UIViewController source)
    {
        animator = new CustomTransitionAnimator ();
        return animator;
    }
}
```

Когда происходит переход, система создает экземпляр `IUIViewControllerContextTransitioning`, который передается методам аниматор. `IUIViewControllerContextTransitioning` содержит `ContainerView`, где происходит анимация, а также контроллер представления, инициирующий переход, и контроллер представления, на который выполняется переход.

Класс `UIViewControllerAnimatedTransitioning` обрабатывает реальную анимацию. Необходимо реализовать два метода:

1. `TransitionDuration` — Возвращает длительность анимации в секундах.
1. `AnimateTransition` — выполняет фактическую анимацию.

Например, следующий класс реализует `UIViewControllerAnimatedTransitioning`, чтобы анимировать кадр представления контроллера:

```csharp
public class CustomTransitionAnimator : UIViewControllerAnimatedTransitioning
{
    public CustomTransitionAnimator ()
    {
    }

    public override double TransitionDuration (IUIViewControllerContextTransitioning transitionContext)
    {
        return 1.0;
    }

    public override void AnimateTransition (IUIViewControllerContextTransitioning transitionContext)
    {
        var inView = transitionContext.ContainerView;
        var toVC = transitionContext.GetViewControllerForKey (UITransitionContext.ToViewControllerKey);
        var toView = toVC.View;

        inView.AddSubview (toView);

        var frame = toView.Frame;
        toView.Frame = CGRect.Empty;

        UIView.Animate (TransitionDuration (transitionContext), () => {
            toView.Frame = new CGRect (20, 20, frame.Width - 40, frame.Height - 40);
        }, () => {
            transitionContext.CompleteTransition (true);
        });
    }
}
```

Теперь при нажатии кнопки используется анимация, реализованная в классе `UIViewControllerAnimatedTransitioning`:

 ![](transitions-images/custom-transition.png "An example of the zoom in effect running")

## <a name="collection-view-transitions"></a>Переходы представления коллекции

Представления коллекций имеют встроенную поддержку для создания анимированных переходов:

- **Контроллеры навигации** — анимированный переход между двумя экземплярами `UICollectionViewController` может при необходимости обрабатываться автоматически, когда `UINavigationController` управляет ими.
- **Макет перехода** — новый класс `UICollectionViewTransitionLayout` позволяет интерактивно переходить между макетами.

### <a name="navigation-controller-transitions"></a>Переходы контроллера навигации

При использовании в контроллере навигации `UICollectionViewController` включает поддержку анимированных переходов между контроллерами. Эта поддержка является встроенной и требует лишь нескольких простых действий для реализации:

1. Задайте для `UseLayoutToLayoutNavigationTransitions` значение `false` на `UICollectionViewController`.
1. Добавьте экземпляр `UICollectionViewController` в корень стека контроллера навигации.
1. Создайте второй `UICollectionViewController` и задайте для его свойства `UseLayoutToLayoutNavigtionTransitions` значение `true`.
1. Отправьте второй `UICollectionViewController` в стек контроллера навигации.

Следующий код добавляет подкласс `UICollectionViewController` с именем `ImagesCollectionViewController` в корень стека контроллера навигации со свойством `UseLayoutToLayoutNavigationTransitions`, равным `false`:

```csharp
UIWindow window;
ImagesCollectionViewController viewController;
UICollectionViewFlowLayout layout;
UINavigationController navController;

public override bool FinishedLaunching (UIApplication app, NSDictionary options)
{
    window = new UIWindow (UIScreen.MainScreen.Bounds);

    // create and initialize a UICollectionViewFlowLayout
    layout = new UICollectionViewFlowLayout (){
        SectionInset = new UIEdgeInsets (10,5,10,5),
        MinimumInteritemSpacing = 5,
        MinimumLineSpacing = 5,
        ItemSize = new CGSize (100, 100)
    };

    viewController = new ImagesCollectionViewController (layout) {
            UseLayoutToLayoutNavigationTransitions = false
        };

    navController = new UINavigationController (viewController);

    window.RootViewController = navController;
    window.MakeKeyAndVisible ();

    return true;
}
```

При выборе элемента создается второй экземпляр `ImagesController`, только на этот раз используя другой класс макета. Для этого контроллера `UseLayoutToLayoutNavigtionTransitions` имеет значение `true`, как показано ниже:

```csharp
CircleLayout circleLayout;
ImagesCollectionViewController controller2;

...

public override void ItemSelected (UICollectionView collectionView, NSIndexPath indexPath)
{
    // UseLayoutToLayoutNavigationTransitions when item is selected
    circleLayout = new CircleLayout (Monkeys.Instance.Count){
        ItemSize = new CGSize (100, 100)
    };

    controller2 = new ImagesCollectionViewController (circleLayout) {
        UseLayoutToLayoutNavigationTransitions = true
        };

    NavigationController.PushViewController (controller2, true);
}
```

Перед добавлением контроллера в стек навигации необходимо задать свойство `UseLayoutToLayoutNavigationTransitions`. Если это свойство установлено, то стандартный горизонтальный переход заменяется анимированным переходом между макетами двух контроллеров, как показано ниже:

![](transitions-images/nav2.png "An animated transition between the layouts of the two controllers")

### <a name="transition-layout"></a>Макет перехода

Помимо поддержки переходов макета в контроллерах навигации, теперь доступен новый макет с именем `UICollectionViewTransitionLayout`. Этот класс макета разрешает интерактивное управление во время перехода макета, позволяя задавать `TransitionProgress` из кода. `UICollectionViewTransitionLayout` отличается от-и не заменяет собой метод `SetCollectionViewLayout` из iOS 6, который привел к появлению анимированного перехода в макет. Этот метод не предоставляет встроенную поддержку для управления ходом выполнения анимированного перехода.

 `UICollectionViewTransitionLayout` позволяет, например, настроить распознаватель жестов для управления переходом между макетами в ответ на взаимодействие с пользователем, управляя исходным макетом, а также предполагаемой структурой для перехода.

Ниже приведены действия по реализации интерактивного перехода в распознавателе жестов с помощью `UICollectionViewTransitionLayout`.

1. Создайте распознаватель жестов.
1. Вызовите метод `StartInteractiveTransition` `UICollectionView`, передав ему целевой макет и обработчик завершения.
1. Задайте свойство `TransitionProgress` экземпляра `UICollectionViewTransitionLayout`, возвращаемого методом `StartInteractiveTransition`.
1. Сделать макет недействительным.
1. Вызовите метод `FinishInteractiveTransition` `UICollectionView`, чтобы завершить переход, или метод `CancelInteractiveTransition`, чтобы отменить его.  `FinishInteractiveTransition` заставляет анимацию завершить переход к целевому макету, в то время как `CancelInteractiveTransition` результаты анимации, возвращающей исходный макет.
1. Обработайте завершение перехода в обработчике завершения метода `StartInteractiveTransition`.
1. Добавьте распознаватель жестов в представление коллекции.

В следующем коде реализуется интерактивный переход макета в рамках распознавателя жестов сжатия:

```csharp
imagesController = new ImagesCollectionViewController (flowLayout);

nfloat sf = 0.4f;
UICollectionViewTransitionLayout trLayout = null;
UICollectionViewLayout nextLayout;

pinch = new UIPinchGestureRecognizer (g => {

    var progress = Math.Abs(1.0f -  g.Scale)/sf;

    if(trLayout == null){
        if(imagesController.CollectionView.CollectionViewLayout is CircleLayout)
            nextLayout = flowLayout;
        else
            nextLayout = circleLayout;

        trLayout = imagesController.CollectionView.StartInteractiveTransition (nextLayout, (completed, finished) => {
            Console.WriteLine ("transition completed");
            trLayout = null;
        });
    }

    trLayout.TransitionProgress = (nfloat)progress;

    imagesController.CollectionView.CollectionViewLayout.InvalidateLayout ();

    if(g.State == UIGestureRecognizerState.Ended){
        if (trLayout.TransitionProgress > 0.5f)
            imagesController.CollectionView.FinishInteractiveTransition ();
        else
            imagesController.CollectionView.CancelInteractiveTransition ();
    }

});

imagesController.CollectionView.AddGestureRecognizer (pinch);

```

Когда пользователь сжимает представление коллекции, `TransitionProgress` задается относительно шкалы сжатия. В этой реализации, если пользователь завершает сжатие до завершения перехода 50%, переход отменяется. В противном случае переход завершается.

## <a name="related-links"></a>Связанные ссылки

- [Введение в iOS 7 (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/introtoios7)
- [Обзор пользовательского интерфейса iOS 7](~/ios/platform/introduction-to-ios7/ios7-ui.md)
- [Фоновая обработка](~/ios/app-fundamentals/backgrounding/index.md)
