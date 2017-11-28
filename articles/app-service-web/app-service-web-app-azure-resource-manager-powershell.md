---
title: "příkazy aaaAzure založené na správci prostředků prostředí PowerShell pro webové aplikace Azure | Microsoft Docs"
description: "Zjistěte, jak toouse hello nové toomanage příkazy prostředí PowerShell využívající Azure Resource Manager webových aplikacích Azure."
services: app-service\web
documentationcenter: 
author: ahmedelnably
manager: stefsch
editor: 
ms.assetid: 4231fbba-61e5-4f92-b958-531c066fb87f
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/29/2016
ms.author: aelnably
ms.openlocfilehash: bbb821e89daa315280436e84e11316217bb644d8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-azure-resource-manager-based-powershell-toomanage-azure-web-apps"></a><span data-ttu-id="62bcd-103">Pomocí prostředí PowerShell Azure Resource Manager-Based tooManage Azure Web Apps</span><span class="sxs-lookup"><span data-stu-id="62bcd-103">Using Azure Resource Manager-Based PowerShell tooManage Azure Web Apps</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="62bcd-104">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="62bcd-104">Azure CLI</span></span>](app-service-web-app-azure-resource-manager-xplat-cli.md)
> * [<span data-ttu-id="62bcd-105">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="62bcd-105">Azure PowerShell</span></span>](app-service-web-app-azure-resource-manager-powershell.md)
> 
> 

<span data-ttu-id="62bcd-106">Microsoft Azure PowerShell verze 1.0.0 byly přidány nové příkazy, které poskytují hello uživatele hello možnost toouse založené na správci prostředků Azure PowerShell příkazy toomanage webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="62bcd-106">With Microsoft Azure PowerShell version 1.0.0 new commands have been added, that give hello user hello ability toouse Azure Resource Manager-based PowerShell commands toomanage Web Apps.</span></span>

<span data-ttu-id="62bcd-107">toolearn o správě skupin prostředků, najdete v části [použití Azure Powershellu s Azure Resource Manager](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="62bcd-107">toolearn about managing Resource Groups, see [Using Azure PowerShell with Azure Resource Manager](../powershell-azure-resource-manager.md).</span></span> 

<span data-ttu-id="62bcd-108">toolearn o hello úplný seznam parametrů a možnosti pro hello rutiny prostředí PowerShell najdete v části hello [úplné referenční informace o rutinách rutin prostředí PowerShell využívající webové aplikace Azure Resource Manager](https://msdn.microsoft.com/library/mt619237.aspx)</span><span class="sxs-lookup"><span data-stu-id="62bcd-108">toolearn about hello full list of parameters and options for hello PowerShell cmdlets, see hello [full Cmdlet Reference of Web App Azure Resource Manager-based PowerShell Cmdlets](https://msdn.microsoft.com/library/mt619237.aspx)</span></span>

## <a name="managing-app-service-plans"></a><span data-ttu-id="62bcd-109">Správa služby App Service plány</span><span class="sxs-lookup"><span data-stu-id="62bcd-109">Managing App Service Plans</span></span>
### <a name="create-an-app-service-plan"></a><span data-ttu-id="62bcd-110">Vytvořit plán služby App Service</span><span class="sxs-lookup"><span data-stu-id="62bcd-110">Create an App Service Plan</span></span>
<span data-ttu-id="62bcd-111">toocreate plán služby app service, použijte hello **New-AzureRmAppServicePlan** rutiny.</span><span class="sxs-lookup"><span data-stu-id="62bcd-111">toocreate an app service plan, use hello **New-AzureRmAppServicePlan** cmdlet.</span></span>

<span data-ttu-id="62bcd-112">Následují popisy hello různé parametry:</span><span class="sxs-lookup"><span data-stu-id="62bcd-112">Following are descriptions of hello different parameters:</span></span>

* <span data-ttu-id="62bcd-113">**Název**: název hello plán služby app service.</span><span class="sxs-lookup"><span data-stu-id="62bcd-113">**Name**: name of hello app service plan.</span></span>
* <span data-ttu-id="62bcd-114">**Umístění**: plán umístění služby.</span><span class="sxs-lookup"><span data-stu-id="62bcd-114">**Location**: service plan location.</span></span>
* <span data-ttu-id="62bcd-115">**Název skupiny prostředků**: skupinu prostředků, která zahrnuje hello nově vytvořený plán služby app service.</span><span class="sxs-lookup"><span data-stu-id="62bcd-115">**ResourceGroupName**: resource group that includes hello newly created app service plan.</span></span>
* <span data-ttu-id="62bcd-116">**Úroveň**: hello potřeby cenová úroveň (výchozí hodnota je volné, ostatní možnosti patří sdílené, Basic, Standard a Premium.)</span><span class="sxs-lookup"><span data-stu-id="62bcd-116">**Tier**:  hello desired pricing tier (Default is Free, other options are Shared, Basic, Standard, and Premium.)</span></span>
* <span data-ttu-id="62bcd-117">**WorkerSize**: hello velikost pracovních procesů (výchozí hodnota je malý, pokud byl zadán parametr vrstvy hello jako Basic, Standard nebo Premium.</span><span class="sxs-lookup"><span data-stu-id="62bcd-117">**WorkerSize**: hello size of workers (Default is small if hello Tier parameter was specified as Basic, Standard, or Premium.</span></span> <span data-ttu-id="62bcd-118">Další možnosti jsou střední a velké).</span><span class="sxs-lookup"><span data-stu-id="62bcd-118">Other options are Medium, and Large.)</span></span>
* <span data-ttu-id="62bcd-119">**NumberofWorkers**: hello počet pracovních procesů v plánu služby app hello (výchozí hodnota je 1).</span><span class="sxs-lookup"><span data-stu-id="62bcd-119">**NumberofWorkers**: hello number of workers in hello app service plan (Default value is 1).</span></span> 

<span data-ttu-id="62bcd-120">Příklad toouse tuto rutinu:</span><span class="sxs-lookup"><span data-stu-id="62bcd-120">Example toouse this cmdlet:</span></span>

    New-AzureRmAppServicePlan -Name ContosoAppServicePlan -Location "South Central US" -ResourceGroupName ContosoAzureResourceGroup -Tier Premium -WorkerSize Large -NumberofWorkers 10

### <a name="create-an-app-service-plan-in-an-app-service-environment"></a><span data-ttu-id="62bcd-121">Vytvořit plán služby App Service ve službě App Service Environment</span><span class="sxs-lookup"><span data-stu-id="62bcd-121">Create an App Service Plan in an App Service Environment</span></span>
<span data-ttu-id="62bcd-122">toocreate aplikační službu plánování ve službě app service environment, použijte hello stejný příkaz **New-AzureRmAppServicePlan** příkaz s názvem další parametry toospecify hello App Service Environment společnosti a App Service Environment je název skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="62bcd-122">toocreate an app service plan in an app service environment, use hello same command **New-AzureRmAppServicePlan** command with extra parameters toospecify hello ASE's name and ASE's resource group name.</span></span>

<span data-ttu-id="62bcd-123">Příklad toouse tuto rutinu:</span><span class="sxs-lookup"><span data-stu-id="62bcd-123">Example toouse this cmdlet:</span></span>

    New-AzureRmAppServicePlan -Name ContosoAppServicePlan -Location "South Central US" -ResourceGroupName ContosoAzureResourceGroup -AseName constosoASE -AseResourceGroupName contosoASERG -Tier Premium -WorkerSize Large -NumberofWorkers 10

<span data-ttu-id="62bcd-124">Další informace o službě app service environment, zkontrolujte toolearn [Úvod tooApp Service Environment](app-service-app-service-environment-intro.md)</span><span class="sxs-lookup"><span data-stu-id="62bcd-124">toolearn more about app service environment, check [Introduction tooApp Service Environment](app-service-app-service-environment-intro.md)</span></span>

### <a name="list-existing-app-service-plans"></a><span data-ttu-id="62bcd-125">Seznam existující plán služby App Service</span><span class="sxs-lookup"><span data-stu-id="62bcd-125">List Existing App Service Plans</span></span>
<span data-ttu-id="62bcd-126">používat toolist hello existující aplikace služby plány **Get-AzureRmAppServicePlan** rutiny.</span><span class="sxs-lookup"><span data-stu-id="62bcd-126">toolist hello existing app service plans, use **Get-AzureRmAppServicePlan** cmdlet.</span></span>

<span data-ttu-id="62bcd-127">toolist všechny plány služby app v rámci svého předplatného, použijte:</span><span class="sxs-lookup"><span data-stu-id="62bcd-127">toolist all app service plans under your subscription, use:</span></span> 

    Get-AzureRmAppServicePlan

<span data-ttu-id="62bcd-128">toolist všechny plány služby app v určité skupiny zdrojů, použijte:</span><span class="sxs-lookup"><span data-stu-id="62bcd-128">toolist all app service plans under a specific resource group, use:</span></span>

    Get-AzureRmAppServicePlan -ResourceGroupname ContosoAzureResourceGroup

<span data-ttu-id="62bcd-129">tooget plán služeb konkrétní aplikaci, použijte:</span><span class="sxs-lookup"><span data-stu-id="62bcd-129">tooget a specific app service plan, use:</span></span>

    Get-AzureRmAppServicePlan -Name ContosoAppServicePlan


### <a name="configure-an-existing-app-service-plan"></a><span data-ttu-id="62bcd-130">Konfigurace existující plán služby App Service</span><span class="sxs-lookup"><span data-stu-id="62bcd-130">Configure an existing App Service Plan</span></span>
<span data-ttu-id="62bcd-131">toochange hello nastavení existujícího plánu služby app, použijte hello **Set-AzureRmAppServicePlan** rutiny.</span><span class="sxs-lookup"><span data-stu-id="62bcd-131">toochange hello settings for an existing app service plan, use hello **Set-AzureRmAppServicePlan** cmdlet.</span></span> <span data-ttu-id="62bcd-132">Můžete změnit úroveň hello, velikost pracovního procesu a hello počet pracovních procesů</span><span class="sxs-lookup"><span data-stu-id="62bcd-132">You can change hello tier, worker size, and hello number of workers</span></span> 

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Tier Standard -WorkerSize Medium -NumberofWorkers 9

#### <a name="scaling-an-app-service-plan"></a><span data-ttu-id="62bcd-133">Škálování plán služby App Service</span><span class="sxs-lookup"><span data-stu-id="62bcd-133">Scaling an App Service Plan</span></span>
<span data-ttu-id="62bcd-134">tooscale existující plán služby App Service, použijte:</span><span class="sxs-lookup"><span data-stu-id="62bcd-134">tooscale an existing App Service Plan, use:</span></span>

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -NumberofWorkers 9

#### <a name="changing-hello-worker-size-of-an-app-service-plan"></a><span data-ttu-id="62bcd-135">Změna velikosti pracovní hello plánu služby App Service</span><span class="sxs-lookup"><span data-stu-id="62bcd-135">Changing hello worker size of an App Service Plan</span></span>
<span data-ttu-id="62bcd-136">velikost hello toochange pracovníků v existující plán služby App Service, použijte:</span><span class="sxs-lookup"><span data-stu-id="62bcd-136">toochange hello size of workers in an existing App Service Plan, use:</span></span>

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -WorkerSize Medium

#### <a name="changing-hello-tier-of-an-app-service-plan"></a><span data-ttu-id="62bcd-137">Změna hello úroveň plán služby App Service</span><span class="sxs-lookup"><span data-stu-id="62bcd-137">Changing hello Tier of an App Service Plan</span></span>
<span data-ttu-id="62bcd-138">toochange úroveň hello existující plán služby App Service, použijte:</span><span class="sxs-lookup"><span data-stu-id="62bcd-138">toochange hello tier of an existing App Service Plan, use:</span></span>

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Tier Standard

### <a name="delete-an-existing-app-service-plan"></a><span data-ttu-id="62bcd-139">Odstranit existující plán služby App Service</span><span class="sxs-lookup"><span data-stu-id="62bcd-139">Delete an existing App Service Plan</span></span>
<span data-ttu-id="62bcd-140">toodelete existující plán služby app service, všechny přiřazené webové aplikace potřeba toobe nepřesunul nebo neodstranil nejdřív.</span><span class="sxs-lookup"><span data-stu-id="62bcd-140">toodelete an existing app service plan, all assigned web apps need toobe moved or deleted first.</span></span> <span data-ttu-id="62bcd-141">Potom pomocí hello **odebrat AzureRmAppServicePlan** rutiny můžete odstranit hello plán služby app service.</span><span class="sxs-lookup"><span data-stu-id="62bcd-141">Then using hello **Remove-AzureRmAppServicePlan** cmdlet you can delete hello app service plan.</span></span>

    Remove-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup

## <a name="managing-app-service-web-apps"></a><span data-ttu-id="62bcd-142">Správa webové aplikace aplikační služby</span><span class="sxs-lookup"><span data-stu-id="62bcd-142">Managing App Service Web Apps</span></span>
### <a name="create-a-web-app"></a><span data-ttu-id="62bcd-143">Vytvoření webové aplikace</span><span class="sxs-lookup"><span data-stu-id="62bcd-143">Create a Web App</span></span>
<span data-ttu-id="62bcd-144">toocreate webové aplikace, použijte hello **New-AzureRmWebApp** rutiny.</span><span class="sxs-lookup"><span data-stu-id="62bcd-144">toocreate a web app, use hello **New-AzureRmWebApp** cmdlet.</span></span>

<span data-ttu-id="62bcd-145">Následují popisy hello různé parametry:</span><span class="sxs-lookup"><span data-stu-id="62bcd-145">Following are descriptions of hello different parameters:</span></span>

* <span data-ttu-id="62bcd-146">**Název**: název webové aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="62bcd-146">**Name**: name for hello web app.</span></span>
* <span data-ttu-id="62bcd-147">**AppServicePlan**: název pro plán služby hello použít toohost hello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="62bcd-147">**AppServicePlan**: name for hello service plan used toohost hello web app.</span></span>
* <span data-ttu-id="62bcd-148">**Název skupiny prostředků**: skupinu prostředků, který je hostitelem hello plán služby App service.</span><span class="sxs-lookup"><span data-stu-id="62bcd-148">**ResourceGroupName**: resource group that hosts hello App service plan.</span></span>
* <span data-ttu-id="62bcd-149">**Umístění**: hello umístění webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="62bcd-149">**Location**: hello web app location.</span></span>

<span data-ttu-id="62bcd-150">Příklad toouse tuto rutinu:</span><span class="sxs-lookup"><span data-stu-id="62bcd-150">Example toouse this cmdlet:</span></span>

    New-AzureRmWebApp -Name ContosoWebApp -AppServicePlan ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Location "South Central US"

### <a name="create-a-web-app-in-an-app-service-environment"></a><span data-ttu-id="62bcd-151">Vytvoření webové aplikace ve službě App Service Environment</span><span class="sxs-lookup"><span data-stu-id="62bcd-151">Create a Web App in an App Service Environment</span></span>
<span data-ttu-id="62bcd-152">toocreate webové aplikace v App Service prostředí (App Service Environment).</span><span class="sxs-lookup"><span data-stu-id="62bcd-152">toocreate a web app in an App Service Environment (ASE).</span></span> <span data-ttu-id="62bcd-153">Použití hello stejné **New-AzureRmWebApp** příkaz s názvem hello App Service Environment toospecify další parametry a hello název skupiny prostředků, které patří hello App Service Environment.</span><span class="sxs-lookup"><span data-stu-id="62bcd-153">Use hello same **New-AzureRmWebApp** command with extra parameters toospecify hello ASE name and hello resource group name that hello ASE belongs to.</span></span>

    New-AzureRmWebApp -Name ContosoWebApp -AppServicePlan ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Location "South Central US"  -ASEName ContosoASEName -ASEResourceGroupName ContosoASEResourceGroupName

<span data-ttu-id="62bcd-154">Další informace o službě app service environment, zkontrolujte toolearn [Úvod tooApp Service Environment](app-service-app-service-environment-intro.md)</span><span class="sxs-lookup"><span data-stu-id="62bcd-154">toolearn more about app service environment, check [Introduction tooApp Service Environment](app-service-app-service-environment-intro.md)</span></span>

### <a name="delete-an-existing-web-app"></a><span data-ttu-id="62bcd-155">Odstranit stávající webovou aplikaci</span><span class="sxs-lookup"><span data-stu-id="62bcd-155">Delete an existing Web App</span></span>
<span data-ttu-id="62bcd-156">toodelete stávající webovou aplikaci můžete použít hello **odebrat AzureRmWebApp** rutiny, je třeba název hello toospecify hello webové aplikace a název skupiny prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="62bcd-156">toodelete an existing web app you can use hello **Remove-AzureRmWebApp** cmdlet, you need toospecify hello name of hello web app and hello resource group name.</span></span>

    Remove-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup


### <a name="list-existing-web-apps"></a><span data-ttu-id="62bcd-157">Seznam existující webové aplikace</span><span class="sxs-lookup"><span data-stu-id="62bcd-157">List existing Web Apps</span></span>
<span data-ttu-id="62bcd-158">toolist hello existující webové aplikace, použijte hello **Get-AzureRmWebApp** rutiny.</span><span class="sxs-lookup"><span data-stu-id="62bcd-158">toolist hello existing web apps, use hello **Get-AzureRmWebApp** cmdlet.</span></span>

<span data-ttu-id="62bcd-159">toolist všechny webové aplikace v rámci svého předplatného, použijte:</span><span class="sxs-lookup"><span data-stu-id="62bcd-159">toolist all web apps under your subscription, use:</span></span>

    Get-AzureRmWebApp

<span data-ttu-id="62bcd-160">toolist všechny webové aplikace v rámci určité skupiny zdrojů, použijte:</span><span class="sxs-lookup"><span data-stu-id="62bcd-160">toolist all web apps under a specific resource group, use:</span></span>

    Get-AzureRmWebApp -ResourceGroupname ContosoAzureResourceGroup

<span data-ttu-id="62bcd-161">tooget konkrétní webovou aplikaci, použijte:</span><span class="sxs-lookup"><span data-stu-id="62bcd-161">tooget a specific web app, use:</span></span>

    Get-AzureRmWebApp -Name ContosoWebApp

### <a name="configure-an-existing-web-app"></a><span data-ttu-id="62bcd-162">Konfigurace existující webové aplikace</span><span class="sxs-lookup"><span data-stu-id="62bcd-162">Configure an existing Web App</span></span>
<span data-ttu-id="62bcd-163">toochange hello nastavení a konfigurace pro stávající webovou aplikaci, použijte hello **Set-AzureRmWebApp** rutiny.</span><span class="sxs-lookup"><span data-stu-id="62bcd-163">toochange hello settings and configurations for an existing web app, use hello **Set-AzureRmWebApp** cmdlet.</span></span> <span data-ttu-id="62bcd-164">Úplný seznam parametrů, zkontrolujte hello [rutiny odkazem](https://msdn.microsoft.com/library/mt652487.aspx)</span><span class="sxs-lookup"><span data-stu-id="62bcd-164">For a full list of parameters, check hello [Cmdlet reference link](https://msdn.microsoft.com/library/mt652487.aspx)</span></span>

<span data-ttu-id="62bcd-165">Příklad (1): pomocí této rutiny toochange připojovací řetězce</span><span class="sxs-lookup"><span data-stu-id="62bcd-165">Example (1): use this cmdlet toochange connection strings</span></span>

    $connectionstrings = @{ ContosoConn1 = @{ Type = “MySql”; Value = “MySqlConn”}; ContosoConn2 = @{ Type = “SQLAzure”; Value = “SQLAzureConn”} }
    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -ConnectionStrings $connectionstrings

<span data-ttu-id="62bcd-166">Příklad (2): Přidání nebo změna nastavení aplikace</span><span class="sxs-lookup"><span data-stu-id="62bcd-166">Example (2): add or change app settings</span></span>

    $appsettings = @{appsetting1 = "appsetting1value"; appsetting2 = "appsetting2value"}
    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -AppSettings $appsettings


<span data-ttu-id="62bcd-167">Příklad (3): nastavte hello webové aplikace toorun v režimu 64-bit</span><span class="sxs-lookup"><span data-stu-id="62bcd-167">Example (3):  set hello web app toorun in 64-bit mode</span></span>

    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -Use32BitWorkerProcess $False

### <a name="change-hello-state-of-an-existing-web-app"></a><span data-ttu-id="62bcd-168">Změnit stav hello stávající webovou aplikaci</span><span class="sxs-lookup"><span data-stu-id="62bcd-168">Change hello state of an existing Web App</span></span>
#### <a name="restart-a-web-app"></a><span data-ttu-id="62bcd-169">Restartování webové aplikace</span><span class="sxs-lookup"><span data-stu-id="62bcd-169">Restart a web app</span></span>
<span data-ttu-id="62bcd-170">toorestart webové aplikace, je nutné zadat název a prostředek skupiny hello hello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="62bcd-170">toorestart a web app, you must specify hello name and resource group of hello web app.</span></span>

    Restart-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

#### <a name="stop-a-web-app"></a><span data-ttu-id="62bcd-171">Zastavení webové aplikace</span><span class="sxs-lookup"><span data-stu-id="62bcd-171">Stop a web app</span></span>
<span data-ttu-id="62bcd-172">toostop webové aplikace, je nutné zadat název a prostředek skupiny hello hello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="62bcd-172">toostop a web app, you must specify hello name and resource group of hello web app.</span></span>

    Stop-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

#### <a name="start-a-web-app"></a><span data-ttu-id="62bcd-173">Spustí webovou aplikaci</span><span class="sxs-lookup"><span data-stu-id="62bcd-173">Start a web app</span></span>
<span data-ttu-id="62bcd-174">toostart webové aplikace, je nutné zadat název a prostředek skupiny hello hello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="62bcd-174">toostart a web app, you must specify hello name and resource group of hello web app.</span></span>

    Start-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

### <a name="manage-web-app-publishing-profiles"></a><span data-ttu-id="62bcd-175">Správa profilů publikování webové aplikace</span><span class="sxs-lookup"><span data-stu-id="62bcd-175">Manage Web App Publishing profiles</span></span>
<span data-ttu-id="62bcd-176">Každý webové aplikace se profil publikování, které můžou být použité toopublish aplikace, několik operací mohou být provedeny u profilů publikování.</span><span class="sxs-lookup"><span data-stu-id="62bcd-176">Each web app has a publishing profile that can be used toopublish your apps, several operations can be executed on publishing profiles.</span></span>

#### <a name="get-publishing-profile"></a><span data-ttu-id="62bcd-177">Získat profil publikování</span><span class="sxs-lookup"><span data-stu-id="62bcd-177">Get Publishing Profile</span></span>
<span data-ttu-id="62bcd-178">hello tooget publikování profil pro webovou aplikaci, použijte:</span><span class="sxs-lookup"><span data-stu-id="62bcd-178">tooget hello publishing profile for a web app, use:</span></span>

    Get-AzureRmWebAppPublishingProfile -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -OutputFile .\publishingprofile.txt

<span data-ttu-id="62bcd-179">Tento příkaz vrátí, že hello publikování profil toohello příkazového řádku také výstup hello publikování profil tooa textového souboru.</span><span class="sxs-lookup"><span data-stu-id="62bcd-179">This command echoes hello publishing profile toohello command line as well output hello publishing profile tooa text file.</span></span>

#### <a name="reset-publishing-profile"></a><span data-ttu-id="62bcd-180">Resetování profilu publikování</span><span class="sxs-lookup"><span data-stu-id="62bcd-180">Reset Publishing Profile</span></span>
<span data-ttu-id="62bcd-181">tooreset i pro FTP a webové nasazení pro webovou aplikaci, použijte hello publikování heslo:</span><span class="sxs-lookup"><span data-stu-id="62bcd-181">tooreset both hello publishing password for FTP and web deploy for a web app, use:</span></span>

    Reset-AzureRmWebAppPublishingProfile -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

### <a name="manage-web-app-certificates"></a><span data-ttu-id="62bcd-182">Spravovat certifikáty webové aplikace</span><span class="sxs-lookup"><span data-stu-id="62bcd-182">Manage Web App Certificates</span></span>
<span data-ttu-id="62bcd-183">toolearn o tom, jak toomanage webové aplikace certifikáty, najdete v části [vazby certifikátů SSL pomocí prostředí PowerShell](app-service-web-app-powershell-ssl-binding.md)</span><span class="sxs-lookup"><span data-stu-id="62bcd-183">toolearn about how toomanage web app certificates, see [SSL Certificates binding using PowerShell](app-service-web-app-powershell-ssl-binding.md)</span></span>

### <a name="next-steps"></a><span data-ttu-id="62bcd-184">Další kroky</span><span class="sxs-lookup"><span data-stu-id="62bcd-184">Next Steps</span></span>
* <span data-ttu-id="62bcd-185">toolearn o podpoře Azure Resource Manager PowerShell najdete v části [použití Azure Powershellu s Azure Resource Manager.](../powershell-azure-resource-manager.md)</span><span class="sxs-lookup"><span data-stu-id="62bcd-185">toolearn about Azure Resource Manager PowerShell support, see [Using Azure PowerShell with Azure Resource Manager.](../powershell-azure-resource-manager.md)</span></span>
* <span data-ttu-id="62bcd-186">toolearn o prostředí App Service najdete v části [Úvod tooApp Service Environment.](app-service-app-service-environment-intro.md)</span><span class="sxs-lookup"><span data-stu-id="62bcd-186">toolearn about App Service Environments, see [Introduction tooApp Service Environment.](app-service-app-service-environment-intro.md)</span></span>
* <span data-ttu-id="62bcd-187">toolearn o správě certifikátů App Service SSL pomocí prostředí PowerShell, najdete v části [vazby certifikátů SSL pomocí prostředí PowerShell.](app-service-web-app-powershell-ssl-binding.md)</span><span class="sxs-lookup"><span data-stu-id="62bcd-187">toolearn about managing App Service SSL certificates using PowerShell, see [SSL Certificates binding using PowerShell.](app-service-web-app-powershell-ssl-binding.md)</span></span>
* <span data-ttu-id="62bcd-188">toolearn o hello úplný seznam rutin prostředí PowerShell založené na správci prostředků Azure pro webové aplikace Azure, najdete v části [Azure Cmdlet Reference rutin Powershellu webové aplikace Azure Resource Manager.](https://msdn.microsoft.com/library/mt619237.aspx)</span><span class="sxs-lookup"><span data-stu-id="62bcd-188">toolearn about hello full list of Azure Resource Manager-based PowerShell cmdlets for Azure Web Apps, see [Azure Cmdlet Reference of Web Apps Azure Resource Manager PowerShell Cmdlets.](https://msdn.microsoft.com/library/mt619237.aspx)</span></span>
* * <span data-ttu-id="62bcd-189">toolearn o správě služby App Service pomocí rozhraní příkazového řádku, najdete v části [Using Azure Resource Manager-Based XPlat rozhraní příkazového řádku pro webové aplikace Azure.](app-service-web-app-azure-resource-manager-xplat-cli.md)</span><span class="sxs-lookup"><span data-stu-id="62bcd-189">toolearn about managing App Service using CLI, see [Using Azure Resource Manager-Based XPlat CLI for Azure Web App.](app-service-web-app-azure-resource-manager-xplat-cli.md)</span></span>

