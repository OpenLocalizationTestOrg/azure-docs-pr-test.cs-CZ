---
title: "Postup datového zdroje dat."
description: "Postupy: článek zvýraznění postup zahrnují profily úrovni tabulky a sloupce dat, při registraci zdroje dat v Azure Data Catalog a pochopit zdroje dat pomocí profilů data."
services: data-catalog
documentationcenter: 
author: spelluru
manager: NA
editor: 
tags: 
ms.assetid: 94a8274b-5c9c-4962-a4b1-2fed38a3d919
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/03/2017
ms.author: spelluru
ms.openlocfilehash: 8f4174f0ed74706b8275c8b1f0a62753f2834fa2
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="data-profile-data-sources"></a><span data-ttu-id="c3bd6-103">Zdroje dat profilů dat</span><span class="sxs-lookup"><span data-stu-id="c3bd6-103">Data profile data sources</span></span>
## <a name="introduction"></a><span data-ttu-id="c3bd6-104">Úvod</span><span class="sxs-lookup"><span data-stu-id="c3bd6-104">Introduction</span></span>
<span data-ttu-id="c3bd6-105">**Microsoft Azure Data Catalog** je plně spravovaná Cloudová služba, která slouží jako systém registrace a systém pro zjišťování zdrojů dat organizace.</span><span class="sxs-lookup"><span data-stu-id="c3bd6-105">**Microsoft Azure Data Catalog** is a fully managed cloud service that serves as a system of registration and system of discovery for enterprise data sources.</span></span> <span data-ttu-id="c3bd6-106">Jinými slovy **Azure Data Catalog** se točí kolem osoby zjišťovat, pochopit a používat zdroje dat a pomáhá organizacím získat větší hodnotu z jejich stávající data.</span><span class="sxs-lookup"><span data-stu-id="c3bd6-106">In other words, **Azure Data Catalog** is all about helping people discover, understand, and use data sources, and helping organizations to get more value from their existing data.</span></span> <span data-ttu-id="c3bd6-107">Při registraci zdroje dat s **Azure Data Catalog**, jeho metadata se zkopíruje a indexované službou, ale není článek existuje ukončení.</span><span class="sxs-lookup"><span data-stu-id="c3bd6-107">When a data source is registered with **Azure Data Catalog**, its metadata is copied and indexed by the service, but the story doesn’t end there.</span></span>

<span data-ttu-id="c3bd6-108">**Dat profilace** funkce **Azure Data Catalog** prozkoumá data z podporovaných zdrojů dat v katalogu a shromažďuje statistické údaje a informace o těchto datech.</span><span class="sxs-lookup"><span data-stu-id="c3bd6-108">The **Data Profiling** feature of **Azure Data Catalog** examines the data from supported data sources in your catalog and collects statistics and information about that data.</span></span> <span data-ttu-id="c3bd6-109">Je snadné zahrnout profil datových prostředků.</span><span class="sxs-lookup"><span data-stu-id="c3bd6-109">It's easy to include a profile of your data assets.</span></span> <span data-ttu-id="c3bd6-110">Když si zaregistrujete datový prostředek, zvolte **zahrnují Data profilu** v nástroj registrace zdroje dat.</span><span class="sxs-lookup"><span data-stu-id="c3bd6-110">When you register a data asset, choose **Include Data Profile** in the data source registration tool.</span></span>

## <a name="what-is-data-profiling"></a><span data-ttu-id="c3bd6-111">Co je profilace dat</span><span class="sxs-lookup"><span data-stu-id="c3bd6-111">What is Data Profiling</span></span>
<span data-ttu-id="c3bd6-112">Data profilování prozkoumá dat ve zdroji dat, která je registrována a shromažďuje statistické údaje a informace o těchto datech.</span><span class="sxs-lookup"><span data-stu-id="c3bd6-112">Data profiling examines the data in the data source being registered, and collects statistics and information about that data.</span></span> <span data-ttu-id="c3bd6-113">Během zjišťování zdroje dat ve statistikách vám pomohou určit, vhodnosti data k vyřešení obchodního problému.</span><span class="sxs-lookup"><span data-stu-id="c3bd6-113">During data source discovery, these statistics can help you determine the suitability of the data to solve their business problem.</span></span>

<!-- In [How to discover data sources](data-catalog-how-to-discover.md), you learn about **Azure Data Catalog's** extensive search capabilities including searching for data assets that have a profile. See [How to include a data profile when registering a data source](#howto). -->

<span data-ttu-id="c3bd6-114">Následující zdroje dat podporují profilace dat:</span><span class="sxs-lookup"><span data-stu-id="c3bd6-114">The following data sources support data profiling:</span></span>

* <span data-ttu-id="c3bd6-115">SQL Server (včetně databáze SQL Azure a Azure SQL Data Warehouse) tabulek a zobrazení</span><span class="sxs-lookup"><span data-stu-id="c3bd6-115">SQL Server (including Azure SQL DB and Azure SQL Data Warehouse) tables and views</span></span>
* <span data-ttu-id="c3bd6-116">Oracle tabulek a zobrazení</span><span class="sxs-lookup"><span data-stu-id="c3bd6-116">Oracle tables and views</span></span>
* <span data-ttu-id="c3bd6-117">Teradata tabulek a zobrazení</span><span class="sxs-lookup"><span data-stu-id="c3bd6-117">Teradata tables and views</span></span>
* <span data-ttu-id="c3bd6-118">Tabulek Hive</span><span class="sxs-lookup"><span data-stu-id="c3bd6-118">Hive tables</span></span>

<span data-ttu-id="c3bd6-119">Včetně profily dat při registraci datových prostředků pomáhá uživatelům odpovězte otázky o zdrojích dat, včetně:</span><span class="sxs-lookup"><span data-stu-id="c3bd6-119">Including data profiles when registering data assets helps users answer questions about data sources, including:</span></span>

* <span data-ttu-id="c3bd6-120">Ji lze vyřešit Moje obchodního problému?</span><span class="sxs-lookup"><span data-stu-id="c3bd6-120">Can it be used to solve my business problem?</span></span>
* <span data-ttu-id="c3bd6-121">Data v souladu s konkrétní standardy nebo vzory?</span><span class="sxs-lookup"><span data-stu-id="c3bd6-121">Does the data conform to particular standards or patterns?</span></span>
* <span data-ttu-id="c3bd6-122">Jaké jsou některé anomálií zdroje dat?</span><span class="sxs-lookup"><span data-stu-id="c3bd6-122">What are some of the anomalies of the data source?</span></span>
* <span data-ttu-id="c3bd6-123">Jaké jsou možné problémy integrace tato data do Moje aplikace?</span><span class="sxs-lookup"><span data-stu-id="c3bd6-123">What are possible challenges of integrating this data into my application?</span></span>

> [!NOTE]
> <span data-ttu-id="c3bd6-124">Můžete také přidat dokumentace pro prostředek k popisu, jak může integrovat dat do aplikace.</span><span class="sxs-lookup"><span data-stu-id="c3bd6-124">You can also add documentation to an asset to describe how data could be integrated into an application.</span></span> <span data-ttu-id="c3bd6-125">V tématu [postup dokumentování zdrojů dat](data-catalog-how-to-documentation.md).</span><span class="sxs-lookup"><span data-stu-id="c3bd6-125">See [How to document data sources](data-catalog-how-to-documentation.md).</span></span>
>
>

<a name="howto"/>

## <a name="how-to-include-a-data-profile-when-registering-a-data-source"></a><span data-ttu-id="c3bd6-126">Jak se zahrnuje profil dat při registraci zdroje dat</span><span class="sxs-lookup"><span data-stu-id="c3bd6-126">How to include a data profile when registering a data source</span></span>
<span data-ttu-id="c3bd6-127">Je snadné zahrnout profil datového zdroje.</span><span class="sxs-lookup"><span data-stu-id="c3bd6-127">It's easy to include a profile of your data source.</span></span> <span data-ttu-id="c3bd6-128">Při registraci zdroje dat, v **objekty k registraci** panel Nástroj registrace zdroje dat, vyberte **zahrnují Data profilu**.</span><span class="sxs-lookup"><span data-stu-id="c3bd6-128">When you register a data source, in the **Objects to be registered** panel of the data source registration tool, choose **Include Data Profile**.</span></span>

![](media/data-catalog-data-profile/data-catalog-register-profile.png)

<span data-ttu-id="c3bd6-129">Další informace o tom, jak registrace zdrojů dat naleznete v tématu [postup registrace zdrojů dat](data-catalog-how-to-register.md) a [Začínáme s Azure Data Catalog](data-catalog-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="c3bd6-129">To learn more about how to register data sources, see [How to register data sources](data-catalog-how-to-register.md) and [Get started with Azure Data Catalog](data-catalog-get-started.md).</span></span>

## <a name="filtering-on-data-assets-that-include-data-profiles"></a><span data-ttu-id="c3bd6-130">Filtrování datových prostředků, které zahrnují data profily</span><span class="sxs-lookup"><span data-stu-id="c3bd6-130">Filtering on data assets that include data profiles</span></span>
<span data-ttu-id="c3bd6-131">Pokud chcete zjistit, datové prostředky, které zahrnují data profilu, můžete zahrnout `has:tableDataProfiles` nebo `has:columnsDataProfiles` jako jeden z vašich podmínek vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="c3bd6-131">To discover data assets that include a data profile, you can include `has:tableDataProfiles` or `has:columnsDataProfiles` as one of your search terms.</span></span>

> [!NOTE]
> <span data-ttu-id="c3bd6-132">Výběr **zahrnují Data profilu** ve zdroji dat zahrnuje nástroj pro registraci tabulky a informace o profilu úrovni sloupce.</span><span class="sxs-lookup"><span data-stu-id="c3bd6-132">Selecting **Include Data Profile** in the data source registration tool includes both table and column-level profile information.</span></span> <span data-ttu-id="c3bd6-133">Rozhraní API katalogu Data Catalog umožňuje však datové prostředky se mají být zaregistrována jen jednu sadu zahrnuty informace o profilu.</span><span class="sxs-lookup"><span data-stu-id="c3bd6-133">However, the Data Catalog API allows data assets to be registered with only one set of profile information included.</span></span>
>
>

## <a name="viewing-data-profile-information"></a><span data-ttu-id="c3bd6-134">Informace o profilu zobrazení dat</span><span class="sxs-lookup"><span data-stu-id="c3bd6-134">Viewing data profile information</span></span>
<span data-ttu-id="c3bd6-135">Jakmile zjistíte, zdroj dat vhodný k profilu, můžete zobrazit podrobnosti dat profilu.</span><span class="sxs-lookup"><span data-stu-id="c3bd6-135">Once you find a suitable data source with a profile, you can view the data profile details.</span></span> <span data-ttu-id="c3bd6-136">Chcete-li zobrazit data profilu, vyberte datový prostředek a zvolte **Data profilu** v okně portálu katalogu Data Catalog.</span><span class="sxs-lookup"><span data-stu-id="c3bd6-136">To view the data profile, select a data asset and choose **Data Profile** in the Data Catalog portal window.</span></span>

![](media/data-catalog-data-profile/data-catalog-view.png)

<span data-ttu-id="c3bd6-137">Data profilu v **Azure Data Catalog** zobrazuje tabulky a sloupce profil informace, včetně:</span><span class="sxs-lookup"><span data-stu-id="c3bd6-137">A data profile in **Azure Data Catalog** shows table and column profile information including:</span></span>

### <a name="object-data-profile"></a><span data-ttu-id="c3bd6-138">Objekt dat profilu</span><span class="sxs-lookup"><span data-stu-id="c3bd6-138">Object data profile</span></span>
* <span data-ttu-id="c3bd6-139">Počet řádků</span><span class="sxs-lookup"><span data-stu-id="c3bd6-139">Number of rows</span></span>
* <span data-ttu-id="c3bd6-140">Velikost tabulky</span><span class="sxs-lookup"><span data-stu-id="c3bd6-140">Table size</span></span>
* <span data-ttu-id="c3bd6-141">Poslední aktualizace objektu</span><span class="sxs-lookup"><span data-stu-id="c3bd6-141">When the object was last updated</span></span>

### <a name="column-data-profile"></a><span data-ttu-id="c3bd6-142">Sloupec dat profilu</span><span class="sxs-lookup"><span data-stu-id="c3bd6-142">Column data profile</span></span>
* <span data-ttu-id="c3bd6-143">Datový typ sloupce</span><span class="sxs-lookup"><span data-stu-id="c3bd6-143">Column data type</span></span>
* <span data-ttu-id="c3bd6-144">Počet jedinečných hodnot</span><span class="sxs-lookup"><span data-stu-id="c3bd6-144">Number of distinct values</span></span>
* <span data-ttu-id="c3bd6-145">Počet řádků s hodnotami NULL</span><span class="sxs-lookup"><span data-stu-id="c3bd6-145">Number of rows with NULL values</span></span>
* <span data-ttu-id="c3bd6-146">Minimum, maximum, průměr a standardní odchylce pro hodnoty ve sloupcích</span><span class="sxs-lookup"><span data-stu-id="c3bd6-146">Minimum, maximum, average, and standard deviation for column values</span></span>

## <a name="summary"></a><span data-ttu-id="c3bd6-147">Souhrn</span><span class="sxs-lookup"><span data-stu-id="c3bd6-147">Summary</span></span>
<span data-ttu-id="c3bd6-148">Data profilování poskytuje statistiky a informace o registrovaných datových prostředků můžete zjistit vhodnosti dat k řešení obchodních problémů.</span><span class="sxs-lookup"><span data-stu-id="c3bd6-148">Data profiling provides statistics and information about registered data assets to help you determine the suitability of the data to solve business problems.</span></span> <span data-ttu-id="c3bd6-149">Zadávání poznámek a dokumentace zdroje dat, data profily můžete uživatelům lépe pochopili, vaše data.</span><span class="sxs-lookup"><span data-stu-id="c3bd6-149">Along with annotating, and documenting data sources, data profiles can give users a deeper understanding of your data.</span></span>

## <a name="see-also"></a><span data-ttu-id="c3bd6-150">Viz také</span><span class="sxs-lookup"><span data-stu-id="c3bd6-150">See Also</span></span>
* [<span data-ttu-id="c3bd6-151">Postup registrace zdrojů dat</span><span class="sxs-lookup"><span data-stu-id="c3bd6-151">How to register data sources</span></span>](data-catalog-how-to-register.md)
* [<span data-ttu-id="c3bd6-152">Začínáme s Azure Data Catalogem</span><span class="sxs-lookup"><span data-stu-id="c3bd6-152">Get started with Azure Data Catalog</span></span>](data-catalog-get-started.md)
