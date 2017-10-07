---
title: "aaaCreate identitu pro aplikace v portálu Azure | Microsoft Docs"
description: "Popisuje, jak toocreate novou aplikaci Azure Active Directory a objektu, který lze použít s hello řízení přístupu na základě rolí v Azure Resource Manager toomanage služby přístup tooresources."
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
ms.openlocfilehash: 9624715ac612f42df6f9e9e67b8233bd4b914174
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-portal-toocreate-an-azure-active-directory-application-and-service-principal-that-can-access-resources"></a><span data-ttu-id="c36a3-103">Použijte portál toocreate aplikaci Azure Active Directory a objektu služby, které mají přístup k prostředkům</span><span class="sxs-lookup"><span data-stu-id="c36a3-103">Use portal toocreate an Azure Active Directory application and service principal that can access resources</span></span>

<span data-ttu-id="c36a3-104">Když máte aplikaci, která potřebuje tooaccess nebo úpravám prostředků, musíte nastavit aplikaci Azure Active Directory (AD) a přiřadit tooit hello požadované oprávnění.</span><span class="sxs-lookup"><span data-stu-id="c36a3-104">When you have an application that needs tooaccess or modify resources, you must set up an Azure Active Directory (AD) application and assign hello required permissions tooit.</span></span> <span data-ttu-id="c36a3-105">Tento postup je vhodnější toorunning aplikace hello vlastní oprávnění, protože:</span><span class="sxs-lookup"><span data-stu-id="c36a3-105">This approach is preferable toorunning hello app under your own credentials because:</span></span>

* <span data-ttu-id="c36a3-106">Můžete přiřadit oprávnění toohello identita aplikace, která se liší od vlastní oprávnění.</span><span class="sxs-lookup"><span data-stu-id="c36a3-106">You can assign permissions toohello app identity that are different than your own permissions.</span></span> <span data-ttu-id="c36a3-107">Obvykle se tato oprávnění se s omezeným přístupem tooexactly jaké aplikace hello musí toodo.</span><span class="sxs-lookup"><span data-stu-id="c36a3-107">Typically, these permissions are restricted tooexactly what hello app needs toodo.</span></span>
* <span data-ttu-id="c36a3-108">Pokud vaše odpovědnosti změnit nemají aplikace hello toochange pověření.</span><span class="sxs-lookup"><span data-stu-id="c36a3-108">You do not have toochange hello app's credentials if your responsibilities change.</span></span> 
* <span data-ttu-id="c36a3-109">Při provádění bezobslužného skriptu, můžete použít ověřování pomocí certifikátu tooautomate.</span><span class="sxs-lookup"><span data-stu-id="c36a3-109">You can use a certificate tooautomate authentication when executing an unattended script.</span></span>

<span data-ttu-id="c36a3-110">Toto téma ukazuje, jak tooperform ty kroky prostřednictvím portálu hello.</span><span class="sxs-lookup"><span data-stu-id="c36a3-110">This topic shows you how tooperform those steps through hello portal.</span></span> <span data-ttu-id="c36a3-111">Zaměřuje se na jednoho klienta aplikace, kde aplikace hello je určený toorun v rámci organizace jenom jeden.</span><span class="sxs-lookup"><span data-stu-id="c36a3-111">It focuses on a single-tenant application where hello application is intended toorun within only one organization.</span></span> <span data-ttu-id="c36a3-112">Obvykle používají aplikace jednoho klienta pro-obchodní aplikace, které běží v rámci vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="c36a3-112">You typically use single-tenant applications for line-of-business applications that run within your organization.</span></span>
 
## <a name="required-permissions"></a><span data-ttu-id="c36a3-113">Požadovaná oprávnění</span><span class="sxs-lookup"><span data-stu-id="c36a3-113">Required permissions</span></span>
<span data-ttu-id="c36a3-114">toocomplete tohoto tématu, musíte mít dostatečná oprávnění tooregister aplikace s klientovi Azure AD a přiřazení role tooa aplikace hello ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="c36a3-114">toocomplete this topic, you must have sufficient permissions tooregister an application with your Azure AD tenant, and assign hello application tooa role in your Azure subscription.</span></span> <span data-ttu-id="c36a3-115">Ujistíme se, že máte správná oprávnění tooperform hello těchto kroků.</span><span class="sxs-lookup"><span data-stu-id="c36a3-115">Let's make sure you have hello right permissions tooperform those steps.</span></span>

### <a name="check-azure-active-directory-permissions"></a><span data-ttu-id="c36a3-116">Zkontrolujte oprávnění služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c36a3-116">Check Azure Active Directory permissions</span></span>
1. <span data-ttu-id="c36a3-117">Přihlaste se tooyour účet Azure prostřednictvím hello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="c36a3-117">Log in tooyour Azure Account through hello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="c36a3-118">Vyberte **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="c36a3-118">Select **Azure Active Directory**.</span></span>

     ![Vyberte služby azure active directory](./media/resource-group-create-service-principal-portal/select-active-directory.png)
3. <span data-ttu-id="c36a3-120">V Azure Active Directory, vyberte **uživatelská nastavení**.</span><span class="sxs-lookup"><span data-stu-id="c36a3-120">In Azure Active Directory, select **User settings**.</span></span>

     ![Vyberte nastavení uživatele](./media/resource-group-create-service-principal-portal/select-user-settings.png)
4. <span data-ttu-id="c36a3-122">Zkontrolujte hello **registrace aplikace** nastavení.</span><span class="sxs-lookup"><span data-stu-id="c36a3-122">Check hello **App registrations** setting.</span></span> <span data-ttu-id="c36a3-123">Pokud nastavení příliš**Ano**, uživatelé bez oprávnění správce můžete zaregistrovat aplikace AD.</span><span class="sxs-lookup"><span data-stu-id="c36a3-123">If set too**Yes**, non-admin users can register AD apps.</span></span> <span data-ttu-id="c36a3-124">Toto nastavení znamená, že každý uživatel v klientovi Azure AD hello můžete zaregistrovat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c36a3-124">This setting means any user in hello Azure AD tenant can register an app.</span></span> <span data-ttu-id="c36a3-125">Abyste mohli pokračovat příliš[oprávnění předplatné Azure zkontrolujte](#check-azure-subscription-permissions).</span><span class="sxs-lookup"><span data-stu-id="c36a3-125">You can proceed too[Check Azure subscription permissions](#check-azure-subscription-permissions).</span></span>

     ![Zobrazit registrace aplikace](./media/resource-group-create-service-principal-portal/view-app-registrations.png)
5. <span data-ttu-id="c36a3-127">Pokud registrace aplikace hello nastavení je nastaven příliš**ne**jen správce uživatelé mohou registrovat aplikace.</span><span class="sxs-lookup"><span data-stu-id="c36a3-127">If hello app registrations setting is set too**No**, only admin users can register apps.</span></span> <span data-ttu-id="c36a3-128">Je nutné toocheck, zda je váš účet správce pro tenanta Azure AD hello.</span><span class="sxs-lookup"><span data-stu-id="c36a3-128">You need toocheck whether your account is an admin for hello Azure AD tenant.</span></span> <span data-ttu-id="c36a3-129">Vyberte **přehled** a **najít uživatele** z rychlé úlohy.</span><span class="sxs-lookup"><span data-stu-id="c36a3-129">Select **Overview** and **Find a user** from Quick tasks.</span></span>

     ![Najít uživatele](./media/resource-group-create-service-principal-portal/find-user.png)
6. <span data-ttu-id="c36a3-131">Vyhledávání pro váš účet a vyberte ho, když ji najít.</span><span class="sxs-lookup"><span data-stu-id="c36a3-131">Search for your account, and select it when you find it.</span></span>

     ![Hledat uživatele](./media/resource-group-create-service-principal-portal/show-user.png)
7. <span data-ttu-id="c36a3-133">Pro váš účet, vyberte **Directory role**.</span><span class="sxs-lookup"><span data-stu-id="c36a3-133">For your account, select **Directory role**.</span></span> 

     ![role adresáře](./media/resource-group-create-service-principal-portal/select-directory-role.png)
8. <span data-ttu-id="c36a3-135">Zobrazte vaše role přiřazené adresář ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c36a3-135">View your assigned directory role in Azure AD.</span></span> <span data-ttu-id="c36a3-136">Pokud je váš účet přiřazenou roli uživatele toohello, ale hello registrace nastavením aplikace (z předchozích kroků hello), je omezený tooadmin uživatelé, požádejte vašeho správce tooeither přiřadit můžete tooan roli správce, nebo tooenable uživatelé tooregister aplikace.</span><span class="sxs-lookup"><span data-stu-id="c36a3-136">If your account is assigned toohello User role, but hello app registration setting (from hello preceding steps) is limited tooadmin users, ask your administrator tooeither assign you tooan administrator role, or tooenable users tooregister apps.</span></span>

     ![role zobrazení](./media/resource-group-create-service-principal-portal/view-role.png)

### <a name="check-azure-subscription-permissions"></a><span data-ttu-id="c36a3-138">Zkontrolujte oprávnění předplatného Azure</span><span class="sxs-lookup"><span data-stu-id="c36a3-138">Check Azure subscription permissions</span></span>
<span data-ttu-id="c36a3-139">Ve vašem předplatném Azure, musí mít váš účet `Microsoft.Authorization/*/Write` přístup tooassign role tooa aplikace AD.</span><span class="sxs-lookup"><span data-stu-id="c36a3-139">In your Azure subscription, your account must have `Microsoft.Authorization/*/Write` access tooassign an AD app tooa role.</span></span> <span data-ttu-id="c36a3-140">Tato akce je poskytována prostřednictvím hello [vlastníka](../active-directory/role-based-access-built-in-roles.md#owner) role nebo [správce přístupu uživatelů](../active-directory/role-based-access-built-in-roles.md#user-access-administrator) role.</span><span class="sxs-lookup"><span data-stu-id="c36a3-140">This action is granted through hello [Owner](../active-directory/role-based-access-built-in-roles.md#owner) role or [User Access Administrator](../active-directory/role-based-access-built-in-roles.md#user-access-administrator) role.</span></span> <span data-ttu-id="c36a3-141">Pokud je váš účet přiřazenou toohello **Přispěvatel** role, nemáte dostatečná oprávnění.</span><span class="sxs-lookup"><span data-stu-id="c36a3-141">If your account is assigned toohello **Contributor** role, you do not have adequate permission.</span></span> <span data-ttu-id="c36a3-142">Obdržíte chybu při pokusu o role hlavního tooa služby tooassign hello.</span><span class="sxs-lookup"><span data-stu-id="c36a3-142">You will receive an error when attempting tooassign hello service principal tooa role.</span></span> 

<span data-ttu-id="c36a3-143">toocheck oprávnění svého předplatného:</span><span class="sxs-lookup"><span data-stu-id="c36a3-143">toocheck your subscription permissions:</span></span>

1. <span data-ttu-id="c36a3-144">Nejsou-li již zobrazena v účtu Azure AD z předchozích kroků hello, vyberte **Azure Active Directory** v levém podokně hello.</span><span class="sxs-lookup"><span data-stu-id="c36a3-144">If you are not already looking at your Azure AD account from hello preceding steps, select **Azure Active Directory** from hello left pane.</span></span>

2. <span data-ttu-id="c36a3-145">Najít váš účet Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c36a3-145">Find your Azure AD account.</span></span> <span data-ttu-id="c36a3-146">Vyberte **přehled** a **najít uživatele** z rychlé úlohy.</span><span class="sxs-lookup"><span data-stu-id="c36a3-146">Select **Overview** and **Find a user** from Quick tasks.</span></span>

     ![Najít uživatele](./media/resource-group-create-service-principal-portal/find-user.png)
2. <span data-ttu-id="c36a3-148">Vyhledávání pro váš účet a vyberte ho, když ji najít.</span><span class="sxs-lookup"><span data-stu-id="c36a3-148">Search for your account, and select it when you find it.</span></span>

     ![Hledat uživatele](./media/resource-group-create-service-principal-portal/show-user.png) 
     
3. <span data-ttu-id="c36a3-150">Vyberte **prostředky Azure**.</span><span class="sxs-lookup"><span data-stu-id="c36a3-150">Select **Azure resources**.</span></span>

     ![Vyberte prostředky](./media/resource-group-create-service-principal-portal/select-azure-resources.png) 
3. <span data-ttu-id="c36a3-152">Zobrazit přiřazené role a zjistit, jestli máte tooassign odpovídající oprávnění AD aplikace tooa role.</span><span class="sxs-lookup"><span data-stu-id="c36a3-152">View your assigned roles, and determine if you have adequate permissions tooassign an AD app tooa role.</span></span> <span data-ttu-id="c36a3-153">Pokud ne, požádejte správce tooadd vaše předplatné můžete tooUser přístupu k roli správce.</span><span class="sxs-lookup"><span data-stu-id="c36a3-153">If not, ask your subscription administrator tooadd you tooUser Access Administrator role.</span></span> <span data-ttu-id="c36a3-154">V hello následující bitové kopie je uživatel hello přiřazené toohello roli vlastníka pro obě předplatná, což znamená, že tento uživatel má odpovídající oprávnění.</span><span class="sxs-lookup"><span data-stu-id="c36a3-154">In hello following image, hello user is assigned toohello Owner role for two subscriptions, which means that user has adequate permissions.</span></span> 

     ![Zobrazit oprávnění](./media/resource-group-create-service-principal-portal/view-assigned-roles.png)

## <a name="create-an-azure-active-directory-application"></a><span data-ttu-id="c36a3-156">Vytvoření aplikace Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c36a3-156">Create an Azure Active Directory application</span></span>
1. <span data-ttu-id="c36a3-157">Přihlaste se tooyour účet Azure prostřednictvím hello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="c36a3-157">Log in tooyour Azure Account through hello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="c36a3-158">Vyberte **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="c36a3-158">Select **Azure Active Directory**.</span></span>

     ![Vyberte služby azure active directory](./media/resource-group-create-service-principal-portal/select-active-directory.png)

4. <span data-ttu-id="c36a3-160">Vyberte **registrace aplikace**.</span><span class="sxs-lookup"><span data-stu-id="c36a3-160">Select **App registrations**.</span></span>   

     ![Vyberte aplikaci registrace](./media/resource-group-create-service-principal-portal/select-app-registrations.png)
5. <span data-ttu-id="c36a3-162">Vyberte **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="c36a3-162">Select **Add**.</span></span>

     ![Přidat aplikaci](./media/resource-group-create-service-principal-portal/select-add-app.png)

6. <span data-ttu-id="c36a3-164">Zadejte název a adresu URL pro aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="c36a3-164">Provide a name and URL for hello application.</span></span> <span data-ttu-id="c36a3-165">Vyberte buď **webovou aplikaci nebo API** nebo **nativní** hello typ aplikace se má toocreate.</span><span class="sxs-lookup"><span data-stu-id="c36a3-165">Select either **Web app / API** or **Native** for hello type of application you want toocreate.</span></span> <span data-ttu-id="c36a3-166">Po nastavení hodnoty hello, vyberte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="c36a3-166">After setting hello values, select **Create**.</span></span>

     ![název aplikace](./media/resource-group-create-service-principal-portal/create-app.png)

<span data-ttu-id="c36a3-168">Vytvoření vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="c36a3-168">You have created your application.</span></span>

## <a name="get-application-id-and-authentication-key"></a><span data-ttu-id="c36a3-169">Získat klíč ID a ověřování aplikace</span><span class="sxs-lookup"><span data-stu-id="c36a3-169">Get application ID and authentication key</span></span>
<span data-ttu-id="c36a3-170">Pokud protokolování prostřednictvím kódu programu, je potřeba hello ID pro vaše aplikace a ověřovací klíč.</span><span class="sxs-lookup"><span data-stu-id="c36a3-170">When programmatically logging in, you need hello ID for your application and an authentication key.</span></span> <span data-ttu-id="c36a3-171">tooget tyto hodnoty, hello použijte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c36a3-171">tooget those values, use hello following steps:</span></span>

1. <span data-ttu-id="c36a3-172">Z **registrace aplikace** v Azure Active Directory, vyberte svou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c36a3-172">From **App registrations** in Azure Active Directory, select your application.</span></span>

     ![Vyberte aplikaci](./media/resource-group-create-service-principal-portal/select-app.png)
2. <span data-ttu-id="c36a3-174">Kopírování hello **ID aplikace** a ukládá je v kódu aplikace.</span><span class="sxs-lookup"><span data-stu-id="c36a3-174">Copy hello **Application ID** and store it in your application code.</span></span> <span data-ttu-id="c36a3-175">Hello aplikace v hello [ukázkové aplikace](#sample-applications) naleznete v části toothis hodnotu jako id klienta hello.</span><span class="sxs-lookup"><span data-stu-id="c36a3-175">hello applications in hello [sample applications](#sample-applications) section refer toothis value as hello client id.</span></span>

     ![id klienta](./media/resource-group-create-service-principal-portal/copy-app-id.png)
3. <span data-ttu-id="c36a3-177">Vyberte toogenerate ověřovací klíč, **klíče**.</span><span class="sxs-lookup"><span data-stu-id="c36a3-177">toogenerate an authentication key, select **Keys**.</span></span>

     ![Vyberte klíče](./media/resource-group-create-service-principal-portal/select-keys.png)
4. <span data-ttu-id="c36a3-179">Zadejte popis hello klíče a dobu trvání pro klíč hello.</span><span class="sxs-lookup"><span data-stu-id="c36a3-179">Provide a description of hello key, and a duration for hello key.</span></span> <span data-ttu-id="c36a3-180">Až budete hotoví, vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="c36a3-180">When done, select **Save**.</span></span>

     ![klíč uložit](./media/resource-group-create-service-principal-portal/save-key.png)

     <span data-ttu-id="c36a3-182">Po uložení hello klíč, je zobrazená hodnota hello hello klíče.</span><span class="sxs-lookup"><span data-stu-id="c36a3-182">After saving hello key, hello value of hello key is displayed.</span></span> <span data-ttu-id="c36a3-183">Tato hodnota zkopírujte, protože nejste klíč možné tooretrieve hello později.</span><span class="sxs-lookup"><span data-stu-id="c36a3-183">Copy this value because you are not able tooretrieve hello key later.</span></span> <span data-ttu-id="c36a3-184">Hodnota klíče hello poskytnete s toolog ID aplikace hello v jako aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="c36a3-184">You provide hello key value with hello application ID toolog in as hello application.</span></span> <span data-ttu-id="c36a3-185">Uložení klíče hodnoty hello, kde vaše aplikace, můžete jej načíst.</span><span class="sxs-lookup"><span data-stu-id="c36a3-185">Store hello key value where your application can retrieve it.</span></span>

     ![Uložit klíč](./media/resource-group-create-service-principal-portal/copy-key.png)

## <a name="get-tenant-id"></a><span data-ttu-id="c36a3-187">Získání ID klienta</span><span class="sxs-lookup"><span data-stu-id="c36a3-187">Get tenant ID</span></span>
<span data-ttu-id="c36a3-188">Při protokolování prostřednictvím kódu programu, je nutné ID klienta hello toopass s žádostí o ověření.</span><span class="sxs-lookup"><span data-stu-id="c36a3-188">When programmatically logging in, you need toopass hello tenant ID with your authentication request.</span></span> 

1. <span data-ttu-id="c36a3-189">ID klienta tooget hello, vyberte **vlastnosti** pro vašeho tenanta Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c36a3-189">tooget hello tenant ID, select **Properties** for your Azure AD tenant.</span></span> 

     ![Vyberte vlastnosti, které Azure AD](./media/resource-group-create-service-principal-portal/select-ad-properties.png)

2. <span data-ttu-id="c36a3-191">Kopírování hello **ID adresáře**.</span><span class="sxs-lookup"><span data-stu-id="c36a3-191">Copy hello **Directory ID**.</span></span> <span data-ttu-id="c36a3-192">Tato hodnota je vaše ID klienta.</span><span class="sxs-lookup"><span data-stu-id="c36a3-192">This value is your tenant ID.</span></span>

     ![id klienta](./media/resource-group-create-service-principal-portal/copy-directory-id.png)

## <a name="assign-application-toorole"></a><span data-ttu-id="c36a3-194">Přiřadit toorole aplikace</span><span class="sxs-lookup"><span data-stu-id="c36a3-194">Assign application toorole</span></span>
<span data-ttu-id="c36a3-195">tooaccess prostředky v rámci vašeho předplatného, je nutné přiřadit role tooa aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="c36a3-195">tooaccess resources in your subscription, you must assign hello application tooa role.</span></span> <span data-ttu-id="c36a3-196">Rozhodněte, jakou roli představuje hello správná oprávnění pro aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="c36a3-196">Decide which role represents hello right permissions for hello application.</span></span> <span data-ttu-id="c36a3-197">toolearn o hello dostupných rolí, najdete v části [RBAC: integrovaným rolím](../active-directory/role-based-access-built-in-roles.md).</span><span class="sxs-lookup"><span data-stu-id="c36a3-197">toolearn about hello available roles, see [RBAC: Built in Roles](../active-directory/role-based-access-built-in-roles.md).</span></span>

<span data-ttu-id="c36a3-198">Můžete nastavit obor hello na úrovni hello hello předplatné, skupinu prostředků nebo prostředek.</span><span class="sxs-lookup"><span data-stu-id="c36a3-198">You can set hello scope at hello level of hello subscription, resource group, or resource.</span></span> <span data-ttu-id="c36a3-199">Oprávnění jsou zděděné toolower úrovně rozsahu.</span><span class="sxs-lookup"><span data-stu-id="c36a3-199">Permissions are inherited toolower levels of scope.</span></span> <span data-ttu-id="c36a3-200">Například přidání že aplikační role čtečky toohello pro skupinu prostředků znamená, že můžete číst hello skupinu prostředků a všechny prostředky, které obsahuje.</span><span class="sxs-lookup"><span data-stu-id="c36a3-200">For example, adding an application toohello Reader role for a resource group means it can read hello resource group and any resources it contains.</span></span>

1. <span data-ttu-id="c36a3-201">Přejděte toohello úroveň oboru, které chcete aplikaci tooassign hello.</span><span class="sxs-lookup"><span data-stu-id="c36a3-201">Navigate toohello level of scope you wish tooassign hello application to.</span></span> <span data-ttu-id="c36a3-202">Například tooassign role v hello obor předplatného, vyberte **odběry**.</span><span class="sxs-lookup"><span data-stu-id="c36a3-202">For example, tooassign a role at hello subscription scope, select **Subscriptions**.</span></span> <span data-ttu-id="c36a3-203">Místo toho můžete třeba vybrat skupinu prostředků nebo prostředek.</span><span class="sxs-lookup"><span data-stu-id="c36a3-203">You could instead select a resource group or resource.</span></span>

     ![Vyberte předplatné](./media/resource-group-create-service-principal-portal/select-subscription.png)

2. <span data-ttu-id="c36a3-205">Vyberte hello určitý odběr (skupinu prostředků nebo prostředek) tooassign hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="c36a3-205">Select hello particular subscription (resource group or resource) tooassign hello application to.</span></span>

     ![Vyberte předplatné pro přiřazení](./media/resource-group-create-service-principal-portal/select-one-subscription.png)

3. <span data-ttu-id="c36a3-207">Vyberte **přístup k ovládacímu prvku (IAM)**.</span><span class="sxs-lookup"><span data-stu-id="c36a3-207">Select **Access Control (IAM)**.</span></span>

     ![Vybrat přístupu](./media/resource-group-create-service-principal-portal/select-access-control.png)

4. <span data-ttu-id="c36a3-209">Vyberte **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="c36a3-209">Select **Add**.</span></span>

     ![Vyberte Přidat](./media/resource-group-create-service-principal-portal/select-add.png)
6. <span data-ttu-id="c36a3-211">Vyberte roli hello chcete tooassign toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="c36a3-211">Select hello role you wish tooassign toohello application.</span></span> <span data-ttu-id="c36a3-212">Hello následující obrázek ukazuje hello **čtečky** role.</span><span class="sxs-lookup"><span data-stu-id="c36a3-212">hello following image shows hello **Reader** role.</span></span>

     ![Vyberte roli](./media/resource-group-create-service-principal-portal/select-role.png)

8. <span data-ttu-id="c36a3-214">Vyhledávání pro vaši aplikaci a vyberte jej.</span><span class="sxs-lookup"><span data-stu-id="c36a3-214">Search for your application, and select it.</span></span>

     ![Hledat aplikace](./media/resource-group-create-service-principal-portal/search-app.png)
9. <span data-ttu-id="c36a3-216">Vyberte **OK** toofinish přiřazení hello role.</span><span class="sxs-lookup"><span data-stu-id="c36a3-216">Select **OK** toofinish assigning hello role.</span></span> <span data-ttu-id="c36a3-217">Zobrazí aplikace v seznamu hello uživatelů přiřazenou roli tooa pro tento obor.</span><span class="sxs-lookup"><span data-stu-id="c36a3-217">You see your application in hello list of users assigned tooa role for that scope.</span></span>

## <a name="log-in-as-hello-application"></a><span data-ttu-id="c36a3-218">Přihlaste se jako aplikace hello</span><span class="sxs-lookup"><span data-stu-id="c36a3-218">Log in as hello application</span></span>

<span data-ttu-id="c36a3-219">Aplikace je teď nastavený v Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="c36a3-219">Your application is now set up in Azure Active Directory.</span></span> <span data-ttu-id="c36a3-220">Máte ID a klíč toouse pro přihlášení jako aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="c36a3-220">You have an ID and key toouse for signing in as hello application.</span></span> <span data-ttu-id="c36a3-221">aplikace Hello je přiřazen tooa role, která poskytuje ho určité akce, které můžete provádět.</span><span class="sxs-lookup"><span data-stu-id="c36a3-221">hello application is assigned tooa role that gives it certain actions it can perform.</span></span> <span data-ttu-id="c36a3-222">Informace o protokolování jako aplikace hello prostřednictvím různých platforem najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="c36a3-222">For information about logging in as hello application through different platforms, see:</span></span>

* [<span data-ttu-id="c36a3-223">PowerShell</span><span class="sxs-lookup"><span data-stu-id="c36a3-223">PowerShell</span></span>](resource-group-authenticate-service-principal.md#provide-credentials-through-powershell)
* [<span data-ttu-id="c36a3-224">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="c36a3-224">Azure CLI</span></span>](resource-group-authenticate-service-principal-cli.md#provide-credentials-through-azure-cli)
* [<span data-ttu-id="c36a3-225">REST</span><span class="sxs-lookup"><span data-stu-id="c36a3-225">REST</span></span>](/rest/api/#create-the-request)
* [<span data-ttu-id="c36a3-226">.NET</span><span class="sxs-lookup"><span data-stu-id="c36a3-226">.NET</span></span>](/dotnet/azure/dotnet-sdk-azure-authenticate?view=azure-dotnet)
* [<span data-ttu-id="c36a3-227">Java</span><span class="sxs-lookup"><span data-stu-id="c36a3-227">Java</span></span>](/java/azure/java-sdk-azure-authenticate)
* [<span data-ttu-id="c36a3-228">Node.js</span><span class="sxs-lookup"><span data-stu-id="c36a3-228">Node.js</span></span>](/nodejs/azure/node-sdk-azure-get-started?view=azure-node-2.0.0)
* [<span data-ttu-id="c36a3-229">Python</span><span class="sxs-lookup"><span data-stu-id="c36a3-229">Python</span></span>](/python/azure/python-sdk-azure-authenticate?view=azure-python)
* [<span data-ttu-id="c36a3-230">Ruby</span><span class="sxs-lookup"><span data-stu-id="c36a3-230">Ruby</span></span>](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/)


## <a name="next-steps"></a><span data-ttu-id="c36a3-231">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c36a3-231">Next steps</span></span>
* <span data-ttu-id="c36a3-232">tooset až víceklientské aplikace, najdete v části [tooauthorization Příručka pro vývojáře s hello rozhraní API služby Azure Resource Manager](resource-manager-api-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="c36a3-232">tooset up a multi-tenant application, see [Developer's guide tooauthorization with hello Azure Resource Manager API](resource-manager-api-authentication.md).</span></span>
* <span data-ttu-id="c36a3-233">toolearn o zadání zásady zabezpečení, najdete v části [řízení přístupu na základě Role v Azure](../active-directory/role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="c36a3-233">toolearn about specifying security policies, see [Azure Role-based Access Control](../active-directory/role-based-access-control-configure.md).</span></span>  
* <span data-ttu-id="c36a3-234">Seznam dostupných akcí, které může být povolen nebo odepřen toousers najdete v tématu [poskytovatel prostředků Azure Resource Manager operations](../active-directory/role-based-access-control-resource-provider-operations.md).</span><span class="sxs-lookup"><span data-stu-id="c36a3-234">For a list of available actions that can be granted or denied toousers, see [Azure Resource Manager Resource Provider operations](../active-directory/role-based-access-control-resource-provider-operations.md).</span></span>
