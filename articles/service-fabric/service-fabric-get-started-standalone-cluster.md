---
title: "aaaSet clusteru Azure Service Fabric samostatné | Microsoft Docs"
description: "Vytvoření clusteru s podporou samostatné vývoj s tři uzly systémem hello stejný počítač. Po dokončení této instalace, bude připravená toocreate clusteru s více počítači."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/06/2017
ms.author: dekapur
ms.openlocfilehash: e4d0ea9fc3b8475160bd8ed19fd3716463791cc5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-service-fabric-standalone-cluster"></a><span data-ttu-id="60429-104">Vytvoření vašeho prvního samostatného clusteru Service Fabric</span><span class="sxs-lookup"><span data-stu-id="60429-104">Create your first Service Fabric standalone cluster</span></span>
<span data-ttu-id="60429-105">Můžete vytvořit samostatný cluster Service Fabric na všechny virtuální počítače nebo počítače se systémem Windows Server 2012 R2 nebo Windows Server 2016, místní nebo v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="60429-105">You can create a Service Fabric standalone cluster on any virtual machines or computers running Windows Server 2012 R2 or Windows Server 2016, on-premises or in hello cloud.</span></span> <span data-ttu-id="60429-106">Tento rychlý start vám pomůže toocreate samostatný cluster s podporou vývoj právě za několik minut.</span><span class="sxs-lookup"><span data-stu-id="60429-106">This quickstart helps you toocreate a development standalone cluster in just a few minutes.</span></span>  <span data-ttu-id="60429-107">Po provedení postupu budete mít cluster se třemi uzly, který běží v jednom počítači a do kterého můžete nasazovat aplikace.</span><span class="sxs-lookup"><span data-stu-id="60429-107">When you're finished, you have a three-node cluster running on a single computer that you can deploy apps to.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="60429-108">Než začnete</span><span class="sxs-lookup"><span data-stu-id="60429-108">Before you begin</span></span>
<span data-ttu-id="60429-109">Service Fabric nabízí instalaci balíčku toocreate Service Fabric samostatné clustery.</span><span class="sxs-lookup"><span data-stu-id="60429-109">Service Fabric provides a setup package toocreate Service Fabric standalone clusters.</span></span>  <span data-ttu-id="60429-110">[Stáhnout instalační balíček hello](http://go.microsoft.com/fwlink/?LinkId=730690).</span><span class="sxs-lookup"><span data-stu-id="60429-110">[Download hello setup package](http://go.microsoft.com/fwlink/?LinkId=730690).</span></span>  <span data-ttu-id="60429-111">Rozbalte hello instalace tooa složku balíčku na hello počítač nebo virtuální počítač, kde nastavujete hello vývojový cluster.</span><span class="sxs-lookup"><span data-stu-id="60429-111">Unzip hello setup package tooa folder on hello computer or virtual machine where you are setting up hello development cluster.</span></span>  <span data-ttu-id="60429-112">jsou podrobně popsány Hello obsah instalačního balíčku hello [zde](service-fabric-cluster-standalone-package-contents.md).</span><span class="sxs-lookup"><span data-stu-id="60429-112">hello contents of hello setup package are described in detail [here](service-fabric-cluster-standalone-package-contents.md).</span></span>

<span data-ttu-id="60429-113">Správce clusteru Hello nasazení a konfigurace clusteru hello musí mít oprávnění správce na počítači hello.</span><span class="sxs-lookup"><span data-stu-id="60429-113">hello cluster administrator deploying and configuring hello cluster must have administrator privileges on hello computer.</span></span> <span data-ttu-id="60429-114">Service Fabric nelze nainstalovat na řadič domény.</span><span class="sxs-lookup"><span data-stu-id="60429-114">You cannot install Service Fabric on a domain controller.</span></span>

## <a name="validate-hello-environment"></a><span data-ttu-id="60429-115">Ověření hello prostředí</span><span class="sxs-lookup"><span data-stu-id="60429-115">Validate hello environment</span></span>
<span data-ttu-id="60429-116">Hello *TestConfiguration.ps1* skript v balíčku samostatné hello slouží jako nejlepší toovalidate analyzátor postupy, zda clusteru můžou být nasazené na dané prostředí.</span><span class="sxs-lookup"><span data-stu-id="60429-116">hello *TestConfiguration.ps1* script in hello standalone package is used as a best practices analyzer toovalidate whether a cluster can be deployed on a given environment.</span></span> <span data-ttu-id="60429-117">[Příprava nasazení](service-fabric-cluster-standalone-deployment-preparation.md) seznamy hello požadavky a požadavky na prostředí.</span><span class="sxs-lookup"><span data-stu-id="60429-117">[Deployment preparation](service-fabric-cluster-standalone-deployment-preparation.md) lists hello pre-requisites and environment requirements.</span></span> <span data-ttu-id="60429-118">Spusťte skript tooverify hello, pokud můžete vytvořit cluster vývoj hello:</span><span class="sxs-lookup"><span data-stu-id="60429-118">Run hello script tooverify if you can create hello development cluster:</span></span>

```powershell
.\TestConfiguration.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json
```
## <a name="create-hello-cluster"></a><span data-ttu-id="60429-119">Vytvoření clusteru hello</span><span class="sxs-lookup"><span data-stu-id="60429-119">Create hello cluster</span></span>
<span data-ttu-id="60429-120">Několik ukázkových clusteru konfiguračních souborů jsou nainstalovány s hello instalační balíček.</span><span class="sxs-lookup"><span data-stu-id="60429-120">Several sample cluster configuration files are installed with hello setup package.</span></span> <span data-ttu-id="60429-121">*ClusterConfig.Unsecure.DevCluster.json* je nejjednodušší konfiguraci clusteru hello: nezabezpečená, tři uzly clusteru se systémem na jednom počítači.</span><span class="sxs-lookup"><span data-stu-id="60429-121">*ClusterConfig.Unsecure.DevCluster.json* is hello simplest cluster configuration: an unsecure, three-node cluster running on a single computer.</span></span>  <span data-ttu-id="60429-122">Další konfigurační soubory popisují clustery s jedním nebo více počítači zabezpečené pomocí certifikátů X.509 nebo s použitím zabezpečení systému Windows.</span><span class="sxs-lookup"><span data-stu-id="60429-122">Other config files describe single or multi-machine clusters secured with X.509 certificates or Windows security.</span></span>  <span data-ttu-id="60429-123">Nemáte potřebovat toomodify hello výchozí konfigurace nastavení pro tento kurz, ale projděte hello konfiguračního souboru a seznamte se s hello nastavení.</span><span class="sxs-lookup"><span data-stu-id="60429-123">You don't need toomodify any of hello default config settings for this tutorial, but look through hello config file and get familiar with hello settings.</span></span>  <span data-ttu-id="60429-124">Hello **uzly** část popisuje hello tři uzly v clusteru hello: název, IP adresa, [typ uzlu, doména selhání a upgradu domény](service-fabric-cluster-manifest.md#nodes-on-the-cluster).</span><span class="sxs-lookup"><span data-stu-id="60429-124">hello **nodes** section describes hello three nodes in hello cluster: name, IP address, [node type, fault domain, and upgrade domain](service-fabric-cluster-manifest.md#nodes-on-the-cluster).</span></span>  <span data-ttu-id="60429-125">Hello **vlastnosti** oddíl definuje hello [zabezpečení, úroveň spolehlivosti, diagnostiky kolekce a typy uzlů](service-fabric-cluster-manifest.md#cluster-properties) pro hello cluster.</span><span class="sxs-lookup"><span data-stu-id="60429-125">hello **properties** section defines hello [security, reliability level, diagnostics collection, and types of nodes](service-fabric-cluster-manifest.md#cluster-properties) for hello cluster.</span></span>

<span data-ttu-id="60429-126">Tento cluster není zabezpečený.</span><span class="sxs-lookup"><span data-stu-id="60429-126">This cluster is unsecure.</span></span>  <span data-ttu-id="60429-127">Každý se může anonymně připojit a provádět operace správy, proto by produkční clustery vždy měly být zabezpečené pomocí certifikátů X.509 nebo zabezpečení systému Windows.</span><span class="sxs-lookup"><span data-stu-id="60429-127">Anyone can connect anonymously and perform management operations, so production clusters should always be secured using X.509 certificates or Windows security.</span></span>  <span data-ttu-id="60429-128">Zabezpečení je nakonfigurovaná pouze při vytváření clusteru a není možné tooenable zabezpečení po vytvoření clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="60429-128">Security is only configured at cluster creation time and it is not possible tooenable security after hello cluster is created.</span></span>  <span data-ttu-id="60429-129">Čtení [zabezpečení clusteru](service-fabric-cluster-security.md) toolearn Další informace o zabezpečení clusteru Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="60429-129">Read [Secure a cluster](service-fabric-cluster-security.md) toolearn more about Service Fabric cluster security.</span></span>  

<span data-ttu-id="60429-130">toocreate hello vývoj tři uzly clusteru, spusťte hello *CreateServiceFabricCluster.ps1* skriptu z relace prostředí PowerShell správce:</span><span class="sxs-lookup"><span data-stu-id="60429-130">toocreate hello three-node development cluster, run hello *CreateServiceFabricCluster.ps1* script from an administrator PowerShell session:</span></span>

```powershell
.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json -AcceptEULA
```

<span data-ttu-id="60429-131">balíček modulu runtime Service Fabric Hello je automaticky stáhnout a nainstalovat v době vytváření clusteru.</span><span class="sxs-lookup"><span data-stu-id="60429-131">hello Service Fabric runtime package is automatically downloaded and installed at time of cluster creation.</span></span>

## <a name="connect-toohello-cluster"></a><span data-ttu-id="60429-132">Připojte toohello cluster</span><span class="sxs-lookup"><span data-stu-id="60429-132">Connect toohello cluster</span></span>
<span data-ttu-id="60429-133">Vývojový cluster se třemi uzly je nyní spuštěn.</span><span class="sxs-lookup"><span data-stu-id="60429-133">Your three-node development cluster is now running.</span></span> <span data-ttu-id="60429-134">Hello modulu ServiceFabric PowerShell se instaluje s hello runtime.</span><span class="sxs-lookup"><span data-stu-id="60429-134">hello ServiceFabric PowerShell module is installed with hello runtime.</span></span>  <span data-ttu-id="60429-135">Můžete ověřit, že hello clusteru je spuštěn z hello stejný počítač nebo ze vzdáleného počítače pomocí modulu runtime Service Fabric hello.</span><span class="sxs-lookup"><span data-stu-id="60429-135">You can verify that hello cluster is running from hello same computer or from a remote computer with hello Service Fabric runtime.</span></span>  <span data-ttu-id="60429-136">Hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) rutina vytvoří cluster toohello připojení.</span><span class="sxs-lookup"><span data-stu-id="60429-136">hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet establishes a connection toohello cluster.</span></span>   

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint localhost:19000
```
<span data-ttu-id="60429-137">V tématu [clusteru zabezpečené připojení tooa](service-fabric-connect-to-secure-cluster.md) pro další příklady připojování tooa clusteru.</span><span class="sxs-lookup"><span data-stu-id="60429-137">See [Connect tooa secure cluster](service-fabric-connect-to-secure-cluster.md) for other examples of connecting tooa cluster.</span></span> <span data-ttu-id="60429-138">Po připojování toohello clusteru pomocí hello [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) rutiny toodisplay seznam uzlů v clusteru a stavové informace pro každý uzel hello.</span><span class="sxs-lookup"><span data-stu-id="60429-138">After connecting toohello cluster, use hello [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) cmdlet toodisplay a list of nodes in hello cluster and status information for each node.</span></span> <span data-ttu-id="60429-139">Položka **HealthState** by měla mít pro každý uzel hodnotu *OK*.</span><span class="sxs-lookup"><span data-stu-id="60429-139">**HealthState** should be *OK* for each node.</span></span>

```powershell
PS C:\temp\Microsoft.Azure.ServiceFabric.WindowsServer> Get-ServiceFabricNode |Format-Table

NodeDeactivationInfo NodeName IpAddressOrFQDN NodeType  CodeVersion  ConfigVersion NodeStatus NodeUpTime NodeDownTime HealthState
-------------------- -------- --------------- --------  -----------  ------------- ---------- ---------- ------------ -----------
                     vm2      localhost       NodeType2 5.6.220.9494 0                     Up 00:03:38   00:00:00              OK
                     vm1      localhost       NodeType1 5.6.220.9494 0                     Up 00:03:38   00:00:00              OK
                     vm0      localhost       NodeType0 5.6.220.9494 0                     Up 00:02:43   00:00:00              OK
```

## <a name="visualize-hello-cluster-using-service-fabric-explorer"></a><span data-ttu-id="60429-140">Vizualizace hello clusteru pomocí Service Fabric Exploreru</span><span class="sxs-lookup"><span data-stu-id="60429-140">Visualize hello cluster using Service Fabric explorer</span></span>
<span data-ttu-id="60429-141">[Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) je nástroj vhodný pro vizualizaci clusteru a správu aplikací.</span><span class="sxs-lookup"><span data-stu-id="60429-141">[Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) is a good tool for visualizing your cluster and managing applications.</span></span>  <span data-ttu-id="60429-142">Service Fabric Explorer je služba, která běží v clusteru hello, kterým můžete přistupovat pomocí prohlížeče tak, že přejdete příliš[http://localhost: 19080/Explorer](http://localhost:19080/Explorer).</span><span class="sxs-lookup"><span data-stu-id="60429-142">Service Fabric Explorer is a service that runs in hello cluster, which you access using a browser by navigating too[http://localhost:19080/Explorer](http://localhost:19080/Explorer).</span></span> 

<span data-ttu-id="60429-143">řídicí panel clusteru Hello obsahuje přehled clusteru, včetně shrnutí stavu uzlu a aplikace.</span><span class="sxs-lookup"><span data-stu-id="60429-143">hello cluster dashboard provides an overview of your cluster, including a summary of application and node health.</span></span> <span data-ttu-id="60429-144">zobrazení uzlu Hello zobrazuje fyzické rozložení hello hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="60429-144">hello node view shows hello physical layout of hello cluster.</span></span> <span data-ttu-id="60429-145">Pro daný uzel můžete zjistit, které aplikace mají v uzlu nasazený kód.</span><span class="sxs-lookup"><span data-stu-id="60429-145">For a given node, you can inspect which applications have code deployed on that node.</span></span>

![Service Fabric Explorer][service-fabric-explorer]

## <a name="remove-hello-cluster"></a><span data-ttu-id="60429-147">Odebrat hello cluster</span><span class="sxs-lookup"><span data-stu-id="60429-147">Remove hello cluster</span></span>
<span data-ttu-id="60429-148">tooremove clusteru, spusťte hello *RemoveServiceFabricCluster.ps1* skript prostředí PowerShell ze složky balíčku hello a předejte jí hello cesta toohello JSON konfigurační soubor.</span><span class="sxs-lookup"><span data-stu-id="60429-148">tooremove a cluster, run hello *RemoveServiceFabricCluster.ps1* PowerShell script from hello package folder and pass in hello path toohello JSON configuration file.</span></span> <span data-ttu-id="60429-149">Volitelně můžete zadat umístění pro protokol hello hello odstranění.</span><span class="sxs-lookup"><span data-stu-id="60429-149">You can optionally specify a location for hello log of hello deletion.</span></span>

```powershell
# Removes Service Fabric cluster nodes from each computer in hello configuration file.
.\RemoveServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json -Force
```

<span data-ttu-id="60429-150">tooremove runtime Service Fabric hello z hello počítači, spusťte následující skript prostředí PowerShell ze složky balíčku hello hello.</span><span class="sxs-lookup"><span data-stu-id="60429-150">tooremove hello Service Fabric runtime from hello computer, run hello following PowerShell script from hello package folder.</span></span>

```powershell
# Removes Service Fabric from hello current computer.
.\CleanFabric.ps1
```

## <a name="next-steps"></a><span data-ttu-id="60429-151">Další kroky</span><span class="sxs-lookup"><span data-stu-id="60429-151">Next steps</span></span>
<span data-ttu-id="60429-152">Teď, když jste nastavili vývojový samostatný cluster, zkuste hello následující články:</span><span class="sxs-lookup"><span data-stu-id="60429-152">Now that you have set up a development standalone cluster, try hello following articles:</span></span>
* <span data-ttu-id="60429-153">[Nastavení samostatného clusteru s více počítači](service-fabric-cluster-creation-for-windows-server.md) a povolení zabezpečení</span><span class="sxs-lookup"><span data-stu-id="60429-153">[Set up a multi-machine standalone cluster](service-fabric-cluster-creation-for-windows-server.md) and enable security.</span></span>
* [<span data-ttu-id="60429-154">Nasazení aplikací s použitím prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="60429-154">Deploy apps using PowerShell</span></span>](service-fabric-deploy-remove-applications.md)

[service-fabric-explorer]: ./media/service-fabric-get-started-standalone-cluster/sfx.png
