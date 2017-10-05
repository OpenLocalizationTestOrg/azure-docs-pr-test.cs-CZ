---
title: "Azure Service Fabric událostí agregace se systémem Windows Azure Diagnostics | Microsoft Docs"
description: "Další informace o agregaci a shromažďování událostí pomocí WAD pro monitorování a Diagnostika Azure Service Fabric clusterů."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/17/2017
ms.author: dekapur
ms.openlocfilehash: 5773361fdec4cb8ee54fa2856f6aa969d5dac4e9
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="event-aggregation-and-collection-using-windows-azure-diagnostics"></a><span data-ttu-id="19231-103">Seskupení událostí a kolekce s použitím Windows Azure Diagnostics</span><span class="sxs-lookup"><span data-stu-id="19231-103">Event aggregation and collection using Windows Azure Diagnostics</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="19231-104">Windows</span><span class="sxs-lookup"><span data-stu-id="19231-104">Windows</span></span>](service-fabric-diagnostics-event-aggregation-wad.md)
> * [<span data-ttu-id="19231-105">Linux</span><span class="sxs-lookup"><span data-stu-id="19231-105">Linux</span></span>](service-fabric-diagnostics-event-aggregation-lad.md)
>
>

<span data-ttu-id="19231-106">Když používáte cluster služby Azure Service Fabric, je vhodné shromažďovat protokoly ze všech uzlů v centrálním umístění.</span><span class="sxs-lookup"><span data-stu-id="19231-106">When you're running an Azure Service Fabric cluster, it's a good idea to collect the logs from all the nodes in a central location.</span></span> <span data-ttu-id="19231-107">S protokoly v centrálním umístění, vám pomáhají analyzovat a vyřešit problémy v clusteru nebo problémy v aplikací a služeb spuštěných v daném clusteru.</span><span class="sxs-lookup"><span data-stu-id="19231-107">Having the logs in a central location helps you analyze and troubleshoot issues in your cluster, or issues in the applications and services running in that cluster.</span></span>

<span data-ttu-id="19231-108">Jeden způsob, jak nahrát a shromažďování protokolů je použití rozšíření Windows Azure Diagnostics (WAD), který odešle protokoly do služby Azure Storage a má také možnost odeslat protokoly služby Azure Application Insights nebo Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="19231-108">One way to upload and collect logs is to use the Windows Azure Diagnostics (WAD) extension, which uploads logs to Azure Storage, and also has the option to send logs to Azure Application Insights or Event Hubs.</span></span> <span data-ttu-id="19231-109">Externího procesu můžete také použít ke čtení události z úložiště a umístit je o analýzy platformy produkt, například [analýzy protokolů OMS](../log-analytics/log-analytics-service-fabric.md) nebo jiné řešení pro analýzu protokolu.</span><span class="sxs-lookup"><span data-stu-id="19231-109">You can also use an external process to read the events from storage and place them in an analysis platform product, such as [OMS Log Analytics](../log-analytics/log-analytics-service-fabric.md) or another log-parsing solution.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="19231-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="19231-110">Prerequisites</span></span>
<span data-ttu-id="19231-111">Tyto nástroje se používají k provádění některých operací v tomto dokumentu:</span><span class="sxs-lookup"><span data-stu-id="19231-111">These tools are used to perform some of the operations in this document:</span></span>

* <span data-ttu-id="19231-112">[Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md) (související s Azure Cloud Services, ale má správné informace a příklady)</span><span class="sxs-lookup"><span data-stu-id="19231-112">[Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md) (related to Azure Cloud Services but has good information and examples)</span></span>
* [<span data-ttu-id="19231-113">Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="19231-113">Azure Resource Manager</span></span>](../azure-resource-manager/resource-group-overview.md)
* [<span data-ttu-id="19231-114">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="19231-114">Azure PowerShell</span></span>](/powershell/azure/overview)
* [<span data-ttu-id="19231-115">Klient Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="19231-115">Azure Resource Manager client</span></span>](https://github.com/projectkudu/ARMClient)
* [<span data-ttu-id="19231-116">Šablona Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="19231-116">Azure Resource Manager template</span></span>](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="log-and-event-sources"></a><span data-ttu-id="19231-117">Protokol a událostí zdroje</span><span class="sxs-lookup"><span data-stu-id="19231-117">Log and event sources</span></span>

### <a name="service-fabric-platform-events"></a><span data-ttu-id="19231-118">Události platformy Service Fabric</span><span class="sxs-lookup"><span data-stu-id="19231-118">Service Fabric platform events</span></span>
<span data-ttu-id="19231-119">Jak je popsáno v [v tomto článku](service-fabric-diagnostics-event-generation-infra.md), Service Fabric nastaví můžete s kanály, několik protokolování se na pole, ve kterých jsou následující kanály snadno nakonfigurované s WAD k odeslání, sledování a diagnostická data do úložiště tabulky nebo jinde:</span><span class="sxs-lookup"><span data-stu-id="19231-119">As discussed in [this article](service-fabric-diagnostics-event-generation-infra.md), Service Fabric sets you up with a few out-of-the-box logging channels, of which the following channels are easily configured with WAD to send monitoring and diagnostics data to a storage table or elsewhere:</span></span>
  * <span data-ttu-id="19231-120">Provozní události: vyšší úrovně operace, které provádí platformy Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="19231-120">Operational events: higher-level operations that the Service Fabric platform performs.</span></span> <span data-ttu-id="19231-121">Mezi příklady patří vytvoření aplikací a služeb, uzel změny stavu a informace o upgradu.</span><span class="sxs-lookup"><span data-stu-id="19231-121">Examples include creation of applications and services, node state changes, and upgrade information.</span></span> <span data-ttu-id="19231-122">Tyto jsou vygenerované jako protokoly trasování událostí pro Windows (ETW)</span><span class="sxs-lookup"><span data-stu-id="19231-122">These are emitted as Event Tracing for Windows (ETW) logs</span></span>
  * [<span data-ttu-id="19231-123">Programování událostí modelu Reliable Actors</span><span class="sxs-lookup"><span data-stu-id="19231-123">Reliable Actors programming model events</span></span>](service-fabric-reliable-actors-diagnostics.md)
  * [<span data-ttu-id="19231-124">Spolehlivé služby programovací model události</span><span class="sxs-lookup"><span data-stu-id="19231-124">Reliable Services programming model events</span></span>](service-fabric-reliable-services-diagnostics.md)

### <a name="application-events"></a><span data-ttu-id="19231-125">Události aplikace</span><span class="sxs-lookup"><span data-stu-id="19231-125">Application events</span></span>
 <span data-ttu-id="19231-126">Události vygenerované z kódu vaší aplikací a služeb a zapisují pomocí pomocná třída EventSource, součástí šablony sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="19231-126">Events emitted from your applications' and services' code and written out by using the EventSource helper class provided in the Visual Studio templates.</span></span> <span data-ttu-id="19231-127">Další informace o tom, jak psát EventSource protokoly z vaší aplikace, najdete v části [monitorování a Diagnostika služby v instalačním programu místním počítači vývoj](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).</span><span class="sxs-lookup"><span data-stu-id="19231-127">For more information on how to write EventSource logs from your application, see [Monitor and diagnose services in a local machine development setup](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).</span></span>

## <a name="deploy-the-diagnostics-extension"></a><span data-ttu-id="19231-128">Nasazení rozšíření diagnostiky</span><span class="sxs-lookup"><span data-stu-id="19231-128">Deploy the Diagnostics extension</span></span>
<span data-ttu-id="19231-129">Prvním krokem při shromažďování protokolů je k nasazení rozšíření diagnostiky na všech virtuálních počítačích v clusteru Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="19231-129">The first step in collecting logs is to deploy the Diagnostics extension on each of the VMs in the Service Fabric cluster.</span></span> <span data-ttu-id="19231-130">Rozšíření diagnostiky shromažďuje protokoly na každý virtuální počítač a odesílá je do účtu úložiště, který určíte.</span><span class="sxs-lookup"><span data-stu-id="19231-130">The Diagnostics extension collects logs on each VM and uploads them to the storage account that you specify.</span></span> <span data-ttu-id="19231-131">Kroky trochu záviset na tom, zda používáte portál Azure nebo Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="19231-131">The steps vary a little based on whether you use the Azure portal or Azure Resource Manager.</span></span> <span data-ttu-id="19231-132">Kroky také lišit v závislosti na tom, jestli nasazení je součástí vytváření clusteru nebo je pro cluster, který již existuje.</span><span class="sxs-lookup"><span data-stu-id="19231-132">The steps also vary based on whether the deployment is part of cluster creation or is for a cluster that already exists.</span></span> <span data-ttu-id="19231-133">Podívejme se na postup pro jednotlivé scénáře.</span><span class="sxs-lookup"><span data-stu-id="19231-133">Let's look at the steps for each scenario.</span></span>

### <a name="deploy-the-diagnostics-extension-as-part-of-cluster-creation-through-azure-portal"></a><span data-ttu-id="19231-134">Nasazení rozšíření diagnostiky jako součást vytváření clusteru prostřednictvím portálu Azure</span><span class="sxs-lookup"><span data-stu-id="19231-134">Deploy the Diagnostics extension as part of cluster creation through Azure portal</span></span>
<span data-ttu-id="19231-135">Nasazení rozšíření diagnostiky pro virtuální počítače v clusteru jako součást vytváření clusteru, použijte panel nastavení diagnostiky vidět na následujícím obrázku – Ujistěte se, že diagnostiky je nastavená na **na** (výchozí nastavení).</span><span class="sxs-lookup"><span data-stu-id="19231-135">To deploy the Diagnostics extension to the VMs in the cluster as part of cluster creation, you use the Diagnostics settings panel shown in the following image - ensure that Diagnostics is set to **On** (the default setting).</span></span> <span data-ttu-id="19231-136">Po vytvoření clusteru, nelze změnit tato nastavení pomocí portálu.</span><span class="sxs-lookup"><span data-stu-id="19231-136">After you create the cluster, you can't change these settings by using the portal.</span></span>

![Nastavení diagnostiky Azure na portálu pro vytvoření clusteru](media/service-fabric-diagnostics-event-aggregation-wad/azure-enable-diagnostics.png)

<span data-ttu-id="19231-138">Při vytváření clusteru pomocí portálu, důrazně doporučujeme, abyste si stáhli šablony **předtím, než kliknete na tlačítko OK** k vytvoření clusteru.</span><span class="sxs-lookup"><span data-stu-id="19231-138">When you're creating a cluster by using the portal, we highly recommend that you download the template **before you click OK** to create the cluster.</span></span> <span data-ttu-id="19231-139">Podrobnosti najdete v části [nastavit cluster Service Fabric pomocí šablony Azure Resource Manager](service-fabric-cluster-creation-via-arm.md).</span><span class="sxs-lookup"><span data-stu-id="19231-139">For details, refer to [Set up a Service Fabric cluster by using an Azure Resource Manager template](service-fabric-cluster-creation-via-arm.md).</span></span> <span data-ttu-id="19231-140">Šablonu, kterou chcete provést změny později, budete potřebovat, protože nemůže provést některé změny pomocí portálu.</span><span class="sxs-lookup"><span data-stu-id="19231-140">You'll need the template to make changes later, because you can't make some changes by using the portal.</span></span>

### <a name="deploy-the-diagnostics-extension-as-part-of-cluster-creation-by-using-azure-resource-manager"></a><span data-ttu-id="19231-141">Nasazení rozšíření diagnostiky jako součást vytváření clusteru pomocí Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="19231-141">Deploy the Diagnostics extension as part of cluster creation by using Azure Resource Manager</span></span>
<span data-ttu-id="19231-142">K vytvoření clusteru pomocí Správce prostředků, musíte přidat konfiguraci diagnostiky JSON šablony Resource Manageru úplné clusteru před vytvořením clusteru.</span><span class="sxs-lookup"><span data-stu-id="19231-142">To create a cluster by using Resource Manager, you need to add the Diagnostics configuration JSON to the full cluster Resource Manager template before you create the cluster.</span></span> <span data-ttu-id="19231-143">Poskytujeme šablony Resource Manageru ukázkový pět VM cluster s konfigurací diagnostiky přidána jako součást naše ukázky šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="19231-143">We provide a sample five-VM cluster Resource Manager template with Diagnostics configuration added to it as part of our Resource Manager template samples.</span></span> <span data-ttu-id="19231-144">Zobrazí se v tomto umístění v galerii Azure Samples: [pěti uzly clusteru s ukázkou šablony správce prostředků diagnostiky](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype-wad).</span><span class="sxs-lookup"><span data-stu-id="19231-144">You can see it at this location in the Azure Samples gallery: [Five-node cluster with Diagnostics Resource Manager template sample](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype-wad).</span></span>

<span data-ttu-id="19231-145">Pokud chcete zobrazit nastavení diagnostiky v šabloně Resource Manager, otevřete soubor azuredeploy.json a vyhledejte **IaaSDiagnostics**.</span><span class="sxs-lookup"><span data-stu-id="19231-145">To see the Diagnostics setting in the Resource Manager template, open the azuredeploy.json file and search for **IaaSDiagnostics**.</span></span> <span data-ttu-id="19231-146">Pokud chcete vytvořit cluster pomocí této šablony, vyberte **nasadit do Azure** tlačítko k dispozici na předchozí odkaz.</span><span class="sxs-lookup"><span data-stu-id="19231-146">To create a cluster by using this template, select the **Deploy to Azure** button available at the previous link.</span></span>

<span data-ttu-id="19231-147">Alternativně můžete stáhnout ukázkový Resource Manager, provést změny a vytvořte cluster se změněné šablony pomocí `New-AzureRmResourceGroupDeployment` příkazu v okně prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="19231-147">Alternatively, you can download the Resource Manager sample, make changes to it, and create a cluster with the modified template by using the `New-AzureRmResourceGroupDeployment` command in an Azure PowerShell window.</span></span> <span data-ttu-id="19231-148">Zobrazit následující kód pro parametry, které můžete předat do příkazu.</span><span class="sxs-lookup"><span data-stu-id="19231-148">See the following code for the parameters that you pass in to the command.</span></span> <span data-ttu-id="19231-149">Podrobné informace o tom, jak nasazení skupiny prostředků pomocí prostředí PowerShell najdete v článku [nasazení skupiny prostředků pomocí šablony Azure Resource Manager](../azure-resource-manager/resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="19231-149">For detailed information on how to deploy a resource group by using PowerShell, see the article [Deploy a resource group with the Azure Resource Manager template](../azure-resource-manager/resource-group-template-deploy.md).</span></span>

### <a name="deploy-the-diagnostics-extension-to-an-existing-cluster"></a><span data-ttu-id="19231-150">Nasazení rozšíření diagnostiky k existujícímu clusteru</span><span class="sxs-lookup"><span data-stu-id="19231-150">Deploy the Diagnostics extension to an existing cluster</span></span>
<span data-ttu-id="19231-151">Pokud máte existující cluster, který nemá diagnostiky nasazené, nebo pokud chcete upravit existující konfigurace, můžete přidat nebo aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="19231-151">If you have an existing cluster that doesn't have Diagnostics deployed, or if you want to modify an existing configuration, you can add or update it.</span></span> <span data-ttu-id="19231-152">Úprava šablony Resource Manageru, který se používá k vytvoření stávajícího clusteru nebo stáhnout šablonu z portálu, jak je popsáno výše.</span><span class="sxs-lookup"><span data-stu-id="19231-152">Modify the Resource Manager template that's used to create the existing cluster or download the template from the portal as described earlier.</span></span> <span data-ttu-id="19231-153">Upravte soubor template.json provedením následujících úloh.</span><span class="sxs-lookup"><span data-stu-id="19231-153">Modify the template.json file by performing the following tasks.</span></span>

<span data-ttu-id="19231-154">Přidáte nový prostředek úložiště do šablony přidáním do oddílu prostředků.</span><span class="sxs-lookup"><span data-stu-id="19231-154">Add a new storage resource to the template by adding to the resources section.</span></span>

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

 <span data-ttu-id="19231-155">V dalším kroku přidejte do části Parametry hned za definice účet úložiště, mezi `supportLogStorageAccountName` a `vmNodeType0Name`.</span><span class="sxs-lookup"><span data-stu-id="19231-155">Next, add to the parameters section just after the storage account definitions, between `supportLogStorageAccountName` and `vmNodeType0Name`.</span></span> <span data-ttu-id="19231-156">Nahraďte zástupný text *sem zadejte název účtu úložiště* s názvem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="19231-156">Replace the placeholder text *storage account name goes here* with the name of the storage account.</span></span>

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
<span data-ttu-id="19231-157">Potom aktualizovat `VirtualMachineProfile` souboru template.json přidáním následujícího kódu v rámci pole rozšíření.</span><span class="sxs-lookup"><span data-stu-id="19231-157">Then, update the `VirtualMachineProfile` section of the template.json file by adding the following code within the extensions array.</span></span> <span data-ttu-id="19231-158">Nezapomeňte přidat čárka na začátku nebo konci, v závislosti na tom, kde je vložen.</span><span class="sxs-lookup"><span data-stu-id="19231-158">Be sure to add a comma at the beginning or the end, depending on where it's inserted.</span></span>

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

<span data-ttu-id="19231-159">Jakmile upravíte soubor template.json, jak je popsáno, znovu publikujte šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="19231-159">After you modify the template.json file as described, republish the Resource Manager template.</span></span> <span data-ttu-id="19231-160">Pokud byl exportován šablony, spuštěním souboru deploy.ps1 znovu publikuje uzamkl šablony.</span><span class="sxs-lookup"><span data-stu-id="19231-160">If the template was exported, running the deploy.ps1 file republishes the template.</span></span> <span data-ttu-id="19231-161">Poté, co nasadíte, ujistěte se, že **ProvisioningState** je **úspěšné**.</span><span class="sxs-lookup"><span data-stu-id="19231-161">After you deploy, ensure that **ProvisioningState** is **Succeeded**.</span></span>

## <a name="collect-health-and-load-events"></a><span data-ttu-id="19231-162">Shromažďování stavu a načtení události</span><span class="sxs-lookup"><span data-stu-id="19231-162">Collect health and load events</span></span>

<span data-ttu-id="19231-163">Počínaje 5.4 verzi Service Fabric, stavu a zatížení metriky události jsou k dispozici pro kolekci.</span><span class="sxs-lookup"><span data-stu-id="19231-163">Starting with the 5.4 release of Service Fabric, health and load metric events are available for collection.</span></span> <span data-ttu-id="19231-164">Tyto události podle událostí generovaných systému nebo v kódu pomocí stavu nebo zatížení rozhraní API pro vytváření sestav, jako [ReportPartitionHealth](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportpartitionhealth.aspx) nebo [ReportLoad](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportload.aspx).</span><span class="sxs-lookup"><span data-stu-id="19231-164">These events reflect events generated by the system or your code by using the health or load reporting APIs such as [ReportPartitionHealth](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportpartitionhealth.aspx) or [ReportLoad](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportload.aspx).</span></span> <span data-ttu-id="19231-165">To umožňuje pro agregaci a zobrazení stavu systému v čase a pro výstrahy na základě stavu nebo zatížení událostí.</span><span class="sxs-lookup"><span data-stu-id="19231-165">This allows for aggregating and viewing system health over time and for alerting based on health or load events.</span></span> <span data-ttu-id="19231-166">Chcete-li zobrazit tyto události v sadě Visual Studio prohlížeč diagnostických událostí přidat "Microsoft-ServiceFabric:4:0x4000000000000008" do seznamu zprostředkovatelů trasování událostí pro Windows.</span><span class="sxs-lookup"><span data-stu-id="19231-166">To view these events in Visual Studio's Diagnostic Event Viewer add "Microsoft-ServiceFabric:4:0x4000000000000008" to the list of ETW providers.</span></span>

<span data-ttu-id="19231-167">Shromažďovat události, upravte šablonu Resource Manager, aby patří</span><span class="sxs-lookup"><span data-stu-id="19231-167">To collect the events, modify the Resource Manager template to include</span></span>

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

## <a name="collect-reverse-proxy-events"></a><span data-ttu-id="19231-168">Shromažďování událostí reverzní proxy server</span><span class="sxs-lookup"><span data-stu-id="19231-168">Collect reverse proxy events</span></span>

<span data-ttu-id="19231-169">Od verze 5.7 verzi Service Fabric [reverse proxy](service-fabric-reverseproxy.md) události jsou k dispozici pro kolekci.</span><span class="sxs-lookup"><span data-stu-id="19231-169">Starting with the 5.7 release of Service Fabric, [reverse proxy](service-fabric-reverseproxy.md) events are available for collection.</span></span>
<span data-ttu-id="19231-170">Reverzní proxy server vysílá události do dvou kanálů, jeden obsahující chybové události odrážející zpracování chyb a další tak obsahující podrobné události týkající se všechny žádosti požadavku zpracovat reverzní proxy server.</span><span class="sxs-lookup"><span data-stu-id="19231-170">Reverse proxy emits events into two channels, one containing error events reflecting request processing failures and the other one containing verbose events about all the requests processed at the reverse proxy.</span></span> 

1. <span data-ttu-id="19231-171">Shromažďovat události chyb: Chcete-li zobrazit tyto události v sadě Visual Studio prohlížeč diagnostických událostí přidat "Microsoft-ServiceFabric:4:0x4000000000000010" do seznamu zprostředkovatelů trasování událostí pro Windows.</span><span class="sxs-lookup"><span data-stu-id="19231-171">Collect error events: To view these events in Visual Studio's Diagnostic Event Viewer add "Microsoft-ServiceFabric:4:0x4000000000000010" to the list of ETW providers.</span></span>
<span data-ttu-id="19231-172">Shromažďovat události z Azure clusterů, upravte šablonu Resource Manager, aby patří</span><span class="sxs-lookup"><span data-stu-id="19231-172">To collect the events from Azure clusters, modify the Resource Manager template to include</span></span>

```json
  "EtwManifestProviderConfiguration": [
    {
      "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8",
      "scheduledTransferLogLevelFilter": "Information",
      "scheduledTransferKeywordFilter": "4611686018427387920",
      "scheduledTransferPeriod": "PT5M",
      "DefaultEvents": {
        "eventDestination": "ServiceFabricSystemEventTable"
      }
    }
```

2. <span data-ttu-id="19231-173">Shromažďování všech požadavků zpracování události: V sadě Visual Studio na diagnostiky prohlížeče událostí, aktualizace Microsoft-ServiceFabric položku v seznamu zprostředkovatele trasování událostí pro Windows a "Microsoft-ServiceFabric:4:0x4000000000000020".</span><span class="sxs-lookup"><span data-stu-id="19231-173">Collect all request processing events: In Visual Studio's Diagnostic Event Viewer, update the Microsoft-ServiceFabric entry in the ETW provider list to "Microsoft-ServiceFabric:4:0x4000000000000020".</span></span>
<span data-ttu-id="19231-174">Azure Service Fabric clusterů upravte zahrnout šablona resource Manageru</span><span class="sxs-lookup"><span data-stu-id="19231-174">For Azure Service Fabric clusters, modify the resource manager template to include</span></span>

```json
  "EtwManifestProviderConfiguration": [
    {
      "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8",
      "scheduledTransferLogLevelFilter": "Information",
      "scheduledTransferKeywordFilter": "4611686018427387936",
      "scheduledTransferPeriod": "PT5M",
      "DefaultEvents": {
        "eventDestination": "ServiceFabricSystemEventTable"
      }
    }
```
> <span data-ttu-id="19231-175">Doporučujeme uvážlivě umožňuje shromažďování událostí z tohoto kanálu, jak to shromažďuje všechny přenosy přes reverzní proxy server a můžete rychle spotřebovávají kapacitu úložiště.</span><span class="sxs-lookup"><span data-stu-id="19231-175">It is recommended to judiciously enable collecting events from this channel as this collects all traffic through the reverse proxy and can quickly consume storage capacity.</span></span>

<span data-ttu-id="19231-176">Pro Azure Service Fabric clusterů jsou události ze všech uzlů shromažďovat a agregovat do SystemEventTable.</span><span class="sxs-lookup"><span data-stu-id="19231-176">For Azure Service Fabric clusters, the events from all the nodes are collected and aggregated in the SystemEventTable.</span></span>
<span data-ttu-id="19231-177">Podrobné řešení potíží s události reverzní proxy server, naleznete [reverzní proxy server diagnostiky průvodce](service-fabric-reverse-proxy-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="19231-177">For detailed troubleshooting of the reverse proxy events, refer the [reverse proxy diagnostics guide](service-fabric-reverse-proxy-diagnostics.md).</span></span>

## <a name="collect-from-new-eventsource-channels"></a><span data-ttu-id="19231-178">Shromáždit z nové EventSource kanály</span><span class="sxs-lookup"><span data-stu-id="19231-178">Collect from new EventSource channels</span></span>

<span data-ttu-id="19231-179">K aktualizaci diagnostiky pro shromažďování protokolů z nové EventSource kanály, které představují novou aplikaci se o nasazení, provést stejné kroky podle předchozích pokynů pro nastavení diagnostiky pro existující clusteru.</span><span class="sxs-lookup"><span data-stu-id="19231-179">To update Diagnostics to collect logs from new EventSource channels that represent a new application that you're about to deploy, perform the same steps as previously described for the setup of Diagnostics for an existing cluster.</span></span>

<span data-ttu-id="19231-180">Aktualizace `EtwEventSourceProviderConfiguration` v souboru template.json pro přidání položek nové kanály EventSource před použitím konfigurace aktualizací pomocí `New-AzureRmResourceGroupDeployment` příkaz prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="19231-180">Update the `EtwEventSourceProviderConfiguration` section in the template.json file to add entries for the new EventSource channels before you apply the configuration update by using the `New-AzureRmResourceGroupDeployment` PowerShell command.</span></span> <span data-ttu-id="19231-181">Název zdroje událostí je definován jako součást kódu v souboru ServiceEventSource.cs generovaný Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="19231-181">The name of the event source is defined as part of your code in the Visual Studio-generated ServiceEventSource.cs file.</span></span>

<span data-ttu-id="19231-182">Například pokud je váš zdroj událostí Moje Eventsource, přidejte následující kód do tabulky s názvem MyDestinationTableName umístit události z Moje Eventsource.</span><span class="sxs-lookup"><span data-stu-id="19231-182">For example, if your event source is named My-Eventsource, add the following code to place the events from My-Eventsource into a table named MyDestinationTableName.</span></span>

```json
        {
            "provider": "My-Eventsource",
            "scheduledTransferPeriod": "PT5M",
            "DefaultEvents": {
            "eventDestination": "MyDestinationTableName"
            }
        }
```

<span data-ttu-id="19231-183">Pokud chcete shromáždit čítače výkonu nebo protokoly událostí, upravte šablony Resource Manageru pomocí příklady součástí [vytvoření virtuálního počítače s Windows pomocí monitorování a Diagnostika pomocí šablony Azure Resource Manager](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="19231-183">To collect performance counters or event logs, modify the Resource Manager template by using the examples provided in [Create a Windows virtual machine with monitoring and diagnostics by using an Azure Resource Manager template](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="19231-184">Potom se znovu publikujte šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="19231-184">Then, republish the Resource Manager template.</span></span>

## <a name="collect-performance-counters"></a><span data-ttu-id="19231-185">Shromažďování čítačů výkonu</span><span class="sxs-lookup"><span data-stu-id="19231-185">Collect Performance Counters</span></span>

<span data-ttu-id="19231-186">Ke shromažďování metrik výkonu z clusteru, přidejte čítače výkonu pro vaše "WadCfg > DiagnosticMonitorConfiguration" v šabloně Resource Manageru pro váš cluster.</span><span class="sxs-lookup"><span data-stu-id="19231-186">To collect performance metrics from your cluster, add the performance counters to your "WadCfg > DiagnosticMonitorConfiguration" in the Resource Manager template for your cluster.</span></span> <span data-ttu-id="19231-187">V tématu [čítače výkonu služby Fabric](service-fabric-diagnostics-event-generation-perf.md) pro čítače výkonu doporučujeme shromažďování.</span><span class="sxs-lookup"><span data-stu-id="19231-187">See [Service Fabric Performance Counters](service-fabric-diagnostics-event-generation-perf.md) for performance counters that we recommend collecting.</span></span>

<span data-ttu-id="19231-188">Například Zde jsme nastavit jeden čítač výkonu vzorkovat každých 15 sekund (to se dá změnit a dodržuje formát "PT\<čas >\<jednotky >", například by PT3M ukázkové v intervalech tříminutové) a přenášená k příslušné úložiště tabulka každou minutu.</span><span class="sxs-lookup"><span data-stu-id="19231-188">For example, here we set one performance counter, sampled every 15 seconds (this can be changed and follows the format of "PT\<time>\<unit>", for example, PT3M would sample at three minute intervals), and transferred to the appropriate storage table every one minute.</span></span>

  ```json
  "PerformanceCounters": {
      "scheduledTransferPeriod": "PT1M",
      "PerformanceCounterConfiguration": [
          {
              "counterSpecifier": "\\Processor(_Total)\\% Processor Time",
              "sampleRate": "PT15S",
              "unit": "Percent",
              "annotation": [
              ],
              "sinks": ""
          }
      ]
  }
  ```
  
<span data-ttu-id="19231-189">Pokud používáte jímky Application Insights, jak je popsáno v následující části a chcete tyto metriky objeví ve službě Application Insights, pak nezapomeňte přidat název podřízený v části "jímky", jak je uvedeno výše.</span><span class="sxs-lookup"><span data-stu-id="19231-189">If you are using an Application Insights sink, as described in the section below, and want these metrics to show up in Application Insights, then make sure to add the sink name in the "sinks" section as shown above.</span></span> <span data-ttu-id="19231-190">Kromě toho zvažte vytvoření samostatné tabulky k odeslání čítače výkonu, tak jejich nemáte velkého množství lidí dat pocházejících z dalších kanálů protokolování, které jste povolili.</span><span class="sxs-lookup"><span data-stu-id="19231-190">Additionally, consider creating a separate table to send your Performance Counters to, so they don't crowd out the data coming from the other logging channels you have enabled.</span></span>


## <a name="send-logs-to-application-insights"></a><span data-ttu-id="19231-191">Odeslání protokolů s Application Insights</span><span class="sxs-lookup"><span data-stu-id="19231-191">Send logs to Application Insights</span></span>

<span data-ttu-id="19231-192">Odeslání dat monitorování a Diagnostika Statistika aplikace (AI) lze provést v rámci konfigurace WAD.</span><span class="sxs-lookup"><span data-stu-id="19231-192">Sending monitoring and diagnostics data to Application Insights (AI) can be done as part of the WAD configuration.</span></span> <span data-ttu-id="19231-193">Pokud se rozhodnete použít AI pro analýzu událostí a vizualizace, přečtěte si [analýza události a vizualizace s Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md) nastavit jímky AI jako součást vaší "WadCfg".</span><span class="sxs-lookup"><span data-stu-id="19231-193">If you decide to use AI for event analysis and visualization, read [Event Analysis and Visualization with Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md) to set up an AI Sink as part of your "WadCfg".</span></span>

## <a name="next-steps"></a><span data-ttu-id="19231-194">Další kroky</span><span class="sxs-lookup"><span data-stu-id="19231-194">Next steps</span></span>

<span data-ttu-id="19231-195">Jakmile jste správně nakonfigurovali Azure diagnostics, zobrazí se data v tabulkách úložiště z protokolů trasování událostí pro Windows a EventSource.</span><span class="sxs-lookup"><span data-stu-id="19231-195">Once you have correctly configured Azure diagnostics, you will see data in your Storage tables from the ETW and EventSource logs.</span></span> <span data-ttu-id="19231-196">Pokud se rozhodnete použít OMS, Kibana nebo jakékoli jiné data analýzy a vizualizace platformě, která není přímo nakonfigurovanou v šabloně Resource Manager, ujistěte se, zda je nastavení platformou vaší volby pro čtení dat z těchto tabulek úložiště.</span><span class="sxs-lookup"><span data-stu-id="19231-196">If you choose to use OMS, Kibana, or any other data analytics and visualization platform that is not directly configured in the Resource Manager template, make sure to set up the platform of your choice to read in the data from these storage tables.</span></span> <span data-ttu-id="19231-197">Díky tomuto pro OMS je relativně jednoduchá a je vysvětleno v [událostí a protokolu analýzu prostřednictvím OMS](service-fabric-diagnostics-event-analysis-oms.md).</span><span class="sxs-lookup"><span data-stu-id="19231-197">Doing this for OMS is relatively trivial, and is explained in [Event and log analysis through OMS](service-fabric-diagnostics-event-analysis-oms.md).</span></span> <span data-ttu-id="19231-198">Application Insights je bit ve speciálním případě v tomto smyslu získáte vzhledem k tomu může být nakonfigurovaný jako součást konfigurace rozšíření diagnostiky, takže [příslušném článku](service-fabric-diagnostics-event-analysis-appinsights.md) Pokud se rozhodnete použít AI.</span><span class="sxs-lookup"><span data-stu-id="19231-198">Application Insights is a bit of a special case in this sense, since it can be configured as part of the Diagnostics extension configuration, so refer to the [appropriate article](service-fabric-diagnostics-event-analysis-appinsights.md) if you choose to use AI.</span></span>

>[!NOTE]
><span data-ttu-id="19231-199">Aktuálně neexistuje žádný způsob, jak filtrovat nebo je naopak události, které se odesílají do tabulky.</span><span class="sxs-lookup"><span data-stu-id="19231-199">There is currently no way to filter or groom the events that are sent to the table.</span></span> <span data-ttu-id="19231-200">Pokud nemáte implementujte proces odebrání události v tabulce, tabulka pořád roste s tím.</span><span class="sxs-lookup"><span data-stu-id="19231-200">If you don't implement a process to remove events from the table, the table will continue to grow.</span></span> <span data-ttu-id="19231-201">V současné době je příkladem data výmazu dat služby spuštěné [sledovací zařízení ukázka](https://github.com/Azure-Samples/service-fabric-watchdog-service), a se doporučuje, když napsat jeden pro sami taky Pokud dobrý důvody pro ukládání protokolů nad rámec 30 nebo 90 den časový rámec.</span><span class="sxs-lookup"><span data-stu-id="19231-201">Currently, there is an example of a data grooming service running in the [Watchdog sample](https://github.com/Azure-Samples/service-fabric-watchdog-service), and it is recommended that you write one for yourself as well, unless there is a good reason for you to store logs beyond a 30 or 90 day timeframe.</span></span>

* [<span data-ttu-id="19231-202">Zjistěte, jak shromažďování čítače výkonu nebo protokoly pomocí rozšíření diagnostiky</span><span class="sxs-lookup"><span data-stu-id="19231-202">Learn how to collect performance counters or logs by using the Diagnostics extension</span></span>](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="19231-203">Analýza události a vizualizace s Application Insights</span><span class="sxs-lookup"><span data-stu-id="19231-203">Event Analysis and Visualization with Application Insights</span></span>](service-fabric-diagnostics-event-analysis-appinsights.md)
* [<span data-ttu-id="19231-204">Analýza události a vizualizace s OMS</span><span class="sxs-lookup"><span data-stu-id="19231-204">Event Analysis and Visualization with OMS</span></span>](service-fabric-diagnostics-event-analysis-oms.md)