<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="81204-101">このチュートリアルでは、Microsoft Graph API を使用してユーザーの予定表情報を取得する PHP web アプリを構築する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="81204-101">This tutorial teaches you how to build a PHP web app that uses the Microsoft Graph API to retrieve calendar information for a user.</span></span>

> [!TIP]
> <span data-ttu-id="81204-102">完成したチュートリアルをダウンロードするだけで済む場合は、2つの方法でダウンロードできます。</span><span class="sxs-lookup"><span data-stu-id="81204-102">If you prefer to just download the completed tutorial, you can download it in two ways.</span></span>
>
> - <span data-ttu-id="81204-103">[PHP クイックスタート](https://developer.microsoft.com/graph/quick-start?platform=option-php)をダウンロードして、作業中のコードを分単位で取得します。</span><span class="sxs-lookup"><span data-stu-id="81204-103">Download the [PHP quick start](https://developer.microsoft.com/graph/quick-start?platform=option-php) to get working code in minutes.</span></span>
> - <span data-ttu-id="81204-104">[GitHub リポジトリ](https://github.com/microsoftgraph/msgraph-training-phpapp)をダウンロードするか、クローンを作成します。</span><span class="sxs-lookup"><span data-stu-id="81204-104">Download or clone the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-phpapp).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="81204-105">前提条件</span><span class="sxs-lookup"><span data-stu-id="81204-105">Prerequisites</span></span>

<span data-ttu-id="81204-106">このチュートリアルを開始する前に、開発用コンピューターに[PHP](http://php.net/downloads.php)、 [Composer](https://getcomposer.org/)、 [Laravel](https://laravel.com/)をインストールしておく必要があります。</span><span class="sxs-lookup"><span data-stu-id="81204-106">Before you start this tutorial, you should have [PHP](http://php.net/downloads.php), [Composer](https://getcomposer.org/), and [Laravel](https://laravel.com/) installed on your development machine.</span></span>

> [!NOTE]
> <span data-ttu-id="81204-107">このチュートリアルは、PHP バージョン7.2 で記述されています。</span><span class="sxs-lookup"><span data-stu-id="81204-107">This tutorial was written with PHP version 7.2.</span></span> <span data-ttu-id="81204-108">このガイドの手順は、他のバージョンでは動作しますが、テストされていません。</span><span class="sxs-lookup"><span data-stu-id="81204-108">The steps in this guide may work with other versions, but that has not been tested.</span></span>

## <a name="feedback"></a><span data-ttu-id="81204-109">フィードバック</span><span class="sxs-lookup"><span data-stu-id="81204-109">Feedback</span></span>

<span data-ttu-id="81204-110">このチュートリアルに関するフィードバックは、 [GitHub リポジトリ](https://github.com/microsoftgraph/msgraph-training-phpapp)に記入してください。</span><span class="sxs-lookup"><span data-stu-id="81204-110">Please provide any feedback on this tutorial in the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-phpapp).</span></span>