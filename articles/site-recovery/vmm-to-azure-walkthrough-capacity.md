---
title: "aaaPlan kapacity a škálování pro virtuální počítač Hyper-V tooAzure replikace (s VMM) s Azure Site Recovery | Microsoft Docs"
description: "Použijte tento článek tooplan kapacity a škálování při replikaci virtuálních počítačů Hyper-V v nástroji VMM cloudů tooAzure s Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 41c3c83e-6b1a-496a-8179-498c552ef0c7
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/23/2017
ms.author: raynew
ms.openlocfilehash: 9818ada9bb21f60ac00b3894696201b06630cb2b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-3-plan-capacity-and-scaling-for-hyper-v-with-vmm-tooazure-replication"></a>Krok 3: Plánování kapacity a škálování pro replikaci tooAzure technologie Hyper-V (s nástrojem VMM)

Po zkontrolování hello [požadavky nasazení](vmm-to-azure-walkthrough-prerequisites.md), použijte tento článek toofigure se plánování kapacity a škálování při replikaci místní virtuální počítače Hyper-V v cloudech tooAzure System Center Virtual Machine Manager (VMM), s [Azure Site Recovery](site-recovery-overview.md).

Po přečtení tohoto článku, post jakékoli komentáře v dolní části hello, nebo požádat technické dotazy na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="how-do-i-start-capacity-planning"></a>Jak spustit plánování kapacity?


Můžete shromáždit informace o prostředí replikace a pak plánování kapacity pomocí hello data, společně s hello aspekty zvýrazněných v tomto článku.


## <a name="gather-information"></a>Shromážděte informace

1. Získat informace o prostředí replikace, včetně virtuální počítačů, disků na virtuální počítač a úložišti na disk.
2. Určete vaše denní míry změn s ohledem pro replikovaná data. Stáhnout hello [nástroj plánování kapacity služby technologie Hyper-V](https://www.microsoft.com/download/details.aspx?id=39057) míru změn tooget hello. Doporučujeme, aby že tento nástroj spustíte přes toocapture týden průměry.
 

## <a name="figure-out-capacity"></a>Zjistěte, kapacity

Na základě informací hello jste shromáždění spustíte hello [nástroje Plánovač kapacity obnovení lokality](http://aka.ms/asr-capacity-planner-excel) tooanalyze vaše zdrojové prostředí a úlohy, odhadnout požadavky na šířku pásma a server pro umístění zdroje hello a hello prostředky (virtuální počítače a úložiště atd), které je třeba v cílovém umístění hello. Hello nástroj můžete spustit v několika způsoby:

- Rychlé plánování: Spusťte nástroj hello v tomto režimu tooget sítě a serveru projekce založené na průměrném počtu virtuálních počítačů, disků, úložiště a míru změn.
- Podrobné plánování: Spusťte nástroj hello v tomto režimu a uveďte podrobné informace o každé zatížení na úrovni virtuálního počítače. Analýza kompatibility virtuálních počítačů a získat projekce sítě a serveru.

Nyní spusťte nástroj hello:

1. Stáhnout hello [nástroj](http://aka.ms/asr-capacity-planner-excel)
2. toorun hello rychlé planner, postupujte podle [tyto pokyny](site-recovery-capacity-planner.md#run-the-quick-planner)a vyberte hello scénář **tooAzure technologie Hyper-V**.
3. toorun hello podrobné planner, postupujte podle [tyto pokyny](site-recovery-capacity-planner.md#run-the-detailed-planner)a vyberte hello scénář **technologie Hyper-V tooAzure**.

## <a name="control-network-bandwidth"></a>Řídí šířku pásma sítě

Poté, co jste počítané hello šířky pásma, které potřebujete, máte několik možností pro ovládání hello objemu šířky pásma používané pro replikaci:

* **Omezení šířky pásma**: přenos Hyper-V, která se replikují tooAzure prochází konkrétního hostitele technologie Hyper-V. Můžete omezit šířku pásma na hostitelském serveru hello.
* **Ovlivnění šířky pásma**: můžete ovlivnit hello pásma sítě používané pro replikaci pomocí několika klíčů registru.

### <a name="throttle-bandwidth"></a>Omezení šířky pásma
1. Otevřete hello Microsoft Azure Backup konzoly MMC modul snap-in na serveru hostitele technologie Hyper-V hello. Ve výchozím nastavení je k dispozici na ploše hello nebo v C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin zástupce služby Microsoft Azure Backup.
2. V modulu snap-in hello klikněte na **změnit vlastnosti**.
3. Na hello **omezování** vyberte **povolit využití šířky pásma Internetu u operací zálohování omezení**a nastavte hello limity pro pracovní a nepracovní dobu. Platné rozsahy jsou 512 kB/s too102 MB/s za sekundu.

    ![Omezení šířky pásma](./media/vmm-to-azure-walkthrough-capacity/throttle2.png)

Můžete taky hello [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) rutiny tooset omezení. Tady je ukázkový skript:

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

**Set-OBMachineSetting -NoThrottle** označuje, že není požadováno žádné omezování.

### <a name="influence-network-bandwidth"></a>Ovlivnění šířky pásma sítě
1. V registru hello přejděte příliš**HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**.
   * tooinfluence hello šířky pásma provoz na replikaci disku, upravte hodnotu hello hello **UploadThreadsPerVM**, nebo vytvořte klíč hello, pokud neexistuje.
   * tooinfluence hello šířky pásma pro přenosy navrácení služeb po obnovení z Azure, upravte hodnotu hello **DownloadThreadsPerVM**.
2. Hello výchozí hodnota je 4. V síti "nadměrným zřízením" tyto klíče registru by mělo být změněno z hello výchozí hodnoty. maximální Hello je 32. Monitorování přenosů toooptimize hello hodnotu.

## <a name="next-steps"></a>Další kroky

Přejděte příliš[krok 4: plánování sítě](vmm-to-azure-walkthrough-network.md).
