---
title: "Vzdálená komunikace aaaService v Service Fabric | Microsoft Docs"
description: "Vzdálená komunikace Service Fabric umožňuje klientům a službám toocommunicate prostřednictvím služby s použitím vzdálené volání procedury."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: BharatNarasimman
ms.assetid: abfaf430-fea0-4974-afba-cfc9f9f2354b
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 04/20/2017
ms.author: vturecek
ms.openlocfilehash: 14682cf8671a85e04144eccf97803ab67c258875
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="service-remoting-with-reliable-services"></a><span data-ttu-id="a5371-103">Služba vzdálené komunikace se službami Reliable Services</span><span class="sxs-lookup"><span data-stu-id="a5371-103">Service remoting with Reliable Services</span></span>
<span data-ttu-id="a5371-104">Pro služby, které nejsou svázané tooa konkrétní komunikační protokol nebo zásobníku, například WebAPI, Windows Communication Foundation (WCF) nebo jiných hello spolehlivé služby framework poskytuje mechanismus tooquickly vzdálenou komunikaci a snadno nastavit vzdáleného volání procedur pro služby.</span><span class="sxs-lookup"><span data-stu-id="a5371-104">For services that are not tied tooa particular communication protocol or stack, such as WebAPI, Windows Communication Foundation (WCF), or others, hello Reliable Services framework provides a remoting mechanism tooquickly and easily set up remote procedure call for services.</span></span>

## <a name="set-up-remoting-on-a-service"></a><span data-ttu-id="a5371-105">Nastavování vzdálené komunikace na službě</span><span class="sxs-lookup"><span data-stu-id="a5371-105">Set up remoting on a service</span></span>
<span data-ttu-id="a5371-106">Nastavení vzdálené komunikace pro služby se provádí ve dvou jednoduchých kroků:</span><span class="sxs-lookup"><span data-stu-id="a5371-106">Setting up remoting for a service is done in two simple steps:</span></span>

1. <span data-ttu-id="a5371-107">Vytvořte rozhraní pro vaše tooimplement služby.</span><span class="sxs-lookup"><span data-stu-id="a5371-107">Create an interface for your service tooimplement.</span></span> <span data-ttu-id="a5371-108">Toto rozhraní definuje hello metody, které budou k dispozici pro vzdálené volání procedury vaší služby.</span><span class="sxs-lookup"><span data-stu-id="a5371-108">This interface defines hello methods that will be available for a remote procedure call on your service.</span></span> <span data-ttu-id="a5371-109">Hello metody musí být vrácení úloh asynchronních metod.</span><span class="sxs-lookup"><span data-stu-id="a5371-109">hello methods must be task-returning asynchronous methods.</span></span> <span data-ttu-id="a5371-110">musí implementovat rozhraní Hello `Microsoft.ServiceFabric.Services.Remoting.IService` toosignal, který hello služby má vzdálené komunikace rozhraní.</span><span class="sxs-lookup"><span data-stu-id="a5371-110">hello interface must implement `Microsoft.ServiceFabric.Services.Remoting.IService` toosignal that hello service has a remoting interface.</span></span>
2. <span data-ttu-id="a5371-111">Použijte vzdálenou komunikaci naslouchací proces ve službě.</span><span class="sxs-lookup"><span data-stu-id="a5371-111">Use a remoting listener in your service.</span></span> <span data-ttu-id="a5371-112">Jedná se `ICommunicationListener` implementace, která poskytuje funkce vzdálené komunikace.</span><span class="sxs-lookup"><span data-stu-id="a5371-112">This is an `ICommunicationListener` implementation that provides remoting capabilities.</span></span> <span data-ttu-id="a5371-113">Hello `Microsoft.ServiceFabric.Services.Remoting.Runtime` obor názvů obsahuje metody rozšíření`CreateServiceRemotingListener` pro bezstavové a stavové služby, které můžete použít toocreate naslouchacího procesu vzdálené komunikace používat hello výchozí vzdálenou komunikaci přenosového protokolu.</span><span class="sxs-lookup"><span data-stu-id="a5371-113">hello `Microsoft.ServiceFabric.Services.Remoting.Runtime` namespace contains an extension method,`CreateServiceRemotingListener` for both stateless and stateful services that can be used toocreate a remoting listener using hello default remoting transport protocol.</span></span>

<span data-ttu-id="a5371-114">Poznámka: hello `Remoting` obor názvů je k dispozici jako samostatné nuget balíček s názvem`Microsoft.ServiceFabric.Services.Remoting`</span><span class="sxs-lookup"><span data-stu-id="a5371-114">Note: hello `Remoting` namespace is available as a seperate nuget package called `Microsoft.ServiceFabric.Services.Remoting`</span></span> 

<span data-ttu-id="a5371-115">Například hello následující bezstavové služby zpřístupní jedné metody tooget "Hello, World" přes vzdálené volání procedury.</span><span class="sxs-lookup"><span data-stu-id="a5371-115">For example, hello following stateless service exposes a single method tooget "Hello World" over a remote procedure call.</span></span>

```csharp
using Microsoft.ServiceFabric.Services.Communication.Runtime;
using Microsoft.ServiceFabric.Services.Remoting;
using Microsoft.ServiceFabric.Services.Remoting.Runtime;
using Microsoft.ServiceFabric.Services.Runtime;

public interface IMyService : IService
{
    Task<string> HelloWorldAsync();
}

class MyService : StatelessService, IMyService
{
    public MyService(StatelessServiceContext context)
        : base (context)
    {
    }

    public Task HelloWorldAsync()
    {
        return Task.FromResult("Hello!");
    }

    protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        return new[] { new ServiceInstanceListener(context =>            this.CreateServiceRemotingListener(context)) };
    }
}
```
> [!NOTE]
> <span data-ttu-id="a5371-116">Hello argumentů a hello vrátit typy v rozhraní služby hello může být jakékoli jednoduchý, komplexní nebo vlastní typy, ale musí být serializovatelný podle hello .NET [DataContractSerializer](https://msdn.microsoft.com/library/ms731923.aspx).</span><span class="sxs-lookup"><span data-stu-id="a5371-116">hello arguments and hello return types in hello service interface can be any simple, complex, or custom types, but they must be serializable by hello .NET [DataContractSerializer](https://msdn.microsoft.com/library/ms731923.aspx).</span></span>
>
>

## <a name="call-remote-service-methods"></a><span data-ttu-id="a5371-117">Volání metody vzdálené služby</span><span class="sxs-lookup"><span data-stu-id="a5371-117">Call remote service methods</span></span>
<span data-ttu-id="a5371-118">Volání metod pro službu pomocí hello vzdálené komunikace zásobníku se provádí pomocí služby toohello místní proxy prostřednictvím hello `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxy` třídy.</span><span class="sxs-lookup"><span data-stu-id="a5371-118">Calling methods on a service by using hello remoting stack is done by using a local proxy toohello service through hello `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxy` class.</span></span> <span data-ttu-id="a5371-119">Hello `ServiceProxy` metoda vytvoří místní proxy server pomocí hello implementuje stejné rozhraní, která hello služby.</span><span class="sxs-lookup"><span data-stu-id="a5371-119">hello `ServiceProxy` method creates a local proxy by using hello same interface that hello service implements.</span></span> <span data-ttu-id="a5371-120">Pomocí proxy můžete jednoduše volat metody na rozhraní hello vzdáleně.</span><span class="sxs-lookup"><span data-stu-id="a5371-120">With that proxy, you can simply call methods on hello interface remotely.</span></span>

```csharp

IMyService helloWorldClient = ServiceProxy.Create<IMyService>(new Uri("fabric:/MyApplication/MyHelloWorldService"));

string message = await helloWorldClient.HelloWorldAsync();

```

<span data-ttu-id="a5371-121">Vzdálená komunikace framework Hello rozšíří výjimky vydané v klientovi toohello služby hello.</span><span class="sxs-lookup"><span data-stu-id="a5371-121">hello remoting framework propagates exceptions thrown at hello service toohello client.</span></span> <span data-ttu-id="a5371-122">Proto výjimek logiky na klientovi hello pomocí `ServiceProxy` můžete přímo zpracování výjimek, které hello vyvolává službu.</span><span class="sxs-lookup"><span data-stu-id="a5371-122">So exception-handling logic at hello client by using `ServiceProxy` can directly handle exceptions that hello service throws.</span></span>

## <a name="service-proxy-lifetime"></a><span data-ttu-id="a5371-123">Doby platnosti Proxy služby</span><span class="sxs-lookup"><span data-stu-id="a5371-123">Service Proxy Lifetime</span></span>
<span data-ttu-id="a5371-124">Vytvoření ServiceProxy je lightweight operace, takže uživatel může vytvořit tolik, jako je potřebují.</span><span class="sxs-lookup"><span data-stu-id="a5371-124">ServiceProxy creation is a lightweight operation, so user can create as many as they need it.</span></span> <span data-ttu-id="a5371-125">Proxy server služby lze znovu použít, dokud ho uživatel potřebovat.</span><span class="sxs-lookup"><span data-stu-id="a5371-125">Service Proxy can be re-used as long as user need it.</span></span> <span data-ttu-id="a5371-126">Uživatel může znovu použít hello stejné proxy serveru v případě výjimky.</span><span class="sxs-lookup"><span data-stu-id="a5371-126">User can re-use hello same proxy in case of Exception.</span></span> <span data-ttu-id="a5371-127">Každý ServiceProxy obsahuje webovými servery klienta použít toosend zprávy přes přenosu hello.</span><span class="sxs-lookup"><span data-stu-id="a5371-127">Each ServiceProxy contains communciation client used toosend messages over hello wire.</span></span> <span data-ttu-id="a5371-128">Při volání rozhraní API, máme toosee interní kontrolu, pokud komunikace klienta použít platný.</span><span class="sxs-lookup"><span data-stu-id="a5371-128">While invoking API, we have internal check toosee if communication client used is valid.</span></span> <span data-ttu-id="a5371-129">Podle toho, že výsledků, znovu vytvoříme hello komunikace klienta.</span><span class="sxs-lookup"><span data-stu-id="a5371-129">Based on that result, we re-create hello communication client.</span></span> <span data-ttu-id="a5371-130">Uživatel proto nemusí toorecreate serviceproxy v případě výjimky.</span><span class="sxs-lookup"><span data-stu-id="a5371-130">Hence user do not need toorecreate serviceproxy in case of Exception.</span></span>

### <a name="serviceproxyfactory-lifetime"></a><span data-ttu-id="a5371-131">Doba platnosti ServiceProxyFactory</span><span class="sxs-lookup"><span data-stu-id="a5371-131">ServiceProxyFactory Lifetime</span></span>
<span data-ttu-id="a5371-132">[ServiceProxyFactory](https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicefabric.services.remoting.client.serviceproxyfactory) je objekt factory, který vytvoří proxy server pro různé vzdálené komunikace rozhraní.</span><span class="sxs-lookup"><span data-stu-id="a5371-132">[ServiceProxyFactory](https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicefabric.services.remoting.client.serviceproxyfactory) is a factory that creates proxy for different remoting interfaces.</span></span> <span data-ttu-id="a5371-133">Pokud používáte ServiceProxy.Create rozhraní API pro vytváření proxy, framework vytvoří hello singelton ServiceProxyFactory.</span><span class="sxs-lookup"><span data-stu-id="a5371-133">If you use API ServiceProxy.Create for creating proxy, then framework creates hello singelton ServiceProxyFactory.</span></span>
<span data-ttu-id="a5371-134">Je užitečné toocreate jeden ručně Pokud budete potřebovat toooverride [IServiceRemotingClientFactory](https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicefabric.services.remoting.client.iserviceremotingclientfactory) vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="a5371-134">It is useful toocreate one manually when you need toooverride [IServiceRemotingClientFactory](https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicefabric.services.remoting.client.iserviceremotingclientfactory) properties.</span></span>
<span data-ttu-id="a5371-135">Objekt Factory je náročná operace.</span><span class="sxs-lookup"><span data-stu-id="a5371-135">Factory is an expensive operation.</span></span> <span data-ttu-id="a5371-136">ServiceProxyFactory udržuje mezipaměť komunikace klienta.</span><span class="sxs-lookup"><span data-stu-id="a5371-136">ServiceProxyFactory maintains cache of communication client.</span></span>
<span data-ttu-id="a5371-137">Osvědčeným postupem je toocache ServiceProxyFactory jako dlouho.</span><span class="sxs-lookup"><span data-stu-id="a5371-137">Best practice is toocache ServiceProxyFactory for as long as possible.</span></span>

## <a name="remoting-exception-handling"></a><span data-ttu-id="a5371-138">Vzdálená komunikace zpracování výjimek</span><span class="sxs-lookup"><span data-stu-id="a5371-138">Remoting Exception Handling</span></span>
<span data-ttu-id="a5371-139">Všechny hello vzdálené došlo k výjimce rozhraním API služby, jsou odesílány zpět toohello klienta v AggregateException.</span><span class="sxs-lookup"><span data-stu-id="a5371-139">All hello remote exception thrown by service API, are sent back toohello client as AggregateException.</span></span> <span data-ttu-id="a5371-140">RemoteExceptions musí být serializovatelný kontraktu jinak [ServiceException](https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicefabric.services.communication.serviceexception) toohello proxy rozhraní API s Chyba serializace hello je vyvolána v ní.</span><span class="sxs-lookup"><span data-stu-id="a5371-140">RemoteExceptions should be DataContract Serializable otherwise [ServiceException](https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicefabric.services.communication.serviceexception) is thrown toohello proxy API with hello serialization error in it.</span></span>

<span data-ttu-id="a5371-141">ServiceProxy zpracovávat všechny výjimky převzetí služeb při selhání pro oddíl hello služby, kterou je vytvořeno.</span><span class="sxs-lookup"><span data-stu-id="a5371-141">ServiceProxy does handle all Failover Exception for hello service partition it  is created for.</span></span> <span data-ttu-id="a5371-142">Znovu přeloží koncové body hello, pokud je Exceptions(Non-Transient Exceptions) převzetí služeb při selhání a opakování volání hello hello správný koncový bod.</span><span class="sxs-lookup"><span data-stu-id="a5371-142">It re-resolves hello endpoints if there is Failover Exceptions(Non-Transient Exceptions) and retries hello call with hello correct endpoint.</span></span> <span data-ttu-id="a5371-143">Počet opakovaných pokusů pro převzetí služeb při selhání výjimka je neomezené.</span><span class="sxs-lookup"><span data-stu-id="a5371-143">Number of retries for failover Exception is indefinite.</span></span>
<span data-ttu-id="a5371-144">V případě TransientExceptions se pouze pokusí hello volání.</span><span class="sxs-lookup"><span data-stu-id="a5371-144">In case of TransientExceptions, it only retries hello call.</span></span>

<span data-ttu-id="a5371-145">Výchozí parametry opakování se poskytují podle [OperationRetrySettings].</span><span class="sxs-lookup"><span data-stu-id="a5371-145">Default retry parameters are provied by [OperationRetrySettings].</span></span> <span data-ttu-id="a5371-146">(https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicefabric.services.communication.client.operationretrysettings) Uživatel může konfigurovat tyto hodnoty pomocí předání OperationRetrySettings objekt tooServiceProxyFactory konstruktor.</span><span class="sxs-lookup"><span data-stu-id="a5371-146">(https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicefabric.services.communication.client.operationretrysettings) User can configure these values by passing OperationRetrySettings object tooServiceProxyFactory constructor.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a5371-147">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a5371-147">Next steps</span></span>
* [<span data-ttu-id="a5371-148">Webové rozhraní API s OWIN v spolehlivé služby</span><span class="sxs-lookup"><span data-stu-id="a5371-148">Web API with OWIN in Reliable Services</span></span>](service-fabric-reliable-services-communication-webapi.md)
* [<span data-ttu-id="a5371-149">WCF komunikaci se službami Reliable Services</span><span class="sxs-lookup"><span data-stu-id="a5371-149">WCF communication with Reliable Services</span></span>](service-fabric-reliable-services-communication-wcf.md)
* [<span data-ttu-id="a5371-150">Zabezpečení komunikace pro spolehlivé služby</span><span class="sxs-lookup"><span data-stu-id="a5371-150">Securing communication for Reliable Services</span></span>](service-fabric-reliable-services-secure-communication.md)
