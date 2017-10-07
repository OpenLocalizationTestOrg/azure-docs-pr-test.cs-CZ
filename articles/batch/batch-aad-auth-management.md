---
title: "řešení Batch správy tooauthenticate aaaUse Azure Active Directory | Microsoft Docs"
description: "Aplikace vytvořené s Azure resource manager a poskytovatele prostředků Batch hello ověřování s Azure AD."
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 04/27/2017
ms.author: tamram
ms.openlocfilehash: 192aa9f8d7cbfc0282a4a0c33ab1659f1f351525
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="authenticate-batch-management-solutions-with-active-directory"></a>Ověření řešení pro správu Batch se službou Active Directory

Aplikace, které volání služby Azure Batch Management hello ověření pomocí [Azure Active Directory] [ aad_about] (Azure AD). Azure AD je víceklientské cloudový adresář společnosti Microsoft a služba identity management. Azure samotné používá Azure AD pro ověřování hello jeho zákazníků, správci služeb a organizační uživatele.

Knihovna rozhraní Batch Management .NET Hello zpřístupní typy pro práci s účty Batch, klíče účtu, aplikace a balíčky aplikací. Knihovna rozhraní Batch Management .NET Hello je klient poskytovatel prostředků Azure a je použít v kombinaci s [Azure Resource Manager] [ resman_overview] toomanage tyto prostředky prostřednictvím kódu programu. Azure AD je požadovaná tooauthenticate požadavky prostřednictvím všechny klienty poskytovatele prostředků Azure, včetně hello knihovny Batch Management .NET a prostřednictvím [Azure Resource Manager][resman_overview].

V tomto článku jsme prozkoumat pomocí Azure AD tooauthenticate z aplikací, které pomocí knihovny Batch Management .NET hello. Ukážeme, jak toouse Azure AD tooauthenticate správce předplatného nebo spolusprávce pomocí integrované ověřování. Používáme hello [AccountManagment] [ acct_mgmt_sample] ukázkový projekt, dostupná na Githubu, toowalk přes Azure AD pomocí knihovny Batch Management .NET hello.

toolearn Další informace o použití knihovny Batch Management .NET hello a ukázkové AccountManagement hello, najdete v části [Batch Správa účtů a kvót s hello Batch Management Klientská knihovna pro .NET](batch-management-dotnet.md).

## <a name="register-your-application-with-azure-ad"></a>Registrace vaší aplikace s Azure AD

Hello Azure [Active Directory Authentication Library] [ aad_adal] (ADAL) poskytuje programovací rozhraní tooAzure AD pro použití v rámci vaší aplikace. toocall ADAL z aplikace, je nutné zaregistrovat aplikaci v klient služby Azure AD. Při registraci vaší aplikace, je třeba zadat Azure AD s informacemi o aplikaci, včetně názvu pro něj v rámci klienta Azure AD hello. Potom Azure AD poskytuje ID aplikací použít tooassociate aplikaci s Azure AD za běhu. toolearn Další informace o ID aplikace hello, najdete v části [aplikace a služby hlavní objekty ve službě Azure Active Directory](../active-directory/develop/active-directory-application-objects.md).

hello tooregister AccountManagement ukázkovou aplikaci, postupujte podle kroků hello v hello [přidání aplikace](../active-directory/develop/active-directory-integrating-applications.md#adding-an-application) kapitoly [integrace aplikací s Azure Active Directory] [ aad_integrate]. Zadejte **nativní klientská aplikace** pro typ hello aplikace. Hello oborový standard identifikátor URI OAuth 2.0 pro hello **identifikátor URI pro přesměrování** je `urn:ietf:wg:oauth:2.0:oob`. Můžete však zadat libovolný platný identifikátor URI (například `http://myaccountmanagementsample`) pro hello **identifikátor URI pro přesměrování**, protože to není vyžadováno toobe skutečné koncový bod:

![](./media/batch-aad-auth-management/app-registration-management-plane.png)

Po dokončení procesu registrace hello budete najdete v části ID aplikace hello a hello ID objektu (instanční objekt) uvedené pro vaši aplikaci.  

![](./media/batch-aad-auth-management/app-registration-client-id.png)

## <a name="grant-hello-azure-resource-manager-api-access-tooyour-application"></a>Udělit hello rozhraní API služby Azure Resource Manager přístup tooyour aplikace

V dalším kroku budete potřebovat toodelegate přístup tooyour aplikace toohello rozhraní API Správce prostředků Azure. identifikátor Hello Azure AD pro hello rozhraní API Správce prostředků je **Windows Azure Service Management API**.

Postupujte podle těchto kroků v hello portálu Azure:

1. V levém navigačním podokně hello hello portálu Azure, zvolte **více služeb**, klikněte na tlačítko **registrace aplikace**a klikněte na tlačítko **přidat**.
2. Hledat hello název vaší aplikace v seznamu hello registrací aplikace:

    ![Vyhledávání pro název aplikace](./media/batch-aad-auth-management/search-app-registration.png)

3. Zobrazení hello **nastavení** okno. V hello **přístup pomocí rozhraní API** vyberte **požadovaná oprávnění**.
4. Klikněte na tlačítko **přidat** tooadd nové požadované oprávnění. 
5. V kroku 1 zadejte **Windows Azure Service Management API**, vyberte toto rozhraní API hello seznam výsledků a klikněte na hello **vyberte** tlačítko.
6. V kroku 2, vyberte hello zaškrtněte políčko vedle příliš**Azure přístup k modelu nasazení classic jako uživatele organizaci**a klikněte na tlačítko hello **vyberte** tlačítko.
7. Klikněte na tlačítko hello **provádí** tlačítko.

Hello **požadovaných oprávnění** okno teď ukazuje, že aplikace tooyour oprávnění jsou udělena tooboth hello ADAL a rozhraní API Resource Manager. Oprávnění tooADAL ve výchozím nastavení při první registraci vaší aplikace s Azure AD.

![Delegovat oprávnění toohello rozhraní API služby Azure Resource Manager](./media/batch-aad-auth-management/required-permissions-management-plane.png)

## <a name="azure-ad-endpoints"></a>Koncové body Azure AD

tooauthenticate vašich řešení Batch Management s Azure AD, budete potřebovat dva dobře známé koncové body.

- Hello **běžné koncového bodu Azure AD** poskytuje obecné pověření shromažďování rozhraní, pokud je konkrétní klienta není zadaný, jako v případě hello integrované ověřování:

    `https://login.microsoftonline.com/common`

- Hello **koncový bod Azure Resource Manager** je použité tooacquire token pro ověřování požadavků toohello Batch management service:

    `https://management.core.windows.net/`

Hello AccountManagement ukázkovou aplikaci definuje konstanty pro tyto koncové body. Ponechte tyto konstanty beze změny:

```csharp
// Azure Active Directory "common" endpoint.
private const string AuthorityUri = "https://login.microsoftonline.com/common";
// Azure Resource Manager endpoint 
private const string ResourceUri = "https://management.core.windows.net/";
```

## <a name="reference-your-application-id"></a>Referenční ID vaší aplikace 

Klientské aplikace používá za běhu hello aplikace ID (také odkazované tooas hello ID klienta) tooaccess Azure AD. Po registraci vaší aplikace v portálu Azure hello aktualizujte váš kód toouse hello aplikace ID, které poskytuje Azure AD pro zaregistrovanou aplikaci. V hello AccountManagement ukázkovou aplikaci zkopírujte z příslušné konstanta hello Azure portálu toohello vaše ID aplikace:

```csharp
// Specify hello unique identifier (hello "Client ID") for your application. This is required so that your
// native client application (i.e. this sample) can access hello Microsoft Azure AD Graph API. For information
// about registering an application in Azure Active Directory, please see "Adding an Application" here:
// https://azure.microsoft.com/documentation/articles/active-directory-integrating-applications/
private const string ClientId = "<application-id>";
```
Taky zkopírujte identifikátor URI, který jste zadali během procesu registrace hello hello přesměrování. Hello přesměrování URI zadaný v kódu musí odpovídat hello přesměrování identifikátor URI, který jste zadali při registraci aplikace hello.

```csharp
// hello URI toowhich Azure AD will redirect in response tooan OAuth 2.0 request. This value is
// specified by you when you register an application with AAD (see ClientId comment). It does not
// need toobe a real endpoint, but must be a valid URI (e.g. https://accountmgmtsampleapp).
private const string RedirectUri = "http://myaccountmanagementsample";
```

## <a name="acquire-an-azure-ad-authentication-token"></a>Získání tokenu ověřování Azure AD

Po registraci hello AccountManagement ukázka v klientovi Azure AD hello a aktualizaci hello ukázka zdrojového kódu se hodnoty, ukázka hello je připraven tooauthenticate pomocí služby Azure AD. Při spuštění ukázkové hello hello ADAL pokusí tooacquire ověřovací token. V tomto kroku budete vyzváni k zadání pověření Microsoft: 

```csharp
// Obtain an access token using hello "common" AAD resource. This allows hello application
// tooquery AAD for information that lies outside hello application's tenant (such as for
// querying subscription information in your Azure account).
AuthenticationContext authContext = new AuthenticationContext(AuthorityUri);
AuthenticationResult authResult = authContext.AcquireToken(ResourceUri,
                                                        ClientId,
                                                        new Uri(RedirectUri),
                                                        PromptBehavior.Auto);
```

Po zadání přihlašovacích údajů můžete pokračovat hello ukázkovou aplikaci toohello tooissue ověření žádosti o služby Batch management. 

## <a name="next-steps"></a>Další kroky

Další informace o spouštění hello [AccountManagement ukázkovou aplikaci][acct_mgmt_sample], najdete v části [Batch Správa účtů a kvót s hello Batch Management Klientská knihovna pro .NET](batch-management-dotnet.md).

toolearn Další informace o službě Azure AD, najdete v části hello [Azure Active Directory dokumentaci](https://docs.microsoft.com/azure/active-directory/). Podrobné příklady znázorňující, jak jsou k dispozici v hello toouse ADAL [ukázky kódu Azure](https://azure.microsoft.com/resources/samples/?service=active-directory) knihovny.

aplikace služby Batch tooauthenticate pomocí služby Azure AD, najdete v části [řešení služby Batch ověření pomocí služby Active Directory](batch-aad-auth.md). 


[aad_about]: ../active-directory/active-directory-whatis.md "Co je Azure Active Directory?"
[aad_adal]: ../active-directory/active-directory-authentication-libraries.md
[aad_auth_scenarios]: ../active-directory/active-directory-authentication-scenarios.md "Scénáře ověřování pro Azure AD"
[aad_integrate]: ../active-directory/active-directory-integrating-applications.md "Integrace aplikací s Azure Active Directory"
[acct_mgmt_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/AccountManagement
[azure_portal]: http://portal.azure.com
[resman_overview]: ../azure-resource-manager/resource-group-overview.md
