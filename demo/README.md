# <a name="how-to-run-the-completed-project"></a>完成したプロジェクトを実行する方法

## <a name="prerequisites"></a>前提条件

このフォルダーで完了したプロジェクトを実行するには、以下が必要です。

- 開発用コンピューターにインストールされている[PHP。](http://php.net/downloads.php) PHP がない場合は、ダウンロード オプションの前のリンクを参照してください。 (**注:** このチュートリアルは PHP バージョン 7.4.4 で記述されています。 このガイドの手順は他のバージョンでも動作する可能性がありますが、テストは行ってはいではありません)。
- 開発用コンピューターにインストールされた[Composer。](https://getcomposer.org/)
- [Laravel が](https://laravel.com/) 開発用コンピューターにインストールされている。
- メールボックスがメールボックスを持つ個人の Microsoft アカウントOutlook.com Microsoft の仕事用アカウントまたは学校アカウント。

Microsoft アカウントをお持ちない場合は、無料アカウントを取得するためのオプションが 2 つ提供されています。

- 新しい [個人用 Microsoft アカウントにサインアップできます](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1)。
- Office [365 開発者プログラムにサインアップして、365](https://developer.microsoft.com/office/dev-program) サブスクリプションを無料Office取得できます。

## <a name="register-a-web-application-with-the-azure-active-directory-admin-center"></a>Azure Active Directory 管理センターに Web アプリケーションを登録する

1. ブラウザーを開き、[Azure Active Directory 管理センター](https://aad.portal.azure.com)へ移動します。 **個人用アカウント** (別名: Microsoft アカウント)、または **職場/学校アカウント** を使用してログインします。

1. 左側のナビゲーションで **[Azure Active Directory]** を選択し、それから **[管理]** で **[アプリの登録]** を選択します。

    ![アプリの登録のスクリーンショット ](/tutorial/images/aad-portal-app-registrations.png)

1. **[新規登録]** を選択します。 **[アプリケーションを登録]** ページで、次のように値を設定します。

    - `PHP Graph Tutorial` に **[名前]** を設定します。
    - **[サポートされているアカウントの種類]** を **[任意の組織のディレクトリ内のアカウントと個人用の Microsoft アカウント]** に設定します。
    - **[リダイレクト URI]** で、最初のドロップダウン リストを `Web` に設定し、それから `http://localhost:8000/callback` に値を設定します。

    ![[アプリケーションを登録する] ページのスクリーンショット](/tutorial/images/aad-register-an-app.png)

1. **[登録]** を選択します。 PHP **Graph チュートリアル ページ** で、アプリケーション **(クライアント) ID** の値をコピーして保存します。次の手順で必要になります。

    ![新しいアプリ登録のアプリケーション ID のスクリーンショット](/tutorial/images/aad-application-id.png)

1. **[管理]** で **[証明書とシークレット]** を選択します。 **[新しいクライアント シークレット]** ボタンを選択します。 **[説明]** に値を入力して、**[有効期限]** のオプションのいずれかを選び、**[追加]** を選択します。

    ![[クライアントシークレットの追加] ダイアログのスクリーンショット](/tutorial/images/aad-new-client-secret.png)

1. このページを離れる前に、クライアント シークレットの値をコピーします。 次の手順で行います。

    > [!IMPORTANT]
    > このクライアント シークレットは今後表示されないため、この段階で必ずコピーするようにしてください。

    ![新規追加されたクライアント シークレットのスクリーンショット](/tutorial/images/aad-copy-client-secret.png)

## <a name="configure-the-sample"></a>サンプルを構成する

1. ファイルの名前 `example.env` を変更します `.env` 。
1. ファイルを `.env` 編集し、次の変更を行います。
    1. アプリ `YOUR_APP_ID_HERE` 登録ポータル **から取得** したアプリケーション ID に置き換える。
    1. アプリ `YOUR_APP_PASSWORD_HERE` 登録ポータルから受け取ったパスワードに置き換える。
1. コマンド ライン インターフェイス (CLI) で、このディレクトリに移動し、次のコマンドを実行して要件をインストールします。

    ```Shell
    composer install
    ```

1. コマンド ライン インターフェイス (CLI) で、次のコマンドを実行してアプリケーション キーを生成します。

    ```Shell
    php artisan key:generate
    ```

## <a name="run-the-sample"></a>サンプルを実行する

1. CLI で次のコマンドを実行して、アプリケーションを起動します。

    ```Shell
    php artisan serve
    ```

1. Web ブラウザーを開き、`http://localhost:8000` を参照します。
