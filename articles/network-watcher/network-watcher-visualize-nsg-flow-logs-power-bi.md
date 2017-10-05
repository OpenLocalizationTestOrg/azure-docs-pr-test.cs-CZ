---
title: "Skupina zabezpečení sítě Azure visualizing toku protokoly s Power BI | Microsoft Docs"
description: "Tato stránka popisuje, jak k vizualizaci protokolů NSG toku s Power BI."
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
ms.openlocfilehash: d8c61ca2a3bd5affe02e8f9500655db6699a245f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="visualizing-network-security-group-flow-logs-with-power-bi"></a><span data-ttu-id="d3549-103">Skupina zabezpečení sítě visualizing toku protokoly s Power BI</span><span class="sxs-lookup"><span data-stu-id="d3549-103">Visualizing Network Security Group flow logs with Power BI</span></span>

<span data-ttu-id="d3549-104">Skupina zabezpečení sítě toku protokolů umožňují zobrazit informace o příchozí a odchozí provoz IP na skupiny zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="d3549-104">Network Security Group flow logs allow you to view information about ingress and egress IP traffic on Network Security Groups.</span></span> <span data-ttu-id="d3549-105">Tyto toku zobrazit protokoly odchozí a příchozí tok na základě pravidla na síťový adaptér tok se vztahuje na, 5-n-tice informace o toku (zdrojové nebo cílové IP adresy, zdrojový nebo cílový Port, protokol), a pokud byl provoz povolené nebo zakázané.</span><span class="sxs-lookup"><span data-stu-id="d3549-105">These flow logs show outbound and inbound flows on a per rule basis, the NIC the flow applies to, 5-tuple information about the flow (Source/Destination IP, Source/Destination Port, Protocol), and if the traffic was allowed or denied.</span></span>

<span data-ttu-id="d3549-106">Může být složité a získáte přehled o data protokolování toku tak, že ručně soubory protokolu.</span><span class="sxs-lookup"><span data-stu-id="d3549-106">It can be difficult to gain insights into flow logging data by manually searching the log files.</span></span> <span data-ttu-id="d3549-107">V tomto článku jsme poskytují řešení vizualizovat nejnovější toku protokolů a další informace o přenosu v síti.</span><span class="sxs-lookup"><span data-stu-id="d3549-107">In this article, we provide a solution to visualize your most recent flow logs and learn about traffic on your network.</span></span>

## <a name="scenario"></a><span data-ttu-id="d3549-108">Scénář</span><span class="sxs-lookup"><span data-stu-id="d3549-108">Scenario</span></span>

<span data-ttu-id="d3549-109">V následujícím scénáři jsme Power BI desktop připojit k účtu úložiště, které jsme nakonfigurovali jako jímky pro naše data NSG toku protokolování.</span><span class="sxs-lookup"><span data-stu-id="d3549-109">In the following scenario, we connect Power BI desktop to the storage account we have configured as the sink for our NSG Flow Logging data.</span></span> <span data-ttu-id="d3549-110">Když jsme se připojit k naší účtu úložiště, Power BI soubory ke stažení a analyzuje protokoly zajistit vizuální reprezentace přenos, který je přihlášen pomocí skupin zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="d3549-110">After we connect to our storage account, Power BI downloads and parses the logs to provide a visual representation of the traffic that is logged by Network Security groups.</span></span>

<span data-ttu-id="d3549-111">Pomocí vizuály zadané v šabloně, které můžete zkontrolovat:</span><span class="sxs-lookup"><span data-stu-id="d3549-111">Using the visuals supplied in the template you can examine:</span></span>

* <span data-ttu-id="d3549-112">Horní Talkers</span><span class="sxs-lookup"><span data-stu-id="d3549-112">Top Talkers</span></span>
* <span data-ttu-id="d3549-113">Časových řad toku dat rozhodnutím směr a pravidla</span><span class="sxs-lookup"><span data-stu-id="d3549-113">Time Series Flow Data by direction and rule decision</span></span>
* <span data-ttu-id="d3549-114">Toky podle adresy MAC rozhraní sítě</span><span class="sxs-lookup"><span data-stu-id="d3549-114">Flows by Network Interface MAC address</span></span>
* <span data-ttu-id="d3549-115">Toky NSG a pravidla</span><span class="sxs-lookup"><span data-stu-id="d3549-115">Flows by NSG and Rule</span></span>
* <span data-ttu-id="d3549-116">Toky podle cílový Port</span><span class="sxs-lookup"><span data-stu-id="d3549-116">Flows by Destination Port</span></span>

<span data-ttu-id="d3549-117">Šablona zadaná je upravitelné, takže můžete upravit tak, aby nová data, vizuály, přidat nebo upravit dotazy, aby vyhovovaly potřebám vaší.</span><span class="sxs-lookup"><span data-stu-id="d3549-117">The template provided is editable so you can modify it to add new data, visuals, or edit queries to suit your needs.</span></span>

## <a name="setup"></a><span data-ttu-id="d3549-118">Nastavení</span><span class="sxs-lookup"><span data-stu-id="d3549-118">Setup</span></span>

<span data-ttu-id="d3549-119">Než začnete, musíte mít síťové zabezpečení skupiny toku povoleným protokolováním na jeden nebo více skupin zabezpečení sítě ve vašem účtu.</span><span class="sxs-lookup"><span data-stu-id="d3549-119">Before you begin, you must have Network Security Group Flow Logging enabled on one or many Network Security Groups in your account.</span></span> <span data-ttu-id="d3549-120">Pokyny k povolení zabezpečení sítě toku protokolů, najdete v následujícím článku: [Úvod do toku protokolování pro skupiny zabezpečení sítě](network-watcher-nsg-flow-logging-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d3549-120">For instructions on enabling Network Security flow logs, refer to the following article: [Introduction to flow logging for Network Security Groups](network-watcher-nsg-flow-logging-overview.md).</span></span>

<span data-ttu-id="d3549-121">Musí také mít Power BI Desktop klient nainstalován na váš počítač a dostatek volného místa v počítači ke stažení a načíst data protokolu, která existuje v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="d3549-121">You must also have the Power BI Desktop client installed on your machine, and enough free space on your machine to download and load the log data that exists in your storage account.</span></span>

![Diagram aplikace Visio][1]

### <a name="steps"></a><span data-ttu-id="d3549-123">Kroky</span><span class="sxs-lookup"><span data-stu-id="d3549-123">Steps</span></span>

1. <span data-ttu-id="d3549-124">Stáhněte si a nainstalujte následující šablony Power BI v aplikaci Power BI Desktop [PowerBI sledovací proces sítě toku protokoly šablony](https://aka.ms/networkwatcherpowerbiflowlogstemplate)</span><span class="sxs-lookup"><span data-stu-id="d3549-124">Download and open the following Power BI template in the Power BI Desktop Application [Network Watcher PowerBI flow logs template](https://aka.ms/networkwatcherpowerbiflowlogstemplate)</span></span>
1. <span data-ttu-id="d3549-125">Zadejte požadované parametry dotazu</span><span class="sxs-lookup"><span data-stu-id="d3549-125">Enter the required Query parameters</span></span>
    1. <span data-ttu-id="d3549-126">**StorageAccountName** – Určuje název účtu úložiště obsahující protokoly toku NSG, které chcete načíst a vizualizaci.</span><span class="sxs-lookup"><span data-stu-id="d3549-126">**StorageAccountName** – Specifies to the name of the storage account containing the NSG flow logs that you would like to load and visualize.</span></span>
    1. <span data-ttu-id="d3549-127">**NumberOfLogFiles** – určuje počet souborů protokolu, které chcete stáhnout a vizualizovat v Power BI.</span><span class="sxs-lookup"><span data-stu-id="d3549-127">**NumberOfLogFiles** – Specifies the number of log files that you would like to download and visualize in Power BI.</span></span> <span data-ttu-id="d3549-128">Pokud je zadán 50, například 50 nejnovějších souborů protokolu.</span><span class="sxs-lookup"><span data-stu-id="d3549-128">For example, if 50 is specified, the 50 latest log files.</span></span> <span data-ttu-id="d3549-129">Vypnuto máme 2 skupiny Nsg povolené a nakonfigurované k odeslání protokolů toku NSG k tomuto účtu a potom je možné zobrazit posledních 25 hodin protokolů.</span><span class="sxs-lookup"><span data-stu-id="d3549-129">Ff we have 2 NSGs enabled and configured to send NSG flow logs to this account, then the past 25 hours of logs can be viewed.</span></span>

    ![hlavní Power BI][2]

1. <span data-ttu-id="d3549-131">Zadejte přístupový klíč účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="d3549-131">Enter the Access Key for your storage account.</span></span> <span data-ttu-id="d3549-132">Platný přístupových klíčů můžete najít tak, že přejdete na váš účet úložiště ve službě Azure portálu a vyberete **přístupové klíče** z nabídky nastavení.</span><span class="sxs-lookup"><span data-stu-id="d3549-132">You can find valid access keys by navigating to your storage account in the Azure portal and selecting **Access Keys** from the Settings menu.</span></span> <span data-ttu-id="d3549-133">Klikněte na tlačítko **Connect** pak změny.</span><span class="sxs-lookup"><span data-stu-id="d3549-133">Click **Connect** then apply changes.</span></span>

    ![Přístupové klávesy][3]

    ![přístupový klíč 2][4]

4.  <span data-ttu-id="d3549-136">Protokoly jsou stáhnout a analyzovat a mohou nyní využívat předem vytvořené vizuální prvky.</span><span class="sxs-lookup"><span data-stu-id="d3549-136">Your logs are download and parsed and you can now utilize the pre-created visuals.</span></span>

## <a name="understanding-the-visuals"></a><span data-ttu-id="d3549-137">Principy vizuálech</span><span class="sxs-lookup"><span data-stu-id="d3549-137">Understanding the visuals</span></span>

<span data-ttu-id="d3549-138">Zadané v šabloně jsou sady vizuální prvky, které pomáhají smysl data protokolu toku NSG.</span><span class="sxs-lookup"><span data-stu-id="d3549-138">Provided in the template are a set of visuals that help make sense of the NSG Flow Log data.</span></span> <span data-ttu-id="d3549-139">Následující obrázky znázorňují ukázka co řídicí panel vypadá, když naplněný daty.</span><span class="sxs-lookup"><span data-stu-id="d3549-139">The following images show a sample of what the dashboard looks like when populated with data.</span></span> <span data-ttu-id="d3549-140">Níže budeme každý visual podrobněji prozkoumat</span><span class="sxs-lookup"><span data-stu-id="d3549-140">Below we examine each visual in greater detail</span></span> 

![powerbi][5]
 
<span data-ttu-id="d3549-142">Horní Talkers visual ukazuje zadané IP adresy, které spustili většina připojení za období.</span><span class="sxs-lookup"><span data-stu-id="d3549-142">The Top Talkers visual shows the IPs that have initiated the most connections over the period specified.</span></span> <span data-ttu-id="d3549-143">Velikost polí odpovídá relativní počet připojení.</span><span class="sxs-lookup"><span data-stu-id="d3549-143">The size of the boxes corresponds to the relative number of connections.</span></span> 

![toptalkers][6]

<span data-ttu-id="d3549-145">Následující grafů řady čas zobrazit toků za období.</span><span class="sxs-lookup"><span data-stu-id="d3549-145">The following time series graphs show the number of flows over the period.</span></span> <span data-ttu-id="d3549-146">Horní grafu je oddělených směr toku a čím nižší je oddělených rozhodnutí provedené (povolit nebo zakázat).</span><span class="sxs-lookup"><span data-stu-id="d3549-146">The upper graph is segmented by the flow direction, and the lower is segmented by the decision made (allow or deny).</span></span> <span data-ttu-id="d3549-147">Tento vizuál můžete prozkoumat vaše provoz trendů v čase a sledujte jakékoli neobvyklé špičky nebo odmítnout v provozu nebo segmentace přenosů dat.</span><span class="sxs-lookup"><span data-stu-id="d3549-147">With this visual, you can examine your traffic trends over time, and spot any abnormal spikes or decline in traffic or traffic segmentation.</span></span>

![flowsoverperiod][7]

<span data-ttu-id="d3549-149">Následující zobrazení grafů toky na síťové rozhraní s horní oddělených směr toku a dolní oddělených rozhodnutí provedeny.</span><span class="sxs-lookup"><span data-stu-id="d3549-149">The following graphs show the flows per Network interface, with the upper segmented by flow direction and the lower segmented by decision made.</span></span> <span data-ttu-id="d3549-150">Pomocí těchto informací je možné získat přehledy, které virtuální počítače předány nejvíce relativně k ostatním, a pokud provoz na konkrétním virtuálním počítači se povoluje nebo odepírá.</span><span class="sxs-lookup"><span data-stu-id="d3549-150">With this information, you can gain insights into which of your VMs communicated the most relative to others, and if traffic to a specific VM is being allowed or denied.</span></span>

![flowspernic][8]

<span data-ttu-id="d3549-152">Následující tabulka wheel prstenec ukazuje rozpis toky podle cílový Port.</span><span class="sxs-lookup"><span data-stu-id="d3549-152">The following donut wheel chart shows a breakdown of Flows by Destination Port.</span></span> <span data-ttu-id="d3549-153">Pomocí těchto informací můžete zobrazit nejčastěji používané cílové porty použité v zadaném období.</span><span class="sxs-lookup"><span data-stu-id="d3549-153">With this information, you can view the most commonly used destination ports used within the specified period.</span></span>

![prstenec][9]

<span data-ttu-id="d3549-155">Následující pruhový graf zobrazuje tok NSG a pravidla.</span><span class="sxs-lookup"><span data-stu-id="d3549-155">The following bar chart shows the Flow by NSG and Rule.</span></span> <span data-ttu-id="d3549-156">Tyto informace a uvidíte odpovědná za většina provozu a rozpis provozu skupiny Nsg na skupinu NSG pravidlem.</span><span class="sxs-lookup"><span data-stu-id="d3549-156">With this information, you can see the NSGs responsible for the most traffic, and the breakdown of traffic on an NSG by rule.</span></span>

![barchart][10]
 
<span data-ttu-id="d3549-158">Následující informační grafy zobrazují informace o přítomen v protokolech, toků zachytit přes období a datum nejdřívější protokolu zachytit skupiny Nsg.</span><span class="sxs-lookup"><span data-stu-id="d3549-158">The following informational charts display information about the NSGs present in the logs, the number of Flows captured over the period, and the date of the earliest log captured.</span></span> <span data-ttu-id="d3549-159">Tyto informace získáte představu o jaké skupiny Nsg se protokolována a rozsah kalendářních dat toky.</span><span class="sxs-lookup"><span data-stu-id="d3549-159">This information gives you an idea of what NSGs are being logged and the date range of flows.</span></span>

![infochart1][11]

![infochart2][12]

<span data-ttu-id="d3549-162">Tato šablona zahrnuje následující průřezy, aby bylo možné zobrazit pouze data, která vás zajímají nejvíc.</span><span class="sxs-lookup"><span data-stu-id="d3549-162">This template includes the following slicers to allow you to view only the data you are most interested in.</span></span> <span data-ttu-id="d3549-163">Můžete filtrovat podle skupin prostředků, skupiny Nsg a pravidla.</span><span class="sxs-lookup"><span data-stu-id="d3549-163">You can filter on your resource groups, NSGs, and rules.</span></span> <span data-ttu-id="d3549-164">Můžete také filtrovat podle informace 5 řazené kolekce členů, rozhodnutí a době, kdy byla zapsána do protokolu.</span><span class="sxs-lookup"><span data-stu-id="d3549-164">You can also filter on 5-tuple information, decision, and the time the log was written.</span></span>

![průřezy][13]

## <a name="conclusion"></a><span data-ttu-id="d3549-166">Závěr</span><span class="sxs-lookup"><span data-stu-id="d3549-166">Conclusion</span></span>

<span data-ttu-id="d3549-167">Můžeme vám ukázal, v tomto scénáři, pomocí protokolů toku skupiny zabezpečení sítě poskytuje sledovací proces sítě a Power BI vám budeme schopni vizualizaci a porozumění provoz.</span><span class="sxs-lookup"><span data-stu-id="d3549-167">We showed in this scenario that by using Network Security Group Flow logs provided by Network Watcher and Power BI, we are able to visualize and understand the traffic.</span></span> <span data-ttu-id="d3549-168">Pomocí zadané šablony Power BI stáhne protokoly přímo ze služby storage a zpracovává je místně.</span><span class="sxs-lookup"><span data-stu-id="d3549-168">Using the provided template, Power BI downloads the logs directly from storage and processes them locally.</span></span> <span data-ttu-id="d3549-169">Čas potřebný k načtení šablony se liší v závislosti na počtu požadované soubory a stahovat celková velikost souborů.</span><span class="sxs-lookup"><span data-stu-id="d3549-169">Time taken to load the template varies depending on the number of files requested and total size of files downloaded.</span></span>

<span data-ttu-id="d3549-170">Nebojte se, že tato šablona pro vaše potřeby přizpůsobit.</span><span class="sxs-lookup"><span data-stu-id="d3549-170">Feel free to customize this template for your needs.</span></span> <span data-ttu-id="d3549-171">Existuje mnoho mnoho způsobů, můžete Power BI s sítě toku protokolů skupiny zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="d3549-171">There are many numerous ways that you can use Power BI with Network Security Group Flow Logs.</span></span> 

## <a name="notes"></a><span data-ttu-id="d3549-172">Poznámky</span><span class="sxs-lookup"><span data-stu-id="d3549-172">Notes</span></span>

* <span data-ttu-id="d3549-173">Protokoly ve výchozím nastavení se ukládají do`https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/`</span><span class="sxs-lookup"><span data-stu-id="d3549-173">Logs by default are stored in `https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/`</span></span>

    * <span data-ttu-id="d3549-174">Pokud existuje další data z jiného adresáře je třeba upravit jejich dotazy pro vyžádání obsahu a proces data.</span><span class="sxs-lookup"><span data-stu-id="d3549-174">If other data exists in another directory they the queries to pull and process the data must be modified.</span></span>

* <span data-ttu-id="d3549-175">Zadaná šablona se nedoporučuje pro použití s více než 1 GB protokolů.</span><span class="sxs-lookup"><span data-stu-id="d3549-175">The provided template is not recommended for use with more than 1 GB of logs.</span></span>

* <span data-ttu-id="d3549-176">Pokud máte velké množství protokoly, doporučujeme vám, že byste prozkoumat řešení pomocí jiného úložiště dat jako Data Lake nebo SQL server.</span><span class="sxs-lookup"><span data-stu-id="d3549-176">If you have a large amount of logs, we recommend that you investigate a solution using another data store like Data Lake or SQL server.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d3549-177">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d3549-177">Next Steps</span></span>

<span data-ttu-id="d3549-178">Zjistěte, jak k vizualizaci toku protokolů NSG se zásobníkem Elastick navštivte stránky [vizualizovat vzorky síťového provozu do a z virtuálních počítačů pomocí nástroje s otevřeným zdrojem](network-watcher-using-open-source-tools.md)</span><span class="sxs-lookup"><span data-stu-id="d3549-178">Learn how to visualize your NSG flow logs with the Elastick Stack by visiting [Visualize network traffic patterns to and from your VMs using open source tools](network-watcher-using-open-source-tools.md)</span></span>

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
