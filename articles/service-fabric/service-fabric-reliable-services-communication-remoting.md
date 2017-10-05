---
title: "<span data-ttu-id=\"6a37b-101\">Vzdálená komunikace služby v Service Fabric | Microsoft Docs</span><span class=\"sxs-lookup\"><span data-stu-id=\"6a37b-101\">Service remoting in Service Fabric | Microsoft Docs</span></span>"
description: "<span data-ttu-id=\"6a37b-102\">Vzdálená komunikace Service Fabric umožňuje služby a klienti komunikovat se službami s použitím vzdálené volání procedury.</span><span class=\"sxs-lookup\"><span data-stu-id=\"6a37b-102\">Service Fabric remoting allows clients and services to communicate with services by using a remote procedure call.</span></span>"
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
ms.openlocfilehash: 92a8894f24c234fbf38eda086531b524cceccfc1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="service-remoting-with-reliable-services"></a><span data-ttu-id="6a37b-103">Služba vzdálené komunikace se službami Reliable Services</span><span class="sxs-lookup"><span data-stu-id="6a37b-103">Service remoting with Reliable Services</span></span>
<span data-ttu-id="6a37b-104">Pro služby, které nejsou svázané s konkrétní komunikační protokol nebo zásobníku, například WebAPI, Windows Communication Foundation (WCF) nebo jiné spolehlivé služby framework poskytuje mechanismus vzdálenou komunikaci rychle a snadno nastavit vzdáleného volání procedur pro služby.</span><span class="sxs-lookup"><span data-stu-id="6a37b-104">For services that are not tied to a particular communication protocol or stack, such as WebAPI, Windows Communication Foundation (WCF), or others, the Reliable Services framework provides a remoting mechanism to quickly and easily set up remote procedure call for services.</span></span>

## <a name="set-up-remoting-on-a-service"></a><span data-ttu-id="6a37b-105">Nastavování vzdálené komunikace na službě</span><span class="sxs-lookup"><span data-stu-id="6a37b-105">Set up remoting on a service</span></span>
<span data-ttu-id="6a37b-106">Nastavení vzdálené komunikace pro služby se provádí ve dvou jednoduchých kroků:</span><span class="sxs-lookup"><span data-stu-id="6a37b-106">Setting up remoting for a service is done in two simple steps:</span></span>

1. <span data-ttu-id="6a37b-107">Vytvořte rozhraní služby k implementaci.</span><span class="sxs-lookup"><span data-stu-id="6a37b-107">Create an interface for your service to implement.</span></span> <span data-ttu-id="6a37b-108">Toto rozhraní definuje metody, které budou k dispozici pro vzdálené volání procedury vaší služby.</span><span class="sxs-lookup"><span data-stu-id="6a37b-108">This interface defines the methods that will be available for a remote procedure call on your service.</span></span> <span data-ttu-id="6a37b-109">Metody musí být vrácení úloh asynchronních metod.</span><span class="sxs-lookup"><span data-stu-id="6a37b-109">The methods must be task-returning asynchronous methods.</span></span> <span data-ttu-id="6a37b-110">Musí implementovat rozhraní `Microsoft.ServiceFabric.Services.Remoting.IService` signál, že služba má vzdálené komunikace rozhraní.</span><span class="sxs-lookup"><span data-stu-id="6a37b-110">The interface must implement `Microsoft.ServiceFabric.Services.Remoting.IService` to signal that the service has a remoting interface.</span></span>
2. <span data-ttu-id="6a37b-111">Použijte vzdálenou komunikaci naslouchací proces ve službě.</span><span class="sxs-lookup"><span data-stu-id="6a37b-111">Use a remoting listener in your service.</span></span> <span data-ttu-id="6a37b-112">Jedná se `ICommunicationListener` implementace, která poskytuje funkce vzdálené komunikace.</span><span class="sxs-lookup"><span data-stu-id="6a37b-112">This is an `ICommunicationListener` implementation that provides remoting capabilities.</span></span> <span data-ttu-id="6a37b-113">`Microsoft.ServiceFabric.Services.Remoting.Runtime` Obor názvů obsahuje metody rozšíření`CreateServiceRemotingListener` pro bezstavové a stavové služby, které lze použít k vytvoření naslouchací proces vzdálenou komunikaci pomocí protokolu přenosu výchozí vzdálenou komunikaci.</span><span class="sxs-lookup"><span data-stu-id="6a37b-113">The `Microsoft.ServiceFabric.Services.Remoting.Runtime` namespace contains an extension method,`CreateServiceRemotingListener` for both stateless and stateful services that can be used to create a remoting listener using the default remoting transport protocol.</span></span>

<span data-ttu-id="6a37b-114">Poznámka: `Remoting` obor názvů je k dispozici jako samostatné nuget balíček s názvem`Microsoft.ServiceFabric.Services.Remoting`</span><span class="sxs-lookup"><span data-stu-id="6a37b-114">Note: The `Remoting` namespace is available as a seperate nuget package called `Microsoft.ServiceFabric.Services.Remoting`</span></span> 

<span data-ttu-id="6a37b-115">Například následující bezstavové služby zpřístupní jedné metody přes vzdálené volání procedury získat "Hello World".</span><span class="sxs-lookup"><span data-stu-id="6a37b-115">For example, the following stateless service exposes a single method to get "Hello World" over a remote procedure call.</span></span>

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
> <span data-ttu-id="6a37b-116">Argumenty a návratové typy v rozhraní služby může být jakékoli jednoduchý, komplexní nebo vlastní typy, ale musí být serializovatelný pomocí .NET [DataContractSerializer](https://msdn.microsoft.com/library/ms731923.aspx).</span><span class="sxs-lookup"><span data-stu-id="6a37b-116">The arguments and the return types in the service interface can be any simple, complex, or custom types, but they must be serializable by the .NET [DataContractSerializer](https://msdn.microsoft.com/library/ms731923.aspx).</span></span>
>
>

## <a name="call-remote-service-methods"></a><span data-ttu-id="6a37b-117">Volání metody vzdálené služby</span><span class="sxs-lookup"><span data-stu-id="6a37b-117">Call remote service methods</span></span>
<span data-ttu-id="6a37b-118">Volání metod pro službu pomocí vzdálené komunikace zásobníku se provádí pomocí místní proxy server a služby pomocí `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxy` třídy.</span><span class="sxs-lookup"><span data-stu-id="6a37b-118">Calling methods on a service by using the remoting stack is done by using a local proxy to the service through the `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxy` class.</span></span> <span data-ttu-id="6a37b-119">`ServiceProxy` Metoda vytvoří místní proxy server pomocí stejné rozhraní, který implementuje službu.</span><span class="sxs-lookup"><span data-stu-id="6a37b-119">The `ServiceProxy` method creates a local proxy by using the same interface that the service implements.</span></span> <span data-ttu-id="6a37b-120">Pomocí proxy můžete jednoduše volat metody na rozhraní vzdáleně.</span><span class="sxs-lookup"><span data-stu-id="6a37b-120">With that proxy, you can simply call methods on the interface remotely.</span></span>

```csharp

IMyService helloWorldClient = ServiceProxy.Create<IMyService>(new Uri("fabric:/MyApplication/MyHelloWorldService"));

string message = await helloWorldClient.HelloWorldAsync();

```

<span data-ttu-id="6a37b-121">Vzdálená komunikace framework rozšíří výjimek vyvolaných ve službách do klienta.</span><span class="sxs-lookup"><span data-stu-id="6a37b-121">The remoting framework propagates exceptions thrown at the service to the client.</span></span> <span data-ttu-id="6a37b-122">Logika tak výjimek na straně klienta pomocí `ServiceProxy` můžete přímo zpracování výjimek, které vyvolá službu.</span><span class="sxs-lookup"><span data-stu-id="6a37b-122">So exception-handling logic at the client by using `ServiceProxy` can directly handle exceptions that the service throws.</span></span>

## <a name="service-proxy-lifetime"></a><span data-ttu-id="6a37b-123">Doby platnosti Proxy služby</span><span class="sxs-lookup"><span data-stu-id="6a37b-123">Service Proxy Lifetime</span></span>
<span data-ttu-id="6a37b-124">Vytvoření ServiceProxy je lightweight operace, takže uživatel může vytvořit tolik, jako je potřebují.</span><span class="sxs-lookup"><span data-stu-id="6a37b-124">ServiceProxy creation is a lightweight operation, so user can create as many as they need it.</span></span> <span data-ttu-id="6a37b-125">Proxy server služby lze znovu použít, dokud ho uživatel potřebovat.</span><span class="sxs-lookup"><span data-stu-id="6a37b-125">Service Proxy can be re-used as long as user need it.</span></span> <span data-ttu-id="6a37b-126">Uživatel může znovu použít stejné proxy serveru v případě výjimky.</span><span class="sxs-lookup"><span data-stu-id="6a37b-126">User can re-use the same proxy in case of Exception.</span></span> <span data-ttu-id="6a37b-127">Každý ServiceProxy obsahuje webovými servery klienta používá k odeslání zprávy prostřednictvím sítě.</span><span class="sxs-lookup"><span data-stu-id="6a37b-127">Each ServiceProxy contains communciation client used to send messages over the wire.</span></span> <span data-ttu-id="6a37b-128">Při volání rozhraní API, máme interní zkontrolujte, jestli je komunikace klienta použít platný.</span><span class="sxs-lookup"><span data-stu-id="6a37b-128">While invoking API, we have internal check to see if communication client used is valid.</span></span> <span data-ttu-id="6a37b-129">Podle toho, že výsledků, znovu vytvoříme komunikace klienta.</span><span class="sxs-lookup"><span data-stu-id="6a37b-129">Based on that result, we re-create the communication client.</span></span> <span data-ttu-id="6a37b-130">Uživatel proto není potřeba znovu vytvořit serviceproxy v případě výjimky.</span><span class="sxs-lookup"><span data-stu-id="6a37b-130">Hence user do not need to recreate serviceproxy in case of Exception.</span></span>

### <a name="serviceproxyfactory-lifetime"></a><span data-ttu-id="6a37b-131">Doba platnosti ServiceProxyFactory</span><span class="sxs-lookup"><span data-stu-id="6a37b-131">ServiceProxyFactory Lifetime</span></span>
<span data-ttu-id="6a37b-132">[ServiceProxyFactory](https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicefabric.services.remoting.client.serviceproxyfactory) je objekt factory, který vytvoří proxy server pro různé vzdálené komunikace rozhraní.</span><span class="sxs-lookup"><span data-stu-id="6a37b-132">[ServiceProxyFactory](https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicefabric.services.remoting.client.serviceproxyfactory) is a factory that creates proxy for different remoting interfaces.</span></span> <span data-ttu-id="6a37b-133">Pokud používáte ServiceProxy.Create rozhraní API pro vytváření proxy, framework vytvoří singelton ServiceProxyFactory.</span><span class="sxs-lookup"><span data-stu-id="6a37b-133">If you use API ServiceProxy.Create for creating proxy, then framework creates the singelton ServiceProxyFactory.</span></span>
<span data-ttu-id="6a37b-134">Je vhodné vytvořit jednu ručně, až budete potřebovat k přepsání [IServiceRemotingClientFactory](https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicefabric.services.remoting.client.iserviceremotingclientfactory) vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="6a37b-134">It is useful to create one manually when you need to override [IServiceRemotingClientFactory](https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicefabric.services.remoting.client.iserviceremotingclientfactory) properties.</span></span>
<span data-ttu-id="6a37b-135">Objekt Factory je náročná operace.</span><span class="sxs-lookup"><span data-stu-id="6a37b-135">Factory is an expensive operation.</span></span> <span data-ttu-id="6a37b-136">ServiceProxyFactory udržuje mezipaměť komunikace klienta.</span><span class="sxs-lookup"><span data-stu-id="6a37b-136">ServiceProxyFactory maintains cache of communication client.</span></span>
<span data-ttu-id="6a37b-137">Osvědčeným postupem je pro ukládání do mezipaměti ServiceProxyFactory jako dlouho.</span><span class="sxs-lookup"><span data-stu-id="6a37b-137">Best practice is to cache ServiceProxyFactory for as long as possible.</span></span>

## <a name="remoting-exception-handling"></a><span data-ttu-id="6a37b-138">Vzdálená komunikace zpracování výjimek</span><span class="sxs-lookup"><span data-stu-id="6a37b-138">Remoting Exception Handling</span></span>
<span data-ttu-id="6a37b-139">Všechny vzdálené výjimky vyvolané rozhraní API služby, jsou odesílány zpět do klienta v AggregateException.</span><span class="sxs-lookup"><span data-stu-id="6a37b-139">All the remote exception thrown by service API, are sent back to the client as AggregateException.</span></span> <span data-ttu-id="6a37b-140">RemoteExceptions musí být serializovatelný kontraktu jinak [ServiceException](https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicefabric.services.communication.serviceexception) je vyvolána k proxy serveru rozhraní API s Chyba serializace v ní.</span><span class="sxs-lookup"><span data-stu-id="6a37b-140">RemoteExceptions should be DataContract Serializable otherwise [ServiceException](https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicefabric.services.communication.serviceexception) is thrown to the proxy API with the serialization error in it.</span></span>

<span data-ttu-id="6a37b-141">ServiceProxy zpracovávat všechny výjimky převzetí služeb při selhání pro oddíl služby, kterou je vytvořeno.</span><span class="sxs-lookup"><span data-stu-id="6a37b-141">ServiceProxy does handle all Failover Exception for the service partition it  is created for.</span></span> <span data-ttu-id="6a37b-142">Znovu přeloží koncových bodů při převzetí služeb při selhání Exceptions(Non-Transient Exceptions) a opakuje volání s správný koncový bod.</span><span class="sxs-lookup"><span data-stu-id="6a37b-142">It re-resolves the endpoints if there is Failover Exceptions(Non-Transient Exceptions) and retries the call with the correct endpoint.</span></span> <span data-ttu-id="6a37b-143">Počet opakovaných pokusů pro převzetí služeb při selhání výjimka je neomezené.</span><span class="sxs-lookup"><span data-stu-id="6a37b-143">Number of retries for failover Exception is indefinite.</span></span>
<span data-ttu-id="6a37b-144">V případě TransientExceptions se pouze pokusí volání.</span><span class="sxs-lookup"><span data-stu-id="6a37b-144">In case of TransientExceptions, it only retries the call.</span></span>

<span data-ttu-id="6a37b-145">Výchozí parametry opakování se poskytují podle [OperationRetrySettings].</span><span class="sxs-lookup"><span data-stu-id="6a37b-145">Default retry parameters are provied by [OperationRetrySettings].</span></span> <span data-ttu-id="6a37b-146">(https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicefabric.services.communication.client.operationretrysettings) Uživatel může konfigurovat tyto hodnoty pomocí předání objektu OperationRetrySettings ServiceProxyFactory konstruktor.</span><span class="sxs-lookup"><span data-stu-id="6a37b-146">(https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicefabric.services.communication.client.operationretrysettings) User can configure these values by passing OperationRetrySettings object to ServiceProxyFactory constructor.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6a37b-147">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6a37b-147">Next steps</span></span>
* [<span data-ttu-id="6a37b-148">Webové rozhraní API s OWIN v spolehlivé služby</span><span class="sxs-lookup"><span data-stu-id="6a37b-148">Web API with OWIN in Reliable Services</span></span>](service-fabric-reliable-services-communication-webapi.md)
* [<span data-ttu-id="6a37b-149">WCF komunikaci se službami Reliable Services</span><span class="sxs-lookup"><span data-stu-id="6a37b-149">WCF communication with Reliable Services</span></span>](service-fabric-reliable-services-communication-wcf.md)
* [<span data-ttu-id="6a37b-150">Zabezpečení komunikace pro spolehlivé služby</span><span class="sxs-lookup"><span data-stu-id="6a37b-150">Securing communication for Reliable Services</span></span>](service-fabric-reliable-services-secure-communication.md)
