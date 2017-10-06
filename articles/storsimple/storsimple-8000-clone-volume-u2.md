---
title: "aaaClone svazek na řady StorSimple 8000 | Microsoft Docs"
description: "Popisuje hello klon různé typy a využití a vysvětluje, jak můžete pomocí zálohovacího skladu tooclone svazku na zařízení řady StorSimple 8000."
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
ms.workload: TBD
ms.date: 07/26/2017
ms.author: alkohli
ms.openlocfilehash: 4f7e1f62f17c7b2bd72820a00a5ab87b7e192332
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-in-azure-portal-tooclone-a-volume"></a>Použít službu StorSimple Manager zařízení hello ve službě Azure portálu tooclone svazku

## <a name="overview"></a>Přehled

Tento kurz popisuje, jak můžete pomocí zálohovacího skladu tooclone svazku prostřednictvím hello **katalog zálohování** okno. Také vysvětluje hello rozdíl mezi *přechodný* a *trvalé* provede klonování. Hello pokyny v tomto kurzu platí zařízení řady StorSimple 8000 hello tooall verzi Update 3 nebo novější.

Hello služby StorSimple Manager zařízení **katalog zálohování** zobrazuje všechny zálohovací sklady hello, které vytvářejí, když jsou provedeny ruční nebo automatické zálohy. Pak můžete vybrat svazek v tooclone zálohovacího skladu.

 ![Zálohovací sklad seznamu](./media/storsimple-8000-clone-volume-u2/bucatalog.png)

## <a name="considerations-for-cloning-a-volume"></a>Důležité informace týkající se klonování svazku

Vezměte v úvahu následující informace při klonování svazku hello.

- Klon se chová v hello stejný způsob jako regulární svazek. Všechny operace, které je možné na svazku je k dispozici pro klonování hello.

- Monitorování a výchozí zálohování jsou automaticky zakázán na klonovaný svazku. Potřebovali byste tooconfigure svazek klonovaný pro žádné zálohy.

- Místně vázaný svazek je klonovat jako vrstvený svazek. Pokud třeba hello místně vázaný toobe klonovaný svazku, můžete převést hello klon tooa místně vázaný svazek po úspěšném dokončení operace klonování hello. Pro informace o převodu tooa vrstvený svazek, který je místně připnutý svazku, přejděte příliš[změnit typ svazku hello](storsimple-8000-manage-volumes-u2.md#change-the-volume-type).

- Pokud se pokusíte tooconvert, který svazek klonovaný ze vrstvené toolocally připnutý okamžitě po klonování (Pokud je stále přechodný klon), převod hello selže s touto hello následující chybová zpráva:

    `Unable toomodify hello usage type for volume {0}. This can happen if hello volume being modified is a transient clone and hasn’t been made permanent. Take a cloud snapshot of this volume and then retry hello modify operation.`

    Tato chyba byl přijat pouze v případě, že se klonování na tooa jiné zařízení. Můžete úspěšně převést toolocally svazku hello připnutý Pokud převedete hello přechodný klon tooa trvalé klonování. Pořízení snímku cloudu hello přechodný klon tooconvert ho tooa trvalé klonování.

## <a name="create-a-clone-of-a-volume"></a>Vytvořit klon svazku

Můžete vytvořit klon na hello stejného zařízení, jiná zařízení nebo i o zařízení cloudu pomocí místní nebo cloudové snímku.

Hello následující postup popisuje, jak zálohovat toocreate klonu z hello katalogu.  Alternativní metoda tooinitiate klonování je příliš toogo**svazky**, vyberte svazek, pak klikněte pravým tlačítkem na tooinvoke hello kontextovou nabídku a vyberte **klon**.

Proveďte následující kroky toocreate klon svazku z katalogu zálohování hello hello.

#### <a name="tooclone-a-volume"></a>tooclone svazku

1. Přejděte služby StorSimple Manager zařízení tooyour a pak klikněte na tlačítko **katalog zálohování**.

2. Vyberte zálohovací sklad následujícím způsobem:
   
   1. Vyberte příslušné zařízení hello.
   2. V rozevíracím seznamu hello výběru hello svazek nebo zálohování zásady zálohování hello chcete tooselect.
   3. Zadejte časové rozmezí hello.
   4. Klikněte na tlačítko **použít** tooexecute tento dotaz.

    Hello zálohování přidružená k zásadě hello vybraný svazek nebo zálohování by se zobrazit v seznamu hello zálohovací sklady.
   
    ![Zálohovací sklad seznamu](./media/storsimple-8000-clone-volume-u2/bucatalog.png)
     
3. Rozbalte hello zálohovacího skladu tooview hello přidružené svazky. Tyto svazky musí být převedeno do režimu offline v hostiteli hello a zařízení, než bude možné obnovit. Získat přístup ke svazkům hello na hello **svazky** okno zařízení a pak hello postupujte podle kroků v [do offline režimu svazek](storsimple-8000-manage-volumes-u2.md#take-a-volume-offline) tootake je v offline režimu.
   
   > [!IMPORTANT]
   > Ujistěte se, že jste provedli hello svazky do offline režimu na hostiteli hello nejprve, před provedením hello svazky do režimu offline v zařízení hello. Pokud neprovedete offline hello svazky na hostiteli hello, může potenciálně vést k poškození toodata.
   
4. Přejděte zpět toohello **katalog zálohování** a vyberte svazek v zálohovacím skladu. Klikněte pravým tlačítkem a pak z hello kontextové nabídky, vyberte **klon**.

   ![Zálohovací sklad seznamu](./media/storsimple-8000-clone-volume-u2/clonevol3b.png) 

3. V hello **klon** okně hello následující kroky:
   
    1. Určete cílové zařízení. Toto je hello umístění, kde bude vytvořen hello klonování. Můžete vybrat hello stejné zařízení nebo zadejte jiné zařízení.

      > [!NOTE]
      > Ujistěte se, že požadovaná pro klon hello kapacita hello je nižší než hello kapacity, které jsou k dispozici na cílovém zařízení hello.
       
    2. Zadejte název jedinečné svazku pro vaše klon. Hello název musí obsahovat 3 až 127 znaků.
      
        > [!NOTE]
        > Hello **klonování svazku jako** pole bude **nastavování** i v případě, že se klonování místně vázaný svazek. Nelze změnit toto nastavení; ale pokud klonovaný svazku toobe místně vázaný také potřebovat hello, můžete převést hello klon tooa místně vázaný svazek po úspěšně vytvořit klon hello. Pro informace o převodu tooa vrstvený svazek, který je místně připnutý svazku, přejděte příliš[změnit typ svazku hello](storsimple-8000-manage-volumes-u2.md#change-the-volume-type).
          
    3. V části **připojení hostitele**, zadejte záznam řízení přístupu (ACR) pro klon hello. Můžete přidat nové ACR nebo vyberte možnost ze seznamu existující hello. Hello ACR určí, které hostitele může přístup Tenhle klon.
      
        ![Zálohovací sklad seznamu](./media/storsimple-8000-clone-volume-u2/clonevol3a.png) 

    4. Klikněte na tlačítko **klon** toocomplete hello operaci.

4. Úloha klonování se zahájí a zobrazí se upozornění při klonování hello je úspěšně vytvořen. Klikněte na úlohu oznámení hello nebo přejděte příliš**úlohy** okno toomonitor hello klon úlohy.

    ![Zálohovací sklad seznamu](./media/storsimple-8000-clone-volume-u2/clonevol5.png)

7. Po dokončení úlohy klon hello přejděte tooyour zařízení a pak klikněte na tlačítko **svazky**. V seznamu hello svazků, měli byste vidět hello klon, který byl právě vytvořen v hello stejné kontejneru svazků, který má hello zdrojový svazek.

    ![Zálohovací sklad seznamu](./media/storsimple-8000-clone-volume-u2/clonevol6.png)

Klon, který je vytvořen tímto způsobem je přechodný klonu. Další informace o typech klonování najdete v tématu [přechodný oproti trvalé klony](#transient-vs-permanent-clones).


## <a name="transient-vs-permanent-clones"></a>Pouze dočasné a trvalé klony
Přechodný klony vytvoří jenom v případě, že klonování tooanother zařízení. Může klonovat konkrétní svazku z jiného zařízení zálohovacího skladu tooa spravuje hello Správce zařízení StorSimple. klon přechodný Hello obsahuje odkazy na toohello data v původním svazku hello a používá tento tooread dat a zápisu místně na cílovém zařízení hello.

Po provedení cloudový snímek přechodný klonování je výsledný klon hello *trvalé* klonování. Během tohoto procesu kopii dat hello se vytvoří v cloudu hello a hello toocopy času, které tato data je určena velikostí hello hello dat a hello Azure latenci (Toto je kopie Azure do Azure). Tento proces může trvat tooweeks dnů. klon přechodný Hello stane trvalé klonování a nemá žádné odkazy toohello původní svazek data, která byla naklonována ze.

## <a name="scenarios-for-transient-and-permanent-clones"></a>Scénáře pro přechodný a trvalé klony
Hello následující oddíly popisují příklad situacích, ve kterých může být použit klony přechodný a trvalé.

### <a name="item-level-recovery-with-a-transient-clone"></a>Obnovení na úrovni položek s přechodný klon
Je nutné toorecover jeden rok starý soubor s Microsoft PowerPoint. Váš správce IT identifikuje hello konkrétní zálohování z této doby a pak filtry hello svazku. Správce Hello pak provede klonování svazku hello, vyhledá hello soubor, který hledáte a poskytuje ji tooyou. V tomto scénáři se používá přechodný klonu.

### <a name="testing-in-hello-production-environment-with-a-permanent-clone"></a>Testování hello produkčního prostředí s trvalé klon
Je nutné tooverify chyby testování v produkčním prostředí hello. Vytvořit klon svazku hello hello produkčního prostředí a pak proveďte cloudový snímek tento toocreate klonování svazku nezávislé klonovaný. V tomto scénáři se používá trvalá klonu.

## <a name="next-steps"></a>Další kroky
* Zjistěte, jak příliš[obnovit svazek StorSimple ze zálohovacího skladu](storsimple-8000-restore-from-backup-set-u2.md).
* Zjistěte, jak příliš[použití hello tooadminister service Manager zařízení StorSimple zařízení StorSimple](storsimple-8000-manager-service-administration.md).

