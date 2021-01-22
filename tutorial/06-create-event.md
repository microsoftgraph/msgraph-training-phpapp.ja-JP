<!-- markdownlint-disable MD002 MD041 -->

このセクションでは、ユーザーの予定表にイベントを作成する機能を追加します。

## <a name="create-new-event-form"></a>新しいイベント フォームを作成する

1. **./resources/views** ディレクトリに新しいファイルを作成し、 `newevent.blade.php` 次のコードを追加します。

    :::code language="php" source="../demo/graph-tutorial/resources/views/newevent.blade.php" id="NewEventFormSnippet":::

## <a name="add-controller-actions"></a>コントローラーアクションを追加する

1. **./app/Http/Controllers/CalendarController.php** を開き、次の関数を追加してフォームをレンダリングします。

    :::code language="php" source="../demo/graph-tutorial/app/Http/Controllers/CalendarController.php" id="getNewEventFormSnippet":::

1. 次の関数を追加して、ユーザーの送信時にフォーム データを受信し、ユーザーの予定表に新しいイベントを作成します。

    :::code language="php" source="../demo/graph-tutorial/app/Http/Controllers/CalendarController.php" id="createNewEventSnippet":::

    このコードの動作を検討します。

    - 出席者フィールドの入力を Graph 出席者オブジェクトの配列に [変換](https://docs.microsoft.com/graph/api/resources/attendee?view=graph-rest-1.0) します。
    - フォーム入力から [イベント](https://docs.microsoft.com/graph/api/resources/event?view=graph-rest-1.0) を作成します。
    - エンドポイントに POST を送信 `/me/events` し、予定表ビューにリダイレクトします。

1. 変更内容をすべて保存し、サーバーを再起動します。 [新 **しいイベント] ボタン** を使用して、新しいイベント フォームに移動します。

1. フォームの値を入力します。 現在の週の開始日を使用します。 [**作成**] を選択します。

    ![新しいイベント フォームのスクリーンショット](images/create-event-01.png)

1. アプリが予定表ビューにリダイレクトするときに、新しいイベントが結果に表示されるのを確認します。
