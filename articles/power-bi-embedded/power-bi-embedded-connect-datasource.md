---
title: "Power BI Embedded - připojování zdroje dat tooa aaaMicrosoft"
description: "Power BI Embedded, připojit toodata zdroje"
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.assetid: 2a4caeb3-255d-4215-9554-0ca8e3568c13
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 01/06/2017
ms.author: asaxton
ms.openlocfilehash: b1aad6e638104716d90f7e1d060eefcbc9daedbc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-tooa-data-source"></a><span data-ttu-id="22099-103">Připojení zdroje dat tooa</span><span class="sxs-lookup"><span data-stu-id="22099-103">Connect tooa data source</span></span>
<span data-ttu-id="22099-104">S **Power BI Embedded**, můžete vložení sestavy do vlastní aplikace.</span><span class="sxs-lookup"><span data-stu-id="22099-104">With **Power BI Embedded**, you can embed reports into your own app.</span></span> <span data-ttu-id="22099-105">Při vložení sestavy Power BI do vaší aplikace hello sestav připojí toohello základní data podle **import** kopii dat hello nebo pomocí **připojení přímo** toohello datový zdroj pomocí  **DirectQuery**.</span><span class="sxs-lookup"><span data-stu-id="22099-105">When you embed a Power BI report into your app, hello report connects toohello underlying data by **importing** a copy of hello data or by **connecting directly** toohello data source using **DirectQuery**.</span></span>

<span data-ttu-id="22099-106">Tady jsou hello rozdíly mezi použitím **Import** a **DirectQuery**.</span><span class="sxs-lookup"><span data-stu-id="22099-106">Here are hello differences between using **Import** and **DirectQuery**.</span></span>

| <span data-ttu-id="22099-107">Import</span><span class="sxs-lookup"><span data-stu-id="22099-107">Import</span></span> | <span data-ttu-id="22099-108">DirectQuery</span><span class="sxs-lookup"><span data-stu-id="22099-108">DirectQuery</span></span> |
| --- | --- |
| <span data-ttu-id="22099-109">Tabulky, sloupce *a data* se naimportují, tedy zkopírují do datové sady sestavu hello.</span><span class="sxs-lookup"><span data-stu-id="22099-109">Tables, columns, *and data* are imported or copied into hello report's dataset.</span></span> <span data-ttu-id="22099-110">toosee změní základní dat došlo k toohello, musíte aktualizovat, nebo importovat, celou aktuální datovou sadu znovu.</span><span class="sxs-lookup"><span data-stu-id="22099-110">toosee changes that occurred toohello underlying data, you must refresh, or import, a complete, current dataset again.</span></span> |<span data-ttu-id="22099-111">Pouze *tabulky a sloupce* se naimportují, tedy zkopírují do datové sady sestavu hello.</span><span class="sxs-lookup"><span data-stu-id="22099-111">Only *tables and columns* are imported or copied into hello report's dataset.</span></span> <span data-ttu-id="22099-112">Vždy zobrazí hello nejnovější data.</span><span class="sxs-lookup"><span data-stu-id="22099-112">You always view hello most current data.</span></span> |

<span data-ttu-id="22099-113">S Power BI Embedded jste můžete použít DirectQuery s cloudovými zdroji dat, ale není místní zdroje dat v tuto chvíli.</span><span class="sxs-lookup"><span data-stu-id="22099-113">With Power BI Embedded, you can use DirectQuery with cloud data sources but not on-premises data sources at this time.</span></span>

> [!NOTE]
> <span data-ttu-id="22099-114">Hello místní brána dat není podporován s Power BI Embedded v tuto chvíli.</span><span class="sxs-lookup"><span data-stu-id="22099-114">hello On-Premises Data Gateway is not supported with Power BI Embedded at this time.</span></span> <span data-ttu-id="22099-115">To znamená, že DirectQuery nelze použít s místní datové zdroje.</span><span class="sxs-lookup"><span data-stu-id="22099-115">This means you cannot use DirectQuery with on-premises data sources.</span></span>

## <a name="supported-data-sources"></a><span data-ttu-id="22099-116">Podporované zdroje dat</span><span class="sxs-lookup"><span data-stu-id="22099-116">Supported data sources</span></span>

<span data-ttu-id="22099-117">**DirectQuery**</span><span class="sxs-lookup"><span data-stu-id="22099-117">**DirectQuery**</span></span>
* <span data-ttu-id="22099-118">Databáze SQL Azure</span><span class="sxs-lookup"><span data-stu-id="22099-118">Azure SQL database</span></span>
* <span data-ttu-id="22099-119">Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="22099-119">Azure SQL Data Warehouse</span></span>

<span data-ttu-id="22099-120">**Import**</span><span class="sxs-lookup"><span data-stu-id="22099-120">**Import**</span></span>

<span data-ttu-id="22099-121">Můžete importovat pomocí všechny hello dostupných zdrojů dat v rámci Power BI Desktop.</span><span class="sxs-lookup"><span data-stu-id="22099-121">You can import using all of hello available data sources within Power BI Desktop.</span></span> <span data-ttu-id="22099-122">Budete **není** být schopný toorefresh dat v rámci Power BI Embedded.</span><span class="sxs-lookup"><span data-stu-id="22099-122">You will **not** be able toorefresh that data within Power BI Embedded.</span></span> <span data-ttu-id="22099-123">Je nutné změny tooupload tooyour soubor PBIX souboru tooPower BI Embedded.</span><span class="sxs-lookup"><span data-stu-id="22099-123">You will have tooupload changes tooyour PBIX file tooPower BI Embedded.</span></span> <span data-ttu-id="22099-124">Toto je z důvodu toono brána k dispozici.</span><span class="sxs-lookup"><span data-stu-id="22099-124">This is due toono available gateway.</span></span> 

## <a name="benefits-of-using-directquery"></a><span data-ttu-id="22099-125">Výhody použití DirectQuery</span><span class="sxs-lookup"><span data-stu-id="22099-125">Benefits of using DirectQuery</span></span>
<span data-ttu-id="22099-126">Existují dvě hlavní výhody při použití **DirectQuery**:</span><span class="sxs-lookup"><span data-stu-id="22099-126">There are two primary benefits when using **DirectQuery**:</span></span>

* <span data-ttu-id="22099-127">**DirectQuery** hello umožňuje vizualizace sestavení v velmi rozsáhlých datových sad, pokud v opačném případě by bylo nebude proveditelné toofirst importovat všechna data.</span><span class="sxs-lookup"><span data-stu-id="22099-127">**DirectQuery** lets you build visualizations over very large datasets, where it otherwise would be unfeasible toofirst import all of hello data.</span></span>
* <span data-ttu-id="22099-128">Základní změny dat může vyžadovat aktualizaci dat a pro některé sestavy hello potřebovat toodisplay aktuální data můžete vyžadovat přenos velkých objemů dat, vytváření nebude proveditelné znovu je importovat data.</span><span class="sxs-lookup"><span data-stu-id="22099-128">Underlying data changes can require a refresh of data, and for some reports, hello need toodisplay current data can require large data transfers, making re-importing data unfeasible.</span></span> <span data-ttu-id="22099-129">Naopak **DirectQuery** sestavy vždy používat aktuální data.</span><span class="sxs-lookup"><span data-stu-id="22099-129">By contrast, **DirectQuery** reports always use current data.</span></span>

## <a name="limitations-of-directquery"></a><span data-ttu-id="22099-130">Omezení DirectQuery</span><span class="sxs-lookup"><span data-stu-id="22099-130">Limitations of DirectQuery</span></span>
   <span data-ttu-id="22099-131">Existuje několik omezení toousing **DirectQuery**:</span><span class="sxs-lookup"><span data-stu-id="22099-131">There are a few limitations toousing **DirectQuery**:</span></span>

* <span data-ttu-id="22099-132">Všechny tabulky musí pocházet z jedné databáze.</span><span class="sxs-lookup"><span data-stu-id="22099-132">All tables must come from a single database.</span></span>
* <span data-ttu-id="22099-133">Pokud hello dotazu je příliš složité, dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="22099-133">If hello query is overly complex, an error will occur.</span></span> <span data-ttu-id="22099-134">Chyba hello tooremedy hello dotazu musí Refaktorovat, tak, aby byl menší komplexní.</span><span class="sxs-lookup"><span data-stu-id="22099-134">tooremedy hello error you must refactor hello query so it is less complex.</span></span> <span data-ttu-id="22099-135">Pokud dotaz hello musí být komplexní, budete potřebovat data hello tooimport místo použití **DirectQuery**.</span><span class="sxs-lookup"><span data-stu-id="22099-135">If hello query must be complex, you will need tooimport hello data instead of using **DirectQuery**.</span></span>
* <span data-ttu-id="22099-136">Filtrování relace je omezená tooa jeden směr, nikoli obou směrech.</span><span class="sxs-lookup"><span data-stu-id="22099-136">Relationship filtering is limited tooa single direction, rather than both directions.</span></span>
* <span data-ttu-id="22099-137">Nelze změnit hello datový typ sloupce.</span><span class="sxs-lookup"><span data-stu-id="22099-137">You cannot change hello data type of a column.</span></span>
* <span data-ttu-id="22099-138">Ve výchozím omezení jsou umístěny na výrazy jazyka DAX v míry povoleny.</span><span class="sxs-lookup"><span data-stu-id="22099-138">By default, limitations are placed on DAX expressions allowed in measures.</span></span> <span data-ttu-id="22099-139">V tématu [DirectQuery a míry](#measures).</span><span class="sxs-lookup"><span data-stu-id="22099-139">See [DirectQuery and measures](#measures).</span></span>

<a name="measures"/>

## <a name="directquery-and-measures"></a><span data-ttu-id="22099-140">DirectQuery a míry.</span><span class="sxs-lookup"><span data-stu-id="22099-140">DirectQuery and measures</span></span>
<span data-ttu-id="22099-141">tooensure dotazů odesílaných toohello podkladové zdroje dat mají přijatelný výkon, jsou omezení vynucená pro míry.</span><span class="sxs-lookup"><span data-stu-id="22099-141">tooensure queries sent toohello underlying data source have acceptable performance, limitations are imposed on measures.</span></span> <span data-ttu-id="22099-142">Při použití **Power BI Desktop**rozšířené uživatelé mohou toobypass toto omezení výběrem **soubor > Možnosti a nastavení > Možnosti**.</span><span class="sxs-lookup"><span data-stu-id="22099-142">When using **Power BI Desktop**, advanced users can choose toobypass this limitation by choosing **File > Options and settings > Options**.</span></span> <span data-ttu-id="22099-143">V hello **možnosti** dialogovém okně, vyberte **DirectQuery**a vyberte možnost hello **Povolit neomezené míry v režimu DirectQuery**.</span><span class="sxs-lookup"><span data-stu-id="22099-143">In hello **Options** dialog, choose **DirectQuery**, and select hello option **Allow unrestricted measures in DirectQuery mode**.</span></span> <span data-ttu-id="22099-144">Pokud je vybraná tato možnost, lze použít jakýkoli DAX výraz, který je platný pro míru.</span><span class="sxs-lookup"><span data-stu-id="22099-144">When that option is selected, any DAX expression that is valid for a measure can be used.</span></span> <span data-ttu-id="22099-145">Uživatelé musí být vědomi; ale, že některé výrazy, provádět velmi dobře při importu dat hello může vést k velmi pomalé dotazy toohello back-end zdroj při v **DirectQuery** režimu.</span><span class="sxs-lookup"><span data-stu-id="22099-145">Users must be aware; however, that some expressions that perform very well when hello data is imported may result in very slow queries toohello backend source when in **DirectQuery** mode.</span></span> 

## <a name="see-also"></a><span data-ttu-id="22099-146">Viz také</span><span class="sxs-lookup"><span data-stu-id="22099-146">See Also</span></span>
* [<span data-ttu-id="22099-147">Začínáme s Microsoft Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="22099-147">Get started with Microsoft Power BI Embedded</span></span>](power-bi-embedded-get-started.md)
* [<span data-ttu-id="22099-148">Power BI Desktop</span><span class="sxs-lookup"><span data-stu-id="22099-148">Power BI Desktop</span></span>](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)

<span data-ttu-id="22099-149">Chcete se ještě na něco zeptat?</span><span class="sxs-lookup"><span data-stu-id="22099-149">More questions?</span></span> [<span data-ttu-id="22099-150">Zkuste hello komunitě Power BI</span><span class="sxs-lookup"><span data-stu-id="22099-150">Try hello Power BI Community</span></span>](http://community.powerbi.com/)

