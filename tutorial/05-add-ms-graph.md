<!-- markdownlint-disable MD002 MD041 -->

この演習では、Microsoft Graph をアプリケーションに組み込む必要があります。 このアプリケーションでは [、Microsoft Graph ライブラリを使用](https://github.com/microsoftgraph/msgraph-sdk-php) して Microsoft Graph を呼び出します。

## <a name="get-calendar-events-from-outlook"></a>Outlook からカレンダー イベントを取得する

1. **./app** ディレクトリに新しいディレクトリを作成し、そのディレクトリに新しいファイルを作成し、次の `TimeZones` `TimeZones.php` コードを追加します。

    :::code language="php" source="../demo/graph-tutorial/app/TimeZones/TimeZones.php":::

    このクラスは、Windows タイム ゾーン名と IANA タイム ゾーン識別子の単純なマッピングを実装します。

1. **./app/Http/Controllers** ディレクトリに新しいファイルを作成し、次 `CalendarController.php` のコードを追加します。

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

        $viewData['dateRange'] = $startOfWeek->format('M j, Y').' - '.$endOfWeek->format('M j, Y');

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
    - パラメーター `startDateTime` は `endDateTime` 、ビューの開始と終了を定義します。
    - この `$select` パラメーターは、各イベントで返されるフィールドを、ビューが実際に使用するフィールドに限定します。
    - パラメーターは、作成された日時で結果を並べ替え、最新のアイテム `$orderby` が最初に表示されます。
    - パラメーター `$top` は、結果を 25 イベントに制限します。
    - ヘッダーにより、応答の開始時刻と終了時刻がユーザーの優先タイム ゾーン `Prefer: outlook.timezone=""` に調整されます。

1. **./routes/web.php** のルートを更新して、この新しいコントローラーにルートを追加します。

    ```php
    Route::get('/calendar', 'CalendarController@calendar');
    ```

1. サインインし、ナビゲーション バー **の [予定表** ] リンクをクリックします。 すべてが正常に機能していれば、ユーザーのカレンダーにイベントの JSON ダンプが表示されます。

## <a name="display-the-results"></a>結果の表示

結果を表示するとき、よりユーザー フレンドリなビューを追加できます。

1. **./resources/views** ディレクトリに新しいファイルを作成し、 `calendar.blade.php` 次のコードを追加します。

    :::code language="php" source="../demo/graph-tutorial/resources/views/calendar.blade.php" id="CalendarSnippet":::

    これにより、イベントのコレクションがループされ、各イベントにテーブル行が追加されます。

1. **./routes/web.php** のルートを更新して、次のルートを追加します `/calendar/new` 。 これらの関数は次のセクションで実装しますが **、calendar.blade.php** が参照するルートを定義する必要があります。

    ```php
    Route::get('/calendar/new', 'CalendarController@getNewEventForm');
    Route::post('/calendar/new', 'CalendarController@createNewEvent');
    ```

1. `return response()->json($events);` `calendar` **./app/Http/Controllers/CalendarController.php** のアクションから行を削除し、次のコードに置き換えます。

    ```php
    $viewData['events'] = $events;
    return view('calendar', $viewData);
    ```

1. ページを更新すると、アプリはイベントのテーブルをレンダリングする必要があります。

    ![イベント表のスクリーンショット](./images/add-msgraph-01.png)
