---
title: Xamarin の統合されたストーリーボード
description: このドキュメントでは、Xamarin. iOS の統合されたストーリーボードについて説明します。 統合されたストーリーボードを使用すると、開発者は1つのインターフェイス定義で複数の画面サイズをサポートできます。
ms.prod: xamarin
ms.assetid: F6F70374-FC2A-4401-A712-A16D0F9B340F
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/20/2017
ms.openlocfilehash: 13891100d3571f9e847243172aa974072f46e7fe
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/13/2020
ms.locfileid: "79306255"
---
# <a name="unified-storyboards-in-xamarinios"></a>Xamarin の統合されたストーリーボード

iOS 8 には、統合されたストーリーボードという、ユーザーインターフェイスを作成するための、使いやすい新しい機構が含まれています。 1つのストーリーボードを使用してさまざまなハードウェア画面サイズをすべてカバーすると、高速および応答性の高いビューを "設計時に1回、使用する多" スタイルで作成できます。

開発者は、iPhone と iPad デバイス用に個別の特定のストーリーボードを作成する必要がなくなったため、共通のインターフェイスを使用してアプリケーションを設計し、さまざまなサイズのクラスでそのインターフェイスをカスタマイズする柔軟性があります。 この方法では、各フォームファクターの長所に合わせてアプリケーションを調整できます。各ユーザーインターフェイスは、最適なエクスペリエンスを提供するように調整できます。

<a name="size-classes" />

## <a name="size-classes"></a>サイズクラス

IOS 8 より前の開発者は、`UIInterfaceOrientation` と `UIInterfaceIdiom` を使用して、縦モードと横モードを区別し、iPhone と iPad デバイスの間を区別していました。 IOS8 では、方向とデバイスは*サイズクラス*を使用して決定されます。

デバイスは、垂直軸と水平軸の両方でサイズクラスによって定義され、iOS 8 には2種類のサイズクラスがあります。

- **Regular** –これは、サイズの大きい画面 (iPad など)、または大きなサイズの印象を与えるガジェット (`UIScrollView` など) のためのものです。
- **Compact** –これは、より小さなデバイス (iPhone など) を対象としています。 このサイズでは、デバイスの向きが考慮されます。

2つの概念が一緒に使用されている場合は、次の図に示すように、異なる向きの両方で使用できるさまざまなサイズを定義する2つの x 2 グリッドが生成されます。

 [![](unified-storyboards-images/sizeclassgrid.png "A 2 x 2 grid that defines the different possible sizes that can be used in Regular and Compact orientations")](unified-storyboards-images/sizeclassgrid.png#lightbox)

開発者は、(上の図に示すように) レイアウトが異なる4つの方法のいずれかを使用するビューコントローラーを作成できます。

### <a name="ipad-size-classes"></a>iPad サイズクラス

IPad は、サイズが原因で、両方の向きに対して**通常**のクラスサイズが使用されます。

 [![](unified-storyboards-images/image1.png "iPad Size Classes")](unified-storyboards-images/image1.png#lightbox)

### <a name="iphone-size-classes"></a>iPhone サイズクラス

IPhone のサイズクラスは、デバイスの向きによって異なります。

 [![](unified-storyboards-images/iphonesizeclasses.png "iPhone Size Classes")](unified-storyboards-images/iphonesizeclasses.png#lightbox)

- デバイスが縦モードのとき、画面には水平方向と**標準**時の**コンパクト**なクラスがあります。
- デバイスが横モードの場合、画面クラスは縦モードから逆になります。

### <a name="iphone-6-plus-size-classes"></a>iPhone 6 Plus サイズクラス

サイズは、縦向きの場合は以前のスマートフォンと同じですが、横には異なります。

[![](unified-storyboards-images/iphone6sizeclasses.png "iPhone 6 Plus Size Classes")](unified-storyboards-images/iphone6sizeclasses.png#lightbox)

IPhone 6 Plus には十分な大きさの画面があるため、横モードでは通常の幅サイズクラスを持つことができます。

### <a name="support-for-a-new-screen-scale"></a>新しい画面スケールのサポート

IPhone 6 Plus では、3.0 (元の iPhone 画面解像度3倍) の画面スケールファクターで新しい Retina HD ディスプレイが使用されます。 これらのデバイスで最適なエクスペリエンスを提供するには、この画面のスケール用に設計された新しいアートワークを追加します。 Xcode 6 以降では、アセットカタログには、1倍、2倍、3倍のサイズで画像を含めることができます。新しいイメージ資産を追加するだけで、iOS は iPhone 6 Plus で実行されるときに正しい資産を選択します。

IOS のイメージ読み込み動作でも、イメージファイルの `@3x` サフィックスが認識されます。 たとえば、開発者がアプリケーションのバンドルに、`MonkeyIcon.png`、`MonkeyIcon@2x.png`、および `MonkeyIcon@3x.png`という名前のイメージアセット (異なる解像度) を含めているとします。 IPhone 6 に加えて、開発者が次のコードを使用してイメージを読み込む場合は、`MonkeyIcon@3x.png` イメージが自動的に使用されます。

```csharp
UIImage icon = UIImage.FromFile("MonkeyImage.png");
```

または、`MonkeyIcon.png`として iOS デザイナーを使用してイメージを UI 要素に割り当てた場合、iPhone 6 Plus でも `MonkeyIcon@3x.png` が自動的に使用されます。

<a name="dynamic-launch-screens" />

### <a name="dynamic-launch-screens"></a>動的な起動画面

アプリが実際に起動されていることをユーザーにフィードバックするために、iOS アプリケーションを起動しているときに、起動画面ファイルがスプラッシュスクリーンとして表示されます。 IOS 8 より前の開発者は、アプリケーションが実行されているデバイスの種類、向き、画面の解像度ごとに、複数の `Default.png` イメージ資産を含める必要があります。

IOS 8 を初めて使用する場合、開発者は、Auto Layout クラスと Size クラスを使用して、すべてのデバイス、解像度、および向きに対して機能する*動的な起動画面*を作成する、Xcode の単一のアトミック `.xib` ファイルを作成できます。 これにより、開発者が必要なすべてのイメージ資産を作成して維持するために必要な作業量が減少するだけでなく、アプリケーションのインストール済みバンドルのサイズが縮小されます。

## <a name="traits"></a>Traits

特徴は、環境の変化に合わせてレイアウトがどのように変化するかを決定するために使用できるプロパティです。 これらは、一連のプロパティ (`UIUserInterfaceSizeClass`に基づく `HorizontalSizeClass` と `VerticalSizeClass`)、およびインターフェイス表現 (`UIUserInterfaceIdiom`) と表示スケールで構成されます。

上記のすべての状態は、Apple が特徴コレクション (`UITraitCollection`) として参照するコンテナーにラップされます。これには、プロパティだけでなくその値も含まれます。

## <a name="trait-environment"></a>特徴環境

特徴環境は、iOS 8 の新しいインターフェイスであり、次のオブジェクトの特徴コレクションを返すことができます。

- 画面 (`UIScreens`)。
- Windows (`UIWindows`)。
- コントローラー (`UIViewController`) を表示します。
- ビュー (`UIView`)。
- プレゼンテーションコントローラー (`UIPresentationController`)。

開発者は、特徴環境によって返される特徴コレクションを使用して、ユーザーインターフェイスをどのようにレイアウトするかを決定します。

すべての特徴環境は、次の図に示すように階層を作成します。

 [![](unified-storyboards-images/viewhierarchy.png "The Trait Environments hierarchy diagram")](unified-storyboards-images/viewhierarchy.png#lightbox)

上記の特徴の特徴コレクションでは、既定で親環境から子環境にフローします。

特徴環境には、現在の特徴コレクションを取得するだけでなく、`TraitCollectionDidChange` メソッドもあります。このメソッドは、ビューまたはビューコントローラーサブクラスでオーバーライドできます。 開発者は、この方法を使用して、特徴が変更されたときに特性に依存する UI 要素を変更できます。

## <a name="typical-trait-collections"></a>一般的な特徴コレクション

このセクションでは、iOS 8 を使用する場合にユーザーが経験する一般的な特徴コレクションについて説明します。

次に示すのは、開発者が iPhone で見ることができる一般的な特徴コレクションです。

|プロパティ|値|
|--- |--- |
|`HorizontalSizeClass`|Compact (英語の可能性あり)|
|`VerticalSizeClass`|通常|
|`UserInterfaceIdom`|Phone|
|`DisplayScale`|2.0|

上のセットは、すべての特徴プロパティの値を持つため、完全修飾された特徴コレクションを表します。

また、値の一部が欠落している特徴コレクションがある可能性もあります (Apple は*未指定*として参照します)。

|プロパティ|値|
|--- |--- |
|`HorizontalSizeClass`|Compact (英語の可能性あり)|
|`VerticalSizeClass`|未指定|
|`UserInterfaceIdom`|未指定|
|`DisplayScale`|未指定|

ただし、一般に、開発者が特徴のコレクションに特徴の環境を要求すると、上記の例に示すように、完全修飾コレクションが返されます。

特徴環境 (ビュー、ビューコントローラーなど) が現在のビュー階層の内部にない場合、開発者は1つまたは複数の特徴プロパティの指定されていない値を取得する可能性があります。

また、`UITraitCollection.FromHorizontalSizeClass`など、Apple によって提供されるいずれかの作成方法を使用して新しいコレクションを作成する場合、開発者は、部分的に修飾された特徴コレクションも取得します。

複数の特徴コレクションに対して実行できる操作の1つは、相互に比較することです。これには、特徴コレクションに別の特徴コレクションが含まれている場合は、それらを相互に比較する必要があります。 *コンテインメント*の目的は、2番目のコレクションで指定されたすべての特徴について、値が最初のコレクションの値と正確に一致する必要があることです。

2つの特徴をテストするには、`UITraitCollection` の `Contains` メソッドを使用して、テストする特徴の値を渡します。

開発者は、コード内で手動で比較を実行して、ビューまたはビューコントローラーのレイアウト方法を決定できます。 ただし、`UIKit` は、このメソッドを内部的に使用して、外観プロキシなどの機能の一部を提供します。

## <a name="appearance-proxy"></a>表示プロキシ

外観プロキシは、開発者がビューのプロパティをカスタマイズできるように、以前のバージョンの iOS で導入されました。 特徴コレクションをサポートするために、iOS 8 で拡張されています。

外観プロキシには、渡された特定の特徴コレクションの新しい外観プロキシを返す新しいメソッド `AppearanceForTraitCollection`が含まれるようになりました。 開発者がその外観プロキシで実行するカスタマイズは、指定された特徴コレクションに準拠するビューに対してのみ有効になります。

通常、開発者は、部分的に指定された特徴コレクションを `AppearanceForTraitCollection` メソッドに渡します。たとえば、水平方向のサイズクラス Compact を指定した場合は、水平方向に圧縮されたアプリケーション内の任意のビューをカスタマイズできます。

## <a name="uiimage"></a>UIImage

Apple が特徴コレクションを追加した別のクラスが `UIImage`ます。 これまで、開発者は、アプリケーションに含めるビットマップグラフィックアセット (アイコンなど) の @1X と @2x バージョンを指定する必要がありました。

iOS 8 が拡張され、開発者は特徴コレクションに基づいてイメージカタログに複数のバージョンのイメージを含めることができるようになりました。 たとえば、開発者はコンパクトな特徴クラスを使用するためのイメージを小さくし、他のすべてのコレクションに対してイメージを完全にサイズ設定することができます。

イメージの1つが `UIImageView` クラスの内部で使用されている場合、イメージビューには、その特徴コレクションの正しいバージョンのイメージが自動的に表示されます。 特徴環境が変更された場合 (ユーザーがデバイスを縦向きから横方向に切り替えた場合など)、新しい特徴コレクションに合わせて新しいイメージサイズが自動的に選択され、イメージの現在のバージョンのサイズに合わせてサイズが変更されます。さ.

## <a name="uiimageasset"></a>UIImageAsset

Apple は `UIImageAsset` と呼ばれる iOS 8 に新しいクラスを追加し、開発者がイメージ選択をさらに細かく制御できるようにしました。

イメージ資産は、イメージのさまざまなバージョンをすべてラップし、開発者が渡された特徴コレクションに一致する特定のイメージを要求できるようにします。 イメージは、イメージアセットから追加または削除できます。

イメージ資産の詳細については、Apple の[Uiimageasset](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIImageAsset_Ref/index.html#//apple_ref/occ/cl/UIImageAsset)のドキュメントを参照してください。

## <a name="combining-trait-collections"></a>特徴コレクションの結合

特徴コレクションに対して開発者が実行できるもう1つの関数は、結合されたコレクションになる2つのを加算することです。この場合、1つのコレクションの未指定の値は、2つ目のコレクションの指定した値に置き換えられます。 これは、`UITraitCollection` クラスの `FromTraitsFromCollections` メソッドを使用して行います。

前述のように、いずれかの特徴が特徴コレクションのいずれかに指定されておらず、別の特性に指定されている場合、値は指定されたバージョンに設定されます。 ただし、特定の値の複数のバージョンが指定されている場合は、最後の特徴のコレクションの値が使用される値になります。

## <a name="adaptive-view-controllers"></a>アダプティブビューコントローラー

このセクションでは、iOS ビューおよびビューコントローラーが、開発者のアプリケーションで自動的に適応できるように、特徴クラスとサイズクラスの概念を採用した方法の詳細について説明します。

### <a name="split-view-controller"></a>ビューコントローラーの分割

IOS 8 の最も多くを変更したビューコントローラークラスの1つに、`UISplitViewController` クラスがあります。 以前は、多くの場合、開発者はアプリケーションの iPad バージョンで分割ビューコントローラーを使用します。その後、iPhone バージョンのアプリのビュー階層の完全に異なるバージョンを提供する必要があります。

IOS 8 では、`UISplitViewController` クラスを両方のプラットフォーム (iPad および iPhone) で使用できます。これにより、開発者は iPhone と iPad の両方で機能する1つのビューコントローラー階層を作成できます。

IPhone が横向きの場合、分割ビューコントローラーは、iPad に表示される場合と同様に、ビューを並べて表示します。

### <a name="overriding-traits"></a>オーバーライド (特徴を)

特徴環境は、次の図に示すように、親コンテナーから子コンテナーにカスケードされます。これは、iPad 上の分割ビューコントローラーが横向きに表示されることを示しています。

 [![](unified-storyboards-images/cascadingclasses01.png "A Split View Controller on an iPad in the landscape orientation")](unified-storyboards-images/cascadingclasses01.png#lightbox)

IPad では、水平方向と垂直方向の両方に通常のサイズクラスがあるため、分割ビューにはマスタービューと詳細ビューの両方が表示されます。

サイズクラスが両方の方向で圧縮されている iPhone では、分割ビューコントローラーは詳細ビューのみを表示します。次に例を示します。

 [![](unified-storyboards-images/cascadingclasses02.png "The Split View Controller only displays the detail view")](unified-storyboards-images/cascadingclasses02.png#lightbox)

開発者が横方向の iPhone でマスタービューと詳細ビューの両方を表示する必要があるアプリケーションでは、開発者は分割ビューコントローラーの親コンテナーを挿入し、特徴コレクションをオーバーライドする必要があります。 次の図に示すようになります。

 [![](unified-storyboards-images/cascadingclasses03.png "The developer must insert a parent container for the Split View Controller and override the Trait Collection")](unified-storyboards-images/cascadingclasses03.png#lightbox)

`UIView` が分割ビューコントローラーの親として設定され、`SetOverrideTraitCollection` メソッドが、新しい特徴コレクションを渡し、分割ビューコントローラーを対象とするビューで呼び出されます。 新しい特徴コレクションでは、`HorizontalSizeClass`をオーバーライドして `Regular`に設定します。これにより、分割ビューコントローラーでは、iPhone 上のマスタービューと詳細ビューの両方が横向きに表示されます。

`VerticalSizeClass` が `unspecified`に設定されていることに注意してください。これにより、新しい特徴コレクションを親の特徴コレクションに追加し、その結果、子分割ビューコントローラーの `Compact VerticalSizeClass` になります。

### <a name="trait-changes"></a>特徴の変更

このセクションでは、特徴の環境が変更されたときに特徴コレクションがどのように変化するかについて詳しく説明します。 たとえば、デバイスが縦向きから横方向に回転しているとします。

 [![](unified-storyboards-images/traittransitions01.png "The portrait to landscape Trait Changes overview")](unified-storyboards-images/traittransitions01.png#lightbox)

まず、iOS 8 では、移行を行うための準備としていくつかのセットアップを行います。 次に、システムによって遷移状態がアニメーション化されます。 最後に、iOS 8 は、移行中に必要な一時的な状態をクリーンアップします。

iOS 8 には、次の表に示すように、開発者が特徴の変更に参加するために使用できるいくつかのコールバックが用意されています。

|段階|コールバック|説明|
|--- |--- |--- |
|セットアップ|<ul><li>`WillTransitionToTraitCollection`</li><li>`TraitCollectionDidChange`</li></ul>|<ul><li>このメソッドは、特徴コレクションが新しい値に設定される前に、特徴変更の開始時に呼び出されます。</li><li>このメソッドは、特徴コレクションの値が変更されたときに、アニメーションが行われる前に呼び出されます。</li></ul>|
|アニメーション|`WillTransitionToTraitCollection`|このメソッドに渡される遷移コーディネーターには `AnimateAlongside` プロパティがあり、これを使用すると、開発者は既定のアニメーションと共に実行されるアニメーションを追加できます。|
|クリーンアップ|`WillTransitionToTraitCollection`|移行の実行後に、開発者が独自のクリーンアップコードを含めるためのメソッドを提供します。|

`WillTransitionToTraitCollection` メソッドは、ビューコントローラーを特徴コレクションの変更と共にアニメーション化する場合に適しています。 `WillTransitionToTraitCollection` メソッドは、ビューコントローラー (`UIViewController`) でのみ使用でき、`UIViews`などの他の特徴環境では使用できません。

`TraitCollectionDidChange` は、`UIView` クラスを使用する場合に適しています。開発者は、その特性が変更されたときに UI を更新する必要があります。

### <a name="collapsing-the-split-view-controllers"></a>分割ビューコントローラーの折りたたみ

では、分割ビューコントローラーが2つの列から1つの列ビューに折りたたまれたときの動作について詳しく見ていきましょう。 この変更の一環として、次の2つのプロセスを実行する必要があります。

- 既定では、分割ビューコントローラーは、折りたたみが発生した後に、ビューとしてプライマリビューコントローラーを使用します。 開発者は、`UISplitViewControllerDelegate` の `GetPrimaryViewControllerForCollapsingSplitViewController` メソッドをオーバーライドし、折りたたまれた状態で表示するビューコントローラーを提供することで、この動作をオーバーライドできます。
- セカンダリビューコントローラーは、プライマリビューコントローラーにマージする必要があります。 通常、開発者はこの手順に対して何らかの操作を行う必要はありません。分割ビューコントローラーには、ハードウェアデバイスに基づくこのフェーズの自動的な処理が含まれます。 ただし、開発者がこの変更を操作する必要がある特殊なケースもあります。 `UISplitViewControllerDelegate` の `CollapseSecondViewController` メソッドを呼び出すと、詳細ビューの代わりに、折りたたみが発生したときにマスタービューコントローラーを表示できます。

### <a name="expanding-the-split-view-controller"></a>分割ビューコントローラーを展開する

ここで、分割ビューコントローラーが折りたたまれた状態から展開された場合の動作について詳しく見ていきましょう。 この場合も、次の2つの段階があります。

- まず、新しいプライマリビューコントローラーを定義します。 既定では、分割ビューコントローラーは、折りたたまれたビューのプライマリビューコントローラーを自動的に使用します。 ここでも、開発者は、`UISplitViewControllerDelegate` の `GetPrimaryViewControllerForExpandingSplitViewController` メソッドを使用して、この動作をオーバーライドできます。
- プライマリビューコントローラーを選択したら、セカンダリビューコントローラーを再作成する必要があります。 ここでも、分割ビューコントローラーには、ハードウェアデバイスに基づくこのフェーズの自動処理が含まれています。 開発者は、`UISplitViewControllerDelegate` の `SeparateSecondaryViewController` メソッドを呼び出すことによって、この動作をオーバーライドできます。

分割ビューコントローラーでは、プライマリビューコントローラーは、`UISplitViewControllerDelegate`の `CollapseSecondViewController` および `SeparateSecondaryViewController` メソッドを実装することによって、ビューの展開と折りたたみの両方でパーツを再生します。 `UINavigationController` は、これらのメソッドを実装して、セカンダリビューコントローラーを自動的にプッシュしてポップします。

### <a name="showing-view-controllers"></a>ビューコントローラーの表示

Apple が iOS 8 に加えたもう1つの変更は、開発者がビューコントローラーを表示する方法です。 以前は、アプリケーションにリーフビューコントローラー (テーブルビューコントローラーなど) があり、開発者が別のビューコントローラーを表示した場合 (たとえば、ユーザーがセルをタップした場合など)、アプリケーションはコントローラー階層を介してナビゲーションビューコントローラーに戻り、それに対して `PushViewController` メソッドを呼び出して新しいビューを表示します。

これにより、ナビゲーションコントローラーと、それが実行されていた環境との間に非常に密接な結合が行われました。 IOS 8 では、Apple は次の2つの新しい方法を提供してこれを分離しました。

- `ShowViewController` –環境に基づいて新しいビューコントローラーを表示するように適応します。 たとえば、`UINavigationController` では、単に新しいビューをスタックにプッシュします。 分割ビューコントローラーでは、新しいビューコントローラーが新しいプライマリビューコントローラーとして左側に表示されます。 コンテナービューコントローラーが存在しない場合、新しいビューはモーダルビューコントローラーとして表示されます。
- `ShowDetailViewController` – `ShowViewController`と同様の方法で機能しますが、詳細ビューを渡される新しいビューコントローラーに置き換えるために、分割ビューコントローラーに実装されます。 (IPhone アプリケーションで見られるように) 分割ビューコントローラーが折りたたまれている場合は、呼び出しが `ShowViewController` メソッドにリダイレクトされ、新しいビューがプライマリビューコントローラーとして表示されます。 ここでも、コンテナービューコントローラーが存在しない場合、新しいビューはモーダルビューコントローラーとして表示されます。

これらのメソッドは、リーフビューコントローラーから開始し、新しいビューの表示を処理する適切なコンテナービューコントローラーが見つかるまでビュー階層をウォークします。

開発者は `ShowViewController` を実装し、独自のカスタムビューコントローラーに `ShowDetailViewController` して、`UINavigationController` と `UISplitViewController` が提供するのと同じ自動化された機能を実現できます。

### <a name="how-it-works"></a>しくみ

このセクションでは、これらのメソッドが実際に iOS 8 でどのように実装されているかを見ていきます。 まず、新しい `GetTargetForAction` メソッドを見てみましょう。

 [![](unified-storyboards-images/gettargetforaction.png "The new GetTargetForAction method")](unified-storyboards-images/gettargetforaction.png#lightbox)

このメソッドは、正しいコンテナービューコントローラーが見つかるまで階層チェーンをウォークします。 例 :

1. `ShowViewController` メソッドが呼び出されると、このメソッドを実装するチェーン内の最初のビューコントローラーがナビゲーションコントローラーになるため、新しいビューの親として使用されます。
1. `ShowDetailViewController` メソッドが代わりに呼び出された場合、分割ビューコントローラーは、それを実装するための最初のビューコントローラーであるため、親として使用されます。

`GetTargetForAction` メソッドは、特定のアクションを実装するビューコントローラーを特定し、そのアクションを受信するかどうかをそのビューコントローラーに要求することによって機能します。 このメソッドはパブリックであるため、開発者は、組み込みの `ShowViewController` および `ShowDetailViewController` メソッドと同様に機能する独自のカスタムメソッドを作成できます。

## <a name="adaptive-presentation"></a>適応型プレゼンテーション

IOS 8 では、Apple は Segue プレゼンテーション (`UIPopoverPresentationController`) のアダプティブも作成しました。 そのため、Segue Presentation View Controller は通常の Segue ビューを標準サイズクラスで自動的に表示しますが、完全な画面を水平方向のコンパクトなサイズのクラス (iPhone など) で表示します。

統合されたストーリーボードシステム内の変更に対応するために、表示されるビューコントローラーを管理するための新しいコントローラーオブジェクトが作成されました (`UIPresentationController`)。 このコントローラーは、ビューコントローラーが表示された時点から、破棄されるまで作成されます。 このクラスは管理クラスであるため、ビューコントローラーのスーパークラスと見なすことができます。これは、ビューコントローラー (向きなど) に影響するデバイスの変更に応答するため、プレゼンテーションコントローラーが制御するビューコントローラーに戻されます。

開発者が `PresentViewController` メソッドを使用してビューコントローラーを提示すると、プレゼンテーションプロセスの管理が `UIKit`に渡されます。 UIKit では、作成されているスタイルに適したコントローラーが処理されます。ただし、ビューコントローラーのスタイルが `UIModalPresentationCustom`に設定されている場合にのみ例外が発生します。 ここでは、アプリケーションは、`UIKit` コントローラーを使用するのではなく、独自のプレゼンテーションコントローラーを提供できます。

### <a name="custom-presentation-styles"></a>カスタムプレゼンテーションスタイル

カスタムプレゼンテーションスタイルでは、開発者はカスタムプレゼンテーションコントローラーを使用することができます。 このカスタムコントローラーを使用すると、その関係を持つビューの外観と動作を変更できます。

<a name="size-classes"/>

## <a name="working-with-size-classes"></a>サイズクラスの使用

この記事に含まれているアダプティブフォト Xamarin プロジェクトは、iOS 8 統合インターフェイスアプリケーションでサイズクラスとアダプティブビューコントローラーを使用するための実用的な例を提供します。

アプリケーションは、IOS デザイナーを使用して統合されたストーリーボードを作成するのではなく、コードから完全に UI を作成しますが、同じ手法が適用されます。 この記事の後半では、Xamarin アプリケーションで、統合されたストーリーボードと iOS デザイナーでサイズクラスを使用する方法について説明します。

次に、アダプティブフォトプロジェクトが、アダプティブアプリケーションを作成するために、iOS 8 でいくつかのサイズクラス機能を実装する方法について詳しく見ていきましょう。

### <a name="adapting-to-trait-environment-changes"></a>特徴環境の変更に適応する

IPhone でアダプティブフォトアプリケーションを実行しているときに、ユーザーがデバイスを縦向きから横方向に回転させると、分割ビューコントローラーには、マスタービューと詳細ビューの両方が表示されます。

 [![](unified-storyboards-images/rotation.png "The Split View Controller will display both the master and details view as seen here")](unified-storyboards-images/rotation.png#lightbox)

これを実現するには、ビューコントローラーの `UpdateConstraintsForTraitCollection` メソッドをオーバーライドし、`VerticalSizeClass`の値に基づいて制約を調整します。 例 :

```csharp
public void UpdateConstraintsForTraitCollection (UITraitCollection collection)
{
    var views = NSDictionary.FromObjectsAndKeys (
        new object[] { TopLayoutGuide, ImageView, NameLabel, ConversationsLabel, PhotosLabel },
        new object[] { "topLayoutGuide", "imageView", "nameLabel", "conversationsLabel", "photosLabel" }
    );

    var newConstraints = new List<NSLayoutConstraint> ();
    if (collection.VerticalSizeClass == UIUserInterfaceSizeClass.Compact) {
        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("|[imageView]-[nameLabel]-|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("[imageView]-[conversationsLabel]-|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("[imageView]-[photosLabel]-|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("V:|[topLayoutGuide]-[nameLabel]-[conversationsLabel]-[photosLabel]",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("V:|[topLayoutGuide][imageView]|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.Add (NSLayoutConstraint.Create (ImageView, NSLayoutAttribute.Width, NSLayoutRelation.Equal,
            View, NSLayoutAttribute.Width, 0.5f, 0.0f));
    } else {
        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("|[imageView]|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("|-[nameLabel]-|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("|-[conversationsLabel]-|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("|-[photosLabel]-|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("V:[topLayoutGuide]-[nameLabel]-[conversationsLabel]-[photosLabel]-20-[imageView]|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));
    }

    if (constraints != null)
        View.RemoveConstraints (constraints.ToArray ());

    constraints = newConstraints;
    View.AddConstraints (constraints.ToArray ());
}
```

### <a name="adding-transition-animations"></a>切り替え効果アニメーションの追加

アダプティブフォトアプリケーションの分割ビューコントローラーが折りたたまれていない状態になったときに、ビューコントローラーの `WillTransitionToTraitCollection` メソッドをオーバーライドすることにより、アニメーションが既定のアニメーションに追加されます。 例 :

```csharp
public override void WillTransitionToTraitCollection (UITraitCollection traitCollection, IUIViewControllerTransitionCoordinator coordinator)
{
    base.WillTransitionToTraitCollection (traitCollection, coordinator);
    coordinator.AnimateAlongsideTransition ((UIViewControllerTransitionCoordinatorContext) => {
        UpdateConstraintsForTraitCollection (traitCollection);
        View.SetNeedsLayout ();
    }, (UIViewControllerTransitionCoordinatorContext) => {
    });
}
```

### <a name="overriding-the-trait-environment"></a>特徴環境のオーバーライド

前述のように、アダプティブフォトアプリケーションでは、iPhone デバイスが横ビューにあるときに、分割ビューコントローラーに詳細とマスタービューの両方を強制的に表示させます。

これは、ビューコントローラーで次のコードを使用して実現されました。

```csharp
private UITraitCollection forcedTraitCollection = new UITraitCollection ();
...

public UITraitCollection ForcedTraitCollection {
    get {
        return forcedTraitCollection;
    }

    set {
        if (value != forcedTraitCollection) {
            forcedTraitCollection = value;
            UpdateForcedTraitCollection ();
        }
    }
}
...

public override void ViewWillTransitionToSize (SizeF toSize, IUIViewControllerTransitionCoordinator coordinator)
{
    ForcedTraitCollection = toSize.Width > 320.0f ?
         UITraitCollection.FromHorizontalSizeClass (UIUserInterfaceSizeClass.Regular) :
         new UITraitCollection ();

    base.ViewWillTransitionToSize (toSize, coordinator);
}

public void UpdateForcedTraitCollection ()
{
    SetOverrideTraitCollection (forcedTraitCollection, viewController);
}
```

### <a name="expanding-and-collapsing-the-split-view-controller"></a>分割ビューコントローラーの展開と折りたたみ

次に、分割ビューコントローラーの展開と折りたたみの動作が Xamarin でどのように実装されたかを見てみましょう。 `AppDelegate`では、分割ビューコントローラーが作成されると、これらの変更を処理するためのデリゲートが割り当てられます。

```csharp
public class SplitViewControllerDelegate : UISplitViewControllerDelegate
{
    public override bool CollapseSecondViewController (UISplitViewController splitViewController,
        UIViewController secondaryViewController, UIViewController primaryViewController)
    {
        AAPLPhoto photo = ((CustomViewController)secondaryViewController).Aapl_containedPhoto (null);
        if (photo == null) {
            return true;
        }

        if (primaryViewController.GetType () == typeof(CustomNavigationController)) {
            var viewControllers = new List<UIViewController> ();
            foreach (var controller in ((UINavigationController)primaryViewController).ViewControllers) {
                var type = controller.GetType ();
                MethodInfo method = type.GetMethod ("Aapl_containsPhoto");

                if ((bool)method.Invoke (controller, new object[] { null })) {
                    viewControllers.Add (controller);
                }
            }

            ((UINavigationController)primaryViewController).ViewControllers = viewControllers.ToArray<UIViewController> ();
        }

        return false;
    }

    public override UIViewController SeparateSecondaryViewController (UISplitViewController splitViewController,
        UIViewController primaryViewController)
    {
        if (primaryViewController.GetType () == typeof(CustomNavigationController)) {
            foreach (var controller in ((CustomNavigationController)primaryViewController).ViewControllers) {
                var type = controller.GetType ();
                MethodInfo method = type.GetMethod ("Aapl_containedPhoto");

                if (method.Invoke (controller, new object[] { null }) != null) {
                    return null;
                }
            }
        }

        return new AAPLEmptyViewController ();
    }
}
```

`SeparateSecondaryViewController` メソッドは、写真が表示されているかどうかをテストし、その状態に基づいてアクションを実行します。 写真が表示されていない場合は、マスタービューコントローラーが表示されるように、セカンダリビューコントローラーを折りたたみます。

`CollapseSecondViewController` メソッドは、分割ビューコントローラーを展開するときに使用されます。これにより、スタック上に写真が存在するかどうかが確認され、そのビューに戻ります。

### <a name="moving-between-view-controllers"></a>ビューコントローラー間の移動

次に、Adaptive Photos アプリケーションがビューコントローラー間をどのように移動するかを見てみましょう。 ユーザーがテーブルからセルを選択すると `AAPLConversationViewController` クラスで、`ShowDetailViewController` メソッドが呼び出され、詳細ビューが表示されます。

```csharp
public override void RowSelected (UITableView tableView, NSIndexPath indexPath)
{
    var photo = PhotoForIndexPath (indexPath);
    var controller = new AAPLPhotoViewController ();
    controller.Photo = photo;

    int photoNumber = indexPath.Row + 1;
    int photoCount = (int)Conversation.Photos.Count;
    controller.Title = string.Format ("{0} of {1}", photoNumber, photoCount);
    ShowDetailViewController (controller, this);
}
```

### <a name="displaying-disclosure-indicators"></a>開示インジケーターの表示

アダプティブフォトアプリケーションでは、特徴環境の変更に基づいて、開示インジケーターが非表示になっているか、表示されている場所がいくつかあります。 これは、次のコードを使用して処理されます。

```csharp
public bool Aapl_willShowingViewControllerPushWithSender ()
{
    var selector = new Selector ("Aapl_willShowingViewControllerPushWithSender");
    var target = this.GetTargetViewControllerForAction (selector, this);

    if (target != null) {
        var type = target.GetType ();
        MethodInfo method = type.GetMethod ("Aapl_willShowingDetailViewControllerPushWithSender");
        return (bool)method.Invoke (target, new object[] { });
    } else {
        return false;
    }
}

public bool Aapl_willShowingDetailViewControllerPushWithSender ()
{
    var selector = new Selector ("Aapl_willShowingDetailViewControllerPushWithSender");
    var target = this.GetTargetViewControllerForAction (selector, this);

    if (target != null) {
        var type = target.GetType ();
        MethodInfo method = type.GetMethod ("Aapl_willShowingDetailViewControllerPushWithSender");
        return (bool)method.Invoke (target, new object[] { });
    } else {
        return false;
    }
}
```

これらは、上記で説明した `GetTargetViewControllerForAction` 方法を使用して実装されます。

テーブルビューコントローラーがデータを表示するときには、前に実装したメソッドを使用して、プッシュが行われるかどうか、また、それに応じて開示インジケーターを表示するかどうかを確認します。

```csharp
public override void WillDisplay (UITableView tableView, UITableViewCell cell, NSIndexPath indexPath)
{
    bool pushes = ShouldShowConversationViewForIndexPath (indexPath) ?
         Aapl_willShowingViewControllerPushWithSender () :
         Aapl_willShowingDetailViewControllerPushWithSender ();

    cell.Accessory = pushes ? UITableViewCellAccessory.DisclosureIndicator : UITableViewCellAccessory.None;
    var conversation = ConversationForIndexPath (indexPath);
    cell.TextLabel.Text = conversation.Name;
}
```

### <a name="new-showdetailtargetdidchangenotification-type"></a>新しい `ShowDetailTargetDidChangeNotification` の種類

Apple では、分割ビューコントローラーの `ShowDetailTargetDidChangeNotification`から、サイズのクラスと特徴の環境を操作するための新しい通知の種類が追加されました。 この通知は、コントローラーの展開時や折りたたみ時など、分割ビューコントローラーのターゲット詳細ビューが変更されるたびに送信されます。

アダプティブフォトアプリケーションは、この通知を使用して、詳細ビューコントローラーが変更されたときに、公開インジケーターの状態を更新します。

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();
    TableView.RegisterClassForCellReuse (typeof(UITableViewCell), AAPLListTableViewControllerCellIdentifier);
    NSNotificationCenter.DefaultCenter.AddObserver (this, new Selector ("showDetailTargetDidChange:"),
        UIViewController.ShowDetailTargetDidChangeNotification, null);
    ClearsSelectionOnViewWillAppear = false;
}
```

アダプティブフォトアプリケーションについて詳しく見ていきましょう。サイズのクラス、特徴コレクション、およびアダプティブビューコントローラーを使用して、Xamarin の統合アプリケーションを簡単に作成する方法をすべて確認できます。

## <a name="unified-storyboards"></a>統合ストーリーボード

IOS 8 を初めて使用する場合、統合されたストーリーボードを使用すると、複数のサイズクラスを対象にすることにより、iPhone と iPad の両方のデバイスに表示できる統合されたストーリーボードファイルを作成できます。 統合されたストーリーボードを使用することにより、開発者は UI 固有のコードを記述するだけで、作成と保守を行うためのインターフェイス設計は1つしかありません。

統合されたストーリーボードの主な利点は次のとおりです。

- IPhone と iPad で同じストーリーボードファイルを使用します。
- IOS 6 と iOS 7 に後方展開します。
- Xamarin iOS Designer 内から、さまざまなデバイス、向き、OS バージョンのレイアウトをプレビューします。

この機能は、で完全にサポートされてい Visual Studio for Mac

<a name="enabling-size-classes" />

### <a name="enabling-size-classes"></a>サイズクラスを有効にする

既定では、新しい Xamarin. iOS プロジェクトはすべて、クラスのサイズを変更します。 以前のプロジェクトのストーリーボード内でサイズクラスとアダプティブセグエを使用するには、まず iOS デザイナー内から Xcode 6 統合ストーリーボード形式に変換する必要があります。

これを行うには、iOS デザイナーで変換するストーリーボードを開き、[**サイズクラスを使用**する] チェックボックスをオンにします。

 [![](unified-storyboards-images/sizeclass01.png "The Use Size Classes check box")](unified-storyboards-images/sizeclass01.png#lightbox)

IOS デザイナーによって、開発者がサイズクラスを使用するようにストーリーボードの形式を変換する必要があることが確認されます。

 [![](unified-storyboards-images/sizeclass02.png "The use Size Classes alert")](unified-storyboards-images/sizeclass02.png#lightbox)

> [!IMPORTANT]
> また、サイズクラスが正常に機能するためには、自動レイアウトもチェックする必要があります。

### <a name="generic-device-types"></a>一般的なデバイスの種類

サイズクラスを使用するようにストーリーボードが変換されると、デザインサーフェイスに再表示され、デバイスが汎用になる**ようにビューが表示**されます。

 [![](unified-storyboards-images/sizeclass03.png "View as a Generic device type")](unified-storyboards-images/sizeclass03.png#lightbox)

デバイスの種類として [汎用] を選択すると、すべてのビューコントローラーのサイズが 600 x 600 二乗に変更されます。 この正方形は、任意の幅と高さのサイズを表します。 IOS Designer がこのモードのときは、すべてのサイズクラスに編集が適用されます。

開発者は、デザイン画面を iPhone として表示することもできます。

 [![](unified-storyboards-images/sizeclass04.png "Viewing the design surface as an iPhone")](unified-storyboards-images/sizeclass04.png#lightbox)

または、iPad として表示します。

 [![](unified-storyboards-images/sizeclass05.png "Viewing the design surface as an iPad")](unified-storyboards-images/sizeclass05.png#lightbox)

### <a name="select-a-size-class"></a>サイズクラスを選択してください

サイズクラスセレクターボタンはデザインサーフェイスの左上隅に表示されます ([表示] ボックスの近く)。 これにより、現在編集中のサイズクラスを開発者が選択できるようになります。

 [![](unified-storyboards-images/sizeclass06.png "Select a Size Class")](unified-storyboards-images/sizeclass06.png#lightbox)

セレクターは、サイズクラスの選択を 3 x 3 グリッドとして表示します。 グリッド内の各四角形は、幅クラスと高さクラスの組み合わせを表します。 中央の四角形では、任意の幅/高さサイズのクラス (統合されたストーリーボードの既定のビュー) を選択します。 この四角形を選択すると、開発者は既定のレイアウトを編集します。これは、他のすべての構成によって継承されます。

グリッドの左上隅にある四角形は、Compact Width/Compact Height Size クラスを表します。

 [![](unified-storyboards-images/sizeclass07.png "The Compact Width/Compact Height Size Class")](unified-storyboards-images/sizeclass07.png#lightbox)

このモードは、横長の iPhone に対応しています。 グリッドの右下隅にある四角形は、iPad を表す通常の幅/標準の高さサイズクラスを表します。

 [![](unified-storyboards-images/sizeclass08.png "The Regular Width/Regular Height Size Class")](unified-storyboards-images/sizeclass08.png#lightbox)

縦向きで iPhone のレイアウトを編集するには、左下隅にある四角形を選択します。 これは、Compact Width/Regular Height Size クラスを表します。

 [![](unified-storyboards-images/sizeclass09.png "The Compact Width/Regular Height Size Class")](unified-storyboards-images/sizeclass09.png#lightbox)

四角形をクリックして選択すると、デザインサーフェイスによってビューコントローラーのサイズが新しい選択に合わせて変更されます。

 [![](unified-storyboards-images/sizeclass10.png "The Design Surface will change the size of the View Controllers to match the new selection as shown")](unified-storyboards-images/sizeclass10.png#lightbox)

サイズクラスの詳細および iPhones と Ipad のレイアウトへの影響については、この記事の「サイズクラス」セクションを参照してください。

### <a name="adaptive-segue-types"></a>Adaptive セグエの種類

開発者が以前にストーリーボードを使用していた場合は、**プッシュ**、**モーダル**、および**segue**の既存のセグエの種類について理解しておく必要があります。 統合されたストーリーボードファイルでサイズクラスが有効になっている場合、次のアダプティブセグエの種類 (上記で説明した新しいビューコントローラー API に対応する) が使用可能になります。詳細を**表示**して**表示**する。

> [!IMPORTANT]
> サイズクラスが有効になっている場合、既存のセグエはすべて新しい型に変換されます。

マスタービューに単純なゲームナビゲーションメニューがある分割ビューコントローラーと統合されたストーリーボードを使用する iOS 8 アプリケーションの例を見てください。 ユーザーがメニューボタンをクリックすると、選択した項目のビューコントローラーが、iPad で実行されている場合、分割ビューコントローラーの詳細セクションに表示されます。 IPhone では、アイテムの View Controller をナビゲーションスタックにプッシュする必要があります。

この効果を実現するには、iOS デザイナーで [] ボタンをクリックし、表示するビューコントローラーに線をドラッグします。 マウスボタンが離されたら、[セグエ Type] ポップアップメニューから [`Show Detail`] を選択します。

 [![](unified-storyboards-images/segue01.png "Select Show Detail from the Segue Type Popup menu")](unified-storyboards-images/segue01.png#lightbox)

新しいセグエは、ボタンとビューコントローラーの間に作成されます。 次に、iPhone シミュレーターでアプリケーションを実行すると、メインメニューが表示されます。

 [![](unified-storyboards-images/segue02.png "The Main Menu")](unified-storyboards-images/segue02.png#lightbox)

[ゲームの**選択**] ボタンをクリックすると、項目の View Controller がナビゲーションスタックにプッシュされます。

 [![](unified-storyboards-images/segue03.png "The items View Controller will be pushed onto the Navigation Stack as shown")](unified-storyboards-images/segue03.png#lightbox)

IPhone シミュレーターを停止し、iPad シミュレーターでアプリケーションを実行します。 横向きに切り替えると、メインメニューが再び表示されます。

 [![](unified-storyboards-images/segue04.png "The main menu displayed")](unified-storyboards-images/segue04.png#lightbox)

ここでも、 **[ゲームの選択]** ボタンをクリックすると、アイテムの view Controller が分割ビューコントローラーの詳細セクションに表示されます。

 [![](unified-storyboards-images/segue05.png "The items View Controller shown in the Details section of the Split View Controller")](unified-storyboards-images/segue05.png#lightbox)

### <a name="excluding-an-element-from-a-size-class"></a>サイズクラスからの要素の除外

特定のサイズクラス内では、特定の要素 (ビュー、コントロール、制約など) が必要ない場合があります。 サイズクラスから要素を除外するには、**デザインサーフェイス**で除外する目的の項目を選択します。 **プロパティエクスプローラー**の一番下までスクロールし、**歯車**のドロップダウンメニューをクリックします。 項目を除外する**幅**と**高さ**の組み合わせを選択してください:

[![](unified-storyboards-images/exclude-a.png "Select the combination of Width and Height")](unified-storyboards-images/exclude-a.png#lightbox)

新しい*除外ケース*が、**プロパティエクスプローラー**の下部にある要素に追加されます。 次に、指定されたサイズクラスの **[インストール済み]** チェックボックスをオフにします。

[![](unified-storyboards-images/exclude-b.png "Uncheck the Installed checkbox")](unified-storyboards-images/exclude-b.png#lightbox)

デザインサーフェイスを、アイテムが除外された幅と高さに切り替えます。指定されたサイズクラスからは削除されていますが、UI デザイン全体は削除されていません。

 [![](unified-storyboards-images/exclude02.png "Switch the Design Surface to the Width and Height that the item was excluded from")](unified-storyboards-images/exclude02.png#lightbox)

任意の幅と高さのいずれかのサイズのクラスに切り替えて、要素がまだ配置されている場合は、次のようになります。

 [![](unified-storyboards-images/exclude03.png "Switching back to the Any Width/Any Height size class")](unified-storyboards-images/exclude03.png#lightbox)

アプリケーションが iPad シミュレーターで実行されると、要素が表示されます。

 [![](unified-storyboards-images/exclude04.png "The element shown when the running app in the iPad Simulator")](unified-storyboards-images/exclude04.png#lightbox)

また、iPhone シミュレーターでアプリケーションを実行すると、要素が不足しています。

 [![](unified-storyboards-images/exclude05.png "The element missing when the running app in the iPhone Simulator")](unified-storyboards-images/exclude05.png#lightbox)

要素から除外のケースを削除するには、**デザインサーフェイス**で要素を選択し、**プロパティエクスプローラー**の一番下までスクロールして、削除するケースの横にある [ **-** ] ボタンをクリックします。

統合されたストーリーボードの実装を確認するには、このドキュメントに添付されているサンプル Xamarin iOS 8 アプリケーションの `UnifiedStoryboard` を参照してください。

## <a name="dynamic-launch-screens"></a>動的な起動画面

アプリが実際に起動されていることをユーザーにフィードバックするために、iOS アプリケーションを起動しているときに、起動画面ファイルがスプラッシュスクリーンとして表示されます。 IOS 8 より前の開発者は、アプリケーションが実行されているデバイスの種類、向き、画面の解像度ごとに、複数の `Default.png` イメージ資産を含める必要があります。 たとえば、`Default@2x.png`、`Default-Landscape@2x~ipad.png`、`Default-Portrait@2x~ipad.png`などです。

新しい iPhone 6 および iPhone 6 Plus デバイス (および今後の Apple Watch) で、既存の iPhone と iPad デバイスをすべて使用することで、作成および管理する必要がある `Default.png` スタートアップ画面のイメージ資産のさまざまなサイズ、向き、および解像度を表すことができます。 さらに、これらのファイルは非常に大きくなる可能性があり、成果物アプリケーションバンドルを "肥大化" し、iTunes App Store からアプリケーションをダウンロードするために必要な時間を増やします (携帯ネットワークを介して配信できない場合もあります)。さらに、エンドユーザーのデバイスで必要とされるストレージの量を増やします。

IOS 8 を初めて使用する場合、開発者は、Auto Layout クラスと Size クラスを使用して、すべてのデバイス、解像度、および向きに対して機能する*動的な起動画面*を作成する、Xcode の単一のアトミック `.xib` ファイルを作成できます。 これにより、開発者が必要なすべてのイメージ資産を作成して維持するために必要な作業量を削減できるだけでなく、アプリケーションのインストール済みバンドルのサイズを大幅に削減できます。

動的起動画面には、次の制限事項と考慮事項があります。

- `UIKit` クラスのみを使用します。
- `UIView` または `UIViewController` オブジェクトである単一のルートビューを使用します。
- アプリケーションのコードには接続しないでください (**アクション**や**アウトレット**を追加しないでください)。
- `UIWebView` オブジェクトを追加しないでください。
- カスタムクラスは使用しないでください。
- ランタイム属性は使用しないでください。

上記のガイドラインを考慮して、既存の Xamarin iOS 8 プロジェクトに動的起動画面を追加する方法を見てみましょう。

次を実行します。

1. **Visual Studio for Mac**を開き、**ソリューション**を読み込んで、動的起動画面をに追加します。
2. **ソリューションエクスプローラー**で、`MainStoryboard.storyboard` ファイルを右クリックし、[ **Open With** > **Xcode Interface Builder**] を選択します。

    [![](unified-storyboards-images/dls01.png "Open With Xcode Interface Builder")](unified-storyboards-images/dls01.png#lightbox)
3. Xcode で、[**ファイル** > **新しい** > **ファイル...** ] を選択します。

    [![](unified-storyboards-images/dls02.png "Select File / New")](unified-storyboards-images/dls02.png#lightbox)
4. [ **IOS** > **ユーザーインターフェイス** > **起動画面**] を選択し、 **[次へ]** ボタンをクリックします。

    [![](unified-storyboards-images/dls03.png "Select iOS / User Interface / Launch Screen")](unified-storyboards-images/dls03.png#lightbox)
5. ファイルに `LaunchScreen.xib` という名前を指定し、 **[作成]** ボタンをクリックします。

    [![](unified-storyboards-images/dls04.png "Name the file LaunchScreen.xib")](unified-storyboards-images/dls04.png#lightbox)
6. 起動画面のデザインを編集するには、グラフィック要素を追加し、レイアウトの制約を使用して、特定のデバイス、向き、画面のサイズに合わせます。

    [![](unified-storyboards-images/dls05.png "Editing the design of the launch screen")](unified-storyboards-images/dls05.png#lightbox)
7. `LaunchScreen.xib` に対する変更を保存します。
8. [**アプリケーション] ターゲット**と **[全般]** タブを選択します。

    [![](unified-storyboards-images/dls06.png "Select the Applications Target and the General tab")](unified-storyboards-images/dls06.png#lightbox)
9. **[情報の選択]** ボタンをクリックし、Xamarin アプリの `Info.plist` を選択して、 **[選択]** ボタンをクリックします。

    [![](unified-storyboards-images/dls07.png "Select the Info.plist for the Xamarin app")](unified-storyboards-images/dls07.png#lightbox)
10. **[アプリアイコンと起動イメージ]** セクションで、 **[起動画面ファイル]** ドロップダウンを開き、上で作成した `LaunchScreen.xib` を選択します。

    [![](unified-storyboards-images/dls08.png "Choose the LaunchScreen.xib")](unified-storyboards-images/dls08.png#lightbox)
11. 変更内容をファイルに保存し、Visual Studio for Mac に戻ります。
12. Visual Studio for Mac が Xcode との変更の同期を完了するまで待ちます。
13. **ソリューションエクスプローラー**で、**リソース**フォルダーを右クリックし、 **[追加]**  >  **[ファイルの追加...]** の順に選択します。

    [![](unified-storyboards-images/dls09.png "Select Add / Add Files...")](unified-storyboards-images/dls09.png#lightbox)
14. 上で作成した `LaunchScreen.xib` ファイルを選択し、 **[開く]** ボタンをクリックします。

    [![](unified-storyboards-images/dls10.png "Select the LaunchScreen.xib file")](unified-storyboards-images/dls10.png#lightbox)
15. アプリケーションをビルドします。

### <a name="testing-the-dynamic-launch-screen"></a>動的起動画面のテスト

Visual Studio for Mac で、iPhone 4 Retina シミュレーターを選択し、アプリケーションを実行します。 動的起動画面は、正しい形式と向きで表示されます。

[![](unified-storyboards-images/dls11.png "The Dynamic Launch Screen displayed in the vertical orientation")](unified-storyboards-images/dls11.png#lightbox)

Visual Studio for Mac でアプリケーションを停止し、iPad iOS 8 デバイスを選択します。 アプリケーションを実行します。起動画面は、このデバイスと向きに対して正しく書式設定されます。

[![](unified-storyboards-images/dls12.png "The Dynamic Launch Screen displayed in the horizontal orientation")](unified-storyboards-images/dls12.png#lightbox)

Visual Studio for Mac に戻り、アプリケーションの実行を停止します。

### <a name="working-with-ios-7"></a>IOS 7 の使用

IOS 7 との下位互換性を維持するには、通常の `Default.png` イメージ資産を iOS 8 アプリケーションに通常どおりに含めます。 ios は以前の動作に戻り、iOS 7 デバイスで実行しているときにこれらのファイルをスタートアップ画面として使用します。

Xamarin で動的起動画面の実装を確認するには、このドキュメントに添付されている[動的起動](https://docs.microsoft.com/samples/xamarin/ios-samples/ios8-dynamiclaunchscreen)画面のサンプル iOS 8 アプリケーションを確認してください。

## <a name="summary"></a>要約

この記事では、サイズクラスの概要と、iPhone と iPad デバイスでのレイアウトへの影響について簡単に説明しました。 ここでは、特徴、特徴環境、特徴コレクションがサイズクラスを使用して統合インターフェイスを作成するしくみについて説明しました。 アダプティブビューコントローラーについて簡単に説明し、統合インターフェイス内でサイズクラスを使用する方法について説明しました。 ここでは、Xamarin iOS 8 アプリケーション内のコードC#から、サイズクラスと統合インターフェイスを完全に実装する方法を見てきました。

最後に、この記事では、iOS デバイス間で動作する Xamarin iOS Designer を使用して統合されたストーリーボードを作成するための基本について説明し、すべての iOS 8 デバイスで起動画面として表示される単一の動的起動画面を作成する方法について説明します。

## <a name="related-links"></a>関連リンク

- [アダプティブフォト (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios8-adaptivephotos)
- [動的な起動画面 (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios8-dynamiclaunchscreen)
- [iOS 8 の概要](~/ios/platform/introduction-to-ios8.md)
- [IOS8 での動的レイアウト-進化 2014 (ビデオ)](https://youtu.be/f3mMGlS-lM4)
- [Uiプレゼンテーションコントローラー](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIPresentationController_class/)
- [UIImageAsset](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIImageAsset_Ref/index.html#//apple_ref/occ/cl/UIImageAsset)
