---
title: "Nastavení samostatného clusteru Azure Service Fabric | Dokumentace Microsoftu"
description: "Můžete vytvořit samostatný vývojový cluster se třemi uzly spuštěnými ve stejném počítači. Po dokončení tohoto nastavení budete umět sestavit cluster s více počítači."
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
ms.openlocfilehash: 5c8f4c784eed7b64810a3dd1c36c043d22a66936
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-your-first-service-fabric-standalone-cluster"></a><span data-ttu-id="537d7-104">Vytvoření vašeho prvního samostatného clusteru Service Fabric</span><span class="sxs-lookup"><span data-stu-id="537d7-104">Create your first Service Fabric standalone cluster</span></span>
<span data-ttu-id="537d7-105">Samostatný cluster Service Fabric můžete vytvořit v každém virtuálním nebo fyzickém počítači se systémem Windows Server 2012 R2 nebo Windows Server 2016, a to místně nebo v cloudu.</span><span class="sxs-lookup"><span data-stu-id="537d7-105">You can create a Service Fabric standalone cluster on any virtual machines or computers running Windows Server 2012 R2 or Windows Server 2016, on-premises or in the cloud.</span></span> <span data-ttu-id="537d7-106">Tento rychlý postup vám pomůže vytvořit samostatný vývojový cluster za několik minut.</span><span class="sxs-lookup"><span data-stu-id="537d7-106">This quickstart helps you to create a development standalone cluster in just a few minutes.</span></span>  <span data-ttu-id="537d7-107">Po provedení postupu budete mít cluster se třemi uzly, který běží v jednom počítači a do kterého můžete nasazovat aplikace.</span><span class="sxs-lookup"><span data-stu-id="537d7-107">When you're finished, you have a three-node cluster running on a single computer that you can deploy apps to.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="537d7-108">Než začnete</span><span class="sxs-lookup"><span data-stu-id="537d7-108">Before you begin</span></span>
<span data-ttu-id="537d7-109">Service Fabric nabízí instalační balíček pro vytváření samostatných clusterů Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="537d7-109">Service Fabric provides a setup package to create Service Fabric standalone clusters.</span></span>  <span data-ttu-id="537d7-110">[Stáhněte instalační balíček](http://go.microsoft.com/fwlink/?LinkId=730690).</span><span class="sxs-lookup"><span data-stu-id="537d7-110">[Download the setup package](http://go.microsoft.com/fwlink/?LinkId=730690).</span></span>  <span data-ttu-id="537d7-111">Rozbalte instalační balíček do složky na počítači nebo virtuálním počítači, kde nastavujete vývojový cluster.</span><span class="sxs-lookup"><span data-stu-id="537d7-111">Unzip the setup package to a folder on the computer or virtual machine where you are setting up the development cluster.</span></span>  <span data-ttu-id="537d7-112">Obsah instalačního balíčku je podrobně popsán [zde](service-fabric-cluster-standalone-package-contents.md).</span><span class="sxs-lookup"><span data-stu-id="537d7-112">The contents of the setup package are described in detail [here](service-fabric-cluster-standalone-package-contents.md).</span></span>

<span data-ttu-id="537d7-113">Správce clusteru, který cluster nasazuje a konfiguruje, musí mít v příslušném počítači oprávnění správce.</span><span class="sxs-lookup"><span data-stu-id="537d7-113">The cluster administrator deploying and configuring the cluster must have administrator privileges on the computer.</span></span> <span data-ttu-id="537d7-114">Service Fabric nelze nainstalovat na řadič domény.</span><span class="sxs-lookup"><span data-stu-id="537d7-114">You cannot install Service Fabric on a domain controller.</span></span>

## <a name="validate-the-environment"></a><span data-ttu-id="537d7-115">Ověření prostředí</span><span class="sxs-lookup"><span data-stu-id="537d7-115">Validate the environment</span></span>
<span data-ttu-id="537d7-116">Skript *TestConfiguration.ps1* v samostatném balíčku se používá jako analyzátor s osvědčenými postupy pro ověření, jestli je možné cluster nasadit do daného prostředí.</span><span class="sxs-lookup"><span data-stu-id="537d7-116">The *TestConfiguration.ps1* script in the standalone package is used as a best practices analyzer to validate whether a cluster can be deployed on a given environment.</span></span> <span data-ttu-id="537d7-117">V článku [Příprava nasazení](service-fabric-cluster-standalone-deployment-preparation.md) jsou vypsány předpoklady a požadavky prostředí.</span><span class="sxs-lookup"><span data-stu-id="537d7-117">[Deployment preparation](service-fabric-cluster-standalone-deployment-preparation.md) lists the pre-requisites and environment requirements.</span></span> <span data-ttu-id="537d7-118">Spusťte skript a ověřte, jestli můžete vývojový cluster vytvořit:</span><span class="sxs-lookup"><span data-stu-id="537d7-118">Run the script to verify if you can create the development cluster:</span></span>

```powershell
.\TestConfiguration.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json
```
## <a name="create-the-cluster"></a><span data-ttu-id="537d7-119">Vytvoření clusteru</span><span class="sxs-lookup"><span data-stu-id="537d7-119">Create the cluster</span></span>
<span data-ttu-id="537d7-120">Několik ukázkových konfiguračních souborů clusteru se nainstaluje spolu s instalačním balíčkem.</span><span class="sxs-lookup"><span data-stu-id="537d7-120">Several sample cluster configuration files are installed with the setup package.</span></span> <span data-ttu-id="537d7-121">Soubor *ClusterConfig.Unsecure.DevCluster.json* představuje nejjednodušší konfiguraci clusteru: nezabezpečený cluster se třemi uzly spuštěnými v jednom počítači.</span><span class="sxs-lookup"><span data-stu-id="537d7-121">*ClusterConfig.Unsecure.DevCluster.json* is the simplest cluster configuration: an unsecure, three-node cluster running on a single computer.</span></span>  <span data-ttu-id="537d7-122">Další konfigurační soubory popisují clustery s jedním nebo více počítači zabezpečené pomocí certifikátů X.509 nebo s použitím zabezpečení systému Windows.</span><span class="sxs-lookup"><span data-stu-id="537d7-122">Other config files describe single or multi-machine clusters secured with X.509 certificates or Windows security.</span></span>  <span data-ttu-id="537d7-123">Nemusíte upravovat žádná výchozí nastavení konfigurace pro tento kurz, ale projděte si konfigurační soubor a seznamte se s nastavením.</span><span class="sxs-lookup"><span data-stu-id="537d7-123">You don't need to modify any of the default config settings for this tutorial, but look through the config file and get familiar with the settings.</span></span>  <span data-ttu-id="537d7-124">Část **nodes** popisuje tři uzly v clusteru: název, IP adresa, [typ uzlu, doména selhání a upgradovací doména](service-fabric-cluster-manifest.md#nodes-on-the-cluster).</span><span class="sxs-lookup"><span data-stu-id="537d7-124">The **nodes** section describes the three nodes in the cluster: name, IP address, [node type, fault domain, and upgrade domain](service-fabric-cluster-manifest.md#nodes-on-the-cluster).</span></span>  <span data-ttu-id="537d7-125">Část **properties** definuje [zabezpečení, úroveň spolehlivosti, shromažďování diagnostických dat a typy uzlů](service-fabric-cluster-manifest.md#cluster-properties) pro cluster.</span><span class="sxs-lookup"><span data-stu-id="537d7-125">The **properties** section defines the [security, reliability level, diagnostics collection, and types of nodes](service-fabric-cluster-manifest.md#cluster-properties) for the cluster.</span></span>

<span data-ttu-id="537d7-126">Tento cluster není zabezpečený.</span><span class="sxs-lookup"><span data-stu-id="537d7-126">This cluster is unsecure.</span></span>  <span data-ttu-id="537d7-127">Každý se může anonymně připojit a provádět operace správy, proto by produkční clustery vždy měly být zabezpečené pomocí certifikátů X.509 nebo zabezpečení systému Windows.</span><span class="sxs-lookup"><span data-stu-id="537d7-127">Anyone can connect anonymously and perform management operations, so production clusters should always be secured using X.509 certificates or Windows security.</span></span>  <span data-ttu-id="537d7-128">Zabezpečení se konfiguruje jenom při vytváření clusteru a není možné povolit zabezpečení po vytvoření clusteru.</span><span class="sxs-lookup"><span data-stu-id="537d7-128">Security is only configured at cluster creation time and it is not possible to enable security after the cluster is created.</span></span>  <span data-ttu-id="537d7-129">V článku [Zabezpečení clusteru](service-fabric-cluster-security.md) najdete další informace o zabezpečení clusteru Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="537d7-129">Read [Secure a cluster](service-fabric-cluster-security.md) to learn more about Service Fabric cluster security.</span></span>  

<span data-ttu-id="537d7-130">Chcete-li vytvořit vývojový cluster se třemi uzly, spusťte skript *CreateServiceFabricCluster.ps1* z relace prostředí PowerShell správce:</span><span class="sxs-lookup"><span data-stu-id="537d7-130">To create the three-node development cluster, run the *CreateServiceFabricCluster.ps1* script from an administrator PowerShell session:</span></span>

```powershell
.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json -AcceptEULA
```

<span data-ttu-id="537d7-131">Balíček běhového prostředí Service Fabric se automaticky stáhne a nainstaluje v době vytváření clusteru.</span><span class="sxs-lookup"><span data-stu-id="537d7-131">The Service Fabric runtime package is automatically downloaded and installed at time of cluster creation.</span></span>

## <a name="connect-to-the-cluster"></a><span data-ttu-id="537d7-132">Připojení ke clusteru</span><span class="sxs-lookup"><span data-stu-id="537d7-132">Connect to the cluster</span></span>
<span data-ttu-id="537d7-133">Vývojový cluster se třemi uzly je nyní spuštěn.</span><span class="sxs-lookup"><span data-stu-id="537d7-133">Your three-node development cluster is now running.</span></span> <span data-ttu-id="537d7-134">Modul PowerShell ServiceFabric se instaluje spolu s modulem runtime.</span><span class="sxs-lookup"><span data-stu-id="537d7-134">The ServiceFabric PowerShell module is installed with the runtime.</span></span>  <span data-ttu-id="537d7-135">Ověřit, že je cluster spuštěn, můžete ze stejného počítače nebo ze vzdáleného počítače s modulem runtime Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="537d7-135">You can verify that the cluster is running from the same computer or from a remote computer with the Service Fabric runtime.</span></span>  <span data-ttu-id="537d7-136">Rutina [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) vytvoří připojení ke clusteru.</span><span class="sxs-lookup"><span data-stu-id="537d7-136">The [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet establishes a connection to the cluster.</span></span>   

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint localhost:19000
```
<span data-ttu-id="537d7-137">Další příklady připojení ke clusteru najdete v článku [Připojení k zabezpečenému clusteru](service-fabric-connect-to-secure-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="537d7-137">See [Connect to a secure cluster](service-fabric-connect-to-secure-cluster.md) for other examples of connecting to a cluster.</span></span> <span data-ttu-id="537d7-138">Po připojení ke clusteru zobrazte pomocí rutiny [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) seznam uzlů v clusteru a stavové informace pro každý uzel.</span><span class="sxs-lookup"><span data-stu-id="537d7-138">After connecting to the cluster, use the [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) cmdlet to display a list of nodes in the cluster and status information for each node.</span></span> <span data-ttu-id="537d7-139">Položka **HealthState** by měla mít pro každý uzel hodnotu *OK*.</span><span class="sxs-lookup"><span data-stu-id="537d7-139">**HealthState** should be *OK* for each node.</span></span>

```powershell
PS C:\temp\Microsoft.Azure.ServiceFabric.WindowsServer> Get-ServiceFabricNode |Format-Table

NodeDeactivationInfo NodeName IpAddressOrFQDN NodeType  CodeVersion  ConfigVersion NodeStatus NodeUpTime NodeDownTime HealthState
-------------------- -------- --------------- --------  -----------  ------------- ---------- ---------- ------------ -----------
                     vm2      localhost       NodeType2 5.6.220.9494 0                     Up 00:03:38   00:00:00              OK
                     vm1      localhost       NodeType1 5.6.220.9494 0                     Up 00:03:38   00:00:00              OK
                     vm0      localhost       NodeType0 5.6.220.9494 0                     Up 00:02:43   00:00:00              OK
```

## <a name="visualize-the-cluster-using-service-fabric-explorer"></a><span data-ttu-id="537d7-140">Vizualizace clusteru pomocí Service Fabric Exploreru</span><span class="sxs-lookup"><span data-stu-id="537d7-140">Visualize the cluster using Service Fabric explorer</span></span>
<span data-ttu-id="537d7-141">[Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) je nástroj vhodný pro vizualizaci clusteru a správu aplikací.</span><span class="sxs-lookup"><span data-stu-id="537d7-141">[Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) is a good tool for visualizing your cluster and managing applications.</span></span>  <span data-ttu-id="537d7-142">Service Fabric Explorer je služba, která běží v clusteru. Můžete k ní přistupovat prostřednictvím prohlížeče, pokud přejdete na adresu [http://localhost:19080/Explorer](http://localhost:19080/Explorer).</span><span class="sxs-lookup"><span data-stu-id="537d7-142">Service Fabric Explorer is a service that runs in the cluster, which you access using a browser by navigating to [http://localhost:19080/Explorer](http://localhost:19080/Explorer).</span></span> 

<span data-ttu-id="537d7-143">Řídicí panel clusteru poskytuje přehled o clusteru včetně souhrnu stavu aplikací a uzlů.</span><span class="sxs-lookup"><span data-stu-id="537d7-143">The cluster dashboard provides an overview of your cluster, including a summary of application and node health.</span></span> <span data-ttu-id="537d7-144">Zobrazení uzlu obsahuje fyzické rozložení clusteru.</span><span class="sxs-lookup"><span data-stu-id="537d7-144">The node view shows the physical layout of the cluster.</span></span> <span data-ttu-id="537d7-145">Pro daný uzel můžete zjistit, které aplikace mají v uzlu nasazený kód.</span><span class="sxs-lookup"><span data-stu-id="537d7-145">For a given node, you can inspect which applications have code deployed on that node.</span></span>

![Service Fabric Explorer][service-fabric-explorer]

## <a name="remove-the-cluster"></a><span data-ttu-id="537d7-147">Odebrání clusteru</span><span class="sxs-lookup"><span data-stu-id="537d7-147">Remove the cluster</span></span>
<span data-ttu-id="537d7-148">Chcete-li cluster odebrat, spusťte skript *RemoveServiceFabricCluster.ps1* prostředí PowerShell ze složky balíčku a předejte mu cestu ke konfiguračnímu souboru JSON.</span><span class="sxs-lookup"><span data-stu-id="537d7-148">To remove a cluster, run the *RemoveServiceFabricCluster.ps1* PowerShell script from the package folder and pass in the path to the JSON configuration file.</span></span> <span data-ttu-id="537d7-149">Volitelně můžete určit umístění pro protokol odstranění.</span><span class="sxs-lookup"><span data-stu-id="537d7-149">You can optionally specify a location for the log of the deletion.</span></span>

```powershell
# Removes Service Fabric cluster nodes from each computer in the configuration file.
.\RemoveServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json -Force
```

<span data-ttu-id="537d7-150">Pokud chcete z počítače odebrat modul runtime Service Fabric, spusťte následující skript prostředí PowerShell ze složky balíčku.</span><span class="sxs-lookup"><span data-stu-id="537d7-150">To remove the Service Fabric runtime from the computer, run the following PowerShell script from the package folder.</span></span>

```powershell
# Removes Service Fabric from the current computer.
.\CleanFabric.ps1
```

## <a name="next-steps"></a><span data-ttu-id="537d7-151">Další kroky</span><span class="sxs-lookup"><span data-stu-id="537d7-151">Next steps</span></span>
<span data-ttu-id="537d7-152">Teď, když jste nastavili samostatný vývojový cluster, zkuste následující články:</span><span class="sxs-lookup"><span data-stu-id="537d7-152">Now that you have set up a development standalone cluster, try the following articles:</span></span>
* <span data-ttu-id="537d7-153">[Nastavení samostatného clusteru s více počítači](service-fabric-cluster-creation-for-windows-server.md) a povolení zabezpečení</span><span class="sxs-lookup"><span data-stu-id="537d7-153">[Set up a multi-machine standalone cluster](service-fabric-cluster-creation-for-windows-server.md) and enable security.</span></span>
* [<span data-ttu-id="537d7-154">Nasazení aplikací s použitím prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="537d7-154">Deploy apps using PowerShell</span></span>](service-fabric-deploy-remove-applications.md)

[service-fabric-explorer]: ./media/service-fabric-get-started-standalone-cluster/sfx.png
