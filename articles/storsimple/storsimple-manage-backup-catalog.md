---
title: "aaaManage katalog záloh StorSimple | Microsoft Docs"
description: "Vysvětluje, jak toouse hello StorSimple Manager služby zálohování katalogu stránky toolist, vyberte a odstranit zálohovací sklady pro svazek."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: ad81bee9-fe43-40b3-a384-b15fb274ecd9
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 04/28/2016
ms.author: v-sharos
ms.openlocfilehash: 14f565c174a10da2c9e2f934a533a5e493f77226
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-toomanage-your-backup-catalog"></a>Použít toomanage služby StorSimple Manager hello katalog záloh
## <a name="overview"></a>Přehled
Hello služby StorSimple Manager **zálohování katalogu** stránka zobrazuje všechny zálohovací sklady hello, které vytvářejí, když jsou provedeny ruční nebo plánované zálohy. Můžete použít tuto stránku toolist všechny hello zálohy pro zásady zálohování nebo svazek, vyberte nebo odstranit zálohy, nebo použít zálohování toorestore nebo klonování svazku.

Tento kurz popisuje, jak toolist, vyberte a delete zálohovacího skladu. toolearn jak toorestore zařízení ze zálohy, přejděte příliš[obnovit vaše zařízení ze zálohovacího skladu](storsimple-restore-from-backup-set.md). jak tooclone svazku, přejděte příliš toolearn[klonovat svazek StorSimple](storsimple-clone-volume.md).

![Zálohování katalogu](./media/storsimple-manage-backup-catalog/backupcatalog.png) 

Hello **zálohování katalogu** stránka obsahuje dotaz toonarrow výběr zálohovacího skladu. Můžete filtrovat hello zálohovací sklady, které jsou načteny podle hello následující parametry:

* **Zařízení** – hello zařízení, na které hello zálohovací sklad vytvořen.
* **Zálohování zásady nebo svazek** – hello zásady zálohování nebo svazku přidruženém k tohoto zálohovacího skladu.
* **Od a do** – hello rozsah data a času v okamžiku vytvoření zálohovacího skladu hello.

Hello filtrované zálohovací sklady jsou pak v tabulce podle hello následující atributy:

* **Název** – hello název zásady zálohování hello nebo svazku přidruženém k hello zálohovacího skladu.
* **Velikost** – hello skutečná velikost hello zálohovacího skladu.
* **Vytvořit na** – hello datum a čas, kdy byla vytvořena hello zálohy. 
* **Typ** – zálohovací sklady může být místní snímky nebo cloudových snímků. Místní snímek je zálohování všech dat uložených místně na zařízení hello svazku, zatímco cloudový snímek odkazuje toohello zálohování svazku dat umístěných v cloudu hello. Místní snímky poskytují rychlejší přístup, že jsou pro záleží na odolnosti dat zvolena cloudových snímků.
* **Iniciováno** – hello zálohy lze inicializovat automaticky podle plánu nebo ručně uživatelem. Můžete použít zálohování tooschedule zásady zálohování. Alternativně můžete použít hello **provést zálohování** tootake možnost ručního zálohování.

## <a name="list-backup-sets-for-a-volume"></a>Seznam zálohovací sklady pro svazek clusteru
Proveďte následující kroky toolist hello všechny hello zálohy pro svazek.

#### <a name="toolist-backup-sets"></a>zálohovací sklady toolist
1. Na stránce služby StorSimple Manager hello, klikněte na hello **katalog zálohování** kartě.
2. Filtrovat hello výběr následujícím způsobem:
   
   1. Vyberte příslušné zařízení hello.
   2. V rozevíracím seznamu hello vyberte svazek tooview hello odpovídající hello zálohy.
   3. Zadejte časové rozmezí hello.
   4. Klikněte na ikonu zaškrtnutí hello ![Ikona zaškrtnutí](./media/storsimple-manage-backup-catalog/HCS_CheckIcon.png) tooexecute tento dotaz.
      
      zálohování Hello přidružené hello vybraný svazek by se zobrazit v seznamu hello zálohovací sklady.

## <a name="select-a-backup-set"></a>Vyberte zálohovací sklad
Proveďte následující kroky tooselect zálohu nastavení pro zásadu svazek nebo zálohování hello.

#### <a name="tooselect-a-backup-set"></a>tooselect zálohovacího skladu
1. Na stránce služby StorSimple Manager hello, klikněte na hello **katalog zálohování** kartě.
2. Filtrovat hello výběr následujícím způsobem:
   
   1. Vyberte příslušné zařízení hello.
   2. V rozevíracím seznamu hello výběru hello svazek nebo zálohování zásady zálohování hello chcete tooselect.
   3. Zadejte časové rozmezí hello.
   4. Klikněte na ikonu zaškrtnutí hello ![Ikona zaškrtnutí](./media/storsimple-manage-backup-catalog/HCS_CheckIcon.png) tooexecute tento dotaz.
      
      Hello zálohování přidružená k zásadě hello vybraný svazek nebo zálohování by se zobrazit v seznamu hello zálohovací sklady.
3. Vyberte a rozbalte zálohovacího skladu. Hello **obnovení** a **odstranit** možnosti se zobrazí v dolní části hello hello stránky. Můžete provést některý z těchto akcí na hello zálohovacího skladu, který jste vybrali.

## <a name="delete-a-backup-set"></a>Odstranit zálohovacího skladu
Odstraňte zálohy, když už nechcete tooretain hello data s ním spojená. Proveďte následující kroky toodelete zálohovacího skladu hello.

#### <a name="toodelete-a-backup-set"></a>toodelete zálohovacího skladu
1. Na stránce služby StorSimple Manager hello, klikněte na hello **katalog zálohování karta**.
2. Filtrovat hello výběr následujícím způsobem:
   
   1. Vyberte příslušné zařízení hello.
   2. V rozevíracím seznamu hello výběru hello svazek nebo zálohování zásady zálohování hello chcete tooselect.
   3. Zadejte časové rozmezí hello.
   4. Klikněte na ikonu zaškrtnutí hello ![Ikona zaškrtnutí](./media/storsimple-manage-backup-catalog/HCS_CheckIcon.png) tooexecute tento dotaz.
      
      Hello zálohování přidružená k zásadě hello vybraný svazek nebo zálohování by se zobrazit v seznamu hello zálohovací sklady.
3. Vyberte a rozbalte zálohovacího skladu. Hello **obnovení** a **odstranit** možnosti se zobrazí v dolní části hello hello stránky. Klikněte na **Odstranit**.
4. Při odstranění hello právě probíhá a při úspěšně dokončil, budete upozorněni. Po odstranění hello probíhá, aktualizujte hello dotazů na této stránce. zálohovací sklad Hello odstranit se nebude zobrazovat v hello seznamu sad záloh.

## <a name="next-steps"></a>Další kroky
* Zjistěte, jak příliš[použití hello zálohování katalogu toorestore zařízení ze zálohovacího skladu](storsimple-restore-from-backup-set.md).
* Zjistěte, jak příliš[použití hello tooadminister služby StorSimple Manager zařízení StorSimple](storsimple-manager-service-administration.md).

