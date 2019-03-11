# <a name="how-to-run-the-completed-project"></a><span data-ttu-id="94508-101">完了したプロジェクトを実行する方法</span><span class="sxs-lookup"><span data-stu-id="94508-101">How to run the completed project</span></span>

## <a name="prerequisites"></a><span data-ttu-id="94508-102">前提条件</span><span class="sxs-lookup"><span data-stu-id="94508-102">Prerequisites</span></span>

<span data-ttu-id="94508-103">このフォルダーで完了したプロジェクトを実行するには、次のものが必要です。</span><span class="sxs-lookup"><span data-stu-id="94508-103">To run the completed project in this folder, you need the following:</span></span>

- <span data-ttu-id="94508-104">開発用コンピューターにインストールされている[PHP](http://php.net/downloads.php) 。</span><span class="sxs-lookup"><span data-stu-id="94508-104">[PHP](http://php.net/downloads.php) installed on your development machine.</span></span> <span data-ttu-id="94508-105">PHP がインストールされていない場合は、「ダウンロードオプション」の前のリンクを参照してください。</span><span class="sxs-lookup"><span data-stu-id="94508-105">If you do not have PHP, visit the previous link for download options.</span></span> <span data-ttu-id="94508-106">(**注:** このチュートリアルは、PHP バージョン7.2 で記述されています。</span><span class="sxs-lookup"><span data-stu-id="94508-106">(**Note:** This tutorial was written with PHP version 7.2.</span></span> <span data-ttu-id="94508-107">このガイドの手順は、他のバージョンでは動作しますが、テストされていません。)</span><span class="sxs-lookup"><span data-stu-id="94508-107">The steps in this guide may work with other versions, but that has not been tested.)</span></span>
- <span data-ttu-id="94508-108">開発用コンピューターにインストールされた[Composer](https://getcomposer.org/) 。</span><span class="sxs-lookup"><span data-stu-id="94508-108">[Composer](https://getcomposer.org/) installed on your development machine.</span></span>
- <span data-ttu-id="94508-109">開発用コンピューターにインストールされている[Laravel](https://laravel.com/) 。</span><span class="sxs-lookup"><span data-stu-id="94508-109">[Laravel](https://laravel.com/) installed on your development machine.</span></span>
- <span data-ttu-id="94508-110">Outlook.com 上のメールボックスを持つ個人の Microsoft アカウント、または microsoft 職場または学校のアカウントのいずれか。</span><span class="sxs-lookup"><span data-stu-id="94508-110">Either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span>

<span data-ttu-id="94508-111">Microsoft アカウントを持っていない場合は、無料のアカウントを取得するためのオプションがいくつかあります。</span><span class="sxs-lookup"><span data-stu-id="94508-111">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="94508-112">[新しい個人用 Microsoft アカウントにサインアップ](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1)することができます。</span><span class="sxs-lookup"><span data-stu-id="94508-112">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="94508-113">[office 365 開発者プログラムにサインアップ](https://developer.microsoft.com/office/dev-program)して、無料の office 365 サブスクリプションを取得することができます。</span><span class="sxs-lookup"><span data-stu-id="94508-113">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

## <a name="register-a-web-application-with-the-application-registration-portal"></a><span data-ttu-id="94508-114">web アプリケーションをアプリケーション登録ポータルに登録する</span><span class="sxs-lookup"><span data-stu-id="94508-114">Register a web application with the Application Registration Portal</span></span>

1. <span data-ttu-id="94508-115">ブラウザーを開き、[アプリケーション登録ポータル](https://apps.dev.microsoft.com)に移動します。</span><span class="sxs-lookup"><span data-stu-id="94508-115">Open a browser and navigate to the [Application Registration Portal](https://apps.dev.microsoft.com).</span></span> <span data-ttu-id="94508-116">**個人アカウント**(別名: Microsoft アカウント) または**職場または学校のアカウント**を使用してログインします。</span><span class="sxs-lookup"><span data-stu-id="94508-116">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="94508-117">ページの上部にある [**アプリの追加**] を選択します。</span><span class="sxs-lookup"><span data-stu-id="94508-117">Select **Add an app** at the top of the page.</span></span>

    > <span data-ttu-id="94508-118">**注:** ページに [**アプリの追加**] ボタンが複数表示されている場合は、[収束した**アプリ**] リストに対応するボタンを選択します。</span><span class="sxs-lookup"><span data-stu-id="94508-118">**Note:** If you see more than one **Add an app** button on the page, select the one that corresponds to the **Converged apps** list.</span></span>

1. <span data-ttu-id="94508-119">[**アプリケーションの登録**] ページで、[**アプリケーション名**] を [ **PHP Graph のチュートリアル**] に設定し、[**作成**] を選択します。</span><span class="sxs-lookup"><span data-stu-id="94508-119">On the **Register your application** page, set the **Application Name** to **PHP Graph Tutorial** and select **Create**.</span></span>

    ![アプリ登録ポータル web サイトで新しいアプリを作成するスクリーンショット](/tutorial/images/arp-create-app-01.png)

1. <span data-ttu-id="94508-121">[ **PHP Graph のチュートリアル登録**] ページの [**プロパティ**] セクションで、後で必要になるように**アプリケーション Id**をコピーします。</span><span class="sxs-lookup"><span data-stu-id="94508-121">On the **PHP Graph Tutorial Registration** page, under the **Properties** section, copy the **Application Id** as you will need it later.</span></span>

    ![新しく作成されたアプリケーションの ID のスクリーンショット](/tutorial/images/arp-create-app-02.png)

1. <span data-ttu-id="94508-123">[**アプリケーションシークレット**] セクションまで下にスクロールします。</span><span class="sxs-lookup"><span data-stu-id="94508-123">Scroll down to the **Application Secrets** section.</span></span>

    1. <span data-ttu-id="94508-124">[**新しいパスワードの生成**] を選択します。</span><span class="sxs-lookup"><span data-stu-id="94508-124">Select **Generate New Password**.</span></span>
    1. <span data-ttu-id="94508-125">[**新しいパスワードが生成さ**れました] ダイアログで、後で必要になるように、ボックスの内容をコピーします。</span><span class="sxs-lookup"><span data-stu-id="94508-125">In the **New password generated** dialog, copy the contents of the box as you will need it later.</span></span>

        > <span data-ttu-id="94508-126">**重要:** このパスワードは今後表示されないため、ここでコピーしてください。</span><span class="sxs-lookup"><span data-stu-id="94508-126">**Important:** This password is never shown again, so make sure you copy it now.</span></span>

    ![新しく作成されたアプリケーションのパスワードのスクリーンショット](/tutorial/images/arp-create-app-03.png)

1. <span data-ttu-id="94508-128">[**プラットフォーム**] セクションまで下にスクロールします。</span><span class="sxs-lookup"><span data-stu-id="94508-128">Scroll down to the **Platforms** section.</span></span>

    1. <span data-ttu-id="94508-129">[**プラットフォームの追加**] を選択します。</span><span class="sxs-lookup"><span data-stu-id="94508-129">Select **Add Platform**.</span></span>
    1. <span data-ttu-id="94508-130">[**プラットフォームの追加**] ダイアログで、[ **Web**] を選択します。</span><span class="sxs-lookup"><span data-stu-id="94508-130">In the **Add Platform** dialog, select **Web**.</span></span>

        ![アプリのプラットフォームを作成するスクリーンショット](/tutorial/images/arp-create-app-04.png)

    1. <span data-ttu-id="94508-132">[ **Web**プラットフォーム] ボックスに、 `http://localhost:8000/callback` **リダイレクト url**の url を入力します。</span><span class="sxs-lookup"><span data-stu-id="94508-132">In the **Web** platform box, enter the URL `http://localhost:8000/callback` for the **Redirect URLs**.</span></span>

        ![アプリケーションに新たに追加された Web プラットフォームのスクリーンショット](/tutorial/images/arp-create-app-05.png)

1. <span data-ttu-id="94508-134">ページの一番下までスクロールして、[**保存**] を選択します。</span><span class="sxs-lookup"><span data-stu-id="94508-134">Scroll to the bottom of the page and select **Save**.</span></span>

## <a name="configure-the-sample"></a><span data-ttu-id="94508-135">サンプルを構成する</span><span class="sxs-lookup"><span data-stu-id="94508-135">Configure the sample</span></span>

1. <span data-ttu-id="94508-136">ファイルの`.env.example`名前を`.env`に変更します。</span><span class="sxs-lookup"><span data-stu-id="94508-136">Rename the `.env.example` file to `.env`.</span></span>
1. <span data-ttu-id="94508-137">`.env`ファイルを編集し、次のように変更します。</span><span class="sxs-lookup"><span data-stu-id="94508-137">Edit the `.env` file and make the following changes.</span></span>
    1. <span data-ttu-id="94508-138">を`YOUR_APP_ID_HERE`アプリ登録ポータルで取得した**アプリケーション Id**に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="94508-138">Replace `YOUR_APP_ID_HERE` with the **Application Id** you got from the App Registration Portal.</span></span>
    1. <span data-ttu-id="94508-139">を`YOUR_APP_PASSWORD_HERE`アプリ登録ポータルから取得したパスワードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="94508-139">Replace `YOUR_APP_PASSWORD_HERE` with the password you got from the App Registration Portal.</span></span>
1. <span data-ttu-id="94508-140">コマンドラインインターフェイス (CLI) で、このディレクトリに移動し、次のコマンドを実行して要件をインストールします。</span><span class="sxs-lookup"><span data-stu-id="94508-140">In your command-line interface (CLI), navigate to this directory and run the following command to install requirements.</span></span>

    ```Shell
    composer install
    ```
1. <span data-ttu-id="94508-141">コマンドラインインターフェイス (CLI) で、次のコマンドを実行してアプリケーションキーを生成します。</span><span class="sxs-lookup"><span data-stu-id="94508-141">In your command-line interface (CLI), run the following command to generate an application key.</span></span>

    ```Shell
    php artisan key:generate
    ```

## <a name="run-the-sample"></a><span data-ttu-id="94508-142">サンプルを実行する</span><span class="sxs-lookup"><span data-stu-id="94508-142">Run the sample</span></span>

1. <span data-ttu-id="94508-143">CLI で次のコマンドを実行して、アプリケーションを起動します。</span><span class="sxs-lookup"><span data-stu-id="94508-143">Run the following command in your CLI to start the application.</span></span>

    ```Shell
    php artisan serve
    ```

1. <span data-ttu-id="94508-144">Web ブラウザーを開き、`http://localhost:8000` を参照します。</span><span class="sxs-lookup"><span data-stu-id="94508-144">Open a browser and browse to `http://localhost:8000`.</span></span>