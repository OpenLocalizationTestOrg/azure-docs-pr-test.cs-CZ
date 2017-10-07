---
title: "aaaService komunikace s hello rozhraní ASP.NET Web API | Microsoft Docs"
description: "Zjistěte, jak hello komunikace služby tooimplement podrobné pomocí rozhraní ASP.NET Web API s OWIN samoobslužné hostování v hello spolehlivé Services API."
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
ms.date: 02/10/2017
ms.author: vturecek
redirect_url: /azure/service-fabric/service-fabric-reliable-services-communication-aspnetcore
ms.openlocfilehash: 3fb18fcb141ada0d79a0acda3dccbc7fb044346d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-service-fabric-web-api-services-with-owin-self-hosting"></a>Začínáme: Service Fabric webového rozhraní API služby s vlastním hostování OWIN
Azure Service Fabric přináší hello výkon v ruce při se rozhodování, jak chcete vaši toocommunicate služby s uživateli a mezi sebou. Tento kurz se zaměřuje na implementaci komunikace služby pomocí rozhraní ASP.NET Web API Open Web Interface pro .NET (OWIN) samoobslužné hostování v Service Fabric spolehlivé Services API. Jsme budete pustíte hluboko do hello spolehlivé služby modulární komunikace rozhraní API. Také použijeme webového rozhraní API v tooshow podrobný příklad můžete jak tooset si vlastní komunikace naslouchací proces.

## <a name="introduction-tooweb-api-in-service-fabric"></a>Úvod tooWeb rozhraní API v Service Fabric
Rozhraní ASP.NET Web API je Oblíbené a výkonné rozhraní pro vytváření rozhraní API HTTP nad hello rozhraní .NET Framework. Pokud už nejste obeznámeni s hello framework, přečtěte si téma [Začínáme s ASP.NET Web API 2](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) toolearn Další.

Webové rozhraní API v Service Fabric je hello stejné rozhraní ASP.NET Web API znáte a rádi. Hello rozdíl je, jak můžete *hostitele* aplikace webového rozhraní API. Nebudete používat Microsoft Internetové informační služby (IIS). toobetter pochopit rozdíl hello, můžeme rozdělit na dvě části:

1. aplikace Hello webového rozhraní API (včetně řadiče a modely)
2. Hello hostitele (hello webový server, obvykle IIS)

Vlastní aplikace webového rozhraní API se nezmění. Se neliší od aplikací webového rozhraní API, které máte napsaný v posledních hello a musí být schopný toosimply přesunutí přes většinu kódu aplikace. Pokud jste si ve službě IIS, kde hostovat aplikace hello hostování, ale může být jen málo liší od co jste slouží jako. Předtím, než získáme toohello hostování část, Začněme s něco další známé: hello aplikace webového rozhraní API.

## <a name="create-hello-application"></a>Vytvoření aplikace hello
Začněte vytvořením nové aplikace Service Fabric pomocí jednoho bezstavové služby v sadě Visual Studio 2015.

Šablony sady Visual Studio pro bezstavové služby pomocí rozhraní Web API je k dispozici tooyou. V tomto kurzu využijeme projekt webového rozhraní API od začátku, jejímž výsledkem co byste získali jste vybrali této šablony.

Vyberte prázdné toolearn projektu bezstavové služby, jak můžete toobuild projekt webového rozhraní API od začátku, nebo můžete začít s hello bezstavové služby šablony webového rozhraní API a jednoduše podle nich zorientujete.  

prvním krokem Hello je toopull v některé balíčky NuGet pro webové rozhraní API. balíček Hello chceme toouse je Microsoft.AspNet.WebApi.OwinSelfHost. Tento balíček obsahuje všechny potřebné balíčky webového rozhraní API hello a hello *hostitele* balíčky. To bude důležité později.

Po dokončení instalace hello balíčků, můžete začít vytváření hello základní strukturu projekt webového rozhraní API. Pokud jste použili webového rozhraní API, struktura projektu hello velmi povědomé. Začněte přidáním `Controllers` directory a řadič jednoduché hodnoty:

**ValuesController.cs**

```csharp
using System.Collections.Generic;
using System.Web.Http;

namespace WebService.Controllers
{
    public class ValuesController : ApiController
    {
        // GET api/values 
        public IEnumerable<string> Get()
        {
            return new string[] { "value1", "value2" };
        }

        // GET api/values/5 
        public string Get(int id)
        {
            return "value";
        }

        // POST api/values 
        public void Post([FromBody]string value)
        {
        }

        // PUT api/values/5 
        public void Put(int id, [FromBody]string value)
        {
        }

        // DELETE api/values/5 
        public void Delete(int id)
        {
        }
    }
}

```

V dalším kroku přidejte třídu spuštění na hello projektu kořenové tooregister hello směrování, formátování a dalších nastavení konfigurace. Toto je také kde webového rozhraní API připojí toohello *hostitele*, který bude kdykoli znovu spustit později. 

**Startup.cs**

```csharp
using System.Web.Http;
using Owin;

namespace WebService
{
    public static class Startup
    {
        public static void ConfigureApp(IAppBuilder appBuilder)
        {
            // Configure Web API for self-host. 
            HttpConfiguration config = new HttpConfiguration();

            config.Routes.MapHttpRoute(
                name: "DefaultApi",
                routeTemplate: "api/{controller}/{id}",
                defaults: new { id = RouteParameter.Optional }
            );

            appBuilder.UseWebApi(config);
        }
    }
}
```

Je to pro část aplikace hello. V tomto okamžiku nastavili jsme právě hello základní webového rozhraní API rozložení projektu. Pokud by neměl vypadat výrazně liší od projekty webového rozhraní API, že jste vytvořili v minulosti hello nebo z šablony webového rozhraní API základní hello. Obchodní logika je třeba do hello řadiče a modely jako obvykle.

Nyní co můžeme udělat o hostování tak, aby jeho ve skutečnosti spuštěním?

## <a name="service-hosting"></a>Služby hostování
V Service Fabric služby běží v *procesu hostitele služby*, spustitelný soubor, který spouští službu kódu. Při zápisu služby pomocí hello spolehlivé rozhraní API služby projektu služby právě zkompiluje tooan spustitelný soubor, který zaregistruje typ vaší služby a spustí váš kód. To platí ve většině případů při zápisu služby v Service Fabric v rozhraní .NET. Při otevření souboru Program.cs v projektu hello bezstavové služby, byste měli vidět:

```csharp
using System;
using System.Diagnostics;
using System.Threading;
using Microsoft.ServiceFabric.Services.Runtime;

internal static class Program
{
    private static void Main()
    {
        try
        {
            ServiceRuntime.RegisterServiceAsync("WebServiceType",
                context => new WebService(context)).GetAwaiter().GetResult();

            ServiceEventSource.Current.ServiceTypeRegistered(Process.GetCurrentProcess().Id, typeof(WebService).Name);

            // Prevents this host process from terminating so services keeps running. 
            Thread.Sleep(Timeout.Infinite);
        }
        catch (Exception e)
        {
            ServiceEventSource.Current.ServiceHostInitializationFailed(e.ToString());
            throw;
        }
    }
}

```

Pokud toto vypadá nápadně jako hello vstupní bod tooa konzolovou aplikaci, která je, protože je.

Další podrobnosti o hello proces hostitele služby a registraci služby jsou nad rámec tohoto článku hello. Je důležité tooknow pro, ale teď, když *kódu služby běží ve svém vlastním procesu*.

## <a name="self-host-web-api-with-an-owin-host"></a>Hostování na vlastním serveru webového rozhraní API s hostitelem OWIN
Vzhledem k tomu, že je kód aplikace webového rozhraní API hostované ve svém vlastním procesu, jak můžete spojit se tooa webový server? Zadejte [OWIN](http://owin.org/). OWIN je jednoduše smlouva mezi webových aplikací .NET a webovými servery. Tradičně při ASP.NET (až tooMVC 5) se používá, hello webové aplikace je úzce párované tooIIS prostřednictvím System.Web. Však webového rozhraní API implementuje OWIN, takže píšete webovou aplikaci, která odpojené z hello webový server, který je hostitelem. Z toho důvodu můžete použít *hostovanou na vlastním* OWIN webový server, který můžete spustit v vlastního procesu. To odpovídá perfektně s Service Fabric hello hostování se model, který jsme právě popsané.

V tomto článku použijeme Katana jako hello OWIN hostitele pro hello aplikace webového rozhraní API. Katana je implementace hostitele OWIN open source založený na [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx) a hello Windows [rozhraní API serveru HTTP](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).

> [!NOTE]
> Další informace o Katana, přejděte toohello toolearn [Katana lokality](http://www.asp.net/aspnet/overview/owin-and-katana/an-overview-of-project-katana). Rychlý přehled o toouse Katana tooself hostitele webového rozhraní API, najdete v části [OWIN použití tooSelf hostitele ASP.NET Web API 2](http://www.asp.net/web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api).
> 
> 

## <a name="set-up-hello-web-server"></a>Nastavit webový server hello
Hello spolehlivé rozhraní API služby poskytuje ke vstupnímu bodu komunikace, kde můžete zařadit zásobníky komunikace, které umožňují uživatelům a klienti tooconnect toohello služby:

```csharp

protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    ...
}

```

Hello webový server (a další komunikačního balíku, které můžete použít v budoucí, jako je například technologie WebSockets hello) by měl použít hello ICommunicationListener rozhraní toointegrate správně s hello systémem. důvody Hello začnou více zřejmá v hello následující kroky.

Nejdřív vytvořte třídu s názvem OwinCommunicationListener, který implementuje ICommunicationListener:

**OwinCommunicationListener.cs**

```csharp
using Microsoft.Owin.Hosting;
using Microsoft.ServiceFabric.Services.Communication.Runtime;
using Owin;
using System;
using System.Fabric;
using System.Globalization;
using System.Threading;
using System.Threading.Tasks;

namespace WebService
{
    internal class OwinCommunicationListener : ICommunicationListener
    {
        public void Abort()
        {
        }

        public Task CloseAsync(CancellationToken cancellationToken)
        {
        }

        public Task<string> OpenAsync(CancellationToken cancellationToken)
        {
        }
    }
}
```

rozhraní ICommunicationListener Hello poskytuje tři metody toomanage naslouchací proces komunikace služby:

* *OpenAsync*. Zahájit naslouchání požadavkům.
* *CloseAsync*. Zastavil naslouchání požadavkům, dokončit všechny během letu žádosti a korektně vypnout.
* *Abort –*. Všechno, co zrušte a zastavte okamžitě.

Soukromá třída členy tooget spustili, přidat, pro naslouchací proces hello věcí bude potřebovat toofunction. Tyto se inicializovat pomocí konstruktoru hello a později použije při nastavování hello naslouchání adresy URL.

```csharp
internal class OwinCommunicationListener : ICommunicationListener
{
    private readonly ServiceEventSource eventSource;
    private readonly Action<IAppBuilder> startup;
    private readonly ServiceContext serviceContext;
    private readonly string endpointName;
    private readonly string appRoot;

    private IDisposable webApp;
    private string publishAddress;
    private string listeningAddress;

    public OwinCommunicationListener(Action<IAppBuilder> startup, ServiceContext serviceContext, ServiceEventSource eventSource, string endpointName)
        : this(startup, serviceContext, eventSource, endpointName, null)
    {
    }

    public OwinCommunicationListener(Action<IAppBuilder> startup, ServiceContext serviceContext, ServiceEventSource eventSource, string endpointName, string appRoot)
    {
        if (startup == null)
        {
            throw new ArgumentNullException(nameof(startup));
        }

        if (serviceContext == null)
        {
            throw new ArgumentNullException(nameof(serviceContext));
        }

        if (endpointName == null)
        {
            throw new ArgumentNullException(nameof(endpointName));
        }

        if (eventSource == null)
        {
            throw new ArgumentNullException(nameof(eventSource));
        }

        this.startup = startup;
        this.serviceContext = serviceContext;
        this.endpointName = endpointName;
        this.eventSource = eventSource;
        this.appRoot = appRoot;
    }


    ...

```

## <a name="implement-openasync"></a>Implementace OpenAsync
tooset hello webový server, budete potřebovat dva údaje:

* *Předponu adresy URL cesta*. I když tato položka je nepovinná, je vhodné pro tooset můžete tento až teď tak, aby ve vaší aplikaci můžete bezpečně hostovat několik webových služeb.
* *Port*.

Předtím, než získáte port pro hello webový server, je důležité pochopit, že Service Fabric poskytuje aplikační vrstvu, která funguje jako vyrovnávací paměť mezi aplikací a hello základní operační systém, který běží na. Jako takový Service Fabric nabízí způsob tooconfigure *koncové body* pro vaše služby. Service Fabric zajišťuje, aby byly k dispozici pro vaše služba toouse koncové body. Tímto způsobem, že nemáte tooconfigure je sami v hello základní prostředí operačního systému. Snadno můžete hostovat aplikace Service Fabric v různých prostředích bez nutnosti toomake aplikace tooyour žádné změny. (Například je možné hostovat hello stejnou aplikaci v Azure nebo ve svém vlastním datovém centru.)

Konfigurace koncového bodu protokolu HTTP v PackageRoot\ServiceManifest.xml:

```xml
<Resources>
    <Endpoints>
        <Endpoint Name="ServiceEndpoint" Type="Input" Protocol="http" Port="8281" />
    </Endpoints>
</Resources>

```

Tento krok je důležité, protože proces hostitele služby hello spouští pověřeními s omezeným přístupem (síťové služby v systému Windows). To znamená, že služby nebudete mít přístup tooset až koncový bod HTTP svoje vlastní. Service Fabric pomocí konfigurace koncového bodu hello zná tooset hello řádného přístup ovládací prvek seznamu (ACL) pro hello, který bude naslouchat adresa URL, která hello služby. Service Fabric také poskytuje standardní místo tooconfigure koncové body.

Zpět v OwinCommunicationListener.cs můžete začít implementace OpenAsync. Toto je, kde spustit hello webový server. Nejdřív získat informace o koncovém hello a vytvořit hello adresu URL, který bude naslouchat hello služby. Adresa URL Hello se liší v závislosti na tom, jestli se používá naslouchací proces hello v bezstavové služby nebo stavové služby. Pro stavové služby musí naslouchací proces hello toocreate jedinečný pro každé repliky stavové služby se sleduje v adresu. Pro bezstavové služby může být adresa hello mnohem jednodušší. 

```csharp
public Task<string> OpenAsync(CancellationToken cancellationToken)
{
    var serviceEndpoint = this.serviceContext.CodePackageActivationContext.GetEndpoint(this.endpointName);
    var protocol = serviceEndpoint.Protocol;
    int port = serviceEndpoint.Port;

    if (this.serviceContext is StatefulServiceContext)
    {
        StatefulServiceContext statefulServiceContext = this.serviceContext as StatefulServiceContext;

        this.listeningAddress = string.Format(
            CultureInfo.InvariantCulture,
            "{0}://+:{1}/{2}{3}/{4}/{5}",
            protocol,
            port,
            string.IsNullOrWhiteSpace(this.appRoot)
                ? string.Empty
                : this.appRoot.TrimEnd('/') + '/',
            statefulServiceContext.PartitionId,
            statefulServiceContext.ReplicaId,
            Guid.NewGuid());
    }
    else if (this.serviceContext is StatelessServiceContext)
    {
        this.listeningAddress = string.Format(
            CultureInfo.InvariantCulture,
            "{0}://+:{1}/{2}",
            protocol,
            port,
            string.IsNullOrWhiteSpace(this.appRoot)
                ? string.Empty
                : this.appRoot.TrimEnd('/') + '/');
    }
    else
    {
        throw new InvalidOperationException();
    }

    ...

```

Všimněte si, že se tady používá "http://+". Toto je toomake se, že tento webový server hello naslouchá na všechny dostupné adresy, včetně místního hostitele, plně kvalifikovaný název domény a IP adresy počítače hello.

Hello OpenAsync implementace je jedním z hello nejdůležitější z důvodů, proč hello webového serveru (nebo všechny komunikačního balíku) je implementována jako ICommunicationListener, nikoli jen s ho otevřít přímo z `RunAsync()` ve službě hello. Hello vrácená hodnota z OpenAsync je hello adresu, která hello webový server naslouchá na. Pokud tato adresa se vrátí toohello systému, zaregistruje hello adresu službou hello. Service Fabric poskytuje rozhraní API, která umožňuje klientským a dalších služeb toothen požádejte pro tuto adresu podle názvu služby. To je důležité, protože není statickou adresu služby hello. Služby přesouvání hello clusteru za účelem vyrovnávání a dostupnosti prostředků. Toto je hello mechanismus, který umožňuje klientům tooresolve hello naslouchání adresu pro službu.

OpenAsync si uvědomit, spustí hello webový server a vrátí hello adresu, která naslouchá na. Všimněte si, že naslouchá na "http://+", ale před OpenAsync vrátí hello adresu, hello "+" je nahrazena hello IP adresu nebo plně kvalifikovaný název domény hello uzel, který je aktuálně v. Hello adresu, kterou vrátí tato metoda je, co není zaregistrována hello systému. Je také co klienti a dalším službám uvidí při vyzvou pro adresu služby. Pro klienty toocorrectly připojení tooit, potřebují adresu hello skutečná IP nebo plně kvalifikovaný název domény.

```csharp
    ...

    this.publishAddress = this.listeningAddress.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);

    try
    {
        this.eventSource.Message("Starting web server on " + this.listeningAddress);

        this.webApp = WebApp.Start(this.listeningAddress, appBuilder => this.startup.Invoke(appBuilder));

        this.eventSource.Message("Listening on " + this.publishAddress);

        return Task.FromResult(this.publishAddress);
    }
    catch (Exception ex)
    {
        this.eventSource.Message("Web server failed tooopen endpoint {0}. {1}", this.endpointName, ex.ToString());

        this.StopWebServer();

        throw;
    }
}

```

Všimněte si, že to odkazuje hello spuštění třídu, která byla předána toohello OwinCommunicationListener v konstruktoru hello. Tato instance spuštění používá hello webový server toobootstrap hello aplikace webového rozhraní API.

Hello `ServiceEventSource.Current.Message()` řádku se zobrazí v okně diagnostické události hello později, při spuštění aplikace tooconfirm hello tento hello webový server byl úspěšně spuštěn.

## <a name="implement-closeasync-and-abort"></a>Implementace CloseAsync a přerušení
Nakonec implementovat CloseAsync a přerušení toostop hello webový server. uvolnění popisovač hello serveru, který jste vytvořili během OpenAsync můžete zastavit Hello webový server.

```csharp
public Task CloseAsync(CancellationToken cancellationToken)
{
    this.eventSource.Message("Closing web server on endpoint {0}", this.endpointName);

    this.StopWebServer();

    return Task.FromResult(true);
}

public void Abort()
{
    this.eventSource.Message("Aborting web server on endpoint {0}", this.endpointName);

    this.StopWebServer();
}

private void StopWebServer()
{
    if (this.webApp != null)
    {
        try
        {
            this.webApp.Dispose();
        }
        catch (ObjectDisposedException)
        {
            // no-op
        }
    }
}
```

V tomto příkladu implementace CloseAsync a přerušení jednoduše zastavit hello webový server. Můžete zvolit tooperform více řádně koordinované vypnutí hello webového serveru v CloseAsync. Vypnutí hello může například čekat na během letu požadavky toobe dokončit před hello vrátit.

## <a name="start-hello-web-server"></a>Spustí webový server hello
Teď už připravený toocreate a vrátit instanci třídy OwinCommunicationListener toostart hello webový server. Zpět v hello třída služby (WebService.cs), přepište hello `CreateServiceInstanceListeners()` metoda:

```csharp
protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    var endpoints = Context.CodePackageActivationContext.GetEndpoints()
                           .Where(endpoint => endpoint.Protocol == EndpointProtocol.Http || endpoint.Protocol == EndpointProtocol.Https)
                           .Select(endpoint => endpoint.Name);

    return endpoints.Select(endpoint => new ServiceInstanceListener(
        serviceContext => new OwinCommunicationListener(Startup.ConfigureApp, serviceContext, ServiceEventSource.Current, endpoint), endpoint));
}
```

Kde to je hello webového rozhraní API *aplikace* a hello OWIN *hostitele* nakonec splňovat. Hello hostitele (OwinCommunicationListener) je zadána instance hello *aplikace* (webového rozhraní API) prostřednictvím hello třída při spuštění. Service Fabric spravuje pak životního cyklu. Tento stejný vzor obvykle platí s žádné komunikačního balíku.

## <a name="put-it-all-together"></a>Spojení všech součástí dohromady
V tomto příkladu nepotřebujete toodo ničím v hello `RunAsync()` proto, že přepsání můžete jednoduše odeberou.

implementace Hello konečné služby by měl být velmi jednoduché. Potřebuje pouze toocreate hello komunikace naslouchací proces:

```csharp
using System;
using System.Collections.Generic;
using System.Fabric;
using System.Fabric.Description;
using System.Linq;
using Microsoft.ServiceFabric.Services.Communication.Runtime;
using Microsoft.ServiceFabric.Services.Runtime;

namespace WebService
{
    internal sealed class WebService : StatelessService
    {
        public WebService(StatelessServiceContext context)
            : base(context)
        { }

        protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
        {
            var endpoints = Context.CodePackageActivationContext.GetEndpoints()
                                   .Where(endpoint => endpoint.Protocol == EndpointProtocol.Http || endpoint.Protocol == EndpointProtocol.Https)
                                   .Select(endpoint => endpoint.Name);

            return endpoints.Select(endpoint => new ServiceInstanceListener(
                serviceContext => new OwinCommunicationListener(Startup.ConfigureApp, serviceContext, ServiceEventSource.Current, endpoint), endpoint));
        }
    }
}
```

Hello dokončení `OwinCommunicationListener` třídy:

```csharp
using System;
using System.Diagnostics;
using System.Fabric;
using System.Globalization;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.Owin.Hosting;
using Microsoft.ServiceFabric.Services.Communication.Runtime;
using Owin;

namespace WebService
{
    internal class OwinCommunicationListener : ICommunicationListener
    {
        private readonly ServiceEventSource eventSource;
        private readonly Action<IAppBuilder> startup;
        private readonly ServiceContext serviceContext;
        private readonly string endpointName;
        private readonly string appRoot;

        private IDisposable webApp;
        private string publishAddress;
        private string listeningAddress;

        public OwinCommunicationListener(Action<IAppBuilder> startup, ServiceContext serviceContext, ServiceEventSource eventSource, string endpointName)
            : this(startup, serviceContext, eventSource, endpointName, null)
        {
        }

        public OwinCommunicationListener(Action<IAppBuilder> startup, ServiceContext serviceContext, ServiceEventSource eventSource, string endpointName, string appRoot)
        {
            if (startup == null)
            {
                throw new ArgumentNullException(nameof(startup));
            }

            if (serviceContext == null)
            {
                throw new ArgumentNullException(nameof(serviceContext));
            }

            if (endpointName == null)
            {
                throw new ArgumentNullException(nameof(endpointName));
            }

            if (eventSource == null)
            {
                throw new ArgumentNullException(nameof(eventSource));
            }

            this.startup = startup;
            this.serviceContext = serviceContext;
            this.endpointName = endpointName;
            this.eventSource = eventSource;
            this.appRoot = appRoot;
        }

        public Task<string> OpenAsync(CancellationToken cancellationToken)
        {
            var serviceEndpoint = this.serviceContext.CodePackageActivationContext.GetEndpoint(this.endpointName);
            var protocol = serviceEndpoint.Protocol;
            int port = serviceEndpoint.Port;

            if (this.serviceContext is StatefulServiceContext)
            {
                StatefulServiceContext statefulServiceContext = this.serviceContext as StatefulServiceContext;

                this.listeningAddress = string.Format(
                    CultureInfo.InvariantCulture,
                    "{0}://+:{1}/{2}{3}/{4}/{5}",
                    protocol,
                    port,
                    string.IsNullOrWhiteSpace(this.appRoot)
                        ? string.Empty
                        : this.appRoot.TrimEnd('/') + '/',
                    statefulServiceContext.PartitionId,
                    statefulServiceContext.ReplicaId,
                    Guid.NewGuid());
            }
            else if (this.serviceContext is StatelessServiceContext)
            {
                this.listeningAddress = string.Format(
                    CultureInfo.InvariantCulture,
                    "{0}://+:{1}/{2}",
                    protocol,
                    port,
                    string.IsNullOrWhiteSpace(this.appRoot)
                        ? string.Empty
                        : this.appRoot.TrimEnd('/') + '/');
            }
            else
            {
                throw new InvalidOperationException();
            }

            this.publishAddress = this.listeningAddress.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);

            try
            {
                this.eventSource.Message("Starting web server on " + this.listeningAddress);

                this.webApp = WebApp.Start(this.listeningAddress, appBuilder => this.startup.Invoke(appBuilder));

                this.eventSource.Message("Listening on " + this.publishAddress);

                return Task.FromResult(this.publishAddress);
            }
            catch (Exception ex)
            {
                this.eventSource.Message("Web server failed tooopen endpoint {0}. {1}", this.endpointName, ex.ToString());

                this.StopWebServer();

                throw;
            }
        }

        public Task CloseAsync(CancellationToken cancellationToken)
        {
            this.eventSource.Message("Closing web server on endpoint {0}", this.endpointName);

            this.StopWebServer();

            return Task.FromResult(true);
        }

        public void Abort()
        {
            this.eventSource.Message("Aborting web server on endpoint {0}", this.endpointName);

            this.StopWebServer();
        }

        private void StopWebServer()
        {
            if (this.webApp != null)
            {
                try
                {
                    this.webApp.Dispose();
                }
                catch (ObjectDisposedException)
                {
                    // no-op
                }
            }
        }
    }
}
```

Teď, když jste umístili všech částí hello na místě, projekt by měl vypadat jako typický aplikace webového rozhraní API s spolehlivé rozhraní API služby vstupní body a hostiteli OWIN.

## <a name="run-and-connect-through-a-web-browser"></a>Spuštění a připojení přes webový prohlížeč
Pokud jste neprovedli, [nastavení vývojového prostředí](service-fabric-get-started.md).

Teď můžete vytvářet a nasazovat služby. Stiskněte klávesu **F5** v sadě Visual Studio toobuild a nasazení aplikace hello. V okně diagnostické události hello zobrazí zprávu, která ukazuje na tento webový server hello otevřít na http://localhost:8281 /.

> [!NOTE]
> Pokud hello port již byla otevřena jiným procesem na počítači, může se zobrazit chyba sem. To znamená, že tento naslouchací proces hello nelze otevřít. Pokud se hello případ, zkuste použít jiný port pro hello konfigurace koncového bodu v ServiceManifest.xml.
> 
> 

Jakmile je spuštěna služba hello otevřete prohlížeč a přejděte příliš[http://localhost:8281/api/hodnoty](http://localhost:8281/api/values) tootest ho.

## <a name="scale-it-out"></a>Škálovat tak
Škálování bezstavové webové aplikace obvykle znamená přidáním další počítače a otáčí až hello webové aplikace na ně. Jádro Orchestrace Service Fabric to můžete provést za vás, při každém přidání nových uzlů clusteru tooa. Při vytváření instancí bezstavové služby, můžete zadat číslo hello instancí chcete toocreate. Service Fabric umístí na uzly v clusteru hello tento počet instancí. A zkontroluje, zda není toocreate více než jednu instanci na jednoho libovolného uzlu. Můžete také určit, aby tooalways Service Fabric vytvořit instanci v každém uzlu tak, že zadáte **-1** pro počet instancí hello. To zaručuje, že při každém přidání tooscale uzly na cluster, instanci bezstavové služby vytvořit na nové uzly hello. Tato hodnota je vlastnost hello instance služby, tak, že nastavíte při vytváření instance služby. Můžete to udělat pomocí prostředí PowerShell:

```powershell

New-ServiceFabricService -ApplicationName "fabric:/WebServiceApplication" -ServiceName "fabric:/WebServiceApplication/WebService" -ServiceTypeName "WebServiceType" -Stateless -PartitionSchemeSingleton -InstanceCount -1

```

Můžete také k tomu při definování výchozí služba v projektu sady Visual Studio bezstavové služby:

```xml

<DefaultServices>
  <Service Name="WebService">
    <StatelessService ServiceTypeName="WebServiceType" InstanceCount="-1">
      <SingletonPartition />
    </StatelessService>
  </Service>
</DefaultServices>

```

Další informace o způsobu toocreate aplikace a instance služby, najdete v části [nasazení aplikace](service-fabric-deploy-remove-applications.md).

## <a name="next-steps"></a>Další kroky
[Ladění aplikace Service Fabric pomocí sady Visual Studio](service-fabric-debugging-your-application.md)

