---
title: "aaaPlan kapacity a škálování pro tooAzure replikace VMware s Azure Site Recovery | Microsoft Docs"
description: "Použijte tento článek tooplan kapacity a škálování při replikaci virtuálních počítačů VMware tooAzure s Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 0a1cd8eb-a8f7-4228-ab84-9449e0b2887b
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/27/2017
ms.author: rayne
ms.openlocfilehash: 551533ab7090d85c216be242ea92781deb8287ab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-3-plan-capacity-and-scaling-for-vmware-tooazure-replication"></a>Krok 3: Plánování kapacity a škálování pro replikaci tooAzure VMware

Použijte tento článek toofigure plánování kapacity a škálování při replikaci místní virtuální počítače VMware a fyzické servery tooAzure s [Azure Site Recovery](site-recovery-overview.md).

POST dotazy a na konci hello tohoto článku nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="how-do-i-start-capacity-planning"></a>Jak spustit plánování kapacity?

Shromážděte informace o prostředí replikace a pak plánování kapacity, na základě těchto informací, společně s hello aspekty zvýrazněných v tomto článku.


## <a name="gather-information"></a>Shromážděte informace

1. Stáhnout hello [nástroj pro nasazení Planner](https://aka.ms/asr-deployment-planner) pro replikaci VMware.
2. [Přečtěte si tento článek](site-recovery-deployment-planner.md) toounderstand jak toorun hello nástroj.
3. Pomocí nástroje hello shromažďovat informace o kompatibilní a nekompatibilní virtuálních počítačů, disků na virtuální počítač, a změn dat na disk. Nástroj Hello platí i pro nároky na šířku pásma sítě a hello infrastrukturu Azure, které jsou potřebné pro úspěšné replikace a testovací převzetí služeb při selhání.

## <a name="replication-considerations"></a>Aspekty replikace

Před zahájením nasazení v úvahu tyto skutečnosti:

**Komponenta** | **Podrobnosti** |
--- | --- | ---
**Replikace** | **Maximální denní míry změn:** chráněného počítače lze použít pouze jeden server proces a jeden procesový server může zpracovat denní míry změny až too2 TB. Proto je 2 TB hello maximální denní data změnit rychlost, kterou je podporována pro chráněný počítač.<br/><br/> **Maximální propustnost:** replikovaného počítače můžou patřit tooone účet úložiště v Azure. Standardní účet úložiště může zpracovat nesmí být delší než 20 000 požadavků za sekundu, a doporučujeme, abyste hello počet vstupně výstupních operací za sekundu (IOPS) napříč zdrojový počítač too20, 000. Například pokud je zdrojový počítač s 5 disky a 120 IOPS (8 kb velikost) na zdrojovém počítači hello generuje každého disku, pak bude v rámci hello Azure za maximální IOPS disku 500. (hello počet účtů úložiště vyžaduje je rovna toohello celkový zdrojový počítač IOPS, dělený 20 000.)

## <a name="configuration-server-capacity"></a>Konfigurace serveru kapacity

Hello konfigurační server by měl být schopný toohandle hello denní změnu míra kapacity napříč všechny úlohy spuštěné na chráněných počítačích a potřebuje dostatečná šířka pásma toocontinuously replikovat data tooAzure úložiště.

Jako osvědčený postup najít hello konfigurační server hello stejné síti a segment sítě LAN jako hello počítače chcete tooprotect. Může být umístěn na jinou síť, ale počítače budete chtít tooprotect by měl mít tooit viditelnost vrstvy 3 sítě.

## <a name="sizing-recommendations"></a>Změna velikosti doporučení

Hello tabulka shrnuje doporučení nastavení velikosti podle využití procesoru.

**VYUŽITÍ PROCESORU** | **Paměť** | **Velikost disku mezipaměti** | **Míry změny dat** | **Chráněné počítače**
--- | --- | --- | --- | ---
8 Vcpu (2 sockets * @ 2,5 GHz [GHz] 4 jádra) | 16 GB | 300 GB | 500 GB nebo méně | Replikovat počítače méně než 100.
12 Vcpu (2 sockets * @ 2,5 GHz 6 jader) | 18 GB | 600 GB | 500 GB too1 TB | Replikovat mezi 100 150 počítačů.
16 Vcpu (2 sockets * @ 2,5 GHz 8 jader) | 32 GB | 1 TB | 1 TB too2 TB | Replikovat mezi 150 až 200 počítačů.
Nasazení jiný procesový server | | | > 2 TB | Nasaďte další proces servery, pokud replikujete více než 200 počítačů, nebo pokud změny dat hello denní míra překročí 2 TB.

Kde:

* Každý zdrojový počítač je nakonfigurovaný s 3 disky 100 GB.
* Použili jsme testu typovou úlohou úložiště 8 disky SAS ot. / min, 10 kb s RAID 10 pro měření disku mezipaměti.

## <a name="process-server-capacity"></a>Proces serveru kapacity


Hello procesový server přijímá data replikace z chráněného počítače a optimalizuje je pomocí ukládání do mezipaměti, komprese a šifrování. Pak odešle hello data tooAzure.

- počítač serveru proces Hello by měl mít dostatek prostředků tooperform tyto úlohy.
- Hello první procesový server je nainstalována ve výchozím nastavení na konfiguračním serveru hello. Můžete nasadit další proces servery tooscale prostředí.
- procesový server Hello používá diskové mezipaměti. Použijte samostatné mezipaměti disk 600 GB nebo více změn dat toohandle uložené v události hello přetížení sítě nebo výpadek.
- Pokud potřebujete tooprotect více než 200 počítačů nebo denní míry změny hello je větší než 2 TB, můžete přidat proces servery toohandle hello replikace zatížení. tooscale out, můžete postupovat následovně:
    - Zvyšte počet hello konfigurační servery. Například můžete chránit až too400 počítačů se dvěma servery konfigurace.
    - Přidat další servery proces a použijte tyto toohandle provoz místo (nebo kromě) hello konfigurační server.


### <a name="example-process-server-scaling"></a>Příklad procesu serveru škálování

Hello následující tabulka popisuje scénář, ve kterém:

* Nemáte v plánu toouse hello konfigurační server jako procesní server.
* Nastavili jste si serveru další proces.
* Jste nakonfigurovali chráněné virtuální počítače toouse hello další procesový server.
* Každý počítač chráněného zdroje je nakonfigurovaný s tři disky 100 GB.

**Konfigurační server** | **Další procesového serveru** | **Velikost disku mezipaměti** | **Míry změny dat** | **Chráněné počítače**
--- | --- | --- | --- | ---
8 Vcpu (2 sockets * 4 jádra @ 2,5 GHz), 16 GB paměti | 4 Vcpu (2 sockets * 2 jádra @ 2,5 GHz), 8 GB paměti | 300 GB | 250 GB nebo méně | Replikovat počítače 85 nebo méně.
8 Vcpu (2 sockets * 4 jádra @ 2,5 GHz), 16 GB paměti | 8 Vcpu (2 sockets * 4 jádra @ 2,5 GHz), 12 GB paměti | 600 GB | TB too1 250 GB | Replikovat mezi 85 150 počítačů.
12 Vcpu (2 sockets * @ 2,5 GHz 6 jader), 18 GB paměti | 12 Vcpu (2 sockets * @ 2,5 GHz 6 jader) 24 GB paměti | 1 TB | 1 TB too2 TB | Replikovat mezi 150 225 počítačů.

způsob Hello, ve kterém můžete škálovat vaše servery závisí na vaši volbu pro model škálování nebo Škálováním na více systémů.  Vertikální navýšení kapacity nasazením několik vyšší kategorie konfigurace a proces servery nebo vertikální navýšení kapacity provádíte nasazení více serverů s méně prostředků. Například pokud budete potřebovat tooprotect 220 počítače, můžete to udělat buď hello následující:

* Nastavení konfigurace serveru hello s 12 virtuálních procesorů, 18 GB paměti a serveru další proces s 12 virtuálních procesorů, 24 GB paměti. Nakonfigurujte server další proces hello chráněné počítače toouse pouze.
* Nastavte dva servery configuration (2 x 8 virtuálních procesorů, 16 GB paměti RAM) a dva další proces servery (1 × 8 virtuálních procesorů a 4 virtuální procesory x 1 toohandle 135 + 85 [220] počítače). Konfigurace serverů další proces hello chráněné počítače toouse pouze.

## <a name="deploy-additional-process-servers"></a>Nasadit další proces servery

Postupujte podle těchto pokynů tooset nahoru Další procesový server. Po nastavení hello serveru, migrovat toouse počítače zdrojového ho.

1. V **servery Site Recovery**, klikněte na konfigurační server hello > **+ procesový Server**.
2. V **typ serveru**, klikněte na tlačítko **procesového serveru (místní)**.

    ![Procesový server](./media/vmware-walkthrough-capacity/migrate-ps2.png)
3. Stáhněte si soubor hello Unified instalace nástroje Site Recovery.
4. Spusťte instalační program tooinstall hello procesový server a zaregistrovat v trezoru hello.
5. V **před zahájením**, vyberte **přidat další proces servery tooscale se nasazení**.
6. V **podrobnosti o konfiguraci serveru**, zadejte IP adresu hello hello konfigurační server a hello přístupové heslo. Pokud nemáte heslo hello, získáte ho spuštěním **[SiteRecoveryInstallationFolder]\home\sysystems\bin\genpassphrase.exe – n** na hello konfiguračním serveru.

    ![Konfigurace serveru](./media/vmware-walkthrough-capacity/add-ps2.png)
7. Dokončení hello zbytek instalační program v hello stejně jako jste to udělali při nastavování hello konfigurační server.

### <a name="migrate-machines-toouse-hello-process-server"></a>Migrace počítačů toouse hello procesového serveru

1. V **nastavení** > **servery Site Recovery**, klikněte na konfigurační server hello > **zpracovat servery**.
2. Klikněte pravým tlačítkem na hello procesový server aktuálně používán > **přepínač**.

    ![Přepínač procesového serveru](./media/vmware-walkthrough-capacity/migrate-ps3.png)
3. V **vyberte cílový procesový server**vyberte hello procesový server chcete toouse a vyberte virtuální počítače hello zpracuje tento server hello.
4. Klikněte na ikonu informací o hello. toohelp provedete načíst rozhodnutí, hello průměrná místa, které je potřeba tooreplicate se zobrazí každý vybraný virtuální počítač toohello nový procesový server.
5. Klikněte na tlačítko hello zaškrtnutí toostart replikace toohello nový procesový server.

## <a name="control-network-bandwidth"></a>Řídí šířku pásma sítě

Po spuštění [nástroje nasazení Plánovač hello](site-recovery-deployment-planner.md) toocalculate hello šířku pásma potřebujete pro replikaci (hello počáteční replikace a pak rozdílové), můžete řídit objem hello šířky pásma pro replikaci pomocí několika možností:

* **Omezení šířky pásma**: VMware provoz, který replikuje tooAzure prochází konkrétní procesní server. Můžete omezit šířku pásma hello počítače, které běží jako servery procesu.
* **Ovlivnění šířky pásma**: můžete ovlivnit hello šířku pásma používanou pro replikaci pomocí několika klíčů registru:
  * Hello **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\UploadThreadsPerVM** hodnotu registru určuje hello počet vláken, které se používají pro přenos dat (počáteční nebo rozdílové replikace) disku. Vyšší hodnota zvětšuje hello šířku pásma sítě používané pro replikaci.
  * Hello **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\DownloadThreadsPerVM** určuje hello počet vláken používaných pro přenos dat během navrácení služeb po obnovení.

### <a name="throttle-bandwidth"></a>Omezení šířky pásma

1. Otevřete hello Azure Backup konzoly MMC modul snap-in na hello počítač fungoval jako server hello procesu. Ve výchozím nastavení, je k dispozici na ploše hello, nebo v následující složce hello zástupce pro zálohování: C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin.
2. V modulu snap-in hello, klikněte na **změnit vlastnosti**.
3. Na hello **omezování** vyberte **povolit využití šířky pásma Internetu u operací zálohování omezení**.
4. Nastavte hello limity pro pracovní a nepracovní dobu. Platné rozsahy jsou 512 kB/s too102 MB/s za sekundu.

    ![Omezení](./media/vmware-walkthrough-capacity/throttle2.png)

Můžete taky hello [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) rutiny tooset omezení. Tady je ukázkový skript:

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

**Set-OBMachineSetting -NoThrottle** označuje, že není požadováno žádné omezování.

### <a name="influence-network-bandwidth-for-a-vm"></a>Ovlivnění šířky pásma sítě pro virtuální počítač

1. V registru hello virtuálního počítače, přejděte příliš**HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**.
   * tooinfluence hello šířky pásma provoz na replikaci disku, upravte hodnotu hello **UploadThreadsPerVM**, nebo vytvořte klíč hello, pokud neexistuje.
   * tooinfluence hello šířky pásma pro přenosy navrácení služeb po obnovení z Azure, upravte hodnotu hello **DownloadThreadsPerVM**.
2. Hello výchozí hodnota je 4. V síti zřízený třeba upravit tyto klíče registru. maximální Hello je 32. Monitorování přenosů toooptimize hello hodnotu.




## <a name="next-steps"></a>Další kroky

Přejděte příliš[krok 4: plánování sítě](vmware-walkthrough-network.md).
