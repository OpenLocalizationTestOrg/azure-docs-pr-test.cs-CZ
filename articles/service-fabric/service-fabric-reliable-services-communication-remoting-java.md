---
title: "Vzdálená komunikace aaaService v Azure Service Fabric | Microsoft Docs"
description: "Vzdálená komunikace Service Fabric umožňuje klientům a službám toocommunicate prostřednictvím služby s použitím vzdálené volání procedury."
services: service-fabric
documentationcenter: java
author: PavanKunapareddyMSFT
manager: timlt
ms.assetid: 
ms.service: service-fabric
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 06/30/2017
ms.author: pakunapa
ms.openlocfilehash: 1177a5ede91352dc61422f2df7424b0d5645147d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="service-remoting-with-reliable-services"></a><span data-ttu-id="232e3-103">Služba vzdálené komunikace se službami Reliable Services</span><span class="sxs-lookup"><span data-stu-id="232e3-103">Service remoting with Reliable Services</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="232e3-104">C# v systému Windows</span><span class="sxs-lookup"><span data-stu-id="232e3-104">C# on Windows</span></span>](service-fabric-reliable-services-communication-remoting.md)
> * [<span data-ttu-id="232e3-105">Java v Linuxu</span><span class="sxs-lookup"><span data-stu-id="232e3-105">Java on Linux</span></span>](service-fabric-reliable-services-communication-remoting-java.md)
>
>

<span data-ttu-id="232e3-106">Hello spolehlivé služby framework poskytuje mechanismus tooquickly vzdálenou komunikaci a snadno nastavit vzdáleného volání procedur pro služby.</span><span class="sxs-lookup"><span data-stu-id="232e3-106">hello Reliable Services framework provides a remoting mechanism tooquickly and easily set up remote procedure call for services.</span></span>

## <a name="set-up-remoting-on-a-service"></a><span data-ttu-id="232e3-107">Nastavování vzdálené komunikace na službě</span><span class="sxs-lookup"><span data-stu-id="232e3-107">Set up remoting on a service</span></span>
<span data-ttu-id="232e3-108">Nastavení vzdálené komunikace pro služby se provádí ve dvou jednoduchých kroků:</span><span class="sxs-lookup"><span data-stu-id="232e3-108">Setting up remoting for a service is done in two simple steps:</span></span>

1. <span data-ttu-id="232e3-109">Vytvořte rozhraní pro vaše tooimplement služby.</span><span class="sxs-lookup"><span data-stu-id="232e3-109">Create an interface for your service tooimplement.</span></span> <span data-ttu-id="232e3-110">Toto rozhraní definuje hello metody, které jsou k dispozici pro vzdálené volání procedury vaší služby.</span><span class="sxs-lookup"><span data-stu-id="232e3-110">This interface defines hello methods that are available for a remote procedure call on your service.</span></span> <span data-ttu-id="232e3-111">Hello metody musí být vrácení úloh asynchronních metod.</span><span class="sxs-lookup"><span data-stu-id="232e3-111">hello methods must be task-returning asynchronous methods.</span></span> <span data-ttu-id="232e3-112">musí implementovat rozhraní Hello `microsoft.serviceFabric.services.remoting.Service` toosignal, který hello služby má vzdálené komunikace rozhraní.</span><span class="sxs-lookup"><span data-stu-id="232e3-112">hello interface must implement `microsoft.serviceFabric.services.remoting.Service` toosignal that hello service has a remoting interface.</span></span>
2. <span data-ttu-id="232e3-113">Použijte vzdálenou komunikaci naslouchací proces ve službě.</span><span class="sxs-lookup"><span data-stu-id="232e3-113">Use a remoting listener in your service.</span></span> <span data-ttu-id="232e3-114">Jedná se `CommunicationListener` implementace, která poskytuje funkce vzdálené komunikace.</span><span class="sxs-lookup"><span data-stu-id="232e3-114">This is an `CommunicationListener` implementation that provides remoting capabilities.</span></span> <span data-ttu-id="232e3-115">`FabricTransportServiceRemotingListener`lze použít toocreate naslouchací proces vzdálenou komunikaci pomocí protokolu přenosu vzdálenou komunikaci výchozí hello.</span><span class="sxs-lookup"><span data-stu-id="232e3-115">`FabricTransportServiceRemotingListener` can be used toocreate a remoting listener using hello default remoting transport protocol.</span></span>

<span data-ttu-id="232e3-116">Například hello následující bezstavové služby zpřístupní jedné metody tooget "Hello, World" přes vzdálené volání procedury.</span><span class="sxs-lookup"><span data-stu-id="232e3-116">For example, hello following stateless service exposes a single method tooget "Hello World" over a remote procedure call.</span></span>

```java
import java.util.ArrayList;
import java.util.concurrent.CompletableFuture;
import java.util.List;
import microsoft.servicefabric.services.communication.runtime.ServiceInstanceListener;
import microsoft.servicefabric.services.remoting.Service;
import microsoft.servicefabric.services.runtime.StatelessService;

public interface MyService extends Service {
    CompletableFuture<String> helloWorldAsync();
}

class MyServiceImpl extends StatelessService implements MyService {
    public MyServiceImpl(StatelessServiceContext context) {
       super(context);
    }

    public CompletableFuture<String> helloWorldAsync() {
        return CompletableFuture.completedFuture("Hello!");
    }

    @Override
    protected List<ServiceInstanceListener> createServiceInstanceListeners() {
        ArrayList<ServiceInstanceListener> listeners = new ArrayList<>();
        listeners.add(new ServiceInstanceListener((context) -> {
            return new FabricTransportServiceRemotingListener(context,this);
        }));
        return listeners;
    }
}
```

> [!NOTE]
> <span data-ttu-id="232e3-117">argumenty Hello a hello vrátí typy v rozhraní služby hello může být jakékoli jednoduchý, komplexní nebo vlastní typy, ale musí být serializovatelný.</span><span class="sxs-lookup"><span data-stu-id="232e3-117">hello arguments and hello return types in hello service interface can be any simple, complex, or custom types, but they must be serializable.</span></span>
>
>

## <a name="call-remote-service-methods"></a><span data-ttu-id="232e3-118">Volání metody vzdálené služby</span><span class="sxs-lookup"><span data-stu-id="232e3-118">Call remote service methods</span></span>
<span data-ttu-id="232e3-119">Volání metod pro službu pomocí hello vzdálené komunikace zásobníku se provádí pomocí služby toohello místní proxy prostřednictvím hello `microsoft.serviceFabric.services.remoting.client.ServiceProxyBase` třídy.</span><span class="sxs-lookup"><span data-stu-id="232e3-119">Calling methods on a service by using hello remoting stack is done by using a local proxy toohello service through hello `microsoft.serviceFabric.services.remoting.client.ServiceProxyBase` class.</span></span> <span data-ttu-id="232e3-120">Hello `ServiceProxyBase` metoda vytvoří místní proxy server pomocí hello implementuje stejné rozhraní, která hello služby.</span><span class="sxs-lookup"><span data-stu-id="232e3-120">hello `ServiceProxyBase` method creates a local proxy by using hello same interface that hello service implements.</span></span> <span data-ttu-id="232e3-121">Pomocí proxy můžete jednoduše volat metody na rozhraní hello vzdáleně.</span><span class="sxs-lookup"><span data-stu-id="232e3-121">With that proxy, you can simply call methods on hello interface remotely.</span></span>

```java

MyService helloWorldClient = ServiceProxyBase.create(MyService.class, new URI("fabric:/MyApplication/MyHelloWorldService"));

CompletableFuture<String> message = helloWorldClient.helloWorldAsync();

```

<span data-ttu-id="232e3-122">Vzdálená komunikace framework Hello rozšíří výjimky vydané v klientovi toohello služby hello.</span><span class="sxs-lookup"><span data-stu-id="232e3-122">hello remoting framework propagates exceptions thrown at hello service toohello client.</span></span> <span data-ttu-id="232e3-123">Proto výjimek logiky na klientovi hello pomocí `ServiceProxyBase` můžete přímo zpracování výjimek, které hello vyvolává službu.</span><span class="sxs-lookup"><span data-stu-id="232e3-123">So exception-handling logic at hello client by using `ServiceProxyBase` can directly handle exceptions that hello service throws.</span></span>

## <a name="service-proxy-lifetime"></a><span data-ttu-id="232e3-124">Doby platnosti Proxy služby</span><span class="sxs-lookup"><span data-stu-id="232e3-124">Service Proxy Lifetime</span></span>
<span data-ttu-id="232e3-125">Vytvoření ServiceProxy je lightweight operace, takže uživatel může vytvořit tolik, jako je potřebují.</span><span class="sxs-lookup"><span data-stu-id="232e3-125">ServiceProxy creation is a lightweight operation, so user can create as many as they need it.</span></span> <span data-ttu-id="232e3-126">Proxy server služby lze znovu použít, dokud ho uživatel potřebovat.</span><span class="sxs-lookup"><span data-stu-id="232e3-126">Service Proxy can be re-used as long as user need it.</span></span> <span data-ttu-id="232e3-127">Uživatel může znovu použít hello stejné proxy serveru v případě výjimky.</span><span class="sxs-lookup"><span data-stu-id="232e3-127">User can re-use hello same proxy in case of Exception.</span></span> <span data-ttu-id="232e3-128">Každý ServiceProxy obsahuje zprávy toosend klient používá komunikaci přes přenosu hello.</span><span class="sxs-lookup"><span data-stu-id="232e3-128">Each ServiceProxy contains communication client used toosend messages over hello wire.</span></span> <span data-ttu-id="232e3-129">Při volání rozhraní API, máme toosee interní kontrolu, pokud komunikace klienta použít platný.</span><span class="sxs-lookup"><span data-stu-id="232e3-129">While invoking API, we have internal check toosee if communication client used is valid.</span></span> <span data-ttu-id="232e3-130">Podle toho, že výsledků, znovu vytvoříme hello komunikace klienta.</span><span class="sxs-lookup"><span data-stu-id="232e3-130">Based on that result, we re-create hello communication client.</span></span> <span data-ttu-id="232e3-131">Uživatel proto nemusí toorecreate serviceproxy v případě výjimky.</span><span class="sxs-lookup"><span data-stu-id="232e3-131">Hence user do not need toorecreate serviceproxy in case of Exception.</span></span>

### <a name="serviceproxyfactory-lifetime"></a><span data-ttu-id="232e3-132">Doba platnosti ServiceProxyFactory</span><span class="sxs-lookup"><span data-stu-id="232e3-132">ServiceProxyFactory Lifetime</span></span>
<span data-ttu-id="232e3-133">[FabricServiceProxyFactory](https://docs.microsoft.com/en-us/java/api/microsoft.servicefabric.services.remoting.client._fabric_service_proxy_factory) je objekt factory, který vytvoří proxy server pro různé vzdálené komunikace rozhraní.</span><span class="sxs-lookup"><span data-stu-id="232e3-133">[FabricServiceProxyFactory](https://docs.microsoft.com/en-us/java/api/microsoft.servicefabric.services.remoting.client._fabric_service_proxy_factory) is a factory that creates proxy for different remoting interfaces.</span></span> <span data-ttu-id="232e3-134">Pokud používáte rozhraní API `ServiceProxyBase.create` k vytváření proxy serveru, pak framework vytvoří `FabricServiceProxyFactory`.</span><span class="sxs-lookup"><span data-stu-id="232e3-134">If you use API `ServiceProxyBase.create` for creating proxy, then framework creates a `FabricServiceProxyFactory`.</span></span>
<span data-ttu-id="232e3-135">Je užitečné toocreate jeden ručně Pokud budete potřebovat toooverride [ServiceRemotingClientFactory](https://docs.microsoft.com/en-us/java/api/microsoft.servicefabric.services.remoting.client._service_remoting_client_factory) vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="232e3-135">It is useful toocreate one manually when you need toooverride [ServiceRemotingClientFactory](https://docs.microsoft.com/en-us/java/api/microsoft.servicefabric.services.remoting.client._service_remoting_client_factory) properties.</span></span>
<span data-ttu-id="232e3-136">Objekt Factory je náročná operace.</span><span class="sxs-lookup"><span data-stu-id="232e3-136">Factory is an expensive operation.</span></span> <span data-ttu-id="232e3-137">`FabricServiceProxyFactory`udržuje mezipaměť komunikace klientů.</span><span class="sxs-lookup"><span data-stu-id="232e3-137">`FabricServiceProxyFactory` maintains cache of communication clients.</span></span>
<span data-ttu-id="232e3-138">Osvědčeným postupem je toocache `FabricServiceProxyFactory` jako dlouho.</span><span class="sxs-lookup"><span data-stu-id="232e3-138">Best practice is toocache `FabricServiceProxyFactory` for as long as possible.</span></span>

## <a name="remoting-exception-handling"></a><span data-ttu-id="232e3-139">Vzdálená komunikace zpracování výjimek</span><span class="sxs-lookup"><span data-stu-id="232e3-139">Remoting Exception Handling</span></span>
<span data-ttu-id="232e3-140">Všechny hello vzdálené došlo k výjimce rozhraním API služby, jsou odesílány zpět toohello klienta jako runtimeexception – nebo FabricException.</span><span class="sxs-lookup"><span data-stu-id="232e3-140">All hello remote exception thrown by service API, are sent back toohello client either as RuntimeException or FabricException.</span></span>

<span data-ttu-id="232e3-141">ServiceProxy zpracovávat všechny výjimky převzetí služeb při selhání pro oddíl hello služby, kterou je vytvořeno.</span><span class="sxs-lookup"><span data-stu-id="232e3-141">ServiceProxy does handle all Failover Exception for hello service partition it  is created for.</span></span> <span data-ttu-id="232e3-142">Znovu přeloží koncové body hello, pokud je Exceptions(Non-Transient Exceptions) převzetí služeb při selhání a opakování volání hello hello správný koncový bod.</span><span class="sxs-lookup"><span data-stu-id="232e3-142">It re-resolves hello endpoints if there is Failover Exceptions(Non-Transient Exceptions) and retries hello call with hello correct endpoint.</span></span> <span data-ttu-id="232e3-143">Počet opakovaných pokusů pro převzetí služeb při selhání výjimka je neomezené.</span><span class="sxs-lookup"><span data-stu-id="232e3-143">Number of retries for failover Exception is indefinite.</span></span>
<span data-ttu-id="232e3-144">V případě TransientExceptions se pouze pokusí hello volání.</span><span class="sxs-lookup"><span data-stu-id="232e3-144">In case of TransientExceptions, it only retries hello call.</span></span>

<span data-ttu-id="232e3-145">Výchozí parametry opakování se poskytují podle [OperationRetrySettings].</span><span class="sxs-lookup"><span data-stu-id="232e3-145">Default retry parameters are provied by [OperationRetrySettings].</span></span> <span data-ttu-id="232e3-146">(https://docs.microsoft.com/en-us/java/api/microsoft.servicefabric.services.communication.client._operation_retry_settings) Uživatel může konfigurovat tyto hodnoty pomocí předání OperationRetrySettings objekt tooServiceProxyFactory konstruktor.</span><span class="sxs-lookup"><span data-stu-id="232e3-146">(https://docs.microsoft.com/en-us/java/api/microsoft.servicefabric.services.communication.client._operation_retry_settings) User can configure these values by passing OperationRetrySettings object tooServiceProxyFactory constructor.</span></span>

## <a name="next-steps"></a><span data-ttu-id="232e3-147">Další kroky</span><span class="sxs-lookup"><span data-stu-id="232e3-147">Next steps</span></span>
* [<span data-ttu-id="232e3-148">Zabezpečení komunikace pro spolehlivé služby</span><span class="sxs-lookup"><span data-stu-id="232e3-148">Securing communication for Reliable Services</span></span>](service-fabric-reliable-services-secure-communication.md)
