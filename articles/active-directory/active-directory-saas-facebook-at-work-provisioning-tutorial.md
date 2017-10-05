---
title: "Kurz: Konfigurace síti na pracovišti ve službě Facebook pro zřizování uživatelů | Microsoft Docs"
description: "Naučte se automaticky zřizovat a zrušte zřízení uživatelských účtů ze služby Azure AD k firemní síti pomocí sítě Facebook."
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
ms.openlocfilehash: a347eedbf5511dc83e1bc7721667441cfb87cb59
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="tutorial-configure-workplace-by-facebook-for-user-provisioning"></a><span data-ttu-id="9a62e-103">Kurz: Konfigurace síti na pracovišti ve službě Facebook pro zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="9a62e-103">Tutorial: Configure Workplace by Facebook for user provisioning</span></span>

<span data-ttu-id="9a62e-104">Tento kurz popisuje kroky nutné automaticky zřizovat a deaktivace zřízení uživatelských účtů z Azure Active Directory (Azure AD) k síti na pracovišti ve službě Facebook.</span><span class="sxs-lookup"><span data-stu-id="9a62e-104">This tutorial shows you the steps necessary to automatically provision and de-provision user accounts from Azure Active Directory (Azure AD) to Workplace by Facebook.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9a62e-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="9a62e-105">Prerequisites</span></span>

<span data-ttu-id="9a62e-106">Při konfiguraci integrace Azure AD se na pracovišti ve službě Facebook, budete potřebovat následující:</span><span class="sxs-lookup"><span data-stu-id="9a62e-106">To configure Azure AD integration with Workplace by Facebook, you need the following:</span></span>

- <span data-ttu-id="9a62e-107">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="9a62e-107">An Azure AD subscription</span></span>
- <span data-ttu-id="9a62e-108">Předplatné povolené firemní síti pomocí sítě Facebook jednotné přihlašování (SSO)</span><span class="sxs-lookup"><span data-stu-id="9a62e-108">A Workplace by Facebook single sign-on (SSO) enabled subscription</span></span>

<span data-ttu-id="9a62e-109">Chcete-li otestovat kroky v tomto kurzu, postupujte podle následujících doporučení:</span><span class="sxs-lookup"><span data-stu-id="9a62e-109">To test the steps in this tutorial, follow these recommendations:</span></span>

- <span data-ttu-id="9a62e-110">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="9a62e-110">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="9a62e-111">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat [nabídka zkušební verze jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9a62e-111">If you don't have an Azure AD trial environment, you can get a [one-month trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="assign-users-to-workplace-by-facebook"></a><span data-ttu-id="9a62e-112">Přiřazení uživatelů k síti na pracovišti ve službě Facebook.</span><span class="sxs-lookup"><span data-stu-id="9a62e-112">Assign users to Workplace by Facebook</span></span>

<span data-ttu-id="9a62e-113">Azure AD používá koncept označované jako "úlohy" k určení uživatelů, kteří obdrželi přístup k vybrané aplikace.</span><span class="sxs-lookup"><span data-stu-id="9a62e-113">Azure AD uses a concept called "assignments" to determine which users should receive access to selected apps.</span></span> <span data-ttu-id="9a62e-114">V kontextu uživatele automatické zřizování účtu jsou synchronizovány pouze uživatelé a skupiny, které byly přiřazeny k aplikaci ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9a62e-114">In the context of automatic user account provisioning, only the users and groups that have been assigned to an application in Azure AD are synchronized.</span></span>

<span data-ttu-id="9a62e-115">Než nakonfigurujete a povolíte službu zřizování, rozhodněte, jaké uživatelů a skupin ve službě Azure AD představují uživatele, kteří potřebují přístup k pracovní ploše aplikace Facebook.</span><span class="sxs-lookup"><span data-stu-id="9a62e-115">Before configuring and enabling the provisioning service, decide what users and groups in Azure AD represent the users who need access to your Workplace by Facebook app.</span></span> <span data-ttu-id="9a62e-116">Lze potom přiřadit těmto uživatelům k pracovní ploše pomocí aplikace Facebook podle pokynů v [přiřadit uživatele nebo skupinu enterprise aplikace](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="9a62e-116">You can then assign these users to your Workplace by Facebook app by following the instructions in [Assign a user or group to an enterprise app](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal).</span></span>

>[!IMPORTANT]
>*   <span data-ttu-id="9a62e-117">Otestujte konfiguraci zřizování přiřazením jednoho uživatele Azure AD k firemní síti pomocí sítě Facebook.</span><span class="sxs-lookup"><span data-stu-id="9a62e-117">Test the provisioning configuration by assigning a single Azure AD user to Workplace by Facebook.</span></span> <span data-ttu-id="9a62e-118">Přiřaďte dalších uživatelů a skupin později.</span><span class="sxs-lookup"><span data-stu-id="9a62e-118">Assign additional users and groups later.</span></span>
>*   <span data-ttu-id="9a62e-119">Když přiřadíte uživatele k firemní síti pomocí sítě Facebook, musíte vybrat platné uživatelské role.</span><span class="sxs-lookup"><span data-stu-id="9a62e-119">When you assign a user to Workplace by Facebook, you must select a valid user role.</span></span> <span data-ttu-id="9a62e-120">Roli výchozí přístup nefunguje pro zřizování.</span><span class="sxs-lookup"><span data-stu-id="9a62e-120">The Default Access role does not work for provisioning.</span></span>

## <a name="enable-automated-user-provisioning"></a><span data-ttu-id="9a62e-121">Povolit zřizování automatizované uživatelů</span><span class="sxs-lookup"><span data-stu-id="9a62e-121">Enable automated user provisioning</span></span>

<span data-ttu-id="9a62e-122">Tato část vás provede připojení služby Azure AD s uživatelským účtem zřizování API pracovního místa ve službě Facebook.</span><span class="sxs-lookup"><span data-stu-id="9a62e-122">This section guides you through connecting your Azure AD to the user account provisioning API of Workplace by Facebook.</span></span> <span data-ttu-id="9a62e-123">Také zjistíte, jak nakonfigurovat službu zřizování vytvářet, aktualizovat a zakázat přiřazené uživatelské účty v síti na pracovišti ve službě Facebook.</span><span class="sxs-lookup"><span data-stu-id="9a62e-123">You also learn how to configure the provisioning service to create, update, and disable assigned user accounts in Workplace by Facebook.</span></span> <span data-ttu-id="9a62e-124">To je založené na uživatele a přiřazení skupiny ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9a62e-124">This is based on user and group assignment in Azure AD.</span></span>

>[!Tip]
><span data-ttu-id="9a62e-125">Můžete také povolit na základě SAML jednotného přihlašování pro pracoviště ve službě Facebook, pomocí postupujte podle pokynů uvedených v [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="9a62e-125">You can also choose to enabled SAML-based SSO for Workplace by Facebook, by following the instructions provided in the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="9a62e-126">Jednotné přihlašování se dá nakonfigurovat nezávisle na automatické zřizování, i když tyto dvě funkce vzájemně doplňují.</span><span class="sxs-lookup"><span data-stu-id="9a62e-126">SSO can be configured independently of automatic provisioning, though these two features complement each other.</span></span>

### <a name="configure-user-account-provisioning-to-workplace-by-facebook-in-azure-ad"></a><span data-ttu-id="9a62e-127">Konfigurace uživatelského účtu ve službě Azure AD zřizování k síti na pracovišti ve službě Facebook.</span><span class="sxs-lookup"><span data-stu-id="9a62e-127">Configure user account provisioning to Workplace by Facebook in Azure AD</span></span>

<span data-ttu-id="9a62e-128">Azure AD podporuje možnost automaticky synchronizovat Podrobnosti účtu přiřazené uživatelů k firemní síti pomocí sítě Facebook.</span><span class="sxs-lookup"><span data-stu-id="9a62e-128">Azure AD supports the ability to automatically synchronize the account details of assigned users to Workplace by Facebook.</span></span> <span data-ttu-id="9a62e-129">Toto automatické synchronizace umožňuje síti na pracovišti ve službě Facebook se získat data, je nutné autorizovat uživatele pro přístup, než je pokus o přihlášení za poprvé.</span><span class="sxs-lookup"><span data-stu-id="9a62e-129">This automatic synchronization enables Workplace by Facebook to get the data it needs to authorize users for access, before them attempting to sign in for the first time.</span></span> <span data-ttu-id="9a62e-130">Také zrušte technologii uživatelům v síti na pracovišti ve službě Facebook při odvolání přístupu ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9a62e-130">It also de-provisions users from Workplace by Facebook when access has been revoked in Azure AD.</span></span>

1. <span data-ttu-id="9a62e-131">V [portál Azure](https://portal.azure.com), vyberte **Azure Active Directory** > **podnikové aplikace** > **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="9a62e-131">In the [Azure portal](https://portal.azure.com), select **Azure Active Directory** > **Enterprise Apps** > **All applications**.</span></span>

2. <span data-ttu-id="9a62e-132">Pokud jste již nakonfigurovali síti na pracovišti ve službě Facebook pro jednotné přihlašování, vyhledejte pomocí pole hledání instanci síti na pracovišti ve službě Facebook.</span><span class="sxs-lookup"><span data-stu-id="9a62e-132">If you have already configured Workplace by Facebook for SSO, search for your instance of Workplace by Facebook by using the search field.</span></span> <span data-ttu-id="9a62e-133">Jinak vyberte možnost **přidat** a vyhledejte **síti na pracovišti ve službě Facebook** v galerii aplikací.</span><span class="sxs-lookup"><span data-stu-id="9a62e-133">Otherwise, select **Add** and search for **Workplace by Facebook** in the application gallery.</span></span> <span data-ttu-id="9a62e-134">Vyberte **síti na pracovišti ve službě Facebook** ve výsledcích hledání a přidejte ji do seznamu aplikací.</span><span class="sxs-lookup"><span data-stu-id="9a62e-134">Select **Workplace by Facebook** from the search results, and add it to your list of applications.</span></span>

3. <span data-ttu-id="9a62e-135">Vyberte instanci síti na pracovišti ve službě Facebook a pak vyberte **zřizování** kartě.</span><span class="sxs-lookup"><span data-stu-id="9a62e-135">Select your instance of Workplace by Facebook, and then select the **Provisioning** tab.</span></span>

4. <span data-ttu-id="9a62e-136">Nastavit **režimu zřizování** k **automatické**.</span><span class="sxs-lookup"><span data-stu-id="9a62e-136">Set **Provisioning Mode** to **Automatic**.</span></span> 

    ![Snímek obrazovky pracovního místa ve službě Facebook možnosti zřizování](./media/active-directory-saas-facebook-at-work-provisioning-tutorial/provisioning.png)

5. <span data-ttu-id="9a62e-138">V části **přihlašovací údaje správce** zadejte **tajný klíč tokenu** a **URL klienta** ze svého pracoviště správcem sítě Facebook.</span><span class="sxs-lookup"><span data-stu-id="9a62e-138">Under the **Admin Credentials** section, enter the **Secret Token** and the **Tenant URL** of your Workplace by Facebook administrator.</span></span>

6. <span data-ttu-id="9a62e-139">Na portálu Azure vyberte **Test připojení** zajistit Azure AD může připojit k pracovní ploše aplikace Facebook.</span><span class="sxs-lookup"><span data-stu-id="9a62e-139">In the Azure portal, select **Test Connection** to ensure Azure AD can connect to your Workplace by Facebook app.</span></span> <span data-ttu-id="9a62e-140">Pokud se nepovede připojit, zajistěte, aby vaše pracoviště pomocí účtu sítě Facebook oprávnění správce týmu.</span><span class="sxs-lookup"><span data-stu-id="9a62e-140">If the connection fails, ensure that your Workplace by Facebook account has Team Admin permissions.</span></span>

7. <span data-ttu-id="9a62e-141">Zadejte e-mailovou adresu uživatele nebo skupiny, který by měly dostávat oznámení zřizování Chyba v **e-mailové oznámení** pole a zaškrtnutím políčka.</span><span class="sxs-lookup"><span data-stu-id="9a62e-141">Enter the email address of a person or group who should receive provisioning error notifications in the **Notification Email** field, and check the check box.</span></span>

8. <span data-ttu-id="9a62e-142">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="9a62e-142">Select **Save**.</span></span>

9. <span data-ttu-id="9a62e-143">V části mapování vyberte **synchronizaci Azure Active Directory Users k firemní síti pomocí sítě Facebook**.</span><span class="sxs-lookup"><span data-stu-id="9a62e-143">Under the Mappings section, select **Synchronize Azure Active Directory Users to Workplace by Facebook**.</span></span>

10. <span data-ttu-id="9a62e-144">V **mapování atributů** , projděte si uživatelské atributy, které jsou synchronizovány z Azure AD k firemní síti pomocí sítě Facebook.</span><span class="sxs-lookup"><span data-stu-id="9a62e-144">In the **Attribute Mappings** section, review the user attributes that are synchronized from Azure AD to Workplace by Facebook.</span></span> <span data-ttu-id="9a62e-145">Atributy vybrán jako **párování** vlastnosti jsou používány tak, aby odpovídaly uživatelské účty v síti na pracovišti Facebook pro operace aktualizace.</span><span class="sxs-lookup"><span data-stu-id="9a62e-145">The attributes selected as **Matching** properties are used to match the user accounts in Workplace by Facebook for update operations.</span></span> <span data-ttu-id="9a62e-146">Všechny změny potvrdit, vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="9a62e-146">To commit any changes, select **Save**.</span></span>

11. <span data-ttu-id="9a62e-147">Chcete-li povolit zřizování služby pro pracoviště ve službě Facebook, v Azure AD **nastavení** změňte **Stav zřizování** k **na**.</span><span class="sxs-lookup"><span data-stu-id="9a62e-147">To enable the Azure AD provisioning service for Workplace by Facebook, in the **Settings** section, change the **Provisioning Status** to **On**.</span></span>

12. <span data-ttu-id="9a62e-148">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="9a62e-148">Select **Save**.</span></span>

<span data-ttu-id="9a62e-149">Další informace o tom, jak nakonfigurovat automatické zřizování najdete v tématu [dokumentace Facebook](https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers).</span><span class="sxs-lookup"><span data-stu-id="9a62e-149">For more information on how to configure automatic provisioning, see [the Facebook documentation](https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers).</span></span>

<span data-ttu-id="9a62e-150">Nyní můžete vytvořit testovací účet.</span><span class="sxs-lookup"><span data-stu-id="9a62e-150">You can now create a test account.</span></span> <span data-ttu-id="9a62e-151">Chcete-li ověřit, že účet byl synchronizován k síti na pracovišti ve službě Facebook Počkejte až 20 minut.</span><span class="sxs-lookup"><span data-stu-id="9a62e-151">Wait for up to 20 minutes to verify that the account has been synchronized to Workplace by Facebook.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9a62e-152">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="9a62e-152">Additional resources</span></span>

* [<span data-ttu-id="9a62e-153">Správa uživatelů zřizování účtu pro podnikové aplikace</span><span class="sxs-lookup"><span data-stu-id="9a62e-153">Managing user account provisioning for enterprise apps</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="9a62e-154">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="9a62e-154">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="9a62e-155">Konfigurovat jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="9a62e-155">Configure single sign-on</span></span>](active-directory-saas-facebook-at-work-tutorial.md)

