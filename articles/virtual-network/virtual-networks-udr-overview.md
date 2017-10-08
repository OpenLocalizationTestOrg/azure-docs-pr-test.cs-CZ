---
title: "trasy definované uživatelem aaaUser a předávání IP v Azure | Microsoft Docs"
description: "Zjistěte, jak tooconfigure trasy definované uživatelem (UDR) a předávání IP adres tooforward provozu toonetwork virtuálních zařízení v Azure."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
ms.assetid: c39076c4-11b7-4b46-a904-817503c4b486
ms.service: virtual-network
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f1f1d46166d5a7c776f472b7ade1354d943ece10
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="user-defined-routes-and-ip-forwarding"></a>Uživatelem definované trasy a předávání IP

Když přidáte virtuální počítače (VM) tooa virtuální síť (VNet) v Azure, si všimnete hello virtuální počítače automaticky se možné toocommunicate mezi sebou přes síť hello. Není nutné toospecify bránu, i když hello virtuální počítače jsou v různých podsítích. Hello totéž platí pro komunikaci z virtuálních počítačů toohello hello veřejného Internetu a dokonce tooyour do místní sítě při hybridní připojení z Azure tooyour vlastní datacenter je k dispozici.

Tento tok komunikace je možné, protože Azure pomocí řady systémových tras toodefine toky provozu IP. Systémové trasy řídí tok hello komunikace v hello následující scénáře:

* V nástroji hello stejné podsíti.
* Z podsítě tooanother v rámci virtuální sítě.
* Z virtuálních počítačů toohello Internetu.
* Z virtuální síť tooanother virtuální sítě prostřednictvím brány sítě VPN.
* Z virtuální sítě tooanother virtuální sítě prostřednictvím sítě VNet partnerského vztahu (řetězení služby).
* Z virtuální sítě tooyour místní sítě prostřednictvím brány sítě VPN.

Hello obrázek níže znázorňuje jednoduché uspořádání s virtuální síť a dvě podsítě, několika virtuálními počítači, společně s hello systémové trasy, které umožňují tooflow provozu IP.

![Systémové trasy v Azure](./media/virtual-networks-udr-overview/Figure1.png)

Ačkoli použití systémových tras hello automaticky usnadňuje provoz vašeho nasazení, existují případy, ve kterých chcete toocontrol hello směrování paketů prostřednictvím virtuálního zařízení. Můžete tak tak, že vytvoříte trasy definované uživatelem, který zadáte hello další segment směrování paketů místo předávaných tooa určité podsíti toogo tooyour virtuální zařízení a povolení IP předávání hello virtuálnímu počítači spuštěnému jako virtuální zařízení hello.

Hello obrázek níže znázorňuje příklad tras definovaných uživatelem a předávání paketů tooforce IP odeslány tooone podsíť z jiné toogo procházet virtuálním zařízením ve třetí podsíti.

![Systémové trasy v Azure](./media/virtual-networks-udr-overview/Figure2.png)

> [!IMPORTANT]
> Trasy definované uživatelem jsou použité tootraffic z podsítě z libovolného zdroje (například síťových rozhraní připojených tooVMs) v podsíti hello. Nelze vytvořit trasy toospecify jak provoz zadá podsíť z Internetu, hello pro instanci. Hello zařízení jsou dál provoz toocannot být v hello stejné podsíti, kde hello provoz pochází. Pro svoje zařízení vždycky vytvořte samostatnou podsíť. 
> 
> 

## <a name="route-resource"></a>Prostředek trasy
Se pakety směrují přes síť TCP/IP podle směrovací tabulky definované v každém uzlu ve fyzické síti hello. Směrovací tabulka je že kolekce jednotlivých tras používá toodecide, kde tooforward pakety na základě hello cílové IP adresy. Trasa se skládá z následujících hello:

| Vlastnost | Popis | Omezení | Požadavky |
| --- | --- | --- | --- |
| Předpona adresy |Hello cílové CIDR toowhich hello trasa vztahuje, například 10.1.0.0/16. |Musí být platný rozsah CIDR, který reprezentuje adresy ve hello veřejný Internet, virtuální síť Azure nebo místního datového centra. |Ujistěte se, zda text hello **předpona adresy** neobsahuje adresu hello hello **adresa dalšího směrování**, jinak se pakety dostanou do smyčky z hello zdroj toohello další segment nikdy nedorazí Cílový Hello. |
| Typ dalšího segmentu |Hello typ směrování Azure hello paketu by měly být odeslány na. |Musí být jedna z hello následující hodnoty: <br/> **Virtuální síť**. Představuje místní virtuální síť hello. Například pokud máte dvě podsítě, 10.1.0.0/16 a 10.2.0.0/16 v hello stejné virtuální síti, hello trasa každé podsítě ve směrovací tabulce hello bude mít hodnotu dalšího segmentu z *virtuální sítě*. <br/> **Brána virtuální sítě**. Představuje bránu Azure S2S VPN Gateway. <br/> **Internet.** Představuje hello výchozí internetovou bránu poskytovanou podle hello infrastruktury Azure. <br/> **Virtuální zařízení.** Představuje virtuální zařízení jste přidali tooyour virtuální síť Azure. <br/> **Žádný**. Představuje černou díru. Pakety předávané tooa černá díra nebudou předávat vůbec. |Zvažte použití **virtuální zařízení** toodirect provozu tooa virtuálního počítače nebo služby Vyrovnávání zatížení Azure interní IP adresu.  Tento typ umožňuje specifikaci hello adresy IP, jak je popsáno níže. Zvažte použití **žádné** zadejte toostop pakety z průchodu tooa zadané cílové. |
| Adresa dalšího segmentu |Adresa dalšího směrování Hello obsahuje hello IP adresu, kterou mají předávat pakety. Hodnoty dalšího směrování jsou povolené jenom v trasách, kde je typ dalšího přechodu hello *virtuální zařízení*. |Musí být IP adresa, která je dostupná v rámci hello virtuální sítě, kdy se používá hello trasy definované uživatelem, aniž by bylo nutné **brány virtuální sítě**. Hello IP adresa má toobe na hello stejné virtuální sítě v případě, že je použito, nebo na peered virtuální sítě. |Pokud hello IP adresa představuje virtuální počítač, ujistěte se, povolíte [předávání IP](#IP-forwarding) v Azure hello virtuálních počítačů. Pokud hello IP adresa představuje hello interní IP adresy Vyrovnávání zatížení Azure, ujistěte se, že máte odpovídající pravidlo pro každý port Vyrovnávání zatížení chcete vyrovnávat tooload.|

V prostředí Azure PowerShell některé hodnoty "NextHopType" hello mají odlišné názvy:

* Virtuální síť je VnetLocal
* Brána virtuální sítě je VirtualNetworkGateway
* Virtuální zařízení je VirtualAppliance
* Internet je Internet
* Žádný je žádný

### <a name="system-routes"></a>Systémové trasy
Každá podsíť vytvořená ve virtuální síti se automaticky přidruží k směrovací tabulce, která obsahuje hello následující pravidla systémových tras:

* **Pravidlo místní virtuální sítě:** Toto pravidlo se automaticky vytvoří pro každou podsíť ve virtuální síti. Určuje, že je přímé propojení mezi virtuálními počítači hello v hello virtuální sítě a neexistuje žádný zprostředkující další segment.
* **Místní pravidlo**: Toto pravidlo platí tooall přenosy určené toohello rozsah adres pro místní a jako cíl dalšího segmentu hello používá bránu sítě VPN.
* **Internetové pravidlo**: Toto pravidlo zpracovává všechny přenosy určené toohello veřejného Internetu (0.0.0.0/0 předpony adres) a internetovou bránu infrastruktury používá hello jako hello dalšího směrování pro všechny přenosy určené toohello Internetu.

### <a name="user-defined-routes"></a>Trasy definované uživatelem
Pro většinu prostředí potřebujete jenom systémové trasy hello už v Azure definované. Můžete však nutné toocreate směrovací tabulku a přidejte jeden nebo více trasy v určitých případech, například:

* Vynucené tunelování toohello Internetu prostřednictvím místní sítě.
* Použití virtuálních zařízení v prostředí Azure.

Ve scénářích hello výše bude mít toocreate směrovací tabulku a přidat tooit trasy definované uživatelem. Můžete mít víc směrovacích tabulek a hello stejná směrovací tabulka může být přidružené tooone nebo další podsítě. A každou podsíť může být přidružené tooa jedné směrovací tabulce. Všechny virtuální počítače a cloudové služby v podsíti, použijte hello trasy tabulku asociovanou toothat podsíť.

Podsítě spoléhají na systémové trasy, dokud směrovací tabulka je přidružené toohello podsítě. Jakmile existuje přidružení, směrování se provádí na základě nejdelší shody předpony (LPM) jak mezi trasami definovanými uživateli, tak mezi systémovými trasami. Pokud existuje víc tras s hello stejné LPM odpovídat pak trasa se vybere na základě původu v hello následující pořadí:

1. Trasa definovaná uživatelem
2. Trasa protokolu BGP (pokud se používá služba ExpressRoute)
3. Systémová trasa

toolearn způsobu toocreate uživatelem definované trasy, najdete v [jak tooCreate tras a povolení předávání IP v Azure](virtual-network-create-udr-arm-template.md).

> [!IMPORTANT]
> Trasy definované uživatelem jsou pouze použité tooAzure virtuální počítače a cloudové služby. Například pokud chcete, aby tooadd virtuální zařízení brány firewall mezi místní sítí a Azure, budete mít toocreate trasu definovanou uživatelem pro směrovací tabulky Azure, který předává veškerý provoz směřující toohello místní adresu místa toohello virtuální zařízení. Můžete také přidat, že uživatel definované trasy (UDR) toohello GatewaySubnet tooforward veškerý provoz z místní tooAzure prostřednictvím hello virtuální zařízení. Toto je nedávný dodatek.
> 
> 

### <a name="bgp-routes"></a>Trasy protokolu BGP
Pokud máte spojení ExpressRoute mezi místní sítí a Azure, můžete povolit toopropagate trasy protokolu BGP z vaší místní síti tooAzure. Tyto trasy protokolu BGP se používají v hello stejným způsobem jako systémové trasy a uživatel trasy definované v jednotlivých podsítích Azure. Další informace najdete v tématu [Úvod do služby ExpressRoute](../expressroute/expressroute-introduction.md).

> [!IMPORTANT]
> Můžete nakonfigurovat vaše prostředí Azure toouse vynucené tunelování prostřednictvím místní sítě tak, že vytvoříte trasu definovanou uživatelem pro podsíť 0.0.0.0/0, který používá bránu sítě VPN hello jako další segment hello. To ale funguje jenom v případě, že používáte bránu sítě VPN, nikoli službu ExpressRoute. U služby ExpressRoute se vynucené tunelování konfiguruje prostřednictvím BGP.
> 
> 

## <a name="ip-forwarding"></a>Předávání IP
Jak je popsáno výše, jedním z hlavních důvodů toocreate hello trasu definovanou uživatelem je tooforward provoz tooa virtuální zařízení. Virtuální zařízení není nic jiného než virtuální počítač, který běží aplikace používá toohandle síťový provoz nějakým způsobem, jako je například Brána firewall nebo zařízením NAT.

Tato virtuální zařízení, virtuálních počítačů musí být schopný tooreceive příchozí provoz, který nebyly upraveny tooitself. tooallow přenosy virtuálních počítačů tooreceive řešit tooother cíle, je nutné povolit předávání IP adres pro hello virtuálních počítačů. Toto je nastavení, nikoli nastavení hello hostovaného operačního systému Azure.

## <a name="next-steps"></a>Další kroky
* Zjistěte, jak příliš[vytvářet trasy v modelu nasazení Resource Manager hello](virtual-network-create-udr-arm-template.md) a přidružovat je toosubnets. 
* Zjistěte, jak příliš[vytvářet trasy v modelu nasazení classic hello](virtual-network-create-udr-classic-ps.md) a přidružovat je toosubnets.

