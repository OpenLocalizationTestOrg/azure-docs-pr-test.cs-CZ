---
title: 'Kurz: Azure Active Directory integrace s Jive | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Jive."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 6fbfdbe7-d66c-4305-9fea-76d6a6a92830
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: b1c0d0bc2d79427c055f577fe5f9d30d10f1bbdd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-jive-for-user-provisioning"></a><span data-ttu-id="12dee-103">Kurz: Konfigurace Jive pro zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="12dee-103">Tutorial: Configuring Jive for User Provisioning</span></span>

<span data-ttu-id="12dee-104">cílem Hello tohoto kurzu je tooshow hello kroky nutné tooperform v Jive a Azure AD tooautomatically zřídit a deaktivace zřízení uživatelských účtů z tooJive Azure AD.</span><span class="sxs-lookup"><span data-stu-id="12dee-104">hello objective of this tutorial is tooshow you hello steps you need tooperform in Jive and Azure AD tooautomatically provision and de-provision user accounts from Azure AD tooJive.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="12dee-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="12dee-105">Prerequisites</span></span>

<span data-ttu-id="12dee-106">Hello scénáři uvedeném v tomto kurzu se předpokládá, že už máte hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="12dee-106">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

*   <span data-ttu-id="12dee-107">Klienta služby Azure Active directory.</span><span class="sxs-lookup"><span data-stu-id="12dee-107">An Azure Active directory tenant.</span></span>
*   <span data-ttu-id="12dee-108">Jive jednotného přihlašování povolené předplatné.</span><span class="sxs-lookup"><span data-stu-id="12dee-108">A Jive single-sign on enabled subscription.</span></span>
*   <span data-ttu-id="12dee-109">Uživatelský účet v Jive s oprávněními správce týmu.</span><span class="sxs-lookup"><span data-stu-id="12dee-109">A user account in Jive with Team Admin permissions.</span></span>

## <a name="assigning-users-toojive"></a><span data-ttu-id="12dee-110">Přiřazení uživatelů tooJive</span><span class="sxs-lookup"><span data-stu-id="12dee-110">Assigning users tooJive</span></span>

<span data-ttu-id="12dee-111">Azure Active Directory používá koncept názvem "přiřazení" toodetermine uživatelů, kteří obdrželi přístup tooselected aplikace.</span><span class="sxs-lookup"><span data-stu-id="12dee-111">Azure Active Directory uses a concept called "assignments" toodetermine which users should receive access tooselected apps.</span></span> <span data-ttu-id="12dee-112">V kontextu hello zřizování účtu automatické uživatele se synchronizují pouze hello uživatelů a skupin, které byly "přiřazeny" tooan aplikace ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="12dee-112">In hello context of automatic user account provisioning, only hello users and groups that have been "assigned" tooan application in Azure AD is synchronized.</span></span>

<span data-ttu-id="12dee-113">Před konfigurací a povolení hello zřizování služby, musíte toodecide jaké uživatelů nebo skupin ve službě Azure AD představují hello uživatele, kteří potřebují přístup k aplikaci Jive tooyour.</span><span class="sxs-lookup"><span data-stu-id="12dee-113">Before configuring and enabling hello provisioning service, you need toodecide what users and/or groups in Azure AD represent hello users who need access tooyour Jive app.</span></span> <span data-ttu-id="12dee-114">Jakmile se rozhodli, můžete přiřadit tyto aplikace Jive tooyour uživatelů podle pokynů hello zde:</span><span class="sxs-lookup"><span data-stu-id="12dee-114">Once decided, you can assign these users tooyour Jive app by following hello instructions here:</span></span>

[<span data-ttu-id="12dee-115">Přiřadit uživatele nebo skupinu tooan firemní aplikace</span><span class="sxs-lookup"><span data-stu-id="12dee-115">Assign a user or group tooan enterprise app</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-toojive"></a><span data-ttu-id="12dee-116">Důležité tipy pro přiřazení uživatelů tooJive</span><span class="sxs-lookup"><span data-stu-id="12dee-116">Important tips for assigning users tooJive</span></span>

*   <span data-ttu-id="12dee-117">Dále je doporučeno jednoho uživatele Azure AD přiřadit hello tootest tooJive zřizování konfigurace.</span><span class="sxs-lookup"><span data-stu-id="12dee-117">It is recommended that a single Azure AD user be assigned tooJive tootest hello provisioning configuration.</span></span> <span data-ttu-id="12dee-118">Další uživatele nebo skupiny může být přiřazen později.</span><span class="sxs-lookup"><span data-stu-id="12dee-118">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="12dee-119">Při přiřazování tooJive uživatele, musíte vybrat platné uživatelské role.</span><span class="sxs-lookup"><span data-stu-id="12dee-119">When assigning a user tooJive, you must select a valid user role.</span></span> <span data-ttu-id="12dee-120">role "Výchozí přístup" Hello nefunguje pro zřizování.</span><span class="sxs-lookup"><span data-stu-id="12dee-120">hello "Default Access" role does not work for provisioning.</span></span>

## <a name="enable-user-provisioning"></a><span data-ttu-id="12dee-121">Povolit zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="12dee-121">Enable User Provisioning</span></span>

<span data-ttu-id="12dee-122">Tato část vás provede připojením vaší služby Azure AD tooJive uživatelský účet zřizování rozhraní API a konfigurace hello zřizování služby toocreate, aktualizovat a zakázat přiřazené uživatelské účty v Jive podle přiřazení uživatelů a skupin ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="12dee-122">This section guides you through connecting your Azure AD tooJive's user account provisioning API, and configuring hello provisioning service toocreate, update, and disable assigned user accounts in Jive based on user and group assignment in Azure AD.</span></span>

> [!TIP]
> <span data-ttu-id="12dee-123">Můžete také tooenabled na základě SAML jednotné přihlašování pro Jive, hello pokynů uvedených v [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="12dee-123">You may also choose tooenabled SAML-based Single Sign-On for Jive, following hello instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="12dee-124">Jednotné přihlašování se dá nakonfigurovat nezávisle na automatické zřizování, i když tyto dvě funkce doplnění navzájem.</span><span class="sxs-lookup"><span data-stu-id="12dee-124">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>

### <a name="tooconfigure-user-account-provisioning"></a><span data-ttu-id="12dee-125">tooconfigure uživatel účet zřizování:</span><span class="sxs-lookup"><span data-stu-id="12dee-125">tooconfigure user account provisioning:</span></span>

<span data-ttu-id="12dee-126">Hello cílem této části je toooutline jak tooenable zřizování uživatelů služby Active Directory uživatele tooJive účty.</span><span class="sxs-lookup"><span data-stu-id="12dee-126">hello objective of this section is toooutline how tooenable user provisioning of Active Directory user accounts tooJive.</span></span>
<span data-ttu-id="12dee-127">V rámci tohoto postupu jsou požadované tooprovide potřebujete toorequest z Jive.com token zabezpečení uživatele.</span><span class="sxs-lookup"><span data-stu-id="12dee-127">As part of this procedure, you are required tooprovide a user security token you need toorequest from Jive.com.</span></span>

1. <span data-ttu-id="12dee-128">V hello [portál Azure](https://portal.azure.com), procházet toohello **Azure Active Directory > podnikové aplikace > všechny aplikace** části.</span><span class="sxs-lookup"><span data-stu-id="12dee-128">In hello [Azure portal](https://portal.azure.com), browse toohello **Azure Active Directory > Enterprise Apps > All applications** section.</span></span>

2. <span data-ttu-id="12dee-129">Pokud jste již nakonfigurovali Jive pro jednotné přihlašování, vyhledejte instanci Jive pomocí hello vyhledávací pole.</span><span class="sxs-lookup"><span data-stu-id="12dee-129">If you have already configured Jive for single sign-on, search for your instance of Jive using hello search field.</span></span> <span data-ttu-id="12dee-130">Jinak vyberte možnost **přidat** a vyhledejte **Jive** v galerii aplikací hello.</span><span class="sxs-lookup"><span data-stu-id="12dee-130">Otherwise, select **Add** and search for **Jive** in hello application gallery.</span></span> <span data-ttu-id="12dee-131">Vyberte Jive z výsledků hledání hello a přidejte ji tooyour seznam aplikací.</span><span class="sxs-lookup"><span data-stu-id="12dee-131">Select Jive from hello search results, and add it tooyour list of applications.</span></span>

3. <span data-ttu-id="12dee-132">Vyberte instanci Jive a pak vyberte hello **zřizování** kartě.</span><span class="sxs-lookup"><span data-stu-id="12dee-132">Select your instance of Jive, then select hello **Provisioning** tab.</span></span>

4. <span data-ttu-id="12dee-133">Sada hello **režimu zřizování** příliš**automatické**.</span><span class="sxs-lookup"><span data-stu-id="12dee-133">Set hello **Provisioning Mode** too**Automatic**.</span></span> 

    ![Zřizování](./media/active-directory-saas-jive-provisioning-tutorial/provisioning.png)

5. <span data-ttu-id="12dee-135">V části hello **přihlašovací údaje správce** části, zadejte následující nastavení konfigurace hello:</span><span class="sxs-lookup"><span data-stu-id="12dee-135">Under hello **Admin Credentials** section, provide hello following configuration settings:</span></span>
   
    <span data-ttu-id="12dee-136">a.</span><span class="sxs-lookup"><span data-stu-id="12dee-136">a.</span></span> <span data-ttu-id="12dee-137">V hello **uživatelské jméno správce Jive** textovému poli, typ Jive účet název, který má hello **správce systému** profil v Jive.com přiřazen.</span><span class="sxs-lookup"><span data-stu-id="12dee-137">In hello **Jive Admin User Name** textbox, type a Jive account name that has hello **System Administrator** profile in Jive.com assigned.</span></span>
   
    <span data-ttu-id="12dee-138">b.</span><span class="sxs-lookup"><span data-stu-id="12dee-138">b.</span></span> <span data-ttu-id="12dee-139">V hello **heslo správce Jive** textovému poli, zadejte hello heslo pro tento účet.</span><span class="sxs-lookup"><span data-stu-id="12dee-139">In hello **Jive Admin Password** textbox, type hello password for this account.</span></span>
   
    <span data-ttu-id="12dee-140">c.</span><span class="sxs-lookup"><span data-stu-id="12dee-140">c.</span></span> <span data-ttu-id="12dee-141">V hello **URL klienta Jive** textovému poli, URL typu hello Jive klienta.</span><span class="sxs-lookup"><span data-stu-id="12dee-141">In hello **Jive Tenant URL** textbox, type hello Jive tenant URL.</span></span>
      
      > [!NOTE]
      > <span data-ttu-id="12dee-142">Hello Jive klienta adresa URL je adresa URL, která se používá ve vaší organizaci toolog v tooJive.</span><span class="sxs-lookup"><span data-stu-id="12dee-142">hello Jive tenant URL is URL that is used by your organization toolog in tooJive.</span></span>  
      > <span data-ttu-id="12dee-143">Obvykle hello adresa URL má hello následující formát: **www.\< organizace\>. jive.com**.</span><span class="sxs-lookup"><span data-stu-id="12dee-143">Typically, hello URL has hello following format: **www.\<organization\>.jive.com**.</span></span>          

6. <span data-ttu-id="12dee-144">V hello portálu Azure, klikněte na **Test připojení** tooensure Azure AD můžete připojit tooyour Jive aplikaci.</span><span class="sxs-lookup"><span data-stu-id="12dee-144">In hello Azure portal, click **Test Connection** tooensure Azure AD can connect tooyour Jive app.</span></span>

7. <span data-ttu-id="12dee-145">Zadejte hello e-mailovou adresu uživatele nebo skupiny, který by měly dostávat oznámení zřizování Chyba v hello **e-mailové oznámení** pole a zaškrtněte políčko hello níže.</span><span class="sxs-lookup"><span data-stu-id="12dee-145">Enter hello email address of a person or group who should receive provisioning error notifications in hello **Notification Email** field, and check hello checkbox below.</span></span>

8. <span data-ttu-id="12dee-146">Klikněte na tlačítko **uložit.**</span><span class="sxs-lookup"><span data-stu-id="12dee-146">Click **Save.**</span></span>

9. <span data-ttu-id="12dee-147">V části hello části mapování, vyberte **tooJive synchronizaci uživatelů Azure Active Directory.**</span><span class="sxs-lookup"><span data-stu-id="12dee-147">Under hello Mappings section, select **Synchronize Azure Active Directory Users tooJive.**</span></span>

10. <span data-ttu-id="12dee-148">V hello **mapování atributů** , projděte si hello uživatelské atributy, které jsou synchronizované z tooJive Azure AD.</span><span class="sxs-lookup"><span data-stu-id="12dee-148">In hello **Attribute Mappings** section, review hello user attributes that are synchronized from Azure AD tooJive.</span></span> <span data-ttu-id="12dee-149">Hello atributy vybrán jako **párování** vlastnosti jsou použité toomatch hello uživatelské účty v Jive pro operace aktualizace.</span><span class="sxs-lookup"><span data-stu-id="12dee-149">hello attributes selected as **Matching** properties are used toomatch hello user accounts in Jive for update operations.</span></span> <span data-ttu-id="12dee-150">Vyberte toocommit tlačítko hello uložit změny.</span><span class="sxs-lookup"><span data-stu-id="12dee-150">Select hello Save button toocommit any changes.</span></span>

11. <span data-ttu-id="12dee-151">tooenable hello zřizování služby Azure AD pro Jive, změna hello **Stav zřizování** příliš**na** v části Nastavení hello</span><span class="sxs-lookup"><span data-stu-id="12dee-151">tooenable hello Azure AD provisioning service for Jive, change hello **Provisioning Status** too**On** in hello Settings section</span></span>

12. <span data-ttu-id="12dee-152">Klikněte na tlačítko **uložit.**</span><span class="sxs-lookup"><span data-stu-id="12dee-152">Click **Save.**</span></span>

<span data-ttu-id="12dee-153">Spustí hello počáteční synchronizaci všech uživatelů a skupiny přiřazené tooJive v hello uživatelé a skupiny oddílu.</span><span class="sxs-lookup"><span data-stu-id="12dee-153">It starts hello initial synchronization of any users and/or groups assigned tooJive in hello Users and Groups section.</span></span> <span data-ttu-id="12dee-154">počáteční synchronizace Hello trvá déle tooperform než následné synchronizace, ke kterým dochází přibližně každých 20 minut, dokud se službou hello.</span><span class="sxs-lookup"><span data-stu-id="12dee-154">hello initial sync takes longer tooperform than subsequent syncs, which occur approximately every 20 minutes as long as hello service is running.</span></span> <span data-ttu-id="12dee-155">Můžete použít hello **podrobnosti synchronizace** části toomonitor průběh a postupujte podle pokynů odkazy tooprovisioning aktivity sestavy, které popisují všechny akce prováděné hello zřizování služby ve vaší aplikaci Jive.</span><span class="sxs-lookup"><span data-stu-id="12dee-155">You can use hello **Synchronization Details** section toomonitor progress and follow links tooprovisioning activity reports, which describe all actions performed by hello provisioning service on your Jive app.</span></span>

<span data-ttu-id="12dee-156">Nyní můžete vytvořit testovací účet.</span><span class="sxs-lookup"><span data-stu-id="12dee-156">You can now create a test account.</span></span> <span data-ttu-id="12dee-157">Počkejte, až minut too20 tooverify, který hello účet byl synchronizován tooJive.</span><span class="sxs-lookup"><span data-stu-id="12dee-157">Wait for up too20 minutes tooverify that hello account has been synchronized tooJive.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="12dee-158">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="12dee-158">Additional resources</span></span>

* [<span data-ttu-id="12dee-159">Správa uživatelů zřizování účtu pro podnikové aplikace</span><span class="sxs-lookup"><span data-stu-id="12dee-159">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="12dee-160">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="12dee-160">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="12dee-161">Konfigurovat jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="12dee-161">Configure Single Sign-on</span></span>](active-directory-saas-jive-tutorial.md)