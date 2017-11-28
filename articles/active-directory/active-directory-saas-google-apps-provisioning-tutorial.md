---
title: "Kurz: Konfigurace Google Apps pro zřizování automatické uživatelů v Azure | Microsoft Docs"
description: "Naučte se automaticky zřizovat a zrušte zřízení uživatelských účtů ze služby Azure AD do Google Apps."
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
ms.openlocfilehash: b061f0ddad70be4a5ca48d48d1a737d6af8afa8d
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/29/2017
---
# <a name="tutorial-configuring-google-apps-for-automatic-user-provisioning"></a><span data-ttu-id="7f463-103">Kurz: Konfigurace Google Apps pro zřizování automatické uživatelů</span><span class="sxs-lookup"><span data-stu-id="7f463-103">Tutorial: Configuring Google Apps for automatic user provisioning</span></span>

<span data-ttu-id="7f463-104">Cílem tohoto kurzu je tak, aby zobrazovalo kroky, které je třeba provést v Google Apps a službou Azure AD a automaticky zřizovat a zrušte zřízení uživatelských účtů ze služby Azure AD do Google Apps.</span><span class="sxs-lookup"><span data-stu-id="7f463-104">The objective of this tutorial is to show you the steps you need to perform in Google Apps and Azure AD to automatically provision and de-provision user accounts from Azure AD to Google Apps.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7f463-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="7f463-105">Prerequisites</span></span>

<span data-ttu-id="7f463-106">Scénář uvedených v tomto kurzu se předpokládá, že už máte následující položky:</span><span class="sxs-lookup"><span data-stu-id="7f463-106">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

*   <span data-ttu-id="7f463-107">Klienta služby Azure Active directory.</span><span class="sxs-lookup"><span data-stu-id="7f463-107">An Azure Active directory tenant.</span></span>
*   <span data-ttu-id="7f463-108">Pro pracovní nebo Google Apps pro vzdělávací organizace musí mít platný klienta pro Google Apps.</span><span class="sxs-lookup"><span data-stu-id="7f463-108">You must have a valid tenant for Google Apps for Work or Google Apps for Education.</span></span> <span data-ttu-id="7f463-109">Bezplatný zkušební účet můžete použít buď služby.</span><span class="sxs-lookup"><span data-stu-id="7f463-109">You may use a free trial account for either service.</span></span>
*   <span data-ttu-id="7f463-110">Uživatelský účet v Google Apps s oprávněními správce týmu.</span><span class="sxs-lookup"><span data-stu-id="7f463-110">A user account in Google Apps with Team Admin permissions.</span></span>

## <a name="assigning-users-to-google-apps"></a><span data-ttu-id="7f463-111">Přiřazení uživatelů ke Google Apps</span><span class="sxs-lookup"><span data-stu-id="7f463-111">Assigning users to Google Apps</span></span>

<span data-ttu-id="7f463-112">Azure Active Directory používá koncept označované jako "úlohy" k určení uživatelů, kteří obdrželi přístup k vybrané aplikace.</span><span class="sxs-lookup"><span data-stu-id="7f463-112">Azure Active Directory uses a concept called "assignments" to determine which users should receive access to selected apps.</span></span> <span data-ttu-id="7f463-113">V kontextu uživatele automatické zřizování účtu se synchronizují pouze uživatelé a skupiny, které byly "přiřazeny" aplikace ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7f463-113">In the context of automatic user account provisioning, only the users and groups that have been "assigned" to an application in Azure AD is synchronized.</span></span>

<span data-ttu-id="7f463-114">Před konfigurací a povolení zřizování služby, musíte rozhodnout, jaké uživatelů nebo skupin ve službě Azure AD představují uživatele, kteří potřebují přístup k vaší aplikaci Google Apps.</span><span class="sxs-lookup"><span data-stu-id="7f463-114">Before configuring and enabling the provisioning service, you need to decide what users and/or groups in Azure AD represent the users who need access to your Google Apps app.</span></span> <span data-ttu-id="7f463-115">Jakmile se rozhodli, můžete přiřadit tyto uživatele do aplikace pro Google Apps podle pokynů tady: [přiřadit uživatele nebo skupinu enterprise aplikace](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)</span><span class="sxs-lookup"><span data-stu-id="7f463-115">Once decided, you can assign these users to your Google Apps app by following the instructions here: [Assign a user or group to an enterprise app](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)</span></span>

> [!IMPORTANT]
>*   <span data-ttu-id="7f463-116">Dále je doporučeno jednoho uživatele Azure AD pro Google Apps přidělí otestovat konfiguraci zřizování.</span><span class="sxs-lookup"><span data-stu-id="7f463-116">It is recommended that a single Azure AD user be assigned to Google Apps to test the provisioning configuration.</span></span> <span data-ttu-id="7f463-117">Další uživatele nebo skupiny může být přiřazen později.</span><span class="sxs-lookup"><span data-stu-id="7f463-117">Additional users and/or groups may be assigned later.</span></span>
>*   <span data-ttu-id="7f463-118">Při přiřazování uživatele Google Apps, je nutné zvolit roli uživatele nebo "Skupiny" v dialogovém okně přiřazení.</span><span class="sxs-lookup"><span data-stu-id="7f463-118">When assigning a user to Google Apps, you must select the User or "Group" role in the assignment dialog.</span></span> <span data-ttu-id="7f463-119">Roli "Výchozí přístup" nefunguje pro zřizování.</span><span class="sxs-lookup"><span data-stu-id="7f463-119">The "Default Access" role does not work for provisioning.</span></span>

## <a name="enable-automated-user-provisioning"></a><span data-ttu-id="7f463-120">Povolit zřizování automatizované uživatelů</span><span class="sxs-lookup"><span data-stu-id="7f463-120">Enable automated user provisioning</span></span>

<span data-ttu-id="7f463-121">Tato část příručky vás připojení služby Azure AD k rozhraní API Google Apps uživatel účet zřizování a konfigurací zřizování službu, kterou chcete vytvořit, aktualizovat a zakázat přiřazené uživatelské účty v Google Apps na základě uživatele a přiřazení skupiny ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7f463-121">This section guides you through connecting your Azure AD to Google Apps's user account provisioning API, and configuring the provisioning service to create, update, and disable assigned user accounts in Google Apps based on user and group assignment in Azure AD.</span></span>

>[!Tip]
><span data-ttu-id="7f463-122">Také můžete povolit na základě SAML jednotné přihlašování pro Google Apps, postupujte podle pokynů uvedených v [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="7f463-122">You may also choose to enabled SAML-based Single Sign-On for Google Apps, following the instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="7f463-123">Jednotné přihlašování se dá nakonfigurovat nezávisle na automatické zřizování, i když tyto dvě funkce doplnění navzájem.</span><span class="sxs-lookup"><span data-stu-id="7f463-123">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>

### <a name="configure-automatic-user-account-provisioning"></a><span data-ttu-id="7f463-124">Konfigurace automatického uživatele zřizování účtu</span><span class="sxs-lookup"><span data-stu-id="7f463-124">Configure automatic user account provisioning</span></span>

> [!NOTE]
> <span data-ttu-id="7f463-125">Jiné vhodným řešením pro automatizaci zřizování uživatelů ke Google Apps je použití [Google Apps Directory Sync (GADS)](https://support.google.com/a/answer/106368?hl=en) která zřizuje identitami místní služby Active Directory pro Google Apps.</span><span class="sxs-lookup"><span data-stu-id="7f463-125">Another viable option for automating user provisioning to Google Apps is to use [Google Apps Directory Sync (GADS)](https://support.google.com/a/answer/106368?hl=en) which provisions your on-premises Active Directory identities to Google Apps.</span></span> <span data-ttu-id="7f463-126">Naproti tomu zřídí řešení v tomto kurzu uživatelů Azure Active Directory (cloud) a poštovní skupiny Google Apps.</span><span class="sxs-lookup"><span data-stu-id="7f463-126">In contrast, the solution in this tutorial provisions your Azure Active Directory (cloud) users and mail-enabled groups to Google Apps.</span></span> 

1. <span data-ttu-id="7f463-127">Přihlaste se k [konzoly pro správu aplikace Google](http://admin.google.com/) pomocí účtu správce a klikněte na tlačítko **zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="7f463-127">Sign into the [Google Apps Admin Console](http://admin.google.com/) using your administrator account, and click **Security**.</span></span> <span data-ttu-id="7f463-128">Pokud nevidíte odkaz, mohou být skryty pod **více ovládacích prvků** nabídky v dolní části obrazovky.</span><span class="sxs-lookup"><span data-stu-id="7f463-128">If you don't see the link, it may be hidden under the **More Controls** menu at the bottom of the screen.</span></span>
   
    ![Klikněte na Zabezpečení.][10]

2. <span data-ttu-id="7f463-130">Na **zabezpečení** klikněte na tlačítko **referenční dokumentace rozhraní API**.</span><span class="sxs-lookup"><span data-stu-id="7f463-130">On the **Security** page, click **API Reference**.</span></span>
   
    ![Klikněte na tlačítko referenční dokumentace rozhraní API.][15]

3. <span data-ttu-id="7f463-132">Vyberte **povolit rozhraní API pro přístup**.</span><span class="sxs-lookup"><span data-stu-id="7f463-132">Select **Enable API access**.</span></span>
   
    ![Klikněte na tlačítko referenční dokumentace rozhraní API.][16]

    > [!IMPORTANT]
    > <span data-ttu-id="7f463-134">Pro každého uživatele, který máte v úmyslu zřízení Google Apps, svoje uživatelské jméno ve službě Azure Active Directory *musí* být vázáno vlastní doménu.</span><span class="sxs-lookup"><span data-stu-id="7f463-134">For every user that you intend to provision to Google Apps, their username in Azure Active Directory *must* be tied to a custom domain.</span></span> <span data-ttu-id="7f463-135">Například uživatelská jména, které vypadají bob@contoso.onmicrosoft.com není přijat Google Apps, zatímco bob@contoso.com byla přijata.</span><span class="sxs-lookup"><span data-stu-id="7f463-135">For example, usernames that look like bob@contoso.onmicrosoft.com is not accepted by Google Apps, whereas bob@contoso.com is accepted.</span></span> <span data-ttu-id="7f463-136">Existujícího uživatele domény můžete změnit úpravou jejich vlastnosti ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7f463-136">You can change an existing user's domain by editing their properties in Azure AD.</span></span> <span data-ttu-id="7f463-137">Pokyny, jak nastavit vlastní doménu pro Azure Active Directory a Google Apps jsou součástí následující kroky.</span><span class="sxs-lookup"><span data-stu-id="7f463-137">Instructions for how to set a custom domain for both Azure Active Directory and Google Apps are included in following steps.</span></span>
      
4. <span data-ttu-id="7f463-138">Pokud zatím jste nepřidali vlastního názvu domény do Azure Active Directory, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="7f463-138">If you haven't added a custom domain name to your Azure Active Directory yet, then follow the following steps:</span></span>
  
    <span data-ttu-id="7f463-139">a.</span><span class="sxs-lookup"><span data-stu-id="7f463-139">a.</span></span> <span data-ttu-id="7f463-140">V [portál Azure](https://portal.azure.com), v levém navigačním podokně klikněte na tlačítko **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="7f463-140">In the [Azure portal](https://portal.azure.com), on the left navigation pane, click **Active Directory**.</span></span> <span data-ttu-id="7f463-141">V seznamu adresáře vyberte svůj adresář.</span><span class="sxs-lookup"><span data-stu-id="7f463-141">In the directory list, select your directory.</span></span> 

    <span data-ttu-id="7f463-142">b.</span><span class="sxs-lookup"><span data-stu-id="7f463-142">b.</span></span> <span data-ttu-id="7f463-143">Klikněte na tlačítko **název domény** v levém navigačním podokně a pak klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="7f463-143">Click **Domains name** on the left navigation pane, and then click **Add**.</span></span>
     
     ![Domény](./media/active-directory-saas-google-apps-provisioning-tutorial/domain_1.png)

     ![Přidání domény](./media/active-directory-saas-google-apps-provisioning-tutorial/domain_2.png)

    <span data-ttu-id="7f463-146">c.</span><span class="sxs-lookup"><span data-stu-id="7f463-146">c.</span></span> <span data-ttu-id="7f463-147">Zadejte název domény do **název domény** pole.</span><span class="sxs-lookup"><span data-stu-id="7f463-147">Type your domain name into the **Domain name** field.</span></span> <span data-ttu-id="7f463-148">Tento název domény by měl být stejný jako název domény, kterou chcete použít pro Google Apps.</span><span class="sxs-lookup"><span data-stu-id="7f463-148">This domain name should be the same domain name that you intend to use for Google Apps.</span></span> <span data-ttu-id="7f463-149">Až budete připravení, klikněte na **přidáním domény** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="7f463-149">When ready, click the **Add Domain** button.</span></span>
     
     ![Název domény](./media/active-directory-saas-google-apps-provisioning-tutorial/domain_3.png)

    <span data-ttu-id="7f463-151">d.</span><span class="sxs-lookup"><span data-stu-id="7f463-151">d.</span></span> <span data-ttu-id="7f463-152">Klikněte na tlačítko **Další** přejděte na stránku pro ověření.</span><span class="sxs-lookup"><span data-stu-id="7f463-152">Click **Next** to go to the verification page.</span></span> <span data-ttu-id="7f463-153">Pokud chcete ověřit, že jste vlastníkem této domény, je nutné upravit záznamy DNS domény podle hodnoty na této stránce.</span><span class="sxs-lookup"><span data-stu-id="7f463-153">To verify that you own this domain, you must edit the domain's DNS records according to the values provided on this page.</span></span> <span data-ttu-id="7f463-154">Můžete ověřit pomocí buď **záznamů MX** nebo **záznamů TXT**podle toho, co jste vybrali pro **typ záznamu** možnost.</span><span class="sxs-lookup"><span data-stu-id="7f463-154">You may choose to verify using either **MX records** or **TXT records**, depending on what you select for the **Record Type** option.</span></span> <span data-ttu-id="7f463-155">Komplexní pokyny o tom, jak ověřit název domény se službou Azure AD, najdete v části [přidat vlastní název domény do Azure AD](https://go.microsoft.com/fwLink/?LinkID=278919&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="7f463-155">For more comprehensive instructions on how to verify domain name with Azure AD, refer [Add your own domain name to Azure AD](https://go.microsoft.com/fwLink/?LinkID=278919&clcid=0x409).</span></span>
     
     ![Domény](./media/active-directory-saas-google-apps-provisioning-tutorial/domain_4.png)

    <span data-ttu-id="7f463-157">e.</span><span class="sxs-lookup"><span data-stu-id="7f463-157">e.</span></span> <span data-ttu-id="7f463-158">Opakujte předchozí kroky u všech domén, které chcete přidat do vašeho adresáře.</span><span class="sxs-lookup"><span data-stu-id="7f463-158">Repeat the preceding steps for all the domains that you intend to add to your directory.</span></span>

5. <span data-ttu-id="7f463-159">Teď, když si ověříte, že všechny vaše domény s Azure AD, je nutné nyní ověřit jejich znovu s Google Apps.</span><span class="sxs-lookup"><span data-stu-id="7f463-159">Now that you have verified all your domains with Azure AD, you must now verify them again with Google Apps.</span></span> <span data-ttu-id="7f463-160">Pro každou doménu, která již není registrován u služby Google Apps proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="7f463-160">For each domain that isn't already registered with Google Apps, perform the following steps:</span></span>
   
    <span data-ttu-id="7f463-161">a.</span><span class="sxs-lookup"><span data-stu-id="7f463-161">a.</span></span> <span data-ttu-id="7f463-162">V [konzoly pro správu aplikace Google](http://admin.google.com/), klikněte na tlačítko **domény**.</span><span class="sxs-lookup"><span data-stu-id="7f463-162">In the [Google Apps Admin Console](http://admin.google.com/), click **Domains**.</span></span>
     
     ![Klikněte na domény][20]

    <span data-ttu-id="7f463-164">b.</span><span class="sxs-lookup"><span data-stu-id="7f463-164">b.</span></span> <span data-ttu-id="7f463-165">Klikněte na tlačítko **přidejte k doméně nebo alias domény**.</span><span class="sxs-lookup"><span data-stu-id="7f463-165">Click **Add a domain or a domain alias**.</span></span>
     
     ![Přidání nové domény][21]

    <span data-ttu-id="7f463-167">c.</span><span class="sxs-lookup"><span data-stu-id="7f463-167">c.</span></span> <span data-ttu-id="7f463-168">Vyberte **přidat jiné domény**a zadejte název domény, který chcete přidat.</span><span class="sxs-lookup"><span data-stu-id="7f463-168">Select **Add another domain**, and type in the name of the domain that you would like to add.</span></span>
     
     ![Zadejte název domény][22]

    <span data-ttu-id="7f463-170">d.</span><span class="sxs-lookup"><span data-stu-id="7f463-170">d.</span></span> <span data-ttu-id="7f463-171">Klikněte na tlačítko **pokračovat a ověření vlastnictví domény**.</span><span class="sxs-lookup"><span data-stu-id="7f463-171">Click **Continue and verify domain ownership**.</span></span> <span data-ttu-id="7f463-172">Postupujte podle pokynů k ověření, že jste vlastníkem názvu domény.</span><span class="sxs-lookup"><span data-stu-id="7f463-172">Then follow the steps to verify that you own the domain name.</span></span> <span data-ttu-id="7f463-173">Komplexní pokyny o tom, jak ověřit doménu s Google Apps naleznete v tématu.</span><span class="sxs-lookup"><span data-stu-id="7f463-173">For comprehensive instructions on how to verify your domain with Google Apps, see.</span></span> <span data-ttu-id="7f463-174">[Ověřit vlastnictví lokality s Google Apps](https://support.google.com/webmasters/answer/35179).</span><span class="sxs-lookup"><span data-stu-id="7f463-174">[Verify your site ownership with Google Apps](https://support.google.com/webmasters/answer/35179).</span></span>

    <span data-ttu-id="7f463-175">e.</span><span class="sxs-lookup"><span data-stu-id="7f463-175">e.</span></span> <span data-ttu-id="7f463-176">Opakujte předchozí kroky pro další domény, které chcete přidat do Google Apps.</span><span class="sxs-lookup"><span data-stu-id="7f463-176">Repeat the preceding steps for any additional domains that you intend to add to Google Apps.</span></span>
     
     > [!WARNING]
     > <span data-ttu-id="7f463-177">Pokud změníte primární doménu vašeho klienta Google Apps, a pokud již máte nakonfigurovat jednotné přihlašování s Azure AD, pak je nutné zopakovat krok #3 pod [krok dva: Povolit jednotné přihlašování](#step-two-enable-single-sign-on).</span><span class="sxs-lookup"><span data-stu-id="7f463-177">If you change the primary domain for your Google Apps tenant, and if you have already configured single sign-on with Azure AD, then you have to repeat step #3 under [Step Two: Enable Single Sign-On](#step-two-enable-single-sign-on).</span></span>
       
6. <span data-ttu-id="7f463-178">V [konzoly pro správu aplikace Google](http://admin.google.com/), klikněte na tlačítko **rolí správce**.</span><span class="sxs-lookup"><span data-stu-id="7f463-178">In the [Google Apps Admin Console](http://admin.google.com/), click **Admin Roles**.</span></span>
   
     ![Klikněte na Google Apps][26]

7. <span data-ttu-id="7f463-180">Určí, které účet správce, který chcete použít ke správě zřizování uživatelů.</span><span class="sxs-lookup"><span data-stu-id="7f463-180">Determine which admin account you would like to use to manage user provisioning.</span></span> <span data-ttu-id="7f463-181">Pro **role správce** tohoto účtu, upravit **oprávnění** pro tuto roli.</span><span class="sxs-lookup"><span data-stu-id="7f463-181">For the **admin role** of that account, edit the **Privileges** for that role.</span></span> <span data-ttu-id="7f463-182">Ujistěte se, obsahuje všechny **oprávnění rozhraní API Správce** povolené tak, aby tento účet slouží pro zřizování.</span><span class="sxs-lookup"><span data-stu-id="7f463-182">Make sure it has all the **Admin API Privileges** enabled so that this account can be used for provisioning.</span></span>
   
     ![Klikněte na Google Apps][27]
   
    > [!NOTE]
    > <span data-ttu-id="7f463-184">Pokud konfigurujete provozním prostředí, osvědčeným postupem je vytvoření účtu správce v Google Apps speciálně pro tento krok.</span><span class="sxs-lookup"><span data-stu-id="7f463-184">If you are configuring a production environment, the best practice is to create an admin account in Google Apps specifically for this step.</span></span> <span data-ttu-id="7f463-185">Tyto účty musí mít roli správce s ním spojená s potřebnými oprávněními rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="7f463-185">These accounts must have an admin role associated with it that has the necessary API privileges.</span></span>
     
8. <span data-ttu-id="7f463-186">V [portál Azure](https://portal.azure.com), vyhledejte **Azure Active Directory > podnikové aplikace > všechny aplikace** části.</span><span class="sxs-lookup"><span data-stu-id="7f463-186">In the [Azure portal](https://portal.azure.com), browse to the **Azure Active Directory > Enterprise Apps > All applications** section.</span></span>

9. <span data-ttu-id="7f463-187">Pokud jste již nakonfigurovali Google Apps pro jednotné přihlašování, vyhledávání pro instanci služby Google Apps pomocí pole hledání.</span><span class="sxs-lookup"><span data-stu-id="7f463-187">If you have already configured Google Apps for single sign-on, search for your instance of Google Apps using the search field.</span></span> <span data-ttu-id="7f463-188">Jinak vyberte možnost **přidat** a vyhledejte **Google Apps** v galerii aplikací.</span><span class="sxs-lookup"><span data-stu-id="7f463-188">Otherwise, select **Add** and search for **Google Apps** in the application gallery.</span></span> <span data-ttu-id="7f463-189">Vyberte Google Apps ve výsledcích hledání a přidejte ji do seznamu aplikací.</span><span class="sxs-lookup"><span data-stu-id="7f463-189">Select Google Apps from the search results, and add it to your list of applications.</span></span>

10. <span data-ttu-id="7f463-190">Vyberte instanci služby Google Apps a pak vyberte **zřizování** kartě.</span><span class="sxs-lookup"><span data-stu-id="7f463-190">Select your instance of Google Apps, then select the **Provisioning** tab.</span></span>

11. <span data-ttu-id="7f463-191">Nastavte **režimu zřizování** k **automatické**.</span><span class="sxs-lookup"><span data-stu-id="7f463-191">Set the **Provisioning Mode** to **Automatic**.</span></span> 

     ![Zřizování](./media/active-directory-saas-google-apps-provisioning-tutorial/provisioning.png)

12. <span data-ttu-id="7f463-193">V části **přihlašovací údaje správce** klikněte na tlačítko **Authorize**.</span><span class="sxs-lookup"><span data-stu-id="7f463-193">Under the **Admin Credentials** section, click **Authorize**.</span></span> <span data-ttu-id="7f463-194">Otevře se dialogové okno Google Apps autorizace v nové okno prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="7f463-194">It opens a Google Apps authorization dialog in a new browser window.</span></span>

13. <span data-ttu-id="7f463-195">Potvrďte, že byste chtěli poskytnout Azure Active Directory oprávnění k provádění změn pro vašeho klienta Google Apps.</span><span class="sxs-lookup"><span data-stu-id="7f463-195">Confirm that you would like to give Azure Active Directory permission to make changes to your Google Apps tenant.</span></span> <span data-ttu-id="7f463-196">Klikněte na tlačítko **přijmout**.</span><span class="sxs-lookup"><span data-stu-id="7f463-196">Click **Accept**.</span></span>
    
     ![Zkontrolujte oprávnění.][28]

14. <span data-ttu-id="7f463-198">Na portálu Azure klikněte na tlačítko **Test připojení** zajistit Azure AD může připojit k aplikaci Google Apps.</span><span class="sxs-lookup"><span data-stu-id="7f463-198">In the Azure portal, click **Test Connection** to ensure Azure AD can connect to your Google Apps app.</span></span> <span data-ttu-id="7f463-199">Pokud se nepovede připojit, ujistěte se, váš účet Google Apps má oprávnění správce týmu a zkuste to **"Ověřit"** krok opakujte.</span><span class="sxs-lookup"><span data-stu-id="7f463-199">If the connection fails, ensure your Google Apps account has Team Admin permissions and try the **"Authorize"** step again.</span></span>

15. <span data-ttu-id="7f463-200">Zadejte e-mailovou adresu uživatele nebo skupiny, který by měly dostávat oznámení zřizování Chyba v **e-mailové oznámení** pole a zaškrtnutím políčka.</span><span class="sxs-lookup"><span data-stu-id="7f463-200">Enter the email address of a person or group who should receive provisioning error notifications in the **Notification Email** field, and check the checkbox.</span></span>

16. <span data-ttu-id="7f463-201">Klikněte na tlačítko **uložit.**</span><span class="sxs-lookup"><span data-stu-id="7f463-201">Click **Save.**</span></span>

17. <span data-ttu-id="7f463-202">V části mapování vyberte **synchronizaci Azure Active Directory uživatelům Google Apps.**</span><span class="sxs-lookup"><span data-stu-id="7f463-202">Under the Mappings section, select **Synchronize Azure Active Directory Users to Google Apps.**</span></span>

18. <span data-ttu-id="7f463-203">V **mapování atributů** , projděte si uživatelské atributy, které jsou synchronizované z Azure AD Google Apps.</span><span class="sxs-lookup"><span data-stu-id="7f463-203">In the **Attribute Mappings** section, review the user attributes that are synchronized from Azure AD to Google Apps.</span></span> <span data-ttu-id="7f463-204">Atributy vybrán jako **párování** vlastnosti se používají tak, aby odpovídaly uživatelské účty v Google Apps pro operace aktualizace.</span><span class="sxs-lookup"><span data-stu-id="7f463-204">The attributes selected as **Matching** properties are used to match the user accounts in Google Apps for update operations.</span></span> <span data-ttu-id="7f463-205">Kliknutím na tlačítko Uložit potvrzení změny.</span><span class="sxs-lookup"><span data-stu-id="7f463-205">Select the Save button to commit any changes.</span></span>

19. <span data-ttu-id="7f463-206">Chcete-li povolit zřizování pro Google Apps služby Azure AD, změňte **Stav zřizování** k **na** v sekci nastavení</span><span class="sxs-lookup"><span data-stu-id="7f463-206">To enable the Azure AD provisioning service for Google Apps, change the **Provisioning Status** to **On** in the Settings section</span></span>

20. <span data-ttu-id="7f463-207">Klikněte na tlačítko **uložit.**</span><span class="sxs-lookup"><span data-stu-id="7f463-207">Click **Save.**</span></span>

<span data-ttu-id="7f463-208">Spustí počáteční synchronizaci všech uživatelů a skupiny přiřazené ke Google Apps v části Uživatelé a skupiny.</span><span class="sxs-lookup"><span data-stu-id="7f463-208">It starts the initial synchronization of any users and/or groups assigned to Google Apps in the Users and Groups section.</span></span> <span data-ttu-id="7f463-209">Počáteční synchronizace trvá déle než následné synchronizace, ke kterým dochází přibližně každých 20 minut, dokud se službou provést.</span><span class="sxs-lookup"><span data-stu-id="7f463-209">The initial sync takes longer to perform than subsequent syncs, which occur approximately every 20 minutes as long as the service is running.</span></span> <span data-ttu-id="7f463-210">Můžete použít **podrobnosti synchronizace** části monitorovat průběh a odkazech zřízení sestavy aktivity, které popisují všechny akce prováděné při zřizování služby ve vaší aplikaci Google Apps.</span><span class="sxs-lookup"><span data-stu-id="7f463-210">You can use the **Synchronization Details** section to monitor progress and follow links to provisioning activity reports, which describe all actions performed by the provisioning service on your Google Apps app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7f463-211">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="7f463-211">Additional resources</span></span>

* [<span data-ttu-id="7f463-212">Správa uživatelů zřizování účtu pro podnikové aplikace</span><span class="sxs-lookup"><span data-stu-id="7f463-212">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="7f463-213">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="7f463-213">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="7f463-214">Konfigurovat jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="7f463-214">Configure Single Sign-on</span></span>](active-directory-saas-google-apps-tutorial.md)



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