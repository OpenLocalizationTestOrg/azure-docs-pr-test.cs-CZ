---
title: "propustnost sítě virtuálního počítače Azure aaaTesting | Microsoft Docs"
description: "Zjistěte, jak tootest virtuální počítač Azure sítě propustnost."
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
ms.openlocfilehash: 2da85c27bc8d16a443b215891f4cd0460f41926f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="bandwidththroughput-testing-ntttcp"></a><span data-ttu-id="56c45-103">Šířky pásma nebo propustnost testování (NTTTCP)</span><span class="sxs-lookup"><span data-stu-id="56c45-103">Bandwidth/Throughput testing (NTTTCP)</span></span>

<span data-ttu-id="56c45-104">Při testování propustnost sítě v Azure, je nejlepší toouse nástroj, který cílí hello sítě pro testování a minimalizuje hello použití další prostředky, které by mohlo mít vliv na výkon.</span><span class="sxs-lookup"><span data-stu-id="56c45-104">When testing network throughput performance in Azure, it's best toouse a tool that targets hello network for testing and minimizes hello use of other resources that could impact performance.</span></span> <span data-ttu-id="56c45-105">Doporučuje se NTTTCP.</span><span class="sxs-lookup"><span data-stu-id="56c45-105">NTTTCP is recommended.</span></span>

<span data-ttu-id="56c45-106">Zkopírujte nástroj tootwo hello virtuální počítače Azure hello stejnou velikost.</span><span class="sxs-lookup"><span data-stu-id="56c45-106">Copy hello tool tootwo Azure VMs of hello same size.</span></span> <span data-ttu-id="56c45-107">Jeden virtuální počítač funguje jako odesílatele a hello jiných jako příjemce.</span><span class="sxs-lookup"><span data-stu-id="56c45-107">One VM functions as SENDER and hello other as RECEIVER.</span></span>

#### <a name="deploying-vms-for-testing"></a><span data-ttu-id="56c45-108">Nasazení virtuálních počítačů pro testování</span><span class="sxs-lookup"><span data-stu-id="56c45-108">Deploying VMs for testing</span></span>
<span data-ttu-id="56c45-109">Pro účely tohoto testu hello hello dva virtuální počítače by měla být ve buď hello stejné cloudové služby nebo hello stejné skupiny dostupnosti, aby jsme mohli používat svoje interní IP adresy a vyloučit hello nástroje pro vyrovnávání zatížení z testu hello.</span><span class="sxs-lookup"><span data-stu-id="56c45-109">For hello purposes of this test, hello two VMs should be in either hello same Cloud Service or hello same Availability Set so that we can use their internal IPs and exclude hello Load Balancers from hello test.</span></span> <span data-ttu-id="56c45-110">Je možné tootest s hello virtuální IP adresy, ale tento druh testování je mimo rozsah hello tohoto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="56c45-110">It is possible tootest with hello VIP but this kind of testing is outside hello scope of this document.</span></span>
 
<span data-ttu-id="56c45-111">Poznamenejte si hello příjemce IP adresy.</span><span class="sxs-lookup"><span data-stu-id="56c45-111">Make a note of hello RECEIVER's IP address.</span></span> <span data-ttu-id="56c45-112">Umožňuje volání této IP "a.b.c.r"</span><span class="sxs-lookup"><span data-stu-id="56c45-112">Let's call that IP "a.b.c.r"</span></span>

<span data-ttu-id="56c45-113">Poznamenejte si hello počet jader na hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="56c45-113">Make a note of hello number of cores on hello VM.</span></span> <span data-ttu-id="56c45-114">To umožňuje volání "\#num\_jader"</span><span class="sxs-lookup"><span data-stu-id="56c45-114">Let's call this "\#num\_cores"</span></span>
 
<span data-ttu-id="56c45-115">Spusťte hello NTTTCP otestovat 300 sekund (nebo 5 minut) na hello odesílatele virtuálních počítačů a příjemce virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="56c45-115">Run hello NTTTCP test for 300 seconds (or 5 minutes) on hello sender VM and receiver VM.</span></span>

<span data-ttu-id="56c45-116">Tip: Při nastavování tento test pro hello poprvé, můžete se pokusit kratší názor období tooget testovací dříve.</span><span class="sxs-lookup"><span data-stu-id="56c45-116">Tip: When setting up this test for hello first time, you might try a shorter test period tooget feedback sooner.</span></span> <span data-ttu-id="56c45-117">Jakmile nástroj hello funguje podle očekávání, rozšiřte hello zkušební období too300 sekund hello nejpřesnější výsledky.</span><span class="sxs-lookup"><span data-stu-id="56c45-117">Once hello tool is working as expected, extend hello test period too300 seconds for hello most accurate results.</span></span>

> [!NOTE]
> <span data-ttu-id="56c45-118">Odesílatel Hello **a** musíte zadat příjemce **hello stejné** testovací parametr doba trvání (-t).</span><span class="sxs-lookup"><span data-stu-id="56c45-118">hello sender **and** receiver must specify **hello same** test duration parameter (-t).</span></span>

<span data-ttu-id="56c45-119">tootest jednoho datového proudu TCP 10 sekund:</span><span class="sxs-lookup"><span data-stu-id="56c45-119">tootest a single TCP stream for 10 seconds:</span></span>

<span data-ttu-id="56c45-120">Příjemce parametry: ntttcp - r -t 10 - P 1</span><span class="sxs-lookup"><span data-stu-id="56c45-120">Receiver parameters: ntttcp -r -t 10 -P 1</span></span>

<span data-ttu-id="56c45-121">Odesílatel parametry: ntttcp-s10.27.33.7 -t 10 - n 1 -P 1</span><span class="sxs-lookup"><span data-stu-id="56c45-121">Sender parameters: ntttcp -s10.27.33.7 -t 10 -n 1 -P 1</span></span>

> [!NOTE]
> <span data-ttu-id="56c45-122">Hello předchozích ukázkových by měly být použité tooconfirm jenom při konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="56c45-122">hello preceding sample should only be used tooconfirm your configuration.</span></span> <span data-ttu-id="56c45-123">Testování platný příklady jsou popsané dál v tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="56c45-123">Valid examples of testing are covered later in this document.</span></span>

## <a name="testing-vms-running-windows"></a><span data-ttu-id="56c45-124">Testování virtuální počítače se systémem WINDOWS:</span><span class="sxs-lookup"><span data-stu-id="56c45-124">Testing VMs running WINDOWS:</span></span>

#### <a name="get-ntttcp-onto-hello-vms"></a><span data-ttu-id="56c45-125">NTTTCP dostanou do hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="56c45-125">Get NTTTCP onto hello VMs.</span></span>

<span data-ttu-id="56c45-126">Stažení nejnovější verze hello: <https://gallery.technet.microsoft.com/NTttcp-Version-528-Now-f8b12769></span><span class="sxs-lookup"><span data-stu-id="56c45-126">Download hello latest version: <https://gallery.technet.microsoft.com/NTttcp-Version-528-Now-f8b12769></span></span>

<span data-ttu-id="56c45-127">Nebo je-li přesunout hledání: <https://www.bing.com/search?q=ntttcp+download> \< – musí být nejprve dosáhl</span><span class="sxs-lookup"><span data-stu-id="56c45-127">Or search for it if moved: <https://www.bing.com/search?q=ntttcp+download>\< -- should be first hit</span></span>

<span data-ttu-id="56c45-128">Zvažte umístění NTTTCP samostatné složky, jako je c:\\nástroje</span><span class="sxs-lookup"><span data-stu-id="56c45-128">Consider putting NTTTCP in separate folder, like c:\\tools</span></span>

#### <a name="allow-ntttcp-through-hello-windows-firewall"></a><span data-ttu-id="56c45-129">Povolit NTTTCP přes bránu firewall systému Windows hello</span><span class="sxs-lookup"><span data-stu-id="56c45-129">Allow NTTTCP through hello Windows firewall</span></span>
<span data-ttu-id="56c45-130">Na hello příjemce vytvořte pravidlo povolení na hello brány Windows Firewall tooallow tooarrive provoz NTTTCP.</span><span class="sxs-lookup"><span data-stu-id="56c45-130">On hello RECEIVER, create an Allow rule on hello Windows Firewall tooallow the NTTTCP traffic tooarrive.</span></span> <span data-ttu-id="56c45-131">Místo příchozí tooallow specifické porty protokolu TCP je nejjednodušší tooallow hello celý NTTTCP program podle názvu.</span><span class="sxs-lookup"><span data-stu-id="56c45-131">It's easiest tooallow hello entire NTTTCP program by name rather than tooallow specific TCP ports inbound.</span></span>

<span data-ttu-id="56c45-132">Povolit ntttcp prostřednictvím hello brány Windows Firewall takto:</span><span class="sxs-lookup"><span data-stu-id="56c45-132">Allow ntttcp through hello Windows Firewall like this:</span></span>

<span data-ttu-id="56c45-133">netsh advfirewall firewall přidejte pravidlo program =\<cesta\>\\ntttcp.exe název = "ntttcp" protokol = všechny dir = v akci = povolit povolit = yes profilu = ANY</span><span class="sxs-lookup"><span data-stu-id="56c45-133">netsh advfirewall firewall add rule program=\<PATH\>\\ntttcp.exe name="ntttcp" protocol=any dir=in action=allow enable=yes profile=ANY</span></span>

<span data-ttu-id="56c45-134">Například, pokud jste zkopírovali ntttcp.exe toohello "c:\\nástroje" složky, bude příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="56c45-134">For example, if you copied ntttcp.exe toohello "c:\\tools" folder, this would be hello command:</span></span> 

<span data-ttu-id="56c45-135">netsh advfirewall firewall přidejte pravidlo program = c:\\nástroje\\ntttcp.exe název = "ntttcp" protokol = všechny dir = v akci = povolit povolit = yes profilu = ANY</span><span class="sxs-lookup"><span data-stu-id="56c45-135">netsh advfirewall firewall add rule program=c:\\tools\\ntttcp.exe name="ntttcp" protocol=any dir=in action=allow enable=yes profile=ANY</span></span>

#### <a name="running-ntttcp-tests"></a><span data-ttu-id="56c45-136">Spouštění testů NTTTCP</span><span class="sxs-lookup"><span data-stu-id="56c45-136">Running NTTTCP tests</span></span>

<span data-ttu-id="56c45-137">Spusťte NTTTCP na hello příjemce (**spustit z CMD**, nikoli z prostředí PowerShell):</span><span class="sxs-lookup"><span data-stu-id="56c45-137">Start NTTTCP on hello RECEIVER (**run from CMD**, not from PowerShell):</span></span>

<span data-ttu-id="56c45-138">ntttcp - r – m [2\*\#num\_jader],\*, a.b.c.r -t 300</span><span class="sxs-lookup"><span data-stu-id="56c45-138">ntttcp -r –m [2\*\#num\_cores],\*,a.b.c.r -t 300</span></span>

<span data-ttu-id="56c45-139">Pokud hello virtuálních počítačů má čtyři jader a IP adresu 10.0.0.4, by vypadat například takto:</span><span class="sxs-lookup"><span data-stu-id="56c45-139">If hello VM has four cores and an IP address of 10.0.0.4, it would look like this:</span></span>

<span data-ttu-id="56c45-140">ntttcp - r – m 8,\*, 10.0.0.4 -t 300</span><span class="sxs-lookup"><span data-stu-id="56c45-140">ntttcp -r –m 8,\*,10.0.0.4 -t 300</span></span>


<span data-ttu-id="56c45-141">Spusťte NTTTCP na hello odesílatele (**spustit z CMD**, nikoli z prostředí PowerShell):</span><span class="sxs-lookup"><span data-stu-id="56c45-141">Start NTTTCP on hello SENDER (**run from CMD**, not from PowerShell):</span></span>

<span data-ttu-id="56c45-142">ntttcp -s – m 8,\*, 10.0.0.4 -t 300</span><span class="sxs-lookup"><span data-stu-id="56c45-142">ntttcp -s –m 8,\*,10.0.0.4 -t 300</span></span> 

<span data-ttu-id="56c45-143">Počkejte, než hello výsledků.</span><span class="sxs-lookup"><span data-stu-id="56c45-143">Wait for hello results.</span></span>


## <a name="testing-vms-running-linux"></a><span data-ttu-id="56c45-144">Testování virtuální počítače se systémem LINUX:</span><span class="sxs-lookup"><span data-stu-id="56c45-144">Testing VMs running LINUX:</span></span>

<span data-ttu-id="56c45-145">Použijte nttcp pro linux.</span><span class="sxs-lookup"><span data-stu-id="56c45-145">Use nttcp-for-linux.</span></span> <span data-ttu-id="56c45-146">Je k dispozici z <https://github.com/Microsoft/ntttcp-for-linux></span><span class="sxs-lookup"><span data-stu-id="56c45-146">It is available from <https://github.com/Microsoft/ntttcp-for-linux></span></span>

<span data-ttu-id="56c45-147">Na hello virtuální počítače s Linuxem (odesílatele i příjemce) spusťte tyto příkazy a příprava ntttcp pro linux na virtuální počítače:</span><span class="sxs-lookup"><span data-stu-id="56c45-147">On hello Linux VMs (both SENDER and RECEIVER), run these commands to prepare ntttcp-for-linux on your VMs:</span></span>

<span data-ttu-id="56c45-148">CentOS – instalace Git:</span><span class="sxs-lookup"><span data-stu-id="56c45-148">CentOS - Install Git:</span></span>
``` bash
  yum install gcc -y  
  yum install git -y
```
<span data-ttu-id="56c45-149">Ubuntu – instalace Git:</span><span class="sxs-lookup"><span data-stu-id="56c45-149">Ubuntu - Install Git:</span></span>
``` bash
 apt-get -y install build-essential  
 apt-get -y install git
```
<span data-ttu-id="56c45-150">Ujistěte se a nainstalujte na obou:</span><span class="sxs-lookup"><span data-stu-id="56c45-150">Make and Install on both:</span></span>
``` bash
 git clone <https://github.com/Microsoft/ntttcp-for-linux>
 cd ntttcp-for-linux/src
 make && make install
```

<span data-ttu-id="56c45-151">Jako třeba Windows hello jsme předpokládají, že příjemce Linux hello IP je 10.0.0.4</span><span class="sxs-lookup"><span data-stu-id="56c45-151">As in hello Windows example, we assume hello Linux RECEIVER's IP is 10.0.0.4</span></span>

<span data-ttu-id="56c45-152">Spusťte na hello příjemce NTTTCP pro Linux:</span><span class="sxs-lookup"><span data-stu-id="56c45-152">Start NTTTCP-for-Linux on hello RECEIVER:</span></span>

``` bash
ntttcp -r -t 300
```

<span data-ttu-id="56c45-153">A na hello odesílatele, spusťte:</span><span class="sxs-lookup"><span data-stu-id="56c45-153">And on hello SENDER, run:</span></span>

``` bash
ntttcp -s10.0.0.4 -t 300
```
 
<span data-ttu-id="56c45-154">Test délka výchozí too60 sekund Pokud žádný čas parametr je zadána.</span><span class="sxs-lookup"><span data-stu-id="56c45-154">Test length defaults too60 seconds if no time parameter is given</span></span>

## <a name="testing-between-vms-running-windows-and-linux"></a><span data-ttu-id="56c45-155">Testování mezi virtuální počítače se systémem Windows a LINUX:</span><span class="sxs-lookup"><span data-stu-id="56c45-155">Testing between VMs running Windows and LINUX:</span></span>

<span data-ttu-id="56c45-156">Pro tohoto scénáře jsme by měl povolit režim hello no-sync, můžete spustit hello test.</span><span class="sxs-lookup"><span data-stu-id="56c45-156">On this scenarios we should enable hello no-sync mode so hello test can run.</span></span> <span data-ttu-id="56c45-157">To se provádí pomocí hello **-N příznak** pro systémy Linux a **příznak -ns** pro systém Windows.</span><span class="sxs-lookup"><span data-stu-id="56c45-157">This is done by using hello **-N flag** for Linux, and **-ns flag** for Windows.</span></span>

#### <a name="from-linux-toowindows"></a><span data-ttu-id="56c45-158">Z Linux tooWindows:</span><span class="sxs-lookup"><span data-stu-id="56c45-158">From Linux tooWindows:</span></span>

<span data-ttu-id="56c45-159">Příjemce <Windows>:</span><span class="sxs-lookup"><span data-stu-id="56c45-159">Receiver <Windows>:</span></span>

``` bash
ntttcp -r -m <2 x nr cores>,*,<Windows server IP>
```

<span data-ttu-id="56c45-160">Odesílatel <Linux> :</span><span class="sxs-lookup"><span data-stu-id="56c45-160">Sender <Linux> :</span></span>

``` bash
ntttcp -s -m <2 x nr cores>,*,<Windows server IP> -N -t 300
```

#### <a name="from-windows-toolinux"></a><span data-ttu-id="56c45-161">Z Windows tooLinux:</span><span class="sxs-lookup"><span data-stu-id="56c45-161">From Windows tooLinux:</span></span>

<span data-ttu-id="56c45-162">Příjemce <Linux>:</span><span class="sxs-lookup"><span data-stu-id="56c45-162">Receiver <Linux>:</span></span>

``` bash
ntttcp -r -m <2 x nr cores>,*,<Linux server IP>
```

<span data-ttu-id="56c45-163">Odesílatel <Windows>:</span><span class="sxs-lookup"><span data-stu-id="56c45-163">Sender <Windows>:</span></span>

``` bash
ntttcp -s -m <2 x nr cores>,*,<Linux  server IP> -ns -t 300
```

## <a name="next-steps"></a><span data-ttu-id="56c45-164">Další kroky</span><span class="sxs-lookup"><span data-stu-id="56c45-164">Next steps</span></span>
* <span data-ttu-id="56c45-165">V závislosti na výsledky, je možné, místnosti příliš[optimalizovat počítače propustnost sítě](virtual-network-optimize-network-bandwidth.md) pro váš scénář.</span><span class="sxs-lookup"><span data-stu-id="56c45-165">Depending on results, there may be room too[Optimize network throughput machines](virtual-network-optimize-network-bandwidth.md) for your scenario.</span></span>
* <span data-ttu-id="56c45-166">Další informace s [Azure Virtual Network nejčastější dotazy (FAQ)](virtual-networks-faq.md)</span><span class="sxs-lookup"><span data-stu-id="56c45-166">Learn more wtih [Azure Virtual Network frequently asked questions (FAQ)](virtual-networks-faq.md)</span></span>
