<!-- A-series - compute-intensive instances, H-series -->

Hello velikosti A8-A11 a H-series se také označují jako *náročné instance*. Hello hardwaru, který spouští tyto velikosti je navržena a optimalizována pro náročné a aplikace, modelování a simulace clusteru aplikace náročné na sítě, včetně vysoce výkonné výpočetní (HPC). Hello A8-A11 řady používá Intel Xeon E5-. 2670 @ 2.6 GHZ a hello H-series. Intel Xeon E5-2667 v3 @ 3,2 GHz. 

Virtuální počítače Azure H-series jsou hello další generace s vysokým výkonem, že virtuální počítače zaměřené na výpočetní potřeby vysoké end, jako molekulární modelování a výpočet dynamiky kapaliny. Tyto 8 až 16 virtuálních procesorů virtuální počítače jsou postavené na technologii procesoru hello Intel. Haswell E5-2667 V3 s funkcí DDR4 paměti a založená na SSD – dočasné úložiště. 

Kromě toho toohello významné výkon procesoru, hello H-series nabízí různé možnosti sítě s nízkou latencí RDMA pomocí FDR InfiniBand a několik paměti konfigurace toosupport náročné výpočetní požadavky na paměť.



## <a name="h-series"></a>H-series

ACU: 290–300

| Velikost | Virtuální procesory | Paměť: GiB | Dočasné úložiště (SSD): GiB | Max. datových disků | Maximální propustnost disku: IOPS | Maximální počet síťových karet |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_H8 |8 |56 |1000 |16 |16 × 500 |2  |
| Standard_H16 |16 |112 |2000 |32 |32 × 500 |4 |
| Standard_H8m |8 |112 |1000 |16 |16 × 500 |2  |
| Standard_H16m |16 |224 |2000 |32 |32 × 500 |4  |
| Standard_H16r* |16 |112 |2000 |32 |32 × 500 |4  |
| Standard_H16mr* |16 |224 |2000 |32 |32 × 500 |4 |

* Pro aplikace MPI umožňuje síť FDR InfiniBand vyhrazenou síť back-end s přímým přístupem do paměti vzdáleného počítače (RDMA), která poskytuje mimořádně nízkou latenci a velkou šířku pásma.

<br>



## <a name="a-series---compute-intensive-instances"></a>A-series – Instance náročné na výpočetní výkon

ACU: 225

| Velikost | Virtuální procesory | Paměť: GiB | Dočasné úložiště (HDD): GiB | Max. datových disků | Maximální propustnost datového disku: IOPS | Maximální počet síťových karet|
| --- | --- | --- | --- | --- | --- | --- |
| Standard_A8* |8 |56 |382 |16 |16×500 |2 |
| Standard_A9* |16 |112 |382 |16 |16×500 |4 |
| Standard_A10 |8 |56 |382 |16 |16×500 |2  |
| Standard_A11 |16 |112 |382 |16 |16×500 |4 |

* Pro aplikace MPI umožňuje síť FDR InfiniBand vyhrazenou síť back-end s přímým přístupem do paměti vzdáleného počítače (RDMA), která poskytuje mimořádně nízkou latenci a velkou šířku pásma.

<br>



