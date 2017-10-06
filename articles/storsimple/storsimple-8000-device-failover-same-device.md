---
title: "aaaStorSimple převzetí služeb při selhání, zotavení po havárii pro řady 8000 zařízení | Microsoft Docs"
description: "Zjistěte, jak toofail přes vaše toohello zařízení StorSimple ve stejném zařízení."
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
ms.date: 06/23/2017
ms.author: alkohli
ms.openlocfilehash: b0b4216c7af6745ff68b85ca3d655691b43b4334
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="fail-over-your-storsimple-physical-device-toosame-device"></a>Selhání zařízení toosame fyzického zařízení StorSimple

## <a name="overview"></a>Přehled

Tento kurz popisuje hello kroky požadované toofail přes tooitself fyzického zařízení StorSimple 8000 řad, pokud dojde k havárii. StorSimple použije hello zařízení převzetí služeb při selhání funkce toomigrate data z fyzického zařízení zdroj hello datacenter tooanother fyzického zařízení. Hello pokyny v tomto kurzu platí tooStorSimple 8000 řady fyzických zařízení spuštěná verze softwaru Update 3 nebo novější.

toolearn informace o převzetí služeb při selhání zařízení a jak je použité toorecover po havárii, přejděte příliš[převzetí služeb při selhání a zotavení po havárii pro řadu zařízení StorSimple 8000](storsimple-8000-device-failover-disaster-recovery.md).

toofail přes tooanother fyzického zařízení fyzické zařízení, přejděte příliš[převzetí služeb při selhání toohello stejné fyzického zařízení StorSimple](storsimple-8000-device-failover-physical-device.md). toofail přes tooa fyzického zařízení StorSimple cloudu zařízení StorSimple, přejděte příliš[převzít tooa zařízení StorSimple cloudu](storsimple-8000-device-failover-cloud-appliance.md).


## <a name="prerequisites"></a>Požadavky

- Ujistěte se, že jste si přečetli hello aspekty pro převzetí služeb při selhání zařízení. Další informace, přejděte příliš[častá rozhodnutí při převzetí služeb při selhání zařízení](storsimple-8000-device-failover-disaster-recovery.md).


## <a name="steps-toofail-over-toohello-same-device"></a>Kroky toofail přes toohello stejného zařízení

Proveďte následující kroky, pokud potřebujete toofail přes toohello hello stejné zařízení.

1. Cloudové snímky všech svazků hello proveďte v zařízení. Další informace, přejděte příliš[zálohování toocreate služby pomocí Správce zařízení StorSimple](storsimple-8000-manage-backup-policies-u2.md).
2. Obnovte výchozí hodnoty pro toofactory vaše zařízení. Postupujte podle hello podrobné pokyny v [jak tooreset toofactory zařízení StorSimple výchozí nastavení](storsimple-8000-manage-device-controller.md#reset-the-device-to-factory-default-settings).
3. Přejděte služby StorSimple Manager zařízení toohello a pak vyberte **zařízení**. V hello **zařízení** okně hello staré zařízení by měl zobrazit jako **Offline**.

    ![Zdrojového zařízení do offline režimu](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev2.png)

4. Konfigurace zařízení a zaregistrujte ho znovu pomocí služby StorSimple Manager zařízení. Hello nově registrovaná zařízení by měl zobrazit jako **připraveni tooset až**. název zařízení Hello hello nového zařízení je hello stejný jako původní zařízení hello ale s příponou číslice tooindicate hello zařízení byla výchozí toofactory resetování a registraci znovu.

    ![Nově registrovaná zařízení připraveno tooset nahoru](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev3.png)
5. Pro nové zařízení hello dokončení instalace zařízení hello. Další informace, přejděte příliš[krok 4: dokončení minimální instalace zařízení](storsimple-8000-deployment-walkthrough-u2.md#step-4-complete-minimum-device-setup). Na hello **zařízení** okně hello stav zařízení hello změní příliš**Online**.

   > [!IMPORTANT]
   > **Dokončení minimální konfigurace hello nejprve nebo vaše zotavení po Havárii může selhat.**

    ![Online nově registrovaná zařízení](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev7.png)

6. Vyberte zařízení staré hello (offline stavu) a na panelu příkazů hello, klikněte na **převzetí služeb při selhání**. V hello **převzetí služeb při selhání** okně vyberte původní zařízení jako zdroj hello a zadejte hello cílové zařízení hello nově registrovaného zařízení.

    ![Souhrn převzetí služeb při selhání](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev11.png)

    Podrobné pokyny najdete v části příliš[selhání fyzického zařízení tooanother](#fail-over-to-another-physical-device).

7. Úloha obnovení zařízení se vytvoří, můžete monitorovat z hello **úlohy** okno.

8. Po úspěšném dokončení úlohy hello hello nové zařízení pro přístup k a přejděte toohello **kontejnery svazků** okno. Ověřte, že všechny kontejnery svazků hello z původního zařízení hello byly migrovány toohello nové zařízení.

   ![Kontejnery svazků migrovat](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev13.png)

9. Po dokončení převzetí služeb při selhání hello můžete deaktivovat a odstranit stará zařízení hello z portálu hello. Vyberte hello staré zařízení (offline), klikněte pravým tlačítkem a pak vyberte **deaktivovat**. Po deaktivaci hello zařízení hello stav zařízení hello se aktualizuje.

     ![Deaktivovat zdrojového zařízení](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev14.png)

10. Vyberte hello deaktivovat zařízení, klikněte pravým tlačítkem a pak vyberte **odstranit**. Toto zařízení hello odstraní z hello seznam zařízení.

    ![Odstranit zdrojového zařízení](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev15.png)



## <a name="next-steps"></a>Další kroky

* Po provedení převzetí služeb při selhání, můžete potřebovat příliš[deaktivujte nebo odstraňte zařízení StorSimple](storsimple-8000-deactivate-and-delete-device.md).
* Informace o tom, jak toouse hello Správce zařízení StorSimple služby, přejděte příliš[použití hello tooadminister service Manager zařízení StorSimple zařízení StorSimple](storsimple-8000-manager-service-administration.md).

