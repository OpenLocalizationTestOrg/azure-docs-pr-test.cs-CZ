---
title: "aaaNode.js Příručka Začínáme | Microsoft Docs"
description: "Zjistěte, jak toocreate jednoduché Node.js webové aplikace a nasadit tooan cloudové služby Azure."
services: cloud-services
documentationcenter: nodejs
author: TomArcher
manager: routlaw
editor: 
ms.assetid: 50951a87-fed4-48e0-bcfa-453b9e50452e
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: hero-article
ms.date: 08/17/2017
ms.author: tarcher
ms.openlocfilehash: 22945bfcc1b0e5da2a2d37dc5cc86be013cc0b5c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="build-and-deploy-a-nodejs-application-tooan-azure-cloud-service"></a><span data-ttu-id="39c6f-103">Sestavení a nasazení tooan aplikace Node.js Cloudová služba Azure</span><span class="sxs-lookup"><span data-stu-id="39c6f-103">Build and deploy a Node.js application tooan Azure Cloud Service</span></span>

<span data-ttu-id="39c6f-104">Tento kurz ukazuje, jak toocreate jednoduché Node.js aplikace běžící v cloudové službě Azure.</span><span class="sxs-lookup"><span data-stu-id="39c6f-104">This tutorial shows how toocreate a simple Node.js application running in an Azure Cloud Service.</span></span> <span data-ttu-id="39c6f-105">Cloudové služby jsou hello stavebními bloky škálovatelných cloudových aplikací v Azure.</span><span class="sxs-lookup"><span data-stu-id="39c6f-105">Cloud Services are hello building blocks of scalable cloud applications in Azure.</span></span> <span data-ttu-id="39c6f-106">Umožňují hello oddělení, nezávislou správu a škálování součástí front-end a back-end vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="39c6f-106">They allow hello separation and independent management and scale-out of front-end and back-end components of your application.</span></span>  <span data-ttu-id="39c6f-107">Cloud Services poskytují výkonný vyhrazený virtuální počítač, který spolehlivě hostuje jednotlivé role.</span><span class="sxs-lookup"><span data-stu-id="39c6f-107">Cloud Services provide a robust dedicated virtual machine for hosting each role reliably.</span></span>

<span data-ttu-id="39c6f-108">Další informace o cloudové služby a jejich porovnání tooAzure weby a virtuálních počítačů najdete v tématu [porovnání webů Azure, cloudové služby a virtuální počítače].</span><span class="sxs-lookup"><span data-stu-id="39c6f-108">For more information on Cloud Services, and how they compare tooAzure Websites and Virtual machines, see [Azure Websites, Cloud Services and Virtual Machines comparison].</span></span>

> [!TIP]
> <span data-ttu-id="39c6f-109">Hledáte toobuild jednoduchý Web?</span><span class="sxs-lookup"><span data-stu-id="39c6f-109">Looking toobuild a simple website?</span></span> <span data-ttu-id="39c6f-110">Pokud váš scénář zahrnuje jen jednoduchý front-endový web, zvažte [Použití jednoduché webové aplikace].</span><span class="sxs-lookup"><span data-stu-id="39c6f-110">If your scenario involves just a simple website front-end, consider [using a lightweight web app].</span></span> <span data-ttu-id="39c6f-111">Můžete snadno upgradovat tooa cloudové služby jako vaše webová aplikace naroste a vaše požadavky se změní.</span><span class="sxs-lookup"><span data-stu-id="39c6f-111">You can easily upgrade tooa Cloud Service as your web app grows and your requirements change.</span></span>

<span data-ttu-id="39c6f-112">V následujícím kurzu si vytvoříte jednoduchou webovou aplikaci, hostovanou v rámci webové role.</span><span class="sxs-lookup"><span data-stu-id="39c6f-112">By following this tutorial, you will build a simple web application hosted inside a web role.</span></span> <span data-ttu-id="39c6f-113">Budou používat hello výpočetní emulátor tootest vaši aplikaci místně a potom ho pomocí nástroje příkazového řádku Powershellu nasadit.</span><span class="sxs-lookup"><span data-stu-id="39c6f-113">You will use hello compute emulator tootest your application locally, then deploy it using PowerShell command-line tools.</span></span>

<span data-ttu-id="39c6f-114">aplikace Hello je jednoduchou aplikaci "hello world":</span><span class="sxs-lookup"><span data-stu-id="39c6f-114">hello application is a simple "hello world" application:</span></span>

![Webový prohlížeč zobrazující webovou stránku Hello World hello][A web browser displaying hello Hello World web page]

## <a name="prerequisites"></a><span data-ttu-id="39c6f-116">Požadavky</span><span class="sxs-lookup"><span data-stu-id="39c6f-116">Prerequisites</span></span>
> [!NOTE]
> <span data-ttu-id="39c6f-117">Tento kurz používá prostředí Azure PowerShell, které vyžaduje systém Windows.</span><span class="sxs-lookup"><span data-stu-id="39c6f-117">This tutorial uses Azure PowerShell, which requires Windows.</span></span>

* <span data-ttu-id="39c6f-118">Nainstalujte a nakonfigurujte [Azure Powershell].</span><span class="sxs-lookup"><span data-stu-id="39c6f-118">Install and configure [Azure Powershell].</span></span>
* <span data-ttu-id="39c6f-119">Stáhněte a nainstalujte hello [Azure SDK pro .NET 2.7].</span><span class="sxs-lookup"><span data-stu-id="39c6f-119">Download and install hello [Azure SDK for .NET 2.7].</span></span> <span data-ttu-id="39c6f-120">Hello nainstalovat Instalační program, vyberte:</span><span class="sxs-lookup"><span data-stu-id="39c6f-120">In hello install setup, select:</span></span>
  * <span data-ttu-id="39c6f-121">MicrosoftAzureAuthoringTools</span><span class="sxs-lookup"><span data-stu-id="39c6f-121">MicrosoftAzureAuthoringTools</span></span>
  * <span data-ttu-id="39c6f-122">MicrosoftAzureComputeEmulator</span><span class="sxs-lookup"><span data-stu-id="39c6f-122">MicrosoftAzureComputeEmulator</span></span>

## <a name="create-an-azure-cloud-service-project"></a><span data-ttu-id="39c6f-123">Vytvoření projektu Azure Cloud Service</span><span class="sxs-lookup"><span data-stu-id="39c6f-123">Create an Azure Cloud Service project</span></span>
<span data-ttu-id="39c6f-124">Proveďte následující úlohy toocreate nový projekt Azure Cloud Service, společně s základní uživatelské rozhraní Node.js hello:</span><span class="sxs-lookup"><span data-stu-id="39c6f-124">Perform hello following tasks toocreate a new Azure Cloud Service project, along with basic Node.js scaffolding:</span></span>

1. <span data-ttu-id="39c6f-125">Spustit **prostředí Windows PowerShell** jako správce; z hello **nabídce Start** nebo **obrazovce Start**, vyhledejte **prostředí Windows PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="39c6f-125">Run **Windows PowerShell** as Administrator; from hello **Start Menu** or **Start Screen**, search for **Windows PowerShell**.</span></span>
2. <span data-ttu-id="39c6f-126">[Připojení Powershellu] tooyour předplatné.</span><span class="sxs-lookup"><span data-stu-id="39c6f-126">[Connect PowerShell] tooyour subscription.</span></span>
3. <span data-ttu-id="39c6f-127">Zadejte hello následující prostředí PowerShell rutinu toocreate toocreate hello projektu:</span><span class="sxs-lookup"><span data-stu-id="39c6f-127">Enter hello following PowerShell cmdlet toocreate toocreate hello project:</span></span>

        New-AzureServiceProject helloworld

    ![výsledek Hello hello New-AzureService helloworld příkazu][hello result of hello New-AzureService helloworld command]

    <span data-ttu-id="39c6f-129">Hello **New-AzureServiceProject** rutina generuje základní strukturu pro publikování tooa aplikace Node.js cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="39c6f-129">hello **New-AzureServiceProject** cmdlet generates a basic structure for publishing a Node.js application tooa Cloud Service.</span></span> <span data-ttu-id="39c6f-130">Obsahuje konfigurační soubory, které jsou nezbytné pro publikování tooAzure.</span><span class="sxs-lookup"><span data-stu-id="39c6f-130">It contains configuration files necessary for publishing tooAzure.</span></span> <span data-ttu-id="39c6f-131">Hello rutina také změní pracovní adresář toohello adresář pro službu hello.</span><span class="sxs-lookup"><span data-stu-id="39c6f-131">hello cmdlet also changes your working directory toohello directory for hello service.</span></span>

    <span data-ttu-id="39c6f-132">Hello rutina vytvoří hello následující soubory:</span><span class="sxs-lookup"><span data-stu-id="39c6f-132">hello cmdlet creates hello following files:</span></span>

   * <span data-ttu-id="39c6f-133">**ServiceConfiguration.Cloud.cscfg**, **ServiceConfiguration.Local.cscfg** a **ServiceDefinition.csdef**: soubory specifické pro Azure, které jsou zapotřebí pro publikování aplikace.</span><span class="sxs-lookup"><span data-stu-id="39c6f-133">**ServiceConfiguration.Cloud.cscfg**, **ServiceConfiguration.Local.cscfg** and **ServiceDefinition.csdef**: Azure-specific files necessary for publishing your application.</span></span> <span data-ttu-id="39c6f-134">Další informace najdete v tématu [Přehled vytváření hostované služby pro Azure].</span><span class="sxs-lookup"><span data-stu-id="39c6f-134">For more information, see [Overview of Creating a Hosted Service for Azure].</span></span>
   * <span data-ttu-id="39c6f-135">**deploymentSettings.json**: uloží místní nastavení, které jsou používány hello rutin nasazení prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="39c6f-135">**deploymentSettings.json**: Stores local settings that are used by hello Azure PowerShell deployment cmdlets.</span></span>
4. <span data-ttu-id="39c6f-136">Zadejte následující příkaz tooadd nové webové role hello:</span><span class="sxs-lookup"><span data-stu-id="39c6f-136">Enter hello following command tooadd a new web role:</span></span>

       Add-AzureNodeWebRole

   ![výstup Hello hello příkazu Add-AzureNodeWebRole][hello output of hello Add-AzureNodeWebRole command]

   <span data-ttu-id="39c6f-138">Hello **Add-AzureNodeWebRole** rutina vytvoří základní aplikaci Node.js.</span><span class="sxs-lookup"><span data-stu-id="39c6f-138">hello **Add-AzureNodeWebRole** cmdlet creates a basic Node.js application.</span></span> <span data-ttu-id="39c6f-139">Také upraví hello **.csfg** a **.csdef** soubory tooadd položky konfigurace pro novou roli hello.</span><span class="sxs-lookup"><span data-stu-id="39c6f-139">It also modifies hello **.csfg** and **.csdef** files tooadd configuration entries for hello new role.</span></span>

   > [!NOTE]
   > <span data-ttu-id="39c6f-140">Pokud nezadáte název role, použije se výchozí název.</span><span class="sxs-lookup"><span data-stu-id="39c6f-140">If you do not specify a role name, a default name is used.</span></span> <span data-ttu-id="39c6f-141">Název můžete zadat jako první parametr rutiny hello:`Add-AzureNodeWebRole MyRole`</span><span class="sxs-lookup"><span data-stu-id="39c6f-141">You can provide a name as hello first cmdlet parameter: `Add-AzureNodeWebRole MyRole`</span></span>

<span data-ttu-id="39c6f-142">aplikace Node.js Hello je definována v souboru hello **server.js**, který je umístěn v adresáři hello hello webovou roli (**WebRole1** ve výchozím nastavení).</span><span class="sxs-lookup"><span data-stu-id="39c6f-142">hello Node.js app is defined in hello file **server.js**, located in hello directory for hello web role (**WebRole1** by default).</span></span> <span data-ttu-id="39c6f-143">Tady je kód hello:</span><span class="sxs-lookup"><span data-stu-id="39c6f-143">Here is hello code:</span></span>

    var http = require('http');
    var port = process.env.port || 1337;
    http.createServer(function (req, res) {
        res.writeHead(200, { 'Content-Type': 'text/plain' });
        res.end('Hello World\n');
    }).listen(port);

<span data-ttu-id="39c6f-144">Tento kód je v podstatě hello stejná hodnota jako text "Hello, World" hello ukázku hello [nodejs.org] web, s výjimkou používá hello číslo portu přiřazené hello cloudového prostředí.</span><span class="sxs-lookup"><span data-stu-id="39c6f-144">This code is essentially hello same as hello "Hello World" sample on hello [nodejs.org] website, except it uses hello port number assigned by hello cloud environment.</span></span>

## <a name="deploy-hello-application-tooazure"></a><span data-ttu-id="39c6f-145">Nasazení aplikace tooAzure hello</span><span class="sxs-lookup"><span data-stu-id="39c6f-145">Deploy hello application tooAzure</span></span>

> [!NOTE]
> <span data-ttu-id="39c6f-146">toocomplete tohoto kurzu potřebujete účet Azure.</span><span class="sxs-lookup"><span data-stu-id="39c6f-146">toocomplete this tutorial, you need an Azure account.</span></span> <span data-ttu-id="39c6f-147">Můžete si [aktivovat výhody předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A85619ABF) nebo si [zaregistrovat bezplatný účet](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A85619ABF).</span><span class="sxs-lookup"><span data-stu-id="39c6f-147">You can [activate your MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A85619ABF) or [sign up for a free account](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A85619ABF).</span></span>

### <a name="download-hello-azure-publishing-settings"></a><span data-ttu-id="39c6f-148">Hello Azure stáhnout nastavení publikování</span><span class="sxs-lookup"><span data-stu-id="39c6f-148">Download hello Azure publishing settings</span></span>
<span data-ttu-id="39c6f-149">toodeploy tooAzure vaší aplikace, musíte nejprve stáhnout hello publikování nastavení pro vaše předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="39c6f-149">toodeploy your application tooAzure, you must first download hello publishing settings for your Azure subscription.</span></span>

1. <span data-ttu-id="39c6f-150">Spusťte následující rutiny Azure Powershellu hello:</span><span class="sxs-lookup"><span data-stu-id="39c6f-150">Run hello following Azure PowerShell cmdlet:</span></span>

       Get-AzurePublishSettingsFile

   <span data-ttu-id="39c6f-151">Tímto dojde k použití prohlížeče toonavigate toohello stránky stažení nastavení publikování.</span><span class="sxs-lookup"><span data-stu-id="39c6f-151">This will use your browser toonavigate toohello publish settings download page.</span></span> <span data-ttu-id="39c6f-152">Může být výzvami toolog pomocí Account Microsoft.</span><span class="sxs-lookup"><span data-stu-id="39c6f-152">You may be prompted toolog in with a Microsoft Account.</span></span> <span data-ttu-id="39c6f-153">Pokud ano, použijte účet hello spojený s předplatným Azure.</span><span class="sxs-lookup"><span data-stu-id="39c6f-153">If so, use hello account associated with your Azure subscription.</span></span>

   <span data-ttu-id="39c6f-154">Uložte hello stáhnout umístění souboru tooa profil, který lze snadno přistupovat.</span><span class="sxs-lookup"><span data-stu-id="39c6f-154">Save hello downloaded profile tooa file location you can easily access.</span></span>
2. <span data-ttu-id="39c6f-155">Spusťte následující rutinu tooimport hello stažený profil publikování:</span><span class="sxs-lookup"><span data-stu-id="39c6f-155">Run following cmdlet tooimport hello publishing profile you downloaded:</span></span>

       Import-AzurePublishSettingsFile [path toofile]

    > [!NOTE]
    > <span data-ttu-id="39c6f-156">Po importu hello nastavení publikování, zvažte odstranění hello stáhli soubor .publishSettings, protože obsahuje informace, které by někomu mohly umožnit tooaccess váš účet.</span><span class="sxs-lookup"><span data-stu-id="39c6f-156">After importing hello publish settings, consider deleting hello downloaded .publishSettings file, because it contains information that could allow someone tooaccess your account.</span></span>

### <a name="publish-hello-application"></a><span data-ttu-id="39c6f-157">Publikování aplikace hello</span><span class="sxs-lookup"><span data-stu-id="39c6f-157">Publish hello application</span></span>
<span data-ttu-id="39c6f-158">toopublish spustit hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="39c6f-158">toopublish, run hello following commands:</span></span>

      $ServiceName = "NodeHelloWorld" + $(Get-Date -Format ('ddhhmm'))
    Publish-AzureServiceProject -ServiceName $ServiceName  -Location "East US" -Launch

* <span data-ttu-id="39c6f-159">**-ServiceName** určuje hello název pro nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="39c6f-159">**-ServiceName** specifies hello name for hello deployment.</span></span> <span data-ttu-id="39c6f-160">Toto musí být jedinečný název, jinak hello publikovat proces se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="39c6f-160">This must be a unique name, otherwise hello publish process will fail.</span></span> <span data-ttu-id="39c6f-161">Hello **Get-Date** přiřadí k řetězci datum/čas, aby hello název jedinečný.</span><span class="sxs-lookup"><span data-stu-id="39c6f-161">hello **Get-Date** command tacks on a date/time string that should make hello name unique.</span></span>
* <span data-ttu-id="39c6f-162">**-Umístění** určuje hello datacenter, jejímž hostitelem aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="39c6f-162">**-Location** specifies hello datacenter that hello application will be hosted in.</span></span> <span data-ttu-id="39c6f-163">seznam dostupných datových center, použijte hello toosee **Get-AzureLocation** rutiny.</span><span class="sxs-lookup"><span data-stu-id="39c6f-163">toosee a list of available datacenters, use hello **Get-AzureLocation** cmdlet.</span></span>
* <span data-ttu-id="39c6f-164">**-Launch** otevře okno prohlížeče a po dokončení nasazení přejde toohello hostované služby.</span><span class="sxs-lookup"><span data-stu-id="39c6f-164">**-Launch** opens a browser window and navigates toohello hosted service after deployment has completed.</span></span>

<span data-ttu-id="39c6f-165">Po publikování bude úspěšné, zobrazí se odpověď podobnou toohello následující:</span><span class="sxs-lookup"><span data-stu-id="39c6f-165">After publishing succeeds, you will see a response similar toohello following:</span></span>

![výstup Hello hello příkazu Publish-AzureService][hello output of hello Publish-AzureService command]

> [!NOTE]
> <span data-ttu-id="39c6f-167">Může trvat několik minut, než toodeploy aplikace hello a k dispozici při prvním publikování.</span><span class="sxs-lookup"><span data-stu-id="39c6f-167">It can take several minutes for hello application toodeploy and become available when first published.</span></span>

<span data-ttu-id="39c6f-168">Po dokončení nasazení hello bude okno prohlížeče otevřete a přejděte toohello cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="39c6f-168">Once hello deployment has completed, a browser window will open and navigate toohello cloud service.</span></span>

![Okno prohlížeče zobrazující hello hello world stránky; Hello URL značí, že hello stránka hostována v Azure.][A browser window displaying hello hello world page; hello URL indicates hello page is hosted on Azure.]

<span data-ttu-id="39c6f-170">Aplikace je nyní spuštěna v systému Azure.</span><span class="sxs-lookup"><span data-stu-id="39c6f-170">Your application is now running on Azure.</span></span>

<span data-ttu-id="39c6f-171">Hello **Publish-AzureServiceProject** rutina provede hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="39c6f-171">hello **Publish-AzureServiceProject** cmdlet performs hello following steps:</span></span>

1. <span data-ttu-id="39c6f-172">Vytvoří toodeploy balíčku.</span><span class="sxs-lookup"><span data-stu-id="39c6f-172">Creates a package toodeploy.</span></span> <span data-ttu-id="39c6f-173">Hello balíček obsahuje všechny hello soubory ve složce aplikace.</span><span class="sxs-lookup"><span data-stu-id="39c6f-173">hello package contains all hello files in your application folder.</span></span>
2. <span data-ttu-id="39c6f-174">Vytvoří nový **účet úložiště**, pokud žádný neexistuje.</span><span class="sxs-lookup"><span data-stu-id="39c6f-174">Creates a new **storage account** if one does not exist.</span></span> <span data-ttu-id="39c6f-175">Hello účtu úložiště Azure je balíček aplikace hello toostore používané během nasazení.</span><span class="sxs-lookup"><span data-stu-id="39c6f-175">hello Azure storage account is used toostore hello application package during deployment.</span></span> <span data-ttu-id="39c6f-176">Po dokončení nasazení můžete bezpečně odstranit účet úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="39c6f-176">You can safely delete hello storage account after deployment is done.</span></span>
3. <span data-ttu-id="39c6f-177">Vytvoří novou **cloudovou službu**, pokud žádná neexistuje.</span><span class="sxs-lookup"><span data-stu-id="39c6f-177">Creates a new **cloud service** if one does not already exist.</span></span> <span data-ttu-id="39c6f-178">A **Cloudová služba** je hello kontejner, ve kterém je vaše aplikace hostována, když je nasazené tooAzure.</span><span class="sxs-lookup"><span data-stu-id="39c6f-178">A **cloud service** is hello container in which your application is hosted when it is deployed tooAzure.</span></span> <span data-ttu-id="39c6f-179">Další informace najdete v tématu [Přehled vytváření hostované služby pro Azure].</span><span class="sxs-lookup"><span data-stu-id="39c6f-179">For more information, see [Overview of Creating a Hosted Service for Azure].</span></span>
4. <span data-ttu-id="39c6f-180">Publikuje tooAzure balíčku nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="39c6f-180">Publishes hello deployment package tooAzure.</span></span>

## <a name="stopping-and-deleting-your-application"></a><span data-ttu-id="39c6f-181">Zastavení a odstranění vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="39c6f-181">Stopping and deleting your application</span></span>
<span data-ttu-id="39c6f-182">Po nasazení aplikace, může být vhodné toodisable ji tak, že se můžete vyhnout poplatkům.</span><span class="sxs-lookup"><span data-stu-id="39c6f-182">After deploying your application, you may want toodisable it so you can avoid extra costs.</span></span> <span data-ttu-id="39c6f-183">Azure účtuje poplatky za instance webových rolí podle času (v hodinách), který na serveru spotřebují.</span><span class="sxs-lookup"><span data-stu-id="39c6f-183">Azure bills web role instances per hour of server time consumed.</span></span> <span data-ttu-id="39c6f-184">Čas serveru se spotřebovává, jakmile vaše aplikace je nasazená, i když hello instance nejsou spuštěné a jsou ve stavu hello byla zastavena.</span><span class="sxs-lookup"><span data-stu-id="39c6f-184">Server time is consumed once your application is deployed, even if hello instances are not running and are in hello stopped state.</span></span>

1. <span data-ttu-id="39c6f-185">V okně prostředí Windows PowerShell hello zastavte hello nasazení služby vytvořené v předchozí části hello s hello následující rutiny:</span><span class="sxs-lookup"><span data-stu-id="39c6f-185">In hello Windows PowerShell window, stop hello service deployment created in hello previous section with hello following cmdlet:</span></span>

       Stop-AzureService

   <span data-ttu-id="39c6f-186">Zastavování služby hello může trvat několik minut.</span><span class="sxs-lookup"><span data-stu-id="39c6f-186">Stopping hello service may take several minutes.</span></span> <span data-ttu-id="39c6f-187">Pokud je hello služba zastavená, obdržíte zprávu s upozorněním, že se zastavil.</span><span class="sxs-lookup"><span data-stu-id="39c6f-187">When hello service is stopped, you receive a message indicating that it has stopped.</span></span>

   ![Hello stav příkazu hello Stop-AzureService][hello status of hello Stop-AzureService command]
2. <span data-ttu-id="39c6f-189">toodelete hello služba, volání hello následující rutiny:</span><span class="sxs-lookup"><span data-stu-id="39c6f-189">toodelete hello service, call hello following cmdlet:</span></span>

       Remove-AzureService

   <span data-ttu-id="39c6f-190">Po zobrazení výzvy zadejte **Y** toodelete hello služby.</span><span class="sxs-lookup"><span data-stu-id="39c6f-190">When prompted, enter **Y** toodelete hello service.</span></span>

   <span data-ttu-id="39c6f-191">Odstraněním hello služby může trvat několik minut.</span><span class="sxs-lookup"><span data-stu-id="39c6f-191">Deleting hello service may take several minutes.</span></span> <span data-ttu-id="39c6f-192">Po odstranění hello služby obdržíte zprávu s upozorněním, že se odstranila služba hello.</span><span class="sxs-lookup"><span data-stu-id="39c6f-192">After hello service has been deleted you receive a message indicating that hello service was deleted.</span></span>

   ![Hello stav příkazu hello Remove-AzureService][hello status of hello Remove-AzureService command]

   > [!NOTE]
   > <span data-ttu-id="39c6f-194">Odstraněním služby hello nedojde k odstranění účtu úložiště hello, který byl vytvořen při prvním publikování služby hello a bude pokračovat toobe účtovány poplatky za úložiště použít.</span><span class="sxs-lookup"><span data-stu-id="39c6f-194">Deleting hello service does not delete hello storage account that was created when hello service was initially published, and you will continue toobe billed for storage used.</span></span> <span data-ttu-id="39c6f-195">Pokud se nic jiného používá hello úložiště, může být vhodné toodelete ho.</span><span class="sxs-lookup"><span data-stu-id="39c6f-195">If nothing else is using hello storage, you may want toodelete it.</span></span>

## <a name="next-steps"></a><span data-ttu-id="39c6f-196">Další kroky</span><span class="sxs-lookup"><span data-stu-id="39c6f-196">Next steps</span></span>
<span data-ttu-id="39c6f-197">Další informace najdete v tématu hello [středisku pro vývojáře Node.js].</span><span class="sxs-lookup"><span data-stu-id="39c6f-197">For more information, see hello [Node.js Developer Center].</span></span>

<!-- URL List -->

[porovnání webů Azure, cloudové služby a virtuální počítače]: ../app-service-web/choose-web-site-cloud-service-vm.md
[Použití jednoduché webové aplikace]: ../app-service-web/app-service-web-get-started-nodejs.md
[Azure Powershell]: /powershell/azureps-cmdlets-docs
[Azure SDK pro .NET 2.7]: http://www.microsoft.com/en-us/download/details.aspx?id=48178
[Připojení Powershellu]: /powershell/azureps-cmdlets-docs#step-3-connect
[nodejs.org]: http://nodejs.org/
[Přehled vytváření hostované služby pro Azure]: https://azure.microsoft.com/documentation/services/cloud-services/
[středisku pro vývojáře Node.js]: https://azure.microsoft.com/develop/nodejs/

<!-- IMG List -->

[hello result of hello New-AzureService helloworld command]: ./media/cloud-services-nodejs-develop-deploy-app/node9.png
[hello output of hello Add-AzureNodeWebRole command]: ./media/cloud-services-nodejs-develop-deploy-app/node11.png
[A web browser displaying hello Hello World web page]: ./media/cloud-services-nodejs-develop-deploy-app/node14.png
[hello output of hello Publish-AzureService command]: ./media/cloud-services-nodejs-develop-deploy-app/node19.png
[A browser window displaying hello hello world page; hello URL indicates hello page is hosted on Azure.]: ./media/cloud-services-nodejs-develop-deploy-app/node21.png
[hello status of hello Stop-AzureService command]: ./media/cloud-services-nodejs-develop-deploy-app/node48.png
[hello status of hello Remove-AzureService command]: ./media/cloud-services-nodejs-develop-deploy-app/node49.png
