---
title: "aaaApplication mapy ve službě Azure Application Insights | Microsoft Docs"
description: "Vizuální prezentaci hello závislostí mezi součástmi aplikace označeny klíčových ukazatelů výkonu a výstrahy."
services: application-insights
documentationcenter: 
author: SoubhagyaDash
manager: carmonm
ms.assetid: 3bf37fe9-70d7-4229-98d6-4f624d256c36
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: 96ab753a100ea53ec7d367e3559b6622ab6fd182
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="application-map-in-application-insights"></a>Mapa aplikace ve službě Application Insights
V [Azure Application Insights](app-insights-overview.md), mapa aplikace je visual rozložení vztahů závislosti hello komponent vaší aplikace. Jednotlivé komponenty zobrazuje klíčových ukazatelů výkonu například zatížení, výkonu, chyb a upozornění, toohelp že zjistit všechny součásti, která způsobila problém výkonu nebo selhání. Můžete kliknout na prostřednictvím z jakékoli součásti toomore podrobné diagnostiky, jako je například události Application Insights. Pokud vaše aplikace používá služby Azure, můžete také kliknout na prostřednictvím diagnostics tooAzure, jako je například doporučení Poradce pro databáze SQL.

Jako další grafy budete moct připnout aplikaci mapy toohello řídicí panel Azure, kde je plně funkční. 

## <a name="open-hello-application-map"></a>Otevřete hello aplikace mapy
Otevřete hello mapy v okně Přehled hello pro vaši aplikaci:

![Otevřete aplikaci mapy](./media/app-insights-app-map/01.png)

![Mapa aplikace](./media/app-insights-app-map/02.png)

Hello mapa znázorňuje:

* Testy dostupnosti
* Součásti klientské strany (monitorovat pomocí hello JavaScript SDK)
* Klientská součást produktu
* Závislosti hello součásti klienta a serveru

Můžete rozbalit nebo sbalit skupiny odkaz závislost:

![Sbalit](./media/app-insights-app-map/03.png)

Pokud máte velký počet závislostí jednoho typu (SQL, HTTP atd.), se mohou objevit seskupené. 

![seskupené závislosti](./media/app-insights-app-map/03-2.png)

## <a name="spot-problems"></a>Identifikaci problémů
Každý uzel má ukazatele relevantní výkonu, jako je například hello zatížení, výkonu a selhání sazby za tuto součást. 

Ikony upozornění upozorňují na možné problémy. Oranžové upozornění znamená, že tam jsou chyby v žádostech, zobrazení stránek nebo volání závislostí. Red se rozumí míra selhání výše 5 %. Pokud chcete tooadjust tyto prahové hodnoty, otevřete panel Možnosti.

![selhání ikony](./media/app-insights-app-map/04.png)

Aktivní výstrahy také zobrazit nahoru: 

![aktivní výstrahy](./media/app-insights-app-map/05.png)

Pokud používáte SQL Azure, je ikonu, která ukazuje, kdy jsou doporučení na tom, jak může zlepšit výkon. 

![Azure doporučení](./media/app-insights-app-map/06.png)

Klikněte na možnost žádné ikonu tooget další podrobnosti:

![Azure doporučení](./media/app-insights-app-map/07.png)

## <a name="diagnostic-click-through"></a>Diagnostické klikněte na tlačítko prostřednictvím
Každý z uzlů hello na mapě hello nabízí cílové kliknutím prostřednictvím pro diagnostiku. Hello možnosti se liší v závislosti na typu hello hello uzlu.

![Možnosti serveru](./media/app-insights-app-map/09.png)

Pro součásti, které jsou hostované v Azure hello možnosti zahrnují toothem přímé odkazy.

## <a name="filters-and-time-range"></a>Filtry a časový rozsah
Ve výchozím nastavení shrnuje hello mapy všechna data hello k dispozici pro zvolené období hello. Ale můžete filtrovat, názvy pouze konkrétní operaci tooinclude nebo závislosti.

* Název operace: Jedná se o zobrazení stránky a typy požadavků na straně serveru. Pomocí této možnosti hello mapa zobrazuje hello klíčového ukazatele výkonu na uzlu serveru nebo klientské hello pouze operacím hello vybrané. Zobrazuje hello závislosti volat v kontextu hello těchto konkrétních operací.
* Název základní závislosti: Jedná se o hello AJAX prohlížeče závislosti a závislosti na straně serveru. Pokud sestavu vlastní závislosti telemetrie s hello TrackDependency API zároveň jsou zde. Můžete vybrat hello tooshow závislosti na mapě hello. Tento výběr aktuálně nefiltruje požadavků na straně serveru hello nebo hello zobrazení stránky na straně klienta.

![Nastavení filtrů](./media/app-insights-app-map/11.png)

## <a name="save-filters"></a>Uložit filtry
filtry hello toosave jste použili, kódu pin hello filtrované zobrazení na [řídicí panel](app-insights-dashboards.md).

![Toodashboard PIN kód](./media/app-insights-app-map/12.png)

## <a name="error-pane"></a>Podokno chyby
Při kliknutí na uzel v mapě hello, podokně aplikace chyba se zobrazí na pravé straně hello shrnutí selhání pro tento uzel. Chyby jsou nejprve seskupené podle ID operace a potom seskupené podle ID problému.

![Podokno chyby](./media/app-insights-app-map/error-pane.png)

Kliknutím na selhání přejdete toohello poslední instance tohoto selhání.

## <a name="resource-health"></a>Stav prostředků
Pro některé typy prostředků v hello horní části podokna hello chyba se zobrazí stav prostředku. Například kliknutím na uzel SQL se zobrazí stav databáze hello a všechny výstrahy, které mají aktivováno.

![Stav prostředků](./media/app-insights-app-map/resource-health.png)

Můžete kliknout na hello prostředků název tooview standardní přehled metriky pro tento prostředek.

## <a name="end-to-end-system-app-maps"></a>Systém začátku do konce aplikace mapy

*Vyžaduje SDK verze 2.3 nebo vyšší*

Pokud aplikace obsahuje několik komponenty, například k back endové službě kromě toohello webové aplikace - pak můžete zobrazit je všechny na mapě jednu integrované aplikaci.

![Nastavení filtrů](./media/app-insights-app-map/multi-component-app-map.png)

Mapa aplikace Hello vyhledá uzly serveru pomocí následujících závislostí HTTP volání mezi servery s hello nainstalované Application Insights SDK. Každý prostředek Application Insights se předpokládá toocontain jeden server.

### <a name="multi-role-app-map-preview"></a>Aplikace služby role mapy (preview)

Hello preview aplikace s více role mapy funkce umožňuje mapy aplikace hello toouse s více servery odesílání dat toohello stejný prostředek Application Insights nebo klíč instrumentace. Servery v mapě hello jsou oddělených hello cloud_RoleName vlastnost telemetrie položky. Nastavit *mapování aplikace s více rolí* příliš*na* z hello verze Preview okno tooenable tuto konfiguraci.

Tento přístup může požadovaných, v aplikaci micro-services, nebo v jiných scénářích, kde se mají události toocorrelate na více serverech v rámci jednoho prostředku Application Insights.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player] 

## <a name="feedback"></a>Váš názor
Zadejte prosím zpětnou vazbu prostřednictvím možnosti hello portálu zpětné vazby.

![Obrázek MapLink-1](./media/app-insights-app-map/13.png)


## <a name="next-steps"></a>Další kroky

* [Azure Portal](https://portal.azure.com)
