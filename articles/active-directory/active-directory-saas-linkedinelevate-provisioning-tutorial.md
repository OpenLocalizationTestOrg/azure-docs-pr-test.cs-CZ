---
title: "Kurz: Konfigurace LinkedIn zvýšení oprávnění pro uživatele automatické zřizování s Azure Active Directory | Microsoft Docs"
description: "Zjistěte, jak tooconfigure Azure Active Directory tooautomatically zřídit a deaktivace zřízení uživatelských účtů tooLinkedIn zvýšení."
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
ms.openlocfilehash: 08201c078ece0054e75ec0c004840e5186e0e704
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-linkedin-elevate-for-automatic-user-provisioning"></a><span data-ttu-id="e9841-103">Kurz: Konfigurace LinkedIn zvýšení oprávnění pro uživatele automatické zřizování</span><span class="sxs-lookup"><span data-stu-id="e9841-103">Tutorial: Configuring LinkedIn Elevate for Automatic User Provisioning</span></span>


<span data-ttu-id="e9841-104">cílem Hello tohoto kurzu je tooshow hello kroky nutné tooperform v LinkedIn zvýšení oprávnění a Azure AD tooautomatically zřídit a deaktivace zřízení uživatelských účtů z Azure AD tooLinkedIn zvýšení.</span><span class="sxs-lookup"><span data-stu-id="e9841-104">hello objective of this tutorial is tooshow you hello steps you need tooperform in LinkedIn Elevate and Azure AD tooautomatically provision and de-provision user accounts from Azure AD tooLinkedIn Elevate.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="e9841-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e9841-105">Prerequisites</span></span>

<span data-ttu-id="e9841-106">Hello scénáři uvedeném v tomto kurzu se předpokládá, že už máte hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="e9841-106">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

*   <span data-ttu-id="e9841-107">Klient služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e9841-107">An Azure Active Directory tenant</span></span>
*   <span data-ttu-id="e9841-108">Klienta LinkedIn zvýšení oprávnění</span><span class="sxs-lookup"><span data-stu-id="e9841-108">A LinkedIn Elevate tenant</span></span> 
*   <span data-ttu-id="e9841-109">Účet správce v zvýšení oprávnění LinkedIn s toohello přístup k centru účtů LinkedIn</span><span class="sxs-lookup"><span data-stu-id="e9841-109">An administrator account in LinkedIn Elevate with access toohello LinkedIn Account Center</span></span>

> [!NOTE]
> <span data-ttu-id="e9841-110">Azure Active Directory se integruje s LinkedIn zvýšení oprávnění pomocí hello [SCIM](http://www.simplecloud.info/) protokolu.</span><span class="sxs-lookup"><span data-stu-id="e9841-110">Azure Active Directory integrates with LinkedIn Elevate using hello [SCIM](http://www.simplecloud.info/) protocol.</span></span>

## <a name="assigning-users-toolinkedin-elevate"></a><span data-ttu-id="e9841-111">Přiřazení uživatelů tooLinkedIn zvýšení</span><span class="sxs-lookup"><span data-stu-id="e9841-111">Assigning users tooLinkedIn Elevate</span></span>

<span data-ttu-id="e9841-112">Azure Active Directory používá koncept názvem "přiřazení" toodetermine uživatelů, kteří obdrželi přístup tooselected aplikace.</span><span class="sxs-lookup"><span data-stu-id="e9841-112">Azure Active Directory uses a concept called "assignments" toodetermine which users should receive access tooselected apps.</span></span> <span data-ttu-id="e9841-113">V kontextu hello zřizování účtu automatické uživatele se budou synchronizovat pouze hello uživatelů a skupin, které byly "přiřazeny" tooan aplikace ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e9841-113">In hello context of automatic user account provisioning, only hello users and groups that have been "assigned" tooan application in Azure AD will be synchronized.</span></span> 

<span data-ttu-id="e9841-114">Než nakonfigurujete a povolíte hello zřizování služby, je nutné toodecide jaké uživatelů nebo skupin ve službě Azure AD představují hello uživatele, kteří potřebují přístup k tooLinkedIn zvýšení.</span><span class="sxs-lookup"><span data-stu-id="e9841-114">Before configuring and enabling hello provisioning service, you will need toodecide what users and/or groups in Azure AD represent hello users who need access tooLinkedIn Elevate.</span></span> <span data-ttu-id="e9841-115">Jakmile se rozhodli, můžete přiřadit tyto uživatele tooLinkedIn zvýšení podle pokynů hello tady:</span><span class="sxs-lookup"><span data-stu-id="e9841-115">Once decided, you can assign these users tooLinkedIn Elevate by following hello instructions here:</span></span>

[<span data-ttu-id="e9841-116">Přiřadit uživatele nebo skupinu tooan firemní aplikace</span><span class="sxs-lookup"><span data-stu-id="e9841-116">Assign a user or group tooan enterprise app</span></span>](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-toolinkedin-elevate"></a><span data-ttu-id="e9841-117">Důležité tipy pro přiřazení uživatelů tooLinkedIn zvýšení</span><span class="sxs-lookup"><span data-stu-id="e9841-117">Important tips for assigning users tooLinkedIn Elevate</span></span>

*   <span data-ttu-id="e9841-118">Dále je doporučeno jednoho uživatele Azure AD přiřadit tooLinkedIn zvýšení tootest hello zřizování konfigurace.</span><span class="sxs-lookup"><span data-stu-id="e9841-118">It is recommended that a single Azure AD user be assigned tooLinkedIn Elevate tootest hello provisioning configuration.</span></span> <span data-ttu-id="e9841-119">Další uživatele nebo skupiny může být přiřazen později.</span><span class="sxs-lookup"><span data-stu-id="e9841-119">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="e9841-120">Při přiřazování uživatelů tooLinkedIn zvýšení, je nutné vybrat hello **uživatele** role v dialogovém okně přiřazení hello.</span><span class="sxs-lookup"><span data-stu-id="e9841-120">When assigning a user tooLinkedIn Elevate, you must select hello **User** role in hello assignment dialog.</span></span> <span data-ttu-id="e9841-121">role "Výchozí přístup" Hello nefunguje pro zřizování.</span><span class="sxs-lookup"><span data-stu-id="e9841-121">hello "Default Access" role does not work for provisioning.</span></span>


## <a name="configuring-user-provisioning-toolinkedin-elevate"></a><span data-ttu-id="e9841-122">Konfigurace tooLinkedIn zvýšení zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="e9841-122">Configuring user provisioning tooLinkedIn Elevate</span></span>

<span data-ttu-id="e9841-123">Tato část vás provede připojením zvýšení vaší služby Azure AD tooLinkedIn SCIM uživatelský účet zřizování rozhraní API a konfigurace hello zřizování služby toocreate, aktualizovat a zakázat přiřazené uživatelské účty v LinkedIn zvýšení oprávnění na základě uživatele a skupiny přiřazení ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e9841-123">This section guides you through connecting your Azure AD tooLinkedIn Elevate's SCIM user account provisioning API, and configuring hello provisioning service toocreate, update and disable assigned user accounts in LinkedIn Elevate based on user and group assignment in Azure AD.</span></span>

<span data-ttu-id="e9841-124">**Tip:** můžete také zvolit tooenabled na základě SAML jednotné přihlašování pro zvýšení oprávnění LinkedIn, hello pokynů uvedených v [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="e9841-124">**Tip:** You may also choose tooenabled SAML-based Single Sign-On for LinkedIn Elevate, following hello instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="e9841-125">Jednotné přihlašování se dá nakonfigurovat nezávisle na automatické zřizování, i když tyto dvě funkce vzájemně doplňují.</span><span class="sxs-lookup"><span data-stu-id="e9841-125">Single sign-on can be configured independently of automatic provisioning, though these two features complement each other.</span></span>


### <a name="tooconfigure-automatic-user-account-provisioning-toolinkedin-elevate-in-azure-ad"></a><span data-ttu-id="e9841-126">tooconfigure automatické uživatelský účet zřizování tooLinkedIn zvýšení ve službě Azure AD:</span><span class="sxs-lookup"><span data-stu-id="e9841-126">tooconfigure automatic user account provisioning tooLinkedIn Elevate in Azure AD:</span></span>


<span data-ttu-id="e9841-127">prvním krokem Hello je tooretrieve LinkedIn přístupový token.</span><span class="sxs-lookup"><span data-stu-id="e9841-127">hello first step is tooretrieve your LinkedIn access token.</span></span> <span data-ttu-id="e9841-128">Pokud jste správce podnikové sítě, můžete zřídit samoobslužné přístupový token.</span><span class="sxs-lookup"><span data-stu-id="e9841-128">If you are an Enterprise administrator, you can self-provision an access token.</span></span> <span data-ttu-id="e9841-129">V centru váš účet, přejděte příliš**nastavení &gt; globální nastavení** a otevřete hello **SCIM instalace** panelu.</span><span class="sxs-lookup"><span data-stu-id="e9841-129">In your account center, go too**Settings &gt; Global Settings** and open hello **SCIM Setup** panel.</span></span>

> [!NOTE]
> <span data-ttu-id="e9841-130">Pokud se připojujete centra účtů hello přímo a nikoli v rámci odkaz, můžete dosáhnout pomocí následujících kroků hello.</span><span class="sxs-lookup"><span data-stu-id="e9841-130">If you are accessing hello account center directly rather than through a link, you can reach it using hello following steps.</span></span>

1)  <span data-ttu-id="e9841-131">Přihlaste se tooAccount Center.</span><span class="sxs-lookup"><span data-stu-id="e9841-131">Sign in tooAccount Center.</span></span>

2)  <span data-ttu-id="e9841-132">Vyberte **správce &gt; nastavení správce** .</span><span class="sxs-lookup"><span data-stu-id="e9841-132">Select **Admin &gt; Admin Settings** .</span></span>

3)  <span data-ttu-id="e9841-133">Klikněte na tlačítko **Advanced integrace** na levém bočním panelu hello.</span><span class="sxs-lookup"><span data-stu-id="e9841-133">Click **Advanced Integrations** on hello left sidebar.</span></span> <span data-ttu-id="e9841-134">Jste směrovanou toohello centra účtů.</span><span class="sxs-lookup"><span data-stu-id="e9841-134">You are directed toohello account center.</span></span>

4)  <span data-ttu-id="e9841-135">Klikněte na tlačítko **+ přidat novou konfiguraci SCIM** a použijte postup hello vyplněním každé pole.</span><span class="sxs-lookup"><span data-stu-id="e9841-135">Click **+ Add new SCIM configuration** and follow hello procedure by filling in each field.</span></span>

> <span data-ttu-id="e9841-136">Když autoassign licencí není povolen, znamená to, že je synchronizovat pouze data uživatele.</span><span class="sxs-lookup"><span data-stu-id="e9841-136">When auto­assign licenses is not enabled, it means that only user data is synced.</span></span>

![LinkedIn zvýšení zřizování](./media/active-directory-saas-linkedin-elevate-provisioning-tutorial/linkedin_elevate1.PNG)

> <span data-ttu-id="e9841-138">Pokud je povoleno autolicense přiřazení, budete potřebovat toonote instanci aplikace a typ licence.</span><span class="sxs-lookup"><span data-stu-id="e9841-138">When auto­license assignment is enabled, you need toonote the application instance and license type.</span></span> <span data-ttu-id="e9841-139">Licence jsou přiřazeny na první přijde, nejprve sloužit základ, dokud jsou provedeny všechny licence hello.</span><span class="sxs-lookup"><span data-stu-id="e9841-139">Licenses are assigned on a first come, first serve basis until all hello licenses are taken.</span></span>

![LinkedIn zvýšení zřizování](./media/active-directory-saas-linkedin-elevate-provisioning-tutorial/linkedin_elevate2.PNG)

5)  <span data-ttu-id="e9841-141">Klikněte na tlačítko **vygenerovat token**.</span><span class="sxs-lookup"><span data-stu-id="e9841-141">Click **Generate token**.</span></span> <span data-ttu-id="e9841-142">Měli byste vidět zobrazení tokenu přístupu v části hello **přístupový token** pole.</span><span class="sxs-lookup"><span data-stu-id="e9841-142">You should see your access token display under hello **Access token** field.</span></span>

6)  <span data-ttu-id="e9841-143">Uložte přístup tokenu tooyour schránky nebo počítač před opuštěním stránky hello.</span><span class="sxs-lookup"><span data-stu-id="e9841-143">Save your access token tooyour clipboard or computer before leaving hello page.</span></span>

7) <span data-ttu-id="e9841-144">V dalším kroku přihlásit toohello [portál Azure](https://portal.azure.com)a vyhledejte toohello **Azure Active Directory > podnikové aplikace > všechny aplikace** části.</span><span class="sxs-lookup"><span data-stu-id="e9841-144">Next, sign in toohello [Azure portal](https://portal.azure.com), and browse toohello **Azure Active Directory > Enterprise Apps > All applications**  section.</span></span>

8) <span data-ttu-id="e9841-145">Pokud jste již nakonfigurovali LinkedIn zvýšení oprávnění pro jednotné přihlašování, vyhledejte instanci LinkedIn zvýšení oprávnění pomocí pole hledání hello.</span><span class="sxs-lookup"><span data-stu-id="e9841-145">If you have already configured LinkedIn Elevate for single sign-on, search for your instance of LinkedIn Elevate using hello search field.</span></span> <span data-ttu-id="e9841-146">Jinak vyberte možnost **přidat** a vyhledejte **zvýšení oprávnění LinkedIn** v galerii aplikací hello.</span><span class="sxs-lookup"><span data-stu-id="e9841-146">Otherwise, select **Add** and search for **LinkedIn Elevate** in hello application gallery.</span></span> <span data-ttu-id="e9841-147">Vyberte výsledky hledání hello LinkedIn zvýšení oprávnění a přidejte ji tooyour seznam aplikací.</span><span class="sxs-lookup"><span data-stu-id="e9841-147">Select LinkedIn Elevate from hello search results, and add it tooyour list of applications.</span></span>

9)  <span data-ttu-id="e9841-148">Vyberte instanci LinkedIn zvýšení oprávnění a potom vyberte hello **zřizování** kartě.</span><span class="sxs-lookup"><span data-stu-id="e9841-148">Select your instance of LinkedIn Elevate, then select hello **Provisioning** tab.</span></span>

10) <span data-ttu-id="e9841-149">Sada hello **režimu zřizování** příliš**automatické**.</span><span class="sxs-lookup"><span data-stu-id="e9841-149">Set hello **Provisioning Mode** too**Automatic**.</span></span>

![LinkedIn zvýšení zřizování](./media/active-directory-saas-linkedin-elevate-provisioning-tutorial/linkedin_elevate3.PNG)

11)  <span data-ttu-id="e9841-151">Vyplňte následující pole v části hello **přihlašovací údaje správce** :</span><span class="sxs-lookup"><span data-stu-id="e9841-151">Fill in hello following fields under **Admin Credentials** :</span></span>

* <span data-ttu-id="e9841-152">V hello **URL klienta** zadejte https://api.linkedin.com.</span><span class="sxs-lookup"><span data-stu-id="e9841-152">In hello **Tenant URL** field, enter https://api.linkedin.com.</span></span>

* <span data-ttu-id="e9841-153">V hello **tajný klíč tokenu** pole, zadejte hello přístupový token jste vygenerovali v kroku 1 a klikněte na **Test připojení** .</span><span class="sxs-lookup"><span data-stu-id="e9841-153">In hello **Secret Token** field, enter hello access token you generated in step 1 and click **Test Connection** .</span></span>

* <span data-ttu-id="e9841-154">Měli byste vidět úspěšné oznámení na straně ÚETUChcete hello portálu.</span><span class="sxs-lookup"><span data-stu-id="e9841-154">You should see a success notification on hello upper­right side of   your portal.</span></span>

12) <span data-ttu-id="e9841-155">Zadejte hello e-mailovou adresu uživatele nebo skupiny, který by měly dostávat oznámení zřizování Chyba v hello **e-mailové oznámení** pole a zaškrtněte políčko hello níže.</span><span class="sxs-lookup"><span data-stu-id="e9841-155">Enter hello email address of a person or group who should receive provisioning error notifications in hello **Notification Email** field, and check hello checkbox below.</span></span>

13) <span data-ttu-id="e9841-156">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="e9841-156">Click **Save**.</span></span> 

14) <span data-ttu-id="e9841-157">V hello **mapování atributů** , projděte si atributy hello uživatelů a skupin, které se mají synchronizovat ze služby Azure AD tooLinkedIn zvýšení.</span><span class="sxs-lookup"><span data-stu-id="e9841-157">In hello **Attribute Mappings** section, review hello user and group attributes that will be synchronized from Azure AD tooLinkedIn Elevate.</span></span> <span data-ttu-id="e9841-158">Všimněte si, že hello atributy vybrán jako **párování** vlastnosti budou použité toomatch hello uživatelské účty a skupiny v LinkedIn zvýšení oprávnění pro operace aktualizace.</span><span class="sxs-lookup"><span data-stu-id="e9841-158">Note that hello attributes selected as **Matching** properties will be used toomatch hello user accounts and groups in LinkedIn Elevate for update operations.</span></span> <span data-ttu-id="e9841-159">Vyberte toocommit tlačítko hello uložit změny.</span><span class="sxs-lookup"><span data-stu-id="e9841-159">Select hello Save button toocommit any changes.</span></span>

![LinkedIn zvýšení zřizování](./media/active-directory-saas-linkedin-elevate-provisioning-tutorial/linkedin_elevate4.PNG)

15) <span data-ttu-id="e9841-161">tooenable hello zřizování služby Azure AD pro LinkedIn zvýšení oprávnění, změna hello **Stav zřizování** příliš**na** v hello **nastavení** části</span><span class="sxs-lookup"><span data-stu-id="e9841-161">tooenable hello Azure AD provisioning service for LinkedIn Elevate, change hello **Provisioning Status** too**On** in hello **Settings** section</span></span>

16) <span data-ttu-id="e9841-162">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="e9841-162">Click **Save**.</span></span> 

<span data-ttu-id="e9841-163">Tato akce spustí počáteční synchronizaci hello všechny uživatele nebo skupiny přiřazené tooLinkedIn zvýšení v části Uživatelé a skupiny hello.</span><span class="sxs-lookup"><span data-stu-id="e9841-163">This will start hello initial synchronization of any users and/or groups assigned tooLinkedIn Elevate in hello Users and Groups section.</span></span> <span data-ttu-id="e9841-164">Všimněte si, že počáteční synchronizace hello bude trvat déle tooperform než následné synchronizace, ke kterým dochází přibližně každých 20 minut, dokud se službou hello.</span><span class="sxs-lookup"><span data-stu-id="e9841-164">Note that hello initial sync will take longer tooperform than subsequent syncs, which occur approximately every 20 minutes as long as hello service is running.</span></span> <span data-ttu-id="e9841-165">Můžete použít hello **podrobnosti synchronizace** části toomonitor průběh a postupujte podle pokynů odkazy tooprovisioning aktivity sestavy, které popisují všechny akce prováděné hello zřizování služby ve vaší aplikaci LinkedIn zvýšení oprávnění.</span><span class="sxs-lookup"><span data-stu-id="e9841-165">You can use hello **Synchronization Details** section toomonitor progress and follow links tooprovisioning activity reports, which describe all actions performed by hello provisioning service on your LinkedIn Elevate app.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="e9841-166">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="e9841-166">Additional Resources</span></span>

* [<span data-ttu-id="e9841-167">Správa uživatelů zřizování účtu pro podnikové aplikace</span><span class="sxs-lookup"><span data-stu-id="e9841-167">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-enterprise-apps-manage-provisioning.md)
* [<span data-ttu-id="e9841-168">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="e9841-168">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
