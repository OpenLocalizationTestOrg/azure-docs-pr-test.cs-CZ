---
title: 'Kurz: Azure Active Directory integrace s Salesforce | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a služby Salesforce."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 49384b8b-3836-4eb1-b438-1c46bb9baf6f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: a916be8dbf0b4c6173cda873936a53cd1f3ff12b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-salesforce-for-automatic-user-provisioning"></a><span data-ttu-id="a72a1-103">Kurz: Konfigurace služby Salesforce pro zřizování automatické uživatelů</span><span class="sxs-lookup"><span data-stu-id="a72a1-103">Tutorial: Configuring Salesforce for Automatic User Provisioning</span></span>

<span data-ttu-id="a72a1-104">cílem Hello tohoto kurzu je tooshow hello kroky požadované tooperform v Salesforce a Azure AD tooautomatically zřídit a deaktivace zřízení uživatelských účtů z tooSalesforce Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a72a1-104">hello objective of this tutorial is tooshow hello steps required tooperform in Salesforce and Azure AD tooautomatically provision and de-provision user accounts from Azure AD tooSalesforce.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a72a1-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a72a1-105">Prerequisites</span></span>

<span data-ttu-id="a72a1-106">Hello scénáři uvedeném v tomto kurzu se předpokládá, že už máte hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="a72a1-106">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

*   <span data-ttu-id="a72a1-107">Klienta služby Azure Active directory.</span><span class="sxs-lookup"><span data-stu-id="a72a1-107">An Azure Active directory tenant.</span></span>
*   <span data-ttu-id="a72a1-108">Pro pracovní nebo Salesforce pro vzdělávací organizace musí mít platný klient pro služby Salesforce.</span><span class="sxs-lookup"><span data-stu-id="a72a1-108">You must have a valid tenant for Salesforce for Work or Salesforce for Education.</span></span> <span data-ttu-id="a72a1-109">Bezplatný zkušební účet můžete použít buď služby.</span><span class="sxs-lookup"><span data-stu-id="a72a1-109">You may use a free trial     account for either service.</span></span>
*   <span data-ttu-id="a72a1-110">Uživatelský účet v Salesforce s oprávněními správce týmu.</span><span class="sxs-lookup"><span data-stu-id="a72a1-110">A user account in Salesforce with Team Admin permissions.</span></span>

## <a name="assigning-users-toosalesforce"></a><span data-ttu-id="a72a1-111">Přiřazení uživatelů tooSalesforce</span><span class="sxs-lookup"><span data-stu-id="a72a1-111">Assigning users tooSalesforce</span></span>

<span data-ttu-id="a72a1-112">Azure Active Directory používá koncept názvem "přiřazení" toodetermine uživatelů, kteří obdrželi přístup tooselected aplikace.</span><span class="sxs-lookup"><span data-stu-id="a72a1-112">Azure Active Directory uses a concept called "assignments" toodetermine which users should receive access tooselected apps.</span></span> <span data-ttu-id="a72a1-113">V kontextu hello zřizování účtu automatické uživatele se synchronizují pouze hello uživatelů a skupin, které byly "přiřazeny" tooan aplikace ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a72a1-113">In hello context of automatic user account provisioning, only hello users and groups that have been "assigned" tooan application in Azure AD is synchronized.</span></span>

<span data-ttu-id="a72a1-114">Než nakonfigurujete a povolíte hello zřizování služby, je nutné toodecide, jaké uživatelů nebo skupin v Azure AD představují hello uživatelé, kteří potřebují přístup k aplikaci Salesforce tooyour.</span><span class="sxs-lookup"><span data-stu-id="a72a1-114">Before configuring and enabling hello provisioning service, you need toodecide what users and/or groups in Azure AD represent hello users who need access tooyour Salesforce app.</span></span> <span data-ttu-id="a72a1-115">Jakmile se rozhodli, můžete přiřadit aplikaci Salesforce tooyour těchto uživatelů podle pokynů hello tady:</span><span class="sxs-lookup"><span data-stu-id="a72a1-115">Once decided, you can assign these users tooyour Salesforce app by following hello instructions here:</span></span>

[<span data-ttu-id="a72a1-116">Přiřadit uživatele nebo skupinu tooan firemní aplikace</span><span class="sxs-lookup"><span data-stu-id="a72a1-116">Assign a user or group tooan enterprise app</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-toosalesforce"></a><span data-ttu-id="a72a1-117">Důležité tipy pro přiřazení uživatelů tooSalesforce</span><span class="sxs-lookup"><span data-stu-id="a72a1-117">Important tips for assigning users tooSalesforce</span></span>

*   <span data-ttu-id="a72a1-118">Dále je doporučeno jednoho uživatele Azure AD je přiřazen hello tootest tooSalesforce zřizování konfigurace.</span><span class="sxs-lookup"><span data-stu-id="a72a1-118">It is recommended that a single Azure AD user is assigned tooSalesforce tootest hello provisioning configuration.</span></span> <span data-ttu-id="a72a1-119">Další uživatele nebo skupiny může být přiřazen později.</span><span class="sxs-lookup"><span data-stu-id="a72a1-119">Additional users and/or groups may be assigned later.</span></span>

*  <span data-ttu-id="a72a1-120">Při přiřazování tooSalesforce uživatele, musíte vybrat platné uživatelské role.</span><span class="sxs-lookup"><span data-stu-id="a72a1-120">When assigning a user tooSalesforce, you must select a valid user role.</span></span> <span data-ttu-id="a72a1-121">role "Výchozí přístup" Hello nefunguje pro zřizování</span><span class="sxs-lookup"><span data-stu-id="a72a1-121">hello "Default Access" role does not work for provisioning</span></span>

    > [!NOTE]
    > <span data-ttu-id="a72a1-122">Tato aplikace importuje vlastní role ze služby Salesforce jako součást hello zřizování, který hello může zákazník tooselect při přiřazování uživatelů</span><span class="sxs-lookup"><span data-stu-id="a72a1-122">This app imports custom roles from Salesforce as part of hello provisioning process, which hello customer may want tooselect when assigning users</span></span>

## <a name="enable-automated-user-provisioning"></a><span data-ttu-id="a72a1-123">Povolit automatické zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="a72a1-123">Enable Automated User Provisioning</span></span>

<span data-ttu-id="a72a1-124">Tato část vás provede připojením vaší služby Azure AD tooSalesforce uživatelský účet zřizování rozhraní API a konfiguraci hello zřizování služby toocreate, aktualizovat a zakázat přiřazené uživatelské účty v Salesforce podle přiřazení uživatelů a skupin ve službě Azure AD .</span><span class="sxs-lookup"><span data-stu-id="a72a1-124">This section guides you through connecting your Azure AD tooSalesforce's user account provisioning API, and configuring hello provisioning service toocreate, update, and disable assigned user accounts in Salesforce based on user and group assignment in Azure AD.</span></span>

>[!Tip]
><span data-ttu-id="a72a1-125">Můžete také zvolit tooenabled na základě SAML jednotné přihlašování pro služby Salesforce, hello pokynů uvedených v [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="a72a1-125">You may also choose tooenabled SAML-based Single Sign-On for Salesforce, following hello instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="a72a1-126">Jednotné přihlašování se dá nakonfigurovat nezávisle na automatické zřizování, i když tyto dvě funkce doplnění navzájem.</span><span class="sxs-lookup"><span data-stu-id="a72a1-126">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>

### <a name="tooconfigure-automatic-user-account-provisioning"></a><span data-ttu-id="a72a1-127">tooconfigure automatické uživatel účet zřizování:</span><span class="sxs-lookup"><span data-stu-id="a72a1-127">tooconfigure automatic user account provisioning:</span></span>

<span data-ttu-id="a72a1-128">Hello cílem této části je toooutline jak tooenable zřizování uživatelů služby Active Directory uživatele tooSalesforce účty.</span><span class="sxs-lookup"><span data-stu-id="a72a1-128">hello objective of this section is toooutline how tooenable user provisioning of Active Directory user accounts tooSalesforce.</span></span>

1. <span data-ttu-id="a72a1-129">V hello [portál Azure](https://portal.azure.com), procházet toohello **Azure Active Directory > podnikové aplikace > všechny aplikace** části.</span><span class="sxs-lookup"><span data-stu-id="a72a1-129">In hello [Azure portal](https://portal.azure.com), browse toohello **Azure Active Directory > Enterprise Apps > All applications** section.</span></span>

2. <span data-ttu-id="a72a1-130">Pokud jste již nakonfigurovali Salesforce pro jednotné přihlašování, vyhledávání pro instanci služby Salesforce pomocí hello vyhledávací pole.</span><span class="sxs-lookup"><span data-stu-id="a72a1-130">If you have already configured Salesforce for single sign-on, search for your instance of Salesforce using hello search field.</span></span> <span data-ttu-id="a72a1-131">Jinak vyberte možnost **přidat** a vyhledejte **Salesforce** v galerii aplikací hello.</span><span class="sxs-lookup"><span data-stu-id="a72a1-131">Otherwise, select **Add** and search for **Salesforce** in hello application gallery.</span></span> <span data-ttu-id="a72a1-132">Vyberte Salesforce z výsledků hledání hello a přidejte ji tooyour seznam aplikací.</span><span class="sxs-lookup"><span data-stu-id="a72a1-132">Select Salesforce from hello search results, and add it tooyour list of applications.</span></span>

3. <span data-ttu-id="a72a1-133">Vyberte instanci služby Salesforce a pak vyberte hello **zřizování** kartě.</span><span class="sxs-lookup"><span data-stu-id="a72a1-133">Select your instance of Salesforce, then select hello **Provisioning** tab.</span></span>

4. <span data-ttu-id="a72a1-134">Sada hello **režimu zřizování** příliš**automatické**.</span><span class="sxs-lookup"><span data-stu-id="a72a1-134">Set hello **Provisioning Mode** too**Automatic**.</span></span> 
<span data-ttu-id="a72a1-135">![Zřizování](./media/active-directory-saas-salesforce-provisioning-tutorial/provisioning.png)</span><span class="sxs-lookup"><span data-stu-id="a72a1-135">![provisioning](./media/active-directory-saas-salesforce-provisioning-tutorial/provisioning.png)</span></span>

5. <span data-ttu-id="a72a1-136">V části hello **přihlašovací údaje správce** části, zadejte následující nastavení konfigurace hello:</span><span class="sxs-lookup"><span data-stu-id="a72a1-136">Under hello **Admin Credentials** section, provide hello following configuration settings:</span></span>
   
    <span data-ttu-id="a72a1-137">a.</span><span class="sxs-lookup"><span data-stu-id="a72a1-137">a.</span></span> <span data-ttu-id="a72a1-138">V hello **uživatelské jméno správce** textovému poli, typ Salesforce účet název, který má hello **správce systému** profil v Salesforce.com přiřazen.</span><span class="sxs-lookup"><span data-stu-id="a72a1-138">In hello **Admin User Name** textbox, type a Salesforce account name that has hello **System Administrator** profile in Salesforce.com assigned.</span></span>
   
    <span data-ttu-id="a72a1-139">b.</span><span class="sxs-lookup"><span data-stu-id="a72a1-139">b.</span></span> <span data-ttu-id="a72a1-140">V hello **heslo správce** textovému poli, zadejte hello heslo pro tento účet.</span><span class="sxs-lookup"><span data-stu-id="a72a1-140">In hello **Admin Password** textbox, type hello password for this account.</span></span>

6. <span data-ttu-id="a72a1-141">tooget vašem tokenu zabezpečení služby Salesforce, otevřete novou kartu a přihlášení do hello stejný účet správce služby Salesforce.</span><span class="sxs-lookup"><span data-stu-id="a72a1-141">tooget your Salesforce security token, open a new tab and sign into hello same Salesforce admin account.</span></span> <span data-ttu-id="a72a1-142">Na hello pravém horním rohu stránky hello, klikněte na své jméno a potom klikněte na **Moje nastavení**.</span><span class="sxs-lookup"><span data-stu-id="a72a1-142">On hello top right corner of hello page, click your name, and then click **My Settings**.</span></span>

     <span data-ttu-id="a72a1-143">![Povolit automatické uživatele zajišťování](./media/active-directory-saas-salesforce-provisioning-tutorial/sf-my-settings.png "povolit zřizování automatické uživatelů")</span><span class="sxs-lookup"><span data-stu-id="a72a1-143">![Enable automatic user provisioning](./media/active-directory-saas-salesforce-provisioning-tutorial/sf-my-settings.png "Enable automatic user provisioning")</span></span>
7. <span data-ttu-id="a72a1-144">V levém navigačním podokně hello, klikněte na tlačítko **osobní** tooexpand hello související části a pak klikněte na **resetovat Moje zabezpečení tokenu**.</span><span class="sxs-lookup"><span data-stu-id="a72a1-144">On hello left navigation pane, click **Personal** tooexpand hello related section, and then click **Reset My Security Token**.</span></span>
  
    <span data-ttu-id="a72a1-145">![Povolit automatické uživatele zajišťování](./media/active-directory-saas-salesforce-provisioning-tutorial/sf-personal-reset.png "povolit zřizování automatické uživatelů")</span><span class="sxs-lookup"><span data-stu-id="a72a1-145">![Enable automatic user provisioning](./media/active-directory-saas-salesforce-provisioning-tutorial/sf-personal-reset.png "Enable automatic user provisioning")</span></span>
8. <span data-ttu-id="a72a1-146">Na hello **resetovat Moje zabezpečení tokenu** klikněte na tlačítko **resetovat tokenu zabezpečení** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a72a1-146">On hello **Reset My Security Token** page, click **Reset Security Token** button.</span></span>

    <span data-ttu-id="a72a1-147">![Povolit automatické uživatele zajišťování](./media/active-directory-saas-salesforce-provisioning-tutorial/sf-reset-token.png "povolit zřizování automatické uživatelů")</span><span class="sxs-lookup"><span data-stu-id="a72a1-147">![Enable automatic user provisioning](./media/active-directory-saas-salesforce-provisioning-tutorial/sf-reset-token.png "Enable automatic user provisioning")</span></span>
9. <span data-ttu-id="a72a1-148">Zkontrolujte e-mailovou schránku hello spojené s tímto účtem správce.</span><span class="sxs-lookup"><span data-stu-id="a72a1-148">Check hello email inbox associated with this admin account.</span></span> <span data-ttu-id="a72a1-149">Vyhledejte e-mailu z Salesforce.com, který obsahuje hello nový token zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="a72a1-149">Look for an email from Salesforce.com that contains hello new security token.</span></span>
10. <span data-ttu-id="a72a1-150">Zkopírujte hello tokenu, přejděte tooyour okno Azure AD a vložte ho do hello **soketu tokenu** pole.</span><span class="sxs-lookup"><span data-stu-id="a72a1-150">Copy hello token, go tooyour Azure AD window, and paste it into hello **Socket Token** field.</span></span>

11. <span data-ttu-id="a72a1-151">V hello portálu Azure, klikněte na **Test připojení** tooensure Azure AD můžete připojit aplikaci tooyour Salesforce.</span><span class="sxs-lookup"><span data-stu-id="a72a1-151">In hello Azure portal, click **Test Connection** tooensure Azure AD can connect tooyour Salesforce app.</span></span>

12. <span data-ttu-id="a72a1-152">V hello **e-mailové oznámení** zadejte hello e-mailovou adresu uživatele nebo skupiny, kdo by měly dostávat oznámení zřizování chyby a zaškrtněte políčko hello níže.</span><span class="sxs-lookup"><span data-stu-id="a72a1-152">In hello **Notification Email** field, enter hello email address of a person or group who should receive provisioning error notifications, and check hello checkbox below.</span></span>

13. <span data-ttu-id="a72a1-153">Klikněte na tlačítko **uložit.**</span><span class="sxs-lookup"><span data-stu-id="a72a1-153">Click **Save.**</span></span>  
    
14.  <span data-ttu-id="a72a1-154">V části hello části mapování, vyberte **tooSalesforce synchronizaci uživatelů Azure Active Directory.**</span><span class="sxs-lookup"><span data-stu-id="a72a1-154">Under hello Mappings section, select **Synchronize Azure Active Directory Users tooSalesforce.**</span></span>

15. <span data-ttu-id="a72a1-155">V hello **mapování atributů** , projděte si hello uživatelské atributy, které jsou synchronizované z tooSalesforce Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a72a1-155">In hello **Attribute Mappings** section, review hello user attributes that are synchronized from Azure AD tooSalesforce.</span></span> <span data-ttu-id="a72a1-156">Všimněte si, že hello atributy vybrán jako **párování** vlastnosti jsou použité toomatch hello uživatelské účty v Salesforce pro operace aktualizace.</span><span class="sxs-lookup"><span data-stu-id="a72a1-156">Note that hello attributes selected as **Matching** properties are used toomatch hello user accounts in Salesforce for update operations.</span></span> <span data-ttu-id="a72a1-157">Vyberte toocommit tlačítko hello uložit změny.</span><span class="sxs-lookup"><span data-stu-id="a72a1-157">Select hello Save button toocommit any changes.</span></span>

16. <span data-ttu-id="a72a1-158">tooenable hello zřizování služby Azure AD pro služby Salesforce, změna hello **Stav zřizování** příliš**na** v části Nastavení hello</span><span class="sxs-lookup"><span data-stu-id="a72a1-158">tooenable hello Azure AD provisioning service for Salesforce, change hello **Provisioning Status** too**On** in hello Settings section</span></span>

17. <span data-ttu-id="a72a1-159">Klikněte na tlačítko **uložit.**</span><span class="sxs-lookup"><span data-stu-id="a72a1-159">Click **Save.**</span></span>

<span data-ttu-id="a72a1-160">Tím se spustí hello počáteční synchronizaci všech uživatelů a skupiny přiřazené tooSalesforce v hello uživatelé a skupiny oddílu.</span><span class="sxs-lookup"><span data-stu-id="a72a1-160">This starts hello initial synchronization of any users and/or groups assigned tooSalesforce in hello Users and Groups section.</span></span> <span data-ttu-id="a72a1-161">Všimněte si, že počáteční synchronizace hello trvá déle tooperform než následné synchronizace, ke kterým dochází přibližně každých 20 minut, dokud se službou hello.</span><span class="sxs-lookup"><span data-stu-id="a72a1-161">Note that hello initial sync takes longer tooperform than subsequent syncs, which occur approximately every 20 minutes as long as hello service is running.</span></span> <span data-ttu-id="a72a1-162">Můžete použít hello **podrobnosti synchronizace** části toomonitor průběh a postupujte podle pokynů odkazy tooprovisioning aktivity sestavy, které popisují všechny akce prováděné hello zřizování služby ve vaší aplikaci Salesforce.</span><span class="sxs-lookup"><span data-stu-id="a72a1-162">You can use hello **Synchronization Details** section toomonitor progress and follow links tooprovisioning activity reports, which describe all actions performed by hello provisioning service on your Salesforce app.</span></span>

<span data-ttu-id="a72a1-163">Nyní můžete vytvořit testovací účet.</span><span class="sxs-lookup"><span data-stu-id="a72a1-163">You can now create a test account.</span></span> <span data-ttu-id="a72a1-164">Počkejte, až minut too20 tooverify, který hello účet byl synchronizován tooSalesforce.</span><span class="sxs-lookup"><span data-stu-id="a72a1-164">Wait for up too20 minutes tooverify that hello account has been synchronized tooSalesforce.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a72a1-165">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="a72a1-165">Additional resources</span></span>

* [<span data-ttu-id="a72a1-166">Správa uživatelů zřizování účtu pro podnikové aplikace</span><span class="sxs-lookup"><span data-stu-id="a72a1-166">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a72a1-167">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="a72a1-167">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="a72a1-168">Konfigurovat jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="a72a1-168">Configure Single Sign-on</span></span>](active-directory-saas-salesforce-tutorial.md)