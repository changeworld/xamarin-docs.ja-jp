---
title: NuGet マルチプラットフォームライブラリプロジェクト (別名 Nugetizer 3000)
description: このドキュメントでは、Nugetizer 3000 ツールを使用して、プラットフォーム間でコードを共有する NuGet パッケージを自動的に作成する方法について説明します。
ms.prod: xamarin
ms.assetid: F0A5A9BB-86CD-44C9-8EE8-74D1E5E74A30
author: davidortinau
ms.author: daortin
ms.date: 07/25/2018
ms.openlocfilehash: 5744bb9947b196ee319535729338bcf64a5cd09e
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73016763"
---
# <a name="nuget-multiplatform-library-projects-nugetizer-3000"></a>NuGet マルチプラットフォームライブラリプロジェクト (Nugetizer 3000)

_' Nugetizer 3000 ' を使用してプラットフォーム間でコードを共有する NuGet パッケージを自動的に作成します。_

_Nugetizer 3000_を使用して、プラットフォーム間でコードを共有する NuGet パッケージを自動的に作成することができます。 これにより、既存のライブラリプロジェクトから NuGet パッケージを作成したり、新しい**マルチプラットフォームライブラリプロジェクト**を作成したりすることができます。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Visual Studio for Mac には、Nugetizer 3000 が含まれており、**ライブラリ > Mulitplatform library**プロジェクトの種類を**ファイル > 新しい**ウィンドウで検索 &ndash; ます。

[![](images/mulitplatform-library-sml.png "Create new Multiplatform Library window")](images/mulitplatform-library.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual Studio で Nugetizer 3000 を使用するには、 [VSIX インストーラーをダウンロードして実行](https://bit.ly/nugetizer-2017)してください。

-----

## <a name="building-nuget-packages"></a>NuGet パッケージのビルド

構成が完了すると、プロジェクトのすべてのビルドによって完全な NuGet パッケージが出力されます。このパッケージを使用して、他のアプリと内部でコードを共有したり、 [NuGet.org](https://www.nuget.org)にアップロードしたりできます。

この機能を使用するには、次の3つのシナリオがあります。

- [既存のライブラリ プロジェクト](existing-library.md)

  既存の PCL (または .NET Standard) プロジェクトから NuGet パッケージを作成します。

- [新しいマルチプラットフォームライブラリプロジェクトの作成](single-codebase.md)

  PCL または .NET Standard を使用して、NuGet 経由で共通コードを共有する新しいライブラリを作成します。

- [新しいプラットフォーム固有のライブラリプロジェクトの作成](platform-specific.md)

  IOS および Android 用のプラットフォーム固有のコードを含む新しいライブラリと NuGet を作成し、共有プロジェクトを使用して、iOS または Android 固有の機能をサポートするための共通コードおよびプラットフォーム固有のプロジェクトを含めます。

任意の NuGet パッケージに追加する必要がある必須およびオプションのメタデータの詳細については、[メタデータガイド](metadata.md)を参照してください。

## <a name="further-nuget-information"></a>NuGet の詳細情報

詳細については、 [Xamarin 用に nuget を手動で作成](~/cross-platform/app-fundamentals/nuget-manual.md)する方法と、[アプリに NuGet パッケージを含める](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)方法に関する説明を参照してください。

Microsoft の[NuGet ドキュメント](https://docs.microsoft.com/nuget/)には、Visual Studio での**nupkg**形式と NuGet パッケージの使用に関する詳細情報が記載されています。

NuGet パッケージプロジェクトの設計に関する説明 (別名) NuGetizer 3000) は、 [NuGet GitHub リポジトリ](https://github.com/NuGet/Home/wiki/NuGetizer-3000)で入手できます。

## <a name="related-links"></a>関連リンク

- [NuGetizer-3000 ユースケース](https://github.com/NuGet/Home/wiki/NuGetizer-Core-Scenarios)
- [Xamarin の NuGet パッケージを手動で作成する](~/cross-platform/app-fundamentals/nuget-manual.md)
- [NuGet のドキュメント](https://docs.microsoft.com/nuget/)
