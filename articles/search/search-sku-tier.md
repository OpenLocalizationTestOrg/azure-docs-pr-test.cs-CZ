---
title: "Zvolte SKU nebo cenovou úroveň pro službu Azure Search | Microsoft Docs"
description: "Vyhledávání systému Azure se dá zřídit na tyto identifikátory SKU: volné, Basic a Standard, kde Standard je k dispozici v různých konfigurace prostředků a kapacity úrovně."
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
tags: azure-portal
ms.assetid: 8d4b7bca-02a5-43ee-b3f8-03551dfb32fd
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 10/24/2016
ms.author: heidist
ms.openlocfilehash: f9f3a7b2369818791ffac1c8eeccef45216c2ff0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="choose-a-sku-or-pricing-tier-for-azure-search"></a><span data-ttu-id="83c78-103">Zvolte SKU nebo cenovou úroveň pro službu Azure Search.</span><span class="sxs-lookup"><span data-stu-id="83c78-103">Choose a SKU or pricing tier for Azure Search</span></span>
<span data-ttu-id="83c78-104">Ve službě Azure Search [je služba zřízena](search-create-service-portal.md) na konkrétní cenová úroveň nebo SKU.</span><span class="sxs-lookup"><span data-stu-id="83c78-104">In Azure Search, a [service is provisioned](search-create-service-portal.md) at a specific pricing tier or SKU.</span></span> <span data-ttu-id="83c78-105">Mezi možnosti patří **volné**, **základní**, nebo **standardní**, kde **standardní** je k dispozici v několika konfigurace a kapacity.</span><span class="sxs-lookup"><span data-stu-id="83c78-105">Options include **Free**, **Basic**, or **Standard**, where **Standard** is available in multiple configurations and capacities.</span></span>

<span data-ttu-id="83c78-106">Účelem tohoto článku je vám pomohou zvolit vrstvu.</span><span class="sxs-lookup"><span data-stu-id="83c78-106">The purpose of this article is to help you choose a tier.</span></span> <span data-ttu-id="83c78-107">Pokud kapacitu vrstvy jím je příliš malá, musíte zřídit novou službu na vyšší úroveň a potom ho znovu načtěte vaší indexy.</span><span class="sxs-lookup"><span data-stu-id="83c78-107">If a tier's capacity turns out to be too low, you will need to provision a new service at the higher tier and then reload your indexes.</span></span> <span data-ttu-id="83c78-108">Neexistuje žádné místní upgrade stejné služby z jednoho identifikátoru SKU do jiného.</span><span class="sxs-lookup"><span data-stu-id="83c78-108">There is no in-place upgrade of the same service from one SKU to another.</span></span>

> [!NOTE]
> <span data-ttu-id="83c78-109">Po zvolte úroveň a [zřídit službu vyhledávání](search-create-service-portal.md)počty oddílu v rámci služby, a může zvýšit repliky.</span><span class="sxs-lookup"><span data-stu-id="83c78-109">After you choose a tier and [provision a search service](search-create-service-portal.md), you can increase replica and partition counts within the service.</span></span> <span data-ttu-id="83c78-110">Pokyny najdete v tématu [škálovat prostředek úrovně pro dotaz a indexování úlohy](search-capacity-planning.md).</span><span class="sxs-lookup"><span data-stu-id="83c78-110">For guidance, see [Scale resource levels for query and indexing workloads](search-capacity-planning.md).</span></span>
>
>

## <a name="how-to-approach-a-pricing-tier-decision"></a><span data-ttu-id="83c78-111">Postupy, dosahují cenovou úroveň rozhodnutí</span><span class="sxs-lookup"><span data-stu-id="83c78-111">How to approach a pricing tier decision</span></span>
<span data-ttu-id="83c78-112">Ve službě Azure Search určuje úroveň kapacitu, není dostupnosti funkce.</span><span class="sxs-lookup"><span data-stu-id="83c78-112">In Azure Search, the tier determines capacity, not feature availability.</span></span> <span data-ttu-id="83c78-113">Obecně platí funkce jsou dostupné v každé vrstvě, včetně funkce verze preview.</span><span class="sxs-lookup"><span data-stu-id="83c78-113">Generally, features are available at every tier, including preview features.</span></span> <span data-ttu-id="83c78-114">Jedinou výjimkou je žádná podpora pro indexery ve vysokém rozlišení S3.</span><span class="sxs-lookup"><span data-stu-id="83c78-114">The one exception is no support for indexers in S3 HD.</span></span>

> [!TIP]
> <span data-ttu-id="83c78-115">Doporučujeme vám, že vždy zřizovat **volné** služby (jeden na předplatné s bez časového omezení) tak, aby jeho snadno dostupné pro šedé – projekty.</span><span class="sxs-lookup"><span data-stu-id="83c78-115">We recommend that you always provision a **Free** service (one per subscription, with no expiration) so that its readily available for light-weight projects.</span></span> <span data-ttu-id="83c78-116">Použití **volné** služba pro testování a vyhodnocení; vytvořte druhý fakturovatelný službu na **základní** nebo **standardní** vrstva pro produkci nebo větší testovací úlohy.</span><span class="sxs-lookup"><span data-stu-id="83c78-116">Use the **Free** service for testing and evaluation; create a second billable service at the **Basic** or **Standard** tier for production or larger test workloads.</span></span>
>
>

<span data-ttu-id="83c78-117">Kapacita a nákladů na běžící služba přejděte ruční v dolním.</span><span class="sxs-lookup"><span data-stu-id="83c78-117">Capacity and costs of running the service go hand-in-hand.</span></span> <span data-ttu-id="83c78-118">Informace v tomto článku vám může pomoct rozhodnout, které zajišťuje rovnováhu mezi SKU, ale pro některý z mohla být užitečná, je třeba alespoň hrubý odhady na následující:</span><span class="sxs-lookup"><span data-stu-id="83c78-118">Information in this article can help you decide which SKU delivers the right balance, but for any of it to be useful, you need at least rough estimates on the following:</span></span>

* <span data-ttu-id="83c78-119">Počet a velikost indexy, které chcete vytvořit</span><span class="sxs-lookup"><span data-stu-id="83c78-119">Number and size of indexes you plan to create</span></span>
* <span data-ttu-id="83c78-120">Počet a velikost dokumenty nahrát</span><span class="sxs-lookup"><span data-stu-id="83c78-120">Number and size of documents to upload</span></span>
* <span data-ttu-id="83c78-121">Přehled o dotazu svazku, z hlediska dotazy na druhý (QPS)</span><span class="sxs-lookup"><span data-stu-id="83c78-121">Some idea of query volume, in terms of Queries Per Second (QPS)</span></span>

<span data-ttu-id="83c78-122">Počet a velikost jsou důležité, protože maximální limitu prostřednictvím pevný limit na počet indexů nebo dokumenty ve službě nebo na prostředky (úložiště nebo repliky), který používá služba.</span><span class="sxs-lookup"><span data-stu-id="83c78-122">Number and size are important because maximum limits are reached through a hard limit on the count of indexes or documents in a service, or on resources (storage or replicas) used by the service.</span></span> <span data-ttu-id="83c78-123">Skutečné limit služby je podle toho, co získá vyčerpáte první: prostředky nebo objekty.</span><span class="sxs-lookup"><span data-stu-id="83c78-123">The actual limit for your service is whichever gets used up first: resources or objects.</span></span>

<span data-ttu-id="83c78-124">Odhady v dolním má následující kroky zjednodušit proces:</span><span class="sxs-lookup"><span data-stu-id="83c78-124">With estimates in hand, the following steps should simplify the process:</span></span>

* <span data-ttu-id="83c78-125">**Krok 1** zkontrolujte-li se dozvědět o dostupných parametrech SKU popisy.</span><span class="sxs-lookup"><span data-stu-id="83c78-125">**Step 1** Review the SKU descriptions below to learn about available options.</span></span>
* <span data-ttu-id="83c78-126">**Krok 2** odpovězte na otázky níže přicházejí na předběžné rozhodnutí.</span><span class="sxs-lookup"><span data-stu-id="83c78-126">**Step 2** Answer the questions below to arrive at a preliminary decision.</span></span>
* <span data-ttu-id="83c78-127">**Krok 3** dokončit vaše rozhodnutí Kontrola pevných limitů na úložiště a ceny.</span><span class="sxs-lookup"><span data-stu-id="83c78-127">**Step 3** Finalize your decision by reviewing hard limits on storage and pricing.</span></span>

## <a name="sku-descriptions"></a><span data-ttu-id="83c78-128">Popisy SKU</span><span class="sxs-lookup"><span data-stu-id="83c78-128">SKU descriptions</span></span>
<span data-ttu-id="83c78-129">Následující tabulka obsahuje popis každého vrstvy.</span><span class="sxs-lookup"><span data-stu-id="83c78-129">The following table provides descriptions of each tier.</span></span>

| <span data-ttu-id="83c78-130">Úroveň</span><span class="sxs-lookup"><span data-stu-id="83c78-130">Tier</span></span> | <span data-ttu-id="83c78-131">Primární scénáře</span><span class="sxs-lookup"><span data-stu-id="83c78-131">Primary scenarios</span></span> |
| --- | --- |
| <span data-ttu-id="83c78-132">**Volné**</span><span class="sxs-lookup"><span data-stu-id="83c78-132">**Free**</span></span> |<span data-ttu-id="83c78-133">Sdílené služby, bez poplatků, používat pro vyhodnocení, šetření nebo malé úlohy.</span><span class="sxs-lookup"><span data-stu-id="83c78-133">A shared service, at no charge, used for evaluation, investigation, or small workloads.</span></span> <span data-ttu-id="83c78-134">Vzhledem k tomu, že jsou sdílena s další odběratele, propustnost dotazu a indexování se liší podle uživatele, kteří je pomocí služby.</span><span class="sxs-lookup"><span data-stu-id="83c78-134">Because it's shared with other subscribers, query throughput and indexing varies based on who else is using the service.</span></span> <span data-ttu-id="83c78-135">Kapacita je malý (50 MB nebo 3 indexy s až 10 000 dokumentů. každý).</span><span class="sxs-lookup"><span data-stu-id="83c78-135">Capacity is small (50 MB or 3 indexes with up 10,000 documents each).</span></span> |
| <span data-ttu-id="83c78-136">**Basic**</span><span class="sxs-lookup"><span data-stu-id="83c78-136">**Basic**</span></span> |<span data-ttu-id="83c78-137">Malé produkční úlohy na vyhrazeném hardwaru.</span><span class="sxs-lookup"><span data-stu-id="83c78-137">Small production workloads on dedicated hardware.</span></span> <span data-ttu-id="83c78-138">Vysoce dostupný.</span><span class="sxs-lookup"><span data-stu-id="83c78-138">Highly available.</span></span> <span data-ttu-id="83c78-139">Kapacita je až 3 repliky a 1 oddílu (2 GB).</span><span class="sxs-lookup"><span data-stu-id="83c78-139">Capacity is up to 3 replicas and 1 partition (2 GB).</span></span> |
| <span data-ttu-id="83c78-140">**S1**</span><span class="sxs-lookup"><span data-stu-id="83c78-140">**S1**</span></span> |<span data-ttu-id="83c78-141">Standardní 1 podporuje flexibilní kombinace oddíly (12) a repliky (12), použít pro střední produkční zatížení na vyhrazeném hardwaru.</span><span class="sxs-lookup"><span data-stu-id="83c78-141">Standard 1 supports flexible combinations of partitions (12) and replicas (12), used for medium production workloads on dedicated hardware.</span></span> <span data-ttu-id="83c78-142">Přidělením replik v kombinacích nepodporuje maximální počet jednotek 36 fakturovatelný vyhledávání a oddíly.</span><span class="sxs-lookup"><span data-stu-id="83c78-142">You can allocate partitions and replicas in combinations supported by a maximum number of 36 billable search units.</span></span> <span data-ttu-id="83c78-143">Na této úrovni oddíly jsou 25 GB a QPS je přibližně 15 dotazů za sekundu.</span><span class="sxs-lookup"><span data-stu-id="83c78-143">At this level, partitions are 25 GB each and QPS is approximately 15 queries per second.</span></span> |
| <span data-ttu-id="83c78-144">**S2**</span><span class="sxs-lookup"><span data-stu-id="83c78-144">**S2**</span></span> |<span data-ttu-id="83c78-145">Spustí standardní 2 větší úlohy v produkčním prostředí pomocí stejné 36 vyhledávání jednotky jako S1, ale s větší velikostí oddílů a repliky.</span><span class="sxs-lookup"><span data-stu-id="83c78-145">Standard 2 runs larger production workloads using the same 36 search units as S1 but with larger sized partitions and replicas.</span></span> <span data-ttu-id="83c78-146">Na této úrovni oddíly jsou 100 GB a QPS je o 60 dotazů za sekundu.</span><span class="sxs-lookup"><span data-stu-id="83c78-146">At this level, partitions are 100 GB each and QPS is about 60 queries per second.</span></span> |
| <span data-ttu-id="83c78-147">**S3**</span><span class="sxs-lookup"><span data-stu-id="83c78-147">**S3**</span></span> |<span data-ttu-id="83c78-148">Standardní 3 běží úměrně větší úlohy v produkčním prostředí na vyšší end systémy, v konfiguracích až 12 oddíly nebo 12 repliky jednotek v části 36 vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="83c78-148">Standard 3 runs proportionally larger production workloads on higher end systems, in configurations of up to 12 partitions or 12 replicas under 36 search units.</span></span> <span data-ttu-id="83c78-149">Na této úrovni oddíly jsou 200 GB a QPS je více než 60 dotazů za sekundu.</span><span class="sxs-lookup"><span data-stu-id="83c78-149">At this level, partitions are 200 GB each and QPS is more than 60 queries per second.</span></span> |
| <span data-ttu-id="83c78-150">**S3 HD**</span><span class="sxs-lookup"><span data-stu-id="83c78-150">**S3 HD**</span></span> |<span data-ttu-id="83c78-151">Standardní 3 hustotou je určená pro velký počet menší indexy.</span><span class="sxs-lookup"><span data-stu-id="83c78-151">Standard 3 High Density is designed for a large number of smaller indexes.</span></span> <span data-ttu-id="83c78-152">Může mít až 3 oddíly, na 200 GB.</span><span class="sxs-lookup"><span data-stu-id="83c78-152">You can have up to 3 partitions, at 200 GB each.</span></span> <span data-ttu-id="83c78-153">QPS je více než 60 dotazů za sekundu.</span><span class="sxs-lookup"><span data-stu-id="83c78-153">QPS is more than 60 queries per second.</span></span> |

> [!NOTE]
> <span data-ttu-id="83c78-154">Maximální hodnoty repliky a oddíl se účtují se jako vyhledávání jednotek (36 maximální pro službu), které ukládá nižší limit efektivní než maximální znamená v hodnotě řez.</span><span class="sxs-lookup"><span data-stu-id="83c78-154">Replica and partition maximums are billed out as search units (36 unit maximum per service), which imposes a lower effective limit than what the maximum implies at face value.</span></span> <span data-ttu-id="83c78-155">Například pokud chcete použít maximálně 12 repliky, může mít nejvýše 3 oddíly (12 * 3 = 36 jednotek).</span><span class="sxs-lookup"><span data-stu-id="83c78-155">For example, to use the maximum of 12 replicas, you could have at most 3 partitions (12 * 3 = 36 units).</span></span> <span data-ttu-id="83c78-156">Podobně použít maximální oddíly, snížíte repliky 3.</span><span class="sxs-lookup"><span data-stu-id="83c78-156">Similarly, to use maximum partitions, reduce replicas to 3.</span></span> <span data-ttu-id="83c78-157">V tématu [škálovat prostředek úrovně pro dotaz a indexování úlohy ve službě Azure Search](search-capacity-planning.md) pro graf na povolené kombinace.</span><span class="sxs-lookup"><span data-stu-id="83c78-157">See [Scale resource levels for query and indexing workloads in Azure Search](search-capacity-planning.md) for a chart on allowable combinations.</span></span>
>
>

## <a name="review-limits-per-tier"></a><span data-ttu-id="83c78-158">Zkontrolujte omezení na vrstvě</span><span class="sxs-lookup"><span data-stu-id="83c78-158">Review limits per tier</span></span>
<span data-ttu-id="83c78-159">Následující graf je podmnožinou omezení z [omezení služby ve službě Azure Search](search-limits-quotas-capacity.md).</span><span class="sxs-lookup"><span data-stu-id="83c78-159">The following chart is a subset of the limits from [Service Limits in Azure Search](search-limits-quotas-capacity.md).</span></span> <span data-ttu-id="83c78-160">Zobrazí se seznam faktory nejpravděpodobněji mít vliv na rozhodnutí SKU.</span><span class="sxs-lookup"><span data-stu-id="83c78-160">It lists the factors most likely to impact a SKU decision.</span></span> <span data-ttu-id="83c78-161">Tento graf může být při revizi níže uvedené otázky.</span><span class="sxs-lookup"><span data-stu-id="83c78-161">You can refer to this chart when reviewing the questions below.</span></span>

| <span data-ttu-id="83c78-162">Prostředek</span><span class="sxs-lookup"><span data-stu-id="83c78-162">Resource</span></span> | <span data-ttu-id="83c78-163">Free</span><span class="sxs-lookup"><span data-stu-id="83c78-163">Free</span></span> | <span data-ttu-id="83c78-164">Basic</span><span class="sxs-lookup"><span data-stu-id="83c78-164">Basic</span></span> | <span data-ttu-id="83c78-165">S1</span><span class="sxs-lookup"><span data-stu-id="83c78-165">S1</span></span> | <span data-ttu-id="83c78-166">S2</span><span class="sxs-lookup"><span data-stu-id="83c78-166">S2</span></span> | <span data-ttu-id="83c78-167">S3</span><span class="sxs-lookup"><span data-stu-id="83c78-167">S3</span></span> | <span data-ttu-id="83c78-168">S3 HD</span><span class="sxs-lookup"><span data-stu-id="83c78-168">S3 HD</span></span> |
| --- | --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="83c78-169">Smlouva SLA</span><span class="sxs-lookup"><span data-stu-id="83c78-169">Service Level Agreement (SLA)</span></span> |<span data-ttu-id="83c78-170">Ne <sup>1</sup></span><span class="sxs-lookup"><span data-stu-id="83c78-170">No <sup>1</sup></span></span> |<span data-ttu-id="83c78-171">Ano</span><span class="sxs-lookup"><span data-stu-id="83c78-171">Yes</span></span> |<span data-ttu-id="83c78-172">Ano</span><span class="sxs-lookup"><span data-stu-id="83c78-172">Yes</span></span> |<span data-ttu-id="83c78-173">Ano</span><span class="sxs-lookup"><span data-stu-id="83c78-173">Yes</span></span> |<span data-ttu-id="83c78-174">Ano</span><span class="sxs-lookup"><span data-stu-id="83c78-174">Yes</span></span> |<span data-ttu-id="83c78-175">Ano</span><span class="sxs-lookup"><span data-stu-id="83c78-175">Yes</span></span> |
| <span data-ttu-id="83c78-176">Omezení indexu</span><span class="sxs-lookup"><span data-stu-id="83c78-176">Index limits</span></span> |<span data-ttu-id="83c78-177">3</span><span class="sxs-lookup"><span data-stu-id="83c78-177">3</span></span> |<span data-ttu-id="83c78-178">5</span><span class="sxs-lookup"><span data-stu-id="83c78-178">5</span></span> |<span data-ttu-id="83c78-179">50</span><span class="sxs-lookup"><span data-stu-id="83c78-179">50</span></span> |<span data-ttu-id="83c78-180">200</span><span class="sxs-lookup"><span data-stu-id="83c78-180">200</span></span> |<span data-ttu-id="83c78-181">200</span><span class="sxs-lookup"><span data-stu-id="83c78-181">200</span></span> |<span data-ttu-id="83c78-182">1000 <sup>2</sup></span><span class="sxs-lookup"><span data-stu-id="83c78-182">1000 <sup>2</sup></span></span> |
| <span data-ttu-id="83c78-183">Omezení dokumentů vztahuje</span><span class="sxs-lookup"><span data-stu-id="83c78-183">Document limits</span></span> |<span data-ttu-id="83c78-184">10 000 celkem</span><span class="sxs-lookup"><span data-stu-id="83c78-184">10,000 total</span></span> |<span data-ttu-id="83c78-185">1 milion pro službu</span><span class="sxs-lookup"><span data-stu-id="83c78-185">1 million per service</span></span> |<span data-ttu-id="83c78-186">15 milionů na oddíly</span><span class="sxs-lookup"><span data-stu-id="83c78-186">15 million per partition</span></span> |<span data-ttu-id="83c78-187">60 milionů na oddíly</span><span class="sxs-lookup"><span data-stu-id="83c78-187">60 million per partition</span></span> |<span data-ttu-id="83c78-188">120 milionů na oddíly</span><span class="sxs-lookup"><span data-stu-id="83c78-188">120 million per partition</span></span> |<span data-ttu-id="83c78-189">1 milion na index</span><span class="sxs-lookup"><span data-stu-id="83c78-189">1 million per index</span></span> |
| <span data-ttu-id="83c78-190">Maximální oddíly</span><span class="sxs-lookup"><span data-stu-id="83c78-190">Maximum partitions</span></span> |<span data-ttu-id="83c78-191">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="83c78-191">N/A</span></span> |<span data-ttu-id="83c78-192">1</span><span class="sxs-lookup"><span data-stu-id="83c78-192">1</span></span> |<span data-ttu-id="83c78-193">12</span><span class="sxs-lookup"><span data-stu-id="83c78-193">12</span></span> |<span data-ttu-id="83c78-194">12</span><span class="sxs-lookup"><span data-stu-id="83c78-194">12</span></span> |<span data-ttu-id="83c78-195">12</span><span class="sxs-lookup"><span data-stu-id="83c78-195">12</span></span> |<span data-ttu-id="83c78-196">3 <sup>2</sup></span><span class="sxs-lookup"><span data-stu-id="83c78-196">3 <sup>2</sup></span></span> |
| <span data-ttu-id="83c78-197">Velikost oddílu</span><span class="sxs-lookup"><span data-stu-id="83c78-197">Partition size</span></span> |<span data-ttu-id="83c78-198">Celkový počet 50 MB</span><span class="sxs-lookup"><span data-stu-id="83c78-198">50 MB total</span></span> |<span data-ttu-id="83c78-199">2 GB pro službu</span><span class="sxs-lookup"><span data-stu-id="83c78-199">2 GB per service</span></span> |<span data-ttu-id="83c78-200">25 GB na oddíly</span><span class="sxs-lookup"><span data-stu-id="83c78-200">25 GB per partition</span></span> |<span data-ttu-id="83c78-201">100 GB na oddíl (až do maximálního počtu 1.2 TB pro službu)</span><span class="sxs-lookup"><span data-stu-id="83c78-201">100 GB per partition (up to a maximum of 1.2 TB per service)</span></span> |<span data-ttu-id="83c78-202">200 GB na oddíl (až do maximálního počtu 2.4 TB pro službu)</span><span class="sxs-lookup"><span data-stu-id="83c78-202">200 GB per partition (up to a maximum of 2.4 TB per service)</span></span> |<span data-ttu-id="83c78-203">200 GB (až do maximálního počtu 600 GB pro službu)</span><span class="sxs-lookup"><span data-stu-id="83c78-203">200 GB (up to a maximum of 600 GB per service)</span></span> |
| <span data-ttu-id="83c78-204">Maximální repliky</span><span class="sxs-lookup"><span data-stu-id="83c78-204">Maximum replicas</span></span> |<span data-ttu-id="83c78-205">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="83c78-205">N/A</span></span> |<span data-ttu-id="83c78-206">3</span><span class="sxs-lookup"><span data-stu-id="83c78-206">3</span></span> |<span data-ttu-id="83c78-207">12</span><span class="sxs-lookup"><span data-stu-id="83c78-207">12</span></span> |<span data-ttu-id="83c78-208">12</span><span class="sxs-lookup"><span data-stu-id="83c78-208">12</span></span> |<span data-ttu-id="83c78-209">12</span><span class="sxs-lookup"><span data-stu-id="83c78-209">12</span></span> |<span data-ttu-id="83c78-210">12</span><span class="sxs-lookup"><span data-stu-id="83c78-210">12</span></span> |
| <span data-ttu-id="83c78-211">Dotazy na za sekundu</span><span class="sxs-lookup"><span data-stu-id="83c78-211">Queries per second</span></span> |<span data-ttu-id="83c78-212">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="83c78-212">N/A</span></span> |<span data-ttu-id="83c78-213">Přibližně 3 na repliku</span><span class="sxs-lookup"><span data-stu-id="83c78-213">~3 per replica</span></span> |<span data-ttu-id="83c78-214">Přibližně 15 na repliku</span><span class="sxs-lookup"><span data-stu-id="83c78-214">~15 per replica</span></span> |<span data-ttu-id="83c78-215">Přibližně 60 na repliku</span><span class="sxs-lookup"><span data-stu-id="83c78-215">~60 per replica</span></span> |<span data-ttu-id="83c78-216">Více než 60 na repliku</span><span class="sxs-lookup"><span data-stu-id="83c78-216">>60 per replica</span></span> |<span data-ttu-id="83c78-217">Více než 60 na repliku</span><span class="sxs-lookup"><span data-stu-id="83c78-217">>60 per replica</span></span> |

<span data-ttu-id="83c78-218"><sup>1</sup> volné vrstvy a preview funkce není součástí smlouvy o úrovni služeb (SLA).</span><span class="sxs-lookup"><span data-stu-id="83c78-218"><sup>1</sup> Free tier and preview features do not come with service level agreements (SLAs).</span></span> <span data-ttu-id="83c78-219">Pro všechny fakturovatelné úrovně SLA projeví při zřizování dostatečná redundance pro vaši službu.</span><span class="sxs-lookup"><span data-stu-id="83c78-219">For all billable tiers, SLAs take effect when you provision sufficient redundancy for your service.</span></span> <span data-ttu-id="83c78-220">Dvě nebo více replik jsou požadovány pro SLA dotazu (načíst).</span><span class="sxs-lookup"><span data-stu-id="83c78-220">Two or more replicas are required for query (read) SLA.</span></span> <span data-ttu-id="83c78-221">Tři nebo více replik jsou požadovány pro dotazy a indexování smlouvy o úrovni služeb (pro čtení a zápis).</span><span class="sxs-lookup"><span data-stu-id="83c78-221">Three or more replicas are required for query and indexing (read-write) SLA.</span></span> <span data-ttu-id="83c78-222">Počet oddílů není k posouzení SLA.</span><span class="sxs-lookup"><span data-stu-id="83c78-222">The number of partitions is not an SLA consideration.</span></span> 

<span data-ttu-id="83c78-223"><sup>2</sup> S3 a S3 HD jsou zajišťované identické vysoké kapacity infrastruktury ale každé z nich dosáhne své maximální limit různými způsoby.</span><span class="sxs-lookup"><span data-stu-id="83c78-223"><sup>2</sup> S3 and S3 HD are backed by identical high capacity infrastructure but each one reaches its maximum limit in different ways.</span></span> <span data-ttu-id="83c78-224">S3 cílí menší počet velké indexy.</span><span class="sxs-lookup"><span data-stu-id="83c78-224">S3 targets a smaller number of very large indexes.</span></span> <span data-ttu-id="83c78-225">Jako takový jeho maximální limit je vázán na prostředků (pro každou službu 2.4 TB).</span><span class="sxs-lookup"><span data-stu-id="83c78-225">As such, its maximum limit is resource-bound (2.4 TB for each service).</span></span> <span data-ttu-id="83c78-226">S3 HD cílí velký počet velmi malé indexy.</span><span class="sxs-lookup"><span data-stu-id="83c78-226">S3 HD targets a large number of very small indexes.</span></span> <span data-ttu-id="83c78-227">Na 1000 indexy S3 HD dosaženo omezení ve formě indexu omezení.</span><span class="sxs-lookup"><span data-stu-id="83c78-227">At 1,000 indexes, S3 HD reaches its limits in the form of index constraints.</span></span> <span data-ttu-id="83c78-228">Pokud jste zákazník S3 HD, který vyžaduje více než 1 000 indexy, obraťte se na Microsoft Support informace o tom, jak pokračovat.</span><span class="sxs-lookup"><span data-stu-id="83c78-228">If you are an S3 HD customer who requires more than 1,000 indexes, contact Microsoft Support for information on how to proceed.</span></span>

## <a name="eliminate-skus-that-dont-meet-requirements"></a><span data-ttu-id="83c78-229">Odstranění položek SKU, které nesplňují požadavky</span><span class="sxs-lookup"><span data-stu-id="83c78-229">Eliminate SKUs that don't meet requirements</span></span>
<span data-ttu-id="83c78-230">Na následující otázky vám může pomoct přicházejí na správné rozhodnutí SKU pro úlohy.</span><span class="sxs-lookup"><span data-stu-id="83c78-230">The following questions can help you arrive at the right SKU decision for your workload.</span></span>

1. <span data-ttu-id="83c78-231">Máte **smlouvy o úrovni služeb (SLA)** požadavky?</span><span class="sxs-lookup"><span data-stu-id="83c78-231">Do you have **Service Level Agreement (SLA)** requirements?</span></span> <span data-ttu-id="83c78-232">Můžete použít libovolné fakturovatelný úrovně (základní na nahoru), ale je nutné nakonfigurovat služby pro redundanci.</span><span class="sxs-lookup"><span data-stu-id="83c78-232">You can use any billable tier (Basic on up), but you must configure your service for redundancy.</span></span> <span data-ttu-id="83c78-233">Dvě nebo více replik jsou požadovány pro SLA dotazu (načíst).</span><span class="sxs-lookup"><span data-stu-id="83c78-233">Two or more replicas are required for query (read) SLA.</span></span> <span data-ttu-id="83c78-234">Tři nebo více replik jsou požadovány pro dotazy a indexování smlouvy o úrovni služeb (pro čtení a zápis).</span><span class="sxs-lookup"><span data-stu-id="83c78-234">Three or more replicas are required for query and indexing (read-write) SLA.</span></span> <span data-ttu-id="83c78-235">Počet oddílů není k posouzení SLA.</span><span class="sxs-lookup"><span data-stu-id="83c78-235">The number of partitions is not an SLA consideration.</span></span>
2. <span data-ttu-id="83c78-236">**Kolik indexuje** chcete?</span><span class="sxs-lookup"><span data-stu-id="83c78-236">**How many indexes** do you require?</span></span> <span data-ttu-id="83c78-237">Jeden z největších proměnné řešení do rozhodnutí SKU je počet indexů nepodporuje každý SKU.</span><span class="sxs-lookup"><span data-stu-id="83c78-237">One of the biggest variables factoring into a SKU decision is the number of indexes supported by each SKU.</span></span> <span data-ttu-id="83c78-238">Podpora index je výrazně liší úrovni v nižší cenové úrovně.</span><span class="sxs-lookup"><span data-stu-id="83c78-238">Index support is at markedly different levels in the lower pricing tiers.</span></span> <span data-ttu-id="83c78-239">Požadavky na počet indexů může být primární určující SKU rozhodnutí.</span><span class="sxs-lookup"><span data-stu-id="83c78-239">Requirements on number of indexes could be a primary determinant of a SKU decision.</span></span>
3. <span data-ttu-id="83c78-240">**Kolik dokumenty** bude načten do každé indexu?</span><span class="sxs-lookup"><span data-stu-id="83c78-240">**How many documents** will be loaded into each index?</span></span> <span data-ttu-id="83c78-241">Počet a velikost dokumentů určí případnou velikost index.</span><span class="sxs-lookup"><span data-stu-id="83c78-241">The number and size of documents will determine the eventual size of the index.</span></span> <span data-ttu-id="83c78-242">Za předpokladu, že můžete odhadnout velikost předpokládané indexu, můžete porovnat počet pro velikost oddílu za SKU, prodloužena počet oddílů, které jsou potřebné pro uložení indexu této velikosti.</span><span class="sxs-lookup"><span data-stu-id="83c78-242">Assuming you can estimate the projected size of the index, you can compare that number against the partition size per SKU, extended by the number of partitions required to store an index of that size.</span></span>
4. <span data-ttu-id="83c78-243">**Co je očekávané dotazu zatížení**?</span><span class="sxs-lookup"><span data-stu-id="83c78-243">**What is the expected query load**?</span></span> <span data-ttu-id="83c78-244">Jakmile jsou požadavky na úložiště pochopeny, vezměte v úvahu dotazu úlohy.</span><span class="sxs-lookup"><span data-stu-id="83c78-244">Once storage requirements are understood, consider query workloads.</span></span> <span data-ttu-id="83c78-245">S2 a obě položky S3 nabízejí téměř ekvivalentní propustnost, ale bude požadavků SLA vyloučit všechny preview SKU.</span><span class="sxs-lookup"><span data-stu-id="83c78-245">S2 and both S3 SKUs offer near-equivalent throughput, but SLA requirements will rule out any preview SKUs.</span></span>
5. <span data-ttu-id="83c78-246">Pokud uvažujete o úroveň S2 nebo S3, zjistěte, zda chcete, aby [indexery](search-indexer-overview.md).</span><span class="sxs-lookup"><span data-stu-id="83c78-246">If you are considering the S2 or S3 tier, determine whether you require [indexers](search-indexer-overview.md).</span></span> <span data-ttu-id="83c78-247">Indexery ještě nejsou k dispozici pro vrstvu S3 HD.</span><span class="sxs-lookup"><span data-stu-id="83c78-247">Indexers are not yet available for the S3 HD tier.</span></span> <span data-ttu-id="83c78-248">Alternativní způsob je použití push model aktualizací index, kde můžete psát kód aplikace tak, aby nabízel sady dat do indexu.</span><span class="sxs-lookup"><span data-stu-id="83c78-248">Alternative approach is to use a push model for index updates, where you write application code to push a data set to an index.</span></span>

<span data-ttu-id="83c78-249">Většina zákazníků může pravidlo konkrétní SKU příchozí nebo odchozí, na základě jejich odpovědí na otázky výše.</span><span class="sxs-lookup"><span data-stu-id="83c78-249">Most customers can rule a specific SKU in or out based on their answers to the above questions.</span></span> <span data-ttu-id="83c78-250">Pokud si pořád nejste jistí které SKU pomocí, můžete Ptejte se na webu MSDN nebo StackOverflow fóra, nebo požádejte podporu Azure o další pokyny.</span><span class="sxs-lookup"><span data-stu-id="83c78-250">If you still aren't sure which SKU to go with, you can post questions to MSDN or StackOverflow forums, or contact Azure Support for further guidance.</span></span>

## <a name="decision-validation-does-the-sku-offer-sufficient-storage-and-qps"></a><span data-ttu-id="83c78-251">Decision ověření: verze SKU nabízí systém dostatečné úložiště a QPS?</span><span class="sxs-lookup"><span data-stu-id="83c78-251">Decision validation: does the SKU offer sufficient storage and QPS?</span></span>
<span data-ttu-id="83c78-252">V posledním kroku, pokroku [stránce s cenami](https://azure.microsoft.com/pricing/details/search/) a [částech-service a za index v omezení služby](search-limits-quotas-capacity.md) znovu zkontrolovat odhady proti limitů předplatného a služby.</span><span class="sxs-lookup"><span data-stu-id="83c78-252">As a last step, revisit the [pricing page](https://azure.microsoft.com/pricing/details/search/) and the [per-service and per-index sections in Service Limits](search-limits-quotas-capacity.md) to double-check your estimates against subscription and service limits.</span></span>

<span data-ttu-id="83c78-253">Pokud požadavky na cenu nebo úložiště jsou mimo rozsah, můžete chtít Refaktorovat zatížení mezi více menší služeb (například).</span><span class="sxs-lookup"><span data-stu-id="83c78-253">If either the price or storage requirements are out of bounds, you might want to refactor the workloads among multiple smaller services (for example).</span></span> <span data-ttu-id="83c78-254">Na podrobnější úrovni můžete změnit návrh indexy na menší hodnotu, nebo použití filtrů k efektivní dotazy.</span><span class="sxs-lookup"><span data-stu-id="83c78-254">On more granular level, you could redesign indexes to be smaller, or use filters to make queries more efficient.</span></span>

> [!NOTE]
> <span data-ttu-id="83c78-255">Požadavky na úložiště může být přepsání zvýšeným, pokud dokumenty obsahují nadbytečné data.</span><span class="sxs-lookup"><span data-stu-id="83c78-255">Storage requirements can be over-inflated if documents contain extraneous data.</span></span> <span data-ttu-id="83c78-256">V ideálním případě dokumenty obsahují pouze s možností vyhledávání dat nebo metadat.</span><span class="sxs-lookup"><span data-stu-id="83c78-256">Ideally, documents contain only searchable data or metadata.</span></span> <span data-ttu-id="83c78-257">Binární data je jiný prohledávat a je třeba uložit odděleně (třeba v tabulce nebo objekt blob úložiště Azure) s pole v indexu pro uložení adresy URL odkaz na externí data.</span><span class="sxs-lookup"><span data-stu-id="83c78-257">Binary data is non-searchable and should be stored separately (perhaps in an Azure table or blob storage) with a field in the index to hold a URL reference to the external data.</span></span> <span data-ttu-id="83c78-258">Maximální velikost jednotlivých dokumentu je 16 MB (nebo méně Pokud jste hromadné odesílání více dokumentů v jedné žádosti).</span><span class="sxs-lookup"><span data-stu-id="83c78-258">The maximum size of an individual document is 16 MB (or less if you are bulk uploading multiple documents in one request).</span></span> <span data-ttu-id="83c78-259">V tématu [omezení ve službě Azure Search služby](search-limits-quotas-capacity.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="83c78-259">See [Service limits in Azure Search](search-limits-quotas-capacity.md) for more information.</span></span>
>
>

## <a name="next-step"></a><span data-ttu-id="83c78-260">Další krok</span><span class="sxs-lookup"><span data-stu-id="83c78-260">Next step</span></span>
<span data-ttu-id="83c78-261">Jakmile víte, které je vpravo fit SKU, pokračujte v těchto krocích:</span><span class="sxs-lookup"><span data-stu-id="83c78-261">Once you know which SKU is the right fit, continue on with these steps:</span></span>

* [<span data-ttu-id="83c78-262">Vytvoření služby search na portálu</span><span class="sxs-lookup"><span data-stu-id="83c78-262">Create a search service in the portal</span></span>](search-create-service-portal.md)
* [<span data-ttu-id="83c78-263">Změnit přidělení oddíly a repliky škálování služby</span><span class="sxs-lookup"><span data-stu-id="83c78-263">Change the allocation of partitions and replicas to scale your service</span></span>](search-capacity-planning.md)