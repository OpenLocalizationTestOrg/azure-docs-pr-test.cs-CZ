---
title: "Kontroly paketů s sledovací proces sítě Azure | Microsoft Docs"
description: "Tento článek popisuje, jak používat sledovací proces sítě k provádění hloubkové kontroly paketů shromážděné z virtuálního počítače"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 7b907d00-9c35-40f5-a61e-beb7b782276f
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 91c47bb8922a9be21dff72e7cf64b29b14a36e9e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="packet-inspection-with-azure-network-watcher"></a><span data-ttu-id="8c9ac-103">Kontroly paketů s sledovací proces sítě Azure</span><span class="sxs-lookup"><span data-stu-id="8c9ac-103">Packet inspection with Azure Network Watcher</span></span>

<span data-ttu-id="8c9ac-104">Pomocí paketu funkci zachycení sledovací proces sítě, můžete zahájit a spravovat zachycení relace na virtuální počítače Azure z portálu, prostředí PowerShell, rozhraní příkazového řádku a programově pomocí sady SDK a REST API.</span><span class="sxs-lookup"><span data-stu-id="8c9ac-104">Using the packet capture feature of Network Watcher, you can initiate and manage captures sessions on your Azure VMs from the portal, PowerShell, CLI, and programmatically through the SDK and REST API.</span></span> <span data-ttu-id="8c9ac-105">Zachytáváním paketů umožňuje adresu scénáře, které vyžadují dat na úrovni paketů tím, že poskytuje informace ve snadno použitelného formátu.</span><span class="sxs-lookup"><span data-stu-id="8c9ac-105">Packet capture allows you to address scenarios that require packet level data by providing the information in a readily usable format.</span></span> <span data-ttu-id="8c9ac-106">Využívání volně dostupných nástrojů k kontrolovat data, můžete prověřit komunikace odesílané do a z virtuálních počítačů a získáte přehled o síťový provoz.</span><span class="sxs-lookup"><span data-stu-id="8c9ac-106">Leveraging freely available tools to inspect the data, you can examine communications sent to and from your VMs and gain insights into your network traffic.</span></span> <span data-ttu-id="8c9ac-107">Některé příklad použití paketu zachycení dat patří: příčin problémů sítě nebo aplikace, zjišťování sítě pokusy o zneužití a narušení nebo udržování dodržování legislativní předpisů.</span><span class="sxs-lookup"><span data-stu-id="8c9ac-107">Some example uses of packet capture data include: investigating network or application issues, detecting network misuse and intrusion attempts, or maintaining regulatory compliance.</span></span> <span data-ttu-id="8c9ac-108">V tomto článku jsme ukazují, jak otevřít soubor zachytávání paketů poskytuje sledovací proces sítě pomocí nástroje populární open source.</span><span class="sxs-lookup"><span data-stu-id="8c9ac-108">In this article, we show how to open a packet capture file provided by Network Watcher using a popular open source tool.</span></span> <span data-ttu-id="8c9ac-109">Poskytujeme také příklady znázorňující způsob výpočtu latence připojení, identifikovat neobvyklé provoz a zkontrolujte síťových statistiky.</span><span class="sxs-lookup"><span data-stu-id="8c9ac-109">We will also provide examples showing how to calculate a connection latency, identify abnormal traffic, and examine networking statistics.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="8c9ac-110">Než začnete</span><span class="sxs-lookup"><span data-stu-id="8c9ac-110">Before you begin</span></span>

<span data-ttu-id="8c9ac-111">Tento článek probírá některé scénáře předem nakonfigurovaná na zachytáváním paketů, která byla dříve spuštěna.</span><span class="sxs-lookup"><span data-stu-id="8c9ac-111">This article goes through some pre-configured scenarios on a packet capture that was run previously.</span></span> <span data-ttu-id="8c9ac-112">Tyto scénáře ukazují možnosti, které jsou přístupné kontrolou zachytáváním paketů.</span><span class="sxs-lookup"><span data-stu-id="8c9ac-112">These scenarios illustrate capabilities that can be accessed by reviewing a packet capture.</span></span> <span data-ttu-id="8c9ac-113">Tento scénář používá [WireShark](https://www.wireshark.org/) kontrola zachytáváním paketů.</span><span class="sxs-lookup"><span data-stu-id="8c9ac-113">This scenario uses [WireShark](https://www.wireshark.org/) to inspect the packet capture.</span></span>

<span data-ttu-id="8c9ac-114">Tento scénář předpokládá, že jste již spustili zachytáváním paketů na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="8c9ac-114">This scenario assumes you already ran a packet capture on a virtual machine.</span></span> <span data-ttu-id="8c9ac-115">Další informace o vytvoření návštěvu zachytávání paketů [zachytávali portálu spravovat pakety](network-watcher-packet-capture-manage-portal.md) nebo se zbytkem navštivte stránky [Správa Zachytávali pakety REST API](network-watcher-packet-capture-manage-rest.md).</span><span class="sxs-lookup"><span data-stu-id="8c9ac-115">To learn how to create a packet capture visit [Manage packet captures with the portal](network-watcher-packet-capture-manage-portal.md) or with REST by visiting [Managing Packet Captures with REST API](network-watcher-packet-capture-manage-rest.md).</span></span>

## <a name="scenario"></a><span data-ttu-id="8c9ac-116">Scénář</span><span class="sxs-lookup"><span data-stu-id="8c9ac-116">Scenario</span></span>

<span data-ttu-id="8c9ac-117">V tomto scénáři můžete:</span><span class="sxs-lookup"><span data-stu-id="8c9ac-117">In this scenario, you:</span></span>

* <span data-ttu-id="8c9ac-118">Zkontrolujte zachytávání paketů</span><span class="sxs-lookup"><span data-stu-id="8c9ac-118">Review a packet capture</span></span>

## <a name="calculate-network-latency"></a><span data-ttu-id="8c9ac-119">Vypočítat latence sítě</span><span class="sxs-lookup"><span data-stu-id="8c9ac-119">Calculate network latency</span></span>

<span data-ttu-id="8c9ac-120">V tomto scénáři ukážeme, jak zobrazit čas počáteční odezvy (požadavku) ke kterým dochází mezi dva koncové body konverzace protokolu TCP (Transmission Control).</span><span class="sxs-lookup"><span data-stu-id="8c9ac-120">In this scenario, we show how to view the initial Round Trip Time (RTT) of a Transmission Control Protocol (TCP) conversation occurring between two endpoints.</span></span>

<span data-ttu-id="8c9ac-121">Po vytvoření připojení protokolu TCP, postupujte podle prvních tří paketů odeslaných za připojení vzor obvykle označuje jako třícestný handshake.</span><span class="sxs-lookup"><span data-stu-id="8c9ac-121">When a TCP connection is established, the first three packets sent in the connection follow a pattern commonly referred to as the three-way handshake.</span></span> <span data-ttu-id="8c9ac-122">Prověřením první dva paketů odeslaných za této metody handshake počáteční požadavek od klienta a na odpověď od serveru jsme můžete vypočítat latence při toto připojení bylo vytvořeno.</span><span class="sxs-lookup"><span data-stu-id="8c9ac-122">By examining the first two packets sent in this handshake, an initial request from the client and a response from the server, we can calculate the latency when this connection was established.</span></span> <span data-ttu-id="8c9ac-123">Tato čekací doba se označuje jako čas odezvy (požadavku).</span><span class="sxs-lookup"><span data-stu-id="8c9ac-123">This latency is referred to as the Round Trip Time (RTT).</span></span> <span data-ttu-id="8c9ac-124">Další informace o protokolu TCP a třícestné naleznete v následujícím zdroji.</span><span class="sxs-lookup"><span data-stu-id="8c9ac-124">For more information on the TCP protocol and the three-way handshake refer to the following resource.</span></span> <span data-ttu-id="8c9ac-125">https://support.microsoft.com/en-US/Help/172983/Explanation-of-the-three-way-handshake-VIA-TCP-IP</span><span class="sxs-lookup"><span data-stu-id="8c9ac-125">https://support.microsoft.com/en-us/help/172983/explanation-of-the-three-way-handshake-via-tcp-ip</span></span>

### <a name="step-1"></a><span data-ttu-id="8c9ac-126">Krok 1</span><span class="sxs-lookup"><span data-stu-id="8c9ac-126">Step 1</span></span>

<span data-ttu-id="8c9ac-127">Spusťte WireShark</span><span class="sxs-lookup"><span data-stu-id="8c9ac-127">Launch WireShark</span></span>

### <a name="step-2"></a><span data-ttu-id="8c9ac-128">Krok 2</span><span class="sxs-lookup"><span data-stu-id="8c9ac-128">Step 2</span></span>

<span data-ttu-id="8c9ac-129">Zatížení **CAP** soubor z vaší zachytáváním paketů.</span><span class="sxs-lookup"><span data-stu-id="8c9ac-129">Load the **.cap** file from your packet capture.</span></span> <span data-ttu-id="8c9ac-130">Tento soubor nachází v objektu blob se uloží do našich místně na virtuálním počítači, v závislosti na tom, jak jste nakonfigurovali.</span><span class="sxs-lookup"><span data-stu-id="8c9ac-130">This file can be found in the blob it was saved in our locally on the virtual machine, depending on how you configured it.</span></span>

### <a name="step-3"></a><span data-ttu-id="8c9ac-131">Krok 3</span><span class="sxs-lookup"><span data-stu-id="8c9ac-131">Step 3</span></span>

<span data-ttu-id="8c9ac-132">Počáteční čas odezvy (požadavku) v TCP konverzace zobrazíte jsme bude pouze vidí první dva pakety zahrnutých v TCP handshake.</span><span class="sxs-lookup"><span data-stu-id="8c9ac-132">To view the initial Round Trip Time (RTT) in TCP conversations, we will only be looking at the first two packets involved in the TCP handshake.</span></span> <span data-ttu-id="8c9ac-133">Použijeme v třícestné, první dva pakety, které jsou [SYN], [SYN, ACK] paketů.</span><span class="sxs-lookup"><span data-stu-id="8c9ac-133">We will be using the first two packets in the three-way handshake, which are the [SYN], [SYN, ACK] packets.</span></span> <span data-ttu-id="8c9ac-134">Že jsou pojmenované pro příznaky, které jsou nastaveny v hlavičce protokolu TCP.</span><span class="sxs-lookup"><span data-stu-id="8c9ac-134">They are named for flags set in the TCP header.</span></span> <span data-ttu-id="8c9ac-135">V tomto scénáři se nepoužijí poslední paketů v handshake paket [ACK].</span><span class="sxs-lookup"><span data-stu-id="8c9ac-135">The last packet in the handshake, the [ACK] packet, will not be used in this scenario.</span></span> <span data-ttu-id="8c9ac-136">Klient odešle paket [SYN].</span><span class="sxs-lookup"><span data-stu-id="8c9ac-136">The [SYN] packet is sent by the client.</span></span> <span data-ttu-id="8c9ac-137">Po přijetí je server odešle paket [ACK] jako potvrzení přijetí SYN z klienta.</span><span class="sxs-lookup"><span data-stu-id="8c9ac-137">Once it is received the server sends the [ACK] packet as an acknowledgement of receiving the SYN from the client.</span></span> <span data-ttu-id="8c9ac-138">Využití fakt, že odpověď serveru vyžaduje minimální režii, jsme vypočítat požadavku odečtením čas [SYN, ACK] byl přijat paket klienta podle času [SYN] byl odeslán paket klientem.</span><span class="sxs-lookup"><span data-stu-id="8c9ac-138">Leveraging the fact that the server’s response requires very little overhead, we calculate the RTT by subtracting the time the [SYN, ACK] packet was received by the client by the time [SYN] packet was sent by the client.</span></span>

<span data-ttu-id="8c9ac-139">Pomocí WireShark tato hodnota je vypočítána pro nás.</span><span class="sxs-lookup"><span data-stu-id="8c9ac-139">Using WireShark this value is calculated for us.</span></span>

<span data-ttu-id="8c9ac-140">První dva pakety snadno zobrazit v třícestné TCP, jsme bude využívat funkce pro filtrování poskytované WireShark.</span><span class="sxs-lookup"><span data-stu-id="8c9ac-140">To more easily view the first two packets in the TCP three-way handshake, we will utilize the filtering capability provided by WireShark.</span></span>

<span data-ttu-id="8c9ac-141">Pokud chcete použít filtr v WireShark, rozbalte položku "Transmission Control Protocol" segmentu paketu [SYN] na vašem snímku a prozkoumat příznaky nastaveny v hlavičce protokolu TCP.</span><span class="sxs-lookup"><span data-stu-id="8c9ac-141">To apply the filter in WireShark, expand the “Transmission Control Protocol” Segment of a [SYN] packet in your capture and examine the flags set in the TCP header.</span></span>

<span data-ttu-id="8c9ac-142">Vzhledem k tomu, že chceme filtrujte všechny [SYN] a [SYN, ACK] paketů, v části Příznaky cofirm, že je bit Syn nastaven na hodnotu 1, a potom klikněte pravým tlačítkem na Syn bit -> použít jako filtr -> vybrané.</span><span class="sxs-lookup"><span data-stu-id="8c9ac-142">Since we are looking to filter on all [SYN] and [SYN, ACK] packets, under flags cofirm that the Syn bit is set to 1, then right click on the Syn bit -> Apply as Filter -> Selected.</span></span>

![Obrázek 7][7]

### <a name="step-4"></a><span data-ttu-id="8c9ac-144">Krok 4</span><span class="sxs-lookup"><span data-stu-id="8c9ac-144">Step 4</span></span>

<span data-ttu-id="8c9ac-145">Teď, když jste provedli filtrování okna pouze zobrazíte pakety s nastaveným bitem [SYN], můžete snadno vybrat konverzace, které vás zajímají zobrazíte počáteční požadavku.</span><span class="sxs-lookup"><span data-stu-id="8c9ac-145">Now that you have filtered the window to only see packets with the [SYN] bit set, you can easily select conversations you are interested in to view the initial RTT.</span></span> <span data-ttu-id="8c9ac-146">Jednoduchý způsob, jak zobrazit požadavku v WireShark jednoduše klikněte na rozevírací nabídce, označen "SEQ/ACK" analysis.</span><span class="sxs-lookup"><span data-stu-id="8c9ac-146">A simple way to view the RTT in WireShark simply click the dropdown marked “SEQ/ACK” analysis.</span></span> <span data-ttu-id="8c9ac-147">Zobrazí se požadavku zobrazí.</span><span class="sxs-lookup"><span data-stu-id="8c9ac-147">You will then see the RTT displayed.</span></span> <span data-ttu-id="8c9ac-148">V takovém případě požadavku byla 0.0022114 sekund nebo 2.211 ms.</span><span class="sxs-lookup"><span data-stu-id="8c9ac-148">In this case, the RTT was 0.0022114 seconds, or 2.211 ms.</span></span>

![Obrázek 8][8]

## <a name="unwanted-protocols"></a><span data-ttu-id="8c9ac-150">Nežádoucí protokoly</span><span class="sxs-lookup"><span data-stu-id="8c9ac-150">Unwanted protocols</span></span>

<span data-ttu-id="8c9ac-151">Můžete mít mnoho aplikací běžících na instanci virtuálního počítače, které jste nasadili v Azure.</span><span class="sxs-lookup"><span data-stu-id="8c9ac-151">You can have many applications running on a virtual machine instance you have deployed in Azure.</span></span> <span data-ttu-id="8c9ac-152">Mnoho z těchto aplikací komunikují přes síť, možná bez explicitního oprávnění.</span><span class="sxs-lookup"><span data-stu-id="8c9ac-152">Many of these applications communicate over the network, perhaps without your explicit permission.</span></span> <span data-ttu-id="8c9ac-153">Uložení síťovou komunikaci pomocí zachytáváním paketů, jsme prozkoumat jak aplikace mluvíme v síti a vyhledejte všechny problémy.</span><span class="sxs-lookup"><span data-stu-id="8c9ac-153">Using packet capture to store network communication, we can investigate how application are talking on the network and look for any issues.</span></span>

<span data-ttu-id="8c9ac-154">V tomto příkladu jsme Zkontrolujte předchozí spustili zachytáváním paketů pro nežádoucí protokoly, které můžou signalizovat neověřené komunikace z aplikace běžící na vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="8c9ac-154">In this example, we review a previous ran packet capture for unwanted protocols that may indicate unauthorized communication from an application running on your machine.</span></span>

### <a name="step-1"></a><span data-ttu-id="8c9ac-155">Krok 1</span><span class="sxs-lookup"><span data-stu-id="8c9ac-155">Step 1</span></span>

<span data-ttu-id="8c9ac-156">Pomocí stejné zachycení v předchozím scénáři, klikněte na **statistiky** > **protokol hierarchie**</span><span class="sxs-lookup"><span data-stu-id="8c9ac-156">Using the same capture in the previous scenario click **Statistics** > **Protocol Hierarchy**</span></span>

![protokol hierarchie nabídky][2]

<span data-ttu-id="8c9ac-158">Zobrazí se okno hierarchie protokolu.</span><span class="sxs-lookup"><span data-stu-id="8c9ac-158">The protocol hierarchy window appears.</span></span> <span data-ttu-id="8c9ac-159">Toto zobrazení obsahuje seznam všech protokolů, které byly používány během relaci zachytávání a počet paketů odeslaných a přijatých pomocí protokolů.</span><span class="sxs-lookup"><span data-stu-id="8c9ac-159">This view provides a list of all the protocols that were in use during the capture session and the number of packets transmitted and received using the protocols.</span></span> <span data-ttu-id="8c9ac-160">Toto zobrazení může být užitečné pro hledání nežádoucí síťový provoz na virtuální počítače nebo v síti.</span><span class="sxs-lookup"><span data-stu-id="8c9ac-160">This view can be useful for finding unwanted network traffic on your virtual machines or network.</span></span>

![protokol hierarchie otevřít][3]

<span data-ttu-id="8c9ac-162">Jak vidíte na následujícím snímku obrazovky, se přenosů dat pomocí BitTorrent protokol, který se používá pro sdílení souborů typu peer to peer.</span><span class="sxs-lookup"><span data-stu-id="8c9ac-162">As you can see in the following screen capture, there was traffic using the BitTorrent protocol, which is used for peer to peer file sharing.</span></span> <span data-ttu-id="8c9ac-163">Jako správce neočekáváte najdete v části BitTorrent provozu na tomto konkrétní virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="8c9ac-163">As an administrator you do not expect to see BitTorrent traffic on this particular virtual machines.</span></span> <span data-ttu-id="8c9ac-164">Nyní si vědom tento provoz, můžete odebrat typu peer to peer software, který nainstalovat na tomto virtuálním počítači, nebo blokování provozu pomocí skupin zabezpečení sítě nebo brána Firewall.</span><span class="sxs-lookup"><span data-stu-id="8c9ac-164">Now you aware of this traffic, you can remove the peer to peer software that installed on this virtual machine, or block the traffic using Network Security Groups or a Firewall.</span></span> <span data-ttu-id="8c9ac-165">Kromě toho může zvolit paketu zachycení podle plánu, aby do použití protokolu na virtuálních počítačích můžete zkontrolovat pravidelně.</span><span class="sxs-lookup"><span data-stu-id="8c9ac-165">Additionally, you may elect to run packet captures on a schedule, so you can review the protocol use on your virtual machines regularly.</span></span> <span data-ttu-id="8c9ac-166">Příklad o tom, jak automatizovat úlohy sítě v azure najdete v článku [monitorování síťových prostředků pomocí azure automation](network-watcher-monitor-with-azure-automation.md)</span><span class="sxs-lookup"><span data-stu-id="8c9ac-166">For an example on how to automate network tasks in azure visit [Monitor network resources with azure automation](network-watcher-monitor-with-azure-automation.md)</span></span>

## <a name="finding-top-destinations-and-ports"></a><span data-ttu-id="8c9ac-167">Hledání hlavní cíle a porty</span><span class="sxs-lookup"><span data-stu-id="8c9ac-167">Finding top destinations and ports</span></span>

<span data-ttu-id="8c9ac-168">Principy typů přenosů, koncových bodů a porty přenášená přes je důležité při sledování nebo řešení potíží s aplikací a prostředkům v síti.</span><span class="sxs-lookup"><span data-stu-id="8c9ac-168">Understanding the types of traffic, the endpoints, and the ports communicated over is an important when monitoring or troubleshooting applications and resources on your network.</span></span> <span data-ttu-id="8c9ac-169">Použití souboru zachytávání paketů z výše, jsme můžete rychle zjistěte hlavní cíle, které naše počítač komunikuje s a porty využívání.</span><span class="sxs-lookup"><span data-stu-id="8c9ac-169">Utilizing a packet capture file from above, we can quickly learn the top destinations our VM is communicating with and the ports being utilized.</span></span>

### <a name="step-1"></a><span data-ttu-id="8c9ac-170">Krok 1</span><span class="sxs-lookup"><span data-stu-id="8c9ac-170">Step 1</span></span>

<span data-ttu-id="8c9ac-171">Pomocí stejné zachycení v předchozím scénáři, klikněte na **statistiky** > **IPv4 statistiky** > **cíle a porty**</span><span class="sxs-lookup"><span data-stu-id="8c9ac-171">Using the same capture in the previous scenario click **Statistics** > **IPv4 Statistics** > **Destinations and Ports**</span></span>

![okno zachytávání paketů][4]

### <a name="step-2"></a><span data-ttu-id="8c9ac-173">Krok 2</span><span class="sxs-lookup"><span data-stu-id="8c9ac-173">Step 2</span></span>

<span data-ttu-id="8c9ac-174">Jak jsme projděte výsledků, které vystupoval řádek, nebyly k dispozici více připojení na portu 111.</span><span class="sxs-lookup"><span data-stu-id="8c9ac-174">As we look through the results a line stands out, there were multiple connections on port 111.</span></span> <span data-ttu-id="8c9ac-175">Nejčastěji používaných portů byla 3389, což je Vzdálená plocha a zbývající jsou dynamické porty RPC.</span><span class="sxs-lookup"><span data-stu-id="8c9ac-175">The most used port was 3389, which is remote desktop, and the remaining are RPC dynamic ports.</span></span>

<span data-ttu-id="8c9ac-176">Tento provoz může to znamenat nic, je port, který byl použit pro mnoho připojení a neznámý správci.</span><span class="sxs-lookup"><span data-stu-id="8c9ac-176">While this traffic may mean nothing, it is a port that was used for many connections and is unknown to the administrator.</span></span>

![Obrázek 5][5]

### <a name="step-3"></a><span data-ttu-id="8c9ac-178">Krok 3</span><span class="sxs-lookup"><span data-stu-id="8c9ac-178">Step 3</span></span>

<span data-ttu-id="8c9ac-179">Teď, když jsme určili out of místní port jsme můžete filtrovat naší zachycení na port.</span><span class="sxs-lookup"><span data-stu-id="8c9ac-179">Now that we have determined an out of place port we can filter our capture based on the port.</span></span>

<span data-ttu-id="8c9ac-180">Filtr v tomto scénáři by byl:</span><span class="sxs-lookup"><span data-stu-id="8c9ac-180">The filter in this scenario would be:</span></span>

```
tcp.port == 111
```

<span data-ttu-id="8c9ac-181">Jsme do textového pole filtru zadejte text filtru z výše a stiskněte klávesu enter.</span><span class="sxs-lookup"><span data-stu-id="8c9ac-181">We enter the filter text from above in the filter textbox and hit enter.</span></span>

![Obrázek 6][6]

<span data-ttu-id="8c9ac-183">Ve výsledcích bychom můžete vidět, že veškerý provoz pochází z místního virtuálního počítače ve stejné podsíti.</span><span class="sxs-lookup"><span data-stu-id="8c9ac-183">From the results, we can see all the traffic is coming from a local virtual machine on the same subnet.</span></span> <span data-ttu-id="8c9ac-184">Pokud stále nemáte Chápeme, proč dochází k tento provoz, jsme můžete prohlédnout další paketů, chcete-li zjistit, proč v provádění těchto volání na portu 111.</span><span class="sxs-lookup"><span data-stu-id="8c9ac-184">If we still don’t understand why this traffic is occurring, we can further inspect the packets to determine why it is making these calls on port 111.</span></span> <span data-ttu-id="8c9ac-185">Tyto informace můžeme proveďte příslušnou akci.</span><span class="sxs-lookup"><span data-stu-id="8c9ac-185">With this information we can take the appropriate action.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8c9ac-186">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8c9ac-186">Next steps</span></span>

<span data-ttu-id="8c9ac-187">Další informace o dalších diagnostických funkcí sledovací proces sítě navštivte stránky [Přehled monitorování sítě Azure](network-watcher-monitoring-overview.md)</span><span class="sxs-lookup"><span data-stu-id="8c9ac-187">Learn about the other diagnostic features of Network Watcher by visiting [Azure network monitoring overview](network-watcher-monitoring-overview.md)</span></span>

[1]: ./media/network-watcher-deep-packet-inspection/figure1.png
[2]: ./media/network-watcher-deep-packet-inspection/figure2.png
[3]: ./media/network-watcher-deep-packet-inspection/figure3.png
[4]: ./media/network-watcher-deep-packet-inspection/figure4.png
[5]: ./media/network-watcher-deep-packet-inspection/figure5.png
[6]: ./media/network-watcher-deep-packet-inspection/figure6.png
[7]: ./media/network-watcher-deep-packet-inspection/figure7.png
[8]: ./media/network-watcher-deep-packet-inspection/figure8.png













