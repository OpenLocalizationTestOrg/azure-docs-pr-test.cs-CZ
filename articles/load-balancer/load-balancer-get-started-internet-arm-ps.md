---
title: "aaaCreate Azure internetového vyrovnávání zátěže – prostředí PowerShell | Microsoft Docs"
description: "Zjistěte, jak toocreate internetové službu Vyrovnávání zatížení ve službě Správce prostředků pomocí prostředí PowerShell"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
tags: azure-resource-manager
ms.assetid: 8257f548-7019-417f-b15f-d004a1eec826
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: e4e0e5271bc83c23fc62c0910e784c57d2b30065
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started"></a>Vytvoření internetového nástroje pro vyrovnávání zatížení v Resource Manageru pomocí prostředí PowerShell

> [!div class="op_single_selector"]
> * [Azure Portal](../load-balancer/load-balancer-get-started-internet-portal.md)
> * [PowerShell](../load-balancer/load-balancer-get-started-internet-arm-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-internet-arm-cli.md)
> * [Šablona](../load-balancer/load-balancer-get-started-internet-arm-template.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Tento článek se týká modelu nasazení Resource Manager hello. Můžete také [zjistěte, jak toocreate internetové nástroj pro vyrovnávání zatížení pomocí modelu nasazení classic hello](load-balancer-get-started-internet-classic-cli.md).

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="deploying-hello-solution-by-using-azure-powershell"></a>Nasazení řešení hello pomocí prostředí Azure PowerShell

Hello následující postupy popisují, jak toocreate internetové nástroj pro vyrovnávání zatížení pomocí Azure Resource Manager pomocí prostředí PowerShell. S Azure Resource Manager, každého prostředku se vytvoří a konfigurovat individuálně a potom se spojí dohromady toocreate nástroj pro vyrovnávání zatížení.

Musíte vytvořit a konfigurovat hello následující objekty toodeploy nástroj pro vyrovnávání zatížení:

* Konfigurace front-endových IP adres: obsahuje veřejné IP adresy pro příchozí síťový provoz.
* Fond adres back-end: obsahuje síťová rozhraní (NIC) pro hello virtuální počítače tooreceive síťový provoz z nástroje pro vyrovnávání zatížení hello.
* Pravidla Vyrovnávání zatížení: obsahuje pravidla, které mapují veřejný port na hello zatížení vyrovnávání tooa portu ve fondu adres back-end hello.
* Příchozí pravidla NAT: obsahuje pravidla, které mapují veřejný port na hello zatížení vyrovnávání tooa portu pro konkrétní virtuální počítač ve fondu adres back-end hello.
* Sondy: obsahuje dostupnosti toocheck stavu sondy používané instancí virtuálního počítače ve fondu adres back-end hello.

Další informace najdete v tématu [Podpora služby Load Balancer v Azure Resource Manageru](load-balancer-arm.md).

## <a name="set-up-powershell-toouse-resource-manager"></a>Nastavení prostředí PowerShell toouse Resource Manager

Ujistěte se, že máte nejnovější verzi produkční hello hello modulu Azure Resource Manager pro prostředí PowerShell:

1. Přihlaste se tooAzure.

    ```powershell
    Login-AzureRmAccount
    ```

    Po zobrazení výzvy zadejte své přihlašovací údaje.

2. Zkontrolujte předplatná hello pro účet hello.

    ```powershell
    Get-AzureRmSubscription
    ```

3. Zvolte, které vaše toouse předplatných Azure.

    ```powershell
    Select-AzureRmSubscription -SubscriptionId 'GUID of subscription'
    ```

4. Vytvořte skupinu prostředků. (Tento krok přeskočte, pokud používáte některou ze stávajících skupin prostředků.)

    ```powershell
    New-AzureRmResourceGroup -Name NRP-RG -location "West US"
    ```

## <a name="create-a-virtual-network-and-a-public-ip-address-for-hello-front-end-ip-pool"></a>Vytvořit virtuální síť a veřejnou IP adresu pro fond hello front-end IP adres

1. Vytvořte podsíť a virtuální síť.

    ```powershell
    $backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24
    New-AzureRmvirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet
    ```

2. Vytvořit prostředek služby Azure veřejnou IP adresu, s názvem **PublicIP**, toobe používá front-endu fond IP adres s názvem DNS hello **loadbalancernrp.westus.cloudapp.azure.com**. následující příkaz hello používá hello Typ statického přidělování.

    ```powershell
    $publicIP = New-AzureRmPublicIpAddress -Name PublicIp -ResourceGroupName NRP-RG -Location 'West US' -AllocationMethod Static -DomainNameLabel loadbalancernrp
    ```

   > [!IMPORTANT]
   > Vyrovnávání zatížení Hello používá hello domény popisek hello veřejnou IP adresu jako předponu pro její plně kvalifikovaný název domény. To se liší od modelu nasazení classic hello, který používá hello cloudové služby jako hello nástroj pro vyrovnávání zatížení plně kvalifikovaný název domény.
   > V tomto příkladu hello plně kvalifikovaný název domény je **loadbalancernrp.westus.cloudapp.azure.com**.

## <a name="create-a-front-end-ip-pool-and-a-back-end-address-pool"></a>Vytvoření front-endového fondu IP adres a back-endového fondu adres

1. Vytvořte front-end IP fond s názvem **LB front-endu** používající hello **PublicIp** prostředků.

    ```powershell
    $frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name LB-Frontend -PublicIpAddress $publicIP
    ```

2. Vytvořte back-endový fond adres s názvem **LB-backend**.

    ```powershell
    $beaddresspool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name LB-backend
    ```

## <a name="create-nat-rules-a-load-balancer-rule-a-probe-and-a-load-balancer"></a>Vytvoření pravidel překladu adres (NAT), pravidla nástroje pro vyrovnávání zatížení, testu a nástroje pro vyrovnávání zatížení

Tento příklad vytvoří hello následující položky:

* Tootranslate pravidlo NAT všechny příchozí přenosy na portu 3441 tooport 3389
* Tootranslate pravidlo NAT všechny příchozí přenosy na portu 3442 tooport 3389
* Stav testu toocheck pravidlo hello na stránku s názvem **HealthProbe.aspx**
* Toobalance pravidlo Vyrovnávání zatížení všechny příchozí přenosy na portu 80 tooport 80 na hello adresy ve fondu back-end hello
* Nástroj pro vyrovnávání zatížení, který používá všechny tyto objekty.

Proveďte následující kroky:

1. Vytvoření pravidla NAT hello.

    ```powershell
    $inboundNATRule1= New-AzureRmLoadBalancerInboundNatRuleConfig -Name RDP1 -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3441 -BackendPort 3389

    $inboundNATRule2= New-AzureRmLoadBalancerInboundNatRuleConfig -Name RDP2 -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3442 -BackendPort 3389
    ```

2. Vytvořte test stavu. Existují dva způsoby tooconfigure sondu:

    Test protokolu HTTP

    ```powershell
    $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name HealthProbe -RequestPath 'HealthProbe.aspx' -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2
    ```

    Test protokolu TCP

    ```powershell
    $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name HealthProbe -Protocol Tcp -Port 80 -IntervalInSeconds 15 -ProbeCount 2
    ```

3. Vytvořte pravidlo nástroje pro vyrovnávání zatížení.

    ```powershell
    $lbrule = New-AzureRmLoadBalancerRuleConfig -Name HTTP -FrontendIpConfiguration $frontendIP -BackendAddressPool  $beAddressPool -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 80
    ```

4. Vytvořte nástroj pro vyrovnávání zatížení hello pomocí hello dříve vytvořené objekty.

    ```powershell
    $NRPLB = New-AzureRmLoadBalancer -ResourceGroupName NRP-RG -Name NRP-LB -Location 'West US' -FrontendIpConfiguration $frontendIP -InboundNatRule $inboundNATRule1,$inboundNatRule2 -LoadBalancingRule $lbrule -BackendAddressPool $beAddressPool -Probe $healthProbe
    ```

## <a name="create-nics"></a>Vytvoření síťových rozhraní

Vytvořit síťových rozhraní (nebo upravit existující) a přidružovat je tooNAT pravidel, pravidla nástroje pro vyrovnávání zatížení a sondy:

1. Získáte hello virtuální síť a podsíť virtuální sítě, kde síťové adaptéry hello musí toobe vytvořili.

    ```powershell
    $vnet = Get-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG
    $backendSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet
    ```

2. Vytvořit síťový adaptér s názvem **lb nic1 být**a přidružte ji k první pravidlo NAT hello a fond back-end adres první (a pouze) hello.

    ```powershell
    $backendnic1= New-AzureRmNetworkInterface -ResourceGroupName NRP-RG -Name lb-nic1-be -Location 'West US' -PrivateIpAddress 10.0.2.6 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[0]
    ```

3. Vytvořit síťový adaptér s názvem **lb nic2 být**a přidružte ji k hello druhé pravidlo NAT a fond back-end adres první (a pouze) hello.

    ```powershell
    $backendnic2= New-AzureRmNetworkInterface -ResourceGroupName NRP-RG -Name lb-nic2-be -Location 'West US' -PrivateIpAddress 10.0.2.7 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[1]
    ```

4. Zkontrolujte síťové adaptéry hello.

        $backendnic1

    Očekávaný výstup:

        Name                 : lb-nic1-be
        ResourceGroupName    : NRP-RG
        Location             : westus
        Id                   : /subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
        Etag                 : W/"d448256a-e1df-413a-9103-a137e07276d1"
        ResourceGuid         : 896cac4f-152a-40b9-b079-3e2201a5906e
        ProvisioningState    : Succeeded
        Tags                 :
        VirtualMachine       : null
        IpConfigurations     : [
                            {
                            "Name": "ipconfig1",
                            "Etag": "W/\"d448256a-e1df-413a-9103-a137e07276d1\"",
                            "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be/ipConfigurations/ipconfig1",
                            "PrivateIpAddress": "10.0.2.6",
                            "PrivateIpAllocationMethod": "Static",
                            "Subnet": {
                                "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/virtualNetworks/NRPVNet/subnets/LB-Subnet-BE"
                            },
                            "ProvisioningState": "Succeeded",
                            "PrivateIpAddressVersion": "IPv4",
                            "PublicIpAddress": {
                                "Id": null
                            },
                            "LoadBalancerBackendAddressPools": [
                                {
                                "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/loadBalancers/NRPlb/backendAddressPools/LB-backend"
                                }
                            ],
                            "LoadBalancerInboundNatRules": [
                                {
                                "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/loadBalancers/NRPlb/inboundNatRules/RDP1"
                                }
                            ],
                            "Primary": true,
                            "ApplicationGatewayBackendAddressPools": []
                            }
                        ]
        DnsSettings          : {
                            "DnsServers": [],
                            "AppliedDnsServers": [],
                            "InternalDomainNameSuffix": "prcwibzcuvie5hnxav0yjks2cd.dx.internal.cloudapp.net"
                        }
        EnableIPForwarding   : False
        NetworkSecurityGroup : null
        Primary              :

5. Použití hello `Add-AzureRmVMNetworkInterface` rutiny tooassign hello síťové adaptéry toodifferent virtuálních počítačů.

## <a name="create-a-virtual-machine"></a>Vytvoření virtuálního počítače

Pokyny k vytvoření virtuálního počítače a přiřazení síťového rozhraní najdete v tématu [Vytvoření virtuálního počítače Azure pomocí prostředí PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json).

## <a name="add-hello-network-interface-toohello-load-balancer"></a>Přidat hello síťové rozhraní toohello nástroj pro vyrovnávání zatížení

1. Nástroj pro vyrovnávání zatížení hello načíst z Azure.

    Načte prostředek pro vyrovnávání zatížení hello do proměnné (Pokud jste to neudělali, ještě). Proměnná Hello se nazývá **$lb**. Hello použijte stejné názvy hello prostředek pro vyrovnávání zatížení, který jste vytvořili dříve.

    ```powershell
    $lb= get-azurermloadbalancer -name NRP-LB -resourcegroupname NRP-RG
    ```

2. Načíst proměnnou tooa hello konfigurace back-end.

    ```powershell
    $backend=Get-AzureRmLoadBalancerBackendAddressPoolConfig -name LB-backend -LoadBalancer $lb
    ```

3. Zatížení hello už vytvořené síťové rozhraní do proměnné. Název proměnné Hello je **$nic**. Hello název síťového rozhraní je hello stejný jako ten, z hello předchozího příkladu.

    ```powershell
    $nic =get-azurermnetworkinterface -name lb-nic1-be -resourcegroupname NRP-RG
    ```

4. Změna konfigurace back-end hello hello síťového rozhraní.

    ```powershell
    $nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$backend
    ```

5. Uložte objekt rozhraní sítě hello.

    ```powershell
    Set-AzureRmNetworkInterface -NetworkInterface $nic
    ```

    Po přidání fondu back-end pro vyrovnávání zatížení toohello síťové rozhraní začne přijímá síťový provoz na základě hello pravidel Vyrovnávání zatížení pro tento prostředek pro vyrovnávání zatížení.

## <a name="update-an-existing-load-balancer"></a>Aktualizace stávajícího nástroje pro vyrovnávání zatížení

1. Pomocí hello vyrovnávání zátěže z předchozího příkladu Dobrý den, přiřazení proměnné objektu toohello nástroje pro vyrovnávání zatížení **$slb** pomocí `Get-AzureLoadBalancer`.

    ```powershell
    $slb = get-AzureRmLoadBalancer -Name NRP-LB -ResourceGroupName NRP-RG
    ```

2. V následujícím příkladu hello přidat příchozí pravidlo NAT--pomocí portu 81 hello front-end fondu a portu 8181 pro fond back-end hello – tooan existující nástroj pro vyrovnávání zatížení.

    ```powershell
    $slb | Add-AzureRmLoadBalancerInboundNatRuleConfig -Name NewRule -FrontendIpConfiguration $slb.FrontendIpConfigurations[0] -FrontendPort 81  -BackendPort 8181 -Protocol TCP
    ```

3. Uložte novou konfiguraci hello pomocí `Set-AzureLoadBalancer`.

    ```powershell
    $slb | Set-AzureRmLoadBalancer
    ```

## <a name="remove-a-load-balancer"></a>Odebrání nástroje pro vyrovnávání zatížení

Použijte příkaz hello `Remove-AzureLoadBalancer` toodelete Vyrovnávání zatížení dříve vytvořenou s názvem **NRP-LB** ve skupině prostředků s názvem **NRP-RG**.

```powershell
Remove-AzureRmLoadBalancer -Name NRP-LB -ResourceGroupName NRP-RG
```

> [!NOTE]
> Můžete použít volitelný přepínač hello **-Force** tooavoid hello řádku pro odstranění.

## <a name="next-steps"></a>Další kroky

[Začínáme s konfigurací interního nástroje pro vyrovnávání zatížení](load-balancer-get-started-ilb-arm-ps.md)

[Konfigurace distribučního režimu nástroje pro vyrovnávání zatížení](load-balancer-distribution-mode.md)

[Konfigurace nastavení časového limitu nečinnosti protokolu TCP pro nástroj pro vyrovnávání zatížení](load-balancer-tcp-idle-timeout.md)
