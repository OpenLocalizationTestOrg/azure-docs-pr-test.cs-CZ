---
title: 'Kurz: Azure Active Directory integrace s Citrix GoToMeeting | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Citrix GoToMeeting."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 0f59fedb-2cf8-48d2-a5fb-222ed943ff78
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 3c6eed5309dfa384c292b0cf63f8aa58988add81
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-citrix-gotomeeting-for-automatic-user-provisioning"></a><span data-ttu-id="3ef28-103">Kurz: Konfigurace systému Citrix GoToMeeting pro zřizování automatické uživatelů</span><span class="sxs-lookup"><span data-stu-id="3ef28-103">Tutorial: Configuring Citrix GoToMeeting for Automatic User Provisioning</span></span>

<span data-ttu-id="3ef28-104">cílem Hello tohoto kurzu je tooshow hello kroky nutné tooperform v Citrix GoToMeeting a Azure AD tooautomatically zřídit a deaktivace zřízení uživatelských účtů z Azure AD tooCitrix GoToMeeting.</span><span class="sxs-lookup"><span data-stu-id="3ef28-104">hello objective of this tutorial is tooshow you hello steps you need tooperform in Citrix GoToMeeting and Azure AD tooautomatically provision and de-provision user accounts from Azure AD tooCitrix GoToMeeting.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3ef28-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3ef28-105">Prerequisites</span></span>

<span data-ttu-id="3ef28-106">Hello scénáři uvedeném v tomto kurzu se předpokládá, že už máte hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="3ef28-106">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

*   <span data-ttu-id="3ef28-107">Klienta služby Azure Active directory.</span><span class="sxs-lookup"><span data-stu-id="3ef28-107">An Azure Active directory tenant.</span></span>
*   <span data-ttu-id="3ef28-108">Citrix GoToMeeting jednotného přihlašování povolené předplatné.</span><span class="sxs-lookup"><span data-stu-id="3ef28-108">A Citrix GoToMeeting single-sign on enabled subscription.</span></span>
*   <span data-ttu-id="3ef28-109">Uživatelský účet v Citrix GoToMeeting s oprávněními správce týmu.</span><span class="sxs-lookup"><span data-stu-id="3ef28-109">A user account in Citrix GoToMeeting with Team Admin permissions.</span></span>

## <a name="assigning-users-toocitrix-gotomeeting"></a><span data-ttu-id="3ef28-110">Přiřazení uživatelů tooCitrix GoToMeeting</span><span class="sxs-lookup"><span data-stu-id="3ef28-110">Assigning users tooCitrix GoToMeeting</span></span>

<span data-ttu-id="3ef28-111">Azure Active Directory používá koncept názvem "přiřazení" toodetermine uživatelů, kteří obdrželi přístup tooselected aplikace.</span><span class="sxs-lookup"><span data-stu-id="3ef28-111">Azure Active Directory uses a concept called "assignments" toodetermine which users should receive access tooselected apps.</span></span> <span data-ttu-id="3ef28-112">V kontextu hello zřizování účtu automatické uživatele se synchronizují pouze hello uživatelů a skupin, které byly "přiřazeny" tooan aplikace ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3ef28-112">In hello context of automatic user account provisioning, only hello users and groups that have been "assigned" tooan application in Azure AD is synchronized.</span></span>

<span data-ttu-id="3ef28-113">Než nakonfigurujete a povolíte hello zřizování služby, musíte toodecide jaké uživatelů nebo skupin ve službě Azure AD představují hello uživatele, kteří potřebují přístup k tooyour Citrix GoToMeeting aplikace.</span><span class="sxs-lookup"><span data-stu-id="3ef28-113">Before configuring and enabling hello provisioning service, you need toodecide what users and/or groups in Azure AD represent hello users who need access tooyour Citrix GoToMeeting app.</span></span> <span data-ttu-id="3ef28-114">Jakmile se rozhodli, můžete přiřadit tyto uživatele tooyour aplikace Citrix GoToMeeting podle pokynů hello tady:</span><span class="sxs-lookup"><span data-stu-id="3ef28-114">Once decided, you can assign these users tooyour Citrix GoToMeeting app by following hello instructions here:</span></span>

[<span data-ttu-id="3ef28-115">Přiřadit uživatele nebo skupinu tooan firemní aplikace</span><span class="sxs-lookup"><span data-stu-id="3ef28-115">Assign a user or group tooan enterprise app</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-toocitrix-gotomeeting"></a><span data-ttu-id="3ef28-116">Důležité tipy pro přiřazení uživatelů tooCitrix GoToMeeting</span><span class="sxs-lookup"><span data-stu-id="3ef28-116">Important tips for assigning users tooCitrix GoToMeeting</span></span>

*   <span data-ttu-id="3ef28-117">Dále je doporučeno jednoho uživatele Azure AD je přiřazen tooCitrix GoToMeeting tootest hello zřizování konfigurace.</span><span class="sxs-lookup"><span data-stu-id="3ef28-117">It is recommended that a single Azure AD user is assigned tooCitrix GoToMeeting tootest hello provisioning configuration.</span></span> <span data-ttu-id="3ef28-118">Další uživatele nebo skupiny může být přiřazen později.</span><span class="sxs-lookup"><span data-stu-id="3ef28-118">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="3ef28-119">Při přiřazování uživatelů tooCitrix GoToMeeting, musíte vybrat platné uživatelské role.</span><span class="sxs-lookup"><span data-stu-id="3ef28-119">When assigning a user tooCitrix GoToMeeting, you must select a valid user role.</span></span> <span data-ttu-id="3ef28-120">role "Výchozí přístup" Hello nefunguje pro zřizování.</span><span class="sxs-lookup"><span data-stu-id="3ef28-120">hello "Default Access" role does not work for provisioning.</span></span>

## <a name="enable-automated-user-provisioning"></a><span data-ttu-id="3ef28-121">Povolit automatické zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="3ef28-121">Enable Automated User Provisioning</span></span>

<span data-ttu-id="3ef28-122">Tato část vás provede připojením GoToMeeting vaší služby Azure AD tooCitrix uživatelský účet zřizování rozhraní API a konfiguraci hello zřizování služby toocreate, aktualizovat a zakázat přiřazené uživatelské účty v systému Citrix GoToMeeting na základě uživatele a skupiny přiřazení ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3ef28-122">This section guides you through connecting your Azure AD tooCitrix GoToMeeting's user account provisioning API, and configuring hello provisioning service toocreate, update, and disable assigned user accounts in Citrix GoToMeeting based on user and group assignment in Azure AD.</span></span>

> [!TIP]
> <span data-ttu-id="3ef28-123">Můžete také zvolit tooenabled na základě SAML jednotné přihlašování pro Citrix GoToMeeting, hello pokynů uvedených v [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="3ef28-123">You may also choose tooenabled SAML-based Single Sign-On for Citrix GoToMeeting, following hello instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="3ef28-124">Jednotné přihlašování se dá nakonfigurovat nezávisle na automatické zřizování, i když tyto dvě funkce doplnění navzájem.</span><span class="sxs-lookup"><span data-stu-id="3ef28-124">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>

### <a name="tooconfigure-automatic-user-account-provisioning"></a><span data-ttu-id="3ef28-125">tooconfigure automatické uživatel účet zřizování:</span><span class="sxs-lookup"><span data-stu-id="3ef28-125">tooconfigure automatic user account provisioning:</span></span>

1. <span data-ttu-id="3ef28-126">V hello [portál Azure](https://portal.azure.com), procházet toohello **Azure Active Directory > podnikové aplikace > všechny aplikace** části.</span><span class="sxs-lookup"><span data-stu-id="3ef28-126">In hello [Azure portal](https://portal.azure.com), browse toohello **Azure Active Directory > Enterprise Apps > All applications** section.</span></span>

2. <span data-ttu-id="3ef28-127">Pokud jste již nakonfigurovali Citrix GoToMeeting pro jednotné přihlašování, vyhledejte instanci Citrix GoToMeeting pomocí hello vyhledávací pole.</span><span class="sxs-lookup"><span data-stu-id="3ef28-127">If you have already configured Citrix GoToMeeting for single sign-on, search for your instance of Citrix GoToMeeting using hello search field.</span></span> <span data-ttu-id="3ef28-128">Jinak vyberte možnost **přidat** a vyhledejte **Citrix GoToMeeting** v galerii aplikací hello.</span><span class="sxs-lookup"><span data-stu-id="3ef28-128">Otherwise, select **Add** and search for **Citrix GoToMeeting** in hello application gallery.</span></span> <span data-ttu-id="3ef28-129">Vyberte Citrix GoToMeeting z výsledků hledání hello a přidejte ji tooyour seznam aplikací.</span><span class="sxs-lookup"><span data-stu-id="3ef28-129">Select Citrix GoToMeeting from hello search results, and add it tooyour list of applications.</span></span>

3. <span data-ttu-id="3ef28-130">Vyberte instanci Citrix GoToMeeting a pak vyberte hello **zřizování** kartě.</span><span class="sxs-lookup"><span data-stu-id="3ef28-130">Select your instance of Citrix GoToMeeting, then select hello **Provisioning** tab.</span></span>

4. <span data-ttu-id="3ef28-131">Sada hello **zřizování** režimu příliš**automatické**.</span><span class="sxs-lookup"><span data-stu-id="3ef28-131">Set hello **Provisioning** Mode too**Automatic**.</span></span> 

    ![Zřizování](./media/active-directory-saas-citrixgotomeeting-provisioning-tutorial/provisioning.png)

5. <span data-ttu-id="3ef28-133">V části hello části přihlašovací údaje správce proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="3ef28-133">Under hello Admin Credentials section, perform hello following steps:</span></span>
   
    <span data-ttu-id="3ef28-134">a.</span><span class="sxs-lookup"><span data-stu-id="3ef28-134">a.</span></span> <span data-ttu-id="3ef28-135">V hello **uživatelské jméno správce GoToMeeting Citrix** textovému poli, zadejte jméno uživatele hello správce.</span><span class="sxs-lookup"><span data-stu-id="3ef28-135">In hello **Citrix GoToMeeting Admin User Name** textbox, type hello user name of an administrator.</span></span>

    <span data-ttu-id="3ef28-136">b.</span><span class="sxs-lookup"><span data-stu-id="3ef28-136">b.</span></span> <span data-ttu-id="3ef28-137">V hello **heslo správce systému Citrix GoToMeeting** textovému poli, heslo správce hello.</span><span class="sxs-lookup"><span data-stu-id="3ef28-137">In hello **Citrix GoToMeeting Admin Password** textbox, hello administrator's password.</span></span>

6. <span data-ttu-id="3ef28-138">V hello portálu Azure, klikněte na **Test připojení** tooensure Azure AD můžete připojit tooyour Citrix GoToMeeting aplikace.</span><span class="sxs-lookup"><span data-stu-id="3ef28-138">In hello Azure portal, click **Test Connection** tooensure Azure AD can connect tooyour Citrix GoToMeeting app.</span></span> <span data-ttu-id="3ef28-139">Pokud hello připojení nezdaří, zkontrolujte účtu Citrix GoToMeeting má oprávnění správce týmu a zkuste to hello **"Přihlašovací údaje správce"** krok opakujte.</span><span class="sxs-lookup"><span data-stu-id="3ef28-139">If hello connection fails, ensure your Citrix GoToMeeting account has Team Admin permissions and try hello **"Admin Credentials"** step again.</span></span>

7. <span data-ttu-id="3ef28-140">Zadejte hello e-mailovou adresu uživatele nebo skupiny, který by měly dostávat oznámení zřizování Chyba v hello **e-mailové oznámení** pole a zaškrtněte políčko hello.</span><span class="sxs-lookup"><span data-stu-id="3ef28-140">Enter hello email address of a person or group who should receive provisioning error notifications in hello **Notification Email** field, and check hello checkbox.</span></span>

8. <span data-ttu-id="3ef28-141">Klikněte na tlačítko **uložit.**</span><span class="sxs-lookup"><span data-stu-id="3ef28-141">Click **Save.**</span></span>

9. <span data-ttu-id="3ef28-142">V části hello části mapování, vyberte **synchronizaci uživatelů Azure Active Directory tooCitrix GoToMeeting.**</span><span class="sxs-lookup"><span data-stu-id="3ef28-142">Under hello Mappings section, select **Synchronize Azure Active Directory Users tooCitrix GoToMeeting.**</span></span>

10. <span data-ttu-id="3ef28-143">V hello **mapování atributů** , projděte si hello uživatelské atributy, které jsou synchronizované z Azure AD tooCitrix GoToMeeting.</span><span class="sxs-lookup"><span data-stu-id="3ef28-143">In hello **Attribute Mappings** section, review hello user attributes that are synchronized from Azure AD tooCitrix GoToMeeting.</span></span> <span data-ttu-id="3ef28-144">Hello atributy vybrán jako **párování** vlastnosti jsou použité toomatch hello uživatelské účty v systému Citrix GoToMeeting pro operace aktualizace.</span><span class="sxs-lookup"><span data-stu-id="3ef28-144">hello attributes selected as **Matching** properties are used toomatch hello user accounts in Citrix GoToMeeting for update operations.</span></span> <span data-ttu-id="3ef28-145">Vyberte toocommit tlačítko hello uložit změny.</span><span class="sxs-lookup"><span data-stu-id="3ef28-145">Select hello Save button toocommit any changes.</span></span>

11. <span data-ttu-id="3ef28-146">tooenable hello zřizování služby Azure AD pro Citrix GoToMeeting, změna hello **Stav zřizování** příliš**na** v části Nastavení hello</span><span class="sxs-lookup"><span data-stu-id="3ef28-146">tooenable hello Azure AD provisioning service for Citrix GoToMeeting, change hello **Provisioning Status** too**On** in hello Settings section</span></span>

12. <span data-ttu-id="3ef28-147">Klikněte na tlačítko **uložit.**</span><span class="sxs-lookup"><span data-stu-id="3ef28-147">Click **Save.**</span></span>

<span data-ttu-id="3ef28-148">Spustí počáteční synchronizaci hello všechny uživatele nebo skupiny přiřazené tooCitrix GoToMeeting v části Uživatelé a skupiny hello.</span><span class="sxs-lookup"><span data-stu-id="3ef28-148">It starts hello initial synchronization of any users and/or groups assigned tooCitrix GoToMeeting in hello Users and Groups section.</span></span> <span data-ttu-id="3ef28-149">počáteční synchronizace Hello trvá déle tooperform než následné synchronizace, ke kterým dochází přibližně každých 20 minut, dokud se službou hello.</span><span class="sxs-lookup"><span data-stu-id="3ef28-149">hello initial sync takes longer tooperform than subsequent syncs, which occur approximately every 20 minutes as long as hello service is running.</span></span> <span data-ttu-id="3ef28-150">Můžete použít hello **podrobnosti synchronizace** části toomonitor průběh a postupujte podle pokynů odkazy tooprovisioning aktivity sestavy, které popisují všechny akce prováděné hello zřizování služby ve vaší aplikaci Citrix GoToMeeting.</span><span class="sxs-lookup"><span data-stu-id="3ef28-150">You can use hello **Synchronization Details** section toomonitor progress and follow links tooprovisioning activity reports, which describe all actions performed by hello provisioning service on your Citrix GoToMeeting app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3ef28-151">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="3ef28-151">Additional resources</span></span>

* [<span data-ttu-id="3ef28-152">Správa uživatelů zřizování účtu pro podnikové aplikace</span><span class="sxs-lookup"><span data-stu-id="3ef28-152">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="3ef28-153">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="3ef28-153">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="3ef28-154">Konfigurovat jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="3ef28-154">Configure Single Sign-on</span></span>](active-directory-saas-citrix-gotomeeting-tutorial.md)


