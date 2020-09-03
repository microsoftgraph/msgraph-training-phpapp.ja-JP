# <a name="contributing-to-microsoft-graph-training-repositories"></a>Microsoft Graph トレーニングリポジトリへの投稿

このプロジェクトへの投稿にご協力いただき、ありがとうございます。 プル要求を送信する前に、次の点を考慮してください。

## <a name="overview"></a>概要

このリポジトリのコードは、次の3つの目的を果たします。

- [チュートリアル](/tutorial)フォルダー内の Markdown ファイルは、「 [Microsoft Graph のチュートリアル](https://docs.microsoft.com/graph/tutorials)」ページにチュートリアルとして公開されています。
- [Demo](/demo)フォルダーのサンプルプロジェクトは、 [Microsoft Graph クイックスタート](https://developer.microsoft.com/graph/quick-start)のソースです。**\***
- Demo フォルダーのサンプルプロジェクトも GitHub から直接ダウンロードすることができ、単純な構成を行った後はそのまま実行する必要があります。

> **\*** すべてのトレーニングリポジトリがクイックスタート (まだ) で利用できるわけではありません。

この点は重要です。1つの場所での変更によっては、同期を維持するために別の場所の変更が必要に *なることがあり* ます。可能であれば、Markdown ファイルは、ソースコードファイルを (カスタム構文を使用して) 直接参照するので `:::code` 、ソース内のコードを更新すると Markdown のコードが自動的に更新されるようになります。

## <a name="updating-code"></a>コードの更新

`:::code`Markdown で使用される構文は、ソースコードファイル内の特定のコメントに依存します。 コメントは次のようになります。

```csharp
// <MySnippet>
Console.WriteLine("Hello World!");
// </MySnippet>
```

これらの "マーカー" コメントの間でコードを更新すると、Markdown ファイルは、Microsoft Graph ドキュメントサイトに公開されたときに、自動的に変更を取得します。 これらのコメントの外部にあるコードを更新する場合は、対応する Markdown を更新する必要があります。

## <a name="adding-features"></a>機能の追加

Enthusiasm は歓迎しますが、プル要求を送信して新しい機能をサンプルに追加しないでください。 このリポジトリは、主に "最初のアプリをビルドする" チュートリアルであるため、機能セットは設計によって制限されています。

## <a name="submitting-pull-requests"></a>プル要求の送信

すべてのプル要求をブランチに送信してください `master` 。

## <a name="when-do-changes-get-published"></a>変更が公開されるのはいつですか。

[Microsoft Graph チュートリアル](https://docs.microsoft.com/graph/tutorials)サイトへの更新プログラムの公開は、自動的には行われません。 変更は、最初にブランチに昇格する必要があり `live` ます。その後、サイト管理者がビルドを開始する必要があります。 これは、通常、「必要に応じて」に基づいて行われます。

## <a name="code-of-conduct"></a>Code of conduct

このプロジェクトでは、[Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/) が採用されています。詳細については、「[規範に関する FAQ](https://opensource.microsoft.com/codeofconduct/faq/)」を参照してください。または、その他の質問やコメントがあれば、[opencode@microsoft.com](mailto:opencode@microsoft.com) までにお問い合わせください。
