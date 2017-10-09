### <a name="server-auth"></a>Postup: Ověřování pomocí zprostředkovatele (tok na straně serveru)
toohave Mobile Apps spravovat hello proces ověřování v aplikaci, je nutné zaregistrovat vaší aplikace pomocí zprostředkovatele identity. V Azure App Service, budete potřebovat ID aplikace hello tooconfigure a tajný klíč poskytované poskytovatelem.
Další informace najdete v tématu hello kurzu [přidat ověřování tooyour aplikace](../articles/app-service-mobile/app-service-mobile-cordova-get-started-users.md).

Po registraci zprostředkovatele identity volání hello `.login()` metoda s hello název poskytovatele. Například toologin službou Facebook použít hello následující kód:

```
client.login("facebook").done(function (results) {
     alert("You are now logged in as: " + results.userId);
}, function (err) {
     alert("Error: " + err);
});
```

Hello platné hodnoty pro zprostředkovatele hello jsou 'aad', 'facebook., 'google', 'microsoftaccount' a "twitter".

> [!NOTE]
> Ověřování Google přes tok na straně serveru aktuálně nefunguje.  tooauthenticate službou Google, je nutné použít [tok klienta metoda](#client-auth).

Azure App Service v tomto případě spravuje tok ověřování hello OAuth 2.0.  Zobrazí přihlašovací stránku hello hello vybraného zprostředkovatele a vygeneruje ověřovací token služby App Service za úspěšného přihlášení pomocí zprostředkovatele identity hello. Funkce Hello přihlášení, po dokončení vrátí objekt JSON, který zveřejňuje hello ID uživatele a služby App Service ověřovací token v hello pole ID uživatele a authenticationToken, v uvedeném pořadí. Tento token se může uložit do mezipaměti a znovu požívat do vypršení platnosti.

###<a name="client-auth"></a>Postup: Ověřování pomocí zprostředkovatele (tok na straně klienta)

Aplikace můžete také nezávisle obraťte se na zprostředkovatele identity hello a pak zadejte hello vrátil tokenu tooyour služby App Service pro ověřování. Tento tok klienta umožňuje tooprovide jednom přihlášení pro uživatele nebo tooretrieve další uživatelské dat od poskytovatele identity hello.

#### <a name="social-authentication-basic-example"></a>Základní příklad sociálního ověřování

Tento příklad používá k ověřování klientskou sadu SDK Facebooku:

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
Tento příklad předpokládá token tuto hello zadaný hello příslušného poskytovatele sady SDK je uložen v tokenu proměnná hello.

#### <a name="microsoft-account-example"></a>Příklad s účtem Microsoft

Následující příklad používá Hello hello Live SDK, která podporuje – jednotné přihlášení pro aplikace Windows Store pomocí Account Microsoft:

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

Tento příklad získá token z Live připojení, který je zadaný tooyour služby App Service při volání funkce hello přihlášení.

###<a name="auth-getinfo"></a>Postupy: získání informací o hello ověřeného uživatele

informace o ověřování Hello lze načíst z hello `/.auth/me` volání koncového bodu pomocí protokolu HTTP s žádnou knihovnu AJAX.  Ujistěte se, nastavíte hello `X-ZUMO-AUTH` záhlaví tooyour ověřovací token.  Hello ověřovací token je uložen v `client.currentUser.mobileServiceAuthenticationToken`.  Například toouse hello načtení rozhraní API:

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

Fetch je k dispozici jako [balíček npm](https://www.npmjs.com/package/whatwg-fetch) nebo ke stažení přes prohlížeč na webu [CDNJS](https://cdnjs.com/libraries/fetch). Můžete také použít jQuery nebo jiné informace hello toofetch AJAX API.  Přijatá data jsou ve formátu objektu JSON.
