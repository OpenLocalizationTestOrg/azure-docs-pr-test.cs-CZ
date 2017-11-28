---
title: "Azure Active Directory B2C: Použití hello rozhraní Graph API | Microsoft Docs"
description: "Jak toocall hello rozhraní Graph API pro klienta B2C pomocí procesu hello tooautomate identity aplikace."
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
ms.openlocfilehash: a17cdc4adf57dbf22592d99ef8ecde41652763fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-b2c-use-hello-graph-api"></a><span data-ttu-id="402f9-103">Azure AD B2C: Použijte hello rozhraní Graph API</span><span class="sxs-lookup"><span data-stu-id="402f9-103">Azure AD B2C: Use hello Graph API</span></span>
<span data-ttu-id="402f9-104">Azure Active Directory (Azure AD) B2C klienty zpravidla toobe velké.</span><span class="sxs-lookup"><span data-stu-id="402f9-104">Azure Active Directory (Azure AD) B2C tenants tend toobe very large.</span></span> <span data-ttu-id="402f9-105">To znamená, že mnoho běžné úlohy správy klienta nutné toobe provést prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="402f9-105">This means that many common tenant management tasks need toobe performed programmatically.</span></span> <span data-ttu-id="402f9-106">Primární příkladem je Správa uživatelů.</span><span class="sxs-lookup"><span data-stu-id="402f9-106">A primary example is user management.</span></span> <span data-ttu-id="402f9-107">Může být nutné toomigrate existujícího klienta B2C úložiště tooa uživatele.</span><span class="sxs-lookup"><span data-stu-id="402f9-107">You might need toomigrate an existing user store tooa B2C tenant.</span></span> <span data-ttu-id="402f9-108">Mohou má toohost registrace uživatele na vlastní stránce a vytvářet uživatelské účty ve vašem adresáři Azure AD B2C pozadí hello.</span><span class="sxs-lookup"><span data-stu-id="402f9-108">You may want toohost user registration on your own page and create user accounts in your Azure AD B2C directory behind hello scenes.</span></span> <span data-ttu-id="402f9-109">Tyto typy úloh vyžadují hello možnost toocreate, číst, aktualizovat a odstraňovat uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="402f9-109">These types of tasks require hello ability toocreate, read, update, and delete user accounts.</span></span> <span data-ttu-id="402f9-110">Můžete provést tyto úlohy pomocí hello Azure AD Graph API.</span><span class="sxs-lookup"><span data-stu-id="402f9-110">You can do these tasks by using hello Azure AD Graph API.</span></span>

<span data-ttu-id="402f9-111">U klientů B2C existují dva primární režimy komunikaci s hello rozhraní Graph API.</span><span class="sxs-lookup"><span data-stu-id="402f9-111">For B2C tenants, there are two primary modes of communicating with hello Graph API.</span></span>

* <span data-ttu-id="402f9-112">Pro interaktivní, jedno spuštění úlohy by měla fungovat jako účet správce v klienta B2C hello při provádění úloh, hello.</span><span class="sxs-lookup"><span data-stu-id="402f9-112">For interactive, run-once tasks, you should act as an administrator account in hello B2C tenant when you perform hello tasks.</span></span> <span data-ttu-id="402f9-113">Tento režim vyžaduje toosign správce se pomocí přihlašovacích údajů, před kterou správce může provádět všechny toohello volání rozhraní Graph API.</span><span class="sxs-lookup"><span data-stu-id="402f9-113">This mode requires an administrator toosign in with credentials before that admin can perform any calls toohello Graph API.</span></span>
* <span data-ttu-id="402f9-114">Pro automatizované, nepřetržité úlohy měli byste použít nějaký typ účtu služby, který zadáte pomocí úloh správy tooperform hello potřebná oprávnění.</span><span class="sxs-lookup"><span data-stu-id="402f9-114">For automated, continuous tasks, you should use some type of service account that you provide with hello necessary privileges tooperform management tasks.</span></span> <span data-ttu-id="402f9-115">Ve službě Azure AD to provedete tak, že registrace aplikace a ověřování tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="402f9-115">In Azure AD, you can do this by registering an application and authenticating tooAzure AD.</span></span> <span data-ttu-id="402f9-116">To se provádí pomocí **ID aplikace** používající hello [udělení pověření klienta OAuth 2.0](../active-directory/develop/active-directory-authentication-scenarios.md#daemon-or-server-application-to-web-api).</span><span class="sxs-lookup"><span data-stu-id="402f9-116">This is done by using an **Application ID** that uses hello [OAuth 2.0 client credentials grant](../active-directory/develop/active-directory-authentication-scenarios.md#daemon-or-server-application-to-web-api).</span></span> <span data-ttu-id="402f9-117">V takovém případě hello aplikace funguje jako samostatně, nikoli jako uživatel, toocall hello rozhraní Graph API.</span><span class="sxs-lookup"><span data-stu-id="402f9-117">In this case, hello application acts as itself, not as a user, toocall hello Graph API.</span></span>

<span data-ttu-id="402f9-118">V tomto článku probereme, jak tooperform hello případ automatizované použití.</span><span class="sxs-lookup"><span data-stu-id="402f9-118">In this article, we'll discuss how tooperform hello automated-use case.</span></span> <span data-ttu-id="402f9-119">toodemonstrate, jsme budete sestavení rozhraní .NET 4.5 `B2CGraphClient` který provede uživatele vytvořit, číst, aktualizovat a odstranit operace.</span><span class="sxs-lookup"><span data-stu-id="402f9-119">toodemonstrate, we'll build a .NET 4.5 `B2CGraphClient` that performs user create, read, update, and delete (CRUD) operations.</span></span> <span data-ttu-id="402f9-120">Hello klienta bude mít rozhraní Windows příkazového řádku (CLI), který vám umožní tooinvoke různé metody.</span><span class="sxs-lookup"><span data-stu-id="402f9-120">hello client will have a Windows command-line interface (CLI) that allows you tooinvoke various methods.</span></span> <span data-ttu-id="402f9-121">Kód hello je však zapsána toobehave neinteraktivní, automatizované způsobem.</span><span class="sxs-lookup"><span data-stu-id="402f9-121">However, hello code is written toobehave in a noninteractive, automated fashion.</span></span>

## <a name="get-an-azure-ad-b2c-tenant"></a><span data-ttu-id="402f9-122">Získání klienta Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="402f9-122">Get an Azure AD B2C tenant</span></span>
<span data-ttu-id="402f9-123">Předtím, než můžete vytvořit aplikace nebo uživatele, nebo vůbec komunikovat s Azure AD, budete potřebovat klienta Azure AD B2C a účet globálního správce v klientovi hello.</span><span class="sxs-lookup"><span data-stu-id="402f9-123">Before you can create applications or users, or interact with Azure AD at all, you will need an Azure AD B2C tenant and a global administrator account in hello tenant.</span></span> <span data-ttu-id="402f9-124">Pokud nemáte klienta již, [Začínáme s Azure AD B2C](active-directory-b2c-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="402f9-124">If you don't have a tenant already, [get started with Azure AD B2C](active-directory-b2c-get-started.md).</span></span>

## <a name="register-your-application-in-your-tenant"></a><span data-ttu-id="402f9-125">Registrace vaší aplikace ve vašem klientovi</span><span class="sxs-lookup"><span data-stu-id="402f9-125">Register your application in your tenant</span></span>
<span data-ttu-id="402f9-126">Až budete mít klienta B2C, kterou potřebujete tooregister aplikace prostřednictvím hello [portálu Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="402f9-126">After you have a B2C tenant, you need tooregister your application via hello [Azure Portal](https://portal.azure.com).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="402f9-127">toouse hello rozhraní Graph API s svého klienta B2C, budete potřebovat tooregister vyhrazené aplikace pomocí hello obecného *registrace aplikace* okno v hello portálu Azure, **není** Azure AD B2C  *Aplikace* okno.</span><span class="sxs-lookup"><span data-stu-id="402f9-127">toouse hello Graph API with your B2C tenant, you will need tooregister a dedicated application by using hello generic *App Registrations* blade in hello Azure Portal, **NOT** Azure AD B2C's *Applications* blade.</span></span> <span data-ttu-id="402f9-128">Nelze znovu použít už existující B2C aplikace hello, zaregistrované v Azure AD B2C hello *aplikace* okno.</span><span class="sxs-lookup"><span data-stu-id="402f9-128">You can't reuse hello already-existing B2C applications that you registered in hello Azure AD B2C's *Applications* blade.</span></span>

1. <span data-ttu-id="402f9-129">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="402f9-129">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="402f9-130">Zvolte klienta služby Azure AD B2C výběrem účtu v hello pravém horním rohu stránky hello.</span><span class="sxs-lookup"><span data-stu-id="402f9-130">Choose your Azure AD B2C tenant by selecting your account in hello top right corner of hello page.</span></span>
3. <span data-ttu-id="402f9-131">V levém navigačním podokně hello zvolte **více služeb**, klikněte na tlačítko **registrace aplikace**a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="402f9-131">In hello left-hand navigation pane, choose **More Services**, click **App Registrations**, and click **Add**.</span></span>
4. <span data-ttu-id="402f9-132">Postupujte podle pokynů hello a vytvořte novou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="402f9-132">Follow hello prompts and create a new application.</span></span> 
    1. <span data-ttu-id="402f9-133">Vyberte **webovou aplikaci nebo API** jako typ aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="402f9-133">Select **Web App / API** as hello Application Type.</span></span>    
    2. <span data-ttu-id="402f9-134">Zadejte **žádný identifikátor URI přesměrování** (např. https://B2CGraphAPI) jako není relevantní pro tento příklad.</span><span class="sxs-lookup"><span data-stu-id="402f9-134">Provide **any redirect URI** (e.g. https://B2CGraphAPI) as it's not relevant for this example.</span></span>  
5. <span data-ttu-id="402f9-135">Hello aplikace se teď zobrazí v hello seznam aplikací, klikněte na jeho tooobtain hello **ID aplikace** (také označované jako ID klienta).</span><span class="sxs-lookup"><span data-stu-id="402f9-135">hello application will now show up in hello list of applications, click on it tooobtain hello **Application ID** (also known as Client ID).</span></span> <span data-ttu-id="402f9-136">Zkopírujte jej, protože ho budete potřebovat v další části.</span><span class="sxs-lookup"><span data-stu-id="402f9-136">Copy it as you'll need it in a later section.</span></span>
6. <span data-ttu-id="402f9-137">V okně Nastavení hello, klikněte na **klíče** a přidejte nový klíč (také označovaný jako sdílený tajný klíč klienta).</span><span class="sxs-lookup"><span data-stu-id="402f9-137">In hello Settings blade, click on **Keys** and add a new key (also known as client secret).</span></span> <span data-ttu-id="402f9-138">Také zkopírujte jej pro použití v další části.</span><span class="sxs-lookup"><span data-stu-id="402f9-138">Also copy it for use in a later section.</span></span>

## <a name="configure-create-read-and-update-permissions-for-your-application"></a><span data-ttu-id="402f9-139">Konfigurace vytvářet, číst a aktualizovat oprávnění pro vaši aplikaci</span><span class="sxs-lookup"><span data-stu-id="402f9-139">Configure create, read and update permissions for your application</span></span>
<span data-ttu-id="402f9-140">Nyní je třeba tooconfigure tooget vaší aplikace, které všechny hello požadované oprávnění toocreate, číst, aktualizovat a odstranit uživatele.</span><span class="sxs-lookup"><span data-stu-id="402f9-140">Now you need tooconfigure your application tooget all hello required permissions toocreate, read, update and delete users.</span></span>

1. <span data-ttu-id="402f9-141">Pokračování v hello okno registrace aplikace portálu Azure, vyberte svou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="402f9-141">Continuing in hello Azure portal's App Registrations blade, select your application.</span></span>
2. <span data-ttu-id="402f9-142">V okně Nastavení hello, klikněte na **požadovaná oprávnění**.</span><span class="sxs-lookup"><span data-stu-id="402f9-142">In hello Settings blade, click on **Required permissions**.</span></span>
3. <span data-ttu-id="402f9-143">V okně hello potřebná oprávnění, klikněte na **Windows Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="402f9-143">In hello Required permissions blade, click on **Windows Azure Active Directory**.</span></span>
4. <span data-ttu-id="402f9-144">V okně hello povolit přístup, vyberte hello **pro čtení a zápis dat adresáře** oprávnění z **oprávnění aplikací** a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="402f9-144">In hello Enable Access  blade, select hello **Read and write directory data** permission from **Application Permissions** and click **Save**.</span></span>
5. <span data-ttu-id="402f9-145">Nakonec zpět v okně hello potřebná oprávnění, klikněte na hello **udělit oprávnění** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="402f9-145">Finally, back in hello Required permissions blade, click on hello **Grant Permissions** button.</span></span>

<span data-ttu-id="402f9-146">Teď máte aplikaci, která má oprávnění toocreate, čtení a aktualizaci uživatelé z vašeho klienta B2C.</span><span class="sxs-lookup"><span data-stu-id="402f9-146">You now have an application that has permission toocreate, read and update users from your B2C tenant.</span></span>

## <a name="configure-delete-permissions-for-your-application"></a><span data-ttu-id="402f9-147">Konfigurace odstranění oprávnění pro vaši aplikaci</span><span class="sxs-lookup"><span data-stu-id="402f9-147">Configure delete permissions for your application</span></span>
<span data-ttu-id="402f9-148">V současné době hello *pro čtení a zápis dat adresáře* nemá oprávnění **není** zahrnují možnost toodo hello žádné odstranění například odstraňování uživatelů.</span><span class="sxs-lookup"><span data-stu-id="402f9-148">Currently, hello *Read and write directory data* permission does **NOT** include hello ability toodo any deletions such as deleting users.</span></span> <span data-ttu-id="402f9-149">Pokud chcete uživatelům aplikace hello možnost toodelete toogive, budete potřebovat toodo tyto další kroky, které se týkají prostředí PowerShell, jinak, můžete přeskočit toohello další části.</span><span class="sxs-lookup"><span data-stu-id="402f9-149">If you want toogive your application hello ability toodelete users, you'll need toodo these extra steps that involve PowerShell, otherwise, you can skip toohello next section.</span></span>

<span data-ttu-id="402f9-150">Nejprve stáhnout a nainstalovat hello [Microsoft Online Services Sign-In Assistant](http://go.microsoft.com/fwlink/?LinkID=286152).</span><span class="sxs-lookup"><span data-stu-id="402f9-150">First, download and install hello [Microsoft Online Services Sign-In Assistant](http://go.microsoft.com/fwlink/?LinkID=286152).</span></span> <span data-ttu-id="402f9-151">Potom stáhněte a nainstalujte hello [64-bit modulu Azure Active Directory pro prostředí Windows PowerShell](http://go.microsoft.com/fwlink/p/?linkid=236297).</span><span class="sxs-lookup"><span data-stu-id="402f9-151">Then download and install hello [64-bit Azure Active Directory module for Windows PowerShell](http://go.microsoft.com/fwlink/p/?linkid=236297).</span></span>

<span data-ttu-id="402f9-152">Po instalaci modulu PowerShell text hello, otevřete prostředí PowerShell a připojte tooyour klienta B2C.</span><span class="sxs-lookup"><span data-stu-id="402f9-152">After you install hello PowerShell module, open PowerShell and connect tooyour B2C tenant.</span></span> <span data-ttu-id="402f9-153">Po spuštění `Get-Credential`, zobrazí se výzva pro uživatelské jméno a heslo, zadejte hello uživatelské jméno a heslo účtu správce klienta B2C.</span><span class="sxs-lookup"><span data-stu-id="402f9-153">After you run `Get-Credential`, you will be prompted for a user name and password, Enter hello user name and password of your B2C tenant administrator account.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="402f9-154">Je třeba toouse B2C účet správce klienta, který je **místní** toohello B2C klienta.</span><span class="sxs-lookup"><span data-stu-id="402f9-154">You need toouse a B2C tenant administrator account that is **local** toohello B2C tenant.</span></span> <span data-ttu-id="402f9-155">Tyto účty vypadat takto: myusername@myb2ctenant.onmicrosoft.com.</span><span class="sxs-lookup"><span data-stu-id="402f9-155">These accounts look like this: myusername@myb2ctenant.onmicrosoft.com.</span></span>

```powershell
Connect-MsolService
```

<span data-ttu-id="402f9-156">Nyní použijeme hello **ID aplikace** ve skriptu hello níže tooassign hello aplikace hello uživatelské účet správce role, která umožní uživatelům toodelete.</span><span class="sxs-lookup"><span data-stu-id="402f9-156">Now we'll use hello **Application ID** in hello script below tooassign hello application hello user account administrator role which will allow it toodelete users.</span></span> <span data-ttu-id="402f9-157">Tyto role mají dobře známé identifikátory, takže potřebujete toodo není vstupní vaše **ID aplikace** ve skriptu hello níže.</span><span class="sxs-lookup"><span data-stu-id="402f9-157">These roles have well-known identifiers, so all you need toodo is input your **Application ID** in hello script below.</span></span>

```powershell
$applicationId = "<YOUR_APPLICATION_ID>"
$sp = Get-MsolServicePrincipal -AppPrincipalId $applicationId
Add-MsolRoleMember -RoleObjectId fe930be7-5e62-47db-91af-98c3a49a38b1 -RoleMemberObjectId $sp.ObjectId -RoleMemberType servicePrincipal
```

<span data-ttu-id="402f9-158">Aplikaci teď má také oprávnění toodelete uživatelů z vašeho klienta B2C.</span><span class="sxs-lookup"><span data-stu-id="402f9-158">Your application now also has permissions toodelete users from your B2C tenant.</span></span>

## <a name="download-configure-and-build-hello-sample-code"></a><span data-ttu-id="402f9-159">Stažení, konfigurace a sestavení hello ukázkový kód</span><span class="sxs-lookup"><span data-stu-id="402f9-159">Download, configure, and build hello sample code</span></span>
<span data-ttu-id="402f9-160">Nejprve stáhnout ukázkový kód hello a získat běh aplikace.</span><span class="sxs-lookup"><span data-stu-id="402f9-160">First, download hello sample code and get it running.</span></span> <span data-ttu-id="402f9-161">Potom jsme bude trvat bližší pohled na ho.</span><span class="sxs-lookup"><span data-stu-id="402f9-161">Then we will take a closer look at it.</span></span>  <span data-ttu-id="402f9-162">Můžete [stáhnout hello ukázkový kód v souboru ZIP](https://github.com/AzureADQuickStarts/B2C-GraphAPI-DotNet/archive/master.zip).</span><span class="sxs-lookup"><span data-stu-id="402f9-162">You can [download hello sample code as a .zip file](https://github.com/AzureADQuickStarts/B2C-GraphAPI-DotNet/archive/master.zip).</span></span> <span data-ttu-id="402f9-163">Můžete ho také klonovat do adresáře podle vašeho výběru:</span><span class="sxs-lookup"><span data-stu-id="402f9-163">You can also clone it into a directory of your choice:</span></span>

```
git clone https://github.com/AzureADQuickStarts/B2C-GraphAPI-DotNet.git
```

<span data-ttu-id="402f9-164">Otevřete hello `B2CGraphClient\B2CGraphClient.sln` řešení sady Visual Studio v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="402f9-164">Open hello `B2CGraphClient\B2CGraphClient.sln` Visual Studio solution in Visual Studio.</span></span> <span data-ttu-id="402f9-165">V hello `B2CGraphClient` projekt, otevřete hello soubor `App.config`.</span><span class="sxs-lookup"><span data-stu-id="402f9-165">In hello `B2CGraphClient` project, open hello file `App.config`.</span></span> <span data-ttu-id="402f9-166">Tři nastavení aplikace hello nahradíte vlastními hodnotami:</span><span class="sxs-lookup"><span data-stu-id="402f9-166">Replace hello three app settings with your own values:</span></span>

```
<appSettings>
    <add key="b2c:Tenant" value="{Your Tenant Name}" />
    <add key="b2c:ClientId" value="{hello ApplicationID from above}" />
    <add key="b2c:ClientSecret" value="{hello Key from above}" />
</appSettings>
```

[!INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

<span data-ttu-id="402f9-167">Pak klikněte pravým tlačítkem na hello `B2CGraphClient` řešení a znovu sestavit ukázku, hello.</span><span class="sxs-lookup"><span data-stu-id="402f9-167">Next, right-click on hello `B2CGraphClient` solution and rebuild hello sample.</span></span> <span data-ttu-id="402f9-168">Pokud jste úspěšné, teď byste měli mít `B2C.exe` spustitelný soubor umístěný ve `B2CGraphClient\bin\Debug`.</span><span class="sxs-lookup"><span data-stu-id="402f9-168">If you are successful, you should now have a `B2C.exe` executable file located in `B2CGraphClient\bin\Debug`.</span></span>

## <a name="build-user-crud-operations-by-using-hello-graph-api"></a><span data-ttu-id="402f9-169">Vytvoření operace CRUD uživatele pomocí rozhraní Graph API hello</span><span class="sxs-lookup"><span data-stu-id="402f9-169">Build user CRUD operations by using hello Graph API</span></span>
<span data-ttu-id="402f9-170">Otevřete toouse hello B2CGraphClient, `cmd` Windows příkazového řádku a změňte váš adresář toohello `Debug` adresáře.</span><span class="sxs-lookup"><span data-stu-id="402f9-170">toouse hello B2CGraphClient, open a `cmd` Windows command prompt and change your directory toohello `Debug` directory.</span></span> <span data-ttu-id="402f9-171">Spusťte hello `B2C Help` příkaz.</span><span class="sxs-lookup"><span data-stu-id="402f9-171">Then run hello `B2C Help` command.</span></span>

```
> cd B2CGraphClient\bin\Debug
> B2C Help
```

<span data-ttu-id="402f9-172">Tato akce zobrazí stručný popis každého příkazu.</span><span class="sxs-lookup"><span data-stu-id="402f9-172">This will display a brief description of each command.</span></span> <span data-ttu-id="402f9-173">Pokaždé, když vyvolat jeden z těchto příkazů `B2CGraphClient` provede požadavek toohello Azure AD Graph API.</span><span class="sxs-lookup"><span data-stu-id="402f9-173">Each time you invoke one of these commands, `B2CGraphClient` makes a request toohello Azure AD Graph API.</span></span>

### <a name="get-an-access-token"></a><span data-ttu-id="402f9-174">Získání přístupového tokenu</span><span class="sxs-lookup"><span data-stu-id="402f9-174">Get an access token</span></span>
<span data-ttu-id="402f9-175">Všechny žádosti o toohello rozhraní Graph API vyžaduje token přístupu pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="402f9-175">Any request toohello Graph API requires an access token for authentication.</span></span> <span data-ttu-id="402f9-176">`B2CGraphClient`používá hello open-source Active Directory Authentication Library (ADAL) toohelp získat přístupové tokeny.</span><span class="sxs-lookup"><span data-stu-id="402f9-176">`B2CGraphClient` uses hello open-source Active Directory Authentication Library (ADAL) toohelp acquire access tokens.</span></span> <span data-ttu-id="402f9-177">ADAL usnadňuje získávání tokenu poskytuje jednoduché rozhraní API a postará o některé důležité podrobnosti, jako je například ukládání do mezipaměti přístupových tokenů.</span><span class="sxs-lookup"><span data-stu-id="402f9-177">ADAL makes token acquisition easier by providing a simple API and taking care of some important details, such as caching access tokens.</span></span> <span data-ttu-id="402f9-178">Toouse ADAL tooget tokeny, ale nemáte.</span><span class="sxs-lookup"><span data-stu-id="402f9-178">You don't have toouse ADAL tooget tokens, though.</span></span> <span data-ttu-id="402f9-179">Můžete také získat tokeny tím, že vytvoří požadavky HTTP.</span><span class="sxs-lookup"><span data-stu-id="402f9-179">You can also get tokens by crafting HTTP requests.</span></span>

> [!NOTE]
> <span data-ttu-id="402f9-180">Tento příklad používá ADAL v2 v pořadí toocommunicate s hello rozhraní Graph API.</span><span class="sxs-lookup"><span data-stu-id="402f9-180">This code sample uses ADAL v2 in order toocommunicate with hello Graph API.</span></span>  <span data-ttu-id="402f9-181">ADAL verze 2 nebo 3 je nutné použít v pořadí tooget přístupové tokeny, které se dají použít s hello Azure AD Graph API.</span><span class="sxs-lookup"><span data-stu-id="402f9-181">You must use ADAL v2 or v3 in order tooget access tokens which can be used with hello Azure AD Graph API.</span></span>
> 
> 

<span data-ttu-id="402f9-182">Když `B2CGraphClient` spustí, vytvoří instanci hello `B2CGraphClient` třídy.</span><span class="sxs-lookup"><span data-stu-id="402f9-182">When `B2CGraphClient` runs, it creates an instance of hello `B2CGraphClient` class.</span></span> <span data-ttu-id="402f9-183">Nastaví generování uživatelského rozhraní ověřování pomocí knihovny ADAL Hello konstruktoru pro tuto třídu:</span><span class="sxs-lookup"><span data-stu-id="402f9-183">hello constructor for this class sets up an ADAL authentication scaffolding:</span></span>

```C#
public B2CGraphClient(string clientId, string clientSecret, string tenant)
{
    // hello client_id, client_secret, and tenant are provided in Program.cs, which pulls hello values from App.config
    this.clientId = clientId;
    this.clientSecret = clientSecret;
    this.tenant = tenant;

    // hello AuthenticationContext is ADAL's primary class, in which you indicate hello tenant toouse.
    this.authContext = new AuthenticationContext("https://login.microsoftonline.com/" + tenant);

    // hello ClientCredential is where you pass in your client_id and client_secret, which are
    // provided tooAzure AD in order tooreceive an access_token by using hello app's identity.
    this.credential = new ClientCredential(clientId, clientSecret);
}
```

<span data-ttu-id="402f9-184">Použijeme hello `B2C Get-User` příkaz jako příklad.</span><span class="sxs-lookup"><span data-stu-id="402f9-184">We'll use hello `B2C Get-User` command as an example.</span></span> <span data-ttu-id="402f9-185">Když `B2C Get-User` je volána bez jakékoli další vstupy hello hello volání rozhraní příkazového řádku `B2CGraphClient.GetAllUsers(...)` metoda.</span><span class="sxs-lookup"><span data-stu-id="402f9-185">When `B2C Get-User` is invoked without any additional inputs, hello CLI calls hello `B2CGraphClient.GetAllUsers(...)` method.</span></span> <span data-ttu-id="402f9-186">Tato metoda volá `B2CGraphClient.SendGraphGetRequest(...)`, který odešle metody GET protokolu HTTP žádosti toohello rozhraní Graph API.</span><span class="sxs-lookup"><span data-stu-id="402f9-186">This method calls `B2CGraphClient.SendGraphGetRequest(...)`, which submits an HTTP GET request toohello Graph API.</span></span> <span data-ttu-id="402f9-187">Před `B2CGraphClient.SendGraphGetRequest(...)` zasílá hello požadavek GET, nejdřív získá přístup tokenu pomocí ADAL:</span><span class="sxs-lookup"><span data-stu-id="402f9-187">Before `B2CGraphClient.SendGraphGetRequest(...)` sends hello GET request, it first gets an access token by using ADAL:</span></span>

```C#
public async Task<string> SendGraphGetRequest(string api, string query)
{
    // First, use ADAL tooacquire a token by using hello app's identity (hello credential)
    // hello first parameter is hello resource we want an access_token for; in this case, hello Graph API.
    AuthenticationResult result = authContext.AcquireToken("https://graph.windows.net", credential);

    ...

```

<span data-ttu-id="402f9-188">Můžete získat přístup tokenu pro rozhraní Graph API hello podle hello volání ADAL `AuthenticationContext.AcquireToken(...)` metoda.</span><span class="sxs-lookup"><span data-stu-id="402f9-188">You can get an access token for hello Graph API by calling hello ADAL `AuthenticationContext.AcquireToken(...)` method.</span></span> <span data-ttu-id="402f9-189">Vrátí ADAL `access_token` představující identitu aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="402f9-189">ADAL then returns an `access_token` that represents hello application's identity.</span></span>

### <a name="read-users"></a><span data-ttu-id="402f9-190">Uživatelé pro čtení</span><span class="sxs-lookup"><span data-stu-id="402f9-190">Read users</span></span>
<span data-ttu-id="402f9-191">Pokud chcete tooget seznam uživatelů, nebo určitého uživatele sám hello rozhraní Graph API, můžete odeslat HTTP `GET` požadavku toohello `/users` koncový bod.</span><span class="sxs-lookup"><span data-stu-id="402f9-191">When you want tooget a list of users or get a particular user from hello Graph API, you can send an HTTP `GET` request toohello `/users` endpoint.</span></span> <span data-ttu-id="402f9-192">Žádost o pro všechny uživatele hello v klienta vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="402f9-192">A request for all of hello users in a tenant looks like this:</span></span>

```
GET https://graph.windows.net/contosob2c.onmicrosoft.com/users?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
```

<span data-ttu-id="402f9-193">toosee tuto žádost, spusťte:</span><span class="sxs-lookup"><span data-stu-id="402f9-193">toosee this request, run:</span></span>

 ```
 > B2C Get-User
 ```

<span data-ttu-id="402f9-194">Existují dvě toonote důležité věci:</span><span class="sxs-lookup"><span data-stu-id="402f9-194">There are two important things toonote:</span></span>

* <span data-ttu-id="402f9-195">Hello získaných prostřednictvím ADAL přístupový token se přidá toohello `Authorization` záhlaví s použitím hello `Bearer` schéma.</span><span class="sxs-lookup"><span data-stu-id="402f9-195">hello access token acquired via ADAL is added toohello `Authorization` header by using hello `Bearer` scheme.</span></span>
* <span data-ttu-id="402f9-196">Pro B2C klientů, je nutné použít parametr dotazu hello `api-version=1.6`.</span><span class="sxs-lookup"><span data-stu-id="402f9-196">For B2C tenants, you must use hello query parameter `api-version=1.6`.</span></span>

<span data-ttu-id="402f9-197">Obě tyto podrobnosti jsou zpracovávány v hello `B2CGraphClient.SendGraphGetRequest(...)` metoda:</span><span class="sxs-lookup"><span data-stu-id="402f9-197">Both of these details are handled in hello `B2CGraphClient.SendGraphGetRequest(...)` method:</span></span>

```C#
public async Task<string> SendGraphGetRequest(string api, string query)
{
    ...

    // For B2C user management, be sure toouse hello 1.6 Graph API version.
    HttpClient http = new HttpClient();
    string url = "https://graph.windows.net/" + tenant + api + "?" + "api-version=1.6";
    if (!string.IsNullOrEmpty(query))
    {
        url += "&" + query;
    }

    // Append hello access token for hello Graph API toohello Authorization header of hello request by using hello Bearer scheme.
    HttpRequestMessage request = new HttpRequestMessage(HttpMethod.Get, url);
    request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);
    HttpResponseMessage response = await http.SendAsync(request);

    ...
```

### <a name="create-consumer-user-accounts"></a><span data-ttu-id="402f9-198">Vytvoření příjemce uživatelské účty</span><span class="sxs-lookup"><span data-stu-id="402f9-198">Create consumer user accounts</span></span>
<span data-ttu-id="402f9-199">Při vytváření uživatelských účtů v svého klienta B2C, můžete odeslat HTTP `POST` požadavku toohello `/users` koncový bod:</span><span class="sxs-lookup"><span data-stu-id="402f9-199">When you create user accounts in your B2C tenant, you can send an HTTP `POST` request toohello `/users` endpoint:</span></span>

```
POST https://graph.windows.net/contosob2c.onmicrosoft.com/users?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
Content-Type: application/json
Content-Length: 338

{
    // All of these properties are required toocreate consumer users.

    "accountEnabled": true,
    "signInNames": [                            // controls which identifier hello user uses toosign in toohello account
        {
            "type": "emailAddress",             // can be 'emailAddress' or 'userName'
            "value": "joeconsumer@gmail.com"
        }
    ],
    "creationType": "LocalAccount",            // always set too'LocalAccount'
    "displayName": "Joe Consumer",                // a value that can be used for displaying toohello end user
    "mailNickname": "joec",                        // an email alias for hello user
    "passwordProfile": {
        "password": "P@ssword!",
        "forceChangePasswordNextLogin": false   // always set toofalse
    },
    "passwordPolicies": "DisablePasswordExpiration"
}
```

<span data-ttu-id="402f9-200">Většina těchto vlastností v této žádosti o jsou požadované toocreate spotřebitelské uživatele.</span><span class="sxs-lookup"><span data-stu-id="402f9-200">Most of these properties in this request are required toocreate consumer users.</span></span> <span data-ttu-id="402f9-201">Další, klikněte na tlačítko toolearn [zde](https://msdn.microsoft.com/library/azure/ad/graph/api/users-operations#CreateLocalAccountUser).</span><span class="sxs-lookup"><span data-stu-id="402f9-201">toolearn more, click [here](https://msdn.microsoft.com/library/azure/ad/graph/api/users-operations#CreateLocalAccountUser).</span></span> <span data-ttu-id="402f9-202">Všimněte si, že hello `//` komentáře byly zahrnuty pro obrázek.</span><span class="sxs-lookup"><span data-stu-id="402f9-202">Note that hello `//` comments have been included for illustration.</span></span> <span data-ttu-id="402f9-203">V skutečné požadavku je nezahrnujte.</span><span class="sxs-lookup"><span data-stu-id="402f9-203">Do not include them in an actual request.</span></span>

<span data-ttu-id="402f9-204">toosee hello požadavku, spusťte jeden z hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="402f9-204">toosee hello request, run one of hello following commands:</span></span>

```
> B2C Create-User ..\..\..\usertemplate-email.json
> B2C Create-User ..\..\..\usertemplate-username.json
```

<span data-ttu-id="402f9-205">Hello `Create-User` příkaz má soubor .json jako vstupní parametr.</span><span class="sxs-lookup"><span data-stu-id="402f9-205">hello `Create-User` command takes a .json file as an input parameter.</span></span> <span data-ttu-id="402f9-206">Tato položka obsahuje reprezentaci JSON objektu uživatele.</span><span class="sxs-lookup"><span data-stu-id="402f9-206">This contains a JSON representation of a user object.</span></span> <span data-ttu-id="402f9-207">Existují dva ukázkové soubory .json v ukázkovém kódu hello: `usertemplate-email.json` a `usertemplate-username.json`.</span><span class="sxs-lookup"><span data-stu-id="402f9-207">There are two sample .json files in hello sample code: `usertemplate-email.json` and `usertemplate-username.json`.</span></span> <span data-ttu-id="402f9-208">Můžete upravit tyto soubory toosuit vašim potřebám.</span><span class="sxs-lookup"><span data-stu-id="402f9-208">You can modify these files toosuit your needs.</span></span> <span data-ttu-id="402f9-209">Kromě toho toohello požadovaná pole výše, několik volitelná pole, které můžete použít jsou zahrnuty v těchto souborech.</span><span class="sxs-lookup"><span data-stu-id="402f9-209">In addition toohello required fields above, several optional fields that you can use are included in these files.</span></span> <span data-ttu-id="402f9-210">Informace o volitelných polí hello lze nalézt v hello [odkaz na Azure AD Graph API entity](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity).</span><span class="sxs-lookup"><span data-stu-id="402f9-210">Details on hello optional fields can be found in hello [Azure AD Graph API entity reference](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity).</span></span>

<span data-ttu-id="402f9-211">Můžete vidět, jak je požadavek POST hello vytvořený v `B2CGraphClient.SendGraphPostRequest(...)`.</span><span class="sxs-lookup"><span data-stu-id="402f9-211">You can see how hello POST request is constructed in `B2CGraphClient.SendGraphPostRequest(...)`.</span></span>

* <span data-ttu-id="402f9-212">Připojí toohello tokenu přístupu `Authorization` hlavičky požadavku hello.</span><span class="sxs-lookup"><span data-stu-id="402f9-212">It attaches an access token toohello `Authorization` header of hello request.</span></span>
* <span data-ttu-id="402f9-213">Nastaví `api-version=1.6`.</span><span class="sxs-lookup"><span data-stu-id="402f9-213">It sets `api-version=1.6`.</span></span>
* <span data-ttu-id="402f9-214">Obsahuje objekt uživatele hello JSON v textu hello hello požadavku.</span><span class="sxs-lookup"><span data-stu-id="402f9-214">It includes hello JSON user object in hello body of hello request.</span></span>

> [!NOTE]
> <span data-ttu-id="402f9-215">Pokud hello účty, které chcete toomigrate z existující úložiště uživatele má nižší síly hesla než hello [silné heslo sílu vynucováno Azure AD B2C](https://msdn.microsoft.com/library/azure/jj943764.aspx), můžete zakázat pomocí hello požadaveknasilnéheslohello`DisableStrongPassword`hodnota v hello `passwordPolicies` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="402f9-215">If hello accounts that you want toomigrate from an existing user store has lower password strength than hello [strong password strength enforced by Azure AD B2C](https://msdn.microsoft.com/library/azure/jj943764.aspx), you can disable hello strong password requirement using hello `DisableStrongPassword` value in hello `passwordPolicies` property.</span></span> <span data-ttu-id="402f9-216">Například můžete upravit hello vytvořit požadavek uživatele výše uvedeného následujícím způsobem: `"passwordPolicies": "DisablePasswordExpiration, DisableStrongPassword"`.</span><span class="sxs-lookup"><span data-stu-id="402f9-216">For instance, you can modify hello create user request provided above as follows: `"passwordPolicies": "DisablePasswordExpiration, DisableStrongPassword"`.</span></span>
> 
> 

### <a name="update-consumer-user-accounts"></a><span data-ttu-id="402f9-217">Aktualizovat příjemce uživatelské účty</span><span class="sxs-lookup"><span data-stu-id="402f9-217">Update consumer user accounts</span></span>
<span data-ttu-id="402f9-218">Při aktualizaci uživatelských objektů hello proces je podobný toohello jeden že použijete toocreate uživatelské objekty.</span><span class="sxs-lookup"><span data-stu-id="402f9-218">When you update user objects, hello process is similar toohello one you use toocreate user objects.</span></span> <span data-ttu-id="402f9-219">Tento proces používá hello HTTP, ale `PATCH` metoda:</span><span class="sxs-lookup"><span data-stu-id="402f9-219">But this process uses hello HTTP `PATCH` method:</span></span>

```
PATCH https://graph.windows.net/contosob2c.onmicrosoft.com/users/<user-object-id>?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
Content-Type: application/json
Content-Length: 37

{
    "displayName": "Joe Consumer",                // this request updates only hello user's displayName
}
```

<span data-ttu-id="402f9-220">Zkuste tooupdate uživatele tím, že aktualizuje vaše soubory JSON se nová data.</span><span class="sxs-lookup"><span data-stu-id="402f9-220">Try tooupdate a user by updating your JSON files with new data.</span></span> <span data-ttu-id="402f9-221">Pak můžete použít `B2CGraphClient` toorun jednu z těchto příkazů:</span><span class="sxs-lookup"><span data-stu-id="402f9-221">You can then use `B2CGraphClient` toorun one of these commands:</span></span>

```
> B2C Update-User <user-object-id> ..\..\..\usertemplate-email.json
> B2C Update-User <user-object-id> ..\..\..\usertemplate-username.json
```

<span data-ttu-id="402f9-222">Zkontrolujte hello `B2CGraphClient.SendGraphPatchRequest(...)` metoda podrobnosti o tom toosend tuto žádost.</span><span class="sxs-lookup"><span data-stu-id="402f9-222">Inspect hello `B2CGraphClient.SendGraphPatchRequest(...)` method for details on how toosend this request.</span></span>

### <a name="search-users"></a><span data-ttu-id="402f9-223">Vyhledávat uživatele</span><span class="sxs-lookup"><span data-stu-id="402f9-223">Search users</span></span>
<span data-ttu-id="402f9-224">Můžete hledat uživatele v svého klienta B2C v několika způsoby.</span><span class="sxs-lookup"><span data-stu-id="402f9-224">You can search for users in your B2C tenant in a couple of ways.</span></span> <span data-ttu-id="402f9-225">Jeden pomocí ID objektu nebo dva pomocí hello uživatele přihlašovací identifikátor uživatele hello (tj, hello `signInNames` vlastnost).</span><span class="sxs-lookup"><span data-stu-id="402f9-225">One, using hello user's object ID or two, using hello user's sign-in identifer (i.e., hello `signInNames` property).</span></span>

<span data-ttu-id="402f9-226">Spusťte jeden z následujících příkazů toosearch pro konkrétního uživatele hello:</span><span class="sxs-lookup"><span data-stu-id="402f9-226">Run one of hello following commands toosearch for a specific user:</span></span>

```
> B2C Get-User <user-object-id>
> B2C Get-User <filter-query-expression>
```

<span data-ttu-id="402f9-227">Tady je několik příkladů:</span><span class="sxs-lookup"><span data-stu-id="402f9-227">Here are a couple of examples:</span></span>

```
> B2C Get-User 2bcf1067-90b6-4253-9991-7f16449c2d91
> B2C Get-User $filter=signInNames/any(x:x/value%20eq%20%27joeconsumer@gmail.com%27)
```

### <a name="delete-users"></a><span data-ttu-id="402f9-228">Odstranit uživatele</span><span class="sxs-lookup"><span data-stu-id="402f9-228">Delete users</span></span>
<span data-ttu-id="402f9-229">Hello proces odstranění uživatele je jednoduchý.</span><span class="sxs-lookup"><span data-stu-id="402f9-229">hello process for deleting a user is straightforward.</span></span> <span data-ttu-id="402f9-230">Použití hello HTTP `DELETE` metoda konstrukce hello adresy URL a s hello Opravte ID objektu:</span><span class="sxs-lookup"><span data-stu-id="402f9-230">Use hello HTTP `DELETE` method and construct hello URL with hello correct object ID:</span></span>

```
DELETE https://graph.windows.net/contosob2c.onmicrosoft.com/users/<user-object-id>?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
```

<span data-ttu-id="402f9-231">toosee příklad, zadejte tento příkaz a zobrazení hello odstranit požadavek, který je konzoly na tištěných toohello:</span><span class="sxs-lookup"><span data-stu-id="402f9-231">toosee an example, enter this command and view hello delete request that is printed toohello console:</span></span>

```
> B2C Delete-User <object-id-of-user>
```

<span data-ttu-id="402f9-232">Zkontrolujte hello `B2CGraphClient.SendGraphDeleteRequest(...)` metoda podrobnosti o tom toosend tuto žádost.</span><span class="sxs-lookup"><span data-stu-id="402f9-232">Inspect hello `B2CGraphClient.SendGraphDeleteRequest(...)` method for details on how toosend this request.</span></span>

<span data-ttu-id="402f9-233">Můžete provádět mnoho dalších akcí s hello Azure AD Graph API v přidání toouser správy.</span><span class="sxs-lookup"><span data-stu-id="402f9-233">You can perform many other actions with hello Azure AD Graph API in addition toouser management.</span></span> <span data-ttu-id="402f9-234">[Referenční dokumentace rozhraní API Azure AD Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) poskytuje podrobnosti pro každou akci, společně s požadavky na ukázky.</span><span class="sxs-lookup"><span data-stu-id="402f9-234">The [Azure AD Graph API reference](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) provides details on each action, along with sample requests.</span></span>

## <a name="use-custom-attributes"></a><span data-ttu-id="402f9-235">Použití vlastních atributů</span><span class="sxs-lookup"><span data-stu-id="402f9-235">Use custom attributes</span></span>
<span data-ttu-id="402f9-236">Většina aplikací příjemce potřebovat toostore nějaký typ informace vlastního uživatelského profilu.</span><span class="sxs-lookup"><span data-stu-id="402f9-236">Most consumer applications need toostore some type of custom user profile information.</span></span> <span data-ttu-id="402f9-237">Jedním ze způsobů, můžete k tomu je toodefine vlastní atribut v svého klienta B2C.</span><span class="sxs-lookup"><span data-stu-id="402f9-237">One way you can do this is toodefine a custom attribute in your B2C tenant.</span></span> <span data-ttu-id="402f9-238">Poté můžete ke tento atribut hello stejným způsobem jako považovat všechny ostatní vlastnosti v objektu user.</span><span class="sxs-lookup"><span data-stu-id="402f9-238">You can then treat that attribute hello same way you treat any other property on a user object.</span></span> <span data-ttu-id="402f9-239">Je možné aktualizovat atribut hello odstranění hello atribut dotazu hello atribut odesílat hello atribut jako deklaraci v tokeny přihlášení a další.</span><span class="sxs-lookup"><span data-stu-id="402f9-239">You can update hello attribute, delete hello attribute, query by hello attribute, send hello attribute as a claim in sign-in tokens, and more.</span></span>

<span data-ttu-id="402f9-240">toodefine vlastní atribut v svého klienta B2C, najdete v části hello [odkaz vlastní atribut B2C](active-directory-b2c-reference-custom-attr.md).</span><span class="sxs-lookup"><span data-stu-id="402f9-240">toodefine a custom attribute in your B2C tenant, see hello [B2C custom attribute reference](active-directory-b2c-reference-custom-attr.md).</span></span>

<span data-ttu-id="402f9-241">Můžete zobrazit hello vlastní atributy definované ve vašem klientovi B2C pomocí `B2CGraphClient`:</span><span class="sxs-lookup"><span data-stu-id="402f9-241">You can view hello custom attributes defined in your B2C tenant by using `B2CGraphClient`:</span></span>

```
> B2C Get-B2C-Application
> B2C Get-Extension-Attribute <object-id-in-the-output-of-the-above-command>
```

<span data-ttu-id="402f9-242">výstup Hello tyto funkce zjistí hello podrobnosti o jednotlivých vlastních atributů, jako například:</span><span class="sxs-lookup"><span data-stu-id="402f9-242">hello output of these functions reveals hello details of each custom attribute, such as:</span></span>

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

<span data-ttu-id="402f9-243">Můžete použít hello celý název, například `extension_55dc0861f9a44eb999e0a8a872204adb_Jersey_Number`, jako vlastnost na uživatelské objekty.</span><span class="sxs-lookup"><span data-stu-id="402f9-243">You can use hello full name, such as `extension_55dc0861f9a44eb999e0a8a872204adb_Jersey_Number`, as a property on your user objects.</span></span>  <span data-ttu-id="402f9-244">Aktualizujte si soubor .json hello nové vlastnosti a hodnotu pro vlastnost hello a pak spusťte:</span><span class="sxs-lookup"><span data-stu-id="402f9-244">Update your .json file with hello new property and a value for hello property, and then run:</span></span>

```
> B2C Update-User <object-id-of-user> <path-to-json-file>
```

<span data-ttu-id="402f9-245">Pomocí `B2CGraphClient`, máte aplikaci služby, která můžete spravovat vaše uživatele klienta B2C prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="402f9-245">By using `B2CGraphClient`, you have a service application that can manage your B2C tenant users programmatically.</span></span> <span data-ttu-id="402f9-246">`B2CGraphClient`používá vlastní aplikace identity tooauthenticate toohello Azure AD Graph API.</span><span class="sxs-lookup"><span data-stu-id="402f9-246">`B2CGraphClient` uses its own application identity tooauthenticate toohello Azure AD Graph API.</span></span> <span data-ttu-id="402f9-247">Je také získá tokeny pomocí tajný klíč klienta.</span><span class="sxs-lookup"><span data-stu-id="402f9-247">It also acquires tokens by using a client secret.</span></span> <span data-ttu-id="402f9-248">Protože tato funkce se začlenit do vaší aplikace, mějte na paměti několik klíčových bodů pro B2C aplikace:</span><span class="sxs-lookup"><span data-stu-id="402f9-248">As you incorporate this functionality into your application, remember a few key points for B2C apps:</span></span>

* <span data-ttu-id="402f9-249">Je nutné toogrant hello aplikace hello příslušná oprávnění v klientovi hello.</span><span class="sxs-lookup"><span data-stu-id="402f9-249">You need toogrant hello application hello proper permissions in hello tenant.</span></span>
* <span data-ttu-id="402f9-250">Nyní musíte toouse ADAL (ne MSAL) tooget přístupových tokenů.</span><span class="sxs-lookup"><span data-stu-id="402f9-250">For now, you need toouse ADAL (not MSAL) tooget access tokens.</span></span> <span data-ttu-id="402f9-251">(Můžete také odeslat zprávy protokolu přímo, bez použití knihovny.)</span><span class="sxs-lookup"><span data-stu-id="402f9-251">(You can also send protocol messages directly, without using a library.)</span></span>
* <span data-ttu-id="402f9-252">Když zavoláte hello rozhraní Graph API, použijte `api-version=1.6`.</span><span class="sxs-lookup"><span data-stu-id="402f9-252">When you call hello Graph API, use `api-version=1.6`.</span></span>
* <span data-ttu-id="402f9-253">Při vytváření a aktualizovat spotřebitelské uživatele, jsou povinné, několik vlastností, jak je popsáno výše.</span><span class="sxs-lookup"><span data-stu-id="402f9-253">When you create and update consumer users, a few properties are required, as described above.</span></span>

<span data-ttu-id="402f9-254">Pokud máte jakékoli dotazy nebo požadavků pro akce, které si přejete tooperform pomocí hello rozhraní Graph API na svého klienta B2C poznámky v tomto článku nebo v úložišti ukázkový kód GitHub hello soubor problém.</span><span class="sxs-lookup"><span data-stu-id="402f9-254">If you have any questions or requests for actions you would like tooperform by using hello Graph API on your B2C tenant, leave a comment on this article or file an issue in hello GitHub code sample repository.</span></span>

