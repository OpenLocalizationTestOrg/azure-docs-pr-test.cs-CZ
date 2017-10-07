---
title: "aaaStorSimple převzetí služeb při selhání, tooa obnovení po havárii cloudu zařízení StorSimple | Microsoft Docs"
description: "Zjistěte, jak toofail přes vaše tooa fyzického zařízení řady StorSimple 8000 cloudové zařízení."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/03/2017
ms.author: alkohli
ms.openlocfilehash: e8a0bca057024358e3a557fe85a42ddefea36cff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="fail-over-tooyour-storsimple-cloud-appliance"></a>Selhání tooyour cloudu zařízení StorSimple

## <a name="overview"></a>Přehled

Tento kurz popisuje hello kroky požadované toofail přes tooa fyzického zařízení řady StorSimple 8000 cloudu zařízení StorSimple, pokud dojde k havárii. StorSimple použije hello zařízení převzetí služeb při selhání funkce toomigrate data z fyzického zařízení zdroj hello datacenter tooa cloudu zařízení běží v Azure. Hello pokyny v tomto kurzu platí tooStorSimple 8000 řady fyzických zařízení a zařízení cloudu spuštěná verze softwaru Update 3 nebo novější.

toolearn informace o převzetí služeb při selhání zařízení a jak je použité toorecover po havárii, přejděte příliš[převzetí služeb při selhání a zotavení po havárii pro řadu zařízení StorSimple 8000](storsimple-8000-device-failover-disaster-recovery.md).

toofail přes StorSimple fyzického zařízení tooanother fyzické zařízení, přejděte příliš[selhání fyzického zařízení StorSimple tooa](storsimple-8000-device-failover-physical-device.md). toofail přes tooitself zařízení, přejděte příliš[převzetí služeb při selhání toohello stejné fyzického zařízení StorSimple](storsimple-8000-device-failover-same-device.md).

## <a name="prerequisites"></a>Požadavky

- Ujistěte se, že jste si přečetli hello aspekty pro převzetí služeb při selhání zařízení. Další informace, přejděte příliš[častá rozhodnutí při převzetí služeb při selhání zařízení](storsimple-8000-device-failover-disaster-recovery.md).

- Musíte mít cloudu zařízení StorSimple, vytvořený a nakonfigurovaný před spuštěním tohoto postupu. Pokud spuštěná aktualizace softwaru verze 3 nebo novější, zvažte použití zařízení 8020 cloudu pro hello zotavení po Havárii. Hello 8020 model má 64 TB a používá úložiště Premium. Další informace, přejděte příliš[nasadit a spravovat zařízení s StorSimple cloudu](storsimple-8000-cloud-appliance-u2.md).

## <a name="steps-toofail-over-tooa-cloud-appliance"></a>Kroky toofail přes tooa cloudu zařízení

Proveďte následující kroky toorestore hello zařízení tooa cílový Cloud zařízení StorSimple hello.

1.  Ověřte, že tento kontejner svazků hello, které chcete toofail přes přidruženy cloudových snímků. Další informace, přejděte příliš[zálohování toocreate služby pomocí Správce zařízení StorSimple](storsimple-8000-manage-backup-policies-u2.md).
2. Přejděte služby StorSimple Manager zařízení tooyour a klikněte na tlačítko **zařízení**. V hello **zařízení** okno, přejděte toohello seznam zařízení připojená k vaší službě.
    ![Vyberte zařízení](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev1.png)
3. Vyberte a klikněte na vaše zdrojové zařízení. Hello zdrojové zařízení má hello kontejnery svazků, které chcete toofail přes. Přejděte příliš**Nastavení > kontejnery svazků**.

    ![Vyberte zařízení](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev2.png)
    
4. Vyberte kontejner svazků, které chcete toofail přes tooanother zařízení. Klikněte na tlačítko hello svazku kontejneru toodisplay hello seznam svazků v tomto kontejneru. Vyberte svazek, klikněte pravým tlačítkem a klikněte na **přepnout do režimu Offline** tootake hello svazek do režimu offline.

    ![Vyberte zařízení](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev5.png)

5. Tento postup opakujte pro všechny svazky hello v kontejneru svazků hello.

     ![Vyberte zařízení](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev7.png)

6. Předchozí krok zopakujte hello pro všechny kontejnery svazků hello chcete toofail přes tooanother zařízení.

7. Přejděte zpět toohello **zařízení** okno. Na panelu příkazů hello, klikněte na **převzetí služeb při selhání**.

    ![Klikněte na tlačítko selhání přes](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev8.png)
8. V hello **převzetí služeb při selhání** okně provést hello následující kroky:
   
    1. Klikněte na tlačítko **zdroj**. Vyberte hello svazku kontejnery toofail přes. **Pouze hello kontejnery svazků s přidružený cloud snímky a offline svazky jsou zobrazeny.**
        ![Vyberte zdroj](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev11.png)
    2. Klikněte na tlačítko **cíl**. Z rozevíracího seznamu hello k dispozici zařízení vyberte zařízení s cílový cloud. **Pouze hello zařízení, která mají dostatečná kapacita tooaccommodate zdrojového svazku kontejnery se zobrazí v seznamu hello.**

        ![Vyberte cíl](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev12.png)

    3. Zkontrolujte nastavení hello převzetí služeb při selhání v rámci **Souhrn** a vyberte hello políčko o tom, že hello svazky v kontejnerech vybraný svazek offline. 

        ![Zkontrolujte nastavení převzetí služeb při selhání](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev13.png)

9. Bude vytvořena úloha převzetí služeb při selhání. toomonitor hello převzetí služeb při selhání úlohy, klikněte na úlohu oznámení hello.

    ![Úloha monitoru převzetí služeb při selhání](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev13.png)

10. Po dokončení převzetí služeb při selhání hello vraťte toohello **zařízení** okno.

    1. Vyberte hello zařízení, která byla použita jako cíl hello hello převzetí služeb při selhání.

       ![Vyberte zařízení](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev14.png)

    2. Klikněte na tlačítko **kontejnery svazků**. Všechny kontejnery svazků hello, společně s hello svazky z původního zařízení hello by měl být uvedený.

       Pokud hello kontejneru svazků, které při selhání má místně vázaný svazky, jsou tyto svazky při selhání jako vrstvené svazky. O cloudu zařízení StorSimple nepodporuje místně vázaných svazků.

       ![Kontejnery svazků cílové zobrazení](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev17.png)


## <a name="next-steps"></a>Další kroky

* Po provedení převzetí služeb při selhání, můžete potřebovat příliš[deaktivujte nebo odstraňte zařízení StorSimple](storsimple-8000-deactivate-and-delete-device.md).

* Informace o tom, jak toouse hello Správce zařízení StorSimple služby, přejděte příliš[použití hello tooadminister service Manager zařízení StorSimple zařízení StorSimple](storsimple-8000-manager-service-administration.md).

