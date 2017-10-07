---
title: aaaAzure AD B2C | Microsoft Docs
description: "Jak toobuild webového rozhraní API .NET pomocí Azure Active Directory B2C zabezpečené, pomocí přístupových tokenů OAuth 2.0 pro ověřování."
services: active-directory-b2c
documentationcenter: .net
author: parakhj
manager: krassk
editor: 
ms.assetid: 7146ed7f-2eb5-49e9-8d8b-ea1a895e1966
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 03/17/2017
ms.author: parakhj
ms.openlocfilehash: d45364216deda38ef44b60dd11e86d9a089ad509
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-build-a-net-web-api"></a>Azure Active Directory B2C: Sestavení webového rozhraní API .NET

S Azure Active Directory (Azure AD) B2C můžete zabezpečit webové rozhraní API pomocí přístupových tokenů OAuth 2.0. Tyto tokeny umožňují vašeho klienta toohello tooauthenticate aplikace API. Tento článek ukazuje, jak toocreate rozhraní API .NET MVC "seznam úkolů", umožňuje uživatelům vašeho klienta úlohy tooCRUD aplikace. Hello webového rozhraní API zabezpečené pomocí Azure AD B2C a pouze umožňuje ověřeným uživatelům toomanage jejich seznamu úkolů.

## <a name="create-an-azure-ad-b2c-directory"></a>Vytvoření adresáře Azure AD B2C

Před použitím Azure AD B2C musíte vytvořit adresář, nebo klienta. Adresář je kontejner pro všechny vaše uživatele, aplikace, skupiny a další. Pokud ho ještě nemáte, [vytvořte adresář B2C](active-directory-b2c-get-started.md) předtím, než budete pokračovat.

> [!NOTE]
> Hello klientská aplikace a webové rozhraní API musí používat adresář hello stejnou službou Azure AD B2C.
>

## <a name="create-a-web-api"></a>Vytvoření webového rozhraní API

Dále musíte ve svém adresáři B2C toocreate webové aplikace API. Díky tomu získá informace o Azure AD, se vyžaduje toosecurely komunikaci s vaší aplikací. toocreate na aplikace, postupujte podle [tyto pokyny](active-directory-b2c-app-registration.md). Ujistěte se, že:

* Zahrnout **webové aplikace** nebo **webové rozhraní API** v aplikaci hello.
* Použití hello **identifikátor URI pro přesměrování** `https://localhost:44332/` pro hello webovou aplikaci. Toto je výchozí umístění hello hello klienta webové aplikace pro tuto ukázku kódu.
* Kopírování hello **ID aplikace** který je přiřazený tooyour aplikace. Budete ho potřebovat později.
* Zadejte identifikátor aplikace do **Identifikátor URI ID aplikace**.
* Přidání oprávnění prostřednictvím hello **publikovaná obory** nabídky.

  [!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Vytvořte svoje zásady

V Azure AD B2C je každé uživatelské rozhraní definováno [zásadou](active-directory-b2c-reference-policies.md). Budete potřebovat toocreate toocommunicate zásady s Azure AD B2C. Doporučujeme, abyste pomocí hello kombinaci registrace-množství nebo zásad přihlašování, jak je popsáno v hello [článku o zásadách](active-directory-b2c-reference-policies.md). Při vytváření zásady nezapomeňte na následující:

* Zvolit **Zobrazovaný název** a další atributy registrace ve svojí zásadě.
* Zvolit **Zobrazovaný název** a deklarace identity **ID objektu** jako deklarace identity aplikace v každé zásadě. Můžete zvolit i další deklarace identity.
* Kopírování hello **název** po jejím vytvoření každé zásady. Název zásady hello budete potřebovat později.

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Po úspěšném vytvoření zásad hello jste připravené toobuild vaší aplikace.

## <a name="download-hello-code"></a>Stáhněte si kód hello

Hello kódu pro účely tohoto kurzu je udržovaný na [Githubu](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi). Ukázka hello může klonovat spuštěním:

```console
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi.git
```

Po stažení ukázkového kódu hello hello otevřete Visual Studio .sln souboru tooget spustit. Hello soubor řešení obsahuje dva projekty: `TaskWebApp` a `TaskService`. `TaskWebApp`je webová aplikace MVC, která hello uživatel komunikuje se službou. `TaskService`je back endové webové rozhraní API hello aplikace, které ukládá seznam úkolů každého uživatele. V tomto článku pouze zabývat hello `TaskService` aplikace. toolearn jak toobuild `TaskWebApp` pomocí Azure AD B2C, najdete v tématu [naše kurz .NET pro aplikace webových](active-directory-b2c-devquickstarts-web-dotnet-susi.md).

### <a name="update-hello-azure-ad-b2c-configuration"></a>Aktualizace konfigurace hello Azure AD B2C

Naše ukázka je ID zásad a klienta hello nakonfigurované toouse naše ukázka klienta. Pokud chcete toouse vlastního klienta, budete potřebovat toodo hello následující:

1. Otevřete `web.config` v hello `TaskService` projektu a nahraďte hodnoty hello
    * `ida:Tenant` názvem vašeho tenanta
    * `ida:ClientId` identifikátorem aplikace webového rozhraní API
    * `ida:SignUpSignInPolicyId` názvem zásady registrace/přihlášení

2. Otevřete `web.config` v hello `TaskWebApp` projektu a nahraďte hodnoty hello
    * `ida:Tenant` názvem vašeho tenanta
    * `ida:ClientId` identifikátorem webového aplikace
    * `ida:ClientSecret` tajným klíčem webové aplikace
    * `ida:SignUpSignInPolicyId` názvem zásady registrace/přihlášení
    * `ida:EditProfilePolicyId` názvem zásady pro úpravu profilu
    * `ida:ResetPasswordPolicyId` názvem zásady pro resetování hesla


## <a name="secure-hello-api"></a>Zabezpečení rozhraní API hello

Pokud máte klienta, který volá vaše rozhraní API, můžete rozhraní API (například `TaskService`) zabezpečit pomocí nosných tokenů OAuth 2.0. To zajistí, že každé rozhraní API tooyour požadavek bude pouze platný, pokud hello žádost obsahuje token nosiče. Vaše rozhraní API může přijímat a ověřovat nosné tokeny pomocí knihovny Microsoft's Open Web Interface pro .NET (OWIN).

### <a name="install-owin"></a>Instalace OWIN

Začněte instalací ověřovacího kanálu OWIN OAuth hello pomocí hello Konzola správce balíčků Visual Studio.

```Console
PM> Install-Package Microsoft.Owin.Security.OAuth -ProjectName TaskService
PM> Install-Package Microsoft.Owin.Security.Jwt -ProjectName TaskService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TaskService
```

Nainstaluje hello middleware OWIN, který bude přijmout a ověřit nosné tokeny.

### <a name="add-an-owin-startup-class"></a>Přidání třídy pro spuštění OWIN

Přidání třídy toohello spuštění OWIN rozhraní API volat `Startup.cs`.  Klikněte pravým tlačítkem na projekt hello, vyberte **přidat** a **nová položka**a poté vyhledejte OWIN. Hello OWIN middleware vyvolá hello `Configuration(…)` metoda při spuštění aplikace.

V našem ukázce jsme příliš změnili deklaraci třídy hello`public partial class Startup` a implementovat hello jiných součástí třídy hello v `App_Start\Startup.Auth.cs`. Uvnitř hello `Configuration` metoda, jsme přidali volání příliš`ConfigureAuth`, která je definována v `Startup.Auth.cs`. Po hello úpravy `Startup.cs` vypadá jako hello následující:

```CSharp
// Startup.cs

public partial class Startup
{
    // hello OWIN middleware will invoke this method when hello app starts
    public void Configuration(IAppBuilder app)
    {
        // ConfigureAuth defined in other part of hello class
        ConfigureAuth(app);
    }
}
```

### <a name="configure-oauth-20-authentication"></a>Konfigurace ověřování OAuth 2.0

Soubor otevřete hello `App_Start\Startup.Auth.cs`a implementovat hello `ConfigureAuth(...)` metoda. Například by měl vypadat jako následující hello:

```CSharp
// App_Start\Startup.Auth.cs

 public partial class Startup
    {
        // These values are pulled from web.config
        public static string AadInstance = ConfigurationManager.AppSettings["ida:AadInstance"];
        public static string Tenant = ConfigurationManager.AppSettings["ida:Tenant"];
        public static string ClientId = ConfigurationManager.AppSettings["ida:ClientId"];
        public static string SignUpSignInPolicy = ConfigurationManager.AppSettings["ida:SignUpSignInPolicyId"];
        public static string DefaultPolicy = SignUpSignInPolicy;

        /*
         * Configure hello authorization OWIN middleware.
         */
        public void ConfigureAuth(IAppBuilder app)
        {
            TokenValidationParameters tvps = new TokenValidationParameters
            {
                // Accept only those tokens where hello audience of hello token is equal toohello client ID of this app
                ValidAudience = ClientId,
                AuthenticationType = Startup.DefaultPolicy
            };

            app.UseOAuthBearerAuthentication(new OAuthBearerAuthenticationOptions
            {
                // This SecurityTokenProvider fetches hello Azure AD B2C metadata & signing keys from hello OpenIDConnect metadata endpoint
                AccessTokenFormat = new JwtFormat(tvps, new OpenIdConnectCachingSecurityTokenProvider(String.Format(AadInstance, Tenant, DefaultPolicy)))
            });
        }
    }
```

### <a name="secure-hello-task-controller"></a>Zabezpečení ovladače úkolů hello

Až aplikace hello ověřování nakonfigurované toouse OAuth 2.0, můžete webového rozhraní API zabezpečit přidáním `[Authorize]` ovladače úkolů toohello značky. Toto je hello řadiče, kde všechny manipulace se seznamem úkolů probíhá, takže byste měli zabezpečit celý ovladač hello na úrovni třídy hello. Můžete také přidat hello `[Authorize]` značky tooindividual akce pro podrobnější řízení přístupu.

```CSharp
// Controllers\TasksController.cs

[Authorize]
public class TasksController : ApiController
{
    ...
}
```

### <a name="get-user-information-from-hello-token"></a>Získání informací o uživateli z hello tokenu

`TasksController`ukládá úkoly do databáze, kde má každý úkol přidruženého uživatele, který "vlastní" hello úloh. Hello vlastník je identifikován pomocí hello uživatele **ID objektu**. (To je proto potřeba ID objektu hello tooadd jako aplikace deklarace identity ve všech zásad.)

```CSharp
// Controllers\TasksController.cs

public IEnumerable<Models.Task> Get()
{
    string owner = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
    IEnumerable<Models.Task> userTasks = db.Tasks.Where(t => t.owner == owner);
    return userTasks;
}
```

### <a name="validate-hello-permissions-in-hello-token"></a>Ověření oprávnění hello v tokenu hello

Běžné požadavky pro webové rozhraní API je toovalidate hello "obory" v hello token. Tím se zajistí, že tento hello uživatel souhlasí toohello oprávnění požadované tooaccess hello úkolů seznamu služby.

```CSharp
public IEnumerable<Models.Task> Get()
{
    if (ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/scope").Value != "read")
    {
        throw new HttpResponseException(new HttpResponseMessage {
            StatusCode = HttpStatusCode.Unauthorized,
            ReasonPhrase = "hello Scope claim does not contain 'read' or scope claim not found"
        });
    }
    ...
}
```

## <a name="run-hello-sample-app"></a>Spuštění ukázkové aplikace hello

Nakonec sestavte a spusťte `TaskWebApp` a `TaskService`. Vytvořte nějaké úkoly v seznamu úkolů hello uživatele a Všimněte si, že jsou zachované v hello rozhraní API i po ukončení a restartování klienta hello.

## <a name="edit-your-policies"></a>Úprava zásad

Po zabezpečení rozhraní API pomocí Azure AD B2C, můžete experimentovat s vaší zásady Sign v nebo registrace-množství a zobrazení hello efekty (nebo neexistenci) na hello rozhraní API. Můžete manipulovat s deklaracemi identity aplikace hello v hello zásady a změna hello informací o uživateli, který je k dispozici v hello webového rozhraní API. Deklarace identity, které přidáte bude k dispozici tooyour .NET MVC webového rozhraní API v hello `ClaimsPrincipal` objektu, jak je popsáno výše v tomto článku.
