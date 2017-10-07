---
title: "Architektura hello aaaReview pro fyzický server replikace tooAzure pomocí Azure Site Recovery | Microsoft Docs"
description: "Tento článek obsahuje přehled součásti a architektura použít při replikaci tooAzure místní fyzických serverů s hello služba Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 09c9b136-35f5-465e-8d0f-f4c5d5d9f880
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: 4f334c181fa6f0821adf978ebe9dbd7d36a3c753
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-1-review-hello-architecture-for-physical-server-replication-tooazure"></a>Krok 1: Posouzení hello architektura pro tooAzure replikace fyzického serveru

Tento článek popisuje hello součástech a procesech použít při replikaci tooAzure fyzických serverů Windows nebo Linuxem místní, pomocí hello [Azure Site Recovery](site-recovery-overview.md) služby.

Odeslat všechny komentáře v dolní části hello tohoto článku nebo pokládání dotazů v hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="architectural-components"></a>Komponenty architektury

Hello tabulka shrnuje hello součásti, které potřebujete.

**Komponenta** | **Požadavek** | **Podrobnosti**
--- | --- | ---
**Azure** | Potřebujete účet Azure, účet úložiště Azure a sítě Azure. | Replikovaná data jsou uložena v účtu úložiště hello a vytvoří se virtuální počítače Azure s hello replikovat data, když dojde k převzetí služeb při selhání. Virtuální počítače Azure připojit toohello virtuální síť Azure, když vytváří.
**Konfigurační server** | Jeden místní server pro správu (fyzický server nebo virtuální počítač VMware) používající všech hello místních součásti Site Recovery. Mezi ně patří, a konfigurační server, procesový server, hlavní cílový server. | součásti serveru configuration Hello koordinuje komunikaci mezi místními a Azure a spravuje replikaci dat.
 **Procesový server**  | Ve výchozím nastavení na konfiguračním serveru hello nainstalovaná. | Funguje jako replikační brána. Přijímá data replikace, optimalizuje je pomocí ukládání do mezipaměti, komprese a šifrování a odešle ji tooAzure úložiště.<br/><br/> procesový server Hello také obstará nabízenou instalaci hello Mobility service tooprotected počítačů.<br/><br/> Můžete přidat další samostatný proces vyhrazené servery, toohandle zvýšení svazky provoz replikace.
 **Hlavní cílový server** | Ve výchozím nastavení na konfiguračním serveru hello místně nainstalován. | Zpracovává replikační data během navracení služeb z Azure po obnovení.<br/><br/> Pokud je objem přenosů při navracení služeb po obnovení příliš vysoký, můžete nasadit samostatný hlavní cílový server pro účely navrácení služeb po obnovení.
**Replikované servery** | Hello službu mobility je nainstalován na každém serveru Windows nebo Linuxem chcete tooreplicate. Ho může být nainstalován ručně na každém počítači, nebo pomocí nabízené instalace z hello procesový server.
**Navrácení služeb po obnovení** | Navrácení služeb po obnovení VMware je infrastruktura vyžaduje. Nelze označit back tooa fyzický server.


**Obrázek 1: Fyzické tooAzure součásti**

![Komponenty](./media/physical-walkthrough-architecture/arch-enhanced.png)

## <a name="replication-process"></a>Proces replikace

1. Nastavit hello nasazení, včetně místní a Azure součásti. V trezoru služeb zotavení hello zadejte hello replikace zdroj a cíl, nastavení konfigurace serveru hello, vytvořte zásadu replikace, nasazení hello služby Mobility, povolit replikaci a spustit testovací převzetí služeb.
2.  Replikovat počítače v souladu s hello zásady replikace a počáteční kopii dat hello je tooAzure replikované úložiště.
4. Po dokončení počáteční replikace začne replikace tooAzure rozdílové změny. Sledované změny se pro jednotlivé počítače ukládají do souboru .hrl.
    - Replikaci počítačů komunikovat s hello konfigurační server na portu 443 protokolu HTTPS příchozí pro správu replikace.
    - Replikaci počítače odesílají replikace dat toohello procesový server na portu HTTPS 9443 příchozí (lze upravit).
    - konfigurační server Hello orchestruje replikaci správy s Azure přes port HTTPS 443 odchozí.
    - Hello procesový server přijímá data ze zdrojového počítače, optimalizuje je zašifruje a odešle ji tooAzure úložiště přes port 443 odchozí.
    - Pokud povolíte více virtuálních počítačů konzistence, pak počítače v replikační skupině hello vzájemně komunikovat přes port 20004. Konzistence více virtuálních počítačů znamená, že seskupíte víc virtuálních počítačů do replikační skupiny, v rámci které se sdílí body obnovení konzistentní vzhledem k selháním a konzistentní vzhledem k aplikacím, když dojde k převzetí služeb při selhání. To je užitečné, pokud počítače běží stejné zatížení hello a je třeba toobe konzistentní.
5. Přenosy jsou replikované tooAzure úložiště veřejné koncové body, přes hello internet. Alternativně můžete použít [veřejný partnerský vztah](../expressroute/expressroute-circuit-peerings.md#public-peering) Azure ExpressRoute. Provoz replikace prostřednictvím VPN site-to-site místní lokalitě tooAzure není podporována.

**Obrázek 2: Replikace fyzických tooAzure**

![Rozšířené](./media/physical-walkthrough-architecture/v2a-architecture-henry.png)

## <a name="failover-and-failback-process"></a>Proces převzetí služeb při selhání a navrácení služeb po obnovení

Po ověření, že převzetí služeb při selhání funguje podle očekávání, můžete spustit neplánované převzetí služeb při selhání tooAzure podle potřeby. Při spuštění převzetí služeb při selhání se virtuální počítače Azure vytvořené z replikovaná data. Pak, když primární místní lokalitu opět k dispozici, může selhat zpět. Poznámky:

- Plánované převzetí služeb není podporované.
- Můžete převzít jeden počítač nebo vytvořit [plány obnovení](site-recovery-create-recovery-plans.md), toofail přes víc počítačů společně.
- Potvrzení převzetí služeb při selhání toostart přístupem hello zatížení z hello repliky virtuálního počítače Azure.
- Když hello primární lokality je opět k dispozici, můžete replikaci hello počítače z primární lokality sekundární toohello hello. Potom spusťte neplánované převzetí služeb při selhání z hello sekundární lokality. Po potvrzení toto převzetí služeb při selhání, data budou zpět na místní a budete potřebovat tooAzure tooenable replikace.

Navrácení služeb po obnovení součásti zahrnují:

- **Dočasný procesní server v Azure**: budete potřebovat tooset nahoru virtuálnímu počítači Azure tooact jako procesní server, toohandle replikace z Azure. Tento virtuální počítač je možné po navrácení služeb po obnovení odstranit.
- **Připojení k síti VPN**: budete potřebovat připojení k síti VPN (nebo Azure ExpressRoute) z hello Azure toohello místní síť.
- **Samostatný místní hlavní cílový server**: hello místní hlavní cílový server (nainstalována ve výchozím nastavení na konfiguračním serveru hello) zpracovává navrácení služeb po obnovení. Pokud tak zpět velké objemy přenosů, měli byste nastavit samostatný místní hlavní cílový server pro tento účel.
- **Navrácení služeb po obnovení zásad**: budete potřebovat zásadu navrácení služeb po obnovení. Ty se vytvoří automaticky při vytvoření zásad replikace.
- **Infrastruktury VMware**: musí selhat zpět tooan místní virtuální počítač VMware. To znamená, že se vyžaduje místní infrastruktury VMware, i když replikujete tooAzure místní fyzických serverů.

**Obrázek 3: Fyzický server navrácení služeb po obnovení**

![Navrácení služeb po obnovení](./media/physical-walkthrough-architecture/enhanced-failback.png)


## <a name="next-steps"></a>Další kroky

Přejděte příliš[krok 2: ověření předpoklady a omezení](physical-walkthrough-prerequisites.md)
