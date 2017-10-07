---
title: "aaaAzure aplikace Insights Telemetrie datový Model - kontextu Telemetrie | Microsoft Docs"
description: "Application Insights telemetrie kontextu datový model"
services: application-insights
documentationcenter: .net
author: SergeyKanzhelev
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 05/15/2017
ms.author: sergkanz
ms.openlocfilehash: 6cdd6240d1c448f883d104a871ee9d5f6b5af2ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="telemetry-context-application-insights-data-model"></a>Kontext telemetrie: Application Insights datový model

Každá položka telemetrie může mít pole silného typu kontextu. Každé pole umožňuje určitého scénáře monitorování. Použijte hello vlastní vlastnosti kolekce toostore vlastní nebo specifické pro aplikaci kontextové informace.


##<a name="application-version"></a>Verze aplikace

Informace v pole kontextu hello aplikace je vždy o hello aplikaci, která odesílá telemetrii hello. Verze aplikace je použité tooanalyze trend změny v chování aplikace hello a její nasazení toohello korelace.

Maximální délka: 1024


##<a name="client-ip-address"></a>IP adresa klienta

IP adresa Hello hello klientského zařízení. IPv4 a IPv6 jsou podporovány. Při odesílání telemetrických dat ze služby hello umístění kontext je o hello uživateli, který spustil hello operace ve službě hello. Application Insights extrahovat informace hello geografického umístění, z IP adresy klienta hello a pak ho zkrátit. IP adresa klienta samostatně proto nelze použít jako osobní údaje koncového uživatele. 

Maximální délka: 46


##<a name="device-type"></a>Typ zařízení

Původně byl použit v tomto poli tooindicate hello typ hello zařízení hello koncový uživatel aplikace hello používá. Dnes použít především telemetrie JavaScript toodistinguish s hello typ zařízení, prohlížeč, z telemetrických dat na straně serveru s typem zařízení hello "Počítač".

Maximální délka: 64


##<a name="operation-id"></a>Id operace

Jedinečný identifikátor hello kořenové operace. Tento identifikátor umožňuje toogroup telemetrie napříč více součástí. V tématu [telemetrie korelace](application-insights-correlation.md) podrobnosti. id operace Hello vytvoří žádost o nebo zobrazení stránky. Všechny další telemetrií nastaví hodnotu tohoto pole toohello pro hello obsahující zobrazení požadavku nebo stránky. 

Maximální délka: 128


##<a name="parent-operation-id"></a>ID nadřazené operace

Jedinečný identifikátor okamžitou nadřazené položce telemetrie hello Hello. V tématu [telemetrie korelace](application-insights-correlation.md) podrobnosti.

Maximální délka: 128


##<a name="operation-name"></a>Název operace

Hello název operace hello (skupiny). Název operace Hello vytvoří žádost o nebo zobrazení stránky. Všechny ostatní položky telemetrie nastavit hodnotu tohoto pole toohello pro hello obsahující zobrazení požadavku nebo stránky. Název operace se používá pro hledání všech hello telemetrie položek pro skupinu operací (například "GET domovské/indexem"). Tato vlastnost kontextu je použité tooanswer otázky typu "jaké jsou typické výjimky hello vydané na této stránce."

Maximální délka: 1024


##<a name="synthetic-source-of-hello-operation"></a>Syntetické zdroj operace hello

Název syntetické zdroje. Nějaké telemetrie z aplikace hello může představovat syntetické provoz. Může být web prohledávacího modulu indexování hello webu, testy dostupnosti webu nebo trasování z diagnostiky knihoven jako Application Insights SDK sám sebe.

Maximální délka: 1024


##<a name="session-id"></a>Id relace

ID relace - hello instanci hello uživatelské interakce s aplikací hello. Informace v polích kontextu relace hello je vždy o hello koncového uživatele. Při odesílání telemetrických dat ze služby kontextu relace hello je o hello uživateli, který spustil hello operace ve službě hello.

Maximální délka: 64


##<a name="anonymous-user-id"></a>Id anonymní uživatele

Id anonymního uživatele. Představuje hello koncový uživatel aplikace hello. Při odesílání telemetrických dat ze služby, o hello uživateli, který spustil hello operace ve službě hello je hello uživatelský kontext.

[Vzorkování](app-insights-sampling.md) je jedním z hello techniky toominimize hello objem shromážděných telemetrie. Vzorkování algoritmus pokusy o tooeither ukázka příchozí nebo odchozí všechny hello korelační telemetrie. Id anonymní uživatele se používá pro generování skóre vzorkování. Takže id anonymní uživatele by mělo být dostatečně náhodná hodnota. 

Pomocí anonymního uživatele id toostore uživatelského jména je zneužití hello pole. Pomocí id uživatele ověřený.

Maximální délka: 128


##<a name="authenticated-user-id"></a>Id ověřeného uživatele

Id ověřeného uživatele. Hello opačným id anonymního uživatele, toto pole představuje hello uživatele s popisným názvem. Od jeho PII informace nejsou shromažďovány ve výchozím nastavení většina SDK.

Maximální délka: 1024


##<a name="account-id"></a>Id účtu

V aplikacích víceklientské jde hello účet ID nebo názvem, který uživatel hello funguje s. Příklady může být ID předplatného pro Azure portal nebo blog název platforma blogu.

Maximální délka: 1024


##<a name="cloud-role"></a>Role cloudu

Název aplikace hello hello role je součástí. Mapuje přímo toohello název role v azure. Může být také použít toodistinguish malých služeb, které jsou součástí jedné aplikace.

Maximální délka: 256


##<a name="cloud-role-instance"></a>Instance role cloudu

Název instance text hello, kde je spuštěna aplikace hello. Název počítače pro místní, název instance pro Azure.

Maximální délka: 256


##<a name="internal-sdk-version"></a>Interní: Verze sady SDK

Verze sady SDK. V tématu https://github.com/Microsoft/ApplicationInsights-Home/blob/master/SDK-AUTHORING.md#sdk-version-specification informace.

Maximální délka: 64


##<a name="internal-node-name"></a>Interní: Název uzlu

Toto pole představuje název uzlu hello používá pro účely fakturace. Použijte toooverride hello standardní detekce uzlů.

Maximální délka: 256


## <a name="next-steps"></a>Další kroky

- Zjistěte, jak příliš[rozšířit a filtrovat telemetrie](app-insights-api-filtering-sampling.md).
- V tématu [datový model](application-insights-data-model.md) Application Insights typy a data modelu.
- Podívejte se na kolekci vlastností kontextu standardní [konfigurace](app-insights-configuration-with-applicationinsights-config.md#telemetry-initializers-aspnet).
