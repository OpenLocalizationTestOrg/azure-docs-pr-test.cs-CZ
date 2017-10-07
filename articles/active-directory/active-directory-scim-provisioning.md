---
title: "aaaUsing systému pro správu identit napříč doménami automaticky zřízení uživatelů a skupin ze služby Azure Active Directory tooapplications | Microsoft Docs"
description: "Azure Active Directory, mohou automaticky poskytovat uživatelé a skupiny tooany aplikace nebo identity úložiště, které je přední stěnou webovou službou hello rozhraní definované v hello specifikace protokolu SCIM"
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
ms.openlocfilehash: 43045c97e68d0d22db598dcb5ec23481c4e97718
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-system-for-cross-domain-identity-management-tooautomatically-provision-users-and-groups-from-azure-active-directory-tooapplications"></a><span data-ttu-id="db30a-103">Pomocí systému pro správu identit napříč doménami tooautomatically zřizování uživatelů a skupin ze služby Azure Active Directory tooapplications</span><span class="sxs-lookup"><span data-stu-id="db30a-103">Using System for Cross-Domain Identity Management tooautomatically provision users and groups from Azure Active Directory tooapplications</span></span>

## <a name="overview"></a><span data-ttu-id="db30a-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="db30a-104">Overview</span></span>
<span data-ttu-id="db30a-105">Azure Active Directory (Azure AD), mohou automaticky poskytovat uživatelé a skupiny tooany aplikace nebo identity úložiště, které je přední stěnou webovou službou hello rozhraní definované v hello [systému pro napříč doménami Identity Management (SCIM) 2.0 Specifikace protokolu](https://tools.ietf.org/html/draft-ietf-scim-api-19).</span><span class="sxs-lookup"><span data-stu-id="db30a-105">Azure Active Directory (Azure AD) can automatically provision users and groups tooany application or identity store that is fronted by a web service with hello interface defined in hello [System for Cross-Domain Identity Management (SCIM) 2.0 protocol specification](https://tools.ietf.org/html/draft-ietf-scim-api-19).</span></span> <span data-ttu-id="db30a-106">Azure Active Directory můžete odesílat požadavky toocreate, upravit nebo odstranit přiřazené uživatelů a skupin toohello webové služby.</span><span class="sxs-lookup"><span data-stu-id="db30a-106">Azure Active Directory can send requests toocreate, modify, or delete assigned users and groups toohello web service.</span></span> <span data-ttu-id="db30a-107">Hello webové služby může překládat pak tyto požadavky do operací na úložiště identit cíl hello.</span><span class="sxs-lookup"><span data-stu-id="db30a-107">hello web service can then translate those requests into operations on hello target identity store.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="db30a-108">Společnost Microsoft doporučuje, která můžete spravovat Azure AD pomocí hello [centra pro správu Azure AD](https://aad.portal.azure.com) v hello hello portál Azure místo použití portálu Azure classic, kterou se odkazuje v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="db30a-108">Microsoft recommends that you manage Azure AD using hello [Azure AD admin center](https://aad.portal.azure.com) in hello Azure portal instead of using hello Azure classic portal referenced in this article.</span></span> 



<span data-ttu-id="db30a-109">![][0]
*Obrázek 1: Zřizování z úložiště identit Azure Active Directory tooan prostřednictvím webové služby*</span><span class="sxs-lookup"><span data-stu-id="db30a-109">![][0]
*Figure 1: Provisioning from Azure Active Directory tooan identity store via a web service*</span></span>

<span data-ttu-id="db30a-110">Tato funkce může používá ve spojení s hello "přineste vlastní aplikace" funkcích ve službě Azure AD tooenable jednotné přihlašování a automatické zřizování pro aplikace, které poskytují nebo jsou přední stěnou webovou službou SCIM uživatelů.</span><span class="sxs-lookup"><span data-stu-id="db30a-110">This capability can be used in conjunction with hello “bring your own app” capability in Azure AD tooenable single sign-on and automatic user provisioning for applications that provide or are fronted by a SCIM web service.</span></span>

<span data-ttu-id="db30a-111">Existují dva případy použití pro pomocí SCIM v Azure Active Directory:</span><span class="sxs-lookup"><span data-stu-id="db30a-111">There are two use cases for using SCIM in Azure Active Directory:</span></span>

* <span data-ttu-id="db30a-112">**Zřizování uživatelů a skupin tooapplications, které podporují SCIM** aplikace, které podporují SCIM 2.0 a používala tokeny nosičů OAuth pro ověřování pracuje s Azure AD bez konfigurace.</span><span class="sxs-lookup"><span data-stu-id="db30a-112">**Provisioning users and groups tooapplications that support SCIM** Applications that support SCIM 2.0 and use OAuth bearer tokens for authentication works with Azure AD without configuration.</span></span>
* <span data-ttu-id="db30a-113">**Vytvoření vlastního řešení zřizování pro aplikace, které podporují jiné zajišťování na základě rozhraní API** pro jiný SCIM aplikace, můžete vytvořit koncový bod tootranslate SCIM mezi koncový bod Azure AD SCIM hello a podporuje všechny rozhraní API hello aplikace pro zřizování uživatelů.</span><span class="sxs-lookup"><span data-stu-id="db30a-113">**Build your own provisioning solution for applications that support other API-based provisioning** For non-SCIM applications, you can create a SCIM endpoint tootranslate between hello Azure AD SCIM endpoint and any API hello application supports for user provisioning.</span></span> <span data-ttu-id="db30a-114">toohelp vyvíjet koncový bod SCIM, poskytujeme knihovny společné jazykové infrastruktury (CLI) společně s ukázky kódu, které ukazují, jak toodo zadejte koncový bod SCIM a převede SCIM zprávy.</span><span class="sxs-lookup"><span data-stu-id="db30a-114">toohelp you develop a SCIM endpoint, we provide Common Language Infrastructure (CLI) libraries along with code samples that show you how toodo provide a SCIM endpoint and translate SCIM messages.</span></span>  

## <a name="provisioning-users-and-groups-tooapplications-that-support-scim"></a><span data-ttu-id="db30a-115">Zřizování uživatelů a skupin tooapplications, které podporují SCIM</span><span class="sxs-lookup"><span data-stu-id="db30a-115">Provisioning users and groups tooapplications that support SCIM</span></span>
<span data-ttu-id="db30a-116">Azure AD může být nakonfigurované tooautomatically přiřazené zřizování uživatelů a skupin tooapplications, implementovat [systému pro správu identit napříč doménami 2 (SCIM)](https://tools.ietf.org/html/draft-ietf-scim-api-19) webové služby a přijímat tokeny nosičů OAuth pro ověřování .</span><span class="sxs-lookup"><span data-stu-id="db30a-116">Azure AD can be configured tooautomatically provision assigned users and groups tooapplications that implement a [System for Cross-domain Identity Management 2 (SCIM)](https://tools.ietf.org/html/draft-ietf-scim-api-19) web service and accept OAuth bearer tokens for authentication.</span></span> <span data-ttu-id="db30a-117">V rámci specifikace hello SCIM 2.0 aplikace musí splňovat tyto požadavky:</span><span class="sxs-lookup"><span data-stu-id="db30a-117">Within hello SCIM 2.0 specification, applications must meet these requirements:</span></span>

* <span data-ttu-id="db30a-118">Podporuje vytváření uživatelů nebo skupin, podle části 3.3 hello SCIM protokolu.</span><span class="sxs-lookup"><span data-stu-id="db30a-118">Supports creating users and/or groups, as per section 3.3 of hello SCIM protocol.</span></span>  
* <span data-ttu-id="db30a-119">Úprava uživatele nebo skupiny s požadavky patch podle části 3.5.2 hello SCIM protokol podporuje.</span><span class="sxs-lookup"><span data-stu-id="db30a-119">Supports modifying users and/or groups with patch requests as per section 3.5.2 of hello SCIM protocol.</span></span>  
* <span data-ttu-id="db30a-120">Podporuje načítání známých prostředků podle části 3.4.1 hello SCIM protokolu.</span><span class="sxs-lookup"><span data-stu-id="db30a-120">Supports retrieving a known resource as per section 3.4.1 of hello SCIM protocol.</span></span>  
* <span data-ttu-id="db30a-121">Podporuje dotazování uživatelů nebo skupin, podle části 3.4.2 hello SCIM protokolu.</span><span class="sxs-lookup"><span data-stu-id="db30a-121">Supports querying users and/or groups, as per section 3.4.2 of hello SCIM protocol.</span></span>  <span data-ttu-id="db30a-122">Ve výchozím nastavení externalId se zadají dotaz uživatelé a skupiny se zadají dotaz displayName.</span><span class="sxs-lookup"><span data-stu-id="db30a-122">By default, users are queried by externalId and groups are queried by displayName.</span></span>  
* <span data-ttu-id="db30a-123">Dotazování uživatele podle ID a správcem podle části 3.4.2 hello SCIM protokol podporuje.</span><span class="sxs-lookup"><span data-stu-id="db30a-123">Supports querying user by ID and by manager as per section 3.4.2 of hello SCIM protocol.</span></span>  
* <span data-ttu-id="db30a-124">Podporuje dotazování skupin podle ID a členové podle části 3.4.2 hello SCIM protokolu.</span><span class="sxs-lookup"><span data-stu-id="db30a-124">Supports querying groups by ID and by member as per section 3.4.2 of hello SCIM protocol.</span></span>  
* <span data-ttu-id="db30a-125">Přijímá tokeny nosičů OAuth pro autorizaci podle části 2.1 hello SCIM protokolu.</span><span class="sxs-lookup"><span data-stu-id="db30a-125">Accepts OAuth bearer tokens for authorization as per section 2.1 of hello SCIM protocol.</span></span>

<span data-ttu-id="db30a-126">Zeptejte se svého poskytovatele aplikace nebo v dokumentaci poskytovatele aplikace pro příkazy kompatibility s těmito požadavky.</span><span class="sxs-lookup"><span data-stu-id="db30a-126">Check with your application provider, or your application provider's documentation for statements of compatibility with these requirements.</span></span>

### <a name="getting-started"></a><span data-ttu-id="db30a-127">Začínáme</span><span class="sxs-lookup"><span data-stu-id="db30a-127">Getting started</span></span>
<span data-ttu-id="db30a-128">Aplikace, které podporují profil SCIM hello popsané v tomto článku může být připojené tooAzure služby Active Directory pomocí funkce hello "bez Galerie aplikace" v galerii aplikací Azure AD hello.</span><span class="sxs-lookup"><span data-stu-id="db30a-128">Applications that support hello SCIM profile described in this article can be connected tooAzure Active Directory using hello "non-gallery application" feature in hello Azure AD application gallery.</span></span> <span data-ttu-id="db30a-129">Po spuštění připojené, Azure AD proces synchronizace každých 20 minut, kde vyžádá aplikace hello SCIM koncový bod pro přiřazené uživatele a skupiny a vytvoří nebo upraví je podle toohello podrobnosti o přiřazení.</span><span class="sxs-lookup"><span data-stu-id="db30a-129">Once connected, Azure AD runs a synchronization process every 20 minutes where it queries hello application's SCIM endpoint for assigned users and groups, and creates or modifies them according toohello assignment details.</span></span>

<span data-ttu-id="db30a-130">**tooconnect aplikace, která podporuje SCIM:**</span><span class="sxs-lookup"><span data-stu-id="db30a-130">**tooconnect an application that supports SCIM:**</span></span>

1. <span data-ttu-id="db30a-131">Přihlaste se příliš[hello portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="db30a-131">Sign in too[hello Azure portal](https://portal.azure.com).</span></span> 
2. <span data-ttu-id="db30a-132">Procházet příliš ** Azure Active Directory > podnikové aplikace a vyberte **novou aplikaci > všechny > aplikace bez Galerie**.</span><span class="sxs-lookup"><span data-stu-id="db30a-132">Browse too**Azure Active Directory > Enterprise Applications, and select **New application > All > Non-gallery application**.</span></span>
3. <span data-ttu-id="db30a-133">Zadejte název pro vaši aplikaci a klikněte na tlačítko **přidat** ikonu toocreate objekt aplikace.</span><span class="sxs-lookup"><span data-stu-id="db30a-133">Enter a name for your application, and click **Add** icon toocreate an app object.</span></span>
    
  <span data-ttu-id="db30a-134">![][1]
  *Obrázek 2: Galerii aplikací Azure AD*</span><span class="sxs-lookup"><span data-stu-id="db30a-134">![][1]
*Figure 2: Azure AD application gallery*</span></span>
    
4. <span data-ttu-id="db30a-135">Na obrazovce výsledné hello vyberte hello **zřizování** kartě v levém sloupci hello.</span><span class="sxs-lookup"><span data-stu-id="db30a-135">In hello resulting screen, select hello **Provisioning** tab in hello left column.</span></span>
5. <span data-ttu-id="db30a-136">V hello **režimu zřizování** nabídce vyberte možnost **automatické**.</span><span class="sxs-lookup"><span data-stu-id="db30a-136">In hello **Provisioning Mode** menu, select **Automatic**.</span></span>
    
  <span data-ttu-id="db30a-137">![][2]
  *Obrázek 3: Konfigurace zřizování v hello portálu Azure*</span><span class="sxs-lookup"><span data-stu-id="db30a-137">![][2]
*Figure 3: Configuring provisioning in hello Azure portal*</span></span>
    
6. <span data-ttu-id="db30a-138">V hello **URL klienta** pole, zadejte adresu URL hello aplikace hello SCIM koncového bodu.</span><span class="sxs-lookup"><span data-stu-id="db30a-138">In hello **Tenant URL** field, enter hello URL of hello application's SCIM endpoint.</span></span> <span data-ttu-id="db30a-139">Příklad: https://api.contoso.com/scim/v2/</span><span class="sxs-lookup"><span data-stu-id="db30a-139">Example: https://api.contoso.com/scim/v2/</span></span>
7. <span data-ttu-id="db30a-140">Pokud koncový bod SCIM hello vyžaduje tokenu nosiče OAuth z vystavitele než Azure AD, tak kopie hello požadované tokenu nosiče OAuth do hello volitelné **tajný klíč tokenu** pole.</span><span class="sxs-lookup"><span data-stu-id="db30a-140">If hello SCIM endpoint requires an OAuth bearer token from an issuer other than Azure AD, then copy hello required OAuth bearer token into hello optional **Secret Token** field.</span></span> <span data-ttu-id="db30a-141">Pokud toto pole je prázdné, Azure AD zahrnuty tokenu nosiče OAuth, který je vydán z Azure AD s každou žádostí.</span><span class="sxs-lookup"><span data-stu-id="db30a-141">If this field is left blank, then Azure AD included an OAuth bearer token issued from Azure AD with each request.</span></span> <span data-ttu-id="db30a-142">Aplikace, které používají Azure AD jako zprostředkovatele identity můžete ověřit tento Azure AD-vydán token.</span><span class="sxs-lookup"><span data-stu-id="db30a-142">Apps that use Azure AD as an identity provider can validate this Azure AD -issued token.</span></span>
8. <span data-ttu-id="db30a-143">Klikněte na tlačítko hello **Test připojení** tlačítko toohave Azure Active Directory pokus o tooconnect toohello SCIM koncový bod.</span><span class="sxs-lookup"><span data-stu-id="db30a-143">Click hello **Test Connection** button toohave Azure Active Directory attempt tooconnect toohello SCIM endpoint.</span></span> <span data-ttu-id="db30a-144">Pokud hello pokusy selžou, zobrazí se informace o chybě.</span><span class="sxs-lookup"><span data-stu-id="db30a-144">If hello attempts fail, error information is displayed.</span></span>  
9. <span data-ttu-id="db30a-145">Pokud aplikace toohello tooconnect pokusy o hello úspěšné, pak klikněte na **Uložit** přihlašovací údaje správce toosave hello.</span><span class="sxs-lookup"><span data-stu-id="db30a-145">If hello attempts tooconnect toohello application succeed, then click **Save** toosave hello admin credentials.</span></span>
10. <span data-ttu-id="db30a-146">V hello **mapování** část, existují dvě sady vybrat mapování atributů: jeden pro uživatelské objekty a jeden pro objekty skupiny.</span><span class="sxs-lookup"><span data-stu-id="db30a-146">In hello **Mappings** section, there are two selectable sets of attribute mappings: one for user objects and one for group objects.</span></span> <span data-ttu-id="db30a-147">Vyberte každý jeden tooreview hello atributy, které jsou synchronizované z tooyour aplikace Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="db30a-147">Select each one tooreview hello attributes that are synchronized from Azure Active Directory tooyour app.</span></span> <span data-ttu-id="db30a-148">Hello atributy vybrán jako **párování** vlastnosti jsou použité toomatch hello uživatelů a skupin v aplikaci pro operace aktualizace.</span><span class="sxs-lookup"><span data-stu-id="db30a-148">hello attributes selected as **Matching** properties are used toomatch hello users and groups in your app for update operations.</span></span> <span data-ttu-id="db30a-149">Vyberte toocommit tlačítko hello uložit změny.</span><span class="sxs-lookup"><span data-stu-id="db30a-149">Select hello Save button toocommit any changes.</span></span>

    >[!NOTE]
    ><span data-ttu-id="db30a-150">Volitelně můžete zakázat synchronizaci objektů skupiny zakázáním hello "skupiny" mapování.</span><span class="sxs-lookup"><span data-stu-id="db30a-150">You can optionally disable syncing of group objects by disabling hello "groups" mapping.</span></span> 

11. <span data-ttu-id="db30a-151">V části **nastavení**, hello **oboru** pole definuje, které uživatele nebo skupiny jsou synchronizovány.</span><span class="sxs-lookup"><span data-stu-id="db30a-151">Under **Settings**, hello **Scope** field defines which users and or groups are synchronized.</span></span> <span data-ttu-id="db30a-152">Výběr "Synchronizace přiřadit pouze uživatelé a skupiny" (doporučeno) bude synchronizovat pouze uživatelé a skupiny přiřazené v hello **uživatelů a skupin** kartě.</span><span class="sxs-lookup"><span data-stu-id="db30a-152">Selecting "Sync only assigned users and groups" (recommended) will only sync users and groups assigned in hello **Users and groups** tab.</span></span>
12. <span data-ttu-id="db30a-153">Po dokončení konfiguraci změnit hello **Stav zřizování** příliš**na**.</span><span class="sxs-lookup"><span data-stu-id="db30a-153">Once your configuration is complete, change hello **Provisioning Status** too**On**.</span></span>
13. <span data-ttu-id="db30a-154">Klikněte na tlačítko **Uložit** toostart hello zřizování služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="db30a-154">Click **Save** toostart hello Azure AD provisioning service.</span></span> 
14. <span data-ttu-id="db30a-155">Pokud synchronizaci pouze přiřazenou uživatelů a skupin (doporučeno), se že hello tooselect **uživatelů a skupin** a přiřaďte hello uživatelů nebo skupin, které chcete toosync.</span><span class="sxs-lookup"><span data-stu-id="db30a-155">If syncing only assigned users and groups (recommended), be sure tooselect hello **Users and groups** tab and assign hello users and/or groups you wish toosync.</span></span>

<span data-ttu-id="db30a-156">Po zahájení hello počáteční synchronizaci, můžete hello **protokoly auditu** kartě toomonitor průběhu, který zobrazuje všechny akce prováděné hello zřizování služby ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="db30a-156">Once hello initial synchronization has started, you can use hello **Audit logs** tab toomonitor progress, which shows all actions performed by hello provisioning service on your app.</span></span> <span data-ttu-id="db30a-157">Další informace o zřizování hello Azure AD tooread jak protokolů najdete v tématu [zprávy o zřizování účtu automatické uživatele](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span><span class="sxs-lookup"><span data-stu-id="db30a-157">For more information on how tooread hello Azure AD provisioning logs, see [Reporting on automatic user account provisioning](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span></span>

>[!NOTE]
><span data-ttu-id="db30a-158">počáteční synchronizace Hello trvá déle tooperform než následné synchronizace, ke kterým dochází přibližně každých 20 minut, dokud se službou hello.</span><span class="sxs-lookup"><span data-stu-id="db30a-158">hello initial sync takes longer tooperform than subsequent syncs, which occur approximately every 20 minutes as long as hello service is running.</span></span> 


## <a name="building-your-own-provisioning-solution-for-any-application"></a><span data-ttu-id="db30a-159">Vytváření vlastní zřizování řešení pro všechny aplikace</span><span class="sxs-lookup"><span data-stu-id="db30a-159">Building your own provisioning solution for any application</span></span>
<span data-ttu-id="db30a-160">Vytvořením SCIM webová služba, která rozhraní s Azure Active Directory, můžete povolit jeden uživatel přihlašování a automatické zřizování prakticky jakékoli aplikace, která poskytuje zřizování rozhraní API REST nebo SOAP uživatelů.</span><span class="sxs-lookup"><span data-stu-id="db30a-160">By creating a SCIM web service that interfaces with Azure Active Directory, you can enable single sign-on and automatic user provisioning for virtually any application that provides a REST or SOAP user provisioning API.</span></span>

<span data-ttu-id="db30a-161">Zde je, jak to funguje:</span><span class="sxs-lookup"><span data-stu-id="db30a-161">Here’s how it works:</span></span>

1. <span data-ttu-id="db30a-162">Azure AD poskytuje společné jazykové infrastruktury knihovny s názvem [Microsoft.SystemForCrossDomainIdentityManagement](https://www.nuget.org/packages/Microsoft.SystemForCrossDomainIdentityManagement/).</span><span class="sxs-lookup"><span data-stu-id="db30a-162">Azure AD provides a common language infrastructure library named [Microsoft.SystemForCrossDomainIdentityManagement](https://www.nuget.org/packages/Microsoft.SystemForCrossDomainIdentityManagement/).</span></span> <span data-ttu-id="db30a-163">Systémoví integrátoři a vývojářům toocreate této knihovny a nasadit můžete použít koncový bod na základě SCIM webové služby lze připojit aplikace Azure AD tooany úložiště identit.</span><span class="sxs-lookup"><span data-stu-id="db30a-163">System integrators and developers can use this library toocreate and deploy a SCIM-based web service endpoint capable of connecting Azure AD tooany application’s identity store.</span></span>
2. <span data-ttu-id="db30a-164">Mapování se implementují ve hello webové služby toomap hello standardizované schématu toohello uživatele schéma uživatele a protokol vyžaduje aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="db30a-164">Mappings are implemented in hello web service toomap hello standardized user schema toohello user schema and protocol required by hello application.</span></span>
3. <span data-ttu-id="db30a-165">Adresa URL koncového bodu Hello je zaregistrován ve službě Azure AD jako součást vlastní aplikaci v galerii aplikací hello.</span><span class="sxs-lookup"><span data-stu-id="db30a-165">hello endpoint URL is registered in Azure AD as part of a custom application in hello application gallery.</span></span>
4. <span data-ttu-id="db30a-166">Uživatelé a skupiny přiřazené toothis aplikace ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="db30a-166">Users and groups are assigned toothis application in Azure AD.</span></span> <span data-ttu-id="db30a-167">Po přiřazení jsou vloženy do fronty toobe synchronizované toohello cílovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="db30a-167">Upon assignment, they are put into a queue toobe synchronized toohello target application.</span></span> <span data-ttu-id="db30a-168">Proces synchronizace Hello zpracování hello fronty spouští každých 20 minut.</span><span class="sxs-lookup"><span data-stu-id="db30a-168">hello synchronization process handling hello queue runs every 20 minutes.</span></span>

### <a name="code-samples"></a><span data-ttu-id="db30a-169">Ukázky kódů</span><span class="sxs-lookup"><span data-stu-id="db30a-169">Code Samples</span></span>
<span data-ttu-id="db30a-170">toomake to zpracovat snadnější, sadu [ukázky kódu jsou](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master) jsou k dispozici, vytvořit koncový bod webové služby SCIM a ukázka automatické zřizování.</span><span class="sxs-lookup"><span data-stu-id="db30a-170">toomake this process easier, a set of [code samples](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master) are provided that create a SCIM web service endpoint and demonstrate automatic provisioning.</span></span> <span data-ttu-id="db30a-171">Jeden ukázka je zprostředkovatele, který udržuje soubor s řádky představující uživatele a skupiny hodnot oddělených čárkami.</span><span class="sxs-lookup"><span data-stu-id="db30a-171">One sample is of a provider that maintains a file with rows of comma-separated values representing users and groups.</span></span>  <span data-ttu-id="db30a-172">je Hello další zprostředkovatele, který funguje u služby Amazon Web Services identita a správa přístupu hello.</span><span class="sxs-lookup"><span data-stu-id="db30a-172">hello other is of a provider that operates on hello Amazon Web Services Identity and Access Management service.</span></span>  

<span data-ttu-id="db30a-173">**Požadavky**</span><span class="sxs-lookup"><span data-stu-id="db30a-173">**Prerequisites**</span></span>

* <span data-ttu-id="db30a-174">Visual Studio 2013 nebo novější</span><span class="sxs-lookup"><span data-stu-id="db30a-174">Visual Studio 2013 or later</span></span>
* [<span data-ttu-id="db30a-175">Azure SDK pro .NET</span><span class="sxs-lookup"><span data-stu-id="db30a-175">Azure SDK for .NET</span></span>](https://azure.microsoft.com/downloads/)
* <span data-ttu-id="db30a-176">Počítače s Windows, který podporuje toobe framework 4.5 ASP.NET hello používá jako hello SCIM koncový bod.</span><span class="sxs-lookup"><span data-stu-id="db30a-176">Windows machine that supports hello ASP.NET framework 4.5 toobe used as hello SCIM endpoint.</span></span> <span data-ttu-id="db30a-177">Tento počítač musí být přístupné z cloudu hello</span><span class="sxs-lookup"><span data-stu-id="db30a-177">This machine must be accessible from hello cloud</span></span>
* [<span data-ttu-id="db30a-178">Předplatné Azure zkušební nebo licencovanou verzi Azure AD Premium</span><span class="sxs-lookup"><span data-stu-id="db30a-178">An Azure subscription with a trial or licensed version of Azure AD Premium</span></span>](https://azure.microsoft.com/services/active-directory/)
* <span data-ttu-id="db30a-179">Ukázka Amazon AWS Hello vyžaduje knihovny z hello [AWS nástrojů pro Visual Studio](http://docs.aws.amazon.com/AWSToolkitVS/latest/UserGuide/tkv_setup.html).</span><span class="sxs-lookup"><span data-stu-id="db30a-179">hello Amazon AWS sample requires libraries from hello [AWS Toolkit for Visual Studio](http://docs.aws.amazon.com/AWSToolkitVS/latest/UserGuide/tkv_setup.html).</span></span> <span data-ttu-id="db30a-180">Další informace najdete v tématu hello README hello ukázka je součástí souboru.</span><span class="sxs-lookup"><span data-stu-id="db30a-180">For more information, see hello README file included with hello sample.</span></span>

### <a name="getting-started"></a><span data-ttu-id="db30a-181">Začínáme</span><span class="sxs-lookup"><span data-stu-id="db30a-181">Getting Started</span></span>
<span data-ttu-id="db30a-182">Hello nejjednodušší způsob, jak tooimplement, které SCIM koncový bod, který může přijmout zřizování požadavky z Azure AD je toobuild a nasaďte hello ukázka kódu, který produkuje hello zřízené uživatelé tooa textový soubor s oddělovači (CSV) souboru.</span><span class="sxs-lookup"><span data-stu-id="db30a-182">hello easiest way tooimplement a SCIM endpoint that can accept provisioning requests from Azure AD is toobuild and deploy hello code sample that outputs hello provisioned users tooa comma-separated value (CSV) file.</span></span>

<span data-ttu-id="db30a-183">**toocreate SCIM koncový bod ukázka:**</span><span class="sxs-lookup"><span data-stu-id="db30a-183">**toocreate a sample SCIM endpoint:**</span></span>

1. <span data-ttu-id="db30a-184">Stáhněte si balíček ukázkový kód hello v [https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master)</span><span class="sxs-lookup"><span data-stu-id="db30a-184">Download hello code sample package at [https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master)</span></span>
2. <span data-ttu-id="db30a-185">Rozbalte balíček hello a umístěte ji na počítač se systémem Windows v umístění, jako je například C:\AzureAD-BYOA-Provisioning-Samples\.</span><span class="sxs-lookup"><span data-stu-id="db30a-185">Unzip hello package and place it on your Windows machine at a location such as C:\AzureAD-BYOA-Provisioning-Samples\.</span></span>
3. <span data-ttu-id="db30a-186">V této složce spusťte hello FileProvisioningAgent řešení v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="db30a-186">In this folder, launch hello FileProvisioningAgent solution in Visual Studio.</span></span>
4. <span data-ttu-id="db30a-187">Vyberte **nástroje > Správce balíčků knihoven > Konzola správce balíčků**a spusťte následující příkazy pro hello FileProvisioningAgent tooresolve hello řešení odkazy na projekt hello:</span><span class="sxs-lookup"><span data-stu-id="db30a-187">Select **Tools > Library Package Manager > Package Manager Console**, and execute hello following commands for hello FileProvisioningAgent project tooresolve hello solution references:</span></span>
  ```` 
   Install-Package Microsoft.SystemForCrossDomainIdentityManagement
   Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
   Install-Package Microsoft.Owin.Diagnostics
   Install-Package Microsoft.Owin.Host.SystemWeb
  ````
5. <span data-ttu-id="db30a-188">Sestavení projektu FileProvisioningAgent hello.</span><span class="sxs-lookup"><span data-stu-id="db30a-188">Build hello FileProvisioningAgent project.</span></span>
6. <span data-ttu-id="db30a-189">Spuštění aplikace příkazového řádku hello v systému Windows (jako správce) a použít hello **cd** příkaz toochange hello directory tooyour **\AzureAD-BYOA-Provisioning-Samples\ProvisioningAgent\bin\Debug** složky.</span><span class="sxs-lookup"><span data-stu-id="db30a-189">Launch hello Command Prompt application in Windows (as an Administrator), and use hello **cd** command toochange hello directory tooyour **\AzureAD-BYOA-Provisioning-Samples\ProvisioningAgent\bin\Debug** folder.</span></span>
7. <span data-ttu-id="db30a-190">Spusťte následující příkaz, nahraďte < adresa > hello IP adresy nebo názvu domény počítače Windows hello hello:</span><span class="sxs-lookup"><span data-stu-id="db30a-190">Run hello following command, replacing <ip-address> with hello IP address or domain name of hello Windows machine:</span></span>
  ````   
   FileAgnt.exe http://<ip-address>:9000 TargetFile.csv
  ````
8. <span data-ttu-id="db30a-191">V systému Windows v rámci **nastavení systému Windows > síť a Internet nastavení**, vyberte hello **brány Windows Firewall > Upřesnit nastavení**a vytvořit **příchozí pravidlo** , umožňuje příchozí přístup tooport 9000.</span><span class="sxs-lookup"><span data-stu-id="db30a-191">In Windows under **Windows Settings > Network & Internet Settings**, select hello **Windows Firewall > Advanced Settings**, and create an **Inbound Rule** that allows inbound access tooport 9000.</span></span>
9. <span data-ttu-id="db30a-192">Pokud je počítač Windows hello za směrovačem, hello směrovač potřebám toobe nakonfigurované tooperform překlad síťových přístup mezi její port 9000, je zveřejněné toohello internet a port 9000 na počítači Windows hello.</span><span class="sxs-lookup"><span data-stu-id="db30a-192">If hello Windows machine is behind a router, hello router needs toobe configured tooperform Network Access Translation between its port 9000 that is exposed toohello internet, and port 9000 on hello Windows machine.</span></span> <span data-ttu-id="db30a-193">To je potřeba pro Azure AD toobe možné tooaccess tento koncový bod v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="db30a-193">This is required for Azure AD toobe able tooaccess this endpoint in hello cloud.</span></span>

<span data-ttu-id="db30a-194">**tooregister hello ukázka SCIM koncový bod ve službě Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="db30a-194">**tooregister hello sample SCIM endpoint in Azure AD:**</span></span>

1. <span data-ttu-id="db30a-195">Přihlaste se příliš[hello portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="db30a-195">Sign in too[hello Azure portal](https://portal.azure.com).</span></span> 
2. <span data-ttu-id="db30a-196">Procházet příliš ** Azure Active Directory > podnikové aplikace a vyberte **novou aplikaci > všechny > aplikace bez Galerie**.</span><span class="sxs-lookup"><span data-stu-id="db30a-196">Browse too**Azure Active Directory > Enterprise Applications, and select **New application > All > Non-gallery application**.</span></span>
3. <span data-ttu-id="db30a-197">Zadejte název pro vaši aplikaci a klikněte na tlačítko **přidat** ikonu toocreate objekt aplikace.</span><span class="sxs-lookup"><span data-stu-id="db30a-197">Enter a name for your application, and click **Add** icon toocreate an app object.</span></span> <span data-ttu-id="db30a-198">objekt aplikace Hello vytvořený je určený toorepresent hello cílové aplikace by zřizování tooand implementace jeden pro přihlašování a ne jenom hello SCIM koncový bod.</span><span class="sxs-lookup"><span data-stu-id="db30a-198">hello application object created is intended toorepresent hello target app you would be provisioning tooand implementing single sign-on for, and not just hello SCIM endpoint.</span></span>
4. <span data-ttu-id="db30a-199">Na obrazovce výsledné hello vyberte hello **zřizování** kartě v levém sloupci hello.</span><span class="sxs-lookup"><span data-stu-id="db30a-199">In hello resulting screen, select hello **Provisioning** tab in hello left column.</span></span>
5. <span data-ttu-id="db30a-200">V hello **režimu zřizování** nabídce vyberte možnost **automatické**.</span><span class="sxs-lookup"><span data-stu-id="db30a-200">In hello **Provisioning Mode** menu, select **Automatic**.</span></span>
    
  <span data-ttu-id="db30a-201">![][2]
  *Obrázek 4: Konfigurace zřizování v hello portálu Azure*</span><span class="sxs-lookup"><span data-stu-id="db30a-201">![][2]
*Figure 4: Configuring provisioning in hello Azure portal*</span></span>
    
6. <span data-ttu-id="db30a-202">V hello **URL klienta** zadejte hello vystaven internetové adresy URL a port SCIM koncový bod.</span><span class="sxs-lookup"><span data-stu-id="db30a-202">In hello **Tenant URL** field, enter hello internet-exposed URL and port of your SCIM endpoint.</span></span> <span data-ttu-id="db30a-203">To by něco jako http://testmachine.contoso.com:9000 nebo http://<ip-address>:9000/, kde < adresa > je hello internet zveřejněné IP adresu.</span><span class="sxs-lookup"><span data-stu-id="db30a-203">This would be something like http://testmachine.contoso.com:9000 or http://<ip-address>:9000/, where <ip-address> is hello internet exposed IP address.</span></span>  
7. <span data-ttu-id="db30a-204">Pokud koncový bod SCIM hello vyžaduje tokenu nosiče OAuth z vystavitele než Azure AD, tak kopie hello požadované tokenu nosiče OAuth do hello volitelné **tajný klíč tokenu** pole.</span><span class="sxs-lookup"><span data-stu-id="db30a-204">If hello SCIM endpoint requires an OAuth bearer token from an issuer other than Azure AD, then copy hello required OAuth bearer token into hello optional **Secret Token** field.</span></span> <span data-ttu-id="db30a-205">Pokud toto pole je prázdné, bude obsahovat Azure AD tokenu nosiče OAuth, který je vydán z Azure AD s každou žádostí.</span><span class="sxs-lookup"><span data-stu-id="db30a-205">If this field is left blank, then Azure AD will include an OAuth bearer token issued from Azure AD with each request.</span></span> <span data-ttu-id="db30a-206">Aplikace, které používají Azure AD jako zprostředkovatele identity můžete ověřit tento Azure AD-vydán token.</span><span class="sxs-lookup"><span data-stu-id="db30a-206">Apps that use Azure AD as an identity provider can validate this Azure AD -issued token.</span></span>
8. <span data-ttu-id="db30a-207">Klikněte na tlačítko hello **Test připojení** tlačítko toohave Azure Active Directory pokus o tooconnect toohello SCIM koncový bod.</span><span class="sxs-lookup"><span data-stu-id="db30a-207">Click hello **Test Connection** button toohave Azure Active Directory attempt tooconnect toohello SCIM endpoint.</span></span> <span data-ttu-id="db30a-208">Pokud hello pokusy selžou, zobrazí se informace o chybě.</span><span class="sxs-lookup"><span data-stu-id="db30a-208">If hello attempts fail, error information is displayed.</span></span>  
9. <span data-ttu-id="db30a-209">Pokud aplikace toohello tooconnect pokusy o hello úspěšné, pak klikněte na **Uložit** přihlašovací údaje správce toosave hello.</span><span class="sxs-lookup"><span data-stu-id="db30a-209">If hello attempts tooconnect toohello application succeed, then click **Save** toosave hello admin credentials.</span></span>
10. <span data-ttu-id="db30a-210">V hello **mapování** část, existují dvě sady vybrat mapování atributů: jeden pro uživatelské objekty a jeden pro objekty skupiny.</span><span class="sxs-lookup"><span data-stu-id="db30a-210">In hello **Mappings** section, there are two selectable sets of attribute mappings: one for user objects and one for group objects.</span></span> <span data-ttu-id="db30a-211">Vyberte každý jeden tooreview hello atributy, které jsou synchronizované z tooyour aplikace Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="db30a-211">Select each one tooreview hello attributes that are synchronized from Azure Active Directory tooyour app.</span></span> <span data-ttu-id="db30a-212">Hello atributy vybrán jako **párování** vlastnosti jsou použité toomatch hello uživatelů a skupin v aplikaci pro operace aktualizace.</span><span class="sxs-lookup"><span data-stu-id="db30a-212">hello attributes selected as **Matching** properties are used toomatch hello users and groups in your app for update operations.</span></span> <span data-ttu-id="db30a-213">Vyberte toocommit tlačítko hello uložit změny.</span><span class="sxs-lookup"><span data-stu-id="db30a-213">Select hello Save button toocommit any changes.</span></span>
11. <span data-ttu-id="db30a-214">V části **nastavení**, hello **oboru** pole definuje, které uživatele nebo skupiny jsou synchronizovány.</span><span class="sxs-lookup"><span data-stu-id="db30a-214">Under **Settings**, hello **Scope** field defines which users and or groups are synchronized.</span></span> <span data-ttu-id="db30a-215">Výběr "Synchronizace přiřadit pouze uživatelé a skupiny" (doporučeno) bude synchronizovat pouze uživatelé a skupiny přiřazené v hello **uživatelů a skupin** kartě.</span><span class="sxs-lookup"><span data-stu-id="db30a-215">Selecting "Sync only assigned users and groups" (recommended) will only sync users and groups assigned in hello **Users and groups** tab.</span></span>
12. <span data-ttu-id="db30a-216">Po dokončení konfiguraci změnit hello **Stav zřizování** příliš**na**.</span><span class="sxs-lookup"><span data-stu-id="db30a-216">Once your configuration is complete, change hello **Provisioning Status** too**On**.</span></span>
13. <span data-ttu-id="db30a-217">Klikněte na tlačítko **Uložit** toostart hello zřizování služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="db30a-217">Click **Save** toostart hello Azure AD provisioning service.</span></span> 
14. <span data-ttu-id="db30a-218">Pokud synchronizaci pouze přiřazenou uživatelů a skupin (doporučeno), se že hello tooselect **uživatelů a skupin** a přiřaďte hello uživatelů nebo skupin, které chcete toosync.</span><span class="sxs-lookup"><span data-stu-id="db30a-218">If syncing only assigned users and groups (recommended), be sure tooselect hello **Users and groups** tab and assign hello users and/or groups you wish toosync.</span></span>

<span data-ttu-id="db30a-219">Po zahájení hello počáteční synchronizaci, můžete hello **protokoly auditu** kartě toomonitor průběhu, který zobrazuje všechny akce prováděné hello zřizování služby ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="db30a-219">Once hello initial synchronization has started, you can use hello **Audit logs** tab toomonitor progress, which shows all actions performed by hello provisioning service on your app.</span></span> <span data-ttu-id="db30a-220">Další informace o zřizování hello Azure AD tooread jak protokolů najdete v tématu [zprávy o zřizování účtu automatické uživatele](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span><span class="sxs-lookup"><span data-stu-id="db30a-220">For more information on how tooread hello Azure AD provisioning logs, see [Reporting on automatic user account provisioning](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span></span>

<span data-ttu-id="db30a-221">ověření hello ukázka Hello posledním krokem je tooopen hello TargetFile.csv souboru ve složce \AzureAD-BYOA-Provisioning-Samples\ProvisioningAgent\bin\Debug hello na počítač se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="db30a-221">hello final step in verifying hello sample is tooopen hello TargetFile.csv file in hello \AzureAD-BYOA-Provisioning-Samples\ProvisioningAgent\bin\Debug folder on your Windows machine.</span></span> <span data-ttu-id="db30a-222">Jakmile se spustí hello procesu zřizování, tento soubor zobrazuje hello podrobnosti o veškerém přiřazené a zřízení uživatelů a skupin.</span><span class="sxs-lookup"><span data-stu-id="db30a-222">Once hello provisioning process is run, this file shows hello details of all assigned and provisioned users and groups.</span></span>

### <a name="development-libraries"></a><span data-ttu-id="db30a-223">Vývojářské knihovny</span><span class="sxs-lookup"><span data-stu-id="db30a-223">Development libraries</span></span>
<span data-ttu-id="db30a-224">toodevelop vlastní webové služby, který splňuje specifikace SCIM toohello, nejdřív seznámit se s hello poskytované společností Microsoft toohelp urychlit proces vývoje hello následující knihovny:</span><span class="sxs-lookup"><span data-stu-id="db30a-224">toodevelop your own web service that conforms toohello SCIM specification, first familiarize yourself with hello following libraries provided by Microsoft toohelp accelerate hello development process:</span></span> 

1. <span data-ttu-id="db30a-225">Společné jazykové infrastruktury (CLI) knihovny jsou nabízena pro použití s jazyky na základě této infrastruktury, jako je například C#.</span><span class="sxs-lookup"><span data-stu-id="db30a-225">Common Language Infrastructure (CLI) libraries are offered for use with languages based on that infrastructure, such as C#.</span></span> <span data-ttu-id="db30a-226">Jedna z těchto knihoven [Microsoft.SystemForCrossDomainIdentityManagement.Service](https://www.nuget.org/packages/Microsoft.SystemForCrossDomainIdentityManagement/), deklaruje rozhraní, Microsoft.SystemForCrossDomainIdentityManagement.IProvider, znázorňuje následující obrázek hello: A Používání knihoven hello vývojáře by implementovat dané rozhraní s třídu, která může označovat, obecně jako zprostředkovatel.</span><span class="sxs-lookup"><span data-stu-id="db30a-226">One of those libraries, [Microsoft.SystemForCrossDomainIdentityManagement.Service](https://www.nuget.org/packages/Microsoft.SystemForCrossDomainIdentityManagement/), declares an interface, Microsoft.SystemForCrossDomainIdentityManagement.IProvider, shown in hello following illustration:  A developer using hello libraries would implement that interface with a class that may be referred to, generically, as a provider.</span></span> <span data-ttu-id="db30a-227">Hello knihovny povolte hello vývojáře toodeploy webové služby, který splňuje specifikace SCIM toohello.</span><span class="sxs-lookup"><span data-stu-id="db30a-227">hello libraries enable hello developer toodeploy a web service that conforms toohello SCIM specification.</span></span> <span data-ttu-id="db30a-228">Hello webová služba může být buď hostovaný v rámci služby IIS nebo libovolného spustitelného souboru Common Language Infrastructure sestavení.</span><span class="sxs-lookup"><span data-stu-id="db30a-228">hello web service can be either hosted within Internet Information Services, or any executable Common Language Infrastructure assembly.</span></span> <span data-ttu-id="db30a-229">Žádost je přeložit na zprostředkovatele toohello volání metody, které by být naprogramovaný tak podle hello vývojáře toooperate na některé úložiště identit.</span><span class="sxs-lookup"><span data-stu-id="db30a-229">Request is translated into calls toohello provider’s methods, which would be programmed by hello developer toooperate on some identity store.</span></span>
  
  ![][3]
  
2. <span data-ttu-id="db30a-230">[Obslužné rutiny trasy Express](http://expressjs.com/guide/routing.html) jsou k dispozici pro analýzu node.js požadavek objekty, které představují volání (jak je definována hello SCIM specification), tooa node.js webové služby.</span><span class="sxs-lookup"><span data-stu-id="db30a-230">[Express route handlers](http://expressjs.com/guide/routing.html) are available for parsing node.js request objects representing calls (as defined by hello SCIM specification), made tooa node.js web service.</span></span>   

### <a name="building-a-custom-scim-endpoint"></a><span data-ttu-id="db30a-231">Vytváření koncový bod vlastní SCIM</span><span class="sxs-lookup"><span data-stu-id="db30a-231">Building a Custom SCIM Endpoint</span></span>
<span data-ttu-id="db30a-232">Používání knihoven hello rozhraní příkazového řádku, vývojáře, kteří používají tyto knihovny hostování své služby v rámci všech spustitelný soubor sestavení Common Language Infrastructure, nebo Internetová informační služba.</span><span class="sxs-lookup"><span data-stu-id="db30a-232">Using hello CLI libraries, developers using those libraries can host their services within any executable Common Language Infrastructure assembly, or within Internet Information Services.</span></span> <span data-ttu-id="db30a-233">Tady je ukázkový kód pro hostování služby v rámci spustitelný soubor sestavení, na adrese hello http://localhost:9000:</span><span class="sxs-lookup"><span data-stu-id="db30a-233">Here is sample code for hosting a service within an executable assembly, at hello address http://localhost:9000:</span></span> 

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

<span data-ttu-id="db30a-234">Tato služba musí mít HTTP adresa a server ověřování certifikát z které hello kořenové certifikační autority je jedním z následujících hello:</span><span class="sxs-lookup"><span data-stu-id="db30a-234">This service must have an HTTP address and server authentication certificate of which hello root certification authority is one of hello following:</span></span> 

* <span data-ttu-id="db30a-235">CNNIC</span><span class="sxs-lookup"><span data-stu-id="db30a-235">CNNIC</span></span>
* <span data-ttu-id="db30a-236">Comodo</span><span class="sxs-lookup"><span data-stu-id="db30a-236">Comodo</span></span>
* <span data-ttu-id="db30a-237">CyberTrust</span><span class="sxs-lookup"><span data-stu-id="db30a-237">CyberTrust</span></span>
* <span data-ttu-id="db30a-238">DigiCert</span><span class="sxs-lookup"><span data-stu-id="db30a-238">DigiCert</span></span>
* <span data-ttu-id="db30a-239">GeoTrust</span><span class="sxs-lookup"><span data-stu-id="db30a-239">GeoTrust</span></span>
* <span data-ttu-id="db30a-240">GlobalSign</span><span class="sxs-lookup"><span data-stu-id="db30a-240">GlobalSign</span></span>
* <span data-ttu-id="db30a-241">Go Daddy</span><span class="sxs-lookup"><span data-stu-id="db30a-241">Go Daddy</span></span>
* <span data-ttu-id="db30a-242">VeriSign</span><span class="sxs-lookup"><span data-stu-id="db30a-242">Verisign</span></span>
* <span data-ttu-id="db30a-243">WoSign</span><span class="sxs-lookup"><span data-stu-id="db30a-243">WoSign</span></span>

<span data-ttu-id="db30a-244">Ověřovací certifikát serverů může být vázané tooa port na hostitele Windows hello nástroj shell sítě:</span><span class="sxs-lookup"><span data-stu-id="db30a-244">A server authentication certificate can be bound tooa port on a Windows host using hello network shell utility:</span></span> 

    netsh http add sslcert ipport=0.0.0.0:443 certhash=0000000000003ed9cd0c315bbb6dc1c08da5e6 appid={00112233-4455-6677-8899-AABBCCDDEEFF}  

<span data-ttu-id="db30a-245">Zde hello hodnota poskytnutá pro hello certhash argument je hello kryptografický otisk certifikátu hello, zatímco hello hodnota poskytnutá pro hello appid argument je libovolný identifikátor, globálně jedinečný.</span><span class="sxs-lookup"><span data-stu-id="db30a-245">Here, hello value provided for hello certhash argument is hello thumbprint of hello certificate, while hello value provided for hello appid argument is an arbitrary globally unique identifier.</span></span>  

<span data-ttu-id="db30a-246">toohost hello služby v rámci Internetové informační služby, vývojář by pomocí třídy v oboru názvů výchozí hello hello sestavení s názvem spuštění sestavit sestavení knihovny CLA kódu.</span><span class="sxs-lookup"><span data-stu-id="db30a-246">toohost hello service within Internet Information Services, a developer would build a CLA code library assembly with a class named Startup in hello default namespace of hello assembly.</span></span>  <span data-ttu-id="db30a-247">Zde je ukázka této třídy:</span><span class="sxs-lookup"><span data-stu-id="db30a-247">Here is a sample of such a class:</span></span> 

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

### <a name="handling-endpoint-authentication"></a><span data-ttu-id="db30a-248">Zpracování koncový bod ověřování</span><span class="sxs-lookup"><span data-stu-id="db30a-248">Handling endpoint authentication</span></span>
<span data-ttu-id="db30a-249">Žádosti z Azure Active Directory zahrnovat tokenu nosiče OAuth 2.0.</span><span class="sxs-lookup"><span data-stu-id="db30a-249">Requests from Azure Active Directory include an OAuth 2.0 bearer token.</span></span>   <span data-ttu-id="db30a-250">Každá žádost služby přijímající hello by měl ověřit hello vystavitele jako jménem klienta Azure Active Directory hello očekávání, pro přístup k toohello Azure Active Directory Graph webové služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="db30a-250">Any service receiving hello request should authenticate hello issuer as being Azure Active Directory on behalf of hello expected Azure Active Directory tenant, for access toohello Azure Active Directory Graph web service.</span></span>  <span data-ttu-id="db30a-251">V tokenu hello hello vystavitele identifikovaný deklaraci identity iss, například "iss": "https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/".</span><span class="sxs-lookup"><span data-stu-id="db30a-251">In hello token, hello issuer is identified by an iss claim, like, "iss":"https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/".</span></span>  <span data-ttu-id="db30a-252">V tomto příkladu hello základní adresa hello hodnoty deklarace identity, https://sts.windows.net, identifikuje Azure Active Directory jako hello vystavitele, zatímco hello relativní adresu segmentu, cbb1a5ac-f33b-45fa-9bf5-f37db0fed422, je jedinečný identifikátor hello Azure Active Klient Directory jménem které hello byl vydán token.</span><span class="sxs-lookup"><span data-stu-id="db30a-252">In this example, hello base address of hello claim value, https://sts.windows.net, identifies Azure Active Directory as hello issuer, while hello relative address segment, cbb1a5ac-f33b-45fa-9bf5-f37db0fed422, is a unique identifier of hello Azure Active Directory tenant on behalf of which hello token was issued.</span></span>  <span data-ttu-id="db30a-253">Pokud hello token vydán pro přístup k hello Azure Active Directory Graph webové služby, potom hello identifikátor této služby, 00000002-0000-0000-c000-000000000000, by měla být v hello hodnotu oblast hello token deklarace identity.</span><span class="sxs-lookup"><span data-stu-id="db30a-253">If hello token was issued for accessing hello Azure Active Directory Graph web service, then hello identifier of that service, 00000002-0000-0000-c000-000000000000, should be in hello value of hello token’s aud claim.</span></span>  

<span data-ttu-id="db30a-254">Vývojáři pomocí knihovny CLA hello od společnosti Microsoft pro vytváření SCIM služby můžete ověřování žádostí z Azure Active Directory pomocí balíček Microsoft.Owin.Security.ActiveDirectory hello pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="db30a-254">Developers using hello CLA libraries provided by Microsoft for building a SCIM service can authenticate requests from Azure Active Directory using hello Microsoft.Owin.Security.ActiveDirectory package by following these steps:</span></span> 

1. <span data-ttu-id="db30a-255">U zprostředkovatele implementujte hello Microsoft.SystemForCrossDomainIdentityManagement.IProvider.StartupBehavior vlastnost tak, že ho vrátit toobe metoda volána, když je spuštěna služba hello:</span><span class="sxs-lookup"><span data-stu-id="db30a-255">In a provider, implement hello Microsoft.SystemForCrossDomainIdentityManagement.IProvider.StartupBehavior property by having it return a method toobe called whenever hello service is started:</span></span> 

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

2. <span data-ttu-id="db30a-256">Přidejte následující kód toothat metoda toohave hello všechny žádosti o tooany koncových bodů služby hello ověřený jako opatřené tokenem vydaným službou Azure Active Directory jménem zadaného klienta, pro přístup k toohello Azure AD Graph webové služby:</span><span class="sxs-lookup"><span data-stu-id="db30a-256">Add hello following code toothat method toohave any request tooany of hello service’s endpoints authenticated as bearing a token issued by Azure Active Directory on behalf of a specified tenant, for access toohello Azure AD Graph web service:</span></span> 

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
        Tenant = "03F9FCBC-EA7B-46C2-8466-F81917F3C15E" // Substitute hello appropriate tenant’s 
                                                      // identifier for this one.  
      };

      applicationBuilder.UseWindowsAzureActiveDirectoryBearerAuthentication(authenticationOptions);
    }
  ````


## <a name="user-and-group-schema"></a><span data-ttu-id="db30a-257">Schéma uživatelů a skupin</span><span class="sxs-lookup"><span data-stu-id="db30a-257">User and group schema</span></span>
<span data-ttu-id="db30a-258">Azure Active Directory můžete zřídit dva typy prostředků tooSCIM webových služeb.</span><span class="sxs-lookup"><span data-stu-id="db30a-258">Azure Active Directory can provision two types of resources tooSCIM web services.</span></span>  <span data-ttu-id="db30a-259">Tyto typy prostředků jsou uživatelé a skupiny.</span><span class="sxs-lookup"><span data-stu-id="db30a-259">Those types of resources are users and groups.</span></span>  

<span data-ttu-id="db30a-260">Uživatel prostředky jsou označeny hello identifikátor schématu, urn: ietf:params:scim:schemas:extension:enterprise:2.0:User, který je součástí této specifikace protokolu: http://tools.ietf.org/html/draft-ietf-scim-core-schema.</span><span class="sxs-lookup"><span data-stu-id="db30a-260">User resources are identified by hello schema identifier, urn:ietf:params:scim:schemas:extension:enterprise:2.0:User, which is included in this protocol specification: http://tools.ietf.org/html/draft-ietf-scim-core-schema.</span></span>  <span data-ttu-id="db30a-261">Hello výchozí mapování atributů hello uživatelů ve službě Azure Active Directory toohello atributy urn: ietf:params:scim:schemas:extension:enterprise:2.0:User prostředků je uvedené v tabulce 1, níže.</span><span class="sxs-lookup"><span data-stu-id="db30a-261">hello default mapping of hello attributes of users in Azure Active Directory toohello attributes of urn:ietf:params:scim:schemas:extension:enterprise:2.0:User resources is provided in table 1, below.</span></span>  

<span data-ttu-id="db30a-262">Skupiny prostředků jsou identifikovány hello identifikátor schématu, http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group.</span><span class="sxs-lookup"><span data-stu-id="db30a-262">Group resources are identified by hello schema identifier, http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group.</span></span>  <span data-ttu-id="db30a-263">Tabulka 2 níže ukazuje hello výchozí mapování atributů hello skupin v Azure Active Directory toohello atributy http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group prostředků.</span><span class="sxs-lookup"><span data-stu-id="db30a-263">Table 2, below, shows hello default mapping of hello attributes of groups in Azure Active Directory toohello attributes of http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group resources.</span></span>  

### <a name="table-1-default-user-attribute-mapping"></a><span data-ttu-id="db30a-264">Tabulka 1: Výchozí mapování atributů uživatele</span><span class="sxs-lookup"><span data-stu-id="db30a-264">Table 1: Default user attribute mapping</span></span>
| <span data-ttu-id="db30a-265">Uživatele Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="db30a-265">Azure Active Directory user</span></span> | <span data-ttu-id="db30a-266">název urn: ietf:params:scim:schemas:extension:enterprise:2.0:User</span><span class="sxs-lookup"><span data-stu-id="db30a-266">urn:ietf:params:scim:schemas:extension:enterprise:2.0:User</span></span> |
| --- | --- |
| <span data-ttu-id="db30a-267">IsSoftDeleted</span><span class="sxs-lookup"><span data-stu-id="db30a-267">IsSoftDeleted</span></span> |<span data-ttu-id="db30a-268">Aktivní</span><span class="sxs-lookup"><span data-stu-id="db30a-268">active</span></span> |
| <span data-ttu-id="db30a-269">displayName</span><span class="sxs-lookup"><span data-stu-id="db30a-269">displayName</span></span> |<span data-ttu-id="db30a-270">displayName</span><span class="sxs-lookup"><span data-stu-id="db30a-270">displayName</span></span> |
| <span data-ttu-id="db30a-271">TelephoneNumber faxu</span><span class="sxs-lookup"><span data-stu-id="db30a-271">Facsimile-TelephoneNumber</span></span> |<span data-ttu-id="db30a-272">.value phoneNumbers [typ eq "fax"]</span><span class="sxs-lookup"><span data-stu-id="db30a-272">phoneNumbers[type eq "fax"].value</span></span> |
| <span data-ttu-id="db30a-273">givenName</span><span class="sxs-lookup"><span data-stu-id="db30a-273">givenName</span></span> |<span data-ttu-id="db30a-274">name.givenName</span><span class="sxs-lookup"><span data-stu-id="db30a-274">name.givenName</span></span> |
| <span data-ttu-id="db30a-275">pracovní funkce</span><span class="sxs-lookup"><span data-stu-id="db30a-275">jobTitle</span></span> |<span data-ttu-id="db30a-276">Název</span><span class="sxs-lookup"><span data-stu-id="db30a-276">title</span></span> |
| <span data-ttu-id="db30a-277">E-mailu</span><span class="sxs-lookup"><span data-stu-id="db30a-277">mail</span></span> |<span data-ttu-id="db30a-278">.value e-mailů [typ eq "práce"]</span><span class="sxs-lookup"><span data-stu-id="db30a-278">emails[type eq "work"].value</span></span> |
| <span data-ttu-id="db30a-279">mailNickname</span><span class="sxs-lookup"><span data-stu-id="db30a-279">mailNickname</span></span> |<span data-ttu-id="db30a-280">externalId</span><span class="sxs-lookup"><span data-stu-id="db30a-280">externalId</span></span> |
| <span data-ttu-id="db30a-281">Správce</span><span class="sxs-lookup"><span data-stu-id="db30a-281">manager</span></span> |<span data-ttu-id="db30a-282">Správce</span><span class="sxs-lookup"><span data-stu-id="db30a-282">manager</span></span> |
| <span data-ttu-id="db30a-283">mobilní</span><span class="sxs-lookup"><span data-stu-id="db30a-283">mobile</span></span> |<span data-ttu-id="db30a-284">.value phoneNumbers [eq typ "mobilní"]</span><span class="sxs-lookup"><span data-stu-id="db30a-284">phoneNumbers[type eq "mobile"].value</span></span> |
| <span data-ttu-id="db30a-285">objectId</span><span class="sxs-lookup"><span data-stu-id="db30a-285">objectId</span></span> |<span data-ttu-id="db30a-286">id</span><span class="sxs-lookup"><span data-stu-id="db30a-286">id</span></span> |
| <span data-ttu-id="db30a-287">PSČ</span><span class="sxs-lookup"><span data-stu-id="db30a-287">postalCode</span></span> |<span data-ttu-id="db30a-288">adresy [typ eq "práce"] .postalCode</span><span class="sxs-lookup"><span data-stu-id="db30a-288">addresses[type eq "work"].postalCode</span></span> |
| <span data-ttu-id="db30a-289">proxy adresy</span><span class="sxs-lookup"><span data-stu-id="db30a-289">proxy-Addresses</span></span> |<span data-ttu-id="db30a-290">[Zadejte eq "ostatní"] e-mailů. Hodnota</span><span class="sxs-lookup"><span data-stu-id="db30a-290">emails[type eq "other"].Value</span></span> |
| <span data-ttu-id="db30a-291">fyzický. doručení OfficeName</span><span class="sxs-lookup"><span data-stu-id="db30a-291">physical-Delivery-OfficeName</span></span> |<span data-ttu-id="db30a-292">adresy [Zadejte eq "ostatní"]. Formátu</span><span class="sxs-lookup"><span data-stu-id="db30a-292">addresses[type eq "other"].Formatted</span></span> |
| <span data-ttu-id="db30a-293">StreetAddress</span><span class="sxs-lookup"><span data-stu-id="db30a-293">streetAddress</span></span> |<span data-ttu-id="db30a-294">adresy [typ eq "práce"] .streetAddress</span><span class="sxs-lookup"><span data-stu-id="db30a-294">addresses[type eq "work"].streetAddress</span></span> |
| <span data-ttu-id="db30a-295">Příjmení</span><span class="sxs-lookup"><span data-stu-id="db30a-295">surname</span></span> |<span data-ttu-id="db30a-296">name.familyName</span><span class="sxs-lookup"><span data-stu-id="db30a-296">name.familyName</span></span> |
| <span data-ttu-id="db30a-297">telefonní číslo</span><span class="sxs-lookup"><span data-stu-id="db30a-297">telephone-Number</span></span> |<span data-ttu-id="db30a-298">.value phoneNumbers [typ eq "práce"]</span><span class="sxs-lookup"><span data-stu-id="db30a-298">phoneNumbers[type eq "work"].value</span></span> |
| <span data-ttu-id="db30a-299">PrincipalName uživatele</span><span class="sxs-lookup"><span data-stu-id="db30a-299">user-PrincipalName</span></span> |<span data-ttu-id="db30a-300">Uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="db30a-300">userName</span></span> |

### <a name="table-2-default-group-attribute-mapping"></a><span data-ttu-id="db30a-301">Tabulka 2: Výchozí skupiny mapování atributů</span><span class="sxs-lookup"><span data-stu-id="db30a-301">Table 2: Default group attribute mapping</span></span>
| <span data-ttu-id="db30a-302">Skupiny Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="db30a-302">Azure Active Directory group</span></span> | <span data-ttu-id="db30a-303">http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group</span><span class="sxs-lookup"><span data-stu-id="db30a-303">http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group</span></span> |
| --- | --- |
| <span data-ttu-id="db30a-304">displayName</span><span class="sxs-lookup"><span data-stu-id="db30a-304">displayName</span></span> |<span data-ttu-id="db30a-305">externalId</span><span class="sxs-lookup"><span data-stu-id="db30a-305">externalId</span></span> |
| <span data-ttu-id="db30a-306">E-mailu</span><span class="sxs-lookup"><span data-stu-id="db30a-306">mail</span></span> |<span data-ttu-id="db30a-307">.value e-mailů [typ eq "práce"]</span><span class="sxs-lookup"><span data-stu-id="db30a-307">emails[type eq "work"].value</span></span> |
| <span data-ttu-id="db30a-308">mailNickname</span><span class="sxs-lookup"><span data-stu-id="db30a-308">mailNickname</span></span> |<span data-ttu-id="db30a-309">displayName</span><span class="sxs-lookup"><span data-stu-id="db30a-309">displayName</span></span> |
| <span data-ttu-id="db30a-310">Členy</span><span class="sxs-lookup"><span data-stu-id="db30a-310">members</span></span> |<span data-ttu-id="db30a-311">Členy</span><span class="sxs-lookup"><span data-stu-id="db30a-311">members</span></span> |
| <span data-ttu-id="db30a-312">objectId</span><span class="sxs-lookup"><span data-stu-id="db30a-312">objectId</span></span> |<span data-ttu-id="db30a-313">id</span><span class="sxs-lookup"><span data-stu-id="db30a-313">id</span></span> |
| <span data-ttu-id="db30a-314">proxyAddresses</span><span class="sxs-lookup"><span data-stu-id="db30a-314">proxyAddresses</span></span> |<span data-ttu-id="db30a-315">[Zadejte eq "ostatní"] e-mailů. Hodnota</span><span class="sxs-lookup"><span data-stu-id="db30a-315">emails[type eq "other"].Value</span></span> |

## <a name="user-provisioning-and-de-provisioning"></a><span data-ttu-id="db30a-316">Zřizování uživatelů a jeho rušení</span><span class="sxs-lookup"><span data-stu-id="db30a-316">User provisioning and de-provisioning</span></span>
<span data-ttu-id="db30a-317">Následující obrázek ukazuje hello zprávy, že Azure Active Directory odešle životního cyklu tooa SCIM služby toomanage hello uživatele v jiné úložiště identit Hello.</span><span class="sxs-lookup"><span data-stu-id="db30a-317">hello following illustration shows hello messages that Azure Active Directory sends tooa SCIM service toomanage hello lifecycle of a user in another identity store.</span></span> <span data-ttu-id="db30a-318">Hello diagram také ukazuje, jak SCIM implementovaná pomocí rozhraní příkazového řádku knihovny hello poskytovat službu Microsoft pro vytvoření, že tyto služby převede tyto požadavky na volání metody toohello zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="db30a-318">hello diagram also shows how a SCIM service implemented using hello CLI libraries provided by Microsoft for building such services translate those requests into calls toohello methods of a provider.</span></span>  

<span data-ttu-id="db30a-319">![][4]
*Obrázek 5: Zřizování uživatelů a jeho rušení pořadí*</span><span class="sxs-lookup"><span data-stu-id="db30a-319">![][4]
*Figure 5: User provisioning and de-provisioning sequence*</span></span>

1. <span data-ttu-id="db30a-320">Azure dotazů služby Active Directory hello služby pro uživatele s hodnotou atributu externalId odpovídající hodnota atributu mailNickname hello uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="db30a-320">Azure Active Directory queries hello service for a user with an externalId attribute value matching hello mailNickname attribute value of a user in Azure AD.</span></span> <span data-ttu-id="db30a-321">Hello dotazu je vyjádřena jako žádost protokolu HTTP (Hypertext Transfer), jako je tento příklad, ve kterém jyoung je ukázka mailNickname uživatele v Azure Active Directory:</span><span class="sxs-lookup"><span data-stu-id="db30a-321">hello query is expressed as a Hypertext Transfer Protocol (HTTP) request such as this example, wherein jyoung is a sample of a mailNickname of a user in Azure Active Directory:</span></span> 
  ````
    GET https://.../scim/Users?filter=externalId eq jyoung HTTP/1.1
    Authorization: Bearer ...
  ````
  <span data-ttu-id="db30a-322">Pokud služba hello je vytvořená pomocí knihovny Common Language Infrastructure hello od společnosti Microsoft pro implementaci služby SCIM, žádost hello přeložit na volání toohello dotazu metoda zprostředkovatele služeb hello.</span><span class="sxs-lookup"><span data-stu-id="db30a-322">If hello service was built using hello Common Language Infrastructure libraries provided by Microsoft for implementing SCIM services, then hello request is translated into a call toohello Query method of hello service’s provider.</span></span>  <span data-ttu-id="db30a-323">Tady je hello podpis této metody:</span><span class="sxs-lookup"><span data-stu-id="db30a-323">Here is hello signature of that method:</span></span> 
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
  <span data-ttu-id="db30a-324">Zde je definice hello hello Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters rozhraní:</span><span class="sxs-lookup"><span data-stu-id="db30a-324">Here is hello definition of hello Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters interface:</span></span> 
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
  <span data-ttu-id="db30a-325">V následující ukázkový dotaz pro uživatele s danou hodnotou atributu externalId hello hello hodnoty hello argumentů předaných metoda dotazu toohello jsou:</span><span class="sxs-lookup"><span data-stu-id="db30a-325">In hello following sample of a query for a user with a given value for hello externalId attribute, values of hello arguments passed toohello Query method are:</span></span> 
  * <span data-ttu-id="db30a-326">Parametry. AlternateFilters.Count: 1</span><span class="sxs-lookup"><span data-stu-id="db30a-326">parameters.AlternateFilters.Count: 1</span></span>
  * <span data-ttu-id="db30a-327">Parametry. AlternateFilters.ElementAt(0). AttributePath: "externalId"</span><span class="sxs-lookup"><span data-stu-id="db30a-327">parameters.AlternateFilters.ElementAt(0).AttributePath: "externalId"</span></span>
  * <span data-ttu-id="db30a-328">Parametry. AlternateFilters.ElementAt(0). Porovnávací operátor: ComparisonOperator.Equals</span><span class="sxs-lookup"><span data-stu-id="db30a-328">parameters.AlternateFilters.ElementAt(0).ComparisonOperator: ComparisonOperator.Equals</span></span>
  * <span data-ttu-id="db30a-329">Parametry. AlternateFilter.ElementAt(0). ComparisonValue: "jyoung"</span><span class="sxs-lookup"><span data-stu-id="db30a-329">parameters.AlternateFilter.ElementAt(0).ComparisonValue: "jyoung"</span></span>
  * <span data-ttu-id="db30a-330">correlationIdentifier: System.Net.Http.HttpRequestMessage.GetOwinEnvironment["owin. ID žádosti"]</span><span class="sxs-lookup"><span data-stu-id="db30a-330">correlationIdentifier: System.Net.Http.HttpRequestMessage.GetOwinEnvironment["owin.RequestId"]</span></span> 

2. <span data-ttu-id="db30a-331">Pokud hello odpovědi tooa dotazu toohello webové služby pro uživatele s hodnotou atributu externalId odpovídající hodnota atributu mailNickname hello uživatele nevrátí žádné uživatele, pak Azure Active Directory požadavky tohoto poskytování služeb hello uživatele odpovídající toohello jednu v Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="db30a-331">If hello response tooa query toohello web service for a user with an externalId attribute value that matches hello mailNickname attribute value of a user does not return any users, then Azure Active Directory requests that hello service provision a user corresponding toohello one in Azure Active Directory.</span></span>  <span data-ttu-id="db30a-332">Tady je příklad této žádosti:</span><span class="sxs-lookup"><span data-stu-id="db30a-332">Here is an example of such a request:</span></span> 
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
  <span data-ttu-id="db30a-333">knihovny Common Language Infrastructure Hello od společnosti Microsoft pro implementaci služby SCIM by převede na volání toohello metodu Create zprostředkovatele služeb hello této žádosti.</span><span class="sxs-lookup"><span data-stu-id="db30a-333">hello Common Language Infrastructure libraries provided by Microsoft for implementing SCIM services would translate that request into a call toohello Create method of hello service’s provider.</span></span>  <span data-ttu-id="db30a-334">Metoda Create Hello má tento podpis:</span><span class="sxs-lookup"><span data-stu-id="db30a-334">hello Create method has this signature:</span></span> 
  ````
    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.Resource is defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  

    System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource> Create(
      Microsoft.SystemForCrossDomainIdentityManagement.Resource resource, 
      string correlationIdentifier);
  ````
  <span data-ttu-id="db30a-335">V žádosti o tooprovision uživatele hello hello prostředků argument hodnotu instanci hello Microsoft.SystemForCrossDomainIdentityManagement.</span><span class="sxs-lookup"><span data-stu-id="db30a-335">In a request tooprovision a user, hello value of hello resource argument is an instance of hello Microsoft.SystemForCrossDomainIdentityManagement.</span></span> <span data-ttu-id="db30a-336">Třída Core2EnterpriseUser definovaných v hello Microsoft.SystemForCrossDomainIdentityManagement.Schemas knihovny.</span><span class="sxs-lookup"><span data-stu-id="db30a-336">Core2EnterpriseUser class, defined in hello Microsoft.SystemForCrossDomainIdentityManagement.Schemas library.</span></span>  <span data-ttu-id="db30a-337">Pokud hello požadavek tooprovision hello uživatele úspěšné, pak hello implementace metody hello je očekávané tooreturn instanci hello Microsoft.SystemForCrossDomainIdentityManagement.</span><span class="sxs-lookup"><span data-stu-id="db30a-337">If hello request tooprovision hello user succeeds, then hello implementation of hello method is expected tooreturn an instance of hello Microsoft.SystemForCrossDomainIdentityManagement.</span></span> <span data-ttu-id="db30a-338">Třída Core2EnterpriseUser s hodnotou hello hello identifikátor vlastnosti nastavit toohello jedinečný identifikátor uživatele nově zřízeného hello.</span><span class="sxs-lookup"><span data-stu-id="db30a-338">Core2EnterpriseUser class, with hello value of hello Identifier property set toohello unique identifier of hello newly provisioned user.</span></span>  

3. <span data-ttu-id="db30a-339">tooupdate uživatele známé tooexist v úložišti identity přední stěnou pomocí SCIM, Azure Active Directory bude pokračovat podle požaduje hello aktuální stav daného uživatele ze služby hello s žádostí. například:</span><span class="sxs-lookup"><span data-stu-id="db30a-339">tooupdate a user known tooexist in an identity store fronted by an SCIM, Azure Active Directory proceeds by requesting hello current state of that user from hello service with a request such as:</span></span> 
  ````
    GET ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...
  ````
  <span data-ttu-id="db30a-340">Ve službě sestaven pomocí knihovny Common Language Infrastructure hello od společnosti Microsoft pro implementaci služby SCIM je požadavek hello přeložit na toohello volání metody načtení poskytovatele služeb hello.</span><span class="sxs-lookup"><span data-stu-id="db30a-340">In a service built using hello Common Language Infrastructure libraries provided by Microsoft for implementing SCIM services, hello request is translated into a call toohello Retrieve method of hello service’s provider.</span></span>  <span data-ttu-id="db30a-341">Tady je hello podpis metody načtení hello:</span><span class="sxs-lookup"><span data-stu-id="db30a-341">Here is hello signature of hello Retrieve method:</span></span> 
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
  <span data-ttu-id="db30a-342">V příkladu hello požadavek tooretrieve hello aktuálního stavu uživatele hello hodnoty vlastností hello objektu hello zadaný jako hodnota hello argumentu hello parametry jsou následující:</span><span class="sxs-lookup"><span data-stu-id="db30a-342">In hello example of a request tooretrieve hello current state of a user, hello values of hello properties of hello object provided as hello value of hello parameters argument are as follows:</span></span> 
  
  * <span data-ttu-id="db30a-343">Identifikátor: "54D382A4-2050-4C03-94D1-E769F1D15682"</span><span class="sxs-lookup"><span data-stu-id="db30a-343">Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"</span></span>
  * <span data-ttu-id="db30a-344">SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"</span><span class="sxs-lookup"><span data-stu-id="db30a-344">SchemaIdentifier: "urn:ietf:params:scim:schemas:extension:enterprise:2.0:User"</span></span>

4. <span data-ttu-id="db30a-345">Pokud atribut typu odkaz toobe aktualizován, pak Azure Active Directory dotazy hello služby toodetermine zda hello aktuální hodnotu atributu hello odkaz v úložišti identity hello přední stěnou pomocí služby hello již odpovídá hello hodnotu tohoto atributu. v Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="db30a-345">If a reference attribute is toobe updated, then Azure Active Directory queries hello service toodetermine whether or not hello current value of hello reference attribute in hello identity store fronted by hello service already matches hello value of that attribute in Azure Active Directory.</span></span> <span data-ttu-id="db30a-346">Pro uživatele hello pouze z které hello aktuální hodnota je dotazován tímto způsobem je atribut hello správce.</span><span class="sxs-lookup"><span data-stu-id="db30a-346">For users, hello only attribute of which hello current value is queried in this way is hello manager attribute.</span></span> <span data-ttu-id="db30a-347">Tady je příklad požadavek toodetermine zda atribut manager hello objektu konkrétní uživatel aktuálně má určitou hodnotu:</span><span class="sxs-lookup"><span data-stu-id="db30a-347">Here is an example of a request toodetermine whether hello manager attribute of a particular user object currently has a certain value:</span></span> 
  ````
    GET ~/scim/Users?filter=id eq 54D382A4-2050-4C03-94D1-E769F1D15682 and manager eq 2819c223-7f76-453a-919d-413861904646&attributes=id HTTP/1.1
    Authorization: Bearer ...
  ````
  <span data-ttu-id="db30a-348">Hello hodnota parametru dotazu hello atributy, id, označuje, která by splnila hello výraz zadaný jako hodnota hello parametru dotazu filtru hello pokud existuje objekt uživatele, pak služba hello je očekávané toorespond s urn: ietf:params:scim:schemas: Jádro: 2.0:User nebo urn: ietf:params:scim:schemas:extension:enterprise:2.0:User prostředků, včetně pouze hello hodnota atributu id tohoto zdroje.</span><span class="sxs-lookup"><span data-stu-id="db30a-348">hello value of hello attributes query parameter, id, signifies that if a user object exists that satisfies hello expression provided as hello value of hello filter query parameter, then hello service is expected toorespond with a urn:ietf:params:scim:schemas:core:2.0:User or urn:ietf:params:scim:schemas:extension:enterprise:2.0:User resource, including only hello value of that resource’s id attribute.</span></span>  <span data-ttu-id="db30a-349">Hello hodnotu hello **id** atribut je známý toohello žadatele.</span><span class="sxs-lookup"><span data-stu-id="db30a-349">hello value of hello **id** attribute is known toohello requestor.</span></span> <span data-ttu-id="db30a-350">Je součástí hello hodnota parametru dotazu filtru hello; účelem Hello žádostí o jeho je ve skutečnosti toorequest minimální reprezentace prostředku, který neodpovídajících výraz filtru hello jako indikaci, jestli všechny například objekt již existuje.</span><span class="sxs-lookup"><span data-stu-id="db30a-350">It is included in hello value of hello filter query parameter; hello purpose of asking for it is actually toorequest a minimal representation of a resource that satisfying hello filter expression as an indication of whether or not any such object exists.</span></span>   

  <span data-ttu-id="db30a-351">Pokud služba hello je vytvořená pomocí knihovny Common Language Infrastructure hello od společnosti Microsoft pro implementaci služby SCIM, žádost hello přeložit na volání toohello dotazu metoda zprostředkovatele služeb hello.</span><span class="sxs-lookup"><span data-stu-id="db30a-351">If hello service was built using hello Common Language Infrastructure libraries provided by Microsoft for implementing SCIM services, then hello request is translated into a call toohello Query method of hello service’s provider.</span></span> <span data-ttu-id="db30a-352">Hodnota Hello hello vlastnosti objektu hello zadaný jako hodnota hello argumentu hello parametry jsou následující:</span><span class="sxs-lookup"><span data-stu-id="db30a-352">hello value of hello properties of hello object provided as hello value of hello parameters argument are as follows:</span></span> 
  
  * <span data-ttu-id="db30a-353">Parametry. AlternateFilters.Count: 2</span><span class="sxs-lookup"><span data-stu-id="db30a-353">parameters.AlternateFilters.Count: 2</span></span>
  * <span data-ttu-id="db30a-354">Parametry. AlternateFilters.ElementAt(x). AttributePath: "id"</span><span class="sxs-lookup"><span data-stu-id="db30a-354">parameters.AlternateFilters.ElementAt(x).AttributePath: "id"</span></span>
  * <span data-ttu-id="db30a-355">Parametry. AlternateFilters.ElementAt(x). Porovnávací operátor: ComparisonOperator.Equals</span><span class="sxs-lookup"><span data-stu-id="db30a-355">parameters.AlternateFilters.ElementAt(x).ComparisonOperator: ComparisonOperator.Equals</span></span>
  * <span data-ttu-id="db30a-356">Parametry. AlternateFilter.ElementAt(x). ComparisonValue: "54D382A4-2050-4C03-94D1-E769F1D15682"</span><span class="sxs-lookup"><span data-stu-id="db30a-356">parameters.AlternateFilter.ElementAt(x).ComparisonValue: "54D382A4-2050-4C03-94D1-E769F1D15682"</span></span>
  * <span data-ttu-id="db30a-357">Parametry. AlternateFilters.ElementAt(y). AttributePath: "správce"</span><span class="sxs-lookup"><span data-stu-id="db30a-357">parameters.AlternateFilters.ElementAt(y).AttributePath: "manager"</span></span>
  * <span data-ttu-id="db30a-358">Parametry. AlternateFilters.ElementAt(y). Porovnávací operátor: ComparisonOperator.Equals</span><span class="sxs-lookup"><span data-stu-id="db30a-358">parameters.AlternateFilters.ElementAt(y).ComparisonOperator: ComparisonOperator.Equals</span></span>
  * <span data-ttu-id="db30a-359">Parametry. AlternateFilter.ElementAt(y). ComparisonValue: "2819c223-7f76-453a-919d-413861904646"</span><span class="sxs-lookup"><span data-stu-id="db30a-359">parameters.AlternateFilter.ElementAt(y).ComparisonValue: "2819c223-7f76-453a-919d-413861904646"</span></span>
  * <span data-ttu-id="db30a-360">Parametry. RequestedAttributePaths.ElementAt(0): "id"</span><span class="sxs-lookup"><span data-stu-id="db30a-360">parameters.RequestedAttributePaths.ElementAt(0): "id"</span></span>
  * <span data-ttu-id="db30a-361">Parametry. SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"</span><span class="sxs-lookup"><span data-stu-id="db30a-361">parameters.SchemaIdentifier: "urn:ietf:params:scim:schemas:extension:enterprise:2.0:User"</span></span>

  <span data-ttu-id="db30a-362">Zde hello hodnota indexu hello x může být 0 a hello hodnotu hello index y může být 1, nebo hello hodnota x může být 1 a hello hodnotu y, může být 0, v závislosti na hello pořadí hello výrazy hello parametr dotazu filtru.</span><span class="sxs-lookup"><span data-stu-id="db30a-362">Here, hello value of hello index x may be 0 and hello value of hello index y may be 1, or hello value of x may be 1 and hello value of y may be 0, depending on hello order of hello expressions of hello filter query parameter.</span></span>   

5. <span data-ttu-id="db30a-363">Tady je příklad požadavku z Azure Active Directory tooan SCIM služby tooupdate uživatele:</span><span class="sxs-lookup"><span data-stu-id="db30a-363">Here is an example of a request from Azure Active Directory tooan SCIM service tooupdate a user:</span></span> 
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
  <span data-ttu-id="db30a-364">knihovny Microsoft Common Language Infrastructure Hello pro implementaci služby SCIM by převede hello požadavek na volání toohello aktualizační metody poskytovatele služeb hello.</span><span class="sxs-lookup"><span data-stu-id="db30a-364">hello Microsoft Common Language Infrastructure libraries for implementing SCIM services would translate hello request into a call toohello Update method of hello service’s provider.</span></span> <span data-ttu-id="db30a-365">Tady je hello podpis hello metoda aktualizace:</span><span class="sxs-lookup"><span data-stu-id="db30a-365">Here is hello signature of hello Update method:</span></span> 
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
    <span data-ttu-id="db30a-366">V příkladu hello tooupdate požadavku uživatele má zadaný jako hodnota hello argumentu oprava hello objekt hello hodnoty těchto vlastností:</span><span class="sxs-lookup"><span data-stu-id="db30a-366">In hello example of a request tooupdate a user, hello object provided as hello value of hello patch argument has these property values:</span></span> 
  
  * <span data-ttu-id="db30a-367">ResourceIdentifier.Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"</span><span class="sxs-lookup"><span data-stu-id="db30a-367">ResourceIdentifier.Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"</span></span>
  * <span data-ttu-id="db30a-368">ResourceIdentifier.SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"</span><span class="sxs-lookup"><span data-stu-id="db30a-368">ResourceIdentifier.SchemaIdentifier: "urn:ietf:params:scim:schemas:extension:enterprise:2.0:User"</span></span>
  * <span data-ttu-id="db30a-369">(PatchRequest jako PatchRequest2). Operations.Count: 1</span><span class="sxs-lookup"><span data-stu-id="db30a-369">(PatchRequest as PatchRequest2).Operations.Count: 1</span></span>
  * <span data-ttu-id="db30a-370">(PatchRequest jako PatchRequest2). Operations.ElementAt(0). OperationName: OperationName.Add</span><span class="sxs-lookup"><span data-stu-id="db30a-370">(PatchRequest as PatchRequest2).Operations.ElementAt(0).OperationName: OperationName.Add</span></span>
  * <span data-ttu-id="db30a-371">(PatchRequest jako PatchRequest2). Operations.ElementAt(0). Path.AttributePath: "správce"</span><span class="sxs-lookup"><span data-stu-id="db30a-371">(PatchRequest as PatchRequest2).Operations.ElementAt(0).Path.AttributePath: "manager"</span></span>
  * <span data-ttu-id="db30a-372">(PatchRequest jako PatchRequest2). Operations.ElementAt(0). Value.Count: 1</span><span class="sxs-lookup"><span data-stu-id="db30a-372">(PatchRequest as PatchRequest2).Operations.ElementAt(0).Value.Count: 1</span></span>
  * <span data-ttu-id="db30a-373">(PatchRequest jako PatchRequest2). Operations.ElementAt(0). Value.ElementAt(0). Referenční dokumentace: http://.../scim/Users/2819c223-7f76-453a-919d-413861904646</span><span class="sxs-lookup"><span data-stu-id="db30a-373">(PatchRequest as PatchRequest2).Operations.ElementAt(0).Value.ElementAt(0).Reference: http://.../scim/Users/2819c223-7f76-453a-919d-413861904646</span></span>
  * <span data-ttu-id="db30a-374">(PatchRequest jako PatchRequest2). Operations.ElementAt(0). Value.ElementAt(0). Hodnota: 2819c223-7f76-453a-919d-413861904646</span><span class="sxs-lookup"><span data-stu-id="db30a-374">(PatchRequest as PatchRequest2).Operations.ElementAt(0).Value.ElementAt(0).Value: 2819c223-7f76-453a-919d-413861904646</span></span>

6. <span data-ttu-id="db30a-375">toode-provision uživatele z identity úložiště přední stěnou služba SCIM, Azure AD, jako odešle žádost:</span><span class="sxs-lookup"><span data-stu-id="db30a-375">toode-provision a user from an identity store fronted by an SCIM service, Azure AD sends a request such as:</span></span> 
  ````
    DELETE ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...
  ````
  <span data-ttu-id="db30a-376">Pokud služba hello je vytvořená pomocí knihovny Common Language Infrastructure hello od společnosti Microsoft pro implementaci služby SCIM, žádost hello přeložit na volání toohello metodu Delete zprostředkovatele služeb hello.</span><span class="sxs-lookup"><span data-stu-id="db30a-376">If hello service was built using hello Common Language Infrastructure libraries provided by Microsoft for implementing SCIM services, then hello request is translated into a call toohello Delete method of hello service’s provider.</span></span>   <span data-ttu-id="db30a-377">Tato metoda má tento podpis:</span><span class="sxs-lookup"><span data-stu-id="db30a-377">That method has this signature:</span></span> 
  ````
    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier, 
    // is defined in Microsoft.SystemForCrossDomainIdentityManagement.Protocol. 
    System.Threading.Tasks.Task Delete(
      Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier  
        resourceIdentifier, 
      string correlationIdentifier);
  ````
  <span data-ttu-id="db30a-378">zadaný jako hodnota hello argumentu resourceIdentifier hello Hello objekt má tyto hodnoty vlastností v příkladu hello žádost toode-provision uživatele:</span><span class="sxs-lookup"><span data-stu-id="db30a-378">hello object provided as hello value of hello resourceIdentifier argument has these property values in hello example of a request toode-provision a user:</span></span> 
  
  * <span data-ttu-id="db30a-379">ResourceIdentifier.Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"</span><span class="sxs-lookup"><span data-stu-id="db30a-379">ResourceIdentifier.Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"</span></span>
  * <span data-ttu-id="db30a-380">ResourceIdentifier.SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"</span><span class="sxs-lookup"><span data-stu-id="db30a-380">ResourceIdentifier.SchemaIdentifier: "urn:ietf:params:scim:schemas:extension:enterprise:2.0:User"</span></span>

## <a name="group-provisioning-and-de-provisioning"></a><span data-ttu-id="db30a-381">Zřizování skupiny a jeho rušení</span><span class="sxs-lookup"><span data-stu-id="db30a-381">Group provisioning and de-provisioning</span></span>
<span data-ttu-id="db30a-382">Následující obrázek ukazuje hello zprávy, že Azure AcD odešle životního cyklu tooa SCIM služby toomanage hello skupiny v jiné úložiště identit Hello.</span><span class="sxs-lookup"><span data-stu-id="db30a-382">hello following illustration shows hello messages that Azure AcD sends tooa SCIM service toomanage hello lifecycle of a group in another identity store.</span></span>  <span data-ttu-id="db30a-383">Tyto zprávy se liší od hello zprávy, která se týkají toousers třemi způsoby:</span><span class="sxs-lookup"><span data-stu-id="db30a-383">Those messages differ from hello messages pertaining toousers in three ways:</span></span> 

* <span data-ttu-id="db30a-384">Hello schéma skupiny prostředků je označený jako http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group.</span><span class="sxs-lookup"><span data-stu-id="db30a-384">hello schema of a group resource is identified as http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group.</span></span>  
* <span data-ttu-id="db30a-385">Požadavky, že tooretrieve skupiny stanovení tento atribut členy hello je toobe vyloučené z jakémukoli prostředku, zadaný v požadavku toohello odpovědi.</span><span class="sxs-lookup"><span data-stu-id="db30a-385">Requests tooretrieve groups stipulate that hello members attribute is toobe excluded from any resource provided in response toohello request.</span></span>  
* <span data-ttu-id="db30a-386">Toodetermine žádosti o tom, jestli atribut typu odkaz má určitou hodnotu se žádostí o hello členů atributu.</span><span class="sxs-lookup"><span data-stu-id="db30a-386">Requests toodetermine whether a reference attribute has a certain value are requests about hello members attribute.</span></span>  

<span data-ttu-id="db30a-387">![][5]
*Obrázek 6: Zřizování skupiny a jeho rušení pořadí*</span><span class="sxs-lookup"><span data-stu-id="db30a-387">![][5]
*Figure 6: Group provisioning and de-provisioning sequence*</span></span>

## <a name="related-articles"></a><span data-ttu-id="db30a-388">Související články</span><span class="sxs-lookup"><span data-stu-id="db30a-388">Related articles</span></span>
* [<span data-ttu-id="db30a-389">Rejstřík článků o správě aplikací ve službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="db30a-389">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
* [<span data-ttu-id="db30a-390">Automatizace zřizování uživatelů nebo jeho rušení tooSaaS aplikace</span><span class="sxs-lookup"><span data-stu-id="db30a-390">Automate User Provisioning/Deprovisioning tooSaaS Apps</span></span>](active-directory-saas-app-provisioning.md)
* [<span data-ttu-id="db30a-391">Přizpůsobení mapování atributů pro zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="db30a-391">Customizing Attribute Mappings for User Provisioning</span></span>](active-directory-saas-customizing-attribute-mappings.md)
* [<span data-ttu-id="db30a-392">Zapisují se výrazy pro mapování atributů</span><span class="sxs-lookup"><span data-stu-id="db30a-392">Writing Expressions for Attribute Mappings</span></span>](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [<span data-ttu-id="db30a-393">Filtry pro zřizování uživatelů oborů</span><span class="sxs-lookup"><span data-stu-id="db30a-393">Scoping Filters for User Provisioning</span></span>](active-directory-saas-scoping-filters.md)
* [<span data-ttu-id="db30a-394">Účet zřizování oznámení</span><span class="sxs-lookup"><span data-stu-id="db30a-394">Account Provisioning Notifications</span></span>](active-directory-saas-account-provisioning-notifications.md)
* [<span data-ttu-id="db30a-395">Seznam kurzů tooIntegrate aplikace SaaS</span><span class="sxs-lookup"><span data-stu-id="db30a-395">List of Tutorials on How tooIntegrate SaaS Apps</span></span>](active-directory-saas-tutorial-list.md)

<!--Image references-->
[0]: ./media/active-directory-scim-provisioning/scim-figure-1.PNG
[1]: ./media/active-directory-scim-provisioning/scim-figure-2a.PNG
[2]: ./media/active-directory-scim-provisioning/scim-figure-2b.PNG
[3]: ./media/active-directory-scim-provisioning/scim-figure-3.PNG
[4]: ./media/active-directory-scim-provisioning/scim-figure-4.PNG
[5]: ./media/active-directory-scim-provisioning/scim-figure-5.PNG
