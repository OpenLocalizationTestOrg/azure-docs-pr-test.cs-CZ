---
title: "aaaPlan mapování sítě pro replikaci virtuálního počítače technologie Hyper-V pomocí Site Recovery | Microsoft Docs"
description: "Nastavte mapování sítě pro replikaci virtuálního počítače technologie Hyper-V z místního datového centra tooAzure, nebo tooa sekundární lokality."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: tysonn
ms.assetid: fcaa2f52-489d-4c1c-865f-9e78e000b351
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 05/23/2017
ms.author: raynew
ms.openlocfilehash: 86199b5840ea10fd33630bcc75d14340a49e01bd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="plan-network-mapping-for-hyper-v-vm-replication-with-site-recovery"></a>Plánování mapování sítě pro replikaci virtuálního počítače technologie Hyper-V pomocí Site Recovery



Tento článek vám pomůže toounderstand a plán pro síť během replikace tooAzure virtuálních počítačů Hyper-V, nebo sekundární lokality tooa mapování, pomocí hello [služba Azure Site Recovery](site-recovery-overview.md).

Po přečtení tohoto článku post všechny komentáře v dolní části hello tohoto článku nebo technické dotazy posílejte na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="network-mapping-for-replication-tooazure"></a>Mapování sítě pro replikaci tooAzure

Mapování sítě se používá při replikaci virtuálních počítačů Hyper-V (spravované v nástroji VMM) tooAzure. Síťové mapování mapuje sítě virtuálních počítačů na zdrojovém serveru VMM a cílové sítě Azure. Mapování hello následující:

- **Síťové připojení**– jistotu, že replikované virtuální počítače Azure připojené toohello namapované síťové. Všechny počítače, které převzetí služeb při selhání na hello stejné se mohou připojit tooeach jiných, i když při selhání v plánech různých obnovení.
- **Brána sítě**– Pokud na hello cílové síti Azure nastavená brána sítě, virtuální počítače můžete připojit tooother místní virtuální počítače.

Poznámky:

- Namapujete zdrojové tooan sítě virtuálních počítačů ve VMM virtuální síť Azure.
- Po převzetí služeb při selhání. virtuální počítače Azure v hello bude Zdrojová síť připojená toohello namapované cílový virtuální sítě.
- Přidat nové virtuální počítače jsou připojené toohello zdrojové síti virtuálních počítačů toohello namapované síti Azure, když dojde k replikaci.
- Pokud má hello Cílová síť více podsítí a jedna z těchto podsítí má hello stejný název jako podsíť, na které hello zdrojový virtuální počítač nachází, pak virtuální počítač repliky hello připojí po převzetí služeb při selhání toothat cílové podsíti.
- Pokud není žádná cílová podsíť s odpovídajícím názvem, hello virtuální počítač připojí toohello první podsíť v síti hello.


## <a name="network-mapping-for-replication-tooa-secondary-datacenter"></a>Mapování sítě pro replikaci tooa sekundárního datacentra

Mapování sítě se používá při replikaci virtuálních počítačů Hyper-V (spravované v System Center Virtual Machine Manager (VMM)) tooa sekundárního datacentra. Mapování sítě zajišťuje mapování mezi sítěmi virtuálních počítačů na zdrojovém serveru VMM a sítě virtuálních počítačů na cílovém serveru VMM. Mapování hello následující:

- **Síťové připojení**– sítě tooappropriate připojí virtuální počítače po převzetí služeb při selhání. Hello repliku virtuálního počítače budou připojené toohello cílové sítě, která je namapované toohello zdrojovou síť.
- **Optimální umístění**– optimálně místech hello replikované virtuální počítače na hostitelských serverech technologie Hyper-V. Virtuální počítače repliky jsou umístěny na hostitelích, že může přístup hello mapovat sítě virtuálních počítačů.
- **Žádné mapování sítě**– Pokud nenakonfigurujete mapování sítě, virtuální počítače replik nebudou připojené tooany sítě virtuálních počítačů po převzetí služeb při selhání.

Poznámky:

- Mapování sítě můžete nakonfigurovat sítě virtuálních počítačů na dvěma servery VMM nebo na jednom serveru VMM Pokud dvě lokality jsou spravovány nástrojem hello stejný server.
- Při mapování správně nakonfigurovaná a je povolená replikace, virtuální počítač na primárním umístění hello bude připojený tooa sítě a jeho repliky v cílové umístění hello se připojí tooits mapovat sítě.
-
- Pokud sítě byla nastavili správně v nástroji VMM, když při mapování sítě vyberte cílovou síť virtuálních počítačů, hello VMM zdroj cloudy, které používají hello zdrojové síti virtuálních počítačů se zobrazí, společně s hello dostupných cílových sítí virtuálních počítačů na hello cíl cloudy, které se používají pro ochrana.
- Pokud hello Cílová síť více podsítí a jedna z těchto podsítí má stejný název jako hello podsítě, na které hello zdrojový virtuální počítač nachází, pak hello hello bude virtuální počítač repliky po převzetí služeb při selhání připojené toothat cílové podsíti. Pokud není žádná cílová podsíť s odpovídajícím názvem, hello virtuální počítač bude připojený toohello první podsíť v síti hello.



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



## <a name="next-steps"></a>Další kroky

Další informace o [plánování hello síťové infrastruktury](site-recovery-network-design.md).
