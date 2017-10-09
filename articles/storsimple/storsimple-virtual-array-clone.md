---
title: "aaaClone zálohu pole virtuální zařízení StorSimple | Microsoft Docs"
description: "Zjistěte, jak tooclone zálohování a obnovení souboru z pole virtuální zařízení StorSimple."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: af6e979c-55e3-477c-b53e-a76a697f80c9
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/21/2016
ms.author: alkohli
ms.openlocfilehash: 21bfcae48ee07762179cf00ce842b6094abe18ee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="clone-from-a-backup-of-your-storsimple-virtual-array"></a>Klonování ze záloh pole virtuální zařízení StorSimple

## <a name="overview"></a>Přehled

Tento článek popisuje podrobný jak tooclone zálohu nastavení sdílených složek nebo svazků na vaše virtuální pole Microsoft Azure StorSimple. Klonovaný zálohování Hello je použité toorecover soubor odstranil nebo ztraceny. Hello článek obsahuje také tooperform podrobné kroky, které obnovení na úrovni položek na pole virtuální zařízení StorSimple nakonfigurovali jako souborový server.

## <a name="clone-shares-from-a-backup-set"></a>Klonování sdílené složky ze zálohovacího skladu

**Než se pokusíte tooclone sdílených složek, zajistěte, abyste měli dostatek místa na hello zařízení toocomplete tuto operaci.** tooclone ze zálohy se v hello [portál Azure](https://portal.azure.com/), proveďte následující kroky hello.

#### <a name="tooclone-a-share"></a>tooclone sdílené složky

1. Procházet příliš**zařízení** okno. Vyberte a klikněte na zařízení a pak klikněte na tlačítko **sdílené složky**. Vyberte sdílenou složku hello, které chcete tooclone, klikněte pravým tlačítkem na hello sdílenou složku tooinvoke hello kontextové nabídky. Vyberte **klon**.
   
   ![Vytvořit kopii zálohy](./media/storsimple-virtual-array-clone/cloneshare1.png)
2. V hello **klon** okně klikněte na tlačítko **zálohování > vyberte** a pak hello následující: 
   
   a.    Filtrujte zálohování v tomto zařízení pro hello časový rozsah. Můžete si vybrat z **za posledních 7 dní**, **za posledních 30 dnů**, a **za poslední rok**.
   
   b.    V seznamu hello filtrované záloh zobrazí vyberte zálohování tooclone z.
   
   c.    Klikněte na **OK**.
   
   ![Vytvořit kopii zálohy](./media/storsimple-virtual-array-clone/cloneshare3.png)
3. V hello **klon** okně klikněte na tlačítko **cíle nastavení** a pak hello následující:
   
   a.    Zadejte název sdílené složky. Název sdílené složky Hello musí obsahovat 3 127 znaků.
   
   b.    Volitelně zadejte popis pro sdílenou složku klonovaný hello.
   
   c.    Nelze změnit typ hello sdílené složky hello, které obnovujete. Sdílenou složku vrstvené naklonována jako vrstvené a sdílenou složku místně vázaný jako místně vázaný.
   
   d.    kapacita Hello je nastaven jako velikost rovna toohello hello sdílené složky, které jsou klonování z.
   
   e.    Přiřaďte hello správci pro tuto sdílenou složku. Po dokončení klonování hello budete moct toomodify hello vlastností sdílené složky pomocí Průzkumníka souborů.
   
   f.    Klikněte na **OK**.
   
   ![Vytvořit kopii zálohy](./media/storsimple-virtual-array-clone/cloneshare6.png)

4. Klikněte na tlačítko **klon** toostart úlohu klonování. Po dokončení úlohy hello spustí operaci klonování hello a zobrazí se upozornění. průběh hello toomonitor klon, přejděte toohello **úlohy** a klikněte na hello úlohy tooview podrobnosti úlohy.
5. Po úspěšném vytvoření klonu hello přejděte zpět toohello **sdílené složky** okno na vašem zařízení.
6. Teď si můžete zobrazit hello novou klonovaný sdílenou složku v hello seznamu sdílených složek ve vašem zařízení. Sdílenou složku vrstvené naklonována jako vrstvené a místně vázaný sdílenou složku jako sdílenou složku místně vázaný.
   
   ![Vytvořit kopii zálohy](./media/storsimple-virtual-array-clone/cloneshare10.png)

## <a name="clone-volumes-from-a-backup-set"></a>Klonování svazků ze zálohovacího skladu

tooclone ze zálohy se v hello portál Azure, máte tooperform kroky toohello podobné těm, které jsou při klonování sdílenou složku. operace klonování Hello provede klonování hello zálohování tooa nový svazek na hello stejné virtuální zařízení; Nelze klonovat tooa jiné zařízení.

#### <a name="tooclone-a-volume"></a>tooclone svazku

1. Procházet příliš**zařízení** okno. Vyberte a klikněte na zařízení a pak klikněte na tlačítko **svazky**. Vybr hello svazku, které chcete tooclone, klikněte pravým tlačítkem na hello svazku tooinvoke hello kontextové nabídky. Vyberte **klon**.
   
   ![Klonování svazku](./media/storsimple-virtual-array-clone/clonevolume1.png)
2. V hello **klon** okně klikněte na tlačítko **zálohování** a pak hello následující: 
   
   a.    Filtrujte zálohování v tomto zařízení pro hello časový rozsah. Můžete si vybrat z **za posledních 7 dní**, **za posledních 30 dnů**, a **za poslední rok**. 
   
   b.    V seznamu hello filtrované záloh zobrazí vyberte zálohování tooclone z.
   
   c.    Klikněte na **OK**.
   
   ![Vytvořit kopii zálohy](./media/storsimple-virtual-array-clone/clonevolume3.png)
3. V hello **klon** okně klikněte na tlačítko **cíle svazku nastavení** a pak hello následující::
   
   a. název zařízení Hello se automaticky vyplní.
   
   b. Zadejte název svazku hello **klonovat svazku**. název svazku Hello musí obsahovat 3 znaky too127.
   
   c. Typ svazku Hello bude automaticky nastavena toohello původního svazku. Vrstvený svazek je klonovat, protože vrstvené a připnuté místně vázaný svazek jako místně.
   
   d. Pro hello **připojení hostitele**, klikněte na tlačítko **vyberte**.
   
   ![Vytvořit kopii zálohy](./media/storsimple-virtual-array-clone/clonevolume4.png)
4. V hello **připojení hostitele** okně, vyberte z existující ACR nebo přidejte nové ACR. tooadd nové ACR, budete potřebovat tooprovide název ACR a hostitele hello IQN. Klikněte na **Vybrat**.
   
   ![Vytvořit kopii zálohy](./media/storsimple-virtual-array-clone/clonevolume5.png)
5. Klikněte na tlačítko **klon** toolaunch úlohu klonování.
   
   ![Vytvořit kopii zálohy](./media/storsimple-virtual-array-clone/clonevolume6.png)  
6. Klonování se spustí po vytvoření úlohy hello klonování. Po vytvoření hello klonování se zobrazí v okně hello svazky na vašem zařízení. Všimněte si, že je jako vrstvené klonovat vrstvený svazek a místně vázaný svazek je klonovat jako místně vázaný svazek.
   
   ![Vytvořit kopii zálohy](./media/storsimple-virtual-array-clone/clonevolume8.png)
7. Jakmile online na hello seznam svazků se zobrazí hello svazek, svazek hello je k dispozici pro použití. Na hostiteli iniciátor iSCSI hello aktualizujte seznam hello cílů v okně Vlastnosti iniciátoru iSCSI. Nový cíl, který obsahuje název klonovaného svazku hello by se zobrazit jako "neaktivní" ve sloupci Stav hello.
8. Vyberte cíl hello a klikněte na tlačítko **Connect**. Cíl připojené toohello po hello iniciátor hello stav by se měl změnit příliš**připojeno**.
9. V hello **Správa disků** okně hello připojené svazky zobrazí jak ukazuje následující obrázek hello. Klikněte pravým tlačítkem na hello zjištěné svazek (klikněte na název disku hello) a potom klikněte na **Online**.

> [!IMPORTANT]
> Při pokusu o tooclone svazek nebo sdílenou složku ze zálohovacího skladu, pokud úloha hello klonování selže, cílový svazek nebo sdílenou složku stále možné vytvářet hello portálu. Je důležité odstranit tohoto cílového svazku nebo sdílené složky v portálu toominimize hello budoucí potíží vzniklých tohoto elementu.
> 
> 

## <a name="item-level-recovery-ilr"></a>Obnovení na úrovni položek (ILR)

Tato verze přináší obnovení na úrovni položek hello (ILR) v poli virtuální zařízení StorSimple nakonfigurovaný jako souborový server. Funkce Hello umožňuje toodo podrobné obnovení souborů a složek ze záloh cloudu všechny sdílené složky hello na zařízení StorSimple hello. Můžete načíst odstraněných souborů z poslední zálohy použití modelu samoobslužné služby.

Každé sdílené složky mají *.backups* složku, která obsahuje nejnovější zálohy hello. Můžete přejděte toohello požadované zálohování, zkopírujte příslušné soubory a složky ze zálohy hello a jejich obnovení. Tato funkce eliminuje tooadministrators volání pro obnovení souborů ze zálohy.

1. Při provádění hello ILR, můžete zobrazit hello zálohování pomocí Průzkumníka souborů. Klikněte na konkrétní sdílenou složku hello, kterou chcete toolook na hello zálohování. Zobrazí se *.backups* složky vytvořil v rámci hello sdílenou složku, která ukládá všechny zálohy hello. Rozbalte hello *.backups* složky tooview hello zálohy. Složka Hello ukazuje hello pohledem hello celý zálohování hierarchie. Toto zobrazení je vytvořený na vyžádání a trvá jenom pár sekund toocreate obvykle.
   
   posledních pět zálohy Hello se zobrazují tímto způsobem a může být použité tooperform na úrovni položky obnovení. Hello pět nedávné zálohy současně obsahovat hello výchozí naplánovaný a hello ručního zálohování.
   
   * **Naplánované zálohy** pojmenovány jako &lt;název zařízení&gt;DailySchedule RRRRMMDD HHMMSS UTC.
   * **Ruční zálohování** pojmenovány jako Ad-hoc RRRRMMDD-HHMMSS-UTC.
     
     ![](./media/storsimple-virtual-array-clone/image14.png)

2. Identifikujte hello zálohu obsahující hello nejnovější verzi souboru hello odstranit. V případě, že název složky hello obsahuje časové razítko UTC v každé z předchozích případech hello, je čas hello které hello byla vytvořena složka hello skutečné zařízení čas, kdy hello zálohování zahájena. Použijte hello složky časové razítko toolocate a určit hello zálohy.

3. Vyhledejte složku hello nebo hello soubor, který chcete toorestore hello zálohování, který jste identifikovali v předchozím kroku hello. Všimněte si, že můžete zobrazit jenom hello soubory nebo složky, které máte oprávnění pro. Pokud nemají přístup k určité soubory nebo složky, obraťte se na správce sdílené složky. Správce Hello pomocí oprávnění ke sdílení hello tooedit Průzkumníka souborů a získáte přístup toohello určitého souboru nebo složky. Je, že doporučené osvědčený postup, který hello správce sdílená složka je skupina uživatelů místo jednoho uživatele.

4. Zkopírujte soubor hello nebo hello složky toohello příslušné složky na souborovém serveru StorSimple.

## <a name="next-steps"></a>Další kroky

Další informace o příliš[spravovat vaše pole virtuální zařízení StorSimple pomocí hello místního webového uživatelského rozhraní](storsimple-ova-web-ui-admin.md).

