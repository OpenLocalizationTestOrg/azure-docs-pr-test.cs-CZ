---
title: "Plánování kapacity pro Azure Search | Microsoft Docs"
description: "Upravte oddíl a repliky prostředky počítače ve službě Azure Search, kde je každý prostředek za cenu v jednotkách fakturovatelný vyhledávání."
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
tags: azure-portal
ms.assetid: 1dc16afe-56f9-439d-8874-1733ae1a2b74
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 02/08/2017
ms.author: heidist
ms.openlocfilehash: 26f5e71f3d00161a92de702209e224008ec8a5ae
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="scale-resource-levels-for-query-and-indexing-workloads-in-azure-search"></a><span data-ttu-id="9113a-103">Škálování prostředku úrovně pro dotaz a indexování úlohy ve službě Azure Search</span><span class="sxs-lookup"><span data-stu-id="9113a-103">Scale resource levels for query and indexing workloads in Azure Search</span></span>
<span data-ttu-id="9113a-104">Po jste [zvolte cenovou úroveň](search-sku-tier.md) a [zřídit službu vyhledávání](search-create-service-portal.md), dalším krokem je volitelně zvýšit počet replik nebo oddíly, které používá vaše služba.</span><span class="sxs-lookup"><span data-stu-id="9113a-104">After you [choose a pricing tier](search-sku-tier.md) and [provision a search service](search-create-service-portal.md), the next step is to optionally increase the number of replicas or partitions used by your service.</span></span> <span data-ttu-id="9113a-105">Každá úroveň nabízí pevný počet fakturace jednotky.</span><span class="sxs-lookup"><span data-stu-id="9113a-105">Each tier offers a fixed number of billing units.</span></span> <span data-ttu-id="9113a-106">Tento článek vysvětluje, jak přidělit tyto jednotky k dosažení optimálního konfigurace, který vyrovnává vaše požadavky na spuštění dotazu, indexování a úložiště.</span><span class="sxs-lookup"><span data-stu-id="9113a-106">This article explains how to allocate those units to achieve an optimal configuration that balances your requirements for query execution, indexing, and storage.</span></span>

<span data-ttu-id="9113a-107">Konfigurace prostředků je k dispozici při nastavování služby v [úroveň Basic](http://aka.ms/azuresearchbasic) nebo jeden z [standardní úrovně](search-limits-quotas-capacity.md).</span><span class="sxs-lookup"><span data-stu-id="9113a-107">Resource configuration is available when you set up a service at the [Basic tier](http://aka.ms/azuresearchbasic) or one of the [Standard tiers](search-limits-quotas-capacity.md).</span></span> <span data-ttu-id="9113a-108">Pro fakturovatelný služby na tyto úrovně je kapacity dokupovat v jednotkách po *jednotek vyhledávání* (SUs) kde každý oddíl a repliky počítá jako jedna SU.</span><span class="sxs-lookup"><span data-stu-id="9113a-108">For billable services at these tiers, capacity is purchased in increments of *search units* (SUs) where each partition and replica counts as one SU.</span></span> 

<span data-ttu-id="9113a-109">Použití méně služby SUs výsledky v úměrně nižší faktury.</span><span class="sxs-lookup"><span data-stu-id="9113a-109">Using fewer SUs results in a proportionally lower bill.</span></span> <span data-ttu-id="9113a-110">Fakturace pro platí, dokud je nastavení služby.</span><span class="sxs-lookup"><span data-stu-id="9113a-110">Billing is in effect for as long as the service is set up.</span></span> <span data-ttu-id="9113a-111">Pokud nepoužíváte dočasně služby, je jediným způsobem, jak se vyhnout fakturace odstraněním služby a znovu vytvořit ji v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="9113a-111">If you are temporarily not using a service, the only way to avoid billing is by deleting the service and then re-creating it when you need it.</span></span>

> [!Note]
> <span data-ttu-id="9113a-112">Odstraněním služby odstraníte všechno, co na něm.</span><span class="sxs-lookup"><span data-stu-id="9113a-112">Deleting a service deletes everything on it.</span></span> <span data-ttu-id="9113a-113">Neexistuje žádné zařízení v rámci Azure Search pro zálohování a obnovení trvalá data vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="9113a-113">There is no facility within Azure Search for backing up and restoring persisted search data.</span></span> <span data-ttu-id="9113a-114">K opětovnému nasazení stávajícího indexu na novou službu, byste měli spustit program použitý k vytvoření a načtení ho původně.</span><span class="sxs-lookup"><span data-stu-id="9113a-114">To redeploy an existing index on a new service, you should run the program used to create and load it originally.</span></span> 

## <a name="terminology-partitions-and-replicas"></a><span data-ttu-id="9113a-115">Terminologie: oddíly a repliky</span><span class="sxs-lookup"><span data-stu-id="9113a-115">Terminology: partitions and replicas</span></span>
<span data-ttu-id="9113a-116">Oddíly a repliky jsou primární prostředky, které zpět vyhledávací službu.</span><span class="sxs-lookup"><span data-stu-id="9113a-116">Partitions and replicas are the primary resources that back a search service.</span></span>

| <span data-ttu-id="9113a-117">Prostředek</span><span class="sxs-lookup"><span data-stu-id="9113a-117">Resource</span></span> | <span data-ttu-id="9113a-118">Definice</span><span class="sxs-lookup"><span data-stu-id="9113a-118">Definition</span></span> |
|----------|------------|
|<span data-ttu-id="9113a-119">*Oddíly*</span><span class="sxs-lookup"><span data-stu-id="9113a-119">*Partitions*</span></span> | <span data-ttu-id="9113a-120">Poskytuje index úložiště a vstupně-výstupních operací pro operace čtení a zápisu (například když znovu sestavit nebo aktualizovat index).</span><span class="sxs-lookup"><span data-stu-id="9113a-120">Provides index storage and I/O for read/write operations (for example, when rebuilding or refreshing an index).</span></span>|
|<span data-ttu-id="9113a-121">*Repliky*</span><span class="sxs-lookup"><span data-stu-id="9113a-121">*Replicas*</span></span> | <span data-ttu-id="9113a-122">Instance služby vyhledávání, používají primárně k operace dotazů Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="9113a-122">Instances of the search service, used primarily to load balance query operations.</span></span> <span data-ttu-id="9113a-123">Každou repliku vždy hostuje jednu kopii indexu.</span><span class="sxs-lookup"><span data-stu-id="9113a-123">Each replica always hosts one copy of an index.</span></span> <span data-ttu-id="9113a-124">Pokud máte 12 repliky, budete mít 12 kopie každý index načíst služby.</span><span class="sxs-lookup"><span data-stu-id="9113a-124">If you have 12 replicas, you will have 12 copies of every index loaded on the service.</span></span>|

> [!NOTE]
> <span data-ttu-id="9113a-125">Neexistuje žádný způsob, jak přímo pracovat s nebo spravovat, které indexy spustit v replice.</span><span class="sxs-lookup"><span data-stu-id="9113a-125">There is no way to directly manipulate or manage which indexes run on a replica.</span></span> <span data-ttu-id="9113a-126">Jednu kopii každého index v každé repliky je součástí architektury služby.</span><span class="sxs-lookup"><span data-stu-id="9113a-126">One copy of each index on every replica is part of the service architecture.</span></span>
>

## <a name="how-to-allocate-partitions-and-replicas"></a><span data-ttu-id="9113a-127">Postup přidělení replik a oddíly</span><span class="sxs-lookup"><span data-stu-id="9113a-127">How to allocate partitions and replicas</span></span>
<span data-ttu-id="9113a-128">Ve službě Azure Search je služba původně přidělena minimální úroveň prostředků, který se skládá z jednoho oddílu a jednu repliku.</span><span class="sxs-lookup"><span data-stu-id="9113a-128">In Azure Search, a service is initially allocated a minimal level of resources consisting of one partition and one replica.</span></span> <span data-ttu-id="9113a-129">Pro vrstvy, které ji podporují můžete upravit přírůstkově výpočetní prostředky zvýšením oddílů, pokud potřebujete další úložiště a vstupně-výstupních operací, nebo přidat další repliky větší svazky dotazu nebo lepší výkon.</span><span class="sxs-lookup"><span data-stu-id="9113a-129">For tiers that support it, you can incrementally adjust computational resources by increasing partitions if you need more storage and I/O, or add more replicas for larger query volumes or better performance.</span></span> <span data-ttu-id="9113a-130">Služba jednotného musí mít dostatek prostředků ke zpracování všech úloh (indexování a dotazy).</span><span class="sxs-lookup"><span data-stu-id="9113a-130">A single service must have sufficient resources to handle all workloads (indexing and queries).</span></span> <span data-ttu-id="9113a-131">Nelze rozdělit zatížení mezi více služeb.</span><span class="sxs-lookup"><span data-stu-id="9113a-131">You cannot subdivide workloads among multiple services.</span></span>

<span data-ttu-id="9113a-132">Chcete-li zvýšit nebo změnit přidělení repliky a oddíly, doporučujeme pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="9113a-132">To increase or change the allocation of replicas and partitions, we recommend using the Azure portal.</span></span> <span data-ttu-id="9113a-133">Na portálu vynucuje omezení povolené kombinace, které zůstat nižší než maximální limit:</span><span class="sxs-lookup"><span data-stu-id="9113a-133">The portal enforces limits on allowable combinations that stay below maximum limits:</span></span>

1. <span data-ttu-id="9113a-134">Přihlaste se k [portál Azure](https://portal.azure.com/) a vyberte službu vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="9113a-134">Sign in to the [Azure portal](https://portal.azure.com/) and select the search service.</span></span>
2. <span data-ttu-id="9113a-135">V **nastavení**, otevřete **škálování** okno a pomocí posuvníků zvýšení nebo snížení počtu oddílů a repliky.</span><span class="sxs-lookup"><span data-stu-id="9113a-135">In **Settings**, open the **Scale** blade and use the sliders to increase or decrease the number of partitions and replicas.</span></span>

<span data-ttu-id="9113a-136">Pokud budete potřebovat založených na skriptech nebo založené na kódu zřizování přístup, [REST API pro správu](https://msdn.microsoft.com/library/azure/dn832687.aspx) je alternativa k portálu.</span><span class="sxs-lookup"><span data-stu-id="9113a-136">If you require a script-based or code-based provisioning approach, the [Management REST API](https://msdn.microsoft.com/library/azure/dn832687.aspx) is an alternative to the portal.</span></span>

<span data-ttu-id="9113a-137">Obecně platí hledání aplikace potřebují další repliky než oddíly, zvláště pokud operací služby jsou upřednostněno dotazu úlohy.</span><span class="sxs-lookup"><span data-stu-id="9113a-137">Generally, search applications need more replicas than partitions, particularly when the service operations are biased toward query workloads.</span></span> <span data-ttu-id="9113a-138">V části na [vysokou dostupnost](#HA) vysvětluje, proč.</span><span class="sxs-lookup"><span data-stu-id="9113a-138">The section on [high availability](#HA) explains why.</span></span>

> [!NOTE]
> <span data-ttu-id="9113a-139">Po zřízení služby, nelze upgradovat na vyšší skladová položka.</span><span class="sxs-lookup"><span data-stu-id="9113a-139">After a service is provisioned, it cannot be upgraded to a higher SKU.</span></span> <span data-ttu-id="9113a-140">Musíte se k vytvoření vyhledávací službu v novou vrstvu a znovu načíst vaše indexy.</span><span class="sxs-lookup"><span data-stu-id="9113a-140">You will need to create a search service at the new tier and reload your indexes.</span></span> <span data-ttu-id="9113a-141">V tématu [vytvoření služby Azure Search na portálu](search-create-service-portal.md) o pomoc se zřizováním služby.</span><span class="sxs-lookup"><span data-stu-id="9113a-141">See [Create an Azure Search service in the portal](search-create-service-portal.md) for help with service provisioning.</span></span>
>
>

<a id="HA"></a>

## <a name="high-availability"></a><span data-ttu-id="9113a-142">Vysoká dostupnost</span><span class="sxs-lookup"><span data-stu-id="9113a-142">High availability</span></span>
<span data-ttu-id="9113a-143">Protože je snadné a relativně rychlé škálování, doporučujeme obvykle začínáte s jedním oddílem a jednu nebo dvě repliky a pak rozšiřování škálování využívajících jako dotaz svazky sestavení.</span><span class="sxs-lookup"><span data-stu-id="9113a-143">Because it's easy and relatively fast to scale up, we generally recommend that you start with one partition and one or two replicas, and then scale up as query volumes build.</span></span> <span data-ttu-id="9113a-144">Pro mnoho služeb na úrovně Basic nebo S1 poskytuje jeden oddíl dostatečné úložiště a vstupně-výstupní operace (v 15 milionů dokumentů na oddíl).</span><span class="sxs-lookup"><span data-stu-id="9113a-144">For many services at the Basic or S1 tiers, one partition provides sufficient storage and I/O (at 15 million documents per partition).</span></span>

<span data-ttu-id="9113a-145">Spuštěné úlohy dotazu především na replikách.</span><span class="sxs-lookup"><span data-stu-id="9113a-145">Query workloads run primarily on replicas.</span></span> <span data-ttu-id="9113a-146">Pokud potřebujete další propustnost nebo vysokou dostupnost, bude pravděpodobně vyžadovat další repliky.</span><span class="sxs-lookup"><span data-stu-id="9113a-146">If you need more throughput or high availability, you will probably require additional replicas.</span></span>

<span data-ttu-id="9113a-147">Obecná doporučení pro zajištění vysoké dostupnosti jsou:</span><span class="sxs-lookup"><span data-stu-id="9113a-147">General recommendations for high availability are:</span></span>

* <span data-ttu-id="9113a-148">Dvě repliky pro vysokou dostupnost úloh jen pro čtení (dotazy)</span><span class="sxs-lookup"><span data-stu-id="9113a-148">Two replicas for high availability of read-only workloads (queries)</span></span>
* <span data-ttu-id="9113a-149">Tři nebo více replik pro vysokou dostupnost úloh pro čtení a zápis (dotazy a indexování přidat, aktualizovat ani odstranit jednotlivé dokumenty)</span><span class="sxs-lookup"><span data-stu-id="9113a-149">Three or more replicas for high availability of read/write workloads (queries plus indexing as individual documents are added, updated, or deleted)</span></span>

<span data-ttu-id="9113a-150">Smlouvy o úrovni služeb (SLA) pro službu Azure Search cílí v operacích dotazu a aktualizace indexu, které se skládají z přidání, aktualizace nebo odstranění dokumentů.</span><span class="sxs-lookup"><span data-stu-id="9113a-150">Service level agreements (SLA) for Azure Search are targeted at query operations and at index updates that consist of adding, updating, or deleting documents.</span></span>

### <a name="index-availability-during-a-rebuild"></a><span data-ttu-id="9113a-151">Index dostupnosti při opětovném sestavení</span><span class="sxs-lookup"><span data-stu-id="9113a-151">Index availability during a rebuild</span></span>

<span data-ttu-id="9113a-152">Vysoké dostupnosti pro službu Azure Search se vztahují na dotazy a index aktualizace, které není zahrnují nové sestavení indexu.</span><span class="sxs-lookup"><span data-stu-id="9113a-152">High availability for Azure Search pertains to queries and index updates that don't involve rebuilding an index.</span></span> <span data-ttu-id="9113a-153">Pokud odstraníte pole, změnit datový typ nebo název pole, musíte znovu sestavte index.</span><span class="sxs-lookup"><span data-stu-id="9113a-153">If you delete a field, change a data type, or rename a field, you will need to rebuild the index.</span></span> <span data-ttu-id="9113a-154">Nové sestavení indexu, musíte odstranit index, znovu vytvořte index a znovu načíst data.</span><span class="sxs-lookup"><span data-stu-id="9113a-154">To rebuild the index, you must delete the index, re-create the index, and reload the data.</span></span>

> [!NOTE]
> <span data-ttu-id="9113a-155">Můžete přidat nové pole do indexu Azure Search bez znovu sestavit index.</span><span class="sxs-lookup"><span data-stu-id="9113a-155">You can add new fields to an Azure Search index without rebuilding the index.</span></span> <span data-ttu-id="9113a-156">Hodnota nové pole bude mít hodnotu null pro všechny dokumenty již v indexu.</span><span class="sxs-lookup"><span data-stu-id="9113a-156">The value of the new field will be null for all documents already in the index.</span></span>

<span data-ttu-id="9113a-157">Údržbu index dostupnosti při opětovném sestavení, musí mít kopii index s jiným názvem na stejnou službu nebo kopii index se stejným názvem na jinou službu a pak zadejte přesměrování nebo logika převzetí služeb při selhání ve vašem kódu.</span><span class="sxs-lookup"><span data-stu-id="9113a-157">To maintain index availability during a rebuild, you must have a copy of the index with a different name on the same service, or a copy of the index with the same name on a different service, and then provide redirection or failover logic in your code.</span></span>

## <a name="disaster-recovery"></a><span data-ttu-id="9113a-158">Zotavení po havárii</span><span class="sxs-lookup"><span data-stu-id="9113a-158">Disaster recovery</span></span>
<span data-ttu-id="9113a-159">V současné době není k dispozici žádné předdefinované mechanismus pro zotavení po havárii.</span><span class="sxs-lookup"><span data-stu-id="9113a-159">Currently, there is no built-in mechanism for disaster recovery.</span></span> <span data-ttu-id="9113a-160">Přidání oddílů nebo repliky může být nesprávný strategie pro splnění cílů obnovení po havárii.</span><span class="sxs-lookup"><span data-stu-id="9113a-160">Adding partitions or replicas would be the wrong strategy for meeting disaster recovery objectives.</span></span> <span data-ttu-id="9113a-161">Nejběžnějším přístupem je přidání redundance na úrovni služby nastavením druhý vyhledávací službu v jiné oblasti.</span><span class="sxs-lookup"><span data-stu-id="9113a-161">The most common approach is to add redundancy at the service level by setting up a second search service in another region.</span></span> <span data-ttu-id="9113a-162">Stejně jako u dostupnosti během opětovné sestavení indexu přesměrování nebo logika převzetí služeb při selhání musí pocházet z vašeho kódu.</span><span class="sxs-lookup"><span data-stu-id="9113a-162">As with availability during an index rebuild, the redirection or failover logic must come from your code.</span></span>

## <a name="increase-query-performance-with-replicas"></a><span data-ttu-id="9113a-163">Zvýšení výkonu dotazů s replikami</span><span class="sxs-lookup"><span data-stu-id="9113a-163">Increase query performance with replicas</span></span>
<span data-ttu-id="9113a-164">Latence dotazu je indikátorem, jsou potřeba další repliky.</span><span class="sxs-lookup"><span data-stu-id="9113a-164">Query latency is an indicator that additional replicas are needed.</span></span> <span data-ttu-id="9113a-165">Obecně platí je prvním krokem k zlepšení výkonu dotazů, chcete-li přidat více tento prostředek.</span><span class="sxs-lookup"><span data-stu-id="9113a-165">Generally, a first step toward improving query performance is to add more of this resource.</span></span> <span data-ttu-id="9113a-166">Při přidávání repliky, další kopie index znovu online podporujících větší zatížení dotazu a načíst vyvážit požadavky přes víc replik.</span><span class="sxs-lookup"><span data-stu-id="9113a-166">As you add replicas, additional copies of the index are brought online to support bigger query workloads and to load balance the requests over the multiple replicas.</span></span>

<span data-ttu-id="9113a-167">Nemůžeme poskytovat pevný odhady na dotazy za sekundu (QPS): dotaz výkon závisí na složitosti dotazu a konkurenční úlohy.</span><span class="sxs-lookup"><span data-stu-id="9113a-167">We cannot provide hard estimates on queries per second (QPS): query performance depends on the complexity of the query and competing workloads.</span></span> <span data-ttu-id="9113a-168">V průměru repliku na základní nebo S1 SKU může obsluhovat o 15 QPS, ale vaše propustnost bude vyšší nebo nižší v závislosti na složitosti dotazu (Fasetové dotazy jsou složitější) a latence sítě.</span><span class="sxs-lookup"><span data-stu-id="9113a-168">On average, a replica at Basic or S1 SKUs can service about 15 QPS, but your throughput will be higher or lower depending on query complexity (faceted queries are more complex) and network latency.</span></span> <span data-ttu-id="9113a-169">Je také důležité vědět, že i když přidání repliky výborný přidá rozsah a výkon, výsledek není výhradně lineární: Přidání tři repliky nezaručuje Trojitá propustnost.</span><span class="sxs-lookup"><span data-stu-id="9113a-169">Also, it's important to recognize that although adding replicas will definitely add scale and performance, the result is not strictly linear: adding three replicas does not guarantee triple throughput.</span></span>

<span data-ttu-id="9113a-170">Další informace o QPS, včetně přístupy k odhadování QPS pro zatížení, najdete v části [Správa služby Search](search-manage.md).</span><span class="sxs-lookup"><span data-stu-id="9113a-170">To learn about QPS, including approaches for estimating QPS for your workloads, see [Manage your Search service](search-manage.md).</span></span>

## <a name="increase-indexing-performance-with-partitions"></a><span data-ttu-id="9113a-171">Zvýšení výkonu indexování s oddíly</span><span class="sxs-lookup"><span data-stu-id="9113a-171">Increase indexing performance with partitions</span></span>
<span data-ttu-id="9113a-172">Vyhledávání aplikací, které vyžadují téměř aktualizace dat v reálném čase, bude nutné úměrně další oddíly než repliky.</span><span class="sxs-lookup"><span data-stu-id="9113a-172">Search applications that require near real-time data refresh will need proportionally more partitions than replicas.</span></span> <span data-ttu-id="9113a-173">Přidání oddílů šíří operace čtení a zápisu napříč větší počet výpočetní prostředky.</span><span class="sxs-lookup"><span data-stu-id="9113a-173">Adding partitions spreads read/write operations across a larger number of compute resources.</span></span> <span data-ttu-id="9113a-174">Nabízí také více místa na disku pro uložení další indexy a dokumenty.</span><span class="sxs-lookup"><span data-stu-id="9113a-174">It also gives you more disk space for storing additional indexes and documents.</span></span>

<span data-ttu-id="9113a-175">Indexy větší trvat déle dotazu.</span><span class="sxs-lookup"><span data-stu-id="9113a-175">Larger indexes take longer to query.</span></span> <span data-ttu-id="9113a-176">Jako takový je možné, že každý další nárůst oddíly vyžaduje menší, ale přímo úměrná zvýšení replik.</span><span class="sxs-lookup"><span data-stu-id="9113a-176">As such, you might find that every incremental increase in partitions requires a smaller but proportional increase in replicas.</span></span> <span data-ttu-id="9113a-177">Složitost dotazy a dotaz svazek bude dvoufaktorového do jak rychle provádění dotazu zapnutý,.</span><span class="sxs-lookup"><span data-stu-id="9113a-177">The complexity of your queries and query volume will factor into how quickly query execution is turned around.</span></span>

## <a name="basic-tier-partition-and-replica-combinations"></a><span data-ttu-id="9113a-178">Základní úroveň: kombinace parametrů oddílu a repliky</span><span class="sxs-lookup"><span data-stu-id="9113a-178">Basic tier: Partition and replica combinations</span></span>
<span data-ttu-id="9113a-179">Základní služby může mít přesně jeden oddíl a až tři repliky, maximum omezit tři služby SUS.</span><span class="sxs-lookup"><span data-stu-id="9113a-179">A Basic service can have exactly one partition and up to three replicas, for a maximum limit of three SUs.</span></span> <span data-ttu-id="9113a-180">Pouze upravit prostředek je repliky.</span><span class="sxs-lookup"><span data-stu-id="9113a-180">The only adjustable resource is replicas.</span></span> <span data-ttu-id="9113a-181">Pro zajištění vysoké dostupnosti u dotazů na potřebujete minimálně dvě repliky.</span><span class="sxs-lookup"><span data-stu-id="9113a-181">You need a minimum of two replicas for high availability on queries.</span></span>

<a id="chart"></a>

## <a name="standard-tiers-partition-and-replica-combinations"></a><span data-ttu-id="9113a-182">Standardní vrstev: kombinace parametrů oddílu a repliky</span><span class="sxs-lookup"><span data-stu-id="9113a-182">Standard tiers: Partition and replica combinations</span></span>
<span data-ttu-id="9113a-183">Tato tabulka ukazuje služby SUs potřebné k podpoře kombinace repliky a oddíly, závislosti limitu 36 SU pro všechny standardní úrovně.</span><span class="sxs-lookup"><span data-stu-id="9113a-183">This table shows the SUs required to support combinations of replicas and partitions, subject to the 36-SU limit, for all Standard tiers.</span></span>

|   | <span data-ttu-id="9113a-184">**oddíl 1**</span><span class="sxs-lookup"><span data-stu-id="9113a-184">**1 partition**</span></span> | <span data-ttu-id="9113a-185">**2 oddíly**</span><span class="sxs-lookup"><span data-stu-id="9113a-185">**2 partitions**</span></span> | <span data-ttu-id="9113a-186">**3 oddíly**</span><span class="sxs-lookup"><span data-stu-id="9113a-186">**3 partitions**</span></span> | <span data-ttu-id="9113a-187">**4 oddíly**</span><span class="sxs-lookup"><span data-stu-id="9113a-187">**4 partitions**</span></span> | <span data-ttu-id="9113a-188">**6 oddíly**</span><span class="sxs-lookup"><span data-stu-id="9113a-188">**6 partitions**</span></span> | <span data-ttu-id="9113a-189">**12 oddíly**</span><span class="sxs-lookup"><span data-stu-id="9113a-189">**12 partitions**</span></span> |
| --- | --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="9113a-190">**1 repliky**</span><span class="sxs-lookup"><span data-stu-id="9113a-190">**1 replica**</span></span> |<span data-ttu-id="9113a-191">1 SU</span><span class="sxs-lookup"><span data-stu-id="9113a-191">1 SU</span></span> |<span data-ttu-id="9113a-192">2 SU</span><span class="sxs-lookup"><span data-stu-id="9113a-192">2 SU</span></span> |<span data-ttu-id="9113a-193">3 SU</span><span class="sxs-lookup"><span data-stu-id="9113a-193">3 SU</span></span> |<span data-ttu-id="9113a-194">4 SU</span><span class="sxs-lookup"><span data-stu-id="9113a-194">4 SU</span></span> |<span data-ttu-id="9113a-195">6 SU</span><span class="sxs-lookup"><span data-stu-id="9113a-195">6 SU</span></span> |<span data-ttu-id="9113a-196">12 SU</span><span class="sxs-lookup"><span data-stu-id="9113a-196">12 SU</span></span> |
| <span data-ttu-id="9113a-197">**2 repliky**</span><span class="sxs-lookup"><span data-stu-id="9113a-197">**2 replicas**</span></span> |<span data-ttu-id="9113a-198">2 SU</span><span class="sxs-lookup"><span data-stu-id="9113a-198">2 SU</span></span> |<span data-ttu-id="9113a-199">4 SU</span><span class="sxs-lookup"><span data-stu-id="9113a-199">4 SU</span></span> |<span data-ttu-id="9113a-200">6 SU</span><span class="sxs-lookup"><span data-stu-id="9113a-200">6 SU</span></span> |<span data-ttu-id="9113a-201">8 SU</span><span class="sxs-lookup"><span data-stu-id="9113a-201">8 SU</span></span> |<span data-ttu-id="9113a-202">12 SU</span><span class="sxs-lookup"><span data-stu-id="9113a-202">12 SU</span></span> |<span data-ttu-id="9113a-203">24 SU</span><span class="sxs-lookup"><span data-stu-id="9113a-203">24 SU</span></span> |
| <span data-ttu-id="9113a-204">**3 repliky**</span><span class="sxs-lookup"><span data-stu-id="9113a-204">**3 replicas**</span></span> |<span data-ttu-id="9113a-205">3 SU</span><span class="sxs-lookup"><span data-stu-id="9113a-205">3 SU</span></span> |<span data-ttu-id="9113a-206">6 SU</span><span class="sxs-lookup"><span data-stu-id="9113a-206">6 SU</span></span> |<span data-ttu-id="9113a-207">9 SU</span><span class="sxs-lookup"><span data-stu-id="9113a-207">9 SU</span></span> |<span data-ttu-id="9113a-208">12 SU</span><span class="sxs-lookup"><span data-stu-id="9113a-208">12 SU</span></span> |<span data-ttu-id="9113a-209">18 SU</span><span class="sxs-lookup"><span data-stu-id="9113a-209">18 SU</span></span> |<span data-ttu-id="9113a-210">36 SU</span><span class="sxs-lookup"><span data-stu-id="9113a-210">36 SU</span></span> |
| <span data-ttu-id="9113a-211">**4 repliky**</span><span class="sxs-lookup"><span data-stu-id="9113a-211">**4 replicas**</span></span> |<span data-ttu-id="9113a-212">4 SU</span><span class="sxs-lookup"><span data-stu-id="9113a-212">4 SU</span></span> |<span data-ttu-id="9113a-213">8 SU</span><span class="sxs-lookup"><span data-stu-id="9113a-213">8 SU</span></span> |<span data-ttu-id="9113a-214">12 SU</span><span class="sxs-lookup"><span data-stu-id="9113a-214">12 SU</span></span> |<span data-ttu-id="9113a-215">16 SU</span><span class="sxs-lookup"><span data-stu-id="9113a-215">16 SU</span></span> |<span data-ttu-id="9113a-216">24 SU</span><span class="sxs-lookup"><span data-stu-id="9113a-216">24 SU</span></span> |<span data-ttu-id="9113a-217">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="9113a-217">N/A</span></span> |
| <span data-ttu-id="9113a-218">**5 repliky**</span><span class="sxs-lookup"><span data-stu-id="9113a-218">**5 replicas**</span></span> |<span data-ttu-id="9113a-219">5 SU</span><span class="sxs-lookup"><span data-stu-id="9113a-219">5 SU</span></span> |<span data-ttu-id="9113a-220">10 SU</span><span class="sxs-lookup"><span data-stu-id="9113a-220">10 SU</span></span> |<span data-ttu-id="9113a-221">15 SU</span><span class="sxs-lookup"><span data-stu-id="9113a-221">15 SU</span></span> |<span data-ttu-id="9113a-222">20 SU</span><span class="sxs-lookup"><span data-stu-id="9113a-222">20 SU</span></span> |<span data-ttu-id="9113a-223">30 SU</span><span class="sxs-lookup"><span data-stu-id="9113a-223">30 SU</span></span> |<span data-ttu-id="9113a-224">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="9113a-224">N/A</span></span> |
| <span data-ttu-id="9113a-225">**6 repliky**</span><span class="sxs-lookup"><span data-stu-id="9113a-225">**6 replicas**</span></span> |<span data-ttu-id="9113a-226">6 SU</span><span class="sxs-lookup"><span data-stu-id="9113a-226">6 SU</span></span> |<span data-ttu-id="9113a-227">12 SU</span><span class="sxs-lookup"><span data-stu-id="9113a-227">12 SU</span></span> |<span data-ttu-id="9113a-228">18 SU</span><span class="sxs-lookup"><span data-stu-id="9113a-228">18 SU</span></span> |<span data-ttu-id="9113a-229">24 SU</span><span class="sxs-lookup"><span data-stu-id="9113a-229">24 SU</span></span> |<span data-ttu-id="9113a-230">36 SU</span><span class="sxs-lookup"><span data-stu-id="9113a-230">36 SU</span></span> |<span data-ttu-id="9113a-231">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="9113a-231">N/A</span></span> |
| <span data-ttu-id="9113a-232">**12 repliky**</span><span class="sxs-lookup"><span data-stu-id="9113a-232">**12 replicas**</span></span> |<span data-ttu-id="9113a-233">12 SU</span><span class="sxs-lookup"><span data-stu-id="9113a-233">12 SU</span></span> |<span data-ttu-id="9113a-234">24 SU</span><span class="sxs-lookup"><span data-stu-id="9113a-234">24 SU</span></span> |<span data-ttu-id="9113a-235">36 SU</span><span class="sxs-lookup"><span data-stu-id="9113a-235">36 SU</span></span> |<span data-ttu-id="9113a-236">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="9113a-236">N/A</span></span> |<span data-ttu-id="9113a-237">Není dostupné.</span><span class="sxs-lookup"><span data-stu-id="9113a-237">N/A</span></span> |<span data-ttu-id="9113a-238">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="9113a-238">N/A</span></span> |

<span data-ttu-id="9113a-239">Služba SUs, ceny a kapacity jsou podrobně vysvětleny na webu Azure.</span><span class="sxs-lookup"><span data-stu-id="9113a-239">SUs, pricing, and capacity are explained in detail on the Azure website.</span></span> <span data-ttu-id="9113a-240">Další informace najdete v tématu [podrobnosti o cenách](https://azure.microsoft.com/pricing/details/search/).</span><span class="sxs-lookup"><span data-stu-id="9113a-240">For more information, see [Pricing Details](https://azure.microsoft.com/pricing/details/search/).</span></span>

> [!NOTE]
> <span data-ttu-id="9113a-241">Počet replik a oddíly, které jsou rovnoměrně rozděleny do 12 (konkrétně 1, 2, 3, 4, 6, 12).</span><span class="sxs-lookup"><span data-stu-id="9113a-241">The number of replicas and partitions divides evenly into 12 (specifically, 1, 2, 3, 4, 6, 12).</span></span> <span data-ttu-id="9113a-242">Je to proto, že každý index Azure Search předem rozdělí na 12 horizontálních oddílů, takže je možné rozdělit do stejné části pro všechny oddíly.</span><span class="sxs-lookup"><span data-stu-id="9113a-242">This is because Azure Search pre-divides each index into 12 shards so that it can be spread in equal portions across all partitions.</span></span> <span data-ttu-id="9113a-243">Například pokud vaše služba má tři oddíly a vytvoření indexu, každý oddíl bude obsahovat čtyři horizontálních oddílů indexu.</span><span class="sxs-lookup"><span data-stu-id="9113a-243">For example, if your service has three partitions and you create an index, each partition will contain four shards of the index.</span></span> <span data-ttu-id="9113a-244">Jak Azure Search horizontálních oddílů na index je podrobností implementace, v budoucích vezích se můžou změnit.</span><span class="sxs-lookup"><span data-stu-id="9113a-244">How Azure Search shards an index is an implementation detail, subject to change in future releases.</span></span> <span data-ttu-id="9113a-245">I když číslo 12 dnes, není pravděpodobné, že toto číslo vždycky být 12 v budoucnu.</span><span class="sxs-lookup"><span data-stu-id="9113a-245">Although the number is 12 today, you shouldn't expect that number to always be 12 in the future.</span></span>
>
>

## <a name="billing-formula-for-replica-and-partition-resources"></a><span data-ttu-id="9113a-246">Vzorec fakturace pro repliky a oddíl prostředky</span><span class="sxs-lookup"><span data-stu-id="9113a-246">Billing formula for replica and partition resources</span></span>
<span data-ttu-id="9113a-247">Vzorec pro výpočet, kolik služby SUs se používají pro konkrétní kombinace je produkt repliky a oddíly, nebo (R X P = SU).</span><span class="sxs-lookup"><span data-stu-id="9113a-247">The formula for calculating how many SUs are used for specific combinations is the product of replicas and partitions, or (R X P = SU).</span></span> <span data-ttu-id="9113a-248">Například se jako devět služby SUs fakturuje tři repliky násobí hodnotou tři oddíly.</span><span class="sxs-lookup"><span data-stu-id="9113a-248">For example, three replicas multiplied by three partitions is billed as nine SUs.</span></span>

<span data-ttu-id="9113a-249">Cena za SU, je dáno úroveň, s nižší sazbou za jednotku fakturace pro základní než pro Standard.</span><span class="sxs-lookup"><span data-stu-id="9113a-249">Cost per SU is determined by the tier, with a lower per-unit billing rate for Basic than for Standard.</span></span> <span data-ttu-id="9113a-250">Vyhodnotí pro každou vrstvu lze najít v [podrobnosti o cenách](https://azure.microsoft.com/pricing/details/search/).</span><span class="sxs-lookup"><span data-stu-id="9113a-250">Rates for each tier can be found on [Pricing Details](https://azure.microsoft.com/pricing/details/search/).</span></span>