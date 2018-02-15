---
title: "Azure Active Directory B2C: Přístupy migrace uživatele"
description: "Informace o základní a rozšířené koncepty na migraci uživatele pomocí rozhraní Graph API a volitelně pomocí vlastních zásad Azure AD B2C."
services: active-directory-b2c
documentationcenter: 
author: yoelhor
manager: mtillman
editor: 
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 10/04/2017
ms.author: yoelh
ms.openlocfilehash: 25023359e3f1eeb241f6f0e70bcb179aa32974af
ms.sourcegitcommit: 694e40a193980dea1e2f945471071f11030d5641
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/29/2018
---
# <a name="azure-active-directory-b2c-user-migration"></a><span data-ttu-id="f0b8b-103">Azure Active Directory B2C: Migrace uživatelů</span><span class="sxs-lookup"><span data-stu-id="f0b8b-103">Azure Active Directory B2C: User migration</span></span>
<span data-ttu-id="f0b8b-104">Když migrujete zprostředkovatele identity do Azure Active Directory B2C (Azure AD B2C), možná budete také muset migrovat uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-104">When you migrate your identity provider to Azure Active Directory B2C (Azure AD B2C), you might also need to migrate the user account.</span></span> <span data-ttu-id="f0b8b-105">Tento článek vysvětluje, jak migrovat existující uživatelské účty z kteréhokoli zprostředkovatele identity do Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-105">This article explains how to migrate existing user accounts from any identity provider to Azure AD B2C.</span></span> <span data-ttu-id="f0b8b-106">Článek by neměl být doporučený, ale místo toho popisuje dvě několik přístupů.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-106">The article is not meant to be prescriptive but, rather, it describes two of several approaches.</span></span> <span data-ttu-id="f0b8b-107">Vývojář je zodpovědná za vhodnosti obou těchto přístupů.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-107">The developer is responsible for the suitability of each approach.</span></span>

## <a name="user-migration-flows"></a><span data-ttu-id="f0b8b-108">Toky migrace uživatele</span><span class="sxs-lookup"><span data-stu-id="f0b8b-108">User migration flows</span></span>
<span data-ttu-id="f0b8b-109">S Azure AD B2C, můžete migrovat uživateli prostřednictvím [rozhraní Graph API](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet).</span><span class="sxs-lookup"><span data-stu-id="f0b8b-109">With Azure AD B2C, you can migrate users through [Graph API](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet).</span></span> <span data-ttu-id="f0b8b-110">Proces migrace uživatele, které patří do dvou toky:</span><span class="sxs-lookup"><span data-stu-id="f0b8b-110">The user migration process falls into two flows:</span></span>

* <span data-ttu-id="f0b8b-111">**Před migrací**: Tento tok platí, pokud máte buď zrušte přístup k přihlašovacím údajům uživatele (uživatelské jméno a heslo) nebo přihlašovací údaje jsou šifrované, ale lze je dešifrovat.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-111">**Pre-migration**: This flow applies when you either have clear access to a user's credentials (user name and password) or the credentials are encrypted, but you can decrypt them.</span></span> <span data-ttu-id="f0b8b-112">Proces před migrací zahrnuje čtení z původního zprostředkovatele identity uživatelů a vytváření nových účtů v adresáři Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-112">The pre-migration process involves reading the users from the old identity provider and creating new accounts in the Azure AD B2C directory.</span></span>

* <span data-ttu-id="f0b8b-113">**Před migrací a heslo resetovat**: Tento tok platí, pokud heslo uživatele není dostupný.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-113">**Pre-migration and password reset**: This flow applies when a user's password is not accessible.</span></span> <span data-ttu-id="f0b8b-114">Příklad:</span><span class="sxs-lookup"><span data-stu-id="f0b8b-114">For example:</span></span>
    * <span data-ttu-id="f0b8b-115">Je heslo uloženo ve formátu HASH.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-115">The password is stored in HASH format.</span></span>
    * <span data-ttu-id="f0b8b-116">Heslo je uloženo v zprostředkovatele identity, ke kterému nelze přistupovat.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-116">The password is stored in an identity provider that you can't access.</span></span> <span data-ttu-id="f0b8b-117">Původní zprostředkovatele identity ověří přihlašovací údaje uživatele při volání webové služby.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-117">Your old identity provider validates the user credential by calling a web service.</span></span>

<span data-ttu-id="f0b8b-118">V obou toky je nejprve spustit proces před migrací, čtení uživatelů z původního zprostředkovatele identity a vytvořit nové účty v adresáři Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-118">In both flows, you first run the pre-migration process, read the users from your old identity provider, and create new accounts in the Azure AD B2C directory.</span></span> <span data-ttu-id="f0b8b-119">Pokud nemáte heslo, vytvoříte účet pomocí náhodně generovaný heslo.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-119">If you do not have the password, you create the account by using a password that's generated randomly.</span></span> <span data-ttu-id="f0b8b-120">Pak požádejte uživatele, chcete-li změnit heslo, nebo při prvním přihlášení uživatele, Azure AD B2C požádá uživatele, aby ho resetovat.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-120">You then ask the user to change the password or, when the user signs in for the first time, Azure AD B2C asks the user to reset it.</span></span>

## <a name="password-policy"></a><span data-ttu-id="f0b8b-121">Zásady hesel</span><span class="sxs-lookup"><span data-stu-id="f0b8b-121">Password policy</span></span>
<span data-ttu-id="f0b8b-122">Zásady hesel Azure AD B2C (pro lokální účty) je založena na zásady služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-122">The Azure AD B2C password policy (for local accounts) is based on Azure AD policy.</span></span> <span data-ttu-id="f0b8b-123">Azure AD B2C registrace nebo přihlášení a heslo resetovat zásady použití síly hesla "silné" a není vypršení platnosti hesla.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-123">The Azure AD B2C sign-up or sign-in and password reset policies use the "strong" password strength and doesn't expire any passwords.</span></span> <span data-ttu-id="f0b8b-124">Další informace najdete v tématu [zásady hesel služby Azure AD](https://msdn.microsoft.com/library/azure/jj943764.aspx).</span><span class="sxs-lookup"><span data-stu-id="f0b8b-124">For more information, see [Azure AD password policy](https://msdn.microsoft.com/library/azure/jj943764.aspx).</span></span>

<span data-ttu-id="f0b8b-125">Pokud účty, které chcete migrovat pomocí slabší síly hesla, než [silné heslo sílu vynucováno Azure AD B2C](https://msdn.microsoft.com/library/azure/jj943764.aspx), můžete zakázat požadavek na silné heslo.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-125">If the accounts that you want to migrate use a weaker password strength than the [strong password strength enforced by Azure AD B2C](https://msdn.microsoft.com/library/azure/jj943764.aspx), you can disable the strong password requirement.</span></span> <span data-ttu-id="f0b8b-126">Chcete-li změnit výchozí zásady hesel, nastavte `passwordPolicies` vlastnost `DisableStrongPassword`.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-126">To change the default password policy, set the `passwordPolicies` property to `DisableStrongPassword`.</span></span> <span data-ttu-id="f0b8b-127">Požadavek na vytvoření uživatele můžete například upravit následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="f0b8b-127">For example, you can modify the create user request as follows:</span></span> 

```JSON
"passwordPolicies": "DisablePasswordExpiration, DisableStrongPassword"
```

## <a name="step-1-use-graph-api-to-migrate-users"></a><span data-ttu-id="f0b8b-128">Krok 1: Použití Graph API pro migraci uživatelů</span><span class="sxs-lookup"><span data-stu-id="f0b8b-128">Step 1: Use Graph API to migrate users</span></span>
<span data-ttu-id="f0b8b-129">Vytváření účtu uživatele Azure AD B2C prostřednictvím rozhraní Graph API (heslo nebo s náhodné heslo).</span><span class="sxs-lookup"><span data-stu-id="f0b8b-129">You create the Azure AD B2C user account via Graph API (with the password or with a random password).</span></span> <span data-ttu-id="f0b8b-130">Tato část popisuje proces vytváření uživatelských účtů v adresáři Azure AD B2C pomocí rozhraní Graph API.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-130">This section describes the process of creating user accounts in the Azure AD B2C directory by using Graph API.</span></span>

### <a name="step-11-register-your-application-in-your-tenant"></a><span data-ttu-id="f0b8b-131">Krok 1.1: Registrace vaší aplikace ve vašem klientovi</span><span class="sxs-lookup"><span data-stu-id="f0b8b-131">Step 1.1: Register your application in your tenant</span></span>
<span data-ttu-id="f0b8b-132">Ke komunikaci s rozhraní Graph API, nejprve musí mít účet služby s oprávněními správce.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-132">To communicate with the Graph API, you first must have a service account with administrative privileges.</span></span> <span data-ttu-id="f0b8b-133">Ve službě Azure AD je třeba zaregistrovat aplikaci a ověřování do služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-133">In Azure AD, you register an application and authentication to Azure AD.</span></span> <span data-ttu-id="f0b8b-134">Přihlašovací údaje aplikací jsou **ID aplikace** a **tajný klíč aplikace**.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-134">The application credentials are **Application ID** and **Application Secret**.</span></span> <span data-ttu-id="f0b8b-135">Aplikace funguje jako samostatně, nikoli jako uživatel, k volání rozhraní Graph API.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-135">The application acts as itself, not as a user, to call the Graph API.</span></span>

<span data-ttu-id="f0b8b-136">Nejprve zaregistrujte aplikaci migrace ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-136">First, register your migration application in Azure AD.</span></span> <span data-ttu-id="f0b8b-137">Pak vytvořte klíč pomocí aplikace (tajný klíč aplikace) a nastavte aplikaci s oprávnění k zápisu.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-137">Then, create an application key (application secret) and set the application with write privileges.</span></span>

1. <span data-ttu-id="f0b8b-138">Přihlaste se k webu [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="f0b8b-138">Sign in to the [Azure portal](https://portal.azure.com/).</span></span>

2. <span data-ttu-id="f0b8b-139">Zvolte služby Azure AD **B2C** klienta tak, že vyberete svůj účet v horní pravé části okna.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-139">Choose your Azure AD **B2C** tenant by selecting your account at the top right of the window.</span></span>

3. <span data-ttu-id="f0b8b-140">V levém podokně vyberte **Azure Active Directory** (ne Azure AD B2C).</span><span class="sxs-lookup"><span data-stu-id="f0b8b-140">In the left pane, select **Azure Active Directory** (not Azure AD B2C).</span></span> <span data-ttu-id="f0b8b-141">Pokud chcete ji najít, možná budete muset vybrat možnost **více služeb**.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-141">To find it, you might need to select **More Services**.</span></span>

4. <span data-ttu-id="f0b8b-142">Vyberte **registrace aplikace**.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-142">Select **App registrations**.</span></span>

5. <span data-ttu-id="f0b8b-143">Vyberte **nové registrace aplikace**.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-143">Select **New application registration**.</span></span>

    ![Registrace nové aplikace](media/active-directory-b2c-user-migration/pre-migration-app-registration.png)

6. <span data-ttu-id="f0b8b-145">Vytvoření nové aplikace následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="f0b8b-145">Create a new application by doing the following:</span></span>
    * <span data-ttu-id="f0b8b-146">Pro **název**, použijte **B2CUserMigration** nebo jakýkoli jiný název, který chcete.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-146">For **Name**, use **B2CUserMigration** or any other name you want.</span></span>
    * <span data-ttu-id="f0b8b-147">Pro **typ aplikace**, použijte **webové aplikace nebo rozhraní API**.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-147">For **Application type**, use **Web app/API**.</span></span>
    * <span data-ttu-id="f0b8b-148">Pro **přihlašovací adresa URL**, použijte **https://localhost** (protože není relevantní pro tuto aplikaci).</span><span class="sxs-lookup"><span data-stu-id="f0b8b-148">For **Sign-on URL**, use **https://localhost** (as it's not relevant for this application).</span></span>
    * <span data-ttu-id="f0b8b-149">Vyberte **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-149">Select **Create**.</span></span>

7. <span data-ttu-id="f0b8b-150">Po vytvoření aplikace, v **aplikace** seznam, vyberte nově vytvořenou **B2CUserMigration** aplikace.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-150">After the application is created, in the **Applications** list, select the newly created **B2CUserMigration** application.</span></span>

8. <span data-ttu-id="f0b8b-151">Vyberte **vlastnosti**, kopie **ID aplikace**a uložit pro pozdější použití.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-151">Select **Properties**, copy the **Application ID**, and save it for later.</span></span>

### <a name="step-12-create-the-application-secret"></a><span data-ttu-id="f0b8b-152">Krok 1.2: Vytvoření tajný klíč aplikace</span><span class="sxs-lookup"><span data-stu-id="f0b8b-152">Step 1.2: Create the application secret</span></span>
1. <span data-ttu-id="f0b8b-153">Na portálu Azure **zaregistrovat aplikaci** vyberte **klíče**.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-153">In the Azure portal **Registered App** window, select **Keys**.</span></span>

2. <span data-ttu-id="f0b8b-154">Přidejte nový klíč (také označované jako tajný klíč klienta) a zkopírujte klíč pro pozdější použití.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-154">Add a new key (also known as a client secret), and then copy the key for later use.</span></span>

    ![ID aplikace a klíče](media/active-directory-b2c-user-migration/pre-migration-app-id-and-key.png)

### <a name="step-13-grant-administrative-permission-to-your-application"></a><span data-ttu-id="f0b8b-156">Krok 1.3: Udělení oprávnění pro správu do vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="f0b8b-156">Step 1.3: Grant administrative permission to your application</span></span>
1. <span data-ttu-id="f0b8b-157">Na portálu Azure **zaregistrovat aplikaci** vyberte **požadovaná oprávnění**.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-157">In the Azure portal **Registered App** window, select **Required permissions**.</span></span>

2. <span data-ttu-id="f0b8b-158">Vyberte **Windows Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-158">Select **Windows Azure Active Directory**.</span></span>

3. <span data-ttu-id="f0b8b-159">V **povolit přístup** podokně v části **oprávnění aplikací**, vyberte **pro čtení a zápis dat adresáře**a potom vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-159">In the **Enable Access** pane, under **Application Permissions**, select **Read and write directory data**, and then select **Save**.</span></span>

4. <span data-ttu-id="f0b8b-160">V **požadovaná oprávnění** podokně, vyberte **udělit oprávnění**.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-160">In the **Required permissions** pane, select **Grant Permissions**.</span></span>

    ![Oprávnění aplikací](media/active-directory-b2c-user-migration/pre-migration-app-registration-permissions.png)

<span data-ttu-id="f0b8b-162">Nyní máte aplikace s oprávněními k vytváření, čtení a aktualizaci uživatele z vašeho klienta Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-162">Now you have an application with permissions to create, read, and update users from your Azure AD B2C tenant.</span></span>

### <a name="step-14-optional-environment-cleanup"></a><span data-ttu-id="f0b8b-163">Krok 1.4: Vyčištění prostředí (volitelné)</span><span class="sxs-lookup"><span data-stu-id="f0b8b-163">Step 1.4: (Optional) Environment cleanup</span></span>
<span data-ttu-id="f0b8b-164">Čtení a zápis dat oprávnění directory *není* zahrnovat právo odstranit uživatele.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-164">Read and write directory data permissions do *not* include the right to delete users.</span></span> <span data-ttu-id="f0b8b-165">Chcete-li poskytují aplikace možnost odstranit uživatele (za účelem vyčištění prostředí), musíte provést další krok, který zahrnuje prostředí PowerShell a nastavit oprávnění pro správce účtu uživatele.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-165">To give your application the ability to delete users (to clean up your environment), you must perform an extra step, which involves running PowerShell to set User Account Administrator permissions.</span></span> <span data-ttu-id="f0b8b-166">Jinak můžete přeskočit k další části.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-166">Otherwise, you can skip to the next section.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f0b8b-167">Musíte použít účet správce klienta B2C, který je *místní* klienta B2C.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-167">You must use a B2C tenant administrator account that is *local* to the B2C tenant.</span></span> <span data-ttu-id="f0b8b-168">Syntaxe názvu účtu není  *admin@contosob2c.onmicrosoft.com* .</span><span class="sxs-lookup"><span data-stu-id="f0b8b-168">The account name syntax is *admin@contosob2c.onmicrosoft.com*.</span></span>

>[!NOTE]
> <span data-ttu-id="f0b8b-169">Následující skript prostředí PowerShell vyžaduje [Azure Active Directory PowerShell verze 2](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?view=azureadps-2.0).</span><span class="sxs-lookup"><span data-stu-id="f0b8b-169">The following PowerShell script requires [Azure Active Directory PowerShell Version 2](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?view=azureadps-2.0).</span></span>

<span data-ttu-id="f0b8b-170">V tento skript prostředí PowerShell postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="f0b8b-170">In this PowerShell script, do the following:</span></span>
1. <span data-ttu-id="f0b8b-171">Připojení k online službě.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-171">Connect to your online service.</span></span> <span data-ttu-id="f0b8b-172">Chcete-li tak učinit, spusťte `Connect-AzureAD` rutiny v prostředí Windows PowerShell, příkazový řádek a zadejte svoje přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-172">To do so, run the `Connect-AzureAD` cmdlet at the Windows PowerShell command prompt, and provide your credentials.</span></span> 

2. <span data-ttu-id="f0b8b-173">Použití **ID aplikace** přiřadit role správce účtu uživatele aplikace.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-173">Use the **Application ID** to assign the application the user account administrator role.</span></span> <span data-ttu-id="f0b8b-174">Tyto role mají dobře známé identifikátory, tak, aby všechny musíte udělat zadejte vaše **ID aplikace** ve skriptu.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-174">These roles have well-known identifiers, so all you need to do is enter your **Application ID** in the script.</span></span>

```PowerShell
Connect-AzureAD

$AppId = "<Your application ID>"

# Fetch Azure AD application to assign to role
$roleMember = Get-AzureADServicePrincipal -Filter "AppId eq '$AppId'"

# Fetch User Account Administrator role instance
$role = Get-AzureADDirectoryRole | Where-Object {$_.displayName -eq 'User Account Administrator'}

# If role instance does not exist, instantiate it based on the role template
if ($role -eq $null) {
    # Instantiate an instance of the role template
    $roleTemplate = Get-AzureADDirectoryRoleTemplate | Where-Object {$_.displayName -eq 'User Account Administrator'}
    Enable-AzureADDirectoryRole -RoleTemplateId $roleTemplate.ObjectId

    # Fetch User Account Administrator role instance again
    $role = Get-AzureADDirectoryRole | Where-Object {$_.displayName -eq 'User Account Administrator'}
}

# Add application to role
Add-AzureADDirectoryRoleMember -ObjectId $role.ObjectId -RefObjectId $roleMember.ObjectId

# Fetch role membership for role to confirm
Get-AzureADDirectoryRoleMember -ObjectId $role.ObjectId
```

<span data-ttu-id="f0b8b-175">Změna `$AppId` hodnotu s Azure AD **ID aplikace**.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-175">Change the `$AppId` value with your Azure AD **Application ID**.</span></span>

## <a name="step-2-pre-migration-application-sample"></a><span data-ttu-id="f0b8b-176">Krok 2: Ukázka aplikace před migrací</span><span class="sxs-lookup"><span data-stu-id="f0b8b-176">Step 2: Pre-migration application sample</span></span>
<span data-ttu-id="f0b8b-177">[Stáhnout a spustit ukázkový kód](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-user-migration).</span><span class="sxs-lookup"><span data-stu-id="f0b8b-177">[Download and run the sample code](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-user-migration).</span></span> <span data-ttu-id="f0b8b-178">Můžete ho stáhnout jako soubor ZIP.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-178">You can download it as a .zip file.</span></span>

### <a name="step-21-edit-the-migration-data-file"></a><span data-ttu-id="f0b8b-179">Krok 2.1: Upravit soubor dat migrace</span><span class="sxs-lookup"><span data-stu-id="f0b8b-179">Step 2.1: Edit the migration data file</span></span>
<span data-ttu-id="f0b8b-180">Ukázková aplikace používá soubor JSON, který obsahuje data fiktivní uživatele.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-180">The sample app uses a JSON file that contains dummy user data.</span></span> <span data-ttu-id="f0b8b-181">Po vás ukázku úspěšně spustíte můžete změnit kód, který využívají data z vlastní databáze.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-181">After you successfully run the sample, you can change the code to consume the data from your own database.</span></span> <span data-ttu-id="f0b8b-182">Nebo můžete exportovat profil uživatele do souboru JSON a poté nastavte aplikaci používat tento soubor.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-182">Or you can export the user profile to a JSON file, and then set the app to use that file.</span></span>

<span data-ttu-id="f0b8b-183">Chcete-li upravit soubor JSON, otevřete `AADB2C.UserMigration.sln` řešení sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-183">To edit the JSON file, open the `AADB2C.UserMigration.sln` Visual Studio solution.</span></span> <span data-ttu-id="f0b8b-184">V `AADB2C.UserMigration` projekt, otevřete `UsersData.json` souboru.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-184">In the `AADB2C.UserMigration` project, open the `UsersData.json` file.</span></span>

![Soubor dat uživatele](media/active-directory-b2c-user-migration/pre-migration-data-file.png)

<span data-ttu-id="f0b8b-186">Jak vidíte, soubor obsahuje seznam entitami uživatelů.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-186">As you can see, the file contains a list of user entities.</span></span> <span data-ttu-id="f0b8b-187">Každá entita uživatel má následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="f0b8b-187">Each user entity has the following properties:</span></span>
* <span data-ttu-id="f0b8b-188">e-mail</span><span class="sxs-lookup"><span data-stu-id="f0b8b-188">email</span></span>
* <span data-ttu-id="f0b8b-189">displayName</span><span class="sxs-lookup"><span data-stu-id="f0b8b-189">displayName</span></span>
* <span data-ttu-id="f0b8b-190">firstName</span><span class="sxs-lookup"><span data-stu-id="f0b8b-190">firstName</span></span>
* <span data-ttu-id="f0b8b-191">lastName</span><span class="sxs-lookup"><span data-stu-id="f0b8b-191">lastName</span></span>
* <span data-ttu-id="f0b8b-192">heslo (nesmí být prázdné)</span><span class="sxs-lookup"><span data-stu-id="f0b8b-192">password (can be empty)</span></span>

> [!NOTE]
> <span data-ttu-id="f0b8b-193">Při kompilaci, Visual Studio zkopíruje soubor do `bin` adresáře.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-193">At compile time, Visual Studio copies the file to the `bin` directory.</span></span>

### <a name="step-22-configure-the-application-settings"></a><span data-ttu-id="f0b8b-194">Krok 2.2: Konfigurace nastavení aplikace</span><span class="sxs-lookup"><span data-stu-id="f0b8b-194">Step 2.2: Configure the application settings</span></span>
<span data-ttu-id="f0b8b-195">V části `AADB2C.UserMigration` projekt, otevřete *App.config* souboru.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-195">Under the `AADB2C.UserMigration` project, open the *App.config* file.</span></span> <span data-ttu-id="f0b8b-196">Následující nastavení aplikace nahraďte vlastními hodnotami:</span><span class="sxs-lookup"><span data-stu-id="f0b8b-196">Replace the following app settings with your own values:</span></span>

```XML
<appSettings>
    <add key="b2c:Tenant" value="{Your Tenant Name}" />
    <add key="b2c:ClientId" value="{The ApplicationID from above}" />
    <add key="b2c:ClientSecret" value="{The Key from above}" />
    <add key="MigrationFile" value="{Name of a JSON file containing the users' data; for example, UsersData.json}" />
    <add key="BlobStorageConnectionString" value="{Your connection Azure table string}" />
    
</appSettings>
```

> [!NOTE]
> * <span data-ttu-id="f0b8b-197">Použití služby Azure table připojovací řetězec je popsané v následujících částech.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-197">The use of an Azure table connection string is described in the next sections.</span></span>
> * <span data-ttu-id="f0b8b-198">Název vašeho klienta B2C je doména, kterou jste zadali při vytváření klienta a ten se zobrazí na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-198">Your B2C tenant name is the domain that you entered during tenant creation, and it is displayed in the Azure portal.</span></span> <span data-ttu-id="f0b8b-199">Název klienta většinou končí příponou *. onmicrosoft.com* (například *contosob2c.onmicrosoft.com*).</span><span class="sxs-lookup"><span data-stu-id="f0b8b-199">The tenant name usually ends with the suffix *.onmicrosoft.com* (for example, *contosob2c.onmicrosoft.com*).</span></span>
>

### <a name="step-23-run-the-pre-migration-process"></a><span data-ttu-id="f0b8b-200">Krok 2.3: Spusťte proces před migrací</span><span class="sxs-lookup"><span data-stu-id="f0b8b-200">Step 2.3: Run the pre-migration process</span></span>
<span data-ttu-id="f0b8b-201">Klikněte pravým tlačítkem myši `AADB2C.UserMigration` řešení a pak znovu sestavit ukázku.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-201">Right-click the `AADB2C.UserMigration` solution, and then rebuild the sample.</span></span> <span data-ttu-id="f0b8b-202">Pokud jste úspěšné, teď byste měli mít `UserMigration.exe` spustitelný soubor umístěný ve `AADB2C.UserMigration\bin\Debug`.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-202">If you are successful, you should now have a `UserMigration.exe` executable file located in `AADB2C.UserMigration\bin\Debug`.</span></span> <span data-ttu-id="f0b8b-203">Pokud chcete spustit proces migrace, použijte jednu z následujících parametrů příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="f0b8b-203">To run the migration process, use one of the following command-line parameters:</span></span>

* <span data-ttu-id="f0b8b-204">K **migrovat uživatele s heslem**, použijte `UserMigration.exe 1` příkaz.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-204">To **migrate users with password**, use the `UserMigration.exe 1` command.</span></span>

* <span data-ttu-id="f0b8b-205">K **migraci uživatelů s náhodné heslo**, použijte `UserMigration.exe 2` příkaz.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-205">To **migrate users with random password**, use the `UserMigration.exe 2` command.</span></span> <span data-ttu-id="f0b8b-206">Tato operace vytvoří také entitu tabulky Azure.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-206">This operation also creates an Azure table entity.</span></span> <span data-ttu-id="f0b8b-207">Později nakonfigurujte zásady tak, aby volání rozhraní REST API služby.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-207">Later, you configure the policy to call the REST API service.</span></span> <span data-ttu-id="f0b8b-208">Služba Azure table používá ke sledování a správě procesu migrace.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-208">The service uses an Azure table to track and manage the migration process.</span></span>

![Ukázkový proces migrace](media/active-directory-b2c-user-migration/pre-migration-demo.png)

### <a name="step-24-check-the-pre-migration-process"></a><span data-ttu-id="f0b8b-210">Krok 2.4: Kontrola proces před migrací</span><span class="sxs-lookup"><span data-stu-id="f0b8b-210">Step 2.4: Check the pre-migration process</span></span>
<span data-ttu-id="f0b8b-211">Ověření migrace, použijte jednu z následujících dvou metod:</span><span class="sxs-lookup"><span data-stu-id="f0b8b-211">To validate the migration, use one of the following two methods:</span></span>

* <span data-ttu-id="f0b8b-212">Chcete-li vyhledat uživatele podle zobrazovaného názvu, použijte portál Azure:</span><span class="sxs-lookup"><span data-stu-id="f0b8b-212">To search for a user by display name, use the Azure portal:</span></span>
    
    <span data-ttu-id="f0b8b-213">a.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-213">a.</span></span> <span data-ttu-id="f0b8b-214">Otevřete **Azure AD B2C**a potom vyberte **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-214">Open **Azure AD B2C**, and then select **Users and Groups**.</span></span>

    <span data-ttu-id="f0b8b-215">b.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-215">b.</span></span> <span data-ttu-id="f0b8b-216">Do vyhledávacího pole zadejte zobrazovaný název uživatele a zobrazte profil uživatele.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-216">In the search box, type the user's display name, and then view the user's profile.</span></span> 

* <span data-ttu-id="f0b8b-217">Chcete-li načíst uživatele tak, že přihlášení e-mailovou adresu, použijte této ukázkové aplikaci:</span><span class="sxs-lookup"><span data-stu-id="f0b8b-217">To retrieve a user by sign-in email address, use this sample application:</span></span>

    <span data-ttu-id="f0b8b-218">a.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-218">a.</span></span> <span data-ttu-id="f0b8b-219">Spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f0b8b-219">Run the following command:</span></span>

    ```Console
        UserMigration.exe 3 {email address}
    ```
        
    > [!TIP]
    > <span data-ttu-id="f0b8b-220">Můžete také uložit výstup do souboru JSON pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="f0b8b-220">You can also save the output to a JSON file by using the following command:</span></span>
    >
    >```Console
    >    UserMigration.exe 3 {email address} >> UserProfile.json
    >```

    > [!TIP]
    > <span data-ttu-id="f0b8b-221">Můžete také načíst uživatele podle zobrazovaného názvu pomocí následujícího příkazu: `UserMigration.exe 4 <Display name>`.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-221">You can also retrieve a user by display name by using the following command: `UserMigration.exe 4 <Display name>`.</span></span>

    <span data-ttu-id="f0b8b-222">b.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-222">b.</span></span> <span data-ttu-id="f0b8b-223">Otevřete *UserProfile.json* souboru v editoru JSON.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-223">Open the *UserProfile.json* file in a JSON editor.</span></span> <span data-ttu-id="f0b8b-224">S kódem jazyka Visual Studio, můžete ho naformátovat dokumentu JSON výběrem Shift + Alt + F nebo výběrem **formátovat dokument** v místní nabídce.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-224">With Visual Studio Code, you can format a JSON document by selecting Shift+Alt+F or by selecting **Format Document** in the context menu.</span></span>

    ![The UserProfile.json file](media/active-directory-b2c-user-migration/pre-migration-get-by-email2.png)

### <a name="step-25-optional-environment-cleanup"></a><span data-ttu-id="f0b8b-226">Krok 2.5: Vyčištění prostředí (volitelné)</span><span class="sxs-lookup"><span data-stu-id="f0b8b-226">Step 2.5: (Optional) Environment cleanup</span></span>
<span data-ttu-id="f0b8b-227">Pokud chcete vyčistit až klientovi Azure AD a odebrat uživatele z adresáře služby Azure AD, spusťte `UserMigration.exe 5` příkaz.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-227">If you want to clean up your Azure AD tenant and remove users from the Azure AD directory, run the `UserMigration.exe 5` command.</span></span>

> [!NOTE]
> * <span data-ttu-id="f0b8b-228">Vyčistěte váš klient, nakonfigurujte oprávnění správce účtu uživatele pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-228">To clean up your tenant, configure User Account Administrator permissions for your application.</span></span>
> * <span data-ttu-id="f0b8b-229">Ukázková aplikace migrace vyčistí všichni uživatelé, kteří jsou uvedeny v souboru JSON.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-229">The sample migration app cleans up all users who are listed in the JSON file.</span></span>

### <a name="step-26-sign-in-with-migrated-users-with-password"></a><span data-ttu-id="f0b8b-230">Krok 2.6: Přihlaste se pomocí migrovanými uživateli (s heslem)</span><span class="sxs-lookup"><span data-stu-id="f0b8b-230">Step 2.6: Sign in with migrated users (with password)</span></span>
<span data-ttu-id="f0b8b-231">Po spuštění proces před migrací s hesly uživatelské účty jsou připravené k použití a moct uživatel přihlásit do vaší aplikace pomocí Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-231">After you run the pre-migration process with user passwords, the accounts are ready to use, and users can sign in to your application by using Azure AD B2C.</span></span> <span data-ttu-id="f0b8b-232">Pokud nemáte přístup k uživatelská hesla, pokračujte v další části.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-232">If you don't have access to user passwords, continue to the next section.</span></span>

## <a name="step-3-help-users-reset-their-password"></a><span data-ttu-id="f0b8b-233">Krok 3: Pomoc uživatelům resetovat heslo</span><span class="sxs-lookup"><span data-stu-id="f0b8b-233">Step 3: Help users reset their password</span></span>
<span data-ttu-id="f0b8b-234">Pokud provádíte migraci uživatelů s náhodné heslo, se musí obnovit své heslo.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-234">If you migrate users with a random password, they must reset their password.</span></span> <span data-ttu-id="f0b8b-235">Aby mohl snadněji resetování hesla, odeslání Uvítacího e-mailu s odkazem k resetování hesla.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-235">To help them reset the password, send a welcome email with a link to reset the password.</span></span>

<span data-ttu-id="f0b8b-236">Chcete-li získat odkaz na vaše zásady resetování hesel, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="f0b8b-236">To get the link to your password reset policy, do the following:</span></span>

1. <span data-ttu-id="f0b8b-237">Vyberte **nastavení Azure AD B2C**a potom vyberte **resetovat heslo** vlastnosti zásad.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-237">Select **Azure AD B2C Settings**, and then select **Reset password** policy properties.</span></span>

2. <span data-ttu-id="f0b8b-238">Vyberte svou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-238">Select your application.</span></span>

    >[!NOTE]
    ><span data-ttu-id="f0b8b-239">Spustit nyní vyžaduje alespoň jedné aplikace do být preregistered u klienta.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-239">Run Now requires at least one application to be preregistered on the tenant.</span></span> <span data-ttu-id="f0b8b-240">Další postup registrace aplikace najdete v tématu Azure AD B2C [Začínáme](active-directory-b2c-get-started.md) článek nebo [registrace aplikace](active-directory-b2c-app-registration.md) článku.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-240">To learn how to register applications, see the Azure AD B2C [Get started](active-directory-b2c-get-started.md) article or the [Application registration](active-directory-b2c-app-registration.md) article.</span></span> 

3. <span data-ttu-id="f0b8b-241">Vyberte **spustit nyní**a potom zkontrolujte zásady.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-241">Select **Run now**, and then check the policy.</span></span>

4. <span data-ttu-id="f0b8b-242">V **spustit nyní koncový bod** pole, zkopírujte adresu URL a potom ji odešlete uživatelům.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-242">In the **Run now endpoint** box, copy the URL, and then send it to your users.</span></span>

    ![Sada protokolů diagnostiky](media/active-directory-b2c-user-migration/pre-migration-policy-uri.png)

## <a name="step-4-optional-change-your-policy-to-check-and-set-the-user-migration-status"></a><span data-ttu-id="f0b8b-244">Krok 4: (Volitelné) změnu vaše zásady ke kontrole a nastavení migrace stavu uživatele</span><span class="sxs-lookup"><span data-stu-id="f0b8b-244">Step 4: (Optional) Change your policy to check and set the user migration status</span></span>

> [!NOTE]
> <span data-ttu-id="f0b8b-245">Chcete-li zkontrolovat a změnit migrace stavu uživatele, musíte použít vlastní zásadu.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-245">To check and change the user migration status, you must use a custom policy.</span></span> <span data-ttu-id="f0b8b-246">Další informace najdete v tématu [začít pracovat s vlastními zásadami](active-directory-b2c-get-started-custom.md).</span><span class="sxs-lookup"><span data-stu-id="f0b8b-246">For more information, see [Get started with custom policies](active-directory-b2c-get-started-custom.md).</span></span>
>

<span data-ttu-id="f0b8b-247">Když uživatelé se pokusí přihlásit se bez nejprve resetuje se heslo, vaše zásady by měl vrátit popisný chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-247">When users try to sign in without resetting the password first, your policy should return a friendly error message.</span></span> <span data-ttu-id="f0b8b-248">Příklad:</span><span class="sxs-lookup"><span data-stu-id="f0b8b-248">For example:</span></span> 
><span data-ttu-id="f0b8b-249">*Vašeho hesla vypršela. Aby to udělal, vyberte odkaz resetovat heslo.*</span><span class="sxs-lookup"><span data-stu-id="f0b8b-249">*Your password has expired. To reset it, select the Reset Password link.*</span></span> 

<span data-ttu-id="f0b8b-250">Tento volitelný krok vyžaduje použití vlastních zásad Azure AD B2C, jak je popsáno v [Začínáme s vlastními zásadami](active-directory-b2c-get-started-custom.md) článku.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-250">This optional step requires the use of Azure AD B2C custom policies, as described in the [Getting started with custom policies](active-directory-b2c-get-started-custom.md) article.</span></span>

<span data-ttu-id="f0b8b-251">V této části změnit zásady tak, aby zkontrolovat stav migrace uživatelů na přihlášení.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-251">In this section, you change the policy to check the user migration status on sign-in.</span></span> <span data-ttu-id="f0b8b-252">Pokud uživatel nebyl změnit heslo, vrátí chybovou zprávu protokolu HTTP 409 uživateli výzvu k výběru **zapomněli jste heslo?** odkaz.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-252">If the user didn't change the password, return an HTTP 409 error message that asks the user to select the **Forgot your password?** link.</span></span> 

<span data-ttu-id="f0b8b-253">Chcete-li sledovat Změna hesla, použijte Azure table.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-253">To track the password change, you use an Azure table.</span></span> <span data-ttu-id="f0b8b-254">Když spustíte proces před migrací s parametrem příkazového řádku `2`, vytvořit entitu uživatele v Azure tabulce.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-254">When you run the pre-migration process with the command-line parameter `2`, you create a user entity in an Azure table.</span></span> <span data-ttu-id="f0b8b-255">Služby provede následující akce:</span><span class="sxs-lookup"><span data-stu-id="f0b8b-255">Your service does the following:</span></span>

* <span data-ttu-id="f0b8b-256">Na přihlášení vyvolá zásad Azure AD B2C migrace služba RESTful, odesílání e-mailovou zprávu jako vstupní deklarace identity.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-256">On sign-in, the Azure AD B2C policy invokes your migration RESTful service, sending an email message as an input claim.</span></span> <span data-ttu-id="f0b8b-257">Služba hledá e-mailovou adresu v tabulce Azure.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-257">The service searches for the email address in the Azure table.</span></span> <span data-ttu-id="f0b8b-258">Pokud existuje daná adresa služby vyvolá chybovou zprávu: *je nutné změnit heslo*.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-258">If the address exists, the service throws an error message: *You must change password*.</span></span>

* <span data-ttu-id="f0b8b-259">Jakmile uživatel úspěšně změní heslo, odeberte z tabulky Azure entity.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-259">After the user successfully changes the password, remove the entity from the Azure table.</span></span>

>[!NOTE]
><span data-ttu-id="f0b8b-260">Pro zjednodušení ukázce používáme Azure table.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-260">We use an Azure table to simplify the sample.</span></span> <span data-ttu-id="f0b8b-261">V některé z databází nebo jako vlastní vlastnost v účtu Azure AD B2C můžete uložit stav migrace.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-261">You can store the migration status in any database or as a custom property in the Azure AD B2C account.</span></span>

### <a name="41-update-your-application-setting"></a><span data-ttu-id="f0b8b-262">4.1: aktualizace nastavení vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="f0b8b-262">4.1: Update your application setting</span></span>
1. <span data-ttu-id="f0b8b-263">Chcete-li otestovat ukázkové rozhraní RESTful API, otevřete `AADB2C.UserMigration.sln` řešení sady Visual Studio v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-263">To test the RESTful API demo, open the `AADB2C.UserMigration.sln` Visual Studio solution in Visual Studio.</span></span> 

2. <span data-ttu-id="f0b8b-264">V `AADB2C.UserMigration.API` projekt, otevřete *App.config* souboru.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-264">In the `AADB2C.UserMigration.API` project, open the *App.config* file.</span></span> <span data-ttu-id="f0b8b-265">Nastavení aplikace nahraďte vlastní hodnotu:</span><span class="sxs-lookup"><span data-stu-id="f0b8b-265">Replace the app setting with your own value:</span></span>

    ```XML
    <appSettings>
        <add key="BlobStorageConnectionString" value="{The Azure Blob storage connection string"} />
    </appSettings>
    ```

### <a name="step-42-deploy-your-web-application-to-azure-app-service"></a><span data-ttu-id="f0b8b-266">Krok 4.2: Nasazení webové aplikace do služby Azure App Service</span><span class="sxs-lookup"><span data-stu-id="f0b8b-266">Step 4.2: Deploy your web application to Azure App Service</span></span>
<span data-ttu-id="f0b8b-267">Publikování vaši rozhraní API služby Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-267">Publish your API service to Azure App Service.</span></span> <span data-ttu-id="f0b8b-268">Další informace najdete v tématu [nasazení vaší aplikace do Azure App Service](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy).</span><span class="sxs-lookup"><span data-stu-id="f0b8b-268">For more information, see [Deploy your app to Azure App Service](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy).</span></span>

### <a name="step-43-add-a-technical-profile-and-technical-profile-validation-to-your-policy"></a><span data-ttu-id="f0b8b-269">Krok 4.3: Přidejte technické profilu a technické profil ověření do vaší zásady</span><span class="sxs-lookup"><span data-stu-id="f0b8b-269">Step 4.3: Add a technical profile and technical profile validation to your policy</span></span> 
1. <span data-ttu-id="f0b8b-270">V pracovním adresáři, otevřete *TrustFrameworkExtensions.xml* soubor rozšíření zásad.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-270">In your working directory, open the *TrustFrameworkExtensions.xml* extension policy file.</span></span> 

2. <span data-ttu-id="f0b8b-271">Vyhledejte `<ClaimsProviders>` uzel a potom v uzlu, přidejte následující fragment kódu XML.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-271">Search for the `<ClaimsProviders>` node and then, in the node, add the following XML snippet.</span></span> <span data-ttu-id="f0b8b-272">Ujistěte se, chcete-li změnit hodnotu `ServiceUrl` tak, aby odkazoval na vaši adresu URL koncového bodu.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-272">Be sure to change the value of `ServiceUrl` to point to your endpoint URL.</span></span> 

    ```XML
    <ClaimsProvider>
        <DisplayName>REST APIs</DisplayName>
        <TechnicalProfiles>
    
        <TechnicalProfile Id="LocalAccountSignIn">
            <DisplayName>Local account just in time migration</DisplayName>
            <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
            <Metadata>
            <Item Key="ServiceUrl">http://{your-app}.azurewebsites.net/api/PrePasswordReset/LoalAccountSignIn</Item>
            <Item Key="AuthenticationType">None</Item>
            <Item Key="SendClaimsIn">Body</Item>
            </Metadata>
            <InputClaims>
            <InputClaim ClaimTypeReferenceId="signInName" PartnerClaimType="email" />
            </InputClaims>
            <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
        </TechnicalProfile>
    
        <TechnicalProfile Id="LocalAccountPasswordReset">
            <DisplayName>Local account just in time migration</DisplayName>
            <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
            <Metadata>
            <Item Key="ServiceUrl">http://{your-app}.azurewebsites.net/api/PrePasswordReset/PasswordUpdated</Item>
            <Item Key="AuthenticationType">None</Item>
            <Item Key="SendClaimsIn">Body</Item>
            </Metadata>
            <InputClaims>
            <InputClaim ClaimTypeReferenceId="email" PartnerClaimType="email" />
            </InputClaims>
            <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
        </TechnicalProfile>
        </TechnicalProfiles>
    </ClaimsProvider>
    
    <ClaimsProvider>
        <DisplayName>Local Account</DisplayName>
        <TechnicalProfiles>
    
        <!-- This technical profile uses a validation technical profile to authenticate the user. -->
        <TechnicalProfile Id="SelfAsserted-LocalAccountSignin-Email">
            <ValidationTechnicalProfiles>
            <ValidationTechnicalProfile ReferenceId="LocalAccountSignIn" />
            </ValidationTechnicalProfiles>
        </TechnicalProfile>
    
        <TechnicalProfile Id="LocalAccountWritePasswordUsingObjectId">
            <ValidationTechnicalProfiles>
            <ValidationTechnicalProfile ReferenceId="LocalAccountPasswordReset" />
            </ValidationTechnicalProfiles>
        </TechnicalProfile>
    
        </TechnicalProfiles>
    </ClaimsProvider>
    ```

<span data-ttu-id="f0b8b-273">Předchozí technické profil definuje jeden vstupní deklaraci identity: `signInName` (Odeslat jako e-mailu).</span><span class="sxs-lookup"><span data-stu-id="f0b8b-273">The preceding technical profile defines one input claim: `signInName` (send as email).</span></span> <span data-ttu-id="f0b8b-274">Na přihlášení deklarace identity posílá RESTful koncový bod.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-274">On sign-in, the claim is sent to your RESTful endpoint.</span></span>

<span data-ttu-id="f0b8b-275">Po definování technické profil pro vaše rozhraní RESTful API, řekněte vaše zásady Azure AD B2C volat technické profilu.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-275">After you define the technical profile for your RESTful API, tell your Azure AD B2C policy to call the technical profile.</span></span> <span data-ttu-id="f0b8b-276">Fragment kódu XML přepsání `SelfAsserted-LocalAccountSignin-Email`, která je definována v základní zásady.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-276">The XML snippet overrides `SelfAsserted-LocalAccountSignin-Email`, which is defined in the base policy.</span></span> <span data-ttu-id="f0b8b-277">Také přidá fragment kódu XML `ValidationTechnicalProfile`, s ReferenceId odkazující na váš profil technické `LocalAccountUserMigration`.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-277">The XML snippet also adds `ValidationTechnicalProfile`, with ReferenceId pointing to your technical profile `LocalAccountUserMigration`.</span></span> 

### <a name="step-44-upload-the-policy-to-your-tenant"></a><span data-ttu-id="f0b8b-278">Krok 4.4: Nahrajte zásady klienta</span><span class="sxs-lookup"><span data-stu-id="f0b8b-278">Step 4.4: Upload the policy to your tenant</span></span>
1. <span data-ttu-id="f0b8b-279">V [portál Azure](https://portal.azure.com), přepnout [kontextu klienta služby Azure AD B2C](active-directory-b2c-navigate-to-b2c-context.md)a potom vyberte **Azure AD B2C**.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-279">In the [Azure portal](https://portal.azure.com), switch to the [context of your Azure AD B2C tenant](active-directory-b2c-navigate-to-b2c-context.md), and then select **Azure AD B2C**.</span></span>

2. <span data-ttu-id="f0b8b-280">Vyberte **Identity rozhraní Framework**.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-280">Select **Identity Experience Framework**.</span></span>

3. <span data-ttu-id="f0b8b-281">Vyberte **všechny zásady**.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-281">Select **All Policies**.</span></span>

4. <span data-ttu-id="f0b8b-282">Vyberte **nahrát zásady**.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-282">Select **Upload Policy**.</span></span>

5. <span data-ttu-id="f0b8b-283">Vyberte **přepsat zásady, pokud existuje** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-283">Select the **Overwrite the policy if it exists** check box.</span></span>

6. <span data-ttu-id="f0b8b-284">Nahrát *TrustFrameworkExtensions.xml* souboru a ujistěte se, že předává ověření.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-284">Upload the *TrustFrameworkExtensions.xml* file, and ensure that it passes validation.</span></span>

### <a name="step-45-test-the-custom-policy-by-using-run-now"></a><span data-ttu-id="f0b8b-285">Krok 4.5: Testování spustit pomocí vlastních zásad</span><span class="sxs-lookup"><span data-stu-id="f0b8b-285">Step 4.5: Test the custom policy by using Run Now</span></span>
1. <span data-ttu-id="f0b8b-286">Vyberte **nastavení Azure AD B2C**a pak přejděte na **Identity rozhraní Framework**.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-286">Select **Azure AD B2C Settings**, and then go to **Identity Experience Framework**.</span></span>

2. <span data-ttu-id="f0b8b-287">Otevřete **B2C_1A_signup_signin**, předávající stranu vlastních zásad, které jste nahráli a potom vyberte **spustit nyní**.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-287">Open **B2C_1A_signup_signin**, the relying party (RP) custom policy that you uploaded, and then select **Run now**.</span></span>

3. <span data-ttu-id="f0b8b-288">Pokuste se přihlaste jedním z migrovaných uživatelské přihlašovací údaje a potom vyberte **přihlásit**.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-288">Try to sign in with one of the migrated users' credentials, and then select **Sign In**.</span></span> <span data-ttu-id="f0b8b-289">Rozhraní API REST vyvoláním se následující chybová zpráva:</span><span class="sxs-lookup"><span data-stu-id="f0b8b-289">Your REST API should throw the following error message:</span></span>

    ![Sada protokolů diagnostiky](media/active-directory-b2c-user-migration/pre-migration-error-message.png)

### <a name="step-46-optional-troubleshoot-your-rest-api"></a><span data-ttu-id="f0b8b-291">Krok 4.6: (Volitelné) řešení REST API</span><span class="sxs-lookup"><span data-stu-id="f0b8b-291">Step 4.6: (Optional) Troubleshoot your REST API</span></span>
<span data-ttu-id="f0b8b-292">Můžete zobrazit a sledování informací o protokolování v skoro v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-292">You can view and monitor logging information in near-real time.</span></span>

1. <span data-ttu-id="f0b8b-293">V nabídce nastavení RESTful aplikace v části **monitorování**, vyberte **diagnostické protokoly**.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-293">On your RESTful application's settings menu, under **Monitoring**, select **Diagnostic logs**.</span></span> 

2. <span data-ttu-id="f0b8b-294">Nastavit **protokolování aplikací (systém souborů)** k **na**.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-294">Set **Application Logging (Filesystem)** to **On**.</span></span> 

3. <span data-ttu-id="f0b8b-295">Nastavte **úroveň** k **podrobné**.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-295">Set the **Level** to **Verbose**.</span></span>

4. <span data-ttu-id="f0b8b-296">Vyberte **uložit**</span><span class="sxs-lookup"><span data-stu-id="f0b8b-296">Select **Save**</span></span>

    ![Sada protokolů diagnostiky](media/active-directory-b2c-user-migration/pre-migration-diagnostic-logs.png)

5. <span data-ttu-id="f0b8b-298">Na **nastavení** nabídce vyberte možnost **datový proud protokolu**.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-298">On the **Settings** menu, select **Log stream**.</span></span>

6. <span data-ttu-id="f0b8b-299">Zkontrolujte výstup rozhraní RESTful API.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-299">Check the output of the RESTful API.</span></span>

<span data-ttu-id="f0b8b-300">Další informace najdete v tématu [streamování protokoly a konzole](https://docs.microsoft.com/azure/app-service-web/web-sites-streaming-logs-and-console).</span><span class="sxs-lookup"><span data-stu-id="f0b8b-300">For more information, see [Streaming logs and the console](https://docs.microsoft.com/azure/app-service-web/web-sites-streaming-logs-and-console).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f0b8b-301">Diagnostické protokoly můžete použijte pouze během vývoje a testování.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-301">Use the diagnostics logs only during development and testing.</span></span> <span data-ttu-id="f0b8b-302">Výstup rozhraní RESTful API může obsahovat důvěrné informace, které by neměly být vystaveny v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-302">The RESTful API output might contain confidential information that should not be exposed in production.</span></span>
>

## <a name="optional-download-the-complete-policy-files"></a><span data-ttu-id="f0b8b-303">(Volitelné) Stáhnout soubory dokončení zásad</span><span class="sxs-lookup"><span data-stu-id="f0b8b-303">(Optional) Download the complete policy files</span></span>
<span data-ttu-id="f0b8b-304">Po dokončení [začít pracovat s vlastními zásadami](active-directory-b2c-get-started-custom.md) návod, doporučujeme vám vytvořit váš scénář pomocí vlastních zásad pro soubory.</span><span class="sxs-lookup"><span data-stu-id="f0b8b-304">After you complete the [Get started with custom policies](active-directory-b2c-get-started-custom.md) walkthrough, we recommend that you build your scenario by using your own custom policy files.</span></span> <span data-ttu-id="f0b8b-305">Pro vaši informaci uvádíme [ukázkové soubory zásad](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-user-migration).</span><span class="sxs-lookup"><span data-stu-id="f0b8b-305">For your reference, we have provided [Sample policy files](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-user-migration).</span></span> 
