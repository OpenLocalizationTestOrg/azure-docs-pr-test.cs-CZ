---
title: "Ověření sítě VPN propustnost tooa Microsoft Azure Virtual Network | Microsoft Docs"
description: "Hello účelem tohoto dokumentu je toohelp uživatele ověřit propustnost sítě hello ze své místní prostředky tooan virtuální počítač Azure."
services: vpn-gateway
documentationcenter: na
author: chadmath
manager: jasmc
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/10/2017
ms.author: radwiv;chadmat;genli
ms.openlocfilehash: 9cf0b67145645a3c2a9556b0cc910066cc62a9ab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toovalidate-vpn-throughput-tooa-virtual-network"></a><span data-ttu-id="2a306-103">Jak toovalidate VPN propustnost tooa virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="2a306-103">How toovalidate VPN throughput tooa virtual network</span></span>

<span data-ttu-id="2a306-104">Připojení k bráně VPN vám umožní zabezpečit připojení mezi různými místy tooestablish mezi vaší virtuální sítě v rámci Azure a místní infrastrukturu IT.</span><span class="sxs-lookup"><span data-stu-id="2a306-104">A VPN gateway connection enables you tooestablish secure, cross-premises connectivity between your Virtual Network within Azure and your on-premises IT infrastructure.</span></span>

<span data-ttu-id="2a306-105">Tento článek ukazuje, jak toovalidate propustnost sítě z hello místní prostředky tooan virtuální počítač Azure.</span><span class="sxs-lookup"><span data-stu-id="2a306-105">This article shows how toovalidate network throughput from hello on-premises resources tooan Azure virtual machine.</span></span> <span data-ttu-id="2a306-106">Obsahuje také pokyny k odstraňování problémů.</span><span class="sxs-lookup"><span data-stu-id="2a306-106">It also provides troubleshooting guidance.</span></span>

>[!NOTE]
><span data-ttu-id="2a306-107">Tento článek je určený toohelp Diagnostika a řešení běžných problémů.</span><span class="sxs-lookup"><span data-stu-id="2a306-107">This article is intended toohelp diagnose and fix common issues.</span></span> <span data-ttu-id="2a306-108">Pokud jste nelze toosolve hello problém pomocí hello následující informace, [obraťte se na podporu](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span><span class="sxs-lookup"><span data-stu-id="2a306-108">If you're unable toosolve hello issue by using hello following information, [contact support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span></span>
>
>

## <a name="overview"></a><span data-ttu-id="2a306-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="2a306-109">Overview</span></span>

<span data-ttu-id="2a306-110">Hello připojení brány sítě VPN zahrnuje hello následující součásti:</span><span class="sxs-lookup"><span data-stu-id="2a306-110">hello VPN gateway connection involves hello following components:</span></span>

- <span data-ttu-id="2a306-111">Místní zařízení VPN (zobrazení seznamu [ověřit zařízení VPN)](vpn-gateway-about-vpn-devices.md#devicetable).</span><span class="sxs-lookup"><span data-stu-id="2a306-111">On-premises VPN device (view a list of [validated VPN devices)](vpn-gateway-about-vpn-devices.md#devicetable).</span></span>
- <span data-ttu-id="2a306-112">Veřejný Internet</span><span class="sxs-lookup"><span data-stu-id="2a306-112">Public Internet</span></span>
- <span data-ttu-id="2a306-113">Služba Azure VPN gateway</span><span class="sxs-lookup"><span data-stu-id="2a306-113">Azure VPN gateway</span></span>
- <span data-ttu-id="2a306-114">Virtuální počítače Azure</span><span class="sxs-lookup"><span data-stu-id="2a306-114">Azure virtual machine</span></span>

<span data-ttu-id="2a306-115">Hello následující diagram znázorňuje hello logické připojení místní síti tooan virtuální síť Azure prostřednictvím sítě VPN.</span><span class="sxs-lookup"><span data-stu-id="2a306-115">hello following diagram shows hello logical connectivity of an on-premises network tooan Azure virtual network through VPN.</span></span>

![TooMSFT logické síti zákazníka připojení k síti pomocí sítě VPN](./media/vpn-gateway-validate-throughput-to-vnet/VPNPerf.png)

## <a name="calculate-hello-maximum-expected-ingressegress"></a><span data-ttu-id="2a306-117">Vypočítat hello maximální očekávaný vstup/výstup</span><span class="sxs-lookup"><span data-stu-id="2a306-117">Calculate hello maximum expected ingress/egress</span></span>

1.  <span data-ttu-id="2a306-118">Určete požadavky na propustnost směrného plánu vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="2a306-118">Determine your application's baseline throughput requirements.</span></span>
2.  <span data-ttu-id="2a306-119">Určení vaší omezení propustnosti brány Azure VPN.</span><span class="sxs-lookup"><span data-stu-id="2a306-119">Determine your Azure VPN gateway throughput limits.</span></span> <span data-ttu-id="2a306-120">Nápovědu najdete v části hello "agregovaná propustnost podle typů SKU a síť VPN" [plánování a návrhu pro bránu VPN](vpn-gateway-plan-design.md).</span><span class="sxs-lookup"><span data-stu-id="2a306-120">For help, see hello "Aggregate throughput by SKU and VPN type" section of [Planning and design for VPN Gateway](vpn-gateway-plan-design.md).</span></span>
3.  <span data-ttu-id="2a306-121">Určení hello [virtuálního počítače Azure propustnost pokyny](../virtual-machines/virtual-machines-windows-sizes.md) pro vaše velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="2a306-121">Determine hello [Azure VM throughput guidance](../virtual-machines/virtual-machines-windows-sizes.md) for your VM size.</span></span>
4.  <span data-ttu-id="2a306-122">Určuje šířku pásma vašeho poskytovatele služeb sítě Internet (ISP).</span><span class="sxs-lookup"><span data-stu-id="2a306-122">Determine your Internet Service Provider (ISP) bandwidth.</span></span>
5.  <span data-ttu-id="2a306-123">Vypočítat očekávané propustnost - minimálně šířka pásma (virtuální počítač, brány, poskytovatele internetových služeb) * 0,8.</span><span class="sxs-lookup"><span data-stu-id="2a306-123">Calculate your expected throughput - Least bandwidth of (VM, Gateway, ISP) * 0.8.</span></span>

<span data-ttu-id="2a306-124">Pokud vaše počítané propustnost nesplňuje požadavky na propustnost směrného plánu vaší aplikace, musíte šířky pásma hello tooincrease hello prostředku, který jste určili jako hello potíže.</span><span class="sxs-lookup"><span data-stu-id="2a306-124">If your calculated throughput does not meet your application's baseline throughput requirements, you need tooincrease hello bandwidth of hello resource that you identified as hello bottleneck.</span></span> <span data-ttu-id="2a306-125">tooresize služby Azure VPN Gateway najdete v části [změna skladová položka brány](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#gwsku).</span><span class="sxs-lookup"><span data-stu-id="2a306-125">tooresize an Azure VPN Gateway, see [Changing a gateway SKU](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#gwsku).</span></span> <span data-ttu-id="2a306-126">tooresize virtuálního počítače, najdete v části [změnit velikost virtuálního počítače](../virtual-machines/virtual-machines-windows-resize-vm.md).</span><span class="sxs-lookup"><span data-stu-id="2a306-126">tooresize a virtual machine, see [Resize a VM](../virtual-machines/virtual-machines-windows-resize-vm.md).</span></span> <span data-ttu-id="2a306-127">Pokud k těmto problémům očekávané šířky pásma Internetu, můžete také toocontact vašeho poskytovatele internetových služeb.</span><span class="sxs-lookup"><span data-stu-id="2a306-127">If you are not experiencing expected Internet bandwidth, you may also want toocontact your ISP.</span></span>

## <a name="validate-network-throughput-by-using-performance-tools"></a><span data-ttu-id="2a306-128">Ověření propustnost sítě pomocí nástroje pro sledování výkonu</span><span class="sxs-lookup"><span data-stu-id="2a306-128">Validate network throughput by using performance tools</span></span>

<span data-ttu-id="2a306-129">Toto ověření je třeba provést v době mimo špičku jako sytost propustnost tunelové propojení VPN během testování není poskytnuta přesné výsledky.</span><span class="sxs-lookup"><span data-stu-id="2a306-129">This validation should be performed during non-peak hours, as VPN tunnel throughput saturation during testing does not give accurate results.</span></span>

<span data-ttu-id="2a306-130">Hello nástroj, který jsme použít pro tento test je iPerf, který má klient i server režimy a funguje v systému Windows a Linux.</span><span class="sxs-lookup"><span data-stu-id="2a306-130">hello tool we use for this test is iPerf, which works on both Windows and Linux and has both client and server modes.</span></span> <span data-ttu-id="2a306-131">Je omezená too3 GB/s pro virtuální počítače Windows.</span><span class="sxs-lookup"><span data-stu-id="2a306-131">It is limited too3 Gbps for Windows VMs.</span></span>

<span data-ttu-id="2a306-132">Tento nástroj neprovádí žádné operace toodisk pro čtení a zápis.</span><span class="sxs-lookup"><span data-stu-id="2a306-132">This tool does not perform any read/write operations toodisk.</span></span> <span data-ttu-id="2a306-133">Vytvoří výhradně generovaný sám sebou TCP provoz z jednoho koncového toohello jiné.</span><span class="sxs-lookup"><span data-stu-id="2a306-133">It solely produces self-generated TCP traffic from one end toohello other.</span></span> <span data-ttu-id="2a306-134">Vygeneroval statistiky podle experimenty, které měří hello šířky pásma mezi klientem a serverem uzly k dispozici.</span><span class="sxs-lookup"><span data-stu-id="2a306-134">It generated statistics based on experimentation that measures hello bandwidth available between client and server nodes.</span></span> <span data-ttu-id="2a306-135">Při testování mezi dvěma uzly, jeden funguje jako hello server a hello jiných jako klient.</span><span class="sxs-lookup"><span data-stu-id="2a306-135">When testing between two nodes, one acts as hello server and hello other as a client.</span></span> <span data-ttu-id="2a306-136">Po dokončení se tento test, doporučujeme zpětné hello role tootest obě nahrávání a stahování propustnost v obou uzlech.</span><span class="sxs-lookup"><span data-stu-id="2a306-136">Once this test is completed, we recommend that you reverse hello roles tootest both upload and download throughput on both nodes.</span></span>

### <a name="download-iperf"></a><span data-ttu-id="2a306-137">Stažení nástroje iPerf</span><span class="sxs-lookup"><span data-stu-id="2a306-137">Download iPerf</span></span>
<span data-ttu-id="2a306-138">Stáhněte si [iPerf](https://iperf.fr/download/iperf_3.1/iperf-3.1.2-win64.zip).</span><span class="sxs-lookup"><span data-stu-id="2a306-138">Download [iPerf](https://iperf.fr/download/iperf_3.1/iperf-3.1.2-win64.zip).</span></span> <span data-ttu-id="2a306-139">Podrobnosti najdete v tématu [iPerf dokumentaci](https://iperf.fr/iperf-doc.php).</span><span class="sxs-lookup"><span data-stu-id="2a306-139">For details, see [iPerf documentation](https://iperf.fr/iperf-doc.php).</span></span>

 >[!NOTE]
 ><span data-ttu-id="2a306-140">Hello produkty třetích stran, které tento článek popisuje, pocházejí od společností, které jsou nezávislé na Microsoftu.</span><span class="sxs-lookup"><span data-stu-id="2a306-140">hello third-party products that this article discusses are manufactured by companies that are independent of Microsoft.</span></span> <span data-ttu-id="2a306-141">Společnost Microsoft neposkytuje žádné záruky, předpokládanou ani jinou o hello výkonem a spolehlivostí těchto produktů.</span><span class="sxs-lookup"><span data-stu-id="2a306-141">Microsoft makes no warranty, implied or otherwise, about hello performance or reliability of these products.</span></span>
 >
 >

### <a name="run-iperf-iperf3exe"></a><span data-ttu-id="2a306-142">Spuštění nástroje iPerf (iperf3.exe)</span><span class="sxs-lookup"><span data-stu-id="2a306-142">Run iPerf (iperf3.exe)</span></span>
1. <span data-ttu-id="2a306-143">Povolte pravidlo NSG nebo seznamu ACL umožňuje hello provoz (pro veřejné IP adresy testování na virtuálním počítači Azure).</span><span class="sxs-lookup"><span data-stu-id="2a306-143">Enable an NSG/ACL rule allowing hello traffic (for public IP address testing on Azure VM).</span></span>

2. <span data-ttu-id="2a306-144">V obou uzlech povolte výjimku brány firewall pro port 5001.</span><span class="sxs-lookup"><span data-stu-id="2a306-144">On both nodes, enable a firewall exception for port 5001.</span></span>

    <span data-ttu-id="2a306-145">**Windows:** následující hello spustit příkaz jako správce:</span><span class="sxs-lookup"><span data-stu-id="2a306-145">**Windows:** Run hello following command as an administrator:</span></span>

    ```CMD
    netsh advfirewall firewall add rule name="Open Port 5001" dir=in action=allow protocol=TCP localport=5001
    ```

    <span data-ttu-id="2a306-146">pravidlo hello tooremove při testování je dokončena, spusťte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="2a306-146">tooremove hello rule when testing is complete, run this command:</span></span>

    ```CMD
    netsh advfirewall firewall delete rule name="Open Port 5001" protocol=TCP localport=5001
    ```
    </br>
    <span data-ttu-id="2a306-147">**Azure Linux:** obrázků Azure Linux je projektovou brány firewall.</span><span class="sxs-lookup"><span data-stu-id="2a306-147">**Azure Linux:**  Azure Linux images have permissive firewalls.</span></span> <span data-ttu-id="2a306-148">Pokud je aplikace, která naslouchá na portu, hello provoz je povolený prostřednictvím.</span><span class="sxs-lookup"><span data-stu-id="2a306-148">If there is an application listening on a port, hello traffic is allowed through.</span></span> <span data-ttu-id="2a306-149">Vlastní Image, které jsou zabezpečené může být nutné explicitně otevřené porty.</span><span class="sxs-lookup"><span data-stu-id="2a306-149">Custom images that are secured may need ports opened explicitly.</span></span> <span data-ttu-id="2a306-150">Běžné brány firewall vrstvy operačního systému Linux zahrnují `iptables`, `ufw`, nebo `firewalld`.</span><span class="sxs-lookup"><span data-stu-id="2a306-150">Common Linux OS-layer firewalls include `iptables`, `ufw`, or `firewalld`.</span></span>

3. <span data-ttu-id="2a306-151">Na uzlu serveru hello změňte adresář toohello, které je extrahován iperf3.exe.</span><span class="sxs-lookup"><span data-stu-id="2a306-151">On hello server node, change toohello directory where iperf3.exe is extracted.</span></span> <span data-ttu-id="2a306-152">Potom spusťte iPerf v režimu serveru a nastavte ji toolisten na portu 5001 jako hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="2a306-152">Then run iPerf in server mode and set it toolisten on port 5001 as hello following commands:</span></span>

     ```CMD
     cd c:\iperf-3.1.2-win65

     iperf3.exe -s -p 5001
     ```

4. <span data-ttu-id="2a306-153">V uzlu hello klienta změna toohello adresáře, kde je nástroj iperf extrahovat a pak spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="2a306-153">On hello client node, change toohello directory where iperf tool is extracted and then run hello following command:</span></span>

    ```CMD
    iperf3.exe -c <IP of hello iperf Server> -t 30 -p 5001 -P 32
    ```

    <span data-ttu-id="2a306-154">Klient Hello způsobuje přenosy na portu 5001 toohello serveru po dobu 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="2a306-154">hello client is inducing traffic on port 5001 toohello server for 30 seconds.</span></span> <span data-ttu-id="2a306-155">Hello příznak '-P "určující používáme 32 uzel serveru toohello souběžných připojení.</span><span class="sxs-lookup"><span data-stu-id="2a306-155">hello flag '-P ' that indicates we are using 32 simultaneous connections toohello server node.</span></span>

    <span data-ttu-id="2a306-156">Hello následující obrazovka ukazuje výstup hello z tohoto příkladu:</span><span class="sxs-lookup"><span data-stu-id="2a306-156">hello following screen shows hello output from this example:</span></span>

    ![Výstup](./media/vpn-gateway-validate-throughput-to-vnet/06theoutput.png)

5. <span data-ttu-id="2a306-158">(Volitelné) toopreserve hello testování výsledků, spusťte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="2a306-158">(OPTIONAL) toopreserve hello testing results, run this command:</span></span>

    ```CMD
    iperf3.exe -c IPofTheServerToReach -t 30 -p 5001 -P 32  >> output.txt
    ```

6. <span data-ttu-id="2a306-159">Po dokončení předchozích kroků hello spusťte hello stejný postup s rolemi hello vrátit zpět, tak, aby hello uzel serveru teď bude hello klienta a naopak.</span><span class="sxs-lookup"><span data-stu-id="2a306-159">After completing hello previous steps, execute hello same steps with hello roles reversed, so that hello server node will now be hello client and vice-versa.</span></span>

## <a name="address-slow-file-copy-issues"></a><span data-ttu-id="2a306-160">Řeší problémy kopie pomalé souboru</span><span class="sxs-lookup"><span data-stu-id="2a306-160">Address slow file copy issues</span></span>
<span data-ttu-id="2a306-161">Může dojít k pomalé souboru kopírování při pomocí Průzkumníka Windows nebo přetahování a vkládání přes relaci protokolu RDP.</span><span class="sxs-lookup"><span data-stu-id="2a306-161">You may experience slow file coping when using Windows Explorer or dragging and dropping through an RDP session.</span></span> <span data-ttu-id="2a306-162">Tento problém je obvykle kvůli tooone nebo obou hello následující faktory:</span><span class="sxs-lookup"><span data-stu-id="2a306-162">This problem is normally due tooone or both of hello following factors:</span></span>

- <span data-ttu-id="2a306-163">Při kopírování souborů aplikace kopírování souboru, jako je například Průzkumník Windows a protokolu RDP, nepoužívejte více vláken.</span><span class="sxs-lookup"><span data-stu-id="2a306-163">File copy applications, such as Windows Explorer and RDP, do not use multiple threads when copying files.</span></span> <span data-ttu-id="2a306-164">Pro lepší výkon použijte aplikaci kopie Vícevláknová souborového [Richcopy](https://technet.microsoft.com/en-us/magazine/2009.04.utilityspotlight.aspx) toocopy soubory pomocí 16 nebo 32 vláken.</span><span class="sxs-lookup"><span data-stu-id="2a306-164">For better performance, use a multi-threaded file copy application such as [Richcopy](https://technet.microsoft.com/en-us/magazine/2009.04.utilityspotlight.aspx) toocopy files by using 16 or 32 threads.</span></span> <span data-ttu-id="2a306-165">Klikněte na tlačítko toochange hello vlákno číslo pro kopírování souborů v Richcopy, **akce** > **možnosti kopírování** > **kopírování souboru**.</span><span class="sxs-lookup"><span data-stu-id="2a306-165">toochange hello thread number for file copy in Richcopy, click **Action** > **Copy options** > **File copy**.</span></span><br><br><span data-ttu-id="2a306-166">
![Pomalé souboru kopie problémy](./media/vpn-gateway-validate-throughput-to-vnet/Richcopy.png)</span><span class="sxs-lookup"><span data-stu-id="2a306-166">
![Slow file copy issues](./media/vpn-gateway-validate-throughput-to-vnet/Richcopy.png)</span></span><br>
- <span data-ttu-id="2a306-167">Nedostatečná rychlost čtení/zápis disku virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="2a306-167">Insufficient VM disk read/write speed.</span></span> <span data-ttu-id="2a306-168">Další informace najdete v tématu [řešení potíží s Azure Storage](../storage/common/storage-e2e-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="2a306-168">For more information, see [Azure Storage Troubleshooting](../storage/common/storage-e2e-troubleshooting.md).</span></span>

## <a name="on-premises-device-external-facing-interface"></a><span data-ttu-id="2a306-169">Místní zařízení externí přístupných rozhraní</span><span class="sxs-lookup"><span data-stu-id="2a306-169">On-premises device external facing interface</span></span>
<span data-ttu-id="2a306-170">Pokud hello místní zařízení VPN internetového IP adresa je součástí hello [místní sítě](vpn-gateway-howto-site-to-site-resource-manager-portal.md#LocalNetworkGateway) definice v Azure, se můžete setkat nemohou toobring hello VPN ojediněle odpojí nebo problémy s výkonem.</span><span class="sxs-lookup"><span data-stu-id="2a306-170">If hello on-premises VPN device Internet-facing IP address is included in hello [local network](vpn-gateway-howto-site-to-site-resource-manager-portal.md#LocalNetworkGateway) definition in Azure, you may experience inability toobring up hello VPN, sporadic disconnects, or performance issues.</span></span>

## <a name="checking-latency"></a><span data-ttu-id="2a306-171">Kontrola latence</span><span class="sxs-lookup"><span data-stu-id="2a306-171">Checking latency</span></span>
<span data-ttu-id="2a306-172">Tracert tootrace tooMicrosoft Azure hraniční zařízení toodetermine použijte, pokud nejsou žádné zpoždění vyšší než 100 ms mezi segmenty směrování.</span><span class="sxs-lookup"><span data-stu-id="2a306-172">Use tracert tootrace tooMicrosoft Azure Edge device toodetermine if there are any delays exceeding 100 ms between hops.</span></span>

<span data-ttu-id="2a306-173">Z hello do místní sítě, spusťte *tracert* toohello VIP hello brány Azure nebo virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="2a306-173">From hello on-premises network, run *tracert* toohello VIP of hello Azure Gateway or VM.</span></span> <span data-ttu-id="2a306-174">Jakmile se zobrazí pouze * vrátí, víte, bylo dosaženo hello Azure okraj.</span><span class="sxs-lookup"><span data-stu-id="2a306-174">Once you see only * returned, you know you have reached hello Azure edge.</span></span> <span data-ttu-id="2a306-175">Když se názvy DNS, které zahrnují "MSN" vrátil, víte, že jste dosáhli páteřní Microsoft hello.</span><span class="sxs-lookup"><span data-stu-id="2a306-175">When you see DNS names that include "MSN" returned, you know you have reached hello Microsoft backbone.</span></span><br><br><span data-ttu-id="2a306-176">
![Kontrola latence](./media/vpn-gateway-validate-throughput-to-vnet/08checkinglatency.png)</span><span class="sxs-lookup"><span data-stu-id="2a306-176">
![Checking Latency](./media/vpn-gateway-validate-throughput-to-vnet/08checkinglatency.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="2a306-177">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2a306-177">Next steps</span></span>
<span data-ttu-id="2a306-178">Další informace a nápovědu najdete na naší hello následující odkazy:</span><span class="sxs-lookup"><span data-stu-id="2a306-178">For more information or help, check out hello following links:</span></span>

- [<span data-ttu-id="2a306-179">Optimalizovat propustnost sítě pro virtuální počítače Azure</span><span class="sxs-lookup"><span data-stu-id="2a306-179">Optimize network throughput for Azure virtual machines</span></span>](../virtual-network/virtual-network-optimize-network-bandwidth.md)
- [<span data-ttu-id="2a306-180">Podporu společnosti Microsoft</span><span class="sxs-lookup"><span data-stu-id="2a306-180">Microsoft Support</span></span>](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)
