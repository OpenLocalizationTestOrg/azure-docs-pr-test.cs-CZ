---
title: "Návrh a implementaci databáze Oracle na platformě Azure | Microsoft Docs"
description: "Návrh a implementaci k databázi Oracle v prostředí Azure."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: v-shiuma
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 6/22/2017
ms.author: rclaus
ms.openlocfilehash: 1af7e1d40a0eb129875dd6a30ac899f2025bee13
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="design-and-implement-an-oracle-database-in-azure"></a><span data-ttu-id="08be6-103">Návrh a implementaci k databázi Oracle v Azure</span><span class="sxs-lookup"><span data-stu-id="08be6-103">Design and implement an Oracle database in Azure</span></span>

## <a name="assumptions"></a><span data-ttu-id="08be6-104">Předpoklady</span><span class="sxs-lookup"><span data-stu-id="08be6-104">Assumptions</span></span>

- <span data-ttu-id="08be6-105">Plánování migrace z místní databáze Oracle do Azure.</span><span class="sxs-lookup"><span data-stu-id="08be6-105">You're planning to migrate an Oracle database from on-premises to Azure.</span></span>
- <span data-ttu-id="08be6-106">Máte představu o různé metriky v sestavách Oracle AWR.</span><span class="sxs-lookup"><span data-stu-id="08be6-106">You have an understanding of the various metrics in Oracle AWR reports.</span></span>
- <span data-ttu-id="08be6-107">Máte základní znalosti aplikace výkonu a využití platformy.</span><span class="sxs-lookup"><span data-stu-id="08be6-107">You have a baseline understanding of application performance and platform utilization.</span></span>

## <a name="goals"></a><span data-ttu-id="08be6-108">Cíle</span><span class="sxs-lookup"><span data-stu-id="08be6-108">Goals</span></span>

- <span data-ttu-id="08be6-109">Pochopit, jak optimalizovat Oracle nasazení v Azure.</span><span class="sxs-lookup"><span data-stu-id="08be6-109">Understand how to optimize your Oracle deployment in Azure.</span></span>
- <span data-ttu-id="08be6-110">Prozkoumejte možností pro databázi Oracle v prostředí Azure ladění výkonu.</span><span class="sxs-lookup"><span data-stu-id="08be6-110">Explore performance tuning options for an Oracle database in an Azure environment.</span></span>

## <a name="the-differences-between-an-on-premises-and-azure-implementation"></a><span data-ttu-id="08be6-111">Rozdíly mezi místním a Azure implementace</span><span class="sxs-lookup"><span data-stu-id="08be6-111">The differences between an on-premises and Azure implementation</span></span> 

<span data-ttu-id="08be6-112">Toto jsou některé důležité věci, třeba mít na paměti, když jste migrace místní aplikace do Azure.</span><span class="sxs-lookup"><span data-stu-id="08be6-112">Following are some important things to keep in mind when you're migrating on-premises applications to Azure.</span></span> 

<span data-ttu-id="08be6-113">Jeden důležitý rozdíl je, že v implementaci Azure prostředkům, například virtuálních počítačů, disků a virtuální sítě sdílejí mezi ostatní klienty.</span><span class="sxs-lookup"><span data-stu-id="08be6-113">One important difference is that in an Azure implementation, resources such as VMs, disks, and virtual networks are shared among other clients.</span></span> <span data-ttu-id="08be6-114">Kromě toho prostředky můžete omezeny na základě požadavků.</span><span class="sxs-lookup"><span data-stu-id="08be6-114">In addition, resources can be throttled based on the requirements.</span></span> <span data-ttu-id="08be6-115">Místo zaměřené na zamezení selhání provozu (MTBF), Azure zaměřují na zbývajících selhání (MTTR).</span><span class="sxs-lookup"><span data-stu-id="08be6-115">Instead of focusing on avoiding failing (MTBF), Azure is more focused on surviving the failure (MTTR).</span></span>

<span data-ttu-id="08be6-116">Následující tabulka uvádí některé rozdíly mezi místními implementace a implementaci Azure pro databázi Oracle.</span><span class="sxs-lookup"><span data-stu-id="08be6-116">The following table lists some of the differences between an on-premises implementation and an Azure implementation of an Oracle database.</span></span>

> 
> |  | <span data-ttu-id="08be6-117">**Místní implementace**</span><span class="sxs-lookup"><span data-stu-id="08be6-117">**On-premises implementation**</span></span> | <span data-ttu-id="08be6-118">**Azure implementace**</span><span class="sxs-lookup"><span data-stu-id="08be6-118">**Azure implementation**</span></span> |
> | --- | --- | --- |
> | <span data-ttu-id="08be6-119">**Sítě**</span><span class="sxs-lookup"><span data-stu-id="08be6-119">**Networking**</span></span> |<span data-ttu-id="08be6-120">LAN NEBO WAN</span><span class="sxs-lookup"><span data-stu-id="08be6-120">LAN/WAN</span></span>  |<span data-ttu-id="08be6-121">SDN (softwarově definované sítě)</span><span class="sxs-lookup"><span data-stu-id="08be6-121">SDN (software-defined networking)</span></span>|
> | <span data-ttu-id="08be6-122">**Skupina zabezpečení**</span><span class="sxs-lookup"><span data-stu-id="08be6-122">**Security group**</span></span> |<span data-ttu-id="08be6-123">Nástroje pro omezení adresy IP a portu</span><span class="sxs-lookup"><span data-stu-id="08be6-123">IP/port restriction tools</span></span> |[<span data-ttu-id="08be6-124">Skupina zabezpečení sítě (NSG)</span><span class="sxs-lookup"><span data-stu-id="08be6-124">Network Security Group (NSG)</span></span>](https://azure.microsoft.com/blog/network-security-groups) |
> | <span data-ttu-id="08be6-125">**Odolnost proti**</span><span class="sxs-lookup"><span data-stu-id="08be6-125">**Resilience**</span></span> |<span data-ttu-id="08be6-126">MTBF (střední čas mezi selhání)</span><span class="sxs-lookup"><span data-stu-id="08be6-126">MTBF (mean time between failures)</span></span> |<span data-ttu-id="08be6-127">MTTR (střední čas k obnovení)</span><span class="sxs-lookup"><span data-stu-id="08be6-127">MTTR (mean time to recovery)</span></span>|
> | <span data-ttu-id="08be6-128">**Plánovaná údržba**</span><span class="sxs-lookup"><span data-stu-id="08be6-128">**Planned maintenance**</span></span> |<span data-ttu-id="08be6-129">Opravy chyb a upgrady</span><span class="sxs-lookup"><span data-stu-id="08be6-129">Patching/upgrades</span></span>|<span data-ttu-id="08be6-130">[Skupiny dostupnosti](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-availability-sets-guidelines) (opravy a upgrady spravovat přes Azure)</span><span class="sxs-lookup"><span data-stu-id="08be6-130">[Availability sets](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-availability-sets-guidelines) (patching/upgrades managed by Azure)</span></span> |
> | <span data-ttu-id="08be6-131">**Prostředek**</span><span class="sxs-lookup"><span data-stu-id="08be6-131">**Resource**</span></span> |<span data-ttu-id="08be6-132">Vyhrazený</span><span class="sxs-lookup"><span data-stu-id="08be6-132">Dedicated</span></span>  |<span data-ttu-id="08be6-133">Sdílet s ostatními klienty</span><span class="sxs-lookup"><span data-stu-id="08be6-133">Shared with other clients</span></span>|
> | <span data-ttu-id="08be6-134">**Oblasti**</span><span class="sxs-lookup"><span data-stu-id="08be6-134">**Regions**</span></span> |<span data-ttu-id="08be6-135">Datová centra</span><span class="sxs-lookup"><span data-stu-id="08be6-135">Datacenters</span></span> |[<span data-ttu-id="08be6-136">Dvojice oblast</span><span class="sxs-lookup"><span data-stu-id="08be6-136">Region pairs</span></span>](https://docs.microsoft.com/azure/virtual-machines/windows/regions-and-availability)|
> | <span data-ttu-id="08be6-137">**Storage**</span><span class="sxs-lookup"><span data-stu-id="08be6-137">**Storage**</span></span> |<span data-ttu-id="08be6-138">Síť SAN nebo fyzické disky</span><span class="sxs-lookup"><span data-stu-id="08be6-138">SAN/physical disks</span></span> |[<span data-ttu-id="08be6-139">Spravovat Azure storage</span><span class="sxs-lookup"><span data-stu-id="08be6-139">Azure-managed storage</span></span>](https://azure.microsoft.com/pricing/details/managed-disks/?v=17.23h)|
> | <span data-ttu-id="08be6-140">**Škálování**</span><span class="sxs-lookup"><span data-stu-id="08be6-140">**Scale**</span></span> |<span data-ttu-id="08be6-141">Vertikální Škálováním</span><span class="sxs-lookup"><span data-stu-id="08be6-141">Vertical scale</span></span> |<span data-ttu-id="08be6-142">Horizontální škálování</span><span class="sxs-lookup"><span data-stu-id="08be6-142">Horizontal scale</span></span>|


### <a name="requirements"></a><span data-ttu-id="08be6-143">Požadavky</span><span class="sxs-lookup"><span data-stu-id="08be6-143">Requirements</span></span>

- <span data-ttu-id="08be6-144">Určete rychlost velikosti a nárůst databáze.</span><span class="sxs-lookup"><span data-stu-id="08be6-144">Determine the database size and growth rate.</span></span>
- <span data-ttu-id="08be6-145">Určete požadavky IOPS, které můžete odhadnout na základě Oracle AWR sestavy nebo jiné sítě, nástroje pro sledování.</span><span class="sxs-lookup"><span data-stu-id="08be6-145">Determine the IOPS requirements, which you can estimate based on Oracle AWR reports or other network monitoring tools.</span></span>

## <a name="configuration-options"></a><span data-ttu-id="08be6-146">Možnosti konfigurace</span><span class="sxs-lookup"><span data-stu-id="08be6-146">Configuration options</span></span>

<span data-ttu-id="08be6-147">Existují čtyři potenciálních oblastí, které můžete vyladit ke zlepšení výkonu prostředí Azure:</span><span class="sxs-lookup"><span data-stu-id="08be6-147">There are four potential areas that you can tune to improve performance in an Azure environment:</span></span>

- <span data-ttu-id="08be6-148">Velikost virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="08be6-148">Virtual machine size</span></span>
- <span data-ttu-id="08be6-149">Propustnost sítě</span><span class="sxs-lookup"><span data-stu-id="08be6-149">Network throughput</span></span>
- <span data-ttu-id="08be6-150">Typy disků a konfigurace</span><span class="sxs-lookup"><span data-stu-id="08be6-150">Disk types and configurations</span></span>
- <span data-ttu-id="08be6-151">Nastavení mezipaměti na disku</span><span class="sxs-lookup"><span data-stu-id="08be6-151">Disk cache settings</span></span>

### <a name="generate-an-awr-report"></a><span data-ttu-id="08be6-152">Generování sestavy AWR</span><span class="sxs-lookup"><span data-stu-id="08be6-152">Generate an AWR report</span></span>

<span data-ttu-id="08be6-153">Pokud máte existující databázi Oracle a plánování migrace do Azure, máte několik možností.</span><span class="sxs-lookup"><span data-stu-id="08be6-153">If you have an existing an Oracle database and are planning to migrate to Azure, you have several options.</span></span> <span data-ttu-id="08be6-154">Můžete spustit sestavy Oracle AWR metriky (IOPS, MB/s, GiBs a tak dále).</span><span class="sxs-lookup"><span data-stu-id="08be6-154">You can run the Oracle AWR report to get the metrics (IOPS, Mbps, GiBs, and so on).</span></span> <span data-ttu-id="08be6-155">Potom vyberte virtuální počítač podle metriky, které jste shromáždili.</span><span class="sxs-lookup"><span data-stu-id="08be6-155">Then choose the VM based on the metrics that you collected.</span></span> <span data-ttu-id="08be6-156">Nebo můžete kontaktujte tým infrastruktury podobné informace.</span><span class="sxs-lookup"><span data-stu-id="08be6-156">Or you can contact your infrastructure team to get similar information.</span></span>

<span data-ttu-id="08be6-157">Můžete zvážit spouštění sestavy AWR během pravidelné a špičkovým úlohy, takže můžete porovnat.</span><span class="sxs-lookup"><span data-stu-id="08be6-157">You might consider running your AWR report during both regular and peak workloads, so you can compare.</span></span> <span data-ttu-id="08be6-158">Na základě těchto zpráv, může Velikost virtuálních počítačů na základě průměrné zatížení nebo maximální zátěž.</span><span class="sxs-lookup"><span data-stu-id="08be6-158">Based on these reports, you can size the VMs based on either the average workload or the maximum workload.</span></span>

<span data-ttu-id="08be6-159">Tady je příklad toho, jak vygenerovat zprávu o AWR:</span><span class="sxs-lookup"><span data-stu-id="08be6-159">Following is an example of how to generate an AWR report:</span></span>

```bash
$ sqlplus / as sysdba
SQL> EXEC DBMS_WORKLOAD_REPOSITORY.CREATE_SNAPSHOT;
SQL> @?/rdbms/admin/awrrpt.sql
```

### <a name="key-metrics"></a><span data-ttu-id="08be6-160">Klíčové metriky</span><span class="sxs-lookup"><span data-stu-id="08be6-160">Key metrics</span></span>

<span data-ttu-id="08be6-161">Následují metriky, které můžete získat z AWR sestavy:</span><span class="sxs-lookup"><span data-stu-id="08be6-161">Following are the metrics that you can obtain from the AWR report:</span></span>

- <span data-ttu-id="08be6-162">Celkový počet jader</span><span class="sxs-lookup"><span data-stu-id="08be6-162">Total number of cores</span></span>
- <span data-ttu-id="08be6-163">Procesor o rychlosti</span><span class="sxs-lookup"><span data-stu-id="08be6-163">CPU clock speed</span></span>
- <span data-ttu-id="08be6-164">Celkové paměti v GB</span><span class="sxs-lookup"><span data-stu-id="08be6-164">Total memory in GB</span></span>
- <span data-ttu-id="08be6-165">Využití procesoru</span><span class="sxs-lookup"><span data-stu-id="08be6-165">CPU utilization</span></span>
- <span data-ttu-id="08be6-166">Rychlost přenosu dat ve špičce</span><span class="sxs-lookup"><span data-stu-id="08be6-166">Peak data transfer rate</span></span>
- <span data-ttu-id="08be6-167">Počet vstupně-výstupních operací změny (čtení a zápis)</span><span class="sxs-lookup"><span data-stu-id="08be6-167">Rate of I/O changes (read/write)</span></span>
- <span data-ttu-id="08be6-168">Vrátit protokolu rychlost (MB/s)</span><span class="sxs-lookup"><span data-stu-id="08be6-168">Redo log rate (MBPs)</span></span>
- <span data-ttu-id="08be6-169">Propustnost sítě</span><span class="sxs-lookup"><span data-stu-id="08be6-169">Network throughput</span></span>
- <span data-ttu-id="08be6-170">Míra latence sítě (dolní nebo horní)</span><span class="sxs-lookup"><span data-stu-id="08be6-170">Network latency rate (low/high)</span></span>
- <span data-ttu-id="08be6-171">Velikost databáze v GB</span><span class="sxs-lookup"><span data-stu-id="08be6-171">Database size in GB</span></span>
- <span data-ttu-id="08be6-172">Bajtů přijatých prostřednictvím SQL * Net z/do klienta</span><span class="sxs-lookup"><span data-stu-id="08be6-172">Bytes received via SQL*Net from/to client</span></span>

### <a name="virtual-machine-size"></a><span data-ttu-id="08be6-173">Velikost virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="08be6-173">Virtual machine size</span></span>

#### <a name="1-estimate-vm-size-based-on-cpu-memory-and-io-usage-from-the-awr-report"></a><span data-ttu-id="08be6-174">1. Odhad velikosti virtuálního počítače na základě využití procesoru, paměti a vstupu a výstupu ze sestavy AWR</span><span class="sxs-lookup"><span data-stu-id="08be6-174">1. Estimate VM size based on CPU, memory, and I/O usage from the AWR report</span></span>

<span data-ttu-id="08be6-175">Jednou z věcí, které může vypadat v je nejvyšší pět vypršel popředí události, které signalizují, kde jsou kritická místa systému.</span><span class="sxs-lookup"><span data-stu-id="08be6-175">One thing you might look at is the top five timed foreground events that indicate where the system bottlenecks are.</span></span>

<span data-ttu-id="08be6-176">V následujícím diagramu, například synchronizace souboru protokolu je v horní části.</span><span class="sxs-lookup"><span data-stu-id="08be6-176">For example, in the following diagram, the log file sync is at the top.</span></span> <span data-ttu-id="08be6-177">Znamená počet počká, které jsou požadovány, než LGWR zapíše protokolu vyrovnávací paměti do souboru protokolu operaci znovu.</span><span class="sxs-lookup"><span data-stu-id="08be6-177">It indicates the number of waits that are required before the LGWR writes the log buffer to the redo log file.</span></span> <span data-ttu-id="08be6-178">Tyto výsledky naznačují, že jsou lépe provádění úložiště nebo disky.</span><span class="sxs-lookup"><span data-stu-id="08be6-178">These results indicate that better performing storage or disks are required.</span></span> <span data-ttu-id="08be6-179">Diagram navíc také zobrazuje počet procesor (jádra) a velikost paměti.</span><span class="sxs-lookup"><span data-stu-id="08be6-179">In addition, the diagram also shows the number of CPU (cores) and the amount of memory.</span></span>

![Snímek obrazovky stránky sestavy AWR](./media/oracle-design/cpu_memory_info.png)

<span data-ttu-id="08be6-181">Následující diagram znázorňuje celkový počet vstupně-výstupní operace čtení a zápisu.</span><span class="sxs-lookup"><span data-stu-id="08be6-181">The following diagram shows the total I/O of read and write.</span></span> <span data-ttu-id="08be6-182">Existovaly 59 GB číst a zapisovat během doby sestavy 247.3 GB.</span><span class="sxs-lookup"><span data-stu-id="08be6-182">There were 59 GB read and 247.3 GB written during the time of the report.</span></span>

![Snímek obrazovky stránky sestavy AWR](./media/oracle-design/io_info.png)

#### <a name="2-choose-a-vm"></a><span data-ttu-id="08be6-184">2. Vyberte virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="08be6-184">2. Choose a VM</span></span>

<span data-ttu-id="08be6-185">Na základě informací shromážděných ze sestavy AWR, dalším krokem je vybrat virtuální počítač podobné velikosti, která vyhovuje vašim požadavkům.</span><span class="sxs-lookup"><span data-stu-id="08be6-185">Based on the information that you collected from the AWR report, the next step is to choose a VM of a similar size that meets your requirements.</span></span> <span data-ttu-id="08be6-186">Seznam dostupných virtuálních počítačů najdete v článku [paměťově optimalizované](https://docs.microsoft.com/azure/virtual-machinFine tune es/virtual-machines-windows-sizes-memory).</span><span class="sxs-lookup"><span data-stu-id="08be6-186">You can find a list of available VMs in the article [Memory optimized](https://docs.microsoft.com/azure/virtual-machinFine tune es/virtual-machines-windows-sizes-memory).</span></span>

#### <a name="3-fine-tune-the-vm-sizing-with-a-similar-vm-series-based-on-the-acu"></a><span data-ttu-id="08be6-187">3. Upřesnit nastavení velikosti virtuálních počítačů s řadu virtuálních počítačů podobné podle ACU</span><span class="sxs-lookup"><span data-stu-id="08be6-187">3. Fine-tune the VM sizing with a similar VM series based on the ACU</span></span>

<span data-ttu-id="08be6-188">Po virtuálního počítače jste vybrali, věnujte pozornost ACU pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="08be6-188">After you've chosen the VM, pay attention to the ACU for the VM.</span></span> <span data-ttu-id="08be6-189">Můžete vybrat jiný virtuální počítač na základě hodnoty ACU, která lépe splňuje vaše požadavky.</span><span class="sxs-lookup"><span data-stu-id="08be6-189">You might choose a different VM based on the ACU value that better suits your requirements.</span></span> <span data-ttu-id="08be6-190">Další informace najdete v tématu [Azure výpočetní jednotky](https://docs.microsoft.com/azure/virtual-machines/windows/acu).</span><span class="sxs-lookup"><span data-stu-id="08be6-190">For more information, see [Azure compute unit](https://docs.microsoft.com/azure/virtual-machines/windows/acu).</span></span>

![Snímek obrazovky stránky ACU jednotky](./media/oracle-design/acu_units.png)

### <a name="network-throughput"></a><span data-ttu-id="08be6-192">Propustnost sítě</span><span class="sxs-lookup"><span data-stu-id="08be6-192">Network throughput</span></span>

<span data-ttu-id="08be6-193">Následující diagram znázorňuje vztah mezi propustnost a IOPS:</span><span class="sxs-lookup"><span data-stu-id="08be6-193">The following diagram shows the relation between throughput and IOPS:</span></span>

![Snímek obrazovky propustnost](./media/oracle-design/throughput.png)

<span data-ttu-id="08be6-195">Celkovou šířku propustnost je odhadovaný podle následující informace:</span><span class="sxs-lookup"><span data-stu-id="08be6-195">The total network throughput is estimated based on the following information:</span></span>
- <span data-ttu-id="08be6-196">SQL * Net provoz</span><span class="sxs-lookup"><span data-stu-id="08be6-196">SQL*Net traffic</span></span>
- <span data-ttu-id="08be6-197">MB/s x počet serverů (například Oracle Data Guard výstupní proud)</span><span class="sxs-lookup"><span data-stu-id="08be6-197">MBps x number of servers (outbound stream such as Oracle Data Guard)</span></span>
- <span data-ttu-id="08be6-198">Dalších faktorech, jako je například aplikace replikace</span><span class="sxs-lookup"><span data-stu-id="08be6-198">Other factors, such as application replication</span></span>

![Snímek obrazovky SQL * Net propustnost](./media/oracle-design/sqlnet_info.png)

<span data-ttu-id="08be6-200">Podle potřeb šířky pásma sítě, existují různé typy brány můžete vybírat.</span><span class="sxs-lookup"><span data-stu-id="08be6-200">Based on your network bandwidth requirements, there are various gateway types for you to choose from.</span></span> <span data-ttu-id="08be6-201">Mezi ně patří basic VpnGw a Azure ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="08be6-201">These include basic, VpnGw, and Azure ExpressRoute.</span></span> <span data-ttu-id="08be6-202">Další informace najdete v tématu [brány VPN stránce s cenami](https://azure.microsoft.com/en-us/pricing/details/vpn-gateway/?v=17.23h).</span><span class="sxs-lookup"><span data-stu-id="08be6-202">For more information, see the [VPN gateway pricing page](https://azure.microsoft.com/en-us/pricing/details/vpn-gateway/?v=17.23h).</span></span>

<span data-ttu-id="08be6-203">**Recommendations** (Doporučení)</span><span class="sxs-lookup"><span data-stu-id="08be6-203">**Recommendations**</span></span>

- <span data-ttu-id="08be6-204">Latence sítě vyšší ve srovnání s místním nasazením.</span><span class="sxs-lookup"><span data-stu-id="08be6-204">Network latency is higher compared to an on-premises deployment.</span></span> <span data-ttu-id="08be6-205">Snižuje sítě zaokrouhlí služebních cest může výrazně zlepšit výkon.</span><span class="sxs-lookup"><span data-stu-id="08be6-205">Reducing network round trips can greatly improve performance.</span></span>
- <span data-ttu-id="08be6-206">Pokud chcete zkrátit odezvy, konsolidovat aplikace, které mají vysokou transakce nebo "chatty" aplikací na jednom virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="08be6-206">To reduce round-trips, consolidate applications that have high transactions or “chatty” apps on the same virtual machine.</span></span>

### <a name="disk-types-and-configurations"></a><span data-ttu-id="08be6-207">Typy disků a konfigurace</span><span class="sxs-lookup"><span data-stu-id="08be6-207">Disk types and configurations</span></span>

- <span data-ttu-id="08be6-208">*Výchozí OS disky*: tyto typy disků nabízejí trvalých dat a ukládání do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="08be6-208">*Default OS disks*: These disk types offer persistent data and caching.</span></span> <span data-ttu-id="08be6-209">Jsou optimalizované pro přístup k operační systém při spuštění a není určeno pro buď transakcí nebo datového skladu (analytical) úlohy.</span><span class="sxs-lookup"><span data-stu-id="08be6-209">They are optimized for OS access at startup, and aren't designed for either transactional or data warehouse (analytical) workloads.</span></span>

- <span data-ttu-id="08be6-210">*Nespravované disky*: pomocí těchto typů disku spravovat účty úložiště, které ukládají soubory virtuálního pevného disku (VHD), které odpovídají vaše disky virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="08be6-210">*Unmanaged disks*: With these disk types, you manage the storage accounts that store the virtual hard disk (VHD) files that correspond to your VM disks.</span></span> <span data-ttu-id="08be6-211">Soubory VHD jsou uloženy jako objekty BLOB stránky v účtech úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="08be6-211">VHD files are stored as page blobs in Azure storage accounts.</span></span>

- <span data-ttu-id="08be6-212">*Spravované disky*: spravuje účty úložiště, které používáte pro disky virtuálních počítačů Azure.</span><span class="sxs-lookup"><span data-stu-id="08be6-212">*Managed disks*: Azure manages the storage accounts that you use for your VM disks.</span></span> <span data-ttu-id="08be6-213">Určete typ disku (premium nebo standard) a velikost disku, které potřebujete.</span><span class="sxs-lookup"><span data-stu-id="08be6-213">You specify the disk type (premium or standard) and the size of the disk that you need.</span></span> <span data-ttu-id="08be6-214">Azure vytváří a spravuje disku za vás.</span><span class="sxs-lookup"><span data-stu-id="08be6-214">Azure creates and manages the disk for you.</span></span>

- <span data-ttu-id="08be6-215">*Disky úložiště Premium*: tyto typy disků jsou nejvhodnější pro úlohy v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="08be6-215">*Premium storage disks*: These disk types are best suited for production workloads.</span></span> <span data-ttu-id="08be6-216">Premium storage podporuje disky virtuálních počítačů, které lze připojit k určité virtuálních počítačů velikost series, jako je například řady DS, DSv2, GS a F virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="08be6-216">Premium storage supports VM disks that can be attached to specific size-series VMs, such as DS, DSv2, GS, and F series VMs.</span></span> <span data-ttu-id="08be6-217">Premium disk se dodává s jinou velikostí, a můžete si vybrat mezi disky rozsahu od 32 GB do 4096 GB.</span><span class="sxs-lookup"><span data-stu-id="08be6-217">The premium disk comes with different sizes, and you can choose between disks ranging from 32 GB to 4,096 GB.</span></span> <span data-ttu-id="08be6-218">Velikost každého disku má svou vlastní specifikace výkonu.</span><span class="sxs-lookup"><span data-stu-id="08be6-218">Each disk size has its own performance specifications.</span></span> <span data-ttu-id="08be6-219">V závislosti na požadavcích vaší aplikace můžete jeden nebo více disků připojit k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="08be6-219">Depending on your application requirements, you can attach one or more disks to your VM.</span></span>

<span data-ttu-id="08be6-220">Když vytvoříte nový disk spravované z portálu, můžete **typ účtu** pro typ disku, kterou chcete použít.</span><span class="sxs-lookup"><span data-stu-id="08be6-220">When you create a new managed disk from the portal, you can choose the **Account type** for the type of disk you want to use.</span></span> <span data-ttu-id="08be6-221">Mějte na paměti, že ne všechny dostupné disky jsou zobrazeny v rozevírací nabídce.</span><span class="sxs-lookup"><span data-stu-id="08be6-221">Keep in mind that not all available disks are shown in the drop-down menu.</span></span> <span data-ttu-id="08be6-222">Když vyberete konkrétní velikost virtuálního počítače, v nabídce zobrazuje pouze úložiště k dispozici premium SKU, které jsou založeny na velikost tohoto virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="08be6-222">After you choose a particular VM size, the menu shows only the available premium storage SKUs that are based on that VM size.</span></span>

![Snímek obrazovky stránky spravovaných disků](./media/oracle-design/premium_disk01.png)

<span data-ttu-id="08be6-224">Další informace najdete v tématu [vysoce výkonné úložiště Premium a spravované disky pro virtuální počítače](https://docs.microsoft.com/azure/storage/storage-premium-storage).</span><span class="sxs-lookup"><span data-stu-id="08be6-224">For more information, see [High-performance Premium Storage and managed disks for VMs](https://docs.microsoft.com/azure/storage/storage-premium-storage).</span></span>

<span data-ttu-id="08be6-225">Po dokončení konfigurace úložiště na virtuálním počítači, můžete chtít načíst testování disky před vytvořením databáze.</span><span class="sxs-lookup"><span data-stu-id="08be6-225">After you configure your storage on a VM, you might want to load test the disks before creating a database.</span></span> <span data-ttu-id="08be6-226">Znalost vstupně-výstupních operací míry z hlediska latence a propustnosti vám pomohou určit, pokud očekávaný propustnost s latencí cíle podporují virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="08be6-226">Knowing the I/O rate in terms of both latency and throughput can help you determine if the VMs support the expected throughput with latency targets.</span></span>

<span data-ttu-id="08be6-227">Existuje několik nástrojů pro zatížení testování aplikací, jako je Oracle Orion, Sysbench a Fio.</span><span class="sxs-lookup"><span data-stu-id="08be6-227">There are a number of tools for application load testing, such as Oracle Orion, Sysbench, and Fio.</span></span>

<span data-ttu-id="08be6-228">Znovu spusťte zátěžový test, poté, co nasadíte databázi Oracle.</span><span class="sxs-lookup"><span data-stu-id="08be6-228">Run the load test again after you've deployed an Oracle database.</span></span> <span data-ttu-id="08be6-229">Spuštění úlohy obyčejnými a špičkovým a výsledky zobrazeny směrného plánu vašeho prostředí.</span><span class="sxs-lookup"><span data-stu-id="08be6-229">Start your regular and peak workloads, and the results show you the baseline of your environment.</span></span>

<span data-ttu-id="08be6-230">Může to být víc důležité velikost úložiště založené na rychlost, jakou IOPS spíše než velikost úložiště.</span><span class="sxs-lookup"><span data-stu-id="08be6-230">It might be more important to size the storage based on the IOPS rate rather than the storage size.</span></span> <span data-ttu-id="08be6-231">Například pokud požadovaná IOPS je 5 000, ale potřebujete jenom 200 GB, může stále získáte disku P30 třída premium i když se dodává s více než 200 GB úložiště.</span><span class="sxs-lookup"><span data-stu-id="08be6-231">For example, if the required IOPS is 5,000, but you only need 200 GB, you might still get the P30 class premium disk even though it comes with more than 200 GB of storage.</span></span>

<span data-ttu-id="08be6-232">Rychlost, jakou IOPS je možné získat ze sestavy AWR.</span><span class="sxs-lookup"><span data-stu-id="08be6-232">The IOPS rate can be obtained from the AWR report.</span></span> <span data-ttu-id="08be6-233">Je dáno protokolu opakování, fyzických čtení a zápisů rychlost.</span><span class="sxs-lookup"><span data-stu-id="08be6-233">It's determined by the redo log, physical reads, and writes rate.</span></span>

![Snímek obrazovky stránky sestavy AWR](./media/oracle-design/awr_report.png)

<span data-ttu-id="08be6-235">Například velikost opakování je 12,200,000 bajtů za sekundu, které se rovná 11.63 MB/s.</span><span class="sxs-lookup"><span data-stu-id="08be6-235">For example, the redo size is 12,200,000 bytes per second, which is equal to 11.63 MBPs.</span></span>
<span data-ttu-id="08be6-236">IOPS se 12,200,000 / 2,358 = 5,174.</span><span class="sxs-lookup"><span data-stu-id="08be6-236">The IOPS is 12,200,000 / 2,358 = 5,174.</span></span>

<span data-ttu-id="08be6-237">Až budete mít přehledné informace o vstupně-výstupní požadavky, můžete zvolit kombinaci jednotek, které jsou nejvhodnější pro naplnění těchto požadavků.</span><span class="sxs-lookup"><span data-stu-id="08be6-237">After you have a clear picture of the I/O requirements, you can choose a combination of drives that are best suited to meet those requirements.</span></span>

<span data-ttu-id="08be6-238">**Recommendations** (Doporučení)</span><span class="sxs-lookup"><span data-stu-id="08be6-238">**Recommendations**</span></span>

- <span data-ttu-id="08be6-239">Pro data tabulkového prostoru, rozloženy vstupně-výstupní úlohy počet disků pomocí úložiště spravovaný nebo Oracle ASM.</span><span class="sxs-lookup"><span data-stu-id="08be6-239">For data tablespace, spread the I/O workload across a number of disks by using managed storage or Oracle ASM.</span></span>
- <span data-ttu-id="08be6-240">Jak pro operace náročné na čtení a zápisu náročných zvyšuje velikost bloku vstupně-výstupních operací, přidejte další datové disky.</span><span class="sxs-lookup"><span data-stu-id="08be6-240">As the I/O block size increases for read-intensive and write-intensive operations, add more data disks.</span></span>
- <span data-ttu-id="08be6-241">Zvětšete velikost bloku u velkých sekvenčních procesů.</span><span class="sxs-lookup"><span data-stu-id="08be6-241">Increase the block size for large sequential processes.</span></span>
- <span data-ttu-id="08be6-242">Komprese dat použijte ke snížení vstupně-výstupní operace (pro data a indexů).</span><span class="sxs-lookup"><span data-stu-id="08be6-242">Use data compression to reduce I/O (for both data and indexes).</span></span>
- <span data-ttu-id="08be6-243">Oddělené opakování protokoly, systém a temps a vrátit zpět TS na samostatné datové disky.</span><span class="sxs-lookup"><span data-stu-id="08be6-243">Separate redo logs, system, and temps, and undo TS on separate data disks.</span></span>
- <span data-ttu-id="08be6-244">Umístěte všechny soubory aplikace na výchozí disky operačního systému (/ dev/sda).</span><span class="sxs-lookup"><span data-stu-id="08be6-244">Don't put any application files on default OS disks (/dev/sda).</span></span> <span data-ttu-id="08be6-245">Tyto disky nejsou optimalizovány pro rychlé virtuální počítač nemusí časy spuštění a poskytují dobrý výkon pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="08be6-245">These disks aren't optimized for fast VM boot times, and they might not provide good performance for your application.</span></span>

### <a name="disk-cache-settings"></a><span data-ttu-id="08be6-246">Nastavení mezipaměti na disku</span><span class="sxs-lookup"><span data-stu-id="08be6-246">Disk cache settings</span></span>

<span data-ttu-id="08be6-247">Existují tři možnosti pro ukládání do mezipaměti hostitele:</span><span class="sxs-lookup"><span data-stu-id="08be6-247">There are three options for host caching:</span></span>

- <span data-ttu-id="08be6-248">*Jen pro čtení*: všechny požadavky jsou uložená v mezipaměti pro budoucí čtení.</span><span class="sxs-lookup"><span data-stu-id="08be6-248">*Read-only*: All requests are cached for future reads.</span></span> <span data-ttu-id="08be6-249">Všech zápisů jsou nastavené jako trvalé přímo do Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="08be6-249">All writes are persisted directly to Azure Blob storage.</span></span>

- <span data-ttu-id="08be6-250">*Čtení a zápis*: Toto je "čtení napřed" algoritmu.</span><span class="sxs-lookup"><span data-stu-id="08be6-250">*Read-write*: This is a “read-ahead” algorithm.</span></span> <span data-ttu-id="08be6-251">Čtení a zápisu jsou uložená v mezipaměti pro budoucí čtení.</span><span class="sxs-lookup"><span data-stu-id="08be6-251">The reads and writes are cached for future reads.</span></span> <span data-ttu-id="08be6-252">Non zápisu prostřednictvím zápisů jsou trvalé nejprve do místní mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="08be6-252">Non-write-through writes are persisted to the local cache first.</span></span> <span data-ttu-id="08be6-253">Pro systém SQL Server zůstávají zápisy do úložiště Azure, protože používá přímým zápisem.</span><span class="sxs-lookup"><span data-stu-id="08be6-253">For SQL Server, writes are persisted to Azure Storage because it uses write-through.</span></span> <span data-ttu-id="08be6-254">Také poskytuje nejnižší latenci disku pro lehké úlohy.</span><span class="sxs-lookup"><span data-stu-id="08be6-254">It also provides the lowest disk latency for light workloads.</span></span>

- <span data-ttu-id="08be6-255">*Žádný* (zakázáno): pomocí této možnosti můžete obejít mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="08be6-255">*None* (disabled): By using this option, you can bypass the cache.</span></span> <span data-ttu-id="08be6-256">Všechna data převede na disk a trvalé do služby Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="08be6-256">All the data is transferred to disk and persisted to Azure Storage.</span></span> <span data-ttu-id="08be6-257">Tato metoda poskytuje nejvyšší rychlost vstupně-výstupních operací, pro zatížení s intenzivním vstupně-výstupních operací.</span><span class="sxs-lookup"><span data-stu-id="08be6-257">This method gives you the highest I/O rate for I/O intensive workloads.</span></span> <span data-ttu-id="08be6-258">Musíte také vzít v úvahu "transakce náklady".</span><span class="sxs-lookup"><span data-stu-id="08be6-258">You also need to take “transaction cost” into consideration.</span></span>

<span data-ttu-id="08be6-259">**Recommendations** (Doporučení)</span><span class="sxs-lookup"><span data-stu-id="08be6-259">**Recommendations**</span></span>

<span data-ttu-id="08be6-260">Chcete-li maximalizovat propustnost, doporučujeme začínat **žádné** pro použití mezipaměti u hostitele.</span><span class="sxs-lookup"><span data-stu-id="08be6-260">To maximize the throughput, we recommend that you  start with **None** for host caching.</span></span> <span data-ttu-id="08be6-261">Pro Storage úrovně Premium, mějte "překážek" je nutné zakázat, když připojíte pomocí systému souborů **jen pro čtení** nebo **žádné** možnosti.</span><span class="sxs-lookup"><span data-stu-id="08be6-261">For Premium Storage, keep in mind that you must disable the "barriers" when you mount the file system with the **ReadOnly** or **None** options.</span></span> <span data-ttu-id="08be6-262">Aktualizujte soubor /etc/fstab s UUID na discích.</span><span class="sxs-lookup"><span data-stu-id="08be6-262">Update the /etc/fstab file with the UUID to the disks.</span></span>

<span data-ttu-id="08be6-263">Další informace najdete v tématu [Premium úložiště pro virtuální počítače s Linuxem](https://docs.microsoft.com/azure/storage/storage-premium-storage#premium-storage-for-linux-vms).</span><span class="sxs-lookup"><span data-stu-id="08be6-263">For more information, see [Premium Storage for Linux VMs](https://docs.microsoft.com/azure/storage/storage-premium-storage#premium-storage-for-linux-vms).</span></span>

![Snímek obrazovky stránky spravovaných disků](./media/oracle-design/premium_disk02.png)

- <span data-ttu-id="08be6-265">Pro disky operačního systému, použít výchozí **pro čtení a zápis** ukládání do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="08be6-265">For OS disks, use default **Read/Write** caching.</span></span>
- <span data-ttu-id="08be6-266">Pro systém, TEMP a vrácení zpět pomocí **žádné** pro ukládání do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="08be6-266">For SYSTEM, TEMP, and UNDO use **None** for caching.</span></span>
- <span data-ttu-id="08be6-267">Pro DATA, použijte **žádné** pro ukládání do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="08be6-267">For DATA, use **None** for caching.</span></span> <span data-ttu-id="08be6-268">Ale pokud vaše databáze je jen pro čtení nebo náročné na čtení, použijte **jen pro čtení** ukládání do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="08be6-268">But if your database is read-only or read-intensive, use **Read-only** caching.</span></span>

<span data-ttu-id="08be6-269">Po uložení nastavení disku vaše data nelze změnit nastavení mezipaměti hostitele, pokud se odpojit disk na úrovni operačního systému a znovu připojte po provedení změn.</span><span class="sxs-lookup"><span data-stu-id="08be6-269">After your data disk setting is saved, you can't change the host cache setting unless you unmount the drive at the OS level and then remount it after you've made the change.</span></span>


## <a name="security"></a><span data-ttu-id="08be6-270">Zabezpečení</span><span class="sxs-lookup"><span data-stu-id="08be6-270">Security</span></span>

<span data-ttu-id="08be6-271">Po nastavení a konfiguraci prostředí Azure, dalším krokem je zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="08be6-271">After you have set up and configured your Azure environment, the next step is to secure your network.</span></span> <span data-ttu-id="08be6-272">Tady jsou některá doporučení:</span><span class="sxs-lookup"><span data-stu-id="08be6-272">Here are some recommendations:</span></span>

- <span data-ttu-id="08be6-273">*Zásady skupiny NSG*: NSG může být definovaná podsíť nebo síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="08be6-273">*NSG policy*: NSG can be defined by a subnet or NIC.</span></span> <span data-ttu-id="08be6-274">Je jednodušší pro řízení přístupu na úrovni podsítě jak pro zabezpečení a vynutit směrování pro věcmi, jako jsou brány firewall pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="08be6-274">It's simpler to control access at the subnet level both for security and force routing for things like application firewalls.</span></span>

- <span data-ttu-id="08be6-275">*Jumpbox*: lépe zabezpečeného přístupu správci nesmí připojovat přímo k služba aplikace nebo databáze.</span><span class="sxs-lookup"><span data-stu-id="08be6-275">*Jumpbox*: For more secure access, administrators should not directly connect to the application service or database.</span></span> <span data-ttu-id="08be6-276">Jumpbox slouží jako média mezi počítačem správce a prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="08be6-276">A jumpbox is used as a media between the administrator machine and Azure resources.</span></span>
<span data-ttu-id="08be6-277">![Snímek obrazovky stránky Jumpbox topologie](./media/oracle-design/jumpbox.png)</span><span class="sxs-lookup"><span data-stu-id="08be6-277">![Screenshot of the Jumpbox topology page](./media/oracle-design/jumpbox.png)</span></span>

    <span data-ttu-id="08be6-278">Počítač správce by měl nabízejí IP omezený přístup jenom jumpbox.</span><span class="sxs-lookup"><span data-stu-id="08be6-278">The administrator machine should offer IP-restricted access to the jumpbox only.</span></span> <span data-ttu-id="08be6-279">Jumpbox mají mít přístup k aplikaci a databázi.</span><span class="sxs-lookup"><span data-stu-id="08be6-279">The jumpbox should have access to the application and database.</span></span>

- <span data-ttu-id="08be6-280">*Privátní sítě* (podsítě): doporučujeme mít aplikace služby a databáze v samostatných podsítích v tak lepší řízení lze nastavit pomocí zásad skupiny NSG.</span><span class="sxs-lookup"><span data-stu-id="08be6-280">*Private network* (subnets): We recommend that you have the application service and database on separate subnets, so better control can be set by NSG policy.</span></span>


## <a name="additional-reading"></a><span data-ttu-id="08be6-281">Další čtení</span><span class="sxs-lookup"><span data-stu-id="08be6-281">Additional reading</span></span>

- [<span data-ttu-id="08be6-282">Konfigurace Oracle ASM</span><span class="sxs-lookup"><span data-stu-id="08be6-282">Configure Oracle ASM</span></span>](configure-oracle-asm.md)
- [<span data-ttu-id="08be6-283">Konfigurace Oracle Data Guard</span><span class="sxs-lookup"><span data-stu-id="08be6-283">Configure Oracle Data Guard</span></span>](configure-oracle-dataguard.md)
- [<span data-ttu-id="08be6-284">Konfigurace brány Golden Oracle</span><span class="sxs-lookup"><span data-stu-id="08be6-284">Configure Oracle Golden Gate</span></span>](configure-oracle-golden-gate.md)
- [<span data-ttu-id="08be6-285">Oracle zálohování a obnovení</span><span class="sxs-lookup"><span data-stu-id="08be6-285">Oracle backup and recovery</span></span>](oracle-backup-recovery.md)

## <a name="next-steps"></a><span data-ttu-id="08be6-286">Další kroky</span><span class="sxs-lookup"><span data-stu-id="08be6-286">Next steps</span></span>

- [<span data-ttu-id="08be6-287">Kurz: Vytvoření vysoce dostupné virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="08be6-287">Tutorial: Create highly available VMs</span></span>](../../linux/create-cli-complete.md)
- [<span data-ttu-id="08be6-288">Prozkoumejte ukázky rozhraní příkazového řádku Azure nasazení virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="08be6-288">Explore VM deployment Azure CLI samples</span></span>](../../linux/cli-samples.md)
