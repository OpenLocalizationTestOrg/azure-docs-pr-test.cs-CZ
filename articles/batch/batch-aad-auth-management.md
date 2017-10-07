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
# <a name="authenticate-batch-management-solutions-with-active-directory"></a><span data-ttu-id="4217e-103">Ověření řešení pro správu Batch se službou Active Directory</span><span class="sxs-lookup"><span data-stu-id="4217e-103">Authenticate Batch Management solutions with Active Directory</span></span>

<span data-ttu-id="4217e-104">Aplikace, které volání služby Azure Batch Management hello ověření pomocí [Azure Active Directory] [ aad_about] (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="4217e-104">Applications that call hello Azure Batch Management service authenticate with [Azure Active Directory][aad_about] (Azure AD).</span></span> <span data-ttu-id="4217e-105">Azure AD je víceklientské cloudový adresář společnosti Microsoft a služba identity management.</span><span class="sxs-lookup"><span data-stu-id="4217e-105">Azure AD is Microsoft’s multi-tenant cloud based directory and identity management service.</span></span> <span data-ttu-id="4217e-106">Azure samotné používá Azure AD pro ověřování hello jeho zákazníků, správci služeb a organizační uživatele.</span><span class="sxs-lookup"><span data-stu-id="4217e-106">Azure itself uses Azure AD for hello authentication of its customers, service administrators, and organizational users.</span></span>

<span data-ttu-id="4217e-107">Knihovna rozhraní Batch Management .NET Hello zpřístupní typy pro práci s účty Batch, klíče účtu, aplikace a balíčky aplikací.</span><span class="sxs-lookup"><span data-stu-id="4217e-107">hello Batch Management .NET library exposes types for working with Batch accounts, account keys, applications, and application packages.</span></span> <span data-ttu-id="4217e-108">Knihovna rozhraní Batch Management .NET Hello je klient poskytovatel prostředků Azure a je použít v kombinaci s [Azure Resource Manager] [ resman_overview] toomanage tyto prostředky prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="4217e-108">hello Batch Management .NET library is an Azure resource provider client, and is used together with [Azure Resource Manager][resman_overview] toomanage these resources programmatically.</span></span> <span data-ttu-id="4217e-109">Azure AD je požadovaná tooauthenticate požadavky prostřednictvím všechny klienty poskytovatele prostředků Azure, včetně hello knihovny Batch Management .NET a prostřednictvím [Azure Resource Manager][resman_overview].</span><span class="sxs-lookup"><span data-stu-id="4217e-109">Azure AD is required tooauthenticate requests made through any Azure resource provider client, including hello Batch Management .NET library, and through [Azure Resource Manager][resman_overview].</span></span>

<span data-ttu-id="4217e-110">V tomto článku jsme prozkoumat pomocí Azure AD tooauthenticate z aplikací, které pomocí knihovny Batch Management .NET hello.</span><span class="sxs-lookup"><span data-stu-id="4217e-110">In this article, we explore using Azure AD tooauthenticate from applications that use hello Batch Management .NET library.</span></span> <span data-ttu-id="4217e-111">Ukážeme, jak toouse Azure AD tooauthenticate správce předplatného nebo spolusprávce pomocí integrované ověřování.</span><span class="sxs-lookup"><span data-stu-id="4217e-111">We show how toouse Azure AD tooauthenticate a subscription administrator or co-administrator, using integrated authentication.</span></span> <span data-ttu-id="4217e-112">Používáme hello [AccountManagment] [ acct_mgmt_sample] ukázkový projekt, dostupná na Githubu, toowalk přes Azure AD pomocí knihovny Batch Management .NET hello.</span><span class="sxs-lookup"><span data-stu-id="4217e-112">We use hello [AccountManagment][acct_mgmt_sample] sample project, available on GitHub, toowalk through using Azure AD with hello Batch Management .NET library.</span></span>

<span data-ttu-id="4217e-113">toolearn Další informace o použití knihovny Batch Management .NET hello a ukázkové AccountManagement hello, najdete v části [Batch Správa účtů a kvót s hello Batch Management Klientská knihovna pro .NET](batch-management-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="4217e-113">toolearn more about using hello Batch Management .NET library and hello AccountManagement sample, see [Manage Batch accounts and quotas with hello Batch Management client library for .NET](batch-management-dotnet.md).</span></span>

## <a name="register-your-application-with-azure-ad"></a><span data-ttu-id="4217e-114">Registrace vaší aplikace s Azure AD</span><span class="sxs-lookup"><span data-stu-id="4217e-114">Register your application with Azure AD</span></span>

<span data-ttu-id="4217e-115">Hello Azure [Active Directory Authentication Library] [ aad_adal] (ADAL) poskytuje programovací rozhraní tooAzure AD pro použití v rámci vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="4217e-115">hello Azure [Active Directory Authentication Library][aad_adal] (ADAL) provides a programmatic interface tooAzure AD for use within your applications.</span></span> <span data-ttu-id="4217e-116">toocall ADAL z aplikace, je nutné zaregistrovat aplikaci v klient služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4217e-116">toocall ADAL from your application, you must register your application in an Azure AD tenant.</span></span> <span data-ttu-id="4217e-117">Při registraci vaší aplikace, je třeba zadat Azure AD s informacemi o aplikaci, včetně názvu pro něj v rámci klienta Azure AD hello.</span><span class="sxs-lookup"><span data-stu-id="4217e-117">When you register your application, you supply Azure AD with information about your application, including a name for it within hello Azure AD tenant.</span></span> <span data-ttu-id="4217e-118">Potom Azure AD poskytuje ID aplikací použít tooassociate aplikaci s Azure AD za běhu.</span><span class="sxs-lookup"><span data-stu-id="4217e-118">Azure AD then provides an application ID that you use tooassociate your application with Azure AD at runtime.</span></span> <span data-ttu-id="4217e-119">toolearn Další informace o ID aplikace hello, najdete v části [aplikace a služby hlavní objekty ve službě Azure Active Directory](../active-directory/develop/active-directory-application-objects.md).</span><span class="sxs-lookup"><span data-stu-id="4217e-119">toolearn more about hello application ID, see [Application and service principal objects in Azure Active Directory](../active-directory/develop/active-directory-application-objects.md).</span></span>

<span data-ttu-id="4217e-120">hello tooregister AccountManagement ukázkovou aplikaci, postupujte podle kroků hello v hello [přidání aplikace](../active-directory/develop/active-directory-integrating-applications.md#adding-an-application) kapitoly [integrace aplikací s Azure Active Directory] [ aad_integrate].</span><span class="sxs-lookup"><span data-stu-id="4217e-120">tooregister hello AccountManagement sample application, follow hello steps in hello [Adding an Application](../active-directory/develop/active-directory-integrating-applications.md#adding-an-application) section in [Integrating applications with Azure Active Directory][aad_integrate].</span></span> <span data-ttu-id="4217e-121">Zadejte **nativní klientská aplikace** pro typ hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="4217e-121">Specify **Native Client Application** for hello type of application.</span></span> <span data-ttu-id="4217e-122">Hello oborový standard identifikátor URI OAuth 2.0 pro hello **identifikátor URI pro přesměrování** je `urn:ietf:wg:oauth:2.0:oob`.</span><span class="sxs-lookup"><span data-stu-id="4217e-122">hello industry standard OAuth 2.0 URI for hello **Redirect URI** is `urn:ietf:wg:oauth:2.0:oob`.</span></span> <span data-ttu-id="4217e-123">Můžete však zadat libovolný platný identifikátor URI (například `http://myaccountmanagementsample`) pro hello **identifikátor URI pro přesměrování**, protože to není vyžadováno toobe skutečné koncový bod:</span><span class="sxs-lookup"><span data-stu-id="4217e-123">However, you can specify any valid URI (such as `http://myaccountmanagementsample`) for hello **Redirect URI**, as it does not need toobe a real endpoint:</span></span>

![](./media/batch-aad-auth-management/app-registration-management-plane.png)

<span data-ttu-id="4217e-124">Po dokončení procesu registrace hello budete najdete v části ID aplikace hello a hello ID objektu (instanční objekt) uvedené pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4217e-124">Once you complete hello registration process, you'll see hello application ID and hello object (service principal) ID listed for your application.</span></span>  

![](./media/batch-aad-auth-management/app-registration-client-id.png)

## <a name="grant-hello-azure-resource-manager-api-access-tooyour-application"></a><span data-ttu-id="4217e-125">Udělit hello rozhraní API služby Azure Resource Manager přístup tooyour aplikace</span><span class="sxs-lookup"><span data-stu-id="4217e-125">Grant hello Azure Resource Manager API access tooyour application</span></span>

<span data-ttu-id="4217e-126">V dalším kroku budete potřebovat toodelegate přístup tooyour aplikace toohello rozhraní API Správce prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="4217e-126">Next, you'll need toodelegate access tooyour application toohello Azure Resource Manager API.</span></span> <span data-ttu-id="4217e-127">identifikátor Hello Azure AD pro hello rozhraní API Správce prostředků je **Windows Azure Service Management API**.</span><span class="sxs-lookup"><span data-stu-id="4217e-127">hello Azure AD identifier for hello Resource Manager API is **Windows Azure Service Management API**.</span></span>

<span data-ttu-id="4217e-128">Postupujte podle těchto kroků v hello portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="4217e-128">Follow these steps in hello Azure portal:</span></span>

1. <span data-ttu-id="4217e-129">V levém navigačním podokně hello hello portálu Azure, zvolte **více služeb**, klikněte na tlačítko **registrace aplikace**a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="4217e-129">In hello left-hand navigation pane of hello Azure portal, choose **More Services**, click **App Registrations**, and click **Add**.</span></span>
2. <span data-ttu-id="4217e-130">Hledat hello název vaší aplikace v seznamu hello registrací aplikace:</span><span class="sxs-lookup"><span data-stu-id="4217e-130">Search for hello name of your application in hello list of app registrations:</span></span>

    ![Vyhledávání pro název aplikace](./media/batch-aad-auth-management/search-app-registration.png)

3. <span data-ttu-id="4217e-132">Zobrazení hello **nastavení** okno.</span><span class="sxs-lookup"><span data-stu-id="4217e-132">Display hello **Settings** blade.</span></span> <span data-ttu-id="4217e-133">V hello **přístup pomocí rozhraní API** vyberte **požadovaná oprávnění**.</span><span class="sxs-lookup"><span data-stu-id="4217e-133">In hello **API Access** section, select **Required permissions**.</span></span>
4. <span data-ttu-id="4217e-134">Klikněte na tlačítko **přidat** tooadd nové požadované oprávnění.</span><span class="sxs-lookup"><span data-stu-id="4217e-134">Click **Add** tooadd a new required permission.</span></span> 
5. <span data-ttu-id="4217e-135">V kroku 1 zadejte **Windows Azure Service Management API**, vyberte toto rozhraní API hello seznam výsledků a klikněte na hello **vyberte** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="4217e-135">In step 1, enter **Windows Azure Service Management API**, select that API from hello list of results, and click hello **Select** button.</span></span>
6. <span data-ttu-id="4217e-136">V kroku 2, vyberte hello zaškrtněte políčko vedle příliš**Azure přístup k modelu nasazení classic jako uživatele organizaci**a klikněte na tlačítko hello **vyberte** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="4217e-136">In step 2, select hello check box next too**Access Azure classic deployment model as organization users**, and click hello **Select** button.</span></span>
7. <span data-ttu-id="4217e-137">Klikněte na tlačítko hello **provádí** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="4217e-137">Click hello **Done** button.</span></span>

<span data-ttu-id="4217e-138">Hello **požadovaných oprávnění** okno teď ukazuje, že aplikace tooyour oprávnění jsou udělena tooboth hello ADAL a rozhraní API Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="4217e-138">hello **Required Permissions** blade now shows that permissions tooyour application are granted tooboth hello ADAL and Resource Manager APIs.</span></span> <span data-ttu-id="4217e-139">Oprávnění tooADAL ve výchozím nastavení při první registraci vaší aplikace s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4217e-139">Permissions are granted tooADAL by default when you first register your app with Azure AD.</span></span>

![Delegovat oprávnění toohello rozhraní API služby Azure Resource Manager](./media/batch-aad-auth-management/required-permissions-management-plane.png)

## <a name="azure-ad-endpoints"></a><span data-ttu-id="4217e-141">Koncové body Azure AD</span><span class="sxs-lookup"><span data-stu-id="4217e-141">Azure AD endpoints</span></span>

<span data-ttu-id="4217e-142">tooauthenticate vašich řešení Batch Management s Azure AD, budete potřebovat dva dobře známé koncové body.</span><span class="sxs-lookup"><span data-stu-id="4217e-142">tooauthenticate your Batch Management solutions with Azure AD, you'll need two well-known endpoints.</span></span>

- <span data-ttu-id="4217e-143">Hello **běžné koncového bodu Azure AD** poskytuje obecné pověření shromažďování rozhraní, pokud je konkrétní klienta není zadaný, jako v případě hello integrované ověřování:</span><span class="sxs-lookup"><span data-stu-id="4217e-143">hello **Azure AD common endpoint** provides a generic credential gathering interface when a specific tenant is not provided, as in hello case of integrated authentication:</span></span>

    `https://login.microsoftonline.com/common`

- <span data-ttu-id="4217e-144">Hello **koncový bod Azure Resource Manager** je použité tooacquire token pro ověřování požadavků toohello Batch management service:</span><span class="sxs-lookup"><span data-stu-id="4217e-144">hello **Azure Resource Manager endpoint** is used tooacquire a token for authenticating requests toohello Batch management service:</span></span>

    `https://management.core.windows.net/`

<span data-ttu-id="4217e-145">Hello AccountManagement ukázkovou aplikaci definuje konstanty pro tyto koncové body.</span><span class="sxs-lookup"><span data-stu-id="4217e-145">hello AccountManagement sample application defines constants for these endpoints.</span></span> <span data-ttu-id="4217e-146">Ponechte tyto konstanty beze změny:</span><span class="sxs-lookup"><span data-stu-id="4217e-146">Leave these constants unchanged:</span></span>

```csharp
// Azure Active Directory "common" endpoint.
private const string AuthorityUri = "https://login.microsoftonline.com/common";
// Azure Resource Manager endpoint 
private const string ResourceUri = "https://management.core.windows.net/";
```

## <a name="reference-your-application-id"></a><span data-ttu-id="4217e-147">Referenční ID vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="4217e-147">Reference your application ID</span></span> 

<span data-ttu-id="4217e-148">Klientské aplikace používá za běhu hello aplikace ID (také odkazované tooas hello ID klienta) tooaccess Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4217e-148">Your client application uses hello application ID (also referred tooas hello client ID) tooaccess Azure AD at runtime.</span></span> <span data-ttu-id="4217e-149">Po registraci vaší aplikace v portálu Azure hello aktualizujte váš kód toouse hello aplikace ID, které poskytuje Azure AD pro zaregistrovanou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4217e-149">Once you've registered your application in hello Azure portal, update your code toouse hello application ID provided by Azure AD for your registered application.</span></span> <span data-ttu-id="4217e-150">V hello AccountManagement ukázkovou aplikaci zkopírujte z příslušné konstanta hello Azure portálu toohello vaše ID aplikace:</span><span class="sxs-lookup"><span data-stu-id="4217e-150">In hello AccountManagement sample application, copy your application ID from hello Azure portal toohello appropriate constant:</span></span>

```csharp
// Specify hello unique identifier (hello "Client ID") for your application. This is required so that your
// native client application (i.e. this sample) can access hello Microsoft Azure AD Graph API. For information
// about registering an application in Azure Active Directory, please see "Adding an Application" here:
// https://azure.microsoft.com/documentation/articles/active-directory-integrating-applications/
private const string ClientId = "<application-id>";
```
<span data-ttu-id="4217e-151">Taky zkopírujte identifikátor URI, který jste zadali během procesu registrace hello hello přesměrování.</span><span class="sxs-lookup"><span data-stu-id="4217e-151">Also copy hello redirect URI that you specified during hello registration process.</span></span> <span data-ttu-id="4217e-152">Hello přesměrování URI zadaný v kódu musí odpovídat hello přesměrování identifikátor URI, který jste zadali při registraci aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="4217e-152">hello redirect URI specified in your code must match hello redirect URI that you provided when you registered hello application.</span></span>

```csharp
// hello URI toowhich Azure AD will redirect in response tooan OAuth 2.0 request. This value is
// specified by you when you register an application with AAD (see ClientId comment). It does not
// need toobe a real endpoint, but must be a valid URI (e.g. https://accountmgmtsampleapp).
private const string RedirectUri = "http://myaccountmanagementsample";
```

## <a name="acquire-an-azure-ad-authentication-token"></a><span data-ttu-id="4217e-153">Získání tokenu ověřování Azure AD</span><span class="sxs-lookup"><span data-stu-id="4217e-153">Acquire an Azure AD authentication token</span></span>

<span data-ttu-id="4217e-154">Po registraci hello AccountManagement ukázka v klientovi Azure AD hello a aktualizaci hello ukázka zdrojového kódu se hodnoty, ukázka hello je připraven tooauthenticate pomocí služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4217e-154">After you register hello AccountManagement sample in hello Azure AD tenant and update hello sample source code with your values, hello sample is ready tooauthenticate using Azure AD.</span></span> <span data-ttu-id="4217e-155">Při spuštění ukázkové hello hello ADAL pokusí tooacquire ověřovací token.</span><span class="sxs-lookup"><span data-stu-id="4217e-155">When you run hello sample, hello ADAL attempts tooacquire an authentication token.</span></span> <span data-ttu-id="4217e-156">V tomto kroku budete vyzváni k zadání pověření Microsoft:</span><span class="sxs-lookup"><span data-stu-id="4217e-156">At this step, it prompts you for your Microsoft credentials:</span></span> 

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

<span data-ttu-id="4217e-157">Po zadání přihlašovacích údajů můžete pokračovat hello ukázkovou aplikaci toohello tooissue ověření žádosti o služby Batch management.</span><span class="sxs-lookup"><span data-stu-id="4217e-157">After you provide your credentials, hello sample application can proceed tooissue authenticated requests toohello Batch management service.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="4217e-158">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4217e-158">Next steps</span></span>

<span data-ttu-id="4217e-159">Další informace o spouštění hello [AccountManagement ukázkovou aplikaci][acct_mgmt_sample], najdete v části [Batch Správa účtů a kvót s hello Batch Management Klientská knihovna pro .NET](batch-management-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="4217e-159">For more information on running hello [AccountManagement sample application][acct_mgmt_sample], see [Manage Batch accounts and quotas with hello Batch Management client library for .NET](batch-management-dotnet.md).</span></span>

<span data-ttu-id="4217e-160">toolearn Další informace o službě Azure AD, najdete v části hello [Azure Active Directory dokumentaci](https://docs.microsoft.com/azure/active-directory/).</span><span class="sxs-lookup"><span data-stu-id="4217e-160">toolearn more about Azure AD, see hello [Azure Active Directory Documentation](https://docs.microsoft.com/azure/active-directory/).</span></span> <span data-ttu-id="4217e-161">Podrobné příklady znázorňující, jak jsou k dispozici v hello toouse ADAL [ukázky kódu Azure](https://azure.microsoft.com/resources/samples/?service=active-directory) knihovny.</span><span class="sxs-lookup"><span data-stu-id="4217e-161">In-depth examples showing how toouse ADAL are available in hello [Azure Code Samples](https://azure.microsoft.com/resources/samples/?service=active-directory) library.</span></span>

<span data-ttu-id="4217e-162">aplikace služby Batch tooauthenticate pomocí služby Azure AD, najdete v části [řešení služby Batch ověření pomocí služby Active Directory](batch-aad-auth.md).</span><span class="sxs-lookup"><span data-stu-id="4217e-162">tooauthenticate Batch service applications using Azure AD, see [Authenticate Batch service solutions with Active Directory](batch-aad-auth.md).</span></span> 


[aad_about]: ../active-directory/active-directory-whatis.md "Co je Azure Active Directory?"
[aad_adal]: ../active-directory/active-directory-authentication-libraries.md
[aad_auth_scenarios]: ../active-directory/active-directory-authentication-scenarios.md "Scénáře ověřování pro Azure AD"
[aad_integrate]: ../active-directory/active-directory-integrating-applications.md "Integrace aplikací s Azure Active Directory"
[acct_mgmt_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/AccountManagement
[azure_portal]: http://portal.azure.com
[resman_overview]: ../azure-resource-manager/resource-group-overview.md
