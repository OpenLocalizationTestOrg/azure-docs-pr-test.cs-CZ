---
title: "aaaGet další mimo Azure Application Insights | Microsoft Docs"
description: "Po Začínáme se službou Application Insights, zde je souhrn hello funkcí, které můžete prozkoumat."
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
ms.openlocfilehash: 2023728afcf5aa5ecab8b957c8517d4872668765
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="more-telemetry-from-application-insights"></a>Další telemetrie z Application Insights
Až budete mít [přidat kódu ASP.NET Application Insights tooyour](app-insights-asp-net.md), existuje několik způsobů, jak tooget i další telemetrie. 

| Akce | Co získáte|
|---|---|
|(Servery služby IIS) [Nainstalujte monitorování stavu](http://go.microsoft.com/fwlink/?LinkId=506648) na každý počítač serveru.<br/>(Službě azure web apps) Hello Azure ovládacího panelu pro hello webovou aplikaci otevřete okno Application Insights hello.| [**Čítače výkonu**](app-insights-performance-counters.md)<br/>[**Výjimky** ](app-insights-asp-net-exceptions.md) – podrobné trasování zásobníku<br/>[**Závislosti**](app-insights-asp-net-dependencies.md)|
|[Přidat hello JavaScript fragment kódu tooyour webové stránky](app-insights-javascript.md)|[Stránka výkonu](app-insights-web-track-usage.md), výjimky prohlížeče, výkon AJAX. Vlastní telemetrii na straně klienta.|
|[Vytvoření testy dostupnosti webu](app-insights-monitor-web-app-availability.md)|Zasílání upozornění, pokud váš web přestane být k dispozici|
|[Ujistěte se, buildinfo.config](https://msdn.microsoft.com/library/dn449058.aspx) je generován nástroje MSBuild|[Sestavení annotationsin metriky grafy](https://blogs.msdn.microsoft.com/visualstudioalm/2013/11/14/implementing-deployment-markers-in-application-insights/)
|[Zápis vlastní události a metriky](app-insights-api-custom-events-metrics.md)|Počet obchodní události a metriky, sledovat podrobné informace o použití a další.|
|[Profil živý web](https://aka.ms/AIProfilerPreview)|Podrobné funkce časování z svou živou webovou aplikaci|






