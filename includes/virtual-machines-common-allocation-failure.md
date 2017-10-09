
Pokud není Azure problém řešený v tomto článku, navštivte hello [fóra Azure na webu MSDN a Stack Overflow](https://azure.microsoft.com/support/forums/). Problém můžete zveřejnit na tyto fóra nebo too@AzureSupport na Twitteru. Navíc můžete soubor žádost o podporu Azure tak, že vyberete **získat podporu** na hello [podporu Azure](https://azure.microsoft.com/support/options/) lokality.

## <a name="general-troubleshooting-steps"></a>Obecné řešení potíží
### <a name="troubleshoot-common-allocation-failures-in-hello-classic-deployment-model"></a>Řešení potíží s běžné chyby v přidělení v modelu nasazení classic hello
Tyto kroky můžete vyřešit velký počet chyb alokace ve virtuálních počítačích:

* Změnit velikost hello virtuálních počítačů tooa jinou velikost virtuálního počítače.<br>
    Klikněte na tlačítko **procházet všechny** > **virtuálních počítačů (klasické)** > virtuálního počítače > **nastavení** > **velikost**. Podrobné pokyny najdete v tématu [změnit velikost virtuálního počítače hello](https://msdn.microsoft.com/library/dn168976.aspx).
* Odstranit všechny virtuální počítače z hello cloudové služby a znovu vytvořit virtuální počítače.<br>
    Klikněte na tlačítko **procházet všechny** > **virtuálních počítačů (klasické)** > virtuálního počítače > **odstranit**. Potom klikněte na **nový** > **výpočetní** > [bitovou kopii virtuálního počítače].

### <a name="troubleshoot-common-allocation-failures-in-hello-azure-resource-manager-deployment-model"></a>Řešení potíží s běžné chyby v přidělení v modelu nasazení Azure Resource Manager hello
Tyto kroky můžete vyřešit velký počet chyb alokace ve virtuálních počítačích:

* Zastavit (zrušit přidělení) všech virtuálních počítačích v hello nastavte stejné dostupnosti a pak restartujte každé z nich.<br>
    toostop: klikněte na tlačítko **skupiny prostředků** > vaší skupiny prostředků > **prostředky** > vaše skupina dostupnosti > **virtuální počítače** > virtuálního počítače >  **Zastavit**.
  
    Po zastavení všech virtuálních počítačů, vyberte hello první virtuální počítač a klikněte na tlačítko **spustit**.

## <a name="background-information"></a>Základní informace
### <a name="how-allocation-works"></a>Jak funguje přidělení
do clusterů do několika oddílů Hello serverů v datových centrech Azure. Za normálních okolností požadavku na přidělení dojde k pokusu o několik clustery, ale je možné, že určitá omezení z požadavek na přidělení hello vynutit hello platformy Azure tooattempt hello požadavku v pouze jeden cluster. V tomto článku budeme označovat toothis jako "definovaného tooa cluster." Obrázek 1 níže znázorňuje hello malá normální přidělení, které dojde k pokusu o několik clusterů. Obrázek 2 znázorňuje hello malá přidělení, má připnutý tooCluster 2, protože se jedná, je hostitelem hello existující cloudové služby CS_1 nebo dostupnost.
![Diagram přidělení](./media/virtual-machines-common-allocation-failure/Allocation1.png)

### <a name="why-allocation-failures-happen"></a>Proč nastat chyby v přidělení
Při požadavku na přidělení je vázaný tooa cluster, existuje vyšší pravděpodobnost selhávat toofind volných prostředků, protože fond k dispozici zdrojů hello je menší. Kromě toho pokud požadavek na přidělení definovaného tooa clusteru, ale tento cluster nepodporuje hello typ prostředku, který jste požadovali, vaši žádost se nezdaří i v případě, že má hello cluster uvolnění prostředků. Obrázek 3 níže znázorňuje hello případu, kdy připojené přidělení nezdaří, protože hello pouze candidate cluster nebude mít uvolnění prostředků. Obrázek 4 ukazuje hello případu, kdy připojené přidělení nezdaří, protože hello pouze candidate clusteru nepodporuje hello požadovaná velikost virtuálního počítače, i když má hello cluster uvolnění prostředků.

![Došlo k chybě definovaného přidělení](./media/virtual-machines-common-allocation-failure/Allocation2.png)

## <a name="detailed-troubleshoot-steps-specific-allocation-failure-scenarios-in-hello-classic-deployment-model"></a>Podrobné řešení potíží s kroky konkrétní přidělení selhání scénáře v modelu nasazení classic hello
Tady uvádíme běžné scénáře přidělení, které způsobí toobe požadavku přidělení připnutý. Můžeme vám podrobně každý scénář dále v tomto článku.

* Změnit velikost virtuálního počítače nebo přidat virtuální počítače nebo instance role tooan stávající cloudovou službu
* Restartování částečně zastavena (deallocated) virtuálních počítačů
* Restartujte plně zastaveném (deallocated) virtuálních počítačů
* Pracovní nebo produkční nasazení (platforma jako služba pouze)
* Skupina vztahů (blízkosti virtuálního počítače nebo služby)
* Na základě vztahů skupiny virtuální sítě

Jakmile se zobrazí chyba přidělení, najdete v části Pokud některé popsané scénáře hello vztahují tooyour chyby. Použijte Chyba přidělení hello vrácený hello platformy Azure tooidentify hello odpovídající scénář. Pokud je připnutý váš požadavek, odeberte některé z hello Připnutí omezení tooopen požadavku toomore clusterů, a tím zvýšit riziko hello přidělení úspěch.

Obecně platí, tak dlouho, dokud hello chyba neindikuje "hello požadovaná velikost virtuálního počítače nepodporuje", můžete vždy opakovat později, protože mohla být dost prostředků uvolněno v clusteru tooaccommodate hello vaši žádost. Pokud hello problém je, že tento hello požadovaná velikost virtuálního počítače nepodporuje, zkuste jinou velikost virtuálního počítače. Hello jedinou možností, jinak hodnota je tooremove hello Připnutí omezení.

Dvě běžné scénáře selhání jsou související tooaffinity skupiny. V posledních hello skupiny vztahů byla instance používané tooprovide těsné blízkosti tooVMs nebo služby nebo byly použité tooenable hello vytvoření virtuální sítě. S hello zavedením regionálních virtuálních sítí jsou skupiny vztahů už požadované toocreate virtuální sítě. S hello snížení latence sítě v infrastrukturu Azure se změnila skupiny vztahů toouse hello doporučení pro virtuální počítač nebo služba blízkosti.

Diagram 5 níže uvede hello taxonomii scénářů přidělení hello (připnutý).
![Taxonomii připojené přidělení](./media/virtual-machines-common-allocation-failure/Allocation3.png)

> [!NOTE]
> Chyba Hello uvedené v každém scénáři přidělení je zkrácené formě. Odkazovat toohello [vyhledávací řetězec chyby](#Error string lookup) pro podrobné informace o chybě řetězce.
> 
> 

## <a name="allocation-scenario-resize-a-vm-or-add-vms-or-role-instances-tooan-existing-cloud-service"></a>Přidělení scénář: Změna velikosti virtuálního počítače nebo přidání role virtuálních počítačů nebo instancí tooan stávající cloudovou službu
**Chyba**

Upgrade_VMSizeNotSupported nebo GeneralError

**Příčinu Připnutí clusteru**

A žádosti o tooresize virtuálního počítače nebo přidat virtuální počítač nebo role instance tooan existující cloudové služby má toobe na hello původní cluster, který je hostitelem hello stávající cloudovou službu. Vytváření nové cloudové služby umožňuje jiného clusteru, která má volných prostředků nebo podporuje hello velikost virtuálního počítače, který jste požadovali, toofind hello platformy Azure.

**Alternativní řešení**

Pokud je chyba hello Upgrade_VMSizeNotSupported *, zkuste jinou velikost virtuálního počítače. Pokud pomocí jinou velikost virtuálního počítače není možné, ale pokud je přijatelné toouse jiný virtuální adresy IP (VIP), vytvořte novou toohost služby cloud hello nového virtuálního počítače a přidejte hello nové cloudové služby toohello regionální virtuální síť hello stávající virtuální počítače se spuštěným systémem. Pokud vaše stávající cloudovou službu nepoužívá regionální virtuální síť, můžete přesto vytvořit novou virtuální síť pro hello novou cloudovou službu a pak připojit vaše [existující virtuální síť toohello nové virtuální sítě](https://azure.microsoft.com/blog/vnet-to-vnet-connecting-virtual-networks-in-azure-across-different-regions/). Další informace [regionálních virtuálních sítí](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/).

Pokud hello chyba GeneralError *, je pravděpodobné, že hello typ prostředku (například konkrétní velikost virtuálního počítače) podporuje hello clusteru, ale hello cluster nebude mít uvolnění prostředků momentálně hello. Podobné toohello nad scénáři přidat hello potřeby výpočetních prostředků procesem vytvoření nové cloudové služby (poznámka má hello novou cloudovou službu toouse různých virtuálních IP adres) a použít tooconnect regionální virtuální síť cloudové služby.

## <a name="allocation-scenario-restart-partially-stopped-deallocated-vms"></a>Scénář přidělení: restartování částečně zastavena (deallocated) virtuálních počítačů
**Chyba**

GeneralError *

**Příčinu Připnutí clusteru**

Částečné navrácení znamená (deallocated) jeden nebo více, ale ne všechny virtuální počítače v cloudové službě byla zastavena. Při zastavení (zrušit přidělení) virtuálního počítače, hello přidružené uvolnění prostředků. Restartování tohoto zastaveném (deallocated) virtuálního počítače je proto novou žádost o přidělení. Restartování virtuálních počítačů v částečně deallocated Cloudová služba je ekvivalentní tooadding virtuální počítače tooan stávající cloudovou službu. požadavek na přidělení Hello má toobe na hello původní cluster, který je hostitelem hello stávající cloudovou službu. Vytváření jiné cloudové služby umožňuje jiného clusteru, který má volný prostředek nebo podporuje hello velikost virtuálního počítače, který jste požadovali, toofind hello platformy Azure.

**Alternativní řešení**

Pokud je přijatelné toouse jiný virtuální IP adresy, odstraňte hello zastavena (deallocated) virtuálních počítačů (ale zachovat hello přidružené disky) a přidejte hello virtuální počítače zpět pomocí jiné cloudové služby. Použijte tooconnect regionální virtuální síť cloudové služby:

* Pokud vaše stávající cloudovou službu používá regionální virtuální síť, jednoduše přidejte hello nové cloudové služby toohello stejné virtuální síti.
* Pokud vaše stávající cloudovou službu nepoužívá regionální virtuální síť, vytvořit novou virtuální síť pro hello novou cloudovou službu a potom [připojit existující virtuální síť toohello nové virtuální sítě](https://azure.microsoft.com/blog/vnet-to-vnet-connecting-virtual-networks-in-azure-across-different-regions/). Další informace [regionálních virtuálních sítí](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/).

## <a name="allocation-scenario-restart-fully-stopped-deallocated-vms"></a>Scénář přidělení: restartování plně zastavena (deallocated) virtuálních počítačů
**Chyba**

GeneralError *

**Příčinu Připnutí clusteru**

Úplné navrácení znamená, že jste zastavili (nepřiřazeném) všech virtuálních počítačů z cloudové služby. přidělení Hello požadavků toorestart tyto virtuální počítače mají toobe na hello původní cluster, který je hostitelem hello cloudové služby. Vytváření nové cloudové služby umožňuje jiného clusteru, která má volných prostředků nebo podporuje hello velikost virtuálního počítače, který jste požadovali, toofind hello platformy Azure.

**Alternativní řešení**

Pokud je přijatelné toouse různých virtuálních IP adres, hello odstranit původní zastaví (deallocated) virtuálních počítačů (ale zachovat hello přidružené disky) a odstranit hello odpovídající cloudové služby (hello přidružené výpočetních prostředků uvolnila již při jeho zastavení (deallocated) Hello virtuálních počítačů). Vytvořte nový text tooadd cloudové služby hello virtuální počítače zpět.

## <a name="allocation-scenario-stagingproduction-deployments-platform-as-a-service-only"></a>Scénář přidělení: pracovní nebo produkční nasazení (platforma jako služba pouze)
**Chyba**

New_General * nebo New_VMSizeNotSupported *

**Příčinu Připnutí clusteru**

Hello pracovní a provozní nasazení hello cloudové služby jsou hostované v hello stejného clusteru. Když přidáte hello druhé nasazení, hello odpovídající požadavek na přidělení se pokusí v hello stejného clusteru, který je hostitelem hello prvním nasazení.

**Alternativní řešení**

Odstraňte hello prvním nasazení a hello původní cloudové služby a znovu nasaďte hello cloudové služby. Tato akce může být umístěn hello prvním nasazení v clusteru, který má dostatek volných prostředků toofit obou nasazení nebo v clusteru, který podporuje hello velikosti virtuálních počítačů, které jste požádali.

## <a name="allocation-scenario-affinity-group-vmservice-proximity"></a>Scénář přidělení: Skupina vztahů (blízkosti virtuálního počítače nebo služby)
**Chyba**

New_General * nebo New_VMSizeNotSupported *

**Příčinu Připnutí clusteru**

Všechny výpočetních prostředků je skupina vztahů přiřazené tooan svázané tooone clusteru. Nové žádosti o výpočetních prostředků v této skupině vztahů jsou aplikovány v hello stejný cluster, kde jsou hostované hello existující prostředky. To platí, jestli jsou vytvořeny nové prostředky hello prostřednictvím novou cloudovou službu nebo stávající cloudovou službu.

**Alternativní řešení**

Pokud skupiny vztahů je nezbytné, není pomocí skupiny vztahů nebo seskupení výpočetní prostředky do více skupin vztahů.

## <a name="allocation-scenario-affinity-group-based-virtual-network"></a>Scénář přidělení: na základě vztahů skupiny virtuální sítě
**Chyba**

New_General * nebo New_VMSizeNotSupported *

**Příčinu Připnutí clusteru**

Předtím, než byly zavedeny regionálních virtuálních sítí, jste požadované tooassociate virtuální sítě pomocí skupiny vztahů. V důsledku toho jsou vázány výpočetní prostředky, které jsou umístěny do skupiny vztahů hello stejné omezení, jak je popsáno v hello "přidělení scénář: Skupina vztahů (virtuálního počítače nebo služby blízkosti)" část výše. Hello výpočetní prostředky jsou vázané tooone clusteru.

**Alternativní řešení**

Pokud není nutné skupinu vztahů, vytvořte nový regionální virtuální síť pro hello nové prostředky, které přidáváte, a potom [připojit existující virtuální síť toohello nové virtuální sítě](https://azure.microsoft.com/blog/vnet-to-vnet-connecting-virtual-networks-in-azure-across-different-regions/). Další informace [regionálních virtuálních sítí](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/).

Alternativně můžete [migrace vaší virtuální sítě na základě vztahů skupiny tooa regionální virtuální síť](https://azure.microsoft.com/blog/2014/11/26/migrating-existing-services-to-regional-scope/)a poté znovu přidejte hello požadovaných prostředků.

## <a name="detailed-troubleshooting-steps-specific-allocation-failure-scenarios-in-hello-azure-resource-manager-deployment-model"></a>Podrobné kroky konkrétní přidělení selhání scénáře v modelu nasazení Azure Resource Manager hello řešení potíží
Tady uvádíme běžné scénáře přidělení, které způsobí toobe požadavku přidělení připnutý. Můžeme vám podrobně každý scénář dále v tomto článku.

* Změnit velikost virtuálního počítače nebo přidat virtuální počítače nebo instance role tooan stávající cloudovou službu
* Restartování částečně zastavena (deallocated) virtuálních počítačů
* Restartujte plně zastaveném (deallocated) virtuálních počítačů

Jakmile se zobrazí chyba přidělení, najdete v části Pokud některé popsané scénáře hello vztahují tooyour chyby. Použijte Chyba přidělení hello vrácený hello platformy Azure tooidentify hello odpovídající scénář. Pokud vaše žádost je vázaný tooan existující cluster, odeberte některé z hello Připnutí omezení tooopen požadavku toomore clusterů, a tím zvýšit riziko hello přidělení úspěch.

Obecně platí, tak dlouho, dokud hello chyba neindikuje "hello požadovaná velikost virtuálního počítače nepodporuje", můžete vždy opakovat později, protože mohla být dost prostředků uvolněno v clusteru tooaccommodate hello vaši žádost. Pokud hello problém je, že tento hello požadovaná velikost virtuálního počítače nepodporuje, níže najdete alternativní řešení.

## <a name="allocation-scenario-resize-a-vm-or-add-vms-tooan-existing-availability-set"></a>Přidělení scénář: Změna velikosti virtuálního počítače nebo přidat virtuální počítače tooan stávající sadu dostupnosti
**Chyba**

Upgrade_VMSizeNotSupported * nebo GeneralError *

**Příčinu Připnutí clusteru**

A žádosti o tooresize virtuálního počítače nebo přidat virtuální počítač tooan stávající sady dostupnosti. má toobe na hello původní cluster, který je hostitelem hello stávající sadu dostupnosti. Vytvoření nové sady dostupnosti. umožňuje jiného clusteru, která má volných prostředků nebo podporuje hello velikost virtuálního počítače, který jste požadovali, toofind hello platformy Azure.

**Alternativní řešení**

Pokud je chyba hello Upgrade_VMSizeNotSupported *, zkuste jinou velikost virtuálního počítače. Pokud pomocí jinou velikost virtuálního počítače není možné, zastavte všechny virtuální počítače ve skupině dostupnosti hello. Můžete změnit velikost hello hello virtuálního počítače, který bude přidělit hello tooa cluster virtuálních počítačů, která podporuje hello potřeby velikost virtuálního počítače.

Pokud hello chyba GeneralError *, je pravděpodobné, že hello typ prostředku (například konkrétní velikost virtuálního počítače) podporuje hello clusteru, ale hello cluster nebude mít uvolnění prostředků momentálně hello. Pokud hello virtuálních počítačů může být součástí jiné skupině dostupnosti, vytvořit nový virtuální počítač jiné skupině dostupnosti (v hello stejné oblasti). Tento nový virtuální počítač lze přidat toohello stejné virtuální síti.  

## <a name="allocation-scenario-restart-partially-stopped-deallocated-vms"></a>Scénář přidělení: restartování částečně zastavena (deallocated) virtuálních počítačů
**Chyba**

GeneralError *

**Příčinu Připnutí clusteru**

Částečné navrácení znamená, že jste zastavili (deallocated) jeden nebo více, ale ne všechny, virtuální počítače ve skupině dostupnosti nastavena. Při zastavení (zrušit přidělení) virtuálního počítače, hello přidružené uvolnění prostředků. Restartování tohoto zastaveném (deallocated) virtuálního počítače je proto novou žádost o přidělení. Restartování virtuálních počítačů v sadě dostupnosti částečně deallocated ekvivalentní tooadding existující dostupnosti virtuálních počítačů tooan nastavena. požadavek na přidělení Hello má toobe na hello původní cluster, který je hostitelem hello stávající sadu dostupnosti.

**Alternativní řešení**

Zastavte všechny virtuální počítače ve skupině před restartováním hello první dostupnosti hello. Tím bude zajištěno, že se nový pokus o přidělení spustit a, do nového clusteru lze vybrat s dostupné kapacity.

## <a name="allocation-scenario-restart-fully-stopped-deallocated"></a>Scénář přidělení: restartování plně zastavena (deallocated)
**Chyba**

GeneralError *

**Příčinu Připnutí clusteru**

Úplné navrácení znamená, že jste zastavili (nepřiřazeném) všech virtuálních počítačích v nastavení dostupnosti. požadavek na přidělení Hello toorestart těchto virtuálních počítačů se zaměří na všechny clustery, které podporují hello požadované velikosti.

**Alternativní řešení**

Vyberte nové tooallocate velikost virtuálního počítače. Pokud to nefunguje, zkuste to prosím znovu později.

## <a name="error-string-lookup"></a>Vyhledávací řetězec chyby
**New_VMSizeNotSupported***

"hello virtuálních počítačů velikost (nebo kombinace velikostí virtuálních počítačů) požadovaná tímto nasazením nejde zřídit kvůli omezení toodeployment požadavku. Pokud je to možné, zkuste uvolnit omezení, jako jsou třeba vazby virtuální sítě, nasazení tooa hostované služby není žádné další nasazení, které jsou v ní a tooa různých vztahů skupiny nebo bez skupin vztahů, nebo zkuste nasazení tooa jiné oblasti. "

**New_General***

"Přidělení selhalo; nelze toosatisfy omezení v požadavku. Hello požadované nasazení nové služby je vázané tooan skupin vztahů, nebo jeho cílem virtuální síť, nebo existuje stávajícího nasazení v této hostované služby. Některé z těchto podmínek omezuje nové nasazení toospecific hello Azure prostředky. Opakujte akci později nebo zkuste zmenšit velikost virtuálního počítače hello nebo počet instancí role. Případně pokud je to možné, odeberte hello výše uvedené omezení nebo vyzkoušejte nasazení v jiné oblasti tooa."

**Upgrade_VMSizeNotSupported***

"Nelze tooupgrade hello nasazení. Hello požadovaná velikost virtuálního počítače XXX nemusí být k dispozici v hello prostředích, která podporují existující nasazení hello. Prosím opakujte akci později, zkuste to znovu s jinou velikost virtuálního počítače nebo zmenšete počet instancí role nebo vytvořit nasazení pod prázdnou hostovanou službou s novou skupinou vztahů nebo bez vazby na skupinu vztahů."

**GeneralError***

"hello serveru došlo k vnitřní chybě. Zkuste provést požadavek hello." Nebo "Tooproduce neúspěšné přidělení pro službu hello."

