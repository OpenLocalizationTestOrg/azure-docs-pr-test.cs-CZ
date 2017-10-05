---
title: 'Kurz: Azure Active Directory integrace s Dropbox pro firmy | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Dropbox pro firmy."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 0f3a42e4-6897-4234-af84-b47c148ec3e1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 6f7616e47322242f01a13d763f71c93d4ac06a92
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-configuring-dropbox-for-business-for-automatic-user-provisioning"></a><span data-ttu-id="35031-103">Kurz: Konfigurace Dropbox pro firmy pro zřizování automatické uživatelů</span><span class="sxs-lookup"><span data-stu-id="35031-103">Tutorial: Configuring Dropbox for Business for Automatic User Provisioning</span></span>

<span data-ttu-id="35031-104">Cílem tohoto kurzu je tak, aby zobrazovalo kroky, které je třeba provést v Dropbox pro firmy s Azure AD a automaticky zřizovat a zrušte zřízení uživatelských účtů ze služby Azure AD do Dropboxu pro firmy.</span><span class="sxs-lookup"><span data-stu-id="35031-104">The objective of this tutorial is to show you the steps you need to perform in Dropbox for Business and Azure AD to automatically provision and de-provision user accounts from Azure AD to Dropbox for Business.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="35031-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="35031-105">Prerequisites</span></span>

<span data-ttu-id="35031-106">Scénář uvedených v tomto kurzu se předpokládá, že už máte následující položky:</span><span class="sxs-lookup"><span data-stu-id="35031-106">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

*   <span data-ttu-id="35031-107">Klienta služby Azure Active directory.</span><span class="sxs-lookup"><span data-stu-id="35031-107">An Azure Active directory tenant.</span></span>
*   <span data-ttu-id="35031-108">Dropbox pro obchodní jednotného přihlašování povolené předplatné.</span><span class="sxs-lookup"><span data-stu-id="35031-108">A Dropbox for Business single-sign on enabled subscription.</span></span>
*   <span data-ttu-id="35031-109">Uživatelský účet v Dropbox pro firmy s oprávněními správce týmu.</span><span class="sxs-lookup"><span data-stu-id="35031-109">A user account in Dropbox for Business with Team Admin permissions.</span></span>

## <a name="assigning-users-to-dropbox-for-business"></a><span data-ttu-id="35031-110">Přiřazování uživatelů do Dropboxu pro firmy</span><span class="sxs-lookup"><span data-stu-id="35031-110">Assigning users to Dropbox for Business</span></span>

<span data-ttu-id="35031-111">Azure Active Directory používá koncept označované jako "úlohy" k určení uživatelů, kteří obdrželi přístup k vybrané aplikace.</span><span class="sxs-lookup"><span data-stu-id="35031-111">Azure Active Directory uses a concept called "assignments" to determine which users should receive access to selected apps.</span></span> <span data-ttu-id="35031-112">V kontextu uživatele automatické zřizování účtu se synchronizují pouze uživatelé a skupiny, které byly "přiřazeny" aplikace ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="35031-112">In the context of automatic user account provisioning, only the users and groups that have been "assigned" to an application in Azure AD is synchronized.</span></span>

<span data-ttu-id="35031-113">Před konfigurací a povolení zřizování služby, musíte rozhodnout, jaké uživatelů nebo skupin ve službě Azure AD představují uživatele, kteří potřebují přístup k vaší schránky pro obchodní aplikace.</span><span class="sxs-lookup"><span data-stu-id="35031-113">Before configuring and enabling the provisioning service, you need to decide what users and/or groups in Azure AD represent the users who need access to your Dropbox for Business app.</span></span> <span data-ttu-id="35031-114">Jakmile se rozhodli, můžete přiřadit tyto uživatele do vaší schránky pro obchodní aplikace podle pokynů tady:</span><span class="sxs-lookup"><span data-stu-id="35031-114">Once decided, you can assign these users to your Dropbox for Business app by following the instructions here:</span></span>

[<span data-ttu-id="35031-115">Přiřazení uživatele nebo skupiny do aplikace enterprise</span><span class="sxs-lookup"><span data-stu-id="35031-115">Assign a user or group to an enterprise app</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-dropbox-for-business"></a><span data-ttu-id="35031-116">Důležité tipy pro přiřazování uživatelů do Dropboxu pro firmy</span><span class="sxs-lookup"><span data-stu-id="35031-116">Important tips for assigning users to Dropbox for Business</span></span>

*   <span data-ttu-id="35031-117">Dále je doporučeno jednoho uživatele Azure AD je přiřazen do Dropboxu pro firmy k testování této konfigurace zřizování.</span><span class="sxs-lookup"><span data-stu-id="35031-117">It is recommended that a single Azure AD user is assigned to Dropbox for Business to test the provisioning configuration.</span></span> <span data-ttu-id="35031-118">Další uživatele nebo skupiny může být přiřazen později.</span><span class="sxs-lookup"><span data-stu-id="35031-118">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="35031-119">Při přiřazování uživatele Dropbox pro firmy, musíte vybrat platné uživatelské role.</span><span class="sxs-lookup"><span data-stu-id="35031-119">When assigning a user to Dropbox for Business, you must select a valid user role.</span></span> <span data-ttu-id="35031-120">Roli "Výchozí přístup" nefunguje pro zřizování...</span><span class="sxs-lookup"><span data-stu-id="35031-120">The "Default Access" role does not work for provisioning..</span></span>

## <a name="enable-automated-user-provisioning"></a><span data-ttu-id="35031-121">Povolit automatické zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="35031-121">Enable Automated User Provisioning</span></span>

<span data-ttu-id="35031-122">Tato část vás provede připojení služby Azure AD k Dropboxu pro firmy na uživatelský účet zřizování rozhraní API a konfiguraci zřizování službu, kterou chcete vytvořit, aktualizovat a zakázat přiřazené uživatelské účty v Dropbox pro firmy na základě uživatele a skupiny přiřazení ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="35031-122">This section guides you through connecting your Azure AD to Dropbox for Business's user account provisioning API, and configuring the provisioning service to create, update, and disable assigned user accounts in Dropbox for Business based on user and group assignment in Azure AD.</span></span>

>[!Tip]
><span data-ttu-id="35031-123">Také můžete povolit na základě SAML jednotné přihlašování pro Dropbox pro firmy, postupujte podle pokynů uvedených v [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="35031-123">You may also choose to enabled SAML-based Single Sign-On for Dropbox for Business, following the instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="35031-124">Jednotné přihlašování se dá nakonfigurovat nezávisle na automatické zřizování, i když tyto dvě funkce doplnění navzájem.</span><span class="sxs-lookup"><span data-stu-id="35031-124">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>

### <a name="to-configure-automatic-user-account-provisioning"></a><span data-ttu-id="35031-125">Konfigurace automatického uživatele zřizování účtu:</span><span class="sxs-lookup"><span data-stu-id="35031-125">To configure automatic user account provisioning:</span></span>

1. <span data-ttu-id="35031-126">V [portál Azure](https://portal.azure.com), vyhledejte **Azure Active Directory > podnikové aplikace > všechny aplikace** části.</span><span class="sxs-lookup"><span data-stu-id="35031-126">In the [Azure portal](https://portal.azure.com), browse to the **Azure Active Directory > Enterprise Apps > All applications** section.</span></span>

2. <span data-ttu-id="35031-127">Pokud jste již nakonfigurovali Dropbox pro firmy pro jednotné přihlašování, vyhledejte instanci Dropboxu pro firmy pomocí pole hledání.</span><span class="sxs-lookup"><span data-stu-id="35031-127">If you have already configured Dropbox for Business for single sign-on, search for your instance of Dropbox for Business using the search field.</span></span> <span data-ttu-id="35031-128">Jinak vyberte možnost **přidat** a vyhledejte **Dropbox pro firmy** v galerii aplikací.</span><span class="sxs-lookup"><span data-stu-id="35031-128">Otherwise, select **Add** and search for **Dropbox for Business** in the application gallery.</span></span> <span data-ttu-id="35031-129">Ve výsledcích hledání vyberte Dropbox pro firmy a přidat do seznamu aplikací.</span><span class="sxs-lookup"><span data-stu-id="35031-129">Select Dropbox for Business from the search results, and add it to your list of applications.</span></span>

3. <span data-ttu-id="35031-130">Vyberte instanci Dropboxu pro firmy a pak vyberte **zřizování** kartě.</span><span class="sxs-lookup"><span data-stu-id="35031-130">Select your instance of Dropbox for Business, then select the **Provisioning** tab.</span></span>

4. <span data-ttu-id="35031-131">Nastavte **režimu zřizování** k **automatické**.</span><span class="sxs-lookup"><span data-stu-id="35031-131">Set the **Provisioning Mode** to **Automatic**.</span></span> 

    ![Zřizování](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/provisioning.png)

5. <span data-ttu-id="35031-133">V části **přihlašovací údaje správce** klikněte na tlačítko **Authorize**.</span><span class="sxs-lookup"><span data-stu-id="35031-133">Under the **Admin Credentials** section, click **Authorize**.</span></span> <span data-ttu-id="35031-134">Dropbox pro obchodní přihlašovací dialogové okno otevře v novém okně prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="35031-134">It opens a Dropbox for Business login dialog in a new browser window.</span></span>

6. <span data-ttu-id="35031-135">Na **přihlášení do Dropboxu k propojení s Azure AD** dialogové okno, přihlášení do vaší schránky pro obchodní klienta.</span><span class="sxs-lookup"><span data-stu-id="35031-135">On the **Sign-in to Dropbox to link with Azure AD** dialog, sign in to your Dropbox for Business tenant.</span></span>

     <span data-ttu-id="35031-136">![Zřizování uživatelů](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/ic769518.png "zřizování uživatelů")</span><span class="sxs-lookup"><span data-stu-id="35031-136">![User provisioning](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/ic769518.png "User provisioning")</span></span>

7. <span data-ttu-id="35031-137">Potvrďte, že byste chtěli poskytnout Azure Active Directory oprávnění k provádění změn do vaší schránky pro obchodní klienta.</span><span class="sxs-lookup"><span data-stu-id="35031-137">Confirm that you would like to give Azure Active Directory permission to make changes to your Dropbox for Business tenant.</span></span> <span data-ttu-id="35031-138">Klikněte na tlačítko **povolit**.</span><span class="sxs-lookup"><span data-stu-id="35031-138">Click **Allow**.</span></span>
    
      <span data-ttu-id="35031-139">![Zřizování uživatelů](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/ic769519.png "zřizování uživatelů")</span><span class="sxs-lookup"><span data-stu-id="35031-139">![User provisioning](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/ic769519.png "User provisioning")</span></span>

8. <span data-ttu-id="35031-140">Na portálu Azure klikněte na tlačítko **Test připojení** zajistit Azure AD může připojit k vaší schránky pro obchodní aplikace.</span><span class="sxs-lookup"><span data-stu-id="35031-140">In the Azure portal, click **Test Connection** to ensure Azure AD can connect to your Dropbox for Business app.</span></span> <span data-ttu-id="35031-141">Pokud připojení nezdaří, zkontrolujte vaši Dropbox pro obchodní účet má oprávnění správce týmu a zkuste to **"Ověřit"** krok opakujte.</span><span class="sxs-lookup"><span data-stu-id="35031-141">If the connection fails, ensure your Dropbox for Business account has Team Admin permissions and try the **"Authorize"** step again.</span></span>

9. <span data-ttu-id="35031-142">Zadejte e-mailovou adresu uživatele nebo skupiny, který by měly dostávat oznámení zřizování Chyba v **e-mailové oznámení** pole a zaškrtnutím políčka.</span><span class="sxs-lookup"><span data-stu-id="35031-142">Enter the email address of a person or group who should receive provisioning error notifications in the **Notification Email** field, and check the checkbox.</span></span>

10. <span data-ttu-id="35031-143">Klikněte na tlačítko **uložit.**</span><span class="sxs-lookup"><span data-stu-id="35031-143">Click **Save.**</span></span>

11. <span data-ttu-id="35031-144">V části mapování vyberte **synchronizaci Azure Active Directory Users na Dropbox pro firmy.**</span><span class="sxs-lookup"><span data-stu-id="35031-144">Under the Mappings section, select **Synchronize Azure Active Directory Users to Dropbox for Business.**</span></span>

12. <span data-ttu-id="35031-145">V **mapování atributů** , projděte si uživatelské atributy, které jsou synchronizované z Azure AD Dropbox pro firmy.</span><span class="sxs-lookup"><span data-stu-id="35031-145">In the **Attribute Mappings** section, review the user attributes that are synchronized from Azure AD to Dropbox for Business.</span></span> <span data-ttu-id="35031-146">Atributy vybrán jako **párování** vlastnosti se používají tak, aby odpovídaly uživatelské účty v Dropbox pro firmy pro operace aktualizace.</span><span class="sxs-lookup"><span data-stu-id="35031-146">The attributes selected as **Matching** properties are used to match the user accounts in Dropbox for Business for update operations.</span></span> <span data-ttu-id="35031-147">Kliknutím na tlačítko Uložit potvrzení změny.</span><span class="sxs-lookup"><span data-stu-id="35031-147">Select the Save button to commit any changes.</span></span>

13. <span data-ttu-id="35031-148">Povolit Azure AD zřizování služby pro Dropbox pro firmy, změňte **Stav zřizování** k **na** v části Nastavení</span><span class="sxs-lookup"><span data-stu-id="35031-148">To enable the Azure AD provisioning service for Dropbox for Business, change the **Provisioning Status** to **On** in the Settings section</span></span>

14. <span data-ttu-id="35031-149">Klikněte na tlačítko **uložit.**</span><span class="sxs-lookup"><span data-stu-id="35031-149">Click **Save.**</span></span>

<span data-ttu-id="35031-150">Spustí počáteční synchronizaci všech uživatelů a skupiny přiřazené k Dropboxu pro firmy v části Uživatelé a skupiny.</span><span class="sxs-lookup"><span data-stu-id="35031-150">It starts the initial synchronization of any users and/or groups assigned to Dropbox for Business in the Users and Groups section.</span></span> <span data-ttu-id="35031-151">Počáteční synchronizace trvá déle než následné synchronizace, ke kterým dochází přibližně každých 20 minut, dokud se službou provést.</span><span class="sxs-lookup"><span data-stu-id="35031-151">The initial sync takes longer to perform than subsequent syncs, which occur approximately every 20 minutes as long as the service is running.</span></span> <span data-ttu-id="35031-152">Můžete použít **podrobnosti synchronizace** části monitorovat průběh a odkazech zřízení sestavy aktivity, které popisují všechny akce, které provádí službu zřizování na vaší schránky pro obchodní aplikace.</span><span class="sxs-lookup"><span data-stu-id="35031-152">You can use the **Synchronization Details** section to monitor progress and follow links to provisioning activity reports, which describe all actions performed by the provisioning service on your Dropbox for Business app.</span></span>

<span data-ttu-id="35031-153">Nyní můžete vytvořit testovací účet.</span><span class="sxs-lookup"><span data-stu-id="35031-153">You can now create a test account.</span></span> <span data-ttu-id="35031-154">Chcete-li ověřit, že účet umístění byl synchronizován do Dropboxu pro firmy Počkejte až 20 minut.</span><span class="sxs-lookup"><span data-stu-id="35031-154">Wait for up to 20 minutes to verify that the account has been synchronized to Dropbox for Business.</span></span>

<span data-ttu-id="35031-155">Související stav je indikován úspěšně dokončila uživatele zřizování cyklu.</span><span class="sxs-lookup"><span data-stu-id="35031-155">A successfully completed user provisioning cycle is indicated by a related status.</span></span>

<span data-ttu-id="35031-156">![Přiřazení uživatelů](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/IC769523.png "přiřazení uživatelů")</span><span class="sxs-lookup"><span data-stu-id="35031-156">![Assign users](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/IC769523.png "Assign users")</span></span>


## <a name="additional-resources"></a><span data-ttu-id="35031-157">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="35031-157">Additional resources</span></span>

* [<span data-ttu-id="35031-158">Správa uživatelů zřizování účtu pro podnikové aplikace</span><span class="sxs-lookup"><span data-stu-id="35031-158">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="35031-159">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="35031-159">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="35031-160">Konfigurovat jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="35031-160">Configure Single Sign-on</span></span>](active-directory-saas-dropboxforbusiness-tutorial.md)