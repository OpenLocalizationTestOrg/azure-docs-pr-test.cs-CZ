---
title: "událost aaaReal čas zpracování pomocí zpracování událostí Stream Analytics | Microsoft Docs"
description: "Zjistěte, jak můžou spolupracovat sadu Azure services pro povolení zpracování událostí v reálném čase a analýzy."
keywords: "zpracování v reálném čase, událostí zpracování, referenční architektura"
services: stream-analytics,event-hubs,storage,sql-database
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: 
ms.assetid: 11af48bc-313c-4527-8c80-91088dc9f3c6
ms.service: stream-analytics
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2017
ms.author: jeffstok
ms.openlocfilehash: a43c503d709609ba61e9932822d30bc2208906ab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="reference-architecture-real-time-event-processing-with-microsoft-azure-stream-analytics"></a>Referenční architektura: událostí v reálném čase zpracování pomocí služby Microsoft Azure Stream Analytics
Hello referenční architektura pro zpracování pomocí služby Azure Stream Analytics událostí v reálném čase je určený tooprovide obecný plán pro nasazení v reálném čase platforma jako služba (PaaS) řešení zpracování datového proudu s Microsoft Azure.

## <a name="summary"></a>Souhrn
Obvyklým řešení pro analýzu měla vycházet z funkcí, jako je ETL (extrakce, transformace, zatížení) a datových skladů, kde data jsou uložené před tooanalysis. Změna požadavky, včetně další data rychle které nabízíte tento existující toohello limit modelu. Hello možnost tooanalyze dat v rámci přesunutí datové proudy předchozí toostorage je jedno řešení a i když není novou funkci, hello přístup nebyl přijat široce mezi všechny pocházejícími odvětví. 

Microsoft Azure poskytuje katalog rozsáhlé analytics technologií, které podporují řadu řešení pro různé scénáře a požadavky na podporu. Vyberte, které toodeploy služby Azure pro-komplexní řešení může být složité zadané hello spektra nabídek. Tento dokument je navrženou toodescribe hello funkce a vzájemná spolupráce z hello různé služby Azure, které podporují řešení vysílání datového proudu událostí. Také vysvětluje některé hello scénáře, ve kterých zákazníci využívat tento typ přístupu.

## <a name="contents"></a>Obsah
* Shrnutí
* Analýza tooReal čas Úvod
* Nabízená hodnota dat v reálném čase v Azure
* Časté scénáře pro analýzu v reálném čase
* Architektura a komponenty
  * Zdroje dat
  * Vrstva integraci dat.
  * Vrstva analýzu v reálném čase
  * Vrstvy úložiště dat
  * Prezentace / spotřeba vrstvy
* Závěr

**Autor:** Charlese Feddersen, architekt řešení datového centra Statistika vynikající výsledky, společnosti Microsoft Corporation.

**Publikovat:** leden 2015

**Revize:** 1.0

**Stáhnout:** [událostí v reálném čase zpracování pomocí služby Microsoft Azure Stream Analytics](http://download.microsoft.com/download/6/2/3/623924DE-B083-4561-9624-C1AB62B5F82B/real-time-event-processing-with-microsoft-azure-stream-analytics.pdf)

## <a name="get-help"></a>Podpora
Další podporu naleznete v našem [fóru služby Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Další kroky
* [Úvod tooAzure Stream Analytics](stream-analytics-introduction.md)
* [Začínáme používat službu Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Škálování služby Stream Analytics](stream-analytics-scale-jobs.md)
* [Referenční příručka k jazyku Azure Stream Analytics Query Language](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)

