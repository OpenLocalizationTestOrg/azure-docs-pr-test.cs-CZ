---
title: "aaaDeactivate a odstranit virtuální pole Microsoft Azure StorSimple | Microsoft Docs"
description: "Popisuje, jak zařízení StorSimple tooremove ze služby tak, že nejprve deaktivace služby a poté ji odstranit."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: a929f5bc-03e2-4b01-b925-973db236f19f
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2016
ms.author: alkohli
ms.openlocfilehash: b1f3ddb5822d19965739777e238af19b507df984
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deactivate-and-delete-a-storsimple-virtual-array"></a>Deaktivovat a odstranit pole virtuální zařízení StorSimple

## <a name="overview"></a>Přehled

Pokud deaktivujete o pole virtuální zařízení StorSimple, můžete zrušit hello připojení mezi hello zařízení a služby StorSimple Manager zařízení odpovídající hello. Tento kurz vysvětluje postup:

* Deaktivace zařízení 
* Odstranit deaktivované zařízení

Hello informace v tomto článku se vztahují pouze tooStorSimple virtuální pole. Informace o řady 8000 přejděte toohow příliš[deaktivujte nebo odstraňte zařízení](storsimple-deactivate-and-delete-device.md).

## <a name="when-toodeactivate"></a>Když toodeactivate?

Deaktivace je trvalé a nedá se vrátit zpátky. Deaktivované zařízení s hello služby StorSimple Manager zařízení nelze zaregistrovat znovu. Můžete třeba toodeactivate a odstranit o virtuální zařízení StorSimple pole v hello následující scénáře:

* **Plánované převzetí služeb při selhání** : zařízení je online a plánování toofail přes zařízení. Pokud plánujete tooupgrade tooa větší zařízení, musíte toofail přes zařízení. Po převodu vlastnictví hello dat a dokončení hello převzetí služeb při selhání, hello zdrojového zařízení se automaticky odstraní.
* **Neplánované převzetí služeb při selhání** : zařízení je offline a potřebují toofail přes hello zařízení. Tento scénář může dojít při havárii, když je výpadek v datovém centru hello a primární zařízení je mimo provoz. Máte v plánu toofail přes hello zařízení tooa sekundární zařízení. Po převodu vlastnictví hello dat a dokončení hello převzetí služeb při selhání, hello zdrojového zařízení se automaticky odstraní.
* **Vyřadit z provozu** : Chcete toodecommission hello zařízení. To vyžaduje, abyste toofirst deaktivovat hello zařízení a potom jej odstraňte. Po deaktivaci zařízení jste už mít přístup k všechna data, která je uložený místně. Můžete pouze přístup a obnovit data hello, které jsou uložená v cloudu hello. Pokud máte v plánu tookeep hello zařízení dat po deaktivaci, byste měli před deaktivací zařízení vzít cloudový snímek všechna vaše data. Tento snímek cloudu vám umožní toorecover všechny hello data v pozdější fázi.

## <a name="deactivate-a-device"></a>Deaktivace zařízení

toodeactivate zařízení, proveďte následující kroky hello.

#### <a name="toodeactivate-hello-device"></a>toodeactivate hello zařízení

1. Ve službě, přejděte příliš**správy > zařízení**. V hello **zařízení** okno, klikněte na tlačítko a vyberte hello zařízení chcete toodeactivate.
   
    ![Vyberte zařízení toodeactivate](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete7.png)
2. Ve vaší **řídicí panel zařízení** okně klikněte na tlačítko **... Další** a hello seznamu, vyberte **deaktivovat**.
   
    ![Kliknutím na možnost deaktivovat](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete8.png)
3. V hello **deaktivovat** okno, název typu hello zařízení a pak klikněte na tlačítko **deaktivovat**. 
   
    ![Potvrďte deaktivovat](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete1.png)
   
    Hello deaktivovat spuštění procesu a trvá několik minut toocomplete.
   
    ![Deaktivace v průběhu](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete2.png)
4. Po deaktivaci aktualizuje hello seznam zařízení.
   
    ![Deaktivovat dokončení](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete3.png)
   
    Nyní můžete odstranit toto zařízení.

## <a name="delete-hello-device"></a>Odstranit hello zařízení

Zařízení má první deaktivované toodelete toobe ho. Odstranění zařízení odebere ze seznamu hello zařízení připojených toohello služby. Služba Hello pak již nebude možné spravovat zařízení hello odstranit. Hello data přidružená k hello zařízení ale zůstává v cloudu hello. Tato data pak nabíhají poplatky.

toodelete hello zařízení, proveďte následující kroky hello.

#### <a name="toodelete-hello-device"></a>toodelete hello zařízení

1. Přejděte ve vašem Správci zařízení StorSimple příliš**správy > zařízení**. V hello **zařízení** okně vyberte deaktivované zařízení chcete toodelete.
2. V hello **řídicí panel zařízení** okně klikněte na tlačítko **... Další** a pak klikněte na **odstranit**.
   
   ![Vyberte zařízení toodelete](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete4.png)
3. V hello **odstranit** okno, název typu hello odstranění hello tooconfirm zařízení a pak klikněte na tlačítko **odstranit**. Odstraňování hello zařízení neodstraní data v cloudu hello přidružené hello zařízení. 
   
   ![Potvrzení odstranění](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete5.png) 
4. odstranění Hello spustí a trvá několik minut toocomplete.
   
   ![Probíhá odstraňování](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete6.png)
   
    Po odstranění hello zařízení můžete zobrazit hello aktualizovat seznam zařízení.

## <a name="next-steps"></a>Další kroky

* Informace o tom toofail přes, přejděte příliš[převzetí služeb při selhání a zotavení po havárii vaše pole virtuální zařízení StorSimple](storsimple-virtual-array-failover-dr.md).

* toolearn Další informace o tom, jak toouse hello služby StorSimple Manager zařízení, přejděte příliš[použití hello tooadminister služby StorSimple Manager zařízení StorSimple virtuální pole](storsimple-virtual-array-manager-service-administration.md). 

