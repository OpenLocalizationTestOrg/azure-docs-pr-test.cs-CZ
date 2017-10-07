---
title: "Kurz: Konfigurace Cerner střed pro zřizování automatické uživatelů s Azure Active Directory | Microsoft Docs"
description: "Zjistěte, jak Azure Active Directory tooautomatically tooconfigure zřídit soupisky tooa uživatelé v Cerner střed."
services: active-directory
documentationcenter: 
author: asmalser-msft
writer: asmalser-msft
manager: stevenpo
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/26/2017
ms.author: asmalser-msft
ms.openlocfilehash: e96da98e783d24e7f34ae924824f909eead75f54
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-cerner-central-for-automatic-user-provisioning"></a><span data-ttu-id="79c9b-103">Kurz: Konfigurace pro zřizování uživatelů automatické Cerner – střed</span><span class="sxs-lookup"><span data-stu-id="79c9b-103">Tutorial: Configuring Cerner Central for Automatic User Provisioning</span></span>

<span data-ttu-id="79c9b-104">cílem Hello tohoto kurzu je tooshow hello kroky nutné tooperform v Cerner střed a Azure AD tooautomatically zřídit a deaktivace zřízení uživatelských účtů z Azure AD tooa uživatele soupisky in – střed Cerner.</span><span class="sxs-lookup"><span data-stu-id="79c9b-104">hello objective of this tutorial is tooshow you hello steps you need tooperform in Cerner Central and Azure AD tooautomatically provision and de-provision user accounts from Azure AD tooa user roster in Cerner Central.</span></span> 


## <a name="prerequisites"></a><span data-ttu-id="79c9b-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="79c9b-105">Prerequisites</span></span>

<span data-ttu-id="79c9b-106">Hello scénáři uvedeném v tomto kurzu se předpokládá, že už máte hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="79c9b-106">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

*   <span data-ttu-id="79c9b-107">Klient služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="79c9b-107">An Azure Active Directory tenant</span></span>
*   <span data-ttu-id="79c9b-108">Klient Cerner – střed</span><span class="sxs-lookup"><span data-stu-id="79c9b-108">A Cerner Central tenant</span></span> 

> [!NOTE]
> <span data-ttu-id="79c9b-109">Azure Active Directory se integruje s centrální Cerner pomocí hello [SCIM](http://www.simplecloud.info/) protokolu.</span><span class="sxs-lookup"><span data-stu-id="79c9b-109">Azure Active Directory integrates with Cerner Central using hello [SCIM](http://www.simplecloud.info/) protocol.</span></span>

## <a name="assigning-users-toocerner-central"></a><span data-ttu-id="79c9b-110">Přiřazení uživatelů tooCerner – střed</span><span class="sxs-lookup"><span data-stu-id="79c9b-110">Assigning users tooCerner Central</span></span>

<span data-ttu-id="79c9b-111">Azure Active Directory používá koncept názvem "přiřazení" toodetermine uživatelů, kteří obdrželi přístup tooselected aplikace.</span><span class="sxs-lookup"><span data-stu-id="79c9b-111">Azure Active Directory uses a concept called "assignments" toodetermine which users should receive access tooselected apps.</span></span> <span data-ttu-id="79c9b-112">V kontextu hello zřizování účtu automatické uživatele jsou synchronizovány pouze hello uživatelů a skupin, které byly "přiřazeny" tooan aplikace ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="79c9b-112">In hello context of automatic user account provisioning, only hello users and groups that have been "assigned" tooan application in Azure AD are synchronized.</span></span> 

<span data-ttu-id="79c9b-113">Než nakonfigurujete a povolíte hello zřizování služby, byste měli rozhodnout, jaké uživatelů nebo skupin ve službě Azure AD představují hello uživatelů, kteří potřebují přístup k tooCerner střed.</span><span class="sxs-lookup"><span data-stu-id="79c9b-113">Before configuring and enabling hello provisioning service, you should decide what users and/or groups in Azure AD represent hello users who need access tooCerner Central.</span></span> <span data-ttu-id="79c9b-114">Jakmile se rozhodli, můžete přiřadit tyto uživatele tooCerner – střed podle pokynů hello tady:</span><span class="sxs-lookup"><span data-stu-id="79c9b-114">Once decided, you can assign these users tooCerner Central by following hello instructions here:</span></span>

[<span data-ttu-id="79c9b-115">Přiřadit uživatele nebo skupinu tooan firemní aplikace</span><span class="sxs-lookup"><span data-stu-id="79c9b-115">Assign a user or group tooan enterprise app</span></span>](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-toocerner-central"></a><span data-ttu-id="79c9b-116">Důležité tipy pro přiřazení uživatelů tooCerner – střed</span><span class="sxs-lookup"><span data-stu-id="79c9b-116">Important tips for assigning users tooCerner Central</span></span>

*   <span data-ttu-id="79c9b-117">Dále je doporučeno jednoho uživatele Azure AD přiřadit hello centrální tootest tooCerner zřizování konfigurace.</span><span class="sxs-lookup"><span data-stu-id="79c9b-117">It is recommended that a single Azure AD user be assigned tooCerner Central tootest hello provisioning configuration.</span></span> <span data-ttu-id="79c9b-118">Další uživatele nebo skupiny může být přiřazen později.</span><span class="sxs-lookup"><span data-stu-id="79c9b-118">Additional users and/or groups may be assigned later.</span></span>

* <span data-ttu-id="79c9b-119">Po dokončení pro jednoho uživatele počáteční testování Cerner střed doporučuje přiřazení hello celý seznam uživatelů určený tooaccess soupisky žádné Cerner řešení (nikoli pouze Cerner střed) toobe zřízený tooCerner na uživatele.</span><span class="sxs-lookup"><span data-stu-id="79c9b-119">Once initial testing is complete for a single user, Cerner Central recommends assigning hello entire list of users intended tooaccess any Cerner solution (not just Cerner Central) toobe provisioned tooCerner’s user roster.</span></span>  <span data-ttu-id="79c9b-120">Jiná řešení Cerner využívají tento seznam uživatelů v soupisky hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="79c9b-120">Other Cerner solutions leverage this list of users in hello user roster.</span></span>

*   <span data-ttu-id="79c9b-121">Při přiřazování uživatelů tooCerner – střed, je nutné vybrat hello **uživatele** role v dialogovém okně přiřazení hello.</span><span class="sxs-lookup"><span data-stu-id="79c9b-121">When assigning a user tooCerner Central, you must select hello **User** role in hello assignment dialog.</span></span> <span data-ttu-id="79c9b-122">Uživatelé s rolí "Výchozí přístup" hello jsou vyloučeny z zřizování.</span><span class="sxs-lookup"><span data-stu-id="79c9b-122">Users with hello "Default Access" role are excluded from provisioning.</span></span>


## <a name="configuring-user-provisioning-toocerner-central"></a><span data-ttu-id="79c9b-123">Konfigurace uživatele zřizování tooCerner – střed</span><span class="sxs-lookup"><span data-stu-id="79c9b-123">Configuring user provisioning tooCerner Central</span></span>

<span data-ttu-id="79c9b-124">Tato část vás provede připojením soupisky střed vaší služby Azure AD tooCerner uživatele pomocí rozhraní API pro zřizování na Cerner SCIM uživatelského účtu a konfiguraci hello zřizování služby toocreate, aktualizovat a zakázat přiřazené uživatele na základě účtů v Cerner – střed na přiřazení uživatelů a skupin ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="79c9b-124">This section guides you through connecting your Azure AD tooCerner Central’s User Roster using Cerner's SCIM user account provisioning API, and configuring hello provisioning service toocreate, update, and disable assigned user accounts in Cerner Central based on user and group assignment in Azure AD.</span></span>

> [!TIP]
> <span data-ttu-id="79c9b-125">Můžete také zvolit tooenabled na základě SAML jednotné přihlašování pro Cerner ústředí, hello pokynů uvedených v [portál Azure (https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="79c9b-125">You may also choose tooenabled SAML-based Single Sign-On for Cerner Central, following hello instructions provided in [Azure portal (https://portal.azure.com).</span></span> <span data-ttu-id="79c9b-126">Jednotné přihlašování se dá nakonfigurovat nezávisle na automatické zřizování, i když tyto dvě funkce vzájemně doplňují.</span><span class="sxs-lookup"><span data-stu-id="79c9b-126">Single sign-on can be configured independently of automatic provisioning, though these two features complement each other.</span></span> <span data-ttu-id="79c9b-127">Další informace najdete v tématu hello [Cerner střed jeden přihlašování kurzu](active-directory-saas-cernercentral-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="79c9b-127">For more information, see hello [Cerner Central single sign-on tutorial](active-directory-saas-cernercentral-tutorial.md).</span></span>


### <a name="tooconfigure-automatic-user-account-provisioning-toocerner-central-in-azure-ad"></a><span data-ttu-id="79c9b-128">tooconfigure automatické uživatelský účet zřizování tooCerner střed ve službě Azure AD:</span><span class="sxs-lookup"><span data-stu-id="79c9b-128">tooconfigure automatic user account provisioning tooCerner Central in Azure AD:</span></span>


<span data-ttu-id="79c9b-129">V pořadí tooprovision uživatelské účty tooCerner – střed budete potřebovat toorequest účet Cerner střed systému z Cerner a vygenerování tokenu nosiče OAuth, Azure AD můžete použít tooconnect tooCerner SCIM koncový bod.</span><span class="sxs-lookup"><span data-stu-id="79c9b-129">In order tooprovision user accounts tooCerner Central, you’ll need toorequest a Cerner Central system account from Cerner, and generate an OAuth bearer token that Azure AD can use tooconnect tooCerner's SCIM endpoint.</span></span> <span data-ttu-id="79c9b-130">Doporučujeme také, že integrace hello provést v prostředí izolovaného prostoru Cerner před nasazením tooproduction.</span><span class="sxs-lookup"><span data-stu-id="79c9b-130">It is also recommended that hello integration be performed in a Cerner sandbox environment before deploying tooproduction.</span></span>

1.  <span data-ttu-id="79c9b-131">prvním krokem Hello je osoby hello tooensure Správa hello Cerner a integrace Azure AD mají CernerCare účet, který je požadovaný tooaccess hello dokumentace nezbytné toocomplete hello pokyny.</span><span class="sxs-lookup"><span data-stu-id="79c9b-131">hello first step is tooensure hello people managing hello Cerner and Azure AD integration have a CernerCare account, which is required tooaccess hello documentation necessary toocomplete hello instructions.</span></span> <span data-ttu-id="79c9b-132">V případě potřeby používejte adresy URL hello níže toocreate CernerCare účty v každé příslušné prostředí.</span><span class="sxs-lookup"><span data-stu-id="79c9b-132">If necessary, use hello URLs below toocreate CernerCare accounts in each applicable environment.</span></span>

   * <span data-ttu-id="79c9b-133">Izolovaný prostor: https://sandboxcernercare.com/accounts/create</span><span class="sxs-lookup"><span data-stu-id="79c9b-133">Sandbox:  https://sandboxcernercare.com/accounts/create</span></span>

   * <span data-ttu-id="79c9b-134">Produkční: https://cernercare.com/accounts/create</span><span class="sxs-lookup"><span data-stu-id="79c9b-134">Production:  https://cernercare.com/accounts/create</span></span>  

2.  <span data-ttu-id="79c9b-135">Účet systému dále musí být vytvořen pro Azure AD.</span><span class="sxs-lookup"><span data-stu-id="79c9b-135">Next, a system account must be created for Azure AD.</span></span> <span data-ttu-id="79c9b-136">Použijte pokyny hello níže toorequest účet systému pro vaše prostředí izolovaného prostoru a provozní.</span><span class="sxs-lookup"><span data-stu-id="79c9b-136">Use hello instructions below toorequest a System Account for your sandbox and production environments.</span></span>

   * <span data-ttu-id="79c9b-137">Pokyny: https://wiki.ucern.com/display/CernerCentral/Requesting+A+System+Account</span><span class="sxs-lookup"><span data-stu-id="79c9b-137">Instructions:  https://wiki.ucern.com/display/CernerCentral/Requesting+A+System+Account</span></span>

   * <span data-ttu-id="79c9b-138">Izolovaný prostor: https://sandboxcernercentral.com/system-accounts/</span><span class="sxs-lookup"><span data-stu-id="79c9b-138">Sandbox: https://sandboxcernercentral.com/system-accounts/</span></span>

   * <span data-ttu-id="79c9b-139">Produkční: https://cernercentral.com/system-accounts/</span><span class="sxs-lookup"><span data-stu-id="79c9b-139">Production:  https://cernercentral.com/system-accounts/</span></span>

3.  <span data-ttu-id="79c9b-140">V dalším kroku generovat tokenu nosiče OAuth pro všechny systémové účty.</span><span class="sxs-lookup"><span data-stu-id="79c9b-140">Next, generate an OAuth bearer token for each of your system accounts.</span></span> <span data-ttu-id="79c9b-141">toodo se hello postupujte podle pokynů níže.</span><span class="sxs-lookup"><span data-stu-id="79c9b-141">toodo this, follow hello instructions below.</span></span>

   * <span data-ttu-id="79c9b-142">Pokyny: https://wiki.ucern.com/display/public/reference/Accessing+Cerner%27s+Web+Services+Using+A+System+Account+Bearer+Token</span><span class="sxs-lookup"><span data-stu-id="79c9b-142">Instructions:  https://wiki.ucern.com/display/public/reference/Accessing+Cerner%27s+Web+Services+Using+A+System+Account+Bearer+Token</span></span>

   * <span data-ttu-id="79c9b-143">Izolovaný prostor: https://sandboxcernercentral.com/system-accounts/</span><span class="sxs-lookup"><span data-stu-id="79c9b-143">Sandbox: https://sandboxcernercentral.com/system-accounts/</span></span>

   * <span data-ttu-id="79c9b-144">Produkční: https://cernercentral.com/system-accounts/</span><span class="sxs-lookup"><span data-stu-id="79c9b-144">Production:  https://cernercentral.com/system-accounts/</span></span>

4. <span data-ttu-id="79c9b-145">Nakonec můžete ID sféry soupisky tooacquire uživatelů pro obě hello izolovaného prostoru a provozní prostředí v Cerner toocomplete hello konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="79c9b-145">Finally, you need tooacquire User Roster Realm IDs for both hello sandbox and production environments in Cerner toocomplete hello configuration.</span></span> <span data-ttu-id="79c9b-146">Informace o tom tooacquire, najdete v tématu: https://wiki.ucern.com/display/public/reference/Publishing+Identity+Data+Using+SCIM.</span><span class="sxs-lookup"><span data-stu-id="79c9b-146">For information on how tooacquire this, see: https://wiki.ucern.com/display/public/reference/Publishing+Identity+Data+Using+SCIM.</span></span> 

5. <span data-ttu-id="79c9b-147">Teď můžete konfigurovat Azure AD tooprovision uživatelské účty tooCerner.</span><span class="sxs-lookup"><span data-stu-id="79c9b-147">Now you can configure Azure AD tooprovision user accounts tooCerner.</span></span> <span data-ttu-id="79c9b-148">Přihlaste se toohello [portál Azure](https://portal.azure.com)a vyhledejte toohello **Azure Active Directory > podnikové aplikace > všechny aplikace** části.</span><span class="sxs-lookup"><span data-stu-id="79c9b-148">Sign in toohello [Azure portal](https://portal.azure.com), and browse toohello **Azure Active Directory > Enterprise Apps > All applications**  section.</span></span>

6. <span data-ttu-id="79c9b-149">Pokud jste již nakonfigurovali Cerner střed pro jednotné přihlašování, vyhledejte instanci Cerner střed pomocí hello vyhledávací pole.</span><span class="sxs-lookup"><span data-stu-id="79c9b-149">If you have already configured Cerner Central for single sign-on, search for your instance of Cerner Central using hello search field.</span></span> <span data-ttu-id="79c9b-150">Jinak vyberte možnost **přidat** a vyhledejte **Cerner střed** v galerii aplikací hello.</span><span class="sxs-lookup"><span data-stu-id="79c9b-150">Otherwise, select **Add** and search for **Cerner Central** in hello application gallery.</span></span> <span data-ttu-id="79c9b-151">Vyberte Cerner střed z výsledků hledání hello a přidejte ji tooyour seznam aplikací.</span><span class="sxs-lookup"><span data-stu-id="79c9b-151">Select Cerner Central from hello search results, and add it tooyour list of applications.</span></span>

7.  <span data-ttu-id="79c9b-152">Vyberte instanci Cerner střed a pak vyberte hello **zřizování** kartě.</span><span class="sxs-lookup"><span data-stu-id="79c9b-152">Select your instance of Cerner Central, then select hello **Provisioning** tab.</span></span>

8.  <span data-ttu-id="79c9b-153">Sada hello **režimu zřizování** příliš**automatické**.</span><span class="sxs-lookup"><span data-stu-id="79c9b-153">Set hello **Provisioning Mode** too**Automatic**.</span></span>

   ![Střed Cerner zřizování](./media/active-directory-saas-cernercentral-provisioning-tutorial/Cerner.PNG)

9.  <span data-ttu-id="79c9b-155">Vyplňte následující pole v části hello **přihlašovací údaje správce**:</span><span class="sxs-lookup"><span data-stu-id="79c9b-155">Fill in hello following fields under **Admin Credentials**:</span></span>

   * <span data-ttu-id="79c9b-156">V hello **URL klienta** pole, zadejte adresu URL ve formátu hello níže nahraďte "User-soupisky-sféry-ID" s ID sféry hello jste získali v kroku #4.</span><span class="sxs-lookup"><span data-stu-id="79c9b-156">In hello **Tenant URL** field, enter a URL in hello format below, replacing "User-Roster-Realm-ID" with hello realm ID you acquired in step #4.</span></span>

> <span data-ttu-id="79c9b-157">Izolovaný prostor: https://user-roster-api.sandboxcernercentral.com/scim/v1/Realms/User-Roster-Realm-ID/</span><span class="sxs-lookup"><span data-stu-id="79c9b-157">Sandbox: https://user-roster-api.sandboxcernercentral.com/scim/v1/Realms/User-Roster-Realm-ID/</span></span> 

> <span data-ttu-id="79c9b-158">Produkční: https://user-roster-api.cernercentral.com/scim/v1/Realms/User-Roster-Realm-ID/</span><span class="sxs-lookup"><span data-stu-id="79c9b-158">Production: https://user-roster-api.cernercentral.com/scim/v1/Realms/User-Roster-Realm-ID/</span></span> 

   * <span data-ttu-id="79c9b-159">V hello **tajný klíč tokenu** pole, zadejte tokenu nosiče OAuth hello jste vygenerovali v kroku #3 a klikněte na **Test připojení**.</span><span class="sxs-lookup"><span data-stu-id="79c9b-159">In hello **Secret Token** field, enter hello OAuth bearer token you generated in step #3 and click **Test Connection**.</span></span>

   * <span data-ttu-id="79c9b-160">Měli byste vidět úspěšné oznámení na straně ÚETUChcete hello portálu.</span><span class="sxs-lookup"><span data-stu-id="79c9b-160">You should see a success notification on hello upper­right side of your portal.</span></span>

10. <span data-ttu-id="79c9b-161">Zadejte hello e-mailovou adresu uživatele nebo skupiny, který by měly dostávat oznámení zřizování Chyba v hello **e-mailové oznámení** pole a zaškrtněte políčko hello níže.</span><span class="sxs-lookup"><span data-stu-id="79c9b-161">Enter hello email address of a person or group who should receive provisioning error notifications in hello **Notification Email** field, and check hello checkbox below.</span></span>

11. <span data-ttu-id="79c9b-162">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="79c9b-162">Click **Save**.</span></span> 

12. <span data-ttu-id="79c9b-163">V hello **mapování atributů** , projděte si hello uživatele a skupiny toobe atributy synchronizované z Azure AD tooCerner střed.</span><span class="sxs-lookup"><span data-stu-id="79c9b-163">In hello **Attribute Mappings** section, review hello user and group attributes toobe synchronized from Azure AD tooCerner Central.</span></span> <span data-ttu-id="79c9b-164">Hello atributy vybrán jako **párování** vlastnosti jsou použité toomatch hello uživatelské účty a skupiny v centrální Cerner pro operace aktualizace.</span><span class="sxs-lookup"><span data-stu-id="79c9b-164">hello attributes selected as **Matching** properties are used toomatch hello user accounts and groups in Cerner Central for update operations.</span></span> <span data-ttu-id="79c9b-165">Vyberte toocommit tlačítko hello uložit změny.</span><span class="sxs-lookup"><span data-stu-id="79c9b-165">Select hello Save button toocommit any changes.</span></span>

13. <span data-ttu-id="79c9b-166">tooenable hello zřizování služby Azure AD pro Cerner ústředí, změna hello **Stav zřizování** příliš**na** v hello **nastavení** části</span><span class="sxs-lookup"><span data-stu-id="79c9b-166">tooenable hello Azure AD provisioning service for Cerner Central, change hello **Provisioning Status** too**On** in hello **Settings** section</span></span>

14. <span data-ttu-id="79c9b-167">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="79c9b-167">Click **Save**.</span></span> 

<span data-ttu-id="79c9b-168">Tím se spustí počáteční synchronizaci hello všechny uživatele nebo skupiny přiřazené tooCerner střed v části Uživatelé a skupiny hello.</span><span class="sxs-lookup"><span data-stu-id="79c9b-168">This starts hello initial synchronization of any users and/or groups assigned tooCerner Central in hello Users and Groups section.</span></span> <span data-ttu-id="79c9b-169">počáteční synchronizace Hello trvá déle tooperform než následné synchronizace, ke kterým dochází přibližně každých 20 minut, dokud běží hello zřizování služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="79c9b-169">hello initial sync takes longer tooperform than subsequent syncs, which occur approximately every 20 minutes as long as hello Azure AD provisioning service is running.</span></span> <span data-ttu-id="79c9b-170">Můžete použít hello **podrobnosti synchronizace** části toomonitor průběh a postupujte podle pokynů odkazy tooprovisioning aktivity sestavy, které popisují všechny akce prováděné hello zřizování služby ve vaší aplikaci Cerner střed.</span><span class="sxs-lookup"><span data-stu-id="79c9b-170">You can use hello **Synchronization Details** section toomonitor progress and follow links tooprovisioning activity reports, which describe all actions performed by hello provisioning service on your Cerner Central app.</span></span>

<span data-ttu-id="79c9b-171">Další informace o zřizování hello Azure AD tooread jak protokolů najdete v tématu [zprávy o zřizování účtu automatické uživatele](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span><span class="sxs-lookup"><span data-stu-id="79c9b-171">For more information on how tooread hello Azure AD provisioning logs, see [Reporting on automatic user account provisioning](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="79c9b-172">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="79c9b-172">Additional resources</span></span>

* [<span data-ttu-id="79c9b-173">Střed Cerner: Publikování dat identity pomocí služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="79c9b-173">Cerner Central: Publishing identity data using Azure AD</span></span>](https://wiki.ucern.com/display/public/reference/Publishing+Identity+Data+Using+Azure+AD)
* [<span data-ttu-id="79c9b-174">Kurz: Konfigurace Cerner střed pro jednotné přihlašování s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="79c9b-174">Tutorial: Configuring Cerner Central for single sign-on with Azure Active Directory</span></span>](active-directory-saas-cernercentral-tutorial.md)
* [<span data-ttu-id="79c9b-175">Správa uživatelů zřizování účtu pro podnikové aplikace</span><span class="sxs-lookup"><span data-stu-id="79c9b-175">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-enterprise-apps-manage-provisioning.md)
* [<span data-ttu-id="79c9b-176">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="79c9b-176">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a><span data-ttu-id="79c9b-177">Další kroky</span><span class="sxs-lookup"><span data-stu-id="79c9b-177">Next steps</span></span>
* <span data-ttu-id="79c9b-178">[Zjistěte, jak tooreview protokoly a sestavy get na zřizování aktivity](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span><span class="sxs-lookup"><span data-stu-id="79c9b-178">[Learn how tooreview logs and get reports on provisioning activity](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span></span>
