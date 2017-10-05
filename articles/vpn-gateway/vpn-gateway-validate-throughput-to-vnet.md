---
title: "Ověření propustnost sítě VPN do virtuální sítě Microsoft Azure | Microsoft Docs"
description: "Účelem tohoto dokumentu je pomoct uživatele ověřit propustnost sítě ze svých místních prostředků pro virtuální počítač Azure."
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
ms.openlocfilehash: 2e0347854b5d30c955a50a01d6f7ba08e24f94b6
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-validate-vpn-throughput-to-a-virtual-network"></a><span data-ttu-id="34e27-103">Postup ověření propustnost sítě VPN do virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="34e27-103">How to validate VPN throughput to a virtual network</span></span>

<span data-ttu-id="34e27-104">Umožňuje vytvořit bezpečné, mezi různými místy připojení mezi virtuální sítě v rámci Azure a místní připojení k bráně VPN infrastruktury IT.</span><span class="sxs-lookup"><span data-stu-id="34e27-104">A VPN gateway connection enables you to establish secure, cross-premises connectivity between your Virtual Network within Azure and your on-premises IT infrastructure.</span></span>

<span data-ttu-id="34e27-105">Tento článek ukazuje, jak ověřit propustnost sítě z místních prostředků pro virtuální počítač Azure.</span><span class="sxs-lookup"><span data-stu-id="34e27-105">This article shows how to validate network throughput from the on-premises resources to an Azure virtual machine.</span></span> <span data-ttu-id="34e27-106">Obsahuje také pokyny k odstraňování problémů.</span><span class="sxs-lookup"><span data-stu-id="34e27-106">It also provides troubleshooting guidance.</span></span>

>[!NOTE]
><span data-ttu-id="34e27-107">Tento článek je určený k diagnostice a řešení běžných problémů.</span><span class="sxs-lookup"><span data-stu-id="34e27-107">This article is intended to help diagnose and fix common issues.</span></span> <span data-ttu-id="34e27-108">Pokud se nemůžete tento problém vyřešit pomocí následujících informací [obraťte se na podporu](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span><span class="sxs-lookup"><span data-stu-id="34e27-108">If you're unable to solve the issue by using the following information, [contact support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span></span>
>
>

## <a name="overview"></a><span data-ttu-id="34e27-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="34e27-109">Overview</span></span>

<span data-ttu-id="34e27-110">Připojení brány sítě VPN zahrnuje následující součásti:</span><span class="sxs-lookup"><span data-stu-id="34e27-110">The VPN gateway connection involves the following components:</span></span>

- <span data-ttu-id="34e27-111">Místní zařízení VPN (zobrazení seznamu [ověřit zařízení VPN)](vpn-gateway-about-vpn-devices.md#devicetable).</span><span class="sxs-lookup"><span data-stu-id="34e27-111">On-premises VPN device (view a list of [validated VPN devices)](vpn-gateway-about-vpn-devices.md#devicetable).</span></span>
- <span data-ttu-id="34e27-112">Veřejný Internet</span><span class="sxs-lookup"><span data-stu-id="34e27-112">Public Internet</span></span>
- <span data-ttu-id="34e27-113">Služba Azure VPN gateway</span><span class="sxs-lookup"><span data-stu-id="34e27-113">Azure VPN gateway</span></span>
- <span data-ttu-id="34e27-114">Virtuální počítače Azure</span><span class="sxs-lookup"><span data-stu-id="34e27-114">Azure virtual machine</span></span>

<span data-ttu-id="34e27-115">Následující diagram znázorňuje logické připojení místní síti pro virtuální síť Azure prostřednictvím sítě VPN.</span><span class="sxs-lookup"><span data-stu-id="34e27-115">The following diagram shows the logical connectivity of an on-premises network to an Azure virtual network through VPN.</span></span>

![Logické připojení pro zákazníka síťová MSFT síti pomocí sítě VPN](./media/vpn-gateway-validate-throughput-to-vnet/VPNPerf.png)

## <a name="calculate-the-maximum-expected-ingressegress"></a><span data-ttu-id="34e27-117">Vypočítat maximální očekávaný vstup/výstup</span><span class="sxs-lookup"><span data-stu-id="34e27-117">Calculate the maximum expected ingress/egress</span></span>

1.  <span data-ttu-id="34e27-118">Určete požadavky na propustnost směrného plánu vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="34e27-118">Determine your application's baseline throughput requirements.</span></span>
2.  <span data-ttu-id="34e27-119">Určení vaší omezení propustnosti brány Azure VPN.</span><span class="sxs-lookup"><span data-stu-id="34e27-119">Determine your Azure VPN gateway throughput limits.</span></span> <span data-ttu-id="34e27-120">Nápovědu najdete v části "Agregovaná propustnost podle typů SKU a síť VPN" [plánování a návrhu pro bránu VPN](vpn-gateway-plan-design.md).</span><span class="sxs-lookup"><span data-stu-id="34e27-120">For help, see the "Aggregate throughput by SKU and VPN type" section of [Planning and design for VPN Gateway](vpn-gateway-plan-design.md).</span></span>
3.  <span data-ttu-id="34e27-121">Určení [virtuálního počítače Azure propustnost pokyny](../virtual-machines/virtual-machines-windows-sizes.md) pro vaše velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="34e27-121">Determine the [Azure VM throughput guidance](../virtual-machines/virtual-machines-windows-sizes.md) for your VM size.</span></span>
4.  <span data-ttu-id="34e27-122">Určuje šířku pásma vašeho poskytovatele služeb sítě Internet (ISP).</span><span class="sxs-lookup"><span data-stu-id="34e27-122">Determine your Internet Service Provider (ISP) bandwidth.</span></span>
5.  <span data-ttu-id="34e27-123">Vypočítat očekávané propustnost - minimálně šířka pásma (virtuální počítač, brány, poskytovatele internetových služeb) * 0,8.</span><span class="sxs-lookup"><span data-stu-id="34e27-123">Calculate your expected throughput - Least bandwidth of (VM, Gateway, ISP) * 0.8.</span></span>

<span data-ttu-id="34e27-124">Pokud vaše počítané propustnost nesplňuje požadavky na propustnost směrného plánu vaší aplikace, je potřeba zvýšit šířku pásma k prostředku, který jste určili jako kritická místa.</span><span class="sxs-lookup"><span data-stu-id="34e27-124">If your calculated throughput does not meet your application's baseline throughput requirements, you need to increase the bandwidth of the resource that you identified as the bottleneck.</span></span> <span data-ttu-id="34e27-125">Ke změně velikosti služby Azure VPN Gateway, najdete v části [změna skladová položka brány](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#gwsku).</span><span class="sxs-lookup"><span data-stu-id="34e27-125">To resize an Azure VPN Gateway, see [Changing a gateway SKU](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#gwsku).</span></span> <span data-ttu-id="34e27-126">Chcete-li změnit velikost virtuálního počítače, přečtěte si téma [změnit velikost virtuálního počítače](../virtual-machines/virtual-machines-windows-resize-vm.md).</span><span class="sxs-lookup"><span data-stu-id="34e27-126">To resize a virtual machine, see [Resize a VM](../virtual-machines/virtual-machines-windows-resize-vm.md).</span></span> <span data-ttu-id="34e27-127">Pokud k těmto problémům očekávané šířky pásma Internetu, můžete také na bezpečném místě.</span><span class="sxs-lookup"><span data-stu-id="34e27-127">If you are not experiencing expected Internet bandwidth, you may also want to contact your ISP.</span></span>

## <a name="validate-network-throughput-by-using-performance-tools"></a><span data-ttu-id="34e27-128">Ověření propustnost sítě pomocí nástroje pro sledování výkonu</span><span class="sxs-lookup"><span data-stu-id="34e27-128">Validate network throughput by using performance tools</span></span>

<span data-ttu-id="34e27-129">Toto ověření je třeba provést v době mimo špičku jako sytost propustnost tunelové propojení VPN během testování není poskytnuta přesné výsledky.</span><span class="sxs-lookup"><span data-stu-id="34e27-129">This validation should be performed during non-peak hours, as VPN tunnel throughput saturation during testing does not give accurate results.</span></span>

<span data-ttu-id="34e27-130">Nástroj, který používáme pro tento test je iPerf, který má klient i server režimy a funguje v systému Windows a Linux.</span><span class="sxs-lookup"><span data-stu-id="34e27-130">The tool we use for this test is iPerf, which works on both Windows and Linux and has both client and server modes.</span></span> <span data-ttu-id="34e27-131">Je omezený na 3 GB/s pro virtuální počítače Windows.</span><span class="sxs-lookup"><span data-stu-id="34e27-131">It is limited to 3 Gbps for Windows VMs.</span></span>

<span data-ttu-id="34e27-132">Tento nástroj neprovádí žádné operace čtení a zápisu na disk.</span><span class="sxs-lookup"><span data-stu-id="34e27-132">This tool does not perform any read/write operations to disk.</span></span> <span data-ttu-id="34e27-133">Vytvoří výhradně generovaný sám sebou TCP provoz z jednoho elementu end na druhý.</span><span class="sxs-lookup"><span data-stu-id="34e27-133">It solely produces self-generated TCP traffic from one end to the other.</span></span> <span data-ttu-id="34e27-134">Vygeneroval statistiky podle experimenty, které měří šířku pásma mezi klientem a serverem uzly k dispozici.</span><span class="sxs-lookup"><span data-stu-id="34e27-134">It generated statistics based on experimentation that measures the bandwidth available between client and server nodes.</span></span> <span data-ttu-id="34e27-135">Při testování mezi dvěma uzly, jeden funguje jako server a další jako klient.</span><span class="sxs-lookup"><span data-stu-id="34e27-135">When testing between two nodes, one acts as the server and the other as a client.</span></span> <span data-ttu-id="34e27-136">Po dokončení se tento test, doporučujeme zpětné rolí k testování obou nahrávání a stahování propustnost v obou uzlech.</span><span class="sxs-lookup"><span data-stu-id="34e27-136">Once this test is completed, we recommend that you reverse the roles to test both upload and download throughput on both nodes.</span></span>

### <a name="download-iperf"></a><span data-ttu-id="34e27-137">Stažení nástroje iPerf</span><span class="sxs-lookup"><span data-stu-id="34e27-137">Download iPerf</span></span>
<span data-ttu-id="34e27-138">Stáhněte si [iPerf](https://iperf.fr/download/iperf_3.1/iperf-3.1.2-win64.zip).</span><span class="sxs-lookup"><span data-stu-id="34e27-138">Download [iPerf](https://iperf.fr/download/iperf_3.1/iperf-3.1.2-win64.zip).</span></span> <span data-ttu-id="34e27-139">Podrobnosti najdete v tématu [iPerf dokumentaci](https://iperf.fr/iperf-doc.php).</span><span class="sxs-lookup"><span data-stu-id="34e27-139">For details, see [iPerf documentation](https://iperf.fr/iperf-doc.php).</span></span>

 >[!NOTE]
 ><span data-ttu-id="34e27-140">Produkty třetích stran, které tento článek popisuje, pocházejí od společností, které jsou nezávislé na Microsoftu.</span><span class="sxs-lookup"><span data-stu-id="34e27-140">The third-party products that this article discusses are manufactured by companies that are independent of Microsoft.</span></span> <span data-ttu-id="34e27-141">Společnost Microsoft neposkytuje žádné záruky, předpokládanou ani jinou souvislosti s výkonem a spolehlivostí těchto produktů.</span><span class="sxs-lookup"><span data-stu-id="34e27-141">Microsoft makes no warranty, implied or otherwise, about the performance or reliability of these products.</span></span>
 >
 >

### <a name="run-iperf-iperf3exe"></a><span data-ttu-id="34e27-142">Spuštění nástroje iPerf (iperf3.exe)</span><span class="sxs-lookup"><span data-stu-id="34e27-142">Run iPerf (iperf3.exe)</span></span>
1. <span data-ttu-id="34e27-143">Povolte pravidlo NSG nebo seznamu ACL umožňuje provoz (pro veřejné IP adresy testování na virtuálním počítači Azure).</span><span class="sxs-lookup"><span data-stu-id="34e27-143">Enable an NSG/ACL rule allowing the traffic (for public IP address testing on Azure VM).</span></span>

2. <span data-ttu-id="34e27-144">V obou uzlech povolte výjimku brány firewall pro port 5001.</span><span class="sxs-lookup"><span data-stu-id="34e27-144">On both nodes, enable a firewall exception for port 5001.</span></span>

    <span data-ttu-id="34e27-145">**Windows:** jako správce spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="34e27-145">**Windows:** Run the following command as an administrator:</span></span>

    ```CMD
    netsh advfirewall firewall add rule name="Open Port 5001" dir=in action=allow protocol=TCP localport=5001
    ```

    <span data-ttu-id="34e27-146">Pro odebrání pravidla při testování skončí, spusťte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="34e27-146">To remove the rule when testing is complete, run this command:</span></span>

    ```CMD
    netsh advfirewall firewall delete rule name="Open Port 5001" protocol=TCP localport=5001
    ```
    </br>
    <span data-ttu-id="34e27-147">**Azure Linux:** obrázků Azure Linux je projektovou brány firewall.</span><span class="sxs-lookup"><span data-stu-id="34e27-147">**Azure Linux:**  Azure Linux images have permissive firewalls.</span></span> <span data-ttu-id="34e27-148">Pokud je aplikace, která naslouchá na portu, provoz může prostřednictvím.</span><span class="sxs-lookup"><span data-stu-id="34e27-148">If there is an application listening on a port, the traffic is allowed through.</span></span> <span data-ttu-id="34e27-149">Vlastní Image, které jsou zabezpečené může být nutné explicitně otevřené porty.</span><span class="sxs-lookup"><span data-stu-id="34e27-149">Custom images that are secured may need ports opened explicitly.</span></span> <span data-ttu-id="34e27-150">Běžné brány firewall vrstvy operačního systému Linux zahrnují `iptables`, `ufw`, nebo `firewalld`.</span><span class="sxs-lookup"><span data-stu-id="34e27-150">Common Linux OS-layer firewalls include `iptables`, `ufw`, or `firewalld`.</span></span>

3. <span data-ttu-id="34e27-151">Na uzlu serveru přejděte do adresáře, které je extrahován iperf3.exe.</span><span class="sxs-lookup"><span data-stu-id="34e27-151">On the server node, change to the directory where iperf3.exe is extracted.</span></span> <span data-ttu-id="34e27-152">Potom spusťte iPerf v režimu serveru a nastavte ji naslouchat na portu 5001 jako následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="34e27-152">Then run iPerf in server mode and set it to listen on port 5001 as the following commands:</span></span>

     ```CMD
     cd c:\iperf-3.1.2-win65

     iperf3.exe -s -p 5001
     ```

4. <span data-ttu-id="34e27-153">Na uzlu klienta přejděte do adresáře, kde je nástroj iperf extrahovat a pak spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="34e27-153">On the client node, change to the directory where iperf tool is extracted and then run the following command:</span></span>

    ```CMD
    iperf3.exe -c <IP of the iperf Server> -t 30 -p 5001 -P 32
    ```

    <span data-ttu-id="34e27-154">Klient způsobuje přenosy na portu 5001 k serveru pro 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="34e27-154">The client is inducing traffic on port 5001 to the server for 30 seconds.</span></span> <span data-ttu-id="34e27-155">Příznak "-P" určující používáme 32 současných připojení k uzlu serveru.</span><span class="sxs-lookup"><span data-stu-id="34e27-155">The flag '-P ' that indicates we are using 32 simultaneous connections to the server node.</span></span>

    <span data-ttu-id="34e27-156">Na následující obrazovce se zobrazí výstup z tohoto příkladu:</span><span class="sxs-lookup"><span data-stu-id="34e27-156">The following screen shows the output from this example:</span></span>

    ![Výstup](./media/vpn-gateway-validate-throughput-to-vnet/06theoutput.png)

5. <span data-ttu-id="34e27-158">(VOLITELNÉ) Pokud chcete zachovat testování výsledky, spusťte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="34e27-158">(OPTIONAL) To preserve the testing results, run this command:</span></span>

    ```CMD
    iperf3.exe -c IPofTheServerToReach -t 30 -p 5001 -P 32  >> output.txt
    ```

6. <span data-ttu-id="34e27-159">Po dokončení předchozích kroků provést stejný postup s rolemi vrátit zpět, tak, aby uzel serveru bude nyní klienta a naopak.</span><span class="sxs-lookup"><span data-stu-id="34e27-159">After completing the previous steps, execute the same steps with the roles reversed, so that the server node will now be the client and vice-versa.</span></span>

## <a name="address-slow-file-copy-issues"></a><span data-ttu-id="34e27-160">Řeší problémy kopie pomalé souboru</span><span class="sxs-lookup"><span data-stu-id="34e27-160">Address slow file copy issues</span></span>
<span data-ttu-id="34e27-161">Může dojít k pomalé souboru kopírování při pomocí Průzkumníka Windows nebo přetahování a vkládání přes relaci protokolu RDP.</span><span class="sxs-lookup"><span data-stu-id="34e27-161">You may experience slow file coping when using Windows Explorer or dragging and dropping through an RDP session.</span></span> <span data-ttu-id="34e27-162">Tento problém je obvykle kvůli jedno nebo obě následující faktory:</span><span class="sxs-lookup"><span data-stu-id="34e27-162">This problem is normally due to one or both of the following factors:</span></span>

- <span data-ttu-id="34e27-163">Při kopírování souborů aplikace kopírování souboru, jako je například Průzkumník Windows a protokolu RDP, nepoužívejte více vláken.</span><span class="sxs-lookup"><span data-stu-id="34e27-163">File copy applications, such as Windows Explorer and RDP, do not use multiple threads when copying files.</span></span> <span data-ttu-id="34e27-164">Pro lepší výkon použijte aplikaci kopie Vícevláknová souborového [Richcopy](https://technet.microsoft.com/en-us/magazine/2009.04.utilityspotlight.aspx) kopírovat soubory pomocí 16 nebo 32 vláken.</span><span class="sxs-lookup"><span data-stu-id="34e27-164">For better performance, use a multi-threaded file copy application such as [Richcopy](https://technet.microsoft.com/en-us/magazine/2009.04.utilityspotlight.aspx) to copy files by using 16 or 32 threads.</span></span> <span data-ttu-id="34e27-165">Chcete-li změnit počet vláken pro kopírování souborů v Richcopy, klikněte na tlačítko **akce** > **možnosti kopírování** > **kopírování souboru**.</span><span class="sxs-lookup"><span data-stu-id="34e27-165">To change the thread number for file copy in Richcopy, click **Action** > **Copy options** > **File copy**.</span></span><br><br><span data-ttu-id="34e27-166">
![Pomalé souboru kopie problémy](./media/vpn-gateway-validate-throughput-to-vnet/Richcopy.png)</span><span class="sxs-lookup"><span data-stu-id="34e27-166">
![Slow file copy issues](./media/vpn-gateway-validate-throughput-to-vnet/Richcopy.png)</span></span><br>
- <span data-ttu-id="34e27-167">Nedostatečná rychlost čtení/zápis disku virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="34e27-167">Insufficient VM disk read/write speed.</span></span> <span data-ttu-id="34e27-168">Další informace najdete v tématu [řešení potíží s Azure Storage](../storage/common/storage-e2e-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="34e27-168">For more information, see [Azure Storage Troubleshooting](../storage/common/storage-e2e-troubleshooting.md).</span></span>

## <a name="on-premises-device-external-facing-interface"></a><span data-ttu-id="34e27-169">Místní zařízení externí přístupných rozhraní</span><span class="sxs-lookup"><span data-stu-id="34e27-169">On-premises device external facing interface</span></span>
<span data-ttu-id="34e27-170">Pokud je součástí místní zařízení VPN internetového IP adresu [místní sítě](vpn-gateway-howto-site-to-site-resource-manager-portal.md#LocalNetworkGateway) definice v Azure, můžete zaznamenat neschopnost otevře místní okno, odpojení sítě VPN, ojediněle, problémy s výkonem.</span><span class="sxs-lookup"><span data-stu-id="34e27-170">If the on-premises VPN device Internet-facing IP address is included in the [local network](vpn-gateway-howto-site-to-site-resource-manager-portal.md#LocalNetworkGateway) definition in Azure, you may experience inability to bring up the VPN, sporadic disconnects, or performance issues.</span></span>

## <a name="checking-latency"></a><span data-ttu-id="34e27-171">Kontrola latence</span><span class="sxs-lookup"><span data-stu-id="34e27-171">Checking latency</span></span>
<span data-ttu-id="34e27-172">Tracert pro trasování k Microsoft Azure hraniční zařízení použijte k určení, zda neexistují žádné zpoždění vyšší než 100 ms mezi segmenty směrování.</span><span class="sxs-lookup"><span data-stu-id="34e27-172">Use tracert to trace to Microsoft Azure Edge device to determine if there are any delays exceeding 100 ms between hops.</span></span>

<span data-ttu-id="34e27-173">Z místní sítě, spusťte *tracert* do virtuální IP adresu brány Azure nebo virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="34e27-173">From the on-premises network, run *tracert* to the VIP of the Azure Gateway or VM.</span></span> <span data-ttu-id="34e27-174">Jakmile se zobrazí pouze * vrátí, víte, bylo dosaženo Azure okraj.</span><span class="sxs-lookup"><span data-stu-id="34e27-174">Once you see only * returned, you know you have reached the Azure edge.</span></span> <span data-ttu-id="34e27-175">Když se názvy DNS, které zahrnují "MSN" vrátil, víte, že jste dosáhli páteřní společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="34e27-175">When you see DNS names that include "MSN" returned, you know you have reached the Microsoft backbone.</span></span><br><br><span data-ttu-id="34e27-176">
![Kontrola latence](./media/vpn-gateway-validate-throughput-to-vnet/08checkinglatency.png)</span><span class="sxs-lookup"><span data-stu-id="34e27-176">
![Checking Latency](./media/vpn-gateway-validate-throughput-to-vnet/08checkinglatency.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="34e27-177">Další kroky</span><span class="sxs-lookup"><span data-stu-id="34e27-177">Next steps</span></span>
<span data-ttu-id="34e27-178">Další informace a nápovědu najdete na následujících odkazech:</span><span class="sxs-lookup"><span data-stu-id="34e27-178">For more information or help, check out the following links:</span></span>

- [<span data-ttu-id="34e27-179">Optimalizovat propustnost sítě pro virtuální počítače Azure</span><span class="sxs-lookup"><span data-stu-id="34e27-179">Optimize network throughput for Azure virtual machines</span></span>](../virtual-network/virtual-network-optimize-network-bandwidth.md)
- [<span data-ttu-id="34e27-180">Podporu společnosti Microsoft</span><span class="sxs-lookup"><span data-stu-id="34e27-180">Microsoft Support</span></span>](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)
