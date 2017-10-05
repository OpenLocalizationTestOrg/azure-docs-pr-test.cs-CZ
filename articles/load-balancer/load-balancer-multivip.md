---
title: "Více aplikačními virtuální IP adresy pro cloudové služby"
description: "Přehled o víc virtuálními IP adresami a jak nastavit několika virtuálními IP adresami v cloudové službě"
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
ms.openlocfilehash: f40e0501eed8d5f296e7c79d8a35705a695ae6fd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configure-multiple-vips-for-a-cloud-service"></a><span data-ttu-id="2d9a2-103">Konfigurace více virtuální IP adresy pro cloudové služby</span><span class="sxs-lookup"><span data-stu-id="2d9a2-103">Configure multiple VIPs for a cloud service</span></span>

<span data-ttu-id="2d9a2-104">Cloudové služby Azure získat přístup prostřednictvím veřejného Internetu pomocí IP adresy poskytované platformou Azure.</span><span class="sxs-lookup"><span data-stu-id="2d9a2-104">You can access Azure cloud services over the public Internet by using an IP address provided by Azure.</span></span> <span data-ttu-id="2d9a2-105">Tato veřejná IP adresa se označuje jako virtuální IP adresy (virtuální IP adresy) vzhledem k tomu, že je propojená se službou Vyrovnávání zatížení Azure a není virtuální počítač (VM) instancí v rámci cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="2d9a2-105">This public IP address is referred to as a VIP (virtual IP) since it is linked to the Azure load balancer, and not the Virtual Machine (VM) instances within the cloud service.</span></span> <span data-ttu-id="2d9a2-106">Všechny instance virtuálního počítače v rámci cloudové služby můžete přistupovat pomocí jedné virtuální IP adresy.</span><span class="sxs-lookup"><span data-stu-id="2d9a2-106">You can access any VM instance within a cloud service by using a single VIP.</span></span>

<span data-ttu-id="2d9a2-107">Existují však scénáře, ve kterých může potřebovat více než jeden bod VIP jako položku do stejné cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="2d9a2-107">However, there are scenarios in which you may need more than one VIP as an entry point to the same cloud service.</span></span> <span data-ttu-id="2d9a2-108">Cloudové služby může například hostování více webů, který vyžaduje připojení SSL s použitím výchozího portu 443, jako je každá lokalita hostované pro různých zákazníků, nebo klienta.</span><span class="sxs-lookup"><span data-stu-id="2d9a2-108">For instance, your cloud service may host multiple websites that require SSL connectivity using the default port of 443, as each site is hosted for a different customer, or tenant.</span></span> <span data-ttu-id="2d9a2-109">V tomto scénáři musíte mít různé veřejných přístupných IP adresu pro každý web.</span><span class="sxs-lookup"><span data-stu-id="2d9a2-109">In this scenario, you need to have a different public facing IP address for each website.</span></span> <span data-ttu-id="2d9a2-110">Následující obrázek znázorňuje typické víceklientské webhosting s potřeba víc certifikátů SSL na stejném veřejného portu.</span><span class="sxs-lookup"><span data-stu-id="2d9a2-110">The diagram below illustrates a typical multi-tenant web hosting with a need for multiple SSL certificates on the same public port.</span></span>

![Scénář více virtuálních IP adres SSL](./media/load-balancer-multivip/Figure1.png)

<span data-ttu-id="2d9a2-112">V příkladu nahoře všechny virtuální adresy VIP používají stejný veřejný port (443) a provoz se přesměruje na jeden nebo virtuální počítače na jedinečný privátní port pro interní IP adresu cloudové služby hostování všechny weby s vyrovnáváním zatížení Další.</span><span class="sxs-lookup"><span data-stu-id="2d9a2-112">In the example above, all VIPs use the same public port (443) and traffic is redirected to one or more load balanced VMs on a unique private port for the internal IP address of the cloud service hosting all the websites.</span></span>

> [!NOTE]
> <span data-ttu-id="2d9a2-113">Jiné situaci vyžadující použití několika virtuálními IP adresami je hostitelem více SQL AlwaysOn dostupnosti naslouchací procesy skupiny na stejnou sadu virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="2d9a2-113">Another situation requiring the use the multiple VIPs is hosting multiple SQL AlwaysOn availability group listeners on the same set of Virtual Machines.</span></span>

<span data-ttu-id="2d9a2-114">Virtuální IP adresy jsou dynamické ve výchozím nastavení, což znamená, že v průběhu času mění skutečné přiřazené ke cloudové službě IP adresy.</span><span class="sxs-lookup"><span data-stu-id="2d9a2-114">VIPs are dynamic by default, which means that the actual IP address assigned to the cloud service may change over time.</span></span> <span data-ttu-id="2d9a2-115">Aby nedocházelo, můžete vyhradit VIP pro vaši službu.</span><span class="sxs-lookup"><span data-stu-id="2d9a2-115">To prevent that from happening, you can reserve a VIP for your service.</span></span> <span data-ttu-id="2d9a2-116">Další informace o vyhrazenou virtuální IP adresy, najdete v části [vyhrazené veřejné IP adresy](../virtual-network/virtual-networks-reserved-public-ip.md).</span><span class="sxs-lookup"><span data-stu-id="2d9a2-116">To learn more about reserved VIPs, see [Reserved Public IP](../virtual-network/virtual-networks-reserved-public-ip.md).</span></span>

> [!NOTE]
> <span data-ttu-id="2d9a2-117">Najdete v tématu [ceny IP adresu](https://azure.microsoft.com/pricing/details/ip-addresses/) informace o cenách na virtuální IP adresy a vyhrazené IP adresy.</span><span class="sxs-lookup"><span data-stu-id="2d9a2-117">Please see [IP Address pricing](https://azure.microsoft.com/pricing/details/ip-addresses/) for information on pricing on VIPs and reserved IPs.</span></span>

<span data-ttu-id="2d9a2-118">Můžete pomocí prostředí PowerShell k ověření virtuální IP adresy používané vaší cloudové služby, a také přidat a odebrat virtuální IP adresy, přidružit VIP na koncový bod a konfigurovat konkrétní VIP pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="2d9a2-118">You can use PowerShell to verify the VIPs used by your cloud services, as well as add and remove VIPs, associate a VIP to an endpoint, and configure load balancing on a specific VIP.</span></span>

## <a name="limitations"></a><span data-ttu-id="2d9a2-119">Omezení</span><span class="sxs-lookup"><span data-stu-id="2d9a2-119">Limitations</span></span>

<span data-ttu-id="2d9a2-120">V tomto okamžiku funkce více virtuálních IP adres je omezený na následující scénáře:</span><span class="sxs-lookup"><span data-stu-id="2d9a2-120">At this time, Multi VIP functionality is limited to the following scenarios:</span></span>

* <span data-ttu-id="2d9a2-121">**Pouze IaaS**.</span><span class="sxs-lookup"><span data-stu-id="2d9a2-121">**IaaS only**.</span></span> <span data-ttu-id="2d9a2-122">Více virtuálních IP adres můžete povolit jenom pro cloudové služby, které obsahují virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="2d9a2-122">You can only enable Multi VIP for cloud services that contain VMs.</span></span> <span data-ttu-id="2d9a2-123">Více virtuálních IP adres nelze použít ve scénářích PaaS s instancí rolí.</span><span class="sxs-lookup"><span data-stu-id="2d9a2-123">You cannot use Multi VIP in PaaS scenarios with role instances.</span></span>
* <span data-ttu-id="2d9a2-124">**Prostředí PowerShell pouze**.</span><span class="sxs-lookup"><span data-stu-id="2d9a2-124">**PowerShell only**.</span></span> <span data-ttu-id="2d9a2-125">Více virtuálních IP adres může spravovat jenom pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="2d9a2-125">You can only manage Multi VIP by using PowerShell.</span></span>

<span data-ttu-id="2d9a2-126">Tato omezení jsou dočasné a může kdykoli změnit.</span><span class="sxs-lookup"><span data-stu-id="2d9a2-126">These limitations are temporary, and may change at any time.</span></span> <span data-ttu-id="2d9a2-127">Zajistěte, aby k pokroku tuto stránku k ověření budoucí změny.</span><span class="sxs-lookup"><span data-stu-id="2d9a2-127">Make sure to revisit this page to verify future changes.</span></span>

## <a name="how-to-add-a-vip-to-a-cloud-service"></a><span data-ttu-id="2d9a2-128">Postup přidání virtuální IP adresu do cloudové služby</span><span class="sxs-lookup"><span data-stu-id="2d9a2-128">How to add a VIP to a cloud service</span></span>
<span data-ttu-id="2d9a2-129">Chcete-li přidat virtuální IP adresu k službě, spusťte následující příkaz prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="2d9a2-129">To add a VIP to your service, run the following PowerShell command:</span></span>

```powershell
Add-AzureVirtualIP -VirtualIPName Vip3 -ServiceName myService
```

<span data-ttu-id="2d9a2-130">Tento příkaz zobrazí výsledek podobně jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="2d9a2-130">This command displays a result similar to the following sample:</span></span>

    OperationDescription OperationId                          OperationStatus
    -------------------- -----------                          ---------------
    Add-AzureVirtualIP   4bd7b638-d2e7-216f-ba38-5221233d70ce Succeeded

## <a name="how-to-remove-a-vip-from-a-cloud-service"></a><span data-ttu-id="2d9a2-131">Postup odebrání virtuální IP adresu z cloudové služby</span><span class="sxs-lookup"><span data-stu-id="2d9a2-131">How to remove a VIP from a cloud service</span></span>
<span data-ttu-id="2d9a2-132">Odebrat virtuální IP adresu přidat do služby v příkladu nahoře, spusťte následující příkaz prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="2d9a2-132">To remove the VIP added to your service in the example above, run the following PowerShell command:</span></span>

```powershell
Remove-AzureVirtualIP -VirtualIPName Vip3 -ServiceName myService
```

> [!IMPORTANT]
> <span data-ttu-id="2d9a2-133">Pokud ji nemá žádné koncové body přidružené k němu můžete pouze odebrat virtuální IP adresu.</span><span class="sxs-lookup"><span data-stu-id="2d9a2-133">You can only remove a VIP if it has no endpoints associated to it.</span></span>


## <a name="how-to-retrieve-vip-information-from-a-cloud-service"></a><span data-ttu-id="2d9a2-134">Jak načíst informace o virtuálních IP adres z cloudové služby</span><span class="sxs-lookup"><span data-stu-id="2d9a2-134">How to retrieve VIP information from a Cloud Service</span></span>
<span data-ttu-id="2d9a2-135">Načtení virtuální IP adresy přidružené ke cloudové službě, spusťte následující skript prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="2d9a2-135">To retrieve the VIPs associated with a cloud service, run the following PowerShell script:</span></span>

```powershell
$deployment = Get-AzureDeployment -ServiceName myService
$deployment.VirtualIPs
```

<span data-ttu-id="2d9a2-136">Skript zobrazí výsledek podobně jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="2d9a2-136">The script displays a result similar to the following sample:</span></span>

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

<span data-ttu-id="2d9a2-137">V tomto příkladu cloudové služby má 3 virtuální IP adresy:</span><span class="sxs-lookup"><span data-stu-id="2d9a2-137">In this example, the cloud service has 3 VIPs:</span></span>

* <span data-ttu-id="2d9a2-138">**Vip1** je výchozí VIP, víte, že vzhledem k tomu, že je hodnota pro IsDnsProgrammedName nastavena na hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="2d9a2-138">**Vip1** is the default VIP, you know that because the value for IsDnsProgrammedName is set to true.</span></span>
* <span data-ttu-id="2d9a2-139">**Vip2** a **Vip3** nejsou použity jako nemají žádné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="2d9a2-139">**Vip2** and **Vip3** are not used as they do not have any IP addresses.</span></span> <span data-ttu-id="2d9a2-140">Pouze používají Pokud přidružíte koncového bodu virtuální IP adresu.</span><span class="sxs-lookup"><span data-stu-id="2d9a2-140">They will only be used if you associate an endpoint to the VIP.</span></span>

> [!NOTE]
> <span data-ttu-id="2d9a2-141">Vaše předplatné bude pouze vám účtována navíc VIP po jsou spojeny s koncový bod.</span><span class="sxs-lookup"><span data-stu-id="2d9a2-141">Your subscription will only be charged for extra VIPs once they are associated with an endpoint.</span></span> <span data-ttu-id="2d9a2-142">Další informace o cenách najdete v tématu [ceny IP adresu](https://azure.microsoft.com/pricing/details/ip-addresses/).</span><span class="sxs-lookup"><span data-stu-id="2d9a2-142">For more information on pricing, see [IP Address pricing](https://azure.microsoft.com/pricing/details/ip-addresses/).</span></span>

## <a name="how-to-associate-a-vip-to-an-endpoint"></a><span data-ttu-id="2d9a2-143">Postup přidružení VIP na koncový bod</span><span class="sxs-lookup"><span data-stu-id="2d9a2-143">How to associate a VIP to an endpoint</span></span>

<span data-ttu-id="2d9a2-144">Chcete-li přidružit VIP na cloudové služby na koncový bod, spusťte následující příkaz prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="2d9a2-144">To associate a VIP on a cloud service to an endpoint, run the following PowerShell command:</span></span>

```powershell
Get-AzureVM -ServiceName myService -Name myVM1 |
    Add-AzureEndpoint -Name myEndpoint -Protocol tcp -LocalPort 8080 -PublicPort 80 -VirtualIPName Vip2 |
    Update-AzureVM
```

<span data-ttu-id="2d9a2-145">Příkaz vytvoří koncový bod propojené s názvem VIP *Vip2* na portu *80*a odkazů na virtuální počítač s názvem *myVM1* v rámci cloudové služby s názvem *Moje_služba* pomocí *TCP* na portu *8080*.</span><span class="sxs-lookup"><span data-stu-id="2d9a2-145">The command creates an endpoint linked to the VIP called *Vip2* on port *80*, and links it to the VM named *myVM1* in a cloud service named *myService* using *TCP* on port *8080*.</span></span>

<span data-ttu-id="2d9a2-146">Pokud chcete ověřit konfiguraci, spusťte následující příkaz prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="2d9a2-146">To verify the configuration, run the following PowerShell command:</span></span>

```powershell
$deployment = Get-AzureDeployment -ServiceName myService
$deployment.VirtualIPs
```

<span data-ttu-id="2d9a2-147">Výstup bude vypadat podobně jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="2d9a2-147">The output looks similar to the following example:</span></span>

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

## <a name="how-to-enable-load-balancing-on-a-specific-vip"></a><span data-ttu-id="2d9a2-148">Postup povolení konkrétní VIP pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="2d9a2-148">How to enable load balancing on a specific VIP</span></span>

<span data-ttu-id="2d9a2-149">Více virtuálních počítačů pro účely Vyrovnávání zatížení je možné přidružit jednu VIP.</span><span class="sxs-lookup"><span data-stu-id="2d9a2-149">You can associate a single VIP with multiple virtual machines for load balancing purposes.</span></span> <span data-ttu-id="2d9a2-150">Například máte cloudové služby s názvem *Moje_služba*a dva virtuální počítače s názvem *myVM1* a *Můjvp2*.</span><span class="sxs-lookup"><span data-stu-id="2d9a2-150">For example, you have a cloud service named *myService*, and two virtual machines named *myVM1* and *myVM2*.</span></span> <span data-ttu-id="2d9a2-151">A cloudové služby má několika virtuálními IP adresami, jeden z nich s názvem *Vip2*.</span><span class="sxs-lookup"><span data-stu-id="2d9a2-151">And your cloud service has multiple VIPs, one of them named *Vip2*.</span></span> <span data-ttu-id="2d9a2-152">Pokud chcete, aby všechny přenosy na portu *81* na *Vip2* je rovnoměrně rozdělen mezi *myVM1* a *Můjvp2* na portu *8181 *, spusťte následující skript prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="2d9a2-152">If you want to ensure that all traffic to port *81* on *Vip2* is balanced between *myVM1* and *myVM2* on port *8181*, run the following PowerShell script:</span></span>

```powershell
Get-AzureVM -ServiceName myService -Name myVM1 |
    Add-AzureEndpoint -Name myEndpoint -LoadBalancedEndpointSetName myLBSet -Protocol tcp -LocalPort 8181 -PublicPort 81 -VirtualIPName Vip2 -DefaultProbe |
    Update-AzureVM

Get-AzureVM -ServiceName myService -Name myVM2 |
    Add-AzureEndpoint -Name myEndpoint -LoadBalancedEndpointSetName myLBSet -Protocol tcp -LocalPort 8181 -PublicPort 81 -VirtualIPName Vip2  -DefaultProbe |
    Update-AzureVM
```

<span data-ttu-id="2d9a2-153">Můžete také aktualizovat nástroj pro vyrovnávání zatížení použít jiný virtuální IP adresy.</span><span class="sxs-lookup"><span data-stu-id="2d9a2-153">You can also update your load balancer to use a different VIP.</span></span> <span data-ttu-id="2d9a2-154">Například pokud spustíte následující příkaz prostředí PowerShell, se změní sadu používat s názvem Vip1 VIP pro vyrovnávání zatížení:</span><span class="sxs-lookup"><span data-stu-id="2d9a2-154">For example, if you run the PowerShell command below, you will change the load balancing set to use a VIP named Vip1:</span></span>

```powershell
Set-AzureLoadBalancedEndpoint -ServiceName myService -LBSetName myLBSet -VirtualIPName Vip1
```

## <a name="next-steps"></a><span data-ttu-id="2d9a2-155">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2d9a2-155">Next Steps</span></span>

[<span data-ttu-id="2d9a2-156">Analýzy protokolů pro vyrovnávání zatížení Azure</span><span class="sxs-lookup"><span data-stu-id="2d9a2-156">Log analytics for Azure Load Balance</span></span>](load-balancer-monitor-log.md)

[<span data-ttu-id="2d9a2-157">Přehled nástroje pro vyrovnávání zatížení přístupných Internetu</span><span class="sxs-lookup"><span data-stu-id="2d9a2-157">Internet facing load balancer overview</span></span>](load-balancer-internet-overview.md)

[<span data-ttu-id="2d9a2-158">Začněte na internetové nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="2d9a2-158">Get started on Internet facing load balancer</span></span>](load-balancer-get-started-internet-arm-ps.md)

[<span data-ttu-id="2d9a2-159">Přehled virtuálních sítí</span><span class="sxs-lookup"><span data-stu-id="2d9a2-159">Virtual Network Overview</span></span>](../virtual-network/virtual-networks-overview.md)

[<span data-ttu-id="2d9a2-160">Vyhrazená IP adresa REST API</span><span class="sxs-lookup"><span data-stu-id="2d9a2-160">Reserved IP REST APIs</span></span>](https://msdn.microsoft.com/library/azure/dn722420.aspx)
