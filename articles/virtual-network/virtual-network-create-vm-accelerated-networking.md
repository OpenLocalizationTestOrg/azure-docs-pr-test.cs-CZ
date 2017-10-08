---
title: "aaaCreate virtuálního počítače Azure pomocí Accelerated sítě | Microsoft Docs"
description: "Zjistěte, jak toocreate virtuální počítač s Accelerated sítě."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/10/2017
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 241362891bfe083ab32c2f558be54f63f87c0693
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-machine-with-accelerated-networking"></a><span data-ttu-id="4a031-103">Vytvoření virtuálního počítače pomocí Accelerated sítě</span><span class="sxs-lookup"><span data-stu-id="4a031-103">Create a virtual machine with Accelerated Networking</span></span>

<span data-ttu-id="4a031-104">V tomto kurzu zjistíte, jak toocreate virtuálního počítače (virtuální počítač Azure) pomocí Accelerated sítě.</span><span class="sxs-lookup"><span data-stu-id="4a031-104">In this tutorial, you learn how toocreate an Azure Virtual Machine (VM) with Accelerated Networking.</span></span> <span data-ttu-id="4a031-105">Zrychlený sítě je GA pro systém Windows a ve veřejné verzi Preview pro konkrétní distribuce systému Linux.</span><span class="sxs-lookup"><span data-stu-id="4a031-105">Accelerated Networking is GA for Windows and in a Public Preview for specific Linux distributions.</span></span> <span data-ttu-id="4a031-106">Zrychlený sítě umožňuje jeden kořenový vstupně-výstupních operací virtualizace (SR-IOV) tooa virtuálního počítače, výrazně zlepšit sítě.</span><span class="sxs-lookup"><span data-stu-id="4a031-106">Accelerated networking enables single root I/O virtualization (SR-IOV) tooa VM, greatly improving its networking performance.</span></span> <span data-ttu-id="4a031-107">Tato cesta vysoce výkonné obchází hello hostitele z hello datapath snižuje latence, zpoždění a využití procesoru pro použití s hello nejnáročnější zatížení sítě na podporované typy virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="4a031-107">This high-performance path bypasses hello host from hello datapath reducing latency, jitter, and CPU utilization, for use with hello most demanding network workloads on supported VM types.</span></span> <span data-ttu-id="4a031-108">Následující obrázek ukazuje komunikaci mezi dvěma virtuálními počítači (VM) s i bez Zrychlený sítě Hello:</span><span class="sxs-lookup"><span data-stu-id="4a031-108">hello following picture shows communication between two virtual machines (VM) with and without accelerated networking:</span></span>

![Porovnání](./media/virtual-network-create-vm-accelerated-networking/image1.png)

<span data-ttu-id="4a031-110">Bez Zrychlený sítě, musí všechny síťové přenosy a deaktivovat hello virtuálních počítačů procházení hello hostitele a virtuálního přepínače hello.</span><span class="sxs-lookup"><span data-stu-id="4a031-110">Without accelerated networking, all networking traffic in and out of hello VM must traverse hello host and hello virtual switch.</span></span> <span data-ttu-id="4a031-111">Hello virtuální přepínač poskytuje všechny vynucení zásad, například jako skupin zabezpečení sítě přístup seznamy řízení, izolace a jiné síťové virtualizované služby toonetwork provoz.</span><span class="sxs-lookup"><span data-stu-id="4a031-111">hello virtual switch provides all policy enforcement, such as network security groups, access control lists, isolation, and other network virtualized services toonetwork traffic.</span></span> <span data-ttu-id="4a031-112">Další informace o virtuální přepínače, přečtěte si hello toolearn [virtualizace sítě Hyper-V a virtuálního přepínače](https://technet.microsoft.com/library/jj945275.aspx) článku.</span><span class="sxs-lookup"><span data-stu-id="4a031-112">toolearn more about virtual switches, read hello [Hyper-V network virtualization and virtual switch](https://technet.microsoft.com/library/jj945275.aspx) article.</span></span>

<span data-ttu-id="4a031-113">Zrychlený sítě, síťové přenosy dorazí na rozhraní sítě hello Virtuálního počítače (NIC) a předá toohello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="4a031-113">With accelerated networking, network traffic arrives at hello VM's network interface (NIC), and is then forwarded toohello VM.</span></span> <span data-ttu-id="4a031-114">Všechny zásady sítě, které hello virtuální přepínač platí bez Zrychlený sítě se sníženou zátěží a použít v hardwaru.</span><span class="sxs-lookup"><span data-stu-id="4a031-114">All network policies that hello virtual switch applies without accelerated networking are offloaded and applied in hardware.</span></span> <span data-ttu-id="4a031-115">Použít zásady v hardwaru umožňuje hello seskupování tooforward síťový provoz přímo toohello virtuálních počítačů, obcházení hello hostitele a virtuálního přepínače hello, při zachování všech hello zásady, které se použijí v hello hostitele.</span><span class="sxs-lookup"><span data-stu-id="4a031-115">Applying policy in hardware enables hello NIC tooforward network traffic directly toohello VM, bypassing hello host and hello virtual switch, while maintaining all hello policy it applied in hello host.</span></span>

<span data-ttu-id="4a031-116">Hello výhod Zrychlený sítě se projeví pouze toohello virtuální počítač, který je zapnutá.</span><span class="sxs-lookup"><span data-stu-id="4a031-116">hello benefits of accelerated networking only apply toohello VM that it is enabled on.</span></span> <span data-ttu-id="4a031-117">Nejlepších výsledků dosáhnete hello, je ideální tooenable tuto funkci na alespoň dva virtuální počítače připojené toohello stejné virtuální síti Azure (VNet).</span><span class="sxs-lookup"><span data-stu-id="4a031-117">For hello best results, it is ideal tooenable this feature on at least two VMs connected toohello same Azure Virtual Network (VNet).</span></span> <span data-ttu-id="4a031-118">Při komunikaci mezi virtuálními sítěmi nebo připojování místní, tato funkce má minimální dopad toooverall latence.</span><span class="sxs-lookup"><span data-stu-id="4a031-118">When communicating across VNets or connecting on-premises, this feature has minimal impact toooverall latency.</span></span>

> [!WARNING]
> <span data-ttu-id="4a031-119">Tento Linux verzi Public Preview nemusí mít stejnou úroveň dostupnost a spolehlivost, stejně jako funkce, které jsou hello obecné dostupnosti k verzi.</span><span class="sxs-lookup"><span data-stu-id="4a031-119">This Linux Public Preview may not have hello same level of availability and reliability as features that are in general availability release.</span></span> <span data-ttu-id="4a031-120">Hello funkce nepodporuje, může mít omezené možnosti a nemusí být k dispozici ve všech Azure umístění.</span><span class="sxs-lookup"><span data-stu-id="4a031-120">hello feature is not supported, may have constrained capabilities, and may not be available in all Azure locations.</span></span> <span data-ttu-id="4a031-121">Hello nejaktuálnější upozornění na stav této funkce a dostupnost zkontrolujte hello Azure Virtual Network aktualizací stránky.</span><span class="sxs-lookup"><span data-stu-id="4a031-121">For hello most up-to-date notifications on availability and status of this feature, check hello Azure Virtual Network updates page.</span></span>

## <a name="benefits"></a><span data-ttu-id="4a031-122">Výhody</span><span class="sxs-lookup"><span data-stu-id="4a031-122">Benefits</span></span>
* <span data-ttu-id="4a031-123">**Nižší latenci vyšší pakety za sekundu (pps):** odebrání hello virtuální přepínač z hello datapath odebere hello doby, po paketů v hello hostitele pro zpracování zásad a zvyšuje hello počet paketů, které lze zpracovat uvnitř hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="4a031-123">**Lower Latency / Higher packets per second (pps):** Removing hello virtual switch from hello datapath removes hello time packets spend in hello host for policy processing and increases hello number of packets that can be processed inside hello VM.</span></span>
* <span data-ttu-id="4a031-124">**Snižuje zpoždění:** virtuální přepínač zpracování závisí na množství hello zásady, které musí použít toobe a hello zatížení procesoru Dobrý den, které provádí zpracování hello.</span><span class="sxs-lookup"><span data-stu-id="4a031-124">**Reduced jitter:** Virtual switch processing depends on hello amount of policy that needs toobe applied and hello workload of hello CPU that is doing hello processing.</span></span> <span data-ttu-id="4a031-125">Snižování zátěže hardwaru toohello vynucení zásad hello odstraní tento variabilita tím, že doručování paketů přímo přepne toohello virtuálních počítačů, odebere hello hostitele tooVM komunikace a všechna přerušení softwaru a kontext.</span><span class="sxs-lookup"><span data-stu-id="4a031-125">Offloading hello policy enforcement toohello hardware removes that variability by delivering packets directly toohello VM, removing hello host tooVM communication and all software interrupts and context switches.</span></span>
* <span data-ttu-id="4a031-126">**Snížení využití procesoru:** Bypassing hello virtuální přepínač na hostiteli hello vede využití procesoru tooless ke zpracování síťového provozu.</span><span class="sxs-lookup"><span data-stu-id="4a031-126">**Decreased CPU utilization:** Bypassing hello virtual switch in hello host leads tooless CPU utilization for processing network traffic.</span></span>

## <span data-ttu-id="4a031-127"><a name="Limitations"></a>Omezení</span><span class="sxs-lookup"><span data-stu-id="4a031-127"><a name="Limitations"></a>Limitations</span></span>
<span data-ttu-id="4a031-128">Při používání této funkce existují Hello následující omezení:</span><span class="sxs-lookup"><span data-stu-id="4a031-128">hello following limitations exist when using this capability:</span></span>

* <span data-ttu-id="4a031-129">**Vytvoření rozhraní sítě:** Accelerated sítě lze povolit pouze pro nový síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="4a031-129">**Network interface creation:** Accelerated networking can only be enabled for a new NIC.</span></span> <span data-ttu-id="4a031-130">Nelze nastavit pro existující síťovou.</span><span class="sxs-lookup"><span data-stu-id="4a031-130">It cannot be enabled for an existing NIC.</span></span>
* <span data-ttu-id="4a031-131">**Vytvoření virtuálního počítače:** A síťovým Adaptérem s Zrychlený sítě povolené může obsahovat jenom být připojené tooa virtuálních počítačů při hello virtuální počítač je vytvořen.</span><span class="sxs-lookup"><span data-stu-id="4a031-131">**VM creation:** A NIC with accelerated networking enabled can only be attached tooa VM when hello VM is created.</span></span> <span data-ttu-id="4a031-132">Hello síťový adaptér nesmí být připojené tooan existující virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="4a031-132">hello NIC cannot be attached tooan existing VM.</span></span>
* <span data-ttu-id="4a031-133">**Oblasti:** virtuálních počítačů Windows s Zrychlený sítě jsou nabízena v oblastech nejvíce Azure.</span><span class="sxs-lookup"><span data-stu-id="4a031-133">**Regions:** Windows VMs with accelerated networking are offered in most Azure regions.</span></span> <span data-ttu-id="4a031-134">Virtuální počítače Linux s Zrychlený sítě jsou nabízena v několika oblastech.</span><span class="sxs-lookup"><span data-stu-id="4a031-134">Linux VMs with accelerated networking are offered in multiple regions.</span></span> <span data-ttu-id="4a031-135">Hello oblastí, které tato funkce je dostupná v zvětšuje.</span><span class="sxs-lookup"><span data-stu-id="4a031-135">hello regions this capability is available in is expanding.</span></span> <span data-ttu-id="4a031-136">Naleznete v blogu aktualizace virtuální sítě na Azure hello níže hello nejnovější informace.</span><span class="sxs-lookup"><span data-stu-id="4a031-136">Please see hello Azure Virtual Networking Updates blog below for hello latest information.</span></span>   
* <span data-ttu-id="4a031-137">**Podporované operační systémy:** Windows: Microsoft Windows Server 2012 R2 Datacenter a Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="4a031-137">**Supported operating systems:** Windows: Microsoft Windows Server 2012 R2 Datacenter and Windows Server 2016.</span></span> <span data-ttu-id="4a031-138">Linux: Ubuntu Server 16.04 LTS s 4.4.0-77 jádra nebo vyšší, SLES 12 SP2, RHEL 7.3 a CentOS 7.3 (publikováno "Podvodný Wave software").</span><span class="sxs-lookup"><span data-stu-id="4a031-138">Linux: Ubuntu Server 16.04 LTS with kernel 4.4.0-77 or higher, SLES 12 SP2, RHEL 7.3 and CentOS 7.3 (Published by “Rogue Wave Software”).</span></span>
* <span data-ttu-id="4a031-139">**Velikost virtuálního počítače:** velikost optimalizované výpočetní instance s osmi nebo více jader a obecné účely.</span><span class="sxs-lookup"><span data-stu-id="4a031-139">**VM Size:** General purpose and compute-optimized instance sizes with eight or more cores.</span></span> <span data-ttu-id="4a031-140">Další informace najdete v tématu hello [Windows](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) a [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) články velikostí virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="4a031-140">For more information, see hello [Windows](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) and [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) VM sizes articles.</span></span> <span data-ttu-id="4a031-141">Hello sadu podporované velikosti instance virtuálních počítačů bude rozšiřovat v budoucnu hello.</span><span class="sxs-lookup"><span data-stu-id="4a031-141">hello set of supported VM instance sizes will expand in hello future.</span></span>
* <span data-ttu-id="4a031-142">**Nasazení pomocí Azure Resource Manager (ARM) pouze:** Accelerated síťové služby není k dispozici pro nasazení prostřednictvím ASM/RDFE.</span><span class="sxs-lookup"><span data-stu-id="4a031-142">**Deployment through Azure Resource Manager (ARM) only:** Accelerated Networking is not available for deployment through ASM/RDFE.</span></span>

<span data-ttu-id="4a031-143">Omezení toothese změny jsou oznámeno prostřednictvím hello [virtuální sítí Azure aktualizace](https://azure.microsoft.com/updates/accelerated-networking-in-preview) stránky.</span><span class="sxs-lookup"><span data-stu-id="4a031-143">Changes toothese limitations are announced through hello [Azure Virtual Networking updates](https://azure.microsoft.com/updates/accelerated-networking-in-preview) page.</span></span>

## <a name="create-a-windows-vm"></a><span data-ttu-id="4a031-144">Vytvoření virtuálního počítače s Windows</span><span class="sxs-lookup"><span data-stu-id="4a031-144">Create a Windows VM</span></span>
<span data-ttu-id="4a031-145">Můžete použít hello portál Azure nebo Azure [prostředí PowerShell](#windows-powershell) toocreate hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="4a031-145">You can use hello Azure portal or Azure [PowerShell](#windows-powershell) toocreate hello VM.</span></span>

### <span data-ttu-id="4a031-146"><a name="windows-portal"></a>Portál</span><span class="sxs-lookup"><span data-stu-id="4a031-146"><a name="windows-portal"></a>Portal</span></span>

1. <span data-ttu-id="4a031-147">Z internetového prohlížeče, otevřete hello Azure [portál](https://portal.azure.com) a přihlaste se pomocí vaší Azure [účet](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account).</span><span class="sxs-lookup"><span data-stu-id="4a031-147">From an Internet browser, open hello Azure [portal](https://portal.azure.com) and sign in with your Azure [account](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account).</span></span> <span data-ttu-id="4a031-148">Pokud účet nemáte, můžete si zaregistrovat [bezplatnou zkušební verzi](https://azure.microsoft.com/offers/ms-azr-0044p).</span><span class="sxs-lookup"><span data-stu-id="4a031-148">If you don't already have an account, you can sign up for a [free trial](https://azure.microsoft.com/offers/ms-azr-0044p).</span></span>
2. <span data-ttu-id="4a031-149">Hello portálu, klikněte na tlačítko **+ nový** > **výpočetní** > **Windows Server 2016 Datacenter**.</span><span class="sxs-lookup"><span data-stu-id="4a031-149">In hello portal, click **+ New** > **Compute** > **Windows Server 2016 Datacenter**.</span></span> 
3. <span data-ttu-id="4a031-150">V hello **Windows Server 2016 Datacenter** okno, které se zobrazí, ponechejte *Resource Manager* vybrané pod **vybrat model nasazení**a klikněte na tlačítko **vytvořit** .</span><span class="sxs-lookup"><span data-stu-id="4a031-150">In hello **Windows Server 2016 Datacenter** blade that appears, leave *Resource Manager* selected under **Select a deployment model**, and click **Create**.</span></span>
4. <span data-ttu-id="4a031-151">V hello **Základy** okno, které se zobrazí, zadejte hello následující hodnoty, ponechejte zbývající výchozí možnosti hello nebo vyberte nebo zadejte vlastní hodnoty a klikněte na tlačítko hello **OK** tlačítko:</span><span class="sxs-lookup"><span data-stu-id="4a031-151">In hello **Basics** blade that appears, enter hello following values, leave hello remaining default options or select or enter your own values, and click hello **OK** button:</span></span>

    |<span data-ttu-id="4a031-152">Nastavení</span><span class="sxs-lookup"><span data-stu-id="4a031-152">Setting</span></span>|<span data-ttu-id="4a031-153">Hodnota</span><span class="sxs-lookup"><span data-stu-id="4a031-153">Value</span></span>|
    |---|---|
    |<span data-ttu-id="4a031-154">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="4a031-154">Name</span></span>|<span data-ttu-id="4a031-155">Můjvp</span><span class="sxs-lookup"><span data-stu-id="4a031-155">MyVm</span></span>|
    |<span data-ttu-id="4a031-156">Skupina prostředků</span><span class="sxs-lookup"><span data-stu-id="4a031-156">Resource group</span></span>|<span data-ttu-id="4a031-157">Nechte **vytvořit nový** vybrané a zadejte *MyResourceGroup*</span><span class="sxs-lookup"><span data-stu-id="4a031-157">Leave **Create new** selected and enter *MyResourceGroup*</span></span>|
    |<span data-ttu-id="4a031-158">Umístění</span><span class="sxs-lookup"><span data-stu-id="4a031-158">Location</span></span>|<span data-ttu-id="4a031-159">Západní USA 2</span><span class="sxs-lookup"><span data-stu-id="4a031-159">West US 2</span></span>|

    <span data-ttu-id="4a031-160">Pokud jste nový tooAzure, další informace o [skupiny prostředků](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group), [odběry](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription), a [umístění](https://azure.microsoft.com/regions) (která se také označují tooas oblasti).</span><span class="sxs-lookup"><span data-stu-id="4a031-160">If you're new tooAzure, learn more about [Resource groups](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group), [subscriptions](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription), and [locations](https://azure.microsoft.com/regions) (which are also referred tooas regions).</span></span>
5. <span data-ttu-id="4a031-161">V hello **zvolte velikost** okno, které se zobrazí, zadejte *8* v hello **minimální počet jader** pole a pak klikněte na **zobrazit všechny**.</span><span class="sxs-lookup"><span data-stu-id="4a031-161">In hello **Choose a size** blade that appears, enter *8* in hello **Minimum cores** box, then click **View all**.</span></span>
6. <span data-ttu-id="4a031-162">Klikněte na tlačítko **DS4_V2 standardní**, nebo podporují se všechny virtuální počítač, pak klikněte na tlačítko hello **vyberte** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="4a031-162">Click **DS4_V2 Standard**, or any supported VM, then click hello **Select** button.</span></span>
7. <span data-ttu-id="4a031-163">V hello **nastavení** okno, které se zobrazí, ponechejte všechna nastavení jako-je kromě klikněte na tlačítko **povoleno** v části **Accelerated sítě**, pak klikněte na tlačítko hello **OK** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="4a031-163">In hello **Settings** blade that appears, leave all settings as-is, except click **Enabled** under **Accelerated networking**, then click hello **OK** button.</span></span> <span data-ttu-id="4a031-164">**Poznámka:** , pokud v předchozích krocích jste vybrali hodnoty pro velikost virtuálního počítače, operační systém nebo umístění, které nejsou uvedené v hello [omezení](#Limitations) tohoto článku **Accelerated sítě**nejsou viditelná.</span><span class="sxs-lookup"><span data-stu-id="4a031-164">**Note:** If, in previous steps, you selected values for VM size, operating system, or location that aren't listed in hello [Limitations](#Limitations) section of this article, **Accelerated networking** isn't visible.</span></span>
8. <span data-ttu-id="4a031-165">V hello **Souhrn** okno, které se zobrazí, klikněte na tlačítko hello **OK** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="4a031-165">In hello **Summary** blade that appears, click hello **OK** button.</span></span> <span data-ttu-id="4a031-166">Azure začne vytvářet hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="4a031-166">Azure starts creating hello VM.</span></span> <span data-ttu-id="4a031-167">Vytvoření virtuálního počítače trvá několik minut.</span><span class="sxs-lookup"><span data-stu-id="4a031-167">VM creation takes a few minutes.</span></span>
9. <span data-ttu-id="4a031-168">tooinstall hello accelerated síťové ovladače pro Windows, dokončení hello kroky hello [konfigurace systému Windows](#configure-windows) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="4a031-168">tooinstall hello accelerated networking driver for Windows, complete hello steps in hello [Configure Windows](#configure-windows) section of this article.</span></span>

### <span data-ttu-id="4a031-169"><a name="windows-powershell"></a>Prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="4a031-169"><a name="windows-powershell"></a>PowerShell</span></span>
1. <span data-ttu-id="4a031-170">Nainstalujte nejnovější verzi prostředí Azure PowerShell hello hello [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) modulu.</span><span class="sxs-lookup"><span data-stu-id="4a031-170">Install hello latest version of hello Azure PowerShell [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) module.</span></span> <span data-ttu-id="4a031-171">Pokud jste nový tooAzure prostředí PowerShell, přečtěte si hello [Přehled prostředí Azure PowerShell](/powershell/azure/get-started-azureps?toc=%2fazure%2fvirtual-network%2ftoc.json) článku.</span><span class="sxs-lookup"><span data-stu-id="4a031-171">If you're new tooAzure PowerShell, read hello [Azure PowerShell overview](/powershell/azure/get-started-azureps?toc=%2fazure%2fvirtual-network%2ftoc.json) article.</span></span>
2. <span data-ttu-id="4a031-172">Spusťte relaci prostředí PowerShell kliknutím na tlačítko Spustit Windows hello zadáním **prostředí powershell**, pak levým na **prostředí PowerShell** z výsledků hledání hello.</span><span class="sxs-lookup"><span data-stu-id="4a031-172">Start a PowerShell session by clicking hello Windows Start button, typing **powershell**, then clicking **PowerShell** from hello search results.</span></span>
3. <span data-ttu-id="4a031-173">V okně prostředí PowerShell, zadejte hello `login-azurermaccount` příkaz toosign se pomocí vaší Azure [účet](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account).</span><span class="sxs-lookup"><span data-stu-id="4a031-173">In your PowerShell window, enter hello `login-azurermaccount` command toosign in with your Azure [account](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account).</span></span> <span data-ttu-id="4a031-174">Pokud účet nemáte, můžete si zaregistrovat [bezplatnou zkušební verzi](https://azure.microsoft.com/offers/ms-azr-0044p).</span><span class="sxs-lookup"><span data-stu-id="4a031-174">If you don't already have an account, you can sign up for a [free trial](https://azure.microsoft.com/offers/ms-azr-0044p).</span></span>
4. <span data-ttu-id="4a031-175">V prohlížeči zkopírujte následující skript hello:</span><span class="sxs-lookup"><span data-stu-id="4a031-175">In your browser, copy hello following script:</span></span>
    ```powershell
    $RgName="MyResourceGroup"
    $Location="westus2"
    
    # Create a resource group
    New-AzureRmResourceGroup `
      -Name $RgName `
      -Location $Location
    
    # Create a subnet
    $Subnet = New-AzureRmVirtualNetworkSubnetConfig `
      -Name MySubnet `
      -AddressPrefix 10.0.0.0/24
    
    # Create a virtual network
    $Vnet=New-AzureRmVirtualNetwork `
      -ResourceGroupName $RgName `
      -Location $Location `
      -Name MyVnet `
      -AddressPrefix 10.0.0.0/16 `
      -Subnet $Subnet

    # Create a public IP address
    $Pip = New-AzureRmPublicIpAddress `
      -Name MyPublicIp `
      -ResourceGroupName $RgName `
      -Location $Location `
      -AllocationMethod Static

    # Create a virtual network interface and associate hello public IP address tooit
    $Nic = New-AzureRmNetworkInterface `
      -Name MyNic `
      -ResourceGroupName $RgName `
      -Location $Location `
      -SubnetId $Vnet.Subnets[0].Id `
      -PublicIpAddressId $Pip.Id `
      -EnableAcceleratedNetworking
     
    # Define a credential object for hello VM. PowerShell prompts you for a username and password.
    $Cred = Get-Credential

    # Create a virtual machine configuration
    $VmConfig = New-AzureRmVMConfig `
      -VMName MyVM -VMSize Standard_DS4_v2 | `
      Set-AzureRmVMOperatingSystem `
      -Windows `
      -ComputerName myVM `
      -Credential $Cred | `
    Set-AzureRmVMSourceImage `
      -PublisherName MicrosoftWindowsServer `
      -Offer WindowsServer `
      -Skus 2016-Datacenter `
      -Version latest | `
    Add-AzureRmVMNetworkInterface -Id $Nic.Id 

    # Create hello virtual machine.    
    New-AzureRmVM `
      -ResourceGroupName $RgName `
      -Location $Location `
      -VM $VmConfig
    #
    ```
5. <span data-ttu-id="4a031-176">V okně prostředí PowerShell klikněte pravým tlačítkem na toopaste hello skript a spusťte ho spustíte.</span><span class="sxs-lookup"><span data-stu-id="4a031-176">In your PowerShell window, right-click toopaste hello script and start executing it.</span></span> <span data-ttu-id="4a031-177">Zobrazí se výzva k zadání uživatelského jména a hesla.</span><span class="sxs-lookup"><span data-stu-id="4a031-177">You are prompted for a username and password.</span></span> <span data-ttu-id="4a031-178">Při připojování tooit v dalším kroku hello používejte tyto přihlašovací údaje toolog v toohello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="4a031-178">Use these credentials toolog in toohello VM when connecting tooit in hello next step.</span></span> <span data-ttu-id="4a031-179">Pokud skript hello selže a změnit hodnoty ve skriptu hello před jeho provedením, potvrďte hello hodnoty, které jste použili pro velikost virtuálního počítače, operační systém, a umístění jsou uvedeny v hello [omezení](#Limitations) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="4a031-179">If hello script fails, and you changed values in hello script before executing it, confirm hello values you used for VM size, operating system, and location are listed in hello [Limitations](#Limitations) section of this article.</span></span>
6. <span data-ttu-id="4a031-180">tooinstall hello accelerated síťové ovladače pro Windows, dokončení hello kroky hello [konfigurace systému Windows](#configure-windows) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="4a031-180">tooinstall hello accelerated networking driver for Windows, complete hello steps in hello [Configure Windows](#configure-windows) section of this article.</span></span>

### <span data-ttu-id="4a031-181"><a name="configure-windows"></a>Konfigurace systému Windows</span><span class="sxs-lookup"><span data-stu-id="4a031-181"><a name="configure-windows"></a>Configure Windows</span></span>
<span data-ttu-id="4a031-182">Jakmile vytvoříte hello virtuálních počítačů v Azure, je nutné nainstalovat hello Zrychlený síťové ovladače pro Windows.</span><span class="sxs-lookup"><span data-stu-id="4a031-182">Once you create hello VM in Azure, you must install hello accelerated networking driver for Windows.</span></span> <span data-ttu-id="4a031-183">Před dokončením hello následující kroky, kterou jste vytvořili virtuální počítač s Windows v Zrychlený sítě pomocí buď hello [portál](#windows-portal) nebo [prostředí PowerShell](#windows-powershell) kroky v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="4a031-183">Before completing hello following steps, you must have created a Windows VM with accelerated networking using either hello [portal](#windows-portal) or [PowerShell](#windows-powershell) steps in this article.</span></span> 

1. <span data-ttu-id="4a031-184">Z internetového prohlížeče, otevřete hello Azure [portál](https://portal.azure.com) a přihlaste se pomocí vaší Azure [účet](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account).</span><span class="sxs-lookup"><span data-stu-id="4a031-184">From an Internet browser, open hello Azure [portal](https://portal.azure.com) and sign in with your Azure [account](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account).</span></span> <span data-ttu-id="4a031-185">Pokud účet nemáte, můžete si zaregistrovat [bezplatnou zkušební verzi](https://azure.microsoft.com/offers/ms-azr-0044p).</span><span class="sxs-lookup"><span data-stu-id="4a031-185">If you don't already have an account, you can sign up for a [free trial](https://azure.microsoft.com/offers/ms-azr-0044p).</span></span>
2. <span data-ttu-id="4a031-186">V poli hello, který obsahuje hello text *vyhledávání prostředků* hello horní části hello portálu Azure, zadejte *Můjvp*.</span><span class="sxs-lookup"><span data-stu-id="4a031-186">In hello box that contains hello text *Search resources* at hello top of hello Azure portal, type *MyVm*.</span></span> <span data-ttu-id="4a031-187">Když **Můjvp** se zobrazí ve výsledcích hledání hello, klikněte na něj.</span><span class="sxs-lookup"><span data-stu-id="4a031-187">When **MyVm** appears in hello search results, click it.</span></span>
3. <span data-ttu-id="4a031-188">V hello **Můjvp** okno, které se zobrazí, klikněte na tlačítko hello **Connect** tlačítka na hello levém horním rohu okna hello.</span><span class="sxs-lookup"><span data-stu-id="4a031-188">In hello **MyVm** blade that appears, click hello **Connect** button in hello top left corner of hello blade.</span></span> <span data-ttu-id="4a031-189">**Poznámka:** Pokud **vytváření** je viditelný v hello **Connect** tlačítko Azure ještě nebyla dokončena vytváření hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="4a031-189">**Note:** If **Creating** is visible under hello **Connect** button, Azure has not yet finished creating hello VM.</span></span> <span data-ttu-id="4a031-190">Klikněte na tlačítko **připojit** až poté, co se již nezobrazují **vytváření** pod hello **Connect** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="4a031-190">Click **Connect** only after you no longer see **Creating** under hello **Connect** button.</span></span>
4. <span data-ttu-id="4a031-191">Povolit váš prohlížeč toodownload hello **MyVm.rdp** souboru.</span><span class="sxs-lookup"><span data-stu-id="4a031-191">Allow your browser toodownload hello **MyVm.rdp** file.</span></span>  <span data-ttu-id="4a031-192">Po stažení, klikněte na soubor tooopen hello ho.</span><span class="sxs-lookup"><span data-stu-id="4a031-192">Once downloaded, click hello file tooopen it.</span></span> 
5. <span data-ttu-id="4a031-193">Klikněte na tlačítko hello **připojit** tlačítka na hello **připojení ke vzdálené ploše** pole, který se zobrazí upozornění, že hello nelze identifikovat vydavatele hello vzdáleného připojení.</span><span class="sxs-lookup"><span data-stu-id="4a031-193">Click hello **Connect** button in hello **Remote Desktop Connection** box that appears, notifying you that hello publisher of hello remote connection can't be identified.</span></span>
6. <span data-ttu-id="4a031-194">V hello **zabezpečení systému Windows** pole, které se zobrazí, klikněte na tlačítko **další možnosti**, pak klikněte na tlačítko **použít jiný účet**.</span><span class="sxs-lookup"><span data-stu-id="4a031-194">In hello **Windows Security** box that appears, click **More choices**, then click **Use a different account**.</span></span> <span data-ttu-id="4a031-195">Zadejte hello uživatelské jméno a heslo, které jste zadali v kroku 4, a pak klikněte na tlačítko hello **OK** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="4a031-195">Enter hello username and password you entered in step 4, then click hello **OK** button.</span></span>
7. <span data-ttu-id="4a031-196">Klikněte na tlačítko hello **Ano** tlačítko pole hello připojení ke vzdálené ploše, který se zobrazí upozornění, že nelze ověřit identitu hello hello vzdáleného počítače.</span><span class="sxs-lookup"><span data-stu-id="4a031-196">Click hello **Yes** button in hello Remote Desktop Connection box that appears, notifying you that hello identity of hello remote computer cannot be verified.</span></span>
8. <span data-ttu-id="4a031-197">Klikněte pravým tlačítkem na tlačítko Start systému Windows hello a klikněte na tlačítko **Správce zařízení**.</span><span class="sxs-lookup"><span data-stu-id="4a031-197">Right-click hello Windows Start button and click **Device Manager**.</span></span> <span data-ttu-id="4a031-198">Rozbalte hello **síťové adaptéry** uzlu.</span><span class="sxs-lookup"><span data-stu-id="4a031-198">Expand hello **Network adapters** node.</span></span> <span data-ttu-id="4a031-199">Potvrďte, že hello **Mellanox ConnectX 3 virtuální adaptér Ethernet funkce** se zobrazí, jak je znázorněno v následujícím obrázku hello:</span><span class="sxs-lookup"><span data-stu-id="4a031-199">Confirm that hello **Mellanox ConnectX-3 Virtual Function Ethernet Adapter** appears, as shown in hello following picture:</span></span>
   
    ![Správce zařízení](./media/virtual-network-create-vm-accelerated-networking/image2.png)

9. <span data-ttu-id="4a031-201">Pro virtuální počítač je nyní k dispozici Zrychlený sítě.</span><span class="sxs-lookup"><span data-stu-id="4a031-201">Accelerated Networking is now enabled for your VM.</span></span>

## <a name="create-a-linux-vm"></a><span data-ttu-id="4a031-202">Vytvoření virtuálního počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="4a031-202">Create a Linux VM</span></span>
<span data-ttu-id="4a031-203">Můžete použít hello portál Azure nebo Azure [prostředí PowerShell](#linux-powershell) toocreate Ubuntu nebo SLES virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="4a031-203">You can use hello Azure portal or Azure [PowerShell](#linux-powershell) toocreate an Ubuntu or SLES VM.</span></span> <span data-ttu-id="4a031-204">RHEL a CentOS virtuální počítače se jiný pracovní postup.</span><span class="sxs-lookup"><span data-stu-id="4a031-204">For RHEL and CentOS VMs there is a different workflow.</span></span>  <span data-ttu-id="4a031-205">Podrobnosti viz níže uvedené pokyny hello.</span><span class="sxs-lookup"><span data-stu-id="4a031-205">Please see hello instructions below.</span></span>

### <span data-ttu-id="4a031-206"><a name="linux-portal"></a>Portál</span><span class="sxs-lookup"><span data-stu-id="4a031-206"><a name="linux-portal"></a>Portal</span></span>
1. <span data-ttu-id="4a031-207">Registrace pro hello accelerated sítě pro Linux preview provedením kroků 1 až 5 hello [vytvoření virtuálního počítače s Linuxem - PowerShell](#linux-powershell) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="4a031-207">Register for hello accelerated networking for Linux preview by completing steps 1-5 of hello [Create a Linux VM - PowerShell](#linux-powershell) section of this article.</span></span>  <span data-ttu-id="4a031-208">Nelze zaregistrovat hello preview hello portálu.</span><span class="sxs-lookup"><span data-stu-id="4a031-208">You cannot register for hello preview in hello portal.</span></span>
2. <span data-ttu-id="4a031-209">Dokončení kroků 1 – 8 v hello [vytvoření virtuálního počítače s Windows – portál](#windows-portal) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="4a031-209">Complete steps 1-8 in hello [Create a Windows VM - portal](#windows-portal) section of this article.</span></span> <span data-ttu-id="4a031-210">V kroku 2, klikněte na tlačítko **Ubuntu Server 16.04 LTS** místo **Windows Server 2016 Datacenter**.</span><span class="sxs-lookup"><span data-stu-id="4a031-210">In step 2, click **Ubuntu Server 16.04 LTS** instead of **Windows Server 2016 Datacenter**.</span></span> <span data-ttu-id="4a031-211">V tomto kurzu zvolte toouse heslo, nikoli klíč SSH, i když pro nasazení v produkčním prostředí, můžete použít buď.</span><span class="sxs-lookup"><span data-stu-id="4a031-211">For this tutorial, choose toouse a password, rather than an SSH key, though for production deployments, you can use either.</span></span> <span data-ttu-id="4a031-212">Pokud **Accelerated sítě** po dokončení kroku 7 hello není k dispozici [vytvoření virtuálního počítače s Windows – portál](#windows-portal) části tohoto článku je pravděpodobně pro jednu z následujících důvodů hello:</span><span class="sxs-lookup"><span data-stu-id="4a031-212">If **Accelerated networking** does not appear when you complete step 7 of hello [Create a Windows VM - portal](#windows-portal) section of this article, it's likely for one of hello following reasons:</span></span>
    - <span data-ttu-id="4a031-213">Nejsou ještě registrované pro hello preview.</span><span class="sxs-lookup"><span data-stu-id="4a031-213">You are not yet registered for hello preview.</span></span> <span data-ttu-id="4a031-214">Potvrďte, že je váš stav registrace **registrovaná**, jak je popsáno v kroku 4 hello [vytvoření virtuálního počítače s Linuxem - Powershell](#linux-powershell) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="4a031-214">Confirm that your registration state is **Registered**, as explained in step 4 of hello [Create a Linux VM - Powershell](#linux-powershell) section of this article.</span></span> <span data-ttu-id="4a031-215">**Poznámka:** Pokud jste se účastnili hello Accelerated sítě pro virtuální počítače Windows preview, (jeho žádné delší toouse nezbytné tooregister Accelerated sítě pro virtuální počítače Windows), které nejsou registrované automaticky pro hello Accelerated sítě pro Virtuální počítače s Linuxem preview.</span><span class="sxs-lookup"><span data-stu-id="4a031-215">**Note:** If you participated in hello Accelerated networking for Windows VMs preview (it's no longer necessary tooregister toouse Accelerated networking for Windows VMs), you are not automatically registered for hello Accelerated networking for Linux VMs preview.</span></span> <span data-ttu-id="4a031-216">Je třeba zaregistrovat pro hello Accelerated sítě pro virtuální počítače s Linuxem náhled tooparticipate v ní.</span><span class="sxs-lookup"><span data-stu-id="4a031-216">You must register for hello Accelerated networking for Linux VMs preview tooparticipate in it.</span></span>
    - <span data-ttu-id="4a031-217">Nevybrali jste velikost virtuálního počítače, operační systém nebo umístění, které jsou uvedené v hello [omezení](#limitations) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="4a031-217">You have not selected a VM size, operating system, or location listed in hello [Limitations](#limitations) section of this article.</span></span>
3. <span data-ttu-id="4a031-218">tooinstall hello accelerated síťové ovladače pro Linux, dokončení hello kroky hello [konfigurace Linux](#configure-linux) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="4a031-218">tooinstall hello accelerated networking driver for Linux, complete hello steps in hello [Configure Linux](#configure-linux) section of this article.</span></span>

### <span data-ttu-id="4a031-219"><a name="linux-powershell"></a>Prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="4a031-219"><a name="linux-powershell"></a>PowerShell</span></span>

>[!WARNING]
><span data-ttu-id="4a031-220">Pokud chcete vytvořit virtuální počítače s Linuxem se Zrychlený sítěmi v odběru a pak pokus toocreate virtuální počítač s Windows se Zrychlený sítěmi v hello stejného předplatného, vytvoření virtuálního počítače Windows hello může selhat.</span><span class="sxs-lookup"><span data-stu-id="4a031-220">If you create Linux VMs with accelerated networking in a subscription, and then attempt toocreate a Windows VM with accelerated networking in hello same subscription, hello Windows VM creation may fail.</span></span> <span data-ttu-id="4a031-221">Během této verzi preview se doporučuje otestovat Linux a virtuálních počítačů Windows s Zrychlený sítě v samostatných předplatných.</span><span class="sxs-lookup"><span data-stu-id="4a031-221">During this preview, it's recommended that you test Linux and Windows VMs with accelerated networking in separate subscriptions.</span></span>
>

1. <span data-ttu-id="4a031-222">Nainstalujte nejnovější verzi prostředí Azure PowerShell hello hello [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) modulu.</span><span class="sxs-lookup"><span data-stu-id="4a031-222">Install hello latest version of hello Azure PowerShell [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) module.</span></span> <span data-ttu-id="4a031-223">Pokud jste nový tooAzure prostředí PowerShell, přečtěte si hello [Přehled prostředí Azure PowerShell](/powershell/azure/get-started-azureps?toc=%2fazure%2fvirtual-network%2ftoc.json) článku.</span><span class="sxs-lookup"><span data-stu-id="4a031-223">If you're new tooAzure PowerShell, read hello [Azure PowerShell overview](/powershell/azure/get-started-azureps?toc=%2fazure%2fvirtual-network%2ftoc.json) article.</span></span>
2. <span data-ttu-id="4a031-224">Spusťte relaci prostředí PowerShell kliknutím na tlačítko Spustit Windows hello zadáním **prostředí powershell**, pak levým na **prostředí PowerShell** z výsledků hledání hello.</span><span class="sxs-lookup"><span data-stu-id="4a031-224">Start a PowerShell session by clicking hello Windows Start button, typing **powershell**, then clicking **PowerShell** from hello search results.</span></span>
3. <span data-ttu-id="4a031-225">V okně prostředí PowerShell, zadejte hello `login-azurermaccount` příkaz toosign se pomocí vaší Azure [účet](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account).</span><span class="sxs-lookup"><span data-stu-id="4a031-225">In your PowerShell window, enter hello `login-azurermaccount` command toosign in with your Azure [account](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account).</span></span> <span data-ttu-id="4a031-226">Pokud účet nemáte, můžete si zaregistrovat [bezplatnou zkušební verzi](https://azure.microsoft.com/offers/ms-azr-0044p).</span><span class="sxs-lookup"><span data-stu-id="4a031-226">If you don't already have an account, you can sign up for a [free trial](https://azure.microsoft.com/offers/ms-azr-0044p).</span></span>
4. <span data-ttu-id="4a031-227">Registrace pro hello accelerated sítě pro Azure preview provedením hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="4a031-227">Register for hello accelerated networking for Azure preview by completing hello following steps:</span></span>
    - <span data-ttu-id="4a031-228">Odeslat e-mail příliš[ axnpreview@microsoft.com ](mailto:axnpreview@microsoft.com?subject=Request%20to%20enable%20subscription%20%3csubscription%20id%3e) s ID předplatného Azure a zamýšlené použití.</span><span class="sxs-lookup"><span data-stu-id="4a031-228">Send an email too[axnpreview@microsoft.com](mailto:axnpreview@microsoft.com?subject=Request%20to%20enable%20subscription%20%3csubscription%20id%3e) with your Azure subscription ID and intended use.</span></span> <span data-ttu-id="4a031-229">Počkejte na potvrzení e-mailu společnosti Microsoft o vaše předplatné povolený.</span><span class="sxs-lookup"><span data-stu-id="4a031-229">Please wait for an email confirmation from Microsoft about your subscription being enabled.</span></span>
    - <span data-ttu-id="4a031-230">Zadejte následující příkaz tooconfirm, které jsou registrovány pro verzi preview hello hello:</span><span class="sxs-lookup"><span data-stu-id="4a031-230">Enter hello following command tooconfirm you are registered for hello preview:</span></span>
    
        ```powershell
        Get-AzureRmProviderFeature -FeatureName AllowAcceleratedNetworkingForLinux -ProviderNamespace Microsoft.Network
        ```

        <span data-ttu-id="4a031-231">Pokračujte krokem 5 až **registrovaná** se zobrazí v hello výstup po zadání hello předchozí příkaz.</span><span class="sxs-lookup"><span data-stu-id="4a031-231">Do not continue with step 5 until **Registered** appears in hello output after you enter hello previous command.</span></span> <span data-ttu-id="4a031-232">Hello následující výstup před pokračováním musí vypadat výstupu:</span><span class="sxs-lookup"><span data-stu-id="4a031-232">Your output must look like hello following output before continuing:</span></span>
    
        ```powershell
        FeatureName                        ProviderName      RegistrationState
        -----------                        ------------      -----------------
        AllowAcceleratedNetworkingForLinux Microsoft.Network Registered
        ```
        
      >[!NOTE]
      ><span data-ttu-id="4a031-233">Pokud jste se účastnili hello Accelerated sítě pro virtuální počítače Windows preview, (jeho žádné delší toouse nezbytné tooregister Accelerated sítě pro virtuální počítače Windows), které nejsou registrované automaticky pro hello Accelerated sítě pro virtuální počítače s Linuxem preview.</span><span class="sxs-lookup"><span data-stu-id="4a031-233">If you participated in hello Accelerated networking for Windows VMs preview (it's no longer necessary tooregister toouse Accelerated networking for Windows VMs), you are not automatically registered for hello Accelerated networking for Linux VMs preview.</span></span> <span data-ttu-id="4a031-234">Je třeba zaregistrovat pro hello Accelerated sítě pro virtuální počítače s Linuxem náhled tooparticipate v ní.</span><span class="sxs-lookup"><span data-stu-id="4a031-234">You must register for hello Accelerated networking for Linux VMs preview tooparticipate in it.</span></span>
      >
5. <span data-ttu-id="4a031-235">V prohlížeči zkopírujte následující skript podle potřeby nahraďte Ubuntu nebo SLES hello.</span><span class="sxs-lookup"><span data-stu-id="4a031-235">In your browser, copy hello following script substituting Ubuntu or SLES as desired.</span></span>  <span data-ttu-id="4a031-236">Znovu Redhat a CentOS mít jiný pracovní postup uvedené níže:</span><span class="sxs-lookup"><span data-stu-id="4a031-236">Again, Redhat and CentOS have a different workflow outlined below:</span></span>

    ```powershell
    $RgName="MyResourceGroup"
    $Location="westus2"
    
    # Create a resource group
    New-AzureRmResourceGroup `
      -Name $RgName `
      -Location $Location
    
    # Create a subnet
    $Subnet = New-AzureRmVirtualNetworkSubnetConfig `
      -Name MySubnet `
      -AddressPrefix 10.0.0.0/24
    
    # Create a virtual network
    $Vnet=New-AzureRmVirtualNetwork `
      -ResourceGroupName $RgName `
      -Location $Location `
      -Name MyVnet `
      -AddressPrefix 10.0.0.0/16 `
      -Subnet $Subnet

    # Create a public IP address
    $Pip = New-AzureRmPublicIpAddress `
      -Name MyPublicIp `
      -ResourceGroupName $RgName `
      -Location $Location `
      -AllocationMethod Static

    # Create a virtual network interface and associate hello public IP address tooit
    $Nic = New-AzureRmNetworkInterface `
      -Name MyNic `
      -ResourceGroupName $RgName `
      -Location $Location `
      -SubnetId $Vnet.Subnets[0].Id `
      -PublicIpAddressId $Pip.Id `
      -EnableAcceleratedNetworking
     
    # Create a new Storage account and define hello new VM’s OSDisk name and its URI
    # Must end with ".vhd" extension
    $OSDiskName = "MyOsDiskName.vhd"
    # Storage account name must be between 3 and 24 characters in length and use numbers and lower-case letters only.
    $OSDiskSAName = "thestorageaccountname"  
    $StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $RgName -Name $OSDiskSAName -Type "Standard_GRS" -Location $Location
    $OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName
 
    # Define a credential object for hello VM. PowerShell prompts you for a username and password.
    $Cred = Get-Credential

    # Create a virtual machine configuration
    $VmConfig = New-AzureRmVMConfig `
      -VMName MyVM -VMSize Standard_DS4_v2 | `
      Set-AzureRmVMOperatingSystem `
      -Linux `
      -ComputerName myVM `
      -Credential $Cred | `
    Set-AzureRmVMSourceImage `
      -PublisherName Canonical `
      -Offer UbuntuServer `
      -Skus 16.04-LTS `
      -Version latest | `
    Add-AzureRmVMNetworkInterface -Id $Nic.Id | `
    Set-AzureRmVMOSDisk -Name $OSDiskName `
      -VhdUri $OSDiskUri `
      -CreateOption FromImage 

    # Create hello virtual machine.    
    New-AzureRmVM `
      -ResourceGroupName $RgName `
      -Location $Location `
      -VM $VmConfig
    ```
    
6. <span data-ttu-id="4a031-237">V okně prostředí PowerShell klikněte pravým tlačítkem na toopaste hello skript a spusťte ho spustíte.</span><span class="sxs-lookup"><span data-stu-id="4a031-237">In your PowerShell window, right-click toopaste hello script and start executing it.</span></span> <span data-ttu-id="4a031-238">Zobrazí se výzva k zadání uživatelského jména a hesla.</span><span class="sxs-lookup"><span data-stu-id="4a031-238">You are prompted for a username and password.</span></span> <span data-ttu-id="4a031-239">Při připojování tooit v dalším kroku hello používejte tyto přihlašovací údaje toolog v toohello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="4a031-239">Use these credentials toolog in toohello VM when connecting tooit in hello next step.</span></span> <span data-ttu-id="4a031-240">Pokud se skript hello nezdaří, zkontrolujte, že:</span><span class="sxs-lookup"><span data-stu-id="4a031-240">If hello script fails, confirm that:</span></span>
    - <span data-ttu-id="4a031-241">Jste zaregistrováni pro hello preview, jak je popsáno v kroku 4</span><span class="sxs-lookup"><span data-stu-id="4a031-241">You are registered for hello preview, as explained in step 4</span></span>
    - <span data-ttu-id="4a031-242">Pokud jste změnili velikost, typ operačního systému nebo hodnoty umístění ve skriptu hello virtuálního počítače před jeho provedením, zda text hello hodnoty jsou uvedeny ve hello [omezení](#Limitations) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="4a031-242">If you changed VM size, operating system type, or location values in hello script before executing it, that hello values are listed in hello [Limitations](#Limitations) section of this article.</span></span>
7. <span data-ttu-id="4a031-243">tooinstall hello accelerated síťové ovladače pro Linux, dokončení hello kroky hello [konfigurace Linux](#configure-linux) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="4a031-243">tooinstall hello accelerated networking driver for Linux, complete hello steps in hello [Configure Linux](#configure-linux) section of this article.</span></span>

### <span data-ttu-id="4a031-244"><a name="configure-linux"></a>Konfigurace systému Linux</span><span class="sxs-lookup"><span data-stu-id="4a031-244"><a name="configure-linux"></a>Configure Linux</span></span>

<span data-ttu-id="4a031-245">Jakmile vytvoříte hello virtuálních počítačů v Azure, je nutné nainstalovat hello Zrychlený síťové ovladače pro Linux.</span><span class="sxs-lookup"><span data-stu-id="4a031-245">Once you create hello VM in Azure, you must install hello accelerated networking driver for Linux.</span></span> <span data-ttu-id="4a031-246">Před dokončením hello následující kroky, kterou jste vytvořili virtuální počítač s Linuxem v Zrychlený sítě pomocí buď hello [portál](#linux-portal) nebo [prostředí PowerShell](#linux-powershell) kroky v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="4a031-246">Before completing hello following steps, you must have created a Linux VM with accelerated networking using either hello [portal](#linux-portal) or [PowerShell](#linux-powershell) steps in this article.</span></span> 

1. <span data-ttu-id="4a031-247">Z internetového prohlížeče, otevřete hello Azure [portál](https://portal.azure.com) a přihlaste se pomocí vaší Azure [účet](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account).</span><span class="sxs-lookup"><span data-stu-id="4a031-247">From an Internet browser, open hello Azure [portal](https://portal.azure.com) and sign in with your Azure [account](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account).</span></span> <span data-ttu-id="4a031-248">Pokud účet nemáte, můžete si zaregistrovat [bezplatnou zkušební verzi](https://azure.microsoft.com/offers/ms-azr-0044p).</span><span class="sxs-lookup"><span data-stu-id="4a031-248">If you don't already have an account, you can sign up for a [free trial](https://azure.microsoft.com/offers/ms-azr-0044p).</span></span>
2. <span data-ttu-id="4a031-249">Hello horní části hello portálu toohello napravo od hello *vyhledávání prostředků* panel, klikněte na tlačítko hello **> _** ikonu toostart cloudové prostředí Bash (Preview).</span><span class="sxs-lookup"><span data-stu-id="4a031-249">At hello top of hello portal, toohello right of hello *Search resources* bar, click hello **>_** icon toostart a Bash cloud shell (Preview).</span></span> <span data-ttu-id="4a031-250">Hello Bash cloudové prostředí podokně se zobrazí v dolní části hello hello portálu a za několik sekund, uvede  **username@Azure:~ $** řádku.</span><span class="sxs-lookup"><span data-stu-id="4a031-250">hello Bash cloud shell pane appears at hello bottom of hello portal and after a few seconds, presents a **username@Azure:~$** prompt.</span></span> <span data-ttu-id="4a031-251">I když můžete SSH toohello virtuálního počítače z vašeho počítače, nikoli hello cloudové prostředí, hello pokyny v tomto kurzu se předpokládá, že používáte hello cloudové prostředí.</span><span class="sxs-lookup"><span data-stu-id="4a031-251">Though you can SSH toohello VM from your computer, rather than hello cloud shell, hello instructions in this tutorial assume you're using hello cloud shell.</span></span>
3. <span data-ttu-id="4a031-252">V horní části hello hello portálu, hello pole obsahující hello text *vyhledávání prostředků*, typ *Můjvp*.</span><span class="sxs-lookup"><span data-stu-id="4a031-252">At hello top of hello portal, in hello box that contains hello text *Search resources*, type *MyVm*.</span></span> <span data-ttu-id="4a031-253">Když **Můjvp** se zobrazí ve výsledcích hledání hello, klikněte na něj.</span><span class="sxs-lookup"><span data-stu-id="4a031-253">When **MyVm** appears in hello search results, click it.</span></span>
4. <span data-ttu-id="4a031-254">V hello **Můjvp** okno, které se zobrazí, klikněte na tlačítko hello **Connect** tlačítka na hello levém horním rohu okna hello.</span><span class="sxs-lookup"><span data-stu-id="4a031-254">In hello **MyVm** blade that appears, click hello **Connect** button in hello top left corner of hello blade.</span></span> <span data-ttu-id="4a031-255">**Poznámka:** Pokud **vytváření** je viditelný v hello **Connect** tlačítko Azure ještě nebyla dokončena vytváření hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="4a031-255">**Note:** If **Creating** is visible under hello **Connect** button, Azure has not yet finished creating hello VM.</span></span> <span data-ttu-id="4a031-256">Klikněte na tlačítko **připojit** až poté, co se již nezobrazují **vytváření** pod hello **Connect** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="4a031-256">Click **Connect** only after you no longer see **Creating** under hello **Connect** button.</span></span>
5. <span data-ttu-id="4a031-257">Azure otevře pole informující o tooenter hello `ssh adminuser@<ipaddress>`.</span><span class="sxs-lookup"><span data-stu-id="4a031-257">Azure opens a box telling you tooenter hello `ssh adminuser@<ipaddress>`.</span></span> <span data-ttu-id="4a031-258">Zadejte tento příkaz hello cloudové prostředí (nebo kopii z hello pole, která objevil v kroku 4 a vložte ji do prostředí cloudu toohello), stiskněte klávesu Enter.</span><span class="sxs-lookup"><span data-stu-id="4a031-258">Enter this command in hello cloud shell (or copy it from hello box that appeared in step 4 and paste it in toohello cloud shell), then press Enter.</span></span>
6. <span data-ttu-id="4a031-259">Zadejte **Ano** toohello otázku s dotazem, můžete podle potřeby toocontinue připojení, stiskněte klávesu Enter.</span><span class="sxs-lookup"><span data-stu-id="4a031-259">Enter **yes** toohello question asking you if you want toocontinue connecting, then press Enter.</span></span>
7. <span data-ttu-id="4a031-260">Zadejte heslo hello, kterou jste zadali při vytváření hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="4a031-260">Enter hello password you entered when creating hello VM.</span></span> <span data-ttu-id="4a031-261">Po úspěšném přihlášení toohello virtuálních počítačů, se zobrazí adminuser@MyVm:~ $ řádku.</span><span class="sxs-lookup"><span data-stu-id="4a031-261">Once successfully logged in toohello VM, you see an adminuser@MyVm:~$ prompt.</span></span> <span data-ttu-id="4a031-262">Nyní jste přihlášeni toohello virtuálních počítačů prostřednictvím relace prostředí cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="4a031-262">You are now logged in toohello VM through hello cloud shell session.</span></span> <span data-ttu-id="4a031-263">**Poznámka:** cloudové prostředí relace vyprší po 10 minutách nečinnosti.</span><span class="sxs-lookup"><span data-stu-id="4a031-263">**Note:** Cloud shell sessions time out after 10 minutes of inactivity.</span></span>

<span data-ttu-id="4a031-264">Hello pokyny v tomto okamžiku záviset na hello distribuce, který používáte.</span><span class="sxs-lookup"><span data-stu-id="4a031-264">At this point, hello instructions vary based on hello distribution you are using.</span></span> 

#### <a name="ubuntusles"></a><span data-ttu-id="4a031-265">Ubuntu/SLES</span><span class="sxs-lookup"><span data-stu-id="4a031-265">Ubuntu/SLES</span></span>

1. <span data-ttu-id="4a031-266">Zadejte na příkazovém řádku hello `uname -r` a potvrďte hello verze pro:</span><span class="sxs-lookup"><span data-stu-id="4a031-266">At hello prompt, enter `uname -r` and confirm hello version for:</span></span>

    * <span data-ttu-id="4a031-267">Ubuntu je "4.4.0-77-generic", nebo vyšší</span><span class="sxs-lookup"><span data-stu-id="4a031-267">Ubuntu is "4.4.0-77-generic," or greater</span></span>
    * <span data-ttu-id="4a031-268">SLES je "4.4.59-92.20-default" nebo vyšší</span><span class="sxs-lookup"><span data-stu-id="4a031-268">SLES is "4.4.59-92.20-default" or greater</span></span>

2. <span data-ttu-id="4a031-269">Vytvoření vazby mezi hello standardní síťového adaptéru vNIC a síťového adaptéru vNIC hello accelerated spuštěné hello příkazy, které následují.</span><span class="sxs-lookup"><span data-stu-id="4a031-269">Create a bond between hello standard networking vNIC and hello accelerated networking vNIC by running hello commands that follow.</span></span> <span data-ttu-id="4a031-270">Provoz sítě používá hello vyšší provádění Zrychlený síťového adaptéru vNIC a při hello dokumentových zajistí, že síťové přenosy proces se nepřerušil napříč změny konfigurace.</span><span class="sxs-lookup"><span data-stu-id="4a031-270">Network traffic uses hello higher performing accelerated networking vNIC, while hello bond ensures that networking traffic is not interrupted across certain configuration changes.</span></span>
          
     ```bash
     wget https://raw.githubusercontent.com/LIS/lis-next/master/tools/sriov/configure_hv_sriov.sh
     chmod +x ./configure_hv_sriov.sh
     sudo ./configure_hv_sriov.sh
     ```
3. <span data-ttu-id="4a031-271">Po spouštění skriptu hello hello virtuální počítač restartuje po pozastavení 60 sekund.</span><span class="sxs-lookup"><span data-stu-id="4a031-271">After running hello script, hello VM will restart after a 60 second pause.</span></span>
4. <span data-ttu-id="4a031-272">Jednou hello restartování virtuálního počítače znovu tooit podle kroků 5 až 7 znovu.</span><span class="sxs-lookup"><span data-stu-id="4a031-272">Once hello VM is restarted, reconnect tooit by completing steps 5-7 again.</span></span>
5. <span data-ttu-id="4a031-273">Spustit hello `ifconfig` příkazů a potvrdit, že bond0 zase a hello rozhraní se zobrazuje jako nahoru.</span><span class="sxs-lookup"><span data-stu-id="4a031-273">Run hello `ifconfig` command and confirm that bond0 has come up and hello interface is showing as UP.</span></span> 
 
 >[!NOTE]
      ><span data-ttu-id="4a031-274">Aplikace, které používají Zrychlený sítě musí komunikovat přes hello *bond0* rozhraní není *eth0*.</span><span class="sxs-lookup"><span data-stu-id="4a031-274">Applications using accelerated networking must communicate over hello *bond0* interface, not *eth0*.</span></span>  <span data-ttu-id="4a031-275">Název rozhraní Hello může změnit než Zrychlený sítě dosáhne obecné dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="4a031-275">hello interface name may change before accelerated networking reaches general availability.</span></span>

#### <a name="rhelcentos"></a><span data-ttu-id="4a031-276">RHEL nebo CentOS</span><span class="sxs-lookup"><span data-stu-id="4a031-276">RHEL/CentOS</span></span>

<span data-ttu-id="4a031-277">Vytváření Red Hat Enterprise Linux nebo CentOS 7.3 virtuálního počítače vyžaduje některé další kroky tooload hello nejnovější ovladače potřebné pro rozhraní SR-IOV a hello ovladač virtuální funkce (VF) pro hello síťové karty.</span><span class="sxs-lookup"><span data-stu-id="4a031-277">Creating a Red Hat Enterprise Linux or CentOS 7.3 VM requires some extra steps tooload hello latest drivers needed for SR-IOV and hello Virtual Function (VF) driver for hello network card.</span></span> <span data-ttu-id="4a031-278">Hello první fázi hello pokyny připraví obrázek, který lze použít toomake jeden nebo více virtuálních počítačů, které se mají ovladače hello předem načtená.</span><span class="sxs-lookup"><span data-stu-id="4a031-278">hello first phase of hello instructions prepares an image that can be used toomake one or more virtual machines that have hello drivers pre-loaded.</span></span>

##### <a name="phase-one-prepare-a-red-hat-enterprise-linux-or-centos-73-base-image"></a><span data-ttu-id="4a031-279">Fáze 1: Příprava Red Hat Enterprise Linux nebo CentOS 7.3 základní image.</span><span class="sxs-lookup"><span data-stu-id="4a031-279">Phase one: prepare a Red Hat Enterprise Linux or CentOS 7.3 base image.</span></span> 

1.  <span data-ttu-id="4a031-280">Zřídit jiný - SR-IOV CentOS 7.3 virtuálního počítače v Azure</span><span class="sxs-lookup"><span data-stu-id="4a031-280">Provision a non-SRIOV CentOS 7.3 VM on Azure</span></span>

2.  <span data-ttu-id="4a031-281">Instalovat 4.2.2:</span><span class="sxs-lookup"><span data-stu-id="4a031-281">Install LIS 4.2.2:</span></span>
    
    ```bash
    wget http://download.microsoft.com/download/6/8/F/68FE11B8-FAA4-4F8D-8C7D-74DA7F2CFC8C/lis-rpms-4.2.2-2.tar.gz
    tar -xvf lis-rpms-4.2.2-2.tar.gz
    cd LISISO && sudo ./install.sh
    ```

3.  <span data-ttu-id="4a031-282">Stáhnout konfigurační soubory</span><span class="sxs-lookup"><span data-stu-id="4a031-282">Download config files</span></span>
    
    ```bash
    cd /etc/udev/rules.d/  
    sudo wget https://raw.githubusercontent.com/LIS/lis-next/master/tools/sriov/60-hyperv-vf-name.rules 
    cd /usr/sbin/
    sudo wget https://raw.githubusercontent.com/LIS/lis-next/master/tools/sriov/hv_vf_name 
    sudo chmod +x hv_vf_name
    cd /etc/sysconfig/network-scripts/
    sudo wget https://raw.githubusercontent.com/LIS/lis-next/master/tools/sriov/ifcfg-vf1   
    ```

4.  <span data-ttu-id="4a031-283">Zrušení zřízení tohoto virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="4a031-283">Deprovision this VM</span></span>

    ```bash
    sudo waagent -deprovision+user 
    ```

5.  <span data-ttu-id="4a031-284">Z portálu Azure Zastavit tento virtuální počítač; a přejděte na tooVM "disky", zaznamenat hello OSDisk URI virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="4a031-284">From Azure portal, stop this VM; and go tooVM’s "Disks", capture hello OSDisk’s VHD URI.</span></span> <span data-ttu-id="4a031-285">Tento identifikátor URI obsahuje název hello základní image virtuálního pevného disku a jeho účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="4a031-285">This URI contains hello base image’s VHD name and its storage account.</span></span> 
 
##### <a name="phase-two-provision-new-vms-on-azure"></a><span data-ttu-id="4a031-286">Fáze 2: zřizovat nové virtuální počítače v Azure</span><span class="sxs-lookup"><span data-stu-id="4a031-286">Phase two: Provision new VMs on Azure</span></span>

1.  <span data-ttu-id="4a031-287">Zřídit nové virtuální počítače na základě pomocí New-AzureRMVMConfig pomocí hello základní image virtuálního pevného disku zachytit v první fázi, pomocí AcceleratedNetworking povolit na kartě vNIC hello:</span><span class="sxs-lookup"><span data-stu-id="4a031-287">Provision new VMs based with New-AzureRMVMConfig using hello base image VHD captured in phase one, with AcceleratedNetworking enabled on hello vNIC:</span></span>

    ```powershell
    $RgName="MyResourceGroup"
    $Location="westus2"
    
    # Create a resource group
    New-AzureRmResourceGroup `
     -Name $RgName `
     -Location $Location

    # Create a subnet
    $Subnet = New-AzureRmVirtualNetworkSubnetConfig `
     -Name MySubnet `
     -AddressPrefix 10.0.0.0/24

    # Create a virtual network
    $Vnet=New-AzureRmVirtualNetwork `
     -ResourceGroupName $RgName `
     -Location $Location `
     -Name MyVnet `
     -AddressPrefix 10.0.0.0/16 `
     -Subnet $Subnet
    
    # Create a public IP address
    $Pip = New-AzureRmPublicIpAddress `
     -Name MyPublicIp `
     -ResourceGroupName $RgName `
     -Location $Location `
     -AllocationMethod Static
    
    # Create a virtual network interface and associate hello public IP address tooit
    $Nic = New-AzureRmNetworkInterface `
     -Name MyNic `
     -ResourceGroupName $RgName `
     -Location $Location `
     -SubnetId $Vnet.Subnets[0].Id `
     -PublicIpAddressId $Pip.Id `
     -EnableAcceleratedNetworking
    
    # Specify hello base image's VHD URI (from phase one step 5). 
    # Note: hello storage account of this base image vhd should have "Storage service encryption" disabled
    # See more from here: https://docs.microsoft.com/en-us/azure/storage/storage-service-encryption
    # This is just an example URI, you will need tooreplace this when running this script
    $sourceUri="https://myexamplesa.blob.core.windows.net/vhds/CentOS73-Base-Test120170629111341.vhd" 

    # Specify a URI for hello location from which hello new image binary large object (BLOB) is copied toostart hello virtual machine. 
    # Must end with ".vhd" extension
    $OsDiskName = "MyOsDiskName.vhd" 
    $destOsDiskUri = "https://myexamplesa.blob.core.windows.net/vhds/" + $OsDiskName
    
    # Define a credential object for hello VM. PowerShell prompts you for a username and password.
    $Cred = Get-Credential
    
    # Create a custom virtual machine configuration
    $VmConfig = New-AzureRmVMConfig `
     -VMName MyVM -VMSize Standard_DS4_v2 | `
    Set-AzureRmVMOperatingSystem `
     -Linux `
     -ComputerName myVM `
     -Credential $Cred | `
    Add-AzureRmVMNetworkInterface -Id $Nic.Id | `
    Set-AzureRmVMOSDisk `
     -Name $OsDiskName `
     -SourceImageUri $sourceUri `
     -VhdUri $destOsDiskUri `
     -CreateOption FromImage `
     -Linux
    
    # Create hello virtual machine.    
    New-AzureRmVM `
     -ResourceGroupName $RgName `
     -Location $Location `
     -VM $VmConfig
    ```

2.  <span data-ttu-id="4a031-288">Po spustit virtuální počítače, ověřte hello VF zařízení podle "lspci" a zkontrolujte položku Mellanox hello.</span><span class="sxs-lookup"><span data-stu-id="4a031-288">After VMs boot up, check hello VF device by "lspci" and check hello Mellanox entry.</span></span> <span data-ttu-id="4a031-289">Například bychom měli vidět této položky ve výstupu lspci hello:</span><span class="sxs-lookup"><span data-stu-id="4a031-289">For example, we should see this item in hello lspci output:</span></span>
    
    ```
    0001:00:02.0 Ethernet controller: Mellanox Technologies MT27500/MT27520 Family [ConnectX-3/ConnectX-3 Pro Virtual Function]
    ```
    
3.  <span data-ttu-id="4a031-290">Spusťte skript záruční hello:</span><span class="sxs-lookup"><span data-stu-id="4a031-290">Run hello bonding script by:</span></span>

    ```bash
    sudo bondvf.sh
    ```

4.  <span data-ttu-id="4a031-291">Restartovat hello nové virtuální počítače:</span><span class="sxs-lookup"><span data-stu-id="4a031-291">Reboot hello new VMs:</span></span>

    ```bash
    sudo reboot
    ```

<span data-ttu-id="4a031-292">Hello virtuálního počítače by měla začínat znakem bond0 nakonfigurované a hello Accelerated sítě cesty povolené.</span><span class="sxs-lookup"><span data-stu-id="4a031-292">hello VM should start with bond0 configured and hello Accelerated Networking path enabled.</span></span>  <span data-ttu-id="4a031-293">Spustit `ifconfig` tooconfirm.</span><span class="sxs-lookup"><span data-stu-id="4a031-293">Run `ifconfig` tooconfirm.</span></span>
