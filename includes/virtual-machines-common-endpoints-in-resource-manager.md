Koncové body tooAzure Hello přístup funguje trochu jinak mezi modely nasazení Classic a Resource Manager hello. V modelu nasazení Resource Manager hello nyní máte hello flexibilitu toocreate sítě filtry, které řídí hello tok provozu do a z virtuálních počítačů. Tyto filtry vám umožňují toocreate komplexní prostředích sítí nad rámec koncový bod jednoduché jako model nasazení Classic hello. Tento článek obsahuje přehled skupin zabezpečení sítě a jak se liší od pomocí koncových bodů Classic, vytváření těchto pravidla, filtrování a ukázkové scénáře nasazení.

## <a name="overview-of-resource-manager-deployments"></a>Přehled nasazení Resource Manager
Koncové body v modelu nasazení Classic hello se nahrazují skupin zabezpečení sítě a pravidla seznamu ACL řízení přístupu. Rychlé kroky pro implementaci pravidla seznamu ACL skupiny zabezpečení sítě jsou:

* Vytvořte skupinu zabezpečení sítě.
* tooallow nebo zakážou provoz, definování pravidel seznamu ACL skupiny zabezpečení sítě.
* Přiřadíte síťové rozhraní tooa skupinu zabezpečení sítě nebo podsítě virtuální sítě.

Pokud jsou podmínka mimo tooalso provádět přesměrování portů, potřebujete tooplace nástroj pro vyrovnávání zatížení před virtuálního počítače a použít pravidla NAT. Rychlé kroky pro implementaci služby Vyrovnávání zatížení a pravidla NAT vypadat takto:

* Vytvořte nástroj pro vyrovnávání zatížení.
* Vytvořte fond back-end a přidejte toohello fondu virtuálních počítačů.
* Definování pravidel NAT pro přesměrování portu hello vyžaduje.
* Přiřaďte pravidel NAT tooyour virtuálních počítačů.

## <a name="network-security-group-overview"></a>Přehled skupinu zabezpečení sítě
Skupiny zabezpečení sítě zadejte úroveň zabezpečení pro vás určité porty tooallow a tooaccess podsítě virtuálních počítačů. Obvykle vždy máte skupinu zabezpečení sítě poskytuje tuto vrstvu zabezpečení mezi virtuálními počítači a hello mimo world. Skupiny zabezpečení sítě můžou být použité tooa podsíť virtuální sítě nebo konkrétním síťovém rozhraní pro virtuální počítač. Místo vytvoření koncového bodu pravidla seznamu ACL, je nyní vytvořit pravidla seznamu ACL skupiny zabezpečení sítě. Tato pravidla seznamu ACL poskytují mnohem větší kontrolu než jednoduše vytváření tooforward koncový bod zadaný port. Další informace najdete v tématu [co je skupina zabezpečení sítě?](../articles/virtual-network/virtual-networks-nsg.md)

> [!TIP]
> Můžete přiřadit podsítě toomultiple skupin zabezpečení sítě nebo síťová rozhraní. Neexistuje žádné mapování 1:1. Můžete vytvořit skupinu zabezpečení sítě s společnou sadu pravidel seznamu ACL a použít toomultiple podsítě nebo síťová rozhraní. Navíc může být použité tooresources napříč vaše předplatné skupinu zabezpečení sítě (na základě [řízení přístupu na základě Role](../articles/active-directory/role-based-access-control-what-is.md).

## <a name="load-balancers-overview"></a>Přehled nástroje pro vyrovnávání zatížení
V modelu nasazení Classic hello by Azure provést všechny hello překlad adres (NAT) a portu přesměrovávání na cloudové služby za vás. Při vytváření koncový bod, zadali byste hello externí port tooexpose společně s přenosy toodirect interní port hello. Skupiny zabezpečení sítě samy o sobě Neprovádět tento stejný NAT a přesměrování portu. 

tooallow toocreate pravidla NAT pro takové port předávání, vytvoříte k nástroji pro vyrovnávání zatížení Azure ve vaší skupině prostředků. Znovu, je nástroj pro vyrovnávání zatížení hello granulární dostatek tooonly použít toospecific virtuální počítače v případě potřeby. Hello Azure načíst pracovní pravidla NAT nástroje pro vyrovnávání spolu s tooprovide pravidla seznamu ACL skupiny zabezpečení sítě mnohem větší flexibilitu a řízení než bylo dosažitelný pomocí koncové body cloudové služby. Další informace najdete v tématu hello [přehled nástroje pro vyrovnávání zatížení](../articles/load-balancer/load-balancer-overview.md).

## <a name="network-security-group-acl-rules"></a>Pravidla seznamu ACL skupinu zabezpečení sítě
Pravidla seznamu ACL umožňují definovat, jaké přenos můžete do aplikace a z virtuálního počítače založené na určité porty a rozsahy portů a protokolů. Pravidla jsou přiřazeny tooindividual virtuálních počítačů nebo tooa podsíť. Hello následující snímek obrazovky je příklad pravidla seznamu ACL pro běžné webový server:

![Pravidla seznamu ACL seznamu skupiny zabezpečení sítě](./media/virtual-machines-common-endpoints-in-resource-manager/example-acl-rules.png)

Pravidla seznamu ACL platí podle priority metriky který určíte - hello vyšší hello hodnotu, hello s nižší prioritou hello. Každá skupina zabezpečení sítě má tři výchozí pravidla, která jsou určená toohandle hello toku Azure síťových přenosů dat, s explicitního `DenyAllInbound` jako poslední pravidlo hello. Výchozí pravidla seznamu ACL jsou dané toonot s nízkou prioritou narušovat pravidla, které vytvoříte.

## <a name="assigning-network-security-groups"></a>Přiřazení skupiny zabezpečení sítě
Můžete přiřadit podsítě tooa skupinu zabezpečení sítě nebo síťové rozhraní. Tento přístup vám umožní toobe granulární, podle potřeby při použití vašeho seznamu ACL pravidla tooonly konkrétní virtuální počítač nebo k zajištění, společnou sadu pravidel seznamu ACL jsou použité tooall virtuální počítače nepatří do podsítě:

![Použít skupiny Nsg toonetwork rozhraní nebo podsítě](./media/virtual-machines-common-endpoints-in-resource-manager/apply-nsg-to-resources.png)

chování Hello hello skupinu zabezpečení sítě nemění v závislosti na právě přiřazován tooa podsíť nebo síťové rozhraní. Běžný scénář nasazení má hello skupinu zabezpečení sítě přiřadit tooa podsíť tooensure dodržování předpisů všechny připojené toothat podsíť virtuálních počítačů. Další informace najdete v tématu [použití zabezpečení sítě skupiny tooresources](../articles/virtual-network/virtual-networks-nsg.md#associating-nsgs).

## <a name="default-behavior-of-network-security-groups"></a>Výchozí chování skupiny zabezpečení sítě
V závislosti na tom, jak a kdy vytvoříte skupinu zabezpečení vaší sítě je možné vytvářet výchozí pravidla pro přístup k protokolu RDP toopermit virtuálních počítačů Windows na TCP port 3389. Výchozí pravidla pro virtuální počítače s Linuxem povolit přístup SSH na TCP port 22. Tato pravidla seznamu ACL automatického jsou vytvářené v hello následující podmínky:

* Pokud vytvoříte virtuální počítač s Windows přes portál hello a přijmout hello výchozí akce toocreate skupinu zabezpečení sítě, vytvoří se seznamu ACL pravidlo tooallow TCP port 3389 (RDP).
* Pokud vytvoříte virtuální počítač s Linuxem pomocí portálu hello a přijmout hello výchozí akce toocreate skupinu zabezpečení sítě, vytvoří se seznamu ACL pravidlo tooallow port TCP 22 (SSH).

V jiných podmínkách tato pravidla seznamu ACL výchozí nejsou vytvořeny. Nemůžete se připojit tooyour virtuálních počítačů bez vytvoření hello příslušná pravidla seznamu ACL. Tyto podmínky hello následující běžné akce:

* Vytvoření skupiny zabezpečení sítě prostřednictvím portálu hello jako samostatné akce toocreating hello virtuálních počítačů.
* Vytvoření skupiny zabezpečení sítě prostřednictvím kódu programu prostřednictvím prostředí PowerShell a rozhraní příkazového řádku Azure, rozhraní Rest API, atd.
* Vytvoření virtuálního počítače a jeho přiřazení tooan existující skupinu zabezpečení sítě, který ještě nemá odpovídající pravidlo seznamu ACL hello definované.

V předchozích případech všechny hello musíte toocreate pravidla seznamu ACL pro připojení virtuálních počítačů tooallow hello příslušné vzdálené správy.

## <a name="default-behavior-of-a-vm-without-a-network-security-group"></a>Výchozí chování virtuálního počítače bez skupinu zabezpečení sítě
Můžete vytvořit virtuální počítač bez vytvoření skupiny zabezpečení sítě. V těchto situacích se můžete připojit tooyour virtuálních počítačů pomocí protokolu RDP nebo SSH bez vytvoření všechna pravidla seznamu ACL. Podobně pokud jste nainstalovali webovou službu na portu 80, služby je automaticky dostupné vzdáleně. Hello virtuální počítač má všechny otevřené porty.

> [!NOTE]
> Stále potřebujete toohave veřejnou IP adresu přiřazenou tooa virtuálních počítačů v pořadí pro všechny vzdálená připojení. Nemá skupinu zabezpečení sítě pro podsíť nebo síť rozhraní hello nezveřejňuje externí přenosy tooany hello virtuálních počítačů. Hello výchozí akce při vytváření virtuálního počítače přes portál hello je toocreate novou veřejnou IP adresu. Pro všechny ostatní způsoby vytvoření virtuálního počítače, jako jsou prostředí PowerShell, rozhraní příkazového řádku Azure nebo šablony Resource Manageru veřejnou IP adresu se nevytvoří automaticky pokud explicitně požadována. výchozí akce Hello prostřednictvím portálu hello je také toocreate skupinu zabezpečení sítě. Tato akce výchozí znamená, že by neměl skončili v situaci s zveřejněné virtuální počítač, který má žádná síť filtrování na místě.

## <a name="understanding-load-balancers-and-nat-rules"></a>Pochopení nástroje pro vyrovnávání zatížení a pravidla NAT
V modelu nasazení Classic hello můžete vytvořit koncové body, které také provést přesměrování portu. Když vytvoříte virtuální počítač v modelu nasazení Classic hello, by automaticky vytvoří pravidla seznamu ACL pro protokolu RDP nebo SSH. Tato pravidla nebude vystavit TCP port 3389 nebo TCP port 22 v uvedeném pořadí toohello mimo world. Místo toho port TCP vysoké hodnoty by zpřístupnit mapující toohello příslušné interní port. Můžete také vytvořit vlastní pravidla seznamu ACL podobným způsobem, jako je například zveřejněte webový server na TCP portu 4280 toohello mimo world. Zobrazí se tato pravidla seznamu ACL a mapování portů v hello následující snímek obrazovky z portálu Classic hello:

![Předávání portů s koncovými body Classic](./media/virtual-machines-common-endpoints-in-resource-manager/classic-endpoints-port-forwarding.png)

Pomocí skupin zabezpečení sítě že funkce předávání portů zpracovává nástroj pro vyrovnávání zatížení. Další informace najdete v tématu [v Azure Vyrovnávání zatížení](../articles/load-balancer/load-balancer-overview.md). Hello následující příklad ukazuje Vyrovnávání zatížení s NAT pravidlo tooperform předávání portů TCP port 4222 toohello interní port TCP 22 virtuálního počítače:

![Načíst pravidla NAT nástroje pro vyrovnávání pro přesměrování portu](./media/virtual-machines-common-endpoints-in-resource-manager/load-balancer-nat-rules.png)

> [!NOTE]
> Při implementaci služby Vyrovnávání zatížení obvykle nepřiřadíte hello virtuální počítač na veřejnou IP adresu. Místo toho nástroj pro vyrovnávání zatížení hello má veřejné tooit adresu přiřazenou IP. Stále potřebujete toocreate vaše skupina zabezpečení sítě a seznamu ACL pravidla toodefine hello tok přenosů dat do aplikace a z virtuálního počítače. Hello pravidla NAT nástroje pro vyrovnávání zatížení jsou jednoduše toodefine, jaké porty jsou povolené prostřednictvím hello nástroj pro vyrovnávání zatížení a jak získat distribuován do back-end hello virtuálních počítačů. Jako takový je nutné toocreate pravidlo NAT pro provoz tooflow prostřednictvím hello nástroj pro vyrovnávání zatížení. tooallow hello provoz tooreach hello virtuálního počítače, vytvořte pravidlo seznamu ACL skupiny zabezpečení sítě.
