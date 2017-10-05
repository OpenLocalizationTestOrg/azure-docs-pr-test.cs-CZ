---
title: "Diagnostikovat chyby a výjimky v webové aplikace pomocí služby Azure Application Insights | Microsoft Docs"
description: "Zachytit výjimky z aplikací ASP.NET společně s telemetrická žádost."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: d1e98390-3ce4-4d04-9351-144314a42aa2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: 7eeacdc6677ccdebb1653e94a163ecb47090b7ee
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="diagnose-exceptions-in-your-web-apps-with-application-insights"></a>Diagnostikovat výjimky ve webových aplikacích pomocí služby Application Insights
Výjimky v svou živou webovou aplikaci oznamuje [Application Insights](app-insights-overview.md). Neúspěšných požadavků mohou korelovat s výjimky a dalších událostí na klienta a serveru, takže můžete rychle diagnostikovat příčin.

## <a name="set-up-exception-reporting"></a>Nastavit generování sestav výjimky
* Mít výjimky nahlášené z vaší aplikace. server:
  * Nainstalujte [Application Insights SDK](app-insights-asp-net.md) v kódu aplikace, nebo
  * Webové servery služby IIS: Spusťte [agenta Application Insights](app-insights-monitor-performance-live-website-now.md); nebo
  * Službě Azure web apps: Přidat [rozšíření Application Insights](app-insights-azure-web-apps.md)
  * Webové aplikace v jazyce Java: Nainstalujte [agenta Java](app-insights-java-agent.md)
* Nainstalujte [fragment kódu JavaScript](app-insights-javascript.md) na webových stránkách zachycení výjimky v prohlížeči.
* Některé architektury aplikace nebo s některými nastaveními budete muset provést některé další kroky pro zachycení více výjimek:
  * [Webové formuláře](#web-forms)
  * [MVC](#mvc)
  * [1.* webové rozhraní API](#web-api-1)
  * [2.* webové rozhraní API](#web-api-2)
  * [WCF](#wcf)

## <a name="diagnosing-exceptions-using-visual-studio"></a>Diagnostikování výjimky pomocí sady Visual Studio
Otevřete aplikaci řešení v sadě Visual Studio, které pomáhají při ladění.

Spusťte aplikaci na vašem serveru nebo na počítači pro vývoj pomocí F5.

Otevřete okno hledání Application Insights v sadě Visual Studio a nastavte ji chcete zobrazit události z vaší aplikace. Při ladění, můžete k tomu jednoduše kliknutím na tlačítko Application Insights.

![Klikněte pravým tlačítkem na projekt a vyberte Application Insights, otevřete.](./media/app-insights-asp-net-exceptions/34.png)

Všimněte si, že můžete filtrovat sestavě zobrazily jenom výjimky.

*Žádné výjimky zobrazující? V tématu [zachycení výjimky](#exceptions).*

Klikněte na sestavu výjimky a zobrazit jeho trasování zásobníku.
Klikněte na odkaz řádku v trasování zásobníku, otevřete soubor odpovídající kód.  

V kódu Všimněte si, že Codelensu zobrazuje data o výjimky:

![Oznámení Codelensu výjimek.](./media/app-insights-asp-net-exceptions/35.png)

## <a name="diagnosing-failures-using-the-azure-portal"></a>Diagnostikování chyb pomocí portálu Azure
Z přehledu nástroje Application Insights vaší aplikace dlaždici selhání ukazuje grafy výjimek a neúspěšné požadavky HTTP, společně s seznam požadavek adresy URL, které způsobí selhání nejčastěji se vyskytující.

![Vyberte nastavení, selhání](./media/app-insights-asp-net-exceptions/012-start.png)

Klikněte na prostřednictvím jednoho z typů selhání výjimka v seznamu získat u jednotlivých výskytů výjimky, kde můžete zobrazit podrobnosti a trasováním zásobníku:

![Vyberte instanci chybné žádosti a v části Podrobnosti o výjimce, získat na instance výjimky.](./media/app-insights-asp-net-exceptions/030-req-drill.png)

**Alternativně** můžete spustit ze seznamu požadavků a najít výjimky s ním souvisejí.

*Žádné výjimky zobrazující? V tématu [zachycení výjimky](#exceptions).*


## <a name="custom-tracing-and-log-data"></a>Vlastní trasování a data protokolu
K získání diagnostických dat specifické pro vaši aplikaci, můžete vložit kódu pro odeslání vlastní telemetrická data. Toto zobrazí ve vyhledávání diagnostiky společně se žádosti, zobrazení stránky a další automaticky shromažďovat data.

Máte několik možností:

* [TrackEvent()](app-insights-api-custom-events-metrics.md#trackevent) se obvykle používá ke sledování vzorů využití, ale také odesílá data se zobrazí v části vlastní události ve vyhledávání diagnostiky. Události jsou pojmenované a přenášet vlastnosti řetězce a číselné metriky, ve kterém můžete [filtrovat diagnostických hledání](app-insights-diagnostic-search.md).
* [TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace) umožňuje odesílat delší data, jako jsou informace o POST.
* [TrackException()](#exceptions) odešle trasování zásobníku. [Další informace o výjimky](#exceptions).
* Pokud už používáte rozhraní protokolování, jako je Log4Net nebo NLog, můžete [zaznamenat tyto protokoly](app-insights-asp-net-trace-logs.md) a zobrazovat ve vyhledávání diagnostiky společně se data požadavku a výjimky.

Pokud chcete zobrazit tyto události, otevřete [vyhledávání](app-insights-diagnostic-search.md), otevřete filtr a potom vyberte vlastní události, trasování nebo výjimky.

![Procházení](./media/app-insights-asp-net-exceptions/viewCustomEvents.png)

> [!NOTE]
> Pokud vaše aplikace generuje mnoho telemetrických dat, sníží modul adaptivního vzorkování automaticky objem dat odesílaných na portál tím, že budou odesílány pouze reprezentativní vzorky událostí. Události, které jsou součástí stejné operace bude vybraná nebo nevybrány jako skupina, takže mohou procházet mezi souvisejícími událostmi. [Další informace o vzorkování.](app-insights-sampling.md)
>
>

### <a name="how-to-see-request-post-data"></a>Jak zjistit, data požadavku POST
Podrobnosti požadavku neobsahují data odeslaná do vaší aplikace v volání POST. Aby tato data nahlášené:

* [Instalace sady SDK](app-insights-asp-net.md) ve vašem projektu aplikace.
* Vložení kódu v aplikaci k volání [Microsoft.ApplicationInsights.TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace). Odesílání dat POST v parametru zprávy. Neexistuje omezení povolenou velikost, takže pokuste odeslat jenom základní data.
* Pokud byste prozkoumat chybných požadavků, najdete přidružené trasování.  

![Procházení](./media/app-insights-asp-net-exceptions/060-req-related.png)

## <a name="exceptions"></a>Zachytávání výjimek a související diagnostických dat
Na první pohled neuvidíte na portálu všechny výjimky, které způsobí selhání ve vaší aplikaci. Zobrazí všechny výjimky prohlížeče (Pokud používáte [JavaScript SDK](app-insights-javascript.md) na webových stránkách). Ale většina serveru výjimky jsou zachyceny službou IIS a máte k zápisu verze kódu k jejich zobrazení.

Můžete:

* **Protokolování výjimek explicitně** vložením kódu do obslužné rutiny výjimek nahlásit výjimky.
* **Zachytit výjimky automaticky** nakonfigurováním požadované rozhraní ASP.NET. Potřebné dodatky se liší pro různé typy framework.

## <a name="reporting-exceptions-explicitly"></a>Explicitně Reporting výjimky
Nejjednodušší způsob, jak se má vložit volání pro TrackException() do obslužné rutiny výjimek.

JavaScript

    try
    { ...
    }
    catch (ex)
    {
      appInsights.trackException(ex, "handler loc",
        {Game: currentGame.Name,
         State: currentGame.State.ToString()});
    }

C#

    var telemetry = new TelemetryClient();
    ...
    try
    { ...
    }
    catch (Exception ex)
    {
       // Set up some properties:
       var properties = new Dictionary <string, string>
         {{"Game", currentGame.Name}};

       var measurements = new Dictionary <string, double>
         {{"Users", currentGame.Users.Count}};

       // Send the exception telemetry:
       telemetry.TrackException(ex, properties, measurements);
    }

JAZYKA VISUAL BASIC

    Dim telemetry = New TelemetryClient
    ...
    Try
      ...
    Catch ex as Exception
      ' Set up some properties:
      Dim properties = New Dictionary (Of String, String)
      properties.Add("Game", currentGame.Name)

      Dim measurements = New Dictionary (Of String, Double)
      measurements.Add("Users", currentGame.Users.Count)

      ' Send the exception telemetry:
      telemetry.TrackException(ex, properties, measurements)
    End Try

Vlastnosti a měření parametry jsou volitelné, ale jsou užitečné pro [filtrování a přidání](app-insights-diagnostic-search.md) doplňující informace. Například pokud máte aplikaci, která můžete spustit několik hry, může najít všechny sestavy výjimky související s konkrétní hru. Můžete přidat libovolný počet položek, jako má každý slovníku.

## <a name="browser-exceptions"></a>Výjimky prohlížečů
Většina prohlížeče výjimek jsou hlášeny.

Pokud vaše webová stránka obsahuje soubory skriptu ze sítě pro doručování obsahu nebo v jiných doménách, ujistěte se, vaše značky script má atribut ```crossorigin="anonymous"```, a že server odešle [hlavičky CORS](http://enable-cors.org/). To vám umožní získat trasování zásobníku a podrobnosti pro neošetřených výjimek jazyka JavaScript z těchto prostředků.

## <a name="web-forms"></a>Webové formuláře
U webových formulářů bude možné ke shromažďování výjimky, když nejsou žádné přesměrování nakonfigurované CustomErrors modulu protokolu HTTP.

Ale pokud máte aktivní přesměrování, přidejte následující řádky do funkce Application_Error Global.asax.cs. (Pokud jste ještě nemáte, přidejte souboru Global.asax.)

*C#*

    void Application_Error(object sender, EventArgs e)
    {
      if (HttpContext.Current.IsCustomErrorEnabled && Server.GetLastError  () != null)
      {
         var ai = new TelemetryClient(); // or re-use an existing instance

         ai.TrackException(Server.GetLastError());
      }
    }


## <a name="mvc"></a>MVC
Pokud [CustomErrors](https://msdn.microsoft.com/library/h0hfz6fc.aspx) konfigurace je `Off`, bude k dispozici pro výjimky [modulu HTTP](https://msdn.microsoft.com/library/ms178468.aspx) ke shromažďování. Ale pokud je `RemoteOnly` (výchozí), nebo `On`, bude výjimka nezaškrtnuté a nejsou k dispozici pro službu Application Insights automaticky shromažďovat. Můžete to opravíme přepsáním [System.Web.Mvc.HandleErrorAttribute – třída](http://msdn.microsoft.com/library/system.web.mvc.handleerrorattribute.aspx)a použití přepsaného třídu, jak je uvedeno pro různé verze MVC níže ([githubu zdroj](https://github.com/AppInsightsSamples/Mvc2UnhandledExceptions/blob/master/MVC2App/Controllers/AiHandleErrorAttribute.cs)):

    using System;
    using System.Web.Mvc;
    using Microsoft.ApplicationInsights;

    namespace MVC2App.Controllers
    {
      [AttributeUsage(AttributeTargets.Class | AttributeTargets.Method, Inherited = true, AllowMultiple = true)]
      public class AiHandleErrorAttribute : HandleErrorAttribute
      {
        public override void OnException(ExceptionContext filterContext)
        {
            if (filterContext != null && filterContext.HttpContext != null && filterContext.Exception != null)
            {
                //If customError is Off, then AI HTTPModule will report the exception
                if (filterContext.HttpContext.IsCustomErrorEnabled)
                {   //or reuse instance (recommended!). see note above  
                    var ai = new TelemetryClient();
                    ai.TrackException(filterContext.Exception);
                }
            }
            base.OnException(filterContext);
        }
      }
    }

#### <a name="mvc-2"></a>MVC 2
Atribut HandleError nahraďte váš nový atribut v řadičů.

    namespace MVC2App.Controllers
    {
       [AiHandleError]
       public class HomeController : Controller
       {
    ...

[Ukázka](https://github.com/AppInsightsSamples/Mvc2UnhandledExceptions)

#### <a name="mvc-3"></a>MVC 3
Zaregistrovat `AiHandleErrorAttribute` jako globální filtr ve funkci Global.asax.cs:

    public class MyMvcApplication : System.Web.HttpApplication
    {
      public static void RegisterGlobalFilters(GlobalFilterCollection filters)
      {
         filters.Add(new AiHandleErrorAttribute());
      }
     ...

[Ukázka](https://github.com/AppInsightsSamples/Mvc3UnhandledExceptionTelemetry)

#### <a name="mvc-4-mvc5"></a>MVC 4, MVC5
Zaregistrujte AiHandleErrorAttribute jako globální filtr ve funkci FilterConfig.cs:

    public class FilterConfig
    {
      public static void RegisterGlobalFilters(GlobalFilterCollection filters)
      {
        // Default replaced with the override to track unhandled exceptions
        filters.Add(new AiHandleErrorAttribute());
      }
    }

[Ukázka](https://github.com/AppInsightsSamples/Mvc5UnhandledExceptionTelemetry)

## <a name="web-api-1x"></a>Webové rozhraní API 1.x
Přepište System.Web.Http.Filters.ExceptionFilterAttribute:

    using System.Web.Http.Filters;
    using Microsoft.ApplicationInsights;

    namespace WebAPI.App_Start
    {
      public class AiExceptionFilterAttribute : ExceptionFilterAttribute
      {
        public override void OnException(HttpActionExecutedContext actionExecutedContext)
        {
            if (actionExecutedContext != null && actionExecutedContext.Exception != null)
            {  //or reuse instance (recommended!). see note above
                var ai = new TelemetryClient();
                ai.TrackException(actionExecutedContext.Exception);    
            }
            base.OnException(actionExecutedContext);
        }
      }
    }

Můžete přidat tento atribut přepsaného na konkrétní řadiče, nebo ho přidat do konfigurace globálních filtrů v třídy WebApiConfig:

    using System.Web.Http;
    using WebApi1.x.App_Start;

    namespace WebApi1.x
    {
      public static class WebApiConfig
      {
        public static void Register(HttpConfiguration config)
        {
            config.Routes.MapHttpRoute(name: "DefaultApi", routeTemplate: "api/{controller}/{id}",
                defaults: new { id = RouteParameter.Optional });
            ...
            config.EnableSystemDiagnosticsTracing();

            // Capture exceptions for Application Insights:
            config.Filters.Add(new AiExceptionFilterAttribute());
        }
      }
    }

[Ukázka](https://github.com/AppInsightsSamples/WebApi_1.x_UnhandledExceptions)

Existuje několik případů, které nelze zpracovat filtry výjimek. Například:

* Výjimky vydané z konstruktorů řadiče.
* Výjimek vyvolaných z obslužné rutiny zpráv.
* Výjimky vydané během trasování.
* Výjimky vydané během serializace obsahu odpovědi.

## <a name="web-api-2x"></a>Webové rozhraní API 2.x
Přidání implementace IExceptionLogger:

    using System.Web.Http.ExceptionHandling;
    using Microsoft.ApplicationInsights;

    namespace ProductsAppPureWebAPI.App_Start
    {
      public class AiExceptionLogger : ExceptionLogger
      {
        public override void Log(ExceptionLoggerContext context)
        {
            if (context !=null && context.Exception != null)
            {//or reuse instance (recommended!). see note above
                var ai = new TelemetryClient();
                ai.TrackException(context.Exception);
            }
            base.Log(context);
        }
      }
    }

Přidejte tuto pro služby WebApiConfig:

    using System.Web.Http;
    using System.Web.Http.ExceptionHandling;
    using ProductsAppPureWebAPI.App_Start;

    namespace WebApi2WithMVC
    {
      public static class WebApiConfig
      {
        public static void Register(HttpConfiguration config)
        {
            // Web API configuration and services

            // Web API routes
            config.MapHttpAttributeRoutes();

            config.Routes.MapHttpRoute(
                name: "DefaultApi",
                routeTemplate: "api/{controller}/{id}",
                defaults: new { id = RouteParameter.Optional }
            );
            config.Services.Add(typeof(IExceptionLogger), new AiExceptionLogger());
        }
      }
  }

[Ukázka](https://github.com/AppInsightsSamples/WebApi_2.x_UnhandledExceptions)

Jako alternativy které může:

1. Nahraďte pouze ExceptionHandler vlastní implementaci IExceptionHandler. Volá se, jenom když rozhraní je možné zvolit které zprávu odpovědi na odeslání (při připojení není přerušena pro instanci)
2. Filtry výjimek (jak je popsáno v části na řadičích 1.x webového rozhraní API výše) – ve všech případech není volána.

## <a name="wcf"></a>WCF
Přidejte třídu, která rozšiřuje atribut a implementuje IErrorHandler a IServiceBehavior.

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.ServiceModel.Description;
    using System.ServiceModel.Dispatcher;
    using System.Web;
    using Microsoft.ApplicationInsights;

    namespace WcfService4.ErrorHandling
    {
      public class AiLogExceptionAttribute : Attribute, IErrorHandler, IServiceBehavior
      {
        public void AddBindingParameters(ServiceDescription serviceDescription,
            System.ServiceModel.ServiceHostBase serviceHostBase,
            System.Collections.ObjectModel.Collection<ServiceEndpoint> endpoints,
            System.ServiceModel.Channels.BindingParameterCollection bindingParameters)
        {
        }

        public void ApplyDispatchBehavior(ServiceDescription serviceDescription,
            System.ServiceModel.ServiceHostBase serviceHostBase)
        {
            foreach (ChannelDispatcher disp in serviceHostBase.ChannelDispatchers)
            {
                disp.ErrorHandlers.Add(this);
            }
        }

        public void Validate(ServiceDescription serviceDescription,
            System.ServiceModel.ServiceHostBase serviceHostBase)
        {
        }

        bool IErrorHandler.HandleError(Exception error)
        {//or reuse instance (recommended!). see note above
            var ai = new TelemetryClient();

            ai.TrackException(error);
            return false;
        }

        void IErrorHandler.ProvideFault(Exception error,
            System.ServiceModel.Channels.MessageVersion version,
            ref System.ServiceModel.Channels.Message fault)
        {
        }
      }
    }

Přidejte atribut do implementace služby:

    namespace WcfService4
    {
        [AiLogException]
        public class Service1 : IService1
        {
         ...

[Ukázka](https://github.com/AppInsightsSamples/WCFUnhandledExceptions)

## <a name="exception-performance-counters"></a>Čítače výkonu výjimky
Pokud máte [nainstalovat agenta Application Insights](app-insights-monitor-performance-live-website-now.md) na serveru, můžete získat graf míry výjimky měří v rozhraní .NET. To zahrnuje zpracovávaný i neošetřené výjimky .NET.

Otevřete okno Průzkumníka metrika, přidejte nový graf a vyberte **výjimka míra**, uvedené v části čítače výkonu.

Rozhraní .NET framework vypočítá rychlost počítání počet výjimek v intervalu a vydělí délku intervalu.

Všimněte si, že bude liší od počtu "Výjimky" vypočítána pomocí portálu služby Application Insights počítání TrackException sestavy. Během intervalů vzorkování se liší a sady SDK neodešle TrackException sestavy pro všechny zpracovat a neošetřené výjimky.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player] 

## <a name="next-steps"></a>Další kroky
* [Sledování REST, SQL a jiné volání závislosti](app-insights-asp-net-dependencies.md)
* [Monitorování časů načtení stránky, výjimek prohlížeče a volání AJAX](app-insights-javascript.md)
* [Čítače sledování výkonu](app-insights-performance-counters.md)
