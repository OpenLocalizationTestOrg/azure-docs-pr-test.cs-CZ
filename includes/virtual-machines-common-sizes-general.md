
<!-- A-series, Av2-series, D-series, Dv2-series, DS-series*, DSv2-series* -->

- Hello A-series a Av2-series virtuálních počítačů můžete nasadit na různé typy hardwaru a procesory. velikost Hello je omezen na hello hardware, výkon toooffer konzistentní procesoru pro hello spuštěna instance, bez ohledu na to hello hardwaru, který je nasazen na základě. toodetermine hello fyzický hardware na kterém je nasazený této velikosti, dotaz hello virtuální hardware z v rámci hello virtuálního počítače.

- Virtuální počítače, D-series jsou navrženou toorun aplikace, které potřebují vyšší výpočetní výkon a výkon dočasné disku. Virtuální počítače D-series zadejte rychlejších procesorů vyšší poměr paměti pro virtuální procesory a na jednotku SSD (SSD) pro dočasným diskovým hello. Podrobnosti najdete v tématu hello oznámení na hello Azure blog [nové velikosti virtuálního počítače D-Series](https://azure.microsoft.com/blog/2014/09/22/new-d-series-virtual-machine-sizes/).

- Dv2-series, pokračovací toohello původní D-series, funkce výkonnější procesor. Hello Dv2-series procesoru je asi 35 % rychlejší než hello D-series procesoru. Je založena na hello nejnovější generace 2.4 v3® GHz Intel Xeon E5-2673 procesoru (Haswell) a s hello Intel Turbo nárůst technologie 2.0, můžete přejít do too3.1 GHz. má Hello Dv2-series hello stejné konfigurace paměti a disku jako hello D-series.

- Hello úroveň basic velikosti jsou především pro vývoj úlohy a dalších aplikací, které nevyžadují zatížení vyrovnávání, automatické škálování nebo náročné na paměť virtuálních počítačů. Informace o velikostech virtuálních počítačů vhodnějších pro produkční aplikace najdete v tématu (Velikosti virtuálních počítačů)[virtual-machines-size-specs.md] a informace o cenách virtuálních počítačů najdete v tématu [Ceny virtuálních počítačů](https://azure.microsoft.com/pricing/details/virtual-machines/).

## <a name="dsv3-series"></a>Dsv3-series

ACU: 160–190

Dsv3-series velikosti jsou založené na hello 2.3 v4® GHz Intel XEON E5-2673 procesoru (Broadwell) a mohou dosáhnout 3.5GHz s Intel Turbo nárůst technologie 2.0 a používat úložiště úrovně premium. Hello Dsv3-series velikosti nabízejí kombinaci virtuální procesory, paměť a dočasné úložiště pro většinu úlohy v produkčním prostředí.


| Velikost             | Virtuální procesory | Paměť: GiB | Dočasné úložiště (SSD): GiB | Max. datových disků | Maximální propustnost dočasného úložiště a úložiště v mezipaměti: IOPS / MB/s (velikost mezipaměti v GiB) | Maximální propustnost disku bez mezipaměti: IOPS / MB/s | Max. počet síťových karet / Očekávaný výkon sítě (Mb/s) |
|------------------|--------|-------------|----------------|----------------|-----------------------------------------------------------------------|-------------------------------------------|------------------------------------------------|
| Standard_D2s_v3  | 2      | 8           | 16             | 4              | 4 000 / 32 (50)                                                       | 3 200 / 48                                | 2 / střední                                   |
| Standard_D4s_v3  | 4      | 16          | 32             | 8              | 8 000 / 64 (100)                                                      | 6 400 / 96                                | 2 / střední                                   |
| Standard_D8s_v3  | 8      | 32          | 64             | 16             | 16 000 / 128 (200)                                                    | 12 800 / 192                              | 4 / vysoká                                       |
| Standard_D16s_v3 | 16     | 64          | 128            | 32             | 32 000 / 256 (400)                                                    | 25 600 / 384                              | 8 / vysoká                                       |


## <a name="dv3-series"></a>Dv3-series

ACU: 160–190

Dv3-series velikosti jsou založené na hello 2.3 v4® GHz Intel XEON E5-2673 procesoru (Broadwell) a můžete dosáhnout 3.5GHz s Intel Turbo nárůst technologie 2.0. Hello Dv3-series velikosti nabízejí kombinaci virtuální procesory, paměť a dočasné úložiště pro většinu úlohy v produkčním prostředí.

Úložiště datových disků se účtuje nezávisle na virtuálních počítačích. toouse prémiové disky úložiště, použijte hello Dsv3 velikosti. Hello ceny a fakturace měřidla Dsv3 velikostí hello jsou stejné jako Dv3-series. 


| Velikost            | Virtuální procesory | Paměť: GiB | Dočasné úložiště (SSD): GiB | Max. datových disků | Maximální propustnost dočasného úložiště: IOPS / čtení v MB/s / zápis v MB/s | Max. počet NIC / Šířka pásma sítě |
|-----------------|-----------|-------------|----------------|----------------|----------------------------------------------------------|------------------------------|
| Standard_D2_v3  | 2         | 8           | 50             | 4              | 3 000 / 46 / 23                                               | 2 / střední                 |
| Standard_D4_v3  | 4         | 16          | 100            | 8              | 6 000 / 93 / 46                                               | 2 / střední                 |
| Standard_D8_v3  | 8         | 32          | 200            | 16             | 12 000 / 187 / 93                                             | 4 / vysoká                     |
| Standard_D16_v3 | 16        | 64          | 400            | 32             | 24 000 / 375 / 187                                            | 8 / vysoká                     |


## <a name="dsv2-series"></a>DSv2-series

ACU: 210–250

| Velikost | Virtuální procesory | Paměť: GiB | Dočasné úložiště (SSD): GiB | Max. datových disků | Maximální propustnost dočasného úložiště a úložiště v mezipaměti: IOPS / MB/s (velikost mezipaměti v GiB) | Maximální propustnost disku bez mezipaměti: IOPS / MB/s | Max. počet síťových karet / Očekávaný výkon sítě (Mb/s) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS1_v2 |1 |3,5 |7 |2 |4 000 / 32 (43) |3 200 / 48 |2 / 750 |
| Standard_DS2_v2 |2 |7 |14 |4 |8 000 / 64 (86) |6 400 / 96 |2 / 1 500 |
| Standard_DS3_v2 |4 |14 |28 |8 |16 000 / 128 (172) |12 800 / 192 |4 / 3 000 |
| Standard_DS4_v2 |8 |28 |56 |16 |32 000 / 256 (344) |25 600 / 384 |8 / 6 000 |
| Standard_DS5_v2 |16 |56 |112 |32 |64 000 / 512 (688) |51 200 / 768 |8 / 6 000–12 000 &#8224;|



## <a name="dv2-series"></a>Dv2-series

ACU: 210–250

| Velikost              | Virtuální procesory | Paměť: GiB | Dočasné úložiště (SSD): GiB | Maximální propustnost dočasného úložiště: IOPS / čtení v MB/s / zápis v MB/s | Maximální propustnost datových disků: IOPS | Max. počet síťových karet / Očekávaný výkon sítě (Mb/s) |
|-------------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_D1_v2    | 1         | 3,5         | 50             | 3000 / 46 / 23                                           | 2 / 2×500                         | 2 / 750                 |
| Standard_D2_v2    | 2         | 7           | 100            | 6000 / 93 / 46                                           | 4 / 4×500                         | 2 / 1 500                     |
| Standard_D3_v2    | 4         | 14          | 200            | 12000 / 187 / 93                                         | 8 / 8×500                         | 4 / 3 000                     |
| Standard_D4_v2    | 8         | 28          | 400            | 24000 / 375 / 187                                        | 16 / 16×500                       | 8 / 6 000                     |
| Standard_D5_v2    | 16        | 56          | 800            | 48000 / 750 / 375                                        | 32 / 32×500                       | 8 / 6 000–12 000 &#8224;          |


<br>

## <a name="ds-series"></a>DS-series

ACU: 160

| Velikost | Virtuální procesory | Paměť: GiB | Dočasné úložiště (SSD): GiB | Max. datových disků | Maximální propustnost dočasného úložiště a úložiště v mezipaměti: IOPS / MB/s (velikost mezipaměti v GiB) | Maximální propustnost disku bez mezipaměti: IOPS / MB/s | Max. počet síťových karet / Očekávaný výkon sítě (Mb/s) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS1 |1 |3,5 |7 |2 |4 000 / 32 (43) |3 200 / 32 |2 / 500 |
| Standard_DS2 |2 |7 |14 |4 |8 000 / 64 (86) |6 400 / 64 |2 / 1 000 |
| Standard_DS3 |4 |14 |28 |8 |16 000 / 128 (172) |12 800 / 128 |4 / 2 000 |
| Standard_DS4 |8 |28 |56 |16 |32 000 / 256 (344) |25 600 / 256 |8 / 4 000 |

<br>

## <a name="d-series"></a>D-series 

ACU: 160

| Velikost         | Virtuální procesory | Paměť: GiB | Dočasné úložiště (SSD): GiB | Maximální propustnost dočasného úložiště: IOPS / čtení v MB/s / zápis v MB/s | Maximální propustnost datových disků: IOPS | Max. počet síťových karet / Očekávaný výkon sítě (Mb/s) |
|--------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_D1  | 1         | 3,5         | 50             | 3000 / 46 / 23                                           | 2 / 2×500                         | 2 / 500                 |
| Standard_D2  | 2         | 7           | 100            | 6000 / 93 / 46                                           | 4 / 4×500                         | 2 / 1 000                     |
| Standard_D3  | 4         | 14          | 200            | 12000 / 187 / 93                                         | 8 / 8×500                         | 4 / 2 000                     |
| Standard_D4  | 8         | 28          | 400            | 24000 / 375 / 187                                        | 16 / 16×500                       | 8 / 4 000                     |

<br>


## <a name="av2-series"></a>Av2-series

ACU: 100

| Velikost            | Virtuální procesory | Paměť: GiB | Dočasné úložiště (SSD): GiB | Maximální propustnost dočasného úložiště: IOPS / čtení v MB/s / zápis v MB/s | Maximální propustnost datových disků: IOPS | Max. počet síťových karet / Očekávaný výkon sítě (Mb/s) | 
|-----------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_A1_v2  | 1         | 2           | 10             | 1000 / 20 / 10                                           | 2 / 2×500               | 2 / 250                 |
| Standard_A2_v2  | 2         | 4           | 20             | 2000 / 40 / 20                                           | 4 / 4×500               | 2 / 500                 |
| Standard_A4_v2  | 4         | 8           | 40             | 4000 / 80 / 40                                           | 8 / 8×500               | 4 / 1 000                     |
| Standard_A8_v2  | 8         | 16          | 80             | 8000 / 160 / 80                                          | 16 / 16×500             | 8 / 2 000                     |
| Standard_A2m_v2 | 2         | 16          | 20             | 2000 / 40 / 20                                           | 4 / 4×500               | 2 / 500                 |
| Standard_A4m_v2 | 4         | 32          | 40             | 4000 / 80 / 40                                           | 8 / 8×500               | 4 / 1 000                     |
| Standard_A8m_v2 | 8         | 64          | 80             | 8000 / 160 / 80                                          | 16 / 16×500             | 8 / 2 000                     |

<br>

## <a name="a-series"></a>A-Series

ACU: 50–100

| Velikost | Virtuální procesory | Paměť: GiB | Dočasné úložiště (HDD): GiB | Max. datových disků | Maximální propustnost datového disku: IOPS | Max. počet síťových karet / Očekávaný výkon sítě (Mb/s)  |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_A0* |1 |0,768 |20 |1 |1×500 |2 / 100 |
| Standard_A1 |1 |1,75 |70 |2 |2×500 |2 / 500  |
| Standard_A2 |2 |3,5 |135 |4 |4×500 |2 / 500 |
| Standard_A3 |4 |7 |285 |8 |8×500 |2 / 1 000 |
| Standard_A4 |8 |14 |605 |16 |16×500 |4 / 2 000 |
| Standard_A5 |2 |14 |135 |4 |4×500 |2 / 500 |
| Standard_A6 |4 |28 |285 |8 |8×500 |2 / 1 000 |
| Standard_A7 |8 |56 |605 |16 |16×500 |4 / 2 000 |
<br>

* hello A0 velikost je povolená odebíraných na fyzickém hardwaru hello. Pro jenom tato konkrétní velikost jiné zákaznických nasazení může mít vliv na výkon hello spuštěné úlohy. relativní výkon Hello popsané níže jako základní hello očekávání, předmět tooan přibližnou variabilita 15 procent.

### <a name="standard-a0---a4-using-cli-and-powershell"></a>Velikosti Standard A0–A4 při použití rozhraní příkazového řádku a PowerShellu
V modelu nasazení classic hello se mírně liší v rozhraní příkazového řádku a prostředí PowerShell některé názvy velikost virtuálního počítače:

* Standard_A0 je ExtraSmall 
* Standard_A1 je Small
* Standard_A2 je Medium
* Standard_A3 je Large
* Standard_A4 je ExtraLarge

## <a name="basic-a"></a>Basic A

|Velikost – Velikost\Název | Virtuální procesory |Memory (Paměť)|Síťové karty (Max.)|Max. velikost dočasného disku |Max. počet datových disků (každý o velikosti 1 023 GB)|Max. IOPS (300 na disk)|
|---|---|---|---|---|---|---|
|A0\Basic_A0|1|768 MB|2| 20 GB|1|1×300|
|A1\Basic_A1|1|1,75 GB|2| 40 GB |2|2×300|
|A2\Basic_A2|2|3,5 GB|2| 60 GB|4|4×300|
|A3\Basic_A3|4|7 GB|2| 120 GB |8|8×300|
|A4\Basic_A4|8|14 GB|2| 240 GB |16|16×300|
