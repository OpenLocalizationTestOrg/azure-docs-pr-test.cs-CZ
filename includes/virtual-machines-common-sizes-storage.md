
Hello Ls-series je optimalizovaná pro úlohy, které vyžadují nízkou latencí dočasné úložiště, jako jsou databáze NoSQL, včetně Cloudera Cassandra, MongoDB a Redis. Hello Ls-series nabízí až too32 Vcpu, pomocí hello [procesor Intel Xeon® E5 v3 rodiny](http://www.intel.com/content/www/us/en/processors/xeon/xeon-e5-solutions.html). Získá Hello Ls-series hello stejného výkonu procesoru jako hello G nebo GS-Series a dodává se s 8 GiB paměti na jeden virtuální procesor.  

## <a name="ls-series"></a>Řada Ls

ACU: 180–240
 
| Velikost          | Virtuální procesory | Paměť: GiB | Dočasné úložiště (SSD): GiB | Max. datových disků | Maximální propustnost dočasného úložiště a úložiště v mezipaměti: IOPS / MB/s (velikost mezipaměti v GiB) | Maximální propustnost disku bez mezipaměti: IOPS / MB/s | Max. počet síťových karet / Očekávaný výkon sítě (Mb/s) | 
|---------------|-----------|-------------|--------------------------|----------------|-------------------------------------------------------------|-------------------------------------------|------------------------------| 
| Standard_L4s  | 4    | 32   | 678   | 8              | NA / NA (0)          | 5 000 / 125                               | 2 / 4 000       | 
| Standard_L8s  | 8    | 64   | 1 388 | 16             | NA / NA (0)          | 10 000 / 250                              | 4 / 8 000  | 
| Standard_L16s | 16   | 128  | 2 807 | 32             | NA / NA (0)          | 20 000 / 500                              | 8 / 6 000–16 000 &#8224; | 
| Standard_L32s* | 32 | 256  | 5,630 | 64             | NA / NA (0)          | 40 000 / 1 000                            | 8 / 20 000 | 
 

propustnost maximální disku Hello možné s virtuálními počítači Ls-series může být omezené hello počet, velikost a proložení všech připojenými disky. Podrobnosti viz článek [Storage úrovně Premium: Vysoce výkonné úložiště pro virtuální počítače Azure](../articles/storage/common/storage-premium-storage.md). 

* Instance je izolovaná toohardware vyhrazené tooa jednoho zákazníka.

