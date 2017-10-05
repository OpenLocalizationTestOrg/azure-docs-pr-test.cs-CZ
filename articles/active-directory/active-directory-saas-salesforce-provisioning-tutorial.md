---
title: 'Kurz: Azure Active Directory integrace s Salesforce | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a služby Salesforce."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 49384b8b-3836-4eb1-b438-1c46bb9baf6f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: a573a7ef79e28c50ae0923849a88f88af40f21be
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-configuring-salesforce-for-automatic-user-provisioning"></a><span data-ttu-id="b0003-103">Kurz: Konfigurace služby Salesforce pro zřizování automatické uživatelů</span><span class="sxs-lookup"><span data-stu-id="b0003-103">Tutorial: Configuring Salesforce for Automatic User Provisioning</span></span>

<span data-ttu-id="b0003-104">Cílem tohoto kurzu je zobrazit kroky potřebné k provedení v Salesforce a Azure AD pro automatické zřizování a deaktivace zřízení uživatelských účtů ze služby Azure AD do služby Salesforce.</span><span class="sxs-lookup"><span data-stu-id="b0003-104">The objective of this tutorial is to show the steps required to perform in Salesforce and Azure AD to automatically provision and de-provision user accounts from Azure AD to Salesforce.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b0003-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b0003-105">Prerequisites</span></span>

<span data-ttu-id="b0003-106">Scénář uvedených v tomto kurzu se předpokládá, že už máte následující položky:</span><span class="sxs-lookup"><span data-stu-id="b0003-106">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

*   <span data-ttu-id="b0003-107">Klienta služby Azure Active directory.</span><span class="sxs-lookup"><span data-stu-id="b0003-107">An Azure Active directory tenant.</span></span>
*   <span data-ttu-id="b0003-108">Pro pracovní nebo Salesforce pro vzdělávací organizace musí mít platný klient pro služby Salesforce.</span><span class="sxs-lookup"><span data-stu-id="b0003-108">You must have a valid tenant for Salesforce for Work or Salesforce for Education.</span></span> <span data-ttu-id="b0003-109">Bezplatný zkušební účet můžete použít buď služby.</span><span class="sxs-lookup"><span data-stu-id="b0003-109">You may use a free trial     account for either service.</span></span>
*   <span data-ttu-id="b0003-110">Uživatelský účet v Salesforce s oprávněními správce týmu.</span><span class="sxs-lookup"><span data-stu-id="b0003-110">A user account in Salesforce with Team Admin permissions.</span></span>

## <a name="assigning-users-to-salesforce"></a><span data-ttu-id="b0003-111">Přiřazování uživatelů do služby Salesforce</span><span class="sxs-lookup"><span data-stu-id="b0003-111">Assigning users to Salesforce</span></span>

<span data-ttu-id="b0003-112">Azure Active Directory používá koncept označované jako "úlohy" k určení uživatelů, kteří obdrželi přístup k vybrané aplikace.</span><span class="sxs-lookup"><span data-stu-id="b0003-112">Azure Active Directory uses a concept called "assignments" to determine which users should receive access to selected apps.</span></span> <span data-ttu-id="b0003-113">V kontextu uživatele automatické zřizování účtu se synchronizují pouze uživatelé a skupiny, které byly "přiřazeny" aplikace ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b0003-113">In the context of automatic user account provisioning, only the users and groups that have been "assigned" to an application in Azure AD is synchronized.</span></span>

<span data-ttu-id="b0003-114">Před konfigurací a povolení zřizování služby, musíte rozhodnout, jaké uživatelů nebo skupin ve službě Azure AD představují uživatele, kteří potřebují přístup k vaší aplikaci Salesforce.</span><span class="sxs-lookup"><span data-stu-id="b0003-114">Before configuring and enabling the provisioning service, you need to decide what users and/or groups in Azure AD represent the users who need access to your Salesforce app.</span></span> <span data-ttu-id="b0003-115">Jakmile se rozhodli, můžete přiřadit těmto uživatelům aplikace Salesforce podle pokynů tady:</span><span class="sxs-lookup"><span data-stu-id="b0003-115">Once decided, you can assign these users to your Salesforce app by following the instructions here:</span></span>

[<span data-ttu-id="b0003-116">Přiřazení uživatele nebo skupiny do aplikace enterprise</span><span class="sxs-lookup"><span data-stu-id="b0003-116">Assign a user or group to an enterprise app</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-salesforce"></a><span data-ttu-id="b0003-117">Důležité tipy pro přiřazování uživatelů do služby Salesforce</span><span class="sxs-lookup"><span data-stu-id="b0003-117">Important tips for assigning users to Salesforce</span></span>

*   <span data-ttu-id="b0003-118">Dále je doporučeno jednoho uživatele Azure AD se přiřadí ke Salesforce a otestovat konfiguraci zřizování.</span><span class="sxs-lookup"><span data-stu-id="b0003-118">It is recommended that a single Azure AD user is assigned to Salesforce to test the provisioning configuration.</span></span> <span data-ttu-id="b0003-119">Další uživatele nebo skupiny může být přiřazen později.</span><span class="sxs-lookup"><span data-stu-id="b0003-119">Additional users and/or groups may be assigned later.</span></span>

*  <span data-ttu-id="b0003-120">Při přiřazování uživatele do služby Salesforce, musíte vybrat platné uživatelské role.</span><span class="sxs-lookup"><span data-stu-id="b0003-120">When assigning a user to Salesforce, you must select a valid user role.</span></span> <span data-ttu-id="b0003-121">Roli "Výchozí přístup" nefunguje pro zřizování</span><span class="sxs-lookup"><span data-stu-id="b0003-121">The "Default Access" role does not work for provisioning</span></span>

    > [!NOTE]
    > <span data-ttu-id="b0003-122">Tato aplikace importuje vlastní role ze služby Salesforce jako součást procesu zřizování, který může zákazník vyberte při přiřazování uživatelů</span><span class="sxs-lookup"><span data-stu-id="b0003-122">This app imports custom roles from Salesforce as part of the provisioning process, which the customer may want to select when assigning users</span></span>

## <a name="enable-automated-user-provisioning"></a><span data-ttu-id="b0003-123">Povolit automatické zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="b0003-123">Enable Automated User Provisioning</span></span>

<span data-ttu-id="b0003-124">Tato část vás provede připojení k Salesforce na uživatelský účet zřizování rozhraní API služby Azure AD a konfiguraci zřizování službu, kterou chcete vytvořit, aktualizovat a zakažte přiřazené uživatelské účty v Salesforce podle přiřazení uživatelů a skupin ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b0003-124">This section guides you through connecting your Azure AD to Salesforce's user account provisioning API, and configuring the provisioning service to create, update, and disable assigned user accounts in Salesforce based on user and group assignment in Azure AD.</span></span>

>[!Tip]
><span data-ttu-id="b0003-125">Můžete také pro Salesforce povoleno na základě SAML jednotné přihlašování, postupujte podle pokynů uvedených v [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="b0003-125">You may also choose to enabled SAML-based Single Sign-On for Salesforce, following the instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="b0003-126">Jednotné přihlašování se dá nakonfigurovat nezávisle na automatické zřizování, i když tyto dvě funkce doplnění navzájem.</span><span class="sxs-lookup"><span data-stu-id="b0003-126">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>

### <a name="to-configure-automatic-user-account-provisioning"></a><span data-ttu-id="b0003-127">Konfigurace automatického uživatele zřizování účtu:</span><span class="sxs-lookup"><span data-stu-id="b0003-127">To configure automatic user account provisioning:</span></span>

<span data-ttu-id="b0003-128">Cílem této části se popisují postup povolení zřizování uživatelů služby Active Directory uživatelských účtů do služby Salesforce.</span><span class="sxs-lookup"><span data-stu-id="b0003-128">The objective of this section is to outline how to enable user provisioning of Active Directory user accounts to Salesforce.</span></span>

1. <span data-ttu-id="b0003-129">V [portál Azure](https://portal.azure.com), vyhledejte **Azure Active Directory > podnikové aplikace > všechny aplikace** části.</span><span class="sxs-lookup"><span data-stu-id="b0003-129">In the [Azure portal](https://portal.azure.com), browse to the **Azure Active Directory > Enterprise Apps > All applications** section.</span></span>

2. <span data-ttu-id="b0003-130">Pokud jste již nakonfigurovali Salesforce pro jednotné přihlašování, vyhledávání pro instanci služby Salesforce pomocí pole hledání.</span><span class="sxs-lookup"><span data-stu-id="b0003-130">If you have already configured Salesforce for single sign-on, search for your instance of Salesforce using the search field.</span></span> <span data-ttu-id="b0003-131">Jinak vyberte možnost **přidat** a vyhledejte **Salesforce** v galerii aplikací.</span><span class="sxs-lookup"><span data-stu-id="b0003-131">Otherwise, select **Add** and search for **Salesforce** in the application gallery.</span></span> <span data-ttu-id="b0003-132">Vyberte Salesforce ve výsledcích hledání a přidejte ji do seznamu aplikací.</span><span class="sxs-lookup"><span data-stu-id="b0003-132">Select Salesforce from the search results, and add it to your list of applications.</span></span>

3. <span data-ttu-id="b0003-133">Vyberte instanci služby Salesforce a pak vyberte **zřizování** kartě.</span><span class="sxs-lookup"><span data-stu-id="b0003-133">Select your instance of Salesforce, then select the **Provisioning** tab.</span></span>

4. <span data-ttu-id="b0003-134">Nastavte **režimu zřizování** k **automatické**.</span><span class="sxs-lookup"><span data-stu-id="b0003-134">Set the **Provisioning Mode** to **Automatic**.</span></span> 
<span data-ttu-id="b0003-135">![Zřizování](./media/active-directory-saas-salesforce-provisioning-tutorial/provisioning.png)</span><span class="sxs-lookup"><span data-stu-id="b0003-135">![provisioning](./media/active-directory-saas-salesforce-provisioning-tutorial/provisioning.png)</span></span>

5. <span data-ttu-id="b0003-136">V části **přihlašovací údaje správce** části, zadejte následující nastavení konfigurace:</span><span class="sxs-lookup"><span data-stu-id="b0003-136">Under the **Admin Credentials** section, provide the following configuration settings:</span></span>
   
    <span data-ttu-id="b0003-137">a.</span><span class="sxs-lookup"><span data-stu-id="b0003-137">a.</span></span> <span data-ttu-id="b0003-138">V **uživatelské jméno správce** textovému poli, zadejte název, který má účtu Salesforce **správce systému** profil v Salesforce.com přiřazen.</span><span class="sxs-lookup"><span data-stu-id="b0003-138">In the **Admin User Name** textbox, type a Salesforce account name that has the **System Administrator** profile in Salesforce.com assigned.</span></span>
   
    <span data-ttu-id="b0003-139">b.</span><span class="sxs-lookup"><span data-stu-id="b0003-139">b.</span></span> <span data-ttu-id="b0003-140">V **heslo správce** textovému poli, zadejte heslo pro tento účet.</span><span class="sxs-lookup"><span data-stu-id="b0003-140">In the **Admin Password** textbox, type the password for this account.</span></span>

6. <span data-ttu-id="b0003-141">Chcete-li získat token zabezpečení služby Salesforce, otevřete novou kartu a přihlaste na stejný účet správce služby Salesforce.</span><span class="sxs-lookup"><span data-stu-id="b0003-141">To get your Salesforce security token, open a new tab and sign into the same Salesforce admin account.</span></span> <span data-ttu-id="b0003-142">V pravém horním rohu stránky klikněte na své jméno a potom klikněte **Moje nastavení**.</span><span class="sxs-lookup"><span data-stu-id="b0003-142">On the top right corner of the page, click your name, and then click **My Settings**.</span></span>

     <span data-ttu-id="b0003-143">![Povolit automatické uživatele zajišťování](./media/active-directory-saas-salesforce-provisioning-tutorial/sf-my-settings.png "povolit zřizování automatické uživatelů")</span><span class="sxs-lookup"><span data-stu-id="b0003-143">![Enable automatic user provisioning](./media/active-directory-saas-salesforce-provisioning-tutorial/sf-my-settings.png "Enable automatic user provisioning")</span></span>
7. <span data-ttu-id="b0003-144">V levém navigačním podokně klikněte na tlačítko **osobní** rozbalte související část, a potom klikněte na **resetovat Moje zabezpečení tokenu**.</span><span class="sxs-lookup"><span data-stu-id="b0003-144">On the left navigation pane, click **Personal** to expand the related section, and then click **Reset My Security Token**.</span></span>
  
    <span data-ttu-id="b0003-145">![Povolit automatické uživatele zajišťování](./media/active-directory-saas-salesforce-provisioning-tutorial/sf-personal-reset.png "povolit zřizování automatické uživatelů")</span><span class="sxs-lookup"><span data-stu-id="b0003-145">![Enable automatic user provisioning](./media/active-directory-saas-salesforce-provisioning-tutorial/sf-personal-reset.png "Enable automatic user provisioning")</span></span>
8. <span data-ttu-id="b0003-146">Na **resetovat Moje zabezpečení tokenu** klikněte na tlačítko **resetovat tokenu zabezpečení** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b0003-146">On the **Reset My Security Token** page, click **Reset Security Token** button.</span></span>

    <span data-ttu-id="b0003-147">![Povolit automatické uživatele zajišťování](./media/active-directory-saas-salesforce-provisioning-tutorial/sf-reset-token.png "povolit zřizování automatické uživatelů")</span><span class="sxs-lookup"><span data-stu-id="b0003-147">![Enable automatic user provisioning](./media/active-directory-saas-salesforce-provisioning-tutorial/sf-reset-token.png "Enable automatic user provisioning")</span></span>
9. <span data-ttu-id="b0003-148">Zkontrolujte e-mailovou schránku spojené s tímto účtem správce.</span><span class="sxs-lookup"><span data-stu-id="b0003-148">Check the email inbox associated with this admin account.</span></span> <span data-ttu-id="b0003-149">Vyhledejte e-mailu z Salesforce.com, který obsahuje nový token zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="b0003-149">Look for an email from Salesforce.com that contains the new security token.</span></span>
10. <span data-ttu-id="b0003-150">Zkopírujte token, přejděte do okna vaší služby Azure AD a vložte ji do **soketu tokenu** pole.</span><span class="sxs-lookup"><span data-stu-id="b0003-150">Copy the token, go to your Azure AD window, and paste it into the **Socket Token** field.</span></span>

11. <span data-ttu-id="b0003-151">Na portálu Azure klikněte na tlačítko **Test připojení** zajistit Azure AD může připojit k aplikaci Salesforce.</span><span class="sxs-lookup"><span data-stu-id="b0003-151">In the Azure portal, click **Test Connection** to ensure Azure AD can connect to your Salesforce app.</span></span>

12. <span data-ttu-id="b0003-152">V **e-mailové oznámení** pole, zadejte e-mailovou adresu uživatele nebo skupiny, který by měl zřizování chyba oznámení dostávat a zaškrtněte políčko níže.</span><span class="sxs-lookup"><span data-stu-id="b0003-152">In the **Notification Email** field, enter the email address of a person or group who should receive provisioning error notifications, and check the checkbox below.</span></span>

13. <span data-ttu-id="b0003-153">Klikněte na tlačítko **uložit.**</span><span class="sxs-lookup"><span data-stu-id="b0003-153">Click **Save.**</span></span>  
    
14.  <span data-ttu-id="b0003-154">V části mapování vyberte **synchronizaci Azure Active Directory Users do služby Salesforce.**</span><span class="sxs-lookup"><span data-stu-id="b0003-154">Under the Mappings section, select **Synchronize Azure Active Directory Users to Salesforce.**</span></span>

15. <span data-ttu-id="b0003-155">V **mapování atributů** , projděte si uživatelské atributy, které jsou synchronizované z Azure AD do služby Salesforce.</span><span class="sxs-lookup"><span data-stu-id="b0003-155">In the **Attribute Mappings** section, review the user attributes that are synchronized from Azure AD to Salesforce.</span></span> <span data-ttu-id="b0003-156">Všimněte si, že atributy vybrán jako **párování** vlastnosti se používají tak, aby odpovídaly uživatelské účty v Salesforce pro operace aktualizace.</span><span class="sxs-lookup"><span data-stu-id="b0003-156">Note that the attributes selected as **Matching** properties are used to match the user accounts in Salesforce for update operations.</span></span> <span data-ttu-id="b0003-157">Kliknutím na tlačítko Uložit potvrzení změny.</span><span class="sxs-lookup"><span data-stu-id="b0003-157">Select the Save button to commit any changes.</span></span>

16. <span data-ttu-id="b0003-158">Povolit Azure AD zřizování služby pro služby Salesforce, změňte **Stav zřizování** k **na** v části Nastavení</span><span class="sxs-lookup"><span data-stu-id="b0003-158">To enable the Azure AD provisioning service for Salesforce, change the **Provisioning Status** to **On** in the Settings section</span></span>

17. <span data-ttu-id="b0003-159">Klikněte na tlačítko **uložit.**</span><span class="sxs-lookup"><span data-stu-id="b0003-159">Click **Save.**</span></span>

<span data-ttu-id="b0003-160">Tím se spustí počáteční synchronizaci všech uživatelů a skupiny přiřazené do služby Salesforce v části Uživatelé a skupiny.</span><span class="sxs-lookup"><span data-stu-id="b0003-160">This starts the initial synchronization of any users and/or groups assigned to Salesforce in the Users and Groups section.</span></span> <span data-ttu-id="b0003-161">Všimněte si, že počáteční synchronizace trvá déle než následné synchronizace, ke kterým dochází přibližně každých 20 minut, dokud se službou provést.</span><span class="sxs-lookup"><span data-stu-id="b0003-161">Note that the initial sync takes longer to perform than subsequent syncs, which occur approximately every 20 minutes as long as the service is running.</span></span> <span data-ttu-id="b0003-162">Můžete použít **podrobnosti synchronizace** části monitorovat průběh a odkazech zřízení sestavy aktivity, které popisují všechny akce prováděné při zřizování služby ve vaší aplikaci Salesforce.</span><span class="sxs-lookup"><span data-stu-id="b0003-162">You can use the **Synchronization Details** section to monitor progress and follow links to provisioning activity reports, which describe all actions performed by the provisioning service on your Salesforce app.</span></span>

<span data-ttu-id="b0003-163">Nyní můžete vytvořit testovací účet.</span><span class="sxs-lookup"><span data-stu-id="b0003-163">You can now create a test account.</span></span> <span data-ttu-id="b0003-164">Chcete-li ověřit, že účet umístění byl synchronizován do služby Salesforce Počkejte až 20 minut.</span><span class="sxs-lookup"><span data-stu-id="b0003-164">Wait for up to 20 minutes to verify that the account has been synchronized to Salesforce.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b0003-165">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="b0003-165">Additional resources</span></span>

* [<span data-ttu-id="b0003-166">Správa uživatelů zřizování účtu pro podnikové aplikace</span><span class="sxs-lookup"><span data-stu-id="b0003-166">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b0003-167">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="b0003-167">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="b0003-168">Konfigurovat jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="b0003-168">Configure Single Sign-on</span></span>](active-directory-saas-salesforce-tutorial.md)