---
title: Architektura hello aaaReview tooAzure replikace VMware | Microsoft Docs
description: "Tento článek obsahuje přehled součásti a architektura použít při replikaci tooAzure místní virtuální počítače VMware s hello služba Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: c413efcd-d750-4b22-b34b-15bcaa03934a
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27.017
ms.author: raynew
ms.openlocfilehash: 7acb1d8e83a846dca79e175fd1af9324f2c31e65
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-1-review-hello-architecture-for-vmware-replication-tooazure"></a>Krok 1: Posouzení hello architektura pro tooAzure replikace VMware

Tento článek popisuje hello součástech a procesech používá při replikaci místní tooAzure virtuální počítače VMware pomocí hello [Azure Site Recovery](site-recovery-overview.md) služby.

Odeslat všechny komentáře v dolní části hello tohoto článku nebo pokládání dotazů v hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="architectural-components"></a>Komponenty architektury

Hello tabulka shrnuje hello součásti, které potřebujete.

**Komponenta** | **Požadavek** | **Podrobnosti**
--- | --- | ---
**Azure** | Potřebujete předplatné Azure, účet úložiště Azure a sítě Azure. | Replikovaná data je uložený v účtu úložiště hello. Virtuální počítače Azure vytvořené mají hello replikovat data, když dojde k převzetí služeb při selhání z vaší místní lokalitě. Hello virtuálních počítačích Azure připojit toohello virtuální síť Azure, když vytváří.
**Konfigurační server** | Jeden místní server pro správu (virtuální počítač VMware) používající všech hello místních součásti Site Recovery pro nasazení hello. Mezi ně patří, a konfigurační server, procesový server, hlavní cílový server. | součásti serveru configuration Hello koordinuje komunikaci mezi místními a Azure a spravuje replikaci dat.
 **Procesový server:**  | Ve výchozím nastavení na konfiguračním serveru hello nainstalovaná. | Funguje jako replikační brána. Přijímá data replikace, optimalizuje je pomocí ukládání do mezipaměti, komprese a šifrování a odešle ji tooAzure úložiště.<br/><br/> procesový server Hello také obstará nabízenou instalaci počítače tooprotected služby Mobility hello a provádí automatického zjišťování virtuálních počítačů VMware.<br/><br/> S růstem nasazení můžete přidat další samostatný proces vyhrazené servery toohandle zvýšení svazky provoz replikace.
 **Hlavní cílový server** | Ve výchozím nastavení na konfiguračním serveru hello místně nainstalován. | Zpracovává replikační data během navracení služeb z Azure po obnovení.<br/><br/> Pokud je objem přenosů při navracení služeb po obnovení příliš vysoký, můžete nasadit samostatný hlavní cílový server pro účely navrácení služeb po obnovení.
**Servery VMware** | Hostované virtuální počítače VMware na serverech vSphere ESXi a doporučujeme systému vCenter server toomanage hello hostitele. | Můžete přidat servery VMware, tooyour trezor služeb zotavení.
**Replikované počítače** | Hello služba Mobility se nainstalují na jednotlivé virtuální počítače VMware můžete replikovat. Ho může být nainstalován ručně na každém počítači, nebo pomocí nabízené instalace z hello procesový server.

**Obrázek 1: VMware tooAzure součásti**

![Komponenty](./media/vmware-walkthrough-architecture/arch-enhanced.png)

## <a name="replication-process"></a>Proces replikace

1. Nastavit hello nasazení, včetně místní a Azure součásti. V trezoru služeb zotavení hello zadejte hello replikace zdroj a cíl, nastavení konfigurace serveru hello, vytvořte zásadu replikace, nasazení hello služby Mobility, povolit replikaci a spustit testovací převzetí služeb.
2. Replikovat počítače v souladu s hello zásady replikace a počáteční kopii dat hello je tooAzure replikované úložiště.
3. Po dokončení počáteční replikace začne replikace tooAzure rozdílové změny. Sledované změny se pro jednotlivé počítače ukládají do souboru .hrl.
    - Replikaci počítačů komunikovat s hello konfigurační server na portu 443 protokolu HTTPS příchozí pro správu replikace.
    - Replikaci počítače odesílají replikace dat toohello procesový server na portu HTTPS 9443 příchozí (lze upravit).
    - konfigurační server Hello orchestruje replikaci správy s Azure přes port HTTPS 443 odchozí.
    - Hello procesový server přijímá data ze zdrojového počítače, optimalizuje je zašifruje a odešle ji tooAzure úložiště přes port 443 odchozí.
    - Pokud povolíte více virtuálních počítačů konzistence, pak počítače v replikační skupině hello vzájemně komunikovat přes port 20004. Konzistence více virtuálních počítačů znamená, že seskupíte víc virtuálních počítačů do replikační skupiny, v rámci které se sdílí body obnovení konzistentní vzhledem k selháním a konzistentní vzhledem k aplikacím, když dojde k převzetí služeb při selhání. To je užitečné, pokud počítače běží stejné zatížení hello a je třeba toobe konzistentní.
4. Přenosy jsou replikované tooAzure úložiště veřejné koncové body, přes hello internet. Alternativně můžete použít [veřejný partnerský vztah](../expressroute/expressroute-circuit-peerings.md#public-peering) Azure ExpressRoute. Provoz replikace prostřednictvím VPN site-to-site místní lokalitě tooAzure není podporována.


**Obrázek 2: VMware tooAzure replikace**

![Rozšířené](./media/vmware-walkthrough-architecture/v2a-architecture-henry.png)

## <a name="failover-and-failback-process"></a>Proces převzetí služeb při selhání a navrácení služeb po obnovení

1. Po ověření, že převzetí služeb při selhání funguje podle očekávání, můžete spustit neplánované převzetí služeb při selhání tooAzure podle potřeby. Plánované převzetí služeb není podporované.
2. Můžete převzít jeden počítač nebo vytvořit [plány obnovení](site-recovery-create-recovery-plans.md), toofail přes víc virtuálních počítačů.
3. V průběhu převzetí služeb při selhání se v Azure vytvoří replika virtuálního počítače. Potvrzení převzetí služeb při selhání toostart přístupem hello zatížení z hello repliky virtuálního počítače Azure.
4. Až bude vaše místní lokalita opět dostupná, můžete službu navrátit. Nastavení infrastruktury navrácení služeb po obnovení, zahájení replikace z primární toohello sekundární lokality hello hello počítač a spustit neplánované převzetí služeb při selhání z hello sekundární lokality. Po potvrzení toto převzetí služeb při selhání, data budou zpět na místní a budete potřebovat tooAzure tooenable replikace. [Další informace](site-recovery-failback-azure-to-vmware.md)

Pro navrácení služby po obnovení existuje několik požadavků:


- **Dočasný procesní server v Azure**: Pokud chcete, aby toofail zpět z Azure po převzetí služeb při selhání, budete potřebovat tooset si virtuální počítač Azure nakonfigurovaný jako procesní server, toohandle replikace z Azure. Tento virtuální počítač je možné po navrácení služeb po obnovení odstranit.
- **Připojení k síti VPN**: pro navrácení služeb po obnovení budete potřebovat připojení VPN (nebo Azure ExpressRoute) nastavit z hello Azure toohello místní síť.
- **Samostatný místní hlavní cílový server**: hello místní hlavní cílový server zpracovává navrácení služeb po obnovení. Hello hlavní cílový server je nainstalován na serveru pro správu hello ve výchozím nastavení, ale pokud se obnovení vracíte větší objemy přenosů měli nastavit samostatný místní hlavní cílový server pro tento účel.
- **Navrácení služeb po obnovení zásad**: tooreplicate back tooyour místní lokality, musíte zásadu navrácení služeb po obnovení. Ty se vytvoří automaticky při vytvoření zásad replikace.
- **Infrastruktury VMware**: musí selhat zpět tooan místní virtuální počítač VMware. To znamená, že se vyžaduje místní infrastruktury VMware, i když replikujete tooAzure místní fyzických serverů.

**Obr. 3: Navrácení služeb po obnovení – VMware nebo fyzický server**

![Navrácení služeb po obnovení](./media/vmware-walkthrough-architecture/enhanced-failback.png)


## <a name="next-steps"></a>Další kroky

Přejděte příliš[krok 2: ověření předpoklady a omezení](vmware-walkthrough-prerequisites.md)
