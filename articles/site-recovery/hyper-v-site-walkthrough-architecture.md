---
title: Architektura hello aaaReview tooAzure replikace (bez System Center VMM) technologie Hyper-V s Azure Site Recovery | Microsoft Docs
description: "Tento článek obsahuje přehled součásti a architektura použít při replikaci virtuálních počítačů technologie Hyper-V (bez VMM) tooAzure místní službou Azure Site Recovery hello."
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
ms.date: 06/22/2017
ms.author: raynew
ms.openlocfilehash: ec148e87ab2fa17485001ee447ad9f9bf795840e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-1-review-hello-architecture-for-hyper-v-replication-tooazure"></a>Krok 1: Posouzení hello architektura pro tooAzure replikace technologie Hyper-V


Tento článek popisuje hello součásti a procesů při replikaci lokálních virtuálních počítačů Hyper-V (které nejsou spravovány nástrojem System Center VMM), tooAzure pomocí hello [Azure Site Recovery](site-recovery-overview.md) služby.

Odeslat všechny komentáře dole hello v tomto článku, nebo v hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).



## <a name="architectural-components"></a>Komponenty architektury

Existuje řada komponent zahrnutých při replikaci virtuálních počítačů Hyper-V tooAzure bez VMM.

**Komponenta** | **Umístění** | **Podrobnosti**
--- | --- | ---
**Azure** | Pro Azure potřebujete účet Microsoft Azure, účet úložiště Azure a síť Azure. | Replikovaná data jsou uložena v účtu úložiště hello a vytvoří se virtuální počítače Azure s hello replikovat data, když dojde k převzetí služeb při selhání z vaší místní lokalitě.<br/><br/> Hello virtuálních počítačích Azure připojit toohello virtuální síť Azure, když vytváří.
**Hyper-V** | Hostitelé a clustery Hyper-V se shromažďují do lokalit Hyper-V. agent zprostředkovatele Azure Site Recovery a obnovení služby Hello je nainstalován na každém hostiteli technologie Hyper-V. | Hello zprostředkovatele orchestruje replikaci pomocí Site Recovery přes hello Internetu. agenta služeb zotavení Hello zpracovává data replikace.<br/><br/> Komunikace z hello poskytovatele i agenta hello je zabezpečená a šifrovaná. Šifrují se rovněž replikovaná data v úložišti Azure.
**Virtuální počítače Hyper-V** | Na hostitelském serveru Hyper-V potřebujete jeden nebo několik spuštěných virtuálních počítačů. | Nic musí tooexplicitly nainstalovaných na virtuálních počítačích.

Další informace o požadavcích pro nasazení hello a požadavcích pro každou z těchto součástí v hello [matici podpory](site-recovery-support-matrix-to-azure.md).

**Obrázek 1: Technologie Hyper-V lokality tooAzure replikace**

![Komponenty](./media/hyper-v-site-walkthrough-architecture/arch-onprem-azure-hypervsite.png)


## <a name="replication-process"></a>Proces replikace

**Obrázek 2: Replikace a obnovení proces tooAzure replikace technologie Hyper-V**

![pracovní postup](./media/hyper-v-site-walkthrough-architecture/arch-hyperv-azure-workflow.png)

### <a name="enable-protection"></a>Povolení ochrany

1. Po povolení ochrany pro virtuální počítač technologie Hyper-V v hello portálu Azure nebo místním hello **povolení ochrany** spustí.
2. Hello úlohy se zkontroluje, aby hello počítač splňuje požadavky, před vyvoláním hello [CreateReplicationRelationship](https://msdn.microsoft.com/library/hh850036.aspx), tooset replikaci s hello nastavení, které jste nakonfigurovali.
3. Hello úloha spustí počáteční replikace vyvoláním hello [StartReplication](https://msdn.microsoft.com/library/hh850303.aspx) metodu, tooinitialize úplné replikace virtuálního počítače a virtuální disky tooAzure odesílání hello Virtuálního počítače.
4. Můžete monitorovat hello úlohy v hello **úlohy** kartě.
 
### <a name="replicate-hello-initial-data"></a>Replikovat počáteční data hello

1. Při aktivaci počáteční replikace se pořídí [snímek virtuálního počítače Hyper-V](https://technet.microsoft.com/library/dd560637.aspx).
2. Virtuální pevné disky se replikují jeden po druhém, dokud jsou všechny zkopírovaný tooAzure. Ho může nějakou dobu trvat, v závislosti na hello velikost virtuálního počítače a šířku pásma sítě. toooptimize využití sítě, najdete v části [jak toomanage místní využití šířky pásma sítě ochrany tooAzure](https://support.microsoft.com/kb/3056159).
3. Pokud dojde k změny na disku, když probíhá počáteční replikace, hello technologie Hyper-V Replica Replication Tracker sleduje tyto změny jako protokoly replikace technologie Hyper-V (.hrl). Tyto soubory jsou umístěny v hello stejné složce jako disky hello. Každý disk má přidružený soubor .hrl, odešle toosecondary úložiště.
4. Hello soubory snímků a protokolů spotřebovávají prostředky disku, když probíhá počáteční replikace.
5. Po dokončení počáteční replikace hello je odstranění snímku virtuálního počítače hello. Rozdílové změny na disku v protokolu hello jsou synchronizovaná a sloučené toohello nadřazený disk.


### <a name="finalize-protection"></a>Dokončení ochrany

1. Po počáteční replikaci hello dokončení hello **dokončit ochranu na virtuálním počítači hello** nakonfiguruje úloha sítě a další postreplikační nastavení tak, aby hello virtuální počítač je chráněný.
2. Pokud replikujete tooAzure, můžete potřebovat tootweak hello nastavení pro virtuální počítač hello tak, aby byl připraven k převzetí služeb při selhání. V tomto okamžiku můžete spustit toocheck testovací převzetí služeb při selhání, které všechno funguje podle očekávání.

### <a name="replicate-hello-delta"></a>Replikovat rozdílová hello

1. Po počáteční replikaci hello zahájí rozdílová synchronizace, v souladu s nastavením replikace.
2. Hello technologie Hyper-V Replica Replication Tracker sleduje změny hello tooa virtuální pevný disk jako soubory .hrl. Každý disk nakonfigurovaný pro replikaci má přidružený soubor .hrl. Tento protokol se odesílají toohello zákazníkův účet úložiště, po dokončení počáteční replikace. Pokud je do protokolu přenosu tooAzure, se sledují změny hello hello primární disku v souboru protokolu, v hello stejný adresář.
3. Během počáteční a rozdílové replikace můžete monitorovat hello virtuálních počítačů v hello zobrazení virtuálních počítačů. [Další informace](site-recovery-monitoring-and-troubleshooting.md#monitor-replication-health-for-virtual-machines).  

### <a name="synchronize-replication"></a>Synchronizace replikace

1. Pokud rozdílová replikace selže a úplná replikace by byla náročná, pokud jde o šířku pásma nebo čas, pak se virtuální počítač označí pro resynchronizaci. Například pokud hello soubory .hrl dosáhnou 50 % velikosti disku hello, pak hello virtuálního počítače budou označeny pro opětovnou synchronizaci.
2.  Resynchronizace minimalizuje hello množství dat odesílaných výpočtů kontrolních součtů hello zdrojové a cílové virtuální počítače a odesláním pouze hello rozdílová data. Resynchronizace využívá algoritmus vytváření bloků s pevnou velikostí, kdy jsou zdrojové a cílové soubory rozdělené do pevných bloků. Kontrolní součty pro každého bloku jsou generovány a potom porovná toodetermine, která blokuje z hello zdroj nutné toobe použité toohello cíle.
3. Po dokončení resynchronizace by měla pokračovat normální rozdílová replikace. Ve výchozím nastavení se Opakovaná synchronizace naplánované toorun automaticky mimo pracovní dobu, ale můžete ji u virtuálního počítače ručně. Například můžete pokračovat v resynchronizaci, pokud dojde k výpadku sítě nebo jinému výpadku. toodo se vyberte hello virtuálních počítačů v portálu hello > **Opětovná synchronizace**.

    ![Ruční resynchronizace](./media/hyper-v-site-walkthrough-architecture/image4.png)


### <a name="retry-logic"></a>Logika opakování

Pokud dojde k chybě replikace, je předdefinován opakovaný pokus. Tuto logiku je možné rozdělit do dvou kategorií:

**Kategorie** | **Podrobnosti**
--- | ---
**Neopravitelné chyby** | Pokus se nebude opakovat. Stav virtuálního počítače bude **Kritický** a bude nutný zásah správce. Příklady těchto chyb: přerušený řetězec virtuálního pevného disku; Neplatný stav hello repliky virtuálních počítačů; Chyby ověřování sítě: chyb autorizace; Chyby nenalezení (pro samostatné servery Hyper-V) virtuální počítač
**Opravitelné chyby** | Opakování dojde k každých intervalu replikace pomocí exponenciální back vypnout, která zvyšuje interval opakování hello z hello začátek hello první pokus o 1, 2, 4, 8 a 10 minut. Pokud chyba přetrvává, bude se pokus opakovat každých 30 minut. Příkladem můžou být chyby sítě, chyby kvůli malému místu na disku nebo nedostatek paměti. |



## <a name="failover-and-failback-process"></a>Proces převzetí služeb při selhání a navrácení služeb po obnovení

1. Můžete spustit plánované, nebo neplánované [převzetí služeb při selhání](site-recovery-failover.md) z tooAzure místní virtuální počítače Hyper-V. Pokud spustíte plánované převzetí služeb při selhání, pak zdrojové virtuální počítače jsou vypnout tooensure nedošlo ke ztrátě dat.
2. Můžete převzít jeden počítač nebo vytvořit [plány obnovení](site-recovery-create-recovery-plans.md) tooorchestrate převzetí služeb při selhání více počítačů.
4. Po spuštění hello převzetí služeb při selhání, musí být schopný toosee hello vytvoří replikované virtuální počítače v Azure. V případě potřeby můžete přiřadit toohello veřejnou adresu IP virtuálního počítače.
5. Potom potvrzení toostart hello převzetí služeb při selhání přístupu k hello zatížení z hello repliky virtuálního počítače Azure.
6. Až bude vaše místní lokalita opět dostupná, můžete [navrátit služby po obnovení](site-recovery-failback-from-azure-to-hyper-v.md). Ji plánované převzetí služeb při selhání z Azure toohello primární lokality. Plánované převzetí služeb, které můžete vyberte toofailback toohello stejného virtuálního počítače nebo tooan alternativní umístění a synchronizovat změny mezi Azure a místními, tooensure nedošlo ke ztrátě dat. Virtuální počítače vytvořené na místě, potvrzení převzetí služeb při selhání hello.




## <a name="next-steps"></a>Další kroky

Přejděte příliš[krok 2: Přečtěte si požadavky nasazení hello](hyper-v-site-walkthrough-prerequisites.md)
