---
title: "Kurz: Konfigurace LinkedIn zvýšení oprávnění pro uživatele automatické zřizování s Azure Active Directory | Microsoft Docs"
description: "Informace o konfiguraci Azure Active Directory a automaticky zřizovat a zrušte zřízení uživatelských účtů do LinkedIn zvýšení oprávnění."
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
ms.date: 04/15/2017
ms.author: asmalser-msft
ms.openlocfilehash: 526666301aad1e5284c621024649d9cd52c92d18
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-configuring-linkedin-elevate-for-automatic-user-provisioning"></a><span data-ttu-id="c3f2f-103">Kurz: Konfigurace LinkedIn zvýšení oprávnění pro uživatele automatické zřizování</span><span class="sxs-lookup"><span data-stu-id="c3f2f-103">Tutorial: Configuring LinkedIn Elevate for Automatic User Provisioning</span></span>


<span data-ttu-id="c3f2f-104">Cílem tohoto kurzu je tak, aby zobrazovalo kroky, které je třeba provést v LinkedIn zvýšení oprávnění a Azure AD a automaticky zřizovat a zrušte zřízení uživatelských účtů ze služby Azure AD LinkedIn zvýšení oprávnění.</span><span class="sxs-lookup"><span data-stu-id="c3f2f-104">The objective of this tutorial is to show you the steps you need to perform in LinkedIn Elevate and Azure AD to automatically provision and de-provision user accounts from Azure AD to LinkedIn Elevate.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="c3f2f-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c3f2f-105">Prerequisites</span></span>

<span data-ttu-id="c3f2f-106">Scénář uvedených v tomto kurzu se předpokládá, že už máte následující položky:</span><span class="sxs-lookup"><span data-stu-id="c3f2f-106">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

*   <span data-ttu-id="c3f2f-107">Klient služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c3f2f-107">An Azure Active Directory tenant</span></span>
*   <span data-ttu-id="c3f2f-108">Klienta LinkedIn zvýšení oprávnění</span><span class="sxs-lookup"><span data-stu-id="c3f2f-108">A LinkedIn Elevate tenant</span></span> 
*   <span data-ttu-id="c3f2f-109">Účet správce v zvýšení oprávnění LinkedIn přístup k centru účtů LinkedIn</span><span class="sxs-lookup"><span data-stu-id="c3f2f-109">An administrator account in LinkedIn Elevate with access to the LinkedIn Account Center</span></span>

> [!NOTE]
> <span data-ttu-id="c3f2f-110">Azure Active Directory se integruje s LinkedIn zvýšení oprávnění pomocí [SCIM](http://www.simplecloud.info/) protokolu.</span><span class="sxs-lookup"><span data-stu-id="c3f2f-110">Azure Active Directory integrates with LinkedIn Elevate using the [SCIM](http://www.simplecloud.info/) protocol.</span></span>

## <a name="assigning-users-to-linkedin-elevate"></a><span data-ttu-id="c3f2f-111">Přiřazení uživatelů ke zvýšení oprávnění LinkedIn</span><span class="sxs-lookup"><span data-stu-id="c3f2f-111">Assigning users to LinkedIn Elevate</span></span>

<span data-ttu-id="c3f2f-112">Azure Active Directory používá koncept označované jako "úlohy" k určení uživatelů, kteří obdrželi přístup k vybrané aplikace.</span><span class="sxs-lookup"><span data-stu-id="c3f2f-112">Azure Active Directory uses a concept called "assignments" to determine which users should receive access to selected apps.</span></span> <span data-ttu-id="c3f2f-113">V kontextu uživatele automatické zřizování účtu se budou synchronizovat pouze uživatelé a skupiny, které byly "přiřazeny" aplikace ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c3f2f-113">In the context of automatic user account provisioning, only the users and groups that have been "assigned" to an application in Azure AD will be synchronized.</span></span> 

<span data-ttu-id="c3f2f-114">Před konfigurací a povolení zřizování služby, musíte se rozhodnout, jaké uživatelů nebo skupin ve službě Azure AD představují uživatele, kteří potřebují přístup ke zvýšení oprávnění LinkedIn.</span><span class="sxs-lookup"><span data-stu-id="c3f2f-114">Before configuring and enabling the provisioning service, you will need to decide what users and/or groups in Azure AD represent the users who need access to LinkedIn Elevate.</span></span> <span data-ttu-id="c3f2f-115">Jakmile se rozhodli, můžete přiřadit těmto uživatelům LinkedIn zvýšení oprávnění podle pokynů tady:</span><span class="sxs-lookup"><span data-stu-id="c3f2f-115">Once decided, you can assign these users to LinkedIn Elevate by following the instructions here:</span></span>

[<span data-ttu-id="c3f2f-116">Přiřazení uživatele nebo skupiny do aplikace enterprise</span><span class="sxs-lookup"><span data-stu-id="c3f2f-116">Assign a user or group to an enterprise app</span></span>](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-linkedin-elevate"></a><span data-ttu-id="c3f2f-117">Důležité tipy pro přiřazení uživatelů ke zvýšení oprávnění LinkedIn</span><span class="sxs-lookup"><span data-stu-id="c3f2f-117">Important tips for assigning users to LinkedIn Elevate</span></span>

*   <span data-ttu-id="c3f2f-118">Dále je doporučeno jednoho uživatele Azure AD pro testování zřizování konfigurace přidělí LinkedIn zvýšení oprávnění.</span><span class="sxs-lookup"><span data-stu-id="c3f2f-118">It is recommended that a single Azure AD user be assigned to LinkedIn Elevate to test the provisioning configuration.</span></span> <span data-ttu-id="c3f2f-119">Další uživatele nebo skupiny může být přiřazen později.</span><span class="sxs-lookup"><span data-stu-id="c3f2f-119">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="c3f2f-120">Při přiřazení uživatele k LinkedIn zvýšení oprávnění, je nutné vybrat **uživatele** role v dialogovém okně přiřazení.</span><span class="sxs-lookup"><span data-stu-id="c3f2f-120">When assigning a user to LinkedIn Elevate, you must select the **User** role in the assignment dialog.</span></span> <span data-ttu-id="c3f2f-121">Roli "Výchozí přístup" nefunguje pro zřizování.</span><span class="sxs-lookup"><span data-stu-id="c3f2f-121">The "Default Access" role does not work for provisioning.</span></span>


## <a name="configuring-user-provisioning-to-linkedin-elevate"></a><span data-ttu-id="c3f2f-122">Konfiguraci zřizování uživatelů ke zvýšení oprávnění LinkedIn</span><span class="sxs-lookup"><span data-stu-id="c3f2f-122">Configuring user provisioning to LinkedIn Elevate</span></span>

<span data-ttu-id="c3f2f-123">Tato část vás provede připojení k LinkedIn zvýšení SCIM uživatelský účet zřizování rozhraní API služby Azure AD a konfiguraci zřizování služby vytvářet, aktualizovat a zakázat přiřazení uživatelských účtů ve LinkedIn zvýšení oprávnění na základě uživatele a přiřazení skupiny ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c3f2f-123">This section guides you through connecting your Azure AD to LinkedIn Elevate's SCIM user account provisioning API, and configuring the provisioning service to create, update and disable assigned user accounts in LinkedIn Elevate based on user and group assignment in Azure AD.</span></span>

<span data-ttu-id="c3f2f-124">**Tip:** také můžete povolit na základě SAML jednotné přihlašování pro zvýšení oprávnění LinkedIn, postupujte podle pokynů uvedených v [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="c3f2f-124">**Tip:** You may also choose to enabled SAML-based Single Sign-On for LinkedIn Elevate, following the instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="c3f2f-125">Jednotné přihlašování se dá nakonfigurovat nezávisle na automatické zřizování, i když tyto dvě funkce vzájemně doplňují.</span><span class="sxs-lookup"><span data-stu-id="c3f2f-125">Single sign-on can be configured independently of automatic provisioning, though these two features complement each other.</span></span>


### <a name="to-configure-automatic-user-account-provisioning-to-linkedin-elevate-in-azure-ad"></a><span data-ttu-id="c3f2f-126">Konfigurace automatického účet zřizování uživatelů ke zvýšení oprávnění LinkedIn ve službě Azure AD:</span><span class="sxs-lookup"><span data-stu-id="c3f2f-126">To configure automatic user account provisioning to LinkedIn Elevate in Azure AD:</span></span>


<span data-ttu-id="c3f2f-127">Prvním krokem je načíst LinkedIn přístupový token.</span><span class="sxs-lookup"><span data-stu-id="c3f2f-127">The first step is to retrieve your LinkedIn access token.</span></span> <span data-ttu-id="c3f2f-128">Pokud jste správce podnikové sítě, můžete zřídit samoobslužné přístupový token.</span><span class="sxs-lookup"><span data-stu-id="c3f2f-128">If you are an Enterprise administrator, you can self-provision an access token.</span></span> <span data-ttu-id="c3f2f-129">V centru váš účet, přejděte na **nastavení &gt; globální nastavení** a otevřete **SCIM instalace** panelu.</span><span class="sxs-lookup"><span data-stu-id="c3f2f-129">In your account center, go to **Settings &gt; Global Settings** and open the **SCIM Setup** panel.</span></span>

> [!NOTE]
> <span data-ttu-id="c3f2f-130">Pokud se připojujete centra účtů přímo a nikoli v rámci odkaz, můžete dosáhnout pomocí následujících kroků.</span><span class="sxs-lookup"><span data-stu-id="c3f2f-130">If you are accessing the account center directly rather than through a link, you can reach it using the following steps.</span></span>

1)  <span data-ttu-id="c3f2f-131">Přihlaste se k účtu Center.</span><span class="sxs-lookup"><span data-stu-id="c3f2f-131">Sign in to Account Center.</span></span>

2)  <span data-ttu-id="c3f2f-132">Vyberte **správce &gt; nastavení správce** .</span><span class="sxs-lookup"><span data-stu-id="c3f2f-132">Select **Admin &gt; Admin Settings** .</span></span>

3)  <span data-ttu-id="c3f2f-133">Klikněte na tlačítko **Advanced integrace** na levém bočním panelu.</span><span class="sxs-lookup"><span data-stu-id="c3f2f-133">Click **Advanced Integrations** on the left sidebar.</span></span> <span data-ttu-id="c3f2f-134">Budete přesměrováni do centra účtů.</span><span class="sxs-lookup"><span data-stu-id="c3f2f-134">You are directed to the account center.</span></span>

4)  <span data-ttu-id="c3f2f-135">Klikněte na tlačítko **+ přidat novou konfiguraci SCIM** a postupujte podle pokynů podle hodnot v každé pole.</span><span class="sxs-lookup"><span data-stu-id="c3f2f-135">Click **+ Add new SCIM configuration** and follow the procedure by filling in each field.</span></span>

> <span data-ttu-id="c3f2f-136">Když autoassign licencí není povolen, znamená to, že je synchronizovat pouze data uživatele.</span><span class="sxs-lookup"><span data-stu-id="c3f2f-136">When auto­assign licenses is not enabled, it means that only user data is synced.</span></span>

![LinkedIn zvýšení zřizování](./media/active-directory-saas-linkedin-elevate-provisioning-tutorial/linkedin_elevate1.PNG)

> <span data-ttu-id="c3f2f-138">Pokud je povoleno autolicense přiřazení, budete muset poznamenejte si instanci aplikace a typ licence.</span><span class="sxs-lookup"><span data-stu-id="c3f2f-138">When auto­license assignment is enabled, you need to note the application instance and license type.</span></span> <span data-ttu-id="c3f2f-139">Licence jsou přiřazeny na první přijde, nejprve sloužit základ, dokud všechny licence se provádějí.</span><span class="sxs-lookup"><span data-stu-id="c3f2f-139">Licenses are assigned on a first come, first serve basis until all the licenses are taken.</span></span>

![LinkedIn zvýšení zřizování](./media/active-directory-saas-linkedin-elevate-provisioning-tutorial/linkedin_elevate2.PNG)

5)  <span data-ttu-id="c3f2f-141">Klikněte na tlačítko **vygenerovat token**.</span><span class="sxs-lookup"><span data-stu-id="c3f2f-141">Click **Generate token**.</span></span> <span data-ttu-id="c3f2f-142">Měli byste vidět zobrazení tokenu přístupu v části **přístupový token** pole.</span><span class="sxs-lookup"><span data-stu-id="c3f2f-142">You should see your access token display under the **Access token** field.</span></span>

6)  <span data-ttu-id="c3f2f-143">Uložte přístupový token do schránky nebo počítač před opuštěním stránky.</span><span class="sxs-lookup"><span data-stu-id="c3f2f-143">Save your access token to your clipboard or computer before leaving the page.</span></span>

7) <span data-ttu-id="c3f2f-144">V dalším kroku se přihlaste k [portál Azure](https://portal.azure.com)a přejděte do **Azure Active Directory > podnikové aplikace > všechny aplikace** části.</span><span class="sxs-lookup"><span data-stu-id="c3f2f-144">Next, sign in to the [Azure portal](https://portal.azure.com), and browse to the **Azure Active Directory > Enterprise Apps > All applications**  section.</span></span>

8) <span data-ttu-id="c3f2f-145">Pokud jste již nakonfigurovali LinkedIn zvýšení oprávnění pro jednotné přihlašování, vyhledejte instanci LinkedIn zvýšení oprávnění pomocí pole hledání.</span><span class="sxs-lookup"><span data-stu-id="c3f2f-145">If you have already configured LinkedIn Elevate for single sign-on, search for your instance of LinkedIn Elevate using the search field.</span></span> <span data-ttu-id="c3f2f-146">Jinak vyberte možnost **přidat** a vyhledejte **zvýšení oprávnění LinkedIn** v galerii aplikací.</span><span class="sxs-lookup"><span data-stu-id="c3f2f-146">Otherwise, select **Add** and search for **LinkedIn Elevate** in the application gallery.</span></span> <span data-ttu-id="c3f2f-147">Ve výsledcích hledání vyberte LinkedIn zvýšení oprávnění a přidat do seznamu aplikací.</span><span class="sxs-lookup"><span data-stu-id="c3f2f-147">Select LinkedIn Elevate from the search results, and add it to your list of applications.</span></span>

9)  <span data-ttu-id="c3f2f-148">Vyberte instanci LinkedIn zvýšení oprávnění a potom vyberte **zřizování** kartě.</span><span class="sxs-lookup"><span data-stu-id="c3f2f-148">Select your instance of LinkedIn Elevate, then select the **Provisioning** tab.</span></span>

10) <span data-ttu-id="c3f2f-149">Nastavte **režimu zřizování** k **automatické**.</span><span class="sxs-lookup"><span data-stu-id="c3f2f-149">Set the **Provisioning Mode** to **Automatic**.</span></span>

![LinkedIn zvýšení zřizování](./media/active-directory-saas-linkedin-elevate-provisioning-tutorial/linkedin_elevate3.PNG)

11)  <span data-ttu-id="c3f2f-151">Vyplňte následující pole v části **přihlašovací údaje správce** :</span><span class="sxs-lookup"><span data-stu-id="c3f2f-151">Fill in the following fields under **Admin Credentials** :</span></span>

* <span data-ttu-id="c3f2f-152">V **URL klienta** zadejte https://api.linkedin.com.</span><span class="sxs-lookup"><span data-stu-id="c3f2f-152">In the **Tenant URL** field, enter https://api.linkedin.com.</span></span>

* <span data-ttu-id="c3f2f-153">V **tajný klíč tokenu** pole, zadejte přístupový token, který jste vygenerovali v kroku 1 a klikněte na **Test připojení** .</span><span class="sxs-lookup"><span data-stu-id="c3f2f-153">In the **Secret Token** field, enter the access token you generated in step 1 and click **Test Connection** .</span></span>

* <span data-ttu-id="c3f2f-154">Měli byste vidět úspěšné oznámení na straně ÚETUChcete portálu.</span><span class="sxs-lookup"><span data-stu-id="c3f2f-154">You should see a success notification on the upper­right side of   your portal.</span></span>

12) <span data-ttu-id="c3f2f-155">Zadejte e-mailovou adresu uživatele nebo skupiny, který by měly dostávat oznámení zřizování Chyba v **e-mailové oznámení** pole a zaškrtněte políčko níže.</span><span class="sxs-lookup"><span data-stu-id="c3f2f-155">Enter the email address of a person or group who should receive provisioning error notifications in the **Notification Email** field, and check the checkbox below.</span></span>

13) <span data-ttu-id="c3f2f-156">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="c3f2f-156">Click **Save**.</span></span> 

14) <span data-ttu-id="c3f2f-157">V **mapování atributů** , projděte si atributy uživatelů a skupin, které se mají synchronizovat ze služby Azure AD LinkedIn zvýšení oprávnění.</span><span class="sxs-lookup"><span data-stu-id="c3f2f-157">In the **Attribute Mappings** section, review the user and group attributes that will be synchronized from Azure AD to LinkedIn Elevate.</span></span> <span data-ttu-id="c3f2f-158">Všimněte si, že atributy vybrán jako **párování** vlastnosti se použije tak, aby odpovídaly uživatelské účty a skupiny v LinkedIn zvýšení oprávnění pro operace aktualizace.</span><span class="sxs-lookup"><span data-stu-id="c3f2f-158">Note that the attributes selected as **Matching** properties will be used to match the user accounts and groups in LinkedIn Elevate for update operations.</span></span> <span data-ttu-id="c3f2f-159">Kliknutím na tlačítko Uložit potvrzení změny.</span><span class="sxs-lookup"><span data-stu-id="c3f2f-159">Select the Save button to commit any changes.</span></span>

![LinkedIn zvýšení zřizování](./media/active-directory-saas-linkedin-elevate-provisioning-tutorial/linkedin_elevate4.PNG)

15) <span data-ttu-id="c3f2f-161">Povolit zřizování služby pro zvýšení oprávnění LinkedIn Azure AD, změňte **Stav zřizování** k **na** v **nastavení** části</span><span class="sxs-lookup"><span data-stu-id="c3f2f-161">To enable the Azure AD provisioning service for LinkedIn Elevate, change the **Provisioning Status** to **On** in the **Settings** section</span></span>

16) <span data-ttu-id="c3f2f-162">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="c3f2f-162">Click **Save**.</span></span> 

<span data-ttu-id="c3f2f-163">Tato akce spustí počáteční synchronizaci všech uživatelů a skupiny přiřazené LinkedIn zvýšení oprávnění v části Uživatelé a skupiny.</span><span class="sxs-lookup"><span data-stu-id="c3f2f-163">This will start the initial synchronization of any users and/or groups assigned to LinkedIn Elevate in the Users and Groups section.</span></span> <span data-ttu-id="c3f2f-164">Všimněte si, že počáteční synchronizace bude trvat déle než následné synchronizace, ke kterým dochází přibližně každých 20 minut, dokud je služba spuštěná.</span><span class="sxs-lookup"><span data-stu-id="c3f2f-164">Note that the initial sync will take longer to perform than subsequent syncs, which occur approximately every 20 minutes as long as the service is running.</span></span> <span data-ttu-id="c3f2f-165">Můžete použít **podrobnosti synchronizace** části monitorovat průběh a odkazech zřízení sestavy aktivity, které popisují všechny akce prováděné při zřizování služby ve vaší aplikaci LinkedIn zvýšení oprávnění.</span><span class="sxs-lookup"><span data-stu-id="c3f2f-165">You can use the **Synchronization Details** section to monitor progress and follow links to provisioning activity reports, which describe all actions performed by the provisioning service on your LinkedIn Elevate app.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="c3f2f-166">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="c3f2f-166">Additional Resources</span></span>

* [<span data-ttu-id="c3f2f-167">Správa uživatelů zřizování účtu pro podnikové aplikace</span><span class="sxs-lookup"><span data-stu-id="c3f2f-167">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-enterprise-apps-manage-provisioning.md)
* [<span data-ttu-id="c3f2f-168">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="c3f2f-168">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
