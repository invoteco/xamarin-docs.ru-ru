---
title: DualScreenInfo в Xamarin.Forms
description: В этом руководстве рассматривается использование элемента DualScreenInfo из Xamarin.Forms для оптимизации интерфейса приложения на двухэкранных устройствах, таких как Surface Duo и Surface Neo.
ms.prod: xamarin
ms.assetid: dd5eb074-f4cb-4ab4-b47d-76f862ac7cfa
ms.technology: xamarin-forms
author: davidortinau
ms.author: daortin
ms.date: 02/08/2020
ms.openlocfilehash: cae704b7d6300a9dc5eb456f0dec8989813c6581
ms.sourcegitcommit: 07941cf9704ff88cf4087de5ebdea623ff54edb1
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/11/2020
ms.locfileid: "77145618"
---
# <a name="xamarinforms-dualscreeninfo"></a>DualScreenInfo в Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://github.com/xamarin/xamarin-forms-samples/tree/pre-release/UserInterface/DualScreenDemos)

_DualScreenInfo позволяет обнаруживать макеты, определять их связь с двухэкранным представлением и сообщать об изменениях._

## <a name="properties"></a>Свойства

- `SpanningBounds` — при использовании двух экранов это свойство возвращает два прямоугольника, указывающих границы каждой видимой области. Если окно не занимает два экрана, возвращается пустой массив.
- `HingeBounds` указывает расположение сгиба на экране.
- `IsLandscape` указывает, находится ли устройство в альбомной ориентации. Это полезно, поскольку собственные API-интерфейсы устройства неверно сообщают ориентацию, когда приложение расширено на два экрана.
- `PropertyChanged` активируется при изменении каких-либо свойств.
- `SpanMode` указывает режим макета: вертикальный, горизонтальный или однопанельный.

## <a name="android-only-property"></a>Свойство только для Android

Данное свойство доступно только при обращении к DualScreenInfo из проекта на платформе Android.
Вы можете использовать это свойство из настраиваемого отрисовщика.

- `GetHingeAngleAsync` получает текущий угол сгиба устройства. При использовании симулятора свойство HingeAngle можно задать путем изменения значения датчика давления.

## <a name="android-custom-renderer-for-polling-hinge-angle"></a>Настраиваемый отрисовщик Android для опроса угла сгиба

```csharp
public class HingeAngleLabelRenderer : Xamarin.Forms.Platform.Android.FastRenderers.LabelRenderer
{
    System.Timers.Timer _hingeTimer;
    public HingeAngleLabelRenderer(Context context) : base(context)
    {
    }

    async void OnTimerElapsed(object sender, System.Timers.ElapsedEventArgs e)
    {
        if (_hingeTimer == null)
            return;

        _hingeTimer.Stop();
        var hingeAngle = await DualScreenInfo.Current.GetHingeAngleAsync();

        Device.BeginInvokeOnMainThread(() =>
        {
            if (_hingeTimer != null)
                Element.Text = hingeAngle.ToString();
        });

        if (_hingeTimer != null)
            _hingeTimer.Start();
    }

    protected override void OnElementChanged(ElementChangedEventArgs<Label> e)
    {
        base.OnElementChanged(e);

        if (_hingeTimer == null)
        {
            _hingeTimer = new System.Timers.Timer(100);
            _hingeTimer.Elapsed += OnTimerElapsed;
            _hingeTimer.Start();
        }
    }

    protected override void Dispose(bool disposing)
    {
        if (_hingeTimer != null)
        {
            _hingeTimer.Elapsed -= OnTimerElapsed;
            _hingeTimer.Stop();
            _hingeTimer = null;
        }

        base.Dispose(disposing);
    }
}
```

## <a name="access-dualscreeninfo-for-application-window"></a>Доступ к DualScreenInfo для окна приложения

```csharp
DualScreenInfo currentWindow = DualScreenInfo.Current;

// Retrieve absolute position of the hinge on the screen
var hingeBounds = currentWindow.HingeBounds;

// check if app window is spanned across two screens
if(currentWindow.SpanMode == TwoPaneViewMode.SinglePane)
{
    // window is only on one screen
}
else if(currentWindow.SpanMode == TwoPaneViewMode.Tall)
{
    // window is spanned across two screens and oriented as landscape
}
else if(currentWindow.SpanMode == TwoPaneViewMode.Wide)
{
    // window is spanned across two screens and oriented as portrait
}

// Detect if any of the properties on DualScreenInfo change.
// This is useful to detect if the app window gets spanned
// across two screens or put on only one  
currentWindow.PropertyChanged += OnDualScreenInfoChanged;
```

## <a name="apply-dualscreeninfo-to-your-own-layouts"></a>Применение DualScreenInfo к вашим собственным макетам

DualScreenInfo содержит конструктор, который может принять макет и предоставить сведения о его использовании на двух экранах устройств.

```xaml
<Grid x:Name="grid" ColumnSpacing="0">
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="{Binding Column1Width}"></ColumnDefinition>
        <ColumnDefinition Width="{Binding Column2Width}"></ColumnDefinition>
        <ColumnDefinition Width="{Binding Column3Width}"></ColumnDefinition>
    </Grid.ColumnDefinitions>

    <Label FontSize="Large" VerticalOptions="Center" HorizontalOptions="End" Text="I should be on the left side of the hinge"></Label>
    <Label FontSize="Large" VerticalOptions="Center" HorizontalOptions="Start" Grid.Column="2" Text="I should be on the right side of the hinge"></Label>
</Grid>
```

```csharp
public partial class GridUsingDualScreenInfo : ContentPage
{
    public DualScreenInfo DualScreenInfo { get; }
    public double Column1Width { get; set; }
    public double Column2Width { get; set; }
    public double Column3Width { get; set; }

    public GridUsingDualScreenInfo()
    {
        InitializeComponent();
        DualScreenInfo = new DualScreenInfo(grid);
        BindingContext = this;
    }

    protected override void OnAppearing()
    {
        base.OnAppearing();
        DualScreenInfo.PropertyChanged += OnInfoPropertyChanged;
        UpdateColumns();
    }

    protected override void OnDisappearing()
    {
        base.OnDisappearing();
        DualScreenInfo.PropertyChanged -= OnInfoPropertyChanged;
    }

    void UpdateColumns()
    {
        // Check if grid is on two screens
        if (DualScreenInfo.SpanningBounds.Length > 0)
        {
            // set the width of the first column to the width of the layout
            // that's on the left screen
            Column1Width = DualScreenInfo.SpanningBounds[0].Width;

            // set the middle column to the width of the hinge
            Column2Width = DualScreenInfo.HingeBounds.Width;

            // set the width of the third column to the width of the layout
            // that's on the right screen
            Column3Width = DualScreenInfo.SpanningBounds[1].Width;
        }
        else
        {
            Column1Width = 100;
            Column2Width = 0;
            Column3Width = 100;
        }

        OnPropertyChanged(nameof(Column1Width));
        OnPropertyChanged(nameof(Column2Width));
        OnPropertyChanged(nameof(Column3Width));

    }

    void OnInfoPropertyChanged(object sender, System.ComponentModel.PropertyChangedEventArgs e)
    {
        UpdateColumns();
    }
}
```

![](dual-screen-info-images/grid-on-two-screens.png "Positioning Grid on Two Screens")

## <a name="related-links"></a>Связанные ссылки

- [DualScreen (пример)](https://github.com/xamarin/xamarin-forms-samples/tree/pre-release/UserInterface/DualScreenDemos)
