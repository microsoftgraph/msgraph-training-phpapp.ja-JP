<!-- markdownlint-disable MD002 MD041 -->

この演習では、前の演習のアプリケーションを拡張して、Azure AD での認証をサポートします。 これは、Microsoft Graph を呼び出すのに必要な OAuth アクセス トークンを取得するために必要です。 この手順では [、oauth2-client ライブラリを](https://github.com/thephpleague/oauth2-client) アプリケーションに統合します。

1. PHP アプリケーション **のルートにある .env** ファイルを開き、ファイルの末尾に次のコードを追加します。

    :::code language="ini" source="../demo/graph-tutorial/example.env" range="51-57":::

1. アプリケーション `YOUR_APP_ID_HERE` 登録ポータルのアプリケーション ID に置き換え、生成したパスワード `YOUR_APP_SECRET_HERE` に置き換える。

    > [!IMPORTANT]
    > git などのソース管理を使用している場合は、アプリ ID とパスワードが誤って漏洩しないように、ファイルをソース管理から除外する良い時期です `.env` 。

1. ./config フォルダーに新しい **ファイルを作成** し、 `azure.php` 次のコードを追加します。

    :::code language="php" source="../demo/graph-tutorial/config/azure.php":::

## <a name="implement-sign-in"></a>サインインの実装

1. **./app/Http/Controllers** ディレクトリに新しいファイルを作成し、次 `AuthController.php` のコードを追加します。

    ```php
    <?php

    namespace App\Http\Controllers;

    use App\Http\Controllers\Controller;
    use Illuminate\Http\Request;

    class AuthController extends Controller
    {
      public function signin()
      {
        // Initialize the OAuth client
        $oauthClient = new \League\OAuth2\Client\Provider\GenericProvider([
          'clientId'                => config('azure.appId'),
          'clientSecret'            => config('azure.appSecret'),
          'redirectUri'             => config('azure.redirectUri'),
          'urlAuthorize'            => config('azure.authority').config('azure.authorizeEndpoint'),
          'urlAccessToken'          => config('azure.authority').config('azure.tokenEndpoint'),
          'urlResourceOwnerDetails' => '',
          'scopes'                  => config('azure.scopes')
        ]);

        $authUrl = $oauthClient->getAuthorizationUrl();

        // Save client state so we can validate in callback
        session(['oauthState' => $oauthClient->getState()]);

        // Redirect to AAD signin page
        return redirect()->away($authUrl);
      }

      public function callback(Request $request)
      {
        // Validate state
        $expectedState = session('oauthState');
        $request->session()->forget('oauthState');
        $providedState = $request->query('state');

        if (!isset($expectedState)) {
          // If there is no expected state in the session,
          // do nothing and redirect to the home page.
          return redirect('/');
        }

        if (!isset($providedState) || $expectedState != $providedState) {
          return redirect('/')
            ->with('error', 'Invalid auth state')
            ->with('errorDetail', 'The provided auth state did not match the expected value');
        }

        // Authorization code should be in the "code" query param
        $authCode = $request->query('code');
        if (isset($authCode)) {
          // Initialize the OAuth client
          $oauthClient = new \League\OAuth2\Client\Provider\GenericProvider([
            'clientId'                => config('azure.appId'),
            'clientSecret'            => config('azure.appSecret'),
            'redirectUri'             => config('azure.redirectUri'),
            'urlAuthorize'            => config('azure.authority').config('azure.authorizeEndpoint'),
            'urlAccessToken'          => config('azure.authority').config('azure.tokenEndpoint'),
            'urlResourceOwnerDetails' => '',
            'scopes'                  => config('azure.scopes')
          ]);

          try {
            // Make the token request
            $accessToken = $oauthClient->getAccessToken('authorization_code', [
              'code' => $authCode
            ]);

            // TEMPORARY FOR TESTING!
            return redirect('/')
              ->with('error', 'Access token received')
              ->with('errorDetail', $accessToken->getToken());
          }
          catch (League\OAuth2\Client\Provider\Exception\IdentityProviderException $e) {
            return redirect('/')
              ->with('error', 'Error requesting access token')
              ->with('errorDetail', $e->getMessage());
          }
        }

        return redirect('/')
          ->with('error', $request->query('error'))
          ->with('errorDetail', $request->query('error_description'));
      }
    }
    ```

    これにより、次の 2 つのアクションを持つコントローラーが `signin` 定義されます `callback` 。

    このアクションにより、Azure AD サインイン URL が生成され `signin` 、OAuth クライアントによって生成された値が保存され、ブラウザーが Azure AD サインイン ページにリダイレクトされます `state` 。

    アクション `callback` は、サインインの完了後に Azure がリダイレクトする場所です。 このアクションにより、値が保存された値と一致する必要があります。その後、ユーザーはアクセス トークンを要求するために Azure から送信された認証 `state` コードを使用します。 次に、一時的なエラー値でアクセス トークンを使用してホーム ページにリダイレクトします。 これを使用して、次に進む前にサインインが機能しているのを確認します。

1. ルートを **./routes/web.php に追加します**。

    ```php
    Route::get('/signin', 'AuthController@signin');
    Route::get('/callback', 'AuthController@callback');
    ```

1. サーバーを起動し、参照します `https://localhost:8000` 。 [サインイン] ボタンをクリックすると、`https://login.microsoftonline.com` にリダイレクトされます。 Microsoft アカウントでログインします。

1. 同意プロンプトを確認します。 アクセス許可の一覧は、.env で構成されたアクセス許可スコープの一覧 **に対応します**。

    - **アクセス権を付与** したデータへのアクセスを維持する: ( ) このアクセス許可は、更新トークンを取得するために `offline_access` MSAL によって要求されます。
    - **サインインしてプロファイルを** 読み取る: ( ) このアクセス許可により、アプリはログインしているユーザーのプロファイルとプロファイル写真 `User.Read` を取得できます。
    - **メールボックスの設定を読み取る:** ( ) このアクセス許可により、アプリはタイム ゾーンや時刻形式を含むユーザーのメールボックス設定 `MailboxSettings.Read` を読み取りできます。
    - **予定表へのフル** アクセス権: ( ) このアクセス許可により、アプリはユーザーの予定表のイベントの読み取り、新しいイベントの追加、既存のイベントの変更を `Calendars.ReadWrite` 行います。

1. 要求されたアクセス許可に同意します。 ブラウザーがアプリにリダイレクトし、トークンが表示されます。

### <a name="get-user-details"></a>ユーザーの詳細情報を取得する

このセクションでは、Microsoft Graph からユーザーのプロファイルを取得するメソッド `callback` を更新します。

1. 次の `use` ステートメントを行の下の **/app/Http/Controllers/AuthController.php** の上部に追加 `namespace App\Http\Controllers;` します。

    ```php
    use Microsoft\Graph\Graph;
    use Microsoft\Graph\Model;
    ```

1. メソッド内 `try` のブロックを `callback` 次のコードに置き換えます。

    ```php
    try {
      // Make the token request
      $accessToken = $oauthClient->getAccessToken('authorization_code', [
        'code' => $authCode
      ]);

      $graph = new Graph();
      $graph->setAccessToken($accessToken->getToken());

      $user = $graph->createRequest('GET', '/me?$select=displayName,mail,mailboxSettings,userPrincipalName')
        ->setReturnType(Model\User::class)
        ->execute();

      // TEMPORARY FOR TESTING!
      return redirect('/')
        ->with('error', 'Access token received')
        ->with('errorDetail', 'User:'.$user->getDisplayName().', Token:'.$accessToken->getToken());
    }
    ```

新しいコードはオブジェクトを作成し、アクセス トークンを割り当て、それを使用してユーザーのプロファイル `Graph` を要求します。 テスト用の一時的な出力にユーザーの表示名を追加します。

## <a name="storing-the-tokens"></a>トークンの格納

トークンを取得できるようになったので、トークンをアプリに格納する手順を実装します。 これはサンプル アプリのため、わかりやすくするために、セッションに保存します。 実際のアプリでは、データベースのような、より信頼性の高い安全なストレージ ソリューションを使用します。

1. **./app** ディレクトリに新しいディレクトリを作成し、そのディレクトリに新しいファイルを作成し、次の `TokenStore` `TokenCache.php` コードを追加します。

    ```php
    <?php

    namespace App\TokenStore;

    class TokenCache {
      public function storeTokens($accessToken, $user) {
        session([
          'accessToken' => $accessToken->getToken(),
          'refreshToken' => $accessToken->getRefreshToken(),
          'tokenExpires' => $accessToken->getExpires(),
          'userName' => $user->getDisplayName(),
          'userEmail' => null !== $user->getMail() ? $user->getMail() : $user->getUserPrincipalName(),
          'userTimeZone' => $user->getMailboxSettings()->getTimeZone()
        ]);
      }

      public function clearTokens() {
        session()->forget('accessToken');
        session()->forget('refreshToken');
        session()->forget('tokenExpires');
        session()->forget('userName');
        session()->forget('userEmail');
        session()->forget('userTimeZone');
      }

      public function getAccessToken() {
        // Check if tokens exist
        if (empty(session('accessToken')) ||
            empty(session('refreshToken')) ||
            empty(session('tokenExpires'))) {
          return '';
        }

        return session('accessToken');
      }
    }
    ```

1. 次のステートメントを行の下の `use` **./app/Http/Controllers/AuthController.php** の上部に追加 `namespace App\Http\Controllers;` します。

    ```php
    use App\TokenStore\TokenCache;
    ```

1. 既存の `try` 関数のブロックを次 `callback` に置き換える。

    :::code language="php" source="../demo/graph-tutorial/app/Http/Controllers/AuthController.php" id="StoreTokensSnippet":::

## <a name="implement-sign-out"></a>サインアウトを実装する

この新機能をテストする前に、サインアウトする方法を追加します。

1. 次のアクションをクラスに追加 `AuthController` します。

    :::code language="php" source="../demo/graph-tutorial/app/Http/Controllers/AuthController.php" id="SignOutSnippet":::

1. このアクションを **./routes/web.php に追加します**。

    ```php
    Route::get('/signout', 'AuthController@signout');
    ```

1. サーバーを再起動し、サインイン プロセスを実行します。 ホーム ページに戻る必要がありますが、サインイン中を示すために UI が変更される必要があります。

    ![サインイン後のホーム ページのスクリーンショット](./images/add-aad-auth-01.png)

1. 右上隅にあるユーザー アバターをクリックして、[サインアウト **] リンクにアクセス** します。 **[サインアウト]** をクリックすると、セッションがリセットされ、ホーム ページに戻ります。

    ![[サインアウト] リンクのドロップダウン メニューのスクリーンショット](./images/add-aad-auth-02.png)

## <a name="refreshing-tokens"></a>トークンの更新

この時点で、アプリケーションはアクセス トークンを持ち、API 呼び出しのヘッダー `Authorization` で送信されます。 これは、アプリがユーザーの代わりに Microsoft Graph にアクセスできるトークンです。

ただし、このトークンは一時的なものです。 トークンは発行後 1 時間で期限切れになります。 ここで、更新トークンが役に立ちます。 更新トークンを使用すると、ユーザーが再度サインインしなくても、アプリは新しいアクセス トークンを要求できます。 トークン更新を実装するためにトークン管理コードを更新します。

1. **./app/TokenStore/TokenCache.php** を開き、次の関数をクラスに追加 `TokenCache` します。

    :::code language="php" source="../demo/graph-tutorial/app/TokenStore/TokenCache.php" id="UpdateTokensSnippet":::

1. 既存の `getAccessToken` 関数を、以下の関数で置き換えます。

    :::code language="php" source="../demo/graph-tutorial/app/TokenStore/TokenCache.php" id="GetAccessTokenSnippet":::

このメソッドは、最初にアクセス トークンの有効期限が切れているか、有効期限が近い状態になっているか確認します。 更新トークンがある場合は、更新トークンを使用して新しいトークンを取得し、キャッシュを更新して新しいアクセス トークンを返します。
