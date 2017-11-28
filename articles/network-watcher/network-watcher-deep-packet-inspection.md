---
title: "aaaPacket kontroly pomocí sledovací proces sítě Azure | Microsoft Docs"
description: "Tento článek popisuje, jak se shromažďují toouse sledovací proces sítě tooperform hloubkové kontroly paketů z virtuálního počítače"
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
ms.openlocfilehash: 4aeddcd482edc4df3d63e87b5c4b0788c540850b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="packet-inspection-with-azure-network-watcher"></a><span data-ttu-id="2e250-103">Kontroly paketů s sledovací proces sítě Azure</span><span class="sxs-lookup"><span data-stu-id="2e250-103">Packet inspection with Azure Network Watcher</span></span>

<span data-ttu-id="2e250-104">Pomocí funkce zachytávání paketů hello sledovací proces sítě, můžete zahájit a spravovat relace zachycení na virtuální počítače Azure z hello portálu, prostředí PowerShell, rozhraní příkazového řádku a programově prostřednictvím hello SDK a REST API.</span><span class="sxs-lookup"><span data-stu-id="2e250-104">Using hello packet capture feature of Network Watcher, you can initiate and manage captures sessions on your Azure VMs from hello portal, PowerShell, CLI, and programmatically through hello SDK and REST API.</span></span> <span data-ttu-id="2e250-105">Zachytáváním paketů umožňuje tooaddress scénáře, které vyžadují dat na úrovni paketů tím, že poskytuje hello informace ve snadno použitelného formátu.</span><span class="sxs-lookup"><span data-stu-id="2e250-105">Packet capture allows you tooaddress scenarios that require packet level data by providing hello information in a readily usable format.</span></span> <span data-ttu-id="2e250-106">Využití dat hello tooinspect volně dostupných nástrojů, můžete prozkoumat komunikace tooand odeslaný virtuální počítače a proniknout do vašeho síťového provozu.</span><span class="sxs-lookup"><span data-stu-id="2e250-106">Leveraging freely available tools tooinspect hello data, you can examine communications sent tooand from your VMs and gain insights into your network traffic.</span></span> <span data-ttu-id="2e250-107">Některé příklad použití paketu zachycení dat patří: příčin problémů sítě nebo aplikace, zjišťování sítě pokusy o zneužití a narušení nebo udržování dodržování legislativní předpisů.</span><span class="sxs-lookup"><span data-stu-id="2e250-107">Some example uses of packet capture data include: investigating network or application issues, detecting network misuse and intrusion attempts, or maintaining regulatory compliance.</span></span> <span data-ttu-id="2e250-108">V tomto článku jsme ukazují, jak tooopen souboru zachytávání paketů poskytuje sledovací proces sítě pomocí nástroje populární open source.</span><span class="sxs-lookup"><span data-stu-id="2e250-108">In this article, we show how tooopen a packet capture file provided by Network Watcher using a popular open source tool.</span></span> <span data-ttu-id="2e250-109">Poskytujeme také příklady, jak identifikovat neobvyklé provoz toocalculate latence připojení, a zkontrolujte síťových statistiky.</span><span class="sxs-lookup"><span data-stu-id="2e250-109">We will also provide examples showing how toocalculate a connection latency, identify abnormal traffic, and examine networking statistics.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="2e250-110">Než začnete</span><span class="sxs-lookup"><span data-stu-id="2e250-110">Before you begin</span></span>

<span data-ttu-id="2e250-111">Tento článek probírá některé scénáře předem nakonfigurovaná na zachytáváním paketů, která byla dříve spuštěna.</span><span class="sxs-lookup"><span data-stu-id="2e250-111">This article goes through some pre-configured scenarios on a packet capture that was run previously.</span></span> <span data-ttu-id="2e250-112">Tyto scénáře ukazují možnosti, které jsou přístupné kontrolou zachytáváním paketů.</span><span class="sxs-lookup"><span data-stu-id="2e250-112">These scenarios illustrate capabilities that can be accessed by reviewing a packet capture.</span></span> <span data-ttu-id="2e250-113">Tento scénář používá [WireShark](https://www.wireshark.org/) zachytáváním paketů tooinspect hello.</span><span class="sxs-lookup"><span data-stu-id="2e250-113">This scenario uses [WireShark](https://www.wireshark.org/) tooinspect hello packet capture.</span></span>

<span data-ttu-id="2e250-114">Tento scénář předpokládá, že jste již spustili zachytáváním paketů na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="2e250-114">This scenario assumes you already ran a packet capture on a virtual machine.</span></span> <span data-ttu-id="2e250-115">jak toocreate zachytáváním paketů navštivte toolearn [zachytávali spravovat pakety hello portál](network-watcher-packet-capture-manage-portal.md) nebo se zbytkem navštivte stránky [Správa Zachytávali pakety REST API](network-watcher-packet-capture-manage-rest.md).</span><span class="sxs-lookup"><span data-stu-id="2e250-115">toolearn how toocreate a packet capture visit [Manage packet captures with hello portal](network-watcher-packet-capture-manage-portal.md) or with REST by visiting [Managing Packet Captures with REST API](network-watcher-packet-capture-manage-rest.md).</span></span>

## <a name="scenario"></a><span data-ttu-id="2e250-116">Scénář</span><span class="sxs-lookup"><span data-stu-id="2e250-116">Scenario</span></span>

<span data-ttu-id="2e250-117">V tomto scénáři můžete:</span><span class="sxs-lookup"><span data-stu-id="2e250-117">In this scenario, you:</span></span>

* <span data-ttu-id="2e250-118">Zkontrolujte zachytávání paketů</span><span class="sxs-lookup"><span data-stu-id="2e250-118">Review a packet capture</span></span>

## <a name="calculate-network-latency"></a><span data-ttu-id="2e250-119">Vypočítat latence sítě</span><span class="sxs-lookup"><span data-stu-id="2e250-119">Calculate network latency</span></span>

<span data-ttu-id="2e250-120">V tomto scénáři ukážeme, jak tooview hello počáteční odezvy čas požadavku protokolu TCP (Transmission Control) konverzace, ke kterým dochází mezi dva koncové body.</span><span class="sxs-lookup"><span data-stu-id="2e250-120">In this scenario, we show how tooview hello initial Round Trip Time (RTT) of a Transmission Control Protocol (TCP) conversation occurring between two endpoints.</span></span>

<span data-ttu-id="2e250-121">Po vytvoření připojení protokolu TCP, hello první tři paketů odeslaných za hello připojení postupujte podle vzoru obvykle označuje tooas hello třícestný metody handshake.</span><span class="sxs-lookup"><span data-stu-id="2e250-121">When a TCP connection is established, hello first three packets sent in hello connection follow a pattern commonly referred tooas hello three-way handshake.</span></span> <span data-ttu-id="2e250-122">Prověřením hello první dva paketů odeslaných za této metody handshake, úvodního požadavku od klienta hello a odpověď ze serveru hello jsme můžete vypočítat hello latence při toto připojení bylo vytvořeno.</span><span class="sxs-lookup"><span data-stu-id="2e250-122">By examining hello first two packets sent in this handshake, an initial request from hello client and a response from hello server, we can calculate hello latency when this connection was established.</span></span> <span data-ttu-id="2e250-123">Tato čekací doba je odkazované tooas hello doba odezvy (požadavku).</span><span class="sxs-lookup"><span data-stu-id="2e250-123">This latency is referred tooas hello Round Trip Time (RTT).</span></span> <span data-ttu-id="2e250-124">Další informace o protokolu TCP hello a hello třícestné najdete v části toohello následujících prostředků.</span><span class="sxs-lookup"><span data-stu-id="2e250-124">For more information on hello TCP protocol and hello three-way handshake refer toohello following resource.</span></span> <span data-ttu-id="2e250-125">https://support.microsoft.com/en-US/Help/172983/Explanation-of-the-three-way-handshake-VIA-TCP-IP</span><span class="sxs-lookup"><span data-stu-id="2e250-125">https://support.microsoft.com/en-us/help/172983/explanation-of-the-three-way-handshake-via-tcp-ip</span></span>

### <a name="step-1"></a><span data-ttu-id="2e250-126">Krok 1</span><span class="sxs-lookup"><span data-stu-id="2e250-126">Step 1</span></span>

<span data-ttu-id="2e250-127">Spusťte WireShark</span><span class="sxs-lookup"><span data-stu-id="2e250-127">Launch WireShark</span></span>

### <a name="step-2"></a><span data-ttu-id="2e250-128">Krok 2</span><span class="sxs-lookup"><span data-stu-id="2e250-128">Step 2</span></span>

<span data-ttu-id="2e250-129">Zatížení hello **CAP** soubor z vaší zachytáváním paketů.</span><span class="sxs-lookup"><span data-stu-id="2e250-129">Load hello **.cap** file from your packet capture.</span></span> <span data-ttu-id="2e250-130">Tento soubor nachází v objektu blob hello se uloží do našich místně na virtuální počítač hello, v závislosti na tom, jak jste nakonfigurovali.</span><span class="sxs-lookup"><span data-stu-id="2e250-130">This file can be found in hello blob it was saved in our locally on hello virtual machine, depending on how you configured it.</span></span>

### <a name="step-3"></a><span data-ttu-id="2e250-131">Krok 3</span><span class="sxs-lookup"><span data-stu-id="2e250-131">Step 3</span></span>

<span data-ttu-id="2e250-132">tooview hello počáteční odezvy čas požadavku v TCP konverzace, jsme bude pouze vidí první dva pakety hello zahrnutých v hello TCP handshake.</span><span class="sxs-lookup"><span data-stu-id="2e250-132">tooview hello initial Round Trip Time (RTT) in TCP conversations, we will only be looking at hello first two packets involved in hello TCP handshake.</span></span> <span data-ttu-id="2e250-133">Použijeme hello první dva pakety hello třícestné, kde jsou hello [SYN], [SYN, ACK] paketů.</span><span class="sxs-lookup"><span data-stu-id="2e250-133">We will be using hello first two packets in hello three-way handshake, which are hello [SYN], [SYN, ACK] packets.</span></span> <span data-ttu-id="2e250-134">Že jsou pojmenované pro nastaveny v hlavičce protokolu TCP hello příznaky.</span><span class="sxs-lookup"><span data-stu-id="2e250-134">They are named for flags set in hello TCP header.</span></span> <span data-ttu-id="2e250-135">v tomto scénáři se nepoužijí Hello poslední paketů v hello handshake, hello [ACK] paketů.</span><span class="sxs-lookup"><span data-stu-id="2e250-135">hello last packet in hello handshake, hello [ACK] packet, will not be used in this scenario.</span></span> <span data-ttu-id="2e250-136">hello klient odešle paket Hello [SYN].</span><span class="sxs-lookup"><span data-stu-id="2e250-136">hello [SYN] packet is sent by hello client.</span></span> <span data-ttu-id="2e250-137">Po přijetí je hello server odešle paket hello [ACK] jako potvrzení přijetí hello SYN z klienta hello.</span><span class="sxs-lookup"><span data-stu-id="2e250-137">Once it is received hello server sends hello [ACK] packet as an acknowledgement of receiving hello SYN from hello client.</span></span> <span data-ttu-id="2e250-138">Využití hello fakt, že odpověď serveru hello vyžaduje velmi malé nároky, jsme vypočítat hello odeslání požadavku odečtením hello době hello [SYN, ACK] paketu byla přijata klientem hello hello čas [SYN] paket klientem hello.</span><span class="sxs-lookup"><span data-stu-id="2e250-138">Leveraging hello fact that hello server’s response requires very little overhead, we calculate hello RTT by subtracting hello time hello [SYN, ACK] packet was received by hello client by hello time [SYN] packet was sent by hello client.</span></span>

<span data-ttu-id="2e250-139">Pomocí WireShark tato hodnota je vypočítána pro nás.</span><span class="sxs-lookup"><span data-stu-id="2e250-139">Using WireShark this value is calculated for us.</span></span>

<span data-ttu-id="2e250-140">první dva pakety hello toomore snadno zobrazit v hello TCP třícestné, jsme, že bude využívat hello filtrování schopnosti poskytované WireShark.</span><span class="sxs-lookup"><span data-stu-id="2e250-140">toomore easily view hello first two packets in hello TCP three-way handshake, we will utilize hello filtering capability provided by WireShark.</span></span>

<span data-ttu-id="2e250-141">Filtr hello tooapply v WireShark, rozbalte hello "Transmission Control Protocol" Segment paketu [SYN] na vašem snímku a prozkoumejte nastaveny v hlavičce protokolu TCP hello příznaky hello.</span><span class="sxs-lookup"><span data-stu-id="2e250-141">tooapply hello filter in WireShark, expand hello “Transmission Control Protocol” Segment of a [SYN] packet in your capture and examine hello flags set in hello TCP header.</span></span>

<span data-ttu-id="2e250-142">Vzhledem k tomu, že jsme hledáte toofilter na všechny [SYN] a [SYN, ACK] paketů, v části Příznaky cofirm, že je hello Syn bit nastavený too1, a potom klikněte pravým tlačítkem na hello Syn bit -> použít jako filtr -> vybrané.</span><span class="sxs-lookup"><span data-stu-id="2e250-142">Since we are looking toofilter on all [SYN] and [SYN, ACK] packets, under flags cofirm that hello Syn bit is set too1, then right click on hello Syn bit -> Apply as Filter -> Selected.</span></span>

![Obrázek 7][7]

### <a name="step-4"></a><span data-ttu-id="2e250-144">Krok 4</span><span class="sxs-lookup"><span data-stu-id="2e250-144">Step 4</span></span>

<span data-ttu-id="2e250-145">Teď, když jste provedli filtrování hello okno tooonly najdete pakety s nastaven bit hello [SYN], můžete snadno vybrat konverzace, které vás zajímají tooview hello počáteční požadavku.</span><span class="sxs-lookup"><span data-stu-id="2e250-145">Now that you have filtered hello window tooonly see packets with hello [SYN] bit set, you can easily select conversations you are interested in tooview hello initial RTT.</span></span> <span data-ttu-id="2e250-146">Jednoduchý způsob tooview hello požadavku v WireShark jednoduše klikněte na rozevírací hello označena analysis "SEQ/ACK".</span><span class="sxs-lookup"><span data-stu-id="2e250-146">A simple way tooview hello RTT in WireShark simply click hello dropdown marked “SEQ/ACK” analysis.</span></span> <span data-ttu-id="2e250-147">Zobrazí se text hello, zobrazí požadavku.</span><span class="sxs-lookup"><span data-stu-id="2e250-147">You will then see hello RTT displayed.</span></span> <span data-ttu-id="2e250-148">V takovém případě hello požadavku byla 0.0022114 sekund nebo 2.211 ms.</span><span class="sxs-lookup"><span data-stu-id="2e250-148">In this case, hello RTT was 0.0022114 seconds, or 2.211 ms.</span></span>

![Obrázek 8][8]

## <a name="unwanted-protocols"></a><span data-ttu-id="2e250-150">Nežádoucí protokoly</span><span class="sxs-lookup"><span data-stu-id="2e250-150">Unwanted protocols</span></span>

<span data-ttu-id="2e250-151">Můžete mít mnoho aplikací běžících na instanci virtuálního počítače, které jste nasadili v Azure.</span><span class="sxs-lookup"><span data-stu-id="2e250-151">You can have many applications running on a virtual machine instance you have deployed in Azure.</span></span> <span data-ttu-id="2e250-152">Mnoho z těchto aplikací komunikují přes síť hello, možná bez explicitního oprávnění.</span><span class="sxs-lookup"><span data-stu-id="2e250-152">Many of these applications communicate over hello network, perhaps without your explicit permission.</span></span> <span data-ttu-id="2e250-153">Pomocí paketu zachycení toostore síťové komunikace, jsme prozkoumejte jak aplikace mluvíme v síti hello a vyhledejte všechny problémy.</span><span class="sxs-lookup"><span data-stu-id="2e250-153">Using packet capture toostore network communication, we can investigate how application are talking on hello network and look for any issues.</span></span>

<span data-ttu-id="2e250-154">V tomto příkladu jsme Zkontrolujte předchozí spustili zachytáváním paketů pro nežádoucí protokoly, které můžou signalizovat neověřené komunikace z aplikace běžící na vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="2e250-154">In this example, we review a previous ran packet capture for unwanted protocols that may indicate unauthorized communication from an application running on your machine.</span></span>

### <a name="step-1"></a><span data-ttu-id="2e250-155">Krok 1</span><span class="sxs-lookup"><span data-stu-id="2e250-155">Step 1</span></span>

<span data-ttu-id="2e250-156">Pomocí hello stejné zachycení v hello předchozí scénář, klikněte na tlačítko **statistiky** > **protokol hierarchie**</span><span class="sxs-lookup"><span data-stu-id="2e250-156">Using hello same capture in hello previous scenario click **Statistics** > **Protocol Hierarchy**</span></span>

![protokol hierarchie nabídky][2]

<span data-ttu-id="2e250-158">Zobrazí se okno hierarchie protokol Hello.</span><span class="sxs-lookup"><span data-stu-id="2e250-158">hello protocol hierarchy window appears.</span></span> <span data-ttu-id="2e250-159">Toto zobrazení obsahuje seznam všech hello protokolů, které byly používány během relace zachytávání hello a hello počet paketů odeslaných a přijatých pomocí protokolů hello.</span><span class="sxs-lookup"><span data-stu-id="2e250-159">This view provides a list of all hello protocols that were in use during hello capture session and hello number of packets transmitted and received using hello protocols.</span></span> <span data-ttu-id="2e250-160">Toto zobrazení může být užitečné pro hledání nežádoucí síťový provoz na virtuální počítače nebo v síti.</span><span class="sxs-lookup"><span data-stu-id="2e250-160">This view can be useful for finding unwanted network traffic on your virtual machines or network.</span></span>

![protokol hierarchie otevřít][3]

<span data-ttu-id="2e250-162">Jak vidíte v hello následující snímek obrazovky, se přenosů dat pomocí hello BitTorrent protokol, který se používá pro sdílení souborů toopeer partnera.</span><span class="sxs-lookup"><span data-stu-id="2e250-162">As you can see in hello following screen capture, there was traffic using hello BitTorrent protocol, which is used for peer toopeer file sharing.</span></span> <span data-ttu-id="2e250-163">Jako správce neočekáváte toosee BitTorrent provozu na tomto konkrétní virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="2e250-163">As an administrator you do not expect toosee BitTorrent traffic on this particular virtual machines.</span></span> <span data-ttu-id="2e250-164">Nyní si vědom tento provoz odebráním hello sdílené toopeer software, který nainstalovaný na tento virtuální počítač nebo hello bloku přenosů pomocí skupin zabezpečení sítě nebo brána Firewall.</span><span class="sxs-lookup"><span data-stu-id="2e250-164">Now you aware of this traffic, you can remove hello peer toopeer software that installed on this virtual machine, or block hello traffic using Network Security Groups or a Firewall.</span></span> <span data-ttu-id="2e250-165">Kromě toho může zvolit toorun paketu zachycení podle plánu, tak hello použití protokolu na virtuálních počítačích můžete zkontrolovat pravidelně.</span><span class="sxs-lookup"><span data-stu-id="2e250-165">Additionally, you may elect toorun packet captures on a schedule, so you can review hello protocol use on your virtual machines regularly.</span></span> <span data-ttu-id="2e250-166">Příklad na jak tooautomate sítě úlohy v azure naleznete [monitorování síťových prostředků pomocí azure automation](network-watcher-monitor-with-azure-automation.md)</span><span class="sxs-lookup"><span data-stu-id="2e250-166">For an example on how tooautomate network tasks in azure visit [Monitor network resources with azure automation](network-watcher-monitor-with-azure-automation.md)</span></span>

## <a name="finding-top-destinations-and-ports"></a><span data-ttu-id="2e250-167">Hledání hlavní cíle a porty</span><span class="sxs-lookup"><span data-stu-id="2e250-167">Finding top destinations and ports</span></span>

<span data-ttu-id="2e250-168">Principy typů hello provozu, hello koncových bodů a přenášená přes porty hello je důležité při sledování nebo řešení potíží s aplikací a prostředkům v síti.</span><span class="sxs-lookup"><span data-stu-id="2e250-168">Understanding hello types of traffic, hello endpoints, and hello ports communicated over is an important when monitoring or troubleshooting applications and resources on your network.</span></span> <span data-ttu-id="2e250-169">Použití souboru zachytávání paketů z výše, jsme můžete rychle zjistěte hello Nejoblíbenější cíle naše virtuální počítač komunikuje s a hello porty využívání.</span><span class="sxs-lookup"><span data-stu-id="2e250-169">Utilizing a packet capture file from above, we can quickly learn hello top destinations our VM is communicating with and hello ports being utilized.</span></span>

### <a name="step-1"></a><span data-ttu-id="2e250-170">Krok 1</span><span class="sxs-lookup"><span data-stu-id="2e250-170">Step 1</span></span>

<span data-ttu-id="2e250-171">Pomocí hello stejné zachycení v hello předchozí scénář, klikněte na tlačítko **statistiky** > **IPv4 statistiky** > **cíle a porty**</span><span class="sxs-lookup"><span data-stu-id="2e250-171">Using hello same capture in hello previous scenario click **Statistics** > **IPv4 Statistics** > **Destinations and Ports**</span></span>

![okno zachytávání paketů][4]

### <a name="step-2"></a><span data-ttu-id="2e250-173">Krok 2</span><span class="sxs-lookup"><span data-stu-id="2e250-173">Step 2</span></span>

<span data-ttu-id="2e250-174">Jako podíváme prostřednictvím hello výsledky vystupoval řádek, nebyly k dispozici více připojení na portu 111.</span><span class="sxs-lookup"><span data-stu-id="2e250-174">As we look through hello results a line stands out, there were multiple connections on port 111.</span></span> <span data-ttu-id="2e250-175">Hello nejvíce používá port byla 3389, což je Vzdálená plocha a zbývající hello jsou dynamické porty RPC.</span><span class="sxs-lookup"><span data-stu-id="2e250-175">hello most used port was 3389, which is remote desktop, and hello remaining are RPC dynamic ports.</span></span>

<span data-ttu-id="2e250-176">Tento provoz může to znamenat nic, je port, který byl použit pro mnoho připojení a neznámé toohello oprávnění správce.</span><span class="sxs-lookup"><span data-stu-id="2e250-176">While this traffic may mean nothing, it is a port that was used for many connections and is unknown toohello administrator.</span></span>

![Obrázek 5][5]

### <a name="step-3"></a><span data-ttu-id="2e250-178">Krok 3</span><span class="sxs-lookup"><span data-stu-id="2e250-178">Step 3</span></span>

<span data-ttu-id="2e250-179">Teď, když jsme určili out of místní port jsme můžete filtrovat naší zachycení na hello port.</span><span class="sxs-lookup"><span data-stu-id="2e250-179">Now that we have determined an out of place port we can filter our capture based on hello port.</span></span>

<span data-ttu-id="2e250-180">Filtr Hello v tomto scénáři by byl:</span><span class="sxs-lookup"><span data-stu-id="2e250-180">hello filter in this scenario would be:</span></span>

```
tcp.port == 111
```

<span data-ttu-id="2e250-181">Jsme zadejte text filtru hello z výše v textovém poli filtru hello a stisknutí klávesy enter.</span><span class="sxs-lookup"><span data-stu-id="2e250-181">We enter hello filter text from above in hello filter textbox and hit enter.</span></span>

![Obrázek 6][6]

<span data-ttu-id="2e250-183">Z výsledků hello uvidíme veškerý provoz hello pochází z místní virtuální počítač na hello stejné podsíti.</span><span class="sxs-lookup"><span data-stu-id="2e250-183">From hello results, we can see all hello traffic is coming from a local virtual machine on hello same subnet.</span></span> <span data-ttu-id="2e250-184">Pokud stále nemáte Chápeme, proč dochází k tento provoz, jsme můžete prohlédnout další toodetermine pakety hello Proč v provádění těchto volání na portu 111.</span><span class="sxs-lookup"><span data-stu-id="2e250-184">If we still don’t understand why this traffic is occurring, we can further inspect hello packets toodetermine why it is making these calls on port 111.</span></span> <span data-ttu-id="2e250-185">Tyto informace budeme moci provést vhodnou akci hello.</span><span class="sxs-lookup"><span data-stu-id="2e250-185">With this information we can take hello appropriate action.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2e250-186">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2e250-186">Next steps</span></span>

<span data-ttu-id="2e250-187">Další informace o hello jiné diagnostické funkce sledovací proces sítě navštivte stránky [Přehled monitorování sítě Azure](network-watcher-monitoring-overview.md)</span><span class="sxs-lookup"><span data-stu-id="2e250-187">Learn about hello other diagnostic features of Network Watcher by visiting [Azure network monitoring overview](network-watcher-monitoring-overview.md)</span></span>

[1]: ./media/network-watcher-deep-packet-inspection/figure1.png
[2]: ./media/network-watcher-deep-packet-inspection/figure2.png
[3]: ./media/network-watcher-deep-packet-inspection/figure3.png
[4]: ./media/network-watcher-deep-packet-inspection/figure4.png
[5]: ./media/network-watcher-deep-packet-inspection/figure5.png
[6]: ./media/network-watcher-deep-packet-inspection/figure6.png
[7]: ./media/network-watcher-deep-packet-inspection/figure7.png
[8]: ./media/network-watcher-deep-packet-inspection/figure8.png













