<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="78b67-101">このチュートリアルでは、Microsoft Graph API を使用してユーザーの予定表情報を取得する PHP Web アプリを構築する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="78b67-101">This tutorial teaches you how to build a PHP web app that uses the Microsoft Graph API to retrieve calendar information for a user.</span></span>

> [!TIP]
> <span data-ttu-id="78b67-102">完成したチュートリアルをダウンロードするだけの場合は、2 つの方法でダウンロードできます。</span><span class="sxs-lookup"><span data-stu-id="78b67-102">If you prefer to just download the completed tutorial, you can download it in two ways.</span></span>
>
> - <span data-ttu-id="78b67-103">PHP クイック [スタートをダウンロードして、](https://developer.microsoft.com/graph/quick-start?platform=option-php) 作業コードを数分で取得します。</span><span class="sxs-lookup"><span data-stu-id="78b67-103">Download the [PHP quick start](https://developer.microsoft.com/graph/quick-start?platform=option-php) to get working code in minutes.</span></span>
> - <span data-ttu-id="78b67-104">GitHub リポジトリを [ダウンロードまたは複製します](https://github.com/microsoftgraph/msgraph-training-phpapp)。</span><span class="sxs-lookup"><span data-stu-id="78b67-104">Download or clone the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-phpapp).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="78b67-105">前提条件</span><span class="sxs-lookup"><span data-stu-id="78b67-105">Prerequisites</span></span>

<span data-ttu-id="78b67-106">このチュートリアルを開始する前に、[開発](http://php.net/downloads.php)用コンピューターに[PHP、Composer、Laravel](https://getcomposer.org/)がインストールされている必要があります。 [](https://laravel.com/)</span><span class="sxs-lookup"><span data-stu-id="78b67-106">Before you start this tutorial, you should have [PHP](http://php.net/downloads.php), [Composer](https://getcomposer.org/), and [Laravel](https://laravel.com/) installed on your development machine.</span></span>

<span data-ttu-id="78b67-107">また、メールボックスを持つ個人用の Microsoft アカウントが Outlook.com Microsoft の仕事用アカウントまたは学校アカウントである必要があります。</span><span class="sxs-lookup"><span data-stu-id="78b67-107">You should also have either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span> <span data-ttu-id="78b67-108">Microsoft アカウントをお持ちない場合は、無料アカウントを取得するためのオプションが 2 つ提供されています。</span><span class="sxs-lookup"><span data-stu-id="78b67-108">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="78b67-109">新しい [個人用 Microsoft アカウントにサインアップできます](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1)。</span><span class="sxs-lookup"><span data-stu-id="78b67-109">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="78b67-110">Office [365 開発者プログラムにサインアップして、365](https://developer.microsoft.com/office/dev-program) サブスクリプションを無料Office取得できます。</span><span class="sxs-lookup"><span data-stu-id="78b67-110">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="78b67-111">このチュートリアルは、PHP バージョン 8.0.1、Composer バージョン 2.0.8、Laravel インストーラー バージョン 4.1.1 で記述されました。</span><span class="sxs-lookup"><span data-stu-id="78b67-111">This tutorial was written with PHP version 8.0.1, Composer version 2.0.8, and Laravel installer version 4.1.1.</span></span> <span data-ttu-id="78b67-112">このガイドの手順は他のバージョンでも動作する可能性がありますが、テストは行ってはいではありません。</span><span class="sxs-lookup"><span data-stu-id="78b67-112">The steps in this guide may work with other versions, but that has not been tested.</span></span>

## <a name="feedback"></a><span data-ttu-id="78b67-113">フィードバック</span><span class="sxs-lookup"><span data-stu-id="78b67-113">Feedback</span></span>

<span data-ttu-id="78b67-114">このチュートリアルに関するフィードバックは [、GitHub リポジトリで提供してください](https://github.com/microsoftgraph/msgraph-training-phpapp)。</span><span class="sxs-lookup"><span data-stu-id="78b67-114">Please provide any feedback on this tutorial in the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-phpapp).</span></span>
