---
title: "aaaConfigure samostatný cluster Azure Service Fabric | Microsoft Docs"
description: "Zjistěte, jak tooconfigure samostatné nebo privátní cluster Service Fabric."
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
ms.openlocfilehash: ce2ad387162a05668bbd3a271c754776fe471850
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configuration-settings-for-standalone-windows-cluster"></a><span data-ttu-id="e4e07-103">Nastavení konfigurace pro samostatný cluster Windows</span><span class="sxs-lookup"><span data-stu-id="e4e07-103">Configuration settings for standalone Windows cluster</span></span>
<span data-ttu-id="e4e07-104">Tento článek popisuje, jak pomocí clusteru samostatné Service Fabric tooconfigure hello ***souboru ClusterConfig.JSON*** souboru.</span><span class="sxs-lookup"><span data-stu-id="e4e07-104">This article describes how tooconfigure a standalone Service Fabric cluster using hello ***ClusterConfig.JSON*** file.</span></span> <span data-ttu-id="e4e07-105">Tyto informace toospecify souboru jako uzly hello Service Fabric a jejich IP adresy, různé typy uzlů můžete použít na hello clusteru, konfigurace zabezpečení hello, jakož i hello topologie sítě z hlediska selhání nebo upgradovat domén pro vaše samostatnou cluster.</span><span class="sxs-lookup"><span data-stu-id="e4e07-105">You can use this file toospecify information such as hello Service Fabric nodes and their IP addresses, different types of nodes on hello cluster, hello security configurations as well as hello network topology in terms of fault/upgrade domains, for your standalone cluster.</span></span>

<span data-ttu-id="e4e07-106">Pokud jste [Stáhnout balíček Service Fabric samostatné hello](service-fabric-cluster-creation-for-windows-server.md#downloadpackage), několik ukázky hello souboru ClusterConfig.JSON souboru jsou stažené tooyour pracovní počítače.</span><span class="sxs-lookup"><span data-stu-id="e4e07-106">When you [download hello standalone Service Fabric package](service-fabric-cluster-creation-for-windows-server.md#downloadpackage), a few samples of hello ClusterConfig.JSON file are downloaded tooyour work machine.</span></span> <span data-ttu-id="e4e07-107">Ukázky Hello s *DevCluster* v jejich názvy vám pomůže vytvořit cluster s všechny tři uzly na stejný počítač, jako jsou logické uzly hello.</span><span class="sxs-lookup"><span data-stu-id="e4e07-107">hello samples having *DevCluster* in their names will help you create a cluster with all three nodes on hello same machine, like logical nodes.</span></span> <span data-ttu-id="e4e07-108">Z toho musí být nejméně jeden uzel označen jako primárního uzlu.</span><span class="sxs-lookup"><span data-stu-id="e4e07-108">Out of these, at least one node must be marked as a primary node.</span></span> <span data-ttu-id="e4e07-109">Tento cluster je vhodný pro vývoj nebo testovací prostředí a není podporován jako provozní cluster.</span><span class="sxs-lookup"><span data-stu-id="e4e07-109">This cluster is useful for a development or test environment and is not supported as a production cluster.</span></span> <span data-ttu-id="e4e07-110">Ukázky Hello s *MultiMachine* v jejich názvy vám pomůže vytvořit cluster produkční kvality s každý uzel na samostatný počítač.</span><span class="sxs-lookup"><span data-stu-id="e4e07-110">hello samples having *MultiMachine* in their names, will help you create a production quality cluster, with each node on a separate machine.</span></span>

1. <span data-ttu-id="e4e07-111">*ClusterConfig.Unsecure.DevCluster.JSON* a *ClusterConfig.Unsecure.MultiMachine.JSON* ukazují, jak toocreate test zabezpečená nebo produkčního prostředí clusteru v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="e4e07-111">*ClusterConfig.Unsecure.DevCluster.JSON* and *ClusterConfig.Unsecure.MultiMachine.JSON* show how toocreate an unsecured test or production cluster respectively.</span></span> 
2. <span data-ttu-id="e4e07-112">*ClusterConfig.Windows.DevCluster.JSON* a *ClusterConfig.Windows.MultiMachine.JSON* zobrazit jak toocreate testu nebo produkční clusteru zabezpečené pomocí [zabezpečení systému Windows](service-fabric-windows-cluster-windows-security.md).</span><span class="sxs-lookup"><span data-stu-id="e4e07-112">*ClusterConfig.Windows.DevCluster.JSON* and  *ClusterConfig.Windows.MultiMachine.JSON* show how toocreate test or production cluster, secured using [Windows security](service-fabric-windows-cluster-windows-security.md).</span></span>
3. <span data-ttu-id="e4e07-113">*ClusterConfig.X509.DevCluster.JSON* a *ClusterConfig.X509.MultiMachine.JSON* zobrazit jak toocreate testu nebo produkční clusteru zabezpečené pomocí [X509 zabezpečení na základě certifikátu](service-fabric-windows-cluster-x509-security.md).</span><span class="sxs-lookup"><span data-stu-id="e4e07-113">*ClusterConfig.X509.DevCluster.JSON* and *ClusterConfig.X509.MultiMachine.JSON* show how toocreate test or production cluster, secured using [X509 certificate-based security](service-fabric-windows-cluster-x509-security.md).</span></span> 

<span data-ttu-id="e4e07-114">Teď vyzkoušíme hello různé části ***souboru ClusterConfig.JSON*** souboru, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="e4e07-114">Now we will examine hello various sections of a ***ClusterConfig.JSON*** file as below.</span></span>

## <a name="general-cluster-configurations"></a><span data-ttu-id="e4e07-115">Konfigurace obecných clusteru</span><span class="sxs-lookup"><span data-stu-id="e4e07-115">General cluster configurations</span></span>
<span data-ttu-id="e4e07-116">Toto se vztahuje hello široký clusteru určité konfigurace, jak je uvedené v následující fragment hello JSON.</span><span class="sxs-lookup"><span data-stu-id="e4e07-116">This covers hello broad cluster specific configurations, as shown in hello JSON snippet below.</span></span>

    "name": "SampleCluster",
    "clusterConfigurationVersion": "1.0.0",
    "apiVersion": "01-2017",

<span data-ttu-id="e4e07-117">Cluster Service Fabric tooyour žádné popisný název můžete udělit přiřazením toohello **název** proměnné.</span><span class="sxs-lookup"><span data-stu-id="e4e07-117">You can give any friendly name tooyour Service Fabric cluster by assigning it toohello **name** variable.</span></span> <span data-ttu-id="e4e07-118">Hello **clusterConfigurationVersion** je číslo verze hello clusteru; je třeba zvýšit pokaždé, když upgradujete cluster Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="e4e07-118">hello **clusterConfigurationVersion** is hello version number of your cluster; you should increase it every time you upgrade your Service Fabric cluster.</span></span> <span data-ttu-id="e4e07-119">Ale nechat hello **apiVersion** toohello výchozí hodnotu.</span><span class="sxs-lookup"><span data-stu-id="e4e07-119">You should however leave hello **apiVersion** toohello default value.</span></span>

<a id="clusternodes"></a>

## <a name="nodes-on-hello-cluster"></a><span data-ttu-id="e4e07-120">Uzly v clusteru hello</span><span class="sxs-lookup"><span data-stu-id="e4e07-120">Nodes on hello cluster</span></span>
<span data-ttu-id="e4e07-121">Můžete nakonfigurovat hello uzly v clusteru Service Fabric pomocí hello **uzly** jako hello následující fragment kódu ukazuje.</span><span class="sxs-lookup"><span data-stu-id="e4e07-121">You can configure hello nodes on your Service Fabric cluster by using hello **nodes** section, as hello following snippet shows.</span></span>

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

<span data-ttu-id="e4e07-122">Cluster Service Fabric musí obsahovat aspoň 3 uzly.</span><span class="sxs-lookup"><span data-stu-id="e4e07-122">A Service Fabric cluster must contain at least 3 nodes.</span></span> <span data-ttu-id="e4e07-123">Můžete přidat další uzly toothis oddíl podle vašeho nastavení.</span><span class="sxs-lookup"><span data-stu-id="e4e07-123">You can add more nodes toothis section as per your setup.</span></span> <span data-ttu-id="e4e07-124">Hello následující tabulka vysvětluje hello nastavení konfigurace pro každý uzel.</span><span class="sxs-lookup"><span data-stu-id="e4e07-124">hello following table explains hello configuration settings for each node.</span></span>

| <span data-ttu-id="e4e07-125">**Konfigurace uzlu**</span><span class="sxs-lookup"><span data-stu-id="e4e07-125">**Node configuration**</span></span> | <span data-ttu-id="e4e07-126">**Popis**</span><span class="sxs-lookup"><span data-stu-id="e4e07-126">**Description**</span></span> |
| --- | --- |
| <span data-ttu-id="e4e07-127">nodeName</span><span class="sxs-lookup"><span data-stu-id="e4e07-127">nodeName</span></span> |<span data-ttu-id="e4e07-128">Můžete udělit libovolného uzlu toohello popisný název.</span><span class="sxs-lookup"><span data-stu-id="e4e07-128">You can give any friendly name toohello node.</span></span> |
| <span data-ttu-id="e4e07-129">IP adresa</span><span class="sxs-lookup"><span data-stu-id="e4e07-129">iPAddress</span></span> |<span data-ttu-id="e4e07-130">Zjistěte, tím otevřete okno příkazového řádku a zadáte hello IP adresu vašeho uzlu `ipconfig`.</span><span class="sxs-lookup"><span data-stu-id="e4e07-130">Find out hello IP address of your node by opening a command window and typing `ipconfig`.</span></span> <span data-ttu-id="e4e07-131">Poznamenejte si adresu hello IPV4 a přiřaďte ho toohello **iPAddress** proměnné.</span><span class="sxs-lookup"><span data-stu-id="e4e07-131">Note hello IPV4 address and assign it toohello **iPAddress** variable.</span></span> |
| <span data-ttu-id="e4e07-132">nodeTypeRef</span><span class="sxs-lookup"><span data-stu-id="e4e07-132">nodeTypeRef</span></span> |<span data-ttu-id="e4e07-133">Každý uzel může být přiřazen jiný uzel typu.</span><span class="sxs-lookup"><span data-stu-id="e4e07-133">Each node can be assigned a different node type.</span></span> <span data-ttu-id="e4e07-134">Hello [typy uzlů](#nodetypes) jsou definovány v následující části hello.</span><span class="sxs-lookup"><span data-stu-id="e4e07-134">hello [node types](#nodetypes) are defined in hello section below.</span></span> |
| <span data-ttu-id="e4e07-135">faultDomain</span><span class="sxs-lookup"><span data-stu-id="e4e07-135">faultDomain</span></span> |<span data-ttu-id="e4e07-136">Chyby domény povolit clusteru správci toodefine hello fyzickém uzlu, který může selhat v hello stejný čas kvůli tooshared fyzické závislosti.</span><span class="sxs-lookup"><span data-stu-id="e4e07-136">Fault domains enable cluster administrators toodefine hello physical nodes that might fail at hello same time due tooshared physical dependencies.</span></span> |
| <span data-ttu-id="e4e07-137">upgradeDomain</span><span class="sxs-lookup"><span data-stu-id="e4e07-137">upgradeDomain</span></span> |<span data-ttu-id="e4e07-138">Domén upgradu popisují sadu uzlů, které jsou vypnuty pro Service Fabric upgraduje na o hello stejný čas.</span><span class="sxs-lookup"><span data-stu-id="e4e07-138">Upgrade domains describe sets of nodes that are shut down for Service Fabric upgrades at about hello same time.</span></span> <span data-ttu-id="e4e07-139">Můžete toowhich tooassign které uzly domén upgradu, jako nejsou omezeny všechny fyzické požadavky.</span><span class="sxs-lookup"><span data-stu-id="e4e07-139">You can choose which nodes tooassign toowhich Upgrade domains, as they are not limited by any physical requirements.</span></span> |

## <a name="cluster-properties"></a><span data-ttu-id="e4e07-140">Vlastnosti clusteru</span><span class="sxs-lookup"><span data-stu-id="e4e07-140">Cluster properties</span></span>
<span data-ttu-id="e4e07-141">Hello **vlastnosti** je oddíl v souboru ClusterConfig.JSON hello použité tooconfigure hello clusteru následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="e4e07-141">hello **properties** section in hello ClusterConfig.JSON is used tooconfigure hello cluster as follows.</span></span>

<a id="reliability"></a>

### <a name="reliability"></a><span data-ttu-id="e4e07-142">Spolehlivost</span><span class="sxs-lookup"><span data-stu-id="e4e07-142">Reliability</span></span>
<span data-ttu-id="e4e07-143">Koncept Hello **reliabilityLevel** definuje hello počet replik nebo instancí hello Service Fabric systémových služeb, které můžou běžet na hello primární uzly clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="e4e07-143">hello concept of **reliabilityLevel** defines hello number of replicas or instances of hello Service Fabric system services that can run on hello primary nodes of hello cluster.</span></span> <span data-ttu-id="e4e07-144">To určuje spolehlivost hello z těchto služeb a proto hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="e4e07-144">It determines hello reliability of these services and hence hello cluster.</span></span> <span data-ttu-id="e4e07-145">Hodnota Hello je počítanou hello systému v době vytváření a upgrade clusteru.</span><span class="sxs-lookup"><span data-stu-id="e4e07-145">hello value is calcuated by hello system at cluster creation and upgrade time.</span></span>

### <a name="diagnostics"></a><span data-ttu-id="e4e07-146">Diagnostika</span><span class="sxs-lookup"><span data-stu-id="e4e07-146">Diagnostics</span></span>
<span data-ttu-id="e4e07-147">Hello **diagnosticsStore** část vám tooconfigure parametry tooenable Diagnostika a řešení potíží uzel nebo selhání clusteru, jak je znázorněno v následujícím fragmentu kódu hello.</span><span class="sxs-lookup"><span data-stu-id="e4e07-147">hello **diagnosticsStore** section allows you tooconfigure parameters tooenable diagnostics and troubleshooting node or cluster failures, as shown in hello following snippet.</span></span> 

    "diagnosticsStore": {
        "metadata":  "Please replace hello diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "FileShare",
        "IsEncrypted": "false",
        "connectionstring": "c:\\ProgramData\\SF\\DiagnosticsStore"
    }

<span data-ttu-id="e4e07-148">Hello **metadata** popis vašeho clusteru diagnostiky a může nastavit podle vašeho nastavení.</span><span class="sxs-lookup"><span data-stu-id="e4e07-148">hello **metadata** is a description of your cluster diagnostics and can be set as per your setup.</span></span> <span data-ttu-id="e4e07-149">Tyto proměnné pomáhají při shromažďování ETW protokolů trasování, výpisy stavu systému a také čítače výkonu.</span><span class="sxs-lookup"><span data-stu-id="e4e07-149">These variables help in collecting ETW trace logs, crash dumps as well as performance counters.</span></span> <span data-ttu-id="e4e07-150">Čtení [protokolu trasování](https://msdn.microsoft.com/library/windows/hardware/ff552994.aspx) a [trasování ETW](https://msdn.microsoft.com/library/ms751538.aspx) protokoly trasování pro další informace o trasování událostí pro Windows.</span><span class="sxs-lookup"><span data-stu-id="e4e07-150">Read [Tracelog](https://msdn.microsoft.com/library/windows/hardware/ff552994.aspx) and [ETW Tracing](https://msdn.microsoft.com/library/ms751538.aspx) for more information on ETW trace logs.</span></span> <span data-ttu-id="e4e07-151">Všechny protokoly, včetně [havárií výpisy](https://blogs.technet.microsoft.com/askperf/2008/01/08/understanding-crash-dump-files/) a [čítače výkonu](https://msdn.microsoft.com/library/windows/desktop/aa373083.aspx) může být směrovanou toohello **connectionString** složky v počítači.</span><span class="sxs-lookup"><span data-stu-id="e4e07-151">All logs including [Crash dumps](https://blogs.technet.microsoft.com/askperf/2008/01/08/understanding-crash-dump-files/) and [performance counters](https://msdn.microsoft.com/library/windows/desktop/aa373083.aspx) can be directed toohello **connectionString** folder on your machine.</span></span> <span data-ttu-id="e4e07-152">Můžete také použít *azurestorage* pro ukládání diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="e4e07-152">You can also use *AzureStorage* for storing diagnostics.</span></span> <span data-ttu-id="e4e07-153">Níže najdete fragment ukázka.</span><span class="sxs-lookup"><span data-stu-id="e4e07-153">See below for a sample snippet.</span></span>

    "diagnosticsStore": {
        "metadata":  "Please replace hello diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "AzureStorage",
        "IsEncrypted": "false",
        "connectionstring": "xstore:DefaultEndpointsProtocol=https;AccountName=[AzureAccountName];AccountKey=[AzureAccountKey]"
    }

### <a name="security"></a><span data-ttu-id="e4e07-154">Zabezpečení</span><span class="sxs-lookup"><span data-stu-id="e4e07-154">Security</span></span>
<span data-ttu-id="e4e07-155">Hello **zabezpečení** je nezbytné pro cluster Service Fabric zabezpečené samostatné části.</span><span class="sxs-lookup"><span data-stu-id="e4e07-155">hello **security** section is necessary for a secure standalone Service Fabric cluster.</span></span> <span data-ttu-id="e4e07-156">Hello následující fragment kódu ukazuje součástí této části.</span><span class="sxs-lookup"><span data-stu-id="e4e07-156">hello following snippet shows a part of this section.</span></span>

    "security": {
        "metadata": "This cluster is secured using X509 certificates.",
        "ClusterCredentialType": "X509",
        "ServerCredentialType": "X509",
        . . .
    }

<span data-ttu-id="e4e07-157">Hello **metadata** popis zabezpečení clusteru a může nastavit podle vašeho nastavení.</span><span class="sxs-lookup"><span data-stu-id="e4e07-157">hello **metadata** is a description of your secure cluster and can be set as per your setup.</span></span> <span data-ttu-id="e4e07-158">Hello **ClusterCredentialType** a **ServerCredentialType** určit typ hello zabezpečení, která implementuje hello cluster a uzly hello.</span><span class="sxs-lookup"><span data-stu-id="e4e07-158">hello **ClusterCredentialType** and **ServerCredentialType** determine hello type of security that hello cluster and hello nodes will implement.</span></span> <span data-ttu-id="e4e07-159">Lze nastavit tooeither *X509* pro zabezpečení na základě certifikátů nebo *Windows* pro zabezpečení služby založené na Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="e4e07-159">They can be set tooeither *X509* for a certificate-based security, or *Windows* for an Azure Active Directory-based security.</span></span> <span data-ttu-id="e4e07-160">Hello zbytek hello **zabezpečení** části budou založeny na typ hello hello zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="e4e07-160">hello rest of hello **security** section will be based on hello type of hello security.</span></span> <span data-ttu-id="e4e07-161">Čtení [zabezpečení na základě certifikátů v clusteru s podporou samostatné](service-fabric-windows-cluster-x509-security.md) nebo [zabezpečení systému Windows v clusteru s podporou samostatné](service-fabric-windows-cluster-windows-security.md) informace o způsobu toofill out hello zbytku hello **zabezpečení**části.</span><span class="sxs-lookup"><span data-stu-id="e4e07-161">Read [Certificates-based security in a standalone cluster](service-fabric-windows-cluster-x509-security.md) or [Windows security in a standalone cluster](service-fabric-windows-cluster-windows-security.md) for information on how toofill out hello rest of hello **security** section.</span></span>

<a id="nodetypes"></a>

### <a name="node-types"></a><span data-ttu-id="e4e07-162">Typy uzlů</span><span class="sxs-lookup"><span data-stu-id="e4e07-162">Node Types</span></span>
<span data-ttu-id="e4e07-163">Hello **nodeTypes** část popisuje typ hello hello uzlů, které má váš cluster.</span><span class="sxs-lookup"><span data-stu-id="e4e07-163">hello **nodeTypes** section describes hello type of hello nodes that your cluster has.</span></span> <span data-ttu-id="e4e07-164">Typ nejméně jeden uzel lze uvést pro cluster, jak je znázorněno v následující fragment hello.</span><span class="sxs-lookup"><span data-stu-id="e4e07-164">At least one node type must be specified for a cluster, as shown in hello snippet below.</span></span> 

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

<span data-ttu-id="e4e07-165">Hello **název** je hello popisný název pro tento typ konkrétním uzlu.</span><span class="sxs-lookup"><span data-stu-id="e4e07-165">hello **name** is hello friendly name for this particular node type.</span></span> <span data-ttu-id="e4e07-166">toocreate uzlu tento typ uzlu přiřadit jeho popisný název toohello **nodeTypeRef** proměnné pro tento uzel, jak je uvedeno [výše](#clusternodes).</span><span class="sxs-lookup"><span data-stu-id="e4e07-166">toocreate a node of this node type, assign its friendly name toohello **nodeTypeRef** variable for that node, as mentioned [above](#clusternodes).</span></span> <span data-ttu-id="e4e07-167">Pro každý typ uzlu definujte koncové body hello připojení, která se bude používat.</span><span class="sxs-lookup"><span data-stu-id="e4e07-167">For each node type, define hello connection endpoints that will be used.</span></span> <span data-ttu-id="e4e07-168">Jakékoli číslo portu pro tyto koncové body připojení můžete vybrat tak dlouho, dokud nedošlo ke konfliktu s žádné koncové body v tomto clusteru.</span><span class="sxs-lookup"><span data-stu-id="e4e07-168">You can choose any port number for these connection endpoints, as long as they do not conflict with any other endpoints in this cluster.</span></span> <span data-ttu-id="e4e07-169">V clusteru s několika uzly budou jeden nebo více primární uzlů (tj. **isPrimary** nastavit příliš*true*), v závislosti na hello [ **reliabilityLevel** ](#reliability).</span><span class="sxs-lookup"><span data-stu-id="e4e07-169">In a multi-node cluster, there will be one or more primary nodes (i.e. **isPrimary** set too*true*), depending on hello [**reliabilityLevel**](#reliability).</span></span> <span data-ttu-id="e4e07-170">Čtení [aspekty plánování kapacity služby cluster Service Fabric](service-fabric-cluster-capacity.md) informace o **nodeTypes** a **reliabilityLevel**a tooknow co jsou primární a hello typy uzel není primární.</span><span class="sxs-lookup"><span data-stu-id="e4e07-170">Read [Service Fabric cluster capacity planning considerations](service-fabric-cluster-capacity.md) for information on **nodeTypes** and **reliabilityLevel**, and tooknow what are primary and hello non-primary node types.</span></span> 

#### <a name="endpoints-used-tooconfigure-hello-node-types"></a><span data-ttu-id="e4e07-171">Koncové body používá typy uzlů tooconfigure hello</span><span class="sxs-lookup"><span data-stu-id="e4e07-171">Endpoints used tooconfigure hello node types</span></span>
* <span data-ttu-id="e4e07-172">*clientConnectionEndpointPort* je port hello používá hello klienta tooconnect toohello cluster, při použití rozhraní API klienta hello.</span><span class="sxs-lookup"><span data-stu-id="e4e07-172">*clientConnectionEndpointPort* is hello port used by hello client tooconnect toohello cluster, when using hello client APIs.</span></span> 
* <span data-ttu-id="e4e07-173">*clusterConnectionEndpointPort* je hello port, ve kterém uzly hello vzájemně komunikovat.</span><span class="sxs-lookup"><span data-stu-id="e4e07-173">*clusterConnectionEndpointPort* is hello port at which hello nodes communicate with each other.</span></span>
* <span data-ttu-id="e4e07-174">*leaseDriverEndpointPort* je hello port používaný hello clusteru zapůjčení ovladač toofind out, pokud jsou stále aktivní hello uzly.</span><span class="sxs-lookup"><span data-stu-id="e4e07-174">*leaseDriverEndpointPort* is hello port used by hello cluster lease driver toofind out if hello nodes are still active.</span></span> 
* <span data-ttu-id="e4e07-175">*serviceConnectionEndpointPort* je hello port je používán hello aplikacím a službám nasazeným v uzlu, toocommunicate s klientem hello Service Fabric na příslušném uzlu.</span><span class="sxs-lookup"><span data-stu-id="e4e07-175">*serviceConnectionEndpointPort* is hello port used by hello applications and services deployed on a node, toocommunicate with hello Service Fabric client on that particular node.</span></span>
* <span data-ttu-id="e4e07-176">*httpGatewayEndpointPort* je port hello používá hello Service Fabric Explorer tooconnect toohello cluster.</span><span class="sxs-lookup"><span data-stu-id="e4e07-176">*httpGatewayEndpointPort* is hello port used by hello Service Fabric Explorer tooconnect toohello cluster.</span></span>
* <span data-ttu-id="e4e07-177">*ephemeralPorts* přepsat hello [dynamické porty používané hello OS](https://support.microsoft.com/kb/929851).</span><span class="sxs-lookup"><span data-stu-id="e4e07-177">*ephemeralPorts* override hello [dynamic ports used by hello OS](https://support.microsoft.com/kb/929851).</span></span> <span data-ttu-id="e4e07-178">Service Fabric bude používat jako porty aplikace součástí a zbývající hello bude k dispozici pro hello operačního systému.</span><span class="sxs-lookup"><span data-stu-id="e4e07-178">Service Fabric will use a part of these as application ports and hello remaining will be available for hello OS.</span></span> <span data-ttu-id="e4e07-179">Tento rozsah toohello existující rozsah součástí hello operačního systému, také mapuje proto pro všechny účely, můžete použít hello rozsahy zadané v souborech JSON ukázka hello.</span><span class="sxs-lookup"><span data-stu-id="e4e07-179">It will also map this range toohello existing range present in hello OS, so for all purposes, you can use hello ranges given in hello sample JSON files.</span></span> <span data-ttu-id="e4e07-180">Je nutné toomake se, že hello rozdíl mezi počáteční hello hello porty a end je alespoň 255.</span><span class="sxs-lookup"><span data-stu-id="e4e07-180">You need toomake sure that hello difference between hello start and hello end ports is at least 255.</span></span> <span data-ttu-id="e4e07-181">Do konfliktu se může spustit, pokud tento rozdíl je příliš nízká, protože tento rozsah je sdílena s hello operačního systému.</span><span class="sxs-lookup"><span data-stu-id="e4e07-181">You may run into conflicts if this difference is too low, since this range is shared with hello operating system.</span></span> <span data-ttu-id="e4e07-182">V tématu hello nakonfigurované dynamický rozsah portů spuštěním `netsh int ipv4 show dynamicport tcp`.</span><span class="sxs-lookup"><span data-stu-id="e4e07-182">See hello configured dynamic port range by running `netsh int ipv4 show dynamicport tcp`.</span></span>
* <span data-ttu-id="e4e07-183">*applicationPorts* hello porty, které se použijí aplikace Service Fabric hello.</span><span class="sxs-lookup"><span data-stu-id="e4e07-183">*applicationPorts* are hello ports that will be used by hello Service Fabric applications.</span></span> <span data-ttu-id="e4e07-184">rozsah portů aplikace Hello by měl být dostatečně velký toocover požadavek na koncový bod hello aplikací.</span><span class="sxs-lookup"><span data-stu-id="e4e07-184">hello application port range should be large enough toocover hello endpoint requirement of your applications.</span></span> <span data-ttu-id="e4e07-185">Tento rozsah by měl být výhradní z hello dynamický rozsah portů na počítači hello, tj. hello *ephemeralPorts* rozsah nastavený v konfiguraci hello.</span><span class="sxs-lookup"><span data-stu-id="e4e07-185">This range should be exclusive from hello dynamic port range on hello machine, i.e. hello *ephemeralPorts* range as set in hello configuration.</span></span>  <span data-ttu-id="e4e07-186">Service Fabric se použít vždy, když nové porty jsou povinné, jakož i postará o otevření hello brány firewall pro tyto porty.</span><span class="sxs-lookup"><span data-stu-id="e4e07-186">Service Fabric will use these whenever new ports are required, as well as take care of opening hello firewall for these ports.</span></span> 
* <span data-ttu-id="e4e07-187">*reverseProxyEndpointPort* je koncový bod volitelné reverzní proxy server.</span><span class="sxs-lookup"><span data-stu-id="e4e07-187">*reverseProxyEndpointPort* is an optional reverse proxy endpoint.</span></span> <span data-ttu-id="e4e07-188">V tématu [Service Fabric Reverse Proxy](service-fabric-reverseproxy.md) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="e4e07-188">See [Service Fabric Reverse Proxy](service-fabric-reverseproxy.md) for more details.</span></span> 

### <a name="log-settings"></a><span data-ttu-id="e4e07-189">Nastavení protokolu</span><span class="sxs-lookup"><span data-stu-id="e4e07-189">Log Settings</span></span>
<span data-ttu-id="e4e07-190">Hello **fabricSettings** část vám tooset hello kořenové adresáře pro data hello Service Fabric a protokoly.</span><span class="sxs-lookup"><span data-stu-id="e4e07-190">hello **fabricSettings** section allows you tooset hello root directories for hello Service Fabric data and logs.</span></span> <span data-ttu-id="e4e07-191">Můžete přizpůsobit tyto pouze při vytváření počáteční clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="e4e07-191">You can customize these only during hello initial cluster creation.</span></span> <span data-ttu-id="e4e07-192">Ukázka fragmentu v této části jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="e4e07-192">See below for a sample snippet of this section.</span></span>

    "fabricSettings": [{
        "name": "Setup",
        "parameters": [{
            "name": "FabricDataRoot",
            "value": "C:\\ProgramData\\SF"
        }, {
            "name": "FabricLogRoot",
            "value": "C:\\ProgramData\\SF\\Log"
    }]

<span data-ttu-id="e4e07-193">Doporučujeme pomocí jednotce operačního systému jako hello proměnná FabricDataRoot a adresáři FabricLogRoot, poskytuje další spolehlivost proti dojde k chybě operačního systému.</span><span class="sxs-lookup"><span data-stu-id="e4e07-193">We recommended using a non-OS drive as hello FabricDataRoot and FabricLogRoot as it provides more reliability against OS crashes.</span></span> <span data-ttu-id="e4e07-194">Všimněte si, že pokud přizpůsobíte pouze kořenové datové hello, pak hello protokolu kořenové bude umístěna o jednu úroveň pod kořenové datové hello.</span><span class="sxs-lookup"><span data-stu-id="e4e07-194">Note that if you customize only hello data root, then hello log root will be placed one level below hello data root.</span></span>

### <a name="stateful-reliable-service-settings"></a><span data-ttu-id="e4e07-195">Nastavení stavové spolehlivé služby</span><span class="sxs-lookup"><span data-stu-id="e4e07-195">Stateful Reliable Service Settings</span></span>
<span data-ttu-id="e4e07-196">Hello **KtlLogger** část vám tooset hello globální nastavení konfigurace pro spolehlivé služby.</span><span class="sxs-lookup"><span data-stu-id="e4e07-196">hello **KtlLogger** section allows you tooset hello global configuration settings for Reliable Services.</span></span> <span data-ttu-id="e4e07-197">Další informace o těchto nastaveních najdete [konfigurovat stavová spolehlivé služby](service-fabric-reliable-services-configuration.md).</span><span class="sxs-lookup"><span data-stu-id="e4e07-197">For more details on these settings read [Configure stateful reliable services](service-fabric-reliable-services-configuration.md).</span></span>
<span data-ttu-id="e4e07-198">Následující příklad Hello ukazuje, jak toochange hello hello sdílené transakčního protokolu, který získá vytváří tooback všechny spolehlivé kolekce pro stavové služby.</span><span class="sxs-lookup"><span data-stu-id="e4e07-198">hello example below shows how toochange hello hello shared transaction log that gets created tooback any reliable collections for stateful services.</span></span>

    "fabricSettings": [{
        "name": "KtlLogger",
        "parameters": [{
            "name": "SharedLogSizeInMB",
            "value": "4096"
        }]
    }]

### <a name="add-on-features"></a><span data-ttu-id="e4e07-199">Funkce rozšíření</span><span class="sxs-lookup"><span data-stu-id="e4e07-199">Add-on features</span></span>
<span data-ttu-id="e4e07-200">Funkce rozšíření tooconfigure, hello apiVersion musí být nakonfigurován jako "04-2017' nebo vyšší a addonFeatures musí toobe nakonfigurovat:</span><span class="sxs-lookup"><span data-stu-id="e4e07-200">tooconfigure add-on features, hello apiVersion should be configured as '04-2017' or higher, and addonFeatures needs toobe configured:</span></span>

    "apiVersion": "04-2017",
    "properties": {
      "addOnFeatures": [
          "DnsService",
          "RepairManager"
      ]
    }

### <a name="container-support"></a><span data-ttu-id="e4e07-201">Podpora kontejnerů</span><span class="sxs-lookup"><span data-stu-id="e4e07-201">Container support</span></span>
<span data-ttu-id="e4e07-202">Podpora kontejnerů tooenable pro windows server kontejneru a technologie hyper-v kontejneru pro samostatné clustery, musí hello 'DnsService' rozšíření funkce toobe povolena.</span><span class="sxs-lookup"><span data-stu-id="e4e07-202">tooenable container support for both windows server container and hyper-v container for standalone clusters, hello 'DnsService' add-on feature needs toobe enabled.</span></span>


## <a name="next-steps"></a><span data-ttu-id="e4e07-203">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e4e07-203">Next steps</span></span>
<span data-ttu-id="e4e07-204">Až budete mít soubor dokončení souboru ClusterConfig.JSON nakonfigurovaný podle vaší samostatné nastavení clusteru, můžete nasadit cluster následující článek hello [vytvořit cluster Service Fabric samostatné](service-fabric-cluster-creation-for-windows-server.md) a poté pokračujte příliš[vizualizace vašeho clusteru pomocí Service Fabric Exploreru](service-fabric-visualizing-your-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="e4e07-204">Once you have a complete ClusterConfig.JSON file configured as per your standalone cluster setup, you can deploy your cluster by following hello article [Create a standalone Service Fabric cluster](service-fabric-cluster-creation-for-windows-server.md) and then proceed too[visualizing your cluster with Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).</span></span>

