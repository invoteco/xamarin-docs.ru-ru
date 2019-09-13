---
title: Карауселвиев Xamarin. Forms
description: По умолчанию Карауселвиев будет отображать свои элементы в горизонтальном списке. Однако он также имеет доступ к тем же макетам, что и CollectionView, включая вертикальную ориентацию.
ms.prod: xamarin
ms.assetid: fede0382-c972-4023-a4ea-fe5cadec91a6
ms.technology: xamarin-forms
author: pauldipietro
ms.author: padipi
ms.date: 09/09/2019
ms.openlocfilehash: 5bbaeead524089ea604cfd9a9fd7f3a85d04e724
ms.sourcegitcommit: e83035c746f165ee6d03f2e9fd0066ee4f20a9fb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/11/2019
ms.locfileid: "70908346"
---
# <a name="xamarinforms-carouselview-layouts"></a>Макеты Карауселвиев Xamarin. Forms

![](~/media/shared/preview.png "Этот API в настоящее время предоставляется в режиме предварительной версии")

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/CarouselViewDemos/)

## <a name="introduction"></a>Вступление

Большая часть функций макета, доступных для Карауселвиев, исходит от CollectionView. Вы можете обратиться к [документации по макету](../collectionview/layout.md) CollectionView, чтобы увидеть использование различных макетов.

## <a name="differences-from-collectionview"></a>Отличия от CollectionView

По умолчанию элементы в Карауселвиев будут ориентированы по горизонтали, как и Типичная функция обоймы в приложениях.

Карауселвиев также предоставляет некоторые дополнительные свойства:

> [!IMPORTANT]
> Дополнительные свойства для Карауселвиев по-прежнему размещаются в разработке, и этот список еще не завершен.

| API | Функция |
|---|---|---|
| нумберофсидеитемс | Задает количество элементов, которые отображаются на каждой стороне текущего элемента. Значение по умолчанию — 0.
| пикареаинсетс | Позволяет легко показать пользователю, что Карауселвиев содержит дополнительные элементы для прокрутки, настраивая уровень видимости, смежный с текущим элементом.

## <a name="setting-the-number-of-fully-visible-items"></a>Задание количества полностью отображаемых элементов

По умолчанию Карауселвиев отображает один элемент целиком на экране. Пользователи могут задать `NumberOfSideItems` для свойства значение Разрешить отображение дополнительных элементов рядом с текущим элементом. Обратите внимание, что любое `PeekAreaInsets` значение, установленное в, будет по-прежнему применяться.

```xaml
<StackLayout Margin="20">
    <CarouselView ItemsSource="{Binding Monkeys}" HeightRequest="125" NumberOfSideItems="1">
        <CarouselView.ItemTemplate>
            <DataTemplate>
                <Frame BorderColor="Black">
                    <Grid>
                        <Grid.RowDefinitions>
                            <RowDefinition Height="Auto" />
                            <RowDefinition Height="Auto" />
                        </Grid.RowDefinitions>
                        <Grid.ColumnDefinitions>
                            <ColumnDefinition Width="Auto" />
                            <ColumnDefinition Width="Auto" />
                        </Grid.ColumnDefinitions>
                        <Label Grid.Column="1"
                            Text="{Binding Name}"
                            FontAttributes="Bold"
                            FontSize="14"/>
                        <Label Grid.Row="1"
                            Grid.Column="1"
                            Text="{Binding Location}"
                            FontAttributes="Italic"
                            FontSize="12"
                            VerticalOptions="End" />
                    </Grid>
                </Frame>
            </DataTemplate>
        </CarouselView.ItemTemplate>
    </CarouselView>
</StackLayout>
```

[ ![Снимок экрана элемента карауселвиев с элементами на стороне Android](carouselview-images/side-items.png "карауселвиев") ] (carouselview-images/side-items-large.png#lightbox "Элементы карауселвиев")

## <a name="making-adjacent-items-partially-visible"></a>Создание частично видимых смежных элементов

При использовании карауселвиев в приложении может быть полезно указать пользователю, что функция карауселвиев подобным образом, задав `PeekAreaInsets` для свойства ненулевое значение (по умолчанию), которое частично предоставляет их на экране.

```xaml
<StackLayout Margin="20">
  <CarouselView ItemsSource="{Binding Monkeys}" HeightRequest="125" PeekAreaInsets="100">
      <CarouselView.ItemTemplate>
          <DataTemplate>
              <Frame BorderColor="Black">
                  <Grid>
                      <Grid.RowDefinitions>
                          <RowDefinition Height="Auto" />
                          <RowDefinition Height="Auto" />
                      </Grid.RowDefinitions>
                      <Grid.ColumnDefinitions>
                          <ColumnDefinition Width="Auto" />
                          <ColumnDefinition Width="Auto" />
                      </Grid.ColumnDefinitions>
                      <Label Grid.Column="1"
                          Text="{Binding Name}"
                          FontAttributes="Bold"
                          FontSize="14"/>
                      <Label Grid.Row="1"
                          Grid.Column="1"
                          Text="{Binding Location}"
                          FontAttributes="Italic"
                          FontSize="12"
                          VerticalOptions="End" />
                  </Grid>
              </Frame>
          </DataTemplate>
      </CarouselView.ItemTemplate>
  </CarouselView>
</StackLayout>
```

[ ![Снимок экрана элемента карауселвиев с элементами на стороне Android](carouselview-images/peek-area-insets.png "карауселвиев") ] (carouselview-images/peek-area-insets-large.png#lightbox "Элементы карауселвиев")

## <a name="related-links"></a>Связанные ссылки

- [CollectionView (пример)](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/CarouselViewDemos/)
- [Документация по макету CollectionView](../collectionview/layout.md)
