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
# <a name="wcf-based-communication-stack-for-reliable-services"></a><span data-ttu-id="3a5af-103">Komunikace na základě WCF zásobníku pro spolehlivé služby</span><span class="sxs-lookup"><span data-stu-id="3a5af-103">WCF-based communication stack for Reliable Services</span></span>
<span data-ttu-id="3a5af-104">služby Hello spolehlivé Services framework umožňuje autorům toochoose hello komunikačního balíku chtějí toouse pro jejich službu.</span><span class="sxs-lookup"><span data-stu-id="3a5af-104">hello Reliable Services framework allows service authors toochoose hello communication stack that they want toouse for their service.</span></span> <span data-ttu-id="3a5af-105">Můžete zařadit hello komunikačního balíku libovolný prostřednictvím hello **ICommunicationListener** vrácená z hello [CreateServiceReplicaListeners nebo CreateServiceInstanceListeners](service-fabric-reliable-services-communication.md) metody.</span><span class="sxs-lookup"><span data-stu-id="3a5af-105">They can plug in hello communication stack of their choice via hello **ICommunicationListener** returned from hello [CreateServiceReplicaListeners or CreateServiceInstanceListeners](service-fabric-reliable-services-communication.md) methods.</span></span> <span data-ttu-id="3a5af-106">Hello framework poskytuje implementaci hello komunikačního balíku podle hello Windows Communication Foundation (WCF) pro autory služby, kteří chtějí toouse komunikace na základě WCF.</span><span class="sxs-lookup"><span data-stu-id="3a5af-106">hello framework provides an implementation of hello communication stack based on hello Windows Communication Foundation (WCF) for service authors who want toouse WCF-based communication.</span></span>

## <a name="wcf-communication-listener"></a><span data-ttu-id="3a5af-107">Naslouchací proces komunikace WCF</span><span class="sxs-lookup"><span data-stu-id="3a5af-107">WCF Communication Listener</span></span>
<span data-ttu-id="3a5af-108">implementace Hello WCF specifické **ICommunicationListener** zajišťuje hello **Microsoft.ServiceFabric.Services.Communication.Wcf.Runtime.WcfCommunicationListener** třídy.</span><span class="sxs-lookup"><span data-stu-id="3a5af-108">hello WCF-specific implementation of **ICommunicationListener** is provided by hello **Microsoft.ServiceFabric.Services.Communication.Wcf.Runtime.WcfCommunicationListener** class.</span></span>

<span data-ttu-id="3a5af-109">Přidejte vyslovte máme kontraktu služby typu`ICalculator`</span><span class="sxs-lookup"><span data-stu-id="3a5af-109">Lest say we have a service contract of type `ICalculator`</span></span>

```csharp
[ServiceContract]
public interface ICalculator
{
    [OperationContract]
    Task<int> Add(int value1, int value2);
}
```

<span data-ttu-id="3a5af-110">Můžeme vytvořit naslouchací proces komunikace WCF v hello služby hello následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="3a5af-110">We can create a WCF communication listener in hello service hello following manner.</span></span>

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

## <a name="writing-clients-for-hello-wcf-communication-stack"></a><span data-ttu-id="3a5af-111">Zápis klientů pro hello WCF komunikačního balíku</span><span class="sxs-lookup"><span data-stu-id="3a5af-111">Writing clients for hello WCF communication stack</span></span>
<span data-ttu-id="3a5af-112">Pro zápis klientů toocommunicate službou pomocí WCF, hello framework poskytuje **WcfClientCommunicationFactory**, což je hello WCF konkrétní implementace [ClientCommunicationFactoryBase](service-fabric-reliable-services-communication.md).</span><span class="sxs-lookup"><span data-stu-id="3a5af-112">For writing clients toocommunicate with services by using WCF, hello framework provides **WcfClientCommunicationFactory**, which is hello WCF-specific implementation of [ClientCommunicationFactoryBase](service-fabric-reliable-services-communication.md).</span></span>

```csharp

public WcfCommunicationClientFactory(
    Binding clientBinding = null,
    IEnumerable<IExceptionHandler> exceptionHandlers = null,
    IServicePartitionResolver servicePartitionResolver = null,
    string traceId = null,
    object callback = null);
```

<span data-ttu-id="3a5af-113">Hello WCF komunikační kanál je přístupná z hello **WcfCommunicationClient** vytvořené hello **WcfCommunicationClientFactory**.</span><span class="sxs-lookup"><span data-stu-id="3a5af-113">hello WCF communication channel can be accessed from hello **WcfCommunicationClient** created by hello **WcfCommunicationClientFactory**.</span></span>

```csharp

public class WcfCommunicationClient : ServicePartitionClient<WcfCommunicationClient<ICalculator>>
   {
       public WcfCommunicationClient(ICommunicationClientFactory<WcfCommunicationClient<ICalculator>> communicationClientFactory, Uri serviceUri, ServicePartitionKey partitionKey = null, TargetReplicaSelector targetReplicaSelector = TargetReplicaSelector.Default, string listenerName = null, OperationRetrySettings retrySettings = null)
           : base(communicationClientFactory, serviceUri, partitionKey, targetReplicaSelector, listenerName, retrySettings)
       {
       }
   }

```

<span data-ttu-id="3a5af-114">Kód klienta můžete použít hello **WcfCommunicationClientFactory** společně s hello **WcfCommunicationClient** které implementuje **ServicePartitionClient** toodetermine Hello koncový bod služby a komunikovat se službou hello.</span><span class="sxs-lookup"><span data-stu-id="3a5af-114">Client code can use hello **WcfCommunicationClientFactory** along with hello **WcfCommunicationClient** which implements **ServicePartitionClient** toodetermine hello service endpoint and communicate with hello service.</span></span>

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
> <span data-ttu-id="3a5af-115">Výchozí Hello ServicePartitionResolver předpokládá, že tento klient hello běží ve stejném clusteru jako služba hello.</span><span class="sxs-lookup"><span data-stu-id="3a5af-115">hello default ServicePartitionResolver assumes that hello client is running in same cluster as hello service.</span></span> <span data-ttu-id="3a5af-116">Pokud tedy není hello případě vytvořit objekt ServicePartitionResolver a předat hello koncové body připojení clusteru.</span><span class="sxs-lookup"><span data-stu-id="3a5af-116">If that is not hello case, create a ServicePartitionResolver object and pass in hello cluster connection endpoints.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="3a5af-117">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3a5af-117">Next steps</span></span>
* [<span data-ttu-id="3a5af-118">Vzdálené volání procedur u vzdálené komunikace spolehlivé služby</span><span class="sxs-lookup"><span data-stu-id="3a5af-118">Remote procedure call with Reliable Services remoting</span></span>](service-fabric-reliable-services-communication-remoting.md)
* [<span data-ttu-id="3a5af-119">Webové rozhraní API s OWIN v spolehlivé služby</span><span class="sxs-lookup"><span data-stu-id="3a5af-119">Web API with OWIN in Reliable Services</span></span>](service-fabric-reliable-services-communication-webapi.md)
* [<span data-ttu-id="3a5af-120">Zabezpečení komunikace pro spolehlivé služby</span><span class="sxs-lookup"><span data-stu-id="3a5af-120">Securing communication for Reliable Services</span></span>](service-fabric-reliable-services-secure-communication.md)

