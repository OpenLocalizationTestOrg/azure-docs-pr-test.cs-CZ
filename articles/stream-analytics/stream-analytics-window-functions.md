---
title: "funkce analýzy okno tooStream aaaIntroduction | Microsoft Docs"
description: "Další informace o hello tři okno funkce v Stream Analytics (přeskakujícího, vše, klouzavé)."
keywords: "přeskakujícího okna, posuvném okně, posílání okna"
documentationcenter: 
services: stream-analytics
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 0d8d8717-5d23-43f0-b475-af078ab4627d
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: 7fc36eb9afb2b28e2791d925d26923145eb38074
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toostream-analytics-window-functions"></a>Funkce analýzy okno tooStream Úvod
V mnoha reálném čase streamování scénáře je nutné tooperform operace pouze na hello data obsažená v dočasné systému windows. Nativní podpora pro oddílová funkce je klíčovou funkcí Azure Stream Analytics, která přemísťuje hello ručička na vývojáře produktivitu při vytváření úlohy zpracování komplexní datového proudu. Stream Analytics umožňuje vývojářům toouse [ **Přeskakujícího**](https://msdn.microsoft.com/library/dn835055.aspx), [ **Hopping** ](https://msdn.microsoft.com/library/dn835041.aspx) a [ **posuvné** ](https://msdn.microsoft.com/library/dn835051.aspx) dočasné operace tooperform systému windows na datový proud. Je to, že všechny [okno](https://msdn.microsoft.com/library/dn835019.aspx) operations výstup výsledků v hello **end** okna hello. výstup Hello okna hello bude jedinou událost podle hello používá agregační funkci. Hello událost může mít hello časové razítko konce hello okna hello a všechny funkce okna jsou definovány s pevnou délkou. Nakonec je důležité toonote, který se má použít všechny funkce okna v [ **GROUP BY** ](https://msdn.microsoft.com/library/dn835023.aspx) klauzule.

![Stream Analytics okno funkce koncepty](media/stream-analytics-window-functions/stream-analytics-window-functions-conceptual.png)

## <a name="tumbling-window"></a>Přeskakující okno
Přeskakující okno funkce jsou použité toosegment datový proud do různých čas segmentů a provádět funkce proti, jako je například následující příklad hello. Hello klíče differentiators z Přeskakující okno se, že zopakují, se nepřekrývají, a událost nemůže patřit toomore než jeden přeskakující okno.

![Funkce okno analýzy datového proudu přeskakujícího Úvod](media/stream-analytics-window-functions/stream-analytics-window-functions-tumbling-intro.png)

## <a name="hopping-window"></a>Skákající okno
Skákající okno funkce směrování dál v čase prostřednictvím uplynutí určité doby. To může být snadno toothink z nich jako Přeskakující windows, které může dojít k překrytí, tak události můžou patřit toomore než jednu sadu výsledků Hopping okno. toomake okno Hopping hello stejné jako Přeskakující okno jednoduše by zadejte toobe velikost skoku hello hello stejná jako velikost okna hello. 

![Stream Analytics okno funkce skákající Úvod](media/stream-analytics-window-functions/stream-analytics-window-functions-hopping-intro.png)

## <a name="sliding-window"></a>Posuvné okno
Posuvné okno funkce, na rozdíl od Přeskakujícího nebo posílání windows, vytvořit výstup **pouze** při výskytu události. Každý okno bude mít aspoň jednu událost a okno hello nepřetržitě přesune dál € (Epsilon –). Jako posílání Windows může patřit události toomore než jeden klouzavé interval.

![Stream Analytics okno funkce klouzavé Úvod](media/stream-analytics-window-functions/stream-analytics-window-functions-sliding-intro.png)

## <a name="getting-help-with-window-functions"></a>Získání nápovědy k okno funkce
Další podporu naleznete v našem [fóru služby Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Další kroky
* [Úvod tooAzure Stream Analytics](stream-analytics-introduction.md)
* [Začínáme používat službu Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Škálování služby Stream Analytics](stream-analytics-scale-jobs.md)
* [Referenční příručka k jazyku Azure Stream Analytics Query Language](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)

