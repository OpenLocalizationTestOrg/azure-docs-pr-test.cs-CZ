---
title: 'Upgrade aplikace: upgrade parametry | Microsoft Docs'
description: "Popisuje parametry související tooupgrading aplikace Service Fabric, včetně stavu kontroly tooperform a zásady tooautomatically zpět hello upgradu."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: a4170ac6-192e-44a8-b93d-7e39c92a347e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: abd0ba48c223be9aa0909c7a0100ba5986430db3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="application-upgrade-parameters"></a>Parametry upgradu aplikace
Tento článek popisuje různé parametry, které se vztahují během hello upgradu aplikace Azure Service Fabric hello. Parametry Hello zahrnují hello název a verze aplikace hello. Jsou knoflíky, které řídí hello vypršení časových limitů a kontroly stavu, které se použijí při upgradu hello a určují hello zásady, které je nutné použít, když se upgrade nezdaří.

<br>

| Parametr | Popis |
| --- | --- |
| ApplicationName |Název aplikace hello, která se upgraduje. Příklady: fabric: / VisualObjects, fabric: / ClusterMonitor |
| TargetApplicationTypeVersion |Hello verze typu aplikace hello, který hello upgradu cíle. |
| FailureAction |Hello akci prováděnou Service Fabric hello upgrade selže. Hello aplikace může být vrácena zpět toohello před aktualizací verze (vrácení) nebo upgradu hello je pravděpodobně zastavená v doména upgradu současné hello. V druhém případě hello režimu upgradu hello je také změněné tooManual. Povolené hodnoty jsou vrácení zpět a ručně. |
| HealthCheckWaitDurationSec |Hello toowait čas (v sekundách) po upgradu hello dokončení v doméně pro upgrade hello před Service Fabric vyhodnotí hello stavu aplikace hello. Tato doba trvání lze považovat také za hello času, které aplikace by měl být spuštěn, než ho lze považovat za v pořádku. Pokud kontrola stavu hello úspěšně projde, pokračuje procesu upgradu hello toohello další upgradovací doméně.  Pokud se nezdaří Kontrola stavu hello, Service Fabric čeká intervalu (hello UpgradeHealthCheckInterval) před opakovaným pokusem o kontrolu stavu hello znovu, dokud hello dosaženo HealthCheckRetryTimeout. Výchozí Hello a doporučená hodnota je 0 sekund. |
| HealthCheckRetryTimeoutSec |Hello doby (v sekundách), Service Fabric pokračuje vyhodnocení stavu tooperform než prohlásí hello upgrade, protože se nezdařilo. Výchozí hodnota Hello je 600 sekund. Tato doba trvání spustí po dosažení HealthCheckWaitDuration. V rámci této HealthCheckRetryTimeout Service Fabric může provádět více kontroly stavu o stavu aplikace hello. Hello výchozí hodnota je 10 minut a je nutné odpovídajícím způsobem přizpůsobit pro vaši aplikaci. |
| HealthCheckStableDurationSec |tooverify Hello doba trvání (v sekundách), který hello aplikace je stabilní před přesunutí toohello další upgradovací doméně nebo dokončení hello upgradu. Tato čekací doba trvání je použité tooprevent nebyla rozpoznána změn stavu správné po se provádí kontrola stavu hello. Hello výchozí hodnota je 120 sekundách a by odpovídajícím způsobem přizpůsobit pro vaši aplikaci. |
| UpgradeDomainTimeoutSec |Maximální doba (v sekundách) pro upgrade na jednu doménu upgradu. Pokud tento časový limit bude dosažen, hello upgradu zastaví a pokračuje podle nastavení hello UpgradeFailureAction. Hello výchozí hodnota je nikdy (nekonečné) a by odpovídajícím způsobem přizpůsobit pro vaši aplikaci. |
| UpgradeTimeout |Časový limit (v sekundách), platí pro celou upgrade hello. Pokud tento časový limit bude dosažen, hello upgradu zastaví a UpgradeFailureAction se aktivuje. Hello výchozí hodnota je nikdy (nekonečné) a by odpovídajícím způsobem přizpůsobit pro vaši aplikaci. |
| UpgradeHealthCheckInterval |četnost Hello, že je zaškrtnuté hello stav. Tento parametr je zadán v hello ClusterManager části hello *clusteru* *manifest*a není určena jako součást upgradu rutiny hello. Hello výchozí hodnota je 60 sekund. |

<br>

## <a name="service-health-evaluation-during-application-upgrade"></a>Vyhodnocení stavu služby během upgradu aplikace
<br>
kritéria hodnocení stavu Hello jsou volitelné. Když kritérií hodnocení stavu hello nejsou zadané při spuštění upgradu, Service Fabric používá zásady stavu aplikace hello zadaný v hello ApplicationManifest.xml instanci aplikace hello.

<br>

| Parametr | Popis |
| --- | --- |
| ConsiderWarningAsError |Výchozí hodnota je False. Považovat za události stavu hello upozornění pro aplikace hello chyby při vyhodnocování stavu hello aplikace hello během upgradu. Ve výchozím nastavení Service Fabric nevyhodnocuje upozornění stavu události toobe selhání (chyby), takže hello upgradu můžete pokračovat, i když jsou události upozornění. |
| MaxPercentUnhealthyDeployedApplications |Výchozí a doporučená hodnota je 0. Určete maximální počet nasazených aplikací hello (viz hello [stavu části](service-fabric-health-introduction.md)), může být není v pořádku, než aplikace hello není v pořádku a selže hello upgradu. Tento parametr definuje stav aplikací hello na uzlu hello a pomáhá rozpoznat problémy při upgradu. Obvykle hello replik aplikace hello získat Vyrovnávání zatížení sítě toohello jiného uzlu, který umožňuje tooappear aplikace hello v pořádku, což umožňuje upgradu tooproceed hello. Zadáním striktní MaxPercentUnhealthyDeployedApplications stavu Service Fabric můžete rychle zjistit problém s balíčkem aplikace hello a pomohli vytvořit rychlé upgradu selhání. |
| MaxPercentUnhealthyServices |Výchozí a doporučená hodnota je 0. Určete maximální počet služeb hello v instanci aplikace hello, který může být není v pořádku, než aplikace hello není v pořádku a hello upgradu se nezdaří. |
| MaxPercentUnhealthyPartitionsPerService |Výchozí a doporučená hodnota je 0. Zadejte maximální počet oddílů hello ve službě, která může být není v pořádku, než hello služby není v pořádku. |
| MaxPercentUnhealthyReplicasPerPartition |Výchozí a doporučená hodnota je 0. Zadejte maximální počet replik hello v oddílu, který může být není v pořádku, než hello oddílu není v pořádku. |
| UpgradeReplicaSetCheckTimeout |**Bezstavové služby**– v jedné doméně upgradu, Service Fabric pokusí tooensure, že jsou k dispozici další instance služby hello. Pokud počet instancí cílové hello je víc než jedna, Service Fabric čeká víc než jednu instanci toobe k dispozici, až tooa maximální hodnotu časového limitu. Tento časový limit je zadán pomocí vlastnosti UpgradeReplicaSetCheckTimeout hello. Pokud vyprší časový limit hello, Service Fabric pokračuje v upgradu hello, bez ohledu na počet hello instancí služby. Pokud počet instancí cílové hello, Service Fabric nečeká a bude okamžitě pokračovat hello upgradu. **Stavové služby**– v jedné doméně upgradu, Service Fabric pokusí tooensure, který hello repliky sada má kvorum. Service Fabric čeká kvora toobe, který je k dispozici, až tooa maximální hodnotu časového limitu (určeného vlastností UpgradeReplicaSetCheckTimeout hello). Pokud vyprší časový limit hello, Service Fabric pokračuje v upgradu hello, bez ohledu na to kvora. Toto nastavení je sada jako nikdy (nekonečné) při vracení dál a 900 sekund při vrácení zpět. |
| ForceRestart |Při aktualizaci konfigurace nebo datového balíčku bez aktualizace kódu služby hello, restartování služby hello jenom v případě hello ForceRestart vlastnost nastavenou tootrue. Po dokončení aktualizace hello Service Fabric upozorní hello služby a zda je k dispozici nový balíček konfigurace nebo datový balíček. Hello služba je zodpovědná za použití hello změny. V případě potřeby můžete restartovat službu hello sám sebe. |

<br>
<br>
jeden typ služby pro instanci aplikace lze zadat kritéria MaxPercentUnhealthyServices, MaxPercentUnhealthyPartitionsPerService a MaxPercentUnhealthyReplicasPerPartition Hello. Nastavení těchto parametrů za služby umožňuje pro různé služby aplikaci toocontain typy pomocí různých vyhodnocení zásad. Typ služby bezstavové brány může mít například MaxPercentUnhealthyPartitionsPerService, který je odlišný od typu stavový stroj služby pro konkrétní instanci aplikace.

## <a name="next-steps"></a>Další kroky
[Upgrade vaší aplikace pomocí sady Visual Studio](service-fabric-application-upgrade-tutorial.md) vás provede upgrade aplikace pomocí sady Visual Studio.

[Upgrade vaší aplikace pomocí prostředí Powershell](service-fabric-application-upgrade-tutorial-powershell.md) vás provede upgrade aplikace pomocí prostředí PowerShell.

[Upgrade vaší aplikace pomocí Service Fabric rozhraní příkazového řádku v systému Linux](service-fabric-application-lifecycle-sfctl.md#upgrade-application) vás provede upgrade aplikace pomocí Service Fabric rozhraní příkazového řádku.

[Upgrade vaší aplikace pomocí modulu plug-in Eclipse Service Fabric](service-fabric-get-started-eclipse.md#upgrade-your-service-fabric-java-application)

Aby aplikace upgradů kompatibilní metodou učení jak toouse [serializace dat](service-fabric-application-upgrade-data-serialization.md).

Zjistěte, jak toouse pokročilé funkce při upgradu vaší aplikace tím, že odkazuje příliš[Pokročilá témata](service-fabric-application-upgrade-advanced.md).

Řešení běžných potíží v upgradů aplikací tím, že odkazuje toohello kroky v [řešení potíží s aplikací upgrady](service-fabric-application-upgrade-troubleshooting.md).
