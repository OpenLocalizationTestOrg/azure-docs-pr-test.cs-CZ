---
title: "aaaStorSimple virtuální pole po havárii obnovení a zařízení převzetí služeb při selhání | Microsoft Docs"
description: "Další informace o toofailover pole virtuální zařízení StorSimple."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 3c1f9c62-af57-4634-a0d8-435522d969aa
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5f125efd1ffb94489cdfa7cfaafae7d57cc10131
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="disaster-recovery-and-device-failover-for-your-storsimple-virtual-array-via-azure-portal"></a>Po havárii obnovení a zařízení převzetí služeb při selhání pro virtuální zařízení StorSimple pole prostřednictvím portálu Azure

## <a name="overview"></a>Přehled
Tento článek popisuje hello zotavení po havárii pro Microsoft Azure StorSimple virtuální pole včetně hello podrobné kroky toofail přes tooanother virtuální pole. Převzetí služeb při selhání vám umožní toomove svá data z *zdroj* zařízení v datovém centru tooa hello *cíl* zařízení. Hello cílové zařízení může být umístěn v hello stejné nebo jiné zeměpisné umístění. Hello zařízení převzetí služeb při selhání je pro zařízení hello. Data v cloudu hello hello zdrojového zařízení během převzetí služeb při selhání, změní vlastnictví toothat hello cílové zařízení.

Tento článek je použít tooStorSimple pouze virtuální pole. toofail přes zařízení s řady 8000 přejděte příliš[zařízení převzetí služeb při selhání a zotavení po havárii zařízení StorSimple](storsimple-device-failover-disaster-recovery.md).

## <a name="what-is-disaster-recovery-and-device-failover"></a>Co je po havárii obnovení a zařízení převzetí služeb při selhání?

Ve scénáři zotavení po havárii hello primární zařízení přestane fungovat. V tomto scénáři můžete přesouvat data v cloudu hello přidružené zařízení tooanother hello zařízení se nezdařilo. Můžete použít primární zařízení hello jako hello *zdroj* a zadejte jiné zařízení jako hello *cíl*. Tento proces je hello odkazované tooas *převzetí služeb při selhání*. Během převzetí služeb při selhání všechny hello svazky nebo sdílené složky hello ze zdrojového zařízení hello změnit vlastnictví a jsou přenášená toohello cílové zařízení. Žádné filtrování dat hello je povolen.

Zotavení po Havárii je modelovaná jako zařízení úplné obnovení pomocí hello heat mapa – na základě vrstvení a sledování. Heat mapa je definována přiřazením hodnota toohello heat, na základě dat pro čtení a zápis vzory. Tato heat mapovat pak nejnižší heat datové bloky toohello cloudu vrstev nejprve hello zachováním hello vysoké heat (většina používá) datových bloků v místní vrstvy hello. Během zotavení po Havárii StorSimple používá hello heat mapa toorestore a rehydrataci při spotřebě hello data z cloudu hello. zařízení Hello načte všechny hello svazky nebo složky v poslední poslední zálohu hello (jak stanoví interně) a provádí obnovení z této zálohy. pole virtuálním Hello orchestruje hello celý proces zotavení po Havárii.

> [!IMPORTANT]
> Hello zdrojového zařízení se odstraní na konci hello zařízení převzetí služeb při selhání, a proto není podporována navrácení služeb po obnovení.
> 
> 

Zotavení po havárii je pomocí funkce převzetí služeb při selhání zařízení hello orchestrovat a je zahájeno z hello **zařízení** okno. Toto okno podporován všechny hello StorSimple zařízení připojených tooyour Správce zařízení StorSimple služby. Pro každé zařízení můžete zobrazit hello popisný název, stav, zřízený a maximální kapacitu, typ a modelu.

## <a name="prerequisites-for-device-failover"></a>Předpoklady pro převzetí služeb při selhání zařízení

### <a name="prerequisites"></a>Požadavky

Zařízení převzetí služeb při selhání Ujistěte se ujistí, že hello následující požadavky:

* Hello zdrojového zařízení potřebuje toobe v **deaktivováno** stavu.
* Hello cílové zařízení potřebuje tooshow až jako **připraveni tooset až** v hello portálu Azure. Zřízení cílového virtuální pole Dobrý den stejné nebo vyšší kapacitě. Použijte hello místního webového uživatelského rozhraní tooconfigure a úspěšně registroval hello cílového virtuální pole.
  
  > [!IMPORTANT]
  > Nepokoušejte tooconfigure hello registrované virtuálního zařízení prostřednictvím služby hello. Žádná konfigurace zařízení je třeba provést prostřednictvím služby hello.
  > 
  > 
* Hello cílové zařízení nemůže mít stejný název jako hello zdrojového zařízení hello.
* Hello zdrojové a cílové zařízení mít toobe hello stejného typu. Pouze můžete převzít virtuální pole nakonfigurovaný jako souborový server tooanother souborový server. Hello totéž platí pro server se službou iSCSI.
* Pro souborový server zotavení po Havárii, doporučujeme připojení k hello cílové zařízení toohello stejné domény jako zdroj hello. Tato konfigurace zajistí, že jsou oprávnění k sdíleným složkám hello vyřešit automaticky. Pouze hello převzetí služeb při selhání tooa cílové zařízení v hello stejné domény.
* zařízení, která mají hello stejné nebo větší kapacitu porovnání toohello zdrojového zařízení jsou Hello dostupných cílových zařízení pro zotavení po Havárii. Hello zařízení, které jsou připojené tooyour služby, ale hello nesplňují kritéria dostatek místa nejsou k dispozici jako cílová zařízení.

### <a name="other-considerations"></a>Další důležité informace

* Plánované převzetí služeb při selhání 
  
  * Doporučujeme provést všechny hello svazky nebo sdílené složky na hello zdrojového zařízení do offline režimu.
  * Doporučujeme provést zálohu hello zařízení a poté pokračovat ztráty dat toominimize hello převzetí služeb při selhání. 
* Neplánované převzetí služeb při selhání hello zařízení používá hello nejnovější zálohy toorestore hello data.

### <a name="device-failover-prechecks"></a>Zařízení prechecks převzetí služeb při selhání

Před hello zotavení po Havárii je spuštěn v okamžiku provede hello zařízení prechecks. Těmito kontrolami pomáhají zajistit, že nevyskytnou žádné chyby při zahájí zotavení po Havárii. Hello prechecks patří:

* Probíhá ověřování účtu úložiště hello.
* Kontrola, zda text hello cloudu připojení tooAzure.
* Kontrola dostupného místa na cílové zařízení hello.
* Kontrola, zda má svazek iSCSI zdrojového serveru zařízení
  
  * platné názvy ACR.
  * Neplatný název IQN (ne vyšší než 220 znaků).
  * platný hesla CHAP (12 až 16 znaků).

Pokud jsou některé z předchozích prechecks hello selhání, nemůže pokračovat s hello zotavení po Havárii. Tyto potíže odstranit a poté opakujte zotavení po Havárii.

Po úspěšném dokončení hello zotavení po Havárii, je hello vlastnictví hello data v cloudu na zařízení zdroj hello přenášená toohello cílové zařízení. Hello zdrojového zařízení je pak už v hello portálu k dispozici. Blokovaný přístup tooall hello svazků nebo sdílených složek na hello zdrojového zařízení a hello cílové zařízení stane aktivní.

> [!IMPORTANT]
> I když hello zařízení již není k dispozici, je hello virtuálního počítače, kterou jste zřídili v systému hostitele hello nadále spotřebovávají prostředky. Po úspěšném dokončení hello zotavení po Havárii můžete odstranit tento virtuální počítač z vašeho systému hostitele.
> 
> 

## <a name="fail-over-tooa-virtual-array"></a>Selhání tooa virtuální pole

Doporučujeme zřídit, nakonfigurovat a zaregistrovat další pole virtuální zařízení StorSimple pomocí služby StorSimple Manager zařízení před spuštěním tohoto postupu.

> [!IMPORTANT]
> 
> * Nelze převzít z StorSimple 8000 řady zařízení tooa 1200 virtuálního zařízení.
> * Můžete převzít z informace o zpracování Standard FIPS (Federal) povoleno virtuálního zařízení tooanother FIPS povolené zařízení nebo zařízení bez FIPS tooa nasadit hello Government portálu.


Proveďte následující kroky toorestore hello zařízení tooa cílový virtuální zařízení StorSimple hello.

1. Zřídíte a nakonfigurujete cílového zařízení, která splňuje hello [požadavky pro převzetí služeb při selhání zařízení](#prerequisites). Dokončete konfiguraci zařízení hello prostřednictvím hello místního webového uživatelského rozhraní a zaregistrovat ji tooyour service Manager zařízení StorSimple. Pokud vytváříte souborový server, přejděte toostep 1 z [nastavit jako souborový server](storsimple-virtual-array-deploy3-fs-setup.md#step-1-complete-the-local-web-ui-setup-and-register-your-device). Pokud vytváříte serveru iSCSI, přejděte toostep 1 z [nastavený jako iSCSI server](storsimple-virtual-array-deploy3-iscsi-setup.md#step-1-complete-the-local-web-ui-setup-and-register-your-device).

2. Na hostiteli hello trvat svazky nebo sdílené složky do offline režimu. tootake hello svazky nebo sdílené složky v režimu offline, odkazovat toohello operačního systému – konkrétní pokyny pro hostitele hello. Pokud již offline, je třeba tootake všechny hello svazky nebo sdílené složky do režimu offline v zařízení hello provedením následujících hello.
   
    1. Přejděte příliš**zařízení** okno a vybrat zařízení.
   
    2. Přejděte příliš**Nastavení > Správa > sdílené složky** (nebo **Nastavení > Správa > svazky**). 
   
    3. Vyberte sdílené složky nebo svazek, klikněte pravým tlačítkem a vyberte **převést do režimu offline**. 
   
    4. Po zobrazení výzvy k potvrzení, zkontrolujte **beru na vědomí hello dopad této sdílené složce přepnutím do režimu offline.** 
   
    5. Klikněte na tlačítko **převést do režimu offline**.

3. Ve službě StorSimple Manager zařízení přejděte příliš**správy > zařízení**. V hello **zařízení** okně vyberte a klikněte na vaše zdrojové zařízení.

4. Ve vaší **řídicí panel zařízení** okně klikněte na tlačítko **deaktivovat**.

5. V hello **deaktivovat** okně se zobrazí výzva k potvrzení. Deaktivace zařízení je *trvalé* proces, který nelze vrátit zpět. Můžete se také upozorněni tootake sdílených složek nebo svazků do offline režimu na hostiteli hello. Zadejte hello zařízení název tooconfirm a klikněte na **deaktivovat**.
   
    ![](./media/storsimple-virtual-array-failover-dr/failover1.png)
6. deaktivace Hello spustí. Po úspěšném dokončení hello deaktivace obdržíte oznámení.
   
    ![](./media/storsimple-virtual-array-failover-dr/failover2.png)
7. Na stránce zařízení hello hello zařízení stavu bude nyní změnit příliš**deaktivováno**.
    ![](./media/storsimple-virtual-array-failover-dr/failover3.png)
8. V hello **zařízení** , vyberte a klikněte na hello deaktivované zdrojového zařízení pro převzetí služeb při selhání. 
9. V hello **řídicí panel zařízení** okně klikněte na tlačítko **převzetí služeb při selhání**. 
10. V hello **převzít zařízení** okně hello následující:
    
    1. se automaticky vyplní pole zařízení zdroj Hello. Všimněte si hello celkové velikosti dat pro hello zdrojového zařízení. velikost datového Hello musí být menší než hello dostupné kapacity na hello cílové zařízení. Zkontrolujte podrobnosti hello přidružené hello zdrojového zařízení, jako je například název zařízení, celková kapacita a názvy hello hello sdílených složek, které jsou převzetí služeb při selhání.

    2. Z rozevíracího seznamu hello zařízení k dispozici, zvolte **cílové zařízení**. V rozevíracím seznamu hello se zobrazí pouze hello zařízení, které mají dostatečnou kapacitu.

    3. Zkontrolujte, zda **beru na vědomí, že tato operace bude převzít data toohello cílové zařízení**. 

    4. Klikněte na tlačítko **převzetí služeb při selhání**.
    
        ![](./media/storsimple-virtual-array-failover-dr/failover4.png)
11. Spustí úlohu převzetí služeb při selhání a přijímat oznámení. Přejděte příliš**zařízení > úlohy** toomonitor hello převzetí služeb při selhání.
    
     ![](./media/storsimple-virtual-array-failover-dr/failover5.png)
12. V hello **úlohy** okně uvidíte převzetí služeb při selhání úlohy vytvořené pro hello zdrojového zařízení. Tato úloha provede prechecks hello zotavení po Havárii.
    
    ![](./media/storsimple-virtual-array-failover-dr/failover6.png)
    
     Po úspěšné prechecks hello zotavení po Havárii se bude hello převzetí služeb při selhání úlohy spawn úlohy obnovení pro každou sdílenou složku nebo svazek, který na vaše zdrojové zařízení existuje.
    
    ![](./media/storsimple-virtual-array-failover-dr/failover7.png)
13. Po dokončení, přejděte toohello hello převzetí služeb při selhání **zařízení** okno.
    
    1. Vyberte a klikněte na zařízení StorSimple hello, která byla použita jako hello cílové zařízení pro proces převzetí služeb při selhání hello.
    2. Přejděte příliš**Nastavení > Správa > sdílené složky** (nebo **svazky** Pokud iSCSI server). V hello **sdílené složky** okno, můžete zobrazit všechny složky hello (svazky) z původního zařízení hello.
        ![](./media/storsimple-virtual-array-failover-dr/failover9.png)
14. Budete potřebovat příliš[vytvoření aliasu DNS](https://support.microsoft.com/kb/168322) tak, aby všechny aplikace, které se pokoušíte hello tooconnect můžete získat přesměrovaného toohello nové zařízení.

## <a name="errors-during-dr"></a>Chyby během zotavení po Havárii

**Výpadek připojení cloudu během zotavení po Havárii**

Pokud připojení cloudu hello dojde k narušení po zotavení po Havárii začala a před dokončením hello zařízení obnovení, hello zotavení po Havárii se nezdaří. Dostanete oznámení, failore. Hello cílové zařízení pro zotavení po Havárii je označena jako *nepoužitelný.* Nemůžete použít hello stejné cílové zařízení pro budoucí DRs.

**Žádné kompatibilní cílových zařízení**

Pokud hello dostupných cílových zařízení nemáte k dispozici dostatek místa, zobrazí chyba toohello vliv, nejsou kompatibilní cílové zařízení.

**Precheck selhání**

Pokud není splněna jedna z hello prechecks, uvidíte precheck selhání.

## <a name="business-continuity-disaster-recovery-bcdr"></a>Obchodní kontinuity zotavení po havárii (BCDR)

Obchodní kontinuity po havárii (BCDR) obnovení scénář nastane, když hello datové centrum Azure celý přestane fungovat. To může mít vliv služby StorSimple Manager zařízení a hello související zařízení StorSimple.

Pokud jsou zařízení StorSimple, které byly zaregistrovány těsně před došlo k havárii, pak tato zařízení StorSimple může být nutné toobe odstranit. Po havárii hello můžete znovu vytvořit a nakonfigurovat tato zařízení.

## <a name="next-steps"></a>Další kroky

Další informace o příliš[spravovat vaše pole virtuální zařízení StorSimple pomocí hello místního webového uživatelského rozhraní](storsimple-ova-web-ui-admin.md).

