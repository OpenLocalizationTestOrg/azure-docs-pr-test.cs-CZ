---
title: "aaaDeploy služby StorSimple Manager zařízení | Microsoft Docs"
description: "Popisuje, jak toocreate a odstranění hello služby StorSimple Manager zařízení v hello portálu Azure a popisuje, jak toomanage hello registrační klíč služby."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 28499494-8c4d-4a7f-9d44-94b2b8a93c93
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/29/2016
ms.author: alkohli
ms.openlocfilehash: 9064053addc7b3dfcce08b47e81b38c2e0e1b559
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-hello-storsimple-device-manager-service-for-storsimple-virtual-array"></a>Nasazení služby StorSimple Manager zařízení hello pole virtuální zařízení StorSimple
## <a name="overview"></a>Přehled

Hello služby StorSimple Manager zařízení běží ve službě Microsoft Azure a připojí zařízení StorSimple toomultiple. Po vytvoření služby hello, můžete ji použít toomanage hello zařízení z hello Microsoft Azure portálu spuštěnému v prohlížeči. To vám umožní toomonitor všechna hello zařízení, které jsou připojené toohello Správce zařízení StorSimple služby z jedné centrální umístění, a současně minimalizujete její související administrativní zátěže.

Hello, běžné úkoly, související, tooa služby StorSimple Manager zařízení jsou:

* Vytvoření služby
* Odstranění služby
* Získat registrační klíč služby hello
* Znovu vygenerovat registrační klíč služby hello

Tento kurz popisuje, jak tooperform z hello předchozí úlohy. Hello informace obsažené v tomto článku je použít jenom tooStorSimple virtuální pole. Další informace o řady StorSimple 8000 přejděte příliš[nasazení služby StorSimple Manager](storsimple-manage-service.md).

## <a name="create-a-service"></a>Vytvoření služby

toocreate služby, je třeba toohave:

* Předplatné s smlouvu Enterprise Agreement
* Aktivní účet úložiště Microsoft Azure
* fakturační informace, které se používá pro správu přístupu Hello

Můžete také toogenerate účet úložiště při vytváření služby hello.

Jeden služby můžete spravovat více zařízení. Zařízení, ale nemůžou zahrnovat víc služeb. Velký podnik může mít více instancí toowork služby pomocí různých předplatných, organizace nebo i umístění nasazení.

> [!NOTE]
> Je třeba samostatné instance řadu zařízení StorSimple Manager zařízení služby toomanage StorSimple 8000 a pole virtuální zařízení StorSimple.


Proveďte následující kroky toocreate služby hello.

[!INCLUDE [storsimple-virtual-array-create-new-service](../../includes/storsimple-virtual-array-create-new-service.md)]

## <a name="delete-a-service"></a>Odstranění služby

Před odstraněním služby, ujistěte se, že žádné připojená zařízení ji používají. Pokud služba hello se používá, deaktivujte hello připojené zařízení. Hello deaktivovat operaci severu hello připojení mezi hello zařízení a hello služby, ale zachovat data zařízení hello v cloudu hello.

> [!IMPORTANT]
> Po odstranění služby hello operace se nedá vrátit. Jakékoli zařízení, které se pomocí služby hello potřebovat toobe tovární nastavení, než se dá použít s jinou službu. V tomto scénáři hello místní data na zařízení hello, jakož i hello konfigurace budou ztraceny.
 

Proveďte následující kroky toodelete služby hello.

#### <a name="toodelete-a-service"></a>toodelete služby

1. Přejděte příliš**všechny prostředky**. Vyhledávání služby StorSimple Manager zařízení. Vyberte službu hello chcete toodelete.
   
    ![Vyberte toodelete služby](./media/storsimple-virtual-array-manage-service/deleteservice2.png)
2. Přejděte tooensure řídicí panel služby tooyour neexistují žádné zařízení připojených toohello služby. Pokud nejsou žádná zařízení registrovaná ve službě, zobrazí se také vliv toohello zprávy informační zpráva. Klikněte na **Odstranit**.
   
    ![Odstranění služby](./media/storsimple-virtual-array-manage-service/deleteservice3.png)

3. Po zobrazení výzvy k potvrzení, klikněte na tlačítko **Ano** v hello potvrzení oznámení. 
   
    ![Potvrdit odstranění služby](./media/storsimple-virtual-array-manage-service/deleteservice4.png)
4. To může trvat několik minut, než toobe služby hello odstranit. Po hello služba se úspěšně odstranila, budete upozorněni.
   
    ![Odstranění úspěšné služby](./media/storsimple-virtual-array-manage-service/deleteservice6.png)

Hello seznam služeb se aktualizují.

 ![Aktualizovaný seznam služeb](./media/storsimple-virtual-array-manage-service/deleteservice7.png)

## <a name="get-hello-service-registration-key"></a>Získat registrační klíč služby hello
Po úspěšném vytvoření služby budete potřebovat tooregister zařízení StorSimple službou hello. tooregister prvního zařízení StorSimple, bude nutné hello registrační klíč služby. tooregister další zařízení s existující službu StorSimple, budete potřebovat hello registrační klíč a hello šifrovacího klíče dat služby (který je generován během registrace na první zařízení hello). Další informace o hello šifrovacího klíče dat služby najdete v tématu [zabezpečení zařízení StorSimple](storsimple-security.md). Hello registrační klíč můžete získat přístup k hello **klíče** okno pro vaši službu.

Proveďte následující kroky tooget hello služby registrační klíč hello.

#### <a name="tooget-hello-service-registration-key"></a>registrační klíč služby hello tooget
1. V hello **Manager zařízení StorSimple** okně přejděte příliš**správy &gt;**  **klíče**.
   
   ![Okno Klíče](./media/storsimple-virtual-array-manage-service/getregkey2.png)
2. V hello **klíče** otevře se okno, registrační klíč služby. Zkopírujte hello registrační klíč pomocí ikona kopírování hello. 

Registrační klíč služby hello mějte do bezpečného umístění. Budete potřebovat tento klíč, a také šifrovacího klíče dat služby hello, tooregister další zařízení s touto službou. Po získání registračního klíče služby hello, budete potřebovat tooconfigure zařízení prostřednictvím hello Windows Powershellu pro StorSimple rozhraní.

## <a name="regenerate-hello-service-registration-key"></a>Znovu vygenerovat registrační klíč služby hello
Je nutné tooregenerate registrační klíč služby, pokud jsou požadované tooperform střídání klíče nebo pokud došlo ke změně hello seznamu správců služeb. Pokud jste znovu vygenerovat klíč hello, hello nový klíč slouží pouze k registraci dalších zařízení. Hello zařízení, které již byly zaregistrovány nejsou ovlivněny tímto procesem.

Proveďte následující kroky tooregenerate registrační klíč služby hello.

#### <a name="tooregenerate-hello-service-registration-key"></a>registrační klíč služby hello tooregenerate
1. V hello **Manager zařízení StorSimple** okně přejděte příliš**správy &gt;**  **klíče**.
   
   ![Okno Klíče](./media/storsimple-virtual-array-manage-service/getregkey2.png)
2. V hello **klíče** okně klikněte na tlačítko **znovu vygenerovat**.
   
   ![Klikněte na tlačítko znovu vygenerovat](./media/storsimple-virtual-array-manage-service/getregkey5.png)
3. V hello **registrační klíč znovu generovat služby** okně zkontrolujte hello akce při vyžaduje hello klíče se generují. Všechny následné zařízení hello, které jsou registrované s touto službou použije hello nový registrační klíč. Klikněte na tlačítko **znovu vygenerovat** tooconfirm. Po dokončení registrace hello, budete upozorněni.
   
   ![Zkontrolujte znovu generovat klíče](./media/storsimple-virtual-array-manage-service/getregkey3.png)
4. Zobrazí se nový registrační klíč služby.
   
    ![Zkontrolujte znovu generovat klíče](./media/storsimple-virtual-array-manage-service/getregkey4.png)
   
   Zkopírujte tento klíč a uložit ho pro registraci nové zařízení s touto službou.

## <a name="next-steps"></a>Další kroky
* Zjistěte, jak příliš[Začínáme](storsimple-virtual-array-deploy1-portal-prep.md) s polem virtuální zařízení StorSimple.
* Zjistěte, jak příliš[spravovat zařízení StorSimple](storsimple-ova-web-ui-admin.md).

