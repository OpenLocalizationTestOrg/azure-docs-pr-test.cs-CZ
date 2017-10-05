---
title: "Kurz: Konfigurace GitHub pro zřizování automatické uživatelů s Azure Active Directory | Microsoft Docs"
description: "Informace o konfiguraci Azure Active Directory a automaticky zřizovat a zrušte zřídit uživatelské účty na Githubu."
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
ms.openlocfilehash: 3cc70273e95dbf4913e7bbcd8a37bd9a52987b60
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/29/2017
---
# <a name="tutorial-configuring-github-for-automatic-user-provisioning"></a><span data-ttu-id="5cb6b-103">Kurz: Konfigurace GitHub pro zřizování automatické uživatelů</span><span class="sxs-lookup"><span data-stu-id="5cb6b-103">Tutorial: Configuring GitHub for Automatic User Provisioning</span></span>


<span data-ttu-id="5cb6b-104">Cílem tohoto kurzu je tak, aby zobrazovalo kroky, které je třeba provést v Githubu a Azure AD a automaticky zřizovat a zrušte zřízení uživatelských účtů ze služby Azure AD na Githubu.</span><span class="sxs-lookup"><span data-stu-id="5cb6b-104">The objective of this tutorial is to show you the steps you need to perform in GitHub and Azure AD to automatically provision and de-provision user accounts from Azure AD to GitHub.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="5cb6b-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5cb6b-105">Prerequisites</span></span>

<span data-ttu-id="5cb6b-106">Scénář uvedených v tomto kurzu se předpokládá, že už máte následující položky:</span><span class="sxs-lookup"><span data-stu-id="5cb6b-106">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

*   <span data-ttu-id="5cb6b-107">Klienta služby Azure Active directory</span><span class="sxs-lookup"><span data-stu-id="5cb6b-107">An Azure Active directory tenant</span></span>
*   <span data-ttu-id="5cb6b-108">Github klienta s [obchodní plán](https://help.github.com/articles/organization-billing-plans/#business-plan) nebo lépe povolena.</span><span class="sxs-lookup"><span data-stu-id="5cb6b-108">A Github tenant with the [Business plan](https://help.github.com/articles/organization-billing-plans/#business-plan) or better enabled</span></span> 
*   <span data-ttu-id="5cb6b-109">Uživatelský účet na webu GitHub s oprávnění správce</span><span class="sxs-lookup"><span data-stu-id="5cb6b-109">A user account in GitHub with Admin permissions</span></span> 

> [!NOTE]
> <span data-ttu-id="5cb6b-110">Azure AD zřizování integrace spoléhá na [Githubu SCIM API](https://developer.github.com/v3/scim/), který je k dispozici na Githubu týmy na obchodní plán nebo lepší.</span><span class="sxs-lookup"><span data-stu-id="5cb6b-110">The Azure AD provisioning integration relies on the [GitHub SCIM API](https://developer.github.com/v3/scim/), which is available to Github teams on the Business plan or better.</span></span>

## <a name="assigning-users-to-github"></a><span data-ttu-id="5cb6b-111">Přiřazení uživatelů ke Githubu</span><span class="sxs-lookup"><span data-stu-id="5cb6b-111">Assigning users to GitHub</span></span>

<span data-ttu-id="5cb6b-112">Azure Active Directory používá koncept označované jako "úlohy" k určení uživatelů, kteří obdrželi přístup k vybrané aplikace.</span><span class="sxs-lookup"><span data-stu-id="5cb6b-112">Azure Active Directory uses a concept called "assignments" to determine which users should receive access to selected apps.</span></span> <span data-ttu-id="5cb6b-113">V kontextu uživatele automatické zřizování účtu se synchronizují pouze uživatelé a skupiny, které byly "přiřazeny" aplikace ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5cb6b-113">In the context of automatic user account provisioning, only the users and groups that have been "assigned" to an application in Azure AD is synchronized.</span></span> 

<span data-ttu-id="5cb6b-114">Před konfigurací a povolení zřizování služby, musíte rozhodnout, jaké uživatelů nebo skupin ve službě Azure AD představují uživatele, kteří potřebují přístup k vaší aplikaci Githubu.</span><span class="sxs-lookup"><span data-stu-id="5cb6b-114">Before configuring and enabling the provisioning service, you need to decide what users and/or groups in Azure AD represent the users who need access to your GitHub app.</span></span> <span data-ttu-id="5cb6b-115">Jakmile se rozhodli, můžete přiřadit těmto uživatelům aplikace Githubu podle pokynů tady:</span><span class="sxs-lookup"><span data-stu-id="5cb6b-115">Once decided, you can assign these users to your GitHub app by following the instructions here:</span></span>

[<span data-ttu-id="5cb6b-116">Přiřazení uživatele nebo skupiny do aplikace enterprise</span><span class="sxs-lookup"><span data-stu-id="5cb6b-116">Assign a user or group to an enterprise app</span></span>](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-github"></a><span data-ttu-id="5cb6b-117">Důležité tipy pro přiřazení uživatelů ke Githubu</span><span class="sxs-lookup"><span data-stu-id="5cb6b-117">Important tips for assigning users to GitHub</span></span>

*   <span data-ttu-id="5cb6b-118">Dále je doporučeno jednoho uživatele Azure AD se přiřadí ke Githubu a otestovat konfiguraci zřizování.</span><span class="sxs-lookup"><span data-stu-id="5cb6b-118">It is recommended that a single Azure AD user is assigned to GitHub to test the provisioning configuration.</span></span> <span data-ttu-id="5cb6b-119">Další uživatele nebo skupiny může být přiřazen později.</span><span class="sxs-lookup"><span data-stu-id="5cb6b-119">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="5cb6b-120">Při přiřazení uživatele k Githubu, je nutné vybrat buď **uživatele** role nebo jinou platnou specifické pro aplikaci rolí (Pokud je k dispozici) v dialogovém okně přiřazení.</span><span class="sxs-lookup"><span data-stu-id="5cb6b-120">When assigning a user to GitHub, you must select either the **User** role, or another valid application-specific role (if available) in the assignment dialog.</span></span> <span data-ttu-id="5cb6b-121">**Výchozího přístupu k** role nefunguje pro zřizování a tito uživatelé se přeskočí.</span><span class="sxs-lookup"><span data-stu-id="5cb6b-121">The **Default Access** role does not work for provisioning, and these users are skipped.</span></span>


## <a name="configuring-user-provisioning-to-github"></a><span data-ttu-id="5cb6b-122">Konfiguraci zřizování uživatelů na Githubu</span><span class="sxs-lookup"><span data-stu-id="5cb6b-122">Configuring user provisioning to GitHub</span></span> 

<span data-ttu-id="5cb6b-123">Tato část vás provede připojení k Githubu uživatelský účet zřizování rozhraní API služby Azure AD a konfiguraci zřizování službu, kterou chcete vytvořit, aktualizovat a zakažte přiřazené uživatelské účty v Githubu podle přiřazení uživatelů a skupin ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5cb6b-123">This section guides you through connecting your Azure AD to GitHub's user account provisioning API, and configuring the provisioning service to create, update, and disable assigned user accounts in GitHub based on user and group assignment in Azure AD.</span></span>

> [!TIP]
> <span data-ttu-id="5cb6b-124">Můžete také povolit na základě SAML jednotné přihlašování pro GitHub, postupujte podle pokynů uvedených v [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="5cb6b-124">You may also choose to enabled SAML-based Single Sign-On for GitHub, following the instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="5cb6b-125">Jednotné přihlašování se dá nakonfigurovat nezávisle na automatické zřizování, i když tyto dvě funkce doplnění navzájem.</span><span class="sxs-lookup"><span data-stu-id="5cb6b-125">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>


### <a name="configure-automatic-user-account-provisioning-to-github-in-azure-ad"></a><span data-ttu-id="5cb6b-126">Konfigurace automatického účet zřizování uživatelů ke Githubu ve službě Azure AD</span><span class="sxs-lookup"><span data-stu-id="5cb6b-126">Configure automatic user account provisioning to GitHub in Azure AD</span></span>


1. <span data-ttu-id="5cb6b-127">V [portál Azure](https://portal.azure.com), vyhledejte **Azure Active Directory > podnikové aplikace > všechny aplikace** části.</span><span class="sxs-lookup"><span data-stu-id="5cb6b-127">In the [Azure portal](https://portal.azure.com), browse to the **Azure Active Directory > Enterprise Apps > All applications**  section.</span></span>

2. <span data-ttu-id="5cb6b-128">Pokud jste již nakonfigurovali GitHub pro jednotné přihlašování, vyhledejte instanci Githubu pomocí pole hledání.</span><span class="sxs-lookup"><span data-stu-id="5cb6b-128">If you have already configured GitHub for single sign-on, search for your instance of GitHub using the search field.</span></span> <span data-ttu-id="5cb6b-129">Jinak vyberte možnost **přidat** a vyhledejte **Githubu** v galerii aplikací.</span><span class="sxs-lookup"><span data-stu-id="5cb6b-129">Otherwise, select **Add** and search for **GitHub** in the application gallery.</span></span> <span data-ttu-id="5cb6b-130">Vyberte Githubu ve výsledcích hledání a přidejte ji do seznamu aplikací.</span><span class="sxs-lookup"><span data-stu-id="5cb6b-130">Select GitHub from the search results, and add it to your list of applications.</span></span>

3. <span data-ttu-id="5cb6b-131">Vyberte instanci Githubu a pak vyberte **zřizování** kartě.</span><span class="sxs-lookup"><span data-stu-id="5cb6b-131">Select your instance of GitHub, then select the **Provisioning** tab.</span></span>

4. <span data-ttu-id="5cb6b-132">Nastavte **režimu zřizování** k **automatické**.</span><span class="sxs-lookup"><span data-stu-id="5cb6b-132">Set the **Provisioning Mode** to **Automatic**.</span></span>

    ![Zřizování Githubu](./media/active-directory-saas-github-provisioning-tutorial/GitHub1.png)

5. <span data-ttu-id="5cb6b-134">V části **přihlašovací údaje správce** klikněte na tlačítko **Authorize**.</span><span class="sxs-lookup"><span data-stu-id="5cb6b-134">Under the **Admin Credentials** section, click **Authorize**.</span></span> <span data-ttu-id="5cb6b-135">Tato operace otevře dialogové okno Githubu autorizace v nové okno prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="5cb6b-135">This operation opens a GitHub authorization dialog in a new browser window.</span></span> 

6. <span data-ttu-id="5cb6b-136">V novém okně se přihlaste pomocí účtu správce Githubu.</span><span class="sxs-lookup"><span data-stu-id="5cb6b-136">In the new window, sign into GitHub using your Admin account.</span></span> <span data-ttu-id="5cb6b-137">V dialogovém okně výsledné autorizace, vyberte tým Githubu, který chcete povolit zajišťování pro a pak vyberte **Authorize**.</span><span class="sxs-lookup"><span data-stu-id="5cb6b-137">In the resulting authorization dialog, select the GitHub team that you want to enable provisioning for, and then select **Authorize**.</span></span> <span data-ttu-id="5cb6b-138">Po dokončení se vraťte k portálu Azure k dokončení konfigurace zřizování.</span><span class="sxs-lookup"><span data-stu-id="5cb6b-138">Once completed, return to the Azure portal to complete the provisioning configuration.</span></span>

    ![Dialogové okno autorizace](./media/active-directory-saas-github-provisioning-tutorial/GitHub2.png)

7. <span data-ttu-id="5cb6b-140">Na portálu Azure vstupní **URL klienta** a klikněte na tlačítko **Test připojení** zajistit Azure AD může připojit k aplikaci Githubu.</span><span class="sxs-lookup"><span data-stu-id="5cb6b-140">In the Azure portal, input **Tenant URL** and click **Test Connection** to ensure Azure AD can connect to your GitHub app.</span></span> <span data-ttu-id="5cb6b-141">Pokud se nepovede připojit, ujistěte se, váš účet GitHub má oprávnění správce a **URl klienta** je správně zadané hodnoty, a akci opakujte krok "Ověřit" (mohou představovat **URL klienta** pravidlem: "https:// API.github.com/scim/v2/Organizations/ + < Organizations_name > ", vaší organizace můžete najít v rámci účtu GitHub: **nastavení** > **organizace**).</span><span class="sxs-lookup"><span data-stu-id="5cb6b-141">If the connection fails, ensure your GitHub account has Admin permissions and **Tenant URl** is inputted correctly, then try the "Authorize" step again (you can constitute **Tenant URL** by rule: "https://api.github.com/scim/v2/organizations/ + <Organizations_name>", you can find your organizations under your GitHub account: **Settings** > **Organizations**).</span></span>

    ![Dialogové okno autorizace](./media/active-directory-saas-github-provisioning-tutorial/GitHub3.png)

8. <span data-ttu-id="5cb6b-143">Zadejte e-mailovou adresu uživatele nebo skupiny, který by měly dostávat oznámení zřizování Chyba v **e-mailové oznámení** pole a zaškrtněte políčko "Odesílat e-mailové oznámení, pokud dojde k chybě."</span><span class="sxs-lookup"><span data-stu-id="5cb6b-143">Enter the email address of a person or group who should receive provisioning error notifications in the **Notification Email** field, and check the checkbox "Send an email notification when a failure occurs."</span></span>

9. <span data-ttu-id="5cb6b-144">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="5cb6b-144">Click **Save**.</span></span> 

10. <span data-ttu-id="5cb6b-145">V části mapování vyberte **synchronizaci Azure Active Directory Users na Githubu**.</span><span class="sxs-lookup"><span data-stu-id="5cb6b-145">Under the Mappings section, select **Synchronize Azure Active Directory Users to GitHub**.</span></span>

11. <span data-ttu-id="5cb6b-146">V **mapování atributů** , projděte si uživatelské atributy, které jsou synchronizované z Azure AD Githubu.</span><span class="sxs-lookup"><span data-stu-id="5cb6b-146">In the **Attribute Mappings** section, review the user attributes that are synchronized from Azure AD to GitHub.</span></span> <span data-ttu-id="5cb6b-147">Atributy vybrán jako **párování** vlastnosti se používají tak, aby odpovídaly uživatelské účty v Githubu pro operace aktualizace.</span><span class="sxs-lookup"><span data-stu-id="5cb6b-147">The attributes selected as **Matching** properties are used to match the user accounts in GitHub for update operations.</span></span> <span data-ttu-id="5cb6b-148">Kliknutím na tlačítko Uložit potvrzení změny.</span><span class="sxs-lookup"><span data-stu-id="5cb6b-148">Select the Save button to commit any changes.</span></span>

12. <span data-ttu-id="5cb6b-149">Povolit Azure AD zřizování služby pro GitHub, změňte **Stav zřizování** k **na** v **nastavení** části</span><span class="sxs-lookup"><span data-stu-id="5cb6b-149">To enable the Azure AD provisioning service for GitHub, change the **Provisioning Status** to **On** in the **Settings** section</span></span>

13. <span data-ttu-id="5cb6b-150">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="5cb6b-150">Click **Save**.</span></span> 

<span data-ttu-id="5cb6b-151">Tato operace spustí počáteční synchronizaci všech uživatelů a skupiny přiřazené ke Githubu v části Uživatelé a skupiny.</span><span class="sxs-lookup"><span data-stu-id="5cb6b-151">This operation starts the initial synchronization of any users and/or groups assigned to GitHub in the Users and Groups section.</span></span> <span data-ttu-id="5cb6b-152">Počáteční synchronizace trvá déle než následné synchronizace, ke kterým dochází přibližně každých 20 minut, dokud se službou provést.</span><span class="sxs-lookup"><span data-stu-id="5cb6b-152">The initial sync takes longer to perform than subsequent syncs, which occur approximately every 20 minutes as long as the service is running.</span></span> <span data-ttu-id="5cb6b-153">Můžete použít **podrobnosti synchronizace** části monitorovat průběh a odkazech zřízení sestavy aktivity, které popisují všechny akce prováděné při zřizování služby.</span><span class="sxs-lookup"><span data-stu-id="5cb6b-153">You can use the **Synchronization Details** section to monitor progress and follow links to provisioning activity reports, which describe all actions performed by the provisioning service.</span></span>

<span data-ttu-id="5cb6b-154">Další informace o tom, jak číst zřizování protokoly služby Azure AD najdete v tématu [zprávy o zřizování účtu automatické uživatele](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span><span class="sxs-lookup"><span data-stu-id="5cb6b-154">For more information on how to read the Azure AD provisioning logs, see [Reporting on automatic user account provisioning](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span></span>


## <a name="additional-resources"></a><span data-ttu-id="5cb6b-155">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="5cb6b-155">Additional resources</span></span>

* [<span data-ttu-id="5cb6b-156">Správa uživatelů zřizování účtu pro podnikové aplikace</span><span class="sxs-lookup"><span data-stu-id="5cb6b-156">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-enterprise-apps-manage-provisioning.md)
* [<span data-ttu-id="5cb6b-157">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="5cb6b-157">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a><span data-ttu-id="5cb6b-158">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5cb6b-158">Next steps</span></span>

* [<span data-ttu-id="5cb6b-159">Zjistěte, jak získat sestavy o zřizování aktivity a zkontrolujte protokoly</span><span class="sxs-lookup"><span data-stu-id="5cb6b-159">Learn how to review logs and get reports on provisioning activity</span></span>](active-directory-saas-provisioning-reporting.md)
