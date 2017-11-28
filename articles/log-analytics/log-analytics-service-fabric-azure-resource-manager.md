---
title: "aaaAssess aplikace Service Fabric pomocí analýzy protokolů hello portálu Azure | Microsoft Docs"
description: "V analýzy protokolů pomocí hello Azure portálu tooassess hello riziko a stavu aplikace Service Fabric, můžete použít Service Fabric řešení hello micro-services, uzly a clustery."
services: log-analytics
documentationcenter: 
author: niniikhena
manager: jochan
editor: 
ms.assetid: 9c91aacb-c48e-466c-b792-261f25940c0c
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: nini
ms.openlocfilehash: 891c7f6e5ed511ac18599bdc280ab3dc09700fbe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="assess-service-fabric-applications-and-micro-services-with-hello-azure-portal"></a><span data-ttu-id="41960-103">Hodnocení aplikace Service Fabric a micro-services s hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="41960-103">Assess Service Fabric applications and micro-services with hello Azure portal</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="41960-104">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="41960-104">Resource Manager</span></span>](log-analytics-service-fabric-azure-resource-manager.md)
> * [<span data-ttu-id="41960-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="41960-105">PowerShell</span></span>](log-analytics-service-fabric.md)
>
>

![Symbol Service Fabric](./media/log-analytics-service-fabric/service-fabric-assessment-symbol.png)

<span data-ttu-id="41960-107">Tento článek popisuje, jak toouse hello řešení Service Fabric v toohelp Log Analytics identifikovat a řešit problémy mezi cluster Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="41960-107">This article describes how toouse hello Service Fabric solution in Log Analytics toohelp identify and troubleshoot issues across your Service Fabric cluster.</span></span>

<span data-ttu-id="41960-108">Hello Service Fabric řešení používá Azure Diagnostics data z virtuálních počítačů služby infrastruktury, shromažďováním těchto dat z Azure WAD tabulek.</span><span class="sxs-lookup"><span data-stu-id="41960-108">hello Service Fabric solution uses Azure Diagnostics data from your Service Fabric VMs, by collecting this data from your Azure WAD tables.</span></span> <span data-ttu-id="41960-109">Analýzy protokolů potom načte události framework Service Fabric, včetně **spolehlivé události služby**, **události objektu Actor**, **provozní události**, a **vlastní Události trasování událostí**.</span><span class="sxs-lookup"><span data-stu-id="41960-109">Log Analytics then reads Service Fabric framework events, including **Reliable Service Events**, **Actor Events**, **Operational Events**, and **Custom ETW events**.</span></span> <span data-ttu-id="41960-110">S hello řídicí panel řešení jsou možné tooview významné problémy a relevantní události ve vašem prostředí Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="41960-110">With hello solution dashboard, you are able tooview notable issues and relevant events in your Service Fabric environment.</span></span>

<span data-ttu-id="41960-111">tooget začít s hello řešení, je nutné tooconnect pracovní prostor analýzy protokolů tooa clusteru Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="41960-111">tooget started with hello solution, you need tooconnect your Service Fabric cluster tooa Log Analytics workspace.</span></span> <span data-ttu-id="41960-112">Tady jsou tři tooconsider scénáře:</span><span class="sxs-lookup"><span data-stu-id="41960-112">Here are three scenarios tooconsider:</span></span>

1. <span data-ttu-id="41960-113">Pokud jste nenasadili cluster Service Fabric, použijte kroky hello v ***nasadit pracovní prostor analýzy protokolů Cluster Service Fabric připojené tooa*** toodeploy do nového clusteru a když jste nakonfigurovali tooreport tooLog Analytics.</span><span class="sxs-lookup"><span data-stu-id="41960-113">If you have not deployed your Service Fabric cluster, use hello steps in ***Deploy a Service Fabric Cluster connected tooa Log Analytics workspace*** toodeploy a new cluster and have it configured tooreport tooLog Analytics.</span></span>
2. <span data-ttu-id="41960-114">Pokud potřebujete toocollect čítače výkonu z vaší hostitelů toouse jiných OMS řešení, jako je například zabezpečení na váš Cluster Service Fabric, postupujte podle kroků hello v ***nasadit Cluster Service Fabric připojené tooa pracovní prostor analýzy protokolů pomocí rozšíření virtuálního počítače nainstalovat.***</span><span class="sxs-lookup"><span data-stu-id="41960-114">If you need toocollect performance counters from your hosts toouse other OMS solutions such as Security on your Service Fabric Cluster, follow hello steps in ***Deploy a Service Fabric Cluster connected tooa Log Analytics workspace with VM Extension installed.***</span></span>
3. <span data-ttu-id="41960-115">Pokud jste už jste nasadili cluster Service Fabric a chcete tooconnect ho tooLog analýzy, postupujte podle kroků hello v ***přidání existující účet úložiště tooLog Analytics.***</span><span class="sxs-lookup"><span data-stu-id="41960-115">If you have already deployed your Service Fabric cluster and want tooconnect it tooLog Analytics, follow hello steps in ***Adding an existing storage account tooLog Analytics.***</span></span>

## <a name="deploy-a-service-fabric-cluster-connected-tooa-log-analytics-workspace"></a><span data-ttu-id="41960-116">Nasazení pracovní prostor analýzy protokolů tooa Cluster Service Fabric připojen.</span><span class="sxs-lookup"><span data-stu-id="41960-116">Deploy a Service Fabric Cluster connected tooa Log Analytics workspace.</span></span>
<span data-ttu-id="41960-117">Tato šablona hello následující:</span><span class="sxs-lookup"><span data-stu-id="41960-117">This template does hello following:</span></span>

1. <span data-ttu-id="41960-118">Pracovní prostor analýzy protokolů služby Azure Service Fabric clusteru už připojená tooa nasadí.</span><span class="sxs-lookup"><span data-stu-id="41960-118">Deploys an Azure Service Fabric cluster already connected tooa Log Analytics workspace.</span></span> <span data-ttu-id="41960-119">Možnost toocreate hello máte při nasazování šablony hello nebo vstupní hello název již existující pracovní prostor analýzy protokolů nový pracovní prostor.</span><span class="sxs-lookup"><span data-stu-id="41960-119">You have hello option toocreate a new workspace while deploying hello template, or input hello name of an already existing Log Analytics workspace.</span></span>
2. <span data-ttu-id="41960-120">Přidá hello diagnostické úložiště účet toohello pracovní prostor analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="41960-120">Adds hello diagnostic storage account toohello Log Analytics workspace.</span></span>
3. <span data-ttu-id="41960-121">Umožňuje řešení hello Service Fabric v pracovním prostoru analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="41960-121">Enables hello Service Fabric solution in your Log Analytics workspace.</span></span>

<span data-ttu-id="41960-122">[![Nasazení tooAzure](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fazure%2Fazure-quickstart-templates%2Fmaster%2Fservice-fabric-oms%2F%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="41960-122">[![Deploy tooAzure](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fazure%2Fazure-quickstart-templates%2Fmaster%2Fservice-fabric-oms%2F%2Fazuredeploy.json)</span></span>

<span data-ttu-id="41960-123">Jakmile vyberete hello nasazení výše uvedené tlačítko, hello Azure otevře portál s parametry, můžete tooedit.</span><span class="sxs-lookup"><span data-stu-id="41960-123">Once you select hello deploy button above, hello Azure portal opens with parameters for you tooedit.</span></span> <span data-ttu-id="41960-124">Být jisti toocreate novou skupinu prostředků, pokud je zadat nový název pracovního prostoru analýzy protokolů:</span><span class="sxs-lookup"><span data-stu-id="41960-124">Be sure toocreate a new resource group if you input a new Log Analytics workspace name:</span></span>

![Service Fabric](./media/log-analytics-service-fabric/2.png)

![Service Fabric](./media/log-analytics-service-fabric/3.png)

<span data-ttu-id="41960-127">Přijměte právní podmínky hello a klikněte na **vytvořit** toostart hello nasazení.</span><span class="sxs-lookup"><span data-stu-id="41960-127">Accept hello legal terms and click **Create** toostart hello deployment.</span></span> <span data-ttu-id="41960-128">Po dokončení nasazení hello by měl vidět hello nový pracovní prostor a vytvoření clusteru a hello WADServiceFabric * událostí, WADWindowsEventLogs a WADETWEvent tabulek, které jsou přidány:</span><span class="sxs-lookup"><span data-stu-id="41960-128">Once hello deployment is completed, you should see hello new workspace and cluster created, and hello WADServiceFabric*Event, WADWindowsEventLogs and WADETWEvent tables added:</span></span>

![Service Fabric](./media/log-analytics-service-fabric/4.png)

## <a name="deploy-a-service-fabric-cluster-connected-tooa-log-analytics-workspace-with-vm-extension-installed"></a><span data-ttu-id="41960-130">Pracovní prostor analýzy protokolů Cluster Service Fabric připojené tooa nasaďte s nainstalovat rozšíření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="41960-130">Deploy a Service Fabric Cluster connected tooa Log Analytics workspace with VM Extension installed.</span></span>

<span data-ttu-id="41960-131">Tato šablona hello následující:</span><span class="sxs-lookup"><span data-stu-id="41960-131">This template does hello following:</span></span>

1. <span data-ttu-id="41960-132">Pracovní prostor analýzy protokolů služby Azure Service Fabric clusteru už připojená tooa nasadí.</span><span class="sxs-lookup"><span data-stu-id="41960-132">Deploys an Azure Service Fabric cluster already connected tooa Log Analytics workspace.</span></span> <span data-ttu-id="41960-133">Můžete vytvořit nový pracovní prostor nebo použijte existující.</span><span class="sxs-lookup"><span data-stu-id="41960-133">You can create a new workspace or use an existing one.</span></span>
2. <span data-ttu-id="41960-134">Přidá hello pracovní prostor analýzy protokolů toohello diagnostické úložiště účtů.</span><span class="sxs-lookup"><span data-stu-id="41960-134">Adds hello diagnostic storage accounts toohello Log Analytics workspace.</span></span>
3. <span data-ttu-id="41960-135">Umožňuje řešení hello Service Fabric v pracovní prostor analýzy protokolů hello.</span><span class="sxs-lookup"><span data-stu-id="41960-135">Enables hello Service Fabric solution in hello Log Analytics workspace.</span></span>
4. <span data-ttu-id="41960-136">Nainstaluje rozšíření agenta MMA hello v jednotlivých sad v clusteru Service Fabric škálování virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="41960-136">Installs hello MMA agent extension in each virtual machine scale set in your Service Fabric cluster.</span></span> <span data-ttu-id="41960-137">Instalaci agenta MMA hello jsou metriky výkonu možné tooview o uzly.</span><span class="sxs-lookup"><span data-stu-id="41960-137">With hello MMA agent installed, you are able tooview performance metrics about your nodes.</span></span>

<span data-ttu-id="41960-138">[![Nasazení tooAzure](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fazure%2Fazure-quickstart-templates%2Fmaster%2Fservice-fabric-vmss-oms%2F%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="41960-138">[![Deploy tooAzure](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fazure%2Fazure-quickstart-templates%2Fmaster%2Fservice-fabric-vmss-oms%2F%2Fazuredeploy.json)</span></span>

<span data-ttu-id="41960-139">Následující text hello stejný postup uvedený výš vstupní hello potřebné parametry a ji nasazení.</span><span class="sxs-lookup"><span data-stu-id="41960-139">Following hello same steps above, input hello necessary parameters, and kick off a deployment.</span></span> <span data-ttu-id="41960-140">Měli byste vidět znovu WAD tabulek všechny vytvořené, clusteru a nový pracovní prostor hello:</span><span class="sxs-lookup"><span data-stu-id="41960-140">Once again you should see hello new workspace, cluster and WAD tables all created:</span></span>

![Service Fabric](./media/log-analytics-service-fabric/5.png)

### <a name="viewing-performance-data"></a><span data-ttu-id="41960-142">Zobrazení dat výkonu</span><span class="sxs-lookup"><span data-stu-id="41960-142">Viewing Performance Data</span></span>

<span data-ttu-id="41960-143">tooview dat výkonu z uzlů:</span><span class="sxs-lookup"><span data-stu-id="41960-143">tooview Perf Data from your nodes:</span></span>


[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

- <span data-ttu-id="41960-144">Spusťte pracovní prostor analýzy protokolů hello z hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="41960-144">Launch hello Log Analytics workspace from hello Azure portal.</span></span>
  <span data-ttu-id="41960-145">![Service Fabric](./media/log-analytics-service-fabric/6.png)</span><span class="sxs-lookup"><span data-stu-id="41960-145">![Service Fabric](./media/log-analytics-service-fabric/6.png)</span></span>
- <span data-ttu-id="41960-146">Přejděte v levém podokně hello tooSettings a vyberte Data >> čítačů výkonu systému Windows >> "hello přidat vybrané čítače výkonu": ![Service Fabric](./media/log-analytics-service-fabric/7.png)</span><span class="sxs-lookup"><span data-stu-id="41960-146">Go tooSettings on hello left pane, and select Data >> Windows Performance Counters >> "Add hello selected performance counters": ![Service Fabric](./media/log-analytics-service-fabric/7.png)</span></span>
- <span data-ttu-id="41960-147">V hledání protokolů použijte následující dotazy toodelve do klíčových metrik týkajících se uzly hello:</span><span class="sxs-lookup"><span data-stu-id="41960-147">In Log Search, use hello following queries toodelve into key metrics about your nodes:</span></span>

    <span data-ttu-id="41960-148">a.</span><span class="sxs-lookup"><span data-stu-id="41960-148">a.</span></span> <span data-ttu-id="41960-149">Porovnání hello průměrné využití procesoru pro všechny uzly v hello poslední hodinu toosee uzlů, které jsou problémy s a v jaké časovém intervalu uzlu měl Špička:</span><span class="sxs-lookup"><span data-stu-id="41960-149">Compare hello average CPU Utilization across all your nodes in hello last one hour toosee which nodes are having issues and at what time interval a node had a spike:</span></span>

    ```
    Type=Perf ObjectName=Processor CounterName="% Processor Time"|measure avg(CounterValue) by Computer Interval 1HOUR.
    ```

    ![Service Fabric](./media/log-analytics-service-fabric/10.png)

    <span data-ttu-id="41960-151">b.</span><span class="sxs-lookup"><span data-stu-id="41960-151">b.</span></span> <span data-ttu-id="41960-152">Zobrazit podobné spojnicových grafů pro dostupná paměť na každý uzel k tomuto dotazu:</span><span class="sxs-lookup"><span data-stu-id="41960-152">View similar line charts for available memory on each node with this query:</span></span>

    ```
    Type=Perf ObjectName=Memory CounterName="Available MBytes Memory" | measure avg(CounterValue) by Computer Interval 1HOUR.
    ```

    <span data-ttu-id="41960-153">tooview seznam všech uzlech, zobrazující hello přesný průměrnou hodnotu pro počet MB k dispozici pro každý uzel, použijte tento dotaz:</span><span class="sxs-lookup"><span data-stu-id="41960-153">tooview a listing of all your nodes, showing hello exact average value for Available MBytes for each node, use this query:</span></span>

    ```
    Type=Perf (ObjectName=Memory) (CounterName="Available MBytes") | measure avg(CounterValue) by Computer
    ```

    ![Service Fabric](./media/log-analytics-service-fabric/11.png)

    <span data-ttu-id="41960-155">c.</span><span class="sxs-lookup"><span data-stu-id="41960-155">c.</span></span> <span data-ttu-id="41960-156">V případě hello má toodrill dolů na konkrétním uzlu tak, že prověří hello hodinové průměr minimální, maximální a 75 percentilu využití procesoru, budete moct toodo tím, že pomocí tohoto dotazu (nahraďte pole počítače):</span><span class="sxs-lookup"><span data-stu-id="41960-156">In hello case that you want toodrill down into a specific node by examining hello hourly average, minimum, maximum and 75-percentile CPU usage, you're able toodo this by using this query (replace Computer field):</span></span>

    ```
    Type=Perf CounterName="% Processor Time" InstanceName=_Total Computer="BaconDC01.BaconLand.com"| measure min(CounterValue), avg(CounterValue), percentile75(CounterValue), max(CounterValue) by Computer Interval 1HOUR
    ```

    ![Service Fabric](./media/log-analytics-service-fabric/12.png)

<span data-ttu-id="41960-158">Další informace o metriky výkonu v analýzy protokolů hello [Operations Management Suite blog](https://blogs.technet.microsoft.com/msoms/tag/metrics/).</span><span class="sxs-lookup"><span data-stu-id="41960-158">Read more information about performance metrics in Log Analytics at hello [Operations Management Suite blog](https://blogs.technet.microsoft.com/msoms/tag/metrics/).</span></span>


## <a name="adding-an-existing-storage-account-toolog-analytics"></a><span data-ttu-id="41960-159">Přidání existující účet úložiště tooLog Analytics</span><span class="sxs-lookup"><span data-stu-id="41960-159">Adding an existing storage account tooLog Analytics</span></span>

<span data-ttu-id="41960-160">Tato šablona jednoduše přidá vaše stávající úložiště účtů tooa nový nebo existující pracovní prostor analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="41960-160">This template simply adds your existing storage accounts tooa new or existing Log Analytics workspace.</span></span>

<span data-ttu-id="41960-161">[![Nasazení tooAzure](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Foms-existing-storage-account%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="41960-161">[![Deploy tooAzure](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Foms-existing-storage-account%2Fazuredeploy.json)</span></span>

> [!NOTE]
> <span data-ttu-id="41960-162">Výběr skupiny prostředků, pokud pracujete s již existující pracovní prostor analýzy protokolů, vyberte "Použití existující" a vyhledejte hello skupinu prostředků obsahující pracovní prostor analýzy protokolů hello.</span><span class="sxs-lookup"><span data-stu-id="41960-162">In selecting a Resource Group, if you're working with an already existing Log Analytics workspace, select "Use Existing" and search for hello resource group containing hello Log Analytics workspace.</span></span> <span data-ttu-id="41960-163">Vytvořit nový jeden Pokud jinak.</span><span class="sxs-lookup"><span data-stu-id="41960-163">Create a new one if otherwise.</span></span>
> <span data-ttu-id="41960-164">![Service Fabric](./media/log-analytics-service-fabric/8.png)</span><span class="sxs-lookup"><span data-stu-id="41960-164">![Service Fabric](./media/log-analytics-service-fabric/8.png)</span></span>
>
>

<span data-ttu-id="41960-165">Po nasazení této šablony, bude možné toosee hello úložiště účet připojené tooyour pracovní prostor analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="41960-165">After this template has been deployed, you will be able toosee hello storage account connected tooyour Log Analytics workspace.</span></span> <span data-ttu-id="41960-166">V tomto případě přidané jeden další úložiště účet toohello Exchange prostoru nově vytvořené výše.</span><span class="sxs-lookup"><span data-stu-id="41960-166">In this instance, I added one more storage account toohello Exchange workspace I created above.</span></span>
<span data-ttu-id="41960-167">![Service Fabric](./media/log-analytics-service-fabric/9.png)</span><span class="sxs-lookup"><span data-stu-id="41960-167">![Service Fabric](./media/log-analytics-service-fabric/9.png)</span></span>

## <a name="view-service-fabric-events"></a><span data-ttu-id="41960-168">Zobrazit události Service Fabric</span><span class="sxs-lookup"><span data-stu-id="41960-168">View Service Fabric events</span></span>

<span data-ttu-id="41960-169">Po dokončení nasazení hello a povolil hello řešení Service Fabric v pracovním prostoru, vybrat hello **Service Fabric** dlaždici na řídicím panelu hello analýzy protokolů portálu toolaunch hello Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="41960-169">Once hello deployments are completed and hello Service Fabric solution has been enabled in your workspace, select hello **Service Fabric** tile in hello Log Analytics portal toolaunch hello Service Fabric dashboard.</span></span> <span data-ttu-id="41960-170">řídicí panel Hello obsahuje sloupce hello hello následující tabulka.</span><span class="sxs-lookup"><span data-stu-id="41960-170">hello dashboard includes hello columns in hello following table.</span></span> <span data-ttu-id="41960-171">Každý sloupec uvádí hello top 10 události pomocí počet odpovídajících, že kritéria sloupce pro hello zadaný časový rozsah.</span><span class="sxs-lookup"><span data-stu-id="41960-171">Each column lists hello top 10 events by count matching that column's criteria for hello specified time range.</span></span> <span data-ttu-id="41960-172">Můžete spustit hledání protokolu, které poskytuje celý seznam hello kliknutím **zobrazit všechny** v pravém dolním hello jednotlivých sloupců, nebo klikněte na záhlaví sloupce hello.</span><span class="sxs-lookup"><span data-stu-id="41960-172">You can run a log search that provides hello entire list by clicking **See all** at hello right bottom of each column, or by clicking hello column header.</span></span>

| <span data-ttu-id="41960-173">**Události Service Fabric**</span><span class="sxs-lookup"><span data-stu-id="41960-173">**Service Fabric event**</span></span> | <span data-ttu-id="41960-174">**Popis**</span><span class="sxs-lookup"><span data-stu-id="41960-174">**description**</span></span> |
| --- | --- |
| <span data-ttu-id="41960-175">Významné problémy</span><span class="sxs-lookup"><span data-stu-id="41960-175">Notable Issues</span></span> |<span data-ttu-id="41960-176">Zobrazí problémy, jako je RunAsyncFailures RunAsynCancellations a seznamy uzlu.</span><span class="sxs-lookup"><span data-stu-id="41960-176">A Display of issues such as RunAsyncFailures RunAsynCancellations and Node Downs.</span></span> |
| <span data-ttu-id="41960-177">Provozní události</span><span class="sxs-lookup"><span data-stu-id="41960-177">Operational Events</span></span> |<span data-ttu-id="41960-178">Významné provozní události, jako je například upgradu aplikace a nasazení.</span><span class="sxs-lookup"><span data-stu-id="41960-178">Notable operational events such as application upgrade and deployments.</span></span> |
| <span data-ttu-id="41960-179">Události spolehlivé služby</span><span class="sxs-lookup"><span data-stu-id="41960-179">Reliable Service Events</span></span> |<span data-ttu-id="41960-180">Události významné spolehlivé služby takové Runasyncinvocations.</span><span class="sxs-lookup"><span data-stu-id="41960-180">Notable reliable service events such a Runasyncinvocations.</span></span> |
| <span data-ttu-id="41960-181">Události objektu actor</span><span class="sxs-lookup"><span data-stu-id="41960-181">Actor Events</span></span> |<span data-ttu-id="41960-182">Významné objektu actor události vygenerované modulem vaší micro služby, jako je výjimky vyvolané metodu objektu actor, objektu actor aktivací a deaktivací a tak dále.</span><span class="sxs-lookup"><span data-stu-id="41960-182">Notable actor events generated by your micro-services, such as exceptions thrown by an actor method, actor activations and deactivations, and so on.</span></span> |
| <span data-ttu-id="41960-183">Události aplikace</span><span class="sxs-lookup"><span data-stu-id="41960-183">Application Events</span></span> |<span data-ttu-id="41960-184">Všechny vlastní události trasování událostí generovaných aplikací.</span><span class="sxs-lookup"><span data-stu-id="41960-184">All custom ETW events generated by your applications.</span></span> |

![Řídicí panel Service Fabric](./media/log-analytics-service-fabric/sf3.png)

![Řídicí panel Service Fabric](./media/log-analytics-service-fabric/sf4.png)

<span data-ttu-id="41960-187">Hello následující tabulka uvádí metody shromažďování dat a další podrobnosti o tom, jak se data shromažďují pro Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="41960-187">hello following table shows data collection methods and other details about how data is collected for Service Fabric.</span></span>

| <span data-ttu-id="41960-188">Platforma</span><span class="sxs-lookup"><span data-stu-id="41960-188">platform</span></span> | <span data-ttu-id="41960-189">Přímé agenta</span><span class="sxs-lookup"><span data-stu-id="41960-189">Direct Agent</span></span> | <span data-ttu-id="41960-190">Agent nástroje Operations Manager</span><span class="sxs-lookup"><span data-stu-id="41960-190">Operations Manager agent</span></span> | <span data-ttu-id="41960-191">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="41960-191">Azure Storage</span></span> | <span data-ttu-id="41960-192">Nástroj Operations Manager vyžaduje?</span><span class="sxs-lookup"><span data-stu-id="41960-192">Operations Manager required?</span></span> | <span data-ttu-id="41960-193">Dat agenta nástroje Operations Manager odeslána prostřednictvím skupiny pro správu</span><span class="sxs-lookup"><span data-stu-id="41960-193">Operations Manager agent data sent via management group</span></span> | <span data-ttu-id="41960-194">Frekvence kolekce</span><span class="sxs-lookup"><span data-stu-id="41960-194">collection frequency</span></span> |
| --- | --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="41960-195">Windows</span><span class="sxs-lookup"><span data-stu-id="41960-195">Windows</span></span> |  |  | <span data-ttu-id="41960-196">&#8226;</span><span class="sxs-lookup"><span data-stu-id="41960-196">&#8226;</span></span> |  |  |<span data-ttu-id="41960-197">10 minut</span><span class="sxs-lookup"><span data-stu-id="41960-197">10 minutes</span></span> |

> [!NOTE]
> <span data-ttu-id="41960-198">Kliknutím můžete změnit rozsah hello z těchto událostí v hello Service Fabric řešení **dat podle posledních 7 dnů** hello horní části řídicího panelu hello.</span><span class="sxs-lookup"><span data-stu-id="41960-198">You can change hello scope of these events in hello Service Fabric solution by clicking **Data based on last 7 days** at hello top of hello dashboard.</span></span> <span data-ttu-id="41960-199">Můžete také zobrazit události generované v rámci hello posledních sedmi dnů, jeden den nebo šest hodin.</span><span class="sxs-lookup"><span data-stu-id="41960-199">You can also show events generated within hello last seven days, one day, or six hours.</span></span> <span data-ttu-id="41960-200">Nebo můžete vybrat **vlastní** toospecify vlastní období.</span><span class="sxs-lookup"><span data-stu-id="41960-200">Or, you can select **Custom** toospecify a custom date range.</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="41960-201">Další kroky</span><span class="sxs-lookup"><span data-stu-id="41960-201">Next steps</span></span>

* <span data-ttu-id="41960-202">Použití [protokolu hledání v analýzy protokolů](log-analytics-log-searches.md) tooview podrobná data události Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="41960-202">Use [Log Searches in Log Analytics](log-analytics-log-searches.md) tooview detailed Service Fabric event data.</span></span>
