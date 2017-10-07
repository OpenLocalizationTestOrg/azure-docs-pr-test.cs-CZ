---
title: "aaaAzure Mobile Engagement Průvodce odstraňováním potíží - Analytics"
description: "Řešení potíží s analýzy, monitorování, segmentaci a řídicí panel v Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: 04a7020a-ad74-4491-be69-0bd574890029
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 69c6ff8f5c8540f8ba8b85b9ffec55acc59329fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-guide-for-analytics-monitoring-segmentation-and-dashboard-issues"></a>Průvodce řešením potíží pro problémy analýzy, monitorování, segmentaci a řídicí panel
Hello následují možných problémů se můžete setkat s jak Azure Mobile Engagement shromažďuje informace o aplikacích, zařízení a uživatelů.

## <a name="missingdelayed-information"></a>Chybějící nebo opožděná informace
### <a name="issue"></a>Problém
* V uvedených v analýzy, segmentace nebo řídicí panel je zpožděno informace.
* Chybí informace z monitorování.
* Chybí informace z analýzy, segmentace nebo řídicí panel.
* Nedosáhli limitů segmentace.

### <a name="causes"></a>Způsobí, že
* Můžete použít hello Analytics rozhraní API, rozhraní API monitorování, a rozhraní API segmenty toosee Pokud žádná data jste chybí hello uživatelského rozhraní je viditelná prostřednictvím hello rozhraní API.
* Pokud hello Azure Mobile Engagement SDK není správně integrovaná do aplikace nebudete moct toosee informace v hello Analytics segmentace, monitorování nebo řídicí panely.
* Segmenty nelze změnit po jejich vytvoření, segmenty může být pouze "klonovaný" (zkopírovat) nebo "zničen" (odstranit). Segmenty může obsahovat jenom 10 kritéria.
* Hello nejlepší způsob, jak tootest chybějící informace z monitorování je toosetup testovací zařízení, odinstalujte a znovu nainstalujte aplikaci hello na hello testovací zařízení.
* Informace se aktualizují každých 24 hodin pro analýzy, segmentace nebo řídicí panely.
* Informace v nové segmenty, se nemusí zobrazit do 24 hodin po jejich vytvoření i v případě hello segment je založena na předchozí informace.
* Filtrování dat analýzy v hello uživatelského rozhraní se zobrazí všechny příklady tohoto typu bez ohledu na verzi hello vaší aplikace (např.) "Dojde k chybě" filtrovat podle názvu se zobrazí z verze 1 a 2 verzi aplikace).
* Hello časové období pro analýzy je založena na hello datum z hello uživatelských nastavení zařízení, takže uživatel, jehož telefon je správně nastaveno datum hello může zobrazí v hello nesprávný časové období.
* Žádné na straně serveru, který data se protokolují, když použijete tlačítko hello příliš "test" nabízených oznámení, data se protokolují pouze pro skutečné nabízené kampaně.

## <a name="cant-locate-items-in-ui"></a>Nelze najít položky v uživatelském rozhraní
### <a name="issue"></a>Problém
* Nelze vytvořit segmenty založené na určité součástí nebo informace o aplikaci vlastní značky kritéria.
* Nelze najít určité součástí nebo informace o aplikaci vlastní značky kritéria analýzy, monitorování nebo řídicí panely.
* Nemůže interpretovat data hello analýzy, monitorování, segmentace nebo řídicí panely.

### <a name="causes"></a>Způsobí, že
* Některé položky součástí a informace o aplikaci značky jsou dostupné jen jako nabízené kritéria, ale nemusí být přidány tooa segmentu nebo viditelné z analýzy, monitorování nebo řídicí panel. 
* Pro integrovaný položky a informace o aplikaci značky, které nelze přidat tooa segmentu, budete potřebovat toosetup seznamu cílových kritérií pro každý hello tooperform kampaň stejné funkce jako cílení na základě v segmentu.
* V tématu hello kontextové nabídky v hello analýzy, monitorování, segmentaci a řídicí panely částech hello Azure Mobile Engagement uživatelského rozhraní pro další pomoc a jak tooinformation.

## <a name="crash-troubleshooting"></a>Řešení potíží s havárií
### <a name="issue"></a>Problém
* Aplikace spadne uvedených v analýzy, monitorování nebo řídicí panel.

### <a name="causes"></a>Způsobí, že
* tootroubleshoot aplikace spadne zobrazená v analýzy, monitorování nebo řídicího panelu zkontrolujte, zda toocheck hello poznámky k verzi pro známé problémy s předchozími verzemi hello SDK.
* řešení potíží s toofurther aplikace dojde k chybě provádění událost z testovací zařízení s instalaci aplikace a vyhledejte ID zařízení v části hello "monitorování – události" hello Azure Mobile Engagement uživatelského rozhraní. Potom proveďte hello událost, která je příčinou toocrash vaší aplikace a vyhledejte další informace v hello "Havárií – monitorování" části hello Azure Mobile Engagement uživatelského rozhraní. 

