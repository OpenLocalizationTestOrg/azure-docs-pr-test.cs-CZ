---
title: "Testování propustnost sítě virtuálního počítače Azure | Microsoft Docs"
description: "Zjistěte, jak otestovat propustnost sítě virtuálního počítače Azure."
services: virtual-network
documentationcenter: na
author: steveesp
manager: Gerald DeGrace
editor: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/21/2017
ms.author: steveesp
ms.openlocfilehash: ccebc722386a19014674d7a59757a3685bd50793
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="bandwidththroughput-testing-ntttcp"></a><span data-ttu-id="146d8-103">Šířky pásma nebo propustnost testování (NTTTCP)</span><span class="sxs-lookup"><span data-stu-id="146d8-103">Bandwidth/Throughput testing (NTTTCP)</span></span>

<span data-ttu-id="146d8-104">Při testování propustnost sítě v Azure, je nejvhodnější použít nástroj, který cílí sítě pro testování a minimalizuje použití další prostředky, které by mohlo mít vliv na výkon.</span><span class="sxs-lookup"><span data-stu-id="146d8-104">When testing network throughput performance in Azure, it's best to use a tool that targets the network for testing and minimizes the use of other resources that could impact performance.</span></span> <span data-ttu-id="146d8-105">Doporučuje se NTTTCP.</span><span class="sxs-lookup"><span data-stu-id="146d8-105">NTTTCP is recommended.</span></span>

<span data-ttu-id="146d8-106">Zkopírujte nástroj na dva virtuální počítače Azure stejnou velikost.</span><span class="sxs-lookup"><span data-stu-id="146d8-106">Copy the tool to two Azure VMs of the same size.</span></span> <span data-ttu-id="146d8-107">Jeden virtuální počítač funguje jako odesílatele a druhý jako příjemce.</span><span class="sxs-lookup"><span data-stu-id="146d8-107">One VM functions as SENDER and the other as RECEIVER.</span></span>

#### <a name="deploying-vms-for-testing"></a><span data-ttu-id="146d8-108">Nasazení virtuálních počítačů pro testování</span><span class="sxs-lookup"><span data-stu-id="146d8-108">Deploying VMs for testing</span></span>
<span data-ttu-id="146d8-109">Pro účely tohoto testu musí být dva virtuální počítače ve stejné cloudové služby nebo do stejné skupiny dostupnosti, aby jsme používat svoje interní IP adresy a vyloučit z testu nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="146d8-109">For the purposes of this test, the two VMs should be in either the same Cloud Service or the same Availability Set so that we can use their internal IPs and exclude the Load Balancers from the test.</span></span> <span data-ttu-id="146d8-110">Je možné otestovat s virtuální IP adresa, ale tento druh testování je mimo rámec tohoto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="146d8-110">It is possible to test with the VIP but this kind of testing is outside the scope of this document.</span></span>
 
<span data-ttu-id="146d8-111">Poznamenejte si IP adresa příjemce.</span><span class="sxs-lookup"><span data-stu-id="146d8-111">Make a note of the RECEIVER's IP address.</span></span> <span data-ttu-id="146d8-112">Umožňuje volání této IP "a.b.c.r"</span><span class="sxs-lookup"><span data-stu-id="146d8-112">Let's call that IP "a.b.c.r"</span></span>

<span data-ttu-id="146d8-113">Poznamenejte si počet jader na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="146d8-113">Make a note of the number of cores on the VM.</span></span> <span data-ttu-id="146d8-114">To umožňuje volání "\#num\_jader"</span><span class="sxs-lookup"><span data-stu-id="146d8-114">Let's call this "\#num\_cores"</span></span>
 
<span data-ttu-id="146d8-115">Spusťte NTTTCP test pro 300 sekund (nebo 5 minut) na virtuální počítač odesílatele a příjemce virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="146d8-115">Run the NTTTCP test for 300 seconds (or 5 minutes) on the sender VM and receiver VM.</span></span>

<span data-ttu-id="146d8-116">Tip: Při nastavování tento test poprvé, můžete se pokusit kratší dobu testovací k získání zpětné vazby dříve.</span><span class="sxs-lookup"><span data-stu-id="146d8-116">Tip: When setting up this test for the first time, you might try a shorter test period to get feedback sooner.</span></span> <span data-ttu-id="146d8-117">Jakmile nástroj funguje podle očekávání, rozšiřte na 300 sekund, aby nejpřesnější výsledky zkušebního období.</span><span class="sxs-lookup"><span data-stu-id="146d8-117">Once the tool is working as expected, extend the test period to 300 seconds for the most accurate results.</span></span>

> [!NOTE]
> <span data-ttu-id="146d8-118">Odesílatel **a** musíte zadat příjemce **stejné** testovací parametr doba trvání (-t).</span><span class="sxs-lookup"><span data-stu-id="146d8-118">The sender **and** receiver must specify **the same** test duration parameter (-t).</span></span>

<span data-ttu-id="146d8-119">Při testování jednoho datového proudu TCP 10 sekund:</span><span class="sxs-lookup"><span data-stu-id="146d8-119">To test a single TCP stream for 10 seconds:</span></span>

<span data-ttu-id="146d8-120">Příjemce parametry: ntttcp - r -t 10 - P 1</span><span class="sxs-lookup"><span data-stu-id="146d8-120">Receiver parameters: ntttcp -r -t 10 -P 1</span></span>

<span data-ttu-id="146d8-121">Odesílatel parametry: ntttcp-s10.27.33.7 -t 10 - n 1 -P 1</span><span class="sxs-lookup"><span data-stu-id="146d8-121">Sender parameters: ntttcp -s10.27.33.7 -t 10 -n 1 -P 1</span></span>

> [!NOTE]
> <span data-ttu-id="146d8-122">V předchozím příkladu lze používat pouze k potvrzení vaší konfigurace.</span><span class="sxs-lookup"><span data-stu-id="146d8-122">The preceding sample should only be used to confirm your configuration.</span></span> <span data-ttu-id="146d8-123">Testování platný příklady jsou popsané dál v tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="146d8-123">Valid examples of testing are covered later in this document.</span></span>

## <a name="testing-vms-running-windows"></a><span data-ttu-id="146d8-124">Testování virtuální počítače se systémem WINDOWS:</span><span class="sxs-lookup"><span data-stu-id="146d8-124">Testing VMs running WINDOWS:</span></span>

#### <a name="get-ntttcp-onto-the-vms"></a><span data-ttu-id="146d8-125">Získáte NTTTCP do virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="146d8-125">Get NTTTCP onto the VMs.</span></span>

<span data-ttu-id="146d8-126">Stažení nejnovější verze: <https://gallery.technet.microsoft.com/NTttcp-Version-528-Now-f8b12769></span><span class="sxs-lookup"><span data-stu-id="146d8-126">Download the latest version: <https://gallery.technet.microsoft.com/NTttcp-Version-528-Now-f8b12769></span></span>

<span data-ttu-id="146d8-127">Nebo je-li přesunout hledání: <https://www.bing.com/search?q=ntttcp+download> \< – musí být nejprve dosáhl</span><span class="sxs-lookup"><span data-stu-id="146d8-127">Or search for it if moved: <https://www.bing.com/search?q=ntttcp+download>\< -- should be first hit</span></span>

<span data-ttu-id="146d8-128">Zvažte umístění NTTTCP samostatné složky, jako je c:\\nástroje</span><span class="sxs-lookup"><span data-stu-id="146d8-128">Consider putting NTTTCP in separate folder, like c:\\tools</span></span>

#### <a name="allow-ntttcp-through-the-windows-firewall"></a><span data-ttu-id="146d8-129">Povolit NTTTCP přes bránu Windows firewall</span><span class="sxs-lookup"><span data-stu-id="146d8-129">Allow NTTTCP through the Windows firewall</span></span>
<span data-ttu-id="146d8-130">Na příjemce vytvořte pravidlo povolení v bráně Windows Firewall umožňuje provoz NTTTCP příchod.</span><span class="sxs-lookup"><span data-stu-id="146d8-130">On the RECEIVER, create an Allow rule on the Windows Firewall to allow the NTTTCP traffic to arrive.</span></span> <span data-ttu-id="146d8-131">Je to nejjednodušší provádět umožnit programu celý NTTTCP podle názvu místo povolit příchozí určité porty TCP.</span><span class="sxs-lookup"><span data-stu-id="146d8-131">It's easiest to allow the entire NTTTCP program by name rather than to allow specific TCP ports inbound.</span></span>

<span data-ttu-id="146d8-132">Povolit ntttcp přes bránu Windows Firewall takto:</span><span class="sxs-lookup"><span data-stu-id="146d8-132">Allow ntttcp through the Windows Firewall like this:</span></span>

<span data-ttu-id="146d8-133">netsh advfirewall firewall přidejte pravidlo program =\<cesta\>\\ntttcp.exe název = "ntttcp" protokol = všechny dir = v akci = povolit povolit = yes profilu = ANY</span><span class="sxs-lookup"><span data-stu-id="146d8-133">netsh advfirewall firewall add rule program=\<PATH\>\\ntttcp.exe name="ntttcp" protocol=any dir=in action=allow enable=yes profile=ANY</span></span>

<span data-ttu-id="146d8-134">Například, pokud jste zkopírovali ntttcp.exe k "c:\\nástroje" složky, bude příkaz:</span><span class="sxs-lookup"><span data-stu-id="146d8-134">For example, if you copied ntttcp.exe to the "c:\\tools" folder, this would be the command:</span></span> 

<span data-ttu-id="146d8-135">netsh advfirewall firewall přidejte pravidlo program = c:\\nástroje\\ntttcp.exe název = "ntttcp" protokol = všechny dir = v akci = povolit povolit = yes profilu = ANY</span><span class="sxs-lookup"><span data-stu-id="146d8-135">netsh advfirewall firewall add rule program=c:\\tools\\ntttcp.exe name="ntttcp" protocol=any dir=in action=allow enable=yes profile=ANY</span></span>

#### <a name="running-ntttcp-tests"></a><span data-ttu-id="146d8-136">Spouštění testů NTTTCP</span><span class="sxs-lookup"><span data-stu-id="146d8-136">Running NTTTCP tests</span></span>

<span data-ttu-id="146d8-137">Spusťte NTTTCP na příjemce (**spustit z CMD**, nikoli z prostředí PowerShell):</span><span class="sxs-lookup"><span data-stu-id="146d8-137">Start NTTTCP on the RECEIVER (**run from CMD**, not from PowerShell):</span></span>

<span data-ttu-id="146d8-138">ntttcp - r – m [2\*\#num\_jader],\*, a.b.c.r -t 300</span><span class="sxs-lookup"><span data-stu-id="146d8-138">ntttcp -r –m [2\*\#num\_cores],\*,a.b.c.r -t 300</span></span>

<span data-ttu-id="146d8-139">Pokud virtuální počítač má čtyři jader a IP adresu 10.0.0.4, by vypadat například takto:</span><span class="sxs-lookup"><span data-stu-id="146d8-139">If the VM has four cores and an IP address of 10.0.0.4, it would look like this:</span></span>

<span data-ttu-id="146d8-140">ntttcp - r – m 8,\*, 10.0.0.4 -t 300</span><span class="sxs-lookup"><span data-stu-id="146d8-140">ntttcp -r –m 8,\*,10.0.0.4 -t 300</span></span>


<span data-ttu-id="146d8-141">Spusťte NTTTCP na odesílatele (**spustit z CMD**, nikoli z prostředí PowerShell):</span><span class="sxs-lookup"><span data-stu-id="146d8-141">Start NTTTCP on the SENDER (**run from CMD**, not from PowerShell):</span></span>

<span data-ttu-id="146d8-142">ntttcp -s – m 8,\*, 10.0.0.4 -t 300</span><span class="sxs-lookup"><span data-stu-id="146d8-142">ntttcp -s –m 8,\*,10.0.0.4 -t 300</span></span> 

<span data-ttu-id="146d8-143">Počkejte, než pro dané výsledky.</span><span class="sxs-lookup"><span data-stu-id="146d8-143">Wait for the results.</span></span>


## <a name="testing-vms-running-linux"></a><span data-ttu-id="146d8-144">Testování virtuální počítače se systémem LINUX:</span><span class="sxs-lookup"><span data-stu-id="146d8-144">Testing VMs running LINUX:</span></span>

<span data-ttu-id="146d8-145">Použijte nttcp pro linux.</span><span class="sxs-lookup"><span data-stu-id="146d8-145">Use nttcp-for-linux.</span></span> <span data-ttu-id="146d8-146">Je k dispozici z <https://github.com/Microsoft/ntttcp-for-linux></span><span class="sxs-lookup"><span data-stu-id="146d8-146">It is available from <https://github.com/Microsoft/ntttcp-for-linux></span></span>

<span data-ttu-id="146d8-147">Na Linux virtuálních počítačů (odesílatele i příjemce) spusťte tyto příkazy a příprava ntttcp pro linux na virtuální počítače:</span><span class="sxs-lookup"><span data-stu-id="146d8-147">On the Linux VMs (both SENDER and RECEIVER), run these commands to prepare ntttcp-for-linux on your VMs:</span></span>

<span data-ttu-id="146d8-148">CentOS – instalace Git:</span><span class="sxs-lookup"><span data-stu-id="146d8-148">CentOS - Install Git:</span></span>
``` bash
  yum install gcc -y  
  yum install git -y
```
<span data-ttu-id="146d8-149">Ubuntu – instalace Git:</span><span class="sxs-lookup"><span data-stu-id="146d8-149">Ubuntu - Install Git:</span></span>
``` bash
 apt-get -y install build-essential  
 apt-get -y install git
```
<span data-ttu-id="146d8-150">Ujistěte se a nainstalujte na obou:</span><span class="sxs-lookup"><span data-stu-id="146d8-150">Make and Install on both:</span></span>
``` bash
 git clone <https://github.com/Microsoft/ntttcp-for-linux>
 cd ntttcp-for-linux/src
 make && make install
```

<span data-ttu-id="146d8-151">Jako v příkladu Windows předpokládáme, že příjemce Linux IP je 10.0.0.4</span><span class="sxs-lookup"><span data-stu-id="146d8-151">As in the Windows example, we assume the Linux RECEIVER's IP is 10.0.0.4</span></span>

<span data-ttu-id="146d8-152">Spusťte na příjemce NTTTCP pro Linux:</span><span class="sxs-lookup"><span data-stu-id="146d8-152">Start NTTTCP-for-Linux on the RECEIVER:</span></span>

``` bash
ntttcp -r -t 300
```

<span data-ttu-id="146d8-153">A na odesílatele, spusťte:</span><span class="sxs-lookup"><span data-stu-id="146d8-153">And on the SENDER, run:</span></span>

``` bash
ntttcp -s10.0.0.4 -t 300
```
 
<span data-ttu-id="146d8-154">Test výchozí hodnota je délka 60 sekund, pokud žádný čas parametr je zadána.</span><span class="sxs-lookup"><span data-stu-id="146d8-154">Test length defaults to 60 seconds if no time parameter is given</span></span>

## <a name="testing-between-vms-running-windows-and-linux"></a><span data-ttu-id="146d8-155">Testování mezi virtuální počítače se systémem Windows a LINUX:</span><span class="sxs-lookup"><span data-stu-id="146d8-155">Testing between VMs running Windows and LINUX:</span></span>

<span data-ttu-id="146d8-156">Na této scénáře jsme by měl povolit režim no-sync, můžete spustit test.</span><span class="sxs-lookup"><span data-stu-id="146d8-156">On this scenarios we should enable the no-sync mode so the test can run.</span></span> <span data-ttu-id="146d8-157">To se provádí pomocí **-N příznak** pro systémy Linux a **příznak -ns** pro systém Windows.</span><span class="sxs-lookup"><span data-stu-id="146d8-157">This is done by using the **-N flag** for Linux, and **-ns flag** for Windows.</span></span>

#### <a name="from-linux-to-windows"></a><span data-ttu-id="146d8-158">Z Linux do Windows:</span><span class="sxs-lookup"><span data-stu-id="146d8-158">From Linux to Windows:</span></span>

<span data-ttu-id="146d8-159">Příjemce <Windows>:</span><span class="sxs-lookup"><span data-stu-id="146d8-159">Receiver <Windows>:</span></span>

``` bash
ntttcp -r -m <2 x nr cores>,*,<Windows server IP>
```

<span data-ttu-id="146d8-160">Odesílatel <Linux> :</span><span class="sxs-lookup"><span data-stu-id="146d8-160">Sender <Linux> :</span></span>

``` bash
ntttcp -s -m <2 x nr cores>,*,<Windows server IP> -N -t 300
```

#### <a name="from-windows-to-linux"></a><span data-ttu-id="146d8-161">Ze systému Windows do systému Linux:</span><span class="sxs-lookup"><span data-stu-id="146d8-161">From Windows to Linux:</span></span>

<span data-ttu-id="146d8-162">Příjemce <Linux>:</span><span class="sxs-lookup"><span data-stu-id="146d8-162">Receiver <Linux>:</span></span>

``` bash
ntttcp -r -m <2 x nr cores>,*,<Linux server IP>
```

<span data-ttu-id="146d8-163">Odesílatel <Windows>:</span><span class="sxs-lookup"><span data-stu-id="146d8-163">Sender <Windows>:</span></span>

``` bash
ntttcp -s -m <2 x nr cores>,*,<Linux  server IP> -ns -t 300
```

## <a name="next-steps"></a><span data-ttu-id="146d8-164">Další kroky</span><span class="sxs-lookup"><span data-stu-id="146d8-164">Next steps</span></span>
* <span data-ttu-id="146d8-165">V závislosti na výsledky, může být místo pro [optimalizovat počítače propustnost sítě](virtual-network-optimize-network-bandwidth.md) pro váš scénář.</span><span class="sxs-lookup"><span data-stu-id="146d8-165">Depending on results, there may be room to [Optimize network throughput machines](virtual-network-optimize-network-bandwidth.md) for your scenario.</span></span>
* <span data-ttu-id="146d8-166">Další informace s [Azure Virtual Network nejčastější dotazy (FAQ)](virtual-networks-faq.md)</span><span class="sxs-lookup"><span data-stu-id="146d8-166">Learn more wtih [Azure Virtual Network frequently asked questions (FAQ)](virtual-networks-faq.md)</span></span>
