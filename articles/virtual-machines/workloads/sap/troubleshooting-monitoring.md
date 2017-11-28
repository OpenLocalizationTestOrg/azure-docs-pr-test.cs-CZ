---
title: "aaaTroubleshooting a monitorování systému SAP HANA v Azure (velké instance) | Microsoft Docs"
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
ms.openlocfilehash: 1f1cd35820e227fd99af495431cd4b826aa53600
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tootroubleshoot-and-monitor-sap-hana-large-instances-on-azure"></a><span data-ttu-id="dde8a-103">Jak tootroubleshoot a monitorování SAP HANA (velké instance) na Azure</span><span class="sxs-lookup"><span data-stu-id="dde8a-103">How tootroubleshoot and monitor SAP HANA (large instances) on Azure</span></span>


## <a name="monitoring-in-sap-hana-on-azure-large-instances"></a><span data-ttu-id="dde8a-104">Monitorování v SAP HANA v Azure (velké instance)</span><span class="sxs-lookup"><span data-stu-id="dde8a-104">Monitoring in SAP HANA on Azure (Large Instances)</span></span>

<span data-ttu-id="dde8a-105">SAP HANA v Azure (velké instance) se neliší od jiných IaaS nasazení – stačí toomonitor co hello operační systém a aplikace hello je dělat a jak tyto využívat hello následující prostředky:</span><span class="sxs-lookup"><span data-stu-id="dde8a-105">SAP HANA on Azure (Large Instances) is no different from any other IaaS deployment — you need toomonitor what hello OS and hello application is doing and how these consume hello following resources:</span></span>

- <span data-ttu-id="dde8a-106">Procesor</span><span class="sxs-lookup"><span data-stu-id="dde8a-106">CPU</span></span>
- <span data-ttu-id="dde8a-107">Memory (Paměť)</span><span class="sxs-lookup"><span data-stu-id="dde8a-107">Memory</span></span>
- <span data-ttu-id="dde8a-108">Šířka pásma sítě</span><span class="sxs-lookup"><span data-stu-id="dde8a-108">Network bandwidth</span></span>
- <span data-ttu-id="dde8a-109">Místo na disku</span><span class="sxs-lookup"><span data-stu-id="dde8a-109">Disk space</span></span>

<span data-ttu-id="dde8a-110">Jak pomocí Azure Virtual Machines, je nutné toofigure out jestli postačí hello třídy prostředků s názvem výše, nebo jestli tyto získat vyčerpané.</span><span class="sxs-lookup"><span data-stu-id="dde8a-110">As with Azure Virtual Machines you need toofigure out whether hello resource classes named above are sufficient, or whether these get depleted.</span></span> <span data-ttu-id="dde8a-111">Tady je podrobněji na každém z různých tříd hello:</span><span class="sxs-lookup"><span data-stu-id="dde8a-111">Here is more detail on each of hello different classes:</span></span>

<span data-ttu-id="dde8a-112">**Využití prostředků procesoru:** hello poměr, která SAP definována pro určité úlohy proti HANA je vynucené toomake, že by měl být dostatek procesoru prostředky k dispozici toowork prostřednictvím hello data, která je uložená v paměti.</span><span class="sxs-lookup"><span data-stu-id="dde8a-112">**CPU resource consumption:** hello ratio that SAP defined for certain workload against HANA is enforced toomake sure that there should be enough CPU resources available toowork through hello data that is stored in memory.</span></span> <span data-ttu-id="dde8a-113">Nicméně může být případech, kde HANA spotřebovává velké množství procesoru zpracování dotazů kvůli toomissing indexy nebo podobné problémy.</span><span class="sxs-lookup"><span data-stu-id="dde8a-113">Nevertheless, there might be cases where HANA consumes a lot of CPU executing queries due toomissing indexes or similar issues.</span></span> <span data-ttu-id="dde8a-114">To znamená, že je třeba sledovat využití prostředků procesoru hello HANA instance velké jednotky, jakož i spotřebovávají hello konkrétní HANA služby prostředků procesoru.</span><span class="sxs-lookup"><span data-stu-id="dde8a-114">This means you should monitor CPU resource consumption of hello HANA large instance unit as well as CPU resources consumed by hello specific HANA services.</span></span>

<span data-ttu-id="dde8a-115">**Využití paměti:** je důležité toomonitor z v rámci HANA i mimo HANA na jednotce hello.</span><span class="sxs-lookup"><span data-stu-id="dde8a-115">**Memory consumption:** Is important toomonitor from within HANA, as well as outside of HANA on hello unit.</span></span> <span data-ttu-id="dde8a-116">V rámci HANA sledujte, jak hello dat spotřebovává přidělené paměti v toostay pořadí v rámci hello požadované změny velikosti pokyny SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="dde8a-116">Within HANA, monitor how hello data is consuming HANA allocated memory in order toostay within hello required sizing guidelines of SAP.</span></span> <span data-ttu-id="dde8a-117">Chcete taky toomonitor využití paměti na hello velké Instance úrovně toomake se, že další nainstalovaný jiný HANA softwaru není využívat příliš mnoho paměti a proto pokouší s HANA paměti.</span><span class="sxs-lookup"><span data-stu-id="dde8a-117">You also want toomonitor memory consumption on hello Large Instance level toomake sure that additional installed non-HANA software does not consume too much memory, and therefore compete with HANA for memory.</span></span>

<span data-ttu-id="dde8a-118">**Šířka pásma sítě:** brány virtuální sítě Azure hello je omezena šířka pásma dat Přesun do hello virtuální síť Azure, takže je vhodné hello toomonitor hello data přijatá všechny virtuální počítače Azure v rámci virtuální sítě toofigure na tom, jak zavřít jsou toohello omezení hello Azure Brána SKU, které jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="dde8a-118">**Network bandwidth:** hello Azure VNet gateway is limited in bandwidth of data moving into hello Azure VNet, so it is helpful toomonitor hello data received by all hello Azure VMs within a VNet toofigure out how close you are toohello limits of hello Azure gateway SKU you selected.</span></span> <span data-ttu-id="dde8a-119">Na jednotce hello HANA velké Instance zkontrolujte smysl toomonitor příchozí a odchozí síťový provoz i a sledovat tookeep hello svazků, které jsou zpracovávány v čase.</span><span class="sxs-lookup"><span data-stu-id="dde8a-119">On hello HANA Large Instance unit, it does make sense toomonitor incoming and outgoing network traffic as well, and tookeep track of hello volumes that are handled over time.</span></span>

<span data-ttu-id="dde8a-120">**Místo na disku:** využívání místa na disku obvykle zvyšuje v čase.</span><span class="sxs-lookup"><span data-stu-id="dde8a-120">**Disk space:** Disk space consumption usually increases over time.</span></span> <span data-ttu-id="dde8a-121">Existuje mnoho důvodů pro to, ale většina všech: datový svazek se zvyšuje, provádění zálohování transakčního protokolu, ukládání trasovacích souborů a provádění úložiště snímků.</span><span class="sxs-lookup"><span data-stu-id="dde8a-121">There are many reasons for this, but most of all are: data volume increases, execution of transaction log backups, storing trace files, and performing storage snapshots.</span></span> <span data-ttu-id="dde8a-122">Proto je důležité toomonitor využití místa na disku a spravovat místo na disku hello spojené s jednotkou HANA velké Instance hello.</span><span class="sxs-lookup"><span data-stu-id="dde8a-122">Therefore, it is important toomonitor disk space usage and manage hello disk space associated with hello HANA Large Instance unit.</span></span>

## <a name="monitoring-and-troubleshooting-from-hana-side"></a><span data-ttu-id="dde8a-123">Monitorování a řešení potíží s ze strany HANA</span><span class="sxs-lookup"><span data-stu-id="dde8a-123">Monitoring and troubleshooting from HANA side</span></span>

<span data-ttu-id="dde8a-124">V pořadí tooeffectively analyzovat problémy související tooSAP HANA v Azure (velké instance), je užitečné toonarrow dolů hello hlavní příčinu problému.</span><span class="sxs-lookup"><span data-stu-id="dde8a-124">In order tooeffectively analyze problems related tooSAP HANA on Azure (Large Instances), it is useful toonarrow down hello root cause of a problem.</span></span> <span data-ttu-id="dde8a-125">SAP publikovali velké množství toohelp dokumentaci vám.</span><span class="sxs-lookup"><span data-stu-id="dde8a-125">SAP has published a large amount of documentation toohelp you.</span></span>

<span data-ttu-id="dde8a-126">Použít nejčastější dotazy související tooSAP HANA výkonu najdete v následující poznámky k SAP hello:</span><span class="sxs-lookup"><span data-stu-id="dde8a-126">Applicable FAQs related tooSAP HANA performance can be found in hello following SAP Notes:</span></span>

- [<span data-ttu-id="dde8a-127">Poznámka SAP #2222200 – nejčastější dotazy: SAP HANA sítě</span><span class="sxs-lookup"><span data-stu-id="dde8a-127">SAP Note #2222200 – FAQ: SAP HANA Network</span></span>](https://launchpad.support.sap.com/#/notes/2222200)
- [<span data-ttu-id="dde8a-128">Poznámka SAP #2100040 – nejčastější dotazy: SAP HANA procesoru</span><span class="sxs-lookup"><span data-stu-id="dde8a-128">SAP Note #2100040 – FAQ: SAP HANA CPU</span></span>](https://launchpad.support.sap.com/#/notes/0002100040)
- [<span data-ttu-id="dde8a-129">Poznámka SAP #199997 – nejčastější dotazy: SAP HANA paměti</span><span class="sxs-lookup"><span data-stu-id="dde8a-129">SAP Note #199997 – FAQ: SAP HANA Memory</span></span>](https://launchpad.support.sap.com/#/notes/2177064)
- [<span data-ttu-id="dde8a-130">Poznámka SAP #200000 – nejčastější dotazy: Optimalizace výkonu SAP HANA</span><span class="sxs-lookup"><span data-stu-id="dde8a-130">SAP Note #200000 – FAQ: SAP HANA Performance Optimization</span></span>](https://launchpad.support.sap.com/#/notes/2000000)
- [<span data-ttu-id="dde8a-131">Poznámka SAP #199930 – nejčastější dotazy: SAP HANA vstupně-výstupních operací analýzy</span><span class="sxs-lookup"><span data-stu-id="dde8a-131">SAP Note #199930 – FAQ: SAP HANA I/O Analysis</span></span>](https://launchpad.support.sap.com/#/notes/1999930)
- [<span data-ttu-id="dde8a-132">Poznámka SAP #2177064 – nejčastější dotazy: SAP HANA službu restartovat a dojde k chybě</span><span class="sxs-lookup"><span data-stu-id="dde8a-132">SAP Note #2177064 – FAQ: SAP HANA Service Restart and Crashes</span></span>](https://launchpad.support.sap.com/#/notes/2177064)

<span data-ttu-id="dde8a-133">**Výstrahy SAP HANA**</span><span class="sxs-lookup"><span data-stu-id="dde8a-133">**SAP HANA Alerts**</span></span>

<span data-ttu-id="dde8a-134">Jako první krok protokolech hello aktuální SAP HANA výstrahy.</span><span class="sxs-lookup"><span data-stu-id="dde8a-134">As a first step, check hello current SAP HANA alert logs.</span></span> <span data-ttu-id="dde8a-135">V systému SAP HANA Studio přejděte příliš**konzole pro správu: upozornění: Zobrazit: všechny výstrahy**.</span><span class="sxs-lookup"><span data-stu-id="dde8a-135">In SAP HANA Studio, go too**Administration Console: Alerts: Show: all alerts**.</span></span> <span data-ttu-id="dde8a-136">Na této kartě se zobrazí všechny výstrahy SAP HANA pro konkrétní hodnoty (Volná fyzická paměť, využití procesoru atd.), které spadají mimo hello nastavit minimální a maximální prahové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="dde8a-136">This tab will show all SAP HANA alerts for specific values (free physical memory, CPU utilization, etc.) that fall outside of hello set minimum and maximum thresholds.</span></span> <span data-ttu-id="dde8a-137">Ve výchozím nastavení kontroluje se automaticky aktualizují každých 15 minut.</span><span class="sxs-lookup"><span data-stu-id="dde8a-137">By default, checks are auto-refreshed every 15 minutes.</span></span>

![V systému SAP HANA Studio přejděte tooAdministration konzoly: upozornění: zobrazení: všechny výstrahy](./media/troubleshooting-monitoring/image1-show-alerts.png)

<span data-ttu-id="dde8a-139">**VYUŽITÍ PROCESORU**</span><span class="sxs-lookup"><span data-stu-id="dde8a-139">**CPU**</span></span>

<span data-ttu-id="dde8a-140">Pro aktivaci z důvodu nastavení prahové hodnoty tooimproper výstrahu řešení je tooreset toohello výchozí hodnota nebo přijatelnější prahovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="dde8a-140">For an alert triggered due tooimproper threshold setting, a resolution is tooreset toohello default value or a more reasonable threshold value.</span></span>

![Obnovit výchozí hodnotu toohello nebo přijatelnější prahová hodnota](./media/troubleshooting-monitoring/image2-cpu-utilization.png)

<span data-ttu-id="dde8a-142">Hello následující upozornění může znamenat problémy prostředků procesoru:</span><span class="sxs-lookup"><span data-stu-id="dde8a-142">hello following alerts may indicate CPU resource problems:</span></span>

- <span data-ttu-id="dde8a-143">Využití procesoru hostitele (výstrah 5)</span><span class="sxs-lookup"><span data-stu-id="dde8a-143">Host CPU Usage (Alert 5)</span></span>
- <span data-ttu-id="dde8a-144">Poslední operace úložného bodu (výstrah 28)</span><span class="sxs-lookup"><span data-stu-id="dde8a-144">Most recent savepoint operation (Alert 28)</span></span>
- <span data-ttu-id="dde8a-145">Doba trvání uloženého bodu (výstrah 54)</span><span class="sxs-lookup"><span data-stu-id="dde8a-145">Savepoint duration (Alert 54)</span></span>

<span data-ttu-id="dde8a-146">Můžete si všimnout vysoké spotřeby procesoru ve vaší databázi SAP HANA z jednoho z následujících hello:</span><span class="sxs-lookup"><span data-stu-id="dde8a-146">You may notice high CPU consumption on your SAP HANA database from one of hello following:</span></span>

- <span data-ttu-id="dde8a-147">Výstrahy 5 (využití procesoru hostitele) se vyvolá pro aktuální nebo posledních využití procesoru</span><span class="sxs-lookup"><span data-stu-id="dde8a-147">Alert 5 (Host CPU usage) is raised for current or past CPU usage</span></span>
- <span data-ttu-id="dde8a-148">Hello zobrazené na úvodní obrazovka přehled využití procesoru</span><span class="sxs-lookup"><span data-stu-id="dde8a-148">hello displayed CPU usage on hello overview screen</span></span>

![Zobrazí využití procesoru na úvodní obrazovka Přehled](./media/troubleshooting-monitoring/image3-cpu-usage.png)

<span data-ttu-id="dde8a-150">Hello zatížení graf může zobrazit vysoké využití procesoru, nebo vysoké spotřebu v posledních hello:</span><span class="sxs-lookup"><span data-stu-id="dde8a-150">hello Load graph might show high CPU consumption, or high consumption in hello past:</span></span>

![Hello zatížení graf může zobrazit vysoké využití procesoru, nebo vysoké spotřebu v posledních hello](./media/troubleshooting-monitoring/image4-load-graph.png)

<span data-ttu-id="dde8a-152">Výstraha aktivuje kvůli využití toohigh procesoru může být způsobeno několik důvodů, včetně, mimo jiné: provádění určitých transakcí, načítání dat, ukotvených úloh, dlouho běžící příkazů jazyka SQL a výkonu chybných dotazů (například s BW na HANA datové krychle).</span><span class="sxs-lookup"><span data-stu-id="dde8a-152">An alert triggered due toohigh CPU utilization could be caused by several reasons, including, but not limited to: execution of certain transactions, data loading, hanging of jobs, long running SQL statements, and bad query performance (for example, with BW on HANA cubes).</span></span>

<span data-ttu-id="dde8a-153">Odkazovat toohello [řešení potíží s SAP HANA: související způsobí, že využití procesoru a řešení](http://help.sap.com/saphelp_hanaplatform/helpdata/en/4f/bc915462db406aa2fe92b708b95189/content.htm?frameset=/en/db/6ca50424714af8b370960c04ce667b/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=46&amp;show_children=false) lokality podrobné kroky řešení problémů.</span><span class="sxs-lookup"><span data-stu-id="dde8a-153">Refer toohello [SAP HANA Troubleshooting: CPU Related Causes and Solutions](http://help.sap.com/saphelp_hanaplatform/helpdata/en/4f/bc915462db406aa2fe92b708b95189/content.htm?frameset=/en/db/6ca50424714af8b370960c04ce667b/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=46&amp;show_children=false) site for detailed troubleshooting steps.</span></span>

<span data-ttu-id="dde8a-154">**Operační systém**</span><span class="sxs-lookup"><span data-stu-id="dde8a-154">**Operating System**</span></span>

<span data-ttu-id="dde8a-155">Jedním z nejdůležitějších hello kontrol pro SAP HANA v systému Linux je toomake jistotu, že transparentní obrovské stránky je zakázáno najdete v tématu [SAP Poznámka #2131662 – transparentní obrovské stránky (THP) na serverech SAP HANA](https://launchpad.support.sap.com/#/notes/2131662).</span><span class="sxs-lookup"><span data-stu-id="dde8a-155">One of hello most important checks for SAP HANA on Linux is toomake sure that Transparent Huge Pages are disabled, see [SAP Note #2131662 – Transparent Huge Pages (THP) on SAP HANA Servers](https://launchpad.support.sap.com/#/notes/2131662).</span></span>

- <span data-ttu-id="dde8a-156">Můžete zkontrolovat, zda transparentní obrovské stránky jsou povoleny prostřednictvím hello následující příkaz Linux: **cat /sys/kernel/mm/transparent\_hugepage/povoleno**</span><span class="sxs-lookup"><span data-stu-id="dde8a-156">You can check if Transparent Huge Pages are enabled through hello following Linux command: **cat /sys/kernel/mm/transparent\_hugepage/enabled**</span></span>
- <span data-ttu-id="dde8a-157">Pokud _vždy_ je uzavřený v závorkách, jak je uvedeno níže, znamená, že jsou povoleny hello transparentní obrovské stránky: [vždy] madvise nikdy; Pokud _nikdy_ je uzavřený v závorkách, jak je uvedeno níže, znamená to, že hello průhledný Velký stránky jsou zakázány: vždy madvise [nikdy]</span><span class="sxs-lookup"><span data-stu-id="dde8a-157">If _always_ is enclosed in brackets as below, it means that hello Transparent Huge Pages are enabled: [always] madvise never; if _never_ is enclosed in brackets as below, it means that hello Transparent Huge Pages are disabled: always madvise [never]</span></span>

<span data-ttu-id="dde8a-158">Následující příkaz Linux Hello by měla vrátit nic: **ot. / min - qa | grep ulimit.**</span><span class="sxs-lookup"><span data-stu-id="dde8a-158">hello following Linux command should return nothing: **rpm -qa | grep ulimit.**</span></span> <span data-ttu-id="dde8a-159">Pokud se zobrazí _ulimit_ je nainstalovaný, odinstalovat okamžitě.</span><span class="sxs-lookup"><span data-stu-id="dde8a-159">If it appears _ulimit_ is installed, uninstall it immediately.</span></span>

<span data-ttu-id="dde8a-160">**Paměť**</span><span class="sxs-lookup"><span data-stu-id="dde8a-160">**Memory**</span></span>

<span data-ttu-id="dde8a-161">Můžete pozorovat tato šířka hello paměti přidělené hello SAP HANA databáze je vyšší, než se očekávalo.</span><span class="sxs-lookup"><span data-stu-id="dde8a-161">You may observe that hello amount of memory allocated by hello SAP HANA database is higher than expected.</span></span> <span data-ttu-id="dde8a-162">Hello následující výstrahy signalizují potíže s velké množství paměti:</span><span class="sxs-lookup"><span data-stu-id="dde8a-162">hello following alerts indicate issues with high memory usage:</span></span>

- <span data-ttu-id="dde8a-163">Využití hostitele fyzické paměti (výstrahy 1)</span><span class="sxs-lookup"><span data-stu-id="dde8a-163">Host physical memory usage (Alert 1)</span></span>
- <span data-ttu-id="dde8a-164">Využití paměti název serveru (upozornění 12)</span><span class="sxs-lookup"><span data-stu-id="dde8a-164">Memory usage of name server (Alert 12)</span></span>
- <span data-ttu-id="dde8a-165">Celkové využití paměti úložiště sloupec tabulky (výstrah 40)</span><span class="sxs-lookup"><span data-stu-id="dde8a-165">Total memory usage of Column Store tables (Alert 40)</span></span>
- <span data-ttu-id="dde8a-166">Využití paměti služby (výstrah 43)</span><span class="sxs-lookup"><span data-stu-id="dde8a-166">Memory usage of services (Alert 43)</span></span>
- <span data-ttu-id="dde8a-167">Využití paměti hlavní úložiště sloupec úložiště tabulek (45 výstrah)</span><span class="sxs-lookup"><span data-stu-id="dde8a-167">Memory usage of main storage of Column Store tables (Alert 45)</span></span>
- <span data-ttu-id="dde8a-168">Soubory modulu runtime výpisu (výstrah 46)</span><span class="sxs-lookup"><span data-stu-id="dde8a-168">Runtime dump files (Alert 46)</span></span>

<span data-ttu-id="dde8a-169">Odkazovat toohello [řešení potíží s SAP HANA: problémy s pamětí](http://help.sap.com/saphelp_hanaplatform/helpdata/en/db/6ca50424714af8b370960c04ce667b/content.htm?frameset=/en/59/5eaa513dde43758b51378ab3315ebb/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=26&amp;show_children=false) lokality podrobné kroky řešení problémů.</span><span class="sxs-lookup"><span data-stu-id="dde8a-169">Refer toohello [SAP HANA Troubleshooting: Memory Problems](http://help.sap.com/saphelp_hanaplatform/helpdata/en/db/6ca50424714af8b370960c04ce667b/content.htm?frameset=/en/59/5eaa513dde43758b51378ab3315ebb/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=26&amp;show_children=false) site for detailed troubleshooting steps.</span></span>

<span data-ttu-id="dde8a-170">**Síť**</span><span class="sxs-lookup"><span data-stu-id="dde8a-170">**Network**</span></span>

<span data-ttu-id="dde8a-171">Odkazovat příliš[SAP Poznámka #2081065 – řešení potíží s SAP HANA sítě](https://launchpad.support.sap.com/#/notes/2081065) a proveďte kroky v této Poznámka SAP řešení potíží se sítí hello.</span><span class="sxs-lookup"><span data-stu-id="dde8a-171">Refer too[SAP Note #2081065 – Troubleshooting SAP HANA Network](https://launchpad.support.sap.com/#/notes/2081065) and perform hello network troubleshooting steps in this SAP Note.</span></span>

1. <span data-ttu-id="dde8a-172">Analýza odezvy čas mezi serverem a klientem.</span><span class="sxs-lookup"><span data-stu-id="dde8a-172">Analyzing round-trip time between server and client.</span></span>
  <span data-ttu-id="dde8a-173">A.</span><span class="sxs-lookup"><span data-stu-id="dde8a-173">A.</span></span> <span data-ttu-id="dde8a-174">Spuštění skriptu SQL hello [ _HANA\_sítě\_klienti_](https://launchpad.support.sap.com/#/notes/1969700)_._</span><span class="sxs-lookup"><span data-stu-id="dde8a-174">Run hello SQL script [_HANA\_Network\_Clients_](https://launchpad.support.sap.com/#/notes/1969700)_._</span></span>
  
2. <span data-ttu-id="dde8a-175">Analýza internodium komunikace.</span><span class="sxs-lookup"><span data-stu-id="dde8a-175">Analyze internode communication.</span></span>
  <span data-ttu-id="dde8a-176">A.</span><span class="sxs-lookup"><span data-stu-id="dde8a-176">A.</span></span> <span data-ttu-id="dde8a-177">Spuštění skriptu SQL [ _HANA\_sítě\_služby_](https://launchpad.support.sap.com/#/notes/1969700)_._</span><span class="sxs-lookup"><span data-stu-id="dde8a-177">Run SQL script [_HANA\_Network\_Services_](https://launchpad.support.sap.com/#/notes/1969700)_._</span></span>

3. <span data-ttu-id="dde8a-178">Spusťte příkaz Linux **ifconfig** (hello výstup ukazuje, pokud se vyskytnou ztráty paketů).</span><span class="sxs-lookup"><span data-stu-id="dde8a-178">Run Linux command **ifconfig** (hello output shows if any packet losses are occurring).</span></span>
4. <span data-ttu-id="dde8a-179">Spusťte příkaz Linux **tcpdump**.</span><span class="sxs-lookup"><span data-stu-id="dde8a-179">Run Linux command **tcpdump**.</span></span>

<span data-ttu-id="dde8a-180">Můžete také použít s otevřeným zdrojem hello [IPERF](https://iperf.fr/) nástroje (nebo podobnou) výkon sítě toomeasure reálné aplikaci.</span><span class="sxs-lookup"><span data-stu-id="dde8a-180">Also, use hello open source [IPERF](https://iperf.fr/) tool (or similar) toomeasure real application network performance.</span></span>

<span data-ttu-id="dde8a-181">Odkazovat toohello [řešení potíží s SAP HANA: výkon sítě a problémů s připojením k](http://help.sap.com/saphelp_hanaplatform/helpdata/en/a3/ccdff1aedc4720acb24ed8826938b6/content.htm?frameset=/en/dc/6ff98fa36541e997e4c719a632cbd8/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=142&amp;show_children=false) lokality podrobné kroky řešení problémů.</span><span class="sxs-lookup"><span data-stu-id="dde8a-181">Refer toohello [SAP HANA Troubleshooting: Networking Performance and Connectivity Problems](http://help.sap.com/saphelp_hanaplatform/helpdata/en/a3/ccdff1aedc4720acb24ed8826938b6/content.htm?frameset=/en/dc/6ff98fa36541e997e4c719a632cbd8/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=142&amp;show_children=false) site for detailed troubleshooting steps.</span></span>

<span data-ttu-id="dde8a-182">**Úložiště**</span><span class="sxs-lookup"><span data-stu-id="dde8a-182">**Storage**</span></span>

<span data-ttu-id="dde8a-183">Z hlediska koncový uživatel aplikace (nebo hello systém jako celek) běží pomalu, neodpovídá nebo můžete i zdá se, že toohang Pokud dochází k potížím s výkonem vstupně-výstupní operace.</span><span class="sxs-lookup"><span data-stu-id="dde8a-183">From an end-user perspective, an application (or hello system as a whole) runs sluggishly, is unresponsive, or can even seem toohang if there are issues with I/O performance.</span></span> <span data-ttu-id="dde8a-184">V hello **svazky** kartě v SAP HANA Studio uvidíte hello připojené svazky a svazky, které jsou používány jednotlivých služeb.</span><span class="sxs-lookup"><span data-stu-id="dde8a-184">In hello **Volumes** tab in SAP HANA Studio, you can see hello attached volumes, and what volumes are used by each service.</span></span>

![Na kartě hello svazky v SAP HANA Studio vidíte, že hello připojené svazky a svazky, které jsou používány každé služby](./media/troubleshooting-monitoring/image5-volumes-tab-a.png)

<span data-ttu-id="dde8a-186">Připojené svazky v dolní části hello úvodní obrazovka, které se zobrazí podrobnosti o hello svazků, jako jsou soubory a vstupně-výstupních operací statistiky.</span><span class="sxs-lookup"><span data-stu-id="dde8a-186">Attached volumes in hello lower part of hello screen you can see details of hello volumes, such as files and I/O statistics.</span></span>

![Připojené svazky v dolní části hello úvodní obrazovka, které se zobrazí podrobnosti o hello svazků, jako jsou soubory a vstupně-výstupních operací statistiky](./media/troubleshooting-monitoring/image6-volumes-tab-b.png)

<span data-ttu-id="dde8a-188">Odkazovat toohello [řešení potíží s SAP HANA: vstupně-výstupních operací souvisejících hlavní příčiny a řešení](http://help.sap.com/saphelp_hanaplatform/helpdata/en/dc/6ff98fa36541e997e4c719a632cbd8/content.htm?frameset=/en/47/4cb08a715c42fe9f7cc5efdc599959/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=55&amp;show_children=false) a [řešení potíží s SAP HANA: disku související hlavní příčiny a řešení](http://help.sap.com/saphelp_hanaplatform/helpdata/en/47/4cb08a715c42fe9f7cc5efdc599959/content.htm?frameset=/en/44/3e1db4f73d42da859008df4f69e37a/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=53&amp;show_children=false) lokality podrobné kroky řešení problémů.</span><span class="sxs-lookup"><span data-stu-id="dde8a-188">Refer toohello [SAP HANA Troubleshooting: I/O Related Root Causes and Solutions](http://help.sap.com/saphelp_hanaplatform/helpdata/en/dc/6ff98fa36541e997e4c719a632cbd8/content.htm?frameset=/en/47/4cb08a715c42fe9f7cc5efdc599959/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=55&amp;show_children=false) and [SAP HANA Troubleshooting: Disk Related Root Causes and Solutions](http://help.sap.com/saphelp_hanaplatform/helpdata/en/47/4cb08a715c42fe9f7cc5efdc599959/content.htm?frameset=/en/44/3e1db4f73d42da859008df4f69e37a/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=53&amp;show_children=false) site for detailed troubleshooting steps.</span></span>

<span data-ttu-id="dde8a-189">**Diagnostické nástroje**</span><span class="sxs-lookup"><span data-stu-id="dde8a-189">**Diagnostic Tools**</span></span>

<span data-ttu-id="dde8a-190">Provést kontrolu stavu SAP HANA prostřednictvím HANA\_konfigurace\_Minichecks.</span><span class="sxs-lookup"><span data-stu-id="dde8a-190">Perform an SAP HANA Health Check through HANA\_Configuration\_Minichecks.</span></span> <span data-ttu-id="dde8a-191">Tento nástroj vrátí potenciálně kritickým technických problémů, které by měl mít bylo vyvoláno už jako výstrahy v SAP HANA Studio.</span><span class="sxs-lookup"><span data-stu-id="dde8a-191">This tool returns potentially critical technical issues that should have already been raised as alerts in SAP HANA Studio.</span></span>

<span data-ttu-id="dde8a-192">Odkazovat příliš[SAP Poznámka #1969700 – kolekce příkaz SQL pro SAP HANA](https://launchpad.support.sap.com/#/notes/1969700) a stáhnout hello SQL Statements.zip souboru připojené toothat Poznámka.</span><span class="sxs-lookup"><span data-stu-id="dde8a-192">Refer too[SAP Note #1969700 – SQL statement collection for SAP HANA](https://launchpad.support.sap.com/#/notes/1969700) and download hello SQL Statements.zip file attached toothat note.</span></span> <span data-ttu-id="dde8a-193">Uložte tento soubor .zip na místní pevný disk hello.</span><span class="sxs-lookup"><span data-stu-id="dde8a-193">Store this .zip file on hello local hard drive.</span></span>

<span data-ttu-id="dde8a-194">V nástroji Studio SAP HANA na hello **informace o systému** kartě, klikněte pravým tlačítkem na hello **název** sloupce a vyberte **příkazů SQL Import**.</span><span class="sxs-lookup"><span data-stu-id="dde8a-194">In SAP HANA Studio, on hello **System Information** tab, right-click in hello **Name** column and select **Import SQL Statements**.</span></span>

![V nástroji Studio SAP HANA na kartě hello informace o systému klikněte pravým tlačítkem na název sloupce hello a vyberte Import SQL příkazy](./media/troubleshooting-monitoring/image7-import-statements-a.png)

<span data-ttu-id="dde8a-196">Vyberte hello SQL Statements.zip souboru uložené místně a budou importovány do složky s hello odpovídajících příkazů SQL.</span><span class="sxs-lookup"><span data-stu-id="dde8a-196">Select hello SQL Statements.zip file stored locally, and a folder with hello corresponding SQL statements will be imported.</span></span> <span data-ttu-id="dde8a-197">V tomto okamžiku hello mnoho různých diagnostiky kontroly můžete spustit pomocí těchto příkazů SQL.</span><span class="sxs-lookup"><span data-stu-id="dde8a-197">At this point, hello many different diagnostic checks can be run with these SQL statements.</span></span>

<span data-ttu-id="dde8a-198">Například nároky na šířku pásma replikaci systému SAP HANA tootest, klikněte pravým tlačítkem na hello **šířky pásma** příkaz pod **replikace: šířky pásma** a vyberte **otevřete** v konzole SQL.</span><span class="sxs-lookup"><span data-stu-id="dde8a-198">For example, tootest SAP HANA System Replication bandwidth requirements, right-click hello **Bandwidth** statement under **Replication: Bandwidth** and select **Open** in SQL Console.</span></span>

<span data-ttu-id="dde8a-199">příkaz SQL Hello otevře povolení toobe vstupních parametrů (změna oddílu), změnit a potom spuštěn.</span><span class="sxs-lookup"><span data-stu-id="dde8a-199">hello complete SQL statement opens allowing input parameters (modification section) toobe changed and then executed.</span></span>

![příkaz SQL Hello otevře změnit a potom spuštěn povolení toobe vstupních parametrů (Změna část)](./media/troubleshooting-monitoring/image8-import-statements-b.png)

<span data-ttu-id="dde8a-201">Dalším příkladem je klikněte pravým tlačítkem na v příkazech hello pod **replikace: Přehled**.</span><span class="sxs-lookup"><span data-stu-id="dde8a-201">Another example is right-clicking on hello statements under **Replication: Overview**.</span></span> <span data-ttu-id="dde8a-202">Vyberte **Execute** hello místní nabídce:</span><span class="sxs-lookup"><span data-stu-id="dde8a-202">Select **Execute** from hello context menu:</span></span>

![Dalším příkladem je klikněte pravým tlačítkem na v příkazech hello pod replikace: Přehled.](./media/troubleshooting-monitoring/image9-import-statements-c.png)

<span data-ttu-id="dde8a-205">Výsledkem je informace, které pomáhají při řešení problémů:</span><span class="sxs-lookup"><span data-stu-id="dde8a-205">This results in information that helps with troubleshooting:</span></span>

![Tato akce způsobí informace, které pomáhají při řešení problémů](./media/troubleshooting-monitoring/image10-import-statements-d.png)

<span data-ttu-id="dde8a-207">Hello stejné pro HANA\_konfigurace\_Minichecks a zkontrolujte všechny _X_ značky v hello _C_ sloupce (kritické).</span><span class="sxs-lookup"><span data-stu-id="dde8a-207">Do hello same for HANA\_Configuration\_Minichecks and check for any _X_ marks in hello _C_ (Critical) column.</span></span>

<span data-ttu-id="dde8a-208">Ukázka výstupu:</span><span class="sxs-lookup"><span data-stu-id="dde8a-208">Sample outputs:</span></span>

<span data-ttu-id="dde8a-209">**HANA\_konfigurace\_MiniChecks\_Rev102.01 + 1** pro kontroluje obecné SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="dde8a-209">**HANA\_Configuration\_MiniChecks\_Rev102.01+1** for general SAP HANA checks.</span></span>

![HANA\_konfigurace\_MiniChecks\_Rev102.01 + 1 pro obecné kontroly SAP HANA](./media/troubleshooting-monitoring/image11-configuration-minichecks.png)

<span data-ttu-id="dde8a-211">**HANA\_služby\_přehled** přehled co SAP HANA aktuálně běží služby.</span><span class="sxs-lookup"><span data-stu-id="dde8a-211">**HANA\_Services\_Overview** for an overview of what SAP HANA services are currently running.</span></span>

![HANA\_služby\_přehled přehled co SAP HANA aktuálně běží služby](./media/troubleshooting-monitoring/image12-services-overview.png)

<span data-ttu-id="dde8a-213">**HANA\_služby\_statistiky** pro SAP HANA služby informace (procesor, paměť, atd.).</span><span class="sxs-lookup"><span data-stu-id="dde8a-213">**HANA\_Services\_Statistics** for SAP HANA service information (CPU, memory, etc.).</span></span>

![<span data-ttu-id="dde8a-214">HANA\_služby\_statistiku pro SAP HANA informace o služby</span><span class="sxs-lookup"><span data-stu-id="dde8a-214">HANA\_Services\_Statistics for SAP HANA service information</span></span> ](./media/troubleshooting-monitoring/image13-services-statistics.png)

<span data-ttu-id="dde8a-215">**HANA\_konfigurace\_přehled\_Rev110 +** obecné informace o instanci hello SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="dde8a-215">**HANA\_Configuration\_Overview\_Rev110+** for general information on hello SAP HANA instance.</span></span>

![HANA\_konfigurace\_přehled\_Rev110 + obecné informace o instanci hello SAP HANA](./media/troubleshooting-monitoring/image14-configuration-overview.png)

<span data-ttu-id="dde8a-217">**HANA\_konfigurace\_parametry\_Rev70 +** toocheck parametry SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="dde8a-217">**HANA\_Configuration\_Parameters\_Rev70+** toocheck SAP HANA parameters.</span></span>

![HANA\_konfigurace\_parametry\_parametry SAP HANA toocheck Rev70 +](./media/troubleshooting-monitoring/image15-configuration-parameters.png)

