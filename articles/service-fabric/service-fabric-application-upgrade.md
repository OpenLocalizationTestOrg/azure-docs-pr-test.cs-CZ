---
title: upgrade aplikace Fabric aaaService | Microsoft Docs
description: "Tento článek obsahuje úvod tooupgrading aplikace Service Fabric, včetně výběrem možnosti upgradu režimy a provádění kontroly stavu."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: 803c9c63-373a-4d6a-8ef2-ea97e16e88dd
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: 6f649ef4a5c0afab682522bcba7d2d66a4268ead
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-application-upgrade"></a>Upgrade aplikace Service Fabric
Aplikace Azure Service Fabric je kolekce služeb. Během upgradu Service Fabric porovná hello nové [manifest aplikace](service-fabric-application-model.md#describe-an-application) s hello předchozí verze a určuje, které služby v aplikaci hello vyžadovat aktualizace. Service Fabric porovnává verze hello čísla ve službě hello manifesty s hello čísla verze v předchozí verzi hello. Je-li služba se nezměnila, není aktualizován dané služby.

## <a name="rolling-upgrades-overview"></a>Vrácení upgradu – přehled
V postupného upgradu aplikace hello upgradu provádí ve fázích. V každé fázi je hello upgrade použité tooa dílčí sadu uzlů v clusteru hello, názvem domény služby aktualizace. V důsledku toho hello aplikace bude k dispozici v rámci upgradu hello. Během upgradu hello hello clusteru může obsahovat kombinaci hello staré a nové verze.

Z tohoto důvodu musí být hello dvě verze dopředného a zpětně kompatibilní. Pokud nejsou kompatibilní, Správce aplikací hello je zodpovědná za pracovní více fází upgradu toomaintain dostupnosti. V případě více fází upgradu je prvním krokem hello upgrade tooan zprostředkující verze hello aplikace, který je kompatibilní s hello předchozí verze. druhým krokem Hello je hello tooupgrade konečné se verzi, která dělí kompatibilitu s verzí hello před aktualizací, ale je kompatibilní s verzí zprostředkující hello.

Aktualizace domény jsou určené v manifestu clusteru hello, pokud konfigurujete hello cluster. Aktualizace domén neobdrží aktualizace v určitém pořadí. Doména aktualizace je logická jednotka nasazení pro aplikaci. Aktualizace domén povolit hello služby tooremain na vysokou dostupnost během upgradu.

Bez vrácení upgrady je možné, pokud je použité tooall uzly v clusteru hello, což je případ hello, pokud má jenom jednu aktualizační doménu aplikace hello hello upgrade. Tento přístup se nedoporučuje, protože služba hello přestane fungovat a není k dispozici v době hello upgradu. Kromě toho Azure neposkytuje žádné záruky, při nastavení clusteru s doménou pouze jednu aktualizaci.

## <a name="health-checks-during-upgrades"></a>Kontroly stavu během upgradu
Pro účely upgradu mají zásady stavu toobe nastavit (nebo mohou být použity výchozí hodnoty). Upgrade se říká úspěšné když jsou všechny aktualizace domény upgradován během hello zadané vypršení časových limitů, a když se všechny aktualizace domén se považují za v pořádku.  Domény v pořádku aktualizace znamená, že aktualizace domény hello předán všechny kontroly stavu hello určená v zásadách stavu hello. Například může zásady stavu vyžádá, musí být všechny služby v rámci instance aplikace *pořádku*, jak je definováno stavu Service Fabric.

Zásady stavu a kontroly během upgradu pomocí Service Fabric se bez ohledu na služby a aplikace. To znamená, že se provádějí žádné testy specifickou pro službu.  Například služby může mít požadavek propustnost, ale Service Fabric nemá hello informace toocheck propustnost. Odkazovat toohello [stavu články](service-fabric-health-introduction.md) pro hello kontroly, které se provádí. Hello kontroly, které dojít během upgradu zahrnout testy pro zda byl balíček aplikace hello zkopírován správně, zda byla spuštěna hello instance a tak dále.

Stav aplikace Hello je agregaci hello podřízených entit aplikace hello. Stručně řečeno Service Fabric vyhodnotí hello stav aplikace hello prostřednictvím hello stavu, který je hlášen na aplikace hello. Také vyhodnocuje hello stavu všech hello služeb pro aplikace hello tímto způsobem. Service Fabric další vyhodnotí hello stav služby aplikace hello agregování hello stav jejich podřízené položky, jako je například hello služby repliky. Jakmile splňují zásady stavu aplikace hello hello upgradu pokračovat. Pokud porušení zásady stavu hello upgradu aplikace hello se nezdaří.

## <a name="upgrade-modes"></a>Upgrade režimy
Hello režim, který doporučujeme pro upgradu aplikace je hello monitorovat režimu, který je režim hello běžně používá. Monitorované režimu provede hello upgrade na jednu aktualizační doménu a pokud kontroluje všechny stavy průchodu (podle hello zadanou zásadu), se přesune na další aktualizaci domény toohello automaticky.  Pokud selhání kontroly stavu a/nebo se dosáhne vypršení časových limitů, hello upgrade je pro doménu aktualizace hello buď vrácena nebo režim hello je změněné toounmonitored ručně. Můžete nakonfigurovat hello upgradu toochoose jednu z těchto dvou režimů pro upgrade se nezdařilo. 

Sledována ruční režim musí po každé aktualizaci na doméně aktualizace, tookick vypnout hello upgradu na další aktualizaci domény hello ruční zásah. Budou provedeny žádné kontroly stavu Service Fabric. Správce Hello provádí hello stavu nebo stavu kontroly před zahájením upgradu hello v hello další aktualizaci domény.

## <a name="upgrade-default-services"></a>Upgradujte výchozí služby
Během procesu upgradu hello aplikace lze upgradovat výchozích služeb v rámci aplikace Service Fabric. Výchozí služby jsou definovány v hello [manifest aplikace](service-fabric-application-model.md#describe-an-application). Standardní pravidla Hello upgradu výchozích služeb jsou:

1. Výchozí služby v hello nové [manifest aplikace](service-fabric-application-model.md#describe-an-application) které neexistují v clusteru hello se vytvářejí.
> [!TIP]
> [EnableDefaultServicesUpgrade](service-fabric-cluster-fabric-settings.md#fabric-settings-that-you-can-customize) vyžaduje toobe nastavit hello tooenable tootrue následující pravidla. Tato funkce je podporovaná ze v5.5.

2. Výchozí služby existující v obou předchozích [manifest aplikace](service-fabric-application-model.md#describe-an-application) a jsou aktualizovány novou verzi. Popis služby v nové verzi hello by přepsala těch, které již v clusteru hello. Upgrade aplikace by vrácení zpět automaticky při aktualizaci selhání výchozí služby.
3. Výchozí služby v předchozí hello [manifest aplikace](service-fabric-application-model.md#describe-an-application) , ale ne v nové verzi hello se odstraní. **Všimněte si, že tento odstraňování výchozích služeb nelze vrátit zpět.**

V případě, že aplikace je vrácena upgradu, výchozí služby se stav vrácený toohello před zahájením upgradu. Ale odstraněné služby nikdy možné vytvořit.

## <a name="application-upgrade-flowchart"></a>Vývojový diagram upgradu aplikace
Vývojový diagram Hello níže vám může pomoct pochopit hello upgradu aplikace Service Fabric. Konkrétně hello toku popisuje jak hello vypršení časových limitů, včetně *HealthCheckStableDuration*, *HealthCheckRetryTimeout*, a *UpgradeHealthCheckInterval*, pomáhá řídit při upgradu hello v jedné aktualizační doméně se považuje za úspěch nebo selhání.

![Hello upgradu pro Fabric aplikaci služby][image]

## <a name="next-steps"></a>Další kroky
[Upgrade vaší aplikace pomocí sady Visual Studio](service-fabric-application-upgrade-tutorial.md) vás provede upgrade aplikace pomocí sady Visual Studio.

[Upgrade vaší aplikace pomocí prostředí Powershell](service-fabric-application-upgrade-tutorial-powershell.md) vás provede upgrade aplikace pomocí prostředí PowerShell.

Řídí, jak vaše aplikace upgraduje pomocí [Upgrade parametry](service-fabric-application-upgrade-parameters.md).

Aby aplikace upgradů kompatibilní metodou učení jak toouse [serializace dat](service-fabric-application-upgrade-data-serialization.md).

Zjistěte, jak toouse pokročilé funkce při upgradu vaší aplikace tím, že odkazuje příliš[Pokročilá témata](service-fabric-application-upgrade-advanced.md).

Řešení běžných potíží v upgradů aplikací tím, že odkazuje toohello kroky v [řešení potíží s aplikací upgrady](service-fabric-application-upgrade-troubleshooting.md).

[image]: media/service-fabric-application-upgrade/service-fabric-application-upgrade-flowchart.png
