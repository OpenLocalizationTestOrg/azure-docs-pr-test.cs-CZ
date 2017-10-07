---
title: "Azure Active Directory B2C: Použití hello rozhraní Graph API | Microsoft Docs"
description: "Jak toocall hello rozhraní Graph API pro klienta B2C pomocí procesu hello tooautomate identity aplikace."
services: active-directory-b2c
documentationcenter: .net
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: f9904516-d9f7-43b1-ae4f-e4d9eb1c67a0
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/07/2017
ms.author: parakhj
ms.openlocfilehash: a17cdc4adf57dbf22592d99ef8ecde41652763fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-b2c-use-hello-graph-api"></a>Azure AD B2C: Použijte hello rozhraní Graph API
Azure Active Directory (Azure AD) B2C klienty zpravidla toobe velké. To znamená, že mnoho běžné úlohy správy klienta nutné toobe provést prostřednictvím kódu programu. Primární příkladem je Správa uživatelů. Může být nutné toomigrate existujícího klienta B2C úložiště tooa uživatele. Mohou má toohost registrace uživatele na vlastní stránce a vytvářet uživatelské účty ve vašem adresáři Azure AD B2C pozadí hello. Tyto typy úloh vyžadují hello možnost toocreate, číst, aktualizovat a odstraňovat uživatelské účty. Můžete provést tyto úlohy pomocí hello Azure AD Graph API.

U klientů B2C existují dva primární režimy komunikaci s hello rozhraní Graph API.

* Pro interaktivní, jedno spuštění úlohy by měla fungovat jako účet správce v klienta B2C hello při provádění úloh, hello. Tento režim vyžaduje toosign správce se pomocí přihlašovacích údajů, před kterou správce může provádět všechny toohello volání rozhraní Graph API.
* Pro automatizované, nepřetržité úlohy měli byste použít nějaký typ účtu služby, který zadáte pomocí úloh správy tooperform hello potřebná oprávnění. Ve službě Azure AD to provedete tak, že registrace aplikace a ověřování tooAzure AD. To se provádí pomocí **ID aplikace** používající hello [udělení pověření klienta OAuth 2.0](../active-directory/develop/active-directory-authentication-scenarios.md#daemon-or-server-application-to-web-api). V takovém případě hello aplikace funguje jako samostatně, nikoli jako uživatel, toocall hello rozhraní Graph API.

V tomto článku probereme, jak tooperform hello případ automatizované použití. toodemonstrate, jsme budete sestavení rozhraní .NET 4.5 `B2CGraphClient` který provede uživatele vytvořit, číst, aktualizovat a odstranit operace. Hello klienta bude mít rozhraní Windows příkazového řádku (CLI), který vám umožní tooinvoke různé metody. Kód hello je však zapsána toobehave neinteraktivní, automatizované způsobem.

## <a name="get-an-azure-ad-b2c-tenant"></a>Získání klienta Azure AD B2C
Předtím, než můžete vytvořit aplikace nebo uživatele, nebo vůbec komunikovat s Azure AD, budete potřebovat klienta Azure AD B2C a účet globálního správce v klientovi hello. Pokud nemáte klienta již, [Začínáme s Azure AD B2C](active-directory-b2c-get-started.md).

## <a name="register-your-application-in-your-tenant"></a>Registrace vaší aplikace ve vašem klientovi
Až budete mít klienta B2C, kterou potřebujete tooregister aplikace prostřednictvím hello [portálu Azure](https://portal.azure.com).

> [!IMPORTANT]
> toouse hello rozhraní Graph API s svého klienta B2C, budete potřebovat tooregister vyhrazené aplikace pomocí hello obecného *registrace aplikace* okno v hello portálu Azure, **není** Azure AD B2C  *Aplikace* okno. Nelze znovu použít už existující B2C aplikace hello, zaregistrované v Azure AD B2C hello *aplikace* okno.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Zvolte klienta služby Azure AD B2C výběrem účtu v hello pravém horním rohu stránky hello.
3. V levém navigačním podokně hello zvolte **více služeb**, klikněte na tlačítko **registrace aplikace**a klikněte na tlačítko **přidat**.
4. Postupujte podle pokynů hello a vytvořte novou aplikaci. 
    1. Vyberte **webovou aplikaci nebo API** jako typ aplikace hello.    
    2. Zadejte **žádný identifikátor URI přesměrování** (např. https://B2CGraphAPI) jako není relevantní pro tento příklad.  
5. Hello aplikace se teď zobrazí v hello seznam aplikací, klikněte na jeho tooobtain hello **ID aplikace** (také označované jako ID klienta). Zkopírujte jej, protože ho budete potřebovat v další části.
6. V okně Nastavení hello, klikněte na **klíče** a přidejte nový klíč (také označovaný jako sdílený tajný klíč klienta). Také zkopírujte jej pro použití v další části.

## <a name="configure-create-read-and-update-permissions-for-your-application"></a>Konfigurace vytvářet, číst a aktualizovat oprávnění pro vaši aplikaci
Nyní je třeba tooconfigure tooget vaší aplikace, které všechny hello požadované oprávnění toocreate, číst, aktualizovat a odstranit uživatele.

1. Pokračování v hello okno registrace aplikace portálu Azure, vyberte svou aplikaci.
2. V okně Nastavení hello, klikněte na **požadovaná oprávnění**.
3. V okně hello potřebná oprávnění, klikněte na **Windows Azure Active Directory**.
4. V okně hello povolit přístup, vyberte hello **pro čtení a zápis dat adresáře** oprávnění z **oprávnění aplikací** a klikněte na tlačítko **Uložit**.
5. Nakonec zpět v okně hello potřebná oprávnění, klikněte na hello **udělit oprávnění** tlačítko.

Teď máte aplikaci, která má oprávnění toocreate, čtení a aktualizaci uživatelé z vašeho klienta B2C.

## <a name="configure-delete-permissions-for-your-application"></a>Konfigurace odstranění oprávnění pro vaši aplikaci
V současné době hello *pro čtení a zápis dat adresáře* nemá oprávnění **není** zahrnují možnost toodo hello žádné odstranění například odstraňování uživatelů. Pokud chcete uživatelům aplikace hello možnost toodelete toogive, budete potřebovat toodo tyto další kroky, které se týkají prostředí PowerShell, jinak, můžete přeskočit toohello další části.

Nejprve stáhnout a nainstalovat hello [Microsoft Online Services Sign-In Assistant](http://go.microsoft.com/fwlink/?LinkID=286152). Potom stáhněte a nainstalujte hello [64-bit modulu Azure Active Directory pro prostředí Windows PowerShell](http://go.microsoft.com/fwlink/p/?linkid=236297).

Po instalaci modulu PowerShell text hello, otevřete prostředí PowerShell a připojte tooyour klienta B2C. Po spuštění `Get-Credential`, zobrazí se výzva pro uživatelské jméno a heslo, zadejte hello uživatelské jméno a heslo účtu správce klienta B2C.

> [!IMPORTANT]
> Je třeba toouse B2C účet správce klienta, který je **místní** toohello B2C klienta. Tyto účty vypadat takto: myusername@myb2ctenant.onmicrosoft.com.

```powershell
Connect-MsolService
```

Nyní použijeme hello **ID aplikace** ve skriptu hello níže tooassign hello aplikace hello uživatelské účet správce role, která umožní uživatelům toodelete. Tyto role mají dobře známé identifikátory, takže potřebujete toodo není vstupní vaše **ID aplikace** ve skriptu hello níže.

```powershell
$applicationId = "<YOUR_APPLICATION_ID>"
$sp = Get-MsolServicePrincipal -AppPrincipalId $applicationId
Add-MsolRoleMember -RoleObjectId fe930be7-5e62-47db-91af-98c3a49a38b1 -RoleMemberObjectId $sp.ObjectId -RoleMemberType servicePrincipal
```

Aplikaci teď má také oprávnění toodelete uživatelů z vašeho klienta B2C.

## <a name="download-configure-and-build-hello-sample-code"></a>Stažení, konfigurace a sestavení hello ukázkový kód
Nejprve stáhnout ukázkový kód hello a získat běh aplikace. Potom jsme bude trvat bližší pohled na ho.  Můžete [stáhnout hello ukázkový kód v souboru ZIP](https://github.com/AzureADQuickStarts/B2C-GraphAPI-DotNet/archive/master.zip). Můžete ho také klonovat do adresáře podle vašeho výběru:

```
git clone https://github.com/AzureADQuickStarts/B2C-GraphAPI-DotNet.git
```

Otevřete hello `B2CGraphClient\B2CGraphClient.sln` řešení sady Visual Studio v sadě Visual Studio. V hello `B2CGraphClient` projekt, otevřete hello soubor `App.config`. Tři nastavení aplikace hello nahradíte vlastními hodnotami:

```
<appSettings>
    <add key="b2c:Tenant" value="{Your Tenant Name}" />
    <add key="b2c:ClientId" value="{hello ApplicationID from above}" />
    <add key="b2c:ClientSecret" value="{hello Key from above}" />
</appSettings>
```

[!INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

Pak klikněte pravým tlačítkem na hello `B2CGraphClient` řešení a znovu sestavit ukázku, hello. Pokud jste úspěšné, teď byste měli mít `B2C.exe` spustitelný soubor umístěný ve `B2CGraphClient\bin\Debug`.

## <a name="build-user-crud-operations-by-using-hello-graph-api"></a>Vytvoření operace CRUD uživatele pomocí rozhraní Graph API hello
Otevřete toouse hello B2CGraphClient, `cmd` Windows příkazového řádku a změňte váš adresář toohello `Debug` adresáře. Spusťte hello `B2C Help` příkaz.

```
> cd B2CGraphClient\bin\Debug
> B2C Help
```

Tato akce zobrazí stručný popis každého příkazu. Pokaždé, když vyvolat jeden z těchto příkazů `B2CGraphClient` provede požadavek toohello Azure AD Graph API.

### <a name="get-an-access-token"></a>Získání přístupového tokenu
Všechny žádosti o toohello rozhraní Graph API vyžaduje token přístupu pro ověřování. `B2CGraphClient`používá hello open-source Active Directory Authentication Library (ADAL) toohelp získat přístupové tokeny. ADAL usnadňuje získávání tokenu poskytuje jednoduché rozhraní API a postará o některé důležité podrobnosti, jako je například ukládání do mezipaměti přístupových tokenů. Toouse ADAL tooget tokeny, ale nemáte. Můžete také získat tokeny tím, že vytvoří požadavky HTTP.

> [!NOTE]
> Tento příklad používá ADAL v2 v pořadí toocommunicate s hello rozhraní Graph API.  ADAL verze 2 nebo 3 je nutné použít v pořadí tooget přístupové tokeny, které se dají použít s hello Azure AD Graph API.
> 
> 

Když `B2CGraphClient` spustí, vytvoří instanci hello `B2CGraphClient` třídy. Nastaví generování uživatelského rozhraní ověřování pomocí knihovny ADAL Hello konstruktoru pro tuto třídu:

```C#
public B2CGraphClient(string clientId, string clientSecret, string tenant)
{
    // hello client_id, client_secret, and tenant are provided in Program.cs, which pulls hello values from App.config
    this.clientId = clientId;
    this.clientSecret = clientSecret;
    this.tenant = tenant;

    // hello AuthenticationContext is ADAL's primary class, in which you indicate hello tenant toouse.
    this.authContext = new AuthenticationContext("https://login.microsoftonline.com/" + tenant);

    // hello ClientCredential is where you pass in your client_id and client_secret, which are
    // provided tooAzure AD in order tooreceive an access_token by using hello app's identity.
    this.credential = new ClientCredential(clientId, clientSecret);
}
```

Použijeme hello `B2C Get-User` příkaz jako příklad. Když `B2C Get-User` je volána bez jakékoli další vstupy hello hello volání rozhraní příkazového řádku `B2CGraphClient.GetAllUsers(...)` metoda. Tato metoda volá `B2CGraphClient.SendGraphGetRequest(...)`, který odešle metody GET protokolu HTTP žádosti toohello rozhraní Graph API. Před `B2CGraphClient.SendGraphGetRequest(...)` zasílá hello požadavek GET, nejdřív získá přístup tokenu pomocí ADAL:

```C#
public async Task<string> SendGraphGetRequest(string api, string query)
{
    // First, use ADAL tooacquire a token by using hello app's identity (hello credential)
    // hello first parameter is hello resource we want an access_token for; in this case, hello Graph API.
    AuthenticationResult result = authContext.AcquireToken("https://graph.windows.net", credential);

    ...

```

Můžete získat přístup tokenu pro rozhraní Graph API hello podle hello volání ADAL `AuthenticationContext.AcquireToken(...)` metoda. Vrátí ADAL `access_token` představující identitu aplikace hello.

### <a name="read-users"></a>Uživatelé pro čtení
Pokud chcete tooget seznam uživatelů, nebo určitého uživatele sám hello rozhraní Graph API, můžete odeslat HTTP `GET` požadavku toohello `/users` koncový bod. Žádost o pro všechny uživatele hello v klienta vypadá takto:

```
GET https://graph.windows.net/contosob2c.onmicrosoft.com/users?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
```

toosee tuto žádost, spusťte:

 ```
 > B2C Get-User
 ```

Existují dvě toonote důležité věci:

* Hello získaných prostřednictvím ADAL přístupový token se přidá toohello `Authorization` záhlaví s použitím hello `Bearer` schéma.
* Pro B2C klientů, je nutné použít parametr dotazu hello `api-version=1.6`.

Obě tyto podrobnosti jsou zpracovávány v hello `B2CGraphClient.SendGraphGetRequest(...)` metoda:

```C#
public async Task<string> SendGraphGetRequest(string api, string query)
{
    ...

    // For B2C user management, be sure toouse hello 1.6 Graph API version.
    HttpClient http = new HttpClient();
    string url = "https://graph.windows.net/" + tenant + api + "?" + "api-version=1.6";
    if (!string.IsNullOrEmpty(query))
    {
        url += "&" + query;
    }

    // Append hello access token for hello Graph API toohello Authorization header of hello request by using hello Bearer scheme.
    HttpRequestMessage request = new HttpRequestMessage(HttpMethod.Get, url);
    request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);
    HttpResponseMessage response = await http.SendAsync(request);

    ...
```

### <a name="create-consumer-user-accounts"></a>Vytvoření příjemce uživatelské účty
Při vytváření uživatelských účtů v svého klienta B2C, můžete odeslat HTTP `POST` požadavku toohello `/users` koncový bod:

```
POST https://graph.windows.net/contosob2c.onmicrosoft.com/users?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
Content-Type: application/json
Content-Length: 338

{
    // All of these properties are required toocreate consumer users.

    "accountEnabled": true,
    "signInNames": [                            // controls which identifier hello user uses toosign in toohello account
        {
            "type": "emailAddress",             // can be 'emailAddress' or 'userName'
            "value": "joeconsumer@gmail.com"
        }
    ],
    "creationType": "LocalAccount",            // always set too'LocalAccount'
    "displayName": "Joe Consumer",                // a value that can be used for displaying toohello end user
    "mailNickname": "joec",                        // an email alias for hello user
    "passwordProfile": {
        "password": "P@ssword!",
        "forceChangePasswordNextLogin": false   // always set toofalse
    },
    "passwordPolicies": "DisablePasswordExpiration"
}
```

Většina těchto vlastností v této žádosti o jsou požadované toocreate spotřebitelské uživatele. Další, klikněte na tlačítko toolearn [zde](https://msdn.microsoft.com/library/azure/ad/graph/api/users-operations#CreateLocalAccountUser). Všimněte si, že hello `//` komentáře byly zahrnuty pro obrázek. V skutečné požadavku je nezahrnujte.

toosee hello požadavku, spusťte jeden z hello následující příkazy:

```
> B2C Create-User ..\..\..\usertemplate-email.json
> B2C Create-User ..\..\..\usertemplate-username.json
```

Hello `Create-User` příkaz má soubor .json jako vstupní parametr. Tato položka obsahuje reprezentaci JSON objektu uživatele. Existují dva ukázkové soubory .json v ukázkovém kódu hello: `usertemplate-email.json` a `usertemplate-username.json`. Můžete upravit tyto soubory toosuit vašim potřebám. Kromě toho toohello požadovaná pole výše, několik volitelná pole, které můžete použít jsou zahrnuty v těchto souborech. Informace o volitelných polí hello lze nalézt v hello [odkaz na Azure AD Graph API entity](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity).

Můžete vidět, jak je požadavek POST hello vytvořený v `B2CGraphClient.SendGraphPostRequest(...)`.

* Připojí toohello tokenu přístupu `Authorization` hlavičky požadavku hello.
* Nastaví `api-version=1.6`.
* Obsahuje objekt uživatele hello JSON v textu hello hello požadavku.

> [!NOTE]
> Pokud hello účty, které chcete toomigrate z existující úložiště uživatele má nižší síly hesla než hello [silné heslo sílu vynucováno Azure AD B2C](https://msdn.microsoft.com/library/azure/jj943764.aspx), můžete zakázat pomocí hello požadaveknasilnéheslohello`DisableStrongPassword`hodnota v hello `passwordPolicies` vlastnost. Například můžete upravit hello vytvořit požadavek uživatele výše uvedeného následujícím způsobem: `"passwordPolicies": "DisablePasswordExpiration, DisableStrongPassword"`.
> 
> 

### <a name="update-consumer-user-accounts"></a>Aktualizovat příjemce uživatelské účty
Při aktualizaci uživatelských objektů hello proces je podobný toohello jeden že použijete toocreate uživatelské objekty. Tento proces používá hello HTTP, ale `PATCH` metoda:

```
PATCH https://graph.windows.net/contosob2c.onmicrosoft.com/users/<user-object-id>?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
Content-Type: application/json
Content-Length: 37

{
    "displayName": "Joe Consumer",                // this request updates only hello user's displayName
}
```

Zkuste tooupdate uživatele tím, že aktualizuje vaše soubory JSON se nová data. Pak můžete použít `B2CGraphClient` toorun jednu z těchto příkazů:

```
> B2C Update-User <user-object-id> ..\..\..\usertemplate-email.json
> B2C Update-User <user-object-id> ..\..\..\usertemplate-username.json
```

Zkontrolujte hello `B2CGraphClient.SendGraphPatchRequest(...)` metoda podrobnosti o tom toosend tuto žádost.

### <a name="search-users"></a>Vyhledávat uživatele
Můžete hledat uživatele v svého klienta B2C v několika způsoby. Jeden pomocí ID objektu nebo dva pomocí hello uživatele přihlašovací identifikátor uživatele hello (tj, hello `signInNames` vlastnost).

Spusťte jeden z následujících příkazů toosearch pro konkrétního uživatele hello:

```
> B2C Get-User <user-object-id>
> B2C Get-User <filter-query-expression>
```

Tady je několik příkladů:

```
> B2C Get-User 2bcf1067-90b6-4253-9991-7f16449c2d91
> B2C Get-User $filter=signInNames/any(x:x/value%20eq%20%27joeconsumer@gmail.com%27)
```

### <a name="delete-users"></a>Odstranit uživatele
Hello proces odstranění uživatele je jednoduchý. Použití hello HTTP `DELETE` metoda konstrukce hello adresy URL a s hello Opravte ID objektu:

```
DELETE https://graph.windows.net/contosob2c.onmicrosoft.com/users/<user-object-id>?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
```

toosee příklad, zadejte tento příkaz a zobrazení hello odstranit požadavek, který je konzoly na tištěných toohello:

```
> B2C Delete-User <object-id-of-user>
```

Zkontrolujte hello `B2CGraphClient.SendGraphDeleteRequest(...)` metoda podrobnosti o tom toosend tuto žádost.

Můžete provádět mnoho dalších akcí s hello Azure AD Graph API v přidání toouser správy. [Referenční dokumentace rozhraní API Azure AD Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) poskytuje podrobnosti pro každou akci, společně s požadavky na ukázky.

## <a name="use-custom-attributes"></a>Použití vlastních atributů
Většina aplikací příjemce potřebovat toostore nějaký typ informace vlastního uživatelského profilu. Jedním ze způsobů, můžete k tomu je toodefine vlastní atribut v svého klienta B2C. Poté můžete ke tento atribut hello stejným způsobem jako považovat všechny ostatní vlastnosti v objektu user. Je možné aktualizovat atribut hello odstranění hello atribut dotazu hello atribut odesílat hello atribut jako deklaraci v tokeny přihlášení a další.

toodefine vlastní atribut v svého klienta B2C, najdete v části hello [odkaz vlastní atribut B2C](active-directory-b2c-reference-custom-attr.md).

Můžete zobrazit hello vlastní atributy definované ve vašem klientovi B2C pomocí `B2CGraphClient`:

```
> B2C Get-B2C-Application
> B2C Get-Extension-Attribute <object-id-in-the-output-of-the-above-command>
```

výstup Hello tyto funkce zjistí hello podrobnosti o jednotlivých vlastních atributů, jako například:

```JSON
{
      "odata.type": "Microsoft.DirectoryServices.ExtensionProperty",
      "objectType": "ExtensionProperty",
      "objectId": "cec6391b-204d-42fe-8f7c-89c2b1964fca",
      "deletionTimestamp": null,
      "appDisplayName": "",
      "name": "extension_55dc0861f9a44eb999e0a8a872204adb_Jersey_Number",
      "dataType": "Integer",
      "isSyncedFromOnPremises": false,
      "targetObjects": [
        "User"
      ]
}
```

Můžete použít hello celý název, například `extension_55dc0861f9a44eb999e0a8a872204adb_Jersey_Number`, jako vlastnost na uživatelské objekty.  Aktualizujte si soubor .json hello nové vlastnosti a hodnotu pro vlastnost hello a pak spusťte:

```
> B2C Update-User <object-id-of-user> <path-to-json-file>
```

Pomocí `B2CGraphClient`, máte aplikaci služby, která můžete spravovat vaše uživatele klienta B2C prostřednictvím kódu programu. `B2CGraphClient`používá vlastní aplikace identity tooauthenticate toohello Azure AD Graph API. Je také získá tokeny pomocí tajný klíč klienta. Protože tato funkce se začlenit do vaší aplikace, mějte na paměti několik klíčových bodů pro B2C aplikace:

* Je nutné toogrant hello aplikace hello příslušná oprávnění v klientovi hello.
* Nyní musíte toouse ADAL (ne MSAL) tooget přístupových tokenů. (Můžete také odeslat zprávy protokolu přímo, bez použití knihovny.)
* Když zavoláte hello rozhraní Graph API, použijte `api-version=1.6`.
* Při vytváření a aktualizovat spotřebitelské uživatele, jsou povinné, několik vlastností, jak je popsáno výše.

Pokud máte jakékoli dotazy nebo požadavků pro akce, které si přejete tooperform pomocí hello rozhraní Graph API na svého klienta B2C poznámky v tomto článku nebo v úložišti ukázkový kód GitHub hello soubor problém.

