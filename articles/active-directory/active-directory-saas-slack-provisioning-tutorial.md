---
title: "Kurz: Konfigurace Slack pro zřizování automatické uživatelů s Azure Active Directory | Microsoft Docs"
description: "Zjistěte, jak tooconfigure Azure Active Directory tooautomatically zřídit a deaktivace zřízení uživatelských účtů tooSlack."
services: active-directory
documentationcenter: 
author: asmalser-msft
writer: asmalser-msft
manager: sakula
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: asmalser-msft
ms.reviewer: asmalser
ms.openlocfilehash: d0a565bbe0bd7e229b9dd99b1ebbaf67d93a2206
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-slack-for-automatic-user-provisioning"></a><span data-ttu-id="b8147-103">Kurz: Konfigurace Slack pro zřizování automatické uživatelů</span><span class="sxs-lookup"><span data-stu-id="b8147-103">Tutorial: Configuring Slack for Automatic User Provisioning</span></span>


<span data-ttu-id="b8147-104">Hello cílem tohoto kurzu je tooshow hello kroky nutné tooperform Slack a Azure AD tooautomatically zřídit a deaktivace zřízení uživatelských účtů z tooSlack Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b8147-104">hello objective of this tutorial is tooshow you hello steps you need tooperform in Slack and Azure AD tooautomatically provision and de-provision user accounts from Azure AD tooSlack.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="b8147-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b8147-105">Prerequisites</span></span>

<span data-ttu-id="b8147-106">Hello scénáři uvedeném v tomto kurzu se předpokládá, že už máte hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="b8147-106">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

*   <span data-ttu-id="b8147-107">Klienta služby Azure Active Active directory</span><span class="sxs-lookup"><span data-stu-id="b8147-107">An Azure Active Active directory tenant</span></span>
*   <span data-ttu-id="b8147-108">Systému Slack klienta s hello [Plus plán](https://aadsyncfabric.slack.com/pricing) nebo lépe povolena.</span><span class="sxs-lookup"><span data-stu-id="b8147-108">A Slack tenant with hello [Plus plan](https://aadsyncfabric.slack.com/pricing) or better enabled</span></span> 
*   <span data-ttu-id="b8147-109">Uživatelský účet v systému Slack s oprávněními správce Team</span><span class="sxs-lookup"><span data-stu-id="b8147-109">A user account in Slack with Team Admin permissions</span></span> 

<span data-ttu-id="b8147-110">Poznámka: hello Azure AD zřizování integrace spoléhá na hello [Slack SCIM API](https://api.slack.com/scim) který je k dispozici tooSlack týmy na hello Plus plán nebo lepší.</span><span class="sxs-lookup"><span data-stu-id="b8147-110">Note: hello Azure AD provisioning integration relies on hello [Slack SCIM API](https://api.slack.com/scim) which is available tooSlack teams on hello Plus plan or better.</span></span>

## <a name="assigning-users-tooslack"></a><span data-ttu-id="b8147-111">Přiřazení uživatelů tooSlack</span><span class="sxs-lookup"><span data-stu-id="b8147-111">Assigning users tooSlack</span></span>

<span data-ttu-id="b8147-112">Azure Active Directory používá koncept názvem "přiřazení" toodetermine uživatelů, kteří obdrželi přístup tooselected aplikace.</span><span class="sxs-lookup"><span data-stu-id="b8147-112">Azure Active Directory uses a concept called "assignments" toodetermine which users should receive access tooselected apps.</span></span> <span data-ttu-id="b8147-113">V kontextu hello zřizování účtu automatické uživatele se budou synchronizovat pouze hello uživatelů a skupin, které byly "přiřazeny" tooan aplikace ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b8147-113">In hello context of automatic user account provisioning, only hello users and groups that have been "assigned" tooan application in Azure AD will be synchronized.</span></span> 

<span data-ttu-id="b8147-114">Než nakonfigurujete a povolíte hello zřizování služby, je nutné toodecide jaké uživatelů nebo skupin v Azure AD představují hello uživatelé, kteří potřebují přístup k aplikaci tooyour Slack.</span><span class="sxs-lookup"><span data-stu-id="b8147-114">Before configuring and enabling hello provisioning service, you will need toodecide what users and/or groups in Azure AD represent hello users who need access tooyour Slack app.</span></span> <span data-ttu-id="b8147-115">Jakmile se rozhodli, můžete přiřadit tyto aplikace Slack tooyour uživatelů podle pokynů hello tady:</span><span class="sxs-lookup"><span data-stu-id="b8147-115">Once decided, you can assign these users tooyour Slack app by following hello instructions here:</span></span>

[<span data-ttu-id="b8147-116">Přiřadit uživatele nebo skupinu tooan firemní aplikace</span><span class="sxs-lookup"><span data-stu-id="b8147-116">Assign a user or group tooan enterprise app</span></span>](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-tooslack"></a><span data-ttu-id="b8147-117">Důležité tipy pro přiřazení uživatelů tooSlack</span><span class="sxs-lookup"><span data-stu-id="b8147-117">Important tips for assigning users tooSlack</span></span>

*   <span data-ttu-id="b8147-118">Dále je doporučeno jednoho uživatele Azure AD přiřadit hello tootest tooSlack zřizování konfigurace.</span><span class="sxs-lookup"><span data-stu-id="b8147-118">It is recommended that a single Azure AD user be assigned tooSlack tootest hello provisioning configuration.</span></span> <span data-ttu-id="b8147-119">Další uživatele nebo skupiny může být přiřazen později.</span><span class="sxs-lookup"><span data-stu-id="b8147-119">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="b8147-120">Při přiřazování tooSlack uživatele, je nutné vybrat hello **uživatele** nebo role "Skupina" v dialogovém okně přiřazení hello.</span><span class="sxs-lookup"><span data-stu-id="b8147-120">When assigning a user tooSlack, you must select hello **User** or "Group" role in hello assignment dialog.</span></span> <span data-ttu-id="b8147-121">role "Výchozí přístup" Hello nefunguje pro zřizování.</span><span class="sxs-lookup"><span data-stu-id="b8147-121">hello "Default Access" role does not work for provisioning.</span></span>


## <a name="configuring-user-provisioning-tooslack"></a><span data-ttu-id="b8147-122">Konfigurace tooSlack zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="b8147-122">Configuring user provisioning tooSlack</span></span> 

<span data-ttu-id="b8147-123">Tato část vás provede připojením vaší služby Azure AD tooSlack uživatelský účet zřizování rozhraní API a konfigurace hello zřizování služby toocreate, aktualizovat a zakázat přiřazené uživatelské účty v systému Slack podle přiřazení uživatelů a skupin ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b8147-123">This section guides you through connecting your Azure AD tooSlack's user account provisioning API, and configuring hello provisioning service toocreate, update and disable assigned user accounts in Slack based on user and group assignment in Azure AD.</span></span>

<span data-ttu-id="b8147-124">**Tip:** můžete také tooenabled na základě SAML jednotné přihlašování pro Slack, hello pokynů uvedených v (portál Azure) [https://portal.azure.com].</span><span class="sxs-lookup"><span data-stu-id="b8147-124">**Tip:** You may also choose tooenabled SAML-based Single Sign-On for Slack, following hello instructions provided in (Azure portal)[https://portal.azure.com].</span></span> <span data-ttu-id="b8147-125">Jednotné přihlašování se dá nakonfigurovat nezávisle na automatické zřizování, i když tyto dvě funkce doplnění navzájem.</span><span class="sxs-lookup"><span data-stu-id="b8147-125">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>


### <a name="tooconfigure-automatic-user-account-provisioning-tooslack-in-azure-ad"></a><span data-ttu-id="b8147-126">tooconfigure automatické uživatelský účet zřizování tooSlack ve službě Azure AD:</span><span class="sxs-lookup"><span data-stu-id="b8147-126">tooconfigure automatic user account provisioning tooSlack in Azure AD:</span></span>


1)  <span data-ttu-id="b8147-127">V hello [portál Azure](https://portal.azure.com), procházet toohello **Azure Active Directory > podnikové aplikace > všechny aplikace** části.</span><span class="sxs-lookup"><span data-stu-id="b8147-127">In hello [Azure portal](https://portal.azure.com), browse toohello **Azure Active Directory > Enterprise Apps > All applications**  section.</span></span>

2) <span data-ttu-id="b8147-128">Pokud jste již nakonfigurovali Slack pro jednotné přihlašování, vyhledejte instanci Slack pomocí hello vyhledávací pole.</span><span class="sxs-lookup"><span data-stu-id="b8147-128">If you have already configured Slack for single sign-on, search for your instance of Slack using hello search field.</span></span> <span data-ttu-id="b8147-129">Jinak vyberte možnost **přidat** a vyhledejte **Slack** v galerii aplikací hello.</span><span class="sxs-lookup"><span data-stu-id="b8147-129">Otherwise, select **Add** and search for **Slack** in hello application gallery.</span></span> <span data-ttu-id="b8147-130">Vyberte Slack z výsledků hledání hello a přidejte ji tooyour seznam aplikací.</span><span class="sxs-lookup"><span data-stu-id="b8147-130">Select Slack from hello search results, and add it tooyour list of applications.</span></span>

3)  <span data-ttu-id="b8147-131">Vyberte instanci systému Slack a pak vyberte hello **zřizování** kartě.</span><span class="sxs-lookup"><span data-stu-id="b8147-131">Select your instance of Slack, then select hello **Provisioning** tab.</span></span>

4)  <span data-ttu-id="b8147-132">Sada hello **režimu zřizování** příliš**automatické**.</span><span class="sxs-lookup"><span data-stu-id="b8147-132">Set hello **Provisioning Mode** too**Automatic**.</span></span>

![Slack zřizování](./media/active-directory-saas-slack-provisioning-tutorial/Slack1.PNG)

5)  <span data-ttu-id="b8147-134">V části hello **přihlašovací údaje správce** klikněte na tlačítko **Authorize**.</span><span class="sxs-lookup"><span data-stu-id="b8147-134">Under hello **Admin Credentials** section, click **Authorize**.</span></span> <span data-ttu-id="b8147-135">Otevře se dialogové okno Slack autorizace v nové okno prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="b8147-135">This opens a Slack authorization dialog in a new browser window.</span></span> 

6) <span data-ttu-id="b8147-136">V novém okně hello se přihlaste pomocí účtu správce Team Slack.</span><span class="sxs-lookup"><span data-stu-id="b8147-136">In hello new window, sign into Slack using your Team Admin account.</span></span> <span data-ttu-id="b8147-137">v dialogu autorizace výsledné hello, vyberte hello Slack týmu, který má být tooenable zřizování pro a potom vyberte **Authorize**.</span><span class="sxs-lookup"><span data-stu-id="b8147-137">in hello resulting authorization dialog, select hello Slack team that you want tooenable provisioning for, and then select **Authorize**.</span></span> <span data-ttu-id="b8147-138">Po dokončení, vrátí toohello Azure portálu toocomplete hello zřizování konfigurace.</span><span class="sxs-lookup"><span data-stu-id="b8147-138">Once completed, return toohello Azure portal toocomplete hello provisioning configuration.</span></span>

![Dialogové okno autorizace](./media/active-directory-saas-slack-provisioning-tutorial/Slack3.PNG)

7) <span data-ttu-id="b8147-140">V hello portálu Azure, klikněte na **Test připojení** tooensure Azure AD můžete připojit tooyour Slack aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b8147-140">In hello Azure portal, click **Test Connection** tooensure Azure AD can connect tooyour Slack app.</span></span> <span data-ttu-id="b8147-141">Pokud hello připojení nezdaří, zkontrolujte, zda že váš Slack účet má oprávnění správce týmu a hello "Ověřit" krok opakujte.</span><span class="sxs-lookup"><span data-stu-id="b8147-141">If hello connection fails, ensure your Slack account has Team Admin permissions and try hello "Authorize" step again.</span></span>

8) <span data-ttu-id="b8147-142">Zadejte hello e-mailovou adresu uživatele nebo skupiny, který by měly dostávat oznámení zřizování Chyba v hello **e-mailové oznámení** pole a zaškrtněte políčko hello níže.</span><span class="sxs-lookup"><span data-stu-id="b8147-142">Enter hello email address of a person or group who should receive provisioning error notifications in hello **Notification Email** field, and check hello checkbox below.</span></span>

9) <span data-ttu-id="b8147-143">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="b8147-143">Click **Save**.</span></span> 

10) <span data-ttu-id="b8147-144">V části hello části mapování, vyberte **synchronizaci uživatelů Azure Active Directory tooSlack**.</span><span class="sxs-lookup"><span data-stu-id="b8147-144">Under hello Mappings section, select **Synchronize Azure Active Directory Users tooSlack**.</span></span>

11) <span data-ttu-id="b8147-145">V hello **mapování atributů** , projděte si hello uživatelské atributy, které se mají synchronizovat z tooSlack Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b8147-145">In hello **Attribute Mappings** section, review hello user attributes that will be synchronized from Azure AD tooSlack.</span></span> <span data-ttu-id="b8147-146">Všimněte si, že hello atributy vybrán jako **párování** vlastnosti budou použité toomatch hello uživatelské účty v systému Slack pro operace aktualizace.</span><span class="sxs-lookup"><span data-stu-id="b8147-146">Note that hello attributes selected as **Matching** properties will be used toomatch hello user accounts in Slack for update operations.</span></span> <span data-ttu-id="b8147-147">Vyberte toocommit tlačítko hello uložit změny.</span><span class="sxs-lookup"><span data-stu-id="b8147-147">Select hello Save button toocommit any changes.</span></span>

12) <span data-ttu-id="b8147-148">tooenable hello zřizování služby Azure AD pro Slack, změna hello **Stav zřizování** příliš**na** v hello **nastavení** části</span><span class="sxs-lookup"><span data-stu-id="b8147-148">tooenable hello Azure AD provisioning service for Slack, change hello **Provisioning Status** too**On** in hello **Settings** section</span></span>

13) <span data-ttu-id="b8147-149">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="b8147-149">Click **Save**.</span></span> 

<span data-ttu-id="b8147-150">Tato akce spustí hello počáteční synchronizaci všech uživatelů a skupiny přiřazené tooSlack v hello uživatelé a skupiny oddílu.</span><span class="sxs-lookup"><span data-stu-id="b8147-150">This will start hello initial synchronization of any users and/or groups assigned tooSlack in hello Users and Groups section.</span></span> <span data-ttu-id="b8147-151">Všimněte si, že počáteční synchronizace hello bude trvat déle tooperform než následné synchronizace, ke kterým dochází přibližně každých 10 minut, dokud se službou hello.</span><span class="sxs-lookup"><span data-stu-id="b8147-151">Note that hello initial sync will take longer tooperform than subsequent syncs, which occur approximately every 10 minutes as long as hello service is running.</span></span> <span data-ttu-id="b8147-152">Můžete použít hello **podrobnosti synchronizace** části toomonitor průběh a postupujte podle pokynů odkazy tooprovisioning aktivity sestavy, které popisují všechny akce prováděné hello zřizování služby ve vaší aplikaci Slack.</span><span class="sxs-lookup"><span data-stu-id="b8147-152">You can use hello **Synchronization Details** section toomonitor progress and follow links tooprovisioning activity reports, which describe all actions performed by hello provisioning service on your Slack app.</span></span>

## <a name="optional-configuring-group-object-provisioning-tooslack"></a><span data-ttu-id="b8147-153">[Nepovinné] Konfigurace objektu skupiny zřizování tooSlack</span><span class="sxs-lookup"><span data-stu-id="b8147-153">[Optional] Configuring group object provisioning tooSlack</span></span> 

<span data-ttu-id="b8147-154">Volitelně můžete povolit zajišťování hello objektů skupiny z tooSlack Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b8147-154">Optionally, you can enable hello provisioning of group objects from Azure AD tooSlack.</span></span> <span data-ttu-id="b8147-155">To se liší od "přiřazení skupiny uživatelů", v tomto objektu skutečné skupiny hello kromě tooits členy bude replikován z tooSlack Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b8147-155">This is different from "assigning groups of users", in that hello actual group object in addition tooits members will be replicated from Azure AD tooSlack.</span></span> <span data-ttu-id="b8147-156">Například pokud máte skupinu s názvem "Moje skupina" ve službě Azure AD, bude vytvořen identitical skupinu s názvem "Moje skupina" uvnitř Slack.</span><span class="sxs-lookup"><span data-stu-id="b8147-156">For example, if you have a group named "My Group" in Azure AD, an identitical group named "My Group" will be created inside Slack.</span></span>

### <a name="tooenable-provisioning-of-group-objects"></a><span data-ttu-id="b8147-157">tooenable zřizování objektů skupiny:</span><span class="sxs-lookup"><span data-stu-id="b8147-157">tooenable provisioning of group objects:</span></span>

1) <span data-ttu-id="b8147-158">V části hello části mapování, vyberte **synchronizaci skupinám Azure Active Directory tooSlack**.</span><span class="sxs-lookup"><span data-stu-id="b8147-158">Under hello Mappings section, select **Synchronize Azure Active Directory Groups tooSlack**.</span></span>

2) <span data-ttu-id="b8147-159">V okně hello mapování atributů nastavte tooYes povoleno.</span><span class="sxs-lookup"><span data-stu-id="b8147-159">In hello Attribute Mapping blade, set Enabled tooYes.</span></span>

3) <span data-ttu-id="b8147-160">V hello **mapování atributů** , projděte si hello skupiny atributy, které se mají synchronizovat z tooSlack Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b8147-160">In hello **Attribute Mappings** section, review hello group attributes that will be synchronized from Azure AD tooSlack.</span></span> <span data-ttu-id="b8147-161">Všimněte si, že hello atributy vybrán jako **párování** vlastnosti budou použité toomatch hello skupiny v systému Slack pro operace aktualizace.</span><span class="sxs-lookup"><span data-stu-id="b8147-161">Note that hello attributes selected as **Matching** properties will be used toomatch hello groups in Slack for update operations.</span></span> 

4) <span data-ttu-id="b8147-162">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="b8147-162">Click **Save**.</span></span>

<span data-ttu-id="b8147-163">Tento výsledek v tooSlack objekty přiřazené žádné skupiny v hello **uživatelů a skupin** část plně synchronizovaných z tooSlack Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b8147-163">This result in any group objects assigned tooSlack in hello **Users and Groups** section being fully synchronized from Azure AD tooSlack.</span></span> <span data-ttu-id="b8147-164">Můžete použít hello **podrobnosti synchronizace** části toomonitor průběh a postupujte podle pokynů odkazy tooprovisioning aktivity sestavy, které popisují všechny akce prováděné hello zřizování služby ve vaší aplikaci Slack.</span><span class="sxs-lookup"><span data-stu-id="b8147-164">You can use hello **Synchronization Details** section toomonitor progress and follow links tooprovisioning activity reports, which describe all actions performed by hello provisioning service on your Slack app.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="b8147-165">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="b8147-165">Additional Resources</span></span>

* [<span data-ttu-id="b8147-166">Správa uživatelů zřizování účtu pro podnikové aplikace</span><span class="sxs-lookup"><span data-stu-id="b8147-166">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-enterprise-apps-manage-provisioning.md)
* [<span data-ttu-id="b8147-167">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="b8147-167">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
