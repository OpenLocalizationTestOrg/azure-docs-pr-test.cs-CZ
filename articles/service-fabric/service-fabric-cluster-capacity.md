---
title: aaaPlanning hello kapacity clusteru Service Fabric | Microsoft Docs
description: "Informace o plánování kapacity clusteru Service Fabric. Nodetypes, odolnost a spolehlivost vrstev"
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: timlt
editor: 
ms.assetid: 4c584f4a-cb1f-400c-b61f-1f797f11c982
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/24/2017
ms.author: chackdan
ms.openlocfilehash: 83272ce7fefe698eef755cf66493c2874cc3b120
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-cluster-capacity-planning-considerations"></a>Aspekty plánování kapacity služby cluster Service Fabric
Pro všechna produkční nasazení plánování kapacity je důležitý krok. Tady jsou některé z položek hello mít tooconsider jako součást tohoto procesu.

* toostart vašeho clusteru musí se s typy Hello počet uzlu
* Hello vlastnosti každého typu uzlu (velikost, primární, internetový bod, počet virtuálních počítačů, atd.)
* Hello spolehlivost a odolnost vlastnosti clusteru hello

Stručně dejte nám zkontrolujte, každá z těchto položek.

## <a name="hello-number-of-node-types-your-cluster-needs-toostart-out-with"></a>toostart vašeho clusteru musí se s typy Hello počet uzlu
Nejprve je třeba toofigure jaké hello clusteru, kterou vytváříte, bude toobe používá pro a plánování toho, jaké typy aplikací toodeploy do tohoto clusteru. Pokud si nejste zrušte cíleně hello hello clusteru, se pravděpodobně ještě připraveni kapacity hello tooenter procesu plánování.

Vytvořte hello počet typy uzlů, které cluster potřebuje toostart se s.  Každý typ uzlu je namapované tooa sadu škálování virtuálního počítače. Každý typ uzlu je možné škálovat pak nebo dolů nezávisle, mají různé sady otevřené porty a může mít různé kapacity metriky. Proto rozhodnutí hello hello počtu typy uzlů v podstatě dodává dolů toohello následující aspekty:

* Má vaše aplikace více služeb a některý z nich potřebovat toobe veřejné nebo internetové? Typická aplikace obsahují služby front-endu brány, která přijímá vstup z klienta a jeden nebo více back endové služby, které komunikují s hello front-endové služby. Proto v tomto případě se vám stát tím, že mají alespoň dva typy uzlů.
* Mají vašim službám (které tvoří vaši aplikaci) musí jiné infrastruktuře například větší paměti RAM nebo vyšší cyklů procesoru? Dejte nám Předpokládejme třeba, že budete chtít toodeploy obsahuje front-endová služba a služba back-end aplikace hello. Hello front-endovou službu můžete spustit na menší VMs (velikosti virtuálních počítačů jako D2), které mají toohello otevřené porty Internetu.  Hello back endové služby, ale výpočet náročné na prostředky a potřebuje toorun na větší VMs (s velikostí virtuálního počítače jako D4 D6, D15), které nejsou internet směrem k.
  
  V tomto příkladu i když se rozhodnete tooput všechny hello služby na jeden uzel typu, doporučujeme umístit je v clusteru se dvěma typy uzlů.  To umožňuje pro každý typ toohave jedinečné vlastnosti uzlu například připojení k Internetu nebo velikost virtuálního počítače. Hello počet virtuálních počítačů je možné rozšířit nezávisle, stejně.  
* Vzhledem k tomu, že nelze předpovídat budoucí hello, pomocí faktů, které znáte z a rozhodněte o hello počet typy uzlů, které vaše aplikace potřebují toostart s. Vždy můžete přidat nebo odebrat typy uzlů později. Cluster Service Fabric musí mít alespoň jeden uzel typu.

## <a name="hello-properties-of-each-node-type"></a>Hello vlastnosti každého typu uzlu
Hello **typ uzlu** se dají považovat za ekvivalentní tooroles v cloudové služby. Typy uzlů definovat velikosti virtuálních počítačů hello hello počet virtuálních počítačů a jejich vlastnosti. Každý typ uzlu, který je definován v clusteru Service Fabric je nastavený jako sada škálování samostatný virtuální počítač. Škálovací sadu virtuálních počítačů je prostředek výpočtů Azure můžete použít toodeploy a spravovat kolekci virtuálních počítačů jako sada. Definovaný jako škálovací sadu virtuálních počítačů distinct, každý typ uzlu je možné škálovat pak nebo dolů nezávisle, mají různé sady otevřené porty a může mít různé kapacity metriky.

Čtení [tento dokument](service-fabric-cluster-nodetypes.md) Další informace o vztahu hello Nodetypes toovirtual škálovací sady počítačů, jak tooRDP do jedné instance hello otevřít nové porty atd.

Cluster může mít více než jeden typ uzlu, ale typ primárního uzlu hello (hello první, kterou definujete na portálu hello) musí mít aspoň pět virtuálních počítačů pro clustery používat pro produkční zatížení (nebo minimálně tři virtuální počítače pro testovacích clusterů). Při vytváření clusteru hello pomocí šablony Resource Manageru, poté vyhledejte **je primární** atribut v definici typu uzlu hello. Hello primárního uzlu typ je typ uzlu hello, kde jsou umístěny Service Fabric systémových služeb.  

### <a name="primary-node-type"></a>Typ primárního uzlu
Pro cluster s několika typy uzlů budete potřebovat toochoose jeden z nich toobe primární. Zde jsou hello vlastnosti typu primárního uzlu:

* Hello **minimální velikosti virtuálních počítačů** pro typ hello primárním uzlu, který je určen podle hello **odolnost vrstvy** zvolíte. Výchozí hodnota Hello pro vrstvu odolnost hello je Bronzová. Posuňte se dolů podrobné informace o jaké úroveň odolnosti hello je a hello hodnoty, které může trvat.  
* Hello **minimální počet virtuálních počítačů** pro typ hello primárním uzlu, který je určen podle hello **úroveň spolehlivosti** zvolíte. Výchozí hodnota Hello pro úroveň spolehlivosti hello je Silver. Posuňte se dolů podrobné informace o jaké úroveň spolehlivosti hello je a hello hodnoty, které může trvat. 


* Hello Service Fabric systémových služeb (například hello Správce clusteru služby nebo služby úložiště Image Store) jsou umístěny na primárním uzlu, který typ hello a Ano hello spolehlivost a odolnost hello clusteru je dáno hello spolehlivost vrstvy hodnota a odolnost vrstvy hodnota, kterou jste vybrali pro typ hello primárního uzlu.

![Snímek obrazovky, který má dva typy uzlů clusteru ][SystemServices]

### <a name="non-primary-node-type"></a>Typ uzlu není primární
Pro cluster s několika typy uzlů je jeden typ primárního uzlu a rest hello z nich jsou není primární. Zde jsou hello vlastnosti uzlu není primární typu:

* minimální velikost Hello virtuálních počítačů pro tento typ uzlu je určen podle hello odolnost vrstvy, které zvolíte. Výchozí hodnota Hello pro vrstvu odolnost hello je Bronzová. Posuňte se dolů podrobné informace o jaké úroveň odolnosti hello je a hello hodnoty, které může trvat.  
* Hello minimální počet virtuálních počítačů pro tento typ uzlu může být jedna. Měli byste ale vybrat toto číslo na základě počtu hello repliky hello aplikací nebo služeb, které chcete toorun v tomto typu uzlu. Po nasazení hello clusteru může být zvýšena Hello počet virtuálních počítačů v uzlu typu.

## <a name="hello-durability-characteristics-of-hello-cluster"></a>Vlastnosti odolnost Hello hello clusteru
úroveň odolnosti Hello je použité tooindicate toohello systému hello pověření, která mají virtuální počítače se základní infrastrukturu Azure hello. V primárním uzlu, který typ hello toto oprávnění umožňuje Service Fabric toopause žádosti úrovni infrastruktury virtuálních počítačů (například restartování virtuálního počítače, obnovení z Image virtuálního počítače nebo migrace virtuálních počítačů), vliv hello požadavky kvora pro hello systémových služeb a vaše stavové služby. V hello typy uzel není primární toto oprávnění umožňuje Service Fabric toopause všechny virtuální počítač úrovně infrastruktury žádosti restartování virtuálního počítače, obnovení z Image virtuálních počítačů, virtuálních počítačů migrace atd., které mají vliv hello požadavky kvora pro váš stavové služby spuštěné v ní.

Toto oprávnění je vyjádřena v hello následující hodnoty:

* Zlatý - hello infrastruktury, které úlohy můžete pozastavit po dobu dvou hodin za UD. Gold odolnost můžete povolit jenom pro SKU úplné uzlu virtuálního počítače jako D15_V2 G5 atd.
* Stříbrná - hello infrastruktury úlohy můžete pozastavit po dobu 10 minut na jednu UD a je k dispozici na všechny standardní virtuální počítače z jediného jádra a vyšší.
* Bronzová - žádná oprávnění. Toto je výchozí hello. Tato úroveň odolnosti používat jenom pro typy uzlů, které používají _pouze_ Bezstavová zatížení. 

> [!WARNING]
> NodeTypes s bronzová odolnost získat _žádné oprávnění_. To znamená, že nebude infrastruktury úlohy, které mají vliv Bezstavová zatížení zastavit nebo zpoždění. Je možné tyto úlohy může ovlivnit stále úlohy, způsobuje výpadek nebo jiných problémů. Pro všechny řazení produkční úlohy běží s aspoň se doporučuje Silver. 
> 

Získáte úroveň toochoose odolnost pro každou z vaší typy uzlů. Můžete zvolit jeden typ uzlu toohave odolnost úrovně Gold nebo stříbrný a hello jiné mají bronzová ve hello stejného clusteru. **Musí zachovat minimální počet uzlů 5 pro jakýkoli uzel typ, který má odolnost zlatý nebo silver**. 

**Výhody používání úrovně Silver nebo zlatý odolnost**
 
1. Snižuje hello počet požadovaných kroků v rámci škálování operace (to znamená, deaktivace uzlu a odebrat ServiceFabricNodeState se nazývá automaticky)
2. Snižuje riziko hello ztráty dat z důvodu tooa spouštěná zákazníka na místě virtuálních počítačů SKU změnu operací, o infrastrukturu Azure.
     
**Nevýhody použití úrovně Silver nebo zlatý odolnost**
 
1. Nasazení tooyour sadu škálování virtuálního počítače a dalších souvisejících prostředků Azure) může být zpoždění, můžete vypršení časového limitu nebo může být blokovány zcela problémy v clusteru nebo na úrovni infrastruktury hello. 
2. Zvyšuje hello počet [události životního cyklu repliky](service-fabric-reliable-services-advanced-usage.md#stateful-service-replica-lifecycle ) (například primární záměna) z důvodu tooautomated uzlu deaktivací během operací infrastruktury Azure.

### <a name="recommendations-on-when-toouse-silver-or-gold-durability-levels"></a>Doporučení když odolnost úrovně Silver nebo zlatý toouse

Použití Silver nebo zlatý odolnost pro všechny uzly typy stateful tohoto hostitele služby můžete očekávat při tooscale (snížit počet instancí virtuálního počítače) často, a si přejete, operace nasazení se odloží považuje zjednodušit tyto operace škálování v. scénáře Hello Škálováním na více systémů (Přidání instance virtuálních počítačů) do zvoleného hello odolnost vrstvy nejsou dostupné, jenom škálování v nepodporuje.

### <a name="operational-recommendations-for-hello-node-type-that-you-have-set-toosilver-or-gold-durability-level"></a>Provozní doporučení pro uzel hello zadejte, že jste si nastavili toosilver nebo zlatý odolnost úrovně.

1. Zachovat cluster a aplikace v pořádku po celou dobu a ujistěte se, že aplikace reagovat tooall [služby události životního cyklu repliky](service-fabric-reliable-services-advanced-usage.md#stateful-service-replica-lifecycle) (jako jsou repliky v sestavení zasekl) včas.
2. Přijmout bezpečnější způsoby toomake změnu SKU virtuálních počítačů (škálování nahoru/dolů): Změna hello skladová položka virtuálních počítačů sady škálování virtuálního počítače je ze své podstaty nezabezpečené operace a tak je nutno Pokud je to možné. Tady je proces hello provedením tooavoid běžné problémy.
    - **Pro neprimární nodetypes:** se doporučuje vytvořit novou sadu škálování virtuálního počítače, úpravě hello umístění omezení tooinclude hello nové sady škálování virtuálních počítačů nebo uzel typ služby a potom snížit hello původní sadu škálování virtuálního počítače instance počet too0, jeden uzel v čase (to je toomake jistotu, že odebrání uzlů hello nemají vliv hello spolehlivost hello clusteru).
    - **Primární NodeType hello:** doporučujeme neměnit SKU virtuálních počítačů typu hello primárního uzlu. Pokud hello důvodu hello nové SKU je kapacity, doporučujeme přidávání více instancí nebo pokud je to možné, vytvořte nový cluster. Pokud máte žádný výběr, proveďte úpravy toohello modelu nastavit škálování virtuálního počítače definice tooreflect hello nové SKU. Pokud cluster obsahuje pouze jeden typ uzlu, ujistěte se, že všechny stavové aplikace reagovat tooall [služby události životního cyklu repliky](service-fabric-reliable-services-advanced-usage.md#stateful-service-replica-lifecycle) (jako je zablokované repliky v sestavení) v a včasné a které vaše služba repliky sestavit Doba trvání je méně než pět minut (úroveň stříbrným odolnost). 


> [!WARNING]
> Změna hello velikost virtuálního počítače SKU pro Škálovací sady virtuálních počítačů není spuštěna alespoň stříbrným odolnost není doporučeno. Změna velikosti virtuálních počítačů SKU je operace destruktivní dat místní infrastrukturu. Bez alespoň některé toodelay možnost nebo sledování tuto změnu, je možné, že hello operaci může způsobit, že dataloss pro stavové služby nebo způsobit další provozní problémy nepředvídaných, i pro Bezstavová zatížení. 
> 
    
3. Udržovat minimální počet pět uzlů pro všechny sadu škálování virtuálního počítače s MR povoleno
4. Není odstranit náhodných instance virtuálních počítačů, vždy použijte sadu škálování virtuálního počítače vertikálně funkce. odstranění Hello náhodných instancí virtuálních počítačů má potenciál vytváření nevyváženosti v instanci virtuálního počítače hello rozloženy UD a FD. Tato nevyváženosti může nepříznivě ovlivnit hello systémy možnost tooproperly nástroj pro vyrovnávání zatížení mezi hello služby instancí a službám repliky.
6. Pokud používáte škálování, potom nastavte pravidla hello tak, aby měřítka ve (odebrání instancí virtuálního počítače) se provádějí pouze jeden uzel v čase. Škálování dolů více než jednu instanci v čase není bezpečná.
7. Pokud škálování dolů typu primárního uzlu, je by nikdy škálovat dolů více než jakou úroveň spolehlivosti hello umožňuje.


## <a name="hello-reliability-characteristics-of-hello-cluster"></a>Vlastnosti spolehlivost Hello hello clusteru
Hello úroveň spolehlivosti se používá tooset hello počet replik, které chcete toorun v tomto clusteru na primárním uzlu, který typ hello hello systémových služeb. Dobrý den další hello počet replik, hello spolehlivější hello systémové služby byly v clusteru.  

úroveň spolehlivosti Hello může trvat hello následující hodnoty:

* Platinové - spustit hello systémových služeb s replikou cíl nastavit počet 9
* Zlatý - spustit hello systémových služeb s replikou cíl nastavit počet 7
* Stříbrná - spustit hello systémových služeb s počtem sady replik cíl 5 
* Bronzová - spustit hello systémových služeb s počtem sady replik cíl 3

> [!NOTE]
> úroveň spolehlivosti Hello, vybraných určuje hello minimální počet uzlů, které musí mít typ vašeho primárního uzlu. 
> 
> 


### <a name="recommendations-for-hello-reliability-tier"></a>Doporučení pro úroveň spolehlivosti hello.

 Když zvětšit nebo zmenšit velikost hello clusteru (hello součet instance virtuálních počítačů v všechny typy uzlů), musíte aktualizovat hello spolehlivost clusteru z jedné vrstvy tooanother. Tato funkce se aktivuje hello upgrady potřebné toochange hello systému služby repliky sadu počtu clusterů. Počkejte hello upgrade v průběhu toocomplete před provedením jakékoli jiné změny toohello clusteru, jako je přidání uzlů.  Můžete sledovat průběh hello hello upgrade na Service Fabric Explorer nebo spuštěním [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)

Zde je hello doporučení na Volba úrovně spolehlivost hello.

| **Velikost clusteru** | **Úroveň spolehlivosti** |
| --- | --- |
| 1 |Nezadávejte hello parametr úroveň spolehlivosti, systém hello je vypočte |
| 3 |Bronzová |
| 5 nebo 6|Stříbrná |
| 7 nebo 8 |Zlatý |
| 9 a vyšší |Platinové |




## <a name="primary-node-type---capacity-guidance"></a>Typ primárního uzlu – pokyny kapacity

Tady je hello pokyny pro plánování kapacity typ primárního uzlu hello

1. **Počet virtuálních počítačů instancí toorun žádné produkční úlohy v Azure:** musíte zadat minimální velikost písma primárního uzlu 5. 
2. **Počet virtuálních počítačů instancí toorun testovací úlohy v Azure** můžete zadat velikost písma minimální primárního uzlu 1 nebo 3. Dobrý den jednoho uzlu clusteru, spustí se zvláštní konfiguraci a tak, škálování mimo tento cluster nepodporuje. Hello jednoho uzlu clusteru, nemá žádné spolehlivost a proto v šabloně Resource Manager, máte tooremove nebo Ne, zadejte pro tuto konfiguraci (není nastavení hodnota konfigurace hello není dost). Pokud jste nastavili hello jeden uzel clusteru nastavit prostřednictvím portálu a potom hello konfigurace se automaticky stará. clustery s uzly 1 a 3 nejsou podporovány pro spuštění úlohy v produkčním prostředí. 
3. **Virtuální počítač SKU:** typ primárního uzlu je, kde spustit hello systémových služeb, tak hello SKU virtuálních počítačů, které zvolíte, musí provést do účtu hello celkové špičkovým zatížení můžete naplánovat tooplace do clusteru hello. Tady je analogie tooillustrate co I zde - znamenat myslíte hello primárního uzlu typu jako vaše "plíce", je co nabízí kyslíku tooyour mozku, a proto pokud hello mozku nedostane dostatek kyslíku, vaše textu vyskytne. 

Vzhledem k tomu, že požadavků na kapacitu hello clusteru je určen podle zatížení plánování toorun v clusteru hello, nemůžeme vám poskytovat s kvalitativní pokyny pro konkrétní úlohu, ale tady je toohelp hello obecné pokyny, které začít pracovat

Pro produkční zatížení 


- Hello doporučuje SKU virtuálního počítače je standardní D3 nebo standardní D3_V2 nebo ekvivalentní minimálně 14 GB místní SSD.
- použití Hello minimální podporované SKU virtuálního počítače je standardní D1 nebo standardní D1_V2 nebo ekvivalentní minimálně 14 GB místní SSD. 
- Částečné základní SKU virtuálního počítače jako standardní A0 nejsou podporovány pro úlohy v produkčním prostředí.
- Standardní SKU A1 není podporována pro produkční zatížení z důvodů výkonu.


## <a name="non-primary-node-type---capacity-guidance-for-stateful-workloads"></a>Typ uzlu není primární - kapacity pokyny pro stavová zatížení

Tento průvodce je určený pro stavová zatížení pomocí Service fabric [spolehlivé kolekce nebo reliable Actors](service-fabric-choose-framework.md) , kterou používáte v hello typ uzlu není primární.


**Počet instancí virtuálního počítače:** pro produkční zatížení, které jsou stavové, doporučujeme je spustit s minimální a cílové repliky počtu 5. To znamená, že v stabilního stavu skončili s replikou (ze sady replik) v každé doméně selhání a upgradu domény. Koncept vrstvy Hello celou spolehlivost pro typ primárního uzlu hello je toospecify způsob, jak toto nastavení pro systémových služeb. Proto hello stejný problém platí tooyour stavové služby.

Proto pro úlohy v produkčním prostředí hello minimální doporučené jiný primární uzlem typu velikost je 5, pokud používáte stavová zatížení v ní.


**Virtuální počítač SKU:** Toto je typ uzlu hello spuštěným aplikační služby, takže hello SKU virtuálního počítače jste vybrali pro, je třeba vzít v zátěž ve špičce hello účet můžete naplánovat tooplace do každého uzlu. Dobrý den požadavků na kapacitu hello nodetype, je dáno pracovního vytížení, že máte v plánu toorun v clusteru hello, takže jsme nelze poskytují kvalitativní pokyny pro konkrétní úlohy, ale tady je toohelp hello obecné pokyny, které začít pracovat

Pro produkční zatížení 

- Hello doporučuje SKU virtuálního počítače je standardní D3 nebo standardní D3_V2 nebo ekvivalentní minimálně 14 GB místní SSD.
- použití Hello minimální podporované SKU virtuálního počítače je standardní D1 nebo standardní D1_V2 nebo ekvivalentní minimálně 14 GB místní SSD. 
- Částečné základní SKU virtuálního počítače jako standardní A0 nejsou podporovány pro úlohy v produkčním prostředí.
- Standardní SKU A1 konkrétně není podporován pro produkční zatížení z důvodů výkonu.


## <a name="non-primary-node-type---capacity-guidance-for-stateless-workloads"></a>Typ uzlu není primární - pokyny Bezstavová zatížení, kapacity

V tomto návodu bezstavových úloh, které běží na neprimární nodetype hello.

**Počet instancí virtuálního počítače:** pro produkční zatížení, které jsou bezstavové, velikost písma jiný - primárního uzlu hello minimální podporované je 2. To vám umožní toorun můžete dvě bezstavové instance aplikace, a umožňuje vaší služby toosurvive hello ztrátě instance virtuálního počítače. 

> [!NOTE]
> Pokud váš cluster běží v service fabric starší verze než 5.6, z důvodu tooa značit potíže v hello runtime (Tento problém vyřešen v 5.6), škálování směrem dolů neprimární uzel typu tooless než 5, výsledkem stavu clusteru vypnutí není v pořádku, dokud zavoláte [ Odebrat ServiceFabricNodeState cmd](https://docs.microsoft.com/powershell/servicefabric/vlatest/Remove-ServiceFabricNodeState) s názvem příslušný uzel hello. Čtení [provést cluster Service Fabric příchozí nebo odchozí](service-fabric-cluster-scale-up-down.md) další podrobnosti
> 
>

**Virtuální počítač SKU:** Toto je typ uzlu hello spuštěným aplikační služby, takže hello SKU virtuálního počítače jste vybrali pro, je třeba vzít v zátěž ve špičce hello účet můžete naplánovat tooplace do každého uzlu. Dobrý den požadavků na kapacitu hello nodetype, je dáno pracovního vytížení, že máte v plánu toorun v clusteru hello, takže jsme nelze poskytují kvalitativní pokyny pro konkrétní úlohy, ale tady je toohelp hello obecné pokyny, které začít pracovat

Pro produkční zatížení 


- Hello doporučuje SKU virtuálního počítače je standardní D3 nebo standardní D3_V2 nebo ekvivalentní. 
- použití Hello minimální podporované SKU virtuálního počítače je standardní D1 nebo standardní D1_V2 nebo ekvivalentní. 
- Částečné základní SKU virtuálního počítače jako standardní A0 nejsou podporovány pro úlohy v produkčním prostředí.
- Standardní SKU A1 není podporována pro produkční zatížení z důvodů výkonu.

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->

## <a name="next-steps"></a>Další kroky
Po dokončení plánování kapacity a nastavení clusteru, přečtěte si následující hello:

* [Zabezpečení clusteru Service Fabric](service-fabric-cluster-security.md)
* [Úvod modelu stavu Service Fabric](service-fabric-health-introduction.md)
* [Nastavit vztah Nodetypes tooVirtual počítač měřítka](service-fabric-cluster-nodetypes.md)

<!--Image references-->
[SystemServices]: ./media/service-fabric-cluster-capacity/SystemServices.png
