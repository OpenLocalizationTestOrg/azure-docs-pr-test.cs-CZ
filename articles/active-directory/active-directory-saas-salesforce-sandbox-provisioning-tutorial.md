---
title: "Kurz: Azure Active Directory integrace s izolovaného prostoru Salesforce | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a izolovaného prostoru služby Salesforce."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: bab73fda-6754-411d-9288-f73ecdaa486d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: jeedes
ms.openlocfilehash: 7d3c655a754f83284c386d2007c604a731367814
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-configuring-salesforce-sandbox-for-automatic-user-provisioning"></a><span data-ttu-id="3f660-103">Kurz: Konfigurace služby Salesforce izolovaného prostoru pro zřizování automatické uživatelů</span><span class="sxs-lookup"><span data-stu-id="3f660-103">Tutorial: Configuring Salesforce Sandbox for Automatic User Provisioning</span></span>

<span data-ttu-id="3f660-104">Cílem tohoto kurzu je tak, aby zobrazovalo kroky, které je třeba provést v izolovaného prostoru Salesforce a Azure AD a automaticky zřizovat a zrušte zřízení uživatelských účtů ze služby Azure AD do izolovaného prostoru služby Salesforce.</span><span class="sxs-lookup"><span data-stu-id="3f660-104">The objective of this tutorial is to show you the steps you need to perform in Salesforce Sandbox and Azure AD to automatically provision and de-provision user accounts from Azure AD to Salesforce Sandbox.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3f660-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3f660-105">Prerequisites</span></span>

<span data-ttu-id="3f660-106">Scénář uvedených v tomto kurzu se předpokládá, že už máte následující položky:</span><span class="sxs-lookup"><span data-stu-id="3f660-106">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

*   <span data-ttu-id="3f660-107">Klienta služby Azure Active directory.</span><span class="sxs-lookup"><span data-stu-id="3f660-107">An Azure Active directory tenant.</span></span>
*   <span data-ttu-id="3f660-108">Pro služby Salesforce izolovaného prostoru pro pracovní nebo Salesforce izolovaného prostoru pro vzdělávací organizace musí mít platný klienta.</span><span class="sxs-lookup"><span data-stu-id="3f660-108">You must have a valid tenant for Salesforce Sandbox for Work or Salesforce Sandbox for Education.</span></span> <span data-ttu-id="3f660-109">Bezplatný zkušební účet můžete použít buď služby.</span><span class="sxs-lookup"><span data-stu-id="3f660-109">You may use a free trial     account for either service.</span></span>
*   <span data-ttu-id="3f660-110">Uživatelský účet v izolovaném prostoru Salesforce s oprávněními správce týmu.</span><span class="sxs-lookup"><span data-stu-id="3f660-110">A user account in Salesforce Sandbox with Team Admin permissions.</span></span>

## <a name="assigning-users-to-salesforce-sandbox"></a><span data-ttu-id="3f660-111">Přiřazování uživatelů do izolovaného prostoru Salesforce</span><span class="sxs-lookup"><span data-stu-id="3f660-111">Assigning users to Salesforce Sandbox</span></span>

<span data-ttu-id="3f660-112">Azure Active Directory používá koncept označované jako "úlohy" k určení uživatelů, kteří obdrželi přístup k vybrané aplikace.</span><span class="sxs-lookup"><span data-stu-id="3f660-112">Azure Active Directory uses a concept called "assignments" to determine which users should receive access to selected apps.</span></span> <span data-ttu-id="3f660-113">V kontextu uživatele automatické zřizování účtu jsou synchronizovány pouze uživatelé a skupiny, které byly "přiřazeny" aplikace ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3f660-113">In the context of automatic user account provisioning, only the users and groups that have been "assigned" to an application in Azure AD are synchronized.</span></span>

<span data-ttu-id="3f660-114">Před konfigurací a povolení zřizování služby, musíte rozhodnout, jaké uživatelů nebo skupin ve službě Azure AD představují uživatele, kteří potřebují přístup k vaší aplikaci Salesforce izolovaného prostoru.</span><span class="sxs-lookup"><span data-stu-id="3f660-114">Before configuring and enabling the provisioning service, you need to decide what users and/or groups in Azure AD represent the users who need access to your Salesforce Sandbox app.</span></span> <span data-ttu-id="3f660-115">Jakmile se rozhodli, můžete přiřadit tito uživatelé do izolovaného prostoru Salesforce aplikace podle pokynů tady:</span><span class="sxs-lookup"><span data-stu-id="3f660-115">Once decided, you can assign these users to your Salesforce Sandbox app by following the instructions here:</span></span>

[<span data-ttu-id="3f660-116">Přiřazení uživatele nebo skupiny do aplikace enterprise</span><span class="sxs-lookup"><span data-stu-id="3f660-116">Assign a user or group to an enterprise app</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-salesforce-sandbox"></a><span data-ttu-id="3f660-117">Důležité tipy pro přiřazování uživatelů do izolovaného prostoru Salesforce</span><span class="sxs-lookup"><span data-stu-id="3f660-117">Important tips for assigning users to Salesforce Sandbox</span></span>

* <span data-ttu-id="3f660-118">Dále je doporučeno jednoho uživatele Azure AD se přiřadí ke izolovaného prostoru Salesforce a otestovat konfiguraci zřizování.</span><span class="sxs-lookup"><span data-stu-id="3f660-118">It is recommended that a single Azure AD user is assigned to Salesforce Sandbox to test the provisioning configuration.</span></span> <span data-ttu-id="3f660-119">Další uživatele nebo skupiny může být přiřazen později.</span><span class="sxs-lookup"><span data-stu-id="3f660-119">Additional users and/or groups may be assigned later.</span></span>

* <span data-ttu-id="3f660-120">Při přiřazení uživatele k izolovanému prostoru služby Salesforce, musíte vybrat platné uživatelské role.</span><span class="sxs-lookup"><span data-stu-id="3f660-120">When assigning a user to Salesforce Sandbox, you must select a valid user role.</span></span> <span data-ttu-id="3f660-121">Roli "Výchozí přístup" nefunguje pro zřizování.</span><span class="sxs-lookup"><span data-stu-id="3f660-121">The "Default Access" role does not work for provisioning.</span></span>

> [!NOTE]
> <span data-ttu-id="3f660-122">Tato aplikace importuje vlastní role ze služby Salesforce izolovaného prostoru v rámci procesu zřizování, který může zákazník vyberte při přiřazování uživatelů.</span><span class="sxs-lookup"><span data-stu-id="3f660-122">This app imports custom roles from Salesforce Sandbox as part of the provisioning process, which the customer may want to select when assigning users.</span></span>

## <a name="enable-automated-user-provisioning"></a><span data-ttu-id="3f660-123">Povolit automatické zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="3f660-123">Enable Automated User Provisioning</span></span>

<span data-ttu-id="3f660-124">Tato část vás provede připojení k Salesforce izolovaného uživatelský účet zřizování rozhraní API služby Azure AD a konfiguraci zřizování službu, kterou chcete vytvořit, aktualizovat a zakázat přiřazené uživatelské účty v izolovaném prostoru Salesforce podle přiřazení uživatelů a skupin ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3f660-124">This section guides you through connecting your Azure AD to Salesforce Sandbox's user account provisioning API, and configuring the provisioning service to create, update, and disable assigned user accounts in Salesforce Sandbox based on user and group assignment in Azure AD.</span></span>

>[!Tip]
><span data-ttu-id="3f660-125">Můžete také pro izolovaný prostor Salesforce povoleno na základě SAML jednotné přihlašování, postupujte podle pokynů uvedených v [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="3f660-125">You may also choose to enabled SAML-based Single Sign-On for Salesforce Sandbox, following the instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="3f660-126">Jednotné přihlašování se dá nakonfigurovat nezávisle na automatické zřizování, i když tyto dvě funkce doplnění navzájem.</span><span class="sxs-lookup"><span data-stu-id="3f660-126">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>

### <a name="to-configure-automatic-user-account-provisioning"></a><span data-ttu-id="3f660-127">Konfigurace automatického uživatele zřizování účtu:</span><span class="sxs-lookup"><span data-stu-id="3f660-127">To configure automatic user account provisioning:</span></span>

<span data-ttu-id="3f660-128">Cílem této části se popisují postup povolení zřizování uživatelů služby Active Directory uživatelských účtů do izolovaného prostoru služby Salesforce.</span><span class="sxs-lookup"><span data-stu-id="3f660-128">The objective of this section is to outline how to enable user provisioning of Active Directory user accounts to Salesforce Sandbox.</span></span>

1. <span data-ttu-id="3f660-129">V [portál Azure](https://portal.azure.com), vyhledejte **Azure Active Directory > podnikové aplikace > všechny aplikace** části.</span><span class="sxs-lookup"><span data-stu-id="3f660-129">In the [Azure portal](https://portal.azure.com), browse to the **Azure Active Directory > Enterprise Apps > All applications** section.</span></span>

2. <span data-ttu-id="3f660-130">Pokud jste již nakonfigurovali Salesforce izolovaného prostoru pro jednotné přihlašování, vyhledávání pro instanci služby Salesforce izolovaného prostoru pomocí pole hledání.</span><span class="sxs-lookup"><span data-stu-id="3f660-130">If you have already configured Salesforce Sandbox for single sign-on, search for your instance of Salesforce Sandbox using the search field.</span></span> <span data-ttu-id="3f660-131">Jinak vyberte možnost **přidat** a vyhledejte **izolovaného prostoru Salesforce** v galerii aplikací.</span><span class="sxs-lookup"><span data-stu-id="3f660-131">Otherwise, select **Add** and search for **Salesforce Sandbox** in the application gallery.</span></span> <span data-ttu-id="3f660-132">Vyberte Salesforce izolovaného prostoru ve výsledcích hledání a přidejte ji do seznamu aplikací.</span><span class="sxs-lookup"><span data-stu-id="3f660-132">Select Salesforce Sandbox from the search results, and add it to your list of applications.</span></span>

3. <span data-ttu-id="3f660-133">Vyberte instanci služby Salesforce izolovaného prostoru a pak vyberte **zřizování** kartě.</span><span class="sxs-lookup"><span data-stu-id="3f660-133">Select your instance of Salesforce Sandbox, then select the **Provisioning** tab.</span></span>

4. <span data-ttu-id="3f660-134">Nastavte **režimu zřizování** k **automatické**.</span><span class="sxs-lookup"><span data-stu-id="3f660-134">Set the **Provisioning Mode** to **Automatic**.</span></span> 
    <span data-ttu-id="3f660-135">![Zřizování](./media/active-directory-saas-salesforce-sandbox-provisioning-tutorial/provisioning.png)</span><span class="sxs-lookup"><span data-stu-id="3f660-135">![provisioning](./media/active-directory-saas-salesforce-sandbox-provisioning-tutorial/provisioning.png)</span></span>

5. <span data-ttu-id="3f660-136">V části **přihlašovací údaje správce** části, zadejte následující nastavení konfigurace:</span><span class="sxs-lookup"><span data-stu-id="3f660-136">Under the **Admin Credentials** section, provide the following configuration settings:</span></span>
   
    <span data-ttu-id="3f660-137">a.</span><span class="sxs-lookup"><span data-stu-id="3f660-137">a.</span></span> <span data-ttu-id="3f660-138">V **uživatelské jméno správce** textovému poli, zadejte název, který má účet služby Salesforce izolovaném prostoru **správce systému** profil v Salesforce.com přiřazen.</span><span class="sxs-lookup"><span data-stu-id="3f660-138">In the **Admin User Name** textbox, type a Salesforce Sandbox account name that has the **System Administrator** profile in Salesforce.com assigned.</span></span>
   
    <span data-ttu-id="3f660-139">b.</span><span class="sxs-lookup"><span data-stu-id="3f660-139">b.</span></span> <span data-ttu-id="3f660-140">V **heslo správce** textovému poli, zadejte heslo pro tento účet.</span><span class="sxs-lookup"><span data-stu-id="3f660-140">In the **Admin Password** textbox, type the password for this account.</span></span>

6. <span data-ttu-id="3f660-141">Se získat token zabezpečení izolovaného prostoru služby Salesforce, otevřete novou kartu a přihlášení do stejného účtu správce izolovaného prostoru služby Salesforce.</span><span class="sxs-lookup"><span data-stu-id="3f660-141">To get your Salesforce Sandbox security token, open a new tab and sign into the same Salesforce Sandbox admin account.</span></span> <span data-ttu-id="3f660-142">V pravém horním rohu stránky klikněte na své jméno a potom klikněte **Moje nastavení**.</span><span class="sxs-lookup"><span data-stu-id="3f660-142">On the top right corner of the page, click your name, and then click **My Settings**.</span></span>

     <span data-ttu-id="3f660-143">![Povolit automatické uživatele zajišťování](./media/active-directory-saas-salesforce-sandbox-provisioning-tutorial/sf-my-settings.png "povolit zřizování automatické uživatelů")</span><span class="sxs-lookup"><span data-stu-id="3f660-143">![Enable automatic user provisioning](./media/active-directory-saas-salesforce-sandbox-provisioning-tutorial/sf-my-settings.png "Enable automatic user provisioning")</span></span>
7. <span data-ttu-id="3f660-144">V levém navigačním podokně klikněte na tlačítko **osobní** rozbalte související část, a potom klikněte na **resetovat Moje zabezpečení tokenu**.</span><span class="sxs-lookup"><span data-stu-id="3f660-144">On the left navigation pane, click **Personal** to expand the related section, and then click **Reset My Security Token**.</span></span>
  
    <span data-ttu-id="3f660-145">![Povolit automatické uživatele zajišťování](./media/active-directory-saas-salesforce-sandbox-provisioning-tutorial/sf-personal-reset.png "povolit zřizování automatické uživatelů")</span><span class="sxs-lookup"><span data-stu-id="3f660-145">![Enable automatic user provisioning](./media/active-directory-saas-salesforce-sandbox-provisioning-tutorial/sf-personal-reset.png "Enable automatic user provisioning")</span></span>
8. <span data-ttu-id="3f660-146">Na **resetovat Moje zabezpečení tokenu** klikněte na tlačítko **resetovat tokenu zabezpečení** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3f660-146">On the **Reset My Security Token** page, click the **Reset Security Token** button.</span></span>

    <span data-ttu-id="3f660-147">![Povolit automatické uživatele zajišťování](./media/active-directory-saas-salesforce-sandbox-provisioning-tutorial/sf-reset-token.png "povolit zřizování automatické uživatelů")</span><span class="sxs-lookup"><span data-stu-id="3f660-147">![Enable automatic user provisioning](./media/active-directory-saas-salesforce-sandbox-provisioning-tutorial/sf-reset-token.png "Enable automatic user provisioning")</span></span>
9. <span data-ttu-id="3f660-148">Zkontrolujte e-mailovou schránku spojené s tímto účtem správce.</span><span class="sxs-lookup"><span data-stu-id="3f660-148">Check the email inbox associated with this admin account.</span></span> <span data-ttu-id="3f660-149">Vyhledejte e-mailu ze služby Salesforce Sandbox.com, který obsahuje nový token zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="3f660-149">Look for an email from Salesforce Sandbox.com that contains the new security token.</span></span>
10. <span data-ttu-id="3f660-150">Zkopírujte token, přejděte do okna vaší služby Azure AD a vložte ji do **soketu tokenu** pole.</span><span class="sxs-lookup"><span data-stu-id="3f660-150">Copy the token, go to your Azure AD window, and paste it into the **Socket Token** field.</span></span>

11. <span data-ttu-id="3f660-151">Na portálu Azure klikněte na tlačítko **Test připojení** zajistit Azure AD může připojit k aplikaci Salesforce izolovaného prostoru.</span><span class="sxs-lookup"><span data-stu-id="3f660-151">In the Azure portal, click **Test Connection** to ensure Azure AD can connect to your Salesforce Sandbox app.</span></span>

12. <span data-ttu-id="3f660-152">V **e-mailové oznámení** pole, zadejte e-mailovou adresu uživatele nebo skupiny, kdo by měly dostávat oznámení zřizování chyby a zaškrtněte políčko.</span><span class="sxs-lookup"><span data-stu-id="3f660-152">In the **Notification Email** field, enter the email address of a person or group who should receive provisioning error notifications, and check the checkbox.</span></span>

13. <span data-ttu-id="3f660-153">Klikněte na tlačítko **uložit.**</span><span class="sxs-lookup"><span data-stu-id="3f660-153">Click **Save.**</span></span>  
    
14.  <span data-ttu-id="3f660-154">V části mapování vyberte **synchronizaci Azure Active Directory Users do izolovaného prostoru služby Salesforce.**</span><span class="sxs-lookup"><span data-stu-id="3f660-154">Under the Mappings section, select **Synchronize Azure Active Directory Users to Salesforce Sandbox.**</span></span>

15. <span data-ttu-id="3f660-155">V **mapování atributů** , projděte si uživatelské atributy, které jsou synchronizované z Azure AD izolovaného prostoru služby Salesforce.</span><span class="sxs-lookup"><span data-stu-id="3f660-155">In the **Attribute Mappings** section, review the user attributes that are synchronized from Azure AD to Salesforce Sandbox.</span></span> <span data-ttu-id="3f660-156">Atributy vybrán jako **párování** vlastnosti se používají tak, aby odpovídaly uživatelské účty v Salesforce izolovaného prostoru pro operace aktualizace.</span><span class="sxs-lookup"><span data-stu-id="3f660-156">The attributes selected as **Matching** properties are used to match the user accounts in Salesforce Sandbox for update operations.</span></span> <span data-ttu-id="3f660-157">Kliknutím na tlačítko Uložit potvrzení změny.</span><span class="sxs-lookup"><span data-stu-id="3f660-157">Select the Save button to commit any changes.</span></span>

16. <span data-ttu-id="3f660-158">Povolit zřizování služby pro izolovaný prostor Salesforce Azure AD, změňte **Stav zřizování** k **na** v části Nastavení</span><span class="sxs-lookup"><span data-stu-id="3f660-158">To enable the Azure AD provisioning service for Salesforce Sandbox, change the **Provisioning Status** to **On** in the Settings section</span></span>

17. <span data-ttu-id="3f660-159">Klikněte na tlačítko **uložit.**</span><span class="sxs-lookup"><span data-stu-id="3f660-159">Click **Save.**</span></span>


<span data-ttu-id="3f660-160">Spustí počáteční synchronizaci všech uživatelů a skupiny přiřazené k Salesforce izolovaného prostoru v části Uživatelé a skupiny.</span><span class="sxs-lookup"><span data-stu-id="3f660-160">It starts the initial synchronization of any users and/or groups assigned to Salesforce Sandbox in the Users and Groups section.</span></span> <span data-ttu-id="3f660-161">Počáteční synchronizace trvá déle než následné synchronizace, ke kterým dochází přibližně každých 20 minut, dokud se službou provést.</span><span class="sxs-lookup"><span data-stu-id="3f660-161">The initial sync takes longer to perform than subsequent syncs, which occur approximately every 20 minutes as long as the service is running.</span></span> <span data-ttu-id="3f660-162">Můžete použít **podrobnosti synchronizace** části monitorovat průběh a odkazech zřízení sestavy aktivity, které popisují všechny akce, které provádí službu zřizování na aplikaci Salesforce izolovaného prostoru.</span><span class="sxs-lookup"><span data-stu-id="3f660-162">You can use the **Synchronization Details** section to monitor progress and follow links to provisioning activity reports, which describe all actions performed by the provisioning service on Salesforce Sandbox app.</span></span>

<span data-ttu-id="3f660-163">Nyní můžete vytvořit testovací účet.</span><span class="sxs-lookup"><span data-stu-id="3f660-163">You can now create a test account.</span></span> <span data-ttu-id="3f660-164">Chcete-li ověřit, že účet umístění byl synchronizován do služby salesforce Počkejte až 20 minut.</span><span class="sxs-lookup"><span data-stu-id="3f660-164">Wait for up to 20 minutes to verify that the account has been synchronized to salesforce.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3f660-165">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="3f660-165">Additional resources</span></span>

* [<span data-ttu-id="3f660-166">Správa uživatelů zřizování účtu pro podnikové aplikace</span><span class="sxs-lookup"><span data-stu-id="3f660-166">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="3f660-167">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="3f660-167">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="3f660-168">Konfigurovat jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="3f660-168">Configure Single Sign-on</span></span>](active-directory-saas-salesforcesandbox-tutorial.md)