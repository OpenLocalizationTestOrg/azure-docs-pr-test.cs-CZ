---
title: "Kurz: Konfigurace LinkedIn Learning pro zřizování automatické uživatelů s Azure Active Directory | Microsoft Docs"
description: "Informace o konfiguraci Azure Active Directory a automaticky zřizovat a zrušte zřízení uživatelských účtů do LinkedIn Learning."
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
ms.openlocfilehash: 5eb2b1594eedb2a135d7b8cd501a33d8264e136b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-configuring-linkedin-learning-for-automatic-user-provisioning"></a><span data-ttu-id="7aa06-103">Kurz: Konfigurace LinkedIn Learning pro zřizování automatické uživatelů</span><span class="sxs-lookup"><span data-stu-id="7aa06-103">Tutorial: Configuring LinkedIn Learning for Automatic User Provisioning</span></span>


<span data-ttu-id="7aa06-104">Cílem tohoto kurzu je tak, aby zobrazovalo kroky, které je třeba provést v LinkedIn učení a Azure AD a automaticky zřizovat a zrušte zřízení uživatelských účtů ze služby Azure AD do LinkedIn Learning.</span><span class="sxs-lookup"><span data-stu-id="7aa06-104">The objective of this tutorial is to show you the steps you need to perform in LinkedIn Learning and Azure AD to automatically provision and de-provision user accounts from Azure AD to LinkedIn Learning.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="7aa06-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="7aa06-105">Prerequisites</span></span>

<span data-ttu-id="7aa06-106">Scénář uvedených v tomto kurzu se předpokládá, že už máte následující položky:</span><span class="sxs-lookup"><span data-stu-id="7aa06-106">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

*   <span data-ttu-id="7aa06-107">Klient služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7aa06-107">An Azure Active Directory tenant</span></span>
*   <span data-ttu-id="7aa06-108">Klienta LinkedIn učení</span><span class="sxs-lookup"><span data-stu-id="7aa06-108">A LinkedIn Learning tenant</span></span> 
*   <span data-ttu-id="7aa06-109">Účet správce v LinkedIn Learning přístup k centru účtů LinkedIn</span><span class="sxs-lookup"><span data-stu-id="7aa06-109">An administrator account in LinkedIn Learning with access to the LinkedIn Account Center</span></span>

> [!NOTE]
> <span data-ttu-id="7aa06-110">Azure Active Directory se integruje s použitím LinkedIn Learning [SCIM](http://www.simplecloud.info/) protokolu.</span><span class="sxs-lookup"><span data-stu-id="7aa06-110">Azure Active Directory integrates with LinkedIn Learning using the [SCIM](http://www.simplecloud.info/) protocol.</span></span>

## <a name="assigning-users-to-linkedin-learning"></a><span data-ttu-id="7aa06-111">Přiřazení uživatelů k LinkedIn Learning</span><span class="sxs-lookup"><span data-stu-id="7aa06-111">Assigning users to LinkedIn Learning</span></span>

<span data-ttu-id="7aa06-112">Azure Active Directory používá koncept označované jako "úlohy" k určení uživatelů, kteří obdrželi přístup k vybrané aplikace.</span><span class="sxs-lookup"><span data-stu-id="7aa06-112">Azure Active Directory uses a concept called "assignments" to determine which users should receive access to selected apps.</span></span> <span data-ttu-id="7aa06-113">V kontextu uživatele automatické zřizování účtu se budou synchronizovat pouze uživatelé a skupiny, které byly "přiřazeny" aplikace ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7aa06-113">In the context of automatic user account provisioning, only the users and groups that have been "assigned" to an application in Azure AD will be synchronized.</span></span> 

<span data-ttu-id="7aa06-114">Před konfigurací a povolení zřizování služby, musíte se rozhodnout, jaké uživatelů nebo skupin ve službě Azure AD představují uživatele, kteří potřebují přístup k LinkedIn Learning.</span><span class="sxs-lookup"><span data-stu-id="7aa06-114">Before configuring and enabling the provisioning service, you will need to decide what users and/or groups in Azure AD represent the users who need access to LinkedIn Learning.</span></span> <span data-ttu-id="7aa06-115">Jakmile se rozhodli, můžete přiřadit těmto uživatelům LinkedIn Learning podle pokynů tady:</span><span class="sxs-lookup"><span data-stu-id="7aa06-115">Once decided, you can assign these users to LinkedIn Learning by following the instructions here:</span></span>

[<span data-ttu-id="7aa06-116">Přiřazení uživatele nebo skupiny do aplikace enterprise</span><span class="sxs-lookup"><span data-stu-id="7aa06-116">Assign a user or group to an enterprise app</span></span>](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-linkedin-learning"></a><span data-ttu-id="7aa06-117">Důležité tipy pro přiřazování uživatelů do LinkedIn Learning</span><span class="sxs-lookup"><span data-stu-id="7aa06-117">Important tips for assigning users to LinkedIn Learning</span></span>

*   <span data-ttu-id="7aa06-118">Dále je doporučeno jednoho uživatele Azure AD pro LinkedIn Learning přidělí otestovat konfiguraci zřizování.</span><span class="sxs-lookup"><span data-stu-id="7aa06-118">It is recommended that a single Azure AD user be assigned to LinkedIn Learning to test the provisioning configuration.</span></span> <span data-ttu-id="7aa06-119">Další uživatele nebo skupiny může být přiřazen později.</span><span class="sxs-lookup"><span data-stu-id="7aa06-119">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="7aa06-120">Při přiřazení uživatele k LinkedIn Learning, je nutné vybrat **uživatele** role v dialogovém okně přiřazení.</span><span class="sxs-lookup"><span data-stu-id="7aa06-120">When assigning a user to LinkedIn Learning, you must select the **User** role in the assignment dialog.</span></span> <span data-ttu-id="7aa06-121">Roli "Výchozí přístup" nefunguje pro zřizování.</span><span class="sxs-lookup"><span data-stu-id="7aa06-121">The "Default Access" role does not work for provisioning.</span></span>


## <a name="configuring-user-provisioning-to-linkedin-learning"></a><span data-ttu-id="7aa06-122">Konfiguraci zřizování uživatelů pro učení LinkedIn</span><span class="sxs-lookup"><span data-stu-id="7aa06-122">Configuring user provisioning to LinkedIn Learning</span></span>

<span data-ttu-id="7aa06-123">Tato část vás provede připojení k LinkedIn Learning SCIM uživatelský účet zřizování rozhraní API služby Azure AD a konfiguraci zřizování služby vytvářet, aktualizovat a zakázat přiřadit uživatelské účty v LinkedIn Learning na základě uživatele a přiřazení skupiny ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7aa06-123">This section guides you through connecting your Azure AD to LinkedIn Learning's SCIM user account provisioning API, and configuring the provisioning service to create, update and disable assigned user accounts in LinkedIn Learning based on user and group assignment in Azure AD.</span></span>

> [!TIP]
> <span data-ttu-id="7aa06-124">Také můžete povolit na základě SAML jednotné přihlašování pro učení LinkedIn, postupujte podle pokynů uvedených v [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="7aa06-124">You may also choose to enabled SAML-based Single Sign-On for LinkedIn Learning, following the instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="7aa06-125">Jednotné přihlašování se dá nakonfigurovat nezávisle na automatické zřizování, i když tyto dvě funkce vzájemně doplňují.</span><span class="sxs-lookup"><span data-stu-id="7aa06-125">Single sign-on can be configured independently of automatic provisioning, though these two features complement each other.</span></span>


### <a name="to-configure-automatic-user-account-provisioning-to-linkedin-learning-in-azure-ad"></a><span data-ttu-id="7aa06-126">Konfigurace automatického účet zřizování uživatelů pro LinkedIn Learning v Azure AD:</span><span class="sxs-lookup"><span data-stu-id="7aa06-126">To configure automatic user account provisioning to LinkedIn Learning in Azure AD:</span></span>


<span data-ttu-id="7aa06-127">Prvním krokem je načíst LinkedIn přístupový token.</span><span class="sxs-lookup"><span data-stu-id="7aa06-127">The first step is to retrieve your LinkedIn access token.</span></span> <span data-ttu-id="7aa06-128">Pokud jste správce podnikové sítě, můžete zřídit samoobslužné přístupový token.</span><span class="sxs-lookup"><span data-stu-id="7aa06-128">If you are an Enterprise administrator, you can self-provision an access token.</span></span> <span data-ttu-id="7aa06-129">V centru váš účet, přejděte na **nastavení &gt; globální nastavení** a otevřete **SCIM instalace** panelu.</span><span class="sxs-lookup"><span data-stu-id="7aa06-129">In your account center, go to **Settings &gt; Global Settings** and open the **SCIM Setup** panel.</span></span>

> [!NOTE]
> <span data-ttu-id="7aa06-130">Pokud se připojujete centra účtů přímo a nikoli v rámci odkaz, můžete dosáhnout pomocí následujících kroků.</span><span class="sxs-lookup"><span data-stu-id="7aa06-130">If you are accessing the account center directly rather than through a link, you can reach it using the following steps.</span></span>

1)  <span data-ttu-id="7aa06-131">Přihlaste se k účtu Center.</span><span class="sxs-lookup"><span data-stu-id="7aa06-131">Sign in to Account Center.</span></span>

2)  <span data-ttu-id="7aa06-132">Vyberte **správce &gt; nastavení správce** .</span><span class="sxs-lookup"><span data-stu-id="7aa06-132">Select **Admin &gt; Admin Settings** .</span></span>

3)  <span data-ttu-id="7aa06-133">Klikněte na tlačítko **Advanced integrace** na levém bočním panelu.</span><span class="sxs-lookup"><span data-stu-id="7aa06-133">Click **Advanced Integrations** on the left sidebar.</span></span> <span data-ttu-id="7aa06-134">Budete přesměrováni do centra účtů.</span><span class="sxs-lookup"><span data-stu-id="7aa06-134">You are directed to the account center.</span></span>

4)  <span data-ttu-id="7aa06-135">Klikněte na tlačítko **+ přidat novou konfiguraci SCIM** a postupujte podle pokynů podle hodnot v každé pole.</span><span class="sxs-lookup"><span data-stu-id="7aa06-135">Click **+ Add new SCIM configuration** and follow the procedure by filling in each field.</span></span>

> <span data-ttu-id="7aa06-136">Když autoassign licencí není povolen, znamená to, že je synchronizovat pouze data uživatele.</span><span class="sxs-lookup"><span data-stu-id="7aa06-136">When auto­assign licenses is not enabled, it means that only user data is synced.</span></span>

![LinkedIn učení zřizování](./media/active-directory-saas-linkedinlearning-provisioning-tutorial/linkedin_1.PNG)

> <span data-ttu-id="7aa06-138">Pokud je povoleno autolicense přiřazení, budete muset poznamenejte si instanci aplikace a typ licence.</span><span class="sxs-lookup"><span data-stu-id="7aa06-138">When auto­license assignment is enabled, you need to note the application instance and license type.</span></span> <span data-ttu-id="7aa06-139">Licence jsou přiřazeny na první přijde, nejprve sloužit základ, dokud všechny licence se provádějí.</span><span class="sxs-lookup"><span data-stu-id="7aa06-139">Licenses are assigned on a first come, first serve basis until all the licenses are taken.</span></span>

![LinkedIn učení zřizování](./media/active-directory-saas-linkedinlearning-provisioning-tutorial/linkedin_2.PNG)

5)  <span data-ttu-id="7aa06-141">Klikněte na tlačítko **vygenerovat token**.</span><span class="sxs-lookup"><span data-stu-id="7aa06-141">Click **Generate token**.</span></span> <span data-ttu-id="7aa06-142">Měli byste vidět zobrazení tokenu přístupu v části **přístupový token** pole.</span><span class="sxs-lookup"><span data-stu-id="7aa06-142">You should see your access token display under the **Access token** field.</span></span>

6)  <span data-ttu-id="7aa06-143">Uložte přístupový token do schránky nebo počítač před opuštěním stránky.</span><span class="sxs-lookup"><span data-stu-id="7aa06-143">Save your access token to your clipboard or computer before leaving the page.</span></span>

7) <span data-ttu-id="7aa06-144">V dalším kroku se přihlaste k [portál Azure](https://portal.azure.com)a přejděte do **Azure Active Directory > podnikové aplikace > všechny aplikace** části.</span><span class="sxs-lookup"><span data-stu-id="7aa06-144">Next, sign in to the [Azure portal](https://portal.azure.com), and browse to the **Azure Active Directory > Enterprise Apps > All applications**  section.</span></span>

8) <span data-ttu-id="7aa06-145">Pokud jste již nakonfigurovali LinkedIn Learning pro jednotné přihlašování, vyhledejte instanci LinkedIn Learning pomocí pole hledání.</span><span class="sxs-lookup"><span data-stu-id="7aa06-145">If you have already configured LinkedIn Learning for single sign-on, search for your instance of LinkedIn Learning using the search field.</span></span> <span data-ttu-id="7aa06-146">Jinak vyberte možnost **přidat** a vyhledejte **LinkedIn Learning** v galerii aplikací.</span><span class="sxs-lookup"><span data-stu-id="7aa06-146">Otherwise, select **Add** and search for **LinkedIn Learning** in the application gallery.</span></span> <span data-ttu-id="7aa06-147">Vyberte LinkedIn Learning ve výsledcích hledání a přidejte ji do seznamu aplikací.</span><span class="sxs-lookup"><span data-stu-id="7aa06-147">Select LinkedIn Learning from the search results, and add it to your list of applications.</span></span>

9)  <span data-ttu-id="7aa06-148">Vyberte instanci LinkedIn učení a pak vyberte **zřizování** kartě.</span><span class="sxs-lookup"><span data-stu-id="7aa06-148">Select your instance of LinkedIn Learning, then select the **Provisioning** tab.</span></span>

10) <span data-ttu-id="7aa06-149">Nastavte **režimu zřizování** k **automatické**.</span><span class="sxs-lookup"><span data-stu-id="7aa06-149">Set the **Provisioning Mode** to **Automatic**.</span></span>

![LinkedIn učení zřizování](./media/active-directory-saas-linkedinlearning-provisioning-tutorial/linkedin_3.PNG)

11)  <span data-ttu-id="7aa06-151">Vyplňte následující pole v části **přihlašovací údaje správce** :</span><span class="sxs-lookup"><span data-stu-id="7aa06-151">Fill in the following fields under **Admin Credentials** :</span></span>

* <span data-ttu-id="7aa06-152">V **URL klienta** zadejte https://api.linkedin.com.</span><span class="sxs-lookup"><span data-stu-id="7aa06-152">In the **Tenant URL** field, enter https://api.linkedin.com.</span></span>

* <span data-ttu-id="7aa06-153">V **tajný klíč tokenu** pole, zadejte přístupový token, který jste vygenerovali v kroku 1 a klikněte na **Test připojení** .</span><span class="sxs-lookup"><span data-stu-id="7aa06-153">In the **Secret Token** field, enter the access token you generated in step 1 and click **Test Connection** .</span></span>

* <span data-ttu-id="7aa06-154">Měli byste vidět úspěšné oznámení na straně ÚETUChcete portálu.</span><span class="sxs-lookup"><span data-stu-id="7aa06-154">You should see a success notification on the upper­right side of   your portal.</span></span>

12) <span data-ttu-id="7aa06-155">Zadejte e-mailovou adresu uživatele nebo skupiny, který by měly dostávat oznámení zřizování Chyba v **e-mailové oznámení** pole a zaškrtněte políčko níže.</span><span class="sxs-lookup"><span data-stu-id="7aa06-155">Enter the email address of a person or group who should receive provisioning error notifications in the **Notification Email** field, and check the checkbox below.</span></span>

13) <span data-ttu-id="7aa06-156">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="7aa06-156">Click **Save**.</span></span> 

14) <span data-ttu-id="7aa06-157">V **mapování atributů** , projděte si atributy uživatelů a skupin, které budou synchronizované z Azure AD LinkedIn Learning.</span><span class="sxs-lookup"><span data-stu-id="7aa06-157">In the **Attribute Mappings** section, review the user and group attributes that will be synchronized from Azure AD to LinkedIn Learning.</span></span> <span data-ttu-id="7aa06-158">Všimněte si, že atributy vybrán jako **párování** vlastnosti se použije tak, aby odpovídaly uživatelské účty a skupiny v LinkedIn Learning pro operace aktualizace.</span><span class="sxs-lookup"><span data-stu-id="7aa06-158">Note that the attributes selected as **Matching** properties will be used to match the user accounts and groups in LinkedIn Learning for update operations.</span></span> <span data-ttu-id="7aa06-159">Kliknutím na tlačítko Uložit potvrzení změny.</span><span class="sxs-lookup"><span data-stu-id="7aa06-159">Select the Save button to commit any changes.</span></span>

![LinkedIn učení zřizování](./media/active-directory-saas-linkedinlearning-provisioning-tutorial/linkedin_4.PNG)

15) <span data-ttu-id="7aa06-161">Povolit zřizování služby pro LinkedIn Learning Azure AD, změňte **Stav zřizování** k **na** v **nastavení** části</span><span class="sxs-lookup"><span data-stu-id="7aa06-161">To enable the Azure AD provisioning service for LinkedIn Learning, change the **Provisioning Status** to **On** in the **Settings** section</span></span>

16) <span data-ttu-id="7aa06-162">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="7aa06-162">Click **Save**.</span></span> 

<span data-ttu-id="7aa06-163">Tato akce spustí počáteční synchronizaci všech uživatelů a skupiny přiřazené k LinkedIn učení v části Uživatelé a skupiny.</span><span class="sxs-lookup"><span data-stu-id="7aa06-163">This will start the initial synchronization of any users and/or groups assigned to LinkedIn Learning in the Users and Groups section.</span></span> <span data-ttu-id="7aa06-164">Všimněte si, že počáteční synchronizace bude trvat déle než následné synchronizace, ke kterým dochází přibližně každých 20 minut, dokud je služba spuštěná.</span><span class="sxs-lookup"><span data-stu-id="7aa06-164">Note that the initial sync will take longer to perform than subsequent syncs, which occur approximately every 20 minutes as long as the service is running.</span></span> <span data-ttu-id="7aa06-165">Můžete použít **podrobnosti synchronizace** oddílu monitorovat průběh a odkazech zřízení sestavy aktivity, které popisují všechny akce prováděné při zřizování služby ve vaší aplikaci LinkedIn Learning.</span><span class="sxs-lookup"><span data-stu-id="7aa06-165">You can use the **Synchronization Details** section to monitor progress and follow links to provisioning activity reports, which describe all actions performed by the provisioning service on your LinkedIn Learning app.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="7aa06-166">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="7aa06-166">Additional Resources</span></span>

* [<span data-ttu-id="7aa06-167">Správa uživatelů zřizování účtu pro podnikové aplikace</span><span class="sxs-lookup"><span data-stu-id="7aa06-167">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-enterprise-apps-manage-provisioning.md)
* [<span data-ttu-id="7aa06-168">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="7aa06-168">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
