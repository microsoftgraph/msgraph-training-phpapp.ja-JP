<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="4d624-101">最初に、新しい Laravel プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="4d624-101">Begin by creating a new Laravel project.</span></span>

1. <span data-ttu-id="4d624-102">コマンドラインインターフェイス (CLI) を開き、ファイルを作成する権限があるディレクトリに移動し、次のコマンドを実行して新しい PHP アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="4d624-102">Open your command-line interface (CLI), navigate to a directory where you have rights to create files, and run the following command to create a new PHP app.</span></span>

    ```Shell
    laravel new graph-tutorial
    ```

1. <span data-ttu-id="4d624-103">**Graph のチュートリアル**ディレクトリに移動し、次のコマンドを入力してローカル web サーバーを開始します。</span><span class="sxs-lookup"><span data-stu-id="4d624-103">Navigate to the **graph-tutorial** directory and enter the following command to start a local web server.</span></span>

    ```Shell
    php artisan serve
    ```

1. <span data-ttu-id="4d624-104">ブラウザーを開き、`http://localhost:8000` に移動します。</span><span class="sxs-lookup"><span data-stu-id="4d624-104">Open your browser and navigate to `http://localhost:8000`.</span></span> <span data-ttu-id="4d624-105">すべてが動作している場合は、既定の Laravel ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="4d624-105">If everything is working, you will see a default Laravel page.</span></span> <span data-ttu-id="4d624-106">そのページが表示されない場合は、 [Laravel のドキュメント](https://laravel.com/docs/7.x)を確認してください。</span><span class="sxs-lookup"><span data-stu-id="4d624-106">If you don't see that page, check the [Laravel docs](https://laravel.com/docs/7.x).</span></span>

## <a name="install-packages"></a><span data-ttu-id="4d624-107">パッケージをインストールする</span><span class="sxs-lookup"><span data-stu-id="4d624-107">Install packages</span></span>

<span data-ttu-id="4d624-108">に進む前に、後で使用する追加のパッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="4d624-108">Before moving on, install some additional packages that you will use later:</span></span>

- <span data-ttu-id="4d624-109">[oauth2-](https://github.com/thephpleague/oauth2-client)サインインおよび OAuth トークンフローを処理するためのクライアントです。</span><span class="sxs-lookup"><span data-stu-id="4d624-109">[oauth2-client](https://github.com/thephpleague/oauth2-client) for handling sign-in and OAuth token flows.</span></span>
- <span data-ttu-id="4d624-110">microsoft graph に電話をかけるための[グラフ](https://github.com/microsoftgraph/msgraph-sdk-php)です。</span><span class="sxs-lookup"><span data-stu-id="4d624-110">[microsoft-graph](https://github.com/microsoftgraph/msgraph-sdk-php) for making calls to Microsoft Graph.</span></span>

1. <span data-ttu-id="4d624-111">CLI で次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="4d624-111">Run the following command in your CLI.</span></span>

    ```Shell
    composer require league/oauth2-client microsoft/microsoft-graph
    ```

## <a name="design-the-app"></a><span data-ttu-id="4d624-112">アプリを設計する</span><span class="sxs-lookup"><span data-stu-id="4d624-112">Design the app</span></span>

1. <span data-ttu-id="4d624-113">という名前`layout.blade.php`の **./resources/views**ディレクトリに新しいファイルを作成し、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="4d624-113">Create a new file in the **./resources/views** directory named `layout.blade.php` and add the following code.</span></span>

    :::code language="php" source="../demo/graph-tutorial/resources/views/layout.blade.php" id="LayoutSnippet":::

    <span data-ttu-id="4d624-114">このコードはシンプルなスタイルのために [Bootstrap](http://getbootstrap.com/) を追加し、シンプルなアイコンのために [Font Awesome](https://fontawesome.com/) を追加します。</span><span class="sxs-lookup"><span data-stu-id="4d624-114">This code adds [Bootstrap](http://getbootstrap.com/) for simple styling, and [Font Awesome](https://fontawesome.com/) for some simple icons.</span></span> <span data-ttu-id="4d624-115">また、ナビゲーションバーのあるグローバルレイアウトを定義します。</span><span class="sxs-lookup"><span data-stu-id="4d624-115">It also defines a global layout with a nav bar.</span></span>

1. <span data-ttu-id="4d624-116">と`./public`いう名前`css`のディレクトリに新しいディレクトリを作成し、という名前`./public/css` `app.css`のディレクトリに新しいファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="4d624-116">Create a new directory in the `./public` directory named `css`, then create a new file in the `./public/css` directory named `app.css`.</span></span> <span data-ttu-id="4d624-117">次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="4d624-117">Add the following code.</span></span>

    :::code language="css" source="../demo/graph-tutorial/public/css/app.css":::

1. <span data-ttu-id="4d624-118">`./resources/views/welcome.blade.php`ファイルを開き、その内容を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="4d624-118">Open the `./resources/views/welcome.blade.php` file and replace its contents with the following.</span></span>

    :::code language="php" source="../demo/graph-tutorial/resources/views/welcome.blade.php" id="WelcomeSnippet":::

1. <span data-ttu-id="4d624-119">次の関数`Controller`をクラスに追加して、/app/Http/Controllers/Controller.php の基本クラスを更新します **。**</span><span class="sxs-lookup"><span data-stu-id="4d624-119">Update the base `Controller` class in **./app/Http/Controllers/Controller.php** by adding the following function to the class.</span></span>

    :::code language="php" source="../demo/graph-tutorial/app/Http/Controllers/Controller.php" id="LoadViewDataSnippet":::

1. <span data-ttu-id="4d624-120">という名前`./app/Http/Controllers` `HomeController.php`のディレクトリに新しいファイルを作成し、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="4d624-120">Create a new file in the `./app/Http/Controllers` directory named `HomeController.php` and add the following code.</span></span>

    :::code language="php" source="../demo/graph-tutorial/app/Http/Controllers/HomeController.php":::

1. <span data-ttu-id="4d624-121">の`./routes/web.php`ルートを更新して、新しいコントローラーを使用します。</span><span class="sxs-lookup"><span data-stu-id="4d624-121">Update the route in `./routes/web.php` to use the new controller.</span></span> <span data-ttu-id="4d624-122">このファイルの内容全体を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="4d624-122">Replace the entire contents of this file with the following.</span></span>

    ```php
    <?php

    use Illuminate\Support\Facades\Route;

    Route::get('/', 'HomeController@welcome');
    ```

1. <span data-ttu-id="4d624-123">変更内容をすべて保存し、サーバーを再起動します。</span><span class="sxs-lookup"><span data-stu-id="4d624-123">Save all of your changes and restart the server.</span></span> <span data-ttu-id="4d624-124">この時点で、アプリの外観は大きく異なります。</span><span class="sxs-lookup"><span data-stu-id="4d624-124">Now, the app should look very different.</span></span>

    ![デザインが変更されたホーム ページのスクリーンショット](./images/create-app-01.png)
