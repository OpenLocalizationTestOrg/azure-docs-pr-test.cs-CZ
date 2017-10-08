---
title: "aaaDesign a implementujte Oracle databáze v Azure | Microsoft Docs"
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
ms.openlocfilehash: 8fa1207458695df1c7330ec626888b1b6b8d8939
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="design-and-implement-an-oracle-database-in-azure"></a><span data-ttu-id="51fea-103">Návrh a implementaci k databázi Oracle v Azure</span><span class="sxs-lookup"><span data-stu-id="51fea-103">Design and implement an Oracle database in Azure</span></span>

## <a name="assumptions"></a><span data-ttu-id="51fea-104">Předpoklady</span><span class="sxs-lookup"><span data-stu-id="51fea-104">Assumptions</span></span>

- <span data-ttu-id="51fea-105">Plánování toomigrate z tooAzure místní databáze Oracle.</span><span class="sxs-lookup"><span data-stu-id="51fea-105">You're planning toomigrate an Oracle database from on-premises tooAzure.</span></span>
- <span data-ttu-id="51fea-106">Pochopení hello máte v sestavách Oracle AWR různé metriky.</span><span class="sxs-lookup"><span data-stu-id="51fea-106">You have an understanding of hello various metrics in Oracle AWR reports.</span></span>
- <span data-ttu-id="51fea-107">Máte základní znalosti aplikace výkonu a využití platformy.</span><span class="sxs-lookup"><span data-stu-id="51fea-107">You have a baseline understanding of application performance and platform utilization.</span></span>

## <a name="goals"></a><span data-ttu-id="51fea-108">Cíle</span><span class="sxs-lookup"><span data-stu-id="51fea-108">Goals</span></span>

- <span data-ttu-id="51fea-109">Pochopit, jak toooptimize Oracle nasazení v Azure.</span><span class="sxs-lookup"><span data-stu-id="51fea-109">Understand how toooptimize your Oracle deployment in Azure.</span></span>
- <span data-ttu-id="51fea-110">Prozkoumejte možností pro databázi Oracle v prostředí Azure ladění výkonu.</span><span class="sxs-lookup"><span data-stu-id="51fea-110">Explore performance tuning options for an Oracle database in an Azure environment.</span></span>

## <a name="hello-differences-between-an-on-premises-and-azure-implementation"></a><span data-ttu-id="51fea-111">Hello rozdíly mezi místním a Azure implementace</span><span class="sxs-lookup"><span data-stu-id="51fea-111">hello differences between an on-premises and Azure implementation</span></span> 

<span data-ttu-id="51fea-112">Toto jsou některé důležité věci tookeep na paměti, když se migraci místní tooAzure aplikace.</span><span class="sxs-lookup"><span data-stu-id="51fea-112">Following are some important things tookeep in mind when you're migrating on-premises applications tooAzure.</span></span> 

<span data-ttu-id="51fea-113">Jeden důležitý rozdíl je, že v implementaci Azure prostředkům, například virtuálních počítačů, disků a virtuální sítě sdílejí mezi ostatní klienty.</span><span class="sxs-lookup"><span data-stu-id="51fea-113">One important difference is that in an Azure implementation, resources such as VMs, disks, and virtual networks are shared among other clients.</span></span> <span data-ttu-id="51fea-114">Kromě toho prostředky může být omezena na základě požadavků hello.</span><span class="sxs-lookup"><span data-stu-id="51fea-114">In addition, resources can be throttled based on hello requirements.</span></span> <span data-ttu-id="51fea-115">Místo zaměřené na zamezení selhání provozu (MTBF), Azure zaměřují na zbývajících selhání hello (MTTR).</span><span class="sxs-lookup"><span data-stu-id="51fea-115">Instead of focusing on avoiding failing (MTBF), Azure is more focused on surviving hello failure (MTTR).</span></span>

<span data-ttu-id="51fea-116">Hello následující tabulka uvádí některé z hello rozdíly mezi místními implementace a implementaci Azure pro databázi Oracle.</span><span class="sxs-lookup"><span data-stu-id="51fea-116">hello following table lists some of hello differences between an on-premises implementation and an Azure implementation of an Oracle database.</span></span>

> 
> |  | <span data-ttu-id="51fea-117">**Místní implementace**</span><span class="sxs-lookup"><span data-stu-id="51fea-117">**On-premises implementation**</span></span> | <span data-ttu-id="51fea-118">**Azure implementace**</span><span class="sxs-lookup"><span data-stu-id="51fea-118">**Azure implementation**</span></span> |
> | --- | --- | --- |
> | <span data-ttu-id="51fea-119">**Sítě**</span><span class="sxs-lookup"><span data-stu-id="51fea-119">**Networking**</span></span> |<span data-ttu-id="51fea-120">LAN NEBO WAN</span><span class="sxs-lookup"><span data-stu-id="51fea-120">LAN/WAN</span></span>  |<span data-ttu-id="51fea-121">SDN (softwarově definované sítě)</span><span class="sxs-lookup"><span data-stu-id="51fea-121">SDN (software-defined networking)</span></span>|
> | <span data-ttu-id="51fea-122">**Skupina zabezpečení**</span><span class="sxs-lookup"><span data-stu-id="51fea-122">**Security group**</span></span> |<span data-ttu-id="51fea-123">Nástroje pro omezení adresy IP a portu</span><span class="sxs-lookup"><span data-stu-id="51fea-123">IP/port restriction tools</span></span> |[<span data-ttu-id="51fea-124">Skupina zabezpečení sítě (NSG)</span><span class="sxs-lookup"><span data-stu-id="51fea-124">Network Security Group (NSG)</span></span>](https://azure.microsoft.com/blog/network-security-groups) |
> | <span data-ttu-id="51fea-125">**Odolnost proti**</span><span class="sxs-lookup"><span data-stu-id="51fea-125">**Resilience**</span></span> |<span data-ttu-id="51fea-126">MTBF (střední čas mezi selhání)</span><span class="sxs-lookup"><span data-stu-id="51fea-126">MTBF (mean time between failures)</span></span> |<span data-ttu-id="51fea-127">MTTR (toorecovery střední čas)</span><span class="sxs-lookup"><span data-stu-id="51fea-127">MTTR (mean time toorecovery)</span></span>|
> | <span data-ttu-id="51fea-128">**Plánovaná údržba**</span><span class="sxs-lookup"><span data-stu-id="51fea-128">**Planned maintenance**</span></span> |<span data-ttu-id="51fea-129">Opravy chyb a upgrady</span><span class="sxs-lookup"><span data-stu-id="51fea-129">Patching/upgrades</span></span>|<span data-ttu-id="51fea-130">[Skupiny dostupnosti](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-availability-sets-guidelines) (opravy a upgrady spravovat přes Azure)</span><span class="sxs-lookup"><span data-stu-id="51fea-130">[Availability sets](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-availability-sets-guidelines) (patching/upgrades managed by Azure)</span></span> |
> | <span data-ttu-id="51fea-131">**Prostředek**</span><span class="sxs-lookup"><span data-stu-id="51fea-131">**Resource**</span></span> |<span data-ttu-id="51fea-132">Vyhrazený</span><span class="sxs-lookup"><span data-stu-id="51fea-132">Dedicated</span></span>  |<span data-ttu-id="51fea-133">Sdílet s ostatními klienty</span><span class="sxs-lookup"><span data-stu-id="51fea-133">Shared with other clients</span></span>|
> | <span data-ttu-id="51fea-134">**Oblasti**</span><span class="sxs-lookup"><span data-stu-id="51fea-134">**Regions**</span></span> |<span data-ttu-id="51fea-135">Datová centra</span><span class="sxs-lookup"><span data-stu-id="51fea-135">Datacenters</span></span> |[<span data-ttu-id="51fea-136">Dvojice oblast</span><span class="sxs-lookup"><span data-stu-id="51fea-136">Region pairs</span></span>](https://docs.microsoft.com/azure/virtual-machines/windows/regions-and-availability)|
> | <span data-ttu-id="51fea-137">**Úložiště**</span><span class="sxs-lookup"><span data-stu-id="51fea-137">**Storage**</span></span> |<span data-ttu-id="51fea-138">Síť SAN nebo fyzické disky</span><span class="sxs-lookup"><span data-stu-id="51fea-138">SAN/physical disks</span></span> |[<span data-ttu-id="51fea-139">Spravovat Azure storage</span><span class="sxs-lookup"><span data-stu-id="51fea-139">Azure-managed storage</span></span>](https://azure.microsoft.com/pricing/details/managed-disks/?v=17.23h)|
> | <span data-ttu-id="51fea-140">**Škálování**</span><span class="sxs-lookup"><span data-stu-id="51fea-140">**Scale**</span></span> |<span data-ttu-id="51fea-141">Vertikální Škálováním</span><span class="sxs-lookup"><span data-stu-id="51fea-141">Vertical scale</span></span> |<span data-ttu-id="51fea-142">Horizontální škálování</span><span class="sxs-lookup"><span data-stu-id="51fea-142">Horizontal scale</span></span>|


### <a name="requirements"></a><span data-ttu-id="51fea-143">Požadavky</span><span class="sxs-lookup"><span data-stu-id="51fea-143">Requirements</span></span>

- <span data-ttu-id="51fea-144">Určete rychlost velikosti a nárůst databáze hello.</span><span class="sxs-lookup"><span data-stu-id="51fea-144">Determine hello database size and growth rate.</span></span>
- <span data-ttu-id="51fea-145">Určete požadavky hello IOPS, které můžete odhadnout na základě Oracle AWR sestavy nebo jiné sítě, nástroje pro sledování.</span><span class="sxs-lookup"><span data-stu-id="51fea-145">Determine hello IOPS requirements, which you can estimate based on Oracle AWR reports or other network monitoring tools.</span></span>

## <a name="configuration-options"></a><span data-ttu-id="51fea-146">Možnosti konfigurace</span><span class="sxs-lookup"><span data-stu-id="51fea-146">Configuration options</span></span>

<span data-ttu-id="51fea-147">Existují čtyři potenciálních oblastí, abyste mohli vyladit výkon tooimprove v prostředí Azure:</span><span class="sxs-lookup"><span data-stu-id="51fea-147">There are four potential areas that you can tune tooimprove performance in an Azure environment:</span></span>

- <span data-ttu-id="51fea-148">Velikost virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="51fea-148">Virtual machine size</span></span>
- <span data-ttu-id="51fea-149">Propustnost sítě</span><span class="sxs-lookup"><span data-stu-id="51fea-149">Network throughput</span></span>
- <span data-ttu-id="51fea-150">Typy disků a konfigurace</span><span class="sxs-lookup"><span data-stu-id="51fea-150">Disk types and configurations</span></span>
- <span data-ttu-id="51fea-151">Nastavení mezipaměti na disku</span><span class="sxs-lookup"><span data-stu-id="51fea-151">Disk cache settings</span></span>

### <a name="generate-an-awr-report"></a><span data-ttu-id="51fea-152">Generování sestavy AWR</span><span class="sxs-lookup"><span data-stu-id="51fea-152">Generate an AWR report</span></span>

<span data-ttu-id="51fea-153">Pokud máte existující databázi Oracle a plánování toomigrate tooAzure, máte několik možností.</span><span class="sxs-lookup"><span data-stu-id="51fea-153">If you have an existing an Oracle database and are planning toomigrate tooAzure, you have several options.</span></span> <span data-ttu-id="51fea-154">Můžete spustit hello Oracle AWR sestavy tooget hello metriky (IOPS, MB/s, GiBs a tak dále).</span><span class="sxs-lookup"><span data-stu-id="51fea-154">You can run hello Oracle AWR report tooget hello metrics (IOPS, Mbps, GiBs, and so on).</span></span> <span data-ttu-id="51fea-155">Zvolte hello virtuálních počítačů v závislosti na hello metriky, které jste shromáždili.</span><span class="sxs-lookup"><span data-stu-id="51fea-155">Then choose hello VM based on hello metrics that you collected.</span></span> <span data-ttu-id="51fea-156">Nebo se obrátit na váš tým tooget podobné informace o infrastruktuře.</span><span class="sxs-lookup"><span data-stu-id="51fea-156">Or you can contact your infrastructure team tooget similar information.</span></span>

<span data-ttu-id="51fea-157">Můžete zvážit spouštění sestavy AWR během pravidelné a špičkovým úlohy, takže můžete porovnat.</span><span class="sxs-lookup"><span data-stu-id="51fea-157">You might consider running your AWR report during both regular and peak workloads, so you can compare.</span></span> <span data-ttu-id="51fea-158">Na základě těchto zpráv, může Velikost hello virtuální počítače založené na hello průměrné zatížení nebo hello maximální zátěž.</span><span class="sxs-lookup"><span data-stu-id="51fea-158">Based on these reports, you can size hello VMs based on either hello average workload or hello maximum workload.</span></span>

<span data-ttu-id="51fea-159">Tady je příklad toho, jak toogenerate zprávu o AWR:</span><span class="sxs-lookup"><span data-stu-id="51fea-159">Following is an example of how toogenerate an AWR report:</span></span>

```bash
$ sqlplus / as sysdba
SQL> EXEC DBMS_WORKLOAD_REPOSITORY.CREATE_SNAPSHOT;
SQL> @?/rdbms/admin/awrrpt.sql
```

### <a name="key-metrics"></a><span data-ttu-id="51fea-160">Klíčové metriky</span><span class="sxs-lookup"><span data-stu-id="51fea-160">Key metrics</span></span>

<span data-ttu-id="51fea-161">Následují hello metriky, které můžete získat z hello AWR sestavy:</span><span class="sxs-lookup"><span data-stu-id="51fea-161">Following are hello metrics that you can obtain from hello AWR report:</span></span>

- <span data-ttu-id="51fea-162">Celkový počet jader</span><span class="sxs-lookup"><span data-stu-id="51fea-162">Total number of cores</span></span>
- <span data-ttu-id="51fea-163">Procesor o rychlosti</span><span class="sxs-lookup"><span data-stu-id="51fea-163">CPU clock speed</span></span>
- <span data-ttu-id="51fea-164">Celkové paměti v GB</span><span class="sxs-lookup"><span data-stu-id="51fea-164">Total memory in GB</span></span>
- <span data-ttu-id="51fea-165">Využití procesoru</span><span class="sxs-lookup"><span data-stu-id="51fea-165">CPU utilization</span></span>
- <span data-ttu-id="51fea-166">Rychlost přenosu dat ve špičce</span><span class="sxs-lookup"><span data-stu-id="51fea-166">Peak data transfer rate</span></span>
- <span data-ttu-id="51fea-167">Počet vstupně-výstupních operací změny (čtení a zápis)</span><span class="sxs-lookup"><span data-stu-id="51fea-167">Rate of I/O changes (read/write)</span></span>
- <span data-ttu-id="51fea-168">Vrátit protokolu rychlost (MB/s)</span><span class="sxs-lookup"><span data-stu-id="51fea-168">Redo log rate (MBPs)</span></span>
- <span data-ttu-id="51fea-169">Propustnost sítě</span><span class="sxs-lookup"><span data-stu-id="51fea-169">Network throughput</span></span>
- <span data-ttu-id="51fea-170">Míra latence sítě (dolní nebo horní)</span><span class="sxs-lookup"><span data-stu-id="51fea-170">Network latency rate (low/high)</span></span>
- <span data-ttu-id="51fea-171">Velikost databáze v GB</span><span class="sxs-lookup"><span data-stu-id="51fea-171">Database size in GB</span></span>
- <span data-ttu-id="51fea-172">Bajtů přijatých prostřednictvím SQL * Net z / tooclient</span><span class="sxs-lookup"><span data-stu-id="51fea-172">Bytes received via SQL*Net from/tooclient</span></span>

### <a name="virtual-machine-size"></a><span data-ttu-id="51fea-173">Velikost virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="51fea-173">Virtual machine size</span></span>

#### <a name="1-estimate-vm-size-based-on-cpu-memory-and-io-usage-from-hello-awr-report"></a><span data-ttu-id="51fea-174">1. Odhad velikosti virtuálního počítače podle využití procesoru, paměti a vstupu a výstupu z hello AWR sestavy</span><span class="sxs-lookup"><span data-stu-id="51fea-174">1. Estimate VM size based on CPU, memory, and I/O usage from hello AWR report</span></span>

<span data-ttu-id="51fea-175">Jednou z věcí, které může vypadat v je hello nejvyšší pět vypršel popředí události, které určují, kde se kritická místa systému hello.</span><span class="sxs-lookup"><span data-stu-id="51fea-175">One thing you might look at is hello top five timed foreground events that indicate where hello system bottlenecks are.</span></span>

<span data-ttu-id="51fea-176">V následujícím diagramu hello, například synchronizace souboru protokolu hello je v horní části hello.</span><span class="sxs-lookup"><span data-stu-id="51fea-176">For example, in hello following diagram, hello log file sync is at hello top.</span></span> <span data-ttu-id="51fea-177">Označuje, hello počet počká, které jsou požadovány, než hello LGWR zapíše hello protokolu vyrovnávací paměti souboru protokolu toohello operaci znovu.</span><span class="sxs-lookup"><span data-stu-id="51fea-177">It indicates hello number of waits that are required before hello LGWR writes hello log buffer toohello redo log file.</span></span> <span data-ttu-id="51fea-178">Tyto výsledky naznačují, že jsou lépe provádění úložiště nebo disky.</span><span class="sxs-lookup"><span data-stu-id="51fea-178">These results indicate that better performing storage or disks are required.</span></span> <span data-ttu-id="51fea-179">Hello diagram navíc také zobrazuje hello počet procesor (jádra) a hello množství paměti.</span><span class="sxs-lookup"><span data-stu-id="51fea-179">In addition, hello diagram also shows hello number of CPU (cores) and hello amount of memory.</span></span>

![Snímek obrazovky stránky sestavy AWR hello](./media/oracle-design/cpu_memory_info.png)

<span data-ttu-id="51fea-181">Hello následující diagram znázorňuje hello celkový počet vstupně-výstupní operace čtení a zápisu.</span><span class="sxs-lookup"><span data-stu-id="51fea-181">hello following diagram shows hello total I/O of read and write.</span></span> <span data-ttu-id="51fea-182">Existovaly 59 GB číst a zapisovat během doby hello hello sestavy 247.3 GB.</span><span class="sxs-lookup"><span data-stu-id="51fea-182">There were 59 GB read and 247.3 GB written during hello time of hello report.</span></span>

![Snímek obrazovky stránky sestavy AWR hello](./media/oracle-design/io_info.png)

#### <a name="2-choose-a-vm"></a><span data-ttu-id="51fea-184">2. Vyberte virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="51fea-184">2. Choose a VM</span></span>

<span data-ttu-id="51fea-185">Na základě hello informací shromážděných z hello AWR sestavy, hello dalším krokem je toochoose virtuálních počítačů podobné velikosti, která vyhovuje vašim požadavkům.</span><span class="sxs-lookup"><span data-stu-id="51fea-185">Based on hello information that you collected from hello AWR report, hello next step is toochoose a VM of a similar size that meets your requirements.</span></span> <span data-ttu-id="51fea-186">Seznam dostupných virtuálních počítačů najdete v článku hello [paměťově optimalizované](https://docs.microsoft.com/azure/virtual-machinFine tune es/virtual-machines-windows-sizes-memory).</span><span class="sxs-lookup"><span data-stu-id="51fea-186">You can find a list of available VMs in hello article [Memory optimized](https://docs.microsoft.com/azure/virtual-machinFine tune es/virtual-machines-windows-sizes-memory).</span></span>

#### <a name="3-fine-tune-hello-vm-sizing-with-a-similar-vm-series-based-on-hello-acu"></a><span data-ttu-id="51fea-187">3. Upřesnit nastavení velikosti virtuálních počítačů hello podobné řady virtuálního počítače podle hello ACU</span><span class="sxs-lookup"><span data-stu-id="51fea-187">3. Fine-tune hello VM sizing with a similar VM series based on hello ACU</span></span>

<span data-ttu-id="51fea-188">Poté, co jste vybrali hello virtuálních počítačů, věnujte pozornost toohello ACU pro hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="51fea-188">After you've chosen hello VM, pay attention toohello ACU for hello VM.</span></span> <span data-ttu-id="51fea-189">Můžete vybrat jiný virtuální počítač podle hello ACU hodnotu, která lépe splňuje vaše požadavky.</span><span class="sxs-lookup"><span data-stu-id="51fea-189">You might choose a different VM based on hello ACU value that better suits your requirements.</span></span> <span data-ttu-id="51fea-190">Další informace najdete v tématu [Azure výpočetní jednotky](https://docs.microsoft.com/azure/virtual-machines/windows/acu).</span><span class="sxs-lookup"><span data-stu-id="51fea-190">For more information, see [Azure compute unit](https://docs.microsoft.com/azure/virtual-machines/windows/acu).</span></span>

![Snímek obrazovky stránky jednotky ACU hello](./media/oracle-design/acu_units.png)

### <a name="network-throughput"></a><span data-ttu-id="51fea-192">Propustnost sítě</span><span class="sxs-lookup"><span data-stu-id="51fea-192">Network throughput</span></span>

<span data-ttu-id="51fea-193">Následující diagram ukazuje hello vztah mezi propustnost a IOPS Hello:</span><span class="sxs-lookup"><span data-stu-id="51fea-193">hello following diagram shows hello relation between throughput and IOPS:</span></span>

![Snímek obrazovky propustnost](./media/oracle-design/throughput.png)

<span data-ttu-id="51fea-195">propustnost sítě celkový Hello je odhadovaný podle hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="51fea-195">hello total network throughput is estimated based on hello following information:</span></span>
- <span data-ttu-id="51fea-196">SQL * Net provoz</span><span class="sxs-lookup"><span data-stu-id="51fea-196">SQL*Net traffic</span></span>
- <span data-ttu-id="51fea-197">MB/s x počet serverů (například Oracle Data Guard výstupní proud)</span><span class="sxs-lookup"><span data-stu-id="51fea-197">MBps x number of servers (outbound stream such as Oracle Data Guard)</span></span>
- <span data-ttu-id="51fea-198">Dalších faktorech, jako je například aplikace replikace</span><span class="sxs-lookup"><span data-stu-id="51fea-198">Other factors, such as application replication</span></span>

![Snímek obrazovky hello SQL * Net propustnost](./media/oracle-design/sqlnet_info.png)

<span data-ttu-id="51fea-200">Podle potřeb šířky pásma sítě, existují různé typy brány pro toochoose z.</span><span class="sxs-lookup"><span data-stu-id="51fea-200">Based on your network bandwidth requirements, there are various gateway types for you toochoose from.</span></span> <span data-ttu-id="51fea-201">Mezi ně patří basic VpnGw a Azure ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="51fea-201">These include basic, VpnGw, and Azure ExpressRoute.</span></span> <span data-ttu-id="51fea-202">Další informace najdete v tématu hello [brány VPN stránce s cenami](https://azure.microsoft.com/en-us/pricing/details/vpn-gateway/?v=17.23h).</span><span class="sxs-lookup"><span data-stu-id="51fea-202">For more information, see hello [VPN gateway pricing page](https://azure.microsoft.com/en-us/pricing/details/vpn-gateway/?v=17.23h).</span></span>

<span data-ttu-id="51fea-203">**Recommendations** (Doporučení)</span><span class="sxs-lookup"><span data-stu-id="51fea-203">**Recommendations**</span></span>

- <span data-ttu-id="51fea-204">Je latence sítě vyšší porovnání tooan místního nasazení.</span><span class="sxs-lookup"><span data-stu-id="51fea-204">Network latency is higher compared tooan on-premises deployment.</span></span> <span data-ttu-id="51fea-205">Snižuje sítě zaokrouhlí služebních cest může výrazně zlepšit výkon.</span><span class="sxs-lookup"><span data-stu-id="51fea-205">Reducing network round trips can greatly improve performance.</span></span>
- <span data-ttu-id="51fea-206">tooreduce odezvy konsolidovat aplikace, které mají vysokou transakce nebo "chatty" aplikací na hello stejného virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="51fea-206">tooreduce round-trips, consolidate applications that have high transactions or “chatty” apps on hello same virtual machine.</span></span>

### <a name="disk-types-and-configurations"></a><span data-ttu-id="51fea-207">Typy disků a konfigurace</span><span class="sxs-lookup"><span data-stu-id="51fea-207">Disk types and configurations</span></span>

- <span data-ttu-id="51fea-208">*Výchozí OS disky*: tyto typy disků nabízejí trvalých dat a ukládání do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="51fea-208">*Default OS disks*: These disk types offer persistent data and caching.</span></span> <span data-ttu-id="51fea-209">Jsou optimalizované pro přístup k operační systém při spuštění a není určeno pro buď transakcí nebo datového skladu (analytical) úlohy.</span><span class="sxs-lookup"><span data-stu-id="51fea-209">They are optimized for OS access at startup, and aren't designed for either transactional or data warehouse (analytical) workloads.</span></span>

- <span data-ttu-id="51fea-210">*Nespravované disky*: pomocí těchto typů disku spravovat hello účty úložiště, které ukládají soubory hello virtuální pevný disk (VHD), které odpovídají tooyour disky virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="51fea-210">*Unmanaged disks*: With these disk types, you manage hello storage accounts that store hello virtual hard disk (VHD) files that correspond tooyour VM disks.</span></span> <span data-ttu-id="51fea-211">Soubory VHD jsou uloženy jako objekty BLOB stránky v účtech úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="51fea-211">VHD files are stored as page blobs in Azure storage accounts.</span></span>

- <span data-ttu-id="51fea-212">*Spravované disky*: spravuje hello účty úložiště, které používáte pro disky virtuálních počítačů Azure.</span><span class="sxs-lookup"><span data-stu-id="51fea-212">*Managed disks*: Azure manages hello storage accounts that you use for your VM disks.</span></span> <span data-ttu-id="51fea-213">Určete typ disku hello (premium nebo standard) a velikost hello hello disku, které potřebujete.</span><span class="sxs-lookup"><span data-stu-id="51fea-213">You specify hello disk type (premium or standard) and hello size of hello disk that you need.</span></span> <span data-ttu-id="51fea-214">Azure vytváří a spravuje hello disku za vás.</span><span class="sxs-lookup"><span data-stu-id="51fea-214">Azure creates and manages hello disk for you.</span></span>

- <span data-ttu-id="51fea-215">*Disky úložiště Premium*: tyto typy disků jsou nejvhodnější pro úlohy v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="51fea-215">*Premium storage disks*: These disk types are best suited for production workloads.</span></span> <span data-ttu-id="51fea-216">Premium storage podporuje VM disky, které může být připojen toospecific velikost series virtuálních počítačů, jako je například řady DS, DSv2, GS a F virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="51fea-216">Premium storage supports VM disks that can be attached toospecific size-series VMs, such as DS, DSv2, GS, and F series VMs.</span></span> <span data-ttu-id="51fea-217">Hello premium disku se dodává s jinou velikostí, a můžete si vybrat mezi disky od too4 32 GB, 096 GB.</span><span class="sxs-lookup"><span data-stu-id="51fea-217">hello premium disk comes with different sizes, and you can choose between disks ranging from 32 GB too4,096 GB.</span></span> <span data-ttu-id="51fea-218">Velikost každého disku má svou vlastní specifikace výkonu.</span><span class="sxs-lookup"><span data-stu-id="51fea-218">Each disk size has its own performance specifications.</span></span> <span data-ttu-id="51fea-219">V závislosti na požadavcích vaší aplikace můžete připojit jednoho nebo více disků tooyour virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="51fea-219">Depending on your application requirements, you can attach one or more disks tooyour VM.</span></span>

<span data-ttu-id="51fea-220">Když vytvoříte nový disk spravované z hello portálu, můžete hello **typ účtu** hello typu disku se má toouse.</span><span class="sxs-lookup"><span data-stu-id="51fea-220">When you create a new managed disk from hello portal, you can choose hello **Account type** for hello type of disk you want toouse.</span></span> <span data-ttu-id="51fea-221">Mějte na paměti, že ne všechny dostupné disky jsou zobrazeny v rozevírací nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="51fea-221">Keep in mind that not all available disks are shown in hello drop-down menu.</span></span> <span data-ttu-id="51fea-222">Když vyberete konkrétní velikost virtuálního počítače, hello nabídce se zobrazí pouze hello úložiště k dispozici premium SKU, které jsou založeny na velikost tohoto virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="51fea-222">After you choose a particular VM size, hello menu shows only hello available premium storage SKUs that are based on that VM size.</span></span>

![Snímek obrazovky stránky spravovaných disků na hello](./media/oracle-design/premium_disk01.png)

<span data-ttu-id="51fea-224">Další informace najdete v tématu [vysoce výkonné úložiště Premium a spravované disky pro virtuální počítače](https://docs.microsoft.com/azure/storage/storage-premium-storage).</span><span class="sxs-lookup"><span data-stu-id="51fea-224">For more information, see [High-performance Premium Storage and managed disks for VMs](https://docs.microsoft.com/azure/storage/storage-premium-storage).</span></span>

<span data-ttu-id="51fea-225">Po dokončení konfigurace úložiště na virtuálním počítači, můžete chtít tooload testovací hello disků před vytvořením databáze.</span><span class="sxs-lookup"><span data-stu-id="51fea-225">After you configure your storage on a VM, you might want tooload test hello disks before creating a database.</span></span> <span data-ttu-id="51fea-226">Protože víte, že hello vstupně-výstupních operací míry z hlediska latence a propustnosti vám pomohou určit, pokud virtuální počítače hello podporují hello očekává propustnost s latencí cíle.</span><span class="sxs-lookup"><span data-stu-id="51fea-226">Knowing hello I/O rate in terms of both latency and throughput can help you determine if hello VMs support hello expected throughput with latency targets.</span></span>

<span data-ttu-id="51fea-227">Existuje několik nástrojů pro zatížení testování aplikací, jako je Oracle Orion, Sysbench a Fio.</span><span class="sxs-lookup"><span data-stu-id="51fea-227">There are a number of tools for application load testing, such as Oracle Orion, Sysbench, and Fio.</span></span>

<span data-ttu-id="51fea-228">Znovu spusťte hello zátěžový test, poté, co nasadíte databázi Oracle.</span><span class="sxs-lookup"><span data-stu-id="51fea-228">Run hello load test again after you've deployed an Oracle database.</span></span> <span data-ttu-id="51fea-229">Spuštění úlohy obyčejnými a špičkovým a hello zobrazit výsledky hello směrného plánu vašeho prostředí.</span><span class="sxs-lookup"><span data-stu-id="51fea-229">Start your regular and peak workloads, and hello results show you hello baseline of your environment.</span></span>

<span data-ttu-id="51fea-230">Může to být důležitější toosize hello úložiště založené na rychlost IOPS hello než velikost úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="51fea-230">It might be more important toosize hello storage based on hello IOPS rate rather than hello storage size.</span></span> <span data-ttu-id="51fea-231">Například v případě potřeby hello IOPS je 5 000, ale potřebujete jenom 200 GB může stále získáte hello P30 třída premium disku i když se dodává s více než 200 GB úložiště.</span><span class="sxs-lookup"><span data-stu-id="51fea-231">For example, if hello required IOPS is 5,000, but you only need 200 GB, you might still get hello P30 class premium disk even though it comes with more than 200 GB of storage.</span></span>

<span data-ttu-id="51fea-232">míra IOPS Hello je možné získat z hello AWR sestavy.</span><span class="sxs-lookup"><span data-stu-id="51fea-232">hello IOPS rate can be obtained from hello AWR report.</span></span> <span data-ttu-id="51fea-233">Je dáno hello opakování protokolu, fyzických čtení a zápisů rychlost.</span><span class="sxs-lookup"><span data-stu-id="51fea-233">It's determined by hello redo log, physical reads, and writes rate.</span></span>

![Snímek obrazovky stránky sestavy AWR hello](./media/oracle-design/awr_report.png)

<span data-ttu-id="51fea-235">Například velikost znovu hello je 12,200,000 bajtů za sekundu, která je rovna too11.63 MB/s.</span><span class="sxs-lookup"><span data-stu-id="51fea-235">For example, hello redo size is 12,200,000 bytes per second, which is equal too11.63 MBPs.</span></span>
<span data-ttu-id="51fea-236">Hello IOPS je 12,200,000 / 2,358 = 5,174.</span><span class="sxs-lookup"><span data-stu-id="51fea-236">hello IOPS is 12,200,000 / 2,358 = 5,174.</span></span>

<span data-ttu-id="51fea-237">Až budete mít přehledné informace o hello vstupně-výstupní požadavky, můžete zvolit kombinaci jednotek, které jsou nejlépe hodí toomeet tyto požadavky.</span><span class="sxs-lookup"><span data-stu-id="51fea-237">After you have a clear picture of hello I/O requirements, you can choose a combination of drives that are best suited toomeet those requirements.</span></span>

<span data-ttu-id="51fea-238">**Recommendations** (Doporučení)</span><span class="sxs-lookup"><span data-stu-id="51fea-238">**Recommendations**</span></span>

- <span data-ttu-id="51fea-239">Pro data tabulkového prostoru rozloženy hello vstupně-výstupní úlohy počet disků pomocí úložiště spravovaný nebo Oracle ASM.</span><span class="sxs-lookup"><span data-stu-id="51fea-239">For data tablespace, spread hello I/O workload across a number of disks by using managed storage or Oracle ASM.</span></span>
- <span data-ttu-id="51fea-240">Jak pro operace náročné na čtení a zápisu náročných zvyšuje velikost bloku hello vstupně-výstupních operací, přidejte další datové disky.</span><span class="sxs-lookup"><span data-stu-id="51fea-240">As hello I/O block size increases for read-intensive and write-intensive operations, add more data disks.</span></span>
- <span data-ttu-id="51fea-241">Zvětšete velikost bloku hello u velkých sekvenčních procesů.</span><span class="sxs-lookup"><span data-stu-id="51fea-241">Increase hello block size for large sequential processes.</span></span>
- <span data-ttu-id="51fea-242">Používejte data komprese tooreduce vstupně-výstupní operace (pro data a indexů).</span><span class="sxs-lookup"><span data-stu-id="51fea-242">Use data compression tooreduce I/O (for both data and indexes).</span></span>
- <span data-ttu-id="51fea-243">Oddělené opakování protokoly, systém a temps a vrátit zpět TS na samostatné datové disky.</span><span class="sxs-lookup"><span data-stu-id="51fea-243">Separate redo logs, system, and temps, and undo TS on separate data disks.</span></span>
- <span data-ttu-id="51fea-244">Umístěte všechny soubory aplikace na výchozí disky operačního systému (/ dev/sda).</span><span class="sxs-lookup"><span data-stu-id="51fea-244">Don't put any application files on default OS disks (/dev/sda).</span></span> <span data-ttu-id="51fea-245">Tyto disky nejsou optimalizovány pro rychlé virtuální počítač nemusí časy spuštění a poskytují dobrý výkon pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="51fea-245">These disks aren't optimized for fast VM boot times, and they might not provide good performance for your application.</span></span>

### <a name="disk-cache-settings"></a><span data-ttu-id="51fea-246">Nastavení mezipaměti na disku</span><span class="sxs-lookup"><span data-stu-id="51fea-246">Disk cache settings</span></span>

<span data-ttu-id="51fea-247">Existují tři možnosti pro ukládání do mezipaměti hostitele:</span><span class="sxs-lookup"><span data-stu-id="51fea-247">There are three options for host caching:</span></span>

- <span data-ttu-id="51fea-248">*Jen pro čtení*: všechny požadavky jsou uložená v mezipaměti pro budoucí čtení.</span><span class="sxs-lookup"><span data-stu-id="51fea-248">*Read-only*: All requests are cached for future reads.</span></span> <span data-ttu-id="51fea-249">Jsou trvalé všech zápisů přímo tooAzure úložiště objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="51fea-249">All writes are persisted directly tooAzure Blob storage.</span></span>

- <span data-ttu-id="51fea-250">*Čtení a zápis*: Toto je "čtení napřed" algoritmu.</span><span class="sxs-lookup"><span data-stu-id="51fea-250">*Read-write*: This is a “read-ahead” algorithm.</span></span> <span data-ttu-id="51fea-251">Hello čtení a zápisů jsou uložená v mezipaměti pro budoucí čtení.</span><span class="sxs-lookup"><span data-stu-id="51fea-251">hello reads and writes are cached for future reads.</span></span> <span data-ttu-id="51fea-252">Provede zápis non zápisu prostřednictvím jsou nejprve trvalé toohello místní mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="51fea-252">Non-write-through writes are persisted toohello local cache first.</span></span> <span data-ttu-id="51fea-253">Pro systém SQL Server jsou zápisy tooAzure trvalého úložiště, protože používá přímým zápisem.</span><span class="sxs-lookup"><span data-stu-id="51fea-253">For SQL Server, writes are persisted tooAzure Storage because it uses write-through.</span></span> <span data-ttu-id="51fea-254">Také poskytuje nejnižší latenci disku hello pro lehké úlohy.</span><span class="sxs-lookup"><span data-stu-id="51fea-254">It also provides hello lowest disk latency for light workloads.</span></span>

- <span data-ttu-id="51fea-255">*Žádný* (zakázáno): pomocí této možnosti můžete obejít hello mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="51fea-255">*None* (disabled): By using this option, you can bypass hello cache.</span></span> <span data-ttu-id="51fea-256">Všechna data hello je přenášená toodisk a trvalé tooAzure úložiště.</span><span class="sxs-lookup"><span data-stu-id="51fea-256">All hello data is transferred toodisk and persisted tooAzure Storage.</span></span> <span data-ttu-id="51fea-257">Tato metoda poskytuje hello nejvyšší rychlost vstupně-výstupních operací, pro zatížení s intenzivním vstupně-výstupních operací.</span><span class="sxs-lookup"><span data-stu-id="51fea-257">This method gives you hello highest I/O rate for I/O intensive workloads.</span></span> <span data-ttu-id="51fea-258">Budete také potřebovat tootake "transakce náklady" v úvahu.</span><span class="sxs-lookup"><span data-stu-id="51fea-258">You also need tootake “transaction cost” into consideration.</span></span>

<span data-ttu-id="51fea-259">**Recommendations** (Doporučení)</span><span class="sxs-lookup"><span data-stu-id="51fea-259">**Recommendations**</span></span>

<span data-ttu-id="51fea-260">toomaximize hello propustnost, doporučujeme začínat **žádné** pro použití mezipaměti u hostitele.</span><span class="sxs-lookup"><span data-stu-id="51fea-260">toomaximize hello throughput, we recommend that you  start with **None** for host caching.</span></span> <span data-ttu-id="51fea-261">Pro Storage úrovně Premium, mějte překážek"hello" je nutné zakázat, když připojíte hello systém souborů s hello **jen pro čtení** nebo **žádné** možnosti.</span><span class="sxs-lookup"><span data-stu-id="51fea-261">For Premium Storage, keep in mind that you must disable hello "barriers" when you mount hello file system with hello **ReadOnly** or **None** options.</span></span> <span data-ttu-id="51fea-262">Aktualizujte soubor /etc/fstab hello s hello UUID toohello disky.</span><span class="sxs-lookup"><span data-stu-id="51fea-262">Update hello /etc/fstab file with hello UUID toohello disks.</span></span>

<span data-ttu-id="51fea-263">Další informace najdete v tématu [Premium úložiště pro virtuální počítače s Linuxem](https://docs.microsoft.com/azure/storage/storage-premium-storage#premium-storage-for-linux-vms).</span><span class="sxs-lookup"><span data-stu-id="51fea-263">For more information, see [Premium Storage for Linux VMs](https://docs.microsoft.com/azure/storage/storage-premium-storage#premium-storage-for-linux-vms).</span></span>

![Snímek obrazovky stránky spravovaných disků na hello](./media/oracle-design/premium_disk02.png)

- <span data-ttu-id="51fea-265">Pro disky operačního systému, použít výchozí **pro čtení a zápis** ukládání do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="51fea-265">For OS disks, use default **Read/Write** caching.</span></span>
- <span data-ttu-id="51fea-266">Pro systém, TEMP a vrácení zpět pomocí **žádné** pro ukládání do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="51fea-266">For SYSTEM, TEMP, and UNDO use **None** for caching.</span></span>
- <span data-ttu-id="51fea-267">Pro DATA, použijte **žádné** pro ukládání do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="51fea-267">For DATA, use **None** for caching.</span></span> <span data-ttu-id="51fea-268">Ale pokud vaše databáze je jen pro čtení nebo náročné na čtení, použijte **jen pro čtení** ukládání do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="51fea-268">But if your database is read-only or read-intensive, use **Read-only** caching.</span></span>

<span data-ttu-id="51fea-269">Po uložení nastavení disku vaše data nelze změnit nastavení mezipaměti hostitele hello, pokud odpojit hello jednotku v hello úroveň operačního systému a znovu ji připojte po provedení hello změnit.</span><span class="sxs-lookup"><span data-stu-id="51fea-269">After your data disk setting is saved, you can't change hello host cache setting unless you unmount hello drive at hello OS level and then remount it after you've made hello change.</span></span>


## <a name="security"></a><span data-ttu-id="51fea-270">Zabezpečení</span><span class="sxs-lookup"><span data-stu-id="51fea-270">Security</span></span>

<span data-ttu-id="51fea-271">Po nastavení a konfiguraci prostředí Azure, dalším krokem hello je toosecure vaší sítě.</span><span class="sxs-lookup"><span data-stu-id="51fea-271">After you have set up and configured your Azure environment, hello next step is toosecure your network.</span></span> <span data-ttu-id="51fea-272">Tady jsou některá doporučení:</span><span class="sxs-lookup"><span data-stu-id="51fea-272">Here are some recommendations:</span></span>

- <span data-ttu-id="51fea-273">*Zásady skupiny NSG*: NSG může být definovaná podsíť nebo síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="51fea-273">*NSG policy*: NSG can be defined by a subnet or NIC.</span></span> <span data-ttu-id="51fea-274">Je jednodušší toocontrol přístupu na úrovni podsítě hello pro zabezpečení a vynutit směrování pro věcmi, jako jsou brány firewall pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="51fea-274">It's simpler toocontrol access at hello subnet level both for security and force routing for things like application firewalls.</span></span>

- <span data-ttu-id="51fea-275">*Jumpbox*: lépe zabezpečeného přístupu správci nesmí připojovat přímo toohello aplikační služby nebo databáze.</span><span class="sxs-lookup"><span data-stu-id="51fea-275">*Jumpbox*: For more secure access, administrators should not directly connect toohello application service or database.</span></span> <span data-ttu-id="51fea-276">Jumpbox slouží jako média mezi hello správce počítače a prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="51fea-276">A jumpbox is used as a media between hello administrator machine and Azure resources.</span></span>
<span data-ttu-id="51fea-277">![Snímek obrazovky stránky topologie Jumpbox hello](./media/oracle-design/jumpbox.png)</span><span class="sxs-lookup"><span data-stu-id="51fea-277">![Screenshot of hello Jumpbox topology page](./media/oracle-design/jumpbox.png)</span></span>

    <span data-ttu-id="51fea-278">Hello správce počítače by měl nabízejí IP omezený přístup toohello pouze jumpbox.</span><span class="sxs-lookup"><span data-stu-id="51fea-278">hello administrator machine should offer IP-restricted access toohello jumpbox only.</span></span> <span data-ttu-id="51fea-279">Hello jumpbox by měl mít přístup toohello aplikace a databáze.</span><span class="sxs-lookup"><span data-stu-id="51fea-279">hello jumpbox should have access toohello application and database.</span></span>

- <span data-ttu-id="51fea-280">*Privátní sítě* (podsítě): doporučujeme mít hello aplikace služby a databáze v samostatných podsítích v tak lepší řízení lze nastavit pomocí zásad skupiny NSG.</span><span class="sxs-lookup"><span data-stu-id="51fea-280">*Private network* (subnets): We recommend that you have hello application service and database on separate subnets, so better control can be set by NSG policy.</span></span>


## <a name="additional-reading"></a><span data-ttu-id="51fea-281">Další čtení</span><span class="sxs-lookup"><span data-stu-id="51fea-281">Additional reading</span></span>

- [<span data-ttu-id="51fea-282">Konfigurace Oracle ASM</span><span class="sxs-lookup"><span data-stu-id="51fea-282">Configure Oracle ASM</span></span>](configure-oracle-asm.md)
- [<span data-ttu-id="51fea-283">Konfigurace Oracle Data Guard</span><span class="sxs-lookup"><span data-stu-id="51fea-283">Configure Oracle Data Guard</span></span>](configure-oracle-dataguard.md)
- [<span data-ttu-id="51fea-284">Konfigurace brány Golden Oracle</span><span class="sxs-lookup"><span data-stu-id="51fea-284">Configure Oracle Golden Gate</span></span>](configure-oracle-golden-gate.md)
- [<span data-ttu-id="51fea-285">Oracle zálohování a obnovení</span><span class="sxs-lookup"><span data-stu-id="51fea-285">Oracle backup and recovery</span></span>](oracle-backup-recovery.md)

## <a name="next-steps"></a><span data-ttu-id="51fea-286">Další kroky</span><span class="sxs-lookup"><span data-stu-id="51fea-286">Next steps</span></span>

- [<span data-ttu-id="51fea-287">Kurz: Vytvoření vysoce dostupné virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="51fea-287">Tutorial: Create highly available VMs</span></span>](../../linux/create-cli-complete.md)
- [<span data-ttu-id="51fea-288">Prozkoumejte ukázky rozhraní příkazového řádku Azure nasazení virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="51fea-288">Explore VM deployment Azure CLI samples</span></span>](../../linux/cli-samples.md)
