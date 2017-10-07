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
# <a name="service-remoting-with-reliable-services"></a>Služba vzdálené komunikace se službami Reliable Services
Pro služby, které nejsou svázané tooa konkrétní komunikační protokol nebo zásobníku, například WebAPI, Windows Communication Foundation (WCF) nebo jiných hello spolehlivé služby framework poskytuje mechanismus tooquickly vzdálenou komunikaci a snadno nastavit vzdáleného volání procedur pro služby.

## <a name="set-up-remoting-on-a-service"></a>Nastavování vzdálené komunikace na službě
Nastavení vzdálené komunikace pro služby se provádí ve dvou jednoduchých kroků:

1. Vytvořte rozhraní pro vaše tooimplement služby. Toto rozhraní definuje hello metody, které budou k dispozici pro vzdálené volání procedury vaší služby. Hello metody musí být vrácení úloh asynchronních metod. musí implementovat rozhraní Hello `Microsoft.ServiceFabric.Services.Remoting.IService` toosignal, který hello služby má vzdálené komunikace rozhraní.
2. Použijte vzdálenou komunikaci naslouchací proces ve službě. Jedná se `ICommunicationListener` implementace, která poskytuje funkce vzdálené komunikace. Hello `Microsoft.ServiceFabric.Services.Remoting.Runtime` obor názvů obsahuje metody rozšíření`CreateServiceRemotingListener` pro bezstavové a stavové služby, které můžete použít toocreate naslouchacího procesu vzdálené komunikace používat hello výchozí vzdálenou komunikaci přenosového protokolu.

Poznámka: hello `Remoting` obor názvů je k dispozici jako samostatné nuget balíček s názvem`Microsoft.ServiceFabric.Services.Remoting` 

Například hello následující bezstavové služby zpřístupní jedné metody tooget "Hello, World" přes vzdálené volání procedury.

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
> Hello argumentů a hello vrátit typy v rozhraní služby hello může být jakékoli jednoduchý, komplexní nebo vlastní typy, ale musí být serializovatelný podle hello .NET [DataContractSerializer](https://msdn.microsoft.com/library/ms731923.aspx).
>
>

## <a name="call-remote-service-methods"></a>Volání metody vzdálené služby
Volání metod pro službu pomocí hello vzdálené komunikace zásobníku se provádí pomocí služby toohello místní proxy prostřednictvím hello `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxy` třídy. Hello `ServiceProxy` metoda vytvoří místní proxy server pomocí hello implementuje stejné rozhraní, která hello služby. Pomocí proxy můžete jednoduše volat metody na rozhraní hello vzdáleně.

```csharp

IMyService helloWorldClient = ServiceProxy.Create<IMyService>(new Uri("fabric:/MyApplication/MyHelloWorldService"));

string message = await helloWorldClient.HelloWorldAsync();

```

Vzdálená komunikace framework Hello rozšíří výjimky vydané v klientovi toohello služby hello. Proto výjimek logiky na klientovi hello pomocí `ServiceProxy` můžete přímo zpracování výjimek, které hello vyvolává službu.

## <a name="service-proxy-lifetime"></a>Doby platnosti Proxy služby
Vytvoření ServiceProxy je lightweight operace, takže uživatel může vytvořit tolik, jako je potřebují. Proxy server služby lze znovu použít, dokud ho uživatel potřebovat. Uživatel může znovu použít hello stejné proxy serveru v případě výjimky. Každý ServiceProxy obsahuje webovými servery klienta použít toosend zprávy přes přenosu hello. Při volání rozhraní API, máme toosee interní kontrolu, pokud komunikace klienta použít platný. Podle toho, že výsledků, znovu vytvoříme hello komunikace klienta. Uživatel proto nemusí toorecreate serviceproxy v případě výjimky.

### <a name="serviceproxyfactory-lifetime"></a>Doba platnosti ServiceProxyFactory
[ServiceProxyFactory](https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicefabric.services.remoting.client.serviceproxyfactory) je objekt factory, který vytvoří proxy server pro různé vzdálené komunikace rozhraní. Pokud používáte ServiceProxy.Create rozhraní API pro vytváření proxy, framework vytvoří hello singelton ServiceProxyFactory.
Je užitečné toocreate jeden ručně Pokud budete potřebovat toooverride [IServiceRemotingClientFactory](https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicefabric.services.remoting.client.iserviceremotingclientfactory) vlastnosti.
Objekt Factory je náročná operace. ServiceProxyFactory udržuje mezipaměť komunikace klienta.
Osvědčeným postupem je toocache ServiceProxyFactory jako dlouho.

## <a name="remoting-exception-handling"></a>Vzdálená komunikace zpracování výjimek
Všechny hello vzdálené došlo k výjimce rozhraním API služby, jsou odesílány zpět toohello klienta v AggregateException. RemoteExceptions musí být serializovatelný kontraktu jinak [ServiceException](https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicefabric.services.communication.serviceexception) toohello proxy rozhraní API s Chyba serializace hello je vyvolána v ní.

ServiceProxy zpracovávat všechny výjimky převzetí služeb při selhání pro oddíl hello služby, kterou je vytvořeno. Znovu přeloží koncové body hello, pokud je Exceptions(Non-Transient Exceptions) převzetí služeb při selhání a opakování volání hello hello správný koncový bod. Počet opakovaných pokusů pro převzetí služeb při selhání výjimka je neomezené.
V případě TransientExceptions se pouze pokusí hello volání.

Výchozí parametry opakování se poskytují podle [OperationRetrySettings]. (https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicefabric.services.communication.client.operationretrysettings) Uživatel může konfigurovat tyto hodnoty pomocí předání OperationRetrySettings objekt tooServiceProxyFactory konstruktor.

## <a name="next-steps"></a>Další kroky
* [Webové rozhraní API s OWIN v spolehlivé služby](service-fabric-reliable-services-communication-webapi.md)
* [WCF komunikaci se službami Reliable Services](service-fabric-reliable-services-communication-wcf.md)
* [Zabezpečení komunikace pro spolehlivé služby](service-fabric-reliable-services-secure-communication.md)
