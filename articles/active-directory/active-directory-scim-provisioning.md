---
title: "Pomocí systému pro správu identit napříč doménami automaticky zřízení uživatelů a skupin ze služby Azure Active Directory k aplikacím | Microsoft Docs"
description: "Azure Active Directory, mohou automaticky poskytovat uživatelé a skupiny do aplikace nebo identity úložiště, který je přední stěnou webovou službou pomocí rozhraní definované v specifikace protokolu SCIM"
services: active-directory
documentationcenter: 
author: asmalser-msft
manager: femila
editor: 
ms.assetid: 4d86f3dc-e2d3-4bde-81a3-4a0e092551c0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/28/2017
ms.author: asmalser
ms.reviewer: asmalser
ms.custom: aaddev;it-pro;oldportal
ms.openlocfilehash: 91978cee88d55c99bcb63c63cdaf01581ae84668
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="using-system-for-cross-domain-identity-management-to-automatically-provision-users-and-groups-from-azure-active-directory-to-applications"></a><span data-ttu-id="55394-103">Pomocí systému pro správu identit napříč doménami pro automatické zřizování uživatelů a skupin ze služby Azure Active Directory k aplikacím</span><span class="sxs-lookup"><span data-stu-id="55394-103">Using System for Cross-Domain Identity Management to automatically provision users and groups from Azure Active Directory to applications</span></span>

## <a name="overview"></a><span data-ttu-id="55394-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="55394-104">Overview</span></span>
<span data-ttu-id="55394-105">Azure Active Directory (Azure AD), mohou automaticky poskytovat uživatelé a skupiny do aplikace nebo identity úložiště, který je přední stěnou pomocí webové služby pomocí rozhraní definované v [systému pro protokol napříč doménami Identity Management (SCIM) 2.0 specifikace](https://tools.ietf.org/html/draft-ietf-scim-api-19).</span><span class="sxs-lookup"><span data-stu-id="55394-105">Azure Active Directory (Azure AD) can automatically provision users and groups to any application or identity store that is fronted by a web service with the interface defined in the [System for Cross-Domain Identity Management (SCIM) 2.0 protocol specification](https://tools.ietf.org/html/draft-ietf-scim-api-19).</span></span> <span data-ttu-id="55394-106">Azure Active Directory může odesílat požadavky na vytvořit, upravit nebo odstranit přiřazené uživatelů a skupin k webové službě.</span><span class="sxs-lookup"><span data-stu-id="55394-106">Azure Active Directory can send requests to create, modify, or delete assigned users and groups to the web service.</span></span> <span data-ttu-id="55394-107">Webová služba může překládat pak tyto požadavky do operací na úložiště identit cíl.</span><span class="sxs-lookup"><span data-stu-id="55394-107">The web service can then translate those requests into operations on the target identity store.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="55394-108">Společnost Microsoft doporučuje při správě služby Azure AD používat [centrum pro správu Azure AD](https://aad.portal.azure.com) na webu Azure Portal namísto používání portálu Azure Classic, na který odkazuje tento článek.</span><span class="sxs-lookup"><span data-stu-id="55394-108">Microsoft recommends that you manage Azure AD using the [Azure AD admin center](https://aad.portal.azure.com) in the Azure portal instead of using the Azure classic portal referenced in this article.</span></span> 



<span data-ttu-id="55394-109">![][0]
*Obrázek 1: Zřizování z Azure Active Directory k úložišti identity prostřednictvím webové služby*</span><span class="sxs-lookup"><span data-stu-id="55394-109">![][0]
*Figure 1: Provisioning from Azure Active Directory to an identity store via a web service*</span></span>

<span data-ttu-id="55394-110">Tato funkce slouží ve spojení s možností "přineste vlastní aplikace" v Azure AD umožňující jednotného přihlašování a automatické zřizování pro aplikace, které poskytují nebo jsou přední stěnou webovou službou SCIM uživatelů.</span><span class="sxs-lookup"><span data-stu-id="55394-110">This capability can be used in conjunction with the “bring your own app” capability in Azure AD to enable single sign-on and automatic user provisioning for applications that provide or are fronted by a SCIM web service.</span></span>

<span data-ttu-id="55394-111">Existují dva případy použití pro pomocí SCIM v Azure Active Directory:</span><span class="sxs-lookup"><span data-stu-id="55394-111">There are two use cases for using SCIM in Azure Active Directory:</span></span>

* <span data-ttu-id="55394-112">**Zřizování uživatelů a skupin na aplikace, které podporují SCIM** aplikace, které podporují SCIM 2.0 a používala tokeny nosičů OAuth pro ověřování pracuje s Azure AD bez konfigurace.</span><span class="sxs-lookup"><span data-stu-id="55394-112">**Provisioning users and groups to applications that support SCIM** Applications that support SCIM 2.0 and use OAuth bearer tokens for authentication works with Azure AD without configuration.</span></span>
* <span data-ttu-id="55394-113">**Vytvoření vlastního řešení zřizování pro aplikace, které podporují jiné zajišťování na základě rozhraní API** pro jiný SCIM aplikace, můžete vytvořit koncový bod SCIM pro převod mezi koncový bod Azure AD SCIM a jakéhokoli rozhraní API podporuje aplikace pro uživatele zřizování.</span><span class="sxs-lookup"><span data-stu-id="55394-113">**Build your own provisioning solution for applications that support other API-based provisioning** For non-SCIM applications, you can create a SCIM endpoint to translate between the Azure AD SCIM endpoint and any API the application supports for user provisioning.</span></span> <span data-ttu-id="55394-114">Můžete vyvíjet koncový bod SCIM, poskytujeme knihovny společné jazykové infrastruktury (CLI) společně s ukázky kódu, které ukazují, jak k poskytování koncový bod SCIM a převede SCIM zprávy.</span><span class="sxs-lookup"><span data-stu-id="55394-114">To help you develop a SCIM endpoint, we provide Common Language Infrastructure (CLI) libraries along with code samples that show you how to do provide a SCIM endpoint and translate SCIM messages.</span></span>  

## <a name="provisioning-users-and-groups-to-applications-that-support-scim"></a><span data-ttu-id="55394-115">Zřizování uživatelů a skupin na aplikace, které podporují SCIM</span><span class="sxs-lookup"><span data-stu-id="55394-115">Provisioning users and groups to applications that support SCIM</span></span>
<span data-ttu-id="55394-116">Azure AD lze nakonfigurovat, aby automaticky přiřazen zřizování uživatelů a skupin k aplikacím, které implementují [systému pro správu identit napříč doménami 2 (SCIM)](https://tools.ietf.org/html/draft-ietf-scim-api-19) webové služby a přijímat tokeny nosičů OAuth pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="55394-116">Azure AD can be configured to automatically provision assigned users and groups to applications that implement a [System for Cross-domain Identity Management 2 (SCIM)](https://tools.ietf.org/html/draft-ietf-scim-api-19) web service and accept OAuth bearer tokens for authentication.</span></span> <span data-ttu-id="55394-117">V rámci specifikace SCIM 2.0 aplikace musí splňovat tyto požadavky:</span><span class="sxs-lookup"><span data-stu-id="55394-117">Within the SCIM 2.0 specification, applications must meet these requirements:</span></span>

* <span data-ttu-id="55394-118">Podporuje vytváření uživatelů nebo skupin, podle části 3.3 SCIM protokolu.</span><span class="sxs-lookup"><span data-stu-id="55394-118">Supports creating users and/or groups, as per section 3.3 of the SCIM protocol.</span></span>  
* <span data-ttu-id="55394-119">Úprava uživatele nebo skupiny s požadavky patch podle části 3.5.2 protokol SCIM podporuje.</span><span class="sxs-lookup"><span data-stu-id="55394-119">Supports modifying users and/or groups with patch requests as per section 3.5.2 of the SCIM protocol.</span></span>  
* <span data-ttu-id="55394-120">Podporuje načítání známých prostředků podle části 3.4.1 SCIM protokolu.</span><span class="sxs-lookup"><span data-stu-id="55394-120">Supports retrieving a known resource as per section 3.4.1 of the SCIM protocol.</span></span>  
* <span data-ttu-id="55394-121">Podporuje dotazování uživatelů nebo skupin, podle části 3.4.2 SCIM protokolu.</span><span class="sxs-lookup"><span data-stu-id="55394-121">Supports querying users and/or groups, as per section 3.4.2 of the SCIM protocol.</span></span>  <span data-ttu-id="55394-122">Ve výchozím nastavení externalId se zadají dotaz uživatelé a skupiny se zadají dotaz displayName.</span><span class="sxs-lookup"><span data-stu-id="55394-122">By default, users are queried by externalId and groups are queried by displayName.</span></span>  
* <span data-ttu-id="55394-123">Dotazování uživatele podle ID a správcem podle části 3.4.2 protokol SCIM podporuje.</span><span class="sxs-lookup"><span data-stu-id="55394-123">Supports querying user by ID and by manager as per section 3.4.2 of the SCIM protocol.</span></span>  
* <span data-ttu-id="55394-124">Podporuje dotazování skupin podle ID a členové podle části 3.4.2 SCIM protokolu.</span><span class="sxs-lookup"><span data-stu-id="55394-124">Supports querying groups by ID and by member as per section 3.4.2 of the SCIM protocol.</span></span>  
* <span data-ttu-id="55394-125">Přijímá tokeny nosičů OAuth pro autorizaci podle části 2.1 SCIM protokolu.</span><span class="sxs-lookup"><span data-stu-id="55394-125">Accepts OAuth bearer tokens for authorization as per section 2.1 of the SCIM protocol.</span></span>

<span data-ttu-id="55394-126">Zeptejte se svého poskytovatele aplikace nebo v dokumentaci poskytovatele aplikace pro příkazy kompatibility s těmito požadavky.</span><span class="sxs-lookup"><span data-stu-id="55394-126">Check with your application provider, or your application provider's documentation for statements of compatibility with these requirements.</span></span>

### <a name="getting-started"></a><span data-ttu-id="55394-127">Začínáme</span><span class="sxs-lookup"><span data-stu-id="55394-127">Getting started</span></span>
<span data-ttu-id="55394-128">Aplikace, které podporují profilem SCIM popsané v tomto článku může být připojen k Azure Active Directory používá funkci "bez Galerie aplikace" v galerii aplikací Azure AD.</span><span class="sxs-lookup"><span data-stu-id="55394-128">Applications that support the SCIM profile described in this article can be connected to Azure Active Directory using the "non-gallery application" feature in the Azure AD application gallery.</span></span> <span data-ttu-id="55394-129">Po připojení Azure AD spustí proces synchronizace každých 20 minut, kde se dotazuje aplikace SCIM koncový bod pro přiřazené uživatele a skupiny a vytvoří nebo upraví je na základě podrobností přiřazení.</span><span class="sxs-lookup"><span data-stu-id="55394-129">Once connected, Azure AD runs a synchronization process every 20 minutes where it queries the application's SCIM endpoint for assigned users and groups, and creates or modifies them according to the assignment details.</span></span>

<span data-ttu-id="55394-130">**Připojení aplikace, která podporuje SCIM:**</span><span class="sxs-lookup"><span data-stu-id="55394-130">**To connect an application that supports SCIM:**</span></span>

1. <span data-ttu-id="55394-131">Přihlaste se k [portálu Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="55394-131">Sign in to [the Azure portal](https://portal.azure.com).</span></span> 
2. <span data-ttu-id="55394-132">Přejděte do ** Azure Active Directory > podnikové aplikace a vyberte **novou aplikaci > všechny > aplikace bez Galerie**.</span><span class="sxs-lookup"><span data-stu-id="55394-132">Browse to **Azure Active Directory > Enterprise Applications, and select **New application > All > Non-gallery application**.</span></span>
3. <span data-ttu-id="55394-133">Zadejte název pro vaši aplikaci a klikněte na tlačítko **přidat** ikonu pro vytvoření objektu aplikace.</span><span class="sxs-lookup"><span data-stu-id="55394-133">Enter a name for your application, and click **Add** icon to create an app object.</span></span>
    
  <span data-ttu-id="55394-134">![][1]
  *Obrázek 2: Galerii aplikací Azure AD*</span><span class="sxs-lookup"><span data-stu-id="55394-134">![][1]
*Figure 2: Azure AD application gallery*</span></span>
    
4. <span data-ttu-id="55394-135">Na obrazovce výsledné vyberte **zřizování** kartě v levém sloupci.</span><span class="sxs-lookup"><span data-stu-id="55394-135">In the resulting screen, select the **Provisioning** tab in the left column.</span></span>
5. <span data-ttu-id="55394-136">V **režimu zřizování** nabídce vyberte možnost **automatické**.</span><span class="sxs-lookup"><span data-stu-id="55394-136">In the **Provisioning Mode** menu, select **Automatic**.</span></span>
    
  <span data-ttu-id="55394-137">![][2]
  *Obrázek 3: Konfigurace zřizování na portálu Azure*</span><span class="sxs-lookup"><span data-stu-id="55394-137">![][2]
*Figure 3: Configuring provisioning in the Azure portal*</span></span>
    
6. <span data-ttu-id="55394-138">V **URL klienta** pole, zadejte adresu URL koncového bodu SCIM aplikace.</span><span class="sxs-lookup"><span data-stu-id="55394-138">In the **Tenant URL** field, enter the URL of the application's SCIM endpoint.</span></span> <span data-ttu-id="55394-139">Příklad: https://api.contoso.com/scim/v2/</span><span class="sxs-lookup"><span data-stu-id="55394-139">Example: https://api.contoso.com/scim/v2/</span></span>
7. <span data-ttu-id="55394-140">Pokud koncový bod SCIM vyžaduje tokenu nosiče OAuth z vystavitele než Azure AD, zkopírujte do nepovinný požadovaný token nosiče OAuth **tajný klíč tokenu** pole.</span><span class="sxs-lookup"><span data-stu-id="55394-140">If the SCIM endpoint requires an OAuth bearer token from an issuer other than Azure AD, then copy the required OAuth bearer token into the optional **Secret Token** field.</span></span> <span data-ttu-id="55394-141">Pokud toto pole je prázdné, Azure AD zahrnuty tokenu nosiče OAuth, který je vydán z Azure AD s každou žádostí.</span><span class="sxs-lookup"><span data-stu-id="55394-141">If this field is left blank, then Azure AD included an OAuth bearer token issued from Azure AD with each request.</span></span> <span data-ttu-id="55394-142">Aplikace, které používají Azure AD jako zprostředkovatele identity můžete ověřit tento Azure AD-vydán token.</span><span class="sxs-lookup"><span data-stu-id="55394-142">Apps that use Azure AD as an identity provider can validate this Azure AD -issued token.</span></span>
8. <span data-ttu-id="55394-143">Klikněte **Test připojení** tlačítko tak, aby měl Azure Active Directory, pokusí se připojit ke koncovému bodu SCIM.</span><span class="sxs-lookup"><span data-stu-id="55394-143">Click the **Test Connection** button to have Azure Active Directory attempt to connect to the SCIM endpoint.</span></span> <span data-ttu-id="55394-144">Pokud pokusy o selhání, zobrazí se informace o chybě.</span><span class="sxs-lookup"><span data-stu-id="55394-144">If the attempts fail, error information is displayed.</span></span>  
9. <span data-ttu-id="55394-145">Pokud pokusy o připojení k úspěšné aplikaci, pak klikněte na tlačítko **Uložit** uložit přihlašovací údaje správce.</span><span class="sxs-lookup"><span data-stu-id="55394-145">If the attempts to connect to the application succeed, then click **Save** to save the admin credentials.</span></span>
10. <span data-ttu-id="55394-146">V **mapování** část, existují dvě sady vybrat mapování atributů: jeden pro uživatelské objekty a jeden pro objekty skupiny.</span><span class="sxs-lookup"><span data-stu-id="55394-146">In the **Mappings** section, there are two selectable sets of attribute mappings: one for user objects and one for group objects.</span></span> <span data-ttu-id="55394-147">Vyberte každé z nich a zkontrolujte atributy, které jsou synchronizované z Azure Active Directory do vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="55394-147">Select each one to review the attributes that are synchronized from Azure Active Directory to your app.</span></span> <span data-ttu-id="55394-148">Atributy vybrán jako **párování** vlastnosti jsou slouží k přiřazení uživatelů a skupin v aplikaci pro operace aktualizace.</span><span class="sxs-lookup"><span data-stu-id="55394-148">The attributes selected as **Matching** properties are used to match the users and groups in your app for update operations.</span></span> <span data-ttu-id="55394-149">Kliknutím na tlačítko Uložit potvrzení změny.</span><span class="sxs-lookup"><span data-stu-id="55394-149">Select the Save button to commit any changes.</span></span>

    >[!NOTE]
    ><span data-ttu-id="55394-150">Volitelně můžete zakázat synchronizaci objektů skupiny zakázáním "skupiny" mapování.</span><span class="sxs-lookup"><span data-stu-id="55394-150">You can optionally disable syncing of group objects by disabling the "groups" mapping.</span></span> 

11. <span data-ttu-id="55394-151">V části **nastavení**, **oboru** pole definuje, které uživatele nebo skupiny jsou synchronizovány.</span><span class="sxs-lookup"><span data-stu-id="55394-151">Under **Settings**, the **Scope** field defines which users and or groups are synchronized.</span></span> <span data-ttu-id="55394-152">Výběr "Synchronizace přiřadit pouze uživatelé a skupiny" (doporučeno) se synchronizují pouze uživatelé a skupiny přiřazené v **uživatelů a skupin** kartě.</span><span class="sxs-lookup"><span data-stu-id="55394-152">Selecting "Sync only assigned users and groups" (recommended) will only sync users and groups assigned in the **Users and groups** tab.</span></span>
12. <span data-ttu-id="55394-153">Po dokončení konfiguraci změnit **Stav zřizování** k **na**.</span><span class="sxs-lookup"><span data-stu-id="55394-153">Once your configuration is complete, change the **Provisioning Status** to **On**.</span></span>
13. <span data-ttu-id="55394-154">Klikněte na tlačítko **Uložit** zahájíte zřizování služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="55394-154">Click **Save** to start the Azure AD provisioning service.</span></span> 
14. <span data-ttu-id="55394-155">Pokud synchronizaci přiřadit pouze uživatelé a skupiny (doporučeno), je nutné vybrat **uživatelů a skupin** a přiřaďte uživatele a skupiny, které chcete synchronizovat.</span><span class="sxs-lookup"><span data-stu-id="55394-155">If syncing only assigned users and groups (recommended), be sure to select the **Users and groups** tab and assign the users and/or groups you wish to sync.</span></span>

<span data-ttu-id="55394-156">Po dokončení počáteční synchronizace se spustil, můžete **protokoly auditu** a sledovat postup, který zobrazuje všechny akce prováděné při zřizování služby ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="55394-156">Once the initial synchronization has started, you can use the **Audit logs** tab to monitor progress, which shows all actions performed by the provisioning service on your app.</span></span> <span data-ttu-id="55394-157">Další informace o tom, jak číst zřizování protokoly služby Azure AD najdete v tématu [zprávy o zřizování účtu automatické uživatele](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span><span class="sxs-lookup"><span data-stu-id="55394-157">For more information on how to read the Azure AD provisioning logs, see [Reporting on automatic user account provisioning](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span></span>

>[!NOTE]
><span data-ttu-id="55394-158">Počáteční synchronizace trvá déle než následné synchronizace, ke kterým dochází přibližně každých 20 minut, dokud se službou provést.</span><span class="sxs-lookup"><span data-stu-id="55394-158">The initial sync takes longer to perform than subsequent syncs, which occur approximately every 20 minutes as long as the service is running.</span></span> 


## <a name="building-your-own-provisioning-solution-for-any-application"></a><span data-ttu-id="55394-159">Vytváření vlastní zřizování řešení pro všechny aplikace</span><span class="sxs-lookup"><span data-stu-id="55394-159">Building your own provisioning solution for any application</span></span>
<span data-ttu-id="55394-160">Vytvořením SCIM webová služba, která rozhraní s Azure Active Directory, můžete povolit jeden uživatel přihlašování a automatické zřizování prakticky jakékoli aplikace, která poskytuje zřizování rozhraní API REST nebo SOAP uživatelů.</span><span class="sxs-lookup"><span data-stu-id="55394-160">By creating a SCIM web service that interfaces with Azure Active Directory, you can enable single sign-on and automatic user provisioning for virtually any application that provides a REST or SOAP user provisioning API.</span></span>

<span data-ttu-id="55394-161">Zde je, jak to funguje:</span><span class="sxs-lookup"><span data-stu-id="55394-161">Here’s how it works:</span></span>

1. <span data-ttu-id="55394-162">Azure AD poskytuje společné jazykové infrastruktury knihovny s názvem [Microsoft.SystemForCrossDomainIdentityManagement](https://www.nuget.org/packages/Microsoft.SystemForCrossDomainIdentityManagement/).</span><span class="sxs-lookup"><span data-stu-id="55394-162">Azure AD provides a common language infrastructure library named [Microsoft.SystemForCrossDomainIdentityManagement](https://www.nuget.org/packages/Microsoft.SystemForCrossDomainIdentityManagement/).</span></span> <span data-ttu-id="55394-163">Systémoví integrátoři a vývojáři mohou použít tuto knihovnu vytvořit a nasadit koncový bod na základě SCIM webové služby lze připojit k libovolné aplikace úložiště identit Azure AD.</span><span class="sxs-lookup"><span data-stu-id="55394-163">System integrators and developers can use this library to create and deploy a SCIM-based web service endpoint capable of connecting Azure AD to any application’s identity store.</span></span>
2. <span data-ttu-id="55394-164">Mapování se implementují ve webové službě k mapování schéma standardizované uživatele na schéma uživatele a protokol požadované aplikací.</span><span class="sxs-lookup"><span data-stu-id="55394-164">Mappings are implemented in the web service to map the standardized user schema to the user schema and protocol required by the application.</span></span>
3. <span data-ttu-id="55394-165">Adresa URL koncového bodu je zaregistrován ve službě Azure AD jako součást vlastní aplikaci v galerii aplikací.</span><span class="sxs-lookup"><span data-stu-id="55394-165">The endpoint URL is registered in Azure AD as part of a custom application in the application gallery.</span></span>
4. <span data-ttu-id="55394-166">Uživatelé a skupiny přiřazené k této aplikaci ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="55394-166">Users and groups are assigned to this application in Azure AD.</span></span> <span data-ttu-id="55394-167">Po přiřazení jsou vloženy do fronty na synchronizaci do cílové aplikace.</span><span class="sxs-lookup"><span data-stu-id="55394-167">Upon assignment, they are put into a queue to be synchronized to the target application.</span></span> <span data-ttu-id="55394-168">Proces synchronizace zpracování fronty spouští každých 20 minut.</span><span class="sxs-lookup"><span data-stu-id="55394-168">The synchronization process handling the queue runs every 20 minutes.</span></span>

### <a name="code-samples"></a><span data-ttu-id="55394-169">Ukázky kódů</span><span class="sxs-lookup"><span data-stu-id="55394-169">Code Samples</span></span>
<span data-ttu-id="55394-170">Chcete-li tento jednodušší, proces sadu [ukázky kódu jsou](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master) jsou k dispozici, vytvořit koncový bod webové služby SCIM a ukázka automatické zřizování.</span><span class="sxs-lookup"><span data-stu-id="55394-170">To make this process easier, a set of [code samples](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master) are provided that create a SCIM web service endpoint and demonstrate automatic provisioning.</span></span> <span data-ttu-id="55394-171">Jeden ukázka je zprostředkovatele, který udržuje soubor s řádky představující uživatele a skupiny hodnot oddělených čárkami.</span><span class="sxs-lookup"><span data-stu-id="55394-171">One sample is of a provider that maintains a file with rows of comma-separated values representing users and groups.</span></span>  <span data-ttu-id="55394-172">Druhá je zprostředkovatele, který funguje ve službě Amazon Web Services identita a správa přístupu.</span><span class="sxs-lookup"><span data-stu-id="55394-172">The other is of a provider that operates on the Amazon Web Services Identity and Access Management service.</span></span>  

<span data-ttu-id="55394-173">**Požadavky**</span><span class="sxs-lookup"><span data-stu-id="55394-173">**Prerequisites**</span></span>

* <span data-ttu-id="55394-174">Visual Studio 2013 nebo novější</span><span class="sxs-lookup"><span data-stu-id="55394-174">Visual Studio 2013 or later</span></span>
* [<span data-ttu-id="55394-175">Azure SDK pro .NET</span><span class="sxs-lookup"><span data-stu-id="55394-175">Azure SDK for .NET</span></span>](https://azure.microsoft.com/downloads/)
* <span data-ttu-id="55394-176">Windows počítač, který podporuje technologii ASP.NET 4.5 má být použit jako SCIM koncový bod.</span><span class="sxs-lookup"><span data-stu-id="55394-176">Windows machine that supports the ASP.NET framework 4.5 to be used as the SCIM endpoint.</span></span> <span data-ttu-id="55394-177">Tento počítač musí být přístupné z cloudu</span><span class="sxs-lookup"><span data-stu-id="55394-177">This machine must be accessible from the cloud</span></span>
* [<span data-ttu-id="55394-178">Předplatné Azure zkušební nebo licencovanou verzi Azure AD Premium</span><span class="sxs-lookup"><span data-stu-id="55394-178">An Azure subscription with a trial or licensed version of Azure AD Premium</span></span>](https://azure.microsoft.com/services/active-directory/)
* <span data-ttu-id="55394-179">Ukázka Amazon AWS vyžaduje knihovny z [AWS nástrojů pro Visual Studio](http://docs.aws.amazon.com/AWSToolkitVS/latest/UserGuide/tkv_setup.html).</span><span class="sxs-lookup"><span data-stu-id="55394-179">The Amazon AWS sample requires libraries from the [AWS Toolkit for Visual Studio](http://docs.aws.amazon.com/AWSToolkitVS/latest/UserGuide/tkv_setup.html).</span></span> <span data-ttu-id="55394-180">Další informace naleznete v souboru README součástí vzorku.</span><span class="sxs-lookup"><span data-stu-id="55394-180">For more information, see the README file included with the sample.</span></span>

### <a name="getting-started"></a><span data-ttu-id="55394-181">Začínáme</span><span class="sxs-lookup"><span data-stu-id="55394-181">Getting Started</span></span>
<span data-ttu-id="55394-182">Nejjednodušší způsob, jak implementovat SCIM koncový bod, který může přijmout zřizování požadavky z Azure AD je sestavení a nasazení ukázka kódu, který produkuje zřízené uživatele do souboru s oddělovači (CSV).</span><span class="sxs-lookup"><span data-stu-id="55394-182">The easiest way to implement a SCIM endpoint that can accept provisioning requests from Azure AD is to build and deploy the code sample that outputs the provisioned users to a comma-separated value (CSV) file.</span></span>

<span data-ttu-id="55394-183">**Pokud chcete vytvořit koncový bod ukázka SCIM:**</span><span class="sxs-lookup"><span data-stu-id="55394-183">**To create a sample SCIM endpoint:**</span></span>

1. <span data-ttu-id="55394-184">Stáhněte si balíček ukázkový kód v [https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master)</span><span class="sxs-lookup"><span data-stu-id="55394-184">Download the code sample package at [https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master)</span></span>
2. <span data-ttu-id="55394-185">Rozbalte balíček a umístěte ji na počítač se systémem Windows v umístění, jako je například C:\AzureAD-BYOA-Provisioning-Samples\.</span><span class="sxs-lookup"><span data-stu-id="55394-185">Unzip the package and place it on your Windows machine at a location such as C:\AzureAD-BYOA-Provisioning-Samples\.</span></span>
3. <span data-ttu-id="55394-186">V této složce spusťte FileProvisioningAgent řešení v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="55394-186">In this folder, launch the FileProvisioningAgent solution in Visual Studio.</span></span>
4. <span data-ttu-id="55394-187">Vyberte **nástroje > Správce balíčků knihoven > Konzola správce balíčků**a spusťte následující příkazy pro projekt FileProvisioningAgent se vyřešit odkazy na řešení:</span><span class="sxs-lookup"><span data-stu-id="55394-187">Select **Tools > Library Package Manager > Package Manager Console**, and execute the following commands for the FileProvisioningAgent project to resolve the solution references:</span></span>
  ```` 
   Install-Package Microsoft.SystemForCrossDomainIdentityManagement
   Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
   Install-Package Microsoft.Owin.Diagnostics
   Install-Package Microsoft.Owin.Host.SystemWeb
  ````
5. <span data-ttu-id="55394-188">Sestavení projektu FileProvisioningAgent.</span><span class="sxs-lookup"><span data-stu-id="55394-188">Build the FileProvisioningAgent project.</span></span>
6. <span data-ttu-id="55394-189">Spuštění aplikace příkazového řádku v systému Windows (jako správce) a použít **cd** příkazu změňte adresář na vaše **\AzureAD-BYOA-Provisioning-Samples\ProvisioningAgent\bin\Debug** složka.</span><span class="sxs-lookup"><span data-stu-id="55394-189">Launch the Command Prompt application in Windows (as an Administrator), and use the **cd** command to change the directory to your **\AzureAD-BYOA-Provisioning-Samples\ProvisioningAgent\bin\Debug** folder.</span></span>
7. <span data-ttu-id="55394-190">Spusťte následující příkaz, nahraďte IP adresy nebo domény název počítače Windows < adresa >:</span><span class="sxs-lookup"><span data-stu-id="55394-190">Run the following command, replacing <ip-address> with the IP address or domain name of the Windows machine:</span></span>
  ````   
   FileAgnt.exe http://<ip-address>:9000 TargetFile.csv
  ````
8. <span data-ttu-id="55394-191">V systému Windows v rámci **nastavení systému Windows > síť a Internet nastavení**, vyberte **brány Windows Firewall > Upřesnit nastavení**a vytvořit **příchozí pravidlo** , umožňuje příchozí přístup k portu 9000.</span><span class="sxs-lookup"><span data-stu-id="55394-191">In Windows under **Windows Settings > Network & Internet Settings**, select the **Windows Firewall > Advanced Settings**, and create an **Inbound Rule** that allows inbound access to port 9000.</span></span>
9. <span data-ttu-id="55394-192">Pokud je počítač Windows za směrovačem, je potřeba nakonfigurovat k provedení přístup překlad mezi její port 9000, který má přístup k Internetu a port 9000 na počítači Windows směrovači.</span><span class="sxs-lookup"><span data-stu-id="55394-192">If the Windows machine is behind a router, the router needs to be configured to perform Network Access Translation between its port 9000 that is exposed to the internet, and port 9000 on the Windows machine.</span></span> <span data-ttu-id="55394-193">To je potřeba pro Azure AD pro přístup k tomuto koncovému bodu v cloudu.</span><span class="sxs-lookup"><span data-stu-id="55394-193">This is required for Azure AD to be able to access this endpoint in the cloud.</span></span>

<span data-ttu-id="55394-194">**Postup registrace koncového bodu SCIM ukázka ve službě Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="55394-194">**To register the sample SCIM endpoint in Azure AD:**</span></span>

1. <span data-ttu-id="55394-195">Přihlaste se k [portálu Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="55394-195">Sign in to [the Azure portal](https://portal.azure.com).</span></span> 
2. <span data-ttu-id="55394-196">Přejděte do ** Azure Active Directory > podnikové aplikace a vyberte **novou aplikaci > všechny > aplikace bez Galerie**.</span><span class="sxs-lookup"><span data-stu-id="55394-196">Browse to **Azure Active Directory > Enterprise Applications, and select **New application > All > Non-gallery application**.</span></span>
3. <span data-ttu-id="55394-197">Zadejte název pro vaši aplikaci a klikněte na tlačítko **přidat** ikonu pro vytvoření objektu aplikace.</span><span class="sxs-lookup"><span data-stu-id="55394-197">Enter a name for your application, and click **Add** icon to create an app object.</span></span> <span data-ttu-id="55394-198">Objekt aplikace vytvořený je určený k reprezentaci cílové aplikaci jste by zajišťování, které a implementace jednotné přihlašování pro a nikoli pouze SCIM koncového bodu.</span><span class="sxs-lookup"><span data-stu-id="55394-198">The application object created is intended to represent the target app you would be provisioning to and implementing single sign-on for, and not just the SCIM endpoint.</span></span>
4. <span data-ttu-id="55394-199">Na obrazovce výsledné vyberte **zřizování** kartě v levém sloupci.</span><span class="sxs-lookup"><span data-stu-id="55394-199">In the resulting screen, select the **Provisioning** tab in the left column.</span></span>
5. <span data-ttu-id="55394-200">V **režimu zřizování** nabídce vyberte možnost **automatické**.</span><span class="sxs-lookup"><span data-stu-id="55394-200">In the **Provisioning Mode** menu, select **Automatic**.</span></span>
    
  <span data-ttu-id="55394-201">![][2]
  *Obrázek 4: Konfigurace zřizování na portálu Azure*</span><span class="sxs-lookup"><span data-stu-id="55394-201">![][2]
*Figure 4: Configuring provisioning in the Azure portal*</span></span>
    
6. <span data-ttu-id="55394-202">V **URL klienta** pole, zadejte adresu URL a port váš koncový bod SCIM vystavené Internetu.</span><span class="sxs-lookup"><span data-stu-id="55394-202">In the **Tenant URL** field, enter the internet-exposed URL and port of your SCIM endpoint.</span></span> <span data-ttu-id="55394-203">To by něco jako http://testmachine.contoso.com:9000 nebo http://<ip-address>:9000/, kde < adresa > je Internetu zveřejněné IP adresu.</span><span class="sxs-lookup"><span data-stu-id="55394-203">This would be something like http://testmachine.contoso.com:9000 or http://<ip-address>:9000/, where <ip-address> is the internet exposed IP address.</span></span>  
7. <span data-ttu-id="55394-204">Pokud koncový bod SCIM vyžaduje tokenu nosiče OAuth z vystavitele než Azure AD, zkopírujte do nepovinný požadovaný token nosiče OAuth **tajný klíč tokenu** pole.</span><span class="sxs-lookup"><span data-stu-id="55394-204">If the SCIM endpoint requires an OAuth bearer token from an issuer other than Azure AD, then copy the required OAuth bearer token into the optional **Secret Token** field.</span></span> <span data-ttu-id="55394-205">Pokud toto pole je prázdné, bude obsahovat Azure AD tokenu nosiče OAuth, který je vydán z Azure AD s každou žádostí.</span><span class="sxs-lookup"><span data-stu-id="55394-205">If this field is left blank, then Azure AD will include an OAuth bearer token issued from Azure AD with each request.</span></span> <span data-ttu-id="55394-206">Aplikace, které používají Azure AD jako zprostředkovatele identity můžete ověřit tento Azure AD-vydán token.</span><span class="sxs-lookup"><span data-stu-id="55394-206">Apps that use Azure AD as an identity provider can validate this Azure AD -issued token.</span></span>
8. <span data-ttu-id="55394-207">Klikněte **Test připojení** tlačítko tak, aby měl Azure Active Directory, pokusí se připojit ke koncovému bodu SCIM.</span><span class="sxs-lookup"><span data-stu-id="55394-207">Click the **Test Connection** button to have Azure Active Directory attempt to connect to the SCIM endpoint.</span></span> <span data-ttu-id="55394-208">Pokud pokusy o selhání, zobrazí se informace o chybě.</span><span class="sxs-lookup"><span data-stu-id="55394-208">If the attempts fail, error information is displayed.</span></span>  
9. <span data-ttu-id="55394-209">Pokud pokusy o připojení k úspěšné aplikaci, pak klikněte na tlačítko **Uložit** uložit přihlašovací údaje správce.</span><span class="sxs-lookup"><span data-stu-id="55394-209">If the attempts to connect to the application succeed, then click **Save** to save the admin credentials.</span></span>
10. <span data-ttu-id="55394-210">V **mapování** část, existují dvě sady vybrat mapování atributů: jeden pro uživatelské objekty a jeden pro objekty skupiny.</span><span class="sxs-lookup"><span data-stu-id="55394-210">In the **Mappings** section, there are two selectable sets of attribute mappings: one for user objects and one for group objects.</span></span> <span data-ttu-id="55394-211">Vyberte každé z nich a zkontrolujte atributy, které jsou synchronizované z Azure Active Directory do vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="55394-211">Select each one to review the attributes that are synchronized from Azure Active Directory to your app.</span></span> <span data-ttu-id="55394-212">Atributy vybrán jako **párování** vlastnosti jsou slouží k přiřazení uživatelů a skupin v aplikaci pro operace aktualizace.</span><span class="sxs-lookup"><span data-stu-id="55394-212">The attributes selected as **Matching** properties are used to match the users and groups in your app for update operations.</span></span> <span data-ttu-id="55394-213">Kliknutím na tlačítko Uložit potvrzení změny.</span><span class="sxs-lookup"><span data-stu-id="55394-213">Select the Save button to commit any changes.</span></span>
11. <span data-ttu-id="55394-214">V části **nastavení**, **oboru** pole definuje, které uživatele nebo skupiny jsou synchronizovány.</span><span class="sxs-lookup"><span data-stu-id="55394-214">Under **Settings**, the **Scope** field defines which users and or groups are synchronized.</span></span> <span data-ttu-id="55394-215">Výběr "Synchronizace přiřadit pouze uživatelé a skupiny" (doporučeno) se synchronizují pouze uživatelé a skupiny přiřazené v **uživatelů a skupin** kartě.</span><span class="sxs-lookup"><span data-stu-id="55394-215">Selecting "Sync only assigned users and groups" (recommended) will only sync users and groups assigned in the **Users and groups** tab.</span></span>
12. <span data-ttu-id="55394-216">Po dokončení konfiguraci změnit **Stav zřizování** k **na**.</span><span class="sxs-lookup"><span data-stu-id="55394-216">Once your configuration is complete, change the **Provisioning Status** to **On**.</span></span>
13. <span data-ttu-id="55394-217">Klikněte na tlačítko **Uložit** zahájíte zřizování služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="55394-217">Click **Save** to start the Azure AD provisioning service.</span></span> 
14. <span data-ttu-id="55394-218">Pokud synchronizaci přiřadit pouze uživatelé a skupiny (doporučeno), je nutné vybrat **uživatelů a skupin** a přiřaďte uživatele a skupiny, které chcete synchronizovat.</span><span class="sxs-lookup"><span data-stu-id="55394-218">If syncing only assigned users and groups (recommended), be sure to select the **Users and groups** tab and assign the users and/or groups you wish to sync.</span></span>

<span data-ttu-id="55394-219">Po dokončení počáteční synchronizace se spustil, můžete **protokoly auditu** a sledovat postup, který zobrazuje všechny akce prováděné při zřizování služby ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="55394-219">Once the initial synchronization has started, you can use the **Audit logs** tab to monitor progress, which shows all actions performed by the provisioning service on your app.</span></span> <span data-ttu-id="55394-220">Další informace o tom, jak číst zřizování protokoly služby Azure AD najdete v tématu [zprávy o zřizování účtu automatické uživatele](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span><span class="sxs-lookup"><span data-stu-id="55394-220">For more information on how to read the Azure AD provisioning logs, see [Reporting on automatic user account provisioning](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span></span>

<span data-ttu-id="55394-221">V posledním kroku ověření ukázce je k otevření souboru TargetFile.csv ve složce \AzureAD-BYOA-Provisioning-Samples\ProvisioningAgent\bin\Debug na počítač se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="55394-221">The final step in verifying the sample is to open the TargetFile.csv file in the \AzureAD-BYOA-Provisioning-Samples\ProvisioningAgent\bin\Debug folder on your Windows machine.</span></span> <span data-ttu-id="55394-222">Po spuštění procesu zřizování, tento soubor zobrazuje podrobnosti o veškerém přiřazené a zřízení uživatelů a skupin.</span><span class="sxs-lookup"><span data-stu-id="55394-222">Once the provisioning process is run, this file shows the details of all assigned and provisioned users and groups.</span></span>

### <a name="development-libraries"></a><span data-ttu-id="55394-223">Vývojářské knihovny</span><span class="sxs-lookup"><span data-stu-id="55394-223">Development libraries</span></span>
<span data-ttu-id="55394-224">K vývoji vlastní webová služba, která odpovídá specifikaci SCIM, nejdřív seznámíte se s následující knihovny poskytované Microsoft pomoci urychlit proces vývoje:</span><span class="sxs-lookup"><span data-stu-id="55394-224">To develop your own web service that conforms to the SCIM specification, first familiarize yourself with the following libraries provided by Microsoft to help accelerate the development process:</span></span> 

1. <span data-ttu-id="55394-225">Společné jazykové infrastruktury (CLI) knihovny jsou nabízena pro použití s jazyky na základě této infrastruktury, jako je například C#.</span><span class="sxs-lookup"><span data-stu-id="55394-225">Common Language Infrastructure (CLI) libraries are offered for use with languages based on that infrastructure, such as C#.</span></span> <span data-ttu-id="55394-226">Jedna z těchto knihoven [Microsoft.SystemForCrossDomainIdentityManagement.Service](https://www.nuget.org/packages/Microsoft.SystemForCrossDomainIdentityManagement/), deklaruje rozhraní, Microsoft.SystemForCrossDomainIdentityManagement.IProvider, viz následující obrázek: A vývojáře pomocí knihovny by implementovat dané rozhraní s třídu, která může označovat, obecně jako zprostředkovatel.</span><span class="sxs-lookup"><span data-stu-id="55394-226">One of those libraries, [Microsoft.SystemForCrossDomainIdentityManagement.Service](https://www.nuget.org/packages/Microsoft.SystemForCrossDomainIdentityManagement/), declares an interface, Microsoft.SystemForCrossDomainIdentityManagement.IProvider, shown in the following illustration:  A developer using the libraries would implement that interface with a class that may be referred to, generically, as a provider.</span></span> <span data-ttu-id="55394-227">Knihovny povolte vývojáři nasazení webové služby, který splňuje specifikaci SCIM.</span><span class="sxs-lookup"><span data-stu-id="55394-227">The libraries enable the developer to deploy a web service that conforms to the SCIM specification.</span></span> <span data-ttu-id="55394-228">Webová služba může být buď hostovaný v rámci služby IIS nebo libovolného spustitelného souboru Common Language Infrastructure sestavení.</span><span class="sxs-lookup"><span data-stu-id="55394-228">The web service can be either hosted within Internet Information Services, or any executable Common Language Infrastructure assembly.</span></span> <span data-ttu-id="55394-229">Žádost je přeložit na volání metody poskytovatele, které by být naprogramovaný tak vývojáře k provozu na některé úložiště identit.</span><span class="sxs-lookup"><span data-stu-id="55394-229">Request is translated into calls to the provider’s methods, which would be programmed by the developer to operate on some identity store.</span></span>
  
  ![][3]
  
2. <span data-ttu-id="55394-230">[Obslužné rutiny trasy Express](http://expressjs.com/guide/routing.html) jsou k dispozici pro analýzu node.js požadavek objekty, které představují volání (podle specifikace SCIM), vytvářeny k webové službě node.js.</span><span class="sxs-lookup"><span data-stu-id="55394-230">[Express route handlers](http://expressjs.com/guide/routing.html) are available for parsing node.js request objects representing calls (as defined by the SCIM specification), made to a node.js web service.</span></span>   

### <a name="building-a-custom-scim-endpoint"></a><span data-ttu-id="55394-231">Vytváření koncový bod vlastní SCIM</span><span class="sxs-lookup"><span data-stu-id="55394-231">Building a Custom SCIM Endpoint</span></span>
<span data-ttu-id="55394-232">Používání knihoven rozhraní příkazového řádku, vývojáře, kteří používají tyto knihovny hostování své služby v rámci všech spustitelný soubor sestavení Common Language Infrastructure, nebo Internetová informační služba.</span><span class="sxs-lookup"><span data-stu-id="55394-232">Using the CLI libraries, developers using those libraries can host their services within any executable Common Language Infrastructure assembly, or within Internet Information Services.</span></span> <span data-ttu-id="55394-233">Tady je ukázkový kód pro hostování služby v rámci spustitelný soubor sestavení, na adrese http://localhost:9000:</span><span class="sxs-lookup"><span data-stu-id="55394-233">Here is sample code for hosting a service within an executable assembly, at the address http://localhost:9000:</span></span> 

    private static void Main(string[] arguments)
    {
    // Microsoft.SystemForCrossDomainIdentityManagement.IMonitor, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IProvider and 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service are all defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service.dll.  

    Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor = 
      new DevelopersMonitor();
    Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider = 
      new DevelopersProvider(arguments[1]);
    Microsoft.SystemForCrossDomainIdentityManagement.Service webService = null;
    try
    {
        webService = new WebService(monitor, provider);
        webService.Start("http://localhost:9000");

        Console.ReadKey(true);
    }
    finally
    {
        if (webService != null)
        {
            webService.Dispose();
            webService = null;
        }
    }
    }

    public class WebService : Microsoft.SystemForCrossDomainIdentityManagement.Service
    {
    private Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor;
    private Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider;

    public WebService(
      Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitoringBehavior, 
      Microsoft.SystemForCrossDomainIdentityManagement.IProvider providerBehavior)
    {
        this.monitor = monitoringBehavior;
        this.provider = providerBehavior;
    }

    public override IMonitor MonitoringBehavior
    {
        get
        {
            return this.monitor;
        }

        set
        {
            this.monitor = value;
        }
    }

    public override IProvider ProviderBehavior
    {
        get
        {
            return this.provider;
        }

        set
        {
            this.provider = value;
        }
    }
    }

<span data-ttu-id="55394-234">Tato služba musí mít HTTP adresa a server ověřování certifikát které kořenové certifikační autority je jedním z těchto:</span><span class="sxs-lookup"><span data-stu-id="55394-234">This service must have an HTTP address and server authentication certificate of which the root certification authority is one of the following:</span></span> 

* <span data-ttu-id="55394-235">CNNIC</span><span class="sxs-lookup"><span data-stu-id="55394-235">CNNIC</span></span>
* <span data-ttu-id="55394-236">Comodo</span><span class="sxs-lookup"><span data-stu-id="55394-236">Comodo</span></span>
* <span data-ttu-id="55394-237">CyberTrust</span><span class="sxs-lookup"><span data-stu-id="55394-237">CyberTrust</span></span>
* <span data-ttu-id="55394-238">DigiCert</span><span class="sxs-lookup"><span data-stu-id="55394-238">DigiCert</span></span>
* <span data-ttu-id="55394-239">GeoTrust</span><span class="sxs-lookup"><span data-stu-id="55394-239">GeoTrust</span></span>
* <span data-ttu-id="55394-240">GlobalSign</span><span class="sxs-lookup"><span data-stu-id="55394-240">GlobalSign</span></span>
* <span data-ttu-id="55394-241">Go Daddy</span><span class="sxs-lookup"><span data-stu-id="55394-241">Go Daddy</span></span>
* <span data-ttu-id="55394-242">VeriSign</span><span class="sxs-lookup"><span data-stu-id="55394-242">Verisign</span></span>
* <span data-ttu-id="55394-243">WoSign</span><span class="sxs-lookup"><span data-stu-id="55394-243">WoSign</span></span>

<span data-ttu-id="55394-244">Ověřovací certifikát serverů může být vázaný na port na hostitele Windows pomocí nástroje prostředí sítě:</span><span class="sxs-lookup"><span data-stu-id="55394-244">A server authentication certificate can be bound to a port on a Windows host using the network shell utility:</span></span> 

    netsh http add sslcert ipport=0.0.0.0:443 certhash=0000000000003ed9cd0c315bbb6dc1c08da5e6 appid={00112233-4455-6677-8899-AABBCCDDEEFF}  

<span data-ttu-id="55394-245">Hodnota poskytnutá pro certhash argument tady, je kryptografický otisk certifikátu, zatímco hodnota poskytnutá pro appid argument je libovolný identifikátor, globálně jedinečný.</span><span class="sxs-lookup"><span data-stu-id="55394-245">Here, the value provided for the certhash argument is the thumbprint of the certificate, while the value provided for the appid argument is an arbitrary globally unique identifier.</span></span>  

<span data-ttu-id="55394-246">K hostování služby v rámci Internetové informační služby, by vývojář sestavení sestavení knihovny kódu CLA s třídy s názvem spuštění v výchozí obor názvů sestavení.</span><span class="sxs-lookup"><span data-stu-id="55394-246">To host the service within Internet Information Services, a developer would build a CLA code library assembly with a class named Startup in the default namespace of the assembly.</span></span>  <span data-ttu-id="55394-247">Zde je ukázka této třídy:</span><span class="sxs-lookup"><span data-stu-id="55394-247">Here is a sample of such a class:</span></span> 

    public class Startup
    {
    // Microsoft.SystemForCrossDomainIdentityManagement.IWebApplicationStarter, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IMonitor and  
    // Microsoft.SystemForCrossDomainIdentityManagement.Service are all defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service.dll.  

    Microsoft.SystemForCrossDomainIdentityManagement.IWebApplicationStarter starter;

    public Startup()
    {
        Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor = 
          new DevelopersMonitor();
        Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider = 
          new DevelopersProvider();
        this.starter = 
          new Microsoft.SystemForCrossDomainIdentityManagement.WebApplicationStarter(
            provider, 
            monitor);
    }

    public void Configuration(
      Owin.IAppBuilder builder) // Defined in in Owin.dll.  
    {
        this.starter.ConfigureApplication(builder);
    }
    }

### <a name="handling-endpoint-authentication"></a><span data-ttu-id="55394-248">Zpracování koncový bod ověřování</span><span class="sxs-lookup"><span data-stu-id="55394-248">Handling endpoint authentication</span></span>
<span data-ttu-id="55394-249">Žádosti z Azure Active Directory zahrnovat tokenu nosiče OAuth 2.0.</span><span class="sxs-lookup"><span data-stu-id="55394-249">Requests from Azure Active Directory include an OAuth 2.0 bearer token.</span></span>   <span data-ttu-id="55394-250">Všechny služby přijetí žádosti by měl ověřit vystavitele jako jménem očekávané klienta Azure Active Directory pro přístup k webové službě Azure Active Directory Graph Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="55394-250">Any service receiving the request should authenticate the issuer as being Azure Active Directory on behalf of the expected Azure Active Directory tenant, for access to the Azure Active Directory Graph web service.</span></span>  <span data-ttu-id="55394-251">V tokenu, Vystavitel je identifikována deklaraci identity iss, například "iss": "https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/".</span><span class="sxs-lookup"><span data-stu-id="55394-251">In the token, the issuer is identified by an iss claim, like, "iss":"https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/".</span></span>  <span data-ttu-id="55394-252">V tomto příkladu bázové adresy hodnota deklarace identity, https://sts.windows.net, Azure Active Directory identifikuje jako vydavatel, zatímco relativní adresu segmentu cbb1a5ac-f33b-45fa-9bf5-f37db0fed422, je jedinečný identifikátor služby Azure Active Directory Klient jménem kterého byl token vydán.</span><span class="sxs-lookup"><span data-stu-id="55394-252">In this example, the base address of the claim value, https://sts.windows.net, identifies Azure Active Directory as the issuer, while the relative address segment, cbb1a5ac-f33b-45fa-9bf5-f37db0fed422, is a unique identifier of the Azure Active Directory tenant on behalf of which the token was issued.</span></span>  <span data-ttu-id="55394-253">Pokud byl token vydán pro přístup k webové službě Azure Active Directory Graph, by měl být identifikátor této služby, 00000002-0000-0000-c000-000000000000, v hodnotě deklarace identity oblast je token.</span><span class="sxs-lookup"><span data-stu-id="55394-253">If the token was issued for accessing the Azure Active Directory Graph web service, then the identifier of that service, 00000002-0000-0000-c000-000000000000, should be in the value of the token’s aud claim.</span></span>  

<span data-ttu-id="55394-254">Vývojáři pomocí knihovny CLA od společnosti Microsoft pro vytváření SCIM služby můžete ověřování žádostí z Azure Active Directory pomocí balíček Microsoft.Owin.Security.ActiveDirectory pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="55394-254">Developers using the CLA libraries provided by Microsoft for building a SCIM service can authenticate requests from Azure Active Directory using the Microsoft.Owin.Security.ActiveDirectory package by following these steps:</span></span> 

1. <span data-ttu-id="55394-255">U zprostředkovatele implementujte vlastnost Microsoft.SystemForCrossDomainIdentityManagement.IProvider.StartupBehavior tak, že se vrátí metoda, která se má volat při každém spuštění služby:</span><span class="sxs-lookup"><span data-stu-id="55394-255">In a provider, implement the Microsoft.SystemForCrossDomainIdentityManagement.IProvider.StartupBehavior property by having it return a method to be called whenever the service is started:</span></span> 

  ````
    public override Action\<Owin.IAppBuilder, System.Web.Http.HttpConfiguration.HttpConfiguration\> StartupBehavior
    {
      get
      {
        return this.OnServiceStartup;
      }
    }

    private void OnServiceStartup(
      Owin.IAppBuilder applicationBuilder,  // Defined in Owin.dll.  
      System.Web.Http.HttpConfiguration configuration)  // Defined in System.Web.Http.dll.  
    {
    }
  ````

2. <span data-ttu-id="55394-256">Přidejte následující kód do dané metody mít každá žádost o žádné koncové body služby ověřený jako opatřené tokenem vydaným službou Azure Active Directory jménem zadaného klienta, pro přístup k webové službě Azure AD Graph:</span><span class="sxs-lookup"><span data-stu-id="55394-256">Add the following code to that method to have any request to any of the service’s endpoints authenticated as bearing a token issued by Azure Active Directory on behalf of a specified tenant, for access to the Azure AD Graph web service:</span></span> 

  ````
    private void OnServiceStartup(
      Owin.IAppBuilder applicationBuilder IAppBuilder applicationBuilder, 
      System.Web.Http.HttpConfiguration HttpConfiguration configuration)
    {
      // IFilter is defined in System.Web.Http.dll.  
      System.Web.Http.Filters.IFilter authorizationFilter = 
        new System.Web.Http.AuthorizeAttribute(); // Defined in System.Web.Http.dll.configuration.Filters.Add(authorizationFilter);

      // SystemIdentityModel.Tokens.TokenValidationParameters is defined in    
      // System.IdentityModel.Token.Jwt.dll.
      SystemIdentityModel.Tokens.TokenValidationParameters tokenValidationParameters =     
        new TokenValidationParameters()
        {
          ValidAudience = "00000002-0000-0000-c000-000000000000"
        };

      // WindowsAzureActiveDirectoryBearerAuthenticationOptions is defined in 
      // Microsoft.Owin.Security.ActiveDirectory.dll
      Microsoft.Owin.Security.ActiveDirectory.
      WindowsAzureActiveDirectoryBearerAuthenticationOptions authenticationOptions =
        new WindowsAzureActiveDirectoryBearerAuthenticationOptions()    {
        TokenValidationParameters = tokenValidationParameters,
        Tenant = "03F9FCBC-EA7B-46C2-8466-F81917F3C15E" // Substitute the appropriate tenant’s 
                                                      // identifier for this one.  
      };

      applicationBuilder.UseWindowsAzureActiveDirectoryBearerAuthentication(authenticationOptions);
    }
  ````


## <a name="user-and-group-schema"></a><span data-ttu-id="55394-257">Schéma uživatelů a skupin</span><span class="sxs-lookup"><span data-stu-id="55394-257">User and group schema</span></span>
<span data-ttu-id="55394-258">Azure Active Directory můžete zřídit dva typy prostředků, aby SCIM webové služby.</span><span class="sxs-lookup"><span data-stu-id="55394-258">Azure Active Directory can provision two types of resources to SCIM web services.</span></span>  <span data-ttu-id="55394-259">Tyto typy prostředků jsou uživatelé a skupiny.</span><span class="sxs-lookup"><span data-stu-id="55394-259">Those types of resources are users and groups.</span></span>  

<span data-ttu-id="55394-260">Uživatel prostředky jsou označeny identifikátor schématu urn: ietf:params:scim:schemas:extension:enterprise:2.0:User, který je součástí této specifikace protokolu: http://tools.ietf.org/html/draft-ietf-scim-core-schema.</span><span class="sxs-lookup"><span data-stu-id="55394-260">User resources are identified by the schema identifier, urn:ietf:params:scim:schemas:extension:enterprise:2.0:User, which is included in this protocol specification: http://tools.ietf.org/html/draft-ietf-scim-core-schema.</span></span>  <span data-ttu-id="55394-261">Výchozí mapování atributů uživatelům atributy urn: ietf:params:scim:schemas:extension:enterprise:2.0:User prostředků v Azure Active Directory je uvedené v tabulce 1, níže.</span><span class="sxs-lookup"><span data-stu-id="55394-261">The default mapping of the attributes of users in Azure Active Directory to the attributes of urn:ietf:params:scim:schemas:extension:enterprise:2.0:User resources is provided in table 1, below.</span></span>  

<span data-ttu-id="55394-262">Skupiny prostředků jsou identifikovány identifikátor schématu http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group.</span><span class="sxs-lookup"><span data-stu-id="55394-262">Group resources are identified by the schema identifier, http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group.</span></span>  <span data-ttu-id="55394-263">Tabulka 2 níže znázorňuje výchozí mapování atributů skupin v Azure Active Directory atributy http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group prostředků.</span><span class="sxs-lookup"><span data-stu-id="55394-263">Table 2, below, shows the default mapping of the attributes of groups in Azure Active Directory to the attributes of http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group resources.</span></span>  

### <a name="table-1-default-user-attribute-mapping"></a><span data-ttu-id="55394-264">Tabulka 1: Výchozí mapování atributů uživatele</span><span class="sxs-lookup"><span data-stu-id="55394-264">Table 1: Default user attribute mapping</span></span>
| <span data-ttu-id="55394-265">Uživatele Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="55394-265">Azure Active Directory user</span></span> | <span data-ttu-id="55394-266">název urn: ietf:params:scim:schemas:extension:enterprise:2.0:User</span><span class="sxs-lookup"><span data-stu-id="55394-266">urn:ietf:params:scim:schemas:extension:enterprise:2.0:User</span></span> |
| --- | --- |
| <span data-ttu-id="55394-267">IsSoftDeleted</span><span class="sxs-lookup"><span data-stu-id="55394-267">IsSoftDeleted</span></span> |<span data-ttu-id="55394-268">Aktivní</span><span class="sxs-lookup"><span data-stu-id="55394-268">active</span></span> |
| <span data-ttu-id="55394-269">displayName</span><span class="sxs-lookup"><span data-stu-id="55394-269">displayName</span></span> |<span data-ttu-id="55394-270">displayName</span><span class="sxs-lookup"><span data-stu-id="55394-270">displayName</span></span> |
| <span data-ttu-id="55394-271">TelephoneNumber faxu</span><span class="sxs-lookup"><span data-stu-id="55394-271">Facsimile-TelephoneNumber</span></span> |<span data-ttu-id="55394-272">.value phoneNumbers [typ eq "fax"]</span><span class="sxs-lookup"><span data-stu-id="55394-272">phoneNumbers[type eq "fax"].value</span></span> |
| <span data-ttu-id="55394-273">givenName</span><span class="sxs-lookup"><span data-stu-id="55394-273">givenName</span></span> |<span data-ttu-id="55394-274">name.givenName</span><span class="sxs-lookup"><span data-stu-id="55394-274">name.givenName</span></span> |
| <span data-ttu-id="55394-275">pracovní funkce</span><span class="sxs-lookup"><span data-stu-id="55394-275">jobTitle</span></span> |<span data-ttu-id="55394-276">Název</span><span class="sxs-lookup"><span data-stu-id="55394-276">title</span></span> |
| <span data-ttu-id="55394-277">E-mailu</span><span class="sxs-lookup"><span data-stu-id="55394-277">mail</span></span> |<span data-ttu-id="55394-278">.value e-mailů [typ eq "práce"]</span><span class="sxs-lookup"><span data-stu-id="55394-278">emails[type eq "work"].value</span></span> |
| <span data-ttu-id="55394-279">mailNickname</span><span class="sxs-lookup"><span data-stu-id="55394-279">mailNickname</span></span> |<span data-ttu-id="55394-280">externalId</span><span class="sxs-lookup"><span data-stu-id="55394-280">externalId</span></span> |
| <span data-ttu-id="55394-281">Správce</span><span class="sxs-lookup"><span data-stu-id="55394-281">manager</span></span> |<span data-ttu-id="55394-282">Správce</span><span class="sxs-lookup"><span data-stu-id="55394-282">manager</span></span> |
| <span data-ttu-id="55394-283">mobilní</span><span class="sxs-lookup"><span data-stu-id="55394-283">mobile</span></span> |<span data-ttu-id="55394-284">.value phoneNumbers [eq typ "mobilní"]</span><span class="sxs-lookup"><span data-stu-id="55394-284">phoneNumbers[type eq "mobile"].value</span></span> |
| <span data-ttu-id="55394-285">objectId</span><span class="sxs-lookup"><span data-stu-id="55394-285">objectId</span></span> |<span data-ttu-id="55394-286">id</span><span class="sxs-lookup"><span data-stu-id="55394-286">id</span></span> |
| <span data-ttu-id="55394-287">PSČ</span><span class="sxs-lookup"><span data-stu-id="55394-287">postalCode</span></span> |<span data-ttu-id="55394-288">adresy [typ eq "práce"] .postalCode</span><span class="sxs-lookup"><span data-stu-id="55394-288">addresses[type eq "work"].postalCode</span></span> |
| <span data-ttu-id="55394-289">proxy adresy</span><span class="sxs-lookup"><span data-stu-id="55394-289">proxy-Addresses</span></span> |<span data-ttu-id="55394-290">[Zadejte eq "ostatní"] e-mailů. Hodnota</span><span class="sxs-lookup"><span data-stu-id="55394-290">emails[type eq "other"].Value</span></span> |
| <span data-ttu-id="55394-291">fyzický. doručení OfficeName</span><span class="sxs-lookup"><span data-stu-id="55394-291">physical-Delivery-OfficeName</span></span> |<span data-ttu-id="55394-292">adresy [Zadejte eq "ostatní"]. Formátu</span><span class="sxs-lookup"><span data-stu-id="55394-292">addresses[type eq "other"].Formatted</span></span> |
| <span data-ttu-id="55394-293">StreetAddress</span><span class="sxs-lookup"><span data-stu-id="55394-293">streetAddress</span></span> |<span data-ttu-id="55394-294">adresy [typ eq "práce"] .streetAddress</span><span class="sxs-lookup"><span data-stu-id="55394-294">addresses[type eq "work"].streetAddress</span></span> |
| <span data-ttu-id="55394-295">Příjmení</span><span class="sxs-lookup"><span data-stu-id="55394-295">surname</span></span> |<span data-ttu-id="55394-296">name.familyName</span><span class="sxs-lookup"><span data-stu-id="55394-296">name.familyName</span></span> |
| <span data-ttu-id="55394-297">telefonní číslo</span><span class="sxs-lookup"><span data-stu-id="55394-297">telephone-Number</span></span> |<span data-ttu-id="55394-298">.value phoneNumbers [typ eq "práce"]</span><span class="sxs-lookup"><span data-stu-id="55394-298">phoneNumbers[type eq "work"].value</span></span> |
| <span data-ttu-id="55394-299">PrincipalName uživatele</span><span class="sxs-lookup"><span data-stu-id="55394-299">user-PrincipalName</span></span> |<span data-ttu-id="55394-300">Uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="55394-300">userName</span></span> |

### <a name="table-2-default-group-attribute-mapping"></a><span data-ttu-id="55394-301">Tabulka 2: Výchozí skupiny mapování atributů</span><span class="sxs-lookup"><span data-stu-id="55394-301">Table 2: Default group attribute mapping</span></span>
| <span data-ttu-id="55394-302">Skupiny Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="55394-302">Azure Active Directory group</span></span> | <span data-ttu-id="55394-303">http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group</span><span class="sxs-lookup"><span data-stu-id="55394-303">http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group</span></span> |
| --- | --- |
| <span data-ttu-id="55394-304">displayName</span><span class="sxs-lookup"><span data-stu-id="55394-304">displayName</span></span> |<span data-ttu-id="55394-305">externalId</span><span class="sxs-lookup"><span data-stu-id="55394-305">externalId</span></span> |
| <span data-ttu-id="55394-306">E-mailu</span><span class="sxs-lookup"><span data-stu-id="55394-306">mail</span></span> |<span data-ttu-id="55394-307">.value e-mailů [typ eq "práce"]</span><span class="sxs-lookup"><span data-stu-id="55394-307">emails[type eq "work"].value</span></span> |
| <span data-ttu-id="55394-308">mailNickname</span><span class="sxs-lookup"><span data-stu-id="55394-308">mailNickname</span></span> |<span data-ttu-id="55394-309">displayName</span><span class="sxs-lookup"><span data-stu-id="55394-309">displayName</span></span> |
| <span data-ttu-id="55394-310">Členy</span><span class="sxs-lookup"><span data-stu-id="55394-310">members</span></span> |<span data-ttu-id="55394-311">Členy</span><span class="sxs-lookup"><span data-stu-id="55394-311">members</span></span> |
| <span data-ttu-id="55394-312">objectId</span><span class="sxs-lookup"><span data-stu-id="55394-312">objectId</span></span> |<span data-ttu-id="55394-313">id</span><span class="sxs-lookup"><span data-stu-id="55394-313">id</span></span> |
| <span data-ttu-id="55394-314">proxyAddresses</span><span class="sxs-lookup"><span data-stu-id="55394-314">proxyAddresses</span></span> |<span data-ttu-id="55394-315">[Zadejte eq "ostatní"] e-mailů. Hodnota</span><span class="sxs-lookup"><span data-stu-id="55394-315">emails[type eq "other"].Value</span></span> |

## <a name="user-provisioning-and-de-provisioning"></a><span data-ttu-id="55394-316">Zřizování uživatelů a jeho rušení</span><span class="sxs-lookup"><span data-stu-id="55394-316">User provisioning and de-provisioning</span></span>
<span data-ttu-id="55394-317">Následující obrázek znázorňuje zprávy, že Azure Active Directory odešle SCIM službě pro správu životního cyklu uživatele v jiné úložiště identit.</span><span class="sxs-lookup"><span data-stu-id="55394-317">The following illustration shows the messages that Azure Active Directory sends to a SCIM service to manage the lifecycle of a user in another identity store.</span></span> <span data-ttu-id="55394-318">Diagram také ukazuje, jak SCIM implementovaná pomocí rozhraní příkazového řádku knihovny poskytovat službu Microsoft pro vytvoření, že tyto služby převede tyto požadavky na volání metody zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="55394-318">The diagram also shows how a SCIM service implemented using the CLI libraries provided by Microsoft for building such services translate those requests into calls to the methods of a provider.</span></span>  

<span data-ttu-id="55394-319">![][4]
*Obrázek 5: Zřizování uživatelů a jeho rušení pořadí*</span><span class="sxs-lookup"><span data-stu-id="55394-319">![][4]
*Figure 5: User provisioning and de-provisioning sequence*</span></span>

1. <span data-ttu-id="55394-320">Azure Active Directory dotáže služby pro uživatele s hodnotou atributu externalId odpovídající hodnota atributu mailNickname uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="55394-320">Azure Active Directory queries the service for a user with an externalId attribute value matching the mailNickname attribute value of a user in Azure AD.</span></span> <span data-ttu-id="55394-321">Dotaz je vyjádřena jako žádost protokolu HTTP (Hypertext Transfer), jako je tento příklad, ve kterém jyoung je ukázka mailNickname uživatele v Azure Active Directory:</span><span class="sxs-lookup"><span data-stu-id="55394-321">The query is expressed as a Hypertext Transfer Protocol (HTTP) request such as this example, wherein jyoung is a sample of a mailNickname of a user in Azure Active Directory:</span></span> 
  ````
    GET https://.../scim/Users?filter=externalId eq jyoung HTTP/1.1
    Authorization: Bearer ...
  ````
  <span data-ttu-id="55394-322">Pokud služba je vytvořená pomocí knihovny Common Language Infrastructure od společnosti Microsoft pro implementaci služby SCIM, je požadavek přeložit na volání metody dotazu zprostředkovatele služby.</span><span class="sxs-lookup"><span data-stu-id="55394-322">If the service was built using the Common Language Infrastructure libraries provided by Microsoft for implementing SCIM services, then the request is translated into a call to the Query method of the service’s provider.</span></span>  <span data-ttu-id="55394-323">Tady je podpis této metody:</span><span class="sxs-lookup"><span data-stu-id="55394-323">Here is the signature of that method:</span></span> 
  ````
    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.Resource is defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  
    // Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters is defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Protocol.  

    System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource[]> Query(
      Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters parameters, 
      string correlationIdentifier);
  ````
  <span data-ttu-id="55394-324">Tady je definici rozhraní Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters:</span><span class="sxs-lookup"><span data-stu-id="55394-324">Here is the definition of the Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters interface:</span></span> 
  ````
    public interface IQueryParameters: 
      Microsoft.SystemForCrossDomainIdentityManagement.IRetrievalParameters
    {
        System.Collections.Generic.IReadOnlyCollection <Microsoft.SystemForCrossDomainIdentityManagement.IFilter> AlternateFilters 
        { get; }
    }

    public interface Microsoft.SystemForCrossDomainIdentityManagement.IRetrievalParameters
    {
      system.Collections.Generic.IReadOnlyCollection<string> ExcludedAttributePaths 
      { get; }
      System.Collections.Generic.IReadOnlyCollection<string> RequestedAttributePaths 
      { get; }
      string SchemaIdentifier 
      { get; }
    }

    public interface Microsoft.SystemForCrossDomainIdentityManagement.IFilter
    {
        Microsoft.SystemForCrossDomainIdentityManagement.IFilter AdditionalFilter 
          { get; set; }
        string AttributePath 
          { get; } 
        Microsoft.SystemForCrossDomainIdentityManagement.ComparisonOperator FilterOperator 
          { get; }
        string ComparisonValue 
          { get; }
    }

    public enum Microsoft.SystemForCrossDomainIdentityManagement.ComparisonOperator
    {
        Equals
    }
  ````
  <span data-ttu-id="55394-325">V následující ukázce dotazu pro uživatele s danou hodnotou atributu externalId jsou hodnoty argumentů předaný metodě dotazu:</span><span class="sxs-lookup"><span data-stu-id="55394-325">In the following sample of a query for a user with a given value for the externalId attribute, values of the arguments passed to the Query method are:</span></span> 
  * <span data-ttu-id="55394-326">Parametry. AlternateFilters.Count: 1</span><span class="sxs-lookup"><span data-stu-id="55394-326">parameters.AlternateFilters.Count: 1</span></span>
  * <span data-ttu-id="55394-327">Parametry. AlternateFilters.ElementAt(0). AttributePath: "externalId"</span><span class="sxs-lookup"><span data-stu-id="55394-327">parameters.AlternateFilters.ElementAt(0).AttributePath: "externalId"</span></span>
  * <span data-ttu-id="55394-328">Parametry. AlternateFilters.ElementAt(0). Porovnávací operátor: ComparisonOperator.Equals</span><span class="sxs-lookup"><span data-stu-id="55394-328">parameters.AlternateFilters.ElementAt(0).ComparisonOperator: ComparisonOperator.Equals</span></span>
  * <span data-ttu-id="55394-329">Parametry. AlternateFilter.ElementAt(0). ComparisonValue: "jyoung"</span><span class="sxs-lookup"><span data-stu-id="55394-329">parameters.AlternateFilter.ElementAt(0).ComparisonValue: "jyoung"</span></span>
  * <span data-ttu-id="55394-330">correlationIdentifier: System.Net.Http.HttpRequestMessage.GetOwinEnvironment["owin. ID žádosti"]</span><span class="sxs-lookup"><span data-stu-id="55394-330">correlationIdentifier: System.Net.Http.HttpRequestMessage.GetOwinEnvironment["owin.RequestId"]</span></span> 

2. <span data-ttu-id="55394-331">Pokud odpověď na dotaz pro webovou službu pro uživatele s hodnotou atributu externalId odpovídající hodnota atributu mailNickname uživatele nevrátí žádné uživatele, pak Azure Active Directory požadavky že službu zřizovat uživatele odpovídající tomu v Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="55394-331">If the response to a query to the web service for a user with an externalId attribute value that matches the mailNickname attribute value of a user does not return any users, then Azure Active Directory requests that the service provision a user corresponding to the one in Azure Active Directory.</span></span>  <span data-ttu-id="55394-332">Tady je příklad této žádosti:</span><span class="sxs-lookup"><span data-stu-id="55394-332">Here is an example of such a request:</span></span> 
  ````
    POST https://.../scim/Users HTTP/1.1
    Authorization: Bearer ...
    Content-type: application/json
    {
      "schemas":
      [
        "urn:ietf:params:scim:schemas:core:2.0:User",
        "urn:ietf:params:scim:schemas:extension:enterprise:2.0User"],
      "externalId":"jyoung",
      "userName":"jyoung",
      "active":true,
      "addresses":null,
      "displayName":"Joy Young",
      "emails": [
        {
          "type":"work",
          "value":"jyoung@Contoso.com",
          "primary":true}],
      "meta": {
        "resourceType":"User"},
       "name":{
        "familyName":"Young",
        "givenName":"Joy"},
      "phoneNumbers":null,
      "preferredLanguage":null,
      "title":null,
      "department":null,
      "manager":null}
  ````
  <span data-ttu-id="55394-333">Knihovny Common Language Infrastructure od společnosti Microsoft pro implementaci služby SCIM by převede tento požadavek na volání metody vytvoření poskytovatele služby.</span><span class="sxs-lookup"><span data-stu-id="55394-333">The Common Language Infrastructure libraries provided by Microsoft for implementing SCIM services would translate that request into a call to the Create method of the service’s provider.</span></span>  <span data-ttu-id="55394-334">Metodu Create má tento podpis:</span><span class="sxs-lookup"><span data-stu-id="55394-334">The Create method has this signature:</span></span> 
  ````
    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.Resource is defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  

    System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource> Create(
      Microsoft.SystemForCrossDomainIdentityManagement.Resource resource, 
      string correlationIdentifier);
  ````
  <span data-ttu-id="55394-335">V požadavku na přidělení uživatele je hodnota argumentu prostředků instanci Microsoft.SystemForCrossDomainIdentityManagement.</span><span class="sxs-lookup"><span data-stu-id="55394-335">In a request to provision a user, the value of the resource argument is an instance of the Microsoft.SystemForCrossDomainIdentityManagement.</span></span> <span data-ttu-id="55394-336">Třída Core2EnterpriseUser, definována v knihovně Microsoft.SystemForCrossDomainIdentityManagement.Schemas.</span><span class="sxs-lookup"><span data-stu-id="55394-336">Core2EnterpriseUser class, defined in the Microsoft.SystemForCrossDomainIdentityManagement.Schemas library.</span></span>  <span data-ttu-id="55394-337">Pokud neproběhne ke zřízení uživatele, implementace metody musí vrátit instanci Microsoft.SystemForCrossDomainIdentityManagement.</span><span class="sxs-lookup"><span data-stu-id="55394-337">If the request to provision the user succeeds, then the implementation of the method is expected to return an instance of the Microsoft.SystemForCrossDomainIdentityManagement.</span></span> <span data-ttu-id="55394-338">Třída Core2EnterpriseUser s hodnotou vlastnost nastavena na jedinečný identifikátor nově zřízení uživatele na identifikátor.</span><span class="sxs-lookup"><span data-stu-id="55394-338">Core2EnterpriseUser class, with the value of the Identifier property set to the unique identifier of the newly provisioned user.</span></span>  

3. <span data-ttu-id="55394-339">Provést aktualizaci uživatele ví, že existují v úložišti identity přední stěnou podle SCIM, pokračuje Azure Active Directory tím, že požádá aktuální stav daného uživatele ze služby s žádostí. například:</span><span class="sxs-lookup"><span data-stu-id="55394-339">To update a user known to exist in an identity store fronted by an SCIM, Azure Active Directory proceeds by requesting the current state of that user from the service with a request such as:</span></span> 
  ````
    GET ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...
  ````
  <span data-ttu-id="55394-340">Ve službě sestaven pomocí knihovny Common Language Infrastructure od společnosti Microsoft pro implementaci služby SCIM je požadavek přeložit na volání metody načtení zprostředkovatele služby.</span><span class="sxs-lookup"><span data-stu-id="55394-340">In a service built using the Common Language Infrastructure libraries provided by Microsoft for implementing SCIM services, the request is translated into a call to the Retrieve method of the service’s provider.</span></span>  <span data-ttu-id="55394-341">Tady je podpis metody načtení:</span><span class="sxs-lookup"><span data-stu-id="55394-341">Here is the signature of the Retrieve method:</span></span> 
  ````
    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.Resource and 
    // Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters 
    // are defined in Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  
    System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource> 
       Retrieve(
         Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters 
           parameters, 
           string correlationIdentifier);

    public interface 
      Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters:   
        IRetrievalParameters
        {
          Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier 
            ResourceIdentifier 
              { get; }
    }
    public interface Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier
    {
        string Identifier 
          { get; set; }
        string Microsoft.SystemForCrossDomainIdentityManagement.SchemaIdentifier 
          { get; set; }
    }
  ````
  <span data-ttu-id="55394-342">V příkladu požadavek na načtení aktuálního stavu uživatele jsou hodnoty vlastností objektu zadaný jako hodnota argumentu parametry:</span><span class="sxs-lookup"><span data-stu-id="55394-342">In the example of a request to retrieve the current state of a user, the values of the properties of the object provided as the value of the parameters argument are as follows:</span></span> 
  
  * <span data-ttu-id="55394-343">Identifikátor: "54D382A4-2050-4C03-94D1-E769F1D15682"</span><span class="sxs-lookup"><span data-stu-id="55394-343">Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"</span></span>
  * <span data-ttu-id="55394-344">SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"</span><span class="sxs-lookup"><span data-stu-id="55394-344">SchemaIdentifier: "urn:ietf:params:scim:schemas:extension:enterprise:2.0:User"</span></span>

4. <span data-ttu-id="55394-345">Pokud atribut typu odkaz je potřeba aktualizovat, Azure Active Directory dotazuje službu, kterou chcete zjistit, zda aktuální hodnotu atributu odkaz v úložišti identity přední stěnou službou již odpovídá hodnota tohoto atributu v Azure Active Adresář.</span><span class="sxs-lookup"><span data-stu-id="55394-345">If a reference attribute is to be updated, then Azure Active Directory queries the service to determine whether or not the current value of the reference attribute in the identity store fronted by the service already matches the value of that attribute in Azure Active Directory.</span></span> <span data-ttu-id="55394-346">Pro uživatele pouze atributů, které aktuální hodnota je dotazován tímto způsobem je atribut správce.</span><span class="sxs-lookup"><span data-stu-id="55394-346">For users, the only attribute of which the current value is queried in this way is the manager attribute.</span></span> <span data-ttu-id="55394-347">Tady je příklad požadavku k určení, zda atribut manager objektu konkrétní uživatel aktuálně má určitou hodnotu:</span><span class="sxs-lookup"><span data-stu-id="55394-347">Here is an example of a request to determine whether the manager attribute of a particular user object currently has a certain value:</span></span> 
  ````
    GET ~/scim/Users?filter=id eq 54D382A4-2050-4C03-94D1-E769F1D15682 and manager eq 2819c223-7f76-453a-919d-413861904646&attributes=id HTTP/1.1
    Authorization: Bearer ...
  ````
  <span data-ttu-id="55394-348">Hodnota parametru dotazu atributy id, označuje, že, pokud existuje objekt uživatele, který splňuje výraz zadaný jako hodnota parametru dotazu filtru a pak službu musí odpovědět s urn: ietf:params:scim:schemas:core:2.0:User nebo název urn: ietf:params:scim:schemas:extension:enterprise:2.0:User prostředků, včetně pouze hodnota atributu id tohoto zdroje.</span><span class="sxs-lookup"><span data-stu-id="55394-348">The value of the attributes query parameter, id, signifies that if a user object exists that satisfies the expression provided as the value of the filter query parameter, then the service is expected to respond with a urn:ietf:params:scim:schemas:core:2.0:User or urn:ietf:params:scim:schemas:extension:enterprise:2.0:User resource, including only the value of that resource’s id attribute.</span></span>  <span data-ttu-id="55394-349">Hodnota **id** atribut je znám žadatel.</span><span class="sxs-lookup"><span data-stu-id="55394-349">The value of the **id** attribute is known to the requestor.</span></span> <span data-ttu-id="55394-350">Hodnota parametru dotazu filtru; je součástí účelem žádostí o jeho je ve skutečnosti o minimální reprezentace daného prostředku, které splňují výraz filtru jako údaj o tom, zda existuje takový objekt.</span><span class="sxs-lookup"><span data-stu-id="55394-350">It is included in the value of the filter query parameter; the purpose of asking for it is actually to request a minimal representation of a resource that satisfying the filter expression as an indication of whether or not any such object exists.</span></span>   

  <span data-ttu-id="55394-351">Pokud služba je vytvořená pomocí knihovny Common Language Infrastructure od společnosti Microsoft pro implementaci služby SCIM, je požadavek přeložit na volání metody dotazu zprostředkovatele služby.</span><span class="sxs-lookup"><span data-stu-id="55394-351">If the service was built using the Common Language Infrastructure libraries provided by Microsoft for implementing SCIM services, then the request is translated into a call to the Query method of the service’s provider.</span></span> <span data-ttu-id="55394-352">Hodnota vlastnosti objektu zadaný jako hodnota argumentu parametry jsou následující:</span><span class="sxs-lookup"><span data-stu-id="55394-352">The value of the properties of the object provided as the value of the parameters argument are as follows:</span></span> 
  
  * <span data-ttu-id="55394-353">Parametry. AlternateFilters.Count: 2</span><span class="sxs-lookup"><span data-stu-id="55394-353">parameters.AlternateFilters.Count: 2</span></span>
  * <span data-ttu-id="55394-354">Parametry. AlternateFilters.ElementAt(x). AttributePath: "id"</span><span class="sxs-lookup"><span data-stu-id="55394-354">parameters.AlternateFilters.ElementAt(x).AttributePath: "id"</span></span>
  * <span data-ttu-id="55394-355">Parametry. AlternateFilters.ElementAt(x). Porovnávací operátor: ComparisonOperator.Equals</span><span class="sxs-lookup"><span data-stu-id="55394-355">parameters.AlternateFilters.ElementAt(x).ComparisonOperator: ComparisonOperator.Equals</span></span>
  * <span data-ttu-id="55394-356">Parametry. AlternateFilter.ElementAt(x). ComparisonValue: "54D382A4-2050-4C03-94D1-E769F1D15682"</span><span class="sxs-lookup"><span data-stu-id="55394-356">parameters.AlternateFilter.ElementAt(x).ComparisonValue: "54D382A4-2050-4C03-94D1-E769F1D15682"</span></span>
  * <span data-ttu-id="55394-357">Parametry. AlternateFilters.ElementAt(y). AttributePath: "správce"</span><span class="sxs-lookup"><span data-stu-id="55394-357">parameters.AlternateFilters.ElementAt(y).AttributePath: "manager"</span></span>
  * <span data-ttu-id="55394-358">Parametry. AlternateFilters.ElementAt(y). Porovnávací operátor: ComparisonOperator.Equals</span><span class="sxs-lookup"><span data-stu-id="55394-358">parameters.AlternateFilters.ElementAt(y).ComparisonOperator: ComparisonOperator.Equals</span></span>
  * <span data-ttu-id="55394-359">Parametry. AlternateFilter.ElementAt(y). ComparisonValue: "2819c223-7f76-453a-919d-413861904646"</span><span class="sxs-lookup"><span data-stu-id="55394-359">parameters.AlternateFilter.ElementAt(y).ComparisonValue: "2819c223-7f76-453a-919d-413861904646"</span></span>
  * <span data-ttu-id="55394-360">Parametry. RequestedAttributePaths.ElementAt(0): "id"</span><span class="sxs-lookup"><span data-stu-id="55394-360">parameters.RequestedAttributePaths.ElementAt(0): "id"</span></span>
  * <span data-ttu-id="55394-361">Parametry. SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"</span><span class="sxs-lookup"><span data-stu-id="55394-361">parameters.SchemaIdentifier: "urn:ietf:params:scim:schemas:extension:enterprise:2.0:User"</span></span>

  <span data-ttu-id="55394-362">Zde hodnotu indexu x může být 0 a hodnotu index y může být 1, nebo může být hodnota parametru x 1 a hodnota y může být 0, v závislosti na pořadí výrazy parametru dotazu filtru.</span><span class="sxs-lookup"><span data-stu-id="55394-362">Here, the value of the index x may be 0 and the value of the index y may be 1, or the value of x may be 1 and the value of y may be 0, depending on the order of the expressions of the filter query parameter.</span></span>   

5. <span data-ttu-id="55394-363">Tady je příklad požadavku z Azure Active Directory do služby SCIM provést aktualizaci uživatele:</span><span class="sxs-lookup"><span data-stu-id="55394-363">Here is an example of a request from Azure Active Directory to an SCIM service to update a user:</span></span> 
  ````
    PATCH ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...
    Content-type: application/json
    {
      "schemas": 
      [
        "urn:ietf:params:scim:api:messages:2.0:PatchOp"],
      "Operations":
      [
        {
          "op":"Add",
          "path":"manager",
          "value":
            [
              {
                "$ref":"http://.../scim/Users/2819c223-7f76-453a-919d-413861904646",
                "value":"2819c223-7f76-453a-919d-413861904646"}]}]}
  ````
  <span data-ttu-id="55394-364">Knihovny Microsoft Common Language Infrastructure pro implementaci služby SCIM by převede požadavek na volání metody aktualizace zprostředkovatele služby.</span><span class="sxs-lookup"><span data-stu-id="55394-364">The Microsoft Common Language Infrastructure libraries for implementing SCIM services would translate the request into a call to the Update method of the service’s provider.</span></span> <span data-ttu-id="55394-365">Tady je podpis metody aktualizace:</span><span class="sxs-lookup"><span data-stu-id="55394-365">Here is the signature of the Update method:</span></span> 
  ````
    // System.Threading.Tasks.Tasks and 
    // System.Collections.Generic.IReadOnlyCollection<T>
    // are defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.IPatch, 
    // Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier, 
    // Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation, 
    // Microsoft.SystemForCrossDomainIdentityManagement.OperationName, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IPath and 
    // Microsoft.SystemForCrossDomainIdentityManagement.OperationValue 
    // are all defined in Microsoft.SystemForCrossDomainIdentityManagement.Protocol. 

    System.Threading.Tasks.Task Update(
      Microsoft.SystemForCrossDomainIdentityManagement.IPatch patch, 
      string correlationIdentifier);

    public interface Microsoft.SystemForCrossDomainIdentityManagement.IPatch
    {
    Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase 
      PatchRequest 
        { get; set; }
    Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier 
      ResourceIdentifier 
        { get; set; }        
    }

    public class PatchRequest2: 
      Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase
    {
    public System.Collections.Generic.IReadOnlyCollection
      <Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation> 
        Operations
        { get;}

    public void AddOperation(
      Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation operation);
    }

    public class PatchOperation
    {
    public Microsoft.SystemForCrossDomainIdentityManagement.OperationName 
      Name
      { get; set; }

    public Microsoft.SystemForCrossDomainIdentityManagement.IPath 
      Path
      { get; set; }

    public System.Collections.Generic.IReadOnlyCollection
      <Microsoft.SystemForCrossDomainIdentityManagement.OperationValue> Value
      { get; }

    public void AddValue(
      Microsoft.SystemForCrossDomainIdentityManagement.OperationValue value);
    }

    public enum OperationName
    {
      Add,
      Remove,
      Replace
    }

    public interface IPath
    {
      string AttributePath { get; }
      System.Collections.Generic.IReadOnlyCollection<IFilter> SubAttributes { get; }
      Microsoft.SystemForCrossDomainIdentityManagement.IPath ValuePath { get; }
    }

    public class OperationValue
    {
      public string Reference
      { get; set; }

      public string Value
      { get; set; }
    }
  ````
    <span data-ttu-id="55394-366">V příkladu požadavek na aktualizaci uživatele má zadaný jako hodnota argumentu oprava objekt hodnoty těchto vlastností:</span><span class="sxs-lookup"><span data-stu-id="55394-366">In the example of a request to update a user, the object provided as the value of the patch argument has these property values:</span></span> 
  
  * <span data-ttu-id="55394-367">ResourceIdentifier.Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"</span><span class="sxs-lookup"><span data-stu-id="55394-367">ResourceIdentifier.Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"</span></span>
  * <span data-ttu-id="55394-368">ResourceIdentifier.SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"</span><span class="sxs-lookup"><span data-stu-id="55394-368">ResourceIdentifier.SchemaIdentifier: "urn:ietf:params:scim:schemas:extension:enterprise:2.0:User"</span></span>
  * <span data-ttu-id="55394-369">(PatchRequest jako PatchRequest2). Operations.Count: 1</span><span class="sxs-lookup"><span data-stu-id="55394-369">(PatchRequest as PatchRequest2).Operations.Count: 1</span></span>
  * <span data-ttu-id="55394-370">(PatchRequest jako PatchRequest2). Operations.ElementAt(0). OperationName: OperationName.Add</span><span class="sxs-lookup"><span data-stu-id="55394-370">(PatchRequest as PatchRequest2).Operations.ElementAt(0).OperationName: OperationName.Add</span></span>
  * <span data-ttu-id="55394-371">(PatchRequest jako PatchRequest2). Operations.ElementAt(0). Path.AttributePath: "správce"</span><span class="sxs-lookup"><span data-stu-id="55394-371">(PatchRequest as PatchRequest2).Operations.ElementAt(0).Path.AttributePath: "manager"</span></span>
  * <span data-ttu-id="55394-372">(PatchRequest jako PatchRequest2). Operations.ElementAt(0). Value.Count: 1</span><span class="sxs-lookup"><span data-stu-id="55394-372">(PatchRequest as PatchRequest2).Operations.ElementAt(0).Value.Count: 1</span></span>
  * <span data-ttu-id="55394-373">(PatchRequest jako PatchRequest2). Operations.ElementAt(0). Value.ElementAt(0). Referenční dokumentace: http://.../scim/Users/2819c223-7f76-453a-919d-413861904646</span><span class="sxs-lookup"><span data-stu-id="55394-373">(PatchRequest as PatchRequest2).Operations.ElementAt(0).Value.ElementAt(0).Reference: http://.../scim/Users/2819c223-7f76-453a-919d-413861904646</span></span>
  * <span data-ttu-id="55394-374">(PatchRequest jako PatchRequest2). Operations.ElementAt(0). Value.ElementAt(0). Hodnota: 2819c223-7f76-453a-919d-413861904646</span><span class="sxs-lookup"><span data-stu-id="55394-374">(PatchRequest as PatchRequest2).Operations.ElementAt(0).Value.ElementAt(0).Value: 2819c223-7f76-453a-919d-413861904646</span></span>

6. <span data-ttu-id="55394-375">Zrušte zřídit uživatele z identity úložiště přední stěnou služba SCIM, Azure AD, jako odešle žádost:</span><span class="sxs-lookup"><span data-stu-id="55394-375">To de-provision a user from an identity store fronted by an SCIM service, Azure AD sends a request such as:</span></span> 
  ````
    DELETE ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...
  ````
  <span data-ttu-id="55394-376">Pokud služba je vytvořená pomocí knihovny Common Language Infrastructure od společnosti Microsoft pro implementaci služby SCIM, je požadavek přeložit na volání metody odstranění zprostředkovatele služby.</span><span class="sxs-lookup"><span data-stu-id="55394-376">If the service was built using the Common Language Infrastructure libraries provided by Microsoft for implementing SCIM services, then the request is translated into a call to the Delete method of the service’s provider.</span></span>   <span data-ttu-id="55394-377">Tato metoda má tento podpis:</span><span class="sxs-lookup"><span data-stu-id="55394-377">That method has this signature:</span></span> 
  ````
    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier, 
    // is defined in Microsoft.SystemForCrossDomainIdentityManagement.Protocol. 
    System.Threading.Tasks.Task Delete(
      Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier  
        resourceIdentifier, 
      string correlationIdentifier);
  ````
  <span data-ttu-id="55394-378">Zadaný jako hodnota argumentu resourceIdentifier objekt má tyto hodnoty vlastností v příkladu žádost zrušte zřídit uživatele:</span><span class="sxs-lookup"><span data-stu-id="55394-378">The object provided as the value of the resourceIdentifier argument has these property values in the example of a request to de-provision a user:</span></span> 
  
  * <span data-ttu-id="55394-379">ResourceIdentifier.Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"</span><span class="sxs-lookup"><span data-stu-id="55394-379">ResourceIdentifier.Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"</span></span>
  * <span data-ttu-id="55394-380">ResourceIdentifier.SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"</span><span class="sxs-lookup"><span data-stu-id="55394-380">ResourceIdentifier.SchemaIdentifier: "urn:ietf:params:scim:schemas:extension:enterprise:2.0:User"</span></span>

## <a name="group-provisioning-and-de-provisioning"></a><span data-ttu-id="55394-381">Zřizování skupiny a jeho rušení</span><span class="sxs-lookup"><span data-stu-id="55394-381">Group provisioning and de-provisioning</span></span>
<span data-ttu-id="55394-382">Následující obrázek znázorňuje zprávy, že Azure AcD odešle SCIM službě pro správu životního cyklu skupiny v jiné úložiště identit.</span><span class="sxs-lookup"><span data-stu-id="55394-382">The following illustration shows the messages that Azure AcD sends to a SCIM service to manage the lifecycle of a group in another identity store.</span></span>  <span data-ttu-id="55394-383">Tyto zprávy se liší od zprávy týkající se uživatelů třemi způsoby:</span><span class="sxs-lookup"><span data-stu-id="55394-383">Those messages differ from the messages pertaining to users in three ways:</span></span> 

* <span data-ttu-id="55394-384">Schéma skupiny prostředků je označený jako http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group.</span><span class="sxs-lookup"><span data-stu-id="55394-384">The schema of a group resource is identified as http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group.</span></span>  
* <span data-ttu-id="55394-385">Požadavky k načtení skupin stanovení, že je atribut členů mají být vyloučeny z jakéhokoliv prostředku v odpovědi na požadavek.</span><span class="sxs-lookup"><span data-stu-id="55394-385">Requests to retrieve groups stipulate that the members attribute is to be excluded from any resource provided in response to the request.</span></span>  
* <span data-ttu-id="55394-386">Požadavky na určit, zda atribut typu odkaz má určitou hodnotu jsou žádosti o atribut členy.</span><span class="sxs-lookup"><span data-stu-id="55394-386">Requests to determine whether a reference attribute has a certain value are requests about the members attribute.</span></span>  

<span data-ttu-id="55394-387">![][5]
*Obrázek 6: Zřizování skupiny a jeho rušení pořadí*</span><span class="sxs-lookup"><span data-stu-id="55394-387">![][5]
*Figure 6: Group provisioning and de-provisioning sequence*</span></span>

## <a name="related-articles"></a><span data-ttu-id="55394-388">Související články</span><span class="sxs-lookup"><span data-stu-id="55394-388">Related articles</span></span>
* [<span data-ttu-id="55394-389">Rejstřík článků o správě aplikací ve službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="55394-389">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
* [<span data-ttu-id="55394-390">Automatizovat uživatele zřízení nebo zrušení zřízení k aplikacím SaaS</span><span class="sxs-lookup"><span data-stu-id="55394-390">Automate User Provisioning/Deprovisioning to SaaS Apps</span></span>](active-directory-saas-app-provisioning.md)
* [<span data-ttu-id="55394-391">Přizpůsobení mapování atributů pro zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="55394-391">Customizing Attribute Mappings for User Provisioning</span></span>](active-directory-saas-customizing-attribute-mappings.md)
* [<span data-ttu-id="55394-392">Zapisují se výrazy pro mapování atributů</span><span class="sxs-lookup"><span data-stu-id="55394-392">Writing Expressions for Attribute Mappings</span></span>](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [<span data-ttu-id="55394-393">Filtry pro zřizování uživatelů oborů</span><span class="sxs-lookup"><span data-stu-id="55394-393">Scoping Filters for User Provisioning</span></span>](active-directory-saas-scoping-filters.md)
* [<span data-ttu-id="55394-394">Účet zřizování oznámení</span><span class="sxs-lookup"><span data-stu-id="55394-394">Account Provisioning Notifications</span></span>](active-directory-saas-account-provisioning-notifications.md)
* [<span data-ttu-id="55394-395">Seznam kurzů k integraci aplikací SaaS</span><span class="sxs-lookup"><span data-stu-id="55394-395">List of Tutorials on How to Integrate SaaS Apps</span></span>](active-directory-saas-tutorial-list.md)

<!--Image references-->
[0]: ./media/active-directory-scim-provisioning/scim-figure-1.PNG
[1]: ./media/active-directory-scim-provisioning/scim-figure-2a.PNG
[2]: ./media/active-directory-scim-provisioning/scim-figure-2b.PNG
[3]: ./media/active-directory-scim-provisioning/scim-figure-3.PNG
[4]: ./media/active-directory-scim-provisioning/scim-figure-4.PNG
[5]: ./media/active-directory-scim-provisioning/scim-figure-5.PNG
