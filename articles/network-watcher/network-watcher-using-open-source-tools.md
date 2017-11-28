---
title: "aaaVisualize typy provozu sítě s otevřeným zdrojem nástroje a sledovací proces sítě Azure | Microsoft Docs"
description: "Tato stránka popisuje, jak zachytit toouse sledovací proces sítě paketů s Capanalysis toovisualize provoz vzory tooand z virtuálních počítačů."
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
ms.openlocfilehash: fca9a226729162cd90d412c7b699ac54d2257a0d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="visualize-network-traffic-patterns-tooand-from-your-vms-using-open-source-tools"></a><span data-ttu-id="5ba1b-103">Vizualizace tooand vzory pro provoz sítě z virtuálních počítačů pomocí nástroje s otevřeným zdrojem</span><span class="sxs-lookup"><span data-stu-id="5ba1b-103">Visualize network traffic patterns tooand from your VMs using open source tools</span></span>

<span data-ttu-id="5ba1b-104">Zachycení paketu obsahovat dat sítě, která umožňují tooperform sítě forenzní účely a hloubkové kontroly paketů.</span><span class="sxs-lookup"><span data-stu-id="5ba1b-104">Packet captures contain network data that allow you tooperform network forensics and deep packet inspection.</span></span> <span data-ttu-id="5ba1b-105">Existuje mnoho otevře source nástroje, které můžete použít tooanalyze paketu zachycení toogain přehledy o vaší síti.</span><span class="sxs-lookup"><span data-stu-id="5ba1b-105">There are many opens source tools you can use tooanalyze packet captures toogain insights about your network.</span></span> <span data-ttu-id="5ba1b-106">Jeden takový nástroj je CapAnalysis, nástroj vizualizace zachytávání paketů na s otevřeným zdrojem.</span><span class="sxs-lookup"><span data-stu-id="5ba1b-106">One such tool is CapAnalysis, an open source packet capture visualization tool.</span></span> <span data-ttu-id="5ba1b-107">Vizualizace dat zachytávání paketů je výhodný způsob tooquickly odvozena Statistika na vzory a anomálie v rámci vaší sítě.</span><span class="sxs-lookup"><span data-stu-id="5ba1b-107">Visualizing packet capture data is a valuable way tooquickly derive insights on patterns and anomalies within your network.</span></span> <span data-ttu-id="5ba1b-108">Vizualizace také umožňují sdílení tyto přehledy snadné použití způsobem.</span><span class="sxs-lookup"><span data-stu-id="5ba1b-108">Visualizations also provide a means of sharing such insights in an easily consumable manner.</span></span>

<span data-ttu-id="5ba1b-109">Sledovací proces sítě Azure poskytuje že Hello možnost toocapture, které tato data cenné tím, že jste tooperform paketu zachytí ve vaší síti.</span><span class="sxs-lookup"><span data-stu-id="5ba1b-109">Azure’s Network Watcher provides you hello ability toocapture this valuable data by allowing you tooperform packet captures on your network.</span></span> <span data-ttu-id="5ba1b-110">V tomto článku poskytujeme návod, jak toovisualize a získat přehled o paketu zaznamená CapAnalysis pomocí sledovací proces sítě.</span><span class="sxs-lookup"><span data-stu-id="5ba1b-110">In this article, we provide a walkthrough of how toovisualize and gain insights from packet captures using CapAnalysis with Network Watcher.</span></span>

## <a name="scenario"></a><span data-ttu-id="5ba1b-111">Scénář</span><span class="sxs-lookup"><span data-stu-id="5ba1b-111">Scenario</span></span>

<span data-ttu-id="5ba1b-112">Máte jednoduché webové aplikace nasazené na virtuálním počítači v Azure chcete toouse s otevřeným zdrojem nástroje toovisualize identifikovat jeho síťový provoz tooquickly toku vzory a možné anomálie.</span><span class="sxs-lookup"><span data-stu-id="5ba1b-112">You have a simple web application deployed on a VM in Azure want toouse open source tools toovisualize its network traffic tooquickly identify flow patterns and any possible anomalies.</span></span> <span data-ttu-id="5ba1b-113">S sledovací proces sítě můžete získat zachytáváním paketů prostředí sítě a uložte ho přímo na vašem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="5ba1b-113">With Network Watcher, you can obtain a packet capture of your network environment and directly store it on your storage account.</span></span> <span data-ttu-id="5ba1b-114">CapAnalysis můžete ingestování hello zachytáváním paketů přímo z objektu blob úložiště hello a vizualizovat její obsah.</span><span class="sxs-lookup"><span data-stu-id="5ba1b-114">CapAnalysis can then ingest hello packet capture directly from hello storage blob and visualize its contents.</span></span>

![scénář][1]

## <a name="steps"></a><span data-ttu-id="5ba1b-116">Kroky</span><span class="sxs-lookup"><span data-stu-id="5ba1b-116">Steps</span></span>

### <a name="install-capanalysis"></a><span data-ttu-id="5ba1b-117">Nainstalujte CapAnalysis</span><span class="sxs-lookup"><span data-stu-id="5ba1b-117">Install CapAnalysis</span></span>

<span data-ttu-id="5ba1b-118">tooinstall CapAnalysis na virtuálním počítači, najdete pokyny oficiální toohello sem https://www.capanalysis.net/ca/how-to-install-capanalysis.</span><span class="sxs-lookup"><span data-stu-id="5ba1b-118">tooinstall CapAnalysis on a virtual machine, you can refer toohello official instructions here https://www.capanalysis.net/ca/how-to-install-capanalysis.</span></span>
<span data-ttu-id="5ba1b-119">V pořadí CapAnalysis vzdálený přístup, potřebujeme tooopen port 9877 na vašem virtuálním počítači tak, že přidáte nové pravidlo příchozí zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="5ba1b-119">In order access CapAnalysis remotely, we need tooopen port 9877 on your VM by adding a new inbound security rule.</span></span> <span data-ttu-id="5ba1b-120">Další informace o vytváření pravidla ve skupinách zabezpečení sítě, naleznete příliš[vytvořit pravidla v existující skupině](../virtual-network/virtual-networks-create-nsg-arm-pportal.md#create-rules-in-an-existing-nsg).</span><span class="sxs-lookup"><span data-stu-id="5ba1b-120">For more about creating rules in Network Security Groups, refer too[Create rules in an existing NSG](../virtual-network/virtual-networks-create-nsg-arm-pportal.md#create-rules-in-an-existing-nsg).</span></span> <span data-ttu-id="5ba1b-121">Jakmile hello pravidlo byl úspěšně přidán, musí být schopný tooaccess CapAnalysis z`http://<PublicIP>:9877`</span><span class="sxs-lookup"><span data-stu-id="5ba1b-121">Once hello rule has been successfully added, you should be able tooaccess CapAnalysis from `http://<PublicIP>:9877`</span></span>

### <a name="use-azure-network-watcher-toostart-a-packet-capture-session"></a><span data-ttu-id="5ba1b-122">Pomocí paketu zaznamenání relace toostart sledovací proces sítě Azure</span><span class="sxs-lookup"><span data-stu-id="5ba1b-122">Use Azure Network Watcher toostart a packet capture session</span></span>

<span data-ttu-id="5ba1b-123">Sledovací proces sítě vám umožní toocapture pakety tootrack provoz do/z virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="5ba1b-123">Network Watcher allows you toocapture packets tootrack traffic in and out of a virtual machine.</span></span> <span data-ttu-id="5ba1b-124">Najdete pokyny toohello v [zachytávali sledovací proces sítě spravovat pakety](network-watcher-packet-capture-manage-portal.md) toostart relaci zachytávání paketů.</span><span class="sxs-lookup"><span data-stu-id="5ba1b-124">You can refer toohello instructions at [Manage packet captures with Network Watcher](network-watcher-packet-capture-manage-portal.md) toostart a packet capture session.</span></span> <span data-ttu-id="5ba1b-125">Tento zachytáváním paketů se uloží úložiště objektů blob toobe přístup CapAnalysis.</span><span class="sxs-lookup"><span data-stu-id="5ba1b-125">This packet capture can be stored in a storage blob toobe accessed by CapAnalysis.</span></span>

### <a name="upload-a-packet-capture-toocapanalysis"></a><span data-ttu-id="5ba1b-126">Nahrát tooCapAnalysis zachytávání paketů</span><span class="sxs-lookup"><span data-stu-id="5ba1b-126">Upload a packet capture tooCapAnalysis</span></span>
<span data-ttu-id="5ba1b-127">Můžete nahrát přímo zachytáváním paketů, provedenou sledovací proces sítě pomocí karty "Import z adresy URL" hello a poskytuje odkaz toohello úložiště blob, kde je uložený zachytáváním paketů hello.</span><span class="sxs-lookup"><span data-stu-id="5ba1b-127">You can directly upload a packet capture taken by network watcher using hello “Import from URL” tab and providing a link toohello storage blob where hello packet capture is stored.</span></span>

<span data-ttu-id="5ba1b-128">Pokud poskytuje odkaz tooCapAnalysis, ujistěte se, že tooappend adresu URL typu SAS token toohello úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="5ba1b-128">When providing a link tooCapAnalysis, make sure tooappend a SAS token toohello storage blob URL.</span></span>  <span data-ttu-id="5ba1b-129">toodo, přejděte tooShared přístupový podpis z účtu úložiště hello, určit hello povolené oprávnění a stiskněte klávesu toocreate tlačítko hello generovat SAS token.</span><span class="sxs-lookup"><span data-stu-id="5ba1b-129">toodo this, navigate tooShared access signature from hello storage account, designate hello allowed permissions, and press hello Generate SAS button toocreate a token.</span></span> <span data-ttu-id="5ba1b-130">Pak můžete připojit tuto SAS token toohello paketu zachycení úložiště objektů blob adresu URL.</span><span class="sxs-lookup"><span data-stu-id="5ba1b-130">You can then append this SAS token toohello packet capture storage blob URL.</span></span>

<span data-ttu-id="5ba1b-131">Hello výsledné adresa URL bude vypadat přibližně takto: http://storageaccount.blob.core.windows.net/container/location?addSASkeyhere</span><span class="sxs-lookup"><span data-stu-id="5ba1b-131">hello resulting URL will look something like this: http://storageaccount.blob.core.windows.net/container/location?addSASkeyhere</span></span>


### <a name="analyzing-packet-captures"></a><span data-ttu-id="5ba1b-132">Analýza paketů zaznamená</span><span class="sxs-lookup"><span data-stu-id="5ba1b-132">Analyzing packet captures</span></span>

<span data-ttu-id="5ba1b-133">CapAnalysis nabízí různé možnosti toovisualize zachytáváním paketů, každý poskytnete analysis z hlediska různých.</span><span class="sxs-lookup"><span data-stu-id="5ba1b-133">CapAnalysis offers various options toovisualize your packet capture, each providing analysis from a different perspective.</span></span> <span data-ttu-id="5ba1b-134">S tyto souhrny visual můžete pochopit trendy provoz vaší sítě a rychle rozpoznat všechny neobvyklé aktivity.</span><span class="sxs-lookup"><span data-stu-id="5ba1b-134">With these visual summaries, you can understand your network traffic trends and quickly spot any unusual activity.</span></span> <span data-ttu-id="5ba1b-135">Několik z těchto funkcí jsou uvedeny v následující seznamu hello:</span><span class="sxs-lookup"><span data-stu-id="5ba1b-135">A few of these features are shown in hello following list:</span></span>

1. <span data-ttu-id="5ba1b-136">Tok tabulky</span><span class="sxs-lookup"><span data-stu-id="5ba1b-136">Flow Tables</span></span>

    <span data-ttu-id="5ba1b-137">Tato tabulka poskytuje hello seznamu toků v datech hello paketů, hello časové razítko přidružené hello toky a hello různých protokolů, které jsou přidružené k hello toku, stejně jako zdrojové a cílové IP.</span><span class="sxs-lookup"><span data-stu-id="5ba1b-137">This table gives you hello list of flows in hello packet data, hello time stamp associated with hello flows and hello various protocols associated with hello flow, as well as source and destination IP.</span></span>

    ![stránka capanalysis toku][5]

1. <span data-ttu-id="5ba1b-139">Přehled protokolu</span><span class="sxs-lookup"><span data-stu-id="5ba1b-139">Protocol Overview</span></span>

    <span data-ttu-id="5ba1b-140">V tomto podokně můžete tooquickly najdete hello distribuce síťový provoz přes hello různé protokoly a zeměpisných oblastí.</span><span class="sxs-lookup"><span data-stu-id="5ba1b-140">This pane allows you tooquickly see hello distribution of network traffic over hello various protocols and geographies.</span></span>

    ![Přehled protokolu capanalysis][6]

1. <span data-ttu-id="5ba1b-142">Statistika</span><span class="sxs-lookup"><span data-stu-id="5ba1b-142">Statistics</span></span>

    <span data-ttu-id="5ba1b-143">V tomto podokně umožňuje vám statistiku provozu tooview sítě – počet bajtů odeslaných a přijatých ze zdrojové a cílové IP adresy, toky pro každou z hello zdrojové a cílové IP adresy, používá protokol pro různé toky a doba trvání hello toků.</span><span class="sxs-lookup"><span data-stu-id="5ba1b-143">This pane allows you tooview network traffic statistics – bytes sent and received from source and destination IPs, flows for each of hello source and destination IPs, protocol used for various flows, and hello duration of flows.</span></span>

    ![capanalysis statistiky][7]

1. <span data-ttu-id="5ba1b-145">geomap</span><span class="sxs-lookup"><span data-stu-id="5ba1b-145">Geomap</span></span>

    <span data-ttu-id="5ba1b-146">V tomto podokně poskytuje zobrazení mapy síťových přenosů, pomocí barev škálování toohello objem provozu z každé země.</span><span class="sxs-lookup"><span data-stu-id="5ba1b-146">This pane provides you with a map view of your network traffic, with colors scaling toohello volume of traffic from each country.</span></span> <span data-ttu-id="5ba1b-147">Můžete vybrat statistické údaje, zvýrazněné zemích tooview další toku třeba hello podíl data odeslané a přijaté z IP adresy v dané země.</span><span class="sxs-lookup"><span data-stu-id="5ba1b-147">You can select highlighted countries tooview additional flow statistics such as hello proportion of data sent and received from IPs in that country.</span></span>

    ![geomap][8]

1. <span data-ttu-id="5ba1b-149">Filtry</span><span class="sxs-lookup"><span data-stu-id="5ba1b-149">Filters</span></span>

    <span data-ttu-id="5ba1b-150">CapAnalysis poskytuje sadu filtrů pro rychlou analýzu konkrétní paketů.</span><span class="sxs-lookup"><span data-stu-id="5ba1b-150">CapAnalysis provides a set of filters for quick analysis of specific packets.</span></span> <span data-ttu-id="5ba1b-151">Například můžete toofilter hello dat pomocí protokolu toogain konkrétní Statistika na tuto podmnožinu provozu.</span><span class="sxs-lookup"><span data-stu-id="5ba1b-151">For example, you can choose toofilter hello data by protocol toogain specific insights on that subset of traffic.</span></span>

    ![filtry][11]

    <span data-ttu-id="5ba1b-153">Navštivte [https://www.capanalysis.net/ca/#about](https://www.capanalysis.net/ca/#about) toolearn Další informace o všech CapAnalysis možnosti.</span><span class="sxs-lookup"><span data-stu-id="5ba1b-153">Visit [https://www.capanalysis.net/ca/#about](https://www.capanalysis.net/ca/#about) toolearn more about all CapAnalysis' capabilities.</span></span>

## <a name="conclusion"></a><span data-ttu-id="5ba1b-154">Závěr</span><span class="sxs-lookup"><span data-stu-id="5ba1b-154">Conclusion</span></span>

<span data-ttu-id="5ba1b-155">Funkce zachytávání paketů sledovací proces sítě vám umožní toocapture hello data potřebné tooperform sítě forenzních a až lépe porozumíte síťový provoz.</span><span class="sxs-lookup"><span data-stu-id="5ba1b-155">Network Watcher’s packet capture feature allows you toocapture hello data necessary tooperform network forensics and better understand your network traffic.</span></span> <span data-ttu-id="5ba1b-156">V tomto scénáři jsme vám ukázal, jak paketu obrazovek z sledovací proces sítě lze snadno integrovat s otevřeným zdrojem vizualizace nástroje.</span><span class="sxs-lookup"><span data-stu-id="5ba1b-156">In this scenario, we showed how packet captures from Network Watcher can easily be integrated with open source visualization tools.</span></span> <span data-ttu-id="5ba1b-157">Pomocí nástroje s otevřeným zdrojem, jako je zaznamená CapAnalysis toovisualize paketů, můžete provádění hloubkové kontroly paketů a rychlou identifikaci trendů v rámci síťového provozu.</span><span class="sxs-lookup"><span data-stu-id="5ba1b-157">By using open source tools such as CapAnalysis toovisualize packets captures, you can perform deep packet inspection and quickly identify trends within your network traffic.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5ba1b-158">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5ba1b-158">Next steps</span></span>

<span data-ttu-id="5ba1b-159">toolearn Další informace o toku protokolů NSG, navštivte [protokolů NSG toku](network-watcher-nsg-flow-logging-overview.md)</span><span class="sxs-lookup"><span data-stu-id="5ba1b-159">toolearn more about NSG flow logs, visit [NSG Flow logs](network-watcher-nsg-flow-logging-overview.md)</span></span>

<span data-ttu-id="5ba1b-160">Zjistěte, jak toovisualize vaše skupina NSG toku protokoly s Power BI navštivte stránky [vizualizovat NSG toků protokoly s Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span><span class="sxs-lookup"><span data-stu-id="5ba1b-160">Learn how toovisualize your NSG flow logs with Power BI by visiting [Visualize NSG flows logs with Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span></span>
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
