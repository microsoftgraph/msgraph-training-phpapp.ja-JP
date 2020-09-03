<!-- markdownlint-disable MD002 MD041 -->

このチュートリアルでは、Microsoft Graph API を使用してユーザーの予定表情報を取得する PHP web アプリを構築する方法について説明します。

> [!TIP]
> 完成したチュートリアルをダウンロードするだけで済む場合は、2つの方法でダウンロードできます。
>
> - [PHP クイックスタート](https://developer.microsoft.com/graph/quick-start?platform=option-php)をダウンロードして、作業中のコードを分単位で取得します。
> - [GitHub リポジトリ](https://github.com/microsoftgraph/msgraph-training-phpapp)をダウンロードするか、クローンを作成します。

## <a name="prerequisites"></a>前提条件

このチュートリアルを開始する前に、開発用コンピューターに [PHP](http://php.net/downloads.php)、 [Composer](https://getcomposer.org/)、 [Laravel](https://laravel.com/) をインストールしておく必要があります。

また、Outlook.com 上のメールボックスを持つ個人の Microsoft アカウント、または Microsoft 職場または学校のアカウントを所有している必要があります。 Microsoft アカウントを持っていない場合は、無料のアカウントを取得するためのオプションがいくつかあります。

- [新しい個人用 Microsoft アカウントにサインアップ](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1)することができます。
- [Office 365 開発者プログラムにサインアップ](https://developer.microsoft.com/office/dev-program)して、無料の office 365 サブスクリプションを取得することができます。

> [!NOTE]
> このチュートリアルは、PHP バージョン7.4.4、Composer バージョン1.10.10、および Laravel インストーラバージョン3.2.0 で記述されています。 このガイドの手順は、他のバージョンでは動作しますが、テストされていません。

## <a name="feedback"></a>フィードバック

このチュートリアルに関するフィードバックは、 [GitHub リポジトリ](https://github.com/microsoftgraph/msgraph-training-phpapp)に記入してください。
