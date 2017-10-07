---
title: "protokoly aaaVisualizing toku skupinu zabezpečení sítě Azure s Power BI | Microsoft Docs"
description: "Tato stránka popisuje, jak toovisualize NSG toku protokoly s Power BI."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 1e4f95fa-f5f0-4e03-bc25-008fbfc4934c
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: d9adcf256df8fed68c39be1a026ca64cc6b5c6d5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="visualizing-network-security-group-flow-logs-with-power-bi"></a><span data-ttu-id="27064-103">Skupina zabezpečení sítě visualizing toku protokoly s Power BI</span><span class="sxs-lookup"><span data-stu-id="27064-103">Visualizing Network Security Group flow logs with Power BI</span></span>

<span data-ttu-id="27064-104">Skupina zabezpečení sítě toku protokoly povolit tooview informace o příchozí a odchozí provoz IP na skupiny zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="27064-104">Network Security Group flow logs allow you tooview information about ingress and egress IP traffic on Network Security Groups.</span></span> <span data-ttu-id="27064-105">Tyto protokoly toku zobrazit odchozí a příchozí toky na základě na pravidlo, hello seskupování hello toku platí pro, 5-řazené kolekce členů informace o toku hello (zdrojové nebo cílové IP adresy, zdrojový nebo cílový Port, protokol), a pokud bylo povolené nebo zakázané hello přenosy.</span><span class="sxs-lookup"><span data-stu-id="27064-105">These flow logs show outbound and inbound flows on a per rule basis, hello NIC hello flow applies to, 5-tuple information about hello flow (Source/Destination IP, Source/Destination Port, Protocol), and if hello traffic was allowed or denied.</span></span>

<span data-ttu-id="27064-106">Může být obtížné toogain přehledy toku protokolování dat tak, že ručně hello soubory protokolu.</span><span class="sxs-lookup"><span data-stu-id="27064-106">It can be difficult toogain insights into flow logging data by manually searching hello log files.</span></span> <span data-ttu-id="27064-107">V tomto článku Poskytujeme řešení toovisualize vaší poslední toku protokoly a další informace o přenosu v síti.</span><span class="sxs-lookup"><span data-stu-id="27064-107">In this article, we provide a solution toovisualize your most recent flow logs and learn about traffic on your network.</span></span>

## <a name="scenario"></a><span data-ttu-id="27064-108">Scénář</span><span class="sxs-lookup"><span data-stu-id="27064-108">Scenario</span></span>

<span data-ttu-id="27064-109">V následujícím scénáři hello se nám připojit Power BI desktop toohello úložiště účet, který jsme nakonfigurovali jako hello jímku pro naše data NSG toku protokolování.</span><span class="sxs-lookup"><span data-stu-id="27064-109">In hello following scenario, we connect Power BI desktop toohello storage account we have configured as hello sink for our NSG Flow Logging data.</span></span> <span data-ttu-id="27064-110">Po tooour účet úložiště se nám připojit, Power BI se stáhne a analyzuje hello protokoly tooprovide vizuální reprezentace hello provoz, který je zaznamenána podle skupin zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="27064-110">After we connect tooour storage account, Power BI downloads and parses hello logs tooprovide a visual representation of hello traffic that is logged by Network Security groups.</span></span>

<span data-ttu-id="27064-111">Pomocí hello vizuály zadaný v hello šablonu, kterou můžete zkontrolovat:</span><span class="sxs-lookup"><span data-stu-id="27064-111">Using hello visuals supplied in hello template you can examine:</span></span>

* <span data-ttu-id="27064-112">Horní Talkers</span><span class="sxs-lookup"><span data-stu-id="27064-112">Top Talkers</span></span>
* <span data-ttu-id="27064-113">Časových řad toku dat rozhodnutím směr a pravidla</span><span class="sxs-lookup"><span data-stu-id="27064-113">Time Series Flow Data by direction and rule decision</span></span>
* <span data-ttu-id="27064-114">Toky podle adresy MAC rozhraní sítě</span><span class="sxs-lookup"><span data-stu-id="27064-114">Flows by Network Interface MAC address</span></span>
* <span data-ttu-id="27064-115">Toky NSG a pravidla</span><span class="sxs-lookup"><span data-stu-id="27064-115">Flows by NSG and Rule</span></span>
* <span data-ttu-id="27064-116">Toky podle cílový Port</span><span class="sxs-lookup"><span data-stu-id="27064-116">Flows by Destination Port</span></span>

<span data-ttu-id="27064-117">poskytuje šablony Hello je upravitelné, takže můžete upravit ho tooadd nová data, vizuály, nebo upravit dotazy toosuit vašim potřebám.</span><span class="sxs-lookup"><span data-stu-id="27064-117">hello template provided is editable so you can modify it tooadd new data, visuals, or edit queries toosuit your needs.</span></span>

## <a name="setup"></a><span data-ttu-id="27064-118">Nastavení</span><span class="sxs-lookup"><span data-stu-id="27064-118">Setup</span></span>

<span data-ttu-id="27064-119">Než začnete, musíte mít síťové zabezpečení skupiny toku povoleným protokolováním na jeden nebo více skupin zabezpečení sítě ve vašem účtu.</span><span class="sxs-lookup"><span data-stu-id="27064-119">Before you begin, you must have Network Security Group Flow Logging enabled on one or many Network Security Groups in your account.</span></span> <span data-ttu-id="27064-120">Pokyny k povolení zabezpečení sítě toku protokolů, najdete v následujícím článku toohello: [Úvod tooflow protokolování pro skupiny zabezpečení sítě](network-watcher-nsg-flow-logging-overview.md).</span><span class="sxs-lookup"><span data-stu-id="27064-120">For instructions on enabling Network Security flow logs, refer toohello following article: [Introduction tooflow logging for Network Security Groups](network-watcher-nsg-flow-logging-overview.md).</span></span>

<span data-ttu-id="27064-121">Je také nutné mít hello Power BI Desktop nainstalovaného klienta na počítači a dostatek volného místa na váš počítač toodownload a zatížení hello data protokolu, která existuje v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="27064-121">You must also have hello Power BI Desktop client installed on your machine, and enough free space on your machine toodownload and load hello log data that exists in your storage account.</span></span>

![Diagram aplikace Visio][1]

### <a name="steps"></a><span data-ttu-id="27064-123">Kroky</span><span class="sxs-lookup"><span data-stu-id="27064-123">Steps</span></span>

1. <span data-ttu-id="27064-124">Stáhněte a otevřete hello následující šablony Power BI v Power BI Desktop aplikace hello [PowerBI sledovací proces sítě toku protokoly šablony](https://aka.ms/networkwatcherpowerbiflowlogstemplate)</span><span class="sxs-lookup"><span data-stu-id="27064-124">Download and open hello following Power BI template in hello Power BI Desktop Application [Network Watcher PowerBI flow logs template](https://aka.ms/networkwatcherpowerbiflowlogstemplate)</span></span>
1. <span data-ttu-id="27064-125">Zadejte hello požadované parametry dotazu</span><span class="sxs-lookup"><span data-stu-id="27064-125">Enter hello required Query parameters</span></span>
    1. <span data-ttu-id="27064-126">**StorageAccountName** – určuje toohello název účtu úložiště hello obsahující toku NSG hello protokoly, které chcete vytvořit tooload a vizualizaci.</span><span class="sxs-lookup"><span data-stu-id="27064-126">**StorageAccountName** – Specifies toohello name of hello storage account containing hello NSG flow logs that you would like tooload and visualize.</span></span>
    1. <span data-ttu-id="27064-127">**NumberOfLogFiles** – určuje hello počet souborů protokolu, že chcete vytvořit toodownload a vizualizovat v Power BI.</span><span class="sxs-lookup"><span data-stu-id="27064-127">**NumberOfLogFiles** – Specifies hello number of log files that you would like toodownload and visualize in Power BI.</span></span> <span data-ttu-id="27064-128">Například pokud je zadán 50, hello 50 nejnovějších souborů protokolu.</span><span class="sxs-lookup"><span data-stu-id="27064-128">For example, if 50 is specified, hello 50 latest log files.</span></span> <span data-ttu-id="27064-129">Vypnuto máme 2 skupin Nsg povolené a nakonfigurované toosend NSG toku protokoly toothis účet a potom hello posledních 25 hodin protokolů lze zobrazit.</span><span class="sxs-lookup"><span data-stu-id="27064-129">Ff we have 2 NSGs enabled and configured toosend NSG flow logs toothis account, then hello past 25 hours of logs can be viewed.</span></span>

    ![hlavní Power BI][2]

1. <span data-ttu-id="27064-131">Zadejte hello přístupový klíč účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="27064-131">Enter hello Access Key for your storage account.</span></span> <span data-ttu-id="27064-132">Platný přístupových klíčů můžete najít tak, že přejdete tooyour účet úložiště v Azure portálu a vyberete hello **přístupové klíče** z nabídky nastavení hello.</span><span class="sxs-lookup"><span data-stu-id="27064-132">You can find valid access keys by navigating tooyour storage account in hello Azure portal and selecting **Access Keys** from hello Settings menu.</span></span> <span data-ttu-id="27064-133">Klikněte na tlačítko **Connect** pak změny.</span><span class="sxs-lookup"><span data-stu-id="27064-133">Click **Connect** then apply changes.</span></span>

    ![Přístupové klávesy][3]

    ![přístupový klíč 2][4]

4.  <span data-ttu-id="27064-136">Protokoly jsou stáhnout a analyzovat a mohou nyní využívat předem vytvořené vizuály hello.</span><span class="sxs-lookup"><span data-stu-id="27064-136">Your logs are download and parsed and you can now utilize hello pre-created visuals.</span></span>

## <a name="understanding-hello-visuals"></a><span data-ttu-id="27064-137">Principy vizuály hello</span><span class="sxs-lookup"><span data-stu-id="27064-137">Understanding hello visuals</span></span>

<span data-ttu-id="27064-138">Zadaný v hello šablony jsou sadu vizuální prvky, které pomáhají smysl Dobrý den data protokolu toku NSG.</span><span class="sxs-lookup"><span data-stu-id="27064-138">Provided in hello template are a set of visuals that help make sense of hello NSG Flow Log data.</span></span> <span data-ttu-id="27064-139">Hello následující obrázky znázorňují ukázka co řídicí panel hello vypadá jako při naplněný daty.</span><span class="sxs-lookup"><span data-stu-id="27064-139">hello following images show a sample of what hello dashboard looks like when populated with data.</span></span> <span data-ttu-id="27064-140">Níže budeme každý visual podrobněji prozkoumat</span><span class="sxs-lookup"><span data-stu-id="27064-140">Below we examine each visual in greater detail</span></span> 

![powerbi][5]
 
<span data-ttu-id="27064-142">Hello horní Talkers visual ukazuje hello IP adresy, které spustili hello většina připojení přes hello zadané období.</span><span class="sxs-lookup"><span data-stu-id="27064-142">hello Top Talkers visual shows hello IPs that have initiated hello most connections over hello period specified.</span></span> <span data-ttu-id="27064-143">velikost Hello hello polí odpovídá toohello relativní počet připojení.</span><span class="sxs-lookup"><span data-stu-id="27064-143">hello size of hello boxes corresponds toohello relative number of connections.</span></span> 

![toptalkers][6]

<span data-ttu-id="27064-145">Hello následující časové řady grafy zobrazit hello počet toky období hello.</span><span class="sxs-lookup"><span data-stu-id="27064-145">hello following time series graphs show hello number of flows over hello period.</span></span> <span data-ttu-id="27064-146">horní grafu Hello je oddělených směr toku hello a hello nižší je oddělených hello rozhodnutí provedené (povolit nebo zakázat).</span><span class="sxs-lookup"><span data-stu-id="27064-146">hello upper graph is segmented by hello flow direction, and hello lower is segmented by hello decision made (allow or deny).</span></span> <span data-ttu-id="27064-147">Tento vizuál můžete prozkoumat vaše provoz trendů v čase a sledujte jakékoli neobvyklé špičky nebo odmítnout v provozu nebo segmentace přenosů dat.</span><span class="sxs-lookup"><span data-stu-id="27064-147">With this visual, you can examine your traffic trends over time, and spot any abnormal spikes or decline in traffic or traffic segmentation.</span></span>

![flowsoverperiod][7]

<span data-ttu-id="27064-149">Hello následující grafy zobrazují hello toky na síťové rozhraní s hello horní oddělených směr toku a hello nižší segmentované podle rozhodnutí provedeny.</span><span class="sxs-lookup"><span data-stu-id="27064-149">hello following graphs show hello flows per Network interface, with hello upper segmented by flow direction and hello lower segmented by decision made.</span></span> <span data-ttu-id="27064-150">Pomocí těchto informací je možné získat přehled o kterém virtuální počítače oznamovat hello většina relativní tooothers, a pokud provoz tooa konkrétní virtuální počítač se povolený nebo zakázaný.</span><span class="sxs-lookup"><span data-stu-id="27064-150">With this information, you can gain insights into which of your VMs communicated hello most relative tooothers, and if traffic tooa specific VM is being allowed or denied.</span></span>

![flowspernic][8]

<span data-ttu-id="27064-152">Následující graf wheel prstenec Hello ukazuje rozpis toky podle cílový Port.</span><span class="sxs-lookup"><span data-stu-id="27064-152">hello following donut wheel chart shows a breakdown of Flows by Destination Port.</span></span> <span data-ttu-id="27064-153">Pomocí těchto informací můžete zobrazit hello nejčastěji používaná cílové porty používané v rámci hello zadané období.</span><span class="sxs-lookup"><span data-stu-id="27064-153">With this information, you can view hello most commonly used destination ports used within hello specified period.</span></span>

![prstenec][9]

<span data-ttu-id="27064-155">Hello následující pruhový graf zobrazuje hello toku NSG a pravidla.</span><span class="sxs-lookup"><span data-stu-id="27064-155">hello following bar chart shows hello Flow by NSG and Rule.</span></span> <span data-ttu-id="27064-156">Tyto informace a uvidíte hello skupiny Nsg zodpovědná za hello většina provozu a rozpis hello přenosů na skupinu NSG pravidlem.</span><span class="sxs-lookup"><span data-stu-id="27064-156">With this information, you can see hello NSGs responsible for hello most traffic, and hello breakdown of traffic on an NSG by rule.</span></span>

![barchart][10]
 
<span data-ttu-id="27064-158">Hello následující informační grafy zobrazované informace o skupinách Nsg hello k dispozici v protokolech hello, hello toků zachytit po dobu hello a datum hello hello nejdřívější protokolu zaznamenat.</span><span class="sxs-lookup"><span data-stu-id="27064-158">hello following informational charts display information about hello NSGs present in hello logs, hello number of Flows captured over hello period, and hello date of hello earliest log captured.</span></span> <span data-ttu-id="27064-159">Tyto informace získáte představu o jaké skupiny Nsg zaprotokolování a hello rozsah toků.</span><span class="sxs-lookup"><span data-stu-id="27064-159">This information gives you an idea of what NSGs are being logged and hello date range of flows.</span></span>

![infochart1][11]

![infochart2][12]

<span data-ttu-id="27064-162">Tato šablona zahrnuje hello následující průřezy tooallow můžete pouze data hello tooview vás zajímají nejvíc.</span><span class="sxs-lookup"><span data-stu-id="27064-162">This template includes hello following slicers tooallow you tooview only hello data you are most interested in.</span></span> <span data-ttu-id="27064-163">Můžete filtrovat podle skupin prostředků, skupiny Nsg a pravidla.</span><span class="sxs-lookup"><span data-stu-id="27064-163">You can filter on your resource groups, NSGs, and rules.</span></span> <span data-ttu-id="27064-164">Můžete také filtrovat podle informace 5 řazené kolekce členů, rozhodnutí a hello době hello protokolu byla zapsána.</span><span class="sxs-lookup"><span data-stu-id="27064-164">You can also filter on 5-tuple information, decision, and hello time hello log was written.</span></span>

![průřezy][13]

## <a name="conclusion"></a><span data-ttu-id="27064-166">Závěr</span><span class="sxs-lookup"><span data-stu-id="27064-166">Conclusion</span></span>

<span data-ttu-id="27064-167">Jsme vám ukázal v tomto scénáři, pomocí protokolů toku skupiny zabezpečení sítě poskytuje sledovací proces sítě a Power BI jsme jsou možné toovisualize a pochopit hello provoz.</span><span class="sxs-lookup"><span data-stu-id="27064-167">We showed in this scenario that by using Network Security Group Flow logs provided by Network Watcher and Power BI, we are able toovisualize and understand hello traffic.</span></span> <span data-ttu-id="27064-168">Pomocí šablony hello poskytuje Power BI stáhne hello protokoly přímo ze služby storage a zpracovává je místně.</span><span class="sxs-lookup"><span data-stu-id="27064-168">Using hello provided template, Power BI downloads hello logs directly from storage and processes them locally.</span></span> <span data-ttu-id="27064-169">Doba trvání tooload hello šablony se liší v závislosti na hello počet souborů požadovaných a stahovat celková velikost souborů.</span><span class="sxs-lookup"><span data-stu-id="27064-169">Time taken tooload hello template varies depending on hello number of files requested and total size of files downloaded.</span></span>

<span data-ttu-id="27064-170">Zaregistrované volné toocustomize, tato šablona pro vaše potřeby.</span><span class="sxs-lookup"><span data-stu-id="27064-170">Feel free toocustomize this template for your needs.</span></span> <span data-ttu-id="27064-171">Existuje mnoho mnoho způsobů, můžete Power BI s sítě toku protokolů skupiny zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="27064-171">There are many numerous ways that you can use Power BI with Network Security Group Flow Logs.</span></span> 

## <a name="notes"></a><span data-ttu-id="27064-172">Poznámky</span><span class="sxs-lookup"><span data-stu-id="27064-172">Notes</span></span>

* <span data-ttu-id="27064-173">Protokoly ve výchozím nastavení se ukládají do`https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/`</span><span class="sxs-lookup"><span data-stu-id="27064-173">Logs by default are stored in `https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/`</span></span>

    * <span data-ttu-id="27064-174">Pokud existuje další data v jiném adresáři jejich hello toopull dotazy a zpracování dat hello je třeba upravit.</span><span class="sxs-lookup"><span data-stu-id="27064-174">If other data exists in another directory they hello queries toopull and process hello data must be modified.</span></span>

* <span data-ttu-id="27064-175">Hello poskytuje šablony se nedoporučuje pro použití s více než 1 GB protokolů.</span><span class="sxs-lookup"><span data-stu-id="27064-175">hello provided template is not recommended for use with more than 1 GB of logs.</span></span>

* <span data-ttu-id="27064-176">Pokud máte velké množství protokoly, doporučujeme vám, že byste prozkoumat řešení pomocí jiného úložiště dat jako Data Lake nebo SQL server.</span><span class="sxs-lookup"><span data-stu-id="27064-176">If you have a large amount of logs, we recommend that you investigate a solution using another data store like Data Lake or SQL server.</span></span>

## <a name="next-steps"></a><span data-ttu-id="27064-177">Další kroky</span><span class="sxs-lookup"><span data-stu-id="27064-177">Next Steps</span></span>

<span data-ttu-id="27064-178">Zjistěte, jak toovisualize vaše skupina NSG toku protokoly s hello Elastick zásobníku navštivte stránky [vizualizovat tooand vzory pro provoz sítě z virtuálních počítačů pomocí nástroje s otevřeným zdrojem](network-watcher-using-open-source-tools.md)</span><span class="sxs-lookup"><span data-stu-id="27064-178">Learn how toovisualize your NSG flow logs with hello Elastick Stack by visiting [Visualize network traffic patterns tooand from your VMs using open source tools](network-watcher-using-open-source-tools.md)</span></span>

[1]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure1.png
[2]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure2.png
[3]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure3.png
[4]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure4.png
[5]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure5.png
[6]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure6.png
[7]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure7.png
[8]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure8.png
[9]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure9.png
[10]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure10.png
[11]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure11.png
[12]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure12.png
[13]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure13.png
