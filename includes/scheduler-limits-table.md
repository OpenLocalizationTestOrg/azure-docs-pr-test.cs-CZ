Hello následující tabulka popisuje každý hello hlavní kvót, omezení, výchozí hodnoty a omezení ve službě Azure Scheduler.

| Prostředek | Popis limit |
| --- | --- |
| **Velikost úlohy** |Maximální velikost je 16 kB. Pokud PUT nebo opravy výsledkem úlohu větší než těchto mezních hodnot, vrátí se 400 stavový kód chybný požadavek. |
| **Velikost adresy URL žádosti** |Maximální velikost adrese URL žádosti hello je 2048 znaků. |
| **Velikost agregační záhlaví** |Velikost maximální agregační záhlaví je 4096 znaků. |
| **Počet záhlaví** |Hlavička maximální počet je 50 hlavičky. |
| **Velikost obsahu** |Text maximální velikost je 8192 znaků. |
| **Interval opakování** |Maximální počet opakování rozpětí je 18 měsíců. |
| **Čas toostart čas** |Maximální počet "čas toostart čas" je 18 měsíců. |
| **Historie úlohy** |Maximální odpovědi, které jsou uloženy v historii úlohy je 2048 bajtů. |
| **Frekvence** |kvóta maximální frekvence Hello výchozí je 1 hodina bezplatnou kolekci úloh a 1 minuta v kolekci standardní úlohy. Maximální frekvence Hello se dá nakonfigurovat na toobe úlohy shromažďování, nižší než maximální hello. Všechny úlohy v kolekci úloh hello jsou omezené hello hodnotu nastavenou u kolekce úloh hello. Pokud se pokusíte toocreate úlohu s frekvencí vyšší než maximální frekvence hello kolekce úloh hello požadavek selže s 409 konflikt stavový kód. |
| **Úlohy** |kvóta maximální počet úloh výchozí Hello je 5 úlohy v bezplatnou kolekci úloh a 50 úloh v kolekci standardní úlohy. Hello maximální počet úloh, které je možné konfigurovat na kolekci úloh. Všechny úlohy v kolekci úloh hello jsou omezené hello hodnotu nastavenou u kolekce úloh hello. Pokud se pokusíte toocreate víc úloh než hello úlohy maximální kvóty a potom hello požadavek selže s 409 konflikt stavový kód. |
| **Kolekce úloh** |Maximální počet kolekce úloh podle předplatného je 200 000. |
| **Uchování historie úlohy** |Historie úlohy se zachovává kvůli až too2 měsíců nebo si toohello poslední spuštěních 1000. |
| **Uchování dokončené a chybný úlohy** |Dokončené a chybný úlohy jsou uchovávat 60 dnů. |
| **Časový limit** |Je statický časový limit (ne konfigurovat) požadavku 60 sekund pro akce HTTP. Delší spuštění operací postupujte podle asynchronní protokoly HTTP; například vrátit 202 okamžitě, ale pokračovat v práci v pozadí hello. |

