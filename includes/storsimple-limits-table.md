<!--author=alkohli last changed: 12/15/15-->

| Identifikátor omezení | Omezení | Komentáře |
| --- | --- | --- |
| Maximální počet přihlašovacích údajů účtu úložiště |64 | |
| Maximální počet kontejnery svazků |64 | |
| Maximální počet svazků |255 | |
| Maximální počet plány na šablony šířky pásma |168 |Plán pro každou hodinu, každý den v týdnu hello (24 * 7). |
| Maximální velikost vrstvený svazek na fyzických zařízení |64 TB pro 8100 a 8600 |8100 a 8600 jsou fyzické zařízení. |
| Maximální velikost vrstvený svazek na virtuálním zařízením v Azure |30 TB pro 8010 <br></br> 64 TB pro 8020 |8010 a 8020 jsou virtuální zařízení v Azure, které používají úložiště úrovně Standard a Premium Storage v uvedeném pořadí. |
| Maximální velikost místně vázaný svazek na fyzických zařízení |9 TB pro 8100 <br></br> 24 TB pro 8600 |8100 a 8600 jsou fyzické zařízení. |
| Maximální počet připojení iSCSI |512 | |
| Maximální počet připojení iniciátorů iSCSI |512 | |
| Maximální počet záznamů řízení přístupu podle zařízení |64 | |
| Maximální počet svazků na zásady zálohování |24 | |
| Maximální počet záloh uchovávány v zásady zálohování |64 | |
| Maximální počet plány na zásady zálohování |10 | |
| Maximální počet snímků žádný typ, který uchovávání může být na svazku |256 |Jedná se o místní snímky a cloudových snímků. |
| Maximální počet snímků, které můžou být v libovolném zařízení |10 000 | |
| Maximální počet svazků, které lze zpracovat paralelní zálohování, obnovení nebo klonování |16 |<ul><li>Pokud existuje víc než 16 svazky, budou se zpracují postupně zpracování sloty dostupná.</li><li>Nové zálohy klonovaný nebo obnovený vrstvený svazek nemůže proběhnout, dokud hello operace byla dokončena. Pro místní svazek, jsou však zálohy povoleny, po hello svazek je online.</li></ul> |
| Obnovení a klonování obnovit dobu vrstvené svazky |< 2 minuty |<ul><li>svazek Hello je k dispozici během 2 minut operaci obnovení nebo klonování, bez ohledu na velikost svazku hello.</li><li>původně může být pomalejší než běžné jako většinu hello data a metadata se stále nachází v cloudu hello Hello svazku výkon. Jako toky dat ze zařízení StorSimple toohello hello cloudu může zvýšit výkon.</li><li>metadata toodownload celkový čas Hello závisí na hello přidělené velikost svazku. Metadata se automaticky přenesou do zařízení hello hello pozadí rychlostí hello 5 minut za TB dat přidělené svazku. Tato míra může mít vliv cloudu toohello šířky pásma Internetu.</li><li>Hello obnovení nebo operaci klonování je dokončená, když všechny hello metadata jsou na zařízení hello.</li><li>Operace zálohování nelze provést, dokud nebude hello obnovení nebo je plně dokončení operace klonování. |
| Obnovení obnovit dobu místně vázaných svazků |< 2 minuty |<ul><li>svazek Hello je k dispozici během 2 minut hello operaci obnovení, bez ohledu na velikost svazku hello.</li><li>původně může být pomalejší než běžné jako většinu hello data a metadata se stále nachází v cloudu hello Hello svazku výkon. Jako toky dat ze zařízení StorSimple toohello hello cloudu může zvýšit výkon.</li><li>metadata toodownload celkový čas Hello závisí na hello přidělené velikost svazku. Metadata se automaticky přenesou do zařízení hello hello pozadí rychlostí hello 5 minut za TB dat přidělené svazku. Tato míra může mít vliv cloudu toohello šířky pásma Internetu.</li><li>Na rozdíl od vrstvené svazky, v případě hello místně vázaných svazků hello svazku dat je také stažen místně na hello zařízení. operace obnovení Hello je dokončena, pokud všechna data hello svazek znovu toohello zařízení.</li><li>operace obnovení Hello může trvat dlouho a hello celkový čas toocomplete hello obnovení bude záviset na velikosti hello místního svazku hello zřízený, šířky pásma Internetu a hello stávající data v zařízení hello. Operace zálohování na hello místně vázaný svazek jsou povoleny, když probíhá operace obnovení hello. |
| Dynamické obnovení dostupnosti |Poslední převzetí služeb při selhání | |
| Propustnost pro čtení a zápis maximálního počtu klientů (Pokud obsluhovat z vrstvy SSD hello) * |920/720 MB/s se jeden 10GbE síťové rozhraní |Až too2x pomocí funkce MPIO a dvě síťová rozhraní. |
| Propustnost pro čtení a zápis maximálního počtu klientů (Pokud obsluhovat z vrstvy HDD hello) * |120/250 MB/s | |
| Propustnost pro čtení a zápis maximálního počtu klientů (Pokud obsluhovat z vrstvy cloudu hello) * |11/41 MB/s |Propustnost čtení závisí na klientech generování a údržbu dostatečná hloubku fronty vstupně-výstupní operace. |

&#42; Maximální propustnost podle typu vstupně-výstupních operací se měří s 100 procent čtení a zápisu 100 procent scénáře. Skutečná propustnost může být nižší a závisí na vstupně-výstupních operací kombinovat a síťové podmínky.

