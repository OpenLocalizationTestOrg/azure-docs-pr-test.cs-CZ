
>[!NOTE]
>Pro Log Analytics se dřív používal název Operational Insights.
>
>

Hello následující omezení platí tooLog Analytics prostředky podle předplatného:

| Prostředek | Výchozí omezení | Komentáře
| --- | --- | --- |
| Počet bezplatných pracovních prostorů na předplatné | 10 | Toto omezení nejde zvýšit. |
| Počet placených pracovních prostorů na předplatné | Není k dispozici | Mají omezenou hello počet prostředků v rámci skupiny prostředků a počet skupin prostředků na předplatné | 


Hello následující omezení platí tooeach pracovní prostor analýzy protokolů:

|  | Free | Standard | Premium | Standalone | OMS |
| --- | --- | --- | --- | --- | --- |
| Objem shromážděných dat za den |500 MB<sup>1</sup> |Žádný |Žádný | Žádný | Žádný
| Doba uchování dat |7 dní |1 měsíc |12 měsíců | 1 měsíc<sup>2</sup> | 1 měsíc<sup>2</sup>|

<sup>1</sup> Pokud zákazníci dosáhnou své limit přenosu dat denní 500 MB, analýzu dat zastaví a obnoví v hello začátek hello další den. Den vychází ze standardu UTC.

<sup>2</sup> hello období uchovávání dat pro hello samostatné a OMS cenových plánů může být vyšší too730 dní.

| Kategorie | Omezení | Komentáře
| --- | --- | --- |
| Rozhraní API kolekce dat | Maximální velikost pro jeden příspěvek je 30 MB.<br>Maximální velikost pro hodnoty pole je 32 kB. | Rozdělte větší svazky na více příspěvků.<br>Pole delší než 32 kB se oříznou. |
| Search API | 5000 záznamů vrácených pro neagregovaná data<br>500000 záznamů vrácených pro agregovaná data | Agregovaná data je vyhledávání, která zahrnuje hello `measure` příkaz
 
