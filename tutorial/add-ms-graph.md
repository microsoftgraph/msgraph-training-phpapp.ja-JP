<!-- markdownlint-disable MD002 MD041 -->

この演習では、Microsoft Graph をアプリケーションに組み込みます。 このアプリケーションでは、microsoft graph ライブラリ[](https://github.com/microsoftgraph/msgraph-sdk-php)を使用して microsoft Graph への呼び出しを行います。

## <a name="get-calendar-events-from-outlook"></a>Outlook から予定表のイベントを取得する

最初に、[カレンダー] ビューのコントローラーを追加してみましょう。 という名前`./app/Http/Controllers` `CalendarController.php`のフォルダーに新しいファイルを作成し、次のコードを追加します。

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

このコードの内容を検討してください。

- 呼び出し先の URL は`/v1.0/me/events`になります。
- パラメーター `$select`は、各イベントに対して返されるフィールドを、ビューが実際に使用するものだけに制限します。
- パラメーター `$orderby`は、生成された日付と時刻で結果を並べ替えます。最新のアイテムが最初に表示されます。

の`./routes/web.php`ルートを更新して、この新しいコントローラーにルートを追加する

```php
Route::get('/calendar', 'CalendarController@calendar');
```

これで、これをテストできます。 サインインして、ナビゲーションバーの [**予定表**] リンクをクリックします。 すべてが動作する場合は、ユーザーの予定表にイベントの JSON ダンプが表示されます。

## <a name="display-the-results"></a>結果を表示する

これで、ビューを追加して、よりわかりやすい方法で結果を表示することができます。 という名前`./resources/views` `calendar.blade.php`のディレクトリに新しいファイルを作成し、次のコードを追加します。

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

これにより、イベントのコレクションをループ処理して、テーブル行を1つずつ追加します。 `calendar`の`./app/Http/Controllers/CalendarController.php`アクション`return response()->json($events);`から行を削除し、次のコードに置き換えます。

```php
$viewData['events'] = $events;
return view('calendar', $viewData);
```

ページを更新すると、アプリがイベントの表を表示するようになります。

![イベントの表のスクリーンショット](./images/add-msgraph-01.png)