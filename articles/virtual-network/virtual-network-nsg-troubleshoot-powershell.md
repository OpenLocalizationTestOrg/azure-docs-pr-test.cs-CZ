---
title: "aaaTroubleshoot skupin zabezpečení sítě - prostředí PowerShell | Microsoft Docs"
description: "Zjistěte, jak tootroubleshoot skupin zabezpečení sítě v nasazení Azure Resource Manager hello modelu pomocí Azure PowerShell."
services: virtual-network
documentationcenter: na
author: AnithaAdusumilli
manager: narayan
editor: 
tags: azure-resource-manager
ms.assetid: 4c732bb7-5cb1-40af-9e6d-a2a307c2a9c4
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/23/2016
ms.author: anithaa
ms.openlocfilehash: 95fd60fa72cf6d17fa990e3c3eb7d980878f7c15
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-network-security-groups-using-azure-powershell"></a>Řešení potíží s skupin zabezpečení sítě pomocí prostředí Azure PowerShell
> [!div class="op_single_selector"]
> * [Azure Portal](virtual-network-nsg-troubleshoot-portal.md)
> * [PowerShell](virtual-network-nsg-troubleshoot-powershell.md)
> 
> 

Pokud jste nakonfigurovali skupiny zabezpečení sítě (Nsg) ve virtuálním počítači (VM) a dochází problémy s připojením k virtuální počítač, tento článek obsahuje přehled možností diagnostiky pro řešení potíží s toohelp další skupiny Nsg.

Skupiny Nsg umožňují toocontrol hello typy přenosů s tokem do aplikace a z virtuálních počítačů (VM). Skupiny Nsg můžou být použité toosubnets ve virtuální síti Azure (VNet), síťová rozhraní (NIC) nebo obojí. Hello efektivní pravidla použít tooa síťový adaptér jsou hello pravidla, které existují v hello skupiny Nsg použije tooa síťových Adaptérů a hello podsíť, ve které je připojený k agregaci. Pravidla v rámci těchto skupin Nsg můžete někdy vzájemném konfliktu a mít vliv na připojení k síti Virtuálního počítače.  

Můžete zobrazit všechna pravidla efektivní zabezpečení hello od vaší skupiny Nsg použije na síťové adaptéry Virtuálního počítače. Tento článek ukazuje, jak hello problémy s připojením k tootroubleshoot virtuálních počítačů pomocí těchto pravidel v modelu nasazení Azure Resource Manager. Pokud si nejste obeznámeni s virtuální sítí a NSG koncepty, přečtěte si hello [virtuální síť](virtual-networks-overview.md) a [skupin zabezpečení sítě](virtual-networks-nsg.md) přehled články.

## <a name="using-effective-security-rules-tootroubleshoot-vm-traffic-flow"></a>Pomocí tok přenosů dat virtuálního počítače tootroubleshoot efektivní pravidla zabezpečení
Hello scénář, který následuje je příkladem častých problémů připojení:

Virtuální počítač s názvem *VM1* je součástí podsíť s názvem *Subnet1* v rámci virtuální sítě s názvem *WestUS VNet1*. Pokusu o tooconnect toohello virtuálního počítače pomocí protokolu RDP přes TCP port 3389 selže. Skupiny Nsg se použijí v obou hello seskupování *VM1 NIC1* a hello podsíť *Subnet1*. V hello NSG přidružená hello síťové rozhraní je povolen provoz tooTCP portu 3389 *VM1 NIC1*, ale TCP příkazem ping otestovat tooVM1 na portu 3389 selže.

Při tomto příkladu používá TCP port 3389, hello následující postup se dá použít toodetermine selhání příchozí a odchozí připojení prostřednictvím libovolný port.

## <a name="detailed-troubleshooting-steps"></a>Podrobné kroky řešení potíží
Proveďte následující kroky tootroubleshoot skupiny Nsg pro virtuální počítač hello:

1. Spusťte tooAzure relaci a přihlaste se prostředí Azure PowerShell. Pokud si nejste obeznámeni s používáním Azure PowerShell, přečtěte si hello [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) článku.
2. Zadejte následující příkaz tooreturn všechna pravidla NSG použije tooa síťový adaptér s názvem hello *VM1 NIC1* ve skupině prostředků hello *RG1*:
   
        Get-AzureRmEffectiveNetworkSecurityGroup -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1
   
   > [!TIP]
   > Pokud neznáte název hello síťového adaptéru, zadejte následující příkaz tooretrieve hello názvy všech síťových adaptérů ve skupině prostředků hello: 
   > 
   > `Get-AzureRmNetworkInterface -ResourceGroupName RG1 | Format-Table Name`
   > 
   > 
   
    Hello následující text je ukázkový výstup hello efektivní pravidla pro hello *VM1 NIC1* síťovou kartu:
   
        NetworkSecurityGroup   : {
                                   "Id": "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/networkSecurityGroups/VM1-NIC1-NSG"
                                 }
        Association            : {
                                   "NetworkInterface": {
                                     "Id": "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/networkInterfaces/VM1-NIC1"
                                   }
                                 }
        EffectiveSecurityRules : [
                                 {
                                 "Name": "securityRules/allowRDP",
                                 "Protocol": "Tcp",
                                 "SourcePortRange": "0-65535",
                                 "DestinationPortRange": "3389-3389",
                                 "SourceAddressPrefix": "Internet",
                                 "DestinationAddressPrefix": "0.0.0.0/0",
                                 "ExpandedSourceAddressPrefix": [… ],
                                 "ExpandedDestinationAddressPrefix": [],
                                 "Access": "Allow",
                                 "Priority": 1000,
                                 "Direction": "Inbound"
                                 },
                                 {
                                 "Name": "defaultSecurityRules/AllowVnetInBound",
                                 "Protocol": "All",
                                 "SourcePortRange": "0-65535",
                                 "DestinationPortRange": "0-65535",
                                 "SourceAddressPrefix": "VirtualNetwork",
                                 "DestinationAddressPrefix": "VirtualNetwork",
                                 "ExpandedSourceAddressPrefix": [
                                  "10.9.0.0/16",
                                  "168.63.129.16/32",
                                  "10.0.0.0/16",
                                  "10.1.0.0/16"
                                  ],
                                 "ExpandedDestinationAddressPrefix": [
                                  "10.9.0.0/16",
                                  "168.63.129.16/32",
                                  "10.0.0.0/16",
                                  "10.1.0.0/16"
                                  ],
                                  "Access": "Allow",
                                  "Priority": 65000,
                                  "Direction": "Inbound"
                                  },…
                         ]
   
        NetworkSecurityGroup   : {
                                   "Id": 
                                 "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/networkSecurityGroups/Subnet1-NSG"
                                 }
        Association            : {
                                   "Subnet": {
                                     "Id": 
                                 "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/virtualNetworks/WestUS-VNet1/subnets/Subnet1"
                                 }
                                 }
        EffectiveSecurityRules : [
                                 {
                                "Name": "securityRules/denyRDP",
                                "Protocol": "Tcp",
                                "SourcePortRange": "0-65535",
                                "DestinationPortRange": "3389-3389",
                                "SourceAddressPrefix": "Internet",
                                "DestinationAddressPrefix": "0.0.0.0/0",
                                "ExpandedSourceAddressPrefix": [
                                   ... ],
                                "ExpandedDestinationAddressPrefix": [],
                                "Access": "Deny",
                                "Priority": 1000,
                                "Direction": "Inbound"
                                },
                                {
                                "Name": "defaultSecurityRules/AllowVnetInBound",
                                "Protocol": "All",
                                "SourcePortRange": "0-65535",
                                "DestinationPortRange": "0-65535",
                                "SourceAddressPrefix": "VirtualNetwork",
                                "DestinationAddressPrefix": "VirtualNetwork",
                                "ExpandedSourceAddressPrefix": [
                                "10.9.0.0/16",
                                "168.63.129.16/32",
                                "10.0.0.0/16",
                                "10.1.0.0/16"
                                ],
                                "ExpandedDestinationAddressPrefix": [
                                "10.9.0.0/16",
                                "168.63.129.16/32",
                                "10.0.0.0/16",
                                "10.1.0.0/16"
                                ],
                                "Access": "Allow",
                                "Priority": 65000,
                                "Direction": "Inbound"
                                },...
                                ]
   
    Vezměte na vědomí následující informace ve výstupu hello hello:
   
   * Existují dva **skupinu zabezpečení sítě** částech: jeden je přidružená k podsíti (*Subnet1*) a jeden síťový adaptér přidružený (*VM1 NIC1*). V tomto příkladu byla skupina NSG použitá tooeach.
   * **Přidružení** ukazuje hello prostředků (podsíti nebo síťové KARTĚ) je přidružen daný NSG. Pokud hello prostředek NSG přesunout nebo zrušit přidružení bezprostředně před spuštěním tohoto příkazu, může být nutné toowait pár sekund pro změnu tooreflect hello ve výstupu příkazu hello. 
   * Hello názvy pravidel, které začínají *defaultSecurityRules*: když NSG se vytvoří, jsou v něm vytvořit několik výchozí pravidla zabezpečení. Výchozí pravidla nelze odebrat, ale je možné přepsat s vyšší prioritou pravidla.
     Čtení hello [NSG přehled](virtual-networks-nsg.md#default-rules) článku toolearn Další informace o NSG výchozí pravidla zabezpečení.
   * **ExpandedAddressPrefix** rozšíří hello předpony adres pro NSG výchozí značky. Značky představují více předponami adresy. Rozšíření značek hello může být užitečné při řešení potíží s připojením virtuálního počítače z konkrétní předpony. Například s partnerský vztah virtuální síť, značka VIRTUAL_NETWORK rozšíří tooshow peered předponami virtuální sítě v předchozí výstup hello.
     
     > [!NOTE]
     > příkaz pouze ukazuje efektivní pravidla Hello, pokud je skupina NSG přidružená podsíť, síťový adaptér nebo obojí. Virtuální počítač může mít více síťových adaptérů se použít odlišné skupiny Nsg. Při odstraňování problémů s, spusťte příkaz hello pro každý síťový adaptér.
     > 
     > 
3. tooease filtrování přes větší počet pravidel NSG, zadejte následující příkazy tootroubleshoot další hello: 
   
        $NSGs = Get-AzureRmEffectiveNetworkSecurityGroup -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1
        $NSGs.EffectiveSecurityRules | Sort-Object Direction, Access, Priority | Out-GridView
   
    Filtr pro provoz protokolu RDP (TCP port 3389), je použita toohello zobrazení mřížky, jak je znázorněno v následujícím obrázku hello:
   
    ![Seznam pravidel](./media/virtual-network-nsg-troubleshoot-powershell/rules.png)
4. Jak můžete vidět v zobrazení mřížky hello, jsou obě povolit a zakázat pravidla pro protokol RDP. Hello výstup z kroku 2 ukazuje, že hello *DenyRDP* pravidlo je v hello skupina NSG použitá toohello podsítě. Pro příchozí pravidla Nsg použije toohello podsítě jsou zpracován jako první. Pokud je nalezena shoda, nebude zpracován hello skupina NSG použitá toohello síťové rozhraní. V takovém případě hello *DenyRDP* pravidlo z podsítě hello blokuje RDP toohello virtuálních počítačů (**VM1**).
   
   > [!NOTE]
   > Virtuální počítač může mít více síťových adaptérů připojených tooit. Každý může být připojené tooa jiné podsíti. Vzhledem k tomu, že hello příkazy v předchozích krocích hello se spouštějí na síťový adaptér, je důležité tooensure, který zadáte hello seskupování, budete mít hello připojení se nezdařilo. Pokud si nejste jisti, můžete vždy spustit hello příkazy pro každou síťovou kartu připojenou toohello virtuálních počítačů.
   > 
   > 
5. tooRDP do VM1, změna hello *odepřít protokolu RDP (3389)* pravidlo příliš*povolit RDP(3389)* v hello **Subnet1 NSG** NSG. Zkontrolujte, zda TCP port 3389 otevřete otevřením toohello připojení RDP virtuálního počítače nebo pomocí nástroje Pspingu hello. Další informace o Pspingu ve čtení hello [stránky pro stažení Pspingu](https://technet.microsoft.com/sysinternals/psping.aspx)
   
    Můžete nebo odebrání pravidla skupinu NSG pomocí hello informace v hello výstup hello následující příkaz:
   
        Get-Help *-AzureRmNetworkSecurityRuleConfig

## <a name="considerations"></a>Požadavky
Mějte na paměti následující body při řešení potíží s potíže s připojením k hello:

* Výchozí pravidla NSG se blokují příchozí přístup z hello Internetu a povolení jenom virtuální síť příchozí přenosy. Pravidla musí být explicitně přidán tooallow příchozí přístup z Internetu, podle potřeby.
* Pokud neexistují žádná pravidla zabezpečení NSG způsobuje toofail připojení k síti Virtuálního počítače, může být problém hello příčiny:
  * Software brány firewall běžící v rámci operačního systému hello Virtuálního počítače
  * Trasy nakonfigurované pro virtuální zařízení nebo místní provoz. Přenosy z Internetu může být přesměrovaného tooon místní prostřednictvím vynucené tunelování. Připojení k protokolu RDP/SSH z Internetu tooyour hello virtuálních počítačů nemusí fungovat s tímto nastavením, v závislosti na tom, jak hello místní síťový hardware zpracovává tento provoz. Čtení hello [řešení potíží s trasy](virtual-network-routes-troubleshoot-powershell.md) toolearn článku jak toodiagnose trasy problémy, které může být zpomalovat hello tok přenosů dat do a z hello virtuálních počítačů. 
* Pokud máte peered virtuálních sítí, ve výchozím nastavení, rozbalte hello VIRTUAL_NETWORK značky automaticky tooinclude předpon pro peered virtuální sítě. Tyto předpony můžete zobrazit v hello **ExpandedAddressPrefix** seznamu, tootroubleshoot všechny problémy související tooVNet partnerský vztah připojení. 
* Efektivní zabezpečení pravidla se zobrazují pouze pokud je, že skupina NSG přidružená hello Virtuálního počítače síťový adaptér a nebo podsíť. 
* Pokud neexistují žádné skupiny Nsg přidružená hello síťový adaptér nebo podsíť a jestli máte veřejnou IP adresu přiřadit tooyour virtuálních počítačů, budou všechny porty otevřené pro příchozí a odchozí přístup. Pokud hello virtuálních počítačů má veřejnou IP adresu, použití skupin Nsg toohello síťový adaptér nebo podsíť, důrazně doporučujeme.  

