Je důležité toorealize, že existují dva způsoby tooconfigure naslouchací proces skupiny dostupnosti v Azure. Hello způsoby, jak se liší v hello typ nástroje pro vyrovnávání zatížení Azure, kterou použijete při vytvoření naslouchacího procesu hello. Hello následující tabulka popisuje rozdíly hello:

| Typ nástroje pro vyrovnávání zatížení | Implementace | Použijte, když: |
| --- | --- | --- |
| **Externí** |Hello používá *veřejná virtuální IP adresa* hello cloudové služby, který je hostitelem hello virtuální počítače (VM). |Je nutné naslouchací proces hello tooaccess z hello mimo virtuální síť, včetně z hello Internetu. |
| **Interní** |Používá *nástroj pro vyrovnávání zatížení pro vnitřní* privátní adresou pro naslouchací proces hello. |Dostanete naslouchací proces hello pouze z v rámci hello stejné virtuální síti. Tento přístup zahrnuje site-to-site VPN v hybridní scénáře. |

> [!IMPORTANT]
> Pro naslouchací proces, hello používá hello Cloudová služba veřejné VIP (externím vyrovnáváním zatížení), stejně dlouho jako hello klienta, naslouchací proces a databáze jsou ve stejné oblasti Azure, nebudou vám narůstat poplatky za odchozí data. Žádná data, jinak hodnota vrátil prostřednictvím hello naslouchací proces se považuje za odchozí, a je účtovat normální přenosu dat tempem. 
> 
> 

ILB lze nastavit pouze na virtuální sítě s místní rozsah. Existující virtuální sítě, které byly nakonfigurovány pro skupiny vztahů nelze použít ILB. Další informace najdete v tématu [přehled nástroje pro vyrovnávání zatížení pro vnitřní](../articles/load-balancer/load-balancer-internal-overview.md).

