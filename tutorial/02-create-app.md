<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="aebc7-101">まず、新しい Laravel プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="aebc7-101">Begin by creating a new Laravel project.</span></span>

1. <span data-ttu-id="aebc7-102">コマンドライン インターフェイス (CLI) を開き、ファイルを作成する権限を持つディレクトリに移動し、次のコマンドを実行して新しい PHP アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="aebc7-102">Open your command-line interface (CLI), navigate to a directory where you have rights to create files, and run the following command to create a new PHP app.</span></span>

    ```Shell
    laravel new graph-tutorial
    ```

1. <span data-ttu-id="aebc7-103">**graph-tutorial ディレクトリに移動し**、次のコマンドを入力してローカル Web サーバーを起動します。</span><span class="sxs-lookup"><span data-stu-id="aebc7-103">Navigate to the **graph-tutorial** directory and enter the following command to start a local web server.</span></span>

    ```Shell
    php artisan serve
    ```

1. <span data-ttu-id="aebc7-104">ブラウザーを開き、`http://localhost:8000` に移動します。</span><span class="sxs-lookup"><span data-stu-id="aebc7-104">Open your browser and navigate to `http://localhost:8000`.</span></span> <span data-ttu-id="aebc7-105">すべてが動作している場合は、既定の Laravel ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="aebc7-105">If everything is working, you will see a default Laravel page.</span></span> <span data-ttu-id="aebc7-106">If you don't see that page, check the [Laravel docs](https://laravel.com/docs/8.x).</span><span class="sxs-lookup"><span data-stu-id="aebc7-106">If you don't see that page, check the [Laravel docs](https://laravel.com/docs/8.x).</span></span>

## <a name="install-packages"></a><span data-ttu-id="aebc7-107">パッケージをインストールする</span><span class="sxs-lookup"><span data-stu-id="aebc7-107">Install packages</span></span>

<span data-ttu-id="aebc7-108">次に進む前に、後で使用する追加のパッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="aebc7-108">Before moving on, install some additional packages that you will use later:</span></span>

- <span data-ttu-id="aebc7-109">[サインインおよび OAuth](https://github.com/thephpleague/oauth2-client) トークン フローを処理する oauth2-client。</span><span class="sxs-lookup"><span data-stu-id="aebc7-109">[oauth2-client](https://github.com/thephpleague/oauth2-client) for handling sign-in and OAuth token flows.</span></span>
- <span data-ttu-id="aebc7-110">Microsoft Graph を呼び出す[microsoft-graph。](https://github.com/microsoftgraph/msgraph-sdk-php)</span><span class="sxs-lookup"><span data-stu-id="aebc7-110">[microsoft-graph](https://github.com/microsoftgraph/msgraph-sdk-php) for making calls to Microsoft Graph.</span></span>

1. <span data-ttu-id="aebc7-111">CLI で次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="aebc7-111">Run the following command in your CLI.</span></span>

    ```Shell
    composer require league/oauth2-client microsoft/microsoft-graph
    ```

## <a name="design-the-app"></a><span data-ttu-id="aebc7-112">アプリを設計する</span><span class="sxs-lookup"><span data-stu-id="aebc7-112">Design the app</span></span>

1. <span data-ttu-id="aebc7-113">**./resources/views** ディレクトリに新しいファイルを作成し、 `layout.blade.php` 次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="aebc7-113">Create a new file in the **./resources/views** directory named `layout.blade.php` and add the following code.</span></span>

    :::code language="php" source="../demo/graph-tutorial/resources/views/layout.blade.php" id="LayoutSnippet":::

    <span data-ttu-id="aebc7-114">このコードはシンプルなスタイルのために [Bootstrap](http://getbootstrap.com/) を追加し、シンプルなアイコンのために [Font Awesome](https://fontawesome.com/) を追加します。</span><span class="sxs-lookup"><span data-stu-id="aebc7-114">This code adds [Bootstrap](http://getbootstrap.com/) for simple styling, and [Font Awesome](https://fontawesome.com/) for some simple icons.</span></span> <span data-ttu-id="aebc7-115">また、ナビゲーション バーを含むグローバル レイアウトも定義します。</span><span class="sxs-lookup"><span data-stu-id="aebc7-115">It also defines a global layout with a nav bar.</span></span>

1. <span data-ttu-id="aebc7-116">ディレクトリに新しいディレクトリを `./public` 作成し、そのディレクトリに新しい `css` ファイル `./public/css` を作成します `app.css` 。</span><span class="sxs-lookup"><span data-stu-id="aebc7-116">Create a new directory in the `./public` directory named `css`, then create a new file in the `./public/css` directory named `app.css`.</span></span> <span data-ttu-id="aebc7-117">次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="aebc7-117">Add the following code.</span></span>

    :::code language="css" source="../demo/graph-tutorial/public/css/app.css":::

1. <span data-ttu-id="aebc7-118">ファイルを `./resources/views/welcome.blade.php` 開き、その内容を次の内容に置き換えてください。</span><span class="sxs-lookup"><span data-stu-id="aebc7-118">Open the `./resources/views/welcome.blade.php` file and replace its contents with the following.</span></span>

    :::code language="php" source="../demo/graph-tutorial/resources/views/welcome.blade.php" id="WelcomeSnippet":::

1. <span data-ttu-id="aebc7-119">クラスに次の関数を追加して `Controller` **、./app/Http/Controllers/Controller.php** の基本クラスを更新します。</span><span class="sxs-lookup"><span data-stu-id="aebc7-119">Update the base `Controller` class in **./app/Http/Controllers/Controller.php** by adding the following function to the class.</span></span>

    :::code language="php" source="../demo/graph-tutorial/app/Http/Controllers/Controller.php" id="LoadViewDataSnippet":::

1. <span data-ttu-id="aebc7-120">ディレクトリに新しいファイルを `./app/Http/Controllers` 作成し、 `HomeController.php` 次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="aebc7-120">Create a new file in the `./app/Http/Controllers` directory named `HomeController.php` and add the following code.</span></span>

    :::code language="php" source="../demo/graph-tutorial/app/Http/Controllers/HomeController.php":::

1. <span data-ttu-id="aebc7-121">新しいコントローラーを使用 `./routes/web.php` するためにルートを更新します。</span><span class="sxs-lookup"><span data-stu-id="aebc7-121">Update the route in `./routes/web.php` to use the new controller.</span></span> <span data-ttu-id="aebc7-122">このファイルの内容全体を次の内容に置き換えてください。</span><span class="sxs-lookup"><span data-stu-id="aebc7-122">Replace the entire contents of this file with the following.</span></span>

    ```php
    <?php

    use Illuminate\Support\Facades\Route;

    Route::get('/', 'HomeController@welcome');
    ```

1. <span data-ttu-id="aebc7-123">**./app/Providers/RouteServiceProvider.php** を開き、宣言のコミットを解除 `$namespace` します。</span><span class="sxs-lookup"><span data-stu-id="aebc7-123">Open **./app/Providers/RouteServiceProvider.php** and uncomment the `$namespace` declaration.</span></span>

    ```php
    /**
     * This namespace is applied to your controller routes.
     *
     * In addition, it is set as the URL generator's root namespace.
     *
     * @var string
     */
    protected $namespace = 'App\Http\Controllers';
    ```

1. <span data-ttu-id="aebc7-124">変更内容をすべて保存し、サーバーを再起動します。</span><span class="sxs-lookup"><span data-stu-id="aebc7-124">Save all of your changes and restart the server.</span></span> <span data-ttu-id="aebc7-125">これで、アプリは非常に異なって表示されます。</span><span class="sxs-lookup"><span data-stu-id="aebc7-125">Now, the app should look very different.</span></span>

    ![デザインが変更されたホーム ページのスクリーンショット](./images/create-app-01.png)
