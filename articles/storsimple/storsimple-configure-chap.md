---
title: "aaaConfigure CHAP pro zařízení řady StorSimple 8000 | Microsoft Docs"
description: "Popisuje, jak tooconfigure hello Challenge Handshake Authentication Protocol (CHAP) na zařízení StorSimple."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 467044d7-7885-4382-90bd-3148dbbd341f
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: TBD
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: 272ef2c184f56ad262e55410357494c72e45cf83
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-chap-for-your-storsimple-device"></a>Konfigurace protokolů CHAP pro vaše zařízení StorSimple
Tento kurz popisuje, jak tooconfigure CHAP pro vaše zařízení StorSimple. Hello postup popsaný v tomto článku se vztahuje tooStorSimple řady 8000 a také zařízení StorSimple 1200.

CHAP znamená Challenge Handshake Authentication Protocol. Je příslušné schéma ověřování používané servery toovalidate hello identitu vzdálených klientů. ověření Hello je založena na sdílené heslo nebo tajný klíč. CHAP může být jednosměrná (Jednosměrný) nebo vzájemné (obousměrné). Jednosměrné CHAP je při hello cíl ověření iniciátor. Vzájemné nebo zpětného CHAP na hello druhé straně, vyžaduje, aby hello cíl ověření hello iniciátor a pak hello iniciátor ověření hello cíl. Iniciátor ověřování se dá implementovat bez cíl ověřování. Cíl ověřování však můžete implementovat, pouze v případě, že se také implementuje iniciátor ověřování. 

Jako osvědčený postup doporučujeme použít CHAP tooenhance iSCSI zabezpečení.

> [!NOTE]
> Uvědomte si, že protokol IPSEC není aktuálně podporován zařízení StorSimple.
> 
> 

Hello CHAP nastavení v zařízení StorSimple hello se dá nakonfigurovat v hello následující způsoby:

* Jednosměrný nebo jednosměrné ověřování
* Obousměrné nebo vzájemné nebo zpětného ověřování

V každé z těchto případů musí hello portál pro hello zařízení a softwarem iniciátoru iSCSI server hello toobe nakonfigurované. Hello podrobné pokyny pro tuto konfiguraci jsou popsány v následujícím kurzu hello.

## <a name="unidirectional-or-one-way-authentication"></a>Jednosměrný nebo jednosměrné ověřování
V jednosměrný ověřování ověřuje hello cíl hello iniciátor. Toto ověřování vyžaduje, konfigurovat nastavení iniciátoru CHAP hello v zařízení StorSimple hello a hello iSCSI software Initiator na hostiteli hello. Hello podrobné postupy pro zařízení StorSimple a hostitele Windows jsou popsané dále.

#### <a name="tooconfigure-your-device-for-one-way-authentication"></a>tooconfigure zařízení pro jednosměrné ověřování
1. V portálu Azure classic, na hello hello **zařízení** klikněte na tlačítko hello **konfigurace** kartě.
   
    ![Iniciátoru CHAP](./media/storsimple-configure-chap/IC740943.png)
2. Posuňte se dolů na této stránce a hello **iniciátoru CHAP** části:
   
   1. Zadejte uživatelské jméno pro vaše iniciátoru CHAP.
   2. Zadejte heslo pro vaše iniciátoru CHAP.
      
    > [!IMPORTANT]
    > Hello CHAP uživatelské jméno musí obsahovat méně než 233 znaků. Hello CHAP heslo musí být 12 až 16 znaků. Delší uživatelské jméno nebo heslo bude mít za následek selhání ověřování na hostitele Windows hello.
   
   3. Potvrzení hesla hello.
3. Klikněte na **Uložit**. Zobrazí potvrzovací zpráva. Klikněte na tlačítko **OK** toosave hello změny.

#### <a name="tooconfigure-one-way-authentication-on-hello-windows-host-server"></a>tooconfigure jednosměrné ověřování v systému Windows hello hostitelském serveru
1. Na serveru hostitele Windows hello spusťte iniciátor iSCSI hello.
2. V hello **vlastnosti iniciátoru iSCSI** okně provést hello následující kroky:
   
   1. Klikněte na tlačítko hello **zjišťování** kartě.
      
       ![Vlastnosti iniciátoru iSCSI](./media/storsimple-configure-chap/IC740944.png)
   2. Klikněte na tlačítko **zjistit portál**.
3. V hello **zjistit cílový portál** dialogové okno:
   
   1. Zadejte IP adresu hello vašeho zařízení.
   2. Klikněte na tlačítko **rozšířené**.
      
       ![Zjistit cílový portál](./media/storsimple-configure-chap/IC740945.png)
4. V hello **Upřesnit nastavení** dialogové okno:
   
   1. Vyberte hello **povolit CHAP přihlásit** zaškrtávací políčko.
   2. V hello **název** pole, zadejte hello uživatelské jméno, které jste zadali pro hello iniciátoru CHAP portálu classic hello.
   3. V hello **tajný klíč cíle** pole, zadejte hello heslo, které jste zadali pro hello iniciátoru CHAP portálu classic hello.
   4. Klikněte na **OK**.
      
       ![Upřesňující nastavení, obecné](./media/storsimple-configure-chap/IC740946.png)
5. Na hello **cíle** kartě hello **vlastnosti iniciátoru iSCSI** okně hello stav zařízení mají zobrazit jako **připojeno**. Pokud používáte zařízení StorSimple 1200, pak každý svazek se připojí jako cíl iSCSI jak je uvedeno níže. Proto nutné kroky 3 a 4 toobe opakuje pro každý svazek.
   
    ![Svazky připojené jako samostatné cíle](./media/storsimple-configure-chap/chap4.png)
   
   > [!IMPORTANT]
   > Pokud změníte název iSCSI hello, hello nový název se použije pro nové relace iSCSI. Nové nastavení nejsou znovu použít pro existující až do odhlášení a přihlášení.
   > 
   > 

Další informace o konfiguraci CHAP na hostitelském serveru Windows hello přejděte příliš[další aspekty](#additional-considerations).

## <a name="bidirectional-or-mutual-authentication"></a>Obousměrné nebo vzájemné ověřování
V obousměrné ověřování hello cíl ověřuje hello iniciátor a pak hello iniciátor ověřuje hello cíl. To vyžaduje nastavení iniciátoru CHAP tooconfigure hello aplikace hello uživatele, jakož i hello reverse CHAP nastavení na zařízení hello a iSCSI software Initiator na hostiteli hello. Hello následující postupy popisují hello kroky tooconfigure vzájemné ověřování na hello zařízení a na hostitele Windows hello.

#### <a name="tooconfigure-your-device-for-mutual-authentication"></a>tooconfigure zařízení pro vzájemné ověřování
1. V portálu Azure classic, na hello hello **zařízení** klikněte na tlačítko hello **konfigurace** kartě.
   
    ![Cíle CHAP](./media/storsimple-configure-chap/IC740948.png)
2. Posuňte se dolů na této stránce a hello **CHAP cíl** části:
   
   1. Zadejte **Reverse CHAP uživatelské jméno** pro vaše zařízení.
   2. Zadejte **hesla Reverse CHAP** pro vaše zařízení.
   3. Potvrzení hesla hello.
3. V hello **iniciátoru CHAP** části:
   
   1. Zadejte **uživatelské jméno** pro vaše zařízení.
   2. Zadejte **heslo** pro vaše zařízení.
   3. Potvrzení hesla hello.
4. Klikněte na **Uložit**. Zobrazí potvrzovací zpráva. Klikněte na tlačítko **OK** toosave hello změny.

#### <a name="tooconfigure-bidirectional-authentication-on-hello-windows-host-server"></a>tooconfigure obousměrné ověřování v systému Windows hello hostitelském serveru
1. Na serveru hostitele Windows hello spusťte iniciátor iSCSI hello.
2. V hello **vlastnosti iniciátoru iSCSI** okně klikněte na tlačítko hello **konfigurace** kartě.
3. Klikněte na tlačítko **CHAP**.
4. V hello **tajný klíč pro vzájemné CHAP iniciátor iSCSI** dialogové okno:
   
   1. Typ hello **hesla Reverse CHAP** který jste nakonfigurovali v hello portál Azure classic.
   2. Klikněte na **OK**.
      
       ![iSCSI initiator vzájemné CHAP tajný klíč](./media/storsimple-configure-chap/IC740949.png)
5. Klikněte na tlačítko hello **cíle** kartě.
6. Klikněte na tlačítko hello **Connect** tlačítko. 
7. V hello **připojit tooTarget** dialogové okno, klikněte na tlačítko **Upřesnit**.
8. V hello **Upřesnit vlastnosti** dialogové okno:
   
   1. Vyberte hello **povolit CHAP přihlásit** zaškrtávací políčko.
   2. V hello **název** pole, zadejte hello uživatelské jméno, které jste zadali pro hello iniciátoru CHAP portálu classic hello.
   3. V hello **tajný klíč cíle** pole, zadejte hello heslo, které jste zadali pro hello iniciátoru CHAP portálu classic hello.
   4. Vyberte hello **provést vzájemné ověřování** zaškrtávací políčko.
      
       ![Upřesnit nastavení vzájemného ověření](./media/storsimple-configure-chap/IC740950.png)
   5. Klikněte na tlačítko **OK** toocomplete hello CHAP konfigurace

Další informace o konfiguraci CHAP na hostitelském serveru Windows hello přejděte příliš[další aspekty](#additional-considerations).

## <a name="additional-considerations"></a>Další aspekty
Hello **rychlé připojení** funkce nepodporuje připojení, které mají povolené CHAP. Pokud je povoleno CHAP, ujistěte se, že používáte hello **připojit** tlačítko, která je dostupná na hello **cíle** kartě tooconnect tooa cíl.

![Připojit tootarget](./media/storsimple-configure-chap/IC740947.png)

V hello **připojit tooTarget** dialogu, který je vidění, vyberte hello **přidat tato připojení toohello seznam oblíbených cíle** zaškrtávací políčko. Tím se zajistí, že při každém restartování počítače hello, je proveden pokus o toorestore hello připojení toohello iSCSI Oblíbené cíle.

## <a name="errors-during-configuration"></a>Chyby během konfigurace
Pokud vaše konfigurace CHAP je nesprávná, pak se pravděpodobně toosee **selhání ověření** chybová zpráva.

## <a name="verification-of-chap-configuration"></a>Ověření konfigurace CHAP
Můžete ověřit, jestli se používá protokol CHAP provedením následujících kroků hello.

#### <a name="tooverify-your-chap-configuration"></a>tooverify konfiguraci CHAP
1. Klikněte na tlačítko **Oblíbené cíle**.
2. Vyberte cíl hello, pro které jste povolili ověřování.
3. Klikněte na tlačítko **podrobnosti**.
   
    ![Oblíbené cíle vlastnosti iniciátoru iSCSI](./media/storsimple-configure-chap/IC740951.png)
4. V hello **Oblíbené podrobnosti cíl** dialogové okno, Poznámka hello položku v hello **ověřování** pole. Pokud hello konfigurace proběhla úspěšně, by mělo být uvedeno **CHAP**.
   
    ![Podrobnosti o oblíbeného cíle](./media/storsimple-configure-chap/IC740952.png)

## <a name="next-steps"></a>Další kroky
* Další informace o [zabezpečení zařízení StorSimple](storsimple-security.md).
* Další informace o [pomocí hello tooadminister služby StorSimple Manager zařízení StorSimple](storsimple-manager-service-administration.md).

