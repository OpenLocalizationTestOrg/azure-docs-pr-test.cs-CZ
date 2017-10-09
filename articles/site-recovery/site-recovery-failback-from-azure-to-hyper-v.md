---
title: "aaaFailback v Azure Site Recovery pro virtuální počítače Hyper-v | Microsoft Docs"
description: "Azure Site Recovery koordinuje hello replikace, převzetí služeb při selhání a obnovení virtuálních počítačů a fyzických serverů. Další informace o navrácení služeb po obnovení z Azure tooon lokálním datovém centru."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: gauravd
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 08/11/2017
ms.author: ruturajd
ms.openlocfilehash: 50cda9105de6b6fb23e4c62942fdaffc55c3efa4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="failback-in-site-recovery-for-hyper-v-virtual-machines"></a>Navrácení služeb po obnovení ve službě Site Recovery pro virtuální počítače Hyper-V

Tento článek popisuje, jak toofailback virtuální počítače chráněné službou Site Recovery.

## <a name="prerequisites"></a>Požadavky
1. Zkontrolujte, zda že je tento server hello primární lokality VMM server nebo Hyper-V připojen.
2. Měli jste provedli **potvrdit** hello virtuálního počítače.

## <a name="why-is-there-no-button-called-failback"></a>Proč je k dispozici žádné tlačítko názvem navrácení služeb po obnovení?
Na portálu hello neexistuje žádné explicitní gesto názvem navrácení služeb po obnovení. Navrácení služeb po obnovení je krok, kde jste vraťte toohello primární lokality. Podle definice převzetí služeb při selhání je, když jste převzetí služeb při selhání hello virtuální počítače z lokality toorecovery primary(on-premises) (Azure) a navrácení služeb po obnovení je při převzetí služeb při selhání hello virtuální počítače z obnovení zálohování tooprimary.

Při zahájení převzetí služeb při selhání hello okno informující o hello směr hello úlohy. Pokud je směr hello z Azure tooOn místní, je navrácení služeb po obnovení.

## <a name="why-is-there-only-a-planned-failover-gesture-toofailback"></a>Proč je pouze toofailback gesto plánované převzetí služeb při selhání?
Azure je prostředí s vysokou dostupností a virtuální počítače vždy budou k dispozici. Navrácení služeb po obnovení je plánované aktivity, kde se rozhodnete tootake malé výpadek tak, aby hello úlohy můžete spustit znovu spustit místní. Toto předpokládá, že nedošlo ke ztrátě dat. Proto je k dispozici pouze gesto plánované převzetí služeb při selhání, který bude vypnout hello virtuálních počítačů v Azure, stáhněte nejnovější změny hello a ujistěte se, že nedošlo ke ztrátě dat.

## <a name="initiate-failback"></a>Zahájit navrácení služeb po obnovení
Po převzetí služeb při selhání hello primární toosecondary umístění replikované virtuální počítače nejsou chráněny službou Site Recovery a sekundární umístění hello teď funguje jako umístění služby active hello. Postupujte podle těchto postupů toofail back toohello původní primární lokality. Tento postup popisuje, jak toorun plánované převzetí služeb při selhání pro obnovení plánu. Případně můžete spustit hello převzetí služeb při selhání pro jeden virtuální počítač na hello **virtuální počítače** kartě.

1. Vyberte **plány obnovení** > *recoveryplan_name*. Klikněte na tlačítko **převzetí služeb při selhání** > **plánované převzetí služeb při selhání**.
2. Na hello ** potvrďte plánované převzetí služeb při selhání ** vyberte hello zdrojové a cílové umístění. Všimněte si hello směr převzetí služeb při selhání. Pokud hello převzetí služeb při selhání z primárního fungovala jako očekávat a všechny virtuální počítače jsou v hello sekundárního umístění, které toto je pouze pro informaci.
3. Pokud po obnovení zpět z Azure vyberte nastavení v **synchronizace dat**:

   * **Synchronizace dat před převzetí služeb při selhání (pouze synchronizovat rozdílové změny)**– tato možnost minimalizuje prostoje pro virtuální počítače jako synchronizuje bez jejich vypínání. Dobrý den, následující:
     * Fáze 1: Trvá snímek hello virtuálního počítače v Azure a zkopíruje jej toohello hostitele Hyper-V na místě. počítač Hello dál, běží v Azure.
     * Fáze 2: Vypne hello virtuálního počítače v Azure tak, aby žádné nové změny dojít k dispozici. závěrečné sady Hello rozdílové změny jsou přenášená toohello na místním serveru a hello na místním virtuálním počítači spuštění.

    - **Synchronizace dat během pouze převzetí služeb při selhání (úplná ke stažení)**– tuto možnost použijte, pokud jste jste už běží v Azure po dlouhou dobu. Tato možnost je rychlejší, protože Očekáváme, že došlo ke změně většinu hello disku a Neradi bychom toospend čas výpočtu kontrolního součtu. Provede stahování hello disku. Je také užitečné při hello místní virtuální počítač byl odstraněn.

    >[!NOTE]
    >Doporučujeme tuto možnost použijte, pokud jste byla spuštěna Azure nějakou dobu (v měsíci nebo více) nebo hello místní virtuální počítač je odstraněný. Tato možnost nebude provádět výpočty kontrolního součtu.
    >
    >




4. Pokud je povolené šifrování dat pro hello cloud v **šifrovací klíč** hello vyberte certifikát, který byl vydán, pokud povolíte šifrování dat během instalace zprostředkovatele na serveru VMM hello.
5. Zahájit převzetí služeb při selhání hello. Mohou sledovat průběh převzetí služeb při selhání hello na hello **úlohy** kartě.
6. Pokud jste vybrali hello možnost toosynchronize hello data před hello převzetí služeb při selhání, jednou hello počáteční po dokončení synchronizace dat a vy budete připravené tooshut dolů hello virtuálních počítačů v Azure, klikněte na tlačítko **úlohy** název úlohy plánované převzetí služeb při selhání **Dokončení převzetí služeb při selhání**. To vypne hello počítač Azure, přenosy hello nejnovější změny toohello místní virtuální počítač a spustí hello virtuálních počítačů na místě.
7. Teď můžete protokolovat do toovalidate hello virtuálního počítače je k dispozici podle očekávání.
8. Hello virtuální počítač je ve stavu čekání na potvrzení. Klikněte na tlačítko **potvrdit** toocommit hello převzetí služeb při selhání.
9. Teď postupně klikněte na tlačítko toocomplete hello navrácení služeb po obnovení **zpětnou replikaci** toostart ochranu hello virtuální počítač v primární lokalitě hello.

## <a name="failback-tooan-alternate-location"></a>Navrácení služeb po obnovení tooan alternativního umístění
Pokud jste nasadili ochrany mezi [web Hyper-V a Azure](site-recovery-hyper-v-site-to-azure.md) máte tooability toofailback z Azure tooan alternativní místní umístění. To je užitečné, pokud potřebujete tooset si nové místní hardware. Zde je postup ho.

1. Pokud instalujete nový hardware nainstalujte Windows Server 2012 R2 a roli Hyper-V na serveru hello hello.
2. Vytvořte virtuální síťový přepínač s hello stejný název, že jste měli na původní server hello.
3. Vyberte **chráněné položky** -> **skupiny ochrany**  ->  <ProtectionGroupName>  ->  <VirtualMachineName> chcete toofail zpět a vyberte **plánovaná Převzetí služeb při selhání**.
4. V **potvrďte plánované převzetí služeb při selhání** vyberte **vytvořit místní virtuální počítač Pokud neexistuje**.
5. V **název hostitele** vyberte hello nový server hostitele technologie Hyper-V, na kterém chcete tooplace hello virtuálního počítače.
6. Synchronizace dat doporučujeme vybrat možnost hello **synchronizovat data hello před převzetí služeb při selhání hello**. To minimalizuje prostoje pro virtuální počítače jako synchronizuje bez jejich vypínání. Dobrý den, následující:

   * Fáze 1: Trvá snímek hello virtuálního počítače v Azure a zkopíruje jej toohello hostitele Hyper-V na místě. počítač Hello dál, běží v Azure.
   * Fáze 2: Vypne hello virtuálního počítače v Azure tak, aby žádné nové změny dojít k dispozici. Hello poslední sadu změn jsou přenášená toohello na místním serveru a hello na místním virtuálním počítači spuštění.
7. Klikněte na tlačítko hello zaškrtnutí toobegin hello převzetí služeb při selhání (navrácení služeb po obnovení).
8. Po dokončení počáteční synchronizace hello a vy budete připravené tooshut dolů hello virtuálního počítače v Azure, klikněte na tlačítko **úlohy** > <planned failover job> > **dokončení převzetí služeb při selhání**. To ukončí před hello počítač Azure, přenosy hello nejnovější změny toohello místní virtuální počítač a spustí ho.
9. Můžete se přihlásit tooverify hello místní virtuální počítač, který všechno funguje podle očekávání. Pak klikněte na tlačítko **potvrdit** toofinish hello převzetí služeb při selhání.
10. Klikněte na tlačítko **zpětnou replikaci** toostart ochranu hello místní virtuální počítač.

    > [!NOTE]
    > Pokud zrušíte hello navrácení služeb po obnovení úlohy, i když je v kroku synchronizace dat, hello místní virtuální počítač bude v poškozeném stavu. Je to proto, že synchronizace dat zkopíruje hello nejnovější data z virtuálního počítače Azure disky toohello místní datové disky, a až do dokončení synchronizace hello hello diskových dat nemusí být v konzistentním stavu. Pokud hello místní virtuální počítač se spustí po synchronizaci dat se zruší, nemusí spustit. Znovu spustíte převzetí služeb při selhání toocomplete hello synchronizace dat.
    >
    >



## <a name="next-steps"></a>Další kroky

Po dokončení navrácení služeb po obnovení úlohy hello **potvrdit** hello virtuálního počítače. Potvrzení odstraní hello virtuální počítač Azure a jeho disky a připraví toobe hello virtuálního počítače chráněný znovu.

Po **potvrdit**, můžete zahájit hello *zpětnou replikaci*. Tím se spustí ochrana hello virtuálního počítače z back tooAzure místní. Všimněte si to bude pouze replikují hello změny, protože hello virtuální počítač byl vypnut v Azure a odešle rozdílové změny pouze.
