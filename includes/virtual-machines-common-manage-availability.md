## <a name="understand-vm-reboots---maintenance-vs-downtime"></a>Vysvětlení restartování virtuálních počítačů – údržba vs. výpadek
Existují tři scénáře, které můžou vést toovirtual počítače v Azure ovlivněný: neplánované hardwaru údržby, nečekané výpadky a plánované údržby.

* **Neplánované údržby události hardwaru** nastane, když hello platformy Azure předpovídá hello hardwaru nebo všechny platformy součásti související tooa fyzický počítač, je o toofail. Když hello platformy předpovídá selhání, vydá hardwaru neplánované údržby událostí tooreduce hello dopad toohello virtuální počítače hostované na hardwaru. Azure pomocí migrace za provozu technologie toomigrate hello virtuální počítače z hello selhání hardwaru tooa pořádku fyzického počítače. Migrace za provozu je virtuální počítač zachování operace, hello pouze pozastaví virtuální počítač po krátkou dobu. Paměť, otevřených souborů a připojení k síti se zachovají, ale před nebo po hello událostí může snížit výkon. V případech, kdy nejde použít migraci za provozu hello virtuální počítač bude mít nečekané výpadky, jak je popsáno níže.


* **Nečekané výpadky** zřídka nastane, když hello hardware nebo hello fyzické infrastruktuře základní virtuálního počítače došlo k chybě nějakým způsobem. To může zahrnovat selhání místní sítě, selhání místního disku nebo další selhání na úrovni racku. Když se taková selhání detekuje, hello platformy Azure automaticky migruje (heals) váš virtuální počítač tooa pořádku fyzického počítače. Během opravy postup hello, virtuální počítače, plánovaná odstávka (restartování) a v některých případech ztrátě hello dočasné jednotce. Hello připojena operačního systému a datové disky se vždycky zachovají. 

* **Plánované údržby události** jsou pravidelné aktualizace od společnosti Microsoft toohello základní platformu Azure tooimprove celkové spolehlivosti, výkonu a zabezpečení infrastruktury hello platformy, které virtuální počítače spustit na. U většiny těchto aktualizací nemá jejich provedení žádný vliv na vaše služby Virtual Machines ani Cloud Services (viz [Údržba se zachováním virtuálních počítačů](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/preserving-maintenance)). Hello platformy Azure pokusů toouse zachování údržby virtuálního počítače všechny možné příležitosti, existují výjimečných případech, kdy tyto aktualizace vyžadují restartování virtuálního počítače tooapply hello požadované aktualizace toohello základní infrastrukturu. V takovém případě můžete provést plánované údržby Azure s operací údržby-znovu ho zaveďte pomocí inicializace hello údržby pro jejich virtuální počítače hello vhodné časové okno. Další informace najdete v tématu [Plánovaná údržba pro virtuální počítače](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/planned-maintenance/).


dopad hello tooreduce výpadek kvůli tooone nebo více z těchto událostí, doporučujeme následující osvědčené postupy vysoké dostupnosti pro virtuální počítače hello:

* [Konfigurace více virtuálních počítačů ve skupině dostupnosti pro zajištění redundance]
* [Použití spravovaných disků pro virtuální počítače ve skupině dostupnosti]
* [Používat naplánované události tooproactively odpovědi tooVM události, které mají vliv] (https://docs.microsoft.com/en-us/azure/virtual-machines/virtual-machines-scheduled-events)
* [Konfigurace jednotlivých vrstev aplikace v samostatných skupinách dostupnosti]
* [Kombinace nástroje pro vyrovnávání zatížení se skupinami dostupnosti]

## <a name="configure-multiple-virtual-machines-in-an-availability-set-for-redundancy"></a>Konfigurace více virtuálních počítačů ve skupině dostupnosti pro zajištění redundance
aplikace tooyour tooprovide redundanci, doporučujeme seskupit dva nebo více virtuálních počítačů v nastavení dostupnosti. Tato konfigurace zajistí, že během buď plánované i neplánované údržby, je k dispozici alespoň jeden virtuální počítač a splňuje hello 99,95 % smlouva Azure SLA. Další informace najdete v tématu hello [SLA pro virtuální počítače](https://azure.microsoft.com/support/legal/sla/virtual-machines/).

> [!IMPORTANT]
> Nenechávejte ve skupině dostupnosti samostatné instance virtuálních počítačů. Na virtuální počítače v takové konfiguraci se nevztahuje garance SLA a během plánovaných událostí údržby čelí výpadkům, kromě případu, kdy samostatný virtuální počítač využívá službu [Storage úrovně Premium](../articles/storage/common/storage-premium-storage.md). Pro jeden virtuální počítače pomocí storage úrovně premium platí hello smlouva Azure SLA.

Každý virtuální počítač ve vaší sadě dostupnosti je přiřazena **aktualizace domény** a **doména selhání** podle hello základní platformy Azure. Pro sadu dostupnosti daného pět domény bez uživatelsky konfigurovatelného aktualizace přiřazené ve výchozím nastavení (nasazení Resource Manager pak může být vyšší tooprovide až too20 aktualizace domén) tooindicate skupiny virtuálních počítačů a základní fyzický hardware může být restartován v hello stejnou dobu. Při více než pět virtuální počítače jsou konfigurované v rámci jedné dostupnost sady, hello šesté virtuální počítač je umístěn do hello stejné aktualizace domény jako hello první virtuální počítač hello sedmá v hello stejné jako hello druhý virtuální počítač a aktualizace domény na. restartuje se Hello pořadí domén aktualizace, která se resetuje nemusí pokračovat postupně během plánované údržby, ale pouze jednu aktualizační doménu najednou. Restartovaný aktualizace domény je dostane toorecover 30 minut, než údržby se zahájí na jinou aktualizaci domény.

Domén selhání definovat hello skupinu virtuálních počítačů, které sdílejí společné přepínač zdroje a sítě power. Ve výchozím nastavení hello virtuální počítače nakonfigurované v rámci vaší sady dostupnosti jsou oddělené v rámci až toothree domén selhání pro nasazení Resource Manager (dva poruch domén pro Classic). Při umístění virtuálních počítačů do skupiny dostupnosti nechrání aplikace z operačního systému nebo selhání specifické pro aplikaci, omezit hello dopad potenciální selhání fyzického hardwaru, výpadků sítě nebo přerušení napájení.

<!--Image reference-->
   ![Koncepční kreslení hello aktualizace domény a chyby konfigurace domény](./media/virtual-machines-common-manage-availability/ud-fd-configuration.png)

## <a name="use-managed-disks-for-vms-in-an-availability-set"></a>Použití spravovaných disků pro virtuální počítače ve skupině dostupnosti
Pokud aktuálně používáte virtuální počítače s nespravované disky, důrazně doporučujeme vám [převést virtuální počítače ve skupině dostupnosti disky spravované toouse](../articles/virtual-machines/windows/convert-unmanaged-to-managed-disks.md).

[Spravované disky](../articles/virtual-machines/windows/managed-disks-overview.md) poskytuje vyšší spolehlivost pro skupiny dostupnosti zajištěním, které jsou hello disky virtuálních počítačů ve skupině dostupnosti dostatečně navzájem izolované tooavoid jediný bod selhání. Dělá to tak, že automaticky hello disky clustery jiného úložiště. Pokud cluster úložiště nezdaří z důvodu selhání toohardware nebo softwaru, hello instancí virtuálního počítače jenom s disky na těchto razítka se nezdaří.

![Domény selhání spravovaných disků](./media/virtual-machines-common-manage-availability/md-fd.png)

> [!IMPORTANT]
> Hello počet domén selhání pro skupiny dostupnosti spravované se liší podle oblastí – dva nebo tři každou oblast. Hello následující tabulka uvádí počet hello podle oblastí

[!INCLUDE [managed-disks-common-fault-domain-region-list](managed-disks-common-fault-domain-region-list.md)]

Pokud máte v plánu toouse virtuálních počítačů s [nespravované disky](../articles/virtual-machines/windows/about-disks-and-vhds.md#types-of-disks), postupujte podle níže osvědčené postupy pro účty úložiště, kde jsou uloženy virtuální pevné disky (VHD) virtuálních počítačů jako [stránek](https://docs.microsoft.com/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs#about-page-blobs).

1. **Zachovat všechny disky (operačního systému a data), které jsou spojené s virtuálním Počítačem v hello stejný účet úložiště**
2. **Zkontrolujte hello [omezení](../articles/storage/common/storage-scalability-targets.md) hello počtu nespravované disky v účtu úložiště** před přidáním další účet úložiště tooa virtuálních pevných disků
3. **Pro každý virtuální počítač ve skupině dostupnosti použijte samostatný účet úložiště.** Nesdílejte účty úložiště s několika virtuálními počítači v hello stejné skupiny dostupnosti. Je přijatelné pro virtuální počítače napříč různými účty úložiště tooshare skupiny dostupnosti Pokud výše doporučených postupů dodržíte

## <a name="configure-each-application-tier-into-separate-availability-sets"></a>Konfigurace jednotlivých vrstev aplikace v samostatných skupinách dostupnosti
Pokud vaše virtuální počítače jsou téměř všechny identické a sloužit hello stejný účel pro vaši aplikaci, doporučujeme nakonfigurovat sadu dostupnosti pro každou vrstvu vaší aplikace.  Zadáte-li dva různé úrovně v hello stejné skupině dostupnosti, všechny virtuální počítače ve stejné vrstvě aplikace můžete restartovat najednou hello. Nakonfigurováním alespoň dvou virtuálních počítačů ve skupině dostupnosti pro každou vrstvu můžete zajistit, že v každé skupině dostupnosti bude k dispozici alespoň jeden virtuální počítač.

Například by mohlo všechny virtuální počítače hello ve front-endu vaší aplikace s IIS, Apache, Nginx v sadě dostupnosti jeden hello. Ujistěte se, že pouze klientské virtuální počítače jsou umístěny v hello stejné skupiny dostupnosti. Podobně se ujistěte, že jsou ve vlastní skupině dostupnosti umístěné pouze virtuální počítače datové vrstvy, jako jsou například replikované virtuální počítače s SQL Serverem nebo virtuální počítače se serverem MySQL.

<!--Image reference-->
   ![Úrovně aplikace](./media/virtual-machines-common-manage-availability/application-tiers.png)

## <a name="combine-a-load-balancer-with-availability-sets"></a>Kombinace nástroje pro vyrovnávání zatížení se skupinami dostupnosti
Kombinování hello [nástroj pro vyrovnávání zatížení Azure](../articles/load-balancer/load-balancer-overview.md) s dostupnosti nastavit tooget hello většina odolnost aplikace. Hello nástroj pro vyrovnávání zatížení Azure rozděluje zatížení mezi více virtuálními počítači. Pro naše úrovně Standard virtuální počítače je zahrnuta hello nástroj pro vyrovnávání zatížení Azure. Ne všechny vrstvy virtuálního počítače zahrnují hello nástroj pro vyrovnávání zatížení Azure. Další informace o vyrovnávání zatížení virtuálních počítačů najdete v článku [Vyrovnávání zatížení virtuálních počítačů](../articles/virtual-machines/virtual-machines-linux-load-balance.md).

Pokud není nástroj pro vyrovnávání zatížení hello nakonfigurovat toobalance provoz napříč více virtuálních počítačů, pak všechny události plánované údržby ovlivňuje hello pouze provoz slouží virtuálního počítače způsobuje výpadek tooyour aplikační vrstvy. Umístění více virtuálních počítačů hello stejné vrstvy v rámci stejné zatížení vyrovnávání a dostupnost sady povoluje provoz toobe nepřetržitě obsloužených alespoň jedna instance hello.


<!-- Link references -->
[Konfigurace více virtuálních počítačů ve skupině dostupnosti pro zajištění redundance]: #configure-multiple-virtual-machines-in-an-availability-set-for-redundancy
[Konfigurace jednotlivých vrstev aplikace v samostatných skupinách dostupnosti]: #configure-each-application-tier-into-separate-availability-sets
[Kombinace nástroje pro vyrovnávání zatížení se skupinami dostupnosti]: #combine-a-load-balancer-with-availability-sets
[Avoid single instance virtual machines in availability sets]: #avoid-single-instance-virtual-machines-in-availability-sets
[Použití spravovaných disků pro virtuální počítače ve skupině dostupnosti]: #use-managed-disks-for-vms-in-an-availability-set
