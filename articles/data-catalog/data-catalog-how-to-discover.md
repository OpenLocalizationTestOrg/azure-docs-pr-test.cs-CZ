---
title: "Tom, jak zjišťovat zdroje dat v Azure Data Catalog | Microsoft Docs"
description: "V tomto článku se dozvíte, jak ke zjištění registrovaných datových prostředků s Azure Data Catalog, včetně vyhledávání a filtrování a pomocí přístupů zvýraznění funkcí z portálu Azure Data Catalog."
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
ms.openlocfilehash: 9ff67dcb5ecb00440f73f979fd8d2b79a570c674
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-discover-data-sources-in-azure-data-catalog"></a><span data-ttu-id="bf505-103">Tom, jak zjišťovat zdroje dat v Azure Data Catalog</span><span class="sxs-lookup"><span data-stu-id="bf505-103">How to discover data sources in Azure Data Catalog</span></span>
## <a name="introduction"></a><span data-ttu-id="bf505-104">Úvod</span><span class="sxs-lookup"><span data-stu-id="bf505-104">Introduction</span></span>
<span data-ttu-id="bf505-105">Azure Data Catalog je plně spravovaná Cloudová služba, která slouží jako systém registrace a zjišťování datových zdrojů organizace.</span><span class="sxs-lookup"><span data-stu-id="bf505-105">Azure Data Catalog is a fully managed cloud service that serves as a system of registration and discovery for enterprise data sources.</span></span> <span data-ttu-id="bf505-106">Jinými slovy Data Catalog umožňuje uživatelům zjišťovat, pochopit a používat zdroje dat a pomáhá organizacím získat větší hodnotu ze své stávající data.</span><span class="sxs-lookup"><span data-stu-id="bf505-106">In other words, Data Catalog helps people discover, understand, and use data sources, and it helps organizations get more value from their existing data.</span></span> <span data-ttu-id="bf505-107">Po registraci zdroje dat pomocí katalogu Data Catalog jeho metadata je indexovaný služba, takže můžete snadno vyhledat ke zjištění dat, které potřebujete.</span><span class="sxs-lookup"><span data-stu-id="bf505-107">After a data source is registered with Data Catalog, its metadata is indexed by the service, so that you can easily search to discover the data you need.</span></span>

## <a name="searching-and-filtering"></a><span data-ttu-id="bf505-108">Vyhledávání a filtrování</span><span class="sxs-lookup"><span data-stu-id="bf505-108">Searching and filtering</span></span>
<span data-ttu-id="bf505-109">Funkce zjišťování v katalogu Data Catalog používá dva primární mechanismy: vyhledávání a filtrování.</span><span class="sxs-lookup"><span data-stu-id="bf505-109">Discovery in Data Catalog uses two primary mechanisms: searching and filtering.</span></span>

<span data-ttu-id="bf505-110">Vyhledávání je koncipováno tak, aby bylo jak intuitivní, tak výkonné.</span><span class="sxs-lookup"><span data-stu-id="bf505-110">Searching is designed to be both intuitive and powerful.</span></span> <span data-ttu-id="bf505-111">Ve výchozím nastavení se vyhledává shoda hledaných výrazů s libovolnou vlastností v katalogu včetně poznámek přidaných uživatelem.</span><span class="sxs-lookup"><span data-stu-id="bf505-111">By default, search terms are matched against any property in the catalog, including user-provided annotations.</span></span>

<span data-ttu-id="bf505-112">Filtrování je koncipováno jako doplněk k vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="bf505-112">Filtering is designed to complement searching.</span></span> <span data-ttu-id="bf505-113">Můžete vybrat konkrétní charakteristiky jako odborníky, typ zdroje dat, typ objektu a značky.</span><span class="sxs-lookup"><span data-stu-id="bf505-113">You can select specific characteristics such as experts, data source type, object type, and tags.</span></span> <span data-ttu-id="bf505-114">Můžete zobrazit jenom odpovídající datových prostředků a omezit výsledky vyhledávání na odpovídající prostředky.</span><span class="sxs-lookup"><span data-stu-id="bf505-114">You can view only matching data assets, and constrain search results to matching assets.</span></span>

<span data-ttu-id="bf505-115">Pomocí kombinace vyhledávání a filtrování, můžete rychle procházet zdroje dat, které byly zaregistrovány pomocí katalogu Data Catalog zjišťování zdrojů dat, které potřebujete.</span><span class="sxs-lookup"><span data-stu-id="bf505-115">By using a combination of searching and filtering, you can quickly navigate the data sources that have been registered with Data Catalog to discover the data sources you need.</span></span>

## <a name="search-syntax"></a><span data-ttu-id="bf505-116">Syntaxe služby Search</span><span class="sxs-lookup"><span data-stu-id="bf505-116">Search syntax</span></span>
<span data-ttu-id="bf505-117">I když výchozí textové vyhledávání je jednoduché a intuitivní, můžete taky syntaxe vyhledávání katalogu Data Catalog pro větší kontrolu nad výsledky hledání.</span><span class="sxs-lookup"><span data-stu-id="bf505-117">Although the default free text search is simple and intuitive, you can also use Data Catalog search syntax for greater control over the search results.</span></span> <span data-ttu-id="bf505-118">Hledání v katalogu dat podporuje následující techniky:</span><span class="sxs-lookup"><span data-stu-id="bf505-118">Data Catalog search supports the following techniques:</span></span>

| <span data-ttu-id="bf505-119">Technika</span><span class="sxs-lookup"><span data-stu-id="bf505-119">Technique</span></span> | <span data-ttu-id="bf505-120">Použití</span><span class="sxs-lookup"><span data-stu-id="bf505-120">Use</span></span> | <span data-ttu-id="bf505-121">Příklad</span><span class="sxs-lookup"><span data-stu-id="bf505-121">Example</span></span> |
| --- | --- | --- |
| <span data-ttu-id="bf505-122">Základní hledání</span><span class="sxs-lookup"><span data-stu-id="bf505-122">Basic search</span></span> |<span data-ttu-id="bf505-123">Základní vyhledávání, který používá jeden nebo více výrazů vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="bf505-123">Basic search that uses one or more search terms.</span></span> <span data-ttu-id="bf505-124">Výsledky jsou všechny prostředky, které odpovídají všechny vlastnosti s jednou nebo více výrazů uvedených.</span><span class="sxs-lookup"><span data-stu-id="bf505-124">Results are any assets that match any property with one or more of the terms specified.</span></span> |`sales data` |
| <span data-ttu-id="bf505-125">Zkoumání vlastnosti</span><span class="sxs-lookup"><span data-stu-id="bf505-125">Property scoping</span></span> |<span data-ttu-id="bf505-126">Vrátí pouze zdroje dat kde hledaný termín shoduje se zadanou vlastnost.</span><span class="sxs-lookup"><span data-stu-id="bf505-126">Return only data sources where the search term is matched with the specified property.</span></span> |`name:finance` |
| <span data-ttu-id="bf505-127">Logické operátory</span><span class="sxs-lookup"><span data-stu-id="bf505-127">Boolean operators</span></span> |<span data-ttu-id="bf505-128">Rozšíří nebo zúží vyhledávání pomocí logických operací.</span><span class="sxs-lookup"><span data-stu-id="bf505-128">Broaden or narrow a search by using Boolean operations.</span></span> |`finance NOT corporate` |
| <span data-ttu-id="bf505-129">Seskupování pomocí závorek</span><span class="sxs-lookup"><span data-stu-id="bf505-129">Grouping with parenthesis</span></span> |<span data-ttu-id="bf505-130">Závorky lze použijte k seskupení části dotazu k dosažení logické izolace, zejména ve spojení s logickými operátory.</span><span class="sxs-lookup"><span data-stu-id="bf505-130">Use parentheses to group parts of the query to achieve logical isolation, especially in conjunction with Boolean operators.</span></span> |`name:finance AND (tags:Q1 OR tags:Q2)` |
| <span data-ttu-id="bf505-131">Operátory porovnání</span><span class="sxs-lookup"><span data-stu-id="bf505-131">Comparison operators</span></span> |<span data-ttu-id="bf505-132">Porovnávání jiné než rovnost použijte pro vlastnosti, které mají číselné a číselné datové typy.</span><span class="sxs-lookup"><span data-stu-id="bf505-132">Use comparisons other than equality for properties that have numeric and date data types.</span></span> |`modifiedTime > "11/05/2014"` |

<span data-ttu-id="bf505-133">Další informace o hledání v katalogu dat najdete v tématu [Azure Data Catalog](https://msdn.microsoft.com/library/azure/mt267594.aspx) článku.</span><span class="sxs-lookup"><span data-stu-id="bf505-133">For more information about Data Catalog search, see the [Azure Data Catalog](https://msdn.microsoft.com/library/azure/mt267594.aspx) article.</span></span>

## <a name="hit-highlighting"></a><span data-ttu-id="bf505-134">Zvýrazňování položek</span><span class="sxs-lookup"><span data-stu-id="bf505-134">Hit highlighting</span></span>
<span data-ttu-id="bf505-135">Při zobrazení výsledků vyhledávání, všechny zobrazené vlastnosti, které odpovídají zadané hledaným výrazům (například data asset název, popis a značky) se zvýrazní, aby bylo snazší identifikovat, proč daný datový prostředek vrátil zadaný hledaný.</span><span class="sxs-lookup"><span data-stu-id="bf505-135">When you view search results, any displayed properties that match the specified search terms (such as the data asset name, description, and tags) are highlighted to make it easier to identify why a given data asset was returned by a given search.</span></span>

> [!NOTE]
> <span data-ttu-id="bf505-136">Chcete-li vypnout přístupů zvýraznění, použijte **zvýrazněte** přepnout na portálu katalogu Data Catalog.</span><span class="sxs-lookup"><span data-stu-id="bf505-136">To turn off hit highlighting, use the **Highlight** switch in the Data Catalog portal.</span></span>
>
>

<span data-ttu-id="bf505-137">Při zobrazení výsledků vyhledávání, nemusí být vždy zřejmé proč datový prostředek je součástí, dokonce i s přístupů zvýraznění povolena.</span><span class="sxs-lookup"><span data-stu-id="bf505-137">When you view search results, it might not always be obvious why a data asset is included, even with hit highlighting enabled.</span></span> <span data-ttu-id="bf505-138">Protože vyhledávají se všechny vlastnosti ve výchozím nastavení, může být z důvodu shoda na úrovni sloupce vlastnost vrácena datový prostředek.</span><span class="sxs-lookup"><span data-stu-id="bf505-138">Because all properties are searched by default, a data asset might be returned because of a match on a column-level property.</span></span> <span data-ttu-id="bf505-139">A protože více uživatelů může opatřit poznámkami registrované datové prostředky s vlastní značky a popisy, ne všechny metadata mohou být zobrazeny v seznamu výsledků hledání.</span><span class="sxs-lookup"><span data-stu-id="bf505-139">And because multiple users can annotate registered data assets with their own tags and descriptions, not all metadata might be displayed in the list of search results.</span></span>

<span data-ttu-id="bf505-140">Ve výchozím zobrazení vedle sebe, obsahuje každou dlaždici zobrazí ve výsledcích hledání **zobrazení hledaný termín shoduje** ikonu, takže můžete rychle zobrazit počet shod a jejich umístění a přejít k nim, pokud chcete.</span><span class="sxs-lookup"><span data-stu-id="bf505-140">In the default tile view, each tile displayed in the search results includes a **View search term matches** icon, so that you can quickly view the number of matches and their location, and to jump to them if you want.</span></span>

 ![Zvýrazňování položek a hledání odpovídá na portálu Azure Data Catalog](./media/data-catalog-how-to-discover/search-matches.png)

## <a name="summary"></a><span data-ttu-id="bf505-142">Souhrn</span><span class="sxs-lookup"><span data-stu-id="bf505-142">Summary</span></span>
<span data-ttu-id="bf505-143">Protože registraci zdroje dat pomocí katalogu Data Catalog kopie strukturální a popisný metadat ze zdroje dat do katalogu služby, zdroj dat vyhledatelné a pochopitelné jednodušší.</span><span class="sxs-lookup"><span data-stu-id="bf505-143">Because registering a data source with Data Catalog copies structural and descriptive metadata from the data source to the catalog service, the data source becomes easier to discover and understand.</span></span> <span data-ttu-id="bf505-144">Poté, co jste registrováni zdroj dat, můžete zjistit pomocí filtrování a vyhledávání z portálu pro katalog Data Catalog.</span><span class="sxs-lookup"><span data-stu-id="bf505-144">After you've registered a data source, you can discover it by using filtering and search from within the Data Catalog portal.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bf505-145">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bf505-145">Next steps</span></span>
* <span data-ttu-id="bf505-146">Podrobné informace o tom, jak zjistit zdroje dat najdete v tématu [Začínáme s Azure Data Catalog](data-catalog-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="bf505-146">For step-by-step details about how to discover data sources, see [Get Started with Azure Data Catalog](data-catalog-get-started.md).</span></span>
