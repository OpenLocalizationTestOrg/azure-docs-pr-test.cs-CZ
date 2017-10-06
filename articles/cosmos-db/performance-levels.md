---
title: "úrovně výkonu aaaDocumentDB rozhraní API | Microsoft Docs"
description: "Informace o tom, jak úrovně výkonu DocumentDB API umožňují tooreserve propustnosti na kontejneru na základě."
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
ms.openlocfilehash: 716bc11ae238dbb0feebf004ed8d5f8a7515ec6f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="retiring-hello-s1-s2-and-s3-performance-levels"></a><span data-ttu-id="3e14e-103">Vyřazení úrovní výkonu S1, S2 a S3 hello</span><span class="sxs-lookup"><span data-stu-id="3e14e-103">Retiring hello S1, S2, and S3 performance levels</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="3e14e-104">Hello S1, S2 a S3 úrovně výkonu popsané v tomto článku se postupně vyřazuje z provozu a nadále již nebudou k dispozici pro nové účty DocumentDB rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="3e14e-104">hello S1, S2, and S3 performance levels discussed in this article are being retired and are no longer available for new DocumentDB API accounts.</span></span>
>

<span data-ttu-id="3e14e-105">Tento článek obsahuje přehled úrovní výkonu S1, S2 a S3 a popisuje, jak hello kolekcí, které používají tyto úrovně výkonu budou migrované toosingle oddílu kolekce na 1. srpna 2017.</span><span class="sxs-lookup"><span data-stu-id="3e14e-105">This article provides an overview of S1, S2, and S3 performance levels, and discusses how hello collections that use these performance levels will be migrated toosingle partition collections on August 1st, 2017.</span></span> <span data-ttu-id="3e14e-106">Po přečtení tohoto článku, budete moct tooanswer hello následující otázky:</span><span class="sxs-lookup"><span data-stu-id="3e14e-106">After reading this article, you'll be able tooanswer hello following questions:</span></span>

- [<span data-ttu-id="3e14e-107">Proč se výkonu S1, S2 a S3 hello úrovně postupně vyřazuje z provozu?</span><span class="sxs-lookup"><span data-stu-id="3e14e-107">Why are hello S1, S2, and S3 performance levels being retired?</span></span>](#why-retired)
- [<span data-ttu-id="3e14e-108">Jak kolekce tvořené jedním oddílem a dělené kolekce srovnání toohello S1, S2, úrovně výkonu S3?</span><span class="sxs-lookup"><span data-stu-id="3e14e-108">How do single partition collections and partitioned collections compare toohello S1, S2, S3 performance levels?</span></span>](#compare)
- [<span data-ttu-id="3e14e-109">Co mám dělat potřebovat toodo tooensure bez přerušení přístup k datům toomy?</span><span class="sxs-lookup"><span data-stu-id="3e14e-109">What do I need toodo tooensure uninterrupted access toomy data?</span></span>](#uninterrupted-access)
- [<span data-ttu-id="3e14e-110">Jak se po migraci hello změní mé kolekce?</span><span class="sxs-lookup"><span data-stu-id="3e14e-110">How will my collection change after hello migration?</span></span>](#collection-change)
- [<span data-ttu-id="3e14e-111">Jak bude Moje fakturace změnit po jsem migrované toosingle oddílu kolekcí?</span><span class="sxs-lookup"><span data-stu-id="3e14e-111">How will my billing change after I’m migrated toosingle partition collections?</span></span>](#billing-change)
- [<span data-ttu-id="3e14e-112">Co když je potřeba víc než 10 GB úložiště?</span><span class="sxs-lookup"><span data-stu-id="3e14e-112">What if I need more than 10 GB of storage?</span></span>](#more-storage-needed)
- [<span data-ttu-id="3e14e-113">Můžete změnit mezi hello S1, S2 a S3 úrovně výkonu před 1. srpna 2017?</span><span class="sxs-lookup"><span data-stu-id="3e14e-113">Can I change between hello S1, S2, and S3 performance levels before August 1, 2017?</span></span>](#change-before)
- [<span data-ttu-id="3e14e-114">Jak poznám, že při migraci mé kolekce?</span><span class="sxs-lookup"><span data-stu-id="3e14e-114">How will I know when my collection has migrated?</span></span>](#when-migrated)
- [<span data-ttu-id="3e14e-115">Jak migrovat z hello S1, S2, S3 oddílu kolekce toosingle úrovně výkonu na vlastní?</span><span class="sxs-lookup"><span data-stu-id="3e14e-115">How do I migrate from hello S1, S2, S3 performance levels toosingle partition collections on my own?</span></span>](#migrate-diy)
- [<span data-ttu-id="3e14e-116">Jak se ovlivněny vám EA zákazníka?</span><span class="sxs-lookup"><span data-stu-id="3e14e-116">How am I impacted if I'm an EA customer?</span></span>](#ea-customer)

<a name="why-retired"></a>

## <a name="why-are-hello-s1-s2-and-s3-performance-levels-being-retired"></a><span data-ttu-id="3e14e-117">Proč se výkonu S1, S2 a S3 hello úrovně postupně vyřazuje z provozu?</span><span class="sxs-lookup"><span data-stu-id="3e14e-117">Why are hello S1, S2, and S3 performance levels being retired?</span></span>

<span data-ttu-id="3e14e-118">úrovně výkonu S1, S2 a S3 Hello to není flexibilitu hello nabídka, která kolekce DocumentDB API nabízí.</span><span class="sxs-lookup"><span data-stu-id="3e14e-118">hello S1, S2, and S3 performance levels do not offer hello flexibility that DocumentDB API collections offers.</span></span> <span data-ttu-id="3e14e-119">Obě hello propustnost a úložnou kapacitu pomocí hello S1, S2, S3 úrovně výkonu, byly předem nastavené a nenabízí pružnost.</span><span class="sxs-lookup"><span data-stu-id="3e14e-119">With hello S1, S2, S3 performance levels, both hello throughput and storage capacity were pre-set and did not offer elasticity.</span></span> <span data-ttu-id="3e14e-120">Azure Cosmos DB teď nabízí možnost toocustomize hello propustnost a úložiště vám nabízí mnohem větší flexibilitu v vaší tooscale možnost podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="3e14e-120">Azure Cosmos DB now offers hello ability toocustomize your throughput and storage, offering you much more flexibility in your ability tooscale as your needs change.</span></span>

<a name="compare"></a>

## <a name="how-do-single-partition-collections-and-partitioned-collections-compare-toohello-s1-s2-s3-performance-levels"></a><span data-ttu-id="3e14e-121">Jak kolekce tvořené jedním oddílem a dělené kolekce srovnání toohello S1, S2, úrovně výkonu S3?</span><span class="sxs-lookup"><span data-stu-id="3e14e-121">How do single partition collections and partitioned collections compare toohello S1, S2, S3 performance levels?</span></span>

<span data-ttu-id="3e14e-122">Hello následující tabulka porovnává hello propustnost a úložiště možnosti dostupné v kolekce tvořené jedním oddílem, dělené kolekce a S1, S2, úrovně výkonu S3.</span><span class="sxs-lookup"><span data-stu-id="3e14e-122">hello following table compares hello throughput and storage options available in single partition collections, partitioned collections, and S1, S2, S3 performance levels.</span></span> <span data-ttu-id="3e14e-123">Tady je příklad oblasti USA – východ 2:</span><span class="sxs-lookup"><span data-stu-id="3e14e-123">Here is an example for US East 2 region:</span></span>

|   |<span data-ttu-id="3e14e-124">Dělené kolekce</span><span class="sxs-lookup"><span data-stu-id="3e14e-124">Partitioned collection</span></span>|<span data-ttu-id="3e14e-125">Kolekce tvořené jedním oddílem</span><span class="sxs-lookup"><span data-stu-id="3e14e-125">Single partition collection</span></span>|<span data-ttu-id="3e14e-126">S1</span><span class="sxs-lookup"><span data-stu-id="3e14e-126">S1</span></span>|<span data-ttu-id="3e14e-127">S2</span><span class="sxs-lookup"><span data-stu-id="3e14e-127">S2</span></span>|<span data-ttu-id="3e14e-128">S3</span><span class="sxs-lookup"><span data-stu-id="3e14e-128">S3</span></span>|
|---|---|---|---|---|---|
|<span data-ttu-id="3e14e-129">Maximální propustnost</span><span class="sxs-lookup"><span data-stu-id="3e14e-129">Maximum throughput</span></span>|<span data-ttu-id="3e14e-130">Unlimited</span><span class="sxs-lookup"><span data-stu-id="3e14e-130">Unlimited</span></span>|<span data-ttu-id="3e14e-131">10 tis. RU/s</span><span class="sxs-lookup"><span data-stu-id="3e14e-131">10K RU/s</span></span>|<span data-ttu-id="3e14e-132">250 RU/s</span><span class="sxs-lookup"><span data-stu-id="3e14e-132">250 RU/s</span></span>|<span data-ttu-id="3e14e-133">1 tis. RU/s</span><span class="sxs-lookup"><span data-stu-id="3e14e-133">1 K RU/s</span></span>|<span data-ttu-id="3e14e-134">2.5 tis. RU/s</span><span class="sxs-lookup"><span data-stu-id="3e14e-134">2.5 K RU/s</span></span>|
|<span data-ttu-id="3e14e-135">Minimální propustnost</span><span class="sxs-lookup"><span data-stu-id="3e14e-135">Minimum throughput</span></span>|<span data-ttu-id="3e14e-136">2.5 tis. RU/s</span><span class="sxs-lookup"><span data-stu-id="3e14e-136">2.5K RU/s</span></span>|<span data-ttu-id="3e14e-137">400 RU/s</span><span class="sxs-lookup"><span data-stu-id="3e14e-137">400 RU/s</span></span>|<span data-ttu-id="3e14e-138">250 RU/s</span><span class="sxs-lookup"><span data-stu-id="3e14e-138">250 RU/s</span></span>|<span data-ttu-id="3e14e-139">1 tis. RU/s</span><span class="sxs-lookup"><span data-stu-id="3e14e-139">1 K RU/s</span></span>|<span data-ttu-id="3e14e-140">2.5 tis. RU/s</span><span class="sxs-lookup"><span data-stu-id="3e14e-140">2.5 K RU/s</span></span>|
|<span data-ttu-id="3e14e-141">Maximální velikost úložiště</span><span class="sxs-lookup"><span data-stu-id="3e14e-141">Maximum storage</span></span>|<span data-ttu-id="3e14e-142">Unlimited</span><span class="sxs-lookup"><span data-stu-id="3e14e-142">Unlimited</span></span>|<span data-ttu-id="3e14e-143">10 GB</span><span class="sxs-lookup"><span data-stu-id="3e14e-143">10 GB</span></span>|<span data-ttu-id="3e14e-144">10 GB</span><span class="sxs-lookup"><span data-stu-id="3e14e-144">10 GB</span></span>|<span data-ttu-id="3e14e-145">10 GB</span><span class="sxs-lookup"><span data-stu-id="3e14e-145">10 GB</span></span>|<span data-ttu-id="3e14e-146">10 GB</span><span class="sxs-lookup"><span data-stu-id="3e14e-146">10 GB</span></span>|
|<span data-ttu-id="3e14e-147">Cena (každý měsíc)</span><span class="sxs-lookup"><span data-stu-id="3e14e-147">Price (monthly)</span></span>|<span data-ttu-id="3e14e-148">Propustnost: $6 / 100 RU/s</span><span class="sxs-lookup"><span data-stu-id="3e14e-148">Throughput: $6 / 100 RU/s</span></span><br><br><span data-ttu-id="3e14e-149">Úložiště: $ 0,25/GB</span><span class="sxs-lookup"><span data-stu-id="3e14e-149">Storage: $0.25/GB</span></span>|<span data-ttu-id="3e14e-150">Propustnost: $6 / 100 RU/s</span><span class="sxs-lookup"><span data-stu-id="3e14e-150">Throughput: $6 / 100 RU/s</span></span><br><br><span data-ttu-id="3e14e-151">Úložiště: $ 0,25/GB</span><span class="sxs-lookup"><span data-stu-id="3e14e-151">Storage: $0.25/GB</span></span>|<span data-ttu-id="3e14e-152">25 USD</span><span class="sxs-lookup"><span data-stu-id="3e14e-152">$25 USD</span></span>|<span data-ttu-id="3e14e-153">50 USD</span><span class="sxs-lookup"><span data-stu-id="3e14e-153">$50 USD</span></span>|<span data-ttu-id="3e14e-154">100 USD</span><span class="sxs-lookup"><span data-stu-id="3e14e-154">$100 USD</span></span>|

<span data-ttu-id="3e14e-155">Jste si již EA zákazníka?</span><span class="sxs-lookup"><span data-stu-id="3e14e-155">Are you an EA customer?</span></span> <span data-ttu-id="3e14e-156">Pokud ano, najdete v části [jak mě I ovlivněná jsem EA zákazníka?](#ea-customer)</span><span class="sxs-lookup"><span data-stu-id="3e14e-156">If so, see [How am I impacted if I'm an EA customer?](#ea-customer)</span></span>

<a name="uninterrupted-access"></a>

## <a name="what-do-i-need-toodo-tooensure-uninterrupted-access-toomy-data"></a><span data-ttu-id="3e14e-157">Co mám dělat potřebovat toodo tooensure bez přerušení přístup k datům toomy?</span><span class="sxs-lookup"><span data-stu-id="3e14e-157">What do I need toodo tooensure uninterrupted access toomy data?</span></span>

<span data-ttu-id="3e14e-158">Nothing, Cosmos DB zpracovává hello migrace za vás.</span><span class="sxs-lookup"><span data-stu-id="3e14e-158">Nothing, Cosmos DB handles hello migration for you.</span></span> <span data-ttu-id="3e14e-159">Pokud máte kolekci S1, S2 nebo S3, bude vaše aktuální kolekci kolekce tvořené jedním oddílem migrované tooa na 31 července 2017.</span><span class="sxs-lookup"><span data-stu-id="3e14e-159">If you have an S1, S2, or S3 collection, your current collection will be migrated tooa single partition collection on July 31, 2017.</span></span> 

<a name="collection-change"></a>

## <a name="how-will-my-collection-change-after-hello-migration"></a><span data-ttu-id="3e14e-160">Jak se po migraci hello změní mé kolekce?</span><span class="sxs-lookup"><span data-stu-id="3e14e-160">How will my collection change after hello migration?</span></span>

<span data-ttu-id="3e14e-161">Pokud máte kolekci S1, budou migrované tooa kolekce tvořené jedním oddílem s propustností 400 RU/s.</span><span class="sxs-lookup"><span data-stu-id="3e14e-161">If you have an S1 collection, you will be migrated tooa single partition collection with 400 RU/s throughput.</span></span> <span data-ttu-id="3e14e-162">400 RU/s je nejnižší propustnost hello k dispozici kolekce tvořené jedním oddílem.</span><span class="sxs-lookup"><span data-stu-id="3e14e-162">400 RU/s is hello lowest throughput available with single partition collections.</span></span> <span data-ttu-id="3e14e-163">Ale hello náklady v hello kolekce tvořené jedním oddílem je přibližně hello stejně, jako měla platícího s kolekcí vzdálené aplikace S1 400 RU/s a 250 RU/s – tak nejsou platícího pro hello velmi 150 dostupné tooyou RU/s.</span><span class="sxs-lookup"><span data-stu-id="3e14e-163">However, hello cost for 400 RU/s in hello a single partition collection is approximately hello same as you were paying with your S1 collection and 250 RU/s – so you are not paying for hello extra 150 RU/s available tooyou.</span></span>

<span data-ttu-id="3e14e-164">Pokud máte kolekci S2, budou migrované tooa kolekce tvořené jedním oddílem s 1 tis. RU/s.</span><span class="sxs-lookup"><span data-stu-id="3e14e-164">If you have an S2 collection, you will be migrated tooa single partition collection with 1 K RU/s.</span></span> <span data-ttu-id="3e14e-165">Zobrazí se úroveň propustnosti tooyour žádné změny.</span><span class="sxs-lookup"><span data-stu-id="3e14e-165">You will see no change tooyour throughput level.</span></span>

<span data-ttu-id="3e14e-166">Pokud máte kolekci S3, budou migrované tooa kolekce tvořené jedním oddílem s 2,5 tis. RU/s.</span><span class="sxs-lookup"><span data-stu-id="3e14e-166">If you have an S3 collection, you will be migrated tooa single partition collection with 2.5 K RU/s.</span></span> <span data-ttu-id="3e14e-167">Zobrazí se úroveň propustnosti tooyour žádné změny.</span><span class="sxs-lookup"><span data-stu-id="3e14e-167">You will see no change tooyour throughput level.</span></span>

<span data-ttu-id="3e14e-168">Ve všech těchto případech po migraci kolekce je budou moct toocustomize odpovídající úrovni vašeho propustnost nebo ho vertikálně a horizontálně škálovat jako potřebné tooprovide přístup s nízkou latencí tooyour uživatele.</span><span class="sxs-lookup"><span data-stu-id="3e14e-168">In each of these cases, after your collection is migrated, you will be able toocustomize your throughput level, or scale it up and down as needed tooprovide low-latency access tooyour users.</span></span> <span data-ttu-id="3e14e-169">úroveň propustnosti hello toochange po migraci kolekce, jednoduše v hello portálu Azure otevřete účet Cosmos DB, klikněte na možnost škálování, zvolte kolekce a upravte hello úroveň propustnosti, jak ukazuje následující snímek obrazovky hello:</span><span class="sxs-lookup"><span data-stu-id="3e14e-169">toochange hello throughput level after your collection has migrated, simply open your Cosmos DB account in hello Azure portal, click Scale, choose your collection, and then adjust hello throughput level, as shown in hello following screenshot:</span></span>

![Jak hello tooscale propustnost v portálu Azure](./media/performance-levels/portal-scale-throughput.png)

<a name="billing-change"></a>

## <a name="how-will-my-billing-change-after-im-migrated-toohello-single-partition-collections"></a><span data-ttu-id="3e14e-171">Jak bude Moje fakturace změnit po jsem kolekce tvořené jedním oddílem migrované toohello?</span><span class="sxs-lookup"><span data-stu-id="3e14e-171">How will my billing change after I’m migrated toohello single partition collections?</span></span>

<span data-ttu-id="3e14e-172">Za předpokladu, že budete mít 10 S1 kolekcí, 1 GB úložiště pro každou, v oblasti USA – východ hello, a provedete migraci těchto 10 S1 kolekce too10 kolekce tvořené jedním oddílem v 400 RU za sekundu (hello minimální úroveň).</span><span class="sxs-lookup"><span data-stu-id="3e14e-172">Assuming you have 10 S1 collections, 1 GB of storage for each, in hello US East region, and you migrate these 10 S1 collections too10 single partition collections at 400 RU/sec (hello minimum level).</span></span> <span data-ttu-id="3e14e-173">Pokud necháte kolekce hello 10 tvořené jedním oddílem pro úplný měsíc, bude vaše faktura vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="3e14e-173">Your bill will look as follows if you keep hello 10 single partition collections for a full month:</span></span>

![Jak S1 ceny pro 10 kolekce porovná too10 kolekce pomocí ceny pro kolekce tvořené jedním oddílem](./media/performance-levels/s1-vs-standard-pricing.png)

<a name="more-storage-needed"></a>

## <a name="what-if-i-need-more-than-10-gb-of-storage"></a><span data-ttu-id="3e14e-175">Co když je potřeba víc než 10 GB úložiště?</span><span class="sxs-lookup"><span data-stu-id="3e14e-175">What if I need more than 10 GB of storage?</span></span>

<span data-ttu-id="3e14e-176">Jestli máte kolekci s úrovní výkonu S1, S2 nebo S3, nebo mít jeden oddíl kolekce, které mají všechny 10 GB úložiště, které jsou k dispozici, můžete použít toomigrate nástroj pro migraci dat DB Cosmos hello vaše data tooa prakticky oddíly kolekci s neomezené úložiště.</span><span class="sxs-lookup"><span data-stu-id="3e14e-176">Whether you have a collection with an S1, S2, or S3 performance level, or have a single partition collection, all of which have 10 GB of storage available, you can use hello Cosmos DB Data Migration tool toomigrate your data tooa partitioned collection with virtually unlimited storage.</span></span> <span data-ttu-id="3e14e-177">Informace o výhodách hello dělenou kolekci najdete v tématu [dělení a škálování v Azure Cosmos DB](documentdb-partition-data.md).</span><span class="sxs-lookup"><span data-stu-id="3e14e-177">For information about hello benefits of a partitioned collection, see [Partitioning and scaling in Azure Cosmos DB](documentdb-partition-data.md).</span></span> <span data-ttu-id="3e14e-178">Informace o toomigrate vaše S1, S2, S3 nebo kolekce tooa rozdělena na oddíly kolekce tvořené jedním oddílem, viz [migraci z kolekce jedním oddílem toopartitioned](documentdb-partition-data.md#migrating-from-single-partition).</span><span class="sxs-lookup"><span data-stu-id="3e14e-178">For information about how toomigrate your S1, S2, S3, or single partition collection tooa partitioned collection, see [Migrating from single-partition toopartitioned collections](documentdb-partition-data.md#migrating-from-single-partition).</span></span> 

<a name="change-before"></a>

## <a name="can-i-change-between-hello-s1-s2-and-s3-performance-levels-before-august-1-2017"></a><span data-ttu-id="3e14e-179">Můžete změnit mezi hello S1, S2 a S3 úrovně výkonu před 1. srpna 2017?</span><span class="sxs-lookup"><span data-stu-id="3e14e-179">Can I change between hello S1, S2, and S3 performance levels before August 1, 2017?</span></span>

<span data-ttu-id="3e14e-180">Pouze existující účty s výkonu S1, S2 a S3 bude možné toochange a změnit úroveň úrovně výkonu prostřednictvím portálu hello nebo prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="3e14e-180">Only existing accounts with S1, S2, and S3 performance will be able toochange and alter performance level tiers through hello portal or programmatically.</span></span> <span data-ttu-id="3e14e-181">1. srpna 2017 hello S1, S2, a úrovně výkonu S3 nadále již nebudou dostupné.</span><span class="sxs-lookup"><span data-stu-id="3e14e-181">By August 1, 2017, hello S1, S2, and S3 performance levels will no longer be available.</span></span> <span data-ttu-id="3e14e-182">Pokud změníte z kolekce tvořené jedním oddílem tooa S1, S3 nebo S3, nemůže vrátit toohello úrovní výkonu S1, S2 nebo S3.</span><span class="sxs-lookup"><span data-stu-id="3e14e-182">If you change from S1, S3, or S3 tooa single partition collection, you cannot return toohello S1, S2, or S3 performance levels.</span></span>

<a name="when-migrated"></a>

## <a name="how-will-i-know-when-my-collection-has-migrated"></a><span data-ttu-id="3e14e-183">Jak poznám, že při migraci mé kolekce?</span><span class="sxs-lookup"><span data-stu-id="3e14e-183">How will I know when my collection has migrated?</span></span>

<span data-ttu-id="3e14e-184">na 31 července 2017 dojde k migraci Hello.</span><span class="sxs-lookup"><span data-stu-id="3e14e-184">hello migration will occur on July 31, 2017.</span></span> <span data-ttu-id="3e14e-185">Pokud máte kolekci používající hello S1, S2 nebo úrovně výkonu S3, hello Cosmos DB tým vás bude kontaktovat e-mailem před provedením migrace hello.</span><span class="sxs-lookup"><span data-stu-id="3e14e-185">If you have a collection that uses hello S1, S2 or S3 performance levels, hello Cosmos DB team will contact you by email before hello migration takes place.</span></span> <span data-ttu-id="3e14e-186">Po migraci hello dokončení na 1. srpna 2017 hello portál Azure vám ukáže, že kolekce používá cenový model Standard.</span><span class="sxs-lookup"><span data-stu-id="3e14e-186">Once hello migration is complete, on August 1, 2017, hello Azure portal will show that your collection uses Standard pricing.</span></span>

![Jak tooconfirm kolekce má migrovat toohello standardní cenovou úroveň.](./media/performance-levels/portal-standard-pricing-applied.png)

<a name="migrate-diy"></a>

## <a name="how-do-i-migrate-from-hello-s1-s2-s3-performance-levels-toosingle-partition-collections-on-my-own"></a><span data-ttu-id="3e14e-188">Jak migrovat z hello S1, S2, S3 oddílu kolekce toosingle úrovně výkonu na vlastní?</span><span class="sxs-lookup"><span data-stu-id="3e14e-188">How do I migrate from hello S1, S2, S3 performance levels toosingle partition collections on my own?</span></span>

<span data-ttu-id="3e14e-189">Můžete migrovat z hello S1, S2, a úrovně výkonu S3 toosingle oddílu kolekce pomocí hello portál Azure nebo prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="3e14e-189">You can migrate from hello S1, S2, and S3 performance levels toosingle partition collections using hello Azure portal or programmatically.</span></span> <span data-ttu-id="3e14e-190">To provedete na vlastní před 1. srpna toobenefit z možností flexibilní propustnost hello k dispozici kolekce tvořené jedním oddílem, nebo jsme kolekce můžete migrovat na 31 července 2017.</span><span class="sxs-lookup"><span data-stu-id="3e14e-190">You can do this on your own before August 1 toobenefit from hello flexible throughput options available with single partition collections, or we will migrate your collections for you on July 31, 2017.</span></span>

<span data-ttu-id="3e14e-191">**toomigrate toosingle oddílu kolekce pomocí hello portálu Azure**</span><span class="sxs-lookup"><span data-stu-id="3e14e-191">**toomigrate toosingle partition collections using hello Azure portal**</span></span>

1. <span data-ttu-id="3e14e-192">V hello [ **portál Azure**](https://portal.azure.com), klikněte na tlačítko **Azure Cosmos DB**, pak vyberte toomodify účet hello Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="3e14e-192">In hello [**Azure portal**](https://portal.azure.com), click **Azure Cosmos DB**, then select hello Cosmos DB account toomodify.</span></span> 
 
    <span data-ttu-id="3e14e-193">Pokud **Azure Cosmos DB** není na hello vlevo, klikněte na tlačítko >, posuňte se příliš**databáze**, vyberte **Azure Cosmos DB**a potom vyberte účet DocumentDB hello.</span><span class="sxs-lookup"><span data-stu-id="3e14e-193">If **Azure Cosmos DB** is not on hello Jumpbar, click >, scroll too**Databases**, select **Azure Cosmos DB**, and then select hello DocumentDB account.</span></span>  

2. <span data-ttu-id="3e14e-194">V nabídce hello prostředků v části **kontejnery**, klikněte na tlačítko **škálování**hello rozevíracím seznamu vyberte hello kolekce toomodify a pak klikněte na tlačítko **cenová úroveň**.</span><span class="sxs-lookup"><span data-stu-id="3e14e-194">On hello resource menu, under **Containers**, click **Scale**, select hello collection toomodify from hello drop-down list, and then click **Pricing Tier**.</span></span> <span data-ttu-id="3e14e-195">Účty pomocí předem definovaných propustnost mít cenovou úroveň S1, S2 nebo S3.</span><span class="sxs-lookup"><span data-stu-id="3e14e-195">Accounts using pre-defined throughput have a pricing tier of S1, S2, or S3.</span></span>  <span data-ttu-id="3e14e-196">V hello **zvolte cenovou úroveň** okně klikněte na tlačítko **standardní** toochange definované toouser propustnost a potom klikněte na **vyberte** toosave změny.</span><span class="sxs-lookup"><span data-stu-id="3e14e-196">In hello **Choose your pricing tier** blade, click **Standard** toochange toouser-defined throughput, and then click **Select** toosave your change.</span></span>

    ![Snímek obrazovky okna nastavení hello znázorňující, kde toochange hello hodnotu propustnosti](./media/performance-levels/change-performance-set-thoughput.png)

3. <span data-ttu-id="3e14e-198">Zpět v hello **škálování** okno, hello **cenová úroveň** mění příliš**standardní** a hello **propustnost (RU/s)** pole se zobrazí se Výchozí hodnota 400.</span><span class="sxs-lookup"><span data-stu-id="3e14e-198">Back in hello **Scale** blade, hello **Pricing Tier** is changed too**Standard** and hello **Throughput (RU/s)** box is displayed with a default value of 400.</span></span> <span data-ttu-id="3e14e-199">Sada hello propustnost mezi 400 a 10 000 [požadované jednotky](request-units.md)/second (RU/s).</span><span class="sxs-lookup"><span data-stu-id="3e14e-199">Set hello throughput between 400 and 10,000 [Request units](request-units.md)/second (RU/s).</span></span> <span data-ttu-id="3e14e-200">Hello **odhadované měsíčních nákladů** dole hello hello stránka aktualizuje automaticky tooprovide odhad hello měsíční náklady.</span><span class="sxs-lookup"><span data-stu-id="3e14e-200">hello **Estimated Monthly Bill** at hello bottom of hello page updates automatically tooprovide an estimate of hello monthly cost.</span></span> 

    >[!IMPORTANT] 
    > <span data-ttu-id="3e14e-201">Po uložení změn a přesunout toohello standardní cenovou úroveň, nelze vrátit zpět toohello úrovní výkonu S1, S2 nebo S3.</span><span class="sxs-lookup"><span data-stu-id="3e14e-201">Once you save your changes and move toohello Standard pricing tier, you cannot roll back toohello S1, S2, or S3 performance levels.</span></span>

4. <span data-ttu-id="3e14e-202">Klikněte na tlačítko **Uložit** toosave změny.</span><span class="sxs-lookup"><span data-stu-id="3e14e-202">Click **Save** toosave your changes.</span></span>

    <span data-ttu-id="3e14e-203">Pokud zjistíte, že potřebujete další propustnost (větší než 10 000 RU/s) nebo další úložiště (větší než 10GB) můžete vytvořit kolekci oddílů.</span><span class="sxs-lookup"><span data-stu-id="3e14e-203">If you determine that you need more throughput (greater than 10,000 RU/s) or more storage (greater than 10GB) you can create a partitioned collection.</span></span> <span data-ttu-id="3e14e-204">toomigrate kolekce tooa rozdělena na oddíly kolekce tvořené jedním oddílem, najdete v části [migraci z kolekce jedním oddílem toopartitioned](documentdb-partition-data.md#migrating-from-single-partition).</span><span class="sxs-lookup"><span data-stu-id="3e14e-204">toomigrate a single partition collection tooa partitioned collection, see [Migrating from single-partition toopartitioned collections](documentdb-partition-data.md#migrating-from-single-partition).</span></span>

    > [!NOTE]
    > <span data-ttu-id="3e14e-205">Změna z S1, S2 nebo S3 tooStandard může trvat až too2 minut.</span><span class="sxs-lookup"><span data-stu-id="3e14e-205">Changing from S1, S2, or S3 tooStandard may take up too2 minutes.</span></span>
    > 
    > 

<span data-ttu-id="3e14e-206">**hello toomigrate toosingle oddílu kolekce pomocí .NET SDK**</span><span class="sxs-lookup"><span data-stu-id="3e14e-206">**toomigrate toosingle partition collections using hello .NET SDK**</span></span>

<span data-ttu-id="3e14e-207">Další možností pro změnu úrovně výkonu vaší kolekce je pomocí naší sady SDK.</span><span class="sxs-lookup"><span data-stu-id="3e14e-207">Another option for changing your collections' performance levels is through our SDKs.</span></span> <span data-ttu-id="3e14e-208">Tato část se vztahuje pouze změna shromažďování výkonu úrovně pomocí našich [DocumentDB .NET API](documentdb-sdk-dotnet.md), ale hello proces je podobný pro naše dalších sadách SDK.</span><span class="sxs-lookup"><span data-stu-id="3e14e-208">This section only covers changing a collection's performance level using our [DocumentDB .NET API](documentdb-sdk-dotnet.md), but hello process is similar for our other SDKs.</span></span>

<span data-ttu-id="3e14e-209">Zde je fragment kódu pro změnu hello kolekce propustnost too5, 000 jednotek žádosti za sekundu:</span><span class="sxs-lookup"><span data-stu-id="3e14e-209">Here is a code snippet for changing hello collection throughput too5,000 request units per second:</span></span>
    
```C#
    //Fetch hello resource toobe updated
    Offer offer = client.CreateOfferQuery()
                      .Where(r => r.ResourceLink == collection.SelfLink)    
                      .AsEnumerable()
                      .SingleOrDefault();

    // Set hello throughput too5000 request units per second
    offer = new OfferV2(offer, 5000);

    //Now persist these changes toohello database by replacing hello original resource
    await client.ReplaceOfferAsync(offer);
```

<span data-ttu-id="3e14e-210">Navštivte [MSDN](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.aspx) tooview Další příklady a další informace o našich nabídka metody:</span><span class="sxs-lookup"><span data-stu-id="3e14e-210">Visit [MSDN](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.aspx) tooview additional examples and learn more about our offer methods:</span></span>

* [<span data-ttu-id="3e14e-211">**ReadOfferAsync**</span><span class="sxs-lookup"><span data-stu-id="3e14e-211">**ReadOfferAsync**</span></span>](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.readofferasync.aspx)
* [<span data-ttu-id="3e14e-212">**ReadOffersFeedAsync**</span><span class="sxs-lookup"><span data-stu-id="3e14e-212">**ReadOffersFeedAsync**</span></span>](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.readoffersfeedasync.aspx)
* [<span data-ttu-id="3e14e-213">**ReplaceOfferAsync**</span><span class="sxs-lookup"><span data-stu-id="3e14e-213">**ReplaceOfferAsync**</span></span>](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.replaceofferasync.aspx)
* [<span data-ttu-id="3e14e-214">**CreateOfferQuery**</span><span class="sxs-lookup"><span data-stu-id="3e14e-214">**CreateOfferQuery**</span></span>](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.documentqueryable.createofferquery.aspx)

<a name="ea-customer"></a>

## <a name="how-am-i-impacted-if-im-an-ea-customer"></a><span data-ttu-id="3e14e-215">Jak se ovlivněny vám EA zákazníka?</span><span class="sxs-lookup"><span data-stu-id="3e14e-215">How am I impacted if I'm an EA customer?</span></span>

<span data-ttu-id="3e14e-216">Zákazníci EA bude cena chránit až hello konce jejich aktuální kontrakt.</span><span class="sxs-lookup"><span data-stu-id="3e14e-216">EA customers will be price protected until hello end of their current contract.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3e14e-217">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3e14e-217">Next steps</span></span>
<span data-ttu-id="3e14e-218">Další informace o cenách a správa dat s Azure Cosmos DB, toolearn těchto materiálech:</span><span class="sxs-lookup"><span data-stu-id="3e14e-218">toolearn more about pricing and managing data with Azure Cosmos DB, explore these resources:</span></span>

1.  <span data-ttu-id="3e14e-219">[Segmentace dat v databázi Cosmos](documentdb-partition-data.md).</span><span class="sxs-lookup"><span data-stu-id="3e14e-219">[Partitioning data in Cosmos DB](documentdb-partition-data.md).</span></span> <span data-ttu-id="3e14e-220">Pochopení hello rozdíl mezi kontejneru tvořené jedním oddílem a oddílů kontejnery, jakož i tipy k rozdělení tooscale strategie implementace bezproblémově.</span><span class="sxs-lookup"><span data-stu-id="3e14e-220">Understand hello difference between single partition container and partitioned containers, as well as tips on implementing a partitioning strategy tooscale seamlessly.</span></span>
2.  <span data-ttu-id="3e14e-221">[Ceny cosmos DB](https://azure.microsoft.com/pricing/details/cosmos-db/).</span><span class="sxs-lookup"><span data-stu-id="3e14e-221">[Cosmos DB pricing](https://azure.microsoft.com/pricing/details/cosmos-db/).</span></span> <span data-ttu-id="3e14e-222">Další informace o hello náklady na zřizování propustnost a využívání úložiště.</span><span class="sxs-lookup"><span data-stu-id="3e14e-222">Learn about hello cost of provisioning throughput and consuming storage.</span></span>
3.  <span data-ttu-id="3e14e-223">[Požadované jednotky](request-units.md).</span><span class="sxs-lookup"><span data-stu-id="3e14e-223">[Request units](request-units.md).</span></span> <span data-ttu-id="3e14e-224">Pochopení spotřeby hello propustnosti na typy jiné operace, například pro čtení, zápisu, dotazů.</span><span class="sxs-lookup"><span data-stu-id="3e14e-224">Understand hello consumption of throughput for different operation types, for example Read, Write, Query.</span></span>
