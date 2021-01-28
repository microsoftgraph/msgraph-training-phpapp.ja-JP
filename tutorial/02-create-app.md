<!-- markdownlint-disable MD002 MD041 -->

まず、新しい Laravel プロジェクトを作成します。

1. コマンドライン インターフェイス (CLI) を開き、ファイルを作成する権限を持つディレクトリに移動し、次のコマンドを実行して新しい PHP アプリを作成します。

    ```Shell
    laravel new graph-tutorial
    ```

1. **graph-tutorial ディレクトリに移動し**、次のコマンドを入力してローカル Web サーバーを起動します。

    ```Shell
    php artisan serve
    ```

1. ブラウザーを開き、`http://localhost:8000` に移動します。 すべてが動作している場合は、既定の Laravel ページが表示されます。 If you don't see that page, check the [Laravel docs](https://laravel.com/docs/8.x).

## <a name="install-packages"></a>パッケージをインストールする

次に進む前に、後で使用する追加のパッケージをインストールします。

- [サインインおよび OAuth](https://github.com/thephpleague/oauth2-client) トークン フローを処理する oauth2-client。
- Microsoft Graph を呼び出す[microsoft-graph。](https://github.com/microsoftgraph/msgraph-sdk-php)

1. CLI で次のコマンドを実行します。

    ```Shell
    composer require league/oauth2-client microsoft/microsoft-graph
    ```

## <a name="design-the-app"></a>アプリを設計する

1. **./resources/views** ディレクトリに新しいファイルを作成し、 `layout.blade.php` 次のコードを追加します。

    :::code language="php" source="../demo/graph-tutorial/resources/views/layout.blade.php" id="LayoutSnippet":::

    このコードはシンプルなスタイルのために [Bootstrap](http://getbootstrap.com/) を追加し、シンプルなアイコンのために [Font Awesome](https://fontawesome.com/) を追加します。 また、ナビゲーション バーを含むグローバル レイアウトも定義します。

1. ディレクトリに新しいディレクトリを `./public` 作成し、そのディレクトリに新しい `css` ファイル `./public/css` を作成します `app.css` 。 次のコードを追加します。

    :::code language="css" source="../demo/graph-tutorial/public/css/app.css":::

1. ファイルを `./resources/views/welcome.blade.php` 開き、その内容を次の内容に置き換えてください。

    :::code language="php" source="../demo/graph-tutorial/resources/views/welcome.blade.php" id="WelcomeSnippet":::

1. クラスに次の関数を追加して `Controller` **、./app/Http/Controllers/Controller.php** の基本クラスを更新します。

    :::code language="php" source="../demo/graph-tutorial/app/Http/Controllers/Controller.php" id="LoadViewDataSnippet":::

1. ディレクトリに新しいファイルを `./app/Http/Controllers` 作成し、 `HomeController.php` 次のコードを追加します。

    :::code language="php" source="../demo/graph-tutorial/app/Http/Controllers/HomeController.php":::

1. 新しいコントローラーを使用 `./routes/web.php` するためにルートを更新します。 このファイルの内容全体を次の内容に置き換えてください。

    ```php
    <?php

    use Illuminate\Support\Facades\Route;

    Route::get('/', 'HomeController@welcome');
    ```

1. **./app/Providers/RouteServiceProvider.php** を開き、宣言のコミットを解除 `$namespace` します。

    ```php
    /**
     * This namespace is applied to your controller routes.
     *
     * In addition, it is set as the URL generator's root namespace.
     *
     * @var string
     */
    protected $namespace = 'App\Http\Controllers';
    ```

1. 変更内容をすべて保存し、サーバーを再起動します。 これで、アプリは非常に異なって表示されます。

    ![デザインが変更されたホーム ページのスクリーンショット](./images/create-app-01.png)
