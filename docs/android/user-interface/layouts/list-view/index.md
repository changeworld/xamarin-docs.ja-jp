---
title: Xamarin. Android での ListView の使用
description: ListView は、Android アプリケーションの重要な UI コンポーネントです。これは、メニューオプションの短い一覧から、連絡先またはインターネットお気に入りの長い一覧に至るまで、あらゆる場所で使用されます。 この機能を使用すると、組み込みスタイルで書式設定したり、広範にカスタマイズしたりできる行のスクロールリストを簡単に表示できます。
ms.prod: xamarin
ms.assetid: C2BA2705-9B20-01C2-468D-860BDFEDC157
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 04/25/2018
ms.openlocfilehash: f6579e3b70e3788046916db12e201550e7fd5f16
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028886"
---
# <a name="xamarinandroid-listview"></a>Xamarin Android ListView

_ListView は、Android アプリケーションの重要な UI コンポーネントです。これは、メニューオプションの短い一覧から、連絡先またはインターネットお気に入りの長い一覧に至るまで、あらゆる場所で使用されます。この機能を使用すると、組み込みスタイルで書式設定したり、広範にカスタマイズしたりできる行のスクロールリストを簡単に表示できます。_

## <a name="overview"></a>概要

リストビューとアダプターは、Android アプリケーションの最も基本的な構成要素に含まれています。 `ListView` クラスは、短いメニューでも長いスクロールリストでも、データを表示するための柔軟な方法を提供します。 高速スクロール、インデックス、1つまたは複数の選択などの使いやすさ機能を提供し、アプリケーションのためのモバイル対応ユーザーインターフェイスを作成するのに役立ちます。 `ListView` インスタンスでは、行ビューに含まれているデータをフィードするための *Adapter* が必要です。

このガイドでは、`ListView` およびさまざまな `Adapter` クラスを Xamarin Android で実装する方法について説明します。 また、`ListView`の外観をカスタマイズする方法についても説明します。また、メモリ使用量を減らすために行を再利用することの重要性についても説明します。 また、アクティビティのライフサイクルが `ListView` に与える影響と使用 `Adapter` 方法についても説明します。 Xamarin. iOS を使用してクロスプラットフォームアプリケーションを操作している場合、`ListView` コントロールは iOS `UITableView` に構造的に似ています (および Android `Adapter` は `UITableViewSource`に似ています)。

まず、簡単なチュートリアルで `ListView` を紹介し、基本的なコード例を示します。 次に、より高度なトピックへのリンクを使用して、実際のアプリで `ListView` を使用できるようにします。

> [!NOTE]
> `RecyclerView` ウィジェットは、より高度で柔軟な `ListView`のバージョンです。 `RecyclerView` は `ListView` (および `GridView`) の後継となるように設計されているので、新しいアプリ開発には `ListView` ではなく `RecyclerView` を使用することをお勧めします。 詳細については、「 [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)」を参照してください。

## <a name="listview-tutorial"></a>ListView のチュートリアル

[`ListView`](xref:Android.Widget.ListView)は[`ViewGroup`](xref:Android.Views.ViewGroup)
スクロール可能な項目の一覧を作成する。 リスト項目は、 [`IListAdapter`](xref:Android.Widget.IListAdapter)を使用してリストに自動的に挿入されます。

このチュートリアルでは、文字列配列から読み取られた国名のスクロール可能な一覧を作成します。 リスト項目を選択すると、リスト内の項目の位置がトーストメッセージに表示されます。

**HelloListView**という名前の新しいプロジェクトを開始します。

**List_item**という名前の xml ファイルを作成し、 **Resources/Layout/** フォルダー内に保存します。 次のように挿入します。

```xml
<?xml version="1.0" encoding="utf-8"?>
<TextView xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:padding="10dp"
    android:textSize="16sp">
</TextView>
```

このファイルは、 [`ListView`](xref:Android.Widget.ListView)に配置される各項目のレイアウトを定義します。

`MainActivity.cs` を開き、クラスを変更して[`ListActivity`](xref:Android.App.ListActivity) ( [`Activity`](xref:Android.App.Activity)ではなく) を拡張します。

```csharp
public class MainActivity : ListActivity
{
```

[`OnCreate()`](xref:Android.App.Activity.OnCreate*)) メソッドの次のコードを挿入します。

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    ListAdapter = new ArrayAdapter<string> (this, Resource.Layout.list_item, countries);

    ListView.TextFilterEnabled = true;

    ListView.ItemClick += delegate (object sender, AdapterView.ItemClickEventArgs args)
    {
        Toast.MakeText(Application, ((TextView)args.View).Text, ToastLength.Short).Show();
    };
}
```

これにより、アクティビティのレイアウトファイルは読み込まれないことに注意してください (通常は[`SetContentView(int)`](xref:Android.App.Activity.SetContentView*))。
代わりに、 [`ListAdapter`](xref:Android.App.ListActivity.ListAdapter)を設定します。
プロパティは、を自動的に追加し[`ListView`](xref:Android.Widget.ListView)
[`ListActivity`](xref:Android.App.ListActivity)の画面全体を塗りつぶす場合はです。
このメソッドは、 [`ListView`](xref:Android.Widget.ListView)に配置されるリスト項目の配列を管理する[`ArrayAdapter<T>`](xref:Android.Widget.ArrayAdapter`1)を受け取ります。
[`ArrayAdapter<T>`](xref:Android.Widget.ArrayAdapter`1)
コンストラクターは、アプリケーション[`Context`](xref:Android.Content.Context)、(前の手順で作成した) 各リスト項目のレイアウトの説明、および `T[]` または[`Java.Util.IList<T>`](xref:Java.Util.IList)を受け取ります。
[`ListView`](xref:Android.Widget.ListView)に挿入するオブジェクトの配列
(次の定義)。

[`TextFilterEnabled`](xref:Android.Widget.AbsListView.TextFilterEnabled)
プロパティは、 [`ListView`](xref:Android.Widget.ListView)のテキストフィルター処理をオンにして、ユーザーが入力を開始すると一覧がフィルター処理されるようにします。

[`ItemClick`](xref:Android.Widget.AdapterView.ItemClick)
イベントを使用して、クリックするハンドラーをサブスクライブできます。 [`ListView`](xref:Android.Widget.ListView)内の項目
がクリックされ、ハンドラーが呼び出され、 [`Toast`](xref:Android.Widget.Toast)
クリックした項目のテキストを使用して、メッセージが表示されます。

[`ListAdapter`](xref:Android.App.ListActivity.ListAdapter)用に独自のレイアウトファイルを定義するのではなく、プラットフォームによって提供されるリスト項目のデザインを使用できます。
たとえば、`Resource.Layout.list_item`ではなく `Android.Resource.Layout.SimpleListItem1` を使用します。

次の `using` ステートメントを追加します。

```csharp
using System;
```

次に、次の文字列配列を `MainActivity`のメンバーとして追加します。

```csharp
static readonly string[] countries = new String[] {
    "Afghanistan","Albania","Algeria","American Samoa","Andorra",
    "Angola","Anguilla","Antarctica","Antigua and Barbuda","Argentina",
    "Armenia","Aruba","Australia","Austria","Azerbaijan",
    "Bahrain","Bangladesh","Barbados","Belarus","Belgium",
    "Belize","Benin","Bermuda","Bhutan","Bolivia",
    "Bosnia and Herzegovina","Botswana","Bouvet Island","Brazil","British Indian Ocean Territory",
    "British Virgin Islands","Brunei","Bulgaria","Burkina Faso","Burundi",
    "Cote d'Ivoire","Cambodia","Cameroon","Canada","Cape Verde",
    "Cayman Islands","Central African Republic","Chad","Chile","China",
    "Christmas Island","Cocos (Keeling) Islands","Colombia","Comoros","Congo",
    "Cook Islands","Costa Rica","Croatia","Cuba","Cyprus","Czech Republic",
    "Democratic Republic of the Congo","Denmark","Djibouti","Dominica","Dominican Republic",
    "East Timor","Ecuador","Egypt","El Salvador","Equatorial Guinea","Eritrea",
    "Estonia","Ethiopia","Faeroe Islands","Falkland Islands","Fiji","Finland",
    "Former Yugoslav Republic of Macedonia","France","French Guiana","French Polynesia",
    "French Southern Territories","Gabon","Georgia","Germany","Ghana","Gibraltar",
    "Greece","Greenland","Grenada","Guadeloupe","Guam","Guatemala","Guinea","Guinea-Bissau",
    "Guyana","Haiti","Heard Island and McDonald Islands","Honduras","Hong Kong","Hungary",
    "Iceland","India","Indonesia","Iran","Iraq","Ireland","Israel","Italy","Jamaica",
    "Japan","Jordan","Kazakhstan","Kenya","Kiribati","Kuwait","Kyrgyzstan","Laos",
    "Latvia","Lebanon","Lesotho","Liberia","Libya","Liechtenstein","Lithuania","Luxembourg",
    "Macau","Madagascar","Malawi","Malaysia","Maldives","Mali","Malta","Marshall Islands",
    "Martinique","Mauritania","Mauritius","Mayotte","Mexico","Micronesia","Moldova",
    "Monaco","Mongolia","Montserrat","Morocco","Mozambique","Myanmar","Namibia",
    "Nauru","Nepal","Netherlands","Netherlands Antilles","New Caledonia","New Zealand",
    "Nicaragua","Niger","Nigeria","Niue","Norfolk Island","North Korea","Northern Marianas",
    "Norway","Oman","Pakistan","Palau","Panama","Papua New Guinea","Paraguay","Peru",
    "Philippines","Pitcairn Islands","Poland","Portugal","Puerto Rico","Qatar",
    "Reunion","Romania","Russia","Rwanda","Sqo Tome and Principe","Saint Helena",
    "Saint Kitts and Nevis","Saint Lucia","Saint Pierre and Miquelon",
    "Saint Vincent and the Grenadines","Samoa","San Marino","Saudi Arabia","Senegal",
    "Seychelles","Sierra Leone","Singapore","Slovakia","Slovenia","Solomon Islands",
    "Somalia","South Africa","South Georgia and the South Sandwich Islands","South Korea",
    "Spain","Sri Lanka","Sudan","Suriname","Svalbard and Jan Mayen","Swaziland","Sweden",
    "Switzerland","Syria","Taiwan","Tajikistan","Tanzania","Thailand","The Bahamas",
    "The Gambia","Togo","Tokelau","Tonga","Trinidad and Tobago","Tunisia","Turkey",
    "Turkmenistan","Turks and Caicos Islands","Tuvalu","Virgin Islands","Uganda",
    "Ukraine","United Arab Emirates","United Kingdom",
    "United States","United States Minor Outlying Islands","Uruguay","Uzbekistan",
    "Vanuatu","Vatican City","Venezuela","Vietnam","Wallis and Futuna","Western Sahara",
    "Yemen","Yugoslavia","Zambia","Zimbabwe"
  };
```

これは、 [`ListView`](xref:Android.Widget.ListView)に配置される文字列の配列です。

アプリケーションを実行します。 一覧をスクロールするか、「」と入力してフィルター処理し、項目をクリックしてメッセージを表示することができます。 次のように表示されます。

[![の例では、名前が country である ListView のスクリーンショットを示します。](images/01-listview-example-sml.png)](images/01-listview-example.png#lightbox)

ハードコーディングされた文字列配列を使用することは、最適なデザイン方法ではないことに注意してください。 このチュートリアルでは、わかりやすくするために、1つを使用して[`ListView`](xref:Android.Widget.ListView)
ウィジェット. プロジェクト**リソース/値/文字列 .xml**ファイル内の `string-array` リソースを使用して、などの外部リソースによって定義された文字列配列を参照することをお勧めします。 (例:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
  <string name="app_name">HelloListView</string>
  <string-array name="countries_array">
    <item>Bahrain</item>
    <item>Bangladesh</item>
    <item>Barbados</item>
    <item>Belarus</item>
    <item>Belgium</item>
    <item>Belize</item>
    <item>Benin</item>
  </string-array>
</resources>
```

これらのリソース文字列を[`ArrayAdapter`](xref:Android.Widget.ArrayAdapter`1)に使用するには、元の[`ListAdapter`](xref:Android.App.ListActivity.ListAdapter)を置き換えます。
次の行を使用します。

```csharp
string[] countries = Resources.GetStringArray (Resource.Array.countries_array);
ListAdapter = new ArrayAdapter<string> (this, Resource.Layout.list_item, countries);
```

アプリケーションを実行します。 次のように表示されます。

[名前の小さいリストを含む ListView の![例](images/02-smaller-example-sml.png)](images/02-smaller-example.png#lightbox)

## <a name="going-further-with-listview"></a>ListView をさらに進める

残りのトピック (下のリンク) では、`ListView` クラスの操作と、それに使用できるさまざまな種類のアダプターの種類について、包括的に説明します。 構造は、次のとおりです。

- **視覚的な外観**は、`ListView` コントロールの一部とその動作を &ndash; します。

- **クラス**&ndash;、`ListView`を表示するために使用されるクラスの概要を示します。

- **ListView でデータを表示**すると、単純なデータの一覧を表示する方法 &ndash; ます。`ListView's` ユーザビリティ機能を実装する方法さまざまな組み込みの行レイアウトを使用する方法また、アダプターで行ビューを再利用してメモリを節約する方法について説明します。

- **カスタムの外観**&ndash;、カスタムレイアウト、フォント、および色で `ListView` のスタイルを変更します。

- **Sqlite を使用**すると、sqlite データベースのデータを `CursorAdapter`で表示する方法 &ndash; ます。

- **アクティビティライフサイクル**は、ライフサイクルのどこでもデータを設定し、リソースを解放する必要がある場合など、`ListView` アクティビティを実装する際の設計上の考慮事項 &ndash; ます。

(6 つの部分に分かれています) 説明は、「`ListView` クラス自体の概要」から始まり、その使用方法についてより複雑な例を紹介します。

- [ListView のパーツと機能](~/android/user-interface/layouts/list-view/parts-and-functionality.md)
- [ListView にデータを読み込む](~/android/user-interface/layouts/list-view/populating.md)
- [ListView の外観のカスタマイズ](~/android/user-interface/layouts/list-view/customizing-appearance.md)
- [CursorAdapters の使用](~/android/user-interface/layouts/list-view/cursor-adapters.md)
- [ContentProvider の使用](~/android/user-interface/layouts/list-view/content-provider.md)
- [ListView とアクティビティのライフサイクル](~/android/user-interface/layouts/list-view/activity-lifecycle.md)

## <a name="summary"></a>まとめ

この一連のトピック `ListView` 導入され、`ListActivity`の組み込み機能を使用する方法の例をいくつか紹介しました。 ここでは、カラフルなレイアウトと SQLite データベースの使用を許可する `ListView` のカスタム実装について説明し、`ListView` 実装におけるアクティビティのライフサイクルの関連性について簡単に触れました。

## <a name="related-links"></a>関連リンク

- [AccessoryViews (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/accessoryviews)
- [BasicTableAndroid (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/basictableandroid)
- [BasicTableAdapter (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/basictableadapter)
- [BuiltInViews (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/builtinviews)
- [CustomRowView (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/customrowview)
- [FastScroll (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/fastscroll)
- [SectionIndex (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/sectionindex)
- [SimpleCursorTableAdapter (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/simplecursortableadapter)
- [カーソル Tableadapter (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/cursortableadapter)
- [アクティビティライフサイクルのチュートリアル](~/android/app-fundamentals/activity-lifecycle/index.md)
- [テーブルとセルの操作 (Xamarin. iOS)](~/ios/user-interface/controls/tables/index.md)
- [ListView クラスの参照](xref:Android.Widget.ListView)
- [ListActivity クラスリファレンス](xref:Android.App.ListActivity)
- [BaseAdapter クラスリファレンス](xref:Android.Widget.BaseAdapter)
- [ArrayAdapter クラスリファレンス](xref:Android.Widget.ArrayAdapter)
- [カーソルクラス参照のカーソル](xref:Android.Widget.CursorAdapter)
