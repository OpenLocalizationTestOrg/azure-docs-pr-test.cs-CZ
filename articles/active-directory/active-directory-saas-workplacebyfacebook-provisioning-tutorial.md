---
title: "Kurz: Azure Active Directory integrace s síti na pracovišti ve službě Facebook. | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a síti na pracovišti ve službě Facebook."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 6341e67e-8ce6-42dc-a4ea-7295904a53ef
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 551ec353a5ec1da936373587688c299a6f4acca7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-workplace-by-facebook-for-user-provisioning"></a><span data-ttu-id="82f36-103">Kurz: Konfigurace síti na pracovišti ve službě Facebook pro zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="82f36-103">Tutorial: Configuring Workplace by Facebook for User Provisioning</span></span>

<span data-ttu-id="82f36-104">cílem Hello tohoto kurzu je tooshow hello kroky nutné tooperform v síti na pracovišti podle Facebook a Azure AD tooautomatically zřídit a deaktivace zřízení uživatelských účtů z Azure AD tooWorkplace ve službě Facebook.</span><span class="sxs-lookup"><span data-stu-id="82f36-104">hello objective of this tutorial is tooshow you hello steps you need tooperform in Workplace by Facebook and Azure AD tooautomatically provision and de-provision user accounts from Azure AD tooWorkplace by Facebook.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="82f36-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="82f36-105">Prerequisites</span></span>

<span data-ttu-id="82f36-106">tooconfigure integrace Azure AD se na pracovišti ve službě Facebook, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="82f36-106">tooconfigure Azure AD integration with Workplace by Facebook, you need hello following items:</span></span>

- <span data-ttu-id="82f36-107">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="82f36-107">An Azure AD subscription</span></span>
- <span data-ttu-id="82f36-108">Firemní síti pomocí sítě Facebook jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="82f36-108">A Workplace by Facebook single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="82f36-109">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="82f36-109">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="82f36-110">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="82f36-110">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="82f36-111">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="82f36-111">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="82f36-112">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="82f36-112">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="assigning-users-tooworkplace-by-facebook"></a><span data-ttu-id="82f36-113">Přiřazení uživatelů tooWorkplace ve službě Facebook.</span><span class="sxs-lookup"><span data-stu-id="82f36-113">Assigning users tooWorkplace by Facebook</span></span>

<span data-ttu-id="82f36-114">Azure Active Directory používá koncept názvem "přiřazení" toodetermine uživatelů, kteří obdrželi přístup tooselected aplikace.</span><span class="sxs-lookup"><span data-stu-id="82f36-114">Azure Active Directory uses a concept called "assignments" toodetermine which users should receive access tooselected apps.</span></span> <span data-ttu-id="82f36-115">V kontextu hello zřizování účtu automatické uživatele se synchronizují pouze hello uživatelů a skupin, které byly "přiřazeny" tooan aplikace ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="82f36-115">In hello context of automatic user account provisioning, only hello users and groups that have been "assigned" tooan application in Azure AD is synchronized.</span></span>

<span data-ttu-id="82f36-116">Než nakonfigurujete a povolíte hello zřizování služby, je nutné toodecide, jaké uživatelů nebo skupin ve službě Azure AD představují hello uživatele, kteří potřebují přístup k síti na pracovišti tooyour aplikace Facebook.</span><span class="sxs-lookup"><span data-stu-id="82f36-116">Before configuring and enabling hello provisioning service, you need toodecide what users and/or groups in Azure AD represent hello users who need access tooyour Workplace by Facebook app.</span></span> <span data-ttu-id="82f36-117">Jakmile se rozhodli, můžete přiřadit tyto uživatele tooyour síti na pracovišti aplikace Facebook podle pokynů hello tady:</span><span class="sxs-lookup"><span data-stu-id="82f36-117">Once decided, you can assign these users tooyour Workplace by Facebook app by following hello instructions here:</span></span>

[<span data-ttu-id="82f36-118">Přiřadit uživatele nebo skupinu tooan firemní aplikace</span><span class="sxs-lookup"><span data-stu-id="82f36-118">Assign a user or group tooan enterprise app</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-tooworkplace-by-facebook"></a><span data-ttu-id="82f36-119">Důležité tipy pro přiřazení tooWorkplace uživatelů ve službě Facebook.</span><span class="sxs-lookup"><span data-stu-id="82f36-119">Important tips for assigning users tooWorkplace by Facebook</span></span>

*   <span data-ttu-id="82f36-120">Dále je doporučeno jednoho uživatele Azure AD je přiřazena tooWorkplace službou Facebook tootest hello zřizování konfigurace.</span><span class="sxs-lookup"><span data-stu-id="82f36-120">It is recommended that a single Azure AD user is assigned tooWorkplace by Facebook tootest hello provisioning configuration.</span></span> <span data-ttu-id="82f36-121">Další uživatele nebo skupiny může být přiřazen později.</span><span class="sxs-lookup"><span data-stu-id="82f36-121">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="82f36-122">Při přiřazování tooWorkplace uživatele ve službě Facebook, musíte vybrat platné uživatelské role.</span><span class="sxs-lookup"><span data-stu-id="82f36-122">When assigning a user tooWorkplace by Facebook, you must select a valid user role.</span></span> <span data-ttu-id="82f36-123">role "Výchozí přístup" Hello nefunguje pro zřizování.</span><span class="sxs-lookup"><span data-stu-id="82f36-123">hello "Default Access" role does not work for provisioning.</span></span>

## <a name="enable-user-provisioning"></a><span data-ttu-id="82f36-124">Povolit zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="82f36-124">Enable User Provisioning</span></span>

<span data-ttu-id="82f36-125">Tato část vás provede připojení tooWorkplace vaší služby Azure AD, uživatelský účet na Facebooku pro zřizování rozhraní API a konfiguraci hello zřizování služby toocreate, aktualizovat a zakázat přiřazené uživatelské účty v síti na pracovišti ve službě Facebook na základě uživatele a skupiny přiřazení ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="82f36-125">This section guides you through connecting your Azure AD tooWorkplace by Facebook's user account provisioning API, and configuring hello provisioning service toocreate, update, and disable assigned user accounts in Workplace by Facebook based on user and group assignment in Azure AD.</span></span>

>[!Tip]
><span data-ttu-id="82f36-126">Můžete také zvolit tooenabled na základě SAML jednotné přihlašování pro pracoviště podle Facebook, hello pokynů uvedených v [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="82f36-126">You may also choose tooenabled SAML-based Single Sign-On for Workplace by Facebook, following hello instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="82f36-127">Jednotné přihlašování se dá nakonfigurovat nezávisle na automatické zřizování, i když tyto dvě funkce doplnění navzájem.</span><span class="sxs-lookup"><span data-stu-id="82f36-127">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>

### <a name="tooconfigure-user-account-provisioning-tooworkplace-by-facebook-in-azure-ad"></a><span data-ttu-id="82f36-128">uživatelský účet tooconfigure zřizování tooWorkplace ve službě Facebook ve službě Azure AD:</span><span class="sxs-lookup"><span data-stu-id="82f36-128">tooconfigure user account provisioning tooWorkplace by Facebook in Azure AD:</span></span>

<span data-ttu-id="82f36-129">Hello cílem této části je toooutline jak tooenable zřizování služby Active Directory uživatelské účty tooWorkplace ve službě Facebook.</span><span class="sxs-lookup"><span data-stu-id="82f36-129">hello objective of this section is toooutline how tooenable provisioning of Active Directory user accounts tooWorkplace by Facebook.</span></span>

<span data-ttu-id="82f36-130">Azure AD podporuje možnost tooautomatically hello synchronizovat Podrobnosti účtu hello přiřadit tooWorkplace uživatelů ve službě Facebook.</span><span class="sxs-lookup"><span data-stu-id="82f36-130">Azure AD supports hello ability tooautomatically synchronize hello account details of assigned users tooWorkplace by Facebook.</span></span> <span data-ttu-id="82f36-131">Toto automatické synchronizace umožňuje síti na pracovišti Facebook tooget hello data tooauthorize uživatelů pro přístup, je nutné před je prvním pokusem o toosign v pro hello.</span><span class="sxs-lookup"><span data-stu-id="82f36-131">This automatic synchronization enables Workplace by Facebook tooget hello data it needs tooauthorize users for access, before them attempting toosign in for hello first time.</span></span> <span data-ttu-id="82f36-132">Také zrušte technologii uživatelům v síti na pracovišti ve službě Facebook při odvolání přístupu ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="82f36-132">It also de-provisions users from Workplace by Facebook when access has been revoked in Azure AD.</span></span>

1. <span data-ttu-id="82f36-133">V hello [portál Azure](https://portal.azure.com), procházet toohello **Azure Active Directory** > **podnikové aplikace** > **všechny aplikace** části.</span><span class="sxs-lookup"><span data-stu-id="82f36-133">In hello [Azure portal](https://portal.azure.com), browse toohello **Azure Active Directory** > **Enterprise Apps** > **All applications** section.</span></span>

2. <span data-ttu-id="82f36-134">Pokud jste již nakonfigurovali síti na pracovišti ve službě Facebook pro jednotné přihlašování, vyhledejte instanci síti na pracovišti podle Facebook pomocí hello vyhledávací pole.</span><span class="sxs-lookup"><span data-stu-id="82f36-134">If you have already configured Workplace by Facebook for single sign-on, search for your instance of Workplace by Facebook using hello search field.</span></span> <span data-ttu-id="82f36-135">Jinak vyberte možnost **přidat** a vyhledejte **síti na pracovišti ve službě Facebook** v galerii aplikací hello.</span><span class="sxs-lookup"><span data-stu-id="82f36-135">Otherwise, select **Add** and search for **Workplace by Facebook** in hello application gallery.</span></span> <span data-ttu-id="82f36-136">Vyberte síti na pracovišti ve službě Facebook z výsledků hledání hello a přidejte ji tooyour seznam aplikací.</span><span class="sxs-lookup"><span data-stu-id="82f36-136">Select Workplace by Facebook from hello search results, and add it tooyour list of applications.</span></span>

3. <span data-ttu-id="82f36-137">Vyberte instanci síti na pracovišti ve službě Facebook a pak vyberte hello **zřizování** kartě.</span><span class="sxs-lookup"><span data-stu-id="82f36-137">Select your instance of Workplace by Facebook, then select hello **Provisioning** tab.</span></span>

4. <span data-ttu-id="82f36-138">Sada hello **režimu zřizování** příliš**automatické**.</span><span class="sxs-lookup"><span data-stu-id="82f36-138">Set hello **Provisioning Mode** too**Automatic**.</span></span> 

    ![Zřizování](./media/active-directory-saas-workplacebyfacebook-provisioning-tutorial/provisioning.png)

5. <span data-ttu-id="82f36-140">V části hello **přihlašovací údaje správce** části zadejte hello tajný klíč tokenu a hello URL klienta ze svého pracoviště správcem sítě Facebook.</span><span class="sxs-lookup"><span data-stu-id="82f36-140">Under hello **Admin Credentials** section, enter hello Secret Token and hello Tenant URL of your Workplace by Facebook administrator.</span></span>

6. <span data-ttu-id="82f36-141">V hello portálu Azure, klikněte na **Test připojení** tooensure Azure AD můžete připojit tooyour síti na pracovišti aplikace Facebook.</span><span class="sxs-lookup"><span data-stu-id="82f36-141">In hello Azure portal, click **Test Connection** tooensure Azure AD can connect tooyour Workplace by Facebook app.</span></span> <span data-ttu-id="82f36-142">Pokud hello připojení selže, zajistěte, aby že vaše pracoviště Facebook účet má oprávnění správce týmu.</span><span class="sxs-lookup"><span data-stu-id="82f36-142">If hello connection fails, ensure your Workplace by Facebook account has Team Admin permissions.</span></span>

7. <span data-ttu-id="82f36-143">Zadejte hello e-mailovou adresu uživatele nebo skupiny, který by měly dostávat oznámení zřizování Chyba v hello **e-mailové oznámení** pole a zaškrtněte políčko hello.</span><span class="sxs-lookup"><span data-stu-id="82f36-143">Enter hello email address of a person or group who should receive provisioning error notifications in hello **Notification Email** field, and check hello checkbox.</span></span>

8. <span data-ttu-id="82f36-144">Klikněte na tlačítko **uložit.**</span><span class="sxs-lookup"><span data-stu-id="82f36-144">Click **Save.**</span></span>

9. <span data-ttu-id="82f36-145">V části hello části mapování, vyberte **tooWorkplace synchronizaci uživatelů Azure Active Directory ve službě Facebook.**</span><span class="sxs-lookup"><span data-stu-id="82f36-145">Under hello Mappings section, select **Synchronize Azure Active Directory Users tooWorkplace by Facebook.**</span></span>

10. <span data-ttu-id="82f36-146">V hello **mapování atributů** , projděte si hello uživatelské atributy, které jsou synchronizované z Azure AD tooWorkplace ve službě Facebook.</span><span class="sxs-lookup"><span data-stu-id="82f36-146">In hello **Attribute Mappings** section, review hello user attributes that are synchronized from Azure AD tooWorkplace by Facebook.</span></span> <span data-ttu-id="82f36-147">Hello atributy vybrán jako **párování** vlastnosti jsou použité toomatch hello uživatelské účty v síti na pracovišti ve službě Facebook pro operace aktualizace.</span><span class="sxs-lookup"><span data-stu-id="82f36-147">hello attributes selected as **Matching** properties are used toomatch hello user accounts in Workplace by Facebook for update operations.</span></span> <span data-ttu-id="82f36-148">Vyberte toocommit tlačítko hello uložit změny.</span><span class="sxs-lookup"><span data-stu-id="82f36-148">Select hello Save button toocommit any changes.</span></span>

11. <span data-ttu-id="82f36-149">tooenable hello zřizování služby Azure AD pro pracoviště podle Facebook, změna hello **Stav zřizování** příliš**na** v hello **nastavení** části</span><span class="sxs-lookup"><span data-stu-id="82f36-149">tooenable hello Azure AD provisioning service for Workplace by Facebook, change hello **Provisioning Status** too**On** in hello **Settings** section</span></span>

12. <span data-ttu-id="82f36-150">Klikněte na tlačítko **uložit.**</span><span class="sxs-lookup"><span data-stu-id="82f36-150">Click **Save.**</span></span>

<span data-ttu-id="82f36-151">Další informace o tom, tooconfigure automatické zřizování, najdete v části [https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers](https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers)</span><span class="sxs-lookup"><span data-stu-id="82f36-151">For more information on how tooconfigure automatic provisioning, see [https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers](https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers)</span></span>

<span data-ttu-id="82f36-152">Nyní můžete vytvořit testovací účet.</span><span class="sxs-lookup"><span data-stu-id="82f36-152">You can now create a test account.</span></span> <span data-ttu-id="82f36-153">Počkejte, až minut too20 tooverify, který hello účet byl synchronizován tooWorkplace ve službě Facebook.</span><span class="sxs-lookup"><span data-stu-id="82f36-153">Wait for up too20 minutes tooverify that hello account has been synchronized tooWorkplace by Facebook.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="82f36-154">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="82f36-154">Additional resources</span></span>

* [<span data-ttu-id="82f36-155">Správa uživatelů zřizování účtu pro podnikové aplikace</span><span class="sxs-lookup"><span data-stu-id="82f36-155">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="82f36-156">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="82f36-156">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="82f36-157">Konfigurovat jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="82f36-157">Configure Single Sign-on</span></span>](active-directory-saas-workplacebyfacebook-tutorial.md)

