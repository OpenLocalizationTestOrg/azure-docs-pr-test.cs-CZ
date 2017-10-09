## <a name="meaning-of-migration-of-iaas-resources-from-hello-classic-deployment-model-tooresource-manager"></a>Význam migrace prostředky infrastruktury z tooResource modelu nasazení classic hello Manager
Před jsme podrobnostem hello podrobnosti, podíváme se na hello rozdíl mezi dat a správu rovinu operací s prostředky IaaS hello.

* *Rovině řízenísprávy/* popisuje hello volání, které začalo rovině řízení správy/hello nebo hello API úpravy prostředků. Například operace, jako je vytvoření virtuálního počítače, restartování virtuálního počítače a aktualizace virtuální sítě s novou podsíť spravovat hello s prostředky. Neovlivňují přímo připojování toohello instance.
* *Data roviny* Popisuje modul runtime hello vlastní aplikace hello (aplikace) a zahrnuje interakci s instancí, které si projít hello rozhraní API služby Azure. Za interakci v rovině dat neboli interakci s aplikací může být považován přístup k webu nebo načítání dat ze spuštěné instance SQL Serveru nebo serveru MongoDB. Kopírování objektu blob z účtu úložiště a přístup k veřejné IP adresy tooRDP nebo SSH do hello virtuálního počítače jsou taky roviny data. Tyto operace zachovat aplikace hello spuštění pro výpočty, síť a úložiště.

Pozadí hello je hello hello datové roviny stejné mezi hello model nasazení Classic i Resource Manager zásobníku. Během procesu migrace jsme převede hello reprezentace hello prostředky z hello klasického nasazení modelu toothat v zásobníku Resource Manager hello. V důsledku toho je nutné toouse nové nástroje, rozhraní API sady SDK toomanage vaše prostředky v zásobníku Resource Manager hello.

![Snímek obrazovky, který ukazuje rozdíl mezi rovinou správy/řízení a rovinou dat](../articles/virtual-machines/media/virtual-machines-windows-migration-classic-resource-manager/data-control-plane.png)


> [!NOTE]
> V některých scénářích migrace hello platformy Azure zastaví, zruší přidělení a restartuje virtuální počítače. To způsobuje krátké výpadky roviny dat.
>

## <a name="hello-migration-experience"></a>možnosti migrace Hello
Před zahájením migrace prostředí hello, se doporučuje hello následující:

* Ujistěte se, že hello prostředky, které chcete toomigrate nepoužívejte jakékoli konfigurace nebo nepodporované funkce. Platforma hello obvykle zjistí tyto problémy a vygeneruje se chyba.
* Pokud máte virtuální počítače, které nejsou ve virtuální síti, budou se zastaví a navrácena jako součást hello Příprava operaci. Pokud nechcete, aby toolose hello veřejnou IP adresu, podívejte se do rezervování hello IP adresu než hello připravte operaci. Ale pokud hello virtuální počítače ve virtuální síti, že nejsou zastavena a navrácena.
* Plánování migrace během pracovní doby tooaccommodate pro neočekávané chyby, které může dojít během migrace.
* Stáhněte si hello aktuální konfiguraci virtuálních počítačů pomocí prostředí PowerShell, příkazy rozhraní příkazového řádku (CLI) nebo rozhraní REST API toomake snazší pro ověření po hello Příprava krok je dokončená.
* Před zahájením migrace hello, aktualizujte vaše automation nebo operationalization skripty toohandle hello modelu nasazení Resource Manager. Volitelně můžete provést operace GET při hello prostředky jsou v hello připravený stavu.
* Vyhodnoťte hello RBAC zásady, které jsou nakonfigurované na hello klasické prostředky IaaS a plánování po dokončení migrace hello.

pracovní postup migrace Hello je následující

![Snímek obrazovky, který ukazuje pracovní postup migrace hello](../articles/virtual-machines/windows/media/migration-classic-resource-manager/migration-workflow.png)

> [!NOTE]
> Všechny operace hello popsané v následující části hello jsou idempotent. Pokud došlo k potížím. než nepodporované funkce nebo chyby v konfiguraci, doporučujeme zopakovat hello připravit, zrušení nebo potvrzení operace. Hello platformy Azure pokusí hello akci znovu.
>
>

### <a name="validate"></a>Ověření
ověření operace Hello je hello prvním krokem v procesu migrace hello. Hello cílem tohoto kroku je stav hello tooanalyze hello prostředků chcete toomigrate v modelu nasazení classic hello a vrátí úspěch nebo selhání, pokud hello prostředky podporují migrace.

Vyberte virtuální síť hello nebo cloudové služby (Pokud není ve virtuální síti) mají toovalidate k migraci.

* Pokud hello prostředků není schopné migrace, uvádí hello platformy Azure všechny hello důvody proč není podporována pro migraci.

#### <a name="checks-not-done-in-validate"></a>Kontroluje, není potřeba v ověření

Ověření operace pouze analyzuje stav hello hello prostředků v modelu nasazení classic hello. Můžete vyhledat všechny chyby a nepodporované scénáře z důvodu konfigurace toovarious v modelu nasazení classic hello. Není možné toocheck pro všechny problémy, které hello Azure Resource Manager zásobníku může uložit prostředky hello během migrace. Tyto problémy se kontroluje jenom při transformaci v dalším kroku hello migrace, který je příprava podstoupit hello prostředky. Následující tabulka Hello uvádí všechny problémy hello nevráceno ověřením.


|Kontroly sítě není v ověření|
|-|
|Virtuální síť s bránami ER a sítě VPN|
|Připojení brány virtuální sítě v odpojení stavu|
|Všechny okruhy ER jsou předem migrovaných tooAzure zásobníku Resource Manager|
|Azure Resource Manager kvóty vyhledává zdroje informací o sítích, tedy statickou veřejnou IP adresu, dynamické veřejné IP adresy, nástroj pro vyrovnávání zatížení, skupiny zabezpečení sítě, směrovací tabulky, síťová rozhraní |
| Zkontrolujte, zda jsou všechna pravidla pro vyrovnávání zatížení platný napříč nasazení nebo virtuální sítě |
| Zkontrolujte konfliktní privátních IP adres mezi navrácena zastavení virtuálních počítačů v hello stejné virtuální sítě |

### <a name="prepare"></a>Příprava
Příprava operace Hello je hello druhý krok v procesu migrace hello. Hello cílem tohoto kroku je toosimulate hello transformace hello prostředky infrastruktury z prostředků Manager tooResource modelu nasazení classic a prezentovat to vedle sebe pro vás toovisualize.

> [!NOTE] 
> Klasické prostředky se nemění v tomto kroku. Proto je toorun bezpečné krok, pokud se pokoušíte se migrace. 

Vyberete hello virtuální sítě nebo hello cloudové služby (Pokud není virtuální sítě) mají tooprepare k migraci.

* Pokud hello prostředků není schopné migrace, hello platformy Azure zastaví hello procesu migrace a uvádí hello důvod, proč hello Příprava operace se nezdařila.
* Pokud je zdroj hello schopen migrace, hello platformy Azure nejprve zamyká hello roviny management operace pro hello prostředky v rámci migrace. Například můžete nejsou možné tooadd tooa disku dat virtuálních počítačů v rámci migrace.

Hello platformy Azure pak spustí hello migrace metadat z tooResource modelu nasazení classic správce pro migraci prostředků hello.

Po přípravě hello bylo dokončeno, máte možnost hello vizualizace hello prostředky v modelu nasazení classic i Resource Manager. Pro každé cloudové služby v rámci modelu nasazení classic hello hello platformy Azure vytvoří název skupiny prostředků, který má hello vzor `cloud-service-name>-Migrated`.

> [!NOTE]
> Není možné tooselect hello název skupiny prostředků vytvořené pro migrované prostředky (tj. "-migrovat"), ale po dokončení migrace, můžete použít Azure Resource Manager přesunutí funkce toomove prostředky tooany chcete skupinu prostředků. Další informace najdete tooread [přesunout skupiny prostředků toonew prostředků nebo předplatného](../articles/resource-group-move-resources.md)

Tady jsou dvou obrazovkách, které se zobrazí výsledek hello po úspěšného Příprava operace. První obrazovka ukazuje skupinu prostředků, který obsahuje hello původní cloudové služby. Druhý obrazovka ukazuje hello nové "-migrovat" skupinu prostředků, která obsahuje hello ekvivalentní prostředky Azure Resource Manager.

![Snímek obrazovky, který ukazuje portál s cloudovou službou modelu Classic](../articles/virtual-machines/windows/media/migration-classic-resource-manager/portal-classic.png)

![Snímek obrazovky, který ukazuje portál s prostředky Azure Resource Manageru ve fázi přípravy](../articles/virtual-machines/windows/media/migration-classic-resource-manager/portal-arm.png)

Zde je servisní pohled na vaše prostředky po dokončení hello fázi přípravy. Všimněte si, hello prostředků je roviny data hello je hello stejné. Je zobrazena v rovině řízení hello (modelu nasazení classic) a rovině řízení hello (Resource Manager).

![Pozadí hello ve fázi přípravy](../articles/virtual-machines/windows/media/migration-classic-resource-manager/behind-the-scenes-prepare.png)

> [!NOTE]
> Virtuální počítače, které nejsou v klasické virtuální sítě jsou v této fázi migrace navrácena byla zastavena.
>

### <a name="check-manual-or-scripted"></a>Kontrola (ruční nebo pomocí skriptu)
V kroku zkontrolujte hello můžete volitelně použít hello konfigurace, který jste si stáhli starší toovalidate správné hello migrace. Alternativně můžete přihlásit toohello portál a kontrola hello vlastnosti a prostředky toovalidate spokojeni metadata migrace.

Pokud migrujete virtuální síť, většina konfigurací virtuálních počítačů se nerestartuje. U aplikací na těchto virtuálních počítačů můžete ověřit, že aplikace hello je stále spuštěná.

Monitorování a automatizace a toosee provozní skripty můžete otestovat, pokud hello virtuálních počítačích fungují podle očekávání, a pokud aktualizované skripty fungovat správně. Jenom operace GET jsou podporovány, pokud hello prostředky jsou v hello připravený stavu.

Neexistuje žádná sada časový interval, před kterou je třeba toocommit hello migrace. V tomto stavu máte času, kolik chcete. Hello správu roviny však uzamčen pro tyto prostředky až do zrušení nebo potvrzení.

Pokud se zobrazí všechny problémy, můžete vždy abort hello migrace a vraťte toohello modelu nasazení classic. Po návratu zpět hello platformy Azure se otevře hello správu roviny operací s prostředky hello tak, aby mohli obnovit normální provoz na těchto virtuálních počítačích v modelu nasazení classic hello.

### <a name="abort"></a>Přerušení
Přerušení je volitelný krok, můžete použít toorevert modelu nasazení classic toohello změny a ukončit hello migraci. Tato operace odstraní hello Resource Manager metadata pro vaše prostředky, který byl vytvořen v předchozím kroku Příprava hello. 

![Pozadí hello ve fázi přerušení](../articles/virtual-machines/windows/media/migration-classic-resource-manager/behind-the-scenes-abort.png)


> [!NOTE]
> Tuto operaci nelze provést po mít aktivuje hello operace potvrzení.     
>

### <a name="commit"></a>Potvrzení
Po dokončení ověření hello můžete předat hello migrace. Prostředky se už nezobrazují v modelu nasazení classic a jsou k dispozici pouze v modelu nasazení Resource Manager hello. Hello migrované prostředky se dají spravovat jenom hello nového portálu.

> [!NOTE]
> Toto je idempotentní operace. Pokud se nezdaří, se doporučuje hello operaci zkuste zopakovat. Pokud pokračuje toofail, vytvořit lístek podpory, nebo vytvořit fórum post s ClassicIaaSMigration značkou na našem [fórum virtuálních počítačů](https://social.msdn.microsoft.com/Forums/azure/home?forum=WAVirtualMachinesforWindows).
>
>

![Pozadí hello ve fázi potvrzení](../articles/virtual-machines/windows/media/migration-classic-resource-manager/behind-the-scenes-commit.png)

## <a name="where-toobegin-migration"></a>Kde toobegin migrace?

Tady je Vývojový diagram, který ukazuje, jak tooproceed s migrací

![Snímek obrazovky, který zobrazuje kroky migrace hello](../articles/virtual-machines/windows/media/migration-classic-resource-manager/migration-flow.png)

## <a name="translation-of-classic-deployment-model-tooazure-resource-manager-resources"></a>Překlad prostředků Resource Manager tooAzure modelu nasazení classic
Hello modelu nasazení classic a Resource Manager reprezentace hello prostředků najdete v následující tabulce hello. Jiné funkce a prostředky se aktuálně nepodporují.

| Reprezentace v modelu Classic | Reprezentace v modelu Resource Manager | Podrobné poznámky |
| --- | --- | --- |
| Název cloudové služby |Název DNS |Při migraci, se vytvoří novou skupinu prostředků pro všechny cloudové služby s vzoru pro pojmenovávání hello `<cloudservicename>-migrated`. Tato skupina prostředků obsahuje všechny vaše prostředky. Název cloudové služby Hello stane název DNS, který je přidružen hello veřejnou IP adresu. |
| Virtuální počítač |Virtuální počítač |Vlastnosti specifické pro virtuální počítače se migrují beze změny. Určité osProfile informace, jako je název počítače, není uložen v modelu nasazení classic hello a po migraci zůstane prázdné. |
| Prostředky disku připojen tooVM |Implicitní disky připojené tooVM |Disky nejsou modelován jako nejvyšší úrovně prostředky v modelu nasazení Resource Manager hello. Budou se migrují jako implicitní disky v části hello virtuálních počítačů. Jenom ty disky, které jsou připojené tooa virtuálního počítače jsou aktuálně podporovány. Virtuální počítače správce prostředků teď můžete použít klasické účty úložiště, který umožňuje hello disky toobe snadno migrovat bez jakékoli aktualizace. |
| Rozšíření virtuálních počítačů |Rozšíření virtuálních počítačů |Všechna rozšíření prostředků hello, s výjimkou rozšíření XML se migrují z modelu nasazení classic hello. |
| Certifikáty virtuálních počítačů |Certifikáty v Azure Key Vault |Pokud Cloudová služba obsahuje certifikáty služby, Azure nového trezoru klíčů za cloudové služby a přesouvá hello certifikáty do trezoru klíčů hello. virtuální počítače, Hello jsou aktualizované tooreference hello certifikáty z trezoru klíčů hello. <br><br> **Poznámka:** prosím neodstraňujte hello keyvault protože to může způsobit toogo hello virtuálního počítače do stavu selhání. Pracujeme na vylepšení věcí v back-end hello tak, aby trezory klíč je možné bezpečně odstranit nebo přesunout společně s hello nové předplatné tooa virtuálních počítačů. |
| Konfigurace WinRM |Konfigurace WinRM v rámci osProfile |Konfigurace vzdálené správy se přesune s Windows beze změny, jako součást migrace hello. |
| Vlastnost sady dostupnosti |Prostředek sady dostupnosti | Specifikace skupiny dostupnosti byla vlastnost v modelu nasazení classic hello hello virtuálních počítačů. Skupiny dostupnosti stát prostředek nejvyšší úrovně v rámci migrace hello. Hello následující konfigurace nejsou podporovány: více sady dostupnosti za cloudové služby, nebo jeden nebo více dostupnosti nastaví společně s virtuálních počítačů, které nejsou v jakékoli dostupnosti v cloudové službě. |
| Konfigurace sítě na virtuálním počítači |Primární síťové rozhraní |Konfigurace sítě na virtuálním počítači je reprezentován jako hello primární síťové rozhraní prostředků po migraci. Pro virtuální počítače, které nejsou ve virtuální síti během migrace změní hello interní IP adresu. |
| Několik síťových rozhraní na virtuálním počítači |Síťová rozhraní |Pokud virtuální počítač má přidruženo více síťových rozhraní, každé síťové rozhraní se změní na prostředek nejvyšší úrovně v rámci migrace hello v modelu nasazení Resource Manager hello, spolu se všechny vlastnosti hello. |
| Sada koncových bodů s vyrovnáváním zatížení |Nástroj pro vyrovnávání zatížení |V modelu nasazení classic hello hello platformy přiřadit k nástroji pro vyrovnávání zatížení implicitní pro všechny cloudové služby. Během migrace je vytvořen nový prostředek pro vyrovnávání zatížení a pravidla pro vyrovnávání zatížení bude hello sady koncových bodů služby Vyrovnávání zatížení. |
| Příchozí pravidla NAT |Příchozí pravidla NAT |Vstupní koncové body definované na hello virtuálních počítačů jsou pravidla překladu adres převedený tooinbound sítě pod nástrojem pro vyrovnávání zatížení hello během migrace hello. |
| Virtuální IP adresa |Veřejná IP adresa s názvem DNS |Hello virtuální IP adresy se změní na veřejnou IP adresu a souvisí s hello nástroj pro vyrovnávání zatížení. Virtuální IP adresy je možné migrovat pouze pokud je vstupní koncový bod přiřazen tooit. |
| Virtuální síť |Virtuální síť |virtuální síť Hello je migrován, se všechny jeho vlastnosti, toohello modelu nasazení Resource Manager. Vytvoření nové skupiny prostředků s názvem hello `-migrated`. |
| Vyhrazené IP adresy |Veřejná IP adresa s metodou statického přidělování |Vyhrazené IP adresy přidružené k vyrovnávání zatížení hello se migrují společně s hello migrace hello cloudové služby nebo virtuálního počítače hello. Migrace nepřidružených vyhrazených IP adres se aktuálně nepodporuje. |
| Veřejná IP adresa na virtuální počítač |Veřejná IP adresa s metodou dynamického přidělování |Hello veřejnou IP adresu přidruženou hello virtuální počítač je převeden jako prostředek veřejné IP adresy, s toostatic sadu metoda přidělení hello. |
| Skupiny NSG |Skupiny NSG |Skupiny zabezpečení sítě spojené s podsítí se klonují jako součást modelu nasazení Resource Manager toohello migrace hello. během migrace hello se neodebere Hello NSG v modelu nasazení classic hello. Když probíhá migrace hello jsou však blokovány hello správu roviny operace pro hello NSG. |
| Servery DNS |Servery DNS |Servery DNS, které jsou přidružené k virtuální síti nebo hello virtuálních počítačů jsou migrovány jako součást hello odpovídající migraci prostředků, spolu se všechny vlastnosti hello. |
| UDR |UDR |Trasy definované uživatelem, které jsou spojené s podsítí se klonují jako součást modelu nasazení Resource Manager toohello migrace hello. během migrace hello se neodebere Hello UDR v modelu nasazení classic hello. operace správy roviny Hello hello UDR jsou zablokovány, když probíhá migrace hello. |
| Vlastnost předávání IP v konfiguraci sítě na virtuálním počítači |Vlastnost hello seskupování předávání IP |Hello předávání IP na virtuální počítač je převeden tooa vlastnost hello síťového rozhraní během migrace hello. |
| Nástroj pro vyrovnávání zatížení s několika IP adresami |Nástroj pro vyrovnávání zatížení s několika prostředky veřejné IP adresy |Všechny veřejné IP adresy přidružené k vyrovnávání zatížení hello je převeden tooa prostředek veřejné IP a související s nástrojem pro vyrovnávání zatížení hello po migraci. |
| Interní názvy DNS na hello virtuálních počítačů |Interní názvy DNS na hello síťový adaptér |Během migrace hello interní přípony DNS pro virtuální počítače hello jsou migrované tooa jen pro čtení vlastnost s názvem "InternalDomainNameSuffix" na hello síťový adaptér. přípona Hello zůstává beze změny po migraci a řešení virtuálních počítačů má pokračovat toowork jako dříve. |
| Brána virtuální sítě |Brána virtuální sítě |Vlastnosti brány virtuální sítě se migrují beze změny. buď Hello virtuální IP adresy přidružené k hello brány nezmění. |
| Místní síťová lokalita |Brána místní sítě |Vlastnosti místní sítě lokality jsou migrované beze změny tooa nový prostředek s názvem bránu místní sítě. Představuje předpony místních adres a IP adresu vzdálené brány. |
| Odkazy na připojení |Připojení |Odkazy na možnosti připojení mezi bránou a místní síťovou lokalitou v konfiguraci sítě po migraci představuje nově vytvořený prostředek označovaný jako Připojení v Resource Manageru. Všechny vlastnosti připojení odkazu v konfiguračních souborech sítě jsou zkopírované beze změny toohello nově vytvořený prostředek připojení. Připojení tooVNet virtuální síť v klasickém je dosaženo vytvořením dvou tunelových propojení IPsec toolocal sítě reprezentující hello virtuální sítě. Toto je připojení transformovaných tooVnet2Vnet typ v modelu resource manager bez nutnosti brány místní sítě. |

## <a name="changes-tooyour-automation-and-tooling-after-migration"></a>Změny tooyour automatizace a nástrojů po migraci
Jako součást migrace vašich prostředků z modelu nasazení Resource Manager modelu hello klasického nasazení toohello máte tooupdate existující automatizace nebo nástrojů tooensure pokračuje toowork po migraci hello.
