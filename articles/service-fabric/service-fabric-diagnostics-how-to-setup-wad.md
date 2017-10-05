---
title: "Shromažďování protokolů pomocí Azure Diagnostics | Microsoft Docs"
description: "Tento článek popisuje, jak nastavení Azure Diagnostics pro shromažďování protokolů z clusteru Service Fabric běžící v Azure."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 9f7e1fa5-6543-4efd-b53f-39510f18df56
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/30/2017
ms.author: dekapur
ms.openlocfilehash: 190a8a393f2e7d74a792db4efa81f94a18b02221
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="collect-logs-by-using-azure-diagnostics"></a><span data-ttu-id="e94f3-103">Shromažďování protokolů pomocí Azure Diagnostics</span><span class="sxs-lookup"><span data-stu-id="e94f3-103">Collect logs by using Azure Diagnostics</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="e94f3-104">Windows</span><span class="sxs-lookup"><span data-stu-id="e94f3-104">Windows</span></span>](service-fabric-diagnostics-how-to-setup-wad.md)
> * [<span data-ttu-id="e94f3-105">Linux</span><span class="sxs-lookup"><span data-stu-id="e94f3-105">Linux</span></span>](service-fabric-diagnostics-how-to-setup-lad.md)
> 
> 

<span data-ttu-id="e94f3-106">Když používáte cluster služby Azure Service Fabric, je vhodné shromažďovat protokoly ze všech uzlů v centrálním umístění.</span><span class="sxs-lookup"><span data-stu-id="e94f3-106">When you're running an Azure Service Fabric cluster, it's a good idea to collect the logs from all the nodes in a central location.</span></span> <span data-ttu-id="e94f3-107">S protokoly v centrálním umístění, vám pomáhají analyzovat a vyřešit problémy v clusteru nebo problémy v aplikací a služeb spuštěných v daném clusteru.</span><span class="sxs-lookup"><span data-stu-id="e94f3-107">Having the logs in a central location helps you analyze and troubleshoot issues in your cluster, or issues in the applications and services running in that cluster.</span></span>

<span data-ttu-id="e94f3-108">Jeden způsob, jak nahrát a shromažďování protokolů je použití rozšíření diagnostiky Azure, který odešle protokoly do služby Azure Storage, Azure Application Insights nebo Azure Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="e94f3-108">One way to upload and collect logs is to use the Azure Diagnostics extension, which uploads logs to Azure Storage, Azure Application Insights, or Azure Event Hubs.</span></span> <span data-ttu-id="e94f3-109">Protokoly nejsou přímo v úložišti nebo ve službě Event Hubs to užitečné.</span><span class="sxs-lookup"><span data-stu-id="e94f3-109">The logs are not that useful directly in storage or in Event Hubs.</span></span> <span data-ttu-id="e94f3-110">Ale externího procesu můžete použít ke čtení událostí z úložiště a umístit je v produktu, jako [analýzy protokolů](../log-analytics/log-analytics-service-fabric.md) nebo jiné řešení pro analýzu protokolu.</span><span class="sxs-lookup"><span data-stu-id="e94f3-110">But you can use an external process to read the events from storage and place them in a product such as [Log Analytics](../log-analytics/log-analytics-service-fabric.md) or another log-parsing solution.</span></span> <span data-ttu-id="e94f3-111">[Azure Application Insights](https://azure.microsoft.com/services/application-insights/) se dodává s komplexní protokolu vyhledávání a analýzy služby předdefinované.</span><span class="sxs-lookup"><span data-stu-id="e94f3-111">[Azure Application Insights](https://azure.microsoft.com/services/application-insights/) comes with a comprehensive log search and analytics service built-in.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e94f3-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e94f3-112">Prerequisites</span></span>
<span data-ttu-id="e94f3-113">Pomocí těchto nástrojů provést některé operace, které v tomto dokumentu:</span><span class="sxs-lookup"><span data-stu-id="e94f3-113">You use these tools to perform some of the operations in this document:</span></span>

* <span data-ttu-id="e94f3-114">[Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md) (související s Azure Cloud Services, ale má správné informace a příklady)</span><span class="sxs-lookup"><span data-stu-id="e94f3-114">[Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md) (related to Azure Cloud Services but has good information and examples)</span></span>
* [<span data-ttu-id="e94f3-115">Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="e94f3-115">Azure Resource Manager</span></span>](../azure-resource-manager/resource-group-overview.md)
* [<span data-ttu-id="e94f3-116">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="e94f3-116">Azure PowerShell</span></span>](/powershell/azure/overview)
* [<span data-ttu-id="e94f3-117">Klient Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="e94f3-117">Azure Resource Manager client</span></span>](https://github.com/projectkudu/ARMClient)
* [<span data-ttu-id="e94f3-118">Šablona Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="e94f3-118">Azure Resource Manager template</span></span>](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="log-sources-that-you-might-want-to-collect"></a><span data-ttu-id="e94f3-119">Protokol zdroje, které můžete chtít shromažďovat</span><span class="sxs-lookup"><span data-stu-id="e94f3-119">Log sources that you might want to collect</span></span>
* <span data-ttu-id="e94f3-120">**Protokoly Service Fabric**: vygenerované ze platformy na standardní trasování událostí pro Windows (ETW) a EventSource kanály.</span><span class="sxs-lookup"><span data-stu-id="e94f3-120">**Service Fabric logs**: Emitted from the platform to standard Event Tracing for Windows (ETW) and EventSource channels.</span></span> <span data-ttu-id="e94f3-121">Protokoly mohou být jedním z několika typů:</span><span class="sxs-lookup"><span data-stu-id="e94f3-121">Logs can be one of several types:</span></span>
  * <span data-ttu-id="e94f3-122">Provozní události: protokoly pro operace, které provádí platformy Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="e94f3-122">Operational events: Logs for operations that the Service Fabric platform performs.</span></span> <span data-ttu-id="e94f3-123">Mezi příklady patří vytvoření aplikací a služeb, uzel změny stavu a informace o upgradu.</span><span class="sxs-lookup"><span data-stu-id="e94f3-123">Examples include creation of applications and services, node state changes, and upgrade information.</span></span>
  * [<span data-ttu-id="e94f3-124">Programování událostí modelu Reliable Actors</span><span class="sxs-lookup"><span data-stu-id="e94f3-124">Reliable Actors programming model events</span></span>](service-fabric-reliable-actors-diagnostics.md)
  * [<span data-ttu-id="e94f3-125">Spolehlivé služby programovací model události</span><span class="sxs-lookup"><span data-stu-id="e94f3-125">Reliable Services programming model events</span></span>](service-fabric-reliable-services-diagnostics.md)
* <span data-ttu-id="e94f3-126">**Události aplikace**: události vygenerované z kódu vaší služby a zapsané pomocí pomocná třída EventSource, součástí šablony sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e94f3-126">**Application events**: Events emitted from your service's code and written out by using the EventSource helper class provided in the Visual Studio templates.</span></span> <span data-ttu-id="e94f3-127">Další informace o tom, jak psát protokoly z vaší aplikace, najdete v části [monitorování a Diagnostika služby v instalačním programu místním počítači vývoj](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).</span><span class="sxs-lookup"><span data-stu-id="e94f3-127">For more information on how to write logs from your application, see [Monitor and diagnose services in a local machine development setup](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).</span></span>

## <a name="deploy-the-diagnostics-extension"></a><span data-ttu-id="e94f3-128">Nasazení rozšíření diagnostiky</span><span class="sxs-lookup"><span data-stu-id="e94f3-128">Deploy the Diagnostics extension</span></span>
<span data-ttu-id="e94f3-129">Prvním krokem při shromažďování protokolů je k nasazení rozšíření diagnostiky na všech virtuálních počítačích v clusteru Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="e94f3-129">The first step in collecting logs is to deploy the Diagnostics extension on each of the VMs in the Service Fabric cluster.</span></span> <span data-ttu-id="e94f3-130">Rozšíření diagnostiky shromažďuje protokoly na každý virtuální počítač a odesílá je do účtu úložiště, který určíte.</span><span class="sxs-lookup"><span data-stu-id="e94f3-130">The Diagnostics extension collects logs on each VM and uploads them to the storage account that you specify.</span></span> <span data-ttu-id="e94f3-131">Kroky trochu záviset na tom, zda používáte portál Azure nebo Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="e94f3-131">The steps vary a little based on whether you use the Azure portal or Azure Resource Manager.</span></span> <span data-ttu-id="e94f3-132">Kroky také lišit v závislosti na tom, jestli nasazení je součástí vytváření clusteru nebo je pro cluster, který již existuje.</span><span class="sxs-lookup"><span data-stu-id="e94f3-132">The steps also vary based on whether the deployment is part of cluster creation or is for a cluster that already exists.</span></span> <span data-ttu-id="e94f3-133">Podívejme se na postup pro jednotlivé scénáře.</span><span class="sxs-lookup"><span data-stu-id="e94f3-133">Let's look at the steps for each scenario.</span></span>

### <a name="deploy-the-diagnostics-extension-as-part-of-cluster-creation-through-the-portal"></a><span data-ttu-id="e94f3-134">Nasazení rozšíření diagnostiky jako součást vytváření clusteru prostřednictvím portálu</span><span class="sxs-lookup"><span data-stu-id="e94f3-134">Deploy the Diagnostics extension as part of cluster creation through the portal</span></span>
<span data-ttu-id="e94f3-135">Nasazení rozšíření diagnostiky pro virtuální počítače v clusteru jako součást vytváření clusteru, použijte panel nastavení této diagnostiky vidět na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="e94f3-135">To deploy the Diagnostics extension to the VMs in the cluster as part of cluster creation, you use the Diagnostics settings panel shown in the following image.</span></span> <span data-ttu-id="e94f3-136">Povolit Reliable Actors nebo spolehlivé služby shromažďování událostí, ujistěte se, že diagnostiky je nastavená na **na** (výchozí nastavení).</span><span class="sxs-lookup"><span data-stu-id="e94f3-136">To enable Reliable Actors or Reliable Services event collection, ensure that Diagnostics is set to **On** (the default setting).</span></span> <span data-ttu-id="e94f3-137">Po vytvoření clusteru, nelze změnit tato nastavení pomocí portálu.</span><span class="sxs-lookup"><span data-stu-id="e94f3-137">After you create the cluster, you can't change these settings by using the portal.</span></span>

![Nastavení diagnostiky Azure na portálu pro vytvoření clusteru](./media/service-fabric-diagnostics-how-to-setup-wad/portal-cluster-creation-diagnostics-setting.png)

<span data-ttu-id="e94f3-139">Tým podpory Azure *vyžaduje* podporovat protokoly pomáhající při řešení žádosti o podporu, které vytvoříte.</span><span class="sxs-lookup"><span data-stu-id="e94f3-139">The Azure support team *requires* support logs to help resolve any support requests that you create.</span></span> <span data-ttu-id="e94f3-140">Tyto protokoly se shromažďují v reálném čase a jsou uložené v jednom z účty úložiště vytvořené ve skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="e94f3-140">These logs are collected in real time and are stored in one of the storage accounts created in the resource group.</span></span> <span data-ttu-id="e94f3-141">Nastavení diagnostiky konfigurace událostí na úrovni aplikace.</span><span class="sxs-lookup"><span data-stu-id="e94f3-141">The Diagnostics settings configure application-level events.</span></span> <span data-ttu-id="e94f3-142">Tyto události zahrnují [Reliable Actors](service-fabric-reliable-actors-diagnostics.md) události, [spolehlivé služby](service-fabric-reliable-services-diagnostics.md) události a některé události Service Fabric úrovni systému se neukládají v Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="e94f3-142">These events include [Reliable Actors](service-fabric-reliable-actors-diagnostics.md) events, [Reliable Services](service-fabric-reliable-services-diagnostics.md) events, and some system-level Service Fabric events to be stored in Azure Storage.</span></span>

<span data-ttu-id="e94f3-143">Produkty, jako [Elasticsearch](https://www.elastic.co/guide/index.html) nebo vlastního procesu můžete získat události z účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="e94f3-143">Products such as [Elasticsearch](https://www.elastic.co/guide/index.html) or your own process can get the events from the storage account.</span></span> <span data-ttu-id="e94f3-144">Aktuálně neexistuje žádný způsob, jak filtrovat nebo je naopak události, které se odesílají do tabulky.</span><span class="sxs-lookup"><span data-stu-id="e94f3-144">There is currently no way to filter or groom the events that are sent to the table.</span></span> <span data-ttu-id="e94f3-145">Pokud nemáte implementujte proces odebrání události v tabulce, tabulka pořád roste s tím.</span><span class="sxs-lookup"><span data-stu-id="e94f3-145">If you don't implement a process to remove events from the table, the table will continue to grow.</span></span>

<span data-ttu-id="e94f3-146">Při vytváření clusteru pomocí portálu, důrazně doporučujeme, abyste si stáhli šablony **předtím, než kliknete na tlačítko OK** k vytvoření clusteru.</span><span class="sxs-lookup"><span data-stu-id="e94f3-146">When you're creating a cluster by using the portal, we highly recommend that you download the template **before you click OK** to create the cluster.</span></span> <span data-ttu-id="e94f3-147">Podrobnosti najdete v části [nastavit cluster Service Fabric pomocí šablony Azure Resource Manager](service-fabric-cluster-creation-via-arm.md).</span><span class="sxs-lookup"><span data-stu-id="e94f3-147">For details, refer to [Set up a Service Fabric cluster by using an Azure Resource Manager template](service-fabric-cluster-creation-via-arm.md).</span></span> <span data-ttu-id="e94f3-148">Šablonu, kterou chcete provést změny později, budete potřebovat, protože nemůže provést některé změny pomocí portálu.</span><span class="sxs-lookup"><span data-stu-id="e94f3-148">You'll need the template to make changes later, because you can't make some changes by using the portal.</span></span>

<span data-ttu-id="e94f3-149">Pomocí následujících kroků můžete exportovat šablony z portálu.</span><span class="sxs-lookup"><span data-stu-id="e94f3-149">You can export templates from the portal by using the following steps.</span></span> <span data-ttu-id="e94f3-150">Tyto šablony však může být obtížné použít, protože mohou mít hodnoty null, kterým chybí požadované informace.</span><span class="sxs-lookup"><span data-stu-id="e94f3-150">However, these templates can be more difficult to use because they might have null values that are missing required information.</span></span>

1. <span data-ttu-id="e94f3-151">Otevřete vaší skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="e94f3-151">Open your resource group.</span></span>
2. <span data-ttu-id="e94f3-152">Vyberte **nastavení** zobrazíte panel nastavení.</span><span class="sxs-lookup"><span data-stu-id="e94f3-152">Select **Settings** to display the settings panel.</span></span>
3. <span data-ttu-id="e94f3-153">Vyberte **nasazení** zobrazíte panel Historie nasazení.</span><span class="sxs-lookup"><span data-stu-id="e94f3-153">Select **Deployments** to display the deployment history panel.</span></span>
4. <span data-ttu-id="e94f3-154">Vyberte nasazení zobrazit podrobnosti o nasazení.</span><span class="sxs-lookup"><span data-stu-id="e94f3-154">Select a deployment to display the details of the deployment.</span></span>
5. <span data-ttu-id="e94f3-155">Vyberte **exportovat šablonu** zobrazíte panel šablony.</span><span class="sxs-lookup"><span data-stu-id="e94f3-155">Select **Export Template** to display the template panel.</span></span>
6. <span data-ttu-id="e94f3-156">Vyberte **uložit do souboru** exportovat soubor .zip, který obsahuje šablony, parametr a soubory prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e94f3-156">Select **Save to file** to export a .zip file that contains the template, parameter, and PowerShell files.</span></span>

<span data-ttu-id="e94f3-157">Po exportu soubory, budete muset provedete změny.</span><span class="sxs-lookup"><span data-stu-id="e94f3-157">After you export the files, you need to make a modification.</span></span> <span data-ttu-id="e94f3-158">Upravte souboru parameters.JSON tímto kódem a odeberte **adminPassword** elementu.</span><span class="sxs-lookup"><span data-stu-id="e94f3-158">Edit the parameters.json file and remove the **adminPassword** element.</span></span> <span data-ttu-id="e94f3-159">To způsobí výzva k zadání hesla, když se spustí skript nasazení.</span><span class="sxs-lookup"><span data-stu-id="e94f3-159">This will cause a prompt for the password when the deployment script is run.</span></span> <span data-ttu-id="e94f3-160">Když spouštíte skript nasazení, můžete chtít opravit hodnoty null parametru.</span><span class="sxs-lookup"><span data-stu-id="e94f3-160">When you're running the deployment script, you might have to fix null parameter values.</span></span>

<span data-ttu-id="e94f3-161">Použití stažené šabloně aktualizovat konfiguraci:</span><span class="sxs-lookup"><span data-stu-id="e94f3-161">To use the downloaded template to update a configuration:</span></span>

1. <span data-ttu-id="e94f3-162">Extrahujte obsah do složky v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="e94f3-162">Extract the contents to a folder on your local computer.</span></span>
2. <span data-ttu-id="e94f3-163">Upravte obsah, aby odráželo novou konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="e94f3-163">Modify the contents to reflect the new configuration.</span></span>
3. <span data-ttu-id="e94f3-164">Spusťte PowerShell a přejděte do složky, které jste extrahovali obsah.</span><span class="sxs-lookup"><span data-stu-id="e94f3-164">Start PowerShell and change to the folder where you extracted the content.</span></span>
4. <span data-ttu-id="e94f3-165">Spustit **deploy.ps1** a zadat ID předplatného, název skupiny prostředků (použijte stejný název se aktualizovat konfiguraci) a název jedinečný nasazení.</span><span class="sxs-lookup"><span data-stu-id="e94f3-165">Run **deploy.ps1** and fill in the subscription ID, the resource group name (use the same name to update the configuration), and a unique deployment name.</span></span>

### <a name="deploy-the-diagnostics-extension-as-part-of-cluster-creation-by-using-azure-resource-manager"></a><span data-ttu-id="e94f3-166">Nasazení rozšíření diagnostiky jako součást vytváření clusteru pomocí Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="e94f3-166">Deploy the Diagnostics extension as part of cluster creation by using Azure Resource Manager</span></span>
<span data-ttu-id="e94f3-167">K vytvoření clusteru pomocí Správce prostředků, musíte přidat konfiguraci diagnostiky JSON šablony Resource Manageru úplné clusteru před vytvořením clusteru.</span><span class="sxs-lookup"><span data-stu-id="e94f3-167">To create a cluster by using Resource Manager, you need to add the Diagnostics configuration JSON to the full cluster Resource Manager template before you create the cluster.</span></span> <span data-ttu-id="e94f3-168">Poskytujeme šablony Resource Manageru ukázkový pět VM cluster s konfigurací diagnostiky přidána jako součást naše ukázky šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="e94f3-168">We provide a sample five-VM cluster Resource Manager template with Diagnostics configuration added to it as part of our Resource Manager template samples.</span></span> <span data-ttu-id="e94f3-169">Zobrazí se v tomto umístění v galerii Azure Samples: [pěti uzly clusteru s ukázkou šablony správce prostředků diagnostiky](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype).</span><span class="sxs-lookup"><span data-stu-id="e94f3-169">You can see it at this location in the Azure Samples gallery: [Five-node cluster with Diagnostics Resource Manager template sample](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype).</span></span>

<span data-ttu-id="e94f3-170">Pokud chcete zobrazit nastavení diagnostiky v šabloně Resource Manager, otevřete soubor azuredeploy.json a vyhledejte **IaaSDiagnostics**.</span><span class="sxs-lookup"><span data-stu-id="e94f3-170">To see the Diagnostics setting in the Resource Manager template, open the azuredeploy.json file and search for **IaaSDiagnostics**.</span></span> <span data-ttu-id="e94f3-171">Pokud chcete vytvořit cluster pomocí této šablony, vyberte **nasadit do Azure** tlačítko k dispozici na předchozí odkaz.</span><span class="sxs-lookup"><span data-stu-id="e94f3-171">To create a cluster by using this template, select the **Deploy to Azure** button available at the previous link.</span></span>

<span data-ttu-id="e94f3-172">Alternativně můžete stáhnout ukázkový Resource Manager, provést změny a vytvořte cluster se změněné šablony pomocí `New-AzureRmResourceGroupDeployment` příkazu v okně prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e94f3-172">Alternatively, you can download the Resource Manager sample, make changes to it, and create a cluster with the modified template by using the `New-AzureRmResourceGroupDeployment` command in an Azure PowerShell window.</span></span> <span data-ttu-id="e94f3-173">Zobrazit následující kód pro parametry, které můžete předat do příkazu.</span><span class="sxs-lookup"><span data-stu-id="e94f3-173">See the following code for the parameters that you pass in to the command.</span></span> <span data-ttu-id="e94f3-174">Podrobné informace o tom, jak nasazení skupiny prostředků pomocí prostředí PowerShell najdete v článku [nasazení skupiny prostředků pomocí šablony Azure Resource Manager](../azure-resource-manager/resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="e94f3-174">For detailed information on how to deploy a resource group by using PowerShell, see the article [Deploy a resource group with the Azure Resource Manager template](../azure-resource-manager/resource-group-template-deploy.md).</span></span>

```powershell

New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -Name $deploymentName -TemplateFile $pathToARMConfigJsonFile -TemplateParameterFile $pathToParameterFile –Verbose
```

### <a name="deploy-the-diagnostics-extension-to-an-existing-cluster"></a><span data-ttu-id="e94f3-175">Nasazení rozšíření diagnostiky k existujícímu clusteru</span><span class="sxs-lookup"><span data-stu-id="e94f3-175">Deploy the Diagnostics extension to an existing cluster</span></span>
<span data-ttu-id="e94f3-176">Pokud máte existující cluster, který nemá diagnostiky nasazené, nebo pokud chcete upravit existující konfigurace, můžete přidat nebo aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="e94f3-176">If you have an existing cluster that doesn't have Diagnostics deployed, or if you want to modify an existing configuration, you can add or update it.</span></span> <span data-ttu-id="e94f3-177">Úprava šablony Resource Manageru, který se používá k vytvoření stávajícího clusteru nebo stáhnout šablonu z portálu, jak je popsáno výše.</span><span class="sxs-lookup"><span data-stu-id="e94f3-177">Modify the Resource Manager template that's used to create the existing cluster or download the template from the portal as described earlier.</span></span> <span data-ttu-id="e94f3-178">Upravte soubor template.json provedením následujících úloh.</span><span class="sxs-lookup"><span data-stu-id="e94f3-178">Modify the template.json file by performing the following tasks.</span></span>

<span data-ttu-id="e94f3-179">Přidáte nový prostředek úložiště do šablony přidáním do oddílu prostředků.</span><span class="sxs-lookup"><span data-stu-id="e94f3-179">Add a new storage resource to the template by adding to the resources section.</span></span>

```json
{
  "apiVersion": "2015-05-01-preview",
  "type": "Microsoft.Storage/storageAccounts",
  "name": "[parameters('applicationDiagnosticsStorageAccountName')]",
  "location": "[parameters('computeLocation')]",
  "properties": {
    "accountType": "[parameters('applicationDiagnosticsStorageAccountType')]"
  },
  "tags": {
    "resourceType": "Service Fabric",
    "clusterName": "[parameters('clusterName')]"
  }
},
```

 <span data-ttu-id="e94f3-180">V dalším kroku přidejte do části Parametry hned za definice účet úložiště, mezi `supportLogStorageAccountName` a `vmNodeType0Name`.</span><span class="sxs-lookup"><span data-stu-id="e94f3-180">Next, add to the parameters section just after the storage account definitions, between `supportLogStorageAccountName` and `vmNodeType0Name`.</span></span> <span data-ttu-id="e94f3-181">Nahraďte zástupný text *sem zadejte název účtu úložiště* s názvem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="e94f3-181">Replace the placeholder text *storage account name goes here* with the name of the storage account.</span></span>

```json
    "applicationDiagnosticsStorageAccountType": {
      "type": "string",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS"
      ],
      "defaultValue": "Standard_LRS",
      "metadata": {
        "description": "Replication option for the application diagnostics storage account"
      }
    },
    "applicationDiagnosticsStorageAccountName": {
      "type": "string",
      "defaultValue": "storage account name goes here",
      "metadata": {
        "description": "Name for the storage account that contains application diagnostics data from the cluster"
      }
    },
```
<span data-ttu-id="e94f3-182">Potom aktualizovat `VirtualMachineProfile` souboru template.json přidáním následujícího kódu v rámci pole rozšíření.</span><span class="sxs-lookup"><span data-stu-id="e94f3-182">Then, update the `VirtualMachineProfile` section of the template.json file by adding the following code within the extensions array.</span></span> <span data-ttu-id="e94f3-183">Nezapomeňte přidat čárka na začátku nebo konci, v závislosti na tom, kde je vložen.</span><span class="sxs-lookup"><span data-stu-id="e94f3-183">Be sure to add a comma at the beginning or the end, depending on where it's inserted.</span></span>

```json
{
    "name": "[concat(parameters('vmNodeType0Name'),'_Microsoft.Insights.VMDiagnosticsSettings')]",
    "properties": {
        "type": "IaaSDiagnostics",
        "autoUpgradeMinorVersion": true,
        "protectedSettings": {
        "storageAccountName": "[parameters('applicationDiagnosticsStorageAccountName')]",
        "storageAccountKey": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('applicationDiagnosticsStorageAccountName')),'2015-05-01-preview').key1]",
        "storageAccountEndPoint": "https://core.windows.net/"
        },
        "publisher": "Microsoft.Azure.Diagnostics",
        "settings": {
        "WadCfg": {
            "DiagnosticMonitorConfiguration": {
            "overallQuotaInMB": "50000",
            "EtwProviders": {
                "EtwEventSourceProviderConfiguration": [
                {
                    "provider": "Microsoft-ServiceFabric-Actors",
                    "scheduledTransferKeywordFilter": "1",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricReliableActorEventTable"
                    }
                },
                {
                    "provider": "Microsoft-ServiceFabric-Services",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricReliableServiceEventTable"
                    }
                }
                ],
                "EtwManifestProviderConfiguration": [
                {
                    "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8",
                    "scheduledTransferLogLevelFilter": "Information",
                    "scheduledTransferKeywordFilter": "4611686018427387904",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricSystemEventTable"
                    }
                }
                ]
            }
            }
        },
        "StorageAccount": "[parameters('applicationDiagnosticsStorageAccountName')]"
        },
        "typeHandlerVersion": "1.5"
    }
}
```

<span data-ttu-id="e94f3-184">Jakmile upravíte soubor template.json, jak je popsáno, znovu publikujte šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="e94f3-184">After you modify the template.json file as described, republish the Resource Manager template.</span></span> <span data-ttu-id="e94f3-185">Pokud byl exportován šablony, spuštěním souboru deploy.ps1 znovu publikuje uzamkl šablony.</span><span class="sxs-lookup"><span data-stu-id="e94f3-185">If the template was exported, running the deploy.ps1 file republishes the template.</span></span> <span data-ttu-id="e94f3-186">Poté, co nasadíte, ujistěte se, že **ProvisioningState** je **úspěšné**.</span><span class="sxs-lookup"><span data-stu-id="e94f3-186">After you deploy, ensure that **ProvisioningState** is **Succeeded**.</span></span>

## <a name="update-diagnostics-to-collect-health-and-load-events"></a><span data-ttu-id="e94f3-187">Aktualizovat diagnostiky pro shromažďování stavu a načtení události</span><span class="sxs-lookup"><span data-stu-id="e94f3-187">Update diagnostics to collect health and load events</span></span>

<span data-ttu-id="e94f3-188">Počínaje 5.4 verzi Service Fabric, stavu a zatížení metriky události jsou k dispozici pro kolekci.</span><span class="sxs-lookup"><span data-stu-id="e94f3-188">Starting with the 5.4 release of Service Fabric, health and load metric events are available for collection.</span></span> <span data-ttu-id="e94f3-189">Tyto události podle událostí generovaných systému nebo v kódu pomocí stavu nebo zatížení rozhraní API pro vytváření sestav, jako [ReportPartitionHealth](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportpartitionhealth.aspx) nebo [ReportLoad](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportload.aspx).</span><span class="sxs-lookup"><span data-stu-id="e94f3-189">These events reflect events generated by the system or your code by using the health or load reporting APIs such as [ReportPartitionHealth](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportpartitionhealth.aspx) or [ReportLoad](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportload.aspx).</span></span> <span data-ttu-id="e94f3-190">To umožňuje pro agregaci a zobrazení stavu systému v čase a pro výstrahy na základě stavu nebo zatížení událostí.</span><span class="sxs-lookup"><span data-stu-id="e94f3-190">This allows for aggregating and viewing system health over time and for alerting based on health or load events.</span></span> <span data-ttu-id="e94f3-191">Chcete-li zobrazit tyto události v sadě Visual Studio prohlížeč diagnostických událostí přidat "Microsoft-ServiceFabric:4:0x4000000000000008" do seznamu zprostředkovatelů trasování událostí pro Windows.</span><span class="sxs-lookup"><span data-stu-id="e94f3-191">To view these events in Visual Studio's Diagnostic Event Viewer add "Microsoft-ServiceFabric:4:0x4000000000000008" to the list of ETW providers.</span></span>

<span data-ttu-id="e94f3-192">Shromažďovat události, upravte zahrnout šablona resource Manageru</span><span class="sxs-lookup"><span data-stu-id="e94f3-192">To collect the events, modify the resource manager template to include</span></span>

```json
  "EtwManifestProviderConfiguration": [
    {
      "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8",
      "scheduledTransferLogLevelFilter": "Information",
      "scheduledTransferKeywordFilter": "4611686018427387912",
      "scheduledTransferPeriod": "PT5M",
      "DefaultEvents": {
        "eventDestination": "ServiceFabricSystemEventTable"
      }
    }
```

## <a name="update-diagnostics-to-collect-and-upload-logs-from-new-eventsource-channels"></a><span data-ttu-id="e94f3-193">Aktualizovat diagnostiky ke sběru a odeslat protokoly z nové EventSource kanály</span><span class="sxs-lookup"><span data-stu-id="e94f3-193">Update Diagnostics to collect and upload logs from new EventSource channels</span></span>
<span data-ttu-id="e94f3-194">Chcete-li aktualizovat diagnostiky pro shromažďování protokolů z nové EventSource kanály, které představují novou aplikaci, která se chystáte nasadit, proveďte stejný postup jako v [předchozí části](#deploywadarm) pro nastavení diagnostiky pro existující cluster.</span><span class="sxs-lookup"><span data-stu-id="e94f3-194">To update Diagnostics to collect logs from new EventSource channels that represent a new application that you're about to deploy, perform the same steps as in the [previous section](#deploywadarm) for setup of Diagnostics for an existing cluster.</span></span>

<span data-ttu-id="e94f3-195">Aktualizace `EtwEventSourceProviderConfiguration` v souboru template.json pro přidání položek nové kanály EventSource před použitím konfigurace aktualizací pomocí `New-AzureRmResourceGroupDeployment` příkaz prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e94f3-195">Update the `EtwEventSourceProviderConfiguration` section in the template.json file to add entries for the new EventSource channels before you apply the configuration update by using the `New-AzureRmResourceGroupDeployment` PowerShell command.</span></span> <span data-ttu-id="e94f3-196">Název zdroje událostí je definován jako součást kódu v souboru ServiceEventSource.cs generovaný Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e94f3-196">The name of the event source is defined as part of your code in the Visual Studio-generated ServiceEventSource.cs file.</span></span>

<span data-ttu-id="e94f3-197">Například pokud je váš zdroj událostí Moje Eventsource, přidejte následující kód do tabulky s názvem MyDestinationTableName umístit události z Moje Eventsource.</span><span class="sxs-lookup"><span data-stu-id="e94f3-197">For example, if your event source is named My-Eventsource, add the following code to place the events from My-Eventsource into a table named MyDestinationTableName.</span></span>

```json
        {
            "provider": "My-Eventsource",
            "scheduledTransferPeriod": "PT5M",
            "DefaultEvents": {
            "eventDestination": "MyDestinationTableName"
            }
        }
```

<span data-ttu-id="e94f3-198">Pokud chcete shromáždit čítače výkonu nebo protokoly událostí, upravte šablony Resource Manageru pomocí příklady součástí [vytvoření virtuálního počítače s Windows pomocí monitorování a Diagnostika pomocí šablony Azure Resource Manager](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e94f3-198">To collect performance counters or event logs, modify the Resource Manager template by using the examples provided in [Create a Windows virtual machine with monitoring and diagnostics by using an Azure Resource Manager template](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="e94f3-199">Potom se znovu publikujte šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="e94f3-199">Then, republish the Resource Manager template.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e94f3-200">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e94f3-200">Next steps</span></span>
<span data-ttu-id="e94f3-201">Chcete-li podrobněji pochopit, jaké události je vhodné vyhledat při řešení potíží, najdete v části diagnostických událostí vygenerované pro [Reliable Actors](service-fabric-reliable-actors-diagnostics.md) a [spolehlivé služby](service-fabric-reliable-services-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="e94f3-201">To understand in more detail what events you should look for while troubleshooting issues, see the diagnostic events emitted for [Reliable Actors](service-fabric-reliable-actors-diagnostics.md) and [Reliable Services](service-fabric-reliable-services-diagnostics.md).</span></span>

## <a name="related-articles"></a><span data-ttu-id="e94f3-202">Související články</span><span class="sxs-lookup"><span data-stu-id="e94f3-202">Related articles</span></span>
* [<span data-ttu-id="e94f3-203">Zjistěte, jak shromažďování čítače výkonu nebo protokoly pomocí rozšíření diagnostiky</span><span class="sxs-lookup"><span data-stu-id="e94f3-203">Learn how to collect performance counters or logs by using the Diagnostics extension</span></span>](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="e94f3-204">Řešení Service Fabric v analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="e94f3-204">Service Fabric solution in Log Analytics</span></span>](../log-analytics/log-analytics-service-fabric.md)

