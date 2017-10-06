---
title: "aaaReplicate vícevrstvé Citrix XenDesktop a XenApp nasazení pomocí Azure Site Recovery | Microsoft Docs"
description: "Tento článek popisuje, jak tooprotect a obnovit Citrix XenDesktop a XenApp nasazení pomocí Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: ponatara
manager: abhemraj
editor: 
ms.assetid: 9126f5e8-e9ed-4c31-b6b4-bf969c12c184
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: ponatara
ms.openlocfilehash: c4ea9f95f91c585cdcf9d776b02c0967f4c16ab0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-a-multi-tier-citrix-xenapp-and-xendesktop-deployment-using-azure-site-recovery"></a>Replikovat vícevrstvé Citrix XenApp a XenDesktop nasazení pomocí Azure Site Recovery

## <a name="overview"></a>Přehled

Citrix XenDesktop je řešení virtualizace plochy, které nabízí plochy a aplikace jako ondemand služby tooany uživatelé kdekoli. S technologií doručení FlexCast XenDesktop můžete rychle a bezpečně poskytovat aplikace a toousers stolních počítačů.
V současné době Citrix XenApp neposkytuje žádné po havárii možnosti obnovení.

Řešení pro zotavení po havárii funkční, by měl Povolit modelování plány obnovení kolem hello výše architektury komplexní aplikace a také mít hello možnost tooadd přizpůsobit kroky toohandle aplikace mapování mezi různými úrovněmi proto poskytování jedním kliknutím zda snímek řešení v případě havárie úvodní tooa hello nižší RTO.

Tento dokument obsahuje podrobné pokyny pro vytváření řešení zotavení po havárii pro místní nasazení Citrix XenApp na Hyper-V a VMware vSphere platformách. Tento dokument také popisuje, jak tooperform testovací převzetí služeb při selhání (postupu zotavení po havárii) a tooAzure neplánované převzetí služeb při selhání pomocí plánů obnovení, hello podporované konfigurace a požadavky.


## <a name="prerequisites"></a>Požadavky

Než začnete, ujistěte se, že rozumíte hello následující:

1. [Replikaci virtuálního počítače tooAzure](site-recovery-vmware-to-azure.md)
1. Jak příliš[návrh k síti pro obnovení](site-recovery-network-design.md)
1. [Provádění tooAzure testovací převzetí služeb při selhání](site-recovery-test-failover-to-azure.md)
1. [Provádění tooAzure převzetí služeb při selhání](site-recovery-failover.md)
1. Jak příliš[replikace řadiče domény](site-recovery-active-directory.md)
1. Jak příliš[replikaci systému SQL Server](site-recovery-sql.md)

## <a name="deployment-patterns"></a>Vzory pro nasazení

Farmu Citrix XenApp a XenDesktop má obvykle hello následující vzor nasazení:

**Vzor nasazení**

Citrix XenApp a XenDesktop nasazením AD DNS serveru a SQL serveru, Citrix doručení řadiči výkladní skříň serveru, XenApp Master (VDA), Citrix XenApp licenční Server databáze

![Vzor nasazení 1](./media/site-recovery-citrix-xenapp-and-xendesktop/citrix-deployment.png)


## <a name="site-recovery-support"></a>Podpora pro obnovení lokality

Za účelem hello tohoto článku spravovat Citrix nasazení na virtuální počítače VMware vSphere 6.0 nebo System Center VMM 2012 R2 byly použité toosetup zotavení po Havárii.

### <a name="source-and-target"></a>Zdroj a cíl

**Scénář** | **tooa sekundární lokality** | **tooAzure**
--- | --- | ---
**Hyper-V** | Není v oboru | Ano
**VMware** | Není v oboru | Ano
**Fyzický server** | Není v oboru | Ano

### <a name="versions"></a>Verze
Zákazníci můžou nasazovat XenApp součásti, jako virtuální počítače běžící na Hyper-V nebo VMware nebo fyzických serverů. Azure Site Recovery chrání i tooAzure fyzické a virtuální nasazení.
Vzhledem k tomu, že XenApp 7,7 nebo novější je podporovaná v Azure, jenom nasazení tyto verze můžete převzít služby při selhání tooAzure pro migraci nebo obnovení po havárii.

### <a name="things-tookeep-in-mind"></a>Tookeep věcí v paměti

1. Ochrana a obnovení místního nasazení pomocí serveru operačního systému počítače toodeliver XenApp publikované aplikace a XenApp publikovaná stolní počítače podporována.

2. Ochrana a obnovení místního nasazení pomocí plochy operačního systému počítače toodeliver klientských počítačů VDI pro virtuální plochy klientů, včetně Windows 10, není podporována. Toto je, protože automatické obnovení systému nepodporuje obnovení hello počítačů s plochou OS'es.  Kromě toho některé virtuální plochy klientů příchutě (např. Windows 7) se ještě nepodporují pro licencování v Azure. [Další informace](https://azure.microsoft.com/pricing/licensing-faq/) týkající se licencování pro plochy klienta nebo serveru v Azure

3.  Azure Site Recovery nelze replikovat a chránit existující místní relace s více Připojeními nebo systémy současné hodnoty klonovat.
Je nutné toorecreate tyto klony pomocí Azure RM zřizování z řadiče doručení.

4. NetScaler nelze chránit pomocí Azure Site Recovery, protože NetScaler je založena na FreeBSD a Azure Site Recovery nepodporuje ochranu FreeBSD operačního systému. Bude potřebovat toodeploy a po převzetí služeb při selhání tooAzure nakonfigurovat nové zařízení NetScaler z Azure Marketplace.


## <a name="replicating-virtual-machines"></a>Replikace virtuálních počítačů

následující součásti hello Citrix XenApp nasazení Hello potřebovat toobe chráněné tooenable replikace a obnovení.

* Ochrana serveru AD DNS
* Ochranu databáze serveru SQL
* Ochrana systému Citrix doručení řadiče
* Ochrana serveru výkladní skříň.
* Ochrana XenApp hlavního serveru (VDA)
* Ochrana systému Citrix XenApp licenčního serveru


**Replikace na serveru AD DNS**

Naleznete příliš[chránit Active Directory a DNS s Azure Site Recovery](site-recovery-active-directory.md) na pokyny pro replikaci a konfiguraci řadiče domény v Azure.

**Replikace databáze serveru SQL**

Naleznete příliš[chránit SQL Server s zotavení po havárii serveru SQL a Azure Site Recovery](site-recovery-sql.md) podrobné technické informace o hello doporučené možnosti pro ochranu serverů SQL.

Postupujte podle [v tomto návodu](site-recovery-vmware-to-azure.md) toostart replikace hello tooAzure ostatní součásti virtuálních počítačů.

![Ochranu XenApp součásti](./media/site-recovery-citrix-xenapp-and-xendesktop/citrix-enablereplication.png)

**Nastavení výpočtu a sítě**

Poté, co jsou chráněné počítače hello výpočetní hello (stav zobrazí jako "Chráněné" v části replikované položky), a nastavení sítě potřebovat toobe nakonfigurované.
V výpočty a síť > výpočetní vlastnosti, můžete zadat název a cílovou velikost virtuálního počítače Azure hello.
Upravte název toocomply hello s požadavky na Azure, pokud potřebujete. Můžete také zobrazit a přidejte informace o hello Cílová síť, podsíť a IP adresu, která bude přiřazena toohello virtuálního počítače Azure.

Vezměte na vědomí následující hello:

* Můžete nastavit hello cílová IP adresa. Pokud adresu nezadáte, použije hello převzal počítač DHCP. Pokud nastavíte adresu, která není k dispozici na převzetí služeb při selhání, převzetí služeb při selhání hello nebude fungovat. Dobrý den, které lze použít stejnou cílovou IP adresu pro testovací převzetí služeb při selhání, pokud není k dispozici v hello testovací převzetí služeb při selhání sítě hello adresa.

* Pro server hello AD a DNS ponechá hello místní adresu umožňují určit hello stejné adresy jako hello server DNS pro Azure Virtual network hello.

Hello počet síťových adaptérů závisí hello velikost, který jste zadali pro hello cílový virtuální počítač následujícím způsobem:

*   Pokud hello počet síťových adaptérů na zdrojovém počítači hello je menší než nebo rovna toohello počet adaptérů povoleno pro velikost cílového počítače hello pak bude mít cíl hello hello stejný počet adaptérů jako zdroj hello.
*   Pokud hello počet adaptérů pro hello zdrojový virtuální počítač překračuje počet hello povolený pro cílovou velikost hello pak maximální velikost cíle hello se použije.
* Například pokud má zdrojový počítač dva síťové adaptéry a velikost hello cílového počítače podporuje čtyři, bude mít hello cílový počítač dva adaptéry. Pokud má hello zdrojový počítač dva adaptéry, ale hello podporovaná velikost cíle podporuje pouze jeden, bude mít cílový počítač hello jenom jeden adaptér.
*   Pokud hello virtuální počítač více síťových adaptérů připojí se všechny toohello stejné síti.
*   Pokud hello virtuální počítač více síťových adaptérů, pak hello první z nich uvedené v seznamu hello stává hello výchozí síťový adaptér v hello virtuální počítač Azure.


## <a name="creating-a-recovery-plan"></a>Vytvoření plánu obnovení

Po povolení replikace pro virtuální počítače součást XenApp hello hello dalším krokem je toocreate plán obnovení.
Obnovení naplánujte skupiny společně virtuálních počítačů s podobné požadavky pro převzetí služeb při selhání a obnovení.  

**Kroky toocreate plán obnovení**

1. Přidání hello XenApp součásti virtuálních počítačů v hello plán obnovení.
2. Klikněte na tlačítko plány obnovení -> + plán obnovení. Zadejte název intuitivní pro plán obnovení hello.
3. U virtuálních počítačů VMware: Zvolit zdroj jako procesní server VMware, cíl jako Microsoft Azure a modelu nasazení Resource Manager a klikněte na možnost vybrat položky.
4. Pro virtuální počítače Hyper-V: Vyberte zdroj jako VMM server, jako Microsoft Azure a modelu nasazení Resource Manager jako cíl a klikněte na možnost vybrat položky a pak vyberte hello XenApp nasazení virtuálních počítačů.

### <a name="adding-virtual-machines-toofailover-groups"></a>Přidání skupin toofailover virtuální počítače

Plány obnovení může být vlastní tooadd převzetí služeb při selhání skupiny pro konkrétní spuštění pořadí, skripty nebo ruční akce. Následující skupiny Hello potřebovat plán obnovení přidané toohello toobe.

1. Group1 převzetí služeb při selhání: DNS AD
2. Skupina2 převzetí služeb při selhání: Virtuální počítače serveru SQL
2. Převzetí služeb při selhání skupina3: VDA hlavní bitové kopie virtuálního počítače
3. Skupina 4 převzetí služeb při selhání: Řadič doručování a virtuální počítače výkladní skříň serveru


### <a name="adding-scripts-toohello-recovery-plan"></a>Přidávání skriptů toohello plánu obnovení

Skripty, můžete spustit před nebo po určité skupiny v plánu obnovení. Ruční akce může být také být zahrnutý a prováděné během převzetí služeb při selhání.

plán obnovení přizpůsobené Hello vypadá hello níže:

1. Group1 převzetí služeb při selhání: DNS AD
2. Skupina2 převzetí služeb při selhání: Virtuální počítače serveru SQL
3. Převzetí služeb při selhání skupina3: VDA hlavní bitové kopie virtuálního počítače

   >[!NOTE]     
   >Kroky 4, 6 a 7 obsahující ruční nebo skript akce jsou příslušné tooonly místní XenApp > prostředí s MCS/systémy současné hodnoty katalogů.

4. Ruční nebo skript akce skupina 3: vypnutí hlavní počítač VDA hello hlavní VDA virtuálních počítačů, když převzal tooAzure bude ve spuštěném stavu. toocreate nové relace s více Připojeními katalogy pomocí Azure ARM hostování, hlavní hello VDA virtuální počítač je požadovaná toobe v zastaveném (de přidělené) stavu. Vypnutí hello virtuální počítač z portálu Azure.

5. Skupina 4 převzetí služeb při selhání: Řadič doručování a virtuální počítače výkladní skříň serveru
6. Skupina3 ruční nebo skript akce 1:

    ***Přidat připojení hostitele Azure RM***

    Vytvořte připojení hostitele Azure ARM v doručení řadiče počítač tooprovision nové relace s více Připojeními katalogů v Azure. Postupujte podle kroků hello, jak je popsáno v tomto [článku](https://www.citrix.com/blogs/2016/07/21/connecting-to-azure-resource-manager-in-xenapp-xendesktop/).

7. Skupina3 ruční nebo skript akce 2:

    ***Znovu vytvořit relace s více Připojeními katalogů v Azure***

    Hello existující relace s více Připojeními nebo systémy současné hodnoty klony v primární lokalitě hello nebudou replikované tooAzure. Je nutné toorecreate tyto klony pomocí hello replikovaný hlavní VDA a Azure ARM zřizování z řadiče doručení. Postupujte podle kroků hello, jak je popsáno v tomto [článku](https://www.citrix.com/blogs/2016/09/12/using-xenapp-xendesktop-in-azure-resource-manager/) toocreate relace s více Připojeními katalogizuje v Azure.

![Plán obnovení pro XenApp součásti](./media/site-recovery-citrix-xenapp-and-xendesktop/citrix-recoveryplan.png)


   >[!NOTE]
   >Můžete použít skripty v [umístění](https://github.com/Azure/azure-quickstart-templates/blob/>master/asr-automation-recovery/scripts) tooupdate hello DNS s hello převzetí služeb při selhání nové IP adresy z hello > virtuálních počítačů nebo tooattach Vyrovnávání zatížení na hello převzít služby při selhání virtuálního počítače, v případě potřeby.


## <a name="doing-a-test-failover"></a>Provádění testovací převzetí služeb

Postupujte podle [v tomto návodu](site-recovery-test-failover-to-azure.md) toodo testovací převzetí služeb.

![Plán obnovení](./media/site-recovery-citrix-xenapp-and-xendesktop/citrix-tfo.png)


## <a name="doing-a-failover"></a>Převzetím služeb

Postupujte podle [v tomto návodu](site-recovery-failover.md) při dělají převzetí služeb při selhání.

## <a name="next-steps"></a>Další kroky

Můžete [Další](https://aka.ms/citrix-xenapp-xendesktop-with-asr) o replikaci Citrix XenApp a XenDesktop nasazení v tomto dokumentu. Podívejte se na pokyny hello příliš[replikovat jiné aplikace](site-recovery-workload.md) pomocí Site Recovery.
