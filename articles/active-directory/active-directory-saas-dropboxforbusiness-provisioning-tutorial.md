---
title: 'Kurz: Azure Active Directory integrace s Dropbox pro firmy | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Dropbox pro firmy."
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
ms.openlocfilehash: 0fb01eab4f7c6c4516eac64a4343e46ea221f98d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-dropbox-for-business-for-automatic-user-provisioning"></a><span data-ttu-id="d8cac-103">Kurz: Konfigurace Dropbox pro firmy pro zřizování automatické uživatelů</span><span class="sxs-lookup"><span data-stu-id="d8cac-103">Tutorial: Configuring Dropbox for Business for Automatic User Provisioning</span></span>

<span data-ttu-id="d8cac-104">cílem Hello tohoto kurzu je tooshow hello kroky nutné tooperform v Dropbox pro firmy a Azure AD tooautomatically zřídit a deaktivace zřízení uživatelských účtů z Azure AD tooDropbox pro firmy.</span><span class="sxs-lookup"><span data-stu-id="d8cac-104">hello objective of this tutorial is tooshow you hello steps you need tooperform in Dropbox for Business and Azure AD tooautomatically provision and de-provision user accounts from Azure AD tooDropbox for Business.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d8cac-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d8cac-105">Prerequisites</span></span>

<span data-ttu-id="d8cac-106">Hello scénáři uvedeném v tomto kurzu se předpokládá, že už máte hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="d8cac-106">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

*   <span data-ttu-id="d8cac-107">Klienta služby Azure Active directory.</span><span class="sxs-lookup"><span data-stu-id="d8cac-107">An Azure Active directory tenant.</span></span>
*   <span data-ttu-id="d8cac-108">Dropbox pro obchodní jednotného přihlašování povolené předplatné.</span><span class="sxs-lookup"><span data-stu-id="d8cac-108">A Dropbox for Business single-sign on enabled subscription.</span></span>
*   <span data-ttu-id="d8cac-109">Uživatelský účet v Dropbox pro firmy s oprávněními správce týmu.</span><span class="sxs-lookup"><span data-stu-id="d8cac-109">A user account in Dropbox for Business with Team Admin permissions.</span></span>

## <a name="assigning-users-toodropbox-for-business"></a><span data-ttu-id="d8cac-110">Přiřazení uživatelů tooDropbox pro firmy</span><span class="sxs-lookup"><span data-stu-id="d8cac-110">Assigning users tooDropbox for Business</span></span>

<span data-ttu-id="d8cac-111">Azure Active Directory používá koncept názvem "přiřazení" toodetermine uživatelů, kteří obdrželi přístup tooselected aplikace.</span><span class="sxs-lookup"><span data-stu-id="d8cac-111">Azure Active Directory uses a concept called "assignments" toodetermine which users should receive access tooselected apps.</span></span> <span data-ttu-id="d8cac-112">V kontextu hello zřizování účtu automatické uživatele se synchronizují pouze hello uživatelů a skupin, které byly "přiřazeny" tooan aplikace ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d8cac-112">In hello context of automatic user account provisioning, only hello users and groups that have been "assigned" tooan application in Azure AD is synchronized.</span></span>

<span data-ttu-id="d8cac-113">Před konfigurací a povolení hello zřizování služby, musíte toodecide jaké uživatelů nebo skupin ve službě Azure AD představují hello uživatele, kteří potřebují přístup k tooyour Dropbox pro obchodní aplikace.</span><span class="sxs-lookup"><span data-stu-id="d8cac-113">Before configuring and enabling hello provisioning service, you need toodecide what users and/or groups in Azure AD represent hello users who need access tooyour Dropbox for Business app.</span></span> <span data-ttu-id="d8cac-114">Jakmile se rozhodli, můžete přiřadit tyto uživatele tooyour Dropbox pro obchodní aplikace podle pokynů hello tady:</span><span class="sxs-lookup"><span data-stu-id="d8cac-114">Once decided, you can assign these users tooyour Dropbox for Business app by following hello instructions here:</span></span>

[<span data-ttu-id="d8cac-115">Přiřadit uživatele nebo skupinu tooan firemní aplikace</span><span class="sxs-lookup"><span data-stu-id="d8cac-115">Assign a user or group tooan enterprise app</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-toodropbox-for-business"></a><span data-ttu-id="d8cac-116">Důležité tipy pro přiřazení uživatelů tooDropbox pro firmy</span><span class="sxs-lookup"><span data-stu-id="d8cac-116">Important tips for assigning users tooDropbox for Business</span></span>

*   <span data-ttu-id="d8cac-117">Dále je doporučeno jednoho uživatele Azure AD je přiřazen tooDropbox pro hello firmy tootest zřizování konfigurace.</span><span class="sxs-lookup"><span data-stu-id="d8cac-117">It is recommended that a single Azure AD user is assigned tooDropbox for Business tootest hello provisioning configuration.</span></span> <span data-ttu-id="d8cac-118">Další uživatele nebo skupiny může být přiřazen později.</span><span class="sxs-lookup"><span data-stu-id="d8cac-118">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="d8cac-119">Při přiřazování tooDropbox uživatele pro firmy, musíte vybrat platné uživatelské role.</span><span class="sxs-lookup"><span data-stu-id="d8cac-119">When assigning a user tooDropbox for Business, you must select a valid user role.</span></span> <span data-ttu-id="d8cac-120">role "Výchozí přístup" Hello nefunguje pro zřizování...</span><span class="sxs-lookup"><span data-stu-id="d8cac-120">hello "Default Access" role does not work for provisioning..</span></span>

## <a name="enable-automated-user-provisioning"></a><span data-ttu-id="d8cac-121">Povolit automatické zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="d8cac-121">Enable Automated User Provisioning</span></span>

<span data-ttu-id="d8cac-122">Tato část vás provede připojením tooDropbox vaší služby Azure AD pro firmy na uživatelský účet zřizování rozhraní API a konfiguraci hello zřizování služby toocreate, aktualizovat a zakázat přiřazené uživatelské účty v Dropbox pro firmy na základě uživatele a skupiny přiřazení ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d8cac-122">This section guides you through connecting your Azure AD tooDropbox for Business's user account provisioning API, and configuring hello provisioning service toocreate, update, and disable assigned user accounts in Dropbox for Business based on user and group assignment in Azure AD.</span></span>

>[!Tip]
><span data-ttu-id="d8cac-123">Můžete také zvolit tooenabled na základě SAML jednotné přihlašování pro Dropbox pro firmy, hello pokynů uvedených v [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="d8cac-123">You may also choose tooenabled SAML-based Single Sign-On for Dropbox for Business, following hello instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="d8cac-124">Jednotné přihlašování se dá nakonfigurovat nezávisle na automatické zřizování, i když tyto dvě funkce doplnění navzájem.</span><span class="sxs-lookup"><span data-stu-id="d8cac-124">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>

### <a name="tooconfigure-automatic-user-account-provisioning"></a><span data-ttu-id="d8cac-125">tooconfigure automatické uživatel účet zřizování:</span><span class="sxs-lookup"><span data-stu-id="d8cac-125">tooconfigure automatic user account provisioning:</span></span>

1. <span data-ttu-id="d8cac-126">V hello [portál Azure](https://portal.azure.com), procházet toohello **Azure Active Directory > podnikové aplikace > všechny aplikace** části.</span><span class="sxs-lookup"><span data-stu-id="d8cac-126">In hello [Azure portal](https://portal.azure.com), browse toohello **Azure Active Directory > Enterprise Apps > All applications** section.</span></span>

2. <span data-ttu-id="d8cac-127">Pokud jste již nakonfigurovali Dropbox pro firmy pro jednotné přihlašování, vyhledejte instanci Dropboxu pro firmy pomocí hello vyhledávací pole.</span><span class="sxs-lookup"><span data-stu-id="d8cac-127">If you have already configured Dropbox for Business for single sign-on, search for your instance of Dropbox for Business using hello search field.</span></span> <span data-ttu-id="d8cac-128">Jinak vyberte možnost **přidat** a vyhledejte **Dropbox pro firmy** v galerii aplikací hello.</span><span class="sxs-lookup"><span data-stu-id="d8cac-128">Otherwise, select **Add** and search for **Dropbox for Business** in hello application gallery.</span></span> <span data-ttu-id="d8cac-129">Vyberte Dropbox pro firmy ze hello výsledky vyhledávání a přidejte ji tooyour seznam aplikací.</span><span class="sxs-lookup"><span data-stu-id="d8cac-129">Select Dropbox for Business from hello search results, and add it tooyour list of applications.</span></span>

3. <span data-ttu-id="d8cac-130">Vyberte instanci Dropboxu pro firmy a pak vyberte hello **zřizování** kartě.</span><span class="sxs-lookup"><span data-stu-id="d8cac-130">Select your instance of Dropbox for Business, then select hello **Provisioning** tab.</span></span>

4. <span data-ttu-id="d8cac-131">Sada hello **režimu zřizování** příliš**automatické**.</span><span class="sxs-lookup"><span data-stu-id="d8cac-131">Set hello **Provisioning Mode** too**Automatic**.</span></span> 

    ![Zřizování](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/provisioning.png)

5. <span data-ttu-id="d8cac-133">V části hello **přihlašovací údaje správce** klikněte na tlačítko **Authorize**.</span><span class="sxs-lookup"><span data-stu-id="d8cac-133">Under hello **Admin Credentials** section, click **Authorize**.</span></span> <span data-ttu-id="d8cac-134">Dropbox pro obchodní přihlašovací dialogové okno otevře v novém okně prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="d8cac-134">It opens a Dropbox for Business login dialog in a new browser window.</span></span>

6. <span data-ttu-id="d8cac-135">Na hello **přihlášení toolink tooDropbox s Azure AD** dialogové okno, přihlášení tooyour Dropbox pro obchodní klienta.</span><span class="sxs-lookup"><span data-stu-id="d8cac-135">On hello **Sign-in tooDropbox toolink with Azure AD** dialog, sign in tooyour Dropbox for Business tenant.</span></span>

     <span data-ttu-id="d8cac-136">![Zřizování uživatelů](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/ic769518.png "zřizování uživatelů")</span><span class="sxs-lookup"><span data-stu-id="d8cac-136">![User provisioning](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/ic769518.png "User provisioning")</span></span>

7. <span data-ttu-id="d8cac-137">Zkontrolujte, které chcete toogive Azure Active Directory oprávnění toomake změny tooyour Dropbox pro obchodní klienta.</span><span class="sxs-lookup"><span data-stu-id="d8cac-137">Confirm that you would like toogive Azure Active Directory permission toomake changes tooyour Dropbox for Business tenant.</span></span> <span data-ttu-id="d8cac-138">Klikněte na tlačítko **povolit**.</span><span class="sxs-lookup"><span data-stu-id="d8cac-138">Click **Allow**.</span></span>
    
      <span data-ttu-id="d8cac-139">![Zřizování uživatelů](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/ic769519.png "zřizování uživatelů")</span><span class="sxs-lookup"><span data-stu-id="d8cac-139">![User provisioning](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/ic769519.png "User provisioning")</span></span>

8. <span data-ttu-id="d8cac-140">V hello portálu Azure, klikněte na **Test připojení** tooensure Azure AD můžete připojit tooyour Dropbox pro obchodní aplikace.</span><span class="sxs-lookup"><span data-stu-id="d8cac-140">In hello Azure portal, click **Test Connection** tooensure Azure AD can connect tooyour Dropbox for Business app.</span></span> <span data-ttu-id="d8cac-141">Pokud hello připojení nezdaří, zkontrolujte vaši Dropbox pro obchodní účet má oprávnění správce týmu a zkuste to hello **"Ověřit"** krok opakujte.</span><span class="sxs-lookup"><span data-stu-id="d8cac-141">If hello connection fails, ensure your Dropbox for Business account has Team Admin permissions and try hello **"Authorize"** step again.</span></span>

9. <span data-ttu-id="d8cac-142">Zadejte hello e-mailovou adresu uživatele nebo skupiny, který by měly dostávat oznámení zřizování Chyba v hello **e-mailové oznámení** pole a zaškrtněte políčko hello.</span><span class="sxs-lookup"><span data-stu-id="d8cac-142">Enter hello email address of a person or group who should receive provisioning error notifications in hello **Notification Email** field, and check hello checkbox.</span></span>

10. <span data-ttu-id="d8cac-143">Klikněte na tlačítko **uložit.**</span><span class="sxs-lookup"><span data-stu-id="d8cac-143">Click **Save.**</span></span>

11. <span data-ttu-id="d8cac-144">V části hello části mapování, vyberte **tooDropbox synchronizaci uživatelů Azure Active Directory pro firmy.**</span><span class="sxs-lookup"><span data-stu-id="d8cac-144">Under hello Mappings section, select **Synchronize Azure Active Directory Users tooDropbox for Business.**</span></span>

12. <span data-ttu-id="d8cac-145">V hello **mapování atributů** , projděte si hello uživatelské atributy, které jsou synchronizované z Azure AD tooDropbox pro firmy.</span><span class="sxs-lookup"><span data-stu-id="d8cac-145">In hello **Attribute Mappings** section, review hello user attributes that are synchronized from Azure AD tooDropbox for Business.</span></span> <span data-ttu-id="d8cac-146">Hello atributy vybrán jako **párování** vlastnosti jsou použité toomatch hello uživatelské účty v Dropbox pro firmy pro operace aktualizace.</span><span class="sxs-lookup"><span data-stu-id="d8cac-146">hello attributes selected as **Matching** properties are used toomatch hello user accounts in Dropbox for Business for update operations.</span></span> <span data-ttu-id="d8cac-147">Vyberte toocommit tlačítko hello uložit změny.</span><span class="sxs-lookup"><span data-stu-id="d8cac-147">Select hello Save button toocommit any changes.</span></span>

13. <span data-ttu-id="d8cac-148">tooenable hello zřizování služby Azure AD pro Dropbox pro firmy, změna hello **Stav zřizování** příliš**na** v části Nastavení hello</span><span class="sxs-lookup"><span data-stu-id="d8cac-148">tooenable hello Azure AD provisioning service for Dropbox for Business, change hello **Provisioning Status** too**On** in hello Settings section</span></span>

14. <span data-ttu-id="d8cac-149">Klikněte na tlačítko **uložit.**</span><span class="sxs-lookup"><span data-stu-id="d8cac-149">Click **Save.**</span></span>

<span data-ttu-id="d8cac-150">Spustí hello počáteční synchronizaci všech uživatelů a skupiny přiřazené tooDropbox pro firmy v hello uživatelé a skupiny oddílu.</span><span class="sxs-lookup"><span data-stu-id="d8cac-150">It starts hello initial synchronization of any users and/or groups assigned tooDropbox for Business in hello Users and Groups section.</span></span> <span data-ttu-id="d8cac-151">počáteční synchronizace Hello trvá déle tooperform než následné synchronizace, ke kterým dochází přibližně každých 20 minut, dokud se službou hello.</span><span class="sxs-lookup"><span data-stu-id="d8cac-151">hello initial sync takes longer tooperform than subsequent syncs, which occur approximately every 20 minutes as long as hello service is running.</span></span> <span data-ttu-id="d8cac-152">Můžete použít hello **podrobnosti synchronizace** části toomonitor průběh a postupujte podle pokynů odkazy tooprovisioning aktivity sestavy, které popisují všechny akce prováděné hello zřizování služby na vaší schránky pro obchodní aplikace.</span><span class="sxs-lookup"><span data-stu-id="d8cac-152">You can use hello **Synchronization Details** section toomonitor progress and follow links tooprovisioning activity reports, which describe all actions performed by hello provisioning service on your Dropbox for Business app.</span></span>

<span data-ttu-id="d8cac-153">Nyní můžete vytvořit testovací účet.</span><span class="sxs-lookup"><span data-stu-id="d8cac-153">You can now create a test account.</span></span> <span data-ttu-id="d8cac-154">Počkejte, až minut too20 tooverify, který hello účet byl synchronizován tooDropbox pro firmy.</span><span class="sxs-lookup"><span data-stu-id="d8cac-154">Wait for up too20 minutes tooverify that hello account has been synchronized tooDropbox for Business.</span></span>

<span data-ttu-id="d8cac-155">Související stav je indikován úspěšně dokončila uživatele zřizování cyklu.</span><span class="sxs-lookup"><span data-stu-id="d8cac-155">A successfully completed user provisioning cycle is indicated by a related status.</span></span>

<span data-ttu-id="d8cac-156">![Přiřazení uživatelů](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/IC769523.png "přiřazení uživatelů")</span><span class="sxs-lookup"><span data-stu-id="d8cac-156">![Assign users](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/IC769523.png "Assign users")</span></span>


## <a name="additional-resources"></a><span data-ttu-id="d8cac-157">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="d8cac-157">Additional resources</span></span>

* [<span data-ttu-id="d8cac-158">Správa uživatelů zřizování účtu pro podnikové aplikace</span><span class="sxs-lookup"><span data-stu-id="d8cac-158">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d8cac-159">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="d8cac-159">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="d8cac-160">Konfigurovat jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="d8cac-160">Configure Single Sign-on</span></span>](active-directory-saas-dropboxforbusiness-tutorial.md)