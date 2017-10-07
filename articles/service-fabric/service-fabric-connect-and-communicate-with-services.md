---
title: "aaaConnect a komunikovat se službami v Azure Service Fabric | Microsoft Docs"
description: "Zjistěte, jak připojit tooresolve a komunikovat se službami v Service Fabric."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: msfussell
ms.assetid: 7d1052ec-2c9f-443d-8b99-b75c97266e6c
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 5/9/2017
ms.author: vturecek
ms.openlocfilehash: b8b374a71d4c5d21f48a560a3a8c81b357fe418d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-and-communicate-with-services-in-service-fabric"></a><span data-ttu-id="d5a62-103">Připojení a komunikovat se službami v Service Fabric</span><span class="sxs-lookup"><span data-stu-id="d5a62-103">Connect and communicate with services in Service Fabric</span></span>
<span data-ttu-id="d5a62-104">V Service Fabric běží služba někde v clusteru Service Fabric, obvykle se distribuují napříč více virtuálními počítači.</span><span class="sxs-lookup"><span data-stu-id="d5a62-104">In Service Fabric, a service runs somewhere in a Service Fabric cluster, typically distributed across multiple VMs.</span></span> <span data-ttu-id="d5a62-105">Nebylo možné přesouvat z jednoho místa tooanother pomocí hello vlastník služby, nebo automaticky pomocí Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="d5a62-105">It can be moved from one place tooanother, either by hello service owner, or automatically by Service Fabric.</span></span> <span data-ttu-id="d5a62-106">Služby nejsou staticky vázané tooa konkrétní počítač nebo adresu.</span><span class="sxs-lookup"><span data-stu-id="d5a62-106">Services are not statically tied tooa particular machine or address.</span></span>

<span data-ttu-id="d5a62-107">Aplikace Service Fabric se obvykle skládá z mnoha různých služeb, kde každá služba provádí úlohu specializované.</span><span class="sxs-lookup"><span data-stu-id="d5a62-107">A Service Fabric application is generally composed of many different services, where each service performs a specialized task.</span></span> <span data-ttu-id="d5a62-108">Tyto služby může komunikovat s navzájem tooform dokončení funkce, jako je například vykreslování různé části webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="d5a62-108">These services may communicate with each other tooform a complete function, such as rendering different parts of a web application.</span></span> <span data-ttu-id="d5a62-109">Existují také klientské aplikace, které se připojují tooand komunikovat se službami.</span><span class="sxs-lookup"><span data-stu-id="d5a62-109">There are also client applications that connect tooand communicate with services.</span></span> <span data-ttu-id="d5a62-110">Tento dokument popisuje, jak tooset až komunikaci s a mezi vaší služby v Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="d5a62-110">This document discusses how tooset up communication with and between your services in Service Fabric.</span></span>

<span data-ttu-id="d5a62-111">Toto video Microsoft Virtual Academy taky popisuje komunikace služby:<center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=iYFCk76yC_6706218965"></span><span class="sxs-lookup"><span data-stu-id="d5a62-111">This Microsoft Virtual Academy video also discusses service communication: <center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=iYFCk76yC_6706218965"></span></span>  
<img src="./media/service-fabric-connect-and-communicate-with-services/CommunicationVid.png" WIDTH="360" HEIGHT="244">  
</a></center>

## <a name="bring-your-own-protocol"></a><span data-ttu-id="d5a62-112">Přineste si vlastní protokol</span><span class="sxs-lookup"><span data-stu-id="d5a62-112">Bring your own protocol</span></span>
<span data-ttu-id="d5a62-113">Service Fabric pomáhá spravovat životní cyklus hello vašich služeb, ale neprovede rozhodnutí o co dělat, vaše služby.</span><span class="sxs-lookup"><span data-stu-id="d5a62-113">Service Fabric helps manage hello lifecycle of your services but it does not make decisions about what your services do.</span></span> <span data-ttu-id="d5a62-114">To zahrnuje komunikaci.</span><span class="sxs-lookup"><span data-stu-id="d5a62-114">This includes communication.</span></span> <span data-ttu-id="d5a62-115">Po otevření služby pomocí Service Fabric, která je vaše služba možnost tooset až koncový bod pro příchozí požadavky, ať zásobník protokolu nebo komunikaci pomocí chcete.</span><span class="sxs-lookup"><span data-stu-id="d5a62-115">When your service is opened by Service Fabric, that's your service's opportunity tooset up an endpoint for incoming requests, using whatever protocol or communication stack you want.</span></span> <span data-ttu-id="d5a62-116">Vaše služba bude naslouchat na normální **IP: port** adres pomocí žádné schéma adresování, jako je identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="d5a62-116">Your service will listen on a normal **IP:port** address using any addressing scheme, such as a URI.</span></span> <span data-ttu-id="d5a62-117">Hostitelský proces může sdílet více instancí služby nebo repliky, v takovém případě se bude buď potřebovat toouse jiné porty, nebo použijte mechanismus sdílení portů, jako je například jádra ovladač http.sys hello v systému Windows.</span><span class="sxs-lookup"><span data-stu-id="d5a62-117">Multiple service instances or replicas may share a host process, in which case they will either need toouse different ports or use a port-sharing mechanism, such as hello http.sys kernel driver in Windows.</span></span> <span data-ttu-id="d5a62-118">V obou případech musí být jedinečně adresovat každé instance služby nebo repliky v hostitelském procesu.</span><span class="sxs-lookup"><span data-stu-id="d5a62-118">In either case, each service instance or replica in a host process must be uniquely addressable.</span></span>

![Koncové body služby][1]

## <a name="service-discovery-and-resolution"></a><span data-ttu-id="d5a62-120">Zjišťování služby a řešení</span><span class="sxs-lookup"><span data-stu-id="d5a62-120">Service discovery and resolution</span></span>
<span data-ttu-id="d5a62-121">V distribuované systému může služby přesun z tooanother jeden počítač v čase.</span><span class="sxs-lookup"><span data-stu-id="d5a62-121">In a distributed system, services may move from one machine tooanother over time.</span></span> <span data-ttu-id="d5a62-122">K tomu dochází z různých důvodů, včetně prostředků vyrovnávání upgrady, převzetí služeb při selhání nebo Škálováním na více systémů. To znamená, že adresy koncových bodů služby změnit, protože služba hello přesune toonodes různé IP adresy a může otevřít v jiné porty, pokud služba hello používá dynamicky vybraný port.</span><span class="sxs-lookup"><span data-stu-id="d5a62-122">This can happen for various reasons, including resource balancing, upgrades, failovers, or scale-out. This means service endpoint addresses change as hello service moves toonodes with different IP addresses, and may open on different ports if hello service uses a dynamically selected port.</span></span>

![Distribuce služeb][7]

<span data-ttu-id="d5a62-124">Service Fabric nabízí zjišťování a řešení služba byla požádána hello služby DNS.</span><span class="sxs-lookup"><span data-stu-id="d5a62-124">Service Fabric provides a discovery and resolution service called hello Naming Service.</span></span> <span data-ttu-id="d5a62-125">Hello služby DNS udržuje tabulku, která mapuje toohello adresy koncových bodů, které naslouchat požadavkům na pojmenovaných instancí služby.</span><span class="sxs-lookup"><span data-stu-id="d5a62-125">hello Naming Service maintains a table that maps named service instances toohello endpoint addresses they listen on.</span></span> <span data-ttu-id="d5a62-126">Všechny instance s názvem služby v Service Fabric mít jedinečné názvy reprezentován jako identifikátory URI, například `"fabric:/MyApplication/MyService"`.</span><span class="sxs-lookup"><span data-stu-id="d5a62-126">All named service instances in Service Fabric have unique names represented as URIs, for example, `"fabric:/MyApplication/MyService"`.</span></span> <span data-ttu-id="d5a62-127">Hello hello služby nedojde ke změně názvu průběhu životnosti hello hello služby, je pouze adresy koncových bodů hello, které můžou změnit při přesunutí služby.</span><span class="sxs-lookup"><span data-stu-id="d5a62-127">hello name of hello service does not change over hello lifetime of hello service, it's only hello endpoint addresses that can change when services move.</span></span> <span data-ttu-id="d5a62-128">Toto je obdobou toowebsites, které mají konstantní adresy URL, ale kde může změnit hello IP adresu.</span><span class="sxs-lookup"><span data-stu-id="d5a62-128">This is analogous toowebsites that have constant URLs but where hello IP address may change.</span></span> <span data-ttu-id="d5a62-129">A podobně jako tooDNS na hello webu, který se přeloží adresy tooIP adresy URL webu, Service Fabric má doménového registrátora, která mapuje názvy služby, tootheir adresa koncového bodu.</span><span class="sxs-lookup"><span data-stu-id="d5a62-129">And similar tooDNS on hello web, which resolves website URLs tooIP addresses, Service Fabric has a registrar that maps service names tootheir endpoint address.</span></span>

![Koncové body služby][2]

<span data-ttu-id="d5a62-131">Řešení a připojení tooservices zahrnuje následující kroky na opakování hello:</span><span class="sxs-lookup"><span data-stu-id="d5a62-131">Resolving and connecting tooservices involves hello following steps run in a loop:</span></span>

* <span data-ttu-id="d5a62-132">**Vyřešte**: koncový bod hello Get, který služba byla publikována z hello služby DNS.</span><span class="sxs-lookup"><span data-stu-id="d5a62-132">**Resolve**: Get hello endpoint that a service has published from hello Naming Service.</span></span>
* <span data-ttu-id="d5a62-133">**Připojit**: připojení ke službám toohello přes ať protokolu používá na tohoto koncového bodu.</span><span class="sxs-lookup"><span data-stu-id="d5a62-133">**Connect**: Connect toohello service over whatever protocol it uses on that endpoint.</span></span>
* <span data-ttu-id="d5a62-134">**Opakujte**: Pokus o připojení mohou být neúspěšné pro z nejrůznějších důvodů, například pokud hello služby přesunul vzhledem k tomu, že adresa koncového bodu hello poslední čas hello byl vyřešen.</span><span class="sxs-lookup"><span data-stu-id="d5a62-134">**Retry**: A connection attempt may fail for any number of reasons, for example if hello service has moved since hello last time hello endpoint address was resolved.</span></span> <span data-ttu-id="d5a62-135">V takovém případě hello předcházející vyřešte a připojte kroky nutné toobe opakovat a tento cyklus se opakuje, dokud nebude úspěšné připojení hello.</span><span class="sxs-lookup"><span data-stu-id="d5a62-135">In that case, hello preceding resolve and connect steps need toobe retried, and this cycle is repeated until hello connection succeeds.</span></span>

## <a name="connecting-tooother-services"></a><span data-ttu-id="d5a62-136">Připojení služby tooother</span><span class="sxs-lookup"><span data-stu-id="d5a62-136">Connecting tooother services</span></span>
<span data-ttu-id="d5a62-137">Služby připojení tooeach jiné v clusteru s podporou obecně můžete přímý přístup k koncové body hello jiných služeb, protože jsou na hello hello uzly v clusteru s podporou stejné místní síti.</span><span class="sxs-lookup"><span data-stu-id="d5a62-137">Services connecting tooeach other inside a cluster generally can directly access hello endpoints of other services because hello nodes in a cluster are on hello same local network.</span></span> <span data-ttu-id="d5a62-138">toomake je snazší tooconnect mezi službami, Service Fabric nabízí další služby, které používají hello služby DNS.</span><span class="sxs-lookup"><span data-stu-id="d5a62-138">toomake is easier tooconnect between services, Service Fabric provides additional services that use hello Naming Service.</span></span> <span data-ttu-id="d5a62-139">Služba DNS a služby reverzní proxy server.</span><span class="sxs-lookup"><span data-stu-id="d5a62-139">A DNS service and a reverse proxy service.</span></span>


### <a name="dns-service"></a><span data-ttu-id="d5a62-140">Služba DNS</span><span class="sxs-lookup"><span data-stu-id="d5a62-140">DNS service</span></span>
<span data-ttu-id="d5a62-141">Vzhledem k tomu, že mnoho služeb, zejména kontejnerizované služeb, může mít název existující adresu URL, je možné tooresolve tyto pomocí hello standardní protokol DNS (nikoli hello služby DNS protocol) je velmi praktické, zejména v aplikaci "navýšení a posunutí" scénáře.</span><span class="sxs-lookup"><span data-stu-id="d5a62-141">Since many services, especially containerized services, can have an existing URL name, being able tooresolve these using hello standard DNS protocol (rather than hello Naming Service protocol) is very convenient, especially in application "lift and shift" scenarios.</span></span> <span data-ttu-id="d5a62-142">Toto je přesně službou DNS, jaké hello.</span><span class="sxs-lookup"><span data-stu-id="d5a62-142">This is exactly what hello DNS service does.</span></span> <span data-ttu-id="d5a62-143">Ji umožňuje vám toomap DNS názvy tooa název služby a proto přeložit koncový bod IP adresy.</span><span class="sxs-lookup"><span data-stu-id="d5a62-143">It enables you toomap DNS names tooa service name and hence resolve endpoint IP addresses.</span></span> 

<span data-ttu-id="d5a62-144">Jako hello uvedené v následujícím diagramu, hello služba DNS v clusteru Service Fabric hello systému mapuje názvy tooservice názvy DNS, které se pak vyřeší hello služby DNS tooreturn hello koncový bod adresy tooconnect k.</span><span class="sxs-lookup"><span data-stu-id="d5a62-144">As shown in hello following diagram, hello DNS service, running in hello Service Fabric cluster, maps DNS names tooservice names which are then resolved by hello Naming Service tooreturn hello endpoint addresses tooconnect to.</span></span> <span data-ttu-id="d5a62-145">v době vytvoření hello je zadaný název DNS Hello služby hello.</span><span class="sxs-lookup"><span data-stu-id="d5a62-145">hello DNS name for hello service is provided at hello time of creation.</span></span> 

![Koncové body služby][9]

<span data-ttu-id="d5a62-147">Další informace o jak zjistit, toouse hello služba DNS [služba DNS v Azure Service Fabric](service-fabric-dnsservice.md) článku.</span><span class="sxs-lookup"><span data-stu-id="d5a62-147">For more details on how toouse hello DNS service see [DNS service in Azure Service Fabric](service-fabric-dnsservice.md) article.</span></span>

### <a name="reverse-proxy-service"></a><span data-ttu-id="d5a62-148">Reverzní proxy server služby</span><span class="sxs-lookup"><span data-stu-id="d5a62-148">Reverse proxy service</span></span>
<span data-ttu-id="d5a62-149">reverzní proxy server Hello adresy služeb v hello clusteru, který zveřejňuje koncové body HTTP včetně HTTPS.</span><span class="sxs-lookup"><span data-stu-id="d5a62-149">hello reverse proxy addresses services in hello cluster that exposes HTTP endpoints including HTTPS.</span></span> <span data-ttu-id="d5a62-150">reverzní proxy server Hello výrazně zjednodušuje volání jiných služeb a své metody tak, že konkrétní formátu identifikátoru URI a zpracovává hello vyřešit, připojení, opakujte kroky potřebné k toocommunicate jednu službu s použitím jiného hello pojmenování služby.</span><span class="sxs-lookup"><span data-stu-id="d5a62-150">hello reverse proxy greatly simplifies calling other services and their methods by having a specific URI format and handles hello resolve, connect, retry steps required for one service toocommunicate with another using hello Naming Serivce.</span></span> <span data-ttu-id="d5a62-151">Jinými slovy skryje hello Naming Service od vás při volání metody jiných služeb tím, že to stejně jednoduché jako volání adresu URL.</span><span class="sxs-lookup"><span data-stu-id="d5a62-151">In other words, it hides hello Naming Service from you when calling other services by making this as simple as calling a URL.</span></span>

![Koncové body služby][10]

<span data-ttu-id="d5a62-153">Další informace o jak toouse hello reverse službu proxy serveru najdete v tématu [reverzní proxy server v Azure Service Fabric](service-fabric-reverseproxy.md) článku.</span><span class="sxs-lookup"><span data-stu-id="d5a62-153">For more details on how toouse hello reverse proxy service see [Reverse proxy in Azure Service Fabric](service-fabric-reverseproxy.md) article.</span></span>

## <a name="connections-from-external-clients"></a><span data-ttu-id="d5a62-154">Připojení klientů, externí</span><span class="sxs-lookup"><span data-stu-id="d5a62-154">Connections from external clients</span></span>
<span data-ttu-id="d5a62-155">Služby připojení tooeach jiné v clusteru s podporou obecně můžete přímý přístup k koncové body hello jiných služeb, protože jsou na hello hello uzly v clusteru s podporou stejné místní síti.</span><span class="sxs-lookup"><span data-stu-id="d5a62-155">Services connecting tooeach other inside a cluster generally can directly access hello endpoints of other services because hello nodes in a cluster are on hello same local network.</span></span> <span data-ttu-id="d5a62-156">V některých prostředích však clusteru může být za nástroj pro vyrovnávání zatížení, který směruje externí příchozí provoz prostřednictvím omezenou sadu portů.</span><span class="sxs-lookup"><span data-stu-id="d5a62-156">In some environments, however, a cluster may be behind a load balancer that routes external ingress traffic through a limited set of ports.</span></span> <span data-ttu-id="d5a62-157">V těchto případech můžete služby stále vzájemně komunikovat a vyřešit hello adresy pomocí služby DNS, ale dodatečné kroky musí být tooservices tooconnect přijatá tooallow externích klientů.</span><span class="sxs-lookup"><span data-stu-id="d5a62-157">In these cases, services can still communicate with each other and resolve addresses using hello Naming Service, but extra steps must be taken tooallow external clients tooconnect tooservices.</span></span>

## <a name="service-fabric-in-azure"></a><span data-ttu-id="d5a62-158">Service Fabric v Azure</span><span class="sxs-lookup"><span data-stu-id="d5a62-158">Service Fabric in Azure</span></span>
<span data-ttu-id="d5a62-159">Cluster Service Fabric v Azure je umístěn za pro vyrovnávání zatížení Azure.</span><span class="sxs-lookup"><span data-stu-id="d5a62-159">A Service Fabric cluster in Azure is placed behind an Azure Load Balancer.</span></span> <span data-ttu-id="d5a62-160">Všechny externí přenosy toohello cluster musí projít prostřednictvím hello nástroj pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="d5a62-160">All external traffic toohello cluster must pass through hello load balancer.</span></span> <span data-ttu-id="d5a62-161">Hello nástroj pro vyrovnávání zatížení bude automaticky přeposílal provoz na daného portu tooa náhodných příchozí *uzlu* má hello otevřete stejný port.</span><span class="sxs-lookup"><span data-stu-id="d5a62-161">hello load balancer will automatically forward traffic inbound on a given port tooa random *node* that has hello same port open.</span></span> <span data-ttu-id="d5a62-162">Hello nástroj pro vyrovnávání zatížení Azure pouze zná na hello otevřené porty *uzly*, neví o otevřené porty fyzická *služby*.</span><span class="sxs-lookup"><span data-stu-id="d5a62-162">hello Azure Load Balancer only knows about ports open on hello *nodes*, it does not know about ports open by individual *services*.</span></span>

![Azure topologie pro vyrovnávání zatížení a Service Fabric][3]

<span data-ttu-id="d5a62-164">Například v pořadí tooaccept externí přenosy na portu **80**, musí být nakonfigurované hello následující věci:</span><span class="sxs-lookup"><span data-stu-id="d5a62-164">For example, in order tooaccept external traffic on port **80**, hello following things must be configured:</span></span>

1. <span data-ttu-id="d5a62-165">Zápis služba, která naslouchá na portu 80.</span><span class="sxs-lookup"><span data-stu-id="d5a62-165">Write a service that listens on port 80.</span></span> <span data-ttu-id="d5a62-166">Nakonfigurujte port 80 v ServiceManifest.xml hello služby a otevřete naslouchací proces ve službě service hello, například vlastním hostováním webový server.</span><span class="sxs-lookup"><span data-stu-id="d5a62-166">Configure port 80 in hello service's ServiceManifest.xml and open a listener in hello service, for example, a self-hosted web server.</span></span>

    ```xml
    <Resources>
        <Endpoints>
            <Endpoint Name="WebEndpoint" Protocol="http" Port="80" />
        </Endpoints>
    </Resources>
    ```
    ```csharp
        class HttpCommunicationListener : ICommunicationListener
        {
            ...

            public Task<string> OpenAsync(CancellationToken cancellationToken)
            {
                EndpointResourceDescription endpoint =
                    serviceContext.CodePackageActivationContext.GetEndpoint("WebEndpoint");

                string uriPrefix = $"{endpoint.Protocol}://+:{endpoint.Port}/myapp/";

                this.httpListener = new HttpListener();
                this.httpListener.Prefixes.Add(uriPrefix);
                this.httpListener.Start();

                string publishUri = uriPrefix.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);
                return Task.FromResult(publishUri);
            }

            ...
        }

        class WebService : StatelessService
        {
            ...

            protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
            {
                return new[] { new ServiceInstanceListener(context => new HttpCommunicationListener(context))};
            }

            ...
        }
    ```
    ```java
        class HttpCommunicationlistener implements CommunicationListener {
            ...

            @Override
            public CompletableFuture<String> openAsync(CancellationToken arg0) {
                EndpointResourceDescription endpoint =
                    this.serviceContext.getCodePackageActivationContext().getEndpoint("WebEndpoint");
                try {
                    HttpServer server = com.sun.net.httpserver.HttpServer.create(new InetSocketAddress(endpoint.getPort()), 0);
                    server.start();

                    String publishUri = String.format("http://%s:%d/",
                        this.serviceContext.getNodeContext().getIpAddressOrFQDN(), endpoint.getPort());
                    return CompletableFuture.completedFuture(publishUri);
                } catch (IOException e) {
                    throw new RuntimeException(e);
                }
            }

            ...
        }

        class WebService extends StatelessService {
            ...

            @Override
            protected List<ServiceInstanceListener> createServiceInstanceListeners() {
                <ServiceInstanceListener> listeners = new ArrayList<ServiceInstanceListener>();
                listeners.add(new ServiceInstanceListener((context) -> new HttpCommunicationlistener(context)));
                return listeners;       
            }

            ...
        }
    ```
2. <span data-ttu-id="d5a62-167">Vytvořte Service Fabric Cluster v Azure a zadejte port **80** jako vlastní koncový port pro typ hello uzlu, který bude hostitelem služby hello.</span><span class="sxs-lookup"><span data-stu-id="d5a62-167">Create a Service Fabric Cluster in Azure and specify port **80** as a custom endpoint port for hello node type that will host hello service.</span></span> <span data-ttu-id="d5a62-168">Pokud máte více než jeden typ uzlu, můžete nastavit *omezení umístění* na tooensure hello služba spuštěna pouze v hello typu uzlu, který má otevřený port hello vlastní koncový bod.</span><span class="sxs-lookup"><span data-stu-id="d5a62-168">If you have more than one node type, you can set up a *placement constraint* on hello service tooensure it only runs on hello node type that has hello custom endpoint port opened.</span></span>

    ![Otevřete port typu uzlu][4]
3. <span data-ttu-id="d5a62-170">Po vytvoření clusteru hello, nakonfigurujte v clusteru hello skupiny prostředků tooforward přenosy na portu 80 hello nástroj pro vyrovnávání zatížení Azure.</span><span class="sxs-lookup"><span data-stu-id="d5a62-170">Once hello cluster has been created, configure hello Azure Load Balancer in hello cluster's Resource Group tooforward traffic on port 80.</span></span> <span data-ttu-id="d5a62-171">Při vytváření clusteru prostřednictvím hello portálu Azure, to je nastavený automaticky pro každé vlastní koncový port, který byl nakonfigurovaný.</span><span class="sxs-lookup"><span data-stu-id="d5a62-171">When creating a cluster through hello Azure portal, this is set up automatically for each custom endpoint port that was configured.</span></span>

    ![Přenos dat v hello nástroj pro vyrovnávání zatížení Azure][5]
4. <span data-ttu-id="d5a62-173">Test toodetermine jestli Hello používá Azure nástroj pro vyrovnávání zatížení nebo není toosend provozu tooa konkrétním uzlu.</span><span class="sxs-lookup"><span data-stu-id="d5a62-173">hello Azure Load Balancer uses a probe toodetermine whether or not toosend traffic tooa particular node.</span></span> <span data-ttu-id="d5a62-174">Hello testů pravidelně kontroluje koncový bod na každý uzel toodetermine, jestli odpovídá hello uzlu.</span><span class="sxs-lookup"><span data-stu-id="d5a62-174">hello probe periodically checks an endpoint on each node toodetermine whether or not hello node is responding.</span></span> <span data-ttu-id="d5a62-175">Pokud hello test nezdaří tooreceive odpověď po nakonfigurovanou stanovený počet, nástroj pro vyrovnávání zatížení hello zastaví odesílání přenosů toothat uzlu.</span><span class="sxs-lookup"><span data-stu-id="d5a62-175">If hello probe fails tooreceive a response after a configured number of times, hello load balancer stops sending traffic toothat node.</span></span> <span data-ttu-id="d5a62-176">Při vytváření clusteru prostřednictvím hello portálu Azure, sondu se automaticky nastaví pro každý koncový bod vlastní port, který byl nakonfigurován.</span><span class="sxs-lookup"><span data-stu-id="d5a62-176">When creating a cluster through hello Azure portal, a probe is automatically set up for each custom endpoint port that was configured.</span></span>

    ![Přenos dat v hello nástroj pro vyrovnávání zatížení Azure][8]

<span data-ttu-id="d5a62-178">Je důležité tooremember, který hello nástroj pro vyrovnávání zatížení Azure a kontroly hello pouze vědět o hello *uzly*, není hello *služby* spuštěná na uzlech hello.</span><span class="sxs-lookup"><span data-stu-id="d5a62-178">It's important tooremember that hello Azure Load Balancer and hello probe only know about hello *nodes*, not hello *services* running on hello nodes.</span></span> <span data-ttu-id="d5a62-179">Hello nástroj pro vyrovnávání zatížení Azure vždycky odešlou toonodes provoz, který reagovat toohello testu, takže se musí věnovat tooensure je k dispozici v hello uzlů, které jsou možné toorespond toohello testu.</span><span class="sxs-lookup"><span data-stu-id="d5a62-179">hello Azure Load Balancer will always send traffic toonodes that respond toohello probe, so care must be taken tooensure services are available on hello nodes that are able toorespond toohello probe.</span></span>

## <a name="reliable-services-built-in-communication-api-options"></a><span data-ttu-id="d5a62-180">Spolehlivé služby: Možnosti integrované komunikace rozhraní API</span><span class="sxs-lookup"><span data-stu-id="d5a62-180">Reliable Services: Built-in communication API options</span></span>
<span data-ttu-id="d5a62-181">Spolehlivé služby framework Hello se dodává s několik předdefinovaných komunikace možností.</span><span class="sxs-lookup"><span data-stu-id="d5a62-181">hello Reliable Services framework ships with several pre-built communication options.</span></span> <span data-ttu-id="d5a62-182">Hello rozhodnutí o tom, které jeden bude nejlépe fungovat pro vás závisí na volbu hello hello programování modelu, hello komunikace framework a hello programovací jazyk, který vašim službám, které jsou napsané v.</span><span class="sxs-lookup"><span data-stu-id="d5a62-182">hello decision about which one will work best for you depends on hello choice of hello programming model, hello communication framework, and hello programming language that your services are written in.</span></span>

* <span data-ttu-id="d5a62-183">**Žádný konkrétní protokol:** Pokud nemáte konkrétní volbu komunikace framework, ale chcete tooget něco nahoru a spuštěná rychle, je hello ideální možnost pro vás [vzdálené komunikace služby](service-fabric-reliable-services-communication-remoting.md), což umožňuje silně typované vzdálené volání procedur pro Reliable Services a Reliable Actors.</span><span class="sxs-lookup"><span data-stu-id="d5a62-183">**No specific protocol:**  If you don't have a particular choice of communication framework, but you want tooget something up and running quickly, then hello ideal option for you is [service remoting](service-fabric-reliable-services-communication-remoting.md), which allows strongly-typed remote procedure calls for Reliable Services and Reliable Actors.</span></span> <span data-ttu-id="d5a62-184">Toto je nejjednodušší hello a tooget nejrychlejší způsob, jak začít s komunikace služby.</span><span class="sxs-lookup"><span data-stu-id="d5a62-184">This is hello easiest and fastest way tooget started with service communication.</span></span> <span data-ttu-id="d5a62-185">Vzdálená komunikace služby provádí překlad adres služby, připojení, zkuste to znovu a zpracování chyb.</span><span class="sxs-lookup"><span data-stu-id="d5a62-185">Service remoting handles resolution of service addresses, connection, retry, and error handling.</span></span> <span data-ttu-id="d5a62-186">Toto je k dispozici pro C# a aplikací v jazyce Java.</span><span class="sxs-lookup"><span data-stu-id="d5a62-186">This is available for both C# and Java applications.</span></span>
* <span data-ttu-id="d5a62-187">**HTTP**: pro jazykově nezávislého komunikaci protokolu HTTP poskytuje na standardní volbu s nástroji a servery HTTP, které jsou k dispozici v mnoha různých jazycích vše s podporou Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="d5a62-187">**HTTP**: For language-agnostic communication, HTTP provides an industry-standard choice with tools and HTTP servers available in many different languages, all supported by Service Fabric.</span></span> <span data-ttu-id="d5a62-188">Služby mohou používat jakékoli zásobník protokolu HTTP k dispozici, včetně [rozhraní ASP.NET Web API](service-fabric-reliable-services-communication-webapi.md) pro aplikací v C#.</span><span class="sxs-lookup"><span data-stu-id="d5a62-188">Services can use any HTTP stack available, including [ASP.NET Web API](service-fabric-reliable-services-communication-webapi.md) for C# applications.</span></span> <span data-ttu-id="d5a62-189">Klienti, které jsou napsané v C# můžete využít hello `ICommunicationClient` a `ServicePartitionClient` třídy, zatímco pro jazyk Java, použijte hello `CommunicationClient` a `FabricServicePartitionClient` třídy, [pro řešení služby, připojení prostřednictvím protokolu HTTP a opakování smyčky](service-fabric-reliable-services-communication.md).</span><span class="sxs-lookup"><span data-stu-id="d5a62-189">Clients written in C# can leverage hello `ICommunicationClient` and `ServicePartitionClient` classes, whereas for Java, use hello `CommunicationClient` and `FabricServicePartitionClient` classes, [for service resolution, HTTP connections, and retry loops](service-fabric-reliable-services-communication.md).</span></span>
* <span data-ttu-id="d5a62-190">**WCF**: Pokud máte existující kód, který používá WCF jako rozhraní vaší komunikace, pak můžete použít hello `WcfCommunicationListener` na straně serveru hello a `WcfCommunicationClient` a `ServicePartitionClient` třídy pro klienta hello.</span><span class="sxs-lookup"><span data-stu-id="d5a62-190">**WCF**: If you have existing code that uses WCF as your communication framework, then you can use hello `WcfCommunicationListener` for hello server side and `WcfCommunicationClient` and `ServicePartitionClient` classes for hello client.</span></span> <span data-ttu-id="d5a62-191">To ale je k dispozici pouze pro aplikace C# na clusterech systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="d5a62-191">This however is only available for C# applications on Windows based clusters.</span></span> <span data-ttu-id="d5a62-192">Další podrobnosti najdete v tématu Tento článek [provádění WCF hello komunikačního balíku](service-fabric-reliable-services-communication-wcf.md).</span><span class="sxs-lookup"><span data-stu-id="d5a62-192">For more details, see this article about [WCF-based implementation of hello communication stack](service-fabric-reliable-services-communication-wcf.md).</span></span>

## <a name="using-custom-protocols-and-other-communication-frameworks"></a><span data-ttu-id="d5a62-193">Vlastní protokoly a další komunikaci rozhraní</span><span class="sxs-lookup"><span data-stu-id="d5a62-193">Using custom protocols and other communication frameworks</span></span>
<span data-ttu-id="d5a62-194">Služby můžete použít libovolný protokol nebo framework pro komunikaci, jestli je vlastní binární protokol přes TCP sokety nebo streamování události prostřednictvím [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) nebo [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/).</span><span class="sxs-lookup"><span data-stu-id="d5a62-194">Services can use any protocol or framework for communication, whether its a custom binary protocol over TCP sockets, or streaming events through [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) or [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/).</span></span> <span data-ttu-id="d5a62-195">Service Fabric nabízí komunikace rozhraní API, kterou můžete připojit vaše komunikačního balíku do, zatímco všechny hello práce toodiscover a připojit je abstrahované od vás.</span><span class="sxs-lookup"><span data-stu-id="d5a62-195">Service Fabric provides communication APIs that you can plug your communication stack into, while all hello work toodiscover and connect is abstracted from you.</span></span> <span data-ttu-id="d5a62-196">Najdete v článku o hello [spolehlivá služba komunikační model](service-fabric-reliable-services-communication.md) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="d5a62-196">See this article about hello [Reliable Service communication model](service-fabric-reliable-services-communication.md) for more details.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d5a62-197">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d5a62-197">Next steps</span></span>
<span data-ttu-id="d5a62-198">Další informace o konceptech hello a rozhraní API dostupná v hello [spolehlivé služby komunikační model](service-fabric-reliable-services-communication.md), pak rychle začít s [vzdálené komunikace služby](service-fabric-reliable-services-communication-remoting.md) nebo přejděte podrobný toolearn jak toowrite naslouchací proces komunikace pomocí [webového rozhraní API s OWIN hostování na vlastním](service-fabric-reliable-services-communication-webapi.md).</span><span class="sxs-lookup"><span data-stu-id="d5a62-198">Learn more about hello concepts and APIs available in hello [Reliable Services communication model](service-fabric-reliable-services-communication.md), then get started quickly with [service remoting](service-fabric-reliable-services-communication-remoting.md) or go in-depth toolearn how toowrite a communication listener using [Web API with OWIN self-host](service-fabric-reliable-services-communication-webapi.md).</span></span>

[1]: ./media/service-fabric-connect-and-communicate-with-services/serviceendpoints.png
[2]: ./media/service-fabric-connect-and-communicate-with-services/namingservice.png
[3]: ./media/service-fabric-connect-and-communicate-with-services/loadbalancertopology.png
[4]: ./media/service-fabric-connect-and-communicate-with-services/nodeport.png
[5]: ./media/service-fabric-connect-and-communicate-with-services/loadbalancerport.png
[7]: ./media/service-fabric-connect-and-communicate-with-services/distributedservices.png
[8]: ./media/service-fabric-connect-and-communicate-with-services/loadbalancerprobe.png
[9]: ./media/service-fabric-connect-and-communicate-with-services/dns.png
[10]: ./media/service-fabric-reverseproxy/internal-communication.png
