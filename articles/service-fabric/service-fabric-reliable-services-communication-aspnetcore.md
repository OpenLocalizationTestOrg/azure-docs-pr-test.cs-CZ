---
title: aaaService komunikace s hello ASP.NET Core | Microsoft Docs
description: "Zjistěte, jak toouse základní technologie ASP.NET v bezzstavovými i stavovými službami spolehlivé."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 8aa4668d-cbb6-4225-bd2d-ab5925a868f2
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 05/02/2017
ms.author: vturecek
ms.openlocfilehash: 6e6a83ab04390150292f63de5d9b51d290284e50
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="aspnet-core-in-service-fabric-reliable-services"></a>Jádro ASP.NET v Service Fabric spolehlivé služby

ASP.NET Core je nové open source a napříč platformami architektura pro vytváření moderní cloudové aplikace založené na připojené k Internetu, jako třeba webové aplikace, aplikace IoT a back-EndY mobilních. 

Tento článek slouží podrobný průvodce toohosting ASP.NET Core služby v Service Fabric spolehlivé služby pomocí hello **Microsoft.ServiceFabric.AspNetCore.** * sadu balíčků NuGet.

Úvodní kurz ASP.NET Core v Service Fabric a pokyny pro nastavení vývojového prostředí najdete v tématu [vytváření webového front-endu vaší aplikace pomocí ASP.NET Core](service-fabric-add-a-web-frontend.md).

Hello zbývající části tohoto článku předpokládá, že jste již obeznámeni s ASP.NET Core. Pokud ne, doporučujeme čtení prostřednictvím hello [ASP.NET Core Základy](https://docs.microsoft.com/aspnet/core/fundamentals/index).

## <a name="aspnet-core-in-hello-service-fabric-environment"></a>ASP.NET Core v prostředí Service Fabric hello

Když aplikace ASP.NET Core můžete spustit na .NET Core nebo hello úplné rozhraní .NET Framework, Service Fabric služby aktuálně lze spustit jen u hello úplné rozhraní .NET Framework. To znamená, když vytváříte služby ASP.NET Core Service Fabric, je potřeba stále cílit hello úplné rozhraní .NET Framework.

ASP.NET Core lze použít dvěma různými způsoby v Service Fabric:
 - **Hostovaný jako spustitelný soubor hosta**. Toto je primárně používané toorun stávající ASP.NET Core aplikace v Service Fabric beze změn kódu.
 - **Spustit uvnitř spolehlivě**. To umožňuje lepší integraci s hello modulu runtime Service Fabric a umožňuje stavová ASP.NET Core services.

Hello zbytek Tento článek vysvětluje, jak toouse ASP.NET Core uvnitř spolehlivou službu pomocí hello komponenty integrace ASP.NET Core, které jsou součástí s hello Service Fabric SDK. 

## <a name="service-fabric-service-hosting"></a>Hostování služeb Service Fabric

V Service Fabric, jeden nebo více instancí a/nebo repliky služby běžet v *procesu hostitele služby*, spustitelný soubor, který spouští službu kódu. Jako autor služby vlastníte hello služby hostitelský proces a Service Fabric aktivuje a sleduje za vás.

Tradiční ASP.NET (až tooMVC 5) je úzce párované tooIIS prostřednictvím System.Web.dll. ASP.NET Core zajišťuje oddělení mezi hello webového serveru a webové aplikace. To umožňuje webové aplikace toobe přenosné mezi jiné webové servery a také umožňuje webové servery toobe *hostovanou na vlastním*, což znamená, že můžete spustit webový server v vlastní procesu, jako názvem na rozdíl od tooa proces, který je vlastněn vyhrazený webový software serveru jako je například IIS. 

V pořadí toocombine služby Service Fabric a ASP.NET, jako spustitelný soubor hosta nebo v spolehlivě, musí být schopný toostart ASP.NET uvnitř vaší služby hostitelský proces. Vlastní hostování ASP.NET Core vám umožní toodo to.

## <a name="hosting-aspnet-core-in-a-reliable-service"></a>Hostování ASP.NET Core v spolehlivě.
Obvykle vlastním hostováním aplikací ASP.NET Core vytvořte tomuto webovému hostiteli v vstupní bod aplikace, jako je například hello `static void Main()` metoda v `Program.cs`. Životní cyklus hello hello webového hostitele v tomto případě je vázané toohello životního cyklu hello procesu.

![Hostování v procesu ASP.NET Core][0]

Však vstupní bod aplikace hello není hello správném místě toocreate webového hostitele v spolehlivě, protože se používá pouze vstupní bod aplikace hello tooregister služby zadejte pomocí modulu runtime Service Fabric hello, tak, aby ji může vytvořit instance této služby typ. Hello webového hostitele má být vytvořena v spolehlivě sám sebe. V rámci procesu hostitele služby hello instance služby nebo repliky můžete projít více životní cykly. 

Instance spolehlivé služby je reprezentována třídě služby odvozování z `StatelessService` nebo `StatefulService`. je součástí Hello komunikačního balíku pro službu `ICommunicationListener` implementace ve třídě služby. Hello `Microsoft.ServiceFabric.Services.AspNetCore.*` balíčky NuGet obsahovat implementace `ICommunicationListener` , spouštět a spravovat hello ASP.NET Core tomuto webovému hostiteli pro Kestrel nebo WebListener v spolehlivě.

![Hostování ASP.NET Core v spolehlivě.][1]

## <a name="aspnet-core-icommunicationlisteners"></a>ICommunicationListeners ASP.NET Core
Hello `ICommunicationListener` implementace pro Kestrel a WebListener v hello `Microsoft.ServiceFabric.Services.AspNetCore.*` balíčky NuGet podobné použití vzorce ale provádět mírně odlišné akce konkrétní tooeach webový server. 

Obě naslouchací procesy komunikace poskytují konstruktor, který přebírá hello následující argumenty:
 - **`ServiceContext serviceContext`**: hello `ServiceContext` objekt, který obsahuje informace o hello službou.
 - **`string endpointName`**: název hello `Endpoint` konfigurace v ServiceManifest.xml. Toto je primárně kde hello dva naslouchací procesy komunikace liší: WebListener **vyžaduje** `Endpoint` konfigurace, zatímco Kestrel neexistuje.
 - **`Func<string, AspNetCoreCommunicationListener, IWebHost> build`**: lambda, který implementujete, ve kterém se vytváří a zpět `IWebHost`. To vám umožní tooconfigure `IWebHost` hello způsob za normálních okolností byste v aplikaci ASP.NET Core. Hello lambda poskytuje adresy URL, která je generována pro v závislosti na integraci Service Fabric hello možnosti, můžete použít a hello `Endpoint` konfigurace, které zadáte. Aby adresa URL lze potom změnit nebo použít jako-je toostart hello webovým serverem.

## <a name="service-fabric-integration-middleware"></a>Middleware integrace Service Fabric
Hello `Microsoft.ServiceFabric.Services.AspNetCore` balíček NuGet obsahuje hello `UseServiceFabricIntegration` rozšiřující metody na `IWebHostBuilder` , přidá middleware podporující služby prostředků infrastruktury. Tento middleware nakonfiguruje hello Kestrel nebo WebListener `ICommunicationListener` tooregister s adresou URL jedinečné služby hello Service Fabric Naming Service a poté ověří klienta požadavky tooensure klienti se připojují toohello správné služby. To je nezbytné v prostředí s sdílené hostitele, jako jsou Service Fabric, kde můžete spustit na hello stejný fyzický nebo virtuální počítač více webových aplikací, ale nepoužívejte názvy hostitelů jedinečný, klienti tooprevent omylem připojení toohello nesprávnou službu. Tento scénář je podrobně popsaná v další v další části hello.

### <a name="a-case-of-mistaken-identity"></a>V případě chybné identifikace
Repliky služby, bez ohledu na protokol, naslouchat na kombinaci jedinečné IP: port. Jakmile repliku služby bylo zahájeno naslouchání na koncový bod IP: port, hlásí adresu toohello tento koncový bod prostředků infrastruktury pojmenování služby kde mohou být zjištěny klienti nebo jiné služby. Pokud služby používají, které porty dynamicky přiřadit aplikaci, repliku služby může použít shodou hello stejný koncový bod IP: port jiné službě, která byla předtím na hello stejný fyzický nebo virtuální počítač. To může způsobit klienta toomistakely připojit toohello nesprávnou službu. K tomu může dojít, pokud dojde k hello následující posloupnost událostí:

 1. Služby A naslouchá na 10.0.0.1:30000 přes protokol HTTP. 
 2. Klienta přeloží A služby a získá adresu 10.0.0.1:30000
 3. Služby A přesune tooa jiný uzel.
 4. Služba B je umístěn na 10.0.0.1 a shodou hello používá stejný port 30000.
 5. Klient se pokusí o tooconnect tooservice A s 10.0.0.1:30000 adres v mezipaměti.
 6. Klient je nyní připojen úspěšně připojeno tooservice B není porozumění ho toohello nesprávnou službu.

To může způsobit chyby v náhodných intervalech, které může být obtížné toodiagnose. 

### <a name="using-unique-service-urls"></a>Pomocí adresy URL jedinečné služby
tooprevent služby, můžete odeslat koncový bod toohello Naming Service s jedinečným identifikátorem a její ověření tento jedinečný identifikátor během požadavky klientů. Toto je akce spolupráci mezi službami v případě důvěryhodného prostředí nepřátelských klienta. To neposkytuje zabezpečené služby ověřování v prostředí s nepřátelských klienta.

V případě důvěryhodného prostředí hello middleware, který přidává hello `UseServiceFabricIntegration` metoda automaticky připojí adresu toohello jedinečný identifikátor, která je odeslána toohello zásady vytváření názvů služby a ověří, že identifikátor na každý požadavek. Pokud hello identifikátor neodpovídá, hello middleware okamžitě vrátí odpověď HTTP 410 zmizel.

Služby využívající port dynamicky přiřadit měli používat tento middlewaru.

Služby, které používají pevné jedinečné číslo portu není nutné tento problém v prostředí spolupráci. Pevné jedinečné číslo portu se obvykle používá pro externě směřujících služby, které pro klientské aplikace tooconnect k potřebovat dobře známého portu. Například většina internetových webových aplikací používat port 80 nebo 443 pro webové prohlížeče připojení. V takovém případě by nemělo být povoleno hello jedinečný identifikátor.

Hello následující diagram znázorňuje tok požadavku hello s hello middleware povoleno:

![Integrace služby Fabric ASP.NET Core][2]

Kestrel a WebListener `ICommunicationListener` implementace použít tento mechanismus v přesně hello stejným způsobem. I když WebListener může odlišovat interně požadavků podle jedinečné cesty adresy URL pomocí hello základní *http.sys* funkce, která je funkce Sdílení portů *není* používané hello WebListener `ICommunicationListener` implementace vzhledem k tomu, které způsobí HTTP 503 a HTTP 404 chybové kódy stavu v hello scénář popsaný výše. Naopak pak bude velmi obtížné pro klienty toodetermine hello záměr hello chyby, jako HTTP 503 a HTTP 404, se již běžně používané tooindicate dalších chyb. Proto Kestrel i WebListener `ICommunicationListener` implementace standardizovat na hello middleware poskytované hello `UseServiceFabricIntegration` metoda rozšíření tak, aby klienti, stačí tooperform koncového bodu služby znovu vyřešit akce HTTP 410 odpovědi.

## <a name="weblistener-in-reliable-services"></a>WebListener v spolehlivé služby
WebListener mohou být používány spolehlivě importováním hello **Microsoft.ServiceFabric.AspNetCore.WebListener** balíček NuGet. Tento balíček obsahuje `WebListenerCommunicationListener`, implementace `ICommunicationListener`, což vám umožní toocreate ASP.NET Core tomuto webovému hostiteli uvnitř spolehlivě pomocí WebListener jako hello webový server.

WebListener je založený na hello [rozhraní API systému Windows HTTP serveru](https://msdn.microsoft.com/library/windows/desktop/aa364510(v=vs.85).aspx). Tato služba využívá hello *http.sys* ovladač jádra, které používá IIS tooprocess HTTP požadavků a směrovat je tooprocesses spuštění webové aplikace. Díky tomu mohou více procesů v hello stejný fyzický nebo virtuální počítač toohost webové aplikace na hello stejný port, od sebe jednoznačně rozlišeny jedinečná cesta adresy URL nebo název hostitele. Tyto funkce jsou užitečné v Service Fabric pro hostování více webů v hello stejného clusteru.

Hello následující diagram znázorňuje, jak WebListener používá hello *http.sys* ovladač jádra v systému Windows pro sdílení portů:

![ovladač HTTP.sys][3]

### <a name="weblistener-in-a-stateless-service"></a>WebListener v bezstavové služby
toouse `WebListener` v bezstavové služby přepsat hello `CreateServiceInstanceListeners` metoda a vraťte se `WebListenerCommunicationListener` instance:

```csharp
protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    return new ServiceInstanceListener[]
    {
        new ServiceInstanceListener(serviceContext =>
            new WebListenerCommunicationListener(serviceContext, "ServiceEndpoint", (url, listener) =>
                new WebHostBuilder()
                    .UseWebListener()
                    .ConfigureServices(
                        services => services
                            .AddSingleton<StatelessServiceContext>(serviceContext))
                    .UseContentRoot(Directory.GetCurrentDirectory())
                    .UseServiceFabricIntegration(listener, ServiceFabricIntegrationOptions.None)
                    .UseStartup<Startup>()
                    .UseUrls(url)
                    .Build()))
    };
}
```

### <a name="weblistener-in-a-stateful-service"></a>WebListener v stavové služby

`WebListenerCommunicationListener`není aktuálně určený pro použití v stavové služby kvůli toocomplications s hello základní *http.sys* funkce Sdílení portů. Další informace najdete v následující části na dynamický port přidělení s WebListener hello. Pro stavové služby je Kestrel hello doporučená webový server.

### <a name="endpoint-configuration"></a>Konfigurace koncového bodu

`Endpoint` Konfigurace je nutná pro webové servery, které používají Windows HTTP serveru rozhraní API, včetně WebListener hello. Webové servery, které používají hello rozhraní API systému Windows HTTP serveru, musíte nejprve rezervovat jejich adresu URL s *http.sys* (to se obvykle dosahuje hello [netsh](https://msdn.microsoft.com/library/windows/desktop/cc307236(v=vs.85).aspx) nástroj). Tato akce vyžaduje zvýšená oprávnění, které nemají vašim službám ve výchozím nastavení. Hello "http" nebo "https" možnosti hello `Protocol` vlastnost hello `Endpoint` konfigurace v *ServiceManifest.xml* používají konkrétně tooinstruct hello tooregister modulu runtime Service Fabric adresu URL s *http.sys* vaším jménem pomocí hello [ *silné zástupné* ](https://msdn.microsoft.com/library/windows/desktop/aa364698(v=vs.85).aspx) předponu adresy URL.

Například tooreserve `http://+:80` pro služby, hello následující konfigurace je třeba použít v ServiceManifest.xml:

```xml
<ServiceManifest ... >
    ...
    <Resources>
        <Endpoints>
            <Endpoint Name="ServiceEndpoint" Protocol="http" Port="80" />
        </Endpoints>
    </Resources>

</ServiceManifest>
```

A hello název koncového bodu musí být předán toohello `WebListenerCommunicationListener` konstruktor:

```csharp
 new WebListenerCommunicationListener(serviceContext, "ServiceEndpoint", (url, listener) =>
 {
     return new WebHostBuilder()
         .UseWebListener()
         .UseServiceFabricIntegration(listener, ServiceFabricIntegrationOptions.None)
         .UseUrls(url)
         .Build();
 })
```

#### <a name="use-weblistener-with-a-static-port"></a>WebListener pomocí statického portu
toouse statický port s WebListener, zadejte číslo portu hello v hello `Endpoint` konfigurace:

```xml
  <Resources>
    <Endpoints>
      <Endpoint Protocol="http" Name="ServiceEndpoint" Port="80" />
    </Endpoints>
  </Resources>
```

#### <a name="use-weblistener-with-a-dynamic-port"></a>Pomocí WebListener dynamický port
toouse dynamicky přiřazeného portu s WebListener, vynechejte hello `Port` vlastnost hello `Endpoint` konfigurace:

```xml
  <Resources>
    <Endpoints>
      <Endpoint Protocol="http" Name="ServiceEndpoint" />
    </Endpoints>
  </Resources>
```

Všimněte si, že dynamický port přidělena `Endpoint` konfigurace poskytuje jenom jeden port *za hostitelský proces*. Hello aktuální Service Fabric model hostování umožňuje více služby instancí a/nebo repliky toobe hostované v hello stejný proces, což znamená, že každé z nich budou sdílet stejný port při přidělení prostřednictvím hello hello `Endpoint` konfigurace. Více instancí WebListener můžete sdílet port pomocí hello základní *http.sys* nepodporuje sdílení funkce, ale které portů `WebListenerCommunicationListener` kvůli toohello komplikace přináší pro požadavky klientů. Pro používání dynamických portů je Kestrel hello doporučená webový server.

## <a name="kestrel-in-reliable-services"></a>Kestrel v spolehlivé služby
Kestrel mohou být používány spolehlivě importováním hello **Microsoft.ServiceFabric.AspNetCore.Kestrel** balíček NuGet. Tento balíček obsahuje `KestrelCommunicationListener`, implementace `ICommunicationListener`, což vám umožní toocreate ASP.NET Core tomuto webovému hostiteli uvnitř spolehlivě pomocí Kestrel jako hello webový server.

Kestrel je že napříč platformami webový server pro ASP.NET Core podle libuv, knihovny a platformy asynchronní vstupně-výstupní operace. Na rozdíl od WebListener, Kestrel nepoužívá manažera centralizované koncový bod, jako *http.sys*. A na rozdíl od WebListener, Kestrel nepodporuje sdílení portů mezi více procesy. Každá instance Kestrel musíte použít jedinečnou port.

![kestrel][4]

### <a name="kestrel-in-a-stateless-service"></a>Kestrel v bezstavové služby
toouse `Kestrel` v bezstavové služby přepsat hello `CreateServiceInstanceListeners` metoda a vraťte se `KestrelCommunicationListener` instance:

```csharp
protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    return new ServiceInstanceListener[]
    {
        new ServiceInstanceListener(serviceContext =>
            new KestrelCommunicationListener(serviceContext, "ServiceEndpoint", (url, listener) =>
                new WebHostBuilder()
                    .UseKestrel()
                    .ConfigureServices(
                        services => services
                            .AddSingleton<StatelessServiceContext>(serviceContext))
                    .UseContentRoot(Directory.GetCurrentDirectory())
                    .UseServiceFabricIntegration(listener, ServiceFabricIntegrationOptions.UseUniqueServiceUrl)
                    .UseStartup<Startup>()
                    .UseUrls(url)
                    .Build();
            ))
    };
}
```

### <a name="kestrel-in-a-stateful-service"></a>Kestrel v stavové služby
toouse `Kestrel` v stavové služby přepsat hello `CreateServiceReplicaListeners` metoda a vraťte se `KestrelCommunicationListener` instance:

```csharp
protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    return new ServiceReplicaListener[]
    {
        new ServiceReplicaListener(serviceContext =>
            new KestrelCommunicationListener(serviceContext, (url, listener) =>
                new WebHostBuilder()
                    .UseKestrel()
                    .ConfigureServices(
                         services => services
                             .AddSingleton<StatefulServiceContext>(serviceContext)
                             .AddSingleton<IReliableStateManager>(this.StateManager))
                    .UseContentRoot(Directory.GetCurrentDirectory())
                    .UseServiceFabricIntegration(listener, ServiceFabricIntegrationOptions.UseUniqueServiceUrl)
                    .UseStartup<Startup>()
                    .UseUrls(url)
                    .Build();
            ))
    };
}
```

V tomto příkladu instanci typu singleton daného `IReliableStateManager` je k dispozici kontejneru pro vkládání závislosti toohello tomuto webovému hostiteli. To není nezbytně nutné, ale umožňuje toouse `IReliableStateManager` a spolehlivé kolekcí ve vaší metody akce kontroleru MVC.

Všimněte si, že `Endpoint` se název konfigurace **není** zadali příliš`KestrelCommunicationListener` v stavové služby. To je vysvětleno v následující části hello podrobněji.

### <a name="endpoint-configuration"></a>Konfigurace koncového bodu
`Endpoint` Konfigurace není požadovaná toouse Kestrel. 

Kestrel je jednoduchý samostatný webový server; na rozdíl od WebListener (nebo HttpListener), není nutné `Endpoint` konfigurace v *ServiceManifest.xml* protože nevyžaduje předchozí toostarting adresa URL registrace. 

#### <a name="use-kestrel-with-a-static-port"></a>Kestrel pomocí statického portu
Statický port se dá nakonfigurovat v hello `Endpoint` konfigurace ServiceManifest.xml pro použití s Kestrel. I když to není nezbytně nutné, poskytuje dvě potenciální výhody:
 1. Pokud hello port nespadá rozsah portů aplikace hello, protože je otevřen přes bránu firewall hello operačního systému pomocí Service Fabric.
 2. Zadaná adresa URL tooyou prostřednictvím Hello `KestrelCommunicationListener` bude tento port použít.

```xml
  <Resources>
    <Endpoints>
      <Endpoint Protocol="http" Name="ServiceEndpoint" Port="80" />
    </Endpoints>
  </Resources>
```

Pokud `Endpoint` je nakonfigurován, jeho název, musí být předán do hello `KestrelCommunicationListener` konstruktor: 

```csharp
new KestrelCommunicationListener(serviceContext, "ServiceEndpoint", (url, listener) => ...
```

Pokud `Endpoint` není použita konfigurace, vynechejte hello název v hello `KestrelCommunicationListener` konstruktor. V takovém případě se použije dynamický port. Viz další část hello Další informace.

#### <a name="use-kestrel-with-a-dynamic-port"></a>Pomocí Kestrel dynamický port
Kestrel nemůžete použít automatické port přiřazení hello z hello `Endpoint` konfigurace v ServiceManifest.xml, protože port automatické přiřazení z `Endpoint` konfigurace přiřadí jedinečné port na *hostitelském procesu* , a jeden hostitelský proces může obsahovat několik instancí Kestrel. Vzhledem k tomu, že Kestrel nepodporuje sdílení portů, to nebude fungovat jako každá instance Kestrel musí být otevřen na portu jedinečné.

přiřazení dynamický port toouse s Kestrel, jednoduše vynechejte hello `Endpoint` konfigurace v ServiceManifest.xml zcela a nepředávejte toohello název koncového bodu `KestrelCommunicationListener` konstruktor:

```csharp
new KestrelCommunicationListener(serviceContext, (url, listener) => ...
```

V této konfiguraci `KestrelCommunicationListener` automaticky vybere nepoužitého portu z rozsahu portů aplikace hello.

## <a name="scenarios-and-configurations"></a>Scénáře a konfigurace
Tato část popisuje hello následující scénáře a poskytuje hello doporučuje kombinace webového serveru, konfiguraci portů, možností integrace Service Fabric a ostatní nastavení tooachieve správně funguje služby:
 - Externě zveřejněné bezstavové služby ASP.NET Core
 - Pouze interní ASP.NET Core bezstavové služby
 - Pouze interní ASP.NET Core stavové služby

**Externě zveřejněné** služba je ten, který zpřístupní koncový bod dosažitelný z mimo cluster hello, obvykle pomocí služby Vyrovnávání zatížení.

**Jen interní** služby je jedním jejichž koncový bod je pouze dosažitelný z v rámci clusteru hello.

> [!NOTE]
> Stavové služby, které by neměly být obecně koncové body zveřejněné toohello Internetu. Clustery, které jsou za nástroje pro vyrovnávání zatížení, které neberou v řešení služby Service Fabric, jako je například hello Vyrovnávání zatížení Azure, budou stavové služby nelze tooexpose, protože nástroj pro vyrovnávání zatížení hello nebude možné toolocate a směrovat provoz toohello replika odpovídající stavové služby. 

### <a name="externally-exposed-aspnet-core-stateless-services"></a>Externě zveřejněné bezstavové služby ASP.NET Core
WebListener je doporučeno webového serveru pro front-endové služby, které zveřejňují externí, internetových koncových bodů protokolu HTTP na Windows hello. Poskytuje lepší ochranu proti útokům a podporuje funkce, které Kestrel neexistuje, jako jsou ověřování systému Windows a sdílení portů. 

V současné době není podporovaný kestrel jako server edge (internetový). Musí použít reverzní proxy server, například služby IIS nebo Nginx hello toohandle provoz z veřejného Internetu.
 
Kdy použít zveřejněné toohello Internetu, bezstavové služby dobře známé a stabilní koncový bod, který je dostupný prostřednictvím Vyrovnávání zatížení. Toto je adresa URL hello bude poskytovat toousers vaší aplikace. doporučuje se Hello následující konfigurace:

|  |  | **Poznámky k** |
| --- | --- | --- |
| Webový server | WebListener | Pokud je služba hello pouze zveřejněné tooa důvěryhodné síti, takové intranetu, může být použit Kestrel. Jinak WebListener je možnost hello upřednostňovaný. |
| Konfigurace portu | Statické | Dobře známé statický port by měl být nakonfigurovaný v hello `Endpoints` konfigurace ServiceManifest.xml, jako třeba 80 pro protokol HTTP nebo 443 pro protokol HTTPS. |
| ServiceFabricIntegrationOptions | Žádný | Hello `ServiceFabricIntegrationOptions.None` by měl být použit při konfiguraci Service Fabric integrace middleware tak, aby služba hello nepokusí toovalidate příchozích požadavků na jedinečný identifikátor. Externí uživatele vaší aplikace nebude vědět hello jedinečné identifikační informace používané hello middleware. |
| Počet instancí | -1 | V typické případy použití, musí být počet instancí hello nastavení nastavená příliš "-1", aby instance k dispozici na všech uzlech, jež přijímat přenosy z pro vyrovnávání zatížení. |

Pokud více externě zveřejněné služeb sdílet hello stejná sada uzlů, použije jedinečné, ale stabilní cestu adresy URL. To můžete udělat změnou zadaná při konfiguraci IWebHost adresa URL hello. Všimněte si, že to se týká pouze tooWebListener.

 ```csharp
 new WebListenerCommunicationListener(serviceContext, "ServiceEndpoint", (url, listener) =>
 {
     url += "/MyUniqueServicePath";
 
     return new WebHostBuilder()
         .UseWebListener()
         ...
         .UseUrls(url)
         .Build();
 })
 ```

### <a name="internal-only-stateless-aspnet-core-service"></a>Pouze interní bezstavové služby ASP.NET Core
Bezstavové služby, které jsou pouze volat v rámci clusteru hello by měl použít jedinečné adresy URL a dynamicky přiřadit porty tooensure spolupráci mezi více služeb. doporučuje se Hello následující konfigurace:

|  |  | **Poznámky k** |
| --- | --- | --- |
| Webový server | kestrel | I když WebListener mohou být použity pro interní bezstavové služby, je Kestrel hello doporučená tooallow serveru více tooshare instancí služby hostitele.  |
| Konfigurace portu | dynamicky přiřadit | Víc replik stavové služby může sdílet proces hostitele nebo hostitelského operačního systému a proto bude nutné odlišné porty. |
| ServiceFabricIntegrationOptions | UseUniqueServiceUrl | S přiřazením dynamický port toto nastavení zabrání hello zaměněny identity popsané výše. |
| InstanceCount | všechny | počet instancí Hello nastavení můžete nastavit tooany hodnota nezbytné toooperate hello služby. |

### <a name="internal-only-stateful-aspnet-core-service"></a>Pouze interní stavová služba ASP.NET Core
Stavové služby, které jsou pouze volat v rámci clusteru hello měli používat porty dynamicky přiřazené tooensure spolupráci mezi více služeb. doporučuje se Hello následující konfigurace:

|  |  | **Poznámky k** |
| --- | --- | --- |
| Webový server | kestrel | Hello `WebListenerCommunicationListener` není určen pro stavové služby, ve kterých repliky sdílet hostitelském procesu. |
| Konfigurace portu | dynamicky přiřadit | Víc replik stavové služby může sdílet proces hostitele nebo hostitelského operačního systému a proto bude nutné odlišné porty. |
| ServiceFabricIntegrationOptions | UseUniqueServiceUrl | S přiřazením dynamický port toto nastavení zabrání hello zaměněny identity popsané výše. |

## <a name="next-steps"></a>Další kroky
[Ladění aplikace Service Fabric pomocí sady Visual Studio](service-fabric-debugging-your-application.md)

<!--Image references-->
[0]:./media/service-fabric-reliable-services-communication-aspnetcore/webhost-standalone.png
[1]:./media/service-fabric-reliable-services-communication-aspnetcore/webhost-servicefabric.png
[2]:./media/service-fabric-reliable-services-communication-aspnetcore/integration.png
[3]:./media/service-fabric-reliable-services-communication-aspnetcore/httpsys.png
[4]:./media/service-fabric-reliable-services-communication-aspnetcore/kestrel.png
