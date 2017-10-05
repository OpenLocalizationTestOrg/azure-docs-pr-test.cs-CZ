---
title: "Hodnocení aplikace Service Fabric pomocí portálu Azure Log Analytics | Microsoft Docs"
description: "Můžete vytvořit řešení Service Fabric v Log Analytics pomocí portálu Azure k vyhodnocení rizik a stavu Service Fabric aplikací, micro-services, uzly a clustery."
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
ms.openlocfilehash: 8c564c0dcbb2f9be286917b2f4d8a40da5406fae
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="assess-service-fabric-applications-and-micro-services-with-the-azure-portal"></a><span data-ttu-id="919e2-103">Hodnocení aplikace Service Fabric a micro služby pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="919e2-103">Assess Service Fabric applications and micro-services with the Azure portal</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="919e2-104">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="919e2-104">Resource Manager</span></span>](log-analytics-service-fabric-azure-resource-manager.md)
> * [<span data-ttu-id="919e2-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="919e2-105">PowerShell</span></span>](log-analytics-service-fabric.md)
>
>

![Symbol Service Fabric](./media/log-analytics-service-fabric/service-fabric-assessment-symbol.png)

<span data-ttu-id="919e2-107">Tento článek popisuje, jak používat řešení Service Fabric v analýzy protokolů pro identifikaci a řešení potíží s napříč cluster Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="919e2-107">This article describes how to use the Service Fabric solution in Log Analytics to help identify and troubleshoot issues across your Service Fabric cluster.</span></span>

<span data-ttu-id="919e2-108">Service Fabric řešení používá Azure Diagnostics data z virtuálních počítačů služby infrastruktury, shromažďováním těchto dat z Azure WAD tabulek.</span><span class="sxs-lookup"><span data-stu-id="919e2-108">The Service Fabric solution uses Azure Diagnostics data from your Service Fabric VMs, by collecting this data from your Azure WAD tables.</span></span> <span data-ttu-id="919e2-109">Analýzy protokolů potom načte události framework Service Fabric, včetně **spolehlivé události služby**, **události objektu Actor**, **provozní události**, a **vlastní Události trasování událostí**.</span><span class="sxs-lookup"><span data-stu-id="919e2-109">Log Analytics then reads Service Fabric framework events, including **Reliable Service Events**, **Actor Events**, **Operational Events**, and **Custom ETW events**.</span></span> <span data-ttu-id="919e2-110">Pomocí řídicího panelu řešení budete moci zobrazit významné problémy a relevantní události ve vašem prostředí Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="919e2-110">With the solution dashboard, you are able to view notable issues and relevant events in your Service Fabric environment.</span></span>

<span data-ttu-id="919e2-111">Chcete-li začít pracovat s řešením, cluster Service Fabric připojit k pracovní prostor analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="919e2-111">To get started with the solution, you need to connect your Service Fabric cluster to a Log Analytics workspace.</span></span> <span data-ttu-id="919e2-112">Tady jsou tři scénáře, které je třeba zvážit:</span><span class="sxs-lookup"><span data-stu-id="919e2-112">Here are three scenarios to consider:</span></span>

1. <span data-ttu-id="919e2-113">Pokud jste nenasadili cluster Service Fabric, postupujte podle kroků v ***nasadit Service Fabric Cluster připojené k pracovní prostor analýzy protokolů*** k nasazení do nového clusteru a konfigurace. do sestavy k analýze protokolů.</span><span class="sxs-lookup"><span data-stu-id="919e2-113">If you have not deployed your Service Fabric cluster, use the steps in ***Deploy a Service Fabric Cluster connected to a Log Analytics workspace*** to deploy a new cluster and have it configured to report to Log Analytics.</span></span>
2. <span data-ttu-id="919e2-114">Pokud potřebujete shromáždit čítače výkonu z hostitele tak, aby pomocí jiných řešení OMS například zabezpečení na váš Cluster Service Fabric, postupujte podle kroků v ***nasadit Service Fabric Cluster připojené k pracovní prostor analýzy protokolů pomocí rozšíření virtuálního počítače nainstalovat.***</span><span class="sxs-lookup"><span data-stu-id="919e2-114">If you need to collect performance counters from your hosts to use other OMS solutions such as Security on your Service Fabric Cluster, follow the steps in ***Deploy a Service Fabric Cluster connected to a Log Analytics workspace with VM Extension installed.***</span></span>
3. <span data-ttu-id="919e2-115">Pokud už jste nasadili cluster Service Fabric a chcete se připojit k analýze protokolů, postupujte podle kroků v ***přidání stávající účet úložiště k analýze protokolů.***</span><span class="sxs-lookup"><span data-stu-id="919e2-115">If you have already deployed your Service Fabric cluster and want to connect it to Log Analytics, follow the steps in ***Adding an existing storage account to Log Analytics.***</span></span>

## <a name="deploy-a-service-fabric-cluster-connected-to-a-log-analytics-workspace"></a><span data-ttu-id="919e2-116">Nasaďte Cluster Service Fabric, připojení k pracovní prostor analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="919e2-116">Deploy a Service Fabric Cluster connected to a Log Analytics workspace.</span></span>
<span data-ttu-id="919e2-117">Tato šablona provede následující akce:</span><span class="sxs-lookup"><span data-stu-id="919e2-117">This template does the following:</span></span>

1. <span data-ttu-id="919e2-118">Nasazení clusteru služby Azure Service Fabric už připojená k pracovnímu prostoru analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="919e2-118">Deploys an Azure Service Fabric cluster already connected to a Log Analytics workspace.</span></span> <span data-ttu-id="919e2-119">Máte možnost vytvořit nový pracovní prostor při nasazení šablony, nebo zadejte název stávající pracovní prostor analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="919e2-119">You have the option to create a new workspace while deploying the template, or input the name of an already existing Log Analytics workspace.</span></span>
2. <span data-ttu-id="919e2-120">Účet úložiště diagnostiky přidá do pracovního prostoru analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="919e2-120">Adds the diagnostic storage account to the Log Analytics workspace.</span></span>
3. <span data-ttu-id="919e2-121">Umožňuje řešení Service Fabric v pracovním prostoru analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="919e2-121">Enables the Service Fabric solution in your Log Analytics workspace.</span></span>

<span data-ttu-id="919e2-122">[![Nasazení do Azure](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fazure%2Fazure-quickstart-templates%2Fmaster%2Fservice-fabric-oms%2F%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="919e2-122">[![Deploy to Azure](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fazure%2Fazure-quickstart-templates%2Fmaster%2Fservice-fabric-oms%2F%2Fazuredeploy.json)</span></span>

<span data-ttu-id="919e2-123">Jakmile vyberete výše uvedené tlačítko nasadit, portálu Azure se otevře s parametry, je možné upravit.</span><span class="sxs-lookup"><span data-stu-id="919e2-123">Once you select the deploy button above, the Azure portal opens with parameters for you to edit.</span></span> <span data-ttu-id="919e2-124">Ujistěte se, že vytvoříte novou skupinu prostředků, pokud zadejte nový název pracovního prostoru analýzy protokolů:</span><span class="sxs-lookup"><span data-stu-id="919e2-124">Be sure to create a new resource group if you input a new Log Analytics workspace name:</span></span>

![Service Fabric](./media/log-analytics-service-fabric/2.png)

![Service Fabric](./media/log-analytics-service-fabric/3.png)

<span data-ttu-id="919e2-127">Přijměte právní podmínky a klikněte na tlačítko **vytvořit** ke spuštění nasazení.</span><span class="sxs-lookup"><span data-stu-id="919e2-127">Accept the legal terms and click **Create** to start the deployment.</span></span> <span data-ttu-id="919e2-128">Po dokončení nasazení se měly zobrazit nový pracovní prostor a vytvoření clusteru a WADServiceFabric * událostí, WADWindowsEventLogs a WADETWEvent tabulek, které jsou přidány:</span><span class="sxs-lookup"><span data-stu-id="919e2-128">Once the deployment is completed, you should see the new workspace and cluster created, and the WADServiceFabric*Event, WADWindowsEventLogs and WADETWEvent tables added:</span></span>

![Service Fabric](./media/log-analytics-service-fabric/4.png)

## <a name="deploy-a-service-fabric-cluster-connected-to-a-log-analytics-workspace-with-vm-extension-installed"></a><span data-ttu-id="919e2-130">Nasaďte Cluster Service Fabric, připojení k pracovní prostor analýzy protokolů se nainstalovat rozšíření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="919e2-130">Deploy a Service Fabric Cluster connected to a Log Analytics workspace with VM Extension installed.</span></span>

<span data-ttu-id="919e2-131">Tato šablona provede následující akce:</span><span class="sxs-lookup"><span data-stu-id="919e2-131">This template does the following:</span></span>

1. <span data-ttu-id="919e2-132">Nasazení clusteru služby Azure Service Fabric už připojená k pracovnímu prostoru analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="919e2-132">Deploys an Azure Service Fabric cluster already connected to a Log Analytics workspace.</span></span> <span data-ttu-id="919e2-133">Můžete vytvořit nový pracovní prostor nebo použijte existující.</span><span class="sxs-lookup"><span data-stu-id="919e2-133">You can create a new workspace or use an existing one.</span></span>
2. <span data-ttu-id="919e2-134">Účty úložiště diagnostiky přidá do pracovního prostoru analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="919e2-134">Adds the diagnostic storage accounts to the Log Analytics workspace.</span></span>
3. <span data-ttu-id="919e2-135">Umožňuje řešení Service Fabric v pracovním prostoru analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="919e2-135">Enables the Service Fabric solution in the Log Analytics workspace.</span></span>
4. <span data-ttu-id="919e2-136">Instalaci rozšíření agenta MMA v jednotlivých sad v clusteru Service Fabric škálování virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="919e2-136">Installs the MMA agent extension in each virtual machine scale set in your Service Fabric cluster.</span></span> <span data-ttu-id="919e2-137">Instalaci agenta MMA budete moci zobrazovat metrika výkonu o uzly.</span><span class="sxs-lookup"><span data-stu-id="919e2-137">With the MMA agent installed, you are able to view performance metrics about your nodes.</span></span>

<span data-ttu-id="919e2-138">[![Nasazení do Azure](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fazure%2Fazure-quickstart-templates%2Fmaster%2Fservice-fabric-vmss-oms%2F%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="919e2-138">[![Deploy to Azure](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fazure%2Fazure-quickstart-templates%2Fmaster%2Fservice-fabric-vmss-oms%2F%2Fazuredeploy.json)</span></span>

<span data-ttu-id="919e2-139">Stejný postup výše zadejte potřebné parametry a ji nasazení.</span><span class="sxs-lookup"><span data-stu-id="919e2-139">Following the same steps above, input the necessary parameters, and kick off a deployment.</span></span> <span data-ttu-id="919e2-140">Měli byste vidět znovu nový pracovní prostor, clusteru a všechny vytvořené WAD tabulky:</span><span class="sxs-lookup"><span data-stu-id="919e2-140">Once again you should see the new workspace, cluster and WAD tables all created:</span></span>

![Service Fabric](./media/log-analytics-service-fabric/5.png)

### <a name="viewing-performance-data"></a><span data-ttu-id="919e2-142">Zobrazení dat výkonu</span><span class="sxs-lookup"><span data-stu-id="919e2-142">Viewing Performance Data</span></span>

<span data-ttu-id="919e2-143">Zobrazení dat výkonu z uzlů:</span><span class="sxs-lookup"><span data-stu-id="919e2-143">To view Perf Data from your nodes:</span></span>


[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

- <span data-ttu-id="919e2-144">Spusťte pracovní prostor analýzy protokolů z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="919e2-144">Launch the Log Analytics workspace from the Azure portal.</span></span>
  <span data-ttu-id="919e2-145">![Service Fabric](./media/log-analytics-service-fabric/6.png)</span><span class="sxs-lookup"><span data-stu-id="919e2-145">![Service Fabric](./media/log-analytics-service-fabric/6.png)</span></span>
- <span data-ttu-id="919e2-146">V levém podokně přejděte do nastavení a vyberte Data >> čítačů výkonu systému Windows >> "Přidat vybrané čítače výkonu": ![Service Fabric](./media/log-analytics-service-fabric/7.png)</span><span class="sxs-lookup"><span data-stu-id="919e2-146">Go to Settings on the left pane, and select Data >> Windows Performance Counters >> "Add the selected performance counters": ![Service Fabric](./media/log-analytics-service-fabric/7.png)</span></span>
- <span data-ttu-id="919e2-147">V hledání protokolů použijte následující dotazy na pustíte do klíčových metrik týkajících se uzly:</span><span class="sxs-lookup"><span data-stu-id="919e2-147">In Log Search, use the following queries to delve into key metrics about your nodes:</span></span>

    <span data-ttu-id="919e2-148">a.</span><span class="sxs-lookup"><span data-stu-id="919e2-148">a.</span></span> <span data-ttu-id="919e2-149">Za poslední hodinu uzlů, které jsou problémy s a v jaké časovém intervalu uzlu měl Špička porovnejte průměrné využití procesoru pro všechny uzly:</span><span class="sxs-lookup"><span data-stu-id="919e2-149">Compare the average CPU Utilization across all your nodes in the last one hour to see which nodes are having issues and at what time interval a node had a spike:</span></span>

    ```
    Type=Perf ObjectName=Processor CounterName="% Processor Time"|measure avg(CounterValue) by Computer Interval 1HOUR.
    ```

    ![Service Fabric](./media/log-analytics-service-fabric/10.png)

    <span data-ttu-id="919e2-151">b.</span><span class="sxs-lookup"><span data-stu-id="919e2-151">b.</span></span> <span data-ttu-id="919e2-152">Zobrazit podobné spojnicových grafů pro dostupná paměť na každý uzel k tomuto dotazu:</span><span class="sxs-lookup"><span data-stu-id="919e2-152">View similar line charts for available memory on each node with this query:</span></span>

    ```
    Type=Perf ObjectName=Memory CounterName="Available MBytes Memory" | measure avg(CounterValue) by Computer Interval 1HOUR.
    ```

    <span data-ttu-id="919e2-153">Chcete-li zobrazit seznam všech uzlech, zobrazující přesný průměrnou hodnotu pro počet MB k dispozici pro každý uzel, použijte tento dotaz:</span><span class="sxs-lookup"><span data-stu-id="919e2-153">To view a listing of all your nodes, showing the exact average value for Available MBytes for each node, use this query:</span></span>

    ```
    Type=Perf (ObjectName=Memory) (CounterName="Available MBytes") | measure avg(CounterValue) by Computer
    ```

    ![Service Fabric](./media/log-analytics-service-fabric/11.png)

    <span data-ttu-id="919e2-155">c.</span><span class="sxs-lookup"><span data-stu-id="919e2-155">c.</span></span> <span data-ttu-id="919e2-156">V případě, že chcete rozbalit konkrétním uzlu tak, že prověří hodinové průměr, minimální, maximální a 75 percentilu využití procesoru budete moci provést pomocí tohoto dotazu (nahraďte pole počítače):</span><span class="sxs-lookup"><span data-stu-id="919e2-156">In the case that you want to drill down into a specific node by examining the hourly average, minimum, maximum and 75-percentile CPU usage, you're able to do this by using this query (replace Computer field):</span></span>

    ```
    Type=Perf CounterName="% Processor Time" InstanceName=_Total Computer="BaconDC01.BaconLand.com"| measure min(CounterValue), avg(CounterValue), percentile75(CounterValue), max(CounterValue) by Computer Interval 1HOUR
    ```

    ![Service Fabric](./media/log-analytics-service-fabric/12.png)

<span data-ttu-id="919e2-158">Další informace o metriky výkonu v analýzy protokolů v [Operations Management Suite blog](https://blogs.technet.microsoft.com/msoms/tag/metrics/).</span><span class="sxs-lookup"><span data-stu-id="919e2-158">Read more information about performance metrics in Log Analytics at the [Operations Management Suite blog](https://blogs.technet.microsoft.com/msoms/tag/metrics/).</span></span>


## <a name="adding-an-existing-storage-account-to-log-analytics"></a><span data-ttu-id="919e2-159">Přidání stávající účet úložiště k analýze protokolů</span><span class="sxs-lookup"><span data-stu-id="919e2-159">Adding an existing storage account to Log Analytics</span></span>

<span data-ttu-id="919e2-160">Tato šablona jednoduše přidá existující účty úložiště do nového nebo stávajícího pracovního prostoru analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="919e2-160">This template simply adds your existing storage accounts to a new or existing Log Analytics workspace.</span></span>

<span data-ttu-id="919e2-161">[![Nasazení do Azure](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Foms-existing-storage-account%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="919e2-161">[![Deploy to Azure](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Foms-existing-storage-account%2Fazuredeploy.json)</span></span>

> [!NOTE]
> <span data-ttu-id="919e2-162">Výběr skupiny prostředků, pokud pracujete s již existující pracovní prostor analýzy protokolů, vyberte "Použití existující" a vyhledávání pro skupinu prostředků obsahující pracovní prostor analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="919e2-162">In selecting a Resource Group, if you're working with an already existing Log Analytics workspace, select "Use Existing" and search for the resource group containing the Log Analytics workspace.</span></span> <span data-ttu-id="919e2-163">Vytvořit nový jeden Pokud jinak.</span><span class="sxs-lookup"><span data-stu-id="919e2-163">Create a new one if otherwise.</span></span>
> <span data-ttu-id="919e2-164">![Service Fabric](./media/log-analytics-service-fabric/8.png)</span><span class="sxs-lookup"><span data-stu-id="919e2-164">![Service Fabric](./media/log-analytics-service-fabric/8.png)</span></span>
>
>

<span data-ttu-id="919e2-165">Po nasazení této šablony, nebudete moci zobrazit účet úložiště, které jsou připojené k pracovní prostor analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="919e2-165">After this template has been deployed, you will be able to see the storage account connected to your Log Analytics workspace.</span></span> <span data-ttu-id="919e2-166">V tomto případě přidaný do pracovního prostoru Exchange, kterou jste vytvořili výše jeden další účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="919e2-166">In this instance, I added one more storage account to the Exchange workspace I created above.</span></span>
<span data-ttu-id="919e2-167">![Service Fabric](./media/log-analytics-service-fabric/9.png)</span><span class="sxs-lookup"><span data-stu-id="919e2-167">![Service Fabric](./media/log-analytics-service-fabric/9.png)</span></span>

## <a name="view-service-fabric-events"></a><span data-ttu-id="919e2-168">Zobrazit události Service Fabric</span><span class="sxs-lookup"><span data-stu-id="919e2-168">View Service Fabric events</span></span>

<span data-ttu-id="919e2-169">Po dokončení nasazení a řešení Service Fabric povolen v pracovním prostoru, vybrat **Service Fabric** dlaždici na portálu analýzy protokolů a spusťte Service Fabric řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="919e2-169">Once the deployments are completed and the Service Fabric solution has been enabled in your workspace, select the **Service Fabric** tile in the Log Analytics portal to launch the Service Fabric dashboard.</span></span> <span data-ttu-id="919e2-170">Řídicí panel obsahuje sloupce v následující tabulce.</span><span class="sxs-lookup"><span data-stu-id="919e2-170">The dashboard includes the columns in the following table.</span></span> <span data-ttu-id="919e2-171">Každý sloupec uvádí top 10 události podle počtu odpovídající kritériím tento sloupec pro zadaný časový rozsah.</span><span class="sxs-lookup"><span data-stu-id="919e2-171">Each column lists the top 10 events by count matching that column's criteria for the specified time range.</span></span> <span data-ttu-id="919e2-172">Můžete spustit hledání protokolu, které poskytuje celý seznam kliknutím **zobrazit všechny** v pravé dolní jednotlivých sloupců, nebo kliknutím na záhlaví sloupce.</span><span class="sxs-lookup"><span data-stu-id="919e2-172">You can run a log search that provides the entire list by clicking **See all** at the right bottom of each column, or by clicking the column header.</span></span>

| <span data-ttu-id="919e2-173">**Události Service Fabric**</span><span class="sxs-lookup"><span data-stu-id="919e2-173">**Service Fabric event**</span></span> | <span data-ttu-id="919e2-174">**Popis**</span><span class="sxs-lookup"><span data-stu-id="919e2-174">**description**</span></span> |
| --- | --- |
| <span data-ttu-id="919e2-175">Významné problémy</span><span class="sxs-lookup"><span data-stu-id="919e2-175">Notable Issues</span></span> |<span data-ttu-id="919e2-176">Zobrazí problémy, jako je RunAsyncFailures RunAsynCancellations a seznamy uzlu.</span><span class="sxs-lookup"><span data-stu-id="919e2-176">A Display of issues such as RunAsyncFailures RunAsynCancellations and Node Downs.</span></span> |
| <span data-ttu-id="919e2-177">Provozní události</span><span class="sxs-lookup"><span data-stu-id="919e2-177">Operational Events</span></span> |<span data-ttu-id="919e2-178">Významné provozní události, jako je například upgradu aplikace a nasazení.</span><span class="sxs-lookup"><span data-stu-id="919e2-178">Notable operational events such as application upgrade and deployments.</span></span> |
| <span data-ttu-id="919e2-179">Události spolehlivé služby</span><span class="sxs-lookup"><span data-stu-id="919e2-179">Reliable Service Events</span></span> |<span data-ttu-id="919e2-180">Události významné spolehlivé služby takové Runasyncinvocations.</span><span class="sxs-lookup"><span data-stu-id="919e2-180">Notable reliable service events such a Runasyncinvocations.</span></span> |
| <span data-ttu-id="919e2-181">Události objektu actor</span><span class="sxs-lookup"><span data-stu-id="919e2-181">Actor Events</span></span> |<span data-ttu-id="919e2-182">Významné objektu actor události vygenerované modulem vaší micro služby, jako je výjimky vyvolané metodu objektu actor, objektu actor aktivací a deaktivací a tak dále.</span><span class="sxs-lookup"><span data-stu-id="919e2-182">Notable actor events generated by your micro-services, such as exceptions thrown by an actor method, actor activations and deactivations, and so on.</span></span> |
| <span data-ttu-id="919e2-183">Události aplikace</span><span class="sxs-lookup"><span data-stu-id="919e2-183">Application Events</span></span> |<span data-ttu-id="919e2-184">Všechny vlastní události trasování událostí generovaných aplikací.</span><span class="sxs-lookup"><span data-stu-id="919e2-184">All custom ETW events generated by your applications.</span></span> |

![Řídicí panel Service Fabric](./media/log-analytics-service-fabric/sf3.png)

![Řídicí panel Service Fabric](./media/log-analytics-service-fabric/sf4.png)

<span data-ttu-id="919e2-187">Následující tabulka uvádí metody shromažďování dat a další podrobnosti o tom, jak se data shromažďují pro Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="919e2-187">The following table shows data collection methods and other details about how data is collected for Service Fabric.</span></span>

| <span data-ttu-id="919e2-188">Platforma</span><span class="sxs-lookup"><span data-stu-id="919e2-188">platform</span></span> | <span data-ttu-id="919e2-189">Přímé agenta</span><span class="sxs-lookup"><span data-stu-id="919e2-189">Direct Agent</span></span> | <span data-ttu-id="919e2-190">Agent nástroje Operations Manager</span><span class="sxs-lookup"><span data-stu-id="919e2-190">Operations Manager agent</span></span> | <span data-ttu-id="919e2-191">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="919e2-191">Azure Storage</span></span> | <span data-ttu-id="919e2-192">Nástroj Operations Manager vyžaduje?</span><span class="sxs-lookup"><span data-stu-id="919e2-192">Operations Manager required?</span></span> | <span data-ttu-id="919e2-193">Dat agenta nástroje Operations Manager odeslána prostřednictvím skupiny pro správu</span><span class="sxs-lookup"><span data-stu-id="919e2-193">Operations Manager agent data sent via management group</span></span> | <span data-ttu-id="919e2-194">Frekvence kolekce</span><span class="sxs-lookup"><span data-stu-id="919e2-194">collection frequency</span></span> |
| --- | --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="919e2-195">Windows</span><span class="sxs-lookup"><span data-stu-id="919e2-195">Windows</span></span> |  |  | <span data-ttu-id="919e2-196">&#8226;</span><span class="sxs-lookup"><span data-stu-id="919e2-196">&#8226;</span></span> |  |  |<span data-ttu-id="919e2-197">10 minut</span><span class="sxs-lookup"><span data-stu-id="919e2-197">10 minutes</span></span> |

> [!NOTE]
> <span data-ttu-id="919e2-198">Rozsah těchto událostí v Service Fabric řešení můžete změnit kliknutím **dat podle posledních 7 dnů** v horní části řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="919e2-198">You can change the scope of these events in the Service Fabric solution by clicking **Data based on last 7 days** at the top of the dashboard.</span></span> <span data-ttu-id="919e2-199">Můžete také zobrazit události generované během posledních sedmi dnů, jeden den nebo šest hodin.</span><span class="sxs-lookup"><span data-stu-id="919e2-199">You can also show events generated within the last seven days, one day, or six hours.</span></span> <span data-ttu-id="919e2-200">Nebo můžete vybrat **vlastní** zadat vlastní období.</span><span class="sxs-lookup"><span data-stu-id="919e2-200">Or, you can select **Custom** to specify a custom date range.</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="919e2-201">Další kroky</span><span class="sxs-lookup"><span data-stu-id="919e2-201">Next steps</span></span>

* <span data-ttu-id="919e2-202">Použití [protokolu hledání v analýzy protokolů](log-analytics-log-searches.md) zobrazíte podrobné data události Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="919e2-202">Use [Log Searches in Log Analytics](log-analytics-log-searches.md) to view detailed Service Fabric event data.</span></span>
