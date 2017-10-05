---
title: "Pomocí Azure Active Directory k ověření řešení pro správu Batch | Microsoft Docs"
description: "Aplikace vytvořené s Azure resource manager a poskytovatelem prostředků Batch ověřování s Azure AD."
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
ms.openlocfilehash: 26d4adf4f74f9aacc4cf8cf24be293ebdb4d63c8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="authenticate-batch-management-solutions-with-active-directory"></a><span data-ttu-id="ac5e0-103">Ověření řešení pro správu Batch se službou Active Directory</span><span class="sxs-lookup"><span data-stu-id="ac5e0-103">Authenticate Batch Management solutions with Active Directory</span></span>

<span data-ttu-id="ac5e0-104">Aplikace, které volají služby Azure Batch Management ověření pomocí [Azure Active Directory] [ aad_about] (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="ac5e0-104">Applications that call the Azure Batch Management service authenticate with [Azure Active Directory][aad_about] (Azure AD).</span></span> <span data-ttu-id="ac5e0-105">Azure AD je víceklientské cloudový adresář společnosti Microsoft a služba identity management.</span><span class="sxs-lookup"><span data-stu-id="ac5e0-105">Azure AD is Microsoft’s multi-tenant cloud based directory and identity management service.</span></span> <span data-ttu-id="ac5e0-106">Azure samotné používá Azure AD pro ověření jeho zákazníků, správci služeb a organizační uživatele.</span><span class="sxs-lookup"><span data-stu-id="ac5e0-106">Azure itself uses Azure AD for the authentication of its customers, service administrators, and organizational users.</span></span>

<span data-ttu-id="ac5e0-107">Knihovna rozhraní Batch Management .NET poskytuje typy pro práci s účty Batch, klíče účtu, aplikace a balíčky aplikací.</span><span class="sxs-lookup"><span data-stu-id="ac5e0-107">The Batch Management .NET library exposes types for working with Batch accounts, account keys, applications, and application packages.</span></span> <span data-ttu-id="ac5e0-108">Knihovně rozhraní Batch Management .NET je klient poskytovatel prostředků Azure a je použít v kombinaci s [Azure Resource Manager] [ resman_overview] tyto zdroje spravovat prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="ac5e0-108">The Batch Management .NET library is an Azure resource provider client, and is used together with [Azure Resource Manager][resman_overview] to manage these resources programmatically.</span></span> <span data-ttu-id="ac5e0-109">Azure AD je vyžadovaný k ověření požadavky prostřednictvím všechny klienty poskytovatele prostředků Azure, včetně knihovny Batch Management .NET a prostřednictvím [Azure Resource Manager][resman_overview].</span><span class="sxs-lookup"><span data-stu-id="ac5e0-109">Azure AD is required to authenticate requests made through any Azure resource provider client, including the Batch Management .NET library, and through [Azure Resource Manager][resman_overview].</span></span>

<span data-ttu-id="ac5e0-110">V tomto článku jsme prozkoumat pomocí služby Azure AD k ověření z aplikací, které pomocí knihovny Batch Management .NET.</span><span class="sxs-lookup"><span data-stu-id="ac5e0-110">In this article, we explore using Azure AD to authenticate from applications that use the Batch Management .NET library.</span></span> <span data-ttu-id="ac5e0-111">Ukážeme, jak používat Azure AD k ověření předplatného správce nebo spolusprávce použití integrovaného ověřování.</span><span class="sxs-lookup"><span data-stu-id="ac5e0-111">We show how to use Azure AD to authenticate a subscription administrator or co-administrator, using integrated authentication.</span></span> <span data-ttu-id="ac5e0-112">Používáme [AccountManagment] [ acct_mgmt_sample] ukázkový projekt, dostupná na Githubu, vás provede, Azure AD pomocí knihovny Batch Management .NET.</span><span class="sxs-lookup"><span data-stu-id="ac5e0-112">We use the [AccountManagment][acct_mgmt_sample] sample project, available on GitHub, to walk through using Azure AD with the Batch Management .NET library.</span></span>

<span data-ttu-id="ac5e0-113">Další informace o používání knihovny Batch Management .NET a ukázkové AccountManagement najdete v tématu [Batch Správa účtů a kvót pomocí klientské knihovny správy Batch pro .NET](batch-management-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="ac5e0-113">To learn more about using the Batch Management .NET library and the AccountManagement sample, see [Manage Batch accounts and quotas with the Batch Management client library for .NET](batch-management-dotnet.md).</span></span>

## <a name="register-your-application-with-azure-ad"></a><span data-ttu-id="ac5e0-114">Registrace vaší aplikace s Azure AD</span><span class="sxs-lookup"><span data-stu-id="ac5e0-114">Register your application with Azure AD</span></span>

<span data-ttu-id="ac5e0-115">Azure [Active Directory Authentication Library] [ aad_adal] (ADAL) poskytuje programovací rozhraní do služby Azure AD pro použití v rámci vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="ac5e0-115">The Azure [Active Directory Authentication Library][aad_adal] (ADAL) provides a programmatic interface to Azure AD for use within your applications.</span></span> <span data-ttu-id="ac5e0-116">Pro volání ADAL z aplikace, je nutné zaregistrovat aplikaci v klientovi služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ac5e0-116">To call ADAL from your application, you must register your application in an Azure AD tenant.</span></span> <span data-ttu-id="ac5e0-117">Při registraci vaší aplikace, je třeba zadat Azure AD s informacemi o aplikaci, včetně názvu pro něj v rámci klienta Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ac5e0-117">When you register your application, you supply Azure AD with information about your application, including a name for it within the Azure AD tenant.</span></span> <span data-ttu-id="ac5e0-118">Potom Azure AD poskytuje ID aplikace, které použijete k aplikaci přidružit Azure AD v době běhu.</span><span class="sxs-lookup"><span data-stu-id="ac5e0-118">Azure AD then provides an application ID that you use to associate your application with Azure AD at runtime.</span></span> <span data-ttu-id="ac5e0-119">Další informace o ID aplikace, najdete v části [aplikace a služby hlavní objekty ve službě Azure Active Directory](../active-directory/develop/active-directory-application-objects.md).</span><span class="sxs-lookup"><span data-stu-id="ac5e0-119">To learn more about the application ID, see [Application and service principal objects in Azure Active Directory](../active-directory/develop/active-directory-application-objects.md).</span></span>

<span data-ttu-id="ac5e0-120">Pokud chcete zaregistrovat AccountManagement ukázkovou aplikaci, postupujte podle kroků v [přidání aplikace](../active-directory/develop/active-directory-integrating-applications.md#adding-an-application) kapitoly [integrace aplikací s Azure Active Directory][aad_integrate].</span><span class="sxs-lookup"><span data-stu-id="ac5e0-120">To register the AccountManagement sample application, follow the steps in the [Adding an Application](../active-directory/develop/active-directory-integrating-applications.md#adding-an-application) section in [Integrating applications with Azure Active Directory][aad_integrate].</span></span> <span data-ttu-id="ac5e0-121">Zadejte **nativní klientská aplikace** pro typ aplikace.</span><span class="sxs-lookup"><span data-stu-id="ac5e0-121">Specify **Native Client Application** for the type of application.</span></span> <span data-ttu-id="ac5e0-122">Odvětví standardní OAuth 2.0 identifikátor URI **identifikátor URI pro přesměrování** je `urn:ietf:wg:oauth:2.0:oob`.</span><span class="sxs-lookup"><span data-stu-id="ac5e0-122">The industry standard OAuth 2.0 URI for the **Redirect URI** is `urn:ietf:wg:oauth:2.0:oob`.</span></span> <span data-ttu-id="ac5e0-123">Ale můžete zadat libovolný platný identifikátor URI (například `http://myaccountmanagementsample`) pro **identifikátor URI pro přesměrování**, protože nemusí být skutečné koncový bod:</span><span class="sxs-lookup"><span data-stu-id="ac5e0-123">However, you can specify any valid URI (such as `http://myaccountmanagementsample`) for the **Redirect URI**, as it does not need to be a real endpoint:</span></span>

![](./media/batch-aad-auth-management/app-registration-management-plane.png)

<span data-ttu-id="ac5e0-124">Po dokončení procesu registrace se zobrazí ID aplikace a ID objektu (instanční objekt) uvedené pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ac5e0-124">Once you complete the registration process, you'll see the application ID and the object (service principal) ID listed for your application.</span></span>  

![](./media/batch-aad-auth-management/app-registration-client-id.png)

## <a name="grant-the-azure-resource-manager-api-access-to-your-application"></a><span data-ttu-id="ac5e0-125">Udělení přístupu rozhraní API Správce prostředků Azure do vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="ac5e0-125">Grant the Azure Resource Manager API access to your application</span></span>

<span data-ttu-id="ac5e0-126">Potom budete muset delegovat přístup k vaší aplikaci do rozhraní API Správce prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="ac5e0-126">Next, you'll need to delegate access to your application to the Azure Resource Manager API.</span></span> <span data-ttu-id="ac5e0-127">Identifikátor Azure AD pro rozhraní API Správce prostředků je **Windows Azure Service Management API**.</span><span class="sxs-lookup"><span data-stu-id="ac5e0-127">The Azure AD identifier for the Resource Manager API is **Windows Azure Service Management API**.</span></span>

<span data-ttu-id="ac5e0-128">Pomocí těchto kroků na portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="ac5e0-128">Follow these steps in the Azure portal:</span></span>

1. <span data-ttu-id="ac5e0-129">V levém navigačním podokně portálu Azure, zvolte **více služeb**, klikněte na tlačítko **registrace aplikace**a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="ac5e0-129">In the left-hand navigation pane of the Azure portal, choose **More Services**, click **App Registrations**, and click **Add**.</span></span>
2. <span data-ttu-id="ac5e0-130">Vyhledejte název vaší aplikace v seznamu aplikace registrace:</span><span class="sxs-lookup"><span data-stu-id="ac5e0-130">Search for the name of your application in the list of app registrations:</span></span>

    ![Vyhledávání pro název aplikace](./media/batch-aad-auth-management/search-app-registration.png)

3. <span data-ttu-id="ac5e0-132">Zobrazení **nastavení** okno.</span><span class="sxs-lookup"><span data-stu-id="ac5e0-132">Display the **Settings** blade.</span></span> <span data-ttu-id="ac5e0-133">V **přístup pomocí rozhraní API** vyberte **požadovaná oprávnění**.</span><span class="sxs-lookup"><span data-stu-id="ac5e0-133">In the **API Access** section, select **Required permissions**.</span></span>
4. <span data-ttu-id="ac5e0-134">Klikněte na tlačítko **přidat** přidat nové požadovaná oprávnění.</span><span class="sxs-lookup"><span data-stu-id="ac5e0-134">Click **Add** to add a new required permission.</span></span> 
5. <span data-ttu-id="ac5e0-135">V kroku 1 zadejte **Windows Azure Service Management API**, vyberte v seznamu výsledků toto rozhraní API a klikněte **vyberte** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ac5e0-135">In step 1, enter **Windows Azure Service Management API**, select that API from the list of results, and click the **Select** button.</span></span>
6. <span data-ttu-id="ac5e0-136">V kroku 2, zaškrtněte políčko vedle **Azure přístup k modelu nasazení classic jako uživatele organizaci**a klikněte na **vyberte** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ac5e0-136">In step 2, select the check box next to **Access Azure classic deployment model as organization users**, and click the **Select** button.</span></span>
7. <span data-ttu-id="ac5e0-137">Klikněte **provádí** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ac5e0-137">Click the **Done** button.</span></span>

<span data-ttu-id="ac5e0-138">**Požadovaných oprávnění** okno nyní zobrazuje, že k vaší aplikaci oprávnění ADAL i Resource Manager rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="ac5e0-138">The **Required Permissions** blade now shows that permissions to your application are granted to both the ADAL and Resource Manager APIs.</span></span> <span data-ttu-id="ac5e0-139">Jsou udělena oprávnění pro knihovnu ADAL ve výchozím nastavení při první registraci vaší aplikace s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ac5e0-139">Permissions are granted to ADAL by default when you first register your app with Azure AD.</span></span>

![Delegování oprávnění k rozhraní API služby Azure Resource Manager](./media/batch-aad-auth-management/required-permissions-management-plane.png)

## <a name="azure-ad-endpoints"></a><span data-ttu-id="ac5e0-141">Koncové body Azure AD</span><span class="sxs-lookup"><span data-stu-id="ac5e0-141">Azure AD endpoints</span></span>

<span data-ttu-id="ac5e0-142">K ověření vašich řešení Batch Management s Azure AD, budete potřebovat dva dobře známé koncové body.</span><span class="sxs-lookup"><span data-stu-id="ac5e0-142">To authenticate your Batch Management solutions with Azure AD, you'll need two well-known endpoints.</span></span>

- <span data-ttu-id="ac5e0-143">**Běžné koncového bodu Azure AD** poskytuje obecné pověření shromažďování rozhraní při konkrétní klienta není k dispozici, protože v případě integrované ověřování:</span><span class="sxs-lookup"><span data-stu-id="ac5e0-143">The **Azure AD common endpoint** provides a generic credential gathering interface when a specific tenant is not provided, as in the case of integrated authentication:</span></span>

    `https://login.microsoftonline.com/common`

- <span data-ttu-id="ac5e0-144">**Koncový bod Azure Resource Manager** slouží k získání tokenu pro ověřování požadavků na službu Batch management:</span><span class="sxs-lookup"><span data-stu-id="ac5e0-144">The **Azure Resource Manager endpoint** is used to acquire a token for authenticating requests to the Batch management service:</span></span>

    `https://management.core.windows.net/`

<span data-ttu-id="ac5e0-145">Ukázkovou aplikaci AccountManagement definuje konstanty pro tyto koncové body.</span><span class="sxs-lookup"><span data-stu-id="ac5e0-145">The AccountManagement sample application defines constants for these endpoints.</span></span> <span data-ttu-id="ac5e0-146">Ponechte tyto konstanty beze změny:</span><span class="sxs-lookup"><span data-stu-id="ac5e0-146">Leave these constants unchanged:</span></span>

```csharp
// Azure Active Directory "common" endpoint.
private const string AuthorityUri = "https://login.microsoftonline.com/common";
// Azure Resource Manager endpoint 
private const string ResourceUri = "https://management.core.windows.net/";
```

## <a name="reference-your-application-id"></a><span data-ttu-id="ac5e0-147">Referenční ID vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="ac5e0-147">Reference your application ID</span></span> 

<span data-ttu-id="ac5e0-148">Klientské aplikace používá pro přístup k Azure AD v době běhu ID aplikace (také označované jako ID klienta).</span><span class="sxs-lookup"><span data-stu-id="ac5e0-148">Your client application uses the application ID (also referred to as the client ID) to access Azure AD at runtime.</span></span> <span data-ttu-id="ac5e0-149">Po registraci vaší aplikace na portálu Azure aktualizujte kód a použít zadané ID aplikace Azure AD pro zaregistrovanou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ac5e0-149">Once you've registered your application in the Azure portal, update your code to use the application ID provided by Azure AD for your registered application.</span></span> <span data-ttu-id="ac5e0-150">V ukázkové aplikaci AccountManagement zkopírujte do příslušné konstanta vaše ID aplikace z portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="ac5e0-150">In the AccountManagement sample application, copy your application ID from the Azure portal to the appropriate constant:</span></span>

```csharp
// Specify the unique identifier (the "Client ID") for your application. This is required so that your
// native client application (i.e. this sample) can access the Microsoft Azure AD Graph API. For information
// about registering an application in Azure Active Directory, please see "Adding an Application" here:
// https://azure.microsoft.com/documentation/articles/active-directory-integrating-applications/
private const string ClientId = "<application-id>";
```
<span data-ttu-id="ac5e0-151">Taky zkopírujte identifikátor URI, který jste zadali během procesu registrace přesměrování.</span><span class="sxs-lookup"><span data-stu-id="ac5e0-151">Also copy the redirect URI that you specified during the registration process.</span></span> <span data-ttu-id="ac5e0-152">Identifikátor URI, který jste zadali při registraci aplikace přesměrování musí odpovídat přesměrování, které URI zadaný v kódu.</span><span class="sxs-lookup"><span data-stu-id="ac5e0-152">The redirect URI specified in your code must match the redirect URI that you provided when you registered the application.</span></span>

```csharp
// The URI to which Azure AD will redirect in response to an OAuth 2.0 request. This value is
// specified by you when you register an application with AAD (see ClientId comment). It does not
// need to be a real endpoint, but must be a valid URI (e.g. https://accountmgmtsampleapp).
private const string RedirectUri = "http://myaccountmanagementsample";
```

## <a name="acquire-an-azure-ad-authentication-token"></a><span data-ttu-id="ac5e0-153">Získání tokenu ověřování Azure AD</span><span class="sxs-lookup"><span data-stu-id="ac5e0-153">Acquire an Azure AD authentication token</span></span>

<span data-ttu-id="ac5e0-154">Po registraci ukázka AccountManagement v klientovi Azure AD a aktualizujte zdrojový kód ukázkové s hodnoty, ukázce je připraven k ověření pomocí služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ac5e0-154">After you register the AccountManagement sample in the Azure AD tenant and update the sample source code with your values, the sample is ready to authenticate using Azure AD.</span></span> <span data-ttu-id="ac5e0-155">Při spuštění ukázky, se pokusí získat ověřovací token knihovnu ADAL.</span><span class="sxs-lookup"><span data-stu-id="ac5e0-155">When you run the sample, the ADAL attempts to acquire an authentication token.</span></span> <span data-ttu-id="ac5e0-156">V tomto kroku budete vyzváni k zadání pověření Microsoft:</span><span class="sxs-lookup"><span data-stu-id="ac5e0-156">At this step, it prompts you for your Microsoft credentials:</span></span> 

```csharp
// Obtain an access token using the "common" AAD resource. This allows the application
// to query AAD for information that lies outside the application's tenant (such as for
// querying subscription information in your Azure account).
AuthenticationContext authContext = new AuthenticationContext(AuthorityUri);
AuthenticationResult authResult = authContext.AcquireToken(ResourceUri,
                                                        ClientId,
                                                        new Uri(RedirectUri),
                                                        PromptBehavior.Auto);
```

<span data-ttu-id="ac5e0-157">Po zadání přihlašovacích údajů můžete pokračovat ukázkovou aplikaci pro zasílání požadavků na ověření do služby Batch management.</span><span class="sxs-lookup"><span data-stu-id="ac5e0-157">After you provide your credentials, the sample application can proceed to issue authenticated requests to the Batch management service.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="ac5e0-158">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ac5e0-158">Next steps</span></span>

<span data-ttu-id="ac5e0-159">Další informace o spuštění [AccountManagement ukázkovou aplikaci][acct_mgmt_sample], najdete v části [Batch Správa účtů a kvót pomocí klientské knihovny správy Batch pro .NET](batch-management-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="ac5e0-159">For more information on running the [AccountManagement sample application][acct_mgmt_sample], see [Manage Batch accounts and quotas with the Batch Management client library for .NET](batch-management-dotnet.md).</span></span>

<span data-ttu-id="ac5e0-160">Další informace o Azure AD, najdete v článku [Azure Active Directory dokumentaci](https://docs.microsoft.com/azure/active-directory/).</span><span class="sxs-lookup"><span data-stu-id="ac5e0-160">To learn more about Azure AD, see the [Azure Active Directory Documentation](https://docs.microsoft.com/azure/active-directory/).</span></span> <span data-ttu-id="ac5e0-161">Podrobné příklady znázorňující způsob použití knihovny ADAL jsou k dispozici v [ukázky kódu Azure](https://azure.microsoft.com/resources/samples/?service=active-directory) knihovny.</span><span class="sxs-lookup"><span data-stu-id="ac5e0-161">In-depth examples showing how to use ADAL are available in the [Azure Code Samples](https://azure.microsoft.com/resources/samples/?service=active-directory) library.</span></span>

<span data-ttu-id="ac5e0-162">K ověření aplikace služby Batch pomocí služby Azure AD, najdete v části [řešení služby Batch ověření pomocí služby Active Directory](batch-aad-auth.md).</span><span class="sxs-lookup"><span data-stu-id="ac5e0-162">To authenticate Batch service applications using Azure AD, see [Authenticate Batch service solutions with Active Directory](batch-aad-auth.md).</span></span> 


<span data-ttu-id="ac5e0-163">[aad_about]: ../active-directory/active-directory-whatis.md "Co je Azure Active Directory?"</span><span class="sxs-lookup"><span data-stu-id="ac5e0-163">[aad_about]: ../active-directory/active-directory-whatis.md "What is Azure Active Directory?"</span></span>
[aad_adal]: ../active-directory/active-directory-authentication-libraries.md
<span data-ttu-id="ac5e0-164">[aad_auth_scenarios]: ../active-directory/active-directory-authentication-scenarios.md "Scénáře ověřování pro Azure AD"</span><span class="sxs-lookup"><span data-stu-id="ac5e0-164">[aad_auth_scenarios]: ../active-directory/active-directory-authentication-scenarios.md "Authentication Scenarios for Azure AD"</span></span>
<span data-ttu-id="ac5e0-165">[aad_integrate]: ../active-directory/active-directory-integrating-applications.md "Integrace aplikací s Azure Active Directory"</span><span class="sxs-lookup"><span data-stu-id="ac5e0-165">[aad_integrate]: ../active-directory/active-directory-integrating-applications.md "Integrating Applications with Azure Active Directory"</span></span>
[acct_mgmt_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/AccountManagement
[azure_portal]: http://portal.azure.com
[resman_overview]: ../azure-resource-manager/resource-group-overview.md
