---
title: "Kurz: Konfigurace ThousandEyes pro zřizování automatické uživatelů s Azure Active Directory | Microsoft Docs"
description: "Informace o konfiguraci Azure Active Directory a automaticky zřizovat a zrušte zřízení uživatelských účtů do ThousandEyes."
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
ms.openlocfilehash: e6bc2eab3cc1adcf26857ed98d920177a51455ea
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/29/2017
---
# <a name="tutorial-configuring-thousandeyes-for-automatic-user-provisioning"></a><span data-ttu-id="cfefc-103">Kurz: Konfigurace ThousandEyes pro zřizování automatické uživatelů</span><span class="sxs-lookup"><span data-stu-id="cfefc-103">Tutorial: Configuring ThousandEyes for Automatic User Provisioning</span></span>


<span data-ttu-id="cfefc-104">Cílem tohoto kurzu je tak, aby zobrazovalo kroky, které je třeba provést v ThousandEyes a Azure AD a automaticky zřizovat a zrušte zřízení uživatelských účtů ze služby Azure AD do ThousandEyes.</span><span class="sxs-lookup"><span data-stu-id="cfefc-104">The objective of this tutorial is to show you the steps you need to perform in ThousandEyes and Azure AD to automatically provision and de-provision user accounts from Azure AD to ThousandEyes.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="cfefc-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="cfefc-105">Prerequisites</span></span>

<span data-ttu-id="cfefc-106">Scénář uvedených v tomto kurzu se předpokládá, že už máte následující položky:</span><span class="sxs-lookup"><span data-stu-id="cfefc-106">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

*   <span data-ttu-id="cfefc-107">Klienta služby Azure Active directory</span><span class="sxs-lookup"><span data-stu-id="cfefc-107">An Azure Active directory tenant</span></span>
*   <span data-ttu-id="cfefc-108">ThousandEyes klienta s [standardní plán](https://www.thousandeyes.com/pricing) nebo lépe povolena.</span><span class="sxs-lookup"><span data-stu-id="cfefc-108">A ThousandEyes tenant with the [Standard plan](https://www.thousandeyes.com/pricing) or better enabled</span></span> 
*   <span data-ttu-id="cfefc-109">Uživatelský účet v ThousandEyes s oprávněními správce</span><span class="sxs-lookup"><span data-stu-id="cfefc-109">A user account in ThousandEyes with Admin permissions</span></span> 

> [!NOTE]
> <span data-ttu-id="cfefc-110">Azure AD zřizování integrace spoléhá na [ThousandEyes SCIM API](https://success.thousandeyes.com/PublicArticlePage?articleIdParam=kA044000000CnWrCAK), který je k dispozici pro týmy ThousandEyes na plán Standard nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="cfefc-110">The Azure AD provisioning integration relies on the [ThousandEyes SCIM API](https://success.thousandeyes.com/PublicArticlePage?articleIdParam=kA044000000CnWrCAK), which is available to ThousandEyes teams on the Standard plan or better.</span></span>

## <a name="assigning-users-to-thousandeyes"></a><span data-ttu-id="cfefc-111">Přiřazení uživatelů k ThousandEyes</span><span class="sxs-lookup"><span data-stu-id="cfefc-111">Assigning users to ThousandEyes</span></span>

<span data-ttu-id="cfefc-112">Azure Active Directory používá koncept označované jako "úlohy" k určení uživatelů, kteří obdrželi přístup k vybrané aplikace.</span><span class="sxs-lookup"><span data-stu-id="cfefc-112">Azure Active Directory uses a concept called "assignments" to determine which users should receive access to selected apps.</span></span> <span data-ttu-id="cfefc-113">V kontextu uživatele automatické zřizování účtu se synchronizují pouze uživatelé a skupiny, které byly "přiřazeny" aplikace ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="cfefc-113">In the context of automatic user account provisioning, only the users and groups that have been "assigned" to an application in Azure AD is synchronized.</span></span> 

<span data-ttu-id="cfefc-114">Před konfigurací a povolení zřizování služby, musíte rozhodnout, jaké uživatelů nebo skupin ve službě Azure AD představují uživatele, kteří potřebují přístup k vaší aplikaci ThousandEyes.</span><span class="sxs-lookup"><span data-stu-id="cfefc-114">Before configuring and enabling the provisioning service, you need to decide what users and/or groups in Azure AD represent the users who need access to your ThousandEyes app.</span></span> <span data-ttu-id="cfefc-115">Jakmile se rozhodli, můžete přiřadit těmto uživatelům aplikace ThousandEyes podle pokynů tady:</span><span class="sxs-lookup"><span data-stu-id="cfefc-115">Once decided, you can assign these users to your ThousandEyes app by following the instructions here:</span></span>

[<span data-ttu-id="cfefc-116">Přiřazení uživatele nebo skupiny do aplikace enterprise</span><span class="sxs-lookup"><span data-stu-id="cfefc-116">Assign a user or group to an enterprise app</span></span>](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-thousandeyes"></a><span data-ttu-id="cfefc-117">Důležité tipy pro přiřazování uživatelů do ThousandEyes</span><span class="sxs-lookup"><span data-stu-id="cfefc-117">Important tips for assigning users to ThousandEyes</span></span>

*   <span data-ttu-id="cfefc-118">Dále je doporučeno jednoho uživatele Azure AD se přiřadí ke ThousandEyes a otestovat konfiguraci zřizování.</span><span class="sxs-lookup"><span data-stu-id="cfefc-118">It is recommended that a single Azure AD user is assigned to ThousandEyes to test the provisioning configuration.</span></span> <span data-ttu-id="cfefc-119">Další uživatele nebo skupiny může být přiřazen později.</span><span class="sxs-lookup"><span data-stu-id="cfefc-119">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="cfefc-120">Při přiřazení uživatele k ThousandEyes, je nutné vybrat buď **uživatele** role nebo jinou platnou specifické pro aplikaci rolí (Pokud je k dispozici) v dialogovém okně přiřazení.</span><span class="sxs-lookup"><span data-stu-id="cfefc-120">When assigning a user to ThousandEyes, you must select either the **User** role, or another valid application-specific role (if available) in the assignment dialog.</span></span> <span data-ttu-id="cfefc-121">**Výchozího přístupu k** role nefunguje pro zřizování a tito uživatelé se přeskočí.</span><span class="sxs-lookup"><span data-stu-id="cfefc-121">The **Default Access** role does not work for provisioning, and these users are skipped.</span></span>


## <a name="configuring-user-provisioning-to-thousandeyes"></a><span data-ttu-id="cfefc-122">Konfiguraci zřizování uživatelů k ThousandEyes</span><span class="sxs-lookup"><span data-stu-id="cfefc-122">Configuring user provisioning to ThousandEyes</span></span> 

<span data-ttu-id="cfefc-123">Tato část vás provede připojení k ThousandEyes na uživatelský účet zřizování rozhraní API služby Azure AD a konfiguraci zřizování službu, kterou chcete vytvořit, aktualizovat a zakázat přiřazené uživatelské účty v ThousandEyes podle přiřazení uživatelů a skupin ve službě Azure AD .</span><span class="sxs-lookup"><span data-stu-id="cfefc-123">This section guides you through connecting your Azure AD to ThousandEyes's user account provisioning API, and configuring the provisioning service to create, update, and disable assigned user accounts in ThousandEyes based on user and group assignment in Azure AD.</span></span>

> [!TIP]
> <span data-ttu-id="cfefc-124">Můžete také pro ThousandEyes povoleno na základě SAML jednotné přihlašování, postupujte podle pokynů uvedených v [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="cfefc-124">You may also choose to enabled SAML-based Single Sign-On for ThousandEyes, following the instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="cfefc-125">Jednotné přihlašování se dá nakonfigurovat nezávisle na automatické zřizování, i když tyto dvě funkce doplnění navzájem.</span><span class="sxs-lookup"><span data-stu-id="cfefc-125">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>


### <a name="configure-automatic-user-account-provisioning-to-thousandeyes-in-azure-ad"></a><span data-ttu-id="cfefc-126">Konfigurace automatického účet zřizování uživatelů k ThousandEyes ve službě Azure AD</span><span class="sxs-lookup"><span data-stu-id="cfefc-126">Configure automatic user account provisioning to ThousandEyes in Azure AD</span></span>


1. <span data-ttu-id="cfefc-127">V [portál Azure](https://portal.azure.com), vyhledejte **Azure Active Directory > podnikové aplikace > všechny aplikace** části.</span><span class="sxs-lookup"><span data-stu-id="cfefc-127">In the [Azure portal](https://portal.azure.com), browse to the **Azure Active Directory > Enterprise Apps > All applications**  section.</span></span>

2. <span data-ttu-id="cfefc-128">Pokud jste již nakonfigurovali ThousandEyes pro jednotné přihlašování, vyhledejte instanci ThousandEyes pomocí pole hledání.</span><span class="sxs-lookup"><span data-stu-id="cfefc-128">If you have already configured ThousandEyes for single sign-on, search for your instance of ThousandEyes using the search field.</span></span> <span data-ttu-id="cfefc-129">Jinak vyberte možnost **přidat** a vyhledejte **ThousandEyes** v galerii aplikací.</span><span class="sxs-lookup"><span data-stu-id="cfefc-129">Otherwise, select **Add** and search for **ThousandEyes** in the application gallery.</span></span> <span data-ttu-id="cfefc-130">Vyberte ThousandEyes ve výsledcích hledání a přidejte ji do seznamu aplikací.</span><span class="sxs-lookup"><span data-stu-id="cfefc-130">Select ThousandEyes from the search results, and add it to your list of applications.</span></span>

3. <span data-ttu-id="cfefc-131">Vyberte instanci ThousandEyes a pak vyberte **zřizování** kartě.</span><span class="sxs-lookup"><span data-stu-id="cfefc-131">Select your instance of ThousandEyes, then select the **Provisioning** tab.</span></span>

4. <span data-ttu-id="cfefc-132">Nastavte **režimu zřizování** k **automatické**.</span><span class="sxs-lookup"><span data-stu-id="cfefc-132">Set the **Provisioning Mode** to **Automatic**.</span></span>

    ![ThousandEyes zřizování](./media/active-directory-saas-thousandeyes-provisioning-tutorial/ThousandEyes1.png)

5. <span data-ttu-id="cfefc-134">V části **přihlašovací údaje správce** části, zadejte **tajný klíč tokenu** generované vaší ThousandEyes účtu (token můžete najít v rámci účtu ThousandEyes: **zabezpečení & Ověřování**).</span><span class="sxs-lookup"><span data-stu-id="cfefc-134">Under the **Admin Credentials** section, input the **Secret Token** generated by your ThousandEyes's account (you can find the token under your ThousandEyes account: **Security & Authentication**).</span></span> 

    ![ThousandEyes zřizování](./media/active-directory-saas-thousandeyes-provisioning-tutorial/ThousandEyes2.png)

6. <span data-ttu-id="cfefc-136">Na portálu Azure klikněte na tlačítko **Test připojení** zajistit Azure AD může připojit k aplikaci ThousandEyes.</span><span class="sxs-lookup"><span data-stu-id="cfefc-136">In the Azure portal, click **Test Connection** to ensure Azure AD can connect to your ThousandEyes app.</span></span> <span data-ttu-id="cfefc-137">Pokud se nepovede připojit, zajistěte, aby byl váš účet ThousandEyes oprávnění správce a opakujte krok 5.</span><span class="sxs-lookup"><span data-stu-id="cfefc-137">If the connection fails, ensure your ThousandEyes account has Admin permissions and try step 5 again.</span></span>

7. <span data-ttu-id="cfefc-138">Zadejte e-mailovou adresu uživatele nebo skupiny, který by měly dostávat oznámení zřizování Chyba v **e-mailové oznámení** pole a zaškrtněte políčko "Odesílat e-mailové oznámení, pokud dojde k chybě."</span><span class="sxs-lookup"><span data-stu-id="cfefc-138">Enter the email address of a person or group who should receive provisioning error notifications in the **Notification Email** field, and check the checkbox "Send an email notification when a failure occurs."</span></span>

8. <span data-ttu-id="cfefc-139">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="cfefc-139">Click **Save**.</span></span> 

9. <span data-ttu-id="cfefc-140">V části mapování vyberte **synchronizaci Azure Active Directory uživatelům ThousandEyes**.</span><span class="sxs-lookup"><span data-stu-id="cfefc-140">Under the Mappings section, select **Synchronize Azure Active Directory Users to ThousandEyes**.</span></span>

10. <span data-ttu-id="cfefc-141">V **mapování atributů** , projděte si uživatelské atributy, které jsou synchronizované z Azure AD ThousandEyes.</span><span class="sxs-lookup"><span data-stu-id="cfefc-141">In the **Attribute Mappings** section, review the user attributes that are synchronized from Azure AD to ThousandEyes.</span></span> <span data-ttu-id="cfefc-142">Atributy vybrán jako **párování** vlastnosti se používají tak, aby odpovídaly uživatelské účty v ThousandEyes pro operace aktualizace.</span><span class="sxs-lookup"><span data-stu-id="cfefc-142">The attributes selected as **Matching** properties are used to match the user accounts in ThousandEyes for update operations.</span></span> <span data-ttu-id="cfefc-143">Kliknutím na tlačítko Uložit potvrzení změny.</span><span class="sxs-lookup"><span data-stu-id="cfefc-143">Select the Save button to commit any changes.</span></span>

11. <span data-ttu-id="cfefc-144">Povolit zřizování služby pro ThousandEyes Azure AD, změňte **Stav zřizování** k **na** v **nastavení** části</span><span class="sxs-lookup"><span data-stu-id="cfefc-144">To enable the Azure AD provisioning service for ThousandEyes, change the **Provisioning Status** to **On** in the **Settings** section</span></span>

12. <span data-ttu-id="cfefc-145">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="cfefc-145">Click **Save**.</span></span> 

<span data-ttu-id="cfefc-146">Tato operace spustí počáteční synchronizaci všech uživatelů a skupiny přiřazené k ThousandEyes v části Uživatelé a skupiny.</span><span class="sxs-lookup"><span data-stu-id="cfefc-146">This operation starts the initial synchronization of any users and/or groups assigned to ThousandEyes in the Users and Groups section.</span></span> <span data-ttu-id="cfefc-147">Počáteční synchronizace trvá déle než následné synchronizace, ke kterým dochází přibližně každých 20 minut, dokud se službou provést.</span><span class="sxs-lookup"><span data-stu-id="cfefc-147">The initial sync takes longer to perform than subsequent syncs, which occur approximately every 20 minutes as long as the service is running.</span></span> <span data-ttu-id="cfefc-148">Můžete použít **podrobnosti synchronizace** části monitorovat průběh a odkazech zřízení sestavy aktivity, které popisují všechny akce prováděné při zřizování služby.</span><span class="sxs-lookup"><span data-stu-id="cfefc-148">You can use the **Synchronization Details** section to monitor progress and follow links to provisioning activity reports, which describe all actions performed by the provisioning service.</span></span>

<span data-ttu-id="cfefc-149">Další informace o tom, jak číst zřizování protokoly služby Azure AD najdete v tématu [zprávy o zřizování účtu automatické uživatele](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span><span class="sxs-lookup"><span data-stu-id="cfefc-149">For more information on how to read the Azure AD provisioning logs, see [Reporting on automatic user account provisioning](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span></span>


## <a name="additional-resources"></a><span data-ttu-id="cfefc-150">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="cfefc-150">Additional resources</span></span>

* [<span data-ttu-id="cfefc-151">Správa uživatelů zřizování účtu pro podnikové aplikace</span><span class="sxs-lookup"><span data-stu-id="cfefc-151">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-enterprise-apps-manage-provisioning.md)
* [<span data-ttu-id="cfefc-152">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="cfefc-152">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a><span data-ttu-id="cfefc-153">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cfefc-153">Next steps</span></span>

* [<span data-ttu-id="cfefc-154">Zjistěte, jak získat sestavy o zřizování aktivity a zkontrolujte protokoly</span><span class="sxs-lookup"><span data-stu-id="cfefc-154">Learn how to review logs and get reports on provisioning activity</span></span>](active-directory-saas-provisioning-reporting.md)
