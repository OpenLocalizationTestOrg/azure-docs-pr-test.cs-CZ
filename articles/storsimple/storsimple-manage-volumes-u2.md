---
title: "aaaManage formátujte svazky zařízení StorSimple (U2) | Microsoft Docs"
description: "Vysvětluje, jak tooadd, upravit, monitorování a odstranit svazky zařízení StorSimple a tootake je v režimu offline v případě potřeby."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 57896932-0aa5-4805-970c-d13403ae7551
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/28/2016
ms.author: alkohli
ms.openlocfilehash: 8b64f1d92023646cdf881882854d9bc5d7334a10
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-toomanage-volumes-update-2"></a>Použít hello StorSimple Manager service toomanage svazky (Update 2)
[!INCLUDE [storsimple-version-selector-manage-volumes](../../includes/storsimple-version-selector-manage-volumes.md)]

## <a name="overview"></a>Přehled
Tento kurz vysvětluje, jak toouse hello toocreate služby StorSimple Manager a spravovat svazky na zařízení StorSimple hello a virtuální zařízení StorSimple Update 2 nainstalován.

Hello služby StorSimple Manager je rozšíření hello portál Azure classic, která umožňuje spravovat z jedné webové rozhraní vašeho řešení StorSimple. Ve svazcích toomanaging přidání můžete použít toocreate služby StorSimple Manager hello a spravovat služby StorSimple, zobrazit a spravovat zařízení, zobrazovat výstrahy a zobrazit a spravovat zásady zálohování a zálohování katalog hello.

## <a name="volume-types"></a>Typy svazku
Svazky zařízení StorSimple může být:

* **Místně vázaný svazky**: Data v těchto svazcích zůstane na místním zařízení StorSimple hello za všech okolností.
* **Zřízeny vrstvené svazky**: Data v těchto svazků můžete distribuována toohello cloudu.

Archivace svazek je typ vrstvený svazek. Hello větší odstranění duplicit velikost bloku použít pro archivaci svazky umožňuje hello zařízení tootransfer větší segmentů toohello dat v cloudu. 

V případě potřeby můžete změnit typ svazku hello z místní tootiered nebo vrstvené toolocal. Další informace, přejděte příliš[změnit typ svazku hello](#change-the-volume-type).

### <a name="locally-pinned-volumes"></a>Místně vázaných svazků
Místně vázaných svazků jsou zcela zřizované svazky, které není vrstvy toohello dat v cloudu, a zajistí tak místní záruky pro primární data, nezávisle na cloudu připojení. Není data na místně vázaných svazků s odstraněním duplicitních dat a komprimované; Nicméně jsou odstranění duplicit snímky místně vázaných svazků. 

Místně vázaných svazků jsou plně zřízený; proto musí mít dostatek místa na vašem zařízení, při jejich vytváření. Můžete zřídit místně vázaných svazků až tooa maximální velikosti 8 TB na zařízení hello StorSimple 8100 a 20 TB na zařízení 8600 hello. StorSimple si vyhrazuje zbývající volné místo na hello zařízení pro snímky, metadat a zpracování dat hello. Můžete zvýšit velikost hello místně vázaný svazek toohello maximální prostoru k dispozici, ale nelze snížit velikost svazku po vytvoření hello.

Když vytvoříte místně vázaný svazek, se snižuje hello volné místo pro vytvoření vrstvené svazky. Hello zpětné platí také: Pokud máte vrstvené svazky, bude místo hello dostupné pro vytváření místně připojené svazky menší než maximální limit hello uvedené výše. Další informace o místní svazky, najdete v části toohello [nejčastější dotazy na místně vázaných svazků](storsimple-local-volume-faq.md).   

### <a name="tiered-volumes"></a>Vrstvené svazky
Vrstvené svazky jsou dynamicky zřizované svazky, ve které hello často používaná data zůstává v zařízení hello místní a méně často používaná data se automaticky vrstvené toohello cloudu. Dynamické zajišťování je technologie virtualizace, ve kterém úložiště k dispozici, zobrazí se tooexceed fyzické prostředky. Místo předem rezervování dostatečné úložiště, StorSimple používá dynamického zřizování tooallocate akorát toomeet aktuální požadavky na volné místo. Hello elastické povaha cloudového úložiště usnadňuje tento přístup, protože StorSimple můžete zvýšit nebo snížit cloudové úložiště toomeet splněné měnící se požadavky.

Pokud používáte vrstvený svazek pro archivní data, výběr hello hello **použít tento svazek pro archivní data s méně často používaná** políčko změní velikost bloku odstranění duplicit hello svazku too512 KB. Pokud tuto možnost nevyberete, hello příslušný vrstvený svazek bude používat velikost bloku dat 64 KB. Větší velikost bloku odstranění duplicitních dat umožňuje hello zařízení tooexpedite hello přenos velkých objemů archivních dat toohello cloudu.

> [!NOTE]
> Archivace svazků vytvořených před aktualizací 2 verzí StorSimple bude importovat, protože vrstvené hello archivace zaškrtnutým políčkem.
> 
> 

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

## <a name="hello-volumes-page"></a>Svazky stránku Hello
Hello **svazky** stránka vám umožní toomanage hello úložné svazky, které jsou zřízené v hello Microsoft Azure StorSimple zařízení pro vaše iniciátorů (serverů). Zobrazuje hello seznam svazků zařízení StorSimple.

 ![Stránka svazky](./media/storsimple-manage-volumes-u2/VolumePage.png)

Svazek se skládá z řady atributy:

* **Název svazku** – popisný název, který musí být jedinečný a pomáhá identifikovat hello svazku. Tento název se také používá v monitorování sestavy při filtrování na konkrétním svazku.
* **Stav** – může být online nebo offline. Pokud je svazek offline, není viditelné tooinitiators (servery) povolený přístup toouse hello svazku.
* **Kapacita** – určuje hello celkové množství dat, které můžou být uložené ve iniciátor hello (server). Místně vázaný svazky jsou plně zřízený a jsou umístěny na zařízení StorSimple hello. Vrstvené svazky jsou tence zřízený a hello data se odstraňují. S dynamicky zřizované svazky nebude zařízení předem přidělit kapacity fyzického úložiště, interně, nebo v cloudu hello podle tooconfigured kapacita svazku. kapacita svazku Hello je přidělené a využívat na vyžádání.
* **Typ** – Určuje, zda je svazek hello **nastavování** (hello výchozí) nebo **místně vázaný**.
* **Zálohování** – označuje, zda existuje výchozí zásady zálohování pro svazek hello.
* **Přístup k** – určuje hello iniciátorů (serverů), které jsou povoleny přístup toothis svazku. Iniciátory, které nejsou členy záznam řízení přístupu (ACR), který je přidružený svazek hello neuvidí hello svazku.
* **Monitorování** – Určuje, jestli je monitorována svazku. Svazek bude mít povoleno ve výchozím nastavení při vytvoření monitorování. Monitorování bude, ale, bude zakázán pro klonování svazku. tooenable monitorování pro svazek, postupujte podle pokynů hello v [monitorování svazku](#monitor-a-volume). 

Používejte hello pokyny v této kurz tooperform hello následující úlohy:

* Přidání svazku 
* Upravit svazku 
* Změnit typ svazku hello
* Odstranění svazku 
* Svazek převést do režimu offline 
* Monitorování svazku 

## <a name="add-a-volume"></a>Přidání svazku
Můžete [vytvořili svazek](storsimple-deployment-walkthrough-u2.md#step-6-create-a-volume) během nasazování vašeho řešení StorSimple. Přidání svazku je podobným způsobem.

#### <a name="tooadd-a-volume"></a>tooadd svazku
1. Na hello **zařízení** vyberte hello zařízení, na ni dvakrát kliknete a pak klikněte na tlačítko hello **kontejnery svazků** kartě.
2. Vyberte kontejner svazků hello seznamu a klikněte dvakrát na jeho tooaccess hello přidružené hello kontejner svazků.
3. Klikněte na tlačítko **přidat** na hello dolní části stránky hello. Spustí Hello Průvodci přidáním svazku.
   
     ![Průvodce přidáním svazku základní nastavení](./media/storsimple-manage-volumes-u2/TieredVolEx.png)
4. V hello Průvodce přidáním svazku, v části **základní nastavení**, hello následující:
   
   1. Zadejte **Název** svazku.
   2. Vyberte **typ použití** z rozevíracího seznamu hello. Pro úlohy, které vyžadují toobe dat dostupný místně na zařízení hello za všech okolností, vyberte **místně vázaný**. Pro všechny ostatní typy dat, vyberte **nastavování**. (**Nastavování** je výchozí hello.)
   3. Pokud jste vybrali **nastavování** v kroku 2, můžete vybrat hello **použít tento svazek pro archivní data s méně často používaná** tooconfigure zaškrtávací políčko archivaci svazku.
   4. Zadejte hello **zřízená kapacita** svazku v GB nebo TB. V tématu [zřídit kapacitu](#provisioned-capacity) pro maximální velikost pro každý typ zařízení a svazku. Podívejte se na hello **dostupné kapacity** toodetermine jak velké úložiště je ve skutečnosti k dispozici ve vašem zařízení.
5. Klikněte na ikonu šipky hello![Ikona šipky](./media/storsimple-manage-volumes-u2/HCS_ArrowIcon.png). Pokud konfigurujete místně vázaný svazek, zobrazí se následující zpráva hello.
   
    ![Změnit typ zprávu svazku](./media/storsimple-manage-volumes-u2/LocalVolEx.png)
6. Klikněte na ikonu šipky hello ![ikonu šipky](./media/storsimple-manage-volumes-u2/HCS_ArrowIcon.png)znovu toogo toohello **další nastavení** stránky.
   
    ![Přidat další nastavení Průvodce svazku](./media/storsimple-manage-volumes-u2/AddVolume2.png)<br>
7. V části **další nastavení**, přidejte nový záznam řízení přístupu (ACR):
   
   1. Hello rozevíracím seznamu vyberte záznam řízení přístupu (ACR). Alternativně můžete přidat nové ACR. ACRs určit, které hostitele můžou pro přístup svých svazků odpovídající hello IQN hostitele s uvedena v záznamu hello. Pokud nezadáte ACR, zobrazí se následující zprávou hello.
      
        ![Zadejte ACR](./media/storsimple-manage-volumes-u2/SpecifyACR.png)
   2. Doporučujeme vám, že jste vybrali hello **povolit výchozí zálohování tohoto svazku** zaškrtávací políčko.
   3. Klikněte na ikonu zaškrtnutí hello ![Ikona zaškrtnutí](./media/storsimple-manage-volumes-u2/HCS_CheckIcon.png) toocreate hello svazek s hello zadat nastavení.

Nový svazek je nyní připraven toouse.

> [!NOTE]
> Pokud vytvoříte místně vázaný svazek a pak vytvořit jiné místně připnuli svazku okamžitě později, úlohy vytvoření svazku hello spouští sekvenčně. Úloha vytvoření svazku první Hello musí ukončit předtím, než můžete začít hello další úloha vytvoření svazku.
> 
> 

## <a name="modify-a-volume"></a>Upravit svazku
Upravte svazek, když potřebujete tooexpand ho nebo změňte hello hostitele, kteří přístup hello svazku.

> [!IMPORTANT]
> * Pokud změníte velikost svazku hello na hello zařízení, musí velikost svazku hello toobe změnit také v hostiteli se hello. 
> * Hello hostitelské kroků popsaných v tomto poli se pro Windows Server 2012 (2012 R2). Postupy pro Linux nebo jiných hostitelských operačních systémech se liší. Tooyour hostitele operačního systému pokyny najdete v případě úpravy hello svazek na hostitele s jiným operačním systémem. 
> 
> 

#### <a name="toomodify-a-volume"></a>toomodify svazku
1. Na hello **zařízení** vyberte hello zařízení, na ni dvakrát kliknete a pak klikněte na tlačítko hello **kontejnery svazků** kartě.
2. Vyberte kontejner svazků hello seznamu a klikněte dvakrát na jeho svazky hello tooview přidružené hello kontejneru.
3. Vyberte svazek a v dolní části hello hello stránky, klikněte na tlačítko **upravit**. Spustí se Průvodce svazku upravit Hello.
4. V hello upravte průvodce svazku, v části **základní nastavení**, můžete provést následující hello:
   
   * Upravit hello **název**.
   * Převést hello **typ použití** z místně vázaný tootiered nebo vrstvené toolocally připnutý (najdete v části [změnit typ svazku hello](#change-the-volume-type) informace).
   * Zvýšit hello **zřízená kapacita**. Hello **zřízená kapacita** může být pouze zvýšena. Po vytvoření, nelze zmenšit svazek.
5. V části **další nastavení**, můžete upravit hello ACR, za předpokladu, že svazek hello je offline. Pokud je svazek hello online, budete potřebovat tootake ho offline první. Najdete kroky toohello v [do offline režimu svazek](#take-a-volume-offline) předchozí toomodifying hello ACR.
   
   > [!NOTE]
   > Nelze změnit hello **povolit výchozí zálohování** možnost pro svazek hello.
   > 
   > 
6. Uložte změny kliknutím na ikonu zaškrtnutí hello ![ikona zaškrtnutí](./media/storsimple-manage-volumes-u2/HCS_CheckIcon.png). Hello portál Azure classic zobrazí zprávu aktualizací svazku. Zpráva o úspěšném provedení zobrazí při hello svazku byla úspěšně aktualizována.
7. Pokud se rozšířit svazek, proveďte následující kroky v hostitelském počítači Windows hello:
   
   1. Přejděte příliš**Správa počítače** ->**Správa disků**.
   2. Klikněte pravým tlačítkem na **Správa disků** a vyberte **Prohledat disky**.
   3. V seznamu hello disků vyberte hello svazek, který jste aktualizaci, klikněte pravým tlačítkem a pak vyberte **rozšířit svazek**. Spustí se Průvodce rozšířit svazek Hello. Klikněte na **Další**.
   4. Dokončete Průvodce hello, přijetí hello výchozí hodnoty. Po dokončení Průvodce hello hello svazku by měl zobrazit hello zvětšit velikost.
      
      > [!NOTE]
      > Pokud rozbalte místně vázaný svazek a potom rozbalte jiné místně připnuli svazku okamžitě později, hello svazku rozšíření úloh spouští sekvenčně. Úloha rozšíření svazku první Hello musí ukončit předtím, než můžete začít hello další svazek rozšíření úlohy.
      > 
      > 

![Dostupné video](./media/storsimple-manage-volumes-u2/Video_icon.png)**Dostupné video**

toowatch video, které ukazuje, jak tooexpand svazek, klikněte na tlačítko [zde](https://azure.microsoft.com/documentation/videos/expand-a-storsimple-volume/).

## <a name="change-hello-volume-type"></a>Změnit typ svazku hello
Typ svazku hello můžete změnit z vrstvené toolocally připnuté nebo místně vázaný tootiered. Tento převod však by neměl být časté výskyt. Důvody pro převod svazku z vrstvené toolocally připnutý zahrnují:

* Místní záruky týkající se data dostupnosti a výkonu
* Odstranění cloudu latenci a problémy s připojením k cloudu.

Obvykle jsou malé existující svazky, které chcete tooaccess často. Když je vytvořeno místně vázaný svazek plně zřízený. Pokud převádíte tooa místně vázaný svazek vrstvený svazek, StorSimple ověřuje mít dostatek místa na zařízení, než začne hello převod. Pokud máte dostatek místa, dojde k chybě a hello operace, budou zrušeny. 

> [!NOTE]
> Před zahájením převodu z vrstvené toolocally připnutý, ujistěte se, zvažte hello požadavky na místo na vašich dalších zatížení. 
> 
> 

Budete možná chtít toochange tooa místně vázaný svazek vrstvené svazku Pokud budete potřebovat další místo tooprovision jiných svazků. Při převodu hello místně vázaný svazek tootiered hello dostupné kapacity na hello zařízení zvyšuje úroveň hello velikost kapacity hello vydání. Pokud problémy s připojením k zabránění hello převod svazku z místního typu toohello hello vrstvené typu, hello místní svazek bude mít květy vlastnosti vrstvený svazek až do dokončení převodu hello. Je to proto, že některá data mohou přesahovat toohello cloudu. Tato data spilled bude toooccupy volné místo na hello zařízení, které nelze uvolnit, dokud je restartovat a dokončit operaci hello.

> [!NOTE]
> Převod svazku může určitou dobu trvat a nelze zrušit převodu z po jeho spuštění. Při převodu hello, zůstane online Hello svazku a záloh, ale nelze rozbalit nebo obnovit svazek hello během probíhající hello převod.  
> 
> 

Převod z vrstvené tooa místně vázaný svazek může nepříznivě ovlivnit výkon zařízení. Kromě toho hello následující faktory může zvýšit hello doba trvání toocomplete hello převod:

* Není k dispozici dostatečná šířka pásma.
* Není žádná aktuální záloha.

účinky hello toominimize, které mohou mít tyto faktory:

* Zkontrolujte vaše zásady omezení šířky pásma a ujistěte se, že vyhrazené šířky pásma 40 MB/s je k dispozici.
* Naplánovat hello převod hodiny mimo špičku.
* Pořízení snímku cloudu před zahájením převodu hello.

Pokud převádíte více svazků (Podpora různých zátěží), by měl tak, aby vyšší prioritu svazky se převedou nejprve upřednostňovat převodu svazku hello. Například je třeba převést svazky, které jsou hostiteli virtuálních počítačů (VM) nebo svazky s úlohami SQL před převodem svazky s úlohami sdílené složky souborů.

#### <a name="toochange-hello-volume-type"></a>Typ svazku toochange hello
1. Na hello **zařízení** vyberte hello zařízení, na ni dvakrát kliknete a pak klikněte na tlačítko hello **kontejnery svazků** kartě.
2. Vyberte kontejner svazků hello seznamu a klikněte dvakrát na jeho svazky hello tooview přidružené hello kontejneru.
3. Vyberte svazek a v dolní části hello hello stránky, klikněte na tlačítko **upravit**. Spustí se Průvodce svazku upravit Hello.
4. Na hello **základní nastavení** změňte typ použití hello výběrem hello nový typ z hello **typ použití** rozevíracího seznamu.
   
   * Pokud chcete změnit typ hello příliš**místně vázaný**, StorSimple, toosee zkontroluje, jestli je dostatečnou kapacitu.
   * Pokud chcete změnit typ hello příliš**nastavování** a použije tento svazek pro archivní data, vyberte hello **použít tento svazek pro archivní data s méně často používaná** zaškrtávací políčko.
     
       ![Zaškrtávací políčko archivu](./media/storsimple-manage-volumes-u2/ModifyTieredVolEx.png)
5. Klikněte na ikonu šipky hello ![ikonu šipky](./media/storsimple-manage-volumes-u2/HCS_ArrowIcon.png) toogo toohello **další nastavení** stránky. Pokud konfigurujete místně vázaný svazek, hello zobrazí se následující zpráva.
   
    ![Změnit typ zprávu svazku](./media/storsimple-manage-volumes-u2/ModifyLocalVolEx.png)
6. Klikněte na ikonu šipky hello ![ikona šipky](./media/storsimple-manage-volumes-u2/HCS_ArrowIcon.png) znovu toocontinue.
7. Klikněte na ikonu zaškrtnutí hello ![Ikona zaškrtnutí](./media/storsimple-manage-volumes-u2/HCS_CheckIcon.png) proces převodu toostart hello. Hello portál Azure se zobrazí zprávu aktualizací svazku. Zpráva o úspěšném provedení zobrazí při hello svazku byla úspěšně aktualizována.

## <a name="take-a-volume-offline"></a>Svazek převést do režimu offline
Můžete potřebovat tootake svazek offline při plánování toomodify ho nebo odstraňte ji. Pokud je svazek offline, není k dispozici pro přístup pro čtení a zápis. Budete potřebovat tootake hello svazku do offline režimu na hostiteli hello i hello zařízení. 

#### <a name="tootake-a-volume-offline"></a>tootake svazek offline
1. Ujistěte se, že svazek hello dotyčném není používán před přepnutím do režimu offline.
2. Trvat hello svazku do offline režimu na hostiteli hello první. Tím se eliminuje všechny potenciální riziko poškození dat na svazku hello. Konkrétní kroky najdete v části toohello pokyny pro operační systém hostitele.
3. Po hello hostitel je v režimu offline, trvat hello svazku na hello zařízení do offline režimu provedením hello následující kroky:
   
   1. Na hello **zařízení** vyberte hello zařízení, na ni dvakrát kliknete a pak klikněte na tlačítko hello **kontejnery svazků** kartě hello **kontejnery svazků** kartě seznamy v tabulkovém formátu všechny Hello kontejnery svazků, které jsou přidruženy hello zařízení.
   2. Vyberte kontejner svazků a klikněte na něj toodisplay hello seznam všech svazků hello hello kontejneru.
   3. Vyberte svazek a klikněte na tlačítko **převést do režimu offline**.
   4. Po zobrazení výzvy k potvrzení klikněte na **Ano**. Hello svazku by teď měly být offline.
      
      Jakmile je svazek offline, hello **přepnout do režimu Online** bude k dispozici.

> [!NOTE]
> Hello **přepnout do režimu Offline** příkaz odešle žádost toohello zařízení tootake hello svazek offline. Pokud hostitele stále používají hello svazku, to vede k přerušení připojení, ale nebudou hello svazku přepnutím do režimu offline. 
> 
> 

## <a name="delete-a-volume"></a>Odstranění svazku
> [!IMPORTANT]
> Svazek můžete odstranit pouze v případě, že je offline.
> 
> 

Proveďte následující kroky toodelete svazek hello.

#### <a name="toodelete-a-volume"></a>toodelete svazku
1. Na hello **zařízení** vyberte hello zařízení, na ni dvakrát kliknete a pak klikněte na tlačítko hello **kontejnery svazků** kartě.
2. Vyberte kontejner hello svazek, který má svazek hello chcete toodelete. Klikněte na tlačítko hello svazku kontejneru tooaccess hello **svazky** stránky.
3. Všechny svazky hello přidružený tento kontejner se zobrazí v tabulkovém formátu. Zkontrolujte stav hello hello svazku chcete toodelete. Pokud chcete toodelete svazku hello není v režimu offline, proveďte ho offline hello první, následující kroky [do offline režimu svazku](#take-a-volume-offline).
4. Po hello svazek je offline, klikněte na tlačítko **odstranit** v hello dolní části stránky hello.
5. Po zobrazení výzvy k potvrzení klikněte na **Ano**. Hello svazek bude odstraněno a hello **svazky** stránky se zobrazí hello aktualizovat seznam svazků hello kontejneru.
   
   > [!NOTE]
   > Pokud odstraníte místně vázaný svazek, hello místa na disku pro nové svazky nemusí být okamžitě aktualizován. Hello služby StorSimple Manager pravidelně aktualizuje hello volné místo k dispozici. Doporučujeme, že Počkejte několik minut, než se pokusíte toocreate hello nový svazek.<br> Kromě toho pokud odstraníte místně vázaný svazek a potom odstraňte jiné místně připnuli svazku okamžitě později, úlohy odstranění svazku hello spouští sekvenčně. úlohy odstranění svazku první Hello musí ukončit předtím, než můžete začít hello další úlohy odstranění svazku.
   > 
   > 

## <a name="monitor-a-volume"></a>Monitorování svazku
Monitorování svazku můžete toocollect I/E-související statistiky pro svazek. Monitorování je povoleno ve výchozím nastavení pro hello nejprve 32 svazky, které vytvoříte. Ve výchozím nastavení vypnutá monitorování další svazky. Monitorování klonovaný svazků také ve výchozím nastavení vypnutá.

Proveďte následující kroky tooenable hello nebo zakázat monitorování pro svazek.

#### <a name="tooenable-or-disable-volume-monitoring"></a>tooenable nebo zakažte monitorování svazku
1. Na hello **zařízení** vyberte hello zařízení, na ni dvakrát kliknete a pak klikněte na tlačítko hello **kontejnery svazků** kartě.
2. Vyberte kontejner svazků hello, ve které hello svazek nachází a potom klikněte na hello svazku kontejneru tooaccess hello **svazky** stránky.
3. Všechny svazky hello spojené s tímto kontejnerem jsou uvedeny v tabulkovém zobrazení hello. Klikněte a vyberte hello svazek nebo klonování svazku.
4. V dolní části hello hello stránky, klikněte na tlačítko **upravit**.
5. V Průvodci upravit svazek hello pod **základní nastavení**, vyberte **povolit** nebo **zakázat** z hello **monitorování** rozevíracího seznamu.

## <a name="next-steps"></a>Další kroky
* Zjistěte, jak příliš[klonovat svazek StorSimple](storsimple-clone-volume.md).
* Zjistěte, jak příliš[použití hello tooadminister služby StorSimple Manager zařízení StorSimple](storsimple-manager-service-administration.md).

