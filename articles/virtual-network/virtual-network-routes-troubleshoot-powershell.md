---
title: "aaaTroubleshoot trasy – prostředí PowerShell | Microsoft Docs"
description: "Zjistěte, jak tootroubleshoot směruje v modelu nasazení Azure Resource Manager hello pomocí Azure PowerShell."
services: virtual-network
documentationcenter: na
author: AnithaAdusumilli
manager: narayan
editor: 
tags: azure-resource-manager
ms.assetid: bf7dc5e7-9399-460e-8e0d-8992dbed98a6
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/23/2016
ms.author: anithaa
ms.openlocfilehash: 7a07806df5c1d0caee921187e6ad29f6755ab535
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-routes-using-azure-powershell"></a>Řešení potíží s postupy pomocí prostředí Azure PowerShell
> [!div class="op_single_selector"]
> * [Azure Portal](virtual-network-routes-troubleshoot-portal.md)
> * [PowerShell](virtual-network-routes-troubleshoot-powershell.md)
> 
> 

Máte-li tooor problémy síťové připojení z vaší virtuální počítač Azure (VM), tras, které mohou ovlivňovat vaše datové přenosy virtuálních počítačů. Tento článek obsahuje přehled možností diagnostiky pro trasy toohelp pokračovat v řešení potíží.

Směrovací tabulky jsou spojeny s podsítěmi a platí na všech síťových rozhraní (NIC) v této podsíti. následující typy tras Hello může být použité tooeach síťové rozhraní:

* **Systémové trasy:** ve výchozím nastavení, má každá podsíť vytvořená ve virtuální síti Azure (VNet) systému směrovací tabulky, které umožňují místní provoz VNet, místní provoz prostřednictvím brány sítě VPN a přenosy z Internetu. Systémové trasy existovat také pro peered virtuální sítě.
* **Trasy protokolu BGP:** rozšíří toonetwork rozhraní prostřednictvím ExpressRoute nebo VPN připojení site-to-site. Další informace o směrování protokolu BGP načtením hello [protokol BGP s bránami VPN](../vpn-gateway/vpn-gateway-bgp-overview.md) a [přehled ExpressRoute](../expressroute/expressroute-introduction.md) články.
* **Trasy definované uživatelem (UDR):** použití virtuálních zařízení sítě nebo jsou vynutit tunelové propojení provoz tooan do místní sítě prostřednictvím sítě site-to-site VPN, může mít trasy definované uživatelem (udr) přidružené tabulky tras podsítě. Pokud si nejste obeznámeni s udr, přečtěte si hello [trasy definované uživatelem](virtual-networks-udr-overview.md#user-defined-routes) článku.

S hello různé trasy, které můžou být použité tooa síťového rozhraní, může být obtížné toodetermine které agregační trasy jsou platné. toohelp řešení potíží s připojení k síti virtuálních počítačů, můžete zobrazit všechny hello efektivní trasy pro rozhraní sítě v modelu nasazení Azure Resource Manager hello.

## <a name="using-effective-routes-tootroubleshoot-vm-traffic-flow"></a>Pomocí tok přenosů dat virtuálního počítače tootroubleshoot efektivní trasy
Tento článek používá hello podle scénáře jako tooillustrate příklad, jak tootroubleshoot hello platné tras pro síťové rozhraní:

Virtuální počítač (*VM1*) připojené toohello virtuální síť (*VNet1*, předpona: 10.9.0.0/16) selže tooconnect tooa VM(VM3) ve virtuální síti nově peered (*VNet3*, předpony 10.10.0.0/16). Neexistují žádné udr nebo BGP směruje použité tooVM1 NIC1 síťové rozhraní připojené toohello virtuálních počítačů, se použijí jenom systémové trasy.

Tento článek vysvětluje, jak toodetermine hello způsobit selhání připojení hello, pomocí funkce efektivní trasy v modelu nasazení správou prostředků Azure.
Příklad hello používá jenom systémové trasy, hello stejný postup lze použít toodetermine selhání příchozí a odchozí připojení přes jakýkoli typ trasy.

> [!NOTE]
> Pokud virtuální počítač má více než jeden síťový adaptér připojený, zkontrolujte efektivní trasy pro každou hello tooand problémy síťové adaptéry toodiagnose síťové připojení z virtuálního počítače.
> 
> 

### <a name="view-effective-routes-for-a-virtual-machine"></a>Zobrazit účinné postupy pro virtuální počítač
toosee hello agregační tras, které jsou použité tooa virtuálních počítačů, dokončení hello následující kroky:

### <a name="view-effective-routes-for-a-network-interface"></a>Zobrazit účinné postupy pro rozhraní sítě
toosee hello agregační tras, které jsou použité tooa síťového rozhraní, dokončení hello následující kroky:

1. Spusťte tooAzure relaci a přihlaste se prostředí Azure PowerShell. Pokud si nejste obeznámeni s prostředím Azure PowerShell, přečtěte si hello [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) článku.
2. Hello následující příkaz vrátí všechny trasy použít tooa síťové rozhraní s názvem *VM1 NIC1* ve skupině prostředků hello *RG1*.
   
       Get-AzureRmEffectiveRouteTable -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1
   
   > [!TIP]
   > Pokud neznáte název hello síťového rozhraní, zadejte následující příkaz tooretrieve hello názvy všech síťových rozhraní v group.* prostředků hello
   > 
   > 
   
       Get-AzureRmNetworkInterface -ResourceGroupName RG1 | Format-Table Name
   
   Hello následující výstup vypadá podobně jako výstup toohello pro každý trasy použít toohello podsíť hello síťový adaptér je připojen k:
   
       Name :
       State : Active
       AddressPrefix : {10.9.0.0/16}
       NextHopType : VNetLocal
       NextHopIpAddress : {}
   
       Name :
       State : Active
       AddressPrefix : {0.0.0.0/16}
       NextHopType : Internet
       NextHopIpAddress : {}
   
   Všimněte si následujících hello ve výstupu hello:
   
   * **Název**: název platná směrovací hello může být prázdný, pokud není výslovně uvedeno, pro trasy definované uživatelem. 
   * **Stav**: označuje stav platná směrovací hello. Možné hodnoty jsou "Aktivní" nebo "Neplatná"
   * **Adres**: Určuje předponu adresy hello hello efektivní trasy v notaci CIDR. 
   * **nextHopType**: označuje hello další segment pro danou trasu hello. Možné hodnoty jsou *VirtualAppliance*, *Internet*, *VNetLocal*, *VNetPeering*, nebo *Null*. Hodnota *Null* pro **nextHopType** UDR může znamenat neplatná. Například pokud **nextHopType** je *VirtualAppliance* a hello sítě virtuální zařízení virtuálního počítače není zřízený nebo spuštěném stavu. Pokud **nextHopType** je *Brána VPN* a neexistuje žádná brána zřízený/běží v hello zadané virtuální síti, může zneplatní hello trasy.
   * **NextHopIpAddress**: Určuje hello IP adresa dalšího směrování hello hello efektivní trasy.
   
   Hello následující příkaz vrátí hello trasy v jednodušší tooview tabulce:
   
       Get-AzureRmEffectiveRouteTable -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1 | Format-Table
   
   Hello následující výstup je některý obdržel hello scénář popsaný dřív výstup hello:
   
       Name State AddressPrefix NextHopType NextHopIpAddress
       ---- ----- ------------- ----------- ----------------
       Active {10.9.0.0/16} VnetLocal {}
       Active {0.0.0.0/0} Internet {}
3. Neexistuje žádná trasa uvedené toohello *WestUS VNet3* virtuální síť (předpony 10.10.0.0/16)** z *WestUS VNet1* (předpony 10.9.0.0/16) ve výstupu hello hello v předchozím kroku. Jak ukazuje následující obrázek hello, hello partnerského vztahu propojení virtuální sítě s hello *WestUS VNet3* virtuální sítě je v hello *odpojeno* stavu.
   
    ![](./media/virtual-network-routes-troubleshoot-portal/image4.png)
   
    Hello obousměrný odkaz pro partnerský vztah hello se přeruší, která vysvětluje, proč VM1 se nemohl připojit tooVM3 v hello *WestUS VNet3* virtuální sítě. Instalační program partnerského vztahu odkaz obousměrný virtuální síť znovu pro *WestUS VNet1* a *WestUS VNet3* virtuální sítě. výstup Hello po správně vytvoření partnerského vztahu propojení virtuální sítě hello takto:
   
        Name State AddressPrefix NextHopType NextHopIpAddress
        ---- ----- ------------- ----------- ----------------
        Active {10.9.0.0/16} VnetLocal {}
        Active {10.10.0.0/16} VNetPeering {}
        Active {0.0.0.0/0} Internet {}
   
    Po určení hello problém, můžete přidat, odebrat, nebo změňte trasy a směrovacích tabulek. Zadejte následující příkaz toosee seznam hello příkazy používané toodo tak hello:
   
        Get-Help *-AzureRmRouteConfig

## <a name="considerations"></a>Požadavky
Vrátí pár věcí tookeep pamatujte při revizi hello seznam tras:

* Směrování je založena na nejdelší shody předpony (LPM) mezi udr, směrování protokolu BGP a systému. Pokud existuje víc tras s hello stejné LPM shodují, pak trasa se vybere na základě původu v hello následující pořadí:
  
  * Trasy definované uživatelem
  * Trasa protokolu BGP
  * Trasy systému (výchozí)
    
    S efektivní směrování uvidí jenom účinné postupy, které jsou založené na všechny dostupné trasy hello shodou LPM. Ukazuje, jak se ve skutečnosti vyhodnocují hello trasy pro daný síťový adaptér, proto je mnohem snazší konkrétní trasy tootroubleshoot, které mohou ovlivňovat připojení z virtuálního počítače.
* Pokud máte udr a odesílání přenosů tooa sítě virtuální zařízení (hodnocení chyb zabezpečení), s *VirtualAppliance* jako **nextHopType**, ujistěte se, zda je povoleno předávání IP na provoz přijímající hello hello hodnocení chyb zabezpečení nebo dojde ke ztrátě paketů. 
* Pokud vynuceném tunelovém propojení je povoleno, bude veškerý odchozí internetový provoz směrovaný tooon místní. Připojení RDP/SSH z Internetu tooyour, kterou virtuální počítač nemusí fungovat s tímto nastavením, v závislosti na tom, jak hello místní zpracovává tento provoz. 
  Můžete povolit vynucené tunelování:
  * Pokud připojení site-to-site VPN pomocí nastavení trasy definované uživatelem (UDR) s nextHopType jako brány sítě VPN
  * Pokud je výchozí trasy inzerované přes protokol BGP
* Pro virtuální síť správně, partnerský vztah toowork provozu systému trasa se **nextHopType** *VNetPeering* pro hello peered rozsahu předpony virtuální sítě, musí existovat. Pokud tyto trasy neexistuje a hello partnerský vztah virtuální síť odkaz vypadá v pořádku:
  * Počkejte několik sekund a opakovat, pokud se jedná o nově vytvořeným partnerského vztahu odkaz. Někdy trvá déle, toopropagate trasy tooall hello síťových rozhraní v podsíti.
  * Skupina zabezpečení sítě (NSG) pravidla, které mohou ovlivňovat hello přenosové toky. Další informace najdete v tématu hello [odstraňování skupin zabezpečení sítě](virtual-network-nsg-troubleshoot-powershell.md) článku.

