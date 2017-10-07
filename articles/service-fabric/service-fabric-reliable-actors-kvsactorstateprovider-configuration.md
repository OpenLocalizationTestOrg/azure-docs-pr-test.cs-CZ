---
title: "nastavení KVSActorStateProvider aaaChange v Azure mikroslužeb | Microsoft Docs"
description: "Další informace o konfiguraci Azure Service Fabric stavová aktéři typu KVSActorStateProvider."
services: Service-Fabric
documentationcenter: .net
author: sumukhs
manager: timlt
editor: 
ms.assetid: dbed72f4-dda5-4287-bd56-da492710cd96
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 6/29/2017
ms.author: sumukhs
ms.openlocfilehash: e003512678556e68a8926b1b9c6c28d9ae3979d2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-reliable-actors--kvsactorstateprovider"></a>Konfigurace Reliable Actors – KVSActorStateProvider
Výchozí konfigurace hello KVSActorStateProvider můžete upravit změnou hello souborech settings.xml souboru, který je generován ve kořenového adresáře balíčku Microsoft Visual Studio hello složce hello konfigurace pro zadaný objektu actor hello.

modulu runtime Azure Service Fabric Hello předdefinované části z názvů v souboru hello souborech settings.xml a odebírá hello konfigurační hodnoty při vytváření hello základní komponenty modulu runtime.

> [!NOTE]
> Proveďte **není** odstranit nebo změnit názvy části hello hello následující konfigurace v souborech settings.xml souboru hello, který je generován ve hello řešení sady Visual Studio.
> 
> 

## <a name="replicator-security-configuration"></a>Konfigurace zabezpečení replikátoru
Konfigurace zabezpečení Replikátor jsou použité toosecure hello komunikační kanál, který se používá během replikace. To znamená, že služby nelze vzájemně provoz replikace, zajišťující, že hello data, která vysokou dostupnost, je také zabezpečené.
Ve výchozím nastavení zabraňuje konfigurační oddíl prázdný zabezpečení zabezpečení replikace.

### <a name="section-name"></a>Název oddílu
&lt;ActorName&gt;ServiceReplicatorSecurityConfig

## <a name="replicator-configuration"></a>Replikátor konfigurace
Konfigurace Replikátor nakonfigurovat hello Replikátor, která je odpovědná za vytváření vysoce spolehlivé hello zprostředkovatele stavu objektu Actor stavu.
Hello výchozí konfigurace je generován hello šablony sady Visual Studio a měla by stačit. Tato část pojednává o další konfigurace, které jsou k dispozici tootune Replikátor hello.

### <a name="section-name"></a>Název oddílu
&lt;ActorName&gt;ServiceReplicatorConfig

### <a name="configuration-names"></a>Názvy konfigurace
| Name (Název) | Jednotka | Výchozí hodnota | Poznámky |
| --- | --- | --- | --- |
| BatchAcknowledgementInterval |Sekundy |0.015 |Časové období, pro které Replikátor hello na sekundární počká hello po přijetí operace před odesláním zpět potvrzení toohello primární. Další potvrzení toobe odeslané pro operace zpracování v rámci tohoto intervalu se odesílají jako jedna odpověď. |
| ReplicatorEndpoint |Není k dispozici |Žádná výchozí hodnota – povinný parametr |IP adresa a port, který hello primární a sekundární Replikátor použije toocommunicate jiných replikátory hello sady replik. To by měl odkazovat koncový bod TCP prostředků v hello service manifest. Odkazovat příliš[manifestu prostředky služby](service-fabric-service-manifest-resources.md) tooread informace o definování koncový bod prostředků v hello service manifest. |
| RetryInterval |Sekundy |5 |Časové období, po které hello Replikátor znovu přenáší a zpráva, pokud neobdrží potvrzení operace. |
| Maxreplicationmessagesize. |Bajty |50 MB |Maximální velikost data replikace, která mohou být přenesena do jedné zprávy. |
| MaxPrimaryReplicationQueueSize |Počet operací |1024 |Maximální počet operací ve frontě primární hello. Operace uvolněno po primární Replikátor hello přijímá potvrzení z všechny sekundární replikátory hello. Tato hodnota musí být větší než 64 a druhou mocninou 2. |
| MaxSecondaryReplicationQueueSize |Počet operací |2 048 |Maximální počet operací ve frontě sekundární hello. Operace uvolněno po provedení vysoce dostupný prostřednictvím trvalost stavu. Tato hodnota musí být větší než 64 a druhou mocninou 2. |

## <a name="store-configuration"></a>Konfigurace úložiště
Konfigurace úložiště jsou použité tooconfigure hello místní úložiště používané toopersist hello stavu, který je právě replikován.
Hello výchozí konfigurace je generován hello šablony sady Visual Studio a měla by stačit. Tato část pojednává o další konfigurace, které jsou k dispozici tootune hello místního úložiště.

### <a name="section-name"></a>Název oddílu
&lt;ActorName&gt;ServiceLocalStoreConfig

### <a name="configuration-names"></a>Názvy konfigurace
| Name (Název) | Jednotka | Výchozí hodnota | Poznámky |
| --- | --- | --- | --- |
| MaxAsyncCommitDelayInMilliseconds |počet milisekund |200 |Nastaví maximální hello dávkování interval pro potvrzení trvanlivý místního úložiště. |
| MaxVerPages |Počet stránek |16384 |Hello maximální počet stránek verze v hello místní uložení databáze. Určuje maximální počet nezpracovaných transakcí hello. |

## <a name="sample-configuration-file"></a>Vzorový konfigurační soubor
```xml
<?xml version="1.0" encoding="utf-8"?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Section Name="MyActorServiceReplicatorConfig">
      <Parameter Name="ReplicatorEndpoint" Value="MyActorServiceReplicatorEndpoint" />
      <Parameter Name="BatchAcknowledgementInterval" Value="0.05"/>
   </Section>
   <Section Name="MyActorServiceLocalStoreConfig">
      <Parameter Name="MaxVerPages" Value="8192" />
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

