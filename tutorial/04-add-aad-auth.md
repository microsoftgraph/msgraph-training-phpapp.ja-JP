<!-- markdownlint-disable MD002 MD041 -->

この演習では、Azure AD での認証をサポートするために、前の手順で作成したアプリケーションを拡張します。 これは、Microsoft Graph を呼び出すために必要な OAuth アクセストークンを取得するために必要です。 この手順では、 [oauth2](https://github.com/thephpleague/oauth2-client) ライブラリをアプリケーションに統合します。

1. PHP アプリケーションのルートにある **restore.env** ファイルを開き、ファイルの末尾に次のコードを追加します。

    :::code language="ini" source="../demo/graph-tutorial/.env.example" id="OAuthSettingsSnippet":::

1. を `YOUR_APP_ID_HERE` アプリケーション登録ポータルからのアプリケーション ID に置き換え、生成し `YOUR_APP_PASSWORD_HERE` たパスワードに置き換えます。

    > [!IMPORTANT]
    > Git などのソース管理を使用している場合は、 `.env` アプリ ID とパスワードを誤ってリークしないように、ソース管理からファイルを除外することをお勧めします。

## <a name="implement-sign-in"></a>サインインの実装

1. という名前の **/app/Http/Controllers** ディレクトリに新しいファイルを作成 `AuthController.php` し、次のコードを追加します。

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
          'clientId'                => env('OAUTH_APP_ID'),
          'clientSecret'            => env('OAUTH_APP_PASSWORD'),
          'redirectUri'             => env('OAUTH_REDIRECT_URI'),
          'urlAuthorize'            => env('OAUTH_AUTHORITY').env('OAUTH_AUTHORIZE_ENDPOINT'),
          'urlAccessToken'          => env('OAUTH_AUTHORITY').env('OAUTH_TOKEN_ENDPOINT'),
          'urlResourceOwnerDetails' => '',
          'scopes'                  => env('OAUTH_SCOPES')
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
            'clientId'                => env('OAUTH_APP_ID'),
            'clientSecret'            => env('OAUTH_APP_PASSWORD'),
            'redirectUri'             => env('OAUTH_REDIRECT_URI'),
            'urlAuthorize'            => env('OAUTH_AUTHORITY').env('OAUTH_AUTHORIZE_ENDPOINT'),
            'urlAccessToken'          => env('OAUTH_AUTHORITY').env('OAUTH_TOKEN_ENDPOINT'),
            'urlResourceOwnerDetails' => '',
            'scopes'                  => env('OAUTH_SCOPES')
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

    これにより、という2つのアクションを持つコントローラーが定義されます。 `signin` `callback`

    この `signin` アクションは、AZURE ad サインイン URL を生成し、 `state` OAuth クライアントによって生成された値を保存してから、ブラウザーを azure ad サインインページにリダイレクトします。

    この `callback` 操作では、サインインの完了後に Azure がリダイレクトされます。 この操作により、 `state` 値が保存された値と一致するようになり、ユーザーは、アクセストークンを要求するために Azure によって送信された認証コードを確認します。 その後、一時的なエラー値でアクセストークンを使用して、ホームページにリダイレクトします。 これを使用して、サインインが機能していることを確認してから、に進みます。

1. **/Routes/web.php**にルートを追加します。

    ```php
    Route::get('/signin', 'AuthController@signin');
    Route::get('/callback', 'AuthController@callback');
    ```

1. サーバーを起動し、を参照し `https://localhost:8000` ます。 [サインイン] ボタンをクリックすると、`https://login.microsoftonline.com` にリダイレクトされます。 Microsoft アカウントを使用してログインします。

1. 同意プロンプトを調べます。 アクセス許可の一覧は、 **env**で構成されているアクセス許可スコープの一覧に対応します。

    - **アクセス権が付与されているデータへのアクセスを保持する:** ( `offline_access` ) 更新トークンを取得するために、このアクセス許可は msal によって要求されます。
    - **サインインしてプロファイルを読み取る:** ( `User.Read` ) このアクセス許可により、アプリはログインしているユーザーのプロファイルとプロファイル写真を取得できます。
    - **メールボックスの設定の読み取り:** ( `MailboxSettings.Read` ) このアクセス許可により、アプリは、タイムゾーンや時刻の形式など、ユーザーのメールボックス設定を読み取ることができます。
    - **予定表へのフルアクセス:** ( `Calendars.ReadWrite` ) この権限によって、アプリはユーザーの予定表にあるイベントの読み取り、新しいイベントの追加、および既存のイベントの変更を行うことができます。

1. 要求されたアクセス許可への同意。 ブラウザーがアプリにリダイレクトし、トークンが表示されます。

### <a name="get-user-details"></a>ユーザーの詳細情報を取得する

このセクションでは、 `callback` Microsoft Graph からユーザーのプロファイルを取得するメソッドを更新します。

1. 次 `use` のステートメントを **/app/Http/Controllers/AuthController.php**の先頭に追加し `namespace App\Http\Controllers;` ます。行の下にあります。

    ```php
    use Microsoft\Graph\Graph;
    use Microsoft\Graph\Model;
    ```

1. `try`メソッド内のブロックを `callback` 次のコードに置き換えます。

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

新しいコードは、オブジェクトを作成し、 `Graph` アクセストークンを割り当ててから、それを使用してユーザーのプロファイルを要求します。 テストのために、ユーザーの表示名を一時出力に追加します。

## <a name="storing-the-tokens"></a>トークンの格納

トークンを取得できるようになったので、トークンをアプリに格納する手順を実装します。 これはサンプルアプリなので、わかりやすくするために、セッションに格納します。 実際のアプリでは、データベースのような、より信頼性の高い安全なストレージ ソリューションを使用します。

1. という名前の **アプリケーション** ディレクトリに新しいディレクトリを作成し、 `TokenStore` そのディレクトリに新しいファイルを作成して `TokenCache.php` 、次のコードを追加します。

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
          'userEmail' => null !== $user->getMail() ? $user->getMail() : $user->getUserPrincipalName()
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

1. 次のステートメントを/App/Http/Controllers/AuthController.php の先頭に追加します `use` **。** 行の下に `namespace App\Http\Controllers;` あります。

    ```php
    use App\TokenStore\TokenCache;
    ```

1. 既存の `try` 関数の block を `callback` 次のように置き換えます。

    :::code language="php" source="../demo/graph-tutorial/app/Http/Controllers/AuthController.php" id="StoreTokensSnippet":::

## <a name="implement-sign-out"></a>サインアウトを実装する

この新しい機能をテストする前に、サインアウトする方法を追加します。

1. クラスに次のアクションを追加し `AuthController` ます。

    :::code language="php" source="../demo/graph-tutorial/app/Http/Controllers/AuthController.php" id="SignOutSnippet":::

1. このアクションを/routes/web.php に追加します **。**

    ```php
    Route::get('/signout', 'AuthController@signout');
    ```

1. サーバーを再起動し、サインインプロセスを実行します。 ホームページに戻る必要がありますが、UI は、サインインしていることを示すように変更する必要があります。

    ![サインイン後のホーム ページのスクリーンショット](./images/add-aad-auth-01.png)

1. 右上隅にあるユーザーアバターをクリックして、[ **サインアウト** ] リンクにアクセスします。 **[サインアウト]** をクリックすると、セッションがリセットされ、ホーム ページに戻ります。

    ![[サインアウト] リンクのドロップダウン メニューのスクリーンショット](./images/add-aad-auth-02.png)

## <a name="refreshing-tokens"></a>トークンの更新

この時点で、アプリケーションには、API 呼び出しのヘッダーで送信されるアクセストークンがあり `Authorization` ます。 これは、アプリがユーザーに代わって Microsoft Graph にアクセスできるようにするトークンです。

ただし、このトークンは一時的なものです。 トークンが発行された後、有効期限が切れる時間になります。 ここで、更新トークンが役に立ちます。 更新トークンを使用すると、ユーザーが再度サインインしなくても、アプリは新しいアクセス トークンを要求できます。 トークンの更新を実装するために、トークン管理コードを更新します。

1. /App/TokenStore/TokenCache.php を開き、次の関数をクラスに追加します **。** `TokenCache`

    :::code language="php" source="../demo/graph-tutorial/app/TokenStore/TokenCache.php" id="UpdateTokensSnippet":::

1. 既存の `getAccessToken` 関数を、以下の関数で置き換えます。

    :::code language="php" source="../demo/graph-tutorial/app/TokenStore/TokenCache.php" id="GetAccessTokenSnippet":::

このメソッドは、最初にアクセストークンの有効期限が切れているか、期限切れになるかを確認します。 その場合は、更新トークンを使用して新しいトークンを取得し、次にキャッシュを更新して、新しいアクセストークンを返します。
