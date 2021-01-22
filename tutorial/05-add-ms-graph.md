<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="d5806-101">この演習では、Microsoft Graph をアプリケーションに組み込む必要があります。</span><span class="sxs-lookup"><span data-stu-id="d5806-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="d5806-102">このアプリケーションでは [、Microsoft Graph ライブラリを使用](https://github.com/microsoftgraph/msgraph-sdk-php) して Microsoft Graph を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="d5806-102">For this application, you will use the [microsoft-graph](https://github.com/microsoftgraph/msgraph-sdk-php) library to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="d5806-103">Outlook からカレンダー イベントを取得する</span><span class="sxs-lookup"><span data-stu-id="d5806-103">Get calendar events from Outlook</span></span>

1. <span data-ttu-id="d5806-104">**./app** ディレクトリに新しいディレクトリを作成し、そのディレクトリに新しいファイルを作成し、次の `TimeZones` `TimeZones.php` コードを追加します。</span><span class="sxs-lookup"><span data-stu-id="d5806-104">Create a new directory in the **./app** directory named `TimeZones`, then create a new file in that directory named `TimeZones.php`, and add the following code.</span></span>

    :::code language="php" source="../demo/graph-tutorial/app/TimeZones/TimeZones.php":::

    <span data-ttu-id="d5806-105">このクラスは、Windows タイム ゾーン名と IANA タイム ゾーン識別子の単純なマッピングを実装します。</span><span class="sxs-lookup"><span data-stu-id="d5806-105">This class implements a simplistic mapping of Windows time zone names to IANA time zone identifiers.</span></span>

1. <span data-ttu-id="d5806-106">**./app/Http/Controllers** ディレクトリに新しいファイルを作成し、次 `CalendarController.php` のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="d5806-106">Create a new file in the **./app/Http/Controllers** directory named `CalendarController.php`, and add the following code.</span></span>

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

    <span data-ttu-id="d5806-107">このコードの実行内容を考えましょう。</span><span class="sxs-lookup"><span data-stu-id="d5806-107">Consider what this code is doing.</span></span>

    - <span data-ttu-id="d5806-108">呼び出される URL は `/v1.0/me/calendarView` です。</span><span class="sxs-lookup"><span data-stu-id="d5806-108">The URL that will be called is `/v1.0/me/calendarView`.</span></span>
    - <span data-ttu-id="d5806-109">パラメーター `startDateTime` は `endDateTime` 、ビューの開始と終了を定義します。</span><span class="sxs-lookup"><span data-stu-id="d5806-109">The `startDateTime` and `endDateTime` parameters define the start and end of the view.</span></span>
    - <span data-ttu-id="d5806-110">この `$select` パラメーターは、各イベントで返されるフィールドを、ビューが実際に使用するフィールドに限定します。</span><span class="sxs-lookup"><span data-stu-id="d5806-110">The `$select` parameter limits the fields returned for each events to just those the view will actually use.</span></span>
    - <span data-ttu-id="d5806-111">パラメーターは、作成された日時で結果を並べ替え、最新のアイテム `$orderby` が最初に表示されます。</span><span class="sxs-lookup"><span data-stu-id="d5806-111">The `$orderby` parameter sorts the results by the date and time they were created, with the most recent item being first.</span></span>
    - <span data-ttu-id="d5806-112">パラメーター `$top` は、結果を 25 イベントに制限します。</span><span class="sxs-lookup"><span data-stu-id="d5806-112">The `$top` parameter limits the results to 25 events.</span></span>
    - <span data-ttu-id="d5806-113">ヘッダーにより、応答の開始時刻と終了時刻がユーザーの優先タイム ゾーン `Prefer: outlook.timezone=""` に調整されます。</span><span class="sxs-lookup"><span data-stu-id="d5806-113">The `Prefer: outlook.timezone=""` header causes the start and end times in the response to be adjusted to the user's preferred time zone.</span></span>

1. <span data-ttu-id="d5806-114">**./routes/web.php** のルートを更新して、この新しいコントローラーにルートを追加します。</span><span class="sxs-lookup"><span data-stu-id="d5806-114">Update the routes in **./routes/web.php** to add a route to this new controller.</span></span>

    ```php
    Route::get('/calendar', 'CalendarController@calendar');
    ```

1. <span data-ttu-id="d5806-115">サインインし、ナビゲーション バー **の [予定表** ] リンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="d5806-115">Sign in and click the **Calendar** link in the nav bar.</span></span> <span data-ttu-id="d5806-116">すべてが正常に機能していれば、ユーザーのカレンダーにイベントの JSON ダンプが表示されます。</span><span class="sxs-lookup"><span data-stu-id="d5806-116">If everything works, you should see a JSON dump of events on the user's calendar.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="d5806-117">結果の表示</span><span class="sxs-lookup"><span data-stu-id="d5806-117">Display the results</span></span>

<span data-ttu-id="d5806-118">結果を表示するとき、よりユーザー フレンドリなビューを追加できます。</span><span class="sxs-lookup"><span data-stu-id="d5806-118">Now you can add a view to display the results in a more user-friendly manner.</span></span>

1. <span data-ttu-id="d5806-119">**./resources/views** ディレクトリに新しいファイルを作成し、 `calendar.blade.php` 次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="d5806-119">Create a new file in the **./resources/views** directory named `calendar.blade.php` and add the following code.</span></span>

    :::code language="php" source="../demo/graph-tutorial/resources/views/calendar.blade.php" id="CalendarSnippet":::

    <span data-ttu-id="d5806-120">これにより、イベントのコレクションがループされ、各イベントにテーブル行が追加されます。</span><span class="sxs-lookup"><span data-stu-id="d5806-120">That will loop through a collection of events and add a table row for each one.</span></span>

1. <span data-ttu-id="d5806-121">**./routes/web.php** のルートを更新して、次のルートを追加します `/calendar/new` 。</span><span class="sxs-lookup"><span data-stu-id="d5806-121">Update the routes in **./routes/web.php** to add routes for `/calendar/new`.</span></span> <span data-ttu-id="d5806-122">これらの関数は次のセクションで実装しますが **、calendar.blade.php** が参照するルートを定義する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d5806-122">You will implement these functions in the next section, but the route need to be defined now because **calendar.blade.php** references it.</span></span>

    ```php
    Route::get('/calendar/new', 'CalendarController@getNewEventForm');
    Route::post('/calendar/new', 'CalendarController@createNewEvent');
    ```

1. <span data-ttu-id="d5806-123">`return response()->json($events);` `calendar` **./app/Http/Controllers/CalendarController.php** のアクションから行を削除し、次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="d5806-123">Remove the `return response()->json($events);` line from the `calendar` action in **./app/Http/Controllers/CalendarController.php**, and replace it with the following code.</span></span>

    ```php
    $viewData['events'] = $events;
    return view('calendar', $viewData);
    ```

1. <span data-ttu-id="d5806-124">ページを更新すると、アプリはイベントのテーブルをレンダリングする必要があります。</span><span class="sxs-lookup"><span data-stu-id="d5806-124">Refresh the page and the app should now render a table of events.</span></span>

    ![イベント表のスクリーンショット](./images/add-msgraph-01.png)
