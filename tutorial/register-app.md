<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="2549e-101">この演習では、azure Active Directory 管理センターを使用して、新しい azure AD web アプリケーション登録を作成します。</span><span class="sxs-lookup"><span data-stu-id="2549e-101">In this exercise, you will create a new Azure AD web application registration using the Azure Active Directory admin center.</span></span>

1. <span data-ttu-id="2549e-102">ブラウザーを開き、 [Azure Active Directory 管理センター](https://aad.portal.azure.com)に移動します。</span><span class="sxs-lookup"><span data-stu-id="2549e-102">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com).</span></span> <span data-ttu-id="2549e-103">**個人アカウント**(別名: Microsoft アカウント) または**職場または学校のアカウント**を使用してログインします。</span><span class="sxs-lookup"><span data-stu-id="2549e-103">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="2549e-104">左側のナビゲーションで [ **Azure Active Directory** ] を選択し、[**管理**] で [**アプリの登録 (プレビュー)** ] を選択します。</span><span class="sxs-lookup"><span data-stu-id="2549e-104">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations (Preview)** under **Manage**.</span></span>

    ![<span data-ttu-id="2549e-105">アプリの登録のスクリーンショット</span><span class="sxs-lookup"><span data-stu-id="2549e-105">A screenshot of the App registrations</span></span> ](./images/aad-portal-app-registrations.png)

1. <span data-ttu-id="2549e-106">[**新しい登録**] を選択します。</span><span class="sxs-lookup"><span data-stu-id="2549e-106">Select **New registration**.</span></span> <span data-ttu-id="2549e-107">[**アプリケーションの登録**] ページで、次のように値を設定します。</span><span class="sxs-lookup"><span data-stu-id="2549e-107">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="2549e-108">**名前**をに`PHP Graph Tutorial`設定します。</span><span class="sxs-lookup"><span data-stu-id="2549e-108">Set **Name** to `PHP Graph Tutorial`.</span></span>
    - <span data-ttu-id="2549e-109">**任意の組織ディレクトリおよび個人の Microsoft アカウントのアカウント**に、**サポートされているアカウントの種類**を設定します。</span><span class="sxs-lookup"><span data-stu-id="2549e-109">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="2549e-110">[**リダイレクト URI**] で、最初のドロップダウンを`Web`に設定し、値`http://localhost:8000/callback`をに設定します。</span><span class="sxs-lookup"><span data-stu-id="2549e-110">Under **Redirect URI**, set the first drop-down to `Web` and set the value to `http://localhost:8000/callback`.</span></span>

    ![[アプリケーションの登録] ページのスクリーンショット](./images/aad-register-an-app.png)

1. <span data-ttu-id="2549e-112">[**登録**] を選択します。</span><span class="sxs-lookup"><span data-stu-id="2549e-112">Choose **Register**.</span></span> <span data-ttu-id="2549e-113">**PHP Graph のチュートリアル**ページで、**アプリケーション (クライアント) ID**の値をコピーして保存します。次の手順で必要になります。</span><span class="sxs-lookup"><span data-stu-id="2549e-113">On the **PHP Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![新しいアプリの登録のアプリケーション ID のスクリーンショット](./images/aad-application-id.png)

1. <span data-ttu-id="2549e-115">[**証明書の &** ] の [**管理**] で選択します。</span><span class="sxs-lookup"><span data-stu-id="2549e-115">Select **Certificates & secrets** under **Manage**.</span></span> <span data-ttu-id="2549e-116">[**新しいクライアントシークレット**] ボタンを選択します。</span><span class="sxs-lookup"><span data-stu-id="2549e-116">Select the **New client secret** button.</span></span> <span data-ttu-id="2549e-117">[**説明**] に値を入力して、[**有効期限**] のオプションの1つを選択し、[**追加**] を選択します。</span><span class="sxs-lookup"><span data-stu-id="2549e-117">Enter a value in **Description** and select one of the options for **Expires** and choose **Add**.</span></span>

    ![[クライアントシークレットの追加] ダイアログのスクリーンショット](./images/aad-new-client-secret.png)

1. <span data-ttu-id="2549e-119">このページを終了する前に、クライアントシークレット値をコピーします。</span><span class="sxs-lookup"><span data-stu-id="2549e-119">Copy the client secret value before you leave this page.</span></span> <span data-ttu-id="2549e-120">次の手順で必要になります。</span><span class="sxs-lookup"><span data-stu-id="2549e-120">You will need it in the next step.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="2549e-121">このクライアントシークレットが再度表示されることはありません。そのため、ここでコピーしてください。</span><span class="sxs-lookup"><span data-stu-id="2549e-121">This client secret is never shown again, so make sure you copy it now.</span></span>

    ![新しく追加されたクライアントシークレットのスクリーンショット](./images/aad-copy-client-secret.png)