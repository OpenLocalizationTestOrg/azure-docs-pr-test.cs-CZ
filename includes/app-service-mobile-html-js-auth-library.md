### <span data-ttu-id="a93eb-101"><a name="server-auth"></a>Postup: Ověřování pomocí zprostředkovatele (tok na straně serveru)</span><span class="sxs-lookup"><span data-stu-id="a93eb-101"><a name="server-auth"></a>How to: Authenticate with a provider (Server Flow)</span></span>
<span data-ttu-id="a93eb-102">toohave Mobile Apps spravovat hello proces ověřování v aplikaci, je nutné zaregistrovat vaší aplikace pomocí zprostředkovatele identity.</span><span class="sxs-lookup"><span data-stu-id="a93eb-102">toohave Mobile Apps manage hello authentication process in your app, you must register your app with your identity provider.</span></span> <span data-ttu-id="a93eb-103">V Azure App Service, budete potřebovat ID aplikace hello tooconfigure a tajný klíč poskytované poskytovatelem.</span><span class="sxs-lookup"><span data-stu-id="a93eb-103">Then in your Azure App Service, you need tooconfigure hello application ID and secret provided by your provider.</span></span>
<span data-ttu-id="a93eb-104">Další informace najdete v tématu hello kurzu [přidat ověřování tooyour aplikace](../articles/app-service-mobile/app-service-mobile-cordova-get-started-users.md).</span><span class="sxs-lookup"><span data-stu-id="a93eb-104">For more information, see hello tutorial [Add authentication tooyour app](../articles/app-service-mobile/app-service-mobile-cordova-get-started-users.md).</span></span>

<span data-ttu-id="a93eb-105">Po registraci zprostředkovatele identity volání hello `.login()` metoda s hello název poskytovatele.</span><span class="sxs-lookup"><span data-stu-id="a93eb-105">Once you have registered your identity provider, call hello `.login()` method with hello name of your provider.</span></span> <span data-ttu-id="a93eb-106">Například toologin službou Facebook použít hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="a93eb-106">For example, toologin with Facebook use hello following code:</span></span>

```
client.login("facebook").done(function (results) {
     alert("You are now logged in as: " + results.userId);
}, function (err) {
     alert("Error: " + err);
});
```

<span data-ttu-id="a93eb-107">Hello platné hodnoty pro zprostředkovatele hello jsou 'aad', 'facebook., 'google', 'microsoftaccount' a "twitter".</span><span class="sxs-lookup"><span data-stu-id="a93eb-107">hello valid values for hello provider are 'aad', 'facebook', 'google', 'microsoftaccount', and 'twitter'.</span></span>

> [!NOTE]
> <span data-ttu-id="a93eb-108">Ověřování Google přes tok na straně serveru aktuálně nefunguje.</span><span class="sxs-lookup"><span data-stu-id="a93eb-108">Google Authentication does not currently work via Server Flow.</span></span>  <span data-ttu-id="a93eb-109">tooauthenticate službou Google, je nutné použít [tok klienta metoda](#client-auth).</span><span class="sxs-lookup"><span data-stu-id="a93eb-109">tooauthenticate with Google, you must use a [client-flow method](#client-auth).</span></span>

<span data-ttu-id="a93eb-110">Azure App Service v tomto případě spravuje tok ověřování hello OAuth 2.0.</span><span class="sxs-lookup"><span data-stu-id="a93eb-110">In this case, Azure App Service manages hello OAuth 2.0 authentication flow.</span></span>  <span data-ttu-id="a93eb-111">Zobrazí přihlašovací stránku hello hello vybraného zprostředkovatele a vygeneruje ověřovací token služby App Service za úspěšného přihlášení pomocí zprostředkovatele identity hello.</span><span class="sxs-lookup"><span data-stu-id="a93eb-111">It displays hello login page of hello selected provider and generates an App Service authentication token after successful login with hello identity provider.</span></span> <span data-ttu-id="a93eb-112">Funkce Hello přihlášení, po dokončení vrátí objekt JSON, který zveřejňuje hello ID uživatele a služby App Service ověřovací token v hello pole ID uživatele a authenticationToken, v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="a93eb-112">hello login function, when complete, returns a JSON object that exposes both hello user ID and App Service authentication token in hello userId and authenticationToken fields, respectively.</span></span> <span data-ttu-id="a93eb-113">Tento token se může uložit do mezipaměti a znovu požívat do vypršení platnosti.</span><span class="sxs-lookup"><span data-stu-id="a93eb-113">This token can be cached and reused until it expires.</span></span>

###<span data-ttu-id="a93eb-114"><a name="client-auth"></a>Postup: Ověřování pomocí zprostředkovatele (tok na straně klienta)</span><span class="sxs-lookup"><span data-stu-id="a93eb-114"><a name="client-auth"></a>How to: Authenticate with a provider (Client Flow)</span></span>

<span data-ttu-id="a93eb-115">Aplikace můžete také nezávisle obraťte se na zprostředkovatele identity hello a pak zadejte hello vrátil tokenu tooyour služby App Service pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="a93eb-115">Your app can also independently contact hello identity provider and then provide hello returned token tooyour App Service for authentication.</span></span> <span data-ttu-id="a93eb-116">Tento tok klienta umožňuje tooprovide jednom přihlášení pro uživatele nebo tooretrieve další uživatelské dat od poskytovatele identity hello.</span><span class="sxs-lookup"><span data-stu-id="a93eb-116">This client flow enables you tooprovide a single sign-in experience for users or tooretrieve additional user data from hello identity provider.</span></span>

#### <a name="social-authentication-basic-example"></a><span data-ttu-id="a93eb-117">Základní příklad sociálního ověřování</span><span class="sxs-lookup"><span data-stu-id="a93eb-117">Social Authentication basic example</span></span>

<span data-ttu-id="a93eb-118">Tento příklad používá k ověřování klientskou sadu SDK Facebooku:</span><span class="sxs-lookup"><span data-stu-id="a93eb-118">This example uses Facebook client SDK for authentication:</span></span>

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
<span data-ttu-id="a93eb-119">Tento příklad předpokládá token tuto hello zadaný hello příslušného poskytovatele sady SDK je uložen v tokenu proměnná hello.</span><span class="sxs-lookup"><span data-stu-id="a93eb-119">This example assumes that hello token provided by hello respective provider SDK is stored in hello token variable.</span></span>

#### <a name="microsoft-account-example"></a><span data-ttu-id="a93eb-120">Příklad s účtem Microsoft</span><span class="sxs-lookup"><span data-stu-id="a93eb-120">Microsoft Account example</span></span>

<span data-ttu-id="a93eb-121">Následující příklad používá Hello hello Live SDK, která podporuje – jednotné přihlášení pro aplikace Windows Store pomocí Account Microsoft:</span><span class="sxs-lookup"><span data-stu-id="a93eb-121">hello following example uses hello Live SDK, which supports single-sign-on for Windows Store apps by using Microsoft Account:</span></span>

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

<span data-ttu-id="a93eb-122">Tento příklad získá token z Live připojení, který je zadaný tooyour služby App Service při volání funkce hello přihlášení.</span><span class="sxs-lookup"><span data-stu-id="a93eb-122">This example gets a token from Live Connect, which is supplied tooyour App Service by calling hello login function.</span></span>

###<span data-ttu-id="a93eb-123"><a name="auth-getinfo"></a>Postupy: získání informací o hello ověřeného uživatele</span><span class="sxs-lookup"><span data-stu-id="a93eb-123"><a name="auth-getinfo"></a>How to: Obtain information about hello authenticated user</span></span>

<span data-ttu-id="a93eb-124">informace o ověřování Hello lze načíst z hello `/.auth/me` volání koncového bodu pomocí protokolu HTTP s žádnou knihovnu AJAX.</span><span class="sxs-lookup"><span data-stu-id="a93eb-124">hello authentication information can be retrieved from hello `/.auth/me` endpoint using an HTTP call with any AJAX library.</span></span>  <span data-ttu-id="a93eb-125">Ujistěte se, nastavíte hello `X-ZUMO-AUTH` záhlaví tooyour ověřovací token.</span><span class="sxs-lookup"><span data-stu-id="a93eb-125">Ensure you set hello `X-ZUMO-AUTH` header tooyour authentication token.</span></span>  <span data-ttu-id="a93eb-126">Hello ověřovací token je uložen v `client.currentUser.mobileServiceAuthenticationToken`.</span><span class="sxs-lookup"><span data-stu-id="a93eb-126">hello authentication token is stored in `client.currentUser.mobileServiceAuthenticationToken`.</span></span>  <span data-ttu-id="a93eb-127">Například toouse hello načtení rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="a93eb-127">For example, toouse hello fetch API:</span></span>

```
var url = client.applicationUrl + '/.auth/me';
var headers = new Headers();
headers.append('X-ZUMO-AUTH', client.currentUser.mobileServiceAuthenticationToken);
fetch(url, { headers: headers })
    .then(function (data) {
        return data.json()
    }).then(function (user) {
        // hello user object contains hello claims for hello authenticated user
    });
```

<span data-ttu-id="a93eb-128">Fetch je k dispozici jako [balíček npm](https://www.npmjs.com/package/whatwg-fetch) nebo ke stažení přes prohlížeč na webu [CDNJS](https://cdnjs.com/libraries/fetch).</span><span class="sxs-lookup"><span data-stu-id="a93eb-128">Fetch is available as [an npm package](https://www.npmjs.com/package/whatwg-fetch) or for browser download from [CDNJS](https://cdnjs.com/libraries/fetch).</span></span> <span data-ttu-id="a93eb-129">Můžete také použít jQuery nebo jiné informace hello toofetch AJAX API.</span><span class="sxs-lookup"><span data-stu-id="a93eb-129">You could also use jQuery or another AJAX API toofetch hello information.</span></span>  <span data-ttu-id="a93eb-130">Přijatá data jsou ve formátu objektu JSON.</span><span class="sxs-lookup"><span data-stu-id="a93eb-130">Data is received as a JSON object.</span></span>
