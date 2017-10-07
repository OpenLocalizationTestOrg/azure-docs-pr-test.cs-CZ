---
title: "aaaAzure nástroje příkazového řádku založené na správci prostředků a platformy pro webové aplikace Azure | Microsoft Docs"
description: "Zjistěte, jak toouse hello nové nástroje příkazového řádku založené na Azure Resource Manager napříč platformami toomanage webových aplikacích Azure."
services: app-service\web
documentationcenter: 
author: ahmedelnably
manager: stefsch
editor: 
ms.assetid: d415b195-4262-416f-b59f-7e1aef200054
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/29/2016
ms.author: aelnably
ms.openlocfilehash: 5f5e03edcb01154aef3bd220cd27358f69656ef4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-azure-resource-manager-based-xplat-cli-for-azure-app-service"></a><span data-ttu-id="9e4b7-103">Pomocí příkazového řádku XPlat založené na správci prostředků Azure pro službu Azure App Service</span><span class="sxs-lookup"><span data-stu-id="9e4b7-103">Using Azure Resource Manager-Based XPlat CLI for Azure App Service</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="9e4b7-104">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="9e4b7-104">Azure CLI</span></span>](app-service-web-app-azure-resource-manager-xplat-cli.md)
> * [<span data-ttu-id="9e4b7-105">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="9e4b7-105">Azure PowerShell</span></span>](app-service-web-app-azure-resource-manager-powershell.md)

<span data-ttu-id="9e4b7-106">Verze hello nástroje příkazového řádku pro různé platformy Microsoft Azure verze 0.10.5 byly přidány nové příkazy.</span><span class="sxs-lookup"><span data-stu-id="9e4b7-106">With hello release of Microsoft Azure Cross-platform Command-Line Tools version 0.10.5, new commands have been added.</span></span> <span data-ttu-id="9e4b7-107">Tyto příkazy poskytnout hello uživatele hello možnost toouse založené na správci prostředků Azure PowerShell příkazy toomanage služby App Service.</span><span class="sxs-lookup"><span data-stu-id="9e4b7-107">These commands give hello user hello ability toouse Azure Resource Manager-based PowerShell commands toomanage App Service.</span></span>

<span data-ttu-id="9e4b7-108">toolearn o správě skupin prostředků, najdete v části [pomocí rozhraní příkazového řádku Azure toomanage hello Azure skupiny prostředků a prostředky](../azure-resource-manager/xplat-cli-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="9e4b7-108">toolearn about managing Resource Groups, see [Use hello Azure CLI toomanage Azure resources and resource groups](../azure-resource-manager/xplat-cli-azure-resource-manager.md).</span></span> 

> [!NOTE] 
> <span data-ttu-id="9e4b7-109">Také si vyzkoušet [Azure CLI 2.0](https://github.com/Azure/azure-cli), rozhraní příkazového řádku příští generace, napsané v Pythonu pro model nasazení správy prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="9e4b7-109">Also, try out [Azure CLI 2.0](https://github.com/Azure/azure-cli), a next-generation CLI written in Python for hello resource management deployment model.</span></span>
>
>

## <a name="managing-app-service-plans"></a><span data-ttu-id="9e4b7-110">Správa služby App Service plány</span><span class="sxs-lookup"><span data-stu-id="9e4b7-110">Managing App Service Plans</span></span>
### <a name="create-an-app-service-plan"></a><span data-ttu-id="9e4b7-111">Vytvořit plán služby App Service</span><span class="sxs-lookup"><span data-stu-id="9e4b7-111">Create an App Service Plan</span></span>
<span data-ttu-id="9e4b7-112">toocreate plán služby app service, použijte hello **vytvořit azure appserviceplan** příkaz.</span><span class="sxs-lookup"><span data-stu-id="9e4b7-112">toocreate an app service plan, use hello **azure appserviceplan create** command.</span></span>

<span data-ttu-id="9e4b7-113">Následují popisy hello různé parametry:</span><span class="sxs-lookup"><span data-stu-id="9e4b7-113">Following are descriptions of hello different parameters:</span></span>

* <span data-ttu-id="9e4b7-114">**--resource-group**: skupinu prostředků, která zahrnuje hello nově vytvořený plán služby app service.</span><span class="sxs-lookup"><span data-stu-id="9e4b7-114">**--resource-group**: resource group that includes hello newly created app service plan.</span></span>
* <span data-ttu-id="9e4b7-115">**– název**: název hello plán služby app service.</span><span class="sxs-lookup"><span data-stu-id="9e4b7-115">**--name**: name of hello app service plan.</span></span>
* <span data-ttu-id="9e4b7-116">**--umístění**: umístění plánu služby app.</span><span class="sxs-lookup"><span data-stu-id="9e4b7-116">**--location**: app service plan location.</span></span>
* <span data-ttu-id="9e4b7-117">**--sku**: hello potřeby cenovou sku (hello možnosti jsou: F1 (zdarma).</span><span class="sxs-lookup"><span data-stu-id="9e4b7-117">**--sku**:  hello desired pricing sku (hello options are: F1 (Free).</span></span> <span data-ttu-id="9e4b7-118">D1 (sdílené).</span><span class="sxs-lookup"><span data-stu-id="9e4b7-118">D1 (Shared).</span></span> <span data-ttu-id="9e4b7-119">B1 (základní malá), B2 (základní střední) a B3 (Basic velké).</span><span class="sxs-lookup"><span data-stu-id="9e4b7-119">B1 (Basic Small), B2 (Basic Medium), and B3 (Basic Large).</span></span> <span data-ttu-id="9e4b7-120">S1 (standardní malé), S2 (standardní střední) a S3 (standardní velké).</span><span class="sxs-lookup"><span data-stu-id="9e4b7-120">S1 (Standard Small), S2 (Standard Medium), and S3 (Standard Large).</span></span> <span data-ttu-id="9e4b7-121">P1 (malé Premium), P2 (střední Premium) a P3 (velké Premium).)</span><span class="sxs-lookup"><span data-stu-id="9e4b7-121">P1 (Premium Small), P2 (Premium Medium), and P3 (Premium Large).)</span></span>
* <span data-ttu-id="9e4b7-122">**--instancí**: hello počet pracovních procesů v plánu služby app hello (výchozí hodnota je 1).</span><span class="sxs-lookup"><span data-stu-id="9e4b7-122">**--instances**: hello number of workers in hello app service plan (Default value is 1).</span></span>

<span data-ttu-id="9e4b7-123">Příklad toouse tuto rutinu:</span><span class="sxs-lookup"><span data-stu-id="9e4b7-123">Example toouse this cmdlet:</span></span>

    azure appserviceplan create --name ContosoAppServicePlan --location "South Central US" --resource-group ContosoAzureResourceGroup --sku P1 --instances 10

#### <a name="create-a-linux-app-service-plan"></a><span data-ttu-id="9e4b7-124">Vytvoření plánu služby App Service pro Linux</span><span class="sxs-lookup"><span data-stu-id="9e4b7-124">Create a Linux App Service Plan</span></span>

<span data-ttu-id="9e4b7-125">Pomocí stejné hello **vytvořit azure appserviceplan** příkazu s hello speciálním parametrem **– islinux true**.</span><span class="sxs-lookup"><span data-stu-id="9e4b7-125">Using hello same **azure appserviceplan create** command, with hello extra parameter **--islinux true**.</span></span> <span data-ttu-id="9e4b7-126">Všimněte si hello omezení a oblastí, které jsou popsané v [Úvod tooApp služby v systému Linux](app-service-linux-intro.md)</span><span class="sxs-lookup"><span data-stu-id="9e4b7-126">Note hello restrictions and regions outlined in [Introduction tooApp Service on Linux](app-service-linux-intro.md)</span></span>

### <a name="list-existing-app-service-plans"></a><span data-ttu-id="9e4b7-127">Seznam existující plán služby App Service</span><span class="sxs-lookup"><span data-stu-id="9e4b7-127">List Existing App Service Plans</span></span>
<span data-ttu-id="9e4b7-128">používat toolist hello existující aplikace služby plány **seznamu azure appserviceplan** příkaz.</span><span class="sxs-lookup"><span data-stu-id="9e4b7-128">toolist hello existing app service plans, use **azure appserviceplan list** command.</span></span>

<span data-ttu-id="9e4b7-129">toolist všechny plány služby app v určité skupiny zdrojů, použijte:</span><span class="sxs-lookup"><span data-stu-id="9e4b7-129">toolist all app service plans under a specific resource group, use:</span></span>

    azure appserviceplan list --resource-group ContosoAzureResourceGroup

<span data-ttu-id="9e4b7-130">tooget plán služeb konkrétní aplikaci, použijte **zobrazit azure appserviceplan** příkaz:</span><span class="sxs-lookup"><span data-stu-id="9e4b7-130">tooget a specific app service plan, use **azure appserviceplan show** command:</span></span>

    azure appserviceplan show --name ContosoAppServicePlan --resource-group southeastasia

### <a name="configure-an-existing-app-service-plan"></a><span data-ttu-id="9e4b7-131">Konfigurace existující plán služby App Service</span><span class="sxs-lookup"><span data-stu-id="9e4b7-131">Configure an existing App Service Plan</span></span>
<span data-ttu-id="9e4b7-132">toochange hello nastavení existujícího plánu služby app, použijte hello **konfigurace azure appserviceplan** příkaz.</span><span class="sxs-lookup"><span data-stu-id="9e4b7-132">toochange hello settings for an existing app service plan, use hello **azure appserviceplan config** command.</span></span> <span data-ttu-id="9e4b7-133">Můžete změnit hello sku a hello počet pracovních procesů</span><span class="sxs-lookup"><span data-stu-id="9e4b7-133">You can change hello sku, and hello number of workers</span></span> 

    azure appserviceplan config --name ContosoAppServicePlan --resource-group ContosoAzureResourceGroup --sku S1 --instances 9

#### <a name="scaling-an-app-service-plan"></a><span data-ttu-id="9e4b7-134">Škálování plán služby App Service</span><span class="sxs-lookup"><span data-stu-id="9e4b7-134">Scaling an App Service Plan</span></span>
<span data-ttu-id="9e4b7-135">tooscale existující plán služby App Service, použijte:</span><span class="sxs-lookup"><span data-stu-id="9e4b7-135">tooscale an existing App Service Plan, use:</span></span>

    azure appserviceplan config --name ContosoAppServicePlan --resource-group ContosoAzureResourceGroup --instances 9

#### <a name="changing-hello-sku-of-an-app-service-plan"></a><span data-ttu-id="9e4b7-136">Změna hello skladová položka plánu služby App Service</span><span class="sxs-lookup"><span data-stu-id="9e4b7-136">Changing hello SKU of an App Service Plan</span></span>
<span data-ttu-id="9e4b7-137">toochange skladová položka hello existující plán služby App Service, použijte:</span><span class="sxs-lookup"><span data-stu-id="9e4b7-137">toochange hello sku of an existing App Service Plan, use:</span></span>

    azure appserviceplan config --name ContosoAppServicePlan --resource-group ContosoAzureResourceGroup --sku S1


### <a name="delete-an-existing-app-service-plan"></a><span data-ttu-id="9e4b7-138">Odstranit existující plán služby App Service</span><span class="sxs-lookup"><span data-stu-id="9e4b7-138">Delete an existing App Service Plan</span></span>
<span data-ttu-id="9e4b7-139">toodelete existující plán služby app service, všechny přiřazené aplikace potřeba toobe nepřesunul nebo neodstranil nejdřív.</span><span class="sxs-lookup"><span data-stu-id="9e4b7-139">toodelete an existing app service plan, all assigned apps need toobe moved or deleted first.</span></span> <span data-ttu-id="9e4b7-140">Potom pomocí hello **odstranit azure webapp** příkaz odstraníte hello plán služby app service.</span><span class="sxs-lookup"><span data-stu-id="9e4b7-140">Then using hello **azure webapp delete** command you can delete hello app service plan.</span></span>

    azure appserviceplan delete --name ContosoAppServicePlan --resource-group southeastasia

## <a name="managing-app-service-apps"></a><span data-ttu-id="9e4b7-141">Správa aplikací App Service</span><span class="sxs-lookup"><span data-stu-id="9e4b7-141">Managing App Service apps</span></span>
### <a name="create-a-web-app"></a><span data-ttu-id="9e4b7-142">Vytvoření webové aplikace</span><span class="sxs-lookup"><span data-stu-id="9e4b7-142">Create a web app</span></span>
<span data-ttu-id="9e4b7-143">toocreate webové aplikace, použijte hello **vytvořit azure webapp** příkaz.</span><span class="sxs-lookup"><span data-stu-id="9e4b7-143">toocreate a web app, use hello **azure webapp create** command.</span></span>

<span data-ttu-id="9e4b7-144">Následují popisy hello různé parametry:</span><span class="sxs-lookup"><span data-stu-id="9e4b7-144">Following are descriptions of hello different parameters:</span></span>

* <span data-ttu-id="9e4b7-145">**– název**: název webové aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="9e4b7-145">**--name**: name for hello web app.</span></span>
* <span data-ttu-id="9e4b7-146">**--plán**: název pro plán služby hello použít toohost hello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="9e4b7-146">**--plan**: name for hello service plan used toohost hello web app.</span></span>
* <span data-ttu-id="9e4b7-147">**--resource-group**: skupinu prostředků, který je hostitelem hello plán služby App service.</span><span class="sxs-lookup"><span data-stu-id="9e4b7-147">**--resource-group**: resource group that hosts hello App service plan.</span></span>
* <span data-ttu-id="9e4b7-148">**--umístění**: hello umístění webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="9e4b7-148">**--location**: hello web app location.</span></span>

<span data-ttu-id="9e4b7-149">Příklad toouse tuto rutinu:</span><span class="sxs-lookup"><span data-stu-id="9e4b7-149">Example toouse this cmdlet:</span></span>

    azure webapp create --name ContosoWebApp --resource-group ContosoAzureResourceGroup --plan ContosoAppServicePlan --location "South Central US"

### <a name="delete-an-existing-app"></a><span data-ttu-id="9e4b7-150">Odstraňte existující aplikace</span><span class="sxs-lookup"><span data-stu-id="9e4b7-150">Delete an existing app</span></span>
<span data-ttu-id="9e4b7-151">toodelete existující aplikaci, můžete použít hello **odstranit azure webapp** příkaz.</span><span class="sxs-lookup"><span data-stu-id="9e4b7-151">toodelete an existing app, you can use hello **azure webapp delete** command.</span></span> <span data-ttu-id="9e4b7-152">Potřebujete toospecify hello název aplikace hello a název skupiny prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="9e4b7-152">You need toospecify hello name of hello app and hello resource group name.</span></span>

    azure webapp delete --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="list-existing-apps"></a><span data-ttu-id="9e4b7-153">Zobrazí seznam stávajících aplikací</span><span class="sxs-lookup"><span data-stu-id="9e4b7-153">List existing apps</span></span>
<span data-ttu-id="9e4b7-154">toolist hello existující aplikace, použijte hello **azure webapp seznamu** příkaz.</span><span class="sxs-lookup"><span data-stu-id="9e4b7-154">toolist hello existing apps, use hello **azure webapp list** command.</span></span>

<span data-ttu-id="9e4b7-155">toolist všechny aplikace v určité skupiny zdrojů, použijte:</span><span class="sxs-lookup"><span data-stu-id="9e4b7-155">toolist all apps in a specific resource group, use:</span></span>

    azure webapp list --resource-group ContosoAzureResourceGroup

<span data-ttu-id="9e4b7-156">tooget konkrétní aplikaci, použijte hello **azure webapp zobrazit** příkaz.</span><span class="sxs-lookup"><span data-stu-id="9e4b7-156">tooget a specific app, use hello **azure webapp show** command.</span></span>

    azure webapp show --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="configure-an-existing-app"></a><span data-ttu-id="9e4b7-157">Konfigurace existující aplikace</span><span class="sxs-lookup"><span data-stu-id="9e4b7-157">Configure an existing app</span></span>
<span data-ttu-id="9e4b7-158">toochange hello nastavení a konfigurace pro existující aplikace, použijte hello **set config azure webapp** příkaz.</span><span class="sxs-lookup"><span data-stu-id="9e4b7-158">toochange hello settings and configurations for an existing app, use hello **azure webapp config set** command.</span></span>

<span data-ttu-id="9e4b7-159">Příklad (1): Změňte hello verzi php aplikace</span><span class="sxs-lookup"><span data-stu-id="9e4b7-159">Example (1): change hello php version of a app</span></span> 

    azure webapp config set --name ContosoWebApp --resource-group ContosoAzureResourceGroup --phpversion 5.6

<span data-ttu-id="9e4b7-160">Příklad (2): Přidání nebo změna nastavení aplikace</span><span class="sxs-lookup"><span data-stu-id="9e4b7-160">Example (2): add or change app settings</span></span>

    webapp config appsettings set --name ContosoWebApp --resource-group ContosoAzureResourceGroup appsetting1=appsetting1value,appsetting2=appsetting2value

<span data-ttu-id="9e4b7-161">tooknow ostatní konfigurace, které je možné změnit, použijte hello **konfigurace azure webapp nastavit -h** příkaz.</span><span class="sxs-lookup"><span data-stu-id="9e4b7-161">tooknow what other configuration can be changed, use hello **azure webapp config set -h** command.</span></span>

### <a name="change-hello-state-of-an-existing-app"></a><span data-ttu-id="9e4b7-162">Změnit stav hello existující aplikace</span><span class="sxs-lookup"><span data-stu-id="9e4b7-162">Change hello state of an existing app</span></span>
#### <a name="restart-an-app"></a><span data-ttu-id="9e4b7-163">Restartujte aplikace</span><span class="sxs-lookup"><span data-stu-id="9e4b7-163">Restart an app</span></span>
<span data-ttu-id="9e4b7-164">toorestart na aplikace, je nutné zadat název a prostředek skupiny hello aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="9e4b7-164">toorestart an app, you must specify hello name and resource group of hello app.</span></span>

    azure webapp restart --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="stop-an-app"></a><span data-ttu-id="9e4b7-165">Zastavte aplikaci</span><span class="sxs-lookup"><span data-stu-id="9e4b7-165">Stop an app</span></span>
<span data-ttu-id="9e4b7-166">toostop na aplikace, je nutné zadat název a prostředek skupiny hello aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="9e4b7-166">toostop an app, you must specify hello name and resource group of hello app.</span></span>

    azure webapp stop --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="start-an-app"></a><span data-ttu-id="9e4b7-167">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="9e4b7-167">Start an app</span></span>
<span data-ttu-id="9e4b7-168">toostart na aplikace, je nutné zadat název a prostředek skupiny hello aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="9e4b7-168">toostart an app, you must specify hello name and resource group of hello app.</span></span>

    azure webapp start --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="manage-publishing-profiles-of-an-app"></a><span data-ttu-id="9e4b7-169">Správa profilů publikování aplikace</span><span class="sxs-lookup"><span data-stu-id="9e4b7-169">Manage publishing profiles of an app</span></span>
<span data-ttu-id="9e4b7-170">Každá aplikace má profil publikování, které můžou být použité toopublish vašeho kódu.</span><span class="sxs-lookup"><span data-stu-id="9e4b7-170">Each app has a publishing profile that can be used toopublish your code.</span></span>

#### <a name="get-publishing-profile"></a><span data-ttu-id="9e4b7-171">Získat profil publikování</span><span class="sxs-lookup"><span data-stu-id="9e4b7-171">Get Publishing Profile</span></span>
<span data-ttu-id="9e4b7-172">hello tooget publikování pro aplikaci, použijte profil:</span><span class="sxs-lookup"><span data-stu-id="9e4b7-172">tooget hello publishing profile for a app, use:</span></span>

    azure webapp publishingprofile --name ContosoWebApp --resource-group ContosoAzureResourceGroup

<span data-ttu-id="9e4b7-173">Tento příkaz vrátí hello publikování profil uživatelské jméno a heslo toohello příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="9e4b7-173">This command echoes hello publishing profile username and password toohello command line.</span></span>

### <a name="manage-app-hostnames"></a><span data-ttu-id="9e4b7-174">Spravovat aplikace názvy hostitelů</span><span class="sxs-lookup"><span data-stu-id="9e4b7-174">Manage app hostnames</span></span>
<span data-ttu-id="9e4b7-175">vazby názvů hostitelů toomanage pro vaši aplikaci, použijte hello **názvy hostitelů konfigurace azure webapp** příkaz</span><span class="sxs-lookup"><span data-stu-id="9e4b7-175">toomanage hostname bindings for your app, use hello **azure webapp config hostnames** command</span></span>  

#### <a name="list-hostname-bindings"></a><span data-ttu-id="9e4b7-176">Seznam vazby názvů hostitelů.</span><span class="sxs-lookup"><span data-stu-id="9e4b7-176">List hostname bindings</span></span>
<span data-ttu-id="9e4b7-177">tooget hello vazby na aktuální název hostitele pro aplikaci, použijte:</span><span class="sxs-lookup"><span data-stu-id="9e4b7-177">tooget hello current hostname bindings for an app, use:</span></span>

    azure webapp config hostnames list --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="add-hostname-bindings"></a><span data-ttu-id="9e4b7-178">Přidat vazby názvů hostitelů.</span><span class="sxs-lookup"><span data-stu-id="9e4b7-178">Add hostname bindings</span></span>
<span data-ttu-id="9e4b7-179">tooadd hostname vazby tooan aplikace, použijte:</span><span class="sxs-lookup"><span data-stu-id="9e4b7-179">tooadd hostname bindings tooan app, use:</span></span>

    azure webapp config hostnames add --name ContosoWebApp --resource-group ContosoAzureResourceGroup --hostname www.contoso.com

#### <a name="delete-hostname-bindings"></a><span data-ttu-id="9e4b7-180">Odstranit vazby názvů hostitelů.</span><span class="sxs-lookup"><span data-stu-id="9e4b7-180">Delete hostname bindings</span></span>
<span data-ttu-id="9e4b7-181">toodelete vazby názvů hostitelů, použijte:</span><span class="sxs-lookup"><span data-stu-id="9e4b7-181">toodelete hostname bindings, use:</span></span>

    azure webapp config hostnames delete --name ContosoWebApp --resource-group ContosoAzureResourceGroup --hostname www.contoso.com

## <a name="next-steps"></a><span data-ttu-id="9e4b7-182">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9e4b7-182">Next Steps</span></span>
* <span data-ttu-id="9e4b7-183">toolearn týkající se podpory rozhraní příkazového řádku Azure Resource Manager, najdete v části [pomocí rozhraní příkazového řádku Azure toomanage hello Azure skupiny prostředků a prostředky.](../azure-resource-manager/xplat-cli-azure-resource-manager.md)</span><span class="sxs-lookup"><span data-stu-id="9e4b7-183">toolearn about Azure Resource Manager CLI support, see [Use hello Azure CLI toomanage Azure resources and resource groups.](../azure-resource-manager/xplat-cli-azure-resource-manager.md)</span></span>
* <span data-ttu-id="9e4b7-184">toolearn o správě služby App Service pomocí prostředí PowerShell, najdete v části [tooManage Using Azure Resource Manager-Based prostředí PowerShell Azure Web Apps.](app-service-web-app-azure-resource-manager-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="9e4b7-184">toolearn about managing App Service using PowerShell, see [Using Azure Resource Manager-Based PowerShell tooManage Azure Web Apps.](app-service-web-app-azure-resource-manager-powershell.md)</span></span>
* <span data-ttu-id="9e4b7-185">toolearn o službě Azure App Service pro systémy Linux, najdete v části [Úvod tooApp služby v systému Linux](app-service-linux-intro.md)</span><span class="sxs-lookup"><span data-stu-id="9e4b7-185">toolearn about Azure App Service on Linux, see [Introduction tooApp Service on Linux](app-service-linux-intro.md)</span></span>
