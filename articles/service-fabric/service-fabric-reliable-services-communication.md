---
title: "Spolehlivé služby communication – přehled | Microsoft Docs"
description: "Přehled modelu komunikace spolehlivé služby, včetně otevírání moduly pro naslouchání na službách, vyřešte koncových bodů a komunikaci mezi službami."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: BharatNarasimman
ms.assetid: 36217988-420e-409d-b0a4-e0e875b6eac8
ms.service: service-fabric
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 04/07/2017
ms.author: vturecek
ms.openlocfilehash: b418904f50b772c12bfcdbb95beb9312c8b9fb00
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-the-reliable-services-communication-apis"></a><span data-ttu-id="c7e69-103">Jak používat rozhraní API komunikaci spolehlivé služby</span><span class="sxs-lookup"><span data-stu-id="c7e69-103">How to use the Reliable Services communication APIs</span></span>
<span data-ttu-id="c7e69-104">Azure Service Fabric jako platformu je zcela lhostejné o komunikaci mezi službami.</span><span class="sxs-lookup"><span data-stu-id="c7e69-104">Azure Service Fabric as a platform is completely agnostic about communication between services.</span></span> <span data-ttu-id="c7e69-105">Všechny protokoly a zásobníky jsou přijatelné, z UDP do HTTP.</span><span class="sxs-lookup"><span data-stu-id="c7e69-105">All protocols and stacks are acceptable, from UDP to HTTP.</span></span> <span data-ttu-id="c7e69-106">Je to na vývojáře služby zvolit komunikace služby.</span><span class="sxs-lookup"><span data-stu-id="c7e69-106">It's up to the service developer to choose how services should communicate.</span></span> <span data-ttu-id="c7e69-107">Rozhraní spolehlivé služby poskytuje zásobníky předdefinované komunikaci, jakož i rozhraní API, které můžete použít k vytvoření vlastních komunikační součásti.</span><span class="sxs-lookup"><span data-stu-id="c7e69-107">The Reliable Services application framework provides built-in communication stacks as well as APIs that you can use to build your custom communication components.</span></span>

## <a name="set-up-service-communication"></a><span data-ttu-id="c7e69-108">Nastavení komunikace služby</span><span class="sxs-lookup"><span data-stu-id="c7e69-108">Set up service communication</span></span>
<span data-ttu-id="c7e69-109">Spolehlivé služby API používá jednoduché rozhraní pro komunikace služby.</span><span class="sxs-lookup"><span data-stu-id="c7e69-109">The Reliable Services API uses a simple interface for service communication.</span></span> <span data-ttu-id="c7e69-110">Chcete-li spustit koncový bod služby, jednoduše toto rozhraní implementujte:</span><span class="sxs-lookup"><span data-stu-id="c7e69-110">To open an endpoint for your service, simply implement this interface:</span></span>

```csharp

public interface ICommunicationListener
{
    Task<string> OpenAsync(CancellationToken cancellationToken);

    Task CloseAsync(CancellationToken cancellationToken);

    void Abort();
}

```

```java
public interface CommunicationListener {
    CompletableFuture<String> openAsync(CancellationToken cancellationToken);

    CompletableFuture<?> closeAsync(CancellationToken cancellationToken);

    void abort();
}
```

<span data-ttu-id="c7e69-111">Poté můžete přidat implementaci naslouchací proces komunikace vrácením v přepsání metody založené na službě třídy.</span><span class="sxs-lookup"><span data-stu-id="c7e69-111">You can then add your communication listener implementation by returning it in a service-based class method override.</span></span>

<span data-ttu-id="c7e69-112">Pro bezstavové služby:</span><span class="sxs-lookup"><span data-stu-id="c7e69-112">For stateless services:</span></span>

```csharp
class MyStatelessService : StatelessService
{
    protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        ...
    }
    ...
}
```
```java
public class MyStatelessService extends StatelessService {

    @Override
    protected List<ServiceInstanceListener> createServiceInstanceListeners() {
        ...
    }
    ...
}
```

<span data-ttu-id="c7e69-113">Pro stavové služby:</span><span class="sxs-lookup"><span data-stu-id="c7e69-113">For stateful services:</span></span>

> [!NOTE]
> <span data-ttu-id="c7e69-114">Stavová spolehlivé služby ještě nepodporuje v jazyce Java.</span><span class="sxs-lookup"><span data-stu-id="c7e69-114">Stateful reliable services are not supported in Java yet.</span></span>
>
>

```csharp
class MyStatefulService : StatefulService
{
    protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
    {
        ...
    }
    ...
}
```

<span data-ttu-id="c7e69-115">V obou případech vracet kolekci naslouchacího procesu.</span><span class="sxs-lookup"><span data-stu-id="c7e69-115">In both cases, you return a collection of listeners.</span></span> <span data-ttu-id="c7e69-116">To umožňuje služba naslouchala na několik koncových bodů, potenciálně pomocí různých protokolů, s použitím více naslouchací procesy.</span><span class="sxs-lookup"><span data-stu-id="c7e69-116">This allows your service to listen on multiple endpoints, potentially using different protocols, by using multiple listeners.</span></span> <span data-ttu-id="c7e69-117">Například můžete mít naslouchací proces protokolu HTTP a samostatné naslouchací proces protokolu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="c7e69-117">For example, you may have an HTTP listener and a separate WebSocket listener.</span></span> <span data-ttu-id="c7e69-118">Každý naslouchací proces získá název a výsledný kolekce *název: adresa* páry jsou reprezentovány jako objekt JSON, když klient požádá o naslouchání adresy pro instance služby nebo oddíl.</span><span class="sxs-lookup"><span data-stu-id="c7e69-118">Each listener gets a name, and the resulting collection of *name : address* pairs are represented as a JSON object when a client requests the listening addresses for a service instance or a partition.</span></span>

<span data-ttu-id="c7e69-119">Přepsání v bezstavové služby vrátí kolekci ServiceInstanceListeners.</span><span class="sxs-lookup"><span data-stu-id="c7e69-119">In a stateless service, the override returns a collection of ServiceInstanceListeners.</span></span> <span data-ttu-id="c7e69-120">A `ServiceInstanceListener` obsahuje funkci pro vytvoření `ICommunicationListener(C#) / CommunicationListener(Java)` a název.</span><span class="sxs-lookup"><span data-stu-id="c7e69-120">A `ServiceInstanceListener` contains a function to create an `ICommunicationListener(C#) / CommunicationListener(Java)` and gives it a name.</span></span> <span data-ttu-id="c7e69-121">Přepsání pro stavové služby, vrátí kolekci ServiceReplicaListeners.</span><span class="sxs-lookup"><span data-stu-id="c7e69-121">For stateful services, the override returns a collection of ServiceReplicaListeners.</span></span> <span data-ttu-id="c7e69-122">To se mírně liší od jeho protějšku bezstavové, protože `ServiceReplicaListener` má možnost otevření `ICommunicationListener` na sekundárních replikách.</span><span class="sxs-lookup"><span data-stu-id="c7e69-122">This is slightly different from its stateless counterpart, because a `ServiceReplicaListener` has an option to open an `ICommunicationListener` on secondary replicas.</span></span> <span data-ttu-id="c7e69-123">Nejenže můžete použít více naslouchací procesy komunikace ve službě, ale můžete také určit, který naslouchací procesy přijímat požadavky na sekundárních replikách a ty, které naslouchat pouze primární repliky.</span><span class="sxs-lookup"><span data-stu-id="c7e69-123">Not only can you use multiple communication listeners in a service, but you can also specify which listeners accept requests on secondary replicas and which ones listen only on primary replicas.</span></span>

<span data-ttu-id="c7e69-124">Například můžete mít ServiceRemotingListener, která přebírá volání RPC jenom na primární repliky a druhý, vlastní naslouchací proces, který přebírá čtení požadavky na sekundárních replikách prostřednictvím protokolu HTTP:</span><span class="sxs-lookup"><span data-stu-id="c7e69-124">For example, you can have a ServiceRemotingListener that takes RPC calls only on primary replicas, and a second, custom listener that takes read requests on secondary replicas over HTTP:</span></span>

```csharp
protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    return new[]
    {
        new ServiceReplicaListener(context =>
            new MyCustomHttpListener(context),
            "HTTPReadonlyEndpoint",
            true),

        new ServiceReplicaListener(context =>
            this.CreateServiceRemotingListener(context),
            "rpcPrimaryEndpoint",
            false)
    };
}
```

> [!NOTE]
> <span data-ttu-id="c7e69-125">Při vytváření více naslouchací procesy pro službu, každý naslouchací proces **musí** mít jedinečný název.</span><span class="sxs-lookup"><span data-stu-id="c7e69-125">When creating multiple listeners for a service, each listener **must** be given a unique name.</span></span>
>
>

<span data-ttu-id="c7e69-126">Nakonec popisují koncové body, které jsou požadovány pro služby [service manifest](service-fabric-application-model.md) části u koncových bodů.</span><span class="sxs-lookup"><span data-stu-id="c7e69-126">Finally, describe the endpoints that are required for the service in the [service manifest](service-fabric-application-model.md) under the section on endpoints.</span></span>

```xml
<Resources>
    <Endpoints>
      <Endpoint Name="WebServiceEndpoint" Protocol="http" Port="80" />
      <Endpoint Name="OtherServiceEndpoint" Protocol="tcp" Port="8505" />
    <Endpoints>
</Resources>

```

<span data-ttu-id="c7e69-127">Naslouchací proces komunikace můžete přístup k prostředkům koncový bod přidělených z `CodePackageActivationContext` v `ServiceContext`.</span><span class="sxs-lookup"><span data-stu-id="c7e69-127">The communication listener can access the endpoint resources allocated to it from the `CodePackageActivationContext` in the `ServiceContext`.</span></span> <span data-ttu-id="c7e69-128">Naslouchací proces potom lze spustit naslouchaly žádostem, když je otevřen.</span><span class="sxs-lookup"><span data-stu-id="c7e69-128">The listener can then start listening for requests when it is opened.</span></span>

```csharp
var codePackageActivationContext = serviceContext.CodePackageActivationContext;
var port = codePackageActivationContext.GetEndpoint("ServiceEndpoint").Port;

```
```java
CodePackageActivationContext codePackageActivationContext = serviceContext.getCodePackageActivationContext();
int port = codePackageActivationContext.getEndpoint("ServiceEndpoint").getPort();

```

> [!NOTE]
> <span data-ttu-id="c7e69-129">Koncový bod prostředků, které jsou společné pro celý balíček a jsou přiděleny pomocí Service Fabric, když je aktivován balíčku služby.</span><span class="sxs-lookup"><span data-stu-id="c7e69-129">Endpoint resources are common to the entire service package, and they are allocated by Service Fabric when the service package is activated.</span></span> <span data-ttu-id="c7e69-130">Víc replik služby hostované ve stejném ServiceHost může sdílet stejný port.</span><span class="sxs-lookup"><span data-stu-id="c7e69-130">Multiple service replicas hosted in the same ServiceHost may share the same port.</span></span> <span data-ttu-id="c7e69-131">To znamená, že by měly podporovat komunikaci naslouchací proces sdílení portů.</span><span class="sxs-lookup"><span data-stu-id="c7e69-131">This means that the communication listener should support port sharing.</span></span> <span data-ttu-id="c7e69-132">Doporučený způsob to provést, je pro komunikaci naslouchací proces pro použití oddílu, ID a ID repliky nebo instanci, když ji vygeneruje adresu naslouchání.</span><span class="sxs-lookup"><span data-stu-id="c7e69-132">The recommended way of doing this is for the communication listener to use the partition ID and replica/instance ID when it generates the listen address.</span></span>
>
>

### <a name="service-address-registration"></a><span data-ttu-id="c7e69-133">Registrace adresy služby</span><span class="sxs-lookup"><span data-stu-id="c7e69-133">Service address registration</span></span>
<span data-ttu-id="c7e69-134">Volá se služba system *služby DNS* běží na clusterů Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="c7e69-134">A system service called the *Naming Service* runs on Service Fabric clusters.</span></span> <span data-ttu-id="c7e69-135">Služby DNS je doménového registrátora služeb a jejich adresy, které naslouchá na každou instanci nebo repliky služby.</span><span class="sxs-lookup"><span data-stu-id="c7e69-135">The Naming Service is a registrar for services and their addresses that each instance or replica of the service is listening on.</span></span> <span data-ttu-id="c7e69-136">Když `OpenAsync(C#) / openAsync(Java)` metodu `ICommunicationListener(C#) / CommunicationListener(Java)` dokončí, vraťte se jeho hodnota získá registrované ve službě pojmenování.</span><span class="sxs-lookup"><span data-stu-id="c7e69-136">When the `OpenAsync(C#) / openAsync(Java)` method of an `ICommunicationListener(C#) / CommunicationListener(Java)` completes, its return value gets registered in the Naming Service.</span></span> <span data-ttu-id="c7e69-137">Tato návratovou hodnotu, která zveřejnění ve službě pojmenování je řetězec, jehož hodnota může být jakýkoli vůbec.</span><span class="sxs-lookup"><span data-stu-id="c7e69-137">This return value that gets published in the Naming Service is a string whose value can be anything at all.</span></span> <span data-ttu-id="c7e69-138">Tato hodnota řetězce je, co klienti uvidí, když vyzvou ze služby DNS pro adresu pro službu.</span><span class="sxs-lookup"><span data-stu-id="c7e69-138">This string value is what clients see when they ask for an address for the service from the Naming Service.</span></span>

```csharp
public Task<string> OpenAsync(CancellationToken cancellationToken)
{
    EndpointResourceDescription serviceEndpoint = serviceContext.CodePackageActivationContext.GetEndpoint("ServiceEndpoint");
    int port = serviceEndpoint.Port;

    this.listeningAddress = string.Format(
                CultureInfo.InvariantCulture,
                "http://+:{0}/",
                port);

    this.publishAddress = this.listeningAddress.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);

    this.webApp = WebApp.Start(this.listeningAddress, appBuilder => this.startup.Invoke(appBuilder));

    // the string returned here will be published in the Naming Service.
    return Task.FromResult(this.publishAddress);
}
```
```java
public CompletableFuture<String> openAsync(CancellationToken cancellationToken)
{
    EndpointResourceDescription serviceEndpoint = serviceContext.getCodePackageActivationContext.getEndpoint("ServiceEndpoint");
    int port = serviceEndpoint.getPort();

    this.publishAddress = String.format("http://%s:%d/", FabricRuntime.getNodeContext().getIpAddressOrFQDN(), port);

    this.webApp = new WebApp(port);
    this.webApp.start();

    /* the string returned here will be published in the Naming Service.
     */
    return CompletableFuture.completedFuture(this.publishAddress);
}
```

<span data-ttu-id="c7e69-139">Service Fabric poskytuje rozhraní API umožňující klientů a dalším službám, a pak požádejte podle názvu služby pro tuto adresu.</span><span class="sxs-lookup"><span data-stu-id="c7e69-139">Service Fabric provides APIs that allow clients and other services to then ask for this address by service name.</span></span> <span data-ttu-id="c7e69-140">To je důležité, protože není statickou adresu služby.</span><span class="sxs-lookup"><span data-stu-id="c7e69-140">This is important because the service address is not static.</span></span> <span data-ttu-id="c7e69-141">Služby se přesouvají v clusteru pro účely vyrovnávání a dostupnosti prostředků.</span><span class="sxs-lookup"><span data-stu-id="c7e69-141">Services are moved around in the cluster for resource balancing and availability purposes.</span></span> <span data-ttu-id="c7e69-142">Toto je mechanismus, který klientům umožňují překlad adresu naslouchání pro službu.</span><span class="sxs-lookup"><span data-stu-id="c7e69-142">This is the mechanism that allow clients to resolve the listening address for a service.</span></span>

> [!NOTE]
> <span data-ttu-id="c7e69-143">Kompletní návod jak napsat naslouchací proces komunikace, najdete v části [Service Fabric webového rozhraní API služby s vlastním hostování OWIN](service-fabric-reliable-services-communication-webapi.md) pro jazyk C#, zatímco pro jazyk Java můžete napsat vlastní implementaci serveru HTTP, najdete v příkladu aplikace EchoServer na https://github.com/Azure-Samples/service-fabric-java-getting-started.</span><span class="sxs-lookup"><span data-stu-id="c7e69-143">For a complete walk-through of how to write a communication listener, see [Service Fabric Web API services with OWIN self-hosting](service-fabric-reliable-services-communication-webapi.md) for C#, whereas for Java you can write your own HTTP server implementation, see EchoServer application example at https://github.com/Azure-Samples/service-fabric-java-getting-started.</span></span>
>
>

## <a name="communicating-with-a-service"></a><span data-ttu-id="c7e69-144">Komunikace se službou</span><span class="sxs-lookup"><span data-stu-id="c7e69-144">Communicating with a service</span></span>
<span data-ttu-id="c7e69-145">Spolehlivé služby API poskytuje následující knihovny zápis klienty, kteří komunikovat se službami.</span><span class="sxs-lookup"><span data-stu-id="c7e69-145">The Reliable Services API provides the following libraries to write clients that communicate with services.</span></span>

### <a name="service-endpoint-resolution"></a><span data-ttu-id="c7e69-146">Rozlišení koncového bodu služby</span><span class="sxs-lookup"><span data-stu-id="c7e69-146">Service endpoint resolution</span></span>
<span data-ttu-id="c7e69-147">Prvním krokem k komunikace se službou je k vyřešení adresu koncového bodu oddílu, nebo instance služby, kterou chcete, aby komunikoval s.</span><span class="sxs-lookup"><span data-stu-id="c7e69-147">The first step to communication with a service is to resolve an endpoint address of the partition or instance of the service you want to talk to.</span></span> <span data-ttu-id="c7e69-148">`ServicePartitionResolver(C#) / FabricServicePartitionResolver(Java)` Nástroj třída je základní Primitivum, které pomáhají klientům zjistit koncový bod služby za běhu.</span><span class="sxs-lookup"><span data-stu-id="c7e69-148">The `ServicePartitionResolver(C#) / FabricServicePartitionResolver(Java)` utility class is a basic primitive that helps clients determine the endpoint of a service at runtime.</span></span> <span data-ttu-id="c7e69-149">V terminologii Service Fabric, proces určování koncový bod služby se označuje jako *rozlišení koncového bodu služby*.</span><span class="sxs-lookup"><span data-stu-id="c7e69-149">In Service Fabric terminology, the process of determining the endpoint of a service is referred to as the *service endpoint resolution*.</span></span>

<span data-ttu-id="c7e69-150">Pro připojení ke službám v rámci clusteru, můžete vytvořit ServicePartitionResolver pomocí výchozího nastavení.</span><span class="sxs-lookup"><span data-stu-id="c7e69-150">To connect to services within a cluster, ServicePartitionResolver can be created using default settings.</span></span> <span data-ttu-id="c7e69-151">Toto je doporučený způsob většině situací:</span><span class="sxs-lookup"><span data-stu-id="c7e69-151">This is the recommended usage for most situations:</span></span>

```csharp
ServicePartitionResolver resolver = ServicePartitionResolver.GetDefault();
```
```java
FabricServicePartitionResolver resolver = FabricServicePartitionResolver.getDefault();
```

<span data-ttu-id="c7e69-152">Pro připojení ke službám v jiném clusteru, lze vytvořit ServicePartitionResolver sadu koncovým bodům clusteru brány.</span><span class="sxs-lookup"><span data-stu-id="c7e69-152">To connect to services in a different cluster, a ServicePartitionResolver can be created with a set of cluster gateway endpoints.</span></span> <span data-ttu-id="c7e69-153">Upozorňujeme, že jsou koncové body brány právě různými koncovými body pro připojení do stejného clusteru.</span><span class="sxs-lookup"><span data-stu-id="c7e69-153">Note that gateway endpoints are just different endpoints for connecting to the same cluster.</span></span> <span data-ttu-id="c7e69-154">Například:</span><span class="sxs-lookup"><span data-stu-id="c7e69-154">For example:</span></span>

```csharp
ServicePartitionResolver resolver = new  ServicePartitionResolver("mycluster.cloudapp.azure.com:19000", "mycluster.cloudapp.azure.com:19001");
```
```java
FabricServicePartitionResolver resolver = new  FabricServicePartitionResolver("mycluster.cloudapp.azure.com:19000", "mycluster.cloudapp.azure.com:19001");
```

<span data-ttu-id="c7e69-155">Alternativně `ServicePartitionResolver` možné přidělit funkce pro vytváření `FabricClient` interně použít:</span><span class="sxs-lookup"><span data-stu-id="c7e69-155">Alternatively, `ServicePartitionResolver` can be given a function for creating a `FabricClient` to use internally:</span></span>

```csharp
public delegate FabricClient CreateFabricClientDelegate();
```
```java
public FabricServicePartitionResolver(CreateFabricClient createFabricClient) {
...
}

public interface CreateFabricClient {
    public FabricClient getFabricClient();
}
```

<span data-ttu-id="c7e69-156">`FabricClient`je objekt, který se používá ke komunikaci se cluster Service Fabric pro různé operace správy v clusteru.</span><span class="sxs-lookup"><span data-stu-id="c7e69-156">`FabricClient` is the object that is used to communicate with the Service Fabric cluster for various management operations on the cluster.</span></span> <span data-ttu-id="c7e69-157">To je užitečné, pokud chcete mít větší kontrolu nad interakci překladač oddílu služby se váš cluster.</span><span class="sxs-lookup"><span data-stu-id="c7e69-157">This is useful when you want more control over how a service partition resolver interacts with your cluster.</span></span> <span data-ttu-id="c7e69-158">`FabricClient`provede ukládání do mezipaměti interně a je obecně nákladné vytvořit, takže je potřeba znovu použít `FabricClient` instance co nejvíc.</span><span class="sxs-lookup"><span data-stu-id="c7e69-158">`FabricClient` performs caching internally and is generally expensive to create, so it is important to reuse `FabricClient` instances as much as possible.</span></span>

```csharp
ServicePartitionResolver resolver = new  ServicePartitionResolver(() => CreateMyFabricClient());
```
```java
FabricServicePartitionResolver resolver = new  FabricServicePartitionResolver(() -> new CreateFabricClientImpl());
```

<span data-ttu-id="c7e69-159">Vyřešte metoda pak slouží k načtení adresu služby nebo služby u oddílů služby.</span><span class="sxs-lookup"><span data-stu-id="c7e69-159">A resolve method is then used to retrieve the address of a service or a service partition for partitioned services.</span></span>

```csharp
ServicePartitionResolver resolver = ServicePartitionResolver.GetDefault();

ResolvedServicePartition partition =
    await resolver.ResolveAsync(new Uri("fabric:/MyApp/MyService"), new ServicePartitionKey(), cancellationToken);
```
```java
FabricServicePartitionResolver resolver = FabricServicePartitionResolver.getDefault();

CompletableFuture<ResolvedServicePartition> partition =
    resolver.resolveAsync(new URI("fabric:/MyApp/MyService"), new ServicePartitionKey());
```

<span data-ttu-id="c7e69-160">Adresa služby lze vyřešit pomocí snadno ServicePartitionResolver, ale je potřeba další práci zajistěte, aby byl že vyřešen adresu je možné použít správně.</span><span class="sxs-lookup"><span data-stu-id="c7e69-160">A service address can be resolved easily using a ServicePartitionResolver, but more work is required to ensure the resolved address can be used correctly.</span></span> <span data-ttu-id="c7e69-161">Váš klient potřebuje zjistit, zda pokus o připojení se nezdařilo z důvodu přechodná chyba a můžete zkusit znovu (například služba přesunut nebo je dočasně nedostupná), nebo trvalé chybě (např. Služba je Odstraněná nebo požadovaný prostředek již existuje).</span><span class="sxs-lookup"><span data-stu-id="c7e69-161">Your client needs to detect whether the connection attempt failed because of a transient error and can be retried (e.g., service moved or is temporarily unavailable), or a permanent error (e.g., service was deleted or the requested resource no longer exists).</span></span> <span data-ttu-id="c7e69-162">Instance služby nebo repliky můžete pohyb z uzlu do uzlu kdykoli z několika důvodů.</span><span class="sxs-lookup"><span data-stu-id="c7e69-162">Service instances or replicas can move around from node to node at any time for multiple reasons.</span></span> <span data-ttu-id="c7e69-163">Adresa služby přeložit prostřednictvím ServicePartitionResolver může být zastaralé dobu, kterou váš klientský kód pokusí o připojení.</span><span class="sxs-lookup"><span data-stu-id="c7e69-163">The service address resolved through ServicePartitionResolver may be stale by the time your client code attempts to connect.</span></span> <span data-ttu-id="c7e69-164">V takovém případě znovu klient musí znovu překlad adresy.</span><span class="sxs-lookup"><span data-stu-id="c7e69-164">In that case again the client needs to re-resolve the address.</span></span> <span data-ttu-id="c7e69-165">Poskytnutí předchozí `ResolvedServicePartition` ukazuje, že překladač vyžaduje pokus místo jednoduše načíst adresu v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="c7e69-165">Providing the previous `ResolvedServicePartition` indicates that the resolver needs to try again rather than simply retrieve a cached address.</span></span>

<span data-ttu-id="c7e69-166">Obvykle kód klienta nemusí pracovat ServicePartitionResolver přímo.</span><span class="sxs-lookup"><span data-stu-id="c7e69-166">Typically, the client code need not work with the ServicePartitionResolver directly.</span></span> <span data-ttu-id="c7e69-167">Je vytvořen a k předání komunikace klienta továrny v spolehlivé rozhraní API služby.</span><span class="sxs-lookup"><span data-stu-id="c7e69-167">It is created and passed on to communication client factories in the Reliable Services API.</span></span> <span data-ttu-id="c7e69-168">Překladač objekty Factory interně použít ke generování klientského objektu, který umožňuje komunikovat se službami.</span><span class="sxs-lookup"><span data-stu-id="c7e69-168">The factories use the resolver internally to generate a client object that can be used to communicate with services.</span></span>

### <a name="communication-clients-and-factories"></a><span data-ttu-id="c7e69-169">Komunikace klientů a objekty pro vytváření</span><span class="sxs-lookup"><span data-stu-id="c7e69-169">Communication clients and factories</span></span>
<span data-ttu-id="c7e69-170">Knihovna vytváření komunikace implementuje typický vzor opakování selhání zpracování, který usnadňuje Probíhá opakování připojení ke koncovým bodům služby vyřešený.</span><span class="sxs-lookup"><span data-stu-id="c7e69-170">The communication factory library implements a typical fault-handling retry pattern that makes retrying connections to resolved service endpoints easier.</span></span> <span data-ttu-id="c7e69-171">Objekt pro vytváření knihovny poskytuje mechanismus opakování, zatímco poskytují chyba obslužné rutiny.</span><span class="sxs-lookup"><span data-stu-id="c7e69-171">The factory library provides the retry mechanism while you provide the error handlers.</span></span>

<span data-ttu-id="c7e69-172">`ICommunicationClientFactory(C#) / CommunicationClientFactory(Java)`definuje základní rozhraní implementované objekt factory komunikaci klienta, který vytváří klientů, které může kontaktovat službě Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="c7e69-172">`ICommunicationClientFactory(C#) / CommunicationClientFactory(Java)` defines the base interface implemented by a communication client factory that produces clients that can talk to a Service Fabric service.</span></span> <span data-ttu-id="c7e69-173">Implementace CommunicationClientFactory závisí na komunikačního balíku používá služba Service Fabric, kde se chce, aby klient komunikovat.</span><span class="sxs-lookup"><span data-stu-id="c7e69-173">The implementation of the CommunicationClientFactory depends on the communication stack used by the Service Fabric service where the client wants to communicate.</span></span> <span data-ttu-id="c7e69-174">Poskytuje spolehlivé rozhraní API služby `CommunicationClientFactoryBase<TCommunicationClient>`.</span><span class="sxs-lookup"><span data-stu-id="c7e69-174">The Reliable Services API provides a `CommunicationClientFactoryBase<TCommunicationClient>`.</span></span> <span data-ttu-id="c7e69-175">To poskytuje základní implementaci rozhraní CommunicationClientFactory a provádí úlohy, které jsou společné pro všechny balíčky komunikace.</span><span class="sxs-lookup"><span data-stu-id="c7e69-175">This provides a base implementation of the CommunicationClientFactory interface and performs tasks that are common to all the communication stacks.</span></span> <span data-ttu-id="c7e69-176">(Tyto úlohy patří, použití ServicePartitionResolver k určení, koncový bod služby).</span><span class="sxs-lookup"><span data-stu-id="c7e69-176">(These tasks include using a ServicePartitionResolver to determine the service endpoint).</span></span> <span data-ttu-id="c7e69-177">Klienti obvykle implementovat abstraktní třídy CommunicationClientFactoryBase pro zpracování logiky, která je specifická pro komunikačního balíku.</span><span class="sxs-lookup"><span data-stu-id="c7e69-177">Clients usually implement the abstract CommunicationClientFactoryBase class to handle logic that is specific to the communication stack.</span></span>

<span data-ttu-id="c7e69-178">Komunikace klienta pouze přijímá adresu a použije k připojení ke službě.</span><span class="sxs-lookup"><span data-stu-id="c7e69-178">The communication client just receives an address and uses it to connect to a service.</span></span> <span data-ttu-id="c7e69-179">Klienta můžete použít libovolnou protokol ho chce.</span><span class="sxs-lookup"><span data-stu-id="c7e69-179">The client can use whatever protocol it wants.</span></span>

```csharp
class MyCommunicationClient : ICommunicationClient
{
    public ResolvedServiceEndpoint Endpoint { get; set; }

    public string ListenerName { get; set; }

    public ResolvedServicePartition ResolvedServicePartition { get; set; }
}
```
```java
public class MyCommunicationClient implements CommunicationClient {

    private ResolvedServicePartition resolvedServicePartition;
    private String listenerName;
    private ResolvedServiceEndpoint endPoint;

    /*
     * Getters and Setters
     */
}
```

<span data-ttu-id="c7e69-180">Dodavatel klienta především zodpovídá za vytvoření komunikace klientů.</span><span class="sxs-lookup"><span data-stu-id="c7e69-180">The client factory is primarily responsible for creating communication clients.</span></span> <span data-ttu-id="c7e69-181">Pro klienty, kteří nejsou udržovat trvalé připojení, jako je například klientovi HTTP musí objekt pro vytváření pouze k vytvoření a vrátí klientovi.</span><span class="sxs-lookup"><span data-stu-id="c7e69-181">For clients that don't maintain a persistent connection, such as an HTTP client, the factory only needs to create and return the client.</span></span> <span data-ttu-id="c7e69-182">Jiné protokoly, které udržují trvalé připojení, jako je například některé binární protokoly by měl být také ověřené pomocí objektu pro vytváření k určení, zda připojení musí být znovu vytvořena.</span><span class="sxs-lookup"><span data-stu-id="c7e69-182">Other protocols that maintain a persistent connection, such as some binary protocols, should also be validated by the factory to determine whether the connection needs to be re-created.</span></span>  

```csharp
public class MyCommunicationClientFactory : CommunicationClientFactoryBase<MyCommunicationClient>
{
    protected override void AbortClient(MyCommunicationClient client)
    {
    }

    protected override Task<MyCommunicationClient> CreateClientAsync(string endpoint, CancellationToken cancellationToken)
    {
    }

    protected override bool ValidateClient(MyCommunicationClient clientChannel)
    {
    }

    protected override bool ValidateClient(string endpoint, MyCommunicationClient client)
    {
    }
}
```
```java
public class MyCommunicationClientFactory extends CommunicationClientFactoryBase<MyCommunicationClient> {

    @Override
    protected boolean validateClient(MyCommunicationClient clientChannel) {
    }

    @Override
    protected boolean validateClient(String endpoint, MyCommunicationClient client) {
    }

    @Override
    protected CompletableFuture<MyCommunicationClient> createClientAsync(String endpoint) {
    }

    @Override
    protected void abortClient(MyCommunicationClient client) {
    }
}
```

<span data-ttu-id="c7e69-183">Nakonec obslužné rutiny výjimky je zodpovědná za určení, jaká opatření se mají provést, když dojde k výjimce.</span><span class="sxs-lookup"><span data-stu-id="c7e69-183">Finally, an exception handler is responsible for determining what action to take when an exception occurs.</span></span> <span data-ttu-id="c7e69-184">Výjimky jsou rozdělené do **opakovatelné** a **neopakovatelného**.</span><span class="sxs-lookup"><span data-stu-id="c7e69-184">Exceptions are categorized into **retryable** and **non retryable**.</span></span>

* <span data-ttu-id="c7e69-185">**Neopakovatelného** výjimky jednoduše získat znovu vyvolány zpět k volajícímu.</span><span class="sxs-lookup"><span data-stu-id="c7e69-185">**Non retryable** exceptions simply get rethrown back to the caller.</span></span>
* <span data-ttu-id="c7e69-186">**Opakovatelná** výjimky se dále dělí do **přechodný** a **-pouze dočasné**.</span><span class="sxs-lookup"><span data-stu-id="c7e69-186">**retryable** exceptions are further categorized into **transient** and **non-transient**.</span></span>
  * <span data-ttu-id="c7e69-187">**Přechodný** výjimky jsou ty, které můžete jednoduše opakovat bez znovu řešení adresa koncového bodu služby.</span><span class="sxs-lookup"><span data-stu-id="c7e69-187">**Transient** exceptions are those that can simply be retried without re-resolving the service endpoint address.</span></span> <span data-ttu-id="c7e69-188">To bude obsahovat přechodné problémy se sítí nebo služby chybové odpovědi kromě těch, které indikuje, že adresa koncového bodu služby neexistuje.</span><span class="sxs-lookup"><span data-stu-id="c7e69-188">These will include transient network problems or service error responses other than those that indicate the service endpoint address does not exist.</span></span>
  * <span data-ttu-id="c7e69-189">**Bez přechodná** výjimky jsou ty, které vyžadují adresa koncového bodu služby do znovu přeložit.</span><span class="sxs-lookup"><span data-stu-id="c7e69-189">**Non-transient** exceptions are those that require the service endpoint address to be re-resolved.</span></span> <span data-ttu-id="c7e69-190">Mezi ně patří výjimky, které označují, že není dostupný koncový bod služby, označující služby se přesunul do jiného uzlu.</span><span class="sxs-lookup"><span data-stu-id="c7e69-190">These include exceptions that indicate the service endpoint could not be reached, indicating the service has moved to a different node.</span></span>

<span data-ttu-id="c7e69-191">`TryHandleException` Provádí rozhodnutí o dané výjimka.</span><span class="sxs-lookup"><span data-stu-id="c7e69-191">The `TryHandleException` makes a decision about a given exception.</span></span> <span data-ttu-id="c7e69-192">Pokud ho **neví** jaké rozhodnutí, která chcete-li o výjimku, měla by vrátit **false**.</span><span class="sxs-lookup"><span data-stu-id="c7e69-192">If it **does not know** what decisions to make about an exception, it should return **false**.</span></span> <span data-ttu-id="c7e69-193">Pokud ho **vědět** jaké rozhodnutí, měl by odpovídajícím způsobem nastavit výsledek a vrátí **true**.</span><span class="sxs-lookup"><span data-stu-id="c7e69-193">If it **does know** what decision to make, it should set the result accordingly and return **true**.</span></span>

```csharp
class MyExceptionHandler : IExceptionHandler
{
    public bool TryHandleException(ExceptionInformation exceptionInformation, OperationRetrySettings retrySettings, out ExceptionHandlingResult result)
    {
        // if exceptionInformation.Exception is known and is transient (can be retried without re-resolving)
        result = new ExceptionHandlingRetryResult(exceptionInformation.Exception, true, retrySettings, retrySettings.DefaultMaxRetryCount);
        return true;


        // if exceptionInformation.Exception is known and is not transient (indicates a new service endpoint address must be resolved)
        result = new ExceptionHandlingRetryResult(exceptionInformation.Exception, false, retrySettings, retrySettings.DefaultMaxRetryCount);
        return true;

        // if exceptionInformation.Exception is unknown (let the next IExceptionHandler attempt to handle it)
        result = null;
        return false;
    }
}
```
```java
public class MyExceptionHandler implements ExceptionHandler {

    @Override
    public ExceptionHandlingResult handleException(ExceptionInformation exceptionInformation, OperationRetrySettings retrySettings) {        

        /* if exceptionInformation.getException() is known and is transient (can be retried without re-resolving)
         */
        result = new ExceptionHandlingRetryResult(exceptionInformation.getException(), true, retrySettings, retrySettings.getDefaultMaxRetryCount());
        return true;


        /* if exceptionInformation.getException() is known and is not transient (indicates a new service endpoint address must be resolved)
         */
        result = new ExceptionHandlingRetryResult(exceptionInformation.getException(), false, retrySettings, retrySettings.getDefaultMaxRetryCount());
        return true;

        /* if exceptionInformation.getException() is unknown (let the next ExceptionHandler attempt to handle it)
         */
        result = null;
        return false;

    }
}
```
### <a name="putting-it-all-together"></a><span data-ttu-id="c7e69-194">Třeba umisťovat všechny společně</span><span class="sxs-lookup"><span data-stu-id="c7e69-194">Putting it all together</span></span>
<span data-ttu-id="c7e69-195">S `ICommunicationClient(C#) / CommunicationClient(Java)`, `ICommunicationClientFactory(C#) / CommunicationClientFactory(Java)`, a `IExceptionHandler(C#) / ExceptionHandler(Java)` vytvořených na základě komunikační protokol, `ServicePartitionClient(C#) / FabricServicePartitionClient(Java)` všechny společně se zabalí a poskytuje řešení smyčky adresu oddílu zpracování chyb a služeb kolem těchto součástí.</span><span class="sxs-lookup"><span data-stu-id="c7e69-195">With an `ICommunicationClient(C#) / CommunicationClient(Java)`, `ICommunicationClientFactory(C#) / CommunicationClientFactory(Java)`, and `IExceptionHandler(C#) / ExceptionHandler(Java)` built around a communication protocol, a `ServicePartitionClient(C#) / FabricServicePartitionClient(Java)` wraps it all together and provides the fault-handling and service partition address resolution loop around these components.</span></span>

```csharp
private MyCommunicationClientFactory myCommunicationClientFactory;
private Uri myServiceUri;

var myServicePartitionClient = new ServicePartitionClient<MyCommunicationClient>(
    this.myCommunicationClientFactory,
    this.myServiceUri,
    myPartitionKey);

var result = await myServicePartitionClient.InvokeWithRetryAsync(async (client) =>
   {
      // Communicate with the service using the client.
   },
   CancellationToken.None);

```
```java
private MyCommunicationClientFactory myCommunicationClientFactory;
private URI myServiceUri;

FabricServicePartitionClient myServicePartitionClient = new FabricServicePartitionClient<MyCommunicationClient>(
    this.myCommunicationClientFactory,
    this.myServiceUri,
    myPartitionKey);

CompletableFuture<?> result = myServicePartitionClient.invokeWithRetryAsync(client -> {
      /* Communicate with the service using the client.
       */
   });

```

## <a name="next-steps"></a><span data-ttu-id="c7e69-196">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c7e69-196">Next steps</span></span>
* <span data-ttu-id="c7e69-197">Zobrazit příklad komunikaci pomocí protokolu HTTP mezi službami v [ukázkový projekt C# na Githubu](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Services/WordCount) nebo [Java ukázkový projekt na Githubu](https://github.com/Azure-Samples/service-fabric-java-getting-started/tree/master/Services/WatchDog).</span><span class="sxs-lookup"><span data-stu-id="c7e69-197">See an example of HTTP communication between services in a [C# sample project on GitHUb](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Services/WordCount) or [Java sample project on GitHUb](https://github.com/Azure-Samples/service-fabric-java-getting-started/tree/master/Services/WatchDog).</span></span>
* [<span data-ttu-id="c7e69-198">Volání vzdálených procedur s vzdálenou komunikaci spolehlivé služby</span><span class="sxs-lookup"><span data-stu-id="c7e69-198">Remote procedure calls with Reliable Services remoting</span></span>](service-fabric-reliable-services-communication-remoting.md)
* [<span data-ttu-id="c7e69-199">Webové rozhraní API, která používá OWIN v spolehlivé služby</span><span class="sxs-lookup"><span data-stu-id="c7e69-199">Web API that uses OWIN in Reliable Services</span></span>](service-fabric-reliable-services-communication-webapi.md)
* [<span data-ttu-id="c7e69-200">Komunikace WCF pomocí spolehlivé služby</span><span class="sxs-lookup"><span data-stu-id="c7e69-200">WCF communication by using Reliable Services</span></span>](service-fabric-reliable-services-communication-wcf.md)
