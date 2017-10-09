---
title: "Zásobník komunikace služby WCF aaaReliable | Microsoft Docs"
description: "Hello předdefinované WCF komunikačního balíku v Service Fabric poskytuje služba klienta WCF komunikaci pro spolehlivé služby."
services: service-fabric
documentationcenter: .net
author: BharatNarasimman
manager: timlt
editor: vturecek
ms.assetid: 75516e1e-ee57-4bc7-95fe-71ec42d452b2
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 06/07/2017
ms.author: bharatn
ms.openlocfilehash: 7feebef4d46a6ae66d05129f47f9b5911e82aec9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="wcf-based-communication-stack-for-reliable-services"></a>Komunikace na základě WCF zásobníku pro spolehlivé služby
služby Hello spolehlivé Services framework umožňuje autorům toochoose hello komunikačního balíku chtějí toouse pro jejich službu. Můžete zařadit hello komunikačního balíku libovolný prostřednictvím hello **ICommunicationListener** vrácená z hello [CreateServiceReplicaListeners nebo CreateServiceInstanceListeners](service-fabric-reliable-services-communication.md) metody. Hello framework poskytuje implementaci hello komunikačního balíku podle hello Windows Communication Foundation (WCF) pro autory služby, kteří chtějí toouse komunikace na základě WCF.

## <a name="wcf-communication-listener"></a>Naslouchací proces komunikace WCF
implementace Hello WCF specifické **ICommunicationListener** zajišťuje hello **Microsoft.ServiceFabric.Services.Communication.Wcf.Runtime.WcfCommunicationListener** třídy.

Přidejte vyslovte máme kontraktu služby typu`ICalculator`

```csharp
[ServiceContract]
public interface ICalculator
{
    [OperationContract]
    Task<int> Add(int value1, int value2);
}
```

Můžeme vytvořit naslouchací proces komunikace WCF v hello služby hello následujícím způsobem.

```csharp

protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    return new[] { new ServiceReplicaListener((context) =>
        new WcfCommunicationListener<ICalculator>(
            wcfServiceObject:this,
            serviceContext:context,
            //
            // hello name of hello endpoint configured in hello ServiceManifest under hello Endpoints section
            // that identifies hello endpoint that hello WCF ServiceHost should listen on.
            //
            endpointResourceName: "WcfServiceEndpoint",

            //
            // Populate hello binding information that you want hello service toouse.
            //
            listenerBinding: WcfUtility.CreateTcpListenerBinding()
        )
    )};
}

```

## <a name="writing-clients-for-hello-wcf-communication-stack"></a>Zápis klientů pro hello WCF komunikačního balíku
Pro zápis klientů toocommunicate službou pomocí WCF, hello framework poskytuje **WcfClientCommunicationFactory**, což je hello WCF konkrétní implementace [ClientCommunicationFactoryBase](service-fabric-reliable-services-communication.md).

```csharp

public WcfCommunicationClientFactory(
    Binding clientBinding = null,
    IEnumerable<IExceptionHandler> exceptionHandlers = null,
    IServicePartitionResolver servicePartitionResolver = null,
    string traceId = null,
    object callback = null);
```

Hello WCF komunikační kanál je přístupná z hello **WcfCommunicationClient** vytvořené hello **WcfCommunicationClientFactory**.

```csharp

public class WcfCommunicationClient : ServicePartitionClient<WcfCommunicationClient<ICalculator>>
   {
       public WcfCommunicationClient(ICommunicationClientFactory<WcfCommunicationClient<ICalculator>> communicationClientFactory, Uri serviceUri, ServicePartitionKey partitionKey = null, TargetReplicaSelector targetReplicaSelector = TargetReplicaSelector.Default, string listenerName = null, OperationRetrySettings retrySettings = null)
           : base(communicationClientFactory, serviceUri, partitionKey, targetReplicaSelector, listenerName, retrySettings)
       {
       }
   }

```

Kód klienta můžete použít hello **WcfCommunicationClientFactory** společně s hello **WcfCommunicationClient** které implementuje **ServicePartitionClient** toodetermine Hello koncový bod služby a komunikovat se službou hello.

```csharp
// Create binding
Binding binding = WcfUtility.CreateTcpClientBinding();
// Create a partition resolver
IServicePartitionResolver partitionResolver = ServicePartitionResolver.GetDefault();
// create a  WcfCommunicationClientFactory object.
var wcfClientFactory = new WcfCommunicationClientFactory<ICalculator>
    (clientBinding: binding, servicePartitionResolver: partitionResolver);

//
// Create a client for communicating with hello ICalculator service that has been created with the
// Singleton partition scheme.
//
var calculatorServiceCommunicationClient =  new WcfCommunicationClient(
                wcfClientFactory,
                ServiceUri,
                ServicePartitionKey.Singleton);

//
// Call hello service tooperform hello operation.
//
var result = calculatorServiceCommunicationClient.InvokeWithRetryAsync(
                client => client.Channel.Add(2, 3)).Result;

```
> [!NOTE]
> Výchozí Hello ServicePartitionResolver předpokládá, že tento klient hello běží ve stejném clusteru jako služba hello. Pokud tedy není hello případě vytvořit objekt ServicePartitionResolver a předat hello koncové body připojení clusteru.
> 
> 

## <a name="next-steps"></a>Další kroky
* [Vzdálené volání procedur u vzdálené komunikace spolehlivé služby](service-fabric-reliable-services-communication-remoting.md)
* [Webové rozhraní API s OWIN v spolehlivé služby](service-fabric-reliable-services-communication-webapi.md)
* [Zabezpečení komunikace pro spolehlivé služby](service-fabric-reliable-services-secure-communication.md)

