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
# <a name="load-balancing-on-multiple-ip-configurations-using-powershell"></a>Víc konfigurací IP adres pomocí prostředí PowerShell pro vyrovnávání zatížení

> [!div class="op_single_selector"]
> * [Azure Portal](load-balancer-multiple-ip.md)
> * [Rozhraní příkazového řádku](load-balancer-multiple-ip-cli.md)
> * [PowerShell](load-balancer-multiple-ip-powershell.md)

Tento článek popisuje, jak toouse Vyrovnávání zatížení Azure s více IP adres v sekundárním síťovém rozhraní (NIC). V tomto scénáři máme dva virtuální počítače se systémem Windows, každý s primární a sekundární síťový adaptér. Každý z hello sekundární síťové adaptéry mají dvě konfigurace protokolu IP. Každý virtuální počítač hostuje weby contoso.com a fabrikam.com. Každý web je vázané tooone hello konfigurace protokolu IP v síťovém adaptéru hello sekundární. Můžeme použít nástroj pro vyrovnávání zatížení Azure tooexpose dvě front-end IP adresy, jednu pro každý web, toodistribute provoz toohello odpovídající konfiguraci protokolu IP pro web hello. Tento scénář používá hello stejné číslo portu mezi frontends, jak oba back-end fondu IP adres.

![Scénář LB bitové kopie](./media/load-balancer-multiple-ip/lb-multi-ip.PNG)

## <a name="steps-tooload-balance-on-multiple-ip-configurations"></a>Vyrovnávat tooload kroky na víc konfigurací IP adres

Postupujte podle kroků hello tooachieve hello scénář popsaný v tomto článku:

1. Nainstalujte Azure PowerShell. V tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) informace o instalaci hello nejnovější verzi prostředí Azure PowerShell, výběr předplatného a přihlášení tooyour účtu.
2. Vytvoření skupiny prostředků pomocí hello následující nastavení:

    ```powershell
    $location = "westcentralus".
    $myResourceGroup = "contosofabrikam"
    ```

    Další informace najdete v části Krok 2 [vytvořte skupinu prostředků](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json).

3. [Vytvoření skupiny dostupnosti](../virtual-machines/windows/tutorial-availability-sets.md?toc=%2fazure%2fload-balancer%2ftoc.json) toocontain virtuální počítače. V tomto scénáři použijte hello následující příkaz:

    ```powershell
    New-AzureRmAvailabilitySet -ResourceGroupName "contosofabrikam" -Name "myAvailset" -Location "West Central US"
    ```

4. Postupujte podle pokynů kroky 3 až 5 v [vytvoření virtuálního počítače s Windows](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json) článek tooprepare hello vytvoření virtuálního počítače s jeden síťový adaptér. Spustit krok 6.1 a použijte místo krok 6.2 hello:

    ```powershell
    $availset = Get-AzureRmAvailabilitySet -ResourceGroupName "contosofabrikam" -Name "myAvailset"
    New-AzureRmVMConfig -VMName "VM1" -VMSize "Standard_DS1_v2" -AvailabilitySetId $availset.Id
    ```

    Dokončete [vytvoření virtuálního počítače s Windows](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json) kroky 6.3 prostřednictvím 6.8.

5. Přidejte druhý tooeach konfigurace IP z hello virtuálních počítačů. Postupujte podle pokynů hello v [přiřadit více IP adres počítačů toovirtual](../virtual-network/virtual-network-multiple-ip-addresses-powershell.md#add) článku. Použijte hello následující nastavení:

    ```powershell
    $NicName = "VM1-NIC2"
    $RgName = "contosofabrikam"
    $NicLocation = "West Central US"
    $IPConfigName4 = "VM1-ipconfig2"
    $Subnet1 = Get-AzureRmVirtualNetworkSubnetConfig -Name "mySubnet" -VirtualNetwork $myVnet
    ```

    Není nutné tooassociate hello sekundární IP konfigurace s veřejné IP adresy pro hello účelu tohoto kurzu. Upravte hello příkaz tooremove hello veřejné IP přidružení část.

6. Proveďte kroky 4 až 6 tohoto článku znovu pro virtuálního počítače 2. Zda tooreplace hello virtuálních počítačů název tooVM2 být při této činnosti. Všimněte si, že nepotřebujete toocreate virtuální sítě pro hello druhé virtuálních počítačů. Může nebo nemusí vytvořit novou podsíť, na základě vaší případu použití.

7. Vytvořte dvě veřejné IP adresy a uložit je do hello příslušné proměnné, jak je znázorněno:

    ```powershell
    $publicIP1 = New-AzureRmPublicIpAddress -Name PublicIp1 -ResourceGroupName contosofabrikam -Location 'West Central US' -AllocationMethod Dynamic -DomainNameLabel contoso
    $publicIP2 = New-AzureRmPublicIpAddress -Name PublicIp2 -ResourceGroupName contosofabrikam -Location 'West Central US' -AllocationMethod Dynamic -DomainNameLabel fabrikam

    $publicIP1 = Get-AzureRmPublicIpAddress -Name PublicIp1 -ResourceGroupName contosofabrikam
    $publicIP2 = Get-AzureRmPublicIpAddress -Name PublicIp2 -ResourceGroupName contosofabrikam
    ```

8. Vytvořte dvě konfigurace IP front-endu:

    ```powershell
    $frontendIP1 = New-AzureRmLoadBalancerFrontendIpConfig -Name contosofe -PublicIpAddress $publicIP1
    $frontendIP2 = New-AzureRmLoadBalancerFrontendIpConfig -Name fabrikamfe -PublicIpAddress $publicIP2
    ```

9. Vytvoření vašeho back-endové fondy adres, sondu a pravidel Vyrovnávání zatížení:

    ```powershell
    $beaddresspool1 = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name contosopool
    $beaddresspool2 = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name fabrikampool

    $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name HTTP -RequestPath 'index.html' -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2

    $lbrule1 = New-AzureRmLoadBalancerRuleConfig -Name HTTPc -FrontendIpConfiguration $frontendIP1 -BackendAddressPool $beaddresspool1 -Probe $healthprobe -Protocol Tcp -FrontendPort 80 -BackendPort 80
    $lbrule2 = New-AzureRmLoadBalancerRuleConfig -Name HTTPf -FrontendIpConfiguration $frontendIP2 -BackendAddressPool $beaddresspool2 -Probe $healthprobe -Protocol Tcp -FrontendPort 80 -BackendPort 80
    ```

10. Až budete mít tyto prostředky vytvořit, vytvořte nástroj pro vyrovnávání zatížení:

    ```powershell
    $mylb = New-AzureRmLoadBalancer -ResourceGroupName contosofabrikam -Name mylb -Location 'West Central US' -FrontendIpConfiguration $frontendIP1 -LoadBalancingRule $lbrule -BackendAddressPool $beAddressPool -Probe $healthProbe
    ```

11. Přidejte hello druhý back-end adresy fondu a front-endovou IP konfigurace tooyour nově vytvořený nástroj pro vyrovnávání zatížení:

    ```powershell
    $mylb = Get-AzureRmLoadBalancer -Name "mylb" -ResourceGroupName $myResourceGroup | Add-AzureRmLoadBalancerBackendAddressPoolConfig -Name fabrikampool | Set-AzureRmLoadBalancer

    $mylb | Add-AzureRmLoadBalancerFrontendIpConfig -Name fabrikamfe -PublicIpAddress $publicIP2 | Set-AzureRmLoadBalancer
    
    Add-AzureRmLoadBalancerRuleConfig -Name HTTP -LoadBalancer $mylb -FrontendIpConfiguration $frontendIP2 -BackendAddressPool $beaddresspool2 -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 80 | Set-AzureRmLoadBalancer
    ```

12. níže uvedené příkazy Hello získat hello síťové adaptéry a pak přidat že nástroj pro vyrovnávání zatížení obě konfigurace IP každý sekundární síťový adaptér toohello back-end fondu adres Dobrý den:

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

13. Nakonec musíte nakonfigurovat DNS prostředků záznamů toopoint toohello příslušných front-endovou IP adresu hello nástroj pro vyrovnávání zatížení. Může hostovat vaše domény v Azure DNS. Další informace o používání s nástrojem pro vyrovnávání zatížení Azure DNS najdete v tématu [pomocí Azure DNS s jinými službami Azure](../dns/dns-for-azure-services.md).

## <a name="next-steps"></a>Další kroky
- Další informace o tom, jak Vyrovnávání zatížení toocombine služby v Azure v [pomocí služby Vyrovnávání zatížení v Azure](../traffic-manager/traffic-manager-load-balancing-azure.md).
- Zjistěte, jak můžete použít různé typy protokolů v Azure toomanage a řešení vyrovnávání zatížení v [protokolu pro vyrovnávání zatížení Azure analytics](../load-balancer/load-balancer-monitor-log.md).
