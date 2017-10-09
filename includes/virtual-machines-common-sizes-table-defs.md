
## <a name="size-table-definitions"></a>Definice tabulky velikostí

- Kapacita úložiště je v jednotkách GiB, tj. 1024^3 bajtů. Při porovnávání disků měřená v GB (1000 ^ 3 bajtů) toodisks měřená v GiB (1024 ^ 3) mějte na paměti, že kapacita čísel na základě předané v GiB může zobrazit menší. Například 1023 GiB = 1098,4 GB
- Propustnost disku se měří v počtu V/V operací za sekundu (IOPS) a v MB/s, kde 1 MB/s = 10^6 bajtů/s.
- Disky pro ukládání dat můžou fungovat v režimu s mezipamětí, nebo bez ní. Pro data uložená v mezipaměti operaci disku, hello hostitele mezipaměti režim je nastaven příliš**jen pro čtení** nebo **ReadWrite**.  Pro operace bez vyrovnávací paměti dat disku, hello hostitele mezipaměti režim je nastaven příliš**žádné**.
- **Očekávaný výkon sítě** je hello maximální agregované šířky pásma přidělený na typ virtuálního počítače na všech síťových adaptérů pro všechna místa určení. Horní limity se nezaručuje, že, ale jsou určený tooprovide pokyny pro výběr hello správný typ. virtuálních počítačů pro hello určená aplikace. Skutečný výkon sítě bude záviset na řadě faktorů, včetně zahlcení sítě, zatížení aplikací a nastavení sítě. Informace o optimalizaci propustnosti sítě najdete v tématu [Optimalizace propustnosti sítě pro Windows a Linux](../articles/virtual-network/virtual-network-optimize-network-bandwidth.md). očekávaný výkon sítě v systému Linux nebo Windows tooachieve hello, může být nutné tooselect na konkrétní verzi nebo optimalizovat virtuálního počítače. Další informace najdete v tématu [jak otestovat tooreliably pro virtuální počítač propustnost](../articles/virtual-network/virtual-network-bandwidth-testing.md).

- &#8224; 16 virtuálních procesorů výkonu konzistentně dosáhnou hello horní meze v nadcházející verzi.


