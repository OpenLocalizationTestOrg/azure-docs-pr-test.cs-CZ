---
title: "aaaService správce prostředků infrastruktury Cluster - integraci správy | Microsoft Docs"
description: "Přehled integrace hello body mezi hello správce prostředků clusteru a správu prostředků infrastruktury služby."
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 956cd0b8-b6e3-4436-a224-8766320e8cd7
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 9a24c9de121fbe2e8e5e8e4d117e64686918936a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="cluster-resource-manager-integration-with-service-fabric-cluster-management"></a>Integrace manager prostředek clusteru s Správa clusteru Service Fabric
Hello správce prostředků clusteru Service Fabric není disk upgradů Service Fabric, ale je zahrnuta. Hello první tak, aby hello clusteru Resource Manager pomůže se správou je sledování hello požadovaného stavu hello clusteru a hello služeb uvnitř ho. Hello správce prostředků clusteru odesílá sestav stavu, když ji nemůžete uvést do požadované konfigurace hello hello clusteru. Například pokud je nedostatečné kapacity hello správce prostředků clusteru rozešle stav varování a chyby naznačující hello problém. Další část integrace má toodo s fungování upgradu. Hello clusteru Resource Manager mírně mění své chování během upgradu.  

## <a name="health-integration"></a>Integrace stavu
Hello správce prostředků clusteru neustále sleduje hello pravidla, která jste definovali k umístění vaší služby. Také sleduje hello zbývající kapacity pro jednotlivé metriky na uzlech hello a v hello clusteru a clusteru hello jako celek. Pokud ji nemůže splnit tato pravidla nebo pokud není dostatečná kapacita, jsou vydávány stav varování a chyby. Například pokud uzel je nad kapacitu a hello správce prostředků clusteru pokusí toofix hello situaci přesunutím služby. Pokud jej nelze opravit hello situaci vysílá stavu varování, uzel, který je přes kapacity a pro které metriky.

Dalším příkladem upozornění stavu hello Resource Manager je porušení omezení umístění. Například, pokud jste definovali omezení umístění (například `“NodeColor == Blue”`) a hello Resource Manager zjistí porušení omezení, se vydá upozornění stavu. To platí pro vlastní omezení a hello výchozí omezení (např. hello omezení domény selhání a upgradu domény).

Tady je příklad jeden takové zprávy stavu. Sestava stavu hello v takovém případě je pro jeden z oddílů služby system hello. zpráva stavu Hello znamená hello repliky tohoto oddílu jsou dočasně zabalit do příliš málo upgradu domén.

```posh
PS C:\Users\User > Get-WindowsFabricPartitionHealth -PartitionId '00000000-0000-0000-0000-000000000001'


PartitionId           : 00000000-0000-0000-0000-000000000001
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.PLB', Property='ReplicaConstraintViolation_UpgradeDomain', HealthState='Warning', ConsiderWarningAsError=false.

ReplicaHealthStates   :
                        ReplicaId             : 130766528804733380
                        AggregatedHealthState : Ok

                        ReplicaId             : 130766528804577821
                        AggregatedHealthState : Ok

                        ReplicaId             : 130766528854889931
                        AggregatedHealthState : Ok

                        ReplicaId             : 130766528804577822
                        AggregatedHealthState : Ok

                        ReplicaId             : 130837073190680024
                        AggregatedHealthState : Ok

HealthEvents          :
                        SourceId              : System.PLB
                        Property              : ReplicaConstraintViolation_UpgradeDomain
                        HealthState           : Warning
                        SequenceNumber        : 130837100116930204
                        SentAt                : 8/10/2015 7:53:31 PM
                        ReceivedAt            : 8/10/2015 7:53:33 PM
                        TTL                   : 00:01:05
                        Description           : hello Load Balancer has detected a Constraint Violation for this Replica: fabric:/System/FailoverManagerService Secondary Partition 00000000-0000-0000-0000-000000000001 is
                        violating hello Constraint: UpgradeDomain Details: UpgradeDomain ID -- 4, Replica on NodeName -- Node.8 Currently Upgrading -- false Distribution Policy -- Packing
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Ok->Warning = 8/10/2015 7:13:02 PM, LastError = 1/1/0001 12:00:00 AM
```

Tady je co tato zpráva stavu nám oznamuje je:

1. Všechny repliky hello sami jsou v pořádku: každá z nich má AggregatedHealthState: Ok
2. aktuálně probíhá porušení Hello doména Upgrade distribučního omezení. To znamená, že konkrétní upgradu domény má další repliky z tohoto oddílu, než by měl.
3. Uzel obsahuje hello repliky což způsobilo hello porušení. V takovém případě je hello uzel s názvem hello "Node.8"
4. Jestli upgradu se právě děje pro toto oddílu ("aktuálně upgrade--false")
5. Hello zásady distribuce pro tuto službu: "Distribuční zásady – okolních". Toto se řídí hello `RequireDomainDistribution` [umístění zásady](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md#requiring-replica-distribution-and-disallowing-packing). "Balení" znamená, že v tomto případě DomainDistribution byla _není_ potřeby, abychom věděli, že umístění zásady nebyl zadán pro tuto službu. 
6. Když se stalo hello sestavy - 8/10/2015 19:13:02: 00

Informace, jako tento výstrahy zajišťuje, že ještě efektivněji v produkční toolet, které znáte něco nepovede a zároveň používat toodetect a zastaví chybný upgrady. V takovém případě by chceme toosee Pokud jsme můžete zjistit, proč hello Resource Manager měl toopack hello repliky do hello upgradu domény. Obvykle balení je přechodný, protože hello uzly v hello jiných domén upgradu byly dolů, např.

Řekněme, že hello clusteru Resource Manager se pokouší tooplace některé služby, ale nejsou k dispozici žádné řešení, které pracují. Pokud nemůže být umístěn služby, je obvykle pro jednu z následujících důvodů hello:

1. Některé přechodného stavu udělal, je možné tooplace této instance služby nebo repliky správně
2. požadavky na umístění Hello služby jsou unsatisfiable.

V těchto případech sestav stavu z hello clusteru Resource Manager vám pomohou zjistit, proč nelze umístit hello služby. Říkáme tohoto procesu hello omezení odstranění pořadí. Během, provede hello systému hello nakonfigurovat omezení ovlivňující hello služby a záznamy se vyhnete. Tímto způsobem v případě, že služby nejsou možné toobe umístěny, uvidíte uzlů, které byly odstraněny a proč.

## <a name="constraint-types"></a>Omezení typů
Budeme mluvit o každé z různých omezení hello v těchto sestavách stavu. Omezení související toothese zprávy stavu se zobrazí, když repliky nemůže být umístěn.

* **ReplicaExclusionStatic** a **ReplicaExclusionDynamic**: těchto omezení označuje, že řešení byla odmítnuta, protože dva objekty služby z hello stejného oddílu by toobe umístili na hello stejný uzel. Toto není povolená, protože pak selhání tohoto uzlu by zbytečně ovlivnit oddílu. ReplicaExclusionStatic a ReplicaExclusionDynamic jsou téměř stejné pravidlo a hello rozdíly nejsou důležité skutečnosti hello. Pokud vidíte, že odstranění posloupnost omezení obsahující buď hello ReplicaExclusionStatic nebo ReplicaExclusionDynamic omezení, hello správce prostředků clusteru se domnívá, že není dostatek uzlů. To vyžaduje, zbývající řešení toouse tyto neplatné umísťováním, které nejsou povoleny. Hello jiná omezení v pořadí hello bude obvykle Řekněte nám, proč uzly jsou se eliminovala ve nejdřív hello.
* **PlacementConstraint**: Pokud se zobrazí tato zpráva, znamená to, jsme vyloučit některé uzly, protože neodpovídají omezení umístění služby hello. Jsme trasování se omezení umístění hello aktuálně nakonfigurovaný jako součást této zprávy. To je normální, pokud máte definované omezení umístění. Ale pokud omezení umístění je nesprávně příčinou příliš mnoho toobe uzly eliminovat Toto je, jak by si všimnete.
* **NodeCapacity**: Toto omezení znamená, že hello clusteru Resource Manager nemohl umístit hello repliky na hello uvedené uzly vzhledem k tomu, který by put přes kapacity.
* **Spřažení**: Toto omezení označuje, že jsme nemohl umístit hello repliky na uzlech hello vliv vzhledem k tomu, že by to způsobilo porušení omezení spřažení hello. Další informace o spřažení je v [v tomto článku](service-fabric-cluster-resource-manager-advanced-placement-rules-affinity.md)
* **FaultDomain** a **UpgradeDomain**: Toto omezení eliminuje uzly, pokud uvádění hello repliky na hello uvedené uzly by způsobilo balení v konkrétní chyby nebo doméně pro upgrade. Několik příkladů hovoříte o toto omezení jsou uvedeny v tématu hello na [selhání a upgradu omezení domény a výsledné chování](service-fabric-cluster-resource-manager-cluster-description.md)
* **PreferredLocation**: byste neměli vidět normálně toto omezení, protože je ve výchozím nastavení spustí jako optimalizace odstranění uzlů z hello řešení. Hello upřednostňovaná, že omezení umístění je také během upgradu. Během upgradu je služby používané toomove back se toowhere, které byly při spuštění upgradu hello.

## <a name="blocklisting-nodes"></a>Blocklisting uzly
Jiný správce prostředků clusteru sestav stavu zprávy hello je, když jsou blocklisted uzly. Blocklisting si můžete představit jako dočasné omezení, která je automaticky použita pro vás. Uzly získat blocklisted opakovaných neúspěšných dojde při spuštění instance tohoto typu služby. Uzly jsou blocklisted na základě typu za služby. Uzlem může být blocklisted pro typ jedna služba, ale ne jiné. 

Zobrazí se nové často během vývoje blocklisting: Některé chyby způsobí, že vaše toocrash hostitele služby při spuštění. Service Fabric pokusí hostitele služby hello toocreate několikrát, a vyskytuje opakovaně hello selhání. Po několika pokusech hello uzlu získá blocklisted a hello správce prostředků clusteru pokusí toocreate hello jinde služby. Pokud ve více uzlech udržuje nedošlo k této chybě, je možné všechny hello platný uzly v clusteru end hello se zablokuje. Blocklisting rovněž můžete odebrat mnoho uzly, že není dostatek úspěšně spustit hello služby toomeet hello potřeby škálování. Obvykle se zobrazí další chyby nebo upozornění hello správce prostředků clusteru označující, zda je služba hello níže hello požadované repliky nebo počet instancí, stejně jako zprávy stavu udávající neúspěch jaké hello je vznikají toohello blocklisting na prvním místě hello.

Blocklisting není trvalé podmínce. Po několika minutách hello uzel je odebrán z hello blocklist a Service Fabric může znovu aktivujte hello služeb v tomto uzlu. Pokud služby pokračovat toofail, hello uzel je blocklisted pro daný typ služby znovu. 

### <a name="constraint-priorities"></a>Omezení priority

> [!WARNING]
> Změna omezení priority se nedoporučuje a může mít významný negativní vliv na clusteru. Hello níže uvedené informace se poskytuje odkaz hello výchozí omezení priority a jejich chování. 
>

Se všemi těchto omezení vám může mít byla myslím "Hey – myslím, že jsou omezení domény selhání hello je třeba mít v systému. V pořadí tooensure hello omezení domény selhání není došlo k porušení, jsem ochoten tooviolate jiná omezení. "

Omezení lze konfigurovat různých úrovní priority. Jsou to:

   - "pevné" (0)
   - "soft" (1)
   - "optimalizace" (2)
   - "off" (-1). 
   
Většinu hello omezení jsou ve výchozím nastavení nakonfigurované jako pevný omezení.

Změna hello prioritu omezení neobvyklé. Byly časy kde priority omezení potřeby toochange, obvykle toowork kolem některé chyb nebo chování, které vliv hello prostředí. Obecně velmi dobře fungovala hello flexibilitu hello omezení s prioritou infrastruktury, ale nebyla často potřeba. Většina hello času, vše, co je umístěn na jejich výchozí priority. 

úrovně priority Hello nemáte znamenají, že dané omezení _bude_ být došlo k porušení, ani se vždy splnit. Omezení priority definovat pořadí, ve kterém jsou vynucena omezení. Priority definovat hello kompromisy případě, že je možné toosatisfy všechna omezení. Všechna omezení hello obvykle může obsloužit, pokud má něco jinak se děje v prostředí hello. Příklady scénářů, které povede tooconstraint porušení jsou konfliktní omezení, nebo velkého počtu souběžných selhání.

V situacích, rozšířené můžete změnit hello omezení priority. Řekněme například, chtěli tooensure spřažení by porušila vždy v když vydá nezbytné toosolve uzlu kapacity. tooachieve, můžete nastavit prioritu hello hello spřažení omezení příliš "soft" (1) a nechte hello kapacity nastavené omezení příliš "pevný" (0).

Hello výchozí hodnoty priority pro různé omezení hello jsou určené v hello následující konfigurace:

ClusterManifest.xml

```xml
        <Section Name="PlacementAndLoadBalancing">
            <Parameter Name="PlacementConstraintPriority" Value="0" />
            <Parameter Name="CapacityConstraintPriority" Value="0" />
            <Parameter Name="AffinityConstraintPriority" Value="0" />
            <Parameter Name="FaultDomainConstraintPriority" Value="0" />
            <Parameter Name="UpgradeDomainConstraintPriority" Value="1" />
            <Parameter Name="PreferredLocationConstraintPriority" Value="2" />
        </Section>
```

pomocí souboru ClusterConfig.json pro samostatné nasazení nebo Template.json pro Azure hostované clustery:

```json
"fabricSettings": [
  {
    "name": "PlacementAndLoadBalancing",
    "parameters": [
      {
          "name": "PlacementConstraintPriority",
          "value": "0"
      },
      {
          "name": "CapacityConstraintPriority",
          "value": "0"
      },
      {
          "name": "AffinityConstraintPriority",
          "value": "0"
      },
      {
          "name": "FaultDomainConstraintPriority",
          "value": "0"
      },
      {
          "name": "UpgradeDomainConstraintPriority",
          "value": "1"
      },
      {
          "name": "PreferredLocationConstraintPriority",
          "value": "2"
      }
    ]
  }
]
```

## <a name="fault-domain-and-upgrade-domain-constraints"></a>Omezení domény selhání domény a upgrade
Hello správce prostředků clusteru chce tookeep služby šíření mezi doménách selhání a upgradu. Modely to jako omezení uvnitř modul hello clusteru správce prostředků. Další informace o tom, jak se používají a jejich konkrétní chování, projděte si článek hello na [konfigurace clusteru](service-fabric-cluster-resource-manager-cluster-description.md#fault-and-upgrade-domain-constraints-and-resulting-behavior).

Hello správce prostředků clusteru může být nutné toopack několik repliky do upgradu domény v pořadí toodeal s upgrady, chyby nebo další narušení omezení. Balení do domén selhání nebo upgradu obvykle dochází jenom v případě, že existuje několik selhání nebo dalších změn v systému hello brání správné umístění. Pokud chcete tooprevent balení i v těchto situacích, můžete využít hello `RequireDomainDistribution` [umístění zásady](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md#requiring-replica-distribution-and-disallowing-packing). Všimněte si, že to může mít vliv na dostupnost služeb a spolehlivost jako vedlejším účinkem proto pečlivě zvažte.

Pokud hello prostředí je správně nakonfigurována, všechna omezení jsou dodržení i během upgradu. klíče věc Hello je tento Cluster Resource Manager sleduje pro vaše omezení hello. Když zjistí narušení okamžitě sestavy ji a pokusí toocorrect hello problém.

## <a name="hello-preferred-location-constraint"></a>omezení umístění Hello preferované
Hello PreferredLocation omezení je trochu liší, protože má dva různé používá. Jedno použití tohoto omezení je během upgradu aplikace. Hello clusteru Resource Manager automaticky spravuje toto omezení při upgradu. Je použité tooensure který po upgradu se dokončí, vraťte repliky tootheir počáteční umístění. Hello jiné použití hello PreferredLocation omezení je pro hello [ `PreferredPrimaryDomain` umístění zásady](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md). Obě tyto optimalizace a proto je hello PreferredLocation omezení hello pouze omezení nastavit také "Optimalizace" ve výchozím nastavení.

## <a name="upgrades"></a>Upgrady
Hello správce prostředků clusteru také pomáhá při aplikace a upgrady clusteru, za které má dvě úlohy:

* Ujistěte se, že nejsou ohrožená hello pravidla hello clusteru
* Zkuste upgrade přejděte hello toohelp bez problémů

### <a name="keep-enforcing-hello-rules"></a>Zachovat vynucování pravidel hello
Hello hlavní věc toobe vědět, je, že pravidla hello – striktní omezení pro hello jako omezení umístění a kapacity - prosazují stále během upgradu. Omezení umístění zkontrolujte, zda úlohy pouze spustili, kde mohou, i během upgradu. Když jsou vysoce omezené služby, upgrade může trvat déle. Pokud je služba hello nebo hello uzlu, který je spuštěn na snížila o aktualizaci, může být několik možností pro kde můžete přejít.

### <a name="smart-replacements"></a>Inteligentní nahrazení
Při spuštění upgradu hello Resource Manager pořídí snímek aktuální uspořádání hello hello clusteru. Protože každá doména upgradu se dokončí, pokusí tooreturn hello služby, které existovaly v původní uspořádání tohoto upgradu domény tootheir. Tímto způsobem existují maximálně dvě přechody pro službu během upgradu hello. Neexistuje jeden přesunout na uzlu hello vliv a jeden přesunout zpět. Vrácení toohow hello pro cluster nebo služby, který byl předtím, než hello upgrade také zajistí hello upgrade nebude mít vliv na rozložení hello hello clusteru. 

### <a name="reduced-churn"></a>Snížené změn
Jiné z věcí, které se stane při upgradech je tento hello vypne Správce prostředků clusteru služby Vyrovnávání. Brání vyrovnávání zabraňuje zbytečným reakce toohello upgradu, samostatně, jako přesun služby do uzlů, které byly vyprázdněny pro hello upgrade. Je-li upgrade hello dotyčném upgrade clusteru, není během upgradu hello vyvážit hello celý cluster. Omezení kontroly zůstane aktivní, pouze přesun podle proaktivní vyrovnávání hello metrik je zakázána.

### <a name="buffered-capacity--upgrade"></a>Ve vyrovnávací paměti kapacity & upgradu
Obecně chcete hello upgradu toocomplete i v případě, že hello cluster je omezené nebo ukončení toofull. Správa hello kapacity hello clusteru je důležitější-při upgradech než obvykle. Počet domén upgradu mezi 5 a 20 procent kapacity v závislosti na hello je potřeba migrovat jako hello upgradu vrátí prostřednictvím hello clusteru. Které pracují má toogo místo. Kde to je hello představu o [uložená do vyrovnávací paměti kapacity](service-fabric-cluster-resource-manager-cluster-description.md#buffered-capacity) je užitečné. Kapacita ve vyrovnávací paměti je dodržena při běžném provozu. Hello správce prostředků clusteru může během upgradu v případě potřeby zadejte uzly až tootheir celkové kapacity (využívání hello vyrovnávací paměti).

## <a name="next-steps"></a>Další kroky
* Začít od začátku hello a [získat Úvod toohello správce prostředků clusteru Service Fabric](service-fabric-cluster-resource-manager-introduction.md)
