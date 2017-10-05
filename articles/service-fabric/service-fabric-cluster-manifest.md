---
title: "Konfigurace clusteru samostatné Azure Service Fabric | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat samostatnou nebo privátní cluster Service Fabric."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 0c5ec720-8f70-40bd-9f86-cd07b84a219d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/02/2017
ms.author: dekapur
ms.openlocfilehash: 9885dce18dabac4a945dafd219e3ae190e34a83b
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="configuration-settings-for-standalone-windows-cluster"></a><span data-ttu-id="0295c-103">Nastavení konfigurace pro samostatný cluster Windows</span><span class="sxs-lookup"><span data-stu-id="0295c-103">Configuration settings for standalone Windows cluster</span></span>
<span data-ttu-id="0295c-104">Tento článek popisuje postup konfigurace použití clusteru samostatné Service Fabric ***souboru ClusterConfig.JSON*** souboru.</span><span class="sxs-lookup"><span data-stu-id="0295c-104">This article describes how to configure a standalone Service Fabric cluster using the ***ClusterConfig.JSON*** file.</span></span> <span data-ttu-id="0295c-105">Tento soubor můžete zadat informace, jako je Service Fabric uzlů a jejich IP adresy, různé typy uzlů v clusteru, konfigurace zabezpečení, jakož i topologie sítě z hlediska domén selhání nebo upgradovat pro váš cluster samostatné.</span><span class="sxs-lookup"><span data-stu-id="0295c-105">You can use this file to specify information such as the Service Fabric nodes and their IP addresses, different types of nodes on the cluster, the security configurations as well as the network topology in terms of fault/upgrade domains, for your standalone cluster.</span></span>

<span data-ttu-id="0295c-106">Pokud jste [Stáhnout balíček Service Fabric samostatné](service-fabric-cluster-creation-for-windows-server.md#downloadpackage), několik ukázky souboru ClusterConfig.JSON souboru se stáhnou do počítače pracovní.</span><span class="sxs-lookup"><span data-stu-id="0295c-106">When you [download the standalone Service Fabric package](service-fabric-cluster-creation-for-windows-server.md#downloadpackage), a few samples of the ClusterConfig.JSON file are downloaded to your work machine.</span></span> <span data-ttu-id="0295c-107">Ukázky s *DevCluster* v jejich názvy vám pomůže vytvořit cluster s všechny tři uzly na stejném počítači, jako jsou logické uzly.</span><span class="sxs-lookup"><span data-stu-id="0295c-107">The samples having *DevCluster* in their names will help you create a cluster with all three nodes on the same machine, like logical nodes.</span></span> <span data-ttu-id="0295c-108">Z toho musí být nejméně jeden uzel označen jako primárního uzlu.</span><span class="sxs-lookup"><span data-stu-id="0295c-108">Out of these, at least one node must be marked as a primary node.</span></span> <span data-ttu-id="0295c-109">Tento cluster je vhodný pro vývoj nebo testovací prostředí a není podporován jako provozní cluster.</span><span class="sxs-lookup"><span data-stu-id="0295c-109">This cluster is useful for a development or test environment and is not supported as a production cluster.</span></span> <span data-ttu-id="0295c-110">Ukázky s *MultiMachine* v jejich názvy vám pomůže vytvořit cluster produkční kvality s každý uzel na samostatný počítač.</span><span class="sxs-lookup"><span data-stu-id="0295c-110">The samples having *MultiMachine* in their names, will help you create a production quality cluster, with each node on a separate machine.</span></span>

1. <span data-ttu-id="0295c-111">*ClusterConfig.Unsecure.DevCluster.JSON* a *ClusterConfig.Unsecure.MultiMachine.JSON* ukazují, jak vytvořit zabezpečená testu nebo produkční cluster v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="0295c-111">*ClusterConfig.Unsecure.DevCluster.JSON* and *ClusterConfig.Unsecure.MultiMachine.JSON* show how to create an unsecured test or production cluster respectively.</span></span> 
2. <span data-ttu-id="0295c-112">*ClusterConfig.Windows.DevCluster.JSON* a *ClusterConfig.Windows.MultiMachine.JSON* ukazují, jak vytvořit testovací nebo produkčního prostředí clusteru, zabezpečené pomocí [zabezpečení systému Windows](service-fabric-windows-cluster-windows-security.md).</span><span class="sxs-lookup"><span data-stu-id="0295c-112">*ClusterConfig.Windows.DevCluster.JSON* and  *ClusterConfig.Windows.MultiMachine.JSON* show how to create test or production cluster, secured using [Windows security](service-fabric-windows-cluster-windows-security.md).</span></span>
3. <span data-ttu-id="0295c-113">*ClusterConfig.X509.DevCluster.JSON* a *ClusterConfig.X509.MultiMachine.JSON* ukazují, jak vytvořit testovací nebo produkčního prostředí clusteru, zabezpečené pomocí [X509 zabezpečení na základě certifikátu](service-fabric-windows-cluster-x509-security.md).</span><span class="sxs-lookup"><span data-stu-id="0295c-113">*ClusterConfig.X509.DevCluster.JSON* and *ClusterConfig.X509.MultiMachine.JSON* show how to create test or production cluster, secured using [X509 certificate-based security](service-fabric-windows-cluster-x509-security.md).</span></span> 

<span data-ttu-id="0295c-114">Teď vyzkoušíme různé části ***souboru ClusterConfig.JSON*** souboru, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="0295c-114">Now we will examine the various sections of a ***ClusterConfig.JSON*** file as below.</span></span>

## <a name="general-cluster-configurations"></a><span data-ttu-id="0295c-115">Konfigurace obecných clusteru</span><span class="sxs-lookup"><span data-stu-id="0295c-115">General cluster configurations</span></span>
<span data-ttu-id="0295c-116">Toto se vztahuje konkrétní konfigurace široký clusteru, jak je znázorněno v následujícím fragmentu JSON.</span><span class="sxs-lookup"><span data-stu-id="0295c-116">This covers the broad cluster specific configurations, as shown in the JSON snippet below.</span></span>

    "name": "SampleCluster",
    "clusterConfigurationVersion": "1.0.0",
    "apiVersion": "01-2017",

<span data-ttu-id="0295c-117">Můžete udělit libovolný popisný název pro váš cluster Service Fabric přiřazením jeho **název** proměnné.</span><span class="sxs-lookup"><span data-stu-id="0295c-117">You can give any friendly name to your Service Fabric cluster by assigning it to the **name** variable.</span></span> <span data-ttu-id="0295c-118">**ClusterConfigurationVersion** je číslo verze vašeho clusteru; je třeba zvýšit pokaždé, když upgradujete cluster Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="0295c-118">The **clusterConfigurationVersion** is the version number of your cluster; you should increase it every time you upgrade your Service Fabric cluster.</span></span> <span data-ttu-id="0295c-119">Ale nechat **apiVersion** na výchozí hodnotu.</span><span class="sxs-lookup"><span data-stu-id="0295c-119">You should however leave the **apiVersion** to the default value.</span></span>

<a id="clusternodes"></a>

## <a name="nodes-on-the-cluster"></a><span data-ttu-id="0295c-120">Uzly v clusteru</span><span class="sxs-lookup"><span data-stu-id="0295c-120">Nodes on the cluster</span></span>
<span data-ttu-id="0295c-121">Můžete nakonfigurovat uzly v clusteru Service Fabric pomocí **uzly** části, jak ukazuje následující fragment kódu.</span><span class="sxs-lookup"><span data-stu-id="0295c-121">You can configure the nodes on your Service Fabric cluster by using the **nodes** section, as the following snippet shows.</span></span>

    "nodes": [{
        "nodeName": "vm0",
        "iPAddress": "localhost",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r0",
        "upgradeDomain": "UD0"
    }, {
        "nodeName": "vm1",
        "iPAddress": "localhost",
        "nodeTypeRef": "NodeType1",
        "faultDomain": "fd:/dc1/r1",
        "upgradeDomain": "UD1"
    }, {
        "nodeName": "vm2",
        "iPAddress": "localhost",
        "nodeTypeRef": "NodeType2",
        "faultDomain": "fd:/dc1/r2",
        "upgradeDomain": "UD2"
    }],

<span data-ttu-id="0295c-122">Cluster Service Fabric musí obsahovat aspoň 3 uzly.</span><span class="sxs-lookup"><span data-stu-id="0295c-122">A Service Fabric cluster must contain at least 3 nodes.</span></span> <span data-ttu-id="0295c-123">Do této části můžete přidat další uzly, podle vašeho nastavení.</span><span class="sxs-lookup"><span data-stu-id="0295c-123">You can add more nodes to this section as per your setup.</span></span> <span data-ttu-id="0295c-124">Následující tabulka popisuje nastavení konfigurace pro každý uzel.</span><span class="sxs-lookup"><span data-stu-id="0295c-124">The following table explains the configuration settings for each node.</span></span>

| <span data-ttu-id="0295c-125">**Konfigurace uzlu**</span><span class="sxs-lookup"><span data-stu-id="0295c-125">**Node configuration**</span></span> | <span data-ttu-id="0295c-126">**Popis**</span><span class="sxs-lookup"><span data-stu-id="0295c-126">**Description**</span></span> |
| --- | --- |
| <span data-ttu-id="0295c-127">nodeName</span><span class="sxs-lookup"><span data-stu-id="0295c-127">nodeName</span></span> |<span data-ttu-id="0295c-128">Do uzlu můžete dát libovolný popisný název.</span><span class="sxs-lookup"><span data-stu-id="0295c-128">You can give any friendly name to the node.</span></span> |
| <span data-ttu-id="0295c-129">IP adresa</span><span class="sxs-lookup"><span data-stu-id="0295c-129">iPAddress</span></span> |<span data-ttu-id="0295c-130">Zjistěte, tím otevřete okno příkazového řádku a zadáte IP adresu vašeho uzlu `ipconfig`.</span><span class="sxs-lookup"><span data-stu-id="0295c-130">Find out the IP address of your node by opening a command window and typing `ipconfig`.</span></span> <span data-ttu-id="0295c-131">Poznamenejte si adresu IPV4 a přiřadit ji ke **iPAddress** proměnné.</span><span class="sxs-lookup"><span data-stu-id="0295c-131">Note the IPV4 address and assign it to the **iPAddress** variable.</span></span> |
| <span data-ttu-id="0295c-132">nodeTypeRef</span><span class="sxs-lookup"><span data-stu-id="0295c-132">nodeTypeRef</span></span> |<span data-ttu-id="0295c-133">Každý uzel může být přiřazen jiný uzel typu.</span><span class="sxs-lookup"><span data-stu-id="0295c-133">Each node can be assigned a different node type.</span></span> <span data-ttu-id="0295c-134">[Typy uzlů](#nodetypes) jsou definovány v následující části.</span><span class="sxs-lookup"><span data-stu-id="0295c-134">The [node types](#nodetypes) are defined in the section below.</span></span> |
| <span data-ttu-id="0295c-135">faultDomain</span><span class="sxs-lookup"><span data-stu-id="0295c-135">faultDomain</span></span> |<span data-ttu-id="0295c-136">Domén selhání umožňují správcům clusteru definování fyzických uzlů, které může selhat z důvodu sdílené fyzické závislosti současně.</span><span class="sxs-lookup"><span data-stu-id="0295c-136">Fault domains enable cluster administrators to define the physical nodes that might fail at the same time due to shared physical dependencies.</span></span> |
| <span data-ttu-id="0295c-137">upgradeDomain</span><span class="sxs-lookup"><span data-stu-id="0295c-137">upgradeDomain</span></span> |<span data-ttu-id="0295c-138">Domén upgradu popisují sadu uzlů, které jsou vypnuté upgradů Service Fabric v přibližně ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="0295c-138">Upgrade domains describe sets of nodes that are shut down for Service Fabric upgrades at about the same time.</span></span> <span data-ttu-id="0295c-139">Můžete přiřadit které Upgradovacích domén, které uzly jako nejsou omezeny všechny fyzické požadavky.</span><span class="sxs-lookup"><span data-stu-id="0295c-139">You can choose which nodes to assign to which Upgrade domains, as they are not limited by any physical requirements.</span></span> |

## <a name="cluster-properties"></a><span data-ttu-id="0295c-140">Vlastnosti clusteru</span><span class="sxs-lookup"><span data-stu-id="0295c-140">Cluster properties</span></span>
<span data-ttu-id="0295c-141">**Vlastnosti** kapitoly souboru ClusterConfig.JSON slouží ke konfiguraci clusteru následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="0295c-141">The **properties** section in the ClusterConfig.JSON is used to configure the cluster as follows.</span></span>

<a id="reliability"></a>

### <a name="reliability"></a><span data-ttu-id="0295c-142">Spolehlivost</span><span class="sxs-lookup"><span data-stu-id="0295c-142">Reliability</span></span>
<span data-ttu-id="0295c-143">Koncept **reliabilityLevel** definuje počet replik nebo instancí systémových služeb Service Fabric, které můžou běžet na primární uzlů v clusteru.</span><span class="sxs-lookup"><span data-stu-id="0295c-143">The concept of **reliabilityLevel** defines the number of replicas or instances of the Service Fabric system services that can run on the primary nodes of the cluster.</span></span> <span data-ttu-id="0295c-144">Určuje spolehlivost tyto služby a proto clusteru.</span><span class="sxs-lookup"><span data-stu-id="0295c-144">It determines the reliability of these services and hence the cluster.</span></span> <span data-ttu-id="0295c-145">Hodnota je počítanou v systému v době vytváření a upgrade clusteru.</span><span class="sxs-lookup"><span data-stu-id="0295c-145">The value is calcuated by the system at cluster creation and upgrade time.</span></span>

### <a name="diagnostics"></a><span data-ttu-id="0295c-146">Diagnostika</span><span class="sxs-lookup"><span data-stu-id="0295c-146">Diagnostics</span></span>
<span data-ttu-id="0295c-147">**DiagnosticsStore** část vám pomůže nakonfigurovat parametry Povolit diagnostiku a řešení potíží uzel nebo selhání clusteru, jak je znázorněno v následujícím fragmentu kódu.</span><span class="sxs-lookup"><span data-stu-id="0295c-147">The **diagnosticsStore** section allows you to configure parameters to enable diagnostics and troubleshooting node or cluster failures, as shown in the following snippet.</span></span> 

    "diagnosticsStore": {
        "metadata":  "Please replace the diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "FileShare",
        "IsEncrypted": "false",
        "connectionstring": "c:\\ProgramData\\SF\\DiagnosticsStore"
    }

<span data-ttu-id="0295c-148">**Metadata** popis vašeho clusteru diagnostiky a může nastavit podle vašeho nastavení.</span><span class="sxs-lookup"><span data-stu-id="0295c-148">The **metadata** is a description of your cluster diagnostics and can be set as per your setup.</span></span> <span data-ttu-id="0295c-149">Tyto proměnné pomáhají při shromažďování ETW protokolů trasování, výpisy stavu systému a také čítače výkonu.</span><span class="sxs-lookup"><span data-stu-id="0295c-149">These variables help in collecting ETW trace logs, crash dumps as well as performance counters.</span></span> <span data-ttu-id="0295c-150">Čtení [protokolu trasování](https://msdn.microsoft.com/library/windows/hardware/ff552994.aspx) a [trasování ETW](https://msdn.microsoft.com/library/ms751538.aspx) protokoly trasování pro další informace o trasování událostí pro Windows.</span><span class="sxs-lookup"><span data-stu-id="0295c-150">Read [Tracelog](https://msdn.microsoft.com/library/windows/hardware/ff552994.aspx) and [ETW Tracing](https://msdn.microsoft.com/library/ms751538.aspx) for more information on ETW trace logs.</span></span> <span data-ttu-id="0295c-151">Všechny protokoly, včetně [havárií výpisy](https://blogs.technet.microsoft.com/askperf/2008/01/08/understanding-crash-dump-files/) a [čítače výkonu](https://msdn.microsoft.com/library/windows/desktop/aa373083.aspx) může přesměrovat k **connectionString** složky v počítači.</span><span class="sxs-lookup"><span data-stu-id="0295c-151">All logs including [Crash dumps](https://blogs.technet.microsoft.com/askperf/2008/01/08/understanding-crash-dump-files/) and [performance counters](https://msdn.microsoft.com/library/windows/desktop/aa373083.aspx) can be directed to the **connectionString** folder on your machine.</span></span> <span data-ttu-id="0295c-152">Můžete také použít *azurestorage* pro ukládání diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="0295c-152">You can also use *AzureStorage* for storing diagnostics.</span></span> <span data-ttu-id="0295c-153">Níže najdete fragment ukázka.</span><span class="sxs-lookup"><span data-stu-id="0295c-153">See below for a sample snippet.</span></span>

    "diagnosticsStore": {
        "metadata":  "Please replace the diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "AzureStorage",
        "IsEncrypted": "false",
        "connectionstring": "xstore:DefaultEndpointsProtocol=https;AccountName=[AzureAccountName];AccountKey=[AzureAccountKey]"
    }

### <a name="security"></a><span data-ttu-id="0295c-154">Zabezpečení</span><span class="sxs-lookup"><span data-stu-id="0295c-154">Security</span></span>
<span data-ttu-id="0295c-155">**Zabezpečení** je nezbytné pro cluster Service Fabric zabezpečené samostatné části.</span><span class="sxs-lookup"><span data-stu-id="0295c-155">The **security** section is necessary for a secure standalone Service Fabric cluster.</span></span> <span data-ttu-id="0295c-156">Následující fragment kódu ukazuje součástí této části.</span><span class="sxs-lookup"><span data-stu-id="0295c-156">The following snippet shows a part of this section.</span></span>

    "security": {
        "metadata": "This cluster is secured using X509 certificates.",
        "ClusterCredentialType": "X509",
        "ServerCredentialType": "X509",
        . . .
    }

<span data-ttu-id="0295c-157">**Metadata** popis zabezpečení clusteru a může nastavit podle vašeho nastavení.</span><span class="sxs-lookup"><span data-stu-id="0295c-157">The **metadata** is a description of your secure cluster and can be set as per your setup.</span></span> <span data-ttu-id="0295c-158">**ClusterCredentialType** a **ServerCredentialType** určit typ zabezpečení, která implementuje cluster a uzly.</span><span class="sxs-lookup"><span data-stu-id="0295c-158">The **ClusterCredentialType** and **ServerCredentialType** determine the type of security that the cluster and the nodes will implement.</span></span> <span data-ttu-id="0295c-159">Se může být nastaven na hodnotu *X509* pro zabezpečení na základě certifikátů nebo *Windows* pro zabezpečení služby založené na Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="0295c-159">They can be set to either *X509* for a certificate-based security, or *Windows* for an Azure Active Directory-based security.</span></span> <span data-ttu-id="0295c-160">Zbytek **zabezpečení** části budou založeny na typu zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="0295c-160">The rest of the **security** section will be based on the type of the security.</span></span> <span data-ttu-id="0295c-161">Čtení [zabezpečení na základě certifikátů v clusteru s podporou samostatné](service-fabric-windows-cluster-x509-security.md) nebo [zabezpečení systému Windows v clusteru s podporou samostatné](service-fabric-windows-cluster-windows-security.md) informace o tom, jak vyplnit zbytek **zabezpečení** části.</span><span class="sxs-lookup"><span data-stu-id="0295c-161">Read [Certificates-based security in a standalone cluster](service-fabric-windows-cluster-x509-security.md) or [Windows security in a standalone cluster](service-fabric-windows-cluster-windows-security.md) for information on how to fill out the rest of the **security** section.</span></span>

<a id="nodetypes"></a>

### <a name="node-types"></a><span data-ttu-id="0295c-162">Typy uzlů</span><span class="sxs-lookup"><span data-stu-id="0295c-162">Node Types</span></span>
<span data-ttu-id="0295c-163">**NodeTypes** část popisuje typ uzly, které má váš cluster.</span><span class="sxs-lookup"><span data-stu-id="0295c-163">The **nodeTypes** section describes the type of the nodes that your cluster has.</span></span> <span data-ttu-id="0295c-164">Zadejte nejméně jeden uzel lze uvést pro cluster, jak je znázorněno v následujícím fragmentu.</span><span class="sxs-lookup"><span data-stu-id="0295c-164">At least one node type must be specified for a cluster, as shown in the snippet below.</span></span> 

    "nodeTypes": [{
        "name": "NodeType0",
        "clientConnectionEndpointPort": "19000",
        "clusterConnectionEndpointPort": "19001",
        "leaseDriverEndpointPort": "19002"
        "serviceConnectionEndpointPort": "19003",
        "httpGatewayEndpointPort": "19080",
        "reverseProxyEndpointPort": "19081",
        "applicationPorts": {
            "startPort": "20575",
            "endPort": "20605"
        },
        "ephemeralPorts": {
            "startPort": "20606",
            "endPort": "20861"
        },
        "isPrimary": true
    }]

<span data-ttu-id="0295c-165">**Název** je popisný název pro tento typ konkrétním uzlu.</span><span class="sxs-lookup"><span data-stu-id="0295c-165">The **name** is the friendly name for this particular node type.</span></span> <span data-ttu-id="0295c-166">Chcete-li vytvořit uzel typu uzlu, přiřadit popisný název, který **nodeTypeRef** proměnné pro tento uzel, jak je uvedeno [výše](#clusternodes).</span><span class="sxs-lookup"><span data-stu-id="0295c-166">To create a node of this node type, assign its friendly name to the **nodeTypeRef** variable for that node, as mentioned [above](#clusternodes).</span></span> <span data-ttu-id="0295c-167">Pro každý typ uzlu definujte koncové body připojení, která se bude používat.</span><span class="sxs-lookup"><span data-stu-id="0295c-167">For each node type, define the connection endpoints that will be used.</span></span> <span data-ttu-id="0295c-168">Jakékoli číslo portu pro tyto koncové body připojení můžete vybrat tak dlouho, dokud nedošlo ke konfliktu s žádné koncové body v tomto clusteru.</span><span class="sxs-lookup"><span data-stu-id="0295c-168">You can choose any port number for these connection endpoints, as long as they do not conflict with any other endpoints in this cluster.</span></span> <span data-ttu-id="0295c-169">V clusteru s několika uzly budou jeden nebo více primární uzlů (tj. **isPrimary** nastavena na *true*), v závislosti na [ **reliabilityLevel**](#reliability).</span><span class="sxs-lookup"><span data-stu-id="0295c-169">In a multi-node cluster, there will be one or more primary nodes (i.e. **isPrimary** set to *true*), depending on the [**reliabilityLevel**](#reliability).</span></span> <span data-ttu-id="0295c-170">Čtení [aspekty plánování kapacity služby cluster Service Fabric](service-fabric-cluster-capacity.md) informace o **nodeTypes** a **reliabilityLevel**a potřebujete vědět, co jsou primární a typy uzel není primární.</span><span class="sxs-lookup"><span data-stu-id="0295c-170">Read [Service Fabric cluster capacity planning considerations](service-fabric-cluster-capacity.md) for information on **nodeTypes** and **reliabilityLevel**, and to know what are primary and the non-primary node types.</span></span> 

#### <a name="endpoints-used-to-configure-the-node-types"></a><span data-ttu-id="0295c-171">Koncové body, které slouží ke konfiguraci typy uzlů</span><span class="sxs-lookup"><span data-stu-id="0295c-171">Endpoints used to configure the node types</span></span>
* <span data-ttu-id="0295c-172">*clientConnectionEndpointPort* je port používaný klientem pro připojení ke clusteru, pokud používáte rozhraní API klienta.</span><span class="sxs-lookup"><span data-stu-id="0295c-172">*clientConnectionEndpointPort* is the port used by the client to connect to the cluster, when using the client APIs.</span></span> 
* <span data-ttu-id="0295c-173">*clusterConnectionEndpointPort* je port, ve kterém uzly vzájemně komunikovat.</span><span class="sxs-lookup"><span data-stu-id="0295c-173">*clusterConnectionEndpointPort* is the port at which the nodes communicate with each other.</span></span>
* <span data-ttu-id="0295c-174">*leaseDriverEndpointPort* je port je používán ovladač zapůjčení clusteru a zjistěte, zda jsou stále aktivní uzly.</span><span class="sxs-lookup"><span data-stu-id="0295c-174">*leaseDriverEndpointPort* is the port used by the cluster lease driver to find out if the nodes are still active.</span></span> 
* <span data-ttu-id="0295c-175">*serviceConnectionEndpointPort* je port používaný aplikace a služby, které jsou nasazeny na uzlu, ke komunikaci s klientem nástroje Service Fabric na příslušném uzlu.</span><span class="sxs-lookup"><span data-stu-id="0295c-175">*serviceConnectionEndpointPort* is the port used by the applications and services deployed on a node, to communicate with the Service Fabric client on that particular node.</span></span>
* <span data-ttu-id="0295c-176">*httpGatewayEndpointPort* je port používaný pro připojení ke clusteru Service Fabric Exploreru.</span><span class="sxs-lookup"><span data-stu-id="0295c-176">*httpGatewayEndpointPort* is the port used by the Service Fabric Explorer to connect to the cluster.</span></span>
* <span data-ttu-id="0295c-177">*ephemeralPorts* přepsat [dynamické porty používané operačního systému](https://support.microsoft.com/kb/929851).</span><span class="sxs-lookup"><span data-stu-id="0295c-177">*ephemeralPorts* override the [dynamic ports used by the OS](https://support.microsoft.com/kb/929851).</span></span> <span data-ttu-id="0295c-178">Service Fabric bude používat jako porty aplikace součástí a zbývající bude k dispozici pro operační systém.</span><span class="sxs-lookup"><span data-stu-id="0295c-178">Service Fabric will use a part of these as application ports and the remaining will be available for the OS.</span></span> <span data-ttu-id="0295c-179">Také mapuje tento rozsah na existující rozsah, který je součástí operačního systému, takže pro všechny účely, můžete používat rozsahy uvedenému v ukázkové soubory JSON.</span><span class="sxs-lookup"><span data-stu-id="0295c-179">It will also map this range to the existing range present in the OS, so for all purposes, you can use the ranges given in the sample JSON files.</span></span> <span data-ttu-id="0295c-180">Musíte se ujistit, že rozdíl mezi počáteční a koncové porty alespoň 255.</span><span class="sxs-lookup"><span data-stu-id="0295c-180">You need to make sure that the difference between the start and the end ports is at least 255.</span></span> <span data-ttu-id="0295c-181">Do konfliktu se může spustit, pokud tento rozdíl je příliš nízká, protože tento rozsah je sdílen s operačním systémem.</span><span class="sxs-lookup"><span data-stu-id="0295c-181">You may run into conflicts if this difference is too low, since this range is shared with the operating system.</span></span> <span data-ttu-id="0295c-182">V tématu rozsah dynamických portů nakonfigurované tak, že spustíte `netsh int ipv4 show dynamicport tcp`.</span><span class="sxs-lookup"><span data-stu-id="0295c-182">See the configured dynamic port range by running `netsh int ipv4 show dynamicport tcp`.</span></span>
* <span data-ttu-id="0295c-183">*applicationPorts* porty, které se použijí aplikace Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="0295c-183">*applicationPorts* are the ports that will be used by the Service Fabric applications.</span></span> <span data-ttu-id="0295c-184">Rozsah portů aplikace by měl být dostatečně velký pro požadavek koncový bod, vaše aplikace.</span><span class="sxs-lookup"><span data-stu-id="0295c-184">The application port range should be large enough to cover the endpoint requirement of your applications.</span></span> <span data-ttu-id="0295c-185">Tento rozsah by měl být výhradní z rozsahu dynamický port na počítači, tj. *ephemeralPorts* rozsah nastavený v konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="0295c-185">This range should be exclusive from the dynamic port range on the machine, i.e. the *ephemeralPorts* range as set in the configuration.</span></span>  <span data-ttu-id="0295c-186">Service Fabric se použít vždy, když nové porty jsou povinné, jakož i postará o otevření brány firewall pro tyto porty.</span><span class="sxs-lookup"><span data-stu-id="0295c-186">Service Fabric will use these whenever new ports are required, as well as take care of opening the firewall for these ports.</span></span> 
* <span data-ttu-id="0295c-187">*reverseProxyEndpointPort* je koncový bod volitelné reverzní proxy server.</span><span class="sxs-lookup"><span data-stu-id="0295c-187">*reverseProxyEndpointPort* is an optional reverse proxy endpoint.</span></span> <span data-ttu-id="0295c-188">V tématu [Service Fabric Reverse Proxy](service-fabric-reverseproxy.md) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="0295c-188">See [Service Fabric Reverse Proxy](service-fabric-reverseproxy.md) for more details.</span></span> 

### <a name="log-settings"></a><span data-ttu-id="0295c-189">Nastavení protokolu</span><span class="sxs-lookup"><span data-stu-id="0295c-189">Log Settings</span></span>
<span data-ttu-id="0295c-190">**FabricSettings** část vám pomůže nastavit kořenové adresáře pro data Service Fabric a protokoly.</span><span class="sxs-lookup"><span data-stu-id="0295c-190">The **fabricSettings** section allows you to set the root directories for the Service Fabric data and logs.</span></span> <span data-ttu-id="0295c-191">Můžete přizpůsobit tyto pouze při vytváření počáteční clusteru.</span><span class="sxs-lookup"><span data-stu-id="0295c-191">You can customize these only during the initial cluster creation.</span></span> <span data-ttu-id="0295c-192">Ukázka fragmentu v této části jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="0295c-192">See below for a sample snippet of this section.</span></span>

    "fabricSettings": [{
        "name": "Setup",
        "parameters": [{
            "name": "FabricDataRoot",
            "value": "C:\\ProgramData\\SF"
        }, {
            "name": "FabricLogRoot",
            "value": "C:\\ProgramData\\SF\\Log"
    }]

<span data-ttu-id="0295c-193">Je doporučeno použití jednotku operačního systému jako proměnná FabricDataRoot a adresáři FabricLogRoot, jako nabízí že další spolehlivost proti operačního systému, dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="0295c-193">We recommended using a non-OS drive as the FabricDataRoot and FabricLogRoot as it provides more reliability against OS crashes.</span></span> <span data-ttu-id="0295c-194">Všimněte si, že pokud přizpůsobíte pouze kořenové datové, pak kořenu protokolu budou umístěny jednu úroveň pod kořenovým adresářem data.</span><span class="sxs-lookup"><span data-stu-id="0295c-194">Note that if you customize only the data root, then the log root will be placed one level below the data root.</span></span>

### <a name="stateful-reliable-service-settings"></a><span data-ttu-id="0295c-195">Nastavení stavové spolehlivé služby</span><span class="sxs-lookup"><span data-stu-id="0295c-195">Stateful Reliable Service Settings</span></span>
<span data-ttu-id="0295c-196">**KtlLogger** část vám pomůže globální konfiguraci nastavení pro spolehlivé služby.</span><span class="sxs-lookup"><span data-stu-id="0295c-196">The **KtlLogger** section allows you to set the global configuration settings for Reliable Services.</span></span> <span data-ttu-id="0295c-197">Další informace o těchto nastaveních najdete [konfigurovat stavová spolehlivé služby](service-fabric-reliable-services-configuration.md).</span><span class="sxs-lookup"><span data-stu-id="0295c-197">For more details on these settings read [Configure stateful reliable services](service-fabric-reliable-services-configuration.md).</span></span>
<span data-ttu-id="0295c-198">Následující příklad ukazuje, jak změnit sdílené transakčního protokolu, která je vytvořena zálohovat všechny spolehlivé kolekce pro stavové služby.</span><span class="sxs-lookup"><span data-stu-id="0295c-198">The example below shows how to change the the shared transaction log that gets created to back any reliable collections for stateful services.</span></span>

    "fabricSettings": [{
        "name": "KtlLogger",
        "parameters": [{
            "name": "SharedLogSizeInMB",
            "value": "4096"
        }]
    }]

### <a name="add-on-features"></a><span data-ttu-id="0295c-199">Funkce rozšíření</span><span class="sxs-lookup"><span data-stu-id="0295c-199">Add-on features</span></span>
<span data-ttu-id="0295c-200">Ke konfiguraci rozšíření funkcí, apiVersion musí být nakonfigurován jako "04-2017' nebo vyšší a addonFeatures je potřeba nakonfigurovat:</span><span class="sxs-lookup"><span data-stu-id="0295c-200">To configure add-on features, the apiVersion should be configured as '04-2017' or higher, and addonFeatures needs to be configured:</span></span>

    "apiVersion": "04-2017",
    "properties": {
      "addOnFeatures": [
          "DnsService",
          "RepairManager"
      ]
    }

### <a name="container-support"></a><span data-ttu-id="0295c-201">Podpora kontejnerů</span><span class="sxs-lookup"><span data-stu-id="0295c-201">Container support</span></span>
<span data-ttu-id="0295c-202">Povolení podpory kontejner pro kontejner windows serveru a technologie hyper-v kontejneru pro samostatné clustery, musí být povolena funkce rozšíření 'DnsService'.</span><span class="sxs-lookup"><span data-stu-id="0295c-202">To enable container support for both windows server container and hyper-v container for standalone clusters, the 'DnsService' add-on feature needs to be enabled.</span></span>


## <a name="next-steps"></a><span data-ttu-id="0295c-203">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0295c-203">Next steps</span></span>
<span data-ttu-id="0295c-204">Až budete mít soubor dokončení souboru ClusterConfig.JSON nakonfigurovaný podle vaší samostatné nastavení clusteru, můžete nasadit cluster pomocí následujícího článku [vytvořit cluster Service Fabric samostatné](service-fabric-cluster-creation-for-windows-server.md) a pak pokračujte [vizualizace vašeho clusteru pomocí Service Fabric Exploreru](service-fabric-visualizing-your-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="0295c-204">Once you have a complete ClusterConfig.JSON file configured as per your standalone cluster setup, you can deploy your cluster by following the article [Create a standalone Service Fabric cluster](service-fabric-cluster-creation-for-windows-server.md) and then proceed to [visualizing your cluster with Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).</span></span>

