---
title: "aaaTroubleshoot trasy – portál | Microsoft Docs"
description: "Zjistěte, jak hello tootroubleshoot trasy v modelu nasazení Azure Resource Manager hello pomocí portálu Azure."
services: virtual-network
documentationcenter: na
author: AnithaAdusumilli
manager: narayan
editor: 
tags: azure-resource-manager
ms.assetid: bdd8b6dc-02fb-4057-bb23-8289caa9de89
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/23/2016
ms.author: anithaa
ms.openlocfilehash: 579bae91ef3200852032b3953d3cc5d26deada86
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-routes-using-hello-azure-portal"></a>Řešení potíží s postupy pomocí hello portálu Azure
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

1. Přihlášení toohello portál Azure na https://portal.azure.com.
2. Klikněte na tlačítko **další služby**, pak klikněte na tlačítko **virtuální počítače** v zobrazeném seznamu hello.
3. Vyberte virtuální počítač tootroubleshoot hello seznamu, které se zobrazí a zobrazí se okno virtuálních počítačů s možnostmi.
4. Klikněte na tlačítko **Diagnostikujte & řešení problémů** a pak vyberte častých problémů. V tomto příkladu **nelze se připojit toomy virtuální počítač s Windows** je vybrána.

    ![](./media/virtual-network-routes-troubleshoot-portal/image1.png)
5. Kroky se zobrazí pod hello problému, jak je znázorněno v následujícím obrázku hello:

    ![](./media/virtual-network-routes-troubleshoot-portal/image2.png)

    Klikněte na tlačítko *efektivní trasy* hello seznamu doporučené kroky.
6. Hello **efektivní trasy** okno se zobrazí, jak je znázorněno v následujícím obrázku hello:

    ![](./media/virtual-network-routes-troubleshoot-portal/image3.png)

    Pokud virtuální počítač má pouze jeden síťový adaptér, je standardně vybraná. Pokud máte více než jeden síťový adaptér, vyberte hello síťový adaptér, pro které chcete tooview hello efektivní trasy.

   > [!NOTE]
   > Pokud hello virtuálních počítačů spojené s hello síťový adaptér není ve spuštěném stavu, efektivní směrování, se nezobrazí. Hello prvních 200 efektivní tras se zobrazují pouze hello portálu. Úplný seznam hello, klikněte na tlačítko **Stáhnout**. Dále můžete filtrovat podle hello výsledky ze souboru CSV stažené hello.
   >
   >

    Všimněte si následujících hello ve výstupu hello:

   * **Zdroj**: Určuje typ hello trasy. Systémové trasy se zobrazují jako *výchozí*, udr se zobrazují jako *uživatele* a brány trasy (statické nebo BGP) se zobrazují jako *Brána VPN*.
   * **Stav**: označuje stav platná směrovací hello. Možné hodnoty jsou *Active* nebo *neplatný*.
   * **Adres**: Určuje předponu adresy hello hello efektivní trasy v notaci CIDR.
   * **nextHopType**: označuje hello další segment pro danou trasu hello. Možné hodnoty jsou *VirtualAppliance*, *Internet*, *VNetLocal*, *VNetPeering*, nebo *Null*. Hodnota *Null* pro **nextHopType** UDR může znamenat neplatná. Například pokud **nextHopType** je *VirtualAppliance* a hello sítě virtuální zařízení virtuálního počítače není zřízený nebo spuštěném stavu. Pokud **nextHopType** je *Brána VPN* a neexistuje žádná brána zřízený/běží v hello zadané virtuální síti, může zneplatní hello trasy.
7. Neexistuje žádná trasa uvedené toohello *WestUS VNET3* virtuální síti (předponu 10.10.0.0/16) ze hello *WestUS VNet1* (předpony 10.9.0.0/16) hello obrázku v předchozím kroku hello. V následujícím obrázku hello, hello partnerského vztahu odkaz bude v hello *odpojeno* stavu:

    ![](./media/virtual-network-routes-troubleshoot-portal/image4.png)

    Hello obousměrný odkaz pro partnerský vztah hello se přeruší, která vysvětluje, proč VM1 se nemohl připojit tooVM3 v hello *WestUS VNet3* virtuální sítě.
8. Hello následující obrázek znázorňuje hello trasy po vytvoření partnerského vztahu propojení obousměrný hello:

    ![](./media/virtual-network-routes-troubleshoot-portal/image5.png)

U scénářů s více řešení potíží pro vynucené tunelování a směrování vyhodnocení číst hello [aspekty](virtual-network-routes-troubleshoot-portal.md#considerations) tohoto článku.

### <a name="view-effective-routes-for-a-network-interface"></a>Zobrazit účinné postupy pro rozhraní sítě
Pokud pro konkrétní síťové rozhraní (NIC) je ovlivněn tok přenosu sítě, můžete zobrazit úplný seznam efektivní trasy přímo na síťový adaptér. toosee hello agregační tras, které jsou použité tooa síťového adaptéru, dokončení hello následující kroky:

1. Přihlášení toohello portál Azure na https://portal.azure.com.
2. Klikněte na tlačítko **další služby**, pak klikněte na tlačítko **síťová rozhraní**
3. Hledat seznam hello hello název síťového adaptéru, nebo ho vyberte ze seznamu hello, který se zobrazí. V tomto příkladu **VM1 NIC1** je vybrána.
4. Vyberte **efektivní trasy** v hello **síťové rozhraní** okno, jak je znázorněno v následujícím obrázku hello:

       ![](./media/virtual-network-routes-troubleshoot-portal/image6.png)

    Hello **oboru** vybrané výchozí nastavení toohello síťové rozhraní.

      ![](./media/virtual-network-routes-troubleshoot-portal/image7.png)

### <a name="view-effective-routes-for-a-route-table"></a>Zobrazení efektivní trasy pro směrovací tabulku
Při úpravě trasy definované uživatelem (udr) ve směrovací tabulce, můžete chtít tooreview hello dopad hello tras přidávané na konkrétním virtuálním počítači. Směrovací tabulka se dá přidružit libovolný počet podsítí. Nyní můžete zobrazit všechny hello efektivní trasy pro všechny síťové adaptéry, které se aplikují dané směrovací tabulku, hello bez nutnosti tooswitch kontext z hello zadané okno tabulky trasy.

V tomto příkladu UDR (*UDRoute*) je zadána ve směrovací tabulce (*UDRouteTable*). Tato trasa odesílá všechny internetové přenosy od *Subnet1* v hello *WestUS VNet1* virtuální sítě přes virtuální síťové zařízení (hodnocení chyb zabezpečení), v *Podsíť2* z hello stejnou virtuální síť. Hello trasy se zobrazí v hello následujícím obrázku:

![](./media/virtual-network-routes-troubleshoot-portal/image8.png)

trasy agregační toosee hello směrovací tabulka, dokončení hello následující kroky:

1. Přihlášení toohello portál Azure na https://portal.azure.com.
2. Klikněte na tlačítko **další služby**, pak klikněte na tlačítko **směrovacích tabulek**
3. Seznam hledání hello hello směrovací tabulka má toosee agregační trasy pro a vyberte jej. V tomto příkladu **UDRouteTable** je vybrána. Zobrazí se okno hello vybraný směrovací tabulka, jak je znázorněno v následujícím obrázku hello:

    ![](./media/virtual-network-routes-troubleshoot-portal/image9.png)
4. Vyberte **efektivní trasy** v hello **směrovací tabulku** okno. Hello **oboru** nastavena toohello směrovací tabulku jste vybrali.
5. Směrovací tabulka může být použité toomultiple podsítě. Vyberte hello **podsíť** chcete tooreview hello seznamu. V tomto příkladu **Subnet1** je vybrána.
6. Vyberte **síťové rozhraní**. Jsou uvedeny všechny síťové adaptéry připojené toohello vybrané podsítě. V tomto příkladu **VM1 NIC1** je vybrána.

    ![](./media/virtual-network-routes-troubleshoot-portal/image10.png)

   > [!NOTE]
   > Pokud hello síťový adaptér není přidružen spuštěný virtuální počítač, zobrazí se žádné efektivní trasy.
   >
   >

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
  * Skupina zabezpečení sítě (NSG) pravidla, které mohou ovlivňovat hello přenosové toky. Další informace najdete v tématu hello [odstraňování skupin zabezpečení sítě](virtual-network-nsg-troubleshoot-portal.md) článku.
