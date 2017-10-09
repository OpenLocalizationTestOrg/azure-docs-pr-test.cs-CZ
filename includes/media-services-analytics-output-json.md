Úloha Hello vytvoří výstupní soubor JSON, který obsahuje metadata o zjištěných a sledovaných strany. Hello metadat obsahuje souřadnice označující hello umístění řezy, a také vzhled ID číslo naznačuje hello sleduje to jednotlivých. Čísla ID vzhled jsou náchylné k chybám tooreset okolností při čelní vzhled hello dojde ke ztrátě nebo překryté hello rámce, výsledkem některé jednotlivce získávání přiřazeno více ID.

Hello výstup JSON zahrnuje hello následující atributy:

| Element | Popis |
| --- | --- |
| Verze |Vztahuje se toohello verzi hello Video rozhraní API. |
| Index | (Platí pouze tooAzure Redactor média) definuje index rámečku hello hello aktuální událost. |
| Časová osa |"Rysky" za sekundu hello videa. |
| Posun |Toto je posunutí hello čas pro časová razítka. Ve verzi 1.0 rozhraní API, Video bude vždy 0. V budoucích scénáře, které podporujeme, tato hodnota může změnit. |
| kmitočet snímků |Počet snímků za sekundu hello videa. |
| fragmenty |Hello metadata je blokové až do různých segmentů názvem fragmenty. Každý fragment obsahuje počáteční, doba trvání, číslo intervalu a událostí. |
| start |Hello počáteční čas hello první událost v 'rysky'. |
| Doba trvání |Délka Hello hello fragmentu v "rysky". |
| interval |interval Hello každé události položky v rámci hello fragment v "rysky". |
| stránka events |Každá událost obsahuje hello řezy zjištěn a sledují v rámci této doby trvání. Jde pole událostí. vnější pole Hello představuje jeden časový interval. vnitřní pole Hello se skládá z 0 nebo více událostí, které bylo provedeno v tomto bodě v čase. Prázdný závorky [] znamená, že nebyly zjištěny žádné řezy. |
| id |ID Hello hello řez, který je sledován. Toto číslo může nechtěně změnit, pokud se stane nezjištěné řez. Daný individuální by měl mít stejné ID v rámci celkového video hello hello, ale to nemůže zaručit kvůli toolimitations v algoritmus detekce hello (NF pásmová atd.) |
| x, y |vlevo nahoře Hello X a Y souřadnice ohraničujícího pole v normalizovaný škálování 0.0 too1.0 vzhled hello. <br/>-X a Y souřadnice jsou relativní toolandscape vždycky, takže pokud máte na výšku video (nebo obráceným v případě hello systému iOS), musíte tootranspose hello souřadnice. |
| šířky, výšky |Hello šířka a výška hello čelí ohraničující pole v normalizovaný škálování 0.0 too1.0. |
| facesDetected |To se nachází na konci hello hello výsledky JSON a shrnuje hello počet řezy tento algoritmus hello zjistil během hello videa. Protože hello ID můžete nechtěně obnovit, pokud se stane nezjištěné řez (např. řez bude mimo obrazovku, vypadá rychle aplikace), toto číslo nemusí vždy rovná hello true počet tyto řezy v hello video. |

