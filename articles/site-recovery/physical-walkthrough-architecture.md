---
title: "Zkontrolujte architekturu pro fyzický server replikaci do Azure pomocí Azure Site Recovery | Microsoft Docs"
description: "Tento článek obsahuje přehled součásti a architektura používá při replikace místní fyzických serverů do Azure se službou Azure Site Recovery"
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
ms.openlocfilehash: 843c3f1b54f50fe50b162ed242deab717a080830
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="step-1-review-the-architecture-for-physical-server-replication-to-azure"></a>Krok 1: Posouzení architekturu pro fyzický server replikaci do Azure.

Tento článek popisuje součásti a procesům používaným při replikaci místní Windows nebo Linuxem fyzických serverů do Azure, pomocí [Azure Site Recovery](site-recovery-overview.md) služby.

Případné připomínky můžete publikovat na konci tohoto článku nebo na [fóru služby Azure Site Recovery](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="architectural-components"></a>Komponenty architektury

Tabulka shrnuje součásti, které potřebujete.

**Komponenta** | **Požadavek** | **Podrobnosti**
--- | --- | ---
**Azure** | Potřebujete účet Azure, účet úložiště Azure a sítě Azure. | Replikovaná data jsou uložena v účtu úložiště a vytvoří se virtuální počítače Azure s replikovaná data, když dojde k převzetí služeb při selhání. Virtuální počítače Azure připojit k virtuální síť Azure, když vytváří.
**Konfigurační server** | Jeden místní server pro správu (fyzický server nebo virtuální počítač VMware) používající všechny místní součásti Site Recovery. Mezi ně patří, a konfigurační server, procesový server, hlavní cílový server. | Komponenta konfiguračního serveru koordinuje komunikaci mezi místním prostředím a Azure a spravuje replikaci dat.
 **Procesový server**  | Obvykle se instaluje na konfigurační server. | Funguje jako replikační brána. Přijímá data replikace, optimalizuje je pomocí ukládání do mezipaměti, komprese a šifrování a odesílá je do úložiště Azure.<br/><br/> Procesový server také obstará nabízenou instalaci služby Mobility na chráněné počítače.<br/><br/> Můžete přidat další samostatný proces vyhrazené servery pro zpracování zvyšující se množství provoz replikace.
 **Hlavní cílový server** | Obvykle se instaluje na místní konfigurační server. | Zpracovává replikační data během navracení služeb z Azure po obnovení.<br/><br/> Pokud je objem přenosů při navracení služeb po obnovení příliš vysoký, můžete nasadit samostatný hlavní cílový server pro účely navrácení služeb po obnovení.
**Replikované servery** | Službu mobility je nainstalována na každém serveru systému Windows nebo Linux, které chcete replikovat. Můžete ji nainstalovat na každý počítač ručně, nebo pomocí nabízené instalace z procesového serveru.
**Navrácení služeb po obnovení** | Navrácení služeb po obnovení VMware je infrastruktura vyžaduje. Nelze navrátit služby po obnovení v případě fyzického serveru.


**Obrázek 1: Fyzických součástí Azure**

![Komponenty](./media/physical-walkthrough-architecture/arch-enhanced.png)

## <a name="replication-process"></a>Proces replikace

1. Nastavení nasazení, včetně místní a Azure součásti. V trezoru služeb zotavení je třeba zadat replikace zdroje a cíle, nastavení konfigurace serveru, vytvořte zásadu replikace, nasazení služby Mobility, povolit replikaci a spustit testovací převzetí služeb.
2.  Replikovat počítače v souladu s zásady replikace a počáteční kopii data se replikují do úložiště Azure.
4. Po dokončení počáteční replikace začne replikace rozdílové změny do Azure. Sledované změny se pro jednotlivé počítače ukládají do souboru .hrl.
    - Počítače, které se replikují, komunikují s konfiguračním serverem kvůli správě replikace na příchozím portu HTTPS 443.
    - Replikaci počítačů odesílat data replikace na procesový server na port HTTPS 9443 příchozí (lze upravit).
    - Konfigurační server orchestruje správu replikace s Azure přes odchozí port HTTPS 443.
    - Procesový server přijímá data ze zdrojového počítače, optimalizuje je a šifruje, a pak je odesílá do úložiště Azure přes odchozí port 443.
    - Když povolíte konzistenci mezi více virtuálními počítači, budou spolu počítače v replikační skupině komunikovat na portu 20004. Konzistence více virtuálních počítačů znamená, že seskupíte víc virtuálních počítačů do replikační skupiny, v rámci které se sdílí body obnovení konzistentní vzhledem k selháním a konzistentní vzhledem k aplikacím, když dojde k převzetí služeb při selhání. To je užitečné, pokud je počítačích spuštěná stejná úloha a je třeba, aby zůstala konzistentní.
5. Provoz se přes internet replikuje do veřejných koncových bodů úložiště Azure. Alternativně můžete použít [veřejný partnerský vztah](../expressroute/expressroute-circuit-peerings.md#public-peering) Azure ExpressRoute. Přenos replikačních dat přes síť site-to-site VPN z místního serveru do Azure není podporovaný.

**Obrázek 2: Fyzicky Azure replikace**

![Rozšířené](./media/physical-walkthrough-architecture/v2a-architecture-henry.png)

## <a name="failover-and-failback-process"></a>Proces převzetí služeb při selhání a navrácení služeb po obnovení

Po ověření správného fungování testovacího převzetí služeb při selhání můžete podle potřeby spouštět neplánovaná převzetí služeb při selhání do Azure. Při spuštění převzetí služeb při selhání se virtuální počítače Azure vytvořené z replikovaná data. Pak, když primární místní lokalitu opět k dispozici, může selhat zpět. Poznámky:

- Plánované převzetí služeb není podporované.
- Můžete převzít jeden počítač nebo vytvořit [plány obnovení](site-recovery-create-recovery-plans.md), aby převzetí služeb při selhání více počítačů současně.
- Po potvrzení procesu převzetí služeb můžete začít používat úlohu na replikovaném virtuálním počítači Azure.
- Když primární lokality je opět k dispozici, replikovat do primární lokality je počítač ze sekundární. Potom spusťte neplánované převzetí služeb při selhání ze sekundární lokality. Jakmile toto navrácení služeb potvrdíte, budou data zpět v místní lokalitě a budete muset opět povolit replikaci do Azure.

Navrácení služeb po obnovení součásti zahrnují:

- **Dočasný procesní server v Azure**: je třeba nastavit virtuální počítač Azure tak, aby fungoval jako server proces pro zpracování replikace z Azure. Tento virtuální počítač je možné po navrácení služeb po obnovení odstranit.
- **Připojení k síti VPN**: potřebovat připojení VPN (nebo Azure ExpressRoute) ze sítě Azure k místní lokalitě.
- **Samostatný místní hlavní cílový server**: místní hlavní cílový server (nainstalována ve výchozím nastavení na konfiguračním serveru) zpracovává navrácení služeb po obnovení. Pokud tak zpět velké objemy přenosů, měli byste nastavit samostatný místní hlavní cílový server pro tento účel.
- **Navrácení služeb po obnovení zásad**: budete potřebovat zásadu navrácení služeb po obnovení. Ty se vytvoří automaticky při vytvoření zásad replikace.
- **Infrastruktura VMware:** Musíte navrátit služby po obnovení do místního virtuálního počítače VMware. To znamená, že potřebujete místní infrastrukturu VMware, i když místní fyzické servery replikujete do Azure.

**Obrázek 3: Fyzický server navrácení služeb po obnovení**

![Navrácení služeb po obnovení](./media/physical-walkthrough-architecture/enhanced-failback.png)


## <a name="next-steps"></a>Další kroky

Přejděte na [krok 2: ověření předpoklady a omezení](physical-walkthrough-prerequisites.md)
