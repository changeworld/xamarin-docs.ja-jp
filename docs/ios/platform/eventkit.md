---
title: Xamarin. iOS の EventKit
description: このドキュメントでは、EventKit と Xamarin での使用方法について説明します。 ここでは、カレンダー、カレンダーイベント、およびアラームについて説明し、EventKit を使用したプログラミングでよく使用されるクラスについて説明します。
ms.prod: xamarin
ms.assetid: 00E88629-357D-1FCD-4FCE-1330D5D9D32C
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/19/2017
ms.openlocfilehash: 1be6da2bbaf4aeffe00d90945bd06867f929c334
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032547"
---
# <a name="eventkit-in-xamarinios"></a>Xamarin. iOS の EventKit

iOS には、カレンダーアプリケーションとアラームアプリケーションという2つのカレンダー関連アプリケーションが組み込まれています。 カレンダーアプリケーションがカレンダーデータをどのように管理するかを理解するのは簡単ですが、リマインダーアプリケーションはあまり明確ではありません。 アラームには、実際に期限切れになったとき、完了したときなどに、日付を関連付けることができます。そのため、iOS では、予定表のイベントかリマインダーかにかかわらず、カレンダー*データベース*と呼ばれるすべてのカレンダーデータが1か所に格納されます。

EventKit フレームワークを使用すると、Calendar データベースに格納されているカレンダー、*カレンダーイベント*、および*アラーム* *データにアクセス*できます。 IOS 4 以降、カレンダーとカレンダーイベントへのアクセスが利用可能になりましたが、iOS 6 ではリマインダーへのアクセスが新たに追加されました。

このガイドでは、次の内容について説明します。

- **Eventkit の基本**: 主要なクラスを使用して eventkit の基本的な部分を紹介し、それらの使用方法について理解を深めます。 ドキュメントの次の部分に取り組む前に、このセクションを読む必要があります。 
- **一般的な作業**: 一般的なタスクのセクションは、次のような一般的な作業を行う方法に関するクイックリファレンスとして使用することを目的としています。カレンダーを列挙したり、カレンダーイベントやアラームを作成、保存、取得したり、カレンダーイベントを作成および変更するための組み込みのコントローラーを使用したりします。 このセクションは、特定のタスクを参照することを意図しているため、前方からさかのぼって読み取る必要はありません。 

このガイドのすべてのタスクは、付属のサンプルアプリケーションで利用できます。

 [![](eventkit-images/01.png "The companion sample application screens")](eventkit-images/01.png#lightbox)

## <a name="requirements"></a>［要件］

EventKit は iOS 4.0 で導入されましたが、リマインダーデータへのアクセスは iOS 6.0 で導入されました。 そのため、一般的な EventKit 開発を行うには、少なくともバージョン4.0 を対象とし、通知を行うために6.0 をターゲットにする必要があります。

また、アラームアプリケーションはシミュレーターでは使用できません。つまり、アラームデータは、最初に追加しない限り、リマインダーデータも使用できません。 また、アクセス要求は実際のデバイスのユーザーにのみ表示されます。 そのため、デバイスで EventKit 開発が最適にテストされます。

## <a name="event-kit-basics"></a>イベントキットの基礎

EventKit を使用する場合は、共通のクラスとその使用方法を把握しておくことが重要です。 これらのクラスはすべて、`EventKit` と `EventKitUI` (`EKEventEditController`) にあります。

### <a name="eventstore"></a>EventStore

*Eventstore*クラスは、eventstore で任意の操作を実行するために必要なため、eventstore の中で最も重要なクラスです。 これは、すべての EventKit データの永続ストレージ (データベースエンジン) と考えることができます。 `EventStore` から、カレンダーアプリケーションのカレンダーイベントとカレンダーイベントの両方にアクセスできます。また、アラームアプリケーションでアラームを使用することもできます。

`EventStore` はデータベースエンジンに似ているので、長期間にわたって作成および破棄する必要があります。つまり、アプリケーションインスタンスの有効期間中はできるだけ少なくする必要があります。 実際には、アプリケーションに `EventStore` のインスタンスを1つ作成した後は、アプリケーションの有効期間全体にわたってその参照を保持しておくことをお勧めします。この場合、再度必要にならないようにしてください。 さらに、すべての呼び出しは1つの `EventStore` インスタンスに送られます。 このため、単一のインスタンスを使用可能な状態に保つには、シングルトンパターンをお勧めします。

#### <a name="creating-an-event-store"></a>イベントストアの作成

次のコードは、`EventStore` クラスの1つのインスタンスを作成し、アプリケーション内から静的に使用できるようにする効率的な方法を示しています。

```csharp
public class App
{
    public static App Current {
            get { return current; }
    }
    private static App current;

    public EKEventStore EventStore {
            get { return eventStore; }
    }
    protected EKEventStore eventStore;

    static App ()
    {
            current = new App();
    }
    protected App () 
    {
            eventStore = new EKEventStore ( );
    }
}
```

上記のコードでは、アプリケーションの読み込み時に、シングルトンパターンを使用して `EventStore` のインスタンスをインスタンス化しています。 この `EventStore` は、次のようにアプリケーション内からグローバルにアクセスできます。

```csharp
App.Current.EventStore;
```

ここに示したすべての例では、このパターンが使用されているので、`App.Current.EventStore`経由で `EventStore` を参照しています。

#### <a name="requesting-access-to-calendar-and-reminder-data"></a>カレンダーとリマインダーデータへのアクセスの要求

イベントストア経由で任意のデータにアクセスできるようにするには、まず、必要なものに応じて、カレンダーイベントデータまたはアラームデータへのアクセスをアプリケーションが要求する必要があります。 これを容易にするために、`EventStore` は `RequestAccess` と呼ばれるメソッドを公開します。このメソッドを呼び出すと、に渡される `EKEntityType` に応じて、アプリケーションがカレンダーデータまたはアラームデータへのアクセスを要求していることをユーザーに通知するメッセージが表示されます。し. アラートビューが生成されるため、呼び出しは非同期であり、2つのパラメーターを受け取る `NSAction` (またはラムダ) として渡される完了ハンドラーを呼び出します。アクセスが許可されたかどうかのブール値、および `NSError`。つまり、null でない場合は、要求にエラー情報が含まれます。 たとえば、次のコードは、カレンダーイベントデータへのアクセスを要求し、要求が許可されていない場合はアラートビューを表示します。

```csharp
App.Current.EventStore.RequestAccess (EKEntityType.Event, 
    (bool granted, NSError e) => {
            if (granted)
                    //do something here
            else
                    new UIAlertView ( "Access Denied", 
"User Denied Access to Calendar Data", null,
"ok", null).Show ();
            } );
```

要求が許可されると、アプリケーションがデバイスにインストールされている限り、その要求は記憶され、ユーザーに警告が表示されません。
ただし、アクセスは、カレンダーイベントまたはアラームが付与されたリソースの種類に対してのみ与えられます。 アプリケーションが両方にアクセスする必要がある場合は、両方を要求する必要があります。

アクセス許可が記憶されているため、要求を毎回行う方が比較的安価です。そのため、操作を実行する前に常にアクセスを要求することをお勧めします。

また、完了ハンドラーが別の (UI ではない) スレッドで呼び出されるため、完了ハンドラーの UI に対する更新は `InvokeOnMainThread`経由で呼び出される必要があります。そうしないと、例外がスローされ、キャッチされない場合はアプリケーションがクラッシュします。

### <a name="ekentitytype"></a>EKEntityType

`EKEntityType` は、`EventKit` 項目またはデータの型を記述する列挙体です。 `Event` とリマインダーの2つの値があります。 このメソッドは、アクセスまたは取得するデータの種類を `EventKit` `EventStore.RequestAccess` など、さまざまな方法で使用されます。

### <a name="ekcalendar"></a>EKCalendar

 *EKCalendar*は、カレンダーイベントのグループを含む calendar を表します。 予定表は、 *Exchange Server*や*Google*などのサードパーティのプロバイダーの場所に、 *iCloud*でローカルに保存するなど、さまざまな場所に格納することができます。多くの場合 `EKCalendar` を使用して、イベントを検索する場所 `EventKit` を指定したり、イベントを保存する場所を指定したりします。

### <a name="ekeventeditcontroller"></a>EKEventEditController

 *EKEventEditController*は `EventKitUI` 名前空間にあり、カレンダーイベントの編集や作成に使用できる組み込みのコントローラーです。 組み込みカメラコントローラーと同様に、`EKEventEditController` は、UI を表示して保存を処理するために多くの処理を行います。

### <a name="ekevent"></a>EKEvent

 *EKEvent*はカレンダーイベントを表します。 `EKEvent` と `EKReminder` はどちらも `EKCalendarItem` から継承され、`Title`や `Notes`などのフィールドがあります。

### <a name="ekreminder"></a>EKReminder

 *EKReminder*は、アラーム項目を表します。

### <a name="ekspan"></a>EKSpan

*EKSpan*は、再発する可能性があるイベントを変更するときのイベントの範囲を示す列挙体です。*含め*と*FutureEvents*の2つの値があります。 `ThisEvent` は、参照されている系列内の特定のイベントに対してのみ変更が発生することを意味し、`FutureEvents` はそのイベントと今後のすべての繰り返しに影響します。

## <a name="tasks"></a>[タスク]

使いやすくするために、EventKit の使用状況は、次のセクションで説明する一般的なタスクに分割されています。

### <a name="enumerate-calendars"></a>カレンダーの列挙

デバイスでユーザーが構成したカレンダーを列挙するには、`EventStore` で `GetCalendars` を呼び出し、受信する予定のカレンダー (アラームまたはイベント) の種類を渡します。

```csharp
EKCalendar[] calendars = 
App.Current.EventStore.GetCalendars ( EKEntityType.Event );
```

### <a name="add-or-modify-an-event-using-the-built-in-controller"></a>組み込みのコントローラーを使用してイベントを追加または変更する

Calendar アプリケーションの使用時にユーザーに表示されるものと同じ UI を使用してイベントを作成または編集する場合、 *EKEventEditViewController*は多くの処理を実行します。

 [![](eventkit-images/02.png "The UI that is presented to the user when using the Calendar Application")](eventkit-images/02.png#lightbox)

これを使用するには、メソッド内で宣言されている場合にガベージコレクションが行われないように、クラスレベル変数として宣言します。

```csharp
public class HomeController : DialogViewController
{
        protected CreateEventEditViewDelegate eventControllerDelegate;
        ...
}
```

次に、それを起動するには、インスタンス化して、`EventStore`への参照を指定し、それに*EKEventEditViewDelegate*デリゲートを接続してから、`PresentViewController`を使用して表示します。

```csharp
EventKitUI.EKEventEditViewController eventController = 
        new EventKitUI.EKEventEditViewController ();

// set the controller's event store - it needs to know where/how to save the event
eventController.EventStore = App.Current.EventStore;

// wire up a delegate to handle events from the controller
eventControllerDelegate = new CreateEventEditViewDelegate ( eventController );
eventController.EditViewDelegate = eventControllerDelegate;

// show the event controller
PresentViewController ( eventController, true, null );
```

必要に応じて、イベントを事前設定する場合は、次に示すように、新しいイベントを作成するか、保存されているイベントを取得することができます。

```csharp
EKEvent newEvent = EKEvent.FromStore ( App.Current.EventStore );
// set the alarm for 10 minutes from now
newEvent.AddAlarm ( EKAlarm.FromDate ( DateTime.Now.AddMinutes ( 10 ) ) );
// make the event start 20 minutes from now and last 30 minutes
newEvent.StartDate = DateTime.Now.AddMinutes ( 20 );
newEvent.EndDate = DateTime.Now.AddMinutes ( 50 );
newEvent.Title = "Get outside and exercise!";
newEvent.Notes = "This is your reminder to go and exercise for 30 minutes.”;
```

UI を事前設定する場合は、コントローラーのイベントプロパティを必ず設定してください。

```csharp
eventController.Event = newEvent;
```

既存のイベントを使用するには、後の「 *ID でイベントを取得*する」セクションを参照してください。

デリゲートは `Completed` メソッドをオーバーライドする必要があります。これは、ユーザーがダイアログで終了したときにコントローラーによって呼び出されます。

```csharp
protected class CreateEventEditViewDelegate : EventKitUI.EKEventEditViewDelegate
{
        // we need to keep a reference to the controller so we can dismiss it
        protected EventKitUI.EKEventEditViewController eventController;

        public CreateEventEditViewDelegate (EventKitUI.EKEventEditViewController eventController)
        {
                // save our controller reference
                this.eventController = eventController;
        }

        // completed is called when a user eith
        public override void Completed (EventKitUI.EKEventEditViewController controller, EKEventEditViewAction action)
        {
                eventController.DismissViewController (true, null);
                }
        }
}
```

必要に応じて、デリゲートで `Completed` メソッドの*アクション*を確認して、イベントと再保存を変更したり、キャンセルされた場合はありを実行することができます。

```csharp
public override void Completed (EventKitUI.EKEventEditViewController controller, EKEventEditViewAction action)
{
        eventController.DismissViewController (true, null);

        switch ( action ) {

        case EKEventEditViewAction.Canceled:
                break;
        case EKEventEditViewAction.Deleted:
                break;
        case EKEventEditViewAction.Saved:
                // if you wanted to modify the event you could do so here,
// and then save:
                //App.Current.EventStore.SaveEvent ( controller.Event, )
                break;
        }
}
```

### <a name="creating-an-event-programmatically"></a>プログラムによるイベントの作成

コードでイベントを作成するには、`EKEvent` クラスで*Fromstore*ファクトリメソッドを使用し、そこにデータを設定します。

```csharp
EKEvent newEvent = EKEvent.FromStore ( App.Current.EventStore );
// set the alarm for 10 minutes from now
newEvent.AddAlarm ( EKAlarm.FromDate ( DateTime.Now.AddMinutes ( 10 ) ) );
// make the event start 20 minutes from now and last 30 minutes
newEvent.StartDate = DateTime.Now.AddMinutes ( 20 );
newEvent.EndDate = DateTime.Now.AddMinutes ( 50 );
newEvent.Title = "Get outside and do some exercise!";
newEvent.Notes = "This is your motivational event to go and do 30 minutes of exercise. Super important. Do this.";
```

イベントを保存するカレンダーを設定する必要がありますが、設定がない場合は、既定のを使用できます。

```csharp
newEvent.Calendar = App.Current.EventStore.DefaultCalendarForNewEvents;
```

イベントを保存するには、`EventStore`で*Saveevent*メソッドを呼び出します。

```csharp
NSError e;
App.Current.EventStore.SaveEvent ( newEvent, EKSpan.ThisEvent, out e );
```

保存されると、 *EventIdentifier*プロパティは、後でイベントを取得するために使用できる一意の識別子で更新されます。

```csharp
Console.WriteLine ("Event Saved, ID: " + newEvent.CalendarItemIdentifier);
```

 `EventIdentifier` は、文字列形式の GUID です。

### <a name="create-a-reminder-programmatically"></a>プログラムによるリマインダーの作成

コードでリマインダーを作成することは、カレンダーイベントを作成することとほぼ同じです。

```csharp
EKReminder reminder = EKReminder.Create ( App.Current.EventStore );
reminder.Title = "Do something awesome!";
reminder.Calendar = App.Current.EventStore.DefaultCalendarForNewReminders;
```

保存するには、`EventStore`で*SaveReminder*メソッドを呼び出します。

```csharp
NSError e;
App.Current.EventStore.SaveReminder ( reminder, true, out e );
```

### <a name="retrieving-an-event-by-id"></a>ID でイベントを取得する

イベントを ID で取得するには、`EventStore` で*Eventfromidentifier*メソッドを使用し、イベントからプルされた `EventIdentifier` を渡します。

```csharp
EKEvent mySavedEvent = App.Current.EventStore.EventFromIdentifier ( newEvent.EventIdentifier );
```

イベントについては、他に2つの識別子プロパティがありますが、この場合に機能するのは `EventIdentifier` だけです。

### <a name="retrieving-a-reminder-by-id"></a>ID でリマインダーを取得する

リマインダーを取得するには、`EventStore` で*Getcalendaritem*メソッドを使用し、それに*calendaritemidentifier*を渡します。

```csharp
EKCalendarItem myReminder = App.Current.EventStore.GetCalendarItem ( reminder.CalendarItemIdentifier );
```

`GetCalendarItem` は `EKCalendarItem`を返すため、リマインダーデータにアクセスする必要がある場合、または後で `EKReminder` としてインスタンスを使用する必要がある場合は、`EKReminder` にキャストする必要があります。

カレンダーイベントには `GetCalendarItem` を使用しないでください。作成時には機能しません。

### <a name="deleting-an-event"></a>イベントの削除

カレンダーイベントを削除するには、`EventStore` で*Removeevent*を呼び出し、イベントへの参照と適切な `EKSpan`を渡します。

```csharp
NSError e;
App.Current.EventStore.RemoveEvent ( mySavedEvent, EKSpan.ThisEvent, true, out e);
```

ただし、イベントが削除されると、イベント参照が `null`されます。

### <a name="deleting-a-reminder"></a>リマインダーの削除

リマインダーを削除するには、`EventStore` で*RemoveReminder*を呼び出し、リマインダーへの参照を渡します。

```csharp
NSError e;
App.Current.EventStore.RemoveReminder ( myReminder as EKReminder, true, out e);
```

上記のコードでは、`GetCalendarItem` を取得するために使用されたため、`EKReminder`にキャストがあります。

### <a name="searching-for-events"></a>イベントの検索

カレンダーイベントを検索するには、`EventStore`の*PredicateForEvents*メソッドを使用して*NSPredicate*オブジェクトを作成する必要があります。 `NSPredicate` は、iOS が一致を見つけるために使用するクエリデータオブジェクトです。

```csharp
DateTime startDate = DateTime.Now.AddDays ( -7 );
DateTime endDate = DateTime.Now;
// the third parameter is calendars we want to look in, to use all calendars, we pass null
NSPredicate query = App.Current.EventStore.PredicateForEvents ( startDate, endDate, null );
```

`NSPredicate`を作成したら、`EventStore`に対して、次のよう*にメソッドを使用します*。

```csharp
// execute the query
EKCalendarItem[] events = App.Current.EventStore.EventsMatching ( query );
```

クエリは同期 (ブロック) であり、クエリによっては長い時間がかかる場合があるため、新しいスレッドまたはタスクを実行することが必要になる場合があります。

### <a name="searching-for-reminders"></a>リマインダーを検索しています

アラームの検索は、イベントに似ています。述語が必要ですが、呼び出しは既に非同期であるため、スレッドのブロックを気にする必要はありません。

```csharp
// create our NSPredicate which we'll use for the query
NSPredicate query = App.Current.EventStore.PredicateForReminders ( null );

// execute the query
App.Current.EventStore.FetchReminders (
        query, ( EKReminder[] items ) => {
                // do someting with the items
        } );
```

## <a name="summary"></a>まとめ

このドキュメントでは、EventKit フレームワークの重要な部分と、多くの一般的なタスクの概要について説明しました。 ただし、EventKit フレームワークは非常に大規模で強力な機能を備えています。また、バッチ更新、アラームの構成、イベントの繰り返しの構成、カレンダーデータベースでの変更の登録とリッスンなど、ここでは紹介されていない機能が含まれています。ジオフェンスなどを設定します。  詳細については、「Apple の[予定表とアラームのプログラミングガイド](https://developer.apple.com/library/prerelease/ios/#documentation/DataManagement/Conceptual/EventKitProgGuide/Introduction/Introduction.html)」を参照してください。

## <a name="related-links"></a>関連リンク

- [カレンダー (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/calendars)
- [iOS 6 の概要](~/ios/platform/introduction-to-ios6/index.md)
- [予定表とアラームの概要](https://developer.apple.com/library/prerelease/ios/#documentation/DataManagement/Conceptual/EventKitProgGuide/Introduction/Introduction.html)
