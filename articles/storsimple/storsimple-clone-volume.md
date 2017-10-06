---
title: aaaClone svazek StorSimple | Microsoft Docs
description: "Popisuje typy hello různých klonování a kdy toouse a vysvětluje, jak můžete pomocí zálohovacího skladu tooclone svazku."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: b5d615f2-02a7-4222-9867-6c0385ce748c
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: e98d28db1abeb515ba78ab5860e7c5eba0dfcbb5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-tooclone-a-volume"></a>Použít tooclone služby StorSimple Manager hello svazku
[!INCLUDE [storsimple-version-selector-clone-volume](../../includes/storsimple-version-selector-clone-volume.md)]

## <a name="overview"></a>Přehled
Hello služby StorSimple Manager **zálohování katalogu** stránka zobrazuje všechny zálohovací sklady hello, které vytvářejí, když jsou provedeny ruční nebo automatické zálohy. Můžete použít tuto stránku toolist všechny hello zálohy pro zásady zálohování nebo svazek, vyberte nebo odstranit zálohy, nebo použít zálohování toorestore nebo klonování svazku.

![Stránka zálohování katalogu](./media/storsimple-clone-volume/HCS_BackupCatalog.png)  

Tento kurz popisuje, jak můžete pomocí zálohovacího skladu tooclone svazku. Také vysvětluje hello rozdíl mezi *přechodný* a *trvalé* provede klonování. 

## <a name="create-a-clone-of-a-volume"></a>Vytvořit klon svazku
Můžete vytvořit klon na hello stejného zařízení, jiná zařízení nebo i virtuálního počítače pomocí místní nebo cloudový snímek.

#### <a name="tooclone-a-volume"></a>tooclone svazku
1. Na stránce služby StorSimple Manager hello, klikněte na hello **katalog zálohování** a vyberte zálohovacího skladu.
2. Rozbalte hello zálohovacího skladu tooview hello přidružené svazky. Klikněte a vyberte svazek z hello zálohovacího skladu.
   
     ![Klonování svazku](./media/storsimple-clone-volume/HCS_Clone.png) 
3. Klikněte na tlačítko **klon** toobegin klonování hello vybraný svazek.
4. V Průvodci hello klonování svazku v části **zadejte název a umístění**:
   
   1. Určete cílové zařízení. Toto je hello umístění, kde bude vytvořen hello klonování. Můžete vybrat hello stejné zařízení nebo zadejte jiné zařízení. Pokud zvolíte svazku přidruženém k ostatní poskytovatele cloudových služeb (ne Azure), hello rozevíracího seznamu pro hello cílové zařízení se zobrazí pouze fyzických zařízení. Nelze klonovat svazku přidruženém k ostatní poskytovatele cloudových služeb na virtuální zařízení.
      
      > [!NOTE]
      > Ujistěte se, že požadovaná pro klon hello kapacita hello je nižší než hello kapacity, které jsou k dispozici na cílovém zařízení hello.
      > 
      > 
   2. Zadejte název jedinečné svazku pro vaše klon. Hello název musí obsahovat 3 až 127 znaků.
   3. Klikněte na ikonu šipky hello ![ikona šipky](./media/storsimple-clone-volume/HCS_ArrowIcon.png) tooproceed toohello další stránku.
5. V části **zadejte hostitelů, které můžete použít tento svazek**:
   
   1. Zadejte záznam řízení přístupu (ACR) pro klon hello. Můžete přidat nové ACR nebo vyberte možnost ze seznamu existující hello.
   2. Klikněte na ikonu zaškrtnutí hello ![ikona zaškrtnutí](./media/storsimple-clone-volume/HCS_CheckIcon.png)operace toocomplete hello.
6. Úloha klonování bude inicializován a budete informováni při klonování hello je úspěšně vytvořen. Klikněte na tlačítko **zobrazit úlohy** toomonitor hello klon úlohy na hello **úlohy** stránky.
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
Přechodný a trvalé klony vytvoří jenom v případě, že se klonování na tooa jiné zařízení. Může klonovat konkrétní svazku z jiného zařízení tooa zálohovacího skladu. Klon vytvořené v tomto případě je *přechodný* klonování. Hello přechodný klonování bude mít odkazy toohello původního svazku a bude používat tento svazek tooread při zápisu místně. 

Po provedení cloudový snímek přechodný klon hello výsledné klonování bude *trvalé* klonování. Hello trvalé klon je nezávislá a nemá žádné odkazy toohello původní svazek, který byl naklonována ze.  

## <a name="scenarios-for-transient-and-permanent-clones"></a>Scénáře pro přechodný a trvalé klony
Hello následující oddíly popisují příklad situacích, ve kterých může být použit klony přechodný a trvalé.

### <a name="item-level-recovery-with-a-transient-clone"></a>Obnovení na úrovni položek s přechodný klon
Je nutné toorecover jeden rok starý soubor s Microsoft PowerPoint. Váš správce IT identifikuje hello konkrétní zálohování z toto časové období a pak filtry hello svazku. Správce Hello pak provede klonování svazku hello, vyhledá hello soubor, který hledáte a poskytuje ji tooyou. V tomto scénáři se používá přechodný klonu. 

![Dostupné video](./media/storsimple-clone-volume/Video_icon.png)**Dostupné video**

Klikněte na tlačítko toowatch video, které ukazuje, jak můžete použít klonu hello a obnovení funkcí v zařízení StorSimple toorecover odstranit soubory, [zde](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).

### <a name="testing-in-hello-production-environment-with-a-permanent-clone"></a>Testování hello produkčního prostředí s trvalé klon
Je nutné tooverify chyby testování v produkčním prostředí hello. Při vytváření klon hello svazku v provozním prostředí hello pořízení snímku cloudu Tenhle klon. Hello klonovaný svazek je nyní nezávislé. V tomto scénáři se používá trvalá klonu.

## <a name="next-steps"></a>Další kroky
* Zjistěte, jak příliš[obnovit svazek StorSimple ze zálohovacího skladu](storsimple-restore-from-backup-set.md).
* Zjistěte, jak příliš[použití hello tooadminister služby StorSimple Manager zařízení StorSimple](storsimple-manager-service-administration.md).

