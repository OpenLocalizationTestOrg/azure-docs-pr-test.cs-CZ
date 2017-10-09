---
title: "řadiče zařízení řady StorSimple 8000 aaaManage | Microsoft Docs"
description: "Zjistěte, jak toostop, restartovat, vypnout nebo obnovit vaše řadiče zařízení StorSimple."
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
ms.date: 06/19/2017
ms.author: alkohli
ms.openlocfilehash: 5c59582b7ccf7cfeae9e7efbd0e4df9dc1d3871c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-your-storsimple-device-controllers"></a>Spravovat vaše řadiče zařízení StorSimple

## <a name="overview"></a>Přehled

Tento kurz popisuje hello různé operace, které lze provést na řadiče zařízení StorSimple. Hello řadiče v zařízení StorSimple jsou řadiče redundantní (sdílené) v konfiguraci aktivní pasivní. V daném okamžiku jenom jeden řadič je aktivní a zpracovává všechny operace diskových a síťových hello. Hello jiný řadič je v pasivním režimu. Pokud se řadič active hello nezdaří, pasivní řadiče hello automaticky stane aktivní.

Tento kurz obsahuje podrobné pokyny toomanage hello zařízení řadiče pomocí:

* **Řadiče** okna pro vaše zařízení v hello služby StorSimple Manager zařízení.
* Prostředí Windows PowerShell pro StorSimple.

Doporučujeme vám, že spravujete hello řadiče zařízení prostřednictvím služby StorSimple Manager zařízení hello. Pokud se akce lze provést pouze pomocí prostředí Windows PowerShell pro StorSimple, hello kurzu provede poznamenejte si ho.

Po přečtení tohoto kurzu, budete moci:

* Restartování nebo vypnutí řadič zařízení StorSimple
* Vypněte zařízení StorSimple
* Obnovit výchozí toofactory zařízení StorSimple

## <a name="restart-or-shut-down-a-single-controller"></a>Restartování nebo vypnutí jediného kontroleru
Řadič restartování nebo vypnutí není vyžadován jako součást operace normální systému. Operace vypnutí pro jedno zařízení řadiče jsou běžné pouze v případech, ve kterých hardwarová komponenta selhání zařízení vyžaduje nahrazení. V situaci, ve kterém ovlivňuje výkon nadměrného využití paměti nebo chybně fungující řadič může také vyžadovat restartování řadiče. Také můžete potřebovat toorestart řadič po úspěšné řadiče náhradní, pokud chcete tooenable a testovací kontroler hello nahradit.

Restartování zařízení není rušivé tooconnected iniciátory, za předpokladu, že je k dispozici pasivní řadič hello. Pokud není k dispozici řadič pasivní nebo vypnutý, restartujte hello active řadič může vést k přerušení hello služby a výpadky.

> [!IMPORTANT]
> * **Běžícího řadiče nikdy odeberte fyzicky jako to může vést ke ztrátě redundance a k vyššímu riziku výpadku.**
> * Hello následující postup platí jenom toohello fyzického zařízení StorSimple. Informace o tom, jak toostart, zastavení a restartování hello cloudu zařízení StorSimple, najdete v části [pracovat s hello cloudu zařízení](storsimple-8000-cloud-appliance-u2.md##work-with-the-storsimple-cloud-appliance).

Můžete restartovat nebo vypnout řadič jedno zařízení prostřednictvím hello Azure portálu service Manager zařízení StorSimple hello nebo prostředí Windows PowerShell pro StorSimple.

toomanage řadičů zařízení z hello portál Azure, proveďte následující kroky hello.

#### <a name="toorestart-or-shut-down-a-controller-in-azure-portal"></a>toorestart nebo vypnutí řadič v portálu Azure
1. Ve službě StorSimple Manager zařízení přejděte příliš**zařízení**. Vyberte zařízení ze seznamu hello zařízení. 

    ![Vyberte zařízení](./media/storsimple-8000-manage-device-controller/manage-controller1.png)

2. Přejděte příliš**Nastavení > řadiče**.
   
    ![Ověřte, zda jsou v pořádku řadiče zařízení StorSimple](./media/storsimple-8000-manage-device-controller/manage-controller2.png)
3. V hello **řadiče** okno, ověřte, zda je stav hello oba řadiče hello na vašem zařízení **stavu v pořádku**. Vyberte zařízení, klikněte pravým tlačítkem myši a pak vyberte **restartujte** nebo **vypnout**.

    ![Vyberte restartování nebo vypnutí řadiče zařízení StorSimple](./media/storsimple-8000-manage-device-controller/manage-controller3.png)

4. Úloha se vytvoří toorestart nebo vypněte hello řadič a se zobrazí upozornění použít, pokud existuje. toomonitor hello restartování nebo vypnutí, přejděte příliš**služby > protokoly aktivity** a vyfiltrujte službou konkrétní tooyour parametry. Pokud řadič byl vypnut, bude nutné toopush hello power tlačítko tooturn na hello řadiče tooturn ho na.

#### <a name="toorestart-or-shut-down-a-controller-in-windows-powershell-for-storsimple"></a>toorestart nebo vypnutí řadič ve Windows Powershellu pro StorSimple
Proveďte následující kroky tooshut dolů hello nebo restartujte jeden řadič v zařízení StorSimple z hello prostředí Windows PowerShell pro StorSimple.

1. Přístup hello zařízení prostřednictvím konzoly sériového portu hello nebo relace Telnetu ze vzdáleného počítače. tooconnect tooController 0 nebo 1 řadiče, postupujte podle kroků hello v [konzoly sériového portu toohello zařízení tooconnect pro použití klienta PuTTY](storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).
2. V nabídce konzoly sériového portu hello, zvolte možnost 1, **přihlásit úplný přístup**.
3. Hello zpráva hlavičky, poznamenejte si hello řadiče jsou příliš připojené (řadič 0 nebo 1 řadič) a jestli je hello aktivní nebo pasivní (pohotovostní) řadič hello.
   
   * tooshut dolů jediného kontroleru, hello řádku, zadejte:
     
       `Stop-HcsController`
     
       Vypne hello řadiče, které jsou připojené do. Pokud zastavíte hello aktivního řadiče, pak zařízení hello převezme toohello pasivní řadiče.

   * toorestart řadič hello řádku, zadejte:
     
       `Restart-HcsController`
     
       Tento řadič hello restartování, které jsou připojené do. Pokud restartujete hello aktivního řadiče, dojde k chybě přes řadič pasivní toohello před hello restartovat.

## <a name="shut-down-a-storsimple-device"></a>Vypněte zařízení StorSimple

Tato část vysvětluje, jak tooshut dolů spuštěný nebo selhání zařízení StorSimple ze vzdáleného počítače. Zařízení je vypnutý, poté, co se oba řadiče zařízení hello vypnut. Vypnutí zařízení se provádí při hello je fyzicky přesunout, nebo zařízení je vyřazeno z provozu.

> [!IMPORTANT]
> Před vypnutím hello zařízení, zkontrolujte stav hello součástí hello zařízení. Přejděte tooyour zařízení a pak klikněte na tlačítko **Nastavení > stavu hardwaru**. V hello **stav a hardwaru stavu** okno, ověřte, zda je stav hello DIODU všech součástí hello zelená. Pouze v pořádku zařízení má zelený stav. Pokud vaše zařízení je vrácení vypnout tooreplace komponentu nepracuje správně, uvidíte se nezdařila (červený) nebo hello degradovaném stavu (žlutý) pro příslušné součásti.


#### <a name="tooshut-down-a-storsimple-device"></a>tooshut dolů zařízení StorSimple

1. Použití hello [restartování nebo vypnutí řadič](#restart-or-shut-down-a-single-controller) tooidentify postupu a vypnutí hello pasivní řadiče na vašem zařízení. Tuto operaci lze provést v hello portál Azure nebo v prostředí Windows PowerShell pro StorSimple.
2. Opakujte hello výše tooshut krok dolů řadič active hello.
3. Se musí nyní podíváte na hello zpět roviny tohoto zařízení hello. Po hello dva řadiče jsou zcela vypnout, hello stavu LED na oba řadiče hello by měl být blikat red. Pokud potřebujete v tuto chvíli tooturn vypnout hello zařízení zcela, flip hello power přepne na napájení a chlazení moduly (PCMs) toohello OFF pozici. To má vypnout hello zařízení.

## <a name="reset-hello-device-toofactory-default-settings"></a>Resetování hello zařízení toofactory výchozí nastavení

> [!IMPORTANT]
> Pokud potřebujete tooreset toofactory výchozí nastavení zařízení, obraťte se na Microsoft Support. níže popsaný postup Hello měli použít pouze ve spojení s Microsoft Support.

Tento postup popisuje, jak tooreset vaše Microsoft Azure StorSimple toofactory výchozí nastavení zařízení pomocí Windows Powershellu pro StorSimple.
Když zařízení resetujete, odeberete všechna data a nastavení z hello celý cluster ve výchozím nastavení.

Proveďte následující kroky tooreset hello Microsoft Azure StorSimple zařízení toofactory výchozí nastavení:

### <a name="tooreset-hello-device-toodefault-settings-in-windows-powershell-for-storsimple"></a>tooreset hello toodefault nastavení zařízení v prostředí Windows PowerShell pro StorSimple
1. Přístup k zařízení hello pomocí jeho konzoly sériového portu. Zkontrolujte hello banner zpráva tooensure se připojené toohello **Active** řadiče.
2. V nabídce konzoly sériového portu hello, zvolte možnost 1, **přihlásit úplný přístup**.
3. Hello řádku zadejte následující příkaz tooreset hello celý cluster, odebrání všech dat, metadat a řadič nastavení hello:
   
    `Reset-HcsFactoryDefault`
   
    tooinstead resetovat jediného kontroleru, použijte hello [resetování HcsFactoryDefault](http://technet.microsoft.com/library/dn688132.aspx) rutiny s hello `-scope` parametru.)
   
    Hello systém se restartuje vícekrát. Při resetování hello byla úspěšně dokončena, budete upozorněni. V závislosti na modelu hello systému může trvat 45 – 60 minut pro zařízení s 8100 a 60 až 90 minut pro 8600 toofinish tohoto procesu.
   
## <a name="questions-and-answers-about-managing-device-controllers"></a>Otázky a odpovědi týkající se správy řadiče zařízení
V této části jsme vytvořili souhrn některé hello nejčastější dotazy ohledně správy řadiče zařízení StorSimple.

**Otázka:** Co se stane, když oba hello řadiče na zařízení jsou v pořádku a zapnuté a I restartování nebo vypnutí řadič active hello?

**Odpověď:** Pokud jsou obě hello řadiče v zařízení v pořádku a zapnuté, zobrazí se výzva k potvrzení. Můžete se rozhodnout pro:

* **Restartujte řadič active hello** – upozornění se zobrazí restartování řadič active způsobila hello zařízení toofail toohello pasivní řadiče. Hello řadiče se restartuje.
* **Vypnout aktivní řadiče** – oznamuje, že vypíná řadič active výsledkem výpadku. Musíte taky tlačítka napájení hello toopush na tooturn zařízení hello na řadiči hello.

**Otázka:** Co se stane, pokud hello pasivní řadiče v zařízení není k dispozici nebo zapnuté vypnout a I restartování nebo vypnutí řadič active hello?

**Odpověď:** Pokud je pasivní řadič hello na vašem zařízení není k dispozici nebo zapnuté vypnout a vy zvolíte:

* **Restartujte řadič active hello** – oznamuje, že pokračování operace hello způsobí dočasné přerušení služby hello a zobrazí se výzva k potvrzení.
* **Vypnout aktivní řadiče** – oznamuje, že pokračující operace hello výsledkem výpadku. Musíte taky tlačítka napájení hello toopush na jeden nebo oba řadiče tooturn na hello zařízení. Zobrazí se výzva k potvrzení.

**Otázka:** Pokud nemá hello řadiče restartování nebo vypnutí nezdaří tooprogress?

**Odpověď:** Restartování nebo vypnutí řadič může selhat, pokud:

* Probíhá aktualizace zařízení.
* Již probíhá restartování řadiče.
* Vypnutí řadiče již probíhá.

**Otázka:** Jak můžete zjistit, pokud byl restartován nebo vypnut řadič?

**Odpověď:** Můžete zkontrolovat stav řadiče hello v okně řadiče. Stav řadiče Hello se označuje, zda řadič v hello proces restartování nebo vypnutí. Kromě toho hello **výstrahy** okno obsahovat informační výstrahu, pokud řadiče hello je restartován nebo vypnut. Hello operacích restart a vypnutí řadiče se také zaznamenávají v protokolech jsou hello. Další informace o protokoly jsou přejděte příliš[zobrazit protokoly aktivity hello](storsimple-8000-service-dashboard.md#view-the-activity-logs).

**Otázka:** Je k dispozici žádné toohello dopad vstupně-výstupních operací v důsledku řadiče převzetí služeb při selhání?

**Odpověď:** připojení TCP Hello mezi iniciátorů a aktivní řadiče se resetuje v důsledku řadiče převzetí služeb při selhání, ale se znovu vytvoří, když pasivní řadiče hello předpokládá operaci. Během hello během této operace může být pozastavení dočasných (méně než 30 sekund) v vstupně-výstupní aktivitu mezi iniciátorů a hello zařízení.

**Otázka:** Jak se poté, co byla vypnout a odebrat vrátit Moje řadiče tooservice?

**Odpověď:** tooreturn tooservice řadiče, je nutné vložit ji do hello skříň jak je popsáno v [nahradit modul řadiče zařízení StorSimple](storsimple-8000-controller-replacement.md).

## <a name="next-steps"></a>Další kroky
* Pokud narazíte na potíže s řadičů zařízení StorSimple, které nelze vyřešit pomocí hello postupy uvedené v tomto kurzu [kontaktovat Microsoft Support](storsimple-8000-contact-microsoft-support.md).
* toolearn Další informace o používání služby StorSimple Manager zařízení hello, přejděte příliš[použití hello tooadminister service Manager zařízení StorSimple zařízení StorSimple](storsimple-8000-manager-service-administration.md).

