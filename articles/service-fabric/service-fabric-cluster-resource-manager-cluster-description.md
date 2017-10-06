---
title: aaaCluster Popis clusteru Resource Manager | Microsoft Docs
description: "Cluster Service Fabric popisující zadáním domén selhání, Upgrade domény, vlastnosti uzlu a uzel kapacity pro hello správce prostředků clusteru."
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 55f8ab37-9399-4c9a-9e6c-d2d859de6766
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: f2822075976bd54402af5ad56991b5b360dfb1d8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="describing-a-service-fabric-cluster"></a>Popisující service fabric cluster
Hello správce prostředků clusteru Service Fabric nabízí několik mechanismů pro popis clusteru. Během doby běhu hello clusteru Resource Manager používá toto informace tooensure vysokou dostupnost hello běžící v clusteru hello. Při vynucování tyto důležité pravidla, také pokusí toooptimize hello spotřeby prostředků v rámci clusteru hello.

## <a name="key-concepts"></a>Klíčové koncepty
Hello clusteru Resource Manager podporuje několik funkcí, které popisují clusteru:

* Domén selhání
* Domén upgradu
* Vlastnosti uzlu
* Uzel kapacity

## <a name="fault-domains"></a>Domény selhání
Domény selhání je všechny oblasti koordinované selhání. Jeden počítač je domény selhání (protože ho může selhat v jeho vlastní pro různé příčiny z power napájení selhání toodrive selhání toobad seskupování firmware). Počítače hello připojené toohello stejnému přepínači sítě Ethernet jsou v jedné doméně selhání, jako jsou počítače sdílení jednoho zdroje napájení nebo na jednom místě. Vzhledem k tomu, že je přirozené pro toooverlap chyb hardwaru, jsou ze své podstaty hierarchická domén selhání a jsou reprezentovány jako identifikátory URI v Service Fabric.

Je důležité, aby domén selhání jsou správně nastaveny vzhledem k tomu, že Service Fabric používá tato informace toosafely místní služby. Service Fabric nechce, že tooplace služeb tak, aby hello ztrátě domény selhání (způsobené hello selhání některé komponenty) způsobí, že služba toogo dolů. V prostředí Azure Service Fabric používá poskytnuté hello prostředí toocorrectly informace o doméně selhání hello hello nakonfigurujte hello uzly v clusteru hello vaším jménem. Pro samostatné služby infrastruktury, domén selhání jsou definovány v době hello je nastavený tento cluster hello 

> [!WARNING]
> Je důležité, že hello informace o doméně selhání, pokud je přesný tooService prostředků infrastruktury. Například předpokládejme, že uzly clusteru Service Fabric běží uvnitř 10 virtuálních počítačů běžících na pět fyzických hostitelích. V takovém případě i když se 10 virtuálních počítačů, je jenom 5 různých (nejvyšší úrovně) poruch domén. Sdílení hello způsobí, že stejném fyzickém hostiteli virtuálních počítačů tooshare hello stejné kořenová doména selhání, vzhledem k tomu, že hello prostředí virtuálních počítačů koordinované selhání, pokud se nezdaří jejich fyzického hostitele.  
>
> Vzhledem k tomu, že Service Fabric očekává hello doména selhání uzlu není toochange. Další mechanismy zajistit vysokou dostupnost hello virtuálních počítačů, jako například [HA – VMs](https://technet.microsoft.com/en-us/library/cc967323.aspx), použijte transparentní migraci virtuálních počítačů z jednoho hostitele tooanother. Tyto mechanismy nezadávejte překonfigurovat nebo oznámit hello spuštění kódu uvnitř hello virtuálních počítačů. Jako takový jsou **nepodporuje** jako prostředí pro spuštění Service Fabric clusterů. Service Fabric musí být hello pouze vysokou dostupnost technologie. Mechanismy, jako je migrace za provozu virtuálního počítače, sítě SAN, nebo jiné nejsou potřebné. Pokud se používá ve spojení s Service Fabric, tyto mechanismy _snížit_ aplikace dostupnost a spolehlivost vzhledem k tomu, že zavést další složitosti, přidejte centralizované zdroje selhání a využívat spolehlivost a strategie dostupnosti, které je v konfliktu s těmi, která v Service Fabric. 
>
>

V následujícím obrázku hello jsme barvu všechny hello entity, které přispívají tooFault domén a seznam všech hello různých domén selhání, který vést. V tomto příkladu máme datových centrech (dále jen "řadiče domény"), stojany ("R") a okna ("B"). V opačném případě pokud každý okno obsahuje více než jeden virtuální počítač, může dojít další vrstvu v hello hierarchii domény selhání.

<center>
![Uzly uspořádané prostřednictvím domén selhání][Image1]
</center>

Během doby běhu hello správce prostředků clusteru Service Fabric zvažuje hello domén selhání v clusteru hello a plány rozložení. Hello stavová repliky nebo bezstavové instance pro danou službu distribuují tak, aby byly v samostatných doménách selhání. Distribuci hello služby napříč doménami selhání zajistí, že hello dostupnost služby hello nejsou ohrožená selhání doména selhání na libovolné úrovni hierarchie hello.

Správce prostředků clusteru Service Fabric není stará počet vrstev v hello hierarchii domény selhání. Však pokusí tooensure, který hello ke ztrátě všech jednu část hello hierarchie nebude mít vliv na běžící v ní. 

Je vhodné, pokud existují hello stejný počet uzlů na každé úrovni podrobně hello hierarchii domény selhání. Pokud hello "stromu" domén selhání nevyváženou v clusteru, je ztěžuje hello toofigure správce prostředků clusteru se doporučené přidělení hello služeb. Imbalanced rozložení domén selhání znamenají, že hello ke ztrátě některých domén dopad hello dostupnost služeb víc než jiných domén. V důsledku toho odpojí hello správce prostředků clusteru mezi dva cíle: ho chce toouse hello počítače v této doméně "těžká" na jejich umístění služby a chce tooplace služby v jiných doménách, tak, aby hello ztrátě domény není způsobit problémy. 

Co imbalanced domén vypadat? V následujícím diagramu hello ukážeme dvě rozložení jiného clusteru. V prvním příkladu hello jsou rovnoměrně rozloženy hello uzly na hello domén selhání. V druhém příkladu hello jednu doménu selhání má mnoho více uzlů než hello jiných domén selhání. 

<center>
![Dvě různé clusteru rozložení][Image2]
</center>

V Azure hello výběru z nich obsahuje doména selhání uzlu je spravovaná. Ale v závislosti na hello počet uzlů, které můžete zřídit můžete stále skončili s domén selhání s více uzly v nich než jiné. Předpokládejme například že máte pět domén selhání v clusteru hello, ale zřídit sedmi uzlů pro daný typ uzlu. V takovém případě hello první dva domén selhání skončili s více uzly. Pokud budete pokračovat toodeploy další NodeTypes s pouze několik instancí, získá zhoršení hello problém. Proto se doporučuje tuto hello počet uzlů v každém typu uzlu je násobkem hello počet domén selhání.

## <a name="upgrade-domains"></a>Domén upgradu
Další funkce, která pomáhá hello správce prostředků clusteru služby prostředků infrastruktury jsou upgradu domény pochopit hello rozložení hello clusteru. Domén upgradu definovat sadu uzlů, které jsou upgradovány na hello stejnou dobu. Nápověda hello upgradu domény správce prostředků clusteru pochopit a orchestraci management operace, jako je upgrady.

Upgradu domény jsou mnohem jako domén selhání, ale s několik hlavní rozdíly. Nejprve oblasti selhání koordinované hardwaru definování domén selhání. Upgradovacích domén, na druhé straně Dobrý den, jsou definovány pomocí zásad. Získat toodecide kolik chcete místo ho se závisí na prostředí hello. Můžete mít tolik upgradu domény jako uzly. Další rozdíl mezi domén selhání a upgradu domény je, že Upgrade domény nejsou hierarchické. Místo toho se více podobají prostřednictvím jednoduchých značek. 

Hello následující diagram znázorňuje tři že upgradu domény jsou rozdělené mezi tři domény selhání. Také ukazuje jeden možné umístění pro tři různé repliky stavové služby, kde každý skončilo v různých selhání a upgradu domény. Toto umístění umožňuje hello ztrátě domény selhání při uprostřed hello upgrade služby a ještě jednu kopii hello kód a data.  

<center>
![Umístění s doménách selhání a upgradu][Image3]
</center>

Existují výhody a nevýhody toohaving velký počet domén upgradu. Další domény upgradu znamená každého kroku upgradu hello je podrobnější a ovlivňuje menší počet uzlů nebo služby. V důsledku toho méně služeb mají toomove současně, představení méně změn do systému hello. To obvykle tooimprove spolehlivost, protože méně hello služby je ovlivněn jakýkoli problém zavedená během upgradu hello. Další domény upgradu také znamená, že potřebovat méně dostupné vyrovnávací paměti na jiných uzlech toohandle hello dopad hello upgradu. Například pokud máte pět upgradu domén, hello uzly v každé jsou zpracování zhruba 20 % z provozu. Pokud potřebujete tootake dolů upgradu domény pro účely upgradu, aby byla zátěž obvykle musí toogo někde. Vzhledem k tomu, že máte čtyři zbývající domény upgradu, každý musí mít prostor pro přibližně 5 % celkového provozu hello. Další domény upgradu znamená, že budete potřebovat méně vyrovnávací paměti hello uzlů v clusteru hello. Zvažte například, pokud jste měli 10 upgradu domén místo. V takovém případě každý UD by pouze zpracovávat přibližně 10 % celkového provozu hello. Při upgradu kroky prostřednictvím hello clusteru, každou doménu potřebovat pouze toohave prostor pro přibližně 1.1 % celkového provozu hello. Další domény upgradu obvykle umožňují toorun uzly s vyšší využití, vzhledem k tomu, že budete potřebovat méně rezervované kapacity. Hello totéž platí pro domény selhání.  

Nevýhodou Hello mnoho upgradu domény je, že upgrady mívají tootake delší. Service Fabric čeká krátké době po doména Upgrade a provádí kontroly před zahájením tooupgrade hello následujícím kódem. Tyto zpoždění povolit zjišťuje problémy, které jsou zavedené službou hello upgrade před provedením upgradu hello. kompromis Hello je přijatelné, protože chybný změny brání by to ovlivnilo příliš mnoho hello služby najednou.

Příliš málo upgradu domény má mnoho záporné vedlejší účinky – při každé jednotlivé upgradu domény je mimo provoz a upgraduje velkou část celkové kapacity není k dispozici. Například pokud máte pouze tři domény upgradu můžete trvá dolů o 1/3 celkové služby nebo kapacity clusteru současně. Značná část služby najednou s dolů není žádoucí, vzhledem k tomu, že máte dostatečnou kapacitu pro toohave v hello zbytek úlohy clusteru toohandle hello. Zachování této vyrovnávací paměti znamená, že při běžném provozu tyto uzly jsou menší načíst, než by byly jinak. Tím se zvyšuje hello náklady na provozování vaší služby.

Neexistuje žádné skutečné limit toohello celkový počet selhání nebo upgradu domény v prostředí nebo omezení toho, jak se překrývají. Ale nutné dodat, existuje několik běžných vzorů:

- Domén selhání a upgradu domény namapovat 1:1
- Jednu doménu upgradu na uzel (fyzické nebo virtuální instance operačního systému)
- Model "prokládané" nebo "matice", kde hello domén selhání a upgradu domény formuláři matice se počítače, které obvykle běží dolů úhlopříčkami hello

<center>
![Selhání a upgradu domény rozložení][Image4]
</center>

Není definován že žádný nejvhodnější zodpovědět které toochoose rozložení, každá z nich má některé výhody a nevýhody. Například hello 1FD:1UD model je jednoduchý tooset nahoru. Hello 1 doméně pro Upgrade na uzlu modelu je většina jako osoby, které se používají pro. Během upgradu se aktualizuje každý uzel nezávisle. Tato funkce je toohow malé sady počítačů byly upgradovány ručně v posledních hello. 

Nejběžnější model Hello je hello FD/UD matice, kde hello FDs a UDs vytvoří tabulku a uzly jsou umístěny podél hello diagonálních spouštění. Toto je model hello používá ve výchozím nastavení v clusterů Service Fabric v Azure. U clusterů s uzly mnoho všechno, co skončilo vyhledávání jako vzor hustých matice hello výše.

## <a name="fault-and-upgrade-domain-constraints-and-resulting-behavior"></a>Selhání a upgradu domény omezení a výsledné chování
Hello správce prostředků clusteru zpracovává hello desire tookeep služby rovnoměrně rozdělen mezi selhání a upgradu domény jako omezení. Můžete najít další informace o omezení v [v tomto článku](service-fabric-cluster-resource-manager-management-integration.md). Hello omezení stavu selhání a upgradu domény: "pro oddíl dané služby by nikdy existovat rozdíl *větší než jedna* v hello počet objektů služby (instance bezstavové služby nebo stavové služby repliky) mezi dvěma doménami." Tím se zabrání určité přesune nebo ujednání, které porušují toto omezení.

Podívejme se na jedním z příkladů. Řekněme, že se musí na cluster se šesti uzlů, nakonfigurovaný s pěti domén selhání a pět upgradu domény.

|  | FD0 | FD1 | FD2: | FD3 | FD4 |
| --- |:---:|:---:|:---:|:---:|:---:|
| **UD0** |N1 | | | | |
| **UD1** |N6 |N2 | | | |
| **UD2** | | |N3 | | |
| **UD3** | | | |N4 | |
| **UD4** | | | | |N5 |

Nyní Řekněme, že jsme pěti vytvořit službu s TargetReplicaSetSize (nebo bezstavové služby na InstanceCount). repliky Hello zobrazovat N1 N5. Ve skutečnosti N6 se nikdy nepoužívá bez ohledu na to, kolik služby, jako to, které vytvoříte. Ale proč? Podívejme se na hello rozdíl mezi hello aktuální rozložení a co by mohlo dojít, pokud je zvoleno N6.

Tady je hello rozložení jsme tu a hello celkového počtu replik na selhání a upgradu domény:

|  | FD0 | FD1 | FD2: | FD3 | FD4 | UDTotal |
| --- |:---:|:---:|:---:|:---:|:---:|:---:|
| **UD0** |R1 | | | | |1 |
| **UD1** | |R2 | | | |1 |
| **UD2** | | |R3 | | |1 |
| **UD3** | | | |R4 | |1 |
| **UD4** | | | | |R5 |1 |
| **FDTotal** |1 |1 |1 |1 |1 |- |

Toto rozložení jsou rovnoměrně z hlediska uzlů na doméně selhání a upgradu domény. Toto pravidlo jsou rovnoměrně také z hlediska hello počet replik na selhání a upgradu domény. Každá doména má hello stejný počet uzlů a hello stejný počet replik.

Teď se podíváme, co by se stalo jsme použití N6 místo N2. Jak by hello repliky distribuována pak?

|  | FD0 | FD1 | FD2: | FD3 | FD4 | UDTotal |
| --- |:---:|:---:|:---:|:---:|:---:|:---:|
| **UD0** |R1 | | | | |1 |
| **UD1** |R5 | | | | |1 |
| **UD2** | | |R2 | | |1 |
| **UD3** | | | |R3 | |1 |
| **UD4** | | | | |R4 |1 |
| **FDTotal** |2 |0 |1 |1 |1 |- |

Toto rozložení porušuje naše definice hello omezení domény selhání. FD0 má dvě repliky, zatímco FD1 má nula, které hello rozdíl mezi FD0 a FD1 celkem dva. Toto uspořádání Hello správce prostředků clusteru není povoleno. Podobně pokud jsme zachyceny N2 a N6 (namísto N1 a N2) nám se získat:

|  | FD0 | FD1 | FD2: | FD3 | FD4 | UDTotal |
| --- |:---:|:---:|:---:|:---:|:---:|:---:|
| **UD0** | | | | | |0 |
| **UD1** |R5 |R1 | | | |2 |
| **UD2** | | |R2 | | |1 |
| **UD3** | | | |R3 | |1 |
| **UD4** | | | | |R4 |1 |
| **FDTotal** |1 |1 |1 |1 |1 |- |

Toto rozložení jsou rovnoměrně z hlediska domén selhání. Ale nyní ho porušuje omezení upgradu domény hello. To je proto UD0 má nulové repliky, zatímco UD1 má dva. Toto rozložení proto je neplatná a nebude vybráno tím hello správce prostředků clusteru. 

## <a name="configuring-fault-and-upgrade-domains"></a>Konfigurace selhání a upgradu domény
Definování domén selhání a upgradu domény se provádí automaticky v Azure hostovaná nasazení Service Fabric. Service Fabric převezme a používá informace o prostředí hello z Azure.

Pokud vytváříte vlastní cluster (nebo má toorun topologii konkrétní vývojem), můžete zadat informace o doméně selhání a upgradu domény hello sami. V tomto příkladu jsme definovali místní vývojový cluster devět uzlu, který zahrnuje tři "datových centrech" (každý má tři stojany). Tento cluster má také tři domény upgradu rozdělená mezi tyto tři datových centrech. Zde je příklad konfigurace hello: 

ClusterManifest.xml

```xml
  <Infrastructure>
    <!-- IsScaleMin indicates that this cluster runs on one-box /one single server -->
    <WindowsServer IsScaleMin="true">
      <NodeList>
        <Node NodeName="Node01" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType01" FaultDomain="fd:/DC01/Rack01" UpgradeDomain="UpgradeDomain1" IsSeedNode="true" />
        <Node NodeName="Node02" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType02" FaultDomain="fd:/DC01/Rack02" UpgradeDomain="UpgradeDomain2" IsSeedNode="true" />
        <Node NodeName="Node03" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType03" FaultDomain="fd:/DC01/Rack03" UpgradeDomain="UpgradeDomain3" IsSeedNode="true" />
        <Node NodeName="Node04" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType04" FaultDomain="fd:/DC02/Rack01" UpgradeDomain="UpgradeDomain1" IsSeedNode="true" />
        <Node NodeName="Node05" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType05" FaultDomain="fd:/DC02/Rack02" UpgradeDomain="UpgradeDomain2" IsSeedNode="true" />
        <Node NodeName="Node06" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType06" FaultDomain="fd:/DC02/Rack03" UpgradeDomain="UpgradeDomain3" IsSeedNode="true" />
        <Node NodeName="Node07" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType07" FaultDomain="fd:/DC03/Rack01" UpgradeDomain="UpgradeDomain1" IsSeedNode="true" />
        <Node NodeName="Node08" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType08" FaultDomain="fd:/DC03/Rack02" UpgradeDomain="UpgradeDomain2" IsSeedNode="true" />
        <Node NodeName="Node09" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType09" FaultDomain="fd:/DC03/Rack03" UpgradeDomain="UpgradeDomain3" IsSeedNode="true" />
      </NodeList>
    </WindowsServer>
  </Infrastructure>
```

pomocí souboru ClusterConfig.json pro samostatné nasazení

```json
"nodes": [
  {
    "nodeName": "vm1",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc1/r0",
    "upgradeDomain": "UD1"
  },
  {
    "nodeName": "vm2",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc1/r0",
    "upgradeDomain": "UD2"
  },
  {
    "nodeName": "vm3",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc1/r0",
    "upgradeDomain": "UD3"
  },
  {
    "nodeName": "vm4",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc2/r0",
    "upgradeDomain": "UD1"
  },
  {
    "nodeName": "vm5",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc2/r0",
    "upgradeDomain": "UD2"
  },
  {
    "nodeName": "vm6",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc2/r0",
    "upgradeDomain": "UD3"
  },
  {
    "nodeName": "vm7",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc3/r0",
    "upgradeDomain": "UD1"
  },
  {
    "nodeName": "vm8",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc3/r0",
    "upgradeDomain": "UD2"
  },
  {
    "nodeName": "vm9",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc3/r0",
    "upgradeDomain": "UD3"
  }
],
```

> [!NOTE]
> Při definování clusterů pomocí Azure Resource Manager, jsou přiřazeny domén selhání a upgradu domény Azure. Proto hello definice typů uzlů a sady škálování virtuálního počítače do šablony Azure Resource Manager nezahrnuje doména selhání nebo informace o upgradu domény.
>

## <a name="node-properties-and-placement-constraints"></a>Vlastnosti uzlu a omezení umístění
V některých případech (v faktu, většinu času hello) budete tooensure toowant, který určité úlohy spustit pouze v určitých typů uzlů v clusteru hello. Pro některé zatížení může například vyžadovat grafickými procesory nebo SSD a jiné nikoli. Skvělé příklad cílových hardwaru tooparticular úlohy je téměř každé n vrstvá architektura odhlašování došlo. Některé počítače slouží jako hello front-endu nebo API obsluhující strana hello aplikace a jsou zveřejněné toohello klienty nebo hello Internetu. Různé počítače, často se různé hardwarové prostředky, zpracování pracovní hello hello výpočtů nebo úložiště vrstev. Obvykle se jedná o _není_ přímo vystavena tooclients nebo hello Internetu. Service Fabric očekává, že jsou případy, kde konkrétní úlohy musí toorun na konkrétní hardwarové konfigurace. Například:

* stávající vícevrstvé aplikace byla "zrušeno a zapuštěno" do prostředí Service Fabric
* zatížení chce toorun na konkrétní hardware pro výkon, škálování nebo z důvodů zabezpečení izolace
* Úloha by měla být izolované od ostatních úloh pro zásady nebo prostředek spotřebu důvody

toosupport tyto řazení konfigurací služby prostředků infrastruktury je první třídy představu o značky, které můžou být použité toonodes. Tyto značky se nazývají **vlastnosti uzlu**. **Omezení umístění** jsou příkazy hello připojené tooindividual služby, které vyberete pro jednu nebo více vlastností uzlu. Omezení umístění definovat, které by měly běžet služby. Sada Hello omezení je rozšiřitelný – může fungovat všechny dvojice klíč/hodnota. 

<center>
![Různé rozložení zatížení clusteru][Image5]
</center>

### <a name="built-in-node-properties"></a>Součástí vlastnosti uzlu
Service Fabric definuje některé výchozí uzel vlastnosti, které lze použít automaticky bez nutnosti toodefine uživatele hello je. Hello výchozí vlastnosti definované v každém uzlu jsou hello **NodeType** a hello **NodeName**. Můžete napsat tak například omezení umístění jako `"(NodeType == NodeType03)"`. Obecně našli jsme NodeType toobe jedna z vlastností hello nejčastěji používají. Je vhodné vzhledem k tomu, že odpovídá 1:1 s typem počítače. Každý typ počítače odpovídá tooa typu úlohy v tradiční n vrstvá aplikaci.

<center>
![Omezení umístění a vlastnosti uzlu][Image6]
</center>

## <a name="placement-constraint-and-node-property-syntax"></a>Omezení umístění a syntaxe vlastnosti uzlu 
Hodnota Hello zadaná ve vlastnosti uzlu hello může být řetězec, bool, nebo podepsané dlouho. příkaz Hello u služby hello se označuje jako umístění *omezení* vzhledem k tomu, že omezuje kde hello služba může běžet v clusteru hello. Hello omezení může být jakékoli Boolean příkaz, který funguje na vlastnosti hello jiného uzlu v clusteru hello. Hello platný selektory v těchto příkazech boolean jsou:

1) Podmíněné kontroly pro vytvoření konkrétní příkazy

| Příkaz | Syntaxe |
| --- |:---:|
| "rovno" | "==" |
| "není rovno" | "!=" |
| "větší než" | ">" |
| "větší než nebo rovno" | ">=" |
| "menší než" | "<" |
| "menší než nebo rovno" | "<=" |

2) Logická hodnota příkazy pro seskupování a logické operace

| Příkaz | Syntaxe |
| --- |:---:|
| "a" | "&&" |
| "nebo" | "&#124;&#124;" |
| "Ne" | "!" |
| "skupiny jako jediný příkaz" | "()" |

Zde jsou některé příklady příkazů základní omezení.

  * `"Value >= 5"`
  * `"NodeColor != green"`
  * `"((OneProperty < 100) || ((AnotherProperty == false) && (OneProperty >= 100)))"`

Služba hello umístit na něm může mít pouze uzly, kde hello celkové příkaz omezení umístění vyhodnotí příliš "True". Uzly, které nemají vlastnost definovanou se neshodují žádné omezení umístění obsahující tuto vlastnost.

Řekněme, že hello následující uzlu, které byly definovány vlastnosti pro typ daného uzlu:

ClusterManifest.xml

```xml
    <NodeType Name="NodeType01">
      <PlacementProperties>
        <Property Name="HasSSD" Value="true"/>
        <Property Name="NodeColor" Value="green"/>
        <Property Name="SomeProperty" Value="5"/>
      </PlacementProperties>
    </NodeType>
```

pomocí souboru ClusterConfig.json pro samostatné nasazení nebo Template.json pro Azure hostované clustery. 

> [!NOTE]
> V uzlu hello šablony Azure Resource Manager je obvykle parametry typu. Může vypadat třeba "[parameters('vmNodeType1Name')]" místo "NodeType01".
>

```json
"nodeTypes": [
    {
        "name": "NodeType01",
        "placementProperties": {
            "HasSSD": "true",
            "NodeColor": "green",
            "SomeProperty": "5"
        },
    }
],
```

Můžete vytvořit umístění služby *omezení* pro službu, jako následujícím způsobem:

C#

```csharp
FabricClient fabricClient = new FabricClient();
StatefulServiceDescription serviceDescription = new StatefulServiceDescription();
serviceDescription.PlacementConstraints = "(HasSSD == true && SomeProperty >= 4)";
// add other required servicedescription fields
//...
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

Prostředí PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceType -Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementConstraint "HasSSD == true && SomeProperty >= 4"
```

Pokud jsou všechny uzly NodeType01 platné, můžete také vybrat daný typ uzlu s omezením hello "(NodeType == NodeType01)".

Jedním z nástrojů věcí hello o omezení umístění služby je, aby bylo možné aktualizovat dynamicky za běhu. Proto pokud potřebujete, můžete v clusteru hello pohyb služby, přidat a odebrat požadavky atd. Service Fabric postará zajistit, že služba hello zůstává a k dispozici i když jsou vytvářeny těchto typů změn.

C#:

```csharp
StatefulServiceUpdateDescription updateDescription = new StatefulServiceUpdateDescription();
updateDescription.PlacementConstraints = "NodeType == NodeType01";
await fabricClient.ServiceManager.UpdateServiceAsync(new Uri("fabric:/app/service"), updateDescription);
```

Prostředí PowerShell:

```posh
Update-ServiceFabricService -Stateful -ServiceName $serviceName -PlacementConstraints "NodeType == NodeType01"
```

Pro všechny různé služby s názvem instance jsou zadány omezení umístění. Aktualizace vždy provést místo hello (Přepsat) byl dříve zadán.

Definice clusteru Hello definuje vlastnosti hello na uzlu. Změna vlastnosti uzlu vyžaduje s upgradem konfigurace clusteru. Upgrade vlastnosti uzlu vyžaduje jeho nové vlastnosti tooreport toorestart každý uzel došlo k chybě. Tyto vrácení upgradů Service Fabric spravuje.

## <a name="describing-and-managing-cluster-resources"></a>Popisující a Správa prostředků clusteru
Jedním z nejdůležitějších úloh žádné orchestrator je toohelp hello spravovat spotřeby prostředků v clusteru hello. Správa prostředků clusteru může zahrnovat několik různých věcí. Nejprve existuje zajišťuje, že počítače nejsou přetížené. To znamená, a ujistěte se, že počítače nejsou spuštěné další služby, než se může zpracovat. Druhou je, že vyrovnávání a optimalizace, což je důležité toorunning služby efektivně. Cenově platné nebo výkonu nabídky citlivé služeb nemůže povolit některé uzly toobe aktivní zase cold. Aktivní uzly vést tooresource kolizí nebo nízký výkon a cold uzly představují ke znehodnocení části prostředků a vyšší náklady. 

Service Fabric představuje prostředky jako `Metrics`. Metriky jsou všechny logickou nebo fyzickou prostředků, které chcete toodescribe tooService prostředků infrastruktury. Příklady metrik věci jako "WorkQueueDepth" nebo "MemoryInMb". Informace o hello fyzické prostředky, které můžou řídit Service Fabric na uzlech naleznete v tématu [zásad správného řízení prostředků](service-fabric-resource-governance.md). Informace o konfiguraci vlastní metriky a jejich použití naleznete v tématu [v tomto článku](service-fabric-cluster-resource-manager-metrics.md)

Metriky se liší od umísťováním omezení a vlastnosti uzlu. Vlastnosti uzlu jsou statické popisovače hello v samotných uzlech. Metriky popisují prostředky, které mají uzly a že využívat služby při spuštění na uzlu. Vlastnost uzlu může být "HasSSD" a může být nastaven tootrue nebo hodnotu false. Hello množství místa, které jsou k dispozici v této SSD a kolik je využívána služby by metrika jako "DriveSpaceInMb". 

Je důležité toonote, stejně jako omezení umístění a vlastnosti uzlu hello správce prostředků clusteru Service Fabric není pochopit, jaké hello názvy střední metriky hello. Metriky názvy jsou jenom řetězce. Je dobrým zvykem toodeclare jednotky jako součást hello metriky názvy, které vytvoříte, když může být nejednoznačný.

## <a name="capacity"></a>Kapacita
Pokud jste měli vypnuté všechny prostředků *vyrovnávání*, správce prostředků clusteru Service Fabric stále zajistí, že žádný uzel skončila přes své kapacity. Správa přetečení kapacity je možné, pokud hello clusteru je málo místa nebo je větší než libovolného uzlu hello zatížení. Kapacita je jiné *omezení* této hello správce prostředků clusteru používá toounderstand kolik prostředku a uzel má. Zbývající kapacity jsou také sledovány hello clusteru jako celek. Z hlediska metriky jsou vyjádřeny hello kapacitu a spotřeba hello na úrovni služby hello. Tak například metrika hello může být "ClientConnections" a daný uzel je pravděpodobně kapacity pro "ClientConnections" z 32768. Další uzly může mít jiné omezení některé služby spuštěné v tomto uzlu můžete například ho aktuálně spotřebovává 32256 metrika hello "ClientConnections".

Během doby běhu hello správce prostředků clusteru sleduje zbývající kapacity v clusteru hello a na uzlech. V pořadí tootrack odečítá kapacity hello správce prostředků clusteru využití každé služby z uzlu kapacity, kde je spuštěna služba hello. Tyto informace a hello správce prostředků clusteru Service Fabric nepřijdete na to, kde tooplace nebo přesunout repliky tak, aby uzly si projít kapacity.

<center>
![Uzly clusteru a kapacity][Image7]
</center>

C#:

```csharp
StatefulServiceDescription serviceDescription = new StatefulServiceDescription();
ServiceLoadMetricDescription metric = new ServiceLoadMetricDescription();
metric.Name = "ClientConnections";
metric.PrimaryDefaultLoad = 1024;
metric.SecondaryDefaultLoad = 0;
metric.Weight = ServiceLoadMetricWeight.High;
serviceDescription.Metrics.Add(metric);
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

Prostředí PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton –Metric @("ClientConnections,High,1024,0)
```

Můžete zjistit kapacity definované v manifestu clusteru hello:

ClusterManifest.xml

```xml
    <NodeType Name="NodeType03">
      <Capacities>
        <Capacity Name="ClientConnections" Value="65536"/>
      </Capacities>
    </NodeType>
```

pomocí souboru ClusterConfig.json pro samostatné nasazení nebo Template.json pro Azure hostované clustery. 

```json
"nodeTypes": [
    {
        "name": "NodeType03",
        "capacities": {
            "ClientConnections": "65536",
        }
    }
],
```

Běžně služby načíst dynamicky změny. Řekněme, že zatížení repliky "ClientConnections" změnila z 1024 too2048, ale hello uzlu běžel následně pouze měl 512 kapacita zbývající pro tuto metriku. Nyní repliky nebo instance umístění je neplatná, protože v tomto uzlu není dostatek místa. Hello správce prostředků clusteru má tookick a získat hello uzel zpět níže kapacity. Snižuje zátěž hello uzlu, který je nad kapacity přesunutím jeden nebo víc hello replik nebo instancí z tohoto uzlu tooother uzlů. Při přesunu repliky, hello správce prostředků clusteru pokusí toominimize náklady hello pohybů tyto. Náklady na přesunutí popsané v [v tomto článku](service-fabric-cluster-resource-manager-movement-cost.md) a více o hello je správce prostředků clusteru vyrovnává strategie a pravidla je popsaný [zde](service-fabric-cluster-resource-manager-metrics.md).

## <a name="cluster-capacity"></a>Kapacita clusteru
Jak tedy hello správce prostředků clusteru Service Fabric zachovat hello celkové clusteru z příliš zaplnění? Dobře s dynamického zatížení, není k dispozici mnoho ho provést. Služby mohou mít jejich zatížení Špička nezávisle na akce provedené hello správce prostředků clusteru. Cluster s dostatek prostoru dnes v důsledku toho může být underpowered, když jste se famous zítra. Ale nutné dodat, nejsou některé ovládací prvky, které jsou zaručená v tooprevent problémy. Hello prvním, co můžeme udělat je zabránit hello vytvoření nové úlohy, které by způsobily hello clusteru toobecome úplná.

Řekněme, že vytvoření bezstavové služby a obsahuje některé zatížení s ním spojená. Řekněme, že služba hello záleží metrika "DiskSpaceInMb" hello. Dále předpokládejme, že se jedná o probíhající tooconsume pět jednotky "DiskSpaceInMb" pro všechny instance služby hello. Chcete toocreate tři instance služby hello. Výborně! Tak, aby znamená, že potřebujeme 15 jednotky toobe "DiskSpaceInMb" k dispozici v clusteru hello abychom vás tooeven být schopný toocreate tyto instance služby. Hello správce prostředků clusteru průběžně vypočítá hello kapacitu a využití každé metriky tak může zjistit hello zbývající kapacity v clusteru hello. Pokud není k dispozici dostatek místa, vytvořte hello správce prostředků clusteru odmítne hello volání služby.

Vzhledem k tomu, že požadavek hello existuje pouze to, že se 15 jednotky k dispozici, může být tento prostor přiděleno mnoha různými způsoby. Například může existovat jednu jednotku zbývající kapacity v 15 rozdílných uzlech, nebo tři zbývající jednotky kapacity na pět různých uzlech. Pokud hello správce prostředků clusteru můžete přeuspořádat věcí, takže na tři uzly je k dispozici pět jednotky, umístí hello služby. Změna uspořádání hello clusteru je obvykle možné, pokud hello cluster je téměř plná, nebo z nějakého důvodu nelze konsolidovat hello stávající služby.

## <a name="buffered-capacity"></a>Kapacita ve vyrovnávací paměti
Kapacita ve vyrovnávací paměti je další funkcí hello správce prostředků clusteru. Umožňuje rezervace některé část hello celkové kapacity uzlu. Tato kapacity vyrovnávací paměť je pouze použité tooplace služby během upgradu a selhání uzlu. Kapacita ve vyrovnávací paměti je zadán globálně za Metrika pro všechny uzly. Hello hodnotu, kterou vyberete pro hello rezervované kapacity je funkce hello počtu selhání a upgradu domén, budete mít v clusteru hello. Další selhání a upgradu domény znamená, můžete si vybrat nižší číslo pro vaše kapacita ve vyrovnávací paměti. Pokud máte více domén, můžete očekávat snížení objemu toobe vašeho clusteru k dispozici během upgradu a selhání. Určení uložená do vyrovnávací paměti kapacity má smysl pouze, pokud máte také zadaný hello uzlu kapacity pro metriku.

Tady je příklad, jak toospecify do vyrovnávací paměti kapacity:

ClusterManifest.xml

```xml
        <Section Name="NodeBufferPercentage">
            <Parameter Name="SomeMetric" Value="0.15" />
            <Parameter Name="SomeOtherMetric" Value="0.20" />
        </Section>
```

pomocí souboru ClusterConfig.json pro samostatné nasazení nebo Template.json pro Azure hostované clustery:

```json
"fabricSettings": [
  {
    "name": "NodeBufferPercentage",
    "parameters": [
      {
          "name": "SomeMetric",
          "value": "0.15"
      },
      {
          "name": "SomeOtherMetric",
          "value": "0.20"
      }
    ]
  }
]
```

Vytvoření nové služby Hello selže, když hello clusteru je mimo ve vyrovnávací paměti kapacity pro metriku. Brání hello vytvoření nové služby toopreserve hello vyrovnávací paměti zajistí, že upgrady a selhání nezpůsobí, že uzly toogo přes kapacity. Kapacita ve vyrovnávací paměti je nepovinný, ale doporučuje se v jakékoli clusteru, který definuje kapacity pro metriku.

Hello správce prostředků clusteru zpřístupní tyto informace zatížení. Pro jednotlivé metriky tyto informace zahrnují: 
  - Nastavení kapacity Hello do vyrovnávací paměti
  - Celková kapacita Hello
  - aktuální spotřeba Hello
  - jestli je považován za jednotlivé metriky vyrovnáváním nebo ne
  - Statistika týkající se hello směrodatné odchylky
  - Hello uzly, které mají hello většinu a minimálně zatížení  
  
Níže vidíte příklad tento výstup:

```posh
PS C:\Users\user> Get-ServiceFabricClusterLoadInformation
LastBalancingStartTimeUtc : 9/1/2016 12:54:59 AM
LastBalancingEndTimeUtc   : 9/1/2016 12:54:59 AM
LoadMetricInformation     :
                            LoadMetricName        : Metric1
                            IsBalancedBefore      : False
                            IsBalancedAfter       : False
                            DeviationBefore       : 0.192450089729875
                            DeviationAfter        : 0.192450089729875
                            BalancingThreshold    : 1
                            Action                : NoActionNeeded
                            ActivityThreshold     : 0
                            ClusterCapacity       : 189
                            ClusterLoad           : 45
                            ClusterRemainingCapacity : 144
                            NodeBufferPercentage  : 10
                            ClusterBufferedCapacity : 170
                            ClusterRemainingBufferedCapacity : 125
                            ClusterCapacityViolation : False
                            MinNodeLoadValue      : 0
                            MinNodeLoadNodeId     : 3ea71e8e01f4b0999b121abcbf27d74d
                            MaxNodeLoadValue      : 15
                            MaxNodeLoadNodeId     : 2cc648b6770be1bc9824fa995d5b68b1
```

## <a name="next-steps"></a>Další kroky
* Informace o hello architektura a informačního toku v rámci hello správce prostředků clusteru, podívejte se na [v tomto článku](service-fabric-cluster-resource-manager-architecture.md)
* Definování defragmentace metriky je jedním ze způsobů tooconsolidate zatížení na uzlech místo rozšíří ho. toolearn jak tooconfigure defragmentace, získáte příliš[v tomto článku](service-fabric-cluster-resource-manager-defragmentation-metrics.md)
* Začít od začátku hello a [získat Úvod toohello správce prostředků clusteru Service Fabric](service-fabric-cluster-resource-manager-introduction.md)
* toofind si o tom, jak hello správce prostředků clusteru spravuje a vyrovnává zatížení v clusteru hello, podívejte se na článek hello na [Vyrovnávání zatížení](service-fabric-cluster-resource-manager-balancing.md)

[Image1]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-fault-domains.png
[Image2]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-uneven-fault-domain-layout.png
[Image3]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-fault-and-upgrade-domains-with-placement.png
[Image4]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-fault-and-upgrade-domain-layout-strategies.png
[Image5]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-layout-different-workloads.png
[Image6]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-placement-constraints-node-properties.png
[Image7]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-nodes-and-capacity.png
