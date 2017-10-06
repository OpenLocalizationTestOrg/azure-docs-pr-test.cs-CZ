---
title: "aaaHow nemá pracovní tooAzure replikace technologie Hyper-V ve službě Site Recovery? | Dokumentace Microsoftu"
description: "Tento článek obsahuje přehled fungování replikace Hyper-V v Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: c413efcd-d750-4b22-b34b-15bcaa03934a
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/14/2017
ms.author: raynew
ROBOTS: NOINDEX, NOFOLLOW
redirect_url: site-recovery-architecture-hyper-v-to-azure
ms.openlocfilehash: e982806b4d6cdec2f71f82d8c73c17cc50ad3c33
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-does-hyper-v-replication-tooazure-work"></a>Jak funguje tooAzure replikace technologie Hyper-V?

Přečtěte si tento článek toounderstand hello architektura a pracovní postupy pro tooAzure replikace technologie Hyper-V pomocí hello [Azure Site Recovery](site-recovery-overview.md) služby.

Odeslat všechny komentáře dole hello v tomto článku, nebo v hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

Můžete replikovat hello tooAzure následující:
- **Hyper-V s VMM:** Virtuální počítače v místních hostitelích Hyper-V spravovaných v cloudech System Center Virtual Machine Manageru (VMM). Na hostitelích může běžet jakýkoli [podporovaný operační systém](site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers). Můžete replikovat virtuální počítače Hyper-V s jakýmkoli hostovaným operačním systémem, který [podporuje technologie Hyper-V a Azure](https://technet.microsoft.com/en-us/windows-server-docs/compute/hyper-v/supported-windows-guest-operating-systems-for-hyper-v-on-windows).
- **Hyper-V bez VMM:** Místní virtuální počítače v hostitelích Hyper-V, kteří nejsou spravováni v cloudech VMM. Hostitelé získat spuštěním hello [podporované operační systémy](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions). Můžete replikovat virtuální počítače Hyper-V s jakýmkoli hostovaným operačním systémem, který [podporuje technologie Hyper-V a Azure](https://technet.microsoft.com/en-us/windows-server-docs/compute/hyper-v/supported-windows-guest-operating-systems-for-hyper-v-on-windows).


## <a name="architectural-components"></a>Komponenty architektury

**Oblast** | **Komponenta** | **Podrobnosti**
--- | --- | ---
**Azure** | Pro Azure potřebujete účet Microsoft Azure, účet úložiště Azure a síť Azure. | Účty úložiště a sítě můžou být klasické účty nebo účty Resource Manageru.<br/><br/> Replikovaná data jsou uložena v účtu úložiště hello a vytvoří se virtuální počítače Azure s hello replikovat data, když dojde k převzetí služeb při selhání z vaší místní lokalitě.<br/><br/> Hello virtuálních počítačích Azure připojit toohello virtuální síť Azure, když vytváří.
**Server VMM** | Hostitelé Hyper-V umístění v cloudech VMM | Pokud jsou hostitelé Hyper-V spravované v cloudech VMM, zaregistrujte hello VMM server v hello trezor služeb zotavení.<br/><br/> Na serveru VMM hello nainstalujte hello zprostředkovatele služby Site Recovery tooorchestrate replikaci s Azure.<br/><br/> Budete potřebovat logické a sítě virtuálních počítačů nastavit mapování sítě tooconfigure. Síť virtuálních počítačů musí být propojená tooa logické sítě, který je spojen s hello cloudu.
**Hostitel Hyper-V** | Servery Hyper-V je možné nasadit se serverem VMM nebo bez. | Pokud není k dispozici žádný server VMM, hello hello zprostředkovatele služby Site Recovery je nainstalována na replikaci tooorchestrate hello hostitele pomocí Site Recovery přes internet. Pokud je VMM server, hello zprostředkovatele je nainstalována na něm a není na hostiteli hello.<br/><br/> agent služeb zotavení Hello je nainstalován na replikaci dat toohandle hello hostitele.<br/><br/> Komunikace z hello poskytovatele i agenta hello je zabezpečená a šifrovaná. Šifrují se rovněž replikovaná data v úložišti Azure.
**Virtuální počítače Hyper-V** | Budete potřebovat jeden nebo více virtuálních počítačů na serveru hostitele technologie Hyper-V hello. | Nic musí tooexplicitly nainstalovaných na virtuálních počítačích

## <a name="deployment-steps"></a>Kroky nasazení

1. **Azure**: nastavíte hello Azure součásti. Doporučujeme připravit účty úložiště a sítě ještě než začnete s nasazením Site Recovery.
2. **Trezor:** Vytvoříte trezor služby Recovery Services pro Site Recovery a nakonfigurujete nastavení trezoru, včetně konfigurace nastavení zdroje a cíle, nastavení zásad replikace a povolení replikace.
3. **Zdroj a cíl:**
    - **Hostitelé technologie Hyper-V v cloudech VMM**: jako součást zadání nastavení zdroje, stáhněte a nainstalujte hello zprostředkovatele Azure Site Recovery na serveru VMM hello a hello agenta služeb zotavení Azure na každém hostiteli technologie Hyper-V. hello VMM server bude zdroj Hello. cíl Hello je Azure.
    - Hostitelé Hyper-V bez VMM: když zadáte nastavení zdroje, můžete stáhnout a nainstalovat hello zprostředkovatel a agent na každém hostiteli technologie Hyper-V. Během nasazování shromážděte hello hostitele do lokality Hyper-V a zadejte tento server jako zdroj hello. cíl Hello je Azure.

    ![Replikace technologie Hyper-V nebo VMM tooAzure](./media/site-recovery-components/arch-onprem-onprem-azure-vmm.png) ![tooAzure replikace technologie Hyper-V lokality](./media/site-recovery-components/arch-onprem-azure-hypervsite.png)


4. **Zásady replikace**: vytvořte zásadu replikace pro VMM cloud nebo server hello technologie Hyper-V. zásady Hello je použité tooall virtuální počítače umístěné na hostitelích na hello webu nebo v cloudu.
5. **Povolení replikace:** Povolíte replikaci pro virtuální počítače Hyper-V. V souladu s nastavením zásad replikace hello dojde k počáteční replikaci. Se sledují změny dat a replikaci rozdílové změny tooAzure se spustí po dokončení počáteční replikace hello. Sledované změny se pro jednotlivé položky ukládají do souboru .hrl.
6. **Testovací převzetí služeb při selhání**: Spusťte toomake testovací převzetí služeb při selhání, zda vše funguje podle očekávání.

Další informace o nasazení:
- [Začínáme s tooAzure replikace virtuálního počítače technologie Hyper-V - s nástrojem VMM](site-recovery-vmm-to-azure.md)
- [Začínáme s tooAzure replikace virtuálního počítače technologie Hyper-V - bez VMM](site-recovery-hyper-v-site-to-azure.md)

## <a name="hyper-v-replication-workflow"></a>Pracovní postup replikace Hyper-V

### <a name="enable-protection"></a>Povolení ochrany

1. Po povolení ochrany pro virtuální počítač technologie Hyper-V v hello portálu Azure nebo místním hello **povolení ochrany** spustí.
2. Hello úlohy se zkontroluje, aby hello počítač splňuje požadavky, před vyvoláním hello [CreateReplicationRelationship](https://msdn.microsoft.com/library/hh850036.aspx), tooset replikaci s hello nastavení, které jste nakonfigurovali.
3. Hello úloha spustí počáteční replikace vyvoláním hello [StartReplication](https://msdn.microsoft.com/library/hh850303.aspx) metodu, tooinitialize úplné replikace virtuálního počítače a virtuální disky tooAzure odesílání hello Virtuálního počítače.
4. Můžete monitorovat hello úlohy v hello **úlohy** kartě.      ![Seznam úloh](media/site-recovery-hyper-v-azure-architecture/image1.png)![Podrobnosti povolení ochrany](media/site-recovery-hyper-v-azure-architecture/image2.png)

### <a name="initial-replication"></a>Počáteční replikace

1. Při aktivaci počáteční replikace se pořídí [snímek virtuálního počítače Hyper-V](https://technet.microsoft.com/library/dd560637.aspx).
2. Virtuální pevné disky se replikují jeden po druhém, dokud jsou všechny zkopírovaný tooAzure. Ho může nějakou dobu trvat, v závislosti na hello velikost virtuálního počítače a šířku pásma sítě. toooptimize využití sítě, najdete v části [jak toomanage místní využití šířky pásma sítě ochrany tooAzure](https://support.microsoft.com/kb/3056159).
3. Pokud dojde k změny na disku, když probíhá počáteční replikace, hello technologie Hyper-V Replica Replication Tracker sleduje tyto změny jako protokoly replikace technologie Hyper-V (.hrl). Tyto soubory jsou umístěny v hello stejné složce jako disky hello. Každý disk má přidružený soubor .hrl, odešle toosecondary úložiště.
4. Hello soubory snímků a protokolů spotřebovávají prostředky disku, když probíhá počáteční replikace.
5. Po dokončení počáteční replikace hello je odstranění snímku virtuálního počítače hello. Rozdílové změny na disku v protokolu hello jsou synchronizovaná a sloučené toohello nadřazený disk.


### <a name="finalize-protection"></a>Dokončení ochrany

1. Po počáteční replikaci hello dokončení hello **dokončit ochranu na virtuálním počítači hello** nakonfiguruje úloha sítě a další postreplikační nastavení tak, aby hello virtuální počítač je chráněný.
    ![Úloha dokončení ochrany](media/site-recovery-hyper-v-azure-architecture/image3.png)
2. Pokud replikujete tooAzure, můžete potřebovat tootweak hello nastavení pro virtuální počítač hello tak, aby byl připraven k převzetí služeb při selhání. V tomto okamžiku můžete spustit toocheck testovací převzetí služeb při selhání, které všechno funguje podle očekávání.

### <a name="delta-replication"></a>Rozdílová replikace

1. Po počáteční replikaci hello zahájí rozdílová synchronizace, v souladu s nastavením replikace.
2. Hello technologie Hyper-V Replica Replication Tracker sleduje změny hello tooa virtuální pevný disk jako soubory .hrl. Každý disk nakonfigurovaný pro replikaci má přidružený soubor .hrl. Tento protokol se odesílají toohello zákazníkův účet úložiště, po dokončení počáteční replikace. Pokud je do protokolu přenosu tooAzure, se sledují změny hello hello primární disku v souboru protokolu, v hello stejný adresář.
3. Během počáteční a rozdílové replikace můžete monitorovat hello virtuálních počítačů v hello zobrazení virtuálních počítačů. [Další informace](site-recovery-monitoring-and-troubleshooting.md#monitor-replication-health-for-virtual-machines).  

### <a name="replication-synchronization"></a>Synchronizace replikace

1. Pokud rozdílová replikace selže a úplná replikace by byla náročná, pokud jde o šířku pásma nebo čas, pak se virtuální počítač označí pro resynchronizaci. Například pokud hello soubory .hrl dosáhnou 50 % velikosti disku hello, pak hello virtuálního počítače budou označeny pro opětovnou synchronizaci.
2.  Resynchronizace minimalizuje hello množství dat odesílaných výpočtů kontrolních součtů hello zdrojové a cílové virtuální počítače a odesláním pouze hello rozdílová data. Resynchronizace využívá algoritmus vytváření bloků s pevnou velikostí, kdy jsou zdrojové a cílové soubory rozdělené do pevných bloků. Kontrolní součty pro každého bloku jsou generovány a potom porovná toodetermine, která blokuje z hello zdroj nutné toobe použité toohello cíle.
3. Po dokončení resynchronizace by měla pokračovat normální rozdílová replikace. Ve výchozím nastavení se Opakovaná synchronizace naplánované toorun automaticky mimo pracovní dobu, ale můžete ji u virtuálního počítače ručně. Například můžete pokračovat v resynchronizaci, pokud dojde k výpadku sítě nebo jinému výpadku. toodo se vyberte hello virtuálních počítačů v portálu hello > **Opětovná synchronizace**.

    ![Ruční resynchronizace](media/site-recovery-hyper-v-azure-architecture/image4.png)


### <a name="retries"></a>Opakování

Pokud dojde k chybě replikace, je předdefinován opakovaný pokus. Tuto logiku je možné rozdělit do dvou kategorií:

**Kategorie** | **Podrobnosti**
--- | ---
**Neopravitelné chyby** | Pokus se nebude opakovat. Stav virtuálního počítače bude **Kritický** a bude nutný zásah správce. Příklady těchto chyb: přerušený řetězec virtuálního pevného disku; Neplatný stav hello repliky virtuálních počítačů; Chyby ověřování sítě: chyb autorizace; Chyby nenalezení (pro samostatné servery Hyper-V) virtuální počítač
**Opravitelné chyby** | Opakování dojde k každých intervalu replikace pomocí exponenciální back vypnout, která zvyšuje interval opakování hello z hello začátek hello první pokus o 1, 2, 4, 8 a 10 minut. Pokud chyba přetrvává, bude se pokus opakovat každých 30 minut. Příkladem můžou být chyby sítě, chyby kvůli malému místu na disku nebo nedostatek paměti. |

## <a name="protection-and-recovery-lifecycle"></a>Životní cyklus ochrany a zotavení

![pracovní postup](./media/site-recovery-components/arch-hyperv-azure-workflow.png)

## <a name="next-steps"></a>Další kroky

- [Kontrola požadavků nasazení](site-recovery-prereq.md)
- Řešení potíží:
    - [Monitorování a řešení potíží s ochranou](site-recovery-monitoring-and-troubleshooting.md)
    - [Získat pomoc od podpory společnosti Microsoft](site-recovery-monitoring-and-troubleshooting.md#reach-out-for-microsoft-support)
    - [Běžné chyby a jejich řešení](site-recovery-monitoring-and-troubleshooting.md#common-azure-site-recovery-errors-and-their-resolutions)
