---
title: "Služby Azure Active Directory B2C: Použít rozhraní Graph API | Microsoft Docs"
description: "Postup pro volání rozhraní Graph API pro klienta B2C identity aplikací pro automatizaci procesu."
services: active-directory-b2c
documentationcenter: .net
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: f9904516-d9f7-43b1-ae4f-e4d9eb1c67a0
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/07/2017
ms.author: parakhj
ms.openlocfilehash: c838fcad21875c4f813159ad78d4c87129a40a86
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="azure-ad-b2c-use-the-graph-api"></a><span data-ttu-id="55f13-103">Azure AD B2C: Rozhraní Graph API pomocí</span><span class="sxs-lookup"><span data-stu-id="55f13-103">Azure AD B2C: Use the Graph API</span></span>
<span data-ttu-id="55f13-104">Azure Active Directory (Azure AD) B2C klientů jsou obvykle velké.</span><span class="sxs-lookup"><span data-stu-id="55f13-104">Azure Active Directory (Azure AD) B2C tenants tend to be very large.</span></span> <span data-ttu-id="55f13-105">To znamená, že mnoho běžné úlohy správy klienta musí být provedeny prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="55f13-105">This means that many common tenant management tasks need to be performed programmatically.</span></span> <span data-ttu-id="55f13-106">Primární příkladem je Správa uživatelů.</span><span class="sxs-lookup"><span data-stu-id="55f13-106">A primary example is user management.</span></span> <span data-ttu-id="55f13-107">Možná budete muset migrovat stávající úložiště uživatele do klienta B2C.</span><span class="sxs-lookup"><span data-stu-id="55f13-107">You might need to migrate an existing user store to a B2C tenant.</span></span> <span data-ttu-id="55f13-108">Můžete pro hostování registrace uživatele na vlastní stránky a vytvářet uživatelské účty ve svém adresáři Azure AD B2C na pozadí.</span><span class="sxs-lookup"><span data-stu-id="55f13-108">You may want to host user registration on your own page and create user accounts in your Azure AD B2C directory behind the scenes.</span></span> <span data-ttu-id="55f13-109">Tyto typy úloh vyžadovat schopnost vytvářet, číst, aktualizovat a odstraňovat uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="55f13-109">These types of tasks require the ability to create, read, update, and delete user accounts.</span></span> <span data-ttu-id="55f13-110">Můžete provést tyto úlohy pomocí rozhraní Azure AD Graph API.</span><span class="sxs-lookup"><span data-stu-id="55f13-110">You can do these tasks by using the Azure AD Graph API.</span></span>

<span data-ttu-id="55f13-111">U klientů B2C existují dva primární režimy komunikaci pomocí rozhraní Graph API.</span><span class="sxs-lookup"><span data-stu-id="55f13-111">For B2C tenants, there are two primary modes of communicating with the Graph API.</span></span>

* <span data-ttu-id="55f13-112">Pro interaktivní, jedno spuštění úlohy by měla fungovat jako účet správce v klientovi B2C při provádění úlohy.</span><span class="sxs-lookup"><span data-stu-id="55f13-112">For interactive, run-once tasks, you should act as an administrator account in the B2C tenant when you perform the tasks.</span></span> <span data-ttu-id="55f13-113">Tento režim vyžaduje správce a přihlaste se pomocí přihlašovacích údajů, před kterou správce může provádět volání rozhraní Graph API.</span><span class="sxs-lookup"><span data-stu-id="55f13-113">This mode requires an administrator to sign in with credentials before that admin can perform any calls to the Graph API.</span></span>
* <span data-ttu-id="55f13-114">Pro automatizované, nepřetržité úlohy měli byste použít nějaký typ účtu služby, abyste poskytli potřebná oprávnění k provádění úloh správy.</span><span class="sxs-lookup"><span data-stu-id="55f13-114">For automated, continuous tasks, you should use some type of service account that you provide with the necessary privileges to perform management tasks.</span></span> <span data-ttu-id="55f13-115">Ve službě Azure AD to provedete tak, že registrace aplikace a ověřování do služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="55f13-115">In Azure AD, you can do this by registering an application and authenticating to Azure AD.</span></span> <span data-ttu-id="55f13-116">To se provádí pomocí **ID aplikace** používající [udělení pověření klienta OAuth 2.0](../active-directory/develop/active-directory-authentication-scenarios.md#daemon-or-server-application-to-web-api).</span><span class="sxs-lookup"><span data-stu-id="55f13-116">This is done by using an **Application ID** that uses the [OAuth 2.0 client credentials grant](../active-directory/develop/active-directory-authentication-scenarios.md#daemon-or-server-application-to-web-api).</span></span> <span data-ttu-id="55f13-117">V takovém případě aplikace funguje jako samostatně, nikoli jako uživatel, k volání rozhraní Graph API.</span><span class="sxs-lookup"><span data-stu-id="55f13-117">In this case, the application acts as itself, not as a user, to call the Graph API.</span></span>

<span data-ttu-id="55f13-118">V tomto článku probereme, jak provádět případ automatizované použití.</span><span class="sxs-lookup"><span data-stu-id="55f13-118">In this article, we'll discuss how to perform the automated-use case.</span></span> <span data-ttu-id="55f13-119">K předvedení, jsme budete sestavení rozhraní .NET 4.5 `B2CGraphClient` který provede uživatele vytvořit, číst, aktualizovat a odstranit operace.</span><span class="sxs-lookup"><span data-stu-id="55f13-119">To demonstrate, we'll build a .NET 4.5 `B2CGraphClient` that performs user create, read, update, and delete (CRUD) operations.</span></span> <span data-ttu-id="55f13-120">Klient bude mít rozhraní příkazového řádku Windows (CLI), který umožňuje vyvolání různé metody.</span><span class="sxs-lookup"><span data-stu-id="55f13-120">The client will have a Windows command-line interface (CLI) that allows you to invoke various methods.</span></span> <span data-ttu-id="55f13-121">Kód je však zapsána do chovat neinteraktivní, automatizované způsobem.</span><span class="sxs-lookup"><span data-stu-id="55f13-121">However, the code is written to behave in a noninteractive, automated fashion.</span></span>

## <a name="get-an-azure-ad-b2c-tenant"></a><span data-ttu-id="55f13-122">Získání klienta Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="55f13-122">Get an Azure AD B2C tenant</span></span>
<span data-ttu-id="55f13-123">Předtím, než můžete vytvořit aplikace nebo uživatele, nebo vůbec komunikovat s Azure AD, budete potřebovat klienta Azure AD B2C a účet globálního správce v klientovi.</span><span class="sxs-lookup"><span data-stu-id="55f13-123">Before you can create applications or users, or interact with Azure AD at all, you will need an Azure AD B2C tenant and a global administrator account in the tenant.</span></span> <span data-ttu-id="55f13-124">Pokud nemáte klienta již, [Začínáme s Azure AD B2C](active-directory-b2c-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="55f13-124">If you don't have a tenant already, [get started with Azure AD B2C](active-directory-b2c-get-started.md).</span></span>

## <a name="register-your-application-in-your-tenant"></a><span data-ttu-id="55f13-125">Registrace vaší aplikace ve vašem klientovi</span><span class="sxs-lookup"><span data-stu-id="55f13-125">Register your application in your tenant</span></span>
<span data-ttu-id="55f13-126">Až budete mít klienta B2C, budete muset registrace vaší aplikace pomocí [portálu Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="55f13-126">After you have a B2C tenant, you need to register your application via the [Azure Portal](https://portal.azure.com).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="55f13-127">Použít rozhraní Graph API pomocí svého klienta B2C, budete muset registraci vyhrazené aplikace pomocí Obecné *registrace aplikace* okno na portálu Azure **není** Azure AD B2C  *Aplikace* okno.</span><span class="sxs-lookup"><span data-stu-id="55f13-127">To use the Graph API with your B2C tenant, you will need to register a dedicated application by using the generic *App Registrations* blade in the Azure Portal, **NOT** Azure AD B2C's *Applications* blade.</span></span> <span data-ttu-id="55f13-128">Nelze znovu použít už existující B2C aplikace, které jste zaregistrovali v Azure AD B2C *aplikace* okno.</span><span class="sxs-lookup"><span data-stu-id="55f13-128">You can't reuse the already-existing B2C applications that you registered in the Azure AD B2C's *Applications* blade.</span></span>

1. <span data-ttu-id="55f13-129">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="55f13-129">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="55f13-130">Výběrem účtu v pravém horním rohu stránky zvolte vašeho klienta Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="55f13-130">Choose your Azure AD B2C tenant by selecting your account in the top right corner of the page.</span></span>
3. <span data-ttu-id="55f13-131">V levém navigačním podokně zvolte **více služeb**, klikněte na tlačítko **registrace aplikace**a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="55f13-131">In the left-hand navigation pane, choose **More Services**, click **App Registrations**, and click **Add**.</span></span>
4. <span data-ttu-id="55f13-132">Postupujte podle zobrazených výzev a vytvořte novou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="55f13-132">Follow the prompts and create a new application.</span></span> 
    1. <span data-ttu-id="55f13-133">Vyberte **webová aplikace nebo rozhraní API** jako typ aplikace.</span><span class="sxs-lookup"><span data-stu-id="55f13-133">Select **Web App / API** as the Application Type.</span></span>    
    2. <span data-ttu-id="55f13-134">Zadejte **žádný identifikátor URI přesměrování** (např. https://B2CGraphAPI) jako není relevantní pro tento příklad.</span><span class="sxs-lookup"><span data-stu-id="55f13-134">Provide **any redirect URI** (e.g. https://B2CGraphAPI) as it's not relevant for this example.</span></span>  
5. <span data-ttu-id="55f13-135">Budou aplikace teď objeví v seznamu aplikací, klikněte na ho získat **ID aplikace** (také označované jako ID klienta).</span><span class="sxs-lookup"><span data-stu-id="55f13-135">The application will now show up in the list of applications, click on it to obtain the **Application ID** (also known as Client ID).</span></span> <span data-ttu-id="55f13-136">Zkopírujte jej, protože ho budete potřebovat v další části.</span><span class="sxs-lookup"><span data-stu-id="55f13-136">Copy it as you'll need it in a later section.</span></span>
6. <span data-ttu-id="55f13-137">V okně nastavení klikněte na **klíče** a přidejte nový klíč (také označovaný jako sdílený tajný klíč klienta).</span><span class="sxs-lookup"><span data-stu-id="55f13-137">In the Settings blade, click on **Keys** and add a new key (also known as client secret).</span></span> <span data-ttu-id="55f13-138">Také zkopírujte jej pro použití v další části.</span><span class="sxs-lookup"><span data-stu-id="55f13-138">Also copy it for use in a later section.</span></span>

## <a name="configure-create-read-and-update-permissions-for-your-application"></a><span data-ttu-id="55f13-139">Konfigurace vytvářet, číst a aktualizovat oprávnění pro vaši aplikaci</span><span class="sxs-lookup"><span data-stu-id="55f13-139">Configure create, read and update permissions for your application</span></span>
<span data-ttu-id="55f13-140">Teď je potřeba nakonfigurovat vaše aplikace a získejte potřebná oprávnění k vytvářet, číst, aktualizovat a odstraňovat uživatele.</span><span class="sxs-lookup"><span data-stu-id="55f13-140">Now you need to configure your application to get all the required permissions to create, read, update and delete users.</span></span>

1. <span data-ttu-id="55f13-141">Pokračování v okně registrace aplikace portálu Azure, vyberte svou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="55f13-141">Continuing in the Azure portal's App Registrations blade, select your application.</span></span>
2. <span data-ttu-id="55f13-142">V okně nastavení klikněte na **požadovaná oprávnění**.</span><span class="sxs-lookup"><span data-stu-id="55f13-142">In the Settings blade, click on **Required permissions**.</span></span>
3. <span data-ttu-id="55f13-143">V okně potřebná oprávnění, klikněte na **Windows Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="55f13-143">In the Required permissions blade, click on **Windows Azure Active Directory**.</span></span>
4. <span data-ttu-id="55f13-144">V okně povolit přístup, vyberte **pro čtení a zápis dat adresáře** oprávnění z **oprávnění aplikací** a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="55f13-144">In the Enable Access  blade, select the **Read and write directory data** permission from **Application Permissions** and click **Save**.</span></span>
5. <span data-ttu-id="55f13-145">Nakonec zpět v okně potřebná oprávnění, klikněte na **udělit oprávnění** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="55f13-145">Finally, back in the Required permissions blade, click on the **Grant Permissions** button.</span></span>

<span data-ttu-id="55f13-146">Teď máte aplikaci, která má oprávnění k vytváření, čtení a aktualizace uživatelů ze svého klienta B2C.</span><span class="sxs-lookup"><span data-stu-id="55f13-146">You now have an application that has permission to create, read and update users from your B2C tenant.</span></span>

## <a name="configure-delete-permissions-for-your-application"></a><span data-ttu-id="55f13-147">Konfigurace odstranění oprávnění pro vaši aplikaci</span><span class="sxs-lookup"><span data-stu-id="55f13-147">Configure delete permissions for your application</span></span>
<span data-ttu-id="55f13-148">V současné době *pro čtení a zápis dat adresáře* nemá oprávnění **není** zahrnují možnost udělat všechny odstranění například odstraňování uživatelů.</span><span class="sxs-lookup"><span data-stu-id="55f13-148">Currently, the *Read and write directory data* permission does **NOT** include the ability to do any deletions such as deleting users.</span></span> <span data-ttu-id="55f13-149">Pokud chcete poskytnout vaše aplikace umožňuje odstranit uživatele, je potřeba provést tyto další kroky, které se týkají prostředí PowerShell, jinak, můžete přeskočit k další části.</span><span class="sxs-lookup"><span data-stu-id="55f13-149">If you want to give your application the ability to delete users, you'll need to do these extra steps that involve PowerShell, otherwise, you can skip to the next section.</span></span>

<span data-ttu-id="55f13-150">Nejprve stáhnout a nainstalovat [Microsoft Online Services Sign-In Assistant](http://go.microsoft.com/fwlink/?LinkID=286152).</span><span class="sxs-lookup"><span data-stu-id="55f13-150">First, download and install the [Microsoft Online Services Sign-In Assistant](http://go.microsoft.com/fwlink/?LinkID=286152).</span></span> <span data-ttu-id="55f13-151">Potom stáhněte a nainstalujte [64-bit modulu Azure Active Directory pro prostředí Windows PowerShell](http://go.microsoft.com/fwlink/p/?linkid=236297).</span><span class="sxs-lookup"><span data-stu-id="55f13-151">Then download and install the [64-bit Azure Active Directory module for Windows PowerShell](http://go.microsoft.com/fwlink/p/?linkid=236297).</span></span>

<span data-ttu-id="55f13-152">Po instalaci modulu prostředí PowerShell, otevřete prostředí PowerShell a připojte se do svého klienta B2C.</span><span class="sxs-lookup"><span data-stu-id="55f13-152">After you install the PowerShell module, open PowerShell and connect to your B2C tenant.</span></span> <span data-ttu-id="55f13-153">Po spuštění `Get-Credential`, zobrazí se výzva pro zadání uživatelského jména a hesla, zadejte uživatelské jméno a heslo účtu správce klienta B2C.</span><span class="sxs-lookup"><span data-stu-id="55f13-153">After you run `Get-Credential`, you will be prompted for a user name and password, Enter the user name and password of your B2C tenant administrator account.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="55f13-154">Je nutné použít účet správce klienta B2C, který je **místní** klienta B2C.</span><span class="sxs-lookup"><span data-stu-id="55f13-154">You need to use a B2C tenant administrator account that is **local** to the B2C tenant.</span></span> <span data-ttu-id="55f13-155">Tyto účty vypadat takto: myusername@myb2ctenant.onmicrosoft.com.</span><span class="sxs-lookup"><span data-stu-id="55f13-155">These accounts look like this: myusername@myb2ctenant.onmicrosoft.com.</span></span>

```powershell
Connect-MsolService
```

<span data-ttu-id="55f13-156">Nyní použijeme **ID aplikace** v níže přiřadit role správce účtu uživatele, která umožní odstranit uživatele aplikace skriptu.</span><span class="sxs-lookup"><span data-stu-id="55f13-156">Now we'll use the **Application ID** in the script below to assign the application the user account administrator role which will allow it to delete users.</span></span> <span data-ttu-id="55f13-157">Tyto role mají dobře známé identifikátory, takže všechny musíte udělat není vstupní vaše **ID aplikace** v níže uvedeném skriptu.</span><span class="sxs-lookup"><span data-stu-id="55f13-157">These roles have well-known identifiers, so all you need to do is input your **Application ID** in the script below.</span></span>

```powershell
$applicationId = "<YOUR_APPLICATION_ID>"
$sp = Get-MsolServicePrincipal -AppPrincipalId $applicationId
Add-MsolRoleMember -RoleObjectId fe930be7-5e62-47db-91af-98c3a49a38b1 -RoleMemberObjectId $sp.ObjectId -RoleMemberType servicePrincipal
```

<span data-ttu-id="55f13-158">Aplikaci teď má také oprávnění k odstranění uživatelů z vašeho klienta B2C.</span><span class="sxs-lookup"><span data-stu-id="55f13-158">Your application now also has permissions to delete users from your B2C tenant.</span></span>

## <a name="download-configure-and-build-the-sample-code"></a><span data-ttu-id="55f13-159">Stáhněte si, konfigurace a sestavte ukázkový kód</span><span class="sxs-lookup"><span data-stu-id="55f13-159">Download, configure, and build the sample code</span></span>
<span data-ttu-id="55f13-160">Nejprve stáhnout ukázkový kód a získat běh aplikace.</span><span class="sxs-lookup"><span data-stu-id="55f13-160">First, download the sample code and get it running.</span></span> <span data-ttu-id="55f13-161">Potom jsme bude trvat bližší pohled na ho.</span><span class="sxs-lookup"><span data-stu-id="55f13-161">Then we will take a closer look at it.</span></span>  <span data-ttu-id="55f13-162">Můžete [stáhnout ukázkový kód v souboru ZIP](https://github.com/AzureADQuickStarts/B2C-GraphAPI-DotNet/archive/master.zip).</span><span class="sxs-lookup"><span data-stu-id="55f13-162">You can [download the sample code as a .zip file](https://github.com/AzureADQuickStarts/B2C-GraphAPI-DotNet/archive/master.zip).</span></span> <span data-ttu-id="55f13-163">Můžete ho také klonovat do adresáře podle vašeho výběru:</span><span class="sxs-lookup"><span data-stu-id="55f13-163">You can also clone it into a directory of your choice:</span></span>

```
git clone https://github.com/AzureADQuickStarts/B2C-GraphAPI-DotNet.git
```

<span data-ttu-id="55f13-164">Otevřete `B2CGraphClient\B2CGraphClient.sln` řešení sady Visual Studio v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="55f13-164">Open the `B2CGraphClient\B2CGraphClient.sln` Visual Studio solution in Visual Studio.</span></span> <span data-ttu-id="55f13-165">V `B2CGraphClient` projektu, otevřete soubor `App.config`.</span><span class="sxs-lookup"><span data-stu-id="55f13-165">In the `B2CGraphClient` project, open the file `App.config`.</span></span> <span data-ttu-id="55f13-166">Nastavení tři aplikace nahraďte vlastními hodnotami:</span><span class="sxs-lookup"><span data-stu-id="55f13-166">Replace the three app settings with your own values:</span></span>

```
<appSettings>
    <add key="b2c:Tenant" value="{Your Tenant Name}" />
    <add key="b2c:ClientId" value="{The ApplicationID from above}" />
    <add key="b2c:ClientSecret" value="{The Key from above}" />
</appSettings>
```

[!INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

<span data-ttu-id="55f13-167">Pak klikněte pravým tlačítkem na `B2CGraphClient` řešení a znovu sestavit ukázku.</span><span class="sxs-lookup"><span data-stu-id="55f13-167">Next, right-click on the `B2CGraphClient` solution and rebuild the sample.</span></span> <span data-ttu-id="55f13-168">Pokud jste úspěšné, teď byste měli mít `B2C.exe` spustitelný soubor umístěný ve `B2CGraphClient\bin\Debug`.</span><span class="sxs-lookup"><span data-stu-id="55f13-168">If you are successful, you should now have a `B2C.exe` executable file located in `B2CGraphClient\bin\Debug`.</span></span>

## <a name="build-user-crud-operations-by-using-the-graph-api"></a><span data-ttu-id="55f13-169">Vytvoření operace CRUD uživatele pomocí rozhraní Graph API</span><span class="sxs-lookup"><span data-stu-id="55f13-169">Build user CRUD operations by using the Graph API</span></span>
<span data-ttu-id="55f13-170">Chcete-li použít B2CGraphClient, otevřete `cmd` Windows příkazového řádku a přejděte do adresáře `Debug` adresáře.</span><span class="sxs-lookup"><span data-stu-id="55f13-170">To use the B2CGraphClient, open a `cmd` Windows command prompt and change your directory to the `Debug` directory.</span></span> <span data-ttu-id="55f13-171">Spusťte `B2C Help` příkaz.</span><span class="sxs-lookup"><span data-stu-id="55f13-171">Then run the `B2C Help` command.</span></span>

```
> cd B2CGraphClient\bin\Debug
> B2C Help
```

<span data-ttu-id="55f13-172">Tato akce zobrazí stručný popis každého příkazu.</span><span class="sxs-lookup"><span data-stu-id="55f13-172">This will display a brief description of each command.</span></span> <span data-ttu-id="55f13-173">Pokaždé, když vyvolat jeden z těchto příkazů `B2CGraphClient` odešle požadavek do Azure AD Graph API.</span><span class="sxs-lookup"><span data-stu-id="55f13-173">Each time you invoke one of these commands, `B2CGraphClient` makes a request to the Azure AD Graph API.</span></span>

### <a name="get-an-access-token"></a><span data-ttu-id="55f13-174">Získání přístupového tokenu</span><span class="sxs-lookup"><span data-stu-id="55f13-174">Get an access token</span></span>
<span data-ttu-id="55f13-175">Každá žádost o rozhraní Graph API vyžaduje token přístupu pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="55f13-175">Any request to the Graph API requires an access token for authentication.</span></span> <span data-ttu-id="55f13-176">`B2CGraphClient`pomocí open source Active Directory Authentication Library (ADAL) k získání přístupových tokenů.</span><span class="sxs-lookup"><span data-stu-id="55f13-176">`B2CGraphClient` uses the open-source Active Directory Authentication Library (ADAL) to help acquire access tokens.</span></span> <span data-ttu-id="55f13-177">ADAL usnadňuje získávání tokenu poskytuje jednoduché rozhraní API a postará o některé důležité podrobnosti, jako je například ukládání do mezipaměti přístupových tokenů.</span><span class="sxs-lookup"><span data-stu-id="55f13-177">ADAL makes token acquisition easier by providing a simple API and taking care of some important details, such as caching access tokens.</span></span> <span data-ttu-id="55f13-178">Nemáte vám pomůže získat tokeny, když ADAL.</span><span class="sxs-lookup"><span data-stu-id="55f13-178">You don't have to use ADAL to get tokens, though.</span></span> <span data-ttu-id="55f13-179">Můžete také získat tokeny tím, že vytvoří požadavky HTTP.</span><span class="sxs-lookup"><span data-stu-id="55f13-179">You can also get tokens by crafting HTTP requests.</span></span>

> [!NOTE]
> <span data-ttu-id="55f13-180">Tento příklad používá ADAL v2 komunikovat s rozhraní Graph API.</span><span class="sxs-lookup"><span data-stu-id="55f13-180">This code sample uses ADAL v2 in order to communicate with the Graph API.</span></span>  <span data-ttu-id="55f13-181">Pokud chcete získat přístupové tokeny, které se dají použít s Azure AD Graph API je nutné použít ADAL verze 2 nebo 3.</span><span class="sxs-lookup"><span data-stu-id="55f13-181">You must use ADAL v2 or v3 in order to get access tokens which can be used with the Azure AD Graph API.</span></span>
> 
> 

<span data-ttu-id="55f13-182">Když `B2CGraphClient` spustí, vytvoří instanci `B2CGraphClient` třídy.</span><span class="sxs-lookup"><span data-stu-id="55f13-182">When `B2CGraphClient` runs, it creates an instance of the `B2CGraphClient` class.</span></span> <span data-ttu-id="55f13-183">V konstruktoru pro tuto třídu nastaví generování uživatelského rozhraní ověřování pomocí knihovny ADAL:</span><span class="sxs-lookup"><span data-stu-id="55f13-183">The constructor for this class sets up an ADAL authentication scaffolding:</span></span>

```C#
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

<span data-ttu-id="55f13-184">Použijeme `B2C Get-User` příkaz jako příklad.</span><span class="sxs-lookup"><span data-stu-id="55f13-184">We'll use the `B2C Get-User` command as an example.</span></span> <span data-ttu-id="55f13-185">Když `B2C Get-User` je volána bez jakékoli další vstupy volání rozhraní příkazového řádku `B2CGraphClient.GetAllUsers(...)` metoda.</span><span class="sxs-lookup"><span data-stu-id="55f13-185">When `B2C Get-User` is invoked without any additional inputs, the CLI calls the `B2CGraphClient.GetAllUsers(...)` method.</span></span> <span data-ttu-id="55f13-186">Tato metoda volá `B2CGraphClient.SendGraphGetRequest(...)`, který odesílá požadavek HTTP GET pro rozhraní Graph API.</span><span class="sxs-lookup"><span data-stu-id="55f13-186">This method calls `B2CGraphClient.SendGraphGetRequest(...)`, which submits an HTTP GET request to the Graph API.</span></span> <span data-ttu-id="55f13-187">Před `B2CGraphClient.SendGraphGetRequest(...)` odešle požadavek GET, nejdřív získá přístup tokenu pomocí ADAL:</span><span class="sxs-lookup"><span data-stu-id="55f13-187">Before `B2CGraphClient.SendGraphGetRequest(...)` sends the GET request, it first gets an access token by using ADAL:</span></span>

```C#
public async Task<string> SendGraphGetRequest(string api, string query)
{
    // First, use ADAL to acquire a token by using the app's identity (the credential)
    // The first parameter is the resource we want an access_token for; in this case, the Graph API.
    AuthenticationResult result = authContext.AcquireToken("https://graph.windows.net", credential);

    ...

```

<span data-ttu-id="55f13-188">Můžete získat přístup tokenu pro rozhraní Graph API voláním knihovnu ADAL `AuthenticationContext.AcquireToken(...)` metoda.</span><span class="sxs-lookup"><span data-stu-id="55f13-188">You can get an access token for the Graph API by calling the ADAL `AuthenticationContext.AcquireToken(...)` method.</span></span> <span data-ttu-id="55f13-189">Vrátí ADAL `access_token` představující identitu aplikace.</span><span class="sxs-lookup"><span data-stu-id="55f13-189">ADAL then returns an `access_token` that represents the application's identity.</span></span>

### <a name="read-users"></a><span data-ttu-id="55f13-190">Uživatelé pro čtení</span><span class="sxs-lookup"><span data-stu-id="55f13-190">Read users</span></span>
<span data-ttu-id="55f13-191">Pokud chcete získat seznam uživatelů, nebo získat určitého uživatele z rozhraní Graph API, můžete odeslat HTTP `GET` žádost o `/users` koncový bod.</span><span class="sxs-lookup"><span data-stu-id="55f13-191">When you want to get a list of users or get a particular user from the Graph API, you can send an HTTP `GET` request to the `/users` endpoint.</span></span> <span data-ttu-id="55f13-192">Žádost o pro všechny uživatele v klienta vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="55f13-192">A request for all of the users in a tenant looks like this:</span></span>

```
GET https://graph.windows.net/contosob2c.onmicrosoft.com/users?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
```

<span data-ttu-id="55f13-193">Pokud chcete zobrazit tento požadavek, spusťte příkaz:</span><span class="sxs-lookup"><span data-stu-id="55f13-193">To see this request, run:</span></span>

 ```
 > B2C Get-User
 ```

<span data-ttu-id="55f13-194">Všimněte si, dvě důležité věci:</span><span class="sxs-lookup"><span data-stu-id="55f13-194">There are two important things to note:</span></span>

* <span data-ttu-id="55f13-195">Přístupový token získaných prostřednictvím ADAL je přidán do `Authorization` záhlaví s použitím `Bearer` schéma.</span><span class="sxs-lookup"><span data-stu-id="55f13-195">The access token acquired via ADAL is added to the `Authorization` header by using the `Bearer` scheme.</span></span>
* <span data-ttu-id="55f13-196">U klientů B2C, musíte použít parametr dotazu `api-version=1.6`.</span><span class="sxs-lookup"><span data-stu-id="55f13-196">For B2C tenants, you must use the query parameter `api-version=1.6`.</span></span>

<span data-ttu-id="55f13-197">Obě tyto podrobnosti jsou zpracovávány v `B2CGraphClient.SendGraphGetRequest(...)` metoda:</span><span class="sxs-lookup"><span data-stu-id="55f13-197">Both of these details are handled in the `B2CGraphClient.SendGraphGetRequest(...)` method:</span></span>

```C#
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

### <a name="create-consumer-user-accounts"></a><span data-ttu-id="55f13-198">Vytvoření příjemce uživatelské účty</span><span class="sxs-lookup"><span data-stu-id="55f13-198">Create consumer user accounts</span></span>
<span data-ttu-id="55f13-199">Při vytváření uživatelských účtů v svého klienta B2C, můžete odeslat HTTP `POST` žádost o `/users` koncový bod:</span><span class="sxs-lookup"><span data-stu-id="55f13-199">When you create user accounts in your B2C tenant, you can send an HTTP `POST` request to the `/users` endpoint:</span></span>

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

<span data-ttu-id="55f13-200">Většina těchto vlastností v této žádosti o nutné vytvořit spotřebitelské uživatele.</span><span class="sxs-lookup"><span data-stu-id="55f13-200">Most of these properties in this request are required to create consumer users.</span></span> <span data-ttu-id="55f13-201">Chcete-li získat další informace, klikněte na tlačítko [zde](https://msdn.microsoft.com/library/azure/ad/graph/api/users-operations#CreateLocalAccountUser).</span><span class="sxs-lookup"><span data-stu-id="55f13-201">To learn more, click [here](https://msdn.microsoft.com/library/azure/ad/graph/api/users-operations#CreateLocalAccountUser).</span></span> <span data-ttu-id="55f13-202">Všimněte si, že `//` komentáře byly zahrnuty pro obrázek.</span><span class="sxs-lookup"><span data-stu-id="55f13-202">Note that the `//` comments have been included for illustration.</span></span> <span data-ttu-id="55f13-203">V skutečné požadavku je nezahrnujte.</span><span class="sxs-lookup"><span data-stu-id="55f13-203">Do not include them in an actual request.</span></span>

<span data-ttu-id="55f13-204">Pokud chcete zobrazit žádost, spusťte jeden z následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="55f13-204">To see the request, run one of the following commands:</span></span>

```
> B2C Create-User ..\..\..\usertemplate-email.json
> B2C Create-User ..\..\..\usertemplate-username.json
```

<span data-ttu-id="55f13-205">`Create-User` Příkaz má soubor .json jako vstupní parametr.</span><span class="sxs-lookup"><span data-stu-id="55f13-205">The `Create-User` command takes a .json file as an input parameter.</span></span> <span data-ttu-id="55f13-206">Tato položka obsahuje reprezentaci JSON objektu uživatele.</span><span class="sxs-lookup"><span data-stu-id="55f13-206">This contains a JSON representation of a user object.</span></span> <span data-ttu-id="55f13-207">Existují dva ukázkové soubory .json v ukázkovém kódu: `usertemplate-email.json` a `usertemplate-username.json`.</span><span class="sxs-lookup"><span data-stu-id="55f13-207">There are two sample .json files in the sample code: `usertemplate-email.json` and `usertemplate-username.json`.</span></span> <span data-ttu-id="55f13-208">Můžete upravit tyto soubory tak, aby vyhovovala vašim potřebám.</span><span class="sxs-lookup"><span data-stu-id="55f13-208">You can modify these files to suit your needs.</span></span> <span data-ttu-id="55f13-209">Kromě výše uvedených povinná pole jsou zahrnuty několik volitelná pole, které můžete použít v těchto souborech.</span><span class="sxs-lookup"><span data-stu-id="55f13-209">In addition to the required fields above, several optional fields that you can use are included in these files.</span></span> <span data-ttu-id="55f13-210">Podrobnosti o volitelná pole lze nalézt v [odkaz na Azure AD Graph API entity](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity).</span><span class="sxs-lookup"><span data-stu-id="55f13-210">Details on the optional fields can be found in the [Azure AD Graph API entity reference](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity).</span></span>

<span data-ttu-id="55f13-211">Můžete vidět, jak je požadavek POST vytvořený v `B2CGraphClient.SendGraphPostRequest(...)`.</span><span class="sxs-lookup"><span data-stu-id="55f13-211">You can see how the POST request is constructed in `B2CGraphClient.SendGraphPostRequest(...)`.</span></span>

* <span data-ttu-id="55f13-212">Přiloží přístupového tokenu k `Authorization` hlavičky žádosti.</span><span class="sxs-lookup"><span data-stu-id="55f13-212">It attaches an access token to the `Authorization` header of the request.</span></span>
* <span data-ttu-id="55f13-213">Nastaví `api-version=1.6`.</span><span class="sxs-lookup"><span data-stu-id="55f13-213">It sets `api-version=1.6`.</span></span>
* <span data-ttu-id="55f13-214">Obsahuje uživatele objekt JSON v textu požadavku.</span><span class="sxs-lookup"><span data-stu-id="55f13-214">It includes the JSON user object in the body of the request.</span></span>

> [!NOTE]
> <span data-ttu-id="55f13-215">Pokud účty, které chcete provést migraci z existující úložiště uživatele má nižší síly hesla, než [silné heslo sílu vynucováno Azure AD B2C](https://msdn.microsoft.com/library/azure/jj943764.aspx), můžete zakázat pomocí požadavek na silné heslo `DisableStrongPassword` Hodnota v `passwordPolicies` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="55f13-215">If the accounts that you want to migrate from an existing user store has lower password strength than the [strong password strength enforced by Azure AD B2C](https://msdn.microsoft.com/library/azure/jj943764.aspx), you can disable the strong password requirement using the `DisableStrongPassword` value in the `passwordPolicies` property.</span></span> <span data-ttu-id="55f13-216">Například můžete upravit požadavek na vytvoření uživatele výše uvedeného následujícím způsobem: `"passwordPolicies": "DisablePasswordExpiration, DisableStrongPassword"`.</span><span class="sxs-lookup"><span data-stu-id="55f13-216">For instance, you can modify the create user request provided above as follows: `"passwordPolicies": "DisablePasswordExpiration, DisableStrongPassword"`.</span></span>
> 
> 

### <a name="update-consumer-user-accounts"></a><span data-ttu-id="55f13-217">Aktualizovat příjemce uživatelské účty</span><span class="sxs-lookup"><span data-stu-id="55f13-217">Update consumer user accounts</span></span>
<span data-ttu-id="55f13-218">Při aktualizaci uživatelských objektů proces je podobný je ta, kterou použijete k vytvoření uživatelské objekty.</span><span class="sxs-lookup"><span data-stu-id="55f13-218">When you update user objects, the process is similar to the one you use to create user objects.</span></span> <span data-ttu-id="55f13-219">Tento proces používá HTTP, ale `PATCH` metoda:</span><span class="sxs-lookup"><span data-stu-id="55f13-219">But this process uses the HTTP `PATCH` method:</span></span>

```
PATCH https://graph.windows.net/contosob2c.onmicrosoft.com/users/<user-object-id>?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
Content-Type: application/json
Content-Length: 37

{
    "displayName": "Joe Consumer",                // this request updates only the user's displayName
}
```

<span data-ttu-id="55f13-220">Zkuste provést aktualizaci uživatele tím, že aktualizuje vaše soubory JSON se nová data.</span><span class="sxs-lookup"><span data-stu-id="55f13-220">Try to update a user by updating your JSON files with new data.</span></span> <span data-ttu-id="55f13-221">Pak můžete použít `B2CGraphClient` spustit jeden z těchto příkazů:</span><span class="sxs-lookup"><span data-stu-id="55f13-221">You can then use `B2CGraphClient` to run one of these commands:</span></span>

```
> B2C Update-User <user-object-id> ..\..\..\usertemplate-email.json
> B2C Update-User <user-object-id> ..\..\..\usertemplate-username.json
```

<span data-ttu-id="55f13-222">Zkontrolujte `B2CGraphClient.SendGraphPatchRequest(...)` metoda podrobnosti o tom, jak odeslat tuto žádost.</span><span class="sxs-lookup"><span data-stu-id="55f13-222">Inspect the `B2CGraphClient.SendGraphPatchRequest(...)` method for details on how to send this request.</span></span>

### <a name="search-users"></a><span data-ttu-id="55f13-223">Vyhledávat uživatele</span><span class="sxs-lookup"><span data-stu-id="55f13-223">Search users</span></span>
<span data-ttu-id="55f13-224">Můžete hledat uživatele v svého klienta B2C v několika způsoby.</span><span class="sxs-lookup"><span data-stu-id="55f13-224">You can search for users in your B2C tenant in a couple of ways.</span></span> <span data-ttu-id="55f13-225">Jednoho uživatele pomocí objektu ID nebo dva pomocí uživatele přihlašovací identifikátor (tj, `signInNames` vlastnost).</span><span class="sxs-lookup"><span data-stu-id="55f13-225">One, using the user's object ID or two, using the user's sign-in identifer (i.e., the `signInNames` property).</span></span>

<span data-ttu-id="55f13-226">Spusťte jeden z následujících příkazů k vyhledání konkrétního uživatele:</span><span class="sxs-lookup"><span data-stu-id="55f13-226">Run one of the following commands to search for a specific user:</span></span>

```
> B2C Get-User <user-object-id>
> B2C Get-User <filter-query-expression>
```

<span data-ttu-id="55f13-227">Tady je několik příkladů:</span><span class="sxs-lookup"><span data-stu-id="55f13-227">Here are a couple of examples:</span></span>

```
> B2C Get-User 2bcf1067-90b6-4253-9991-7f16449c2d91
> B2C Get-User $filter=signInNames/any(x:x/value%20eq%20%27joeconsumer@gmail.com%27)
```

### <a name="delete-users"></a><span data-ttu-id="55f13-228">Odstranit uživatele</span><span class="sxs-lookup"><span data-stu-id="55f13-228">Delete users</span></span>
<span data-ttu-id="55f13-229">Proces odstranění uživatele je jednoduchá.</span><span class="sxs-lookup"><span data-stu-id="55f13-229">The process for deleting a user is straightforward.</span></span> <span data-ttu-id="55f13-230">Pomocí HTTP `DELETE` metoda a konstrukce adresu URL s správné ID objektu:</span><span class="sxs-lookup"><span data-stu-id="55f13-230">Use the HTTP `DELETE` method and construct the URL with the correct object ID:</span></span>

```
DELETE https://graph.windows.net/contosob2c.onmicrosoft.com/users/<user-object-id>?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
```

<span data-ttu-id="55f13-231">Pokud chcete zobrazit příklad, zadejte tento příkaz a zobrazit odstranit požadavek, který je vytištěno do konzoly:</span><span class="sxs-lookup"><span data-stu-id="55f13-231">To see an example, enter this command and view the delete request that is printed to the console:</span></span>

```
> B2C Delete-User <object-id-of-user>
```

<span data-ttu-id="55f13-232">Zkontrolujte `B2CGraphClient.SendGraphDeleteRequest(...)` metoda podrobnosti o tom, jak odeslat tuto žádost.</span><span class="sxs-lookup"><span data-stu-id="55f13-232">Inspect the `B2CGraphClient.SendGraphDeleteRequest(...)` method for details on how to send this request.</span></span>

<span data-ttu-id="55f13-233">Můžete provádět mnoho dalších akcí s Azure AD Graph API kromě správy uživatelů.</span><span class="sxs-lookup"><span data-stu-id="55f13-233">You can perform many other actions with the Azure AD Graph API in addition to user management.</span></span> <span data-ttu-id="55f13-234">[Referenční dokumentace rozhraní API Azure AD Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) poskytuje podrobnosti pro každou akci, společně s požadavky na ukázky.</span><span class="sxs-lookup"><span data-stu-id="55f13-234">The [Azure AD Graph API reference](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) provides details on each action, along with sample requests.</span></span>

## <a name="use-custom-attributes"></a><span data-ttu-id="55f13-235">Použití vlastních atributů</span><span class="sxs-lookup"><span data-stu-id="55f13-235">Use custom attributes</span></span>
<span data-ttu-id="55f13-236">Většina uživatelských aplikací je nutné uložit nějaký typ informace vlastního uživatelského profilu.</span><span class="sxs-lookup"><span data-stu-id="55f13-236">Most consumer applications need to store some type of custom user profile information.</span></span> <span data-ttu-id="55f13-237">Jedním ze způsobů, můžete k tomu je k definování vlastní atribut v svého klienta B2C.</span><span class="sxs-lookup"><span data-stu-id="55f13-237">One way you can do this is to define a custom attribute in your B2C tenant.</span></span> <span data-ttu-id="55f13-238">Poté můžete ke tento atribut stejným způsobem jako považovat všechny ostatní vlastnosti v objektu user.</span><span class="sxs-lookup"><span data-stu-id="55f13-238">You can then treat that attribute the same way you treat any other property on a user object.</span></span> <span data-ttu-id="55f13-239">Můžete aktualizovat atribut, odstranit atribut, dotaz v atributu, odesílat atribut jako deklaraci v tokeny přihlášení a další.</span><span class="sxs-lookup"><span data-stu-id="55f13-239">You can update the attribute, delete the attribute, query by the attribute, send the attribute as a claim in sign-in tokens, and more.</span></span>

<span data-ttu-id="55f13-240">K definování vlastní atribut v svého klienta B2C, najdete v článku [odkaz vlastní atribut B2C](active-directory-b2c-reference-custom-attr.md).</span><span class="sxs-lookup"><span data-stu-id="55f13-240">To define a custom attribute in your B2C tenant, see the [B2C custom attribute reference](active-directory-b2c-reference-custom-attr.md).</span></span>

<span data-ttu-id="55f13-241">Můžete zobrazit vlastní atributy definované ve vašem klientovi B2C pomocí `B2CGraphClient`:</span><span class="sxs-lookup"><span data-stu-id="55f13-241">You can view the custom attributes defined in your B2C tenant by using `B2CGraphClient`:</span></span>

```
> B2C Get-B2C-Application
> B2C Get-Extension-Attribute <object-id-in-the-output-of-the-above-command>
```

<span data-ttu-id="55f13-242">Výstup z těchto funkcí zobrazí podrobnosti o jednotlivých vlastních atributů, například:</span><span class="sxs-lookup"><span data-stu-id="55f13-242">The output of these functions reveals the details of each custom attribute, such as:</span></span>

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

<span data-ttu-id="55f13-243">Úplný název, můžete použít jako `extension_55dc0861f9a44eb999e0a8a872204adb_Jersey_Number`, jako vlastnost na uživatelské objekty.</span><span class="sxs-lookup"><span data-stu-id="55f13-243">You can use the full name, such as `extension_55dc0861f9a44eb999e0a8a872204adb_Jersey_Number`, as a property on your user objects.</span></span>  <span data-ttu-id="55f13-244">Aktualizujte si soubor .json nové vlastnosti a hodnotu pro vlastnost a potom spusťte:</span><span class="sxs-lookup"><span data-stu-id="55f13-244">Update your .json file with the new property and a value for the property, and then run:</span></span>

```
> B2C Update-User <object-id-of-user> <path-to-json-file>
```

<span data-ttu-id="55f13-245">Pomocí `B2CGraphClient`, máte aplikaci služby, která můžete spravovat vaše uživatele klienta B2C prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="55f13-245">By using `B2CGraphClient`, you have a service application that can manage your B2C tenant users programmatically.</span></span> <span data-ttu-id="55f13-246">`B2CGraphClient`vlastní identity aplikace se používá k ověření Azure AD Graph API.</span><span class="sxs-lookup"><span data-stu-id="55f13-246">`B2CGraphClient` uses its own application identity to authenticate to the Azure AD Graph API.</span></span> <span data-ttu-id="55f13-247">Je také získá tokeny pomocí tajný klíč klienta.</span><span class="sxs-lookup"><span data-stu-id="55f13-247">It also acquires tokens by using a client secret.</span></span> <span data-ttu-id="55f13-248">Protože tato funkce se začlenit do vaší aplikace, mějte na paměti několik klíčových bodů pro B2C aplikace:</span><span class="sxs-lookup"><span data-stu-id="55f13-248">As you incorporate this functionality into your application, remember a few key points for B2C apps:</span></span>

* <span data-ttu-id="55f13-249">Je třeba udělit příslušná oprávnění v klientovi aplikace.</span><span class="sxs-lookup"><span data-stu-id="55f13-249">You need to grant the application the proper permissions in the tenant.</span></span>
* <span data-ttu-id="55f13-250">Nyní potřebujete získat přístupové tokeny pomocí knihovny ADAL (ne MSAL).</span><span class="sxs-lookup"><span data-stu-id="55f13-250">For now, you need to use ADAL (not MSAL) to get access tokens.</span></span> <span data-ttu-id="55f13-251">(Můžete také odeslat zprávy protokolu přímo, bez použití knihovny.)</span><span class="sxs-lookup"><span data-stu-id="55f13-251">(You can also send protocol messages directly, without using a library.)</span></span>
* <span data-ttu-id="55f13-252">Při volání rozhraní Graph API, použijte `api-version=1.6`.</span><span class="sxs-lookup"><span data-stu-id="55f13-252">When you call the Graph API, use `api-version=1.6`.</span></span>
* <span data-ttu-id="55f13-253">Při vytváření a aktualizovat spotřebitelské uživatele, jsou povinné, několik vlastností, jak je popsáno výše.</span><span class="sxs-lookup"><span data-stu-id="55f13-253">When you create and update consumer users, a few properties are required, as described above.</span></span>

<span data-ttu-id="55f13-254">Pokud máte jakékoli dotazy nebo požadavků pro akce, že kterou chcete provést pomocí rozhraní Graph API na svého klienta B2C, poznámky v tomto článku nebo v úložišti GitHub ukázkový kód soubor problém.</span><span class="sxs-lookup"><span data-stu-id="55f13-254">If you have any questions or requests for actions you would like to perform by using the Graph API on your B2C tenant, leave a comment on this article or file an issue in the GitHub code sample repository.</span></span>

