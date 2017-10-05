---
title: "Azure PowerShell využívající Resource Manager příkazy pro webové aplikace Azure | Microsoft Docs"
description: "Zjistěte, jak používat nové příkazy prostředí PowerShell využívající Azure Resource Manager ke správě webových aplikacích Azure."
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
ms.openlocfilehash: 8d574f051a327ba0409e6f25a5886af673d3d5e0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="using-azure-resource-manager-based-powershell-to-manage-azure-web-apps"></a><span data-ttu-id="998dd-103">Pomocí prostředí PowerShell založené na správci prostředků Azure ke správě webových aplikacích Azure</span><span class="sxs-lookup"><span data-stu-id="998dd-103">Using Azure Resource Manager-Based PowerShell to Manage Azure Web Apps</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="998dd-104">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="998dd-104">Azure CLI</span></span>](app-service-web-app-azure-resource-manager-xplat-cli.md)
> * [<span data-ttu-id="998dd-105">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="998dd-105">Azure PowerShell</span></span>](app-service-web-app-azure-resource-manager-powershell.md)
> 
> 

<span data-ttu-id="998dd-106">Microsoft Azure PowerShell verze 1.0.0 byly přidány nové příkazy, které uživateli přidělit možnost použít příkazy prostředí PowerShell využívající Azure Resource Manager ke správě webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="998dd-106">With Microsoft Azure PowerShell version 1.0.0 new commands have been added, that give the user the ability to use Azure Resource Manager-based PowerShell commands to manage Web Apps.</span></span>

<span data-ttu-id="998dd-107">Další informace o správě skupin prostředků, najdete v části [použití Azure Powershellu s Azure Resource Manager](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="998dd-107">To learn about managing Resource Groups, see [Using Azure PowerShell with Azure Resource Manager](../powershell-azure-resource-manager.md).</span></span> 

<span data-ttu-id="998dd-108">Další informace o úplný seznam parametrů a možnosti pro rutiny prostředí PowerShell najdete v tématu [úplné referenční informace o rutinách rutin prostředí PowerShell využívající webové aplikace Azure Resource Manager](https://msdn.microsoft.com/library/mt619237.aspx)</span><span class="sxs-lookup"><span data-stu-id="998dd-108">To learn about the full list of parameters and options for the PowerShell cmdlets, see the [full Cmdlet Reference of Web App Azure Resource Manager-based PowerShell Cmdlets](https://msdn.microsoft.com/library/mt619237.aspx)</span></span>

## <a name="managing-app-service-plans"></a><span data-ttu-id="998dd-109">Správa služby App Service plány</span><span class="sxs-lookup"><span data-stu-id="998dd-109">Managing App Service Plans</span></span>
### <a name="create-an-app-service-plan"></a><span data-ttu-id="998dd-110">Vytvořit plán služby App Service</span><span class="sxs-lookup"><span data-stu-id="998dd-110">Create an App Service Plan</span></span>
<span data-ttu-id="998dd-111">Chcete-li vytvořit plán služby app service, použijte **New-AzureRmAppServicePlan** rutiny.</span><span class="sxs-lookup"><span data-stu-id="998dd-111">To create an app service plan, use the **New-AzureRmAppServicePlan** cmdlet.</span></span>

<span data-ttu-id="998dd-112">Následují popisy různé parametry:</span><span class="sxs-lookup"><span data-stu-id="998dd-112">Following are descriptions of the different parameters:</span></span>

* <span data-ttu-id="998dd-113">**Název**: název plánu služby app service.</span><span class="sxs-lookup"><span data-stu-id="998dd-113">**Name**: name of the app service plan.</span></span>
* <span data-ttu-id="998dd-114">**Umístění**: plán umístění služby.</span><span class="sxs-lookup"><span data-stu-id="998dd-114">**Location**: service plan location.</span></span>
* <span data-ttu-id="998dd-115">**Název skupiny prostředků**: skupinu prostředků, která zahrnuje tarifu nově vytvořené aplikace.</span><span class="sxs-lookup"><span data-stu-id="998dd-115">**ResourceGroupName**: resource group that includes the newly created app service plan.</span></span>
* <span data-ttu-id="998dd-116">**Úroveň**: s požadovanou cenovou úrovní (výchozí hodnota je volné, ostatní možnosti patří sdílené, Basic, Standard a Premium.)</span><span class="sxs-lookup"><span data-stu-id="998dd-116">**Tier**:  the desired pricing tier (Default is Free, other options are Shared, Basic, Standard, and Premium.)</span></span>
* <span data-ttu-id="998dd-117">**WorkerSize**: velikost pracovních procesů (výchozí hodnota je malý, pokud parametr vrstvy byl zadán jako Basic, Standard nebo Premium.</span><span class="sxs-lookup"><span data-stu-id="998dd-117">**WorkerSize**: the size of workers (Default is small if the Tier parameter was specified as Basic, Standard, or Premium.</span></span> <span data-ttu-id="998dd-118">Další možnosti jsou střední a velké).</span><span class="sxs-lookup"><span data-stu-id="998dd-118">Other options are Medium, and Large.)</span></span>
* <span data-ttu-id="998dd-119">**NumberofWorkers**: počet pracovních procesů v aplikaci služby plán (výchozí hodnota je 1).</span><span class="sxs-lookup"><span data-stu-id="998dd-119">**NumberofWorkers**: the number of workers in the app service plan (Default value is 1).</span></span> 

<span data-ttu-id="998dd-120">Příklad pro použití této rutiny:</span><span class="sxs-lookup"><span data-stu-id="998dd-120">Example to use this cmdlet:</span></span>

    New-AzureRmAppServicePlan -Name ContosoAppServicePlan -Location "South Central US" -ResourceGroupName ContosoAzureResourceGroup -Tier Premium -WorkerSize Large -NumberofWorkers 10

### <a name="create-an-app-service-plan-in-an-app-service-environment"></a><span data-ttu-id="998dd-121">Vytvořit plán služby App Service ve službě App Service Environment</span><span class="sxs-lookup"><span data-stu-id="998dd-121">Create an App Service Plan in an App Service Environment</span></span>
<span data-ttu-id="998dd-122">Chcete-li vytvořit plán služby app service ve službě app service environment, použijte stejný příkaz **New-AzureRmAppServicePlan** s další parametry zadejte název App Service Environment a App Service Environment je název skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="998dd-122">To create an app service plan in an app service environment, use the same command **New-AzureRmAppServicePlan** command with extra parameters to specify the ASE's name and ASE's resource group name.</span></span>

<span data-ttu-id="998dd-123">Příklad pro použití této rutiny:</span><span class="sxs-lookup"><span data-stu-id="998dd-123">Example to use this cmdlet:</span></span>

    New-AzureRmAppServicePlan -Name ContosoAppServicePlan -Location "South Central US" -ResourceGroupName ContosoAzureResourceGroup -AseName constosoASE -AseResourceGroupName contosoASERG -Tier Premium -WorkerSize Large -NumberofWorkers 10

<span data-ttu-id="998dd-124">Další informace o službě app service environment, zkontrolujte [Úvod do služby App Service Environment](app-service-app-service-environment-intro.md)</span><span class="sxs-lookup"><span data-stu-id="998dd-124">To learn more about app service environment, check [Introduction to App Service Environment](app-service-app-service-environment-intro.md)</span></span>

### <a name="list-existing-app-service-plans"></a><span data-ttu-id="998dd-125">Seznam existující plán služby App Service</span><span class="sxs-lookup"><span data-stu-id="998dd-125">List Existing App Service Plans</span></span>
<span data-ttu-id="998dd-126">K zobrazení seznamu existující plány služby aplikace, použijte **Get-AzureRmAppServicePlan** rutiny.</span><span class="sxs-lookup"><span data-stu-id="998dd-126">To list the existing app service plans, use **Get-AzureRmAppServicePlan** cmdlet.</span></span>

<span data-ttu-id="998dd-127">K zobrazení seznamu všechny plány služby app v rámci svého předplatného, použijte:</span><span class="sxs-lookup"><span data-stu-id="998dd-127">To list all app service plans under your subscription, use:</span></span> 

    Get-AzureRmAppServicePlan

<span data-ttu-id="998dd-128">K zobrazení seznamu všech plánů služby app podle určité skupiny zdrojů, použijte:</span><span class="sxs-lookup"><span data-stu-id="998dd-128">To list all app service plans under a specific resource group, use:</span></span>

    Get-AzureRmAppServicePlan -ResourceGroupname ContosoAzureResourceGroup

<span data-ttu-id="998dd-129">Plán služby konkrétní aplikaci, použijte:</span><span class="sxs-lookup"><span data-stu-id="998dd-129">To get a specific app service plan, use:</span></span>

    Get-AzureRmAppServicePlan -Name ContosoAppServicePlan


### <a name="configure-an-existing-app-service-plan"></a><span data-ttu-id="998dd-130">Konfigurace existující plán služby App Service</span><span class="sxs-lookup"><span data-stu-id="998dd-130">Configure an existing App Service Plan</span></span>
<span data-ttu-id="998dd-131">Chcete-li změnit nastavení existujícího plánu služby app, použijte **Set-AzureRmAppServicePlan** rutiny.</span><span class="sxs-lookup"><span data-stu-id="998dd-131">To change the settings for an existing app service plan, use the **Set-AzureRmAppServicePlan** cmdlet.</span></span> <span data-ttu-id="998dd-132">Můžete změnit úroveň, pracovní velikost a počet pracovních procesů</span><span class="sxs-lookup"><span data-stu-id="998dd-132">You can change the tier, worker size, and the number of workers</span></span> 

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Tier Standard -WorkerSize Medium -NumberofWorkers 9

#### <a name="scaling-an-app-service-plan"></a><span data-ttu-id="998dd-133">Škálování plán služby App Service</span><span class="sxs-lookup"><span data-stu-id="998dd-133">Scaling an App Service Plan</span></span>
<span data-ttu-id="998dd-134">Pokud chcete použít škálování, existující plán služby App Service, použijte:</span><span class="sxs-lookup"><span data-stu-id="998dd-134">To scale an existing App Service Plan, use:</span></span>

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -NumberofWorkers 9

#### <a name="changing-the-worker-size-of-an-app-service-plan"></a><span data-ttu-id="998dd-135">Změna velikosti pracovního procesu plán služby App Service</span><span class="sxs-lookup"><span data-stu-id="998dd-135">Changing the worker size of an App Service Plan</span></span>
<span data-ttu-id="998dd-136">Chcete-li změnit velikost pracovních procesů v existující plán služby App Service, použijte:</span><span class="sxs-lookup"><span data-stu-id="998dd-136">To change the size of workers in an existing App Service Plan, use:</span></span>

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -WorkerSize Medium

#### <a name="changing-the-tier-of-an-app-service-plan"></a><span data-ttu-id="998dd-137">Při změně úrovně plán služby App Service</span><span class="sxs-lookup"><span data-stu-id="998dd-137">Changing the Tier of an App Service Plan</span></span>
<span data-ttu-id="998dd-138">Pokud chcete změnit úroveň existující plán služby App Service, použijte:</span><span class="sxs-lookup"><span data-stu-id="998dd-138">To change the tier of an existing App Service Plan, use:</span></span>

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Tier Standard

### <a name="delete-an-existing-app-service-plan"></a><span data-ttu-id="998dd-139">Odstranit existující plán služby App Service</span><span class="sxs-lookup"><span data-stu-id="998dd-139">Delete an existing App Service Plan</span></span>
<span data-ttu-id="998dd-140">Pokud chcete odstranit existující plán služby app service, musí všechny přiřazené webové aplikace přesunout ani neodstraní.</span><span class="sxs-lookup"><span data-stu-id="998dd-140">To delete an existing app service plan, all assigned web apps need to be moved or deleted first.</span></span> <span data-ttu-id="998dd-141">Potom pomocí **odebrat AzureRmAppServicePlan** rutiny můžete odstranit plán služby app service.</span><span class="sxs-lookup"><span data-stu-id="998dd-141">Then using the **Remove-AzureRmAppServicePlan** cmdlet you can delete the app service plan.</span></span>

    Remove-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup

## <a name="managing-app-service-web-apps"></a><span data-ttu-id="998dd-142">Správa webové aplikace aplikační služby</span><span class="sxs-lookup"><span data-stu-id="998dd-142">Managing App Service Web Apps</span></span>
### <a name="create-a-web-app"></a><span data-ttu-id="998dd-143">Vytvoření webové aplikace</span><span class="sxs-lookup"><span data-stu-id="998dd-143">Create a Web App</span></span>
<span data-ttu-id="998dd-144">Chcete-li vytvořit webovou aplikaci, použijte **New-AzureRmWebApp** rutiny.</span><span class="sxs-lookup"><span data-stu-id="998dd-144">To create a web app, use the **New-AzureRmWebApp** cmdlet.</span></span>

<span data-ttu-id="998dd-145">Následují popisy různé parametry:</span><span class="sxs-lookup"><span data-stu-id="998dd-145">Following are descriptions of the different parameters:</span></span>

* <span data-ttu-id="998dd-146">**Název**: název webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="998dd-146">**Name**: name for the web app.</span></span>
* <span data-ttu-id="998dd-147">**AppServicePlan**: název plánu služby použít k hostování webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="998dd-147">**AppServicePlan**: name for the service plan used to host the web app.</span></span>
* <span data-ttu-id="998dd-148">**Název skupiny prostředků**: skupinu prostředků, který je hostitelem plán služby App service.</span><span class="sxs-lookup"><span data-stu-id="998dd-148">**ResourceGroupName**: resource group that hosts the App service plan.</span></span>
* <span data-ttu-id="998dd-149">**Umístění**: umístění webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="998dd-149">**Location**: the web app location.</span></span>

<span data-ttu-id="998dd-150">Příklad pro použití této rutiny:</span><span class="sxs-lookup"><span data-stu-id="998dd-150">Example to use this cmdlet:</span></span>

    New-AzureRmWebApp -Name ContosoWebApp -AppServicePlan ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Location "South Central US"

### <a name="create-a-web-app-in-an-app-service-environment"></a><span data-ttu-id="998dd-151">Vytvoření webové aplikace ve službě App Service Environment</span><span class="sxs-lookup"><span data-stu-id="998dd-151">Create a Web App in an App Service Environment</span></span>
<span data-ttu-id="998dd-152">K vytvoření webové aplikace v App Service prostředí (App Service Environment).</span><span class="sxs-lookup"><span data-stu-id="998dd-152">To create a web app in an App Service Environment (ASE).</span></span> <span data-ttu-id="998dd-153">Použijte stejný **New-AzureRmWebApp** příkazu s další parametry, zadejte název App Service Environment a skupině prostředků název, který App Service Environment patří.</span><span class="sxs-lookup"><span data-stu-id="998dd-153">Use the same **New-AzureRmWebApp** command with extra parameters to specify the ASE name and the resource group name that the ASE belongs to.</span></span>

    New-AzureRmWebApp -Name ContosoWebApp -AppServicePlan ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Location "South Central US"  -ASEName ContosoASEName -ASEResourceGroupName ContosoASEResourceGroupName

<span data-ttu-id="998dd-154">Další informace o službě app service environment, zkontrolujte [Úvod do služby App Service Environment](app-service-app-service-environment-intro.md)</span><span class="sxs-lookup"><span data-stu-id="998dd-154">To learn more about app service environment, check [Introduction to App Service Environment](app-service-app-service-environment-intro.md)</span></span>

### <a name="delete-an-existing-web-app"></a><span data-ttu-id="998dd-155">Odstranit stávající webovou aplikaci</span><span class="sxs-lookup"><span data-stu-id="998dd-155">Delete an existing Web App</span></span>
<span data-ttu-id="998dd-156">Chcete-li odstranit stávající webovou aplikaci můžete použít **odebrat AzureRmWebApp** rutiny, je třeba zadat název webové aplikace a název skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="998dd-156">To delete an existing web app you can use the **Remove-AzureRmWebApp** cmdlet, you need to specify the name of the web app and the resource group name.</span></span>

    Remove-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup


### <a name="list-existing-web-apps"></a><span data-ttu-id="998dd-157">Seznam existující webové aplikace</span><span class="sxs-lookup"><span data-stu-id="998dd-157">List existing Web Apps</span></span>
<span data-ttu-id="998dd-158">K zobrazení seznamu existující webové aplikace, použijte **Get-AzureRmWebApp** rutiny.</span><span class="sxs-lookup"><span data-stu-id="998dd-158">To list the existing web apps, use the **Get-AzureRmWebApp** cmdlet.</span></span>

<span data-ttu-id="998dd-159">K zobrazení seznamu všech webových aplikací v rámci svého předplatného, použijte:</span><span class="sxs-lookup"><span data-stu-id="998dd-159">To list all web apps under your subscription, use:</span></span>

    Get-AzureRmWebApp

<span data-ttu-id="998dd-160">K zobrazení seznamu všech webových aplikací v rámci určité skupiny zdrojů, použijte:</span><span class="sxs-lookup"><span data-stu-id="998dd-160">To list all web apps under a specific resource group, use:</span></span>

    Get-AzureRmWebApp -ResourceGroupname ContosoAzureResourceGroup

<span data-ttu-id="998dd-161">Chcete-li získat konkrétní webovou aplikaci, použijte:</span><span class="sxs-lookup"><span data-stu-id="998dd-161">To get a specific web app, use:</span></span>

    Get-AzureRmWebApp -Name ContosoWebApp

### <a name="configure-an-existing-web-app"></a><span data-ttu-id="998dd-162">Konfigurace existující webové aplikace</span><span class="sxs-lookup"><span data-stu-id="998dd-162">Configure an existing Web App</span></span>
<span data-ttu-id="998dd-163">Chcete-li změnit nastavení a konfigurace pro existující webovou aplikaci, použijte **Set-AzureRmWebApp** rutiny.</span><span class="sxs-lookup"><span data-stu-id="998dd-163">To change the settings and configurations for an existing web app, use the **Set-AzureRmWebApp** cmdlet.</span></span> <span data-ttu-id="998dd-164">Úplný seznam parametrů, najdete [rutiny odkazem](https://msdn.microsoft.com/library/mt652487.aspx)</span><span class="sxs-lookup"><span data-stu-id="998dd-164">For a full list of parameters, check the [Cmdlet reference link](https://msdn.microsoft.com/library/mt652487.aspx)</span></span>

<span data-ttu-id="998dd-165">Příklad (1): tuto rutinu použijte k změnit připojovací řetězce</span><span class="sxs-lookup"><span data-stu-id="998dd-165">Example (1): use this cmdlet to change connection strings</span></span>

    $connectionstrings = @{ ContosoConn1 = @{ Type = “MySql”; Value = “MySqlConn”}; ContosoConn2 = @{ Type = “SQLAzure”; Value = “SQLAzureConn”} }
    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -ConnectionStrings $connectionstrings

<span data-ttu-id="998dd-166">Příklad (2): Přidání nebo změna nastavení aplikace</span><span class="sxs-lookup"><span data-stu-id="998dd-166">Example (2): add or change app settings</span></span>

    $appsettings = @{appsetting1 = "appsetting1value"; appsetting2 = "appsetting2value"}
    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -AppSettings $appsettings


<span data-ttu-id="998dd-167">Příklad (3): nastavte webové aplikace na spouštění v režimu 64-bit</span><span class="sxs-lookup"><span data-stu-id="998dd-167">Example (3):  set the web app to run in 64-bit mode</span></span>

    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -Use32BitWorkerProcess $False

### <a name="change-the-state-of-an-existing-web-app"></a><span data-ttu-id="998dd-168">Změnit stav stávající webovou aplikaci</span><span class="sxs-lookup"><span data-stu-id="998dd-168">Change the state of an existing Web App</span></span>
#### <a name="restart-a-web-app"></a><span data-ttu-id="998dd-169">Restartování webové aplikace</span><span class="sxs-lookup"><span data-stu-id="998dd-169">Restart a web app</span></span>
<span data-ttu-id="998dd-170">Pokud chcete restartovat webovou aplikaci, musíte zadat název a prostředek skupiny webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="998dd-170">To restart a web app, you must specify the name and resource group of the web app.</span></span>

    Restart-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

#### <a name="stop-a-web-app"></a><span data-ttu-id="998dd-171">Zastavení webové aplikace</span><span class="sxs-lookup"><span data-stu-id="998dd-171">Stop a web app</span></span>
<span data-ttu-id="998dd-172">Zastavení webové aplikace, musíte zadat název a prostředek skupiny webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="998dd-172">To stop a web app, you must specify the name and resource group of the web app.</span></span>

    Stop-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

#### <a name="start-a-web-app"></a><span data-ttu-id="998dd-173">Spustí webovou aplikaci</span><span class="sxs-lookup"><span data-stu-id="998dd-173">Start a web app</span></span>
<span data-ttu-id="998dd-174">Chcete-li spustí webovou aplikaci, musíte zadat název a prostředek skupiny webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="998dd-174">To start a web app, you must specify the name and resource group of the web app.</span></span>

    Start-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

### <a name="manage-web-app-publishing-profiles"></a><span data-ttu-id="998dd-175">Správa profilů publikování webové aplikace</span><span class="sxs-lookup"><span data-stu-id="998dd-175">Manage Web App Publishing profiles</span></span>
<span data-ttu-id="998dd-176">Každý webové aplikace se profil publikování, kterou můžete použít k publikování aplikace, několik operací mohou být provedeny u profilů publikování.</span><span class="sxs-lookup"><span data-stu-id="998dd-176">Each web app has a publishing profile that can be used to publish your apps, several operations can be executed on publishing profiles.</span></span>

#### <a name="get-publishing-profile"></a><span data-ttu-id="998dd-177">Získat profil publikování</span><span class="sxs-lookup"><span data-stu-id="998dd-177">Get Publishing Profile</span></span>
<span data-ttu-id="998dd-178">Profil publikování pro webovou aplikaci, použijte:</span><span class="sxs-lookup"><span data-stu-id="998dd-178">To get the publishing profile for a web app, use:</span></span>

    Get-AzureRmWebAppPublishingProfile -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -OutputFile .\publishingprofile.txt

<span data-ttu-id="998dd-179">Tento příkaz vrátí profil publikování do příkazového řádku jako dobře výstupní profil publikování do textového souboru.</span><span class="sxs-lookup"><span data-stu-id="998dd-179">This command echoes the publishing profile to the command line as well output the publishing profile to a text file.</span></span>

#### <a name="reset-publishing-profile"></a><span data-ttu-id="998dd-180">Resetování profilu publikování</span><span class="sxs-lookup"><span data-stu-id="998dd-180">Reset Publishing Profile</span></span>
<span data-ttu-id="998dd-181">Obě resetovat heslo publikování FTP a webové nasadit pro webovou aplikaci, použijte:</span><span class="sxs-lookup"><span data-stu-id="998dd-181">To reset both the publishing password for FTP and web deploy for a web app, use:</span></span>

    Reset-AzureRmWebAppPublishingProfile -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

### <a name="manage-web-app-certificates"></a><span data-ttu-id="998dd-182">Spravovat certifikáty webové aplikace</span><span class="sxs-lookup"><span data-stu-id="998dd-182">Manage Web App Certificates</span></span>
<span data-ttu-id="998dd-183">Další informace o správě certifikátů webových aplikací najdete v tématu [vazby certifikátů SSL pomocí prostředí PowerShell](app-service-web-app-powershell-ssl-binding.md)</span><span class="sxs-lookup"><span data-stu-id="998dd-183">To learn about how to manage web app certificates, see [SSL Certificates binding using PowerShell](app-service-web-app-powershell-ssl-binding.md)</span></span>

### <a name="next-steps"></a><span data-ttu-id="998dd-184">Další kroky</span><span class="sxs-lookup"><span data-stu-id="998dd-184">Next Steps</span></span>
* <span data-ttu-id="998dd-185">Další informace o podpoře Azure Resource Manager PowerShell, najdete v části [použití Azure Powershellu s Azure Resource Manager.](../powershell-azure-resource-manager.md)</span><span class="sxs-lookup"><span data-stu-id="998dd-185">To learn about Azure Resource Manager PowerShell support, see [Using Azure PowerShell with Azure Resource Manager.](../powershell-azure-resource-manager.md)</span></span>
* <span data-ttu-id="998dd-186">Další informace o prostředí App Service najdete v tématu [Úvod do služby App Service Environment.](app-service-app-service-environment-intro.md)</span><span class="sxs-lookup"><span data-stu-id="998dd-186">To learn about App Service Environments, see [Introduction to App Service Environment.](app-service-app-service-environment-intro.md)</span></span>
* <span data-ttu-id="998dd-187">Další informace o správě certifikátů SSL aplikace služby pomocí prostředí PowerShell najdete v tématu [vazby certifikátů SSL pomocí prostředí PowerShell.](app-service-web-app-powershell-ssl-binding.md)</span><span class="sxs-lookup"><span data-stu-id="998dd-187">To learn about managing App Service SSL certificates using PowerShell, see [SSL Certificates binding using PowerShell.](app-service-web-app-powershell-ssl-binding.md)</span></span>
* <span data-ttu-id="998dd-188">Další informace o úplný seznam rutin prostředí PowerShell založené na správci prostředků Azure pro webové aplikace Azure najdete v tématu [Azure Cmdlet Reference rutin Powershellu webové aplikace Azure Resource Manager.](https://msdn.microsoft.com/library/mt619237.aspx)</span><span class="sxs-lookup"><span data-stu-id="998dd-188">To learn about the full list of Azure Resource Manager-based PowerShell cmdlets for Azure Web Apps, see [Azure Cmdlet Reference of Web Apps Azure Resource Manager PowerShell Cmdlets.](https://msdn.microsoft.com/library/mt619237.aspx)</span></span>
* * <span data-ttu-id="998dd-189">Další informace o správě služby App Service pomocí rozhraní příkazového řádku najdete v tématu [Using Azure Resource Manager-Based XPlat rozhraní příkazového řádku pro webové aplikace Azure.](app-service-web-app-azure-resource-manager-xplat-cli.md)</span><span class="sxs-lookup"><span data-stu-id="998dd-189">To learn about managing App Service using CLI, see [Using Azure Resource Manager-Based XPlat CLI for Azure Web App.](app-service-web-app-azure-resource-manager-xplat-cli.md)</span></span>

