---
title: "spolehlivé Azure mikroslužeb aaaConfigure | Microsoft Docs"
description: "Další informace o konfiguraci stavové spolehlivé služby v Azure Service Fabric."
services: Service-Fabric
documentationcenter: .net
author: sumukhs
manager: timlt
editor: vturecek
ms.assetid: 9f72373d-31dd-41e3-8504-6e0320a11f0e
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 6/29/2017
ms.author: sumukhs
ms.openlocfilehash: 1e9c2890b62890a777561f25c04dc0fd11db9f1d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-stateful-reliable-services"></a>Konfigurovat stavová spolehlivé služby
Existují dvě sady nastavení konfigurace pro spolehlivé služby. Jedna sada je globální pro všechny spolehlivé služby v clusteru hello při hello druhá sada konkrétní tooa konkrétní spolehlivou službu.

## <a name="global-configuration"></a>Globální konfigurace
Hello globální spolehlivá služba konfigurace je zadáno v manifestu clusteru hello hello clusteru pod hello KtlLogger části. Umožňuje konfiguraci hello sdílené protokolu umístění a velikost a hello globální paměť omezení používané hello protokolovacího nástroje. manifest clusteru Hello je jednoho souboru XML, který obsahuje nastavení a konfigurace, které se vztahují tooall uzly a služby v clusteru hello. soubor Hello se obvykle nazývá ClusterManifest.xml. Můžete zobrazit hello manifestu clusteru pro váš cluster pomocí příkazu prostředí powershell Get-ServiceFabricClusterManifest hello.

### <a name="configuration-names"></a>Názvy konfigurace
| Name (Název) | Jednotka | Výchozí hodnota | Poznámky |
| --- | --- | --- | --- |
| WriteBufferMemoryPoolMinimumInKB |Kilobajtů |8388608 |Minimální počet tooallocate KB v režimu jádra pro hello protokolovač zápis fondu vyrovnávací paměti. Tento fond paměti se používá pro ukládání do mezipaměti informace o stavu před zápisem toodisk. |
| WriteBufferMemoryPoolMaximumInKB |Kilobajtů |Bez omezení |Maximální velikost toowhich hello protokolovacího nástroje zápis vyrovnávací paměti fondu paměti můžou růst. |
| SharedLogId |IDENTIFIKÁTOR GUID |"" |Určuje jedinečný toouse identifikátor GUID pro identifikaci hello výchozí sdíleného souboru protokolu v všechny spolehlivé služeb ve všech uzlech v clusteru hello, které neurčují hello SharedLogId v jejich konkrétní konfiguraci služby. Pokud je zadán SharedLogId, musíte zadat také tento SharedLogPath. |
| SharedLogPath |Plně kvalifikovaný název cesty |"" |Určuje plně kvalifikovanou cestu hello kde hello sdíleného souboru protokolu v případě všech spolehlivé služeb ve všech uzlech v clusteru hello, které neurčují hello SharedLogPath v jejich konkrétní konfiguraci služby. Ale pokud je zadán SharedLogPath, pak SharedLogId musí také uvést. |
| SharedLogSizeInMB |V megabajtech |8192 |Určuje počet hello MB místa na disku přidělit toostatically pro sdílené protokolu hello. Hello hodnota musí být 2 048 nebo větší. |

V Azure ARM nebo v šabloně JSON místní následující hello příklad ukazuje, jak toochange hello hello sdílené transakčního protokolu, který získá vytváří tooback všechny spolehlivé kolekce pro stavové služby.

    "fabricSettings": [{
        "name": "KtlLogger",
        "parameters": [{
            "name": "SharedLogSizeInMB",
            "value": "4096"
        }]
    }]

### <a name="sample-local-developer-cluster-manifest-section"></a>Ukázka místní vývojáře oddílu manifestu clusteru
Pokud chcete toochange to na vašem místním vývojovém prostředí, budete potřebovat soubor místní clustermanifest.xml tooedit hello.

```xml
   <Section Name="KtlLogger">
     <Parameter Name="SharedLogSizeInMB" Value="4096"/>
     <Parameter Name="WriteBufferMemoryPoolMinimumInKB" Value="8192" />
     <Parameter Name="WriteBufferMemoryPoolMaximumInKB" Value="8192" />
     <Parameter Name="SharedLogId" Value="{7668BB54-FE9C-48ed-81AC-FF89E60ED2EF}"/>
     <Parameter Name="SharedLogPath" Value="f:\SharedLog.Log"/>
   </Section>
```

### <a name="remarks"></a>Poznámky
protokolovač Hello se globální fond paměti přidělené z paměti nestránkovaného jádra, která je k dispozici tooall spolehlivé služby v uzlu pro ukládání do mezipaměti data o stavu před zápisem toohello vyhrazené protokolu přidružené hello spolehlivá služba repliky. velikost fondu Hello se řídí hello WriteBufferMemoryPoolMinimumInKB a WriteBufferMemoryPoolMaximumInKB nastavení. WriteBufferMemoryPoolMinimumInKB určuje obě hello počáteční velikost tohoto fondu paměti a může zmenšit hello nejnižší velikost toowhich hello fondem paměti. WriteBufferMemoryPoolMaximumInKB je, že může růst hello nejvyšší velikost toowhich hello fondem paměti. Každou repliku spolehlivá služba, který se otevírá může zvýšit hello velikost fondu paměti hello systému určit velikost až tooWriteBufferMemoryPoolMaximumInKB. Pokud existuje více požadavků na paměť z hello fondu paměti, než je k dispozici, požadavky na paměť se odloží až do paměti je k dispozici. Proto pokud fondu hello zápis vyrovnávací paměti je příliš malá pro konkrétní konfigurací pak výkonu může dojít ke snížení.

nastavení SharedLogId a SharedLogPath Hello jsou vždy použít společně toodefine hello GUID a protokolu pro všechny uzly v clusteru hello sdíleného umístění pro výchozí hello. sdílené protokol Hello výchozí se používá pro všechny spolehlivé služby, které neurčují hello nastavení v souborech settings.xml hello pro konkrétní službu hello. Pro nejlepší výkon musí být sdílené soubory protokolu umístěny na disky, které se používají výhradně pro kolizí tooreduce souboru protokolu hello sdílené.

SharedLogSizeInMB určuje hello množství toopreallocate místa na disku pro protokol sdílené výchozí hello na všech uzlech.  SharedLogId a SharedLogPath nemusí toobe zadaný v pořadí pro SharedLogSizeInMB toobe zadán.

## <a name="service-specific-configuration"></a>Konkrétní konfigurace služby
Můžete upravit výchozí konfigurace stavová spolehlivé služby s použitím balíčku hello konfiguraci (Config) nebo hello implementace služby (kód).

* **Konfigurace** -konfigurace prostřednictvím hello konfigurační balíček se provádí změnou hello souborech Settings.xml souboru, který je generován ve hello kořenového adresáře balíčku Microsoft Visual Studio ve složce Konfigurace hello u každé služby v aplikaci hello.
* **Kód** -konfigurace prostřednictvím kódu se provádí tak, že vytvoříte ReliableStateManager pomocí objekt ReliableStateManagerConfiguration sadou hello požadované možnosti.

Ve výchozím modulu runtime Azure Service Fabric hello předdefinované části z názvů v souboru hello souborech Settings.xml a odebírá hello konfigurační hodnoty při vytváření hello základní komponenty modulu runtime.

> [!NOTE]
> Proveďte **není** odstranit hello části názvy hello následující konfigurace v souborech Settings.xml souboru hello, generovaný v řešení sady Visual Studio hello, pokud máte v plánu tooconfigure služby prostřednictvím kódu.
> Přejmenování hello konfigurace balíčku nebo části názvů bude vyžadovat změny kódu při konfiguraci hello ReliableStateManager.
> 
> 

### <a name="replicator-security-configuration"></a>Konfigurace zabezpečení replikátoru
Konfigurace zabezpečení Replikátor jsou použité toosecure hello komunikační kanál, který se používá během replikace. To znamená, že služby nebude možné toosee vzájemně provoz replikace, zajišťující, že hello data, která je vysoké dostupnosti je také zabezpečení. Ve výchozím nastavení zabraňuje konfigurační oddíl prázdný zabezpečení zabezpečení replikace.

### <a name="default-section-name"></a>Výchozí název oddílu
ReplicatorSecurityConfig

> [!NOTE]
> toochange název oddílu přepsání hello replicatorSecuritySectionName parametr toohello ReliableStateManagerConfiguration konstruktor při vytváření hello ReliableStateManager pro tuto službu.
> 
> 

### <a name="replicator-configuration"></a>Replikátor konfigurace
Konfigurace Replikátor nakonfigurovat hello Replikátor, která je odpovědná za vytváření vysoce spolehlivé hello stavová spolehlivé stav služby replikace a zachování stavu hello místně.
Hello výchozí konfigurace je generován hello šablony sady Visual Studio a měla by stačit. Tato část pojednává o další konfigurace, které jsou k dispozici tootune Replikátor hello.

### <a name="default-section-name"></a>Výchozí název oddílu
ReplicatorConfig

> [!NOTE]
> toochange název oddílu přepsání hello replicatorSettingsSectionName parametr toohello ReliableStateManagerConfiguration konstruktor při vytváření hello ReliableStateManager pro tuto službu.
> 
> 

### <a name="configuration-names"></a>Názvy konfigurace
| Name (Název) | Jednotka | Výchozí hodnota | Poznámky |
| --- | --- | --- | --- |
| BatchAcknowledgementInterval |Sekundy |0.015 |Časové období, pro které Replikátor hello na sekundární počká hello po přijetí operace před odesláním zpět potvrzení toohello primární. Další potvrzení toobe odeslané pro operace zpracování v rámci tohoto intervalu se odesílají jako jedna odpověď. |
| ReplicatorEndpoint |Není k dispozici |Žádná výchozí hodnota – povinný parametr |IP adresa a port, který hello primární a sekundární Replikátor použije toocommunicate jiných replikátory hello sady replik. To by měl odkazovat koncový bod TCP prostředků v hello service manifest. Odkazovat příliš[manifestu prostředky služby](service-fabric-service-manifest-resources.md) tooread informace o definování koncový bod prostředků v service manifest. |
| MaxPrimaryReplicationQueueSize |Počet operací |8192 |Maximální počet operací ve frontě primární hello. Operace uvolněno po primární Replikátor hello přijímá potvrzení z všechny sekundární replikátory hello. Tato hodnota musí být větší než 64 a druhou mocninou 2. |
| MaxSecondaryReplicationQueueSize |Počet operací |16384 |Maximální počet operací ve frontě sekundární hello. Operace uvolněno po provedení vysoce dostupný prostřednictvím trvalost stavu. Tato hodnota musí být větší než 64 a druhou mocninou 2. |
| CheckpointThresholdInMB |MB |50 |Množství místa souboru protokolu, po jejímž uplynutí je kontrolován hello stavu. |
| MaxRecordSizeInKB |kB |1024 |Maximální velikost záznamů, které Replikátor hello můžou zapisovat do protokolu hello. Tato hodnota musí být násobkem 4 a je větší než 16. |
| MinLogSizeInMB |MB |0 (systém určit) |Minimální velikost hello transakčního protokolu. velikost tooa tootruncate nižší než toto nastavení nebude povoleno přihlášení Hello. Hodnota 0 znamená, že určí, že hello Replikátor hello minimální velikost protokolu. Zvýšení hodnoty tuto zvyšuje možnost hello plnění od pravděpodobnost příslušných protokolových záznamy, které ke zkrácení je snížena částečné kopie a přírůstkové zálohování. |
| TruncationThresholdFactor |Koeficient |2 |Určuje, jakou velikost protokolu hello zkrácení aktivuje. Prahová hodnota zkrácení je určen podle MinLogSizeInMB násobí hodnotou TruncationThresholdFactor. TruncationThresholdFactor musí být větší než 1. MinLogSizeInMB * TruncationThresholdFactor musí být menší než MaxStreamSizeInMB. |
| ThrottlingThresholdFactor |Koeficient |4 |Určuje, jakou velikost protokolu hello hello repliky spustí omezené. Omezení prahovou hodnotu (v MB) je dáno maximální ((MinLogSizeInMB * ThrottlingThresholdFactor),(CheckpointThresholdInMB * ThrottlingThresholdFactor)). Omezení prahovou hodnotu (v MB) musí být větší než prahová hodnota zkrácení (v MB). Zkrácení prahovou hodnotu (v MB) musí být menší než MaxStreamSizeInMB. |
| MaxAccumulatedBackupLogSizeInMB |MB |800 |Maximální počet shromážděných velikost (v MB) zálohování protokolů v řetězci dané zálohy protokolu. Požadavky přírůstkové zálohování se nezdaří, pokud hello přírůstkové zálohování by vygeneroval zálohy protokolu, které by způsobily protokoly zálohování hello shromážděných od hello relevantní úplného zálohování toobe větší než tato velikost. V takových případech je uživatel požadované tootake úplné zálohování. |
| SharedLogId |IDENTIFIKÁTOR GUID |"" |Určuje jedinečný toouse identifikátor GUID pro identifikaci sdílený soubor protokolu hello použít s této repliky. Služby obvykle neměli používat toto nastavení. Ale pokud je zadán SharedLogId, pak SharedLogPath musí také uvést. |
| SharedLogPath |Plně kvalifikovaný název cesty |"" |Určuje hello plně kvalifikovanou cestu, kde bude vytvořen hello sdílený soubor protokolu pro tuto repliku. Služby obvykle neměli používat toto nastavení. Ale pokud je zadán SharedLogPath, pak SharedLogId musí také uvést. |
| SlowApiMonitoringDuration |Sekundy |300 |Nastaví interval pro spravované volání rozhraní API sledování hello. Příklad: zadaná uživatelem funkce zálohování zpětného volání. Po uplynutí intervalu hello, sestavy stavu upozornění, bude odeslána toohello správce stavu. |

### <a name="sample-configuration-via-code"></a>Ukázková konfigurace prostřednictvím kódu
```csharp
class Program
{
    /// <summary>
    /// This is hello entry point of hello service host process.
    /// </summary>
    static void Main()
    {
        ServiceRuntime.RegisterServiceAsync("HelloWorldStatefulType",
            context => new HelloWorldStateful(context, 
                new ReliableStateManager(context, 
        new ReliableStateManagerConfiguration(
                        new ReliableStateManagerReplicatorSettings()
            {
                RetryInterval = TimeSpan.FromSeconds(3)
                        }
            )))).GetAwaiter().GetResult();
    }
}    
```
```csharp
class MyStatefulService : StatefulService
{
    public MyStatefulService(StatefulServiceContext context, IReliableStateManagerReplica stateManager)
        : base(context, stateManager)
    { }
    ...
}
```


### <a name="sample-configuration-file"></a>Vzorový konfigurační soubor
```xml
<?xml version="1.0" encoding="utf-8"?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Section Name="ReplicatorConfig">
      <Parameter Name="ReplicatorEndpoint" Value="ReplicatorEndpoint" />
      <Parameter Name="BatchAcknowledgementInterval" Value="0.05"/>
      <Parameter Name="CheckpointThresholdInMB" Value="512" />
   </Section>
   <Section Name="ReplicatorSecurityConfig">
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


### <a name="remarks"></a>Poznámky
BatchAcknowledgementInterval řídí latenci replikace. Hodnota '0' výsledkem hello nejnižší možnou latenci, náklady na hello propustnosti (jako další potvrzení zprávy musí být odeslána a zpracování jednotlivých obsahující méně potvrzení).
Hello větší hodnotu hello BatchAcknowledgementInterval, hello vyšší hello celková propustnost replikace hello náklady na vyšší latence operace. Výsledkem je přímo toohello latenci potvrzení transakcí.

Hodnota Hello CheckpointThresholdInMB ovládací prvky hello množství místa na disku, který hello Replikátor můžete použít informace o stavu toostore v souboru protokolu vyhrazené hello repliky. Zvýšení tuto tooa vyšší hodnotu než výchozí hello může mít za následek Rekonfigurace rychlejší, když novou repliku je přidána toohello sady. Toto je z důvodu přenosu toohello částečné stavu, který probíhá kvůli toohello dostupnost další historie operací v protokolu hello. Potenciálně může prodloužit dobu obnovení hello repliky po chybě.

nastavení MaxRecordSizeInKB Hello definuje maximální velikost hello záznam, který je možné zapsat hello Replikátor do souboru protokolu hello. Ve většině případů hello výchozí záznam 1024 KB velikost je optimální. Pokud však hello služba způsobuje položky dat větší toobe část informace o stavu hello, pak tato hodnota může být nutné toobe vyšší. Výhoda je malá při vytvoření MaxRecordSizeInKB menší než 1024, jako menší záznamů použijte pouze hello prostor potřebný pro menší záznam hello. Očekáváme, že tato hodnota by nutné toobe změnit v pouze výjimečných případech.

nastavení SharedLogId a SharedLogPath Hello jsou vždy použít společně toomake služby použití samostatných sdílených protokolu z hello výchozí sdílené protokolu pro uzel hello. Pro nejlepšího výkonu by měl určovat tolik služby jako možné hello stejné sdílené protokolu. Sdílené soubory protokolu musí být umístěny na disky, které se používají výhradně pro hello sdílené protokolu souboru tooreduce head přesun kolizí. Očekáváme, že tato hodnota by nutné toobe změnit v pouze výjimečných případech.

## <a name="next-steps"></a>Další kroky
* [Ladění aplikace Service Fabric v sadě Visual Studio](service-fabric-debugging-your-application.md)
* [Referenční informace pro vývojáře pro spolehlivé služby](https://msdn.microsoft.com/library/azure/dn706529.aspx)

