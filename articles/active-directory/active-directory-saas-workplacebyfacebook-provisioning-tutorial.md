---
title: "Kurz: Azure Active Directory integrace s síti na pracovišti ve službě Facebook. | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a síti na pracovišti ve službě Facebook."
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
ms.openlocfilehash: 9b22679c304248ed7ba7a6bd9eaf82b64f7143cf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-configuring-workplace-by-facebook-for-user-provisioning"></a><span data-ttu-id="284dd-103">Kurz: Konfigurace síti na pracovišti ve službě Facebook pro zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="284dd-103">Tutorial: Configuring Workplace by Facebook for User Provisioning</span></span>

<span data-ttu-id="284dd-104">Cílem tohoto kurzu je tak, aby zobrazovalo kroky, které je třeba provést v síti na pracovišti Facebook a Azure AD a automaticky zřizovat a zrušte zřízení uživatelských účtů ze služby Azure AD k firemní síti pomocí sítě Facebook.</span><span class="sxs-lookup"><span data-stu-id="284dd-104">The objective of this tutorial is to show you the steps you need to perform in Workplace by Facebook and Azure AD to automatically provision and de-provision user accounts from Azure AD to Workplace by Facebook.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="284dd-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="284dd-105">Prerequisites</span></span>

<span data-ttu-id="284dd-106">Ke konfiguraci integrace služby Azure AD se na pracovišti ve službě Facebook, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="284dd-106">To configure Azure AD integration with Workplace by Facebook, you need the following items:</span></span>

- <span data-ttu-id="284dd-107">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="284dd-107">An Azure AD subscription</span></span>
- <span data-ttu-id="284dd-108">Firemní síti pomocí sítě Facebook jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="284dd-108">A Workplace by Facebook single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="284dd-109">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="284dd-109">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="284dd-110">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="284dd-110">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="284dd-111">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="284dd-111">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="284dd-112">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="284dd-112">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="assigning-users-to-workplace-by-facebook"></a><span data-ttu-id="284dd-113">Přiřazování uživatelů k síti na pracovišti ve službě Facebook.</span><span class="sxs-lookup"><span data-stu-id="284dd-113">Assigning users to Workplace by Facebook</span></span>

<span data-ttu-id="284dd-114">Azure Active Directory používá koncept označované jako "úlohy" k určení uživatelů, kteří obdrželi přístup k vybrané aplikace.</span><span class="sxs-lookup"><span data-stu-id="284dd-114">Azure Active Directory uses a concept called "assignments" to determine which users should receive access to selected apps.</span></span> <span data-ttu-id="284dd-115">V kontextu uživatele automatické zřizování účtu se synchronizují pouze uživatelé a skupiny, které byly "přiřazeny" aplikace ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="284dd-115">In the context of automatic user account provisioning, only the users and groups that have been "assigned" to an application in Azure AD is synchronized.</span></span>

<span data-ttu-id="284dd-116">Před konfigurací a povolení zřizování služby, musíte rozhodnout, jaké uživatelů nebo skupin ve službě Azure AD představují uživatele, kteří potřebují přístup k pracovní ploše aplikace Facebook.</span><span class="sxs-lookup"><span data-stu-id="284dd-116">Before configuring and enabling the provisioning service, you need to decide what users and/or groups in Azure AD represent the users who need access to your Workplace by Facebook app.</span></span> <span data-ttu-id="284dd-117">Jakmile se rozhodli, můžete přiřadit těmto uživatelům k pracovní ploše aplikace Facebook podle pokynů tady:</span><span class="sxs-lookup"><span data-stu-id="284dd-117">Once decided, you can assign these users to your Workplace by Facebook app by following the instructions here:</span></span>

[<span data-ttu-id="284dd-118">Přiřazení uživatele nebo skupiny do aplikace enterprise</span><span class="sxs-lookup"><span data-stu-id="284dd-118">Assign a user or group to an enterprise app</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-workplace-by-facebook"></a><span data-ttu-id="284dd-119">Důležité tipy pro přiřazování uživatelů k síti na pracovišti ve službě Facebook.</span><span class="sxs-lookup"><span data-stu-id="284dd-119">Important tips for assigning users to Workplace by Facebook</span></span>

*   <span data-ttu-id="284dd-120">Dále je doporučeno jednoho uživatele Azure AD je přiřazena k síti na pracovišti ve službě Facebook otestovat konfiguraci zřizování.</span><span class="sxs-lookup"><span data-stu-id="284dd-120">It is recommended that a single Azure AD user is assigned to Workplace by Facebook to test the provisioning configuration.</span></span> <span data-ttu-id="284dd-121">Další uživatele nebo skupiny může být přiřazen později.</span><span class="sxs-lookup"><span data-stu-id="284dd-121">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="284dd-122">Při přiřazení uživatele k firemní síti pomocí sítě Facebook, musíte vybrat platné uživatelské role.</span><span class="sxs-lookup"><span data-stu-id="284dd-122">When assigning a user to Workplace by Facebook, you must select a valid user role.</span></span> <span data-ttu-id="284dd-123">Roli "Výchozí přístup" nefunguje pro zřizování.</span><span class="sxs-lookup"><span data-stu-id="284dd-123">The "Default Access" role does not work for provisioning.</span></span>

## <a name="enable-user-provisioning"></a><span data-ttu-id="284dd-124">Povolit zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="284dd-124">Enable User Provisioning</span></span>

<span data-ttu-id="284dd-125">Tato část vás provede připojení k síti na pracovišti uživatelský účet na Facebooku pro zřizování rozhraní API služby Azure AD a konfiguraci zřizování službu, kterou chcete vytvořit, aktualizovat a zakažte přiřazené uživatelské účty v síti na pracovišti ve službě Facebook podle přiřazení uživatelů a skupin ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="284dd-125">This section guides you through connecting your Azure AD to Workplace by Facebook's user account provisioning API, and configuring the provisioning service to create, update, and disable assigned user accounts in Workplace by Facebook based on user and group assignment in Azure AD.</span></span>

>[!Tip]
><span data-ttu-id="284dd-126">Můžete také povolit na základě SAML jednotné přihlašování pro pracoviště ve službě Facebook, postupujte podle pokynů uvedených v [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="284dd-126">You may also choose to enabled SAML-based Single Sign-On for Workplace by Facebook, following the instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="284dd-127">Jednotné přihlašování se dá nakonfigurovat nezávisle na automatické zřizování, i když tyto dvě funkce doplnění navzájem.</span><span class="sxs-lookup"><span data-stu-id="284dd-127">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>

### <a name="to-configure-user-account-provisioning-to-workplace-by-facebook-in-azure-ad"></a><span data-ttu-id="284dd-128">Konfigurace účtu zřizování uživatelů k firemní síti pomocí sítě Facebook v Azure AD:</span><span class="sxs-lookup"><span data-stu-id="284dd-128">To configure user account provisioning to Workplace by Facebook in Azure AD:</span></span>

<span data-ttu-id="284dd-129">Cílem této části se popisují postup povolení zřizování uživatelských účtů služby Active Directory k firemní síti pomocí sítě Facebook.</span><span class="sxs-lookup"><span data-stu-id="284dd-129">The objective of this section is to outline how to enable provisioning of Active Directory user accounts to Workplace by Facebook.</span></span>

<span data-ttu-id="284dd-130">Azure AD podporuje možnost automaticky synchronizovat Podrobnosti účtu přiřazené uživatelů k firemní síti pomocí sítě Facebook.</span><span class="sxs-lookup"><span data-stu-id="284dd-130">Azure AD supports the ability to automatically synchronize the account details of assigned users to Workplace by Facebook.</span></span> <span data-ttu-id="284dd-131">Toto automatické synchronizace umožňuje síti na pracovišti ve službě Facebook se získat data, je nutné autorizovat uživatele pro přístup, než je pokus o přihlášení za poprvé.</span><span class="sxs-lookup"><span data-stu-id="284dd-131">This automatic synchronization enables Workplace by Facebook to get the data it needs to authorize users for access, before them attempting to sign in for the first time.</span></span> <span data-ttu-id="284dd-132">Také zrušte technologii uživatelům v síti na pracovišti ve službě Facebook při odvolání přístupu ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="284dd-132">It also de-provisions users from Workplace by Facebook when access has been revoked in Azure AD.</span></span>

1. <span data-ttu-id="284dd-133">V [portál Azure](https://portal.azure.com), vyhledejte **Azure Active Directory** > **podnikové aplikace** > **všechny aplikace** části.</span><span class="sxs-lookup"><span data-stu-id="284dd-133">In the [Azure portal](https://portal.azure.com), browse to the **Azure Active Directory** > **Enterprise Apps** > **All applications** section.</span></span>

2. <span data-ttu-id="284dd-134">Pokud jste již nakonfigurovali síti na pracovišti ve službě Facebook pro jednotné přihlašování, vyhledejte instanci síti na pracovišti ve službě Facebook pomocí pole hledání.</span><span class="sxs-lookup"><span data-stu-id="284dd-134">If you have already configured Workplace by Facebook for single sign-on, search for your instance of Workplace by Facebook using the search field.</span></span> <span data-ttu-id="284dd-135">Jinak vyberte možnost **přidat** a vyhledejte **síti na pracovišti ve službě Facebook** v galerii aplikací.</span><span class="sxs-lookup"><span data-stu-id="284dd-135">Otherwise, select **Add** and search for **Workplace by Facebook** in the application gallery.</span></span> <span data-ttu-id="284dd-136">Vyberte síti na pracovišti ve službě Facebook ve výsledcích hledání a přidejte ji do seznamu aplikací.</span><span class="sxs-lookup"><span data-stu-id="284dd-136">Select Workplace by Facebook from the search results, and add it to your list of applications.</span></span>

3. <span data-ttu-id="284dd-137">Vyberte instanci síti na pracovišti ve službě Facebook a pak vyberte **zřizování** kartě.</span><span class="sxs-lookup"><span data-stu-id="284dd-137">Select your instance of Workplace by Facebook, then select the **Provisioning** tab.</span></span>

4. <span data-ttu-id="284dd-138">Nastavte **režimu zřizování** k **automatické**.</span><span class="sxs-lookup"><span data-stu-id="284dd-138">Set the **Provisioning Mode** to **Automatic**.</span></span> 

    ![Zřizování](./media/active-directory-saas-workplacebyfacebook-provisioning-tutorial/provisioning.png)

5. <span data-ttu-id="284dd-140">V části **přihlašovací údaje správce** zadejte tajný klíč tokenu a URL klienta ze svého pracoviště správcem sítě Facebook.</span><span class="sxs-lookup"><span data-stu-id="284dd-140">Under the **Admin Credentials** section, enter the Secret Token and the Tenant URL of your Workplace by Facebook administrator.</span></span>

6. <span data-ttu-id="284dd-141">Na portálu Azure klikněte na tlačítko **Test připojení** zajistit Azure AD může připojit k pracovní ploše aplikace Facebook.</span><span class="sxs-lookup"><span data-stu-id="284dd-141">In the Azure portal, click **Test Connection** to ensure Azure AD can connect to your Workplace by Facebook app.</span></span> <span data-ttu-id="284dd-142">Pokud se nepovede připojit, zajistěte, aby že vaše pracoviště Facebook účet má oprávnění správce týmu.</span><span class="sxs-lookup"><span data-stu-id="284dd-142">If the connection fails, ensure your Workplace by Facebook account has Team Admin permissions.</span></span>

7. <span data-ttu-id="284dd-143">Zadejte e-mailovou adresu uživatele nebo skupiny, který by měly dostávat oznámení zřizování Chyba v **e-mailové oznámení** pole a zaškrtnutím políčka.</span><span class="sxs-lookup"><span data-stu-id="284dd-143">Enter the email address of a person or group who should receive provisioning error notifications in the **Notification Email** field, and check the checkbox.</span></span>

8. <span data-ttu-id="284dd-144">Klikněte na tlačítko **uložit.**</span><span class="sxs-lookup"><span data-stu-id="284dd-144">Click **Save.**</span></span>

9. <span data-ttu-id="284dd-145">V části mapování vyberte **synchronizaci Azure Active Directory Users k firemní síti pomocí sítě Facebook.**</span><span class="sxs-lookup"><span data-stu-id="284dd-145">Under the Mappings section, select **Synchronize Azure Active Directory Users to Workplace by Facebook.**</span></span>

10. <span data-ttu-id="284dd-146">V **mapování atributů** , projděte si uživatelské atributy, které jsou synchronizovány z Azure AD k firemní síti pomocí sítě Facebook.</span><span class="sxs-lookup"><span data-stu-id="284dd-146">In the **Attribute Mappings** section, review the user attributes that are synchronized from Azure AD to Workplace by Facebook.</span></span> <span data-ttu-id="284dd-147">Atributy vybrán jako **párování** vlastnosti jsou používány tak, aby odpovídaly uživatelské účty v síti na pracovišti Facebook pro operace aktualizace.</span><span class="sxs-lookup"><span data-stu-id="284dd-147">The attributes selected as **Matching** properties are used to match the user accounts in Workplace by Facebook for update operations.</span></span> <span data-ttu-id="284dd-148">Kliknutím na tlačítko Uložit potvrzení změny.</span><span class="sxs-lookup"><span data-stu-id="284dd-148">Select the Save button to commit any changes.</span></span>

11. <span data-ttu-id="284dd-149">Povolit Azure AD zřizování služby pro pracoviště ve službě Facebook, změňte **Stav zřizování** k **na** v **nastavení** části</span><span class="sxs-lookup"><span data-stu-id="284dd-149">To enable the Azure AD provisioning service for Workplace by Facebook, change the **Provisioning Status** to **On** in the **Settings** section</span></span>

12. <span data-ttu-id="284dd-150">Klikněte na tlačítko **uložit.**</span><span class="sxs-lookup"><span data-stu-id="284dd-150">Click **Save.**</span></span>

<span data-ttu-id="284dd-151">Další informace o tom, jak nakonfigurovat automatické zřizování najdete v tématu [https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers](https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers)</span><span class="sxs-lookup"><span data-stu-id="284dd-151">For more information on how to configure automatic provisioning, see [https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers](https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers)</span></span>

<span data-ttu-id="284dd-152">Nyní můžete vytvořit testovací účet.</span><span class="sxs-lookup"><span data-stu-id="284dd-152">You can now create a test account.</span></span> <span data-ttu-id="284dd-153">Chcete-li ověřit, že účet byl synchronizován k síti na pracovišti ve službě Facebook Počkejte až 20 minut.</span><span class="sxs-lookup"><span data-stu-id="284dd-153">Wait for up to 20 minutes to verify that the account has been synchronized to Workplace by Facebook.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="284dd-154">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="284dd-154">Additional resources</span></span>

* [<span data-ttu-id="284dd-155">Správa uživatelů zřizování účtu pro podnikové aplikace</span><span class="sxs-lookup"><span data-stu-id="284dd-155">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="284dd-156">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="284dd-156">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="284dd-157">Konfigurovat jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="284dd-157">Configure Single Sign-on</span></span>](active-directory-saas-workplacebyfacebook-tutorial.md)

