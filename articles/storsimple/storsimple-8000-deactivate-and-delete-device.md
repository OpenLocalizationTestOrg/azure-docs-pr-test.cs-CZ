---
title: "aaaDeactivate a odstranění zařízení řady StorSimple 8000 | Microsoft Docs"
description: "Popisuje, jak zařízení StorSimple tooremove ze služby tak, že nejprve deaktivace služby a poté ji odstranit."
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
ms.date: 05/23/2017
ms.author: alkohli
ms.openlocfilehash: 841ecd7f0fb5e425bf23e1fe0044faeab2af4b53
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deactivate-and-delete-a-storsimple-device"></a>Deaktivace a odstranění zařízení StorSimple

## <a name="overview"></a>Přehled

Tento článek popisuje, jak toodeactivate a odstranění zařízení StorSimple, který je připojený tooa služby StorSimple Manager zařízení. Hello pokyny v tomto článku se vztahuje pouze tooStorSimple řady 8000 zařízení včetně zařízení cloudu StorSimple hello. Pokud používáte o pole virtuální zařízení StorSimple, potom přejděte příliš[deaktivace a odstranění o pole virtuální zařízení StorSimple](storsimple-virtual-array-deactivate-and-delete-device.md).

Deaktivace servery hello připojení mezi hello zařízení a služby StorSimple Manager zařízení odpovídající hello. Můžete tootake zařízení StorSimple mimo provoz (např. Pokud jsou výměna nebo upgrade zařízení nebo pokud už používáte StorSimple). Pokud ano, musíte se před odstraněním jej toodeactivate hello zařízení.

Po deaktivaci zařízení žádná data, která byla uložená místně na hello zařízení již není dostupný. Lze obnovit pouze hello data přidružená k hello zařízení, která byla uložená v cloudu hello.

> [!WARNING]
> Deaktivace je trvalé a nedá se vrátit zpátky. Deaktivované zařízení nelze zaregistrovat s hello služby StorSimple Manager zařízení, pokud je obnovit výchozí hodnoty toofactory.
>
> Hello obnovit tovární nastavení proces odstraní všechna data hello, která byla uložená místně na vašem zařízení. Proto je třeba provést cloudový snímek všechna vaše data před deaktivací zařízení. Tento snímek cloudu vám umožní toorecover všechny hello data v pozdější fázi.

Po přečtení tohoto kurzu, budete moci:

* Deaktivace zařízení a odstranění dat hello.
* Deaktivace zařízení a umožňuje uchovávat hello data.

> [!NOTE]
> Před deaktivací fyzického zařízení StorSimple nebo cloudu zařízení, zastavte nebo odstraňte klienty a hostitele, které jsou závislé na tomto zařízení.


## <a name="deactivate-and-delete-data"></a>Deaktivace a odstranění dat

Pokud mají zájem o odstraňování zařízení hello úplně a nechcete, aby tooretain hello dat na zařízení hello, dokončete následující kroky hello.

#### <a name="toodeactivate-hello-device-and-delete-hello-data"></a>toodeactivate hello zařízení a odstranění hello dat

1. Před deaktivací zařízení, je nutné odstranit všechny hello svazku kontejnery (a svazky hello) přidružené hello zařízení. Kontejnery svazků můžete odstranit až poté, co jste odstranili hello přidružené zálohy.

    > [!NOTE]
    > Před deaktivací fyzického zařízení StorSimple nebo cloudu zařízení, ověřte, že hello data z kontejneru svazků hello odstranit je ve skutečnosti odstraněný z hello zařízení. Můžete monitorovat hello cloudu spotřeba grafy a když se zobrazí využití cloudu hello vyřadit z důvodu hello zálohování, které jste odstranili, potom můžete přejít toodeactivate hello zařízení. Pokud deaktivujete hello zařízení před tento rozevírací k hello dat je Bezvýchodná situace v účtu úložiště hello a nabíhají poplatky.

2. Deaktivace zařízení hello následujícím způsobem:
   
   1. Přejděte služby StorSimple Manager zařízení tooyour a klikněte na tlačítko **zařízení**. V hello **zařízení** okně, vyberte hello zařízení chcete toodeactivate, klikněte pravým tlačítkem a pak klikněte na tlačítko **deaktivovat**.

        ![Deaktivace zařízení StorSimple](./media/storsimple-8000-deactivate-and-delete-device/deactivate1.png)
   2. V hello **deaktivovat** okno, zadejte tooconfirm název hello zařízení a potom klikněte na **deaktivovat**. Hello deaktivovat spuštění procesu a trvá několik minut toocomplete.

        ![Deaktivace zařízení StorSimple](./media/storsimple-8000-deactivate-and-delete-device/deactivate2.png)

3. Po deaktivaci můžete odstranit zařízení hello úplně. Odstranění zařízení odebere ze seznamu hello zařízení připojených toohello služby. Služba Hello pak již nebude možné spravovat zařízení hello odstranit. Použijte následující postup toodelete hello zařízení hello:
   
   1. Přejděte služby StorSimple Manager zařízení tooyour a klikněte na tlačítko **zařízení**. V hello **zařízení** okně, vyberte hello deaktivovat zařízení chcete toodelete, klikněte pravým tlačítkem a pak klikněte na tlačítko **odstranit**.

        ![Deaktivace zařízení StorSimple](./media/storsimple-8000-deactivate-and-delete-device/deactivate5.png)
   2. V hello **odstranit** okno, zadejte tooconfirm název hello zařízení a potom klikněte na **odstranit**. odstranění Hello trvá několik minut toocomplete.

        ![Deaktivace zařízení StorSimple](./media/storsimple-8000-deactivate-and-delete-device/deactivate6.png)
   3. Po odstranění hello je úspěšně dokončit, zobrazí se upozornění. seznam zařízení Hello rovněž aktualizuje tooreflect hello odstranění.

## <a name="deactivate-and-retain-data"></a>Deaktivovat a zachovat data

Pokud jsou zajímá odstraňování hello zařízení, ale mají tooretain hello data, dokončete hello následující kroky:

#### <a name="toodeactivate-a-device-and-retain-hello-data"></a>toodeactivate zařízení a umožňuje uchovávat hello data
1. Deaktivujte zařízení hello. Všechny hello kontejnery svazků a snímky hello hello zařízení zůstanou.
   
   1. Přejděte služby StorSimple Manager zařízení tooyour a klikněte na tlačítko **zařízení**. V hello **zařízení** okně, vyberte hello zařízení chcete toodeactivate, klikněte pravým tlačítkem a pak klikněte na tlačítko **deaktivovat**.

         ![Deaktivace zařízení StorSimple](./media/storsimple-8000-deactivate-and-delete-device/deactivate1.png)
   2. V hello **deaktivovat** okno, zadejte tooconfirm název hello zařízení a potom klikněte na **deaktivovat**. Hello deaktivovat spuštění procesu a trvá několik minut toocomplete.

         ![Deaktivace zařízení StorSimple](./media/storsimple-8000-deactivate-and-delete-device/deactivate2.png)
2. Nyní můžete převzít kontejnery svazků hello a hello přidružené snímky. Postupy, přejděte příliš[převzetí služeb při selhání a zotavení po havárii pro zařízení StorSimple](storsimple-8000-device-failover-disaster-recovery.md).
3. Po deaktivaci a převzetí služeb při selhání můžete odstranit zařízení hello úplně. Odstranění zařízení odebere ze seznamu hello zařízení připojených toohello služby. Služba Hello pak již nebude možné spravovat zařízení hello odstranit. toodelete hello zařízení, dokončení hello následující kroky:
   
   1. Přejděte služby StorSimple Manager zařízení tooyour a klikněte na tlačítko **zařízení**. V hello **zařízení** okně, vyberte hello deaktivovat zařízení chcete toodelete, klikněte pravým tlačítkem a pak klikněte na tlačítko **odstranit**.

       ![Deaktivace zařízení StorSimple](./media/storsimple-8000-deactivate-and-delete-device/deactivate5.png)
   2. V hello **odstranit** okno, zadejte tooconfirm název hello zařízení a potom klikněte na **odstranit**. odstranění Hello trvá několik minut toocomplete.

       ![Deaktivace zařízení StorSimple](./media/storsimple-8000-deactivate-and-delete-device/deactivate6.png)
   3. Po odstranění hello je úspěšně dokončit, zobrazí se upozornění. seznam zařízení Hello rovněž aktualizuje tooreflect hello odstranění.

     
## <a name="deactivate-and-delete-a-cloud-appliance"></a>Deaktivace a odstranění zařízení cloudu

Pro zařízení StorSimple cloudu deaktivace z portálu hello zruší přidělení a odstraní hello virtuální počítač a prostředky hello vytvořené při jeho zřizování. Po deaktivaci hello cloudu zařízení nejde obnovit tooits předchozího stavu.

![Deaktivace zařízení StorSimple cloudu](./media/storsimple-8000-deactivate-and-delete-device/deactivate7.png)

Deaktivace výsledkem hello následující akce:

* Hello cloudu zařízení StorSimple se odebere ze služby hello.
* Hello virtuální počítač pro hello cloudu zařízení StorSimple je odstranit.
* Hello disk operačního systému a datové disky, které jsou vytvořené pro hello cloudu zařízení StorSimple se odeberou.
* Hello hostované služby a virtuální sítě, které byly vytvořeny při zřizování zůstanou zachovány. Pokud nepoužíváte tyto entity, odstraňte je ručně.
* Cloudové snímky vytvořené hello StorSimple cloudu zařízení zůstanou zachovány.

Po deaktivaci hello cloudu zařízení odstraníte z hello seznam zařízení. Vyberte hello deaktivovat zařízení, klikněte pravým tlačítkem a pak klikněte na tlačítko **odstranit**. StorSimple vás upozorní, jakmile hello zařízení se odstraní a hello seznam zařízení aktualizací.

## <a name="next-steps"></a>Další kroky

* toorestore hello výchozí toofactory deaktivované zařízení, přejděte příliš[resetovat hello zařízení toofactory výchozí nastavení](storsimple-8000-manage-device-controller.md#reset-the-device-to-factory-default-settings).
* Pro technickou podporu [kontaktovat Microsoft Support](storsimple-8000-contact-microsoft-support.md).
* toolearn Další informace o tom, jak toouse hello služby StorSimple Manager zařízení, přejděte příliš[použití hello tooadminister service Manager zařízení StorSimple zařízení StorSimple](storsimple-8000-manager-service-administration.md).

