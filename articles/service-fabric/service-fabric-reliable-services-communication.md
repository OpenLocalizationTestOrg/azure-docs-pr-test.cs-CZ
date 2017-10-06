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
# <a name="how-toouse-hello-reliable-services-communication-apis"></a>Jak toouse hello spolehlivé služby komunikace rozhraní API
Azure Service Fabric jako platformu je zcela lhostejné o komunikaci mezi službami. Všechny protokoly a zásobníky jsou přijatelné, z UDP tooHTTP. Je to toohello služby vývojáře toochoose komunikace služby. Architektura aplikace na Hello spolehlivé služby poskytuje integrované komunikace balíků i rozhraní API, které můžete použít toobuild vlastní komunikační součásti.

## <a name="set-up-service-communication"></a>Nastavení komunikace služby
Hello spolehlivé rozhraní API služby používá jednoduché rozhraní pro komunikace služby. Toto rozhraní implementovat jednoduše tooopen koncový bod pro vaši službu:

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

Poté můžete přidat implementaci naslouchací proces komunikace vrácením v přepsání metody založené na službě třídy.

Pro bezstavové služby:

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

Pro stavové služby:

> [!NOTE]
> Stavová spolehlivé služby ještě nepodporuje v jazyce Java.
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

V obou případech vracet kolekci naslouchacího procesu. To umožňuje vaší služby toolisten na několik koncových bodů, potenciálně pomocí různých protokolů, s použitím více naslouchací procesy. Například můžete mít naslouchací proces protokolu HTTP a samostatné naslouchací proces protokolu WebSocket. Každý naslouchací proces získá název a kolekci výsledné hello *název: adresa* páry jsou reprezentovány jako objekt JSON, když klient požádá o hello naslouchání adresy pro instance služby nebo oddíl.

V bezstavové služby hello přepsání vrátí kolekci ServiceInstanceListeners. A `ServiceInstanceListener` obsahuje funkce toocreate `ICommunicationListener(C#) / CommunicationListener(Java)` a název. Hello přepsání pro stavové služby, vrátí kolekci ServiceReplicaListeners. To se mírně liší od jeho protějšku bezstavové, protože `ServiceReplicaListener` má tooopen možnost `ICommunicationListener` na sekundárních replikách. Nejenže můžete použít více naslouchací procesy komunikace ve službě, ale můžete také určit, který naslouchací procesy přijímat požadavky na sekundárních replikách a ty, které naslouchat pouze primární repliky.

Například můžete mít ServiceRemotingListener, která přebírá volání RPC jenom na primární repliky a druhý, vlastní naslouchací proces, který přebírá čtení požadavky na sekundárních replikách prostřednictvím protokolu HTTP:

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
> Při vytváření více naslouchací procesy pro službu, každý naslouchací proces **musí** mít jedinečný název.
>
>

Nakonec popisují hello koncové body, které jsou požadovány pro službu hello v hello [service manifest](service-fabric-application-model.md) části hello u koncových bodů.

```xml
<Resources>
    <Endpoints>
      <Endpoint Name="WebServiceEndpoint" Protocol="http" Port="80" />
      <Endpoint Name="OtherServiceEndpoint" Protocol="tcp" Port="8505" />
    <Endpoints>
</Resources>

```

naslouchací proces komunikace Hello mají přístup k prostředkům koncový bod hello přidělené tooit z hello `CodePackageActivationContext` v hello `ServiceContext`. naslouchací proces Hello potom lze spustit naslouchaly žádostem, když je otevřen.

```csharp
var codePackageActivationContext = serviceContext.CodePackageActivationContext;
var port = codePackageActivationContext.GetEndpoint("ServiceEndpoint").Port;

```
```java
CodePackageActivationContext codePackageActivationContext = serviceContext.getCodePackageActivationContext();
int port = codePackageActivationContext.getEndpoint("ServiceEndpoint").getPort();

```

> [!NOTE]
> Koncový bod prostředky jsou běžné balíček toohello celou službou, a jsou přiděleny pomocí Service Fabric, když je aktivován balíček služby hello. Víc replik služby hostované v hello může sdílet stejnou ServiceHost hello stejný port. To znamená, že naslouchací proces hello komunikace by měly podporovat sdílení portů. Hello doporučuje se způsob, jak to provést pro hello komunikaci naslouchacího procesu toouse hello oddíl ID a ID repliky nebo instanci, pokud vygeneruje adresu naslouchání hello.
>
>

### <a name="service-address-registration"></a>Registrace adresy služby
Služba system názvem hello *služby DNS* běží na clusterů Service Fabric. Hello služby DNS je doménového registrátora služeb a jejich adresy, které naslouchá na každou instanci nebo repliky služby hello. Když hello `OpenAsync(C#) / openAsync(Java)` metodu `ICommunicationListener(C#) / CommunicationListener(Java)` dokončí, vraťte se jeho hodnota získá registrované v hello Naming Service. Tato vrátí hodnotu, která se získá publikované v hello služby DNS je řetězec, jehož hodnota může být jakýkoli vůbec. Tato hodnota řetězce je, co klienti uvidí, když vyzvou pro adresu hello službu hello Naming Service.

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

Service Fabric poskytuje rozhraní API, které umožňují klienty a další služby toothen požádejte pro tuto adresu podle názvu služby. To je důležité, protože není statickou adresu služby hello. Služby přesouvání hello clusteru za účelem vyrovnávání a dostupnosti prostředků. Toto je hello mechanismus, který klientům tooresolve hello naslouchání adresu pro službu.

> [!NOTE]
> Pro kompletní návod jak toowrite naslouchací proces komunikace, najdete v části [Service Fabric webového rozhraní API služby s vlastním hostování OWIN](service-fabric-reliable-services-communication-webapi.md) C#, zatímco pro jazyk Java můžete napsat vlastní implementaci serveru HTTP, najdete v části EchoServer aplikace Příklad v https://github.com/Azure-Samples/service-fabric-java-getting-started.
>
>

## <a name="communicating-with-a-service"></a>Komunikace se službou
Hello spolehlivé rozhraní API služby poskytuje hello následující klienti toowrite knihovny, které komunikovat se službami.

### <a name="service-endpoint-resolution"></a>Rozlišení koncového bodu služby
Hello první krok toocommunication službou je tooresolve adresu koncového bodu hello oddílu nebo instance, které chcete tootalk do služby hello. Hello `ServicePartitionResolver(C#) / FabricServicePartitionResolver(Java)` nástroj třída je základní Primitivum, které pomáhají klientům určit hello koncový bod služby za běhu. V terminologii Service Fabric hello proces určení hello koncový bod služby se označují tooas hello *rozlišení koncového bodu služby*.

tooconnect tooservices v rámci clusteru, můžete vytvořit ServicePartitionResolver, pomocí výchozího nastavení. Toto je doporučená využití většině situací hello:

```csharp
ServicePartitionResolver resolver = ServicePartitionResolver.GetDefault();
```
```java
FabricServicePartitionResolver resolver = FabricServicePartitionResolver.getDefault();
```

tooconnect tooservices v jiném clusteru, ServicePartitionResolver lze vytvořit sadu koncovým bodům clusteru brány. Upozorňujeme, že jsou koncové body brány právě různými koncovými body pro připojení toohello stejného clusteru. Například:

```csharp
ServicePartitionResolver resolver = new  ServicePartitionResolver("mycluster.cloudapp.azure.com:19000", "mycluster.cloudapp.azure.com:19001");
```
```java
FabricServicePartitionResolver resolver = new  FabricServicePartitionResolver("mycluster.cloudapp.azure.com:19000", "mycluster.cloudapp.azure.com:19001");
```

Alternativně `ServicePartitionResolver` možné přidělit funkce pro vytváření `FabricClient` toouse interně:

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

`FabricClient`je hello objekt, který je použité toocommunicate se cluster Service Fabric hello pro různé operace správy v clusteru hello. To je užitečné, pokud chcete mít větší kontrolu nad interakci překladač oddílu služby se váš cluster. `FabricClient`provede ukládání do mezipaměti interně a je obvykle levnější toocreate, takže je důležité tooreuse `FabricClient` instance co nejvíc.

```csharp
ServicePartitionResolver resolver = new  ServicePartitionResolver(() => CreateMyFabricClient());
```
```java
FabricServicePartitionResolver resolver = new  FabricServicePartitionResolver(() -> new CreateFabricClientImpl());
```

Metoda vyřešte je pak použít tooretrieve hello adresu služby nebo služby u oddílů služby.

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

Adresa služby lze vyřešit pomocí snadno ServicePartitionResolver, ale je nutná další práci tooensure hello přeložit adresu je možné použít správně. Váš klient musí toodetect, zda se nezdařilo z důvodu přechodná chyba umožňující opakovaný pokus o připojení hello a můžete zkusit znovu (například služba přesunut nebo je dočasně nedostupná), nebo trvalé chybě (např. Služba je Odstraněná nebo hello požadovaný prostředek již existuje). Instance služby nebo repliky můžete přesunout z uzlu toonode kdykoli z několika důvodů. Adresa služby Hello přeložit prostřednictvím ServicePartitionResolver může být váš klientský kód pokusí tooconnect doby hello zastaralé. V takovém případě znovu hello klient potřebuje vyřešit toore hello adresu. Poskytnutí hello předchozí `ResolvedServicePartition` označuje, která znovu hello překladač potřebám tootry nikoli jednoduše načíst adresu v mezipaměti.

Obvykle kód klienta hello nemusí pracovat s hello ServicePartitionResolver přímo. Je vytvořen a předán na objekty Factory klienta toocommunication v hello spolehlivé Services API. objekty Factory Hello používají hello překladač interně toogenerate klientského objektu, který lze použít toocommunicate službou.

### <a name="communication-clients-and-factories"></a>Komunikace klientů a objekty pro vytváření
Knihovna vytváření komunikace Hello implementuje typický vzor opakování selhání zpracování, které usnadňuje koncovým bodům služby tooresolved Probíhá opakování připojení. Knihovna vytváření Hello poskytuje mechanismus opakování hello zatímco poskytují hello chyba obslužné rutiny.

`ICommunicationClientFactory(C#) / CommunicationClientFactory(Java)`definuje základní rozhraní hello implementované objekt factory komunikaci klienta, který vytváří klientů, které může kontaktovat službu tooa Service Fabric. Hello implementace hello CommunicationClientFactory závisí na používá služba hello Service Fabric, kde hello klienta chce toocommunicate hello komunikačního balíku. poskytuje technologie Hello spolehlivé rozhraní API služby `CommunicationClientFactoryBase<TCommunicationClient>`. To poskytuje základní implementaci rozhraní CommunicationClientFactory hello a provádí úlohy, které jsou společné tooall hello komunikace zásobníky. (Tyto úlohy patří pomocí koncového bodu služby ServicePartitionResolver toodetermine hello). Klienti obvykle implementovat hello abstraktní CommunicationClientFactoryBase třída toohandle logiku, která je konkrétní toohello komunikačního balíku.

Hello komunikace klienta pouze obdrží adresu a používá je tooconnect tooa služby. Hello klienta můžete použít libovolnou protokol ho chce.

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

Dodavatel klienta Hello především zodpovídá za vytvoření komunikace klientů. Pro klienty, kteří nejsou udržovat trvalé připojení, jako je například klientovi HTTP musí objekt pro vytváření hello pouze toocreate a návratové hello klienta. Jiné protokoly, které udržují trvalé připojení, jako je například některé binární protokoly, by také být ověřen hello factory toodetermine zda hello připojení musí toobe znovu vytvořit.  

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

Nakonec obslužné rutiny výjimky je zodpovědná za určení jaké akce tootake, když dojde k výjimce. Výjimky jsou rozdělené do **opakovatelné** a **neopakovatelného**.

* **Neopakovatelného** výjimky jednoduše získat znovu vyvolány back toohello volajícího.
* **Opakovatelná** výjimky se dále dělí do **přechodný** a **-pouze dočasné**.
  * **Přechodný** výjimky jsou ty, které můžete jednoduše opakovat bez znovu řešení hello adresa koncového bodu služby. Patří přechodné problémy se sítí nebo služby chybové odpovědi kromě těch, které označují hello adresa koncového bodu služby neexistuje.
  * **Bez přechodná** výjimky jsou ty, které vyžadují hello služby koncový bod adresy toobe znovu přeložit. Mezi ně patří výjimky, které označují, že koncový bod služby hello není dostupný, což značí, že služba hello přesunul tooa jiný uzel.

Hello `TryHandleException` provádí rozhodnutí o dané výjimka. Pokud ho **neví** co toomake rozhodnutí o výjimku, by měla vrátit **false**. Pokud ho **vědět** co toomake rozhodnutí, měl by odpovídajícím způsobem nastavit hello výsledek a vrátí **true**.

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
### <a name="putting-it-all-together"></a>Třeba umisťovat všechny společně
S `ICommunicationClient(C#) / CommunicationClient(Java)`, `ICommunicationClientFactory(C#) / CommunicationClientFactory(Java)`, a `IExceptionHandler(C#) / ExceptionHandler(Java)` vytvořených na základě komunikační protokol, `ServicePartitionClient(C#) / FabricServicePartitionClient(Java)` všechny společně se zabalí a poskytuje odolnost zpracování hello a služby oddílu adresu řešení smyčky kolem těchto součástí.

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

## <a name="next-steps"></a>Další kroky
* Zobrazit příklad komunikaci pomocí protokolu HTTP mezi službami v [ukázkový projekt C# na Githubu](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Services/WordCount) nebo [Java ukázkový projekt na Githubu](https://github.com/Azure-Samples/service-fabric-java-getting-started/tree/master/Services/WatchDog).
* [Volání vzdálených procedur s vzdálenou komunikaci spolehlivé služby](service-fabric-reliable-services-communication-remoting.md)
* [Webové rozhraní API, která používá OWIN v spolehlivé služby](service-fabric-reliable-services-communication-webapi.md)
* [Komunikace WCF pomocí spolehlivé služby](service-fabric-reliable-services-communication-wcf.md)
