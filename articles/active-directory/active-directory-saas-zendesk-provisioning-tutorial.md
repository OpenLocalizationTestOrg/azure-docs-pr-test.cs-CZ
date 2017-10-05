---
title: "Kurz: Konfigurace služby ZenDesk pro zřizování automatické uživatelů s Azure Active Directory | Microsoft Docs"
description: "Informace o konfiguraci Azure Active Directory a automaticky zřizovat a zrušte zřízení uživatelských účtů do této služby."
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
ms.openlocfilehash: 1a1414eefd20e6d7c025da08cfd5ae7c45daad33
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/29/2017
---
# <a name="tutorial-configuring-zendesk-for-automatic-user-provisioning"></a><span data-ttu-id="1008f-103">Kurz: Konfigurace služby ZenDesk pro zřizování automatické uživatelů</span><span class="sxs-lookup"><span data-stu-id="1008f-103">Tutorial: Configuring ZenDesk for Automatic User Provisioning</span></span>


<span data-ttu-id="1008f-104">Cílem tohoto kurzu je tak, aby zobrazovalo kroky, které je třeba provést v Zendesku a Azure AD a automaticky zřizovat a zrušte zřízení uživatelských účtů ze služby Azure AD do služby ZenDesk.</span><span class="sxs-lookup"><span data-stu-id="1008f-104">The objective of this tutorial is to show you the steps you need to perform in ZenDesk and Azure AD to automatically provision and de-provision user accounts from Azure AD to ZenDesk.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="1008f-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="1008f-105">Prerequisites</span></span>

<span data-ttu-id="1008f-106">Scénář uvedených v tomto kurzu se předpokládá, že už máte následující položky:</span><span class="sxs-lookup"><span data-stu-id="1008f-106">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

*   <span data-ttu-id="1008f-107">Klienta služby Azure Active directory</span><span class="sxs-lookup"><span data-stu-id="1008f-107">An Azure Active directory tenant</span></span>
*   <span data-ttu-id="1008f-108">Klienta služby ZenDesk s [plánu podnikového](https://www.zendesk.com/product/pricing/) nebo lépe povolena.</span><span class="sxs-lookup"><span data-stu-id="1008f-108">A ZenDesk tenant with the [Enterprise plan](https://www.zendesk.com/product/pricing/) or better enabled</span></span> 
*   <span data-ttu-id="1008f-109">Uživatelský účet v Zendesku se oprávnění správce</span><span class="sxs-lookup"><span data-stu-id="1008f-109">A user account in ZenDesk with Admin permissions</span></span> 

> [!NOTE]
> <span data-ttu-id="1008f-110">Azure AD zřizování integrace spoléhá na [ZenDesk REST API](https://developer.zendesk.com/rest_api/docs/core/introduction#the-api), který je k dispozici pro týmy ZenDesk základní plán nebo lepší.</span><span class="sxs-lookup"><span data-stu-id="1008f-110">The Azure AD provisioning integration relies on the [ZenDesk REST API](https://developer.zendesk.com/rest_api/docs/core/introduction#the-api), which is available to ZenDesk teams on the Essential plan or better.</span></span>

## <a name="assigning-users-to-zendesk"></a><span data-ttu-id="1008f-111">Přiřazování uživatelů do této služby</span><span class="sxs-lookup"><span data-stu-id="1008f-111">Assigning users to ZenDesk</span></span>

<span data-ttu-id="1008f-112">Azure Active Directory používá koncept označované jako "úlohy" k určení uživatelů, kteří obdrželi přístup k vybrané aplikace.</span><span class="sxs-lookup"><span data-stu-id="1008f-112">Azure Active Directory uses a concept called "assignments" to determine which users should receive access to selected apps.</span></span> <span data-ttu-id="1008f-113">V kontextu uživatele automatické zřizování účtu jsou synchronizovány pouze uživatelé a skupiny, které byly "přiřazeny" aplikace ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1008f-113">In the context of automatic user account provisioning, only the users and groups that have been "assigned" to an application in Azure AD are synchronized.</span></span> 

<span data-ttu-id="1008f-114">Před konfigurací a povolení zřizování služby, musíte rozhodnout, jaké uživatelů nebo skupin ve službě Azure AD představují uživatele, kteří potřebují přístup k vaší aplikaci ZenDesk.</span><span class="sxs-lookup"><span data-stu-id="1008f-114">Before configuring and enabling the provisioning service, you need to decide what users and/or groups in Azure AD represent the users who need access to your ZenDesk app.</span></span> <span data-ttu-id="1008f-115">Jakmile se rozhodli, můžete přiřadit tito uživatelé do vaší aplikace ZenDesk podle pokynů tady:</span><span class="sxs-lookup"><span data-stu-id="1008f-115">Once decided, you can assign these users to your ZenDesk app by following the instructions here:</span></span>

[<span data-ttu-id="1008f-116">Přiřazení uživatele nebo skupiny do aplikace enterprise</span><span class="sxs-lookup"><span data-stu-id="1008f-116">Assign a user or group to an enterprise app</span></span>](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-zendesk"></a><span data-ttu-id="1008f-117">Důležité tipy pro přiřazování uživatelů do této služby</span><span class="sxs-lookup"><span data-stu-id="1008f-117">Important tips for assigning users to ZenDesk</span></span>

*   <span data-ttu-id="1008f-118">Dále je doporučeno jednoho uživatele Azure AD se přiřadí ke Zendesku a otestovat konfiguraci zřizování.</span><span class="sxs-lookup"><span data-stu-id="1008f-118">It is recommended that a single Azure AD user is assigned to ZenDesk to test the provisioning configuration.</span></span> <span data-ttu-id="1008f-119">Další uživatele nebo skupiny může být přiřazen později.</span><span class="sxs-lookup"><span data-stu-id="1008f-119">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="1008f-120">Při přiřazení uživatele k této služby, je nutné vybrat buď **uživatele** role nebo jinou platnou specifické pro aplikaci rolí (Pokud je k dispozici) v dialogovém okně přiřazení.</span><span class="sxs-lookup"><span data-stu-id="1008f-120">When assigning a user to ZenDesk, you must select either the **User** role, or another valid application-specific role (if available) in the assignment dialog.</span></span> <span data-ttu-id="1008f-121">**Výchozího přístupu k** role nefunguje pro zřizování a tito uživatelé se přeskočí.</span><span class="sxs-lookup"><span data-stu-id="1008f-121">The **Default Access** role does not work for provisioning, and these users are skipped.</span></span>

> [!NOTE]
> <span data-ttu-id="1008f-122">Jako přidané funkce zřizování služba načte všechny vlastní role, které jsou definované v Zendesku a naimportuje je do Azure AD, kde mohou být vybrány v dialogovém okně vybrat roli.</span><span class="sxs-lookup"><span data-stu-id="1008f-122">As an added feature, the provisioning service reads any custom roles defined in Zendesk, and imports them into Azure AD where they can be selected in the Select Role dialog.</span></span> <span data-ttu-id="1008f-123">Tyto role se nebude zobrazovat na portálu Azure, po povolení zřizování služby a jeden synchronizační cyklus byla dokončena.</span><span class="sxs-lookup"><span data-stu-id="1008f-123">These roles will be visible in the Azure portal after the provisioning service is enabled and one synchronization cycle has completed.</span></span>

## <a name="configuring-user-provisioning-to-zendesk"></a><span data-ttu-id="1008f-124">Konfiguraci zřizování uživatelů do této služby</span><span class="sxs-lookup"><span data-stu-id="1008f-124">Configuring user provisioning to ZenDesk</span></span> 

<span data-ttu-id="1008f-125">Tato část vás provede připojení k této služby je uživatelský účet zřizování rozhraní API služby Azure AD a konfiguraci zřizování službu, kterou chcete vytvořit, aktualizovat a zakažte přiřazené uživatelské účty v Zendesku podle přiřazení uživatelů a skupin ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1008f-125">This section guides you through connecting your Azure AD to ZenDesk's user account provisioning API, and configuring the provisioning service to create, update, and disable assigned user accounts in ZenDesk based on user and group assignment in Azure AD.</span></span>

> [!TIP] 
> <span data-ttu-id="1008f-126">Také můžete povolit na základě SAML jednotné přihlašování pro ZenDesk, postupujte podle pokynů uvedených v [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="1008f-126">You may also choose to enabled SAML-based Single Sign-On for ZenDesk, following the instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="1008f-127">Jednotné přihlašování se dá nakonfigurovat nezávisle na automatické zřizování, i když tyto dvě funkce doplnění navzájem.</span><span class="sxs-lookup"><span data-stu-id="1008f-127">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>


### <a name="configure-automatic-user-account-provisioning-to-zendesk-in-azure-ad"></a><span data-ttu-id="1008f-128">Konfigurace automatického účet zřizování uživatelů do této služby ve službě Azure AD</span><span class="sxs-lookup"><span data-stu-id="1008f-128">Configure automatic user account provisioning to ZenDesk in Azure AD</span></span>


1. <span data-ttu-id="1008f-129">V [portál Azure](https://portal.azure.com), vyhledejte **Azure Active Directory > podnikové aplikace > všechny aplikace** části.</span><span class="sxs-lookup"><span data-stu-id="1008f-129">In the [Azure portal](https://portal.azure.com), browse to the **Azure Active Directory > Enterprise Apps > All applications**  section.</span></span>

2. <span data-ttu-id="1008f-130">Pokud jste již nakonfigurovali ZenDesk pro jednotné přihlašování, vyhledávání pro instanci služby ZenDesk pomocí pole hledání.</span><span class="sxs-lookup"><span data-stu-id="1008f-130">If you have already configured ZenDesk for single sign-on, search for your instance of ZenDesk using the search field.</span></span> <span data-ttu-id="1008f-131">Jinak vyberte možnost **přidat** a vyhledejte **ZenDesk** v galerii aplikací.</span><span class="sxs-lookup"><span data-stu-id="1008f-131">Otherwise, select **Add** and search for **ZenDesk** in the application gallery.</span></span> <span data-ttu-id="1008f-132">Vyberte ZenDesk ve výsledcích hledání a přidejte ji do seznamu aplikací.</span><span class="sxs-lookup"><span data-stu-id="1008f-132">Select ZenDesk from the search results, and add it to your list of applications.</span></span>

3. <span data-ttu-id="1008f-133">Vyberte instanci služby ZenDesk a pak vyberte **zřizování** kartě.</span><span class="sxs-lookup"><span data-stu-id="1008f-133">Select your instance of ZenDesk, then select the **Provisioning** tab.</span></span>

4. <span data-ttu-id="1008f-134">Nastavte **režimu zřizování** k **automatické**.</span><span class="sxs-lookup"><span data-stu-id="1008f-134">Set the **Provisioning Mode** to **Automatic**.</span></span>

    ![Zřizování této služby](./media/active-directory-saas-zendesk-provisioning-tutorial/ZenDesk1.png)

5. <span data-ttu-id="1008f-136">V části **přihlašovací údaje správce** části, zadejte **uživatelské jméno správce & tokenkey & domény** generované účtu vaší ZenDesk (token můžete najít v rámci vašeho účtu: **správce** > **rozhraní API** > **nastavení**).</span><span class="sxs-lookup"><span data-stu-id="1008f-136">Under the **Admin Credentials** section, input the **Admin Username&tokenkey&Domain** generated by your ZenDesk's account (you can find the token under your account: **Admin** > **API** > **Settings**).</span></span> 

    ![Zřizování této služby](./media/active-directory-saas-zendesk-provisioning-tutorial/ZenDesk2.png)

6. <span data-ttu-id="1008f-138">Na portálu Azure klikněte na tlačítko **Test připojení** zajistit Azure AD může připojit k aplikaci služby ZenDesk.</span><span class="sxs-lookup"><span data-stu-id="1008f-138">In the Azure portal, click **Test Connection** to ensure Azure AD can connect to your ZenDesk app.</span></span> <span data-ttu-id="1008f-139">Pokud se nepovede připojit, ujistěte se, že vašeho účtu ZenDesk má oprávnění správce a opakujte krok 5.</span><span class="sxs-lookup"><span data-stu-id="1008f-139">If the connection fails, ensure your ZenDesk account has Admin permissions and try step 5 again.</span></span>

7. <span data-ttu-id="1008f-140">Zadejte e-mailovou adresu uživatele nebo skupiny, který by měly dostávat oznámení zřizování Chyba v **e-mailové oznámení** pole a zaškrtněte políčko "Odesílat e-mailové oznámení, pokud dojde k chybě."</span><span class="sxs-lookup"><span data-stu-id="1008f-140">Enter the email address of a person or group who should receive provisioning error notifications in the **Notification Email** field, and check the checkbox "Send an email notification when a failure occurs."</span></span>

8. <span data-ttu-id="1008f-141">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="1008f-141">Click **Save**.</span></span> 

9. <span data-ttu-id="1008f-142">V části mapování vyberte **synchronizaci Azure Active Directory uživatelům ZenDesk**.</span><span class="sxs-lookup"><span data-stu-id="1008f-142">Under the Mappings section, select **Synchronize Azure Active Directory Users to ZenDesk**.</span></span>

10. <span data-ttu-id="1008f-143">V **mapování atributů** , projděte si uživatelské atributy, které jsou synchronizované z Azure AD ZenDesk.</span><span class="sxs-lookup"><span data-stu-id="1008f-143">In the **Attribute Mappings** section, review the user attributes that are synchronized from Azure AD to ZenDesk.</span></span> <span data-ttu-id="1008f-144">Atributy vybrán jako **párování** vlastnosti se používají tak, aby odpovídaly uživatelské účty v Zendesku pro operace aktualizace.</span><span class="sxs-lookup"><span data-stu-id="1008f-144">The attributes selected as **Matching** properties are used to match the user accounts in ZenDesk for update operations.</span></span> <span data-ttu-id="1008f-145">Kliknutím na tlačítko Uložit potvrzení změny.</span><span class="sxs-lookup"><span data-stu-id="1008f-145">Select the Save button to commit any changes.</span></span>

11. <span data-ttu-id="1008f-146">Povolit Azure AD zřizování služby pro služby ZenDesk, změňte **Stav zřizování** k **na** v **nastavení** části</span><span class="sxs-lookup"><span data-stu-id="1008f-146">To enable the Azure AD provisioning service for ZenDesk, change the **Provisioning Status** to **On** in the **Settings** section</span></span>

12. <span data-ttu-id="1008f-147">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="1008f-147">Click **Save**.</span></span> 

<span data-ttu-id="1008f-148">Tato operace spustí počáteční synchronizaci všech uživatelů a skupiny přiřazené k této služby v části Uživatelé a skupiny.</span><span class="sxs-lookup"><span data-stu-id="1008f-148">This operation starts the initial synchronization of any users and/or groups assigned to ZenDesk in the Users and Groups section.</span></span> <span data-ttu-id="1008f-149">Počáteční synchronizace trvá déle než následné synchronizace, ke kterým dochází přibližně každých 20 minut, dokud se službou provést.</span><span class="sxs-lookup"><span data-stu-id="1008f-149">The initial sync takes longer to perform than subsequent syncs, which occur approximately every 20 minutes as long as the service is running.</span></span> <span data-ttu-id="1008f-150">Můžete použít **podrobnosti synchronizace** části monitorovat průběh a odkazech zřízení sestavy aktivity, které popisují všechny akce prováděné při zřizování služby.</span><span class="sxs-lookup"><span data-stu-id="1008f-150">You can use the **Synchronization Details** section to monitor progress and follow links to provisioning activity reports, which describe all actions performed by the provisioning service.</span></span>

<span data-ttu-id="1008f-151">Další informace o tom, jak číst zřizování protokoly služby Azure AD najdete v tématu [zprávy o zřizování účtu automatické uživatele](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span><span class="sxs-lookup"><span data-stu-id="1008f-151">For more information on how to read the Azure AD provisioning logs, see [Reporting on automatic user account provisioning](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span></span>


## <a name="additional-resources"></a><span data-ttu-id="1008f-152">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="1008f-152">Additional resources</span></span>

* [<span data-ttu-id="1008f-153">Správa uživatelů zřizování účtu pro podnikové aplikace</span><span class="sxs-lookup"><span data-stu-id="1008f-153">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-enterprise-apps-manage-provisioning.md)
* [<span data-ttu-id="1008f-154">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="1008f-154">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a><span data-ttu-id="1008f-155">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1008f-155">Next steps</span></span>

* [<span data-ttu-id="1008f-156">Zjistěte, jak získat sestavy o zřizování aktivity a zkontrolujte protokoly</span><span class="sxs-lookup"><span data-stu-id="1008f-156">Learn how to review logs and get reports on provisioning activity</span></span>](active-directory-saas-provisioning-reporting.md)
