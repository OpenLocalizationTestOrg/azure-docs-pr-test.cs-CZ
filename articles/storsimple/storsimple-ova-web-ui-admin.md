---
title: "aaaStorSimple virtuální pole webového uživatelského rozhraní správy | Microsoft Docs"
description: "Popisuje, jak Správa základní zařízení tooperform úkoly hello pole virtuální zařízení StorSimple webového uživatelského rozhraní."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: ea65b4c7-a478-43e6-83df-1d9ea62916a6
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 12/1/2016
ms.author: alkohli
ms.openlocfilehash: 31a20a587c4302231f027fcf772a50df33b23407
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-web-ui-tooadminister-your-storsimple-virtual-array"></a>Použití webového uživatelského rozhraní tooadminister hello pole virtuální zařízení StorSimple
![tok procesu instalace](./media/storsimple-ova-web-ui-admin/manage4.png)

## <a name="overview"></a>Přehled
kurzy Hello v tomto článku se vztahují toohello Microsoft Azure StorSimple virtuální pole (také označované jako hello místní virtuální zařízení StorSimple) spuštěné. března 2016 obecné dostupnosti (GA) verze. Tento článek popisuje některé hello komplexní pracovní postupy a úlohy správy, které lze provést na hello pole virtuální zařízení StorSimple. Můžete spravovat hello poli virtuální zařízení StorSimple pomocí služby StorSimple Manager hello uživatelského rozhraní (označují tooas hello portál uživatelského rozhraní) a hello místního webového uživatelského rozhraní pro hello zařízení. Tento článek se zaměřuje na hello úlohy, že můžete provádět pomocí hello webového uživatelského rozhraní.

Tento článek obsahuje hello následující kurzy:

* Získání šifrovacího klíče dat služby hello
* Řešení chyb instalace webového uživatelského rozhraní
* Generovat balíček protokolu
* Vypnutí nebo restartování zařízení

## <a name="get-hello-service-data-encryption-key"></a>Získání šifrovacího klíče dat služby hello
Šifrovací klíč dat služby se vygeneruje, když se zaregistrujete svoje první zařízení hello služby StorSimple Manager. Tento klíč je pak potřeba s hello služby registrace klíče tooregister další zařízení s hello služby StorSimple Manager.

Pokud jste k jejich chybnému umístění šifrovacího klíče dat služby a potřebovat tooretrieve, proveďte následující hello kroky v hello místního webového uživatelského rozhraní hello zařízení zaregistrovali služby.

#### <a name="tooget-hello-service-data-encryption-key"></a>šifrovací klíč dat služby tooget hello
1. Připojte toohello místního webového uživatelského rozhraní. Přejděte příliš**konfigurace** > **nastavení cloudu**.
2. V dolní části hello hello stránky, klikněte na tlačítko **šifrovacího klíče dat služby Get**. Zobrazí se klíč. Zkopírujte a uložte tento klíč.
   
    ![získání šifrovacího klíče dat služby 1](./media/storsimple-ova-web-ui-admin/image27.png)

## <a name="troubleshoot-web-ui-setup-errors"></a>Řešení chyb instalace webového uživatelského rozhraní
V některých případech při konfiguraci zařízení hello hello místního webového uživatelského rozhraní, může dojít k chybám. toodiagnose a vyřešte tyto chyby, je možné spustit testy diagnostiky hello.

#### <a name="toorun-hello-diagnostic-tests"></a>toorun hello diagnostických testů
1. Hello místního webového uživatelského rozhraní, přejděte v příliš**Poradce při potížích s** > **diagnostických testů**.
   
    ![spustit diagnostiku 1](./media/storsimple-ova-web-ui-admin/image29.png)
2. V dolní části hello hello stránky, klikněte na tlačítko **spuštění diagnostických testů**. Tím zahájíte testy toodiagnose všech možných problémů s sítí, zařízení, webový proxy server, čas nebo nastavení cloudu. Budete informováni, že toto zařízení hello zpracovává testy.
3. Po dokončení testů hello, zobrazí se výsledky hello. Hello následující příklad ukazuje výsledky hello diagnostických testů. Upozorňujeme, že nebyly na toto zařízení nakonfigurované nastavení proxy serveru webové hello a proto nebyl spuštěn test proxy webu hello. Všechny hello jiné testy pro nastavení sítě, DNS server a nastavení času byly úspěšné.
   
    ![spustit diagnostiku 2](./media/storsimple-ova-web-ui-admin/image30.png)

## <a name="generate-a-log-package"></a>Generovat balíček protokolu
Balíček protokol se skládá z všechny relevantní protokoly hello, které mohou pomoci Microsoft Support s potíží jakékoli zařízení. V této verzi můžete generovat balíček protokolu pomocí hello místního webového uživatelského rozhraní.

#### <a name="toogenerate-hello-log-package"></a>toogenerate hello protokolu balíčku
1. Hello místního webového uživatelského rozhraní, přejděte v příliš**Poradce při potížích s** > **protokoly systému**.
   
    ![Generovat balíček protokolu 1](./media/storsimple-ova-web-ui-admin/image31.png)
2. V dolní části hello hello stránky, klikněte na tlačítko **vytvořit balíček protokolu**. Bude vytvořen balíček protokolů systému hello. Bude to trvat několik minut.
   
    ![Generovat balíček protokolu 2](./media/storsimple-ova-web-ui-admin/image32.png)
   
    Po hello balíček je úspěšně vytvořen a hello stránka bude aktualizována tooindicate hello čas a datum vytvoření balíčku hello, budete upozorněni.
   
    ![Generovat balíček protokolu 3](./media/storsimple-ova-web-ui-admin/image33.png)
3. Klikněte na tlačítko **balíček ke stažení protokolů**. Komprimované balíčku budou staženy ve vašem systému.
   
    ![Generovat balíček protokolu 4](./media/storsimple-ova-web-ui-admin/image34.png)
4. Můžete rozbalte balíček stažené protokolu hello a zobrazit hello systémových souborech protokolu.

## <a name="shut-down-and-restart-your-device"></a>Vypnutí a restartování zařízení
Můžete vypnout nebo restartovat virtuální zařízení pomocí místní hello webového uživatelského rozhraní. Doporučujeme, aby před restartovat, trvat hello svazky nebo sdílené složky do offline režimu na hostiteli hello a pak hello zařízení. Tím se minimalizují možnost poškození dat. 

#### <a name="tooshut-down-your-virtual-device"></a>tooshut dolů virtuálního zařízení
1. Hello místního webového uživatelského rozhraní, přejděte v příliš**údržby** > **nastavení napájení**.
2. V dolní části hello hello stránky, klikněte na tlačítko **vypnutí**.
   
    ![vypnutí zařízení 1](./media/storsimple-ova-web-ui-admin/image36.png)
3. Zobrazí se upozornění, s informacemi o tom, že vypnutí hello zařízení přeruší všechny vstupně-výstupní operace které byly v průběhu, což vede výpadku. Klikněte na ikonu zaškrtnutí hello ![ikona zaškrtnutí](./media/storsimple-ova-web-ui-admin/image3.png).
   
    ![upozornění vypnutí zařízení](./media/storsimple-ova-web-ui-admin/image37.png)
   
    Budete informováni, že tento vypnout hello byla inicializována.
   
    ![vypnutí zařízení spuštění](./media/storsimple-ova-web-ui-admin/image38.png)
   
    Hello zařízení bude nyní ukončena. Pokud chcete toostart zařízení, budete potřebovat toodo, která prostřednictvím hello Správce technologie Hyper-V.

#### <a name="toorestart-your-virtual-device"></a>toorestart virtuální zařízení
1. Hello místního webového uživatelského rozhraní, přejděte v příliš**údržby** > **nastavení napájení**.
2. V dolní části hello hello stránky, klikněte na tlačítko **restartujte**.
   
    ![restartování zařízení](./media/storsimple-ova-web-ui-admin/image36.png)
3. Zobrazí se upozornění, oznamující, že toto restartování zařízení hello přeruší všechny IOs, které byly v průběhu, což vede výpadku. Klikněte na ikonu zaškrtnutí hello ![ikona zaškrtnutí](./media/storsimple-ova-web-ui-admin/image3.png).
   
    ![upozornění na restartování](./media/storsimple-ova-web-ui-admin/image37.png)
   
    Budete informováni, bylo zahájeno restartování hello.
   
    ![restartování, inicializovat](./media/storsimple-ova-web-ui-admin/image39.png)
   
    Když probíhá hello restartování, dojde ke ztrátě připojení toohello hello uživatelského rozhraní. Pravidelně aktualizujte hello uživatelského rozhraní můžete monitorovat hello restartování. Alternativně můžete monitorovat stav restartování zařízení hello prostřednictvím hello Správce technologie Hyper-V.

## <a name="next-steps"></a>Další kroky
Zjistěte, jak příliš[použití hello toomanage služby StorSimple Manager zařízení](storsimple-virtual-array-manager-service-administration.md).

