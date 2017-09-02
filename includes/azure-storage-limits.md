| Prostředek | Výchozí omezení |
| --- | --- |
| Počet účtů úložiště na předplatné |200<sup>1</sup> |
| Maximální kapacitě účtu úložiště |500 TB<sup>2</sup> |
| Maximální počet kontejnery objektů blob, objekty BLOB, sdílené soubory, tabulky, fronty, entity nebo zpráv na účet úložiště |Bez omezení |
| Maximální velikost kontejneru jediného objektu blob, tabulka nebo fronty |Stejné jako maximální kapacitou účtu |
| Maximální počet bloků v objekt blob bloku nebo přidávání objektů blob |50,000 |
| Maximální velikost bloku v objekt blob bloku |100 MB |
| Maximální velikost objektu blob bloku |50 000 × 100 MB (poli 4.75 TB) |
| Maximální velikost blok v doplňovacím objektu blob |4 MB VOLNÉHO MÍSTA |
| Maximální velikost doplňovacího objektu BLOB |50 000 × 4 MB (poli 195 GB) |
| Maximální velikost objektu blob stránky |8 TB |
| Maximální velikost tabulka entity |1 MB |
| Maximální počet vlastností v tabulce entity |252 |
| Maximální velikost zprávy ve frontě |64 kB |
| Maximální velikost sdílené složky |5 TB |
| Maximální velikost souboru ve sdílené složce |1 TB |
| Maximální počet souborů ve sdílené složce |Pouze limit je 5 TB celkové kapacity sdílené složky |
| Maximální IOPS za sdílené složky |1000 |
| Maximální počet souborů ve sdílené složce |Pouze limit je 5 TB celkové kapacity sdílené složky |
| Maximální počet zásad přístupu uložené na kontejner, sdílené složky, tabulku nebo frontu |5 |
| Maximální počet požadavků na účet úložiště |Objekty BLOB: 20 000 požadavků za sekundu<sup>2</sup> pro objekty BLOB libovolnou platné velikost (omezená jenom ve vstupní/výstupní limity účtu) <br />Soubory: 1000 IOPS (o velikosti 8 KB) na základě sdílené složky <br />Fronty: 20 000 zpráv za sekundu (velikost zprávy v případě 1 KB)<br />Tabulky: 20 000 transakcí za sekundu (v případě 1 KB entity velikost) |
| Cíl propustnost pro jediného objektu blob |Až 60 MB za sekundu, nebo až 500 požadavků za sekundu |
| Cíl propustnost pro jeden fronty (1 KB zpráv) |Až 2000 zpráv za sekundu |
| Cíl propustnost pro jednu tabulku oddílu (1 KB entity) |Až 2000 entity za sekundu |
| Cíl propustnost pro sdílení souborů |Až 60 MB za sekundu |
| Příjem příchozích dat Max<sup>3</sup> na účet úložiště (nám oblastech) |10 GB/s Pokud GRS nebo ZRS<sup>4</sup> povoleno, 20 GB/s pro LRS<sup>2</sup> |
| Maximální počet odchozí<sup>3</sup> na účet úložiště (nám oblastech) |20 GB/s Pokud RA-GRS nebo GRS nebo ZRS<sup>4</sup> povoleno, 30 GB/s pro LRS<sup>2</sup> |
| Příjem příchozích dat Max<sup>3</sup> na účet úložiště (oblastech mimo USA) |5 GB/s Pokud GRS nebo ZRS<sup>4</sup> povoleno, 10 GB/s pro LRS<sup>2</sup> |
| Maximální počet odchozí<sup>3</sup> na účet úložiště (oblastech mimo USA) |10 GB/s Pokud RA-GRS nebo GRS nebo ZRS<sup>4</sup> povoleno, 15 GB/s pro LRS<sup>2</sup> |

<sup>1</sup>to zahrnuje účty úložiště Standard a Premium. Pokud potřebujete více než 200 účtů úložiště, vytvořte žádost prostřednictvím [podpory Azure](https://azure.microsoft.com/support/faq/). Tým Azure Storage se na váš obchodní případ podívá a může schválit až 250 účtů úložiště. 

<sup>2</sup> získat účty úložiště standard roste s tím, za inzerovaný omezení kapacity, vstupní/výstupní a žádost o rychlost do, Zkontrolujte prosím žádost o prostřednictvím [podporu Azure](https://azure.microsoft.com/support/faq/). Tým služby Azure Storage bude zkontrolujte požadavek a může schválit vyšší limity případ od případu.

<sup>3</sup>*příjem příchozích dat* odkazuje na všech dat (počet požadavků) odesílány do účtu úložiště. *Odchozí přenos dat* odkazuje na všechna data (požadavky) přijímané z úložiště účtu.  

<sup>4</sup>možnosti replikace azure Storage patří:
* **RA-GRS**: geograficky redundantní úložiště s přístupem pro čtení. Pokud je povoleno RA-GRS, odchozí cíle pro sekundární umístění jsou stejné jako pro primární umístění.
* **GRS**: geograficky redundantní úložiště. 
* **ZRS**: Zónově redundantní úložiště. K dispozici pouze pro objekty BLOB bloku. 
* **LRS**: místně redundantní úložiště. 


