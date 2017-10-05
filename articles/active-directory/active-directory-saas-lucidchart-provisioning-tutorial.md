---
title: "Kurz: Konfigurace LucidChart pro zřizování automatické uživatelů s Azure Active Directory | Microsoft Docs"
description: "Informace o konfiguraci Azure Active Directory a automaticky zřizovat a zrušte zřízení uživatelských účtů do LucidChart."
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
ms.date: 07/14/2017
ms.author: asmalser-msft
ms.openlocfilehash: 1f9344a5e750360e21ed7dc8e3ed013c2c2e1a45
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/29/2017
---
# <a name="tutorial-configuring-lucidchart-for-automatic-user-provisioning"></a><span data-ttu-id="65d76-103">Kurz: Konfigurace LucidChart pro zřizování automatické uživatelů</span><span class="sxs-lookup"><span data-stu-id="65d76-103">Tutorial: Configuring LucidChart for Automatic User Provisioning</span></span>


<span data-ttu-id="65d76-104">Cílem tohoto kurzu je tak, aby zobrazovalo kroky, které je třeba provést v LucidChart a Azure AD a automaticky zřizovat a zrušte zřízení uživatelských účtů ze služby Azure AD do LucidChart.</span><span class="sxs-lookup"><span data-stu-id="65d76-104">The objective of this tutorial is to show you the steps you need to perform in LucidChart and Azure AD to automatically provision and de-provision user accounts from Azure AD to LucidChart.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="65d76-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="65d76-105">Prerequisites</span></span>

<span data-ttu-id="65d76-106">Scénář uvedených v tomto kurzu se předpokládá, že už máte následující položky:</span><span class="sxs-lookup"><span data-stu-id="65d76-106">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

*   <span data-ttu-id="65d76-107">Klienta služby Azure Active directory</span><span class="sxs-lookup"><span data-stu-id="65d76-107">An Azure Active directory tenant</span></span>
*   <span data-ttu-id="65d76-108">LucidChart klienta s [plánu podnikového](https://www.lucidchart.com/user/117598685#/subscriptionLevel) nebo lépe povolena.</span><span class="sxs-lookup"><span data-stu-id="65d76-108">A LucidChart tenant with the [Enterprise plan](https://www.lucidchart.com/user/117598685#/subscriptionLevel) or better enabled</span></span> 
*   <span data-ttu-id="65d76-109">Uživatelský účet v LucidChart s oprávněními správce</span><span class="sxs-lookup"><span data-stu-id="65d76-109">A user account in LucidChart with Admin permissions</span></span> 

## <a name="assigning-users-to-lucidchart"></a><span data-ttu-id="65d76-110">Přiřazení uživatelů k LucidChart</span><span class="sxs-lookup"><span data-stu-id="65d76-110">Assigning users to LucidChart</span></span>

<span data-ttu-id="65d76-111">Azure Active Directory používá koncept označované jako "úlohy" k určení uživatelů, kteří obdrželi přístup k vybrané aplikace.</span><span class="sxs-lookup"><span data-stu-id="65d76-111">Azure Active Directory uses a concept called "assignments" to determine which users should receive access to selected apps.</span></span> <span data-ttu-id="65d76-112">V kontextu uživatele automatické zřizování účtu se synchronizují pouze uživatelé a skupiny, které byly "přiřazeny" aplikace ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="65d76-112">In the context of automatic user account provisioning, only the users and groups that have been "assigned" to an application in Azure AD is synchronized.</span></span> 

<span data-ttu-id="65d76-113">Před konfigurací a povolení zřizování služby, musíte rozhodnout, jaké uživatelů nebo skupin ve službě Azure AD představují uživatele, kteří potřebují přístup k vaší aplikaci LucidChart.</span><span class="sxs-lookup"><span data-stu-id="65d76-113">Before configuring and enabling the provisioning service, you need to decide what users and/or groups in Azure AD represent the users who need access to your LucidChart app.</span></span> <span data-ttu-id="65d76-114">Jakmile se rozhodli, můžete přiřadit těmto uživatelům aplikace LucidChart podle pokynů tady:</span><span class="sxs-lookup"><span data-stu-id="65d76-114">Once decided, you can assign these users to your LucidChart app by following the instructions here:</span></span>

[<span data-ttu-id="65d76-115">Přiřazení uživatele nebo skupiny do aplikace enterprise</span><span class="sxs-lookup"><span data-stu-id="65d76-115">Assign a user or group to an enterprise app</span></span>](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-lucidchart"></a><span data-ttu-id="65d76-116">Důležité tipy pro přiřazování uživatelů do LucidChart</span><span class="sxs-lookup"><span data-stu-id="65d76-116">Important tips for assigning users to LucidChart</span></span>

*   <span data-ttu-id="65d76-117">Dále je doporučeno jednoho uživatele Azure AD se přiřadí ke LucidChart a otestovat konfiguraci zřizování.</span><span class="sxs-lookup"><span data-stu-id="65d76-117">It is recommended that a single Azure AD user is assigned to LucidChart to test the provisioning configuration.</span></span> <span data-ttu-id="65d76-118">Další uživatele nebo skupiny může být přiřazen později.</span><span class="sxs-lookup"><span data-stu-id="65d76-118">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="65d76-119">Při přiřazení uživatele k LucidChart, je nutné vybrat buď **uživatele** role nebo jinou platnou specifické pro aplikaci rolí (Pokud je k dispozici) v dialogovém okně přiřazení.</span><span class="sxs-lookup"><span data-stu-id="65d76-119">When assigning a user to LucidChart, you must select either the **User** role, or another valid application-specific role (if available) in the assignment dialog.</span></span> <span data-ttu-id="65d76-120">**Výchozího přístupu k** role nefunguje pro zřizování a tito uživatelé se přeskočí.</span><span class="sxs-lookup"><span data-stu-id="65d76-120">The **Default Access** role does not work for provisioning, and these users are skipped.</span></span>


## <a name="configuring-user-provisioning-to-lucidchart"></a><span data-ttu-id="65d76-121">Konfiguraci zřizování uživatelů k LucidChart</span><span class="sxs-lookup"><span data-stu-id="65d76-121">Configuring user provisioning to LucidChart</span></span> 

<span data-ttu-id="65d76-122">Tato část vás provede připojení k LucidChart na uživatelský účet zřizování rozhraní API služby Azure AD a konfiguraci zřizování službu, kterou chcete vytvořit, aktualizovat a zakažte přiřazené uživatelské účty v LucidChart podle přiřazení uživatelů a skupin ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="65d76-122">This section guides you through connecting your Azure AD to LucidChart's user account provisioning API, and configuring the provisioning service to create, update, and disable assigned user accounts in LucidChart based on user and group assignment in Azure AD.</span></span>

> [!TIP]
> <span data-ttu-id="65d76-123">Můžete také pro LucidChart povoleno na základě SAML jednotné přihlašování, postupujte podle pokynů uvedených v [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="65d76-123">You may also choose to enabled SAML-based Single Sign-On for LucidChart, following the instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="65d76-124">Jednotné přihlašování se dá nakonfigurovat nezávisle na automatické zřizování, i když tyto dvě funkce doplnění navzájem.</span><span class="sxs-lookup"><span data-stu-id="65d76-124">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>


### <a name="configure-automatic-user-account-provisioning-to-lucidchart-in-azure-ad"></a><span data-ttu-id="65d76-125">Konfigurace automatického účet zřizování uživatelů k LucidChart ve službě Azure AD</span><span class="sxs-lookup"><span data-stu-id="65d76-125">Configure automatic user account provisioning to LucidChart in Azure AD</span></span>


1. <span data-ttu-id="65d76-126">V [portál Azure](https://portal.azure.com), vyhledejte **Azure Active Directory > podnikové aplikace > všechny aplikace** části.</span><span class="sxs-lookup"><span data-stu-id="65d76-126">In the [Azure portal](https://portal.azure.com), browse to the **Azure Active Directory > Enterprise Apps > All applications**  section.</span></span>

2. <span data-ttu-id="65d76-127">Pokud jste již nakonfigurovali LucidChart pro jednotné přihlašování, vyhledejte instanci LucidChart pomocí pole hledání.</span><span class="sxs-lookup"><span data-stu-id="65d76-127">If you have already configured LucidChart for single sign-on, search for your instance of LucidChart using the search field.</span></span> <span data-ttu-id="65d76-128">Jinak vyberte možnost **přidat** a vyhledejte **LucidChart** v galerii aplikací.</span><span class="sxs-lookup"><span data-stu-id="65d76-128">Otherwise, select **Add** and search for **LucidChart** in the application gallery.</span></span> <span data-ttu-id="65d76-129">Vyberte LucidChart ve výsledcích hledání a přidejte ji do seznamu aplikací.</span><span class="sxs-lookup"><span data-stu-id="65d76-129">Select LucidChart from the search results, and add it to your list of applications.</span></span>

3. <span data-ttu-id="65d76-130">Vyberte instanci LucidChart a pak vyberte **zřizování** kartě.</span><span class="sxs-lookup"><span data-stu-id="65d76-130">Select your instance of LucidChart, then select the **Provisioning** tab.</span></span>

4. <span data-ttu-id="65d76-131">Nastavte **režimu zřizování** k **automatické**.</span><span class="sxs-lookup"><span data-stu-id="65d76-131">Set the **Provisioning Mode** to **Automatic**.</span></span>

    ![Zřizování LucidChart](./media/active-directory-saas-lucidchart-provisioning-tutorial/LucidChart1.png)

5. <span data-ttu-id="65d76-133">V části **přihlašovací údaje správce** části, zadejte **tajný klíč tokenu** generované vaší LucidChart účtu (token můžete najít v rámci vašeho účtu: **Team** > **integrace aplikací** > **SCIM**).</span><span class="sxs-lookup"><span data-stu-id="65d76-133">Under the **Admin Credentials** section, input the **Secret Token** generated by your LucidChart's account (you can find the token under your account: **Team** > **App Integration** > **SCIM**).</span></span> 

    ![Zřizování LucidChart](./media/active-directory-saas-lucidchart-provisioning-tutorial/LucidChart2.png)

6. <span data-ttu-id="65d76-135">Na portálu Azure klikněte na tlačítko **Test připojení** zajistit Azure AD může připojit k aplikaci LucidChart.</span><span class="sxs-lookup"><span data-stu-id="65d76-135">In the Azure portal, click **Test Connection** to ensure Azure AD can connect to your LucidChart app.</span></span> <span data-ttu-id="65d76-136">Pokud se nepovede připojit, zajistěte, aby byl váš účet LucidChart oprávnění správce a opakujte krok 5.</span><span class="sxs-lookup"><span data-stu-id="65d76-136">If the connection fails, ensure your LucidChart account has Admin permissions and try step 5 again.</span></span>

7. <span data-ttu-id="65d76-137">Zadejte e-mailovou adresu uživatele nebo skupiny, který by měly dostávat oznámení zřizování Chyba v **e-mailové oznámení** pole a zaškrtněte políčko "Odesílat e-mailové oznámení, pokud dojde k chybě."</span><span class="sxs-lookup"><span data-stu-id="65d76-137">Enter the email address of a person or group who should receive provisioning error notifications in the **Notification Email** field, and check the checkbox "Send an email notification when a failure occurs."</span></span>

8. <span data-ttu-id="65d76-138">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="65d76-138">Click **Save**.</span></span> 

9. <span data-ttu-id="65d76-139">V části mapování vyberte **synchronizaci Azure Active Directory uživatelům LucidChart**.</span><span class="sxs-lookup"><span data-stu-id="65d76-139">Under the Mappings section, select **Synchronize Azure Active Directory Users to LucidChart**.</span></span>

10. <span data-ttu-id="65d76-140">V **mapování atributů** , projděte si uživatelské atributy, které jsou synchronizované z Azure AD LucidChart.</span><span class="sxs-lookup"><span data-stu-id="65d76-140">In the **Attribute Mappings** section, review the user attributes that are synchronized from Azure AD to LucidChart.</span></span> <span data-ttu-id="65d76-141">Atributy vybrán jako **párování** vlastnosti se používají tak, aby odpovídaly uživatelské účty v LucidChart pro operace aktualizace.</span><span class="sxs-lookup"><span data-stu-id="65d76-141">The attributes selected as **Matching** properties are used to match the user accounts in LucidChart for update operations.</span></span> <span data-ttu-id="65d76-142">Kliknutím na tlačítko Uložit potvrzení změny.</span><span class="sxs-lookup"><span data-stu-id="65d76-142">Select the Save button to commit any changes.</span></span>

11. <span data-ttu-id="65d76-143">Povolit zřizování služby pro LucidChart Azure AD, změňte **Stav zřizování** k **na** v **nastavení** části</span><span class="sxs-lookup"><span data-stu-id="65d76-143">To enable the Azure AD provisioning service for LucidChart, change the **Provisioning Status** to **On** in the **Settings** section</span></span>

12. <span data-ttu-id="65d76-144">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="65d76-144">Click **Save**.</span></span> 

<span data-ttu-id="65d76-145">Tato operace spustí počáteční synchronizaci všech uživatelů a skupiny přiřazené k LucidChart v části Uživatelé a skupiny.</span><span class="sxs-lookup"><span data-stu-id="65d76-145">This operation starts the initial synchronization of any users and/or groups assigned to LucidChart in the Users and Groups section.</span></span> <span data-ttu-id="65d76-146">Počáteční synchronizace trvá déle než následné synchronizace, ke kterým dochází přibližně každých 20 minut, dokud se službou provést.</span><span class="sxs-lookup"><span data-stu-id="65d76-146">The initial sync takes longer to perform than subsequent syncs, which occur approximately every 20 minutes as long as the service is running.</span></span> <span data-ttu-id="65d76-147">Můžete použít **podrobnosti synchronizace** části monitorovat průběh a odkazech zřízení sestavy aktivity, které popisují všechny akce prováděné při zřizování služby.</span><span class="sxs-lookup"><span data-stu-id="65d76-147">You can use the **Synchronization Details** section to monitor progress and follow links to provisioning activity reports, which describe all actions performed by the provisioning service.</span></span>

<span data-ttu-id="65d76-148">Další informace o tom, jak číst zřizování protokoly služby Azure AD najdete v tématu [zprávy o zřizování účtu automatické uživatele](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span><span class="sxs-lookup"><span data-stu-id="65d76-148">For more information on how to read the Azure AD provisioning logs, see [Reporting on automatic user account provisioning](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span></span>


## <a name="additional-resources"></a><span data-ttu-id="65d76-149">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="65d76-149">Additional resources</span></span>

* [<span data-ttu-id="65d76-150">Správa uživatelů zřizování účtu pro podnikové aplikace</span><span class="sxs-lookup"><span data-stu-id="65d76-150">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-enterprise-apps-manage-provisioning.md)
* [<span data-ttu-id="65d76-151">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="65d76-151">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a><span data-ttu-id="65d76-152">Další kroky</span><span class="sxs-lookup"><span data-stu-id="65d76-152">Next steps</span></span>

* [<span data-ttu-id="65d76-153">Zjistěte, jak získat sestavy o zřizování aktivity a zkontrolujte protokoly</span><span class="sxs-lookup"><span data-stu-id="65d76-153">Learn how to review logs and get reports on provisioning activity</span></span>](active-directory-saas-provisioning-reporting.md)
