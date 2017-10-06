---
title: aaaArchitecture Azure Service Fabric | Microsoft Docs
description: "Service Fabric je že platforma distribuovaných systémů použít toobuild spolehlivé, škálovatelné a snadno spravovat aplikace pro hello cloud. Tento článek popisuje architekturu hello Service Fabric."
services: service-fabric
documentationcenter: .net
author: rishirsinha
manager: timlt
editor: rishirsinha
ms.assetid: 6b554243-70cb-4c22-9b28-1a8b4703f45e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/19/2017
ms.author: rsinha
ms.openlocfilehash: 0268578094ad1a0010ef44ed940f828b985f6c40
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-architecture"></a>Architektura Service Fabric
Service Fabric je vytvořené s vrstvami subsystémy. Tyto subsystémy povolit toowrite aplikace, které jsou:

* Vysoce dostupný
* Škálovatelné
* Spravovat
* Možností intenzivního testování

Hello následující diagram znázorňuje hlavní subsystémy hello služby prostředků infrastruktury.

![Diagram architektury Service Fabric](media/service-fabric-architecture/service-fabric-architecture.png)

V rámci distribuované systému hello možnost toosecurely komunikaci mezi uzly v clusteru je zásadní. V hello je základem hello zásobníku subsystému hello přenos, který poskytuje zabezpečenou komunikaci mezi uzly. Výše hello přenosu je subsystému hello federační subsystému, které clustery hello různých uzlech do jedné entity (s názvem clustery), aby mohli Service Fabric rozpoznala chyby, proveďte vedoucí volba a poskytují konzistentní směrování. subsystém Hello spolehlivost, jako vrstva nad hello federační subsystému, zodpovídá za hello spolehlivost Service Fabric služeb prostřednictvím mechanismy, jako je replikace, správy prostředků a převzetí služeb při selhání. subsystém federační Hello také základem hostování a aktivaci hello subsystému, který spravuje životní cyklus hello aplikace v jednom uzlu. subsystém správu Hello spravuje hello životního cyklu aplikací a služeb. subsystém testovatelnosti Hello pomáhá vývojářům aplikací testování jejich služeb prostřednictvím simulované chyb před a po nasazení aplikací a služeb tooproduction prostředí. Service Fabric nabízí možnost hello tooresolve umístění služeb pomocí jeho komunikačního subsystému. programovací modely aplikace jsou zveřejněné toodevelopers vrstva nad tyto subsystémy společně s nástroji tooenable modelu aplikace hello Hello.

## <a name="transport-subsystem"></a>Subsystém přenosu
subsystém přenosu Hello implementuje point-to-point datagram komunikační kanál. Tento kanál se používá pro komunikaci v rámci clusterů service fabric a komunikaci mezi hello service fabric cluster a klienty. Podporuje jednosměrné a schémata komunikace požadavku a odpovědi, které poskytuje hello základ pro implementaci všesměrového a vícesměrového vysílání v hello federační vrstvy. subsystém přenosu Hello zabezpečuje komunikaci pomocí X509 certifikáty nebo zabezpečení systému Windows. Tento subsystém používá se interně pomocí Service Fabric a není přímo přístupný toodevelopers pro programování aplikací.

## <a name="federation-subsystem"></a>Subsystém Federation
Pořadí tooreason o sada uzlů v distribuované systému je nutné toohave konzistentní zobrazení hello systému. subsystém federační Hello používá primitiv komunikace hello poskytované hello přenosu subsystému a oky hello různé uzly do jediného jednotná clusteru, který můžete důvodu o. Poskytuje hello distribuovaných systémů primitiv potřeby podle hello jiné subsystémy - detekce chyb, vedoucí volba a konzistentní směrování. subsystém federační Hello je postavená na distribuované zatřiďovacích tabulkách tokenu mezerami 128-bit. subsystém Hello vytvoří kruhová topologie přes hello uzly se každý uzel v hello prstenec přidělenou podmnožinu hello tokenu místa pro vlastnictví. Pro zjišťování selhání vrstvy hello používá mechanismus "pronájmu" na základě prezenční signálu a arbitrážního. subsystém federační Hello také zaručuje prostřednictvím komplikované spojení a odeslání protokolů, které existuje jenom jeden vlastník token kdykoli. To poskytuje vedoucí volba a konzistentní směrování záruky.

## <a name="reliability-subsystem"></a>Spolehlivost subsystému
Hello spolehlivost subsystému poskytuje hello mechanismus toomake hello stavu služby Service Fabric vysoce dostupný prostřednictvím použití hello hello *Replikátor*, *Failover Manager*, a  *Nástroje pro vyrovnávání prostředků*.

* Hello Replikátor zajistí, že změny stavu v replice hello primární služba bude automaticky replikované toosecondary repliky, zachování konzistence mezi hello primární a sekundární repliky v sadě replik služby. Replikátor Hello je odpovědná za správu kvora mezi hello replik v hello sady replik. Komunikuje se službou hello převzetí služeb při selhání jednotky tooget hello seznam operací tooreplicate a poskytne mu agent Rekonfigurace hello hello konfiguraci sady replik hello. Tato konfigurace označuje, které operace hello repliky potřebovat toobe replikovat. Service Fabric nabízí výchozí Replikátor, názvem Replikátor Fabric, která mohou být využívána hello programovací model rozhraní API toomake hello stav služby vysoce dostupné a spolehlivé.
* Hello Správce převzetí služeb při selhání zajistí, že při přidávání tooor odebrat z clusteru hello uzlů, hello zatížení automaticky rozloží mezi uzly hello k dispozici. V případě selhání uzlu v clusteru hello hello clusteru automaticky rekonfigurují dostupnost toomaintain repliky služby hello.
* Hello Resource Manager umístí repliky služby napříč domény selhání v clusteru hello a zajišťuje, aby všechny jednotky převzetí služeb při selhání provozu. Hello Resource Manager také vyrovnává prostředky služby napříč hello základní sdílený fond rozdělení optimální uniform zatížení tooachieve uzly clusteru.

## <a name="management-subsystem"></a>Subsystém správy
subsystém Hello správu poskytuje služby začátku do konce a správa životního cyklu aplikací. Rutiny prostředí PowerShell a rozhraní API pro správu umožňují tooprovision, nasazení, opravy, upgradu a poté zřizování aplikace bez ztráty dostupnosti. subsystém správu Hello to provede prostřednictvím hello následující služby.

* **Správce clusteru**: Toto je hello primární služba, která interaguje s hello Správce převzetí služeb při selhání z spolehlivost tooplace hello aplikací na uzlech hello podle omezení umístění služby hello. Hello Resource Manager v subsystému převzetí služeb při selhání zajistí hello omezení nikdy přerušená. Správce clusteru Hello spravuje hello životního cyklu aplikací hello z zřídit toode-provision. Integruje se s hello health manager tooensure že dostupnosti aplikace není během upgradu ke ztrátě z hlediska sémantického stavu.
* **Správce stavu**: Tato služba umožňuje sledování stavu aplikací, služeb a entity clusteru. Informace o stavu, který je pak agregovat do úložiště centralizované stavu hello mohou zasílat zprávy o entitami clusteru (jako je například uzlů, oddílů služby a repliky). Tato informace o stavu poskytuje snímek stavu celkové v daném okamžiku hello služby a distribuované ve více uzlech v clusteru hello, takže se budete tootake uzly všechny potřebné opravné akce. Dotaz stavu, že rozhraní API umožňují události stavu hello tooquery hlášené toohello stavu subsystému. Hello stavu dotaz rozhraní API vrátit hello stavu nezpracovaná data uložená v hello stavu ukládání nebo hello agregován, interpretovat data o stavu pro určitý cluster entitu.
* **Úložiště bitových kopií**: Tato služba poskytuje úložiště a distribuci hello binární soubory aplikace. Tato služba poskytuje jednoduché souborů DFS úložiště, kde aplikace hello jsou nahrané tooand stáhnout z.

## <a name="hosting-subsystem"></a>Hostování subsystému
Správce clusteru Hello informuje, že hello hostování subsystému (spuštěná v každém uzlu), který ji obsluhuje požadavkům toomanage konkrétním uzlu. Hello hostování subsystému potom spravuje životní cyklus hello hello aplikace v tomto uzlu. Komunikuje se službou hello spolehlivost a stavu součásti tooensure že hello repliky umístěny správně a jsou v pořádku.

## <a name="communication-subsystem"></a>Komunikačního subsystému
Tento subsystém poskytuje spolehlivé zasílání zpráv v rámci zjišťování hello clusteru a služby pomocí služby pojmenování hello. Hello Naming service řeší služba názvy tooa umístění v clusteru hello a umožňuje uživatelům toomanage služby názvy a vlastnosti. Pomocí služby pojmenování hello, klienti mohou bezpečně komunikovat s jakéhokoli uzlu v clusteru tooresolve hello název služby a načíst metadata služby. Pomocí jednoduchého pojmenování klientského rozhraní API, můžete vyvíjet uživatelé Service Fabric, služeb a klientů podporujících řešení hello aktuální umístění v síti i přes uzel náskok nebo hello Změna velikosti znovu hello clusteru.

## <a name="testability-subsystem"></a>Testovatelnosti subsystému
Testovatelnosti je sada nástrojů, které jsou vytvořené speciálně pro testování služby založený na Service Fabric. Hello nástroje umožňují vývojář snadno vyvolat smysluplný chyb a spuštění testu tooexercise scénáře a ověření hello množství stavy a přechody, které služba bude na základě zkušeností uživatelů v průběhu své životnosti, všechny řízené a bezpečným způsobem. Testovatelnosti také poskytuje mechanismus toorun delší testy, které můžete iteraci v rámci různých možných selhání bez ztráty dostupnosti. To poskytuje test v provozním prostředí.

