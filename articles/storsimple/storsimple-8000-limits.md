---
title: "omezení systému řady aaaStorSimple 8000 | Microsoft Docs"
description: "Popisuje doporučená velikost pro připojení a součásti řady StorSimple 8000 a omezení systému."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: c7392678-0924-46c6-9c59-1665cb9b6586
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 03/28/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: def89a2c1d0ddc71f1743e592c5112b09d72e971
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="what-are-storsimple-8000-series-system-limits"></a>Jaká jsou omezení systému řady StorSimple 8000?

## <a name="overview"></a>Přehled

StorSimple poskytuje škálovatelná a flexibilní úložiště vašeho datového centra. Nicméně existují některá omezení, které jste měli vzít v úvahu při plánování, nasazení a provoz vašeho řešení StorSimple. Hello následující tabulka popisuje tyto limity a uvádí několik doporučení, aby vám hello nejvíce mimo vašeho řešení StorSimple.

| Identifikátor omezení | Omezení | Komentáře |
| --- | --- | --- |
| Maximální počet přihlašovacích údajů účtu úložiště |64 | |
| Maximální počet kontejnery svazků |64 | |
| Maximální počet svazků |255 | |
| Maximální počet místně vázaných svazků |32 | |
| Maximální počet plány na šablony šířky pásma |168 |Plán pro každou hodinu, každý den v týdnu hello (24 * 7). |
| Maximální velikost vrstvený svazek na fyzických zařízení |64 TB pro 8100 a 8600 |8100 a 8600 jsou fyzické zařízení. |
| Maximální velikost vrstvený svazek na virtuálním zařízením v Azure |30 TB pro 8010 <br></br> 64 TB pro 8020 |8010 a 8020 jsou virtuální zařízení v Azure, které používají úložiště úrovně Standard a Premium Storage v uvedeném pořadí. |
| Maximální velikost místně vázaný svazek na fyzických zařízení |Šířka 8,5 TB pro 8100 <br></br> 22,5 TB pro 8600 |8100 a 8600 jsou fyzické zařízení. |
| Maximální počet připojení iSCSI |512 | |
| Maximální počet připojení iniciátorů iSCSI |512 | |
| Maximální počet záznamů řízení přístupu podle zařízení |64 | |
| Maximální počet svazků na zásady zálohování |20 | |
| Maximální počet záloh uchovávány v plánu (v zásadu zálohování) |64 | |
| Maximální počet plány na zásady zálohování |10 | |
| Maximální počet snímků žádný typ, který uchovávání může být na svazku |256 |Tato hodnota zahrnuje místních snímků a cloudových snímků. |
| Maximální počet snímků, které můžou být v libovolném zařízení |10 000 | |
| Maximální počet svazků, které lze zpracovat paralelní zálohování, obnovení nebo klonování |16 |<ul><li>Pokud existuje víc než 16 svazky, jsou zpracovávány postupně, jakmile sloty zpracování k dispozici.</li><li>Nové zálohy klonovaný nebo obnovený vrstvený svazek nemůže proběhnout, dokud hello operace byla dokončena. Pro místní svazek, jsou však zálohy povoleny, po hello svazek je online.</li></ul> |
| Obnovení a klonování obnovit dobu vrstvené svazky |< 2 minuty |<ul><li>svazek Hello je k dispozici během 2 minut operaci obnovení nebo klonování, bez ohledu na velikost svazku hello.</li><li>původně může být pomalejší než běžné jako většinu hello data a metadata se stále nachází v cloudu hello Hello svazku výkon. Jako toky dat ze zařízení StorSimple toohello hello cloudu může zvýšit výkon.</li><li>metadata toodownload celkový čas Hello závisí na hello přidělené velikost svazku. Metadata se automaticky přenesou do zařízení hello hello pozadí rychlostí hello 5 minut za TB dat přidělené svazku. Tato míra může mít vliv cloudu toohello šířky pásma Internetu.</li><li>Hello obnovení nebo operaci klonování je dokončená, když všechny hello metadata jsou na zařízení hello.</li><li>Operace zálohování nelze provést, dokud nebude hello obnovení nebo je plně dokončení operace klonování. |
| Obnovení obnovit dobu místně vázaných svazků |< 2 minuty |<ul><li>svazek Hello je k dispozici během 2 minut hello operaci obnovení, bez ohledu na velikost svazku hello.</li><li>původně může být pomalejší než běžné jako většinu hello data a metadata se stále nachází v cloudu hello Hello svazku výkon. Jako toky dat ze zařízení StorSimple toohello hello cloudu může zvýšit výkon.</li><li>metadata toodownload celkový čas Hello závisí na hello přidělené velikost svazku. Metadata se automaticky přenesou do zařízení hello hello pozadí rychlostí hello 5 minut za TB dat přidělené svazku. Tato míra může mít vliv cloudu toohello šířky pásma Internetu.</li><li>Na rozdíl od vrstvené svazky místně vázaných svazků hello svazku dat je také stažen místně na hello zařízení. operace obnovení Hello je dokončena, pokud všechna data hello svazek znovu toohello zařízení.</li><li>operace obnovení Hello může trvat dlouho. Hello celkový čas toocomplete hello obnovení závisí na velikosti hello místního svazku hello zřízený, šířky pásma Internetu a hello stávající data v zařízení hello. Operace zálohování na hello místně vázaný svazek jsou povoleny, když probíhá operace obnovení hello. |
| Rychlost zpracování pro cloudové snímky |15 minut nebo TB |<ul><li>Minimální hodnota času toomake hello cloudový snímek připravené k odeslání za TB dat přidělené svazku v zálohování. </li><li> Celkový počet cloudových snímků čas se počítá přidáním tento čas toohello snímku nahrávání času, který je ovlivňován toocloud šířky pásma Internetu hello. |
| Propustnost pro čtení a zápis maximálního počtu klientů (Pokud obsluhovat z vrstvy SSD hello) * |920/720 MB/s jeden 10 GbE síťovým rozhraním |Až too2x pomocí funkce MPIO a dvě síťová rozhraní. |
| Propustnost pro čtení a zápis maximálního počtu klientů (Pokud obsluhovat z vrstvy HDD hello) * |120/250 MB/s | |
| Propustnost pro čtení a zápis maximálního počtu klientů (Pokud obsluhovat z vrstvy cloudu hello) * pro Update 3 a vyšší ** |40/60 MB/s pro vrstvené svazky<br><br>60/80 MB/s pro vrstvené svazky s archivace zvolené během vytvoření svazku |Propustnost čtení závisí na klientech generování a údržbu dostatečná hloubku fronty vstupně-výstupní operace. <br><br>Rychlost dosáhnout závisí na rychlosti hello hello základní používá účet úložiště. |

&#42; Maximální propustnost podle typu vstupně-výstupních operací se měří s 100 procent čtení a zápisu 100 procent scénáře. Skutečná propustnost může být nižší a závisí na vstupně-výstupních operací kombinovat a síťové podmínky.

&#42; &#42; Předchozí tooUpdate výkonu čísla 3 může být nižší.

## <a name="next-steps"></a>Další kroky
Zkontrolujte hello [požadavky na systém StorSimple](storsimple-8000-system-requirements.md).

