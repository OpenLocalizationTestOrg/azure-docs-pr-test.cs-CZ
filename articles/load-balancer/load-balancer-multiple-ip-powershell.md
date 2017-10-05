---
title: "Víc konfigurací IP adres v Azure pro vyrovnávání zatížení | Microsoft Docs"
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
ms.openlocfilehash: a8550519f094ca7afcd868a14b313337627f97d3
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="load-balancing-on-multiple-ip-configurations-using-powershell"></a><span data-ttu-id="51cee-103">Víc konfigurací IP adres pomocí prostředí PowerShell pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="51cee-103">Load balancing on multiple IP configurations using PowerShell</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="51cee-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="51cee-104">Portal</span></span>](load-balancer-multiple-ip.md)
> * [<span data-ttu-id="51cee-105">Rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="51cee-105">CLI</span></span>](load-balancer-multiple-ip-cli.md)
> * [<span data-ttu-id="51cee-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="51cee-106">PowerShell</span></span>](load-balancer-multiple-ip-powershell.md)

<span data-ttu-id="51cee-107">Tento článek popisuje, jak používat nástroj pro vyrovnávání zatížení Azure s více IP adres v sekundárním síťovém rozhraní (NIC).</span><span class="sxs-lookup"><span data-stu-id="51cee-107">This article describes how to use Azure Load Balancer with multiple IP addresses on a secondary network interface (NIC).</span></span> <span data-ttu-id="51cee-108">V tomto scénáři máme dva virtuální počítače se systémem Windows, každý s primární a sekundární síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="51cee-108">For this scenario, we have two VMs running Windows, each with a primary and a secondary NIC.</span></span> <span data-ttu-id="51cee-109">Každé sekundární síťové karty má dvě konfigurace protokolu IP.</span><span class="sxs-lookup"><span data-stu-id="51cee-109">Each of the secondary NICs have two IP configurations.</span></span> <span data-ttu-id="51cee-110">Každý virtuální počítač hostuje weby contoso.com a fabrikam.com. Každý web je vázána na jednu z konfigurace protokolu IP v sekundárním síťovém adaptéru.</span><span class="sxs-lookup"><span data-stu-id="51cee-110">Each VM hosts both websites contoso.com and fabrikam.com. Each website is bound to one of the IP configurations on the secondary NIC.</span></span> <span data-ttu-id="51cee-111">Nástroj pro vyrovnávání zatížení Azure používáme ke zveřejnění dvě front-end IP adresy, jednu pro každý web, distribuovat provoz do příslušných konfiguraci protokolu IP pro web.</span><span class="sxs-lookup"><span data-stu-id="51cee-111">We use Azure Load Balancer to expose two frontend IP addresses, one for each website, to distribute traffic to the respective IP configuration for the website.</span></span> <span data-ttu-id="51cee-112">Tento scénář používá stejné číslo portu mezi frontends, jak oba back-end fondu IP adres.</span><span class="sxs-lookup"><span data-stu-id="51cee-112">This scenario uses the same port number across both frontends, as well as both backend pool IP addresses.</span></span>

![Scénář LB bitové kopie](./media/load-balancer-multiple-ip/lb-multi-ip.PNG)

## <a name="steps-to-load-balance-on-multiple-ip-configurations"></a><span data-ttu-id="51cee-114">Postup na víc konfigurací IP adres Vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="51cee-114">Steps to load balance on multiple IP configurations</span></span>

<span data-ttu-id="51cee-115">Použijte následující postup k dosažení scénáři uvedeném v tomto článku:</span><span class="sxs-lookup"><span data-stu-id="51cee-115">Follow the steps below to achieve the scenario outlined in this article:</span></span>

1. <span data-ttu-id="51cee-116">Nainstalujte Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="51cee-116">Install Azure PowerShell.</span></span> <span data-ttu-id="51cee-117">Projděte si článek [Jak nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azure/overview), kde najdete informace o instalaci nejnovější verze prostředí Azure PowerShell, výběru předplatného a přihlášení k účtu.</span><span class="sxs-lookup"><span data-stu-id="51cee-117">See [How to install and configure Azure PowerShell](/powershell/azure/overview) for information about installing the latest version of Azure PowerShell, selecting your subscription, and signing in to your account.</span></span>
2. <span data-ttu-id="51cee-118">Vytvořte skupinu prostředků s následujícím nastavením:</span><span class="sxs-lookup"><span data-stu-id="51cee-118">Create a resource group using the following settings:</span></span>

    ```powershell
    $location = "westcentralus".
    $myResourceGroup = "contosofabrikam"
    ```

    <span data-ttu-id="51cee-119">Další informace najdete v části Krok 2 [vytvořte skupinu prostředků](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="51cee-119">For more information, see Step 2 of [Create a Resource Group](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json).</span></span>

3. <span data-ttu-id="51cee-120">[Vytvoření skupiny dostupnosti](../virtual-machines/windows/tutorial-availability-sets.md?toc=%2fazure%2fload-balancer%2ftoc.json) tak, aby obsahovala vaše virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="51cee-120">[Create an Availability Set](../virtual-machines/windows/tutorial-availability-sets.md?toc=%2fazure%2fload-balancer%2ftoc.json) to contain your VMs.</span></span> <span data-ttu-id="51cee-121">V tomto scénáři použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="51cee-121">For this scenario, use the following command:</span></span>

    ```powershell
    New-AzureRmAvailabilitySet -ResourceGroupName "contosofabrikam" -Name "myAvailset" -Location "West Central US"
    ```

4. <span data-ttu-id="51cee-122">Postupujte podle pokynů kroky 3 až 5 v [vytvoření virtuálního počítače s Windows](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json) článku Příprava vytvoření virtuálního počítače s jeden síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="51cee-122">Follow instructions steps 3 through 5 in [Create a Windows VM](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json) article to prepare the creation of a VM with a single NIC.</span></span> <span data-ttu-id="51cee-123">Spustit krok 6.1 a místo krok 6.2 použít následující:</span><span class="sxs-lookup"><span data-stu-id="51cee-123">Execute step 6.1, and use the following instead of step 6.2:</span></span>

    ```powershell
    $availset = Get-AzureRmAvailabilitySet -ResourceGroupName "contosofabrikam" -Name "myAvailset"
    New-AzureRmVMConfig -VMName "VM1" -VMSize "Standard_DS1_v2" -AvailabilitySetId $availset.Id
    ```

    <span data-ttu-id="51cee-124">Dokončete [vytvoření virtuálního počítače s Windows](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json) kroky 6.3 prostřednictvím 6.8.</span><span class="sxs-lookup"><span data-stu-id="51cee-124">Then complete [Create a Windows VM](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json) steps 6.3 through 6.8.</span></span>

5. <span data-ttu-id="51cee-125">Přidejte druhý konfigurace protokolu IP do všech virtuálních počítačích.</span><span class="sxs-lookup"><span data-stu-id="51cee-125">Add a second IP configuration to each of the VMs.</span></span> <span data-ttu-id="51cee-126">Postupujte podle pokynů v [přiřadit více IP adres k virtuálním počítačům](../virtual-network/virtual-network-multiple-ip-addresses-powershell.md#add) článku.</span><span class="sxs-lookup"><span data-stu-id="51cee-126">Follow the instructions in [Assign multiple IP addresses to virtual machines](../virtual-network/virtual-network-multiple-ip-addresses-powershell.md#add) article.</span></span> <span data-ttu-id="51cee-127">Použijte následující nastavení:</span><span class="sxs-lookup"><span data-stu-id="51cee-127">Use the following configuration settings:</span></span>

    ```powershell
    $NicName = "VM1-NIC2"
    $RgName = "contosofabrikam"
    $NicLocation = "West Central US"
    $IPConfigName4 = "VM1-ipconfig2"
    $Subnet1 = Get-AzureRmVirtualNetworkSubnetConfig -Name "mySubnet" -VirtualNetwork $myVnet
    ```

    <span data-ttu-id="51cee-128">Není nutné přidružit sekundární konfigurace IP veřejné IP adresy pro účely tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="51cee-128">You do not need to associate the secondary IP configurations with public IPs for the purpose of this tutorial.</span></span> <span data-ttu-id="51cee-129">Upravte příkaz odebrat část přidružení veřejné IP.</span><span class="sxs-lookup"><span data-stu-id="51cee-129">Edit the command to remove the public IP association part.</span></span>

6. <span data-ttu-id="51cee-130">Proveďte kroky 4 až 6 tohoto článku znovu pro virtuálního počítače 2.</span><span class="sxs-lookup"><span data-stu-id="51cee-130">Complete steps 4 through 6 of this article again for VM2.</span></span> <span data-ttu-id="51cee-131">Nezapomeňte nahradit název virtuálního počítače do virtuálního počítače 2 při této činnosti.</span><span class="sxs-lookup"><span data-stu-id="51cee-131">Be sure to replace the VM name to VM2 when doing this.</span></span> <span data-ttu-id="51cee-132">Všimněte si, že není potřeba vytvořit virtuální síť pro druhý virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="51cee-132">Note that you do not need to create a virtual network for the second VM.</span></span> <span data-ttu-id="51cee-133">Může nebo nemusí vytvořit novou podsíť, na základě vaší případu použití.</span><span class="sxs-lookup"><span data-stu-id="51cee-133">You may or may not create a new subnet based on your use case.</span></span>

7. <span data-ttu-id="51cee-134">Vytvořte dvě veřejné IP adresy a uložit je do příslušné proměnné, jak je znázorněno:</span><span class="sxs-lookup"><span data-stu-id="51cee-134">Create two public IP addresses and store them in the appropriate variables as shown:</span></span>

    ```powershell
    $publicIP1 = New-AzureRmPublicIpAddress -Name PublicIp1 -ResourceGroupName contosofabrikam -Location 'West Central US' -AllocationMethod Dynamic -DomainNameLabel contoso
    $publicIP2 = New-AzureRmPublicIpAddress -Name PublicIp2 -ResourceGroupName contosofabrikam -Location 'West Central US' -AllocationMethod Dynamic -DomainNameLabel fabrikam

    $publicIP1 = Get-AzureRmPublicIpAddress -Name PublicIp1 -ResourceGroupName contosofabrikam
    $publicIP2 = Get-AzureRmPublicIpAddress -Name PublicIp2 -ResourceGroupName contosofabrikam
    ```

8. <span data-ttu-id="51cee-135">Vytvořte dvě konfigurace IP front-endu:</span><span class="sxs-lookup"><span data-stu-id="51cee-135">Create two frontend IP configurations:</span></span>

    ```powershell
    $frontendIP1 = New-AzureRmLoadBalancerFrontendIpConfig -Name contosofe -PublicIpAddress $publicIP1
    $frontendIP2 = New-AzureRmLoadBalancerFrontendIpConfig -Name fabrikamfe -PublicIpAddress $publicIP2
    ```

9. <span data-ttu-id="51cee-136">Vytvoření vašeho back-endové fondy adres, sondu a pravidel Vyrovnávání zatížení:</span><span class="sxs-lookup"><span data-stu-id="51cee-136">Create your backend address pools, a probe, and your load balancing rules:</span></span>

    ```powershell
    $beaddresspool1 = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name contosopool
    $beaddresspool2 = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name fabrikampool

    $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name HTTP -RequestPath 'index.html' -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2

    $lbrule1 = New-AzureRmLoadBalancerRuleConfig -Name HTTPc -FrontendIpConfiguration $frontendIP1 -BackendAddressPool $beaddresspool1 -Probe $healthprobe -Protocol Tcp -FrontendPort 80 -BackendPort 80
    $lbrule2 = New-AzureRmLoadBalancerRuleConfig -Name HTTPf -FrontendIpConfiguration $frontendIP2 -BackendAddressPool $beaddresspool2 -Probe $healthprobe -Protocol Tcp -FrontendPort 80 -BackendPort 80
    ```

10. <span data-ttu-id="51cee-137">Až budete mít tyto prostředky vytvořit, vytvořte nástroj pro vyrovnávání zatížení:</span><span class="sxs-lookup"><span data-stu-id="51cee-137">Once you have these resources created, create your load balancer:</span></span>

    ```powershell
    $mylb = New-AzureRmLoadBalancer -ResourceGroupName contosofabrikam -Name mylb -Location 'West Central US' -FrontendIpConfiguration $frontendIP1 -LoadBalancingRule $lbrule -BackendAddressPool $beAddressPool -Probe $healthProbe
    ```

11. <span data-ttu-id="51cee-138">Nástroj pro vyrovnávání zatížení nově vytvořený přidejte druhý back-end adresy fondu a front-endové konfiguraci protokolu IP:</span><span class="sxs-lookup"><span data-stu-id="51cee-138">Add the second backend address pool and frontend IP configuration to your newly created load balancer:</span></span>

    ```powershell
    $mylb = Get-AzureRmLoadBalancer -Name "mylb" -ResourceGroupName $myResourceGroup | Add-AzureRmLoadBalancerBackendAddressPoolConfig -Name fabrikampool | Set-AzureRmLoadBalancer

    $mylb | Add-AzureRmLoadBalancerFrontendIpConfig -Name fabrikamfe -PublicIpAddress $publicIP2 | Set-AzureRmLoadBalancer
    
    Add-AzureRmLoadBalancerRuleConfig -Name HTTP -LoadBalancer $mylb -FrontendIpConfiguration $frontendIP2 -BackendAddressPool $beaddresspool2 -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 80 | Set-AzureRmLoadBalancer
    ```

12. <span data-ttu-id="51cee-139">Níže uvedených příkazů získat síťovými adaptéry a poté přidejte obě konfigurace IP každý sekundární síťové karty na back-endových adres nástroje pro vyrovnávání zatížení:</span><span class="sxs-lookup"><span data-stu-id="51cee-139">The commands below get the NICs and then add both IP configurations of each secondary NIC to the backend address pool of the load balancer:</span></span>

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

13. <span data-ttu-id="51cee-140">Nakonec je nutné nakonfigurovat záznamy prostředků DNS tak, aby odkazoval na příslušné front-endovou IP adresu služby Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="51cee-140">Finally, you must configure DNS resource records to point to the respective frontend IP address of the Load Balancer.</span></span> <span data-ttu-id="51cee-141">Může hostovat vaše domény v Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="51cee-141">You may host your domains in Azure DNS.</span></span> <span data-ttu-id="51cee-142">Další informace o používání s nástrojem pro vyrovnávání zatížení Azure DNS najdete v tématu [pomocí Azure DNS s jinými službami Azure](../dns/dns-for-azure-services.md).</span><span class="sxs-lookup"><span data-stu-id="51cee-142">For more information about using Azure DNS with Load Balancer, see [Using Azure DNS with other Azure services](../dns/dns-for-azure-services.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="51cee-143">Další kroky</span><span class="sxs-lookup"><span data-stu-id="51cee-143">Next steps</span></span>
- <span data-ttu-id="51cee-144">Další informace o tom, jak kombinací v Azure v rámci služby Vyrovnávání zatížení [pomocí služby Vyrovnávání zatížení v Azure](../traffic-manager/traffic-manager-load-balancing-azure.md).</span><span class="sxs-lookup"><span data-stu-id="51cee-144">Learn more about how to combine load balancing services in Azure in [Using load-balancing services in Azure](../traffic-manager/traffic-manager-load-balancing-azure.md).</span></span>
- <span data-ttu-id="51cee-145">Zjistěte, jak můžete použít různé typy protokolů v Azure ke správě a odstraňování potíží pro vyrovnávání zatížení v [protokolu pro vyrovnávání zatížení Azure analytics](../load-balancer/load-balancer-monitor-log.md).</span><span class="sxs-lookup"><span data-stu-id="51cee-145">Learn how you can use different types of logs in Azure to manage and troubleshoot load balancer in [Log analytics for Azure Load Balancer](../load-balancer/load-balancer-monitor-log.md).</span></span>
