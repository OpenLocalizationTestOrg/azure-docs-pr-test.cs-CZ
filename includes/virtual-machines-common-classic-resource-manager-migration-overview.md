# <a name="platform-supported-migration-of-iaas-resources-from-classic-tooazure-resource-manager"></a>Podporované platformy migrace prostředky infrastruktury z classic tooAzure Resource Manager
V tomto článku jsme popisují, jak jsme se povolení migrace infrastruktury jako služby (IaaS) prostředky z modelů nasazení hello Classic tooResource správce. Další informace o [funkce služby Správce prostředků Azure a výhody](../articles/azure-resource-manager/resource-group-overview.md). Jsme podrobnosti, jak brány site-to-site tooconnect prostředky z hello dvou modelů nasazení, které společně existovat ve vašem předplatném pomocí virtuální sítě.

## <a name="goal-for-migration"></a>Cíl pro migraci
Resource Manager umožňuje nasazení složitých aplikací prostřednictvím šablon, nakonfiguruje virtuální počítače pomocí rozšíření virtuálního počítače a zahrnuje správu přístupu a označování. Azure Resource Manager zahrnuje škálovatelné a paralelní nasazení pro virtuální počítače do skupiny dostupnosti. Nový model nasazení Hello také poskytuje správu životního cyklu výpočty, síť a úložiště nezávisle. Nakonec je zaměřená na povolení zabezpečení ve výchozím nastavení s vynucením hello virtuálních počítačů ve virtuální síti.

Téměř všechny funkce hello z modelu nasazení classic hello jsou podporovány pro výpočty, síť a úložiště v Azure Resource Manager. toobenefit z hello nové funkce ve službě Správce prostředků Azure, můžete migrovat existující nasazení z modelu nasazení Classic hello.

## <a name="supported-resources-for-migration"></a>Podporované prostředky pro migraci
Při migraci jsou podporovány tyto klasické prostředky IaaS

* Virtuální počítače
* Skupiny dostupnosti
* Cloud Services
* Účty úložiště
* Virtuální sítě
* Brány VPN Gateway
* Express trasy brány _(v hello stejného předplatného jako virtuální síť pouze)_
* Network Security Groups (Skupiny zabezpečení sítě) 
* Směrovací tabulky 
* Vyhrazené IP adresy 

## <a name="supported-scopes-of-migration"></a>Podporované obory migrace
Existují různé způsoby 4 migrace toocomplete prostředků výpočty, síť a úložiště. Jedná se o 

* Migrace virtuálních počítačů (ne ve virtuální síti)
* Migrace virtuálních počítačů (ve virtuální síti)
* Migrace účtů úložiště
* Odpojit prostředky (skupiny zabezpečení sítě, směrovací tabulky a vyhrazené IP adresy)

### <a name="migration-of-virtual-machines-not-in-a-virtual-network"></a>Migrace virtuálních počítačů (ne ve virtuální síti)
V modelu nasazení Resource Manager hello zabezpečení je vynucené pro vaše aplikace ve výchozím nastavení. Všechny virtuální počítače nutné toobe ve virtuální síti v modelu Resource Manager hello. Hello restartování platformy Azure (`Stop`, `Deallocate`, a `Start`) hello virtuální počítače jako součást migrace hello. Máte dvě možnosti pro hello virtuální sítě, které hello virtuální počítače se budou migrovat do:

* Si můžete vyžádat hello platformy toocreate nové virtuální sítě a migrovat virtuální počítač hello do hello nové virtuální sítě.
* Hello virtuálního počítače můžete migrovat do existující virtuální síť ve službě Správce prostředků.

> [!NOTE]
> V tomto rozsahu migrace obě hello roviny management operace a operace datové roviny hello nemusí být povoleny pro určitou dobu během migrace hello.
>
>

### <a name="migration-of-virtual-machines-in-a-virtual-network"></a>Migrace virtuálních počítačů (ve virtuální síti)
Pro většinu konfiguraci virtuálních počítačů je pouze metadata hello migraci mezi modely nasazení Classic a Resource Manager hello. Základní virtuální počítače jsou spuštěné na hello Hello stejný hardware, v hello stejné sítě a s hello stejné úložiště. pro určitou dobu během migrace hello nemusí být povoleno Hello roviny management operace. Hello datové roviny však pokračuje toowork. To znamená, že vaše aplikace spuštěné na virtuálních počítačích (klasické) nevznikají výpadek během migrace hello.

Hello následující konfigurace nejsou aktuálně podporovány. Pokud v budoucnu hello je přidána podpora, některé virtuální počítače v této konfiguraci může způsobit výpadek (přejděte prostřednictvím zastavit, navrácení a restartujte počítač operations).

* Máte více než jeden dostupnosti v rámci jedné cloudové služby.
* Máte jeden nebo více sad dostupnosti a virtuální počítače, které nejsou ve skupině dostupnosti nastavena v jednom cloudové služby.

> [!NOTE]
> Hello správu roviny pro určitou dobu během migrace hello nemusí být povoleno v tomto rozsahu migrace. Pro některé konfigurace jak bylo popsáno výše, datové roviny nastane.
>
>

### <a name="storage-accounts-migration"></a>Migrace účtů úložiště
tooallow bezproblémové migrace, můžete nasadit virtuální počítače správce prostředků v účtu úložiště classic. Díky této funkci výpočetní a síťové prostředky můžete a mají být migrovány nezávisle na účty úložiště. Když provádíte migraci přes virtuální počítače a virtuální síť, musíte toomigrate přes procesu migrace hello toocomplete účty úložiště.

> [!NOTE]
> model nasazení Resource Manager Hello nemá hello koncept Classic Image a disky. Pokud účet úložiště hello je migrované, Classic Image a disky nejsou viditelné v zásobníku Resource Manager hello ale hello zálohování virtuální pevné disky zůstávají v účtu úložiště hello.
>
>

### <a name="unattached-resources-network-security-groups-route-tables--reserved-ips"></a>Odpojit prostředky (skupiny zabezpečení sítě, směrovací tabulky a vyhrazené IP adresy)
Skupiny zabezpečení sítě, směrovací tabulky a vyhrazené IP adresy, které nejsou připojené tooany, které virtuální počítače a virtuální sítě se dají migrovat nezávisle.

<br>

## <a name="unsupported-features-and-configurations"></a>Nepodporované funkce a konfigurace
Nepodporujeme aktuálně některé funkce a konfigurace. Hello následující oddíly popisují Naše doporučení je obcházet.

### <a name="unsupported-features"></a>Nepodporované funkce
Hello následující funkce nejsou aktuálně podporovány. Volitelně můžete tato nastavení odebrat, migrace virtuálních počítačů hello a pak znovu povolte nastavení hello v modelu nasazení Resource Manager hello.

| Poskytovatel prostředků | Funkce | Doporučení |
| --- | --- | --- |
| Compute |Disky nepřidružený virtuálního počítače. | objekty BLOB VHD Hello za tyto disky bude migrována při migraci hello účtu úložiště |
| Compute |Bitové kopie virtuálních počítačů. | objekty BLOB VHD Hello za tyto disky bude migrována při migraci hello účtu úložiště |
| Síť |Seznamy ACL koncových bodů. | Odeberte seznamy ACL koncových bodů a zkuste migraci opakovat. |
| Síť |Virtuální síť s bránu ExpressRoute i bránu VPN  | Odeberte hello brána sítě VPN před zahájením migrace a poté znovu hello brány VPN po dokončení migrace. Další informace o [ExpressRoute migrace](../articles/expressroute/expressroute-migration-classic-resource-manager.md).|
| Síť |ExpressRoute s odkazy autorizace  | Odeberte hello síťové připojení toovirtaul okruh ExpressRoute před zahájením migrace a pak znovu vytvořte připojení hello po dokončení migrace. Další informace o [ExpressRoute migrace](../articles/expressroute/expressroute-migration-classic-resource-manager.md). |
| Síť |Application Gateway | Odeberte hello Application Gateway před zahájením migrace a poté znovu hello Application Gateway po dokončení migrace. |
| Síť |Virtuální sítě pomocí virtuální sítě partnerský vztah. | Migrovat virtuální sítě tooResource Manager a potom partnerský uzel. Další informace o [VNet Peering](../articles/virtual-network/virtual-network-peering-overview.md). | 

### <a name="unsupported-configurations"></a>Nepodporované konfigurace
Hello následující konfigurace nejsou aktuálně podporovány.

| Služba | Konfigurace | Doporučení |
| --- | --- | --- |
| Resource Manager |Na základě řízení přístupu role (RBAC) pro klasické prostředky |Protože hello URI hello prostředků je upravené po migraci, doporučujeme, abyste naplánovali hello RBAC zásady aktualizace, které vyžadují toohappen po migraci. |
| Compute |Více podsítí, které jsou přidružené k virtuálnímu počítači |Aktualizujte hello podsíť konfigurace tooreference pouze podsítě. |
| Compute |Virtuální počítače, které patří tooa virtuální sítě, ale nemáte podsíť explicitní přiřazen |Volitelně můžete odstranit hello virtuálních počítačů. |
| Compute |Virtuální počítače, které mají výstrahy, zásady škálování |migrace Hello prochází a tato nastavení jsou vyřadit. Důrazně doporučujeme vyhodnotit prostředí před hello migrace. Alternativně můžete znovu nakonfigurovat nastavení výstrah hello po dokončení migrace. |
| Compute |Rozšíření virtuálního počítače XML (BGInfo 1.*, Visual Studio Debugger, Web Deploy a vzdálené ladění) |To není podporováno. Doporučujeme, aby odebrání těchto rozšíření z migrace toocontinue hello virtuálního počítače nebo se budou automaticky odstraněna během procesu migrace hello. |
| Compute |Diagnostika spouštění Storage úrovně Premium |Před pokračováním migrace zakážete funkci Diagnostika spouštění pro hello virtuálních počítačů. Diagnostika spouštění v zásobníku Resource Manager hello můžete znovu povolit po dokončení migrace hello. Kromě toho objekty BLOB, které jsou používány pro snímek obrazovky a sériové protokoly měla by být odstraněna, jsou už účtovat poplatek za tyto objekty BLOB. |
| Compute |Cloudové služby, které obsahují webové/role pracovního procesu |Tato možnost není aktuálně podporována. |
| Síť |Virtuální sítě, které obsahují virtuální počítače a webové/role pracovního procesu |Tato možnost není aktuálně podporována. |
| Azure App Service |Virtuální sítě, které obsahují služby App Service Environment |Tato možnost není aktuálně podporována. |
| Azure HDInsight |Virtuální sítě, které obsahují služby HDInsight |Tato možnost není aktuálně podporována. |
| Služby životního cyklu aplikace Microsoft Dynamics |Virtuální sítě, které obsahují virtuální počítače, které jsou spravovány nástrojem služby Dynamics životního cyklu |Tato možnost není aktuálně podporována. |
| Azure AD Domain Services |Virtuální sítě, které obsahují služby Azure AD Domain services |Tato možnost není aktuálně podporována. |
| Azure RemoteApp |Virtuální sítě, které obsahují nasazení Azure RemoteApp |Tato možnost není aktuálně podporována. |
| Azure API Management |Virtuální sítě, které obsahují nasazení Azure API Management |Tato možnost není aktuálně podporována. hello toomigrate IaaS sítě VNET, změňte hello VNET hello nasazení API Management, což je žádná operace výpadku. |
| Compute |Rozšíření Azure Security Center s virtuální sítě, který má připojení přenosu brána sítě VPN nebo brány ExpressRoute s místní server DNS |Azure Security Center automaticky nainstaluje rozšíření na vaše virtuální počítače toomonitor jejich zabezpečení a vygenerovat výstrahy. Tato rozšíření obvykle budou instalovány automaticky pokud hello zásad Azure Security Center je povolená v předplatném hello. Migrace brány ExpressRoute není aktuálně podporována a bran VPN s připojením k přenosu ztratí místního přístupu. Odstranění ExpressRoute bránu nebo migrace brány sítě VPN s připojením k přenosu způsobí, že internet přístup tooVM úložiště účet toobe ztráty, když budete pokračovat s potvrzením hello migrace. v takovém případě jako objekt blob stavu hello hostovaného agenta nelze naplnit, Hello migrace nebude pokračovat. 3 hodiny, než budete pokračovat v migraci doporučujeme toodisable zásad Azure Security Center v předplatném hello. |

