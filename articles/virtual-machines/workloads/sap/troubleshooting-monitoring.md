---
title: "Řešení potíží a monitorování SAP HANA v Azure (velké instance) | Microsoft Docs"
description: "Řešení potíží a monitorovat SAP HANA na Azure (velké instance)."
services: virtual-machines-linux
documentationcenter: 
author: RicksterCDN
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/01/2016
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ee5be707b443cbe42bf4a492d79390e534d4b91f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-troubleshoot-and-monitor-sap-hana-large-instances-on-azure"></a><span data-ttu-id="a853d-103">Postup řešení potíží a monitorovat SAP HANA (velké instance) na Azure</span><span class="sxs-lookup"><span data-stu-id="a853d-103">How to troubleshoot and monitor SAP HANA (large instances) on Azure</span></span>


## <a name="monitoring-in-sap-hana-on-azure-large-instances"></a><span data-ttu-id="a853d-104">Monitorování v SAP HANA v Azure (velké instance)</span><span class="sxs-lookup"><span data-stu-id="a853d-104">Monitoring in SAP HANA on Azure (Large Instances)</span></span>

<span data-ttu-id="a853d-105">Se neliší od jiných IaaS nasazení SAP HANA v Azure (velké instance), je nutné sledovat operačním systémem a aplikace je činnosti a jak tyto využívat následující prostředky:</span><span class="sxs-lookup"><span data-stu-id="a853d-105">SAP HANA on Azure (Large Instances) is no different from any other IaaS deployment — you need to monitor what the OS and the application is doing and how these consume the following resources:</span></span>

- <span data-ttu-id="a853d-106">Procesor</span><span class="sxs-lookup"><span data-stu-id="a853d-106">CPU</span></span>
- <span data-ttu-id="a853d-107">Memory (Paměť)</span><span class="sxs-lookup"><span data-stu-id="a853d-107">Memory</span></span>
- <span data-ttu-id="a853d-108">Šířka pásma sítě</span><span class="sxs-lookup"><span data-stu-id="a853d-108">Network bandwidth</span></span>
- <span data-ttu-id="a853d-109">Místo na disku</span><span class="sxs-lookup"><span data-stu-id="a853d-109">Disk space</span></span>

<span data-ttu-id="a853d-110">Jak pomocí Azure Virtual Machines, je třeba zjistit, jestli jsou dostatečná třídy prostředků s názvem výše, nebo jestli tyto získat vyčerpané.</span><span class="sxs-lookup"><span data-stu-id="a853d-110">As with Azure Virtual Machines you need to figure out whether the resource classes named above are sufficient, or whether these get depleted.</span></span> <span data-ttu-id="a853d-111">Tady je podrobněji na každém z různých tříd:</span><span class="sxs-lookup"><span data-stu-id="a853d-111">Here is more detail on each of the different classes:</span></span>

<span data-ttu-id="a853d-112">**Využití prostředků procesoru:** poměr, která SAP definována pro určité úlohy proti HANA se nevynutí a ujistěte se, že by měla být k dispozici fungovat prostřednictvím data, která je uložená v paměti dostatek prostředků procesoru.</span><span class="sxs-lookup"><span data-stu-id="a853d-112">**CPU resource consumption:** The ratio that SAP defined for certain workload against HANA is enforced to make sure that there should be enough CPU resources available to work through the data that is stored in memory.</span></span> <span data-ttu-id="a853d-113">Nicméně může být případech, kde HANA spotřebovává velké množství provádění dotazů procesoru z důvodu chybějící indexy nebo podobné problémy.</span><span class="sxs-lookup"><span data-stu-id="a853d-113">Nevertheless, there might be cases where HANA consumes a lot of CPU executing queries due to missing indexes or similar issues.</span></span> <span data-ttu-id="a853d-114">To znamená, že je třeba sledovat využití prostředků procesoru HANA velké instance jednotky a také spotřebovávají určitým službám HANA prostředků procesoru.</span><span class="sxs-lookup"><span data-stu-id="a853d-114">This means you should monitor CPU resource consumption of the HANA large instance unit as well as CPU resources consumed by the specific HANA services.</span></span>

<span data-ttu-id="a853d-115">**Využití paměti:** je důležité monitorovat z v rámci HANA i mimo HANA na jednotce.</span><span class="sxs-lookup"><span data-stu-id="a853d-115">**Memory consumption:** Is important to monitor from within HANA, as well as outside of HANA on the unit.</span></span> <span data-ttu-id="a853d-116">V rámci HANA sledujte, jak data spotřebovává přidělené paměti, aby bylo možné zůstat v požadované velikosti pokynů SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="a853d-116">Within HANA, monitor how the data is consuming HANA allocated memory in order to stay within the required sizing guidelines of SAP.</span></span> <span data-ttu-id="a853d-117">Chcete monitorovat využití paměti na úrovni Instance velké a ujistěte se, že další nainstalovaný jiný HANA softwaru nepodporuje využívat příliš mnoho paměti a proto pokouší s HANA paměti.</span><span class="sxs-lookup"><span data-stu-id="a853d-117">You also want to monitor memory consumption on the Large Instance level to make sure that additional installed non-HANA software does not consume too much memory, and therefore compete with HANA for memory.</span></span>

<span data-ttu-id="a853d-118">**Šířka pásma sítě:** brány virtuální sítě Azure je omezena šířka pásma dat Přesun do virtuální sítě Azure, takže je vhodné pro monitorování dat přijatých všechny virtuální počítače pro Azure v rámci virtuální sítě a pokuste se zjistit, jak jste se k omezení Azure skladová položka brány samoobslužné braný.</span><span class="sxs-lookup"><span data-stu-id="a853d-118">**Network bandwidth:** The Azure VNet gateway is limited in bandwidth of data moving into the Azure VNet, so it is helpful to monitor the data received by all the Azure VMs within a VNet to figure out how close you are to the limits of the Azure gateway SKU you selected.</span></span> <span data-ttu-id="a853d-119">Na jednotce HANA velké Instance ho dává smysl pro příchozí a odchozí síťový provoz i monitorování a ke sledování svazků, které jsou zpracovány v čase.</span><span class="sxs-lookup"><span data-stu-id="a853d-119">On the HANA Large Instance unit, it does make sense to monitor incoming and outgoing network traffic as well, and to keep track of the volumes that are handled over time.</span></span>

<span data-ttu-id="a853d-120">**Místo na disku:** využívání místa na disku obvykle zvyšuje v čase.</span><span class="sxs-lookup"><span data-stu-id="a853d-120">**Disk space:** Disk space consumption usually increases over time.</span></span> <span data-ttu-id="a853d-121">Existuje mnoho důvodů pro to, ale většina všech: datový svazek se zvyšuje, provádění zálohování transakčního protokolu, ukládání trasovacích souborů a provádění úložiště snímků.</span><span class="sxs-lookup"><span data-stu-id="a853d-121">There are many reasons for this, but most of all are: data volume increases, execution of transaction log backups, storing trace files, and performing storage snapshots.</span></span> <span data-ttu-id="a853d-122">Proto je důležité monitorovat využití místa na disku a spravovat místo na disku spojené s jednotkou HANA velké Instance.</span><span class="sxs-lookup"><span data-stu-id="a853d-122">Therefore, it is important to monitor disk space usage and manage the disk space associated with the HANA Large Instance unit.</span></span>

## <a name="monitoring-and-troubleshooting-from-hana-side"></a><span data-ttu-id="a853d-123">Monitorování a řešení potíží s ze strany HANA</span><span class="sxs-lookup"><span data-stu-id="a853d-123">Monitoring and troubleshooting from HANA side</span></span>

<span data-ttu-id="a853d-124">Chcete-li efektivně analyzovat problémy související s SAP HANA v Azure (velké instance), je užitečné zúžit hlavní příčinu problému.</span><span class="sxs-lookup"><span data-stu-id="a853d-124">In order to effectively analyze problems related to SAP HANA on Azure (Large Instances), it is useful to narrow down the root cause of a problem.</span></span> <span data-ttu-id="a853d-125">SAP publikovali velké množství dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="a853d-125">SAP has published a large amount of documentation to help you.</span></span>

<span data-ttu-id="a853d-126">Použít nejčastější dotazy týkající se výkonu SAP HANA naleznete v následující poznámky k SAP:</span><span class="sxs-lookup"><span data-stu-id="a853d-126">Applicable FAQs related to SAP HANA performance can be found in the following SAP Notes:</span></span>

- [<span data-ttu-id="a853d-127">Poznámka SAP #2222200 – nejčastější dotazy: SAP HANA sítě</span><span class="sxs-lookup"><span data-stu-id="a853d-127">SAP Note #2222200 – FAQ: SAP HANA Network</span></span>](https://launchpad.support.sap.com/#/notes/2222200)
- [<span data-ttu-id="a853d-128">Poznámka SAP #2100040 – nejčastější dotazy: SAP HANA procesoru</span><span class="sxs-lookup"><span data-stu-id="a853d-128">SAP Note #2100040 – FAQ: SAP HANA CPU</span></span>](https://launchpad.support.sap.com/#/notes/0002100040)
- [<span data-ttu-id="a853d-129">Poznámka SAP #199997 – nejčastější dotazy: SAP HANA paměti</span><span class="sxs-lookup"><span data-stu-id="a853d-129">SAP Note #199997 – FAQ: SAP HANA Memory</span></span>](https://launchpad.support.sap.com/#/notes/2177064)
- [<span data-ttu-id="a853d-130">Poznámka SAP #200000 – nejčastější dotazy: Optimalizace výkonu SAP HANA</span><span class="sxs-lookup"><span data-stu-id="a853d-130">SAP Note #200000 – FAQ: SAP HANA Performance Optimization</span></span>](https://launchpad.support.sap.com/#/notes/2000000)
- [<span data-ttu-id="a853d-131">Poznámka SAP #199930 – nejčastější dotazy: SAP HANA vstupně-výstupních operací analýzy</span><span class="sxs-lookup"><span data-stu-id="a853d-131">SAP Note #199930 – FAQ: SAP HANA I/O Analysis</span></span>](https://launchpad.support.sap.com/#/notes/1999930)
- [<span data-ttu-id="a853d-132">Poznámka SAP #2177064 – nejčastější dotazy: SAP HANA službu restartovat a dojde k chybě</span><span class="sxs-lookup"><span data-stu-id="a853d-132">SAP Note #2177064 – FAQ: SAP HANA Service Restart and Crashes</span></span>](https://launchpad.support.sap.com/#/notes/2177064)

<span data-ttu-id="a853d-133">**Výstrahy SAP HANA**</span><span class="sxs-lookup"><span data-stu-id="a853d-133">**SAP HANA Alerts**</span></span>

<span data-ttu-id="a853d-134">Jako první krok zkontrolujte aktuální výstrahy protokoly SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="a853d-134">As a first step, check the current SAP HANA alert logs.</span></span> <span data-ttu-id="a853d-135">V systému SAP HANA Studio přejděte do **konzole pro správu: upozornění: Zobrazit: všechny výstrahy**.</span><span class="sxs-lookup"><span data-stu-id="a853d-135">In SAP HANA Studio, go to **Administration Console: Alerts: Show: all alerts**.</span></span> <span data-ttu-id="a853d-136">Na této kartě se zobrazí všechny výstrahy SAP HANA pro konkrétní hodnoty (Volná fyzická paměť, využití procesoru atd.), které spadají mimo sadu minimální a maximální prahové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="a853d-136">This tab will show all SAP HANA alerts for specific values (free physical memory, CPU utilization, etc.) that fall outside of the set minimum and maximum thresholds.</span></span> <span data-ttu-id="a853d-137">Ve výchozím nastavení kontroluje se automaticky aktualizují každých 15 minut.</span><span class="sxs-lookup"><span data-stu-id="a853d-137">By default, checks are auto-refreshed every 15 minutes.</span></span>

![V systému SAP HANA Studio přejděte do konzoly pro správu: upozornění: zobrazení: všechny výstrahy](./media/troubleshooting-monitoring/image1-show-alerts.png)

<span data-ttu-id="a853d-139">**VYUŽITÍ PROCESORU**</span><span class="sxs-lookup"><span data-stu-id="a853d-139">**CPU**</span></span>

<span data-ttu-id="a853d-140">Pro aktivaci z důvodu nesprávné prahová hodnota nastavení výstrahu řešení je resetovat na výchozí hodnotu nebo přijatelnější prahovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="a853d-140">For an alert triggered due to improper threshold setting, a resolution is to reset to the default value or a more reasonable threshold value.</span></span>

![Obnoví na výchozí hodnotu nebo přijatelnější prahová hodnota](./media/troubleshooting-monitoring/image2-cpu-utilization.png)

<span data-ttu-id="a853d-142">Tyto výstrahy může znamenat problémy prostředků procesoru:</span><span class="sxs-lookup"><span data-stu-id="a853d-142">The following alerts may indicate CPU resource problems:</span></span>

- <span data-ttu-id="a853d-143">Využití procesoru hostitele (výstrah 5)</span><span class="sxs-lookup"><span data-stu-id="a853d-143">Host CPU Usage (Alert 5)</span></span>
- <span data-ttu-id="a853d-144">Poslední operace úložného bodu (výstrah 28)</span><span class="sxs-lookup"><span data-stu-id="a853d-144">Most recent savepoint operation (Alert 28)</span></span>
- <span data-ttu-id="a853d-145">Doba trvání uloženého bodu (výstrah 54)</span><span class="sxs-lookup"><span data-stu-id="a853d-145">Savepoint duration (Alert 54)</span></span>

<span data-ttu-id="a853d-146">Můžete si všimnout vysoké spotřeby procesoru ve vaší databázi SAP HANA z jednoho z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="a853d-146">You may notice high CPU consumption on your SAP HANA database from one of the following:</span></span>

- <span data-ttu-id="a853d-147">Výstrahy 5 (využití procesoru hostitele) se vyvolá pro aktuální nebo posledních využití procesoru</span><span class="sxs-lookup"><span data-stu-id="a853d-147">Alert 5 (Host CPU usage) is raised for current or past CPU usage</span></span>
- <span data-ttu-id="a853d-148">Zobrazit využití procesoru na obrazovce Přehled</span><span class="sxs-lookup"><span data-stu-id="a853d-148">The displayed CPU usage on the overview screen</span></span>

![Zobrazit využití procesoru na obrazovce Přehled](./media/troubleshooting-monitoring/image3-cpu-usage.png)

<span data-ttu-id="a853d-150">Grafu, pro zatížení může zobrazit vysoké využití procesoru, nebo vysoké spotřebu v minulosti:</span><span class="sxs-lookup"><span data-stu-id="a853d-150">The Load graph might show high CPU consumption, or high consumption in the past:</span></span>

![Grafu, pro zatížení může zobrazit vysoké využití procesoru, nebo vysoké spotřebu v minulosti.](./media/troubleshooting-monitoring/image4-load-graph.png)

<span data-ttu-id="a853d-152">Výstraha aktivuje z důvodu vysoké využití procesoru může být způsobeno několik důvodů, včetně, mimo jiné: provádění určitých transakcí, načítání dat, ukotvených úloh, dlouho běžící příkazů jazyka SQL a výkonu chybných dotazů (například s BW na HANA datové krychle).</span><span class="sxs-lookup"><span data-stu-id="a853d-152">An alert triggered due to high CPU utilization could be caused by several reasons, including, but not limited to: execution of certain transactions, data loading, hanging of jobs, long running SQL statements, and bad query performance (for example, with BW on HANA cubes).</span></span>

<span data-ttu-id="a853d-153">Odkazovat [řešení potíží s SAP HANA: související způsobí, že využití procesoru a řešení](http://help.sap.com/saphelp_hanaplatform/helpdata/en/4f/bc915462db406aa2fe92b708b95189/content.htm?frameset=/en/db/6ca50424714af8b370960c04ce667b/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=46&amp;show_children=false) lokality podrobné kroky řešení problémů.</span><span class="sxs-lookup"><span data-stu-id="a853d-153">Refer to the [SAP HANA Troubleshooting: CPU Related Causes and Solutions](http://help.sap.com/saphelp_hanaplatform/helpdata/en/4f/bc915462db406aa2fe92b708b95189/content.htm?frameset=/en/db/6ca50424714af8b370960c04ce667b/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=46&amp;show_children=false) site for detailed troubleshooting steps.</span></span>

<span data-ttu-id="a853d-154">**Operační systém**</span><span class="sxs-lookup"><span data-stu-id="a853d-154">**Operating System**</span></span>

<span data-ttu-id="a853d-155">Jedním z nejdůležitějších kontroly pro SAP HANA v systému Linux se ujistěte, že transparentní obrovské stránky je zakázáno, najdete v článku [SAP Poznámka #2131662 – transparentní obrovské stránky (THP) na serverech SAP HANA](https://launchpad.support.sap.com/#/notes/2131662).</span><span class="sxs-lookup"><span data-stu-id="a853d-155">One of the most important checks for SAP HANA on Linux is to make sure that Transparent Huge Pages are disabled, see [SAP Note #2131662 – Transparent Huge Pages (THP) on SAP HANA Servers](https://launchpad.support.sap.com/#/notes/2131662).</span></span>

- <span data-ttu-id="a853d-156">Můžete zkontrolovat, zda jsou povoleny transparentní obrovské stránky pomocí následujícího příkazu Linux: **cat /sys/kernel/mm/transparent\_hugepage/povoleno**</span><span class="sxs-lookup"><span data-stu-id="a853d-156">You can check if Transparent Huge Pages are enabled through the following Linux command: **cat /sys/kernel/mm/transparent\_hugepage/enabled**</span></span>
- <span data-ttu-id="a853d-157">Pokud _vždy_ je uzavřený v závorkách, jak je uvedeno níže, znamená to povolení transparentní obrovské stránky: [vždy] madvise nikdy; Pokud _nikdy_ je uzavřený v závorkách, jak je uvedeno níže, znamená to, že transparentní velký Stránky jsou zakázány: vždy madvise [nikdy]</span><span class="sxs-lookup"><span data-stu-id="a853d-157">If _always_ is enclosed in brackets as below, it means that the Transparent Huge Pages are enabled: [always] madvise never; if _never_ is enclosed in brackets as below, it means that the Transparent Huge Pages are disabled: always madvise [never]</span></span>

<span data-ttu-id="a853d-158">Příkaz Linux by měla vrátit nic: **ot. / min - qa | grep ulimit.**</span><span class="sxs-lookup"><span data-stu-id="a853d-158">The following Linux command should return nothing: **rpm -qa | grep ulimit.**</span></span> <span data-ttu-id="a853d-159">Pokud se zobrazí _ulimit_ je nainstalovaný, odinstalovat okamžitě.</span><span class="sxs-lookup"><span data-stu-id="a853d-159">If it appears _ulimit_ is installed, uninstall it immediately.</span></span>

<span data-ttu-id="a853d-160">**Paměť**</span><span class="sxs-lookup"><span data-stu-id="a853d-160">**Memory**</span></span>

<span data-ttu-id="a853d-161">Může sledovat, zda je množství paměti přidělené databázi SAP HANA vyšší, než se očekávalo.</span><span class="sxs-lookup"><span data-stu-id="a853d-161">You may observe that the amount of memory allocated by the SAP HANA database is higher than expected.</span></span> <span data-ttu-id="a853d-162">Tyto výstrahy signalizují potíže s velké množství paměti:</span><span class="sxs-lookup"><span data-stu-id="a853d-162">The following alerts indicate issues with high memory usage:</span></span>

- <span data-ttu-id="a853d-163">Využití hostitele fyzické paměti (výstrahy 1)</span><span class="sxs-lookup"><span data-stu-id="a853d-163">Host physical memory usage (Alert 1)</span></span>
- <span data-ttu-id="a853d-164">Využití paměti název serveru (upozornění 12)</span><span class="sxs-lookup"><span data-stu-id="a853d-164">Memory usage of name server (Alert 12)</span></span>
- <span data-ttu-id="a853d-165">Celkové využití paměti úložiště sloupec tabulky (výstrah 40)</span><span class="sxs-lookup"><span data-stu-id="a853d-165">Total memory usage of Column Store tables (Alert 40)</span></span>
- <span data-ttu-id="a853d-166">Využití paměti služby (výstrah 43)</span><span class="sxs-lookup"><span data-stu-id="a853d-166">Memory usage of services (Alert 43)</span></span>
- <span data-ttu-id="a853d-167">Využití paměti hlavní úložiště sloupec úložiště tabulek (45 výstrah)</span><span class="sxs-lookup"><span data-stu-id="a853d-167">Memory usage of main storage of Column Store tables (Alert 45)</span></span>
- <span data-ttu-id="a853d-168">Soubory modulu runtime výpisu (výstrah 46)</span><span class="sxs-lookup"><span data-stu-id="a853d-168">Runtime dump files (Alert 46)</span></span>

<span data-ttu-id="a853d-169">Odkazovat [řešení potíží s SAP HANA: problémy s pamětí](http://help.sap.com/saphelp_hanaplatform/helpdata/en/db/6ca50424714af8b370960c04ce667b/content.htm?frameset=/en/59/5eaa513dde43758b51378ab3315ebb/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=26&amp;show_children=false) lokality podrobné kroky řešení problémů.</span><span class="sxs-lookup"><span data-stu-id="a853d-169">Refer to the [SAP HANA Troubleshooting: Memory Problems](http://help.sap.com/saphelp_hanaplatform/helpdata/en/db/6ca50424714af8b370960c04ce667b/content.htm?frameset=/en/59/5eaa513dde43758b51378ab3315ebb/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=26&amp;show_children=false) site for detailed troubleshooting steps.</span></span>

<span data-ttu-id="a853d-170">**Síť**</span><span class="sxs-lookup"><span data-stu-id="a853d-170">**Network**</span></span>

<span data-ttu-id="a853d-171">Odkazovat na [SAP Poznámka #2081065 – řešení potíží s SAP HANA sítě](https://launchpad.support.sap.com/#/notes/2081065) a provádět sítě řešení potíží s kroky v této Poznámka SAP.</span><span class="sxs-lookup"><span data-stu-id="a853d-171">Refer to [SAP Note #2081065 – Troubleshooting SAP HANA Network](https://launchpad.support.sap.com/#/notes/2081065) and perform the network troubleshooting steps in this SAP Note.</span></span>

1. <span data-ttu-id="a853d-172">Analýza odezvy čas mezi serverem a klientem.</span><span class="sxs-lookup"><span data-stu-id="a853d-172">Analyzing round-trip time between server and client.</span></span>
  <span data-ttu-id="a853d-173">A.</span><span class="sxs-lookup"><span data-stu-id="a853d-173">A.</span></span> <span data-ttu-id="a853d-174">Spusťte skript SQL [ _HANA\_sítě\_klienti_](https://launchpad.support.sap.com/#/notes/1969700)_._</span><span class="sxs-lookup"><span data-stu-id="a853d-174">Run the SQL script [_HANA\_Network\_Clients_](https://launchpad.support.sap.com/#/notes/1969700)_._</span></span>
  
2. <span data-ttu-id="a853d-175">Analýza internodium komunikace.</span><span class="sxs-lookup"><span data-stu-id="a853d-175">Analyze internode communication.</span></span>
  <span data-ttu-id="a853d-176">A.</span><span class="sxs-lookup"><span data-stu-id="a853d-176">A.</span></span> <span data-ttu-id="a853d-177">Spuštění skriptu SQL [ _HANA\_sítě\_služby_](https://launchpad.support.sap.com/#/notes/1969700)_._</span><span class="sxs-lookup"><span data-stu-id="a853d-177">Run SQL script [_HANA\_Network\_Services_](https://launchpad.support.sap.com/#/notes/1969700)_._</span></span>

3. <span data-ttu-id="a853d-178">Spusťte příkaz Linux **ifconfig** (výstup zobrazuje, pokud se vyskytnou ztráty paketů).</span><span class="sxs-lookup"><span data-stu-id="a853d-178">Run Linux command **ifconfig** (the output shows if any packet losses are occurring).</span></span>
4. <span data-ttu-id="a853d-179">Spusťte příkaz Linux **tcpdump**.</span><span class="sxs-lookup"><span data-stu-id="a853d-179">Run Linux command **tcpdump**.</span></span>

<span data-ttu-id="a853d-180">Rovněž použijte open source [IPERF](https://iperf.fr/) nástroje (nebo podobnou) k měření výkonu sítě reálné aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a853d-180">Also, use the open source [IPERF](https://iperf.fr/) tool (or similar) to measure real application network performance.</span></span>

<span data-ttu-id="a853d-181">Odkazovat [řešení potíží s SAP HANA: výkon sítě a problémů s připojením k](http://help.sap.com/saphelp_hanaplatform/helpdata/en/a3/ccdff1aedc4720acb24ed8826938b6/content.htm?frameset=/en/dc/6ff98fa36541e997e4c719a632cbd8/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=142&amp;show_children=false) lokality podrobné kroky řešení problémů.</span><span class="sxs-lookup"><span data-stu-id="a853d-181">Refer to the [SAP HANA Troubleshooting: Networking Performance and Connectivity Problems](http://help.sap.com/saphelp_hanaplatform/helpdata/en/a3/ccdff1aedc4720acb24ed8826938b6/content.htm?frameset=/en/dc/6ff98fa36541e997e4c719a632cbd8/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=142&amp;show_children=false) site for detailed troubleshooting steps.</span></span>

<span data-ttu-id="a853d-182">**Storage**</span><span class="sxs-lookup"><span data-stu-id="a853d-182">**Storage**</span></span>

<span data-ttu-id="a853d-183">Z hlediska koncový uživatel aplikace (nebo na systém jako celek) běží pomalu, neodpovídá nebo můžete dokonce se zdá, že zablokuje, pokud dochází k potížím s výkonem vstupně-výstupní operace.</span><span class="sxs-lookup"><span data-stu-id="a853d-183">From an end-user perspective, an application (or the system as a whole) runs sluggishly, is unresponsive, or can even seem to hang if there are issues with I/O performance.</span></span> <span data-ttu-id="a853d-184">V **svazky** kartě v SAP HANA Studio uvidíte připojené svazky a svazky, které jsou používány jednotlivých služeb.</span><span class="sxs-lookup"><span data-stu-id="a853d-184">In the **Volumes** tab in SAP HANA Studio, you can see the attached volumes, and what volumes are used by each service.</span></span>

![Na kartě svazky v SAP HANA Studio uvidíte připojené svazky a svazky, které jsou používány každé služby](./media/troubleshooting-monitoring/image5-volumes-tab-a.png)

<span data-ttu-id="a853d-186">Připojené svazky v dolní části obrazovky se zobrazí podrobnosti o na svazcích, jako jsou soubory a vstupně-výstupních operací statistiky.</span><span class="sxs-lookup"><span data-stu-id="a853d-186">Attached volumes in the lower part of the screen you can see details of the volumes, such as files and I/O statistics.</span></span>

![Připojené svazky v dolní části obrazovky se zobrazí podrobnosti o na svazcích, jako jsou soubory a vstupně-výstupních operací statistiky](./media/troubleshooting-monitoring/image6-volumes-tab-b.png)

<span data-ttu-id="a853d-188">Odkazovat [řešení potíží s SAP HANA: vstupně-výstupních operací souvisejících hlavní příčiny a řešení](http://help.sap.com/saphelp_hanaplatform/helpdata/en/dc/6ff98fa36541e997e4c719a632cbd8/content.htm?frameset=/en/47/4cb08a715c42fe9f7cc5efdc599959/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=55&amp;show_children=false) a [řešení potíží s SAP HANA: disku související hlavní příčiny a řešení](http://help.sap.com/saphelp_hanaplatform/helpdata/en/47/4cb08a715c42fe9f7cc5efdc599959/content.htm?frameset=/en/44/3e1db4f73d42da859008df4f69e37a/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=53&amp;show_children=false) lokality podrobné kroky řešení problémů.</span><span class="sxs-lookup"><span data-stu-id="a853d-188">Refer to the [SAP HANA Troubleshooting: I/O Related Root Causes and Solutions](http://help.sap.com/saphelp_hanaplatform/helpdata/en/dc/6ff98fa36541e997e4c719a632cbd8/content.htm?frameset=/en/47/4cb08a715c42fe9f7cc5efdc599959/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=55&amp;show_children=false) and [SAP HANA Troubleshooting: Disk Related Root Causes and Solutions](http://help.sap.com/saphelp_hanaplatform/helpdata/en/47/4cb08a715c42fe9f7cc5efdc599959/content.htm?frameset=/en/44/3e1db4f73d42da859008df4f69e37a/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=53&amp;show_children=false) site for detailed troubleshooting steps.</span></span>

<span data-ttu-id="a853d-189">**Diagnostické nástroje**</span><span class="sxs-lookup"><span data-stu-id="a853d-189">**Diagnostic Tools**</span></span>

<span data-ttu-id="a853d-190">Provést kontrolu stavu SAP HANA prostřednictvím HANA\_konfigurace\_Minichecks.</span><span class="sxs-lookup"><span data-stu-id="a853d-190">Perform an SAP HANA Health Check through HANA\_Configuration\_Minichecks.</span></span> <span data-ttu-id="a853d-191">Tento nástroj vrátí potenciálně kritickým technických problémů, které by měl mít bylo vyvoláno už jako výstrahy v SAP HANA Studio.</span><span class="sxs-lookup"><span data-stu-id="a853d-191">This tool returns potentially critical technical issues that should have already been raised as alerts in SAP HANA Studio.</span></span>

<span data-ttu-id="a853d-192">Odkazovat na [SAP Poznámka #1969700 – kolekce příkaz SQL pro SAP HANA](https://launchpad.support.sap.com/#/notes/1969700) a stáhněte si soubor SQL Statements.zip poznámka připojena.</span><span class="sxs-lookup"><span data-stu-id="a853d-192">Refer to [SAP Note #1969700 – SQL statement collection for SAP HANA](https://launchpad.support.sap.com/#/notes/1969700) and download the SQL Statements.zip file attached to that note.</span></span> <span data-ttu-id="a853d-193">Uložte tento soubor .zip na místní pevný disk.</span><span class="sxs-lookup"><span data-stu-id="a853d-193">Store this .zip file on the local hard drive.</span></span>

<span data-ttu-id="a853d-194">V nástroji SAP HANA Studio na **informace o systému** kartě, klikněte pravým tlačítkem myši **název** sloupce a vyberte **příkazů SQL Import**.</span><span class="sxs-lookup"><span data-stu-id="a853d-194">In SAP HANA Studio, on the **System Information** tab, right-click in the **Name** column and select **Import SQL Statements**.</span></span>

![V systému SAP HANA Studio, na kartě informace o systému klikněte pravým tlačítkem na název sloupce a vyberte Import SQL příkazy](./media/troubleshooting-monitoring/image7-import-statements-a.png)

<span data-ttu-id="a853d-196">Vyberte soubor SQL Statements.zip uložené místně, a budou importovány do složky s odpovídajících příkazů SQL.</span><span class="sxs-lookup"><span data-stu-id="a853d-196">Select the SQL Statements.zip file stored locally, and a folder with the corresponding SQL statements will be imported.</span></span> <span data-ttu-id="a853d-197">V tomto okamžiku mnoho různých diagnostiky kontroly lze spustit pomocí těchto příkazů SQL.</span><span class="sxs-lookup"><span data-stu-id="a853d-197">At this point, the many different diagnostic checks can be run with these SQL statements.</span></span>

<span data-ttu-id="a853d-198">Například k testování replikace systému SAP HANA nároky na šířku pásma, klikněte pravým tlačítkem myši **šířky pásma** příkaz pod **replikace: šířky pásma** a vyberte **otevřete** v Konzola SQL.</span><span class="sxs-lookup"><span data-stu-id="a853d-198">For example, to test SAP HANA System Replication bandwidth requirements, right-click the **Bandwidth** statement under **Replication: Bandwidth** and select **Open** in SQL Console.</span></span>

<span data-ttu-id="a853d-199">Příkaz SQL otevře povolení vstupních parametrů (změna oddílu), změnit a potom spuštěn.</span><span class="sxs-lookup"><span data-stu-id="a853d-199">The complete SQL statement opens allowing input parameters (modification section) to be changed and then executed.</span></span>

![Příkaz SQL otevře povolení vstupních parametrů (změna oddílu), změnit a potom spuštěn](./media/troubleshooting-monitoring/image8-import-statements-b.png)

<span data-ttu-id="a853d-201">Dalším příkladem je klikněte pravým tlačítkem na v příkazech v části **replikace: Přehled**.</span><span class="sxs-lookup"><span data-stu-id="a853d-201">Another example is right-clicking on the statements under **Replication: Overview**.</span></span> <span data-ttu-id="a853d-202">Vyberte **Execute** v místní nabídce:</span><span class="sxs-lookup"><span data-stu-id="a853d-202">Select **Execute** from the context menu:</span></span>

![Dalším příkladem je klikněte pravým tlačítkem na v příkazech v části replikace: Přehled.](./media/troubleshooting-monitoring/image9-import-statements-c.png)

<span data-ttu-id="a853d-205">Výsledkem je informace, které pomáhají při řešení problémů:</span><span class="sxs-lookup"><span data-stu-id="a853d-205">This results in information that helps with troubleshooting:</span></span>

![Tato akce způsobí informace, které pomáhají při řešení problémů](./media/troubleshooting-monitoring/image10-import-statements-d.png)

<span data-ttu-id="a853d-207">Totéž proveďte pro HANA\_konfigurace\_Minichecks a zkontrolujte všechny _X_ označí v _C_ sloupce (kritické).</span><span class="sxs-lookup"><span data-stu-id="a853d-207">Do the same for HANA\_Configuration\_Minichecks and check for any _X_ marks in the _C_ (Critical) column.</span></span>

<span data-ttu-id="a853d-208">Ukázka výstupu:</span><span class="sxs-lookup"><span data-stu-id="a853d-208">Sample outputs:</span></span>

<span data-ttu-id="a853d-209">**HANA\_konfigurace\_MiniChecks\_Rev102.01 + 1** pro kontroluje obecné SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="a853d-209">**HANA\_Configuration\_MiniChecks\_Rev102.01+1** for general SAP HANA checks.</span></span>

![HANA\_konfigurace\_MiniChecks\_Rev102.01 + 1 pro obecné kontroly SAP HANA](./media/troubleshooting-monitoring/image11-configuration-minichecks.png)

<span data-ttu-id="a853d-211">**HANA\_služby\_přehled** přehled co SAP HANA aktuálně běží služby.</span><span class="sxs-lookup"><span data-stu-id="a853d-211">**HANA\_Services\_Overview** for an overview of what SAP HANA services are currently running.</span></span>

![HANA\_služby\_přehled přehled co SAP HANA aktuálně běží služby](./media/troubleshooting-monitoring/image12-services-overview.png)

<span data-ttu-id="a853d-213">**HANA\_služby\_statistiky** pro SAP HANA služby informace (procesor, paměť, atd.).</span><span class="sxs-lookup"><span data-stu-id="a853d-213">**HANA\_Services\_Statistics** for SAP HANA service information (CPU, memory, etc.).</span></span>

![<span data-ttu-id="a853d-214">HANA\_služby\_statistiku pro SAP HANA informace o služby</span><span class="sxs-lookup"><span data-stu-id="a853d-214">HANA\_Services\_Statistics for SAP HANA service information</span></span> ](./media/troubleshooting-monitoring/image13-services-statistics.png)

<span data-ttu-id="a853d-215">**HANA\_konfigurace\_přehled\_Rev110 +** obecné informace o instanci SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="a853d-215">**HANA\_Configuration\_Overview\_Rev110+** for general information on the SAP HANA instance.</span></span>

![HANA\_konfigurace\_přehled\_Rev110 + obecné informace o instanci SAP HANA](./media/troubleshooting-monitoring/image14-configuration-overview.png)

<span data-ttu-id="a853d-217">**HANA\_konfigurace\_parametry\_Rev70 +** Zkontrolujte parametry SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="a853d-217">**HANA\_Configuration\_Parameters\_Rev70+** to check SAP HANA parameters.</span></span>

![HANA\_konfigurace\_parametry\_Rev70 + Zkontrolujte parametry SAP HANA](./media/troubleshooting-monitoring/image15-configuration-parameters.png)

