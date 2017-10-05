---
title: "Webová aplikace Azure založené na správci prostředků mezi rozhraní příkazového řádku nástroje pro Azure | Microsoft Docs"
description: "Naučte se používat nový na základě Azure Resource Manager napříč platformami příkazového řádku nástroje pro správu webových aplikacích Azure."
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
ms.openlocfilehash: 7a03e1417617453c43edcc3787da10d171359757
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="using-azure-resource-manager-based-xplat-cli-for-azure-app-service"></a><span data-ttu-id="fcebb-103">Pomocí příkazového řádku XPlat založené na správci prostředků Azure pro službu Azure App Service</span><span class="sxs-lookup"><span data-stu-id="fcebb-103">Using Azure Resource Manager-Based XPlat CLI for Azure App Service</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="fcebb-104">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="fcebb-104">Azure CLI</span></span>](app-service-web-app-azure-resource-manager-xplat-cli.md)
> * [<span data-ttu-id="fcebb-105">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="fcebb-105">Azure PowerShell</span></span>](app-service-web-app-azure-resource-manager-powershell.md)

<span data-ttu-id="fcebb-106">S vydáním verze nástroje příkazového řádku pro různé platformy Microsoft Azure 0.10.5 byly přidány nové příkazy.</span><span class="sxs-lookup"><span data-stu-id="fcebb-106">With the release of Microsoft Azure Cross-platform Command-Line Tools version 0.10.5, new commands have been added.</span></span> <span data-ttu-id="fcebb-107">Tyto příkazy uživateli přidělit možnost použít příkazy prostředí PowerShell využívající Azure Resource Manager ke správě služby App Service.</span><span class="sxs-lookup"><span data-stu-id="fcebb-107">These commands give the user the ability to use Azure Resource Manager-based PowerShell commands to manage App Service.</span></span>

<span data-ttu-id="fcebb-108">Další informace o správě skupin prostředků, najdete v části [používat rozhraní příkazového řádku Azure ke správě prostředků Azure a skupiny prostředků](../azure-resource-manager/xplat-cli-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="fcebb-108">To learn about managing Resource Groups, see [Use the Azure CLI to manage Azure resources and resource groups](../azure-resource-manager/xplat-cli-azure-resource-manager.md).</span></span> 

> [!NOTE] 
> <span data-ttu-id="fcebb-109">Také si vyzkoušet [Azure CLI 2.0](https://github.com/Azure/azure-cli), rozhraní příkazového řádku příští generace, napsané v Pythonu pro model nasazení prostředků správy.</span><span class="sxs-lookup"><span data-stu-id="fcebb-109">Also, try out [Azure CLI 2.0](https://github.com/Azure/azure-cli), a next-generation CLI written in Python for the resource management deployment model.</span></span>
>
>

## <a name="managing-app-service-plans"></a><span data-ttu-id="fcebb-110">Správa služby App Service plány</span><span class="sxs-lookup"><span data-stu-id="fcebb-110">Managing App Service Plans</span></span>
### <a name="create-an-app-service-plan"></a><span data-ttu-id="fcebb-111">Vytvořit plán služby App Service</span><span class="sxs-lookup"><span data-stu-id="fcebb-111">Create an App Service Plan</span></span>
<span data-ttu-id="fcebb-112">Chcete-li vytvořit plán služby app service, použijte **vytvořit azure appserviceplan** příkaz.</span><span class="sxs-lookup"><span data-stu-id="fcebb-112">To create an app service plan, use the **azure appserviceplan create** command.</span></span>

<span data-ttu-id="fcebb-113">Následují popisy různé parametry:</span><span class="sxs-lookup"><span data-stu-id="fcebb-113">Following are descriptions of the different parameters:</span></span>

* <span data-ttu-id="fcebb-114">**--resource-group**: skupinu prostředků, která zahrnuje tarifu nově vytvořené aplikace.</span><span class="sxs-lookup"><span data-stu-id="fcebb-114">**--resource-group**: resource group that includes the newly created app service plan.</span></span>
* <span data-ttu-id="fcebb-115">**– název**: název plánu služby app service.</span><span class="sxs-lookup"><span data-stu-id="fcebb-115">**--name**: name of the app service plan.</span></span>
* <span data-ttu-id="fcebb-116">**--umístění**: umístění plánu služby app.</span><span class="sxs-lookup"><span data-stu-id="fcebb-116">**--location**: app service plan location.</span></span>
* <span data-ttu-id="fcebb-117">**--sku**: požadovanou cenovou sku (Možnosti jsou: F1 (Free).</span><span class="sxs-lookup"><span data-stu-id="fcebb-117">**--sku**:  the desired pricing sku (The options are: F1 (Free).</span></span> <span data-ttu-id="fcebb-118">D1 (sdílené).</span><span class="sxs-lookup"><span data-stu-id="fcebb-118">D1 (Shared).</span></span> <span data-ttu-id="fcebb-119">B1 (základní malá), B2 (základní střední) a B3 (Basic velké).</span><span class="sxs-lookup"><span data-stu-id="fcebb-119">B1 (Basic Small), B2 (Basic Medium), and B3 (Basic Large).</span></span> <span data-ttu-id="fcebb-120">S1 (standardní malé), S2 (standardní střední) a S3 (standardní velké).</span><span class="sxs-lookup"><span data-stu-id="fcebb-120">S1 (Standard Small), S2 (Standard Medium), and S3 (Standard Large).</span></span> <span data-ttu-id="fcebb-121">P1 (malé Premium), P2 (střední Premium) a P3 (velké Premium).)</span><span class="sxs-lookup"><span data-stu-id="fcebb-121">P1 (Premium Small), P2 (Premium Medium), and P3 (Premium Large).)</span></span>
* <span data-ttu-id="fcebb-122">**--instancí**: počet pracovních procesů v aplikaci služby plán (výchozí hodnota je 1).</span><span class="sxs-lookup"><span data-stu-id="fcebb-122">**--instances**: the number of workers in the app service plan (Default value is 1).</span></span>

<span data-ttu-id="fcebb-123">Příklad pro použití této rutiny:</span><span class="sxs-lookup"><span data-stu-id="fcebb-123">Example to use this cmdlet:</span></span>

    azure appserviceplan create --name ContosoAppServicePlan --location "South Central US" --resource-group ContosoAzureResourceGroup --sku P1 --instances 10

#### <a name="create-a-linux-app-service-plan"></a><span data-ttu-id="fcebb-124">Vytvoření plánu služby App Service pro Linux</span><span class="sxs-lookup"><span data-stu-id="fcebb-124">Create a Linux App Service Plan</span></span>

<span data-ttu-id="fcebb-125">Používající stejný **vytvořit azure appserviceplan** příkazu se speciálním parametrem **– islinux true**.</span><span class="sxs-lookup"><span data-stu-id="fcebb-125">Using the same **azure appserviceplan create** command, with the extra parameter **--islinux true**.</span></span> <span data-ttu-id="fcebb-126">Všimněte si, omezení a oblastí, které jsou popsané v [Úvod do služby App Service v systému Linux](app-service-linux-intro.md)</span><span class="sxs-lookup"><span data-stu-id="fcebb-126">Note the restrictions and regions outlined in [Introduction to App Service on Linux](app-service-linux-intro.md)</span></span>

### <a name="list-existing-app-service-plans"></a><span data-ttu-id="fcebb-127">Seznam existující plán služby App Service</span><span class="sxs-lookup"><span data-stu-id="fcebb-127">List Existing App Service Plans</span></span>
<span data-ttu-id="fcebb-128">K zobrazení seznamu existující plány služby aplikace, použijte **seznamu azure appserviceplan** příkaz.</span><span class="sxs-lookup"><span data-stu-id="fcebb-128">To list the existing app service plans, use **azure appserviceplan list** command.</span></span>

<span data-ttu-id="fcebb-129">K zobrazení seznamu všech plánů služby app podle určité skupiny zdrojů, použijte:</span><span class="sxs-lookup"><span data-stu-id="fcebb-129">To list all app service plans under a specific resource group, use:</span></span>

    azure appserviceplan list --resource-group ContosoAzureResourceGroup

<span data-ttu-id="fcebb-130">Plán služby konkrétní aplikaci, použijte **zobrazit azure appserviceplan** příkaz:</span><span class="sxs-lookup"><span data-stu-id="fcebb-130">To get a specific app service plan, use **azure appserviceplan show** command:</span></span>

    azure appserviceplan show --name ContosoAppServicePlan --resource-group southeastasia

### <a name="configure-an-existing-app-service-plan"></a><span data-ttu-id="fcebb-131">Konfigurace existující plán služby App Service</span><span class="sxs-lookup"><span data-stu-id="fcebb-131">Configure an existing App Service Plan</span></span>
<span data-ttu-id="fcebb-132">Chcete-li změnit nastavení existujícího plánu služby app, použijte **konfigurace azure appserviceplan** příkaz.</span><span class="sxs-lookup"><span data-stu-id="fcebb-132">To change the settings for an existing app service plan, use the **azure appserviceplan config** command.</span></span> <span data-ttu-id="fcebb-133">Můžete změnit sku a počet pracovních procesů</span><span class="sxs-lookup"><span data-stu-id="fcebb-133">You can change the sku, and the number of workers</span></span> 

    azure appserviceplan config --name ContosoAppServicePlan --resource-group ContosoAzureResourceGroup --sku S1 --instances 9

#### <a name="scaling-an-app-service-plan"></a><span data-ttu-id="fcebb-134">Škálování plán služby App Service</span><span class="sxs-lookup"><span data-stu-id="fcebb-134">Scaling an App Service Plan</span></span>
<span data-ttu-id="fcebb-135">Pokud chcete použít škálování, existující plán služby App Service, použijte:</span><span class="sxs-lookup"><span data-stu-id="fcebb-135">To scale an existing App Service Plan, use:</span></span>

    azure appserviceplan config --name ContosoAppServicePlan --resource-group ContosoAzureResourceGroup --instances 9

#### <a name="changing-the-sku-of-an-app-service-plan"></a><span data-ttu-id="fcebb-136">Změna skladová položka plán služby App Service</span><span class="sxs-lookup"><span data-stu-id="fcebb-136">Changing the SKU of an App Service Plan</span></span>
<span data-ttu-id="fcebb-137">Chcete-li změnit sku systému existující plán služby App Service, použijte:</span><span class="sxs-lookup"><span data-stu-id="fcebb-137">To change the sku of an existing App Service Plan, use:</span></span>

    azure appserviceplan config --name ContosoAppServicePlan --resource-group ContosoAzureResourceGroup --sku S1


### <a name="delete-an-existing-app-service-plan"></a><span data-ttu-id="fcebb-138">Odstranit existující plán služby App Service</span><span class="sxs-lookup"><span data-stu-id="fcebb-138">Delete an existing App Service Plan</span></span>
<span data-ttu-id="fcebb-139">Pokud chcete odstranit existující plán služby app service, musí všechny aplikace přiřazené přesunout ani neodstraní.</span><span class="sxs-lookup"><span data-stu-id="fcebb-139">To delete an existing app service plan, all assigned apps need to be moved or deleted first.</span></span> <span data-ttu-id="fcebb-140">Potom pomocí **odstranit azure webapp** příkaz můžete odstranit plán služby app service.</span><span class="sxs-lookup"><span data-stu-id="fcebb-140">Then using the **azure webapp delete** command you can delete the app service plan.</span></span>

    azure appserviceplan delete --name ContosoAppServicePlan --resource-group southeastasia

## <a name="managing-app-service-apps"></a><span data-ttu-id="fcebb-141">Správa aplikací App Service</span><span class="sxs-lookup"><span data-stu-id="fcebb-141">Managing App Service apps</span></span>
### <a name="create-a-web-app"></a><span data-ttu-id="fcebb-142">Vytvoření webové aplikace</span><span class="sxs-lookup"><span data-stu-id="fcebb-142">Create a web app</span></span>
<span data-ttu-id="fcebb-143">Chcete-li vytvořit webovou aplikaci, použijte **vytvořit azure webapp** příkaz.</span><span class="sxs-lookup"><span data-stu-id="fcebb-143">To create a web app, use the **azure webapp create** command.</span></span>

<span data-ttu-id="fcebb-144">Následují popisy různé parametry:</span><span class="sxs-lookup"><span data-stu-id="fcebb-144">Following are descriptions of the different parameters:</span></span>

* <span data-ttu-id="fcebb-145">**– název**: název webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="fcebb-145">**--name**: name for the web app.</span></span>
* <span data-ttu-id="fcebb-146">**--plán**: název plánu služby použít k hostování webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="fcebb-146">**--plan**: name for the service plan used to host the web app.</span></span>
* <span data-ttu-id="fcebb-147">**--resource-group**: skupinu prostředků, který je hostitelem plán služby App service.</span><span class="sxs-lookup"><span data-stu-id="fcebb-147">**--resource-group**: resource group that hosts the App service plan.</span></span>
* <span data-ttu-id="fcebb-148">**--umístění**: umístění webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="fcebb-148">**--location**: the web app location.</span></span>

<span data-ttu-id="fcebb-149">Příklad pro použití této rutiny:</span><span class="sxs-lookup"><span data-stu-id="fcebb-149">Example to use this cmdlet:</span></span>

    azure webapp create --name ContosoWebApp --resource-group ContosoAzureResourceGroup --plan ContosoAppServicePlan --location "South Central US"

### <a name="delete-an-existing-app"></a><span data-ttu-id="fcebb-150">Odstraňte existující aplikace</span><span class="sxs-lookup"><span data-stu-id="fcebb-150">Delete an existing app</span></span>
<span data-ttu-id="fcebb-151">Chcete-li odstranit existující aplikaci, můžete použít **odstranit azure webapp** příkaz.</span><span class="sxs-lookup"><span data-stu-id="fcebb-151">To delete an existing app, you can use the **azure webapp delete** command.</span></span> <span data-ttu-id="fcebb-152">Je třeba zadat název aplikace a název skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="fcebb-152">You need to specify the name of the app and the resource group name.</span></span>

    azure webapp delete --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="list-existing-apps"></a><span data-ttu-id="fcebb-153">Zobrazí seznam stávajících aplikací</span><span class="sxs-lookup"><span data-stu-id="fcebb-153">List existing apps</span></span>
<span data-ttu-id="fcebb-154">K zobrazení seznamu existující aplikace, použijte **azure webapp seznamu** příkaz.</span><span class="sxs-lookup"><span data-stu-id="fcebb-154">To list the existing apps, use the **azure webapp list** command.</span></span>

<span data-ttu-id="fcebb-155">K zobrazení seznamu všech aplikací v určité skupiny zdrojů, použijte:</span><span class="sxs-lookup"><span data-stu-id="fcebb-155">To list all apps in a specific resource group, use:</span></span>

    azure webapp list --resource-group ContosoAzureResourceGroup

<span data-ttu-id="fcebb-156">Chcete-li získat konkrétní aplikaci, použijte **azure webapp zobrazit** příkaz.</span><span class="sxs-lookup"><span data-stu-id="fcebb-156">To get a specific app, use the **azure webapp show** command.</span></span>

    azure webapp show --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="configure-an-existing-app"></a><span data-ttu-id="fcebb-157">Konfigurace existující aplikace</span><span class="sxs-lookup"><span data-stu-id="fcebb-157">Configure an existing app</span></span>
<span data-ttu-id="fcebb-158">Chcete-li změnit nastavení a konfigurace pro existující aplikace, použijte **set config azure webapp** příkaz.</span><span class="sxs-lookup"><span data-stu-id="fcebb-158">To change the settings and configurations for an existing app, use the **azure webapp config set** command.</span></span>

<span data-ttu-id="fcebb-159">Příklad (1): změňte verzi php aplikace</span><span class="sxs-lookup"><span data-stu-id="fcebb-159">Example (1): change the php version of a app</span></span> 

    azure webapp config set --name ContosoWebApp --resource-group ContosoAzureResourceGroup --phpversion 5.6

<span data-ttu-id="fcebb-160">Příklad (2): Přidání nebo změna nastavení aplikace</span><span class="sxs-lookup"><span data-stu-id="fcebb-160">Example (2): add or change app settings</span></span>

    webapp config appsettings set --name ContosoWebApp --resource-group ContosoAzureResourceGroup appsetting1=appsetting1value,appsetting2=appsetting2value

<span data-ttu-id="fcebb-161">Potřebujete vědět, co může být jiná konfigurace změnit, použijte **konfigurace azure webapp nastavit -h** příkaz.</span><span class="sxs-lookup"><span data-stu-id="fcebb-161">To know what other configuration can be changed, use the **azure webapp config set -h** command.</span></span>

### <a name="change-the-state-of-an-existing-app"></a><span data-ttu-id="fcebb-162">Změnit stav existující aplikace</span><span class="sxs-lookup"><span data-stu-id="fcebb-162">Change the state of an existing app</span></span>
#### <a name="restart-an-app"></a><span data-ttu-id="fcebb-163">Restartujte aplikace</span><span class="sxs-lookup"><span data-stu-id="fcebb-163">Restart an app</span></span>
<span data-ttu-id="fcebb-164">Pokud chcete restartovat aplikaci, musíte zadat název a prostředek skupiny aplikace.</span><span class="sxs-lookup"><span data-stu-id="fcebb-164">To restart an app, you must specify the name and resource group of the app.</span></span>

    azure webapp restart --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="stop-an-app"></a><span data-ttu-id="fcebb-165">Zastavte aplikaci</span><span class="sxs-lookup"><span data-stu-id="fcebb-165">Stop an app</span></span>
<span data-ttu-id="fcebb-166">Chcete-li zastavit aplikaci, musíte zadat skupině název a prostředků pro aplikace.</span><span class="sxs-lookup"><span data-stu-id="fcebb-166">To stop an app, you must specify the name and resource group of the app.</span></span>

    azure webapp stop --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="start-an-app"></a><span data-ttu-id="fcebb-167">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="fcebb-167">Start an app</span></span>
<span data-ttu-id="fcebb-168">Chcete-li spustit aplikaci, musíte zadat skupině název a prostředků pro aplikace.</span><span class="sxs-lookup"><span data-stu-id="fcebb-168">To start an app, you must specify the name and resource group of the app.</span></span>

    azure webapp start --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="manage-publishing-profiles-of-an-app"></a><span data-ttu-id="fcebb-169">Správa profilů publikování aplikace</span><span class="sxs-lookup"><span data-stu-id="fcebb-169">Manage publishing profiles of an app</span></span>
<span data-ttu-id="fcebb-170">Každá aplikace má profil publikování, kterou můžete použít k publikování vašeho kódu.</span><span class="sxs-lookup"><span data-stu-id="fcebb-170">Each app has a publishing profile that can be used to publish your code.</span></span>

#### <a name="get-publishing-profile"></a><span data-ttu-id="fcebb-171">Získat profil publikování</span><span class="sxs-lookup"><span data-stu-id="fcebb-171">Get Publishing Profile</span></span>
<span data-ttu-id="fcebb-172">Profil publikování pro aplikaci, použijte:</span><span class="sxs-lookup"><span data-stu-id="fcebb-172">To get the publishing profile for a app, use:</span></span>

    azure webapp publishingprofile --name ContosoWebApp --resource-group ContosoAzureResourceGroup

<span data-ttu-id="fcebb-173">Tento příkaz vrátí publikování profil uživatelské jméno a heslo na příkazový řádek.</span><span class="sxs-lookup"><span data-stu-id="fcebb-173">This command echoes the publishing profile username and password to the command line.</span></span>

### <a name="manage-app-hostnames"></a><span data-ttu-id="fcebb-174">Spravovat aplikace názvy hostitelů</span><span class="sxs-lookup"><span data-stu-id="fcebb-174">Manage app hostnames</span></span>
<span data-ttu-id="fcebb-175">Chcete-li spravovat vazby názvů hostitelů pro vaši aplikaci, použijte **názvy hostitelů konfigurace azure webapp** příkaz</span><span class="sxs-lookup"><span data-stu-id="fcebb-175">To manage hostname bindings for your app, use the **azure webapp config hostnames** command</span></span>  

#### <a name="list-hostname-bindings"></a><span data-ttu-id="fcebb-176">Seznam vazby názvů hostitelů.</span><span class="sxs-lookup"><span data-stu-id="fcebb-176">List hostname bindings</span></span>
<span data-ttu-id="fcebb-177">Chcete-li získat aktuální vazby názvů hostitelů, pro aplikaci, použijte:</span><span class="sxs-lookup"><span data-stu-id="fcebb-177">To get the current hostname bindings for an app, use:</span></span>

    azure webapp config hostnames list --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="add-hostname-bindings"></a><span data-ttu-id="fcebb-178">Přidat vazby názvů hostitelů.</span><span class="sxs-lookup"><span data-stu-id="fcebb-178">Add hostname bindings</span></span>
<span data-ttu-id="fcebb-179">Chcete-li do aplikace přidat vazby názvů hostitelů, použijte:</span><span class="sxs-lookup"><span data-stu-id="fcebb-179">To add hostname bindings to an app, use:</span></span>

    azure webapp config hostnames add --name ContosoWebApp --resource-group ContosoAzureResourceGroup --hostname www.contoso.com

#### <a name="delete-hostname-bindings"></a><span data-ttu-id="fcebb-180">Odstranit vazby názvů hostitelů.</span><span class="sxs-lookup"><span data-stu-id="fcebb-180">Delete hostname bindings</span></span>
<span data-ttu-id="fcebb-181">Chcete-li odstranit vazby názvů hostitelů, použijte:</span><span class="sxs-lookup"><span data-stu-id="fcebb-181">To delete hostname bindings, use:</span></span>

    azure webapp config hostnames delete --name ContosoWebApp --resource-group ContosoAzureResourceGroup --hostname www.contoso.com

## <a name="next-steps"></a><span data-ttu-id="fcebb-182">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fcebb-182">Next Steps</span></span>
* <span data-ttu-id="fcebb-183">Další informace o podpoře Azure Resource Manager CLI, najdete v části [používat rozhraní příkazového řádku Azure ke správě prostředků Azure a skupiny prostředků.](../azure-resource-manager/xplat-cli-azure-resource-manager.md)</span><span class="sxs-lookup"><span data-stu-id="fcebb-183">To learn about Azure Resource Manager CLI support, see [Use the Azure CLI to manage Azure resources and resource groups.](../azure-resource-manager/xplat-cli-azure-resource-manager.md)</span></span>
* <span data-ttu-id="fcebb-184">Další informace o správě služby App Service pomocí prostředí PowerShell najdete v tématu [Using Azure Resource Manager-Based prostředí PowerShell ke správě Azure Web Apps.](app-service-web-app-azure-resource-manager-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="fcebb-184">To learn about managing App Service using PowerShell, see [Using Azure Resource Manager-Based PowerShell to Manage Azure Web Apps.](app-service-web-app-azure-resource-manager-powershell.md)</span></span>
* <span data-ttu-id="fcebb-185">Další informace o službě Azure App Service v systému Linux najdete v tématu [Úvod do služby App Service v systému Linux](app-service-linux-intro.md)</span><span class="sxs-lookup"><span data-stu-id="fcebb-185">To learn about Azure App Service on Linux, see [Introduction to App Service on Linux](app-service-linux-intro.md)</span></span>
