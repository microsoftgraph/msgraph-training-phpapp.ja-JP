<!-- markdownlint-disable MD002 MD041 -->

この演習では、Microsoft Graph をアプリケーションに組み込みます。 このアプリケーションでは、microsoft [graph ライブラリを使用して Microsoft](https://github.com/microsoftgraph/msgraph-sdk-php) graph への呼び出しを行います。

## <a name="get-calendar-events-from-outlook"></a>Outlook からカレンダー イベントを取得する

1. という名前の **アプリケーション** ディレクトリに新しいディレクトリを作成し、 `TimeZones` そのディレクトリに新しいファイルを作成して `TimeZones.php` 、次のコードを追加します。

    :::code language="php" source="../demo/graph-tutorial/app/TimeZones/TimeZones.php":::

    このクラスは、Windows のタイムゾーン名の単純なマッピングを IANA のタイムゾーン識別子に実装します。

1. という名前の **/app/Http/Controllers** ディレクトリに新しいファイルを作成 `CalendarController.php` し、次のコードを追加します。

    ```php
    <?php

    namespace App\Http\Controllers;

    use App\Http\Controllers\Controller;
    use Illuminate\Http\Request;
    use Microsoft\Graph\Graph;
    use Microsoft\Graph\Model;
    use App\TokenStore\TokenCache;
    use App\TimeZones\TimeZones;

    class CalendarController extends Controller
    {
      public function calendar()
      {
        $viewData = $this->loadViewData();

        $graph = $this->getGraph();

        // Get user's timezone
        $timezone = TimeZones::getTzFromWindows($viewData['userTimeZone']);

        // Get start and end of week
        $startOfWeek = new \DateTimeImmutable('sunday -1 week', $timezone);
        $endOfWeek = new \DateTimeImmutable('sunday', $timezone);

        $queryParams = array(
          'startDateTime' => $startOfWeek->format(\DateTimeInterface::ISO8601),
          'endDateTime' => $endOfWeek->format(\DateTimeInterface::ISO8601),
          // Only request the properties used by the app
          '$select' => 'subject,organizer,start,end',
          // Sort them by start time
          '$orderby' => 'start/dateTime',
          // Limit results to 25
          '$top' => 25
        );

        // Append query parameters to the '/me/calendarView' url
        $getEventsUrl = '/me/calendarView?'.http_build_query($queryParams);

        $events = $graph->createRequest('GET', $getEventsUrl)
          // Add the user's timezone to the Prefer header
          ->addHeaders(array(
            'Prefer' => 'outlook.timezone="'.$viewData['userTimeZone'].'"'
          ))
          ->setReturnType(Model\Event::class)
          ->execute();

        return response()->json($events);
      }

      private function getGraph(): Graph
      {
        // Get the access token from the cache
        $tokenCache = new TokenCache();
        $accessToken = $tokenCache->getAccessToken();

        // Create a Graph client
        $graph = new Graph();
        $graph->setAccessToken($accessToken);
        return $graph;
      }
    }
    ```

    このコードの実行内容を考えましょう。

    - 呼び出される URL は `/v1.0/me/calendarView` です。
    - `startDateTime`パラメーターとは、 `endDateTime` ビューの開始と終了を定義します。
    - パラメーターは、 `$select` 各イベントに対して返されるフィールドを、ビューが実際に使用するものだけに制限します。
    - パラメーターは、 `$orderby` 生成された日付と時刻で結果を並べ替えます。最新のアイテムが最初に表示されます。
    - パラメーターは、 `$top` 結果を25個のイベントに制限します。
    - `Prefer: outlook.timezone=""`ヘッダーにより、応答の開始時刻と終了時刻が、ユーザーの優先タイムゾーンに合わせて調整されます。

1. **/Routes/web.php**のルートを更新して、この新しいコントローラーにルートを追加します。

    ```php
    Route::get('/calendar', 'CalendarController@calendar');
    ```

1. サインインして、ナビゲーションバーの [ **予定表** ] リンクをクリックします。 すべてが正常に機能していれば、ユーザーのカレンダーにイベントの JSON ダンプが表示されます。

## <a name="display-the-results"></a>結果の表示

結果を表示するとき、よりユーザー フレンドリなビューを追加できます。

1. という名前の **./resources/views** ディレクトリに新しいファイルを作成 `calendar.blade.php` し、次のコードを追加します。

    :::code language="php" source="../demo/graph-tutorial/resources/views/calendar.blade.php" id="CalendarSnippet":::

    これにより、イベントのコレクションがループされ、各イベントにテーブル行が追加されます。

1. `return response()->json($events);` `calendar` **/App/Http/Controllers/CalendarController.php**のアクションから行を削除し、次のコードに置き換えます。

    ```php
    $viewData['events'] = $events;
    return view('calendar', $viewData);
    ```

1. ページを更新すると、アプリがイベントの表を表示するようになります。

    ![イベント表のスクリーンショット](./images/add-msgraph-01.png)
