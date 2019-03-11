<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="41bba-101">この演習では、アプリケーションレジストリポータル (ARP) を使用して、新しい Azure AD web アプリケーション登録を作成します。</span><span class="sxs-lookup"><span data-stu-id="41bba-101">In this exercise, you will create a new Azure AD web application registration using the Application Registry Portal (ARP).</span></span>

1. <span data-ttu-id="41bba-102">ブラウザーを開き、[アプリケーション登録ポータル](https://apps.dev.microsoft.com)に移動します。</span><span class="sxs-lookup"><span data-stu-id="41bba-102">Open a browser and navigate to the [Application Registration Portal](https://apps.dev.microsoft.com).</span></span> <span data-ttu-id="41bba-103">**個人アカウント**(別名: Microsoft アカウント) または**職場または学校のアカウント**を使用してログインします。</span><span class="sxs-lookup"><span data-stu-id="41bba-103">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="41bba-104">ページの上部にある [**アプリの追加**] を選択します。</span><span class="sxs-lookup"><span data-stu-id="41bba-104">Select **Add an app** at the top of the page.</span></span>

    > [!NOTE]
    > <span data-ttu-id="41bba-105">ページに [**アプリの追加**] ボタンが複数表示されている場合は、[収束した**アプリ**] リストに対応するボタンを選択します。</span><span class="sxs-lookup"><span data-stu-id="41bba-105">If you see more than one **Add an app** button on the page, select the one that corresponds to the **Converged apps** list.</span></span>

1. <span data-ttu-id="41bba-106">[**アプリケーションの登録**] ページで、[**アプリケーション名**] を [ **PHP Graph のチュートリアル**] に設定し、[**作成**] を選択します。</span><span class="sxs-lookup"><span data-stu-id="41bba-106">On the **Register your application** page, set the **Application Name** to **PHP Graph Tutorial** and select **Create**.</span></span>

    ![アプリ登録ポータル web サイトで新しいアプリを作成するスクリーンショット](./images/arp-create-app-01.png)

1. <span data-ttu-id="41bba-108">[ **PHP Graph のチュートリアル登録**] ページの [**プロパティ**] セクションで、後で必要になるように**アプリケーション Id**をコピーします。</span><span class="sxs-lookup"><span data-stu-id="41bba-108">On the **PHP Graph Tutorial Registration** page, under the **Properties** section, copy the **Application Id** as you will need it later.</span></span>

    ![新しく作成されたアプリケーションの ID のスクリーンショット](./images/arp-create-app-02.png)

1. <span data-ttu-id="41bba-110">[**アプリケーションシークレット**] セクションまで下にスクロールします。</span><span class="sxs-lookup"><span data-stu-id="41bba-110">Scroll down to the **Application Secrets** section.</span></span>

    1. <span data-ttu-id="41bba-111">[**新しいパスワードの生成**] を選択します。</span><span class="sxs-lookup"><span data-stu-id="41bba-111">Select **Generate New Password**.</span></span>
    1. <span data-ttu-id="41bba-112">[**新しいパスワードが生成さ**れました] ダイアログで、後で必要になるように、ボックスの内容をコピーします。</span><span class="sxs-lookup"><span data-stu-id="41bba-112">In the **New password generated** dialog, copy the contents of the box as you will need it later.</span></span>

        > [!IMPORTANT]
        > <span data-ttu-id="41bba-113">このパスワードは今後表示されないため、ここでコピーしてください。</span><span class="sxs-lookup"><span data-stu-id="41bba-113">This password is never shown again, so make sure you copy it now.</span></span>

    ![新しく作成されたアプリケーションのパスワードのスクリーンショット](./images/arp-create-app-03.png)

1. <span data-ttu-id="41bba-115">[**プラットフォーム**] セクションまで下にスクロールします。</span><span class="sxs-lookup"><span data-stu-id="41bba-115">Scroll down to the **Platforms** section.</span></span>

    1. <span data-ttu-id="41bba-116">[**プラットフォームの追加**] を選択します。</span><span class="sxs-lookup"><span data-stu-id="41bba-116">Select **Add Platform**.</span></span>
    1. <span data-ttu-id="41bba-117">[**プラットフォームの追加**] ダイアログで、[ **Web**] を選択します。</span><span class="sxs-lookup"><span data-stu-id="41bba-117">In the **Add Platform** dialog, select **Web**.</span></span>

        ![アプリのプラットフォームを作成するスクリーンショット](./images/arp-create-app-04.png)

    1. <span data-ttu-id="41bba-119">[ **Web**プラットフォーム] ボックスに、 `http://localhost:8000/callback` **リダイレクト url**の url を入力します。</span><span class="sxs-lookup"><span data-stu-id="41bba-119">In the **Web** platform box, enter the URL `http://localhost:8000/callback` for the **Redirect URLs**.</span></span>

        ![アプリケーションに新たに追加された Web プラットフォームのスクリーンショット](./images/arp-create-app-05.png)

1. <span data-ttu-id="41bba-121">ページの一番下までスクロールして、[**保存**] を選択します。</span><span class="sxs-lookup"><span data-stu-id="41bba-121">Scroll to the bottom of the page and select **Save**.</span></span>