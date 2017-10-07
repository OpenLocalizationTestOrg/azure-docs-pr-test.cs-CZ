---
title: "aaaSCOM integrace s nástrojem Application Insights | Microsoft Docs"
description: "Pokud si uživatelé SCOM, sledovat výkon a diagnostikovat problémy s nástrojem Application Insights. Komplexní řídicí panely, inteligentní výstrahy, výkonné diagnostické nástroje a analýzy dotazy."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 606e9d03-c0e6-4a77-80e8-61b75efacde0
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 08/12/2016
ms.author: bwren
ms.openlocfilehash: ee9ad462610fd916098a0e292c5bd44f2a873c10
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="application-performance-monitoring-using-application-insights-for-scom"></a>Sledování výkonu aplikací pomocí Application Insights pro SCOM
Pokud používáte System Center Operations Manager (SCOM) toomanage vašich serverů, můžete sledovat výkon a diagnostikovat problémy s výkonem hello pomoci [Azure Application Insights](app-insights-asp-net.md). Application Insights monitoruje webové aplikace příchozí požadavky a odchozí REST a volání SQL, výjimkami a protokolu trasování. Poskytuje řídicí panely pomocí metriky grafů a inteligentní výstrahy, a také výkonné diagnostické vyhledávání a analytické dotazy přes tuto telemetrii. 

Můžete přepnout na Application Insights monitorování pomocí sady management pack SCOM.

## <a name="before-you-start"></a>Než začnete
Předpokládáme:

* Jste obeznámeni s SCOM a že používáte SCOM 2012 R2 nebo 2016 toomanage vaší služby IIS webových serverů.
* Již jste nainstalovali na serverech webové aplikace, které chcete toomonitor s Application Insights.
* Je aplikace framework verze rozhraní .NET 4.5 nebo novější.
* Máte předplatné tooa přístup [Microsoft Azure](https://azure.com) a může podepsat v toohello [portál Azure](https://portal.azure.com). Vaše organizace může mít odběr a můžete přidat vaše tooit účet Microsoft.

(hello vývojový tým může vytvořit hello [Application Insights SDK](app-insights-asp-net.md) do hello webové aplikace. Tento čas sestavení instrumentace je dává větší flexibilitu při psaní vlastní telemetrii. Ale nezáleží: můžete provést kroky hello zde popsané s nebo bez hello SDK integrovanou.)

## <a name="one-time-install-application-insights-management-pack"></a>(Jednou) Instalovat sadu management pack Application Insights
Na počítači hello kde spuštění nástroje Operations Manager:

1. Odinstalujte všechny předchozí verzi sady hello management pack:
   1. V nástroji Operations Manager otevřete správy, sad Management Pack. 
   2. Odstraňte stará verze hello.
2. Stáhněte a nainstalujte hello management pack z katalogu hello.
3. Restartujte nástroj Operations Manager.

## <a name="create-a-management-pack"></a>Vytvořit sadu management pack
1. V nástroji Operations Manager, otevřete **vytváření**, **rozhraní .NET... with Application Insights**, **Průvodce přidáním monitorování**a znovu vyberte **rozhraní .NET... s aplikací Statistika**.
   
    ![](./media/app-insights-scom/020.png)
2. Konfigurace názvu hello po vaší aplikace. (Současně mít tooinstrument jednu aplikaci.)
   
    ![](./media/app-insights-scom/030.png)
3. Hello na stejné stránce průvodce, buď vytvořit novou správy aktualizací Service pack nebo vyberte balíček, který jste předtím vytvořili pro Application Insights.
   
     (hello Application Insights [sada management pack](https://technet.microsoft.com/library/cc974491.aspx) je šablona, ze kterého můžete vytvořit instanci. Můžete opakovaně použít stejnou instanci později hello.)

    ![Hello karta Obecné vlastnosti zadejte název hello aplikace hello. Klikněte na nový a zadejte název pro sadu management pack. Klikněte na tlačítko OK a potom klikněte na tlačítko Další.](./media/app-insights-scom/040.png)

1. Vyberte jednu aplikaci, které chcete toomonitor. Hello vyhledávací funkce hledá mezi aplikace nainstalované na serverech.
   
    ![Na kartě co tooMonitor klikněte na tlačítko Přidat, zadejte část názvu aplikace hello, klikněte na tlačítko Hledat, hello aplikaci a pak přidat, zvolte OK.](./media/app-insights-scom/050.png)
   
    Hello volitelné monitorování oboru pole lze použít toospecify podmnožinu vašim serverům, pokud nechcete, aby aplikace hello toomonitor ve všech serverech.
2. Na další stránce průvodce hello nejprve je nutné zadat vaše přihlašovací údaje toosign v tooMicrosoft Azure.
   
    Na této stránce zvolte prostředek Application Insights hello místo hello telemetrická data toobe analyzovat a zobrazit. 
   
   * Pokud aplikace hello bylo nakonfigurované pro službu Application Insights během vývoje, vyberte svůj existující prostředek.
   * Jinak vytvořte nový prostředek s názvem aplikace hello. Pokud existují další aplikace, které jsou součástí z hello stejném systému, jejich umístění v hello stejnou skupinu prostředků, toomake přístup toohello telemetrie jednodušší toomanage.
     
     Toto nastavení můžete později změnit.
     
     ![Na kartě Nastavení Application Insights klikněte na tlačítko "přihlášení" a zadejte přihlašovací údaje účtu Microsoft Azure. Potom vyberte předplatné, skupinu prostředků a prostředků.](./media/app-insights-scom/060.png)
3. Dokončení hello průvodce.
   
    ![Kliknutí na Vytvořit](./media/app-insights-scom/070.png)

Tento postup opakujte pro každou aplikaci, které chcete toomonitor.

Pokud budete později potřebovat toochange nastavení, znovu ho otevřete vlastnosti hello hello monitorování z okna hello vytváření obsahu.

![V vytváření obsahu, vyberte monitorování výkonu aplikací .NET pomocí nástroje Application Insights, vyberte monitoru a klikněte na položku Vlastnosti.](./media/app-insights-scom/080.png)

## <a name="verify-monitoring"></a>Ověřte monitorování
monitorování Hello nainstalovanou vyhledávání pro vaši aplikaci na každém serveru. Pokud zjistí aplikace hello konfiguruje aplikace hello toomonitor monitorování stavu Application Insights. V případě potřeby nainstaluje nejprve monitorování stavu na serveru hello.

Můžete ověřit, která instance aplikace hello najde:

![V monitorování, otevřete službu Application Insights](./media/app-insights-scom/100.png)

## <a name="view-telemetry-in-application-insights"></a>Zobrazení telemetrie Application Insights
V hello [portál Azure](https://portal.azure.com), procházet toohello prostředků pro vaši aplikaci. Můžete [zobrazili grafy znázorňující telemetrie](app-insights-dashboards.md) z vaší aplikace. (Pokud se ještě zobrazovat na hlavní stránku hello ještě, klikněte na živý datový proud metriky.)

## <a name="next-steps"></a>Další kroky
* [Nastavení řídicího panelu](app-insights-dashboards.md) toobring společně hello nejdůležitější grafy monitorování tato a další aplikace.
* [Další informace o metriky](app-insights-metrics-explorer.md)
* [Nastavení výstrah](app-insights-alerts.md)
* [Diagnostika problémů s výkonem](app-insights-detect-triage-diagnose.md)
* [Efektivní analytické dotazy](app-insights-analytics.md)
* [Testy dostupnosti webu](app-insights-monitor-web-app-availability.md)

