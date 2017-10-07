---
title: aaaHow tooData zdroje dat.
description: "Jak tooarticle zvýraznění jak toouse dat profilů toounderstand zdroje dat a jak profily tooinclude úrovni tabulky a sloupce dat při registraci zdroje dat v Azure Data Catalog."
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
ms.openlocfilehash: 12c9f38501cdaee903d0dcbbdd0b82395f35a187
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="data-profile-data-sources"></a><span data-ttu-id="cec97-103">Zdroje dat profilů dat</span><span class="sxs-lookup"><span data-stu-id="cec97-103">Data profile data sources</span></span>
## <a name="introduction"></a><span data-ttu-id="cec97-104">Úvod</span><span class="sxs-lookup"><span data-stu-id="cec97-104">Introduction</span></span>
<span data-ttu-id="cec97-105">**Microsoft Azure Data Catalog** je plně spravovaná Cloudová služba, která slouží jako systém registrace a systém pro zjišťování zdrojů dat organizace.</span><span class="sxs-lookup"><span data-stu-id="cec97-105">**Microsoft Azure Data Catalog** is a fully managed cloud service that serves as a system of registration and system of discovery for enterprise data sources.</span></span> <span data-ttu-id="cec97-106">Jinými slovy **Azure Data Catalog** všechny o pomoc osoby zjišťovat, pochopit a používat zdroje dat a pomáhá organizacím tooget další hodnoty ze své stávající data.</span><span class="sxs-lookup"><span data-stu-id="cec97-106">In other words, **Azure Data Catalog** is all about helping people discover, understand, and use data sources, and helping organizations tooget more value from their existing data.</span></span> <span data-ttu-id="cec97-107">Při registraci zdroje dat s **Azure Data Catalog**, jeho metadata se zkopíruje a indexované službou hello, ale není hello scénáře ukončení existuje.</span><span class="sxs-lookup"><span data-stu-id="cec97-107">When a data source is registered with **Azure Data Catalog**, its metadata is copied and indexed by hello service, but hello story doesn’t end there.</span></span>

<span data-ttu-id="cec97-108">Hello **dat profilace** funkce **Azure Data Catalog** prozkoumá hello data z podporovaných zdrojů dat v katalogu a shromažďuje statistické údaje a informace o těchto datech.</span><span class="sxs-lookup"><span data-stu-id="cec97-108">hello **Data Profiling** feature of **Azure Data Catalog** examines hello data from supported data sources in your catalog and collects statistics and information about that data.</span></span> <span data-ttu-id="cec97-109">Je snadno tooinclude profil datových prostředků.</span><span class="sxs-lookup"><span data-stu-id="cec97-109">It's easy tooinclude a profile of your data assets.</span></span> <span data-ttu-id="cec97-110">Když si zaregistrujete datový prostředek, zvolte **zahrnují Data profilu** v nástroj registrace zdroje dat hello.</span><span class="sxs-lookup"><span data-stu-id="cec97-110">When you register a data asset, choose **Include Data Profile** in hello data source registration tool.</span></span>

## <a name="what-is-data-profiling"></a><span data-ttu-id="cec97-111">Co je profilace dat</span><span class="sxs-lookup"><span data-stu-id="cec97-111">What is Data Profiling</span></span>
<span data-ttu-id="cec97-112">Data profilování prozkoumá hello dat ve zdroji dat hello registrované a shromažďuje statistické údaje a informace o těchto datech.</span><span class="sxs-lookup"><span data-stu-id="cec97-112">Data profiling examines hello data in hello data source being registered, and collects statistics and information about that data.</span></span> <span data-ttu-id="cec97-113">Během zjišťování zdroje dat ve statistikách vám pomohou určit hello vhodnosti hello data toosolve obchodního problému.</span><span class="sxs-lookup"><span data-stu-id="cec97-113">During data source discovery, these statistics can help you determine hello suitability of hello data toosolve their business problem.</span></span>

<!-- In [How toodiscover data sources](data-catalog-how-to-discover.md), you learn about **Azure Data Catalog's** extensive search capabilities including searching for data assets that have a profile. See [How tooinclude a data profile when registering a data source](#howto). -->

<span data-ttu-id="cec97-114">Hello následující zdroje dat podporují profilace dat:</span><span class="sxs-lookup"><span data-stu-id="cec97-114">hello following data sources support data profiling:</span></span>

* <span data-ttu-id="cec97-115">SQL Server (včetně databáze SQL Azure a Azure SQL Data Warehouse) tabulek a zobrazení</span><span class="sxs-lookup"><span data-stu-id="cec97-115">SQL Server (including Azure SQL DB and Azure SQL Data Warehouse) tables and views</span></span>
* <span data-ttu-id="cec97-116">Oracle tabulek a zobrazení</span><span class="sxs-lookup"><span data-stu-id="cec97-116">Oracle tables and views</span></span>
* <span data-ttu-id="cec97-117">Teradata tabulek a zobrazení</span><span class="sxs-lookup"><span data-stu-id="cec97-117">Teradata tables and views</span></span>
* <span data-ttu-id="cec97-118">Tabulek Hive</span><span class="sxs-lookup"><span data-stu-id="cec97-118">Hive tables</span></span>

<span data-ttu-id="cec97-119">Včetně profily dat při registraci datových prostředků pomáhá uživatelům odpovězte otázky o zdrojích dat, včetně:</span><span class="sxs-lookup"><span data-stu-id="cec97-119">Including data profiles when registering data assets helps users answer questions about data sources, including:</span></span>

* <span data-ttu-id="cec97-120">Můžete je možné použít toosolve Moje obchodního problému?</span><span class="sxs-lookup"><span data-stu-id="cec97-120">Can it be used toosolve my business problem?</span></span>
* <span data-ttu-id="cec97-121">Hello dat odpovídat tooparticular standardy nebo vzory?</span><span class="sxs-lookup"><span data-stu-id="cec97-121">Does hello data conform tooparticular standards or patterns?</span></span>
* <span data-ttu-id="cec97-122">Jaké jsou některé hello anomálií hello zdroje dat?</span><span class="sxs-lookup"><span data-stu-id="cec97-122">What are some of hello anomalies of hello data source?</span></span>
* <span data-ttu-id="cec97-123">Jaké jsou možné problémy integrace tato data do Moje aplikace?</span><span class="sxs-lookup"><span data-stu-id="cec97-123">What are possible challenges of integrating this data into my application?</span></span>

> [!NOTE]
> <span data-ttu-id="cec97-124">Můžete také přidat dokumentaci tooan asset toodescribe tom, jak může být data integrovaná do aplikace.</span><span class="sxs-lookup"><span data-stu-id="cec97-124">You can also add documentation tooan asset toodescribe how data could be integrated into an application.</span></span> <span data-ttu-id="cec97-125">V tématu [jak zdroje dat toodocument](data-catalog-how-to-documentation.md).</span><span class="sxs-lookup"><span data-stu-id="cec97-125">See [How toodocument data sources](data-catalog-how-to-documentation.md).</span></span>
>
>

<a name="howto"/>

## <a name="how-tooinclude-a-data-profile-when-registering-a-data-source"></a><span data-ttu-id="cec97-126">Jak tooinclude dat profilu při registraci zdroje dat</span><span class="sxs-lookup"><span data-stu-id="cec97-126">How tooinclude a data profile when registering a data source</span></span>
<span data-ttu-id="cec97-127">Je snadno tooinclude profil datového zdroje.</span><span class="sxs-lookup"><span data-stu-id="cec97-127">It's easy tooinclude a profile of your data source.</span></span> <span data-ttu-id="cec97-128">Při registraci zdroje dat, v hello **toobe objekty zaregistrován** panelu hello registrace zdroje dat nástroje, zvolte **zahrnují Data profilu**.</span><span class="sxs-lookup"><span data-stu-id="cec97-128">When you register a data source, in hello **Objects toobe registered** panel of hello data source registration tool, choose **Include Data Profile**.</span></span>

![](media/data-catalog-data-profile/data-catalog-register-profile.png)

<span data-ttu-id="cec97-129">toolearn informace o tom, jak zjistit, tooregister datových zdrojů, [jak zdroje dat tooregister](data-catalog-how-to-register.md) a [Začínáme s Azure Data Catalog](data-catalog-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="cec97-129">toolearn more about how tooregister data sources, see [How tooregister data sources](data-catalog-how-to-register.md) and [Get started with Azure Data Catalog](data-catalog-get-started.md).</span></span>

## <a name="filtering-on-data-assets-that-include-data-profiles"></a><span data-ttu-id="cec97-130">Filtrování datových prostředků, které zahrnují data profily</span><span class="sxs-lookup"><span data-stu-id="cec97-130">Filtering on data assets that include data profiles</span></span>
<span data-ttu-id="cec97-131">toodiscover datových prostředků, které zahrnují data profilu, můžete zahrnout `has:tableDataProfiles` nebo `has:columnsDataProfiles` jako jeden z vašich podmínek vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="cec97-131">toodiscover data assets that include a data profile, you can include `has:tableDataProfiles` or `has:columnsDataProfiles` as one of your search terms.</span></span>

> [!NOTE]
> <span data-ttu-id="cec97-132">Výběr **zahrnují Data profilu** v datech hello zahrnuje nástroj registrace zdroje tabulky a informace o profilu úrovni sloupce.</span><span class="sxs-lookup"><span data-stu-id="cec97-132">Selecting **Include Data Profile** in hello data source registration tool includes both table and column-level profile information.</span></span> <span data-ttu-id="cec97-133">Však hello API katalogu Data Catalog umožňuje toobe prostředky dat registrovány jen jednu sadu zahrnuty informace o profilu.</span><span class="sxs-lookup"><span data-stu-id="cec97-133">However, hello Data Catalog API allows data assets toobe registered with only one set of profile information included.</span></span>
>
>

## <a name="viewing-data-profile-information"></a><span data-ttu-id="cec97-134">Informace o profilu zobrazení dat</span><span class="sxs-lookup"><span data-stu-id="cec97-134">Viewing data profile information</span></span>
<span data-ttu-id="cec97-135">Jakmile zjistíte, zdroj dat vhodný k profilu, můžete zobrazit podrobnosti hello data profilu.</span><span class="sxs-lookup"><span data-stu-id="cec97-135">Once you find a suitable data source with a profile, you can view hello data profile details.</span></span> <span data-ttu-id="cec97-136">tooview hello dat profilu, vyberte datový prostředek a zvolte **Data profilu** v okně portálu hello katalogu Data Catalog.</span><span class="sxs-lookup"><span data-stu-id="cec97-136">tooview hello data profile, select a data asset and choose **Data Profile** in hello Data Catalog portal window.</span></span>

![](media/data-catalog-data-profile/data-catalog-view.png)

<span data-ttu-id="cec97-137">Data profilu v **Azure Data Catalog** zobrazuje tabulky a sloupce profil informace, včetně:</span><span class="sxs-lookup"><span data-stu-id="cec97-137">A data profile in **Azure Data Catalog** shows table and column profile information including:</span></span>

### <a name="object-data-profile"></a><span data-ttu-id="cec97-138">Objekt dat profilu</span><span class="sxs-lookup"><span data-stu-id="cec97-138">Object data profile</span></span>
* <span data-ttu-id="cec97-139">Počet řádků</span><span class="sxs-lookup"><span data-stu-id="cec97-139">Number of rows</span></span>
* <span data-ttu-id="cec97-140">Velikost tabulky</span><span class="sxs-lookup"><span data-stu-id="cec97-140">Table size</span></span>
* <span data-ttu-id="cec97-141">Poslední aktualizace hello objektu</span><span class="sxs-lookup"><span data-stu-id="cec97-141">When hello object was last updated</span></span>

### <a name="column-data-profile"></a><span data-ttu-id="cec97-142">Sloupec dat profilu</span><span class="sxs-lookup"><span data-stu-id="cec97-142">Column data profile</span></span>
* <span data-ttu-id="cec97-143">Datový typ sloupce</span><span class="sxs-lookup"><span data-stu-id="cec97-143">Column data type</span></span>
* <span data-ttu-id="cec97-144">Počet jedinečných hodnot</span><span class="sxs-lookup"><span data-stu-id="cec97-144">Number of distinct values</span></span>
* <span data-ttu-id="cec97-145">Počet řádků s hodnotami NULL</span><span class="sxs-lookup"><span data-stu-id="cec97-145">Number of rows with NULL values</span></span>
* <span data-ttu-id="cec97-146">Minimum, maximum, průměr a standardní odchylce pro hodnoty ve sloupcích</span><span class="sxs-lookup"><span data-stu-id="cec97-146">Minimum, maximum, average, and standard deviation for column values</span></span>

## <a name="summary"></a><span data-ttu-id="cec97-147">Souhrn</span><span class="sxs-lookup"><span data-stu-id="cec97-147">Summary</span></span>
<span data-ttu-id="cec97-148">Data profilování poskytuje statistiky a informace o registrované datové prostředky toohelp určit hello vhodnosti hello data toosolve obchodních problémů.</span><span class="sxs-lookup"><span data-stu-id="cec97-148">Data profiling provides statistics and information about registered data assets toohelp you determine hello suitability of hello data toosolve business problems.</span></span> <span data-ttu-id="cec97-149">Zadávání poznámek a dokumentace zdroje dat, data profily můžete uživatelům lépe pochopili, vaše data.</span><span class="sxs-lookup"><span data-stu-id="cec97-149">Along with annotating, and documenting data sources, data profiles can give users a deeper understanding of your data.</span></span>

## <a name="see-also"></a><span data-ttu-id="cec97-150">Viz také</span><span class="sxs-lookup"><span data-stu-id="cec97-150">See Also</span></span>
* [<span data-ttu-id="cec97-151">Jak tooregister zdroje dat</span><span class="sxs-lookup"><span data-stu-id="cec97-151">How tooregister data sources</span></span>](data-catalog-how-to-register.md)
* [<span data-ttu-id="cec97-152">Začínáme s Azure Data Catalogem</span><span class="sxs-lookup"><span data-stu-id="cec97-152">Get started with Azure Data Catalog</span></span>](data-catalog-get-started.md)
