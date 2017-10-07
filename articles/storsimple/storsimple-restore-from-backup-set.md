---
title: "aaaRestore svazek StorSimple ze zálohy | Microsoft Docs"
description: "Vysvětluje, jak toouse hello toorestore stránky zálohování katalogu služby StorSimple Manager svazek StorSimple ze zálohovacího skladu."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: b979782e-3184-4465-ad5f-e4516a5885d2
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: e0efa74b14603be41af0cfc5400de3c39ab8824e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="restore-a-storsimple-volume-from-a-backup-set"></a>Obnovit svazek StorSimple ze zálohovacího skladu
[!INCLUDE [storsimple-version-selector-restore-from-backup](../../includes/storsimple-version-selector-restore-from-backup.md)]

## <a name="overview"></a>Přehled
Hello **zálohování katalogu** stránka zobrazuje všechny zálohovací sklady hello, které vytvářejí, když jsou provedeny ruční nebo automatické zálohy. Můžete použít tuto stránku toolist všechny hello zálohy pro zásady zálohování nebo svazek, vyberte nebo odstranit zálohy, nebo použít zálohování toorestore nebo klonování svazku.

 ![Zálohování stránky katalogu](./media/storsimple-restore-from-backup-set/HCS_BackupCatalog.png)

Tento kurz vysvětluje, jak toouse hello **zálohování katalogu** stránky toorestore svazek v zařízení ze zálohovacího skladu.

## <a name="how-toouse-hello-backup-catalog"></a>Jak toouse hello zálohování katalogu
Hello **zálohování katalogu** stránka obsahuje dotaz, který vám pomůže toonarrow výběr zálohovacího skladu. Můžete filtrovat hello zálohování sad, které jsou načteny podle hello následující parametry:

* **Zařízení** – hello zařízení, na které hello zálohovací sklad vytvořen.
* **Zásady zálohování** nebo **svazku** – hello zásady zálohování nebo svazku přidruženém k tohoto zálohovacího skladu.
* **Z** a **k** – hello rozsah data a času v okamžiku vytvoření zálohovacího skladu hello.

Hello filtrované zálohovací sklady jsou pak v tabulce podle hello následující atributy:

* **Název** – hello název zásady zálohování hello nebo svazku přidruženém k hello zálohovacího skladu.
* **Velikost** – hello skutečná velikost hello zálohovacího skladu.
* **Vytvořit v** – hello datum a čas, kdy byla vytvořena hello zálohy. 
* **Typ** – zálohovací sklady může být místní snímky nebo cloudových snímků. Místní snímek je zálohování všech dat uložených místně na zařízení hello svazku, zatímco cloudový snímek odkazuje toohello zálohování svazku dat umístěných v cloudu hello. Místní snímky poskytují rychlejší přístup, že jsou pro záleží na odolnosti dat zvolena cloudových snímků.
* **Zahájit** – hello zálohy lze inicializovat automaticky podle plánu tooa nebo ručně uživatelem. (Můžete použít zálohování tooschedule zásady zálohování. Alternativně můžete použít hello **provést zálohování** možnost tootake interaktivní zálohování.)

## <a name="how-toorestore-your-storsimple-volume-from-a-backup"></a>Jak toorestore svazek StorSimple ze zálohy
Můžete použít hello **zálohování katalogu** stránka toorestore svazek StorSimple ze konkrétní zálohy. 

> [!WARNING]
> Obnovení ze zálohy nahradí existující svazky hello ze zálohy hello. To může způsobit ztrátu hello všechna data, která byla zapsána po hello zálohy.
> 
> 

Než zahájíte obnovení ve svazku, zajistěte, aby byl hello svazek offline. Budete potřebovat tootake hello svazku do offline režimu na hostiteli hello nejprve a pak hello zařízení. Postupujte podle kroků hello v [do offline režimu svazku](storsimple-manage-volumes.md#take-a-volume-offline). Proveďte následující kroky toorestore svazek ze zálohovacího skladu hello.

### <a name="toorestore-from-a-backup-set"></a>toorestore ze zálohovacího skladu
1. Na stránce služby StorSimple Manager hello, klikněte na hello **katalog zálohování** kartě.
   
    ![Zálohování katalogu](./media/storsimple-restore-from-backup-set/HCS_Restore.png)
2. Vyberte zálohovací sklad následujícím způsobem:
   
   1. Vyberte příslušné zařízení hello.
   2. V rozevíracím seznamu hello výběru hello svazek nebo zálohování zásady zálohování hello chcete tooselect.
   3. Zadejte časové rozmezí hello.
   4. Klikněte na ikonu zaškrtnutí hello ![ikona zaškrtnutí](./media/storsimple-restore-from-backup-set/HCS_CheckIcon.png) tooexecute tento dotaz.
      
      Hello zálohování přidružená k zásadě hello vybraný svazek nebo zálohování by se zobrazit v seznamu hello zálohovací sklady.
3. Rozbalte hello zálohovacího skladu tooview hello přidružené svazky. Tyto svazky musí být převedeno do režimu offline v hostiteli hello a zařízení, než bude možné obnovit. Postupujte podle kroků hello v [do offline režimu svazku](storsimple-manage-volumes.md#take-a-volume-offline).
   
   > [!IMPORTANT]
   > Ujistěte se, že jste provedli hello svazky do offline režimu na hostiteli hello nejprve, před provedením hello svazky do režimu offline v zařízení hello. Pokud neprovedete offline hello svazky na hostiteli hello, může potenciálně vést k poškození toodata.
   > 
   > 
4. Vyberte zálohovacího skladu. Klikněte na tlačítko **obnovení** v hello dolní části stránky hello.
5. Zobrazí se výzva k potvrzení. 
   
    ![Stránka potvrzení](./media/storsimple-restore-from-backup-set/HCS_ConfirmRestore.png)
6. Zkontrolujte informace hello obnovení a klikněte na ikonu zaškrtnutí hello ![ikona zaškrtnutí](./media/storsimple-restore-from-backup-set/HCS_CheckIcon.png). Tím zahájíte obnovení úlohu, která můžete zobrazit pomocí přístupu k hello **úlohy** stránky. 
7. Po dokončení obnovení hello můžete ověřit, že hello obsah svazků jsou nahrazovány svazků ze zálohy hello.

![Dostupné video](./media/storsimple-restore-from-backup-set/Video_icon.png)**Dostupné video**

Klikněte na tlačítko toowatch video, které ukazuje, jak můžete použít klonu hello a obnovení funkcí v zařízení StorSimple toorecover odstranit soubory, [zde](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).

## <a name="next-steps"></a>Další kroky
* Zjistěte, jak příliš[svazky spravovat zařízení StorSimple](storsimple-manage-volumes.md).
* Zjistěte, jak příliš[použití hello tooadminister služby StorSimple Manager zařízení StorSimple](storsimple-manager-service-administration.md).

