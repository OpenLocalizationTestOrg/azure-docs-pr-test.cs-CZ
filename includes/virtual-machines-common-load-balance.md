

Existují dvě úrovně služby Vyrovnávání zatížení pro služby infrastruktury Azure k dispozici:

* **Úroveň DNS**: Vyrovnávání zatížení pro provoz toodifferent cloudové služby umístěné v různých datových centrech, toodifferent weby Azure umístěné v různých datových střediscích nebo tooexternal koncové body. To se provádí pomocí nástroje Azure Traffic Manager a hello kruhové dotazování metoda Vyrovnávání zatížení.
* **Sítě úroveň**: příchozí internetové přenosy toodifferent virtuálních počítačů cloudové služby Vyrovnávání zatížení nebo provozu mezi virtuálními počítači v cloudové službě nebo virtuální sítě vyrovnávání zatížení. To lze provést pomocí nástroje pro vyrovnávání zatížení Azure hello.

## <a name="traffic-manager-load-balancing-for-cloud-services-and-websites"></a>Správce provozu Vyrovnávání zatížení pro služby cloud services a weby
Traffic Manager umožňuje distribuci hello toocontrol tooendpoints provoz uživatele, která může zahrnovat cloudové služby, weby, externí servery a jiné profily Traffic Manageru. Traffic Manager funguje tak, že použití inteligentních zásad modul tooDomain systému DNS (Name) dotazy na názvy domény hello prostředkům na Internetu. Cloudové služby nebo weby lze spustit v různých datových centrech napříč hello, world.

REST nebo prostředí Windows PowerShell tooconfigure externí koncové body ani profily Traffic Manageru musí používat jako koncové body.

Traffic Manager používá tři Vyrovnávání zatížení provozu toodistribute metody:

* **Převzetí služeb při selhání**: tuto metodu použijte, když chcete toouse primární koncový bod pro všechny přenosy, ale zadejte zálohování v případě, že primární hello nedostupný.
* **Výkon**: tuto metodu použijte, pokud máte koncové body v různých zeměpisných místech a chcete, aby požaduje "nejbližší" endpoint hello klientům toouse z hlediska hello nejnižší latenci.
* **Kruhové dotazování:** tuto metodu použijte, pokud chcete toodistribute zatížení mezi sadu cloud services v hello stejné datové centrum nebo přes cloud services nebo weby v různých datových centrech.

Další informace najdete v tématu [o provozu Manager zatížení vyrovnávání metody](../articles/traffic-manager/traffic-manager-routing-methods.md).

Hello následující diagram ukazuje příklad hello metoda Vyrovnávání zatížení s každým pro distribuci přenosů mezi jiné cloudové služby.

![Vyrovnávání zatížení](./media/virtual-machines-common-load-balance/TMSummary.png)

Hello základní proces je hello následující:

1. Internetový klient se dotazuje domény název odpovídající tooa webové služby.
2. DNS předává hello název dotazu požadavku tooTraffic správce.
3. Traffic Manager vybere hello další cloudové služby v hello seznamu kruhové dotazování a odesílá zpět hello název DNS. server DNS Hello internetového klienta přeloží hello název tooan IP adresu a odešle ho toohello internetovým klientem.
4. Hello internetový klient připojí s cloudovou službou hello vybrali Traffic Managerem.

Další informace najdete v tématu [Traffic Manager](../articles/traffic-manager/traffic-manager-overview.md).

## <a name="azure-load-balancing-for-virtual-machines"></a>Azure Vyrovnávání zatížení pro virtuální počítače
Hello virtuální počítače ve stejné cloudové služby nebo virtuální sítě může komunikovat s navzájem přímo pomocí jejich soukromé IP adresy. Počítače a služby mimo hello cloudovou službu nebo virtuální sítě může komunikovat pouze s virtuálními počítači v cloudové službě nebo virtuální sítě s nakonfigurovanou koncový bod. Koncový bod je mapování veřejnou IP adresu a port toothat privátní IP adresy a portu virtuálního počítače nebo webovou roli v rámci cloudové služby Azure.

Hello Vyrovnávání zatížení Azure náhodně distribuuje určitý typ příchozí provoz mezi více virtuálních počítačů nebo služby v konfiguraci se označuje jako sadu s vyrovnáváním zatížení. Můžete například šíří hello zátěž webové žádosti o provozu mezi více webové servery nebo webové role.

Hello následující diagram znázorňuje koncový bod Vyrovnávání zatížení sítě pro standardní (nezašifrované) webový provoz, která je sdílena mezi tři virtuální počítače pro hello veřejné a privátní port TCP 80. Tyto tři virtuální počítače jsou v sadě s vyrovnáváním zatížení.

![Vyrovnávání zatížení](./media/virtual-machines-common-load-balance/LoadBalancing.png)

Další informace najdete v tématu [nástroj pro vyrovnávání zatížení Azure](../articles/load-balancer/load-balancer-overview.md). Hello kroky toocreate sadu Vyrovnávání zatížení sítě, najdete v části [nakonfigurovat sadu s vyrovnáváním zatížení](../articles/load-balancer/load-balancer-get-started-internet-arm-ps.md).

Azure můžete také načíst vyrovnání v rámci cloudové služby nebo virtuální sítě. To se označuje jako interní zátěže a mohou být používány hello následující způsoby:

* tooload rovnováhu mezi servery v různých vrstev vícevrstvé aplikace (například mezi webové a databázová vrstva).
* aplikace (LOB)-obchodní vyrovnávání tooload hostované v Azure bez nutnosti další zátěž vyrovnávání hardwaru nebo softwaru.
* tooinclude místní servery v hello sadu počítačů, jejichž provoz se provede Vyrovnávání zatížení.

Podobně jako tooAzure Vyrovnávání zatížení, interní Vyrovnávání zatížení je usnadněno konfigurace interní sady vyrovnáváním zatížení.

Hello následující diagram ukazuje příklad interního Vyrovnávání zatížení koncového bodu pro řádek obchodní (LOB) aplikace, která je sdílena mezi tři virtuální počítače ve virtuální síti mezi různými místy.

![Vyrovnávání zatížení](./media/virtual-machines-common-load-balance/LOBServers.png)

## <a name="load-balancer-considerations"></a>Důležité informace o vyrovnávání zatížení
Nástroj pro vyrovnávání zatížení je ve výchozím nastavení nakonfigurované tootimeout na nečinnosti relace v 4 minuty. Pokud vaše aplikace za službou Vyrovnávání zatížení odejde připojení nečinnosti pro více než 4 minuty a nemá Keep-Alive konfigurace, hello připojení bude zrušeno. Můžete změnit tooallow chování nástroje pro vyrovnávání zatížení hello [delší časový limit nastavení nástroje pro vyrovnávání zatížení Azure](../articles/load-balancer/load-balancer-tcp-idle-timeout.md).

Další aspekt je typ hello režim distribuce podporovaný nástroj pro vyrovnávání zatížení Azure. Můžete nakonfigurovat spřažení IP zdroje (zdrojová adresa IP, cílovou IP adresu) nebo zdroj protokol IP (zdrojová adresa IP, cílovou IP adresu a protocol). Podívejte se na [režim distribuce pro vyrovnávání zatížení Azure (zdrojové IP spřažení)](../articles/load-balancer/load-balancer-distribution-mode.md) Další informace.

## <a name="next-steps"></a>Další kroky
Hello kroky toocreate sadu Vyrovnávání zatížení sítě, najdete v části [konfigurace interní sady vyrovnáváním zatížení](../articles/load-balancer/load-balancer-get-started-ilb-arm-ps.md).

Další informace o vyrovnávání zatížení najdete v tématu [Vyrovnávání zatížení pro vnitřní](../articles/load-balancer/load-balancer-internal-overview.md).

