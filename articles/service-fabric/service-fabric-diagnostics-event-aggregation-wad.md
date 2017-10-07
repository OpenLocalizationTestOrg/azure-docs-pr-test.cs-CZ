---
title: "aaaAzure Service Fabric událostí agregace s Windows Azure Diagnostics | Microsoft Docs"
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
ms.openlocfilehash: 4827ce66620e61c5b4a8682db55952333113188a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="event-aggregation-and-collection-using-windows-azure-diagnostics"></a><span data-ttu-id="6e9f0-103">Seskupení událostí a kolekce s použitím Windows Azure Diagnostics</span><span class="sxs-lookup"><span data-stu-id="6e9f0-103">Event aggregation and collection using Windows Azure Diagnostics</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="6e9f0-104">Windows</span><span class="sxs-lookup"><span data-stu-id="6e9f0-104">Windows</span></span>](service-fabric-diagnostics-event-aggregation-wad.md)
> * [<span data-ttu-id="6e9f0-105">Linux</span><span class="sxs-lookup"><span data-stu-id="6e9f0-105">Linux</span></span>](service-fabric-diagnostics-event-aggregation-lad.md)
>
>

<span data-ttu-id="6e9f0-106">Když používáte cluster služby Azure Service Fabric, je vhodné toocollect hello protokoly ze všech uzlů hello v centrálním umístění.</span><span class="sxs-lookup"><span data-stu-id="6e9f0-106">When you're running an Azure Service Fabric cluster, it's a good idea toocollect hello logs from all hello nodes in a central location.</span></span> <span data-ttu-id="6e9f0-107">S protokoly hello v centrálním umístění, vám pomáhají analyzovat a vyřešit problémy v clusteru nebo problémy v hello aplikací a služeb spuštěných v daném clusteru.</span><span class="sxs-lookup"><span data-stu-id="6e9f0-107">Having hello logs in a central location helps you analyze and troubleshoot issues in your cluster, or issues in hello applications and services running in that cluster.</span></span>

<span data-ttu-id="6e9f0-108">Jedním ze způsobů tooupload a shromáždit protokoly je toouse hello Windows Azure Diagnostics (WAD) přípony, který odešle tooAzure protokoly úložiště a také obsahuje hello možnost toosend protokoly tooAzure Application Insights nebo Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="6e9f0-108">One way tooupload and collect logs is toouse hello Windows Azure Diagnostics (WAD) extension, which uploads logs tooAzure Storage, and also has hello option toosend logs tooAzure Application Insights or Event Hubs.</span></span> <span data-ttu-id="6e9f0-109">Můžete také použít externího procesu tooread hello události z úložiště a umístit je o analýzy platformy produkt, například [analýzy protokolů OMS](../log-analytics/log-analytics-service-fabric.md) nebo jiné řešení pro analýzu protokolu.</span><span class="sxs-lookup"><span data-stu-id="6e9f0-109">You can also use an external process tooread hello events from storage and place them in an analysis platform product, such as [OMS Log Analytics](../log-analytics/log-analytics-service-fabric.md) or another log-parsing solution.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6e9f0-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="6e9f0-110">Prerequisites</span></span>
<span data-ttu-id="6e9f0-111">Tyto nástroje jsou použité tooperform některé hello operací v tomto dokumentu:</span><span class="sxs-lookup"><span data-stu-id="6e9f0-111">These tools are used tooperform some of hello operations in this document:</span></span>

* <span data-ttu-id="6e9f0-112">[Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md) (týkající se tooAzure cloudové služby, ale má správné informace a příklady)</span><span class="sxs-lookup"><span data-stu-id="6e9f0-112">[Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md) (related tooAzure Cloud Services but has good information and examples)</span></span>
* [<span data-ttu-id="6e9f0-113">Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="6e9f0-113">Azure Resource Manager</span></span>](../azure-resource-manager/resource-group-overview.md)
* [<span data-ttu-id="6e9f0-114">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="6e9f0-114">Azure PowerShell</span></span>](/powershell/azure/overview)
* [<span data-ttu-id="6e9f0-115">Klient Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="6e9f0-115">Azure Resource Manager client</span></span>](https://github.com/projectkudu/ARMClient)
* [<span data-ttu-id="6e9f0-116">Šablona Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="6e9f0-116">Azure Resource Manager template</span></span>](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="log-and-event-sources"></a><span data-ttu-id="6e9f0-117">Protokol a událostí zdroje</span><span class="sxs-lookup"><span data-stu-id="6e9f0-117">Log and event sources</span></span>

### <a name="service-fabric-platform-events"></a><span data-ttu-id="6e9f0-118">Události platformy Service Fabric</span><span class="sxs-lookup"><span data-stu-id="6e9f0-118">Service Fabric platform events</span></span>
<span data-ttu-id="6e9f0-119">Jak je popsáno v [v tomto článku](service-fabric-diagnostics-event-generation-infra.md), Service Fabric nastaví můžete s kanály, několik protokolování se na pole, která hello následující kanály jsou snadno nakonfigurované WAD toosend monitorování a Diagnostika data tooa tabulku úložiště nebo jinde:</span><span class="sxs-lookup"><span data-stu-id="6e9f0-119">As discussed in [this article](service-fabric-diagnostics-event-generation-infra.md), Service Fabric sets you up with a few out-of-the-box logging channels, of which hello following channels are easily configured with WAD toosend monitoring and diagnostics data tooa storage table or elsewhere:</span></span>
  * <span data-ttu-id="6e9f0-120">Provozní události: vyšší úrovně operace, které hello provede platformy Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="6e9f0-120">Operational events: higher-level operations that hello Service Fabric platform performs.</span></span> <span data-ttu-id="6e9f0-121">Mezi příklady patří vytvoření aplikací a služeb, uzel změny stavu a informace o upgradu.</span><span class="sxs-lookup"><span data-stu-id="6e9f0-121">Examples include creation of applications and services, node state changes, and upgrade information.</span></span> <span data-ttu-id="6e9f0-122">Tyto jsou vygenerované jako protokoly trasování událostí pro Windows (ETW)</span><span class="sxs-lookup"><span data-stu-id="6e9f0-122">These are emitted as Event Tracing for Windows (ETW) logs</span></span>
  * [<span data-ttu-id="6e9f0-123">Programování událostí modelu Reliable Actors</span><span class="sxs-lookup"><span data-stu-id="6e9f0-123">Reliable Actors programming model events</span></span>](service-fabric-reliable-actors-diagnostics.md)
  * [<span data-ttu-id="6e9f0-124">Spolehlivé služby programovací model události</span><span class="sxs-lookup"><span data-stu-id="6e9f0-124">Reliable Services programming model events</span></span>](service-fabric-reliable-services-diagnostics.md)

### <a name="application-events"></a><span data-ttu-id="6e9f0-125">Události aplikace</span><span class="sxs-lookup"><span data-stu-id="6e9f0-125">Application events</span></span>
 <span data-ttu-id="6e9f0-126">Události vygenerované z vašich aplikací a služeb kódu a zapsané pomocí hello EventSource pomocná třída součástí hello šablony sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6e9f0-126">Events emitted from your applications' and services' code and written out by using hello EventSource helper class provided in hello Visual Studio templates.</span></span> <span data-ttu-id="6e9f0-127">Další informace o tom, jak toowrite EventSource protokolů z vaší aplikace naleznete v tématu [monitorování a Diagnostika služby v instalačním programu místním počítači vývoj](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).</span><span class="sxs-lookup"><span data-stu-id="6e9f0-127">For more information on how toowrite EventSource logs from your application, see [Monitor and diagnose services in a local machine development setup](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).</span></span>

## <a name="deploy-hello-diagnostics-extension"></a><span data-ttu-id="6e9f0-128">Nasazení rozšíření diagnostiky hello</span><span class="sxs-lookup"><span data-stu-id="6e9f0-128">Deploy hello Diagnostics extension</span></span>
<span data-ttu-id="6e9f0-129">Hello prvním krokem při shromažďování protokolů je rozšíření diagnostiky hello toodeploy na každém hello virtuálních počítačů v clusteru Service Fabric hello.</span><span class="sxs-lookup"><span data-stu-id="6e9f0-129">hello first step in collecting logs is toodeploy hello Diagnostics extension on each of hello VMs in hello Service Fabric cluster.</span></span> <span data-ttu-id="6e9f0-130">Hello rozšíření diagnostiky shromažďuje protokoly na každý virtuální počítač a odesílá je toohello účet úložiště, který určíte.</span><span class="sxs-lookup"><span data-stu-id="6e9f0-130">hello Diagnostics extension collects logs on each VM and uploads them toohello storage account that you specify.</span></span> <span data-ttu-id="6e9f0-131">kroky Hello trochu záviset na tom, zda používáte hello portál Azure nebo Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="6e9f0-131">hello steps vary a little based on whether you use hello Azure portal or Azure Resource Manager.</span></span> <span data-ttu-id="6e9f0-132">Hello kroky také lišit v závislosti na tom, jestli je součástí vytváření clusteru hello nasazení, nebo je pro cluster, který již existuje.</span><span class="sxs-lookup"><span data-stu-id="6e9f0-132">hello steps also vary based on whether hello deployment is part of cluster creation or is for a cluster that already exists.</span></span> <span data-ttu-id="6e9f0-133">Podívejme se na hello kroky pro jednotlivé scénáře.</span><span class="sxs-lookup"><span data-stu-id="6e9f0-133">Let's look at hello steps for each scenario.</span></span>

### <a name="deploy-hello-diagnostics-extension-as-part-of-cluster-creation-through-azure-portal"></a><span data-ttu-id="6e9f0-134">Nasazení hello rozšíření diagnostiky jako součást vytváření clusteru prostřednictvím portálu Azure</span><span class="sxs-lookup"><span data-stu-id="6e9f0-134">Deploy hello Diagnostics extension as part of cluster creation through Azure portal</span></span>
<span data-ttu-id="6e9f0-135">toodeploy hello diagnostiky rozšíření toohello virtuálních počítačů v clusteru hello jako součást vytváření clusteru, použijte panel nastavení diagnostiky hello uvedené v následující hello obrázek – ověřte, zda diagnostiky příliš**na** (hello výchozí nastavení) .</span><span class="sxs-lookup"><span data-stu-id="6e9f0-135">toodeploy hello Diagnostics extension toohello VMs in hello cluster as part of cluster creation, you use hello Diagnostics settings panel shown in hello following image - ensure that Diagnostics is set too**On** (hello default setting).</span></span> <span data-ttu-id="6e9f0-136">Po vytvoření clusteru hello, nelze změnit tato nastavení pomocí portálu hello.</span><span class="sxs-lookup"><span data-stu-id="6e9f0-136">After you create hello cluster, you can't change these settings by using hello portal.</span></span>

![Nastavení Azure Diagnostics hello portálu pro vytvoření clusteru](media/service-fabric-diagnostics-event-aggregation-wad/azure-enable-diagnostics.png)

<span data-ttu-id="6e9f0-138">Při vytváření clusteru pomocí portálu hello, důrazně doporučujeme, abyste si stáhli hello šablony **předtím, než kliknete na tlačítko OK** toocreate hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="6e9f0-138">When you're creating a cluster by using hello portal, we highly recommend that you download hello template **before you click OK** toocreate hello cluster.</span></span> <span data-ttu-id="6e9f0-139">Podrobnosti najdete příliš[nastavit cluster Service Fabric pomocí šablony Azure Resource Manager](service-fabric-cluster-creation-via-arm.md).</span><span class="sxs-lookup"><span data-stu-id="6e9f0-139">For details, refer too[Set up a Service Fabric cluster by using an Azure Resource Manager template](service-fabric-cluster-creation-via-arm.md).</span></span> <span data-ttu-id="6e9f0-140">Změny toomake hello šablony budete potřebovat později, protože nemůže provést některé změny pomocí portálu hello.</span><span class="sxs-lookup"><span data-stu-id="6e9f0-140">You'll need hello template toomake changes later, because you can't make some changes by using hello portal.</span></span>

### <a name="deploy-hello-diagnostics-extension-as-part-of-cluster-creation-by-using-azure-resource-manager"></a><span data-ttu-id="6e9f0-141">Nasazení hello rozšíření diagnostiky jako součást vytváření clusteru pomocí Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="6e9f0-141">Deploy hello Diagnostics extension as part of cluster creation by using Azure Resource Manager</span></span>
<span data-ttu-id="6e9f0-142">toocreate cluster pomocí Resource Manageru, potřebujete tooadd hello diagnostiky konfigurace JSON toohello úplné clusteru šablony Resource Manageru před vytvořením clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="6e9f0-142">toocreate a cluster by using Resource Manager, you need tooadd hello Diagnostics configuration JSON toohello full cluster Resource Manager template before you create hello cluster.</span></span> <span data-ttu-id="6e9f0-143">Poskytujeme šablony Resource Manageru ukázkový pět VM cluster s konfigurací diagnostiky přidat tooit jako součást naše ukázky šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="6e9f0-143">We provide a sample five-VM cluster Resource Manager template with Diagnostics configuration added tooit as part of our Resource Manager template samples.</span></span> <span data-ttu-id="6e9f0-144">Zobrazí se v tomto umístění v galerii Azure Samples hello: [pěti uzly clusteru s ukázkou šablony správce prostředků diagnostiky](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype-wad).</span><span class="sxs-lookup"><span data-stu-id="6e9f0-144">You can see it at this location in hello Azure Samples gallery: [Five-node cluster with Diagnostics Resource Manager template sample](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype-wad).</span></span>

<span data-ttu-id="6e9f0-145">nastavení diagnostiky hello toosee v hello šablony Resource Manageru, otevřete hello soubor azuredeploy.json a vyhledejte **IaaSDiagnostics**.</span><span class="sxs-lookup"><span data-stu-id="6e9f0-145">toosee hello Diagnostics setting in hello Resource Manager template, open hello azuredeploy.json file and search for **IaaSDiagnostics**.</span></span> <span data-ttu-id="6e9f0-146">toocreate cluster pomocí této šablony, vyberte hello **nasazení tooAzure** tlačítko k dispozici na předchozí odkaz hello.</span><span class="sxs-lookup"><span data-stu-id="6e9f0-146">toocreate a cluster by using this template, select hello **Deploy tooAzure** button available at hello previous link.</span></span>

<span data-ttu-id="6e9f0-147">Alternativně můžete stáhnout ukázkový hello Resource Manager, zkontrolujte změny tooit a vytvořte cluster se změněné šablony hello pomocí hello `New-AzureRmResourceGroupDeployment` příkazu v okně prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="6e9f0-147">Alternatively, you can download hello Resource Manager sample, make changes tooit, and create a cluster with hello modified template by using hello `New-AzureRmResourceGroupDeployment` command in an Azure PowerShell window.</span></span> <span data-ttu-id="6e9f0-148">Viz následující kód pro hello parametry, které se předávají v příkazu toohello hello.</span><span class="sxs-lookup"><span data-stu-id="6e9f0-148">See hello following code for hello parameters that you pass in toohello command.</span></span> <span data-ttu-id="6e9f0-149">Podrobné informace o tom, jak toodeploy prostředek skupiny pomocí prostředí PowerShell, najdete v článku hello [nasazení skupiny prostředků pomocí šablony Azure Resource Manager hello](../azure-resource-manager/resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="6e9f0-149">For detailed information on how toodeploy a resource group by using PowerShell, see hello article [Deploy a resource group with hello Azure Resource Manager template](../azure-resource-manager/resource-group-template-deploy.md).</span></span>

### <a name="deploy-hello-diagnostics-extension-tooan-existing-cluster"></a><span data-ttu-id="6e9f0-150">Nasazení hello diagnostiky rozšíření tooan stávajícího clusteru</span><span class="sxs-lookup"><span data-stu-id="6e9f0-150">Deploy hello Diagnostics extension tooan existing cluster</span></span>
<span data-ttu-id="6e9f0-151">Pokud máte existující cluster, který nemá diagnostiky nasazené, nebo pokud chcete toomodify existující konfigurace, můžete přidat nebo aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="6e9f0-151">If you have an existing cluster that doesn't have Diagnostics deployed, or if you want toomodify an existing configuration, you can add or update it.</span></span> <span data-ttu-id="6e9f0-152">Upravit šablonu hello Resource Manager, který je použité toocreate hello existující cluster nebo stáhnout hello šablonu z portálu hello, jak je popsáno výše.</span><span class="sxs-lookup"><span data-stu-id="6e9f0-152">Modify hello Resource Manager template that's used toocreate hello existing cluster or download hello template from hello portal as described earlier.</span></span> <span data-ttu-id="6e9f0-153">Upravte soubor template.json hello provedením hello následující úlohy.</span><span class="sxs-lookup"><span data-stu-id="6e9f0-153">Modify hello template.json file by performing hello following tasks.</span></span>

<span data-ttu-id="6e9f0-154">Přidáte novou šablonu toohello prostředků úložiště přidáním toohello oddílu prostředků.</span><span class="sxs-lookup"><span data-stu-id="6e9f0-154">Add a new storage resource toohello template by adding toohello resources section.</span></span>

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

 <span data-ttu-id="6e9f0-155">Dál přidejte toohello parametry části bezprostředně za hello Definice účet úložiště, mezi `supportLogStorageAccountName` a `vmNodeType0Name`.</span><span class="sxs-lookup"><span data-stu-id="6e9f0-155">Next, add toohello parameters section just after hello storage account definitions, between `supportLogStorageAccountName` and `vmNodeType0Name`.</span></span> <span data-ttu-id="6e9f0-156">Nahraďte zástupný text hello *sem zadejte název účtu úložiště* hello název účtu úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="6e9f0-156">Replace hello placeholder text *storage account name goes here* with hello name of hello storage account.</span></span>

```json
    "applicationDiagnosticsStorageAccountType": {
      "type": "string",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS"
      ],
      "defaultValue": "Standard_LRS",
      "metadata": {
        "description": "Replication option for hello application diagnostics storage account"
      }
    },
    "applicationDiagnosticsStorageAccountName": {
      "type": "string",
      "defaultValue": "storage account name goes here",
      "metadata": {
        "description": "Name for hello storage account that contains application diagnostics data from hello cluster"
      }
    },
```
<span data-ttu-id="6e9f0-157">Aktualizujte hello `VirtualMachineProfile` části souboru template.json hello přidáním hello následující kód do pole rozšíření hello.</span><span class="sxs-lookup"><span data-stu-id="6e9f0-157">Then, update hello `VirtualMachineProfile` section of hello template.json file by adding hello following code within hello extensions array.</span></span> <span data-ttu-id="6e9f0-158">Být jisti tooadd čárka na začátku hello nebo hello end, v závislosti na tom, kde je vložen.</span><span class="sxs-lookup"><span data-stu-id="6e9f0-158">Be sure tooadd a comma at hello beginning or hello end, depending on where it's inserted.</span></span>

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

<span data-ttu-id="6e9f0-159">Po úpravě souboru template.json hello, jak je popsáno, znovu publikujte šablony Resource Manageru hello.</span><span class="sxs-lookup"><span data-stu-id="6e9f0-159">After you modify hello template.json file as described, republish hello Resource Manager template.</span></span> <span data-ttu-id="6e9f0-160">Pokud byl exportován hello šablony, znovu publikuje spouštění souboru deploy.ps1 hello uzamkl hello šablony.</span><span class="sxs-lookup"><span data-stu-id="6e9f0-160">If hello template was exported, running hello deploy.ps1 file republishes hello template.</span></span> <span data-ttu-id="6e9f0-161">Poté, co nasadíte, ujistěte se, že **ProvisioningState** je **úspěšné**.</span><span class="sxs-lookup"><span data-stu-id="6e9f0-161">After you deploy, ensure that **ProvisioningState** is **Succeeded**.</span></span>

## <a name="collect-health-and-load-events"></a><span data-ttu-id="6e9f0-162">Shromažďování stavu a načtení události</span><span class="sxs-lookup"><span data-stu-id="6e9f0-162">Collect health and load events</span></span>

<span data-ttu-id="6e9f0-163">Od verze hello 5.4 Service Fabric, stavu a zatížení metriky události jsou k dispozici pro kolekci.</span><span class="sxs-lookup"><span data-stu-id="6e9f0-163">Starting with hello 5.4 release of Service Fabric, health and load metric events are available for collection.</span></span> <span data-ttu-id="6e9f0-164">Tyto události podle událostí generovaných hello systému nebo v kódu pomocí hello stavu nebo zatížení rozhraní API pro vytváření sestav, jako [ReportPartitionHealth](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportpartitionhealth.aspx) nebo [ReportLoad](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportload.aspx).</span><span class="sxs-lookup"><span data-stu-id="6e9f0-164">These events reflect events generated by hello system or your code by using hello health or load reporting APIs such as [ReportPartitionHealth](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportpartitionhealth.aspx) or [ReportLoad](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportload.aspx).</span></span> <span data-ttu-id="6e9f0-165">To umožňuje pro agregaci a zobrazení stavu systému v čase a pro výstrahy na základě stavu nebo zatížení událostí.</span><span class="sxs-lookup"><span data-stu-id="6e9f0-165">This allows for aggregating and viewing system health over time and for alerting based on health or load events.</span></span> <span data-ttu-id="6e9f0-166">Přidat tyto události v sadě Visual Studio prohlížeč diagnostických událostí tooview "Microsoft-ServiceFabric:4:0x4000000000000008" toohello seznamu zprostředkovatelů trasování událostí pro Windows.</span><span class="sxs-lookup"><span data-stu-id="6e9f0-166">tooview these events in Visual Studio's Diagnostic Event Viewer add "Microsoft-ServiceFabric:4:0x4000000000000008" toohello list of ETW providers.</span></span>

<span data-ttu-id="6e9f0-167">události hello toocollect, upravte tooinclude šablony Resource Manageru hello</span><span class="sxs-lookup"><span data-stu-id="6e9f0-167">toocollect hello events, modify hello Resource Manager template tooinclude</span></span>

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

## <a name="collect-reverse-proxy-events"></a><span data-ttu-id="6e9f0-168">Shromažďování událostí reverzní proxy server</span><span class="sxs-lookup"><span data-stu-id="6e9f0-168">Collect reverse proxy events</span></span>

<span data-ttu-id="6e9f0-169">Od verze hello 5.7 Service Fabric [reverse proxy](service-fabric-reverseproxy.md) události jsou k dispozici pro kolekci.</span><span class="sxs-lookup"><span data-stu-id="6e9f0-169">Starting with hello 5.7 release of Service Fabric, [reverse proxy](service-fabric-reverseproxy.md) events are available for collection.</span></span>
<span data-ttu-id="6e9f0-170">Reverzní proxy server vysílá události do dvou kanálů, jednu obsahující chybu události odrážející selhání zpracování požadavku a další jeden obsahující podrobné události týkající se všech požadavků hello zpracovat reverzní proxy server hello hello.</span><span class="sxs-lookup"><span data-stu-id="6e9f0-170">Reverse proxy emits events into two channels, one containing error events reflecting request processing failures and hello other one containing verbose events about all hello requests processed at hello reverse proxy.</span></span> 

1. <span data-ttu-id="6e9f0-171">Shromažďovat události chyb: tooview přidat tyto události v sadě Visual Studio prohlížeč diagnostických událostí "Microsoft-ServiceFabric:4:0x4000000000000010" toohello seznamu zprostředkovatelů trasování událostí pro Windows.</span><span class="sxs-lookup"><span data-stu-id="6e9f0-171">Collect error events: tooview these events in Visual Studio's Diagnostic Event Viewer add "Microsoft-ServiceFabric:4:0x4000000000000010" toohello list of ETW providers.</span></span>
<span data-ttu-id="6e9f0-172">toocollect hello události z Azure clusterů, upravte tooinclude šablony Resource Manageru hello</span><span class="sxs-lookup"><span data-stu-id="6e9f0-172">toocollect hello events from Azure clusters, modify hello Resource Manager template tooinclude</span></span>

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

2. <span data-ttu-id="6e9f0-173">Shromažďování všech požadavků zpracování události: V sadě Visual Studio na diagnostiky prohlížeče událostí, položku hello Microsoft ServiceFabric update v hello seznam zprostředkovatelů trasování událostí pro Windows příliš "Microsoft-ServiceFabric:4:0x4000000000000020".</span><span class="sxs-lookup"><span data-stu-id="6e9f0-173">Collect all request processing events: In Visual Studio's Diagnostic Event Viewer, update hello Microsoft-ServiceFabric entry in hello ETW provider list too"Microsoft-ServiceFabric:4:0x4000000000000020".</span></span>
<span data-ttu-id="6e9f0-174">Azure Service Fabric clusterů upravte tooinclude šablony správce prostředků hello</span><span class="sxs-lookup"><span data-stu-id="6e9f0-174">For Azure Service Fabric clusters, modify hello resource manager template tooinclude</span></span>

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
> <span data-ttu-id="6e9f0-175">Doporučujeme toojudiciously Povolit shromažďování událostí z tohoto kanálu, jako to shromažďuje všechny přenosy přes reverzní proxy server hello a můžete rychle spotřebovávají kapacitu úložiště.</span><span class="sxs-lookup"><span data-stu-id="6e9f0-175">It is recommended toojudiciously enable collecting events from this channel as this collects all traffic through hello reverse proxy and can quickly consume storage capacity.</span></span>

<span data-ttu-id="6e9f0-176">Pro Azure Service Fabric clusterů hello události ze všech uzlů hello jsou shromažďovány a agregovat do hello SystemEventTable.</span><span class="sxs-lookup"><span data-stu-id="6e9f0-176">For Azure Service Fabric clusters, hello events from all hello nodes are collected and aggregated in hello SystemEventTable.</span></span>
<span data-ttu-id="6e9f0-177">Podrobné řešení potíží s událostí reverzní proxy server hello odkazovat hello [reverzní proxy server diagnostiky průvodce](service-fabric-reverse-proxy-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="6e9f0-177">For detailed troubleshooting of hello reverse proxy events, refer hello [reverse proxy diagnostics guide](service-fabric-reverse-proxy-diagnostics.md).</span></span>

## <a name="collect-from-new-eventsource-channels"></a><span data-ttu-id="6e9f0-178">Shromáždit z nové EventSource kanály</span><span class="sxs-lookup"><span data-stu-id="6e9f0-178">Collect from new EventSource channels</span></span>

<span data-ttu-id="6e9f0-179">tooupdate diagnostiky toocollect protokoly z nové EventSource kanály, které představují novou aplikaci, která jste o toodeploy, provést hello stejný postup jako výše popsané hello nastavení diagnostiky pro existující cluster.</span><span class="sxs-lookup"><span data-stu-id="6e9f0-179">tooupdate Diagnostics toocollect logs from new EventSource channels that represent a new application that you're about toodeploy, perform hello same steps as previously described for hello setup of Diagnostics for an existing cluster.</span></span>

<span data-ttu-id="6e9f0-180">Aktualizovat hello `EtwEventSourceProviderConfiguration` kapitoly položky tooadd souboru template.json hello hello nové EventSource kanály než použijete hello konfigurace aktualizace pomocí hello `New-AzureRmResourceGroupDeployment` příkaz prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="6e9f0-180">Update hello `EtwEventSourceProviderConfiguration` section in hello template.json file tooadd entries for hello new EventSource channels before you apply hello configuration update by using hello `New-AzureRmResourceGroupDeployment` PowerShell command.</span></span> <span data-ttu-id="6e9f0-181">Název Hello hello zdroje událostí je definován jako součást kódu v souboru generovaný Visual Studio ServiceEventSource.cs hello.</span><span class="sxs-lookup"><span data-stu-id="6e9f0-181">hello name of hello event source is defined as part of your code in hello Visual Studio-generated ServiceEventSource.cs file.</span></span>

<span data-ttu-id="6e9f0-182">Například pokud váš zdroj událostí s názvem Moje Eventsource, přidejte následující kód tooplace hello události z Moje Eventsource do tabulky s názvem MyDestinationTableName hello.</span><span class="sxs-lookup"><span data-stu-id="6e9f0-182">For example, if your event source is named My-Eventsource, add hello following code tooplace hello events from My-Eventsource into a table named MyDestinationTableName.</span></span>

```json
        {
            "provider": "My-Eventsource",
            "scheduledTransferPeriod": "PT5M",
            "DefaultEvents": {
            "eventDestination": "MyDestinationTableName"
            }
        }
```

<span data-ttu-id="6e9f0-183">čítače výkonu toocollect nebo protokoly událostí, změna šablony Resource Manageru hello pomocí hello příklady [vytvoření virtuálního počítače s Windows pomocí monitorování a Diagnostika pomocí šablony Azure Resource Manager](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="6e9f0-183">toocollect performance counters or event logs, modify hello Resource Manager template by using hello examples provided in [Create a Windows virtual machine with monitoring and diagnostics by using an Azure Resource Manager template](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="6e9f0-184">Potom se znovu publikujte šablony Resource Manageru hello.</span><span class="sxs-lookup"><span data-stu-id="6e9f0-184">Then, republish hello Resource Manager template.</span></span>

## <a name="collect-performance-counters"></a><span data-ttu-id="6e9f0-185">Shromažďování čítačů výkonu</span><span class="sxs-lookup"><span data-stu-id="6e9f0-185">Collect Performance Counters</span></span>

<span data-ttu-id="6e9f0-186">metriky výkonu toocollect z clusteru, přidejte tooyour čítače výkonu hello "> WadCfg DiagnosticMonitorConfiguration" v hello šablony Resource Manageru pro váš cluster.</span><span class="sxs-lookup"><span data-stu-id="6e9f0-186">toocollect performance metrics from your cluster, add hello performance counters tooyour "WadCfg > DiagnosticMonitorConfiguration" in hello Resource Manager template for your cluster.</span></span> <span data-ttu-id="6e9f0-187">V tématu [čítače výkonu služby Fabric](service-fabric-diagnostics-event-generation-perf.md) pro čítače výkonu doporučujeme shromažďování.</span><span class="sxs-lookup"><span data-stu-id="6e9f0-187">See [Service Fabric Performance Counters](service-fabric-diagnostics-event-generation-perf.md) for performance counters that we recommend collecting.</span></span>

<span data-ttu-id="6e9f0-188">Například Zde jsme nastavit jeden čítač výkonu vzorkovat každých 15 sekund (to se dá změnit a způsobem hello formát "PT\<čas >\<jednotky >", například by PT3M ukázkové na tři minutách) a přenáší toohello příslušné úložiště tabulka každou minutu.</span><span class="sxs-lookup"><span data-stu-id="6e9f0-188">For example, here we set one performance counter, sampled every 15 seconds (this can be changed and follows hello format of "PT\<time>\<unit>", for example, PT3M would sample at three minute intervals), and transferred toohello appropriate storage table every one minute.</span></span>

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
  
<span data-ttu-id="6e9f0-189">Pokud používáte jímky Application Insights, jak je popsáno v následující části hello a chcete tyto metriky tooshow si ve službě Application Insights, zkontrolujte že tooadd hello podřízený název v části "jímky" hello, jak je uvedeno výše.</span><span class="sxs-lookup"><span data-stu-id="6e9f0-189">If you are using an Application Insights sink, as described in hello section below, and want these metrics tooshow up in Application Insights, then make sure tooadd hello sink name in hello "sinks" section as shown above.</span></span> <span data-ttu-id="6e9f0-190">Kromě toho, zvažte vytvoření samostatné tabulky toosend čítače výkonu, tak jejich nemáte velkého množství lidí si hello dat pocházejících z hello jinými protokolování kanály, které jste povolili.</span><span class="sxs-lookup"><span data-stu-id="6e9f0-190">Additionally, consider creating a separate table toosend your Performance Counters to, so they don't crowd out hello data coming from hello other logging channels you have enabled.</span></span>


## <a name="send-logs-tooapplication-insights"></a><span data-ttu-id="6e9f0-191">Odeslání protokolů tooApplication statistiky</span><span class="sxs-lookup"><span data-stu-id="6e9f0-191">Send logs tooApplication Insights</span></span>

<span data-ttu-id="6e9f0-192">Odesílání dat tooApplication monitorování a Diagnostika přehledy (AI) lze provést v rámci konfigurace WAD hello.</span><span class="sxs-lookup"><span data-stu-id="6e9f0-192">Sending monitoring and diagnostics data tooApplication Insights (AI) can be done as part of hello WAD configuration.</span></span> <span data-ttu-id="6e9f0-193">Pokud se rozhodnete toouse AI pro analýzu událostí a vizualizace, přečtěte si [analýza události a vizualizace s Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md) tooset až jímky AI jako součást vaší "WadCfg".</span><span class="sxs-lookup"><span data-stu-id="6e9f0-193">If you decide toouse AI for event analysis and visualization, read [Event Analysis and Visualization with Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md) tooset up an AI Sink as part of your "WadCfg".</span></span>

## <a name="next-steps"></a><span data-ttu-id="6e9f0-194">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6e9f0-194">Next steps</span></span>

<span data-ttu-id="6e9f0-195">Jakmile jste správně nakonfigurovali Azure diagnostics, zobrazí se data v tabulkách úložiště z EventSource protokoly a trasování událostí pro Windows hello.</span><span class="sxs-lookup"><span data-stu-id="6e9f0-195">Once you have correctly configured Azure diagnostics, you will see data in your Storage tables from hello ETW and EventSource logs.</span></span> <span data-ttu-id="6e9f0-196">Pokud si zvolíte toouse OMS, Kibana nebo jakékoli jiné data analýzy a vizualizace platformě, která není nakonfigurovaná přímo hello šablony Resource Manageru, proveďte v hello data z těchto tabulek úložiště zda tooset až hello platforma tooread vašeho výběru.</span><span class="sxs-lookup"><span data-stu-id="6e9f0-196">If you choose toouse OMS, Kibana, or any other data analytics and visualization platform that is not directly configured in hello Resource Manager template, make sure tooset up hello platform of your choice tooread in hello data from these storage tables.</span></span> <span data-ttu-id="6e9f0-197">Díky tomuto pro OMS je relativně jednoduchá a je vysvětleno v [událostí a protokolu analýzu prostřednictvím OMS](service-fabric-diagnostics-event-analysis-oms.md).</span><span class="sxs-lookup"><span data-stu-id="6e9f0-197">Doing this for OMS is relatively trivial, and is explained in [Event and log analysis through OMS](service-fabric-diagnostics-event-analysis-oms.md).</span></span> <span data-ttu-id="6e9f0-198">Application Insights je bit ve speciálním případě v tomto smyslu, protože může být nakonfigurovaný jako součást konfigurace rozšíření diagnostiky hello, takže získáte toohello [příslušném článku](service-fabric-diagnostics-event-analysis-appinsights.md) Pokud si zvolíte toouse AI.</span><span class="sxs-lookup"><span data-stu-id="6e9f0-198">Application Insights is a bit of a special case in this sense, since it can be configured as part of hello Diagnostics extension configuration, so refer toohello [appropriate article](service-fabric-diagnostics-event-analysis-appinsights.md) if you choose toouse AI.</span></span>

>[!NOTE]
><span data-ttu-id="6e9f0-199">Aktuálně neexistuje žádný způsob, jak toofilter nebo výmazu dat hello události, které jsou odesílány toohello tabulky.</span><span class="sxs-lookup"><span data-stu-id="6e9f0-199">There is currently no way toofilter or groom hello events that are sent toohello table.</span></span> <span data-ttu-id="6e9f0-200">Pokud nemáte implementovat zpracování tooremove událostí z tabulky hello, bude hello tabulky toogrow.</span><span class="sxs-lookup"><span data-stu-id="6e9f0-200">If you don't implement a process tooremove events from hello table, hello table will continue toogrow.</span></span> <span data-ttu-id="6e9f0-201">V současné době je příkladem data výmazu dat služby spuštěné v hello [sledovací zařízení ukázka](https://github.com/Azure-Samples/service-fabric-watchdog-service), a se doporučuje, když napsat jeden pro sami taky Pokud dobrý důvody pro vás toostore protokoly nad rámec 30 nebo 90 den časový rámec.</span><span class="sxs-lookup"><span data-stu-id="6e9f0-201">Currently, there is an example of a data grooming service running in hello [Watchdog sample](https://github.com/Azure-Samples/service-fabric-watchdog-service), and it is recommended that you write one for yourself as well, unless there is a good reason for you toostore logs beyond a 30 or 90 day timeframe.</span></span>

* [<span data-ttu-id="6e9f0-202">Zjistěte, jak čítače výkonu toocollect nebo protokoly pomocí hello rozšíření diagnostiky</span><span class="sxs-lookup"><span data-stu-id="6e9f0-202">Learn how toocollect performance counters or logs by using hello Diagnostics extension</span></span>](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="6e9f0-203">Analýza události a vizualizace s Application Insights</span><span class="sxs-lookup"><span data-stu-id="6e9f0-203">Event Analysis and Visualization with Application Insights</span></span>](service-fabric-diagnostics-event-analysis-appinsights.md)
* [<span data-ttu-id="6e9f0-204">Analýza události a vizualizace s OMS</span><span class="sxs-lookup"><span data-stu-id="6e9f0-204">Event Analysis and Visualization with OMS</span></span>](service-fabric-diagnostics-event-analysis-oms.md)