### <span data-ttu-id="b9472-101"><a name="server-auth"></a>Postup: Ověřování pomocí zprostředkovatele (tok na straně serveru)</span><span class="sxs-lookup"><span data-stu-id="b9472-101"><a name="server-auth"></a>How to: Authenticate with a provider (Server Flow)</span></span>
<span data-ttu-id="b9472-102">Pokud chcete, aby funkce Mobile Apps spravovala proces ověřování ve vaší aplikaci, je třeba aplikaci zaregistrovat u vašeho zprostředkovatele identity.</span><span class="sxs-lookup"><span data-stu-id="b9472-102">To have Mobile Apps manage the authentication process in your app, you must register your app with your identity provider.</span></span> <span data-ttu-id="b9472-103">Potom je nutné ve službě Azure App Service nakonfigurovat ID aplikace a tajný klíč, který vám poskytne zprostředkovatel.</span><span class="sxs-lookup"><span data-stu-id="b9472-103">Then in your Azure App Service, you need to configure the application ID and secret provided by your provider.</span></span>
<span data-ttu-id="b9472-104">Další informace najdete v kurzu [Přidání ověřování do aplikace](../articles/app-service-mobile/app-service-mobile-cordova-get-started-users.md).</span><span class="sxs-lookup"><span data-stu-id="b9472-104">For more information, see the tutorial [Add authentication to your app](../articles/app-service-mobile/app-service-mobile-cordova-get-started-users.md).</span></span>

<span data-ttu-id="b9472-105">Jakmile budete zaregistrováni u zprostředkovatele identity, zavolejte metodu `.login()` s názvem vašeho zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="b9472-105">Once you have registered your identity provider, call the `.login()` method with the name of your provider.</span></span> <span data-ttu-id="b9472-106">Například pro přihlášení přes Facebook použijte následující kód:</span><span class="sxs-lookup"><span data-stu-id="b9472-106">For example, to login with Facebook use the following code:</span></span>

```
client.login("facebook").done(function (results) {
     alert("You are now logged in as: " + results.userId);
}, function (err) {
     alert("Error: " + err);
});
```

<span data-ttu-id="b9472-107">Platné hodnoty pro zprostředkovatele jsou „aad“, „facebook“, „google“, „microsoftaccount“ a „twitter“.</span><span class="sxs-lookup"><span data-stu-id="b9472-107">The valid values for the provider are 'aad', 'facebook', 'google', 'microsoftaccount', and 'twitter'.</span></span>

> [!NOTE]
> <span data-ttu-id="b9472-108">Ověřování Google přes tok na straně serveru aktuálně nefunguje.</span><span class="sxs-lookup"><span data-stu-id="b9472-108">Google Authentication does not currently work via Server Flow.</span></span>  <span data-ttu-id="b9472-109">K ověřování Google je třeba použít [metodu toku na straně klienta](#client-auth).</span><span class="sxs-lookup"><span data-stu-id="b9472-109">To authenticate with Google, you must use a [client-flow method](#client-auth).</span></span>

<span data-ttu-id="b9472-110">V tomto případě služba Azure App Service spravuje tok ověřování OAuth 2.0.</span><span class="sxs-lookup"><span data-stu-id="b9472-110">In this case, Azure App Service manages the OAuth 2.0 authentication flow.</span></span>  <span data-ttu-id="b9472-111">Zobrazí přihlašovací stránku vybraného zprostředkovatele a po úspěšném přihlášení pomocí zprostředkovatele identity vygeneruje ověřovací token služby App Service.</span><span class="sxs-lookup"><span data-stu-id="b9472-111">It displays the login page of the selected provider and generates an App Service authentication token after successful login with the identity provider.</span></span> <span data-ttu-id="b9472-112">Funkce login po dokončení vrátí objekt JSON, který vystaví ID uživatele a ověřovací token služby App Service v polích userId a authenticationToken.</span><span class="sxs-lookup"><span data-stu-id="b9472-112">The login function, when complete, returns a JSON object that exposes both the user ID and App Service authentication token in the userId and authenticationToken fields, respectively.</span></span> <span data-ttu-id="b9472-113">Tento token se může uložit do mezipaměti a znovu požívat do vypršení platnosti.</span><span class="sxs-lookup"><span data-stu-id="b9472-113">This token can be cached and reused until it expires.</span></span>

###<span data-ttu-id="b9472-114"><a name="client-auth"></a>Postup: Ověřování pomocí zprostředkovatele (tok na straně klienta)</span><span class="sxs-lookup"><span data-stu-id="b9472-114"><a name="client-auth"></a>How to: Authenticate with a provider (Client Flow)</span></span>

<span data-ttu-id="b9472-115">Vaše aplikace také může nezávisle kontaktovat zprostředkovatele identity a následně poskytnout navrácený token službě App Service k ověření.</span><span class="sxs-lookup"><span data-stu-id="b9472-115">Your app can also independently contact the identity provider and then provide the returned token to your App Service for authentication.</span></span> <span data-ttu-id="b9472-116">Tento tok na straně klienta vám umožní poskytnout uživatelům jednotné přihlašování nebo od zprostředkovatele identity načíst další uživatelská data.</span><span class="sxs-lookup"><span data-stu-id="b9472-116">This client flow enables you to provide a single sign-in experience for users or to retrieve additional user data from the identity provider.</span></span>

#### <a name="social-authentication-basic-example"></a><span data-ttu-id="b9472-117">Základní příklad sociálního ověřování</span><span class="sxs-lookup"><span data-stu-id="b9472-117">Social Authentication basic example</span></span>

<span data-ttu-id="b9472-118">Tento příklad používá k ověřování klientskou sadu SDK Facebooku:</span><span class="sxs-lookup"><span data-stu-id="b9472-118">This example uses Facebook client SDK for authentication:</span></span>

```
client.login(
     "facebook",
     {"access_token": token})
.done(function (results) {
     alert("You are now logged in as: " + results.userId);
}, function (err) {
     alert("Error: " + err);
});

```
<span data-ttu-id="b9472-119">Tento příklad předpokládá, že token poskytnutý příslušnou sadou SDK zprostředkovatele je uložený v proměnné token.</span><span class="sxs-lookup"><span data-stu-id="b9472-119">This example assumes that the token provided by the respective provider SDK is stored in the token variable.</span></span>

#### <a name="microsoft-account-example"></a><span data-ttu-id="b9472-120">Příklad s účtem Microsoft</span><span class="sxs-lookup"><span data-stu-id="b9472-120">Microsoft Account example</span></span>

<span data-ttu-id="b9472-121">Následující příklad používá sadu Live SDK, která podporuje jednotné přihlašování v aplikacích pro Windows Store pomocí účtu Microsoft:</span><span class="sxs-lookup"><span data-stu-id="b9472-121">The following example uses the Live SDK, which supports single-sign-on for Windows Store apps by using Microsoft Account:</span></span>

```
WL.login({ scope: "wl.basic"}).then(function (result) {
      client.login(
            "microsoftaccount",
            {"authenticationToken": result.session.authentication_token})
      .done(function(results){
            alert("You are now logged in as: " + results.userId);
      },
      function(error){
            alert("Error: " + err);
      });
});

```

<span data-ttu-id="b9472-122">Tento příklad získá token ze služby Live Connect a zavoláním funkce login jej předá vaší službě App Service.</span><span class="sxs-lookup"><span data-stu-id="b9472-122">This example gets a token from Live Connect, which is supplied to your App Service by calling the login function.</span></span>

###<span data-ttu-id="b9472-123"><a name="auth-getinfo"></a>Postup: Získání informací o ověřeném uživateli</span><span class="sxs-lookup"><span data-stu-id="b9472-123"><a name="auth-getinfo"></a>How to: Obtain information about the authenticated user</span></span>

<span data-ttu-id="b9472-124">Ověřovací informace můžete načíst z koncového bodu `/.auth/me` voláním HTTP pomocí libovolné knihovny AJAX.</span><span class="sxs-lookup"><span data-stu-id="b9472-124">The authentication information can be retrieved from the `/.auth/me` endpoint using an HTTP call with any AJAX library.</span></span>  <span data-ttu-id="b9472-125">Ujistěte se, že jste hlavičku `X-ZUMO-AUTH` nastavili na váš ověřovací token.</span><span class="sxs-lookup"><span data-stu-id="b9472-125">Ensure you set the `X-ZUMO-AUTH` header to your authentication token.</span></span>  <span data-ttu-id="b9472-126">Ověřovací token je uložen v `client.currentUser.mobileServiceAuthenticationToken`.</span><span class="sxs-lookup"><span data-stu-id="b9472-126">The authentication token is stored in `client.currentUser.mobileServiceAuthenticationToken`.</span></span>  <span data-ttu-id="b9472-127">Například pokud chcete použít rozhraní Fetch API:</span><span class="sxs-lookup"><span data-stu-id="b9472-127">For example, to use the fetch API:</span></span>

```
var url = client.applicationUrl + '/.auth/me';
var headers = new Headers();
headers.append('X-ZUMO-AUTH', client.currentUser.mobileServiceAuthenticationToken);
fetch(url, { headers: headers })
    .then(function (data) {
        return data.json()
    }).then(function (user) {
        // The user object contains the claims for the authenticated user
    });
```

<span data-ttu-id="b9472-128">Fetch je k dispozici jako [balíček npm](https://www.npmjs.com/package/whatwg-fetch) nebo ke stažení přes prohlížeč na webu [CDNJS](https://cdnjs.com/libraries/fetch).</span><span class="sxs-lookup"><span data-stu-id="b9472-128">Fetch is available as [an npm package](https://www.npmjs.com/package/whatwg-fetch) or for browser download from [CDNJS](https://cdnjs.com/libraries/fetch).</span></span> <span data-ttu-id="b9472-129">K načtení informací můžete použít také jQuery nebo jiné rozhraní AJAX API.</span><span class="sxs-lookup"><span data-stu-id="b9472-129">You could also use jQuery or another AJAX API to fetch the information.</span></span>  <span data-ttu-id="b9472-130">Přijatá data jsou ve formátu objektu JSON.</span><span class="sxs-lookup"><span data-stu-id="b9472-130">Data is received as a JSON object.</span></span>
