---
title: "řadiče zařízení StorSimple aaaManage | Microsoft Docs"
description: "Zjistěte, jak toostop, restartovat, vypnout nebo obnovit vaše řadiče zařízení StorSimple."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 4ee989d0-956f-4c14-951e-fd4e490ea09d
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/11/2016
ms.author: alkohli
ms.openlocfilehash: 9a86aa0f4a8fd96c36df206774972602c47a49a6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-your-storsimple-device-controllers"></a>Spravovat vaše řadiče zařízení StorSimple
## <a name="overview"></a>Přehled
Tento kurz popisuje hello různé operace, které lze provést na řadiče zařízení StorSimple. Hello řadiče v zařízení StorSimple jsou řadiče redundantní (sdílené) v konfiguraci aktivní pasivní. V daném okamžiku jenom jeden řadič je aktivní a zpracovává všechny operace diskových a síťových hello. Hello jiný řadič je v pasivním režimu. Pokud se řadič active hello nezdaří, bude hello pasivní řadiče aktivní automaticky.

Tento kurz obsahuje podrobné pokyny toomanage hello zařízení řadiče pomocí:

* **Řadiče** části hello **údržby** stránku hello služby StorSimple Manager
* Prostředí Windows PowerShell pro StorSimple.

Doporučujeme vám, že spravujete hello řadiče zařízení prostřednictvím služby StorSimple Manager hello. Pokud se akce lze provést pouze pomocí prostředí Windows PowerShell pro StorSimple, hello kurzu provede poznamenejte si ho.

Po přečtení tohoto kurzu, budete moci:

* Restartování nebo vypnutí řadič zařízení StorSimple
* Vypněte zařízení StorSimple
* Obnovit výchozí toofactory zařízení StorSimple

## <a name="restart-or-shut-down-a-single-controller"></a>Restartování nebo vypnutí jediného kontroleru
Řadič restartování nebo vypnutí není vyžadován jako součást operace normální systému. Operace vypnutí pro jedno zařízení řadiče jsou běžné pouze v případech, ve kterých hardwarová komponenta selhání zařízení vyžaduje nahrazení. V situaci, ve kterém ovlivňuje výkon nadměrného využití paměti nebo chybně fungující řadič může také vyžadovat restartování řadiče. Také můžete potřebovat toorestart řadič po úspěšné řadiče náhradní, pokud chcete tooenable a testovací kontroler hello nahradit.

Restartování zařízení není rušivé tooconnected iniciátory, za předpokladu, že je k dispozici pasivní řadič hello. Pokud není k dispozici řadič pasivní nebo vypnutý, restartujte hello active řadič může vést k přerušení hello služby a výpadky.

> [!IMPORTANT]
> * **Běžícího řadiče nikdy odeberte fyzicky jako to může vést ke ztrátě redundance a k vyššímu riziku výpadku.**
> * Hello následující postup platí jenom toohello fyzického zařízení StorSimple. Informace o tom, jak toostart, zastavení a restartování hello virtuální zařízení, najdete v části [pracovat s virtuálním zařízením hello](storsimple-virtual-device-u2.md#work-with-the-storsimple-virtual-device).
> 
> 

Můžete restartovat nebo vypnout řadič jedno zařízení pomocí hello portál Azure classic služby StorSimple Manager hello nebo prostředí Windows PowerShell pro StorSimple

toomanage řadičů zařízení z hello portál Azure classic, proveďte následující kroky hello.

#### <a name="toorestart-or-shut-down-a-controller-in-classic-portal"></a>toorestart nebo vypnutí řadič portálu classic
1. Přejděte příliš**zařízení > údržby**.
2. Přejděte příliš**stavu hardwaru** a ověřte, zda je stav hello oba řadiče hello na vašem zařízení **stavu v pořádku**.
   
    ![Ověřte, zda jsou v pořádku řadiče zařízení StorSimple](./media/storsimple-manage-device-controller/IC766017.png)
3. Hello dolní části hello **údržby** klikněte na tlačítko **Správa řadičů**.
   
    ![Správa řadiče zařízení StorSimple](./media/storsimple-manage-device-controller/IC766018.png)</br>
   
   > [!NOTE]
   > Pokud nevidíte **Správa řadičů**, pak je nutné tooinstall aktualizace. Další informace najdete v tématu [aktualizace zařízení StorSimple](storsimple-update-device.md).
   > 
   > 
4. V hello **změnit nastavení řadiče** dialogové okno pole, hello následující:
   
   1. Z hello **vyberte řadiče** rozevíracího seznamu, vyberte hello řadiče, které chcete toomanage. Možnosti Hello jsou řadič 0 a řadič 1. Tyto řadiče se také identifikují aktivní nebo pasivní.
      
      > [!NOTE]
      > Řadič nelze spravovat, pokud je k dispozici nebo je zapnuté vypnout, a nebude se zobrazovat v rozevíracím seznamu hello.
      > 
      > 
   2. Z hello **vybrat akci** rozevíracího seznamu vyberte **restartování řadiče** nebo **vypněte řadič**.
      
       ![Restartujte řadič pasivní zařízení StorSimple](./media/storsimple-manage-device-controller/IC766020.png)
   3. Klikněte na ikonu zaškrtnutí hello ![Ikona zaškrtnutí](./media/storsimple-manage-device-controller/IC740895.png).

To bude restartování nebo vypnutí řadiče hello. Následující tabulka Hello shrnuje hello podrobnosti co se stane, že v závislosti na vybrané možnosti hello provedené v hello **změnit nastavení řadiče** dialogové okno.  

| Výběr # | Pokud zvolíte možnost... | To se stane. |
| --- | --- | --- |
| 1. |Restartujte řadič pasivní hello. |Úloha se vytvoří toorestart hello řadiče a po úspěšném vytvoření hello úlohy se zobrazí oznámení. Tím zahájíte restartování řadiče hello. Restartování hello můžete monitorovat pomocí přístupu k **služby > řídicí panel > Zobrazit protokoly operací** a pak službou konkrétní tooyour parametry filtrování. |
| 2. |Restartujte řadič active hello. |Zobrazí se hello následující upozornění: "Pokud restartujete hello aktivního řadiče, hello zařízení bude převzetí služeb při selhání toohello pasivní řadiče. Chcete toocontinue?" </br>Pokud si zvolíte tooproceed v této operaci, hello další kroky budou používat identické toothose toorestart hello pasivní řadiče (viz výběr 1). |
| 3. |Vypněte pasivní řadič hello. |Zobrazí se následující zpráva hello: "po dokončení vypnutí budete potřebovat toopush hello power na vaše řadiče tooturn ho na tlačítko. Opravdu že chcete tooshut dolů tento řadič?" </br>Pokud si zvolíte tooproceed v této operaci, hello další kroky budou používat identické toothose toorestart hello pasivní řadiče (viz výběr 1). |
| 4. |Vypněte active řadič hello. |Zobrazí se následující zpráva hello: "po dokončení vypnutí budete potřebovat toopush hello power na vaše řadiče tooturn ho na tlačítko. Opravdu že chcete tooshut dolů tento řadič?" </br>Pokud si zvolíte tooproceed v této operaci, hello další kroky budou používat identické toothose toorestart hello pasivní řadiče (viz výběr 1). |

#### <a name="toorestart-or-shut-down-a-controller-in-windows-powershell-for-storsimple"></a>toorestart nebo vypnutí řadič ve Windows Powershellu pro StorSimple
Proveďte následující kroky tooshut dolů hello nebo restartujte jeden řadič v zařízení StorSimple z hello portál Azure classic.

1. Zařízení hello přístup pomocí konzoly sériového portu hello nebo relace Telnetu ze vzdáleného počítače. Připojit tooController 0 nebo 1 řadiče podle následující hello kroky v [konzoly sériového portu toohello zařízení tooconnect pro použití klienta PuTTY](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).
2. V nabídce konzoly sériového portu hello, zvolte možnost 1, **přihlásit úplný přístup**.
3. Hello zpráva hlavičky, poznamenejte si hello řadiče jsou příliš připojené (řadič 0 nebo 1 řadič) a jestli je hello aktivní nebo pasivní (pohotovostní) řadič hello.
   
   * tooshut dolů jediného kontroleru, hello řádku, zadejte:
     
       `Stop-HcsController`
     
       To se zastaví hello řadiče, které jsou připojené do. Pokud zastavíte hello aktivního řadiče, pak ji bude převzetí služeb při selhání toohello pasivní řadiče než vypne.
   * toorestart řadič hello řádku, zadejte:
     
       `Restart-HcsController`
     
       To se restartuje hello řadiče, které jsou připojené do. Pokud restartujete hello aktivního řadiče, se nezdaří přes toohello pasivní řadiče před hello restartovat.

## <a name="shut-down-a-storsimple-device"></a>Vypněte zařízení StorSimple
Tato část vysvětluje, jak tooshut dolů spuštěný nebo selhání zařízení StorSimple ze vzdáleného počítače. Zařízení je vypnutý po vypnutí oba řadiče zařízení hello. Vypnutí zařízení se provádí při hello fyzicky přesunout, nebo zařízení je vyřazeno z provozu.

> [!IMPORTANT]
> Před vypnutím hello zařízení, zkontrolujte stav hello součástí hello zařízení. Přejděte příliš**zařízení > údržby > stavu hardwaru** a ověřte, zda je stav hello DIODU všech součástí hello zelená. Zelený stav bude mít jenom zařízení v pořádku. Pokud vaše zařízení je vrácení vypnout tooreplace komponentu nepracuje správně, uvidíte se nezdařila (červený) nebo hello degradovaném stavu (žlutý) pro příslušné součásti.
> 
> 

#### <a name="tooshut-down-a-storsimple-device"></a>tooshut dolů zařízení StorSimple
1. Použití hello [restartování nebo vypnutí řadič](#restart-or-shut-down-a-single-controller) tooidentify postupu a vypnutí hello pasivní řadiče na vašem zařízení. Tuto operaci lze provést v hello portál Azure classic nebo v prostředí Windows PowerShell pro StorSimple.
2. Opakujte hello výše tooshut krok dolů řadič active hello.
3. Teď musíte toolook v hello zpět ploché hello zařízení. Po hello dva řadiče jsou zcela vypnout, hello stavu LED na oba řadiče hello by měl být blikat red. Pokud potřebujete v tuto chvíli tooturn vypnout hello zařízení zcela, flip hello power přepne na napájení a chlazení moduly (PCMs) toohello OFF pozici. To má vypnout hello zařízení.

<!--#### tooshut down a StorSimple device in Windows PowerShell for StorSimple

1. Connect toohello serial console of hello StorSimple device by following hello steps in [Use PuTTY tooconnect toohello device serial console](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-serial-console).

1. In hello serial console menu, verify from hello banner message that hello controller you are connected toois hello passive controller. If you are connected toohello active controller, disconnect from this controller and connect toohello other controller.

1. In hello serial console menu, choose option 1, **log in with full access**.

1. At hello prompt, type:

    `Stop-HCSController`

    This should shut down hello current controller. tooverify whether hello shutdown has finished, check hello back of hello device. hello controller status LED should be solid red.

1. Repeat steps 1 through 4 tooconnect toohello active controller and then shut it down.

1. After both hello controllers are completely shut down, hello status LEDs on both should be blinking red. If you need tooturn off hello device completely at this time, flip hello power switches on both Power and Cooling Modules (PCMs) toohello OFF position.-->

## <a name="reset-hello-device-toofactory-default-settings"></a>Resetování hello zařízení toofactory výchozí nastavení
> [!IMPORTANT]
> Pokud potřebujete tooreset toofactory výchozí nastavení zařízení, obraťte se na Microsoft Support. níže popsaný postup Hello měli použít pouze ve spojení s Microsoft Support.
> 
> 

Tento postup popisuje, jak tooreset vaše Microsoft Azure StorSimple toofactory výchozí nastavení zařízení pomocí Windows Powershellu pro StorSimple.
Když zařízení resetujete, odeberete všechna data a nastavení z hello celý cluster ve výchozím nastavení.

Proveďte následující kroky tooreset hello Microsoft Azure StorSimple zařízení toofactory výchozí nastavení:

### <a name="tooreset-hello-device-toodefault-settings-in-windows-powershell-for-storsimple"></a>tooreset hello toodefault nastavení zařízení v prostředí Windows PowerShell pro StorSimple
1. Přístup k zařízení hello pomocí jeho konzoly sériového portu. Zkontrolujte hello banner zpráva tooensure se připojené toohello aktivního řadiče.
2. V nabídce konzoly sériového portu hello, zvolte možnost 1, **přihlásit úplný přístup**.
3. Hello řádku zadejte následující příkaz tooreset hello celý cluster, odebrání všech dat, metadat a řadič nastavení hello:
   
    `Reset-HcsFactoryDefault`
   
    tooinstead resetovat jediného kontroleru, použijte hello [resetování HcsFactoryDefault](http://technet.microsoft.com/library/dn688132.aspx) rutiny s hello `-scope` parametru.)
   
    Hello systém se restartuje vícekrát. Při resetování hello byla úspěšně dokončena, budete upozorněni. V závislosti na modelu hello systému může trvat 45 – 60 minut pro zařízení s 8100 a 60 až 90 minut pro 8600 toofinish tohoto procesu.
   
   > [!TIP]
   > * Pokud používáte aktualizace 1.2 nebo dříve použijte hello `–SkipFirmwareVersionCheck` Kontrola verze firmwaru hello tooskip parametr (v opačném případě se zobrazí chybová zpráva o neshodě firmware: Obnovení továrního nastavení nemůže pokračovat z důvodu neshody tooa v verzí firmwaru hello. ).
   > * Postup resetování factory Hello mohou být neúspěšné pro zařízení StorSimple, které jsou spuštěné Update 1 nebo 1.1 hello Government portálu a provedli úspěšné jednoduchá nebo duální řadiče nahrazení (s nahrazení řadiče, které byly dodány s před aktualizací 1 software). To se stane, když obnovit hello tovární nastavení, že obrázek se ověří přítomnost hello soubor SHA1 hello řadiče, která neexistuje před aktualizací softwaru 1. Pokud se tento objekt pro vytváření resetovat chyba, obraťte se na Microsoft Support tooassist vám hello další kroky. Tento problém není seznámili s nahrazení řadiče, které byly odeslány z objektu pro vytváření hello s Update 1 nebo novější softwarem.
   > 
   > 

## <a name="questions-and-answers-about-managing-device-controllers"></a>Otázky a odpovědi týkající se správy řadiče zařízení
V této části jsme vytvořili souhrn některé hello nejčastější dotazy ohledně správy řadiče zařízení StorSimple.

**Otázka:** Co se stane, když oba hello řadiče na zařízení jsou v pořádku a zapnuté a I restartování nebo vypnutí řadič active hello?

**Odpověď:** Pokud jsou obě hello řadiče v zařízení v pořádku a zapnuté, zobrazí se výzva k potvrzení. Můžete se rozhodnout pro:

* **Restartujte řadič active hello** – budete informováni, že restartování řadič služby active způsobí hello zařízení toofail přes toohello pasivní řadiče. Hello řadiče se restartuje.
* **Vypnout aktivní řadiče** – budete informováni, že vypíná řadič služby active dojde k výpadkům. Budete také potřebovat tlačítka napájení hello toopush na tooturn zařízení hello na řadiči hello.

**Otázka:** Co se stane, pokud hello pasivní řadiče v zařízení není k dispozici nebo zapnuté vypnout a I restartování nebo vypnutí řadič active hello?

**Odpověď:** Pokud je pasivní řadič hello na vašem zařízení není k dispozici nebo zapnuté vypnout a vy zvolíte:

* **Restartujte řadič active hello** – budete informováni, že pokračování operace hello bude mít za následek dočasné přerušení služby hello a zobrazí se výzva k potvrzení.
* **Vypnout aktivní řadiče** – budete informováni, že pokračování operace hello dojde k výpadkům a že budete potřebovat tlačítka napájení hello toopush na jeden nebo oba řadiče tooturn na hello zařízení. Zobrazí se výzva k potvrzení.

**Otázka:** Pokud nemá hello řadiče restartování nebo vypnutí nezdaří tooprogress?

**Odpověď:** Restartování nebo vypnutí řadič může selhat, pokud:

* Probíhá aktualizace zařízení.
* Již probíhá restartování řadiče.
* Vypnutí řadiče již probíhá.

**Otázka:** Jak můžete zjistit, pokud byl restartován nebo vypnut řadič?

**Odpověď:** Můžete zkontrolovat stav řadiče hello na stránce údržby hello. Stav řadiče Hello označí, zda řadič byl restartován nebo vypnut. Kromě toho stránku hello výstrahy obsahují informační výstrahu v případě řadiče hello byl restartován nebo vypnut. operace restartování a vypnutí řadiče Hello se zaznamenávají také protokoly operací hello. Další informace o protokoly operací přejděte příliš[zobrazit protokoly operací hello](storsimple-service-dashboard.md#view-the-operations-logs).

**Otázka:** Je k dispozici žádné toohello dopad vstupně-výstupních operací v důsledku řadiče převzetí služeb při selhání?

**Odpověď:** připojení TCP Hello mezi iniciátorů a aktivní řadiče se resetuje v důsledku řadiče převzetí služeb při selhání, ale se znovu vytvoří, když pasivní řadiče hello předpokládá operaci. Během hello během této operace může být pozastavení dočasných (méně než 30 sekund) v vstupně-výstupní aktivitu mezi iniciátorů a hello zařízení.

**Otázka:** Jak se poté, co byla vypnout a odebrat vrátit Moje řadiče tooservice?

**Odpověď:** tooreturn tooservice řadiče, je nutné vložit ji do hello skříň jak je popsáno v [nahradit modul řadiče zařízení StorSimple](storsimple-controller-replacement.md).

## <a name="next-steps"></a>Další kroky
* Pokud narazíte na potíže s řadičů zařízení StorSimple, které nelze vyřešit pomocí hello postupy uvedené v tomto kurzu [kontaktovat Microsoft Support](storsimple-contact-microsoft-support.md).
* toolearn Další informace o používání služby StorSimple Manager hello, přejděte příliš[použití hello tooadminister služby StorSimple Manager zařízení StorSimple](storsimple-manager-service-administration.md).

