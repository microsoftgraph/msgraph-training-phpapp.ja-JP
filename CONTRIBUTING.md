# <a name="contributing-to-microsoft-graph-training-repositories"></a><span data-ttu-id="65880-101">Microsoft Graph トレーニングリポジトリへの投稿</span><span class="sxs-lookup"><span data-stu-id="65880-101">Contributing to Microsoft Graph training repositories</span></span>

<span data-ttu-id="65880-102">このプロジェクトへの投稿にご協力いただき、ありがとうございます。</span><span class="sxs-lookup"><span data-stu-id="65880-102">Thank you for contributing to this project!</span></span> <span data-ttu-id="65880-103">プル要求を送信する前に、次の点を考慮してください。</span><span class="sxs-lookup"><span data-stu-id="65880-103">Before submitting your pull request, be sure to consider the following.</span></span>

## <a name="overview"></a><span data-ttu-id="65880-104">概要</span><span class="sxs-lookup"><span data-stu-id="65880-104">Overview</span></span>

<span data-ttu-id="65880-105">このリポジトリのコードは、次の3つの目的を果たします。</span><span class="sxs-lookup"><span data-stu-id="65880-105">The code in this repository serves three purposes:</span></span>

- <span data-ttu-id="65880-106">[チュートリアル](/tutorial)フォルダー内の Markdown ファイルは、「 [Microsoft Graph のチュートリアル](https://docs.microsoft.com/graph/tutorials)」ページにチュートリアルとして公開されています。</span><span class="sxs-lookup"><span data-stu-id="65880-106">The Markdown files in the [tutorial](/tutorial) folder are published as a tutorial on the [Microsoft Graph tutorials](https://docs.microsoft.com/graph/tutorials) page.</span></span>
- <span data-ttu-id="65880-107">[Demo](/demo)フォルダーのサンプルプロジェクトは、 [Microsoft Graph クイックスタート](https://developer.microsoft.com/graph/quick-start)のソースです。**\***</span><span class="sxs-lookup"><span data-stu-id="65880-107">The sample project in the [demo](/demo) folder is the source for a [Microsoft Graph quick start](https://developer.microsoft.com/graph/quick-start).**\***</span></span>
- <span data-ttu-id="65880-108">Demo フォルダーのサンプルプロジェクトも GitHub から直接ダウンロードすることができ、単純な構成を行った後はそのまま実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="65880-108">The sample project in the demo folder is also downloadable directly from GitHub and should run as-is after some simple configuration.</span></span>

> <span data-ttu-id="65880-109">**\*** すべてのトレーニングリポジトリがクイックスタート (まだ) で利用できるわけではありません。</span><span class="sxs-lookup"><span data-stu-id="65880-109">**\*** Not all training repositories are available as quick starts (yet).</span></span>

<span data-ttu-id="65880-110">この点は重要です。1つの場所での変更によっては、同期を維持するために別の場所の変更が必要に *なることがあり* ます。可能であれば、Markdown ファイルは、ソースコードファイルを (カスタム構文を使用して) 直接参照するので `:::code` 、ソース内のコードを更新すると Markdown のコードが自動的に更新されるようになります。</span><span class="sxs-lookup"><span data-stu-id="65880-110">This is important to keep in mind, because changes in one place *may* require changes in another, to keep things in sync. Whereever possible, the Markdown files refer to the source code files directly (using a custom `:::code` syntax), so that updating code in source will automatically update the code in Markdown.</span></span>

## <a name="updating-code"></a><span data-ttu-id="65880-111">コードの更新</span><span class="sxs-lookup"><span data-stu-id="65880-111">Updating code</span></span>

<span data-ttu-id="65880-112">`:::code`Markdown で使用される構文は、ソースコードファイル内の特定のコメントに依存します。</span><span class="sxs-lookup"><span data-stu-id="65880-112">The `:::code` syntax used in Markdown depends on specific comments in the source code file.</span></span> <span data-ttu-id="65880-113">コメントは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="65880-113">These comments look like the following:</span></span>

```csharp
// <MySnippet>
Console.WriteLine("Hello World!");
// </MySnippet>
```

<span data-ttu-id="65880-114">これらの "マーカー" コメントの間でコードを更新すると、Markdown ファイルは、Microsoft Graph ドキュメントサイトに公開されたときに、自動的に変更を取得します。</span><span class="sxs-lookup"><span data-stu-id="65880-114">If you update code between these "marker" comments, the Markdown files will automatically get those changes when published to the Microsoft Graph documentation site.</span></span> <span data-ttu-id="65880-115">これらのコメントの外部にあるコードを更新する場合は、対応する Markdown を更新する必要があります。</span><span class="sxs-lookup"><span data-stu-id="65880-115">If you update code outside of those comments, it's very likely that you'll need to update the corresponding Markdown.</span></span>

## <a name="adding-features"></a><span data-ttu-id="65880-116">機能の追加</span><span class="sxs-lookup"><span data-stu-id="65880-116">Adding features</span></span>

<span data-ttu-id="65880-117">Enthusiasm は歓迎しますが、プル要求を送信して新しい機能をサンプルに追加しないでください。</span><span class="sxs-lookup"><span data-stu-id="65880-117">While the enthusiasm is appreciated, please don't send pull requests to add new features to the sample.</span></span> <span data-ttu-id="65880-118">このリポジトリは、主に "最初のアプリをビルドする" チュートリアルであるため、機能セットは設計によって制限されています。</span><span class="sxs-lookup"><span data-stu-id="65880-118">Because this repository is primarily a "build your first app" tutorial, the feature set is limited, by design.</span></span>

## <a name="submitting-pull-requests"></a><span data-ttu-id="65880-119">プル要求の送信</span><span class="sxs-lookup"><span data-stu-id="65880-119">Submitting pull requests</span></span>

<span data-ttu-id="65880-120">すべてのプル要求をブランチに送信してください `master` 。</span><span class="sxs-lookup"><span data-stu-id="65880-120">Please submit all pull requests to the `master` branch.</span></span>

## <a name="when-do-changes-get-published"></a><span data-ttu-id="65880-121">変更が公開されるのはいつですか。</span><span class="sxs-lookup"><span data-stu-id="65880-121">When do changes get published?</span></span>

<span data-ttu-id="65880-122">[Microsoft Graph チュートリアル](https://docs.microsoft.com/graph/tutorials)サイトへの更新プログラムの公開は、自動的には行われません。</span><span class="sxs-lookup"><span data-stu-id="65880-122">Publishing of updates to the [Microsoft Graph tutorials](https://docs.microsoft.com/graph/tutorials) site is not automatic.</span></span> <span data-ttu-id="65880-123">変更は、最初にブランチに昇格する必要があり `live` ます。その後、サイト管理者がビルドを開始する必要があります。</span><span class="sxs-lookup"><span data-stu-id="65880-123">Changes must first be promoted to the `live` branch, then a build must be triggered by the site admins.</span></span> <span data-ttu-id="65880-124">これは、通常、「必要に応じて」に基づいて行われます。</span><span class="sxs-lookup"><span data-stu-id="65880-124">This is typically done on an "as-needed" basis.</span></span>

## <a name="code-of-conduct"></a><span data-ttu-id="65880-125">Code of conduct</span><span class="sxs-lookup"><span data-stu-id="65880-125">Code of conduct</span></span>

<span data-ttu-id="65880-p106">このプロジェクトでは、[Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/) が採用されています。詳細については、「[規範に関する FAQ](https://opensource.microsoft.com/codeofconduct/faq/)」を参照してください。または、その他の質問やコメントがあれば、[opencode@microsoft.com](mailto:opencode@microsoft.com) までにお問い合わせください。</span><span class="sxs-lookup"><span data-stu-id="65880-p106">This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/). For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.</span></span>
