---
title: "Připojení a komunikovat se službami v Azure Service Fabric | Microsoft Docs"
description: "Zjistěte, jak vyřešit, připojit a komunikovat se službami v Service Fabric."
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
ms.openlocfilehash: 3e61ad19df34c6a57da43e26bd2ab9d7ecdbf98e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="connect-and-communicate-with-services-in-service-fabric"></a><span data-ttu-id="97fbd-103">Připojení a komunikovat se službami v Service Fabric</span><span class="sxs-lookup"><span data-stu-id="97fbd-103">Connect and communicate with services in Service Fabric</span></span>
<span data-ttu-id="97fbd-104">V Service Fabric běží služba někde v clusteru Service Fabric, obvykle se distribuují napříč více virtuálními počítači.</span><span class="sxs-lookup"><span data-stu-id="97fbd-104">In Service Fabric, a service runs somewhere in a Service Fabric cluster, typically distributed across multiple VMs.</span></span> <span data-ttu-id="97fbd-105">Ho může být z jednoho místa na jiný přesunout, pomocí vlastníka služby, nebo automaticky pomocí Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="97fbd-105">It can be moved from one place to another, either by the service owner, or automatically by Service Fabric.</span></span> <span data-ttu-id="97fbd-106">Služby nejsou staticky vázaný na konkrétní počítač nebo na adresu.</span><span class="sxs-lookup"><span data-stu-id="97fbd-106">Services are not statically tied to a particular machine or address.</span></span>

<span data-ttu-id="97fbd-107">Aplikace Service Fabric se obvykle skládá z mnoha různých služeb, kde každá služba provádí úlohu specializované.</span><span class="sxs-lookup"><span data-stu-id="97fbd-107">A Service Fabric application is generally composed of many different services, where each service performs a specialized task.</span></span> <span data-ttu-id="97fbd-108">Tyto služby může komunikovat s jinými k dokončení funkce, jako je například vykreslování různé části webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="97fbd-108">These services may communicate with each other to form a complete function, such as rendering different parts of a web application.</span></span> <span data-ttu-id="97fbd-109">Existují také klientské aplikace, které se připojují k a komunikovat se službami.</span><span class="sxs-lookup"><span data-stu-id="97fbd-109">There are also client applications that connect to and communicate with services.</span></span> <span data-ttu-id="97fbd-110">Tento dokument popisuje, jak nastavit komunikaci s a mezi vaší služby v Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="97fbd-110">This document discusses how to set up communication with and between your services in Service Fabric.</span></span>

<span data-ttu-id="97fbd-111">Toto video Microsoft Virtual Academy taky popisuje komunikace služby:<center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=iYFCk76yC_6706218965"></span><span class="sxs-lookup"><span data-stu-id="97fbd-111">This Microsoft Virtual Academy video also discusses service communication: <center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=iYFCk76yC_6706218965"></span></span>  
<img src="./media/service-fabric-connect-and-communicate-with-services/CommunicationVid.png" WIDTH="360" HEIGHT="244">  
</a></center>

## <a name="bring-your-own-protocol"></a><span data-ttu-id="97fbd-112">Přineste si vlastní protokol</span><span class="sxs-lookup"><span data-stu-id="97fbd-112">Bring your own protocol</span></span>
<span data-ttu-id="97fbd-113">Service Fabric pomáhá spravovat životní cyklus vašim službám, ale neprovede rozhodnutí o co dělat, vaše služby.</span><span class="sxs-lookup"><span data-stu-id="97fbd-113">Service Fabric helps manage the lifecycle of your services but it does not make decisions about what your services do.</span></span> <span data-ttu-id="97fbd-114">To zahrnuje komunikaci.</span><span class="sxs-lookup"><span data-stu-id="97fbd-114">This includes communication.</span></span> <span data-ttu-id="97fbd-115">Při služby se otevře pomocí Service Fabric, která je vaše služba příležitost k nastavit koncový bod pro příchozí požadavky a pomocí ať protokolu nebo komunikace zásobníku chcete.</span><span class="sxs-lookup"><span data-stu-id="97fbd-115">When your service is opened by Service Fabric, that's your service's opportunity to set up an endpoint for incoming requests, using whatever protocol or communication stack you want.</span></span> <span data-ttu-id="97fbd-116">Vaše služba bude naslouchat na normální **IP: port** adres pomocí žádné schéma adresování, jako je identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="97fbd-116">Your service will listen on a normal **IP:port** address using any addressing scheme, such as a URI.</span></span> <span data-ttu-id="97fbd-117">Více instancí služby nebo repliky může sdílet procesu hostitele, v takovém případě bude buď potřebovat používají jiné porty nebo použijte mechanismu sdílení portů, jako je ovladač http.sys jádra v systému Windows.</span><span class="sxs-lookup"><span data-stu-id="97fbd-117">Multiple service instances or replicas may share a host process, in which case they will either need to use different ports or use a port-sharing mechanism, such as the http.sys kernel driver in Windows.</span></span> <span data-ttu-id="97fbd-118">V obou případech musí být jedinečně adresovat každé instance služby nebo repliky v hostitelském procesu.</span><span class="sxs-lookup"><span data-stu-id="97fbd-118">In either case, each service instance or replica in a host process must be uniquely addressable.</span></span>

![Koncové body služby][1]

## <a name="service-discovery-and-resolution"></a><span data-ttu-id="97fbd-120">Zjišťování služby a řešení</span><span class="sxs-lookup"><span data-stu-id="97fbd-120">Service discovery and resolution</span></span>
<span data-ttu-id="97fbd-121">V systému distribuované služby může přesunout z jednoho počítače do jiného v čase.</span><span class="sxs-lookup"><span data-stu-id="97fbd-121">In a distributed system, services may move from one machine to another over time.</span></span> <span data-ttu-id="97fbd-122">K tomu dochází z různých důvodů, včetně prostředků vyrovnávání upgrady, převzetí služeb při selhání nebo Škálováním na více systémů.</span><span class="sxs-lookup"><span data-stu-id="97fbd-122">This can happen for various reasons, including resource balancing, upgrades, failovers, or scale-out.</span></span> <span data-ttu-id="97fbd-123">To znamená, že adresy koncových bodů služby změnit, protože služba přesune do uzlů se různých IP adres a může otevřít v jiné porty, pokud služba používá dynamicky vybraný port.</span><span class="sxs-lookup"><span data-stu-id="97fbd-123">This means service endpoint addresses change as the service moves to nodes with different IP addresses, and may open on different ports if the service uses a dynamically selected port.</span></span>

![Distribuce služeb][7]

<span data-ttu-id="97fbd-125">Service Fabric nabízí zjišťování a řešení služby názvem služby DNS.</span><span class="sxs-lookup"><span data-stu-id="97fbd-125">Service Fabric provides a discovery and resolution service called the Naming Service.</span></span> <span data-ttu-id="97fbd-126">Služby DNS udržuje tabulku, která mapuje pojmenovaných instancí služby na koncový bod adresy, které budou naslouchat na portu.</span><span class="sxs-lookup"><span data-stu-id="97fbd-126">The Naming Service maintains a table that maps named service instances to the endpoint addresses they listen on.</span></span> <span data-ttu-id="97fbd-127">Všechny instance s názvem služby v Service Fabric mít jedinečné názvy reprezentován jako identifikátory URI, například `"fabric:/MyApplication/MyService"`.</span><span class="sxs-lookup"><span data-stu-id="97fbd-127">All named service instances in Service Fabric have unique names represented as URIs, for example, `"fabric:/MyApplication/MyService"`.</span></span> <span data-ttu-id="97fbd-128">Název služby se nemění během životního cyklu služby, je pouze adresy koncových bodů, které můžou změnit při přesunutí služby.</span><span class="sxs-lookup"><span data-stu-id="97fbd-128">The name of the service does not change over the lifetime of the service, it's only the endpoint addresses that can change when services move.</span></span> <span data-ttu-id="97fbd-129">Toto je obdobou weby, které mají konstantní adresy URL, ale kde může změnit IP adresu.</span><span class="sxs-lookup"><span data-stu-id="97fbd-129">This is analogous to websites that have constant URLs but where the IP address may change.</span></span> <span data-ttu-id="97fbd-130">A podobně jako DNS na webu, který přeloží adresy URL webu na IP adresy, Service Fabric má doménového registrátora, která mapuje názvy služby jejich adresa koncového bodu.</span><span class="sxs-lookup"><span data-stu-id="97fbd-130">And similar to DNS on the web, which resolves website URLs to IP addresses, Service Fabric has a registrar that maps service names to their endpoint address.</span></span>

![Koncové body služby][2]

<span data-ttu-id="97fbd-132">Řešení a připojené ke službám zahrnuje následující kroky na opakování:</span><span class="sxs-lookup"><span data-stu-id="97fbd-132">Resolving and connecting to services involves the following steps run in a loop:</span></span>

* <span data-ttu-id="97fbd-133">**Vyřešte**: získání koncového bodu, který publikovali služby ze služby DNS.</span><span class="sxs-lookup"><span data-stu-id="97fbd-133">**Resolve**: Get the endpoint that a service has published from the Naming Service.</span></span>
* <span data-ttu-id="97fbd-134">**Připojit**: připojit ke službě přes ať protokolu používá na tohoto koncového bodu.</span><span class="sxs-lookup"><span data-stu-id="97fbd-134">**Connect**: Connect to the service over whatever protocol it uses on that endpoint.</span></span>
* <span data-ttu-id="97fbd-135">**Opakujte**: Pokus o připojení, které mohou být neúspěšné pro libovolný počet z důvodů pro příklad, pokud služba přesunul od posledního adresa koncového bodu byl vyřešen.</span><span class="sxs-lookup"><span data-stu-id="97fbd-135">**Retry**: A connection attempt may fail for any number of reasons, for example if the service has moved since the last time the endpoint address was resolved.</span></span> <span data-ttu-id="97fbd-136">V takovém případě předchozím vyřešit a připojení potřeba kroky opakovat a tento cyklus se opakuje, dokud je připojení úspěšné.</span><span class="sxs-lookup"><span data-stu-id="97fbd-136">In that case, the preceding resolve and connect steps need to be retried, and this cycle is repeated until the connection succeeds.</span></span>

## <a name="connecting-to-other-services"></a><span data-ttu-id="97fbd-137">Připojení k jiným službám</span><span class="sxs-lookup"><span data-stu-id="97fbd-137">Connecting to other services</span></span>
<span data-ttu-id="97fbd-138">Služby připojení k sobě navzájem v clusteru s podporou obecně můžete přímo přístup koncových bodů jiných služeb, protože uzly v clusteru jsou ve stejné místní síti.</span><span class="sxs-lookup"><span data-stu-id="97fbd-138">Services connecting to each other inside a cluster generally can directly access the endpoints of other services because the nodes in a cluster are on the same local network.</span></span> <span data-ttu-id="97fbd-139">Chcete-li je jednodušší připojit mezi službami, Service Fabric nabízí další služby, které používají služby DNS.</span><span class="sxs-lookup"><span data-stu-id="97fbd-139">To make is easier to connect between services, Service Fabric provides additional services that use the Naming Service.</span></span> <span data-ttu-id="97fbd-140">Služba DNS a služby reverzní proxy server.</span><span class="sxs-lookup"><span data-stu-id="97fbd-140">A DNS service and a reverse proxy service.</span></span>


### <a name="dns-service"></a><span data-ttu-id="97fbd-141">Služba DNS</span><span class="sxs-lookup"><span data-stu-id="97fbd-141">DNS service</span></span>
<span data-ttu-id="97fbd-142">Od mnoho služeb zejména kontejnerizované služeb, může mít název existující adresu URL, bude možné, chcete-li tyto standardní DNS pomocí protokolu (nikoli protokol služby DNS) je velmi praktické, zejména v scénáře "navýšení a posunutí" aplikací.</span><span class="sxs-lookup"><span data-stu-id="97fbd-142">Since many services, especially containerized services, can have an existing URL name, being able to resolve these using the standard DNS protocol (rather than the Naming Service protocol) is very convenient, especially in application "lift and shift" scenarios.</span></span> <span data-ttu-id="97fbd-143">Toto je přesně jaké služby DNS.</span><span class="sxs-lookup"><span data-stu-id="97fbd-143">This is exactly what the DNS service does.</span></span> <span data-ttu-id="97fbd-144">Umožňuje mapování názvů DNS pro název služby a proto přeložit koncový bod IP adresy.</span><span class="sxs-lookup"><span data-stu-id="97fbd-144">It enables you to map DNS names to a service name and hence resolve endpoint IP addresses.</span></span> 

<span data-ttu-id="97fbd-145">Jak je znázorněno v následujícím diagramu, služba DNS spuštěných v clusteru Service Fabric mapuje názvy služeb, které se pak vyřeší služby DNS vrátit adresy koncových bodů pro připojení k názvy DNS.</span><span class="sxs-lookup"><span data-stu-id="97fbd-145">As shown in the following diagram, the DNS service, running in the Service Fabric cluster, maps DNS names to service names which are then resolved by the Naming Service to return the endpoint addresses to connect to.</span></span> <span data-ttu-id="97fbd-146">V době vytvoření je zadaný název DNS pro službu.</span><span class="sxs-lookup"><span data-stu-id="97fbd-146">The DNS name for the service is provided at the time of creation.</span></span> 

![Koncové body služby][9]

<span data-ttu-id="97fbd-148">Pro další podrobnosti o tom, jak použít DNS služby najdete v tématu [služba DNS v Azure Service Fabric](service-fabric-dnsservice.md) článku.</span><span class="sxs-lookup"><span data-stu-id="97fbd-148">For more details on how to use the DNS service see [DNS service in Azure Service Fabric](service-fabric-dnsservice.md) article.</span></span>

### <a name="reverse-proxy-service"></a><span data-ttu-id="97fbd-149">Reverzní proxy server služby</span><span class="sxs-lookup"><span data-stu-id="97fbd-149">Reverse proxy service</span></span>
<span data-ttu-id="97fbd-150">Reverzní proxy server adresy služeb v clusteru, který zveřejňuje koncové body HTTP včetně HTTPS.</span><span class="sxs-lookup"><span data-stu-id="97fbd-150">The reverse proxy addresses services in the cluster that exposes HTTP endpoints including HTTPS.</span></span> <span data-ttu-id="97fbd-151">Výrazně zjednodušuje volání jiných služeb a jejich metody tak, že konkrétní formát identifikátoru URI a zpracovává překlad reverzní proxy server, připojení, opakujte kroky potřebné k jedné službě komunikovat pomocí služby názvy.</span><span class="sxs-lookup"><span data-stu-id="97fbd-151">The reverse proxy greatly simplifies calling other services and their methods by having a specific URI format and handles the resolve, connect, retry steps required for one service to communicate with another using the Naming Serivce.</span></span> <span data-ttu-id="97fbd-152">Jinými slovy skryje služby DNS od vás při volání metody jiných služeb tím, že to stejně jednoduché jako volání adresu URL.</span><span class="sxs-lookup"><span data-stu-id="97fbd-152">In other words, it hides the Naming Service from you when calling other services by making this as simple as calling a URL.</span></span>

![Koncové body služby][10]

<span data-ttu-id="97fbd-154">Podrobnosti o tom, jak používat službu reverzního proxy serveru najdete v části [reverzní proxy server v Azure Service Fabric](service-fabric-reverseproxy.md) článku.</span><span class="sxs-lookup"><span data-stu-id="97fbd-154">For more details on how to use the reverse proxy service see [Reverse proxy in Azure Service Fabric](service-fabric-reverseproxy.md) article.</span></span>

## <a name="connections-from-external-clients"></a><span data-ttu-id="97fbd-155">Připojení klientů, externí</span><span class="sxs-lookup"><span data-stu-id="97fbd-155">Connections from external clients</span></span>
<span data-ttu-id="97fbd-156">Služby připojení k sobě navzájem v clusteru s podporou obecně můžete přímo přístup koncových bodů jiných služeb, protože uzly v clusteru jsou ve stejné místní síti.</span><span class="sxs-lookup"><span data-stu-id="97fbd-156">Services connecting to each other inside a cluster generally can directly access the endpoints of other services because the nodes in a cluster are on the same local network.</span></span> <span data-ttu-id="97fbd-157">V některých prostředích však clusteru může být za nástroj pro vyrovnávání zatížení, který směruje externí příchozí provoz prostřednictvím omezenou sadu portů.</span><span class="sxs-lookup"><span data-stu-id="97fbd-157">In some environments, however, a cluster may be behind a load balancer that routes external ingress traffic through a limited set of ports.</span></span> <span data-ttu-id="97fbd-158">V těchto případech služby můžete stále vzájemně komunikovat a vyřešit adresy pomocí služby DNS, ale dodatečné kroky musí být přijata tak, aby externí klienti pro připojení ke službám.</span><span class="sxs-lookup"><span data-stu-id="97fbd-158">In these cases, services can still communicate with each other and resolve addresses using the Naming Service, but extra steps must be taken to allow external clients to connect to services.</span></span>

## <a name="service-fabric-in-azure"></a><span data-ttu-id="97fbd-159">Service Fabric v Azure</span><span class="sxs-lookup"><span data-stu-id="97fbd-159">Service Fabric in Azure</span></span>
<span data-ttu-id="97fbd-160">Cluster Service Fabric v Azure je umístěn za pro vyrovnávání zatížení Azure.</span><span class="sxs-lookup"><span data-stu-id="97fbd-160">A Service Fabric cluster in Azure is placed behind an Azure Load Balancer.</span></span> <span data-ttu-id="97fbd-161">Nástroje pro vyrovnávání zatížení musí projít všechny externích přenosů do clusteru.</span><span class="sxs-lookup"><span data-stu-id="97fbd-161">All external traffic to the cluster must pass through the load balancer.</span></span> <span data-ttu-id="97fbd-162">Nástroje pro vyrovnávání zatížení bude automaticky přeposílal provoz na zadaný port pro náhodnou příchozí *uzlu* má stejný port otevřete.</span><span class="sxs-lookup"><span data-stu-id="97fbd-162">The load balancer will automatically forward traffic inbound on a given port to a random *node* that has the same port open.</span></span> <span data-ttu-id="97fbd-163">Nástroje pro vyrovnávání zatížení Azure zná pouze na otevřené porty *uzly*, neví o otevřené porty fyzická *služby*.</span><span class="sxs-lookup"><span data-stu-id="97fbd-163">The Azure Load Balancer only knows about ports open on the *nodes*, it does not know about ports open by individual *services*.</span></span>

![Azure topologie pro vyrovnávání zatížení a Service Fabric][3]

<span data-ttu-id="97fbd-165">Třeba, aby bylo možné přijímat externí přenosy na portu **80**, musí být nakonfigurované následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="97fbd-165">For example, in order to accept external traffic on port **80**, the following things must be configured:</span></span>

1. <span data-ttu-id="97fbd-166">Zápis služba, která naslouchá na portu 80.</span><span class="sxs-lookup"><span data-stu-id="97fbd-166">Write a service that listens on port 80.</span></span> <span data-ttu-id="97fbd-167">Nakonfigurujte port 80 v ServiceManifest.xml služby a otevřete naslouchací proces ve službě, například vlastním hostováním webový server.</span><span class="sxs-lookup"><span data-stu-id="97fbd-167">Configure port 80 in the service's ServiceManifest.xml and open a listener in the service, for example, a self-hosted web server.</span></span>

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
2. <span data-ttu-id="97fbd-168">Vytvořte Service Fabric Cluster v Azure a zadejte port **80** jako vlastní koncový port pro typ uzlu, který bude hostitelem služby.</span><span class="sxs-lookup"><span data-stu-id="97fbd-168">Create a Service Fabric Cluster in Azure and specify port **80** as a custom endpoint port for the node type that will host the service.</span></span> <span data-ttu-id="97fbd-169">Pokud máte více než jeden typ uzlu, můžete nastavit *omezení umístění* na službu, aby měli jistotu, že lze spustit pouze na typ uzlu, který má otevřený port vlastní koncový bod.</span><span class="sxs-lookup"><span data-stu-id="97fbd-169">If you have more than one node type, you can set up a *placement constraint* on the service to ensure it only runs on the node type that has the custom endpoint port opened.</span></span>

    ![Otevřete port typu uzlu][4]
3. <span data-ttu-id="97fbd-171">Po vytvoření clusteru nakonfigurujte nástroj pro vyrovnávání zatížení Azure ve skupině prostředků clusteru, aby přeposílal provoz na portu 80.</span><span class="sxs-lookup"><span data-stu-id="97fbd-171">Once the cluster has been created, configure the Azure Load Balancer in the cluster's Resource Group to forward traffic on port 80.</span></span> <span data-ttu-id="97fbd-172">Při vytváření clusteru prostřednictvím portálu Azure, to je nastavený automaticky pro každé vlastní koncový port, který byl nakonfigurovaný.</span><span class="sxs-lookup"><span data-stu-id="97fbd-172">When creating a cluster through the Azure portal, this is set up automatically for each custom endpoint port that was configured.</span></span>

    ![Přesměrovat provoz nástroji pro vyrovnávání zatížení Azure][5]
4. <span data-ttu-id="97fbd-174">Služba Vyrovnávání zatížení Azure používá sondu k určení, zda chcete odesílat provoz na konkrétním uzlu.</span><span class="sxs-lookup"><span data-stu-id="97fbd-174">The Azure Load Balancer uses a probe to determine whether or not to send traffic to a particular node.</span></span> <span data-ttu-id="97fbd-175">Tato kontrola pravidelně kontroluje koncový bod na každý uzel k určení, zda reaguje uzlu.</span><span class="sxs-lookup"><span data-stu-id="97fbd-175">The probe periodically checks an endpoint on each node to determine whether or not the node is responding.</span></span> <span data-ttu-id="97fbd-176">Pokud tato kontrola se nepodaří po nakonfigurovanou stanovený počet, obdrží odpověď, nástroje pro vyrovnávání zatížení zastaví odesílání provozu do daného uzlu.</span><span class="sxs-lookup"><span data-stu-id="97fbd-176">If the probe fails to receive a response after a configured number of times, the load balancer stops sending traffic to that node.</span></span> <span data-ttu-id="97fbd-177">Při vytváření clusteru prostřednictvím portálu Azure, sondu se automaticky nastaví pro každý koncový bod vlastní port, který byl nakonfigurován.</span><span class="sxs-lookup"><span data-stu-id="97fbd-177">When creating a cluster through the Azure portal, a probe is automatically set up for each custom endpoint port that was configured.</span></span>

    ![Přesměrovat provoz nástroji pro vyrovnávání zatížení Azure][8]

<span data-ttu-id="97fbd-179">Je důležité si pamatovat, že nástroj pro vyrovnávání zatížení Azure a kontroly pouze vědět o *uzly*, nikoli *služby* spuštěné v jednotlivých uzlech.</span><span class="sxs-lookup"><span data-stu-id="97fbd-179">It's important to remember that the Azure Load Balancer and the probe only know about the *nodes*, not the *services* running on the nodes.</span></span> <span data-ttu-id="97fbd-180">Nástroje pro vyrovnávání zatížení Azure bude vždy odesílá data na uzly, které reagují na test, takže musí dát pozor na Ujistěte se, že služby jsou dostupné na uzlech, jež jsou schopné reagovat na tato kontrola.</span><span class="sxs-lookup"><span data-stu-id="97fbd-180">The Azure Load Balancer will always send traffic to nodes that respond to the probe, so care must be taken to ensure services are available on the nodes that are able to respond to the probe.</span></span>

## <a name="reliable-services-built-in-communication-api-options"></a><span data-ttu-id="97fbd-181">Spolehlivé služby: Možnosti integrované komunikace rozhraní API</span><span class="sxs-lookup"><span data-stu-id="97fbd-181">Reliable Services: Built-in communication API options</span></span>
<span data-ttu-id="97fbd-182">Spolehlivé služby framework se dodává s několik předdefinovaných komunikace možností.</span><span class="sxs-lookup"><span data-stu-id="97fbd-182">The Reliable Services framework ships with several pre-built communication options.</span></span> <span data-ttu-id="97fbd-183">Rozhodnutí o tom, které jeden bude nejlépe fungovat pro vás závisí na výběr programovací model, rozhraní komunikace a programovací jazyk, který vašim službám, které jsou napsané v.</span><span class="sxs-lookup"><span data-stu-id="97fbd-183">The decision about which one will work best for you depends on the choice of the programming model, the communication framework, and the programming language that your services are written in.</span></span>

* <span data-ttu-id="97fbd-184">**Žádný konkrétní protokol:** Pokud nemáte konkrétní volbu komunikace framework, ale chcete něco si rychle a spustit, je možnost ideální pro vás [vzdálené komunikace služby](service-fabric-reliable-services-communication-remoting.md), což umožňuje silně typované vzdálené volání procedur pro Reliable Services a Reliable Actors.</span><span class="sxs-lookup"><span data-stu-id="97fbd-184">**No specific protocol:**  If you don't have a particular choice of communication framework, but you want to get something up and running quickly, then the ideal option for you is [service remoting](service-fabric-reliable-services-communication-remoting.md), which allows strongly-typed remote procedure calls for Reliable Services and Reliable Actors.</span></span> <span data-ttu-id="97fbd-185">Toto je nejjednodušší a nejrychlejší způsob, jak začít s komunikace služby.</span><span class="sxs-lookup"><span data-stu-id="97fbd-185">This is the easiest and fastest way to get started with service communication.</span></span> <span data-ttu-id="97fbd-186">Vzdálená komunikace služby provádí překlad adres služby, připojení, zkuste to znovu a zpracování chyb.</span><span class="sxs-lookup"><span data-stu-id="97fbd-186">Service remoting handles resolution of service addresses, connection, retry, and error handling.</span></span> <span data-ttu-id="97fbd-187">Toto je k dispozici pro C# a aplikací v jazyce Java.</span><span class="sxs-lookup"><span data-stu-id="97fbd-187">This is available for both C# and Java applications.</span></span>
* <span data-ttu-id="97fbd-188">**HTTP**: pro jazykově nezávislého komunikaci protokolu HTTP poskytuje na standardní volbu s nástroji a servery HTTP, které jsou k dispozici v mnoha různých jazycích vše s podporou Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="97fbd-188">**HTTP**: For language-agnostic communication, HTTP provides an industry-standard choice with tools and HTTP servers available in many different languages, all supported by Service Fabric.</span></span> <span data-ttu-id="97fbd-189">Služby mohou používat jakékoli zásobník protokolu HTTP k dispozici, včetně [rozhraní ASP.NET Web API](service-fabric-reliable-services-communication-webapi.md) pro aplikací v C#.</span><span class="sxs-lookup"><span data-stu-id="97fbd-189">Services can use any HTTP stack available, including [ASP.NET Web API](service-fabric-reliable-services-communication-webapi.md) for C# applications.</span></span> <span data-ttu-id="97fbd-190">Klienti, které jsou napsané v C# můžete využít `ICommunicationClient` a `ServicePartitionClient` třídy, zatímco pro jazyk Java, použijte `CommunicationClient` a `FabricServicePartitionClient` třídy, [pro řešení služby, připojení prostřednictvím protokolu HTTP a opakování smyčky](service-fabric-reliable-services-communication.md).</span><span class="sxs-lookup"><span data-stu-id="97fbd-190">Clients written in C# can leverage the `ICommunicationClient` and `ServicePartitionClient` classes, whereas for Java, use the `CommunicationClient` and `FabricServicePartitionClient` classes, [for service resolution, HTTP connections, and retry loops](service-fabric-reliable-services-communication.md).</span></span>
* <span data-ttu-id="97fbd-191">**WCF**: Pokud máte existující kód, který používá WCF jako rozhraní vaší komunikace, pak můžete použít `WcfCommunicationListener` na straně serveru a `WcfCommunicationClient` a `ServicePartitionClient` třídy pro klienta.</span><span class="sxs-lookup"><span data-stu-id="97fbd-191">**WCF**: If you have existing code that uses WCF as your communication framework, then you can use the `WcfCommunicationListener` for the server side and `WcfCommunicationClient` and `ServicePartitionClient` classes for the client.</span></span> <span data-ttu-id="97fbd-192">To ale je k dispozici pouze pro aplikace C# na clusterech systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="97fbd-192">This however is only available for C# applications on Windows based clusters.</span></span> <span data-ttu-id="97fbd-193">Další podrobnosti najdete v tématu Tento článek [provádění WCF komunikačního balíku](service-fabric-reliable-services-communication-wcf.md).</span><span class="sxs-lookup"><span data-stu-id="97fbd-193">For more details, see this article about [WCF-based implementation of the communication stack](service-fabric-reliable-services-communication-wcf.md).</span></span>

## <a name="using-custom-protocols-and-other-communication-frameworks"></a><span data-ttu-id="97fbd-194">Vlastní protokoly a další komunikaci rozhraní</span><span class="sxs-lookup"><span data-stu-id="97fbd-194">Using custom protocols and other communication frameworks</span></span>
<span data-ttu-id="97fbd-195">Služby můžete použít libovolný protokol nebo framework pro komunikaci, jestli je vlastní binární protokol přes TCP sokety nebo streamování události prostřednictvím [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) nebo [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/).</span><span class="sxs-lookup"><span data-stu-id="97fbd-195">Services can use any protocol or framework for communication, whether its a custom binary protocol over TCP sockets, or streaming events through [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) or [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/).</span></span> <span data-ttu-id="97fbd-196">Service Fabric nabízí komunikace rozhraní API, kterou můžete připojit vaše komunikačního balíku do, zatímco všechny pracovní vyhledat a připojit je abstrahované od vás.</span><span class="sxs-lookup"><span data-stu-id="97fbd-196">Service Fabric provides communication APIs that you can plug your communication stack into, while all the work to discover and connect is abstracted from you.</span></span> <span data-ttu-id="97fbd-197">Najdete v článku [spolehlivá služba komunikační model](service-fabric-reliable-services-communication.md) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="97fbd-197">See this article about the [Reliable Service communication model](service-fabric-reliable-services-communication.md) for more details.</span></span>

## <a name="next-steps"></a><span data-ttu-id="97fbd-198">Další kroky</span><span class="sxs-lookup"><span data-stu-id="97fbd-198">Next steps</span></span>
<span data-ttu-id="97fbd-199">Další informace o konceptech a rozhraní API dostupná v [spolehlivé služby komunikační model](service-fabric-reliable-services-communication.md), pak rychle začít s [vzdálené komunikace služby](service-fabric-reliable-services-communication-remoting.md) nebo přejděte podrobné informace o psaní komunikace naslouchací proces pomocí [webového rozhraní API s OWIN hostování na vlastním](service-fabric-reliable-services-communication-webapi.md).</span><span class="sxs-lookup"><span data-stu-id="97fbd-199">Learn more about the concepts and APIs available in the [Reliable Services communication model](service-fabric-reliable-services-communication.md), then get started quickly with [service remoting](service-fabric-reliable-services-communication-remoting.md) or go in-depth to learn how to write a communication listener using [Web API with OWIN self-host](service-fabric-reliable-services-communication-webapi.md).</span></span>

[1]: ./media/service-fabric-connect-and-communicate-with-services/serviceendpoints.png
[2]: ./media/service-fabric-connect-and-communicate-with-services/namingservice.png
[3]: ./media/service-fabric-connect-and-communicate-with-services/loadbalancertopology.png
[4]: ./media/service-fabric-connect-and-communicate-with-services/nodeport.png
[5]: ./media/service-fabric-connect-and-communicate-with-services/loadbalancerport.png
[7]: ./media/service-fabric-connect-and-communicate-with-services/distributedservices.png
[8]: ./media/service-fabric-connect-and-communicate-with-services/loadbalancerprobe.png
[9]: ./media/service-fabric-connect-and-communicate-with-services/dns.png
[10]: ./media/service-fabric-reverseproxy/internal-communication.png
