---
title: Xamarin Designer for iOS 内のカスタムコントロール
description: Xamarin Designer for iOS は、プロジェクトで作成されたカスタムコントロールの表示、または Xamarin コンポーネントストアなどの外部ソースからの参照をサポートします。
ms.prod: xamarin
ms.assetid: D8F07D63-B006-4050-9D1B-AC6FCDA71B99
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/22/2017
ms.openlocfilehash: e8c38ec407d13a99e2990a6d4cf39b5a23728b1d
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73003976"
---
# <a name="custom-controls-in-the-xamarin-designer-for-ios"></a>Xamarin Designer for iOS 内のカスタムコントロール

_Xamarin Designer for iOS は、プロジェクトで作成されたカスタムコントロールの表示、または Xamarin コンポーネントストアなどの外部ソースからの参照をサポートします。_

Xamarin Designer for iOS は、アプリケーションのユーザーインターフェイスを視覚化するための強力なツールであり、ほとんどの iOS ビューおよびビューコントローラーに対して WYSIWYG での編集がサポートされています。 アプリには、iOS に組み込まれているものを拡張するカスタムコントロールが含まれている場合もあります。 これらのカスタムコントロールは、いくつかのガイドラインを考慮して記述されている場合、iOS デザイナーでもレンダリングできるため、さらに充実した編集エクスペリエンスが提供されます。 このドキュメントでは、これらのガイドラインについて説明します。

## <a name="requirements"></a>［要件］

次のすべての要件を満たすコントロールがデザインサーフェイスに表示されます。

1. これは、 [Uiview](xref:UIKit.UIView)または[uiviewcontroller](xref:UIKit.UIViewController)の直接または間接のサブクラスです。 他の[NSObject](xref:Foundation.NSObject)サブクラスは、デザイン画面にアイコンとして表示されます。
2. これは、目的の C に公開する[Registerattribute](xref:Foundation.RegisterAttribute)を持っています。
3. これには[、必要な IntPtr コンストラクター](~/ios/internals/api-design/index.md)があります。
4. [IComponent](xref:System.ComponentModel.IComponent)インターフェイスを実装しているか、 [DesignTimeVisibleAttribute](xref:System.ComponentModel.DesignTimeVisibleAttribute)が True に設定されています。

上記の要件を満たすコードで定義されたコントロールは、コンテナーに含まれるプロジェクトがシミュレーター用にコンパイルされるときに、デザイナーに表示されます。 既定では、すべてのカスタムコントロールは、**ツールボックス**の **[カスタムコンポーネント]** セクションに表示されます。 ただし、[カテゴリ属性](xref:System.ComponentModel.CategoryAttribute)をカスタムコントロールのクラスに適用して、別のセクションを指定することもできます。

デザイナーでは、サードパーティの目標 C ライブラリの読み込みはサポートされていません。

## <a name="custom-properties"></a>カスタム プロパティ

次の条件が満たされている場合、カスタムコントロールによって宣言されたプロパティが [プロパティ] パネルに表示されます。

1. プロパティは、パブリック getter および setter を持ちます。
1. プロパティには[Exportattribute](xref:Foundation.ExportAttribute)と[BrowsableAttribute](xref:System.ComponentModel.BrowsableAttribute)が True に設定されています。
1. プロパティの型は、数値型、列挙型、文字列、ブール値、 [SizeF](xref:System.Drawing.SizeF)、 [UIColor](xref:UIKit.UIColor)、または[uiimage](xref:UIKit.UIImage)です。 サポートされている種類のこの一覧は、今後拡張される可能性があります。

プロパティは[Displaynameattribute](xref:System.ComponentModel.DisplayNameAttribute)で修飾して、プロパティパネルに表示されるラベルを指定することもできます。

## <a name="initialization"></a>初期化

`UIViewController` サブクラスでは、デザイナーで作成したビューに依存するコードに[ViewDidLoad](xref:UIKit.UIViewController.ViewDidLoad)メソッドを使用する必要があります。

`UIView` およびその他の `NSObject` サブクラスについては、 [AwakeFromNib](xref:Foundation.NSObject.AwakeFromNib)メソッドを使用して、レイアウトファイルから読み込まれた後でカスタムコントロールの初期化を実行することをお勧めします。 これは、プロパティパネルで設定されたカスタムプロパティは、コントロールのコンストラクターの実行時には設定されませんが、`AwakeFromNib` が呼び出される前に設定されるためです。

```csharp
[Register ("CustomView"), DesignTimeVisible (true)]
public class CustomView : UIView {

    public CustomView (IntPtr handle) : base (handle) { }

    public override void AwakeFromNib ()
    {
        // Initialize the view here.
    }
}
```

コントロールがコードから直接作成されるように設計されている場合は、次のように、共通の初期化コードを持つメソッドを作成することもできます。

```csharp
[Register ("CustomView"), DesignTimeVisible (true)]
public class CustomView : UIView {

    public CustomView (IntPtr handle) : base (handle) { }

    public CustomView ()
    {
        // Called when created from code.
        Initialize ();
    }

    public override void AwakeFromNib ()
    {
        // Called when loaded from xib or storyboard.
        Initialize ();
    }

    void Initialize ()
    {
        // Common initialization code here.
    }
}
```

## <a name="property-initialization-and-awakefromnib"></a>プロパティの初期化と AwakeFromNib

カスタムコンポーネントのデザイン可能なプロパティを初期化するタイミングと場所は、iOS デザイナー内で設定された値を上書きしないように注意する必要があります。 例として、次のコードを実行します。

```csharp
[Register ("CustomView"), DesignTimeVisible (true)]
public class CustomView : UIView {

    [Export ("Counter"), Browsable (true)]
    public int Counter {get; set;}

    public CustomView (IntPtr handle) : base (handle) { }

    public CustomView ()
    {
        // Called when created from code.
        Initialize ();
    }

    public override void AwakeFromNib ()
    {
        // Called when loaded from xib or storyboard.
        Initialize ();
    }

    void Initialize ()
    {
        // Common initialization code here.
        Counter = 0;
    }
}
```

`CustomView` コンポーネントは、iOS デザイナー内で開発者が設定できる `Counter` プロパティを公開します。 ただし、デザイナー内でどのような値が設定されていても、`Counter` プロパティの値は常にゼロ (0) になります。 その理由を説明します。

- `CustomControl` のインスタンスは、ストーリーボードファイルから大きくなっています。
- IOS designer で変更されたプロパティ (たとえば、`Counter` の値を2に設定するなど) が設定されます。
- `AwakeFromNib` メソッドが実行され、コンポーネントの `Initialize` メソッドが呼び出されます。
- 内部 `Initialize` `Counter` プロパティの値がゼロ (0) にリセットされています。

上記の状況を解決するには、`Counter` プロパティを別の場所 (コンポーネントのコンストラクターなど) に初期化するか、`AwakeFromNib` メソッドをオーバーライドせずに、コンポーネントが現在の内容の外部での初期化を必要としない場合に `Initialize` を呼び出します。コンストラクターによって処理されています。

## <a name="design-mode"></a>デザインモード

デザイン画面では、カスタムコントロールはいくつかの制限に従う必要があります。

- アプリバンドルリソースはデザインモードでは使用できません。 画像は、 [uiimage メソッド](xref:UIKit.UIImage)を使用して読み込まれるときに使用できます。
- Web 要求などの非同期操作は、デザインモードでは実行できません。 デザインサーフェイスでは、コントロールの UI に対するアニメーションまたはその他の非同期更新はサポートされていません。

カスタムコントロールは[IComponent](xref:System.ComponentModel.IComponent)を実装し、 [designmode](xref:System.ComponentModel.ISite.DesignMode)プロパティを使用して、デザインサーフェイス上にあるかどうかを確認できます。 この例では、ラベルのデザイン画面に "デザインモード" が表示され、実行時には "Runtime" と表示されます。

```csharp
[Register ("DesignerAwareLabel")]
public class DesignerAwareLabel : UILabel, IComponent {

    #region IComponent implementation

    public ISite Site { get; set; }
    public event EventHandler Disposed;

    #endregion

    public DesignerAwareLabel (IntPtr handle) : base (handle) { }

    public override void AwakeFromNib ()
    {
        if (Site != null && Site.DesignMode)
            Text = "Design Mode";
        else
            Text = "Runtime";
    }
}
```

すべてのメンバーにアクセスしようとする前に、常に `null` の `Site` プロパティを確認する必要があります。 `Site` が `null`場合、コントロールがデザイナーで実行されていないと想定するのは安全です。
デザインモードでは、コントロールのコンストラクターが実行されてから `AwakeFromNib` が呼び出される前に、`Site` が設定されます。

## <a name="debugging"></a>デバッグ

上記の要件を満たすコントロールは、ツールボックスに表示され、画面に表示されます。
コントロールがレンダリングされない場合は、コントロールまたはその依存関係のいずれかにバグがあるかどうかを確認します。

多くの場合、デザイン画面では、他のコントロールのレンダリングを継続しながら、個々のコントロールによってスローされた例外をキャッチできます。 欠陥のあるコントロールは赤いプレースホルダーに置き換えられ、感嘆符アイコンをクリックして例外トレースを表示できます。

 ![](ios-designable-controls-overview-images/exception-box.png "A faulty control as red placeholder and the exception details")

コントロールでデバッグシンボルを使用できる場合、トレースにはファイル名と行番号が含まれます。
スタックトレース内の行をダブルクリックすると、ソースコード内のその行にジャンプします。

デザイナーが欠陥のあるコントロールを分離できない場合は、デザイン画面の上部に警告メッセージが表示されます。

 ![](ios-designable-controls-overview-images/info-bar.png "A warning message at the top of the design surface")

問題のあるコントロールが固定されているか、デザインサーフェイスから削除されると、完全なレンダリングが再開されます。

## <a name="summary"></a>まとめ

この記事では、iOS designer でのカスタムコントロールの作成と適用について説明しました。 まず、デザイン画面に表示するためにコントロールが満たす必要のある要件と、プロパティパネルでカスタムプロパティを公開する要件について説明します。 次に、コントロールと DesignMode プロパティの分離コードを確認します。 最後に、例外がスローされた場合の動作と、その解決方法について説明しました。
