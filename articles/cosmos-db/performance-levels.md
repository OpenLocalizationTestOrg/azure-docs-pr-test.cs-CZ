---
title: "Úrovně výkonu DocumentDB API | Microsoft Docs"
description: "Informace o tom, jak úrovně výkonu DocumentDB API umožňují rezervovat propustnosti na kontejneru na základě."
services: cosmos-db
author: mimig1
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: 7dc21c71-47e2-4e06-aa21-e84af52866f4
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/24/2017
ms.author: mimig
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c8d4733e57eb760dbb8e8ca96f6ba55671d1742f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="retiring-the-s1-s2-and-s3-performance-levels"></a><span data-ttu-id="5eb5a-103">Vyřazení úrovní výkonu S1, S2 a S3</span><span class="sxs-lookup"><span data-stu-id="5eb5a-103">Retiring the S1, S2, and S3 performance levels</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="5eb5a-104">Úrovně výkonu S1, S2 a S3 popsané v tomto článku se postupně vyřazuje z provozu a nadále již nebudou k dispozici pro nové účty DocumentDB rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="5eb5a-104">The S1, S2, and S3 performance levels discussed in this article are being retired and are no longer available for new DocumentDB API accounts.</span></span>
>

<span data-ttu-id="5eb5a-105">Tento článek obsahuje přehled úrovní výkonu S1, S2 a S3 a popisuje, jak kolekcí, které používají tyto úrovně výkonu se budou migrovat do kolekce tvořené jedním oddílem na 1. srpna 2017.</span><span class="sxs-lookup"><span data-stu-id="5eb5a-105">This article provides an overview of S1, S2, and S3 performance levels, and discusses how the collections that use these performance levels will be migrated to single partition collections on August 1st, 2017.</span></span> <span data-ttu-id="5eb5a-106">Po přečtení tohoto článku, budete moct odpovězte si na následující otázky:</span><span class="sxs-lookup"><span data-stu-id="5eb5a-106">After reading this article, you'll be able to answer the following questions:</span></span>

- [<span data-ttu-id="5eb5a-107">Proč se výkonu S1, S2 a S3 úrovně postupně vyřazuje z provozu?</span><span class="sxs-lookup"><span data-stu-id="5eb5a-107">Why are the S1, S2, and S3 performance levels being retired?</span></span>](#why-retired)
- [<span data-ttu-id="5eb5a-108">Jak kolekce tvořené jedním oddílem a dělené kolekce srovnání na S1, S2, úrovně výkonu S3?</span><span class="sxs-lookup"><span data-stu-id="5eb5a-108">How do single partition collections and partitioned collections compare to the S1, S2, S3 performance levels?</span></span>](#compare)
- [<span data-ttu-id="5eb5a-109">Co je třeba provést k zajištění nepřetržitého přístupu k mým datům?</span><span class="sxs-lookup"><span data-stu-id="5eb5a-109">What do I need to do to ensure uninterrupted access to my data?</span></span>](#uninterrupted-access)
- [<span data-ttu-id="5eb5a-110">Jak se po dokončení migrace změní mé kolekce?</span><span class="sxs-lookup"><span data-stu-id="5eb5a-110">How will my collection change after the migration?</span></span>](#collection-change)
- [<span data-ttu-id="5eb5a-111">Jak bude Moje fakturace změnit po I se migrovat do kolekce tvořené jedním oddílem?</span><span class="sxs-lookup"><span data-stu-id="5eb5a-111">How will my billing change after I’m migrated to single partition collections?</span></span>](#billing-change)
- [<span data-ttu-id="5eb5a-112">Co když je potřeba víc než 10 GB úložiště?</span><span class="sxs-lookup"><span data-stu-id="5eb5a-112">What if I need more than 10 GB of storage?</span></span>](#more-storage-needed)
- [<span data-ttu-id="5eb5a-113">Můžete změnit mezi S1, S2 a S3 úrovně výkonu před 1. srpna 2017?</span><span class="sxs-lookup"><span data-stu-id="5eb5a-113">Can I change between the S1, S2, and S3 performance levels before August 1, 2017?</span></span>](#change-before)
- [<span data-ttu-id="5eb5a-114">Jak poznám, že při migraci mé kolekce?</span><span class="sxs-lookup"><span data-stu-id="5eb5a-114">How will I know when my collection has migrated?</span></span>](#when-migrated)
- [<span data-ttu-id="5eb5a-115">Jak provedu migraci z S1, S2, S3 úrovněmi výkonu kolekce tvořené jedním oddílem na vlastní?</span><span class="sxs-lookup"><span data-stu-id="5eb5a-115">How do I migrate from the S1, S2, S3 performance levels to single partition collections on my own?</span></span>](#migrate-diy)
- [<span data-ttu-id="5eb5a-116">Jak se ovlivněny vám EA zákazníka?</span><span class="sxs-lookup"><span data-stu-id="5eb5a-116">How am I impacted if I'm an EA customer?</span></span>](#ea-customer)

<a name="why-retired"></a>

## <a name="why-are-the-s1-s2-and-s3-performance-levels-being-retired"></a><span data-ttu-id="5eb5a-117">Proč se výkonu S1, S2 a S3 úrovně postupně vyřazuje z provozu?</span><span class="sxs-lookup"><span data-stu-id="5eb5a-117">Why are the S1, S2, and S3 performance levels being retired?</span></span>

<span data-ttu-id="5eb5a-118">Úrovně výkonu S1, S2 a S3 nenabízejí kolekce DocumentDB API nabízí flexibilitu.</span><span class="sxs-lookup"><span data-stu-id="5eb5a-118">The S1, S2, and S3 performance levels do not offer the flexibility that DocumentDB API collections offers.</span></span> <span data-ttu-id="5eb5a-119">Kapacita propustnosti i úložiště s S1, S2, úrovně výkonu S3, byly předem nastavené a nenabízí pružnost.</span><span class="sxs-lookup"><span data-stu-id="5eb5a-119">With the S1, S2, S3 performance levels, both the throughput and storage capacity were pre-set and did not offer elasticity.</span></span> <span data-ttu-id="5eb5a-120">Azure Cosmos DB teď nabízí přizpůsobit propustnost a úložiště nabízí mnohem větší flexibilitu v schopnost škálovat podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="5eb5a-120">Azure Cosmos DB now offers the ability to customize your throughput and storage, offering you much more flexibility in your ability to scale as your needs change.</span></span>

<a name="compare"></a>

## <a name="how-do-single-partition-collections-and-partitioned-collections-compare-to-the-s1-s2-s3-performance-levels"></a><span data-ttu-id="5eb5a-121">Jak kolekce tvořené jedním oddílem a dělené kolekce srovnání na S1, S2, úrovně výkonu S3?</span><span class="sxs-lookup"><span data-stu-id="5eb5a-121">How do single partition collections and partitioned collections compare to the S1, S2, S3 performance levels?</span></span>

<span data-ttu-id="5eb5a-122">Následující tabulka porovnává možnosti propustnost a úložiště, které jsou dostupné v kolekce tvořené jedním oddílem, dělené kolekce a S1, S2, úrovně výkonu S3.</span><span class="sxs-lookup"><span data-stu-id="5eb5a-122">The following table compares the throughput and storage options available in single partition collections, partitioned collections, and S1, S2, S3 performance levels.</span></span> <span data-ttu-id="5eb5a-123">Tady je příklad oblasti USA – východ 2:</span><span class="sxs-lookup"><span data-stu-id="5eb5a-123">Here is an example for US East 2 region:</span></span>

|   |<span data-ttu-id="5eb5a-124">Dělené kolekce</span><span class="sxs-lookup"><span data-stu-id="5eb5a-124">Partitioned collection</span></span>|<span data-ttu-id="5eb5a-125">Kolekce tvořené jedním oddílem</span><span class="sxs-lookup"><span data-stu-id="5eb5a-125">Single partition collection</span></span>|<span data-ttu-id="5eb5a-126">S1</span><span class="sxs-lookup"><span data-stu-id="5eb5a-126">S1</span></span>|<span data-ttu-id="5eb5a-127">S2</span><span class="sxs-lookup"><span data-stu-id="5eb5a-127">S2</span></span>|<span data-ttu-id="5eb5a-128">S3</span><span class="sxs-lookup"><span data-stu-id="5eb5a-128">S3</span></span>|
|---|---|---|---|---|---|
|<span data-ttu-id="5eb5a-129">Maximální propustnost</span><span class="sxs-lookup"><span data-stu-id="5eb5a-129">Maximum throughput</span></span>|<span data-ttu-id="5eb5a-130">Unlimited</span><span class="sxs-lookup"><span data-stu-id="5eb5a-130">Unlimited</span></span>|<span data-ttu-id="5eb5a-131">10 tis. RU/s</span><span class="sxs-lookup"><span data-stu-id="5eb5a-131">10K RU/s</span></span>|<span data-ttu-id="5eb5a-132">250 RU/s</span><span class="sxs-lookup"><span data-stu-id="5eb5a-132">250 RU/s</span></span>|<span data-ttu-id="5eb5a-133">1 tis. RU/s</span><span class="sxs-lookup"><span data-stu-id="5eb5a-133">1 K RU/s</span></span>|<span data-ttu-id="5eb5a-134">2.5 tis. RU/s</span><span class="sxs-lookup"><span data-stu-id="5eb5a-134">2.5 K RU/s</span></span>|
|<span data-ttu-id="5eb5a-135">Minimální propustnost</span><span class="sxs-lookup"><span data-stu-id="5eb5a-135">Minimum throughput</span></span>|<span data-ttu-id="5eb5a-136">2.5 tis. RU/s</span><span class="sxs-lookup"><span data-stu-id="5eb5a-136">2.5K RU/s</span></span>|<span data-ttu-id="5eb5a-137">400 RU/s</span><span class="sxs-lookup"><span data-stu-id="5eb5a-137">400 RU/s</span></span>|<span data-ttu-id="5eb5a-138">250 RU/s</span><span class="sxs-lookup"><span data-stu-id="5eb5a-138">250 RU/s</span></span>|<span data-ttu-id="5eb5a-139">1 tis. RU/s</span><span class="sxs-lookup"><span data-stu-id="5eb5a-139">1 K RU/s</span></span>|<span data-ttu-id="5eb5a-140">2.5 tis. RU/s</span><span class="sxs-lookup"><span data-stu-id="5eb5a-140">2.5 K RU/s</span></span>|
|<span data-ttu-id="5eb5a-141">Maximální velikost úložiště</span><span class="sxs-lookup"><span data-stu-id="5eb5a-141">Maximum storage</span></span>|<span data-ttu-id="5eb5a-142">Unlimited</span><span class="sxs-lookup"><span data-stu-id="5eb5a-142">Unlimited</span></span>|<span data-ttu-id="5eb5a-143">10 GB</span><span class="sxs-lookup"><span data-stu-id="5eb5a-143">10 GB</span></span>|<span data-ttu-id="5eb5a-144">10 GB</span><span class="sxs-lookup"><span data-stu-id="5eb5a-144">10 GB</span></span>|<span data-ttu-id="5eb5a-145">10 GB</span><span class="sxs-lookup"><span data-stu-id="5eb5a-145">10 GB</span></span>|<span data-ttu-id="5eb5a-146">10 GB</span><span class="sxs-lookup"><span data-stu-id="5eb5a-146">10 GB</span></span>|
|<span data-ttu-id="5eb5a-147">Cena (každý měsíc)</span><span class="sxs-lookup"><span data-stu-id="5eb5a-147">Price (monthly)</span></span>|<span data-ttu-id="5eb5a-148">Propustnost: $6 / 100 RU/s</span><span class="sxs-lookup"><span data-stu-id="5eb5a-148">Throughput: $6 / 100 RU/s</span></span><br><br><span data-ttu-id="5eb5a-149">Úložiště: $ 0,25/GB</span><span class="sxs-lookup"><span data-stu-id="5eb5a-149">Storage: $0.25/GB</span></span>|<span data-ttu-id="5eb5a-150">Propustnost: $6 / 100 RU/s</span><span class="sxs-lookup"><span data-stu-id="5eb5a-150">Throughput: $6 / 100 RU/s</span></span><br><br><span data-ttu-id="5eb5a-151">Úložiště: $ 0,25/GB</span><span class="sxs-lookup"><span data-stu-id="5eb5a-151">Storage: $0.25/GB</span></span>|<span data-ttu-id="5eb5a-152">25 USD</span><span class="sxs-lookup"><span data-stu-id="5eb5a-152">$25 USD</span></span>|<span data-ttu-id="5eb5a-153">50 USD</span><span class="sxs-lookup"><span data-stu-id="5eb5a-153">$50 USD</span></span>|<span data-ttu-id="5eb5a-154">100 USD</span><span class="sxs-lookup"><span data-stu-id="5eb5a-154">$100 USD</span></span>|

<span data-ttu-id="5eb5a-155">Jste si již EA zákazníka?</span><span class="sxs-lookup"><span data-stu-id="5eb5a-155">Are you an EA customer?</span></span> <span data-ttu-id="5eb5a-156">Pokud ano, najdete v části [jak mě I ovlivněná jsem EA zákazníka?](#ea-customer)</span><span class="sxs-lookup"><span data-stu-id="5eb5a-156">If so, see [How am I impacted if I'm an EA customer?](#ea-customer)</span></span>

<a name="uninterrupted-access"></a>

## <a name="what-do-i-need-to-do-to-ensure-uninterrupted-access-to-my-data"></a><span data-ttu-id="5eb5a-157">Co je třeba provést k zajištění nepřetržitého přístupu k mým datům?</span><span class="sxs-lookup"><span data-stu-id="5eb5a-157">What do I need to do to ensure uninterrupted access to my data?</span></span>

<span data-ttu-id="5eb5a-158">Nothing, Cosmos DB zpracovává migrace za vás.</span><span class="sxs-lookup"><span data-stu-id="5eb5a-158">Nothing, Cosmos DB handles the migration for you.</span></span> <span data-ttu-id="5eb5a-159">Pokud máte kolekci S1, S2 nebo S3, aktuální kolekci budou migrovat do kolekce jednoho oddílu na 31 července 2017.</span><span class="sxs-lookup"><span data-stu-id="5eb5a-159">If you have an S1, S2, or S3 collection, your current collection will be migrated to a single partition collection on July 31, 2017.</span></span> 

<a name="collection-change"></a>

## <a name="how-will-my-collection-change-after-the-migration"></a><span data-ttu-id="5eb5a-160">Jak se po dokončení migrace změní mé kolekce?</span><span class="sxs-lookup"><span data-stu-id="5eb5a-160">How will my collection change after the migration?</span></span>

<span data-ttu-id="5eb5a-161">Pokud máte kolekci S1, budou přeneseny do kolekce tvořené jedním oddílem s propustností 400 RU/s.</span><span class="sxs-lookup"><span data-stu-id="5eb5a-161">If you have an S1 collection, you will be migrated to a single partition collection with 400 RU/s throughput.</span></span> <span data-ttu-id="5eb5a-162">400 RU/s je k dispozici kolekce tvořené jedním oddílem nejnižší propustnost.</span><span class="sxs-lookup"><span data-stu-id="5eb5a-162">400 RU/s is the lowest throughput available with single partition collections.</span></span> <span data-ttu-id="5eb5a-163">Ale náklady pro 400 RU/s kolekce tvořené jedním oddílem je přibližně stejné jako byly platícího s S1 kolekce a 250. RU/s – tak nejsou platícího pro velmi 150 RU/s dostupné.</span><span class="sxs-lookup"><span data-stu-id="5eb5a-163">However, the cost for 400 RU/s in the a single partition collection is approximately the same as you were paying with your S1 collection and 250 RU/s – so you are not paying for the extra 150 RU/s available to you.</span></span>

<span data-ttu-id="5eb5a-164">Pokud máte kolekci S2, budou přeneseny do kolekce tvořené jedním oddílem s 1 tis. RU/s.</span><span class="sxs-lookup"><span data-stu-id="5eb5a-164">If you have an S2 collection, you will be migrated to a single partition collection with 1 K RU/s.</span></span> <span data-ttu-id="5eb5a-165">Zobrazí se žádná změna na vaší propustnost úroveň.</span><span class="sxs-lookup"><span data-stu-id="5eb5a-165">You will see no change to your throughput level.</span></span>

<span data-ttu-id="5eb5a-166">Pokud máte kolekci S3, budou přeneseny do kolekce tvořené jedním oddílem s 2,5 tis. RU/s.</span><span class="sxs-lookup"><span data-stu-id="5eb5a-166">If you have an S3 collection, you will be migrated to a single partition collection with 2.5 K RU/s.</span></span> <span data-ttu-id="5eb5a-167">Zobrazí se žádná změna na vaší propustnost úroveň.</span><span class="sxs-lookup"><span data-stu-id="5eb5a-167">You will see no change to your throughput level.</span></span>

<span data-ttu-id="5eb5a-168">V každém z těchto případech se po migraci kolekce, bude možné přizpůsobit odpovídající úrovni vašeho propustnost, nebo ji škálovat nahoru a dolů tak, aby uživatelům poskytnout přístup s nízkou latencí.</span><span class="sxs-lookup"><span data-stu-id="5eb5a-168">In each of these cases, after your collection is migrated, you will be able to customize your throughput level, or scale it up and down as needed to provide low-latency access to your users.</span></span> <span data-ttu-id="5eb5a-169">Změna úrovně propustnosti po migraci kolekce, jednoduše na portálu Azure otevřete účet Cosmos DB, klikněte na možnost škálování, zvolte kolekce a upravte úroveň propustnosti, jak je znázorněno na následujícím snímku obrazovky:</span><span class="sxs-lookup"><span data-stu-id="5eb5a-169">To change the throughput level after your collection has migrated, simply open your Cosmos DB account in the Azure portal, click Scale, choose your collection, and then adjust the throughput level, as shown in the following screenshot:</span></span>

![Postup škálování propustnosti na portálu Azure](./media/performance-levels/portal-scale-throughput.png)

<a name="billing-change"></a>

## <a name="how-will-my-billing-change-after-im-migrated-to-the-single-partition-collections"></a><span data-ttu-id="5eb5a-171">Jak bude Moje fakturace změnit po I se migrovat do kolekce tvořené jedním oddílem?</span><span class="sxs-lookup"><span data-stu-id="5eb5a-171">How will my billing change after I’m migrated to the single partition collections?</span></span>

<span data-ttu-id="5eb5a-172">Za předpokladu, že budete mít 10 S1 kolekcí, 1 GB úložiště pro každou, v oblasti USA – východ a migrace těchto 10 S1 kolekcí do 10 kolekce tvořené jedním oddílem v 400 RU za sekundu (minimální úroveň).</span><span class="sxs-lookup"><span data-stu-id="5eb5a-172">Assuming you have 10 S1 collections, 1 GB of storage for each, in the US East region, and you migrate these 10 S1 collections to 10 single partition collections at 400 RU/sec (the minimum level).</span></span> <span data-ttu-id="5eb5a-173">Pokud necháte 10 kolekce tvořené jedním oddílem pro úplný měsíc, bude vaše faktura vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="5eb5a-173">Your bill will look as follows if you keep the 10 single partition collections for a full month:</span></span>

![Jak S1 ceny pro 10 kolekce porovnává 10 kolekce pomocí ceny pro kolekce tvořené jedním oddílem](./media/performance-levels/s1-vs-standard-pricing.png)

<a name="more-storage-needed"></a>

## <a name="what-if-i-need-more-than-10-gb-of-storage"></a><span data-ttu-id="5eb5a-175">Co když je potřeba víc než 10 GB úložiště?</span><span class="sxs-lookup"><span data-stu-id="5eb5a-175">What if I need more than 10 GB of storage?</span></span>

<span data-ttu-id="5eb5a-176">Jestli máte kolekci s úrovní výkonu S1, S2 nebo S3, nebo mít jeden oddíl kolekce, které mají všechny 10 GB úložiště, které jsou k dispozici, můžete migrovat data do oddílů kolekce s prakticky neomezené úložiště nástroj pro migraci dat DB Cosmos.</span><span class="sxs-lookup"><span data-stu-id="5eb5a-176">Whether you have a collection with an S1, S2, or S3 performance level, or have a single partition collection, all of which have 10 GB of storage available, you can use the Cosmos DB Data Migration tool to migrate your data to a partitioned collection with virtually unlimited storage.</span></span> <span data-ttu-id="5eb5a-177">Informace o výhodách dělenou kolekci najdete v tématu [dělení a škálování v Azure Cosmos DB](documentdb-partition-data.md).</span><span class="sxs-lookup"><span data-stu-id="5eb5a-177">For information about the benefits of a partitioned collection, see [Partitioning and scaling in Azure Cosmos DB](documentdb-partition-data.md).</span></span> <span data-ttu-id="5eb5a-178">Informace o tom, jak migrovat S1, S2, S3 nebo kolekce jednoho oddílu do dělené kolekce najdete v tématu [migrace z jednoho oddílu do dělené kolekce](documentdb-partition-data.md#migrating-from-single-partition).</span><span class="sxs-lookup"><span data-stu-id="5eb5a-178">For information about how to migrate your S1, S2, S3, or single partition collection to a partitioned collection, see [Migrating from single-partition to partitioned collections](documentdb-partition-data.md#migrating-from-single-partition).</span></span> 

<a name="change-before"></a>

## <a name="can-i-change-between-the-s1-s2-and-s3-performance-levels-before-august-1-2017"></a><span data-ttu-id="5eb5a-179">Můžete změnit mezi S1, S2 a S3 úrovně výkonu před 1. srpna 2017?</span><span class="sxs-lookup"><span data-stu-id="5eb5a-179">Can I change between the S1, S2, and S3 performance levels before August 1, 2017?</span></span>

<span data-ttu-id="5eb5a-180">Pouze existující účty s výkonu S1, S2 a S3 bude moct změnit a změnit úroveň úrovně výkonu prostřednictvím portálu nebo prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="5eb5a-180">Only existing accounts with S1, S2, and S3 performance will be able to change and alter performance level tiers through the portal or programmatically.</span></span> <span data-ttu-id="5eb5a-181">2017 1 srpen úrovní výkonu S1, S2 a S3 bude k dispozici.</span><span class="sxs-lookup"><span data-stu-id="5eb5a-181">By August 1, 2017, the S1, S2, and S3 performance levels will no longer be available.</span></span> <span data-ttu-id="5eb5a-182">Pokud změníte z S1, S3 nebo S3 do kolekce tvořené jedním oddílem, nelze vrátit k úrovní výkonu S1, S2 nebo S3.</span><span class="sxs-lookup"><span data-stu-id="5eb5a-182">If you change from S1, S3, or S3 to a single partition collection, you cannot return to the S1, S2, or S3 performance levels.</span></span>

<a name="when-migrated"></a>

## <a name="how-will-i-know-when-my-collection-has-migrated"></a><span data-ttu-id="5eb5a-183">Jak poznám, že při migraci mé kolekce?</span><span class="sxs-lookup"><span data-stu-id="5eb5a-183">How will I know when my collection has migrated?</span></span>

<span data-ttu-id="5eb5a-184">Na 31 července 2017 dojde k migraci.</span><span class="sxs-lookup"><span data-stu-id="5eb5a-184">The migration will occur on July 31, 2017.</span></span> <span data-ttu-id="5eb5a-185">Pokud máte kolekci, která použije S1, S2 nebo S3 úrovně výkonu, Cosmos DB tým vás bude kontaktovat e-mailem před provedením migrace.</span><span class="sxs-lookup"><span data-stu-id="5eb5a-185">If you have a collection that uses the S1, S2 or S3 performance levels, the Cosmos DB team will contact you by email before the migration takes place.</span></span> <span data-ttu-id="5eb5a-186">Po migraci dokončení na 1. srpna 2017 portálu Azure vám ukáže, že vaše kolekce používá cenový model Standard.</span><span class="sxs-lookup"><span data-stu-id="5eb5a-186">Once the migration is complete, on August 1, 2017, the Azure portal will show that your collection uses Standard pricing.</span></span>

![Způsob ověření kolekce byla migrována do cenová úroveň Standard](./media/performance-levels/portal-standard-pricing-applied.png)

<a name="migrate-diy"></a>

## <a name="how-do-i-migrate-from-the-s1-s2-s3-performance-levels-to-single-partition-collections-on-my-own"></a><span data-ttu-id="5eb5a-188">Jak provedu migraci z S1, S2, S3 úrovněmi výkonu kolekce tvořené jedním oddílem na vlastní?</span><span class="sxs-lookup"><span data-stu-id="5eb5a-188">How do I migrate from the S1, S2, S3 performance levels to single partition collections on my own?</span></span>

<span data-ttu-id="5eb5a-189">Můžete migrovat z úrovní výkonu S1, S2 a S3 kolekce tvořené jedním oddílem pomocí portálu Azure nebo prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="5eb5a-189">You can migrate from the S1, S2, and S3 performance levels to single partition collections using the Azure portal or programmatically.</span></span> <span data-ttu-id="5eb5a-190">To provedete sami před srpen 1, abyste mohli využívat výhod propustnost flexibilní možnosti s kolekce tvořené jedním oddílem, nebo jsme kolekce můžete migrovat na 31 července 2017.</span><span class="sxs-lookup"><span data-stu-id="5eb5a-190">You can do this on your own before August 1 to benefit from the flexible throughput options available with single partition collections, or we will migrate your collections for you on July 31, 2017.</span></span>

<span data-ttu-id="5eb5a-191">**Migrace do kolekce tvořené jedním oddílem pomocí portálu Azure**</span><span class="sxs-lookup"><span data-stu-id="5eb5a-191">**To migrate to single partition collections using the Azure portal**</span></span>

1. <span data-ttu-id="5eb5a-192">V [ **portál Azure**](https://portal.azure.com), klikněte na tlačítko **Azure Cosmos DB**, pak vyberte účet Cosmos DB, který chcete upravit.</span><span class="sxs-lookup"><span data-stu-id="5eb5a-192">In the [**Azure portal**](https://portal.azure.com), click **Azure Cosmos DB**, then select the Cosmos DB account to modify.</span></span> 
 
    <span data-ttu-id="5eb5a-193">Pokud **Azure Cosmos DB** není na panelu vlevo klikněte na tlačítko >, přejděte k položce **databáze**, vyberte **Azure Cosmos DB**a potom vyberte účet DocumentDB.</span><span class="sxs-lookup"><span data-stu-id="5eb5a-193">If **Azure Cosmos DB** is not on the Jumpbar, click >, scroll to **Databases**, select **Azure Cosmos DB**, and then select the DocumentDB account.</span></span>  

2. <span data-ttu-id="5eb5a-194">V nabídce prostředků v části **kontejnery**, klikněte na tlačítko **škálování**, vyberte kolekci, které chcete upravit v rozevíracím seznamu a pak klikněte na tlačítko **cenová úroveň**.</span><span class="sxs-lookup"><span data-stu-id="5eb5a-194">On the resource menu, under **Containers**, click **Scale**, select the collection to modify from the drop-down list, and then click **Pricing Tier**.</span></span> <span data-ttu-id="5eb5a-195">Účty pomocí předem definovaných propustnost mít cenovou úroveň S1, S2 nebo S3.</span><span class="sxs-lookup"><span data-stu-id="5eb5a-195">Accounts using pre-defined throughput have a pricing tier of S1, S2, or S3.</span></span>  <span data-ttu-id="5eb5a-196">V **zvolte cenovou úroveň** okně klikněte na tlačítko **standardní** změnit na uživatelem definované propustnost, a pak klikněte na **vyberte** uložte změny.</span><span class="sxs-lookup"><span data-stu-id="5eb5a-196">In the **Choose your pricing tier** blade, click **Standard** to change to user-defined throughput, and then click **Select** to save your change.</span></span>

    ![Snímek obrazovky okna nastavení ukazující, kde chcete-li změnit hodnotu propustnosti](./media/performance-levels/change-performance-set-thoughput.png)

3. <span data-ttu-id="5eb5a-198">Zpět v **škálování** okně **cenová úroveň** se změní na **standardní** a **propustnost (RU/s)** pole se zobrazí se výchozí hodnota je 400.</span><span class="sxs-lookup"><span data-stu-id="5eb5a-198">Back in the **Scale** blade, the **Pricing Tier** is changed to **Standard** and the **Throughput (RU/s)** box is displayed with a default value of 400.</span></span> <span data-ttu-id="5eb5a-199">Nastavit propustnost mezi 400 a 10 000 [požadované jednotky](request-units.md)/second (RU/s).</span><span class="sxs-lookup"><span data-stu-id="5eb5a-199">Set the throughput between 400 and 10,000 [Request units](request-units.md)/second (RU/s).</span></span> <span data-ttu-id="5eb5a-200">**Odhadované měsíčních nákladů** v dolní části stránky aktualizace automaticky zajistit odhad měsíční náklady.</span><span class="sxs-lookup"><span data-stu-id="5eb5a-200">The **Estimated Monthly Bill** at the bottom of the page updates automatically to provide an estimate of the monthly cost.</span></span> 

    >[!IMPORTANT] 
    > <span data-ttu-id="5eb5a-201">Po uložení změn a přesunout do cenová úroveň Standard, není možné vrátit zpět na úrovní výkonu S1, S2 nebo S3.</span><span class="sxs-lookup"><span data-stu-id="5eb5a-201">Once you save your changes and move to the Standard pricing tier, you cannot roll back to the S1, S2, or S3 performance levels.</span></span>

4. <span data-ttu-id="5eb5a-202">Klikněte na tlačítko **Uložit** uložte provedené změny.</span><span class="sxs-lookup"><span data-stu-id="5eb5a-202">Click **Save** to save your changes.</span></span>

    <span data-ttu-id="5eb5a-203">Pokud zjistíte, že potřebujete další propustnost (větší než 10 000 RU/s) nebo další úložiště (větší než 10GB) můžete vytvořit kolekci oddílů.</span><span class="sxs-lookup"><span data-stu-id="5eb5a-203">If you determine that you need more throughput (greater than 10,000 RU/s) or more storage (greater than 10GB) you can create a partitioned collection.</span></span> <span data-ttu-id="5eb5a-204">K migraci kolekce tvořené jedním oddílem pro dělenou kolekci, najdete v části [migrace z jednoho oddílu do dělené kolekce](documentdb-partition-data.md#migrating-from-single-partition).</span><span class="sxs-lookup"><span data-stu-id="5eb5a-204">To migrate a single partition collection to a partitioned collection, see [Migrating from single-partition to partitioned collections](documentdb-partition-data.md#migrating-from-single-partition).</span></span>

    > [!NOTE]
    > <span data-ttu-id="5eb5a-205">Změna z S1, S2 nebo S3 standardního může trvat až 2 minut.</span><span class="sxs-lookup"><span data-stu-id="5eb5a-205">Changing from S1, S2, or S3 to Standard may take up to 2 minutes.</span></span>
    > 
    > 

<span data-ttu-id="5eb5a-206">**Migrace do kolekce tvořené jedním oddílem pomocí sady .NET SDK**</span><span class="sxs-lookup"><span data-stu-id="5eb5a-206">**To migrate to single partition collections using the .NET SDK**</span></span>

<span data-ttu-id="5eb5a-207">Další možností pro změnu úrovně výkonu vaší kolekce je pomocí naší sady SDK.</span><span class="sxs-lookup"><span data-stu-id="5eb5a-207">Another option for changing your collections' performance levels is through our SDKs.</span></span> <span data-ttu-id="5eb5a-208">Tato část se vztahuje pouze změna shromažďování výkonu úrovně pomocí našich [DocumentDB .NET API](documentdb-sdk-dotnet.md), ale proces je podobný pro naše dalších sadách SDK.</span><span class="sxs-lookup"><span data-stu-id="5eb5a-208">This section only covers changing a collection's performance level using our [DocumentDB .NET API](documentdb-sdk-dotnet.md), but the process is similar for our other SDKs.</span></span>

<span data-ttu-id="5eb5a-209">Zde je fragment kódu pro změnu propustnost kolekce do 5 000 jednotek žádosti za sekundu:</span><span class="sxs-lookup"><span data-stu-id="5eb5a-209">Here is a code snippet for changing the collection throughput to 5,000 request units per second:</span></span>
    
```C#
    //Fetch the resource to be updated
    Offer offer = client.CreateOfferQuery()
                      .Where(r => r.ResourceLink == collection.SelfLink)    
                      .AsEnumerable()
                      .SingleOrDefault();

    // Set the throughput to 5000 request units per second
    offer = new OfferV2(offer, 5000);

    //Now persist these changes to the database by replacing the original resource
    await client.ReplaceOfferAsync(offer);
```

<span data-ttu-id="5eb5a-210">Navštivte [MSDN](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.aspx) zobrazení další příklady a další informace o našich nabídka metody:</span><span class="sxs-lookup"><span data-stu-id="5eb5a-210">Visit [MSDN](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.aspx) to view additional examples and learn more about our offer methods:</span></span>

* [<span data-ttu-id="5eb5a-211">**ReadOfferAsync**</span><span class="sxs-lookup"><span data-stu-id="5eb5a-211">**ReadOfferAsync**</span></span>](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.readofferasync.aspx)
* [<span data-ttu-id="5eb5a-212">**ReadOffersFeedAsync**</span><span class="sxs-lookup"><span data-stu-id="5eb5a-212">**ReadOffersFeedAsync**</span></span>](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.readoffersfeedasync.aspx)
* [<span data-ttu-id="5eb5a-213">**ReplaceOfferAsync**</span><span class="sxs-lookup"><span data-stu-id="5eb5a-213">**ReplaceOfferAsync**</span></span>](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.replaceofferasync.aspx)
* [<span data-ttu-id="5eb5a-214">**CreateOfferQuery**</span><span class="sxs-lookup"><span data-stu-id="5eb5a-214">**CreateOfferQuery**</span></span>](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.documentqueryable.createofferquery.aspx)

<a name="ea-customer"></a>

## <a name="how-am-i-impacted-if-im-an-ea-customer"></a><span data-ttu-id="5eb5a-215">Jak se ovlivněny vám EA zákazníka?</span><span class="sxs-lookup"><span data-stu-id="5eb5a-215">How am I impacted if I'm an EA customer?</span></span>

<span data-ttu-id="5eb5a-216">Zákazníci EA bude cena chránit až do konce jejich aktuální kontrakt.</span><span class="sxs-lookup"><span data-stu-id="5eb5a-216">EA customers will be price protected until the end of their current contract.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5eb5a-217">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5eb5a-217">Next steps</span></span>
<span data-ttu-id="5eb5a-218">Další informace o cenách a správě dat pomocí Azure Cosmos DB najdete v těchto zdrojích:</span><span class="sxs-lookup"><span data-stu-id="5eb5a-218">To learn more about pricing and managing data with Azure Cosmos DB, explore these resources:</span></span>

1.  <span data-ttu-id="5eb5a-219">[Segmentace dat v databázi Cosmos](documentdb-partition-data.md).</span><span class="sxs-lookup"><span data-stu-id="5eb5a-219">[Partitioning data in Cosmos DB](documentdb-partition-data.md).</span></span> <span data-ttu-id="5eb5a-220">Vysvětlení rozdílu kontejneru tvořené jedním oddílem a oddílů kontejnery, jakož i tipy k implementaci strategie dělení bezproblémově škálování.</span><span class="sxs-lookup"><span data-stu-id="5eb5a-220">Understand the difference between single partition container and partitioned containers, as well as tips on implementing a partitioning strategy to scale seamlessly.</span></span>
2.  <span data-ttu-id="5eb5a-221">[Ceny cosmos DB](https://azure.microsoft.com/pricing/details/cosmos-db/).</span><span class="sxs-lookup"><span data-stu-id="5eb5a-221">[Cosmos DB pricing](https://azure.microsoft.com/pricing/details/cosmos-db/).</span></span> <span data-ttu-id="5eb5a-222">Další informace o náklady na zřizování propustnost a využívání úložiště.</span><span class="sxs-lookup"><span data-stu-id="5eb5a-222">Learn about the cost of provisioning throughput and consuming storage.</span></span>
3.  <span data-ttu-id="5eb5a-223">[Požadované jednotky](request-units.md).</span><span class="sxs-lookup"><span data-stu-id="5eb5a-223">[Request units](request-units.md).</span></span> <span data-ttu-id="5eb5a-224">Pochopení spotřeby propustnosti na typy jiné operace, například pro čtení, zápisu, dotazů.</span><span class="sxs-lookup"><span data-stu-id="5eb5a-224">Understand the consumption of throughput for different operation types, for example Read, Write, Query.</span></span>
