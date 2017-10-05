---
title: "Získat další mimo Azure Application Insights | Microsoft Docs"
description: "Po Začínáme se službou Application Insights, zde je souhrn funkcí, které můžete prozkoumat."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: 7ec10a2d-c669-448d-8d45-b486ee32c8db
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 02/03/2017
ms.author: bwren
ms.openlocfilehash: 127fd6e3012bdb0788ed23ae5e8921df651d863b
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="more-telemetry-from-application-insights"></a>Další telemetrie z Application Insights
Až budete mít [přidat Application Insights do kódu ASP.NET](app-insights-asp-net.md), existuje několik způsobů, jak získat i další telemetrie. 

| Akce | Co získáte|
|---|---|
|(Servery služby IIS) [Nainstalujte monitorování stavu](http://go.microsoft.com/fwlink/?LinkId=506648) na každý počítač serveru.<br/>(Službě azure web apps) V Azure ovládacích panelech webové aplikace otevřete okno Application Insights.| [**Čítače výkonu**](app-insights-performance-counters.md)<br/>[**Výjimky** ](app-insights-asp-net-exceptions.md) – podrobné trasování zásobníku<br/>[**Závislosti**](app-insights-asp-net-dependencies.md)|
|[Přidat fragment kódu JavaScript do webové stránky](app-insights-javascript.md)|[Stránka výkonu](app-insights-web-track-usage.md), výjimky prohlížeče, výkon AJAX. Vlastní telemetrii na straně klienta.|
|[Vytvoření testy dostupnosti webu](app-insights-monitor-web-app-availability.md)|Zasílání upozornění, pokud váš web přestane být k dispozici|
|[Ujistěte se, buildinfo.config](https://msdn.microsoft.com/library/dn449058.aspx) je generován nástroje MSBuild|[Sestavení annotationsin metriky grafy](https://blogs.msdn.microsoft.com/visualstudioalm/2013/11/14/implementing-deployment-markers-in-application-insights/)
|[Zápis vlastní události a metriky](app-insights-api-custom-events-metrics.md)|Počet obchodní události a metriky, sledovat podrobné informace o použití a další.|
|[Profil živý web](https://aka.ms/AIProfilerPreview)|Podrobné funkce časování z svou živou webovou aplikaci|






