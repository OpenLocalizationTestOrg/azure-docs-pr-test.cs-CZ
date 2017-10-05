---
title: "Služba komunikaci s rozhraním ASP.NET Web API | Microsoft Docs"
description: "Zjistěte, jak implementovat podrobné komunikace služby pomocí rozhraní ASP.NET Web API OWIN samoobslužné hostování v spolehlivé Services API."
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
ms.openlocfilehash: 73b7e1c0cb93ae7c54780a3aab837b0e5bcdb0a0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-service-fabric-web-api-services-with-owin-self-hosting"></a>Začínáme: Service Fabric webového rozhraní API služby s vlastním hostování OWIN
Azure Service Fabric vloží napájení rukou při se rozhodování, jak chcete, aby vaše služby, aby komunikovat s uživateli a mezi sebou. Tento kurz se zaměřuje na implementaci komunikace služby pomocí rozhraní ASP.NET Web API Open Web Interface pro .NET (OWIN) samoobslužné hostování v Service Fabric spolehlivé Services API. Jsme budete pustíte hluboko do spolehlivé služby modulární komunikace rozhraní API. Také použijeme webového rozhraní API v Podrobný příklad návod, jak nastavit vlastní komunikace naslouchací proces.

## <a name="introduction-to-web-api-in-service-fabric"></a>Úvod do webového rozhraní API v Service Fabric
Rozhraní ASP.NET Web API je Oblíbené a výkonné rozhraní pro vytváření rozhraní API HTTP na rozhraní .NET Framework. Pokud už nejste obeznámeni s rozhraní, přečtěte si téma [Začínáme s ASP.NET Web API 2](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) Další informace.

Webové rozhraní API v Service Fabric je stejné rozhraní ASP.NET Web API znáte a rádi. Rozdíl je, jak můžete *hostitele* aplikace webového rozhraní API. Nebudete používat Microsoft Internetové informační služby (IIS). Abyste lépe pochopili rozdíl, můžeme ho rozdělit na dvě části:

1. Aplikace webového rozhraní API (včetně řadiče a modely)
2. Hostitele (na webový server, obvykle IIS)

Vlastní aplikace webového rozhraní API se nezmění. Se neliší od aplikací webového rozhraní API, které máte napsaný v minulosti a nyní byste měli mít jednoduše přesunout přes většinu kódu aplikace. Ale pokud jste si byla hostování ve službě IIS, kde může být jen málo liší od se používá pro hostitele aplikace. Předtím, než se nám získat hostování část, Začněme s něco další známé: aplikace webového rozhraní API.

## <a name="create-the-application"></a>Vytvoření aplikace
Začněte vytvořením nové aplikace Service Fabric pomocí jednoho bezstavové služby v sadě Visual Studio 2015.

Šablony sady Visual Studio pro bezstavové služby pomocí rozhraní Web API je k dispozici. V tomto kurzu využijeme projekt webového rozhraní API od začátku, jejímž výsledkem co byste získali jste vybrali této šablony.

Vyberte prázdný projekt bezstavové služby se dozvíte, jak sestavit projekt webového rozhraní API od začátku, nebo můžete začít se šablonou bezstavové služby webového rozhraní API a jednoduše podle nich zorientujete.  

Prvním krokem je stáhnout některé balíčky NuGet pro webové rozhraní API. Balíček, který chcete použít je Microsoft.AspNet.WebApi.OwinSelfHost. Tento balíček obsahuje všechny potřebné balíčky webového rozhraní API a *hostitele* balíčky. To bude důležité později.

Po dokončení instalace balíčků, můžete začít vytváření základní strukturu projekt webového rozhraní API. Pokud jste použili webového rozhraní API, struktura projektu velmi povědomé. Začněte přidáním `Controllers` directory a řadič jednoduché hodnoty:

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

V dalším kroku přidejte třídu spuštění v kořenu projektu k registraci směrování, formátování a dalších nastavení konfigurace. Toto je také kde webového rozhraní API připojí k *hostitele*, který bude kdykoli znovu spustit později. 

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

Je to pro část aplikace. V tomto okamžiku nastavili jsme právě základní rozložení projektu webového rozhraní API. Pokud by neměl vypadat výrazně liší od projekty webového rozhraní API, že jste vytvořili v minulosti nebo ze základní šablony webového rozhraní API. Obchodní logiky přejde v kontrolerech a modelech jako obvykle.

Nyní co můžeme udělat o hostování tak, aby jeho ve skutečnosti spuštěním?

## <a name="service-hosting"></a>Služby hostování
V Service Fabric služby běží v *procesu hostitele služby*, spustitelný soubor, který spouští službu kódu. Při zápisu služby pomocí rozhraní API spolehlivé služby projektu služby právě zkompiluje na spustitelný soubor, který zaregistruje typ vaší služby a spustí váš kód. To platí ve většině případů při zápisu služby v Service Fabric v rozhraní .NET. Při otevření souboru Program.cs v projektu bezstavové služby, byste měli vidět:

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

Pokud toto vypadá nápadně jako vstupní bod do konzolovou aplikaci, která je, protože je.

Další podrobnosti o procesu hostitele služby a registraci služby jsou nad rámec tohoto článku. Je důležité vědět, pro, ale teď, když *kódu služby běží ve svém vlastním procesu*.

## <a name="self-host-web-api-with-an-owin-host"></a>Hostování na vlastním serveru webového rozhraní API s hostitelem OWIN
Vzhledem k tomu, že je kód aplikace webového rozhraní API hostované ve svém vlastním procesu, jak vám propojte ho na webový server? Zadejte [OWIN](http://owin.org/). OWIN je jednoduše smlouva mezi webových aplikací .NET a webovými servery. Tradičně při ASP.NET (až MVC 5) se používá, webové aplikace je úzce párované pro službu IIS prostřednictvím System.Web. Ale webového rozhraní API implementuje OWIN, tak z webového serveru, který je hostitelem ho můžete napsat webovou aplikaci, která odpojené. Z toho důvodu můžete použít *hostovanou na vlastním* OWIN webový server, který můžete spustit v vlastního procesu. To odpovídá perfektně s modelem hostování Service Fabric, kterou jsme právě popsané.

V tomto článku použijeme Katana jako hostitel OWIN pro aplikaci webového rozhraní API. Katana je implementace hostitele OWIN open source založený na [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx) a Windows [rozhraní API serveru HTTP](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).

> [!NOTE]
> Další informace o Katana, přejděte na [Katana lokality](http://www.asp.net/aspnet/overview/owin-and-katana/an-overview-of-project-katana). Rychlý přehled o tom, jak používat Katana pro hostování na vlastním serveru webového rozhraní API, najdete v části [použít OWIN Self-Host ASP.NET Web API 2](http://www.asp.net/web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api).
> 
> 

## <a name="set-up-the-web-server"></a>Nastavit webový server
Spolehlivé služby API poskytuje ke vstupnímu bodu komunikace, kde můžete zařadit zásobníky komunikace, které umožňují uživatelům a klienty pro připojení ke službě:

```csharp

protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    ...
}

```

Webový server (a další komunikačního balíku, které můžete použít v budoucnu, jako je například technologie WebSockets) by měl použít rozhraní ICommunicationListener správně integrovat s v systému. Důvody této začnou více zřejmá v následujících krocích.

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

Rozhraní ICommunicationListener poskytuje tři metody pro správu naslouchací proces komunikace služby:

* *OpenAsync*. Zahájit naslouchání požadavkům.
* *CloseAsync*. Zastavil naslouchání požadavkům, dokončit všechny během letu žádosti a korektně vypnout.
* *Abort –*. Všechno, co zrušte a zastavte okamžitě.

Pokud chcete začít, přidejte členy privátní třídy pro naslouchací proces bude potřeba funkce věcí. Tyto se inicializovat pomocí konstruktoru a později použije při nastavování naslouchání adresy URL.

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
Chcete-li nastavit webový server, je třeba dva údaje:

* *Předponu adresy URL cesta*. I když tato položka je nepovinná, že je vhodné, můžete to teď nastavte, aby bylo možné bezpečně hostovat několik webových služeb ve vaší aplikaci.
* *Port*.

Než získáte port na webovém serveru, je důležité pochopit, že Service Fabric poskytuje aplikační vrstvu, která funguje jako vyrovnávací paměť mezi vaší aplikace a její příslušný operační systém, který běží na. Jako takový Service Fabric nabízí způsob, jak nakonfigurovat *koncové body* pro vaše služby. Service Fabric zajistí, že koncové body jsou k dispozici pro vaši službu používat. Tímto způsobem nemáte je konfigurovat sami v základní prostředí operačního systému. Snadno můžete hostovat aplikace Service Fabric v různých prostředích bez nutnosti provádět všechny změny do vaší aplikace. (Například je možné hostovat stejnou aplikaci v Azure nebo ve svém vlastním datovém centru.)

Konfigurace koncového bodu protokolu HTTP v PackageRoot\ServiceManifest.xml:

```xml
<Resources>
    <Endpoints>
        <Endpoint Name="ServiceEndpoint" Type="Input" Protocol="http" Port="8281" />
    </Endpoints>
</Resources>

```

Tento krok je důležité, protože proces hostitele služby běží pověřeními s omezeným přístupem (síťové služby v systému Windows). To znamená, že služby nebudete mít přístup k nastavení koncový bod HTTP na svůj vlastní. Pomocí konfigurace koncového bodu Service Fabric zná k nastavení seznamu řízení správné přístupu (ACL) pro adresu URL, který bude naslouchat službu. Service Fabric také poskytuje standardní místo pro konfiguraci koncových bodů.

Zpět v OwinCommunicationListener.cs můžete začít implementace OpenAsync. Toto je, kde spustit webový server. Nejdřív získat informace o koncový bod a vytvořit adresu URL, kterou služba bude naslouchat na portu. Adresa URL se liší v závislosti na tom, jestli se používá naslouchací proces v bezstavové služby nebo stavové služby. Pro stavové služby je potřeba vytvořit jedinečnou adresu pro každou repliku stavové služby, kterou naslouchá na naslouchací proces. Adresa pro bezstavové služby, může být mnohem jednodušší. 

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

Všimněte si, že se tady používá "http://+". Toto je zajistit, že webový server naslouchá na všechny dostupné adresy, včetně místního hostitele, plně kvalifikovaný název domény a IP adresu počítače.

Implementace OpenAsync je jedním z nejdůležitějších důvodů, proč webového serveru (nebo všechny komunikačního balíku) je implementována jako ICommunicationListener, nikoli jen s ho otevřít přímo z `RunAsync()` ve službě. Vrácená hodnota z OpenAsync je adresa, která naslouchá na webovém serveru. Pokud tato adresa se vrátí do systému, registruje adresu pomocí služby. Service Fabric poskytuje rozhraní API, která umožňuje klientům a dalším službám, a pak požádejte podle názvu služby pro tuto adresu. To je důležité, protože není statickou adresu služby. Služby se přesouvají v clusteru pro účely vyrovnávání a dostupnosti prostředků. Toto je mechanismus, který umožňuje klientům přeložit adresu naslouchání pro službu.

OpenAsync si uvědomit, spustí webový server a vrátí adresu, která naslouchá na. Všimněte si, že naslouchá na "http://+", ale před OpenAsync vrátí adresu, "+" je nahrazena IP adresu nebo plně kvalifikovaný název domény uzlu je aktuálně v. Adresa, vrátí tato metoda je, co není zaregistrována v systému. Je také co klienti a dalším službám uvidí při vyzvou pro adresu služby. Aby klienti mohli k nim připojit správně potřebují skutečná IP nebo plně kvalifikovaný název domény v adrese.

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
        this.eventSource.Message("Web server failed to open endpoint {0}. {1}", this.endpointName, ex.ToString());

        this.StopWebServer();

        throw;
    }
}

```

Všimněte si, že tato třída při spuštění, který byl předán v OwinCommunicationListener v konstruktoru odkazuje. Tato instance spuštění se používá pro webový server k navázání připojení aplikace webového rozhraní API.

`ServiceEventSource.Current.Message()` Řádku se zobrazí v okně diagnostické události později, při spuštění aplikace pro potvrzení, že webový server byl úspěšně spuštěn.

## <a name="implement-closeasync-and-abort"></a>Implementace CloseAsync a přerušení
Nakonec implementujte CloseAsync a přerušení zastavení webového serveru. Uvolnění popisovač serveru, který jste vytvořili během OpenAsync můžete zastavit webový server.

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

V tomto příkladu implementace CloseAsync a přerušení jednoduše zastavení webového serveru. Můžete zvolit provádět více řádně koordinované vypnutí webového serveru v CloseAsync. Ukončení může například čekat na během letu žádosti o splnit, aby návratový.

## <a name="start-the-web-server"></a>Spustí webový server
Nyní jste připraveni vytvořit a vrátit instanci třídy OwinCommunicationListener Start pro server webového. Zpět v třídě služby (WebService.cs) přepsat `CreateServiceInstanceListeners()` metoda:

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

To je, kdy webového rozhraní API *aplikace* a OWIN *hostitele* nakonec splňovat. Hostitel (OwinCommunicationListener) je zadána instance *aplikace* (webového rozhraní API) prostřednictvím třída při spuštění. Service Fabric spravuje pak životního cyklu. Tento stejný vzor obvykle platí s žádné komunikačního balíku.

## <a name="put-it-all-together"></a>Spojení všech součástí dohromady
V tomto příkladu, nemusíte dělat nic `RunAsync()` proto, že přepsání můžete jednoduše odeberou.

Implementace konečné služby by měl být velmi jednoduché. Stačí, když ho vytvořit naslouchací proces komunikace:

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

Kompletní `OwinCommunicationListener` třídy:

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
                this.eventSource.Message("Web server failed to open endpoint {0}. {1}", this.endpointName, ex.ToString());

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

Teď, když jste umístili všechny části na místě, projekt by měl vypadat jako typický aplikace webového rozhraní API s spolehlivé rozhraní API služby vstupní body a hostiteli OWIN.

## <a name="run-and-connect-through-a-web-browser"></a>Spuštění a připojení přes webový prohlížeč
Pokud jste neprovedli, [nastavení vývojového prostředí](service-fabric-get-started.md).

Teď můžete vytvářet a nasazovat služby. Stiskněte klávesu **F5** v sadě Visual Studio pro vytváření a nasazení aplikace. V okně diagnostických událostí zobrazí zprávu, která označuje, že webový server otevřen na http://localhost:8281 /.

> [!NOTE]
> Pokud port již byla otevřena jiným procesem na počítači, může se zobrazit chyba sem. To znamená, že naslouchací proces nelze otevřít. Pokud je to tento případ, zkuste použít jiný port pro konfiguraci koncového bodu v ServiceManifest.xml.
> 
> 

Jakmile je služba spuštěná, spusťte prohlížeč a přejděte do [http://localhost:8281/api/hodnoty](http://localhost:8281/api/values) to vyzkoušíte.

## <a name="scale-it-out"></a>Škálovat tak
Škálování bezstavové webové aplikace obvykle znamená přidáním další počítače a otáčí do webové aplikace na ně. Jádro Orchestrace Service Fabric to můžete provést za vás, při každém přidání nových uzlů do clusteru. Při vytváření instancí bezstavové služby, můžete zadat počet instancí, které chcete vytvořit. Service Fabric umístí tohoto počtu instancí na uzly v clusteru. A umožňuje nezapomeňte vytvořit více než jedna instance na jednoho libovolného uzlu. Můžete také určit, aby Service Fabric vždy vytvořit instanci na každý uzel zadáním **-1** pro počet instancí. To zaručuje, že při každém přidání uzlů pro horizontální škálování vašeho clusteru, bude instance bezstavové služby vytvořena na nových uzlů. Tato hodnota je vlastnost instance služby tak, že nastavíte při vytváření instance služby. Můžete to udělat pomocí prostředí PowerShell:

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

Další informace o tom, jak vytvořit aplikace a instance služby najdete v tématu [nasazení aplikace](service-fabric-deploy-remove-applications.md).

## <a name="next-steps"></a>Další kroky
[Ladění aplikace Service Fabric pomocí sady Visual Studio](service-fabric-debugging-your-application.md)

