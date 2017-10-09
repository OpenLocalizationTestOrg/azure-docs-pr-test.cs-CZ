---
title: "aaaChange hesla služby StorSimple | Microsoft Docs"
description: "Popisuje, jak toouse hello toochange služby StorSimple Manager zařízení StorSimple Snapshot Manager a zařízení hesla správce."
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
ms.date: 07/03/2017
ms.author: alkohli
ms.openlocfilehash: cf884be31b4bbf9e372c0aa11b9da2eadcda35dd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-toochange-your-storsimple-passwords"></a>Použít toochange služby StorSimple Manager zařízení hello hesla služby StorSimple

## <a name="overview"></a>Přehled
portál Azure Hello **nastavení zařízení** možnost obsahuje všechny parametry hello zařízení, které můžete změnit konfiguraci na zařízení StorSimple, který je spravovaný nástrojem service Manager zařízení StorSimple. Tento kurz vysvětluje, jak je možné používat hello **zabezpečení** možnost pod **nastavení zařízení** toochange Správce zařízení nebo heslo Snapshot Manager zařízení StorSimple.

## <a name="change-hello-device-administrator-password"></a>Heslo správce zařízení hello změn
Pokud používáte zařízení StorSimple hello tooaccess rozhraní Windows PowerShell, se vyžaduje tooenter hesla správce zařízení. Pokud služba není zaregistrována hello prvního zařízení StorSimple, hello výchozí heslo pro toto rozhraní je *Heslo1*. Hello zabezpečení vašich dat, se vyžaduje toochange toto heslo v hello konci procesu registrace hello. Nelze ukončit proces registrace hello beze změny toto heslo. Další informace najdete v tématu [krok 3: Konfigurace a registrace zařízení hello pomocí Windows Powershellu pro StorSimple](storsimple-8000-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).

Hello heslo, které se nejprve nastavit pomocí rozhraní Windows PowerShell hello během registrace může prostřednictvím portálu Azure hello později změnit. Proveďte následující heslo správce zařízení hello toochange kroky hello.

#### <a name="toochange-hello-device-administrator-password"></a>heslo správce zařízení toochange hello
1. Přejděte služby StorSimple Manager zařízení tooyour a klikněte na tlačítko **zařízení**.

2. Hello tabulkové seznam všech zařízení, vyberte a klikněte na hello zařízení, jehož heslo chcete toochange.

    ![](./media/storsimple-8000-change-passwords/changepwd1.png)

3. V hello **nastavení** okně přejděte příliš**nastavení zařízení > zabezpečení**.

    ![](./media/storsimple-8000-change-passwords/changepwd2.png)

4. V hello **nastavení zabezpečení** okně klikněte na tlačítko **heslo** hesla správce zařízení toochange hello.

    ![](./media/storsimple-8000-change-passwords/changepwd3.png)

5. V hello **heslo** okno, zadejte heslo správce, který obsahuje z 8 znaků too15. Hello heslo musí být kombinací 3 nebo více velká písmena, malá písmena, číselné a speciální znaky.

6. Potvrzení hesla hello.

    ![](./media/storsimple-8000-change-passwords/changepwd4.png)

7. Klikněte na tlačítko **Uložit** a po zobrazení výzvy k potvrzení, klikněte na tlačítko **Ano**.

    ![](./media/storsimple-8000-change-passwords/changepwd6.png)

Nyní je třeba aktualizovat heslo správce zařízení Hello. Můžete použít rozhraní Windows PowerShell hello tooaccess této změny hesla.

## <a name="set-hello-storsimple-snapshot-manager-password"></a>Nastavení hesla Snapshot Manageru zařízení StorSimple hello
Software Snapshot Manager zařízení StorSimple se nachází na hostiteli s Windows a umožňuje správci toomanage zálohy zařízení StorSimple ve formě hello místních a cloudových snímků.

Při konfiguraci zařízení ve Snapshot Manageru zařízení StorSimple, zobrazí se výzva tooprovide hello zařízení IP adresu a heslo tooauthenticate zařízení úložiště.

Můžete nastavit nebo změnit hello heslo pro StorSimple Snapshot Manager prostřednictvím hello portálu Azure. Proveďte následující kroky tooset hello nebo změnit heslo Snapshot Manager zařízení StorSimple hello.

#### <a name="tooset-hello-storsimple-snapshot-manager-password"></a>heslo Snapshot Manager zařízení StorSimple tooset hello
1. Přejděte služby StorSimple Manager zařízení tooyour a klikněte na tlačítko **zařízení**.

2. Hello tabulkové seznam všech zařízení, vyberte a klikněte na zařízení hello jehož heslo Snapshot Manager zařízení StorSimple úmyslu tooset nebo změnit.

     ![](./media/storsimple-8000-change-passwords/changepwd1.png)

3. V hello **nastavení** okně přejděte příliš**nastavení zařízení > zabezpečení**.

     ![](./media/storsimple-8000-change-passwords/changepwd2.png)

4. V hello **nastavení zabezpečení** okně klikněte na tlačítko **heslo** tooset nebo změňte heslo Snapshot Manager zařízení StorSimple hello.

     ![](./media/storsimple-8000-change-passwords/changepwd3.png) 

5. V hello **heslo** okno, zadejte heslo, které je tvořeno 14 až 15 znaků. Zajistěte, aby že toto heslo hello obsahuje kombinaci 3 nebo více velká písmena, malá písmena, číselné a speciální znaky.

6. Potvrzení hesla hello.

     ![](./media/storsimple-8000-change-passwords/changepwd5.png)

7. Klikněte na tlačítko **Uložit** a po zobrazení výzvy k potvrzení, klikněte na tlačítko **Ano**.

     ![](./media/storsimple-8000-change-passwords/changepwd6.png)

Nyní je třeba aktualizovat heslo Snapshot Manager zařízení StorSimple Hello.

## <a name="next-steps"></a>Další kroky
* Další informace o [zabezpečení zařízení StorSimple](storsimple-8000-security.md).
* Další informace o [úprava konfiguraci zařízení](storsimple-8000-modify-device-config.md).
* Další informace o [pomocí hello tooadminister service Manager zařízení StorSimple zařízení StorSimple](storsimple-8000-manager-service-administration.md).

