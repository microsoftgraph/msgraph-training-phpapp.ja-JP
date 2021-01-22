<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="728d8-101">このセクションでは、ユーザーの予定表にイベントを作成する機能を追加します。</span><span class="sxs-lookup"><span data-stu-id="728d8-101">In this section you will add the ability to create events on the user's calendar.</span></span>

## <a name="create-new-event-form"></a><span data-ttu-id="728d8-102">新しいイベント フォームを作成する</span><span class="sxs-lookup"><span data-stu-id="728d8-102">Create new event form</span></span>

1. <span data-ttu-id="728d8-103">**./resources/views** ディレクトリに新しいファイルを作成し、 `newevent.blade.php` 次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="728d8-103">Create a new file in the **./resources/views** directory named `newevent.blade.php` and add the following code.</span></span>

    :::code language="php" source="../demo/graph-tutorial/resources/views/newevent.blade.php" id="NewEventFormSnippet":::

## <a name="add-controller-actions"></a><span data-ttu-id="728d8-104">コントローラーアクションを追加する</span><span class="sxs-lookup"><span data-stu-id="728d8-104">Add controller actions</span></span>

1. <span data-ttu-id="728d8-105">**./app/Http/Controllers/CalendarController.php** を開き、次の関数を追加してフォームをレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="728d8-105">Open **./app/Http/Controllers/CalendarController.php** and add the following function to render the form.</span></span>

    :::code language="php" source="../demo/graph-tutorial/app/Http/Controllers/CalendarController.php" id="getNewEventFormSnippet":::

1. <span data-ttu-id="728d8-106">次の関数を追加して、ユーザーの送信時にフォーム データを受信し、ユーザーの予定表に新しいイベントを作成します。</span><span class="sxs-lookup"><span data-stu-id="728d8-106">Add the following function to receive the form data when the user's submits, and create a new event on the user's calendar.</span></span>

    :::code language="php" source="../demo/graph-tutorial/app/Http/Controllers/CalendarController.php" id="createNewEventSnippet":::

    <span data-ttu-id="728d8-107">このコードの動作を検討します。</span><span class="sxs-lookup"><span data-stu-id="728d8-107">Consider what this code does.</span></span>

    - <span data-ttu-id="728d8-108">出席者フィールドの入力を Graph 出席者オブジェクトの配列に [変換](https://docs.microsoft.com/graph/api/resources/attendee?view=graph-rest-1.0) します。</span><span class="sxs-lookup"><span data-stu-id="728d8-108">It converts the attendees field input to an array of Graph [attendee](https://docs.microsoft.com/graph/api/resources/attendee?view=graph-rest-1.0) objects.</span></span>
    - <span data-ttu-id="728d8-109">フォーム入力から [イベント](https://docs.microsoft.com/graph/api/resources/event?view=graph-rest-1.0) を作成します。</span><span class="sxs-lookup"><span data-stu-id="728d8-109">It builds an [event](https://docs.microsoft.com/graph/api/resources/event?view=graph-rest-1.0) from the form input.</span></span>
    - <span data-ttu-id="728d8-110">エンドポイントに POST を送信 `/me/events` し、予定表ビューにリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="728d8-110">It sends a POST to the `/me/events` endpoint, then redirects back to the calendar view.</span></span>

1. <span data-ttu-id="728d8-111">変更内容をすべて保存し、サーバーを再起動します。</span><span class="sxs-lookup"><span data-stu-id="728d8-111">Save all of your changes and restart the server.</span></span> <span data-ttu-id="728d8-112">[新 **しいイベント] ボタン** を使用して、新しいイベント フォームに移動します。</span><span class="sxs-lookup"><span data-stu-id="728d8-112">Use the **New event** button to navigate to the new event form.</span></span>

1. <span data-ttu-id="728d8-113">フォームの値を入力します。</span><span class="sxs-lookup"><span data-stu-id="728d8-113">Fill in the values on the form.</span></span> <span data-ttu-id="728d8-114">現在の週の開始日を使用します。</span><span class="sxs-lookup"><span data-stu-id="728d8-114">Use a start date from the current week.</span></span> <span data-ttu-id="728d8-115">[**作成**] を選択します。</span><span class="sxs-lookup"><span data-stu-id="728d8-115">Select **Create**.</span></span>

    ![新しいイベント フォームのスクリーンショット](images/create-event-01.png)

1. <span data-ttu-id="728d8-117">アプリが予定表ビューにリダイレクトするときに、新しいイベントが結果に表示されるのを確認します。</span><span class="sxs-lookup"><span data-stu-id="728d8-117">When the app redirects to the calendar view, verify that your new event is present in the results.</span></span>
