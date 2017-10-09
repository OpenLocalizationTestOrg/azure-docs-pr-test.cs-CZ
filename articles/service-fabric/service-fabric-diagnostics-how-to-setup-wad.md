---
title: "aaaCollect protokolů pomocí Azure Diagnostics | Microsoft Docs"
description: "Tento článek popisuje, jak tooset si Azure Diagnostics toocollect protokolů z clusteru Service Fabric běžící v Azure."
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
ms.openlocfilehash: afbcfbe972b1847ef33bf0539b4398794b1bd56b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="collect-logs-by-using-azure-diagnostics"></a><span data-ttu-id="63272-103">Shromažďování protokolů pomocí Azure Diagnostics</span><span class="sxs-lookup"><span data-stu-id="63272-103">Collect logs by using Azure Diagnostics</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="63272-104">Windows</span><span class="sxs-lookup"><span data-stu-id="63272-104">Windows</span></span>](service-fabric-diagnostics-how-to-setup-wad.md)
> * [<span data-ttu-id="63272-105">Linux</span><span class="sxs-lookup"><span data-stu-id="63272-105">Linux</span></span>](service-fabric-diagnostics-how-to-setup-lad.md)
> 
> 

<span data-ttu-id="63272-106">Když používáte cluster služby Azure Service Fabric, je vhodné toocollect hello protokoly ze všech uzlů hello v centrálním umístění.</span><span class="sxs-lookup"><span data-stu-id="63272-106">When you're running an Azure Service Fabric cluster, it's a good idea toocollect hello logs from all hello nodes in a central location.</span></span> <span data-ttu-id="63272-107">S protokoly hello v centrálním umístění, vám pomáhají analyzovat a vyřešit problémy v clusteru nebo problémy v hello aplikací a služeb spuštěných v daném clusteru.</span><span class="sxs-lookup"><span data-stu-id="63272-107">Having hello logs in a central location helps you analyze and troubleshoot issues in your cluster, or issues in hello applications and services running in that cluster.</span></span>

<span data-ttu-id="63272-108">Jedním ze způsobů tooupload a shromáždit protokoly je toouse hello Azure Diagnostics rozšíření, které nahrávání protokolů tooAzure úložiště, Azure Application Insights nebo Azure Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="63272-108">One way tooupload and collect logs is toouse hello Azure Diagnostics extension, which uploads logs tooAzure Storage, Azure Application Insights, or Azure Event Hubs.</span></span> <span data-ttu-id="63272-109">Hello protokoly nejsou přímo v úložišti nebo ve službě Event Hubs to užitečné.</span><span class="sxs-lookup"><span data-stu-id="63272-109">hello logs are not that useful directly in storage or in Event Hubs.</span></span> <span data-ttu-id="63272-110">Ale můžete použít externího procesu tooread hello události z úložiště a umístit je v produktu, jako [analýzy protokolů](../log-analytics/log-analytics-service-fabric.md) nebo jiné řešení pro analýzu protokolu.</span><span class="sxs-lookup"><span data-stu-id="63272-110">But you can use an external process tooread hello events from storage and place them in a product such as [Log Analytics](../log-analytics/log-analytics-service-fabric.md) or another log-parsing solution.</span></span> <span data-ttu-id="63272-111">[Azure Application Insights](https://azure.microsoft.com/services/application-insights/) se dodává s komplexní protokolu vyhledávání a analýzy služby předdefinované.</span><span class="sxs-lookup"><span data-stu-id="63272-111">[Azure Application Insights](https://azure.microsoft.com/services/application-insights/) comes with a comprehensive log search and analytics service built-in.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="63272-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="63272-112">Prerequisites</span></span>
<span data-ttu-id="63272-113">Pomocí těchto nástrojů tooperform některé hello operací v tomto dokumentu:</span><span class="sxs-lookup"><span data-stu-id="63272-113">You use these tools tooperform some of hello operations in this document:</span></span>

* <span data-ttu-id="63272-114">[Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md) (týkající se tooAzure cloudové služby, ale má správné informace a příklady)</span><span class="sxs-lookup"><span data-stu-id="63272-114">[Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md) (related tooAzure Cloud Services but has good information and examples)</span></span>
* [<span data-ttu-id="63272-115">Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="63272-115">Azure Resource Manager</span></span>](../azure-resource-manager/resource-group-overview.md)
* [<span data-ttu-id="63272-116">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="63272-116">Azure PowerShell</span></span>](/powershell/azure/overview)
* [<span data-ttu-id="63272-117">Klient Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="63272-117">Azure Resource Manager client</span></span>](https://github.com/projectkudu/ARMClient)
* [<span data-ttu-id="63272-118">Šablona Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="63272-118">Azure Resource Manager template</span></span>](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="log-sources-that-you-might-want-toocollect"></a><span data-ttu-id="63272-119">Můžete chtít toocollect zdroje protokolu</span><span class="sxs-lookup"><span data-stu-id="63272-119">Log sources that you might want toocollect</span></span>
* <span data-ttu-id="63272-120">**Protokoly Service Fabric**: nevydává hello platformy toostandard trasování událostí pro Windows (ETW) a EventSource kanály.</span><span class="sxs-lookup"><span data-stu-id="63272-120">**Service Fabric logs**: Emitted from hello platform toostandard Event Tracing for Windows (ETW) and EventSource channels.</span></span> <span data-ttu-id="63272-121">Protokoly mohou být jedním z několika typů:</span><span class="sxs-lookup"><span data-stu-id="63272-121">Logs can be one of several types:</span></span>
  * <span data-ttu-id="63272-122">Provozní události: protokoly pro operace, které hello provede platformy Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="63272-122">Operational events: Logs for operations that hello Service Fabric platform performs.</span></span> <span data-ttu-id="63272-123">Mezi příklady patří vytvoření aplikací a služeb, uzel změny stavu a informace o upgradu.</span><span class="sxs-lookup"><span data-stu-id="63272-123">Examples include creation of applications and services, node state changes, and upgrade information.</span></span>
  * [<span data-ttu-id="63272-124">Programování událostí modelu Reliable Actors</span><span class="sxs-lookup"><span data-stu-id="63272-124">Reliable Actors programming model events</span></span>](service-fabric-reliable-actors-diagnostics.md)
  * [<span data-ttu-id="63272-125">Spolehlivé služby programovací model události</span><span class="sxs-lookup"><span data-stu-id="63272-125">Reliable Services programming model events</span></span>](service-fabric-reliable-services-diagnostics.md)
* <span data-ttu-id="63272-126">**Události aplikace**: události vygenerované z kódu vaší služby a zapsané pomocí hello EventSource pomocná třída součástí šablony sady Visual Studio hello.</span><span class="sxs-lookup"><span data-stu-id="63272-126">**Application events**: Events emitted from your service's code and written out by using hello EventSource helper class provided in hello Visual Studio templates.</span></span> <span data-ttu-id="63272-127">Další informace o tom, jak toowrite protokolů z vaší aplikace naleznete v tématu [monitorování a Diagnostika služby v instalačním programu místním počítači vývoj](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).</span><span class="sxs-lookup"><span data-stu-id="63272-127">For more information on how toowrite logs from your application, see [Monitor and diagnose services in a local machine development setup](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).</span></span>

## <a name="deploy-hello-diagnostics-extension"></a><span data-ttu-id="63272-128">Nasazení rozšíření diagnostiky hello</span><span class="sxs-lookup"><span data-stu-id="63272-128">Deploy hello Diagnostics extension</span></span>
<span data-ttu-id="63272-129">Hello prvním krokem při shromažďování protokolů je rozšíření diagnostiky hello toodeploy na každém hello virtuálních počítačů v clusteru Service Fabric hello.</span><span class="sxs-lookup"><span data-stu-id="63272-129">hello first step in collecting logs is toodeploy hello Diagnostics extension on each of hello VMs in hello Service Fabric cluster.</span></span> <span data-ttu-id="63272-130">Hello rozšíření diagnostiky shromažďuje protokoly na každý virtuální počítač a odesílá je toohello účet úložiště, který určíte.</span><span class="sxs-lookup"><span data-stu-id="63272-130">hello Diagnostics extension collects logs on each VM and uploads them toohello storage account that you specify.</span></span> <span data-ttu-id="63272-131">kroky Hello trochu záviset na tom, zda používáte hello portál Azure nebo Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="63272-131">hello steps vary a little based on whether you use hello Azure portal or Azure Resource Manager.</span></span> <span data-ttu-id="63272-132">Hello kroky také lišit v závislosti na tom, jestli je součástí vytváření clusteru hello nasazení, nebo je pro cluster, který již existuje.</span><span class="sxs-lookup"><span data-stu-id="63272-132">hello steps also vary based on whether hello deployment is part of cluster creation or is for a cluster that already exists.</span></span> <span data-ttu-id="63272-133">Podívejme se na hello kroky pro jednotlivé scénáře.</span><span class="sxs-lookup"><span data-stu-id="63272-133">Let's look at hello steps for each scenario.</span></span>

### <a name="deploy-hello-diagnostics-extension-as-part-of-cluster-creation-through-hello-portal"></a><span data-ttu-id="63272-134">Nasazení hello rozšíření diagnostiky jako součást vytváření clusteru prostřednictvím portálu hello</span><span class="sxs-lookup"><span data-stu-id="63272-134">Deploy hello Diagnostics extension as part of cluster creation through hello portal</span></span>
<span data-ttu-id="63272-135">toodeploy hello diagnostiky rozšíření toohello virtuálních počítačů v clusteru hello jako součást vytváření clusteru, použijte panel nastavení diagnostiky hello znázorňuje následující obrázek hello.</span><span class="sxs-lookup"><span data-stu-id="63272-135">toodeploy hello Diagnostics extension toohello VMs in hello cluster as part of cluster creation, you use hello Diagnostics settings panel shown in hello following image.</span></span> <span data-ttu-id="63272-136">tooenable shromažďování událostí Reliable Actors nebo spolehlivé služby, ověřte, zda diagnostiky příliš**na** (hello výchozí nastavení).</span><span class="sxs-lookup"><span data-stu-id="63272-136">tooenable Reliable Actors or Reliable Services event collection, ensure that Diagnostics is set too**On** (hello default setting).</span></span> <span data-ttu-id="63272-137">Po vytvoření clusteru hello, nelze změnit tato nastavení pomocí portálu hello.</span><span class="sxs-lookup"><span data-stu-id="63272-137">After you create hello cluster, you can't change these settings by using hello portal.</span></span>

![Nastavení Azure Diagnostics hello portálu pro vytvoření clusteru](./media/service-fabric-diagnostics-how-to-setup-wad/portal-cluster-creation-diagnostics-setting.png)

<span data-ttu-id="63272-139">Hello tým podpory Azure *vyžaduje* podpora protokolů toohelp vyřešte všechny žádosti o podporu, které vytvoříte.</span><span class="sxs-lookup"><span data-stu-id="63272-139">hello Azure support team *requires* support logs toohelp resolve any support requests that you create.</span></span> <span data-ttu-id="63272-140">Tyto protokoly se shromažďují v reálném čase a jsou uložené v jednom z hello účty úložiště vytvořené ve skupině prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="63272-140">These logs are collected in real time and are stored in one of hello storage accounts created in hello resource group.</span></span> <span data-ttu-id="63272-141">nastavení diagnostiky Hello nakonfigurovat událostí na úrovni aplikace.</span><span class="sxs-lookup"><span data-stu-id="63272-141">hello Diagnostics settings configure application-level events.</span></span> <span data-ttu-id="63272-142">Tyto události zahrnují [Reliable Actors](service-fabric-reliable-actors-diagnostics.md) události, [spolehlivé služby](service-fabric-reliable-services-diagnostics.md) události a některé úrovni systému toobe události Service Fabric uložené ve službě Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="63272-142">These events include [Reliable Actors](service-fabric-reliable-actors-diagnostics.md) events, [Reliable Services](service-fabric-reliable-services-diagnostics.md) events, and some system-level Service Fabric events toobe stored in Azure Storage.</span></span>

<span data-ttu-id="63272-143">Produkty, jako [Elasticsearch](https://www.elastic.co/guide/index.html) nebo vlastního procesu můžete získat hello události z účtu úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="63272-143">Products such as [Elasticsearch](https://www.elastic.co/guide/index.html) or your own process can get hello events from hello storage account.</span></span> <span data-ttu-id="63272-144">Aktuálně neexistuje žádný způsob, jak toofilter nebo výmazu dat hello události, které jsou odesílány toohello tabulky.</span><span class="sxs-lookup"><span data-stu-id="63272-144">There is currently no way toofilter or groom hello events that are sent toohello table.</span></span> <span data-ttu-id="63272-145">Pokud nemáte implementovat zpracování tooremove událostí z tabulky hello, bude hello tabulky toogrow.</span><span class="sxs-lookup"><span data-stu-id="63272-145">If you don't implement a process tooremove events from hello table, hello table will continue toogrow.</span></span>

<span data-ttu-id="63272-146">Při vytváření clusteru pomocí portálu hello, důrazně doporučujeme, abyste si stáhli hello šablony **předtím, než kliknete na tlačítko OK** toocreate hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="63272-146">When you're creating a cluster by using hello portal, we highly recommend that you download hello template **before you click OK** toocreate hello cluster.</span></span> <span data-ttu-id="63272-147">Podrobnosti najdete příliš[nastavit cluster Service Fabric pomocí šablony Azure Resource Manager](service-fabric-cluster-creation-via-arm.md).</span><span class="sxs-lookup"><span data-stu-id="63272-147">For details, refer too[Set up a Service Fabric cluster by using an Azure Resource Manager template](service-fabric-cluster-creation-via-arm.md).</span></span> <span data-ttu-id="63272-148">Změny toomake hello šablony budete potřebovat později, protože nemůže provést některé změny pomocí portálu hello.</span><span class="sxs-lookup"><span data-stu-id="63272-148">You'll need hello template toomake changes later, because you can't make some changes by using hello portal.</span></span>

<span data-ttu-id="63272-149">Šablony můžete exportovat z portálu hello pomocí hello následující kroky.</span><span class="sxs-lookup"><span data-stu-id="63272-149">You can export templates from hello portal by using hello following steps.</span></span> <span data-ttu-id="63272-150">Tyto šablony však může být obtížnější toouse, protože mohou mít hodnoty null, kterým chybí požadované informace.</span><span class="sxs-lookup"><span data-stu-id="63272-150">However, these templates can be more difficult toouse because they might have null values that are missing required information.</span></span>

1. <span data-ttu-id="63272-151">Otevřete vaší skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="63272-151">Open your resource group.</span></span>
2. <span data-ttu-id="63272-152">Vyberte **nastavení** panel nastavení toodisplay hello.</span><span class="sxs-lookup"><span data-stu-id="63272-152">Select **Settings** toodisplay hello settings panel.</span></span>
3. <span data-ttu-id="63272-153">Vyberte **nasazení** panelu Historie nasazení toodisplay hello.</span><span class="sxs-lookup"><span data-stu-id="63272-153">Select **Deployments** toodisplay hello deployment history panel.</span></span>
4. <span data-ttu-id="63272-154">Vyberte podrobnosti o nasazení toodisplay hello hello nasazení.</span><span class="sxs-lookup"><span data-stu-id="63272-154">Select a deployment toodisplay hello details of hello deployment.</span></span>
5. <span data-ttu-id="63272-155">Vyberte **exportovat šablonu** toodisplay hello šablony panelu.</span><span class="sxs-lookup"><span data-stu-id="63272-155">Select **Export Template** toodisplay hello template panel.</span></span>
6. <span data-ttu-id="63272-156">Vyberte **uložit toofile** tooexport soubor .zip, který obsahuje šablony hello, parametr a soubory prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="63272-156">Select **Save toofile** tooexport a .zip file that contains hello template, parameter, and PowerShell files.</span></span>

<span data-ttu-id="63272-157">Po exportu hello soubory, musíte toomake změna.</span><span class="sxs-lookup"><span data-stu-id="63272-157">After you export hello files, you need toomake a modification.</span></span> <span data-ttu-id="63272-158">Upravte souboru parameters.JSON tímto kódem hello a odeberte hello **adminPassword** elementu.</span><span class="sxs-lookup"><span data-stu-id="63272-158">Edit hello parameters.json file and remove hello **adminPassword** element.</span></span> <span data-ttu-id="63272-159">Při spuštění skriptu nasazení hello způsobí výzva k zadání hesla hello.</span><span class="sxs-lookup"><span data-stu-id="63272-159">This will cause a prompt for hello password when hello deployment script is run.</span></span> <span data-ttu-id="63272-160">Když spouštíte skript nasazení hello, můžete mít hodnoty null parametru toofix.</span><span class="sxs-lookup"><span data-stu-id="63272-160">When you're running hello deployment script, you might have toofix null parameter values.</span></span>

<span data-ttu-id="63272-161">toouse hello stáhnout šablony tooupdate konfigurace:</span><span class="sxs-lookup"><span data-stu-id="63272-161">toouse hello downloaded template tooupdate a configuration:</span></span>

1. <span data-ttu-id="63272-162">Rozbalte složku tooa hello obsah v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="63272-162">Extract hello contents tooa folder on your local computer.</span></span>
2. <span data-ttu-id="63272-163">Upravte hello obsah tooreflect hello novou konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="63272-163">Modify hello contents tooreflect hello new configuration.</span></span>
3. <span data-ttu-id="63272-164">Spusťte PowerShell a změňte toohello složky, které jste extrahovali obsah hello.</span><span class="sxs-lookup"><span data-stu-id="63272-164">Start PowerShell and change toohello folder where you extracted hello content.</span></span>
4. <span data-ttu-id="63272-165">Spustit **deploy.ps1** a vyplňte hello ID odběru, název skupiny prostředků hello (hello použijte stejný název tooupdate hello konfigurace) a název jedinečný nasazení.</span><span class="sxs-lookup"><span data-stu-id="63272-165">Run **deploy.ps1** and fill in hello subscription ID, hello resource group name (use hello same name tooupdate hello configuration), and a unique deployment name.</span></span>

### <a name="deploy-hello-diagnostics-extension-as-part-of-cluster-creation-by-using-azure-resource-manager"></a><span data-ttu-id="63272-166">Nasazení hello rozšíření diagnostiky jako součást vytváření clusteru pomocí Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="63272-166">Deploy hello Diagnostics extension as part of cluster creation by using Azure Resource Manager</span></span>
<span data-ttu-id="63272-167">toocreate cluster pomocí Resource Manageru, potřebujete tooadd hello diagnostiky konfigurace JSON toohello úplné clusteru šablony Resource Manageru před vytvořením clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="63272-167">toocreate a cluster by using Resource Manager, you need tooadd hello Diagnostics configuration JSON toohello full cluster Resource Manager template before you create hello cluster.</span></span> <span data-ttu-id="63272-168">Poskytujeme šablony Resource Manageru ukázkový pět VM cluster s konfigurací diagnostiky přidat tooit jako součást naše ukázky šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="63272-168">We provide a sample five-VM cluster Resource Manager template with Diagnostics configuration added tooit as part of our Resource Manager template samples.</span></span> <span data-ttu-id="63272-169">Zobrazí se v tomto umístění v galerii Azure Samples hello: [pěti uzly clusteru s ukázkou šablony správce prostředků diagnostiky](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype).</span><span class="sxs-lookup"><span data-stu-id="63272-169">You can see it at this location in hello Azure Samples gallery: [Five-node cluster with Diagnostics Resource Manager template sample](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype).</span></span>

<span data-ttu-id="63272-170">nastavení diagnostiky hello toosee v hello šablony Resource Manageru, otevřete hello soubor azuredeploy.json a vyhledejte **IaaSDiagnostics**.</span><span class="sxs-lookup"><span data-stu-id="63272-170">toosee hello Diagnostics setting in hello Resource Manager template, open hello azuredeploy.json file and search for **IaaSDiagnostics**.</span></span> <span data-ttu-id="63272-171">toocreate cluster pomocí této šablony, vyberte hello **nasazení tooAzure** tlačítko k dispozici na předchozí odkaz hello.</span><span class="sxs-lookup"><span data-stu-id="63272-171">toocreate a cluster by using this template, select hello **Deploy tooAzure** button available at hello previous link.</span></span>

<span data-ttu-id="63272-172">Alternativně můžete stáhnout ukázkový hello Resource Manager, zkontrolujte změny tooit a vytvořte cluster se změněné šablony hello pomocí hello `New-AzureRmResourceGroupDeployment` příkazu v okně prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="63272-172">Alternatively, you can download hello Resource Manager sample, make changes tooit, and create a cluster with hello modified template by using hello `New-AzureRmResourceGroupDeployment` command in an Azure PowerShell window.</span></span> <span data-ttu-id="63272-173">Viz následující kód pro hello parametry, které se předávají v příkazu toohello hello.</span><span class="sxs-lookup"><span data-stu-id="63272-173">See hello following code for hello parameters that you pass in toohello command.</span></span> <span data-ttu-id="63272-174">Podrobné informace o tom, jak toodeploy prostředek skupiny pomocí prostředí PowerShell, najdete v článku hello [nasazení skupiny prostředků pomocí šablony Azure Resource Manager hello](../azure-resource-manager/resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="63272-174">For detailed information on how toodeploy a resource group by using PowerShell, see hello article [Deploy a resource group with hello Azure Resource Manager template](../azure-resource-manager/resource-group-template-deploy.md).</span></span>

```powershell

New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -Name $deploymentName -TemplateFile $pathToARMConfigJsonFile -TemplateParameterFile $pathToParameterFile –Verbose
```

### <a name="deploy-hello-diagnostics-extension-tooan-existing-cluster"></a><span data-ttu-id="63272-175">Nasazení hello diagnostiky rozšíření tooan stávajícího clusteru</span><span class="sxs-lookup"><span data-stu-id="63272-175">Deploy hello Diagnostics extension tooan existing cluster</span></span>
<span data-ttu-id="63272-176">Pokud máte existující cluster, který nemá diagnostiky nasazené, nebo pokud chcete toomodify existující konfigurace, můžete přidat nebo aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="63272-176">If you have an existing cluster that doesn't have Diagnostics deployed, or if you want toomodify an existing configuration, you can add or update it.</span></span> <span data-ttu-id="63272-177">Upravit šablonu hello Resource Manager, který je použité toocreate hello existující cluster nebo stáhnout hello šablonu z portálu hello, jak je popsáno výše.</span><span class="sxs-lookup"><span data-stu-id="63272-177">Modify hello Resource Manager template that's used toocreate hello existing cluster or download hello template from hello portal as described earlier.</span></span> <span data-ttu-id="63272-178">Upravte soubor template.json hello provedením hello následující úlohy.</span><span class="sxs-lookup"><span data-stu-id="63272-178">Modify hello template.json file by performing hello following tasks.</span></span>

<span data-ttu-id="63272-179">Přidáte novou šablonu toohello prostředků úložiště přidáním toohello oddílu prostředků.</span><span class="sxs-lookup"><span data-stu-id="63272-179">Add a new storage resource toohello template by adding toohello resources section.</span></span>

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

 <span data-ttu-id="63272-180">Dál přidejte toohello parametry části bezprostředně za hello Definice účet úložiště, mezi `supportLogStorageAccountName` a `vmNodeType0Name`.</span><span class="sxs-lookup"><span data-stu-id="63272-180">Next, add toohello parameters section just after hello storage account definitions, between `supportLogStorageAccountName` and `vmNodeType0Name`.</span></span> <span data-ttu-id="63272-181">Nahraďte zástupný text hello *sem zadejte název účtu úložiště* hello název účtu úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="63272-181">Replace hello placeholder text *storage account name goes here* with hello name of hello storage account.</span></span>

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
<span data-ttu-id="63272-182">Aktualizujte hello `VirtualMachineProfile` části souboru template.json hello přidáním hello následující kód do pole rozšíření hello.</span><span class="sxs-lookup"><span data-stu-id="63272-182">Then, update hello `VirtualMachineProfile` section of hello template.json file by adding hello following code within hello extensions array.</span></span> <span data-ttu-id="63272-183">Být jisti tooadd čárka na začátku hello nebo hello end, v závislosti na tom, kde je vložen.</span><span class="sxs-lookup"><span data-stu-id="63272-183">Be sure tooadd a comma at hello beginning or hello end, depending on where it's inserted.</span></span>

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

<span data-ttu-id="63272-184">Po úpravě souboru template.json hello, jak je popsáno, znovu publikujte šablony Resource Manageru hello.</span><span class="sxs-lookup"><span data-stu-id="63272-184">After you modify hello template.json file as described, republish hello Resource Manager template.</span></span> <span data-ttu-id="63272-185">Pokud byl exportován hello šablony, znovu publikuje spouštění souboru deploy.ps1 hello uzamkl hello šablony.</span><span class="sxs-lookup"><span data-stu-id="63272-185">If hello template was exported, running hello deploy.ps1 file republishes hello template.</span></span> <span data-ttu-id="63272-186">Poté, co nasadíte, ujistěte se, že **ProvisioningState** je **úspěšné**.</span><span class="sxs-lookup"><span data-stu-id="63272-186">After you deploy, ensure that **ProvisioningState** is **Succeeded**.</span></span>

## <a name="update-diagnostics-toocollect-health-and-load-events"></a><span data-ttu-id="63272-187">Aktualizovat stav a zatížení události toocollect diagnostiky</span><span class="sxs-lookup"><span data-stu-id="63272-187">Update diagnostics toocollect health and load events</span></span>

<span data-ttu-id="63272-188">Od verze hello 5.4 Service Fabric, stavu a zatížení metriky události jsou k dispozici pro kolekci.</span><span class="sxs-lookup"><span data-stu-id="63272-188">Starting with hello 5.4 release of Service Fabric, health and load metric events are available for collection.</span></span> <span data-ttu-id="63272-189">Tyto události podle událostí generovaných hello systému nebo v kódu pomocí hello stavu nebo zatížení rozhraní API pro vytváření sestav, jako [ReportPartitionHealth](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportpartitionhealth.aspx) nebo [ReportLoad](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportload.aspx).</span><span class="sxs-lookup"><span data-stu-id="63272-189">These events reflect events generated by hello system or your code by using hello health or load reporting APIs such as [ReportPartitionHealth](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportpartitionhealth.aspx) or [ReportLoad](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportload.aspx).</span></span> <span data-ttu-id="63272-190">To umožňuje pro agregaci a zobrazení stavu systému v čase a pro výstrahy na základě stavu nebo zatížení událostí.</span><span class="sxs-lookup"><span data-stu-id="63272-190">This allows for aggregating and viewing system health over time and for alerting based on health or load events.</span></span> <span data-ttu-id="63272-191">Přidat tyto události v sadě Visual Studio prohlížeč diagnostických událostí tooview "Microsoft-ServiceFabric:4:0x4000000000000008" toohello seznamu zprostředkovatelů trasování událostí pro Windows.</span><span class="sxs-lookup"><span data-stu-id="63272-191">tooview these events in Visual Studio's Diagnostic Event Viewer add "Microsoft-ServiceFabric:4:0x4000000000000008" toohello list of ETW providers.</span></span>

<span data-ttu-id="63272-192">události hello toocollect, upravte tooinclude šablony správce prostředků hello</span><span class="sxs-lookup"><span data-stu-id="63272-192">toocollect hello events, modify hello resource manager template tooinclude</span></span>

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

## <a name="update-diagnostics-toocollect-and-upload-logs-from-new-eventsource-channels"></a><span data-ttu-id="63272-193">Aktualizujte toocollect diagnostiky a odeslat protokoly z nové EventSource kanály</span><span class="sxs-lookup"><span data-stu-id="63272-193">Update Diagnostics toocollect and upload logs from new EventSource channels</span></span>
<span data-ttu-id="63272-194">tooupdate diagnostiky toocollect protokoly z nové EventSource kanály, které představují novou aplikaci, která jste o toodeploy, provádět hello stejné kroky jako hello [předchozí části](#deploywadarm) pro nastavení diagnostiky pro existující cluster.</span><span class="sxs-lookup"><span data-stu-id="63272-194">tooupdate Diagnostics toocollect logs from new EventSource channels that represent a new application that you're about toodeploy, perform hello same steps as in hello [previous section](#deploywadarm) for setup of Diagnostics for an existing cluster.</span></span>

<span data-ttu-id="63272-195">Aktualizovat hello `EtwEventSourceProviderConfiguration` kapitoly položky tooadd souboru template.json hello hello nové EventSource kanály než použijete hello konfigurace aktualizace pomocí hello `New-AzureRmResourceGroupDeployment` příkaz prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="63272-195">Update hello `EtwEventSourceProviderConfiguration` section in hello template.json file tooadd entries for hello new EventSource channels before you apply hello configuration update by using hello `New-AzureRmResourceGroupDeployment` PowerShell command.</span></span> <span data-ttu-id="63272-196">Název Hello hello zdroje událostí je definován jako součást kódu v souboru generovaný Visual Studio ServiceEventSource.cs hello.</span><span class="sxs-lookup"><span data-stu-id="63272-196">hello name of hello event source is defined as part of your code in hello Visual Studio-generated ServiceEventSource.cs file.</span></span>

<span data-ttu-id="63272-197">Například pokud váš zdroj událostí s názvem Moje Eventsource, přidejte následující kód tooplace hello události z Moje Eventsource do tabulky s názvem MyDestinationTableName hello.</span><span class="sxs-lookup"><span data-stu-id="63272-197">For example, if your event source is named My-Eventsource, add hello following code tooplace hello events from My-Eventsource into a table named MyDestinationTableName.</span></span>

```json
        {
            "provider": "My-Eventsource",
            "scheduledTransferPeriod": "PT5M",
            "DefaultEvents": {
            "eventDestination": "MyDestinationTableName"
            }
        }
```

<span data-ttu-id="63272-198">čítače výkonu toocollect nebo protokoly událostí, změna šablony Resource Manageru hello pomocí hello příklady [vytvoření virtuálního počítače s Windows pomocí monitorování a Diagnostika pomocí šablony Azure Resource Manager](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="63272-198">toocollect performance counters or event logs, modify hello Resource Manager template by using hello examples provided in [Create a Windows virtual machine with monitoring and diagnostics by using an Azure Resource Manager template](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="63272-199">Potom se znovu publikujte šablony Resource Manageru hello.</span><span class="sxs-lookup"><span data-stu-id="63272-199">Then, republish hello Resource Manager template.</span></span>

## <a name="next-steps"></a><span data-ttu-id="63272-200">Další kroky</span><span class="sxs-lookup"><span data-stu-id="63272-200">Next steps</span></span>
<span data-ttu-id="63272-201">toounderstand podrobněji událostech, které je vhodné vyhledat při řešení potíží, najdete v části hello diagnostických událostí vygenerované pro [Reliable Actors](service-fabric-reliable-actors-diagnostics.md) a [spolehlivé služby](service-fabric-reliable-services-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="63272-201">toounderstand in more detail what events you should look for while troubleshooting issues, see hello diagnostic events emitted for [Reliable Actors](service-fabric-reliable-actors-diagnostics.md) and [Reliable Services](service-fabric-reliable-services-diagnostics.md).</span></span>

## <a name="related-articles"></a><span data-ttu-id="63272-202">Související články</span><span class="sxs-lookup"><span data-stu-id="63272-202">Related articles</span></span>
* [<span data-ttu-id="63272-203">Zjistěte, jak čítače výkonu toocollect nebo protokoly pomocí hello rozšíření diagnostiky</span><span class="sxs-lookup"><span data-stu-id="63272-203">Learn how toocollect performance counters or logs by using hello Diagnostics extension</span></span>](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="63272-204">Řešení Service Fabric v analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="63272-204">Service Fabric solution in Log Analytics</span></span>](../log-analytics/log-analytics-service-fabric.md)

