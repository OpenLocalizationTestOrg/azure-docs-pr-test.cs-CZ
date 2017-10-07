---
title: "aaaStorSimple převzetí služeb při selhání, zotavení po havárii pro řady 8000 zařízení | Microsoft Docs"
description: "Zjistěte, jak toofail přes vaše tooitself zařízení StorSimple, jiné fyzické zařízení nebo zařízení cloudu."
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
ms.date: 05/03/2017
ms.author: alkohli
ms.openlocfilehash: 9d01ee30b15b77072b1d4dfe9a215abc83ffba28
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="failover-and-disaster-recovery-for-your-storsimple-8000-series-device"></a>Převzetí služeb při selhání a zotavení po havárii pro vaše zařízení řady StorSimple 8000

## <a name="overview"></a>Přehled

Tento článek popisuje funkce převzetí služeb při selhání hello zařízení pro zařízení řady StorSimple 8000 hello a jak tato funkce může být použité toorecover zařízení StorSimple, pokud dojde k havárii. StorSimple použije zařízení převzetí služeb při selhání toomigrate hello data ze zdrojového zařízení hello datacenter tooanother cílové zařízení. Hello pokyny v tomto článku se vztahuje tooStorSimple 8000 řady fyzických zařízení a zařízení cloudu spuštěná verze softwaru Update 3 nebo novější.

StorSimple používá hello **zařízení** funkce převzetí služeb při selhání okno toostart hello zařízení v případě havárie hello. Toto okno obsahuje seznam všech hello StorSimple zařízení připojených tooyour Správce zařízení StorSimple služby.

![Okno zařízení](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev1.png)


## <a name="disaster-recovery-dr-and-device-failover"></a>Zotavení po havárii (DR) a převzetí služeb při selhání zařízení

Ve scénáři zotavení po havárii hello primární zařízení přestane fungovat. Používá hello jako primární zařízení StorSimple _zdroj_ a přesune hello přidruženého cloudu data tooanother _cíl_ zařízení. Tento proces je hello odkazované tooas *převzetí služeb při selhání*. Hello následující obrázek znázorňuje proces hello převzetí služeb při selhání.

![Co se stane v zařízení převzetí služeb při selhání?](./media/storsimple-8000-device-failover-disaster-recovery/failover-dr-flow.png)

Hello cílové zařízení převzetí služeb při selhání může být fyzické zařízení nebo i o zařízení cloudu. Hello cílové zařízení může být umístěn v hello stejné nebo jiné zeměpisné umístění než hello zdrojového zařízení.

Během převzetí služeb při selhání hello můžete vybrat kontejnery svazků pro migraci. Hello vlastnictví tyto kontejnery svazků zařízení StorSimple poté změní z hello zdrojového zařízení toohello cílové zařízení. Jakmile kontejnery svazků hello změnit vlastnictví, odstraní StorSimple těchto kontejnerů z hello zdrojového zařízení. Po dokončení odstranění hello může selhat back hello cílové zařízení. _Navrácení služeb po obnovení_ přenosy hello vlastnictví back toohello původního zdrojového zařízení.

### <a name="cloud-snapshot-used-during-device-failover"></a>Cloudový snímek používá během převzetí služeb při selhání zařízení

Následující zotavení po Havárii je nejnovější zálohy cloudu hello použité toorestore hello data toohello cílové zařízení. Další informace o cloudových snímků, najdete v části [použít tootake služby StorSimple Manager zařízení hello ruční zálohy](storsimple-8000-manage-backup-policies-u2.md#take-a-manual-backup).

Zásady zálohování na řadu zařízení StorSimple 8000 nejsou přidružené zálohy. Pokud máte více zásady zálohování pro hello stejný svazek a potom vybere StorSimple hello zásada zálohování se hello největšího počtu svazků. StorSimple pak použije hello nejnovější zálohy z hello vybrané zásady zálohování toorestore hello dat na hello cílové zařízení.

Předpokládejme, že existují dvě zásady zálohování, *defaultPol* a *customPol*:

* *defaultPol*: jeden svazek, *vol1*, spustí denní od 22:30:00.
* *customPol*: čtyři svazky, *vol1*, *vol2*, *vol3*, *vol4*, spustí denní od 22:00:00.

V takovém případě StorSimple upřednostňuje pro konzistenci s havárií a používá *customPol* protože má více svazků. Hello nejnovější zálohy z této zásady je použité toorestore data. Další informace o tom, toocreate a spravovat zásady zálohování, přejděte příliš[použít zásady zálohování služby toomanage Správce zařízení StorSimple pro hello](storsimple-8000-manage-backup-policies-u2.md).

## <a name="common-considerations-for-device-failover"></a>Častá rozhodnutí při převzetí služeb při selhání zařízení

Než budete selhání zařízení, projděte si hello následující informace:

* Před zahájením selhání zařízení, musí být všechny svazky hello v rámci kontejnery svazků hello offline. V neplánované převzetí služeb při selhání svazky StotSimple automaticky přejde do režimu offline. Pokud provádíte plánované převzetí služeb při selhání (tootest DR), je však třeba provést všechny svazky hello do offline režimu.
* Pouze hello kontejnery svazků, které mají přiřazený cloudový snímek jsou uvedené pro zotavení po Havárii. Musí existovat alespoň jeden kontejner svazků daty toorecover snímku v přidružený cloud.
* Pokud existují cloudové snímky, které jsou rozmístěny napříč více kontejnery svazků, StorSimple převezme tyto kontejnery svazků jako sada. Ve výjimečných instance Pokud existují místní snímky, které jsou rozmístěny napříč více kontejnery svazků, ale nechcete snímky přidružený cloud, StorSimple převzetí služeb při selhání hello místní snímky a místní data hello dojde ke ztrátě po zotavení po Havárii.
* Hello dostupných cílových zařízení pro zotavení po Havárii jsou zařízení, která mají dostatek místa tooaccommodate hello kontejnery vybraný svazek. Všechna zařízení, které nemají dostatek místa, nejsou uvedené jako cílová zařízení.
* Po zotavení po Havárii (pro omezené trvání) výkon datové hello může mít vliv výrazně, jako hello zařízení potřebuje tooaccess hello data z cloudu hello a uložit místně.

#### <a name="device-failover-across-software-versions"></a>Zařízení převzetí služeb při selhání mezi verzemi softwaru

Service Manager zařízení StorSimple v nasazení může mít více zařízení, i fyzického a cloudu, všechny verze spuštění různých softwaru.

Pomocí následující tabulky toodetermine, pokud jste převzetí služeb při selhání nebo selhání back tooanother zařízení se systémem verzi jiný software a chování hello svazku typy během zotavení po Havárii hello.

#### <a name="failover-and-failback-across-software-versions"></a>Převzetí služeb při selhání a navrácení služeb po obnovení mezi verzemi softwaru

| Převzetí služeb při selhání / po obnovení vrátit | Fyzické zařízení | Cloudové zařízení |
| --- | --- | --- |
| TooUpdate Update 3 4 |Vrstvené svazky přes vrstvené nezdaří. <br></br>Místně vázaných svazků převzetí služeb při selhání jako místně vázaný. <br></br> Po převzetí služeb při pořízení snímku na zařízení hello aktualizace 4 [na základě heatmap sledování](storsimple-update4-release-notes.md#whats-new-in-update-4) dojde k. |Místně vázaný svazky převzetí služeb při selhání jako vrstvené. |
| Aktualizace 4 tooUpdate 3 |Vrstvené svazky přes vrstvené nezdaří. <br></br>Místně vázaných svazků převzetí služeb při selhání jako místně vázaný. <br></br> Zálohování použít toorestore zachovat heatmap metadat. <br></br>Na základě Heatmap sledování není k dispozici ve verzi Update 3 následující navrácení služeb po obnovení. |Místně vázaný svazky převzetí služeb při selhání jako vrstvené. |


## <a name="device-failover-scenarios"></a>Scénáře převzetí služeb při selhání zařízení

Pokud dojde k havárii, můžete se rozhodnout toofail přes zařízení StorSimple:

* [fyzické zařízení tooa](storsimple-8000-device-failover-physical-device.md).
* [tooitself](storsimple-8000-device-failover-same-device.md).
* [zařízení cloudu tooa](storsimple-8000-device-failover-cloud-appliance.md).

Hello předchozí články poskytují podrobné pokyny pro každou hello výše případech převzetí služeb při selhání.


## <a name="failback"></a>Navrácení služeb po obnovení

Pro Update 3 a novějších verzích podporuje StorSimple také navrácení služeb po obnovení. Navrácení služeb po obnovení je právě hello provést převzetí služeb při selhání, cílový hello se stane zdrojem hello a hello původního zdrojového zařízení během hello převzetí služeb při selhání teď stane hello cílové zařízení. 

Během navrácení služeb po obnovení StorSimple znovu synchronizuje hello data zpět toohello primární umístění, zastaví hello vstupně-výstupních operací a aktivitě aplikace a přejde zpět toohello původního umístění.

Po dokončení převzetí služeb StorSimple provádí hello následující akce:

* StorSimple vyčistí hello kontejnery svazků, které jsou při selhání z hello zdrojového zařízení.
* StorSimple spustí úlohu na pozadí na kontejneru svazků (převzetí služeb při selhání) na hello zdrojového zařízení. Když zkusíte toofail zpět, zatímco probíhá úloha hello, obdržíte efekt toothat oznámení. Počkejte, dokud je úloha hello dokončení toostart hello navrácení služeb po obnovení.
* Hello doba trvání odstranění hello toocomplete kontejnery svazků závisí na různých faktorech, jako je množství dat, stáří dat hello, počet záloh a hello šířku pásma sítě dostupnou pro operaci hello.

Pokud jsou plánování testovací převzetí služeb při selhání nebo testovací navrácení služeb po obnovení, doporučujeme, abyste otestovali kontejnery svazků s méně dat (GB). Obvykle můžete spustit hello navrácení služeb po obnovení 24 hodin po dokončení převzetí služeb při selhání hello.

## <a name="frequently-asked-questions"></a>Nejčastější dotazy

OTÁZKY. **Co se stane, když hello zotavení po Havárii se nezdaří, nebo má částečný úspěch?**

A. Pokud selže hello zotavení po Havárii, doporučujeme vám, opakujte akci. Hello druhé zařízení převzetí služeb při selhání úlohy si je vědoma hello průběh hello první úlohy a spustí z tohoto bodu a vyšší.

OTÁZKY. **Můžete odstranit zařízení, když probíhá hello zařízení převzetí služeb při selhání?**

A. Zařízení nelze odstranit, když probíhá zotavení po Havárii. Zařízení může odstranit pouze po dokončení hello zotavení po Havárii. Můžete sledovat průběh úlohy převzetí služeb při selhání hello zařízení v hello **úlohy** okno.

OTÁZKY. **Pokud hello uvolňování paměti spusťte na hello zdrojového zařízení, tak, aby hello místní data na zdrojového zařízení se odstraní?**

A. Uvolňování paměti je zapnuta hello zdrojového zařízení až po hello zařízení je zcela vyčištěna. Vyčištění Hello zahrnuje čištění objektů, které mají převzetí služeb při selhání z hello zdrojového zařízení, jako jsou svazky, zálohování objekty (ne data), kontejnery svazků a zásady.

OTÁZKY. **Co se stane, když hello odstranit úlohu přidruženou k hello kontejnery svazků v hello zdrojového zařízení se nezdaří?**

A.  Pokud hello odstranění úlohy nezdaří, můžete ručně odstranit kontejnery svazků hello. V hello **zařízení** okně vyberte vaše zdrojové zařízení a klikněte na **kontejnery svazků**. Vyberte hello kontejnery svazků, které při selhání přes a v hello dolní části okna hello, klikněte na tlačítko **odstranit**. Po odstranění všech hello převzít služby při selhání kontejnery svazků na hello zdrojového zařízení, můžete spustit hello navrácení služeb po obnovení. Další informace, přejděte příliš[odstranit kontejner svazků](storsimple-8000-manage-volume-containers.md#delete-a-volume-container).

## <a name="business-continuity-disaster-recovery-bcdr"></a>Obchodní kontinuity zotavení po havárii (BCDR)

Obchodní kontinuity po havárii (BCDR) obnovení scénář nastane, když hello datové centrum Azure celý přestane fungovat. Tento scénář může mít vliv na služby StorSimple Manager zařízení a hello související zařízení StorSimple.

Pokud zařízení StorSimple byl zaregistrován těsně před došlo k havárii, pak toto zařízení může být nutné tooundergo resetovat výrobní nastavení. Po havárii hello zařízení StorSimple hello se zobrazí v hello portálu Azure jako offline. Toto zařízení musí být odstraněna z portálu hello. Obnovit výchozí nastavení toofactory hello zařízení a zaregistrujte ho znovu službou hello.

## <a name="next-steps"></a>Další kroky

Pokud jste připravené tooperform selhání zařízení, vyberte jednu z hello následující scénáře pro podrobné pokyny:

- [Selhání tooanother fyzického zařízení](storsimple-8000-device-failover-physical-device.md)
- [Převzetí služeb při selhání toohello stejné zařízení](storsimple-8000-device-failover-same-device.md)
- [Selhání tooStorSimple cloudu zařízení](storsimple-8000-device-failover-cloud-appliance.md)

Pokud jste při selhání zařízení, vyberte jednu z hello následující možnosti:

* [Deaktivovat nebo odstranit zařízení StorSimple](storsimple-8000-deactivate-and-delete-device.md).
* [Použití zařízení StorSimple hello tooadminister služby StorSimple Manager zařízení](storsimple-8000-manager-service-administration.md).

