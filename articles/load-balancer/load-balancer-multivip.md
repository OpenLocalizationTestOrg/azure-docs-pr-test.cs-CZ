---
title: "aaaMutiple virtuální IP adresy pro cloudové služby"
description: "Přehled o víc virtuálními IP adresami a jak tooset několika virtuálními IP adresami v cloudové službě"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
ms.assetid: 85f6d26a-3df5-4b8e-96a1-92b2793b5284
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/24/2016
ms.author: kumud
ms.openlocfilehash: b3e0f2b24968cb75a7064484a09ffe94505bb70b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-multiple-vips-for-a-cloud-service"></a><span data-ttu-id="eace8-103">Konfigurace více virtuální IP adresy pro cloudové služby</span><span class="sxs-lookup"><span data-stu-id="eace8-103">Configure multiple VIPs for a cloud service</span></span>

<span data-ttu-id="eace8-104">Cloudové služby Azure můžete přistupovat přes hello veřejného Internetu pomocí IP adresy zadané v Azure.</span><span class="sxs-lookup"><span data-stu-id="eace8-104">You can access Azure cloud services over hello public Internet by using an IP address provided by Azure.</span></span> <span data-ttu-id="eace8-105">Tato veřejná IP adresa je odkazované tooas VIP (virtuální IP adresy) vzhledem k tomu, že je propojena toohello Azure nástroj pro vyrovnávání zatížení a ne hello instancí virtuálního počítače (VM) v rámci hello cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="eace8-105">This public IP address is referred tooas a VIP (virtual IP) since it is linked toohello Azure load balancer, and not hello Virtual Machine (VM) instances within hello cloud service.</span></span> <span data-ttu-id="eace8-106">Všechny instance virtuálního počítače v rámci cloudové služby můžete přistupovat pomocí jedné virtuální IP adresy.</span><span class="sxs-lookup"><span data-stu-id="eace8-106">You can access any VM instance within a cloud service by using a single VIP.</span></span>

<span data-ttu-id="eace8-107">Ale existují scénáře, ve kterých budete potřebovat víc virtuálních IP adres jako vstupní bod toohello stejné cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="eace8-107">However, there are scenarios in which you may need more than one VIP as an entry point toohello same cloud service.</span></span> <span data-ttu-id="eace8-108">Cloudové služby může například hostování více webů, který vyžaduje připojení SSL pomocí hello výchozí port 443, jako je každá lokalita hostované pro různých zákazníků, nebo klienta.</span><span class="sxs-lookup"><span data-stu-id="eace8-108">For instance, your cloud service may host multiple websites that require SSL connectivity using hello default port of 443, as each site is hosted for a different customer, or tenant.</span></span> <span data-ttu-id="eace8-109">V tomto scénáři musíte toohave jinou veřejnou přístupných IP adresu pro každý web.</span><span class="sxs-lookup"><span data-stu-id="eace8-109">In this scenario, you need toohave a different public facing IP address for each website.</span></span> <span data-ttu-id="eace8-110">Hello následující diagram ukazuje typické víceklientské hostování webů s potřeba více SSL certifikáty na hello stejný veřejný port.</span><span class="sxs-lookup"><span data-stu-id="eace8-110">hello diagram below illustrates a typical multi-tenant web hosting with a need for multiple SSL certificates on hello same public port.</span></span>

![Scénář více virtuálních IP adres SSL](./media/load-balancer-multivip/Figure1.png)

<span data-ttu-id="eace8-112">V příkladu hello výše, všechny virtuální IP adresy použijte hello stejný veřejný port (443) a provoz je přesměrovaného tooone nebo další zatížení vyrovnáváním virtuální počítače na jedinečný privátní port pro hello interní IP adresu hello cloudové služby hostování všechny weby hello.</span><span class="sxs-lookup"><span data-stu-id="eace8-112">In hello example above, all VIPs use hello same public port (443) and traffic is redirected tooone or more load balanced VMs on a unique private port for hello internal IP address of hello cloud service hosting all hello websites.</span></span>

> [!NOTE]
> <span data-ttu-id="eace8-113">Jiné situaci vyžadují hello použití hello několika virtuálními IP adresami je hostitelem více posluchače skupiny dostupnosti SQL AlwaysOn na hello stejnou sadu virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="eace8-113">Another situation requiring hello use hello multiple VIPs is hosting multiple SQL AlwaysOn availability group listeners on hello same set of Virtual Machines.</span></span>

<span data-ttu-id="eace8-114">Virtuální IP adresy jsou dynamické ve výchozím nastavení, což znamená, že v průběhu času mění hello skutečné přiřazené IP adresy toohello cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="eace8-114">VIPs are dynamic by default, which means that hello actual IP address assigned toohello cloud service may change over time.</span></span> <span data-ttu-id="eace8-115">tooprevent nedocházelo, můžete vyhradit VIP pro vaši službu.</span><span class="sxs-lookup"><span data-stu-id="eace8-115">tooprevent that from happening, you can reserve a VIP for your service.</span></span> <span data-ttu-id="eace8-116">toolearn Další informace o vyhrazenou virtuální IP adresy, najdete v části [vyhrazené veřejné IP adresy](../virtual-network/virtual-networks-reserved-public-ip.md).</span><span class="sxs-lookup"><span data-stu-id="eace8-116">toolearn more about reserved VIPs, see [Reserved Public IP](../virtual-network/virtual-networks-reserved-public-ip.md).</span></span>

> [!NOTE]
> <span data-ttu-id="eace8-117">Najdete v tématu [ceny IP adresu](https://azure.microsoft.com/pricing/details/ip-addresses/) informace o cenách na virtuální IP adresy a vyhrazené IP adresy.</span><span class="sxs-lookup"><span data-stu-id="eace8-117">Please see [IP Address pricing](https://azure.microsoft.com/pricing/details/ip-addresses/) for information on pricing on VIPs and reserved IPs.</span></span>

<span data-ttu-id="eace8-118">Můžete pomocí prostředí PowerShell tooverify hello virtuální IP adresy používané vaší cloudové služby, a také přidat a odebrat virtuální IP adresy, přidružit tooan koncový bod VIP a konfigurovat konkrétní VIP pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="eace8-118">You can use PowerShell tooverify hello VIPs used by your cloud services, as well as add and remove VIPs, associate a VIP tooan endpoint, and configure load balancing on a specific VIP.</span></span>

## <a name="limitations"></a><span data-ttu-id="eace8-119">Omezení</span><span class="sxs-lookup"><span data-stu-id="eace8-119">Limitations</span></span>

<span data-ttu-id="eace8-120">V tomto okamžiku funkce více virtuálních IP adres je omezený toohello následující scénáře:</span><span class="sxs-lookup"><span data-stu-id="eace8-120">At this time, Multi VIP functionality is limited toohello following scenarios:</span></span>

* <span data-ttu-id="eace8-121">**Pouze IaaS**.</span><span class="sxs-lookup"><span data-stu-id="eace8-121">**IaaS only**.</span></span> <span data-ttu-id="eace8-122">Více virtuálních IP adres můžete povolit jenom pro cloudové služby, které obsahují virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="eace8-122">You can only enable Multi VIP for cloud services that contain VMs.</span></span> <span data-ttu-id="eace8-123">Více virtuálních IP adres nelze použít ve scénářích PaaS s instancí rolí.</span><span class="sxs-lookup"><span data-stu-id="eace8-123">You cannot use Multi VIP in PaaS scenarios with role instances.</span></span>
* <span data-ttu-id="eace8-124">**Prostředí PowerShell pouze**.</span><span class="sxs-lookup"><span data-stu-id="eace8-124">**PowerShell only**.</span></span> <span data-ttu-id="eace8-125">Více virtuálních IP adres může spravovat jenom pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="eace8-125">You can only manage Multi VIP by using PowerShell.</span></span>

<span data-ttu-id="eace8-126">Tato omezení jsou dočasné a může kdykoli změnit.</span><span class="sxs-lookup"><span data-stu-id="eace8-126">These limitations are temporary, and may change at any time.</span></span> <span data-ttu-id="eace8-127">Ujistěte se, že toorevisit tuto stránku tooverify budoucí změny.</span><span class="sxs-lookup"><span data-stu-id="eace8-127">Make sure toorevisit this page tooverify future changes.</span></span>

## <a name="how-tooadd-a-vip-tooa-cloud-service"></a><span data-ttu-id="eace8-128">Jak tooadd VIP tooa cloudové služby</span><span class="sxs-lookup"><span data-stu-id="eace8-128">How tooadd a VIP tooa cloud service</span></span>
<span data-ttu-id="eace8-129">Služba tooyour tooadd VIP, spusťte následující příkaz prostředí PowerShell hello:</span><span class="sxs-lookup"><span data-stu-id="eace8-129">tooadd a VIP tooyour service, run hello following PowerShell command:</span></span>

```powershell
Add-AzureVirtualIP -VirtualIPName Vip3 -ServiceName myService
```

<span data-ttu-id="eace8-130">Tento příkaz zobrazí výsledek podobné toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="eace8-130">This command displays a result similar toohello following sample:</span></span>

    OperationDescription OperationId                          OperationStatus
    -------------------- -----------                          ---------------
    Add-AzureVirtualIP   4bd7b638-d2e7-216f-ba38-5221233d70ce Succeeded

## <a name="how-tooremove-a-vip-from-a-cloud-service"></a><span data-ttu-id="eace8-131">Jak tooremove virtuální IP adresu z cloudové služby</span><span class="sxs-lookup"><span data-stu-id="eace8-131">How tooremove a VIP from a cloud service</span></span>
<span data-ttu-id="eace8-132">tooremove hello VIP přidána tooyour služby v příkladu hello nad spuštění hello následující příkaz prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="eace8-132">tooremove hello VIP added tooyour service in hello example above, run hello following PowerShell command:</span></span>

```powershell
Remove-AzureVirtualIP -VirtualIPName Vip3 -ServiceName myService
```

> [!IMPORTANT]
> <span data-ttu-id="eace8-133">Virtuální IP adresu lze odebrat pouze pokud má žádné tooit přiřazeny koncové body.</span><span class="sxs-lookup"><span data-stu-id="eace8-133">You can only remove a VIP if it has no endpoints associated tooit.</span></span>


## <a name="how-tooretrieve-vip-information-from-a-cloud-service"></a><span data-ttu-id="eace8-134">Jak tooretrieve VIP informace z cloudové služby</span><span class="sxs-lookup"><span data-stu-id="eace8-134">How tooretrieve VIP information from a Cloud Service</span></span>
<span data-ttu-id="eace8-135">tooretrieve hello VIP spojené s cloudovou službou, spusťte následující skript prostředí PowerShell hello:</span><span class="sxs-lookup"><span data-stu-id="eace8-135">tooretrieve hello VIPs associated with a cloud service, run hello following PowerShell script:</span></span>

```powershell
$deployment = Get-AzureDeployment -ServiceName myService
$deployment.VirtualIPs
```

<span data-ttu-id="eace8-136">skript Hello zobrazí výsledek podobné toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="eace8-136">hello script displays a result similar toohello following sample:</span></span>

    Address         : 191.238.74.148
    IsDnsProgrammed : True
    Name            : Vip1
    ReservedIPName  :
    ExtensionData   :

    Address         :
    IsDnsProgrammed :
    Name            : Vip2
    ReservedIPName  :
    ExtensionData   :

    Address         :
    IsDnsProgrammed :
    Name            : Vip3
    ReservedIPName  :
    ExtensionData   :

<span data-ttu-id="eace8-137">V tomto příkladu hello cloudové služby má 3 virtuální IP adresy:</span><span class="sxs-lookup"><span data-stu-id="eace8-137">In this example, hello cloud service has 3 VIPs:</span></span>

* <span data-ttu-id="eace8-138">**Vip1** je hello výchozí VIP, víte, že vzhledem k tomu, že hodnota hello IsDnsProgrammedName nastavena tootrue.</span><span class="sxs-lookup"><span data-stu-id="eace8-138">**Vip1** is hello default VIP, you know that because hello value for IsDnsProgrammedName is set tootrue.</span></span>
* <span data-ttu-id="eace8-139">**Vip2** a **Vip3** nejsou použity jako nemají žádné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="eace8-139">**Vip2** and **Vip3** are not used as they do not have any IP addresses.</span></span> <span data-ttu-id="eace8-140">Pouze používají Pokud přidružíte koncový bod toohello VIP.</span><span class="sxs-lookup"><span data-stu-id="eace8-140">They will only be used if you associate an endpoint toohello VIP.</span></span>

> [!NOTE]
> <span data-ttu-id="eace8-141">Vaše předplatné bude pouze vám účtována navíc VIP po jsou spojeny s koncový bod.</span><span class="sxs-lookup"><span data-stu-id="eace8-141">Your subscription will only be charged for extra VIPs once they are associated with an endpoint.</span></span> <span data-ttu-id="eace8-142">Další informace o cenách najdete v tématu [ceny IP adresu](https://azure.microsoft.com/pricing/details/ip-addresses/).</span><span class="sxs-lookup"><span data-stu-id="eace8-142">For more information on pricing, see [IP Address pricing](https://azure.microsoft.com/pricing/details/ip-addresses/).</span></span>

## <a name="how-tooassociate-a-vip-tooan-endpoint"></a><span data-ttu-id="eace8-143">Jak tooassociate VIP tooan koncový bod</span><span class="sxs-lookup"><span data-stu-id="eace8-143">How tooassociate a VIP tooan endpoint</span></span>

<span data-ttu-id="eace8-144">tooassociate virtuální IP adresu na cloudu tooan koncového bodu služby, spusťte následující příkaz prostředí PowerShell hello:</span><span class="sxs-lookup"><span data-stu-id="eace8-144">tooassociate a VIP on a cloud service tooan endpoint, run hello following PowerShell command:</span></span>

```powershell
Get-AzureVM -ServiceName myService -Name myVM1 |
    Add-AzureEndpoint -Name myEndpoint -Protocol tcp -LocalPort 8080 -PublicPort 80 -VirtualIPName Vip2 |
    Update-AzureVM
```

<span data-ttu-id="eace8-145">Hello příkaz vytvoří koncový bod názvem VIP propojené toohello *Vip2* na portu *80*a propojí ji toohello virtuálního počítače s názvem *myVM1* v rámci cloudové služby s názvem  *Moje_služba* pomocí *TCP* na portu *8080*.</span><span class="sxs-lookup"><span data-stu-id="eace8-145">hello command creates an endpoint linked toohello VIP called *Vip2* on port *80*, and links it toohello VM named *myVM1* in a cloud service named *myService* using *TCP* on port *8080*.</span></span>

<span data-ttu-id="eace8-146">tooverify hello konfigurace, spusťte následující příkaz prostředí PowerShell hello:</span><span class="sxs-lookup"><span data-stu-id="eace8-146">tooverify hello configuration, run hello following PowerShell command:</span></span>

```powershell
$deployment = Get-AzureDeployment -ServiceName myService
$deployment.VirtualIPs
```

<span data-ttu-id="eace8-147">výstup Hello vypadá podobně jako toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="eace8-147">hello output looks similar toohello following example:</span></span>

    Address         : 191.238.74.148
    IsDnsProgrammed : True
    Name            : Vip1
    ReservedIPName  :
    ExtensionData   :

    Address         : 191.238.74.13
    IsDnsProgrammed :
    Name            : Vip2
    ReservedIPName  :
    ExtensionData   :

    Address         :
    IsDnsProgrammed :
    Name            : Vip3
    ReservedIPName  :
    ExtensionData   :

## <a name="how-tooenable-load-balancing-on-a-specific-vip"></a><span data-ttu-id="eace8-148">Jak načíst tooenable konkrétní VIP pro vyrovnávání</span><span class="sxs-lookup"><span data-stu-id="eace8-148">How tooenable load balancing on a specific VIP</span></span>

<span data-ttu-id="eace8-149">Více virtuálních počítačů pro účely Vyrovnávání zatížení je možné přidružit jednu VIP.</span><span class="sxs-lookup"><span data-stu-id="eace8-149">You can associate a single VIP with multiple virtual machines for load balancing purposes.</span></span> <span data-ttu-id="eace8-150">Například máte cloudové služby s názvem *Moje_služba*a dva virtuální počítače s názvem *myVM1* a *Můjvp2*.</span><span class="sxs-lookup"><span data-stu-id="eace8-150">For example, you have a cloud service named *myService*, and two virtual machines named *myVM1* and *myVM2*.</span></span> <span data-ttu-id="eace8-151">A cloudové služby má několika virtuálními IP adresami, jeden z nich s názvem *Vip2*.</span><span class="sxs-lookup"><span data-stu-id="eace8-151">And your cloud service has multiple VIPs, one of them named *Vip2*.</span></span> <span data-ttu-id="eace8-152">Pokud chcete, aby všechny přenosy dat tooport tooensure *81* na *Vip2* je rovnoměrně rozdělen mezi *myVM1* a *Můjvp2* na portu *8181* spusťte hello následující skript prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="eace8-152">If you want tooensure that all traffic tooport *81* on *Vip2* is balanced between *myVM1* and *myVM2* on port *8181*, run hello following PowerShell script:</span></span>

```powershell
Get-AzureVM -ServiceName myService -Name myVM1 |
    Add-AzureEndpoint -Name myEndpoint -LoadBalancedEndpointSetName myLBSet -Protocol tcp -LocalPort 8181 -PublicPort 81 -VirtualIPName Vip2 -DefaultProbe |
    Update-AzureVM

Get-AzureVM -ServiceName myService -Name myVM2 |
    Add-AzureEndpoint -Name myEndpoint -LoadBalancedEndpointSetName myLBSet -Protocol tcp -LocalPort 8181 -PublicPort 81 -VirtualIPName Vip2  -DefaultProbe |
    Update-AzureVM
```

<span data-ttu-id="eace8-153">Můžete také aktualizovat vaše toouse nástroje pro vyrovnávání zatížení různé VIP.</span><span class="sxs-lookup"><span data-stu-id="eace8-153">You can also update your load balancer toouse a different VIP.</span></span> <span data-ttu-id="eace8-154">Například pokud spustíte následující příkaz prostředí PowerShell text hello, změní toouse sadu s názvem Vip1 VIP pro vyrovnávání zatížení hello:</span><span class="sxs-lookup"><span data-stu-id="eace8-154">For example, if you run hello PowerShell command below, you will change hello load balancing set toouse a VIP named Vip1:</span></span>

```powershell
Set-AzureLoadBalancedEndpoint -ServiceName myService -LBSetName myLBSet -VirtualIPName Vip1
```

## <a name="next-steps"></a><span data-ttu-id="eace8-155">Další kroky</span><span class="sxs-lookup"><span data-stu-id="eace8-155">Next Steps</span></span>

[<span data-ttu-id="eace8-156">Analýzy protokolů pro vyrovnávání zatížení Azure</span><span class="sxs-lookup"><span data-stu-id="eace8-156">Log analytics for Azure Load Balance</span></span>](load-balancer-monitor-log.md)

[<span data-ttu-id="eace8-157">Přehled nástroje pro vyrovnávání zatížení přístupných Internetu</span><span class="sxs-lookup"><span data-stu-id="eace8-157">Internet facing load balancer overview</span></span>](load-balancer-internet-overview.md)

[<span data-ttu-id="eace8-158">Začněte na internetové nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="eace8-158">Get started on Internet facing load balancer</span></span>](load-balancer-get-started-internet-arm-ps.md)

[<span data-ttu-id="eace8-159">Přehled virtuálních sítí</span><span class="sxs-lookup"><span data-stu-id="eace8-159">Virtual Network Overview</span></span>](../virtual-network/virtual-networks-overview.md)

[<span data-ttu-id="eace8-160">Vyhrazená IP adresa REST API</span><span class="sxs-lookup"><span data-stu-id="eace8-160">Reserved IP REST APIs</span></span>](https://msdn.microsoft.com/library/azure/dn722420.aspx)
