---
title: "aaaHow tooReprotect z převzal virtuální počítače Azure back tooprimary oblast Azure | Microsoft Docs"
description: "Po převzetí služeb při selhání virtuálních počítačů z jedné oblasti Azure tooanother můžete použít Azure Site Recovery tooprotect hello počítačů v opačném směru. Zjistěte, jak hello kroky toodo opětovné ochrany před převzetí služeb při selhání znovu."
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
ms.openlocfilehash: 991c7ee8f489e84c250230bf73f3e99015c5f051
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="reprotect-from-failed-over-azure-region-back-tooprimary-region"></a>Opětovné ochrany z převzal oblast Azure back tooprimary oblast



>[!NOTE]
>
> Replikace obnovení lokality pro virtuální počítače Azure je aktuálně ve verzi preview.


## <a name="overview"></a>Přehled
Pokud jste [převzetí služeb při selhání](site-recovery-failover.md) hello virtuálních počítačů z jedné oblasti Azure tooanother hello virtuální počítače jsou v nechráněném stavu. Pokud chcete toobring je zpět toohello primární oblasti, budete potřebovat toofirst chránit hello virtuální počítače a potom převzetí služeb při selhání znovu. Není žádný rozdíl mezi postupy můžete převzetí služeb při selhání v jednom směru nebo jiné. Podobně konečná povolení ochrany hello virtuálních počítačů, není žádný rozdíl mezi hello opětovné ochrany post převzetí služeb při selhání a navrácení služeb po obnovení post.
pracovní postupy hello tooexplain opětovné ochrany a tooavoid nejasnostem, použijeme hello primární lokalita hello chráněných počítačů jako oblast východní Asie a obnovení lokality hello hello počítačů jako oblast jihovýchodní Asie. Během převzetí služeb při selhání které budete převzetí služeb při selhání hello virtuální počítače toohello jihovýchodní Asie oblast. Než budete navrácení služeb po obnovení je potřeba tooreprotect hello virtuální počítače z jihovýchodní Asie back tooEast Asie. Tento článek popisuje kroky hello o tooreprotect.

> [!WARNING]
> Pokud máte [dokončit migraci](site-recovery-migrate-to-azure.md#what-do-we-mean-by-migration)hello přesunutý virtuální počítač tooanother prostředek skupiny nebo odstraněné hello virtuální počítač Azure, nemůžete poté navrácení služeb po obnovení.

Po dokončení opětovné ochrany a hello chráněné virtuální počítače jsou replikace, můžete zahájit převzetí služeb při selhání na virtuální počítače toobring hello je zpět tooEast Asie.

POST dotazy nebo připomínky můžete na konci hello tohoto článku nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="prerequisites"></a>Požadavky
1. Hello virtuální počítače by byly potvrzeny.
2. Hello cílovou lokalitu – v tomto případě oblast východní Asie Azure hello by měly být dostupné a musí být schopný tooaccess nebo vytvořit nové prostředky v této oblasti.

## <a name="steps-tooreprotect"></a>Kroky tooreprotect

Následující jsou hello kroky tooreprotect virtuálního počítače pomocí výchozího nastavení hello.

1. V **trezoru** > **replikované položky**, klikněte pravým tlačítkem na hello virtuální počítač, který při selhání a pak vyberte **znovu nastavit ochranu**. Můžete také kliknout na hello počítače a vyberte možnost **znovu nastavit ochranu** z hello příkazová tlačítka.

![Klikněte pravým tlačítkem na tooreprotect](./media/site-recovery-how-to-reprotect-azure-to-azure/reprotect.png)

2. V okně hello, Všimněte si, že směr hello ochrany, **jihovýchodní Asie tooEast Asie**, je již vybrána.

![Znovu nastavte ochranu okno](./media/site-recovery-how-to-reprotect-azure-to-azure/reprotectblade.png)

3. Zkontrolujte hello **prostředků skupiny, sítě, úložiště a dostupnost sady** informace a klikněte na tlačítko OK. Pokud jsou všechny prostředky označené (Nový), bude jako součást hello nastavte znovu vytvořen.

To bude aktivační události úlohy znovu nastavte ochranu úlohu, která se nejdřív počáteční hodnoty cílové lokalitě hello (v tomto případě SEA) s hello nejnovější data, a který dokončí, replikují se tak hello rozdílů před můžete převzetí služeb při selhání zpět tooSoutheast Asie.

### <a name="reprotect-customization"></a>Znovu nastavte ochranu přizpůsobení
Pokud chcete toochoose hello síť účet nebo hello úložiště extrakce během znovu nastavte ochranu, můžete toho dosáhnout pomocí hello přizpůsobit možnosti uvedené v okně hello opětovné ochrany.

![Přizpůsobení možnost](./media/site-recovery-how-to-reprotect-azure-to-azure/customize.png)

Můžete přizpůsobit hello následující vlastnosti he cílového virtuálního počítače během opětovné ochrany.

![Přizpůsobení okna](./media/site-recovery-how-to-reprotect-azure-to-azure/customizeblade.png)

|Vlastnost |Poznámky  |
|---------|---------|
|Cílová skupina prostředků     | Můžete toochange hello cílová skupina prostředků ve kterém se vytvoří tý virtuálního počítače. V rámci hello opětovné ochrany budou odstraněny hello cílového virtuálního počítače, proto můžete novou skupinu prostředků, ve kterém můžete vytvořit hello virtuálních počítačů post převzetí služeb při selhání         |
|Cílová virtuální síť     | Sítě, nelze změnit během opětovné ochrany hello. toochange hello síť, vrátit hello mapování sítě.         |
|Cílové úložiště     | Účet úložiště hello toowhich hello virtuální počítač bude vytvořený post převzetí služeb při selhání, můžete změnit.         |
|Úložiště mezipaměti     | Můžete zadat účet úložiště mezipaměti, který se použije během replikace. Pokud přejdete hello výchozí hodnoty, nový účet úložiště mezipaměti se mají vytvořit, pokud ještě neexistuje.         |
|Skupina dostupnosti     |Pokud virtuální počítač hello ve východní Asie je součástí skupiny dostupnosti, můžete vybrat sadu dostupnosti pro hello cílový virtuální počítač v jihovýchodní Asie. Výchozí nastavení bude najít hello stávající sadu dostupnosti SEA a zkuste toouse ho. Během přizpůsobení můžete určit úplně novou sadu AV.         |


### <a name="what-happens-during-reprotect"></a>Co se stane při opětovné ochrany?

Stejně jako po hello povolit ochranu, jsou následující hello rušivé vlivy, které vytvářeny Pokud použijete výchozí nastavení hello.
1. V oblasti východní Asie hello získá vytvořit účet úložiště mezipaměti.
2. Pokud hello cílový účet úložiště (hello původní účtu úložiště hello virtuálních počítačů jihovýchodní Asie) neexistuje, vytvoří se nový. Název Hello je účet úložiště hello východní Asie virtuálního počítače na konci "Automatické obnovení systému".
3. Pokud sada cíl AV hello neexistuje a výchozí nastavení hello zjistit, že potřebuje toocreate nastavit nové AV, pak bude vytvořen jako součást hello znovu nastavte ochranu úlohy. Pokud jste upravili hello znovu nastavte ochranu a pak se použije sadu AV hello vybrané.
4.

Hello následují hello seznam kroků, které dojít v případě, že spustíte úlohu opětovné ochrany. Toto je straně hello případu hello cíle, které virtuální počítač existuje.

1. Hello požadované rušivé vlivy jsou vytvořené jako součást opětovné ochrany. Pokud již existují, pak jsou opakovaně využívány.
2. Hello cílový straně (jihovýchodní Asie) virtuální počítač nejdřív vypnutý, pokud je spuštěn.
3. Azure Site Recovery zkopírován Hello cíl straně disku virtuálního počítače do kontejneru jako objekt blob počáteční hodnoty.
4. Hello cílový straně virtuální počítač se odstraní.
5. hello aktuální zdroj straně (východní Asie) virtuálního počítače tooreplicate používá objekt blob Hello počáteční hodnoty. Tím se zajistí, že jsou replikovány pouze rozdíly.
6. se synchronizují Hello hlavní změny mezi hello zdrojový disk a objektů blob hello počáteční hodnoty. Tato akce může zabrat některé toocomplete čas.
7. Jakmile znovu nastavte ochranu hello dokončí úlohu, začne hello rozdílová replikace, který vytváří bod obnovení podle zásad hello.

> [!NOTE]
> Nelze chránit na úrovni plánu obnovení. Můžete pouze nastavte znovu v úrovni virtuálního počítače.

Po znovu nastavte ochranu hello úspěšné, hello virtuální počítač přejde chráněném stavu.

## <a name="next-steps"></a>Další kroky

Po zadání chráněném stavu hello virtuálního počítače můžete spustit převzetí služeb při selhání. Hello převzetí služeb při selhání vypnout hello virtuální počítač v oblasti východní Asie Azure a pak vytvoříte a hello jihovýchodní Asie oblast virtuální počítač spustit. Proto je malý výpadku pro aplikaci hello. Ano vyberte hello čas pro převzetí služeb při selhání, pokud vaše aplikace může tolerovat s prodlevou. Doporučujeme nejprve testovací převzetí služeb při selhání hello virtuálního počítače toomake se, že ho na blížící se správně, před zahájením převzetí služeb při selhání.

-   [Kroky tooinitiate testovací převzetí služeb při selhání hello virtuálního počítače](site-recovery-test-failover-to-azure.md)

-   [Kroky tooinitiate převzetí služeb při selhání hello virtuálního počítače](site-recovery-failover.md)
