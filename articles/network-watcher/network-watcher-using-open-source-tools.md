---
title: "Vizualizace typy provozu sítě s otevřeným zdrojem nástroje a sledovací proces sítě Azure | Microsoft Docs"
description: "Tato stránka popisuje, jak použít zachytáváním paketů sledovací proces sítě s Capanalysis k vizualizaci vzory přenosů dat do a z virtuálních počítačů."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 936d881b-49f9-4798-8e45-d7185ec9fe89
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: e27bb694d0cbcf1ff7c9d8ca4682a79c8b5c5cb1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="visualize-network-traffic-patterns-to-and-from-your-vms-using-open-source-tools"></a><span data-ttu-id="59323-103">Vizualizace vzorky síťového provozu do a z virtuálních počítačů pomocí nástroje s otevřeným zdrojem</span><span class="sxs-lookup"><span data-stu-id="59323-103">Visualize network traffic patterns to and from your VMs using open source tools</span></span>

<span data-ttu-id="59323-104">Zachycení paketu obsahovat dat sítě, která vám umožní provádět forenzní sítě a hloubkové kontroly paketů.</span><span class="sxs-lookup"><span data-stu-id="59323-104">Packet captures contain network data that allow you to perform network forensics and deep packet inspection.</span></span> <span data-ttu-id="59323-105">Existuje mnoho otevře source nástroje, které můžete použít k analýze paketu zachycení a získáte přehled o vaší síti.</span><span class="sxs-lookup"><span data-stu-id="59323-105">There are many opens source tools you can use to analyze packet captures to gain insights about your network.</span></span> <span data-ttu-id="59323-106">Jeden takový nástroj je CapAnalysis, nástroj vizualizace zachytávání paketů na s otevřeným zdrojem.</span><span class="sxs-lookup"><span data-stu-id="59323-106">One such tool is CapAnalysis, an open source packet capture visualization tool.</span></span> <span data-ttu-id="59323-107">Vizualizace dat zachytávání paketů je cenné způsob, jak rychle odvozena Statistika na vzory a anomálie v rámci vaší sítě.</span><span class="sxs-lookup"><span data-stu-id="59323-107">Visualizing packet capture data is a valuable way to quickly derive insights on patterns and anomalies within your network.</span></span> <span data-ttu-id="59323-108">Vizualizace také umožňují sdílení tyto přehledy snadné použití způsobem.</span><span class="sxs-lookup"><span data-stu-id="59323-108">Visualizations also provide a means of sharing such insights in an easily consumable manner.</span></span>

<span data-ttu-id="59323-109">Sledovací proces sítě Azure poskytuje možnost pro zachycení tato data cenné tím, že můžete provádět zachycení paketů ve vaší síti.</span><span class="sxs-lookup"><span data-stu-id="59323-109">Azure’s Network Watcher provides you the ability to capture this valuable data by allowing you to perform packet captures on your network.</span></span> <span data-ttu-id="59323-110">V tomto článku jsme poskytují návod, jak vizualizovat a získáte přehled o z paketu zaznamená CapAnalysis pomocí sledovací proces sítě.</span><span class="sxs-lookup"><span data-stu-id="59323-110">In this article, we provide a walkthrough of how to visualize and gain insights from packet captures using CapAnalysis with Network Watcher.</span></span>

## <a name="scenario"></a><span data-ttu-id="59323-111">Scénář</span><span class="sxs-lookup"><span data-stu-id="59323-111">Scenario</span></span>

<span data-ttu-id="59323-112">Můžete mít jednoduché webové aplikace nasazené na virtuálním počítači v Azure Pokud chcete vizualizovat jeho síťový provoz pro rychlou identifikaci vzorů toku a možné anomálie pomocí nástroje s otevřeným zdrojem.</span><span class="sxs-lookup"><span data-stu-id="59323-112">You have a simple web application deployed on a VM in Azure want to use open source tools to visualize its network traffic to quickly identify flow patterns and any possible anomalies.</span></span> <span data-ttu-id="59323-113">S sledovací proces sítě můžete získat zachytáváním paketů prostředí sítě a uložte ho přímo na vašem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="59323-113">With Network Watcher, you can obtain a packet capture of your network environment and directly store it on your storage account.</span></span> <span data-ttu-id="59323-114">CapAnalysis můžete ingestování zachytáváním paketů přímo z storage blob a vizualizovat její obsah.</span><span class="sxs-lookup"><span data-stu-id="59323-114">CapAnalysis can then ingest the packet capture directly from the storage blob and visualize its contents.</span></span>

![scénář][1]

## <a name="steps"></a><span data-ttu-id="59323-116">Kroky</span><span class="sxs-lookup"><span data-stu-id="59323-116">Steps</span></span>

### <a name="install-capanalysis"></a><span data-ttu-id="59323-117">Nainstalujte CapAnalysis</span><span class="sxs-lookup"><span data-stu-id="59323-117">Install CapAnalysis</span></span>

<span data-ttu-id="59323-118">K instalaci CapAnalysis na virtuálním počítači, najdete zde oficiální pokyny https://www.capanalysis.net/ca/how-to-install-capanalysis.</span><span class="sxs-lookup"><span data-stu-id="59323-118">To install CapAnalysis on a virtual machine, you can refer to the official instructions here https://www.capanalysis.net/ca/how-to-install-capanalysis.</span></span>
<span data-ttu-id="59323-119">V pořadí přístup CapAnalysis vzdáleně, je potřeba otevřít port 9877 na vašem virtuálním počítači tak, že přidáte nové pravidlo příchozí zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="59323-119">In order access CapAnalysis remotely, we need to open port 9877 on your VM by adding a new inbound security rule.</span></span> <span data-ttu-id="59323-120">Další informace o vytváření pravidla ve skupinách zabezpečení sítě, naleznete [vytvořit pravidla v existující skupině](../virtual-network/virtual-networks-create-nsg-arm-pportal.md#create-rules-in-an-existing-nsg).</span><span class="sxs-lookup"><span data-stu-id="59323-120">For more about creating rules in Network Security Groups, refer to [Create rules in an existing NSG](../virtual-network/virtual-networks-create-nsg-arm-pportal.md#create-rules-in-an-existing-nsg).</span></span> <span data-ttu-id="59323-121">Jakmile pravidlo byl úspěšně přidán, byste měli mít přístup k CapAnalysis z`http://<PublicIP>:9877`</span><span class="sxs-lookup"><span data-stu-id="59323-121">Once the rule has been successfully added, you should be able to access CapAnalysis from `http://<PublicIP>:9877`</span></span>

### <a name="use-azure-network-watcher-to-start-a-packet-capture-session"></a><span data-ttu-id="59323-122">Sledovací proces sítě Azure použijte pro spuštění relace zachytávání paketů</span><span class="sxs-lookup"><span data-stu-id="59323-122">Use Azure Network Watcher to start a packet capture session</span></span>

<span data-ttu-id="59323-123">Sledovací proces sítě umožňuje zaznamenat pakety sledovat provoz do/z virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="59323-123">Network Watcher allows you to capture packets to track traffic in and out of a virtual machine.</span></span> <span data-ttu-id="59323-124">Můžete se podívat do postupujte podle pokynů v [zachytávali sledovací proces sítě spravovat pakety](network-watcher-packet-capture-manage-portal.md) pro spuštění relace zachytávání paketů.</span><span class="sxs-lookup"><span data-stu-id="59323-124">You can refer to the instructions at [Manage packet captures with Network Watcher](network-watcher-packet-capture-manage-portal.md) to start a packet capture session.</span></span> <span data-ttu-id="59323-125">Tato zachytáváním paketů mohou být uloženy v blob storage CapAnalysis přístup.</span><span class="sxs-lookup"><span data-stu-id="59323-125">This packet capture can be stored in a storage blob to be accessed by CapAnalysis.</span></span>

### <a name="upload-a-packet-capture-to-capanalysis"></a><span data-ttu-id="59323-126">Nahrajte do CapAnalysis zachytávání paketů</span><span class="sxs-lookup"><span data-stu-id="59323-126">Upload a packet capture to CapAnalysis</span></span>
<span data-ttu-id="59323-127">Můžete nahrát přímo zachytáváním paketů, provedenou sledovací proces sítě pomocí kartě "Import z adresy URL" a použít odkaz na objekt blob úložiště, kde je uložený zachytáváním paketů.</span><span class="sxs-lookup"><span data-stu-id="59323-127">You can directly upload a packet capture taken by network watcher using the “Import from URL” tab and providing a link to the storage blob where the packet capture is stored.</span></span>

<span data-ttu-id="59323-128">Pokud poskytuje odkaz na CapAnalysis, ujistěte se, že jste připojení SAS token na adresu URL úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="59323-128">When providing a link to CapAnalysis, make sure to append a SAS token to the storage blob URL.</span></span>  <span data-ttu-id="59323-129">To uděláte, přejděte do sdíleného přístupového podpisu z účtu úložiště, určit povolené oprávnění a klikněte na tlačítko Generovat SAS vytvořit token.</span><span class="sxs-lookup"><span data-stu-id="59323-129">To do this, navigate to Shared access signature from the storage account, designate the allowed permissions, and press the Generate SAS button to create a token.</span></span> <span data-ttu-id="59323-130">Tento token SAS můžete pak připojí k adresu URL paketu zachycení úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="59323-130">You can then append this SAS token to the packet capture storage blob URL.</span></span>

<span data-ttu-id="59323-131">Výsledná adresa URL bude vypadat přibližně takto: http://storageaccount.blob.core.windows.net/container/location?addSASkeyhere</span><span class="sxs-lookup"><span data-stu-id="59323-131">The resulting URL will look something like this: http://storageaccount.blob.core.windows.net/container/location?addSASkeyhere</span></span>


### <a name="analyzing-packet-captures"></a><span data-ttu-id="59323-132">Analýza paketů zaznamená</span><span class="sxs-lookup"><span data-stu-id="59323-132">Analyzing packet captures</span></span>

<span data-ttu-id="59323-133">CapAnalysis nabízí různé možnosti pro vizualizaci zachytáváním paketů, každý poskytnete analysis z hlediska různých.</span><span class="sxs-lookup"><span data-stu-id="59323-133">CapAnalysis offers various options to visualize your packet capture, each providing analysis from a different perspective.</span></span> <span data-ttu-id="59323-134">S tyto souhrny visual můžete pochopit trendy provoz vaší sítě a rychle rozpoznat všechny neobvyklé aktivity.</span><span class="sxs-lookup"><span data-stu-id="59323-134">With these visual summaries, you can understand your network traffic trends and quickly spot any unusual activity.</span></span> <span data-ttu-id="59323-135">V následujícím seznamu jsou uvedeny některé z těchto funkcí:</span><span class="sxs-lookup"><span data-stu-id="59323-135">A few of these features are shown in the following list:</span></span>

1. <span data-ttu-id="59323-136">Tok tabulky</span><span class="sxs-lookup"><span data-stu-id="59323-136">Flow Tables</span></span>

    <span data-ttu-id="59323-137">Tato tabulka obsahuje seznam toky dat paket, časové razítko přidružené na toky a různých protokolů, které jsou přidružené k toku, jakož i zdrojové a cílové IP.</span><span class="sxs-lookup"><span data-stu-id="59323-137">This table gives you the list of flows in the packet data, the time stamp associated with the flows and the various protocols associated with the flow, as well as source and destination IP.</span></span>

    ![stránka capanalysis toku][5]

1. <span data-ttu-id="59323-139">Přehled protokolu</span><span class="sxs-lookup"><span data-stu-id="59323-139">Protocol Overview</span></span>

    <span data-ttu-id="59323-140">V tomto podokně můžete rychle zobrazit distribuci síťového provozu přes různé protokoly a zeměpisných oblastí.</span><span class="sxs-lookup"><span data-stu-id="59323-140">This pane allows you to quickly see the distribution of network traffic over the various protocols and geographies.</span></span>

    ![Přehled protokolu capanalysis][6]

1. <span data-ttu-id="59323-142">Statistika</span><span class="sxs-lookup"><span data-stu-id="59323-142">Statistics</span></span>

    <span data-ttu-id="59323-143">V tomto podokně umožňuje můžete prohlédnout statistiku provozu sítě – počet bajtů odeslaných a přijatých ze zdrojové a cílové IP adresy, toky pro jednotlivé zdrojové a cílové IP adresy, používá protokol pro různé toky a pro dobu trvání toků.</span><span class="sxs-lookup"><span data-stu-id="59323-143">This pane allows you to view network traffic statistics – bytes sent and received from source and destination IPs, flows for each of the source and destination IPs, protocol used for various flows, and the duration of flows.</span></span>

    ![capanalysis statistiky][7]

1. <span data-ttu-id="59323-145">geomap</span><span class="sxs-lookup"><span data-stu-id="59323-145">Geomap</span></span>

    <span data-ttu-id="59323-146">V tomto podokně poskytuje zobrazení mapy síťových přenosů, pomocí barev škálování objem provozu z každé země.</span><span class="sxs-lookup"><span data-stu-id="59323-146">This pane provides you with a map view of your network traffic, with colors scaling to the volume of traffic from each country.</span></span> <span data-ttu-id="59323-147">Můžete vybrat zvýrazněné zemích, chcete-li zobrazit další toku statistické údaje, třeba podíl data odeslané a přijaté z IP adresy v dané země.</span><span class="sxs-lookup"><span data-stu-id="59323-147">You can select highlighted countries to view additional flow statistics such as the proportion of data sent and received from IPs in that country.</span></span>

    ![geomap][8]

1. <span data-ttu-id="59323-149">Filtry</span><span class="sxs-lookup"><span data-stu-id="59323-149">Filters</span></span>

    <span data-ttu-id="59323-150">CapAnalysis poskytuje sadu filtrů pro rychlou analýzu konkrétní paketů.</span><span class="sxs-lookup"><span data-stu-id="59323-150">CapAnalysis provides a set of filters for quick analysis of specific packets.</span></span> <span data-ttu-id="59323-151">Můžete například filtrovat data podle protokolu a získáte přehled o konkrétní na tuto podmnožinu provozu.</span><span class="sxs-lookup"><span data-stu-id="59323-151">For example, you can choose to filter the data by protocol to gain specific insights on that subset of traffic.</span></span>

    ![filtry][11]

    <span data-ttu-id="59323-153">Navštivte [https://www.capanalysis.net/ca/#about](https://www.capanalysis.net/ca/#about) Další informace o všech CapAnalysis možnosti.</span><span class="sxs-lookup"><span data-stu-id="59323-153">Visit [https://www.capanalysis.net/ca/#about](https://www.capanalysis.net/ca/#about) to learn more about all CapAnalysis' capabilities.</span></span>

## <a name="conclusion"></a><span data-ttu-id="59323-154">Závěr</span><span class="sxs-lookup"><span data-stu-id="59323-154">Conclusion</span></span>

<span data-ttu-id="59323-155">Funkce zachytávání paketů sledovací proces sítě umožňuje zaznamenat data potřebná k provedení forenzních sítě a až lépe porozumíte síťový provoz.</span><span class="sxs-lookup"><span data-stu-id="59323-155">Network Watcher’s packet capture feature allows you to capture the data necessary to perform network forensics and better understand your network traffic.</span></span> <span data-ttu-id="59323-156">V tomto scénáři jsme vám ukázal, jak paketu obrazovek z sledovací proces sítě lze snadno integrovat s otevřeným zdrojem vizualizace nástroje.</span><span class="sxs-lookup"><span data-stu-id="59323-156">In this scenario, we showed how packet captures from Network Watcher can easily be integrated with open source visualization tools.</span></span> <span data-ttu-id="59323-157">Pomocí nástroje s otevřeným zdrojem, jako je CapAnalysis k vizualizaci pakety zachycení, můžete provádění hloubkové kontroly paketů a rychlou identifikaci trendů v rámci síťového provozu.</span><span class="sxs-lookup"><span data-stu-id="59323-157">By using open source tools such as CapAnalysis to visualize packets captures, you can perform deep packet inspection and quickly identify trends within your network traffic.</span></span>

## <a name="next-steps"></a><span data-ttu-id="59323-158">Další kroky</span><span class="sxs-lookup"><span data-stu-id="59323-158">Next steps</span></span>

<span data-ttu-id="59323-159">Další informace o toku protokolů NSG, navštivte [protokolů NSG toku](network-watcher-nsg-flow-logging-overview.md)</span><span class="sxs-lookup"><span data-stu-id="59323-159">To learn more about NSG flow logs, visit [NSG Flow logs](network-watcher-nsg-flow-logging-overview.md)</span></span>

<span data-ttu-id="59323-160">Zjistěte, jak toku protokolů NSG s Power BI vizualizovat navštivte stránky [vizualizovat NSG toků protokoly s Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span><span class="sxs-lookup"><span data-stu-id="59323-160">Learn how to visualize your NSG flow logs with Power BI by visiting [Visualize NSG flows logs with Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span></span>
<!--Image references-->

[1]: ./media/network-watcher-using-open-source-tools/figure1.png
[2]: ./media/network-watcher-using-open-source-tools/figure2.png
[3]: ./media/network-watcher-using-open-source-tools/figure3.png
[4]: ./media/network-watcher-using-open-source-tools/figure4.png
[5]: ./media/network-watcher-using-open-source-tools/figure5.png
[6]: ./media/network-watcher-using-open-source-tools/figure6.png
[7]: ./media/network-watcher-using-open-source-tools/figure7.png
[8]: ./media/network-watcher-using-open-source-tools/figure8.png
[9]: ./media/network-watcher-using-open-source-tools/figure9.png
[10]: ./media/network-watcher-using-open-source-tools/figure10.png
[11]: ./media/network-watcher-using-open-source-tools/figure11.png
