---
title: "Nastavení clusteru Azure Service Fabric | Dokumentace Microsoftu"
description: "Rychlé zprovoznění – vytvoření clusteru Service Fabric s Windows nebo Linuxem v Azure."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/24/2017
ms.author: ryanwi
ms.openlocfilehash: ec59450052b377412a28f7eaf55d1f1512b55195
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-your-first-service-fabric-cluster-on-azure"></a><span data-ttu-id="5470a-103">Vytvoření vašeho prvního clusteru Service Fabric v Azure</span><span class="sxs-lookup"><span data-stu-id="5470a-103">Create your first Service Fabric cluster on Azure</span></span>
<span data-ttu-id="5470a-104">[Cluster Service Fabric](service-fabric-deploy-anywhere.md) je síťově propojená sada virtuálních nebo fyzických počítačů, ve které se nasazují a spravují mikroslužby.</span><span class="sxs-lookup"><span data-stu-id="5470a-104">A [Service Fabric cluster](service-fabric-deploy-anywhere.md) is a network-connected set of virtual or physical machines into which your microservices are deployed and managed.</span></span> <span data-ttu-id="5470a-105">Tento Rychlý start vám pomůže během několika minut vytvořit cluster s pěti uzly běžící ve Windows nebo Linuxu, a to prostřednictvím [Azure PowerShellu](https://msdn.microsoft.com/library/dn135248) nebo webu [Azure Portal](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="5470a-105">This quickstart helps you to create a five-node cluster, running on either Windows or Linux, through the [Azure PowerShell](https://msdn.microsoft.com/library/dn135248) or [Azure portal](http://portal.azure.com) in just a few minutes.</span></span>  

<span data-ttu-id="5470a-106">Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.</span><span class="sxs-lookup"><span data-stu-id="5470a-106">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>


## <a name="use-the-azure-portal"></a><span data-ttu-id="5470a-107">Použití webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="5470a-107">Use the Azure portal</span></span>

<span data-ttu-id="5470a-108">Přihlaste se k webu Azure Portal na adrese [http://portal.azure.com](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="5470a-108">Log in to the Azure portal at [http://portal.azure.com](http://portal.azure.com).</span></span>

### <a name="create-the-cluster"></a><span data-ttu-id="5470a-109">Vytvoření clusteru</span><span class="sxs-lookup"><span data-stu-id="5470a-109">Create the cluster</span></span>

1. <span data-ttu-id="5470a-110">Klikněte na tlačítko **Nový** v levém horním rohu webu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="5470a-110">Click the **New** button found on the upper left-hand corner of the Azure portal.</span></span>
2. <span data-ttu-id="5470a-111">V okně **Nový** vyberte **Compute** a potom v okně **Compute** vyberte **Cluster Service Fabric**.</span><span class="sxs-lookup"><span data-stu-id="5470a-111">Select **Compute** from the **New** blade and then select **Service Fabric Cluster** from the **Compute** blade.</span></span>
3. <span data-ttu-id="5470a-112">Pro Service Fabric vyplňte formulář **Základy**.</span><span class="sxs-lookup"><span data-stu-id="5470a-112">Fill out the Service Fabric **Basics** form.</span></span> <span data-ttu-id="5470a-113">V poli **Operační systém** vyberte verzi systému Windows nebo Linux, na které chcete spouštět uzly clusteru.</span><span class="sxs-lookup"><span data-stu-id="5470a-113">For **Operating system**, select the version of Windows or Linux you want the cluster nodes to run.</span></span> <span data-ttu-id="5470a-114">Uživatelské jméno a heslo, které tady zadáte, se používá k přihlášení k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="5470a-114">The user name and password entered here is used to log in to the virtual machine.</span></span> <span data-ttu-id="5470a-115">V části **Skupina prostředků** vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="5470a-115">For **Resource group**, create a new one.</span></span> <span data-ttu-id="5470a-116">Skupina prostředků je logický kontejner, ve kterém se vytváří a hromadně spravují prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="5470a-116">A resource group is a logical container into which Azure resources are created and collectively managed.</span></span> <span data-ttu-id="5470a-117">Jakmile budete hotovi, klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="5470a-117">When complete, click **OK**.</span></span>

    ![Výstup po instalaci clusteru][cluster-setup-basics]

4. <span data-ttu-id="5470a-119">Vyplňte formulář **Konfigurace clusteru**.</span><span class="sxs-lookup"><span data-stu-id="5470a-119">Fill out the **Cluster configuration** form.</span></span>  <span data-ttu-id="5470a-120">Jako **Počet typů uzlu** zadejte 1.</span><span class="sxs-lookup"><span data-stu-id="5470a-120">For **Node type count**, enter "1".</span></span>

5. <span data-ttu-id="5470a-121">Vyberte **Typ uzlu 1 (Primární)** a vyplňte formulář **Konfigurace typu uzlu**.</span><span class="sxs-lookup"><span data-stu-id="5470a-121">Select **Node type 1 (Primary)** and fill out the **Node type configuration** form.</span></span>  <span data-ttu-id="5470a-122">Zadejte název typu uzlu a nastavte [Úroveň odolnosti](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster) na Bronzová.</span><span class="sxs-lookup"><span data-stu-id="5470a-122">Enter a node type name and set the [Durability tier](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster) to "Bronze."</span></span>  <span data-ttu-id="5470a-123">Vyberte velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="5470a-123">Select a VM size.</span></span>

    <span data-ttu-id="5470a-124">Typy uzlů definují velikost virtuálních počítačů, jejich počet, vlastní koncové body a další nastavení pro virtuální počítače daného typu.</span><span class="sxs-lookup"><span data-stu-id="5470a-124">Node types define the VM size, number of VMs, custom endpoints, and other settings for the VMs of that type.</span></span> <span data-ttu-id="5470a-125">Každý definovaný typ uzlu je nastavený jako samostatná škálovací sada virtuálních počítačů, která se používá k nasazení a správě virtuálních počítačů jako sady.</span><span class="sxs-lookup"><span data-stu-id="5470a-125">Each node type defined is set up as a separate virtual machine scale set, which is used to deploy and managed virtual machines as a set.</span></span> <span data-ttu-id="5470a-126">U každého typu uzlu je možné nezávisle vertikálně navýšit nebo snížit kapacitu. Mají různé sady otevřených portů a můžou mít různé metriky kapacity.</span><span class="sxs-lookup"><span data-stu-id="5470a-126">Each node type can be scaled up or down independently, have different sets of ports open, and can have different capacity metrics.</span></span>  <span data-ttu-id="5470a-127">První (nebo primární) typ uzlu je ten, kde jsou hostované systémové služby Service Fabric. Musí mít pět nebo víc virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="5470a-127">The first, or primary, node type is where Service Fabric system services are hosted and must have five or more VMs.</span></span>

    <span data-ttu-id="5470a-128">Důležitým krokem každého produkčního nasazení je [plánování kapacity](service-fabric-cluster-capacity.md).</span><span class="sxs-lookup"><span data-stu-id="5470a-128">For any production deployment, [capacity planning](service-fabric-cluster-capacity.md) is an important step.</span></span>  <span data-ttu-id="5470a-129">V tomto rychlém zprovoznění ale nespouštíme aplikace, takže jako velikost virtuálního počítače vyberte *DS1_v2 Standard*.</span><span class="sxs-lookup"><span data-stu-id="5470a-129">For this quick start, however, you aren't running applications so select a *DS1_v2 Standard* VM size.</span></span>  <span data-ttu-id="5470a-130">Jako [Úroveň spolehlivosti](service-fabric-cluster-capacity.md#the-reliability-characteristics-of-the-cluster) vyberte Stříbrná a počáteční kapacitu škálovací sady virtuálních počítačů nastavte na hodnotu 5.</span><span class="sxs-lookup"><span data-stu-id="5470a-130">Select "Silver" for the [reliability tier](service-fabric-cluster-capacity.md#the-reliability-characteristics-of-the-cluster) and an initial virtual machine scale set capacity of 5.</span></span>  

    <span data-ttu-id="5470a-131">Vlastní koncové body otevřou porty v nástroji Azure Load Balancer, takže se můžete připojit k aplikacím běžícím v clusteru.</span><span class="sxs-lookup"><span data-stu-id="5470a-131">Custom endpoints open up ports in the Azure load balancer so that you can connect with applications running on the cluster.</span></span>  <span data-ttu-id="5470a-132">Zadejte 80, 8172. Otevřou se porty 80 a 8172.</span><span class="sxs-lookup"><span data-stu-id="5470a-132">Enter "80, 8172" to open up ports 80 and 8172.</span></span>

    <span data-ttu-id="5470a-133">Nezaškrtávejte políčko **Konfigurovat rozšířená nastavení**, které se používá pro přizpůsobení koncových bodů správy TCP/HTTP, rozsahů portů aplikací, [omezení umístění](service-fabric-cluster-resource-manager-configure-services.md#placement-constraints) a [vlastnosti kapacity](service-fabric-cluster-resource-manager-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="5470a-133">Do not check the **Configure advanced settings** box, which is used for customizing TCP/HTTP management endpoints, application port ranges, [placement constraints](service-fabric-cluster-resource-manager-configure-services.md#placement-constraints), and [capacity properties](service-fabric-cluster-resource-manager-metrics.md).</span></span>    

    <span data-ttu-id="5470a-134">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="5470a-134">Select **OK**.</span></span>

6. <span data-ttu-id="5470a-135">Ve formuláři **Konfigurace clusteru** nastavte **Diagnostika** na **Zapnuto**.</span><span class="sxs-lookup"><span data-stu-id="5470a-135">In the **Cluster configuration** form, set **Diagnostics** to **On**.</span></span>  <span data-ttu-id="5470a-136">Pro toto rychlé zprovoznění nemusíte zadávat žádné vlastnosti [nastavení prostředků infrastruktury](service-fabric-cluster-fabric-settings.md).</span><span class="sxs-lookup"><span data-stu-id="5470a-136">For this quickstart, you do not need to enter any [fabric setting](service-fabric-cluster-fabric-settings.md) properties.</span></span>  <span data-ttu-id="5470a-137">V poli **Verze prostředků infrastruktury** vyberte režim **automatického** upgradu, takže Microsoft automaticky aktualizuje verzi kódu prostředků infrastruktury v clusteru.</span><span class="sxs-lookup"><span data-stu-id="5470a-137">In **Fabric version**, select **Automatic** upgrade mode so that Microsoft automatically updates the version of the fabric code running the cluster.</span></span>  <span data-ttu-id="5470a-138">Pokud chcete [zvolit podporovanou verzi](service-fabric-cluster-upgrade.md) pro upgrade, nastavte režim na **Ruční**.</span><span class="sxs-lookup"><span data-stu-id="5470a-138">Set the mode to **Manual** if you want to [choose a supported version](service-fabric-cluster-upgrade.md) to upgrade to.</span></span> 

    ![Konfigurace typu uzlu][node-type-config]

    <span data-ttu-id="5470a-140">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="5470a-140">Select **OK**.</span></span>

7. <span data-ttu-id="5470a-141">Vyplňte formulář **Zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="5470a-141">Fill out the **Security** form.</span></span>  <span data-ttu-id="5470a-142">Pro toto rychlé zprovoznění vyberte **Nezabezpečené**.</span><span class="sxs-lookup"><span data-stu-id="5470a-142">For this quick start select **Unsecure**.</span></span>  <span data-ttu-id="5470a-143">Pro úlohy v produkčním prostředí ale důrazně doporučujeme vytvořit zabezpečený cluster vzhledem k tomu, že k nezabezpečenému clusteru se může anonymně připojit kdokoli a provádět operace správy.</span><span class="sxs-lookup"><span data-stu-id="5470a-143">It is highly recommended to create a secure cluster for production workloads, however, since anyone can anonymously connect to an unsecure cluster and perform management operations.</span></span>  

    <span data-ttu-id="5470a-144">Ve službě Service Fabric se k ověřování a šifrování pro zabezpečení různých aspektů clusteru a jeho aplikací využívají certifikáty.</span><span class="sxs-lookup"><span data-stu-id="5470a-144">Certificates are used in Service Fabric to provide authentication and encryption to secure various aspects of a cluster and its applications.</span></span> <span data-ttu-id="5470a-145">Další informace o použití certifikátů ve službě Service Fabric najdete v tématu věnovaném [scénářům zabezpečení clusteru Service Fabric](service-fabric-cluster-security.md).</span><span class="sxs-lookup"><span data-stu-id="5470a-145">For more information on how certificates are used in Service Fabric, see [Service Fabric cluster security scenarios](service-fabric-cluster-security.md).</span></span>  <span data-ttu-id="5470a-146">Pokud chcete povolit ověřování uživatelů pomocí Azure Active Directory nebo nastavit certifikáty pro zabezpečení aplikací, [vytvořte cluster pomocí šablony Resource Manageru](service-fabric-cluster-creation-via-arm.md).</span><span class="sxs-lookup"><span data-stu-id="5470a-146">To enable user authentication using Azure Active Directory or to set up certificates for application security, [create a cluster from a Resource Manager template](service-fabric-cluster-creation-via-arm.md).</span></span>

    <span data-ttu-id="5470a-147">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="5470a-147">Select **OK**.</span></span>

8. <span data-ttu-id="5470a-148">Zkontrolujte souhrnné informace.</span><span class="sxs-lookup"><span data-stu-id="5470a-148">Review the summary.</span></span>  <span data-ttu-id="5470a-149">Pokud chcete stáhnout šablonu Resource Manageru vytvořenou na základě nastavení, která jste zadali, vyberte **Stáhnout šablonu a parametry**.</span><span class="sxs-lookup"><span data-stu-id="5470a-149">If you'd like to download a Resource Manager template built from the settings you entered, select **Download template and parameters**.</span></span>  <span data-ttu-id="5470a-150">Vyberte **Vytvořit**. Cluster se vytvoří.</span><span class="sxs-lookup"><span data-stu-id="5470a-150">Select **Create** to create the cluster.</span></span>

    <span data-ttu-id="5470a-151">Průběh vytváření můžete sledovat v oznámeních.</span><span class="sxs-lookup"><span data-stu-id="5470a-151">You can see the creation progress in the notifications.</span></span> <span data-ttu-id="5470a-152">(Klikněte na ikonu zvonku u stavového řádku v pravém horním rohu obrazovky.) Pokud jste při vytváření clusteru klikli na **Připnout na Úvodní panel**, na tabuli **Start** bude připnuté **Nasazování clusteru Service Fabric**.</span><span class="sxs-lookup"><span data-stu-id="5470a-152">(Click the "Bell" icon near the status bar at the upper right of your screen.) If you clicked **Pin to Startboard** while creating the cluster, you see **Deploying Service Fabric Cluster** pinned to the **Start** board.</span></span>

### <a name="view-cluster-status"></a><span data-ttu-id="5470a-153">Zobrazení stavu clusteru</span><span class="sxs-lookup"><span data-stu-id="5470a-153">View cluster status</span></span>
<span data-ttu-id="5470a-154">Po vytvoření si cluster můžete prohlédnout na portálu v okně **Přehled**.</span><span class="sxs-lookup"><span data-stu-id="5470a-154">Once your cluster is created, you can inspect your cluster in the **Overview** blade in the portal.</span></span> <span data-ttu-id="5470a-155">Na řídicím panelu se zobrazí podrobnosti o clusteru, včetně veřejného koncového bodu clusteru a odkaz na Service Fabric Explorer.</span><span class="sxs-lookup"><span data-stu-id="5470a-155">You can now see the details of your cluster in the dashboard, including the cluster's public endpoint and a link to Service Fabric Explorer.</span></span>

![Stav clusteru][cluster-status]

### <a name="visualize-the-cluster-using-service-fabric-explorer"></a><span data-ttu-id="5470a-157">Vizualizace clusteru pomocí Service Fabric Exploreru</span><span class="sxs-lookup"><span data-stu-id="5470a-157">Visualize the cluster using Service Fabric explorer</span></span>
<span data-ttu-id="5470a-158">[Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) je nástroj vhodný pro vizualizaci clusteru a správu aplikací.</span><span class="sxs-lookup"><span data-stu-id="5470a-158">[Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) is a good tool for visualizing your cluster and managing applications.</span></span>  <span data-ttu-id="5470a-159">Service Fabric Explorer je služba, která běží v clusteru.</span><span class="sxs-lookup"><span data-stu-id="5470a-159">Service Fabric Explorer is a service that runs in the cluster.</span></span>  <span data-ttu-id="5470a-160">K přístupu využijte webový prohlížeč. Na stránce **Přehled** clusteru na portálu klikněte na odkaz **Service Fabric Explorer**.</span><span class="sxs-lookup"><span data-stu-id="5470a-160">Access it using a web browser by clicking the **Service Fabric Explorer** link of the cluster **Overview** page in the portal.</span></span>  <span data-ttu-id="5470a-161">Můžete taky zadat adresu přímo do prohlížeče: [http://quickstartcluster.westus.cloudapp.azure.com:19080/Explorer](http://quickstartcluster.westus.cloudapp.azure.com:19080/Explorer).</span><span class="sxs-lookup"><span data-stu-id="5470a-161">You can also enter the address directly into the browser: [http://quickstartcluster.westus.cloudapp.azure.com:19080/Explorer](http://quickstartcluster.westus.cloudapp.azure.com:19080/Explorer)</span></span>

<span data-ttu-id="5470a-162">Řídicí panel clusteru poskytuje přehled o clusteru včetně souhrnu stavu aplikací a uzlů.</span><span class="sxs-lookup"><span data-stu-id="5470a-162">The cluster dashboard provides an overview of your cluster, including a summary of application and node health.</span></span> <span data-ttu-id="5470a-163">Zobrazení uzlu obsahuje fyzické rozložení clusteru.</span><span class="sxs-lookup"><span data-stu-id="5470a-163">The node view shows the physical layout of the cluster.</span></span> <span data-ttu-id="5470a-164">Pro daný uzel můžete zjistit, které aplikace mají v uzlu nasazený kód.</span><span class="sxs-lookup"><span data-stu-id="5470a-164">For a given node, you can inspect which applications have code deployed on that node.</span></span>

![Service Fabric Explorer][service-fabric-explorer]

### <a name="connect-to-the-cluster-using-powershell"></a><span data-ttu-id="5470a-166">Připojení ke clusteru pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="5470a-166">Connect to the cluster using PowerShell</span></span>
<span data-ttu-id="5470a-167">Připojte se pomocí PowerShellu a ověřte, že cluster je spuštěný.</span><span class="sxs-lookup"><span data-stu-id="5470a-167">Verify that the cluster is running by connecting using PowerShell.</span></span>  <span data-ttu-id="5470a-168">Modul PowerShell ServiceFabric se instaluje spolu se sadou [Service Fabric SDK](service-fabric-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="5470a-168">The ServiceFabric PowerShell module is installed with the [Service Fabric SDK](service-fabric-get-started.md).</span></span>  <span data-ttu-id="5470a-169">Rutina [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) vytvoří připojení ke clusteru.</span><span class="sxs-lookup"><span data-stu-id="5470a-169">The [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet establishes a connection to the cluster.</span></span>   

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint quickstartcluster.westus2.cloudapp.azure.com:19000
```
<span data-ttu-id="5470a-170">Další příklady připojení ke clusteru najdete v článku [Připojení k zabezpečenému clusteru](service-fabric-connect-to-secure-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="5470a-170">See [Connect to a secure cluster](service-fabric-connect-to-secure-cluster.md) for other examples of connecting to a cluster.</span></span> <span data-ttu-id="5470a-171">Po připojení ke clusteru zobrazte pomocí rutiny [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) seznam uzlů v clusteru a stavové informace pro každý uzel.</span><span class="sxs-lookup"><span data-stu-id="5470a-171">After connecting to the cluster, use the [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) cmdlet to display a list of nodes in the cluster and status information for each node.</span></span> <span data-ttu-id="5470a-172">Položka **HealthState** by měla mít pro každý uzel hodnotu *OK*.</span><span class="sxs-lookup"><span data-stu-id="5470a-172">**HealthState** should be *OK* for each node.</span></span>

```powershell
PS C:\Users\sfuser> Get-ServiceFabricNode |Format-Table

NodeDeactivationInfo NodeName     IpAddressOrFQDN NodeType  CodeVersion  ConfigVersion NodeStatus NodeUpTime NodeDownTime HealthState
-------------------- --------     --------------- --------  -----------  ------------- ---------- ---------- ------------ -----------
                     _nodetype1_2 10.0.0.6        nodetype1 5.7.198.9494 1                     Up 03:00:38   00:00:00              Ok
                     _nodetype1_1 10.0.0.5        nodetype1 5.7.198.9494 1                     Up 03:00:38   00:00:00              Ok
                     _nodetype1_0 10.0.0.4        nodetype1 5.7.198.9494 1                     Up 03:00:38   00:00:00              Ok
                     _nodetype1_4 10.0.0.8        nodetype1 5.7.198.9494 1                     Up 03:00:38   00:00:00              Ok
                     _nodetype1_3 10.0.0.7        nodetype1 5.7.198.9494 1                     Up 03:00:38   00:00:00              Ok
```

### <a name="remove-the-cluster"></a><span data-ttu-id="5470a-173">Odebrání clusteru</span><span class="sxs-lookup"><span data-stu-id="5470a-173">Remove the cluster</span></span>
<span data-ttu-id="5470a-174">Cluster Service Fabric se kromě vlastního prostředku clusteru skládá z dalších prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="5470a-174">A Service Fabric cluster is made up of other Azure resources in addition to the cluster resource itself.</span></span> <span data-ttu-id="5470a-175">Proto je ke kompletnímu odstranění clusteru Service Fabric potřeba odstranit taky prostředky, které ho tvoří.</span><span class="sxs-lookup"><span data-stu-id="5470a-175">So to completely delete a Service Fabric cluster you also need to delete all the resources it is made of.</span></span> <span data-ttu-id="5470a-176">Nejjednodušší způsob, jak odstranit cluster a všechny prostředky, které využívá, je odstranit příslušnou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="5470a-176">The simplest way to delete the cluster and all the resources it consumes is to delete the resource group.</span></span> <span data-ttu-id="5470a-177">Další způsoby odstranění clusteru nebo některých (ale ne všech) prostředků ve skupině prostředků najdete v tématu [Odstranění clusteru](service-fabric-cluster-delete.md)</span><span class="sxs-lookup"><span data-stu-id="5470a-177">For other ways to delete a cluster or to delete some (but not all) the resources in a resource group, see [Delete a cluster](service-fabric-cluster-delete.md)</span></span>

<span data-ttu-id="5470a-178">Odstranění skupiny prostředků na webu Azure Portal:</span><span class="sxs-lookup"><span data-stu-id="5470a-178">Delete a resource group in the Azure portal:</span></span>
1. <span data-ttu-id="5470a-179">Přejděte ke clusteru Service Fabric, který chcete odstranit.</span><span class="sxs-lookup"><span data-stu-id="5470a-179">Navigate to the Service Fabric cluster you want to delete.</span></span>
2. <span data-ttu-id="5470a-180">Na stránce základů clusteru klikněte na název **skupiny prostředků**.</span><span class="sxs-lookup"><span data-stu-id="5470a-180">Click the **Resource Group** name on the cluster essentials page.</span></span>
3. <span data-ttu-id="5470a-181">Na stránce **Základy skupiny prostředků** klikněte na **Odstranit skupinu prostředků**, postupujte podle pokynů na této stránce a dokončete odstranění skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="5470a-181">In the **Resource Group Essentials** page, click **Delete resource group** and follow the instructions on that page to complete the deletion of the resource group.</span></span>
    <span data-ttu-id="5470a-182">![Odstranění skupiny prostředků][cluster-delete]</span><span class="sxs-lookup"><span data-stu-id="5470a-182">![Delete the resource group][cluster-delete]</span></span>


## <a name="use-azure-powershell-to-deploy-a-secure-cluster"></a><span data-ttu-id="5470a-183">Nasazení zabezpečeného clusteru pomocí Azure PowerShellu</span><span class="sxs-lookup"><span data-stu-id="5470a-183">Use Azure Powershell to deploy a secure cluster</span></span>
1. <span data-ttu-id="5470a-184">Stáhněte si na počítač [modul Azure PowerShell verze 4.0 nebo vyšší](https://docs.microsoft.com/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="5470a-184">Download the [Azure Powershell module version 4.0 or higher](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) on your machine.</span></span>

2. <span data-ttu-id="5470a-185">Otevřete okno Windows PowerShellu a spusťte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="5470a-185">Open a Windows PowerShell window, Run the following command.</span></span> 
    
    ```powershell

    Get-Command -Module AzureRM.ServiceFabric 
    ```

    <span data-ttu-id="5470a-186">Zobrazený výstup by měl vypadat přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="5470a-186">You should see an output similar to the following.</span></span>

    ![ps-list][ps-list]

3. <span data-ttu-id="5470a-188">Přihlaste se k Azure a vyberte předplatné, pro které chcete vytvořit cluster.</span><span class="sxs-lookup"><span data-stu-id="5470a-188">Login to Azure and Select the subscription to which you want to create the cluster</span></span>

    ```powershell

    Login-AzureRmAccount

    Select-AzureRmSubscription -SubscriptionId "Subcription ID" 
    ```

4. <span data-ttu-id="5470a-189">Nyní vytvořte zabezpečený cluster spuštěním následujícího příkazu.</span><span class="sxs-lookup"><span data-stu-id="5470a-189">Run the following command to now create a secure cluster.</span></span> <span data-ttu-id="5470a-190">Nezapomeňte upravit parametry.</span><span class="sxs-lookup"><span data-stu-id="5470a-190">Do not forget to customize the parameters.</span></span> 

    ```powershell
    $certpwd="Password#1234" | ConvertTo-SecureString -AsPlainText -Force
    $RDPpwd="Password#1234" | ConvertTo-SecureString -AsPlainText -Force 
    $RDPuser="vmadmin"
    $RGname="mycluster" # this is also the name of your cluster
    $clusterloc="SouthCentralUS"
    $subname="$RGname.$clusterloc.cloudapp.azure.com"
    $certfolder="c:\mycertificates\"
    $clustersize=1 # can take values 1, 3-99

    New-AzureRmServiceFabricCluster -ResourceGroupName $RGname -Location $clusterloc -ClusterSize $clustersize -VmUserName $RDPuser -VmPassword $RDPpwd -CertificateSubjectName $subname -CertificatePassword $certpwd -CertificateOutputFolder $certfolder
    ```

    <span data-ttu-id="5470a-191">Dokončení příkazu může trvat 10–30 minut a pak by se měl zobrazit výstup podobný následujícímu.</span><span class="sxs-lookup"><span data-stu-id="5470a-191">The command can take anywhere from 10 minutes to 30 minutes to complete, at the end of it, you should get an output similar to the following.</span></span> <span data-ttu-id="5470a-192">Výstup obsahuje informace o certifikátu, službě KeyVault, do které se nahrál, a místní složce, do které se certifikát zkopíroval.</span><span class="sxs-lookup"><span data-stu-id="5470a-192">The output has information about the certificate, the KeyVault where it was uploaded to, and the local folder where the certificate is copied.</span></span> 

    ![ps-out][ps-out]

5. <span data-ttu-id="5470a-194">Celý výstup zkopírujte a uložte do textového souboru, protože se k němu budeme odkazovat.</span><span class="sxs-lookup"><span data-stu-id="5470a-194">Copy the entire output and save to a text file as we need to refer to it.</span></span> <span data-ttu-id="5470a-195">Z výstupu si poznamenejte následující informace.</span><span class="sxs-lookup"><span data-stu-id="5470a-195">Make a note of the following information from the output.</span></span> 

    - <span data-ttu-id="5470a-196">**CertificateSavedLocalPath** : c:\mycertificates\mycluster20170504141137.pfx</span><span class="sxs-lookup"><span data-stu-id="5470a-196">**CertificateSavedLocalPath** : c:\mycertificates\mycluster20170504141137.pfx</span></span>
    - <span data-ttu-id="5470a-197">**CertificateThumbprint** : C4C1E541AD512B8065280292A8BA6079C3F26F10</span><span class="sxs-lookup"><span data-stu-id="5470a-197">**CertificateThumbprint** : C4C1E541AD512B8065280292A8BA6079C3F26F10</span></span>
    - <span data-ttu-id="5470a-198">**ManagementEndpoint** : https://mycluster.southcentralus.cloudapp.azure.com:19080</span><span class="sxs-lookup"><span data-stu-id="5470a-198">**ManagementEndpoint** : https://mycluster.southcentralus.cloudapp.azure.com:19080</span></span>
    - <span data-ttu-id="5470a-199">**ClientConnectionEndpointPort** : 19000</span><span class="sxs-lookup"><span data-stu-id="5470a-199">**ClientConnectionEndpointPort** : 19000</span></span>

### <a name="install-the-certificate-on-your-local-machine"></a><span data-ttu-id="5470a-200">Instalace certifikátu na místním počítači</span><span class="sxs-lookup"><span data-stu-id="5470a-200">Install the certificate on your local machine</span></span>
  
<span data-ttu-id="5470a-201">Aby bylo možné připojení ke clusteru, je potřeba nainstalovat certifikát do osobního úložiště (Moje) aktuálního uživatele.</span><span class="sxs-lookup"><span data-stu-id="5470a-201">To connect to the cluster, you need to install the certificate into the Personal (My) store of the current user.</span></span> 

<span data-ttu-id="5470a-202">Spusťte následující příkaz PowerShellu:</span><span class="sxs-lookup"><span data-stu-id="5470a-202">Run the following PowerShell</span></span>

```powershell
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My `
        -FilePath C:\mycertificates\the name of the cert.pfx `
        -Password (ConvertTo-SecureString -String certpwd -AsPlainText -Force)
```

<span data-ttu-id="5470a-203">Nyní jste připraveni připojit se ke svému zabezpečenému clusteru.</span><span class="sxs-lookup"><span data-stu-id="5470a-203">You are now ready to connect to your secure cluster.</span></span>

### <a name="connect-to-a-secure-cluster"></a><span data-ttu-id="5470a-204">Připojení k zabezpečenému clusteru</span><span class="sxs-lookup"><span data-stu-id="5470a-204">Connect to a secure cluster</span></span> 

<span data-ttu-id="5470a-205">Spuštěním následujícího příkazu PowerShellu se připojte k zabezpečenému clusteru.</span><span class="sxs-lookup"><span data-stu-id="5470a-205">Run the following PowerShell command to connect to a secure cluster.</span></span> <span data-ttu-id="5470a-206">Podrobnosti o certifikátu se musí shodovat s certifikátem použitým k nastavení clusteru.</span><span class="sxs-lookup"><span data-stu-id="5470a-206">The certificate details must match a certificate that was used to set up the cluster.</span></span> 

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint <Certificate Thumbprint> `
          -FindType FindByThumbprint -FindValue <Certificate Thumbprint> `
          -StoreLocation CurrentUser -StoreName My
```


<span data-ttu-id="5470a-207">Následující příklad ukazuje vyplněné parametry:</span><span class="sxs-lookup"><span data-stu-id="5470a-207">The following example shows the completed parameters:</span></span> 

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint mycluster.southcentralus.cloudapp.azure.com:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -FindType FindByThumbprint -FindValue C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -StoreLocation CurrentUser -StoreName My
```

<span data-ttu-id="5470a-208">Spuštěním následujícího příkazu zkontrolujte, že jste připojeni a cluster je v pořádku.</span><span class="sxs-lookup"><span data-stu-id="5470a-208">Run the following command to check that you are connected and the cluster is healthy.</span></span>

```powershell

Get-ServiceFabricClusterHealth

```
### <a name="publish-your-apps-to-your-cluster-from-visual-studio"></a><span data-ttu-id="5470a-209">Publikování aplikací do clusteru ze sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5470a-209">Publish your apps to your cluster from Visual Studio</span></span>

<span data-ttu-id="5470a-210">Když teď máte nastavený cluster Azure, můžete do něj publikovat aplikace ze sady Visual Studio pomocí postupů v dokumentu [Publikování do clusteru](service-fabric-publish-app-remote-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="5470a-210">Now that you have set up an Azure cluster, you can publish your applications to it from Visual Studio by following the [Publish to an cluster](service-fabric-publish-app-remote-cluster.md) document.</span></span> 

### <a name="remove-the-cluster"></a><span data-ttu-id="5470a-211">Odebrání clusteru</span><span class="sxs-lookup"><span data-stu-id="5470a-211">Remove the cluster</span></span>
<span data-ttu-id="5470a-212">Cluster se kromě vlastního prostředku clusteru skládá z dalších prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="5470a-212">A cluster is made up of other Azure resources in addition to the cluster resource itself.</span></span> <span data-ttu-id="5470a-213">Nejjednodušší způsob, jak odstranit cluster a všechny prostředky, které využívá, je odstranit příslušnou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="5470a-213">The simplest way to delete the cluster and all the resources it consumes is to delete the resource group.</span></span> 

```powershell

Remove-AzureRmResourceGroup -Name $RGname -Force

```

## <a name="next-steps"></a><span data-ttu-id="5470a-214">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5470a-214">Next steps</span></span>
<span data-ttu-id="5470a-215">Teď, když jste nastavili vývojový cluster, zkuste provést následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5470a-215">Now that you have set up a development cluster, try the following:</span></span>
* [<span data-ttu-id="5470a-216">Vytvoření zabezpečeného clusteru na portálu</span><span class="sxs-lookup"><span data-stu-id="5470a-216">Create a secure cluster in the portal</span></span>](service-fabric-cluster-creation-via-portal.md)
* [<span data-ttu-id="5470a-217">Vytvoření clusteru ze šablony</span><span class="sxs-lookup"><span data-stu-id="5470a-217">Create a cluster from a template</span></span>](service-fabric-cluster-creation-via-arm.md) 
* [<span data-ttu-id="5470a-218">Nasazení aplikací s použitím prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="5470a-218">Deploy apps using PowerShell</span></span>](service-fabric-deploy-remove-applications.md)


[cluster-setup-basics]: ./media/service-fabric-get-started-azure-cluster/basics.png
[node-type-config]: ./media/service-fabric-get-started-azure-cluster/nodetypeconfig.png
[cluster-status]: ./media/service-fabric-get-started-azure-cluster/clusterstatus.png
[service-fabric-explorer]: ./media/service-fabric-get-started-azure-cluster/sfx.png
[cluster-delete]: ./media/service-fabric-get-started-azure-cluster/delete.png
[ps-list]: ./media/service-fabric-get-started-azure-cluster/pslist.PNG
[ps-out]: ./media/service-fabric-get-started-azure-cluster/psout.PNG
