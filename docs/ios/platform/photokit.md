---
title: Фотокит в Xamarin. iOS
description: В этом документе описывается Фотокит, обсуждаются объекты модели, как запрашиваются данные модели и сохраняются изменения в библиотеке фотографий.
ms.prod: xamarin
ms.assetid: 7FDEE394-3787-40FA-8372-76A05BF184B3
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/14/2017
ms.openlocfilehash: 5e5cc20e9fbeaf2b00e022ccdbf67286aed6d5ef
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2019
ms.locfileid: "69528811"
---
# <a name="photokit-in-xamarinios"></a>Фотокит в Xamarin. iOS

Фотокит — это новая платформа, позволяющая приложениям запрашивать библиотеку образов системы и создавать настраиваемые пользовательские интерфейсы для просмотра и изменения содержимого. Он включает несколько классов, представляющих изображения и видеоматериалы, а также коллекции ресурсов, таких как альбомы и папки.

## <a name="model-objects"></a>Объекты модели

Фотокит представляет эти активы в том, что вызывает объекты модели. Объекты модели, представляющие сами фотографии и видео, имеют тип `PHAsset`. `PHAsset` Содержит метаданные, например тип носителя ресурса и дату его создания.
Аналогичным образом классы `PHCollectionList` исодержатметаданныеоколлекцияхактивовиспискахколлекцийсоответственно.`PHAssetCollection` Коллекции активов — это группы ресурсов, такие как все фотографии и видео в течение определенного года. Аналогичным образом списки коллекций — это группы коллекций активов, такие как фотографии и видео, сгруппированные по годам.

## <a name="querying-model-data"></a>Запрос данных модели

Фотокит упрощает запрос данных модели с помощью различных методов выборки. Например, чтобы получить все изображения, необходимо вызвать `PFAsset.Fetch`, `PHAssetMediaType.Image` передав тип мультимедиа.

```csharp
PHFetchResult fetchResults = PHAsset.FetchAssets (PHAssetMediaType.Image, null);
```

Экземпляр будет содержать все экземпляры, `PFAsset` представляющие изображения. `PHFetchResult` Чтобы получить сами образы, используйте `PHImageManager` (или `PHCachingImageManager`версию кэширования) для создания запроса образа путем вызова `RequestImageForAsset`. Например, следующий код извлекает изображение для каждого ресурса в `PHFetchResult` для отображения в ячейке представления коллекции:

```csharp
public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
{
    var imageCell = (ImageCell)collectionView.DequeueReusableCell (cellId, indexPath);
    imageMgr.RequestImageForAsset (
        (PHAsset)fetchResults [(uint)indexPath.Item],
        thumbnailSize,
        PHImageContentMode.AspectFill, new PHImageRequestOptions (),
        (img, info) => {
            imageCell.ImageView.Image = img;
        }
    );
    return imageCell;
}
```

Это приводит к отображению сетки изображений, как показано ниже:

![](photokit-images/image4.png "Выполняемое приложение, отображающее сетку изображений")

## <a name="saving-changes-to-the-photo-library"></a>Сохранение изменений в библиотеке фотографий

Это способ управления запросами и считыванием данных. Можно также записать изменения обратно в библиотеку. Так как несколько заинтересованных приложений могут взаимодействовать с фотобиблиотекой системы, можно зарегистрировать наблюдатель, чтобы получать уведомления об изменениях с помощью Фотолибрарйобсервер. Затем, когда поступают изменения, приложение может обновиться соответствующим образом. Например, вот простая реализация для перезагрузки представления коллекций выше:

```csharp
class PhotoLibraryObserver : PHPhotoLibraryChangeObserver
{
    readonly PhotosViewController controller;
    public PhotoLibraryObserver (PhotosViewController controller)

    {
        this.controller = controller;
    }

    public override void PhotoLibraryDidChange (PHChange changeInstance)
    {
        DispatchQueue.MainQueue.DispatchAsync (() => {
        var changes = changeInstance.GetFetchResultChangeDetails (controller.fetchResults);
        controller.fetchResults = changes.FetchResultAfterChanges;
        controller.CollectionView.ReloadData ();
        });
    }
}
```

Для фактической записи изменений из приложения необходимо создать запрос на изменение. Каждый из классов модели имеет связанный класс запроса на изменение. Например, чтобы изменить Фассет, создайте Фассетчанжерекуест. Действия по выполнению изменений, которые записываются обратно в библиотеку фотографий и отправляются наблюдателям, как показано выше:

- Выполните операцию редактирования.
- Сохраните отфильтрованные данные изображения в экземпляре Фконтентедитингаутпут.
- Выполните запрос на изменение, чтобы опубликовать изменения, образуя выходные данные редактирования.

Ниже приведен пример, который записывает изменения в образ, который применяет основной фильтр ноир Image:

```csharp
void ApplyNoirFilter (object sender, EventArgs e)
{

    Asset.RequestContentEditingInput (new PHContentEditingInputRequestOptions (), (input, options) => {

        // perform the editing operation, which applies a noir filter in this case
        var image = CIImage.FromUrl (input.FullSizeImageUrl);
        image = image.CreateWithOrientation((CIImageOrientation)input.FullSizeImageOrientation);
        var noir = new CIPhotoEffectNoir {
            Image = image
        };
        var ciContext = CIContext.FromOptions (null);
        var output = noir.OutputImage;
        var uiImage = UIImage.FromImage (ciContext.CreateCGImage (output, output.Extent));
        imageView.Image = uiImage;
        //
        // save the filtered image data to a PHContentEditingOutput instance
        var editingOutput = new PHContentEditingOutput(input);
        var adjustmentData = new PHAdjustmentData();
        var data = uiImage.AsJPEG();
        NSError error;
        data.Save(editingOutput.RenderedContentUrl, false, out error);
        editingOutput.AdjustmentData = adjustmentData;
        //
        // make a change request to publish the changes form the editing output
        PHPhotoLibrary.GetSharedPhotoLibrary.PerformChanges (() => {
            PHAssetChangeRequest request = PHAssetChangeRequest.ChangeRequest(Asset);
            request.ContentEditingOutput = editingOutput;
        },
        (ok, err) => Console.WriteLine ("photo updated successfully: {0}", ok));
    });
}
```

Когда пользователь нажимает кнопку, применяется фильтр:

![](photokit-images/image5.png "Пример примененного фильтра")

И спасибо за Ффотолибраричанжеобсервер, это изменение отражается в представлении коллекции, когда пользователь переходит назад:

![](photokit-images/image6.png "Изменение отражается в представлении коллекции, когда пользователь переходит назад")
