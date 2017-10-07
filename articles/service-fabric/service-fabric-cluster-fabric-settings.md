---
title: "nastavení clusteru Azure Service Fabric aaaChange | Microsoft Docs"
description: "Tento článek popisuje hello nastavení prostředků infrastruktury a infrastruktury hello upgradu se zásady, které můžete přizpůsobit."
services: service-fabric
documentationcenter: .net
author: chackdan
manager: timlt
editor: 
ms.assetid: 7ced36bf-bd3f-474f-a03a-6ebdbc9677e2
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/15/2017
ms.author: chackdan
ms.openlocfilehash: a8b125f7b4a02547cf4bcf2c936d0c7f1686c1a0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="customize-service-fabric-cluster-settings-and-fabric-upgrade-policy"></a>Přizpůsobení nastavení clusteru Service Fabric a zásady upgradu prostředků infrastruktury
Tento dokument se dozvíte, jak toocustomize hello různé nastavení prostředků infrastruktury a hello zásad upgradu infrastruktury pro váš cluster Service Fabric. Můžete si je přizpůsobit na hello portálu nebo pomocí šablony Azure Resource Manager.

> [!NOTE]
> Ne všechna nastavení může být k dispozici prostřednictvím portálu hello. V případě, že níže uvedených nastavení není k dispozici prostřednictvím portálu hello přizpůsobte pomocí šablony Azure Resource Manager.
> 

## <a name="customizing-service-fabric-cluster-settings-using-azure-resource-manager-templates"></a>Přizpůsobení nastavení clusteru Service Fabric pomocí šablon Azure Resource Manageru
znázornění Hello kroky jak tooadd nové nastavení *MaxDiskQuotaInMB* toohello *diagnostiky* části.

1. Přejděte toohttps://resources.azure.com
2. Přejděte tooyour předplatné rozšířením odběry -> -> skupiny prostředků Microsoft.ServiceFabric -> váš název clusteru
3. V horním pravém rohu hello vyberte možnost "Pro čtení a zápis"
4. Vyberte upravit a aktualizovat hello `fabricSettings` JSON element a přidejte nový element

```
      {
        "name": "Diagnostics",
        "parameters": [
          {
            "name": "MaxDiskQuotaInMB",
            "value": "65536"
          }
        ]
      }
```

## <a name="fabric-settings-that-you-can-customize"></a>Nastavení prostředků infrastruktury, které můžete přizpůsobit
Zde jsou hello nastavení prostředků infrastruktury, které můžete přizpůsobit:

### <a name="section-name-diagnostics"></a>Název oddílu: diagnostiky
| **Parametr** | **Povolené hodnoty** | **Pokyny nebo krátký popis** |
| --- | --- | --- |
| ConsumerInstances |Řetězec |Hello seznam instancí DCA příjemce. |
| ProducerInstances |Řetězec |Hello seznam instancí producent DCA. |
| AppEtwTraceDeletionAgeInDays |Int, výchozí hodnota je 3 |Počet dní, po kterých jsme odstranit staré ETL soubory obsahující trasování ETW aplikací. |
| AppDiagnosticStoreAccessRequiresImpersonation |BOOL, výchozí hodnota je true |Jestli je potřeba zosobnění, při přístupu k diagnostiky ukládá jménem aplikace hello. |
| MaxDiskQuotaInMB |Int, výchozí hodnota je 65536 |Kvóty disku v MB pro technologie Windows Fabric protokolových souborech. |
| DiskFullSafetySpaceInMB |Int, výchozí hodnota je 1 024 |Zbývající místo na disku v MB tooprotect z používá DCA. |
| ApplicationLogsFormatVersion |Int, výchozí hodnota je 0 |Verze pro aplikaci zaznamená formátu. Podporované hodnoty jsou 0 a 1. Verze 1 obsahuje více polí ze záznamu události trasování událostí pro Windows hello než verze 0. |
| ClusterId |Řetězec |jedinečné id Hello hello clusteru. Tím se vygeneruje při vytvoření clusteru hello. |
| EnableTelemetry |BOOL, výchozí hodnota je true |To bude tooenable nebo zakázat telemetrii. |
| EnableCircularTraceSession |BOOL, výchozí hodnota je false |Příznak určuje, zda by měl použít relace cyklické trasování. |

### <a name="section-name-traceetw"></a>Název oddílu: Trasování nebo trasování
| **Parametr** | **Povolené hodnoty** | **Pokyny nebo krátký popis** |
| --- | --- | --- |
| Úroveň |Int, výchozí hodnota je 4 |Úroveň trasování etw může trvat hodnoty 1, 2, 3, 4. toobe podporované hello úroveň trasování je nutné zachovat na 4 |

### <a name="section-name-performancecounterlocalstore"></a>Název oddílu: PerformanceCounterLocalStore
| **Parametr** | **Povolené hodnoty** | **Pokyny nebo krátký popis** |
| --- | --- | --- |
| Hodnotu IsEnabled |BOOL, výchozí hodnota je true |Příznak určuje, zda je povoleno sběru dat čítače výkonu v místním uzlu. |
| SamplingIntervalInSeconds |Int, výchozí hodnota je 60 |Interval vzorkování pro čítače výkonu shromažďovaných. |
| Čítače |Řetězec |Čárkami oddělený seznam toocollect čítače výkonu. |
| MaxCounterBinaryFileSizeInMB |Int, výchozí hodnota je 1 |Maximální velikost (v MB) pro každý soubor binární čítače výkonu. |
| NewCounterBinaryFileCreationIntervalInMinutes |Int, výchozí hodnota je 10 |Maximální interval (v sekundách), která po jejímž uplynutí je vytvořen nový soubor binární čítače výkonu. |

### <a name="section-name-setup"></a>Název oddílu: Instalační program
| **Parametr** | **Povolené hodnoty** | **Pokyny nebo krátký popis** |
| --- | --- | --- |
| Proměnná FabricDataRoot |Řetězec |Service Fabric data kořenový adresář. Výchozí pro Azure je d:\svcfab |
| Adresáři FabricLogRoot |Řetězec |Služba fabric protokolu kořenový adresář. Toto je, kde jsou umístěny SF protokoly a trasování. |
| ServiceRunAsAccountName |Řetězec |název účtu Hello v rámci které toorun služby fabric host. |
| ServiceStartupType |Řetězec |Hello typ spouštění služby fabric host hello. |
| SkipFirewallConfiguration |BOOL, výchozí hodnota je false |Určuje, pokud nastavení brány firewall třeba toobe nastavit hello systému, nebo ne. To platí jenom v případě, že používáte bránu windows firewall. Pokud používáte brány firewall třetích stran, pak musíte otevřít hello porty pro systém a aplikace toouse hello |

### <a name="section-name-transactionalreplicator"></a>Název oddílu: TransactionalReplicator
| **Parametr** | **Povolené hodnoty** | **Pokyny nebo krátký popis** |
| --- | --- | --- |
| MaxCopyQueueSize |Uint, výchozí hodnota je 16384 |Toto je maximální hodnota hello definuje hello počáteční velikost hello fronty, který udržuje operací replikace. Všimněte si, že musí být násobkem 2. Pokud během doby běhu hello fronty zvětšování toothis velikost operace budou omezeny mezi primárním a sekundárním replikátory hello. |
| BatchAcknowledgementInterval | Čas v sekundách, výchozí hodnota je 0,015 | Zadejte časový interval v sekundách. Určuje hello množství času, který hello Replikátor počká po přijetí operace před odesláním zpět potvrzení. Jiné operace přijata během této doby bude mít jejich potvrzení odeslána zpět v do jedné zprávy -> Redukční síťový provoz, ale potenciálně redukční hello propustnost Replikátor hello. |
| Maxreplicationmessagesize. |Uint, výchozí hodnota je 52428800 | Maximální velikost zprávy operací replikace. Výchozí hodnota je 50MB. |
| ReplicatorAddress |řetězec, výchozí je "localhost:0" | koncový bod Hello v podobě řetězce-'IP: port, který je používán hello Replikátor Windows Fabric tooestablish připojení s ostatními replikami v operacích toosend a příjem pořadí. |
| InitialPrimaryReplicationQueueSize |Uint, výchozí hodnota je 64 | Tato hodnota definuje hello počáteční velikost hello fronty, který udržuje hello replikací v hello primární. Všimněte si, že musí být násobkem 2.|
| MaxPrimaryReplicationQueueSize |Uint, výchozí hodnota je 8192 |Toto je maximální počet operací, které může existovat ve frontě primární replikace hello hello. Všimněte si, že musí být násobkem 2. |
| MaxPrimaryReplicationQueueMemorySize |Uint, výchozí hodnota je 0 |Toto je maximální hodnota hello hello primární replikace fronty v bajtech. |
| InitialSecondaryReplicationQueueSize |Uint, výchozí hodnota je 64 |Tato hodnota definuje hello počáteční velikost hello fronty, který udržuje hello replikací v hello sekundární. Všimněte si, že musí být násobkem 2. |
| MaxSecondaryReplicationQueueSize |Uint, výchozí hodnota je 16384 |Toto je maximální počet operací, které může existovat ve frontě replikace sekundární hello hello. Všimněte si, že musí být násobkem 2. |
| MaxSecondaryReplicationQueueMemorySize |Uint, výchozí hodnota je 0 |Toto je maximální hodnota hello hello sekundární replikační fronty v bajtech. |
| SecondaryClearAcknowledgedOperations |BOOL, výchozí hodnota je false |Logická hodnota, která ovládá, zda hello operací na sekundární Replikátor hello jsou vymazány po jejich jsou potvrzené primární toohello (vyprázdněn toohello disk). Nastavení, které tento tooTRUE může mít za následek další disk čtení na nový primární hello, při zachytávání repliky po převzetí služeb. |
| MaxMetadataSizeInKB |Int, výchozí hodnota je 4 |Maximální velikost metadata datový proud protokolu hello. |
| MaxRecordSizeInKB |Uint, výchozí hodnota je 1 024 | Maximální velikost datového proudu záznam protokolu. |
| CheckpointThresholdInMB |Int, výchozí hodnota je 50 |Kontrolní bod se zahájí, při použití protokolů hello překročí tuto hodnotu. |
| MaxAccumulatedBackupLogSizeInMB |Int, výchozí hodnota je 800 |Maximální počet shromážděných velikost (v MB) zálohování protokolů v řetězci dané zálohy protokolu. Požadavky přírůstkové zálohování se nezdaří, pokud hello přírůstkové zálohování by vygeneroval zálohy protokolu, které by způsobily protokoly zálohování hello shromážděných od hello relevantní úplného zálohování toobe větší než tato velikost. V takových případech je uživatel požadované tootake úplné zálohování. |
| MaxWriteQueueDepthInKB |Int, výchozí hodnota je 0 | Int pro maximální zápisu hloubku fronty tohoto protokolovacího nástroje základní hello můžete použít jako zadaný v kilobajtech pro hello protokolu, který je přidružen této repliky. Tato hodnota je hello maximální počet bajtů, které se dají nezpracovaných během aktualizace základní protokolovacího nástroje. To může být 0 pro odpovídající hodnotu nebo násobkem 4 pro základní hello toocompute protokolovacího nástroje. |
| SharedLogId |Řetězec |Identifikátor sdílené protokolu. Toto je identifikátor guid a musí být jedinečný pro každý sdílený protokolu. |
| SharedLogPath |Řetězec |Cesta toohello sdílené protokolu. Pokud tato hodnota je prázdný, použije se hello výchozí sdílené protokolu. |
| SlowApiMonitoringDuration |Čas v sekundách, výchozí hodnota je 300 | Předtím, než je aktivována událost stavu upozornění, zadejte dobu trvání pro rozhraní api.|
| MinLogSizeInMB |Int, výchozí hodnota je 0 |Minimální velikost hello transakčního protokolu. velikost tooa tootruncate nižší než toto nastavení nebude povoleno přihlášení Hello. Hodnota 0 znamená, že určí, že hello Replikátor hello minimální velikost protokolu podle tooother nastavení. Zvýšení hodnoty tuto zvyšuje možnost hello plnění od pravděpodobnost příslušných protokolových záznamy, které ke zkrácení je snížena částečné kopie a přírůstkové zálohování. |

### <a name="section-name-fabricclient"></a>Název oddílu: FabricClient
| **Parametr** | **Povolené hodnoty** | **Pokyny nebo krátký popis** |
| --- | --- | --- |
| NodeAddresses |Výchozí hodnota je řetězec, "" |Kolekce adres (připojovací řetězce) v různých uzlech, které se dají použít toocommunicate s hello hello Naming Service. Původně hello výběrem jedné z adresy hello náhodně připojení klienta. Pokud je zadán více než jeden připojovací řetězec a připojení se nezdaří z důvodu komunikaci nebo vypršení časového limitu; Hello klienta přepínačů adresa dalšího hello toouse postupně. V části hello pojmenování adresu služby opakování podrobnosti na sémantiku opakování. |
| ConnectionInitializationTimeout |Čas v sekundách, výchozí hodnota je 2 |Zadejte časový interval v sekundách. Interval vypršení časového limitu připojení pro každého klienta čas pokusí tooopen bránu toohello připojení. |
| Parametr PartitionLocationCacheLimit |Int, výchozí hodnota je 100000. |Počet oddílů uložená v mezipaměti pro řešení služby (nastavit too0 pro žádné omezení). |
| ServiceChangePollInterval |Čas v sekundách, výchozí hodnota je 120. |Zadejte časový interval v sekundách. Hello interval mezi po sobě jdoucích hlasování služby se změní z hello klienta toohello brány pro registrovanou službu změnu oznámení zpětných volání. |
| KeepAliveIntervalInSeconds |Int, výchozí hodnota je 20 |Hello interval, ve které hello FabricClient přenosu odesílá zprávy keep-alive toohello brány. Pro uživatele 0; keepAlive je zakázána. Musí být kladná hodnota. |
| HealthOperationTimeout |Čas v sekundách, výchozí hodnota je 120. |Zadejte časový interval v sekundách. časový limit Hello zprávy sestavy odeslat tooHealth správce. |
| HealthReportSendInterval |Čas v sekundách, výchozí hodnota je 30 |Zadejte časový interval v sekundách. Hello interval, ve které součást pro vytváření sestav odešle nahromadění stavu hlásí tooHealth správce. |
| HealthReportRetrySendInterval |Čas v sekundách, výchozí hodnota je 30 |Zadejte časový interval v sekundách. Hello interval, ve které součást pro vytváření sestav znovu odešle nahromadění stavu hlásí tooHealth správce. |
| RetryBackoffInterval |Čas v sekundách, výchozí hodnota je 3 |Zadejte časový interval v sekundách. Hello back vypnout interval před opakováním operace hello. |
| MaxFileSenderThreads |Uint, výchozí hodnota je 10 |Hello maximální počet souborů, které se přenáší paralelně. |

### <a name="section-name-common"></a>Název oddílu: běžné
| **Parametr** | **Povolené hodnoty** | **Pokyny nebo krátký popis** |
| --- | --- | --- |
| PerfMonitorInterval |Čas v sekundách, výchozí hodnota je 1 |Zadejte časový interval v sekundách. Interval monitorování výkonu. Nastavení too0 nebo záporná, zakážete sledování. |

### <a name="section-name-healthmanager"></a>Název oddílu: HealthManager
| **Parametr** | **Povolené hodnoty** | **Pokyny nebo krátký popis** |
| --- | --- | --- |
| EnableApplicationTypeHealthEvaluation |BOOL, výchozí hodnota je false |Cluster zásad vyhodnocení stavu: Povolit za vyhodnocení stavu typu aplikace. |

### <a name="section-name-fabricnode"></a>Název oddílu: FabricNode
| **Parametr** | **Povolené hodnoty** | **Pokyny nebo krátký popis** |
| --- | --- | --- |
| StateTraceInterval |Čas v sekundách, výchozí hodnota je 300 |Zadejte časový interval v sekundách. Hello interval pro stav uzlu na každém uzlu a až uzly na FM/FMM trasování. |

### <a name="section-name-nodedomainids"></a>Název oddílu: NodeDomainIds
| **Parametr** | **Povolené hodnoty** | **Pokyny nebo krátký popis** |
| --- | --- | --- |
| UpgradeDomainId |Výchozí hodnota je řetězec, "" |Popisuje hello upgradovací doméně, ke které patří uzlu. |
| PropertyGroup – |NodeFaultDomainIdCollection |Popisuje hello domén selhání, ke které patří uzlu. doména selhání Hello je definována prostřednictvím identifikátor URI, který popisuje umístění hello hello uzlu v datovém centru hello.  Identifikátory URI domény selhání jsou hello formátu fd: / fd/následuje segment cesty identifikátoru URI.|

### <a name="section-name-nodeproperties"></a>Název oddílu: NodeProperties
| **Parametr** | **Povolené hodnoty** | **Pokyny nebo krátký popis** |
| --- | --- | --- |
| PropertyGroup – |NodePropertyCollectionMap |Kolekce párů klíč hodnota řetězce pro vlastnosti uzlu. |

### <a name="section-name-nodecapacities"></a>Název oddílu: NodeCapacities
| **Parametr** | **Povolené hodnoty** | **Pokyny nebo krátký popis** |
| --- | --- | --- |
| PropertyGroup – |NodeCapacityCollectionMap |Kolekce uzlu kapacity pro jiné metriky. |

### <a name="section-name-fabricnode"></a>Název oddílu: FabricNode
| **Parametr** | **Povolené hodnoty** | **Pokyny nebo krátký popis** |
| --- | --- | --- |
| StartApplicationPortRange |Int, výchozí hodnota je 0 |Začátek porty aplikace hello spravuje hostování subsystému. Vyžaduje, pokud EndpointFilteringEnabled platí v Hosting. |
| EndApplicationPortRange |Int, výchozí hodnota je 0 |Konec (ne včetně) porty aplikace hello spravuje hostování subsystému. Vyžaduje, pokud EndpointFilteringEnabled platí v Hosting. |
| ClusterX509StoreName |řetězec, výchozí hodnota je "Moje" |Název úložiště certifikátu X.509, který obsahuje certifikát clusteru pro zabezpečení komunikace mezi clustery. |
| ClusterX509FindType |řetězec, výchozí je "FindByThumbprint" |Určuje, jak toosearch pro cluster certifikátu v úložišti hello určeného ClusterX509StoreName podporované hodnoty: "FindByThumbprint"; "FindBySubjectName" s "FindBySubjectName"; Pokud existuje více shod; Hello jeden hello nejvzdálenější vypršení platnosti se používá. |
| ClusterX509FindValue |Výchozí hodnota je řetězec, "" |Hodnota filtru hledání používá certifikát toolocate clusteru. |
| ClusterX509FindValueSecondary |Výchozí hodnota je řetězec, "" |Hodnota filtru hledání používá certifikát toolocate clusteru. |
| ServerAuthX509StoreName |řetězec, výchozí hodnota je "Moje" |Název úložiště certifikátu X.509, který obsahuje certifikát serveru pro službu entrée. |
| ServerAuthX509FindType |řetězec, výchozí je "FindByThumbprint" |Určuje, jak toosearch pro certifikát serveru v úložišti hello určená hodnotou podporované ServerAuthX509StoreName: FindByThumbprint; FindBySubjectName. |
| ServerAuthX509FindValue |Výchozí hodnota je řetězec, "" |Hodnota filtru hledání používá toolocate certifikát serveru. |
| ServerAuthX509FindValueSecondary |Výchozí hodnota je řetězec, "" |Hodnota filtru hledání používá toolocate certifikát serveru. |
| ClientAuthX509StoreName |řetězec, výchozí hodnota je "Moje" |Název hello úložiště certifikátů X.509, který obsahuje certifikát pro roli správce výchozí FabricClient. |
| ClientAuthX509FindType |řetězec, výchozí je "FindByThumbprint" |Určuje, jak toosearch pro certifikát v úložišti hello určená hodnotou podporované ClientAuthX509StoreName: FindByThumbprint; FindBySubjectName. |
| ClientAuthX509FindValue |Výchozí hodnota je řetězec, "" | Hodnota filtru hledání použít pro roli správce výchozí FabricClient toolocate certifikátu. |
| ClientAuthX509FindValueSecondary |Výchozí hodnota je řetězec, "" |Hodnota filtru hledání použít pro roli správce výchozí FabricClient toolocate certifikátu. |
| UserRoleClientX509StoreName |řetězec, výchozí hodnota je "Moje" |Název hello úložiště certifikátů X.509, který obsahuje certifikát pro výchozí roli uživatele FabricClient. |
| UserRoleClientX509FindType |řetězec, výchozí je "FindByThumbprint" |Určuje, jak toosearch pro certifikát v úložišti hello určená hodnotou podporované UserRoleClientX509StoreName: FindByThumbprint; FindBySubjectName. |
| UserRoleClientX509FindValue |Výchozí hodnota je řetězec, "" |Hodnota filtru hledání používá toolocate certifikát pro výchozí roli uživatele FabricClient. |
| UserRoleClientX509FindValueSecondary |Výchozí hodnota je řetězec, "" |Hodnota filtru hledání používá toolocate certifikát pro výchozí roli uživatele FabricClient. |

### <a name="section-name-paas"></a>Název oddílu: Paas
| **Parametr** | **Povolené hodnoty** | **Pokyny nebo krátký popis** |
| --- | --- | --- |
| ClusterId |Výchozí hodnota je řetězec, "" |X509 certificate úložiště používané konvencemi prostředků infrastruktury pro ochranu konfigurace. |

### <a name="section-name-fabrichost"></a>Název oddílu: hostitele FabricHost
| **Parametr** | **Povolené hodnoty** | **Pokyny nebo krátký popis** |
| --- | --- | --- |
| StopTimeout |Čas v sekundách, výchozí hodnota je 300 |Zadejte časový interval v sekundách. Hello časový limit pro hostovanou službu Aktivace; deaktivace a upgradu. |
| StartTimeout |Čas v sekundách, výchozí hodnota je 300 |Zadejte časový interval v sekundách. Časový limit pro spuštění fabricactivationmanager. |
| ActivationRetryBackoffInterval |Čas v sekundách, výchozí hodnota je 5 |Zadejte časový interval v sekundách. Interval omezení rychlosti na každé selhání aktivace; na každý průběžné aktivace systému hello selhání bude opakovat hello aktivace pro až toohello MaxActivationFailureCount. interval opakování Hello na každý zkuste je produkt průběžné selhání a hello aktivace zpět mimo určený v intervalu aktivace. |
| ActivationMaxRetryInterval |Čas v sekundách, výchozí hodnota je 300 |Zadejte časový interval v sekundách. Maximální interval opakování pro aktivaci. Při každé opakování hello průběžné selhání interval vypočítá se jako minimální (ActivationMaxRetryInterval; Průběžné počet selhání * ActivationRetryBackoffInterval). |
| ActivationMaxFailureCount |Int, výchozí hodnota je 10 |Toto je maximální počet hello, pro který bude opakovat systému se nezdařila aktivace, než se předá. |
| EnableServiceFabricAutomaticUpdates |BOOL, výchozí hodnota je false |Toto je tooenable fabric automatické aktualizaci přes Windows Update. |
| EnableServiceFabricBaseUpgrade |BOOL, výchozí hodnota je false |Toto je základní aktualizaci tooenable pro server. |
| EnableRestartManagement |BOOL, výchozí hodnota je false |Toto je tooenable restartování serveru. |


### <a name="section-name-failovermanager"></a>Název oddílu: FailoverManager
| **Parametr** | **Povolené hodnoty** | **Pokyny nebo krátký popis** |
| --- | --- | --- |
| UserReplicaRestartWaitDuration |Čas v sekundách, výchozí hodnota je 60.0 * 30 |Zadejte časový interval v sekundách. Pokud repliku trvalou ocitne mimo provoz; Technologie Windows Fabric počká na této hodnotě duration pro hello repliky toocome zálohování před vytvořením nové repliky nahrazení (které by vyžadovaly kopii stavu hello). |
| QuorumLossWaitDuration |Čas v sekundách, výchozí hodnota je MaxValue |Zadejte časový interval v sekundách. Toto je hello maximální doba trvání, pro které jsou povoleny toobe oddíl ve stavu dojít ke ztrátě kvora. Pokud hello oddílu je stále ve ztrátě kvora po této hodnotě duration; oddíl Hello budou obnovena před ztrátou kvora že hello dolů repliky jako ztracené. Všimněte si, že to může potenciálně dojít ke ztrátě dat.. |
| UserStandByReplicaKeepDuration |Čas v sekundách, výchozí hodnota je 3600.0 * 24 * 7 |Zadejte časový interval v sekundách. Pokud repliku trvalou vraťte z dolní stav; může mít již bylo nahrazeno. Tento časovač Určuje, jak dlouho hello FM ponechá hello pohotovostní repliky před budou zrušeny. |
| UserMaxStandByReplicaCount |Int, výchozí hodnota je 1 |Hello výchozí maximální počet replik StandBy, které hello systému udržuje pro uživatele služby. |

### <a name="section-name-namingservice"></a>Název oddílu: NamingService
| **Parametr** | **Povolené hodnoty** | **Pokyny nebo krátký popis** |
| --- | --- | --- |
| TargetReplicaSetSize |Int, výchozí hodnota je 7 |Hello počet sad replik pro každý oddíl hello úložiště služby DNS. Zvýšením počtu hello repliky nastaví zvyšuje hello úroveň spolehlivosti hello informace v hello pojmenování úložiště služby; snížení hello změny, která hello informace budou ztraceny v důsledku selhání uzlu; s náklady zvýšené zatížení technologie Windows Fabric a hello množství času trvá tooperform aktualizace toohello pojmenování data.|
|MinReplicaSetSize | Int, výchozí hodnota je 3 | minimální počet replik služby DNS Hello vyžaduje toowrite do toocomplete aktualizace. Pokud jsou repliky méně než tento aktivní v systému hello hello spolehlivost systému odmítne aktualizace toohello pojmenování služby úložiště, dokud repliky jsou obnoveny. Tato hodnota by měla být nikdy více než hello TargetReplicaSetSize. |
|ReplicaRestartWaitDuration | Čas v sekundách, výchozí hodnota je (60.0 * 30)| Zadejte časový interval v sekundách. Když se replika služby DNS ocitne mimo provoz; Tento časovač se spustí.  Když vyprší platnost hello FM zahájíte tooreplace hello replik, které jsou dolů (ho ještě nepovažuje jejich ztráty). |
|QuorumLossWaitDuration | Čas v sekundách, výchozí hodnota je MaxValue | Zadejte časový interval v sekundách. Pokud získá služby DNS do ztrátě kvora; Tento časovač se spustí.  Když vyprší platnost hello FM zvažovat hello dolů repliky jako ztracené; a pokus o toorecover kvora. Ne to, jestli to může způsobit ztrátu dat. |
|StandByReplicaKeepDuration | Čas v sekundách, výchozí hodnota je 3600.0 * 2 | Zadejte časový interval v sekundách. Když repliky služby DNS vraťte z dolní stav; může mít již bylo nahrazeno.  Tento časovač Určuje, jak dlouho hello FM ponechá hello pohotovostní repliky před budou zrušeny. |
|PlacementConstraints | Výchozí hodnota je řetězec, "" | Omezení umístění pro hello služby DNS. |
|ServiceDescriptionCacheLimit | Int, výchozí hodnota je 0 | Hello maximálním počtem položek udržuje v mezipaměti popis služby hodnoty nejdelšího Nepoužití hello v hello úložiště služby DNS (nastavit too0 pro žádné omezení). |
|RepairInterval | Čas v sekundách, výchozí hodnota je 5 | Zadejte časový interval v sekundách. Interval, ve které hello se spustí pojmenování opravit nekonzistence mezi hello autority vlastníka a název vlastníka. |
|MaxNamingServiceHealthReports | Int, výchozí hodnota je 10 | maximální počet Hello pomalé operací, které ukládání pojmenování služby najednou sestavy není v pořádku. Pokud 0; všechny operace pomalé se odesílají. |
| MaxMessageSize |Int, výchozí hodnota je 4*1024*1024 |Hello maximální velikost zprávy pro komunikaci klienta uzlu při použití názvy. Zmírnění útok DOS; Výchozí hodnota je 4 MB volného místa. |
| MaxFileOperationTimeout |Čas v sekundách, výchozí hodnota je 30 |Zadejte časový interval v sekundách. Hello maximální časový limit povolená pro operace služby úložiště souborů. Určení delší časový limit žádosti budou odmítnuty. |
| Jako MaxOperationTimeout |Čas v sekundách, výchozí hodnota je 600 |Zadejte časový interval v sekundách. Hello maximální časový limit povolená pro operace klienta. Určení delší časový limit žádosti budou odmítnuty. |
| MaxClientConnections |Int, výchozí hodnota je 1 000 |Hello maximální povolený počet připojení klientů k jednomu brány. |
| ServiceNotificationTimeout |Čas v sekundách, výchozí hodnota je 30 |Zadejte časový interval v sekundách. časový limit Hello používá při doručování klienta toohello oznámení služby. |
| MaxOutstandingNotificationsPerClient |Int, výchozí hodnota je 1 000 |maximální počet nezpracovaných oznámení před registrace klienta Hello je vynuceně ukončeno hello brány. |
| MaxIndexedEmptyPartitions |Int, výchozí hodnota je 1 000 |indexované Hello maximální počet prázdné oddíly, které zůstanou v mezipaměti hello oznámení pro synchronizaci opětovně se připojujícího klientů. Všechny prázdné oddíly výše toto číslo se odebere z indexu hello ve vzestupném pořadí verze vyhledávání. Připojení klienti stále synchronizovat a přijímat aktualizace zmeškaných prázdný oddílu; ale hello synchronizačního protokolu stane nákladnější. |
| GatewayServiceDescriptionCacheLimit |Int, výchozí hodnota je 0 |Hello maximálním počtem položek udržuje v mezipaměti popis služby hodnoty nejdelšího Nepoužití hello v hello pojmenování brány (nastavit too0 pro žádné omezení). |
| PartitionCount |Int, výchozí hodnota je 3 |Hello počet oddílů hello služby DNS úložiště toobe vytvořili. Každý oddíl vlastní klíč jeden oddíl, který odpovídá tooits index; Ano klíče oddílů [0; PartitionCount) neexistuje. Nastavení služby DNS oddíly zvýší hello měřítka, kterému hello služby DNS můžete provádět v snížením hello průměrný objem data ukládaná společností všechny základní replika se zvyšující číslo hello; za cenu vyšší využití prostředků (od PartitionCount * ReplicaSetSize služby repliky musí být zachovaná).|

### <a name="section-name-runas"></a>Název oddílu: RunAs
| **Parametr** | **Povolené hodnoty** | **Pokyny nebo krátký popis** |
| --- | --- | --- |
| RunAsAccountName |Výchozí hodnota je řetězec, "" |Určuje název účtu RunAs hello. To je potřeba jenom pro "Typu DomainUser" nebo "ManagedServiceAccount" účet typu. Platné hodnoty jsou "doména\uživatel" nebo "user@domain". |
|RunAsAccountType|Výchozí hodnota je řetězec, "" |Určuje typ účtu RunAs hello. To je potřeba pro všechny RunAs části platné hodnoty jsou "Typu DomainUser nebo NetworkService nebo ManagedServiceAccount nebo LocalSystem".|
|Spustit_jako_heslo|Výchozí hodnota je řetězec, "" |Určuje heslo pro účet RunAs hello. To je potřeba jenom pro typ účtu "Typu DomainUser". |

### <a name="section-name-runasfabric"></a>Název oddílu: RunAs_Fabric
| **Parametr** | **Povolené hodnoty** | **Pokyny nebo krátký popis** |
| --- | --- | --- |
| RunAsAccountName |Výchozí hodnota je řetězec, "" |Určuje název účtu RunAs hello. To je potřeba jenom pro "Typu DomainUser" nebo "ManagedServiceAccount" účet typu. Platné hodnoty jsou "doména\uživatel" nebo "user@domain". |
|RunAsAccountType|Výchozí hodnota je řetězec, "" |Určuje typ účtu RunAs hello. To je potřeba pro všechny RunAs části platné hodnoty jsou "LocalUser/typu DomainUser nebo NetworkService nebo ManagedServiceAccount nebo LocalSystem". |
|Spustit_jako_heslo|Výchozí hodnota je řetězec, "" |Určuje heslo pro účet RunAs hello. To je potřeba jenom pro typ účtu "Typu DomainUser". |

### <a name="section-name-runashttpgateway"></a>Název oddílu: RunAs_HttpGateway
| **Parametr** | **Povolené hodnoty** | **Pokyny nebo krátký popis** |
| --- | --- | --- |
| RunAsAccountName |Výchozí hodnota je řetězec, "" |Určuje název účtu RunAs hello. To je potřeba jenom pro "Typu DomainUser" nebo "ManagedServiceAccount" účet typu. Platné hodnoty jsou "doména\uživatel" nebo "user@domain". |
|RunAsAccountType|Výchozí hodnota je řetězec, "" |Určuje typ účtu RunAs hello. To je potřeba pro všechny RunAs části platné hodnoty jsou "LocalUser/typu DomainUser nebo NetworkService nebo ManagedServiceAccount nebo LocalSystem". |
|Spustit_jako_heslo|Výchozí hodnota je řetězec, "" |Určuje heslo pro účet RunAs hello. To je potřeba jenom pro typ účtu "Typu DomainUser". |

### <a name="section-name-runasdca"></a>Název oddílu: RunAs_DCA
| **Parametr** | **Povolené hodnoty** | **Pokyny nebo krátký popis** |
| --- | --- | --- |
| RunAsAccountName |Výchozí hodnota je řetězec, "" |Určuje název účtu RunAs hello. To je potřeba jenom pro "Typu DomainUser" nebo "ManagedServiceAccount" účet typu. Platné hodnoty jsou "doména\uživatel" nebo "user@domain". |
|RunAsAccountType|Výchozí hodnota je řetězec, "" |Určuje typ účtu RunAs hello. To je potřeba pro všechny RunAs části platné hodnoty jsou "LocalUser/typu DomainUser nebo NetworkService nebo ManagedServiceAccount nebo LocalSystem". |
|Spustit_jako_heslo|Výchozí hodnota je řetězec, "" |Určuje heslo pro účet RunAs hello. To je potřeba jenom pro typ účtu "Typu DomainUser". |

### <a name="section-name-httpgateway"></a>Název oddílu: HttpGateway
| **Parametr** | **Povolené hodnoty** | **Pokyny nebo krátký popis** |
| --- | --- | --- |
|Hodnotu IsEnabled|BOOL, výchozí hodnota je false | Povolí nebo zakáže hello httpgateway. Ve výchozím nastavení vypnutá Httpgateway a tato konfigurace musí toobe sady tooenable ho. |
|ActiveListeners |Uint, výchozí hodnota je 50 | Počet čtení toopost toohello http serveru fronty. Tato volba určuje hello počet souběžných požadavků, které může obsloužit hello HttpGateway. |
|MaxEntityBodySize |Uint, výchozí hodnota je 4194304 |  Poskytuje hello maximální velikost textu hello, které se dají očekávat z požadavku http. Výchozí hodnota je 4 MB volného místa. Httpgateway nepodaří žádost, pokud má text velikosti > tuto hodnotu. Velikost minimální čtení bloku je 4096 bajtů. Aby to má toobe > = 4096. |
|HttpGatewayHealthReportSendInterval |Čas v sekundách, výchozí hodnota je 30 | Zadejte časový interval v sekundách. Hello interval, ve které hello Http brány odesílá nahromadění tooHealth sestavy health Manager. |

### <a name="section-name-ktllogger"></a>Název oddílu: KtlLogger
| **Parametr** | **Povolené hodnoty** | **Pokyny nebo krátký popis** |
| --- | --- | --- |
|AutomaticMemoryConfiguration |Int, výchozí hodnota je 1 | Příznak, který indikuje, pokud nastavení paměti hello by měl být automaticky a dynamicky nakonfigurovaný. Pokud nulové pak nastavení konfigurace paměti hello používají přímo a se nezmění na základě systému podmínek. Pokud pak hello paměti nastavení jsou konfigurovány automaticky a může se změnit na základě podmínky systému. |
|WriteBufferMemoryPoolMinimumInKB |Int, výchozí hodnota je 8388608 |Hello počet KB tooinitially přidělit fondu hello zápis vyrovnávací paměti. Použijte hodnotu 0 tooindicate omezení není nastaveno výchozí by měla být v souladu s SharedLogSizeInMB níže. |
|WriteBufferMemoryPoolMaximumInKB | Int, výchozí hodnota je 0 |Hello počet KB tooallow hello zápisu toogrow fondu vyrovnávací paměti než. Použijte 0 tooindicate žádné omezení. |
|MaximumDestagingWriteOutstandingInKB | Int, výchozí hodnota je 0 | Hello počet KB tooallow hello sdílené protokolu tooadvance před hello vyhrazené protokolu. Použijte 0 tooindicate žádné omezení.
|SharedLogPath |Výchozí hodnota je řetězec, "" | Cesta a název toolocation tooplace sdílené protokolu kontejneru. Použití "" pro použití výchozí cestu v kořenovém adresáři data prostředků infrastruktury. |
|SharedLogId |Výchozí hodnota je řetězec, "" |Jedinečný identifikátor guid pro kontejner sdílené protokolu. Použití "" Pokud se používá výchozí cestu v kořenovém adresáři data prostředků infrastruktury. |
|SharedLogSizeInMB |Int, výchozí hodnota je 8192 | Hello počet MB tooallocate v kontejneru sdílené protokolu hello. |

### <a name="section-name-applicationgatewayhttp"></a>Název oddílu: ApplicationGateway/Http
| **Parametr** | **Povolené hodnoty** | **Pokyny nebo krátký popis** |
| --- | --- | --- |
|Hodnotu IsEnabled |BOOL, výchozí hodnota je false | Povolí nebo zakáže hello HttpApplicationGateway. Ve výchozím nastavení vypnutá HttpApplicationGateway a tato konfigurace musí toobe sady tooenable ho. |
|NumberOfParallelOperations | Uint, výchozí hodnota je 5000 | Počet čtení toopost toohello http serveru fronty. Tato volba určuje hello počet souběžných požadavků, které může obsloužit hello HttpGateway. |
|DefaultHttpRequestTimeout |Čas v sekundách. Výchozí hodnota je 120. |Zadejte časový interval v sekundách.  Časový limit požadavku výchozí hello dává požadavků http hello zpracovávána v bráně aplikace hello http. |
|ResolveServiceBackoffInterval |Čas v sekundách, výchozí hodnota je 5 |Zadejte časový interval v sekundách.  Poskytuje hello výchozí back vypnout interval před opakováním operace služby se nezdařilo vyřešit. |
|BodyChunkSize |Uint, výchozí hodnota je 16384 |  Poskytuje velikost hello u hello bloku v bajtech použít tooread hello textu. |
|GatewayAuthCredentialType |řetězec, výchozí je "Žádný" | Určuje typ hello toouse pověření zabezpečení v hello http aplikace brána koncový bod platné hodnoty jsou "žádné / X 509. |
|GatewayX509CertificateStoreName |řetězec, výchozí hodnota je "Moje" | Název úložiště certifikátu X.509, který obsahuje certifikát pro bránu aplikace http. |
|GatewayX509CertificateFindType |řetězec, výchozí je "FindByThumbprint" | Určuje, jak toosearch pro certifikát v úložišti hello určená hodnotou podporované GatewayX509CertificateStoreName: FindByThumbprint; FindBySubjectName. |
|GatewayX509CertificateFindValue | Výchozí hodnota je řetězec, "" | Hodnota filtru hledání používá certifikát brány toolocate hello http aplikace. Tento certifikát je nakonfigurovaná na koncový bod https hello a lze také použít tooverify hello identity aplikace hello v případě potřeby službami hello. FindValue se hledá první; a pokud neexistuje, FindValueSecondary vyhledávat. |
|GatewayX509CertificateFindValueSecondary | Výchozí hodnota je řetězec, "" |Hodnota filtru hledání používá certifikát brány toolocate hello http aplikace. Tento certifikát je nakonfigurovaná na koncový bod https hello a lze také použít tooverify hello identity aplikace hello v případě potřeby službami hello. FindValue se hledá první; a pokud neexistuje, FindValueSecondary vyhledávat.|

### <a name="section-name-management"></a>Název oddílu: správy
| **Parametr** | **Povolené hodnoty** | **Pokyny nebo krátký popis** |
| --- | --- | --- |
| Parametr ImageStoreConnectionString |SecureString | Připojovací řetězec toohello kořenovém pro úložiště bitových kopií. |
| ImageStoreMinimumTransferBPS | Int, výchozí hodnota je 1 024 |Minimální přenosová rychlost Hello mezi hello clusteru a úložiště bitových kopií. Tato hodnota je použité toodetermine hello vypršení časového limitu při přístupu k hello externí úložiště bitových kopií. Tuto hodnotu změňte, pouze pokud je latence hello mezi hello clusteru a úložiště bitových kopií vysoké tooallow více času hello clusteru toodownload z hello externí úložiště bitových kopií. |
|AzureStorageMaxWorkerThreads | Int, výchozí hodnota je 25 | Hello maximální počet pracovních vláken současně. |
|AzureStorageMaxConnections | Int, výchozí hodnota je 5000 | Hello maximální počet souběžných připojení tooazure úložiště. |
|AzureStorageOperationTimeout | Čas v sekundách, výchozí hodnota je 6000 | Zadejte časový interval v sekundách. Časový limit pro toocomplete xstore operaci. |
|ImageCachingEnabled | BOOL, výchozí hodnota je true | Tato konfigurace umožňuje nám tooenable nebo zakázat ukládání do mezipaměti. |
|DisableChecksumValidation | BOOL, výchozí hodnota je false | Tato konfigurace umožňuje nám tooenable nebo zakázat ověření kontrolního součtu při zřizování aplikace. |
|DisableServerSideCopy | BOOL, výchozí hodnota je false | Tato konfigurace povolí nebo zakáže kopii na straně serveru balíčku aplikace na hello úložiště bitových kopií při zřizování aplikace. |

### <a name="section-name-healthmanagerclusterhealthpolicy"></a>Název oddílu: HealthManager/ClusterHealthPolicy
| **Parametr** | **Povolené hodnoty** | **Pokyny nebo krátký popis** |
| --- | --- | --- |
| ConsiderWarningAsError |BOOL, výchozí hodnota je false |Cluster zásad vyhodnocení stavu: upozornění jsou považovány za chyby. |
| MaxPercentUnhealthyNodes | Int, výchozí hodnota je 0 |Cluster zásad vyhodnocení stavu: maximální procento není v pořádku uzly povolené pro hello clusteru toobe v pořádku. |
| MaxPercentUnhealthyApplications | Int, výchozí hodnota je 0 |Cluster zásad vyhodnocení stavu: maximální procento není v pořádku aplikace povolené pro hello clusteru toobe v pořádku. |
|MaxPercentDeltaUnhealthyNodes | Int, výchozí hodnota je 10 |Cluster zásad vyhodnocení stavu upgradu: maximální procento rozdílů není v pořádku uzly povolené pro hello clusteru toobe v pořádku. |
|MaxPercentUpgradeDomainDeltaUnhealthyNodes | Int, výchozí hodnota je 15 |Cluster zásad vyhodnocení stavu upgradu: maximální procento rozdílů není v pořádku uzlů v doméně upgradu povolené pro hello clusteru toobe v pořádku.|

### <a name="section-name-faultanalysisservice"></a>Název oddílu: FaultAnalysisService
| **Parametr** | **Povolené hodnoty** | **Pokyny nebo krátký popis** |
| --- | --- | --- |
| TargetReplicaSetSize |Int, výchozí hodnota je 0 |NOT_PLATFORM_UNIX_START hello TargetReplicaSetSize pro FaultAnalysisService. |
| MinReplicaSetSize |Int, výchozí hodnota je 0 |Hello MinReplicaSetSize pro FaultAnalysisService. |
| ReplicaRestartWaitDuration |Čas v sekundách, výchozí hodnota je 60 minut|Zadejte časový interval v sekundách. Hello ReplicaRestartWaitDuration pro FaultAnalysisService. |
| QuorumLossWaitDuration | Čas v sekundách, výchozí hodnota je MaxValue |Zadejte časový interval v sekundách. Hello QuorumLossWaitDuration pro FaultAnalysisService. |
| StandByReplicaKeepDuration| Čas v sekundách, výchozí hodnota je (60*24*7) minut |Zadejte časový interval v sekundách. Hello StandByReplicaKeepDuration pro FaultAnalysisService. |
| PlacementConstraints | Výchozí hodnota je řetězec, ""| Hello PlacementConstraints pro FaultAnalysisService. |
| StoredActionCleanupIntervalInSeconds | Int, výchozí hodnota je 3600 |Toto je, jak často hello úložiště se vyčistí.  Pouze akce ve stavu terminálu; a která alespoň dokončena CompletedActionKeepDurationInSeconds před bude odebrán. |
| CompletedActionKeepDurationInSeconds | Int, výchozí hodnota je 604800 | Toto je přibližně jak dlouho tookeep akce, které jsou ve stavu terminálu.  To také závisí na StoredActionCleanupIntervalInSeconds; vzhledem k tomu, že toocleanup pracovní hello se provádí pouze v tomto intervalu. 604800 je 7 dní. |
| StoredChaosEventCleanupIntervalInSeconds | Int, výchozí hodnota je 3600 |Toto je, jak často hello úložiště auditování pro čištění; Pokud hello počet událostí je více než 30000; Vyčištění Hello se nové. |

### <a name="section-name-filestoreservice"></a>Název oddílu: FileStoreService
| **Parametr** | **Povolené hodnoty** | **Pokyny nebo krátký popis** |
| --- | --- | --- |
| NamingOperationTimeout |Čas v sekundách, výchozí hodnota je 60 |Zadejte časový interval v sekundách. časový limit Hello k provedení pojmenování operace. |
| QueryOperationTimeout | Čas v sekundách, výchozí hodnota je 60 |Zadejte časový interval v sekundách. časový limit Hello k provedení operace dotazu. |
| MaxCopyOperationThreads | Uint, výchozí hodnota je 0 | maximální počet paralelní souborů Hello této sekundární můžete zkopírovat z primární. '0' == počet jader. |
| MaxFileOperationThreads | Uint, výchozí hodnota je 100 | Hello maximální počet povolených tooperform paralelních vláken FileOperations (kopírování nebo přesunutí) v primární hello. '0' == počet jader. |
| MaxStoreOperations | Uint, výchozí hodnota je 4096. |Hello maximální počet operací transcation paralelní úložiště povolené na primárním prostředku. '0' == počet jader. |
| MaxRequestProcessingThreads | Uint, výchozí hodnota je 200 |maximální počet paralelních vláken Hello povolené tooprocess požadavky v hello primární. '0' == počet jader. |
| MaxSecondaryFileCopyFailureThreshold | Uint, výchozí hodnota je 25| Hello maximální počet opakování kopírování souboru na sekundární než hello. |
| AnonymousAccessEnabled | BOOL, výchozí hodnota je true |Povolí nebo zakáže toohello anonymní přístup, který sdílí FileStoreService. |
| PrimaryAccountType | Výchozí hodnota je řetězec, "" |primární Hello AccountType hello hlavní tooACL hello FileStoreService sdílí. |
| PrimaryAccountUserName | Výchozí hodnota je řetězec, "" |Hello primární účet uživatelské jméno hello hlavní tooACL hello FileStoreService sdílených složek. |
| PrimaryAccountUserPassword | SecureString, výchozí hodnota je prázdná |heslo účtu primární Hello hello hlavní tooACL hello FileStoreService sdílí. |
| FileStoreService | PrimaryAccountNTLMPasswordSecret | SecureString, výchozí hodnota je prázdná | tajný klíč Hello heslo, které používá jako počáteční hodnoty toogenerated stejné heslo, když pomocí ověřování NTLM. |
| PrimaryAccountNTLMX509StoreLocation | řetězec, výchozí je "LocalMachine"| umístění úložiště Hello hello X509 certifikátu používá toogenerate HMAC na hello PrimaryAccountNTLMPasswordSecret při použití ověřování protokolem NTLM. |
| PrimaryAccountNTLMX509StoreName | řetězec, výchozí je "MY"| Název úložiště Hello hello X509 certifikát používaný toogenerate HMAC na hello PrimaryAccountNTLMPasswordSecret při použití ověřování protokolem NTLM. |
| PrimaryAccountNTLMX509Thumbprint | Výchozí hodnota je řetězec, ""|Hello kryptografický otisk certifikátu hello X509 používá toogenerate HMAC na hello PrimaryAccountNTLMPasswordSecret při použití ověřování protokolem NTLM. |
| SecondaryAccountType | Výchozí hodnota je řetězec, ""| sekundární Hello AccountType hello hlavní tooACL hello FileStoreService sdílí. |
| SecondaryAccountUserName | Výchozí hodnota je řetězec, ""| Hello sekundární účet uživatelské jméno hello hlavní tooACL hello FileStoreService sdílených složek. |
| SecondaryAccountUserPassword | SecureString, výchozí hodnota je prázdná |heslo účtu sekundární Hello hello hlavní tooACL hello FileStoreService sdílí.  |
| SecondaryAccountNTLMPasswordSecret | SecureString, výchozí hodnota je prázdná | tajný klíč Hello heslo, které používá jako počáteční hodnoty toogenerated stejné heslo, když pomocí ověřování NTLM. |
| SecondaryAccountNTLMX509StoreLocation | řetězec, výchozí je "LocalMachine" |umístění úložiště Hello hello X509 certifikátu používá toogenerate HMAC na hello SecondaryAccountNTLMPasswordSecret při použití ověřování protokolem NTLM. |
| SecondaryAccountNTLMX509StoreName | řetězec, výchozí je "MY" |Název úložiště Hello hello X509 certifikát používaný toogenerate HMAC na hello SecondaryAccountNTLMPasswordSecret při použití ověřování protokolem NTLM. |
| SecondaryAccountNTLMX509Thumbprint | Výchozí hodnota je řetězec, ""| Hello kryptografický otisk certifikátu hello X509 používá toogenerate HMAC na hello SecondaryAccountNTLMPasswordSecret při použití ověřování protokolem NTLM. |

### <a name="section-name-imagestoreservice"></a>Název oddílu: ImageStoreService
| **Parametr** | **Povolené hodnoty** | **Pokyny nebo krátký popis** |
| --- | --- | --- |
| Povoleno |BOOL, výchozí hodnota je false |Příznak povoleno pro ImageStoreService Hello. |
| TargetReplicaSetSize | Int, výchozí hodnota je 7 |Hello TargetReplicaSetSize pro ImageStoreService. |
| MinReplicaSetSize | Int, výchozí hodnota je 3 |Hello MinReplicaSetSize pro ImageStoreService. |
| ReplicaRestartWaitDuration | Čas v sekundách, výchozí hodnota je 60.0 * 30 | Zadejte časový interval v sekundách. Hello ReplicaRestartWaitDuration pro ImageStoreService. |
| QuorumLossWaitDuration | Čas v sekundách, výchozí hodnota je MaxValue | Zadejte časový interval v sekundách. Hello QuorumLossWaitDuration pro ImageStoreService. |
| StandByReplicaKeepDuration | Čas v sekundách, výchozí hodnota je 3600.0 * 2 | Zadejte časový interval v sekundách. Hello StandByReplicaKeepDuration pro ImageStoreService. |
| PlacementConstraints | Výchozí hodnota je řetězec, "" | Hello PlacementConstraints pro ImageStoreService. |
| ClientUploadTimeout | Čas v sekundách, výchozí hodnota je 1 800 |Zadejte časový interval v sekundách. Hodnota časového limitu pro nejvyšší úrovně odeslání žádosti o službu tooImage úložiště. |
| ClientCopyTimeout | Čas v sekundách, výchozí hodnota je 1 800 | Zadejte časový interval v sekundách. Hodnota časového limitu pro nejvyšší úrovně kopie žádosti o službu tooImage úložiště. |
| ClientDownloadTimeout | Čas v sekundách, výchozí hodnota je 1 800 | Zadejte časový interval v sekundách. Hodnota časového limitu pro nejvyšší úrovně stažení žádosti o službu tooImage úložiště |
| ClientListTimeout | Čas v sekundách, výchozí hodnota je 600 | Zadejte časový interval v sekundách. Hodnota časového limitu pro nejvyšší úrovně seznamu žádosti o službu tooImage úložiště. |
| ClientDefaultTimeout | Čas v sekundách, výchozí hodnota je 180 | Zadejte časový interval v sekundách. Hodnota časového limitu pro všechny požadavky bez odeslání nebo stažení (např. existuje, odstraňte) tooImage služby úložiště. |

### <a name="section-name-imagestoreclient"></a>Název oddílu: ImageStoreClient
| **Parametr** | **Povolené hodnoty** | **Pokyny nebo krátký popis** |
| --- | --- | --- |
| ClientUploadTimeout |Čas v sekundách, výchozí hodnota je 1 800 | Zadejte časový interval v sekundách. Hodnota časového limitu pro nejvyšší úrovně odeslání žádosti o službu tooImage úložiště. |
| ClientCopyTimeout | Čas v sekundách, výchozí hodnota je 1 800 | Zadejte časový interval v sekundách. Hodnota časového limitu pro nejvyšší úrovně kopie žádosti o službu tooImage úložiště. |
|ClientDownloadTimeout | Čas v sekundách, výchozí hodnota je 1 800 | Zadejte časový interval v sekundách. Hodnota časového limitu pro nejvyšší úrovně stažení žádosti o službu tooImage úložiště. |
|ClientListTimeout | Čas v sekundách, výchozí hodnota je 600 |Zadejte časový interval v sekundách. Hodnota časového limitu pro nejvyšší úrovně seznamu žádosti o službu tooImage úložiště. |
|ClientDefaultTimeout | Čas v sekundách, výchozí hodnota je 180 | Zadejte časový interval v sekundách. Hodnota časového limitu pro všechny požadavky bez odeslání nebo stažení (např. existuje, odstraňte) tooImage služby úložiště. |

### <a name="section-name-tokenvalidationservice"></a>Název oddílu: TokenValidationService
| **Parametr** | **Povolené hodnoty** | **Pokyny nebo krátký popis** |
| --- | --- | --- |
| Poskytovatelé |řetězec, výchozí je "Službu DSTS." |Čárkami oddělený seznam tooenable zprostředkovatelů ověření tokenu (platný poskytovatelé jsou: DSTS; AAD). Aktuálně lze kdykoli povolit pouze jednoho zprostředkovatele. |

### <a name="section-name-upgradeorchestrationservice"></a>Název oddílu: UpgradeOrchestrationService
| **Parametr** | **Povolené hodnoty** | **Pokyny nebo krátký popis** |
| --- | --- | --- |
| TargetReplicaSetSize |Int, výchozí hodnota je 0 |Hello TargetReplicaSetSize pro UpgradeOrchestrationService. |
| MinReplicaSetSize |Int, výchozí hodnota je 0 | Hello MinReplicaSetSize pro UpgradeOrchestrationService.
| ReplicaRestartWaitDuration | Čas v sekundách, výchozí hodnota je 60 minut| Zadejte časový interval v sekundách. Hello ReplicaRestartWaitDuration pro UpgradeOrchestrationService. |
| QuorumLossWaitDuration | Čas v sekundách, výchozí hodnota je MaxValue | Zadejte časový interval v sekundách. Hello QuorumLossWaitDuration pro UpgradeOrchestrationService. |
| StandByReplicaKeepDuration | Čas v sekundách, výchozí hodnota je 60*24*7 minut | Zadejte časový interval v sekundách. Hello StandByReplicaKeepDuration pro UpgradeOrchestrationService. |
| PlacementConstraints | Výchozí hodnota je řetězec, "" | Hello PlacementConstraints pro UpgradeOrchestrationService. |
| Autoupgradeenabled – | BOOL, výchozí hodnota je true | Automatické dotazování a upgradu akce v závislosti na soubor stavu cíle. |
| UpgradeApprovalRequired | BOOL, výchozí hodnota je false | Nastavení toomake kód upgradu vyžadovat schválení správce, než budete pokračovat. |

### <a name="section-name-upgradeservice"></a>Název oddílu: UpgradeService
| **Parametr** | **Povolené hodnoty** | **Pokyny nebo krátký popis** |
| --- | --- | --- |
| PlacementConstraints |Výchozí hodnota je řetězec, "" |Hello PlacementConstraints upgradu služby. |
| TargetReplicaSetSize | Int, výchozí hodnota je 3 | Hello TargetReplicaSetSize pro UpgradeService. |
| MinReplicaSetSize | Int, výchozí hodnota je 2 | Hello MinReplicaSetSize pro UpgradeService. |
| CoordinatorType | řetězec, výchozí je "WUTest"| Hello CoordinatorType pro UpgradeService. |
| BaseUrl | Výchozí hodnota je řetězec, "" |BaseUrl pro UpgradeService. |
| ClusterId | Výchozí hodnota je řetězec, "" | ClusterId pro UpgradeService. |
| X509StoreName | řetězec, výchozí hodnota je "Moje"| X509StoreName pro UpgradeService. |
| X509StoreLocation | Výchozí hodnota je řetězec, "" | X509StoreLocation pro UpgradeService. |
| X509FindType | Výchozí hodnota je řetězec, ""| X509FindType pro UpgradeService. |
| X509FindValue | Výchozí hodnota je řetězec, "" | X509FindValue pro UpgradeService. |
| X509SecondaryFindValue | Výchozí hodnota je řetězec, "" | X509SecondaryFindValue pro UpgradeService. |
| OnlyBaseUpgrade | BOOL, výchozí hodnota je false | OnlyBaseUpgrade pro UpgradeService. |
| TestCabFolder | Výchozí hodnota je řetězec, "" | TestCabFolder pro UpgradeService. |

### <a name="section-name-securityclientaccess"></a>Název oddílu: Zabezpečení nebo ClientAccess
| **Parametr** | **Povolené hodnoty** | **Pokyny nebo krátký popis** |
| --- | --- | --- |
| CreateName |řetězec, výchozí je "Admin" |Konfigurace zabezpečení pro identifikátor URI pojmenování vytvoření. |
| DeleteName |řetězec, výchozí je "Admin" |Konfigurace zabezpečení pro identifikátor URI pojmenování odstranění. |
| PropertyWriteBatch |řetězec, výchozí je "Admin" |Konfigurace zabezpečení pro pojmenování vlastnost operací zápisu. |
| CreateService |řetězec, výchozí je "Admin" | Konfigurace zabezpečení k tvorbě služeb. |
| CreateServiceFromTemplate |řetězec, výchozí je "Admin" |Konfigurace zabezpečení pro vytvoření služby ze šablony. |
| UpdateService |řetězec, výchozí je "Admin" |Konfigurace zabezpečení pro aktualizace služby. |
| DeleteService  |řetězec, výchozí je "Admin" |Konfigurace zabezpečení pro odstranění služby. |
| ProvisionApplicationType |řetězec, výchozí je "Admin" | Konfigurace zabezpečení pro aplikace typ zřizování. |
| CreateApplication |řetězec, výchozí je "Admin" | Konfigurace zabezpečení pro vytvoření aplikace. |
| DeleteApplication |řetězec, výchozí je "Admin" | Konfigurace zabezpečení pro odstranění aplikace. |
| UpgradeApplication |řetězec, výchozí je "Admin" | Konfigurace zabezpečení pro spuštění nebo přerušení upgrady aplikací. |
| RollbackApplicationUpgrade |řetězec, výchozí je "Admin" | Konfigurace zabezpečení pro vrácení zpět upgrady aplikací. |
| UnprovisionApplicationType |řetězec, výchozí je "Admin" | Konfigurace zabezpečení k rušení zajišťování typ aplikace. |
| MoveNextUpgradeDomain |řetězec, výchozí je "Admin" | Konfigurace zabezpečení pro obnovení upgradů aplikací s explicitní doménou upgradu. |
| ReportUpgradeHealth |řetězec, výchozí je "Admin" | Konfigurace zabezpečení pro obnovení upgradů aplikací s aktuální průběh upgradu hello. |
| ReportHealth |řetězec, výchozí je "Admin" | Konfigurace zabezpečení pro vytváření sestav stavu. |
| ProvisionFabric |řetězec, výchozí je "Admin" | Konfigurace zabezpečení pro MSI nebo clusteru manifestu zřizování. |
| UpgradeFabric |řetězec, výchozí je "Admin" | Konfigurace zabezpečení pro spuštění clusteru upgrady. |
| RollbackFabricUpgrade |řetězec, výchozí je "Admin" | Konfigurace zabezpečení pro vrácení zpět upgrady clusteru. |
| UnprovisionFabric |řetězec, výchozí je "Admin" | Konfigurace zabezpečení pro MSI nebo clusteru manifestu rušení zajišťování. |
| MoveNextFabricUpgradeDomain |řetězec, výchozí je "Admin" | Konfigurace zabezpečení pro obnovení upgradu clusteru s výslovně upgradu domény. |
| ReportFabricUpgradeHealth |řetězec, výchozí je "Admin" | Konfigurace zabezpečení pro obnovení upgradu clusteru s hello aktuální průběh upgradu. |
| StartInfrastructureTask |řetězec, výchozí je "Admin" | Konfigurace zabezpečení pro spuštění úlohy infrastruktury. |
| FinishInfrastructureTask |řetězec, výchozí je "Admin" | Konfigurace zabezpečení pro úlohy nepřipojujte infrastruktury. |
| ActivateNode |řetězec, výchozí je "Admin" | Konfigurace zabezpečení pro aktivaci a uzlu. |
| DeactivateNode |řetězec, výchozí je "Admin" | Konfigurace zabezpečení pro deaktivace uzlu. |
| DeactivateNodesBatch |řetězec, výchozí je "Admin" | Konfigurace zabezpečení pro deaktivace víc uzlů. |
| RemoveNodeDeactivations |řetězec, výchozí je "Admin" | Konfigurace zabezpečení pro navrácení deaktivace ve více uzlech. |
| GetNodeDeactivationStatus |řetězec, výchozí je "Admin" | Kontrola stavu deaktivace konfigurace zabezpečení. |
| NodeStateRemoved |řetězec, výchozí je "Admin" | Konfigurace zabezpečení pro vytváření sestav stavu uzel odebrán. |
| RecoverPartition |řetězec, výchozí je "Admin" | Konfigurace zabezpečení pro obnovení oddílu. |
| RecoverPartitions |řetězec, výchozí je "Admin" | Konfigurace zabezpečení pro obnovení oddíly. |
| RecoverServicePartitions |řetězec, výchozí je "Admin" | Konfigurace zabezpečení pro obnovení oddílů služby. |
| RecoverSystemPartitions |řetězec, výchozí je "Admin" | Konfigurace zabezpečení pro obnovení systému oddílů služby. |
| ReportFault |řetězec, výchozí je "Admin" | Konfigurace zabezpečení pro vytváření sestav chyb. |
| InvokeInfrastructureCommand |řetězec, výchozí je "Admin" | Konfigurace zabezpečení pro infrastrukturu úloh správy příkazy. |
| FileContent |řetězec, výchozí je "Admin" | Konfigurace zabezpečení pro bitovou kopii ukládat přenos souborů klienta (externí toocluster). |
| FileDownload |řetězec, výchozí je "Admin" | Konfigurace zabezpečení pro image store klienta soubor stažení spuštění (externí toocluster). |
| InternalList |řetězec, výchozí je "Admin" | Konfigurace zabezpečení pro bitovou kopii ukládat operace seznamu souborů klienta (interní). |
| Odstranění |řetězec, výchozí je "Admin" | Operace odstranění klienta úložiště konfigurace zabezpečení pro bitovou kopii. |
| Odeslat |řetězec, výchozí je "Admin" | Operace nahrávání klienta úložiště konfigurace zabezpečení pro bitovou kopii. |
| GetStagingLocation |řetězec, výchozí je "Admin" | Klient pracovní umístění načtení úložiště konfigurace zabezpečení pro bitovou kopii. |
| GetStoreLocation |řetězec, výchozí je "Admin" | Načtení umístění úložiště klienta úložiště konfigurace zabezpečení pro bitovou kopii. |
| NodeControl |řetězec, výchozí je "Admin" | Konfigurace zabezpečení pro spuštění; Probíhá zastavování. a restartování uzlů. |
| CodePackageControl |řetězec, výchozí je "Admin" | Konfigurace zabezpečení pro restartování balíčky kódu. |
| UnreliableTransportControl |řetězec, výchozí je "Admin" | Nespolehlivém přenosu pro přidávání a odebírání chování. |
| MoveReplicaControl |řetězec, výchozí je "Admin" | Přesuňte repliky. |
| PredeployPackageToNode |řetězec, výchozí je "Admin" | Před nasazením rozhraní api. |
| StartPartitionDataLoss |řetězec, výchozí je "Admin" | Indukuje ztrátě dat pro oddíl. |
| StartPartitionQuorumLoss |řetězec, výchozí je "Admin" | Indukuje ztrátě kvora pro oddíl. |
| StartPartitionRestart |řetězec, výchozí je "Admin" | Současně restartuje některé nebo všechny repliky hello oddílu. |
| CancelTestCommand |řetězec, výchozí je "Admin" | Konkrétní TestCommand - zruší, pokud se nachází v cestě. |
| StartChaos |řetězec, výchozí je "Admin" | Spustí Chaos – Pokud ještě není spuštěný. |
| StopChaos |řetězec, výchozí je "Admin" | Chaos - zastaví, pokud byla spuštěna. |
| StartNodeTransition |řetězec, výchozí je "Admin" | Konfigurace zabezpečení pro spuštění uzlu přechod. |
| StartClusterConfigurationUpgrade |řetězec, výchozí je "Admin" | Indukuje StartClusterConfigurationUpgrade na oddíl. |
| GetUpgradesPendingApproval |řetězec, výchozí je "Admin" | Indukuje GetUpgradesPendingApproval na oddíl. |
| StartApprovedUpgrades |řetězec, výchozí je "Admin" | Indukuje StartApprovedUpgrades na oddíl. |
| Ping |řetězec, výchozí je "Admin\|\|Uživatel" | Konfigurace zabezpečení pro příkazy ping klienta. |
| Dotaz |řetězec, výchozí je "Admin\|\|Uživatel" | Konfigurace zabezpečení pro dotazy. |
| NameExists |řetězec, výchozí je "Admin\|\|Uživatel" | Konfigurace zabezpečení pro identifikátor URI pojmenování existence kontroly. |
| EnumerateSubnames |řetězec, výchozí je "Admin\|\|Uživatel" | Konfigurace zabezpečení pro identifikátor URI pojmenování výčet. |
| EnumerateProperties |řetězec, výchozí je "Admin\|\|Uživatel" | Konfigurace zabezpečení u názvů běžných vlastnost výčtu. |
| PropertyReadBatch |řetězec, výchozí je "Admin\|\|Uživatel" | Konfigurace zabezpečení pro pojmenování vlastnost operace čtení. |
| GetServiceDescription |řetězec, výchozí je "Admin\|\|Uživatel" | Konfigurace zabezpečení pro dlouhé dotazování služby oznámení a čtení popis služby. |
| ResolveService |řetězec, výchozí je "Admin\|\|Uživatel" | Konfigurace zabezpečení pro řešení služeb na základě předpisy. |
| ResolveNameOwner |řetězec, výchozí je "Admin\|\|Uživatel" | Konfigurace zabezpečení pro řešení identifikátor URI pojmenování vlastníka. |
| ResolvePartition |řetězec, výchozí je "Admin\|\|Uživatel" | Konfigurace zabezpečení pro řešení systémových služeb. |
| ServiceNotifications |řetězec, výchozí je "Admin\|\|Uživatel" | Konfigurace zabezpečení na základě událostí služby oznámení. |
| PrefixResolveService |řetězec, výchozí je "Admin\|\|Uživatel" | Konfigurace zabezpečení pro překlad předpony služeb na základě předpisy. |
| GetUpgradeStatus |řetězec, výchozí je "Admin\|\|Uživatel" | Konfigurace zabezpečení pro dotazování na stav upgradu aplikace. |
| GetFabricUpgradeStatus |řetězec, výchozí je "Admin\|\|Uživatel" | Konfigurace zabezpečení pro dotazování na stav upgradu clusteru. |
| InvokeInfrastructureQuery |řetězec, výchozí je "Admin\|\|Uživatel" | Konfigurace zabezpečení pro úlohy infrastruktury dotazování. |
| Seznam |řetězec, výchozí je "Admin\|\|Uživatel" | Konfigurace zabezpečení pro bitovou kopii ukládat operace seznamu souborů klienta. |
| ResetPartitionLoad |řetězec, výchozí je "Admin\|\|Uživatel" | Konfigurace zabezpečení pro resetování načítání failoverUnit. |
| ToggleVerboseServicePlacementHealthReporting | řetězec, výchozí je "Admin\|\|Uživatel" | Konfigurace zabezpečení pro přepnutím podrobné ServicePlacement HealthReporting. |
| GetPartitionDataLossProgress | řetězec, výchozí je "Admin\|\|Uživatel" | Načte hello průběh pro volání rozhraní api ke ztrátě dat invoke. |
| GetPartitionQuorumLossProgress | řetězec, výchozí je "Admin\|\|Uživatel" | Načte hello průběh pro vyvolat volání api ztrátě kvora. |
| GetPartitionRestartProgress | řetězec, výchozí je "Admin\|\|Uživatel" | Načte hello průběh pro volání rozhraní api oddílu restartování. |
| GetChaosReport | řetězec, výchozí je "Admin\|\|Uživatel" | Načte stav hello Chaos v daném časovém rozsahu. |
| GetNodeTransitionProgress | řetězec, výchozí je "Admin\|\|Uživatel" | Konfigurace zabezpečení pro získání průběhu na přechod příkaz uzlu. |
| GetClusterConfigurationUpgradeStatus | řetězec, výchozí je "Admin\|\|Uživatel" | Indukuje GetClusterConfigurationUpgradeStatus na oddíl. |
| GetClusterConfiguration | řetězec, výchozí je "Admin\|\|Uživatel" | Indukuje GetClusterConfiguration na oddíl. |

### <a name="section-name-reconfigurationagent"></a>Název oddílu: ReconfigurationAgent
| **Parametr** | **Povolené hodnoty** | **Pokyny nebo krátký popis** |
| --- | --- | --- |
| ApplicationUpgradeMaxReplicaCloseDuration | Čas v sekundách, výchozí hodnota je 900 |Zadejte časový interval v sekundách. Doba trvání Hello, pro které hello systému vyčká, než se ukončuje hostiteli služby, kteří replik, které se zasekla v automatickém zavřít. |
| ServiceApiHealthDuration | Čas v sekundách, výchozí hodnota je 30 minut | Zadejte časový interval v sekundách. ServiceApiHealthDuration definuje, jak dlouho jsme před čekat na toorun rozhraní API služby jsme nahlaste to není v pořádku. |
| ServiceReconfigurationApiHealthDuration | Čas v sekundách, výchozí hodnota je 30 | Zadejte časový interval v sekundách. ServiceReconfigurationApiHealthDuration definuje, jak dlouho je hlášen hello před služby v změny konfigurace není v pořádku. |
| PeriodicApiSlowTraceInterval | Čas v sekundách, výchozí hodnota je 5 minut | Zadejte časový interval v sekundách. PeriodicApiSlowTraceInterval definuje hello interval, za které bude pomalého volání rozhraní API překreslována monitorování hello rozhraní API. |
| NodeDeactivationMaxReplicaCloseDuration | Čas v sekundách, výchozí hodnota je 900 | Zadejte časový interval v sekundách. Maximální doba toowait Hello před ukončením hostitele služby, která blokuje deaktivace uzlu. |
| FabricUpgradeMaxReplicaCloseDuration | Čas v sekundách, výchozí hodnota je 900 | Zadejte časový interval v sekundách. Maximální délka trvání RA Hello bude čekat, než hostitel služby repliky, který není zavřením se ukončuje. |
| IsDeactivationInfoEnabled | BOOL, výchozí hodnota je true | Určuje, zda bude RA použít informace o deaktivaci pro provádění primární zvolen znovu pro nové clustery, tato konfigurace by mělo být nastavené tootrue pro stávajících clusterů, které se upgraduje na postupy najdete v tématu poznámky k verzi hello tooenable to. |

### <a name="section-name-placementandloadbalancing"></a>Název oddílu: PlacementAndLoadBalancing
| **Parametr** | **Povolené hodnoty** | **Pokyny nebo krátký popis** |
| --- | --- | --- |
| TraceCRMReasons |BOOL, výchozí hodnota je true |Určuje, zda tootrace důvody CRM vydané pohybů toohello provozní události kanálu. |
| ValidatePlacementConstraint | BOOL, výchozí hodnota je true | Určuje, zda hello PlacementConstraint výraz pro službu ověření při aktualizaci služby ServiceDescription. |
| PlacementConstraintValidationCacheSize | Int, výchozí hodnota je 10000. | Omezení hello velikost tabulky hello používá pro rychlé ověření a ukládání do mezipaměti výrazů omezení umístění. |
|VerboseHealthReportLimit | Int, výchozí hodnota je 20 | Definuje hello počet repliku má toogo Neumístěná před upozornění stavu údajně pro něj (Pokud je povoleno oznamování stavu verbose). |
|ConstraintViolationHealthReportLimit | Int, výchozí hodnota je 50 | Definuje hello počet, kolikrát má toobe trvale nevyřešené předtím, než jsou prováděny diagnostics a sestav stavu jsou vygenerované omezení bychom při tom porušili repliky. |
|DetailedConstraintViolationHealthReportLimit | Int, výchozí hodnota je 200 | Definuje hello počet, kolikrát má toobe trvale nevyřešené předtím, než jsou prováděny diagnostiky a podrobný stav sestavy jsou vygenerované omezení bychom při tom porušili repliky. |
|DetailedVerboseHealthReportLimit | Int, výchozí hodnota je 200 | Definuje hello počet, kolikrát má repliku Neumístěná toobe trvale Neumístěná před podrobné stavu, které jsou vygenerované sestavy. |
|ConsecutiveDroppedMovementsHealthReportLimit | Int, výchozí hodnota je 20 | Definuje hello počet po sobě jdoucích pokusů vydané ResourceBalancer pohybů jsou vyřazen dříve, než jsou prováděny diagnostiky a jsou vygenerované stavu upozornění. Záporné: Žádná upozornění vygenerované v rámci této podmínky. |
|DetailedNodeListLimit | Int, výchozí hodnota je 15 | Definuje hello počet uzlů na omezení tooinclude před zkrácení hello Neumístěná repliky sestavy. |
|DetailedPartitionListLimit | Int, výchozí hodnota je 15 | Definuje hello počet oddílů na diagnostiky záznam pro omezení tooinclude před zkrácení diagnostiky. |
|DetailedDiagnosticsInfoListLimit | Int, výchozí hodnota je 15 | Definuje hello počtem diagnostických záznamů (s podrobnými informacemi) na omezení tooinclude před zkrácení v diagnostice.|
|PLBRefreshGap | Čas v sekundách, výchozí hodnota je 1 | Zadejte časový interval v sekundách. Definuje hello minimální množství času, které musí uplynout, než PLB aktualizuje stav znovu. |
|MinPlacementInterval | Čas v sekundách, výchozí hodnota je 1 | Zadejte časový interval v sekundách. Definuje minimální množství času, který musí projít před dvě po sobě jdoucích umístění zaokrouhlí hello. |
|MinConstraintCheckInterval | Čas v sekundách, výchozí hodnota je 1 | Zadejte časový interval v sekundách. Definuje minimální množství času, které musí uplynout, než dvě po sobě jdoucích omezení zkontrolujte zaokrouhlí hello. |
|MinLoadBalancingInterval | Čas v sekundách, výchozí hodnota je 5 | Zadejte časový interval v sekundách. Definuje minimální množství času, který musí projít před dvě po sobě jdoucích vyrovnávání zaokrouhlí hello. |
|BalancingDelayAfterNodeDown | Čas v sekundách, výchozí hodnota je 120. |Zadejte časový interval v sekundách. Nespouštět vyrovnávání aktivit během tohoto období po uzlu událost vypnutí. |
|BalancingDelayAfterNewNode | Čas v sekundách, výchozí hodnota je 120. |Zadejte časový interval v sekundách. Nespouštět vyrovnávání aktivit během tohoto období, po přidání nového uzlu. |
|ConstraintFixPartialDelayAfterNodeDown | Čas v sekundách, výchozí hodnota je 120. | Zadejte časový interval v sekundách. To není narušení omezení opravte FaultDomain a UpgradeDomain během tohoto období po uzlu událost vypnutí. |
|ConstraintFixPartialDelayAfterNewNode | Čas v sekundách, výchozí hodnota je 120. | Zadejte časový interval v sekundách. DDo FaultDomain opravit a narušení omezení UpgradeDomain během tohoto období, po přidání nového uzlu. |
|GlobalMovementThrottleThreshold | Uint, výchozí hodnota je 1 000 | Maximální počet pohybů v povolené hello vyrovnávání fázi v posledních hello interval indikován GlobalMovementThrottleCountingInterval. |
|GlobalMovementThrottleThresholdForPlacement | Uint, výchozí hodnota je 0 | Maximální počet pohybů ve fázi umístění v povoleny hello posledních interval indikován GlobalMovementThrottleCountingInterval.0 znamená bez omezení.|
|GlobalMovementThrottleThresholdForBalancing | Uint, výchozí hodnota je 0 | Maximální počet v vyrovnávání fáze povolena posledních hello pohybů interval indikován GlobalMovementThrottleCountingInterval. 0 znamená bez omezení. |
|GlobalMovementThrottleCountingInterval | Čas v sekundách, výchozí hodnota je 600 | Zadejte časový interval v sekundách. Označuje hello délka hello za interval, pro které tootrack za pohybů repliky domény (používá se společně s GlobalMovementThrottleThreshold). Lze nastavit too0 tooignore globální omezení šířky pásma úplně. |
|MovementPerPartitionThrottleThreshold | Uint, výchozí hodnota je 50 | Žádné vyrovnávání související přesun dojde pro oddíl, pokud počet hello vyrovnávání související pohybů pro repliky tohoto oddílu má dosáhli nebo Přesáhli jste MovementPerFailoverUnitThrottleThreshold v posledních hello indikován intervalu MovementPerPartitionThrottleCountingInterval. |
|MovementPerPartitionThrottleCountingInterval | Čas v sekundách, výchozí hodnota je 600 | Zadejte časový interval v sekundách. Označuje hello délka hello za interval, pro které přesuny tootrack repliky pro každý oddíl (používá se společně s MovementPerPartitionThrottleThreshold). |
|PlacementSearchTimeout | Čas v sekundách, výchozí hodnota je 0,5 | Zadejte časový interval v sekundách. Při vkládání služeb; Vyhledejte maximálně tento dlouho před vrácením výsledku. |
|UseMoveCostReports | BOOL, výchozí hodnota je false | Dá pokyn hello LB tooignore hello náklady na element hello vyhodnocování funkce; Výsledná potenciálně velkého počtu přesun lépe vyrovnáváním umístění. |
|PreventTransientOvercommit | BOOL, výchozí hodnota je false | Určuje, by měl PLB okamžitě počet na prostředcích, které uvolní přesune hello inicioval. Ve výchozím nastavení; PLB můžete zahájit přesunutí out a přesunout v na stejném uzlu, který můžete vytvořit přechodný přetížit hello. Nastavení tootrue tento parametr zabrání ty druh overcommits a na vyžádání defragmentace (neboli placementWithMove) bude zakázán. |
|InBuildThrottlingEnabled | BOOL, výchozí hodnota je false | Určí, zda je povoleno omezení šířky pásma v sestavení hello. |
|InBuildThrottlingAssociatedMetric | Výchozí hodnota je řetězec, "" | Hello související název metriky pro toto omezení. |
|InBuildThrottlingGlobalMaxValue | Int, výchozí hodnota je 0 |maximální počet Hello povolené globálně replik ve sestavení. |
|SwapPrimaryThrottlingEnabled | BOOL, výchozí hodnota je false| Určí, zda je povoleno omezení swap primární hello. |
|SwapPrimaryThrottlingAssociatedMetric | Výchozí hodnota je řetězec, ""| Hello související název metriky pro toto omezení. |
|SwapPrimaryThrottlingGlobalMaxValue | Int, výchozí hodnota je 0 | Hello maximální počet replik primárních prohození povoleny globální. |
|PlacementConstraintPriority | Int, výchozí hodnota je 0 | Určuje prioritu hello omezení umístění: 0: pevné; 1: logicky; záporné: ignorovat. |
|PreferredLocationConstraintPriority | Int, výchozí hodnota je 2| Určuje prioritu hello omezení upřednostňované umístění: 0: pevné; 1: logicky; 2: optimalizace; záporné: Ignorovat |
|CapacityConstraintPriority | Int, výchozí hodnota je 0 | Určuje prioritu hello omezení kapacity: 0: pevné; 1: logicky; záporné: ignorovat. |
|AffinityConstraintPriority | Int, výchozí hodnota je 0 | Určuje prioritu hello omezení spřažení: 0: pevné; 1: logicky; záporné: ignorovat. |
|FaultDomainConstraintPriority | Int, výchozí hodnota je 0 | Určuje prioritu hello omezení domény chyby: 0: pevné; 1: logicky; záporné: ignorovat. |
|UpgradeDomainConstraintPriority | Int, výchozí hodnota je 1| Určuje prioritu hello omezení domény upgradu: 0: pevné; 1: logicky; záporné: ignorovat. |
|ScaleoutCountConstraintPriority | Int, výchozí hodnota je 0 | Určuje prioritu hello omezení počtu škálování: 0: pevné; 1: logicky; záporné: ignorovat. |
|ApplicationCapacityConstraintPriority | Int, výchozí hodnota je 0 | Určuje prioritu hello omezení kapacity: 0: pevné; 1: logicky; záporné: ignorovat. |
|MoveParentToFixAffinityViolation | BOOL, výchozí hodnota je false | Nastavení, která určuje, zda může být nadřazené repliky přesunout toofix spřažení omezení.|
|MoveExistingReplicaForPlacement | BOOL, výchozí hodnota je true |Nastavení, která určuje, zda toomove existující repliku během umísťování. |
|UseSeparateSecondaryLoad | BOOL, výchozí hodnota je true | Nastavení, která určuje, zda použít jiný sekundární zatížení. |
|PlaceChildWithoutParent | BOOL, výchozí hodnota je true | Nastavení, která určuje, zda podřízené služby repliky mohou být umístěny, pokud žádnou repliku nadřazené je zapnutý. |
|PartiallyPlaceServices | BOOL, výchozí hodnota je true | Určuje, pokud všechny služby repliky v clusteru se uskuteční "všechny nebo nic" dané omezené vhodný uzly pro ně.|
|InterruptBalancingForAllFailoverUnitUpdates | BOOL, výchozí hodnota je false | Určuje, zda by měl jakýkoli typ aktualizace jednotky převzetí služeb při selhání přerušení rychlé nebo pomalé vyrovnávání spustit. Pomocí zadané "false" vyrovnávání spustit bude přerušena v případě FailoverUnit: je vytvořit/odstranit; Chybí repliky; změnit umístění primární repliku nebo změněné počet replik. Vyrovnávání spustit není přerušena v ostatních případech – Pokud FailoverUnit: má navíc repliky; změnit všechny repliky příznak; změnit jenom oddílu verze nebo jiném případu. |

### <a name="section-name-security"></a>Název oddílu: zabezpečení
| **Parametr** | **Povolené hodnoty** | **Pokyny nebo krátký popis** |
| --- | --- | --- |
| ClusterProtectionLevel |Žádná nebo EncryptAndSign |Žádný (výchozí) pro zabezpečená clustery, EncryptAndSign pro zabezpečené clustery. |

### <a name="section-name-hosting"></a>Název oddílu: hostování
| **Parametr** | **Povolené hodnoty** | **Pokyny nebo krátký popis** |
| --- | --- | --- |
| ServiceTypeRegistrationTimeout |Čas v sekundách, výchozí hodnota je 300 |Maximální dobu povolenou pro toobe ServiceType hello zaregistrována prostředků infrastruktury |
| ServiceTypeDisableFailureThreshold |Celé číslo, výchozí je 1 |Toto je hello prahová hodnota pro hello počet selhání po FailoverManager (FM) oznámení typ služby hello toodisable na tomto uzlu a zkuste to na jiný uzel pro umístění. |
| ActivationRetryBackoffInterval |Čas v sekundách, výchozí hodnota je 5 |Interval omezení rychlosti na každé selhání aktivace; Na každé selhání průběžné aktivace hello hello systému opakování aktivace pro až toohello MaxActivationFailureCount. interval opakování Hello na každý zkuste je produkt průběžné selhání a hello aktivace zpět mimo určený v intervalu aktivace. |
| ActivationMaxRetryInterval |Čas v sekundách, výchozí hodnota je 300 |Na každé selhání průběžné aktivace hello hello systému opakování aktivace pro až tooActivationMaxFailureCount. ActivationMaxRetryInterval Určuje časový interval čekání před opakováním po každé selhání aktivace |
| ActivationMaxFailureCount |Celé číslo, výchozí je 10 |Počet opakování systému se nezdařila aktivace, než se předá |

### <a name="section-name-failovermanager"></a>Název oddílu: FailoverManager
| **Parametr** | **Povolené hodnoty** | **Pokyny nebo krátký popis** |
| --- | --- | --- |
| PeriodicLoadPersistInterval |Čas v sekundách, výchozí hodnota je 10 |Určuje, jak často hello FM zkontrolujte nové sestavy zatížení |

### <a name="section-name-federation"></a>Název oddílu: Federation
| **Parametr** | **Povolené hodnoty** | **Pokyny nebo krátký popis** |
| --- | --- | --- |
| LeaseDuration |Čas v sekundách, výchozí hodnota je 30 |Doba trvání zapůjčení trvající mezi uzlu a své okolí. |
| LeaseDurationAcrossFaultDomain |Čas v sekundách, výchozí hodnota je 30 |Doba trvání zapůjčení trvající mezi uzlu a sousední napříč doménami selhání. |

### <a name="section-name-clustermanager"></a>Název oddílu: ClusterManager
| **Parametr** | **Povolené hodnoty** | **Pokyny nebo krátký popis** |
| --- | --- | --- |
| UpgradeStatusPollInterval |Čas v sekundách, výchozí hodnota je 60 |Hello frekvence dotazování na stav upgradu aplikace. Tato hodnota určuje rychlost hello aktualizace pro všechny GetApplicationUpgradeProgress volání |
| UpgradeHealthCheckInterval |Čas v sekundách, výchozí hodnota je 60 |frekvence Hello stavu kontroluje při upgradech monitorovanou aplikaci. |
| FabricUpgradeStatusPollInterval |Čas v sekundách, výchozí hodnota je 60 |Hello frekvence dotazování na stav upgradu prostředků infrastruktury. Tato hodnota určuje rychlost hello aktualizace pro všechny GetFabricUpgradeProgress volání |
| FabricUpgradeHealthCheckInterval |Čas v sekundách, výchozí hodnota je 60 |frekvence Hello kontrolu stavu během monitorovaných upgrade pro Fabric |
|InfrastructureTaskProcessingInterval | Čas v sekundách, výchozí hodnota je 10 |Zadejte časový interval v sekundách. interval zpracování Hello používá hello infrastruktury úloh zpracování stavu počítače. |
|TargetReplicaSetSize |Int, výchozí hodnota je 7 |Hello TargetReplicaSetSize pro ClusterManager. |
|MinReplicaSetSize |Int, výchozí hodnota je 3 |Hello MinReplicaSetSize pro ClusterManager. |
|ReplicaRestartWaitDuration |Čas v sekundách, výchozí hodnota je (60.0 * 30)|Zadejte časový interval v sekundách. Hello ReplicaRestartWaitDuration pro ClusterManager. |
|QuorumLossWaitDuration |Čas v sekundách, výchozí hodnota je MaxValue | Zadejte časový interval v sekundách. Hello QuorumLossWaitDuration pro ClusterManager. |
|StandByReplicaKeepDuration | Čas v sekundách, výchozí hodnota je (3600.0 * 2)|Zadejte časový interval v sekundách. Hello StandByReplicaKeepDuration pro ClusterManager. |
|PlacementConstraints | Výchozí hodnota je řetězec, "" |Hello PlacementConstraints pro ClusterManager. |
|SkipRollbackUpdateDefaultService | BOOL, výchozí hodnota je false |Hello CM přeskočí navrácení aktualizované výchozí služby během upgradu odvolání aplikace. |
|EnableDefaultServicesUpgrade | BOOL, výchozí hodnota je false |Povolte upgrade výchozí služby během upgradu aplikace. Po upgradu budou přepsána popisy výchozí služby. |
|InfrastructureTaskHealthCheckWaitDuration |Čas v sekundách, výchozí hodnota je 0| Zadejte časový interval v sekundách. Hello množství času toowait před zahájením kontroly stavu po po zpracování úloh infrastrukturu. |
|InfrastructureTaskHealthCheckStableDuration | Čas v sekundách, výchozí hodnota je 0| Zadejte časový interval v sekundách. Hello množství času tooobserve po sobě jdoucích předaný stavu kontroluje před po zpracování úlohy infrastruktury příště úspěšně dokončena. Sledování selhání kontrolu obnoví tento časovač. |
|InfrastructureTaskHealthCheckRetryTimeout | Čas v sekundách, výchozí hodnota je 60 |Zadejte časový interval v sekundách. Hello množství času toospend Probíhá opakování kontroly stavu se nezdařila při po zpracování úloh infrastrukturu. Sledování kontrolu předaný stavu obnoví tento časovač. |
|ImageBuilderTimeoutBuffer |Čas v sekundách, výchozí hodnota je 3 |Zadejte časový interval v sekundách. Hello množství času tooallow pro Image Builder konkrétní časový limit chyby tooreturn toohello klienta. Pokud tato vyrovnávací paměť je příliš malý. pak klienta hello vyprší časový limit než hello server a získá chybu obecný časový limit. |
|MinOperationTimeout | Čas v sekundách, výchozí hodnota je 60 |Zadejte časový interval v sekundách. Hello minimální globálního časového limitu pro operace na ClusterManager interně zpracování. |
|Jako MaxOperationTimeout |Čas v sekundách, výchozí hodnota je MaxValue | Zadejte časový interval v sekundách. Hello maximální globálního časového limitu pro operace na ClusterManager interně zpracování. |
|MaxTimeoutRetryBuffer | Čas v sekundách, výchozí hodnota je 600 |Zadejte časový interval v sekundách. časový limit operace maximální Hello při opakování interně kvůli tootimeouts je <Original Timeout>  +  <MaxTimeoutRetryBuffer>. V přírůstcích po MinOperationTimeout se přidá další časový limit. |
|MaxCommunicationTimeout |Čas v sekundách, výchozí hodnota je 600 |Zadejte časový interval v sekundách. Hello maximální časový limit pro interní komunikaci mezi ClusterManager a jiných služeb system (tj.); Naming Service; Správce převzetí služeb při selhání a atd). Tento časový limit musí být menší než globální jako MaxOperationTimeout (jak může existovat více komunikace mezi součástmi systému pro každou operaci klienta). |
|MaxDataMigrationTimeout |Čas v sekundách, výchozí hodnota je 600 |Zadejte časový interval v sekundách. Hello maximální časový limit pro operace obnovení migrace dat po neproběhla upgrade pro Fabric. |
|MaxOperationRetryDelay |Čas v sekundách, výchozí hodnota je 5| Zadejte časový interval v sekundách. Hello Maximální zpoždění pro interní opakování, když došlo k selhání. |
|ReplicaSetCheckTimeoutRollbackOverride |Čas v sekundách, výchozí hodnota je 1200 | Zadejte časový interval v sekundách. Pokud je nastavená ReplicaSetCheckTimeout toohello maximální hodnotu DWORD; pak je přepsat s hodnotou hello této konfigurace pro účely hello vrácení zpět. Hodnota Hello používaná pro dopředné obnovení nikdy přepsána. |
|ImageBuilderJobQueueThrottle |Int, výchozí hodnota je 10 |Počet omezení pro Image Builder fronty úloh proxy serveru na žádosti o aplikace přístup z více vláken. |

## <a name="next-steps"></a>Další kroky
Přečtěte si tyto články pro další informace o správě clusteru:

[Přidat, mění, odebrat certifikáty ze Azure clusteru](service-fabric-cluster-security-update-certs-azure.md) 

