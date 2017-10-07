---
title: "Azure Active Directory aaaUse, tooauthenticate řešení služby Azure Batch | Microsoft Docs"
description: "Batch podporuje Azure AD pro ověřování z hello služby Batch."
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 06/20/2017
ms.author: tamram
ms.openlocfilehash: 6c825c30f1c80bb059a797a2e78367e599acd109
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="authenticate-batch-service-solutions-with-active-directory"></a>Ověření řešení služby Batch se službou Active Directory

Azure Batch podporuje ověřování s [Azure Active Directory] [ aad_about] (Azure AD). Azure AD je víceklientské cloudový adresář společnosti Microsoft a služba identity management. Azure samotné používá tooauthenticate Azure AD, jeho zákazníků, správci služeb a organizační uživatele.

Pokud používáte ověřování Azure AD pomocí služby Azure Batch, můžete ověřovat v jednom ze dvou způsobů:

- Pomocí **integrované ověřování** tooauthenticate uživatele, který komunikuje s aplikací hello. Aplikace prostřednictvím integrovaného ověřování přihlašovacích údajů uživatele shromažďuje a používá tooBatch tyto přihlašovací údaje tooauthenticate přístup k prostředkům.
- Pomocí **instanční objekt** tooauthenticate bezobslužné aplikace. Objekt služby definuje hello zásady a oprávnění pro aplikaci v aplikaci hello toorepresent pořadí, při přístupu k prostředkům v době běhu.

toolearn Další informace o službě Azure AD, najdete v části hello [Azure Active Directory dokumentaci](https://docs.microsoft.com/azure/active-directory/).

## <a name="authentication-and-pool-allocation-mode"></a>Režim ověřování a fondu přidělení

Při vytváření účtu Batch, můžete zadat, kde by měla být pro tento účet vytvořit fondy přidělená. Můžete tooallocate fondy v předplatném služby Batch výchozí hello nebo v předplatném uživatele. Svou volbu ovlivňuje, jak ověřovat přístup tooresources v daném účtu.

- **Předplatné služby batch**. Ve výchozím nastavení jsou fondy Batch přidělené v předplatné služby Batch. Pokud zvolíte tuto možnost, můžete ověřovat přístup tooresources v daném účtu buď pomocí [sdílený klíč](https://docs.microsoft.com/rest/api/batchservice/authenticate-requests-to-the-azure-batch-service) nebo s Azure AD.
- **Předplatné uživatele.** Můžete tooallocate fondy Batch v předplatném uživatele, který určíte. Pokud zvolíte tuto možnost, je třeba ověřit pomocí služby Azure AD.

## <a name="endpoints-for-authentication"></a>Koncové body pro ověřování

aplikace Batch tooauthenticate s Azure AD, je nutné tooinclude některé známé koncové body v kódu.

### <a name="azure-ad-endpoint"></a>Koncový bod Azure AD

základní Hello je koncový bod autority Azure AD:

`https://login.microsoftonline.com/`

tooauthenticate s Azure AD, použijte tento koncový bod společně s hello ID klienta (ID adresáře). ID klienta Hello identifikuje toouse klienta hello Azure AD pro ověřování. tooretrieve hello ID klienta, postupujte podle kroků hello uvedených v [získání ID tenanta hello vaší služby Azure Active Directory](#get-the-tenant-id-for-your-active-directory):

`https://login.microsoftonline.com/<tenant-id>`

> [!NOTE] 
> koncový bod konkrétního klienta Hello je potřeba při ověření pomocí objektu služby. 
> 
> koncový bod pro konkrétního klienta k Hello je volitelný při ověřování pomocí integrovaného ověřování, ale doporučujeme. Však můžete také použít běžné koncového bodu Azure AD hello. běžné koncový bod Hello poskytuje obecné pověření shromažďování rozhraní, když není k dispozici konkrétní klienta. běžné koncový bod Hello je `https://login.microsoftonline.com/common`.
>
>

Další informace o koncové body Azure AD najdete v tématu [scénáře ověřování pro Azure AD][aad_auth_scenarios].

### <a name="batch-resource-endpoint"></a>Koncový bod prostředků batch

Použití hello **koncový bod prostředků Azure Batch** tooacquire token pro ověřování požadavků služby Batch toohello:

`https://batch.core.windows.net/`

## <a name="register-your-application-with-a-tenant"></a>Registrace aplikace pomocí klienta

Hello prvním krokem při použití služby Azure AD tooauthenticate je registrace vaší aplikace v klient služby Azure AD. Registrace aplikace umožňuje toocall hello Azure [Active Directory Authentication Library] [ aad_adal] (ADAL) z vašeho kódu. Hello ADAL poskytuje rozhraní API pro ověřování s Azure AD z vaší aplikace. Registrace aplikace je vyžadováno, zda máte v plánu toouse integrované ověřování nebo hlavní název služby.

Při registraci vaší aplikace, můžete zadat informace o vaší aplikaci tooAzure AD. Potom Azure AD poskytuje ID aplikací použít tooassociate aplikaci s Azure AD za běhu. toolearn Další informace o ID aplikace hello, najdete v části [aplikace a služby hlavní objekty ve službě Azure Active Directory](../active-directory/develop/active-directory-application-objects.md).

tooregister aplikace Batch, postupujte podle kroků hello v hello [přidání aplikace](../active-directory/develop/active-directory-integrating-applications.md#adding-an-application) kapitoly [integrace aplikací s Azure Active Directory][aad_integrate]. Když si zaregistrujete aplikaci jako nativní aplikaci, můžete zadat libovolný platný identifikátor URI pro hello **identifikátor URI pro přesměrování**. To není vyžadováno toobe skutečné koncový bod.

Poté, co jste registrováni vaší aplikace, se zobrazí ID aplikace hello:

![Registrace aplikace Batch pomocí Azure AD](./media/batch-aad-auth/app-registration-data-plane.png)

Další informace o registraci aplikace s Azure AD najdete v tématu [scénáře ověřování pro Azure AD](../active-directory/develop/active-directory-authentication-scenarios.md).

## <a name="get-hello-tenant-id-for-your-active-directory"></a>Získání ID tenanta hello vaší služby Active Directory

ID klienta Hello identifikuje hello klienta Azure AD, který poskytuje ověřování služby tooyour aplikace. tooget hello ID klienta, proveďte tyto kroky:

1. V hello portálu Azure vyberte vaší služby Active Directory.
2. Klikněte na **Vlastnosti**.
3. Zkopírujte hello GUID hodnota zadaná pro ID hello adresáře. Tato hodnota se označuje taky jako ID hello klienta.

![Zkopírujte ID adresáře hello](./media/batch-aad-auth/aad-directory-id.png)


## <a name="use-integrated-authentication"></a>Použít integrované ověřování

tooauthenticate s integrované ověřování, musíte toogrant toohello tooconnect oprávnění vaší aplikace API služby Batch. Tento krok povoluje aplikace tooauthenticate volání toohello Batch služby rozhraní API pomocí Azure AD.

Jakmile jste [zaregistrovat aplikaci](#register-your-application-with-an-azure-ad-tenant), postupujte podle těchto kroků v hello Azure portálu toogrant, že služba Batch toohello přístup:

1. V levém navigačním podokně hello hello portálu Azure, zvolte **více služeb**, klikněte na tlačítko **registrace aplikace**.
2. Hledat hello název vaší aplikace v seznamu hello registrací aplikace:

    ![Vyhledávání pro název aplikace](./media/batch-aad-auth/search-app-registration.png)

3. Otevřete hello **nastavení** okno pro vaši aplikaci. V hello **přístup pomocí rozhraní API** vyberte **požadovaná oprávnění**.
4. V hello **požadovaná oprávnění** okně klikněte na tlačítko hello **přidat** tlačítko.
5. V kroku 1 vyhledejte hello Batch API. Vyhledávání pro každý z těchto řetězců vyhledejte hello rozhraní API:
    1. **MicrosoftAzureBatch**.
    2. **Microsoft Azure Batch**. Novější tenanti služby Azure AD mohou používat tento název.
    3. **ddbf3205-c6bd-46ae-8127-60eb93363864** se hello ID hello Batch API. 
6. Po nalezení hello Batch API, vyberte ho a klikněte na tlačítko hello **vyberte** tlačítko.
6. V kroku 2, vyberte hello zaškrtněte políčko vedle příliš**služby Azure Batch přístup** a klikněte na tlačítko hello **vyberte** tlačítko.
7. Klikněte na tlačítko hello **provádí** tlačítko.

Hello **požadovaných oprávnění** okno teď zobrazuje, které vaše aplikace Azure AD nemá přístup k tooboth ADAL a hello rozhraní API služby Batch. Oprávnění tooADAL automaticky při první registraci vaší aplikace s Azure AD.

![Rozhraní API udělení oprávnění](./media/batch-aad-auth/required-permissions-data-plane.png)

## <a name="use-a-service-principal"></a>Použít hlavní název služby 

tooauthenticate aplikaci, která běží bezobslužné, použijte objekt služby. Poté, co jste registrováni vaší aplikace, pomocí těchto kroků v hello Azure portálu tooconfigure objekt služby:

1. Požádat o tajný klíč pro vaši aplikaci.
2. Přiřaďte aplikaci tooyour role RBAC.

### <a name="request-a-secret-key-for-your-application"></a>Žádost o tajný klíč pro vaši aplikaci

Když se aplikace ověřuje pomocí objektu služby, odešle ID aplikace hello a tajný klíč tooAzure AD. Budete potřebovat toocreate a zkopírovat tajný klíč toouse hello z vašeho kódu.

Postupujte podle těchto kroků v hello portálu Azure:

1. V levém navigačním podokně hello hello portálu Azure, zvolte **více služeb**, klikněte na tlačítko **registrace aplikace**.
2. Vyhledat hello název vaší aplikace v seznamu hello registrací aplikace.
3. Zobrazení hello **nastavení** okno. V hello **přístup pomocí rozhraní API** vyberte **klíče**.
4. toocreate klíč, zadejte popis pro klíč hello. Pak vyberte dobu trvání pro klíč hello jeden nebo dva roky. 
5. Klikněte na tlačítko hello **Uložit** tlačítko toocreate a zobrazit hello klíč. Zkopírujte hello hodnota klíče tooa bezpečné místo, protože nebudou moct tooaccess ho znovu po nechte okno hello. 

    ![Vytvoření tajný klíč](./media/batch-aad-auth/secret-key.png)

### <a name="assign-an-rbac-role-tooyour-application"></a>Přiřazení aplikace tooyour role RBAC

tooauthenticate s hlavní název služby, je nutné tooassign aplikace tooyour role RBAC. Postupujte následovně:

1. V hello portálu Azure přejděte toohello účtu Batch používá vaše aplikace.
2. V hello **nastavení** okno pro účet Batch hello, vyberte **řízení přístupu (IAM)**.
3. Klikněte na tlačítko hello **přidat** tlačítko. 
4. Z hello **Role** rozevíracího seznamu, vyberte buď hello _Přispěvatel_ nebo _čtečky_ role pro vaši aplikaci. Další informace o těchto rolí najdete v tématu [Začínáme s řízením přístupu na základě rolí v hello portál Azure](../active-directory/role-based-access-control-what-is.md).  
5. V hello **vyberte** zadejte hello název vaší aplikace. Vyberte aplikace hello seznamu a klikněte na tlačítko **Uložit**.

Aplikace by se měla zobrazit v nastavení řízení přístupu s přiřazenou RBAC roli. 

![Přiřazení aplikace tooyour role RBAC](./media/batch-aad-auth/app-rbac-role.png)

### <a name="get-hello-tenant-id-for-your-azure-active-directory"></a>Získání ID tenanta hello vaší služby Azure Active Directory

ID klienta Hello identifikuje hello klienta Azure AD, který poskytuje ověřování služby tooyour aplikace. tooget hello ID klienta, proveďte tyto kroky:

1. V hello portálu Azure vyberte vaší služby Active Directory.
2. Klikněte na **Vlastnosti**.
3. Zkopírujte hello GUID hodnota zadaná pro ID hello adresáře. Tato hodnota se označuje taky jako ID hello klienta.

![Zkopírujte ID adresáře hello](./media/batch-aad-auth/aad-directory-id.png)


## <a name="code-examples"></a>Příklady kódu

Příklady kódu Hello v této části ukazují, jak tooauthenticate s Azure AD pomocí integrované ověřování a pomocí objektu služby. Tyto příklady kódu pomocí rozhraní .NET, ale hello koncepty jsou podobné jako u ostatních jazyků.

> [!NOTE]
> Azure AD ověřovací token platnost vyprší po jedné hodině. Při použití dlohotrvající **BatchClient** objektu, doporučujeme vám, že načtení tokenu z ADAL na každý požadavek tooensure máte vždy platný token. 
>
>
> tooachieve, v rozhraní .NET, napíše, aby metoda, načítá hello tokenu z Azure AD a předat tuto metodu tooa **BatchTokenCredentials** objektu jako delegáta. Hello delegáta metoda je volána v každé žádosti toohello Batch služby tooensure poskytovaná platný token. Ve výchozím nastavení ADAL ukládá do mezipaměti tokenů, aby nový token se načítají z Azure AD pouze v případě potřeby. Další informace o tokenů ve službě Azure AD najdete v tématu [scénáře ověřování pro Azure AD][aad_auth_scenarios].
>
>

### <a name="code-example-using-azure-ad-integrated-authentication-with-batch-net"></a>Příklad kódu: používání služby Azure AD integrované ověřování pomocí rozhraní Batch .NET

tooauthenticate pomocí integrovaného ověřování z Batch .NET, odkaz hello [Azure Batch .NET](https://www.nuget.org/packages/Azure.Batch/) balíčku a hello [ADAL](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) balíčku.

Zahrnout hello následující `using` příkazů v kódu:

```csharp
using Microsoft.Azure.Batch;
using Microsoft.Azure.Batch.Auth;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

Referenční dokumentace hello koncového bodu Azure AD ve vašem kódu, včetně hello klienta ID. tooretrieve hello ID klienta, postupujte podle kroků hello uvedených v [získání ID tenanta hello vaší služby Azure Active Directory](#get-the-tenant-id-for-your-active-directory):

```csharp
private const string AuthorityUri = "https://login.microsoftonline.com/<tenant-id>";
```

Referenční hello koncový bod prostředků služby Batch:

```csharp
private const string BatchResourceUri = "https://batch.core.windows.net/";
```

Referenční vašeho účtu Batch:

```csharp
private const string BatchAccountUrl = "https://myaccount.mylocation.batch.azure.com";
```

Zadejte ID aplikace hello (ID klienta) pro vaši aplikaci. ID aplikace Hello je k dispozici z vaší aplikace registrace v hello portálu Azure:

```csharp
private const string ClientId = "<application-id>";
```

Taky zkopírujte identifikátor URI, který jste zadali během procesu registrace hello hello přesměrování. Hello přesměrování URI zadaný v kódu musí odpovídat hello přesměrování identifikátor URI, který jste zadali při registraci aplikace hello:

```csharp
private const string RedirectUri = "http://mybatchdatasample";
```

Zápisu zpětného volání metoda tooacquire hello ověřovací token ze služby Azure AD. Hello **GetAuthenticationTokenAsync** metoda zpětného volání, které jsou tady uvedené volání ADAL tooauthenticate uživatel, který komunikuje s aplikací hello. Hello **AcquireTokenAsync** metoda zadaný pomocí ADAL hello vyzve uživatele k zadání přihlašovacích údajů a aplikace hello pokračuje jednou hello uživatele poskytuje jim (Pokud již v mezipaměti přihlašovacích údajů):

```csharp
public static async Task<string> GetAuthenticationTokenAsync()
{
    var authContext = new AuthenticationContext(AuthorityUri);

    // Acquire hello authentication token from Azure AD.
    var authResult = await authContext.AcquireTokenAsync(BatchResourceUri, 
                                                        ClientId, 
                                                        new Uri(RedirectUri), 
                                                        new PlatformParameters(PromptBehavior.Auto));

    return authResult.AccessToken;
}
```

Vytvořit **BatchTokenCredentials** objekt, který přebírá hello delegáta jako parametr. Použít tyto přihlašovací údaje tooopen **BatchClient** objektu. Můžete ji použít **BatchClient** objekt pro následující operace proti hello služby Batch:

```csharp
public static async Task PerformBatchOperations()
{
    Func<Task<string>> tokenProvider = () => GetAuthenticationTokenAsync();

    using (var client = await BatchClient.OpenAsync(new BatchTokenCredentials(BatchAccountUrl, tokenProvider)))
    {
        await client.JobOperations.ListJobs().ToListAsync();
    }
}
```

### <a name="code-example-using-an-azure-ad-service-principal-with-batch-net"></a>Příklad kódu: objektu služby Azure AD pomocí rozhraní Batch .NET

tooauthenticate pomocí objektu služby z Batch .NET, odkaz hello [Azure Batch .NET](https://www.nuget.org/packages/Azure.Batch/) balíčku a hello [ADAL](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) balíčku.

Zahrnout hello následující `using` příkazů v kódu:

```csharp
using Microsoft.Azure.Batch;
using Microsoft.Azure.Batch.Auth;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

Referenční dokumentace hello koncového bodu Azure AD ve vašem kódu, včetně hello klienta ID. Pokud používáte hlavní název služby, je nutné zadat koncový bod konkrétního klienta. tooretrieve hello ID klienta, postupujte podle kroků hello uvedených v [získání ID tenanta hello vaší služby Azure Active Directory](#get-the-tenant-id-for-your-active-directory):

```csharp
private const string AuthorityUri = "https://login.microsoftonline.com/<tenant-id>";
```

Referenční hello koncový bod prostředků služby Batch:  

```csharp
private const string BatchResourceUri = "https://batch.core.windows.net/";
```

Referenční vašeho účtu Batch:

```csharp
private const string BatchAccountUrl = "https://myaccount.mylocation.batch.azure.com";
```

Zadejte ID aplikace hello (ID klienta) pro vaši aplikaci. ID aplikace Hello je k dispozici z vaší aplikace registrace v hello portálu Azure:

```csharp
private const string ClientId = "<application-id>";
```

Zadejte hello tajný klíč, který jste zkopírovali ze hello portálu Azure:

```csharp
private const string ClientKey = "<secret-key>";
```

Zápisu zpětného volání metoda tooacquire hello ověřovací token ze služby Azure AD. Hello **GetAuthenticationTokenAsync** metoda zpětného volání, které jsou tady uvedené volání ADAL pro bezobslužné ověřování:

```csharp
public static async Task<string> GetAuthenticationTokenAsync()
{
    AuthenticationContext authContext = new AuthenticationContext(AuthorityUri);
    AuthenticationResult authResult = await authContext.AcquireTokenAsync(BatchResourceUri, new ClientCredential(ClientId, ClientKey));

    return authResult.AccessToken;
}
```

Vytvořit **BatchTokenCredentials** objekt, který přebírá hello delegáta jako parametr. Použít tyto přihlašovací údaje tooopen **BatchClient** objektu. Můžete jej pak použít **BatchClient** objekt pro následující operace proti hello služby Batch:

```csharp
public static async Task PerformBatchOperations()
{
    Func<Task<string>> tokenProvider = () => GetAuthenticationTokenAsync();

    using (var client = await BatchClient.OpenAsync(new BatchTokenCredentials(BatchAccountUrl, tokenProvider)))
    {
        await client.JobOperations.ListJobs().ToListAsync();
    }
}
```

## <a name="next-steps"></a>Další kroky

toolearn Další informace o službě Azure AD, najdete v části hello [Azure Active Directory dokumentaci](https://docs.microsoft.com/azure/active-directory/). Podrobné příklady znázorňující, jak jsou k dispozici v hello toouse ADAL [ukázky kódu Azure](https://azure.microsoft.com/resources/samples/?service=active-directory) knihovny.

toolearn více o objektech služby najdete v části [aplikace a služby hlavní objekty ve službě Azure Active Directory](../active-directory/develop/active-directory-application-objects.md). hello toocreate službu objektu zabezpečení pomocí portálu Azure najdete [pomocí aplikace portálu toocreate Active Directory a objektu služby, které mají přístup k prostředkům](../resource-group-create-service-principal-portal.md). Hlavní název služby můžete vytvořit také pomocí prostředí PowerShell nebo rozhraní příkazového řádku Azure. 

tooauthenticate Batch správy aplikací pomocí služby Azure AD, najdete v části [řešení pro správu Batch ověření pomocí služby Active Directory](batch-aad-auth-management.md). 

[aad_about]: ../active-directory/active-directory-whatis.md "Co je Azure Active Directory?"
[aad_adal]: ../active-directory/active-directory-authentication-libraries.md
[aad_auth_scenarios]: ../active-directory/active-directory-authentication-scenarios.md "Scénáře ověřování pro Azure AD"
[aad_integrate]: ../active-directory/active-directory-integrating-applications.md "Integrace aplikací s Azure Active Directory"
[azure_portal]: http://portal.azure.com
