---
title: Xamarin の PhotoKit
description: このドキュメントでは、PhotoKit について説明し、そのモデルオブジェクトについて説明し、モデルデータのクエリを実行し、変更をフォトライブラリに保存する方法について説明します。
ms.prod: xamarin
ms.assetid: 7FDEE394-3787-40FA-8372-76A05BF184B3
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/14/2017
ms.openlocfilehash: def34efd1fd48cc0e7dd802a6d3e843be1e156a4
ms.sourcegitcommit: 5ddb107b0a56bef8a16fce5bc6846f9673b3b22e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/31/2019
ms.locfileid: "75558808"
---
# <a name="photokit-in-xamarinios"></a>Xamarin の PhotoKit

[![サンプル ダウンロード](~/media/shared/download.png) コードサンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-samplephotoapp/)

PhotoKit は、アプリケーションがシステムイメージライブラリに対してクエリを実行し、その内容を表示および変更するためのカスタムユーザーインターフェイスを作成できるようにするフレームワークです。 これには、イメージとビデオ資産を表す多数のクラスと、アルバムやフォルダーなどの資産のコレクションが含まれます。

## <a name="permissions"></a>[権限]

アプリがフォトライブラリにアクセスできるようにするには、ユーザーにアクセス許可のダイアログが表示されます。 アプリで写真ライブラリを使用する方法を説明するために、次のような**情報を plist**ファイルに記述する必要があります。

```xml
<key>NSPhotoLibraryUsageDescription</key>
<string>Applies filters to photos and updates the original image</string>
```

## <a name="model-objects"></a>モデルオブジェクト

PhotoKit は、モデルオブジェクトを呼び出す対象の資産を表します。 写真とビデオ自体を表すモデルオブジェクトの種類は `PHAsset`です。 `PHAsset` には、資産のメディアの種類や作成日などのメタデータが含まれています。
同様に、`PHAssetCollection` クラスと `PHCollectionList` クラスには、資産コレクションとコレクションリストに関するメタデータがそれぞれ含まれています。 資産コレクションは、特定の年のすべての写真やビデオなどの資産のグループです。 同様に、コレクションリストは、年別にグループ化された写真やビデオなどの資産コレクションのグループです。

## <a name="querying-model-data"></a>モデルデータのクエリ

PhotoKit を使用すると、さまざまなフェッチ方法でモデルデータのクエリを簡単に実行できます。 たとえば、すべてのイメージを取得するには、`PHAsset.Fetch`を呼び出し、`PHAssetMediaType.Image` メディアの種類を渡します。

```csharp
PHFetchResult fetchResults = PHAsset.FetchAssets (PHAssetMediaType.Image, null);
```

`PHFetchResult` インスタンスには、イメージを表すすべての `PHAsset` インスタンスが含まれます。 イメージ自体を取得するには、`PHImageManager` (またはキャッシュバージョン、`PHCachingImageManager`) を使用して、`RequestImageForAsset`を呼び出してイメージに対する要求を行います。 たとえば、次のコードは、コレクションビューセルに表示する `PHFetchResult` 内の各資産のイメージを取得します。

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

この結果、次のようにイメージのグリッドが表示されます。

![](photokit-images/image4.png "The running app displaying a grid of images")

## <a name="saving-changes-to-the-photo-library"></a>フォトライブラリへの変更の保存

これは、クエリとデータの読み取りを処理する方法です。 また、変更をライブラリに書き戻すこともできます。 複数の関心のあるアプリケーションはシステムのフォトライブラリと対話できるため、`PhotoLibraryObserver`を使用して、変更を通知するようにオブザーバーを登録できます。 変更が行われると、アプリケーションがそれに応じて更新される可能性があります。 たとえば、上記のコレクションビューを再読み込みする単純な実装を次に示します。

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

実際に変更をアプリケーションから書き戻すには、変更要求を作成します。 各モデルクラスには、関連付けられた変更要求クラスがあります。 たとえば、`PHAsset`を変更するには、`PHAssetChangeRequest`を作成します。 写真ライブラリに書き戻され、上記のようにオブザーバーに送信される変更を行う手順は次のとおりです。

1. 編集操作を実行します。
2. フィルター処理された画像データを `PHContentEditingOutput` インスタンスに保存します。
3. 編集出力から変更を発行するための変更要求を行います。

コアイメージ noir フィルターを適用するイメージに変更を書き戻す例を次に示します。

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

ユーザーがボタンを選択すると、フィルターが適用されます。

![フィルターが適用される前と後の写真を示す2つの例](photokit-images/image5.png)

`PHPhotoLibraryChangeObserver`のおかげで、ユーザーが移動したときに、変更がコレクションビューに反映されます。

![変更した写真を表示するフォトコレクションビュー](photokit-images/image6.png)
