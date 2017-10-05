---
title: "Odesílání uživatelský kontext povolit použití možnosti v Azure Application Insights | Microsoft Docs"
description: "Sledovat, jak se uživatelé přesunou prostřednictvím služby po každé z nich přiřazení jedinečné, trvalé řetězec ID ve službě Application Insights."
services: application-insights
documentationcenter: 
author: abgreg
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: csharp
ms.topic: article
ms.date: 08/02/2017
ms.author: bwren
ms.openlocfilehash: 9350029c775643be0dcc679b0f4bb9238b5f8aca
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
#  <a name="sending-user-context-to-enable-usage-experiences-in-azure-application-insights"></a>Odesílání uživatelský kontext povolit použití možnosti v Azure Application Insights

## <a name="tracking-users"></a>Sledování uživatelů

Application Insights umožňuje monitorování a sledování uživatelům pomocí sady nástrojů pro použití produktu: 
* [Uživatelé, relace, události](https://docs.microsoft.com/azure/application-insights/app-insights-usage-segmentation)
* [Trychtýře](https://docs.microsoft.com/azure/application-insights/usage-funnels)
* [Uchování](https://docs.microsoft.com/azure/application-insights/app-insights-usage-retention)
* Kohorty
* [Workbooks](https://docs.microsoft.com/azure/application-insights/app-insights-usage-workbooks)

Aby bylo možné sledovat, co uživatel provede v čase, Application Insights pro jednotlivé uživatele nebo relace potřebuje ID. Zahrňte tyto ID každé vlastní zobrazení události nebo stránky.
- Uživatelé, nálevky, uchovávání a kohorty: zahrnují ID uživatele.
- Relace: Zahrnují ID relace.

Pokud vaše aplikace je integrována [JavaScript SDK](https://docs.microsoft.com/azure/application-insights/app-insights-javascript#set-up-application-insights-for-your-web-page), automaticky sledován ID uživatele.

## <a name="choosing-user-ids"></a>Výběr ID uživatele

ID uživatele by měla zachová napříč relacemi uživatele sledovat chování uživatele v průběhu času. Existují různé přístupy k zachování ID.
- Definice uživatele, který už máte ve službě.
- Služba má přístup do prohlížeče, pak může předat soubor cookie s ID prohlížeče v ní. ID pro se uchová, dokud zůstane soubor cookie v prohlížeči uživatele.
- V případě potřeby můžete použít nové ID každé relaci, ale výsledky o uživatelích, bude omezena. Například nebudete moci zobrazit, jak v průběhu času mění chování uživatele.

ID musí být identifikátor Guid nebo jiný řetězec dostatečně složité, se jednoznačně označit každého uživatele. Například to může být dlouho náhodné číslo.

Pokud ID obsahuje osobní identifikační informace o uživateli, se nejedná o správnou hodnotu k odeslání do Application Insights jako ID uživatele. Můžete odeslat toto ID jako [ověřit ID uživatele](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#authenticated-users), ale nesplňuje požadavek na ID uživatele pro scénáře použití.

## <a name="aspnet-apps-set-user-context-in-an-itelemetryinitializer"></a>Aplikace ASP.NET: Nastavit kontext uživatele v ITelemetryInitializer

Vytvořte inicializátoru telemetrie, jak je podrobně popsaná v [zde](https://docs.microsoft.com/azure/application-insights/app-insights-api-filtering-sampling#add-properties-itelemetryinitializer)a nastavte Context.User.Id a Context.Session.Id.

Tento příklad nastaví na identifikátor, který vyprší po relaci ID uživatele. Pokud je to možné použijte ID uživatele, která je uchována napříč relacemi.

*C#*

```C#

    using System;
    using System.Web;
    using Microsoft.ApplicationInsights.Channel;
    using Microsoft.ApplicationInsights.Extensibility;

    namespace MvcWebRole.Telemetry
    {
      /*
       * Custom TelemetryInitializer that sets the user ID.
       *
       */
      public class MyTelemetryInitializer : ITelemetryInitializer
      {
        public void Initialize(ITelemetry telemetry)
        {
            // For a full experience, track each user across sessions. For an incomplete view of user 
            // behavior within a session, store user ID on the HttpContext Session.
            // Set the user ID if we haven't done so yet.
            if (HttpContext.Current.Session["UserId"] == null)
            {
                HttpContext.Current.Session["UserId"] = Guid.NewGuid();
            }

            // Set the user id on the Application Insights telemetry item.
            telemetry.Context.User.Id = (string)HttpContext.Current.Session["UserId"];

            // Set the session id on the Application Insights telemetry item.
            telemetry.Context.Session.Id = HttpContext.Current.Session.SessionID;
        }
      }
    }
```

## <a name="next-steps"></a>Další kroky
- Pokud chcete povolit použití možnosti, zahájit odesílání [vlastních událostí](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) nebo [stránky zobrazení](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).
- Pokud jste již odeslat vlastní události nebo zobrazení stránky, prozkoumejte využití nástroje se dozvíte, jak uživatelé používat služby.
    * [Přehled využití](app-insights-usage-overview.md)
    * [Uživatelů, relací a události](app-insights-usage-segmentation.md)
    * [Trychtýře](usage-funnels.md)
    * [Uchování](app-insights-usage-retention.md)
    * [Workbooks](app-insights-usage-workbooks.md)
