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
# <a name="portals-for-creating-and-editing-log-queries-in-azure-log-analytics"></a><span data-ttu-id="05e17-103">Portály pro vytváření a úpravy dotazů protokolu v Azure Log Analytics</span><span class="sxs-lookup"><span data-stu-id="05e17-103">Portals for creating and editing log queries in Azure Log Analytics</span></span>

> [!NOTE]
> <span data-ttu-id="05e17-104">Tento článek popisuje portály v Azure Log Analytics jazykem nový dotaz.</span><span class="sxs-lookup"><span data-stu-id="05e17-104">This article describes portals in Azure Log Analytics using the new query language.</span></span>  <span data-ttu-id="05e17-105">Můžete získat další informace o nový jazyk a získat postup upgradu pracovního prostoru v [upgradu pracovní prostor analýzy protokolů Azure na nové hledání protokolu](log-analytics-log-search-upgrade.md).</span><span class="sxs-lookup"><span data-stu-id="05e17-105">You can learn more about the new language and get the procedure to upgrade your workspace at [Upgrade your Azure Log Analytics workspace to new log search](log-analytics-log-search-upgrade.md).</span></span>  
>
> <span data-ttu-id="05e17-106">Pokud pracovní prostor nebyla upgradována, aby nové dotazovací jazyk, se seznamte s [najít data pomocí protokolu hledání v analýzy protokolů](log-analytics-log-searches.md) informace o aktuální verzi portálu hledání protokolů.</span><span class="sxs-lookup"><span data-stu-id="05e17-106">If your workspace hasn't been upgraded to the new query language, you should refer to [Find data using log searches in Log Analytics](log-analytics-log-searches.md) for information on the current version of the Log Search portal.</span></span>

<span data-ttu-id="05e17-107">Protokol hledání v různých místech analýzy protokolů se použít k načtení dat z pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="05e17-107">You use log searches in a variety of places throughout Log Analytics to retrieve data from the workspace.</span></span>  <span data-ttu-id="05e17-108">Ve skutečnosti vytváření a úpravy dotazů kromě práce interaktivně se vrácená data, když, máte dvě možnosti, které jsou popsány níže.</span><span class="sxs-lookup"><span data-stu-id="05e17-108">For actually creating and editing queries in addition to working interactively with returned data though, you have two options that are described below.</span></span>  

## <a name="log-search-portal"></a><span data-ttu-id="05e17-109">Protokol hledání portálu</span><span class="sxs-lookup"><span data-stu-id="05e17-109">Log search portal</span></span>
<span data-ttu-id="05e17-110">Portál hledání protokolů je dostupný z portálu Azure nebo na portálu OMS.</span><span class="sxs-lookup"><span data-stu-id="05e17-110">The Log Search portal is accessible from the Azure portal or the OMS portal.</span></span>  <span data-ttu-id="05e17-111">Je vhodné pro vytvoření základní dotazy, které lze vytvořit na jeden řádek.</span><span class="sxs-lookup"><span data-stu-id="05e17-111">It's suitable for creating basic queries that can be created on a single line.</span></span>  <span data-ttu-id="05e17-112">Portálu hledání protokolů můžete používat bez spuštění externí portál a slouží k provádění různých funkcí s protokolu hledání, včetně vytvoření pravidla výstrah, vytvoření skupin počítačů a export výsledků dotazu.</span><span class="sxs-lookup"><span data-stu-id="05e17-112">The Log Search portal can be used without launching an external portal, and you can use it to perform a variety of functions with log searches including creating alert rules, creating computer groups, and exporting the results of the query.</span></span>  

<span data-ttu-id="05e17-113">Hledání protokolů portál nabízí více funkcí pro úpravy dotazu bez nutnosti úplné znalost dotazovacího jazyka.</span><span class="sxs-lookup"><span data-stu-id="05e17-113">The Log Search portal provides multiple features for editing the query without having a full knowledge of the query language.</span></span>  <span data-ttu-id="05e17-114">Zobrazí souhrn těchto funkcí v [vytvořit protokolu hledání v Azure Log Analytics pomocí portálu hledání protokolů](log-analytics-log-search-log-search-portal.md).</span><span class="sxs-lookup"><span data-stu-id="05e17-114">You can get a summary of these features in [Create log searches in Azure Log Analytics using the Log Search portal](log-analytics-log-search-log-search-portal.md).</span></span>


![Protokol hledání portálu](media/log-analytics-log-search-portals/log-search-portal.png)

## <a name="advanced-analytics-portal"></a><span data-ttu-id="05e17-116">Pokročilé analýzy portálu</span><span class="sxs-lookup"><span data-stu-id="05e17-116">Advanced Analytics portal</span></span>
<span data-ttu-id="05e17-117">Portálu pro pokročilou analýzu je vyhrazené portál, který poskytuje pokročilé funkce není k dispozici na portálu hledání protokolů.</span><span class="sxs-lookup"><span data-stu-id="05e17-117">The Advanced Analytics portal is a dedicated portal that provides advanced functionality not available in the Log Search portal.</span></span>  <span data-ttu-id="05e17-118">Funkce zahrnují možnost upravit dotaz na více řádků, selektivně spuštění kódu, závislé na kontextu Intellisense a inteligentní analýzy.</span><span class="sxs-lookup"><span data-stu-id="05e17-118">Features include the ability to edit a query on multiple lines, selectively execute code, context sensitive Intellisense, and Smart Analytics.</span></span>  <span data-ttu-id="05e17-119">Portál Advanced Analytics je nejvhodnější pro návrh složitých dotazů, které jsou buď jako vyhledávání protokolu uložit nebo kopírovat a vkládat data do jiných elementů analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="05e17-119">The Advanced Analytics portal is most suitable for designing complex queries that are either saved as a log search or copied and pasted into other Log Analytics elements.</span></span>  <span data-ttu-id="05e17-120">Spuštění portálu Advanced Analytics z odkaz na portálu hledání protokolů.</span><span class="sxs-lookup"><span data-stu-id="05e17-120">You launch the Advanced Analytics portal from a link in the Log Search portal.</span></span>

![Pokročilé analýzy portálu](media/log-analytics-log-search-portals/advanced-analytics-portal.png)


<span data-ttu-id="05e17-122">Z důvodu jeho pokročilé funkce obvykle použijete portálu pokročilou analýzu jako svůj primární nástroj pro vytváření a úpravy dotazů.</span><span class="sxs-lookup"><span data-stu-id="05e17-122">Because of its advanced features, you'll usually use the Advanced Analytics portal as your primary tool for creating and editing queries.</span></span>  <span data-ttu-id="05e17-123">Jednou jste zjistili, že dotaz funguje podle očekávání, a potom budete zkopírujte a vložte ho jinde například hledání protokolů portálu nebo Návrhář zobrazení.</span><span class="sxs-lookup"><span data-stu-id="05e17-123">Once you've determined that the query works as expected, then you'll copy and paste it elsewhere such as the Log Search portal or View Designer.</span></span>  <span data-ttu-id="05e17-124">Protože portál Advanced Analytics podporuje dotazy s více řádky, i když je nutné vzít v úvahu následující při kopírování dotazu z tento portál.</span><span class="sxs-lookup"><span data-stu-id="05e17-124">Because the Advanced Analytics portal supports queries with multiple lines though, you need to take the following into consideration when copying a query from this portal.</span></span>

- <span data-ttu-id="05e17-125">Komentáře musí odebrat z dotazu před má zkopírovat a vložit do jiného umístění.</span><span class="sxs-lookup"><span data-stu-id="05e17-125">Comments must be removed from the query before it's copied and pasted into another location.</span></span>  <span data-ttu-id="05e17-126">Můžete okomentovat řádek tak, že před s dvě lomítka (/ /).</span><span class="sxs-lookup"><span data-stu-id="05e17-126">You can comment a line by preceding it with two slashes (//).</span></span>  <span data-ttu-id="05e17-127">Když vložíte více dotazu řádku do jednoho řádku, jsou odebrány konce řádků.</span><span class="sxs-lookup"><span data-stu-id="05e17-127">When you paste a multiple line query into a single line, line breaks are removed.</span></span>  <span data-ttu-id="05e17-128">Pokud jsou zahrnuty komentáře, jsou všechny znaky po první komentář považovány za součást komentář.</span><span class="sxs-lookup"><span data-stu-id="05e17-128">If comments are included, all characters after the first comment are considered part of the comment.</span></span>


## <a name="next-steps"></a><span data-ttu-id="05e17-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="05e17-129">Next steps</span></span>

- <span data-ttu-id="05e17-130">Provede kurz k používání [hledání protokolů portál](log-analytics-log-search-log-search-portal.md) nebo [pokročilé analýzy portál](https://go.microsoft.com/fwlink/?linkid=856587) vytváření dotazů.</span><span class="sxs-lookup"><span data-stu-id="05e17-130">Walk through a tutorial on using the [Log Search portal](log-analytics-log-search-log-search-portal.md) or the [Advanced Analytics portal](https://go.microsoft.com/fwlink/?linkid=856587) to create queries.</span></span>
- <span data-ttu-id="05e17-131">Podívejte se [kurz na zápis dotazů](https://go.microsoft.com/fwlink/?linkid=856078) pomocí nového dotazu jazyka.</span><span class="sxs-lookup"><span data-stu-id="05e17-131">Check out a [tutorial on writing queries](https://go.microsoft.com/fwlink/?linkid=856078) using the new query language.</span></span>
