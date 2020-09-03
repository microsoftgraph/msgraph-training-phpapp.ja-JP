<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="63e4f-101">このセクションでは、ユーザーの予定表にイベントを作成する機能を追加します。</span><span class="sxs-lookup"><span data-stu-id="63e4f-101">In this section you will add the ability to create events on the user's calendar.</span></span>

## <a name="create-new-event-form"></a><span data-ttu-id="63e4f-102">新しいイベントフォームを作成する</span><span class="sxs-lookup"><span data-stu-id="63e4f-102">Create new event form</span></span>

1. <span data-ttu-id="63e4f-103">という名前の **./resources/views** ディレクトリに新しいファイルを作成 `newevent.blade.php` し、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="63e4f-103">Create a new file in the **./resources/views** directory named `newevent.blade.php` and add the following code.</span></span>

    :::code language="php" source="../demo/graph-tutorial/resources/views/newevent.blade.php" id="NewEventFormSnippet":::

## <a name="add-controller-actions"></a><span data-ttu-id="63e4f-104">コントローラーアクションを追加する</span><span class="sxs-lookup"><span data-stu-id="63e4f-104">Add controller actions</span></span>

1. <span data-ttu-id="63e4f-105">**/App/Http/Controllers/CalendarController.php**を開き、次の関数を追加してフォームを表示します。</span><span class="sxs-lookup"><span data-stu-id="63e4f-105">Open **./app/Http/Controllers/CalendarController.php** and add the following function to render the form.</span></span>

    :::code language="php" source="../demo/graph-tutorial/app/Http/Controllers/CalendarController.php" id="getNewEventFormSnippet":::

1. <span data-ttu-id="63e4f-106">ユーザーの送信時にフォームデータを受信し、ユーザーの予定表に新しいイベントを作成するために、次の関数を追加します。</span><span class="sxs-lookup"><span data-stu-id="63e4f-106">Add the following function to receive the form data when the user's submits, and create a new event on the user's calendar.</span></span>

    :::code language="php" source="../demo/graph-tutorial/app/Http/Controllers/CalendarController.php" id="createNewEventSnippet":::

    <span data-ttu-id="63e4f-107">このコードの内容を検討してください。</span><span class="sxs-lookup"><span data-stu-id="63e4f-107">Consider what this code does.</span></span>

    - <span data-ttu-id="63e4f-108">出席者フィールドの入力を Graph の [出席者](https://docs.microsoft.com/graph/api/resources/attendee?view=graph-rest-1.0) のオブジェクトの配列に変換します。</span><span class="sxs-lookup"><span data-stu-id="63e4f-108">It converts the attendees field input to an array of Graph [attendee](https://docs.microsoft.com/graph/api/resources/attendee?view=graph-rest-1.0) objects.</span></span>
    - <span data-ttu-id="63e4f-109">フォーム入力から [イベント](https://docs.microsoft.com/graph/api/resources/event?view=graph-rest-1.0) を作成します。</span><span class="sxs-lookup"><span data-stu-id="63e4f-109">It builds an [event](https://docs.microsoft.com/graph/api/resources/event?view=graph-rest-1.0) from the form input.</span></span>
    - <span data-ttu-id="63e4f-110">エンドポイントに投稿を送信 `/me/events` し、予定表ビューにリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="63e4f-110">It sends a POST to the `/me/events` endpoint, then redirects back to the calendar view.</span></span>

1. <span data-ttu-id="63e4f-111">**/Routes/web.php**のルートを更新して、コントローラー上でこれらの新しい機能のルートを追加します。</span><span class="sxs-lookup"><span data-stu-id="63e4f-111">Update the routes in **./routes/web.php** to add routes for these new functions on the controller.</span></span>

    ```php
    Route::get('/calendar/new', 'CalendarController@getNewEventForm');
    Route::post('/calendar/new', 'CalendarController@createNewEvent');
    ```

1. <span data-ttu-id="63e4f-112">変更内容をすべて保存し、サーバーを再起動します。</span><span class="sxs-lookup"><span data-stu-id="63e4f-112">Save all of your changes and restart the server.</span></span> <span data-ttu-id="63e4f-113">[新しい **イベント] ボタン** を使用して、新しいイベントフォームに移動します。</span><span class="sxs-lookup"><span data-stu-id="63e4f-113">Use the **New event** button to navigate to the new event form.</span></span>

1. <span data-ttu-id="63e4f-114">フォーム上の値を入力します。</span><span class="sxs-lookup"><span data-stu-id="63e4f-114">Fill in the values on the form.</span></span> <span data-ttu-id="63e4f-115">現在の週の開始日を使用します。</span><span class="sxs-lookup"><span data-stu-id="63e4f-115">Use a start date from the current week.</span></span> <span data-ttu-id="63e4f-116">**[作成]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="63e4f-116">Select **Create**.</span></span>

    ![新しいイベントフォームのスクリーンショット](images/create-event-01.png)

1. <span data-ttu-id="63e4f-118">アプリが [カレンダー] ビューにリダイレクトされたときに、新しいイベントが結果に存在することを確認します。</span><span class="sxs-lookup"><span data-stu-id="63e4f-118">When the app redirects to the calendar view, verify that your new event is present in the results.</span></span>
