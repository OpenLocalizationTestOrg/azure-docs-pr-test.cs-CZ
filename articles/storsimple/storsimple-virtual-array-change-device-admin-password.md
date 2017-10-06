---
title: "heslo správce zařízení aaaChange pole virtuální zařízení StorSimple | Microsoft Docs"
description: "Popisuje, jak toouse buď hello portálu Azure nebo hesla správce zařízení toochange pole virtuální zařízení StorSimple webového uživatelského rozhraní hello."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 11490814-d9fd-4dc7-9c3b-55dd2c23eaf1
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 531b395df7aeade0a909360797c6b0f0abd9fd1e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="change-hello-storsimple-virtual-array-device-administrator-password-via-storsimple-device-manager"></a>Změna hesla správce zařízení StorSimple virtuální pole hello pomocí Správce zařízení StorSimple

## <a name="overview"></a>Přehled

Při použití hello prostředí Windows PowerShell rozhraní tooaccess hello pole virtuální zařízení StorSimple, jsou požadované tooenter hesla správce zařízení. Pokud zařízení StorSimple hello je nejdřív zřízený a spuštěna, hello výchozí heslo je *Heslo1*. Hello zabezpečení vašich dat, hello výchozí heslo vyprší hello při prvním přihlášení a jsou požadované toochange toto heslo.

Buď hello místního webového uživatelského rozhraní nebo hello Azure portálu toochange hello hesla správce zařízení můžete taky kdykoli po nasazení hello zařízení v provozním prostředí. Každá z těchto postupů je popsána v tomto článku.

 ![Okno zařízení](./media/storsimple-virtual-array-change-device-admin-password/ova-devices-blade.png)

## <a name="use-hello-azure-portal-toochange-hello-password"></a>Použít heslo hello Azure portálu toochange hello

Proveďte následující kroky správce heslo zařízení hello toochange prostřednictvím portálu Azure hello hello.

#### <a name="toochange-hello-device-administrator-password-via-hello-azure-portal"></a>heslo správce zařízení toochange hello prostřednictvím hello portálu Azure

1. Na cílovou stránku hello služby, vyberte svoji službu, klikněte dvakrát na hello název služby a potom v rámci hello **správy** klikněte na tlačítko **zařízení**. Tím se otevře hello **zařízení** okno, které obsahuje seznamy všech zařízení StorSimple virtuální pole.

2. V hello **zařízení** okno, klikněte dvakrát na hello zařízení, která vyžaduje změnu hesla.

3. V hello **nastavení** pro zařízení, klikněte na **zabezpečení**.

4. V hello **nastavení zabezpečení** okně hello následující:
   
   1. Projděte dolů toohello **heslo správce zařízení** části. Zadejte heslo správce, který obsahuje z 8 znaků too15.
   2. Potvrzení hesla hello.
   3. Klikněte na tlačítko **Uložit** hello horní části okna hello.

heslo správce zařízení Hello se nyní aktualizuje. Toto zařízení hello tooaccess upravené heslo můžete místně.

![Okno nastavení zabezpečení](./media/storsimple-virtual-array-change-device-admin-password/ova-change-device-pwd.png)

## <a name="use-hello-local-web-ui-toochange-hello-password"></a>Použijte hello místního webového uživatelského rozhraní toochange hello heslo

Proveďte následující kroky správce heslo zařízení hello toochange hello místního webového uživatelského rozhraní hello.

#### <a name="toochange-hello-device-administrator-password-via-hello-local-web-ui"></a>heslo správce zařízení toochange hello prostřednictvím hello místního webového uživatelského rozhraní

1. V hello místního webového uživatelského rozhraní, klikněte na **údržby** > **Změna hesla** pro vaše zařízení.
   
    ![změnit Heslo1](./media/storsimple-virtual-array-change-device-admin-password/image40.png)
2. Zadejte hello **aktuální heslo**.
3. Zadejte **nové heslo**. Hello heslo musí být dlouhé alespoň 8 znaků. Musí obsahovat 3 4 následující hello: velká písmena, malá písmena, číselné a speciální znaky.
   
    Všimněte si, že vaše heslo nemůže být hello stejná hodnota jako hello posledních 24 hesla.
4. Zadejte znovu heslo tooconfirm hello ho.
   
    ![změnit Heslo2](./media/storsimple-virtual-array-change-device-admin-password/image41.png)
5. V dolní části hello hello stránky, klikněte na tlačítko **použít**. nové heslo Hello se teď použijí. Změna hesla hello neproběhne úspěšně, zobrazí hello následující chybě:
   
    ![Chyba hesla](./media/storsimple-virtual-array-change-device-admin-password/image42.png)
   
    Po úspěšné aktualizaci hello heslo, budete upozorněni. Pak můžete toto zařízení hello tooaccess upravené heslo místně.


## <a name="next-steps"></a>Další kroky
Zjistěte, jak příliš[spravovat vaše pole virtuální zařízení StorSimple](storsimple-ova-web-ui-admin.md).

