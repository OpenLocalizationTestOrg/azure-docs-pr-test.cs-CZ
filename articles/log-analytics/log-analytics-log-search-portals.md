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
# <a name="portals-for-creating-and-editing-log-queries-in-azure-log-analytics"></a><span data-ttu-id="db200-103">Portály pro vytváření a úpravy dotazů protokolu v Azure Log Analytics</span><span class="sxs-lookup"><span data-stu-id="db200-103">Portals for creating and editing log queries in Azure Log Analytics</span></span>

> [!NOTE]
> <span data-ttu-id="db200-104">Tento článek popisuje portály v Azure Log Analytics pomocí dotazovacího jazyka pro nové hello.</span><span class="sxs-lookup"><span data-stu-id="db200-104">This article describes portals in Azure Log Analytics using hello new query language.</span></span>  <span data-ttu-id="db200-105">Můžete získat další informace o nový jazyk hello a získat hello postup tooupgrade pracovního prostoru v [Upgrade vyhledávání protokolu toonew pracovní prostor analýzy protokolů Azure](log-analytics-log-search-upgrade.md).</span><span class="sxs-lookup"><span data-stu-id="db200-105">You can learn more about hello new language and get hello procedure tooupgrade your workspace at [Upgrade your Azure Log Analytics workspace toonew log search](log-analytics-log-search-upgrade.md).</span></span>  
>
> <span data-ttu-id="db200-106">Pokud pracovní prostor nebyla upgradovaná toohello nové dotazovací jazyk, se seznamte s příliš[najít data pomocí protokolu hledání v analýzy protokolů](log-analytics-log-searches.md) informace o aktuální verzi portálu hledání protokolů hello hello.</span><span class="sxs-lookup"><span data-stu-id="db200-106">If your workspace hasn't been upgraded toohello new query language, you should refer too[Find data using log searches in Log Analytics](log-analytics-log-searches.md) for information on hello current version of hello Log Search portal.</span></span>

<span data-ttu-id="db200-107">Používáte protokol hledání v různých místech analýzy protokolů tooretrieve data z pracovního prostoru hello.</span><span class="sxs-lookup"><span data-stu-id="db200-107">You use log searches in a variety of places throughout Log Analytics tooretrieve data from hello workspace.</span></span>  <span data-ttu-id="db200-108">Ve skutečnosti vytváření a úpravy dotazů kromě tooworking interaktivně se ale vrátil data, máte dvě možnosti, které jsou popsány níže.</span><span class="sxs-lookup"><span data-stu-id="db200-108">For actually creating and editing queries in addition tooworking interactively with returned data though, you have two options that are described below.</span></span>  

## <a name="log-search-portal"></a><span data-ttu-id="db200-109">Protokol hledání portálu</span><span class="sxs-lookup"><span data-stu-id="db200-109">Log search portal</span></span>
<span data-ttu-id="db200-110">portál hledání protokolů Hello je dostupné z hello portál Azure nebo na portálu OMS hello.</span><span class="sxs-lookup"><span data-stu-id="db200-110">hello Log Search portal is accessible from hello Azure portal or hello OMS portal.</span></span>  <span data-ttu-id="db200-111">Je vhodné pro vytvoření základní dotazy, které lze vytvořit na jeden řádek.</span><span class="sxs-lookup"><span data-stu-id="db200-111">It's suitable for creating basic queries that can be created on a single line.</span></span>  <span data-ttu-id="db200-112">portál hledání protokolů Hello lze použít bez spuštění externí portál a můžete ji použít tooperform celou řadu funkcí s protokolu hledání, včetně vytvoření pravidla výstrah, vytvoření skupin počítačů a export výsledků hello hello dotazu.</span><span class="sxs-lookup"><span data-stu-id="db200-112">hello Log Search portal can be used without launching an external portal, and you can use it tooperform a variety of functions with log searches including creating alert rules, creating computer groups, and exporting hello results of hello query.</span></span>  

<span data-ttu-id="db200-113">portál hledání protokolů Hello nabízí více funkcí pro úpravy hello dotazu bez nutnosti úplné znalost dotazovacího jazyka pro hello.</span><span class="sxs-lookup"><span data-stu-id="db200-113">hello Log Search portal provides multiple features for editing hello query without having a full knowledge of hello query language.</span></span>  <span data-ttu-id="db200-114">Zobrazí souhrn těchto funkcí v [vytvořit protokolu hledání v Azure Log Analytics pomocí portálu hledání protokolů hello](log-analytics-log-search-log-search-portal.md).</span><span class="sxs-lookup"><span data-stu-id="db200-114">You can get a summary of these features in [Create log searches in Azure Log Analytics using hello Log Search portal](log-analytics-log-search-log-search-portal.md).</span></span>


![Protokol hledání portálu](media/log-analytics-log-search-portals/log-search-portal.png)

## <a name="advanced-analytics-portal"></a><span data-ttu-id="db200-116">Pokročilé analýzy portálu</span><span class="sxs-lookup"><span data-stu-id="db200-116">Advanced Analytics portal</span></span>
<span data-ttu-id="db200-117">portál pokročilou analýzu Hello je vyhrazené portál, který poskytuje pokročilé funkce není k dispozici hello hledání protokolů portálu.</span><span class="sxs-lookup"><span data-stu-id="db200-117">hello Advanced Analytics portal is a dedicated portal that provides advanced functionality not available in hello Log Search portal.</span></span>  <span data-ttu-id="db200-118">Funkce patří možnost tooedit hello dotaz na více řádků, selektivně spuštění kódu, závislé na kontextu Intellisense a inteligentní analýzy.</span><span class="sxs-lookup"><span data-stu-id="db200-118">Features include hello ability tooedit a query on multiple lines, selectively execute code, context sensitive Intellisense, and Smart Analytics.</span></span>  <span data-ttu-id="db200-119">portál pokročilé analýzy Hello je nejvhodnější pro návrh složitých dotazů, které jsou buď jako vyhledávání protokolu uložit nebo kopírovat a vkládat data do jiných elementů analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="db200-119">hello Advanced Analytics portal is most suitable for designing complex queries that are either saved as a log search or copied and pasted into other Log Analytics elements.</span></span>  <span data-ttu-id="db200-120">Spuštění portálu pokročilé analýzy hello pomocí odkazu v portálu hledání protokolů hello.</span><span class="sxs-lookup"><span data-stu-id="db200-120">You launch hello Advanced Analytics portal from a link in hello Log Search portal.</span></span>

![Pokročilé analýzy portálu](media/log-analytics-log-search-portals/advanced-analytics-portal.png)


<span data-ttu-id="db200-122">Z důvodu jeho pokročilé funkce obvykle použijete portál pokročilou analýzu hello jako svůj primární nástroj pro vytváření a úpravy dotazů.</span><span class="sxs-lookup"><span data-stu-id="db200-122">Because of its advanced features, you'll usually use hello Advanced Analytics portal as your primary tool for creating and editing queries.</span></span>  <span data-ttu-id="db200-123">Jakmile jste zjistili, že dotaz hello funguje podle očekávání, a potom budete zkopírujte a vložte ho jinde například portál hledání protokolů hello nebo Návrhář zobrazení.</span><span class="sxs-lookup"><span data-stu-id="db200-123">Once you've determined that hello query works as expected, then you'll copy and paste it elsewhere such as hello Log Search portal or View Designer.</span></span>  <span data-ttu-id="db200-124">Protože portál pokročilé analýzy hello ale podporuje dotazy s více řádky, potřebujete následující hello tootake v úvahu při kopírování dotazu z tento portál.</span><span class="sxs-lookup"><span data-stu-id="db200-124">Because hello Advanced Analytics portal supports queries with multiple lines though, you need tootake hello following into consideration when copying a query from this portal.</span></span>

- <span data-ttu-id="db200-125">Komentáře musí odebrat z dotazu hello před má zkopírovat a vložit do jiného umístění.</span><span class="sxs-lookup"><span data-stu-id="db200-125">Comments must be removed from hello query before it's copied and pasted into another location.</span></span>  <span data-ttu-id="db200-126">Můžete okomentovat řádek tak, že před s dvě lomítka (/ /).</span><span class="sxs-lookup"><span data-stu-id="db200-126">You can comment a line by preceding it with two slashes (//).</span></span>  <span data-ttu-id="db200-127">Když vložíte více dotazu řádku do jednoho řádku, jsou odebrány konce řádků.</span><span class="sxs-lookup"><span data-stu-id="db200-127">When you paste a multiple line query into a single line, line breaks are removed.</span></span>  <span data-ttu-id="db200-128">Pokud jsou zahrnuty komentáře, jsou všechny znaky po první komentář hello považovány za součást hello komentář.</span><span class="sxs-lookup"><span data-stu-id="db200-128">If comments are included, all characters after hello first comment are considered part of hello comment.</span></span>


## <a name="next-steps"></a><span data-ttu-id="db200-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="db200-129">Next steps</span></span>

- <span data-ttu-id="db200-130">Provede kurz k používání hello [hledání protokolů portál](log-analytics-log-search-log-search-portal.md) nebo hello [Advanced Analytics portál](https://go.microsoft.com/fwlink/?linkid=856587) toocreate dotazy.</span><span class="sxs-lookup"><span data-stu-id="db200-130">Walk through a tutorial on using hello [Log Search portal](log-analytics-log-search-log-search-portal.md) or hello [Advanced Analytics portal](https://go.microsoft.com/fwlink/?linkid=856587) toocreate queries.</span></span>
- <span data-ttu-id="db200-131">Podívejte se [kurz na zápis dotazů](https://go.microsoft.com/fwlink/?linkid=856078) pomocí dotazovacího jazyka pro nové hello.</span><span class="sxs-lookup"><span data-stu-id="db200-131">Check out a [tutorial on writing queries](https://go.microsoft.com/fwlink/?linkid=856078) using hello new query language.</span></span>
