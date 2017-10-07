---
title: "Kurz: Konfigurace síti na pracovišti ve službě Facebook pro zřizování uživatelů | Microsoft Docs"
description: "Zjistěte, jak tooautomatically zřídit a deaktivace zřízení uživatelských účtů ze služby Azure AD tooWorkplace ve službě Facebook."
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
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 33d294dbc8f441b29138408b3c9ca41f2141f8af
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configure-workplace-by-facebook-for-user-provisioning"></a><span data-ttu-id="545b3-103">Kurz: Konfigurace síti na pracovišti ve službě Facebook pro zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="545b3-103">Tutorial: Configure Workplace by Facebook for user provisioning</span></span>

<span data-ttu-id="545b3-104">Tento kurz ukazuje hello kroky nezbytné tooautomatically zřídit a deaktivace zřízení uživatelských účtů z tooWorkplace Azure Active Directory (Azure AD) ve službě Facebook.</span><span class="sxs-lookup"><span data-stu-id="545b3-104">This tutorial shows you hello steps necessary tooautomatically provision and de-provision user accounts from Azure Active Directory (Azure AD) tooWorkplace by Facebook.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="545b3-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="545b3-105">Prerequisites</span></span>

<span data-ttu-id="545b3-106">tooconfigure integrace Azure AD se na pracovišti ve službě Facebook, potřebujete následující hello:</span><span class="sxs-lookup"><span data-stu-id="545b3-106">tooconfigure Azure AD integration with Workplace by Facebook, you need hello following:</span></span>

- <span data-ttu-id="545b3-107">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="545b3-107">An Azure AD subscription</span></span>
- <span data-ttu-id="545b3-108">Předplatné povolené firemní síti pomocí sítě Facebook jednotné přihlašování (SSO)</span><span class="sxs-lookup"><span data-stu-id="545b3-108">A Workplace by Facebook single sign-on (SSO) enabled subscription</span></span>

<span data-ttu-id="545b3-109">tootest hello kroky v tomto kurzu, postupujte podle následujících doporučení:</span><span class="sxs-lookup"><span data-stu-id="545b3-109">tootest hello steps in this tutorial, follow these recommendations:</span></span>

- <span data-ttu-id="545b3-110">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="545b3-110">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="545b3-111">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat [nabídka zkušební verze jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="545b3-111">If you don't have an Azure AD trial environment, you can get a [one-month trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="assign-users-tooworkplace-by-facebook"></a><span data-ttu-id="545b3-112">Přiřazení uživatelů tooWorkplace ve službě Facebook.</span><span class="sxs-lookup"><span data-stu-id="545b3-112">Assign users tooWorkplace by Facebook</span></span>

<span data-ttu-id="545b3-113">Azure AD používá koncept názvem "přiřazení" toodetermine uživatelů, kteří obdrželi přístup tooselected aplikace.</span><span class="sxs-lookup"><span data-stu-id="545b3-113">Azure AD uses a concept called "assignments" toodetermine which users should receive access tooselected apps.</span></span> <span data-ttu-id="545b3-114">V kontextu hello zřizování účtu automatické uživatele jsou synchronizovány pouze hello uživatelů a skupin, které byly přiřazeny tooan aplikace ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="545b3-114">In hello context of automatic user account provisioning, only hello users and groups that have been assigned tooan application in Azure AD are synchronized.</span></span>

<span data-ttu-id="545b3-115">Než nakonfigurujete a povolíte hello zřizování služby, rozhodněte, jaké uživatelů a skupin ve službě Azure AD představují hello uživatelů, kteří potřebují přístup k síti na pracovišti tooyour aplikace Facebook.</span><span class="sxs-lookup"><span data-stu-id="545b3-115">Before configuring and enabling hello provisioning service, decide what users and groups in Azure AD represent hello users who need access tooyour Workplace by Facebook app.</span></span> <span data-ttu-id="545b3-116">Pak můžete přiřadit tyto uživatele tooyour síti na pracovišti Facebook aplikací pomocí následujících hello pokyny v [přiřadit uživatele nebo skupinu tooan firemní aplikace](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="545b3-116">You can then assign these users tooyour Workplace by Facebook app by following hello instructions in [Assign a user or group tooan enterprise app](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal).</span></span>

>[!IMPORTANT]
>*   <span data-ttu-id="545b3-117">Test hello zřizování konfigurace přiřazením jedné tooWorkplace uživatele Azure AD ve službě Facebook.</span><span class="sxs-lookup"><span data-stu-id="545b3-117">Test hello provisioning configuration by assigning a single Azure AD user tooWorkplace by Facebook.</span></span> <span data-ttu-id="545b3-118">Přiřaďte dalších uživatelů a skupin později.</span><span class="sxs-lookup"><span data-stu-id="545b3-118">Assign additional users and groups later.</span></span>
>*   <span data-ttu-id="545b3-119">Když přiřadíte uživatele tooWorkplace ve službě Facebook, musíte vybrat platné uživatelské role.</span><span class="sxs-lookup"><span data-stu-id="545b3-119">When you assign a user tooWorkplace by Facebook, you must select a valid user role.</span></span> <span data-ttu-id="545b3-120">role Hello výchozí přístup nefunguje pro zřizování.</span><span class="sxs-lookup"><span data-stu-id="545b3-120">hello Default Access role does not work for provisioning.</span></span>

## <a name="enable-automated-user-provisioning"></a><span data-ttu-id="545b3-121">Povolit zřizování automatizované uživatelů</span><span class="sxs-lookup"><span data-stu-id="545b3-121">Enable automated user provisioning</span></span>

<span data-ttu-id="545b3-122">Tato část vás provede připojením účtu uživatele Azure AD toohello zřizování API pracovního místa ve službě Facebook.</span><span class="sxs-lookup"><span data-stu-id="545b3-122">This section guides you through connecting your Azure AD toohello user account provisioning API of Workplace by Facebook.</span></span> <span data-ttu-id="545b3-123">Také zjistíte, jak tooconfigure hello zřizování služby toocreate, aktualizovat a zakázat přiřazené uživatelské účty v síti na pracovišti ve službě Facebook.</span><span class="sxs-lookup"><span data-stu-id="545b3-123">You also learn how tooconfigure hello provisioning service toocreate, update, and disable assigned user accounts in Workplace by Facebook.</span></span> <span data-ttu-id="545b3-124">To je založené na uživatele a přiřazení skupiny ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="545b3-124">This is based on user and group assignment in Azure AD.</span></span>

>[!Tip]
><span data-ttu-id="545b3-125">Můžete také tooenabled na základě SAML jednotného přihlašování pro pracoviště ve službě Facebook, pomocí pokynů hello součástí hello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="545b3-125">You can also choose tooenabled SAML-based SSO for Workplace by Facebook, by following hello instructions provided in hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="545b3-126">Jednotné přihlašování se dá nakonfigurovat nezávisle na automatické zřizování, i když tyto dvě funkce vzájemně doplňují.</span><span class="sxs-lookup"><span data-stu-id="545b3-126">SSO can be configured independently of automatic provisioning, though these two features complement each other.</span></span>

### <a name="configure-user-account-provisioning-tooworkplace-by-facebook-in-azure-ad"></a><span data-ttu-id="545b3-127">Konfigurace uživatelského účtu ve službě Azure AD zřizování tooWorkplace ve službě Facebook.</span><span class="sxs-lookup"><span data-stu-id="545b3-127">Configure user account provisioning tooWorkplace by Facebook in Azure AD</span></span>

<span data-ttu-id="545b3-128">Azure AD podporuje možnost tooautomatically hello synchronizovat Podrobnosti účtu hello přiřadit tooWorkplace uživatelů ve službě Facebook.</span><span class="sxs-lookup"><span data-stu-id="545b3-128">Azure AD supports hello ability tooautomatically synchronize hello account details of assigned users tooWorkplace by Facebook.</span></span> <span data-ttu-id="545b3-129">Toto automatické synchronizace umožňuje síti na pracovišti Facebook tooget hello data tooauthorize uživatelů pro přístup, je nutné před je prvním pokusem o toosign v pro hello.</span><span class="sxs-lookup"><span data-stu-id="545b3-129">This automatic synchronization enables Workplace by Facebook tooget hello data it needs tooauthorize users for access, before them attempting toosign in for hello first time.</span></span> <span data-ttu-id="545b3-130">Také zrušte technologii uživatelům v síti na pracovišti ve službě Facebook při odvolání přístupu ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="545b3-130">It also de-provisions users from Workplace by Facebook when access has been revoked in Azure AD.</span></span>

1. <span data-ttu-id="545b3-131">V hello [portál Azure](https://portal.azure.com), vyberte **Azure Active Directory** > **podnikové aplikace** > **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="545b3-131">In hello [Azure portal](https://portal.azure.com), select **Azure Active Directory** > **Enterprise Apps** > **All applications**.</span></span>

2. <span data-ttu-id="545b3-132">Pokud jste již nakonfigurovali síti na pracovišti ve službě Facebook pro jednotné přihlašování, vyhledejte pomocí pole hledání text hello instanci síti na pracovišti ve službě Facebook.</span><span class="sxs-lookup"><span data-stu-id="545b3-132">If you have already configured Workplace by Facebook for SSO, search for your instance of Workplace by Facebook by using hello search field.</span></span> <span data-ttu-id="545b3-133">Jinak vyberte možnost **přidat** a vyhledejte **síti na pracovišti ve službě Facebook** v galerii aplikací hello.</span><span class="sxs-lookup"><span data-stu-id="545b3-133">Otherwise, select **Add** and search for **Workplace by Facebook** in hello application gallery.</span></span> <span data-ttu-id="545b3-134">Vyberte **síti na pracovišti ve službě Facebook** z hello výsledky hledání a přidejte ji tooyour seznam aplikací.</span><span class="sxs-lookup"><span data-stu-id="545b3-134">Select **Workplace by Facebook** from hello search results, and add it tooyour list of applications.</span></span>

3. <span data-ttu-id="545b3-135">Vyberte instanci síti na pracovišti ve službě Facebook a pak vyberte hello **zřizování** kartě.</span><span class="sxs-lookup"><span data-stu-id="545b3-135">Select your instance of Workplace by Facebook, and then select hello **Provisioning** tab.</span></span>

4. <span data-ttu-id="545b3-136">Nastavit **režimu zřizování** příliš**automatické**.</span><span class="sxs-lookup"><span data-stu-id="545b3-136">Set **Provisioning Mode** too**Automatic**.</span></span> 

    ![Snímek obrazovky pracovního místa ve službě Facebook možnosti zřizování](./media/active-directory-saas-facebook-at-work-provisioning-tutorial/provisioning.png)

5. <span data-ttu-id="545b3-138">V části hello **přihlašovací údaje správce** zadejte hello **tajný klíč tokenu** a hello **URL klienta** ze svého pracoviště správcem sítě Facebook.</span><span class="sxs-lookup"><span data-stu-id="545b3-138">Under hello **Admin Credentials** section, enter hello **Secret Token** and hello **Tenant URL** of your Workplace by Facebook administrator.</span></span>

6. <span data-ttu-id="545b3-139">V hello portálu Azure, vyberte **Test připojení** tooensure Azure AD můžete připojit tooyour síti na pracovišti aplikace Facebook.</span><span class="sxs-lookup"><span data-stu-id="545b3-139">In hello Azure portal, select **Test Connection** tooensure Azure AD can connect tooyour Workplace by Facebook app.</span></span> <span data-ttu-id="545b3-140">Pokud hello připojení nezdaří, zkontrolujte, zda vaše pracoviště pomocí účtu sítě Facebook oprávnění správce týmu.</span><span class="sxs-lookup"><span data-stu-id="545b3-140">If hello connection fails, ensure that your Workplace by Facebook account has Team Admin permissions.</span></span>

7. <span data-ttu-id="545b3-141">Zadejte hello e-mailovou adresu uživatele nebo skupiny, který by měly dostávat oznámení zřizování Chyba v hello **e-mailové oznámení** pole a zaškrtněte políčko hello.</span><span class="sxs-lookup"><span data-stu-id="545b3-141">Enter hello email address of a person or group who should receive provisioning error notifications in hello **Notification Email** field, and check hello check box.</span></span>

8. <span data-ttu-id="545b3-142">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="545b3-142">Select **Save**.</span></span>

9. <span data-ttu-id="545b3-143">V části hello části mapování, vyberte **tooWorkplace synchronizaci uživatelů Azure Active Directory ve službě Facebook**.</span><span class="sxs-lookup"><span data-stu-id="545b3-143">Under hello Mappings section, select **Synchronize Azure Active Directory Users tooWorkplace by Facebook**.</span></span>

10. <span data-ttu-id="545b3-144">V hello **mapování atributů** , projděte si hello uživatelské atributy, které jsou synchronizované z Azure AD tooWorkplace ve službě Facebook.</span><span class="sxs-lookup"><span data-stu-id="545b3-144">In hello **Attribute Mappings** section, review hello user attributes that are synchronized from Azure AD tooWorkplace by Facebook.</span></span> <span data-ttu-id="545b3-145">Hello atributy vybrán jako **párování** vlastnosti jsou použité toomatch hello uživatelské účty v síti na pracovišti ve službě Facebook pro operace aktualizace.</span><span class="sxs-lookup"><span data-stu-id="545b3-145">hello attributes selected as **Matching** properties are used toomatch hello user accounts in Workplace by Facebook for update operations.</span></span> <span data-ttu-id="545b3-146">Vyberte všechny změny toocommit **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="545b3-146">toocommit any changes, select **Save**.</span></span>

11. <span data-ttu-id="545b3-147">tooenable hello zřizování služby Azure AD pro pracoviště ve službě Facebook, v hello **nastavení** změňte hello **Stav zřizování** příliš**na**.</span><span class="sxs-lookup"><span data-stu-id="545b3-147">tooenable hello Azure AD provisioning service for Workplace by Facebook, in hello **Settings** section, change hello **Provisioning Status** too**On**.</span></span>

12. <span data-ttu-id="545b3-148">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="545b3-148">Select **Save**.</span></span>

<span data-ttu-id="545b3-149">Další informace o tom, tooconfigure automatické zřizování, najdete v části [hello Facebook dokumentaci](https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers).</span><span class="sxs-lookup"><span data-stu-id="545b3-149">For more information on how tooconfigure automatic provisioning, see [hello Facebook documentation](https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers).</span></span>

<span data-ttu-id="545b3-150">Nyní můžete vytvořit testovací účet.</span><span class="sxs-lookup"><span data-stu-id="545b3-150">You can now create a test account.</span></span> <span data-ttu-id="545b3-151">Počkejte, až minut too20 tooverify, který hello účet byl synchronizován tooWorkplace ve službě Facebook.</span><span class="sxs-lookup"><span data-stu-id="545b3-151">Wait for up too20 minutes tooverify that hello account has been synchronized tooWorkplace by Facebook.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="545b3-152">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="545b3-152">Additional resources</span></span>

* [<span data-ttu-id="545b3-153">Správa uživatelů zřizování účtu pro podnikové aplikace</span><span class="sxs-lookup"><span data-stu-id="545b3-153">Managing user account provisioning for enterprise apps</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="545b3-154">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="545b3-154">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="545b3-155">Konfigurovat jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="545b3-155">Configure single sign-on</span></span>](active-directory-saas-facebook-at-work-tutorial.md)

