---
title: Xamarin.Mac 拡張機能のサポート
description: このドキュメントでは、Xamarin. Mac での Finder、共有、および今日の拡張機能のサポートについて説明します。 制限事項と既知の問題、チュートリアルとサンプルアプリへのリンク、および拡張機能を使用するためのヒントについて説明します。
ms.prod: xamarin
ms.assetid: 4148F1BE-DFA0-46B6-9FCD-425A6541F510
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: 9a9dbb63b78b00a9bcac9d7833530da02890afc6
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73017299"
---
# <a name="xamarinmac-extension-support"></a>Xamarin.Mac 拡張機能のサポート

2\.10 Xamarin では、複数の macOS 拡張ポイントにサポートが追加されました。

- 周り
- 共有
- 今日

<a name="Limitations-and-Known-Issues" />

## <a name="limitations-and-known-issues"></a>制限事項と既知の問題

次に、Xamarin. Mac で拡張機能を開発するときに発生する可能性がある制限事項と既知の問題について説明します。

- 現在、Visual Studio for Mac ではデバッグはサポートされていません。 すべてのデバッグは、 **Nslog**と**コンソール**を使用して行う必要があります。 詳細については、以下のヒントセクションを参照してください。
- 拡張機能は、ホストアプリケーションに含まれている必要があります。これは、システムに登録して一度実行する場合に使用します。 その後、**システム環境設定**の**拡張**セクションで有効にする必要があります。 
- 拡張機能のクラッシュによっては、ホストアプリケーションが不安定になり、予期しない動作が発生することがあります。 特に、**通知センター**の**Finder**と**今日**のセクションが "詰まっている" 状態になり、応答しなくなる場合があります。 これは、Xcode の拡張プロジェクトでも経験があり、現在は Xamarin. Mac とは無関係に表示されます。 多くの場合、これはシステムログに表示されることがあります (**コンソール**で、詳細については「ヒント」を参照してください)。繰り返しのエラーメッセージを出力します。 MacOS を再起動すると、この問題が解決します。

<a name="Tips" />

## <a name="tips"></a>ヒント

次のヒントは、Xamarin. Mac で拡張機能を使用する場合に役立ちます。

- 現在、Xamarin. Mac では拡張機能のデバッグがサポートされていないため、デバッグエクスペリエンスは主に実行と `printf` のようなステートメントに依存します。 ただし、拡張機能はサンドボックスプロセスで実行されるため、`Console.WriteLine` は他の Xamarin. Mac アプリケーションと同様に動作しません。 [`NSLog` を直接](https://gist.github.com/chamons/e2e409013a449cfbe1f2fbe5547f6554)呼び出すと、デバッグメッセージがシステムログに出力されます。
- キャッチされていない例外が発生すると、拡張プロセスがクラッシュし、**システムログ**に表示される情報の量がわずかになります。 再スローする前に `NSLog` する `try/catch` (例外) ブロックで、厄介なコードをラップすることが役に立つ場合があります。
- **システムログ**には、**コンソール**アプリの [**アプリケーション** > **ユーティリティ**] からアクセスできます。

    [![](extensions-images/extension02.png "The system log")](extensions-images/extension02.png#lightbox)
- 前述のように、拡張機能ホストアプリケーションを実行すると、システムに登録されます。 登録を解除してアプリケーションバンドルを削除しています。 
- アプリの拡張機能の "存在しない" バージョンが登録されている場合は、次のコマンドを使用して検索します (削除することもできます)。 `plugin kit -mv`

<a name="Walkthrough-and-Sample-App" />

## <a name="walkthrough-and-sample-app"></a>チュートリアルとサンプルアプリ

開発者は、xamarin. iOS 拡張機能と同じ方法で Xamarin. Mac 拡張機能を作成して操作します。詳細については、[拡張機能の概要](~/ios/platform/extensions.md)に関するドキュメントを参照してください。

各拡張機能の種類の小規模で実用的なサンプルを含む Xamarin. Mac プロジェクトの例については、[こちら](https://docs.microsoft.com/samples/xamarin/mac-samples/extensionsamples)を参照してください。

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事では、Xamarin. Mac バージョン 2.10 (およびそれ以降) のアプリで拡張機能を使用する方法について簡単に説明しました。

## <a name="related-links"></a>関連リンク

- [Hello Mac](~/mac/get-started/hello-mac.md)
- [ExtensionSamples](https://docs.microsoft.com/samples/xamarin/mac-samples/extensionsamples)
- [macOS ヒューマン インターフェイス ガイドライン](https://developer.apple.com/design/human-interface-guidelines/macos/overview/themes/)
