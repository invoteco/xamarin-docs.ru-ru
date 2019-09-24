---
title: Несколько окон для iPad в Xamarin. iOS
description: Добавление поддержки нескольких окон в приложения iPad.
ms.prod: xamarin
ms.assetid: 524b6f2e-dbdf-11e9-8a34-2a2ae2dbcce4
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/20/2019
ms.openlocfilehash: b3f96f342679d8be6d2f8fbc0ad5962ca675d2a5
ms.sourcegitcommit: 09bc69d7119a04684c9e804c5cb113b8b1bb7dfc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/24/2019
ms.locfileid: "71213807"
---
# <a name="multiple-windows-for-ipad"></a>Несколько окон для iPad

iOS 13 теперь поддерживает параллельные окна для одного приложения на iPad. Это позволяет создавать новые возможности и перетаскивать взаимодействия между окнами. В этом документе показано, как настроить приложение для поддержки этой функции, а также появились новые функции. 

## <a name="project-configuration"></a>Конфигурация проекта

Чтобы настроить проект для нескольких `info.plist` окон, измените для объявления `NSUserActivityTypes` , указав для приложения iOS, что приложение будет выполнять действия этого типа и `UIApplicationSceneManifest` включить `UIApplicationSupportsMultipleScenes` для нескольких окон и `UISceneConfigurations` связать Сцена с раскадровкой.

```xml
<key>NSUserActivityTypes</key>
<array>
    <string>com.xamarin.Gallery.openDetail</string>
</array>
<key>UIApplicationSceneManifest</key>
<dict>
    <key>UIApplicationSupportsMultipleScenes</key>
    <true/>
    <key>UISceneConfigurations</key>
    <dict>
        <key>UIWindowSceneSessionRoleApplication</key>
        <array>
            <dict>
                <key>UISceneConfigurationName</key>
                <string>Default Configuration</string>
                <key>UISceneDelegateClassName</key>
                <string>SceneDelegate</string>
                <key>UISceneStoryboardFile</key>
                <string>Main</string>
            </dict>
        </array>
    </dict>
</dict>
```

## <a name="opening-multiple-windows"></a>Открытие нескольких окон

Когда приложение открыто и выполняется на iPad, существует несколько способов открыть несколько окон этого приложения, по умолчанию обрабатывающих iOS.

- **Раскрыть приложение** . Коснитесь значка приложения на закрепления, чтобы перейти в этот режим, пока приложение открыто.
- Перетащите значок приложения с закрепления **на начало** работающего приложения, чтобы получить плавающее окно.
- **Разделить экран** . Перетащите значок приложения с стыковочного узла на край экрана iPad, чтобы создать новое параллельное окно.

Дополнительные взаимодействия для входа в режим с несколькими окнами доступны в приложении.

**Перетащите приложение из приложения** . Используйте взаимодействие перетаскивания в приложении, чтобы начать новый `NSUserActivity` , так же, как при перетаскивании значка приложения в предыдущих примерах.

При использовании [взаимодействия с перетаскиванием][0]вы создаете `NSUserActivity` и связываете данные, которые необходимо передать, в новое окно, которое вы запрашиваете для открытия iOS.

```csharp
public UIDragItem [] GetItemsForBeginningDragSession (UICollectionView collectionView, IUIDragSession session, NSIndexPath indexPath)
{
    var selectedPhoto = GetPhoto (indexPath);

    var userActivity = selectedPhoto.OpenDetailUserActivity ();
    var itemProvider = new NSItemProvider (UIImage.FromFile (selectedPhoto.Name));
    itemProvider.RegisterObject (userActivity, NSItemProviderRepresentationVisibility.All);

    var dragItem = new UIDragItem (itemProvider) {
        LocalObject = selectedPhoto
    };

    return new [] { dragItem };
}
```

В приведенном выше `selectedPhoto` коде объект модели имеет метод, `NSUserActivity` возвращающий вызванный `OpenDetailUserActivity()`. По завершении жеста перетаскивания, `UIDragItem` объект с `userActivity` представляется с `NSItemProvider`помощью.

**Явные действия** . пользовательские жесты на кнопках или ссылках предоставляют возможность открывать новое окно.

В можно начать новый `UISceneSession` с помощью вызова `RequestSceneSessionActivation`. `UIApplication` Если существующая сцена уже существует, следует использовать ее. По умолчанию будет создана новая сцена.

```csharp
pubic void ItemSelected(UICollectionView collectionView, NSIndexPath indexPath)
{
    var userActivity = selectedPhoto.OpenDetailUserActivity ();

    UIApplication.SharedApplication.RequestSceneSessionActivation(
        sceneSession: null,
        userActivity: userActivity,
        options: null,
        errorHandler: null
    );
}
```

В `userActivity` этом примере компонент является единственным значением, передаваемым `RequestSceneSessionActivation` в метод, чтобы открыть новое окно приложения на основе явного действия пользователя `ItemSelected` . в данном `UICollectionView`случае это обработчик объекта.

## <a name="related-links"></a>Связанные ссылки

- [Перетаскивание в Xamarin. iOS][0]

[0]: ~/ios/platform/introduction-to-ios11/drag-and-drop.md
