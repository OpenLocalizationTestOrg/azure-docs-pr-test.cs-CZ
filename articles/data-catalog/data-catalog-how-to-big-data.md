---
title: "aaaHow toowork se zdroji dat 'velkých objemů dat. | Microsoft Docs"
description: "Jak tooarticle zvýraznění vzory pro pomocí Azure Data Catalog, velké objemy dat, datových zdrojů, včetně Azure Blob Storage, Azure Data Lake a Hadoop HDFS."
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
ms.openlocfilehash: e478f71f26744975a7d7e1784b74bf50b424cf65
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toowork-with-big-data-sources-in-azure-data-catalog"></a><span data-ttu-id="99644-103">Jak zdroje toowork s velkým objemem dat v Azure Data Catalog</span><span class="sxs-lookup"><span data-stu-id="99644-103">How toowork with big data sources in Azure Data Catalog</span></span>
## <a name="introduction"></a><span data-ttu-id="99644-104">Úvod</span><span class="sxs-lookup"><span data-stu-id="99644-104">Introduction</span></span>
<span data-ttu-id="99644-105">**Microsoft Azure Data Catalog** je plně spravovaná Cloudová služba, která slouží jako systém registrace a systém pro zjišťování zdrojů dat organizace.</span><span class="sxs-lookup"><span data-stu-id="99644-105">**Microsoft Azure Data Catalog** is a fully managed cloud service that serves as a system of registration and system of discovery for enterprise data sources.</span></span> <span data-ttu-id="99644-106">Všechno se točí kolem osoby zjišťovat, pochopit a používat zdroje dat a pomáhá organizacím tooget větší hodnotu ze svých existujících zdrojů dat, včetně velkých objemů dat.</span><span class="sxs-lookup"><span data-stu-id="99644-106">It is all about helping people discover, understand, and use data sources, and helping organizations tooget more value from their existing data sources, including big data.</span></span>

<span data-ttu-id="99644-107">**Azure Data Catalog** podporuje hello registrace Azure Blog úložiště objektů BLOB a adresáře a také Hadoop HDFS souborů a adresářů.</span><span class="sxs-lookup"><span data-stu-id="99644-107">**Azure Data Catalog** supports hello registration of Azure Blog Storage blobs and directories as well as Hadoop HDFS files and directories.</span></span> <span data-ttu-id="99644-108">Hello částečně strukturovaných povaha tyto zdroje dat poskytuje flexibilitu.</span><span class="sxs-lookup"><span data-stu-id="99644-108">hello semi-structured nature of these data sources provides great flexibility.</span></span> <span data-ttu-id="99644-109">Ale tooget hello maximální hodnotu registraci pomocí **Azure Data Catalog**, uživatelé musíte zvážit, jak jsou uspořádány zdroje dat hello.</span><span class="sxs-lookup"><span data-stu-id="99644-109">However, tooget hello most value from registering them with **Azure Data Catalog**, users must consider how hello data sources are organized.</span></span>

## <a name="directories-as-logical-data-sets"></a><span data-ttu-id="99644-110">Adresářů jako logické datové sady</span><span class="sxs-lookup"><span data-stu-id="99644-110">Directories as logical data sets</span></span>
<span data-ttu-id="99644-111">Běžný vzor uspořádání zdroje velkých objemů dat je tootreat adresářů jako logické datové sady.</span><span class="sxs-lookup"><span data-stu-id="99644-111">A common pattern for organizing big data sources is tootreat directories as logical data sets.</span></span> <span data-ttu-id="99644-112">Nejvyšší úrovně adresáře jsou použité toodefine datové sady, zatímco podsložky Definujte oddíly a hello soubory, které obsahují ukládání hello samotná data.</span><span class="sxs-lookup"><span data-stu-id="99644-112">Top-level directories are used toodefine a data set, while subfolders define partitions, and hello files they contain store hello data itself.</span></span>

<span data-ttu-id="99644-113">Příkladem tohoto vzoru může být:</span><span class="sxs-lookup"><span data-stu-id="99644-113">An example of this pattern might be:</span></span>

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

<span data-ttu-id="99644-114">V tomto příkladu vehicle_maintenance_events a location_tracking_events představují logické datové sady.</span><span class="sxs-lookup"><span data-stu-id="99644-114">In this example, vehicle_maintenance_events and location_tracking_events represent logical data sets.</span></span> <span data-ttu-id="99644-115">Každou z těchto složek obsahuje datové soubory, které jsou uspořádané podle rok a měsíc do podsložky.</span><span class="sxs-lookup"><span data-stu-id="99644-115">Each of these folders contains data files that are organized by year and month into subfolders.</span></span> <span data-ttu-id="99644-116">Každou z těchto složek mohou obsahovat stovky nebo tisíce souborů.</span><span class="sxs-lookup"><span data-stu-id="99644-116">Each of these folders could potentially contain hundreds or thousands of files.</span></span>

<span data-ttu-id="99644-117">V tomto vzoru registraci jednotlivých souborů s **Azure Data Catalog** pravděpodobně nemá smysl.</span><span class="sxs-lookup"><span data-stu-id="99644-117">In this pattern, registering individual files with **Azure Data Catalog** probably does not make sense.</span></span> <span data-ttu-id="99644-118">Místo toho zaregistrujte hello adresáře, které představují hello datových sad, které se smysluplný toohello uživatelů, kteří pracují s daty hello.</span><span class="sxs-lookup"><span data-stu-id="99644-118">Instead, register hello directories that represent hello data sets that be meaningful toohello users working with hello data.</span></span>

## <a name="reference-data-files"></a><span data-ttu-id="99644-119">Referenční datové soubory</span><span class="sxs-lookup"><span data-stu-id="99644-119">Reference data files</span></span>
<span data-ttu-id="99644-120">Doplňkové vzor je toostore referenční datové sady jako jednotlivé soubory.</span><span class="sxs-lookup"><span data-stu-id="99644-120">A complementary pattern is toostore reference data sets as individual files.</span></span> <span data-ttu-id="99644-121">Tyto sady dat může považovat za hello "malá" straně velkých objemů dat a jsou často podobné toodimensions ve model analytickými daty.</span><span class="sxs-lookup"><span data-stu-id="99644-121">These data sets may be thought of as hello 'small' side of big data, and are often similar toodimensions in an analytical data model.</span></span> <span data-ttu-id="99644-122">Referenčních dat souborů obsahuje záznamy, které jsou používané tooprovide kontext pro hromadné hello hello souborů data uložená na jiném místě v úložišti velkých objemů dat hello.</span><span class="sxs-lookup"><span data-stu-id="99644-122">Reference data files contain records that are used tooprovide context for hello bulk of hello data files stored elsewhere in hello big data store.</span></span>

<span data-ttu-id="99644-123">Příkladem tohoto vzoru může být:</span><span class="sxs-lookup"><span data-stu-id="99644-123">An example of this pattern might be:</span></span>

    \vehicles.csv
    \maintenance_facilities.csv
    \maintenance_types.csv

<span data-ttu-id="99644-124">Při vědecký pracovník analytik nebo data pracuje s daty hello součástí větší struktury adresářů hello, hello data v těchto souborech odkaz může být použité tooprovide podrobnější informace pro entity, které jsou označuje tooonly podle názvu nebo ID v datech větší hello Sada.</span><span class="sxs-lookup"><span data-stu-id="99644-124">When an analyst or data scientist is working with hello data contained in hello larger directory structures, hello data in these reference files can be used tooprovide more detailed information for entities that are referred tooonly by name or ID in hello larger data set.</span></span>

<span data-ttu-id="99644-125">V tomto vzoru, má smysl tooregister hello individuální referenční datové soubory s **Azure Data Catalog**.</span><span class="sxs-lookup"><span data-stu-id="99644-125">In this pattern, it makes sense tooregister hello individual reference data files with **Azure Data Catalog**.</span></span> <span data-ttu-id="99644-126">Každý soubor představuje datové sady a každé z nich můžou opatřeny poznámkami a zjištěné jednotlivě.</span><span class="sxs-lookup"><span data-stu-id="99644-126">Each file represents a data set, and each one can be annotated and discovered individually.</span></span>

## <a name="alternate-patterns"></a><span data-ttu-id="99644-127">Alternativní vzory</span><span class="sxs-lookup"><span data-stu-id="99644-127">Alternate patterns</span></span>
<span data-ttu-id="99644-128">Hello vzory popsané v předcházející části hello jsou právě dva možné úložiště velkých objemů dat může být uspořádaný, ale každý implementace se liší.</span><span class="sxs-lookup"><span data-stu-id="99644-128">hello patterns described in hello preceding section are just two possible ways a big data store may be organized, but each implementation is different.</span></span> <span data-ttu-id="99644-129">Bez ohledu na to, jak jsou strukturovaná zdroje dat, při registraci zdroje velkých objemů dat s **Azure Data Catalog**, soustředit na registrace hello soubory a adresáře, které představují hello datových sad, které jsou tooothers hodnotu v rámci vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="99644-129">Regardless of how your data sources are structured, when registering big data sources with **Azure Data Catalog**, focus on registering hello files and directories that represent hello data sets that are of value tooothers within your organization.</span></span> <span data-ttu-id="99644-130">Registrace všechny soubory a adresáře může zaplnit katalogu hello, takže je pro uživatele toofind těžší požadované.</span><span class="sxs-lookup"><span data-stu-id="99644-130">Registering all files and directories can clutter hello catalog, making it harder for users toofind what they need.</span></span>

## <a name="summary"></a><span data-ttu-id="99644-131">Souhrn</span><span class="sxs-lookup"><span data-stu-id="99644-131">Summary</span></span>
<span data-ttu-id="99644-132">Registrace zdroje dat s **Azure Data Catalog** umožňuje jejich jednodušší toodiscover a pochopit.</span><span class="sxs-lookup"><span data-stu-id="99644-132">Registering data sources with **Azure Data Catalog** makes them easier toodiscover and understand.</span></span> <span data-ttu-id="99644-133">Registrace a zadávání poznámek k hello velkých objemů dat souborů a adresářů, které představují logické datové sady, může pomoci uživatelům najít a používat zdroje hello velkých objemů dat, které potřebují.</span><span class="sxs-lookup"><span data-stu-id="99644-133">By registering and annotating hello big data files and directories that represent logical data sets, you can help users find and use hello big data sources they need.</span></span>
