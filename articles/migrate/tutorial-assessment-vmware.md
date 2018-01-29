---
title: "Zjišťovat a vyhodnocení místní virtuální počítače VMware pro migraci do Azure s Azure migrovat | Microsoft Docs"
description: "Popisuje, jak zjišťovat a vyhodnocení místní virtuální počítače VMware pro migraci na Azure pomocí služby Azure migrovat."
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 01/08/2018
ms.author: raynew
ms.openlocfilehash: a5019d3f729f2efbd01fca021b0089c7f99b0014
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/08/2018
---
# <a name="discover-and-assess-on-premises-vmware-vms-for-migration-to-azure"></a>Zjišťovat a vyhodnocení místní virtuální počítače VMware pro migraci na Azure

[Azure migrovat](migrate-overview.md) služby vyhodnocuje místní úlohy pro migraci do Azure.

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Vytvoření projektu Azure migrovat.
> * Nastavení se místní kolekce virtuální počítač (VM), na zjištění místní virtuální počítače VMware pro vyhodnocení.
> * Skupiny virtuálních počítačů a vytvořit posouzení.


Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/pricing/free-trial/) před tím, než začnete.


## <a name="prerequisites"></a>Požadavky

- **VMware**: virtuálních počítačů, která chcete migrovat se musí spravovat přes vCenter Server běžící verzi 5.5, 6.0 nebo 6.5. Dále je třeba jedna ESXi hostitele spuštěné verze 5.0 nebo vyšší pro nasazení kolekce virtuálních počítačů. 
 
> [!NOTE]
> Podpora technologie Hyper-V je v našem plán a brzy bude povolena. 

- **účet serveru vCenter**: je třeba účet jen pro čtení pro přístup k systému vCenter Server. Azure migrací používá tento účet k vyhledání virtuálních počítačů na místě.
- **Oprávnění**: V systému vCenter Server, potřebujete oprávnění k vytvoření virtuálního počítače importováním souboru v. VAJÍČKA formátu. 
- **Nastavení statistiky**: nastavení statistiky pro vCenter Server musí být nastavená na úroveň 3 před zahájením nasazení. Pokud je nižší než úroveň 3, hodnocení budou fungovat, ale nejsou shromažďovány údaje o výkonu pro úložiště a sítě. Velikost, kterou doporučení v tomto případě bude provedeno založena na údaje o výkonu pro využití procesoru a paměti a konfigurační data pro disk a síťové adaptéry. 

## <a name="log-in-to-the-azure-portal"></a>Přihlášení k portálu Azure Portal
Přihlaste se k portálu [Azure Portal](https://portal.azure.com).

## <a name="create-a-project"></a>Vytvořit projekt

1. Na portálu Azure klikněte na tlačítko **vytvořit prostředek**.
2. Vyhledejte **Azure migrovat**a vyberte službu **migrovat Azure (preview)** ve výsledcích hledání. Poté klikněte na **Vytvořit**.
3. Zadejte název projektu a předplatné Azure pro projekt.
4. Vytvořte novou skupinu prostředků.
5. Zadejte umístění, v němž vytvořte projekt a pak klikněte na **vytvořit**. Projekt migrovat Azure můžete vytvořit pouze v oblasti západní centrální USA pro tuto verzi preview. Stále však můžete naplánovat migraci pro všechny cílové umístění Azure. K umístění zadanému pro projekt slouží pouze k ukládání metadat získané z virtuálních počítačů na místě. 

    ![Azure Migrate](./media/tutorial-assessment-vmware/project-1.png)
    


## <a name="download-the-collector-appliance"></a>Stáhnout kolekce zařízení

Azure migraci vytvoří místní počítač známé jako kolekce zařízení. Tento virtuální počítač vyhledá virtuální počítače VMware na místě a metadata o nich se odešle do služby Azure migrovat. Chcete-li nastavit kolekce zařízení, je stáhnout. VAJÍČKA souboru a naimportujte ho do místního serveru vCenter vytvořte virtuální počítač.

1. V projektu Azure migrovat, klikněte na tlačítko **Začínáme** > **Discover & hodnocení** > **zjišťovat počítače**.
2. V **zjistit počítače**, klikněte na tlačítko **Stáhnout**, chcete-li stáhnout. VAJÍČKA soubor.
3. V **zkopírujte projektu pověření**, zkopírujte ID projektu a klíče. Tyto musíte při konfiguraci kolekce.

    ![Stáhněte si soubor .ova](./media/tutorial-assessment-vmware/download-ova.png)

### <a name="verify-the-collector-appliance"></a>Ověřte kolekce zařízení

Zkontrolujte, zda. Soubor vajíčka se zabezpečení, před nasazením.

1. Na počítači, do které jste stáhli soubor otevřete příkazové okno na správce.
2. Spusťte následující příkaz pro generování hodnoty hash pro vajíčka:
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Příklad použití:```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3. Toto nastavení by měl odpovídat generované hodnoty hash.
    
    Pro verzi vajíčka 1.0.8.49
    **Algoritmus** | **Hodnota hash**
    --- | ---
    MD5 | 8779eea842a1ac465942295c988ac0c7 
    SHA1 | c136c52a0f785e1fd98865e16479dd103704887d
    SHA256 | 5143b1144836f01dd4eaf84ff94bc1d2c53f51ad04b1ca43ade0d14a527ac3f9

    Verze vajíčka 1.0.8.40:

    **Algoritmus** | **Hodnota hash**
    --- | ---
    MD5 |afbae5a2e7142829659c21fd8a9def3f
    SHA1 | 1751849c1d709cdaef0b02a7350834a754b0e71d
    SHA256 | d093a940aebf6afdc6f616626049e97b1f9f70742a094511277c5f59eacc41ad

## <a name="create-the-collector-vm"></a>Vytvoření kolekce virtuálních počítačů

Stažený soubor importujte do systému vCenter Server.

1. V konzole klienta vSphere klikněte na tlačítko **soubor** > **nasazení šablony OVF**.

    ![Nasazení OVF](./media/tutorial-assessment-vmware/vcenter-wizard.png)

2. V Průvodci nasazením šablony OVF > **zdroj**, zadejte umístění souboru .ova.
3. V **název** a **umístění**, zadejte popisný název pro kolekci virtuálních počítačů a inventář objektu, ve kterém se bude hostovat virtuální počítač.
5. V **hostiteli/clusteru**, zadejte hostitele nebo clusteru v kolekci virtuálních počítačů se spustí.
7. V úložišti zadejte cíl úložiště pro kolekci virtuálních počítačů.
8. V **formát disku**, zadejte typ disku a velikost.
9. V **mapování sítě**, zadejte síť, ke kterému se připojí kolekce virtuálních počítačů. Síť musí připojení k Internetu, k odeslání metadata do Azure. 
10. Zkontrolujte a potvrďte nastavení a pak klikněte na **Dokončit**.

## <a name="run-the-collector-to-discover-vms"></a>Spustit kolekce k vyhledání virtuálních počítačů

1. V konzole klienta vSphere klikněte pravým tlačítkem na virtuální počítač > **otevřít konzolu**.
2. Zadejte předvolby jazyka, časové pásmo a heslo pro zařízení.
3. Na ploše klikněte **spustit kolekce** zástupce.
4. V Azure migraci kolekce, otevřete **nastavit požadavky**.
    - Přijměte licenční podmínky a číst informace o jiných výrobců.
    - Kolekce kontroluje, zda virtuální počítač má přístup k Internetu.
    - Pokud virtuální počítač získá přístup k Internetu prostřednictvím proxy serveru, klikněte na tlačítko **nastavení proxy serveru**a zadat adresu proxy serveru a port naslouchání. Pokud proxy server vyžaduje ověřování, zadejte přihlašovací údaje.

    > [!NOTE]
    > Adresu proxy serveru musí být zadány ve tvaru http://ProxyIPAddress nebo http://ProxyFQDN. Je podporován pouze server proxy protokolu HTTP.

    - Kolekce kontroluje, zda je spuštěna collectorservice. Služba je nainstalovaná ve výchozím nastavení v kolekci virtuálních počítačů.
    - Stáhněte a nainstalujte VMware PowerCLI.

5. V **zadejte podrobnosti o serveru vCenter**, postupujte takto:
    - Zadejte název (FQDN) nebo IP adresa serveru vCenter.
    - V **uživatelské jméno** a **heslo**, zadejte pověření účtu jen pro čtení, které kolekce použije k vyhledání virtuálních počítačů na serveru vCenter server.
    - V **kolekce oboru**, vyberte obor pro zjišťování virtuálních počítačů. Kolekce může zjistit pouze virtuální počítače v zadaném oboru. Obor lze nastavit na konkrétní složce, datacenter nebo clusteru. Měl by neměl obsahovat více než 1 000 virtuálních počítačů. 

6. V **zadejte migrace projektu**, zadejte ID projektu Azure migrovat a klíče, že jste zkopírovali z portálu. Pokud nebylo je zkopírovat, otevřete portál Azure z kolekce virtuálních počítačů. V projektu **přehled** klikněte na tlačítko **zjišťovat počítače**a zkopírujte hodnoty.  
7. V **sledovat průběh kolekce**, monitorování zjišťování a zkontrolujte, že metadata shromážděných z virtuálních počítačů jsou v oboru. Kolekce poskytuje čas přibližnou zjišťování.

> [!NOTE]
> Kolekce podporuje pouze "Angličtina (Spojené státy)" jako jazyk operačního systému a collector jazyk rozhraní. Podpora více jazyků bude brzo.


### <a name="verify-vms-in-the-portal"></a>Ověření virtuálních počítačů na portálu

Zjišťování, že doba závisí na tom, kolik virtuálních počítačů zjišťujete. Obvykle u 100 virtuálních počítačů po dokončení kolekce jeho spuštění trvá přibližně za hodinu zjišťování. 

1. V projektu Planner migrace, klikněte na tlačítko **spravovat** > **počítače**.
2. Zkontrolujte, jestli chcete zjistit virtuální počítače se zobrazovat na portálu.


## <a name="create-and-view-an-assessment"></a>Vytvářet a prohlížet posouzení

Po zjištění virtuálních počítačů, seskupovat je a vytvořit posouzení. 

1. V projektu **přehled** klikněte na tlačítko **+ vytvořit assessment**.
2. Klikněte na tlačítko **zobrazit všechny** zobrazíte vlastnosti hodnocení.
3. Vytvořit skupinu a zadejte název skupiny.
4. Vyberte počítače, které chcete přidat do skupiny.
5. Klikněte na tlačítko **vytvořit Assessment**, chcete-li vytvořit skupinu a hodnocení.
6. Po vytvoření hodnocení ji zobrazit v **přehled** > **řídicí panel**.
7. Klikněte na tlačítko **exportovat assessment**, chcete-li stáhnout jako soubor aplikace Excel.

### <a name="sample-assessment"></a>Ukázka hodnocení

Tady je příklad hodnotící zprávu. Obsahuje informace o tom, jestli jsou kompatibilní pro Azure a odhadované měsíční náklady na virtuální počítače. 

![Zprávy o hodnocení](./media/tutorial-assessment-vmware/assessment-report.png)

#### <a name="azure-readiness"></a>Připravenost pro Azure

Toto zobrazení uvádí stav připravenosti pro každý počítač.

- Pro virtuální počítače, které jsou připravené migrovat Azure doporučuje velikost virtuálního počítače v Azure.
- Pro virtuální počítače, které ještě nejsou připraveny migrovat Azure vysvětluje, proč a poskytuje postup nápravy.
- Azure migrací navrhuje nástroje, které můžete použít pro migraci. Pokud tento počítač je vhodné pro migraci navýšení a shift, doporučuje se služba Azure Site Recovery. Pokud je databáze počítače, je doporučeno službu migrace databáze Azure.

  ![Vyhodnocení připravenosti](./media/tutorial-assessment-vmware/assessment-suitability.png)  

#### <a name="monthly-cost-estimate"></a>Odhad měsíčních nákladů

Toto zobrazení uvádí celkový počet výpočetních a náklady na úložiště spuštěných virtuálních počítačů v Azure společně s podrobnosti pro každý počítač. Odhadované náklady se počítají pomocí doporučení na základě výkonu velikost na počítač a hodnocení vlastností a jeho disky. 

> [!NOTE]
> Odhad nákladů poskytované migrovat Azure se týká spuštění místní virtuální počítače jako infrastruktury Azure jako virtuální počítače služby (IaaS). Azure migrací nebere v úvahu žádné platforma jako služba (PaaS) nebo softwaru jako služby (SaaS) náklady. 

Odhadované měsíční náklady na výpočetních operací a úložiště jsou agregována pro všechny virtuální počítače ve skupině. 

![Hodnocení virtuálního počítače náklady.](./media/tutorial-assessment-vmware/assessment-vm-cost.png) 

Můžete přejít na najdete v podrobnostech pro konkrétní počítač.

![Hodnocení virtuálního počítače náklady.](./media/tutorial-assessment-vmware/assessment-vm-drill.png) 

## <a name="next-steps"></a>Další postup

- [Další informace](how-to-scale-assessment.md) postup zjišťování a vyhodnocení velké prostředí VMware.
- Naučte se vytvářet skupiny vysokou spolehlivostí assessment pomocí [mapování závislostí počítače](how-to-create-group-machine-dependencies.md)
- [Další informace](concepts-assessment-calculation.md) o tom, jak jsou vypočítávány vyhodnocování.
