---
title: "Kurz: Konfigurace Cerner střed pro zřizování automatické uživatelů s Azure Active Directory | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat služby Azure Active Directory tak, aby automaticky zřizovat uživatelům soupisky in – střed Cerner."
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
ms.openlocfilehash: 84613b7f8d7bd031d492a62da0bc53be96ac45a3
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="tutorial-configuring-cerner-central-for-automatic-user-provisioning"></a><span data-ttu-id="9f34e-103">Kurz: Konfigurace pro zřizování uživatelů automatické Cerner – střed</span><span class="sxs-lookup"><span data-stu-id="9f34e-103">Tutorial: Configuring Cerner Central for Automatic User Provisioning</span></span>

<span data-ttu-id="9f34e-104">Cílem tohoto kurzu je tak, aby zobrazovalo kroky, které je třeba provést v Cerner střed a Azure AD a automaticky zřizovat a zrušte zřídit uživatelské účty ze služby Azure AD pro uživatele soupisky in – střed Cerner.</span><span class="sxs-lookup"><span data-stu-id="9f34e-104">The objective of this tutorial is to show you the steps you need to perform in Cerner Central and Azure AD to automatically provision and de-provision user accounts from Azure AD to a user roster in Cerner Central.</span></span> 


## <a name="prerequisites"></a><span data-ttu-id="9f34e-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="9f34e-105">Prerequisites</span></span>

<span data-ttu-id="9f34e-106">Scénář uvedených v tomto kurzu se předpokládá, že už máte následující položky:</span><span class="sxs-lookup"><span data-stu-id="9f34e-106">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

*   <span data-ttu-id="9f34e-107">Klient služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9f34e-107">An Azure Active Directory tenant</span></span>
*   <span data-ttu-id="9f34e-108">Klient Cerner – střed</span><span class="sxs-lookup"><span data-stu-id="9f34e-108">A Cerner Central tenant</span></span> 

> [!NOTE]
> <span data-ttu-id="9f34e-109">Azure Active Directory se integruje s centrální Cerner pomocí [SCIM](http://www.simplecloud.info/) protokolu.</span><span class="sxs-lookup"><span data-stu-id="9f34e-109">Azure Active Directory integrates with Cerner Central using the [SCIM](http://www.simplecloud.info/) protocol.</span></span>

## <a name="assigning-users-to-cerner-central"></a><span data-ttu-id="9f34e-110">Přiřazení uživatelů k Cerner – střed</span><span class="sxs-lookup"><span data-stu-id="9f34e-110">Assigning users to Cerner Central</span></span>

<span data-ttu-id="9f34e-111">Azure Active Directory používá koncept označované jako "úlohy" k určení uživatelů, kteří obdrželi přístup k vybrané aplikace.</span><span class="sxs-lookup"><span data-stu-id="9f34e-111">Azure Active Directory uses a concept called "assignments" to determine which users should receive access to selected apps.</span></span> <span data-ttu-id="9f34e-112">V kontextu uživatele automatické zřizování účtu jsou synchronizovány pouze uživatelé a skupiny, které byly "přiřazeny" aplikace ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9f34e-112">In the context of automatic user account provisioning, only the users and groups that have been "assigned" to an application in Azure AD are synchronized.</span></span> 

<span data-ttu-id="9f34e-113">Než nakonfigurujete a povolíte službu zřizování, byste měli rozhodnout, jaké uživatelů nebo skupin ve službě Azure AD představují uživatele, kteří potřebují přístup k Cerner střed.</span><span class="sxs-lookup"><span data-stu-id="9f34e-113">Before configuring and enabling the provisioning service, you should decide what users and/or groups in Azure AD represent the users who need access to Cerner Central.</span></span> <span data-ttu-id="9f34e-114">Jakmile se rozhodli, můžete přiřadit těmto uživatelům Cerner střed podle pokynů tady:</span><span class="sxs-lookup"><span data-stu-id="9f34e-114">Once decided, you can assign these users to Cerner Central by following the instructions here:</span></span>

[<span data-ttu-id="9f34e-115">Přiřazení uživatele nebo skupiny do aplikace enterprise</span><span class="sxs-lookup"><span data-stu-id="9f34e-115">Assign a user or group to an enterprise app</span></span>](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-cerner-central"></a><span data-ttu-id="9f34e-116">Důležité tipy pro přiřazování uživatelů do Cerner – střed</span><span class="sxs-lookup"><span data-stu-id="9f34e-116">Important tips for assigning users to Cerner Central</span></span>

*   <span data-ttu-id="9f34e-117">Dále je doporučeno jednoho uživatele Azure AD pro centrální Cerner přidělí otestovat konfiguraci zřizování.</span><span class="sxs-lookup"><span data-stu-id="9f34e-117">It is recommended that a single Azure AD user be assigned to Cerner Central to test the provisioning configuration.</span></span> <span data-ttu-id="9f34e-118">Další uživatele nebo skupiny může být přiřazen později.</span><span class="sxs-lookup"><span data-stu-id="9f34e-118">Additional users and/or groups may be assigned later.</span></span>

* <span data-ttu-id="9f34e-119">Po dokončení pro jednoho uživatele počáteční testování Cerner střed doporučuje přiřazení celý seznam uživatelů mají přístup k žádným řešením Cerner (ne jenom Cerner střed) být zřízená soupisky Cerner na uživatele.</span><span class="sxs-lookup"><span data-stu-id="9f34e-119">Once initial testing is complete for a single user, Cerner Central recommends assigning the entire list of users intended to access any Cerner solution (not just Cerner Central) to be provisioned to Cerner’s user roster.</span></span>  <span data-ttu-id="9f34e-120">Jiná řešení Cerner využívají tento seznam uživatelů v soupisky uživatele.</span><span class="sxs-lookup"><span data-stu-id="9f34e-120">Other Cerner solutions leverage this list of users in the user roster.</span></span>

*   <span data-ttu-id="9f34e-121">Při přiřazení uživatele k Cerner – střed, je nutné vybrat **uživatele** role v dialogovém okně přiřazení.</span><span class="sxs-lookup"><span data-stu-id="9f34e-121">When assigning a user to Cerner Central, you must select the **User** role in the assignment dialog.</span></span> <span data-ttu-id="9f34e-122">Uživatelé s rolí "Výchozí přístup" jsou vyloučeny z zřizování.</span><span class="sxs-lookup"><span data-stu-id="9f34e-122">Users with the "Default Access" role are excluded from provisioning.</span></span>


## <a name="configuring-user-provisioning-to-cerner-central"></a><span data-ttu-id="9f34e-123">Konfiguraci zřizování uživatelů k Cerner – střed</span><span class="sxs-lookup"><span data-stu-id="9f34e-123">Configuring user provisioning to Cerner Central</span></span>

<span data-ttu-id="9f34e-124">Tato část vás provede připojení k centrální Cerner soupisky uživatele pomocí na Cerner SCIM uživatelského účtu zřizování rozhraní API služby Azure AD a konfiguraci zřizování službu, kterou chcete vytvořit, aktualizovat a zakázat přiřazené uživatele na základě účtů v Cerner – střed přiřazení uživatelů a skupin ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9f34e-124">This section guides you through connecting your Azure AD to Cerner Central’s User Roster using Cerner's SCIM user account provisioning API, and configuring the provisioning service to create, update, and disable assigned user accounts in Cerner Central based on user and group assignment in Azure AD.</span></span>

> [!TIP]
> <span data-ttu-id="9f34e-125">Také můžete povolit na základě SAML jednotné přihlašování pro centrální Cerner, postupujte podle pokynů uvedených v [portál Azure (https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="9f34e-125">You may also choose to enabled SAML-based Single Sign-On for Cerner Central, following the instructions provided in [Azure portal (https://portal.azure.com).</span></span> <span data-ttu-id="9f34e-126">Jednotné přihlašování se dá nakonfigurovat nezávisle na automatické zřizování, i když tyto dvě funkce vzájemně doplňují.</span><span class="sxs-lookup"><span data-stu-id="9f34e-126">Single sign-on can be configured independently of automatic provisioning, though these two features complement each other.</span></span> <span data-ttu-id="9f34e-127">Další informace najdete v tématu [Cerner střed jeden přihlašování kurzu](active-directory-saas-cernercentral-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="9f34e-127">For more information, see the [Cerner Central single sign-on tutorial](active-directory-saas-cernercentral-tutorial.md).</span></span>


### <a name="to-configure-automatic-user-account-provisioning-to-cerner-central-in-azure-ad"></a><span data-ttu-id="9f34e-128">Konfigurace automatického účet zřizování uživatelů na střed Cerner ve službě Azure AD:</span><span class="sxs-lookup"><span data-stu-id="9f34e-128">To configure automatic user account provisioning to Cerner Central in Azure AD:</span></span>


<span data-ttu-id="9f34e-129">Chcete-li zřídit uživatelských účtů do centrální Cerner, budete muset požádat Cerner Cerner střed systémový účet a vygenerování tokenu nosiče OAuth, Azure AD můžete použít k připojení ke koncovému bodu na Cerner SCIM.</span><span class="sxs-lookup"><span data-stu-id="9f34e-129">In order to provision user accounts to Cerner Central, you’ll need to request a Cerner Central system account from Cerner, and generate an OAuth bearer token that Azure AD can use to connect to Cerner's SCIM endpoint.</span></span> <span data-ttu-id="9f34e-130">Doporučujeme také provést integraci v prostředí izolovaného prostoru Cerner před nasazením do produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="9f34e-130">It is also recommended that the integration be performed in a Cerner sandbox environment before deploying to production.</span></span>

1.  <span data-ttu-id="9f34e-131">V prvním kroku je zajistit osoby Správa Cerner a integrace Azure AD mají CernerCare účet, který je nutné pro přístup k potřebné k dokončení podle pokynů v dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="9f34e-131">The first step is to ensure the people managing the Cerner and Azure AD integration have a CernerCare account, which is required to access the documentation necessary to complete the instructions.</span></span> <span data-ttu-id="9f34e-132">V případě potřeby použijte níže uvedené adresy URL můžete vytvořit účty CernerCare v každé příslušné prostředí.</span><span class="sxs-lookup"><span data-stu-id="9f34e-132">If necessary, use the URLs below to create CernerCare accounts in each applicable environment.</span></span>

   * <span data-ttu-id="9f34e-133">Izolovaný prostor: https://sandboxcernercare.com/accounts/create</span><span class="sxs-lookup"><span data-stu-id="9f34e-133">Sandbox:  https://sandboxcernercare.com/accounts/create</span></span>

   * <span data-ttu-id="9f34e-134">Produkční: https://cernercare.com/accounts/create</span><span class="sxs-lookup"><span data-stu-id="9f34e-134">Production:  https://cernercare.com/accounts/create</span></span>  

2.  <span data-ttu-id="9f34e-135">Účet systému dále musí být vytvořen pro Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9f34e-135">Next, a system account must be created for Azure AD.</span></span> <span data-ttu-id="9f34e-136">Pomocí níže uvedené pokyny o systémový účet pro vaše prostředí izolovaného prostoru a provozní.</span><span class="sxs-lookup"><span data-stu-id="9f34e-136">Use the instructions below to request a System Account for your sandbox and production environments.</span></span>

   * <span data-ttu-id="9f34e-137">Pokyny: https://wiki.ucern.com/display/CernerCentral/Requesting+A+System+Account</span><span class="sxs-lookup"><span data-stu-id="9f34e-137">Instructions:  https://wiki.ucern.com/display/CernerCentral/Requesting+A+System+Account</span></span>

   * <span data-ttu-id="9f34e-138">Izolovaný prostor: https://sandboxcernercentral.com/system-accounts/</span><span class="sxs-lookup"><span data-stu-id="9f34e-138">Sandbox: https://sandboxcernercentral.com/system-accounts/</span></span>

   * <span data-ttu-id="9f34e-139">Produkční: https://cernercentral.com/system-accounts/</span><span class="sxs-lookup"><span data-stu-id="9f34e-139">Production:  https://cernercentral.com/system-accounts/</span></span>

3.  <span data-ttu-id="9f34e-140">V dalším kroku generovat tokenu nosiče OAuth pro všechny systémové účty.</span><span class="sxs-lookup"><span data-stu-id="9f34e-140">Next, generate an OAuth bearer token for each of your system accounts.</span></span> <span data-ttu-id="9f34e-141">Chcete-li to provést, postupujte podle pokynů níže.</span><span class="sxs-lookup"><span data-stu-id="9f34e-141">To do this, follow the instructions below.</span></span>

   * <span data-ttu-id="9f34e-142">Pokyny: https://wiki.ucern.com/display/public/reference/Accessing+Cerner%27s+Web+Services+Using+A+System+Account+Bearer+Token</span><span class="sxs-lookup"><span data-stu-id="9f34e-142">Instructions:  https://wiki.ucern.com/display/public/reference/Accessing+Cerner%27s+Web+Services+Using+A+System+Account+Bearer+Token</span></span>

   * <span data-ttu-id="9f34e-143">Izolovaný prostor: https://sandboxcernercentral.com/system-accounts/</span><span class="sxs-lookup"><span data-stu-id="9f34e-143">Sandbox: https://sandboxcernercentral.com/system-accounts/</span></span>

   * <span data-ttu-id="9f34e-144">Produkční: https://cernercentral.com/system-accounts/</span><span class="sxs-lookup"><span data-stu-id="9f34e-144">Production:  https://cernercentral.com/system-accounts/</span></span>

4. <span data-ttu-id="9f34e-145">Nakonec budete muset získat ID sféry soupisky uživatelů pro izolovaný prostor i produkčním prostředí v Cerner k dokončení konfigurace.</span><span class="sxs-lookup"><span data-stu-id="9f34e-145">Finally, you need to acquire User Roster Realm IDs for both the sandbox and production environments in Cerner to complete the configuration.</span></span> <span data-ttu-id="9f34e-146">Informace o tom, jak získat to najdete v tématu: https://wiki.ucern.com/display/public/reference/Publishing+Identity+Data+Using+SCIM.</span><span class="sxs-lookup"><span data-stu-id="9f34e-146">For information on how to acquire this, see: https://wiki.ucern.com/display/public/reference/Publishing+Identity+Data+Using+SCIM.</span></span> 

5. <span data-ttu-id="9f34e-147">Teď můžete konfigurovat Azure AD pro zřízení uživatelských účtů do Cerner.</span><span class="sxs-lookup"><span data-stu-id="9f34e-147">Now you can configure Azure AD to provision user accounts to Cerner.</span></span> <span data-ttu-id="9f34e-148">Přihlaste se k [portál Azure](https://portal.azure.com)a přejděte do **Azure Active Directory > podnikové aplikace > všechny aplikace** části.</span><span class="sxs-lookup"><span data-stu-id="9f34e-148">Sign in to the [Azure portal](https://portal.azure.com), and browse to the **Azure Active Directory > Enterprise Apps > All applications**  section.</span></span>

6. <span data-ttu-id="9f34e-149">Pokud jste již nakonfigurovali Cerner střed pro jednotné přihlašování, vyhledejte instanci Cerner střed pomocí pole hledání.</span><span class="sxs-lookup"><span data-stu-id="9f34e-149">If you have already configured Cerner Central for single sign-on, search for your instance of Cerner Central using the search field.</span></span> <span data-ttu-id="9f34e-150">Jinak vyberte možnost **přidat** a vyhledejte **Cerner střed** v galerii aplikací.</span><span class="sxs-lookup"><span data-stu-id="9f34e-150">Otherwise, select **Add** and search for **Cerner Central** in the application gallery.</span></span> <span data-ttu-id="9f34e-151">Vyberte Cerner střed ve výsledcích hledání a přidejte ji do seznamu aplikací.</span><span class="sxs-lookup"><span data-stu-id="9f34e-151">Select Cerner Central from the search results, and add it to your list of applications.</span></span>

7.  <span data-ttu-id="9f34e-152">Vyberte instanci Cerner střed a pak vyberte **zřizování** kartě.</span><span class="sxs-lookup"><span data-stu-id="9f34e-152">Select your instance of Cerner Central, then select the **Provisioning** tab.</span></span>

8.  <span data-ttu-id="9f34e-153">Nastavte **režimu zřizování** k **automatické**.</span><span class="sxs-lookup"><span data-stu-id="9f34e-153">Set the **Provisioning Mode** to **Automatic**.</span></span>

   ![Střed Cerner zřizování](./media/active-directory-saas-cernercentral-provisioning-tutorial/Cerner.PNG)

9.  <span data-ttu-id="9f34e-155">Vyplňte následující pole v části **přihlašovací údaje správce**:</span><span class="sxs-lookup"><span data-stu-id="9f34e-155">Fill in the following fields under **Admin Credentials**:</span></span>

   * <span data-ttu-id="9f34e-156">V **URL klienta** pole, zadejte adresu URL ve formátu níže nahraďte "User-soupisky-sféry-ID" s ID sféry jste získali v kroku #4.</span><span class="sxs-lookup"><span data-stu-id="9f34e-156">In the **Tenant URL** field, enter a URL in the format below, replacing "User-Roster-Realm-ID" with the realm ID you acquired in step #4.</span></span>

> <span data-ttu-id="9f34e-157">Izolovaný prostor: https://user-roster-api.sandboxcernercentral.com/scim/v1/Realms/User-Roster-Realm-ID/</span><span class="sxs-lookup"><span data-stu-id="9f34e-157">Sandbox: https://user-roster-api.sandboxcernercentral.com/scim/v1/Realms/User-Roster-Realm-ID/</span></span> 

> <span data-ttu-id="9f34e-158">Produkční: https://user-roster-api.cernercentral.com/scim/v1/Realms/User-Roster-Realm-ID/</span><span class="sxs-lookup"><span data-stu-id="9f34e-158">Production: https://user-roster-api.cernercentral.com/scim/v1/Realms/User-Roster-Realm-ID/</span></span> 

   * <span data-ttu-id="9f34e-159">V **tajný klíč tokenu** pole, zadejte tokenu nosiče OAuth, který jste vygenerovali v kroku #3 a klikněte na **Test připojení**.</span><span class="sxs-lookup"><span data-stu-id="9f34e-159">In the **Secret Token** field, enter the OAuth bearer token you generated in step #3 and click **Test Connection**.</span></span>

   * <span data-ttu-id="9f34e-160">Měli byste vidět úspěšné oznámení na straně ÚETUChcete portálu.</span><span class="sxs-lookup"><span data-stu-id="9f34e-160">You should see a success notification on the upper­right side of your portal.</span></span>

10. <span data-ttu-id="9f34e-161">Zadejte e-mailovou adresu uživatele nebo skupiny, který by měly dostávat oznámení zřizování Chyba v **e-mailové oznámení** pole a zaškrtněte políčko níže.</span><span class="sxs-lookup"><span data-stu-id="9f34e-161">Enter the email address of a person or group who should receive provisioning error notifications in the **Notification Email** field, and check the checkbox below.</span></span>

11. <span data-ttu-id="9f34e-162">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="9f34e-162">Click **Save**.</span></span> 

12. <span data-ttu-id="9f34e-163">V **mapování atributů** , projděte si atributy uživatelů a skupin na střed Cerner synchronizaci z Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9f34e-163">In the **Attribute Mappings** section, review the user and group attributes to be synchronized from Azure AD to Cerner Central.</span></span> <span data-ttu-id="9f34e-164">Atributy vybrán jako **párování** vlastnosti jsou slouží k přiřazení uživatelských účtů a skupin v centrální Cerner pro operace aktualizace.</span><span class="sxs-lookup"><span data-stu-id="9f34e-164">The attributes selected as **Matching** properties are used to match the user accounts and groups in Cerner Central for update operations.</span></span> <span data-ttu-id="9f34e-165">Kliknutím na tlačítko Uložit potvrzení změny.</span><span class="sxs-lookup"><span data-stu-id="9f34e-165">Select the Save button to commit any changes.</span></span>

13. <span data-ttu-id="9f34e-166">Povolit Azure AD zřizování služby pro Cerner ústředí, změňte **Stav zřizování** k **na** v **nastavení** části</span><span class="sxs-lookup"><span data-stu-id="9f34e-166">To enable the Azure AD provisioning service for Cerner Central, change the **Provisioning Status** to **On** in the **Settings** section</span></span>

14. <span data-ttu-id="9f34e-167">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="9f34e-167">Click **Save**.</span></span> 

<span data-ttu-id="9f34e-168">Tím se spustí počáteční synchronizaci všech uživatelů a skupiny přiřazené k Cerner střed v části Uživatelé a skupiny.</span><span class="sxs-lookup"><span data-stu-id="9f34e-168">This starts the initial synchronization of any users and/or groups assigned to Cerner Central in the Users and Groups section.</span></span> <span data-ttu-id="9f34e-169">Počáteční synchronizace trvá déle provést než následné synchronizace, ke kterým dochází přibližně každých 20 minut, dokud běží zřizování služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9f34e-169">The initial sync takes longer to perform than subsequent syncs, which occur approximately every 20 minutes as long as the Azure AD provisioning service is running.</span></span> <span data-ttu-id="9f34e-170">Můžete použít **podrobnosti synchronizace** části monitorovat průběh a odkazech zřízení sestavy aktivity, které popisují všechny akce, které provádí službu zřizování na střed Cerner aplikace.</span><span class="sxs-lookup"><span data-stu-id="9f34e-170">You can use the **Synchronization Details** section to monitor progress and follow links to provisioning activity reports, which describe all actions performed by the provisioning service on your Cerner Central app.</span></span>

<span data-ttu-id="9f34e-171">Další informace o tom, jak číst zřizování protokoly služby Azure AD najdete v tématu [zprávy o zřizování účtu automatické uživatele](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span><span class="sxs-lookup"><span data-stu-id="9f34e-171">For more information on how to read the Azure AD provisioning logs, see [Reporting on automatic user account provisioning](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9f34e-172">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="9f34e-172">Additional resources</span></span>

* [<span data-ttu-id="9f34e-173">Střed Cerner: Publikování dat identity pomocí služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="9f34e-173">Cerner Central: Publishing identity data using Azure AD</span></span>](https://wiki.ucern.com/display/public/reference/Publishing+Identity+Data+Using+Azure+AD)
* [<span data-ttu-id="9f34e-174">Kurz: Konfigurace Cerner střed pro jednotné přihlašování s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9f34e-174">Tutorial: Configuring Cerner Central for single sign-on with Azure Active Directory</span></span>](active-directory-saas-cernercentral-tutorial.md)
* [<span data-ttu-id="9f34e-175">Správa uživatelů zřizování účtu pro podnikové aplikace</span><span class="sxs-lookup"><span data-stu-id="9f34e-175">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-enterprise-apps-manage-provisioning.md)
* [<span data-ttu-id="9f34e-176">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="9f34e-176">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a><span data-ttu-id="9f34e-177">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9f34e-177">Next steps</span></span>
* <span data-ttu-id="9f34e-178">[Zjistěte, jak získat sestavy o zřizování aktivity a zkontrolujte protokoly](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span><span class="sxs-lookup"><span data-stu-id="9f34e-178">[Learn how to review logs and get reports on provisioning activity](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span></span>
