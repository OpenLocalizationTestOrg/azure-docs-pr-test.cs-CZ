---
title: "Replikace virtuálních počítačů VMware a fyzické servery do Azure na portálu classic | Microsoft Docs"
description: "Tento článek popisuje, jak nasadit Azure Site Recovery k orchestraci replikace, převzetí služeb při selhání a obnovení na lokální virtuální počítače VMware a fyzické servery Windows nebo Linuxem do Azure."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: a9022c1f-43c1-4d38-841f-52540025fb46
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: raynew
ROBOTS: NOINDEX, NOFOLLOW
redirect_url: site-recovery-vmware-to-azure
ms.openlocfilehash: 73c3fb5cf4056ddb9554f598ec7f173d81802f17
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="replicate-vmware-virtual-machines-and-physical-servers-to-azure-with-azure-site-recovery"></a>Replikace virtuálních počítačů VMware a fyzické servery do Azure s Azure Site Recovery
> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-vmware-to-azure.md)
> * [Portál classic](site-recovery-vmware-to-azure-classic.md)
> * [Portálu classic (zastaralé)](site-recovery-vmware-to-azure-classic-legacy.md)
>
>

Služba Azure Site Recovery přispívá ke strategii obchodní kontinuitu a po havárii obnovení (BCDR) tím, že orchestruje replikaci, převzetí služeb při selhání a obnovení virtuálních počítačů a fyzických serverů. Počítače je možné replikovat do Azure nebo do sekundárního místního datového centra. Rychlý přehled, najdete v tématu [co je Azure Site Recovery?](site-recovery-overview.md).

## <a name="overview"></a>Přehled
Tento článek popisuje, jak:

* **Replikace virtuálních počítačů VMware do Azure**: nasazení Site Recovery pro koordinaci replikace, převzetí služeb při selhání a obnovení virtuálních počítačů VMware místně do úložiště Azure.
* **Replikace fyzických serverů do Azure**: nasazení Azure Site Recovery pro koordinaci replikace, převzetí služeb při selhání a obnovení místní fyzických Windows a Linux serverů do Azure.

> [!NOTE]
> Tento článek popisuje, jak replikovat do Azure. Pokud chcete replikovat virtuální počítače VMware nebo fyzických serverů Windows nebo Linuxem do sekundárního datacentra, přečtěte si téma [lokality obnovení z VMware do VMware](site-recovery-vmware-to-vmware.md).
>
>

POST jakékoli dotazy nebo připomínky v dolní části tohoto článku nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="enhanced-deployment"></a>Rozšířeného nasazení
Tento článek obsahuje pokyny pro rozšířeného nasazení na portálu Azure classic. Doporučujeme použít tuto verzi pro všechna nová nasazení. Pokud jste už nasazená pomocí starší verze starší verze, doporučujeme migraci na novou verzi. Další informace o migraci najdete v tématu [lokality obnovení z VMware do Azure classic starší](site-recovery-vmware-to-azure-classic-legacy.md#migrate-to-the-enhanced-deployment).

Rozšířeného nasazení je hlavní aktualizace. Zde je souhrn, které jsme provedli jsme vylepšení:

* **Bez infrastruktury virtuálních počítačů v Azure**: Data se replikují přímo k účtu úložiště Azure. Kromě toho není třeba nastavit jakékoliv infrastruktury virtuálních počítačů (např. konfigurační server nebo hlavní cílový server) pro replikaci a převzetí služeb při selhání v případě potřeby se v nasazení starší verze.  
* **Unified instalace**: jedna instalace poskytuje jednoduché nastavení a škálovatelnost pro místní komponenty.
* **Zabezpečit nasazení**: veškerý provoz je zašifrován a komunikaci správu replikace se odesílají přes protokol HTTPS 443.
* **Body obnovení**: podpora pro selhání a obnovení konzistentních s aplikací body pro Windows a Linux prostředí a podpora pro obě jedna konfigurace virtuálních počítačů a více virtuálních počítačů konzistentní.
* **Testovací převzetí služeb při selhání**: podpora pro omezovaly testovací převzetí služeb při selhání do Azure bez dopadu na produkční prostředí nebo pozastavení replikace.
* **Neplánované převzetí služeb při selhání**: podpora pro neplánované převzetí služeb při selhání do Azure rozšířené možnost automaticky vypnout virtuální počítače před převzetí služeb při selhání.
* **Navrácení služeb po obnovení**: integrované navrácení služeb po obnovení, která se replikují jenom rozdílové změny zpět do místního webu.
* **vSphere 6.0**: omezenou podporu pro nasazení VMware vSphere 6.0.

## <a name="how-does-site-recovery-help-protect-virtual-machines-and-physical-servers"></a>Jak Site Recovery pomáhá chránit virtuální počítače a fyzické servery?
* VMware správci mohou nakonfigurovat odlehlého bezpečnostní opatření k ochraně Azure z firemní úlohy a aplikace běžící na virtuálních počítačů VMware. Správce serveru můžete replikovat fyzické místních serverů, Windows a Linux do Azure.
* Konzole Azure Site Recovery poskytuje jedno umístění pro jednoduché nastavení a správu replikace, převzetí služeb při selhání a obnovení procesy.
* Pokud budete replikovat virtuální počítače VMware, které jsou spravovány nástrojem vCenter server, Site Recovery může zjišťovat těchto virtuálních počítačů. Pokud počítače na hostiteli ESXi, Site Recovery vyhledá virtuální počítače na hostiteli.
* Pokud spustíte snadné převzetí služeb při selhání z vaší místní infrastruktury do Azure, může selhat zpět (obnovení) z Azure do virtuálního počítače VMware serverech v lokalitě na místě.
* Můžete nakonfigurovat plány obnovení, které skupiny úloh aplikace, které jsou vrstvené napříč více počítačů. Pokud jste převzetí služeb při selhání tyto plány, Site Recovery poskytuje konzistence více virtuálních počítačů tak, aby počítače, které běží stejné zatížení je možné obnovit společně do konzistentní datového bodu.

## <a name="supported-operating-systems"></a>Podporované operační systémy
### <a name="windows-64-bit-only"></a>Windows (pouze 64bitová verze)
* Windows Server 2008 R2 SP1 +
* Windows Server 2012
* Windows Server 2012 R2

### <a name="linux-64-bit-only"></a>Linux (pouze 64bitová verze)
* Red Hat Enterprise Linux 7.2, 6.7 a 7.1
* CentOS verze 6.5, 6.6, 6.7, 7.0, 7.1 a 7.2
* Oracle Enterprise Linux 6.4 a 6.5 systémem Red Hat kompatibilní jádra nebo nedělitelné Enterprise jádra verze 3 (UEK3)
* SUSE Linux Enterprise Server 11 SP3

## <a name="scenario-architecture"></a>Architektura scénáře
Součásti scénáře:

* **Serveru pro správu místní**: součásti Site Recovery běží server pro správu:
  * **Konfigurační server**: koordinuje komunikaci a spravuje procesy data replikace a obnovení.
  * **Procesový server**: funguje jako replikační brána. Obdrží data z chráněných zdrojových počítačů; optimalizuje je pomocí ukládání do mezipaměti, komprese a šifrování; a odesílá data replikace do úložiště Azure. Také obstará nabízenou instalaci služby Mobility na chráněné počítače a provádí automatického zjišťování virtuálních počítačů VMware.
  * **Hlavní cílový server**: Zpracovává replikační data během navracení služeb z Azure po obnovení.
    Můžete také nasadit server pro správu, který slouží pouze jako procesní server škálování vašeho nasazení.
* **Služba Mobility**: Tato součást je nasazen na každý počítač (virtuálního počítače VMware nebo fyzických serverů), který chcete replikovat do Azure. Se zaznamenává datové zápisy na počítači a předává je na procesní server.
* **Azure**: nemusíte vytvářet žádné virtuální počítače Azure pro zpracování replikace a převzetí služeb při selhání. Služba Site Recovery zpracovává správu dat a data se replikují přímo do úložiště Azure. Replikované virtuální počítače Azure se spouštějí automaticky jenom v případě, že dojde k převzetí služeb při selhání do Azure. Ale pokud chcete, aby v případě selhání zpět z Azure na místní lokalitu, musíte nastavit virtuální počítač Azure tak, aby fungoval jako procesní server.

Na následujícím obrázku (vytvořený Henry Robalino) znázorňuje interakci tyto komponenty:

![Architektura součástí](./media/site-recovery-vmware-to-azure/v2a-architecture-henry.png)

## <a name="capacity-planning"></a>Plánování kapacity
Při plánování kapacity, zde je potřeba se zamyslet:

* **Zdrojové prostředí**: plánování kapacity nebo požadavky VMware infrastruktury a zdrojového počítače.
* **Server pro správu**: plánování pro místní správu serverů se součástmi Site Recovery.
* **Šířka pásma sítě ze zdrojového do cílového**: plánování šířky pásma sítě vyžadovaná pro replikaci mezi zdrojovým a Azure.

### <a name="source-environment-considerations"></a>Aspekty zdrojové prostředí
* **Maximální denní míry změny**: chráněného počítače lze použít pouze jeden procesový server. Jeden procesový server může zpracovat až 2 TB změny dat za den. 2 TB tedy, že maximální denních dat změnit rychlost, kterou je podporována pro chráněný počítač.
* **Maximální propustnost**: replikovaného počítače můžou patřit do jeden účet úložiště v Azure. Standardní účet úložiště může zpracovat nesmí být delší než 20 000 požadavků za sekundu, a doporučujeme, aby byl počet procesorů mezi zdrojového počítače na 20 000. Například pokud je zdrojový počítač s 5 disků a každý disk generuje 120 IOPS (8 KB velikost) ve zdroji, bude v rámci Azure za maximální IOPS disku 500. Počet účtů úložiště vyžaduje = source celkový počet IOPs/20 000.

### <a name="management-server-considerations"></a>Aspekty správy serveru
Server pro správu spustí součásti Site Recovery, které zpracovávají data optimalizace, replikaci a správu. Musí být schopna zpracovávat kapacitu denní míra změn mezi všechny úlohy spuštěné na chráněných počítačích a má dostatečnou šířku pásma pro nepřetržitě replikaci dat do úložiště Azure. Zejména:

* Procesový server přijímá data replikace z chráněného počítače a optimalizuje je pomocí ukládání do mezipaměti, komprese a šifrování před odesláním do Azure. Server pro správu by měla mít dostatek prostředků k provedení těchto úloh.
* Procesový server používá diskové mezipaměti. Doporučujeme disk samostatné mezipaměti 600 GB nebo více zpracovat změny dat, které jsou uložené v případě přetížení sítě nebo výpadek. Během nasazení můžete nakonfigurovat mezipaměť na všechny jednotky, který má minimálně 5 GB dostupného úložiště, ale 600 GB je minimální doporučení.
* Jako osvědčený postup doporučujeme, že server pro správu je umístěný ve stejné síti a segment sítě LAN jako počítače, které chcete chránit. Může být umístěn v jiné síti, ale počítače, které chcete chránit, musí mít L3 viditelnost sítě do ní.

Velikost doporučení pro server pro správu jsou shrnuty v následující tabulce:

| **Server pro správu procesoru** | **Paměť** | **Velikost disku mezipaměti** | **Míry změny dat** | **Chráněné počítače** |
| --- | --- | --- | --- | --- |
| 8 Vcpu (2 sockets * @ 2,5 GHz 4 jádra) |16 GB |300 GB |500 GB nebo méně |Nasazení serveru pro správu pomocí těchto nastavení se replikovat počítače méně než 100. |
| 12 Vcpu (2 sockets * @ 2,5 GHz 6 jader) |18 GB |600 GB |500 GB až 1 TB |Nasazení serveru pro správu pomocí těchto nastavení se replikovat počítače 100 150. |
| 16 Vcpu (2 sockets * @ 2,5 GHz 8 jader) |32 GB |1 TB |1 TB 2 TB |Nasazení serveru pro správu pomocí těchto nastavení se replikovat počítače 150 až 200. |
| Nasazení jiný procesový server | | |> 2 TB |Nasaďte další proces servery, pokud replikujete více než 200 počítačů, nebo pokud se změní denních dat míra překročí 2 TB. |

Kde:

* Každý zdrojový počítač je nakonfigurovaný s 3 disky 100 GB.
* Použili jsme testu typovou úlohou úložiště 8 jednotky SAS 10 000 ot. / min s RAID 10 pro měření disku mezipaměti.

### <a name="network-bandwidth-from-source-to-target"></a>Šířka pásma sítě ze zdrojové do cílové
Zajistěte, aby vypočítat šířku pásma, která by byla zapotřebí pro počáteční replikaci a rozdílová replikace pomocí [nástroje Plánovač kapacity](site-recovery-capacity-planner.md).

#### <a name="throttling-bandwidth-used-for-replication"></a>Omezení šířky pásma používané pro replikaci
Provoz VMware replikovat do Azure projde konkrétní procesní server. Můžete omezit šířku pásma, která je k dispozici pro replikaci Site Recovery na tomto serveru následujícím způsobem:

1. Otevřete modulu snap-in Microsoft Azure Backup konzoly MMC na serveru pro správu hlavní nebo na serveru pro správu spuštěna další zajištěna se proces servery. Ve výchozím nastavení je zástupce služby Microsoft Azure Backup vytvoří na ploše. Nebo ji můžete najít v C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin.
2. V modulu snap-in klikněte na **Změnit vlastnosti**.

    ![Omezení šířky pásma změnit vlastnosti](./media/site-recovery-vmware-to-azure-classic/throttle1.png)
3. Na **omezování** zadejte šířky pásma, která lze použít pro replikaci Site Recovery a použít plánování.

    ![Omezení šířky pásma replikace](./media/site-recovery-vmware-to-azure-classic/throttle2.png)

Můžete také nastavit omezení pomocí prostředí PowerShell. Tady je příklad:

    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth (512*1024) -NonWorkHourBandwidth (2048*1024)

#### <a name="maximizing-bandwidth-usage"></a>Tím se maximalizuje využití šířky pásma
Chcete-li zvýšit šířky pásma pro replikaci využívaných Azure Site Recovery, změňte klíč registru.

Následující klíč řídí počet vláken na replikaci disku, které se používají při replikaci:

    HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication\UploadThreadsPerVM

 Tento klíč registru v nadměrným zřízením síti, je potřeba změnit z jeho výchozí hodnoty. Podporujeme nesmí být delší než 32.  

Další informace o plánování kapacity podrobné najdete v tématu [Plánovač kapacity Site Recovery](site-recovery-capacity-planner.md).

### <a name="additional-process-servers"></a>Další proces servery
Pokud je potřeba chránit více než 200 počítačů nebo vaše denní míra změn je větší, 2 TB, můžete přidat další servery pro zpracování zátěže. Chcete-li škálovat, můžete:

* Zvyšte počet serverů pro správu. Například můžete chránit až 400 počítačů se dvěma servery pro správu.
* Přidat další proces servery a použít pro zpracování provozu mezi místo (nebo kromě) serveru pro správu.

Tato tabulka popisuje scénář, ve kterém:

* Chcete použít jako konfigurační server pouze nastavíte původního serveru pro správu.
* Nastavení serveru další proces.
* Nakonfigurujete chráněné virtuální počítače používat další procesový server.
* Každý počítač chráněného zdroje je nakonfigurovaný s tři disky 100 GB.

| **Původní server pro správu**<br/><br/>(konfigurační server) | **Další procesového serveru** | **Velikost disku mezipaměti** | **Míry změny dat** | **Chráněné počítače** |
| --- | --- | --- | --- | --- |
| 8 Vcpu (2 sockets * 4 jádra @ 2,5 GHz), 16 GB paměti RAM |4 Vcpu (2 sockets * 2 jádra @ 2,5 GHz), 8 GB paměti RAM |300 GB |250 GB nebo méně |Můžete replikovat počítače 85 nebo méně. |
| 8 Vcpu (2 sockets * 4 jádra @ 2,5 GHz), 16 GB paměti RAM |8 Vcpu (2 sockets * 4 jádra @ 2,5 GHz), je 12 GB paměti RAM |600 GB |250 GB až 1 TB |Můžete provádět replikaci 85 150 počítačů. |
| 12 Vcpu (2 sockets * @ 2,5 GHz 6 jader), 18 GB paměti RAM |12 Vcpu (2 sockets * @ 2,5 GHz 6 jader) 24 GB paměti RAM |1 TB |1 TB 2 TB |150 225 počítače můžete replikovat. |

Jak škálovat vaše servery závisí na vaši volbu pro model škálování nebo Škálováním na více systémů. Vertikální navýšení kapacity nasazením několik vyšší kategorie správy a proces servery nebo vertikální navýšení kapacity provádíte nasazení více serverů s méně prostředků. Například: Pokud potřebujete k ochraně 220 počítačů, můžete provést jeden z následujících akcí:

* Nakonfigurujte původního serveru pro správu s 12 Vcpu a 18 GB paměti RAM. Konfigurace serveru další proces s 12 Vcpu a 24 GB paměti RAM. Nakonfigurujte chráněné počítače používat pouze další procesový server.
* Nakonfigurujte dva servery pro správu (2 x 8 Vcpu, 16 GB paměti RAM) a dva další proces servery (1 × 8 Vcpu a 4vCPUs x 1 pro zpracování 135 + 85 (220) počítače). Konfiguraci chráněných počítačů používat servery pouze z další proces.

Postupujte podle pokynů v [nasadit další proces servery](#deploy-additional-process-servers) do nastavení další procesu serveru.

## <a name="before-you-start-deployment"></a>Před zahájením nasazení
Následující tabulka představuje souhrn předpoklady pro tento scénář nasazovat.

### <a name="azure-prerequisites"></a>Požadavky Azure
| **Požadavek** | **Podrobnosti** |
| --- | --- |
| **Účet Azure** |Potřebujete účet [Microsoft Azure](https://azure.microsoft.com/). Můžete začít s [bezplatnou zkušební verzí](https://azure.microsoft.com/pricing/free-trial/). Další informace o cenách za Site Recovery najdete v tématu [Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/). |
| **Úložiště Azure** |K ukládání replikovaných dat potřebujete účet úložiště Azure. Replikovaná data jsou uložena v úložišti Azure a virtuálních počítačích Azure se spouštějí v případě převzetí služeb při selhání. <br/><br/>Potřebujete [účet se standardním geograficky redundantním úložištěm](../storage/storage-redundancy.md#geo-redundant-storage). Účet musí být ve stejné oblasti jako služba Site Recovery a musí být přidružený ke stejnému předplatnému. Replikace na účty úložiště premium není aktuálně podporována a by se neměla používat.<br/><br/>Nepodporujeme přesunutí účty úložiště vytvořené pomocí [portál Azure](../storage/storage-create-storage-account.md) mezi skupinami prostředků. Další informace najdete v tématu [Úvod do Microsoft Azure Storage](../storage/storage-introduction.md).<br/><br/> |
| **Síť Azure** |Budete potřebovat virtuální síť Azure, virtuální počítače Azure připojí po převzetí služeb při selhání. Virtuální síť Azure musí být ve stejné oblasti jako trezor Site Recovery.<br/><br/>K selhání zpět po převzetí služeb při selhání do Azure, budete potřebovat síť VPN pro připojení (nebo Azure ExpressRoute) nastavit ze sítě Azure k místní lokalitě. |

### <a name="on-premises-prerequisites"></a>Místní požadavky
| **Požadavek** | **Podrobnosti** |
| --- | --- |
| **Server pro správu** |Je nutné místnímu serveru Windows 2012 R2 spuštěné na virtuálním počítači nebo fyzickém serveru. Na tomto serveru pro správu jsou nainstalovány všechny součásti Site Recovery na místě.<br/><br/> Doporučujeme že nasadit server jako vysoce dostupný virtuální počítač VMware. Navrácení služeb po obnovení do místní lokality z Azure je vždy virtuální počítače VMware bez ohledu na to, jestli můžete převzít služby při selhání virtuálních počítačů nebo fyzických serverů. Pokud nenakonfigurujete serveru pro správu jako virtuální počítače VMware, budete muset nastavit samostatnou hlavní cílový server jako virtuální počítač VMware přijímat provoz navrácení služeb po obnovení.<br/><br/>Server by neměl být řadič domény.<br/><br/>Server by měl mít statickou IP adresu.<br/><br/>Název hostitele serveru musí být maximálně 15 znaků nebo méně.<br/><br/>Národní prostředí operačního systému musí být pouze angličtinu.<br/><br/>Server pro správu vyžaduje přístup k Internetu.<br/><br/>Odchozí přístup ze serveru musíte následujícím způsobem: dočasný přístup na HTTP 80 během instalace součásti Site Recovery (ke stažení MySQL); probíhající odchozí přístup na protokol HTTPS 443 pro správu replikace; probíhající odchozí přístup na HTTPS 9443 pro provoz replikace (Tento port lze upravit).<br/><br/> Zajistěte, aby že tyto adresy URL jsou přístupné ze serveru pro správu: <br/>- \*.hypervrecoverymanager.windowsazure.com<br/>- \*.accesscontrol.windows.net<br/>- \*.backup.windowsazure.com<br/>- \*.blob.core.windows.net<br/>- \*.store.core.windows.net<br/>-https://www.msftncsi.com/ncsi.txt<br/>- [https://dev.MySQL.com/Get/Archives/MySQL-5.5/MySQL-5.5.37-Win32.msi](https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi " https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi")<br/><br/>Pokud máte pravidla brány firewall založená na adresu IP na serveru, zkontrolujte, jestli tato pravidla umožňují komunikaci s Azure. Musíte povolit [rozsahy IP adres datacentra Azure](https://www.microsoft.com/download/details.aspx?id=41653) a port HTTPS (443). Budete také muset povolených rozsahů IP adres pro oblast Azure svého předplatného a západní USA. Adresa URL [https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi](https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi " https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi") ke stahování MySQL. |
| **Hostitel VMware vCenter/ESXi** |Budete potřebovat jeden nebo více VMware vSphere ESX/ESXi hypervisory Správa virtuálních počítačů VMware ESX/ESXi verze 6.0, 5.5 nebo 5.1 s nejnovější aktualizace.<br/><br/> Doporučujeme nasadit server VMware vCenter pro správu hostitele ESXi. Měla by být spuštěna s vCenter verze 6.0 nebo 5.5 s nejnovějšími aktualizacemi.<br/><br/>Všimněte si, že Site Recovery nepodporuje nové vCenter a vSphere 6.0 funkce, jako křížové řešení vMotion vCenter, virtuální svazky a úložiště DRS. Podpora pro obnovení lokality je omezena na funkce, které byly k dispozici ve verzi 5.5. |
| **Chráněné počítače** |**Azure**<br/><br/>Počítače, které chcete chránit musí být v souladu s [požadavky Azure](site-recovery-prereq.md) pro vytváření virtuálních počítačů Azure.<br><br/>Pokud se chcete připojit k virtuálním počítačům Azure po převzetí služeb při selhání, budete muset povolit připojení ke vzdálené ploše v místní bráně firewall.<br/><br/>Kapacita jednotlivých disků na chráněných počítačích by neměla být větší než 1 023 GB. Virtuální počítač může mít až 64 disků (tedy až 64 TB). Pokud máte disky, které jsou větší než 1 TB, zvažte použití replikace databáze, například SQL serveru Always On nebo Oracle Data Guard.<br/><br/>Minimální 2 GB volného místa na disku instalace pro instalaci součásti.<br/><br/>Sdílené hostované clustery disků nejsou podporované. Pokud máte skupinové nasazení, zvažte použití replikace databáze, například SQL serveru Always On nebo Oracle Data Guard.<br/><br/>Unified Extensible Firmware Interface (UEFI) / není podporované spouštění Extensible Firmware Interface.<br/><br/>Názvy počítačů musí obsahovat 1 až 63 znaků (písmena, číslice a pomlčky). Název musí začínat písmenem nebo číslicí a končit písmenem nebo číslicí. Jakmile je počítač je chráněný, můžete upravit název Azure.<br/><br/>**Virtuální počítače VMware**<br/><br>Budete muset nainstalovat VMware vSphere PowerCLI 6.0. na serveru pro správu (konfigurační server).<br/><br/>Virtuální počítače VMware, který chcete chránit, musí mít nástroje VMware nainstalovaná a spuštěná.<br/><br/>Pokud virtuální počítač má zdroj seskupování síťových adaptérů, jsou převedeny na jednu síťovou kartu po převzetí služeb při selhání do Azure.<br/><br/>Pokud iSCSI disk chráněných virtuálních počítačů, Site Recovery převede chráněný disk iSCSI virtuálního počítače na soubor virtuálního pevného disku při virtuálního počítače při selhání do Azure. Pokud cíle iSCSI, dosažitelný z virtuálního počítače Azure, budou se připojit k cíli iSCSI a v podstatě najdete v části dva disky: disku VHD na virtuálním počítači Azure a zdrojový disk iSCSI. V takovém případě je nutné odpojit cíle iSCSI, který se zobrazí na virtuální počítač Azure při selhání.<br/><br/>Další informace o oprávnění uživatele VMware potřebuje obnovení lokality, najdete v části [VMware oprávnění pro přístup k systému vCenter](#vmware-permissions-for-vcenter-access).<br/><br/> **Počítače Windows serveru (u virtuálního počítače VMware nebo fyzický server)**<br/><br/>Na serveru musí běžet podporovaný 64bitový operační systém: Windows Server 2012 R2, Windows Server 2012 nebo Windows Server 2008 R2 s v minimálně SP1.<br/><br/>Operační systém by měly být nainstalovány na jednotce C a disk operačního systému musí být základní disk systému Windows. (Operačního systému by se neměly instalovat na dynamický disk systému Windows.)<br/><br/>Pro počítače, Windows Server 2008 R2 musíte mít rozhraní .NET Framework 3.5.1 nainstalována.<br/><br/>Musíte zadat účet správce (musí být místní správce na počítači systému Windows) na nabízenou instalaci služby Mobility na serverech s Windows. Pokud uvedený účet je jiný doménový účet, budete muset zakázat řízení vzdáleného přístupu uživatele v místním počítači. Další informace najdete v tématu [instalaci služby mobility nabízenou instalaci](#install-the-mobility-service-with-push-installation).<br/><br/>Site Recovery podporuje virtuální počítače s diskem RDM. Během navrácení služeb po obnovení bude obnovení lokality používat RDM disk, pokud jsou k dispozici původního zdrojového virtuálního počítače a RDM disk. Pokud nejsou k dispozici, během navrácení služeb po obnovení, Site Recovery vytvoří nový soubor VMDK pro každý disk.<br/><br/>**Počítače se systémem Linux**<br/><br/>Je třeba podporovaný 64bitový operační systém: Red Hat Enterprise Linux 6.7; Centos verze 6.5, 6.6 nebo 6.7; Oracle Enterprise Linux 6.4 nebo 6.5 systémem Red Hat kompatibilní jádra nebo nedělitelné Enterprise jádra verze 3 (UEK3); SUSE Linux Enterprise Server 11 SP3.<br/><br/>soubory/etc/hosts na chráněných počítačích by měly obsahovat položky, které mapování názvu místního hostitele na IP adresy přidružené všechny síťové adaptéry. <br/><br/>Pokud se chcete připojit k virtuální počítač Azure s Linuxem po převzetí služeb při selhání pomocí klienta Secure Shell (ssh), ujistěte se, že služba Secure Shell v chráněném počítači je nastavena na spustit automaticky při spuštění systému, a že povolit pravidla brány firewall ssh připojení k němu.<br/><br/>Ochrany lze povolit pouze pro počítače se systémem Linux s následující úložiště: souboru systému (EXT3, ETX4, ReiserFS, XFS); Funkce Multipath softwaru zařízení Mapper (multipath); Správce svazků (LVM2). Fyzické servery s HP CCISS řadič úložiště nejsou podporovány. Systém souborů ReiserFS je podporována pouze na SUSE Linux Enterprise Server 11 SP3.<br/><br/>Site Recovery podporuje virtuální počítače s diskem RDM. Během navrácení služeb po obnovení pro Linux Site Recovery nemá opakovaně RDM disk. Místo toho vytvoří nový soubor VMDK pro každý odpovídající RDM disk. |

Pouze pro virtuální počítač s Linuxem: Zkontrolujte, že nastavená disk.enableUUID=true nastavení v parametrech konfigurace virtuálního počítače v prostředí VMware. Pokud tento řádek neexistuje, přidejte ji. To je nutné zajistit konzistentní UUID k VMDK tak, aby ji připojí správně. Bez tohoto nastavení způsobí navrácení služeb po obnovení úplné stažení i v případě, že virtuální počítač je k dispozici místní. Přidání toto nastavení zajistí, že jsou přenášeny jenom rozdílové změny zpět během navrácení služeb po obnovení.

## <a name="step-1-create-a-vault"></a>Krok 1: Vytvoření trezoru
1. Přihlaste se k webu [Azure Portal](https://manage.windowsazure.com/).
2. Rozbalte položku **datové služby** > **služeb zotavení** a klikněte na tlačítko **trezor Site Recovery**.
3. Klikněte na **Vytvořit nový** > **Rychlé vytvoření**.
4. Jako **Název** zadejte popisný název pro identifikaci trezoru.
5. V rozevírací nabídce **Oblast** vyberte zeměpisnou oblast trezoru. Pokud chcete zkontrolovat oblasti jsou podporované, najdete v části [podrobnosti o cenách Azure Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/).
6. Klikněte na **Vytvořit trezor**.
    ![Vytvoření trezoru](./media/site-recovery-vmware-to-azure-classic/quick-start-create-vault.png)

Podle informací na stavovém řádku si ověřte, že se trezor úspěšně vytvořil. Trezor bude uvedený jako **Active** v hlavním **služeb zotavení** stránky.

## <a name="step-2-set-up-an-azure-network"></a>Krok 2: Nastavení sítě Azure
Nastavte síť Azure, aby virtuální počítače Azure se připojí k síti po převzetí služeb při selhání, a tak, aby navrácení služeb po obnovení k místní lokalitě může fungovat podle očekávání.

1. Na portálu Azure vyberte **vytvořit virtuální síť** a zadejte název sítě, rozsah IP adres a název podsítě.
2. Pokud potřebujete provést navrácení služeb po obnovení, přidejte k síti VPN nebo ExpressRoute. Sítě VPN nebo ExpressRoute můžete přidat k síti i po převzetí služeb při selhání.

Další informace o sítě Azure najdete v tématu [Přehled virtuálních sítí](../virtual-network/virtual-networks-overview.md).

> [!NOTE]
> [Migrace sítí](../azure-resource-manager/resource-group-move-resources.md) napříč skupinami prostředků v rámci stejného předplatného nebo napříč předplatnými se pro sítě použité k nasazení Site Recovery nepodporuje.
>
>

## <a name="step-3-install-the-vmware-components"></a>Krok 3: Instalace komponenty VMware
Pokud chcete replikovat virtuální počítače VMware, proveďte tyto kroky na serveru pro správu:

1. [Stáhněte si](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1) a nainstalujte VMware vSphere PowerCLI 6.0.
2. Restartujte server.

## <a name="step-4-download-a-vault-registration-key"></a>Krok 4: Stáhněte si registrační klíč trezoru
1. Ze serveru pro správu otevřete konzolu Site Recovery v Azure. Na **služeb zotavení** klikněte na trezor otevřete **rychlý Start** stránky. Můžete také otevřít **rychlý Start** kdykoli kliknutím na ikonu.

    ![Ikona rychlý Start](./media/site-recovery-vmware-to-azure-classic/quick-start-icon.png)
2. Na **rychlý Start** klikněte na tlačítko **Příprava cílové prostředky** > **stáhněte si registrační klíč**. Soubor registračního je generován automaticky. Je platná pět dní po jeho vygenerování.

## <a name="step-5-install-the-management-server"></a>Krok 5: Instalace serveru pro správu
> [!TIP]
> Zajistěte, aby že tyto adresy URL jsou přístupné ze serveru pro správu:
>
> * *.hypervrecoverymanager.windowsazure.com
> * *.accesscontrol.windows.net
> * *.backup.windowsazure.com
> * *.blob.core.windows.net
> * *.store.core.windows.net
> * https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi
> * https://www.msftncsi.com/ncsi.txt
>
>



>[!VIDEO https://channel9.msdn.com/Blogs/Azure/Enhanced-VMware-to-Azure-Setup-Registration/player]



1. Na **rychlý Start** stránky, stáhněte sjednocený instalační soubor na server.
2. Spusťte instalační soubor spusťte instalační program **Unified instalace nástroje Site Recovery** průvodce.
3. Na stránce **Než začnete** vyberte **Nainstalovat konfigurační server a procesový server**.

   ![Než začnete](./media/site-recovery-vmware-to-azure-classic/combined-wiz1.png)
4. V **licence k softwaru třetích stran**, klikněte na tlačítko **souhlasím** ke stažení a instalaci MySQL.

    ![Software jiných výrobců](./media/site-recovery-vmware-to-azure-classic/combined-wiz105.PNG)
5. V **registrace**, vyhledejte a vyberte registrační klíč, který jste stáhli z trezoru.

    ![Registrace](./media/site-recovery-vmware-to-azure-classic/combined-wiz3.png)
6. V **nastavení Internetu**, zadejte, jak se zprostředkovatel, který běží na konfiguračním serveru připojit k Azure Site Recovery přes Internet.

   * Pokud chcete nastavit připojení s proxy serverem, který je aktuálně nastavený na počítači, vyberte **Připojit se s existujícím nastavením proxy serveru**.
   * Pokud chcete, aby zprostředkovatel připojil přímo, vyberte **připojit přímo bez proxy**.
   * Pokud stávající proxy server vyžaduje ověřování, nebo pokud chcete používat vlastní proxy server pro připojení poskytovatele, vyberte **Connect s vlastním nastavením proxy**.
     * Pokud používáte vlastní proxy server, budete muset zadat adresu, port a přihlašovací údaje.
     * Pokud používáte proxy server, které by už mít povolené následující adresy URL:
       * *.hypervrecoverymanager.windowsazure.com    
       * *.accesscontrol.windows.net
       * *.backup.windowsazure.com
       * *.blob.core.windows.net
       * *.store.core.windows.net

    ![Brána firewall](./media/site-recovery-vmware-to-azure-classic/combined-wiz4.png)

1. V **Kontrola předpokladů**, instalační program spustí kontrolu, abyste měli jistotu, že můžete spustit instalaci.

    ![Požadavky](./media/site-recovery-vmware-to-azure-classic/combined-wiz5.png)

     Pokud se zobrazí varování u položky **Kontrola synchronizace globálního času**, ověřte, že čas na systémových hodinách (nastavení **Datum a čas**) je stejný jako časové pásmo.

     ![Čas synchronizace problém](./media/site-recovery-vmware-to-azure-classic/time-sync-issue.png)

1. V **MySQL konfigurace**, vytvořit přihlašovací údaje pro přihlášení k instanci serveru MySQL, který bude nainstalován.

    ![MySQL](./media/site-recovery-vmware-to-azure-classic/combined-wiz6.png)
2. Na stránce **Podrobnosti o prostředí** vyberte, zda se chystáte replikovat virtuální počítače VMware. Pokud jste, instalační program kontroluje, že je nainstalována PowerCLI 6.0.

    ![MySQL](./media/site-recovery-vmware-to-azure-classic/combined-wiz7.png)
3. Na stránce **Umístění instalace** vyberte, kam chcete nainstalovat binární soubory a ukládat mezipaměť. Můžete vybrat jednotku, která má minimálně 5 GB volného místa na disku, ale doporučujeme jednotky mezipaměti s alespoň 600 GB volného místa na disku.

   ![Umístění instalace](./media/site-recovery-vmware-to-azure-classic/combined-wiz8.png)
4. V **výběr sítě**, zadejte naslouchací proces (síťový adaptér a SSL port) na kterém je konfigurační server bude odesílat a přijímat data replikace. Můžete upravit výchozí port (9443). Kromě tohoto portu se použije webový server, které orchestrují operací replikace port 443. Nepoužívejte 443 pro příjem přenosů replikace.

    ![Výběr sítě](./media/site-recovery-vmware-to-azure-classic/combined-wiz9.png)



1. Na stránce **Souhrn** zkontrolujte informace a klikněte na **Nainstalovat**. Po dokončení instalace se vygeneruje heslo. To budete potřebovat při povolení replikace, tak ho zkopírujte a uložte ho na bezpečném místě.

   ![Souhrn](./media/site-recovery-vmware-to-azure-classic/combined-wiz10.png)


> [!WARNING]
> Musíte nastavit proxy agenta služeb zotavení Microsoft Azure.
> Po dokončení instalace spusťte prostředí služby Microsoft Azure obnovení z nabídky Start systému Windows. V příkazovém okně, které se otevře spusťte následující sadu příkazů pro nastavení nastavení proxy serveru.
>
>
    $pwd = ConvertTo-SecureString -String ProxyUserPassword
    Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumb – ProxyUserName domain\username -ProxyPassword $pwd
    net stop obengine
    net start obengine
>


### <a name="run-setup-from-the-command-line"></a>Spusťte instalační program z příkazového řádku
Také spuštěním Průvodce jednotná z příkazového řádku, následujícím způsobem:

    UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP address to be used for data transfer] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>]

Kde:

* /ServerMode: Povinné. Určuje, jestli by měl proces a konfigurace serverů nebo na procesní server instalovat instalace (používá se k instalaci dalších proces servery). Vstupní hodnoty: CS, PS.
* InstallDrive: povinné. Určuje složku, ve kterém jsou nainstalované komponenty.
* / MySQLCredFilePath. Povinné. Určuje cestu k souboru, kde jsou uložené přihlašovací údaje serveru MySQL. Získá šablonu, kterou chcete vytvořit soubor.
* / VaultCredFilePath. Povinné. Umístění souboru s přihlašovacími údaji.
* /EnvType: Povinné. Typ instalace. Hodnoty: VMware, NonVMware.
* /PSIP a /CSIP: Povinné. IP adresa procesovým serverem a konfigurační server.
* /PassphraseFilePath: Povinné. Umístění souboru přístupové heslo.
* / ByPassProxy. Volitelné. Určuje server pro správu, která se připojuje k Azure bez serveru proxy.
* /ProxySettingsFilePath: Volitelné. Určuje nastavení pro vlastní proxy server (výchozí proxy server na serveru, který vyžaduje ověření) nebo vlastní proxy server.

## <a name="step-6-set-up-credentials-for-the-vcenter-server"></a>Krok 6: Nastavení pověření pro vCenter server
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Enhanced-VMware-to-Azure-Discovery/player]
>
>

Procesový server může automaticky zjistit virtuální počítače VMware, která se spravují přes vCenter server. Site Recovery pro automatické zjišťování, potřebuje účet a přihlašovací údaje, které můžete přístup k serveru vCenter. Tato akce není relevantní, pokud replikujete jenom fyzických serverů.

Nastavení účtu a přihlašovací údaje:

1. Na serveru vCenter server umožňuje vytvořit roli (**Azure_Site_Recovery**) na úrovni vCenter s [požadovaná oprávnění](#vmware-permissions-for-vcenter-access).
2. Přiřazení **Azure_Site_Recovery** role pro uživatele vCenter.

   > [!NOTE]
   > VCenter uživatelský účet, který má roli jen pro čtení můžete spustit převzetí služeb při selhání, aniž by předtím vypnul chráněných zdrojových počítačů. Pokud chcete vypnout tyto počítače, musíte roli Azure_Site_Recovery. Pokud máte pouze migrace virtuálních počítačů z VMware do Azure a nemusíte navrácení služeb po obnovení, stačí roli jen pro čtení.
   >
   >
3. Chcete-li přidat účet, otevřete **cspsconfigtool**. Je k dispozici jako zástupce na ploše a umístěné ve složce \home\svsystems\bin [umístění INSTALOVAT].
4. Na kartě **Správa účtů** klikněte na **Přidat účet**.

    ![Přidat účet](./media/site-recovery-vmware-to-azure-classic/credentials1.png)
5. V **Podrobnosti účtu**, přidat přihlašovací údaje, které lze použít pro přístup k serveru vCenter. Může trvat déle než 15 minut pro název účtu se objeví na portálu. Chcete-li aktualizovat okamžitě, klikněte na tlačítko **aktualizovat** na **konfigurační servery** kartě.

    ![Podrobnosti](./media/site-recovery-vmware-to-azure-classic/credentials2.png)

## <a name="step-7-add-vcenter-servers-and-esxi-hosts"></a>Krok 7: Přidejte servery vCenter server a hostitelů ESXi
Pokud replikujete virtuální počítače VMware, budete muset přidat vCenter server (nebo hostiteli ESXi).

1. Na **servery** > **konfigurační servery** vyberte **přidat server vCenter**.

    ![vCenter](./media/site-recovery-vmware-to-azure-classic/add-vcenter1.png)
2. Přidejte vCenter server nebo podrobnosti o hostiteli ESXi, název účtu, který jste zadali pro přístup k serveru vCenter server v předchozím kroku a procesového serveru, který se použije ke zjišťování virtuální počítače VMware, která se spravují přes vCenter server. VCenter server nebo hostiteli ESXi musí nacházet ve stejné síti jako server, na kterém je nainstalován na procesní server.

   > [!NOTE]
   > Pokud přidáváte vCenter server nebo hostiteli ESXi pomocí účtu, který nemá oprávnění správce na serveru vCenter nebo hostitele, ujistěte se, vCenter nebo účty ESXi mít tato oprávnění povoleno: Datacenter, úložiště, složku, Jost, sítě a prostředků, Virtuální počítač a vSphere distribuované přepínače. VCenter server potřebuje oprávnění k zobrazení úložiště povolené.
   >
   >

    ![Přidat vCenter server](./media/site-recovery-vmware-to-azure-classic/add-vcenter2.png)
3. Po dokončení zjišťování serveru vCenter bude uvedený na **konfigurační servery** kartě.

    ![vCenter](./media/site-recovery-vmware-to-azure-classic/add-vcenter3.png)

## <a name="step-8-create-a-protection-group"></a>Krok 8: Vytvoření skupiny ochrany
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Enhanced-VMware-to-Azure-Protection/player]
>
>

Skupiny ochrany jsou logickými seskupeními virtuálních počítačů nebo fyzických serverů, které chcete chránit pomocí stejné nastavení ochrany. Použití nastavení ochrany pro skupinu ochrany a tato nastavení se použijí na všechny počítače (virtuální nebo fyzická) přidáte do skupiny.

1. Přejděte na **chráněné položky** > **skupiny ochrany** a klikněte na ikonu Přidat skupinu ochrany.

    ![Vytvořit skupinu ochrany](./media/site-recovery-vmware-to-azure-classic/protection-groups1.png)
2. Na **zadejte nastavení skupiny ochrany** stránky, zadejte název pro skupinu. V **z** rozevíracího seznamu vyberte konfigurační server, na kterém chcete vytvořit skupinu. **Cíl** je Microsoft Azure.

    ![Nastavení skupiny ochrany](./media/site-recovery-vmware-to-azure-classic/protection-groups2.png)
3. Na **zadejte nastavení replikace** nakonfigurujte nastavení replikace, která se použije pro všechny počítače ve skupině.

    ![Skupiny ochrany replikace](./media/site-recovery-vmware-to-azure-classic/protection-groups3.png)

   * **Konzistence pro víc Virtuálních**: Pokud tuto možnost zapnout, se vytváří body obnovení sdílené konzistentní s aplikací mezi počítači ve skupině ochrany. Toto nastavení je nejdůležitější, když všechny počítače ve skupině ochrany běží stejné zatížení. Všechny počítače se obnoví do stejného datového bodu. Toto je k dispozici, jestli replikujete virtuální počítače VMware nebo fyzických serverů (Windows nebo Linux).
   * **Prahová hodnota RPO**: Nastaví plánovaný bod obnovení. Výstrahy jsou generovány, pokud replikace průběžné ochrany dat překročí nakonfigurovanou prahovou hodnotu RPO.
   * **Uchování bodu obnovení**: Určuje okno uchování. Chráněné počítače můžete obnovit do libovolného bodu v rámci tohoto okna.
   * **Frekvence snímkování konzistentní aplikace vzhledem**: Určuje, jak často budou vytvořeny body obnovení, obsahující snímky konzistentní s aplikacemi.

Když zaškrtnete políčko, skupina ochrany vytvořena s názvem, který jste zadali. Kromě toho se vytvoří druhé skupiny ochrany s názvem *název skupiny ochrany*- navrácení služeb po obnovení. Tuto skupinu ochrany se používá, pokud selže zpět do místního serveru po převzetí služeb při selhání do Azure. Skupiny ochrany můžete sledovat, jak jste vytvořili na **chráněné položky** stránky.

## <a name="step-9-install-the-mobility-service"></a>Krok 9: Instalace služby Mobility
Prvním krokem při aktivaci ochrany virtuálních počítačů a fyzických serverů je instalace služby Mobility. Můžete provést dvěma způsoby:

* Automaticky push a nainstalujte službu na každém počítači z procesového serveru. Při přidání počítačů do skupiny ochrany a již používáte příslušnou verzi služby Mobility, nedojde, nabízenou instalaci. Můžete také automaticky nainstalovat službu pomocí metodu nabízené enterprise, jako je WSUS nebo System Center Configuration Manager. Ujistěte se, že jste nastavili server pro správu tím.
* Nainstalujte službu ručně na každý počítač, který chcete chránit. Ujistěte se, že jste nastavili server pro správu tím.

### <a name="install-the-mobility-service-with-push-installation"></a>Nainstalujte službu Mobility nabízenou instalaci
Při přidání počítačů do skupiny ochrany, služba Mobility automaticky instaluje a nainstalovaný na každém počítači procesní server.

#### <a name="prepare-for-automatic-push-on-windows-machines"></a>Příprava pro automatické nabízené oznámení na počítačích s Windows
Příprava počítače s Windows, aby služba Mobility můžete automaticky nainstalovat procesní server:

1. Vytvořte účet, který procesový server můžete použít pro přístup k počítači. Účet by měl mít oprávnění správce (místní nebo doménový). Tyto přihlašovací údaje se používají pouze pro nabízenou instalaci služby Mobility.

   > [!NOTE]
   > Pokud nepoužíváte účet domény, budete muset zakázat řízení vzdáleného přístupu uživatele v místním počítači. Chcete-li to provést, v registru pod HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System, přidejte položku LocalAccountTokenFilterPolicy DWORD s hodnotou 1 v části. Pokud chcete přidat položku registru z příkazu Otevřít rozhraní příkazového řádku nebo pomocí prostředí PowerShell, zadejte  **`REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`** .
   >
   >
2. Pro bránu Windows Firewall na počítači, který chcete chránit, vyberte **povolit aplikace nebo funkci průchod bránou Firewall**a povolte **sdílení souborů a tiskáren** a **Správa systému Windows Instrumentace**. Pro počítače, které patří k doméně můžete nakonfigurovat zásady brány firewall pomocí objektu zásad skupiny.

   ![Nastavení brány firewall](./media/site-recovery-vmware-to-azure-classic/mobility1.png)
3. Přidejte účet, který jste vytvořili:

   * Otevřete nástroj **cspsconfigtool**. Je k dispozici jako zástupce na ploše a umístěné ve složce \home\svsystems\bin [umístění INSTALOVAT].
   * Na kartě **Správa účtů** klikněte na **Přidat účet**.
   * Přidejte účet, který jste vytvořili. Po přidání účtu, budete muset zadat přihlašovací údaje, když přidat počítač do skupiny ochrany.

#### <a name="prepare-for-automatic-push-on-linux-servers"></a>Příprava pro automatické nabízené oznámení na serverech s Linuxem
1. Ujistěte se, že počítač Linux, které chcete chránit, je podporován jak je popsáno v [místní požadavky](#on-premises-prerequisites). Zkontrolujte, zda je síťové připojení mezi počítači, na kterém chcete chránit a serveru pro správu, který běží na procesní server.
2. Vytvořte účet, který procesový server můžete použít pro přístup k počítači. Účet by měl být kořenový uživatel na zdrojovém serveru Linux. Tyto přihlašovací údaje se používají pouze pro nabízenou instalaci služby Mobility.

   * Otevřete nástroj **cspsconfigtool**. Je k dispozici jako zástupce na ploše a umístěné ve složce \home\svsystems\bin [umístění INSTALOVAT].
   * Na kartě **Správa účtů** klikněte na **Přidat účet**.
   * Přidejte účet, který jste vytvořili. Po přidání účtu, budete muset zadat přihlašovací údaje, když přidat počítač do skupiny ochrany.
3. Zkontrolujte, že soubor /etc/hosts na zdrojovém serveru s Linuxem obsahuje položky, které mapují místní název hostitele na IP adresy přidružené ke všem síťovým adaptérům.
4. Instalace nejnovější openssh, openssh serveru a openssl balíčky na počítači, který chcete chránit.
5. Ujistěte se, že SSH je povolené a spuštěné na port 22.
6. V souboru sshd_config povolte následujícím způsobem podsystém SFTP a ověřování heslem:

   * Přihlaste se jako kořenový adresář.
   * V souboru /etc/ssh/sshd_config najdete řádek, který začíná PasswordAuthentication.
   * Zrušte na řádku komentář a změňte hodnotu z **no** na **yes**.
   * Vyhledejte řádek, který začíná na **Subsystem** a zrušte na něm komentář.

     ![Linux přepsat výchozí žádné subsystémů](./media/site-recovery-vmware-to-azure-classic/mobility2.png)

### <a name="install-the-mobility-service-manually"></a>Nainstalujte službu Mobility ručně
Instalační programy jsou k dispozici v C:\Program Files (x86) \Microsoft Azure Site Recovery\home\svsystems\pushinstallsvc\repository.

| Zdrojový operační systém | Instalační soubor služby Mobility |
| --- | --- |
| Windows Server (pouze 64bitová verze) |Microsoft-ASR_UA_9*.0.0_Windows_* release.exe |
| CentOS 6.4, 6.5, 6.6 (pouze 64bitové verze) |Microsoft-ASR_UA_9*.0.0_RHEL6-64_*release.tar.gz |
| SUSE Linux Enterprise Server 11 SP3 (pouze 64bitová verze) |Microsoft-ASR_UA_9*.0.0_SLES11-SP3-64_*release.tar.gz |
| Oracle Enterprise Linux 6.4, 6.5 (pouze 64bitová verze) |Microsoft-ASR_UA_9*.0.0_OL6-64_*release.tar.gz |

#### <a name="install-the-mobility-service-manually-on-a-windows-server"></a>Nainstalujte službu Mobility ručně v systému Windows server
1. Stáhněte a spusťte odpovídající instalační program.
2. V části **Než začnete** vyberte **Služba Mobility**.

    ![Instalace služby mobility](./media/site-recovery-vmware-to-azure-classic/mobility3.png)
3. V **podrobnosti o konfiguraci serveru**, zadejte IP adresu serveru pro správu a heslo, které byl vygenerován při instalaci součásti serveru správy. Přístupové heslo můžete načíst tak, že spustíte  **<SiteRecoveryInstallationFolder>\home\sysystems\bin\genpassphrase.exe – n** na serveru pro správu.

    ![Služba Mobility](./media/site-recovery-vmware-to-azure-classic/mobility6.png)
4. V **umístění instalovat**, zachovat výchozí umístění a klikněte na **Další** zahájit instalaci.
5. V **průběh instalace**, zkontrolujte instalaci a pokud se zobrazí výzva, restartujte počítač.

Můžete taky nainstalovat tak, že zadáte následující text příkazového řádku:

    UnifiedAgent.exe [/Role <Agent/MasterTarget>] [/InstallLocation <Installation Directory>] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>] [/LogFilePath <Log File Path>]

V předchozím příkazu:

* / Role: povinné. Určuje, jestli se má nainstalovat služba Mobility.
* / InstallLocation: povinné. Určuje, kam se má služba nainstalovat.
* / PassphraseFilePath: povinné. Určuje heslo konfiguračního serveru.
* / LogFilePath: povinné. Určuje umístění souboru protokolu instalačního programu.

#### <a name="uninstall-the-mobility-service-manually"></a>Ruční odinstalace služby Mobility
Služba Mobility můžete odinstalovat pomocí **odinstalovat nebo změnit program** v Ovládacích panelech nebo pomocí příkazového řádku.

Příkaz pro odinstalaci služby Mobility pomocí příkazového řádku je:

    MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1}

#### <a name="change-the-ip-address-of-the-management-server"></a>Změna IP adresy serveru pro správu
Po spuštění průvodce můžete změnit IP adresu serveru pro správu následujícím způsobem:

1. Otevřete hostconfig.exe souboru (nachází se na ploše).
2. Na **globální** změňte IP adresu serveru pro správu.

   > [!NOTE]
   > Změňte IP adresu serveru pro správu. Číslo portu pro komunikaci se serverem pro správu musí být 443, a **použití HTTPS** musí být vlevo povolena. Neměňte heslo.
   >
   >

    ![IP adresa serveru pro správu](./media/site-recovery-vmware-to-azure-classic/host-config.png)

#### <a name="install-the-mobility-service-manually-on-a-linux-server"></a>Nainstalujte službu Mobility ručně na Linux server
1. Zkopírujte příslušné vkládání archivu počítač Linux, který chcete chránit. Najdete v tabulce v části [nainstalovat službu Mobility ručně](#install-the-mobility-service-manually) k určení které vkládání archivaci, můžete využít.
2. Otevřete program prostředí a extrahujte komprimované vkládání archivu do místní cestu spuštěním:`tar -xvzf Microsoft-ASR_UA_8.5.0.0*`
3. Vytvořte soubor s názvem passphrase.txt v místním adresáři, do které jste extrahovali obsah vkládání archivu. K tomu, zkopírujte heslo z C:\ProgramData\Microsoft Azure lokality Recovery\private\connection.passphrase na serveru pro správu a uložit ho passphrase.txt spuštěním  *`echo <passphrase> >passphrase.txt`*  v prostředí.
4. Nainstalujte službu Mobility tak, že zadáte následující příkaz:*`sudo ./install -t both -a host -R Agent -d /usr/local/ASR -i <IP address> -p <port> -s y -c https -P passphrase.txt`*
5. Zadejte interní IP adresu serveru pro správu a ujistěte se, že je vybrán port 443.

#### <a name="install-the-mobility-service-from-the-command-line"></a>Nainstalujte službu Mobility z příkazového řádku

Kopírovat heslo z C:\Program Files (x86) \InMage Systems\private\connection na serveru pro správu a uložte ho jako "passphrase.txt" na serveru pro správu. Spusťte následující příkazy. V našem příkladu IP adresu serveru správy je 104.40.75.37 a HTTPS port je 443:

Instalace na provozním serveru:

    ./install -t both -a host -R Agent -d /usr/local/ASR -i 104.40.75.37 -p 443 -s y -c https -P passphrase.txt

Instalace na hlavním cílovém serveru:

    ./install -t both -a host -R MasterTarget -d /usr/local/ASR -i 104.40.75.37 -p 443 -s y -c https -P passphrase.txt


## <a name="step-10-enable-protection-for-a-machine"></a>Krok 10: Povolení ochrany pro počítač
Pokud chcete povolit ochranu, přidejte do skupiny ochrany virtuálních počítačů a fyzických serverů. Než začnete, pamatujte na následující Pokud chráníte virtuální počítače VMware:

* Virtuální počítače VMware jsou zjištěny každých 15 minut, a to může trvat déle než 15 minut se objevily na portálu Site Recovery po zjišťování.
* Změny prostředí na virtuálním počítači (jako je třeba instalace nástroje VMware) může trvat déle než 15 minut k aktualizaci v Site Recovery.
* Čas poslední zjištěné můžete zkontrolovat pro virtuální počítače VMware v **poslední obraťte se na** pole pro hostitele systému vCenter server/ESXi na **konfigurační servery** kartě.
* Pokud po vytvoření skupiny ochrany přidáte vCenter Server nebo hostiteli ESXi, může trvat déle než 15 minut pro portálu Azure Site Recovery k aktualizaci a virtuálních počítačů uvedené **přidat počítače do skupiny ochrany** Dialogové okno.
* Pokud chcete okamžitě pokračovat a přidat počítače do skupiny ochrany, aniž by čekal na naplánovaného zjišťování, zvýrazněte konfigurační server (nemáte klikněte na něj) a klikněte na tlačítko **aktualizovat**.

Navíc platí:

* Doporučujeme, abyste architektury skupin ochrany, aby odpovídaly vašich úloh. Například přidejte počítače, které spustit konkrétní aplikace do stejné skupiny.
* Při přidání počítačů do skupiny ochrany na procesní server automaticky nabízených oznámení a nainstaluje službu Mobility, pokud již není nainstalována. Budete muset mít mechanismus nabízené připravené, jak je popsáno v předchozím kroku.

### <a name="add-machines-to-a-protection-group"></a>Přidání počítačů do skupiny ochrany

1. Přejděte na **chráněné položky** > **skupiny ochrany** > **počítače** > **přidat počítače**.
2. Pokud chráníte virtuální počítače VMware v **vyberte virtuální počítače**, vyberte server vCenter, který spravuje virtuální počítače (nebo EXSi hostitele, na kterém používáte systém) a potom vyberte počítače.

    ![Povolit ochranu pro virtuální počítače](./media/site-recovery-vmware-to-azure-classic/enable-protection2.png)
3. Pokud chráníte fyzické servery, v **vyberte virtuální počítače**, otevřete **přidání fyzických počítačů** průvodce a zadejte popisný název a IP adresu. Potom vyberte operační systém řady.

   ![Povolte ochranu pro fyzické servery](./media/site-recovery-vmware-to-azure-classic/enable-protection1.png)
4. V **zadat cílové prostředky**, vyberte účet úložiště, který používáte pro replikaci a vyberte, zda nastavení se použije pro všechny úlohy. Prémiové účty úložiště nejsou aktuálně podporovány.

   > [!NOTE]
   > Nepodporujeme přesunutí účty úložiště vytvořené pomocí [portál Azure](../storage/storage-create-storage-account.md) mezi skupinami prostředků.                           
   > [Migrace účtů úložiště](../azure-resource-manager/resource-group-move-resources.md) napříč skupinami prostředků v rámci stejného předplatného nebo napříč předplatnými se pro účty úložiště použité k nasazení Site Recovery nepodporuje.
   >
   >

    ![Konfigurace nastavení cíle](./media/site-recovery-vmware-to-azure-classic/enable-protection3.png)
5. V **zadejte účty**, vyberte účet, který jste [nastavit](#install-the-mobility-service-with-push-installation) pro automatické instalaci služby Mobility.

    ![Zadejte účty](./media/site-recovery-vmware-to-azure-classic/enable-protection4.png)
6. Kliknutím na značku zaškrtnutí dokončete přidání počítače do skupiny ochrany a spustit počáteční replikaci pro každý počítač.

   > [!NOTE]
   > Pokud připravil nabízenou instalaci služby Mobility je automaticky nainstaluje na počítače, které nemají ho jako jste přidali do skupiny ochrany. Po instalaci služby spustí úlohu ochrany a selže. Po selhání budete muset restartovat každý počítač, který bude mít nainstalovánu službu Mobility ručně. Po restartování znovu začne úlohu ochrany a dojde k počáteční replikaci.
   >
   >

Můžete sledovat stav na **úlohy** stránky.

![Monitorování stavu na stránce úlohy](./media/site-recovery-vmware-to-azure-classic/enable-protection5.png)

Stav ochrany je možné monitorovat také v **chráněné položky** > *název skupiny ochrany* > **virtuální počítače**. Po dokončení počáteční replikace a data se synchronizují, změní stav počítače na **chráněné**.

![Stav monitorování v chráněné položky](./media/site-recovery-vmware-to-azure-classic/enable-protection6.png)

## <a name="step-11-set-protected-machine-properties"></a>Krok 11: Sada chráněných vlastnosti počítače.
1. Jakmile počítač **chráněné** stav, můžete konfigurovat její vlastnosti převzetí služeb při selhání. V podrobnostech skupiny ochrany, vyberte počítač a otevřete **konfigurace** kartě.
2. Site Recovery automaticky navrhuje vlastnosti pro virtuální počítač Azure a zjistí, že místní síťová nastavení.

    ![Nastavit vlastnosti virtuálního počítače](./media/site-recovery-vmware-to-azure-classic/vm-properties1.png)
3. Můžete změnit tato nastavení:

   * **Název virtuálního počítače Azure**: je to název, který bude přiřazen k počítači v Azure po převzetí služeb při selhání. Název musí splňovat požadavky pro Azure.
   * **Velikost virtuálního počítače Azure**: počet síťových adaptérů je závisí na velikosti zadáte pro cílový virtuální počítač. Další informace o velikosti a adaptéry najdete v tématu [velikost tabulky](../virtual-machines/linux/sizes.md). Poznámky:

     * Když změníte velikost virtuálního počítače a uložit nastavení, počet síťových adaptérů se změní při otevření **konfigurace** kartě příště. Minimální počet síťových adaptérů cílových virtuálních počítačů se rovná minimální počet síťových adaptérů na zdrojovém virtuálním počítači. Maximální počet síťových adaptérů je dáno velikost virtuálního počítače.
       * Pokud počet síťových adaptérů na zdrojovém počítači je menší než nebo roven počtu adaptérů, které jsou povolené pro velikost cílového počítače, bude cíl mít stejný počet adaptérů jako zdroj.
       * Pokud počet adaptérů pro zdrojový virtuální počítač překračuje počet povolený pro cílovou velikost, použije se maximální velikost cíle.
        Pokud má například zdrojový počítač dva síťové adaptéry a velikost cílového počítače podporuje čtyři, bude mít cílový počítač dva adaptéry. Pokud má zdrojový počítač dva adaptéry, ale podporovaná velikost cíle podporuje pouze jeden, bude mít cílový počítač jenom jeden adaptér.
     * Pokud virtuální počítač má několik síťových adaptérů, všechny adaptéry by měl být připojen ke stejné síti Azure.
   * **Síť Azure**: je nutné zadat síť Azure, které virtuální počítače Azure připojí po převzetí služeb při selhání. Pokud nezadáte jeden, virtuální počítače Azure se nepřipojí k žádné síti. Kromě toho budete muset zadat síť Azure, pokud chcete navrácení služeb po obnovení z Azure do místní lokality. Navrácení služeb po obnovení vyžaduje propojení VPN mezi síť Azure a místní sítě.
   * **Azure podsíť IP adres nebo**: pro každý síťový adaptér, vyberte podsíť, k němuž by měl připojit virtuální počítač Azure. Všimněte si, že pokud síťový adaptér zdrojového počítače je nakonfigurován pro použití statické IP adresy, můžete zadat statickou IP adresu pro virtuální počítač Azure. Pokud nezadáte statickou IP adresu, bude přiděleno libovolná dostupná IP adresa. Pokud je zadána cílová IP adresa, ale je již používán jiným virtuálním počítačem v Azure, převzetí služeb při selhání se nezdaří. Pokud síťový adaptér zdrojového počítače je nakonfigurován pro používání protokolu DHCP, budete mít DHCP jako nastavení pro Azure.      

## <a name="step-12-create-a-recovery-plan-and-run-a-failover"></a>Krok 12: Vytvoření plánu obnovení a spusťte převzetí služeb při selhání
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Enhanced-VMware-to-Azure-Failover/player]
>
>

Můžete spustit převzetí služeb při selhání pro jeden počítač, nebo můžete převzít více virtuálních počítačů, které provést stejný úkol nebo spustit stejné zatížení. Chcete-li převzetí služeb při selhání více počítačů najednou, přidejte ho do plánu obnovení.

Vytvoření plánu obnovení:

1. Na **plány obnovení** klikněte na tlačítko **plánu obnovení přidat** a přidejte plán obnovení. Zadejte podrobnosti pro plán a vyberte **Azure** jako cíl.

 ![Konfigurace plánu obnovení](./media/site-recovery-vmware-to-azure-classic/recovery-plan1.png)
2. V **vybrat virtuální počítač**, vyberte skupinu ochrany a pak vyberte počítače do skupiny přidat do plánu obnovení.

 ![Přidat virtuální počítače](./media/site-recovery-vmware-to-azure-classic/recovery-plan2.png)

Můžete upravit plán vytváření skupin a pořadí pořadí, ve kterém jsou počítače v plánu obnovení při selhání. Můžete také přidat skripty a pokynů pro ruční akce. Skripty lze vytvořit ručně nebo pomocí [sad Azure Automation Runbook](site-recovery-runbook-automation.md). Další informace o přizpůsobení plánů obnovení najdete v tématu [vytvořit plány obnovení](site-recovery-create-recovery-plans.md).

## <a name="run-a-failover"></a>Spuštění převzetí služeb při selhání
Před spuštěním převzetí služeb při selhání:

* Ujistěte se, že server pro správu je spuštěné a dostupné. Pokud není, převzetí služeb při selhání se nezdaří.
* Pokud spustíte neplánované převzetí služeb při selhání:

  * Pokud je to možné, měli byste před spuštěním neplánovaného převzetí služeb při selhání vypnout primární počítače. To zajistí, že nebudete mít ve stejnou dobu spuštěný jak zdrojový počítač, tak počítač repliky. Pokud replikujete virtuální počítače VMware, když spustíte neplánované převzetí služeb při selhání, můžete zadat, že Site Recovery se pokuste vypnout zdrojový počítač. V závislosti na stavu primární lokality tato operace může nebo nemusí fungovat. Pokud fyzické servery replikujete, není tato možnost nabízí Site Recovery.
  * Neplánované převzetí služeb při selhání se zastaví data replikace z primárních počítačů tak, aby žádná rozdílová data nebudou předávány po zahájení neplánovaného převzetí služeb při selhání.
  * Pokud se chcete připojit k virtuálnímu počítači repliky v Azure po převzetí služeb při selhání, povolte připojení ke vzdálené ploše na zdrojovém počítači před spuštěním převzetí služeb při selhání. Potom povolí připojení RDP přes bránu firewall. Také budete muset povolit RDP na veřejný koncový bod virtuálního počítače Azure po převzetí služeb při selhání. Postupujte podle [osvědčené postupy](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx) zajistit, že protokol RDP funguje po selhání.

> [!NOTE]
> Chcete-li získat nejlepší výkon při selhání do Azure, ujistěte se, nainstalovaného agenta Azure v chráněném počítači. To pomůže rychleji spuštění počítače a pomáhá diagnostikovat problémy. Azure Agent je k dispozici pro [Linux](https://github.com/Azure/WALinuxAgent) a [Windows](http://go.microsoft.com/fwlink/?LinkID=394789).
>
>

### <a name="run-a-test-failover"></a>Spuštění testovacího převzetí služeb při selhání
Spustit převzetí služeb při selhání simuluje váš převzetí služeb při selhání a obnovení procesy v izolovanou síť, která nemá vliv provozním prostředí a umožňuje běžné replikace pokračovat normální. Testovací převzetí služeb při selhání je initiatd ve zdroji a můžete ho spustit v několika způsoby:

* **Nezadávejte síť Azure**: Pokud spustíte převzetí služeb při selhání bez sítě, test se zkontrolujte, zda virtuální počítače spustit a zobrazí správně v Azure. Virtuální počítače nebudou připojené sítě Azure po převzetí služeb při selhání.
* **Zadejte síť Azure**: Tento typ převzetí služeb při selhání kontroluje očekávaným dodává zobrazí celé prostředí replikace a že jsou virtuální počítače Azure připojený k zadané síti.

Chcete-li spustit testovací převzetí služeb:

1. Na **plány obnovení** vyberte plán a klikněte na tlačítko **testovací převzetí služeb při selhání**.

 ![Vyberte plán](./media/site-recovery-vmware-to-azure-classic/test-failover1.png)
2. V **potvrďte testovací převzetí služeb při selhání**, vyberte **žádné** k označení, že nechcete používat síť Azure pro testovací převzetí služeb, nebo vyberte síť, ke kterému se připojí testovací virtuální počítače po převzetí služeb při selhání. Kliknutím na značku zaškrtnutí zahájíte převzetí služeb při selhání.

 ![Proveďte výběr](./media/site-recovery-vmware-to-azure-classic/test-failover2.png)
3. Sledovat průběh převzetí služeb při selhání na **úlohy** kartě.

 ![Monitorování průběhu](./media/site-recovery-vmware-to-azure-classic/test-failover3.png)
4. Po dokončení převzetí byste také měli vidět Azure repliky na počítač se zobrazí v **virtuální počítače** na portálu Azure. Pokud chcete iniciovat připojení ke vzdálené ploše virtuálního počítače Azure, budete muset otevřít port 3389 na koncový bod virtuálního počítače.
5. Poté, co jste dokončení, když převzetí služeb při selhání dosáhne **Complete** testování fáze, klikněte na tlačítko **dokončení testovacího** ukončíte. V **poznámky**, zaznamenejte a uložte jakékoli připomínky související s testovací převzetí služeb.
6. Klikněte na tlačítko **test převzetí** automaticky vyčistěte testovací prostředí. Po dokončení, se zobrazí převzetí služeb při selhání **Complete** stavu. Všechny prvky nebo virtuální počítače automaticky vytvořené během testu převzetí služeb se odstraní. Pokud převzetí služeb při selhání pokračovat déle než dva týdny, obsahuje vynucené ukončíte.

### <a name="run-an-unplanned-failover"></a>Spustit neplánované převzetí služeb při selhání
Neplánované převzetí služeb při selhání je zahájeno z Azure a možnost provádět i v případě, že primární lokalita není k dispozici.

1. Na **plány obnovení** vyberte plán a klikněte na tlačítko **převzetí služeb při selhání** > **neplánované převzetí služeb při selhání**.

 ![Vyberte plán](./media/site-recovery-vmware-to-azure-classic/unplanned-failover1.png)
2. Pokud replikujete virtuální počítače VMware, můžete zkusit vypnout místní virtuální počítače. Toto je akce typu best effort a převzetí služeb při selhání pokračovat, zda úsilí úspěšné nebo ne. Pokud dojde k neúspěchu, podrobnosti o chybě se zobrazí na **úlohy** v části **neplánované převzetí služeb při selhání úlohy**.

 ![Možnost vypnutí místní virtuální počítače](./media/site-recovery-vmware-to-azure-classic/unplanned-failover2.png)

 > [!NOTE]
 > Tato možnost není dostupná, pokud replikujete fyzických serverů. Budete muset pokusí vypnout ty, ručně Pokud je to možné.
 >
 >

3. V **potvrzení převzetí služeb při selhání**zkontrolujte směr převzetí služeb při selhání (do Azure) a vyberte bod obnovení, který chcete použít převzetí služeb při selhání. Pokud jste povolili více virtuálních počítačů, když jste nakonfigurovali vlastnosti replikace, můžete obnovit do nejnovějšího bodu obnovení aplikace nebo konzistentní při selhání. Můžete také vybrat **bod obnovení vlastní** obnovit do dřívějšího bodu v čase. Kliknutím na značku zaškrtnutí zahájíte převzetí služeb při selhání.

 ![Potvrďte směr převzetí služeb při selhání](./media/site-recovery-vmware-to-azure-classic/unplanned-failover3.png)
4. Počkejte na dokončení úlohy neplánované převzetí služeb při selhání. Můžete sledovat průběh převzetí služeb při selhání na **úlohy** kartě. I v případě, že během neplánované převzetí služeb při selhání dojít k chybám, plán obnovení běží, dokud se nedokončí. Můžete mít také možnost Azure repliky na počítač se zobrazí v **virtuální počítače** na portálu Azure.

### <a name="connect-to-replicated-azure-virtual-machines-after-failover"></a>Připojení k replikovat virtuální počítače Azure po převzetí služeb při selhání
Chcete-li připojit replikované virtuální počítače v Azure po převzetí služeb při selhání, je třeba:

- Připojení vzdálené plochy na primárním počítači povoleno.
- Brána Windows Firewall v primárním počítači nastaveno na úroveň protokolu RDP.
- RDP přidá do veřejného koncového bodu pro virtuální počítač Azure.

Další informace o toto nastavení najdete v tématu [řešení potíží s připojení ke vzdálené ploše po převzetí služeb při selhání pomocí automatické obnovení systému](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).

## <a name="deploy-additional-process-servers"></a>Nasadit další proces servery
Pokud musí horizontální škálování vašeho nasazení nad 200 zdrojového počítače, nebo pokud vaše celkové denní klidové vytížení větší než 2 TB, budete potřebovat další proces serverů pro zpracování provozu. Pokud chcete nastavit server služby další proces, zkontrolujte požadavky na [další proces servery](#additional-process-servers)a potom nastavit procesový server podle následujících pokynů. Po nastavení serveru, můžete nakonfigurovat zdrojový počítač používat.

### <a name="set-up-an-additional-process-server"></a>Nastavení serveru další proces
Nastavení serveru další proces takto:

* Jednotná Průvodce konfiguruje server pro správu jako pouze procesní server.
* Pokud chcete ke správě replikace dat pomocí pouze nový procesový server, budete muset migrovat chráněné počítače.

### <a name="install-the-process-server"></a>Instalace serveru procesu
1. Na **rychlý Start** stránky, stáhněte sjednocený instalační soubor pro instalaci součásti Site Recovery. Spusťte instalační program.
2. V části **Než začnete** vyberte **Přidat další procesové servery pro horizontální navýšení kapacity nasazení**.

 ![Přidání procesového serveru](./media/site-recovery-vmware-to-azure-classic/add-ps1.png)
3. Dokončete průvodce, jako jste to udělali při jste [nastavit](#step-5-install-the-management-server) první server pro správu.
4. V **podrobnosti o konfiguraci serveru**, zadejte IP adresu původního serveru pro správu, na kterém je nainstalovaný konfigurační server a pak zadejte heslo. Pokud nemáte heslo, spusťte  **<SiteRecoveryInstallationFolder>\home\sysystems\bin\genpassphrase.exe – n** na původního serveru pro správu ho chcete zjistit.

 ![Podrobnosti o konfiguraci serveru](./media/site-recovery-vmware-to-azure-classic/add-ps2.png)

### <a name="migrate-machines-to-use-the-new-process-server"></a>Migraci počítačů používat nový procesový server
1. Otevřete **konfigurační servery** > **Server** > *názvem původního serveru pro správu* > **serveru Podrobnosti o**.

 ![Podrobnosti o serveru](./media/site-recovery-vmware-to-azure-classic/update-process-server1.png)
2. V **proces servery** seznamu, vyberte **změnit procesový Server** vedle serveru, který chcete změnit.

 ![Aktualizace procesového serveru](./media/site-recovery-vmware-to-azure-classic/update-process-server2.png)
3. Vyberte **změnit procesový Server**, vyberte **cílový procesový Server**a pak vyberte nový server pro správu. Potom vyberte virtuální počítače, které bude zpracovávat nový procesový server. Klikněte na ikonu informace získat informace o serveru. Průměrná prostor, který je potřeba k replikaci každý vybraný virtuální počítač na nový server procesu se zobrazí vám pomůže provádět rozhodnutí týkající se načtení. Kliknutím na značku zaškrtnutí zahájíte replikace pro nový procesový server.

 ![Změnit procesový server](./media/site-recovery-vmware-to-azure-classic/update-process-server3.png)

## <a name="vmware-permissions-for-vcenter-access"></a>VMware oprávnění pro přístup k systému vCenter
Procesový server může automaticky zjistit virtuální počítače na vCenter server. Pokud chcete provést automatické zjišťování, musíte se k definování role (Azure_Site_Recovery) na úrovni vCenter a povolit Site Recovery k přístupu k serveru vCenter server. Pokud potřebujete pouze k migraci počítačů VMware do Azure a nebudete muset navrácení služeb po obnovení z Azure, můžete definovat roli jen pro čtení, která je dostačující. Nastavení oprávnění, jak je popsáno v [krok 6: nastavení pověření pro vCenter server](#step-6-set-up-credentials-for-the-vcenter-server). Oprávnění role jsou shrnuty v následující tabulce:

| **Role** | **Podrobnosti** | **Oprávnění** |
| --- | --- | --- |
| Azure_Site_Recovery role |Virtuální počítač VMware zjišťování |Přiřadíte tato oprávnění pro server v Center:<br/><br/>Úložiště dat: Přidělit prostor, procházet úložiště, operace se soubory nízké úrovně, odstraňte soubor, soubory aktualizace virtuálního počítače<br/><br/>Síť: Přiřadit sítě<br/><br/>Prostředek: Virtuální počítač přiřadit fondu zdrojů, migrací používá technologii vypnout virtuální počítač, migrace používá technologii na virtuální počítač<br/><br/>Úlohy: Vytvoření úlohy, úloha aktualizace<br/><br/>Virtuální počítač > Konfigurace<br/><br/>Virtuální počítač > interakcí > odpovědět na otázku, připojení zařízení, nakonfigurovat CD média, konfigurovat disketová média, vypnutí napájení, instalaci nástroje VMware<br/><br/>Virtuální počítač > inventáře > vytvořit, registrace, Unregister<br/><br/>Virtuální počítač > zřizování > Povolit stahování virtuálního počítače, nahrávání souborů virtuálního počítače povolit<br/><br/>Virtuální počítač > snímky > odebrat snímky |
| role uživatele vCenter |Virtuální počítač VMware zjišťování nebo převzetí služeb při selhání bez vypnutí zdrojového virtuálního počítače |Přiřadíte tato oprávnění pro server v Center:<br/><br/>Objekt datového centra > Propagate pro podřízený objekt role = jen pro čtení <br/><br/>Uživatel je přiřazena na úrovni datacenter a proto má přístup ke všem objektům v datovém centru. Pokud chcete omezit přístup, přiřadit **žádný přístup** role s **Propagate na podřízené** objekt, který má podřízené objekty (hostitelů ESX, datastores, virtuální počítače a sítě). |
| role uživatele vCenter |Převzetí služeb při selhání a navrácení služeb po obnovení |Přiřadíte tato oprávnění pro server v Center:<br/><br/>Objekt Datacenter – Propagate pro podřízený objekt role = Azure_Site_Recovery<br/><br/>Uživatel je přiřazena na úrovni datacenter a proto má přístup ke všem objektům v datovém centru.  Pokud chcete omezit přístup, přiřadit ** žádný přístup ** role s **Propagate pro podřízený objekt** pro podřízený objekt (hostitelů ESX, datastores, virtuální počítače a sítě). |

## <a name="third-party-software-notices-and-information"></a>Oznámení software třetích stran a informace
<!--Do Not Translate or Localize-->

Software a firmware spuštěné v produktu společnosti Microsoft nebo služba je založena na nebo zahrnuje materiálu z projekty uvedené níže (dále souhrnně nazývané "kód třetí strany").  Společnost Microsoft se není původní autor kód třetích stran.  Původní o autorských právech a licenci, pod kterým společnost Microsoft takový kód třetích stran, jsou nastavené níže uvedenými.

Informace v části A je týkající se součásti kód třetích stran z projekty uvedené níže. Tyto licence a informace jsou uvedené pro pouze pro informační účely.  Tento kód třetích stran, je právě relicensed vám společnost Microsoft pod licenční podmínky softwaru společnosti Microsoft pro produkty společnosti Microsoft nebo služby.  

Informace v části B je týkající se součástí kód třetích stran, které jsou určeny jako dostupné pro vás společností Microsoft v rámci původní licenční podmínky.

Dokončení soubor najdete na [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=529428). Společnost Microsoft si vyhrazuje všechna práva, která nejsou výslovně udělena, zda implicitně, jako překážku uplatnění nároku či jiným způsobem.

## <a name="next-steps"></a>Další kroky
[Další informace o navrácení služeb po obnovení](site-recovery-failback-azure-to-vmware-classic.md) uvést vaše počítače při selhání spuštění v Azure zpět do místního prostředí.
