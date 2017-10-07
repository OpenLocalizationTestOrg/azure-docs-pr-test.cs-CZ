---
title: "aaaManage katalog záloh StorSimple | Microsoft Docs"
description: "Vysvětluje, jak toouse hello toolist stránka služby zálohování katalogu Správce zařízení StorSimple, vyberte a odstranit zálohovací sklady."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/29/2017
ms.author: alkohli
ms.openlocfilehash: e464609e74409a06a198790719abd82ed03c03d9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-toomanage-your-backup-catalog"></a>Použít toomanage služby StorSimple Manager zařízení hello katalog záloh
## <a name="overview"></a>Přehled
Hello služby StorSimple Manager zařízení **zálohování katalogu** zobrazuje všechny zálohovací sklady hello, které vytvářejí, když jsou provedeny ruční nebo plánované zálohy. Můžete použít tuto stránku toolist všechny hello zálohy pro zásady zálohování nebo svazek, vyberte nebo odstranit zálohy, nebo použít zálohování toorestore nebo klonování svazku.

Tento kurz popisuje, jak toolist, vyberte a delete zálohovacího skladu. toolearn jak toorestore zařízení ze zálohy, přejděte příliš[obnovit vaše zařízení ze zálohovacího skladu](storsimple-8000-restore-from-backup-set-u2.md). jak tooclone svazku, přejděte příliš toolearn[klonovat svazek StorSimple](storsimple-8000-clone-volume-u2.md).

![Zálohování katalogu](./media/storsimple-8000-manage-backup-catalog/bucatalog.png) 

Hello **zálohování katalogu** okno obsahuje dotaz toonarrow výběr zálohovacího skladu. Můžete filtrovat hello zálohovací sklady, které jsou načteny podle hello následující parametry:

* **Zařízení** – hello zařízení, na které hello zálohovací sklad vytvořen.
* **Zásady zálohování nebo svazku** – hello zásady zálohování nebo svazku přidruženém k tohoto zálohovacího skladu.
* **Od a do** – hello rozsah data a času v okamžiku vytvoření zálohovacího skladu hello.

Hello filtrované zálohovací sklady jsou pak v tabulce podle hello následující atributy:

* **Název** – hello název zásady zálohování hello nebo svazku přidruženém k hello zálohovacího skladu.
* **Velikost** – hello skutečná velikost hello zálohovacího skladu.
* **Vytvořit na** – hello datum a čas, kdy byla vytvořena hello zálohy. 
* **Typ** – zálohovací sklady může být místní snímky nebo cloudových snímků. Místní snímek je zálohování všech dat uložených místně na zařízení hello svazku, zatímco cloudový snímek odkazuje toohello zálohování svazku dat umístěných v cloudu hello. Místní snímky poskytují rychlejší přístup, že jsou pro záleží na odolnosti dat zvolena cloudových snímků.
* **Iniciováno** – hello zálohy lze inicializovat automaticky podle plánu nebo ručně uživatelem. Můžete použít zálohování tooschedule zásady zálohování. Alternativně můžete použít hello **provést zálohování** tootake možnost ručního zálohování.

## <a name="list-backup-sets-for-a-backup-policy"></a>Zálohování seznamu nastaví pro zásady zálohování
Proveďte následující kroky toolist hello všechny hello zálohy pro zásady zálohování.

#### <a name="toolist-backup-sets"></a>zálohovací sklady toolist
1. Přejděte služby StorSimple Manager zařízení tooyour a klikněte na tlačítko **katalog zálohování**.

2. Filtrovat hello výběr následujícím způsobem:
   
   1. Zadejte časové rozmezí hello.
   2. Vyberte příslušné zařízení hello.
   3. Filtrovat podle **zálohování zásad** tooview hello odpovídající hello zálohy.
   3. Z rozevíracího seznamu hello zásady zálohování vyberte **všechny** tooview všechny hello zálohy na hello vybraná zařízení.
   4. Klikněte na tlačítko **použít** tooexecute tento dotaz.
      
      Hello zálohy přidružené hello vybrané zásady zálohování by se zobrazit v seznamu hello zálohovací sklady.

      ![Přejděte toobackup katalogu](./media/storsimple-8000-manage-backup-catalog/bucatalog1.png)

## <a name="select-a-backup-set"></a>Vyberte zálohovací sklad
Proveďte následující kroky tooselect zálohu nastavení pro zásadu svazek nebo zálohování hello.

#### <a name="tooselect-a-backup-set"></a>tooselect zálohovacího skladu
1. Přejděte služby StorSimple Manager zařízení tooyour a klikněte na tlačítko **katalog zálohování**.
2. Filtrovat hello výběr následujícím způsobem:
   
   1. Zadejte časové rozmezí hello. 
   2. Vyberte příslušné zařízení hello. 
   3. Filtrovat podle zásad svazku nebo zálohu pro zálohování hello chcete tooselect.
   4. Klikněte na tlačítko **použít** tooexecute tento dotaz.
      
      Hello zálohování přidružená k zásadě hello vybraný svazek nebo zálohování by se zobrazit v seznamu hello zálohovací sklady.

      ![Přejděte toobackup katalogu](./media/storsimple-8000-manage-backup-catalog/bucatalog1.png)

3. Vyberte a rozbalte zálohovacího skladu. Nyní můžete vidět hello zálohovací sklady členěné podle hello svazky, které obsahuje. Hello **obnovení** a **odstranit** možnosti jsou dostupné prostřednictvím hello kontextovou nabídku (klikněte pravým tlačítkem) hello zálohovacího skladu. Můžete provést některý z těchto akcí na hello zálohovacího skladu, který jste vybrali.

    ![Přejděte toobackup katalogu](./media/storsimple-8000-manage-backup-catalog/bucatalog2.png)

## <a name="delete-a-backup-set"></a>Odstranit zálohovacího skladu
Odstraňte zálohy, když už nechcete tooretain hello data s ním spojená. Proveďte následující kroky toodelete zálohovacího skladu hello.

#### <a name="toodelete-a-backup-set"></a>toodelete zálohovacího skladu
 Přejděte služby StorSimple Manager zařízení tooyour a klikněte na tlačítko **katalog zálohování**.
2. Filtrovat hello výběr následujícím způsobem:
   
   1. Zadejte časové rozmezí hello. 
   2. Vyberte příslušné zařízení hello. 
   3. Filtrovat podle zásad svazku nebo zálohu pro zálohování hello chcete tooselect.
   4. Klikněte na tlačítko **použít** tooexecute tento dotaz.
      
      Hello zálohování přidružená k zásadě hello vybraný svazek nebo zálohování by se zobrazit v seznamu hello zálohovací sklady.

      ![Přejděte toobackup katalogu](./media/storsimple-8000-manage-backup-catalog/bucatalog1.png)

3. Vyberte a rozbalte zálohovacího skladu. Nyní můžete vidět hello zálohovací sklady členěné podle hello svazky, které obsahuje. Hello **obnovení** a **odstranit** možnosti jsou dostupné prostřednictvím hello kontextovou nabídku (klikněte pravým tlačítkem) hello zálohovacího skladu. Klikněte pravým tlačítkem na vybrané hello zálohovacího skladu a hello místní nabídce vyberte **odstranit**.

    ![Přejděte toobackup katalogu](./media/storsimple-8000-manage-backup-catalog/bucatalog3.png)

4. Po zobrazení výzvy k potvrzení, zkontrolujte hello zobrazí informace a klikněte na tlačítko **odstranit**. vybranou zálohu Hello je trvale odstraněn.

    ![Přejděte toobackup katalogu](./media/storsimple-8000-manage-backup-catalog/bucatalog4.png)  

5. Při odstranění hello právě probíhá a při úspěšně dokončil, budete upozorněni. Po odstranění hello probíhá, aktualizujte hello dotazů na této stránce. zálohovací sklad Hello odstranit se nebude zobrazovat v hello seznamu sad záloh.

    ![Přejděte toobackup katalogu](./media/storsimple-8000-manage-backup-catalog/bucatalog7.png)

## <a name="next-steps"></a>Další kroky
* Zjistěte, jak příliš[použití hello zálohování katalogu toorestore zařízení ze zálohovacího skladu](storsimple-8000-restore-from-backup-set-u2.md).
* Zjistěte, jak příliš[použití hello tooadminister service Manager zařízení StorSimple zařízení StorSimple](storsimple-8000-manager-service-administration.md).

