---
title: "aaaVirtual sítě a virtuální počítače s Windows v Azure | Microsoft Docs"
description: "Další informace o sítě, protože se týká toohello základy vytváření virtuální počítače s Windows v Azure."
services: virtual-machines-windows
documentationcenter: 
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 5493e9f7-7d45-4e98-be9a-657a53708746
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/17/2017
ms.author: davidmu
ms.openlocfilehash: e28a4782f9f6c69f6101e45dbb560ccd694a1e07
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="virtual-networks-and-windows-virtual-machines-in-azure"></a>Virtuální sítě a virtuální počítače s Windows v prostředí Azure 

Když vytváříte virtuální počítač Azure, musíte vytvořit [virtuální síť](../../virtual-network/virtual-networks-overview.md) (VNet), nebo použít existující VNet. Budete také potřebovat toodecide způsobu určený toobe na hello virtuální sítě virtuálních počítačů. Je důležité příliš[plán před vytvořením prostředky](../../virtual-network/virtual-network-vnet-plan-design-arm.md) a ujistěte se, že rozumíte hello [omezení síťové prostředky](../../azure-subscription-service-limits.md#networking-limits).

V hello následující obrázek virtuální počítače jsou vyjádřené webové servery a servery databáze. Každá sada virtuálních počítačů jsou přiřazené podsítě tooseparate hello virtuální sítě.

![Virtuální síť Azure](./media/network-overview/vnetoverview.png)

Můžete buď vytvořit virtuální síť před vytvoření virtuálního počítače nebo hello virtuální sítě můžete vytvořit při vytváření virtuálního počítače. Síť VNet můžete buď vytvořit sami, nebo si ji nechat nastavit při vytváření virtuálního počítače.

Můžete vytvořit tyto prostředky toosupport komunikace se virtuální počítač:

- Síťová rozhraní
- IP adresy
- Virtuální sítě a podsítě

Kromě toho toothose základní prostředky, měli byste také zvážit tyto volitelné prostředky:

- Skupiny zabezpečení sítě
- Nástroje pro vyrovnávání zatížení 

## <a name="network-interfaces"></a>Síťová rozhraní

A [síťové rozhraní (NIC)](../../virtual-network/virtual-network-network-interface.md) je hello propojení mezi virtuální počítač a virtuální sítí (VNet). Virtuální počítač musí mít alespoň jeden síťový adaptér, ale může mít více než jednou, v závislosti na velikosti hello hello vytvoříte virtuální počítač. Přečtěte si, kolik síťových rozhraní podporují jednotlivé velikosti virtuálních počítačů, v článku [Velikosti virtuálních počítačů v Azure](sizes.md). 

Pokud chcete toocreate virtuálního počítače s více než jeden síťový adaptér, je nutné vytvořit hello virtuálních počítačů s alespoň dvě.  Po vytvoření můžete přidat další síťové adaptéry telefonní číslo toohello nepodporuje hello velikost virtuálního počítače, ale nemůžete přidat další síťové adaptéry tooa virtuálního počítače vytvořit pouze s jedním, bez ohledu na to, kolik síťových podporuje hello velikost virtuálního počítače. 

Pokud hello virtuální počítač je přidaný tooan skupinu dostupnosti, musí mít všechny virtuální počítače v rámci sady dostupnosti hello jeden nebo více síťových adaptérů. Virtuální počítače s více než jeden síťový adaptér není požadovaná toohave hello stejný počet síťových adaptérů, ale jejich musí mít alespoň dva.

Každou síťovou kartu připojenou tooa virtuálních počítačů, musí existovat v hello stejné předplatné a umístění jako hello virtuálních počítačů. Každý síťový adaptér musí být připojené tooa virtuální síť, která existuje v hello stejné umístění Azure a předplatné jako hello síťový adaptér. Po vytvoření síťový adaptér můžete změnit hello podsíť, ke kterému je připojen k, ale nemůžete změnit hello je připojený k virtuální síti.  Každý síťový adaptér připojený tooa virtuálního počítače je přiřadit adresu MAC, který nemění, dokud je neodstraní hello virtuálních počítačů.

Tato tabulka uvádí hello metody, které můžete použít toocreate síťové rozhraní.

| Metoda | Popis |
| ------ | ----------- |
| portál Azure | Když vytvoříte virtuální počítač v hello portálu Azure, síťové rozhraní se automaticky vytvoří za vás (síťovou kartu vytvoříte samostatně nelze použít). Hello portál vytvoří virtuální počítač se jenom jeden síťový adaptér. Pokud chcete toocreate virtuálního počítače s více než jeden síťový adaptér, je třeba ji vytvořit s jinou metodu. |
| [Azure PowerShell](../../virtual-network/virtual-network-deploy-multinic-arm-ps.md) | Použití [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) s hello **- PublicIpAddressId** parametr tooprovide hello identifikátor hello veřejné IP adresy, která jste dříve vytvořili. |
| [Azure CLI](../../virtual-network/virtual-network-deploy-multinic-arm-cli.md) | identifikátor hello tooprovide hello veřejných IP adres, které dříve vytvořenou pomocí [vytvořit síťových adaptérů sítě az](https://docs.microsoft.com/cli/azure/network/nic#create) s hello **– veřejná adresa** parametr. |
| [Šablona](../../virtual-network/virtual-network-deploy-multinic-arm-template.md) | Jako vodítko při nasazování síťového rozhraní pomocí šablony použijte článek věnovaný [síťovému rozhraní ve virtuální síti s veřejnou IP adresou](https://github.com/Azure/azure-quickstart-templates/tree/master/101-nic-publicip-dns-vnet). |

## <a name="ip-addresses"></a>IP adresy 

Můžete přiřadit tyto typy [IP adresy](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) tooa síťový adaptér v Azure:

- **Veřejné IP adresy** -použít toocommunicate příchozí a odchozí (bez překlad síťových adres (NAT)) s hello Internetu a dalším prostředkům služby Azure Nepřipojeno tooa virtuální sítě. Přiřazení veřejnou IP adresu tooa síťový adaptér je volitelné. Za veřejné IP adresy se účtuje nominální poplatek a maximální počet těchto adres, které je možné použít pro předplatné, je omezený.
- **Privátní IP adresy** – používá pro komunikaci v rámci virtuální sítě, do místní sítě a hello Internet (s NAT). Je nutné přiřadit alespoň jeden privátní IP adresu tooa virtuálních počítačů. toolearn Další informace o NAT v Azure, přečtěte si [pochopení odchozí připojení v Azure](../../load-balancer/load-balancer-outbound-connections.md).

Můžete přiřadit veřejné IP adresy tooVMs nebo internetové služby Vyrovnávání zatížení. Můžete přiřadit privátní IP adresy tooVMs a interní služby load balancer. Můžete přiřadit IP adresy tooa virtuálního počítače pomocí rozhraní sítě.

Existují dvě metody, ve kterých se přidělit IP adresu tooa prostředku, dynamická nebo statická. Metoda přidělení výchozí Hello je dynamické, kde není přidělit IP adresu, když je vytvořeno. Místo toho se přidělit IP adresu hello po vytvoření virtuálního počítače nebo spuštění zastaveného virtuálního počítače. Hello IP adresa se neuvolní při zastavení nebo odstranit hello virtuálních počítačů. 

tooensure hello IP adresu pro virtuální počítač zůstane hello hello stejné, může explicitně nastavena metoda přidělení hello toostatic. V tomto případě se IP adresa přiřadí okamžitě. Jeho vydání jenom v případě, že odstraníte hello virtuálního počítače nebo změnit jeho toodynamic metoda přidělení.
    
Tato tabulka uvádí hello metody, které můžete použít toocreate IP adresu.

| Metoda | Popis |
| ------ | ----------- |
| [Azure Portal](../../virtual-network/virtual-network-deploy-static-pip-arm-portal.md) | Ve výchozím nastavení jsou dynamické veřejné IP adresy a hello adrese přiřazené toothem může změnit při zastavení nebo odstranit hello virtuálních počítačů. tooguarantee, který hello virtuálního počítače vždy používá hello stejnou veřejnou IP adresu, vytvořit statickou veřejnou IP adresu. Ve výchozím nastavení přiřadí hello portál dynamické privátní IP adresu tooa seskupování při vytváření virtuálního počítače. Tato toostatic můžete změnit po hello je vytvořena virtuálních počítačů.|
| [Azure PowerShell](../../virtual-network/virtual-network-deploy-static-pip-arm-ps.md) | Používáte [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) s hello **- AllocationMethod** parametr jako dynamické nebo statické. |
| [Azure CLI](../../virtual-network/virtual-network-deploy-static-pip-arm-cli.md) | Používáte [vytvoření veřejné sítě az-ip](https://docs.microsoft.com/cli/azure/network/public-ip#create) s hello **– metoda přidělení** parametr jako dynamické nebo statické. |
| [Šablona](../../virtual-network/virtual-network-deploy-static-pip-arm-template.md) | Jako vodítko při nasazování veřejné IP adresy pomocí šablony použijte článek věnovaný [síťovému rozhraní ve virtuální síti s veřejnou IP adresou](https://github.com/Azure/azure-quickstart-templates/tree/master/101-nic-publicip-dns-vnet). |

Po vytvoření veřejné IP adresy, je možné přidružit ho virtuální počítač přiřazením tooa síťový adaptér.

## <a name="virtual-network-and-subnets"></a>Virtuální sítě a podsítě

Podsíť je rozsah IP adres v hello virtuální sítě. Virtuální síť můžete z organizačních a bezpečnostních důvodů rozdělit do několika podsítí. Každý síťový adaptér ve virtuálním počítači je připojených tooone podsítě v jedné virtuální sítě. Síťové adaptéry připojené toosubnets (stejných nebo různých) v rámci virtuální sítě můžete vzájemně komunikovat bez jakékoli další konfigurace.

Když nastavíte virtuální síť, určíte hello topologie, včetně hello k dispozici adresní prostory a podsítě. Pokud hello virtuální sítě je tooother toobe připojené virtuální sítě nebo místní sítě, je nutné vybrat rozsahy adres, které nepřekrývaly. Hello IP adresy jsou soukromá a není přístupný z Internetu, která byla platí pouze pro hello-routeable IP adresy, např. 10.0.0.0/8, 172.16.0.0/12 nebo 192.168.0.0/16 hello. Azure teď zpracovává všechny rozsah adres v rámci hello privátní virtuální síť adresní prostor IP adres, který je dostupný v rámci hello virtuální sítě, v rámci vzájemně propojena virtuálních sítí a z místní umístění. 

Pokud pracujete v rámci organizace, ve kterém někdo je zodpovědná za hello interní sítě, musí komunikovat toothat osoba před výběrem adresní prostor. Zajistěte, aby nedocházelo k překrytí a dát jim vědět, místo hello chcete toouse, nemusíte snaží toouse hello stejný rozsah IP adres. 

Ve výchozím nastavení, není žádná hranice zabezpečení mezi podsítěmi, takže virtuální počítače v každé z těchto podsítí může kontaktovat tooone jiné. Však můžete nastavit skupiny zabezpečení sítě (Nsg), což umožní toocontrol hello provoz toku tooand z podsítí a tooand z virtuálních počítačů. 

Tato tabulka uvádí hello metody, které můžete toocreate virtuální sítě a podsítě.   

| Metoda | Popis |
| ------ | ----------- |
| [Azure Portal](../../virtual-network/virtual-networks-create-vnet-arm-pportal.md) | Pokud necháte Azure vytvořit virtuální síť, když vytvoření virtuálního počítače, název hello je kombinací hello název skupiny prostředků, který obsahuje hello virtuální sítě a **- vnet**. Hello adresní prostor je 10.0.0.0/24, je název požadované podsítě hello **výchozí**, a rozsah adres podsítě hello je 10.0.0.0/24. |
| [Azure PowerShell](../../virtual-network/virtual-networks-create-vnet-arm-ps.md) | Používáte [New-AzureRmVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/resourcemanager/AzureRM.Network/v1.0.13/New-AzureRmVirtualNetworkSubnetConfig) a [New-AzureRmVirtualNetwork](https://docs.microsoft.com/powershell/resourcemanager/AzureRM.Network/v1.0.13/New-AzureRmVirtualNetwork) toocreate podsíť a síť VNet. Můžete také použít [přidat AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/add-azurermvirtualnetworksubnetconfig) tooadd tooan podsíť existující virtuální síť. |
| [Azure CLI](../../virtual-network/virtual-networks-create-vnet-arm-cli.md) | Hello podsíť a hello virtuální sítě se vytvářejí v hello stejný čas. Zadejte **– název podsítě** parametr příliš[vytvoření sítě vnet az](https://docs.microsoft.com/cli/azure/network/vnet#create) s hello název podsítě. |
| [Šablona](../../virtual-network/virtual-networks-create-vnet-arm-template-click.md) | Hello nejjednodušší způsob, jak toocreate virtuální sítě a podsítě, jako je toodownload existující šablony, [virtuální sítě se dvěma podsítěmi](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets)a upravit pro vaše potřeby. |

## <a name="network-security-groups"></a>Skupiny zabezpečení sítě

A [skupinu zabezpečení sítě (NSG)](../../virtual-network/virtual-networks-nsg.md) obsahuje seznam pravidel seznamu řízení přístupu (ACL), která povolují nebo odepírají toosubnets provoz sítě, síťové adaptéry nebo obojí. Skupiny Nsg můžou být spojené s podsítí nebo jednotlivé síťové adaptéry připojené tooa podsítě. Pokud je skupina NSG přidružená k podsíti, pravidla seznamu ACL hello platí tooall hello virtuální počítače v této podsíti. Kromě toho provozu tooan jednotlivých seskupování se dá omezit tím, že přidružíte skupinu NSG přímo tooa síťový adaptér.

Skupiny NSG obsahují dvě sady pravidel: příchozí a odchozí. Hello Priorita pravidla musí být jedinečný v rámci jednotlivých sad. Každé pravidlo má vlastnosti, které popisují protokol, zdrojový a cílový rozsah portů, předpony adres, směr přenosu, prioritu a typ přístupu. 

Všechny skupiny NSG obsahují sadu výchozích pravidel. nelze odstranit Hello výchozí pravidla, ale protože je jim přiřazená nejnižší priorita hello, dají se přepsat pravidly hello, které vytvoříte. 

Pokud přidružíte NSG tooa síťový adaptér, pravidla přístupu k síti hello v hello NSG jsou použité pouze toothat síťový adaptér. Pokud je skupina NSG použitá tooa jeden síťový adaptér na virtuálním počítači více síťovými Kartami, nemá vliv na provoz toohello ostatních síťových karet. Můžete přidružit jiné skupiny Nsg tooa síťového adaptéru (nebo virtuálních počítačů, v závislosti na modelu nasazení hello) a hello podsítě, která je vázána síťovou kartu nebo virtuální počítač. U hello směr provozu se přidělí na základě priority.

Ujistěte se, příliš[plán](../../virtual-network/virtual-networks-nsg.md#planning) skupin Nsg při plánování virtuální počítače a virtuální sítě.

Tato tabulka uvádí hello metody, které můžete použít toocreate skupinu zabezpečení sítě.

| Metoda | Popis |
| ------ | ----------- |
| [Azure Portal](../../virtual-network/virtual-networks-create-nsg-arm-pportal.md) | Při vytváření virtuálního počítače v hello portálu Azure, se automaticky vytvoří skupinu NSG a vytvoří přidružené toohello seskupování hello portálu. Název Hello hello NSG je kombinací hello název hello virtuálních počítačů a **- nsg**. Tato skupina NSG obsahuje jedno příchozí pravidlo s prioritou 1 000, tooRDP sady služby, tooTCP sadu hello protokol, port nastavený too3389 a akce nastavená tooAllow. Pokud chcete tooallow všechny ostatní příchozí provoz toohello virtuálních počítačů, je nutné přidat toohello další pravidla NSG. |
| [Azure PowerShell](../../virtual-network/virtual-networks-create-nsg-arm-ps.md) | Použití [New-AzureRmNetworkSecurityRuleConfig](https://docs.microsoft.com/powershell/resourcemanager/AzureRM.Network/v1.0.13/New-AzureRmNetworkSecurityRuleConfig) a poskytnout informace o hello požadované pravidlo. Použití [New-AzureRmNetworkSecurityGroup](https://docs.microsoft.com/powershell/resourcemanager/AzureRM.Network/v1.0.13/New-AzureRmNetworkSecurityGroup) toocreate hello NSG. Použití [Set-AzureRmVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/resourcemanager/AzureRM.Network/v1.0.13/Set-AzureRmVirtualNetworkSubnetConfig) tooconfigure hello skupina NSG pro podsíť hello. Použití [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork) tooadd hello NSG toohello virtuální sítě. |
| [Azure CLI](../../virtual-network/virtual-networks-create-nsg-arm-cli.md) | Použití [vytvořit az sítě nsg](https://docs.microsoft.com/cli/azure/network/nsg#create) tooinitially vytvořit hello NSG. Použití [vytvořit pravidla nsg sítě az](https://docs.microsoft.com/cli/azure/network/nsg/rule#create) tooadd pravidla toohello NSG. Použití [aktualizace az sítě vnet podsíť](https://docs.microsoft.com/en-us/cli/azure/network/vnet/subnet#update) tooadd hello NSG toohello podsítě. |
| [Šablona](../../virtual-network/virtual-networks-create-nsg-arm-template.md) | Jako vodítko při nasazování skupiny zabezpečení sítě pomocí šablony použijte článek věnovaný [vytvoření skupiny zabezpečení sítě](https://github.com/Azure/azure-quickstart-templates/tree/master/101-security-group-create). |

## <a name="load-balancers"></a>Nástroje pro vyrovnávání zatížení

[Azure nástroj pro vyrovnávání zatížení](../../load-balancer/load-balancer-overview.md) zajišťuje vysoké dostupnosti a výkonu v síti tooyour aplikace. Nástroj pro vyrovnávání zatížení lze nakonfigurovat příliš[vyvážit příchozí přenosy z Internetu](../../load-balancer/load-balancer-internet-overview.md) tooVMs nebo [vyrovnávat přenosy mezi virtuálními počítači ve virtuální síti](../../load-balancer/load-balancer-internal-overview.md). Nástroj pro vyrovnávání zatížení můžou vyrovnávat také komunikaci mezi počítači místní a virtuální počítače v síti mezi různými místy, nebo přesměrovat provoz externí tooa konkrétní virtuální počítač.

zatížení Hello vyrovnávání mapy příchozí a odchozí provoz mezi hello veřejnou IP adresu a port na hello nástroj pro vyrovnávání zatížení a hello privátní IP adresu a port hello virtuálních počítačů.

Když vytvoříte nástroj pro vyrovnávání zatížení, musíte taky zvážit tyto konfigurační prvky:

- **Konfigurace front-endových IP adres** – Nástroj pro vyrovnávání zatížení může zahrnovat jednu nebo několik front-endových IP adres, které se taky označují jako virtuální IP adresy (VIP). Tyto IP adresy sloužit jako příchozího provozu hello.
- **Fond back-end adres** – IP adresy, které jsou přidružené hello seskupování toowhich zatížení je distribuován.
- **Pravidla NAT** -definuje jak příchozí provoz prochází hello front-end IP adresu a distribuované toohello back-end IP adresy.
- **Pravidla pro vyrovnávání zatížení** -mapuje danou front-end IP adresu a port kombinace tooa sadu back-end IP adresy a portu. Nástroj pro vyrovnávání zatížení může mít několik pravidel vyrovnávání zatížení. Každé pravidlo je kombinací front-endové IP adresy a portu a back-endové IP adresy a portu přidružených k virtuálním počítačům.
- **[Sondy](../../load-balancer/load-balancer-custom-probe-overview.md)**  -hello monitorování stavu virtuálních počítačů. Když sondu selže toorespond, nástroj pro vyrovnávání zatížení hello zastaví odesílání toohello nové připojení virtuálního počítače není v pořádku. ovlivněné nejsou Hello existující připojení a nová připojení posílají toohealthy virtuálních počítačů.

Tato tabulka uvádí hello metody, které můžete použít vyrovnávání zatížení internetového toocreate.

| Metoda | Popis |
| ------ | ----------- |
| portál Azure | Nelze vytvořit aktuálně Vyrovnávání zatížení internetového pomocí hello portálu Azure. |
| [Azure PowerShell](../../load-balancer/load-balancer-get-started-internet-arm-ps.md) | identifikátor hello tooprovide hello veřejných IP adres, které dříve vytvořenou pomocí [New-AzureRmLoadBalancerFrontendIpConfig](https://docs.microsoft.com/powershell/resourcemanager/AzureRM.Network/v1.0.13/New-AzureRmLoadBalancerFrontendIpConfig) s hello **- PublicIpAddress** parametr. Použití [New-AzureRmLoadBalancerBackendAddressPoolConfig](https://docs.microsoft.com/powershell/resourcemanager/AzureRM.Network/v1.0.13/New-AzureRmLoadBalancerBackendAddressPoolConfig) toocreate hello konfigurace fondu adres back-end hello. Použití [New-AzureRmLoadBalancerInboundNatRuleConfig](https://docs.microsoft.com/powershell/resourcemanager/AzureRM.Network/v1.0.13/New-AzureRmLoadBalancerInboundNatRuleConfig) toocreate příchozí pravidla NAT přidružené hello front-endové konfigurace protokolu IP, který jste vytvořili. Použití [New-AzureRmLoadBalancerProbeConfig](https://docs.microsoft.com/powershell/resourcemanager/AzureRM.Network/v1.0.13/New-AzureRmLoadBalancerProbeConfig) toocreate hello testy, je nutné. Použití [New-AzureRmLoadBalancerRuleConfig](https://docs.microsoft.com/powershell/resourcemanager/AzureRM.Network/v1.0.13/New-AzureRmLoadBalancerRuleConfig) konfigurace služby Vyrovnávání zatížení toocreate hello. Použití [New-AzureRmLoadBalancer](/powershell/module/azurerm.network/new-azurermloadbalancer) nástroj pro vyrovnávání zatížení toocreate hello.|
| [Azure CLI](../../load-balancer/load-balancer-get-started-internet-arm-cli.md) | Použití [az sítě lb vytvořit](https://docs.microsoft.com/cli/azure/network/lb#create) konfigurace služby Vyrovnávání zatížení počáteční toocreate hello. Použití [az sítě lb ip front-endu-vytvořit](https://docs.microsoft.com/cli/azure/network/lb/frontend-ip#create) tooadd hello veřejnou IP adresu, která jste předtím vytvořili. Použití [az lb fond síťových adres-vytvořit](https://docs.microsoft.com/cli/azure/network/lb/address-pool#create) tooadd hello konfigurace fondu adres back-end hello. Použití [vytvořit az sítě lb příchozí--pravidlo nat](https://docs.microsoft.com/cli/azure/network/lb/inbound-nat-rule#create) pravidla NAT tooadd. Použití [vytvořit pravidlo vyrovnáváním zatížení sítě az](https://docs.microsoft.com/cli/azure/network/lb/rule#create) pravidla nástroje pro vyrovnávání zatížení tooadd hello. Použití [vytvoření testu vyrovnáváním zatížení sítě az](https://docs.microsoft.com/cli/azure/network/lb/probe#create) sondy tooadd hello. |
| [Šablona](../../load-balancer/load-balancer-get-started-internet-arm-template.md) | Použití [2 virtuální počítače v Vyrovnávání zatížení a nakonfigurovat pravidla NAT na hello LB](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-loadbalancer-natrules) jako vodítko pro nasazení pomocí šablony služby Vyrovnávání zatížení. |
    
Tato tabulka uvádí hello metody, které můžete použít toocreate interní nástroj.

| Metoda | Popis |
| ------ | ----------- |
| portál Azure | Nelze vytvořit aktuálně interní nástroj používá hello portálu Azure. |
| [Azure PowerShell](../../load-balancer/load-balancer-get-started-ilb-arm-ps.md) | tooprovide privátní IP adresy v podsíti hello sítě, použijte [New-AzureRmLoadBalancerFrontendIpConfig](https://docs.microsoft.com/powershell/resourcemanager/AzureRM.Network/v1.0.13/New-AzureRmLoadBalancerFrontendIpConfig) s hello **- PrivateIpAddress** parametr. Použití [New-AzureRmLoadBalancerBackendAddressPoolConfig](https://docs.microsoft.com/powershell/resourcemanager/AzureRM.Network/v1.0.13/New-AzureRmLoadBalancerBackendAddressPoolConfig) toocreate hello konfigurace fondu adres back-end hello. Použití [New-AzureRmLoadBalancerInboundNatRuleConfig](https://docs.microsoft.com/powershell/resourcemanager/AzureRM.Network/v1.0.13/New-AzureRmLoadBalancerInboundNatRuleConfig) toocreate příchozí pravidla NAT přidružené hello front-endové konfigurace protokolu IP, který jste vytvořili. Použití [New-AzureRmLoadBalancerProbeConfig](https://docs.microsoft.com/powershell/resourcemanager/AzureRM.Network/v1.0.13/New-AzureRmLoadBalancerProbeConfig) toocreate hello testy, je nutné. Použití [New-AzureRmLoadBalancerRuleConfig](https://docs.microsoft.com/powershell/resourcemanager/AzureRM.Network/v1.0.13/New-AzureRmLoadBalancerRuleConfig) konfigurace služby Vyrovnávání zatížení toocreate hello. Použití [New-AzureRmLoadBalancer](/powershell/module/azurerm.network/new-azurermloadbalancer) nástroj pro vyrovnávání zatížení toocreate hello.|
| [Azure CLI](../../load-balancer/load-balancer-get-started-ilb-arm-cli.md) | Použití hello [az sítě lb vytvořit](https://docs.microsoft.com/cli/azure/network/lb#create) konfigurace služby Vyrovnávání zatížení počáteční hello toocreate příkaz. toodefine hello privátní IP adresu, použijte [az sítě lb ip front-endu-vytvořit](https://docs.microsoft.com/cli/azure/network/lb/frontend-ip#create) s hello **– privátní. ip adresa** parametr. Použití [az lb fond síťových adres-vytvořit](https://docs.microsoft.com/cli/azure/network/lb/address-pool#create) tooadd hello konfigurace fondu adres back-end hello. Použití [vytvořit az sítě lb příchozí--pravidlo nat](https://docs.microsoft.com/cli/azure/network/lb/inbound-nat-rule#create) pravidla NAT tooadd. Použití [vytvořit pravidlo vyrovnáváním zatížení sítě az](https://docs.microsoft.com/cli/azure/network/lb/rule#create) pravidla nástroje pro vyrovnávání zatížení tooadd hello. Použití [vytvoření testu vyrovnáváním zatížení sítě az](https://docs.microsoft.com/cli/azure/network/lb/probe#create) sondy tooadd hello.|
| [Šablona](../../load-balancer/load-balancer-get-started-ilb-arm-template.md) | Použití [2 virtuální počítače v Vyrovnávání zatížení a nakonfigurovat pravidla NAT na hello LB](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-internal-load-balancer) jako vodítko pro nasazení pomocí šablony služby Vyrovnávání zatížení. |

## <a name="vms"></a>Virtuální počítače

Virtuální počítače lze vytvořit v hello stejné virtuální síť a můžete připojit tooeach jiných použití privátních IP adres. Se můžete připojit, i když jsou v různých podsítích bez nutnosti tooconfigure hello bránu nebo použít veřejné IP adresy. tooput virtuální počítače do virtuální sítě, můžete vytvořit hello virtuální síť a pak vytvoříte každý virtuální počítač, můžete je přiřadit toohello virtuální síť a podsíť. Virtuální počítače získají svoje síťové nastavení sítě během nasazení nebo spuštění.  

IP adresa se virtuálním počítačům přiřazuje v okamžiku jejich nasazení. Pokud do virtuální sítě nebo podsítě nasadíte několik virtuálních počítačů, IP adresy se jim přiřazují při spouštění. Dynamickou IP adresu (DIP) je hello interní IP adresu přidruženou virtuálního počítače. Můžete přidělit statickou tooa vyhrazené IP adresy virtuálních počítačů. Pokud přidělíte statické vyhrazené IP adresy, měli byste zvážit použití určité podsíti tooavoid, náhodně opakovaného použití statické vyhrazené IP adresy pro jiný virtuální počítač.  

Pokud chcete vytvořit virtuální počítač a později chcete toomigrate ho do virtuální sítě, není změna jednoduché konfigurace. Je nutné znovu nasadit hello virtuálních počítačů do hello virtuální sítě. Nejjednodušší způsob, jak tooredeploy Hello je toodelete hello virtuálních počítačů, ale nejsou žádné disky připojené tooit a pak znovu vytvořte hello virtuální počítač pomocí hello původní disky v hello virtuální sítě. 

Tato tabulka uvádí metody hello používané toocreate virtuálního počítače ve virtuální síti.

| Metoda | Popis |
| ------ | ----------- |
| [Azure Portal](../virtual-machines-windows-hero-tutorial.md) | Používá hello výchozí sítě nastavení, které byly dříve uvedených toocreate virtuální počítač s jeden síťový adaptér. toocreate virtuálního počítače s více síťovými kartami, je nutné použít jinou metodu. |
| [Azure PowerShell](../virtual-machines-windows-ps-create.md) | Zahrnuje použití hello [přidat AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface) tooadd hello síťový adaptér, který jste vytvořili dříve toohello konfigurace virtuálního počítače. |
| [Šablona](ps-template.md) | Jako vodítko při nasazování virtuálního počítače pomocí šablony použijte článek věnovaný [velmi jednoduchému nasazení virtuálního počítače s Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows). |

## <a name="next-steps"></a>Další kroky

- Zjistěte, jak tooconfigure [trasy definované uživatelem a předávání IP](../../virtual-network/virtual-networks-udr-overview.md). 
- Zjistěte, jak tooconfigure [připojení mezi sítěmi VNet tooVNet](../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md).
- Zjistěte, jak příliš[řešení potíží s trasy](../../virtual-network/virtual-network-routes-troubleshoot-portal.md).
