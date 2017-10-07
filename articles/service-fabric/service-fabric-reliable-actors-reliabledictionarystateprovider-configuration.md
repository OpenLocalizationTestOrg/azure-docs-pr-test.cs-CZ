---
title: "nastavení ReliableDictionaryActorStateProvider aaaChange v Azure mikroslužeb | Microsoft Docs"
description: "Další informace o konfiguraci Azure Service Fabric stavová aktéři typu ReliableDictionaryActorStateProvider."
services: Service-Fabric
documentationcenter: .net
author: sumukhs
manager: timlt
editor: 
ms.assetid: 79b48ffa-2474-4f1c-a857-3471f9590ded
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 6/29/2017
ms.author: sumukhs
ms.openlocfilehash: 44c85a41c90a17669ba874401d7921c94e7be9ec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-reliable-actors--reliabledictionaryactorstateprovider"></a>Konfigurace Reliable Actors – ReliableDictionaryActorStateProvider
Výchozí konfigurace hello ReliableDictionaryActorStateProvider můžete upravit změnou souboru souborech settings.xml hello vygenerovaných kořenového adresáře balíčku sady Visual Studio hello složce hello konfigurace pro zadaný objektu actor hello.

modulu runtime Azure Service Fabric Hello předdefinované části z názvů v souboru hello souborech settings.xml a odebírá hello konfigurační hodnoty při vytváření hello základní komponenty modulu runtime.

> [!NOTE]
> Proveďte **není** odstranit nebo změnit názvy části hello hello následující konfigurace v souborech settings.xml souboru hello, který je generován ve hello řešení sady Visual Studio.
> 
> 

Existují také globální nastavení, které ovlivňují konfiguraci hello ReliableDictionaryActorStateProvider.

## <a name="global-configuration"></a>Globální konfigurace
globální konfiguraci Hello je zadáno v manifestu clusteru hello hello clusteru pod hello KtlLogger části. Umožňuje konfiguraci hello sdílené protokolu umístění a velikost a hello globální paměť omezení používané hello protokolovacího nástroje. Všimněte si, že změny v manifestu clusteru hello vliv na všechny služby, které používají ReliableDictionaryActorStateProvider a spolehlivé stavové služby.

manifest clusteru Hello je jednoho souboru XML, který obsahuje nastavení a konfigurace, které se vztahují tooall uzly a služby v clusteru hello. soubor Hello se obvykle nazývá ClusterManifest.xml. Můžete zobrazit hello manifestu clusteru pro váš cluster pomocí příkazu prostředí powershell Get-ServiceFabricClusterManifest hello.

### <a name="configuration-names"></a>Názvy konfigurace
| Name (Název) | Jednotka | Výchozí hodnota | Poznámky |
| --- | --- | --- | --- |
| WriteBufferMemoryPoolMinimumInKB |Kilobajtů |8388608 |Minimální počet tooallocate KB v režimu jádra pro hello protokolovač zápis fondu vyrovnávací paměti. Tento fond paměti se používá pro ukládání do mezipaměti informace o stavu před zápisem toodisk. |
| WriteBufferMemoryPoolMaximumInKB |Kilobajtů |Bez omezení |Maximální velikost toowhich hello protokolovacího nástroje zápis vyrovnávací paměti fondu paměti můžou růst. |
| SharedLogId |IDENTIFIKÁTOR GUID |"" |Určuje jedinečný toouse identifikátor GUID pro identifikaci hello výchozí sdíleného souboru protokolu v všechny spolehlivé služeb ve všech uzlech v clusteru hello, které neurčují hello SharedLogId v jejich konkrétní konfiguraci služby. Pokud je zadán SharedLogId, musíte zadat také tento SharedLogPath. |
| SharedLogPath |Plně kvalifikovaný název cesty |"" |Určuje plně kvalifikovanou cestu hello kde hello sdíleného souboru protokolu v případě všech spolehlivé služeb ve všech uzlech v clusteru hello, které neurčují hello SharedLogPath v jejich konkrétní konfiguraci služby. Ale pokud je zadán SharedLogPath, pak SharedLogId musí také uvést. |
| SharedLogSizeInMB |V megabajtech |8192 |Určuje počet hello MB místa na disku přidělit toostatically pro sdílené protokolu hello. Hello hodnota musí být 2 048 nebo větší. |

### <a name="sample-cluster-manifest-section"></a>Ukázka oddílu manifestu clusteru
```xml
   <Section Name="KtlLogger">
     <Parameter Name="WriteBufferMemoryPoolMinimumInKB" Value="8192" />
     <Parameter Name="WriteBufferMemoryPoolMaximumInKB" Value="8192" />
     <Parameter Name="SharedLogId" Value="{7668BB54-FE9C-48ed-81AC-FF89E60ED2EF}"/>
     <Parameter Name="SharedLogPath" Value="f:\SharedLog.Log"/>
     <Parameter Name="SharedLogSizeInMB" Value="16383"/>
   </Section>
```

### <a name="remarks"></a>Poznámky
protokolovač Hello se globální fond paměti přidělené z paměti nestránkovaného jádra, která je k dispozici tooall spolehlivé služby v uzlu pro ukládání do mezipaměti data o stavu před zápisem toohello vyhrazené protokolu přidružené hello spolehlivá služba repliky. velikost fondu Hello se řídí hello WriteBufferMemoryPoolMinimumInKB a WriteBufferMemoryPoolMaximumInKB nastavení. WriteBufferMemoryPoolMinimumInKB určuje obě hello počáteční velikost tohoto fondu paměti a může zmenšit hello nejnižší velikost toowhich hello fondem paměti. WriteBufferMemoryPoolMaximumInKB je, že může růst hello nejvyšší velikost toowhich hello fondem paměti. Každou repliku spolehlivá služba, který se otevírá může zvýšit hello velikost fondu paměti hello systému určit velikost až tooWriteBufferMemoryPoolMaximumInKB. Pokud existuje více požadavků na paměť z hello fondu paměti, než je k dispozici, požadavky na paměť se odloží až do paměti je k dispozici. Proto pokud fondu hello zápis vyrovnávací paměti je příliš malá pro konkrétní konfigurací pak výkonu může dojít ke snížení.

nastavení SharedLogId a SharedLogPath Hello jsou vždy použít společně toodefine hello GUID a protokolu pro všechny uzly v clusteru hello sdíleného umístění pro výchozí hello. sdílené protokol Hello výchozí se používá pro všechny spolehlivé služby, které neurčují hello nastavení v souborech settings.xml hello pro konkrétní službu hello. Pro nejlepší výkon musí být sdílené soubory protokolu umístěny na disky, které se používají výhradně pro kolizí tooreduce souboru protokolu hello sdílené.

SharedLogSizeInMB určuje hello množství toopreallocate místa na disku pro protokol sdílené výchozí hello na všech uzlech.  SharedLogId a SharedLogPath nemusí toobe zadaný v pořadí pro SharedLogSizeInMB toobe zadán.

## <a name="replicator-security-configuration"></a>Konfigurace zabezpečení replikátoru
Konfigurace zabezpečení Replikátor jsou použité toosecure hello komunikační kanál, který se používá během replikace. To znamená, že služby neuvidí vzájemně provoz replikace, zajistit vysokou dostupnost dat hello je bezpečnější.
Ve výchozím nastavení zabraňuje konfigurační oddíl prázdný zabezpečení zabezpečení replikace.

### <a name="section-name"></a>Název oddílu
&lt;ActorName&gt;ServiceReplicatorSecurityConfig

## <a name="replicator-configuration"></a>Replikátor konfigurace
Replikátor konfigurace jsou použité tooconfigure hello Replikátor, která je odpovědná za vytváření vysoce spolehlivé hello zprostředkovatele stavu objektu Actor stavu replikace a zachování stavu hello místně.
Hello výchozí konfigurace je generován hello šablony sady Visual Studio a měla by stačit. Tato část pojednává o další konfigurace, které jsou k dispozici tootune Replikátor hello.

### <a name="section-name"></a>Název oddílu
&lt;ActorName&gt;ServiceReplicatorConfig

### <a name="configuration-names"></a>Názvy konfigurace
| Name (Název) | Jednotka | Výchozí hodnota | Poznámky |
| --- | --- | --- | --- |
| BatchAcknowledgementInterval |Sekundy |0.015 |Časové období, pro které Replikátor hello na sekundární počká hello po přijetí operace před odesláním zpět potvrzení toohello primární. Další potvrzení toobe odeslané pro operace zpracování v rámci tohoto intervalu se odesílají jako jedna odpověď. |
| ReplicatorEndpoint |Není k dispozici |Žádná výchozí hodnota – povinný parametr |IP adresa a port, který hello primární a sekundární Replikátor použije toocommunicate jiných replikátory hello sady replik. To by měl odkazovat koncový bod TCP prostředků v hello service manifest. Odkazovat příliš[manifestu prostředky služby](service-fabric-service-manifest-resources.md) tooread informace o definování koncový bod prostředků v service manifest. |
| Maxreplicationmessagesize. |Bajty |50 MB |Maximální velikost data replikace, která mohou být přenesena do jedné zprávy. |
| MaxPrimaryReplicationQueueSize |Počet operací |8192 |Maximální počet operací ve frontě primární hello. Operace uvolněno po primární Replikátor hello přijímá potvrzení z všechny sekundární replikátory hello. Tato hodnota musí být větší než 64 a druhou mocninou 2. |
| MaxSecondaryReplicationQueueSize |Počet operací |16384 |Maximální počet operací ve frontě sekundární hello. Operace uvolněno po provedení vysoce dostupný prostřednictvím trvalost stavu. Tato hodnota musí být větší než 64 a druhou mocninou 2. |
| CheckpointThresholdInMB |MB |200 |Množství místa souboru protokolu, po jejímž uplynutí je kontrolován hello stavu. |
| MaxRecordSizeInKB |kB |1024 |Maximální velikost záznamů, které Replikátor hello můžou zapisovat do protokolu hello. Tato hodnota musí být násobkem 4 a je větší než 16. |
| OptimizeLogForLowerDiskUsage |Logická hodnota |Hodnota TRUE |V případě hodnoty true hello protokolu je nakonfigurován tak, aby hello repliky vyhrazené soubor protokolu se vytvoří pomocí systému souborů NTFS zhuštěného souboru. To snižuje hello skutečné místo na disku pro soubor hello. Pokud má hodnotu NEPRAVDA, je vytvořen hello soubor s pevnou přidělení, které poskytují nejlepší výkon zápisu hello. |
| SharedLogId |Identifikátor GUID |"" |Určuje toouse jedinečný identifikátor guid pro identifikaci sdílený soubor protokolu hello použít s této repliky. Služby obvykle neměli používat toto nastavení. Ale pokud je zadán SharedLogId, pak SharedLogPath musí také uvést. |
| SharedLogPath |Plně kvalifikovaný název cesty |"" |Určuje hello plně kvalifikovanou cestu, kde bude vytvořen hello sdílený soubor protokolu pro tuto repliku. Služby obvykle neměli používat toto nastavení. Ale pokud je zadán SharedLogPath, pak SharedLogId musí také uvést. |

## <a name="sample-configuration-file"></a>Vzorový konfigurační soubor
```xml
<?xml version="1.0" encoding="utf-8"?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Section Name="MyActorServiceReplicatorConfig">
      <Parameter Name="ReplicatorEndpoint" Value="MyActorServiceReplicatorEndpoint" />
      <Parameter Name="BatchAcknowledgementInterval" Value="0.05"/>
      <Parameter Name="CheckpointThresholdInMB" Value="180" />
   </Section>
   <Section Name="MyActorServiceReplicatorSecurityConfig">
      <Parameter Name="CredentialType" Value="X509" />
      <Parameter Name="FindType" Value="FindByThumbprint" />
      <Parameter Name="FindValue" Value="9d c9 06 b1 69 dc 4f af fd 16 97 ac 78 1e 80 67 90 74 9d 2f" />
      <Parameter Name="StoreLocation" Value="LocalMachine" />
      <Parameter Name="StoreName" Value="My" />
      <Parameter Name="ProtectionLevel" Value="EncryptAndSign" />
      <Parameter Name="AllowedCommonNames" Value="My-Test-SAN1-Alice,My-Test-SAN1-Bob" />
   </Section>
</Settings>
```

## <a name="remarks"></a>Poznámky
Parametr BatchAcknowledgementInterval Hello řídí latenci replikace. Hodnota '0' výsledkem hello nejnižší možnou latenci, náklady na hello propustnosti (jako další potvrzení zprávy musí být odeslána a zpracování jednotlivých obsahující méně potvrzení).
Hello větší hodnotu hello BatchAcknowledgementInterval, hello vyšší hello celková propustnost replikace hello náklady na vyšší latence operace. Výsledkem je přímo toohello latenci potvrzení transakcí.

Hello CheckpointThresholdInMB parametr ovládací prvky hello množství místa na disku, který hello Replikátor můžete použít informace o stavu toostore v souboru protokolu vyhrazené hello repliky. Zvýšení tuto tooa vyšší hodnotu než výchozí hello může mít za následek Rekonfigurace rychlejší, když novou repliku je přidána toohello sady. Toto je z důvodu přenosu toohello částečné stavu, který probíhá kvůli toohello dostupnost další historie operací v protokolu hello. Potenciálně může prodloužit dobu obnovení hello repliky po chybě.

Pokud nastavíte OptimizeForLowerDiskUsage tootrue, bude místo souboru protokolu neoprávněně zřízené tak, aby aktivní repliky můžete ukládat další informace o stavu v souborech protokolu i neaktivních replik se bude používat méně místa na disku. Díky tomu je možné toohost další repliky na uzlu. Pokud jste nastavili OptimizeForLowerDiskUsage toofalse, informace o stavu hello se zapíše soubory protokolu toohello rychleji.

nastavení MaxRecordSizeInKB Hello definuje maximální velikost hello záznam, který je možné zapsat hello Replikátor do souboru protokolu hello. Ve většině případů hello výchozí záznam 1024 KB velikost je optimální. Pokud však hello služba způsobuje položky dat větší toobe část informace o stavu hello, pak tato hodnota může být nutné toobe vyšší. Výhoda je malá při vytvoření MaxRecordSizeInKB menší než 1024, jako menší záznamů použijte pouze hello prostor potřebný pro menší záznam hello. Očekáváme, že tato hodnota by nutné toobe změnit pouze ve výjimečných případech.

nastavení SharedLogId a SharedLogPath Hello jsou vždy použít společně toomake služby použití samostatných sdílených protokolu z hello výchozí sdílené protokolu pro uzel hello. Pro nejlepšího výkonu by měl určovat tolik služby jako možné hello stejné sdílené protokolu. Sdílené soubory protokolu musí být umístěny na disky, které se používají výhradně pro hello sdíleného souboru protokolu, tooreduce head přesun kolizí. Očekáváme, že tyto hodnoty by nutné toobe změnit pouze ve výjimečných případech.

