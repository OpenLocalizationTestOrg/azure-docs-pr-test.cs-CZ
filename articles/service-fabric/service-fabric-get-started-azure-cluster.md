---
title: "aaaSet do clusteru služby Azure Service Fabric | Microsoft Docs"
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
ms.openlocfilehash: 13c60e293d19d607bb41ee4859706508c219a833
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-service-fabric-cluster-on-azure"></a><span data-ttu-id="f891a-103">Vytvoření vašeho prvního clusteru Service Fabric v Azure</span><span class="sxs-lookup"><span data-stu-id="f891a-103">Create your first Service Fabric cluster on Azure</span></span>
<span data-ttu-id="f891a-104">[Cluster Service Fabric](service-fabric-deploy-anywhere.md) je síťově propojená sada virtuálních nebo fyzických počítačů, ve které se nasazují a spravují mikroslužby.</span><span class="sxs-lookup"><span data-stu-id="f891a-104">A [Service Fabric cluster](service-fabric-deploy-anywhere.md) is a network-connected set of virtual or physical machines into which your microservices are deployed and managed.</span></span> <span data-ttu-id="f891a-105">Tento rychlý start vám pomůže toocreate pěti uzly clusteru, se systémem Windows nebo Linux, prostřednictvím hello [prostředí Azure PowerShell](https://msdn.microsoft.com/library/dn135248) nebo [portál Azure](http://portal.azure.com) za několik minut.</span><span class="sxs-lookup"><span data-stu-id="f891a-105">This quickstart helps you toocreate a five-node cluster, running on either Windows or Linux, through hello [Azure PowerShell](https://msdn.microsoft.com/library/dn135248) or [Azure portal](http://portal.azure.com) in just a few minutes.</span></span>  

<span data-ttu-id="f891a-106">Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.</span><span class="sxs-lookup"><span data-stu-id="f891a-106">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>


## <a name="use-hello-azure-portal"></a><span data-ttu-id="f891a-107">Hello použití portálu Azure</span><span class="sxs-lookup"><span data-stu-id="f891a-107">Use hello Azure portal</span></span>

<span data-ttu-id="f891a-108">Přihlášení toohello portál Azure na [http://portal.azure.com](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="f891a-108">Log in toohello Azure portal at [http://portal.azure.com](http://portal.azure.com).</span></span>

### <a name="create-hello-cluster"></a><span data-ttu-id="f891a-109">Vytvoření clusteru hello</span><span class="sxs-lookup"><span data-stu-id="f891a-109">Create hello cluster</span></span>

1. <span data-ttu-id="f891a-110">Klikněte na tlačítko hello **nový** nalezeno tlačítko na hello levém horním rohu hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="f891a-110">Click hello **New** button found on hello upper left-hand corner of hello Azure portal.</span></span>
2. <span data-ttu-id="f891a-111">Vyberte **výpočetní** z hello **nový** okna a potom vyberte **Service Fabric Cluster** z hello **výpočetní** okno.</span><span class="sxs-lookup"><span data-stu-id="f891a-111">Select **Compute** from hello **New** blade and then select **Service Fabric Cluster** from hello **Compute** blade.</span></span>
3. <span data-ttu-id="f891a-112">Vyplňte hello Service Fabric **Základy** formuláře.</span><span class="sxs-lookup"><span data-stu-id="f891a-112">Fill out hello Service Fabric **Basics** form.</span></span> <span data-ttu-id="f891a-113">Pro **operačního systému**vyberte hello verzi systému Windows nebo Linux, které chcete hello toorun uzly clusteru.</span><span class="sxs-lookup"><span data-stu-id="f891a-113">For **Operating system**, select hello version of Windows or Linux you want hello cluster nodes toorun.</span></span> <span data-ttu-id="f891a-114">Hello uživatelské jméno a heslo zadané v tomto poli je použité toolog toohello virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="f891a-114">hello user name and password entered here is used toolog in toohello virtual machine.</span></span> <span data-ttu-id="f891a-115">V části **Skupina prostředků** vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="f891a-115">For **Resource group**, create a new one.</span></span> <span data-ttu-id="f891a-116">Skupina prostředků je logický kontejner, ve kterém se vytváří a hromadně spravují prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="f891a-116">A resource group is a logical container into which Azure resources are created and collectively managed.</span></span> <span data-ttu-id="f891a-117">Jakmile budete hotovi, klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="f891a-117">When complete, click **OK**.</span></span>

    ![Výstup po instalaci clusteru][cluster-setup-basics]

4. <span data-ttu-id="f891a-119">Vyplňte hello **konfigurace clusteru** formuláře.</span><span class="sxs-lookup"><span data-stu-id="f891a-119">Fill out hello **Cluster configuration** form.</span></span>  <span data-ttu-id="f891a-120">Jako **Počet typů uzlu** zadejte 1.</span><span class="sxs-lookup"><span data-stu-id="f891a-120">For **Node type count**, enter "1".</span></span>

5. <span data-ttu-id="f891a-121">Vyberte **typ uzlu 1 (primární)** a vyplňte hello **konfiguraci typu uzlu** formuláře.</span><span class="sxs-lookup"><span data-stu-id="f891a-121">Select **Node type 1 (Primary)** and fill out hello **Node type configuration** form.</span></span>  <span data-ttu-id="f891a-122">Zadejte název typu uzlu a nastavte hello [odolnost vrstvy](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster) příliš "Bronzovým."</span><span class="sxs-lookup"><span data-stu-id="f891a-122">Enter a node type name and set hello [Durability tier](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster) too"Bronze."</span></span>  <span data-ttu-id="f891a-123">Vyberte velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="f891a-123">Select a VM size.</span></span>

    <span data-ttu-id="f891a-124">Typy uzlů definovat hello velikost virtuálního počítače, počet virtuálních počítačů, vlastní koncové body, a další nastavení pro hello virtuálních počítačů daného typu.</span><span class="sxs-lookup"><span data-stu-id="f891a-124">Node types define hello VM size, number of VMs, custom endpoints, and other settings for hello VMs of that type.</span></span> <span data-ttu-id="f891a-125">Každý typ uzlu definována je nastavený jako sada škálování samostatný virtuální počítač, který je použité toodeploy a spravovaných virtuálních počítačích jako sada.</span><span class="sxs-lookup"><span data-stu-id="f891a-125">Each node type defined is set up as a separate virtual machine scale set, which is used toodeploy and managed virtual machines as a set.</span></span> <span data-ttu-id="f891a-126">U každého typu uzlu je možné nezávisle vertikálně navýšit nebo snížit kapacitu. Mají různé sady otevřených portů a můžou mít různé metriky kapacity.</span><span class="sxs-lookup"><span data-stu-id="f891a-126">Each node type can be scaled up or down independently, have different sets of ports open, and can have different capacity metrics.</span></span>  <span data-ttu-id="f891a-127">Hello první nebo primární, typ uzlu Určuje, kde jsou hostované Service Fabric systémových služeb a musí mít pět nebo více virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="f891a-127">hello first, or primary, node type is where Service Fabric system services are hosted and must have five or more VMs.</span></span>

    <span data-ttu-id="f891a-128">Důležitým krokem každého produkčního nasazení je [plánování kapacity](service-fabric-cluster-capacity.md).</span><span class="sxs-lookup"><span data-stu-id="f891a-128">For any production deployment, [capacity planning](service-fabric-cluster-capacity.md) is an important step.</span></span>  <span data-ttu-id="f891a-129">V tomto rychlém zprovoznění ale nespouštíme aplikace, takže jako velikost virtuálního počítače vyberte *DS1_v2 Standard*.</span><span class="sxs-lookup"><span data-stu-id="f891a-129">For this quick start, however, you aren't running applications so select a *DS1_v2 Standard* VM size.</span></span>  <span data-ttu-id="f891a-130">Vyberte "Stříbrná" pro hello [úroveň spolehlivosti](service-fabric-cluster-capacity.md#the-reliability-characteristics-of-the-cluster) a počáteční škálovací sady virtuálních počítačů kapacitu 5.</span><span class="sxs-lookup"><span data-stu-id="f891a-130">Select "Silver" for hello [reliability tier](service-fabric-cluster-capacity.md#the-reliability-characteristics-of-the-cluster) and an initial virtual machine scale set capacity of 5.</span></span>  

    <span data-ttu-id="f891a-131">Vlastní koncové body otevře porty nástroji pro vyrovnávání zatížení Azure hello tak, aby můžete propojit s aplikací, které běží na clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="f891a-131">Custom endpoints open up ports in hello Azure load balancer so that you can connect with applications running on hello cluster.</span></span>  <span data-ttu-id="f891a-132">Zadejte "80, 8172" tooopen až porty 80 a 8172.</span><span class="sxs-lookup"><span data-stu-id="f891a-132">Enter "80, 8172" tooopen up ports 80 and 8172.</span></span>

    <span data-ttu-id="f891a-133">Neprovádět kontrolu hello **konfigurovat upřesňující nastavení** pole, která se používá k přizpůsobení koncových bodů správy TCP nebo HTTP, rozsahy portů aplikace, [omezení umístění](service-fabric-cluster-resource-manager-configure-services.md#placement-constraints), a [kapacity vlastnosti](service-fabric-cluster-resource-manager-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="f891a-133">Do not check hello **Configure advanced settings** box, which is used for customizing TCP/HTTP management endpoints, application port ranges, [placement constraints](service-fabric-cluster-resource-manager-configure-services.md#placement-constraints), and [capacity properties](service-fabric-cluster-resource-manager-metrics.md).</span></span>    

    <span data-ttu-id="f891a-134">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="f891a-134">Select **OK**.</span></span>

6. <span data-ttu-id="f891a-135">V hello **konfigurace clusteru** formuláři, nastavte **diagnostiky** příliš**na**.</span><span class="sxs-lookup"><span data-stu-id="f891a-135">In hello **Cluster configuration** form, set **Diagnostics** too**On**.</span></span>  <span data-ttu-id="f891a-136">Pro tento rychlý start, není nutné tooenter všechny [nastavení prostředků infrastruktury](service-fabric-cluster-fabric-settings.md) vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="f891a-136">For this quickstart, you do not need tooenter any [fabric setting](service-fabric-cluster-fabric-settings.md) properties.</span></span>  <span data-ttu-id="f891a-137">V **verze Fabric**, vyberte **automatické** upgrade režimu tak, aby Microsoft automaticky aktualizuje hello verze kódu fabric hello systémem hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="f891a-137">In **Fabric version**, select **Automatic** upgrade mode so that Microsoft automatically updates hello version of hello fabric code running hello cluster.</span></span>  <span data-ttu-id="f891a-138">Nastavit režim hello příliš**ruční** příliš Chcete-li[zvolte na podporovanou verzi](service-fabric-cluster-upgrade.md) tooupgrade k.</span><span class="sxs-lookup"><span data-stu-id="f891a-138">Set hello mode too**Manual** if you want too[choose a supported version](service-fabric-cluster-upgrade.md) tooupgrade to.</span></span> 

    ![Konfigurace typu uzlu][node-type-config]

    <span data-ttu-id="f891a-140">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="f891a-140">Select **OK**.</span></span>

7. <span data-ttu-id="f891a-141">Vyplňte hello **zabezpečení** formuláře.</span><span class="sxs-lookup"><span data-stu-id="f891a-141">Fill out hello **Security** form.</span></span>  <span data-ttu-id="f891a-142">Pro toto rychlé zprovoznění vyberte **Nezabezpečené**.</span><span class="sxs-lookup"><span data-stu-id="f891a-142">For this quick start select **Unsecure**.</span></span>  <span data-ttu-id="f891a-143">Vysoce je doporučeno toocreate zabezpečení clusteru pro úlohy v produkčním prostředí, ale vzhledem k tomu, že každý, kdo anonymně připojení tooan nezabezpečená clusteru a provádět operace správy.</span><span class="sxs-lookup"><span data-stu-id="f891a-143">It is highly recommended toocreate a secure cluster for production workloads, however, since anyone can anonymously connect tooan unsecure cluster and perform management operations.</span></span>  

    <span data-ttu-id="f891a-144">Certifikáty se používají v Service Fabric tooprovide ověřování a šifrování toosecure různé aspekty cluster a jeho aplikace.</span><span class="sxs-lookup"><span data-stu-id="f891a-144">Certificates are used in Service Fabric tooprovide authentication and encryption toosecure various aspects of a cluster and its applications.</span></span> <span data-ttu-id="f891a-145">Další informace o použití certifikátů ve službě Service Fabric najdete v tématu věnovaném [scénářům zabezpečení clusteru Service Fabric](service-fabric-cluster-security.md).</span><span class="sxs-lookup"><span data-stu-id="f891a-145">For more information on how certificates are used in Service Fabric, see [Service Fabric cluster security scenarios](service-fabric-cluster-security.md).</span></span>  <span data-ttu-id="f891a-146">tooenable ověřování uživatelů pomocí služby Azure Active Directory nebo tooset se certifikáty pro zabezpečení aplikací [vytvoření clusteru z šablony Resource Manageru](service-fabric-cluster-creation-via-arm.md).</span><span class="sxs-lookup"><span data-stu-id="f891a-146">tooenable user authentication using Azure Active Directory or tooset up certificates for application security, [create a cluster from a Resource Manager template](service-fabric-cluster-creation-via-arm.md).</span></span>

    <span data-ttu-id="f891a-147">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="f891a-147">Select **OK**.</span></span>

8. <span data-ttu-id="f891a-148">Hello Zkontrolujte souhrn.</span><span class="sxs-lookup"><span data-stu-id="f891a-148">Review hello summary.</span></span>  <span data-ttu-id="f891a-149">Pokud chcete toodownload šablony Resource Manageru z nastavení hello jste zadali, vyberte **stáhnout šablonu a parametry**.</span><span class="sxs-lookup"><span data-stu-id="f891a-149">If you'd like toodownload a Resource Manager template built from hello settings you entered, select **Download template and parameters**.</span></span>  <span data-ttu-id="f891a-150">Vyberte **vytvořit** toocreate hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="f891a-150">Select **Create** toocreate hello cluster.</span></span>

    <span data-ttu-id="f891a-151">Zobrazí se průběh vytváření hello v oznámeních hello.</span><span class="sxs-lookup"><span data-stu-id="f891a-151">You can see hello creation progress in hello notifications.</span></span> <span data-ttu-id="f891a-152">(Klikněte na ikonu zvonku"hello" v blízkosti hello stavového řádku v hello pravé horní části obrazovky.) Pokud jste klikli na **Pin tooStartboard** při vytváření clusteru hello, uvidíte **nasazení Cluster Service Fabric** připnutý toohello **spustit** panelu.</span><span class="sxs-lookup"><span data-stu-id="f891a-152">(Click hello "Bell" icon near hello status bar at hello upper right of your screen.) If you clicked **Pin tooStartboard** while creating hello cluster, you see **Deploying Service Fabric Cluster** pinned toohello **Start** board.</span></span>

### <a name="view-cluster-status"></a><span data-ttu-id="f891a-153">Zobrazení stavu clusteru</span><span class="sxs-lookup"><span data-stu-id="f891a-153">View cluster status</span></span>
<span data-ttu-id="f891a-154">Po vytvoření clusteru si můžete prohlédnout cluster v hello **přehled** okna portálu hello.</span><span class="sxs-lookup"><span data-stu-id="f891a-154">Once your cluster is created, you can inspect your cluster in hello **Overview** blade in hello portal.</span></span> <span data-ttu-id="f891a-155">Zobrazí podrobnosti o hello clusteru na řídicím panelu hello, včetně veřejný koncový bod clusteru hello a odkaz tooService Fabric Exploreru.</span><span class="sxs-lookup"><span data-stu-id="f891a-155">You can now see hello details of your cluster in hello dashboard, including hello cluster's public endpoint and a link tooService Fabric Explorer.</span></span>

![Stav clusteru][cluster-status]

### <a name="visualize-hello-cluster-using-service-fabric-explorer"></a><span data-ttu-id="f891a-157">Vizualizace hello clusteru pomocí Service Fabric Exploreru</span><span class="sxs-lookup"><span data-stu-id="f891a-157">Visualize hello cluster using Service Fabric explorer</span></span>
<span data-ttu-id="f891a-158">[Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) je nástroj vhodný pro vizualizaci clusteru a správu aplikací.</span><span class="sxs-lookup"><span data-stu-id="f891a-158">[Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) is a good tool for visualizing your cluster and managing applications.</span></span>  <span data-ttu-id="f891a-159">Service Fabric Explorer je služba, která běží v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="f891a-159">Service Fabric Explorer is a service that runs in hello cluster.</span></span>  <span data-ttu-id="f891a-160">Přístup pomocí webového prohlížeče kliknutím hello **Service Fabric Explorer** odkaz hello clusteru **přehled** stránku hello portálu.</span><span class="sxs-lookup"><span data-stu-id="f891a-160">Access it using a web browser by clicking hello **Service Fabric Explorer** link of hello cluster **Overview** page in hello portal.</span></span>  <span data-ttu-id="f891a-161">Můžete také zadat adresu hello přímo do prohlížeče hello: [http://quickstartcluster.westus.cloudapp.azure.com:19080/Explorer](http://quickstartcluster.westus.cloudapp.azure.com:19080/Explorer)</span><span class="sxs-lookup"><span data-stu-id="f891a-161">You can also enter hello address directly into hello browser: [http://quickstartcluster.westus.cloudapp.azure.com:19080/Explorer](http://quickstartcluster.westus.cloudapp.azure.com:19080/Explorer)</span></span>

<span data-ttu-id="f891a-162">řídicí panel clusteru Hello obsahuje přehled clusteru, včetně shrnutí stavu uzlu a aplikace.</span><span class="sxs-lookup"><span data-stu-id="f891a-162">hello cluster dashboard provides an overview of your cluster, including a summary of application and node health.</span></span> <span data-ttu-id="f891a-163">zobrazení uzlu Hello zobrazuje fyzické rozložení hello hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="f891a-163">hello node view shows hello physical layout of hello cluster.</span></span> <span data-ttu-id="f891a-164">Pro daný uzel můžete zjistit, které aplikace mají v uzlu nasazený kód.</span><span class="sxs-lookup"><span data-stu-id="f891a-164">For a given node, you can inspect which applications have code deployed on that node.</span></span>

![Service Fabric Explorer][service-fabric-explorer]

### <a name="connect-toohello-cluster-using-powershell"></a><span data-ttu-id="f891a-166">Připojte toohello cluster pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="f891a-166">Connect toohello cluster using PowerShell</span></span>
<span data-ttu-id="f891a-167">Ověřte, zda je že daný cluster hello běží se připojíte pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="f891a-167">Verify that hello cluster is running by connecting using PowerShell.</span></span>  <span data-ttu-id="f891a-168">Hello ServiceFabric je nainstalován modul PowerShell s hello [Service Fabric SDK](service-fabric-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="f891a-168">hello ServiceFabric PowerShell module is installed with hello [Service Fabric SDK](service-fabric-get-started.md).</span></span>  <span data-ttu-id="f891a-169">Hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) rutina vytvoří cluster toohello připojení.</span><span class="sxs-lookup"><span data-stu-id="f891a-169">hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet establishes a connection toohello cluster.</span></span>   

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint quickstartcluster.westus2.cloudapp.azure.com:19000
```
<span data-ttu-id="f891a-170">V tématu [clusteru zabezpečené připojení tooa](service-fabric-connect-to-secure-cluster.md) pro další příklady připojování tooa clusteru.</span><span class="sxs-lookup"><span data-stu-id="f891a-170">See [Connect tooa secure cluster](service-fabric-connect-to-secure-cluster.md) for other examples of connecting tooa cluster.</span></span> <span data-ttu-id="f891a-171">Po připojování toohello clusteru pomocí hello [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) rutiny toodisplay seznam uzlů v clusteru a stavové informace pro každý uzel hello.</span><span class="sxs-lookup"><span data-stu-id="f891a-171">After connecting toohello cluster, use hello [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) cmdlet toodisplay a list of nodes in hello cluster and status information for each node.</span></span> <span data-ttu-id="f891a-172">Položka **HealthState** by měla mít pro každý uzel hodnotu *OK*.</span><span class="sxs-lookup"><span data-stu-id="f891a-172">**HealthState** should be *OK* for each node.</span></span>

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

### <a name="remove-hello-cluster"></a><span data-ttu-id="f891a-173">Odebrat hello cluster</span><span class="sxs-lookup"><span data-stu-id="f891a-173">Remove hello cluster</span></span>
<span data-ttu-id="f891a-174">Cluster Service Fabric se skládá z jiných prostředků Azure kromě toohello prostředek clusteru, sám sebe.</span><span class="sxs-lookup"><span data-stu-id="f891a-174">A Service Fabric cluster is made up of other Azure resources in addition toohello cluster resource itself.</span></span> <span data-ttu-id="f891a-175">Takže toocompletely odstranit cluster Service Fabric je také nutné toodelete všechny prostředky, které se provádí z hello.</span><span class="sxs-lookup"><span data-stu-id="f891a-175">So toocompletely delete a Service Fabric cluster you also need toodelete all hello resources it is made of.</span></span> <span data-ttu-id="f891a-176">Hello nejjednodušší způsob, jak toodelete hello clusteru a všechny prostředky hello, které budou využívat je skupina prostředků toodelete hello.</span><span class="sxs-lookup"><span data-stu-id="f891a-176">hello simplest way toodelete hello cluster and all hello resources it consumes is toodelete hello resource group.</span></span> <span data-ttu-id="f891a-177">Pro jiné způsoby toodelete cluster nebo toodelete některé (ale ne všechny) hello prostředky ve skupině prostředků najdete v části [odstranění clusteru](service-fabric-cluster-delete.md)</span><span class="sxs-lookup"><span data-stu-id="f891a-177">For other ways toodelete a cluster or toodelete some (but not all) hello resources in a resource group, see [Delete a cluster](service-fabric-cluster-delete.md)</span></span>

<span data-ttu-id="f891a-178">Odstranění skupiny prostředků v hello portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="f891a-178">Delete a resource group in hello Azure portal:</span></span>
1. <span data-ttu-id="f891a-179">Přejděte cluster Service Fabric toohello chcete toodelete.</span><span class="sxs-lookup"><span data-stu-id="f891a-179">Navigate toohello Service Fabric cluster you want toodelete.</span></span>
2. <span data-ttu-id="f891a-180">Klikněte na tlačítko hello **skupiny prostředků** názvem na stránce essentials hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="f891a-180">Click hello **Resource Group** name on hello cluster essentials page.</span></span>
3. <span data-ttu-id="f891a-181">V hello **Essentials skupiny prostředků** klikněte na tlačítko **odstranit skupinu prostředků** a postupujte podle pokynů hello na této stránce toocomplete hello odstranění skupiny prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="f891a-181">In hello **Resource Group Essentials** page, click **Delete resource group** and follow hello instructions on that page toocomplete hello deletion of hello resource group.</span></span>
    <span data-ttu-id="f891a-182">![Odstraňte skupinu prostředků hello][cluster-delete]</span><span class="sxs-lookup"><span data-stu-id="f891a-182">![Delete hello resource group][cluster-delete]</span></span>


## <a name="use-azure-powershell-toodeploy-a-secure-cluster"></a><span data-ttu-id="f891a-183">Použití Azure Powershell toodeploy zabezpečení clusteru</span><span class="sxs-lookup"><span data-stu-id="f891a-183">Use Azure Powershell toodeploy a secure cluster</span></span>
1. <span data-ttu-id="f891a-184">Stáhnout hello [prostředí Azure Powershell verze modulu 4.0 nebo vyšší](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) na váš počítač.</span><span class="sxs-lookup"><span data-stu-id="f891a-184">Download hello [Azure Powershell module version 4.0 or higher](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) on your machine.</span></span>

2. <span data-ttu-id="f891a-185">Otevřete okno prostředí Windows PowerShell, hello spusťte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="f891a-185">Open a Windows PowerShell window, Run hello following command.</span></span> 
    
    ```powershell

    Get-Command -Module AzureRM.ServiceFabric 
    ```

    <span data-ttu-id="f891a-186">Měli byste vidět následující toohello podobné výstupu.</span><span class="sxs-lookup"><span data-stu-id="f891a-186">You should see an output similar toohello following.</span></span>

    ![ps-list][ps-list]

3. <span data-ttu-id="f891a-188">Přihlášení tooAzure a vyberte hello předplatné toowhich chcete toocreate hello clusteru</span><span class="sxs-lookup"><span data-stu-id="f891a-188">Login tooAzure and Select hello subscription toowhich you want toocreate hello cluster</span></span>

    ```powershell

    Login-AzureRmAccount

    Select-AzureRmSubscription -SubscriptionId "Subcription ID" 
    ```

4. <span data-ttu-id="f891a-189">Spuštění hello následující příkaz toonow vytvoření clusteru s podporou zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="f891a-189">Run hello following command toonow create a secure cluster.</span></span> <span data-ttu-id="f891a-190">Nevynechali toocustomize hello parametry.</span><span class="sxs-lookup"><span data-stu-id="f891a-190">Do not forget toocustomize hello parameters.</span></span> 

    ```powershell
    $certpwd="Password#1234" | ConvertTo-SecureString -AsPlainText -Force
    $RDPpwd="Password#1234" | ConvertTo-SecureString -AsPlainText -Force 
    $RDPuser="vmadmin"
    $RGname="mycluster" # this is also hello name of your cluster
    $clusterloc="SouthCentralUS"
    $subname="$RGname.$clusterloc.cloudapp.azure.com"
    $certfolder="c:\mycertificates\"
    $clustersize=1 # can take values 1, 3-99

    New-AzureRmServiceFabricCluster -ResourceGroupName $RGname -Location $clusterloc -ClusterSize $clustersize -VmUserName $RDPuser -VmPassword $RDPpwd -CertificateSubjectName $subname -CertificatePassword $certpwd -CertificateOutputFolder $certfolder
    ```

    <span data-ttu-id="f891a-191">příkaz Hello může trvat od toocomplete minut too30 10 minut, na konci hello ho, měli byste obdržet následující toohello podobné výstupu.</span><span class="sxs-lookup"><span data-stu-id="f891a-191">hello command can take anywhere from 10 minutes too30 minutes toocomplete, at hello end of it, you should get an output similar toohello following.</span></span> <span data-ttu-id="f891a-192">výstup Hello má informace o certifikátu hello, hello KeyVault, kde byl odeslán, a hello místní složku, kam je zkopírovali hello certifikátu.</span><span class="sxs-lookup"><span data-stu-id="f891a-192">hello output has information about hello certificate, hello KeyVault where it was uploaded to, and hello local folder where hello certificate is copied.</span></span> 

    ![ps-out][ps-out]

5. <span data-ttu-id="f891a-194">Zkopírujte celou výstupu hello a uložit tooa textového souboru, jako je třeba toorefer tooit.</span><span class="sxs-lookup"><span data-stu-id="f891a-194">Copy hello entire output and save tooa text file as we need toorefer tooit.</span></span> <span data-ttu-id="f891a-195">Poznamenejte si následující informace z výstupu hello hello.</span><span class="sxs-lookup"><span data-stu-id="f891a-195">Make a note of hello following information from hello output.</span></span> 

    - <span data-ttu-id="f891a-196">**CertificateSavedLocalPath** : c:\mycertificates\mycluster20170504141137.pfx</span><span class="sxs-lookup"><span data-stu-id="f891a-196">**CertificateSavedLocalPath** : c:\mycertificates\mycluster20170504141137.pfx</span></span>
    - <span data-ttu-id="f891a-197">**CertificateThumbprint** : C4C1E541AD512B8065280292A8BA6079C3F26F10</span><span class="sxs-lookup"><span data-stu-id="f891a-197">**CertificateThumbprint** : C4C1E541AD512B8065280292A8BA6079C3F26F10</span></span>
    - <span data-ttu-id="f891a-198">**ManagementEndpoint** : https://mycluster.southcentralus.cloudapp.azure.com:19080</span><span class="sxs-lookup"><span data-stu-id="f891a-198">**ManagementEndpoint** : https://mycluster.southcentralus.cloudapp.azure.com:19080</span></span>
    - <span data-ttu-id="f891a-199">**ClientConnectionEndpointPort** : 19000</span><span class="sxs-lookup"><span data-stu-id="f891a-199">**ClientConnectionEndpointPort** : 19000</span></span>

### <a name="install-hello-certificate-on-your-local-machine"></a><span data-ttu-id="f891a-200">Nainstalujte certifikát hello na místním počítači</span><span class="sxs-lookup"><span data-stu-id="f891a-200">Install hello certificate on your local machine</span></span>
  
<span data-ttu-id="f891a-201">tooconnect toohello clusteru, musíte tooinstall hello certifikát do úložiště osobních (My) hello hello aktuálního uživatele.</span><span class="sxs-lookup"><span data-stu-id="f891a-201">tooconnect toohello cluster, you need tooinstall hello certificate into hello Personal (My) store of hello current user.</span></span> 

<span data-ttu-id="f891a-202">Spustit hello následující prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="f891a-202">Run hello following PowerShell</span></span>

```powershell
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My `
        -FilePath C:\mycertificates\hello name of hello cert.pfx `
        -Password (ConvertTo-SecureString -String certpwd -AsPlainText -Force)
```

<span data-ttu-id="f891a-203">Nyní je připraven tooconnect tooyour zabezpečení clusteru.</span><span class="sxs-lookup"><span data-stu-id="f891a-203">You are now ready tooconnect tooyour secure cluster.</span></span>

### <a name="connect-tooa-secure-cluster"></a><span data-ttu-id="f891a-204">Připojte tooa zabezpečené cluster</span><span class="sxs-lookup"><span data-stu-id="f891a-204">Connect tooa secure cluster</span></span> 

<span data-ttu-id="f891a-205">Spustit hello následující příkaz prostředí PowerShell tooconnect tooa zabezpečení clusteru.</span><span class="sxs-lookup"><span data-stu-id="f891a-205">Run hello following PowerShell command tooconnect tooa secure cluster.</span></span> <span data-ttu-id="f891a-206">Podrobnosti o Hello certifikátu musí odpovídat certifikát, který byl použité tooset až hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="f891a-206">hello certificate details must match a certificate that was used tooset up hello cluster.</span></span> 

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint <Certificate Thumbprint> `
          -FindType FindByThumbprint -FindValue <Certificate Thumbprint> `
          -StoreLocation CurrentUser -StoreName My
```


<span data-ttu-id="f891a-207">Následující příklad ukazuje hello Hello dokončil parametry:</span><span class="sxs-lookup"><span data-stu-id="f891a-207">hello following example shows hello completed parameters:</span></span> 

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint mycluster.southcentralus.cloudapp.azure.com:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -FindType FindByThumbprint -FindValue C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -StoreLocation CurrentUser -StoreName My
```

<span data-ttu-id="f891a-208">Spusťte následující příkaz toocheck, že jste připojeni hello a hello clusteru je v pořádku.</span><span class="sxs-lookup"><span data-stu-id="f891a-208">Run hello following command toocheck that you are connected and hello cluster is healthy.</span></span>

```powershell

Get-ServiceFabricClusterHealth

```
### <a name="publish-your-apps-tooyour-cluster-from-visual-studio"></a><span data-ttu-id="f891a-209">Publikování aplikace tooyour clusteru ze sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f891a-209">Publish your apps tooyour cluster from Visual Studio</span></span>

<span data-ttu-id="f891a-210">Teď, když jste nastavili Azure clusteru, můžete publikovat aplikace tooit ze sady Visual Studio pomocí následující hello [publikovat tooan clusteru](service-fabric-publish-app-remote-cluster.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="f891a-210">Now that you have set up an Azure cluster, you can publish your applications tooit from Visual Studio by following hello [Publish tooan cluster](service-fabric-publish-app-remote-cluster.md) document.</span></span> 

### <a name="remove-hello-cluster"></a><span data-ttu-id="f891a-211">Odebrat hello cluster</span><span class="sxs-lookup"><span data-stu-id="f891a-211">Remove hello cluster</span></span>
<span data-ttu-id="f891a-212">Cluster se skládá z jiných prostředků Azure kromě toohello prostředek clusteru, sám sebe.</span><span class="sxs-lookup"><span data-stu-id="f891a-212">A cluster is made up of other Azure resources in addition toohello cluster resource itself.</span></span> <span data-ttu-id="f891a-213">Hello nejjednodušší způsob, jak toodelete hello clusteru a všechny prostředky hello, které budou využívat je skupina prostředků toodelete hello.</span><span class="sxs-lookup"><span data-stu-id="f891a-213">hello simplest way toodelete hello cluster and all hello resources it consumes is toodelete hello resource group.</span></span> 

```powershell

Remove-AzureRmResourceGroup -Name $RGname -Force

```

## <a name="next-steps"></a><span data-ttu-id="f891a-214">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f891a-214">Next steps</span></span>
<span data-ttu-id="f891a-215">Teď, když jste nastavili vývojový cluster, zkuste následující hello:</span><span class="sxs-lookup"><span data-stu-id="f891a-215">Now that you have set up a development cluster, try hello following:</span></span>
* [<span data-ttu-id="f891a-216">Vytvoření clusteru s podporou zabezpečení hello portálu</span><span class="sxs-lookup"><span data-stu-id="f891a-216">Create a secure cluster in hello portal</span></span>](service-fabric-cluster-creation-via-portal.md)
* [<span data-ttu-id="f891a-217">Vytvoření clusteru ze šablony</span><span class="sxs-lookup"><span data-stu-id="f891a-217">Create a cluster from a template</span></span>](service-fabric-cluster-creation-via-arm.md) 
* [<span data-ttu-id="f891a-218">Nasazení aplikací s použitím prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="f891a-218">Deploy apps using PowerShell</span></span>](service-fabric-deploy-remove-applications.md)


[cluster-setup-basics]: ./media/service-fabric-get-started-azure-cluster/basics.png
[node-type-config]: ./media/service-fabric-get-started-azure-cluster/nodetypeconfig.png
[cluster-status]: ./media/service-fabric-get-started-azure-cluster/clusterstatus.png
[service-fabric-explorer]: ./media/service-fabric-get-started-azure-cluster/sfx.png
[cluster-delete]: ./media/service-fabric-get-started-azure-cluster/delete.png
[ps-list]: ./media/service-fabric-get-started-azure-cluster/pslist.PNG
[ps-out]: ./media/service-fabric-get-started-azure-cluster/psout.PNG
