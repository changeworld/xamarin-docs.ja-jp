---
title: Android Device Manager による仮想デバイスの管理
description: この記事では、Android Device Manager を使って、Android の物理デバイスをエミュレートする Android 仮想デバイス (AVD) を作成し、構成する方法を説明します。 仮想デバイスを使うと、物理デバイスがなくてもアプリを実行してテストすることができます。
ms.prod: xamarin
ms.assetid: ECB327F3-FF1C-45CC-9FA6-9C11032BD5EF
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/22/2018
ms.openlocfilehash: a7c1aeafd94d7e2639617cda13312ee8a09e2c94
ms.sourcegitcommit: 26033c087f49873243751deded8037d2da701655
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/25/2018
ms.locfileid: "36935329"
---
# <a name="managing-virtual-devices-with-the-android-device-manager"></a>Android Device Manager による仮想デバイスの管理

_この記事では、Android Device Manager を使って、Android の物理デバイスをエミュレートする Android 仮想デバイス (AVD) を作成し、構成する方法を説明します。仮想デバイスを使うと、物理デバイスがなくてもアプリを実行してテストすることができます。_

## <a name="overview"></a>概要

ハードウェアの高速化が有効になっていることを確認した後は (「[エミュレーター パフォーマンスのためのハードウェア高速化](~/android/get-started/installation/android-emulator/hardware-acceleration.md)」を参照)、_Android Device Manager_ (_Xamarin Android Device Manager_ ともいう) を使って、アプリのテストとデバッグで使用できる仮想デバイスを作成します。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

この記事では、Android Device Manager を使って、Android 仮想デバイスを作成、複製、カスタマイズ、起動する方法について説明します。

[![Android Device Manager の [Devices]\(デバイス\) タブのスクリーンショット](device-manager-images/win/01-devices-dialog-sml.png)](device-manager-images/win/01-devices-dialog.png#lightbox)

[Android Emulator](~/android/deploy-test/debugging/debug-on-emulator.md) で実行する _Android 仮想デバイス_ (AVD) を作成および構成するには、Android Device Manager を使います。
各 AVD は、物理的な Android デバイスをシミュレートするエミュレーター構成です。 これにより、異なる物理 Android デバイスをシミュレートするさまざまな構成でアプリを実行してテストすることができます。

## <a name="requirements"></a>必要条件

Android Device Manager を使うには、次のものが必要です。

-   Visual Studio 2017 バージョン 15.7 以降。 Visual Studio Community、Professional、Enterprise Edition がサポートされています。

-   Visual Studio Tools for Xamarin バージョン 4.9 以降。

-   Android SDK をインストールし (「[Xamarin.Android 向け Android SDK を設定する](~/android/get-started/installation/android-sdk.md)」を参照)、次のセクションの説明のとおり、SDK Tools バージョン 26.1.1 以降をインストールする必要があります。 次の場所に Android SDK をインストールします (まだインストールされていない場合): **C:\\Program Files (x86)\\Android\\android-sdk**


## <a name="launching-the-device-manager"></a>Device Manager の起動

**[ツール]、[Android]、[Android Device Manager]** の順にクリックし、**[ツール]** メニューから Android Device Manager を起動します。

[![[ツール] メニューから起動します](device-manager-images/win/04-tools-menu-sml.png)](device-manager-images/win/04-tools-menu.png#lightbox)

起動時に次のエラー ダイアログが表示される場合は、「[Android Emulator のトラブルシューティング](~/android/get-started/installation/android-emulator/troubleshooting.md)」の回避策の説明を参照してください。

![Android SDK インスタンスのエラー](device-manager-images/win/32-sdk-error.png)

Android Device Manager を使う前に、Android SDK Tools バージョン 26.1.1 以降をインストールする必要があります。 Android SDK Tools 26.1.1 以降がインストールされていない場合、起動時に次のエラー ダイアログが表示されることがあります。

![Android SDK インスタンスのエラー ダイアログ](device-manager-images/win/02-sdk-instance-error.png)

このエラー ダイアログが表示された場合は、**[SDK マネージャーを開く]** をクリックして Android SDK Manager を開きます。 Android SDK Manager で、**[ツール]** タブをクリックして、以下のものをインストールします。

-   **Android SDK Tools 26.1.1** 以降 
-   **Android SDK プラットフォーム ツール 27.0.1** 以降  
-   **Android SDK Build Tools 27.0.3** 以降

これらのパッケージの状態は、次のスクリーンショットに示すように、**[インストール済み]** となります。

[![Android SDK Tools 27.0 のインストール](device-manager-images/win/03-sdk-tools-sml.png)](device-manager-images/win/03-sdk-tools.png#lightbox)

これらのパッケージをインストールした後は、SDK Manager を閉じて Android Device Manager を再起動できます。

## <a name="main-screen"></a>メイン画面

Android Device Manager を初めて起動すると、現在構成されているすべての仮想デバイスが画面に表示されます。 デバイスごとに、**名前**、**オペレーティング システム** (Android API レベル)、**CPU**、**メモリ** サイズ、および画面解像度が表示されます。

[![インストールされているデバイスの一覧とそれらのパラメーター](device-manager-images/win/05-installed-list-sml.png)](device-manager-images/win/05-installed-list.png#lightbox)

一覧のデバイスをクリックすると、右側に **[Start]\(開始\)** ボタンが表示されます。 **[Start]\(開始\)** ボタンをクリックして、この仮想デバイスのエミュレーターを起動できます。

[![デバイス イメージの [Start]\(開始\) ボタン](device-manager-images/win/06-start-button-sml.png)](device-manager-images/win/06-start-button.png#lightbox)

選んだ仮想デバイスでエミュレーターが開始した後、**[Start]\(開始\)** ボタンは **[Stop]\(停止\)** ボタンに変わり、このボタンを使ってエミュレーターを停止できます。

[![実行中のデバイスの [Stop]\(停止\) ボタン](device-manager-images/win/07-stop-button-sml.png)](device-manager-images/win/07-stop-button.png#lightbox)

### <a name="new-device"></a>新しいデバイス

新しいデバイスを作成するには、**[New]\(新規\)** ボタン (画面の右上にあります) をクリックします。

[![新しいデバイスを作成するための [New]\(新規\) ボタン](device-manager-images/win/08-new-button-sml.png)](device-manager-images/win/08-new-button.png#lightbox)

**[New]\(新規\)** をクリックすると、**[New Device]\(新しいデバイス\)** 画面が表示されます。

[![Device Manager の [New Device]\(新しいデバイス\) 画面](device-manager-images/win/09-new-device-editor-sml.png)](device-manager-images/win/09-new-device-editor.png#lightbox)

**[New Device]\(新しいデバイス\)** 画面で新しいデバイスを構成するには、次の手順のようにします。

1. **[Device]\(デバイス\)** プルダウン メニューをクリックして、エミュレートする物理デバイスを選びます。

    [![[Device]\(デバイス\) プルダウン メニュー](device-manager-images/win/10-device-menu-sml.png)](device-manager-images/win/10-device-menu.png#lightbox)

2. **[System image]\(システム イメージ\)** プルダウン メニューをクリックして、この仮想デバイスで使うシステム イメージを選びます。 このメニューの **[Installed]\(インストール済み\)** には、インストールされているシステム デバイス マネージャーのイメージがリストされます。 **[Download]\(ダウンロード\)** セクションには、ご利用の開発用コンピューターでは現在使用できませんが、自動的にインストールできるシステム デバイス マネージャーのイメージがリストされます。

    [![[System image]\(システム イメージ\) プルダウン メニュー](device-manager-images/win/11-system-image-menu-sml.png)](device-manager-images/win/11-system-image-menu.png#lightbox)

3. デバイスの新しい名前を指定します。 次の例では、新しいデバイスに「**Nexus 5 API 25**」という名前を指定しています。

    [![新しいデバイスの名前の指定](device-manager-images/win/12-device-name-sml.png)](device-manager-images/win/12-device-name.png#lightbox)

4. 変更する必要のあるプロパティを編集します。 プロパティを変更する場合は、「[Android 仮想デバイス プロパティの編集](~/android/get-started/installation/android-emulator/device-properties.md)」を参照してください。

5. 明示的に設定する必要がある他のプロパティを追加します。 **[New Device]\(新しいデバイス\)** 画面には最もよく変更されるプロパティのみが表示されていますが、**[Add Property]\(プロパティの追加\)** プルダウン メニュー (左下隅) をクリックしてプロパティを追加できます。 次の例では、`hw.lcd.backlight` プロパティを追加しています。

    [![[Add Property]\(プロパティの追加\) プルダウン メニュー](device-manager-images/win/13-add-property-menu-sml.png)](device-manager-images/win/13-add-property-menu.png#lightbox)

6. 新しいデバイスを作成するには、**[Create]\(作成\)** ボタン (右下隅) をクリックします。

    ![[Create]\(作成\) ボタン](device-manager-images/win/14-create-button.png)

7. **[ライセンスの同意]** 画面が表示される場合があります。 ライセンス条項に同意する場合は、**[Accept]\(同意する\)** をクリックします。

    ![[ライセンスの同意] 画面](device-manager-images/win/15-license-acceptance.png)

8. Android Device Manager により、インストールされている仮想デバイスのリストに新しいデバイスが追加されます。デバイスが作成されている間は、**[Creating]\(作成中\)** という進行状況のインジケーターが表示されます。

    [![作成進行状況インジケーター](device-manager-images/win/16-creating-the-device-sml.png)](device-manager-images/win/16-creating-the-device.png#lightbox)

9. 作成プロセスが完了すると、新しいデバイスがインストール済み仮想デバイスのリストに **[Start]\(開始\)** ボタンと共に表示され、起動できる状態になります。

   [![起動できる状態の新しく作成されたデバイス](device-manager-images/win/17-created-device-sml.png)](device-manager-images/win/17-created-device.png#lightbox)


### <a name="edit-device"></a>デバイスの編集

既存の仮想デバイスを編集するには、デバイスを選んで、**[Edit]\(編集\)** ボタン (画面の右上隅にあります) をクリックします。

[![新しいデバイスを変更するための [Edit]\(編集\) ボタン](device-manager-images/win/19-edit-button-sml.png)](device-manager-images/win/19-edit-button.png#lightbox)

**[Edit]\(編集\)** をクリックすると、選んだ仮想デバイスのデバイス エディターが起動します。

[![デバイス エディター画面](device-manager-images/win/20-device-editor-sml.png)](device-manager-images/win/20-device-editor.png#lightbox)

**デバイス エディター**画面では、最初の列に仮想デバイスのプロパティが一覧表示され、2 番目の列に各プロパティの対応する値が表示されます。 プロパティを選ぶと、そのプロパティの詳しい説明が右側に表示されます。

たとえば、次のスクリーンショットでは、`hw.lcd.density` プロパティを **420** から **240** に変更しています。

[![デバイスの編集の例](device-manager-images/win/21-device-editing-sml.png)](device-manager-images/win/21-device-editing.png#lightbox)

必要な構成の変更を行った後、**[Save]\(保存\)** ボタンをクリックします。
仮想デバイス プロパティの変更の詳細については、「[Android 仮想デバイス プロパティの編集](~/android/get-started/installation/android-emulator/device-properties.md)」を参照してください。

 
### <a name="additional-options"></a>その他のオプション

右上隅の &hellip; メニューから、デバイスの操作に関するその他のオプションを指定できます。

[![その他のオプション メニューの場所](device-manager-images/win/22-overflow-menu-sml.png)](device-manager-images/win/22-overflow-menu.png#lightbox)

その他のオプションのメニューには、次の項目が含まれます。

-   **[Duplicate and Edit]\(複製して編集\)** &ndash; 現在選ばれているデバイスを複製し、異なる一意名を付けて **[New Device]\(新しいデバイス\)** 画面で開きます。 たとえば、**VisualStudio_android-23_x86_phone** を選んで **[Duplicate and Edit]\(複製して編集\)** をクリックすると、名前にカウンターが追加されます。

    [![[Duplicate and Edit]\(複製して編集\) 画面](device-manager-images/win/23-dupe-and-edit-sml.png)](device-manager-images/win/23-dupe-and-edit.png#lightbox)

-   **[Reveal in Explorer]\(エクスプローラーで表示\)** &ndash; 仮想デバイスのファイルが含まれるフォルダーを、Windows のエクスプローラー ウィンドウで開きます。 たとえば、**Nexus 5X API 25** を選んで **[Reveal in Explorer]\(エクスプローラーで表示\)** をクリックすると、次のようなウィンドウが開きます。

    [![[Reveal in Explorer]\(エクスプローラーで表示\) をクリックした結果](device-manager-images/win/24-reveal-in-explorer-sml.png)](device-manager-images/win/24-reveal-in-explorer.png#lightbox)

-   **[Factory Reset]\(出荷時の設定にリセット\)** &ndash; 選んだデバイスを既定の設定にリセットし、デバイスの実行中にユーザーが行ったデバイスの内部状態に対する変更を消去します (存在する場合は、現在の[クイック ブート](~/android/deploy-test/debugging/debug-on-emulator.md#quick-boot)のスナップショットも消去されます)。 仮想デバイスを作成または編集するときに行った変更は消去されません。 このリセットは元に戻すことができないという警告がダイアログ ボックスに表示されます。 **[Wipe user data]\(ユーザー データをワイプ\)** をクリックして、リセットを確定します。

-   **[Delete]\(削除\)** &ndash; 選んだ仮想デバイスを完全に削除します。
    デバイスの削除は元に戻すことができないという警告がダイアログ ボックスに表示されます。 **[Delete]\(削除\)** をクリックして、デバイスを削除することを確認します。


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

この記事では、Android Device Manager を使って、Android 仮想デバイスを作成、複製、カスタマイズ、起動する方法について説明します。

[![Android Device Manager の [Devices]\(デバイス\) タブのスクリーンショット](device-manager-images/mac/01-devices-dialog-sml.png)](device-manager-images/mac/01-devices-dialog.png#lightbox)

> [!NOTE]
> このガイドは、Visual Studio for Mac のみに該当します。
Xamarin Studio は Android Device Manager と互換性がありません。

[Android Emulator](~/android/deploy-test/debugging/debug-on-emulator.md) で実行する *Android 仮想デバイス* (AVD) を作成および構成するには、Android Device Manager を使います。
各 AVD は、物理的な Android デバイスをシミュレートするエミュレーター構成です。 これにより、異なる物理 Android デバイスをシミュレートするさまざまな構成でアプリを実行してテストすることができます。

## <a name="requirements"></a>必要条件

- Visual Studio for Mac 7.5 以降。

- Android SDK 8.0 (API 26) 以降を Android SDK Manager でインストールする必要があります。


## <a name="launching-the-device-manager"></a>Device Manager の起動

**[ツール]、[デバイス マネージャー]** の順にクリックし、Android Device Manager を起動します。

[![[ツール] メニューから起動します](device-manager-images/mac/16-tools-menu-sml.png)](device-manager-images/mac/16-tools-menu.png#lightbox)

Android Device Manager を使う前に、Android SDK Tools バージョン 26.0.2 以降をインストールする必要があります。 Android SDK Tools 26.0.2 以降がインストールされていない場合、起動時に次のエラー ダイアログが表示されます。

![Android SDK インスタンスのエラー ダイアログ](device-manager-images/mac/02-sdk-instance-error.png)

このエラー ダイアログが表示された場合は、**[OK]** をクリックして Android SDK Manager を開きます。 Android SDK Manager で、**[ツール]** タブをクリックして、以下のものをインストールします。

-   **Android SDK Tools 26.0.2** 以降 
-   **Android SDK プラットフォーム ツール 26.0.0** 以降 
-   **Android SDK Build Tools 26.0.0** 以降

これらのパッケージの状態は、次のスクリーンショットに示すように、**[Installed]\(インストール済み\)** となります。

[![Android SDK Tools 26.0 のインストール](device-manager-images/mac/03-sdk-tools-sml.png)](device-manager-images/mac/03-sdk-tools.png#lightbox)

## <a name="main-screen"></a>メイン画面

Android Device Manager を初めて起動すると、現在構成されているすべての仮想デバイスが画面に表示されます。 デバイスごとに、**名前**、**システム イメージ** (Android API レベル)、**CPU**、**メモリ** サイズ、および画面解像度が表示されます。

[![インストールされているデバイスの一覧とそれらのパラメーター](device-manager-images/mac/05-devices-list-sml.png)](device-manager-images/mac/05-devices-list.png#lightbox)

選んだ仮想デバイスのエミュレーターを起動するには、**[Play]\(再生\)** ボタンをクリックします。

[![デバイス イメージの [Start]\(開始\) ボタン](device-manager-images/mac/06-start-button-sml.png)](device-manager-images/mac/06-start-button.png#lightbox)

選んだ仮想デバイスでエミュレーターが開始した後、**[Play]\(再生\)** ボタンは **[Stop]\(停止\)** ボタンに変わり、このボタンを使ってエミュレーターを停止できます。

[![実行中のデバイスの [Stop]\(停止\) ボタン](device-manager-images/mac/07-stop-button-sml.png)](device-manager-images/mac/07-stop-button.png#lightbox)

### <a name="new-device"></a>新しいデバイス

新しいデバイスを作成するには、**[New Device]\(新しいデバイス\)** ボタン (画面の右上にあります) をクリックします。

[![新しいデバイスを作成するための [New]\(新規\) ボタン](device-manager-images/mac/08-new-button-sml.png)](device-manager-images/mac/08-new-button.png#lightbox)

**[New Device]\(新しいデバイス\)** をクリックすると、**[New Device]\(新しいデバイス\)** 画面が表示されます。

[![Device Manager の [New Device]\(新しいデバイス\) 画面](device-manager-images/mac/09-new-device-editor-sml.png)](device-manager-images/mac/09-new-device-editor.png#lightbox)

**[New Device]\(新しいデバイス\)** 画面で新しいデバイスを構成するには、次の手順のようにします。

1. **[Device]\(デバイス\)** プルダウン メニューをクリックして、エミュレートする物理デバイスを選びます。

    [![[Device]\(デバイス\) プルダウン メニュー](device-manager-images/mac/10-device-menu-sml.png)](device-manager-images/mac/10-device-menu.png#lightbox)

2. **[System image]\(システム イメージ\)** プルダウン メニューをクリックして、この仮想デバイスで使うシステム イメージを選びます。 このメニューの **[Installed]\(インストール済み\)** には、インストールされているシステム デバイス マネージャーのイメージがリストされます。 **[Download]\(ダウンロード\)** セクション (表示されている場合) には、ご利用の開発用コンピューターでは現在使用できませんが、自動的にインストールできるシステム デバイス マネージャーのイメージがリストされます。

    [![[System image]\(システム イメージ\) プルダウン メニュー](device-manager-images/mac/11-system-image-menu-sml.png)](device-manager-images/mac/11-system-image-menu.png#lightbox)

3. デバイスの新しい名前を指定します。 次の例では、新しいデバイスに「**Nexus 5X API 25**」という名前を指定しています。

    [![新しいデバイスの名前の指定](device-manager-images/mac/12-device-name-sml.png)](device-manager-images/mac/12-device-name.png#lightbox)

4. 変更する必要のあるプロパティを編集します。 プロパティを変更する場合は、「[Android 仮想デバイス プロパティの編集](~/android/get-started/installation/android-emulator/device-properties.md)」を参照してください。

5. 明示的に設定する必要がある他のプロパティを追加します。 **[New Device]\(新しいデバイス\)** 画面には最もよく変更されるプロパティのみが表示されていますが、**[Add Property]\(プロパティの追加\)** プルダウン メニュー (左下隅) をクリックしてプロパティを追加できます。

    [![[Add Property]\(プロパティの追加\) プルダウン メニュー](device-manager-images/mac/13-add-property-menu-sml.png)](device-manager-images/mac/13-add-property-menu.png#lightbox)

6. **[Custom]\(カスタム\)** をクリックして、デバイスの新しいプロパティを定義することもできます。

    ![[Create]\(作成\) ボタン](device-manager-images/mac/14-custom-button.png)

7. 新しいデバイスを作成するには、**[Create]\(作成\)** ボタン (右下隅) をクリックします。

    ![[Create]\(作成\) ボタン](device-manager-images/mac/15-create-button.png)

8. **[ライセンスの同意]** 画面が表示される場合があります。 ライセンス条項に同意する場合は、**[Accept]\(同意する\)** をクリックします。

9. Android Device Manager により、インストールされている仮想デバイスのリストに新しいデバイスが追加されます。デバイスが作成されている間は、**[Creating]\(作成中\)** という進行状況のインジケーターが表示されます。

    [![作成進行状況インジケーター](device-manager-images/mac/17-creating-the-device-sml.png)](device-manager-images/mac/17-creating-the-device.png#lightbox)

10. 作成プロセスが完了すると、新しいデバイスがデバイスのリストに **[Play]\(再生\)** ボタンと共に表示され、起動できる状態になります。

   [![起動できる状態の新しく作成されたデバイス](device-manager-images/mac/18-created-device-sml.png)](device-manager-images/mac/18-created-device.png#lightbox)


### <a name="edit-device"></a>デバイスの編集

既存の仮想デバイスを編集するには、**[Additional Options]\(追加オプション\)** プルダウン メニュー (歯車アイコン) を選んで、**[Edit]\(編集\)** を選びます。
 
[![新しいデバイスを変更するための [Edit]\(編集\) メニュー選択](device-manager-images/mac/19-edit-button-sml.png)](device-manager-images/mac/19-edit-button.png#lightbox)

**[Edit]\(編集\)** をクリックすると、選んだ仮想デバイスのデバイス エディターが起動します。
 
[![デバイス エディター画面](device-manager-images/mac/20-device-editor-sml.png)](device-manager-images/mac/20-device-editor.png#lightbox)

**デバイス エディター**画面では、最初の列に仮想デバイスのプロパティが一覧表示され、2 番目の列に各プロパティの対応する値が表示されます。 プロパティを選ぶと、そのプロパティの詳しい説明が右側に表示されます。

たとえば、次のスクリーンショットでは、`hw.lcd.density` プロパティを **320** から **240** に変更し、`hw.ramSize` プロパティを **768** に変更しています。
 
[![デバイスの編集の例](device-manager-images/mac/21-device-editing-sml.png)](device-manager-images/mac/21-device-editing.png#lightbox)

必要な構成の変更を行った後、**[Save]\(保存\)** ボタンをクリックします。
仮想デバイス プロパティの変更の詳細については、「[Android 仮想デバイス プロパティの編集](~/android/get-started/installation/android-emulator/device-properties.md)」を参照してください。

 
### <a name="additional-options"></a>その他のオプション

デバイスの操作に関するその他のオプションを、**[Play]\(再生\)** ボタンの左側にあるプルダウン メニューから指定できます。

[![その他のオプション メニューの場所](device-manager-images/mac/22-overflow-menu-sml.png)](device-manager-images/mac/22-overflow-menu.png#lightbox)

その他のオプションのメニューには、次の項目が含まれます。

-   **[Edit]\(編集\)** &ndash; 前述のとおり、現在選ばれているデバイスがデバイス エディターで開かれます。

-   **[Duplicate and Edit]\(複製して編集\)** &ndash; 現在選ばれているデバイスを複製し、異なる一意名を付けて **[New Device]\(新しいデバイス\)** 画面で開きます。
    たとえば、**Nexus 5X API 25** を選んで **[Duplicate and Edit]\(複製して編集\)** をクリックすると、名前にカウンターが追加されます。

    [![[Duplicate and Edit]\(複製して編集\) 画面](device-manager-images/mac/23-dupe-and-edit-sml.png)](device-manager-images/mac/23-dupe-and-edit.png#lightbox)

-   **[Reveal in Finder]\(ファインダーで表示\)** &ndash; 仮想デバイスのファイルが含まれるフォルダーを、macOS のファインダー ウィンドウで開きます。 たとえば、**Nexus 5X API 25** を選んで **[Reveal in Finder]\(ファインダーで表示\)** をクリックすると、次のようなウィンドウが開きます。

    [![[Reveal in Explorer]\(エクスプローラーで表示\) をクリックした結果](device-manager-images/mac/24-reveal-in-finder-sml.png)](device-manager-images/mac/24-reveal-in-finder.png#lightbox)

-   **[Factory Reset]\(出荷時の設定にリセット\)** &ndash; 選んだデバイスを既定の設定にリセットし、デバイスの実行中にユーザーが行ったデバイスの内部状態に対する変更を消去します。 仮想デバイスを作成または編集するときに行った変更は消去されません。 このリセットは元に戻すことができないという警告がダイアログ ボックスに表示されます。 **[Wipe user data]\(ユーザー データをワイプ\)** をクリックして、リセットを確定します。

-   **[Delete]\(削除\)** &ndash; 選んだ仮想デバイスを完全に削除します。
    デバイスの削除は元に戻すことができないという警告がダイアログ ボックスに表示されます。 **[Delete]\(削除\)** をクリックして、デバイスを削除することを確認します。

-----

## <a name="troubleshooting"></a>トラブルシューティング

次のセクションでは、Android Device Manager を使用して仮想デバイスを構成するときに発生する可能性のある問題を診断して回避する方法について説明します。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

### <a name="android-sdk-in-non-standard-location"></a>標準以外の場所にある Android SDK

通常、Android SDK は次の場所にインストールされます。

**C:\\Program Files (x86)\\Android\\android-sdk**

SDK がこの場所にインストールされていない場合、Android Device Manager の起動時に次のエラーが発生する可能性があります。

![Android SDK インスタンスのエラー](troubleshooting-images/win/01-sdk-error.png)

この問題を回避するには、次のようにします。

1. Windows デスクトップから、**C:\\Users\\<*ユーザー名*>\\AppData\\Roaming\\XamarinDeviceManager** に移動します。

    ![Android Device Manager のログ ファイルの場所](troubleshooting-images/win/02-log-files.png)

2. いずれかのログ ファイルをダブルクリックして開き、**構成ファイルのパス**を調べます。 例:

    [![ログ ファイルでの構成ファイルのパス](troubleshooting-images/win/03-config-file-path-sml.png)](troubleshooting-images/win/03-config-file-path.png#lightbox)

3. この場所に移動し、**user.config** をダブルクリックして開きます。 

4. **user.config** で **&lt;UserSettings&gt;** 要素を探し、それに **AndroidSdkPath** 属性を追加します。 Android SDK がインストールされているコンピューター上のパスをこの属性に設定し、ファイルを保存します。 たとえば、Android SDK が **C:\\Programs\\Android\\SDK** にインストールされている場合、**&lt;UserSettings&gt;** は次のようになります。
        
    ```xml
    <UserSettings SdkLibLastWriteTimeUtcTicks="636409365200000000" AndroidSdkPath="C:\Programs\Android\SDK" />
    ```

**user.config** をこのように変更すると、Android Device Manager を起動できるようになります。

### <a name="snapshot-disables-wifi-on-android-oreo"></a>スナップショットによって Android Oreo の WiFi が無効になる

Android Oreo 用に構成された AVD で Wi-Fi アクセスをシミュレートしている場合、スナップショットの後で AVD を再起動すると、Wi-Fi アクセスが無効になる場合があります。

この問題を回避するには、次のようにします。

1. Android Device Manager で AVD を選びます。

2. 追加のオプション メニューから、**[エクスプローラーで表示します]** をクリックします。

3. **snapshots > default_boot** に移動します。

4. **snapshot.pb** ファイルを削除します。

    ![snapshot.pb ファイルの場所](troubleshooting-images/win/05-delete-snapshot.png)

5. AVD を再起動します。 

これらの変更を行った後は、AVD は Wi-Fi が再び機能する状態で再起動します。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

### <a name="snapshot-disables-wifi-on-android-oreo"></a>スナップショットによって Android Oreo の WiFi が無効になる

Android Oreo 用に構成された AVD で Wi-Fi アクセスをシミュレートしている場合、スナップショットの後で AVD を再起動すると、Wi-Fi アクセスが無効になる場合があります。

この問題を回避するには、次のようにします。

1. Android Device Manager で AVD を選びます。

2. 追加のオプション メニューから、**[Finder で表示します]** をクリックします。

3. **snapshots > default_boot** に移動します。

4. **snapshot.pb** ファイルを削除します。

    [![snapshot.pb ファイルの場所](troubleshooting-images/mac/02-delete-snapshot-sml.png)](troubleshooting-images/mac/02-delete-snapshot.png#lightbox)

5. AVD を再起動します。 

これらの変更を行った後は、AVD は Wi-Fi が再び機能する状態で再起動します。

-----

### <a name="generating-a-bug-report"></a>バグ報告の生成

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

上記のトラブルシューティングのヒントで解決できない問題が Android Device Manager で見つかった場合は、タイトル バーを右クリックし、**[Generate Bug Report]\(バグ報告の生成\)** を選択して、バグ報告を提出してください。

[![バグ報告の提出に関するメニュー項目の場所](troubleshooting-images/win/04-bug-report-sml.png)](troubleshooting-images/win/04-bug-report.png#lightbox)


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

上記のトラブルシューティングのヒントで解決できない問題が Android Device Manager で見つかった場合は、**[ヘルプ]、[Generate Bug Report]\(バグ報告の生成\)** の順にクリックして、バグ報告を提出してください。

[![バグ報告の提出に関するメニュー項目の場所](troubleshooting-images/mac/01-bug-report-sml.png)](troubleshooting-images/mac/01-bug-report.png#lightbox)


-----

## <a name="summary"></a>まとめ

このガイドでは、Visual Studio for Mac および Visual Studio Tools for Xamarin で利用できる Android Device Manager について紹介しました。 Android エミュレーターの開始と停止、実行する Android 仮想デバイス (AVD) の選択、仮想デバイスの編集方法などの、重要な機能について説明しました。 さらにカスタマイズを行うためのハードウェア プロファイル プロパティの編集方法について説明し、一般的な問題のトラブルシューティングのヒントを提供しました。


## <a name="related-links"></a>関連リンク

- [Android SDK ツールの変更](~/android/troubleshooting/sdk-cli-tooling-changes.md)
- [Android Emulator でのデバッグ](~/android/deploy-test/debugging/debug-on-emulator.md)
- [SDK Tools のリリース ノート (Google)](https://developer.android.com/studio/releases/sdk-tools)
- [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html)
- [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)