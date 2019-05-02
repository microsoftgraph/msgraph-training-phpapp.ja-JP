<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="68b2a-101">この演習では、Microsoft Graph をアプリケーションに組み込みます。</span><span class="sxs-lookup"><span data-stu-id="68b2a-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="68b2a-102">このアプリケーションでは、microsoft graph ライブラリ[](https://github.com/microsoftgraph/msgraph-sdk-php)を使用して microsoft Graph への呼び出しを行います。</span><span class="sxs-lookup"><span data-stu-id="68b2a-102">For this application, you will use the [microsoft-graph](https://github.com/microsoftgraph/msgraph-sdk-php) library to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="68b2a-103">Outlook から予定表のイベントを取得する</span><span class="sxs-lookup"><span data-stu-id="68b2a-103">Get calendar events from Outlook</span></span>

<span data-ttu-id="68b2a-104">最初に、[カレンダー] ビューのコントローラーを追加してみましょう。</span><span class="sxs-lookup"><span data-stu-id="68b2a-104">Let's start by adding a controller for the calendar view.</span></span> <span data-ttu-id="68b2a-105">という名前`./app/Http/Controllers` `CalendarController.php`のフォルダーに新しいファイルを作成し、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="68b2a-105">Create a new file in the `./app/Http/Controllers` folder named `CalendarController.php`, and add the following code.</span></span>

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

<span data-ttu-id="68b2a-106">このコードの内容を検討してください。</span><span class="sxs-lookup"><span data-stu-id="68b2a-106">Consider what this code is doing.</span></span>

- <span data-ttu-id="68b2a-107">呼び出し先の URL は`/v1.0/me/events`になります。</span><span class="sxs-lookup"><span data-stu-id="68b2a-107">The URL that will be called is `/v1.0/me/events`.</span></span>
- <span data-ttu-id="68b2a-108">パラメーター `$select`は、各イベントに対して返されるフィールドを、ビューが実際に使用するものだけに制限します。</span><span class="sxs-lookup"><span data-stu-id="68b2a-108">The `$select` parameter limits the fields returned for each events to just those the view will actually use.</span></span>
- <span data-ttu-id="68b2a-109">パラメーター `$orderby`は、生成された日付と時刻で結果を並べ替えます。最新のアイテムが最初に表示されます。</span><span class="sxs-lookup"><span data-stu-id="68b2a-109">The `$orderby` parameter sorts the results by the date and time they were created, with the most recent item being first.</span></span>

<span data-ttu-id="68b2a-110">の`./routes/web.php`ルートを更新して、この新しいコントローラーにルートを追加する</span><span class="sxs-lookup"><span data-stu-id="68b2a-110">Update the routes in `./routes/web.php` to add a route to this new controller</span></span>

```php
Route::get('/calendar', 'CalendarController@calendar');
```

<span data-ttu-id="68b2a-111">これで、これをテストできます。</span><span class="sxs-lookup"><span data-stu-id="68b2a-111">Now you can test this.</span></span> <span data-ttu-id="68b2a-112">サインインして、ナビゲーションバーの [**予定表**] リンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="68b2a-112">Sign in and click the **Calendar** link in the nav bar.</span></span> <span data-ttu-id="68b2a-113">すべてが動作する場合は、ユーザーの予定表にイベントの JSON ダンプが表示されます。</span><span class="sxs-lookup"><span data-stu-id="68b2a-113">If everything works, you should see a JSON dump of events on the user's calendar.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="68b2a-114">結果を表示する</span><span class="sxs-lookup"><span data-stu-id="68b2a-114">Display the results</span></span>

<span data-ttu-id="68b2a-115">これで、ビューを追加して、よりわかりやすい方法で結果を表示することができます。</span><span class="sxs-lookup"><span data-stu-id="68b2a-115">Now you can add a view to display the results in a more user-friendly manner.</span></span> <span data-ttu-id="68b2a-116">という名前`./resources/views` `calendar.blade.php`のディレクトリに新しいファイルを作成し、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="68b2a-116">Create a new file in the `./resources/views` directory named `calendar.blade.php` and add the following code.</span></span>

```php
@extends('layout')

@section('content')
<h1>Calendar</h1>
<table class="table">
  <thead>
    <tr>
      <th scope="col">Organizer</th>
      <th scope="col">Subject</th>
      <th scope="col">Start</th>
      <th scope="col">End</th>
    </tr>
  </thead>
  <tbody>
    @isset($events)
      @foreach($events as $event)
        <tr>
          <td>{{ $event->getOrganizer()->getEmailAddress()->getName() }}</td>
          <td>{{ $event->getSubject() }}</td>
          <td>{{ \Carbon\Carbon::parse($event->getStart()->getDateTime())->format('n/j/y g:i A') }}</td>
          <td>{{ \Carbon\Carbon::parse($event->getEnd()->getDateTime())->format('n/j/y g:i A') }}</td>
        </tr>
      @endforeach
    @endif
  </tbody>
</table>
@endsection
```

<span data-ttu-id="68b2a-117">これにより、イベントのコレクションをループ処理して、テーブル行を1つずつ追加します。</span><span class="sxs-lookup"><span data-stu-id="68b2a-117">That will loop through a collection of events and add a table row for each one.</span></span> <span data-ttu-id="68b2a-118">`calendar`の`./app/Http/Controllers/CalendarController.php`アクション`return response()->json($events);`から行を削除し、次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="68b2a-118">Remove the `return response()->json($events);` line from the `calendar` action in `./app/Http/Controllers/CalendarController.php`, and replace it with the following code.</span></span>

```php
$viewData['events'] = $events;
return view('calendar', $viewData);
```

<span data-ttu-id="68b2a-119">ページを更新すると、アプリがイベントの表を表示するようになります。</span><span class="sxs-lookup"><span data-stu-id="68b2a-119">Refresh the page and the app should now render a table of events.</span></span>

![イベントの表のスクリーンショット](./images/add-msgraph-01.png)