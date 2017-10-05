---
title: "Vzdálená komunikace služby v Service Fabric | Microsoft Docs"
description: "Vzdálená komunikace Service Fabric umožňuje služby a klienti komunikovat se službami s použitím vzdálené volání procedury."
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
# <a name="service-remoting-with-reliable-services"></a>Služba vzdálené komunikace se službami Reliable Services
Pro služby, které nejsou svázané s konkrétní komunikační protokol nebo zásobníku, například WebAPI, Windows Communication Foundation (WCF) nebo jiné spolehlivé služby framework poskytuje mechanismus vzdálenou komunikaci rychle a snadno nastavit vzdáleného volání procedur pro služby.

## <a name="set-up-remoting-on-a-service"></a>Nastavování vzdálené komunikace na službě
Nastavení vzdálené komunikace pro služby se provádí ve dvou jednoduchých kroků:

1. Vytvořte rozhraní služby k implementaci. Toto rozhraní definuje metody, které budou k dispozici pro vzdálené volání procedury vaší služby. Metody musí být vrácení úloh asynchronních metod. Musí implementovat rozhraní `Microsoft.ServiceFabric.Services.Remoting.IService` signál, že služba má vzdálené komunikace rozhraní.
2. Použijte vzdálenou komunikaci naslouchací proces ve službě. Jedná se `ICommunicationListener` implementace, která poskytuje funkce vzdálené komunikace. `Microsoft.ServiceFabric.Services.Remoting.Runtime` Obor názvů obsahuje metody rozšíření`CreateServiceRemotingListener` pro bezstavové a stavové služby, které lze použít k vytvoření naslouchací proces vzdálenou komunikaci pomocí protokolu přenosu výchozí vzdálenou komunikaci.

Poznámka: `Remoting` obor názvů je k dispozici jako samostatné nuget balíček s názvem`Microsoft.ServiceFabric.Services.Remoting` 

Například následující bezstavové služby zpřístupní jedné metody přes vzdálené volání procedury získat "Hello World".

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
> Argumenty a návratové typy v rozhraní služby může být jakékoli jednoduchý, komplexní nebo vlastní typy, ale musí být serializovatelný pomocí .NET [DataContractSerializer](https://msdn.microsoft.com/library/ms731923.aspx).
>
>

## <a name="call-remote-service-methods"></a>Volání metody vzdálené služby
Volání metod pro službu pomocí vzdálené komunikace zásobníku se provádí pomocí místní proxy server a služby pomocí `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxy` třídy. `ServiceProxy` Metoda vytvoří místní proxy server pomocí stejné rozhraní, který implementuje službu. Pomocí proxy můžete jednoduše volat metody na rozhraní vzdáleně.

```csharp

IMyService helloWorldClient = ServiceProxy.Create<IMyService>(new Uri("fabric:/MyApplication/MyHelloWorldService"));

string message = await helloWorldClient.HelloWorldAsync();

```

Vzdálená komunikace framework rozšíří výjimek vyvolaných ve službách do klienta. Logika tak výjimek na straně klienta pomocí `ServiceProxy` můžete přímo zpracování výjimek, které vyvolá službu.

## <a name="service-proxy-lifetime"></a>Doby platnosti Proxy služby
Vytvoření ServiceProxy je lightweight operace, takže uživatel může vytvořit tolik, jako je potřebují. Proxy server služby lze znovu použít, dokud ho uživatel potřebovat. Uživatel může znovu použít stejné proxy serveru v případě výjimky. Každý ServiceProxy obsahuje webovými servery klienta používá k odeslání zprávy prostřednictvím sítě. Při volání rozhraní API, máme interní zkontrolujte, jestli je komunikace klienta použít platný. Podle toho, že výsledků, znovu vytvoříme komunikace klienta. Uživatel proto není potřeba znovu vytvořit serviceproxy v případě výjimky.

### <a name="serviceproxyfactory-lifetime"></a>Doba platnosti ServiceProxyFactory
[ServiceProxyFactory](https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicefabric.services.remoting.client.serviceproxyfactory) je objekt factory, který vytvoří proxy server pro různé vzdálené komunikace rozhraní. Pokud používáte ServiceProxy.Create rozhraní API pro vytváření proxy, framework vytvoří singelton ServiceProxyFactory.
Je vhodné vytvořit jednu ručně, až budete potřebovat k přepsání [IServiceRemotingClientFactory](https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicefabric.services.remoting.client.iserviceremotingclientfactory) vlastnosti.
Objekt Factory je náročná operace. ServiceProxyFactory udržuje mezipaměť komunikace klienta.
Osvědčeným postupem je pro ukládání do mezipaměti ServiceProxyFactory jako dlouho.

## <a name="remoting-exception-handling"></a>Vzdálená komunikace zpracování výjimek
Všechny vzdálené výjimky vyvolané rozhraní API služby, jsou odesílány zpět do klienta v AggregateException. RemoteExceptions musí být serializovatelný kontraktu jinak [ServiceException](https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicefabric.services.communication.serviceexception) je vyvolána k proxy serveru rozhraní API s Chyba serializace v ní.

ServiceProxy zpracovávat všechny výjimky převzetí služeb při selhání pro oddíl služby, kterou je vytvořeno. Znovu přeloží koncových bodů při převzetí služeb při selhání Exceptions(Non-Transient Exceptions) a opakuje volání s správný koncový bod. Počet opakovaných pokusů pro převzetí služeb při selhání výjimka je neomezené.
V případě TransientExceptions se pouze pokusí volání.

Výchozí parametry opakování se poskytují podle [OperationRetrySettings]. (https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicefabric.services.communication.client.operationretrysettings) Uživatel může konfigurovat tyto hodnoty pomocí předání objektu OperationRetrySettings ServiceProxyFactory konstruktor.

## <a name="next-steps"></a>Další kroky
* [Webové rozhraní API s OWIN v spolehlivé služby](service-fabric-reliable-services-communication-webapi.md)
* [WCF komunikaci se službami Reliable Services](service-fabric-reliable-services-communication-wcf.md)
* [Zabezpečení komunikace pro spolehlivé služby](service-fabric-reliable-services-secure-communication.md)
