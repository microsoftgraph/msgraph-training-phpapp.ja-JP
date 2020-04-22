<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="b3514-101">この演習では、Microsoft Graph をアプリケーションに組み込みます。</span><span class="sxs-lookup"><span data-stu-id="b3514-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="b3514-102">このアプリケーションでは、microsoft [graph ライブラリを使用して Microsoft](https://github.com/microsoftgraph/msgraph-sdk-php) graph への呼び出しを行います。</span><span class="sxs-lookup"><span data-stu-id="b3514-102">For this application, you will use the [microsoft-graph](https://github.com/microsoftgraph/msgraph-sdk-php) library to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="b3514-103">Outlook からカレンダー イベントを取得する</span><span class="sxs-lookup"><span data-stu-id="b3514-103">Get calendar events from Outlook</span></span>

1. <span data-ttu-id="b3514-104">という名前`CalendarController.php`の **/app/Http/Controllers**ディレクトリに新しいファイルを作成し、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="b3514-104">Create a new file in the **./app/Http/Controllers** directory named `CalendarController.php`, and add the following code.</span></span>

    ```php
    <?php

    namespace App\Http\Controllers;

    use App\Http\Controllers\Controller;
    use Illuminate\Http\Request;
    use Microsoft\Graph\Graph;
    use Microsoft\Graph\Model;
    use App\TokenStore\TokenCache;

    class CalendarController extends Controller
    {
      public function calendar()
      {
        $viewData = $this->loadViewData();

        // Get the access token from the cache
        $tokenCache = new TokenCache();
        $accessToken = $tokenCache->getAccessToken();

        // Create a Graph client
        $graph = new Graph();
        $graph->setAccessToken($accessToken);

        $queryParams = array(
          '$select' => 'subject,organizer,start,end',
          '$orderby' => 'createdDateTime DESC'
        );

        // Append query parameters to the '/me/events' url
        $getEventsUrl = '/me/events?'.http_build_query($queryParams);

        $events = $graph->createRequest('GET', $getEventsUrl)
          ->setReturnType(Model\Event::class)
          ->execute();

        return response()->json($events);
      }
    }
    ```

    <span data-ttu-id="b3514-105">このコードの実行内容を考えましょう。</span><span class="sxs-lookup"><span data-stu-id="b3514-105">Consider what this code is doing.</span></span>

    - <span data-ttu-id="b3514-106">呼び出される URL は `/v1.0/me/events` です。</span><span class="sxs-lookup"><span data-stu-id="b3514-106">The URL that will be called is `/v1.0/me/events`.</span></span>
    - <span data-ttu-id="b3514-107">パラメーター `$select`は、各イベントに対して返されるフィールドを、ビューが実際に使用するものだけに制限します。</span><span class="sxs-lookup"><span data-stu-id="b3514-107">The `$select` parameter limits the fields returned for each events to just those the view will actually use.</span></span>
    - <span data-ttu-id="b3514-108">パラメーター `$orderby`は、生成された日付と時刻で結果を並べ替えます。最新のアイテムが最初に表示されます。</span><span class="sxs-lookup"><span data-stu-id="b3514-108">The `$orderby` parameter sorts the results by the date and time they were created, with the most recent item being first.</span></span>

1. <span data-ttu-id="b3514-109">**/Routes/web.php**のルートを更新して、この新しいコントローラーにルートを追加します。</span><span class="sxs-lookup"><span data-stu-id="b3514-109">Update the routes in **./routes/web.php** to add a route to this new controller.</span></span>

    ```php
    Route::get('/calendar', 'CalendarController@calendar');
    ```

1. <span data-ttu-id="b3514-110">サインインして、ナビゲーションバーの [**予定表**] リンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="b3514-110">Sign in and click the **Calendar** link in the nav bar.</span></span> <span data-ttu-id="b3514-111">すべてが正常に機能していれば、ユーザーのカレンダーにイベントの JSON ダンプが表示されます。</span><span class="sxs-lookup"><span data-stu-id="b3514-111">If everything works, you should see a JSON dump of events on the user's calendar.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="b3514-112">結果の表示</span><span class="sxs-lookup"><span data-stu-id="b3514-112">Display the results</span></span>

<span data-ttu-id="b3514-113">結果を表示するとき、よりユーザー フレンドリなビューを追加できます。</span><span class="sxs-lookup"><span data-stu-id="b3514-113">Now you can add a view to display the results in a more user-friendly manner.</span></span>

1. <span data-ttu-id="b3514-114">という名前`calendar.blade.php`の **./resources/views**ディレクトリに新しいファイルを作成し、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="b3514-114">Create a new file in the **./resources/views** directory named `calendar.blade.php` and add the following code.</span></span>

    :::code language="php" source="../demo/graph-tutorial/resources/views/calendar.blade.php" id="CalendarSnippet":::

    <span data-ttu-id="b3514-115">これにより、イベントのコレクションがループされ、各イベントにテーブル行が追加されます。</span><span class="sxs-lookup"><span data-stu-id="b3514-115">That will loop through a collection of events and add a table row for each one.</span></span>

1. <span data-ttu-id="b3514-116">/App/Http/Controllers/CalendarController.php の`return response()->json($events);` `calendar`アクションから行を削除 **./app/Http/Controllers/CalendarController.php**し、次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="b3514-116">Remove the `return response()->json($events);` line from the `calendar` action in **./app/Http/Controllers/CalendarController.php**, and replace it with the following code.</span></span>

    ```php
    $viewData['events'] = $events;
    return view('calendar', $viewData);
    ```

1. <span data-ttu-id="b3514-117">ページを更新すると、アプリがイベントの表を表示するようになります。</span><span class="sxs-lookup"><span data-stu-id="b3514-117">Refresh the page and the app should now render a table of events.</span></span>

    ![イベント表のスクリーンショット](./images/add-msgraph-01.png)
