---
title: "aaaHow funguje místní počítač replikace tooa sekundární místní lokalitu ve službě Azure Site Recovery? | Dokumentace Microsoftu"
description: "Tento článek obsahuje přehled součásti a architektura používá při replikovat místní virtuální počítače a fyzické servery tooa sekundární lokalitu s hello služba Azure Site Recovery."
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
ms.topic: get-started-article
ms.date: 05/29/2017
ms.author: raynew
ms.openlocfilehash: 097a3f43446fec69ed7f9e0b7f11e8d11f41cc6a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-does-on-premises-machine-replication-tooa-secondary-site-work-in-site-recovery"></a>Jak místní počítač pracovní sekundární lokality tooa replikace ve službě Site Recovery?

Tento článek popisuje hello součásti a procesů zahrnutých při replikaci lokálních virtuálních počítačů a fyzických serverů tooAzure, pomocí hello [Azure Site Recovery](site-recovery-overview.md) služby.

Můžete replikovat hello následující tooa sekundární místní lokalitu:
- Místní virtuální počítače Hyper-V na clustery Hyper-V a samostatné hostitele, které jsou spravované v cloudech System Center Virtual Machine Manageru (VMM).
- Místní virtuální počítače VMware a fyzické servery s Windows nebo Linuxem. V tomto scénáři replikaci spravuje Scout.

Odeslat všechny komentáře dole hello v tomto článku, nebo v hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="replicate-hyper-v-vms-tooa-secondary-on-premises-site"></a>Replikovat virtuální počítače Hyper-V tooa sekundární místní lokalitu


### <a name="architectural-components"></a>Komponenty architektury

Zde je, co potřebujete pro replikaci virtuálních počítačů Hyper-V tooa sekundární lokality.

**Komponenta** | **Umístění** | **Podrobnosti**
--- | --- | ---
**Azure** | Budete potřebovat účet Microsoft. |
**Server VMM** | Doporučujeme, abyste server VMM v primární lokalitě hello a po jednom v sekundární lokalitě hello | Každý server VMM musí být připojené toohello Internetu.<br/><br/> Každý server musí mít alespoň jeden privátní cloud VMM, sadou profil schopností hello technologie Hyper-V.<br/><br/> Hello zprostředkovatele Azure Site Recovery nainstalujete na hello VMM server. Hello zprostředkovatel koordinuje a orchestruje replikaci pomocí hello služba Site Recovery přes hello Internetu. Komunikace mezi hello zprostředkovatele a Azure je zabezpečená a šifrovaná.
**Server Hyper-V** |  Jeden nebo více servery Hyper-V host v hello primárních a sekundárních cloudech VMM.<br/><br/> Servery musí být připojené toohello Internetu.<br/><br/> Data se replikují mezi hello primární a sekundární servery Hyper-V hostiteli prostřednictvím hello LAN nebo VPN pomocí protokolu Kerberos nebo ověření certifikátu.  
**Virtuální počítače Hyper-V** | Umístěný na serveru hostitele technologie Hyper-V hello zdroje. | Hello zdrojový hostitelský server by měl mít aspoň jeden virtuální počítač, který má tooreplicate.

### <a name="replication-process"></a>Proces replikace

1. Nastavit hello účet Azure.
2. Vytvoříte trezor služby Replication Services pro Site Recovery a nakonfigurujete jeho nastavení, včetně těchto:

    - Hello replikace zdroj a cíl (primárních a sekundárních lokalit).
    - Instalace hello zprostředkovatele Azure Site Recovery a agenta služeb zotavení Microsoft Azure hello. Hello zprostředkovatele je nainstalován na serverech VMM a agenta hello na každém hostiteli technologie Hyper-V.
    - Vytvoříte zásady replikace pro zdrojový cloud VMM. zásady Hello je použité tooall virtuální počítače umístěné na hostitelích v cloudu hello.
    - Povolíte replikaci pro virtuální počítače Hyper-V. V souladu s nastavením zásad replikace hello dojde k počáteční replikaci.
4. Se sledují změny dat a replikace rozdílové změny toobegins po dokončení počáteční replikace hello. Sledované změny se pro jednotlivé položky ukládají do souboru .hrl.
5. Při spuštění toomake testovací převzetí služeb při selhání, zda vše funguje.

**Obrázek 1: VMM tooVMM replikace**

![Místní tooon místní](./media/site-recovery-components/arch-onprem-onprem.png)

### <a name="failover-and-failback-process"></a>Proces převzetí služeb při selhání a navrácení služeb po obnovení

1. Můžete spustit plánované nebo neplánované [převzetí služeb při selhání](site-recovery-failover.md) mezi místními lokalitami. Pokud spustíte plánované převzetí služeb při selhání, pak zdrojové virtuální počítače jsou vypnout tooensure nedošlo ke ztrátě dat.
2. Můžete převzít jeden počítač nebo vytvořit [plány obnovení](site-recovery-create-recovery-plans.md) tooorchestrate převzetí služeb při selhání více počítačů.
4. Pokud provádíte tooa sekundární lokality neplánované převzetí služeb při selhání, po převzetí služeb při selhání počítače hello v hello sekundárního umístění nejsou povolené pro ochranu nebo replikace. Pokud jste spustili plánované převzetí služeb při selhání, po převzetí služeb při selhání hello, jsou chráněné počítače v hello sekundárního umístění.
5. Poté potvrďte hello převzetí služeb při selhání toostart přístupem hello zatížení z hello repliky virtuálních počítačů.
6. Při primární lokality je opět k dispozici, můžete zahájit tooreplicate zpětná replikace z primární toohello sekundární lokality hello. Zpětná replikace přináší hello virtuální počítače v chráněném stavu, ale hello sekundárního datového centra je stále aktivní umístění hello.
7. toomake hello primární lokalitu do umístění služby active hello znovu spustíte plánované převzetí služeb při selhání z sekundární tooprimary, za nímž následuje jiný zpětné replikace.




## <a name="replicate-vmware-vmsphysical-servers-tooa-secondary-site"></a>Replikovat virtuální počítače VMware nebo fyzické servery tooa sekundární lokality

Můžete replikovat virtuální počítače VMware nebo fyzických serverů tooa sekundární lokality pomocí nástroje InMage Scout, pomocí těchto součástí architektury:


### <a name="architectural-components"></a>Komponenty architektury

**Komponenta** | **Umístění** | **Podrobnosti**
--- | --- | ---
**Azure** | InMage Scout. | tooobtain InMage Scout potřebujete předplatné Azure.<br/><br/> Po vytvoření trezoru služeb zotavení, stáhnete InMage Scout a nainstalujete hello nejnovější aktualizace tooset až hello nasazení.
**Procesový server** | Umístěný v primární lokalitě | Nasadíte ukládání do mezipaměti toohandle hello proces serveru, kompresi a optimalizaci data.<br/><br/> Také obstará nabízenou instalaci hello chcete tooprotect toomachines Unified Agent.
**Konfigurační server** | Umístěný v sekundární lokalitě | Hello konfigurační server spravuje, konfiguraci a monitorování vašeho nasazení – buď pomocí webu pro správu hello nebo konzole vContinuum hello.
**Server vContinuum** | Volitelné. Nainstalované v hello stejné umístění jako hello konfigurační server. | Poskytuje konzoli pro správu a monitorování chráněného prostředí.
**Hlavní cílový server** | V sekundární lokalitě hello | Hello hlavní cílový server uchovává replikovaná data. Přijímá data z hello procesového serveru, vytváří repliku počítače v sekundární lokalitě hello a obsahuje body pro uchovávání hello.<br/><br/> Hello počet hlavních cílových serverů, které potřebujete, závisí na hello počtu počítačů, které chcete chránit.<br/><br/> Pokud chcete toofail back toohello primární lokality, potřebujete hlavní cílový server existuje příliš. Hello Unified Agent je na tomto serveru nainstalován.
**VMware ESX/ESXi a server vCenter** |  Virtuální počítače jsou hostované na hostitelích ESX nebo ESXi. Hostitelé se spravují pomocí serveru vCenter. | Je nutné tooreplicate infrastruktury VMware virtuálních počítačů VMware.
**Virtuální počítače / fyzické servery** |  Unified Agent nainstalovaný na virtuální počítače VMware a fyzické servery, které chcete tooreplicate. | Hello agent funguje jako zprostředkovatel komunikace mezi všechny součásti hello.


### <a name="replication-process"></a>Proces replikace

1. Nastavit hello součástí serverů v každé lokalitě (konfigurace, proces, hlavní cílový) a nainstalujte hello Unified Agent na počítače, které chcete tooreplicate.
2. Po počáteční replikaci odesílá hello agent na každém počítači rozdílové replikační změny toohello procesový server.
3. Hello procesový server hello dat optimalizuje a přenese na sekundární lokalitě hello toohello hlavní cílový server. Hello konfigurační server spravuje replikační proces hello.

**Obrázek 2: VMware tooVMware replikace**

![VMware tooVMware](./media/site-recovery-components/vmware-to-vmware.png)


## <a name="next-steps"></a>Další kroky

Zkontrolujte hello [matici podpory](site-recovery-support-matrix-to-sec-site.md)
