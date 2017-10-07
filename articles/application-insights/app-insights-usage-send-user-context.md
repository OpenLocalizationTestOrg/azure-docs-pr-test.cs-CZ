---
title: "využití tooenable kontext uživatele aaaSending vyskytne ve službě Azure Application Insights | Microsoft Docs"
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
ms.openlocfilehash: 0e6c2348f53a3ea970060334179b0dd070925e82
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
#  <a name="sending-user-context-tooenable-usage-experiences-in-azure-application-insights"></a>Vyskytne odesílání využití tooenable kontext uživatele ve službě Azure Application Insights

## <a name="tracking-users"></a>Sledování uživatelů

Application Insights umožňuje vám toomonitor a sledovat uživatele provede sadu nástrojů využití produktu: 
* [Uživatelé, relace, události](https://docs.microsoft.com/azure/application-insights/app-insights-usage-segmentation)
* [Trychtýře](https://docs.microsoft.com/azure/application-insights/usage-funnels)
* [Uchování](https://docs.microsoft.com/azure/application-insights/app-insights-usage-retention)
* Kohorty
* [Workbooks](https://docs.microsoft.com/azure/application-insights/app-insights-usage-workbooks)

V pořadí tootrack, co uživatel provede v čase Application Insights pro jednotlivé uživatele nebo relace potřebuje ID. Zahrňte tyto ID každé vlastní zobrazení události nebo stránky.
- Uživatelé, nálevky, uchovávání a kohorty: zahrnují ID uživatele.
- Relace: Zahrnují ID relace.

Pokud vaše aplikace je integrována hello [JavaScript SDK](https://docs.microsoft.com/azure/application-insights/app-insights-javascript#set-up-application-insights-for-your-web-page), automaticky sledován ID uživatele.

## <a name="choosing-user-ids"></a>Výběr ID uživatele

ID uživatele by měla zachovávají tootrack relace uživatele chování uživatelů v průběhu času. Existují různé přístupy pro zachování ID hello.
- Definice uživatele, který už máte ve službě.
- Služba hello má přístup tooa prohlížeče, pak může předat soubor cookie s ID hello prohlížeče v ní. Hello ID zachová pro soubor cookie hello zůstává v prohlížeči hello uživatele.
- V případě potřeby můžete použít nové ID každé relaci, ale výsledky hello o uživatelích, bude omezena. Například nebudete moct toosee jak v průběhu času mění chování uživatele.

Hello ID musí být identifikátor Guid nebo jiný řetězec dostatečně komplexní tooidentify každého uživatele jedinečné. Například to může být dlouho náhodné číslo.

Pokud hello ID obsahuje osobní identifikační informace o uživateli hello, není odpovídající hodnotu toosend tooApplication Insights jako ID uživatele. Můžete odeslat toto ID jako [ověřit ID uživatele](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#authenticated-users), ale nesplňuje požadavek na ID uživatele hello pro scénáře použití.

## <a name="aspnet-apps-set-user-context-in-an-itelemetryinitializer"></a>Aplikace ASP.NET: Nastavit kontext uživatele v ITelemetryInitializer

Vytvořte inicializátoru telemetrie, jak je podrobně popsaná v [zde](https://docs.microsoft.com/azure/application-insights/app-insights-api-filtering-sampling#add-properties-itelemetryinitializer)a nastavte hello Context.User.Id a hello Context.Session.Id.

Tento příklad nastaví hello uživatelské ID tooan identifikátor, který vyprší po hello relaci. Pokud je to možné použijte ID uživatele, která je uchována napříč relacemi.

*C#*

```C#

    using System;
    using System.Web;
    using Microsoft.ApplicationInsights.Channel;
    using Microsoft.ApplicationInsights.Extensibility;

    namespace MvcWebRole.Telemetry
    {
      /*
       * Custom TelemetryInitializer that sets hello user ID.
       *
       */
      public class MyTelemetryInitializer : ITelemetryInitializer
      {
        public void Initialize(ITelemetry telemetry)
        {
            // For a full experience, track each user across sessions. For an incomplete view of user 
            // behavior within a session, store user ID on hello HttpContext Session.
            // Set hello user ID if we haven't done so yet.
            if (HttpContext.Current.Session["UserId"] == null)
            {
                HttpContext.Current.Session["UserId"] = Guid.NewGuid();
            }

            // Set hello user id on hello Application Insights telemetry item.
            telemetry.Context.User.Id = (string)HttpContext.Current.Session["UserId"];

            // Set hello session id on hello Application Insights telemetry item.
            telemetry.Context.Session.Id = HttpContext.Current.Session.SessionID;
        }
      }
    }
```

## <a name="next-steps"></a>Další kroky
- využití tooenable vyskytne, zahájit odesílání [vlastních událostí](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) nebo [stránky zobrazení](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).
- Pokud už odesílat vlastní události nebo zobrazení stránky, prozkoumejte hello využití nástroje toolearn jak uživatelé používat služby.
    * [Přehled využití](app-insights-usage-overview.md)
    * [Uživatelů, relací a události](app-insights-usage-segmentation.md)
    * [Trychtýře](usage-funnels.md)
    * [Uchování](app-insights-usage-retention.md)
    * [Workbooks](app-insights-usage-workbooks.md)
