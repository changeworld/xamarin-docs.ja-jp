---
title: Android プラットフォーム機能
description: この記事では、Android 固有の機能を Xamarin Forms アプリケーションに追加する方法について説明します。
ms.prod: xamarin
ms.assetid: E24168F3-0138-4814-86EA-B467F6B8A545
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/11/2019
ms.openlocfilehash: 7ad7349c89913129cccdd77ac843188cbe668571
ms.sourcegitcommit: 10b4d7952d78f20f753372c53af6feb16918555c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/26/2020
ms.locfileid: "77635541"
---
# <a name="android-platform-features"></a>Android プラットフォーム機能

Android 用の Xamarin. Forms アプリケーションを開発するには、Visual Studio が必要です。 [[サポートされているプラットフォーム] ページ](~/get-started/supported-platforms.md)には、前提条件に関する詳細情報が表示されます。

## <a name="platform-specifics"></a>プラットフォーム固有設定

プラットフォーム仕様はカスタム レンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ利用できる機能の使用を可能にします。

Android では、次のプラットフォーム固有の機能が Xamarin のビュー、ページ、およびレイアウト用に用意されています。

- 描画の順番を決定するための視覚要素の Z オーダーの制御。 詳細については、「 [Android での Visualelement の昇格](visualelement-elevation.md)」を参照してください。
- サポートされている[`VisualElement`](xref:Xamarin.Forms.VisualElement)でレガシカラーモードを無効にする。 詳細については、「 [Android での Visualelement のレガシカラーモード](legacy-color-mode.md)」を参照してください。

Android の Xamarin ビューでは、次のプラットフォーム固有の機能が用意されています。

- 既定のパディングと Android のボタンの影の値を使用します。 詳細については、「 [Android でのボタンの余白と影](button-padding-shadow.md)」を参照してください。
- [`Entry`](xref:Xamarin.Forms.Entry)のソフトキーボードの Input Method Editor オプションを設定します。 詳細については、「 [Android の入力方式エディターオプション](entry-ime-options.md)」を参照してください。
- `ImageButton`でのドロップシャドウの有効化。 詳細については、「 [Android での ImageButton ドロップシャドウ](imagebutton-drop-shadow.md)」を参照してください。
- [`ListView`](xref:Xamarin.Forms.ListView)での高速スクロールを有効にする詳細については、「 [ListView Fast スクロール (Android](listview-fast-scrolling.md))」を参照してください。
- `SwipeView`を開くときに使用される遷移を制御します。 詳細については、「 [SwipeView スワイプ切り替えモード](swipeview-swipetransitionmode.md)」を参照してください。
- [`WebView`](xref:Xamarin.Forms.WebView)が混合コンテンツを表示できるかどうかを制御します。 詳細については、「 [Android での WebView 混合コンテンツ](webview-mixed-content.md)」を参照してください。
- [`WebView`](xref:Xamarin.Forms.WebView)のズームを有効にする。 詳細については、「 [WebView Zoom On Android](webview-zoom-controls.md)」を参照してください。

Android 上の Xamarin. フォームセルには、次のプラットフォーム固有の機能が用意されています。

- [`ListView`](xref:Xamarin.Forms.ListView)内で選択された項目が変更されたときにコンテキストアクションメニューが更新されないように[`ViewCell`](xref:Xamarin.Forms.ViewCell)コンテキストアクションのレガシモードを有効にします。 詳細については、「 [Viewcell コンテキストアクション (Android](viewcell-context-actions.md))」を参照してください。

Android では、次のプラットフォーム固有の機能が Xamarin. Forms ページ用に用意されています。

- [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)のナビゲーションバーの高さを設定します。 詳細については、「 [Android での Navigationpage バーの高さ](navigationpage-bar-height.md)」を参照してください。
- [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)内のページ間を移動するときに切り替え効果アニメーションを無効にする。 詳細については、「 [Android での TabbedPage ページ切り替えアニメーション](tabbedpage-transition-animations.md)」を参照してください。
- [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)内のページ間をスワイプできるようにします。 詳細については、「 [TabbedPage Page スワイプ On Android](tabbedpage-page-swiping.md)」を参照してください。
- [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)上のツールバーの配置と色を設定します。 詳細については、「 [TabbedPage Toolbar Placement And Color On Android](tabbedpage-toolbar-placement-color.md)」を参照してください。

Android 上の Xamarin. Forms [`Application`](xref:Xamarin.Forms.Application)クラスには、次のプラットフォーム固有の機能が用意されています。

- ソフトキーボードの操作モードの設定。 詳細については、「 [Android でのソフトキーボード入力モード](soft-keyboard-input-mode.md)」を参照してください。
- AppCompat を使用するアプリケーションについて、 [`Disappearing`](xref:Xamarin.Forms.Page.Appearing)および[`Appearing`](xref:Xamarin.Forms.Page.Appearing)ページライフサイクルイベントをそれぞれ一時停止と再開で無効にする。 詳細については、「 [Android でのページライフサイクルイベント](page-lifecycle-events.md)」を参照してください。

## <a name="platform-support"></a>プラットフォームのサポート

もともと、既定の Xamarin. Forms Android プロジェクトでは、Android 5.0 より前に共通だった従来のコントロールレンダリングが使用されていました。 テンプレートを使用してビルドされたアプリケーションは、メインアクティビティの基本クラスとして `FormsApplicationActivity` ます。

## <a name="material-design-via-appcompat"></a>AppCompat によるマテリアル設計

Xamarin. Forms Android プロジェクトでは、メインアクティビティの基本クラスとして `FormsAppCompatActivity` が使用されるようになりました。 このクラスは、Android によって提供される**AppCompat**機能を使用して、マテリアルデザインテーマを実装します。

マテリアルデザインのテーマを Xamarin. Forms Android プロジェクトに追加するには、 [AppCompat サポートのインストール手順](appcompat-material-design.md)に従ってください。

次に示すのは、既定の `FormsApplicationActivity`を使用した**Todo**サンプルです。

[![](images/before-appcompat-sml.png "Todo Sample Application Without AppCompat")](images/before-appcompat.png#lightbox "Todo Sample Application Without AppCompat")

これは、`FormsAppCompatActivity` を使用するためにプロジェクトをアップグレードした後のコードと同じです (追加のテーマ情報を追加します)。

[![](images/post-appcompat-sml.png "Todo Sample Application With AppCompat and Theming")](images/post-appcompat.png#lightbox "Todo Sample Application With AppCompat and Theming")

> [!NOTE]
> `FormsAppCompatActivity`を使用する場合、[一部の Android カスタムレンダラーの基本クラス](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md)は異なります。

## <a name="androidx-migration"></a>AndroidX の移行

AndroidX は、Android サポートライブラリを置き換えます。 AndroidX と、AndroidX ライブラリを使用するために Xamarin. Forms アプリを移行する方法については、「 [xamarin. forms での Androidx の移行](~/xamarin-forms/platform/android/androidx-migration.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [マテリアルデザインサポートの追加](appcompat-material-design.md)
