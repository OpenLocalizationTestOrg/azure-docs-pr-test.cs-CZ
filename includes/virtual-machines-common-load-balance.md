

Existují dvě úrovně služby Vyrovnávání zatížení pro služby infrastruktury Azure k dispozici:

* **Úroveň DNS**: Vyrovnávání zatížení pro provoz na jiné cloudové služby umístěné v různých datových centrech na různé weby Azure umístěné v různých datových střediscích, nebo o externí koncové body. To lze provést pomocí Azure Traffic Manager a metodu Vyrovnávání zatížení, kruhové dotazování.
* **Sítě úroveň**: příchozí přenosy z Internetu pro různé virtuální počítače cloudové služby Vyrovnávání zatížení nebo provozu mezi virtuálními počítači v cloudové službě nebo virtuální sítě vyrovnávání zatížení. To lze provést pomocí nástroje pro vyrovnávání zatížení Azure.

## <a name="traffic-manager-load-balancing-for-cloud-services-and-websites"></a>Správce provozu Vyrovnávání zatížení pro služby cloud services a weby
Traffic Manager umožňuje řízení distribuce provozu generovaného uživateli na koncové body, které může zahrnovat cloudové služby, weby, externí servery a jiné profily Traffic Manageru. Traffic Manager funguje tak, že použití modul inteligentních zásad na systému DNS (Domain Name) dotazy na názvy domény internetovým prostředkům. Cloudové služby nebo weby lze spustit v různých datových centrech po celém světě.

REST nebo prostředí Windows PowerShell musíte použít ke konfiguraci externí koncové body ani profily Traffic Manageru jako koncové body.

Traffic Manager používá tři metody vyrovnávání zatížení pro distribuci přenosů:

* **Převzetí služeb při selhání**: tuto metodu použijte, pokud chcete použít primární koncový bod pro všechny přenosy, ale zadejte zálohování v případě, že primární nedostupný.
* **Výkon**: tuto metodu použijte, pokud máte koncové body v různých zeměpisných místech a chcete žádajícím klientům používat "nejbližší" koncového bodu z hlediska nejnižší latenci.
* **Kruhové dotazování:** tuto metodu použijte, pokud chcete rozdělovat mezi sadu cloudové služby ve stejném datovém centru nebo přes cloud services nebo weby v různých datových centrech.

Další informace najdete v tématu [o provozu Manager zatížení vyrovnávání metody](../articles/traffic-manager/traffic-manager-routing-methods.md).

Následující diagram ukazuje příklad metodu pro distribuci přenosů mezi jiné cloudové služby pro vyrovnávání zatížení kruhové dotazování.

![Vyrovnávání zatížení](./media/virtual-machines-common-load-balance/TMSummary.png)

Základní proces je následující:

1. Internetový klient se dotazuje název domény odpovídající k webové službě.
2. DNS předává dotaz požadavek název do Traffic Manageru.
3. Traffic Manager vybere další cloudové služby v seznamu kruhové dotazování a odesílá zpět název DNS. Server DNS klienta Internet překládá název na adresu IP a odešle ji do klientů v síti Internet.
4. Klienta Internet připojí ke službě cloud vybrali Traffic Managerem.

Další informace najdete v tématu [Traffic Manager](../articles/traffic-manager/traffic-manager-overview.md).

## <a name="azure-load-balancing-for-virtual-machines"></a>Azure Vyrovnávání zatížení pro virtuální počítače
Virtuální počítače ve stejné cloudové služby nebo virtuální sítě můžou navzájem komunikují, přímo pomocí jejich privátní IP adresy. Počítače a služby mimo cloudovou službu nebo virtuální sítě mohou komunikovat pouze s virtuálními počítači v cloudové službě nebo virtuální sítě s nakonfigurovanou koncový bod. Koncový bod je mapování veřejnou IP adresu a port pro tuto adresu a port virtuálního počítače nebo webovou roli v rámci cloudové služby Azure.

Vyrovnávání zatížení Azure náhodně distribuuje určitý typ příchozí provoz mezi více virtuálních počítačů nebo služby v konfiguraci se označuje jako sadu s vyrovnáváním zatížení. Můžete například šíří zatížení webový požadavek provoz napříč více webové servery nebo webové role.

Následující diagram znázorňuje koncový bod Vyrovnávání zatížení sítě pro standardní (nezašifrované) webový provoz, která je sdílena mezi tři virtuální počítače pro veřejné a privátní port TCP 80. Tyto tři virtuální počítače jsou v sadě s vyrovnáváním zatížení.

![Vyrovnávání zatížení](./media/virtual-machines-common-load-balance/LoadBalancing.png)

Další informace najdete v tématu [nástroj pro vyrovnávání zatížení Azure](../articles/load-balancer/load-balancer-overview.md). Postup vytvoření sady Vyrovnávání zatížení sítě najdete v tématu [nakonfigurovat sadu s vyrovnáváním zatížení](../articles/load-balancer/load-balancer-get-started-internet-arm-ps.md).

Azure můžete také načíst vyrovnání v rámci cloudové služby nebo virtuální sítě. To se označuje jako interní zátěže a mohou být používány následujícími způsoby:

* Chcete-li načíst rovnováhu mezi servery v různých vrstev vícevrstvé aplikace (například mezi webové a databázová vrstva).
* Načíst vyrovnávání-obchodní (LOB) aplikace hostované v Azure bez nutnosti další zátěž vyrovnávání hardwaru nebo softwaru.
* Chcete-li zahrnout do více počítačů, jejichž provoz se zatížení na místní servery s vyrovnáváním.

Podobně jako zatížení Azure vyrovnávání, interní Vyrovnávání zatížení je usnadněno konfigurace interní sady vyrovnáváním zatížení.

Následující diagram ukazuje příklad interního Vyrovnávání zatížení koncového bodu pro řádek obchodní (LOB) aplikace, která je sdílena mezi tři virtuální počítače ve virtuální síti mezi různými místy.

![Vyrovnávání zatížení](./media/virtual-machines-common-load-balance/LOBServers.png)

## <a name="load-balancer-considerations"></a>Důležité informace o vyrovnávání zatížení
Nástroj pro vyrovnávání zatížení je nakonfigurované ve výchozím nastavení časového limitu na nečinnosti relace v 4 minuty. Pokud vaše aplikace za službou Vyrovnávání zatížení odejde připojení nečinnosti pro více než 4 minuty a nemá Keep-Alive konfiguraci, připojení se zahodí. Můžete změnit chování nástroje pro vyrovnávání zatížení povolit [delší časový limit nastavení nástroje pro vyrovnávání zatížení Azure](../articles/load-balancer/load-balancer-tcp-idle-timeout.md).

Další aspekt je typ režim distribuce podporovaný nástroj pro vyrovnávání zatížení Azure. Můžete nakonfigurovat spřažení IP zdroje (zdrojová adresa IP, cílovou IP adresu) nebo zdroj protokol IP (zdrojová adresa IP, cílovou IP adresu a protocol). Podívejte se na [režim distribuce pro vyrovnávání zatížení Azure (zdrojové IP spřažení)](../articles/load-balancer/load-balancer-distribution-mode.md) Další informace.

## <a name="next-steps"></a>Další kroky
Postup vytvoření sady Vyrovnávání zatížení sítě najdete v tématu [konfigurace interní sady vyrovnáváním zatížení](../articles/load-balancer/load-balancer-get-started-ilb-arm-ps.md).

Další informace o vyrovnávání zatížení najdete v tématu [Vyrovnávání zatížení pro vnitřní](../articles/load-balancer/load-balancer-internal-overview.md).

