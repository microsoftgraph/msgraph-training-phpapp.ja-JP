# <a name="how-to-run-the-completed-project"></a><span data-ttu-id="bc1cf-101">完了したプロジェクトを実行する方法</span><span class="sxs-lookup"><span data-stu-id="bc1cf-101">How to run the completed project</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bc1cf-102">前提条件</span><span class="sxs-lookup"><span data-stu-id="bc1cf-102">Prerequisites</span></span>

<span data-ttu-id="bc1cf-103">このフォルダーで完了したプロジェクトを実行するには、次のものが必要です。</span><span class="sxs-lookup"><span data-stu-id="bc1cf-103">To run the completed project in this folder, you need the following:</span></span>

- <span data-ttu-id="bc1cf-104">開発用コンピューターにインストールされている[PHP](http://php.net/downloads.php) 。</span><span class="sxs-lookup"><span data-stu-id="bc1cf-104">[PHP](http://php.net/downloads.php) installed on your development machine.</span></span> <span data-ttu-id="bc1cf-105">PHP がインストールされていない場合は、「ダウンロードオプション」の前のリンクを参照してください。</span><span class="sxs-lookup"><span data-stu-id="bc1cf-105">If you do not have PHP, visit the previous link for download options.</span></span> <span data-ttu-id="bc1cf-106">(**注:** このチュートリアルは、PHP バージョン7.4.4 で記述されています。</span><span class="sxs-lookup"><span data-stu-id="bc1cf-106">(**Note:** This tutorial was written with PHP version 7.4.4.</span></span> <span data-ttu-id="bc1cf-107">このガイドの手順は、他のバージョンでは動作しますが、テストされていません。)</span><span class="sxs-lookup"><span data-stu-id="bc1cf-107">The steps in this guide may work with other versions, but that has not been tested.)</span></span>
- <span data-ttu-id="bc1cf-108">開発用コンピューターにインストールされた[Composer](https://getcomposer.org/) 。</span><span class="sxs-lookup"><span data-stu-id="bc1cf-108">[Composer](https://getcomposer.org/) installed on your development machine.</span></span>
- <span data-ttu-id="bc1cf-109">開発用コンピューターにインストールされている[Laravel](https://laravel.com/) 。</span><span class="sxs-lookup"><span data-stu-id="bc1cf-109">[Laravel](https://laravel.com/) installed on your development machine.</span></span>
- <span data-ttu-id="bc1cf-110">Outlook.com 上のメールボックスを持つ個人の Microsoft アカウント、または Microsoft 職場または学校のアカウントのいずれか。</span><span class="sxs-lookup"><span data-stu-id="bc1cf-110">Either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span>

<span data-ttu-id="bc1cf-111">Microsoft アカウントを持っていない場合は、無料のアカウントを取得するためのオプションがいくつかあります。</span><span class="sxs-lookup"><span data-stu-id="bc1cf-111">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="bc1cf-112">[新しい個人用 Microsoft アカウントにサインアップ](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1)することができます。</span><span class="sxs-lookup"><span data-stu-id="bc1cf-112">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="bc1cf-113">[Office 365 開発者プログラムにサインアップ](https://developer.microsoft.com/office/dev-program)して、無料の office 365 サブスクリプションを取得することができます。</span><span class="sxs-lookup"><span data-stu-id="bc1cf-113">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

## <a name="register-a-web-application-with-the-azure-active-directory-admin-center"></a><span data-ttu-id="bc1cf-114">Web アプリケーションを Azure Active Directory 管理センターに登録する</span><span class="sxs-lookup"><span data-stu-id="bc1cf-114">Register a web application with the Azure Active Directory admin center</span></span>

1. <span data-ttu-id="bc1cf-115">ブラウザーを開き、[Azure Active Directory 管理センター](https://aad.portal.azure.com)へ移動します。</span><span class="sxs-lookup"><span data-stu-id="bc1cf-115">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com).</span></span> <span data-ttu-id="bc1cf-116">**個人用アカウント** (別名: Microsoft アカウント)、または**職場/学校アカウント**を使用してログインします。</span><span class="sxs-lookup"><span data-stu-id="bc1cf-116">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="bc1cf-117">左側のナビゲーションで **[Azure Active Directory]** を選択し、それから **[管理]** で **[アプリの登録]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="bc1cf-117">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="bc1cf-118">アプリの登録のスクリーンショット</span><span class="sxs-lookup"><span data-stu-id="bc1cf-118">A screenshot of the App registrations</span></span> ](/tutorial/images/aad-portal-app-registrations.png)

1. <span data-ttu-id="bc1cf-119">**[新規登録]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="bc1cf-119">Select **New registration**.</span></span> <span data-ttu-id="bc1cf-120">**[アプリケーションを登録]** ページで、次のように値を設定します。</span><span class="sxs-lookup"><span data-stu-id="bc1cf-120">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="bc1cf-121">`PHP Graph Tutorial` に **[名前]** を設定します。</span><span class="sxs-lookup"><span data-stu-id="bc1cf-121">Set **Name** to `PHP Graph Tutorial`.</span></span>
    - <span data-ttu-id="bc1cf-122">**[サポートされているアカウントの種類]** を **[任意の組織のディレクトリ内のアカウントと個人用の Microsoft アカウント]** に設定します。</span><span class="sxs-lookup"><span data-stu-id="bc1cf-122">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="bc1cf-123">**[リダイレクト URI]** で、最初のドロップダウン リストを `Web` に設定し、それから `http://localhost:8000/callback` に値を設定します。</span><span class="sxs-lookup"><span data-stu-id="bc1cf-123">Under **Redirect URI**, set the first drop-down to `Web` and set the value to `http://localhost:8000/callback`.</span></span>

    ![[アプリケーションを登録する] ページのスクリーンショット](/tutorial/images/aad-register-an-app.png)

1. <span data-ttu-id="bc1cf-125">**[登録]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="bc1cf-125">Choose **Register**.</span></span> <span data-ttu-id="bc1cf-126">**PHP Graph のチュートリアル**ページで、**アプリケーション (クライアント) ID**の値をコピーして保存します。次の手順で必要になります。</span><span class="sxs-lookup"><span data-stu-id="bc1cf-126">On the **PHP Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![新しいアプリ登録のアプリケーション ID のスクリーンショット](/tutorial/images/aad-application-id.png)

1. <span data-ttu-id="bc1cf-128">**[管理]** で **[証明書とシークレット]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="bc1cf-128">Select **Certificates & secrets** under **Manage**.</span></span> <span data-ttu-id="bc1cf-129">**[新しいクライアント シークレット]** ボタンを選択します。</span><span class="sxs-lookup"><span data-stu-id="bc1cf-129">Select the **New client secret** button.</span></span> <span data-ttu-id="bc1cf-130">**[説明]** に値を入力して、**[有効期限]** のオプションのいずれかを選び、**[追加]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="bc1cf-130">Enter a value in **Description** and select one of the options for **Expires** and choose **Add**.</span></span>

    ![[クライアントシークレットの追加] ダイアログのスクリーンショット](/tutorial/images/aad-new-client-secret.png)

1. <span data-ttu-id="bc1cf-132">このページを離れる前に、クライアント シークレットの値をコピーします。</span><span class="sxs-lookup"><span data-stu-id="bc1cf-132">Copy the client secret value before you leave this page.</span></span> <span data-ttu-id="bc1cf-133">次の手順で行います。</span><span class="sxs-lookup"><span data-stu-id="bc1cf-133">You will need it in the next step.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="bc1cf-134">このクライアント シークレットは今後表示されないため、この段階で必ずコピーするようにしてください。</span><span class="sxs-lookup"><span data-stu-id="bc1cf-134">This client secret is never shown again, so make sure you copy it now.</span></span>

    ![新規追加されたクライアント シークレットのスクリーンショット](/tutorial/images/aad-copy-client-secret.png)

## <a name="configure-the-sample"></a><span data-ttu-id="bc1cf-136">サンプルを構成する</span><span class="sxs-lookup"><span data-stu-id="bc1cf-136">Configure the sample</span></span>

1. <span data-ttu-id="bc1cf-137">ファイルの`.env.example`名前を`.env`に変更します。</span><span class="sxs-lookup"><span data-stu-id="bc1cf-137">Rename the `.env.example` file to `.env`.</span></span>
1. <span data-ttu-id="bc1cf-138">`.env`ファイルを編集し、次のように変更します。</span><span class="sxs-lookup"><span data-stu-id="bc1cf-138">Edit the `.env` file and make the following changes.</span></span>
    1. <span data-ttu-id="bc1cf-139">を`YOUR_APP_ID_HERE`アプリ登録ポータルで取得した**アプリケーション Id**に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="bc1cf-139">Replace `YOUR_APP_ID_HERE` with the **Application Id** you got from the App Registration Portal.</span></span>
    1. <span data-ttu-id="bc1cf-140">を`YOUR_APP_PASSWORD_HERE`アプリ登録ポータルから取得したパスワードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="bc1cf-140">Replace `YOUR_APP_PASSWORD_HERE` with the password you got from the App Registration Portal.</span></span>
1. <span data-ttu-id="bc1cf-141">コマンドラインインターフェイス (CLI) で、このディレクトリに移動し、次のコマンドを実行して要件をインストールします。</span><span class="sxs-lookup"><span data-stu-id="bc1cf-141">In your command-line interface (CLI), navigate to this directory and run the following command to install requirements.</span></span>

    ```Shell
    composer install
    ```

1. <span data-ttu-id="bc1cf-142">コマンドラインインターフェイス (CLI) で、次のコマンドを実行してアプリケーションキーを生成します。</span><span class="sxs-lookup"><span data-stu-id="bc1cf-142">In your command-line interface (CLI), run the following command to generate an application key.</span></span>

    ```Shell
    php artisan key:generate
    ```

## <a name="run-the-sample"></a><span data-ttu-id="bc1cf-143">サンプルを実行する</span><span class="sxs-lookup"><span data-stu-id="bc1cf-143">Run the sample</span></span>

1. <span data-ttu-id="bc1cf-144">CLI で次のコマンドを実行して、アプリケーションを起動します。</span><span class="sxs-lookup"><span data-stu-id="bc1cf-144">Run the following command in your CLI to start the application.</span></span>

    ```Shell
    php artisan serve
    ```

1. <span data-ttu-id="bc1cf-145">Web ブラウザーを開き、`http://localhost:8000` を参照します。</span><span class="sxs-lookup"><span data-stu-id="bc1cf-145">Open a browser and browse to `http://localhost:8000`.</span></span>
