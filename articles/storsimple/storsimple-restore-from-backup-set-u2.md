---
title: "aaaRestore svazek StorSimple ze zálohy | Microsoft Docs"
description: "Vysvětluje, jak toouse hello toorestore stránky zálohování katalogu služby StorSimple Manager svazek StorSimple ze zálohovacího skladu."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 6f289c39-96c7-4d57-b68a-4bc2e99aef9d
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 03/22/2017
ms.author: alkohli
ms.openlocfilehash: c2e38765e750749f5764b5cbf2167d3cd5edfe5d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="restore-a-storsimple-volume-from-a-backup-set-update-2"></a>Obnovit svazek StorSimple ze zálohovacího skladu (Update 2)
[!INCLUDE [storsimple-version-selector-restore-from-backup](../../includes/storsimple-version-selector-restore-from-backup.md)]

## <a name="overview"></a>Přehled
Hello **zálohování katalogu** stránka zobrazuje všechny zálohovací sklady hello, které vytvářejí, když jsou provedeny ruční nebo automatické zálohy. Pomocí této stránky toolist a spravovat zálohy, obnovení ze zálohovacího skladu nebo klonování svazku.

 ![Zálohování stránky katalogu](./media/storsimple-restore-from-backup-set-u2/restore.png)

Tento kurz vysvětluje, jak toouse hello **zálohování katalogu** stránka toorestore zařízení ze zálohovacího skladu.

Můžete obnovit svazek z místní nebo cloudové zálohování. V obou případech operace obnovení hello přináší hello svazek online okamžitě, když data se stáhne na pozadí hello. 

## <a name="before-you-restore"></a>Před obnovením
Před spuštěním operace obnovení, byste měli vědět o hello následující upozornění:

* **Do offline režimu hello svazku** – trvat hello svazek offline na obou hello hostiteli a hello zařízení před spuštěním operace obnovení hello. Přestože operace obnovení hello automaticky přináší hello svazek online na hello zařízení, je nutné ručně přenést hello zařízení online na hostiteli hello. Převedete hello svazek online na hostiteli hello, pokud hello svazek je online na hello zařízení. (Není nutné toowait dokončení operace obnovení hello.) Postupy, přejděte příliš[do offline režimu svazku](storsimple-manage-volumes-u2.md#take-a-volume-offline).
* **Typ svazku po obnovení** – odstraněné svazky jsou obnoveny podle typu hello v hello snímku. Svazky, které byly místně vázaný se obnoví jako místně vázaných svazků a svazky, které byly zřízeny vrstvené se obnoví jako vrstvené svazky.
  
    Pro existující svazky přepíše hello aktuální typ použití svazku hello hello typ, který je uložen v hello snímku. Například pokud obnovujete svazek ze snímku, která se provede, když byla vrstvené hello typ svazku a, typ svazku je nyní místně připnuli (z důvodu tooa operaci převodu), potom je obnovit svazek hello jako místně vázaný svazek. Podobně pokud existující místně vázaný svazek je rozšířena a následně obnovit ze starší snímek pořízený při menší hello svazku, hello obnovený svazek zachová aktuální rozbalenou velikost hello.
  
    Svazek nelze převést na vrstvený svazek tooa místně vázaný svazek nebo _naopak_ při hello svazek je obnovena. Počkejte na dokončení operace obnovení hello a pak můžete převést typ tooanother hello svazku. Informace o převodu svazku, přejděte příliš[změnit typ svazku hello](storsimple-manage-volumes-u2.md#change-the-volume-type). 
* **Hello velikost svazku se projeví ve svazku hello obnovit** – Toto je důležitý faktor, pokud obnovujete místně vázaný svazek, který byl odstraněn, (protože místně vázaných svazků jsou plně zřízený). Ujistěte se, abyste měli dostatek místa, než se pokusíte toorestore místně vázaný svazek, která byla dříve odstraněna. 
* **Nelze rozbalit svazku, zatímco je obnovena** – počkejte na dokončení operace obnovení hello před dalším pokusem tooexpand hello svazku. Informace o rozšiřování svazek, přejděte příliš[upravit svazek](storsimple-manage-volumes-u2.md#modify-a-volume).
* **Můžete provést zálohu, když obnovujete svazek místní** – postupy naleznete příliš[použít zásady zálohování služby toomanage StorSimple Manager pro hello](storsimple-manage-backup-policies.md).
* **Můžete zrušit operaci obnovení** – Pokud zrušíte hello úlohy obnovení, pak svazek hello je vrácena zpět toohello stavu, který byl předtím, než jste spustili hello obnovení. Postupy, přejděte příliš[zrušení úlohy](storsimple-manage-jobs-u2.md#cancel-a-job).

## <a name="how-does-restore-work"></a>Jak obnovit práci
Pro zařízení se systémem Update 4 nebo novější se implementuje na základě heatmap obnovení. Jako data tooaccess požadavky hostitele hello dostat hello zařízení, tyto požadavky jsou sledovány a je vytvořen heatmap. Rychlost vysokou požadavků výsledkem bloky dat s vyšší heat, zatímco nižší rychlost požadavků překládá toochunks s nižší heat. Je nutné získat přístup hello data alespoň dvakrát toobe označen jako _aktivní_. Soubor, který se mění je také označena jako _aktivní_. Po zahájení obnovení hello nastane proaktivní dosazováním dat podle hello heatmap. Verze starší než aktualizace 4 hello dat byl stažen během obnovení založená na přístupu jen. 

Na základě Heatmap sledování je povoleno pouze pro vrstvené svazky a místně vázaný svazky nejsou podporované. Obnovení na základě Heatmap není podporováno také při klonování svazku tooanother zařízení. Pokud dojde obnovení na místě a místní snímek toobe svazku hello obnovit v zařízení hello existuje, pak jsme není rehydrataci při spotřebě (protože data je již k dispozici místně). Ve výchozím nastavení při obnovení aplikace hello rehydrataci úloh se spouští které proaktivně rehydrataci při spotřebě dat podle hello heatmap. V aktualizaci 4 můžete rutiny prostředí Windows PowerShell se použité tooquery spuštěných úloh rehydrataci, zrušení úlohy rehydrataci a získat hello stav úlohy rehydrataci hello.

* `Get-HcsRehydrationJob`– Tato rutina načte hello stav úlohy rehydrataci hello. Aktivuje úlohu jeden rehydrataci se pro jeden svazek.
* `Set-HcsRehydrationJob`– Tato rutina vám umožní toopause, zastavit, obnovení hello rehydrataci úlohy, když probíhá rehydrataci hello. 

Další informace o rutinách rehydrataci přejděte příliš[odkazu na rutiny Windows Powershellu pro StorSimple](https://technet.microsoft.com/library/dn688168.aspx).

S automatickou rehdyration obvykle vyšší výkon přechodný pro čtení se očekává. skutečné magniutde Hello vylepšení závisí na různých faktorech, jako je například vzor přístupu, mísení dat a typu dat. toocancel úlohu rehydrataci, můžete použít rutiny prostředí PowerShell hello. Pokud chcete úlohy rehydrataci toopermanently zakázat pro všechny budoucí obnovení hello, obraťte se na Microsoft Support.

## <a name="how-toouse-hello-backup-catalog"></a>Jak toouse hello zálohování katalogu
Hello **zálohování katalogu** stránka obsahuje dotaz, který vám pomůže toonarrow výběr zálohovacího skladu. Můžete filtrovat hello zálohování sad, které jsou načteny podle hello následující parametry:

* **Zařízení** – hello zařízení, na které hello zálohovací sklad vytvořen.
* **Zásady zálohování** nebo **svazku** – hello zásady zálohování nebo svazku přidruženém k tohoto zálohovacího skladu.
* **Z** a **k** – hello rozsah data a času v okamžiku vytvoření zálohovacího skladu hello.

Hello filtrované zálohovací sklady jsou pak v tabulce podle hello následující atributy:

* **Název** – hello název zásady zálohování hello nebo svazku přidruženém k hello zálohovacího skladu.
* **Velikost** – hello skutečná velikost hello zálohovacího skladu.
* **Vytvořit v** – hello datum a čas, kdy byla vytvořena hello zálohy. 
* **Typ** – zálohovací sklady může být místní snímky nebo cloudových snímků. Místní snímek je zálohování všech dat uložených místně na zařízení hello svazku. Cloudový snímek odkazuje toohello zálohování svazku dat umístěných v cloudu hello. Místní snímky poskytují rychlejší přístup, že jsou pro záleží na odolnosti dat zvolena cloudových snímků.
* **Zahájit** – hello zálohy lze inicializovat automaticky podle plánu tooa nebo ručně pomocí. (Můžete použít zálohování tooschedule zásady zálohování. Alternativně můžete použít hello **provést zálohování** možnost tootake interaktivní zálohování.)

## <a name="how-toorestore-your-storsimple-volume-from-a-backup"></a>Jak toorestore svazek StorSimple ze zálohy
Můžete použít hello **zálohování katalogu** stránka toorestore svazek StorSimple ze konkrétní zálohy. Mějte na paměti, ale, že obnovení svazku vrátí hello svazku toohello stavu, ve kterém byla, když hello zálohy. Žádná data, která byla přidána po ztrátě hello operace zálohování.

> [!WARNING]
> Obnovení ze zálohy nahradí existující svazky hello ze zálohy hello. To může způsobit ztrátu hello všechna data, která byla zapsána po hello zálohy.
> 
> 

### <a name="toorestore-your-volume"></a>toorestore svazku
1. Na stránce služby StorSimple Manager hello, klikněte na hello **katalog zálohování** kartě.
   
    ![Zálohování katalogu](./media/storsimple-restore-from-backup-set-u2/restore.png)
2. Vyberte zálohovací sklad následujícím způsobem:
   
   1. Vyberte příslušné zařízení hello.
   2. V rozevíracím seznamu hello výběru hello svazek nebo zálohování zásady zálohování hello chcete tooselect.
   3. Zadejte časové rozmezí hello.
   4. Klikněte na ikonu zaškrtnutí hello ![ikona zaškrtnutí](./media/storsimple-restore-from-backup-set-u2/HCS_CheckIcon.png) tooexecute tento dotaz.
      
      Hello zálohování přidružená k zásadě hello vybraný svazek nebo zálohování by se zobrazit v seznamu hello zálohovací sklady.
3. Rozbalte hello zálohovacího skladu tooview hello přidružené svazky. Tyto svazky musí být převedeno do režimu offline v hostiteli hello a zařízení, než bude možné obnovit. Získat přístup ke svazkům hello na hello **kontejnery svazků** stránky a potom postupujte podle kroků hello v [do offline režimu svazek](storsimple-manage-volumes-u2.md#take-a-volume-offline) tootake je v offline režimu.
   
   > [!IMPORTANT]
   > Ujistěte se, že jste provedli hello svazky do offline režimu na hostiteli hello nejprve, před provedením hello svazky do režimu offline v zařízení hello. Pokud neprovedete offline hello svazky na hostiteli hello, může potenciálně vést k poškození toodata.
   > 
   > 
4. Přejděte zpět toohello **zálohování katalogu** a vyberte zálohovacího skladu.
5. Klikněte na tlačítko **obnovení** v hello dolní části stránky hello.
6. Zobrazí se výzva k potvrzení. Zkontrolujte hello obnovit informace a pak vyberte hello potvrzení zaškrtávací políčko.
   
    ![Stránka potvrzení](./media/storsimple-restore-from-backup-set-u2/ConfirmRestore.png)
7. Klikněte na ikonu zaškrtnutí hello ![ikona zaškrtnutí](./media/storsimple-restore-from-backup-set-u2/HCS_CheckIcon.png). Spustí úlohu obnovení. Můžete zobrazit úlohy hello přístup k hello **úlohy** stránky. 
8. Po dokončení obnovení hello můžete ověřit, že hello obsah svazků jsou nahrazovány svazků ze zálohy hello.

![Dostupné video](./media/storsimple-restore-from-backup-set-u2/Video_icon.png)**Dostupné video**

Klikněte na tlačítko toowatch video, které ukazuje, jak můžete použít klonu hello a obnovení funkcí v zařízení StorSimple toorecover odstranit soubory, [zde](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).

## <a name="if-hello-restore-fails"></a>Pokud hello obnovení se nezdaří
Obdržíte výstrahu, pokud hello se nezdaří operace obnovení z jakéhokoli důvodu. Pokud k tomu dojde, tooverify zálohování seznamu hello aktualizace, která hello zálohování je stále platný. Pokud je záloha hello platná a zda obnovujete z cloudu hello, pak problémy s připojením k může být příčinou problému hello. 

toocomplete hello operaci obnovení, na hostiteli hello trvat hello svazku do režimu offline a opakujte operaci obnovení hello. Žádná data svazku toohello úpravy, které byly provedeny během procesu obnovení hello se ztratí.

## <a name="next-steps"></a>Další kroky
* Zjistěte, jak příliš[svazky spravovat zařízení StorSimple](storsimple-manage-volumes-u2.md).
* Zjistěte, jak příliš[použití hello tooadminister služby StorSimple Manager zařízení StorSimple](storsimple-manager-service-administration.md).

