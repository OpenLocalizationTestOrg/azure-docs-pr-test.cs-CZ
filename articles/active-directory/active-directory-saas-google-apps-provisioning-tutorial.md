---
title: "Kurz: Konfigurace Google Apps pro zřizování automatické uživatelů v Azure | Microsoft Docs"
description: "Zjistěte, jak tooautomatically zřídit a deaktivace zřízení uživatelských účtů ze služby Azure AD tooGoogle aplikace."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 6dbd50b5-589f-4132-b9eb-a53a318a64e5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: d1fa8449bd6013d1627b3552aaa19db1c0f4f46f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-google-apps-for-automatic-user-provisioning"></a><span data-ttu-id="62c78-103">Kurz: Konfigurace Google Apps pro zřizování automatické uživatelů</span><span class="sxs-lookup"><span data-stu-id="62c78-103">Tutorial: Configuring Google Apps for automatic user provisioning</span></span>

<span data-ttu-id="62c78-104">cílem Hello tohoto kurzu je tooshow hello kroky nutné tooperform v Google Apps a službou Azure AD tooautomatically zřídit a zrušte zřízení uživatelských účtů z Azure AD tooGoogle aplikace.</span><span class="sxs-lookup"><span data-stu-id="62c78-104">hello objective of this tutorial is tooshow you hello steps you need tooperform in Google Apps and Azure AD tooautomatically provision and de-provision user accounts from Azure AD tooGoogle Apps.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="62c78-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="62c78-105">Prerequisites</span></span>

<span data-ttu-id="62c78-106">Hello scénáři uvedeném v tomto kurzu se předpokládá, že už máte hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="62c78-106">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

*   <span data-ttu-id="62c78-107">Klienta služby Azure Active directory.</span><span class="sxs-lookup"><span data-stu-id="62c78-107">An Azure Active directory tenant.</span></span>
*   <span data-ttu-id="62c78-108">Pro pracovní nebo Google Apps pro vzdělávací organizace musí mít platný klienta pro Google Apps.</span><span class="sxs-lookup"><span data-stu-id="62c78-108">You must have a valid tenant for Google Apps for Work or Google Apps for Education.</span></span> <span data-ttu-id="62c78-109">Bezplatný zkušební účet můžete použít buď služby.</span><span class="sxs-lookup"><span data-stu-id="62c78-109">You may use a free trial account for either service.</span></span>
*   <span data-ttu-id="62c78-110">Uživatelský účet v Google Apps s oprávněními správce týmu.</span><span class="sxs-lookup"><span data-stu-id="62c78-110">A user account in Google Apps with Team Admin permissions.</span></span>

## <a name="assigning-users-toogoogle-apps"></a><span data-ttu-id="62c78-111">Přiřazení uživatelů tooGoogle aplikace</span><span class="sxs-lookup"><span data-stu-id="62c78-111">Assigning users tooGoogle Apps</span></span>

<span data-ttu-id="62c78-112">Azure Active Directory používá koncept názvem "přiřazení" toodetermine uživatelů, kteří obdrželi přístup tooselected aplikace.</span><span class="sxs-lookup"><span data-stu-id="62c78-112">Azure Active Directory uses a concept called "assignments" toodetermine which users should receive access tooselected apps.</span></span> <span data-ttu-id="62c78-113">V kontextu hello zřizování účtu automatické uživatele se synchronizují pouze hello uživatelů a skupin, které byly "přiřazeny" tooan aplikace ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="62c78-113">In hello context of automatic user account provisioning, only hello users and groups that have been "assigned" tooan application in Azure AD is synchronized.</span></span>

<span data-ttu-id="62c78-114">Před konfigurací a povolení hello zřizování služby, musíte toodecide jaké uživatelů nebo skupin ve službě Azure AD představují hello uživatele, kteří potřebují přístup k aplikaci Google Apps tooyour.</span><span class="sxs-lookup"><span data-stu-id="62c78-114">Before configuring and enabling hello provisioning service, you need toodecide what users and/or groups in Azure AD represent hello users who need access tooyour Google Apps app.</span></span> <span data-ttu-id="62c78-115">Jakmile se rozhodli, můžete přiřadit aplikaci Google Apps tooyour těchto uživatelů podle pokynů hello zde: [přiřadit uživatele nebo skupinu tooan firemní aplikace](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)</span><span class="sxs-lookup"><span data-stu-id="62c78-115">Once decided, you can assign these users tooyour Google Apps app by following hello instructions here: [Assign a user or group tooan enterprise app](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)</span></span>

> [!IMPORTANT]
>*   <span data-ttu-id="62c78-116">Dále je doporučeno jednoho uživatele Azure AD přiřadit tooGoogle aplikace tootest hello zřizování konfigurace.</span><span class="sxs-lookup"><span data-stu-id="62c78-116">It is recommended that a single Azure AD user be assigned tooGoogle Apps tootest hello provisioning configuration.</span></span> <span data-ttu-id="62c78-117">Další uživatele nebo skupiny může být přiřazen později.</span><span class="sxs-lookup"><span data-stu-id="62c78-117">Additional users and/or groups may be assigned later.</span></span>
>*   <span data-ttu-id="62c78-118">Při přiřazování tooGoogle uživatele aplikace, je nutné vybrat hello uživatele nebo roli "Skupina" v dialogovém okně přiřazení hello.</span><span class="sxs-lookup"><span data-stu-id="62c78-118">When assigning a user tooGoogle Apps, you must select hello User or "Group" role in hello assignment dialog.</span></span> <span data-ttu-id="62c78-119">role "Výchozí přístup" Hello nefunguje pro zřizování.</span><span class="sxs-lookup"><span data-stu-id="62c78-119">hello "Default Access" role does not work for provisioning.</span></span>

## <a name="enable-automated-user-provisioning"></a><span data-ttu-id="62c78-120">Povolit zřizování automatizované uživatelů</span><span class="sxs-lookup"><span data-stu-id="62c78-120">Enable automated user provisioning</span></span>

<span data-ttu-id="62c78-121">Tato část vás provede připojením aplikace Azure AD tooGoogle uživatelský účet zřizování rozhraní API a konfiguraci hello zřizování služby toocreate, aktualizovat a zakázat přiřazené uživatelské účty v Google Apps podle přiřazení uživatelů a skupin ve službě Azure AD .</span><span class="sxs-lookup"><span data-stu-id="62c78-121">This section guides you through connecting your Azure AD tooGoogle Apps's user account provisioning API, and configuring hello provisioning service toocreate, update, and disable assigned user accounts in Google Apps based on user and group assignment in Azure AD.</span></span>

>[!Tip]
><span data-ttu-id="62c78-122">Můžete také zvolit tooenabled na základě SAML jednotné přihlašování pro Google Apps, hello pokynů uvedených v [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="62c78-122">You may also choose tooenabled SAML-based Single Sign-On for Google Apps, following hello instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="62c78-123">Jednotné přihlašování se dá nakonfigurovat nezávisle na automatické zřizování, i když tyto dvě funkce doplnění navzájem.</span><span class="sxs-lookup"><span data-stu-id="62c78-123">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>

### <a name="configure-automatic-user-account-provisioning"></a><span data-ttu-id="62c78-124">Konfigurace automatického uživatele zřizování účtu</span><span class="sxs-lookup"><span data-stu-id="62c78-124">Configure automatic user account provisioning</span></span>

> [!NOTE]
> <span data-ttu-id="62c78-125">Jiné vhodným řešením pro automatizaci zřizování aplikace tooGoogle uživatelů je toouse [Google Apps Directory Sync (GADS)](https://support.google.com/a/answer/106368?hl=en) která zřizuje vaší místní služby Active Directory identity tooGoogle aplikace.</span><span class="sxs-lookup"><span data-stu-id="62c78-125">Another viable option for automating user provisioning tooGoogle Apps is toouse [Google Apps Directory Sync (GADS)](https://support.google.com/a/answer/106368?hl=en) which provisions your on-premises Active Directory identities tooGoogle Apps.</span></span> <span data-ttu-id="62c78-126">Naproti tomu hello řešení v tomto kurzu zřídí uživatelů Azure Active Directory (cloud) a tooGoogle poštovní skupiny aplikací.</span><span class="sxs-lookup"><span data-stu-id="62c78-126">In contrast, hello solution in this tutorial provisions your Azure Active Directory (cloud) users and mail-enabled groups tooGoogle Apps.</span></span> 

1. <span data-ttu-id="62c78-127">Přihlaste se k hello [konzoly pro správu aplikace Google](http://admin.google.com/) pomocí účtu správce a klikněte na tlačítko **zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="62c78-127">Sign into hello [Google Apps Admin Console](http://admin.google.com/) using your administrator account, and click **Security**.</span></span> <span data-ttu-id="62c78-128">Pokud nevidíte odkaz hello, mohou být skryty pod hello **více ovládacích prvků** nabídky v hello dolní části obrazovky hello.</span><span class="sxs-lookup"><span data-stu-id="62c78-128">If you don't see hello link, it may be hidden under hello **More Controls** menu at hello bottom of hello screen.</span></span>
   
    ![Klikněte na Zabezpečení.][10]

2. <span data-ttu-id="62c78-130">Na hello **zabezpečení** klikněte na tlačítko **referenční dokumentace rozhraní API**.</span><span class="sxs-lookup"><span data-stu-id="62c78-130">On hello **Security** page, click **API Reference**.</span></span>
   
    ![Klikněte na tlačítko referenční dokumentace rozhraní API.][15]

3. <span data-ttu-id="62c78-132">Vyberte **povolit rozhraní API pro přístup**.</span><span class="sxs-lookup"><span data-stu-id="62c78-132">Select **Enable API access**.</span></span>
   
    ![Klikněte na tlačítko referenční dokumentace rozhraní API.][16]

    > [!IMPORTANT]
    > <span data-ttu-id="62c78-134">Pro každého uživatele, že máte v úmyslu tooprovision tooGoogle aplikace, svoje uživatelské jméno ve službě Azure Active Directory *musí* být vázanou tooa vlastní doménu.</span><span class="sxs-lookup"><span data-stu-id="62c78-134">For every user that you intend tooprovision tooGoogle Apps, their username in Azure Active Directory *must* be tied tooa custom domain.</span></span> <span data-ttu-id="62c78-135">Například uživatelská jména, které vypadají bob@contoso.onmicrosoft.com není přijat Google Apps, zatímco bob@contoso.com byla přijata.</span><span class="sxs-lookup"><span data-stu-id="62c78-135">For example, usernames that look like bob@contoso.onmicrosoft.com is not accepted by Google Apps, whereas bob@contoso.com is accepted.</span></span> <span data-ttu-id="62c78-136">Existujícího uživatele domény můžete změnit úpravou jejich vlastnosti ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="62c78-136">You can change an existing user's domain by editing their properties in Azure AD.</span></span> <span data-ttu-id="62c78-137">Pokyny pro jak tooset vlastní doménu pro Azure Active Directory a Google Apps jsou zahrnuty v následující kroky.</span><span class="sxs-lookup"><span data-stu-id="62c78-137">Instructions for how tooset a custom domain for both Azure Active Directory and Google Apps are included in following steps.</span></span>
      
4. <span data-ttu-id="62c78-138">Pokud zatím jste nepřidali tooyour název vlastní domény Azure Active Directory, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="62c78-138">If you haven't added a custom domain name tooyour Azure Active Directory yet, then follow hello following steps:</span></span>
  
    <span data-ttu-id="62c78-139">a.</span><span class="sxs-lookup"><span data-stu-id="62c78-139">a.</span></span> <span data-ttu-id="62c78-140">V hello [portál Azure](https://portal.azure.com), na levém navigačním podokně text hello, klikněte na **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="62c78-140">In hello [Azure portal](https://portal.azure.com), on hello left navigation pane, click **Active Directory**.</span></span> <span data-ttu-id="62c78-141">V seznamu adresářů hello vyberte svůj adresář.</span><span class="sxs-lookup"><span data-stu-id="62c78-141">In hello directory list, select your directory.</span></span> 

    <span data-ttu-id="62c78-142">b.</span><span class="sxs-lookup"><span data-stu-id="62c78-142">b.</span></span> <span data-ttu-id="62c78-143">Klikněte na tlačítko **název domény** hello levém navigačním podokně a pak klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="62c78-143">Click **Domains name** on hello left navigation pane, and then click **Add**.</span></span>
     
     ![Domény](./media/active-directory-saas-google-apps-provisioning-tutorial/domain_1.png)

     ![Přidání domény](./media/active-directory-saas-google-apps-provisioning-tutorial/domain_2.png)

    <span data-ttu-id="62c78-146">c.</span><span class="sxs-lookup"><span data-stu-id="62c78-146">c.</span></span> <span data-ttu-id="62c78-147">Zadejte název domény do hello **název domény** pole.</span><span class="sxs-lookup"><span data-stu-id="62c78-147">Type your domain name into hello **Domain name** field.</span></span> <span data-ttu-id="62c78-148">Tento název domény by měl být hello stejným názvem domény, že máte v úmyslu toouse pro Google Apps.</span><span class="sxs-lookup"><span data-stu-id="62c78-148">This domain name should be hello same domain name that you intend toouse for Google Apps.</span></span> <span data-ttu-id="62c78-149">Až budete připravení, klikněte na tlačítko hello **přidáním domény** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="62c78-149">When ready, click hello **Add Domain** button.</span></span>
     
     ![Název domény](./media/active-directory-saas-google-apps-provisioning-tutorial/domain_3.png)

    <span data-ttu-id="62c78-151">d.</span><span class="sxs-lookup"><span data-stu-id="62c78-151">d.</span></span> <span data-ttu-id="62c78-152">Klikněte na tlačítko **Další** toogo toohello ověření stránky.</span><span class="sxs-lookup"><span data-stu-id="62c78-152">Click **Next** toogo toohello verification page.</span></span> <span data-ttu-id="62c78-153">tooverify, že jste vlastníkem této domény, je nutné upravit záznamy DNS domény hello podle hodnoty toohello zadané na této stránce.</span><span class="sxs-lookup"><span data-stu-id="62c78-153">tooverify that you own this domain, you must edit hello domain's DNS records according toohello values provided on this page.</span></span> <span data-ttu-id="62c78-154">Můžete se rozhodnout tooverify buď pomocí **záznamů MX** nebo **záznamů TXT**podle toho, co jste vybrali pro hello **typ záznamu** možnost.</span><span class="sxs-lookup"><span data-stu-id="62c78-154">You may choose tooverify using either **MX records** or **TXT records**, depending on what you select for hello **Record Type** option.</span></span> <span data-ttu-id="62c78-155">Komplexní pokyny, jak název domény tooverify s Azure AD, najdete v části [přidat vlastní tooAzure název domény AD](https://go.microsoft.com/fwLink/?LinkID=278919&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="62c78-155">For more comprehensive instructions on how tooverify domain name with Azure AD, refer [Add your own domain name tooAzure AD](https://go.microsoft.com/fwLink/?LinkID=278919&clcid=0x409).</span></span>
     
     ![Domény](./media/active-directory-saas-google-apps-provisioning-tutorial/domain_4.png)

    <span data-ttu-id="62c78-157">e.</span><span class="sxs-lookup"><span data-stu-id="62c78-157">e.</span></span> <span data-ttu-id="62c78-158">Opakováním předchozích kroků pro všechny domény hello, že máte v úmyslu tooadd tooyour directory hello.</span><span class="sxs-lookup"><span data-stu-id="62c78-158">Repeat hello preceding steps for all hello domains that you intend tooadd tooyour directory.</span></span>

5. <span data-ttu-id="62c78-159">Teď, když si ověříte, že všechny vaše domény s Azure AD, je nutné nyní ověřit jejich znovu s Google Apps.</span><span class="sxs-lookup"><span data-stu-id="62c78-159">Now that you have verified all your domains with Azure AD, you must now verify them again with Google Apps.</span></span> <span data-ttu-id="62c78-160">Pro každou doménu, která již není registrován u služby Google Apps proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="62c78-160">For each domain that isn't already registered with Google Apps, perform hello following steps:</span></span>
   
    <span data-ttu-id="62c78-161">a.</span><span class="sxs-lookup"><span data-stu-id="62c78-161">a.</span></span> <span data-ttu-id="62c78-162">V hello [konzoly pro správu aplikace Google](http://admin.google.com/), klikněte na tlačítko **domény**.</span><span class="sxs-lookup"><span data-stu-id="62c78-162">In hello [Google Apps Admin Console](http://admin.google.com/), click **Domains**.</span></span>
     
     ![Klikněte na domény][20]

    <span data-ttu-id="62c78-164">b.</span><span class="sxs-lookup"><span data-stu-id="62c78-164">b.</span></span> <span data-ttu-id="62c78-165">Klikněte na tlačítko **přidejte k doméně nebo alias domény**.</span><span class="sxs-lookup"><span data-stu-id="62c78-165">Click **Add a domain or a domain alias**.</span></span>
     
     ![Přidání nové domény][21]

    <span data-ttu-id="62c78-167">c.</span><span class="sxs-lookup"><span data-stu-id="62c78-167">c.</span></span> <span data-ttu-id="62c78-168">Vyberte **přidat jiné domény**a pak zadejte název hello hello domény, které chcete tooadd.</span><span class="sxs-lookup"><span data-stu-id="62c78-168">Select **Add another domain**, and type in hello name of hello domain that you would like tooadd.</span></span>
     
     ![Zadejte název domény][22]

    <span data-ttu-id="62c78-170">d.</span><span class="sxs-lookup"><span data-stu-id="62c78-170">d.</span></span> <span data-ttu-id="62c78-171">Klikněte na tlačítko **pokračovat a ověření vlastnictví domény**.</span><span class="sxs-lookup"><span data-stu-id="62c78-171">Click **Continue and verify domain ownership**.</span></span> <span data-ttu-id="62c78-172">Potom postupujte podle tooverify hello kroky, které vlastníte hello název domény.</span><span class="sxs-lookup"><span data-stu-id="62c78-172">Then follow hello steps tooverify that you own hello domain name.</span></span> <span data-ttu-id="62c78-173">Pro komplexní pokyny, jak tooverify doménu s Google Apps, najdete v článku.</span><span class="sxs-lookup"><span data-stu-id="62c78-173">For comprehensive instructions on how tooverify your domain with Google Apps, see.</span></span> <span data-ttu-id="62c78-174">[Ověřit vlastnictví lokality s Google Apps](https://support.google.com/webmasters/answer/35179).</span><span class="sxs-lookup"><span data-stu-id="62c78-174">[Verify your site ownership with Google Apps](https://support.google.com/webmasters/answer/35179).</span></span>

    <span data-ttu-id="62c78-175">e.</span><span class="sxs-lookup"><span data-stu-id="62c78-175">e.</span></span> <span data-ttu-id="62c78-176">Opakováním předchozích kroků pro další domény, že máte v úmyslu tooadd tooGoogle aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="62c78-176">Repeat hello preceding steps for any additional domains that you intend tooadd tooGoogle Apps.</span></span>
     
     > [!WARNING]
     > <span data-ttu-id="62c78-177">Pokud změníte hello primární doménu vašeho klienta Google Apps, a pokud již máte nakonfigurovat jednotné přihlašování s Azure AD, pak máte toorepeat krok #3 pod [krok dva: Povolit jednotné přihlašování](#step-two-enable-single-sign-on).</span><span class="sxs-lookup"><span data-stu-id="62c78-177">If you change hello primary domain for your Google Apps tenant, and if you have already configured single sign-on with Azure AD, then you have toorepeat step #3 under [Step Two: Enable Single Sign-On](#step-two-enable-single-sign-on).</span></span>
       
6. <span data-ttu-id="62c78-178">V hello [konzoly pro správu aplikace Google](http://admin.google.com/), klikněte na tlačítko **rolí správce**.</span><span class="sxs-lookup"><span data-stu-id="62c78-178">In hello [Google Apps Admin Console](http://admin.google.com/), click **Admin Roles**.</span></span>
   
     ![Klikněte na Google Apps][26]

7. <span data-ttu-id="62c78-180">Určit, které správce účtu chcete zřizování toouse toomanage uživatelů.</span><span class="sxs-lookup"><span data-stu-id="62c78-180">Determine which admin account you would like toouse toomanage user provisioning.</span></span> <span data-ttu-id="62c78-181">Pro hello **role správce** tohoto účtu, upravit hello **oprávnění** pro tuto roli.</span><span class="sxs-lookup"><span data-stu-id="62c78-181">For hello **admin role** of that account, edit hello **Privileges** for that role.</span></span> <span data-ttu-id="62c78-182">Ujistěte se, obsahuje všechny hello **oprávnění rozhraní API Správce** povolené tak, aby tento účet slouží pro zřizování.</span><span class="sxs-lookup"><span data-stu-id="62c78-182">Make sure it has all hello **Admin API Privileges** enabled so that this account can be used for provisioning.</span></span>
   
     ![Klikněte na Google Apps][27]
   
    > [!NOTE]
    > <span data-ttu-id="62c78-184">Pokud konfigurujete provozním prostředí, je osvědčeným postupem hello toocreate účet správce v Google Apps speciálně pro tento krok.</span><span class="sxs-lookup"><span data-stu-id="62c78-184">If you are configuring a production environment, hello best practice is toocreate an admin account in Google Apps specifically for this step.</span></span> <span data-ttu-id="62c78-185">Tyto účty musí mít roli správce s ním spojená s hello potřebná oprávnění na rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="62c78-185">These accounts must have an admin role associated with it that has hello necessary API privileges.</span></span>
     
8. <span data-ttu-id="62c78-186">V hello [portál Azure](https://portal.azure.com), procházet toohello **Azure Active Directory > podnikové aplikace > všechny aplikace** části.</span><span class="sxs-lookup"><span data-stu-id="62c78-186">In hello [Azure portal](https://portal.azure.com), browse toohello **Azure Active Directory > Enterprise Apps > All applications** section.</span></span>

9. <span data-ttu-id="62c78-187">Pokud jste již nakonfigurovali Google Apps pro jednotné přihlašování, vyhledávání pro instanci služby Google Apps pomocí hello vyhledávací pole.</span><span class="sxs-lookup"><span data-stu-id="62c78-187">If you have already configured Google Apps for single sign-on, search for your instance of Google Apps using hello search field.</span></span> <span data-ttu-id="62c78-188">Jinak vyberte možnost **přidat** a vyhledejte **Google Apps** v galerii aplikací hello.</span><span class="sxs-lookup"><span data-stu-id="62c78-188">Otherwise, select **Add** and search for **Google Apps** in hello application gallery.</span></span> <span data-ttu-id="62c78-189">Vyberte Google Apps z výsledků hledání hello a přidejte ji tooyour seznam aplikací.</span><span class="sxs-lookup"><span data-stu-id="62c78-189">Select Google Apps from hello search results, and add it tooyour list of applications.</span></span>

10. <span data-ttu-id="62c78-190">Vyberte instanci služby Google Apps a pak vyberte hello **zřizování** kartě.</span><span class="sxs-lookup"><span data-stu-id="62c78-190">Select your instance of Google Apps, then select hello **Provisioning** tab.</span></span>

11. <span data-ttu-id="62c78-191">Sada hello **režimu zřizování** příliš**automatické**.</span><span class="sxs-lookup"><span data-stu-id="62c78-191">Set hello **Provisioning Mode** too**Automatic**.</span></span> 

     ![Zřizování](./media/active-directory-saas-google-apps-provisioning-tutorial/provisioning.png)

12. <span data-ttu-id="62c78-193">V části hello **přihlašovací údaje správce** klikněte na tlačítko **Authorize**.</span><span class="sxs-lookup"><span data-stu-id="62c78-193">Under hello **Admin Credentials** section, click **Authorize**.</span></span> <span data-ttu-id="62c78-194">Otevře se dialogové okno Google Apps autorizace v nové okno prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="62c78-194">It opens a Google Apps authorization dialog in a new browser window.</span></span>

13. <span data-ttu-id="62c78-195">Potvrďte, že byste chtěli toogive Azure Active Directory oprávnění toomake změn, které tooyour Google Apps klienta.</span><span class="sxs-lookup"><span data-stu-id="62c78-195">Confirm that you would like toogive Azure Active Directory permission toomake changes tooyour Google Apps tenant.</span></span> <span data-ttu-id="62c78-196">Klikněte na tlačítko **přijmout**.</span><span class="sxs-lookup"><span data-stu-id="62c78-196">Click **Accept**.</span></span>
    
     ![Zkontrolujte oprávnění.][28]

14. <span data-ttu-id="62c78-198">V hello portálu Azure, klikněte na **Test připojení** tooensure Azure AD můžete připojit tooyour aplikaci Google Apps.</span><span class="sxs-lookup"><span data-stu-id="62c78-198">In hello Azure portal, click **Test Connection** tooensure Azure AD can connect tooyour Google Apps app.</span></span> <span data-ttu-id="62c78-199">Pokud hello připojení nezdaří, zkontrolujte oprávnění správce Team má váš účet Google Apps a zkuste to hello **"Ověřit"** krok opakujte.</span><span class="sxs-lookup"><span data-stu-id="62c78-199">If hello connection fails, ensure your Google Apps account has Team Admin permissions and try hello **"Authorize"** step again.</span></span>

15. <span data-ttu-id="62c78-200">Zadejte hello e-mailovou adresu uživatele nebo skupiny, který by měly dostávat oznámení zřizování Chyba v hello **e-mailové oznámení** pole a zaškrtněte políčko hello.</span><span class="sxs-lookup"><span data-stu-id="62c78-200">Enter hello email address of a person or group who should receive provisioning error notifications in hello **Notification Email** field, and check hello checkbox.</span></span>

16. <span data-ttu-id="62c78-201">Klikněte na tlačítko **uložit.**</span><span class="sxs-lookup"><span data-stu-id="62c78-201">Click **Save.**</span></span>

17. <span data-ttu-id="62c78-202">V části hello části mapování, vyberte **synchronizaci uživatelů Azure Active Directory tooGoogle aplikace.**</span><span class="sxs-lookup"><span data-stu-id="62c78-202">Under hello Mappings section, select **Synchronize Azure Active Directory Users tooGoogle Apps.**</span></span>

18. <span data-ttu-id="62c78-203">V hello **mapování atributů** , projděte si hello uživatelské atributy, které jsou synchronizované z Azure AD tooGoogle aplikace.</span><span class="sxs-lookup"><span data-stu-id="62c78-203">In hello **Attribute Mappings** section, review hello user attributes that are synchronized from Azure AD tooGoogle Apps.</span></span> <span data-ttu-id="62c78-204">Hello atributy vybrán jako **párování** vlastnosti jsou použité toomatch hello uživatelské účty v Google Apps pro operace aktualizace.</span><span class="sxs-lookup"><span data-stu-id="62c78-204">hello attributes selected as **Matching** properties are used toomatch hello user accounts in Google Apps for update operations.</span></span> <span data-ttu-id="62c78-205">Vyberte toocommit tlačítko hello uložit změny.</span><span class="sxs-lookup"><span data-stu-id="62c78-205">Select hello Save button toocommit any changes.</span></span>

19. <span data-ttu-id="62c78-206">tooenable hello zřizování služby Azure AD pro Google Apps, změna hello **Stav zřizování** příliš**na** v části Nastavení hello</span><span class="sxs-lookup"><span data-stu-id="62c78-206">tooenable hello Azure AD provisioning service for Google Apps, change hello **Provisioning Status** too**On** in hello Settings section</span></span>

20. <span data-ttu-id="62c78-207">Klikněte na tlačítko **uložit.**</span><span class="sxs-lookup"><span data-stu-id="62c78-207">Click **Save.**</span></span>

<span data-ttu-id="62c78-208">Spustí počáteční synchronizaci hello všechny uživatele nebo skupiny přiřazené tooGoogle aplikace v části Uživatelé a skupiny hello.</span><span class="sxs-lookup"><span data-stu-id="62c78-208">It starts hello initial synchronization of any users and/or groups assigned tooGoogle Apps in hello Users and Groups section.</span></span> <span data-ttu-id="62c78-209">počáteční synchronizace Hello trvá déle tooperform než následné synchronizace, ke kterým dochází přibližně každých 20 minut, dokud se službou hello.</span><span class="sxs-lookup"><span data-stu-id="62c78-209">hello initial sync takes longer tooperform than subsequent syncs, which occur approximately every 20 minutes as long as hello service is running.</span></span> <span data-ttu-id="62c78-210">Můžete použít hello **podrobnosti synchronizace** části toomonitor průběh a postupujte podle pokynů odkazy tooprovisioning aktivity sestavy, které popisují všechny akce prováděné hello zřizování služby ve vaší aplikaci Google Apps.</span><span class="sxs-lookup"><span data-stu-id="62c78-210">You can use hello **Synchronization Details** section toomonitor progress and follow links tooprovisioning activity reports, which describe all actions performed by hello provisioning service on your Google Apps app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="62c78-211">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="62c78-211">Additional resources</span></span>

* [<span data-ttu-id="62c78-212">Správa uživatelů zřizování účtu pro podnikové aplikace</span><span class="sxs-lookup"><span data-stu-id="62c78-212">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="62c78-213">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="62c78-213">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="62c78-214">Konfigurovat jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="62c78-214">Configure Single Sign-on</span></span>](active-directory-saas-google-apps-tutorial.md)



<!--Image references-->

[10]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-security.png
[15]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-api.png
[16]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-api-enabled.png
[20]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-domains.png
[21]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-add-domain.png
[22]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-add-another.png
[24]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-provisioning.png
[25]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-provisioning-auth.png
[26]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-admin.png
[27]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-admin-privileges.png
[28]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-auth.png