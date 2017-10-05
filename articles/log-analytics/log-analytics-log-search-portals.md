---
title: "Portály pro vytváření a úpravy dotazů protokolu v Azure Log Analytics | Microsoft Docs"
description: "Tento článek popisuje portály, které můžete použít v Azure Log Analytics můžete vytvářet a upravovat protokolu hledání."
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
ms.openlocfilehash: 29dfa31d38f85574f84ed351bc5c26224b1a7e16
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="portals-for-creating-and-editing-log-queries-in-azure-log-analytics"></a>Portály pro vytváření a úpravy dotazů protokolu v Azure Log Analytics

> [!NOTE]
> Tento článek popisuje portály v Azure Log Analytics jazykem nový dotaz.  Můžete získat další informace o nový jazyk a získat postup upgradu pracovního prostoru v [upgradu pracovní prostor analýzy protokolů Azure na nové hledání protokolu](log-analytics-log-search-upgrade.md).  
>
> Pokud pracovní prostor nebyla upgradována, aby nové dotazovací jazyk, se seznamte s [najít data pomocí protokolu hledání v analýzy protokolů](log-analytics-log-searches.md) informace o aktuální verzi portálu hledání protokolů.

Protokol hledání v různých místech analýzy protokolů se použít k načtení dat z pracovního prostoru.  Ve skutečnosti vytváření a úpravy dotazů kromě práce interaktivně se vrácená data, když, máte dvě možnosti, které jsou popsány níže.  

## <a name="log-search-portal"></a>Protokol hledání portálu
Portál hledání protokolů je dostupný z portálu Azure nebo na portálu OMS.  Je vhodné pro vytvoření základní dotazy, které lze vytvořit na jeden řádek.  Portálu hledání protokolů můžete používat bez spuštění externí portál a slouží k provádění různých funkcí s protokolu hledání, včetně vytvoření pravidla výstrah, vytvoření skupin počítačů a export výsledků dotazu.  

Hledání protokolů portál nabízí více funkcí pro úpravy dotazu bez nutnosti úplné znalost dotazovacího jazyka.  Zobrazí souhrn těchto funkcí v [vytvořit protokolu hledání v Azure Log Analytics pomocí portálu hledání protokolů](log-analytics-log-search-log-search-portal.md).


![Protokol hledání portálu](media/log-analytics-log-search-portals/log-search-portal.png)

## <a name="advanced-analytics-portal"></a>Pokročilé analýzy portálu
Portálu pro pokročilou analýzu je vyhrazené portál, který poskytuje pokročilé funkce není k dispozici na portálu hledání protokolů.  Funkce zahrnují možnost upravit dotaz na více řádků, selektivně spuštění kódu, závislé na kontextu Intellisense a inteligentní analýzy.  Portál Advanced Analytics je nejvhodnější pro návrh složitých dotazů, které jsou buď jako vyhledávání protokolu uložit nebo kopírovat a vkládat data do jiných elementů analýzy protokolů.  Spuštění portálu Advanced Analytics z odkaz na portálu hledání protokolů.

![Pokročilé analýzy portálu](media/log-analytics-log-search-portals/advanced-analytics-portal.png)


Z důvodu jeho pokročilé funkce obvykle použijete portálu pokročilou analýzu jako svůj primární nástroj pro vytváření a úpravy dotazů.  Jednou jste zjistili, že dotaz funguje podle očekávání, a potom budete zkopírujte a vložte ho jinde například hledání protokolů portálu nebo Návrhář zobrazení.  Protože portál Advanced Analytics podporuje dotazy s více řádky, i když je nutné vzít v úvahu následující při kopírování dotazu z tento portál.

- Komentáře musí odebrat z dotazu před má zkopírovat a vložit do jiného umístění.  Můžete okomentovat řádek tak, že před s dvě lomítka (/ /).  Když vložíte více dotazu řádku do jednoho řádku, jsou odebrány konce řádků.  Pokud jsou zahrnuty komentáře, jsou všechny znaky po první komentář považovány za součást komentář.


## <a name="next-steps"></a>Další kroky

- Provede kurz k používání [hledání protokolů portál](log-analytics-log-search-log-search-portal.md) nebo [pokročilé analýzy portál](https://go.microsoft.com/fwlink/?linkid=856587) vytváření dotazů.
- Podívejte se [kurz na zápis dotazů](https://go.microsoft.com/fwlink/?linkid=856078) pomocí nového dotazu jazyka.
