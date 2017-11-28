---
title: "Přehled komunikace služby aaaReliable | Microsoft Docs"
description: "Přehled hello spolehlivé služby komunikace modelu, včetně otevírání moduly pro naslouchání na službách, vyřešte koncových bodů a komunikaci mezi službami."
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
ms.openlocfilehash: 93a7017b50df0822969daa5ad78302c73e8ba641
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-reliable-services-communication-apis"></a><span data-ttu-id="79012-103">Jak toouse hello spolehlivé služby komunikace rozhraní API</span><span class="sxs-lookup"><span data-stu-id="79012-103">How toouse hello Reliable Services communication APIs</span></span>
<span data-ttu-id="79012-104">Azure Service Fabric jako platformu je zcela lhostejné o komunikaci mezi službami.</span><span class="sxs-lookup"><span data-stu-id="79012-104">Azure Service Fabric as a platform is completely agnostic about communication between services.</span></span> <span data-ttu-id="79012-105">Všechny protokoly a zásobníky jsou přijatelné, z UDP tooHTTP.</span><span class="sxs-lookup"><span data-stu-id="79012-105">All protocols and stacks are acceptable, from UDP tooHTTP.</span></span> <span data-ttu-id="79012-106">Je to toohello služby vývojáře toochoose komunikace služby.</span><span class="sxs-lookup"><span data-stu-id="79012-106">It's up toohello service developer toochoose how services should communicate.</span></span> <span data-ttu-id="79012-107">Architektura aplikace na Hello spolehlivé služby poskytuje integrované komunikace balíků i rozhraní API, které můžete použít toobuild vlastní komunikační součásti.</span><span class="sxs-lookup"><span data-stu-id="79012-107">hello Reliable Services application framework provides built-in communication stacks as well as APIs that you can use toobuild your custom communication components.</span></span>

## <a name="set-up-service-communication"></a><span data-ttu-id="79012-108">Nastavení komunikace služby</span><span class="sxs-lookup"><span data-stu-id="79012-108">Set up service communication</span></span>
<span data-ttu-id="79012-109">Hello spolehlivé rozhraní API služby používá jednoduché rozhraní pro komunikace služby.</span><span class="sxs-lookup"><span data-stu-id="79012-109">hello Reliable Services API uses a simple interface for service communication.</span></span> <span data-ttu-id="79012-110">Toto rozhraní implementovat jednoduše tooopen koncový bod pro vaši službu:</span><span class="sxs-lookup"><span data-stu-id="79012-110">tooopen an endpoint for your service, simply implement this interface:</span></span>

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

<span data-ttu-id="79012-111">Poté můžete přidat implementaci naslouchací proces komunikace vrácením v přepsání metody založené na službě třídy.</span><span class="sxs-lookup"><span data-stu-id="79012-111">You can then add your communication listener implementation by returning it in a service-based class method override.</span></span>

<span data-ttu-id="79012-112">Pro bezstavové služby:</span><span class="sxs-lookup"><span data-stu-id="79012-112">For stateless services:</span></span>

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

<span data-ttu-id="79012-113">Pro stavové služby:</span><span class="sxs-lookup"><span data-stu-id="79012-113">For stateful services:</span></span>

> [!NOTE]
> <span data-ttu-id="79012-114">Stavová spolehlivé služby ještě nepodporuje v jazyce Java.</span><span class="sxs-lookup"><span data-stu-id="79012-114">Stateful reliable services are not supported in Java yet.</span></span>
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

<span data-ttu-id="79012-115">V obou případech vracet kolekci naslouchacího procesu.</span><span class="sxs-lookup"><span data-stu-id="79012-115">In both cases, you return a collection of listeners.</span></span> <span data-ttu-id="79012-116">To umožňuje vaší služby toolisten na několik koncových bodů, potenciálně pomocí různých protokolů, s použitím více naslouchací procesy.</span><span class="sxs-lookup"><span data-stu-id="79012-116">This allows your service toolisten on multiple endpoints, potentially using different protocols, by using multiple listeners.</span></span> <span data-ttu-id="79012-117">Například můžete mít naslouchací proces protokolu HTTP a samostatné naslouchací proces protokolu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="79012-117">For example, you may have an HTTP listener and a separate WebSocket listener.</span></span> <span data-ttu-id="79012-118">Každý naslouchací proces získá název a kolekci výsledné hello *název: adresa* páry jsou reprezentovány jako objekt JSON, když klient požádá o hello naslouchání adresy pro instance služby nebo oddíl.</span><span class="sxs-lookup"><span data-stu-id="79012-118">Each listener gets a name, and hello resulting collection of *name : address* pairs are represented as a JSON object when a client requests hello listening addresses for a service instance or a partition.</span></span>

<span data-ttu-id="79012-119">V bezstavové služby hello přepsání vrátí kolekci ServiceInstanceListeners.</span><span class="sxs-lookup"><span data-stu-id="79012-119">In a stateless service, hello override returns a collection of ServiceInstanceListeners.</span></span> <span data-ttu-id="79012-120">A `ServiceInstanceListener` obsahuje funkce toocreate `ICommunicationListener(C#) / CommunicationListener(Java)` a název.</span><span class="sxs-lookup"><span data-stu-id="79012-120">A `ServiceInstanceListener` contains a function toocreate an `ICommunicationListener(C#) / CommunicationListener(Java)` and gives it a name.</span></span> <span data-ttu-id="79012-121">Hello přepsání pro stavové služby, vrátí kolekci ServiceReplicaListeners.</span><span class="sxs-lookup"><span data-stu-id="79012-121">For stateful services, hello override returns a collection of ServiceReplicaListeners.</span></span> <span data-ttu-id="79012-122">To se mírně liší od jeho protějšku bezstavové, protože `ServiceReplicaListener` má tooopen možnost `ICommunicationListener` na sekundárních replikách.</span><span class="sxs-lookup"><span data-stu-id="79012-122">This is slightly different from its stateless counterpart, because a `ServiceReplicaListener` has an option tooopen an `ICommunicationListener` on secondary replicas.</span></span> <span data-ttu-id="79012-123">Nejenže můžete použít více naslouchací procesy komunikace ve službě, ale můžete také určit, který naslouchací procesy přijímat požadavky na sekundárních replikách a ty, které naslouchat pouze primární repliky.</span><span class="sxs-lookup"><span data-stu-id="79012-123">Not only can you use multiple communication listeners in a service, but you can also specify which listeners accept requests on secondary replicas and which ones listen only on primary replicas.</span></span>

<span data-ttu-id="79012-124">Například můžete mít ServiceRemotingListener, která přebírá volání RPC jenom na primární repliky a druhý, vlastní naslouchací proces, který přebírá čtení požadavky na sekundárních replikách prostřednictvím protokolu HTTP:</span><span class="sxs-lookup"><span data-stu-id="79012-124">For example, you can have a ServiceRemotingListener that takes RPC calls only on primary replicas, and a second, custom listener that takes read requests on secondary replicas over HTTP:</span></span>

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
> <span data-ttu-id="79012-125">Při vytváření více naslouchací procesy pro službu, každý naslouchací proces **musí** mít jedinečný název.</span><span class="sxs-lookup"><span data-stu-id="79012-125">When creating multiple listeners for a service, each listener **must** be given a unique name.</span></span>
>
>

<span data-ttu-id="79012-126">Nakonec popisují hello koncové body, které jsou požadovány pro službu hello v hello [service manifest](service-fabric-application-model.md) části hello u koncových bodů.</span><span class="sxs-lookup"><span data-stu-id="79012-126">Finally, describe hello endpoints that are required for hello service in hello [service manifest](service-fabric-application-model.md) under hello section on endpoints.</span></span>

```xml
<Resources>
    <Endpoints>
      <Endpoint Name="WebServiceEndpoint" Protocol="http" Port="80" />
      <Endpoint Name="OtherServiceEndpoint" Protocol="tcp" Port="8505" />
    <Endpoints>
</Resources>

```

<span data-ttu-id="79012-127">naslouchací proces komunikace Hello mají přístup k prostředkům koncový bod hello přidělené tooit z hello `CodePackageActivationContext` v hello `ServiceContext`.</span><span class="sxs-lookup"><span data-stu-id="79012-127">hello communication listener can access hello endpoint resources allocated tooit from hello `CodePackageActivationContext` in hello `ServiceContext`.</span></span> <span data-ttu-id="79012-128">naslouchací proces Hello potom lze spustit naslouchaly žádostem, když je otevřen.</span><span class="sxs-lookup"><span data-stu-id="79012-128">hello listener can then start listening for requests when it is opened.</span></span>

```csharp
var codePackageActivationContext = serviceContext.CodePackageActivationContext;
var port = codePackageActivationContext.GetEndpoint("ServiceEndpoint").Port;

```
```java
CodePackageActivationContext codePackageActivationContext = serviceContext.getCodePackageActivationContext();
int port = codePackageActivationContext.getEndpoint("ServiceEndpoint").getPort();

```

> [!NOTE]
> <span data-ttu-id="79012-129">Koncový bod prostředky jsou běžné balíček toohello celou službou, a jsou přiděleny pomocí Service Fabric, když je aktivován balíček služby hello.</span><span class="sxs-lookup"><span data-stu-id="79012-129">Endpoint resources are common toohello entire service package, and they are allocated by Service Fabric when hello service package is activated.</span></span> <span data-ttu-id="79012-130">Víc replik služby hostované v hello může sdílet stejnou ServiceHost hello stejný port.</span><span class="sxs-lookup"><span data-stu-id="79012-130">Multiple service replicas hosted in hello same ServiceHost may share hello same port.</span></span> <span data-ttu-id="79012-131">To znamená, že naslouchací proces hello komunikace by měly podporovat sdílení portů.</span><span class="sxs-lookup"><span data-stu-id="79012-131">This means that hello communication listener should support port sharing.</span></span> <span data-ttu-id="79012-132">Hello doporučuje se způsob, jak to provést pro hello komunikaci naslouchacího procesu toouse hello oddíl ID a ID repliky nebo instanci, pokud vygeneruje adresu naslouchání hello.</span><span class="sxs-lookup"><span data-stu-id="79012-132">hello recommended way of doing this is for hello communication listener toouse hello partition ID and replica/instance ID when it generates hello listen address.</span></span>
>
>

### <a name="service-address-registration"></a><span data-ttu-id="79012-133">Registrace adresy služby</span><span class="sxs-lookup"><span data-stu-id="79012-133">Service address registration</span></span>
<span data-ttu-id="79012-134">Služba system názvem hello *služby DNS* běží na clusterů Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="79012-134">A system service called hello *Naming Service* runs on Service Fabric clusters.</span></span> <span data-ttu-id="79012-135">Hello služby DNS je doménového registrátora služeb a jejich adresy, které naslouchá na každou instanci nebo repliky služby hello.</span><span class="sxs-lookup"><span data-stu-id="79012-135">hello Naming Service is a registrar for services and their addresses that each instance or replica of hello service is listening on.</span></span> <span data-ttu-id="79012-136">Když hello `OpenAsync(C#) / openAsync(Java)` metodu `ICommunicationListener(C#) / CommunicationListener(Java)` dokončí, vraťte se jeho hodnota získá registrované v hello Naming Service.</span><span class="sxs-lookup"><span data-stu-id="79012-136">When hello `OpenAsync(C#) / openAsync(Java)` method of an `ICommunicationListener(C#) / CommunicationListener(Java)` completes, its return value gets registered in hello Naming Service.</span></span> <span data-ttu-id="79012-137">Tato vrátí hodnotu, která se získá publikované v hello služby DNS je řetězec, jehož hodnota může být jakýkoli vůbec.</span><span class="sxs-lookup"><span data-stu-id="79012-137">This return value that gets published in hello Naming Service is a string whose value can be anything at all.</span></span> <span data-ttu-id="79012-138">Tato hodnota řetězce je, co klienti uvidí, když vyzvou pro adresu hello službu hello Naming Service.</span><span class="sxs-lookup"><span data-stu-id="79012-138">This string value is what clients see when they ask for an address for hello service from hello Naming Service.</span></span>

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

    // hello string returned here will be published in hello Naming Service.
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

    /* hello string returned here will be published in hello Naming Service.
     */
    return CompletableFuture.completedFuture(this.publishAddress);
}
```

<span data-ttu-id="79012-139">Service Fabric poskytuje rozhraní API, které umožňují klienty a další služby toothen požádejte pro tuto adresu podle názvu služby.</span><span class="sxs-lookup"><span data-stu-id="79012-139">Service Fabric provides APIs that allow clients and other services toothen ask for this address by service name.</span></span> <span data-ttu-id="79012-140">To je důležité, protože není statickou adresu služby hello.</span><span class="sxs-lookup"><span data-stu-id="79012-140">This is important because hello service address is not static.</span></span> <span data-ttu-id="79012-141">Služby přesouvání hello clusteru za účelem vyrovnávání a dostupnosti prostředků.</span><span class="sxs-lookup"><span data-stu-id="79012-141">Services are moved around in hello cluster for resource balancing and availability purposes.</span></span> <span data-ttu-id="79012-142">Toto je hello mechanismus, který klientům tooresolve hello naslouchání adresu pro službu.</span><span class="sxs-lookup"><span data-stu-id="79012-142">This is hello mechanism that allow clients tooresolve hello listening address for a service.</span></span>

> [!NOTE]
> <span data-ttu-id="79012-143">Pro kompletní návod jak toowrite naslouchací proces komunikace, najdete v části [Service Fabric webového rozhraní API služby s vlastním hostování OWIN](service-fabric-reliable-services-communication-webapi.md) C#, zatímco pro jazyk Java můžete napsat vlastní implementaci serveru HTTP, najdete v části EchoServer aplikace Příklad v https://github.com/Azure-Samples/service-fabric-java-getting-started.</span><span class="sxs-lookup"><span data-stu-id="79012-143">For a complete walk-through of how toowrite a communication listener, see [Service Fabric Web API services with OWIN self-hosting](service-fabric-reliable-services-communication-webapi.md) for C#, whereas for Java you can write your own HTTP server implementation, see EchoServer application example at https://github.com/Azure-Samples/service-fabric-java-getting-started.</span></span>
>
>

## <a name="communicating-with-a-service"></a><span data-ttu-id="79012-144">Komunikace se službou</span><span class="sxs-lookup"><span data-stu-id="79012-144">Communicating with a service</span></span>
<span data-ttu-id="79012-145">Hello spolehlivé rozhraní API služby poskytuje hello následující klienti toowrite knihovny, které komunikovat se službami.</span><span class="sxs-lookup"><span data-stu-id="79012-145">hello Reliable Services API provides hello following libraries toowrite clients that communicate with services.</span></span>

### <a name="service-endpoint-resolution"></a><span data-ttu-id="79012-146">Rozlišení koncového bodu služby</span><span class="sxs-lookup"><span data-stu-id="79012-146">Service endpoint resolution</span></span>
<span data-ttu-id="79012-147">Hello první krok toocommunication službou je tooresolve adresu koncového bodu hello oddílu nebo instance, které chcete tootalk do služby hello.</span><span class="sxs-lookup"><span data-stu-id="79012-147">hello first step toocommunication with a service is tooresolve an endpoint address of hello partition or instance of hello service you want tootalk to.</span></span> <span data-ttu-id="79012-148">Hello `ServicePartitionResolver(C#) / FabricServicePartitionResolver(Java)` nástroj třída je základní Primitivum, které pomáhají klientům určit hello koncový bod služby za běhu.</span><span class="sxs-lookup"><span data-stu-id="79012-148">hello `ServicePartitionResolver(C#) / FabricServicePartitionResolver(Java)` utility class is a basic primitive that helps clients determine hello endpoint of a service at runtime.</span></span> <span data-ttu-id="79012-149">V terminologii Service Fabric hello proces určení hello koncový bod služby se označují tooas hello *rozlišení koncového bodu služby*.</span><span class="sxs-lookup"><span data-stu-id="79012-149">In Service Fabric terminology, hello process of determining hello endpoint of a service is referred tooas hello *service endpoint resolution*.</span></span>

<span data-ttu-id="79012-150">tooconnect tooservices v rámci clusteru, můžete vytvořit ServicePartitionResolver, pomocí výchozího nastavení.</span><span class="sxs-lookup"><span data-stu-id="79012-150">tooconnect tooservices within a cluster, ServicePartitionResolver can be created using default settings.</span></span> <span data-ttu-id="79012-151">Toto je doporučená využití většině situací hello:</span><span class="sxs-lookup"><span data-stu-id="79012-151">This is hello recommended usage for most situations:</span></span>

```csharp
ServicePartitionResolver resolver = ServicePartitionResolver.GetDefault();
```
```java
FabricServicePartitionResolver resolver = FabricServicePartitionResolver.getDefault();
```

<span data-ttu-id="79012-152">tooconnect tooservices v jiném clusteru, ServicePartitionResolver lze vytvořit sadu koncovým bodům clusteru brány.</span><span class="sxs-lookup"><span data-stu-id="79012-152">tooconnect tooservices in a different cluster, a ServicePartitionResolver can be created with a set of cluster gateway endpoints.</span></span> <span data-ttu-id="79012-153">Upozorňujeme, že jsou koncové body brány právě různými koncovými body pro připojení toohello stejného clusteru.</span><span class="sxs-lookup"><span data-stu-id="79012-153">Note that gateway endpoints are just different endpoints for connecting toohello same cluster.</span></span> <span data-ttu-id="79012-154">Například:</span><span class="sxs-lookup"><span data-stu-id="79012-154">For example:</span></span>

```csharp
ServicePartitionResolver resolver = new  ServicePartitionResolver("mycluster.cloudapp.azure.com:19000", "mycluster.cloudapp.azure.com:19001");
```
```java
FabricServicePartitionResolver resolver = new  FabricServicePartitionResolver("mycluster.cloudapp.azure.com:19000", "mycluster.cloudapp.azure.com:19001");
```

<span data-ttu-id="79012-155">Alternativně `ServicePartitionResolver` možné přidělit funkce pro vytváření `FabricClient` toouse interně:</span><span class="sxs-lookup"><span data-stu-id="79012-155">Alternatively, `ServicePartitionResolver` can be given a function for creating a `FabricClient` toouse internally:</span></span>

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

<span data-ttu-id="79012-156">`FabricClient`je hello objekt, který je použité toocommunicate se cluster Service Fabric hello pro různé operace správy v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="79012-156">`FabricClient` is hello object that is used toocommunicate with hello Service Fabric cluster for various management operations on hello cluster.</span></span> <span data-ttu-id="79012-157">To je užitečné, pokud chcete mít větší kontrolu nad interakci překladač oddílu služby se váš cluster.</span><span class="sxs-lookup"><span data-stu-id="79012-157">This is useful when you want more control over how a service partition resolver interacts with your cluster.</span></span> <span data-ttu-id="79012-158">`FabricClient`provede ukládání do mezipaměti interně a je obvykle levnější toocreate, takže je důležité tooreuse `FabricClient` instance co nejvíc.</span><span class="sxs-lookup"><span data-stu-id="79012-158">`FabricClient` performs caching internally and is generally expensive toocreate, so it is important tooreuse `FabricClient` instances as much as possible.</span></span>

```csharp
ServicePartitionResolver resolver = new  ServicePartitionResolver(() => CreateMyFabricClient());
```
```java
FabricServicePartitionResolver resolver = new  FabricServicePartitionResolver(() -> new CreateFabricClientImpl());
```

<span data-ttu-id="79012-159">Metoda vyřešte je pak použít tooretrieve hello adresu služby nebo služby u oddílů služby.</span><span class="sxs-lookup"><span data-stu-id="79012-159">A resolve method is then used tooretrieve hello address of a service or a service partition for partitioned services.</span></span>

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

<span data-ttu-id="79012-160">Adresa služby lze vyřešit pomocí snadno ServicePartitionResolver, ale je nutná další práci tooensure hello přeložit adresu je možné použít správně.</span><span class="sxs-lookup"><span data-stu-id="79012-160">A service address can be resolved easily using a ServicePartitionResolver, but more work is required tooensure hello resolved address can be used correctly.</span></span> <span data-ttu-id="79012-161">Váš klient musí toodetect, zda se nezdařilo z důvodu přechodná chyba umožňující opakovaný pokus o připojení hello a můžete zkusit znovu (například služba přesunut nebo je dočasně nedostupná), nebo trvalé chybě (např. Služba je Odstraněná nebo hello požadovaný prostředek již existuje).</span><span class="sxs-lookup"><span data-stu-id="79012-161">Your client needs toodetect whether hello connection attempt failed because of a transient error and can be retried (e.g., service moved or is temporarily unavailable), or a permanent error (e.g., service was deleted or hello requested resource no longer exists).</span></span> <span data-ttu-id="79012-162">Instance služby nebo repliky můžete přesunout z uzlu toonode kdykoli z několika důvodů.</span><span class="sxs-lookup"><span data-stu-id="79012-162">Service instances or replicas can move around from node toonode at any time for multiple reasons.</span></span> <span data-ttu-id="79012-163">Adresa služby Hello přeložit prostřednictvím ServicePartitionResolver může být váš klientský kód pokusí tooconnect doby hello zastaralé.</span><span class="sxs-lookup"><span data-stu-id="79012-163">hello service address resolved through ServicePartitionResolver may be stale by hello time your client code attempts tooconnect.</span></span> <span data-ttu-id="79012-164">V takovém případě znovu hello klient potřebuje vyřešit toore hello adresu.</span><span class="sxs-lookup"><span data-stu-id="79012-164">In that case again hello client needs toore-resolve hello address.</span></span> <span data-ttu-id="79012-165">Poskytnutí hello předchozí `ResolvedServicePartition` označuje, která znovu hello překladač potřebám tootry nikoli jednoduše načíst adresu v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="79012-165">Providing hello previous `ResolvedServicePartition` indicates that hello resolver needs tootry again rather than simply retrieve a cached address.</span></span>

<span data-ttu-id="79012-166">Obvykle kód klienta hello nemusí pracovat s hello ServicePartitionResolver přímo.</span><span class="sxs-lookup"><span data-stu-id="79012-166">Typically, hello client code need not work with hello ServicePartitionResolver directly.</span></span> <span data-ttu-id="79012-167">Je vytvořen a předán na objekty Factory klienta toocommunication v hello spolehlivé Services API.</span><span class="sxs-lookup"><span data-stu-id="79012-167">It is created and passed on toocommunication client factories in hello Reliable Services API.</span></span> <span data-ttu-id="79012-168">objekty Factory Hello používají hello překladač interně toogenerate klientského objektu, který lze použít toocommunicate službou.</span><span class="sxs-lookup"><span data-stu-id="79012-168">hello factories use hello resolver internally toogenerate a client object that can be used toocommunicate with services.</span></span>

### <a name="communication-clients-and-factories"></a><span data-ttu-id="79012-169">Komunikace klientů a objekty pro vytváření</span><span class="sxs-lookup"><span data-stu-id="79012-169">Communication clients and factories</span></span>
<span data-ttu-id="79012-170">Knihovna vytváření komunikace Hello implementuje typický vzor opakování selhání zpracování, které usnadňuje koncovým bodům služby tooresolved Probíhá opakování připojení.</span><span class="sxs-lookup"><span data-stu-id="79012-170">hello communication factory library implements a typical fault-handling retry pattern that makes retrying connections tooresolved service endpoints easier.</span></span> <span data-ttu-id="79012-171">Knihovna vytváření Hello poskytuje mechanismus opakování hello zatímco poskytují hello chyba obslužné rutiny.</span><span class="sxs-lookup"><span data-stu-id="79012-171">hello factory library provides hello retry mechanism while you provide hello error handlers.</span></span>

<span data-ttu-id="79012-172">`ICommunicationClientFactory(C#) / CommunicationClientFactory(Java)`definuje základní rozhraní hello implementované objekt factory komunikaci klienta, který vytváří klientů, které může kontaktovat službu tooa Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="79012-172">`ICommunicationClientFactory(C#) / CommunicationClientFactory(Java)` defines hello base interface implemented by a communication client factory that produces clients that can talk tooa Service Fabric service.</span></span> <span data-ttu-id="79012-173">Hello implementace hello CommunicationClientFactory závisí na používá služba hello Service Fabric, kde hello klienta chce toocommunicate hello komunikačního balíku.</span><span class="sxs-lookup"><span data-stu-id="79012-173">hello implementation of hello CommunicationClientFactory depends on hello communication stack used by hello Service Fabric service where hello client wants toocommunicate.</span></span> <span data-ttu-id="79012-174">poskytuje technologie Hello spolehlivé rozhraní API služby `CommunicationClientFactoryBase<TCommunicationClient>`.</span><span class="sxs-lookup"><span data-stu-id="79012-174">hello Reliable Services API provides a `CommunicationClientFactoryBase<TCommunicationClient>`.</span></span> <span data-ttu-id="79012-175">To poskytuje základní implementaci rozhraní CommunicationClientFactory hello a provádí úlohy, které jsou společné tooall hello komunikace zásobníky.</span><span class="sxs-lookup"><span data-stu-id="79012-175">This provides a base implementation of hello CommunicationClientFactory interface and performs tasks that are common tooall hello communication stacks.</span></span> <span data-ttu-id="79012-176">(Tyto úlohy patří pomocí koncového bodu služby ServicePartitionResolver toodetermine hello).</span><span class="sxs-lookup"><span data-stu-id="79012-176">(These tasks include using a ServicePartitionResolver toodetermine hello service endpoint).</span></span> <span data-ttu-id="79012-177">Klienti obvykle implementovat hello abstraktní CommunicationClientFactoryBase třída toohandle logiku, která je konkrétní toohello komunikačního balíku.</span><span class="sxs-lookup"><span data-stu-id="79012-177">Clients usually implement hello abstract CommunicationClientFactoryBase class toohandle logic that is specific toohello communication stack.</span></span>

<span data-ttu-id="79012-178">Hello komunikace klienta pouze obdrží adresu a používá je tooconnect tooa služby.</span><span class="sxs-lookup"><span data-stu-id="79012-178">hello communication client just receives an address and uses it tooconnect tooa service.</span></span> <span data-ttu-id="79012-179">Hello klienta můžete použít libovolnou protokol ho chce.</span><span class="sxs-lookup"><span data-stu-id="79012-179">hello client can use whatever protocol it wants.</span></span>

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

<span data-ttu-id="79012-180">Dodavatel klienta Hello především zodpovídá za vytvoření komunikace klientů.</span><span class="sxs-lookup"><span data-stu-id="79012-180">hello client factory is primarily responsible for creating communication clients.</span></span> <span data-ttu-id="79012-181">Pro klienty, kteří nejsou udržovat trvalé připojení, jako je například klientovi HTTP musí objekt pro vytváření hello pouze toocreate a návratové hello klienta.</span><span class="sxs-lookup"><span data-stu-id="79012-181">For clients that don't maintain a persistent connection, such as an HTTP client, hello factory only needs toocreate and return hello client.</span></span> <span data-ttu-id="79012-182">Jiné protokoly, které udržují trvalé připojení, jako je například některé binární protokoly, by také být ověřen hello factory toodetermine zda hello připojení musí toobe znovu vytvořit.</span><span class="sxs-lookup"><span data-stu-id="79012-182">Other protocols that maintain a persistent connection, such as some binary protocols, should also be validated by hello factory toodetermine whether hello connection needs toobe re-created.</span></span>  

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

<span data-ttu-id="79012-183">Nakonec obslužné rutiny výjimky je zodpovědná za určení jaké akce tootake, když dojde k výjimce.</span><span class="sxs-lookup"><span data-stu-id="79012-183">Finally, an exception handler is responsible for determining what action tootake when an exception occurs.</span></span> <span data-ttu-id="79012-184">Výjimky jsou rozdělené do **opakovatelné** a **neopakovatelného**.</span><span class="sxs-lookup"><span data-stu-id="79012-184">Exceptions are categorized into **retryable** and **non retryable**.</span></span>

* <span data-ttu-id="79012-185">**Neopakovatelného** výjimky jednoduše získat znovu vyvolány back toohello volajícího.</span><span class="sxs-lookup"><span data-stu-id="79012-185">**Non retryable** exceptions simply get rethrown back toohello caller.</span></span>
* <span data-ttu-id="79012-186">**Opakovatelná** výjimky se dále dělí do **přechodný** a **-pouze dočasné**.</span><span class="sxs-lookup"><span data-stu-id="79012-186">**retryable** exceptions are further categorized into **transient** and **non-transient**.</span></span>
  * <span data-ttu-id="79012-187">**Přechodný** výjimky jsou ty, které můžete jednoduše opakovat bez znovu řešení hello adresa koncového bodu služby.</span><span class="sxs-lookup"><span data-stu-id="79012-187">**Transient** exceptions are those that can simply be retried without re-resolving hello service endpoint address.</span></span> <span data-ttu-id="79012-188">Patří přechodné problémy se sítí nebo služby chybové odpovědi kromě těch, které označují hello adresa koncového bodu služby neexistuje.</span><span class="sxs-lookup"><span data-stu-id="79012-188">These will include transient network problems or service error responses other than those that indicate hello service endpoint address does not exist.</span></span>
  * <span data-ttu-id="79012-189">**Bez přechodná** výjimky jsou ty, které vyžadují hello služby koncový bod adresy toobe znovu přeložit.</span><span class="sxs-lookup"><span data-stu-id="79012-189">**Non-transient** exceptions are those that require hello service endpoint address toobe re-resolved.</span></span> <span data-ttu-id="79012-190">Mezi ně patří výjimky, které označují, že koncový bod služby hello není dostupný, což značí, že služba hello přesunul tooa jiný uzel.</span><span class="sxs-lookup"><span data-stu-id="79012-190">These include exceptions that indicate hello service endpoint could not be reached, indicating hello service has moved tooa different node.</span></span>

<span data-ttu-id="79012-191">Hello `TryHandleException` provádí rozhodnutí o dané výjimka.</span><span class="sxs-lookup"><span data-stu-id="79012-191">hello `TryHandleException` makes a decision about a given exception.</span></span> <span data-ttu-id="79012-192">Pokud ho **neví** co toomake rozhodnutí o výjimku, by měla vrátit **false**.</span><span class="sxs-lookup"><span data-stu-id="79012-192">If it **does not know** what decisions toomake about an exception, it should return **false**.</span></span> <span data-ttu-id="79012-193">Pokud ho **vědět** co toomake rozhodnutí, měl by odpovídajícím způsobem nastavit hello výsledek a vrátí **true**.</span><span class="sxs-lookup"><span data-stu-id="79012-193">If it **does know** what decision toomake, it should set hello result accordingly and return **true**.</span></span>

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

        // if exceptionInformation.Exception is unknown (let hello next IExceptionHandler attempt toohandle it)
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

        /* if exceptionInformation.getException() is unknown (let hello next ExceptionHandler attempt toohandle it)
         */
        result = null;
        return false;

    }
}
```
### <a name="putting-it-all-together"></a><span data-ttu-id="79012-194">Třeba umisťovat všechny společně</span><span class="sxs-lookup"><span data-stu-id="79012-194">Putting it all together</span></span>
<span data-ttu-id="79012-195">S `ICommunicationClient(C#) / CommunicationClient(Java)`, `ICommunicationClientFactory(C#) / CommunicationClientFactory(Java)`, a `IExceptionHandler(C#) / ExceptionHandler(Java)` vytvořených na základě komunikační protokol, `ServicePartitionClient(C#) / FabricServicePartitionClient(Java)` všechny společně se zabalí a poskytuje odolnost zpracování hello a služby oddílu adresu řešení smyčky kolem těchto součástí.</span><span class="sxs-lookup"><span data-stu-id="79012-195">With an `ICommunicationClient(C#) / CommunicationClient(Java)`, `ICommunicationClientFactory(C#) / CommunicationClientFactory(Java)`, and `IExceptionHandler(C#) / ExceptionHandler(Java)` built around a communication protocol, a `ServicePartitionClient(C#) / FabricServicePartitionClient(Java)` wraps it all together and provides hello fault-handling and service partition address resolution loop around these components.</span></span>

```csharp
private MyCommunicationClientFactory myCommunicationClientFactory;
private Uri myServiceUri;

var myServicePartitionClient = new ServicePartitionClient<MyCommunicationClient>(
    this.myCommunicationClientFactory,
    this.myServiceUri,
    myPartitionKey);

var result = await myServicePartitionClient.InvokeWithRetryAsync(async (client) =>
   {
      // Communicate with hello service using hello client.
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
      /* Communicate with hello service using hello client.
       */
   });

```

## <a name="next-steps"></a><span data-ttu-id="79012-196">Další kroky</span><span class="sxs-lookup"><span data-stu-id="79012-196">Next steps</span></span>
* <span data-ttu-id="79012-197">Zobrazit příklad komunikaci pomocí protokolu HTTP mezi službami v [ukázkový projekt C# na Githubu](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Services/WordCount) nebo [Java ukázkový projekt na Githubu](https://github.com/Azure-Samples/service-fabric-java-getting-started/tree/master/Services/WatchDog).</span><span class="sxs-lookup"><span data-stu-id="79012-197">See an example of HTTP communication between services in a [C# sample project on GitHUb](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Services/WordCount) or [Java sample project on GitHUb](https://github.com/Azure-Samples/service-fabric-java-getting-started/tree/master/Services/WatchDog).</span></span>
* [<span data-ttu-id="79012-198">Volání vzdálených procedur s vzdálenou komunikaci spolehlivé služby</span><span class="sxs-lookup"><span data-stu-id="79012-198">Remote procedure calls with Reliable Services remoting</span></span>](service-fabric-reliable-services-communication-remoting.md)
* [<span data-ttu-id="79012-199">Webové rozhraní API, která používá OWIN v spolehlivé služby</span><span class="sxs-lookup"><span data-stu-id="79012-199">Web API that uses OWIN in Reliable Services</span></span>](service-fabric-reliable-services-communication-webapi.md)
* [<span data-ttu-id="79012-200">Komunikace WCF pomocí spolehlivé služby</span><span class="sxs-lookup"><span data-stu-id="79012-200">WCF communication by using Reliable Services</span></span>](service-fabric-reliable-services-communication-wcf.md)
