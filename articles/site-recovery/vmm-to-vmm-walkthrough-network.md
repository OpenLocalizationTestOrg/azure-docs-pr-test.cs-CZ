---
title: "aaaPlan sítě pro Hyper-V replikace tooa sekundární lokalita VMM s Azure Site Recovery | Microsoft Docs"
description: "Tento článek popisuje plánování sítě při replikaci virtuálních počítačů Hyper-V tooa sekundární System Center VMM lokalitu s Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 095f2d73-994e-4fc0-96f2-e03a7d72e492
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: raynew
ms.openlocfilehash: 5934db4a661a2c697a1a799c3848852250ddb451
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-3-plan-networking-for-hyper-v-vm-replication-tooa-secondary-vmm-site"></a>Krok 3: Plánování sítě pro virtuální počítač Hyper-V replikace tooa sekundární lokalita VMM

Po zkontrolování požadavcích pro nasazení, přečtěte si tento článek tooplan sítě při replikaci virtuálních počítačů technologie Hyper-V (VM) spravované v cloudech System Center Virtual Machine Manager (VMM) pomocí sekundární lokality tooa [Azure Site Recovery](site-recovery-overview.md) v hello portálu Azure. 

Po přečtení tohoto článku, post jakékoli komentáře v dolní části hello nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="network-mapping-overview"></a>Přehled mapování sítě

Mapování sítě se používá při replikaci virtuálních počítačů Hyper-V (spravované v nástroji VMM) tooa sekundárního datacentra. Mapování sítě zajišťuje mapování mezi sítěmi virtuálních počítačů na zdrojovém serveru VMM a sítě virtuálních počítačů na cílovém serveru VMM. Mapování hello následující:

- **Síťové připojení**– sítě tooappropriate připojí virtuální počítače po převzetí služeb při selhání. Hello repliku virtuálního počítače budou připojené toohello cílové sítě, která je namapované toohello zdrojovou síť.
- **Optimální umístění**– optimálně místech hello replikované virtuální počítače na hostitelských serverech technologie Hyper-V. Virtuální počítače repliky jsou umístěny na hostitelích, že může přístup hello mapovat sítě virtuálních počítačů.
- **Žádné mapování sítě**– Pokud nenakonfigurujete mapování sítě, virtuální počítače replik nebudou připojené tooany sítě virtuálních počítačů po převzetí služeb při selhání.


### <a name="example"></a>Příklad

Tady je tooillustrate příklad tento mechanismus. Podívejme se na organizaci s dvěma umístěními v New Yorku a Chicagu.

**Umístění** | **Server VMM** | **Sítě virtuálních počítačů** | **Mapovat na**
---|---|---|---
New York | VMM NewYork| VMNetwork1 NewYork | Mapovat tooVMNetwork1 Chicago
 |  | VMNetwork2 NewYork | Není mapováno
Chicago | VMM Chicago| VMNetwork1 Chicago | Mapovat tooVMNetwork1 NewYork
 | | VMNetwork1 Chicago | Není mapováno

V tomto příkladu:

- Když virtuální počítač repliky se vytvoří pro virtuální počítač, který je připojený tooVMNetwork1 NewYork, budou připojené tooVMNetwork1 Chicagu.
- Když virtuální počítač repliky se vytvoří pro VMNetwork2 NewYork nebo VMNetwork2 Chicagu, nebude připojený tooany sítě.

Zde je, jak jsou v naše ukázkové společnosti a hello logické sítě přidružené cloudy hello nastavit cloudech VMM.

#### <a name="cloud-protection-settings"></a>Nastavení ochrany cloudu

**Chráněném cloudu** | **Ochrana cloudu** | **Logické sítě (New Yorku)**  
---|---|---
GoldCloud1 | GoldCloud2 |
SilverCloud1| SilverCloud2 |
GoldCloud2 | <p>Není k dispozici</p><p></p> | <p>LogicalNetwork1 NewYork</p><p>LogicalNetwork1 Chicago</p>
SilverCloud2 | <p>Není k dispozici</p><p></p> | <p>LogicalNetwork1 NewYork</p><p>LogicalNetwork1 Chicago</p>

#### <a name="logical-and-vm-network-settings"></a>Nastavení sítě logické a virtuální počítač

**Umístění** | **Logické sítě** | **Přidružené sítě virtuálních počítačů**
---|---|---
New York | LogicalNetwork1 NewYork | VMNetwork1 NewYork
Chicago | LogicalNetwork1 Chicago | VMNetwork1 Chicago
 | LogicalNetwork2Chicago | VMNetwork2 Chicago

#### <a name="target-network-settings"></a>Nastavení cílové sítě

Na základě tohoto nastavení, když vyberete síť virtuálních počítačů cíl hello, hello následující tabulka znázorňuje hello možnosti, které budou k dispozici.

**Výběr** | **Chráněném cloudu** | **Ochrana cloudu** | **Cílová síť k dispozici**
---|---|---|---
VMNetwork1 Chicago | SilverCloud1 | SilverCloud2 | Dostupné
 | GoldCloud1 | GoldCloud2 | Dostupné
VMNetwork2 Chicago | SilverCloud1 | SilverCloud2 | Není k dispozici
 | GoldCloud1 | GoldCloud2 | Dostupné


Pokud hello Cílová síť více podsítí a jedna z těchto podsítí má stejný název jako hello podsítě, na které hello zdrojový virtuální počítač nachází, pak hello hello bude virtuální počítač repliky po převzetí služeb při selhání připojené toothat cílové podsíti. Pokud není žádná cílová podsíť s odpovídajícím názvem, hello virtuální počítač bude připojený toohello první podsíť v síti hello.


#### <a name="failback-behavior"></a>Chování navrácení služeb po obnovení

Co se stane, že v případě hello navrácení služeb po obnovení (zpětná replikace), toosee Předpokládejme, že je VMNetwork1 NewYork namapované tooVMNetwork1-Chicagu s hello následující nastavení.


**Virtuální počítač** | **Připojené tooVM sítě**
---|---
VM1 | VMNetwork1 sítě
Virtuálního počítače 2 (repliky VM1) | VMNetwork1 Chicago

S těmito nastaveními pojďme si shrnout, co se stane, že v několika možných scénářích.

**Scénář** | **Výsledek**
---|---
Žádná změna v hello vlastnosti sítě virtuálních počítačů 2 po převzetí služeb při selhání. | Virtuální počítač 1 zůstává připojený toohello zdrojovou síť.
Vlastnosti sítě virtuálních počítačů 2 se změní po převzetí služeb při selhání a není připojen. | 1 virtuální počítač není připojen.
Vlastnosti sítě virtuálních počítačů 2 se změní po převzetí služeb při selhání a je připojený tooVMNetwork2 Chicagu. | Pokud není namapované VMNetwork2 Chicagu, bude odpojen virtuálních počítačů 1.
Mapování sítě VMNetwork1 Chicagu se změní. | Virtuální počítač 1 bude nyní mapovat sítě připojené toohello tooVMNetwork1-Chicagu.



## <a name="prepare-for-network-mapping"></a>Příprava mapování sítě

1. Na hello zdrojové a cílové servery VMM měli byste mít logické sítě přidružené cloudy zdrojové a cílové hello. 
2. V hello zdrojové a cílové servery měli byste mít logické sítě propojené toohello sítě virtuálních počítačů.
3. Virtuální počítače na hostitelích technologie Hyper-V v hello zdrojové umístění musí být propojená toohello zdrojové síti virtuálních počítačů. Pokud používáte pouze jeden server VMM, můžete nakonfigurovat mapování sítě virtuálních počítačů na hello stejný server.

Zde je, co se stane, když nastavíte mapování sítě během nasazování Site Recovery:

- Když nastavit mapování sítě a vyberte cílovou síť virtuálních počítačů, hello VMM zdroj cloudy, které používají hello zdrojové síti virtuálních počítačů se zobrazí, společně s hello dostupných cílových sítí virtuálních počítačů v cloudech cíl hello.
- - Pokud mapování správně nakonfigurovaná a je povolená replikace, zdrojového virtuálního počítače budou zdrojové síti virtuálních počítačů připojených tooits a jeho repliky v cílové umístění hello se připojí tooits mapovat síť virtuálních počítačů.
- Pokud má hello Cílová síť více podsítí a jedna z těchto podsítí má stejný název jako hello podsítě, na které hello zdrojový virtuální počítač nachází, pak hello hello bude virtuální počítač repliky po převzetí služeb při selhání připojené toothat cílové podsíti. Pokud není žádná cílová podsíť s odpovídajícím názvem, bude hello virtuálních počítačů připojených toohello první podsíť v síti hello.

## <a name="connect-toovms-after-failover"></a>Po převzetí služeb při selhání připojit tooVMs

Při plánování replikace a převzetí služeb při selhání, je mezi klíčové otázky hello jak tooconnect toohello repliky po převzetí služeb při selhání. Existuje několik možností: 

- **Použít jinou IP adresu**: můžete vybrat toouse jinou IP adresu pro hello replikovaných virtuálních počítačů. V tento scénář hello počítač získá nové adresy IP po převzetí služeb při selhání a je nutná aktualizace DNS.
- **Zachovat hello stejnou IP adresu**: můžete chtít toouse hello stejnou IP adresu pro hello repliku virtuálního počítače. Zachování hello stejné IP adresy zjednodušuje obnovení hello snížením sítě o problémech souvisejících s po převzetí služeb při selhání. 

## <a name="retain-ip-addresses"></a>Zachovat IP adresy

Pokud chcete, aby tooretain hello IP adresy z primární lokality hello po převzetí služeb při selhání toohello sekundární lokality, můžete selhání úplné podsítě a aktualizovat trasy tooindicate hello nové umístění hello IP adres, nebo alternativní nasadit podsíť roztažené mezi hello primární a hello obnovení lokality.

### <a name="stretched-subnet"></a>Roztažené podsítě

V podsíť roztažené hello podsíť je k dispozici současně v obou hello primární a sekundární lokality. Pokud přesunete server a sekundární lokalitou toohello konfigurace IP (vrstvy 3), hello sítě bude směrovat hello provoz toohello nové umístění. 

Z hlediska vrstvy 2 (Datová vrstva odkaz) budete potřebovat síťové zařízení, která můžete spravovat roztažené sítě VLAN. Kromě toho podle roztažení hello sítě VLAN, potenciální doména selhání hello rozšiřuje tooboth lokalit, v podstatě stane jediným bodem selhání. Přestože se pravděpodobně, k tomu mohlo dojít, že všesměrového vysílání storm spuštěná a nemůže být izolované. 


### <a name="subnet-failover"></a>Podsíť převzetí služeb při selhání

Výhody podsíti hello roztažen tak, můžete spustit hello tooobtain převzetí služeb při selhání podsíť bez ve skutečnosti roztažení se. V tomto řešení bude podsíť k dispozici v lokalitě hello zdroje nebo cíle, ale ne v obou současně. toomaintain hello adresní prostor IP adres v hello události převzetí služeb při selhání, můžete programově uspořádat pro hello směrovač infrastruktury toomove hello podsítě z jedné lokality tooanother. V případě, když dojde k převzetí služeb při selhání, by se přesunuty podsítě hello přidružené virtuální počítače. Hello Hlavní nevýhodou je, že v případě hello selhání, máte toomove hello celou podsíť.

### <a name="example"></a>Příklad

Tady je příklad podsíť dokončení převzetí služeb při selhání. aplikace běžící v podsíti 192.168.1.0/24 má primární lokalita Hello. Na převzetí služeb při selhání hello všechny virtuální počítače v této podsíti jsou převzal toohello sekundární lokality a zachovat jejich IP adresy. Toobe trasy potřeba upravit fakt hello tooreflect všechny hello virtuálního počítače virtuální počítače patřící toosubnet 192.168.1.0/24 byl přesunut teď toohello sekundární lokality.

Hello následující obrázky znázorňují hello podsítě před a po převzetí služeb při selhání:

- Před převzetí služeb při selhání 192.168.0.1/24 podsíť je aktivní na hello zdrojové lokality, stane aktivní na sekundární lokalitě hello po převzetí služeb při selhání.
- Hello tras mezi primární lokality a lokality obnovení, třetí a primární lokalitou a třetí lokality a lokality obnovení bude mít toobe odpovídajícím způsobem upravit.

**Před převzetí služeb při selhání**

![Před převzetí služeb při selhání](./media/vmm-to-vmm-walkthrough-network/network-design2.png)

**Po převzetí služeb při selhání**

![Po převzetí služeb při selhání](./media/vmm-to-vmm-walkthrough-network/network-design3.png)

Po převzetí služeb při selhání stane se toto:

- Site Recovery přidělí IP adresu pro každé síťové rozhraní na hello virtuální počítač, z hello fond statických adres IP v příslušné síti hello, pro každou instanci služby VMM.
- Pokud je fond IP adres v sekundární lokalitě hello hello hello stejný jako ve zdrojové lokalitě hello, Site Recovery přiděluje hello stejnou IP adresu (hello zdrojového virtuálního počítače) toohello repliku virtuálního počítače. Hello IP adresa je vyhrazená v nástroji VMM, ale není nastaven jako hello převzetí služeb při selhání IP adresu na hostitele Hyper-V hello. těsně před hello převzetí služeb při selhání je sada adres IP Hello převzetí služeb při selhání na hostiteli technologie Hyper-v.
- Pokud hello stejnou IP adresu není dostupná, Site Recovery přiděluje další dostupnou IP adresu z fondu hello.
- Pokud virtuální počítače používat službu DHCP, není Site Recovery spravovat hello IP adresy. Je třeba toocheck, který hello DHCP server na sekundární lokalitě hello můžete přidělit adresu z hello stejný rozsah jako hello zdrojové lokality.

### <a name="validate-hello-ip-address"></a>Ověřte adresu IP hello

Po povolení ochrany pro virtuální počítač, můžete použít následující ukázka skriptu tooverify hello adresu přiřazenou toohello virtuálních počítačů. Hello stejnou IP adresu se nastavit jako hello převzetí služeb při selhání IP adresu a přiřadit toohello virtuálních počítačů v době hello převzetí služeb při selhání:

    ```
    $vm = Get-SCVirtualMachine -Name <VM_NAME>
    $na = $vm[0].VirtualNetworkAdapters>
    $ip = Get-SCIPAddress -GrantToObjectID $na[0].id
    $ip.address 
    ```

## <a name="changing-ip-addresses"></a>Změna IP adresy

V tomto scénáři se změní hello IP adresy virtuálních počítačů, které převzetí služeb při selhání. Nevýhodou Hello tohoto řešení je hello údržby vyžaduje. Obvykle DNS bude aktualizován po spuštění replikované virtuální počítače. Záznamy DNS může potřebovat změnit toobe nebo fluster v síťového a aktualizované položky mezipaměti. To může vést k výpadkům. Výpadek lze zmírnit následujícím způsobem:

- Použijte nízké hodnoty TTL pro aplikace v síti intranet.
- Použijte následující skript v plánu obnovení Site Recovery, tooupdate hello DNS server tooensure včasné aktualizace hello. Pokud používáte dynamickou registraci DNS nepotřebujete hello skriptu.

    ```
    param(
    string]$Zone,
    [string]$name,
    [string]$IP
    )
    $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
    $newrecord = $record.clone()
    $newrecord.RecordData[0].IPv4Address  =  $IP
    Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord
    ```
    
### <a name="example"></a>Příklad 

Podívejme se na scénář, ve kterém plánujete toouse různých IP adres napříč hello primární a hello obnovení lokality. V tomto příkladu máme různých IP adres napříč primárních a sekundárních lokalit a existuje; web s třetí z aplikace, které hostované na hello primární nebo obnovení lokality lze přistupovat.

- Před převzetí služeb při selhání aplikace se 192.168.1.0/24 hostované podsítí v primární lokalitě hello a jsou nakonfigurované toobe v podsíti 172.16.1.0/24 na sekundární lokalitě hello po převzetí služeb při selhání.
- Trasy připojení nebo síť VPN byla správně nakonfigurována, aby všechny tři servery navzájem přístup.
- Po převzetí služeb při selhání se obnoví v podsíti obnovení hello aplikace. V tomto scénáři je bez nutnosti toofail přes celou podsíť hello a žádné změny jsou potřebné tooreconfigure VPN nebo síťové trasy. Hello převzetí služeb při selhání a některé aktualizace služby DNS, ujistěte se, aby aplikace zůstaly dostupné.
- Pokud DNS není nakonfigurované tooallow dynamické aktualizace, budou zaregistrovat virtuální počítače hello sami hello novou IP adresu, při spuštění po převzetí služeb při selhání.

**Před převzetí služeb při selhání**

![Různé IP - před převzetí služeb při selhání](./media/vmm-to-vmm-walkthrough-network/network-design10.png)

**Po převzetí služeb při selhání**

![Různé IP - po převzetí služeb při selhání](./media/vmm-to-vmm-walkthrough-network/network-design11.png)



## <a name="next-steps"></a>Další kroky

Přejděte příliš[krok 4: Příprava VMM nebo Hyper-V](vmm-to-vmm-walkthrough-vmm-hyper-v.md).


