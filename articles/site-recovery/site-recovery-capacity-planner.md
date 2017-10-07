---
title: aaaEstimate replikace kapacity v Azure | Microsoft Docs
description: "Použijte tento článek tooestimate kapacity při replikaci s Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: 0a1cd8eb-a8f7-4228-ab84-9449e0b2887b
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/05/2017
ms.author: nisoneji
ms.openlocfilehash: 54d10e50dd4fc1b875273c7fc0f38f0e85dadddc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="plan-capacity-for-protecting-virtual-machines-and-physical-servers-in-azure-site-recovery"></a>Plánování kapacity pro ochranu virtuálních počítačů a fyzických serverů ve službě Azure Site Recovery

Nástroj Azure Site Recovery Capacity Planner Hello pomáhá toofigure se vaše požadavky na kapacitu při replikaci virtuálních počítačů Hyper-V, virtuálních počítačů VMware a fyzické servery Windows nebo Linuxem s Azure Site Recovery.

Pomocí Site Recovery Capacity Planner tooanalyze hello vaše zdrojové prostředí a úlohy, odhadnout požadavky na šířku pásma a prostředky serveru, které budete potřebovat pro umístění zdroje hello a hello prostředky (virtuální počítače a úložiště atd), které musíte v cílové hello umístění.

Hello nástroj můžete spustit v několika způsoby:

* **Rychlé plánování**: Spusťte nástroj hello v tomto režimu tooget sítě a serveru projekce založené na průměrném počtu virtuálních počítačů, disků, úložiště a míru změn.
* **Podrobné plánování**: Spusťte nástroj hello v tomto režimu a uveďte podrobné informace o každé zatížení na úrovni virtuálního počítače. Analýza kompatibility virtuálních počítačů a získat projekce sítě a serveru.

## <a name="before-you-start"></a>Než začnete


1. Shromážděte informace o prostředí, včetně virtuálních počítačů, disků na virtuální počítač, úložišti na disk.
2. Určete vaše denní míry změn s ohledem pro replikovaná data. toodo toto:

   * Pokud replikujete virtuální počítače Hyper-V, pak stáhnout hello [nástroj plánování kapacity služby technologie Hyper-V](https://www.microsoft.com/download/details.aspx?id=39057) míru změn tooget hello. [Další informace](site-recovery-capacity-planning-for-hyper-v-replication.md) o tomto nástroji. Doporučujeme, aby že tento nástroj spustíte přes toocapture týden průměry.
   * Pokud replikujete virtuální počítače VMware, použijte hello [Azure Site Recovery nasazení Planner](./site-recovery-deployment-planner.md) toofigure out hello změn rychlost.
   * Pokud fyzické servery replikujete, musíte tooestimate ručně.

## <a name="run-hello-quick-planner"></a>Spustit Rychlé Planner hello
1. Stáhněte si a nainstalujte hello [Azure Site Recovery Capacity Planner](http://aka.ms/asr-capacity-planner-excel) nástroj. Musíte toorun makra, takže vyberte tooenable úpravy a povolit obsahu po zobrazení výzvy.
2. V **vyberte typ planner** vyberte **rychlé Planner** z pole se seznamem hello.

   ![Začínáme](./media/site-recovery-capacity-planner/getting-started.png)
3. V hello **Capacity Planner** listu, zadejte hello požadované informace. Je nutné vyplnit všechna pole hello v kroužku hello – snímek obrazovky níže červeně.

   * V **vyberte váš scénář**, zvolte **technologie Hyper-V tooAzure** nebo **VMware nebo fyzický tooAzure**.
   * V **frekvence (%) změny průměrný denní data**, umístí informace hello shromáždíte pomocí hello [nástroj plánování kapacity služby technologie Hyper-V](site-recovery-capacity-planning-for-hyper-v-replication.md) nebo hello [Azure Site Recovery nasazení Planner](./site-recovery-deployment-planner.md).  
   * **Komprese** se vztahuje pouze toocompression nabízí při replikaci virtuálních počítačů VMware nebo fyzických serverů tooAzure. Jsme odhadnout 30 % nebo více, ale můžete upravit nastavení hello podle potřeby. Pro replikaci virtuálních počítačů Hyper-V tooAzure komprese, můžete použít například Riverbed zařízení třetích stran.
   * V **uchování vstupy**, zadejte, jak dlouho uchovávání replik. Pokud replikujete VMware nebo fyzických serverů, zadejte hodnotu hello ve dnech. Pokud replikujete technologie Hyper-V, zadejte hello doba v hodinách.
   * V **počet hodin, ve které počáteční replikace pro dávku hello virtuálních počítačů by se měla Dokončit** a **počet virtuálních počítačů na jednu dávku počáteční replikace**, zadejte nastavení, které se používají požadavky na toocompute počáteční replikace.  Při nasazení Site Recovery, musí být nahrán hello celý počáteční datové sady.

   ![Vstupy](./media/site-recovery-capacity-planner/inputs.png)
4. Poté, co jste chápat hello hodnoty pro hello zdrojové prostředí, zobrazený výstup zahrnuje:

   * **Šířka pásma vyžadovaná pro replikaci rozdílů** (MB/s). Šířku pásma sítě pro rozdílová replikace se počítá na hello průměrný denní míry změny dat.
   * **Pásma potřebná pro počáteční replikaci** (MB/s). Šířku pásma sítě pro počáteční replikaci se počítá na hodnotách hello počáteční replikace, kterou chcete vložit.
   * **Vyžaduje (v GB) úložiště** je úložiště hello celkový Azure vyžaduje.
   * **Celkový počet IOPS na účty úložiště standard storage** se vypočítává podle velikost jednotky 8 kb IOPS na hello celkové standardní úložiště účtů.  Pro rychlé Planner hello číslo hello je vypočtena na základě všechny hello zdrojové virtuální počítače disky a každý den míry změny dat. Pro hello podrobné Planner, číslo hello je počítané na základě celkového počtu virtuálních počítačů, které jsou mapované toostandard virtuálních počítačích Azure a změny dat, míra na těchto virtuálních počítačích.
   * **Počet účtů úložiště standard storage** poskytuje hello celkový počet tooprotect potřeby účty úložiště standard storage hello virtuálním počítačům. Standardní účet úložiště může obsahovat až too20000 IOPS napříč všechny virtuální počítače hello ve standardní úložiště a na disk je podporováno maximálně 500 IOPS.
   * **Počet objektů blob disky potřebné** poskytuje hello počet disků, které bude vytvořena na úložiště Azure.
   * **Počet účtů úložiště premium požadované** poskytuje hello celkový počet tooprotect potřeby účty úložiště premium hello virtuálním počítačům. Zdrojového virtuálního počítače s vysokou IOPS (větší než 20000) musí prémiový účet úložiště. Prémiový účet úložiště může obsahovat až too80000 IOPS.
   * **Celkový počet IOPS na storage úrovně premium** se vypočítává podle velikost jednotky 256 kB IOPS na hello celkový prémiové účty úložiště.  Pro rychlé Planner hello číslo hello je vypočtena na základě všechny hello zdrojové virtuální počítače disky a každý den míry změny dat. Pro hello podrobné Planner, číslo hello je počítané na základě hello celkový počet virtuálních počítačů, které jsou mapované toopremium virtuální počítač Azure (series a GS DS) a změny dat hello míra na těchto virtuálních počítačích.
   * **Počet serverů konfigurace vyžaduje** ukazuje, kolik serverů konfigurace jsou požadovány pro nasazení hello.
   * **Počet serverů další proces vyžaduje** zobrazuje, zda jsou povinné, v přidání toohello procesový server, který běží na konfigurační server hello ve výchozím nastavení proces další servery.
   * **100 % další úložiště na zdroji hello** zobrazuje, zda je v umístění zdroje hello vyžadována další úložiště.

   ![Výstup](./media/site-recovery-capacity-planner/output.png)

## <a name="run-hello-detailed-planner"></a>Spustit hello podrobné plánovače

1. Stáhněte si a nainstalujte hello [Azure Site Recovery Capacity Planner](http://aka.ms/asr-capacity-planner-excel) nástroj. Musíte toorun makra, takže vyberte tooenable úpravy a povolit obsahu po zobrazení výzvy.
2. V **vyberte typ planner**, vyberte **podrobné Planner** z pole se seznamem hello.

   ![Začínáme](./media/site-recovery-capacity-planner/getting-started-2.png)
3. V hello **zatížení kvalifikace** listu, zadejte hello požadované informace. Je nutné vyplnit všechna hello označené pole.

   * V **jader procesoru**, zadejte hello celkový počet jader na zdrojovém serveru.
   * V **přidělení paměti v MB**, zadejte velikost paměti RAM hello zdrojového serveru.
   * Hello **počet síťových adaptérů**, zadejte hello počet síťových adaptérů na zdrojovém serveru.
   * V **celkové úložiště (v GB)**, zadejte hello celková velikost úložiště virtuálního počítače hello. Například pokud má zdrojový server hello 3 disky s 500 GB, pak celkovou velikost úložiště je 1500 GB.
   * V **počet disků připojených**, zadejte hello celkový počet disků ze zdrojového serveru.
   * V **využití kapacity disku**, zadejte hello průměrné využití.
   * V **denně frekvence (%) změny**, zadejte denní data o hello změnit počet zdrojového serveru.
   * V **mapování Azure velikost**, zadejte hello velikost virtuálního počítače Azure, které chcete toomap. Pokud nechcete toodo tomto ručně, klikněte na tlačítko **výpočetní virtuální počítače IaaS**. Pokud vstupní ruční nastavení a pak klikněte na výpočetní virtuální počítače IaaS, může ruční nastavení hello přepsat, protože proces výpočetní hello automaticky určí nejlepší shodu hello na velikost virtuálního počítače Azure.

   ![Kvalifikace pracovního vytížení](./media/site-recovery-capacity-planner/workload-qualification.png)
4. Pokud kliknete na tlačítko **výpočetní virtuální počítače IaaS** tady je co umožňuje:

   * Ověří hello povinné vstupy.
   * Vypočítá IOPS a navrhne hello nejlepší virtuálního počítače Azure aize shoda pro každý virtuální počítače, který je vhodné pro tooAzure replikace. Pokud správnou velikost virtuálního počítače Azure nelze rozpoznat, že dojde k chybě. Například pokud hello počet disků připojených v 65, chyba je vydána protože hello nejvyšší velikost virtuálního počítače Azure je 64.
   * Navrhuje účet úložiště, který lze použít pro virtuální počítač Azure.
   * Vypočítá hello celkový počet účty úložiště standard a premium úložiště účtů nezbytných pro zatížení hello. Posuňte se dolů tooview hello typ úložiště Azure a účet úložiště hello, které je možné ze zdrojového serveru.
   * Dokončí a seřadí hello rest hello tabulky na základě typu požadované úložiště (standard nebo premium) přiřazené pro virtuální počítač a hello připojený počet disků. U všech virtuálních počítačů, které splňují hello požadavky pro Azure, hello sloupec **je virtuální počítač kvalifikovaný?** ukazuje **Ano**. Pokud virtuální počítač nelze zálohovat tooAzure, zobrazí se chyba.

Sloupce AA tooAE jsou výstupem a zadejte informace pro každý virtuální počítač.

![Kvalifikace pracovního vytížení](./media/site-recovery-capacity-planner/workload-qualification-2.png)

### <a name="example"></a>Příklad
Jako příklad u šesti virtuálních počítačů s hello hodnoty uvedené v tabulce hello hello nástroj vypočítá a přiřadí hello nejvhodnější virtuální počítač Azure a požadavky na úložiště Azure hello.

![Kvalifikace pracovního vytížení](./media/site-recovery-capacity-planner/workload-qualification-3.png)

* V hello příklad výstupu vezměte na vědomí následující hello:

  * první sloupec Hello je ověření pro hello virtuálních počítačů, disků a změn.
  * Pro pět virtuální počítače jsou potřeba dva účty úložiště standard storage a jeden prémiový účet úložiště.
  * VM3 není splňují podmínky pro ochranu, protože jeden nebo více disků se více než 1 TB.
  * VM1 a virtuálního počítače 2 můžete použít první účet standardního úložiště hello
  * VM4 můžete použít hello druhé standardní účet úložiště.
  * VM5 a VM6 potřebovat prémiový účet úložiště, a jak používat jeden účet.

    > [!NOTE]
    > IOPS na úložiště standard a premium jsou vypočítávány na hello úroveň virtuálního počítače a nikoli na úrovni disku. Standardní virtuální počítač dokáže zpracovat až too500 IOPS na disku. Pokud jsou větší než 500 IOPS pro disk, je třeba úložiště premium. Ale pokud IOPS pro disk více než 500, ale IOPS pro hello celkový disky virtuálních počítačů jsou v rámci hello podporují standardní limity pro virtuální počítač Azure (velikost virtuálního počítače, počet disků, počet adaptérů, procesor, paměť), pak hello planner vybere standardní virtuální počítač a ne hello DS nebo GS řady. Je nutné mapování hello aktualizace toomanually Azure velikost buňky odpovídající řady DS nebo GS virtuálních počítačů.


Po všech podrobností o hello jsou na místě, klikněte na tlačítko **odeslání dat toohello planner nástroj** tooopen hello **Capacity Planner** zatížení jsou zvýrazní, tooshow, zda jsou způsobilé pro ochranu, nebo ne.

### <a name="submit-data-in-hello-capacity-planner"></a>Odesílání dat v hello Capacity Planner
1. Když otevřete hello **Capacity Planner** listu je naplněna na základě hello nastavení jste určili. Hello slovo 'zatížení, se zobrazí v hello **Infra vstupy zdroj** buňky, tooshow, který hello vstup je hello **zatížení kvalifikace** listu.
2. Pokud chcete toomake změny, je třeba toomodify hello **zatížení kvalifikace** listu a klikněte na **odeslání dat toohello planner nástroj** znovu.  

   ![Plánovač kapacity](./media/site-recovery-capacity-planner/capacity-planner.png)
