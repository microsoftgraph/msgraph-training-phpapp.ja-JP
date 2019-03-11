# <a name="how-to-run-the-completed-project"></a>完了したプロジェクトを実行する方法

## <a name="prerequisites"></a>前提条件

このフォルダーで完了したプロジェクトを実行するには、次のものが必要です。

- 開発用コンピューターにインストールされている[PHP](http://php.net/downloads.php) 。 PHP がインストールされていない場合は、「ダウンロードオプション」の前のリンクを参照してください。 (**注:** このチュートリアルは、PHP バージョン7.2 で記述されています。 このガイドの手順は、他のバージョンでは動作しますが、テストされていません。)
- 開発用コンピューターにインストールされた[Composer](https://getcomposer.org/) 。
- 開発用コンピューターにインストールされている[Laravel](https://laravel.com/) 。
- Outlook.com 上のメールボックスを持つ個人の Microsoft アカウント、または microsoft 職場または学校のアカウントのいずれか。

Microsoft アカウントを持っていない場合は、無料のアカウントを取得するためのオプションがいくつかあります。

- [新しい個人用 Microsoft アカウントにサインアップ](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1)することができます。
- [office 365 開発者プログラムにサインアップ](https://developer.microsoft.com/office/dev-program)して、無料の office 365 サブスクリプションを取得することができます。

## <a name="register-a-web-application-with-the-application-registration-portal"></a>web アプリケーションをアプリケーション登録ポータルに登録する

1. ブラウザーを開き、[アプリケーション登録ポータル](https://apps.dev.microsoft.com)に移動します。 **個人アカウント**(別名: Microsoft アカウント) または**職場または学校のアカウント**を使用してログインします。

1. ページの上部にある [**アプリの追加**] を選択します。

    > **注:** ページに [**アプリの追加**] ボタンが複数表示されている場合は、[収束した**アプリ**] リストに対応するボタンを選択します。

1. [**アプリケーションの登録**] ページで、[**アプリケーション名**] を [ **PHP Graph のチュートリアル**] に設定し、[**作成**] を選択します。

    ![アプリ登録ポータル web サイトで新しいアプリを作成するスクリーンショット](/tutorial/images/arp-create-app-01.png)

1. [ **PHP Graph のチュートリアル登録**] ページの [**プロパティ**] セクションで、後で必要になるように**アプリケーション Id**をコピーします。

    ![新しく作成されたアプリケーションの ID のスクリーンショット](/tutorial/images/arp-create-app-02.png)

1. [**アプリケーションシークレット**] セクションまで下にスクロールします。

    1. [**新しいパスワードの生成**] を選択します。
    1. [**新しいパスワードが生成さ**れました] ダイアログで、後で必要になるように、ボックスの内容をコピーします。

        > **重要:** このパスワードは今後表示されないため、ここでコピーしてください。

    ![新しく作成されたアプリケーションのパスワードのスクリーンショット](/tutorial/images/arp-create-app-03.png)

1. [**プラットフォーム**] セクションまで下にスクロールします。

    1. [**プラットフォームの追加**] を選択します。
    1. [**プラットフォームの追加**] ダイアログで、[ **Web**] を選択します。

        ![アプリのプラットフォームを作成するスクリーンショット](/tutorial/images/arp-create-app-04.png)

    1. [ **Web**プラットフォーム] ボックスに、 `http://localhost:8000/callback` **リダイレクト url**の url を入力します。

        ![アプリケーションに新たに追加された Web プラットフォームのスクリーンショット](/tutorial/images/arp-create-app-05.png)

1. ページの一番下までスクロールして、[**保存**] を選択します。

## <a name="configure-the-sample"></a>サンプルを構成する

1. ファイルの`.env.example`名前を`.env`に変更します。
1. `.env`ファイルを編集し、次のように変更します。
    1. を`YOUR_APP_ID_HERE`アプリ登録ポータルで取得した**アプリケーション Id**に置き換えます。
    1. を`YOUR_APP_PASSWORD_HERE`アプリ登録ポータルから取得したパスワードに置き換えます。
1. コマンドラインインターフェイス (CLI) で、このディレクトリに移動し、次のコマンドを実行して要件をインストールします。

    ```Shell
    composer install
    ```
1. コマンドラインインターフェイス (CLI) で、次のコマンドを実行してアプリケーションキーを生成します。

    ```Shell
    php artisan key:generate
    ```

## <a name="run-the-sample"></a>サンプルを実行する

1. CLI で次のコマンドを実行して、アプリケーションを起動します。

    ```Shell
    php artisan serve
    ```

1. Web ブラウザーを開き、`http://localhost:8000` を参照します。