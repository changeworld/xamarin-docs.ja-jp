---
title: Xamarin.Forms でのファイル処理
description: Xamarin.Forms を使ったファイルの処理は、.NET Standard ライブラリにあるコードを使うか、埋め込みリソースを使うことで実現できます。
ms.prod: xamarin
ms.assetid: 9987C3F6-5F04-403B-BBB4-ECB024EA6CC8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/21/2018
ms.openlocfilehash: fb3bbda3caee9fdbd490aaea7e119baf470eedd1
ms.sourcegitcommit: 4cf434b126eb7df6b2fd9bb1d71613bf2b6aac0e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/07/2019
ms.locfileid: "71997157"
---
# <a name="file-handling-in-xamarinforms"></a>Xamarin.Forms でのファイル処理

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithfiles)

_Xamarin.Forms を使ったファイルの処理は、.NET Standard ライブラリにあるコードを使うか、埋め込みリソースを使うことで実現できます。_

## <a name="overview"></a>概要

Xamarin.Forms コードは複数のプラットフォームで実行されますが、各プラットフォームには独自のファイルシステムがあります。 そのことは以前、各プラットフォームでネイティブ ファイル API を使用することが最も簡単なファイルの読み書き方法であったことを意味しました。 代替的に、アプリでデータ ファイルを配布する方法として埋め込みリソースが最も単純な解決策となります。 ただし、.NET Standard 2.0 の場合、.NET Standard ライブラリでファイル アクセス コードを共有できます。

イメージ ファイルの処理方法については、[イメージの使用](~/xamarin-forms/user-interface/images.md)に関するページを参照してください。

<a name="Loading_and_Saving_Files" />

## <a name="saving-and-loading-files"></a>ファイルの保存と読み込み

`System.IO` クラスを使用し、各プラットフォームのファイル システムにアクセスできます。 `File` クラスでは、ファイルを作成し、削除し、読み込むことができます。`Directory` クラスでは、ディレクトリの内容を作成し、削除し、列挙できます。 `Stream` サブクラスを使用することもできます。このサブクラスは、ファイル操作をより詳細に制御できます (圧縮やファイル内の位置検索など)。

テキスト ファイルは `File.WriteAllText` メソッドを利用して書き込むことができます。

```csharp
File.WriteAllText(fileName, text);
```

テキスト ファイルは `File.ReadAllText` メソッドを利用して読み込むことができます。

```csharp
string text = File.ReadAllText(fileName);
```

また、`File.Exists` メソッドでは、指定のファイルが存在するかどうかが判断されます。

```csharp
bool doesExist = File.Exists(fileName);
```

各プラットフォームのファイル パスは、`Environment.GetFolderPath` メソッドの最初の引数として [`Environment.SpecialFolder`](xref:System.Environment.SpecialFolder) 列挙の値を使用することで、.NET Standard ライブラリから特定できます。 これを次に、`Path.Combine` メソッドでファイル名と組み合わせることができます。

```csharp
string fileName = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "temp.txt");
```

このような操作はサンプル アプリでデモとして示されています。これには、テキストを保存し、読み込むページが含まれています。

[ファイルの保存![と読み込み](files-images/saveandload-sml.png "アプリでのファイルの保存と読み込み")](files-images/saveandload.png#lightbox "アプリでのファイルの保存と読み込み")

<a name="Loading_Files_Embedded_as_Resources" />

## <a name="loading-files-embedded-as-resources"></a>埋め込みリソースファイルの読み込み

**.NET Standard** アセンブリにファイルを埋め込むには、ファイルを作成するか追加し、 **[ビルド アクション] が[EmbeddedResource]** になっていることを確認します。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![埋め込みリソースビルドアクション](files-images/vs-embeddedresource-sml.png "設定 EmbeddedResource BuildAction")を構成しています](files-images/vs-embeddedresource.png#lightbox "EmbeddedResource BuildAction の設定")

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![.Net standard library に埋め込まれたテキストファイル、埋め込みリソースビルドアクション](files-images/xs-embeddedresource-sml.png "設定 EmbeddedResource BuildAction")](files-images/xs-embeddedresource.png#lightbox "EmbeddedResource BuildAction の設定")

-----

`GetManifestResourceStream` は、その**リソース ID** を使用して埋め込みファイルにアクセスするために使用されます。 既定では、リソース ID は、そのファイルが埋め込まれるプロジェクトの既定の名前空間で始まるファイル名です。この場合、アセンブリは**WorkingWithFiles** 、ファイル名は**libtextresource .txt**になります。そのため、リソース id は `WorkingWithFiles.LibTextResource.txt` です。

```csharp
var assembly = IntrospectionExtensions.GetTypeInfo(typeof(LoadResourceText)).Assembly;
Stream stream = assembly.GetManifestResourceStream("WorkingWithFiles.LibTextResource.txt");
string text = "";
using (var reader = new System.IO.StreamReader (stream))
{  
    text = reader.ReadToEnd ();
}
```

それから `text` 変数を利用してテキストを表示するか、利用しない場合、コードで使用します。 [サンプル アプリ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithfiles)のこのスクリーンショットで、`Label` コントロールでレンダリングされたテキストを確認できます。

 [(files-images/pcltext-sml.png "アプリに表示される .NET Standard ライブラリの") ![.Net Standard Library embedded テキストファイルに埋め込まれたテキストファイル]](files-images/pcltext.png#lightbox "アプリに表示される .NET Standard ライブラリ内の埋め込みテキストファイル")

XML の読み込みと逆シリアル化は同じくらい簡単です。 次のコードは、リソースから読み込まれ、逆シリアル化され、表示のために `ListView` にバインドされる XML ファイルを示します。 この XML ファイルには `Monkey` オブジェクトの配列が含まれています (クラスはサンプル コードで定義されています)。

```csharp
var assembly = IntrospectionExtensions.GetTypeInfo(typeof(LoadResourceText)).Assembly;
Stream stream = assembly.GetManifestResourceStream("WorkingWithFiles.LibXmlResource.xml");
List<Monkey> monkeys;
using (var reader = new System.IO.StreamReader (stream)) {
    var serializer = new XmlSerializer(typeof(List<Monkey>));
    monkeys = (List<Monkey>)serializer.Deserialize(reader);
}
var listView = new ListView ();
listView.ItemsSource = monkeys;
```

 [![.Net standard library に埋め込まれた Xml ファイル]。(files-images/pclxml-sml.png "Listview に表示される .net Standard library の LISTVIEW 埋め込み xml ファイル")に表示されます。](files-images/pclxml.png#lightbox "ListView に表示される .NET standard library の埋め込み XML ファイル")

<a name="Embedding_in_Shared_Projects" />

## <a name="embedding-in-shared-projects"></a>Shared プロジェクトでの埋め込み

共有プロジェクトにも埋め込みリソースとしてファイルを含めることができます。ただし、共有プロジェクトの内容は参照元プロジェクトにコンパイルされるため、埋め込みファイル リソース ID に使用されるプレフィックスは変わることがあります。 つまり、埋め込みファイルごとのリソース ID はプラットフォームによって異なる場合があります。

共有プロジェクトのこの問題には 2 つの解決策があります。

- **プロジェクトを同期する** - 各プラットフォームのプロジェクト プロパティを編集し、**同じ**アセンブリ名と既定の名前空間を使用します。 この値は共有プロジェクトの埋め込みリソース ID のプレフィックスとして "ハードコード" できます。
- **#if コンパイラ ディレクティブ** - コンパイラ ディレクティブを使用して正しいリソース ID プレフィックスを設定し、その値を使用して正しいリソース ID を動的に構築します。

2 つ目のオプションを示すコードを以下に示します。 コンパイラ ディレクティブを使用し、ハードコードされたリソース プレフィックス (通常、参照元プロジェクトの既定の名前空間と同じです) を選択します。 次に、`resourcePrefix` 変数を埋め込みリソース ファイル名と連結することで有効なリソース ID が作成されます。

```csharp
#if __IOS__
var resourcePrefix = "WorkingWithFiles.iOS.";
#endif
#if __ANDROID__
var resourcePrefix = "WorkingWithFiles.Droid.";
#endif

Debug.WriteLine("Using this resource prefix: " + resourcePrefix);
// note that the prefix includes the trailing period '.' that is required
var assembly = IntrospectionExtensions.GetTypeInfo(typeof(SharedPage)).Assembly;
Stream stream = assembly.GetManifestResourceStream
    (resourcePrefix + "SharedTextResource.txt");
```

<a name="Organizing_Resources" />

### <a name="organizing-resources"></a>リソースを整理する

上の例では、.NET Standard ライブラリ プロジェクトのルートにファイルが埋め込まれるものと想定されています。その場合、リソース ID の形式は **Namespace.Filename.Extension** で、`WorkingWithFiles.LibTextResource.txt` や `WorkingWithFiles.iOS.SharedTextResource.txt` のようになります。

埋め込みリソースはフォルダーで整理できます。 埋め込みリソースをフォルダーに入れると、フォルダー名がリソース ID の一部になります (ピリオドで区切られます)。そのため、リソース ID の形式は **Namespace.Folder.Filename.Extension** になります。 サンプル アプリで使用するファイルをフォルダー **MyFolder** に入れると、対応するリソース ID が `WorkingWithFiles.MyFolder.LibTextResource.txt` と `WorkingWithFiles.iOS.MyFolder.SharedTextResource.txt` になります。

<a name="Debugging_Embedded_Resources" />

### <a name="debugging-embedded-resources"></a>埋め込みリソースのデバッグ

特定のリソースが読み込まれない理由がわからない場合があるため、リソースが正しく構成されていることを確認する目的で、次のデバッグ コードをアプリケーションに一時的に追加できます。 所与のアセンブリに埋め込まれている既知のリソースがすべて **[エラー]** パッドに出力されるので、リソース読み込み問題のデバッグに役立ちます。

```csharp
using System.Reflection;
// ...
// use for debugging, not in released app code!
var assembly = IntrospectionExtensions.GetTypeInfo(typeof(SharedPage)).Assembly;
foreach (var res in assembly.GetManifestResourceNames()) {
    System.Diagnostics.Debug.WriteLine("found resource: " + res);
}
```

## <a name="summary"></a>まとめ

この記事では、デバイスでテキストを保存し、読み込むことや埋め込みリソースを読み込むことなど、簡単なファイル操作をいくつか確認しました。 .NET Standard 2.0 の場合、.NET Standard ライブラリでファイル アクセス コードを共有できます。

## <a name="related-links"></a>関連リンク

- [FilesSample](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithfiles)
- [Xamarin.Forms のサンプル](https://github.com/xamarin/xamarin-forms-samples)
- [Xamarin.iOS でファイル システムを操作する](~/ios/app-fundamentals/file-system.md)
