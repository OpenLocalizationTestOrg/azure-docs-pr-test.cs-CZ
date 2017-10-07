---
title: aaaAzure Service Fabric reverse proxy | Microsoft Docs
description: "Pro komunikaci toomicroservices z vnitřní a vnější hello clusteru pomocí Service Fabric reverzní proxy server."
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
ms.openlocfilehash: 0e7835a64ccd74293c7bdd8b41deae414c83dffa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="reverse-proxy-in-azure-service-fabric"></a><span data-ttu-id="539ef-103">Reverzní proxy server v Azure Service Fabric</span><span class="sxs-lookup"><span data-stu-id="539ef-103">Reverse proxy in Azure Service Fabric</span></span>
<span data-ttu-id="539ef-104">reverzní proxy server Hello, která je integrována do Azure Service Fabric řeší mikroslužeb v hello cluster Service Fabric, která zveřejňuje koncových bodů protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="539ef-104">hello reverse proxy that's built into Azure Service Fabric addresses microservices in hello Service Fabric cluster that exposes HTTP endpoints.</span></span>

## <a name="microservices-communication-model"></a><span data-ttu-id="539ef-105">Mikroslužeb komunikační model</span><span class="sxs-lookup"><span data-stu-id="539ef-105">Microservices communication model</span></span>
<span data-ttu-id="539ef-106">Mikroslužeb v Service Fabric obvykle spustit na podmnožinu virtuální počítače v clusteru hello a můžete přesunout z jednoho virtuálního počítače tooanother z různých důvodů.</span><span class="sxs-lookup"><span data-stu-id="539ef-106">Microservices in Service Fabric typically run on a subset of virtual machines in hello cluster and can move from one virtual machine tooanother for various reasons.</span></span> <span data-ttu-id="539ef-107">Ano hello koncové body pro mikroslužeb můžete změnit dynamicky.</span><span class="sxs-lookup"><span data-stu-id="539ef-107">So, hello endpoints for microservices can change dynamically.</span></span> <span data-ttu-id="539ef-108">Hello obvyklý toocommunicate toohello mikroslužbu je, že následující hello vyřešit smyčka:</span><span class="sxs-lookup"><span data-stu-id="539ef-108">hello typical pattern toocommunicate toohello microservice is hello following resolve loop:</span></span>

1. <span data-ttu-id="539ef-109">Přeložit umístění služby hello původně prostřednictvím služby pojmenování hello.</span><span class="sxs-lookup"><span data-stu-id="539ef-109">Resolve hello service location initially through hello naming service.</span></span>
2. <span data-ttu-id="539ef-110">Připojení ke službám toohello.</span><span class="sxs-lookup"><span data-stu-id="539ef-110">Connect toohello service.</span></span>
3. <span data-ttu-id="539ef-111">Určete hello příčinu selhání připojení a vyřešte umístění služby hello znovu, pokud je to nezbytné.</span><span class="sxs-lookup"><span data-stu-id="539ef-111">Determine hello cause of connection failures, and resolve hello service location again when necessary.</span></span>

<span data-ttu-id="539ef-112">Tento proces obvykle zahrnuje zabalení knihovny komunikace klienta ve smyčce opakování, který implementuje hello služby řešení a zkuste to znovu zásady.</span><span class="sxs-lookup"><span data-stu-id="539ef-112">This process generally involves wrapping client-side communication libraries in a retry loop that implements hello service resolution and retry policies.</span></span>
<span data-ttu-id="539ef-113">Další informace najdete v tématu [připojit a komunikovat se službami](service-fabric-connect-and-communicate-with-services.md).</span><span class="sxs-lookup"><span data-stu-id="539ef-113">For more information, see [Connect and communicate with services](service-fabric-connect-and-communicate-with-services.md).</span></span>

### <a name="communicating-by-using-hello-reverse-proxy"></a><span data-ttu-id="539ef-114">Komunikaci pomocí reverzní proxy server hello</span><span class="sxs-lookup"><span data-stu-id="539ef-114">Communicating by using hello reverse proxy</span></span>
<span data-ttu-id="539ef-115">reverzní proxy server Hello v Service Fabric se spustí ve všech uzlech clusteru hello hello.</span><span class="sxs-lookup"><span data-stu-id="539ef-115">hello reverse proxy in Service Fabric runs on all hello nodes in hello cluster.</span></span> <span data-ttu-id="539ef-116">Ji provede procesu překladu, který hello celé služby jménem klienta a pak předá požadavek klienta hello.</span><span class="sxs-lookup"><span data-stu-id="539ef-116">It performs hello entire service resolution process on a client's behalf and then forwards hello client request.</span></span> <span data-ttu-id="539ef-117">Klienti, kteří běží na clusteru hello tedy můžete používat všechny klienta HTTP komunikace knihovny tootalk toohello cílovou službu pomocí hello reverzní proxy server, aby hello spustí místně na stejném uzlu.</span><span class="sxs-lookup"><span data-stu-id="539ef-117">So, clients that run on hello cluster can use any client-side HTTP communication libraries tootalk toohello target service by using hello reverse proxy that runs locally on hello same node.</span></span>

![Interní komunikaci][1]

## <a name="reaching-microservices-from-outside-hello-cluster"></a><span data-ttu-id="539ef-119">Dosažení mikroslužeb z mimo hello clusteru</span><span class="sxs-lookup"><span data-stu-id="539ef-119">Reaching microservices from outside hello cluster</span></span>
<span data-ttu-id="539ef-120">Hello výchozí externí komunikaci modelu pro mikroslužeb je model opt-in, kde každé služby nelze přistupovat přímo z externích klientů.</span><span class="sxs-lookup"><span data-stu-id="539ef-120">hello default external communication model for microservices is an opt-in model where each service cannot be accessed directly from external clients.</span></span> <span data-ttu-id="539ef-121">[Azure nástroj pro vyrovnávání zatížení](../load-balancer/load-balancer-overview.md), což je hranici sítě mezi mikroslužeb a externími klienty, provádí překlad síťových adres a externí předává požadavky toointernal IP: port koncové body.</span><span class="sxs-lookup"><span data-stu-id="539ef-121">[Azure Load Balancer](../load-balancer/load-balancer-overview.md), which is a network boundary between microservices and external clients, performs network address translation and forwards external requests toointernal IP:port endpoints.</span></span> <span data-ttu-id="539ef-122">toomake klienty přímo přístupné tooexternal mikroslužbu koncový bod, musíte napřed nakonfigurovat tooforward provoz tooeach port, který hello služby používá nástroj pro vyrovnávání zatížení v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="539ef-122">toomake a microservice's endpoint directly accessible tooexternal clients, you must first configure Load Balancer tooforward traffic tooeach port that hello service uses in hello cluster.</span></span> <span data-ttu-id="539ef-123">Kromě toho většina mikroslužeb, zejména stavová mikroslužeb, nemáte live na všech uzlech clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="539ef-123">Furthermore, most microservices, especially stateful microservices, don't live on all nodes of hello cluster.</span></span> <span data-ttu-id="539ef-124">Hello mikroslužeb můžete přesouvat mezi uzly na převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="539ef-124">hello microservices can move between nodes on failover.</span></span> <span data-ttu-id="539ef-125">V takových případech nástroj pro vyrovnávání zatížení nemůže efektivně určit umístění hello z cílového uzlu hello hello repliky toowhich předávat provoz.</span><span class="sxs-lookup"><span data-stu-id="539ef-125">In such cases, Load Balancer cannot effectively determine hello location of hello target node of hello replicas toowhich it should forward traffic.</span></span>

### <a name="reaching-microservices-via-hello-reverse-proxy-from-outside-hello-cluster"></a><span data-ttu-id="539ef-126">Dosažení mikroslužeb přes reverzní proxy server hello z clusteru mimo hello</span><span class="sxs-lookup"><span data-stu-id="539ef-126">Reaching microservices via hello reverse proxy from outside hello cluster</span></span>
<span data-ttu-id="539ef-127">Namísto konfigurace hello port jednotlivé služby nástroji pro vyrovnávání zatížení, můžete nakonfigurovat pouze hello port reverzní proxy server hello nástroji pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="539ef-127">Instead of configuring hello port of an individual service in Load Balancer, you can configure just hello port of hello reverse proxy in Load Balancer.</span></span> <span data-ttu-id="539ef-128">Tato konfigurace umožňuje klientům mimo hello cluster dosáhnout služby uvnitř hello clusteru pomocí hello reverzní proxy server bez další konfigurace.</span><span class="sxs-lookup"><span data-stu-id="539ef-128">This configuration lets clients outside hello cluster reach services inside hello cluster by using hello reverse proxy without additional configuration.</span></span>

![Externí komunikace][0]

> [!WARNING]
> <span data-ttu-id="539ef-130">Když konfigurujete hello reverzní proxy server na portu nástroji pro vyrovnávání zatížení, jsou adresovatelné z mimo hello cluster všechny mikroslužeb hello clusteru, které zveřejňují koncový bod HTTP.</span><span class="sxs-lookup"><span data-stu-id="539ef-130">When you configure hello reverse proxy's port in Load Balancer, all microservices in hello cluster that expose an HTTP endpoint are addressable from outside hello cluster.</span></span>
>
>


## <a name="uri-format-for-addressing-services-by-using-hello-reverse-proxy"></a><span data-ttu-id="539ef-131">Formát identifikátoru URI pro adresování služby pomocí reverzní proxy server hello</span><span class="sxs-lookup"><span data-stu-id="539ef-131">URI format for addressing services by using hello reverse proxy</span></span>
<span data-ttu-id="539ef-132">Hello používá reverzní proxy server, které by měly být předávány konkrétní URI identifikátor URI formátu tooidentify hello oddílu toowhich hello příchozí žádosti o službu:</span><span class="sxs-lookup"><span data-stu-id="539ef-132">hello reverse proxy uses a specific uniform resource identifier (URI) format tooidentify hello service partition toowhich hello incoming request should be forwarded:</span></span>

```
http(s)://<Cluster FQDN | internal IP>:Port/<ServiceInstanceName>/<Suffix path>?PartitionKey=<key>&PartitionKind=<partitionkind>&ListenerName=<listenerName>&TargetReplicaSelector=<targetReplicaSelector>&Timeout=<timeout_in_seconds>
```

* <span data-ttu-id="539ef-133">**http (s):** hello reverzní proxy server může být nakonfigurované tooaccept HTTP nebo HTTPS přenosů.</span><span class="sxs-lookup"><span data-stu-id="539ef-133">**http(s):** hello reverse proxy can be configured tooaccept HTTP or HTTPS traffic.</span></span> <span data-ttu-id="539ef-134">Předávání protokolu HTTPS, najdete v části příliš[připojení ke službám zabezpečené tooa reverzní proxy server hello](service-fabric-reverseproxy-configure-secure-communication.md) až budete mít toolisten instalace reverzní proxy server na HTTPS.</span><span class="sxs-lookup"><span data-stu-id="539ef-134">For HTTPS forwarding, refer too[Connect tooa secure service with hello reverse proxy](service-fabric-reverseproxy-configure-secure-communication.md) once you have reverse proxy setup toolisten on HTTPS.</span></span>
* <span data-ttu-id="539ef-135">**Cluster plně kvalifikovaný název domény (FQDN) | interní IP:** pro externí klienty můžete nakonfigurovat hello reverzní proxy server tak, aby byla dostupná prostřednictvím hello clusteru domény, například mycluster.eastus.cloudapp.azure.com. Ve výchozím nastavení spouští hello reverzní proxy server na každý uzel.</span><span class="sxs-lookup"><span data-stu-id="539ef-135">**Cluster fully qualified domain name (FQDN) | internal IP:** For external clients, you can configure hello reverse proxy so that it is reachable through hello cluster domain, such as mycluster.eastus.cloudapp.azure.com. By default, hello reverse proxy runs on every node.</span></span> <span data-ttu-id="539ef-136">Pro interní provoz hello reverzní proxy server dostupný na místního hostitele nebo na všechny IP adresy pro interní uzlu, například 10.0.0.1.</span><span class="sxs-lookup"><span data-stu-id="539ef-136">For internal traffic, hello reverse proxy can be reached on localhost or on any internal node IP, such as 10.0.0.1.</span></span>
* <span data-ttu-id="539ef-137">**Port:** jde hello port, jako třeba 19081, který byl zadaný pro reverzní proxy server hello.</span><span class="sxs-lookup"><span data-stu-id="539ef-137">**Port:** This is hello port, such as 19081, that has been specified for hello reverse proxy.</span></span>
* <span data-ttu-id="539ef-138">**ServiceInstanceName:** jde hello plně kvalifikovaný název hello nasazena instance služby, kterou zkoušíte tooreach bez hello "fabric: /" schéma.</span><span class="sxs-lookup"><span data-stu-id="539ef-138">**ServiceInstanceName:** This is hello fully-qualified name of hello deployed service instance that you are trying tooreach without hello "fabric:/" scheme.</span></span> <span data-ttu-id="539ef-139">Například tooreach hello *fabric: / myapp/Moje_služba/* služby, byste použili *myapp/Moje_služba*.</span><span class="sxs-lookup"><span data-stu-id="539ef-139">For example, tooreach hello *fabric:/myapp/myservice/* service, you would use *myapp/myservice*.</span></span>

    <span data-ttu-id="539ef-140">Název instance služby Hello je malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="539ef-140">hello service instance name is case-sensitive.</span></span> <span data-ttu-id="539ef-141">Pomocí velká a malá písmena jinou pro název instance služby hello v adrese URL hello způsobí, že hello požadavky toofail s 404 (není nalezena).</span><span class="sxs-lookup"><span data-stu-id="539ef-141">Using a different casing for hello service instance name in hello URL causes hello requests toofail with 404 (Not Found).</span></span>
* <span data-ttu-id="539ef-142">**Přípona cesty:** jde hello skutečné cesty URL, například *myapi/hodnoty/přidat/3*, pro hello službu, která chcete tooconnect k.</span><span class="sxs-lookup"><span data-stu-id="539ef-142">**Suffix path:** This is hello actual URL path, such as *myapi/values/add/3*, for hello service that you want tooconnect to.</span></span>
* <span data-ttu-id="539ef-143">**Klíč oddílu:** u oddílů služby, je to klíč počítaného oddílu hello hello oddílu, které chcete tooreach.</span><span class="sxs-lookup"><span data-stu-id="539ef-143">**PartitionKey:** For a partitioned service, this is hello computed partition key of hello partition that you want tooreach.</span></span> <span data-ttu-id="539ef-144">Všimněte si, že toto je *není* hello oddílu ID GUID.</span><span class="sxs-lookup"><span data-stu-id="539ef-144">Note that this is *not* hello partition ID GUID.</span></span> <span data-ttu-id="539ef-145">Tento parametr není vyžadována pro služby, které používají schéma oddílu singleton hello.</span><span class="sxs-lookup"><span data-stu-id="539ef-145">This parameter is not required for services that use hello singleton partition scheme.</span></span>
* <span data-ttu-id="539ef-146">**PartitionKind:** jde hello schéma oddílu služby.</span><span class="sxs-lookup"><span data-stu-id="539ef-146">**PartitionKind:** This is hello service partition scheme.</span></span> <span data-ttu-id="539ef-147">To může být 'Int64Range' nebo "S názvem".</span><span class="sxs-lookup"><span data-stu-id="539ef-147">This can be 'Int64Range' or 'Named'.</span></span> <span data-ttu-id="539ef-148">Tento parametr není vyžadována pro služby, které používají schéma oddílu singleton hello.</span><span class="sxs-lookup"><span data-stu-id="539ef-148">This parameter is not required for services that use hello singleton partition scheme.</span></span>
* <span data-ttu-id="539ef-149">**ListenerName** jsou koncové body hello ze služby hello hello formuláře {"Koncové body": {"Listener1": "Koncovém bodě 1", "Listener2": "Endpoint2"...}}.</span><span class="sxs-lookup"><span data-stu-id="539ef-149">**ListenerName** hello endpoints from hello service are of hello form {"Endpoints":{"Listener1":"Endpoint1","Listener2":"Endpoint2" ...}}.</span></span> <span data-ttu-id="539ef-150">Když služba hello zpřístupňuje několik koncových bodů, ten identifikuje koncový bod hello tento požadavek klienta hello by měl být předán.</span><span class="sxs-lookup"><span data-stu-id="539ef-150">When hello service exposes multiple endpoints, this identifies hello endpoint that hello client request should be forwarded to.</span></span> <span data-ttu-id="539ef-151">To lze vynechat, pokud služba hello má jenom jeden naslouchací proces.</span><span class="sxs-lookup"><span data-stu-id="539ef-151">This can be omitted if hello service has only one listener.</span></span>
* <span data-ttu-id="539ef-152">**TargetReplicaSelector** určuje jak měla by být vybrána hello cíl replik nebo instancí.</span><span class="sxs-lookup"><span data-stu-id="539ef-152">**TargetReplicaSelector** This specifies how hello target replica or instance should be selected.</span></span>
  * <span data-ttu-id="539ef-153">Po stavová hello cílovou službu hello TargetReplicaSelector může být jedna z následujících hello: 'PrimaryReplica', 'RandomSecondaryReplica' nebo 'RandomReplica'.</span><span class="sxs-lookup"><span data-stu-id="539ef-153">When hello target service is stateful, hello TargetReplicaSelector can be one of hello following:  'PrimaryReplica', 'RandomSecondaryReplica', or 'RandomReplica'.</span></span> <span data-ttu-id="539ef-154">Pokud není tento parametr zadán, je výchozí hello 'PrimaryReplica'.</span><span class="sxs-lookup"><span data-stu-id="539ef-154">When this parameter is not specified, hello default is 'PrimaryReplica'.</span></span>
  * <span data-ttu-id="539ef-155">Po bezstavové hello cílovou službu reverzní proxy server vybere náhodných instanci hello služba oddílu tooforward hello žádosti o.</span><span class="sxs-lookup"><span data-stu-id="539ef-155">When hello target service is stateless, reverse proxy picks a random instance of hello service partition tooforward hello request to.</span></span>
* <span data-ttu-id="539ef-156">**Časový limit:** určuje hello časový limit pro požadavek hello HTTP vytvořených službou toohello reverzní proxy server hello jménem hello požadavku klienta.</span><span class="sxs-lookup"><span data-stu-id="539ef-156">**Timeout:**  This specifies hello timeout for hello HTTP request created by hello reverse proxy toohello service on behalf of hello client request.</span></span> <span data-ttu-id="539ef-157">Hello výchozí hodnota je 60 sekund.</span><span class="sxs-lookup"><span data-stu-id="539ef-157">hello default value is 60 seconds.</span></span> <span data-ttu-id="539ef-158">Toto je volitelný parametr.</span><span class="sxs-lookup"><span data-stu-id="539ef-158">This is an optional parameter.</span></span>

### <a name="example-usage"></a><span data-ttu-id="539ef-159">Příklad použití</span><span class="sxs-lookup"><span data-stu-id="539ef-159">Example usage</span></span>
<span data-ttu-id="539ef-160">Jako příklad Podívejme hello *fabric: / MyApp/Moje_služba* služby, které se otevře naslouchací proces protokolu HTTP na hello následující adresu URL:</span><span class="sxs-lookup"><span data-stu-id="539ef-160">As an example, let's take hello *fabric:/MyApp/MyService* service that opens an HTTP listener on hello following URL:</span></span>

```
http://10.0.0.5:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/
```

<span data-ttu-id="539ef-161">Následují hello prostředky služby hello:</span><span class="sxs-lookup"><span data-stu-id="539ef-161">Following are hello resources for hello service:</span></span>

* `/index.html`
* `/api/users/<userId>`

<span data-ttu-id="539ef-162">Pokud služba hello používá hello singleton dělení schéma, hello *PartitionKey* a *PartitionKind* parametrů řetězce dotazu nejsou nutné a hello služby lze získat přístup pomocí hello brány jako:</span><span class="sxs-lookup"><span data-stu-id="539ef-162">If hello service uses hello singleton partitioning scheme, hello *PartitionKey* and *PartitionKind* query string parameters are not required, and hello service can be reached by using hello gateway as:</span></span>

* <span data-ttu-id="539ef-163">Externě:`http://mycluster.eastus.cloudapp.azure.com:19081/MyApp/MyService`</span><span class="sxs-lookup"><span data-stu-id="539ef-163">Externally: `http://mycluster.eastus.cloudapp.azure.com:19081/MyApp/MyService`</span></span>
* <span data-ttu-id="539ef-164">Interně:`http://localhost:19081/MyApp/MyService`</span><span class="sxs-lookup"><span data-stu-id="539ef-164">Internally: `http://localhost:19081/MyApp/MyService`</span></span>

<span data-ttu-id="539ef-165">Pokud služba hello používá hello Uniform Int64 dělení schéma, hello *PartitionKey* a *PartitionKind* parametrů řetězce dotazu, musí být použité tooreach oddílu služby hello:</span><span class="sxs-lookup"><span data-stu-id="539ef-165">If hello service uses hello Uniform Int64 partitioning scheme, hello *PartitionKey* and *PartitionKind* query string parameters must be used tooreach a partition of hello service:</span></span>

* <span data-ttu-id="539ef-166">Externě:`http://mycluster.eastus.cloudapp.azure.com:19081/MyApp/MyService?PartitionKey=3&PartitionKind=Int64Range`</span><span class="sxs-lookup"><span data-stu-id="539ef-166">Externally: `http://mycluster.eastus.cloudapp.azure.com:19081/MyApp/MyService?PartitionKey=3&PartitionKind=Int64Range`</span></span>
* <span data-ttu-id="539ef-167">Interně:`http://localhost:19081/MyApp/MyService?PartitionKey=3&PartitionKind=Int64Range`</span><span class="sxs-lookup"><span data-stu-id="539ef-167">Internally: `http://localhost:19081/MyApp/MyService?PartitionKey=3&PartitionKind=Int64Range`</span></span>

<span data-ttu-id="539ef-168">cesta prostředku hello tooreach hello prostředky, které služba hello zpřístupní, jednoduše umístit po hello název služby v adrese URL hello:</span><span class="sxs-lookup"><span data-stu-id="539ef-168">tooreach hello resources that hello service exposes, simply place hello resource path after hello service name in hello URL:</span></span>

* <span data-ttu-id="539ef-169">Externě:`http://mycluster.eastus.cloudapp.azure.com:19081/MyApp/MyService/index.html?PartitionKey=3&PartitionKind=Int64Range`</span><span class="sxs-lookup"><span data-stu-id="539ef-169">Externally: `http://mycluster.eastus.cloudapp.azure.com:19081/MyApp/MyService/index.html?PartitionKey=3&PartitionKind=Int64Range`</span></span>
* <span data-ttu-id="539ef-170">Interně:`http://localhost:19081/MyApp/MyService/api/users/6?PartitionKey=3&PartitionKind=Int64Range`</span><span class="sxs-lookup"><span data-stu-id="539ef-170">Internally: `http://localhost:19081/MyApp/MyService/api/users/6?PartitionKey=3&PartitionKind=Int64Range`</span></span>

<span data-ttu-id="539ef-171">Brána Hello pak předá adresa URL služby tyto požadavky toohello:</span><span class="sxs-lookup"><span data-stu-id="539ef-171">hello gateway will then forward these requests toohello service's URL:</span></span>

* `http://10.0.0.5:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/index.html`
* `http://10.0.0.5:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/api/users/6`

## <a name="special-handling-for-port-sharing-services"></a><span data-ttu-id="539ef-172">Zvláštní zpracování pro sdílení portů služby</span><span class="sxs-lookup"><span data-stu-id="539ef-172">Special handling for port-sharing services</span></span>
<span data-ttu-id="539ef-173">Služba Azure Application Gateway pokusí tooresolve služby adres znovu a opakujte žádost hello, pokud služba není dostupný.</span><span class="sxs-lookup"><span data-stu-id="539ef-173">Azure Application Gateway attempts tooresolve a service address again and retry hello request when a service cannot be reached.</span></span> <span data-ttu-id="539ef-174">Je to hlavní výhodou Application Gateway, proto kód klienta není nutné tooimplement vlastní řešení služby a vyřešit smyčky.</span><span class="sxs-lookup"><span data-stu-id="539ef-174">This is a major benefit of Application Gateway because client code does not need tooimplement its own service resolution and resolve loop.</span></span>

<span data-ttu-id="539ef-175">Obecně platí když služba není dostupná, instance služby hello nebo repliky se přesunul tooa jiný uzel jako součást životního cyklu normální.</span><span class="sxs-lookup"><span data-stu-id="539ef-175">Generally, when a service cannot be reached, hello service instance or replica has moved tooa different node as part of its normal lifecycle.</span></span> <span data-ttu-id="539ef-176">V takovém případě může zobrazit aplikační brány síťové připojení chyba označující, že koncový bod je, že nejsou otevřeny žádné delší na hello původně přeložit adresy.</span><span class="sxs-lookup"><span data-stu-id="539ef-176">When this happens, Application Gateway might receive a network connection error indicating that an endpoint is no longer open on hello originally resolved address.</span></span>

<span data-ttu-id="539ef-177">Ale replik nebo instancí služby můžete sdílet hostitelský proces a může také sdílet port při hostované na základě ovladače http.sys webového serveru, včetně:</span><span class="sxs-lookup"><span data-stu-id="539ef-177">However, replicas or service instances can share a host process and might also share a port when hosted by an http.sys-based web server, including:</span></span>

* [<span data-ttu-id="539ef-178">System.Net.HttpListener</span><span class="sxs-lookup"><span data-stu-id="539ef-178">System.Net.HttpListener</span></span>](https://msdn.microsoft.com/library/system.net.httplistener%28v=vs.110%29.aspx)
* [<span data-ttu-id="539ef-179">WebListener ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="539ef-179">ASP.NET Core WebListener</span></span>](https://docs.asp.net/latest/fundamentals/servers.html#weblistener)
* [<span data-ttu-id="539ef-180">Katana</span><span class="sxs-lookup"><span data-stu-id="539ef-180">Katana</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.OwinSelfHost/)

<span data-ttu-id="539ef-181">V takovém případě je pravděpodobné, že tento webový server hello je k dispozici v hello hostitelský proces a toorequests, ale hello přeložit instance služby nebo replika již není k dispozici na hostiteli hello reagovat.</span><span class="sxs-lookup"><span data-stu-id="539ef-181">In this situation, it is likely that hello web server is available in hello host process and responding toorequests, but hello resolved service instance or replica is no longer available on hello host.</span></span> <span data-ttu-id="539ef-182">V takovém případě hello brány obdrží odpověď HTTP 404 z hello webový server.</span><span class="sxs-lookup"><span data-stu-id="539ef-182">In this case, hello gateway will receive an HTTP 404 response from hello web server.</span></span> <span data-ttu-id="539ef-183">Proto HTTP 404 má dvě odlišné významy:</span><span class="sxs-lookup"><span data-stu-id="539ef-183">Thus, an HTTP 404 has two distinct meanings:</span></span>

- <span data-ttu-id="539ef-184">Případ #1: adresa služby hello je správný, ale hello prostředek, který hello požadovaného uživatele neexistuje.</span><span class="sxs-lookup"><span data-stu-id="539ef-184">Case #1: hello service address is correct, but hello resource that hello user requested does not exist.</span></span>
- <span data-ttu-id="539ef-185">Případ #2: adresa služby hello jsou nesprávné a hello prostředek, který hello požadovaného uživatele mohou existovat na jiný uzel.</span><span class="sxs-lookup"><span data-stu-id="539ef-185">Case #2: hello service address is incorrect, and hello resource that hello user requested might exist on a different node.</span></span>

<span data-ttu-id="539ef-186">prvním případě Hello je normální HTTP 404, která je považována za k chybě uživatele.</span><span class="sxs-lookup"><span data-stu-id="539ef-186">hello first case is a normal HTTP 404, which is considered a user error.</span></span> <span data-ttu-id="539ef-187">V druhém případě hello však hello uživatel požadoval na prostředek, který neexistuje.</span><span class="sxs-lookup"><span data-stu-id="539ef-187">However, in hello second case, hello user has requested a resource that does exist.</span></span> <span data-ttu-id="539ef-188">Aplikační brána se nemůže toolocate, protože hello vlastní služby přesunula.</span><span class="sxs-lookup"><span data-stu-id="539ef-188">Application Gateway was unable toolocate it because hello service itself has moved.</span></span> <span data-ttu-id="539ef-189">Aplikace musí tooresolve hello adresu brány znovu a opakujte žádost hello.</span><span class="sxs-lookup"><span data-stu-id="539ef-189">Application Gateway needs tooresolve hello address again and retry hello request.</span></span>

<span data-ttu-id="539ef-190">Aplikační brána musí proto způsob toodistinguish mezi těmito dvěma případy.</span><span class="sxs-lookup"><span data-stu-id="539ef-190">Application Gateway thus needs a way toodistinguish between these two cases.</span></span> <span data-ttu-id="539ef-191">toomake, že rozdíl, nápovědu ze serveru hello je vyžadována.</span><span class="sxs-lookup"><span data-stu-id="539ef-191">toomake that distinction, a hint from hello server is required.</span></span>

* <span data-ttu-id="539ef-192">Ve výchozím nastavení aplikační brána předpokládá – případ #2 a znovu se pokusí tooresolve a problém žádost o hello.</span><span class="sxs-lookup"><span data-stu-id="539ef-192">By default, Application Gateway assumes case #2 and attempts tooresolve and issue hello request again.</span></span>
* <span data-ttu-id="539ef-193">tooindicate – případ #1 tooApplication brány, služba hello by měla vrátit hello následující hlavičku HTTP odpovědi:</span><span class="sxs-lookup"><span data-stu-id="539ef-193">tooindicate case #1 tooApplication Gateway, hello service should return hello following HTTP response header:</span></span>

  `X-ServiceFabric : ResourceNotFound`

<span data-ttu-id="539ef-194">Tuto hlavičku HTTP odpovědi označuje normální HTTP 404 situaci, ve které hello požadovaný prostředek neexistuje a adresu tooresolve hello služby Application Gateway se nepokusí znovu.</span><span class="sxs-lookup"><span data-stu-id="539ef-194">This HTTP response header indicates a normal HTTP 404 situation in which hello requested resource does not exist, and Application Gateway will not attempt tooresolve hello service address again.</span></span>

## <a name="setup-and-configuration"></a><span data-ttu-id="539ef-195">Instalace a konfigurace</span><span class="sxs-lookup"><span data-stu-id="539ef-195">Setup and configuration</span></span>

### <a name="enable-reverse-proxy-via-azure-portal"></a><span data-ttu-id="539ef-196">Povolit reverzní proxy server prostřednictvím portálu Azure</span><span class="sxs-lookup"><span data-stu-id="539ef-196">Enable reverse proxy via Azure portal</span></span>

<span data-ttu-id="539ef-197">Portál Azure poskytuje možnost tooenable reverzní proxy server při vytváření nového clusteru Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="539ef-197">Azure portal provides an option tooenable Reverse proxy while creating a new Service Fabric cluster.</span></span>
<span data-ttu-id="539ef-198">V části **cluster Service Fabric vytvořit**, krok 2: Konfigurace clusteru, konfigurace typu uzlu, zaškrtněte políčko hello příliš "Reverzní proxy server povolit".</span><span class="sxs-lookup"><span data-stu-id="539ef-198">Under **Create Service Fabric cluster**, Step 2: Cluster Configuration, Node type configuration, select hello checkbox too"Enable reverse proxy".</span></span>
<span data-ttu-id="539ef-199">Pro konfiguraci zabezpečené reverzní proxy server, můžete zadat certifikát SSL v kroku 3: zabezpečení, konfigurovat nastavení zabezpečení clusteru, vyberte hello políčko příliš "Zahrnout certifikát SSL pro reverzní proxy server" a zadejte podrobnosti o certifikátu hello.</span><span class="sxs-lookup"><span data-stu-id="539ef-199">For configuring secure reverse proxy, SSL certificate can be specified in Step 3: Security, Configure cluster security settings, select hello checkbox too"Include a SSL certificate for reverse proxy" and enter hello certificate details.</span></span>

### <a name="enable-reverse-proxy-via-azure-resource-manager-templates"></a><span data-ttu-id="539ef-200">Povolit reverzní proxy server pomocí šablon Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="539ef-200">Enable reverse proxy via Azure Resource Manager templates</span></span>

<span data-ttu-id="539ef-201">Můžete použít hello [šablony Azure Resource Manageru](service-fabric-cluster-creation-via-arm.md) tooenable hello reverzní proxy server v Service Fabric pro hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="539ef-201">You can use hello [Azure Resource Manager template](service-fabric-cluster-creation-via-arm.md) tooenable hello reverse proxy in Service Fabric for hello cluster.</span></span>

<span data-ttu-id="539ef-202">Odkazovat příliš[konfigurace HTTPS reverzní proxy server v clusteru s podporou zabezpečení](https://github.com/ChackDan/Service-Fabric/tree/master/ARM Templates/ReverseProxySecureSample#configure-https-reverse-proxy-in-a-secure-cluster) pro Azure Resource Manager šablony ukázky tooconfigure zabezpečené reverzní proxy server s certifikáty vyměnit certifikát a zpracování.</span><span class="sxs-lookup"><span data-stu-id="539ef-202">Refer too[Configure HTTPS Reverse Proxy in a secure cluster](https://github.com/ChackDan/Service-Fabric/tree/master/ARM Templates/ReverseProxySecureSample#configure-https-reverse-proxy-in-a-secure-cluster) for Azure Resource Manager template samples tooconfigure secure reverse proxy with a certificate and handling certificate rollover.</span></span>

<span data-ttu-id="539ef-203">Nejprve zobrazí hello šablony pro hello clusteru, které chcete toodeploy.</span><span class="sxs-lookup"><span data-stu-id="539ef-203">First, you get hello template for hello cluster that you want toodeploy.</span></span> <span data-ttu-id="539ef-204">Můžete buď použít hello ukázkové šablony, nebo vytvořit vlastní šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="539ef-204">You can either use hello sample templates or create a custom Resource Manager template.</span></span> <span data-ttu-id="539ef-205">Potom můžete povolit hello reverzní proxy server pomocí hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="539ef-205">Then, you can enable hello reverse proxy by using hello following steps:</span></span>

1. <span data-ttu-id="539ef-206">Zadejte port pro reverzní proxy server hello v hello [oddílu parametry](../azure-resource-manager/resource-group-authoring-templates.md) hello šablony.</span><span class="sxs-lookup"><span data-stu-id="539ef-206">Define a port for hello reverse proxy in hello [Parameters section](../azure-resource-manager/resource-group-authoring-templates.md) of hello template.</span></span>

    ```json
    "SFReverseProxyPort": {
        "type": "int",
        "defaultValue": 19081,
        "metadata": {
            "description": "Endpoint for Service Fabric Reverse proxy"
        }
    },
    ```
2. <span data-ttu-id="539ef-207">Zadejte hello port pro každý z objektů nodetype hello v hello **clusteru** [části Typ prostředku](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="539ef-207">Specify hello port for each of hello nodetype objects in hello **Cluster** [Resource type section](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>

    <span data-ttu-id="539ef-208">Hello port je identifikována hello názvu parametru, reverseProxyEndpointPort.</span><span class="sxs-lookup"><span data-stu-id="539ef-208">hello port is identified by hello parameter name, reverseProxyEndpointPort.</span></span>

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
3. <span data-ttu-id="539ef-209">tooaddress hello reverzní proxy server z mimo hello clusteru Azure nastavit pravidla pro vyrovnávání zatížení Azure hello pro hello port, který jste zadali v kroku 1.</span><span class="sxs-lookup"><span data-stu-id="539ef-209">tooaddress hello reverse proxy from outside hello Azure cluster, set up hello Azure Load Balancer rules for hello port that you specified in step 1.</span></span>

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
4. <span data-ttu-id="539ef-210">tooconfigure certifikáty SSL na portu hello pro hello reverzní proxy server, přidejte hello certifikát toohello ***reverseProxyCertificate*** vlastnost hello **clusteru** [části Typ prostředku](../resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="539ef-210">tooconfigure SSL certificates on hello port for hello reverse proxy, add hello certificate toohello ***reverseProxyCertificate*** property in hello **Cluster** [Resource type section](../resource-group-authoring-templates.md).</span></span>

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

### <a name="supporting-a-reverse-proxy-certificate-thats-different-from-hello-cluster-certificate"></a><span data-ttu-id="539ef-211">Podpora certifikát reverzní proxy server, který se liší od certifikátu clusteru hello</span><span class="sxs-lookup"><span data-stu-id="539ef-211">Supporting a reverse proxy certificate that's different from hello cluster certificate</span></span>
 <span data-ttu-id="539ef-212">Pokud je certifikát reverzní proxy server hello liší od hello certifikát, který zabezpečuje hello clusteru, pak hello dříve zadané certifikátu by měly být nainstalovány na virtuálním počítači hello a doplnili toohello seznam řízení přístupu (ACL), takže můžete Service Fabric k němu přístup.</span><span class="sxs-lookup"><span data-stu-id="539ef-212">If hello reverse proxy certificate is different from hello certificate that secures hello cluster, then hello previously specified certificate should be installed on hello virtual machine and added toohello access control list (ACL) so that Service Fabric can access it.</span></span> <span data-ttu-id="539ef-213">To lze provést v hello **virtualMachineScaleSets** [části Typ prostředku](../resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="539ef-213">This can be done in hello **virtualMachineScaleSets** [Resource type section](../resource-group-authoring-templates.md).</span></span> <span data-ttu-id="539ef-214">Pro instalaci přidejte osProfile toohello tohoto certifikátu.</span><span class="sxs-lookup"><span data-stu-id="539ef-214">For installation, add that certificate toohello osProfile.</span></span> <span data-ttu-id="539ef-215">Rozšíření oddílu Hello hello šablony můžete aktualizovat hello certifikátu v seznamu ACL hello.</span><span class="sxs-lookup"><span data-stu-id="539ef-215">hello extension section of hello template can update hello certificate in hello ACL.</span></span>

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
> <span data-ttu-id="539ef-216">Pokud používáte certifikáty, které se liší od hello clusteru certifikát tooenable hello reverzní proxy server na existující cluster, nainstalujte certifikát reverzní proxy server hello a aktualizovat hello seznamu ACL v clusteru hello před povolením hello reverzní proxy server.</span><span class="sxs-lookup"><span data-stu-id="539ef-216">When you use certificates that are different from hello cluster certificate tooenable hello reverse proxy on an existing cluster, install hello reverse proxy certificate and update hello ACL on hello cluster before you enable hello reverse proxy.</span></span> <span data-ttu-id="539ef-217">Dokončení hello [šablony Azure Resource Manageru](service-fabric-cluster-creation-via-arm.md) nasazení pomocí nastavení hello uvedeno dříve před zahájením nasazení tooenable hello reverzní proxy server v krocích 1 – 4.</span><span class="sxs-lookup"><span data-stu-id="539ef-217">Complete hello [Azure Resource Manager template](service-fabric-cluster-creation-via-arm.md) deployment by using hello settings mentioned previously before you start a deployment tooenable hello reverse proxy in steps 1-4.</span></span>

## <a name="next-steps"></a><span data-ttu-id="539ef-218">Další kroky</span><span class="sxs-lookup"><span data-stu-id="539ef-218">Next steps</span></span>
* <span data-ttu-id="539ef-219">Zobrazit příklad komunikaci pomocí protokolu HTTP mezi službami v [ukázkového projektu na Githubu](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started).</span><span class="sxs-lookup"><span data-stu-id="539ef-219">See an example of HTTP communication between services in a [sample project on GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started).</span></span>
* [<span data-ttu-id="539ef-220">Předávání služby toosecure HTTP s reverzní proxy server hello</span><span class="sxs-lookup"><span data-stu-id="539ef-220">Forwarding toosecure HTTP service with hello reverse proxy</span></span>](service-fabric-reverseproxy-configure-secure-communication.md)
* [<span data-ttu-id="539ef-221">Volání vzdálených procedur s vzdálenou komunikaci spolehlivé služby</span><span class="sxs-lookup"><span data-stu-id="539ef-221">Remote procedure calls with Reliable Services remoting</span></span>](service-fabric-reliable-services-communication-remoting.md)
* [<span data-ttu-id="539ef-222">Webové rozhraní API, která používá OWIN v spolehlivé služby</span><span class="sxs-lookup"><span data-stu-id="539ef-222">Web API that uses OWIN in Reliable Services</span></span>](service-fabric-reliable-services-communication-webapi.md)
* [<span data-ttu-id="539ef-223">Komunikace WCF pomocí spolehlivé služby</span><span class="sxs-lookup"><span data-stu-id="539ef-223">WCF communication by using Reliable Services</span></span>](service-fabric-reliable-services-communication-wcf.md)
* <span data-ttu-id="539ef-224">Možnosti konfigurace další reverzní proxy server, najdete v části ApplicationGateway/Http [nastavení clusteru Service Fabric přizpůsobit](service-fabric-cluster-fabric-settings.md).</span><span class="sxs-lookup"><span data-stu-id="539ef-224">For additional reverse proxy configuration options, refer ApplicationGateway/Http section in [Customize Service Fabric cluster settings](service-fabric-cluster-fabric-settings.md).</span></span>

[0]: ./media/service-fabric-reverseproxy/external-communication.png
[1]: ./media/service-fabric-reverseproxy/internal-communication.png
