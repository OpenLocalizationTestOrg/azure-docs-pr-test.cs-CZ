---
title: Azure Service Fabric reverse proxy | Microsoft Docs
description: "Pro komunikaci mikroslužeb z uvnitř i vně clusteru pomocí Service Fabric reverzní proxy server."
services: service-fabric
documentationcenter: .net
author: BharatNarasimman
manager: timlt
editor: vturecek
ms.assetid: 47f5c1c1-8fc8-4b80-a081-bc308f3655d3
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 08/08/2017
ms.author: bharatn
ms.openlocfilehash: 7897458e9e4a0bbe185bd3f7b4c133c1b26769f9
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="reverse-proxy-in-azure-service-fabric"></a><span data-ttu-id="f4757-103">Reverzní proxy server v Azure Service Fabric</span><span class="sxs-lookup"><span data-stu-id="f4757-103">Reverse proxy in Azure Service Fabric</span></span>
<span data-ttu-id="f4757-104">Reverzní proxy server, která je integrována do Azure Service Fabric řeší mikroslužeb v clusteru Service Fabric, která zveřejňuje koncových bodů protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="f4757-104">The reverse proxy that's built into Azure Service Fabric addresses microservices in the Service Fabric cluster that exposes HTTP endpoints.</span></span>

## <a name="microservices-communication-model"></a><span data-ttu-id="f4757-105">Mikroslužeb komunikační model</span><span class="sxs-lookup"><span data-stu-id="f4757-105">Microservices communication model</span></span>
<span data-ttu-id="f4757-106">Mikroslužeb v Service Fabric obvykle spustit na podmnožinu virtuální počítače v clusteru a můžete přesunout z jednoho virtuálního počítače do druhého z různých důvodů.</span><span class="sxs-lookup"><span data-stu-id="f4757-106">Microservices in Service Fabric typically run on a subset of virtual machines in the cluster and can move from one virtual machine to another for various reasons.</span></span> <span data-ttu-id="f4757-107">Koncové body pro mikroslužeb Ano, můžete změnit dynamicky.</span><span class="sxs-lookup"><span data-stu-id="f4757-107">So, the endpoints for microservices can change dynamically.</span></span> <span data-ttu-id="f4757-108">Typický vzor pro komunikaci se mikroslužbu je následující zacyklení vyřešit:</span><span class="sxs-lookup"><span data-stu-id="f4757-108">The typical pattern to communicate to the microservice is the following resolve loop:</span></span>

1. <span data-ttu-id="f4757-109">Určení umístění služby původně prostřednictvím pojmenování služby.</span><span class="sxs-lookup"><span data-stu-id="f4757-109">Resolve the service location initially through the naming service.</span></span>
2. <span data-ttu-id="f4757-110">Připojte ke službě.</span><span class="sxs-lookup"><span data-stu-id="f4757-110">Connect to the service.</span></span>
3. <span data-ttu-id="f4757-111">Zjistěte příčinu selhání připojení a znovu přeložit umístění služby, pokud je to nezbytné.</span><span class="sxs-lookup"><span data-stu-id="f4757-111">Determine the cause of connection failures, and resolve the service location again when necessary.</span></span>

<span data-ttu-id="f4757-112">Tento proces obvykle zahrnuje zabalení knihovny komunikace klienta ve smyčce opakování, který implementuje zásady služby řešení a zkuste to znovu.</span><span class="sxs-lookup"><span data-stu-id="f4757-112">This process generally involves wrapping client-side communication libraries in a retry loop that implements the service resolution and retry policies.</span></span>
<span data-ttu-id="f4757-113">Další informace najdete v tématu [připojit a komunikovat se službami](service-fabric-connect-and-communicate-with-services.md).</span><span class="sxs-lookup"><span data-stu-id="f4757-113">For more information, see [Connect and communicate with services](service-fabric-connect-and-communicate-with-services.md).</span></span>

### <a name="communicating-by-using-the-reverse-proxy"></a><span data-ttu-id="f4757-114">Komunikaci pomocí reverzní proxy server</span><span class="sxs-lookup"><span data-stu-id="f4757-114">Communicating by using the reverse proxy</span></span>
<span data-ttu-id="f4757-115">Reverzní proxy server v Service Fabric se spustí na všech uzlech v clusteru.</span><span class="sxs-lookup"><span data-stu-id="f4757-115">The reverse proxy in Service Fabric runs on all the nodes in the cluster.</span></span> <span data-ttu-id="f4757-116">Ji provede procesu překladu, který celé služby jménem klienta a pak předá požadavek klienta.</span><span class="sxs-lookup"><span data-stu-id="f4757-116">It performs the entire service resolution process on a client's behalf and then forwards the client request.</span></span> <span data-ttu-id="f4757-117">Klienti, kteří běží na clusteru tak, můžete použít všechny knihovny komunikace HTTP klienta ke komunikaci s cílovou službu pomocí reverzní proxy server, který spustí místně na stejném uzlu.</span><span class="sxs-lookup"><span data-stu-id="f4757-117">So, clients that run on the cluster can use any client-side HTTP communication libraries to talk to the target service by using the reverse proxy that runs locally on the same node.</span></span>

![Interní komunikaci][1]

## <a name="reaching-microservices-from-outside-the-cluster"></a><span data-ttu-id="f4757-119">Dosažení mikroslužeb z mimo cluster</span><span class="sxs-lookup"><span data-stu-id="f4757-119">Reaching microservices from outside the cluster</span></span>
<span data-ttu-id="f4757-120">Výchozí model externí komunikace pro mikroslužeb je model opt-in, kde každé služby nelze přistupovat přímo z externích klientů.</span><span class="sxs-lookup"><span data-stu-id="f4757-120">The default external communication model for microservices is an opt-in model where each service cannot be accessed directly from external clients.</span></span> <span data-ttu-id="f4757-121">[Azure nástroj pro vyrovnávání zatížení](../load-balancer/load-balancer-overview.md), což je hranici sítě mezi mikroslužeb a externími klienty, provádí překlad síťových adres a předává externí požadavky do koncových bodů interní IP: port.</span><span class="sxs-lookup"><span data-stu-id="f4757-121">[Azure Load Balancer](../load-balancer/load-balancer-overview.md), which is a network boundary between microservices and external clients, performs network address translation and forwards external requests to internal IP:port endpoints.</span></span> <span data-ttu-id="f4757-122">Chcete-li koncový bod mikroslužbu přímo přístupné pro externími klienty, musíte nejdřív nakonfigurovat nástroj pro vyrovnávání zatížení pro přenos dat na každý port, který služba používá v clusteru.</span><span class="sxs-lookup"><span data-stu-id="f4757-122">To make a microservice's endpoint directly accessible to external clients, you must first configure Load Balancer to forward traffic to each port that the service uses in the cluster.</span></span> <span data-ttu-id="f4757-123">Kromě toho většina mikroslužeb, zejména stavová mikroslužeb, nemáte live na všech uzlech clusteru.</span><span class="sxs-lookup"><span data-stu-id="f4757-123">Furthermore, most microservices, especially stateful microservices, don't live on all nodes of the cluster.</span></span> <span data-ttu-id="f4757-124">Mikroslužeb můžete přesouvat mezi uzly na převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="f4757-124">The microservices can move between nodes on failover.</span></span> <span data-ttu-id="f4757-125">V takových případech nástroj pro vyrovnávání zatížení nemůže zjistit efektivně umístění cílový uzel replik, na které by měla předávat provoz.</span><span class="sxs-lookup"><span data-stu-id="f4757-125">In such cases, Load Balancer cannot effectively determine the location of the target node of the replicas to which it should forward traffic.</span></span>

### <a name="reaching-microservices-via-the-reverse-proxy-from-outside-the-cluster"></a><span data-ttu-id="f4757-126">Dosažení mikroslužeb přes reverzní proxy server z mimo cluster</span><span class="sxs-lookup"><span data-stu-id="f4757-126">Reaching microservices via the reverse proxy from outside the cluster</span></span>
<span data-ttu-id="f4757-127">Namísto konfigurace port jednotlivé služby nástroji pro vyrovnávání zatížení, můžete nakonfigurovat pouze port reverzní proxy server nástroji pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="f4757-127">Instead of configuring the port of an individual service in Load Balancer, you can configure just the port of the reverse proxy in Load Balancer.</span></span> <span data-ttu-id="f4757-128">Tato konfigurace umožňuje klientům mimo cluster používat služby v clusteru pomocí reverzní proxy server bez další konfigurace.</span><span class="sxs-lookup"><span data-stu-id="f4757-128">This configuration lets clients outside the cluster reach services inside the cluster by using the reverse proxy without additional configuration.</span></span>

![Externí komunikace][0]

> [!WARNING]
> <span data-ttu-id="f4757-130">Při konfiguraci portu reverzní proxy server v nástroj pro vyrovnávání zatížení, všechny mikroslužeb v clusteru, které zveřejňují koncový bod protokolu HTTP jsou adresovatelné z mimo cluster.</span><span class="sxs-lookup"><span data-stu-id="f4757-130">When you configure the reverse proxy's port in Load Balancer, all microservices in the cluster that expose an HTTP endpoint are addressable from outside the cluster.</span></span>
>
>


## <a name="uri-format-for-addressing-services-by-using-the-reverse-proxy"></a><span data-ttu-id="f4757-131">Formát identifikátoru URI pro adresování služby pomocí reverzní proxy server</span><span class="sxs-lookup"><span data-stu-id="f4757-131">URI format for addressing services by using the reverse proxy</span></span>
<span data-ttu-id="f4757-132">Reverzní proxy server používá formát identifikátor URI konkrétní URI k identifikaci oddílu služby, ke kterému má být příchozí žádost předána:</span><span class="sxs-lookup"><span data-stu-id="f4757-132">The reverse proxy uses a specific uniform resource identifier (URI) format to identify the service partition to which the incoming request should be forwarded:</span></span>

```
http(s)://<Cluster FQDN | internal IP>:Port/<ServiceInstanceName>/<Suffix path>?PartitionKey=<key>&PartitionKind=<partitionkind>&ListenerName=<listenerName>&TargetReplicaSelector=<targetReplicaSelector>&Timeout=<timeout_in_seconds>
```

* <span data-ttu-id="f4757-133">**http (s):** reverzní proxy server lze nakonfigurovat, aby přijímal provoz protokolu HTTP nebo HTTPS.</span><span class="sxs-lookup"><span data-stu-id="f4757-133">**http(s):** The reverse proxy can be configured to accept HTTP or HTTPS traffic.</span></span> <span data-ttu-id="f4757-134">Předávání protokolu HTTPS, najdete v části [připojení k službě zabezpečené pomocí reverzní proxy server](service-fabric-reverseproxy-configure-secure-communication.md) až budete mít instalace reverzní proxy server tak, aby naslouchala na HTTPS.</span><span class="sxs-lookup"><span data-stu-id="f4757-134">For HTTPS forwarding, refer to [Connect to a secure service with the reverse proxy](service-fabric-reverseproxy-configure-secure-communication.md) once you have reverse proxy setup to listen on HTTPS.</span></span>
* <span data-ttu-id="f4757-135">**Cluster plně kvalifikovaný název domény (FQDN) | interní IP:** pro externí klienty můžete nakonfigurovat reverzní proxy server tak, aby byla dostupná prostřednictvím doména clusteru, jako je například mycluster.eastus.cloudapp.azure.com. Ve výchozím nastavení spouští reverzní proxy server na každý uzel.</span><span class="sxs-lookup"><span data-stu-id="f4757-135">**Cluster fully qualified domain name (FQDN) | internal IP:** For external clients, you can configure the reverse proxy so that it is reachable through the cluster domain, such as mycluster.eastus.cloudapp.azure.com. By default, the reverse proxy runs on every node.</span></span> <span data-ttu-id="f4757-136">Pro interní provoz reverzní proxy server dostupný na místního hostitele nebo na všechny IP adresy pro interní uzlu, například 10.0.0.1.</span><span class="sxs-lookup"><span data-stu-id="f4757-136">For internal traffic, the reverse proxy can be reached on localhost or on any internal node IP, such as 10.0.0.1.</span></span>
* <span data-ttu-id="f4757-137">**Port:** je to port, jako je například 19081, který byl zadaný pro reverzní proxy server.</span><span class="sxs-lookup"><span data-stu-id="f4757-137">**Port:** This is the port, such as 19081, that has been specified for the reverse proxy.</span></span>
* <span data-ttu-id="f4757-138">**ServiceInstanceName:** Toto je plně kvalifikovaný název instance nasazené služby, který se pokoušíte získat přístup bez "fabric: /" schéma.</span><span class="sxs-lookup"><span data-stu-id="f4757-138">**ServiceInstanceName:** This is the fully-qualified name of the deployed service instance that you are trying to reach without the "fabric:/" scheme.</span></span> <span data-ttu-id="f4757-139">Například pro přístup *fabric: / myapp/Moje_služba/* využije služby, *myapp/Moje_služba*.</span><span class="sxs-lookup"><span data-stu-id="f4757-139">For example, to reach the *fabric:/myapp/myservice/* service, you would use *myapp/myservice*.</span></span>

    <span data-ttu-id="f4757-140">Název instance služby je malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="f4757-140">The service instance name is case-sensitive.</span></span> <span data-ttu-id="f4757-141">Pomocí velká a malá písmena jinou pro název instance služby v adrese URL způsobí, že žádosti, které chcete selhat s 404 (není nalezena).</span><span class="sxs-lookup"><span data-stu-id="f4757-141">Using a different casing for the service instance name in the URL causes the requests to fail with 404 (Not Found).</span></span>
* <span data-ttu-id="f4757-142">**Přípona cesty:** jde skutečné cesty URL, například *myapi/hodnoty/přidat/3*, služby, který chcete připojit k.</span><span class="sxs-lookup"><span data-stu-id="f4757-142">**Suffix path:** This is the actual URL path, such as *myapi/values/add/3*, for the service that you want to connect to.</span></span>
* <span data-ttu-id="f4757-143">**Klíč oddílu:** oddílů služby, jedná se o počítaný oddíl klíč oddílu, který chcete připojit.</span><span class="sxs-lookup"><span data-stu-id="f4757-143">**PartitionKey:** For a partitioned service, this is the computed partition key of the partition that you want to reach.</span></span> <span data-ttu-id="f4757-144">Všimněte si, že toto je *není* identifikátoru GUID ID oddílu.</span><span class="sxs-lookup"><span data-stu-id="f4757-144">Note that this is *not* the partition ID GUID.</span></span> <span data-ttu-id="f4757-145">Tento parametr není vyžadována pro služby, které používají schéma oddílu typu singleton.</span><span class="sxs-lookup"><span data-stu-id="f4757-145">This parameter is not required for services that use the singleton partition scheme.</span></span>
* <span data-ttu-id="f4757-146">**PartitionKind:** Toto je schéma oddílu služby.</span><span class="sxs-lookup"><span data-stu-id="f4757-146">**PartitionKind:** This is the service partition scheme.</span></span> <span data-ttu-id="f4757-147">To může být 'Int64Range' nebo "S názvem".</span><span class="sxs-lookup"><span data-stu-id="f4757-147">This can be 'Int64Range' or 'Named'.</span></span> <span data-ttu-id="f4757-148">Tento parametr není vyžadována pro služby, které používají schéma oddílu typu singleton.</span><span class="sxs-lookup"><span data-stu-id="f4757-148">This parameter is not required for services that use the singleton partition scheme.</span></span>
* <span data-ttu-id="f4757-149">**ListenerName** koncových bodů ze služby jsou ve formátu {"Koncové body": {"Listener1": "Koncovém bodě 1", "Listener2": "Endpoint2"...}}.</span><span class="sxs-lookup"><span data-stu-id="f4757-149">**ListenerName** The endpoints from the service are of the form {"Endpoints":{"Listener1":"Endpoint1","Listener2":"Endpoint2" ...}}.</span></span> <span data-ttu-id="f4757-150">Pokud je služba poskytuje více koncových bodů, tato identifikuje koncového bodu, který požadavek klienta by měl být předán.</span><span class="sxs-lookup"><span data-stu-id="f4757-150">When the service exposes multiple endpoints, this identifies the endpoint that the client request should be forwarded to.</span></span> <span data-ttu-id="f4757-151">To lze vynechat, pokud služba obsahuje pouze jeden naslouchací proces.</span><span class="sxs-lookup"><span data-stu-id="f4757-151">This can be omitted if the service has only one listener.</span></span>
* <span data-ttu-id="f4757-152">**TargetReplicaSelector** určuje jak měla by být vybrána cíl replik nebo instancí.</span><span class="sxs-lookup"><span data-stu-id="f4757-152">**TargetReplicaSelector** This specifies how the target replica or instance should be selected.</span></span>
  * <span data-ttu-id="f4757-153">Po stavová cílovou službu TargetReplicaSelector může být jedna z následujících: 'PrimaryReplica', 'RandomSecondaryReplica' nebo 'RandomReplica'.</span><span class="sxs-lookup"><span data-stu-id="f4757-153">When the target service is stateful, the TargetReplicaSelector can be one of the following:  'PrimaryReplica', 'RandomSecondaryReplica', or 'RandomReplica'.</span></span> <span data-ttu-id="f4757-154">Pokud není tento parametr zadán, výchozí hodnota je 'PrimaryReplica'.</span><span class="sxs-lookup"><span data-stu-id="f4757-154">When this parameter is not specified, the default is 'PrimaryReplica'.</span></span>
  * <span data-ttu-id="f4757-155">Po bezstavové Cílová služba reverzní proxy server vybere náhodných instance oddílu služby k předání požadavku.</span><span class="sxs-lookup"><span data-stu-id="f4757-155">When the target service is stateless, reverse proxy picks a random instance of the service partition to forward the request to.</span></span>
* <span data-ttu-id="f4757-156">**Časový limit:** Určuje časový limit pro požadavek HTTP, které jsou vytvořené reverzní proxy server služby jménem žádost klienta.</span><span class="sxs-lookup"><span data-stu-id="f4757-156">**Timeout:**  This specifies the timeout for the HTTP request created by the reverse proxy to the service on behalf of the client request.</span></span> <span data-ttu-id="f4757-157">Výchozí hodnota je 60 sekund.</span><span class="sxs-lookup"><span data-stu-id="f4757-157">The default value is 60 seconds.</span></span> <span data-ttu-id="f4757-158">Toto je volitelný parametr.</span><span class="sxs-lookup"><span data-stu-id="f4757-158">This is an optional parameter.</span></span>

### <a name="example-usage"></a><span data-ttu-id="f4757-159">Příklad použití</span><span class="sxs-lookup"><span data-stu-id="f4757-159">Example usage</span></span>
<span data-ttu-id="f4757-160">Jako příklad Podívejme *fabric: / MyApp/Moje_služba* služby, které se otevře naslouchací proces protokolu HTTP na následující adresu URL:</span><span class="sxs-lookup"><span data-stu-id="f4757-160">As an example, let's take the *fabric:/MyApp/MyService* service that opens an HTTP listener on the following URL:</span></span>

```
http://10.0.0.5:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/
```

<span data-ttu-id="f4757-161">Prostředky pro službu jsou následující:</span><span class="sxs-lookup"><span data-stu-id="f4757-161">Following are the resources for the service:</span></span>

* `/index.html`
* `/api/users/<userId>`

<span data-ttu-id="f4757-162">Pokud služba používá schéma, vytváření oddílů singleton *PartitionKey* a *PartitionKind* parametrů řetězce dotazu nejsou vyžadovány, a službu lze získat přístup pomocí brány jako:</span><span class="sxs-lookup"><span data-stu-id="f4757-162">If the service uses the singleton partitioning scheme, the *PartitionKey* and *PartitionKind* query string parameters are not required, and the service can be reached by using the gateway as:</span></span>

* <span data-ttu-id="f4757-163">Externě:`http://mycluster.eastus.cloudapp.azure.com:19081/MyApp/MyService`</span><span class="sxs-lookup"><span data-stu-id="f4757-163">Externally: `http://mycluster.eastus.cloudapp.azure.com:19081/MyApp/MyService`</span></span>
* <span data-ttu-id="f4757-164">Interně:`http://localhost:19081/MyApp/MyService`</span><span class="sxs-lookup"><span data-stu-id="f4757-164">Internally: `http://localhost:19081/MyApp/MyService`</span></span>

<span data-ttu-id="f4757-165">Pokud služba používá schéma rozdělení oddílů Uniform Int64 *PartitionKey* a *PartitionKind* parametrů řetězce dotazu, musí být použité k dosažení oddílu služby:</span><span class="sxs-lookup"><span data-stu-id="f4757-165">If the service uses the Uniform Int64 partitioning scheme, the *PartitionKey* and *PartitionKind* query string parameters must be used to reach a partition of the service:</span></span>

* <span data-ttu-id="f4757-166">Externě:`http://mycluster.eastus.cloudapp.azure.com:19081/MyApp/MyService?PartitionKey=3&PartitionKind=Int64Range`</span><span class="sxs-lookup"><span data-stu-id="f4757-166">Externally: `http://mycluster.eastus.cloudapp.azure.com:19081/MyApp/MyService?PartitionKey=3&PartitionKind=Int64Range`</span></span>
* <span data-ttu-id="f4757-167">Interně:`http://localhost:19081/MyApp/MyService?PartitionKey=3&PartitionKind=Int64Range`</span><span class="sxs-lookup"><span data-stu-id="f4757-167">Internally: `http://localhost:19081/MyApp/MyService?PartitionKey=3&PartitionKind=Int64Range`</span></span>

<span data-ttu-id="f4757-168">K dosažení prostředky, které poskytuje službu, jednoduše umístíte cesta prostředku po názvu služby v adrese URL:</span><span class="sxs-lookup"><span data-stu-id="f4757-168">To reach the resources that the service exposes, simply place the resource path after the service name in the URL:</span></span>

* <span data-ttu-id="f4757-169">Externě:`http://mycluster.eastus.cloudapp.azure.com:19081/MyApp/MyService/index.html?PartitionKey=3&PartitionKind=Int64Range`</span><span class="sxs-lookup"><span data-stu-id="f4757-169">Externally: `http://mycluster.eastus.cloudapp.azure.com:19081/MyApp/MyService/index.html?PartitionKey=3&PartitionKind=Int64Range`</span></span>
* <span data-ttu-id="f4757-170">Interně:`http://localhost:19081/MyApp/MyService/api/users/6?PartitionKey=3&PartitionKind=Int64Range`</span><span class="sxs-lookup"><span data-stu-id="f4757-170">Internally: `http://localhost:19081/MyApp/MyService/api/users/6?PartitionKey=3&PartitionKind=Int64Range`</span></span>

<span data-ttu-id="f4757-171">Brána pak předá tyto požadavky na adresu URL služby:</span><span class="sxs-lookup"><span data-stu-id="f4757-171">The gateway will then forward these requests to the service's URL:</span></span>

* `http://10.0.0.5:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/index.html`
* `http://10.0.0.5:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/api/users/6`

## <a name="special-handling-for-port-sharing-services"></a><span data-ttu-id="f4757-172">Zvláštní zpracování pro sdílení portů služby</span><span class="sxs-lookup"><span data-stu-id="f4757-172">Special handling for port-sharing services</span></span>
<span data-ttu-id="f4757-173">Služba Azure Application Gateway se pokusí znovu vyřešte adresu služby a opakovat žádost, pokud služba není dostupný.</span><span class="sxs-lookup"><span data-stu-id="f4757-173">Azure Application Gateway attempts to resolve a service address again and retry the request when a service cannot be reached.</span></span> <span data-ttu-id="f4757-174">Je to hlavní výhodou Application Gateway, proto kód klienta není nutné implementovat vlastní řešení služby a vyřešit smyčky.</span><span class="sxs-lookup"><span data-stu-id="f4757-174">This is a major benefit of Application Gateway because client code does not need to implement its own service resolution and resolve loop.</span></span>

<span data-ttu-id="f4757-175">Obecně platí, pokud služba není dostupný, instance služby ani repliky se přesunul na jiný uzel jako součást životního cyklu normální.</span><span class="sxs-lookup"><span data-stu-id="f4757-175">Generally, when a service cannot be reached, the service instance or replica has moved to a different node as part of its normal lifecycle.</span></span> <span data-ttu-id="f4757-176">V takovém případě Application Gateway může dojít k chybě připojení sítě označující, že koncový bod je již otevřen v původně převedenou adresu.</span><span class="sxs-lookup"><span data-stu-id="f4757-176">When this happens, Application Gateway might receive a network connection error indicating that an endpoint is no longer open on the originally resolved address.</span></span>

<span data-ttu-id="f4757-177">Ale replik nebo instancí služby můžete sdílet hostitelský proces a může také sdílet port při hostované na základě ovladače http.sys webového serveru, včetně:</span><span class="sxs-lookup"><span data-stu-id="f4757-177">However, replicas or service instances can share a host process and might also share a port when hosted by an http.sys-based web server, including:</span></span>

* [<span data-ttu-id="f4757-178">System.Net.HttpListener</span><span class="sxs-lookup"><span data-stu-id="f4757-178">System.Net.HttpListener</span></span>](https://msdn.microsoft.com/library/system.net.httplistener%28v=vs.110%29.aspx)
* [<span data-ttu-id="f4757-179">WebListener ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f4757-179">ASP.NET Core WebListener</span></span>](https://docs.asp.net/latest/fundamentals/servers.html#weblistener)
* [<span data-ttu-id="f4757-180">Katana</span><span class="sxs-lookup"><span data-stu-id="f4757-180">Katana</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.OwinSelfHost/)

<span data-ttu-id="f4757-181">V takovém případě je pravděpodobné, že webový server je k dispozici v procesu hostitele a reagovat na požadavky, ale instance přeložit služby nebo replika již není dostupný na hostiteli.</span><span class="sxs-lookup"><span data-stu-id="f4757-181">In this situation, it is likely that the web server is available in the host process and responding to requests, but the resolved service instance or replica is no longer available on the host.</span></span> <span data-ttu-id="f4757-182">V takovém případě brány se zobrazí odpověď HTTP 404 z webového serveru.</span><span class="sxs-lookup"><span data-stu-id="f4757-182">In this case, the gateway will receive an HTTP 404 response from the web server.</span></span> <span data-ttu-id="f4757-183">Proto HTTP 404 má dvě odlišné významy:</span><span class="sxs-lookup"><span data-stu-id="f4757-183">Thus, an HTTP 404 has two distinct meanings:</span></span>

- <span data-ttu-id="f4757-184">Případ #1: Adresa služby je správný, ale prostředek, který uživatel si vyžádal, neexistuje.</span><span class="sxs-lookup"><span data-stu-id="f4757-184">Case #1: The service address is correct, but the resource that the user requested does not exist.</span></span>
- <span data-ttu-id="f4757-185">Případ #2: Adresa služby jsou nesprávné a prostředek, který uživatel si vyžádal, může existovat na jiný uzel.</span><span class="sxs-lookup"><span data-stu-id="f4757-185">Case #2: The service address is incorrect, and the resource that the user requested might exist on a different node.</span></span>

<span data-ttu-id="f4757-186">Prvním případě je normální HTTP 404, která je považována za k chybě uživatele.</span><span class="sxs-lookup"><span data-stu-id="f4757-186">The first case is a normal HTTP 404, which is considered a user error.</span></span> <span data-ttu-id="f4757-187">V druhém případě však uživatel požadoval na prostředek, který neexistuje.</span><span class="sxs-lookup"><span data-stu-id="f4757-187">However, in the second case, the user has requested a resource that does exist.</span></span> <span data-ttu-id="f4757-188">Aplikační brána se nepodařilo najít, protože samotnou službu přesunula.</span><span class="sxs-lookup"><span data-stu-id="f4757-188">Application Gateway was unable to locate it because the service itself has moved.</span></span> <span data-ttu-id="f4757-189">Aplikační brány je potřeba znovu překlad adresy a opakujte žádost.</span><span class="sxs-lookup"><span data-stu-id="f4757-189">Application Gateway needs to resolve the address again and retry the request.</span></span>

<span data-ttu-id="f4757-190">Aplikační brána proto musí být k rozlišení mezi těmito dvěma případy.</span><span class="sxs-lookup"><span data-stu-id="f4757-190">Application Gateway thus needs a way to distinguish between these two cases.</span></span> <span data-ttu-id="f4757-191">Chcete-li tento rozdíl, je vyžadován nápovědu ze serveru.</span><span class="sxs-lookup"><span data-stu-id="f4757-191">To make that distinction, a hint from the server is required.</span></span>

* <span data-ttu-id="f4757-192">Ve výchozím nastavení Application Gateway předpokládá – případ #2 a pokusí se vyřešit a vydejte žádost znovu.</span><span class="sxs-lookup"><span data-stu-id="f4757-192">By default, Application Gateway assumes case #2 and attempts to resolve and issue the request again.</span></span>
* <span data-ttu-id="f4757-193">K označení – případ #1 aplikační brány, služba by měla vrátit následující hlavičku HTTP odpovědi:</span><span class="sxs-lookup"><span data-stu-id="f4757-193">To indicate case #1 to Application Gateway, the service should return the following HTTP response header:</span></span>

  `X-ServiceFabric : ResourceNotFound`

<span data-ttu-id="f4757-194">Tuto hlavičku HTTP odpovědi označuje normální HTTP 404 situaci, ve kterém požadovaný prostředek neexistuje, a Aplikační brána se nepokusí pro překlad adres služby znovu.</span><span class="sxs-lookup"><span data-stu-id="f4757-194">This HTTP response header indicates a normal HTTP 404 situation in which the requested resource does not exist, and Application Gateway will not attempt to resolve the service address again.</span></span>

## <a name="setup-and-configuration"></a><span data-ttu-id="f4757-195">Instalace a konfigurace</span><span class="sxs-lookup"><span data-stu-id="f4757-195">Setup and configuration</span></span>

### <a name="enable-reverse-proxy-via-azure-portal"></a><span data-ttu-id="f4757-196">Povolit reverzní proxy server prostřednictvím portálu Azure</span><span class="sxs-lookup"><span data-stu-id="f4757-196">Enable reverse proxy via Azure portal</span></span>

<span data-ttu-id="f4757-197">Portál Azure poskytuje možnost povolit reverzní proxy server při vytváření nového clusteru Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="f4757-197">Azure portal provides an option to enable Reverse proxy while creating a new Service Fabric cluster.</span></span>
<span data-ttu-id="f4757-198">V části **cluster Service Fabric vytvořit**, krok 2: Konfigurace clusteru, konfigurace typu uzlu, výběrem zaškrtávacího políčka "Povolit reverzní proxy".</span><span class="sxs-lookup"><span data-stu-id="f4757-198">Under **Create Service Fabric cluster**, Step 2: Cluster Configuration, Node type configuration, select the checkbox to "Enable reverse proxy".</span></span>
<span data-ttu-id="f4757-199">Pro konfiguraci zabezpečené reverzní proxy server, můžete zadat certifikát SSL v kroku 3: zabezpečení, konfigurovat nastavení zabezpečení clusteru, zaškrtněte políčko "Zahrnout certifikát SSL pro reverzní proxy server" a zadejte podrobnosti o certifikátu.</span><span class="sxs-lookup"><span data-stu-id="f4757-199">For configuring secure reverse proxy, SSL certificate can be specified in Step 3: Security, Configure cluster security settings, select the checkbox to "Include a SSL certificate for reverse proxy" and enter the certificate details.</span></span>

### <a name="enable-reverse-proxy-via-azure-resource-manager-templates"></a><span data-ttu-id="f4757-200">Povolit reverzní proxy server pomocí šablon Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="f4757-200">Enable reverse proxy via Azure Resource Manager templates</span></span>

<span data-ttu-id="f4757-201">Můžete použít [šablony Azure Resource Manageru](service-fabric-cluster-creation-via-arm.md) povolit reverzní proxy server v Service Fabric pro cluster.</span><span class="sxs-lookup"><span data-stu-id="f4757-201">You can use the [Azure Resource Manager template](service-fabric-cluster-creation-via-arm.md) to enable the reverse proxy in Service Fabric for the cluster.</span></span>

<span data-ttu-id="f4757-202">Odkazovat na [konfigurace HTTPS reverzní proxy server v clusteru s podporou zabezpečení](https://github.com/ChackDan/Service-Fabric/tree/master/ARM Templates/ReverseProxySecureSample#configure-https-reverse-proxy-in-a-secure-cluster) pro Azure Resource Manager šablony ukázky konfigurace zabezpečeného reverse proxy s certifikáty vyměnit certifikát a zpracování.</span><span class="sxs-lookup"><span data-stu-id="f4757-202">Refer to [Configure HTTPS Reverse Proxy in a secure cluster](https://github.com/ChackDan/Service-Fabric/tree/master/ARM Templates/ReverseProxySecureSample#configure-https-reverse-proxy-in-a-secure-cluster) for Azure Resource Manager template samples to configure secure reverse proxy with a certificate and handling certificate rollover.</span></span>

<span data-ttu-id="f4757-203">Nejdřív získat šablonu pro cluster, který chcete nasadit.</span><span class="sxs-lookup"><span data-stu-id="f4757-203">First, you get the template for the cluster that you want to deploy.</span></span> <span data-ttu-id="f4757-204">Můžete použít ukázkové šablony, nebo vytvořit vlastní šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="f4757-204">You can either use the sample templates or create a custom Resource Manager template.</span></span> <span data-ttu-id="f4757-205">Potom můžete povolit reverzní proxy server pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="f4757-205">Then, you can enable the reverse proxy by using the following steps:</span></span>

1. <span data-ttu-id="f4757-206">Zadejte port pro reverzní proxy server v [oddílu parametry](../azure-resource-manager/resource-group-authoring-templates.md) šablony.</span><span class="sxs-lookup"><span data-stu-id="f4757-206">Define a port for the reverse proxy in the [Parameters section](../azure-resource-manager/resource-group-authoring-templates.md) of the template.</span></span>

    ```json
    "SFReverseProxyPort": {
        "type": "int",
        "defaultValue": 19081,
        "metadata": {
            "description": "Endpoint for Service Fabric Reverse proxy"
        }
    },
    ```
2. <span data-ttu-id="f4757-207">Zadejte port pro každý typ uzlu objektů **clusteru** [části Typ prostředku](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="f4757-207">Specify the port for each of the nodetype objects in the **Cluster** [Resource type section](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>

    <span data-ttu-id="f4757-208">Port je určený podle názvu parametru, reverseProxyEndpointPort.</span><span class="sxs-lookup"><span data-stu-id="f4757-208">The port is identified by the parameter name, reverseProxyEndpointPort.</span></span>

    ```json
    {
        "apiVersion": "2016-09-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
       "nodeTypes": [
          {
           ...
           "reverseProxyEndpointPort": "[parameters('SFReverseProxyPort')]",
           ...
          },
        ...
        ],
        ...
    }
    ```
3. <span data-ttu-id="f4757-209">Chcete-li vyřešit reverzní proxy server z mimo Azure cluster, nastavte pravidla pro vyrovnávání zatížení Azure pro port, který jste zadali v kroku 1.</span><span class="sxs-lookup"><span data-stu-id="f4757-209">To address the reverse proxy from outside the Azure cluster, set up the Azure Load Balancer rules for the port that you specified in step 1.</span></span>

    ```json
    {
        "apiVersion": "[variables('lbApiVersion')]",
        "type": "Microsoft.Network/loadBalancers",
        ...
        ...
        "loadBalancingRules": [
            ...
            {
                "name": "LBSFReverseProxyRule",
                "properties": {
                    "backendAddressPool": {
                        "id": "[variables('lbPoolID0')]"
                    },
                    "backendPort": "[parameters('SFReverseProxyPort')]",
                    "enableFloatingIP": "false",
                    "frontendIPConfiguration": {
                        "id": "[variables('lbIPConfig0')]"
                    },
                    "frontendPort": "[parameters('SFReverseProxyPort')]",
                    "idleTimeoutInMinutes": "5",
                    "probe": {
                        "id": "[concat(variables('lbID0'),'/probes/SFReverseProxyProbe')]"
                    },
                    "protocol": "tcp"
                }
            }
        ],
        "probes": [
            ...
            {
                "name": "SFReverseProxyProbe",
                "properties": {
                    "intervalInSeconds": 5,
                    "numberOfProbes": 2,
                    "port":     "[parameters('SFReverseProxyPort')]",
                    "protocol": "tcp"
                }
            }  
        ]
    }
    ```
4. <span data-ttu-id="f4757-210">Pokud chcete nakonfigurovat certifikáty SSL na portu pro reverzní proxy server, přidejte certifikát, který chcete ***reverseProxyCertificate*** vlastnost **clusteru** [části Typ prostředku](../resource-group-authoring-templates.md) .</span><span class="sxs-lookup"><span data-stu-id="f4757-210">To configure SSL certificates on the port for the reverse proxy, add the certificate to the ***reverseProxyCertificate*** property in the **Cluster** [Resource type section](../resource-group-authoring-templates.md).</span></span>

    ```json
    {
        "apiVersion": "2016-09-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        "dependsOn": [
            "[concat('Microsoft.Storage/storageAccounts/', parameters('supportLogStorageAccountName'))]"
        ],
        "properties": {
            ...
            "reverseProxyCertificate": {
                "thumbprint": "[parameters('sfReverseProxyCertificateThumbprint')]",
                "x509StoreName": "[parameters('sfReverseProxyCertificateStoreName')]"
            },
            ...
            "clusterState": "Default",
        }
    }
    ```

### <a name="supporting-a-reverse-proxy-certificate-thats-different-from-the-cluster-certificate"></a><span data-ttu-id="f4757-211">Podpora certifikát reverzní proxy server, který se liší od certifikátu clusteru</span><span class="sxs-lookup"><span data-stu-id="f4757-211">Supporting a reverse proxy certificate that's different from the cluster certificate</span></span>
 <span data-ttu-id="f4757-212">Pokud certifikát reverzní proxy server se liší od certifikátu, který zabezpečuje clusteru, pak dříve zadaný certifikát musí být nainstalovaný na virtuálním počítači a přidat do seznamu řízení přístupu (ACL), se kterým můžete Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="f4757-212">If the reverse proxy certificate is different from the certificate that secures the cluster, then the previously specified certificate should be installed on the virtual machine and added to the access control list (ACL) so that Service Fabric can access it.</span></span> <span data-ttu-id="f4757-213">To lze provést **virtualMachineScaleSets** [části Typ prostředku](../resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="f4757-213">This can be done in the **virtualMachineScaleSets** [Resource type section](../resource-group-authoring-templates.md).</span></span> <span data-ttu-id="f4757-214">Pro instalaci přidejte do osProfile certifikátu.</span><span class="sxs-lookup"><span data-stu-id="f4757-214">For installation, add that certificate to the osProfile.</span></span> <span data-ttu-id="f4757-215">Rozšíření část šablony, můžete aktualizovat certifikát v seznamu ACL.</span><span class="sxs-lookup"><span data-stu-id="f4757-215">The extension section of the template can update the certificate in the ACL.</span></span>

  ```json
  {
    "apiVersion": "[variables('vmssApiVersion')]",
    "type": "Microsoft.Compute/virtualMachineScaleSets",
    ....
      "osProfile": {
          "adminPassword": "[parameters('adminPassword')]",
          "adminUsername": "[parameters('adminUsername')]",
          "computernamePrefix": "[parameters('vmNodeType0Name')]",
          "secrets": [
            {
              "sourceVault": {
                "id": "[parameters('sfReverseProxySourceVaultValue')]"
              },
              "vaultCertificates": [
                {
                  "certificateStore": "[parameters('sfReverseProxyCertificateStoreValue')]",
                  "certificateUrl": "[parameters('sfReverseProxyCertificateUrlValue')]"
                }
              ]
            }
          ]
        }
   ....
   "extensions": [
          {
              "name": "[concat(parameters('vmNodeType0Name'),'_ServiceFabricNode')]",
              "properties": {
                      "type": "ServiceFabricNode",
                      "autoUpgradeMinorVersion": false,
                      ...
                      "publisher": "Microsoft.Azure.ServiceFabric",
                      "settings": {
                        "clusterEndpoint": "[reference(parameters('clusterName')).clusterEndpoint]",
                        "nodeTypeRef": "[parameters('vmNodeType0Name')]",
                        "dataPath": "D:\\\\SvcFab",
                        "durabilityLevel": "Bronze",
                        "testExtension": true,
                        "reverseProxyCertificate": {
                          "thumbprint": "[parameters('sfReverseProxyCertificateThumbprint')]",
                          "x509StoreName": "[parameters('sfReverseProxyCertificateStoreValue')]"
                        },
                  },
                  "typeHandlerVersion": "1.0"
              }
          },
      ]
    }
  ```
> [!NOTE]
> <span data-ttu-id="f4757-216">Pokud používáte certifikáty, které se liší od clusteru certifikát, který chcete povolit reverzní proxy server na existující cluster, nainstalujte certifikát reverzní proxy server a aktualizace seznamu řízení přístupu v clusteru, než povolíte reverzní proxy server.</span><span class="sxs-lookup"><span data-stu-id="f4757-216">When you use certificates that are different from the cluster certificate to enable the reverse proxy on an existing cluster, install the reverse proxy certificate and update the ACL on the cluster before you enable the reverse proxy.</span></span> <span data-ttu-id="f4757-217">Dokončení [šablony Azure Resource Manageru](service-fabric-cluster-creation-via-arm.md) nasazení s použitím nastavení uvedeno dříve než zahájíte nasazení povolit reverzní proxy server v kroky 1 – 4.</span><span class="sxs-lookup"><span data-stu-id="f4757-217">Complete the [Azure Resource Manager template](service-fabric-cluster-creation-via-arm.md) deployment by using the settings mentioned previously before you start a deployment to enable the reverse proxy in steps 1-4.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f4757-218">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f4757-218">Next steps</span></span>
* <span data-ttu-id="f4757-219">Zobrazit příklad komunikaci pomocí protokolu HTTP mezi službami v [ukázkového projektu na Githubu](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started).</span><span class="sxs-lookup"><span data-stu-id="f4757-219">See an example of HTTP communication between services in a [sample project on GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started).</span></span>
* [<span data-ttu-id="f4757-220">Předávání služby Zabezpečené HTTP s reverzní proxy server</span><span class="sxs-lookup"><span data-stu-id="f4757-220">Forwarding to secure HTTP service with the reverse proxy</span></span>](service-fabric-reverseproxy-configure-secure-communication.md)
* [<span data-ttu-id="f4757-221">Volání vzdálených procedur s vzdálenou komunikaci spolehlivé služby</span><span class="sxs-lookup"><span data-stu-id="f4757-221">Remote procedure calls with Reliable Services remoting</span></span>](service-fabric-reliable-services-communication-remoting.md)
* [<span data-ttu-id="f4757-222">Webové rozhraní API, která používá OWIN v spolehlivé služby</span><span class="sxs-lookup"><span data-stu-id="f4757-222">Web API that uses OWIN in Reliable Services</span></span>](service-fabric-reliable-services-communication-webapi.md)
* [<span data-ttu-id="f4757-223">Komunikace WCF pomocí spolehlivé služby</span><span class="sxs-lookup"><span data-stu-id="f4757-223">WCF communication by using Reliable Services</span></span>](service-fabric-reliable-services-communication-wcf.md)
* <span data-ttu-id="f4757-224">Možnosti konfigurace další reverzní proxy server, najdete v části ApplicationGateway/Http [nastavení clusteru Service Fabric přizpůsobit](service-fabric-cluster-fabric-settings.md).</span><span class="sxs-lookup"><span data-stu-id="f4757-224">For additional reverse proxy configuration options, refer ApplicationGateway/Http section in [Customize Service Fabric cluster settings](service-fabric-cluster-fabric-settings.md).</span></span>

[0]: ./media/service-fabric-reverseproxy/external-communication.png
[1]: ./media/service-fabric-reverseproxy/internal-communication.png
