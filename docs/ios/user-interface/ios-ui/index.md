---
title: iOS のユーザー インターフェイス
description: このドキュメントでは、Xamarin iOS アプリでユーザーインターフェイスを構築する方法について説明しているガイドにリンクしています。 リンク先のガイドでは、外観 API、ユーザーインターフェイスオブジェクトの作成、レイアウトオプションなどについて説明します。
ms.prod: xamarin
ms.assetid: 1BB46561-F503-491E-A27C-7878E7EBE00B
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/14/2017
ms.openlocfilehash: 325898b3c934e25ae1610a3437f787476dcd22cb
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73003340"
---
# <a name="user-interfaces-in-ios"></a>iOS のユーザー インターフェイス

## <a name="appearance-apiintroduction-to-the-appearance-apimd"></a>[外観 API](introduction-to-the-appearance-api.md)

iOS では、ユーザーインターフェイスコントロールの多くの視覚属性を、UIAppearance Api を使用してテーマ設定できます。

## <a name="creating-user-interface-objectsiosuser-interfaceios-uicreating-ui-objectsmd"></a>[ユーザー インターフェイス オブジェクトの作成](~/ios/user-interface/ios-ui/creating-ui-objects.md)

Apple は、関連する機能の一部を "フレームワーク" にグループ化します。これは、Xamarin の iOS 名前空間に相当します。 `UIKit` は、iOS のすべてのユーザーインターフェイスコントロールを含む名前空間です。

## <a name="layout-optionsiosuser-interfaceios-uilayout-optionsmd"></a>[レイアウト オプション](~/ios/user-interface/ios-ui/layout-options.md)

ビューのサイズ変更時または回転時にレイアウトを制御するには、自動サイズ調整と自動レイアウトの2つの異なるメカニズムがあります。

## <a name="providing-haptic-feedbackiosuser-interfaceios-uihaptic-feedbackmd"></a>[Haptic フィードバックの提供](~/ios/user-interface/ios-ui/haptic-feedback.md)

この記事では、iOS 10 で利用可能な新しい種類の haptic フィードバックと、それらを Xamarin. iOS に実装する方法について説明します。

## <a name="working-with-the-ui-threadiosuser-interfaceios-uiui-threadmd"></a>[UI スレッドの操作](~/ios/user-interface/ios-ui/ui-thread.md)

コードでは、メイン (または UI) スレッドからのユーザーインターフェイスコントロールに対してのみ変更を行う必要があります。 別のスレッド (コールバックやバックグラウンドスレッドなど) で発生した UI の更新が画面に表示されない場合や、クラッシュが発生する場合もあります。
