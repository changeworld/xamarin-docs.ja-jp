---
title: バージョン情報とログはどこにありますか
description: このドキュメントでは、Xamarin のバージョン情報とログを検索する場所について説明します。 この情報は、問題を診断したり、バグを報告したり、サポートを受けたりするときに役立ちます。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: CF386485-EAB0-4B9E-AA17-CB1B6462E505
author: davidortinau
ms.author: daortin
ms.date: 03/29/2017
ms.openlocfilehash: 68de58f499788d803aa0af6c68f20e2265b1d6b5
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/13/2020
ms.locfileid: "79305823"
---
# <a name="where-can-i-find-my-version-information-and-logs"></a>バージョン情報とログはどこにありますか

## <a name="outline"></a>外枠

- [バージョン情報](#version-information)
  - Windows のバージョン情報
  - Mac のバージョン情報
  - Android SDK Tools, プラットフォームツール, ビルドツール
- [IDE およびインストーラーのログ](#ide-and-installer-logs)
  - [Windows ログ](#windows-logs)
    - Xamarin Studio
    - Xamarin for Visual Studio
    - Xamarin Universal installer
    - 個々の `.msi` インストーラー、詳細ログ
    - Visual Studio の起動、詳細ログ
  - [Mac ログ](#mac-logs)
    - ビルドホスト
  - Visual Studio for Mac
    - Xamarin Studio
    - Xamarin インストーラー
- [詳細なビルド出力](#verbose-build-output-logs)
- [Xamarin Android および Xamarin iOS アプリのデバッグログ](#debug-logs-for-xamarin-apps)
  - Android `adb` logcat ログ
  - iOS シミュレーターログ (Mac)
  - iOS デバイスログ (Mac)

## <a name="a-idversion-information-nameversion-information-version-information"></a>バージョン情報の <a id="version-information" name="version-information" />

通常は、 **[情報のコピー]** ボタンからすべての情報を送り返すことをお勧めします。 それ以外の場合は、多くの場合、追加情報を要求する必要があります。 たとえば、オペレーティングシステムのバージョン、Xcode のバージョン、インストールされている Android API レベル、.NET バージョンは、問題のトラブルシューティングを支援する際に重要になります。

### <a name="a-idwindows-version-information-namewindows-version-information-windows-version-information"></a>Windows のバージョン情報を <a id="windows-version-information" name="windows-version-information" />する

#### <a name="xamarin-studio"></a>Xamarin Studio

**情報をコピー > > に関するヘルプ > [ボタン]**

#### <a name="visual-studio"></a>Visual Studio

**Microsoft Visual Studio > コピー情報のヘルプ > [ボタン]**

### <a name="a-idmac-version-information-namemac-version-information-mac-version-information"></a><a id="mac-version-information" name="mac-version-information" />Mac のバージョン情報

#### <a name="visual-studio-for-mac"></a>Visual Studio for Mac

**Visual studio について > Visual Studio について > 詳細の表示 > 情報のコピー [button]**

### <a name="a-idandroid-sdk-tools-versions-nameandroid-sdk-tools-versions-android-sdk-tools-platform-tools-build-tools"></a><a id="android-sdk-tools-versions" name="android-sdk-tools-versions" />Android SDK Tools、プラットフォームツール、ビルドツール

Android SDK マネージャーを開き、上部の **[ツール]** セクションのスクリーンショットを取得します。

#### <a name="visual-studio-for-mac"></a>Visual Studio for Mac

**ツール > 開いて Android SDK マネージャー**

#### <a name="visual-studio"></a>Visual Studio

**ツール > Android > Android SDK マネージャーを開く...**

## <a name="a-idide-and-installer-logs-nameide-and-installer-logs-ide-and-installer-logs"></a>IDE およびインストーラーのログの <a id="ide-and-installer-logs" name="ide-and-installer-logs" />

ログの場所ごとに、ログフォルダー全体を圧縮して添付します。

### <a name="a-idwindows-logs-namewindows-logs-windows-logs"></a>Windows ログの <a id="windows-logs" name="windows-logs" />

#### <a name="a-idwindows-logs-xamarin-vs-namewindows-logs-xamarin-vs--visual-studio-tools-for-xamarin"></a>Xamarin の <a id="windows-logs-xamarin-vs" name="windows-logs-xamarin-vs" /> Visual Studio Tools

`%LOCALAPPDATA%\Xamarin\Logs`

#### <a name="a-idvs-2017-namevs-2017--visual-studio-2017"></a><a id="vs-2017" name="vs-2017" /> Visual Studio 2017

[Visual Studio のインストールログを取得する方法](https://docs.microsoft.com/visualstudio/install/troubleshooting-installation-issues#how-to-get-the-visual-studio-installation-logs)

#### <a name="a-idvs-2015-namevs-2015--visual-studio-2015"></a><a id="vs-2015" name="vs-2015" /> Visual Studio 2015

#### <a name="a-idwindows-universal-installer-namewindows-universal-installer--xamarin-universal-installer"></a>Xamarin "Universal" インストーラーの <a id="windows-universal-installer" name="windows-universal-installer" />

`%LOCALAPPDATA%\Xamarin\Universal`

これらは、`XamarinInstaller.exe` インストーラーのログです。

#### <a name="a-idindividual-msi-installers-verbose-logs-nameindividual-msi-installers-verbose-logs-individual-msi-installers-verbose-logs"></a>個々の `.msi` インストーラー、詳細ログを <a id="individual-msi-installers-verbose-logs" name="individual-msi-installers-verbose-logs" />する

```csharp
msiexec /i Xamarin.msi /l*vx "%USERPROFILE%\Desktop\Xamarin.log"
```

参照:[コマンドラインオプション](https://msdn.microsoft.com/library/aa367988.aspx)

#### <a name="a-idvisual-studio-startup-verbose-logs-namevisual-studio-startup-verbose-logs-visual-studio-startup-verbose-logs"></a>Visual Studio の起動、詳細ログの <a id="visual-studio-startup-verbose-logs" name="visual-studio-startup-verbose-logs" />

```csharp
devenv.exe /log "%USERPROFILE%\Desktop\VisualStudio.log"
```

参照: [/log (devenv.exe)](https://msdn.microsoft.com/library/ms241272.aspx)

### <a name="a-idmac-logs-namemac-logs-mac-logs"></a><a id="mac-logs" name="mac-logs" />Mac ログ

Finder の **[フォルダーに >]** メニュー項目を選択して、これらのパスのいずれかをコピーしてダイアログに貼り付けることができます。

#### <a name="a-idmac-logs-visual-studio-namemac-logs-visual-studio-visual-studio-for-mac"></a><a id="mac-logs-visual-studio" name="mac-logs-visual-studio" />Visual Studio for Mac

`~/Library/Logs/VisualStudio/7.0` (この数値は使用しているバージョンによって変わる可能性があります)

このフォルダーは、[ヘルプ-> 開いているログディレクトリ] を使用して開くこともできます。

#### <a name="a-idmac-logs-xamarin-studio-namemac-logs-xamarin-studio-xamarin-studio"></a><a id="mac-logs-xamarin-studio" name="mac-logs-xamarin-studio" />Xamarin Studio

`~/Library/Logs/XamarinStudio-6.0` (この数値は使用しているバージョンによって変わる可能性があります)

このフォルダーは、[ヘルプ-> 開いているログディレクトリ] を使用して開くこともできます。

#### <a name="a-idmac-universal-installer-namemac-universal-installer-xamarin-universal-installer"></a>Xamarin "Universal" インストーラーの <a id="mac-universal-installer" name="mac-universal-installer" />

`~/Library/Logs/XamarinInstaller/Universal`

これらは、`XamarinInstaller.dmg` インストーラーのログです。

#### <a name="a-idmac-build-host-namemac-build-host-xamarin-build-host"></a>Xamarin ビルドホストの <a id="mac-build-host" name="mac-build-host" />

`~/Library/Logs/Xamarin-[MAJOR.MINOR]`

## <a name="a-idverbose-build-output-logs-nameverbose-build-output-logs-verbose-build-output"></a><a id="verbose-build-output-logs" name="verbose-build-output-logs" />の詳細なビルド出力

1. [診断 MSBuild の出力](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output)を有効にします。

2. IOS アプリの場合は、[**プロジェクトのプロパティ] > [Ios ビルド > 全般 (タブ >)** ] の下に `-v -v -v -v` を追加し、追加の mtouch 引数 > 追加のオプションを追加して、詳細な**mtouch 出力**を有効にすることもできます。

3. プロジェクトをクリーンし、リビルドします。

4. IDE からのビルド出力をコピーし、テキストファイルに貼り付けます。
     - Visual Studio (Windows): **> 出力を表示し、出力を表示 >: ビルド**
     - Visual Studio for Mac: **> パッド > エラー > ビルド出力を表示する (タブ)**

## <a name="a-iddebug-logs-for-xamarin-apps-namedebug-logs-for-xamarin-apps-debug-logs-for-xamarinandroid-and-xamarinios-apps"></a>Xamarin Android および Xamarin iOS アプリのデバッグログを <a id="debug-logs-for-xamarin-apps" name="debug-logs-for-xamarin-apps" />する

### <a name="visual-studio-for-mac"></a>Visual Studio for Mac

**アプリケーション出力 > > パッドを表示する**

(このメニュー項目は、アプリが起動された後にのみ表示されることに注意してください)。

### <a name="visual-studio"></a>Visual Studio

**出力の表示 > > 出力の表示: デバッグ**

### <a name="a-idadb-logcat-nameadb-logcat-android-adb-logcat-logs"></a>Android [`adb`](https://developer.android.com/tools/help/adb.html) logcat ログの <a id="adb-logcat" name="adb-logcat" />

`adb` コマンドを実行した後、デスクトップから**android_logcat .txt**ファイルを添付して戻します。 この手順では、デバイスが1つだけ接続されていることを前提とします。

[Android のデバッグログ](~/android/deploy-test/debugging/android-debug-log.md)に関するページも参照してください。

#### <a name="visual-studio"></a>Visual Studio

1. **ツール > android > Android Adb コマンドプロンプトを開始する**
2. ログを消去します: `adb logcat -c`
3. 問題を再現します。
4. ログの出力: `adb logcat -vtime -d > "%USERPROFILE%\Desktop\android_logcat.txt"`

#### <a name="visual-studio-for-mac"></a>Visual Studio for Mac

1. **ツール > Android SDK コマンドプロンプトを開く**
2. ログを消去します: `adb logcat -c`
3. 問題を再現します。
4. ログの出力: `adb logcat -vtime -d > ~/Desktop/android_logcat.txt`

### <a name="a-idios-simulator-logs-nameios-simulator-logs-ios-simulator-logs-on-mac"></a>iOS シミュレーターログの <a id="ios-simulator-logs" name="ios-simulator-logs" />(Mac)

- システムログにアクセスするには、iOS シミュレーターアプリで **デバッグ > システムログを開く** を選択します。

- シミュレーターからクラッシュレポートを表示するには、[アプリ] を開き、`~/Library/Logs > DiagnosticReports`に移動します。

### <a name="a-idios-device-logs-nameios-device-logs-ios-device-logs-on-mac"></a><a id="ios-device-logs" name="ios-device-logs" />iOS デバイスログ (Mac)

#### <a name="visual-studio-for-mac"></a>Visual Studio for Mac

**IOS デバイスログ > > パッドを表示する**

#### <a name="xcode"></a>Xcode

**ウィンドウ > デバイス > $ {DeviceName}**

クラッシュレポートは、 **[デバイスログの表示]** ボタンで使用できます。 デバイスのシステムログは、ウィンドウの下部の [公開] 矢印の下に表示されます。

#### <a name="xcode-5"></a>Xcode 5

**ウィンドウ > オーガナイザー > デバイス (タブ) > $ {DeviceName}**
