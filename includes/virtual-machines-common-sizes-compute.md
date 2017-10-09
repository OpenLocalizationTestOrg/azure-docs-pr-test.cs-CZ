<!-- F-series, Fs-series* -->

F-series je založena na hello 2.4 v3® GHz Intel Xeon E5-2673 procesoru (Haswell), který můžete dosáhnout hodiny rychlosti tak vysoké jako 3.1 GHz s hello Intel Turbo nárůst technologie 2.0. Toto je stejný hello výkonu procesoru jako hello Dv2 řadu virtuálních počítačů.  Za nižší cenu za hodinu seznamu hello F-series je nejlepší hodnota hello ceny výkonu v hello Azure portfolií podle hello Azure výpočetní jednotky (ACU) na jeden virtuální procesor. 

Virtuální počítače řady F-series jsou skvělou volbou pro úlohy, které potřebují rychlejší procesory, ale ne tolik paměti nebo dočasného úložiště na virtuální procesor.  Úlohy, jako například analýzy, herní servery, webové servery a dávkové zpracování výhod hello hodnotu hello F-series.

Hello Fs-series poskytuje všechny výhody hello hello F-series, v přidání tooPremium úložiště.

## <a name="fs-series"></a>Fs-series*

ACU: 210–250

| Velikost | Virtuální procesory | Paměť: GiB | Dočasné úložiště (SSD): GiB | Max. datových disků | Maximální propustnost dočasného úložiště a úložiště v mezipaměti: IOPS / MB/s (velikost mezipaměti v GiB) | Maximální propustnost disku bez mezipaměti: IOPS / MB/s | Max. počet síťových karet / Očekávaný výkon sítě (Mb/s) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_F1s |1 |2 |4 |2 |4 000 / 32 (12) |3 200 / 48 |2 / 750 |
| Standard_F2s |2 |4 |8 |4 |8 000 / 64 (24) |6 400 / 96 |2 / 1 500 |
| Standard_F4s |4 |8 |16 |8 |16 000 / 128 (48) |12 800 / 192 |4 / 3 000 |
| Standard_F8s |8 |16 |32 |16 |32 000 / 256 (96) |25 600 / 384 |8 / 6 000 |
| Standard_F16s |16 |32 |64 |32 |64 000 / 512 (192) |51 200 / 768 |8 / 6 000–12 000 &#8224; |

MB/s = 10^6 bajtů za sekundu a GiB = 1024^3 bajtů.

* hello maximální propustnost disku (IOPS nebo MB/s) možné pomocí řady Fs virtuálních počítačů může být omezena hello číslo, velikost a proložení hello připojené disky.  Podrobnosti viz článek [Storage úrovně Premium: Vysoce výkonné úložiště pro virtuální počítače Azure](../articles/storage/common/storage-premium-storage.md).


<br>

## <a name="f-series"></a>F-series

ACU: 210–250

| Velikost         | Virtuální procesory | Paměť: GiB | Dočasné úložiště (SSD): GiB | Maximální propustnost dočasného úložiště: IOPS / čtení v MB/s / zápis v MB/s | Maximální propustnost datových disků: IOPS | Max. počet síťových karet / Očekávaný výkon sítě (Mb/s) |
|--------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_F1  | 1         | 2           | 16             | 3000 / 46 / 23                                           | 2 / 2×500                         | 2 / 750                 |
| Standard_F2  | 2         | 4           | 32             | 6000 / 93 / 46                                           | 4 / 4×500                         | 2 / 1 500                     |
| Standard_F4  | 4         | 8           | 64             | 12000 / 187 / 93                                         | 8 / 8×500                         | 4 / 3 000                     |
| Standard_F8  | 8         | 16          | 128            | 24000 / 375 / 187                                        | 16 / 16×500                       | 8 / 6 000                     |
| Standard_F16 | 16        | 32          | 256            | 48000 / 750 / 375                                        | 32 / 32×500                       | 8 / 6 000–12 000 &#8224;           |


<br>


