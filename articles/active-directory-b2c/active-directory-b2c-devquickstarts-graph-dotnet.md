---
title: "Pomocí rozhraní Graph API – Azure AD B2C | Microsoft Docs"
description: "Postup pro volání rozhraní Graph API pro klienta B2C identity aplikací pro automatizaci procesu."
services: active-directory-b2c
documentationcenter: .net
author: parakhj
manager: mtillman
editor: parakhj
ms.assetid: f9904516-d9f7-43b1-ae4f-e4d9eb1c67a0
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/07/2017
ms.author: parakhj
ms.openlocfilehash: 33df6c4255d4ca672e65237c8be45b3f0bc7864e
ms.sourcegitcommit: 694e40a193980dea1e2f945471071f11030d5641
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/29/2018
---
# <a name="azure-ad-b2c-use-the-azure-ad-graph-api"></a><span data-ttu-id="a42d2-103">Azure AD B2C: Azure AD Graph API pomocí</span><span class="sxs-lookup"><span data-stu-id="a42d2-103">Azure AD B2C: Use the Azure AD Graph API</span></span>

>[!NOTE]
> <span data-ttu-id="a42d2-104">Je nutné použít [Azure AD Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-operations-overview?f=255&MSPPError=-2147217396) ke správě uživatelů v adresáři služby Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="a42d2-104">You must use the [Azure AD Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-operations-overview?f=255&MSPPError=-2147217396) to manage users in an Azure AD B2C directory.</span></span> <span data-ttu-id="a42d2-105">To se liší od společnosti Microsoft Graph API.</span><span class="sxs-lookup"><span data-stu-id="a42d2-105">This is different from the Microsoft Graph API.</span></span> <span data-ttu-id="a42d2-106">Další informace najdete [tady](https://blogs.msdn.microsoft.com/aadgraphteam/2016/07/08/microsoft-graph-or-azure-ad-graph/).</span><span class="sxs-lookup"><span data-stu-id="a42d2-106">Learn more [here](https://blogs.msdn.microsoft.com/aadgraphteam/2016/07/08/microsoft-graph-or-azure-ad-graph/).</span></span>

<span data-ttu-id="a42d2-107">Azure Active Directory (Azure AD) B2C klientů jsou obvykle velké.</span><span class="sxs-lookup"><span data-stu-id="a42d2-107">Azure Active Directory (Azure AD) B2C tenants tend to be very large.</span></span> <span data-ttu-id="a42d2-108">To znamená, že mnoho běžné úlohy správy klienta musí být provedeny prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="a42d2-108">This means that many common tenant management tasks need to be performed programmatically.</span></span> <span data-ttu-id="a42d2-109">Primární příkladem je Správa uživatelů.</span><span class="sxs-lookup"><span data-stu-id="a42d2-109">A primary example is user management.</span></span> <span data-ttu-id="a42d2-110">Možná budete muset migrovat stávající úložiště uživatele do klienta B2C.</span><span class="sxs-lookup"><span data-stu-id="a42d2-110">You might need to migrate an existing user store to a B2C tenant.</span></span> <span data-ttu-id="a42d2-111">Můžete pro hostování registrace uživatele na vlastní stránky a vytvářet uživatelské účty ve svém adresáři Azure AD B2C na pozadí.</span><span class="sxs-lookup"><span data-stu-id="a42d2-111">You may want to host user registration on your own page and create user accounts in your Azure AD B2C directory behind the scenes.</span></span> <span data-ttu-id="a42d2-112">Tyto typy úloh vyžadovat schopnost vytvářet, číst, aktualizovat a odstraňovat uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="a42d2-112">These types of tasks require the ability to create, read, update, and delete user accounts.</span></span> <span data-ttu-id="a42d2-113">Můžete provést tyto úlohy pomocí rozhraní Azure AD Graph API.</span><span class="sxs-lookup"><span data-stu-id="a42d2-113">You can do these tasks by using the Azure AD Graph API.</span></span>

<span data-ttu-id="a42d2-114">U klientů B2C existují dva primární režimy komunikaci pomocí rozhraní Graph API.</span><span class="sxs-lookup"><span data-stu-id="a42d2-114">For B2C tenants, there are two primary modes of communicating with the Graph API.</span></span>

* <span data-ttu-id="a42d2-115">Pro interaktivní, jedno spuštění úlohy by měla fungovat jako účet správce v klientovi B2C při provádění úlohy.</span><span class="sxs-lookup"><span data-stu-id="a42d2-115">For interactive, run-once tasks, you should act as an administrator account in the B2C tenant when you perform the tasks.</span></span> <span data-ttu-id="a42d2-116">Tento režim vyžaduje správce a přihlaste se pomocí přihlašovacích údajů, před kterou správce může provádět volání rozhraní Graph API.</span><span class="sxs-lookup"><span data-stu-id="a42d2-116">This mode requires an administrator to sign in with credentials before that admin can perform any calls to the Graph API.</span></span>
* <span data-ttu-id="a42d2-117">Pro automatizované, nepřetržité úlohy měli byste použít nějaký typ účtu služby, abyste poskytli potřebná oprávnění k provádění úloh správy.</span><span class="sxs-lookup"><span data-stu-id="a42d2-117">For automated, continuous tasks, you should use some type of service account that you provide with the necessary privileges to perform management tasks.</span></span> <span data-ttu-id="a42d2-118">Ve službě Azure AD to provedete tak, že registrace aplikace a ověřování do služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a42d2-118">In Azure AD, you can do this by registering an application and authenticating to Azure AD.</span></span> <span data-ttu-id="a42d2-119">To se provádí pomocí **ID aplikace** používající [udělení pověření klienta OAuth 2.0](../active-directory/develop/active-directory-authentication-scenarios.md#daemon-or-server-application-to-web-api).</span><span class="sxs-lookup"><span data-stu-id="a42d2-119">This is done by using an **Application ID** that uses the [OAuth 2.0 client credentials grant](../active-directory/develop/active-directory-authentication-scenarios.md#daemon-or-server-application-to-web-api).</span></span> <span data-ttu-id="a42d2-120">V takovém případě aplikace funguje jako samostatně, nikoli jako uživatel, k volání rozhraní Graph API.</span><span class="sxs-lookup"><span data-stu-id="a42d2-120">In this case, the application acts as itself, not as a user, to call the Graph API.</span></span>

<span data-ttu-id="a42d2-121">V tomto článku probereme, jak provádět případ automatizované použití.</span><span class="sxs-lookup"><span data-stu-id="a42d2-121">In this article, we'll discuss how to perform the automated-use case.</span></span> <span data-ttu-id="a42d2-122">K předvedení, jsme budete sestavení rozhraní .NET 4.5 `B2CGraphClient` který provede uživatele vytvořit, číst, aktualizovat a odstranit operace.</span><span class="sxs-lookup"><span data-stu-id="a42d2-122">To demonstrate, we'll build a .NET 4.5 `B2CGraphClient` that performs user create, read, update, and delete (CRUD) operations.</span></span> <span data-ttu-id="a42d2-123">Klient bude mít rozhraní příkazového řádku Windows (CLI), který umožňuje vyvolání různé metody.</span><span class="sxs-lookup"><span data-stu-id="a42d2-123">The client will have a Windows command-line interface (CLI) that allows you to invoke various methods.</span></span> <span data-ttu-id="a42d2-124">Kód je však zapsána do chovat neinteraktivní, automatizované způsobem.</span><span class="sxs-lookup"><span data-stu-id="a42d2-124">However, the code is written to behave in a noninteractive, automated fashion.</span></span>

## <a name="get-an-azure-ad-b2c-tenant"></a><span data-ttu-id="a42d2-125">Získání klienta Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="a42d2-125">Get an Azure AD B2C tenant</span></span>
<span data-ttu-id="a42d2-126">Předtím, než můžete vytvořit aplikace nebo uživatele, nebo vůbec komunikovat s Azure AD, budete potřebovat klienta Azure AD B2C a účet globálního správce v klientovi.</span><span class="sxs-lookup"><span data-stu-id="a42d2-126">Before you can create applications or users, or interact with Azure AD at all, you will need an Azure AD B2C tenant and a global administrator account in the tenant.</span></span> <span data-ttu-id="a42d2-127">Pokud nemáte klienta již, [Začínáme s Azure AD B2C](active-directory-b2c-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="a42d2-127">If you don't have a tenant already, [get started with Azure AD B2C](active-directory-b2c-get-started.md).</span></span>

## <a name="register-your-application-in-your-tenant"></a><span data-ttu-id="a42d2-128">Registrace vaší aplikace ve vašem klientovi</span><span class="sxs-lookup"><span data-stu-id="a42d2-128">Register your application in your tenant</span></span>
<span data-ttu-id="a42d2-129">Až budete mít klienta B2C, budete muset registrace vaší aplikace pomocí [portálu Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="a42d2-129">After you have a B2C tenant, you need to register your application via the [Azure Portal](https://portal.azure.com).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a42d2-130">Použít rozhraní Graph API pomocí svého klienta B2C, budete muset registraci vyhrazené aplikace pomocí Obecné *registrace aplikace* nabídky na portálu Azure **není** Azure AD B2C  *Aplikace* nabídky.</span><span class="sxs-lookup"><span data-stu-id="a42d2-130">To use the Graph API with your B2C tenant, you will need to register a dedicated application by using the generic *App Registrations* menu in the Azure Portal, **NOT** Azure AD B2C's *Applications* menu.</span></span> <span data-ttu-id="a42d2-131">Nelze znovu použít už existující B2C aplikace, které jste zaregistrovali v Azure AD B2C *aplikace* nabídky.</span><span class="sxs-lookup"><span data-stu-id="a42d2-131">You can't reuse the already-existing B2C applications that you registered in the Azure AD B2C's *Applications* menu.</span></span>

1. <span data-ttu-id="a42d2-132">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="a42d2-132">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="a42d2-133">Výběrem účtu v pravém horním rohu stránky zvolte vašeho klienta Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="a42d2-133">Choose your Azure AD B2C tenant by selecting your account in the top right corner of the page.</span></span>
3. <span data-ttu-id="a42d2-134">V levém navigačním podokně zvolte **více služeb**, klikněte na tlačítko **registrace aplikace**a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="a42d2-134">In the left-hand navigation pane, choose **More Services**, click **App Registrations**, and click **Add**.</span></span>
4. <span data-ttu-id="a42d2-135">Postupujte podle zobrazených výzev a vytvořte novou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a42d2-135">Follow the prompts and create a new application.</span></span> 
    1. <span data-ttu-id="a42d2-136">Vyberte **webová aplikace nebo rozhraní API** jako typ aplikace.</span><span class="sxs-lookup"><span data-stu-id="a42d2-136">Select **Web App / API** as the Application Type.</span></span>    
    2. <span data-ttu-id="a42d2-137">Zadejte **žádný identifikátor URI přesměrování** (např. https://B2CGraphAPI) jako není relevantní pro tento příklad.</span><span class="sxs-lookup"><span data-stu-id="a42d2-137">Provide **any redirect URI** (e.g. https://B2CGraphAPI) as it's not relevant for this example.</span></span>  
5. <span data-ttu-id="a42d2-138">Budou aplikace teď objeví v seznamu aplikací, klikněte na ho získat **ID aplikace** (také označované jako ID klienta).</span><span class="sxs-lookup"><span data-stu-id="a42d2-138">The application will now show up in the list of applications, click on it to obtain the **Application ID** (also known as Client ID).</span></span> <span data-ttu-id="a42d2-139">Zkopírujte jej, protože ho budete potřebovat v další části.</span><span class="sxs-lookup"><span data-stu-id="a42d2-139">Copy it as you'll need it in a later section.</span></span>
6. <span data-ttu-id="a42d2-140">V nabídce nastavení, klikněte na **klíče** a přidejte nový klíč (také označovaný jako sdílený tajný klíč klienta).</span><span class="sxs-lookup"><span data-stu-id="a42d2-140">In the Settings menu, click on **Keys** and add a new key (also known as client secret).</span></span> <span data-ttu-id="a42d2-141">Také zkopírujte jej pro použití v další části.</span><span class="sxs-lookup"><span data-stu-id="a42d2-141">Also copy it for use in a later section.</span></span>

## <a name="configure-create-read-and-update-permissions-for-your-application"></a><span data-ttu-id="a42d2-142">Konfigurace vytvářet, číst a aktualizovat oprávnění pro vaši aplikaci</span><span class="sxs-lookup"><span data-stu-id="a42d2-142">Configure create, read and update permissions for your application</span></span>
<span data-ttu-id="a42d2-143">Teď je potřeba nakonfigurovat vaše aplikace a získejte potřebná oprávnění k vytvářet, číst, aktualizovat a odstraňovat uživatele.</span><span class="sxs-lookup"><span data-stu-id="a42d2-143">Now you need to configure your application to get all the required permissions to create, read, update and delete users.</span></span>

1. <span data-ttu-id="a42d2-144">Pokračování v nabídce portálu Azure registrace aplikace, vyberte svou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a42d2-144">Continuing in the Azure portal's App Registrations menu, select your application.</span></span>
2. <span data-ttu-id="a42d2-145">V nabídce nastavení, klikněte na **požadovaná oprávnění**.</span><span class="sxs-lookup"><span data-stu-id="a42d2-145">In the Settings menu, click on **Required permissions**.</span></span>
3. <span data-ttu-id="a42d2-146">V nabídce požadovaná oprávnění, klikněte na **Windows Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="a42d2-146">In the Required permissions menu, click on **Windows Azure Active Directory**.</span></span>
4. <span data-ttu-id="a42d2-147">Povolit přístup z nabídky vyberte **pro čtení a zápis dat adresáře** oprávnění z **oprávnění aplikací** a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="a42d2-147">In the Enable Access  menu, select the **Read and write directory data** permission from **Application Permissions** and click **Save**.</span></span>
5. <span data-ttu-id="a42d2-148">Nakonec zpět v nabídce požadovaná oprávnění, klikněte na **udělit oprávnění** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a42d2-148">Finally, back in the Required permissions menu, click on the **Grant Permissions** button.</span></span>

<span data-ttu-id="a42d2-149">Teď máte aplikaci, která má oprávnění k vytváření, čtení a aktualizace uživatelů ze svého klienta B2C.</span><span class="sxs-lookup"><span data-stu-id="a42d2-149">You now have an application that has permission to create, read and update users from your B2C tenant.</span></span>

> [!NOTE]
> <span data-ttu-id="a42d2-150">Udělení oprávnění Zkontrolujte trvat několik minut na úplné zpracování.</span><span class="sxs-lookup"><span data-stu-id="a42d2-150">Granting permissions make take a few minutes to fully process.</span></span>
> 
> 

## <a name="configure-delete-permissions-for-your-application"></a><span data-ttu-id="a42d2-151">Konfigurace odstranění oprávnění pro vaši aplikaci</span><span class="sxs-lookup"><span data-stu-id="a42d2-151">Configure delete permissions for your application</span></span>
<span data-ttu-id="a42d2-152">V současné době *pro čtení a zápis dat adresáře* nemá oprávnění **není** zahrnují možnost udělat všechny odstranění například odstraňování uživatelů.</span><span class="sxs-lookup"><span data-stu-id="a42d2-152">Currently, the *Read and write directory data* permission does **NOT** include the ability to do any deletions such as deleting users.</span></span> <span data-ttu-id="a42d2-153">Pokud chcete poskytnout vaše aplikace umožňuje odstranit uživatele, je potřeba provést tyto další kroky, které se týkají prostředí PowerShell, jinak, můžete přeskočit k další části.</span><span class="sxs-lookup"><span data-stu-id="a42d2-153">If you want to give your application the ability to delete users, you'll need to do these extra steps that involve PowerShell, otherwise, you can skip to the next section.</span></span>

<span data-ttu-id="a42d2-154">První, pokud ještě nemáte nainstalováno, instalaci [modulu Azure AD PowerShell v1 (MSOnline)](https://docs.microsoft.com/powershell/azure/active-directory/install-msonlinev1?view=azureadps-1.0):</span><span class="sxs-lookup"><span data-stu-id="a42d2-154">First, if you don't already have it installed, install the [Azure AD PowerShell v1 module (MSOnline)](https://docs.microsoft.com/powershell/azure/active-directory/install-msonlinev1?view=azureadps-1.0):</span></span>

```powershell
Install-Module MSOnline
```

<span data-ttu-id="a42d2-155">Po instalaci modulu PowerShell se připojit ke klientovi Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="a42d2-155">After you install the PowerShell module connect to your Azure AD B2C tenant.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a42d2-156">Je nutné použít účet správce klienta B2C, který je **místní** klienta B2C.</span><span class="sxs-lookup"><span data-stu-id="a42d2-156">You need to use a B2C tenant administrator account that is **local** to the B2C tenant.</span></span> <span data-ttu-id="a42d2-157">Tyto účty vypadat takto: myusername@myb2ctenant.onmicrosoft.com.</span><span class="sxs-lookup"><span data-stu-id="a42d2-157">These accounts look like this: myusername@myb2ctenant.onmicrosoft.com.</span></span>

```powershell
Connect-MsolService
```

<span data-ttu-id="a42d2-158">Nyní použijeme **ID aplikace** v níže přiřadit role správce účtu uživatele, která umožní odstranit uživatele aplikace skriptu.</span><span class="sxs-lookup"><span data-stu-id="a42d2-158">Now we'll use the **Application ID** in the script below to assign the application the user account administrator role which will allow it to delete users.</span></span> <span data-ttu-id="a42d2-159">Tyto role mají dobře známé identifikátory, takže všechny musíte udělat není vstupní vaše **ID aplikace** v níže uvedeném skriptu.</span><span class="sxs-lookup"><span data-stu-id="a42d2-159">These roles have well-known identifiers, so all you need to do is input your **Application ID** in the script below.</span></span>

```powershell
$applicationId = "<YOUR_APPLICATION_ID>"
$sp = Get-MsolServicePrincipal -AppPrincipalId $applicationId
Add-MsolRoleMember -RoleObjectId fe930be7-5e62-47db-91af-98c3a49a38b1 -RoleMemberObjectId $sp.ObjectId -RoleMemberType servicePrincipal
```

<span data-ttu-id="a42d2-160">Aplikaci teď má také oprávnění k odstranění uživatelů z vašeho klienta B2C.</span><span class="sxs-lookup"><span data-stu-id="a42d2-160">Your application now also has permissions to delete users from your B2C tenant.</span></span>

## <a name="download-configure-and-build-the-sample-code"></a><span data-ttu-id="a42d2-161">Stáhněte si, konfigurace a sestavte ukázkový kód</span><span class="sxs-lookup"><span data-stu-id="a42d2-161">Download, configure, and build the sample code</span></span>
<span data-ttu-id="a42d2-162">Nejprve stáhnout ukázkový kód a získat běh aplikace.</span><span class="sxs-lookup"><span data-stu-id="a42d2-162">First, download the sample code and get it running.</span></span> <span data-ttu-id="a42d2-163">Potom jsme bude trvat bližší pohled na ho.</span><span class="sxs-lookup"><span data-stu-id="a42d2-163">Then we will take a closer look at it.</span></span>  <span data-ttu-id="a42d2-164">Můžete [stáhnout ukázkový kód v souboru ZIP](https://github.com/AzureADQuickStarts/B2C-GraphAPI-DotNet/archive/master.zip).</span><span class="sxs-lookup"><span data-stu-id="a42d2-164">You can [download the sample code as a .zip file](https://github.com/AzureADQuickStarts/B2C-GraphAPI-DotNet/archive/master.zip).</span></span> <span data-ttu-id="a42d2-165">Můžete ho také klonovat do adresáře podle vašeho výběru:</span><span class="sxs-lookup"><span data-stu-id="a42d2-165">You can also clone it into a directory of your choice:</span></span>

```
git clone https://github.com/AzureADQuickStarts/B2C-GraphAPI-DotNet.git
```

<span data-ttu-id="a42d2-166">Otevřete `B2CGraphClient\B2CGraphClient.sln` řešení sady Visual Studio v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a42d2-166">Open the `B2CGraphClient\B2CGraphClient.sln` Visual Studio solution in Visual Studio.</span></span> <span data-ttu-id="a42d2-167">V `B2CGraphClient` projektu, otevřete soubor `App.config`.</span><span class="sxs-lookup"><span data-stu-id="a42d2-167">In the `B2CGraphClient` project, open the file `App.config`.</span></span> <span data-ttu-id="a42d2-168">Nastavení tři aplikace nahraďte vlastními hodnotami:</span><span class="sxs-lookup"><span data-stu-id="a42d2-168">Replace the three app settings with your own values:</span></span>

```
<appSettings>
    <add key="b2c:Tenant" value="{Your Tenant Name}" />
    <add key="b2c:ClientId" value="{The ApplicationID from above}" />
    <add key="b2c:ClientSecret" value="{The Key from above}" />
</appSettings>
```

[!INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

<span data-ttu-id="a42d2-169">Pak klikněte pravým tlačítkem na `B2CGraphClient` řešení a znovu sestavit ukázku.</span><span class="sxs-lookup"><span data-stu-id="a42d2-169">Next, right-click on the `B2CGraphClient` solution and rebuild the sample.</span></span> <span data-ttu-id="a42d2-170">Pokud jste úspěšné, teď byste měli mít `B2C.exe` spustitelný soubor umístěný ve `B2CGraphClient\bin\Debug`.</span><span class="sxs-lookup"><span data-stu-id="a42d2-170">If you are successful, you should now have a `B2C.exe` executable file located in `B2CGraphClient\bin\Debug`.</span></span>

## <a name="build-user-crud-operations-by-using-the-graph-api"></a><span data-ttu-id="a42d2-171">Vytvoření operace CRUD uživatele pomocí rozhraní Graph API</span><span class="sxs-lookup"><span data-stu-id="a42d2-171">Build user CRUD operations by using the Graph API</span></span>
<span data-ttu-id="a42d2-172">Chcete-li použít B2CGraphClient, otevřete `cmd` Windows příkazového řádku a přejděte do adresáře `Debug` adresáře.</span><span class="sxs-lookup"><span data-stu-id="a42d2-172">To use the B2CGraphClient, open a `cmd` Windows command prompt and change your directory to the `Debug` directory.</span></span> <span data-ttu-id="a42d2-173">Spusťte `B2C Help` příkaz.</span><span class="sxs-lookup"><span data-stu-id="a42d2-173">Then run the `B2C Help` command.</span></span>

```
> cd B2CGraphClient\bin\Debug
> B2C Help
```

<span data-ttu-id="a42d2-174">Tato akce zobrazí stručný popis každého příkazu.</span><span class="sxs-lookup"><span data-stu-id="a42d2-174">This will display a brief description of each command.</span></span> <span data-ttu-id="a42d2-175">Pokaždé, když vyvolat jeden z těchto příkazů `B2CGraphClient` odešle požadavek do Azure AD Graph API.</span><span class="sxs-lookup"><span data-stu-id="a42d2-175">Each time you invoke one of these commands, `B2CGraphClient` makes a request to the Azure AD Graph API.</span></span>

### <a name="get-an-access-token"></a><span data-ttu-id="a42d2-176">Získání přístupového tokenu</span><span class="sxs-lookup"><span data-stu-id="a42d2-176">Get an access token</span></span>
<span data-ttu-id="a42d2-177">Každá žádost o rozhraní Graph API vyžaduje token přístupu pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="a42d2-177">Any request to the Graph API requires an access token for authentication.</span></span> <span data-ttu-id="a42d2-178">`B2CGraphClient` pomocí open source Active Directory Authentication Library (ADAL) k získání přístupových tokenů.</span><span class="sxs-lookup"><span data-stu-id="a42d2-178">`B2CGraphClient` uses the open-source Active Directory Authentication Library (ADAL) to help acquire access tokens.</span></span> <span data-ttu-id="a42d2-179">ADAL usnadňuje získávání tokenu poskytuje jednoduché rozhraní API a postará o některé důležité podrobnosti, jako je například ukládání do mezipaměti přístupových tokenů.</span><span class="sxs-lookup"><span data-stu-id="a42d2-179">ADAL makes token acquisition easier by providing a simple API and taking care of some important details, such as caching access tokens.</span></span> <span data-ttu-id="a42d2-180">Nemáte vám pomůže získat tokeny, když ADAL.</span><span class="sxs-lookup"><span data-stu-id="a42d2-180">You don't have to use ADAL to get tokens, though.</span></span> <span data-ttu-id="a42d2-181">Můžete také získat tokeny tím, že vytvoří požadavky HTTP.</span><span class="sxs-lookup"><span data-stu-id="a42d2-181">You can also get tokens by crafting HTTP requests.</span></span>

> [!NOTE]
> <span data-ttu-id="a42d2-182">Tento příklad používá ADAL v2 komunikovat s rozhraní Graph API.</span><span class="sxs-lookup"><span data-stu-id="a42d2-182">This code sample uses ADAL v2 in order to communicate with the Graph API.</span></span>  <span data-ttu-id="a42d2-183">Pokud chcete získat přístupové tokeny, které se dají použít s Azure AD Graph API je nutné použít ADAL verze 2 nebo 3.</span><span class="sxs-lookup"><span data-stu-id="a42d2-183">You must use ADAL v2 or v3 in order to get access tokens which can be used with the Azure AD Graph API.</span></span>
> 
> 

<span data-ttu-id="a42d2-184">Když `B2CGraphClient` spustí, vytvoří instanci `B2CGraphClient` třídy.</span><span class="sxs-lookup"><span data-stu-id="a42d2-184">When `B2CGraphClient` runs, it creates an instance of the `B2CGraphClient` class.</span></span> <span data-ttu-id="a42d2-185">V konstruktoru pro tuto třídu nastaví generování uživatelského rozhraní ověřování pomocí knihovny ADAL:</span><span class="sxs-lookup"><span data-stu-id="a42d2-185">The constructor for this class sets up an ADAL authentication scaffolding:</span></span>

```csharp
public B2CGraphClient(string clientId, string clientSecret, string tenant)
{
    // The client_id, client_secret, and tenant are provided in Program.cs, which pulls the values from App.config
    this.clientId = clientId;
    this.clientSecret = clientSecret;
    this.tenant = tenant;

    // The AuthenticationContext is ADAL's primary class, in which you indicate the tenant to use.
    this.authContext = new AuthenticationContext("https://login.microsoftonline.com/" + tenant);

    // The ClientCredential is where you pass in your client_id and client_secret, which are
    // provided to Azure AD in order to receive an access_token by using the app's identity.
    this.credential = new ClientCredential(clientId, clientSecret);
}
```

<span data-ttu-id="a42d2-186">Použijeme `B2C Get-User` příkaz jako příklad.</span><span class="sxs-lookup"><span data-stu-id="a42d2-186">We'll use the `B2C Get-User` command as an example.</span></span> <span data-ttu-id="a42d2-187">Když `B2C Get-User` je volána bez jakékoli další vstupy volání rozhraní příkazového řádku `B2CGraphClient.GetAllUsers(...)` metoda.</span><span class="sxs-lookup"><span data-stu-id="a42d2-187">When `B2C Get-User` is invoked without any additional inputs, the CLI calls the `B2CGraphClient.GetAllUsers(...)` method.</span></span> <span data-ttu-id="a42d2-188">Tato metoda volá `B2CGraphClient.SendGraphGetRequest(...)`, který odesílá požadavek HTTP GET pro rozhraní Graph API.</span><span class="sxs-lookup"><span data-stu-id="a42d2-188">This method calls `B2CGraphClient.SendGraphGetRequest(...)`, which submits an HTTP GET request to the Graph API.</span></span> <span data-ttu-id="a42d2-189">Před `B2CGraphClient.SendGraphGetRequest(...)` odešle požadavek GET, nejdřív získá přístup tokenu pomocí ADAL:</span><span class="sxs-lookup"><span data-stu-id="a42d2-189">Before `B2CGraphClient.SendGraphGetRequest(...)` sends the GET request, it first gets an access token by using ADAL:</span></span>

```csharp
public async Task<string> SendGraphGetRequest(string api, string query)
{
    // First, use ADAL to acquire a token by using the app's identity (the credential)
    // The first parameter is the resource we want an access_token for; in this case, the Graph API.
    AuthenticationResult result = authContext.AcquireToken("https://graph.windows.net", credential);

    ...

```

<span data-ttu-id="a42d2-190">Můžete získat přístup tokenu pro rozhraní Graph API voláním knihovnu ADAL `AuthenticationContext.AcquireToken(...)` metoda.</span><span class="sxs-lookup"><span data-stu-id="a42d2-190">You can get an access token for the Graph API by calling the ADAL `AuthenticationContext.AcquireToken(...)` method.</span></span> <span data-ttu-id="a42d2-191">Vrátí ADAL `access_token` představující identitu aplikace.</span><span class="sxs-lookup"><span data-stu-id="a42d2-191">ADAL then returns an `access_token` that represents the application's identity.</span></span>

### <a name="read-users"></a><span data-ttu-id="a42d2-192">Uživatelé pro čtení</span><span class="sxs-lookup"><span data-stu-id="a42d2-192">Read users</span></span>
<span data-ttu-id="a42d2-193">Pokud chcete získat seznam uživatelů, nebo získat určitého uživatele z rozhraní Graph API, můžete odeslat HTTP `GET` žádost o `/users` koncový bod.</span><span class="sxs-lookup"><span data-stu-id="a42d2-193">When you want to get a list of users or get a particular user from the Graph API, you can send an HTTP `GET` request to the `/users` endpoint.</span></span> <span data-ttu-id="a42d2-194">Žádost o pro všechny uživatele v klienta vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="a42d2-194">A request for all of the users in a tenant looks like this:</span></span>

```
GET https://graph.windows.net/contosob2c.onmicrosoft.com/users?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
```

<span data-ttu-id="a42d2-195">Pokud chcete zobrazit tento požadavek, spusťte příkaz:</span><span class="sxs-lookup"><span data-stu-id="a42d2-195">To see this request, run:</span></span>

 ```
 > B2C Get-User
 ```

<span data-ttu-id="a42d2-196">Všimněte si, dvě důležité věci:</span><span class="sxs-lookup"><span data-stu-id="a42d2-196">There are two important things to note:</span></span>

* <span data-ttu-id="a42d2-197">Přístupový token získaných prostřednictvím ADAL je přidán do `Authorization` záhlaví s použitím `Bearer` schéma.</span><span class="sxs-lookup"><span data-stu-id="a42d2-197">The access token acquired via ADAL is added to the `Authorization` header by using the `Bearer` scheme.</span></span>
* <span data-ttu-id="a42d2-198">U klientů B2C, musíte použít parametr dotazu `api-version=1.6`.</span><span class="sxs-lookup"><span data-stu-id="a42d2-198">For B2C tenants, you must use the query parameter `api-version=1.6`.</span></span>

<span data-ttu-id="a42d2-199">Obě tyto podrobnosti jsou zpracovávány v `B2CGraphClient.SendGraphGetRequest(...)` metoda:</span><span class="sxs-lookup"><span data-stu-id="a42d2-199">Both of these details are handled in the `B2CGraphClient.SendGraphGetRequest(...)` method:</span></span>

```csharp
public async Task<string> SendGraphGetRequest(string api, string query)
{
    ...

    // For B2C user management, be sure to use the 1.6 Graph API version.
    HttpClient http = new HttpClient();
    string url = "https://graph.windows.net/" + tenant + api + "?" + "api-version=1.6";
    if (!string.IsNullOrEmpty(query))
    {
        url += "&" + query;
    }

    // Append the access token for the Graph API to the Authorization header of the request by using the Bearer scheme.
    HttpRequestMessage request = new HttpRequestMessage(HttpMethod.Get, url);
    request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);
    HttpResponseMessage response = await http.SendAsync(request);

    ...
```

### <a name="create-consumer-user-accounts"></a><span data-ttu-id="a42d2-200">Vytvoření příjemce uživatelské účty</span><span class="sxs-lookup"><span data-stu-id="a42d2-200">Create consumer user accounts</span></span>
<span data-ttu-id="a42d2-201">Při vytváření uživatelských účtů v svého klienta B2C, můžete odeslat HTTP `POST` žádost o `/users` koncový bod:</span><span class="sxs-lookup"><span data-stu-id="a42d2-201">When you create user accounts in your B2C tenant, you can send an HTTP `POST` request to the `/users` endpoint:</span></span>

```
POST https://graph.windows.net/contosob2c.onmicrosoft.com/users?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
Content-Type: application/json
Content-Length: 338

{
    // All of these properties are required to create consumer users.

    "accountEnabled": true,
    "signInNames": [                            // controls which identifier the user uses to sign in to the account
        {
            "type": "emailAddress",             // can be 'emailAddress' or 'userName'
            "value": "joeconsumer@gmail.com"
        }
    ],
    "creationType": "LocalAccount",            // always set to 'LocalAccount'
    "displayName": "Joe Consumer",                // a value that can be used for displaying to the end user
    "mailNickname": "joec",                        // an email alias for the user
    "passwordProfile": {
        "password": "P@ssword!",
        "forceChangePasswordNextLogin": false   // always set to false
    },
    "passwordPolicies": "DisablePasswordExpiration"
}
```

<span data-ttu-id="a42d2-202">Většina těchto vlastností v této žádosti o nutné vytvořit spotřebitelské uživatele.</span><span class="sxs-lookup"><span data-stu-id="a42d2-202">Most of these properties in this request are required to create consumer users.</span></span> <span data-ttu-id="a42d2-203">Chcete-li získat další informace, klikněte na tlačítko [zde](https://msdn.microsoft.com/library/azure/ad/graph/api/users-operations#CreateLocalAccountUser).</span><span class="sxs-lookup"><span data-stu-id="a42d2-203">To learn more, click [here](https://msdn.microsoft.com/library/azure/ad/graph/api/users-operations#CreateLocalAccountUser).</span></span> <span data-ttu-id="a42d2-204">Všimněte si, že `//` komentáře byly zahrnuty pro obrázek.</span><span class="sxs-lookup"><span data-stu-id="a42d2-204">Note that the `//` comments have been included for illustration.</span></span> <span data-ttu-id="a42d2-205">V skutečné požadavku je nezahrnujte.</span><span class="sxs-lookup"><span data-stu-id="a42d2-205">Do not include them in an actual request.</span></span>

<span data-ttu-id="a42d2-206">Pokud chcete zobrazit žádost, spusťte jeden z následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="a42d2-206">To see the request, run one of the following commands:</span></span>

```
> B2C Create-User ..\..\..\usertemplate-email.json
> B2C Create-User ..\..\..\usertemplate-username.json
```

<span data-ttu-id="a42d2-207">`Create-User` Příkaz má soubor .json jako vstupní parametr.</span><span class="sxs-lookup"><span data-stu-id="a42d2-207">The `Create-User` command takes a .json file as an input parameter.</span></span> <span data-ttu-id="a42d2-208">Tato položka obsahuje reprezentaci JSON objektu uživatele.</span><span class="sxs-lookup"><span data-stu-id="a42d2-208">This contains a JSON representation of a user object.</span></span> <span data-ttu-id="a42d2-209">Existují dva ukázkové soubory .json v ukázkovém kódu: `usertemplate-email.json` a `usertemplate-username.json`.</span><span class="sxs-lookup"><span data-stu-id="a42d2-209">There are two sample .json files in the sample code: `usertemplate-email.json` and `usertemplate-username.json`.</span></span> <span data-ttu-id="a42d2-210">Můžete upravit tyto soubory tak, aby vyhovovala vašim potřebám.</span><span class="sxs-lookup"><span data-stu-id="a42d2-210">You can modify these files to suit your needs.</span></span> <span data-ttu-id="a42d2-211">Kromě výše uvedených povinná pole jsou zahrnuty několik volitelná pole, které můžete použít v těchto souborech.</span><span class="sxs-lookup"><span data-stu-id="a42d2-211">In addition to the required fields above, several optional fields that you can use are included in these files.</span></span> <span data-ttu-id="a42d2-212">Podrobnosti o volitelná pole lze nalézt v [odkaz na Azure AD Graph API entity](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity).</span><span class="sxs-lookup"><span data-stu-id="a42d2-212">Details on the optional fields can be found in the [Azure AD Graph API entity reference](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity).</span></span>

<span data-ttu-id="a42d2-213">Můžete vidět, jak je požadavek POST vytvořený v `B2CGraphClient.SendGraphPostRequest(...)`.</span><span class="sxs-lookup"><span data-stu-id="a42d2-213">You can see how the POST request is constructed in `B2CGraphClient.SendGraphPostRequest(...)`.</span></span>

* <span data-ttu-id="a42d2-214">Přiloží přístupového tokenu k `Authorization` hlavičky žádosti.</span><span class="sxs-lookup"><span data-stu-id="a42d2-214">It attaches an access token to the `Authorization` header of the request.</span></span>
* <span data-ttu-id="a42d2-215">Nastaví `api-version=1.6`.</span><span class="sxs-lookup"><span data-stu-id="a42d2-215">It sets `api-version=1.6`.</span></span>
* <span data-ttu-id="a42d2-216">Obsahuje uživatele objekt JSON v textu požadavku.</span><span class="sxs-lookup"><span data-stu-id="a42d2-216">It includes the JSON user object in the body of the request.</span></span>

> [!NOTE]
> <span data-ttu-id="a42d2-217">Pokud účty, které chcete provést migraci z existující úložiště uživatele má nižší síly hesla, než [silné heslo sílu vynucováno Azure AD B2C](https://msdn.microsoft.com/library/azure/jj943764.aspx), můžete zakázat pomocí požadavek na silné heslo `DisableStrongPassword` Hodnota v `passwordPolicies` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="a42d2-217">If the accounts that you want to migrate from an existing user store has lower password strength than the [strong password strength enforced by Azure AD B2C](https://msdn.microsoft.com/library/azure/jj943764.aspx), you can disable the strong password requirement using the `DisableStrongPassword` value in the `passwordPolicies` property.</span></span> <span data-ttu-id="a42d2-218">Například můžete upravit požadavek na vytvoření uživatele výše uvedeného následujícím způsobem: `"passwordPolicies": "DisablePasswordExpiration, DisableStrongPassword"`.</span><span class="sxs-lookup"><span data-stu-id="a42d2-218">For instance, you can modify the create user request provided above as follows: `"passwordPolicies": "DisablePasswordExpiration, DisableStrongPassword"`.</span></span>
> 
> 

### <a name="update-consumer-user-accounts"></a><span data-ttu-id="a42d2-219">Aktualizovat příjemce uživatelské účty</span><span class="sxs-lookup"><span data-stu-id="a42d2-219">Update consumer user accounts</span></span>
<span data-ttu-id="a42d2-220">Při aktualizaci uživatelských objektů proces je podobný je ta, kterou použijete k vytvoření uživatelské objekty.</span><span class="sxs-lookup"><span data-stu-id="a42d2-220">When you update user objects, the process is similar to the one you use to create user objects.</span></span> <span data-ttu-id="a42d2-221">Tento proces používá HTTP, ale `PATCH` metoda:</span><span class="sxs-lookup"><span data-stu-id="a42d2-221">But this process uses the HTTP `PATCH` method:</span></span>

```
PATCH https://graph.windows.net/contosob2c.onmicrosoft.com/users/<user-object-id>?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
Content-Type: application/json
Content-Length: 37

{
    "displayName": "Joe Consumer",                // this request updates only the user's displayName
}
```

<span data-ttu-id="a42d2-222">Zkuste provést aktualizaci uživatele tím, že aktualizuje vaše soubory JSON se nová data.</span><span class="sxs-lookup"><span data-stu-id="a42d2-222">Try to update a user by updating your JSON files with new data.</span></span> <span data-ttu-id="a42d2-223">Pak můžete použít `B2CGraphClient` spustit jeden z těchto příkazů:</span><span class="sxs-lookup"><span data-stu-id="a42d2-223">You can then use `B2CGraphClient` to run one of these commands:</span></span>

```
> B2C Update-User <user-object-id> ..\..\..\usertemplate-email.json
> B2C Update-User <user-object-id> ..\..\..\usertemplate-username.json
```

<span data-ttu-id="a42d2-224">Zkontrolujte `B2CGraphClient.SendGraphPatchRequest(...)` metoda podrobnosti o tom, jak odeslat tuto žádost.</span><span class="sxs-lookup"><span data-stu-id="a42d2-224">Inspect the `B2CGraphClient.SendGraphPatchRequest(...)` method for details on how to send this request.</span></span>

### <a name="search-users"></a><span data-ttu-id="a42d2-225">Vyhledávat uživatele</span><span class="sxs-lookup"><span data-stu-id="a42d2-225">Search users</span></span>
<span data-ttu-id="a42d2-226">Můžete hledat uživatele v svého klienta B2C v několika způsoby.</span><span class="sxs-lookup"><span data-stu-id="a42d2-226">You can search for users in your B2C tenant in a couple of ways.</span></span> <span data-ttu-id="a42d2-227">Jednoho uživatele pomocí objektu ID nebo dva pomocí uživatele přihlašovací identifikátor (tj, `signInNames` vlastnost).</span><span class="sxs-lookup"><span data-stu-id="a42d2-227">One, using the user's object ID or two, using the user's sign-in identifer (i.e., the `signInNames` property).</span></span>

<span data-ttu-id="a42d2-228">Spusťte jeden z následujících příkazů k vyhledání konkrétního uživatele:</span><span class="sxs-lookup"><span data-stu-id="a42d2-228">Run one of the following commands to search for a specific user:</span></span>

```
> B2C Get-User <user-object-id>
> B2C Get-User <filter-query-expression>
```

<span data-ttu-id="a42d2-229">Tady je několik příkladů:</span><span class="sxs-lookup"><span data-stu-id="a42d2-229">Here are a couple of examples:</span></span>

```
> B2C Get-User 2bcf1067-90b6-4253-9991-7f16449c2d91
> B2C Get-User $filter=signInNames/any(x:x/value%20eq%20%27joeconsumer@gmail.com%27)
```

### <a name="delete-users"></a><span data-ttu-id="a42d2-230">Odstranit uživatele</span><span class="sxs-lookup"><span data-stu-id="a42d2-230">Delete users</span></span>
<span data-ttu-id="a42d2-231">Proces odstranění uživatele je jednoduchá.</span><span class="sxs-lookup"><span data-stu-id="a42d2-231">The process for deleting a user is straightforward.</span></span> <span data-ttu-id="a42d2-232">Pomocí HTTP `DELETE` metoda a konstrukce adresu URL s správné ID objektu:</span><span class="sxs-lookup"><span data-stu-id="a42d2-232">Use the HTTP `DELETE` method and construct the URL with the correct object ID:</span></span>

```
DELETE https://graph.windows.net/contosob2c.onmicrosoft.com/users/<user-object-id>?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
```

<span data-ttu-id="a42d2-233">Pokud chcete zobrazit příklad, zadejte tento příkaz a zobrazit odstranit požadavek, který je vytištěno do konzoly:</span><span class="sxs-lookup"><span data-stu-id="a42d2-233">To see an example, enter this command and view the delete request that is printed to the console:</span></span>

```
> B2C Delete-User <object-id-of-user>
```

<span data-ttu-id="a42d2-234">Zkontrolujte `B2CGraphClient.SendGraphDeleteRequest(...)` metoda podrobnosti o tom, jak odeslat tuto žádost.</span><span class="sxs-lookup"><span data-stu-id="a42d2-234">Inspect the `B2CGraphClient.SendGraphDeleteRequest(...)` method for details on how to send this request.</span></span>

<span data-ttu-id="a42d2-235">Můžete provádět mnoho dalších akcí s Azure AD Graph API kromě správy uživatelů.</span><span class="sxs-lookup"><span data-stu-id="a42d2-235">You can perform many other actions with the Azure AD Graph API in addition to user management.</span></span> <span data-ttu-id="a42d2-236">[Referenční dokumentace rozhraní API Azure AD Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) poskytuje podrobnosti pro každou akci, společně s požadavky na ukázky.</span><span class="sxs-lookup"><span data-stu-id="a42d2-236">The [Azure AD Graph API reference](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) provides details on each action, along with sample requests.</span></span>

## <a name="use-custom-attributes"></a><span data-ttu-id="a42d2-237">Použití vlastních atributů</span><span class="sxs-lookup"><span data-stu-id="a42d2-237">Use custom attributes</span></span>
<span data-ttu-id="a42d2-238">Většina uživatelských aplikací je nutné uložit nějaký typ informace vlastního uživatelského profilu.</span><span class="sxs-lookup"><span data-stu-id="a42d2-238">Most consumer applications need to store some type of custom user profile information.</span></span> <span data-ttu-id="a42d2-239">Jedním ze způsobů, můžete k tomu je k definování vlastní atribut v svého klienta B2C.</span><span class="sxs-lookup"><span data-stu-id="a42d2-239">One way you can do this is to define a custom attribute in your B2C tenant.</span></span> <span data-ttu-id="a42d2-240">Poté můžete ke tento atribut stejným způsobem jako považovat všechny ostatní vlastnosti v objektu user.</span><span class="sxs-lookup"><span data-stu-id="a42d2-240">You can then treat that attribute the same way you treat any other property on a user object.</span></span> <span data-ttu-id="a42d2-241">Můžete aktualizovat atribut, odstranit atribut, dotaz v atributu, odesílat atribut jako deklaraci v tokeny přihlášení a další.</span><span class="sxs-lookup"><span data-stu-id="a42d2-241">You can update the attribute, delete the attribute, query by the attribute, send the attribute as a claim in sign-in tokens, and more.</span></span>

<span data-ttu-id="a42d2-242">K definování vlastní atribut v svého klienta B2C, najdete v článku [odkaz vlastní atribut B2C](active-directory-b2c-reference-custom-attr.md).</span><span class="sxs-lookup"><span data-stu-id="a42d2-242">To define a custom attribute in your B2C tenant, see the [B2C custom attribute reference](active-directory-b2c-reference-custom-attr.md).</span></span>

<span data-ttu-id="a42d2-243">Můžete zobrazit vlastní atributy definované ve vašem klientovi B2C pomocí `B2CGraphClient`:</span><span class="sxs-lookup"><span data-stu-id="a42d2-243">You can view the custom attributes defined in your B2C tenant by using `B2CGraphClient`:</span></span>

```
> B2C Get-B2C-Application
> B2C Get-Extension-Attribute <object-id-in-the-output-of-the-above-command>
```

<span data-ttu-id="a42d2-244">Výstup z těchto funkcí zobrazí podrobnosti o jednotlivých vlastních atributů, například:</span><span class="sxs-lookup"><span data-stu-id="a42d2-244">The output of these functions reveals the details of each custom attribute, such as:</span></span>

```JSON
{
      "odata.type": "Microsoft.DirectoryServices.ExtensionProperty",
      "objectType": "ExtensionProperty",
      "objectId": "cec6391b-204d-42fe-8f7c-89c2b1964fca",
      "deletionTimestamp": null,
      "appDisplayName": "",
      "name": "extension_55dc0861f9a44eb999e0a8a872204adb_Jersey_Number",
      "dataType": "Integer",
      "isSyncedFromOnPremises": false,
      "targetObjects": [
        "User"
      ]
}
```

<span data-ttu-id="a42d2-245">Úplný název, můžete použít jako `extension_55dc0861f9a44eb999e0a8a872204adb_Jersey_Number`, jako vlastnost na uživatelské objekty.</span><span class="sxs-lookup"><span data-stu-id="a42d2-245">You can use the full name, such as `extension_55dc0861f9a44eb999e0a8a872204adb_Jersey_Number`, as a property on your user objects.</span></span>  <span data-ttu-id="a42d2-246">Aktualizujte si soubor .json nové vlastnosti a hodnotu pro vlastnost a potom spusťte:</span><span class="sxs-lookup"><span data-stu-id="a42d2-246">Update your .json file with the new property and a value for the property, and then run:</span></span>

```
> B2C Update-User <object-id-of-user> <path-to-json-file>
```

<span data-ttu-id="a42d2-247">Pomocí `B2CGraphClient`, máte aplikaci služby, která můžete spravovat vaše uživatele klienta B2C prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="a42d2-247">By using `B2CGraphClient`, you have a service application that can manage your B2C tenant users programmatically.</span></span> <span data-ttu-id="a42d2-248">`B2CGraphClient` vlastní identity aplikace se používá k ověření Azure AD Graph API.</span><span class="sxs-lookup"><span data-stu-id="a42d2-248">`B2CGraphClient` uses its own application identity to authenticate to the Azure AD Graph API.</span></span> <span data-ttu-id="a42d2-249">Je také získá tokeny pomocí tajný klíč klienta.</span><span class="sxs-lookup"><span data-stu-id="a42d2-249">It also acquires tokens by using a client secret.</span></span> <span data-ttu-id="a42d2-250">Protože tato funkce se začlenit do vaší aplikace, mějte na paměti několik klíčových bodů pro B2C aplikace:</span><span class="sxs-lookup"><span data-stu-id="a42d2-250">As you incorporate this functionality into your application, remember a few key points for B2C apps:</span></span>

* <span data-ttu-id="a42d2-251">Je třeba udělit příslušná oprávnění v klientovi aplikace.</span><span class="sxs-lookup"><span data-stu-id="a42d2-251">You need to grant the application the proper permissions in the tenant.</span></span>
* <span data-ttu-id="a42d2-252">Nyní potřebujete získat přístupové tokeny pomocí knihovny ADAL (ne MSAL).</span><span class="sxs-lookup"><span data-stu-id="a42d2-252">For now, you need to use ADAL (not MSAL) to get access tokens.</span></span> <span data-ttu-id="a42d2-253">(Můžete také odeslat zprávy protokolu přímo, bez použití knihovny.)</span><span class="sxs-lookup"><span data-stu-id="a42d2-253">(You can also send protocol messages directly, without using a library.)</span></span>
* <span data-ttu-id="a42d2-254">Při volání rozhraní Graph API, použijte `api-version=1.6`.</span><span class="sxs-lookup"><span data-stu-id="a42d2-254">When you call the Graph API, use `api-version=1.6`.</span></span>
* <span data-ttu-id="a42d2-255">Při vytváření a aktualizovat spotřebitelské uživatele, jsou povinné, několik vlastností, jak je popsáno výše.</span><span class="sxs-lookup"><span data-stu-id="a42d2-255">When you create and update consumer users, a few properties are required, as described above.</span></span>

<span data-ttu-id="a42d2-256">Pokud máte jakékoli dotazy nebo požadavků pro akce, že kterou chcete provést pomocí rozhraní Graph API na svého klienta B2C, poznámky v tomto článku nebo v úložišti GitHub ukázkový kód soubor problém.</span><span class="sxs-lookup"><span data-stu-id="a42d2-256">If you have any questions or requests for actions you would like to perform by using the Graph API on your B2C tenant, leave a comment on this article or file an issue in the GitHub code sample repository.</span></span>

