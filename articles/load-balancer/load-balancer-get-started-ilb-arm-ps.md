---
title: "aaaCreate Azure interní nástroj pro vyrovnávání - zatížení, prostředí PowerShell | Microsoft Docs"
description: "Zjistěte, jak toocreate na interní vyrovnávání zátěže pomocí prostředí PowerShell ve službě Správce prostředků"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
tags: azure-resource-manager
ms.assetid: c6c98981-df9d-4dd7-a94b-cc7d1dc99369
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: 4b9657c77aa32a142de49ff7871ed6a396b22223
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-internal-load-balancer-using-powershell"></a>Vytvoření interního nástroje pro vyrovnávání zatížení pomocí prostředí PowerShell

> [!div class="op_single_selector"]
> * [Azure Portal](../load-balancer/load-balancer-get-started-ilb-arm-portal.md)
> * [PowerShell](../load-balancer/load-balancer-get-started-ilb-arm-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-ilb-arm-cli.md)
> * [Šablona](../load-balancer/load-balancer-get-started-ilb-arm-template.md)

[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

> [!NOTE]
> Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../azure-resource-manager/resource-manager-deployment-model.md).  Tento článek popisuje použití modelu nasazení Resource Manager hello, které společnost Microsoft doporučuje pro většinu nasazení nové místo hello [modelu nasazení classic](load-balancer-get-started-ilb-classic-ps.md).

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

Hello následující kroky popisují, jak toocreate na interní vyrovnávání zátěže pomocí Azure Resource Manager pomocí prostředí PowerShell. S Azure Resource Manager, hello položky toocreate Vyrovnávání zatížení pro vnitřní jsou konfigurovat individuálně a potom v kombinaci toocreate nástroj pro vyrovnávání zatížení.

Je třeba toocreate a nakonfigurovat hello následující objekty toodeploy nástroj pro vyrovnávání zatížení:

* Koncová konfigurace IP front - nakonfiguruje hello privátní IP adresu pro příchozí síťový provoz
* Fond adres back-end - nakonfiguruje hello síťových rozhraní, které budou přijímat provoz hello skupinu s vyrovnáváním zatížení, který přichází z fondu IP front-endu
* Pravidla Vyrovnávání zatížení – zdroj a konfigurace místního portu pro hello nástroj pro vyrovnávání zatížení.
* Sondy – konfiguruje test stavu hello stavu pro instance virtuálních počítačů hello.
* Příchozí pravidla NAT – konfiguruje hello portu pravidla toodirectly přístup, jednu z instancí virtuálního počítače hello.

Další informace o komponentách nástroje pro vyrovnávání zatížení s Azure Resource Managerem najdete v tématu [Podpora nástroje Load Balancer v Azure Resource Manageru](load-balancer-arm.md).

Hello následující kroky popisují, jak tooconfigure Vyrovnávání zatížení mezi dvěma virtuálními počítači.

## <a name="setup-powershell-toouse-resource-manager"></a>Instalace prostředí PowerShell toouse Resource Manager

Ujistěte se, že máte hello nejnovější produkční verzi hello Azure modul pro prostředí PowerShell a mít PowerShell nastavit správně tooaccess vašeho předplatného Azure.

### <a name="step-1"></a>Krok 1

```powershell
Login-AzureRmAccount
```

### <a name="step-2"></a>Krok 2

Zkontrolujte předplatná hello pro účet hello

```powershell
Get-AzureRmSubscription
```

Výzvami tooAuthenticate bude pomocí svých přihlašovacích údajů.

### <a name="step-3"></a>Krok 3

Zvolte, které vaše toouse předplatných Azure.

```powershell
Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
```

### <a name="create-resource-group-for-load-balancer"></a>Vytvoření skupiny prostředků pro nástroj pro vyrovnávání zatížení

Vytvořte novou skupinu prostředků (tento krok přeskočte, pokud používáte některou ze stávajících skupin prostředků).

```powershell
New-AzureRmResourceGroup -Name NRP-RG -location "West US"
```

Azure Resource Manager vyžaduje, aby všechny skupiny prostředků určily umístění. To slouží jako hello výchozí umístění pro prostředky v příslušné skupině prostředků. Zajistěte, aby všechny příkazy hello toocreate nástroj pro vyrovnávání zatížení bude používat stejné skupiny prostředků.

V hello příkladu výše jsme vytvořili skupinu prostředků s názvem "NRP-RG" a umístěním "Západní USA".

## <a name="create-virtual-network-and-a-private-ip-address-for-front-end-ip-pool"></a>Vytvoření virtuální sítě a privátní IP adresy pro front-endový fond IP adres

Vytvoří podsíť virtuální sítě hello a přiřadí toovariable $backendSubnet

```powershell
$backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24
```

Vytvořte virtuální síť:

```powershell
$vnet= New-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet
```

Vytvoří virtuální síť hello a přidá hello podsíť virtuální sítě se podsíť lb toohello NRPVNet a přiřadí toovariable $vnet

## <a name="create-front-end-ip-pool-and-backend-address-pool"></a>Vytvoření front-endového fondu IP adres a back-endového fondu adres

Nastavení fondu IP front-endu pro příchozí hello zatížení nástroje pro vyrovnávání zatížení sítě a back-end adresy fondu tooreceive hello přenosy přesměrované vyrovnáváním zatížení.

### <a name="step-1"></a>Krok 1

Vytvoření fondu IP front-endu pomocí hello privátní IP adresu 10.0.2.5 pro hello podsíť 10.0.2.0/24 kterým bude hello příchozí síťový provoz koncový bod.

```powershell
$frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name LB-Frontend -PrivateIpAddress 10.0.2.5 -SubnetId $vnet.subnets[0].Id
```

### <a name="step-2"></a>Krok 2

Nastavení fondu back-end adresy použít tooreceive příchozí provoz z fondu IP front-endu:

```powershell
$beaddresspool= New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "LB-backend"
```

## <a name="create-lb-rules-nat-rules-probe-and-load-balancer"></a>Vytvoření pravidel nástroje pro vyrovnávání zatížení, pravidel překladu adres (NAT), testu a nástroje pro vyrovnávání zatížení

Po vytvoření hello fond IP front-endu a back-endových adres hello, budete potřebovat toocreate hello pravidla, která bude patřit toohello prostředek pro vyrovnávání zatížení:

### <a name="step-1"></a>Krok 1

```powershell
$inboundNATRule1= New-AzureRmLoadBalancerInboundNatRuleConfig -Name "RDP1" -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3441 -BackendPort 3389

$inboundNATRule2= New-AzureRmLoadBalancerInboundNatRuleConfig -Name "RDP2" -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3442 -BackendPort 3389

$healthProbe = New-AzureRmLoadBalancerProbeConfig -Name "HealthProbe" -RequestPath "HealthProbe.aspx" -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2

$lbrule = New-AzureRmLoadBalancerRuleConfig -Name "HTTP" -FrontendIpConfiguration $frontendIP -BackendAddressPool $beAddressPool -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 80
```

výše uvedený příklad Hello vytváří hello následující položky:

* Pravidlo NAT, které všechny příchozí přenosy tooport 3441 přejde tooport 3389.
* druhé pravidlo NAT, která všechny příchozí přenosy tooport 3442 přejde tooport 3389.
* pravidlo Vyrovnávání zatížení, která načte vyrovnávat veškerý příchozí provoz na veřejném portu 80 toolocal portu 80 ve fondu adres hello back-end.
* pravidlo testu, který zkontroluje hello stav pro cestu "HealthProbe.aspx"

### <a name="step-2"></a>Krok 2

Vytvořte nástroj pro vyrovnávání zatížení hello společně přidání všechny objekty (pravidla NAT, pravidla nástroje pro vyrovnávání zatížení, test konfigurace):

```powershell
$NRPLB = New-AzureRmLoadBalancer -ResourceGroupName "NRP-RG" -Name "NRP-LB" -Location "West US" -FrontendIpConfiguration $frontendIP -InboundNatRule $inboundNATRule1,$inboundNatRule2 -LoadBalancingRule $lbrule -BackendAddressPool $beAddressPool -Probe $healthProbe
```

## <a name="create-network-interfaces"></a>Vytvoření síťových rozhraní

Po vytvoření hello interní vyrovnávání zátěže, třeba definovat rozhraní sítě, která bude moci přijmout hello příchozí skupinu s vyrovnáváním zatížení síťový provoz, pravidla NAT a kontroly. v takovém případě Hello síťové rozhraní konfigurované jednotlivě a lze přiřadit tooa virtuální počítač později.

### <a name="step-1"></a>Krok 1

Získáte prostředek hello virtuální síť a podsíť toocreate síťových rozhraní:

```powershell
$vnet = Get-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG

$backendSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet
```

Tento krok vytvoří rozhraní sítě, která bude patřit toohello fond back-end, Vyrovnávání zatížení a přidružit hello první pravidlo NAT RDP pro toto síťové rozhraní:

```powershell
$backendnic1= New-AzureRmNetworkInterface -ResourceGroupName "NRP-RG" -Name lb-nic1-be -Location "West US" -PrivateIpAddress 10.0.2.6 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[0]
```

### <a name="step-2"></a>Krok 2

Vytvořte druhé síťové rozhraní s názvem: LB-Nic2-BE:

Tento krok vytvoří druhé síťové rozhraní, přiřazení toohello stejné Vyrovnávání zatížení zpět ukončení fondu a přidružení hello druhé pravidlo NAT pro RDP vytvořit:

```powershell
$backendnic2= New-AzureRmNetworkInterface -ResourceGroupName "NRP-RG" -Name lb-nic2-be -Location "West US" -PrivateIpAddress 10.0.2.7 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[1]
```

Konečný výsledek Hello zobrazí hello následující:

    $backendnic1

Očekávaný výstup:

    Name                 : lb-nic1-be
    ResourceGroupName    : NRP-RG
    Location             : westus
    Id                   : /subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
    Etag                 : W/"d448256a-e1df-413a-9103-a137e07276d1"
    ProvisioningState    : Succeeded
    Tags                 :
    VirtualMachine       : null
    IpConfigurations     : [
                         {
                           "PrivateIpAddress": "10.0.2.6",
                           "PrivateIpAllocationMethod": "Static",
                           "Subnet": {
                             "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/virtualNetworks/NRPVNet/subnets/LB-Subnet-BE"
                           },
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
                           "ProvisioningState": "Succeeded",
                           "Name": "ipconfig1",
                           "Etag": "W/\"d448256a-e1df-413a-9103-a137e07276d1\"",
                           "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be/ipConfigurations/ipconfig1"
                         }
                       ]
    DnsSettings          : {
                         "DnsServers": [],
                         "AppliedDnsServers": []
                       }
    AppliedDnsSettings   :
    NetworkSecurityGroup : null
    Primary              : False



### <a name="step-3"></a>Krok 3

Používejte hello příkaz Add-AzureRmVMNetworkInterface tooassign hello seskupování tooa virtuální počítač.

Můžete najít hello pokyny krok za krokem toocreate virtuálního počítače a přiřaďte tooa seskupování následující dokumentaci hello: [vytvoření virtuálního počítače Azure pomocí prostředí PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json).

## <a name="add-hello-network-interface"></a>Přidejte hello síťové rozhraní

Pokud již máte virtuální počítač vytvořený, můžete přidat hello síťové rozhraní s hello následující kroky:

### <a name="step-1"></a>Krok 1

Načte prostředek pro vyrovnávání zatížení hello do proměnné (Pokud jste to neudělali, ještě). Proměnná Hello používá je názvem $lb a hello použijte stejné názvy z prostředek pro vyrovnávání zatížení hello vytvořili výše.

```powershell
$lb = Get-AzureRmLoadBalancer –name NRP-LB -resourcegroupname NRP-RG
```

### <a name="step-2"></a>Krok 2

Načíst proměnnou tooa konfigurace back-end hello.

```powershell
$backend = Get-AzureRmLoadBalancerBackendAddressPoolConfig -name backendpool1 -LoadBalancer $lb
```

### <a name="step-3"></a>Krok 3

Zatížení hello už vytvořené síťové rozhraní do proměnné. Název proměnné Hello používá je $ síťový adaptér. název síťového rozhraní Hello používá stejný je hello z výše uvedeném příkladu hello.

```powershell
$nic = Get-AzureRmNetworkInterface –name lb-nic1-be -resourcegroupname NRP-RG
```

### <a name="step-4"></a>Krok 4

Změna konfigurace back-end hello hello síťového rozhraní.

```powershell
$nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$backend
```

### <a name="step-5"></a>Krok 5

Uložte objekt rozhraní sítě hello.

```powershell
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

Po přidání toohello fond back-end Vyrovnávání zatížení rozhraní sítě začne přijímat síťové přenosy podle pravidel pro tento prostředek pro vyrovnávání zatížení vyrovnávání zatížení hello.

## <a name="update-an-existing-load-balancer"></a>Aktualizace stávajícího nástroje pro vyrovnávání zatížení

### <a name="step-1"></a>Krok 1
Použití služby Vyrovnávání zatížení hello z hello příkladu výše, přiřadíte toovariable objekt nástroje pro vyrovnávání zatížení $slb pomocí Get-AzureRmLoadBalancer

```powershell
$slb = Get-AzureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG
```

### <a name="step-2"></a>Krok 2

V následujícím příkladu hello přidáte nové pravidlo příchozí NAT přes port 81 v hello front end a port 8181 pro hello back end fondu tooan existující nástroj pro vyrovnávání zatížení

```powershell
$slb | Add-AzureRmLoadBalancerInboundNatRuleConfig -Name NewRule -FrontendIpConfiguration $slb.FrontendIpConfigurations[0] -FrontendPort 81  -BackendPort 8181 -Protocol Tcp
```

### <a name="step-3"></a>Krok 3

Uložte novou konfiguraci hello pomocí Set-AzureLoadBalancer

```powershell
$slb | Set-AzureRmLoadBalancer
```

## <a name="remove-a-load-balancer"></a>Odebrání nástroje pro vyrovnávání zatížení

Použít hello příkaz Remove-AzureRmLoadBalancer toodelete Vyrovnávání zatížení dříve vytvořenou s názvem "NRP-LB" ve skupině prostředků názvem "NRP-RG"

```powershell
Remove-AzureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG
```

> [!NOTE]
> Můžete použít hello volitelný přepínač - Force tooavoid hello řádku pro odstranění.

## <a name="next-steps"></a>Další kroky

[Konfigurace distribučního režimu nástroje pro vyrovnávání zatížení](load-balancer-distribution-mode.md)

[Konfigurace nastavení časového limitu nečinnosti protokolu TCP pro nástroj pro vyrovnávání zatížení](load-balancer-tcp-idle-timeout.md)
