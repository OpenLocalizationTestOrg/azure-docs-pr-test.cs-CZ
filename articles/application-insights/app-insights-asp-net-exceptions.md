---
title: "aaaDiagnose chyby a výjimky v webové aplikace pomocí služby Azure Application Insights | Microsoft Docs"
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
ms.openlocfilehash: 8930e6d2b29f83ea635c4ecb7afd11fc1d97d085
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="diagnose-exceptions-in-your-web-apps-with-application-insights"></a>Diagnostikovat výjimky ve webových aplikacích pomocí služby Application Insights
Výjimky v svou živou webovou aplikaci oznamuje [Application Insights](app-insights-overview.md). Neúspěšných požadavků mohou korelovat s výjimkami a další události v hello klient i server, takže můžete rychle diagnostikovat hello příčiny.

## <a name="set-up-exception-reporting"></a>Nastavit generování sestav výjimky
* výjimky toohave nahlášené z vaší aplikace. server:
  * Nainstalujte [Application Insights SDK](app-insights-asp-net.md) v kódu aplikace, nebo
  * Webové servery služby IIS: Spusťte [agenta Application Insights](app-insights-monitor-performance-live-website-now.md); nebo
  * Službě Azure web apps: přidejte hello [rozšíření Application Insights](app-insights-azure-web-apps.md)
  * Webové aplikace v jazyce Java: instalace hello [agenta Java](app-insights-java-agent.md)
* Nainstalujte hello [fragment kódu JavaScript](app-insights-javascript.md) ve vaší webové stránky toocatch prohlížeče výjimky.
* V některých aplikační architektury, nebo s některými nastaveními, musíte tootake některé dodatečné kroky toocatch více výjimek:
  * [Webové formuláře](#web-forms)
  * [MVC](#mvc)
  * [1.* webové rozhraní API](#web-api-1)
  * [2.* webové rozhraní API](#web-api-2)
  * [WCF](#wcf)

## <a name="diagnosing-exceptions-using-visual-studio"></a>Diagnostikování výjimky pomocí sady Visual Studio
Otevřete v sadě Visual Studio toohelp s laděním řešení aplikace hello.

Spusťte aplikaci hello na vašem serveru nebo na počítači pro vývoj pomocí F5.

Otevřete okno hello hledání Application Insights v sadě Visual Studio a nastavte ji toodisplay události z vaší aplikace. Při ladění, můžete k tomu jenom kliknutím na tlačítko Application Insights hello.

![Klikněte pravým tlačítkem na projekt hello a vyberte Application Insights, otevřete.](./media/app-insights-asp-net-exceptions/34.png)

Všimněte si, že můžete filtrovat pouze výjimky tooshow hello sestavy.

*Žádné výjimky zobrazující? V tématu [zachycení výjimky](#exceptions).*

Klikněte na jeho trasování zásobníku tooshow sestavy k výjimce.
Klikněte na odkaz řádku v trasování zásobníku hello, tooopen hello relevantní kódu souborů.  

V kódu hello Všimněte si, že Codelensu zobrazuje data o výjimkách hello:

![Oznámení Codelensu výjimek.](./media/app-insights-asp-net-exceptions/35.png)

## <a name="diagnosing-failures-using-hello-azure-portal"></a>Diagnostikování chyb pomocí hello portálu Azure
Z přehledu Application Insights hello vaší aplikace dlaždici selhání hello ukazuje grafy výjimek a k selhání požadavků HTTP, společně s seznam hello žádosti adresy URL, které způsobí selhání nejčastěji se vyskytující hello.

![Vyberte nastavení, selhání](./media/app-insights-asp-net-exceptions/012-start.png)

Klikněte na tlačítko prostřednictvím jednoho z hello se nezdařilo typy výjimek v hello seznamu tooget tooindividual výskytů hello výjimky, kde můžete zobrazit podrobnosti hello a trasování zásobníku:

![Vyberte instanci chybné žádosti a v části Podrobnosti o výjimce, získat tooinstances hello výjimky.](./media/app-insights-asp-net-exceptions/030-req-drill.png)

**Alternativně** můžete spustit z hello seznam požadavků a najít související tooit výjimky.

*Žádné výjimky zobrazující? V tématu [zachycení výjimky](#exceptions).*


## <a name="custom-tracing-and-log-data"></a>Vlastní trasování a data protokolu
tooget diagnostických dat konkrétní tooyour aplikace, můžete vložit toosend kód vlastní telemetrická data. Toto zobrazí ve vyhledávání diagnostiky spolu s hello požadavku, zobrazení stránky a další automaticky shromažďovat data.

Máte několik možností:

* [TrackEvent()](app-insights-api-custom-events-metrics.md#trackevent) se obvykle používá ke sledování vzorů využití, ale také odesílá data se zobrazí v části vlastní události ve vyhledávání diagnostiky hello. Události jsou pojmenované a přenášet vlastnosti řetězce a číselné metriky, ve kterém můžete [filtrovat diagnostických hledání](app-insights-diagnostic-search.md).
* [TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace) umožňuje odesílat delší data, jako jsou informace o POST.
* [TrackException()](#exceptions) odešle trasování zásobníku. [Další informace o výjimky](#exceptions).
* Pokud už používáte rozhraní protokolování, jako je Log4Net nebo NLog, můžete [zaznamenat tyto protokoly](app-insights-asp-net-trace-logs.md) a zobrazovat ve vyhledávání diagnostiky společně se data požadavku a výjimky.

Otevřete tyto události toosee [vyhledávání](app-insights-diagnostic-search.md), otevřete filtr a potom vyberte vlastní události, trasování nebo výjimky.

![Procházení](./media/app-insights-asp-net-exceptions/viewCustomEvents.png)

> [!NOTE]
> Pokud vaše aplikace generuje mnoho telemetrie, hello modul adaptivního vzorkování automaticky sníží hello svazek, který je odeslán toohello portál odesláním pouze reprezentativní části události. Události, které jsou součástí hello stejné operace bude vybrán nebo nevybrány jako skupina, takže mohou procházet mezi souvisejícími událostmi. [Další informace o vzorkování.](app-insights-sampling.md)
>
>

### <a name="how-toosee-request-post-data"></a>Jak toosee POST data žádosti
Podrobnosti požadavku zahrnutí hello data jsou odeslaná aplikace tooyour volání POST. toohave nahlášené tato data:

* [Nainstalujte hello SDK](app-insights-asp-net.md) ve vašem projektu aplikace.
* Vložení kódu do vaší aplikace toocall [Microsoft.ApplicationInsights.TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace). V parametru hello zprávy odesílat hello POST data. Neexistuje omezení toohello povolená velikost, takže vyzkoušejte toosend jenom hello základní data.
* Při řešení chybné žádosti najdete hello přidružené trasování.  

![Procházení](./media/app-insights-asp-net-exceptions/060-req-related.png)

## <a name="exceptions"></a>Zachytávání výjimek a související diagnostických dat
Na první pohled neuvidíte portálu hello všechny hello výjimky, které způsobí selhání ve vaší aplikaci. Zobrazí všechny výjimky prohlížeče (Pokud používáte hello [JavaScript SDK](app-insights-javascript.md) na webových stránkách). Ale většina serveru výjimky jsou zachyceny službou IIS a máte toowrite kousek toosee kód je.

Můžete:

* **Protokolování výjimek explicitně** vložením kódu ve výjimce obslužné rutiny tooreport hello výjimky.
* **Zachytit výjimky automaticky** nakonfigurováním požadované rozhraní ASP.NET. potřebné dodatky Hello se liší pro různé typy framework.

## <a name="reporting-exceptions-explicitly"></a>Explicitně Reporting výjimky
Hello nejjednodušší způsob je tooinsert tooTrackException() volání v obslužné rutiny výjimek.

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

       // Send hello exception telemetry:
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

      ' Send hello exception telemetry:
      telemetry.TrackException(ex, properties, measurements)
    End Try

Hello vlastnosti a měření parametry jsou volitelné, ale jsou užitečné pro [filtrování a přidání](app-insights-diagnostic-search.md) doplňující informace. Například pokud máte aplikaci, která můžete spustit několik hry, může najít všechny hello výjimka sestavy související tooa konkrétní hra. Můžete přidat libovolný počet položek jako jste jako slovník tooeach.

## <a name="browser-exceptions"></a>Výjimky prohlížečů
Většina prohlížeče výjimek jsou hlášeny.

Pokud vaše webová stránka obsahuje soubory skriptu ze sítě pro doručování obsahu nebo v jiných doménách, ujistěte se, vaše značky script má atribut hello ```crossorigin="anonymous"```, a odešle tento server hello [hlavičky CORS](http://enable-cors.org/). To vám umožní tooget trasování zásobníku a podrobností pro neošetřených výjimek jazyka JavaScript z těchto prostředků.

## <a name="web-forms"></a>Webové formuláře
Pro webové formuláře hello modulu HTTP bude možné toocollect hello výjimky Pokud nejsou žádné přesměrování nakonfigurované CustomErrors.

Ale pokud máte aktivní přesměrování, přidejte následující řádky toohello Application_Error funkce v Global.asax.cs hello. (Pokud jste ještě nemáte, přidejte souboru Global.asax.)

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
Pokud hello [CustomErrors](https://msdn.microsoft.com/library/h0hfz6fc.aspx) konfigurace je `Off`, bude k dispozici pro hello výjimky [modulu HTTP](https://msdn.microsoft.com/library/ms178468.aspx) toocollect. Ale pokud je `RemoteOnly` (výchozí), nebo `On`, výjimka hello se odstraní a není k dispozici pro službu Application Insights tooautomatically shromažďovat. Můžete to opravíme přepsáním hello [System.Web.Mvc.HandleErrorAttribute třída](http://msdn.microsoft.com/library/system.web.mvc.handleerrorattribute.aspx)a použití třídy hello přepsat, jak je uvedeno pro hello různé MVC verze níže ([githubu zdroj](https://github.com/AppInsightsSamples/Mvc2UnhandledExceptions/blob/master/MVC2App/Controllers/AiHandleErrorAttribute.cs)):

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
                //If customError is Off, then AI HTTPModule will report hello exception
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
Atribut HandleError hello nahraďte váš nový atribut v řadičů.

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
        // Default replaced with hello override tootrack unhandled exceptions
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

Můžete přidat tento atribut přepsaného toospecific řadiče, nebo jeho přidání konfigurace globálních filtrů toohello do třídy WebApiConfig hello:

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

Existuje několik případů, které filtry výjimek hello nelze zpracovat. Například:

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

Přidejte tuto toohello služby WebApiConfig:

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

1. Nahraďte text hello pouze ExceptionHandler s vlastní implementaci IExceptionHandler. Volá se, jenom když hello framework je stále možné toochoose které odpovědi zprávy toosend (ne, pokud pro instanci přerušena hello připojení)
2. Filtry výjimek (jak je popsáno v části hello na řadičích 1.x webového rozhraní API výše) – ve všech případech není volána.

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

Přidání implementace služby toohello atribut hello:

    namespace WcfService4
    {
        [AiLogException]
        public class Service1 : IService1
        {
         ...

[Ukázka](https://github.com/AppInsightsSamples/WCFUnhandledExceptions)

## <a name="exception-performance-counters"></a>Čítače výkonu výjimky
Pokud máte [nainstalován hello agenta Application Insights](app-insights-monitor-performance-live-website-now.md) na serveru, můžete získat graf výjimky míry hello měří v rozhraní .NET. To zahrnuje zpracovávaný i neošetřené výjimky .NET.

Otevřete okno Průzkumníka metrika, přidejte nový graf a vyberte **výjimka míra**, uvedené v části čítače výkonu.

rozhraní .NET framework Hello vypočítá hello rychlost počítání hello počet výjimek v intervalu a vydělí hello délka intervalu hello.

Všimněte si, že bude liší od počtu hello výjimky vypočítána portálem Application Insights hello počítání TrackException sestavy. během intervalů vzorkování Hello se liší a hello SDK neodešle TrackException sestavy pro všechny zpracovávaný a neošetřené výjimky.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player] 

## <a name="next-steps"></a>Další kroky
* [Sledování REST, SQL a jiné toodependencies volání](app-insights-asp-net-dependencies.md)
* [Monitorování časů načtení stránky, výjimek prohlížeče a volání AJAX](app-insights-javascript.md)
* [Čítače sledování výkonu](app-insights-performance-counters.md)
