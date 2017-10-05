---
title: 'Kurz: Azure Active Directory integrace s Citrix GoToMeeting | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Citrix GoToMeeting."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 0f59fedb-2cf8-48d2-a5fb-222ed943ff78
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 1ddfcd991431a11e5c3e306bd5905003d094ac18
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-configuring-citrix-gotomeeting-for-automatic-user-provisioning"></a><span data-ttu-id="79b02-103">Kurz: Konfigurace systému Citrix GoToMeeting pro zřizování automatické uživatelů</span><span class="sxs-lookup"><span data-stu-id="79b02-103">Tutorial: Configuring Citrix GoToMeeting for Automatic User Provisioning</span></span>

<span data-ttu-id="79b02-104">Cílem tohoto kurzu je tak, aby zobrazovalo kroky, které je třeba provést v Citrix GoToMeeting a Azure AD a automaticky zřizovat a zrušte zřízení uživatelských účtů ze služby Azure AD do Citrix GoToMeeting.</span><span class="sxs-lookup"><span data-stu-id="79b02-104">The objective of this tutorial is to show you the steps you need to perform in Citrix GoToMeeting and Azure AD to automatically provision and de-provision user accounts from Azure AD to Citrix GoToMeeting.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="79b02-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="79b02-105">Prerequisites</span></span>

<span data-ttu-id="79b02-106">Scénář uvedených v tomto kurzu se předpokládá, že už máte následující položky:</span><span class="sxs-lookup"><span data-stu-id="79b02-106">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

*   <span data-ttu-id="79b02-107">Klienta služby Azure Active directory.</span><span class="sxs-lookup"><span data-stu-id="79b02-107">An Azure Active directory tenant.</span></span>
*   <span data-ttu-id="79b02-108">Citrix GoToMeeting jednotného přihlašování povolené předplatné.</span><span class="sxs-lookup"><span data-stu-id="79b02-108">A Citrix GoToMeeting single-sign on enabled subscription.</span></span>
*   <span data-ttu-id="79b02-109">Uživatelský účet v Citrix GoToMeeting s oprávněními správce týmu.</span><span class="sxs-lookup"><span data-stu-id="79b02-109">A user account in Citrix GoToMeeting with Team Admin permissions.</span></span>

## <a name="assigning-users-to-citrix-gotomeeting"></a><span data-ttu-id="79b02-110">Přiřazení uživatelů k Citrix GoToMeeting</span><span class="sxs-lookup"><span data-stu-id="79b02-110">Assigning users to Citrix GoToMeeting</span></span>

<span data-ttu-id="79b02-111">Azure Active Directory používá koncept označované jako "úlohy" k určení uživatelů, kteří obdrželi přístup k vybrané aplikace.</span><span class="sxs-lookup"><span data-stu-id="79b02-111">Azure Active Directory uses a concept called "assignments" to determine which users should receive access to selected apps.</span></span> <span data-ttu-id="79b02-112">V kontextu uživatele automatické zřizování účtu se synchronizují pouze uživatelé a skupiny, které byly "přiřazeny" aplikace ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="79b02-112">In the context of automatic user account provisioning, only the users and groups that have been "assigned" to an application in Azure AD is synchronized.</span></span>

<span data-ttu-id="79b02-113">Před konfigurací a povolení zřizování služby, musíte rozhodnout, jaké uživatelů nebo skupin ve službě Azure AD představují uživatele, kteří potřebují přístup k vaší aplikaci Citrix GoToMeeting.</span><span class="sxs-lookup"><span data-stu-id="79b02-113">Before configuring and enabling the provisioning service, you need to decide what users and/or groups in Azure AD represent the users who need access to your Citrix GoToMeeting app.</span></span> <span data-ttu-id="79b02-114">Jakmile se rozhodli, můžete přiřadit těmto uživatelům aplikace Citrix GoToMeeting podle pokynů tady:</span><span class="sxs-lookup"><span data-stu-id="79b02-114">Once decided, you can assign these users to your Citrix GoToMeeting app by following the instructions here:</span></span>

[<span data-ttu-id="79b02-115">Přiřazení uživatele nebo skupiny do aplikace enterprise</span><span class="sxs-lookup"><span data-stu-id="79b02-115">Assign a user or group to an enterprise app</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-citrix-gotomeeting"></a><span data-ttu-id="79b02-116">Důležité tipy pro přiřazování uživatelů do Citrix GoToMeeting</span><span class="sxs-lookup"><span data-stu-id="79b02-116">Important tips for assigning users to Citrix GoToMeeting</span></span>

*   <span data-ttu-id="79b02-117">Dále je doporučeno jednoho uživatele Azure AD se přiřadí ke Citrix GoToMeeting a otestovat konfiguraci zřizování.</span><span class="sxs-lookup"><span data-stu-id="79b02-117">It is recommended that a single Azure AD user is assigned to Citrix GoToMeeting to test the provisioning configuration.</span></span> <span data-ttu-id="79b02-118">Další uživatele nebo skupiny může být přiřazen později.</span><span class="sxs-lookup"><span data-stu-id="79b02-118">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="79b02-119">Při přiřazování uživatele Citrix GoToMeeting, musíte vybrat platné uživatelské role.</span><span class="sxs-lookup"><span data-stu-id="79b02-119">When assigning a user to Citrix GoToMeeting, you must select a valid user role.</span></span> <span data-ttu-id="79b02-120">Roli "Výchozí přístup" nefunguje pro zřizování.</span><span class="sxs-lookup"><span data-stu-id="79b02-120">The "Default Access" role does not work for provisioning.</span></span>

## <a name="enable-automated-user-provisioning"></a><span data-ttu-id="79b02-121">Povolit automatické zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="79b02-121">Enable Automated User Provisioning</span></span>

<span data-ttu-id="79b02-122">Tato část vás provede připojení k systému Citrix GoToMeeting uživatelský účet zřizování rozhraní API služby Azure AD a konfiguraci zřizování službu, kterou chcete vytvořit, aktualizovat a zakázat přiřazené uživatelské účty v systému Citrix GoToMeeting na základě uživatele a skupiny přiřazení ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="79b02-122">This section guides you through connecting your Azure AD to Citrix GoToMeeting's user account provisioning API, and configuring the provisioning service to create, update, and disable assigned user accounts in Citrix GoToMeeting based on user and group assignment in Azure AD.</span></span>

> [!TIP]
> <span data-ttu-id="79b02-123">Také můžete povolit na základě SAML jednotné přihlašování pro Citrix GoToMeeting, postupujte podle pokynů uvedených v [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="79b02-123">You may also choose to enabled SAML-based Single Sign-On for Citrix GoToMeeting, following the instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="79b02-124">Jednotné přihlašování se dá nakonfigurovat nezávisle na automatické zřizování, i když tyto dvě funkce doplnění navzájem.</span><span class="sxs-lookup"><span data-stu-id="79b02-124">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>

### <a name="to-configure-automatic-user-account-provisioning"></a><span data-ttu-id="79b02-125">Konfigurace automatického uživatele zřizování účtu:</span><span class="sxs-lookup"><span data-stu-id="79b02-125">To configure automatic user account provisioning:</span></span>

1. <span data-ttu-id="79b02-126">V [portál Azure](https://portal.azure.com), vyhledejte **Azure Active Directory > podnikové aplikace > všechny aplikace** části.</span><span class="sxs-lookup"><span data-stu-id="79b02-126">In the [Azure portal](https://portal.azure.com), browse to the **Azure Active Directory > Enterprise Apps > All applications** section.</span></span>

2. <span data-ttu-id="79b02-127">Pokud jste již nakonfigurovali Citrix GoToMeeting pro jednotné přihlašování, vyhledejte instanci Citrix GoToMeeting pomocí pole hledání.</span><span class="sxs-lookup"><span data-stu-id="79b02-127">If you have already configured Citrix GoToMeeting for single sign-on, search for your instance of Citrix GoToMeeting using the search field.</span></span> <span data-ttu-id="79b02-128">Jinak vyberte možnost **přidat** a vyhledejte **Citrix GoToMeeting** v galerii aplikací.</span><span class="sxs-lookup"><span data-stu-id="79b02-128">Otherwise, select **Add** and search for **Citrix GoToMeeting** in the application gallery.</span></span> <span data-ttu-id="79b02-129">Vyberte Citrix GoToMeeting ve výsledcích hledání a přidejte ji do seznamu aplikací.</span><span class="sxs-lookup"><span data-stu-id="79b02-129">Select Citrix GoToMeeting from the search results, and add it to your list of applications.</span></span>

3. <span data-ttu-id="79b02-130">Vyberte instanci Citrix GoToMeeting a pak vyberte **zřizování** kartě.</span><span class="sxs-lookup"><span data-stu-id="79b02-130">Select your instance of Citrix GoToMeeting, then select the **Provisioning** tab.</span></span>

4. <span data-ttu-id="79b02-131">Nastavte **zřizování** režim **automatické**.</span><span class="sxs-lookup"><span data-stu-id="79b02-131">Set the **Provisioning** Mode to **Automatic**.</span></span> 

    ![Zřizování](./media/active-directory-saas-citrixgotomeeting-provisioning-tutorial/provisioning.png)

5. <span data-ttu-id="79b02-133">V části přihlašovací údaje správce proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="79b02-133">Under the Admin Credentials section, perform the following steps:</span></span>
   
    <span data-ttu-id="79b02-134">a.</span><span class="sxs-lookup"><span data-stu-id="79b02-134">a.</span></span> <span data-ttu-id="79b02-135">V **uživatelské jméno správce GoToMeeting Citrix** textovému poli, zadejte uživatelské jméno správce.</span><span class="sxs-lookup"><span data-stu-id="79b02-135">In the **Citrix GoToMeeting Admin User Name** textbox, type the user name of an administrator.</span></span>

    <span data-ttu-id="79b02-136">b.</span><span class="sxs-lookup"><span data-stu-id="79b02-136">b.</span></span> <span data-ttu-id="79b02-137">V **heslo správce systému Citrix GoToMeeting** textovému poli, heslo správce.</span><span class="sxs-lookup"><span data-stu-id="79b02-137">In the **Citrix GoToMeeting Admin Password** textbox, the administrator's password.</span></span>

6. <span data-ttu-id="79b02-138">Na portálu Azure klikněte na tlačítko **Test připojení** zajistit Azure AD může připojit k aplikaci Citrix GoToMeeting.</span><span class="sxs-lookup"><span data-stu-id="79b02-138">In the Azure portal, click **Test Connection** to ensure Azure AD can connect to your Citrix GoToMeeting app.</span></span> <span data-ttu-id="79b02-139">Pokud připojení nezdaří, zkontrolujte účtu Citrix GoToMeeting má oprávnění správce týmu a zkuste to **"Přihlašovací údaje správce"** krok opakujte.</span><span class="sxs-lookup"><span data-stu-id="79b02-139">If the connection fails, ensure your Citrix GoToMeeting account has Team Admin permissions and try the **"Admin Credentials"** step again.</span></span>

7. <span data-ttu-id="79b02-140">Zadejte e-mailovou adresu uživatele nebo skupiny, který by měly dostávat oznámení zřizování Chyba v **e-mailové oznámení** pole a zaškrtnutím políčka.</span><span class="sxs-lookup"><span data-stu-id="79b02-140">Enter the email address of a person or group who should receive provisioning error notifications in the **Notification Email** field, and check the checkbox.</span></span>

8. <span data-ttu-id="79b02-141">Klikněte na tlačítko **uložit.**</span><span class="sxs-lookup"><span data-stu-id="79b02-141">Click **Save.**</span></span>

9. <span data-ttu-id="79b02-142">V části mapování vyberte **synchronizaci Azure Active Directory uživatelům Citrix GoToMeeting.**</span><span class="sxs-lookup"><span data-stu-id="79b02-142">Under the Mappings section, select **Synchronize Azure Active Directory Users to Citrix GoToMeeting.**</span></span>

10. <span data-ttu-id="79b02-143">V **mapování atributů** , projděte si uživatelské atributy, které jsou synchronizované z Azure AD Citrix GoToMeeting.</span><span class="sxs-lookup"><span data-stu-id="79b02-143">In the **Attribute Mappings** section, review the user attributes that are synchronized from Azure AD to Citrix GoToMeeting.</span></span> <span data-ttu-id="79b02-144">Atributy vybrán jako **párování** vlastnosti se používají tak, aby odpovídaly uživatelské účty v systému Citrix GoToMeeting pro operace aktualizace.</span><span class="sxs-lookup"><span data-stu-id="79b02-144">The attributes selected as **Matching** properties are used to match the user accounts in Citrix GoToMeeting for update operations.</span></span> <span data-ttu-id="79b02-145">Kliknutím na tlačítko Uložit potvrzení změny.</span><span class="sxs-lookup"><span data-stu-id="79b02-145">Select the Save button to commit any changes.</span></span>

11. <span data-ttu-id="79b02-146">Povolit Azure AD zřizování služby pro Citrix GoToMeeting, změňte **Stav zřizování** k **na** v části Nastavení</span><span class="sxs-lookup"><span data-stu-id="79b02-146">To enable the Azure AD provisioning service for Citrix GoToMeeting, change the **Provisioning Status** to **On** in the Settings section</span></span>

12. <span data-ttu-id="79b02-147">Klikněte na tlačítko **uložit.**</span><span class="sxs-lookup"><span data-stu-id="79b02-147">Click **Save.**</span></span>

<span data-ttu-id="79b02-148">Spustí počáteční synchronizaci všech uživatelů a skupiny přiřazené k Citrix GoToMeeting v části Uživatelé a skupiny.</span><span class="sxs-lookup"><span data-stu-id="79b02-148">It starts the initial synchronization of any users and/or groups assigned to Citrix GoToMeeting in the Users and Groups section.</span></span> <span data-ttu-id="79b02-149">Počáteční synchronizace trvá déle než následné synchronizace, ke kterým dochází přibližně každých 20 minut, dokud se službou provést.</span><span class="sxs-lookup"><span data-stu-id="79b02-149">The initial sync takes longer to perform than subsequent syncs, which occur approximately every 20 minutes as long as the service is running.</span></span> <span data-ttu-id="79b02-150">Můžete použít **podrobnosti synchronizace** části monitorovat průběh a odkazech zřízení sestavy aktivity, které popisují všechny akce prováděné při zřizování služby ve vaší aplikaci Citrix GoToMeeting.</span><span class="sxs-lookup"><span data-stu-id="79b02-150">You can use the **Synchronization Details** section to monitor progress and follow links to provisioning activity reports, which describe all actions performed by the provisioning service on your Citrix GoToMeeting app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="79b02-151">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="79b02-151">Additional resources</span></span>

* [<span data-ttu-id="79b02-152">Správa uživatelů zřizování účtu pro podnikové aplikace</span><span class="sxs-lookup"><span data-stu-id="79b02-152">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="79b02-153">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="79b02-153">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="79b02-154">Konfigurovat jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="79b02-154">Configure Single Sign-on</span></span>](active-directory-saas-citrix-gotomeeting-tutorial.md)


