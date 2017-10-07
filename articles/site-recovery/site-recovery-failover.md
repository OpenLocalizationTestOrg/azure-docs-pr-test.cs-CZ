---
title: "aaaFailover ve službě Site Recovery | Microsoft Docs"
description: "Azure Site Recovery koordinuje hello replikace, převzetí služeb při selhání a obnovení virtuálních počítačů a fyzických serverů. Další informace o převzetí služeb při selhání tooAzure nebo do sekundárního datacentra."
services: site-recovery
documentationcenter: 
author: prateek9us
manager: gauravd
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/04/2017
ms.author: pratshar
ms.openlocfilehash: 7cacea829d78bb7de2b2d67402291b472b10f023
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="failover-in-site-recovery"></a>Převzetí služeb při selhání v Site Recovery
Tento článek popisuje, jak toofailover virtuálních počítačů a fyzických serverů chráněný pomocí Site Recovery.

## <a name="prerequisites"></a>Požadavky
1. Před provedením převzetí služeb při selhání, proveďte [testovací převzetí služeb při selhání](site-recovery-test-failover-to-azure.md) tooensure, který všechno funguje podle očekávání.
1. [Příprava hello sítě](site-recovery-network-design.md) v cílovém umístění, před provedením převzetí služeb při selhání.  


## <a name="run-a-failover"></a>Spuštění převzetí služeb při selhání
Tento postup popisuje, jak toorun a převzetí služeb při selhání pro [plán obnovení](site-recovery-create-recovery-plans.md). Případně můžete spustit hello převzetí služeb při selhání pro jeden virtuální počítač nebo fyzický server z hello **replikované položky** stránky


![Převzetí služeb při selhání](./media/site-recovery-failover/Failover.png)

1. Vyberte **plány obnovení** > *recoveryplan_name*. Klikněte na tlačítko **převzetí služeb při selhání**
2. Na hello **převzetí služeb při selhání** obrazovku, vyberte **bod obnovení** toofailover k. Můžete použít jednu z hello následující možnosti:
    1.  **Nejnovější** (výchozí): tuto možnost napřed zpracuje všechny hello data, která byla odeslaná tooSite obnovení služby toocreate bod obnovení pro každý virtuální počítač, než selhání tooit. Tato možnost poskytuje hello nejnižší plánovaný bod obnovení (plánovaného bodu obnovení) jako hello virtuálnímu počítači vytvořenému po převzetí služeb při selhání se všechna data hello které bylo replikované služby zotavení tooSite, kdy byla aktivována hello převzetí služeb při selhání.
    1.  **Nejnovější zpracované**: tuto možnost převezme všechny virtuální počítače v hello obnovení plán toohello nejnovější bod obnovení, který již byl zpracován službou Site Recovery. Při provádění převzetí služeb při selhání virtuálního počítače, je také zobrazit časového razítka posledního bodu obnovení zpracování hello. Při provádění převzetí služeb při selhání plánu obnovení, můžete se vrátit tooindividual virtuálního počítače a podívejte se na **body obnovení nejnovější** dlaždici tooget tyto informace. Jak je žádný čas strávený tooprocess hello nezpracovaných dat, tato možnost nabízí možnost převzetí služeb při selhání, nízkou RTO (plánovanou dobu obnovení).
    1.  **Nejnovější aplikace konzistentní**: tuto možnost převezme všechny virtuální počítače v hello obnovení plán toohello nejnovější aplikace konzistentní bod obnovení, který již byl zpracován službou Site Recovery. Při provádění převzetí služeb při selhání virtuálního počítače, je také zobrazit časové razítko poslední bod obnovení konzistentních s aplikací hello. Při provádění převzetí služeb při selhání plánu obnovení, můžete se vrátit tooindividual virtuálního počítače a podívejte se na **body obnovení nejnovější** dlaždici tooget tyto informace.
    1.  **Nejnovější více virtuálních počítačů zpracovat**: Tato možnost je dostupná jenom pro plány obnovení, které mají alespoň jeden virtuální počítač s konzistencí pro víc virtuálních počítačů na. Virtuální počítače, které jsou součástí replikační skupiny převzetí služeb při selhání toohello nejnovější běžné více virtuálních počítačů konzistentní obnovení bodu. Ostatní virtuální počítače převzetí služeb při selhání tootheir nejnovější zpracované bod obnovení.  
    1.  **Nejnovější více virtuálních počítačů konzistentní**: Tato možnost je dostupná jenom pro plány obnovení, které mají alespoň jeden virtuální počítač s ON konzistence více virtuálních počítačů. Virtuální počítače, které jsou součástí replikační skupiny převzetí služeb při selhání toohello nejnovější společný více virtuálních počítačů konzistentní s aplikací bod obnovení. Ostatní virtuální počítače převzetí služeb při selhání tootheir nejnovější konzistentní s aplikací bodu obnovení.
    1.  **Vlastní**: Pokud byste testovací převzetí služeb při selhání virtuálního počítače, pak můžete použít tento bod obnovení konkrétní tooa toofailover možnost.

    > [!NOTE]
    > Hello možnost toochoose bod obnovení je k dispozici, pouze pokud jsou selhání tooAzure.
    >
    >


1. Pokud některé hello virtuální počítače v plánu obnovení hello byly převzetí služeb při selhání při předchozím spuštění a nyní je na zdrojovém i cílovém umístění aktivní hello virtuálních počítačů, můžete použít **změnit směr** možnost toodecide hello směr v měly by proběhnout které převzetí hello.
1. Pokud jste selhání tooAzure a je povolené šifrování dat pro cloud hello (platí jenom v případě, že uživatelé chrání virtuální počítače Hyper-v ze serveru VMM) v **šifrovací klíč** hello vyberte certifikát, který byl vydán, kdy jste Povolit šifrování dat během instalace na serveru VMM hello.
1. Vyberte **vypnout počítač před zahájením převzetí služeb při selhání** Pokud chcete, aby Site Recovery tooattempt toodo hello vypnutí zdrojové virtuální počítače před spuštěním převzetí služeb při selhání. Převzetí služeb při selhání pokračovat i v případě, že vypnutí selže.  

    > [!NOTE]
    > V případě virtuálních počítačů Hyper-v tato možnost také pokusí toosynchronize hello místní data, která nebyla ještě nebyly odeslány toohello služby před spuštěním převzetí služeb při selhání hello.
    >
    >

1. Mohou sledovat průběh převzetí služeb při selhání hello na hello **úlohy** stránky. I v případě, že během neplánované převzetí služeb při selhání dojít k chybám, plán obnovení hello běží, dokud se nedokončí.
1. Po převzetí služeb při selhání hello ověřte tak, že přihlášení tooit hello virtuálního počítače. Pokud chcete, aby toogo jiný bod obnovení pro hello virtuální počítač, pak můžete použít **změnit bod obnovení** možnost.
1. Jakmile budete spokojeni s hello při selhání virtuálního počítače, můžete **potvrdit** hello převzetí služeb při selhání. To odstraní všechny hello body obnovení dostupné službou hello a **změnit bod obnovení** možnost nadále již nebudou dostupné.

## <a name="planned-failover"></a>Plánované převzetí služeb při selhání
Kromě převzetí služeb při selhání Hyper-V virtuální počítače chráněné pomocí Site Recovery také podpora **plánované převzetí služeb při selhání**. Jde o nulové možnost převzetí služeb při selhání data ztráty. Když se aktivuje plánované převzetí služeb při selhání, nejprve hello zdroje, které virtuální počítače jsou vypnuté, hello dat ještě toobe synchronizované se synchronizují a pak se aktivuje převzetí služeb při selhání.

> [!NOTE]
> Pokud jste převzetí služeb při selhání technologie Hyper-v virtuální počítače z jednoho místní lokality tooanother místní lokality, toocome back toohello primární místního serveru máte toofirst **zpětně replikovat** hello virtuální počítač zpět tooprimary lokality a potom aktivační události převzetí služeb při selhání. Pokud hello primární virtuální počítač není k dispozici, pak před spuštěním příliš**zpětně replikovat** máte toorestore hello virtuálního počítače ze zálohy.   
>
>

## <a name="failover-job"></a>Převzetí služeb při selhání úlohy

![Převzetí služeb při selhání](./media/site-recovery-failover/FailoverJob.png)

Když se aktivuje převzetí služeb při selhání, zahrnuje následující kroky:

1. Kontrola předpokladů: Tento krok zajistí, že jsou splněny všechny podmínky požadované pro převzetí služeb při selhání
1. Převzetí služeb při selhání: Tento krok zpracovává hello data a umožňuje připraven, aby virtuální počítač Azure můžete vytvořit mimo ho. Pokud jste vybrali **nejnovější** bodu obnovení, tento krok vytvoří bod obnovení z hello data, která byla odeslána toohello služby.
1. Začátek: Tento krok vytvoří virtuální počítač Azure pomocí zpracování v předchozím kroku hello dat hello.

> [!WARNING]
> **Nemáte zrušit v průběhu převzetí služeb při selhání**: před zahájením převzetí služeb při selhání replikace pro virtuální počítač hello je zastavena. Pokud jste **zrušit** v průběhu úlohy zastaví převzetí služeb při selhání, ale hello virtuální počítač nespustí tooreplicate. Replikace se nedá spustit znovu.
>
>

## <a name="time-taken-for-failover-tooazure"></a>Doba tooAzure převzetí služeb při selhání

Převzetí služeb při selhání virtuálních počítačů v určitých případech vyžaduje velmi přechodný krok, který obvykle trvá přibližně 8 toocomplete too10 minut. Těchto případech jsou jako následující:

* Virtuální počítače VMware pomocí služby mobility verze starší než 9.8
* Fyzické servery 
* Virtuální počítače s Linuxem VMware
* Technologie Hyper-V virtuální počítače chráněné jako fyzických serverů
* Virtuální počítače VMware, kde nejsou zadány jako spouštěcí ovladače následující ovladače 
    * miniport storvsc 
    * VMBus 
    * storflt 
    * Intelide 
    * ATAPI
* Virtuální počítače VMware, které nemají služba DHCP povolena bez ohledu na to, jestli jsou pomocí protokolu DHCP nebo statické IP adresy

V hello všech ostatních případech tento zprostředkující krok není povinný a je výrazně nižší hello doba hello převzetí služeb při selhání. 





## <a name="using-scripts-in-failover"></a>Pomocí skriptů v převzetí služeb při selhání
Můžete chtít tooautomate určité akce přitom převzetí služeb při selhání. Můžete použít skripty nebo [runbooky služby Azure automation](site-recovery-runbook-automation.md) v [plány obnovení](site-recovery-create-recovery-plans.md) toodo který.

## <a name="other-considerations"></a>Další důležité informace
* **Písmeno jednotky** – písmeno jednotky hello tooretain na virtuální počítače po převzetí služeb při selhání můžete nastavit hello **zásada SAN** hello virtuální počítač příliš**OnlineAll**. [Přečtěte si další informace](https://support.microsoft.com/en-us/help/3031135/how-to-preserve-the-drive-letter-for-protected-virtual-machines-that-are-failed-over-or-migrated-to-azure).



## <a name="next-steps"></a>Další kroky
Jakmile budete mít převzetí služeb při selhání virtuálního počítače a hello místního datového centra je k dispozici, měli byste [ **znovu nastavit ochranu** ](site-recovery-how-to-reprotect.md) zálohování virtuálních počítačů VMware toohello místního datového centra.

Použití [ **plánované převzetí služeb při selhání** ](site-recovery-failback-from-azure-to-hyper-v.md) možnost příliš**navrácení služeb po obnovení** virtuální počítače Hyper-v zpátky tooon místní z Azure.

Pokud se nezdařilo přes místní data tooanother virtuálního počítače technologie Hyper-v spravuje VMM server a hello primární datového centra center je k dispozici, pak použít **zpětná replikace** možnost toostart hello replikace zpět toohello primární datové centrum.
