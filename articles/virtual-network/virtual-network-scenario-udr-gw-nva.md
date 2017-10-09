---
title: "aaaHybrid připojení k aplikaci na vrstvě 2 | Microsoft Docs"
description: "Zjistěte, jak toodeploy virtuální zařízení a UDR toocreate prostředí vícevrstvé aplikace v Azure"
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
ms.assetid: 1f509bec-bdd1-470d-8aa4-3cf2bb7f6134
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/05/2016
ms.author: jdial
ms.openlocfilehash: 53599862289dbf9c6ab3289b0cb8dda6430f20f7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="virtual-appliance-scenario"></a>Scénář virtuální zařízení
Běžný scénář mezi větší Azure zákazníků je nutné tooprovide hello Dvojúrovňová aplikaci vystavenou toohello Internetu, současně back úroveň přístupu můžete toohello z překážek místní datacentra. Tento dokument vás provede procesem scénář používající uživatele definované trasy (UDR), brána sítě VPN a toodeploy virtuální zařízení sítě dvouvrstvé prostředí, které splňuje hello následující požadavky:

* Webové aplikace musí být dostupný z hello pouze veřejného Internetu.
* Webové aplikace hello hostitelský server musí být schopný tooaccess aplikační server back-end.
* Všechny přenosy z Internetu toohello webové aplikace hello musí projít virtuální zařízení brány firewall. Tato virtuální zařízení se použije pro jenom přenosy z Internetu.
* Veškerý provoz směřující toohello aplikační server musí projít virtuální zařízení brány firewall. Tato virtuální zařízení se použije pro přístup toohello back-end end serverů a přístup brzo z hello do místní sítě prostřednictvím brány VPN.
* Správci musí být schopný toomanage hello brány firewall virtuální zařízení ze své místní počítače, pomocí brány firewall třetí virtuální zařízení používané výhradně pro účely správy.

Toto je standardní scénář DMZ s DMZ a chráněné síti. Takový scénář lze sestavit v Azure pomocí skupin Nsg, virtuální zařízení brány firewall nebo jejich kombinaci. Hello tabulce jsou uvedeny některé hello výhody a nevýhody mezi skupiny Nsg a virtuální zařízení brány firewall.

|  | Odborníci na | Nevýhody |
| --- | --- | --- |
| SKUPINA NSG |Bez nákladů. <br/>Integrovaná do Azure RBAC. <br/>Pravidla je možné vytvořit v šablony ARM. |Složitost může lišit v prostředích větší. |
| Brána firewall |Plnou kontrolu nad rovinu data. <br/>Centrální správa prostřednictvím konzoly brány firewall. |Náklady na zařízení brány firewall. <br/>Není integrované s Azure RBAC. |

řešení Hello níže používá tooimplement virtuální zařízení brány firewall scénáři hraniční sítě nebo chráněné sítě.

## <a name="considerations"></a>Požadavky
Můžete nasadit hello prostředí popsané výše v Azure pomocí různých funkcí, které jsou k dispozici dnes, a to takto.

* **Virtuální síť (VNet)**. O virtuální síť Azure funguje v podobným způsobem tooan do místní sítě a mohou být segmentovány na jeden nebo více podsítí izolace provozu tooprovide a oddělené oblasti zájmu.
* **Virtuální zařízení**. Několik partneři poskytovat virtuálních zařízení v Azure Marketplace, které je možné pro tři výše popsané brány firewall v hello hello. 
* **Uživatelem definované trasy (UDR)**. Směrovací tabulky mohou obsahovat udr používá Azure sítě toocontrol hello toku paketů v rámci virtuální sítě. Tyto směrovací tabulky může být použité toosubnets. Jedna z funkcí nejnovější hello v Azure je hello možnost tooapply toohello tabulky trasy GatewaySubnet, poskytuje možnost tooforward hello veškerý provoz přicházející do hello virtuální síť Azure ze virtuální zařízení tooa hybridní připojení.
* **Předávání IP**. Ve výchozím nastavení hello Azure sítě modul předávání paketů toovirtual karty síťového rozhraní (NIC) pouze v případě, že hello paketu cílová IP adresa odpovídá hello seskupování IP adresu. Proto pokud UDR definuje, musí se poslat paket tooa zadané virtuální zařízení, hello Azure sítě modul by vyřadit paketu. paket hello tooensure se doručí tooa virtuálního počítače (v tomto případě virtuální zařízení), který není hello skutečné cíl pro hello paketů, musíte tooenable předávání IP adres pro virtuální zařízení hello.
* **Skupin zabezpečení (Nsg) sítě**. Následující příklad Hello není nutné používat skupiny Nsg, ale můžete použít skupiny Nsg použije toohello podsítí a síťových karet v toto řešení toofurther filtrovat hello provoz do/z těchto podsítí a síťových karet.

![Připojení pomocí protokolu IPv6](./media/virtual-network-scenario-udr-gw-nva/figure01.png)

V tomto příkladu je předplatné, které obsahuje hello následující:

* 2 skupin prostředků, není vidět v diagramu hello. 
  * **ONPREMRG**. Obsahuje všechny prostředky potřebné toosimulate do místní sítě.
  * **AZURERG**. Obsahuje všechny prostředky potřebné pro prostředí hello virtuální síť Azure. 
* Virtuální síť s názvem **onpremvnet** používá toomimic datacentrum místní segmentované, jak je uvedeno dále.
  * **onpremsn1**. Podsíť obsahující virtuálního počítače (VM) se systémem Ubuntu toomimic na místním serveru.
  * **onpremsn2**. Podsíť obsahující virtuálního počítače s Ubuntu toomimic místním počítači použít správce.
* Neexistuje jeden virtuální zařízení brány firewall s názvem **OPFW** na **onpremvnet** toomaintain tunel používá příliš**azurevnet**.
* Virtuální síť s názvem **azurevnet** segmentované, jak je uvedeno dále.
  * **azsn1**. Externí brány firewall podsítě používané výhradně pro externí brány firewall hello. Všechny přenosy z Internetu se mají prostřednictvím této podsíti. Tato podsíť obsahuje jenom síťový adaptér propojené toohello externí brány firewall.
  * **azsn2**. Virtuální počítač spuštěn jako webový server, který bude přístupná z Internetu hello hostování podsítě front end.
  * **azsn3**. Back-end podsítě, který je hostitelem virtuálního počítače s back-end serveru aplikace, kterou budou přistupovat hello front-end webový server.
  * **azsn4**. Používá správu podsíť výhradně tooprovide správy přístupu tooall brány firewall virtuální zařízení. Tato podsíť obsahuje jenom síťovou kartu pro každý virtuální zařízení brány firewall použité v řešení hello.
  * **GatewaySubnet**. Azure hybridní připojení podsíť požadované pro ExpressRoute a VPN Gateway tooprovide připojení mezi virtuálními sítěmi Azure a další sítě. 
* Existují 3 virtuálních zařízení brány firewall v hello **azurevnet** sítě. 
  * **AZF1**. Externí brány firewall zveřejněné toohello veřejného Internetu pomocí prostředek veřejné IP adresy v Azure. Je nutné tooensure máte šablonu z hello Marketplace nebo přímo ze svého zařízení dodavatele, že zřizuje o 3-NIC virtuální zařízení.
  * **AZF2**. Vnitřní brána firewall používá toocontrol provoz mezi **azsn2** a **azsn3**. Toto je také 3-NIC virtuální zařízení.
  * **AZF3**. Správa brány firewall přístupný tooadministrators z hello místního datového centra a používá podsíť správu připojené tooa toomanage všech zařízení brány firewall. Můžete najít 2-NIC šablony virtuální zařízení v hello Marketplace, nebo požádejte jeden přímo od dodavatele zařízení.

## <a name="user-defined-routing-udr"></a>Uživatelem definované směrování (UDR)
Každá podsíť v Azure může být propojené tooa UDR tabulka použitá toodefine, jak se směruje provoz v této podsíti inicioval. Pokud jsou definovány žádné udr, Azure používá výchozí trasy tooallow provoz tooflow z jedné podsítě tooanother. toobetter pochopit udr, navštivte [co jsou trasy definované uživatelem a předávání IP](virtual-networks-udr-overview.md#ip-forwarding).

tooensure komunikace probíhá prostřednictvím zařízení brány firewall správné hello, založené na hello poslední požadavek výše, budete potřebovat následující hello toocreate směrovací tabulka obsahující udr v **azurevnet**.

### <a name="azgwudr"></a>azgwudr
V tomto scénáři hello provoz z místní tooAzure se brány firewall hello použité toomanage připojením příliš pouze**AZF3**, a že komunikace musí procházet přes hello interní brány firewall, **AZF2**. Proto je nutné v hello pouze jeden postup **GatewaySubnet** jak je uvedeno níže.

| Cíl | Další směrování | Vysvětlení |
| --- | --- | --- |
| 10.0.4.0/24 |10.0.3.11 |Umožňuje místní brány firewall správy tooreach provoz **AZF3** |

### <a name="azsn2udr"></a>azsn2udr
| Cíl | Další směrování | Vysvětlení |
| --- | --- | --- |
| 10.0.3.0/24 |10.0.2.11 |Umožňuje podsíť back-end toohello provozu hostování hello aplikační server prostřednictvím **AZF2** |
| 0.0.0.0/0 |10.0.2.10 |Umožňuje všechny toobe provoz směrován přes **AZF1** |

### <a name="azsn3udr"></a>azsn3udr
| Cíl | Další směrování | Vysvětlení |
| --- | --- | --- |
| 10.0.2.0/24 |10.0.3.10 |Umožňuje přenos příliš**azsn2** tooflow z aplikace server toohello webový server prostřednictvím **AZF2** |

Musíte taky toocreate směrovací tabulky pro podsítě hello **onpremvnet** toomimic hello místního datového centra.

### <a name="onpremsn1udr"></a>onpremsn1udr
| Cíl | Další směrování | Vysvětlení |
| --- | --- | --- |
| 192.168.2.0/24 |192.168.1.4 |Umožňuje přenos příliš**onpremsn2** prostřednictvím **OPFW** |

### <a name="onpremsn2udr"></a>onpremsn2udr
| Cíl | Další směrování | Vysvětlení |
| --- | --- | --- |
| 10.0.3.0/24 |192.168.2.4 |Umožňuje podsíť toohello zálohovaný provozu v Azure prostřednictvím **OPFW** |
| 192.168.1.0/24 |192.168.2.4 |Umožňuje přenos příliš**onpremsn1** prostřednictvím **OPFW** |

## <a name="ip-forwarding"></a>Předávání IP
UDR a předávání IP adres jsou funkce, které můžete použít v kombinaci tooallow virtuální zařízení toobe používá toocontrol přenosového toku ve službě Azure VNet.  Virtuální zařízení není nic jiného než virtuální počítač, který běží aplikace používá toohandle síťový provoz nějakým způsobem, jako je například Brána firewall nebo zařízením NAT.

Tato virtuální zařízení, virtuálních počítačů musí být schopný tooreceive příchozí provoz, který nebyly upraveny tooitself. tooallow přenosy virtuálních počítačů tooreceive řešit tooother cíle, je nutné povolit předávání IP adres pro hello virtuálních počítačů. Toto je nastavení, nikoli nastavení hello hostovaného operačního systému Azure. Vaše virtuální zařízení stále potřebám toorun některé typ toohandle aplikace hello příchozí provoz a přesměrovávala je správně.

toolearn Další informace o předávání IP adres, navštivte [co jsou trasy definované uživatelem a předávání IP](virtual-networks-udr-overview.md#ip-forwarding).

Jako příklad Představte si, že máte hello po instalaci v Azure virtuální sítě:

* Podsíť **onpremsn1** obsahuje virtuální počítač s názvem **onpremvm1**.
* Podsíť **onpremsn2** obsahuje virtuální počítač s názvem **onpremvm2**.
* Virtuální zařízení s názvem **OPFW** je připojeno příliš**onpremsn1** a **onpremsn2**.
* Trasy definované uživatelem propojené příliš**onpremsn1** Určuje, že všechny přenosy příliš**onpremsn2** , musí se poslat příliš**OPFW**.

Na tento příkaz, pokud **onpremvm1** pokusí tooestablish připojení s **onpremvm2**, hello UDR se použije, a provoz se budou odesílat příliš**OPFW** jako další segment hello. Mějte na paměti, která hello skutečných paketech cílové nedojde ke změně, stále zobrazuje **onpremvm2** je cílový hello. 

Bez povoleno pro předávání IP **OPFW**, hello Azure logic virtuální sítě budou vyřadit pakety hello, jelikož umožňuje pouze tooa toobe odeslat pakety virtuálních počítačů, pokud hello IP adresu Virtuálního počítače je hello cíl pro paket hello.

S předávání IP adres předá hello virtuální síť Azure logic tooOPFW pakety hello, aniž byste museli měnit jeho původní cílovou adresu. **OPFW** musí zpracování pakety hello a určit, jaké toodo s nimi.

Pro scénář hello výše toowork, je nutné povolit předávání IP adres na hello síťových adaptérů pro **OPFW**, **AZF1**, **AZF2**, a **AZF3** , který se používá pro směrování (všechny síťové adaptéry s výjimkou hello ty, které jsou propojené toohello správu podsíť). 

## <a name="firewall-rules"></a>Pravidla brány firewall
Jak je popsáno výše, předávání IP adres pouze zajistí, že jsou pakety odesílány toohello virtuální zařízení. Vaše zařízení pořád potřebuje toodecide co toodo se tyto pakety. Ve scénáři hello výše budete potřebovat toocreate hello následující pravidla v svoje zařízení:

### <a name="opfw"></a>OPFW
OPFW představuje místní zařízení se systémem obsahující hello následující pravidla:

* **Trasy**: všechny přenosy too10.0.0.0/16 (**azurevnet**) prostřednictvím tunelu, musí se poslat **ONPREMAZURE**.
* **Zásady**: Povolí všechny obousměrnou komunikaci mezi **port2** a **ONPREMAZURE**.

### <a name="azf1"></a>AZF1
AZF1 představuje Azure virtuální zařízení obsahující hello následující pravidla:

* **Zásady**: Povolí všechny obousměrnou komunikaci mezi **port1** a **port2**.

### <a name="azf2"></a>AZF2
AZF2 představuje Azure virtuální zařízení obsahující hello následující pravidla:

* **Trasy**: všechny přenosy too10.0.0.0/16 (**onpremvnet**), musí se poslat toohello služba Azure gateway IP adresa (např. 10.0.0.1) prostřednictvím **port1**.
* **Zásady**: Povolí všechny obousměrnou komunikaci mezi **port1** a **port2**.

## <a name="network-security-groups-nsgs"></a>Skupiny zabezpečení sítě (Nsg)
V tomto scénáři nejsou používány skupiny Nsg. Můžete však použít skupiny Nsg tooeach podsíť toorestrict příchozí a odchozí provoz. Může například použít následující pravidla NSG toohello externí FW podsíť hello.

**Příchozí**

* Povolit všechny přenosy TCP z hello Internet tooport 80 na žádné virtuální počítače v podsíti hello.
* Odepřít všechny ostatní přenosy z Internetu hello.

**Odchozí**

* Odepřete všechny přenosy toohello Internetu.

## <a name="high-level-steps"></a>Kroky vysoké úrovně
toodeploy tento scénář, pomocí následujících hello vysoké úrovně kroků.

1. Přihlášení tooyour předplatné Azure.
2. Pokud chcete toodeploy virtuální síť toomimic hello místní sítě, zřízení hello prostředků, které jsou součástí **ONPREMRG**.
3. Zřízení hello prostředků, které jsou součástí **AZURERG**.
4. Tunelové propojení hello zřídit z **onpremvnet** příliš**azurevnet**.
5. Po zřízení jsou všechny prostředky, přihlaste se příliš**onpremvm2** ping 10.0.3.101 tootest připojení mezi **onpremsn2** a **azsn3**.

