---
title: "Vytvoření identity pro aplikaci Azure portálu | Microsoft Docs"
description: "Popisuje, jak vytvořit novou aplikaci Azure Active Directory a objektu služby, které je možné pomocí řízení přístupu na základě rolí ve službě Správce prostředků Azure, které pokud chcete spravovat přístup k prostředkům."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 7068617b-ac5e-47b3-a1de-a18c918297b6
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/15/2017
ms.author: tomfitz
ms.openlocfilehash: 5d24fb99e1095d53e5ea547e53b80178d9cb77c0
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="use-portal-to-create-an-azure-active-directory-application-and-service-principal-that-can-access-resources"></a><span data-ttu-id="ae54d-103">Vytvoření aplikace Azure Active Directory a objektu služby, které mají přístup k prostředkům pomocí portálu</span><span class="sxs-lookup"><span data-stu-id="ae54d-103">Use portal to create an Azure Active Directory application and service principal that can access resources</span></span>

<span data-ttu-id="ae54d-104">Když máte aplikaci, která potřebuje přístup k nebo úpravám prostředků, musíte nastavit aplikaci Azure Active Directory (AD) a přiřadit požadovaná oprávnění.</span><span class="sxs-lookup"><span data-stu-id="ae54d-104">When you have an application that needs to access or modify resources, you must set up an Azure Active Directory (AD) application and assign the required permissions to it.</span></span> <span data-ttu-id="ae54d-105">Tento postup je vhodnější spuštění aplikace vlastní oprávnění, protože:</span><span class="sxs-lookup"><span data-stu-id="ae54d-105">This approach is preferable to running the app under your own credentials because:</span></span>

* <span data-ttu-id="ae54d-106">Můžete přiřadit oprávnění k identitě aplikace, která se liší od vlastní oprávnění.</span><span class="sxs-lookup"><span data-stu-id="ae54d-106">You can assign permissions to the app identity that are different than your own permissions.</span></span> <span data-ttu-id="ae54d-107">Tato oprávnění jsou obvykle omezené na přesně co aplikaci je třeba provést.</span><span class="sxs-lookup"><span data-stu-id="ae54d-107">Typically, these permissions are restricted to exactly what the app needs to do.</span></span>
* <span data-ttu-id="ae54d-108">Nemáte ke změně pověření aplikace, pokud vaše odpovědnosti změnit.</span><span class="sxs-lookup"><span data-stu-id="ae54d-108">You do not have to change the app's credentials if your responsibilities change.</span></span> 
* <span data-ttu-id="ae54d-109">Certifikát můžete použít k automatizaci ověřování při provádění bezobslužného skriptu.</span><span class="sxs-lookup"><span data-stu-id="ae54d-109">You can use a certificate to automate authentication when executing an unattended script.</span></span>

<span data-ttu-id="ae54d-110">Toto téma ukazuje, jak provádět tyto kroky prostřednictvím portálu.</span><span class="sxs-lookup"><span data-stu-id="ae54d-110">This topic shows you how to perform those steps through the portal.</span></span> <span data-ttu-id="ae54d-111">Zaměřuje se na jednoho klienta aplikace, kde je záměrem spustit v rámci organizace jenom jedna aplikace.</span><span class="sxs-lookup"><span data-stu-id="ae54d-111">It focuses on a single-tenant application where the application is intended to run within only one organization.</span></span> <span data-ttu-id="ae54d-112">Obvykle používají aplikace jednoho klienta pro-obchodní aplikace, které běží v rámci vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="ae54d-112">You typically use single-tenant applications for line-of-business applications that run within your organization.</span></span>
 
## <a name="required-permissions"></a><span data-ttu-id="ae54d-113">Požadovaná oprávnění</span><span class="sxs-lookup"><span data-stu-id="ae54d-113">Required permissions</span></span>
<span data-ttu-id="ae54d-114">K dokončení tohoto tématu, musíte mít dostatečná oprávnění k registraci aplikace ve službě klientovi Azure AD a přiřazení aplikace k roli ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="ae54d-114">To complete this topic, you must have sufficient permissions to register an application with your Azure AD tenant, and assign the application to a role in your Azure subscription.</span></span> <span data-ttu-id="ae54d-115">Ujistíme se, že máte správná oprávnění k provedení těchto kroků.</span><span class="sxs-lookup"><span data-stu-id="ae54d-115">Let's make sure you have the right permissions to perform those steps.</span></span>

### <a name="check-azure-active-directory-permissions"></a><span data-ttu-id="ae54d-116">Zkontrolujte oprávnění služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ae54d-116">Check Azure Active Directory permissions</span></span>
1. <span data-ttu-id="ae54d-117">Přihlaste se k účtu Azure prostřednictvím [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="ae54d-117">Log in to your Azure Account through the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="ae54d-118">Vyberte **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="ae54d-118">Select **Azure Active Directory**.</span></span>

     ![Vyberte služby azure active directory](./media/resource-group-create-service-principal-portal/select-active-directory.png)
3. <span data-ttu-id="ae54d-120">V Azure Active Directory, vyberte **uživatelská nastavení**.</span><span class="sxs-lookup"><span data-stu-id="ae54d-120">In Azure Active Directory, select **User settings**.</span></span>

     ![Vyberte nastavení uživatele](./media/resource-group-create-service-principal-portal/select-user-settings.png)
4. <span data-ttu-id="ae54d-122">Zkontrolujte **registrace aplikace** nastavení.</span><span class="sxs-lookup"><span data-stu-id="ae54d-122">Check the **App registrations** setting.</span></span> <span data-ttu-id="ae54d-123">Pokud nastavena na **Ano**, uživatelé bez oprávnění správce můžete zaregistrovat aplikace AD.</span><span class="sxs-lookup"><span data-stu-id="ae54d-123">If set to **Yes**, non-admin users can register AD apps.</span></span> <span data-ttu-id="ae54d-124">Toto nastavení znamená, že každý uživatel v klientovi Azure AD můžete zaregistrovat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ae54d-124">This setting means any user in the Azure AD tenant can register an app.</span></span> <span data-ttu-id="ae54d-125">Můžete přejít k [oprávnění předplatné Azure zkontrolujte](#check-azure-subscription-permissions).</span><span class="sxs-lookup"><span data-stu-id="ae54d-125">You can proceed to [Check Azure subscription permissions](#check-azure-subscription-permissions).</span></span>

     ![Zobrazit registrace aplikace](./media/resource-group-create-service-principal-portal/view-app-registrations.png)
5. <span data-ttu-id="ae54d-127">Pokud registrace aplikace, nastavení je nastavený na **ne**jen správce uživatelé mohou registrovat aplikace.</span><span class="sxs-lookup"><span data-stu-id="ae54d-127">If the app registrations setting is set to **No**, only admin users can register apps.</span></span> <span data-ttu-id="ae54d-128">Je třeba zkontrolovat, jestli je váš účet správce pro tenanta Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ae54d-128">You need to check whether your account is an admin for the Azure AD tenant.</span></span> <span data-ttu-id="ae54d-129">Vyberte **přehled** a **najít uživatele** z rychlé úlohy.</span><span class="sxs-lookup"><span data-stu-id="ae54d-129">Select **Overview** and **Find a user** from Quick tasks.</span></span>

     ![Najít uživatele](./media/resource-group-create-service-principal-portal/find-user.png)
6. <span data-ttu-id="ae54d-131">Vyhledávání pro váš účet a vyberte ho, když ji najít.</span><span class="sxs-lookup"><span data-stu-id="ae54d-131">Search for your account, and select it when you find it.</span></span>

     ![Hledat uživatele](./media/resource-group-create-service-principal-portal/show-user.png)
7. <span data-ttu-id="ae54d-133">Pro váš účet, vyberte **Directory role**.</span><span class="sxs-lookup"><span data-stu-id="ae54d-133">For your account, select **Directory role**.</span></span> 

     ![role adresáře](./media/resource-group-create-service-principal-portal/select-directory-role.png)
8. <span data-ttu-id="ae54d-135">Zobrazte vaše role přiřazené adresář ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ae54d-135">View your assigned directory role in Azure AD.</span></span> <span data-ttu-id="ae54d-136">Pokud je váš účet přiřazenou roli uživatele, ale nastavení registrace aplikace (z předchozích kroků) je omezený na správci, požádejte správce, aby buď přiřadit k roli správce, nebo umožňuje uživatelům registrovat aplikace.</span><span class="sxs-lookup"><span data-stu-id="ae54d-136">If your account is assigned to the User role, but the app registration setting (from the preceding steps) is limited to admin users, ask your administrator to either assign you to an administrator role, or to enable users to register apps.</span></span>

     ![role zobrazení](./media/resource-group-create-service-principal-portal/view-role.png)

### <a name="check-azure-subscription-permissions"></a><span data-ttu-id="ae54d-138">Zkontrolujte oprávnění předplatného Azure</span><span class="sxs-lookup"><span data-stu-id="ae54d-138">Check Azure subscription permissions</span></span>
<span data-ttu-id="ae54d-139">Ve vašem předplatném Azure, musí mít váš účet `Microsoft.Authorization/*/Write` přístup k aplikaci AD přiřadit roli.</span><span class="sxs-lookup"><span data-stu-id="ae54d-139">In your Azure subscription, your account must have `Microsoft.Authorization/*/Write` access to assign an AD app to a role.</span></span> <span data-ttu-id="ae54d-140">Tato akce je poskytována prostřednictvím [vlastníka](../active-directory/role-based-access-built-in-roles.md#owner) role nebo [správce přístupu uživatelů](../active-directory/role-based-access-built-in-roles.md#user-access-administrator) role.</span><span class="sxs-lookup"><span data-stu-id="ae54d-140">This action is granted through the [Owner](../active-directory/role-based-access-built-in-roles.md#owner) role or [User Access Administrator](../active-directory/role-based-access-built-in-roles.md#user-access-administrator) role.</span></span> <span data-ttu-id="ae54d-141">Pokud váš účet je přiřazen k **Přispěvatel** role, nemáte dostatečná oprávnění.</span><span class="sxs-lookup"><span data-stu-id="ae54d-141">If your account is assigned to the **Contributor** role, you do not have adequate permission.</span></span> <span data-ttu-id="ae54d-142">Obdržíte chybu při pokusu o přiřazení objektu služby k roli.</span><span class="sxs-lookup"><span data-stu-id="ae54d-142">You will receive an error when attempting to assign the service principal to a role.</span></span> 

<span data-ttu-id="ae54d-143">Zkontrolujte oprávnění svého předplatného:</span><span class="sxs-lookup"><span data-stu-id="ae54d-143">To check your subscription permissions:</span></span>

1. <span data-ttu-id="ae54d-144">Nejsou-li již zobrazena v účtu Azure AD z předchozích kroků, vyberte **Azure Active Directory** v levém podokně.</span><span class="sxs-lookup"><span data-stu-id="ae54d-144">If you are not already looking at your Azure AD account from the preceding steps, select **Azure Active Directory** from the left pane.</span></span>

2. <span data-ttu-id="ae54d-145">Najít váš účet Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ae54d-145">Find your Azure AD account.</span></span> <span data-ttu-id="ae54d-146">Vyberte **přehled** a **najít uživatele** z rychlé úlohy.</span><span class="sxs-lookup"><span data-stu-id="ae54d-146">Select **Overview** and **Find a user** from Quick tasks.</span></span>

     ![Najít uživatele](./media/resource-group-create-service-principal-portal/find-user.png)
2. <span data-ttu-id="ae54d-148">Vyhledávání pro váš účet a vyberte ho, když ji najít.</span><span class="sxs-lookup"><span data-stu-id="ae54d-148">Search for your account, and select it when you find it.</span></span>

     ![Hledat uživatele](./media/resource-group-create-service-principal-portal/show-user.png) 
     
3. <span data-ttu-id="ae54d-150">Vyberte **prostředky Azure**.</span><span class="sxs-lookup"><span data-stu-id="ae54d-150">Select **Azure resources**.</span></span>

     ![Vyberte prostředky](./media/resource-group-create-service-principal-portal/select-azure-resources.png) 
3. <span data-ttu-id="ae54d-152">Zobrazit přiřazené role a zjistit, pokud máte odpovídající oprávnění k aplikaci AD přiřadit roli.</span><span class="sxs-lookup"><span data-stu-id="ae54d-152">View your assigned roles, and determine if you have adequate permissions to assign an AD app to a role.</span></span> <span data-ttu-id="ae54d-153">Pokud ne, požádejte správce předplatného můžete přidat do role správce přístupu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="ae54d-153">If not, ask your subscription administrator to add you to User Access Administrator role.</span></span> <span data-ttu-id="ae54d-154">Na následujícím obrázku je přiřadit uživatele k roli vlastníka pro obě předplatná, což znamená, že tento uživatel má odpovídající oprávnění.</span><span class="sxs-lookup"><span data-stu-id="ae54d-154">In the following image, the user is assigned to the Owner role for two subscriptions, which means that user has adequate permissions.</span></span> 

     ![Zobrazit oprávnění](./media/resource-group-create-service-principal-portal/view-assigned-roles.png)

## <a name="create-an-azure-active-directory-application"></a><span data-ttu-id="ae54d-156">Vytvoření aplikace Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ae54d-156">Create an Azure Active Directory application</span></span>
1. <span data-ttu-id="ae54d-157">Přihlaste se k účtu Azure prostřednictvím [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="ae54d-157">Log in to your Azure Account through the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="ae54d-158">Vyberte **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="ae54d-158">Select **Azure Active Directory**.</span></span>

     ![Vyberte služby azure active directory](./media/resource-group-create-service-principal-portal/select-active-directory.png)

4. <span data-ttu-id="ae54d-160">Vyberte **registrace aplikace**.</span><span class="sxs-lookup"><span data-stu-id="ae54d-160">Select **App registrations**.</span></span>   

     ![Vyberte aplikaci registrace](./media/resource-group-create-service-principal-portal/select-app-registrations.png)
5. <span data-ttu-id="ae54d-162">Vyberte **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="ae54d-162">Select **Add**.</span></span>

     ![Přidat aplikaci](./media/resource-group-create-service-principal-portal/select-add-app.png)

6. <span data-ttu-id="ae54d-164">Zadejte název a adresu URL pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ae54d-164">Provide a name and URL for the application.</span></span> <span data-ttu-id="ae54d-165">Vyberte buď **webovou aplikaci nebo API** nebo **nativní** pro typ aplikace, kterou chcete vytvořit.</span><span class="sxs-lookup"><span data-stu-id="ae54d-165">Select either **Web app / API** or **Native** for the type of application you want to create.</span></span> <span data-ttu-id="ae54d-166">Po nastavení hodnoty, vyberte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="ae54d-166">After setting the values, select **Create**.</span></span>

     ![název aplikace](./media/resource-group-create-service-principal-portal/create-app.png)

<span data-ttu-id="ae54d-168">Vytvoření vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="ae54d-168">You have created your application.</span></span>

## <a name="get-application-id-and-authentication-key"></a><span data-ttu-id="ae54d-169">Získat klíč ID a ověřování aplikace</span><span class="sxs-lookup"><span data-stu-id="ae54d-169">Get application ID and authentication key</span></span>
<span data-ttu-id="ae54d-170">Pokud protokolování prostřednictvím kódu programu, je potřeba ID pro vaše aplikace a ověřovací klíč.</span><span class="sxs-lookup"><span data-stu-id="ae54d-170">When programmatically logging in, you need the ID for your application and an authentication key.</span></span> <span data-ttu-id="ae54d-171">Tyto hodnoty, použijte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ae54d-171">To get those values, use the following steps:</span></span>

1. <span data-ttu-id="ae54d-172">Z **registrace aplikace** v Azure Active Directory, vyberte svou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ae54d-172">From **App registrations** in Azure Active Directory, select your application.</span></span>

     ![Vyberte aplikaci](./media/resource-group-create-service-principal-portal/select-app.png)
2. <span data-ttu-id="ae54d-174">Kopírování **ID aplikace** a ukládá je v kódu aplikace.</span><span class="sxs-lookup"><span data-stu-id="ae54d-174">Copy the **Application ID** and store it in your application code.</span></span> <span data-ttu-id="ae54d-175">Aplikace v [ukázkové aplikace](#sample-applications) části odkazují na tuto hodnotu jako id klienta.</span><span class="sxs-lookup"><span data-stu-id="ae54d-175">The applications in the [sample applications](#sample-applications) section refer to this value as the client id.</span></span>

     ![id klienta](./media/resource-group-create-service-principal-portal/copy-app-id.png)
3. <span data-ttu-id="ae54d-177">Generovat ověřovací klíč, vyberte **klíče**.</span><span class="sxs-lookup"><span data-stu-id="ae54d-177">To generate an authentication key, select **Keys**.</span></span>

     ![Vyberte klíče](./media/resource-group-create-service-principal-portal/select-keys.png)
4. <span data-ttu-id="ae54d-179">Zadejte popis klíč a dobu trvání pro klíč.</span><span class="sxs-lookup"><span data-stu-id="ae54d-179">Provide a description of the key, and a duration for the key.</span></span> <span data-ttu-id="ae54d-180">Až budete hotoví, vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="ae54d-180">When done, select **Save**.</span></span>

     ![klíč uložit](./media/resource-group-create-service-principal-portal/save-key.png)

     <span data-ttu-id="ae54d-182">Po uložení klíče, se zobrazí hodnota klíče.</span><span class="sxs-lookup"><span data-stu-id="ae54d-182">After saving the key, the value of the key is displayed.</span></span> <span data-ttu-id="ae54d-183">Zkopírujte tuto hodnotu, protože nemůžete později načíst klíč.</span><span class="sxs-lookup"><span data-stu-id="ae54d-183">Copy this value because you are not able to retrieve the key later.</span></span> <span data-ttu-id="ae54d-184">Hodnota klíče byl poskytnout ID aplikace pro přihlášení jako aplikace.</span><span class="sxs-lookup"><span data-stu-id="ae54d-184">You provide the key value with the application ID to log in as the application.</span></span> <span data-ttu-id="ae54d-185">Uložení klíče hodnoty, kde vaše aplikace, můžete jej načíst.</span><span class="sxs-lookup"><span data-stu-id="ae54d-185">Store the key value where your application can retrieve it.</span></span>

     ![Uložit klíč](./media/resource-group-create-service-principal-portal/copy-key.png)

## <a name="get-tenant-id"></a><span data-ttu-id="ae54d-187">Získání ID klienta</span><span class="sxs-lookup"><span data-stu-id="ae54d-187">Get tenant ID</span></span>
<span data-ttu-id="ae54d-188">Když protokolování prostřednictvím kódu programu, potřebujete předat ID klienta s žádostí o ověření.</span><span class="sxs-lookup"><span data-stu-id="ae54d-188">When programmatically logging in, you need to pass the tenant ID with your authentication request.</span></span> 

1. <span data-ttu-id="ae54d-189">Chcete-li získat ID klienta, vyberte **vlastnosti** pro vašeho tenanta Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ae54d-189">To get the tenant ID, select **Properties** for your Azure AD tenant.</span></span> 

     ![Vyberte vlastnosti, které Azure AD](./media/resource-group-create-service-principal-portal/select-ad-properties.png)

2. <span data-ttu-id="ae54d-191">Kopírování **ID adresáře**.</span><span class="sxs-lookup"><span data-stu-id="ae54d-191">Copy the **Directory ID**.</span></span> <span data-ttu-id="ae54d-192">Tato hodnota je vaše ID klienta.</span><span class="sxs-lookup"><span data-stu-id="ae54d-192">This value is your tenant ID.</span></span>

     ![id klienta](./media/resource-group-create-service-principal-portal/copy-directory-id.png)

## <a name="assign-application-to-role"></a><span data-ttu-id="ae54d-194">Přiřazení aplikace k roli</span><span class="sxs-lookup"><span data-stu-id="ae54d-194">Assign application to role</span></span>
<span data-ttu-id="ae54d-195">Pro přístup k prostředkům ve vašem předplatném, je nutné přiřadit aplikace k roli.</span><span class="sxs-lookup"><span data-stu-id="ae54d-195">To access resources in your subscription, you must assign the application to a role.</span></span> <span data-ttu-id="ae54d-196">Rozhodněte, jakou roli představuje správná oprávnění pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ae54d-196">Decide which role represents the right permissions for the application.</span></span> <span data-ttu-id="ae54d-197">Další informace o dostupných rolí najdete v tématu [RBAC: integrovaným rolím](../active-directory/role-based-access-built-in-roles.md).</span><span class="sxs-lookup"><span data-stu-id="ae54d-197">To learn about the available roles, see [RBAC: Built in Roles](../active-directory/role-based-access-built-in-roles.md).</span></span>

<span data-ttu-id="ae54d-198">Rozsah můžete nastavit na úrovni předplatné, skupinu prostředků nebo prostředek.</span><span class="sxs-lookup"><span data-stu-id="ae54d-198">You can set the scope at the level of the subscription, resource group, or resource.</span></span> <span data-ttu-id="ae54d-199">Na nižších úrovních oboru dědí oprávnění.</span><span class="sxs-lookup"><span data-stu-id="ae54d-199">Permissions are inherited to lower levels of scope.</span></span> <span data-ttu-id="ae54d-200">Například přidání aplikace do role Čtenář pro skupinu prostředků znamená, že ho může číst skupině prostředků a všechny prostředky, které obsahuje.</span><span class="sxs-lookup"><span data-stu-id="ae54d-200">For example, adding an application to the Reader role for a resource group means it can read the resource group and any resources it contains.</span></span>

1. <span data-ttu-id="ae54d-201">Přejděte na úroveň oboru, který chcete přiřadit aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ae54d-201">Navigate to the level of scope you wish to assign the application to.</span></span> <span data-ttu-id="ae54d-202">Například přiřazení role v oboru předplatné, vyberte **odběry**.</span><span class="sxs-lookup"><span data-stu-id="ae54d-202">For example, to assign a role at the subscription scope, select **Subscriptions**.</span></span> <span data-ttu-id="ae54d-203">Místo toho můžete třeba vybrat skupinu prostředků nebo prostředek.</span><span class="sxs-lookup"><span data-stu-id="ae54d-203">You could instead select a resource group or resource.</span></span>

     ![Vyberte předplatné](./media/resource-group-create-service-principal-portal/select-subscription.png)

2. <span data-ttu-id="ae54d-205">Vyberte určitý odběr (skupinu prostředků nebo prostředek) aplikaci přiřadit.</span><span class="sxs-lookup"><span data-stu-id="ae54d-205">Select the particular subscription (resource group or resource) to assign the application to.</span></span>

     ![Vyberte předplatné pro přiřazení](./media/resource-group-create-service-principal-portal/select-one-subscription.png)

3. <span data-ttu-id="ae54d-207">Vyberte **přístup k ovládacímu prvku (IAM)**.</span><span class="sxs-lookup"><span data-stu-id="ae54d-207">Select **Access Control (IAM)**.</span></span>

     ![Vybrat přístupu](./media/resource-group-create-service-principal-portal/select-access-control.png)

4. <span data-ttu-id="ae54d-209">Vyberte **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="ae54d-209">Select **Add**.</span></span>

     ![Vyberte Přidat](./media/resource-group-create-service-principal-portal/select-add.png)
6. <span data-ttu-id="ae54d-211">Vyberte roli, kterou chcete přiřadit k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ae54d-211">Select the role you wish to assign to the application.</span></span> <span data-ttu-id="ae54d-212">Na následujícím obrázku **čtečky** role.</span><span class="sxs-lookup"><span data-stu-id="ae54d-212">The following image shows the **Reader** role.</span></span>

     ![Vyberte roli](./media/resource-group-create-service-principal-portal/select-role.png)

8. <span data-ttu-id="ae54d-214">Vyhledávání pro vaši aplikaci a vyberte jej.</span><span class="sxs-lookup"><span data-stu-id="ae54d-214">Search for your application, and select it.</span></span>

     ![Hledat aplikace](./media/resource-group-create-service-principal-portal/search-app.png)
9. <span data-ttu-id="ae54d-216">Vyberte **OK** k dokončení přiřazení role.</span><span class="sxs-lookup"><span data-stu-id="ae54d-216">Select **OK** to finish assigning the role.</span></span> <span data-ttu-id="ae54d-217">Zobrazí aplikace v seznamu Uživatelé s přiřazenou rolí pro tento obor.</span><span class="sxs-lookup"><span data-stu-id="ae54d-217">You see your application in the list of users assigned to a role for that scope.</span></span>

## <a name="log-in-as-the-application"></a><span data-ttu-id="ae54d-218">Přihlaste se jako aplikace</span><span class="sxs-lookup"><span data-stu-id="ae54d-218">Log in as the application</span></span>

<span data-ttu-id="ae54d-219">Aplikace je teď nastavený v Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="ae54d-219">Your application is now set up in Azure Active Directory.</span></span> <span data-ttu-id="ae54d-220">Máte ID a klíč, použít pro přihlášení jako aplikace.</span><span class="sxs-lookup"><span data-stu-id="ae54d-220">You have an ID and key to use for signing in as the application.</span></span> <span data-ttu-id="ae54d-221">Aplikace je přiřadit roli, která poskytuje ho určité akce, které můžete provádět.</span><span class="sxs-lookup"><span data-stu-id="ae54d-221">The application is assigned to a role that gives it certain actions it can perform.</span></span> <span data-ttu-id="ae54d-222">Informace o protokolování jako aplikace prostřednictvím různých platforem najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="ae54d-222">For information about logging in as the application through different platforms, see:</span></span>

* [<span data-ttu-id="ae54d-223">PowerShell</span><span class="sxs-lookup"><span data-stu-id="ae54d-223">PowerShell</span></span>](resource-group-authenticate-service-principal.md#provide-credentials-through-powershell)
* [<span data-ttu-id="ae54d-224">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="ae54d-224">Azure CLI</span></span>](resource-group-authenticate-service-principal-cli.md#provide-credentials-through-azure-cli)
* [<span data-ttu-id="ae54d-225">REST</span><span class="sxs-lookup"><span data-stu-id="ae54d-225">REST</span></span>](/rest/api/#create-the-request)
* [<span data-ttu-id="ae54d-226">.NET</span><span class="sxs-lookup"><span data-stu-id="ae54d-226">.NET</span></span>](/dotnet/azure/dotnet-sdk-azure-authenticate?view=azure-dotnet)
* [<span data-ttu-id="ae54d-227">Java</span><span class="sxs-lookup"><span data-stu-id="ae54d-227">Java</span></span>](/java/azure/java-sdk-azure-authenticate)
* [<span data-ttu-id="ae54d-228">Node.js</span><span class="sxs-lookup"><span data-stu-id="ae54d-228">Node.js</span></span>](/nodejs/azure/node-sdk-azure-get-started?view=azure-node-2.0.0)
* [<span data-ttu-id="ae54d-229">Python</span><span class="sxs-lookup"><span data-stu-id="ae54d-229">Python</span></span>](/python/azure/python-sdk-azure-authenticate?view=azure-python)
* [<span data-ttu-id="ae54d-230">Ruby</span><span class="sxs-lookup"><span data-stu-id="ae54d-230">Ruby</span></span>](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/)


## <a name="next-steps"></a><span data-ttu-id="ae54d-231">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ae54d-231">Next steps</span></span>
* <span data-ttu-id="ae54d-232">Víceklientské aplikace, naleznete v tématu [Příručka pro vývojáře k autorizaci s rozhraním API pro Azure Resource Manager](resource-manager-api-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="ae54d-232">To set up a multi-tenant application, see [Developer's guide to authorization with the Azure Resource Manager API](resource-manager-api-authentication.md).</span></span>
* <span data-ttu-id="ae54d-233">Další informace o určení zásady zabezpečení najdete v tématu [řízení přístupu na základě Role v Azure](../active-directory/role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="ae54d-233">To learn about specifying security policies, see [Azure Role-based Access Control](../active-directory/role-based-access-control-configure.md).</span></span>  
* <span data-ttu-id="ae54d-234">Seznam dostupných akcí, které může být povolen nebo odepřen uživatelům najdete v tématu [poskytovatel prostředků Azure Resource Manager operations](../active-directory/role-based-access-control-resource-provider-operations.md).</span><span class="sxs-lookup"><span data-stu-id="ae54d-234">For a list of available actions that can be granted or denied to users, see [Azure Resource Manager Resource Provider operations](../active-directory/role-based-access-control-resource-provider-operations.md).</span></span>
