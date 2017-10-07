---
title: "příkazy aaaCommon prostředí PowerShell pro virtuální sítě Azure | Microsoft Docs"
description: "Spuštění vytvoření virtuální sítě a jeho přidružené prostředky pro virtuální počítače běžné tooget s příkazy prostředí PowerShell."
services: virtual-machines-windows
documentationcenter: 
author: davidmu1
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 56e1a73c-8299-4996-bd03-f74585caa1dc
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: davidmu
ms.openlocfilehash: b46b78f1b7c5a0c5ba917fb48f568d871e5e8789
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="common-powershell-commands-for-azure-virtual-networks"></a>Běžné příkazy prostředí PowerShell pro Azure Virtual Network

Pokud chcete, aby toocreate virtuálního počítače, je třeba toocreate [virtuální sítě](../../virtual-network/virtual-networks-overview.md) nebo vědět o existující virtuální síť, ve které hello lze přidat virtuální počítač. Obvykle když vytvoříte virtuální počítač, musíte taky tooconsider vytváření hello prostředků popsaných v tomto článku.

V tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) informace o instalaci hello nejnovější verzi prostředí Azure PowerShell, výběr předplatného a přihlášení tooyour účtu.

Některé proměnné můžou být užitečné pro vás, pokud používá více než jeden z hello příkazy v tomto článku:

- $location - hello umístění hello síťových prostředků. Můžete použít [Get-AzureRmLocation](https://docs.microsoft.com/powershell/module/azurerm.resources/get-azurermlocation) toofind [geografické oblasti](https://azure.microsoft.com/regions/) který vám vyhovuje.
- $myResourceGroup - hello název skupiny prostředků hello, kde se nachází hello síťovým prostředkům.

## <a name="create-network-resources"></a>Vytvoření síťové prostředky

| Úkol | Příkaz |
| ---- | ------- |
| Vytvoření konfigurací podsítě |$subnet1 = [nové AzureRmVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) -Name "mySubnet1" - AddressPrefix XX. X.X.X/XX<BR>$Podsíť2 = nové AzureRmVirtualNetworkSubnetConfig-Name "mySubnet2" - AddressPrefix XX. X.X.X/XX<BR><BR>Typické síť může mít podsíť [internetové nástroj pro vyrovnávání zatížení](../../load-balancer/load-balancer-internet-overview.md) a samostatnou podsíť pro [nástroj pro vyrovnávání zatížení pro vnitřní](../../load-balancer/load-balancer-internal-overview.md). |
| Vytvoření virtuální sítě |$vnet = [New-AzureRmVirtualNetwork](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermvirtualnetwork) -Name "myVNet" - ResourceGroupName $myResourceGroup-umístění $location - AddressPrefix XX. X.X.X/XX-podsíť $subnet1, Podsíť2 $ |
| Test pro název jedinečný domény |[Test AzureRmDnsAvailability](https://docs.microsoft.com/powershell/module/azurerm.network/test-azurermdnsavailability) - DomainNameLabel "myDNS"-umístění $location<BR><BR>Můžete zadat název domény DNS [prostředek veřejné IP](../../virtual-network/virtual-network-ip-addresses-overview-arm.md), která vytvoří mapování domainname.location.cloudapp.azure.com toohello veřejných IP adres v hello servery spravovat Azure DNS. Hello název může obsahovat pouze písmena, číslice a pomlčky. Hello první a poslední znak musí být písmeno nebo číslo a název domény hello musí být jedinečný v rámci jeho umístění Azure. Pokud **True** se vrátí, je navržený název globálně jedinečný. |
| Vytvoření veřejné IP adresy |$pip = [New-AzureRmPublicIpAddress](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermpublicipaddress) -Name "myPublicIp" - ResourceGroupName $myResourceGroup - DomainNameLabel "myDNS"-umístění $location - AllocationMethod dynamický<BR><BR>veřejná IP adresa Hello používá hello název domény, který dříve testovány a je používána hello front-endovou konfiguraci služby Vyrovnávání zatížení hello. |
| Vytvoření konfigurace IP front-endu |$frontendIP = [New-AzureRmLoadBalancerFrontendIpConfig](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermloadbalancerfrontendipconfig) -Name "myFrontendIP" - PublicIpAddress $pip<BR><BR>Hello front-endovou konfiguraci zahrnuje hello veřejnou IP adresu, která jste dříve vytvořili pro příchozí síťový provoz. |
| Vytvořit fond adres back-end |$beAddressPool = [New-AzureRmLoadBalancerBackendAddressPoolConfig](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermloadbalancerbackendaddresspoolconfig) -Name "myBackendAddressPool"<BR><BR>Poskytuje možnosti pro interní adresy IP adresy pro back-end hello Dobrý den zatížení vyrovnávání, které jsou přístupné přes síťové rozhraní. |
| Vytvořit sondu |$healthProbe = [New-AzureRmLoadBalancerProbeConfig](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermloadbalancerprobeconfig) -název "myProbe" - RequestPath 'HealthProbe.aspx'-protokolu http-Port 80 - IntervalInSeconds 15 - ProbeCount 2<BR><BR>Obsahuje stav sondy používané toocheck dostupnosti instancí virtuálních počítačů ve fondu adres hello back-end. |
| Vytvořit pravidlo Vyrovnávání zatížení |$lbRule = [New-AzureRmLoadBalancerRuleConfig](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermloadbalancerruleconfig) -název HTTP - FrontendIpConfiguration $frontendIP - BackendAddressPool $beAddressPool-testu $healthProbe-80 protokolu Tcp - FrontendPort - BackendPort 80<BR><BR>Obsahuje pravidla, která přiřadit veřejný port na Vyrovnávání zatížení hello tooa port ve fondu adres hello back-end. |
| Vytvoření příchozího pravidla NAT |$inboundNATRule = [New-AzureRmLoadBalancerInboundNatRuleConfig](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermloadbalancerinboundnatruleconfig) -Name "myInboundRule1" - FrontendIpConfiguration $frontendIP-Protocol TCP - FrontendPort 3441 - BackendPort 3389<BR><BR>Obsahuje pravidla mapování veřejný port na hello zatížení vyrovnávání tooa portu pro konkrétní virtuální počítač ve fondu adres hello back-end. |
| Vytvoření nástroje pro vyrovnávání zatížení |$loadBalancer = [New-AzureRmLoadBalancer](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermloadbalancer) - ResourceGroupName $myResourceGroup-název "myLoadBalancer"-umístění $location - FrontendIpConfiguration $frontendIP - InboundNatRule $inboundNATRule - LoadBalancingRule $lbRule - BackendAddressPool $beAddressPool-testu $healthProbe |
| Vytvořit rozhraní sítě |$nic1 = [New-AzureRmNetworkInterface](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermnetworkinterface) - ResourceGroupName $myResourceGroup-Name "myNIC" - umístění $location - PrivateIpAddress XX. X.X.X-$loadBalancer.BackendAddressPools[0 - LoadBalancerBackendAddressPool podsíť $Podsíť2] - LoadBalancerInboundNatRule $loadBalancer.InboundNatRules[0]<BR><BR>Vytvořte rozhraní sítě pomocí hello veřejnou IP adresu a podsíť virtuální sítě, kterou jste vytvořili. |

## <a name="get-information-about-network-resources"></a>Získat informace o síťové prostředky

| Úkol | Příkaz |
| ---- | ------- |
| Seznam virtuálních sítí |[Get-AzureRmVirtualNetwork](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermvirtualnetwork) - ResourceGroupName $myResourceGroup<BR><BR>Zobrazí všechny hello virtuální sítě ve skupině prostředků hello. |
| Získat informace o virtuální sítě |Get-AzureRmVirtualNetwork-Name "myVNet" - ResourceGroupName $myResourceGroup |
| Seznam podsítí ve virtuální síti |Get-AzureRmVirtualNetwork-Name "myVNet" - ResourceGroupName $myResourceGroup &#124; Vybrat podsítě |
| Získání informací o podsíti |[Get-AzureRmVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermvirtualnetworksubnetconfig) -Name "mySubnet1" - virtuální síť $vnet<BR><BR>Získá informace o podsíti hello v zadané virtuální síti hello. Hodnota Hello $vnet představuje hello objekt vrácený rutinou Get-AzureRmVirtualNetwork. |
| Seznam IP adres |[Get-AzureRmPublicIpAddress](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermpublicipaddress) - ResourceGroupName $myResourceGroup<BR><BR>Uvádí hello veřejné IP adresy ve skupině prostředků hello. |
| Nástroje pro vyrovnávání zatížení seznamu |[Get-AzureRmLoadBalancer](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermloadbalancer) - ResourceGroupName $myResourceGroup<BR><BR>Zobrazí seznam všech hello nástroje pro vyrovnávání zatížení ve skupině prostředků hello. |
| Seznamu síťových rozhraní |[Get-AzureRmNetworkInterface](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermnetworkinterface) - ResourceGroupName $myResourceGroup<BR><BR>Zobrazí seznam všech síťových rozhraní hello ve skupině prostředků hello. |
| Získat informace o rozhraní sítě |Get-AzureRmNetworkInterface-Name "myNIC" - ResourceGroupName $myResourceGroup<BR><BR>Získá informace o konkrétním síťovém rozhraní. |
| Získat hello konfigurace protokolu IP síťového rozhraní |[Get-AzureRmNetworkInterfaceIPConfig](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermnetworkinterfaceipconfig) -Name "myNICIP" - NetworkInterface $nic<BR><BR>Získá informace o konfiguraci protokolu IP hello hello zadané síťové rozhraní. Hodnota Hello $nic představuje hello objekt vrácený rutinou Get-AzureRmNetworkInterface. |

## <a name="manage-network-resources"></a>Správa síťových prostředků

| Úkol | Příkaz |
| ---- | ------- |
| Přidání podsítě virtuální sítě tooa |[Přidat AzureRmVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/azurerm.network/add-azurermvirtualnetworksubnetconfig) - AddressPrefix XX. X.X.X/XX-Name "mySubnet1" - virtuální síť $vnet<BR><BR>Přidá podsíť tooan existující virtuální síť. Hodnota Hello $vnet představuje hello objekt vrácený rutinou Get-AzureRmVirtualNetwork. |
| Odstranění virtuální sítě |[Remove-AzureRmVirtualNetwork](https://docs.microsoft.com/powershell/module/azurerm.network/remove-azurermvirtualnetwork) -Name "myVNet" - ResourceGroupName $myResourceGroup<BR><BR>Odebere zadaný virtuální síťový hello ze skupiny prostředků hello. |
| Odstranit síťové rozhraní |[Odebrat AzureRmNetworkInterface](https://docs.microsoft.com/powershell/module/azurerm.network/remove-azurermnetworkinterface) -Name "myNIC" - ResourceGroupName $myResourceGroup<BR><BR>Odebere ze skupiny prostředků hello hello zadané síťové rozhraní. |
| Odstranění nástroje pro vyrovnávání zatížení |[Odebrat AzureRmLoadBalancer](https://docs.microsoft.com/powershell/module/azurerm.network/remove-azurermloadbalancer) -Name "myLoadBalancer" - ResourceGroupName $myResourceGroup<BR><BR>Odebere hello zadaný pro vyrovnávání zatížení ze skupiny prostředků hello. |
| Odstranit veřejnou IP adresu |[Odebrat AzureRmPublicIpAddress](https://docs.microsoft.com/powershell/module/azurerm.network/remove-azurermpublicipaddress)-Name "myIPAddress" - ResourceGroupName $myResourceGroup<BR><BR>Odebere hello zadat veřejnou IP adresu ze skupiny prostředků hello. |

## <a name="next-steps"></a>Další kroky
* Použití hello síťové rozhraní, kterou jste právě vytvořili, když jste [vytvoření virtuálního počítače](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Další informace o tom, jak můžete [vytvoření virtuálního počítače s více síťovými rozhraními](../../virtual-network/virtual-networks-multiple-nics.md).

