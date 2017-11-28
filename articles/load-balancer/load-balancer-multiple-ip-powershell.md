---
title: "aaaLoad vyrovnávání na víc konfigurací IP adres v Azure | Microsoft Docs"
description: "Vyrovnávání zatížení napříč primární a sekundární konfigurace protokolu IP."
services: load-balancer
documentationcenter: na
author: anavinahar
manager: narayan
editor: na
ms.assetid: 244907cd-b275-4494-aaf7-dcfc4d93edfe
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/16/2017
ms.author: annahar
ms.openlocfilehash: fe1cdb317350942ff759229491c2025e98dd24a1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="load-balancing-on-multiple-ip-configurations-using-powershell"></a><span data-ttu-id="ba897-103">Víc konfigurací IP adres pomocí prostředí PowerShell pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="ba897-103">Load balancing on multiple IP configurations using PowerShell</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="ba897-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="ba897-104">Portal</span></span>](load-balancer-multiple-ip.md)
> * [<span data-ttu-id="ba897-105">Rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="ba897-105">CLI</span></span>](load-balancer-multiple-ip-cli.md)
> * [<span data-ttu-id="ba897-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="ba897-106">PowerShell</span></span>](load-balancer-multiple-ip-powershell.md)

<span data-ttu-id="ba897-107">Tento článek popisuje, jak toouse Vyrovnávání zatížení Azure s více IP adres v sekundárním síťovém rozhraní (NIC).</span><span class="sxs-lookup"><span data-stu-id="ba897-107">This article describes how toouse Azure Load Balancer with multiple IP addresses on a secondary network interface (NIC).</span></span> <span data-ttu-id="ba897-108">V tomto scénáři máme dva virtuální počítače se systémem Windows, každý s primární a sekundární síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="ba897-108">For this scenario, we have two VMs running Windows, each with a primary and a secondary NIC.</span></span> <span data-ttu-id="ba897-109">Každý z hello sekundární síťové adaptéry mají dvě konfigurace protokolu IP.</span><span class="sxs-lookup"><span data-stu-id="ba897-109">Each of hello secondary NICs have two IP configurations.</span></span> <span data-ttu-id="ba897-110">Každý virtuální počítač hostuje weby contoso.com a fabrikam.com. Každý web je vázané tooone hello konfigurace protokolu IP v síťovém adaptéru hello sekundární.</span><span class="sxs-lookup"><span data-stu-id="ba897-110">Each VM hosts both websites contoso.com and fabrikam.com. Each website is bound tooone of hello IP configurations on hello secondary NIC.</span></span> <span data-ttu-id="ba897-111">Můžeme použít nástroj pro vyrovnávání zatížení Azure tooexpose dvě front-end IP adresy, jednu pro každý web, toodistribute provoz toohello odpovídající konfiguraci protokolu IP pro web hello.</span><span class="sxs-lookup"><span data-stu-id="ba897-111">We use Azure Load Balancer tooexpose two frontend IP addresses, one for each website, toodistribute traffic toohello respective IP configuration for hello website.</span></span> <span data-ttu-id="ba897-112">Tento scénář používá hello stejné číslo portu mezi frontends, jak oba back-end fondu IP adres.</span><span class="sxs-lookup"><span data-stu-id="ba897-112">This scenario uses hello same port number across both frontends, as well as both backend pool IP addresses.</span></span>

![Scénář LB bitové kopie](./media/load-balancer-multiple-ip/lb-multi-ip.PNG)

## <a name="steps-tooload-balance-on-multiple-ip-configurations"></a><span data-ttu-id="ba897-114">Vyrovnávat tooload kroky na víc konfigurací IP adres</span><span class="sxs-lookup"><span data-stu-id="ba897-114">Steps tooload balance on multiple IP configurations</span></span>

<span data-ttu-id="ba897-115">Postupujte podle kroků hello tooachieve hello scénář popsaný v tomto článku:</span><span class="sxs-lookup"><span data-stu-id="ba897-115">Follow hello steps below tooachieve hello scenario outlined in this article:</span></span>

1. <span data-ttu-id="ba897-116">Nainstalujte Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ba897-116">Install Azure PowerShell.</span></span> <span data-ttu-id="ba897-117">V tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) informace o instalaci hello nejnovější verzi prostředí Azure PowerShell, výběr předplatného a přihlášení tooyour účtu.</span><span class="sxs-lookup"><span data-stu-id="ba897-117">See [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) for information about installing hello latest version of Azure PowerShell, selecting your subscription, and signing in tooyour account.</span></span>
2. <span data-ttu-id="ba897-118">Vytvoření skupiny prostředků pomocí hello následující nastavení:</span><span class="sxs-lookup"><span data-stu-id="ba897-118">Create a resource group using hello following settings:</span></span>

    ```powershell
    $location = "westcentralus".
    $myResourceGroup = "contosofabrikam"
    ```

    <span data-ttu-id="ba897-119">Další informace najdete v části Krok 2 [vytvořte skupinu prostředků](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ba897-119">For more information, see Step 2 of [Create a Resource Group](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json).</span></span>

3. <span data-ttu-id="ba897-120">[Vytvoření skupiny dostupnosti](../virtual-machines/windows/tutorial-availability-sets.md?toc=%2fazure%2fload-balancer%2ftoc.json) toocontain virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="ba897-120">[Create an Availability Set](../virtual-machines/windows/tutorial-availability-sets.md?toc=%2fazure%2fload-balancer%2ftoc.json) toocontain your VMs.</span></span> <span data-ttu-id="ba897-121">V tomto scénáři použijte hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="ba897-121">For this scenario, use hello following command:</span></span>

    ```powershell
    New-AzureRmAvailabilitySet -ResourceGroupName "contosofabrikam" -Name "myAvailset" -Location "West Central US"
    ```

4. <span data-ttu-id="ba897-122">Postupujte podle pokynů kroky 3 až 5 v [vytvoření virtuálního počítače s Windows](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json) článek tooprepare hello vytvoření virtuálního počítače s jeden síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="ba897-122">Follow instructions steps 3 through 5 in [Create a Windows VM](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json) article tooprepare hello creation of a VM with a single NIC.</span></span> <span data-ttu-id="ba897-123">Spustit krok 6.1 a použijte místo krok 6.2 hello:</span><span class="sxs-lookup"><span data-stu-id="ba897-123">Execute step 6.1, and use hello following instead of step 6.2:</span></span>

    ```powershell
    $availset = Get-AzureRmAvailabilitySet -ResourceGroupName "contosofabrikam" -Name "myAvailset"
    New-AzureRmVMConfig -VMName "VM1" -VMSize "Standard_DS1_v2" -AvailabilitySetId $availset.Id
    ```

    <span data-ttu-id="ba897-124">Dokončete [vytvoření virtuálního počítače s Windows](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json) kroky 6.3 prostřednictvím 6.8.</span><span class="sxs-lookup"><span data-stu-id="ba897-124">Then complete [Create a Windows VM](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json) steps 6.3 through 6.8.</span></span>

5. <span data-ttu-id="ba897-125">Přidejte druhý tooeach konfigurace IP z hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="ba897-125">Add a second IP configuration tooeach of hello VMs.</span></span> <span data-ttu-id="ba897-126">Postupujte podle pokynů hello v [přiřadit více IP adres počítačů toovirtual](../virtual-network/virtual-network-multiple-ip-addresses-powershell.md#add) článku.</span><span class="sxs-lookup"><span data-stu-id="ba897-126">Follow hello instructions in [Assign multiple IP addresses toovirtual machines](../virtual-network/virtual-network-multiple-ip-addresses-powershell.md#add) article.</span></span> <span data-ttu-id="ba897-127">Použijte hello následující nastavení:</span><span class="sxs-lookup"><span data-stu-id="ba897-127">Use hello following configuration settings:</span></span>

    ```powershell
    $NicName = "VM1-NIC2"
    $RgName = "contosofabrikam"
    $NicLocation = "West Central US"
    $IPConfigName4 = "VM1-ipconfig2"
    $Subnet1 = Get-AzureRmVirtualNetworkSubnetConfig -Name "mySubnet" -VirtualNetwork $myVnet
    ```

    <span data-ttu-id="ba897-128">Není nutné tooassociate hello sekundární IP konfigurace s veřejné IP adresy pro hello účelu tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="ba897-128">You do not need tooassociate hello secondary IP configurations with public IPs for hello purpose of this tutorial.</span></span> <span data-ttu-id="ba897-129">Upravte hello příkaz tooremove hello veřejné IP přidružení část.</span><span class="sxs-lookup"><span data-stu-id="ba897-129">Edit hello command tooremove hello public IP association part.</span></span>

6. <span data-ttu-id="ba897-130">Proveďte kroky 4 až 6 tohoto článku znovu pro virtuálního počítače 2.</span><span class="sxs-lookup"><span data-stu-id="ba897-130">Complete steps 4 through 6 of this article again for VM2.</span></span> <span data-ttu-id="ba897-131">Zda tooreplace hello virtuálních počítačů název tooVM2 být při této činnosti.</span><span class="sxs-lookup"><span data-stu-id="ba897-131">Be sure tooreplace hello VM name tooVM2 when doing this.</span></span> <span data-ttu-id="ba897-132">Všimněte si, že nepotřebujete toocreate virtuální sítě pro hello druhé virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="ba897-132">Note that you do not need toocreate a virtual network for hello second VM.</span></span> <span data-ttu-id="ba897-133">Může nebo nemusí vytvořit novou podsíť, na základě vaší případu použití.</span><span class="sxs-lookup"><span data-stu-id="ba897-133">You may or may not create a new subnet based on your use case.</span></span>

7. <span data-ttu-id="ba897-134">Vytvořte dvě veřejné IP adresy a uložit je do hello příslušné proměnné, jak je znázorněno:</span><span class="sxs-lookup"><span data-stu-id="ba897-134">Create two public IP addresses and store them in hello appropriate variables as shown:</span></span>

    ```powershell
    $publicIP1 = New-AzureRmPublicIpAddress -Name PublicIp1 -ResourceGroupName contosofabrikam -Location 'West Central US' -AllocationMethod Dynamic -DomainNameLabel contoso
    $publicIP2 = New-AzureRmPublicIpAddress -Name PublicIp2 -ResourceGroupName contosofabrikam -Location 'West Central US' -AllocationMethod Dynamic -DomainNameLabel fabrikam

    $publicIP1 = Get-AzureRmPublicIpAddress -Name PublicIp1 -ResourceGroupName contosofabrikam
    $publicIP2 = Get-AzureRmPublicIpAddress -Name PublicIp2 -ResourceGroupName contosofabrikam
    ```

8. <span data-ttu-id="ba897-135">Vytvořte dvě konfigurace IP front-endu:</span><span class="sxs-lookup"><span data-stu-id="ba897-135">Create two frontend IP configurations:</span></span>

    ```powershell
    $frontendIP1 = New-AzureRmLoadBalancerFrontendIpConfig -Name contosofe -PublicIpAddress $publicIP1
    $frontendIP2 = New-AzureRmLoadBalancerFrontendIpConfig -Name fabrikamfe -PublicIpAddress $publicIP2
    ```

9. <span data-ttu-id="ba897-136">Vytvoření vašeho back-endové fondy adres, sondu a pravidel Vyrovnávání zatížení:</span><span class="sxs-lookup"><span data-stu-id="ba897-136">Create your backend address pools, a probe, and your load balancing rules:</span></span>

    ```powershell
    $beaddresspool1 = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name contosopool
    $beaddresspool2 = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name fabrikampool

    $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name HTTP -RequestPath 'index.html' -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2

    $lbrule1 = New-AzureRmLoadBalancerRuleConfig -Name HTTPc -FrontendIpConfiguration $frontendIP1 -BackendAddressPool $beaddresspool1 -Probe $healthprobe -Protocol Tcp -FrontendPort 80 -BackendPort 80
    $lbrule2 = New-AzureRmLoadBalancerRuleConfig -Name HTTPf -FrontendIpConfiguration $frontendIP2 -BackendAddressPool $beaddresspool2 -Probe $healthprobe -Protocol Tcp -FrontendPort 80 -BackendPort 80
    ```

10. <span data-ttu-id="ba897-137">Až budete mít tyto prostředky vytvořit, vytvořte nástroj pro vyrovnávání zatížení:</span><span class="sxs-lookup"><span data-stu-id="ba897-137">Once you have these resources created, create your load balancer:</span></span>

    ```powershell
    $mylb = New-AzureRmLoadBalancer -ResourceGroupName contosofabrikam -Name mylb -Location 'West Central US' -FrontendIpConfiguration $frontendIP1 -LoadBalancingRule $lbrule -BackendAddressPool $beAddressPool -Probe $healthProbe
    ```

11. <span data-ttu-id="ba897-138">Přidejte hello druhý back-end adresy fondu a front-endovou IP konfigurace tooyour nově vytvořený nástroj pro vyrovnávání zatížení:</span><span class="sxs-lookup"><span data-stu-id="ba897-138">Add hello second backend address pool and frontend IP configuration tooyour newly created load balancer:</span></span>

    ```powershell
    $mylb = Get-AzureRmLoadBalancer -Name "mylb" -ResourceGroupName $myResourceGroup | Add-AzureRmLoadBalancerBackendAddressPoolConfig -Name fabrikampool | Set-AzureRmLoadBalancer

    $mylb | Add-AzureRmLoadBalancerFrontendIpConfig -Name fabrikamfe -PublicIpAddress $publicIP2 | Set-AzureRmLoadBalancer
    
    Add-AzureRmLoadBalancerRuleConfig -Name HTTP -LoadBalancer $mylb -FrontendIpConfiguration $frontendIP2 -BackendAddressPool $beaddresspool2 -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 80 | Set-AzureRmLoadBalancer
    ```

12. <span data-ttu-id="ba897-139">níže uvedené příkazy Hello získat hello síťové adaptéry a pak přidat že nástroj pro vyrovnávání zatížení obě konfigurace IP každý sekundární síťový adaptér toohello back-end fondu adres Dobrý den:</span><span class="sxs-lookup"><span data-stu-id="ba897-139">hello commands below get hello NICs and then add both IP configurations of each secondary NIC toohello backend address pool of hello load balancer:</span></span>

    ```powershell
    $nic1 = Get-AzureRmNetworkInterface -Name "VM1-NIC2" -ResourceGroupName "MyResourcegroup";
    $nic2 = Get-AzureRmNetworkInterface -Name "VM2-NIC2" -ResourceGroupName "MyResourcegroup";

    $nic1.IpConfigurations[0].LoadBalancerBackendAddressPools.Add($mylb.BackendAddressPools[0]);
    $nic1.IpConfigurations[1].LoadBalancerBackendAddressPools.Add($mylb.BackendAddressPools[1]);
    $nic2.IpConfigurations[0].LoadBalancerBackendAddressPools.Add($mylb.BackendAddressPools[0]);
    $nic2.IpConfigurations[1].LoadBalancerBackendAddressPools.Add($mylb.BackendAddressPools[1]);

    $mylb = $mylb | Set-AzureRmLoadBalancer

    $nic1 | Set-AzureRmNetworkInterface
    $nic2 | Set-AzureRmNetworkInterface
    ```

13. <span data-ttu-id="ba897-140">Nakonec musíte nakonfigurovat DNS prostředků záznamů toopoint toohello příslušných front-endovou IP adresu hello nástroj pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="ba897-140">Finally, you must configure DNS resource records toopoint toohello respective frontend IP address of hello Load Balancer.</span></span> <span data-ttu-id="ba897-141">Může hostovat vaše domény v Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="ba897-141">You may host your domains in Azure DNS.</span></span> <span data-ttu-id="ba897-142">Další informace o používání s nástrojem pro vyrovnávání zatížení Azure DNS najdete v tématu [pomocí Azure DNS s jinými službami Azure](../dns/dns-for-azure-services.md).</span><span class="sxs-lookup"><span data-stu-id="ba897-142">For more information about using Azure DNS with Load Balancer, see [Using Azure DNS with other Azure services](../dns/dns-for-azure-services.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="ba897-143">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ba897-143">Next steps</span></span>
- <span data-ttu-id="ba897-144">Další informace o tom, jak Vyrovnávání zatížení toocombine služby v Azure v [pomocí služby Vyrovnávání zatížení v Azure](../traffic-manager/traffic-manager-load-balancing-azure.md).</span><span class="sxs-lookup"><span data-stu-id="ba897-144">Learn more about how toocombine load balancing services in Azure in [Using load-balancing services in Azure](../traffic-manager/traffic-manager-load-balancing-azure.md).</span></span>
- <span data-ttu-id="ba897-145">Zjistěte, jak můžete použít různé typy protokolů v Azure toomanage a řešení vyrovnávání zatížení v [protokolu pro vyrovnávání zatížení Azure analytics](../load-balancer/load-balancer-monitor-log.md).</span><span class="sxs-lookup"><span data-stu-id="ba897-145">Learn how you can use different types of logs in Azure toomanage and troubleshoot load balancer in [Log analytics for Azure Load Balancer](../load-balancer/load-balancer-monitor-log.md).</span></span>
