---
title: "aaaPortals pro vytváření a úpravy dotazů protokolu v Azure Log Analytics | Microsoft Docs"
description: "Tento článek popisuje hello portálů, můžete použít v Azure Log Analytics toocreate a upravovat vyhledávání protokolu."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: bwren
ms.openlocfilehash: 7a2657574a132b2c4298511bb31259e68113ac91
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="portals-for-creating-and-editing-log-queries-in-azure-log-analytics"></a>Portály pro vytváření a úpravy dotazů protokolu v Azure Log Analytics

> [!NOTE]
> Tento článek popisuje portály v Azure Log Analytics pomocí dotazovacího jazyka pro nové hello.  Můžete získat další informace o nový jazyk hello a získat hello postup tooupgrade pracovního prostoru v [Upgrade vyhledávání protokolu toonew pracovní prostor analýzy protokolů Azure](log-analytics-log-search-upgrade.md).  
>
> Pokud pracovní prostor nebyla upgradovaná toohello nové dotazovací jazyk, se seznamte s příliš[najít data pomocí protokolu hledání v analýzy protokolů](log-analytics-log-searches.md) informace o aktuální verzi portálu hledání protokolů hello hello.

Používáte protokol hledání v různých místech analýzy protokolů tooretrieve data z pracovního prostoru hello.  Ve skutečnosti vytváření a úpravy dotazů kromě tooworking interaktivně se ale vrátil data, máte dvě možnosti, které jsou popsány níže.  

## <a name="log-search-portal"></a>Protokol hledání portálu
portál hledání protokolů Hello je dostupné z hello portál Azure nebo na portálu OMS hello.  Je vhodné pro vytvoření základní dotazy, které lze vytvořit na jeden řádek.  portál hledání protokolů Hello lze použít bez spuštění externí portál a můžete ji použít tooperform celou řadu funkcí s protokolu hledání, včetně vytvoření pravidla výstrah, vytvoření skupin počítačů a export výsledků hello hello dotazu.  

portál hledání protokolů Hello nabízí více funkcí pro úpravy hello dotazu bez nutnosti úplné znalost dotazovacího jazyka pro hello.  Zobrazí souhrn těchto funkcí v [vytvořit protokolu hledání v Azure Log Analytics pomocí portálu hledání protokolů hello](log-analytics-log-search-log-search-portal.md).


![Protokol hledání portálu](media/log-analytics-log-search-portals/log-search-portal.png)

## <a name="advanced-analytics-portal"></a>Pokročilé analýzy portálu
portál pokročilou analýzu Hello je vyhrazené portál, který poskytuje pokročilé funkce není k dispozici hello hledání protokolů portálu.  Funkce patří možnost tooedit hello dotaz na více řádků, selektivně spuštění kódu, závislé na kontextu Intellisense a inteligentní analýzy.  portál pokročilé analýzy Hello je nejvhodnější pro návrh složitých dotazů, které jsou buď jako vyhledávání protokolu uložit nebo kopírovat a vkládat data do jiných elementů analýzy protokolů.  Spuštění portálu pokročilé analýzy hello pomocí odkazu v portálu hledání protokolů hello.

![Pokročilé analýzy portálu](media/log-analytics-log-search-portals/advanced-analytics-portal.png)


Z důvodu jeho pokročilé funkce obvykle použijete portál pokročilou analýzu hello jako svůj primární nástroj pro vytváření a úpravy dotazů.  Jakmile jste zjistili, že dotaz hello funguje podle očekávání, a potom budete zkopírujte a vložte ho jinde například portál hledání protokolů hello nebo Návrhář zobrazení.  Protože portál pokročilé analýzy hello ale podporuje dotazy s více řádky, potřebujete následující hello tootake v úvahu při kopírování dotazu z tento portál.

- Komentáře musí odebrat z dotazu hello před má zkopírovat a vložit do jiného umístění.  Můžete okomentovat řádek tak, že před s dvě lomítka (/ /).  Když vložíte více dotazu řádku do jednoho řádku, jsou odebrány konce řádků.  Pokud jsou zahrnuty komentáře, jsou všechny znaky po první komentář hello považovány za součást hello komentář.


## <a name="next-steps"></a>Další kroky

- Provede kurz k používání hello [hledání protokolů portál](log-analytics-log-search-log-search-portal.md) nebo hello [Advanced Analytics portál](https://go.microsoft.com/fwlink/?linkid=856587) toocreate dotazy.
- Podívejte se [kurz na zápis dotazů](https://go.microsoft.com/fwlink/?linkid=856078) pomocí dotazovacího jazyka pro nové hello.
