---
title: Principy Manifest aplikace Azure Active Directory | Microsoft Docs
description: "Podrobné pokrytí manifestu aplikace Azure Active Directory, která představuje konfiguraci identity aplikace v klientovi služby Azure AD a se používá k zajištění OAuth autorizace, souhlasu prostředí a další."
services: active-directory
documentationcenter: 
author: sureshja
manager: mbaldwin
editor: 
ms.assetid: 4804f3d4-0ff1-4280-b663-f8f10d54d184
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/20/2017
ms.author: sureshja
ms.custom: aaddev
ms.reviewer: elisol
ms.openlocfilehash: d5e18f41d6eb69ccb7eafaa4de2646c4c38df5e2
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="understanding-the-azure-active-directory-application-manifest"></a><span data-ttu-id="8f4da-103">Principy manifest aplikace Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="8f4da-103">Understanding the Azure Active Directory application manifest</span></span>
<span data-ttu-id="8f4da-104">Aplikace, které se integrují s Azure Active Directory (AD) musí být zaregistrován u klienta služby Azure AD, poskytuje konfiguraci trvalé identity pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="8f4da-104">Applications that integrate with Azure Active Directory (AD) must be registered with an Azure AD tenant, providing a persistent identity configuration for the application.</span></span> <span data-ttu-id="8f4da-105">Tato konfigurace je konzultaci za běhu, povolení scénáře, které umožňují, aby aplikace a externí zprostředkovatel ověřování/autorizace služby prostřednictvím služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8f4da-105">This configuration is consulted at runtime, enabling scenarios that allow an application to outsource and broker authentication/authorization through Azure AD.</span></span> <span data-ttu-id="8f4da-106">Další informace o modelu aplikace Azure AD najdete v tématu [přidání, aktualizace a odebrání aplikace] [ ADD-UPD-RMV-APP] článku.</span><span class="sxs-lookup"><span data-stu-id="8f4da-106">For more information about the Azure AD application model, see the [Adding, Updating, and Removing an Application][ADD-UPD-RMV-APP] article.</span></span>

## <a name="updating-an-applications-identity-configuration"></a><span data-ttu-id="8f4da-107">Aktualizuje se konfigurace identity aplikace</span><span class="sxs-lookup"><span data-stu-id="8f4da-107">Updating an application's identity configuration</span></span>
<span data-ttu-id="8f4da-108">Ve skutečnosti více možností pro nejsou k dispozici aktualizace vlastností konfigurace identity aplikace, které se liší v možnosti a stupňů potíže, včetně následujících:</span><span class="sxs-lookup"><span data-stu-id="8f4da-108">There are actually multiple options available for updating the properties on an application's identity configuration, which vary in capabilities and degrees of difficulty, including the following:</span></span>

* <span data-ttu-id="8f4da-109"> **[Portál Azure] [ AZURE-PORTAL] webové uživatelské rozhraní** umožňuje aktualizovat nejběžnější vlastností aplikace.</span><span class="sxs-lookup"><span data-stu-id="8f4da-109">The **[Azure portal's][AZURE-PORTAL] Web user interface** allows you to update the most common properties of an application.</span></span> <span data-ttu-id="8f4da-110">To je chyba nejrychlejší a minimálně náchylné k chybám způsob aktualizace vlastnosti aplikace, ale není poskytnuta úplný přístup na všechny vlastnosti, jako jsou následující dvě metody.</span><span class="sxs-lookup"><span data-stu-id="8f4da-110">This is the quickest and least error prone way of updating your application's properties, but does not give you full access to all properties, like the next two methods.</span></span>
* <span data-ttu-id="8f4da-111">Pro pokročilejší scénáře, kde je potřeba aktualizovat vlastnosti, které nejsou přístupné na portálu Azure classic, můžete upravit **manifest aplikace**.</span><span class="sxs-lookup"><span data-stu-id="8f4da-111">For more advanced scenarios where you need to update properties that are not exposed in the Azure classic portal, you can modify the **application manifest**.</span></span> <span data-ttu-id="8f4da-112">Toto je fokus v tomto článku a je podrobněji popsána spouštění v další části.</span><span class="sxs-lookup"><span data-stu-id="8f4da-112">This is the focus of this article and is discussed in more detail starting in the next section.</span></span>
* <span data-ttu-id="8f4da-113">Je také možné **zápisu aplikace, která používá [rozhraní Graph API] [ GRAPH-API]**  aktualizovat aplikace, které vyžaduje nejvíce úsilí.</span><span class="sxs-lookup"><span data-stu-id="8f4da-113">It's also possible to **write an application that uses the [Graph API][GRAPH-API]** to update your application, which requires the most effort.</span></span> <span data-ttu-id="8f4da-114">To může být atraktivní možnosti i když, pokud jsou zápis software pro správu, nebo muset aktualizovat vlastnosti aplikace v pravidelných intervalech automatizovaně.</span><span class="sxs-lookup"><span data-stu-id="8f4da-114">This may be an attractive option though, if you are writing management software, or need to update application properties on a regular basis in an automated fashion.</span></span>

## <a name="using-the-application-manifest-to-update-an-applications-identity-configuration"></a><span data-ttu-id="8f4da-115">Aktualizace konfigurace identity aplikace pomocí manifest aplikace</span><span class="sxs-lookup"><span data-stu-id="8f4da-115">Using the application manifest to update an application's identity configuration</span></span>
<span data-ttu-id="8f4da-116">Prostřednictvím [portál Azure][AZURE-PORTAL], konfigurace identity aplikace můžete spravovat aktualizace pomocí editoru manifestu vložené manifest aplikace.</span><span class="sxs-lookup"><span data-stu-id="8f4da-116">Through the [Azure portal][AZURE-PORTAL], you can manage your application's identity configuration by updating the application manifest using the inline manifest editor.</span></span> <span data-ttu-id="8f4da-117">Můžete také stáhnout a nahrát manifest aplikace jako soubor ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="8f4da-117">You can also download and upload the application manifest as a JSON file.</span></span> <span data-ttu-id="8f4da-118">Žádný skutečný soubor je uložen v adresáři.</span><span class="sxs-lookup"><span data-stu-id="8f4da-118">No actual file is stored in the directory.</span></span> <span data-ttu-id="8f4da-119">Manifest aplikace je jenom metody GET protokolu HTTP operace u entity aplikace Azure AD Graph API a nahrávání je operace HTTP PATCH u entity aplikací.</span><span class="sxs-lookup"><span data-stu-id="8f4da-119">The application manifest is merely an HTTP GET operation on the Azure AD Graph API Application entity, and the upload is an HTTP PATCH operation on the Application entity.</span></span>

<span data-ttu-id="8f4da-120">Výsledkem je, aby bylo možné porozumět formátu a vlastnosti manifestu aplikace, budete muset odkazovat na rozhraní Graph API [aplikace entity] [ APPLICATION-ENTITY] dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="8f4da-120">As a result, in order to understand the format and properties of the application manifest, you will need to reference the Graph API [Application entity][APPLICATION-ENTITY] documentation.</span></span> <span data-ttu-id="8f4da-121">Příklady aktualizace, které lze provést, i když nahrávání manifest aplikace:</span><span class="sxs-lookup"><span data-stu-id="8f4da-121">Examples of updates that can be performed though application manifest upload include:</span></span>

* <span data-ttu-id="8f4da-122">**Deklarovat obory oprávnění (oauth2Permissions)** vystavené webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="8f4da-122">**Declare permission scopes (oauth2Permissions)** exposed by your web API.</span></span> <span data-ttu-id="8f4da-123">V tématu "Vystavení webové rozhraní API pro další aplikace" v [integrace aplikací s Azure Active Directory] [ INTEGRATING-APPLICATIONS-AAD] informace o implementaci vystupovat jako jiný uživatel pomocí oauth2Permissions delegovaná oprávnění oboru.</span><span class="sxs-lookup"><span data-stu-id="8f4da-123">See the "Exposing Web APIs to Other Applications" topic in [Integrating Applications with Azure Active Directory][INTEGRATING-APPLICATIONS-AAD] for information on implementing user impersonation using the oauth2Permissions delegated permission scope.</span></span> <span data-ttu-id="8f4da-124">Jak je uvedeno nahoře, vlastností entity aplikací jsou popsané v rozhraní Graph API [Entity a komplexní typ] [ APPLICATION-ENTITY] článku, včetně oauth2Permissions vlastnost, která je kolekce z typ [OAuth2Permission][APPLICATION-ENTITY-OAUTH2-PERMISSION].</span><span class="sxs-lookup"><span data-stu-id="8f4da-124">As mentioned previously, Application entity properties are documented in the Graph API [Entity and Complex Type][APPLICATION-ENTITY] reference article, including the oauth2Permissions property which is a collection of type [OAuth2Permission][APPLICATION-ENTITY-OAUTH2-PERMISSION].</span></span>
* <span data-ttu-id="8f4da-125">**Deklarovat aplikační role (appRoles) vystavené aplikace**.</span><span class="sxs-lookup"><span data-stu-id="8f4da-125">**Declare application roles (appRoles) exposed by your app**.</span></span> <span data-ttu-id="8f4da-126">Vlastnost entity aplikace appRoles je kolekce typu [aplikační role][APPLICATION-ENTITY-APP-ROLE].</span><span class="sxs-lookup"><span data-stu-id="8f4da-126">The Application entity's appRoles property is a collection of type [AppRole][APPLICATION-ENTITY-APP-ROLE].</span></span> <span data-ttu-id="8f4da-127">Najdete v článku [řízení přístupu v cloudových aplikacích pomocí služby Azure AD na základě Role] [ RBAC-CLOUD-APPS-AZUREAD] článku příklad implementace.</span><span class="sxs-lookup"><span data-stu-id="8f4da-127">See the [Role based access control in cloud applications using Azure AD][RBAC-CLOUD-APPS-AZUREAD] article for an implementation example.</span></span>
* <span data-ttu-id="8f4da-128">**Deklarovat známé klienta aplikace (knownClientApplications)**, které umožňují logicky tie souhlasu zadaného klienta aplikace na prostředek nebo webové rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="8f4da-128">**Declare known client applications (knownClientApplications)**, which allow you to logically tie the consent of the specified client application(s) to the resource/web API.</span></span>
* <span data-ttu-id="8f4da-129">**Žádosti o službě Azure AD vydat deklaraci členství ve skupině** pro přihlášeného uživatele (groupMembershipClaims).</span><span class="sxs-lookup"><span data-stu-id="8f4da-129">**Request Azure AD to issue group memberships claim** for the signed in user (groupMembershipClaims).</span></span>  <span data-ttu-id="8f4da-130">Můžete nakonfigurovat také vystavovat deklarace identity o členství role uživatele adresáře.</span><span class="sxs-lookup"><span data-stu-id="8f4da-130">This can also be configured to issue claims about the user's directory roles memberships.</span></span> <span data-ttu-id="8f4da-131">Najdete v článku [autorizace v cloudových aplikací pomocí skupiny AD] [ AAD-GROUPS-FOR-AUTHORIZATION] článku příklad implementace.</span><span class="sxs-lookup"><span data-stu-id="8f4da-131">See the [Authorization in Cloud Applications using AD Groups][AAD-GROUPS-FOR-AUTHORIZATION] article for an implementation example.</span></span>
* <span data-ttu-id="8f4da-132">**Aby vaše aplikace podporují OAuth 2.0 implicitní grant** toků (oauth2AllowImplicitFlow).</span><span class="sxs-lookup"><span data-stu-id="8f4da-132">**Allow your application to support OAuth 2.0 Implicit grant** flows (oauth2AllowImplicitFlow).</span></span> <span data-ttu-id="8f4da-133">Tento typ udělení toku se používá s embedded webové stránky JavaScript nebo jedné stránky aplikace (SPA).</span><span class="sxs-lookup"><span data-stu-id="8f4da-133">This type of grant flow is used with embedded JavaScript web pages or Single Page Applications (SPA).</span></span> <span data-ttu-id="8f4da-134">Další informace o poskytnutí implicitní autorizace najdete v tématu [pochopení OAuth2 implicitní tok v Azure Active Directory poskytování][IMPLICIT-GRANT].</span><span class="sxs-lookup"><span data-stu-id="8f4da-134">For more information on the implicit authorization grant, see [Understanding the OAuth2 implicit grant flow in Azure Active Directory][IMPLICIT-GRANT].</span></span>
* <span data-ttu-id="8f4da-135">**Povolit použití X509 certifikáty jako tajný klíč** (keyCredentials).</span><span class="sxs-lookup"><span data-stu-id="8f4da-135">**Enable use of X509 certificates as the secret key** (keyCredentials).</span></span> <span data-ttu-id="8f4da-136">Najdete v článku [vytvářet aplikace, služby a démon v Office 365] [ O365-SERVICE-DAEMON-APPS] a [– Příručka vývojáře pro ověřování s rozhraním API pro Azure Resource Manager] [ DEV-GUIDE-TO-AUTH-WITH-ARM] články Příklady implementace.</span><span class="sxs-lookup"><span data-stu-id="8f4da-136">See the [Build service and daemon apps in Office 365][O365-SERVICE-DAEMON-APPS] and [Developer’s guide to auth with Azure Resource Manager API][DEV-GUIDE-TO-AUTH-WITH-ARM] articles for implementation examples.</span></span>
* <span data-ttu-id="8f4da-137">**Přidat nový identifikátor ID URI aplikace** pro vaši aplikaci (identifierURIs[]).</span><span class="sxs-lookup"><span data-stu-id="8f4da-137">**Add a new App ID URI** for your application (identifierURIs[]).</span></span> <span data-ttu-id="8f4da-138">Identifikátory URI ID aplikace používají k jedinečné identifikaci aplikace v rámci jeho klienta Azure AD (nebo napříč více klientů Azure AD, pro víc klientů scénáře při kvalifikovaný prostřednictvím ověřené vlastní domény).</span><span class="sxs-lookup"><span data-stu-id="8f4da-138">App ID URIs are used to uniquely identify an application within its Azure AD tenant (or across multiple Azure AD tenants, for multi-tenant scenarios when qualified via verified custom domain).</span></span> <span data-ttu-id="8f4da-139">Používají se při vyžadování oprávnění na prostředek aplikace nebo získání tokenu přístupu pro aplikaci prostředků.</span><span class="sxs-lookup"><span data-stu-id="8f4da-139">They are used when requesting permissions to a resource application, or acquiring an access token for a resource application.</span></span> <span data-ttu-id="8f4da-140">Při aktualizaci tento prvek se stejnou aktualizaci přišla odpovídající instanční objekt servicePrincipalNames [] – kolekce, které je umístěn v domácí klienta aplikace.</span><span class="sxs-lookup"><span data-stu-id="8f4da-140">When you update this element, the same update is made to the corresponding service principal's servicePrincipalNames[] collection, which lives in the application's home tenant.</span></span>

<span data-ttu-id="8f4da-141">Manifest aplikace také poskytuje vhodný způsob, jak sledovat stav registrace vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="8f4da-141">The application manifest also provides a good way to track the state of your application registration.</span></span> <span data-ttu-id="8f4da-142">Protože je k dispozici ve formátu JSON, lze do zdrojového kódu, společně s zdrojový kód aplikace zkontrolovat reprezentace souboru.</span><span class="sxs-lookup"><span data-stu-id="8f4da-142">Because it's available in JSON format, the file representation can be checked into your source control, along with your application's source code.</span></span>

## <a name="step-by-step-example"></a><span data-ttu-id="8f4da-143">Krok příklad krok</span><span class="sxs-lookup"><span data-stu-id="8f4da-143">Step by step example</span></span>
<span data-ttu-id="8f4da-144">Umožňuje teď provede kroky potřebné k aktualizaci konfigurace identity aplikace prostřednictvím manifest aplikace.</span><span class="sxs-lookup"><span data-stu-id="8f4da-144">Now lets walk through the steps required to update your application's identity configuration through the application manifest.</span></span> <span data-ttu-id="8f4da-145">Zvýrazníme jeden z předchozích ukázkách znázorňující na prostředek aplikace deklarovat nový obor oprávnění:</span><span class="sxs-lookup"><span data-stu-id="8f4da-145">We will highlight one of the preceding examples, showing how to declare a new permission scope on a resource application:</span></span>

1. <span data-ttu-id="8f4da-146">Přihlaste se na web [Azure Portal][AZURE-PORTAL].</span><span class="sxs-lookup"><span data-stu-id="8f4da-146">Sign in to the [Azure portal][AZURE-PORTAL].</span></span>
2. <span data-ttu-id="8f4da-147">Po došlo k ověření, vyberte vyberete v pravém horním rohu stránky klientovi Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8f4da-147">After you've authenticated, choose your Azure AD tenant by selecting it from the top right corner of the page.</span></span>
3. <span data-ttu-id="8f4da-148">Vyberte **Azure Active Directory** rozšíření z levého navigačního panelu a klikněte na **registrace aplikace**.</span><span class="sxs-lookup"><span data-stu-id="8f4da-148">Select **Azure Active Directory** extension from the left navigation panel and click on **App Registrations**.</span></span>
4. <span data-ttu-id="8f4da-149">Vyhledávání aplikace, které chcete aktualizovat v seznamu a klikněte na něj.</span><span class="sxs-lookup"><span data-stu-id="8f4da-149">Find the application you want to update in the list and click on it.</span></span>
5. <span data-ttu-id="8f4da-150">Na stránce aplikace klikněte na tlačítko **Manifest** otevřete editor pro vložené manifestu.</span><span class="sxs-lookup"><span data-stu-id="8f4da-150">From the application page, click **Manifest** to open the inline manifest editor.</span></span> 
6. <span data-ttu-id="8f4da-151">Manifest pomocí tohoto editoru lze přímo upravit.</span><span class="sxs-lookup"><span data-stu-id="8f4da-151">You can directly edit the manifest using this editor.</span></span> <span data-ttu-id="8f4da-152">Všimněte si, že manifest dodržuje schéma pro [aplikace entity] [ APPLICATION-ENTITY] jak jsme už zmínili dřív: například za předpokladu, že chceme implementace/zveřejněte nové oprávnění názvem "Employees.Read.All" na našem prostředků aplikace (API), jednoduše přidejte element nové za sekundu kolekce oauth2Permissions ie:</span><span class="sxs-lookup"><span data-stu-id="8f4da-152">Note that the manifest follows the schema for the [Application entity][APPLICATION-ENTITY] as we mentioned earlier: For example, assuming we want to implement/expose a new permission called "Employees.Read.All" on our resource application (API), you would simply add a new/second element to the oauth2Permissions collection, ie:</span></span>
   
        "oauth2Permissions": [
        {
        "adminConsentDescription": "Allow the application to access MyWebApplication on behalf of the signed-in user.",
        "adminConsentDisplayName": "Access MyWebApplication",
        "id": "aade5b35-ea3e-481c-b38d-cba4c78682a0",
        "isEnabled": true,
        "type": "User",
        "userConsentDescription": "Allow the application to access MyWebApplication on your behalf.",
        "userConsentDisplayName": "Access MyWebApplication",
        "value": "user_impersonation"
        },
        {
        "adminConsentDescription": "Allow the application to have read-only access to all Employee data.",
        "adminConsentDisplayName": "Read-only access to Employee records",
        "id": "2b351394-d7a7-4a84-841e-08a6a17e4cb8",
        "isEnabled": true,
        "type": "User",
        "userConsentDescription": "Allow the application to have read-only access to your Employee data.",
        "userConsentDisplayName": "Read-only access to your Employee records",
        "value": "Employees.Read.All"
        }
        ],
   
    <span data-ttu-id="8f4da-153">Položka musí být jedinečný, a proto musíte vygenerovat nový globálně jedinečné ID (GUID) pro `"id"` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="8f4da-153">The entry must be unique, and you must therefore generate a new Globally Unique ID (GUID) for the `"id"` property.</span></span> <span data-ttu-id="8f4da-154">V takovém případě vzhledem k tomu, že jsme zadali `"type": "User"`, toto oprávnění může být souhlas pomocí libovolného účtu Azure AD ověřit klienta v aplikaci prostředků nebo rozhraní API je zaregistrován.</span><span class="sxs-lookup"><span data-stu-id="8f4da-154">In this case, because we specified `"type": "User"`, this permission can be consented to by any account authenticated by the Azure AD tenant in which the resource/API application is registered.</span></span> <span data-ttu-id="8f4da-155">Tím získají klienta aplikace oprávnění k přístupu jménem účtu.</span><span class="sxs-lookup"><span data-stu-id="8f4da-155">This grants the client application permission to access it on the account's behalf.</span></span> <span data-ttu-id="8f4da-156">Popis a zobrazovaný název řetězce se používají během souhlasu a pro zobrazení na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="8f4da-156">The description and display name strings are used during consent and for display in the Azure portal.</span></span>
6. <span data-ttu-id="8f4da-157">Po dokončení aktualizace manifest, klikněte na tlačítko **Uložit** uložit manifest.</span><span class="sxs-lookup"><span data-stu-id="8f4da-157">When you're finished updating the manifest, click **Save** to save the manifest.</span></span>  
   
<span data-ttu-id="8f4da-158">Teď, když je uložen v manifestu, můžete udělit registrovaný klientské aplikaci přístup k nové oprávnění jsme přidali výše.</span><span class="sxs-lookup"><span data-stu-id="8f4da-158">Now that the manifest is saved, you can give a registered client application access to the new permission we added above.</span></span> <span data-ttu-id="8f4da-159">Tentokrát webového uživatelského rozhraní portálu Azure můžete použít neupravujte manifest aplikace klienta:</span><span class="sxs-lookup"><span data-stu-id="8f4da-159">This time you can use the Azure portal's Web UI instead of editing the client application's manifest:</span></span>  

1. <span data-ttu-id="8f4da-160">Přejděte do okna **nastavení** klientské aplikace, do které chcete přidat přístup k novým rozhraním API, klikněte na **požadovaných oprávnění** a zvolte **vybrat rozhraní API**.</span><span class="sxs-lookup"><span data-stu-id="8f4da-160">First go to the **Settings** blade of the client application to which you wish to add access to the new API, click **Required Permissions** and choose **Select an API**.</span></span>
2. <span data-ttu-id="8f4da-161">Pak se zobrazí se seznam registrovaných prostředků aplikace (API) v klientovi.</span><span class="sxs-lookup"><span data-stu-id="8f4da-161">Then you will be presented with the list of registered resource applications (APIs) in the tenant.</span></span> <span data-ttu-id="8f4da-162">Klikněte na aplikaci prostředků, vyberte nebo zadejte název aplikace do vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="8f4da-162">Click the resource application to select it, or type the name of the application the search box.</span></span> <span data-ttu-id="8f4da-163">Když jste najde aplikaci, klikněte na tlačítko **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="8f4da-163">When you've found the application, click **Select**.</span></span>  
3. <span data-ttu-id="8f4da-164">Bude to trvat, abyste **vyberte oprávnění** stránku, která se zobrazí v seznamu oprávnění aplikací a delegovaná oprávnění k dispozici pro aplikaci prostředků.</span><span class="sxs-lookup"><span data-stu-id="8f4da-164">This will take you to the **Select Permissions** page, which will show the list of Application Permissions and Delegated Permissions available for the resource application.</span></span> <span data-ttu-id="8f4da-165">Vyberte nové oprávnění, aby bylo možné přidat do seznamu klienta požadovaná oprávnění.</span><span class="sxs-lookup"><span data-stu-id="8f4da-165">Select the new permission in order to add it to the client's requested list of permissions.</span></span> <span data-ttu-id="8f4da-166">Toto nové oprávnění se uloží v konfiguraci identity klientské aplikace, ve vlastnosti kolekce "requiredResourceAccess".</span><span class="sxs-lookup"><span data-stu-id="8f4da-166">This new permission will be stored in the client application's identity configuration, in the "requiredResourceAccess" collection property.</span></span>


<span data-ttu-id="8f4da-167">A je to.</span><span class="sxs-lookup"><span data-stu-id="8f4da-167">That's it.</span></span> <span data-ttu-id="8f4da-168">Teď vaše aplikace bude spuštěna s použitím jejich novou konfiguraci identity.</span><span class="sxs-lookup"><span data-stu-id="8f4da-168">Now your applications will run using their new identity configuration.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8f4da-169">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8f4da-169">Next steps</span></span>
* <span data-ttu-id="8f4da-170">Další informace o vztah mezi objekty aplikace a instanční objekt aplikace v [aplikace a služby hlavní objekty ve službě Azure AD][AAD-APP-OBJECTS].</span><span class="sxs-lookup"><span data-stu-id="8f4da-170">For more details on the relationship between an application's Application and Service Principal object(s), see [Application and service principal objects in Azure AD][AAD-APP-OBJECTS].</span></span>
* <span data-ttu-id="8f4da-171">Najdete v článku [Glosář vývojáře Azure AD] [ AAD-DEVELOPER-GLOSSARY] definice některých koncepty pro vývojáře Azure Active Directory (AD) jádra.</span><span class="sxs-lookup"><span data-stu-id="8f4da-171">See the [Azure AD developer glossary][AAD-DEVELOPER-GLOSSARY] for definitions of some of the core Azure Active Directory (AD) developer concepts.</span></span>

<span data-ttu-id="8f4da-172">Použijte následující sekci komentáře k poskytnutí zpětné vazby a Pomozte nám vylepšit a utvářejí náš obsah.</span><span class="sxs-lookup"><span data-stu-id="8f4da-172">Please use the comments section below to provide feedback and help us refine and shape our content.</span></span>

<!--article references -->
[AAD-APP-OBJECTS]: active-directory-application-objects.md
[AAD-DEVELOPER-GLOSSARY]: active-directory-dev-glossary.md
[AAD-GROUPS-FOR-AUTHORIZATION]: http://www.dushyantgill.com/blog/2014/12/10/authorization-cloud-applications-using-ad-groups/
[ADD-UPD-RMV-APP]: active-directory-integrating-applications.md
[APPLICATION-ENTITY]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[APPLICATION-ENTITY-APP-ROLE]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#approle-type
[APPLICATION-ENTITY-OAUTH2-PERMISSION]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permission-type
[AZURE-PORTAL]: https://portal.azure.com
[DEV-GUIDE-TO-AUTH-WITH-ARM]: http://www.dushyantgill.com/blog/2015/05/23/developers-guide-to-auth-with-azure-resource-manager-api/
[GRAPH-API]: active-directory-graph-api.md
[IMPLICIT-GRANT]: active-directory-dev-understanding-oauth2-implicit-grant.md
[INTEGRATING-APPLICATIONS-AAD]: https://azure.microsoft.com/documentation/articles/active-directory-integrating-applications/
[O365-PERM-DETAILS]: https://msdn.microsoft.com/office/office365/HowTo/application-manifest
[O365-SERVICE-DAEMON-APPS]: https://msdn.microsoft.com/office/office365/howto/building-service-apps-in-office-365
[RBAC-CLOUD-APPS-AZUREAD]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/
