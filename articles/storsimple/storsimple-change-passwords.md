---
title: "aaaChange hesla pomocí Správce zařízení StorSimple | Microsoft Docs"
description: "Popisuje, jak toouse hello toochange služby StorSimple Manager správce hesla služby StorSimple Snapshot Manager a zařízení."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: f178509c-f4e1-48a8-90b2-d4ad050eeb30
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: b2836eb4d3a05e1d2a5eeeeefe66c75f63ba38ed
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-toochange-your-storsimple-passwords"></a>Použít toochange služby StorSimple Manager hello hesla služby StorSimple
## <a name="overview"></a>Přehled
portál Azure classic Hello **konfigurace** stránka obsahuje všechny parametry hello zařízení, které můžete změnit konfiguraci na zařízení StorSimple, který je spravovaný nástrojem služby StorSimple Manager. Tento kurz vysvětluje, jak je možné používat hello **konfigurace** stránka toochange Správce zařízení nebo heslo Snapshot Manager zařízení StorSimple.

## <a name="change-hello-device-administrator-password"></a>Heslo správce zařízení hello změn
Pokud používáte zařízení StorSimple hello tooaccess rozhraní Windows PowerShell, se vyžaduje tooenter hesla správce zařízení. Pokud služba není zaregistrována hello prvního zařízení StorSimple, hello výchozí heslo pro toto rozhraní je *Heslo1*. Hello zabezpečení vašich dat, se vyžaduje toochange toto heslo v hello konci procesu registrace hello. Nelze ukončit proces registrace hello beze změny toto heslo. Další informace najdete v tématu [krok 3: Konfigurace a registrace zařízení hello pomocí Windows Powershellu pro StorSimple](storsimple-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).

Hello heslo, které se nejprve nastavit pomocí rozhraní Windows PowerShell hello během registrace lze potom změnit prostřednictvím hello portál Azure classic. Proveďte následující heslo správce zařízení hello toochange kroky hello.

#### <a name="toochange-hello-device-administrator-password"></a>heslo správce zařízení toochange hello
1. Na portálu classic hello, klikněte na tlačítko **zařízení** > **konfigurace** pro vaše zařízení.
2. Projděte dolů toohello **heslo správce zařízení** části. Zadejte heslo správce, který obsahuje z 8 znaků too15. Hello heslo musí být kombinací 3 nebo více velká písmena, malá písmena, číselné a speciální znaky.
3. Potvrzení hesla hello.
4. Klikněte na tlačítko **Uložit** v hello dolní části stránky hello.

Nyní je třeba aktualizovat heslo správce zařízení Hello. Můžete použít rozhraní Windows PowerShell hello tooaccess této změny hesla.

## <a name="change-hello-storsimple-snapshot-manager-password"></a>Změnit heslo Snapshot Manager zařízení StorSimple hello
Software Snapshot Manager zařízení StorSimple se nachází na hostiteli s Windows a umožňuje správci toomanage zálohy zařízení StorSimple ve formě hello místních a cloudových snímků.

Při konfiguraci zařízení ve Snapshot Manageru zařízení StorSimple, zobrazí se výzva tooprovide hello zařízení IP adresu a heslo tooauthenticate zařízení úložiště. Toto heslo je nejprve nakonfigurovat přes rozhraní Windows PowerShell hello. Další informace najdete v tématu [krok 3: Konfigurace a registrace zařízení hello pomocí Windows Powershellu pro StorSimple](storsimple-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).

Hello heslo, které se nejprve nastavit pomocí rozhraní Windows PowerShell hello během registrace lze potom změnit prostřednictvím portálu classic hello. Proveďte následující kroky toochange hello StorSimple Snapshot Manager heslo hello.

#### <a name="toochange-hello-storsimple-snapshot-manager-password"></a>heslo Snapshot Manager zařízení StorSimple toochange hello
1. Na portálu classic hello, klikněte na tlačítko **zařízení** > **konfigurace** pro vaše zařízení.
2. Projděte dolů toohello **StorSimple Snapshot Manager** části. Zadejte heslo, které je tvořeno 14 až 15 znaků. Zajistěte, aby že toto heslo hello obsahuje kombinaci 3 nebo více velká písmena, malá písmena, číselné a speciální znaky.
3. Potvrzení hesla hello.
4. Klikněte na tlačítko **Uložit** v hello dolní části stránky hello.

Nyní je třeba aktualizovat heslo Snapshot Manager zařízení StorSimple Hello.

## <a name="next-steps"></a>Další kroky
* Další informace o [zabezpečení zařízení StorSimple](storsimple-security.md).
* Další informace o [úprava konfiguraci zařízení](storsimple-modify-device-config.md).
* Další informace o [pomocí hello tooadminister služby StorSimple Manager zařízení StorSimple](storsimple-manager-service-administration.md).

