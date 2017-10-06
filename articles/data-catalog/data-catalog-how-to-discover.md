---
title: aaaHow toodiscover zdroje dat v Azure Data Catalog | Microsoft Docs
description: "Tento článek popisuje, jak toodiscover registrovaných datových prostředků s Azure Data Catalog, včetně vyhledávání a filtrování a pomocí hello dosáhl zvýraznění možnosti portálu Azure Data Catalog hello."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: f72ae3a3-6573-4710-89a7-f13555e1968c
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: 624834b8895dd50c8931c9d3e6f8dc217927c617
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodiscover-data-sources-in-azure-data-catalog"></a><span data-ttu-id="3da70-103">Jak zdroje dat toodiscover v Azure Data Catalog</span><span class="sxs-lookup"><span data-stu-id="3da70-103">How toodiscover data sources in Azure Data Catalog</span></span>
## <a name="introduction"></a><span data-ttu-id="3da70-104">Úvod</span><span class="sxs-lookup"><span data-stu-id="3da70-104">Introduction</span></span>
<span data-ttu-id="3da70-105">Azure Data Catalog je plně spravovaná Cloudová služba, která slouží jako systém registrace a zjišťování datových zdrojů organizace.</span><span class="sxs-lookup"><span data-stu-id="3da70-105">Azure Data Catalog is a fully managed cloud service that serves as a system of registration and discovery for enterprise data sources.</span></span> <span data-ttu-id="3da70-106">Jinými slovy Data Catalog umožňuje uživatelům zjišťovat, pochopit a používat zdroje dat a pomáhá organizacím získat větší hodnotu ze své stávající data.</span><span class="sxs-lookup"><span data-stu-id="3da70-106">In other words, Data Catalog helps people discover, understand, and use data sources, and it helps organizations get more value from their existing data.</span></span> <span data-ttu-id="3da70-107">Po registraci zdroje dat pomocí katalogu Data Catalog jeho metadata je indexovaný službou hello, takže můžete snadno vyhledat toodiscover hello dat, které potřebujete.</span><span class="sxs-lookup"><span data-stu-id="3da70-107">After a data source is registered with Data Catalog, its metadata is indexed by hello service, so that you can easily search toodiscover hello data you need.</span></span>

## <a name="searching-and-filtering"></a><span data-ttu-id="3da70-108">Vyhledávání a filtrování</span><span class="sxs-lookup"><span data-stu-id="3da70-108">Searching and filtering</span></span>
<span data-ttu-id="3da70-109">Funkce zjišťování v katalogu Data Catalog používá dva primární mechanismy: vyhledávání a filtrování.</span><span class="sxs-lookup"><span data-stu-id="3da70-109">Discovery in Data Catalog uses two primary mechanisms: searching and filtering.</span></span>

<span data-ttu-id="3da70-110">Hledání je navrženou toobe intuitivní a efektivní.</span><span class="sxs-lookup"><span data-stu-id="3da70-110">Searching is designed toobe both intuitive and powerful.</span></span> <span data-ttu-id="3da70-111">Ve výchozím nastavení jsou podmínky vyhledávání porovnání všechny vlastnosti v katalogu hello, včetně poznámek zadaný uživatelem.</span><span class="sxs-lookup"><span data-stu-id="3da70-111">By default, search terms are matched against any property in hello catalog, including user-provided annotations.</span></span>

<span data-ttu-id="3da70-112">Filtrování je určená toocomplement vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="3da70-112">Filtering is designed toocomplement searching.</span></span> <span data-ttu-id="3da70-113">Můžete vybrat konkrétní charakteristiky jako odborníky, typ zdroje dat, typ objektu a značky.</span><span class="sxs-lookup"><span data-stu-id="3da70-113">You can select specific characteristics such as experts, data source type, object type, and tags.</span></span> <span data-ttu-id="3da70-114">Můžete zobrazit jenom odpovídající datových prostředků a omezit prostředky toomatching výsledky hledání.</span><span class="sxs-lookup"><span data-stu-id="3da70-114">You can view only matching data assets, and constrain search results toomatching assets.</span></span>

<span data-ttu-id="3da70-115">Pomocí kombinace vyhledávání a filtrování, můžete rychle procházet hello zdroje dat, které byly zaregistrovány pomocí katalogu Data Catalog toodiscover hello datového zdroje, které potřebujete.</span><span class="sxs-lookup"><span data-stu-id="3da70-115">By using a combination of searching and filtering, you can quickly navigate hello data sources that have been registered with Data Catalog toodiscover hello data sources you need.</span></span>

## <a name="search-syntax"></a><span data-ttu-id="3da70-116">Syntaxe služby Search</span><span class="sxs-lookup"><span data-stu-id="3da70-116">Search syntax</span></span>
<span data-ttu-id="3da70-117">Přestože hello výchozí textové vyhledávání je jednoduché a intuitivní, můžete také použít syntaxe vyhledávání katalogu Data Catalog pro větší kontrolu nad výsledky hledání hello.</span><span class="sxs-lookup"><span data-stu-id="3da70-117">Although hello default free text search is simple and intuitive, you can also use Data Catalog search syntax for greater control over hello search results.</span></span> <span data-ttu-id="3da70-118">Hello podporuje vyhledávání katalogu dat následující techniky:</span><span class="sxs-lookup"><span data-stu-id="3da70-118">Data Catalog search supports hello following techniques:</span></span>

| <span data-ttu-id="3da70-119">Technika</span><span class="sxs-lookup"><span data-stu-id="3da70-119">Technique</span></span> | <span data-ttu-id="3da70-120">Použití</span><span class="sxs-lookup"><span data-stu-id="3da70-120">Use</span></span> | <span data-ttu-id="3da70-121">Příklad</span><span class="sxs-lookup"><span data-stu-id="3da70-121">Example</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3da70-122">Základní hledání</span><span class="sxs-lookup"><span data-stu-id="3da70-122">Basic search</span></span> |<span data-ttu-id="3da70-123">Základní vyhledávání, který používá jeden nebo více výrazů vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="3da70-123">Basic search that uses one or more search terms.</span></span> <span data-ttu-id="3da70-124">Výsledky jsou všechny prostředky odpovídající libovolné vlastnosti s jednou nebo více podmínek hello zadán.</span><span class="sxs-lookup"><span data-stu-id="3da70-124">Results are any assets that match any property with one or more of hello terms specified.</span></span> |`sales data` |
| <span data-ttu-id="3da70-125">Zkoumání vlastnosti</span><span class="sxs-lookup"><span data-stu-id="3da70-125">Property scoping</span></span> |<span data-ttu-id="3da70-126">Návratový pouze zdroje dat kde hello hledaný termín shoduje s hello určená vlastnost.</span><span class="sxs-lookup"><span data-stu-id="3da70-126">Return only data sources where hello search term is matched with hello specified property.</span></span> |`name:finance` |
| <span data-ttu-id="3da70-127">Logické operátory</span><span class="sxs-lookup"><span data-stu-id="3da70-127">Boolean operators</span></span> |<span data-ttu-id="3da70-128">Rozšíří nebo zúží vyhledávání pomocí logických operací.</span><span class="sxs-lookup"><span data-stu-id="3da70-128">Broaden or narrow a search by using Boolean operations.</span></span> |`finance NOT corporate` |
| <span data-ttu-id="3da70-129">Seskupování pomocí závorek</span><span class="sxs-lookup"><span data-stu-id="3da70-129">Grouping with parenthesis</span></span> |<span data-ttu-id="3da70-130">Pomocí závorek toogroup částí hello dotazu tooachieve logické izolace, zejména ve spojení s logickými operátory.</span><span class="sxs-lookup"><span data-stu-id="3da70-130">Use parentheses toogroup parts of hello query tooachieve logical isolation, especially in conjunction with Boolean operators.</span></span> |`name:finance AND (tags:Q1 OR tags:Q2)` |
| <span data-ttu-id="3da70-131">Operátory porovnání</span><span class="sxs-lookup"><span data-stu-id="3da70-131">Comparison operators</span></span> |<span data-ttu-id="3da70-132">Porovnávání jiné než rovnost použijte pro vlastnosti, které mají číselné a číselné datové typy.</span><span class="sxs-lookup"><span data-stu-id="3da70-132">Use comparisons other than equality for properties that have numeric and date data types.</span></span> |`modifiedTime > "11/05/2014"` |

<span data-ttu-id="3da70-133">Další informace o hledání v katalogu dat najdete v tématu hello [Azure Data Catalog](https://msdn.microsoft.com/library/azure/mt267594.aspx) článku.</span><span class="sxs-lookup"><span data-stu-id="3da70-133">For more information about Data Catalog search, see hello [Azure Data Catalog](https://msdn.microsoft.com/library/azure/mt267594.aspx) article.</span></span>

## <a name="hit-highlighting"></a><span data-ttu-id="3da70-134">Zvýrazňování položek</span><span class="sxs-lookup"><span data-stu-id="3da70-134">Hit highlighting</span></span>
<span data-ttu-id="3da70-135">Při zobrazení výsledků vyhledávání žádný zobrazí vlastnosti, které odpovídají zadané hello hledaných termínů (například hello data asset název, popis a značky) jsou zvýrazněné toomake ho jednodušší tooidentify, proč daný datový prostředek vrátil zadaný hledaný.</span><span class="sxs-lookup"><span data-stu-id="3da70-135">When you view search results, any displayed properties that match hello specified search terms (such as hello data asset name, description, and tags) are highlighted toomake it easier tooidentify why a given data asset was returned by a given search.</span></span>

> [!NOTE]
> <span data-ttu-id="3da70-136">tooturn vypnout podle zvýraznění, použijte hello **zvýrazněte** přepínač portálu hello katalogu Data Catalog.</span><span class="sxs-lookup"><span data-stu-id="3da70-136">tooturn off hit highlighting, use hello **Highlight** switch in hello Data Catalog portal.</span></span>
>
>

<span data-ttu-id="3da70-137">Při zobrazení výsledků vyhledávání, nemusí být vždy zřejmé proč datový prostředek je součástí, dokonce i s přístupů zvýraznění povolena.</span><span class="sxs-lookup"><span data-stu-id="3da70-137">When you view search results, it might not always be obvious why a data asset is included, even with hit highlighting enabled.</span></span> <span data-ttu-id="3da70-138">Protože vyhledávají se všechny vlastnosti ve výchozím nastavení, může být z důvodu shoda na úrovni sloupce vlastnost vrácena datový prostředek.</span><span class="sxs-lookup"><span data-stu-id="3da70-138">Because all properties are searched by default, a data asset might be returned because of a match on a column-level property.</span></span> <span data-ttu-id="3da70-139">A protože více uživatelů může opatřit poznámkami registrované datové prostředky s vlastní značky a popisy, ne všechny metadata může zobrazit v seznamu hello výsledků hledání.</span><span class="sxs-lookup"><span data-stu-id="3da70-139">And because multiple users can annotate registered data assets with their own tags and descriptions, not all metadata might be displayed in hello list of search results.</span></span>

<span data-ttu-id="3da70-140">V zobrazení tile výchozí hello, každou dlaždici zobrazí ve výsledcích hledání hello zahrnuje **zobrazení hledaný termín shoduje** ikonu, tak, aby hello počet shod a jejich umístění a toojump toothem lze rychle zobrazit, pokud chcete.</span><span class="sxs-lookup"><span data-stu-id="3da70-140">In hello default tile view, each tile displayed in hello search results includes a **View search term matches** icon, so that you can quickly view hello number of matches and their location, and toojump toothem if you want.</span></span>

 ![Zvýrazňování položek a odpovídá vyhledávání v portálu Azure Data Catalog hello](./media/data-catalog-how-to-discover/search-matches.png)

## <a name="summary"></a><span data-ttu-id="3da70-142">Souhrn</span><span class="sxs-lookup"><span data-stu-id="3da70-142">Summary</span></span>
<span data-ttu-id="3da70-143">Protože registraci zdroje dat se katalogu Data Catalog kopiemi strukturální a popisný metadata z hello datového zdroje toohello katalogu služby, hello zdroj dat bude jednodušší toodiscover a pochopit.</span><span class="sxs-lookup"><span data-stu-id="3da70-143">Because registering a data source with Data Catalog copies structural and descriptive metadata from hello data source toohello catalog service, hello data source becomes easier toodiscover and understand.</span></span> <span data-ttu-id="3da70-144">Poté, co jste registrováni zdroj dat, můžete zjistit pomocí filtrování a vyhledávání z portálu hello katalogu Data Catalog.</span><span class="sxs-lookup"><span data-stu-id="3da70-144">After you've registered a data source, you can discover it by using filtering and search from within hello Data Catalog portal.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3da70-145">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3da70-145">Next steps</span></span>
* <span data-ttu-id="3da70-146">Podrobné informace o toodiscover datových zdrojů, najdete v části [Začínáme s Azure Data Catalog](data-catalog-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="3da70-146">For step-by-step details about how toodiscover data sources, see [Get Started with Azure Data Catalog](data-catalog-get-started.md).</span></span>
