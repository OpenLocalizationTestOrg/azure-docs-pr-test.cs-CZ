---
title: "aaaManage svazky zařízení StorSimple (Update 3) | Microsoft Docs"
description: "Vysvětluje, jak tooadd, upravit, monitorování a odstranit svazky zařízení StorSimple a tootake je v režimu offline v případě potřeby."
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
ms.workload: NA
ms.date: 07/19/2017
ms.author: alkohli
ms.openlocfilehash: 6228c4486dd5a7887df670c4c4584c4edcdfc509
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-toomanage-volumes-update-3-or-later"></a>Použijte svazky služby toomanage hello Správce zařízení StorSimple (Update 3 nebo novější)

## <a name="overview"></a>Přehled

Tento kurz vysvětluje, jak toouse hello toocreate service Manager zařízení StorSimple a spravovat svazky na řadu zařízení StorSimple 8000 hello verzi Update 3 nebo novější.

Hello služby StorSimple Manager zařízení je rozšíření hello portálu Azure, která umožňuje spravovat z jedné webové rozhraní vašeho řešení StorSimple. Použijte hello Azure portálu toomanage svazky na všechna zařízení. Můžete také vytvořit a spravovat služby StorSimple, spravovat zařízení, zásady zálohování a zálohování katalogu a zobrazovat výstrahy.

## <a name="volume-types"></a>Typy svazku

Svazky zařízení StorSimple může být:

* **Místně vázaný svazky**: Data v těchto svazcích zůstane na místním zařízení StorSimple hello za všech okolností.
* **Zřízeny vrstvené svazky**: Data v těchto svazků můžete distribuována toohello cloudu.

Archivace svazek je typ vrstvený svazek. Hello větší odstranění duplicit velikost bloku použít pro archivaci svazky umožňuje hello zařízení tootransfer větší segmentů toohello dat v cloudu.

V případě potřeby můžete změnit typ svazku hello z místní tootiered nebo vrstvené toolocal. Další informace, přejděte příliš[změnit typ svazku hello](#change-the-volume-type).

### <a name="locally-pinned-volumes"></a>Místně vázaných svazků

Místně vázaných svazků jsou zcela zřizované svazky, které není vrstvy toohello dat v cloudu, a zajistí tak místní záruky pro primární data, nezávisle na cloudu připojení. Není data na místně vázaných svazků s odstraněním duplicitních dat a komprimované; Nicméně jsou odstranění duplicit snímky místně vázaných svazků. 

Místně vázaných svazků jsou plně zřízený; proto musí mít dostatek místa na vašem zařízení, při jejich vytváření. Můžete zřídit místně vázaných svazků až tooa maximální velikosti 8 TB na zařízení hello StorSimple 8100 a 20 TB na zařízení 8600 hello. StorSimple si vyhrazuje zbývající volné místo na hello zařízení pro snímky, metadat a zpracování dat hello. Můžete zvýšit velikost hello místně vázaný svazek toohello maximální prostoru k dispozici, ale nelze snížit velikost svazku po vytvoření hello.

Když vytvoříte místně vázaný svazek, se snižuje hello volné místo pro vytvoření vrstvené svazky. Hello zpětné platí také: Pokud máte vrstvené svazky, bude místo hello dostupné pro vytváření místně připojené svazky menší než maximální limit hello uvedené výše. Další informace o místní svazky, najdete v části toohello [nejčastější dotazy na místně vázaných svazků](storsimple-8000-local-volume-faq.md).

### <a name="tiered-volumes"></a>Vrstvené svazky

Vrstvené svazky jsou dynamicky zřizované svazky, ve které hello často používaná data zůstává v zařízení hello místní a méně často používaná data se automaticky vrstvené toohello cloudu. Dynamické zajišťování je technologie virtualizace, ve kterém úložiště k dispozici, zobrazí se tooexceed fyzické prostředky. Místo předem rezervování dostatečné úložiště, StorSimple používá dynamického zřizování tooallocate akorát toomeet aktuální požadavky na volné místo. Hello elastické povaha cloudového úložiště usnadňuje tento přístup, protože StorSimple můžete zvýšit nebo snížit cloudové úložiště toomeet splněné měnící se požadavky.

Pokud používáte vrstvený svazek pro archivní data, vyberte hello hello **použít tento svazek pro archivní data s méně často používaná** velikost bloku políčko toochange hello odstranění duplicitních dat pro svazek too512 KB. Pokud tuto možnost nevyberete, hello příslušný vrstvený svazek bude používat velikost bloku dat 64 KB. Větší velikost bloku odstranění duplicitních dat umožňuje hello zařízení tooexpedite hello přenos velkých objemů archivních dat toohello cloudu.


### <a name="provisioned-capacity"></a>Zřízená kapacita

Následující tabulka pro maximální zřízená kapacita pro každý typ zařízení a svazek toohello naleznete v nápovědě. (Všimněte si, že nejsou dostupné na virtuálním zařízení místně vázaných svazků.)

|  | Vrstvený svazek maximální velikost | Maximální místně připnutý velikost svazku |
| --- | --- | --- |
| **Fyzické zařízení** | | |
| 8100 |64 TB |8 TB |
| 8600 |64 TB |20 TB |
| **Virtuální zařízení** | | |
| 8010 |30 TB |Není k dispozici |
| 8020 |64 TB |Není k dispozici |

## <a name="hello-volumes-blade"></a>okno svazky Hello

Hello **svazky** okno vám umožní toomanage hello úložné svazky, které jsou zřízené v hello Microsoft Azure StorSimple zařízení pro vaše iniciátorů (serverů). Zobrazuje hello seznam svazků na službě připojené tooyour zařízení StorSimple hello.

 ![Stránka svazky](./media/storsimple-8000-manage-volumes-u2/volumeslist.png)

Svazek se skládá z řady atributy:

* **Název svazku** – popisný název, který musí být jedinečný a pomáhá identifikovat hello svazku. Tento název se také používá v monitorování sestavy při filtrování na konkrétním svazku. Po vytvoření svazku hello nelze přejmenovat.
* **Stav** – může být online nebo offline. Pokud je svazek offline, není viditelné tooinitiators (servery) povolený přístup toouse hello svazku.
* **Kapacita** – určuje hello celkové množství dat, které můžou být uložené ve iniciátor hello (server). Místně vázaný svazky jsou plně zřízený a jsou umístěny na zařízení StorSimple hello. Vrstvené svazky jsou tence zřízený a hello data se odstraňují. S dynamicky zřizované svazky nebude zařízení předem přidělit kapacity fyzického úložiště, interně, nebo v cloudu hello podle tooconfigured kapacita svazku. kapacita svazku Hello je přidělené a využívat na vyžádání.
* **Typ** – Určuje, zda je svazek hello **nastavování** (hello výchozí) nebo **místně vázaný**.

Používejte hello pokyny v této kurz tooperform hello následující úlohy:

* Přidání svazku 
* Upravit svazku 
* Změnit typ svazku hello
* Odstranění svazku 
* Svazek převést do režimu offline 
* Monitorování svazku 

## <a name="add-a-volume"></a>Přidání svazku

Můžete [vytvořili svazek](storsimple-8000-deployment-walkthrough-u2.md#step-6-create-a-volume) během nasazování zařízení řady StorSimple 8000. Přidání svazku je podobným způsobem.

#### <a name="tooadd-a-volume"></a>tooadd svazku

1. Z hello tabulkové seznam zařízení hello v hello **zařízení** okně vyberte příslušné zařízení. Klikněte na **+ Přidat svazek**.

    ![Přidání nového svazku](./media/storsimple-8000-manage-volumes-u2/step5createvol1.png)

2. V hello **přidat svazek** okno:
   
    1. Hello **vyberte zařízení** s aktuální zařízení se automaticky vyplní pole.

    2. Hello rozevíracím seznamu vyberte kontejner svazků hello, kde je nutné tooadd svazku.

    3.  Zadejte **Název** svazku. Po vytvoření svazku hello nelze přejmenovat hello svazku.

    4. V rozevíracím seznamu hello, vyberte hello **typ** svazku. Pro úlohy, které vyžadují místní záruky, nízkou latenci a vyšší výkon, vyberte typ svazku **Místně vázaný**. Pro ostatní typy dat vyberte typ svazku **Vrstvený**. Pokud tento svazek používáte k archivaci dat, zaškrtněte políčko **Použít tento svazek pro archivní data s méně častým přístupem**.
      
       Vrstvený svazek je zřizovaný dynamicky a lze jej rychle vytvořit. Výběr **použít tento svazek pro archivní data s méně často používaná** pro vrstvený svazek určený pro velikost bloku archivní data změny hello odstranění duplicitních dat pro svazek too512 KB. Pokud políčko není zaškrtnuté, příslušný vrstvený svazek hello používá velikost bloku dat 64 KB. Větší velikost bloku odstranění duplicitních dat umožňuje hello zařízení tooexpedite hello přenos velkých objemů archivních dat toohello cloudu.
       
       Místně vázaný svazek je tlustě zřízený a zajišťuje, že hello primární dat na svazku hello zůstává místní toohello zařízení a nebudou distribuována toohello cloudu.  Pokud vytvoříte místně vázaný svazek, zařízení hello kontrolovat dostupný prostor v místních vrstvách hello tooprovision hello objem hello požadovanou velikost. Hello operaci vytváření místně vázaný svazek může být existující data z cloudu toohello hello zařízení a doba hello toocreate hello svazku může trvat dlouho. Celkový čas Hello závisí na velikosti hello hello zřídit svazek, dostupnou šířku pásma sítě a hello dat ve vašem zařízení.

    5. Zadejte hello **zřízená kapacita** svazku. Poznamenejte si hello kapacity, která je k dispozici podle vybraného typu svazku hello. Hello zadat velikost svazku nesmí překročit hello volné místo.
      
       Můžete zřídit místně vázaných svazků až too8.5 TB a vrstvené svazky až too200 TB na zařízení 8100 hello. Větší zařízení 8600 hello můžete zřídit místně vázaných svazků až too22.5 TB a vrstvené svazky až too500 TB. Volné místo v zařízení hello je požadovaná toohost hello pracovní sady vrstvených svazků, vytváření místně vázaných svazků ovlivní hello místa ke zřizování vrstvených svazků. Proto pokud vytvoříte místně připojený svazek, zmenší se dostupné volné místo pro vytváření vrstvených svazků. Podobně pokud vrstvený svazek je vytvořen, se snižuje hello volného prostoru vytváření místně vázaných svazků.
      
       Pokud v zařízení 8100 zřídíte místně vázaný svazek šířka 8,5 TB (maximální možná velikost), vyčerpáte všechny hello volné místo dostupné v zařízení hello. Nelze vytvořit žádné vrstvené svazky od tohoto okamžiku už nezbude žádné volné místo na hello zařízení toohost hello pracovní sadu hello vrstvené svazku. Hello místa ovlivňují také vrstvené svazky existující. Pokud například používáte zařízení 8100, ve kterém jsou už zřízeny vrstvené svazky o velikosti zhruba 106 TB, k vytváření místně vázaných svazků zbude už jenom 4 TB dostupného volného místa.

    6. V hello **připojení hostitele** pole, klikněte na šipku hello. 

        ![Připojení hostitelé](./media/storsimple-8000-manage-volumes-u2/step5createvol2.png)

    7. V hello **připojení hostitele** okně zvolte existující ACR nebo přidat nový ACR. Pokud si zvolíte nové ACR, potom zadejte **název** acr, zadejte hello **iSCSI Qualified Name** (IQN) hostitele s Windows. Pokud hello IQN nemáte, přejděte příliš[Get hello názvu IQN hostitele Windows serveru](#get-the-iqn-of-a-windows-server-host). Klikněte na možnost **Vytvořit**. Svazek je vytvořen s hello zadané nastavení.

        ![Kliknutí na Vytvořit](./media/storsimple-8000-manage-volumes-u2/step5createvol3.png)

Nový svazek je nyní připraven toouse.

> [!NOTE]
> Pokud vytvoříte místně vázaný svazek a pak vytvořit jiné místně připnuli svazku okamžitě později, úlohy vytvoření svazku hello spouští sekvenčně. Úloha vytvoření svazku první Hello musí ukončit předtím, než můžete začít hello další úloha vytvoření svazku.

## <a name="modify-a-volume"></a>Upravit svazku

Upravte svazek, když potřebujete tooexpand ho nebo změňte hello hostitele, kteří přístup hello svazku.

> [!IMPORTANT]
> * Pokud změníte velikost svazku hello na hello zařízení, musí velikost svazku hello toobe změnit také v hostiteli se hello.
> * Hello hostitelské kroků popsaných v tomto poli se pro Windows Server 2012 (2012 R2). Postupy pro Linux nebo jiných hostitelských operačních systémech se liší. Tooyour hostitele operačního systému pokyny najdete v případě úpravy hello svazek na hostitele s jiným operačním systémem.

#### <a name="toomodify-a-volume"></a>toomodify svazku

1. Přejděte služby StorSimple Manager zařízení tooyour a pak klikněte na tlačítko **zařízení**. Hello tabulkové výpis hello zařízení, vyberte hello zařízení, která obsahuje hello svazku, že máte v úmyslu toomodify. Klikněte na tlačítko **Nastavení > svazky**.

    ![Přejděte tooVolumes okno](./media/storsimple-8000-manage-volumes-u2/modifyvol2.png)

2. Hello tabulkové seznam svazků vyberte hello svazku a klikněte pravým tlačítkem na tooinvoke hello kontextové nabídky. Vyberte **převést do režimu offline** svazku hello tootake upravíte v režimu offline.

    ![Vyberte a do offline režimu svazku](./media/storsimple-8000-manage-volumes-u2/modifyvol4.png)

3. V hello **převést do režimu offline** okně zkontrolovat dopad hello hello svazku přepnutím do režimu offline a zaškrtněte odpovídající políčko hello. Zkontrolujte, zda text hello odpovídající svazek na hostiteli hello offline nejdřív. Informace o tom, jak tootake svazek offline na hostitelském serveru připojený tooStorSimple najdete v části konkrétní pokyny týkající se systému toooperating. Klikněte na tlačítko **převést do režimu offline**.

    ![Zkontrolujte dopad svazku přepnutím do režimu offline](./media/storsimple-8000-manage-volumes-u2/modifyvol5.png)

4. Po hello svazek je offline (jak ukazují stav svazku hello), vyberte svazek hello a klikněte pravým tlačítkem na tooinvoke hello kontextové nabídky. Vyberte **upravit svazek**.

    ![Vyberte Změnit svazku](./media/storsimple-8000-manage-volumes-u2/modifyvol9.png)


5. V hello **upravit svazek** okno, můžete provést hello následující změny:
   
   1. Hello svazku **název** nelze upravit.
   2. Převést hello **typ** z místně vázaný tootiered nebo vrstvené toolocally připnutý (najdete v části [změnit typ svazku hello](#change-the-volume-type) informace).
   3. Zvýšit hello **zřízená kapacita**. Hello **zřízená kapacita** může být pouze zvýšena. Po vytvoření, nelze zmenšit svazek.
   4. V části **připojení hostitele**, můžete upravit hello ACR. toomodify ACR hello svazek musí být v režimu offline.

       ![Zkontrolujte dopad svazku přepnutím do režimu offline](./media/storsimple-8000-manage-volumes-u2/modifyvol11.png)

5. Klikněte na tlačítko **Uložit** toosave změny. Po zobrazení výzvy k potvrzení klikněte na **Ano**. Hello portál Azure se zobrazí zprávu aktualizací svazku. Zpráva o úspěšném provedení zobrazí při hello svazku byla úspěšně aktualizována.

    ![Zkontrolujte dopad svazku přepnutím do režimu offline](./media/storsimple-8000-manage-volumes-u2/modifyvol5.png)

7. Pokud se rozšířit svazek, proveďte následující kroky v hostitelském počítači Windows hello:
   
   1. Přejděte příliš**Správa počítače** ->**Správa disků**.
   2. Klikněte pravým tlačítkem na **Správa disků** a vyberte **Prohledat disky**.
   3. V seznamu hello disků vyberte hello svazek, který jste aktualizaci, klikněte pravým tlačítkem a pak vyberte **rozšířit svazek**. Spustí se Průvodce rozšířit svazek Hello. Klikněte na **Další**.
   4. Dokončete Průvodce hello, přijetí hello výchozí hodnoty. Po dokončení Průvodce hello hello svazku by měl zobrazit hello zvětšit velikost.
      
      > [!NOTE]
      > Pokud rozbalte místně vázaný svazek a potom rozbalte jiné místně připnuli svazku okamžitě později, hello svazku rozšíření úloh spouští sekvenčně. Úloha rozšíření svazku první Hello musí ukončit předtím, než můžete začít hello další svazek rozšíření úlohy.
      

## <a name="change-hello-volume-type"></a>Změnit typ svazku hello

Typ svazku hello můžete změnit z vrstvené toolocally připnuté nebo místně vázaný tootiered. Tento převod však by neměl být časté výskyt.

### <a name="tiered-toolocal-volume-conversion-considerations"></a>Informace o převodu toolocal vrstvený svazek

Důvody pro převod svazku z vrstvené toolocally připnutý zahrnují:

* Místní záruky týkající se data dostupnosti a výkonu
* Odstranění cloudu latenci a problémy s připojením k cloudu.

Obvykle jsou malé existující svazky, které chcete tooaccess často. Když je vytvořeno místně vázaný svazek plně zřízený. 

Pokud převádíte tooa místně vázaný svazek vrstvený svazek, StorSimple ověřuje mít dostatek místa na zařízení, než začne hello převod. Pokud máte dostatek místa, dojde k chybě a hello operace, budou zrušeny. 

> [!NOTE]
> Před zahájením převodu z vrstvené toolocally připnutý, ujistěte se, zvažte hello požadavky na místo na vašich dalších zatížení. 

Převod z vrstvené tooa místně vázaný svazek může nepříznivě ovlivnit výkon zařízení. Kromě toho hello následující faktory může zvýšit hello doba trvání toocomplete hello převod:

* Není k dispozici dostatečná šířka pásma.
* Není žádná aktuální záloha.

účinky hello toominimize, které mohou mít tyto faktory:

* Zkontrolujte vaše zásady omezení šířky pásma a ujistěte se, že vyhrazené šířky pásma 40 MB/s je k dispozici.
* Naplánovat hello převod hodiny mimo špičku.
* Pořízení snímku cloudu před zahájením převodu hello.

Pokud převádíte více svazků (Podpora různých zátěží), by měl tak, aby vyšší prioritu svazky se převedou nejprve upřednostňovat převodu svazku hello. Například je třeba převést svazky, které jsou hostiteli virtuálních počítačů (VM) nebo svazky s úlohami SQL před převodem svazky s úlohami sdílené složky souborů.

### <a name="local-tootiered-volume-conversion-considerations"></a>Informace o převodu svazku místní tootiered

Budete pravděpodobně chtít toochange tooa místně vázaný svazek vrstvené svazku Pokud budete potřebovat další místo tooprovision jiných svazků. Při převodu hello místně vázaný svazek tootiered hello dostupné kapacity na hello zařízení zvyšuje úroveň hello velikost kapacity hello vydání. Pokud problémy s připojením k zabránění hello převod svazku z místního typu toohello hello vrstvené typu, hello místní svazek bude mít květy vlastnosti vrstvený svazek až po dokončení převodu hello. Je to proto, že některá data mohou přesahovat toohello cloudu. Tato data spilled pokračuje toooccupy volné místo na hello zařízení, které nelze uvolnit, dokud je restartovat a dokončit operaci hello.

> [!NOTE]
> Převod svazku může určitou dobu trvat a nelze zrušit převodu z po jeho spuštění. Při převodu hello, zůstane online Hello svazku a záloh, ale nelze rozbalit nebo obnovit svazek hello během probíhající hello převod.


#### <a name="toochange-hello-volume-type"></a>Typ svazku toochange hello

1. Přejděte služby StorSimple Manager zařízení tooyour a pak klikněte na tlačítko **zařízení**. Hello tabulkové výpis hello zařízení, vyberte hello zařízení, která obsahuje hello svazku, že máte v úmyslu toomodify. Klikněte na tlačítko **Nastavení > svazky**.

    ![Přejděte tooVolumes okno](./media/storsimple-8000-manage-volumes-u2/modifyvol2.png)

3. Hello tabulkové seznam svazků vyberte hello svazku a klikněte pravým tlačítkem na tooinvoke hello kontextové nabídky. Vyberte **upravit**.

    ![Vyberte změnit z kontextové nabídky](./media/storsimple-8000-manage-volumes-u2/changevoltype2.png)

4. Na hello **upravit svazek** okně změnit typ svazku hello výběrem hello nový typ z hello **typ** rozevíracího seznamu.
   
   * Pokud chcete změnit typ hello příliš**místně vázaný**, StorSimple, toosee zkontroluje, jestli je dostatečnou kapacitu.
   * Pokud chcete změnit typ hello příliš**nastavování** a použije tento svazek pro archivní data, vyberte hello **použít tento svazek pro archivní data s méně často používaná** zaškrtávací políčko.
   * Pokud konfigurujete místně vázaný svazek jako vrstvené nebo _naopak_, hello zobrazí se následující zpráva.
   
    ![Změna svazek zadejte text zprávy.](./media/storsimple-8000-manage-volumes-u2/changevoltype3.png)

7. Klikněte na tlačítko **Uložit** toosave hello změny. Po zobrazení výzvy k potvrzení, klikněte na tlačítko **Ano** procesu převodu toostart hello. 

    ![Uložte a potvrďte](./media/storsimple-8000-manage-volumes-u2/modifyvol11.png)

8. Hello portál Azure zobrazí oznámení pro vytvoření úlohy hello, že by aktualizace hello svazku. Klikněte na hello oznámení toomonitor hello stav úlohy převodu svazku hello.

    ![Úlohy pro převod svazku](./media/storsimple-8000-manage-volumes-u2/changevoltype5.png)

## <a name="take-a-volume-offline"></a>Svazek převést do režimu offline

Při plánování toomodify nebo odstranění hello svazek může být nutné tootake svazek offline. Pokud je svazek offline, není k dispozici pro přístup pro čtení a zápis. Je třeba provést offline hello svazek na hostiteli hello a hello zařízení.

#### <a name="tootake-a-volume-offline"></a>tootake svazek offline

1. Ujistěte se, že svazek hello dotyčném není používán před přepnutím do režimu offline.
2. Trvat hello svazku do offline režimu na hostiteli hello první. Tím se eliminuje všechny potenciální riziko poškození dat na svazku hello. Konkrétní kroky najdete v části toohello pokyny pro operační systém hostitele.
3. Po hello hostitel je v režimu offline, trvat hello svazku na hello zařízení do offline režimu provedením hello následující kroky:
   
    1. Přejděte služby StorSimple Manager zařízení tooyour a pak klikněte na tlačítko **zařízení**. Hello tabulkové výpis hello zařízení, vyberte hello zařízení, která obsahuje hello svazku, že máte v úmyslu toomodify. Klikněte na tlačítko **Nastavení > svazky**.

        ![Přejděte tooVolumes okno](./media/storsimple-8000-manage-volumes-u2/modifyvol2.png)

    2. Hello tabulkové seznam svazků vyberte hello svazku a klikněte pravým tlačítkem na tooinvoke hello kontextové nabídky. Vyberte **převést do režimu offline** svazku hello tootake upravíte v režimu offline.

        ![Vyberte a do offline režimu svazku](./media/storsimple-8000-manage-volumes-u2/modifyvol4.png)

3. V hello **převést do režimu offline** okně zkontrolovat dopad hello hello svazku přepnutím do režimu offline a zaškrtněte odpovídající políčko hello. Klikněte na tlačítko **převést do režimu offline**. 

    ![Zkontrolujte dopad svazku přepnutím do režimu offline](./media/storsimple-8000-manage-volumes-u2/modifyvol5.png)
      
      Budete informováni hello svazek je offline. Stav svazku Hello rovněž aktualizuje tooOffline.
      
4. Jakmile je svazek offline, pokud vyberete hello svazek a klikněte pravým tlačítkem, **přepnout do režimu Online** bude k dispozici v kontextové nabídce hello.

> [!NOTE]
> Hello **přepnout do režimu Offline** příkaz odešle žádost toohello zařízení tootake hello svazek offline. Pokud hostitele stále používají hello svazku, to vede k přerušení připojení, ale nebudou hello svazku přepnutím do režimu offline.

## <a name="delete-a-volume"></a>Odstranění svazku

> [!IMPORTANT]
> Svazek můžete odstranit pouze v případě, že je offline.

Proveďte následující kroky toodelete svazek hello.

#### <a name="toodelete-a-volume"></a>toodelete svazku

1. Přejděte služby StorSimple Manager zařízení tooyour a pak klikněte na tlačítko **zařízení**. Hello tabulkové výpis hello zařízení, vyberte hello zařízení, která obsahuje hello svazku, že máte v úmyslu toomodify. Klikněte na tlačítko **Nastavení > svazky**.

    ![Přejděte tooVolumes okno](./media/storsimple-8000-manage-volumes-u2/modifyvol2.png)

3. Zkontrolujte stav hello hello svazku chcete toodelete. Pokud chcete toodelete svazku hello není v režimu offline, odpojte jej nejprve. Postupujte podle kroků hello v [do offline režimu svazku](#take-a-volume-offline).
4. Po hello svazek je offline, vyberte svazek hello, klikněte pravým tlačítkem na tooinvoke hello kontextovou nabídku a pak vyberte **odstranit**.

    ![Vyberte možnost odstranit z kontextové nabídky](./media/storsimple-8000-manage-volumes-u2/deletevol1.png)

5. V hello **odstranit** okno zkontrolujte a vyberte hello políčko proti hello dopad odstraněním svazku. Pokud odstraníte svazek, dojde ke ztrátě všech hello data, která se nachází na svazku hello. 

    ![Uložte a potvrďte změny](./media/storsimple-8000-manage-volumes-u2/deletevol2.png)

6. Po odstranění svazku hello hello tabulkového seznamu svazků aktualizuje tooindicate hello odstranění.

    ![Seznam aktualizované svazků](./media/storsimple-8000-manage-volumes-u2/deletevol3.png)
   
   > [!NOTE]
   > Pokud odstraníte místně vázaný svazek, hello místa na disku pro nové svazky nemusí být okamžitě aktualizován. Hello služby StorSimple Manager zařízení pravidelně aktualizuje hello volné místo k dispozici. Doporučujeme, že Počkejte několik minut, než se pokusíte toocreate hello nový svazek.
   >
   > Kromě toho pokud odstraníte místně vázaný svazek a potom odstraňte jiné místně připnuli svazku okamžitě později, úlohy odstranění svazku hello spouští sekvenčně. úlohy odstranění svazku první Hello musí ukončit předtím, než můžete začít hello další úlohy odstranění svazku.

## <a name="monitor-a-volume"></a>Monitorování svazku

Monitorování svazku můžete toocollect I/E-související statistiky pro svazek. Monitorování je povoleno ve výchozím nastavení pro hello nejprve 32 svazky, které vytvoříte. Ve výchozím nastavení vypnutá monitorování další svazky. 

> [!NOTE]
> Monitorování klonovaný svazků je ve výchozím nastavení zakázáno.


Proveďte následující kroky tooenable hello nebo zakázat monitorování pro svazek.

#### <a name="tooenable-or-disable-volume-monitoring"></a>tooenable nebo zakažte monitorování svazku

1. Přejděte služby StorSimple Manager zařízení tooyour a pak klikněte na tlačítko **zařízení**. Hello tabulkové výpis hello zařízení, vyberte hello zařízení, která obsahuje hello svazku, že máte v úmyslu toomodify. Klikněte na tlačítko **Nastavení > svazky**.
2. Hello tabulkové seznam svazků vyberte hello svazku a klikněte pravým tlačítkem na tooinvoke hello kontextové nabídky. Vyberte **upravit**.
3. V hello **upravit svazek** okně pro **monitorování** vyberte **povolit** nebo **zakázat** tooenable nebo zakažte monitorování.

    ![Zakázání sledování](./media/storsimple-8000-manage-volumes-u2/monitorvol1.png) 

4. Klikněte na tlačítko **Uložit** a po zobrazení výzvy k potvrzení, klikněte na tlačítko **Ano**. Hello portál Azure zobrazí oznámení pro aktualizaci hello svazek a potom zpráva o úspěšném provedení po úspěšné aktualizaci hello svazku.

## <a name="next-steps"></a>Další kroky

* Zjistěte, jak příliš[klonovat svazek StorSimple](storsimple-8000-clone-volume-u2.md).
* Zjistěte, jak příliš[použití hello tooadminister service Manager zařízení StorSimple zařízení StorSimple](storsimple-8000-manager-service-administration.md).

