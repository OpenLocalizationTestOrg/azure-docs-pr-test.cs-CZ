---
title: "aaaRestore svazek ze zálohy na řadu zařízení StorSimple 8000 | Microsoft Docs"
description: "Vysvětluje, jak toouse hello toorestore zálohování katalogu služby StorSimple Manager zařízení svazek StorSimple ze zálohovacího skladu."
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
ms.date: 05/23/2017
ms.author: alkohli
ms.openlocfilehash: 0fe2e4c23a23c75ce4058a8531356c94c973c6f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="restore-a-storsimple-volume-from-a-backup-set"></a>Obnovit svazek StorSimple ze zálohovacího skladu

## <a name="overview"></a>Přehled

Tento kurz popisuje operace obnovení hello provést na zařízení řady StorSimple 8000 pomocí existujícího zálohovacího skladu. Použití hello **katalog zálohování** okno toorestore svazku z místní nebo cloudové zálohování. Hello **katalog zálohování** zobrazuje všechny zálohovací sklady hello, které vytvářejí, když jsou provedeny ruční nebo automatické zálohy. operace obnovení Hello ze zálohovacího skladu přináší hello svazek online okamžitě, když data se stáhne na pozadí hello.

Obnovení alternativní metodu toostart je příliš toogo**zařízení > [zařízení] > svazky**. V hello **svazky** okně, vyberte svazek, klikněte pravým tlačítkem na tooinvoke hello kontextovou nabídku a pak vyberte **obnovení**.

## <a name="before-you-restore"></a>Před obnovením

Před zahájením obnovení, zkontrolujte hello těchto aspektů:

* **Je nutné provést hello svazek offline** – trvat hello svazek offline na obou hello hostiteli a hello zařízení před spuštěním operace obnovení hello. Přestože operace obnovení hello automaticky přináší hello svazek online na hello zařízení, je nutné ručně přenést hello zařízení online na hostiteli hello. Převedete hello svazek online na hostiteli hello, jakmile hello svazek je online na hello zařízení. (Není nutné toowait dokončení operace obnovení hello.) Postupy, přejděte příliš[do offline režimu svazku](storsimple-8000-manage-volumes-u2.md#take-a-volume-offline).

* **Typ svazku po obnovení** – odstraněné svazky jsou obnoveny podle typu hello v hello snímek; to znamená, svazky, které byly místně vázaný se obnoví jako místně vázaných svazků a svazky, které byly zřízeny vrstvené se obnoví jako vrstvené svazky.

    Pro existující svazky přepíše hello aktuální typ použití svazku hello hello typ, který je uložen v hello snímku. Například pokud obnovit svazek ze snímku, která se provede, když byla vrstvené hello typ svazku a že typ svazku je nyní místně ukotvena (z důvodu operace převodu tooa, která byla provedena), pak hello svazku se obnoví jako místně vázaný svazek. Podobně pokud existující místně vázaný svazek byla rozšířena a následně obnovit ze starší snímek pořízený při menší hello svazku, hello obnovený svazek bude zachovat aktuální rozbalenou velikost hello.

    Nelze převést svazek vrstvený svazek tooa místně vázaný svazek nebo z tooa místně vázaný svazek vrstvené svazku, zatímco hello svazek je obnovena. Počkejte na dokončení operace obnovení hello a pak můžete převést typ tooanother hello svazku. Informace o převodu svazku, přejděte příliš[změnit typ svazku hello](storsimple-8000-manage-volumes-u2.md#change-the-volume-type). 

* **Hello velikost svazku se projeví ve svazku hello obnovit** – Toto je důležitý faktor, pokud obnovujete místně vázaný svazek, který byl odstraněn, (protože místně vázaných svazků jsou plně zřízený). Ujistěte se, abyste měli dostatek místa, než se pokusíte toorestore místně vázaný svazek, která byla dříve odstraněna.

* **Nelze rozbalit svazku, zatímco je obnovena** – počkejte na dokončení operace obnovení hello před dalším pokusem tooexpand hello svazku. Informace o rozšiřování svazek, přejděte příliš[upravit svazek](storsimple-8000-manage-volumes-u2.md#modify-a-volume).

* **Při obnovení místního svazku můžete provést zálohu** – postupy naleznete příliš[použít zásady zálohování služby toomanage Správce zařízení StorSimple pro hello](storsimple-8000-manage-backup-policies-u2.md).

* **Můžete zrušit operaci obnovení** – Pokud zrušíte hello úlohy obnovení, pak hello svazek bude vrácena zpět toohello stavu, který byl předtím, než jste spustili hello operace obnovení. Postupy, přejděte příliš[zrušení úlohy](storsimple-8000-manage-jobs-u2.md#cancel-a-job).

## <a name="how-does-restore-work"></a>Jak obnovit práci

Pro zařízení se systémem Update 4 nebo novější se implementuje na základě heatmap obnovení. Jako data tooaccess požadavky hostitele hello dostat hello zařízení, tyto požadavky jsou sledovány a je vytvořen heatmap. Rychlost vysokou požadavků výsledkem bloky dat s vyšší heat, zatímco nižší rychlost požadavků překládá toochunks s nižší heat. Je nutné získat přístup hello data alespoň dvakrát toobe označen jako _aktivní_. Soubor, který se mění je také označena jako _aktivní_. Po zahájení obnovení hello nastane proaktivní dosazováním dat podle hello heatmap. Verze starší než aktualizace 4 hello dat se stáhne během obnovení založená na přístupu jen.

následující upozornění Hello použít obnovení založené na tooheatmap:

* Sledování Heatmap je povolená jenom pro vrstvené svazky a místně vázaných svazků nejsou podporovány.

* Při klonování svazku tooanother zařízení, na základě Heatmap obnovení není podporováno. 

* Pokud dojde obnovení na místě a místní snímek toobe svazku hello obnovit v zařízení hello existuje, pak jsme není rehydrataci při spotřebě (protože data je již k dispozici místně). 

* Ve výchozím nastavení při obnovení aplikace hello rehydrataci úloh se spouští které proaktivně rehydrataci při spotřebě dat podle hello heatmap. 

V aktualizaci 4 můžete rutiny prostředí Windows PowerShell se použité tooquery spuštěných úloh rehydrataci, zrušení úlohy rehydrataci a získat hello stav úlohy rehydrataci hello.

* `Get-HcsRehydrationJob`– Tato rutina načte hello stav úlohy rehydrataci hello. Aktivuje úlohu jeden rehydrataci se pro jeden svazek.

* `Set-HcsRehydrationJob`– Tato rutina vám umožní toopause, zastavit, obnovení hello rehydrataci úlohy, když probíhá rehydrataci hello.

Další informace o rutinách rehydrataci přejděte příliš[odkazu na rutiny Windows Powershellu pro StorSimple](https://technet.microsoft.com/library/dn688168.aspx).

S automatickou rehdyration obvykle vyšší výkon přechodný pro čtení se očekává. skutečné magniutde Hello vylepšení závisí na různých faktorech, jako je například vzor přístupu, mísení dat a typu dat. 

toocancel úlohu rehydrataci, můžete použít rutiny prostředí PowerShell hello. Pokud chcete toopermanently zakázat rehydrataci úloh pro všechny budoucí obnovení hello [kontaktovat Microsoft Support](storsimple-8000-contact-microsoft-support.md).

## <a name="how-toouse-hello-backup-catalog"></a>Jak toouse hello zálohování katalogu

Hello **zálohování katalogu** okno obsahuje dotaz, který vám pomůže toonarrow výběr zálohovacího skladu. Můžete filtrovat hello zálohování sad, které jsou načteny podle hello následující parametry:

* **Čas rozsah** – hello rozsah data a času v okamžiku vytvoření zálohovacího skladu hello.
* **Zařízení** – hello zařízení, na které hello zálohovací sklad vytvořen.
* **Zásady zálohování** nebo **svazku** – hello zásady zálohování nebo svazku přidruženém k tohoto zálohovacího skladu.

Hello filtrované zálohovací sklady jsou pak v tabulce podle hello následující atributy:

* **Název** – hello název zásady zálohování hello nebo svazku přidruženém k hello zálohovacího skladu.
* **Typ** – zálohovací sklady může být místní snímky nebo cloudových snímků. Místní snímek je zálohování všech dat uložených místně na zařízení hello svazku, zatímco cloudový snímek odkazuje toohello zálohování svazku dat umístěných v cloudu hello. Místní snímky poskytují rychlejší přístup, že jsou pro záleží na odolnosti dat zvolena cloudových snímků.
* **Velikost** – hello skutečná velikost hello zálohovacího skladu.
* **Vytvořit v** – hello datum a čas, kdy byla vytvořena hello zálohy. 
* **Svazky** -hello počtu svazků, které jsou přidružené k hello zálohovacího skladu.
* **Iniciované** – hello zálohy lze inicializovat automaticky podle plánu tooa nebo ručně uživatelem. (Můžete použít zálohování tooschedule zásady zálohování. Alternativně můžete použít hello **provést zálohování** možnost tootake zálohu interaktivní nebo na vyžádání.)

## <a name="how-toorestore-your-storsimple-volume-from-a-backup"></a>Jak toorestore svazek StorSimple ze zálohy

Můžete použít hello **zálohování katalogu** okno toorestore svazek StorSimple ze konkrétní zálohy. Mějte na paměti, ale, obnovení svazku vrátíte hello svazku toohello stavu, ve kterém byla, když hello zálohy. Žádná data, která byla přidána po hello operace zálohování se ztratí.

> [!WARNING]
> Obnovení ze zálohy nahradí existující svazky hello ze zálohy hello. To může způsobit ztrátu hello všechna data, která byla zapsána po hello zálohy.


### <a name="toorestore-your-volume"></a>toorestore svazku
1. Přejděte služby StorSimple Manager zařízení tooyour a pak klikněte na tlačítko **katalog zálohování**.

2. Vyberte zálohovací sklad následujícím způsobem:
   
   1. Zadejte časové rozmezí hello.
   2. Vyberte příslušné zařízení hello.
   3. V rozevíracím seznamu hello výběru hello svazek nebo zálohování zásady zálohování hello chcete tooselect.
   4. Klikněte na tlačítko **použít** tooexecute tento dotaz.

    Hello zálohování přidružená k zásadě hello vybraný svazek nebo zálohování by se zobrazit v seznamu hello zálohovací sklady.
   
    ![Zálohovací sklad seznamu](./media/storsimple-8000-restore-from-backup-set-u2/bucatalog.png)     
     
3. Rozbalte hello zálohovacího skladu tooview hello přidružené svazky. Tyto svazky musí být převedeno do režimu offline v hostiteli hello a zařízení, než bude možné obnovit. Získat přístup ke svazkům hello na hello **svazky** okno zařízení a pak hello postupujte podle kroků v [do offline režimu svazek](storsimple-8000-manage-volumes-u2.md#take-a-volume-offline) tootake je v offline režimu.
   
   > [!IMPORTANT]
   > Ujistěte se, že jste provedli hello svazky do offline režimu na hostiteli hello nejprve, před provedením hello svazky do režimu offline v zařízení hello. Pokud neprovedete offline hello svazky na hostiteli hello, může potenciálně vést k poškození toodata.
   
4. Přejděte zpět toohello **zálohování katalogu** a vyberte zálohovacího skladu. Klikněte pravým tlačítkem a pak z hello kontextové nabídky, vyberte **obnovení**.

    ![Zálohovací sklad seznamu](./media/storsimple-8000-restore-from-backup-set-u2/restorebu1.png)

5. Zobrazí se výzva k potvrzení. Zkontrolujte hello obnovit informace a pak vyberte hello potvrzení zaškrtávací políčko.
   
    ![Stránka potvrzení](./media/storsimple-8000-restore-from-backup-set-u2/restorebu2.png)

7.  Klikněte na tlačítko **obnovení**. Tím se vyvolá úlohy obnovení, který si můžete zobrazit přístup k hello **úlohy** stránky.

    ![Stránka potvrzení](./media/storsimple-8000-restore-from-backup-set-u2/restorebu5.png)

8. Po dokončení obnovení hello ověřte, že hello obsah svazků jsou nahrazovány svazků ze zálohy hello.


## <a name="if-hello-restore-fails"></a>Pokud hello obnovení se nezdaří

Obdržíte výstrahu, pokud hello se nezdaří operace obnovení z jakéhokoli důvodu. Pokud k tomu dojde, tooverify zálohování seznamu hello aktualizace, která hello zálohování je stále platný. Pokud je záloha hello platná a zda obnovujete z cloudu hello, pak problémy s připojením k může být příčinou problému hello.

toocomplete hello operaci obnovení, na hostiteli hello trvat hello svazku do režimu offline a opakujte operaci obnovení hello. Všimněte si, že žádná data svazku toohello úpravy, které byly provedeny během hello procesu obnovení budou ztraceny.

## <a name="next-steps"></a>Další kroky
* Zjistěte, jak příliš[svazky spravovat zařízení StorSimple](storsimple-8000-manage-volumes-u2.md).
* Zjistěte, jak příliš[použití hello tooadminister service Manager zařízení StorSimple zařízení StorSimple](storsimple-8000-manager-service-administration.md).

