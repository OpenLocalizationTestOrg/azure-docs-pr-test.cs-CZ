---
title: "Jak pracovat se zdroji dat 'velkých objemů dat. | Microsoft Docs"
description: "Postupy: článek zvýraznění vzory pro pomocí Azure Data Catalog, velké objemy dat, datových zdrojů, včetně Azure Blob Storage, Azure Data Lake a Hadoop HDFS."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 626d1568-0780-4726-bad1-9c5000c6b31a
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: 001d80ce42f0e87276e59d70dffb75eb561d96cd
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-work-with-big-data-sources-in-azure-data-catalog"></a><span data-ttu-id="f6f99-103">Jak pracovat s velkými objemy dat zdroje v Azure Data Catalog</span><span class="sxs-lookup"><span data-stu-id="f6f99-103">How to work with big data sources in Azure Data Catalog</span></span>
## <a name="introduction"></a><span data-ttu-id="f6f99-104">Úvod</span><span class="sxs-lookup"><span data-stu-id="f6f99-104">Introduction</span></span>
<span data-ttu-id="f6f99-105">**Microsoft Azure Data Catalog** je plně spravovaná Cloudová služba, která slouží jako systém registrace a systém pro zjišťování zdrojů dat organizace.</span><span class="sxs-lookup"><span data-stu-id="f6f99-105">**Microsoft Azure Data Catalog** is a fully managed cloud service that serves as a system of registration and system of discovery for enterprise data sources.</span></span> <span data-ttu-id="f6f99-106">Všechny o pomoc osoby zjišťovat, pochopit a používat zdroje dat a pomáhají organizacím získat větší hodnotu ze svých existujících zdrojů dat, včetně velkých objemů dat je.</span><span class="sxs-lookup"><span data-stu-id="f6f99-106">It is all about helping people discover, understand, and use data sources, and helping organizations to get more value from their existing data sources, including big data.</span></span>

<span data-ttu-id="f6f99-107">**Azure Data Catalog** podporuje registraci Azure Blog úložiště objektů BLOB a adresáře a také Hadoop HDFS souborů a adresářů.</span><span class="sxs-lookup"><span data-stu-id="f6f99-107">**Azure Data Catalog** supports the registration of Azure Blog Storage blobs and directories as well as Hadoop HDFS files and directories.</span></span> <span data-ttu-id="f6f99-108">Částečně strukturovaných povahu těchto zdrojů dat poskytuje flexibilitu.</span><span class="sxs-lookup"><span data-stu-id="f6f99-108">The semi-structured nature of these data sources provides great flexibility.</span></span> <span data-ttu-id="f6f99-109">Však sám registrace je s největším hodnotu **Azure Data Catalog**, uživatelé, musíte zvážit, jak jsou uspořádány zdroje dat.</span><span class="sxs-lookup"><span data-stu-id="f6f99-109">However, to get the most value from registering them with **Azure Data Catalog**, users must consider how the data sources are organized.</span></span>

## <a name="directories-as-logical-data-sets"></a><span data-ttu-id="f6f99-110">Adresářů jako logické datové sady</span><span class="sxs-lookup"><span data-stu-id="f6f99-110">Directories as logical data sets</span></span>
<span data-ttu-id="f6f99-111">Běžný vzor uspořádání zdroje velkých objemů dat je adresáře považovat za logické datové sady.</span><span class="sxs-lookup"><span data-stu-id="f6f99-111">A common pattern for organizing big data sources is to treat directories as logical data sets.</span></span> <span data-ttu-id="f6f99-112">Nejvyšší úrovně adresáře se používají k definování sady dat, zatímco podsložky Definujte oddíly a ukládat soubory, které obsahují samotná data.</span><span class="sxs-lookup"><span data-stu-id="f6f99-112">Top-level directories are used to define a data set, while subfolders define partitions, and the files they contain store the data itself.</span></span>

<span data-ttu-id="f6f99-113">Příkladem tohoto vzoru může být:</span><span class="sxs-lookup"><span data-stu-id="f6f99-113">An example of this pattern might be:</span></span>

    \vehicle_maintenance_events
        \2013
        \2014
        \2015
            \01
                \2015-01-trailer01.csv
                \2015-01-trailer92.csv
                \2015-01-canister9635.csv
                ...
    \location_tracking_events
        \2013
        ...

<span data-ttu-id="f6f99-114">V tomto příkladu vehicle_maintenance_events a location_tracking_events představují logické datové sady.</span><span class="sxs-lookup"><span data-stu-id="f6f99-114">In this example, vehicle_maintenance_events and location_tracking_events represent logical data sets.</span></span> <span data-ttu-id="f6f99-115">Každou z těchto složek obsahuje datové soubory, které jsou uspořádané podle rok a měsíc do podsložky.</span><span class="sxs-lookup"><span data-stu-id="f6f99-115">Each of these folders contains data files that are organized by year and month into subfolders.</span></span> <span data-ttu-id="f6f99-116">Každou z těchto složek mohou obsahovat stovky nebo tisíce souborů.</span><span class="sxs-lookup"><span data-stu-id="f6f99-116">Each of these folders could potentially contain hundreds or thousands of files.</span></span>

<span data-ttu-id="f6f99-117">V tomto vzoru registraci jednotlivých souborů s **Azure Data Catalog** pravděpodobně nemá smysl.</span><span class="sxs-lookup"><span data-stu-id="f6f99-117">In this pattern, registering individual files with **Azure Data Catalog** probably does not make sense.</span></span> <span data-ttu-id="f6f99-118">Místo toho zaregistrujte adresáře, které představují datových sad, které smysl uživatelům práci s daty.</span><span class="sxs-lookup"><span data-stu-id="f6f99-118">Instead, register the directories that represent the data sets that be meaningful to the users working with the data.</span></span>

## <a name="reference-data-files"></a><span data-ttu-id="f6f99-119">Referenční datové soubory</span><span class="sxs-lookup"><span data-stu-id="f6f99-119">Reference data files</span></span>
<span data-ttu-id="f6f99-120">Doplňkové vzor je ukládání referenční datové sady jako jednotlivé soubory.</span><span class="sxs-lookup"><span data-stu-id="f6f99-120">A complementary pattern is to store reference data sets as individual files.</span></span> <span data-ttu-id="f6f99-121">Tyto sady dat může být představit jako "malá" straně velkých objemů dat a jsou často podobná dimenze ve model analytická data.</span><span class="sxs-lookup"><span data-stu-id="f6f99-121">These data sets may be thought of as the 'small' side of big data, and are often similar to dimensions in an analytical data model.</span></span> <span data-ttu-id="f6f99-122">Referenční datové soubory obsahuje záznamy, které se používají k poskytování kontext pro hromadné souborů data uložená na jiném místě v úložišti velkých objemů dat.</span><span class="sxs-lookup"><span data-stu-id="f6f99-122">Reference data files contain records that are used to provide context for the bulk of the data files stored elsewhere in the big data store.</span></span>

<span data-ttu-id="f6f99-123">Příkladem tohoto vzoru může být:</span><span class="sxs-lookup"><span data-stu-id="f6f99-123">An example of this pattern might be:</span></span>

    \vehicles.csv
    \maintenance_facilities.csv
    \maintenance_types.csv

<span data-ttu-id="f6f99-124">Když analytik nebo data vědecký pracovník pracuje s daty součástí větší struktury adresářů, data v těchto referenčních souborů slouží k poskytují podrobnější informace pro entity, které jsou uvedené jenom podle názvu nebo ID v větší datové sady.</span><span class="sxs-lookup"><span data-stu-id="f6f99-124">When an analyst or data scientist is working with the data contained in the larger directory structures, the data in these reference files can be used to provide more detailed information for entities that are referred to only by name or ID in the larger data set.</span></span>

<span data-ttu-id="f6f99-125">V tomto vzoru, má smysl registraci jednotlivých referenčních dat souborů s **Azure Data Catalog**.</span><span class="sxs-lookup"><span data-stu-id="f6f99-125">In this pattern, it makes sense to register the individual reference data files with **Azure Data Catalog**.</span></span> <span data-ttu-id="f6f99-126">Každý soubor představuje datové sady a každé z nich můžou opatřeny poznámkami a zjištěné jednotlivě.</span><span class="sxs-lookup"><span data-stu-id="f6f99-126">Each file represents a data set, and each one can be annotated and discovered individually.</span></span>

## <a name="alternate-patterns"></a><span data-ttu-id="f6f99-127">Alternativní vzory</span><span class="sxs-lookup"><span data-stu-id="f6f99-127">Alternate patterns</span></span>
<span data-ttu-id="f6f99-128">Vzory popsané v předchozí části jsou právě dva možné způsoby, které může být uspořádaný velkých objemů dat úložiště, ale každý implementace se liší.</span><span class="sxs-lookup"><span data-stu-id="f6f99-128">The patterns described in the preceding section are just two possible ways a big data store may be organized, but each implementation is different.</span></span> <span data-ttu-id="f6f99-129">Bez ohledu na to, jak jsou strukturovaná zdroje dat, při registraci zdroje velkých objemů dat s **Azure Data Catalog**, soustředit na registraci soubory a adresáře, které představují datových sad, které jsou hodnoty s ostatními uživateli v vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="f6f99-129">Regardless of how your data sources are structured, when registering big data sources with **Azure Data Catalog**, focus on registering the files and directories that represent the data sets that are of value to others within your organization.</span></span> <span data-ttu-id="f6f99-130">Registrace všechny soubory a adresáře může zaplnit katalogu, což těžší uživatelům najít požadované.</span><span class="sxs-lookup"><span data-stu-id="f6f99-130">Registering all files and directories can clutter the catalog, making it harder for users to find what they need.</span></span>

## <a name="summary"></a><span data-ttu-id="f6f99-131">Souhrn</span><span class="sxs-lookup"><span data-stu-id="f6f99-131">Summary</span></span>
<span data-ttu-id="f6f99-132">Registrace zdroje dat s **Azure Data Catalog** jejich jednodušší zjišťovat a pochopit.</span><span class="sxs-lookup"><span data-stu-id="f6f99-132">Registering data sources with **Azure Data Catalog** makes them easier to discover and understand.</span></span> <span data-ttu-id="f6f99-133">Registrace a zadávání poznámek k velkých objemů dat souborů a adresářů, které představují logické datové sady, může pomoci uživatelům najít a použít zdroje velkých objemů dat, které potřebují.</span><span class="sxs-lookup"><span data-stu-id="f6f99-133">By registering and annotating the big data files and directories that represent logical data sets, you can help users find and use the big data sources they need.</span></span>
