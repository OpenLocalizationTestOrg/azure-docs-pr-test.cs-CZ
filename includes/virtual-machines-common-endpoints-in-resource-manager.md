Přístup k koncové body Azure funguje trochu jinak mezi modelem nasazení Classic a Resource Manager. V modelu nasazení Resource Manager máte nyní možnost pro vytvoření filtrů sítě, která řídí tok provozu do a z virtuálních počítačů. Tyto filtry vám umožňují vytvořit komplexní prostředích sítí nad rámec koncový bod jednoduché jako model nasazení Classic. Tento článek obsahuje přehled skupin zabezpečení sítě a jak se liší od pomocí koncových bodů Classic, vytváření těchto pravidla, filtrování a ukázkové scénáře nasazení.

## <a name="overview-of-resource-manager-deployments"></a>Přehled nasazení Resource Manager
Koncové body v modelu nasazení Classic se nahrazují skupiny zabezpečení sítě a pravidla seznamu ACL pro řízení přístupu. Rychlé kroky pro implementaci pravidla seznamu ACL skupiny zabezpečení sítě jsou:

* Vytvořte skupinu zabezpečení sítě.
* Chcete-li povolit nebo odepřít provoz, definujte pravidel seznamu ACL skupiny zabezpečení sítě.
* Vaše skupina zabezpečení sítě přiřadíte rozhraní sítě nebo podsítě virtuální sítě.

Pokud jsou chtějí také provést přesměrování portu, musíte umístit Vyrovnávání zatížení před virtuálního počítače a použít pravidla NAT. Rychlé kroky pro implementaci služby Vyrovnávání zatížení a pravidla NAT vypadat takto:

* Vytvořte nástroj pro vyrovnávání zatížení.
* Vytvořte fond back-end a přidejte virtuální počítače do fondu.
* Definování pravidel NAT pro přesměrování požadovaný port.
* Přiřaďte pravidel NAT virtuální počítače.

## <a name="network-security-group-overview"></a>Přehled skupinu zabezpečení sítě
Skupiny zabezpečení sítě zadejte vrstvu zabezpečení můžete povolit určité porty a podsítě pro přístup virtuálních počítačů. Obvykle vždy máte skupinu zabezpečení sítě poskytuje tuto vrstvu zabezpečení mezi virtuální počítače a vnějším světem. Skupiny zabezpečení sítě je použít pro podsíť virtuální sítě nebo konkrétním síťovém rozhraní pro virtuální počítač. Místo vytvoření koncového bodu pravidla seznamu ACL, je nyní vytvořit pravidla seznamu ACL skupiny zabezpečení sítě. Tato pravidla seznamu ACL poskytují mnohem větší kontrolu než jednoduše vytvoření koncového bodu předávat zadaný port. Další informace najdete v tématu [co je skupina zabezpečení sítě?](../articles/virtual-network/virtual-networks-nsg.md)

> [!TIP]
> Skupiny zabezpečení sítě můžete přiřadit k více podsítí nebo síťová rozhraní. Neexistuje žádné mapování 1:1. Můžete vytvořit skupinu zabezpečení sítě s společnou sadu pravidel seznamu ACL a použít pro více podsítí nebo síťová rozhraní. Navíc skupinu zabezpečení sítě, mohou být použity na prostředky u vašeho předplatného (na základě [řízení přístupu na základě Role](../articles/active-directory/role-based-access-control-what-is.md).

## <a name="load-balancers-overview"></a>Přehled nástroje pro vyrovnávání zatížení
V modelu nasazení Classic by Azure provádět překlad adres (NAT) a portu v cloudové službě předávání za vás. Při vytváření koncový bod, zadali byste vystavit společně s interní port směrovat provoz na externím portu. Skupiny zabezpečení sítě samy o sobě Neprovádět tento stejný NAT a přesměrování portu. 

Abyste mohli vytvořit pravidla NAT pro takové port předávání, vytvořte k nástroji pro vyrovnávání zatížení Azure ve vaší skupině prostředků. Nástroje pro vyrovnávání zatížení je opět dostatečně podrobné, chcete-li se vztahují pouze na konkrétní virtuální počítače v případě potřeby. Pravidla NAT nástroje pro vyrovnávání zatížení Azure fungovat společně se pravidla seznamu ACL skupiny zabezpečení sítě k poskytování mnohem větší flexibilitu a řízení než bylo dosažitelný pomocí koncové body cloudové služby. Další informace najdete v tématu [přehled nástroje pro vyrovnávání zatížení](../articles/load-balancer/load-balancer-overview.md).

## <a name="network-security-group-acl-rules"></a>Pravidla seznamu ACL skupinu zabezpečení sítě
Pravidla seznamu ACL umožňují definovat, jaké přenos můžete do aplikace a z virtuálního počítače založené na určité porty a rozsahy portů a protokolů. Pravidla jsou přiřazeny, jednotlivé virtuální počítače nebo do podsítě. Na následujícím snímku obrazovky je příklad pravidla seznamu ACL pro běžné webový server:

![Pravidla seznamu ACL seznamu skupiny zabezpečení sítě](./media/virtual-machines-common-endpoints-in-resource-manager/example-acl-rules.png)

Pravidla seznamu ACL platí podle priority metriky, které zadáte - Čím vyšší hodnota, tím nižší prioritu. Každá skupina zabezpečení sítě má tři výchozí pravidla, které jsou určeny ke zpracování Azure síťových přenosů dat, s pomocí explicitní tok `DenyAllInbound` jako poslední pravidlo. Výchozí pravidla seznamu ACL mají nízkou prioritu nebudou v konfliktu s pravidla, které vytvoříte.

## <a name="assigning-network-security-groups"></a>Přiřazení skupiny zabezpečení sítě
Skupina zabezpečení sítě přiřadit podsítě nebo síťové rozhraní. Tento přístup umožňuje být granulární, podle potřeby při použití pravidel seznamu ACL pouze konkrétní virtuální počítač nebo k zajištění, společnou sadu pravidel seznamu ACL platí pro všechny virtuální počítače nepatří do podsítě:

![Použít skupiny Nsg na síťová rozhraní nebo podsítě](./media/virtual-machines-common-endpoints-in-resource-manager/apply-nsg-to-resources.png)

Chování skupinu zabezpečení sítě nemění v závislosti na přiřazeny podsíť nebo síťové rozhraní. Běžný scénář nasazení má skupina zabezpečení sítě k podsíti přiřazené k zajištění dodržování předpisů pro všechny virtuální počítače připojené k této podsíti. Další informace najdete v tématu [použití skupin zabezpečení sítě k prostředkům](../articles/virtual-network/virtual-networks-nsg.md#associating-nsgs).

## <a name="default-behavior-of-network-security-groups"></a>Výchozí chování skupiny zabezpečení sítě
V závislosti na tom, jak a kdy vytvoříte skupinu zabezpečení vaší sítě je možné vytvářet výchozí pravidla pro virtuální počítače Windows tak, aby povolovala přístup RDP na TCP port 3389. Výchozí pravidla pro virtuální počítače s Linuxem povolit přístup SSH na TCP port 22. Tato pravidla seznamu ACL automatického vytvářejí za následujících podmínek:

* Pokud vytvoříte virtuální počítač s Windows pomocí portálu a přijměte výchozí akci pro vytvoření skupiny zabezpečení sítě, vytvoří se pravidlo seznamu ACL pro povolit TCP port 3389 (RDP).
* Pokud vytvoříte virtuální počítač s Linuxem pomocí portálu a přijměte výchozí akci pro vytvoření skupiny zabezpečení sítě, vytvoří se pravidlo seznamu ACL pro povolit port TCP 22 (SSH).

V jiných podmínkách tato pravidla seznamu ACL výchozí nejsou vytvořeny. Nemůžete se připojit k virtuálnímu počítači bez vytvoření příslušná pravidla seznamu ACL. Následující běžné akce tyto podmínky:

* Vytvoření skupiny zabezpečení sítě prostřednictvím portálu jako samostatné akce k vytvoření virtuálního počítače.
* Vytvoření skupiny zabezpečení sítě prostřednictvím kódu programu prostřednictvím prostředí PowerShell a rozhraní příkazového řádku Azure, rozhraní Rest API, atd.
* Vytvoření virtuálního počítače a jeho přiřazení k existující skupinu zabezpečení sítě, který ještě nemá příslušné pravidlo seznamu ACL definované.

Ve všech předchozích případech musíte vytvořit pravidla seznamu ACL pro virtuální počítač a povolíte připojení příslušné vzdálené správy.

## <a name="default-behavior-of-a-vm-without-a-network-security-group"></a>Výchozí chování virtuálního počítače bez skupinu zabezpečení sítě
Můžete vytvořit virtuální počítač bez vytvoření skupiny zabezpečení sítě. V takových situacích se může připojit k virtuálnímu počítači pomocí protokolu RDP nebo SSH bez vytvoření všechna pravidla seznamu ACL. Podobně pokud jste nainstalovali webovou službu na portu 80, služby je automaticky dostupné vzdáleně. Virtuální počítač má všechny otevřené porty.

> [!NOTE]
> Dál je potřeba mít veřejné IP adresy přiřazené k virtuálnímu počítači v pořadí pro všechny vzdálená připojení. Nemá skupinu zabezpečení sítě pro podsíť nebo síť rozhraní nezveřejňuje virtuální počítač do jakékoli externích přenosů. Výchozí akcí při vytváření virtuálního počítače přes portál, je vytvořit novou veřejnou IP adresu. Pro všechny ostatní způsoby vytvoření virtuálního počítače, jako jsou prostředí PowerShell, rozhraní příkazového řádku Azure nebo šablony Resource Manageru veřejnou IP adresu se nevytvoří automaticky pokud explicitně požadována. Výchozí akce prostřednictvím portálu je taky vytvořit skupinu zabezpečení sítě. Tato akce výchozí znamená, že by neměl skončili v situaci s zveřejněné virtuální počítač, který má žádná síť filtrování na místě.

## <a name="understanding-load-balancers-and-nat-rules"></a>Pochopení nástroje pro vyrovnávání zatížení a pravidla NAT
V modelu nasazení Classic můžete vytvořit koncové body, které také provést přesměrování portu. Když vytvoříte virtuální počítač v modelu nasazení Classic, by automaticky vytvoří pravidla seznamu ACL pro protokolu RDP nebo SSH. Tato pravidla nebude vystavit TCP port 3389 nebo port TCP 22 v uvedeném pořadí k vnějším světem. By být místo toho zpřístupněny port TCP vysoké hodnoty, která se mapuje na interní příslušný port. Můžete také vytvořit vlastní pravidla seznamu ACL podobným způsobem, jako je například zveřejněte webový server na portu TCP 4280 k vnějším světem. Tato pravidla seznamu ACL a mapování portů na následujícím snímku obrazovky z portálu Classic, můžete zjistit:

![Předávání portů s koncovými body Classic](./media/virtual-machines-common-endpoints-in-resource-manager/classic-endpoints-port-forwarding.png)

Pomocí skupin zabezpečení sítě že funkce předávání portů zpracovává nástroj pro vyrovnávání zatížení. Další informace najdete v tématu [v Azure Vyrovnávání zatížení](../articles/load-balancer/load-balancer-overview.md). Následující příklad ukazuje, nástroj pro vyrovnávání zatížení s pravidlem NAT provést přesměrování portu TCP port 4222 interní port TCP 22 virtuálního počítače:

![Načíst pravidla NAT nástroje pro vyrovnávání pro přesměrování portu](./media/virtual-machines-common-endpoints-in-resource-manager/load-balancer-nat-rules.png)

> [!NOTE]
> Při implementaci služby Vyrovnávání zatížení obvykle nepřiřadíte virtuální počítač na veřejnou IP adresu. Místo toho nástroj pro vyrovnávání zatížení má přiřazenou veřejnou IP adresu. Dál je potřeba vytvořit skupinu zabezpečení sítě a seznamu ACL pravidel definovat tok provozu do a z virtuálního počítače. Pravidla NAT nástroje pro vyrovnávání zatížení jsou jednoduše definovat, jaké porty jsou povolené prostřednictvím nástroje pro vyrovnávání zatížení a jak získat distribuován do back-end virtuálních počítačů. Jako takový budete muset vytvořit pravidlo NAT pro provoz procházet skrz nástroje pro vyrovnávání zatížení. Pokud chcete povolit přenosy připojit virtuální počítač, vytvořte pravidlo seznamu ACL skupiny zabezpečení sítě.
