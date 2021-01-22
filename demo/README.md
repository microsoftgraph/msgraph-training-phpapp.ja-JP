# <a name="how-to-run-the-completed-project"></a><span data-ttu-id="812a7-101">完成したプロジェクトを実行する方法</span><span class="sxs-lookup"><span data-stu-id="812a7-101">How to run the completed project</span></span>

## <a name="prerequisites"></a><span data-ttu-id="812a7-102">前提条件</span><span class="sxs-lookup"><span data-stu-id="812a7-102">Prerequisites</span></span>

<span data-ttu-id="812a7-103">このフォルダーで完了したプロジェクトを実行するには、以下が必要です。</span><span class="sxs-lookup"><span data-stu-id="812a7-103">To run the completed project in this folder, you need the following:</span></span>

- <span data-ttu-id="812a7-104">開発用コンピューターにインストールされている[PHP。](http://php.net/downloads.php)</span><span class="sxs-lookup"><span data-stu-id="812a7-104">[PHP](http://php.net/downloads.php) installed on your development machine.</span></span> <span data-ttu-id="812a7-105">PHP がない場合は、ダウンロード オプションの前のリンクを参照してください。</span><span class="sxs-lookup"><span data-stu-id="812a7-105">If you do not have PHP, visit the previous link for download options.</span></span> <span data-ttu-id="812a7-106">(**注:** このチュートリアルは PHP バージョン 7.4.4 で記述されています。</span><span class="sxs-lookup"><span data-stu-id="812a7-106">(**Note:** This tutorial was written with PHP version 7.4.4.</span></span> <span data-ttu-id="812a7-107">このガイドの手順は他のバージョンでも動作する可能性がありますが、テストは行ってはいではありません)。</span><span class="sxs-lookup"><span data-stu-id="812a7-107">The steps in this guide may work with other versions, but that has not been tested.)</span></span>
- <span data-ttu-id="812a7-108">開発用コンピューターにインストールされた[Composer。](https://getcomposer.org/)</span><span class="sxs-lookup"><span data-stu-id="812a7-108">[Composer](https://getcomposer.org/) installed on your development machine.</span></span>
- <span data-ttu-id="812a7-109">[Laravel が](https://laravel.com/) 開発用コンピューターにインストールされている。</span><span class="sxs-lookup"><span data-stu-id="812a7-109">[Laravel](https://laravel.com/) installed on your development machine.</span></span>
- <span data-ttu-id="812a7-110">メールボックスがメールボックスを持つ個人の Microsoft アカウントOutlook.com Microsoft の仕事用アカウントまたは学校アカウント。</span><span class="sxs-lookup"><span data-stu-id="812a7-110">Either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span>

<span data-ttu-id="812a7-111">Microsoft アカウントをお持ちない場合は、無料アカウントを取得するためのオプションが 2 つ提供されています。</span><span class="sxs-lookup"><span data-stu-id="812a7-111">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="812a7-112">新しい [個人用 Microsoft アカウントにサインアップできます](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1)。</span><span class="sxs-lookup"><span data-stu-id="812a7-112">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="812a7-113">Office [365 開発者プログラムにサインアップして、365](https://developer.microsoft.com/office/dev-program) サブスクリプションを無料Office取得できます。</span><span class="sxs-lookup"><span data-stu-id="812a7-113">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

## <a name="register-a-web-application-with-the-azure-active-directory-admin-center"></a><span data-ttu-id="812a7-114">Azure Active Directory 管理センターに Web アプリケーションを登録する</span><span class="sxs-lookup"><span data-stu-id="812a7-114">Register a web application with the Azure Active Directory admin center</span></span>

1. <span data-ttu-id="812a7-115">ブラウザーを開き、[Azure Active Directory 管理センター](https://aad.portal.azure.com)へ移動します。</span><span class="sxs-lookup"><span data-stu-id="812a7-115">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com).</span></span> <span data-ttu-id="812a7-116">**個人用アカウント** (別名: Microsoft アカウント)、または **職場/学校アカウント** を使用してログインします。</span><span class="sxs-lookup"><span data-stu-id="812a7-116">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="812a7-117">左側のナビゲーションで **[Azure Active Directory]** を選択し、それから **[管理]** で **[アプリの登録]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="812a7-117">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="812a7-118">アプリの登録のスクリーンショット</span><span class="sxs-lookup"><span data-stu-id="812a7-118">A screenshot of the App registrations</span></span> ](/tutorial/images/aad-portal-app-registrations.png)

1. <span data-ttu-id="812a7-119">**[新規登録]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="812a7-119">Select **New registration**.</span></span> <span data-ttu-id="812a7-120">**[アプリケーションを登録]** ページで、次のように値を設定します。</span><span class="sxs-lookup"><span data-stu-id="812a7-120">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="812a7-121">`PHP Graph Tutorial` に **[名前]** を設定します。</span><span class="sxs-lookup"><span data-stu-id="812a7-121">Set **Name** to `PHP Graph Tutorial`.</span></span>
    - <span data-ttu-id="812a7-122">**[サポートされているアカウントの種類]** を **[任意の組織のディレクトリ内のアカウントと個人用の Microsoft アカウント]** に設定します。</span><span class="sxs-lookup"><span data-stu-id="812a7-122">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="812a7-123">**[リダイレクト URI]** で、最初のドロップダウン リストを `Web` に設定し、それから `http://localhost:8000/callback` に値を設定します。</span><span class="sxs-lookup"><span data-stu-id="812a7-123">Under **Redirect URI**, set the first drop-down to `Web` and set the value to `http://localhost:8000/callback`.</span></span>

    ![[アプリケーションを登録する] ページのスクリーンショット](/tutorial/images/aad-register-an-app.png)

1. <span data-ttu-id="812a7-125">**[登録]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="812a7-125">Choose **Register**.</span></span> <span data-ttu-id="812a7-126">PHP **Graph チュートリアル ページ** で、アプリケーション **(クライアント) ID** の値をコピーして保存します。次の手順で必要になります。</span><span class="sxs-lookup"><span data-stu-id="812a7-126">On the **PHP Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![新しいアプリ登録のアプリケーション ID のスクリーンショット](/tutorial/images/aad-application-id.png)

1. <span data-ttu-id="812a7-128">**[管理]** で **[証明書とシークレット]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="812a7-128">Select **Certificates & secrets** under **Manage**.</span></span> <span data-ttu-id="812a7-129">**[新しいクライアント シークレット]** ボタンを選択します。</span><span class="sxs-lookup"><span data-stu-id="812a7-129">Select the **New client secret** button.</span></span> <span data-ttu-id="812a7-130">**[説明]** に値を入力して、**[有効期限]** のオプションのいずれかを選び、**[追加]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="812a7-130">Enter a value in **Description** and select one of the options for **Expires** and choose **Add**.</span></span>

    ![[クライアントシークレットの追加] ダイアログのスクリーンショット](/tutorial/images/aad-new-client-secret.png)

1. <span data-ttu-id="812a7-132">このページを離れる前に、クライアント シークレットの値をコピーします。</span><span class="sxs-lookup"><span data-stu-id="812a7-132">Copy the client secret value before you leave this page.</span></span> <span data-ttu-id="812a7-133">次の手順で行います。</span><span class="sxs-lookup"><span data-stu-id="812a7-133">You will need it in the next step.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="812a7-134">このクライアント シークレットは今後表示されないため、この段階で必ずコピーするようにしてください。</span><span class="sxs-lookup"><span data-stu-id="812a7-134">This client secret is never shown again, so make sure you copy it now.</span></span>

    ![新規追加されたクライアント シークレットのスクリーンショット](/tutorial/images/aad-copy-client-secret.png)

## <a name="configure-the-sample"></a><span data-ttu-id="812a7-136">サンプルを構成する</span><span class="sxs-lookup"><span data-stu-id="812a7-136">Configure the sample</span></span>

1. <span data-ttu-id="812a7-137">ファイルの名前 `example.env` を変更します `.env` 。</span><span class="sxs-lookup"><span data-stu-id="812a7-137">Rename the `example.env` file to `.env`.</span></span>
1. <span data-ttu-id="812a7-138">ファイルを `.env` 編集し、次の変更を行います。</span><span class="sxs-lookup"><span data-stu-id="812a7-138">Edit the `.env` file and make the following changes.</span></span>
    1. <span data-ttu-id="812a7-139">アプリ `YOUR_APP_ID_HERE` 登録ポータル **から取得** したアプリケーション ID に置き換える。</span><span class="sxs-lookup"><span data-stu-id="812a7-139">Replace `YOUR_APP_ID_HERE` with the **Application Id** you got from the App Registration Portal.</span></span>
    1. <span data-ttu-id="812a7-140">アプリ `YOUR_APP_PASSWORD_HERE` 登録ポータルから受け取ったパスワードに置き換える。</span><span class="sxs-lookup"><span data-stu-id="812a7-140">Replace `YOUR_APP_PASSWORD_HERE` with the password you got from the App Registration Portal.</span></span>
1. <span data-ttu-id="812a7-141">コマンド ライン インターフェイス (CLI) で、このディレクトリに移動し、次のコマンドを実行して要件をインストールします。</span><span class="sxs-lookup"><span data-stu-id="812a7-141">In your command-line interface (CLI), navigate to this directory and run the following command to install requirements.</span></span>

    ```Shell
    composer install
    ```

1. <span data-ttu-id="812a7-142">コマンド ライン インターフェイス (CLI) で、次のコマンドを実行してアプリケーション キーを生成します。</span><span class="sxs-lookup"><span data-stu-id="812a7-142">In your command-line interface (CLI), run the following command to generate an application key.</span></span>

    ```Shell
    php artisan key:generate
    ```

## <a name="run-the-sample"></a><span data-ttu-id="812a7-143">サンプルを実行する</span><span class="sxs-lookup"><span data-stu-id="812a7-143">Run the sample</span></span>

1. <span data-ttu-id="812a7-144">CLI で次のコマンドを実行して、アプリケーションを起動します。</span><span class="sxs-lookup"><span data-stu-id="812a7-144">Run the following command in your CLI to start the application.</span></span>

    ```Shell
    php artisan serve
    ```

1. <span data-ttu-id="812a7-145">Web ブラウザーを開き、`http://localhost:8000` を参照します。</span><span class="sxs-lookup"><span data-stu-id="812a7-145">Open a browser and browse to `http://localhost:8000`.</span></span>
