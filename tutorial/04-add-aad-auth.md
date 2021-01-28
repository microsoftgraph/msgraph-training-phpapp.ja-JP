<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="8a460-101">この演習では、前の演習のアプリケーションを拡張して、Azure AD での認証をサポートします。</span><span class="sxs-lookup"><span data-stu-id="8a460-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="8a460-102">これは、Microsoft Graph を呼び出すのに必要な OAuth アクセス トークンを取得するために必要です。</span><span class="sxs-lookup"><span data-stu-id="8a460-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="8a460-103">この手順では [、oauth2-client ライブラリを](https://github.com/thephpleague/oauth2-client) アプリケーションに統合します。</span><span class="sxs-lookup"><span data-stu-id="8a460-103">In this step you will integrate the [oauth2-client](https://github.com/thephpleague/oauth2-client) library into the application.</span></span>

1. <span data-ttu-id="8a460-104">PHP アプリケーション **のルートにある .env** ファイルを開き、ファイルの末尾に次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="8a460-104">Open the **.env** file in the root of your PHP application, and add the following code to the end of the file.</span></span>

    :::code language="ini" source="../demo/graph-tutorial/example.env" range="51-57":::

1. <span data-ttu-id="8a460-105">アプリケーション `YOUR_APP_ID_HERE` 登録ポータルのアプリケーション ID に置き換え、生成したパスワード `YOUR_APP_SECRET_HERE` に置き換える。</span><span class="sxs-lookup"><span data-stu-id="8a460-105">Replace `YOUR_APP_ID_HERE` with the application ID from the Application Registration Portal, and replace `YOUR_APP_SECRET_HERE` with the password you generated.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="8a460-106">git などのソース管理を使用している場合は、アプリ ID とパスワードが誤って漏洩しないように、ファイルをソース管理から除外する良い時期です `.env` 。</span><span class="sxs-lookup"><span data-stu-id="8a460-106">If you're using source control such as git, now would be a good time to exclude the `.env` file from source control to avoid inadvertently leaking your app ID and password.</span></span>

1. <span data-ttu-id="8a460-107">./config フォルダーに新しい **ファイルを作成** し、 `azure.php` 次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="8a460-107">Create a new file in the **./config** folder named `azure.php` and add the following code.</span></span>

    :::code language="php" source="../demo/graph-tutorial/config/azure.php":::

## <a name="implement-sign-in"></a><span data-ttu-id="8a460-108">サインインの実装</span><span class="sxs-lookup"><span data-stu-id="8a460-108">Implement sign-in</span></span>

1. <span data-ttu-id="8a460-109">**./app/Http/Controllers** ディレクトリに新しいファイルを作成し、次 `AuthController.php` のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="8a460-109">Create a new file in the **./app/Http/Controllers** directory named `AuthController.php` and add the following code.</span></span>

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

    <span data-ttu-id="8a460-110">これにより、次の 2 つのアクションを持つコントローラーが `signin` 定義されます `callback` 。</span><span class="sxs-lookup"><span data-stu-id="8a460-110">This defines a controller with two actions: `signin` and `callback`.</span></span>

    <span data-ttu-id="8a460-111">このアクションにより、Azure AD サインイン URL が生成され `signin` 、OAuth クライアントによって生成された値が保存され、ブラウザーが Azure AD サインイン ページにリダイレクトされます `state` 。</span><span class="sxs-lookup"><span data-stu-id="8a460-111">The `signin` action generates the Azure AD signin URL, saves the `state` value generated by the OAuth client, then redirects the browser to the Azure AD signin page.</span></span>

    <span data-ttu-id="8a460-112">アクション `callback` は、サインインの完了後に Azure がリダイレクトする場所です。</span><span class="sxs-lookup"><span data-stu-id="8a460-112">The `callback` action is where Azure redirects after the signin is complete.</span></span> <span data-ttu-id="8a460-113">このアクションにより、値が保存された値と一致する必要があります。その後、ユーザーはアクセス トークンを要求するために Azure から送信された認証 `state` コードを使用します。</span><span class="sxs-lookup"><span data-stu-id="8a460-113">That action makes sure the `state` value matches the saved value, then users the authorization code sent by Azure to request an access token.</span></span> <span data-ttu-id="8a460-114">次に、一時的なエラー値でアクセス トークンを使用してホーム ページにリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="8a460-114">It then redirects back to the home page with the access token in the temporary error value.</span></span> <span data-ttu-id="8a460-115">これを使用して、次に進む前にサインインが機能しているのを確認します。</span><span class="sxs-lookup"><span data-stu-id="8a460-115">You'll use this to verify that sign-in is working before moving on.</span></span>

1. <span data-ttu-id="8a460-116">ルートを **./routes/web.php に追加します**。</span><span class="sxs-lookup"><span data-stu-id="8a460-116">Add the routes to **./routes/web.php**.</span></span>

    ```php
    Route::get('/signin', 'AuthController@signin');
    Route::get('/callback', 'AuthController@callback');
    ```

1. <span data-ttu-id="8a460-117">サーバーを起動し、参照します `https://localhost:8000` 。</span><span class="sxs-lookup"><span data-stu-id="8a460-117">Start the server and browse to `https://localhost:8000`.</span></span> <span data-ttu-id="8a460-118">[サインイン] ボタンをクリックすると、`https://login.microsoftonline.com` にリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="8a460-118">Click the sign-in button and you should be redirected to `https://login.microsoftonline.com`.</span></span> <span data-ttu-id="8a460-119">Microsoft アカウントでログインします。</span><span class="sxs-lookup"><span data-stu-id="8a460-119">Login with your Microsoft account.</span></span>

1. <span data-ttu-id="8a460-120">同意プロンプトを確認します。</span><span class="sxs-lookup"><span data-stu-id="8a460-120">Examine the consent prompt.</span></span> <span data-ttu-id="8a460-121">アクセス許可の一覧は、.env で構成されたアクセス許可スコープの一覧 **に対応します**。</span><span class="sxs-lookup"><span data-stu-id="8a460-121">The list of permissions correspond to list of permissions scopes configured in **.env**.</span></span>

    - <span data-ttu-id="8a460-122">**アクセス権を付与** したデータへのアクセスを維持する: ( ) このアクセス許可は、更新トークンを取得するために `offline_access` MSAL によって要求されます。</span><span class="sxs-lookup"><span data-stu-id="8a460-122">**Maintain access to data you have given it access to:** (`offline_access`) This permission is requested by MSAL in order to retrieve refresh tokens.</span></span>
    - <span data-ttu-id="8a460-123">**サインインしてプロファイルを** 読み取る: ( ) このアクセス許可により、アプリはログインしているユーザーのプロファイルとプロファイル写真 `User.Read` を取得できます。</span><span class="sxs-lookup"><span data-stu-id="8a460-123">**Sign you in and read your profile:** (`User.Read`) This permission allows the app to get the logged-in user's profile and profile photo.</span></span>
    - <span data-ttu-id="8a460-124">**メールボックスの設定を読み取る:** ( ) このアクセス許可により、アプリはタイム ゾーンや時刻形式を含むユーザーのメールボックス設定 `MailboxSettings.Read` を読み取りできます。</span><span class="sxs-lookup"><span data-stu-id="8a460-124">**Read your mailbox settings:** (`MailboxSettings.Read`) This permission allows the app to read the user's mailbox settings, including time zone and time format.</span></span>
    - <span data-ttu-id="8a460-125">**予定表へのフル** アクセス権: ( ) このアクセス許可により、アプリはユーザーの予定表のイベントの読み取り、新しいイベントの追加、既存のイベントの変更を `Calendars.ReadWrite` 行います。</span><span class="sxs-lookup"><span data-stu-id="8a460-125">**Have full access to your calendars:** (`Calendars.ReadWrite`) This permission allows the app to read events on the user's calendar, add new events, and modify existing ones.</span></span>

1. <span data-ttu-id="8a460-126">要求されたアクセス許可に同意します。</span><span class="sxs-lookup"><span data-stu-id="8a460-126">Consent to the requested permissions.</span></span> <span data-ttu-id="8a460-127">ブラウザーがアプリにリダイレクトし、トークンが表示されます。</span><span class="sxs-lookup"><span data-stu-id="8a460-127">The browser redirects to the app, showing the token.</span></span>

### <a name="get-user-details"></a><span data-ttu-id="8a460-128">ユーザーの詳細情報を取得する</span><span class="sxs-lookup"><span data-stu-id="8a460-128">Get user details</span></span>

<span data-ttu-id="8a460-129">このセクションでは、Microsoft Graph からユーザーのプロファイルを取得するメソッド `callback` を更新します。</span><span class="sxs-lookup"><span data-stu-id="8a460-129">In this section you'll update the `callback` method to get the user's profile from Microsoft Graph.</span></span>

1. <span data-ttu-id="8a460-130">次の `use` ステートメントを行の下の **/app/Http/Controllers/AuthController.php** の上部に追加 `namespace App\Http\Controllers;` します。</span><span class="sxs-lookup"><span data-stu-id="8a460-130">Add the following `use` statements to the top of **/app/Http/Controllers/AuthController.php**, beneath the `namespace App\Http\Controllers;` line.</span></span>

    ```php
    use Microsoft\Graph\Graph;
    use Microsoft\Graph\Model;
    ```

1. <span data-ttu-id="8a460-131">メソッド内 `try` のブロックを `callback` 次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="8a460-131">Replace the `try` block in the `callback` method with the following code.</span></span>

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

<span data-ttu-id="8a460-132">新しいコードはオブジェクトを作成し、アクセス トークンを割り当て、それを使用してユーザーのプロファイル `Graph` を要求します。</span><span class="sxs-lookup"><span data-stu-id="8a460-132">The new code creates a `Graph` object, assigns the access token, then uses it to request the user's profile.</span></span> <span data-ttu-id="8a460-133">テスト用の一時的な出力にユーザーの表示名を追加します。</span><span class="sxs-lookup"><span data-stu-id="8a460-133">It adds the user's display name to the temporary output for testing.</span></span>

## <a name="storing-the-tokens"></a><span data-ttu-id="8a460-134">トークンの格納</span><span class="sxs-lookup"><span data-stu-id="8a460-134">Storing the tokens</span></span>

<span data-ttu-id="8a460-135">トークンを取得できるようになったので、トークンをアプリに格納する手順を実装します。</span><span class="sxs-lookup"><span data-stu-id="8a460-135">Now that you can get tokens, it's time to implement a way to store them in the app.</span></span> <span data-ttu-id="8a460-136">これはサンプル アプリのため、わかりやすくするために、セッションに保存します。</span><span class="sxs-lookup"><span data-stu-id="8a460-136">Since this is a sample app, for simplicity's sake, you'll store them in the session.</span></span> <span data-ttu-id="8a460-137">実際のアプリでは、データベースのような、より信頼性の高い安全なストレージ ソリューションを使用します。</span><span class="sxs-lookup"><span data-stu-id="8a460-137">A real-world app would use a more reliable secure storage solution, like a database.</span></span>

1. <span data-ttu-id="8a460-138">**./app** ディレクトリに新しいディレクトリを作成し、そのディレクトリに新しいファイルを作成し、次の `TokenStore` `TokenCache.php` コードを追加します。</span><span class="sxs-lookup"><span data-stu-id="8a460-138">Create a new directory in the **./app** directory named `TokenStore`, then create a new file in that directory named `TokenCache.php`, and add the following code.</span></span>

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

1. <span data-ttu-id="8a460-139">次のステートメントを行の下の `use` **./app/Http/Controllers/AuthController.php** の上部に追加 `namespace App\Http\Controllers;` します。</span><span class="sxs-lookup"><span data-stu-id="8a460-139">Add the following `use` statement to the top of **./app/Http/Controllers/AuthController.php**, beneath the `namespace App\Http\Controllers;` line.</span></span>

    ```php
    use App\TokenStore\TokenCache;
    ```

1. <span data-ttu-id="8a460-140">既存の `try` 関数のブロックを次 `callback` に置き換える。</span><span class="sxs-lookup"><span data-stu-id="8a460-140">Replace the `try` block in the existing `callback` function with the following.</span></span>

    :::code language="php" source="../demo/graph-tutorial/app/Http/Controllers/AuthController.php" id="StoreTokensSnippet":::

## <a name="implement-sign-out"></a><span data-ttu-id="8a460-141">サインアウトを実装する</span><span class="sxs-lookup"><span data-stu-id="8a460-141">Implement sign-out</span></span>

<span data-ttu-id="8a460-142">この新機能をテストする前に、サインアウトする方法を追加します。</span><span class="sxs-lookup"><span data-stu-id="8a460-142">Before you test this new feature, add a way to sign out.</span></span>

1. <span data-ttu-id="8a460-143">次のアクションをクラスに追加 `AuthController` します。</span><span class="sxs-lookup"><span data-stu-id="8a460-143">Add the following action to the `AuthController` class.</span></span>

    :::code language="php" source="../demo/graph-tutorial/app/Http/Controllers/AuthController.php" id="SignOutSnippet":::

1. <span data-ttu-id="8a460-144">このアクションを **./routes/web.php に追加します**。</span><span class="sxs-lookup"><span data-stu-id="8a460-144">Add this action to **./routes/web.php**.</span></span>

    ```php
    Route::get('/signout', 'AuthController@signout');
    ```

1. <span data-ttu-id="8a460-145">サーバーを再起動し、サインイン プロセスを実行します。</span><span class="sxs-lookup"><span data-stu-id="8a460-145">Restart the server and go through the sign-in process.</span></span> <span data-ttu-id="8a460-146">ホーム ページに戻る必要がありますが、サインイン中を示すために UI が変更される必要があります。</span><span class="sxs-lookup"><span data-stu-id="8a460-146">You should end up back on the home page, but the UI should change to indicate that you are signed-in.</span></span>

    ![サインイン後のホーム ページのスクリーンショット](./images/add-aad-auth-01.png)

1. <span data-ttu-id="8a460-148">右上隅にあるユーザー アバターをクリックして、[サインアウト **] リンクにアクセス** します。</span><span class="sxs-lookup"><span data-stu-id="8a460-148">Click the user avatar in the top right corner to access the **Sign Out** link.</span></span> <span data-ttu-id="8a460-149">**[サインアウト]** をクリックすると、セッションがリセットされ、ホーム ページに戻ります。</span><span class="sxs-lookup"><span data-stu-id="8a460-149">Clicking **Sign Out** resets the session and returns you to the home page.</span></span>

    ![[サインアウト] リンクのドロップダウン メニューのスクリーンショット](./images/add-aad-auth-02.png)

## <a name="refreshing-tokens"></a><span data-ttu-id="8a460-151">トークンの更新</span><span class="sxs-lookup"><span data-stu-id="8a460-151">Refreshing tokens</span></span>

<span data-ttu-id="8a460-152">この時点で、アプリケーションはアクセス トークンを持ち、API 呼び出しのヘッダー `Authorization` で送信されます。</span><span class="sxs-lookup"><span data-stu-id="8a460-152">At this point your application has an access token, which is sent in the `Authorization` header of API calls.</span></span> <span data-ttu-id="8a460-153">これは、アプリがユーザーの代わりに Microsoft Graph にアクセスできるトークンです。</span><span class="sxs-lookup"><span data-stu-id="8a460-153">This is the token that allows the app to access the Microsoft Graph on the user's behalf.</span></span>

<span data-ttu-id="8a460-154">ただし、このトークンは一時的なものです。</span><span class="sxs-lookup"><span data-stu-id="8a460-154">However, this token is short-lived.</span></span> <span data-ttu-id="8a460-155">トークンは発行後 1 時間で期限切れになります。</span><span class="sxs-lookup"><span data-stu-id="8a460-155">The token expires an hour after it is issued.</span></span> <span data-ttu-id="8a460-156">ここで、更新トークンが役に立ちます。</span><span class="sxs-lookup"><span data-stu-id="8a460-156">This is where the refresh token becomes useful.</span></span> <span data-ttu-id="8a460-157">更新トークンを使用すると、ユーザーが再度サインインしなくても、アプリは新しいアクセス トークンを要求できます。</span><span class="sxs-lookup"><span data-stu-id="8a460-157">The refresh token allows the app to request a new access token without requiring the user to sign in again.</span></span> <span data-ttu-id="8a460-158">トークン更新を実装するためにトークン管理コードを更新します。</span><span class="sxs-lookup"><span data-stu-id="8a460-158">Update the token management code to implement token refresh.</span></span>

1. <span data-ttu-id="8a460-159">**./app/TokenStore/TokenCache.php** を開き、次の関数をクラスに追加 `TokenCache` します。</span><span class="sxs-lookup"><span data-stu-id="8a460-159">Open **./app/TokenStore/TokenCache.php** and add the following function to the `TokenCache` class.</span></span>

    :::code language="php" source="../demo/graph-tutorial/app/TokenStore/TokenCache.php" id="UpdateTokensSnippet":::

1. <span data-ttu-id="8a460-160">既存の `getAccessToken` 関数を、以下の関数で置き換えます。</span><span class="sxs-lookup"><span data-stu-id="8a460-160">Replace the existing `getAccessToken` function with the following.</span></span>

    :::code language="php" source="../demo/graph-tutorial/app/TokenStore/TokenCache.php" id="GetAccessTokenSnippet":::

<span data-ttu-id="8a460-161">このメソッドは、最初にアクセス トークンの有効期限が切れているか、有効期限が近い状態になっているか確認します。</span><span class="sxs-lookup"><span data-stu-id="8a460-161">This method first checks if the access token is expired or close to expiring.</span></span> <span data-ttu-id="8a460-162">更新トークンがある場合は、更新トークンを使用して新しいトークンを取得し、キャッシュを更新して新しいアクセス トークンを返します。</span><span class="sxs-lookup"><span data-stu-id="8a460-162">If it is, then it uses the refresh token to get new tokens, then updates the cache and returns the new access token.</span></span>
