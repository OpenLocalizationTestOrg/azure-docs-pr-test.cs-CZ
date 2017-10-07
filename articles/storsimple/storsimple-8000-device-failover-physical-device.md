---
title: "aaaStorSimple převzetí služeb při selhání, po havárii obnovení tooa StorSimple 8000 řady fyzického zařízení | Microsoft Docs"
description: "Zjistěte, jak toofail přes StorSimple 8000 řady fyzického zařízení tooanother fyzického zařízení."
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
ms.date: 05/03/2017
ms.author: alkohli
ms.openlocfilehash: 29d2576a96c446ff5ffcd98dcd0f5a07f1ab08ca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="fail-over-tooa-storsimple-8000-series-physical-device"></a>Selhání fyzického zařízení StorSimple 8000 tooa řady

## <a name="overview"></a>Přehled

Tento kurz popisuje hello kroky požadované toofail přes StorSimple 8000 řady fyzického zařízení tooanother fyzického zařízení StorSimple, pokud dojde k havárii. StorSimple použije hello zařízení převzetí služeb při selhání funkce toomigrate data z fyzického zařízení zdroj hello datacenter tooanother fyzického zařízení. Hello pokyny v tomto kurzu platí tooStorSimple 8000 řady fyzických zařízení spuštěná verze softwaru Update 3 nebo novější.

toolearn informace o převzetí služeb při selhání zařízení a jak je použité toorecover po havárii, přejděte příliš[převzetí služeb při selhání a zotavení po havárii pro řadu zařízení StorSimple 8000](storsimple-8000-device-failover-disaster-recovery.md).

toofail přes tooa fyzického zařízení StorSimple cloudu zařízení StorSimple, přejděte příliš[převzít tooa zařízení StorSimple cloudu](storsimple-8000-device-failover-cloud-appliance.md). toofail přes tooitself fyzického zařízení přejděte příliš[převzetí služeb při selhání toohello stejné fyzického zařízení StorSimple](storsimple-8000-device-failover-same-device.md).


## <a name="prerequisites"></a>Požadavky

- Ujistěte se, že jste si přečetli hello aspekty pro převzetí služeb při selhání zařízení. Další informace, přejděte příliš[častá rozhodnutí při převzetí služeb při selhání zařízení](storsimple-8000-device-failover-disaster-recovery.md).

- Musíte mít fyzického zařízení StorSimple 8000 řady nasazena v datovém centru hello. Hello zařízení musí používat verzi Update 3 nebo novější verze softwaru. Další informace, přejděte příliš[nasazení místního zařízení StorSimple](storsimple-8000-deployment-walkthrough-u2.md).


## <a name="steps-toofail-over-tooa-physical-device"></a>Kroky toofail přes tooa fyzického zařízení

Proveďte následující kroky toorestore hello fyzického zařízení tooa cílové zařízení.

1. Ověřte, že tento kontejner svazků hello, které chcete toofail přes přidruženy cloudových snímků. Další informace, přejděte příliš[zálohování toocreate služby pomocí Správce zařízení StorSimple](storsimple-8000-manage-backup-policies-u2.md).
2. Přejděte tooyour Správce zařízení StorSimple a pak klikněte na tlačítko **zařízení**. V hello **zařízení** okno, přejděte toohello seznam zařízení připojená k vaší službě.
    ![Vyberte zařízení](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev1.png)
3. Vyberte a klikněte na vaše zdrojové zařízení. Hello zdrojové zařízení má hello kontejnery svazků, které chcete toofail přes. Přejděte příliš**Nastavení > kontejnery svazků**.
4. Vyberte kontejner svazků, které chcete toofail přes tooanother zařízení. Klikněte na tlačítko hello svazku kontejneru toodisplay hello seznam svazků v tomto kontejneru. Vyberte svazek, klikněte pravým tlačítkem a klikněte na **přepnout do režimu Offline** tootake hello svazek do režimu offline. Tento postup opakujte pro všechny svazky hello v kontejneru svazků hello.
5. Předchozí krok zopakujte hello pro všechny kontejnery svazků hello chcete toofail přes tooanother zařízení.
6. Přejděte zpět toohello **zařízení** okno. Na panelu příkazů hello, klikněte na **převzetí služeb při selhání**.
    ![Klikněte na tlačítko selhání přes](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev2.png)
    
7. V hello **převzetí služeb při selhání** okně provést hello následující kroky:
   
   1. Klikněte na tlačítko **zdroj**. Zobrazí se Hello kontejnery svazků pro svazky, které jsou přidružené k cloudových snímků. Pouze hello kontejnery zobrazí jsou způsobilé pro převzetí služeb při selhání. V seznamu hello kontejnery svazků vyberte hello kontejnery svazků, které si přejete toofail přes. **Pouze hello kontejnery svazků s přidružený cloud snímky a offline svazky jsou zobrazeny.**

       ![Vyberte zdroj](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev5.png)
   2. Klikněte na tlačítko **cíl**. Pro kontejnery svazků hello vybrali v předchozím kroku hello vyberte z rozevíracího seznamu hello k dispozici zařízení cílové zařízení. Pouze hello zařízení, která mají dostatečná kapacita tooaccommodate zdrojového svazku kontejnery se zobrazí v seznamu hello.

        ![Vyberte cíl](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev6.png)

   3. Nakonec zkontrolujte všechna nastavení hello převzetí služeb při selhání v **Souhrn**. Po kontrole hello nastavení, vyberte hello políčko o tom, že hello svazky v kontejnerech vybraný svazek offline. Klikněte na **OK**.

       ![Zkontrolujte nastavení převzetí služeb při selhání](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev8.png)
  
8. StorSimple vytvoří úlohu převzetí služeb při selhání. Klikněte na tlačítko hello úlohy oznámení toomonitor hello převzetí služeb při selhání úlohy prostřednictvím hello **úlohy** okno.

    Pokud hello kontejneru svazků, které při selhání má místní svazků, zobrazí jednotlivých úloh obnovení pro každý místní svazek (ne pro vrstvené svazky) v kontejneru hello. Tyto úlohy obnovení může trvat poměrně některé toocomplete čas. Je pravděpodobné, že může dříve dokončení této úlohy hello převzetí služeb při selhání. Tyto svazky budou mít místní záruky, až po dokončení se hello úloh obnovení.

    ![Úloha monitoru převzetí služeb při selhání](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev13.png)

9. Po dokončení převzetí služeb při selhání hello přejděte toohello **zařízení** okno.
   
   1. Vyberte hello zařízení, která byla použita jako hello cílové zařízení pro proces převzetí služeb při selhání hello.

       ![Vyberte zařízení](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev14.png)

   2. Přejděte toohello **kontejnery svazků** okno. Všechny kontejnery svazků hello, společně s hello svazky z původního zařízení hello by měl být uvedený.

       ![Kontejnery svazků cílové zobrazení](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev16.png)


## <a name="next-steps"></a>Další kroky

* Po provedení převzetí služeb při selhání, můžete potřebovat příliš[deaktivujte nebo odstraňte zařízení StorSimple](storsimple-8000-deactivate-and-delete-device.md).
* Informace o tom, jak toouse hello Správce zařízení StorSimple služby, přejděte příliš[použití hello tooadminister service Manager zařízení StorSimple zařízení StorSimple](storsimple-8000-manager-service-administration.md).

