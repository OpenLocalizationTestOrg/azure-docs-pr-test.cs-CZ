---
title: "aaaStorSimple převzetí služeb při selhání a zotavení po havárii | Microsoft Docs"
description: "Zjistěte, jak toofail vaše tooitself zařízení StorSimple, jiného fyzického zařízení nebo pro virtuální zařízení."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 5751598e-49c8-42b3-8121-fea5857a7d83
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/16/2016
ms.author: alkohli
ms.openlocfilehash: 00ce365f8a9095d1f0292e665d7f9eaa844b44ae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="failover-and-disaster-recovery-for-your-storsimple-device"></a>Převzetí služeb při selhání a zotavení po havárii pro zařízení StorSimple
## <a name="overview"></a>Přehled
Tento kurz popisuje hello kroky požadované toofail přes zařízení StorSimple v hello události havárie. Převzetí služeb při selhání vám umožní toomigrate data ze zdrojového zařízení v hello datacenter tooanother fyzické nebo virtuální zařízení nachází v hello stejné nebo jiné zeměpisné umístění. 

Zotavení po havárii (DR) je pomocí funkce převzetí služeb při selhání zařízení hello orchestrovat a je zahájeno z hello **zařízení** stránky. Tato stránka podporován všechny hello StorSimple zařízení připojených tooyour StorSimple Manager service. Pro každé zařízení, hello popisný název, stav, zřízený a maximální kapacitu typ a model se zobrazí.

![Stránka zařízení](./media/storsimple-device-failover-disaster-recovery/IC740972.png)

Hello pokyny v tomto kurzu platí tooStorSimple fyzické a virtuální zařízení mezi všemi verzemi softwaru.

## <a name="disaster-recovery-dr-and-device-failover"></a>Zotavení po havárii (DR) a převzetí služeb při selhání zařízení
Ve scénáři zotavení po havárii hello primární zařízení přestane fungovat. V takovém případě můžete přesunout data v cloudu hello přidružené hello zařízení se nezdařilo tooanother zařízení pomocí hello primární zařízení jako hello *zdroj* a se zadáním jiné zařízení jako hello *cíl*. Můžete vybrat jeden nebo více svazku kontejnery toomigrate toohello cílové zařízení. Tento proces je hello odkazované tooas *převzetí služeb při selhání*. 

Během převzetí služeb při selhání hello hello kontejnery svazků ze zdrojového zařízení hello změnit vlastnictví a jsou přenášená toohello cílové zařízení. Po kontejnery svazků hello změnit vlastnictví, tyto odstraněny ze zdrojového zařízení hello. Po dokončení odstranění hello hello cílové zařízení můžete pak se při selhání zpátky.

Obvykle následující zotavení po Havárii, hello nejnovější zálohy je použité toorestore hello data toohello cílové zařízení. Ale pokud jsou více zásady zálohování pro hello stejný svazek, pak získá zachyceny zásady zálohování hello s hello největší počet svazků a hello nejnovější zálohy z této zásady je použité toorestore hello data na hello cílové zařízení.

Jako příklad, pokud existují dvě zásady zálohování (jedno a jeden vlastní) *defaultPol*, *customPol* s hello následující podrobnosti:

* *defaultPol* : jeden svazek, *vol1*, spustí denní od 22:30:00.
* *customPol* : čtyři svazky, *vol1*, *vol2*, *vol3*, *vol4*, spustí denní od 22:00:00.

V takovém případě *customPol* se použije jako má více svazků a jsme určit jejich prioritu pro konzistenci s havárií. Hello nejnovější zálohy z této zásady je použité toorestore data.

## <a name="considerations-for-device-failover"></a>Důležité informace týkající se zařízení převzetí služeb při selhání
V případě havárie hello můžete se rozhodnout toofail přes zařízení StorSimple:

* tooa fyzického zařízení 
* tooitself
* tooa virtuálního zařízení

Žádné zařízení převzetí služeb při selhání mějte na paměti hello následující:

* Hello požadavky pro zotavení po Havárii je, že jsou všechny svazky hello v rámci kontejnery svazků hello offline a kontejnery svazků hello mají přiřazený cloudový snímek. 
* Hello dostupných cílových zařízení pro zotavení po Havárii jsou zařízení, která mají dostatek místa tooaccommodate hello kontejnery vybraný svazek. 
* Hello zařízení, které jsou připojené tooyour služby, ale nesplňuje kritéria hello dostatek místa, nebudete mít k dispozici jako cílová zařízení.
* Následující zotavení po Havárii, po omezenou dobu výrazně může mít vliv výkon datové hello, jako hello zařízení bude třeba tooaccess hello data z cloudu hello a uložte ho místně.

#### <a name="device-failover-across-software-versions"></a>Zařízení převzetí služeb při selhání mezi verzemi softwaru
Služba StorSimple Manager v nasazení může mít více fyzických i virtuálních zařízení, všechny verze spuštění různých softwaru. V závislosti na verzi softwaru hello se typy hello svazku na hello zařízení může také lišit. Například zařízení spuštěný Update 2 nebo vyšší místně připnutý a vrstvené svazky (s archivace probíhá podmnožinu víceúrovňová). Před aktualizací 2 zařízení na hello druhé straně může mít vrstvené a archivaci svazky. 

Pomocí následující tabulky toodetermine, pokud budete selhání tooanother zařízení se systémem chování jiný software verze a hello typů svazku během zotavení po Havárii hello.

| Převzetí služeb při selhání z | Povolené pro fyzické zařízení | Povolené pro virtuální zařízení |
| --- | --- | --- |
| Aktualizace 2 toopre – aktualizace 1 (verze 0.1, 0.2 a 0.3) |Ne |Ne |
| Aktualizace 2 tooUpdate 1 (1, 1.1, 1.2) |Ano <br></br>Pokud pomocí místně připnuté nebo zřízeny vrstvené svazky nebo kombinaci dvou hello svazky jsou vždy převzetí služeb při selhání jako vrstvené. |Ano<br></br>Pokud pomocí místně připnutý svazky, tyto došlo k selhání přes vrstvené. |
| Aktualizace 2 tooUpdate 2 (novější verze) |Ano<br></br>Pokud používáte místně vázaný nebo vrstvené svazky nebo kombinaci dva, hello svazky jsou vždy převzetí služeb při selhání jako hello od typ svazku; víceúrovňová jako vrstvené a místně vázaný jako místně vázaný. |Ano<br></br>Pokud pomocí místně připnutý svazky, tyto došlo k selhání přes vrstvené. |

#### <a name="partial-failover-across-software-versions"></a>Částečné převzetí služeb při selhání mezi verzemi softwaru
Tyto pokyny použijte, pokud máte v úmyslu tooperform částečné převzetí služeb při selhání pomocí zdrojového zařízení StorSimple se systémem před aktualizací 1 tooa cílové verzi Update 1 nebo novější. 

| Částečné převzetí služeb při selhání | Povolené pro fyzické zařízení | Povolené pro virtuální zařízení |
| --- | --- | --- |
| Před aktualizací 1 (verze 0.1, 0.2 a 0.3) tooUpdate 1 nebo novější |Ano, najdete níže pro nejlepší postup tip hello. |Ano, najdete níže pro nejlepší postup tip hello. |

> [!TIP]
> Došlo ke cloudu metadata a data formátu změně v Update 1 a novějších verzích. Proto nedoporučujeme částečné převzetí služeb při selhání z 1 tooUpdate před aktualizací 1 nebo novější verze. Pokud potřebujete tooperform částečné převzetí služeb při selhání, doporučujeme nejprve nainstalovat Update 1 nebo pozdější hello zařízení (zdrojové a cílové) a poté pokračovat hello převzetí služeb při selhání. 
> 
> 

## <a name="fail-over-tooanother-physical-device"></a>Selhání tooanother fyzického zařízení
Proveďte následující kroky toorestore hello fyzického zařízení tooa cílové zařízení.

1. Ověřte, že tento kontejner svazků hello, které chcete toofail přes přidruženy cloudových snímků.
2. Na hello **zařízení** klikněte na tlačítko hello **kontejnery svazků** kartě.
3. Vyberte kontejner svazků, které chcete toofail přes tooanother zařízení. Klikněte na tlačítko hello svazku kontejneru toodisplay hello seznam svazků v tomto kontejneru. Vyberte svazek a klikněte na tlačítko **přepnout do režimu Offline** tootake hello svazek do režimu offline. Tento postup opakujte pro všechny svazky hello v kontejneru svazků hello.
4. Předchozí krok zopakujte hello pro všechny kontejnery svazků hello chcete toofail přes tooanother zařízení.
5. Na hello **zařízení** klikněte na tlačítko **převzetí služeb při selhání**.
6. V Průvodci hello, které se otevře, v části **zvolte toofail kontejneru svazku přes**:
   
   1. V seznamu hello kontejnery svazků vyberte hello kontejnery svazků, které si přejete toofail přes.
      **Pouze hello kontejnery svazků s přidružený cloud snímky a offline svazky jsou zobrazeny.**
   2. V části **vyberte cílové zařízení** pro hello svazky v kontejnerech hello vybrána, vyberte z rozevíracího seznamu hello k dispozici zařízení cílové zařízení. Pouze hello zařízení, která mají hello dostupné kapacity jsou zobrazeny v rozevíracím seznamu hello.
   3. Nakonec zkontrolujte všechna nastavení hello převzetí služeb při selhání v **potvrzení převzetí služeb při selhání**. Klikněte na ikonu zaškrtnutí hello ![ikona zaškrtnutí](./media/storsimple-device-failover-disaster-recovery/IC740895.png).
7. Je vytvořen projektu převzetí služeb při selhání, který je možné monitorovat prostřednictvím hello **úlohy** stránky. Pokud hello kontejneru svazků, které při selhání má místní svazky, uvidíte jednotlivých úloh obnovení pro každý místní svazek (ne pro vrstvené svazky) v kontejneru hello. Tyto úlohy obnovení může trvat poměrně některé toocomplete čas. Je pravděpodobné, že může dříve dokončení této úlohy hello převzetí služeb při selhání. Všimněte si, že tyto svazky budou mít místní záruky, až po dokončení se hello úloh obnovení. Po dokončení převzetí služeb při selhání hello přejděte toohello **zařízení** stránky.                                            
   
   1. Vyberte hello zařízení, která byla použita jako hello cílové zařízení pro proces převzetí služeb při selhání hello.
   2. Přejděte toohello **kontejnery svazků** stránky. Všechny kontejnery svazků hello, společně s hello svazky z původního zařízení hello by měl být uvedený.

## <a name="failover-using-a-single-device"></a>Převzetí služeb při selhání pomocí jednoho zařízení
Proveďte následující kroky, pokud máte pouze jednu tooperform zařízení je nutné, převzetí služeb při selhání hello.

1. Cloudové snímky všech svazků hello proveďte v zařízení.
2. Obnovte výchozí hodnoty pro toofactory vaše zařízení. Postupujte podle hello podrobné pokyny v [jak tooreset toofactory zařízení StorSimple výchozí nastavení](storsimple-manage-device-controller.md#reset-the-device-to-factory-default-settings).
3. Konfigurace zařízení a zaregistrujte ho znovu pomocí služby StorSimple Manager.
4. Na hello **zařízení** stránky, jako by měl zobrazit hello staré zařízení **Offline**. Hello nově registrovaná zařízení by měl zobrazit jako **Online**.
5. Pro nové zařízení hello proveďte nejdřív hello minimální konfigurace zařízení hello. 
   
   > [!IMPORTANT]
   > **Minimální konfigurace hello neproběhne nejdřív, váš zotavení po Havárii se nezdaří v důsledku chyb v aktuální implementace hello. Toto chování bude opraven v novější verzi.**
   > 
   > 
6. Vyberte zařízení staré hello (offline stavu) a klikněte na tlačítko **převzetí služeb při selhání**. V Průvodci hello, který se zobrazí zadejte hello cílové zařízení jako hello nově registrovaná zařízení a selhání tohoto zařízení. Podrobné pokyny najdete v části příliš[selhání fyzického zařízení tooanother](#fail-over-to-another-physical-device).
7. Úlohu obnovení zařízení se vytvoří, můžete monitorovat z hello **úlohy** stránky.
8. Po úspěšném dokončení úlohy hello hello nové zařízení pro přístup k a přejděte toohello **kontejnery svazků** stránky. Všechny kontejnery svazků hello z původního zařízení hello by teď měly být migrované toohello nové zařízení.

## <a name="fail-over-tooa-storsimple-virtual-device"></a>Selhání tooa virtuální zařízení StorSimple
Musíte mít StorSimple virtuální zařízení vytvořený a nakonfigurovaný předchozí toorunning tento postup. Pokud se softwarem Update 2, zvažte použití virtuálního zařízení 8020 pro hello DR, který má 64 TB a používá úložiště Premium. 

Proveďte následující kroky toorestore hello zařízení tooa cílový virtuální zařízení StorSimple hello.

1. Ověřte, že tento kontejner svazků hello, které chcete toofail přes přidruženy cloudových snímků.
2. Na hello **zařízení** klikněte na tlačítko hello **kontejnery svazků** kartě.
3. Vyberte kontejner svazků, které chcete toofail přes tooanother zařízení. Klikněte na tlačítko hello svazku kontejneru toodisplay hello seznam svazků v tomto kontejneru. Vyberte svazek a klikněte na tlačítko **přepnout do režimu Offline** tootake hello svazek do režimu offline. Tento postup opakujte pro všechny svazky hello v kontejneru svazků hello.
4. Předchozí krok zopakujte hello pro všechny kontejnery svazků hello chcete toofail přes tooanother zařízení.
5. Na hello **zařízení** klikněte na tlačítko **převzetí služeb při selhání**.
6. V Průvodci hello, které se otevře, v části **zvolte toofailover kontejneru svazku**, dokončete následující hello:
   
    a. V seznamu hello kontejnery svazků vyberte hello kontejnery svazků, které si přejete toofail přes.
   
    **Pouze hello kontejnery svazků s přidružený cloud snímky a offline svazky jsou zobrazeny.**
   
    b. V části **vyberte cílové zařízení pro svazky hello v kontejnerech hello vybrané**, vyberte hello virtuálního zařízení StorSimple z rozevíracího seznamu hello k dispozici zařízení. **V rozevíracím seznamu hello se zobrazí pouze hello zařízení, které mají dostatečnou kapacitu.**  
7. Nakonec zkontrolujte všechna nastavení hello převzetí služeb při selhání v **potvrzení převzetí služeb při selhání**. Klikněte na ikonu zaškrtnutí hello ![ikona zaškrtnutí](./media/storsimple-device-failover-disaster-recovery/IC740895.png).
8. Po dokončení převzetí služeb při selhání hello přejděte toohello **zařízení** stránky.
   
    a. Vyberte hello virtuálního zařízení StorSimple používaný jako hello cílové zařízení pro proces převzetí služeb při selhání hello.
   
    b. Přejděte toohello **kontejnery svazků** stránky. Všechny kontejnery svazků hello, společně s hello svazky z původního zařízení hello by měl být nyní uvedený.

![Dostupné video](./media/storsimple-device-failover-disaster-recovery/Video_icon.png)**Dostupné video**

Klikněte na tlačítko toowatch video, které ukazuje, jak můžete obnovit nezdařené přes fyzického zařízení tooa virtuální zařízení v cloudu hello [zde](https://azure.microsoft.com/documentation/videos/storsimple-and-disaster-recovery/).

## <a name="failback"></a>Navrácení služeb po obnovení
Pro Update 3 a novějších verzích podporuje StorSimple také navrácení služeb po obnovení. Po dokončení převzetí služeb při selhání hello hello provedou tyto akce:

* ze zdrojového zařízení hello vyčištěním Hello kontejnery svazků, které jsou převzetí služeb při selhání.
* Úloha na pozadí na kontejneru svazků (převzetí služeb při selhání) se zahájí na hello zdrojového zařízení. Když zkusíte toofailback, když probíhá hello úlohy, obdržíte efekt toothat oznámení. Než se úloha hello dokončení toostart hello navrácení služeb po obnovení budete potřebovat toowait. 
  
    Hello čas toocomplete hello odstranění kontejnery svazků, je závislá na různých faktorech, jako je množství dat, stáří dat hello, počet záloh a hello šířku pásma sítě dostupnou pro operaci hello. Pokud plánujete testovací převzetí služeb při selhání: navrácení: služeb nebo po: obnovení, doporučujeme, abyste otestovali kontejnery svazků s méně dat (GB). Ve většině případů můžete začít 24 hodin po dokončení převzetí služeb při selhání hello hello navrácení služeb po obnovení. 

## <a name="frequently-asked-questions"></a>Nejčastější dotazy
OTÁZKY. **Co se stane, když hello zotavení po Havárii se nezdaří, nebo má částečný úspěch?**

A. Pokud selže hello zotavení po Havárii, doporučujeme vám, opakujte akci. Hello podruhé kolem, zotavení po Havárii ví, co bylo provedeno všechny a při procesu hello zastaveno hello poprvé. Spustí se z tohoto bodu dále Hello procesu zotavení po Havárii. 

OTÁZKY. **Můžete odstranit zařízení, když probíhá hello zařízení převzetí služeb při selhání?**

A. Zařízení nelze odstranit, když probíhá zotavení po Havárii. Zařízení může odstranit pouze po dokončení hello zotavení po Havárii.

OTÁZKY.    **Pokud hello uvolňování paměti spusťte na hello zdrojového zařízení, tak, aby hello místní data na zdrojového zařízení se odstraní?**

A. Uvolňování paměti povolí na hello zdrojového zařízení až po hello zařízení je zcela vyčištěna. Vyčištění Hello zahrnuje čištění objektů, které mají převzetí služeb při selhání z hello zdrojového zařízení, jako jsou svazky, zálohování objekty (ne data), kontejnery svazků a zásady.

OTÁZKY. **Co se stane, když hello odstranit úlohu přidruženou k hello kontejnery svazků v hello zdrojového zařízení se nezdaří?**

A.  Pokud hello odstranit úlohu selže, budete potřebovat toomanually aktivační události odstranění hello kontejnery svazků hello. V hello **zařízení** vyberte vaše zdrojové zařízení a klikněte na tlačítko **kontejnery svazků**. Vyberte hello kontejnery svazků, které při selhání přes a v hello dolní části stránky hello, klikněte na tlačítko **odstranit**. Jakmile jste odstranili všechny hello převzít služby při selhání kontejnery svazků na hello zdrojového zařízení, můžete spustit hello navrácení služeb po obnovení.

## <a name="business-continuity-disaster-recovery-bcdr"></a>Obchodní kontinuity zotavení po havárii (BCDR)
Obchodní kontinuity po havárii (BCDR) obnovení scénář nastane, když hello datové centrum Azure celý přestane fungovat. To může mít vliv služby StorSimple Manager a hello související zařízení StorSimple.

Pokud jsou zařízení StorSimple, které byly zaregistrovány těsně před došlo k havárii, pak tato zařízení StorSimple může být nutné tooundergo resetovat výrobní nastavení. Po havárii hello zařízení StorSimple hello budou zobrazeny v režimu offline. Hello zařízení StorSimple je nutné odstranit z portálu hello a obnovení továrního nastavení by mělo být provedeno, za nímž následuje novou registraci.

## <a name="next-steps"></a>Další kroky
* Po provedení převzetí služeb při selhání, můžete potřebovat příliš[deaktivujte nebo odstraňte zařízení StorSimple](storsimple-deactivate-and-delete-device.md).
* Informace o tom, jak toouse hello StorSimple Manager služby, přejděte příliš[použití hello tooadminister služby StorSimple Manager zařízení StorSimple](storsimple-manager-service-administration.md).

