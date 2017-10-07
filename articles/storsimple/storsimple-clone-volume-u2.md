---
title: "aaaClone svazku řady StorSimple 8000 | Microsoft Docs"
description: "Popisuje typy hello různých klonování a kdy toouse a vysvětluje, jak můžete pomocí zálohovacího skladu tooclone svazku."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 070ac53e-7388-4c48-b8a5-8ed7f9108b2c
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 07/26/2017
ms.author: alkohli
ms.openlocfilehash: f457625d2e3aa173f7ccf26984e1902a64e33b5e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-tooclone-a-volume-update-2"></a>Použít tooclone služby StorSimple Manager hello svazek (Update 2)
[!INCLUDE [storsimple-version-selector-clone-volume](../../includes/storsimple-version-selector-clone-volume.md)]

## <a name="overview"></a>Přehled
Hello služby StorSimple Manager **zálohování katalogu** stránka zobrazuje všechny zálohovací sklady hello, které vytvářejí, když jsou provedeny ruční nebo automatické zálohy. Můžete použít tuto stránku toolist všechny hello zálohy pro zásady zálohování nebo svazek, vyberte nebo odstranit zálohy, nebo použít zálohování toorestore nebo klonování svazku.

![Stránka zálohování katalogu](./media/storsimple-clone-volume-u2/backupCatalog.png)  

Tento kurz popisuje, jak můžete pomocí zálohovacího skladu tooclone svazku. Také vysvětluje hello rozdíl mezi *přechodný* a *trvalé* provede klonování.

> [!NOTE]
> Místně vázaný svazek, budou se klonovat jako vrstvený svazek. Pokud třeba hello místně vázaný toobe klonovaný svazku, můžete převést hello klon tooa místně vázaný svazek po úspěšném dokončení operace klonování hello. Pro informace o převodu tooa vrstvený svazek, který je místně připnutý svazku, přejděte příliš[změnit typ svazku hello](storsimple-manage-volumes-u2.md#change-the-volume-type).
> 
> Pokud se pokusíte tooconvert, který svazek klonovaný ze vrstvené toolocally připnutý okamžitě po klonování (Pokud je stále přechodný klon), převod hello se nezdaří s hello následující chybová zpráva:
> 
> `Unable toomodify hello usage type for volume {0}. This can happen if hello volume being modified is a transient clone and hasn’t been made permanent. Take a cloud snapshot of this volume and then retry hello modify operation.` 
> 
> Tato chyba byl přijat pouze v případě, že se klonování na tooa jiné zařízení. Můžete úspěšně převést toolocally svazku hello připnutý Pokud převedete hello přechodný klon tooa trvalé klonování. tooconvert hello přechodný klon tooa trvalé klonovat, pořízení snímku cloudu ho.
> 
> 

## <a name="create-a-clone-of-a-volume"></a>Vytvořit klon svazku
Můžete vytvořit klon na hello stejného zařízení, i virtuální počítač nebo jiné zařízení pomocí místní nebo cloudové snímku.

#### <a name="tooclone-a-volume"></a>tooclone svazku
1. Na stránce služby StorSimple Manager hello, klikněte na hello **katalog zálohování** a vyberte zálohovacího skladu.
2. Rozbalte hello zálohovacího skladu tooview hello přidružené svazky. Klikněte a vyberte svazek z hello zálohovacího skladu.
   
     ![Klonování svazku](./media/storsimple-clone-volume-u2/CloneVol.png) 
3. Klikněte na tlačítko **klon** toobegin klonování hello vybraný svazek.
4. V Průvodci hello klonování svazku v části **zadejte název a umístění**:
   
   1. Určete cílové zařízení. Toto je hello umístění, kde bude vytvořen hello klonování. Můžete vybrat hello stejné zařízení nebo zadejte jiné zařízení. Pokud zvolíte svazku přidruženém k ostatní poskytovatele cloudových služeb (ne Azure), hello rozevíracího seznamu pro hello cílové zařízení se zobrazí pouze fyzických zařízení. Nelze klonovat svazku přidruženém k ostatní poskytovatele cloudových služeb na virtuální zařízení.
      
      > [!NOTE]
      > Ujistěte se, že požadovaná pro klon hello kapacita hello je nižší než hello kapacity, které jsou k dispozici na cílovém zařízení hello.
      > 
      > 
   2. Zadejte název jedinečné svazku pro vaše klon. Hello název musí obsahovat 3 až 127 znaků. 
      
      > [!NOTE]
      > Hello **klonování svazku jako** pole bude **nastavování** i v případě, že se klonování místně vázaný svazek. Nelze změnit toto nastavení; ale pokud klonovaný svazku toobe místně vázaný také potřebovat hello, můžete převést hello klon tooa místně vázaný svazek po úspěšně vytvořit klon hello. Pro informace o převodu tooa vrstvený svazek, který je místně připnutý svazku, přejděte příliš[změnit typ svazku hello](storsimple-manage-volumes-u2.md#change-the-volume-type).
      > 
      > 
      
        ![Průvodce klon 1](./media/storsimple-clone-volume-u2/clone1.png) 
   3. Klikněte na ikonu šipky hello ![ikona šipky](./media/storsimple-clone-volume-u2/HCS_ArrowIcon.png) tooproceed toohello další stránku.
5. V části **zadejte hostitelů, které můžete použít tento svazek**:
   
   1. Zadejte záznam řízení přístupu (ACR) pro klon hello. Můžete přidat nové ACR nebo vyberte možnost ze seznamu existující hello.
      
        ![Průvodce klon 2](./media/storsimple-clone-volume-u2/clone2.png) 
   2. Klikněte na ikonu zaškrtnutí hello ![ikona zaškrtnutí](./media/storsimple-clone-volume-u2/HCS_CheckIcon.png)operace toocomplete hello.
6. Úloha klonování bude inicializován a budete informováni při klonování hello je úspěšně vytvořen. Klikněte na tlačítko **zobrazit úlohy** toomonitor hello klon úlohy na hello **úlohy** stránky. Zobrazí se následující zpráva po dokončení úlohy klon hello hello:
   
    ![Zpráva klonování](./media/storsimple-clone-volume-u2/CloneMsg.png) 
7. Po hello dokončení úlohy klonu:
   
   1. Přejděte toohello **zařízení** stránku a vyberte hello **kontejnery svazků** kartě. 
   2. Vyberte kontejner hello svazek, který je přidružen hello zdrojový svazek, který jste naklonovali. V seznamu hello svazků měli byste vidět hello klonování, kterou jste právě vytvořili.

> [!NOTE]
> Monitorování a výchozí zálohování jsou automaticky zakázán na klonovaný svazku.
> 
> 

Klon, který je vytvořen tímto způsobem je přechodný klonu. Další informace o typech klonování najdete v tématu [přechodný oproti trvalé klony](#transient-vs-permanent-clones).

Tenhle klon. je teď regulární svazek a všechny operace, které je možné ve svazku, bude k dispozici pro klonování hello. Budete potřebovat tooconfigure tento svazek pro všechny zálohy.

## <a name="transient-vs-permanent-clones"></a>Pouze dočasné a trvalé klony
Přechodný klony vytvoří jenom v případě, že se klonování tooa jiné zařízení. Svazek konkrétní z jiného zařízení zálohovacího skladu tooa spravuje hello StorSimple Manager může klonovat. Hello přechodný klonování bude mají data toohello odkazy v původním svazku hello a použije tento tooread dat a zapsat místně na cílovém zařízení hello. 

Po provedení cloudový snímek přechodný klon hello výsledné klonování bude *trvalé* klonování. Během tohoto procesu kopii dat hello se vytvoří v cloudu hello a hello toocopy času, které tato data je určena velikostí hello hello dat a hello Azure latenci (Toto je kopie Azure do Azure). Tento proces může trvat tooweeks dnů. klon přechodný Hello tímto způsobem se změní na trvalé klonování a nemá žádné odkazy toohello původní svazek data, která byla naklonována ze. 

## <a name="scenarios-for-transient-and-permanent-clones"></a>Scénáře pro přechodný a trvalé klony
Hello následující oddíly popisují příklad situacích, ve kterých může být použit klony přechodný a trvalé.

### <a name="item-level-recovery-with-a-transient-clone"></a>Obnovení na úrovni položek s přechodný klon
Je nutné toorecover jeden rok starý soubor s Microsoft PowerPoint. Váš správce IT identifikuje hello konkrétní zálohování z toto časové období a pak filtry hello svazku. Správce Hello pak provede klonování svazku hello, vyhledá hello soubor, který hledáte a poskytuje ji tooyou. V tomto scénáři se používá přechodný klonu. 

![Dostupné video](./media/storsimple-clone-volume-u2/Video_icon.png)**Dostupné video**

Klikněte na tlačítko toowatch video, které ukazuje, jak můžete použít klonu hello a obnovení funkcí v zařízení StorSimple toorecover odstranit soubory, [zde](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).

### <a name="testing-in-hello-production-environment-with-a-permanent-clone"></a>Testování hello produkčního prostředí s trvalé klon
Je nutné tooverify chyby testování v produkčním prostředí hello. Vytvořit klon svazku hello hello produkčního prostředí a pak proveďte cloudový snímek tento toocreate klonování svazku nezávislé klonovaný. V tomto scénáři se používá trvalá klonu.  

## <a name="next-steps"></a>Další kroky
* Zjistěte, jak příliš[obnovit svazek StorSimple ze zálohovacího skladu](storsimple-restore-from-backup-set-u2.md).
* Zjistěte, jak příliš[použití hello tooadminister služby StorSimple Manager zařízení StorSimple](storsimple-manager-service-administration.md).

