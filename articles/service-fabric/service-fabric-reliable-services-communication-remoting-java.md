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
# <a name="service-remoting-with-reliable-services"></a>Služba vzdálené komunikace se službami Reliable Services
> [!div class="op_single_selector"]
> * [C# v systému Windows](service-fabric-reliable-services-communication-remoting.md)
> * [Java v Linuxu](service-fabric-reliable-services-communication-remoting-java.md)
>
>

Hello spolehlivé služby framework poskytuje mechanismus tooquickly vzdálenou komunikaci a snadno nastavit vzdáleného volání procedur pro služby.

## <a name="set-up-remoting-on-a-service"></a>Nastavování vzdálené komunikace na službě
Nastavení vzdálené komunikace pro služby se provádí ve dvou jednoduchých kroků:

1. Vytvořte rozhraní pro vaše tooimplement služby. Toto rozhraní definuje hello metody, které jsou k dispozici pro vzdálené volání procedury vaší služby. Hello metody musí být vrácení úloh asynchronních metod. musí implementovat rozhraní Hello `microsoft.serviceFabric.services.remoting.Service` toosignal, který hello služby má vzdálené komunikace rozhraní.
2. Použijte vzdálenou komunikaci naslouchací proces ve službě. Jedná se `CommunicationListener` implementace, která poskytuje funkce vzdálené komunikace. `FabricTransportServiceRemotingListener`lze použít toocreate naslouchací proces vzdálenou komunikaci pomocí protokolu přenosu vzdálenou komunikaci výchozí hello.

Například hello následující bezstavové služby zpřístupní jedné metody tooget "Hello, World" přes vzdálené volání procedury.

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
> argumenty Hello a hello vrátí typy v rozhraní služby hello může být jakékoli jednoduchý, komplexní nebo vlastní typy, ale musí být serializovatelný.
>
>

## <a name="call-remote-service-methods"></a>Volání metody vzdálené služby
Volání metod pro službu pomocí hello vzdálené komunikace zásobníku se provádí pomocí služby toohello místní proxy prostřednictvím hello `microsoft.serviceFabric.services.remoting.client.ServiceProxyBase` třídy. Hello `ServiceProxyBase` metoda vytvoří místní proxy server pomocí hello implementuje stejné rozhraní, která hello služby. Pomocí proxy můžete jednoduše volat metody na rozhraní hello vzdáleně.

```java

MyService helloWorldClient = ServiceProxyBase.create(MyService.class, new URI("fabric:/MyApplication/MyHelloWorldService"));

CompletableFuture<String> message = helloWorldClient.helloWorldAsync();

```

Vzdálená komunikace framework Hello rozšíří výjimky vydané v klientovi toohello služby hello. Proto výjimek logiky na klientovi hello pomocí `ServiceProxyBase` můžete přímo zpracování výjimek, které hello vyvolává službu.

## <a name="service-proxy-lifetime"></a>Doby platnosti Proxy služby
Vytvoření ServiceProxy je lightweight operace, takže uživatel může vytvořit tolik, jako je potřebují. Proxy server služby lze znovu použít, dokud ho uživatel potřebovat. Uživatel může znovu použít hello stejné proxy serveru v případě výjimky. Každý ServiceProxy obsahuje zprávy toosend klient používá komunikaci přes přenosu hello. Při volání rozhraní API, máme toosee interní kontrolu, pokud komunikace klienta použít platný. Podle toho, že výsledků, znovu vytvoříme hello komunikace klienta. Uživatel proto nemusí toorecreate serviceproxy v případě výjimky.

### <a name="serviceproxyfactory-lifetime"></a>Doba platnosti ServiceProxyFactory
[FabricServiceProxyFactory](https://docs.microsoft.com/en-us/java/api/microsoft.servicefabric.services.remoting.client._fabric_service_proxy_factory) je objekt factory, který vytvoří proxy server pro různé vzdálené komunikace rozhraní. Pokud používáte rozhraní API `ServiceProxyBase.create` k vytváření proxy serveru, pak framework vytvoří `FabricServiceProxyFactory`.
Je užitečné toocreate jeden ručně Pokud budete potřebovat toooverride [ServiceRemotingClientFactory](https://docs.microsoft.com/en-us/java/api/microsoft.servicefabric.services.remoting.client._service_remoting_client_factory) vlastnosti.
Objekt Factory je náročná operace. `FabricServiceProxyFactory`udržuje mezipaměť komunikace klientů.
Osvědčeným postupem je toocache `FabricServiceProxyFactory` jako dlouho.

## <a name="remoting-exception-handling"></a>Vzdálená komunikace zpracování výjimek
Všechny hello vzdálené došlo k výjimce rozhraním API služby, jsou odesílány zpět toohello klienta jako runtimeexception – nebo FabricException.

ServiceProxy zpracovávat všechny výjimky převzetí služeb při selhání pro oddíl hello služby, kterou je vytvořeno. Znovu přeloží koncové body hello, pokud je Exceptions(Non-Transient Exceptions) převzetí služeb při selhání a opakování volání hello hello správný koncový bod. Počet opakovaných pokusů pro převzetí služeb při selhání výjimka je neomezené.
V případě TransientExceptions se pouze pokusí hello volání.

Výchozí parametry opakování se poskytují podle [OperationRetrySettings]. (https://docs.microsoft.com/en-us/java/api/microsoft.servicefabric.services.communication.client._operation_retry_settings) Uživatel může konfigurovat tyto hodnoty pomocí předání OperationRetrySettings objekt tooServiceProxyFactory konstruktor.

## <a name="next-steps"></a>Další kroky
* [Zabezpečení komunikace pro spolehlivé služby](service-fabric-reliable-services-secure-communication.md)
