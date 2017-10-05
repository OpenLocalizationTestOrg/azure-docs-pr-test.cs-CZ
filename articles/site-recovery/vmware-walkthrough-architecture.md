---
title: Zkontrolujte architekturu replikace VMware do Azure | Microsoft Docs
description: "Tento článek obsahuje přehled součásti a architektura použít při replikaci místní virtuální počítače VMware do Azure se službou Azure Site Recovery"
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
ms.openlocfilehash: 4aae04a793bab11562c20ceec0e1ae8f1a035a0f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="step-1-review-the-architecture-for-vmware-replication-to-azure"></a>Krok 1: Posouzení architekturu replikace VMware do Azure

Tento článek popisuje součásti a procesům používaným při replikaci virtuálních počítačů VMware místně na Azure pomocí [Azure Site Recovery](site-recovery-overview.md) služby.

Případné připomínky můžete publikovat na konci tohoto článku nebo na [fóru služby Azure Site Recovery](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="architectural-components"></a>Komponenty architektury

Tabulka shrnuje součásti, které potřebujete.

**Komponenta** | **Požadavek** | **Podrobnosti**
--- | --- | ---
**Azure** | Potřebujete předplatné Azure, účet úložiště Azure a sítě Azure. | Replikovaná data se ukládají v účtu úložiště. Virtuální počítače Azure vytvořené mají replikovaná data, když dojde k převzetí služeb při selhání z vaší místní lokalitě. Virtuální počítače Azure se připojí k virtuální síti Azure po svém vytvoření.
**Konfigurační server** | Jeden server správy (virtuální počítač VMware), který spouští všechny místní součásti Site Recovery pro nasazení na místní. Mezi ně patří, a konfigurační server, procesový server, hlavní cílový server. | Komponenta konfiguračního serveru koordinuje komunikaci mezi místním prostředím a Azure a spravuje replikaci dat.
 **Procesový server:**  | Obvykle se instaluje na konfigurační server. | Funguje jako replikační brána. Přijímá data replikace, optimalizuje je pomocí ukládání do mezipaměti, komprese a šifrování a odesílá je do úložiště Azure.<br/><br/> Procesový server se také stará o nabízenou instalaci služby Mobility na chráněné počítače a provádí automatické zjišťování virtuálních počítačů VMware.<br/><br/> Jak vaše nasazení poroste, můžete přidávat další samostatné vyhrazené procesní servery pro zpracování zvyšujícího se objemu replikačních přenosů.
 **Hlavní cílový server** | Obvykle se instaluje na místní konfigurační server. | Zpracovává replikační data během navracení služeb z Azure po obnovení.<br/><br/> Pokud je objem přenosů při navracení služeb po obnovení příliš vysoký, můžete nasadit samostatný hlavní cílový server pro účely navrácení služeb po obnovení.
**Servery VMware** | Virtuální počítače VMware jsou hostované na serverech vSphere ESXi a ke správě hostitelů doporučujeme použít server vCenter. | Servery VMware přidáte do trezoru služby Recovery Services.
**Replikované počítače** | Služba Mobility se nainstalují na jednotlivé virtuální počítače VMware můžete replikovat. Můžete ji nainstalovat na každý počítač ručně, nebo pomocí nabízené instalace z procesového serveru.

**Obr. 1: Komponenty pro replikaci z VMware do Azure**

![Komponenty](./media/vmware-walkthrough-architecture/arch-enhanced.png)

## <a name="replication-process"></a>Proces replikace

1. Nastavení nasazení, včetně místní a Azure součásti. V trezoru služeb zotavení je třeba zadat replikace zdroje a cíle, nastavení konfigurace serveru, vytvořte zásadu replikace, nasazení služby Mobility, povolit replikaci a spustit testovací převzetí služeb.
2. Replikovat počítače v souladu s zásady replikace a počáteční kopii data se replikují do úložiště Azure.
3. Po dokončení počáteční replikace začne replikace rozdílové změny do Azure. Sledované změny se pro jednotlivé počítače ukládají do souboru .hrl.
    - Počítače, které se replikují, komunikují s konfiguračním serverem kvůli správě replikace na příchozím portu HTTPS 443.
    - Replikaci počítačů odesílat data replikace na procesový server na port HTTPS 9443 příchozí (lze upravit).
    - Konfigurační server orchestruje správu replikace s Azure přes odchozí port HTTPS 443.
    - Procesový server přijímá data ze zdrojového počítače, optimalizuje je a šifruje, a pak je odesílá do úložiště Azure přes odchozí port 443.
    - Když povolíte konzistenci mezi více virtuálními počítači, budou spolu počítače v replikační skupině komunikovat na portu 20004. Konzistence více virtuálních počítačů znamená, že seskupíte víc virtuálních počítačů do replikační skupiny, v rámci které se sdílí body obnovení konzistentní vzhledem k selháním a konzistentní vzhledem k aplikacím, když dojde k převzetí služeb při selhání. To je užitečné, pokud je počítačích spuštěná stejná úloha a je třeba, aby zůstala konzistentní.
4. Provoz se přes internet replikuje do veřejných koncových bodů úložiště Azure. Alternativně můžete použít [veřejný partnerský vztah](../expressroute/expressroute-circuit-peerings.md#public-peering) Azure ExpressRoute. Přenos replikačních dat přes síť site-to-site VPN z místního serveru do Azure není podporovaný.


**Obr. 2: Replikace z VMware do Azure**

![Rozšířené](./media/vmware-walkthrough-architecture/v2a-architecture-henry.png)

## <a name="failover-and-failback-process"></a>Proces převzetí služeb při selhání a navrácení služeb po obnovení

1. Po ověření správného fungování testovacího převzetí služeb při selhání můžete podle potřeby spouštět neplánovaná převzetí služeb při selhání do Azure. Plánované převzetí služeb není podporované.
2. Můžete převzít služby při selhání jednoho počítače nebo vytvořit [plány obnovení](site-recovery-create-recovery-plans.md) pro převzetí služeb při selhání více virtuálních počítačů.
3. V průběhu převzetí služeb při selhání se v Azure vytvoří replika virtuálního počítače. Po potvrzení procesu převzetí služeb můžete začít používat úlohu na replikovaném virtuálním počítači Azure.
4. Až bude vaše místní lokalita opět dostupná, můžete službu navrátit. Nastavíte infrastrukturu navrácení služeb po obnovení, zahájíte replikaci počítače ze sekundární lokality na primární a spustíte neplánované převzetí služeb při selhání ze sekundární lokality. Jakmile toto navrácení služeb potvrdíte, budou data zpět v místní lokalitě a budete muset opět povolit replikaci do Azure. [Další informace](site-recovery-failback-azure-to-vmware.md)

Pro navrácení služby po obnovení existuje několik požadavků:


- **Dočasný procesní server v Azure**: Pokud chcete po převzetí služeb při selhání tyto služby po obnovení vrátit, budete muset pro zpracování replikace z Azure nastavit virtuální počítač Azure nakonfigurovaný jako procesní server. Tento virtuální počítač je možné po navrácení služeb po obnovení odstranit.
- **Připojení k síti VPN:** Pro navrácení služeb po obnovení budete potřebovat připojení k síti VPN (nebo Azure ExpressRoute) nastavené ze sítě Azure k místní lokalitě.
- **Samostatný lokální hlavní cílový server:** Místní hlavní cílový server se stará o navrácení služeb po obnovení. Hlavní cílový server je ve výchozím nastavení nainstalován na serveru pro správu, ale pokud po obnovení vracíte větší objemy přenosů, měli byste pro tento účel nastavit samostatný místní hlavní cílový server.
- **Zásady navrácení služeb po obnovení:** Pro zpětnou replikaci do vaší místní lokality budete potřebovat zásady navrácení služeb. Ty se vytvoří automaticky při vytvoření zásad replikace.
- **Infrastruktura VMware:** Musíte navrátit služby po obnovení do místního virtuálního počítače VMware. To znamená, že potřebujete místní infrastrukturu VMware, i když místní fyzické servery replikujete do Azure.

**Obr. 3: Navrácení služeb po obnovení – VMware nebo fyzický server**

![Navrácení služeb po obnovení](./media/vmware-walkthrough-architecture/enhanced-failback.png)


## <a name="next-steps"></a>Další kroky

Přejděte na [krok 2: ověření předpoklady a omezení](vmware-walkthrough-prerequisites.md)
