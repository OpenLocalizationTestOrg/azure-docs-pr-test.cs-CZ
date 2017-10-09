---
title: "aaaAzure zotavení po havárii Service Fabric | Microsoft Docs"
description: "Azure Service Fabric nabízí hello možnosti potřebné toodeal ve všech typech havárie. Tento článek popisuje typy hello havárie, které můžou nastat a jak toodeal s nimi."
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 04b8348fb63e8a1c76a8f722c4c8255b339908e2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="disaster-recovery-in-azure-service-fabric"></a>Zotavení po havárii v Azure Service Fabric
Důležitou součástí doručování vysoká dostupnost zajišťuje, že služby přežijí všechny různých typů chyb. To je obzvláště důležité pro chyby, které neplánované a mimo váš dosah. Tento článek popisuje některé běžné selhání režimy, které může být havárie není-li modelovány a spravovat správně. Také popisují akce a jejich zmírnění tootake, pokud přesto došlo k havárii. cílem Hello je toolimit nebo eliminovat hello riziko výpadku nebo ztráty dat, když dojde k selhání, plánované nebo jinak, mohlo dojít.

## <a name="avoiding-disaster"></a>Zamezení po havárii
Primární cílem Service Fabric je toohelp modelu vašeho prostředí a vaše služby tak, že běžné typy selhání nejsou havárie. 

Obecně existují dva typy po havárii nebo selhání scénáře:

1. Hardwarový nebo softwarový chyb
2. Provozní chyb

### <a name="hardware-and-software-faults"></a>Chyby hardwaru a softwaru
Hardware a software chyby mohou být nepředvídatelné. Hello chyb toosurvive nejjednodušší způsob, jak je spuštěna další kopie hello služby předané hranice selhání hardwaru nebo softwaru. Například pokud vaše služba běží pouze na jednu konkrétní počítač, pak hello selhání tohoto jednoho počítače je po havárii pro danou službu. Hello jednoduchý způsob tooavoid tento po havárii je tooensure, zda je ve skutečnosti spuštěna služba hello ve více počítačích. Testování je také nutné tooensure hello selhání jeden počítač nemá přerušit hello službou. Plánování kapacity zajišťuje jinde vytvořením nahrazení instance a že snížení kapacity nemá přetížení hello zbývající služby. Hello stejného vzoru funguje bez ohledu na to, co se pokoušíte tooavoid hello selhání. Například. Pokud máte obavy o selhání hello sítě SAN, spuštěním napříč více sítě SAN. Pokud máte obavy o hello ztrátě rack serverů, můžete spustit napříč rackovými stojany. Pokud máte obavy o ztrátě hello datových center, měli spustit služby napříč více oblastí Azure nebo datových centrech. 

Při spuštění v tomto typu rozložené režimu, jste stále o subjektu toosome typy souběžných selhání, ale jediný a i několik chyb určitého typu (například: jeden odkaz selhání virtuálního počítače nebo sítě) jsou zpracovávány automaticky (a proto už "havárie"). Service Fabric nabízí mnoho mechanismy pro rozšíření hello clusteru a zpracovává zpět přináší selhání uzlů a služby. Service Fabric také umožňuje spouštět velký počet instancí služeb v pořadí tooavoid tyto typy neplánované výpadky z měnící do skutečné havárie.

Může být z důvodů, proč spuštění dostatečně velké na to toospan nasazení přes selhání není v rámci výpočetních procesů. Například může trvat více hardwarových prostředků než nejste ochotná toopay pro relativní toohello riziko selhání. Při plánování práce s distribuovaných aplikací, může to být, stav replikace a další komunikaci směrování stojí napříč geografické vzdálenosti příčiny nepřijatelné latence. Místo, kde je tento řádek vykreslovat se liší pro jednotlivé aplikace. Pro chyby softwaru konkrétně hello selhání může být ve službě hello pokoušíte tooscale. Další kopie není v tomto případě zabránit hello po havárii, vzhledem k tomu, že stav selhání hello je korelační napříč všemi instancemi hello.

### <a name="operational-faults"></a>Provozní chyb
I v případě služby je rozloženy na celém světě hello s mnoha propouštění, může stále přetrvávají katastrofální události. Pokud například někdo neúmyslně překonfigurujete hello název dns pro službu hello nebo odstraní přímý. Jako příklad předpokládejme měl stavové služby Service Fabric a někdo omylem odstranit tuto službu. Pokud se některé další omezení rizik, služby a všechny hello stavu, jako je nyní zmizel. Tyto typy provozu havárie ("bohužel") vyžadují různé způsoby zmírnění rizik a kroky pro obnovení než regulární neplánované výpadky. 

Hello doporučené způsoby tooavoid mají tyto typy provozu chyb
1. omezit přístup provozní toohello prostředí
2. výhradně nebezpečné operace auditu
3. použít automatizace, zabránit ruční nebo mimo pásmo změny a ověřit konkrétní změny před prostředí skutečného hello před jejich přijetí
4. Zkontrolujte, zda jsou destruktivní operace "soft". Logicky operations není se projeví okamžitě nebo může být v rámci některé časové okno vrátit zpět

Service Fabric nabízí některé mechanismy tooprevent provozní chyb, jako je například poskytuje [na základě rolí](service-fabric-cluster-security-roles.md) přístup k ovládacímu prvku pro operace clusteru. Většina těchto provozní chyb však vyžadují organizační úsilí a dalšími systémy. Service Fabric nabízejí některé mechanismus zbývajících provozní chyb, zejména zálohování a obnovení pro stavové služby.

## <a name="managing-failures"></a>Správa chyb
cílem Hello Service Fabric je téměř vždy Automatická správa chyb. Nicméně, v pořadí toohandle některé typy chyb, musí mít další kód. Jiné typy chyb by _není_ automaticky řešit kvůli důvodům, zabezpečení a obchodní kontinuity. 

### <a name="handling-single-failures"></a>Jeden selhání zpracování
Jednoho počítače může selhat pro nejrůznějším důvodů. Některé z nich se příčiny hardwaru, jako je napájení a sítě selhání hardwaru. Jiné chyby jsou v softwaru. Mezi ně patří selhání hello skutečné operačního systému a samotnou službu hello. Service Fabric automaticky rozpozná tyto typy chyb, včetně případů, kdy hello počítač stal izolované od ostatních počítačů z důvodu toonetwork problémy.

Bez ohledu na typ hello služby spuštěna jedna instance výsledky výpadek pro tuto službu Pokud z nějakého důvodu selže této jednu kopii hello kódu. 

V pořadí toohandle jediné chyby, hello nejjednodušší, musíte je tooensure, které jsou vaše služby spuštěné na více než jeden uzel ve výchozím nastavení. Pro bezstavové služby, můžete to provést tak, že `InstanceCount` větší než 1. Pro stavové služby, je vždy hello minimální doporučení `TargetReplicaSetSize` a `MinReplicaSetSize` aspoň 3. Spuštění více kopií kódu služby zajišťuje, že vaše služba dokáže zpracovat žádné jediné chyby automaticky. 

### <a name="handling-coordinated-failures"></a>Koordinované zpracování chyb
Koordinované selhání může dojít v clusteru s podporou kvůli tooeither plánované nebo selhání neplánované infrastruktury a změny nebo změny plánované softwaru. Service Fabric modely infrastruktury zón, které dojde k selhání koordinované jako domén selhání. Oblasti, které budou mít změny koordinované softwaru jsou modelovat jako upgradu domény. Další informace o doménách selhání a upgradu najdete v [tento dokument](service-fabric-cluster-resource-manager-cluster-description.md) definice a topologie clusteru, který popisuje.

Ve výchozím nastavení považuje Service Fabric doménách selhání a upgradu, při plánování, které by měly běžet vaše služby. Ve výchozím nastavení Service Fabric pokusí tooensure, která vašim službám spustit mezi několik domén selhání a upgradu, pokud změny plánovaná nebo neplánovaná vašim službám zůstanou dostupné. 

Předpokládejme například že selhání zdroje napájení současně způsobí stojany toofail počítače. S více kopií hello službu hello ztrátě velký počet počítačů v doméně selhání selhání se změní jenom další příklad jediné chyby pro danou službu. Z tohoto důvodu Správa domén selhání je důležité tooensuring vysokou dostupnost vašich služeb. Při spuštění v Azure Service Fabric, jsou automaticky spravované domény selhání. V jiných prostředích nemusí být. Pokud vytváříte vlastní clustery místně, že toomap a plánování rozložení domény selhání správně.

Domén upgradu jsou užitečné pro modelování oblasti, kde softwaru bude upgradován toobe v hello stejný čas. Z toho důvodu upgradu domény definovat také často hello hranice, kde je softwaru během plánované upgrady ukončena. Upgrady Service Fabric a vaše služby postupujte podle hello stejného modelu. Další informace o vrácení upgrady, domén upgradu a hello Service Fabric stavu modelu, která pomáhá zabránit neúmyslnému změny v hello clusteru a služby, které mají vliv najdete v těchto dokumentů:

 - [Upgrade aplikace](service-fabric-application-upgrade.md)
 - [Kurz upgradu aplikace](service-fabric-application-upgrade-tutorial.md)
 - [Model stavu prostředků infrastruktury služby](service-fabric-health-introduction.md)

Můžete vizualizovat hello rozložení pomocí mapy hello clusteru v clusteru [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md):

<center>
![Uzly rozloženy domén selhání v Service Fabric Exploreru][sfx-cluster-map]
</center>

> [!NOTE]
> Modelování oblasti nezdaří, vrácení upgrady, spuštěním mnoho instancí kódu služby a stav, tooensure pravidla umístění vaší služby spusťte napříč doménami selhání a upgradu a sledování stavu předdefinované jsou právě **některé** z hello Funkce, které poskytuje Service Fabric v pořadí tookeep normální provozní problémy a chyby z měnící do havárie. 
>

### <a name="handling-simultaneous-hardware-or-software-failures"></a>Zpracování souběžných selhání hardwaru nebo softwaru
Vyšší už jsme mluvili o jeden selhání. Jak vidíte, jsou právě udržováním více kopií hello kód (a stavu) spuštěná v doménách selhání a upgradu snadno toohandle pro bezstavové a stavové služby. Více souběžných náhodné chyby může také dojít. Jedná se o pravděpodobnější toolead tooan skutečné po havárii.


### <a name="random-failures-leading-tooservice-failures"></a>Náhodné chyby úvodní tooservice selhání
Řekněme, že služba hello provedla `InstanceCount` 5 a několik uzlů, které jsou spuštěné tyto instance všechny chybné v hello stejný čas. Service Fabric reaguje automaticky vytváření instancí nahrazení na jiných uzlech. Bude pokračovat, dokud hello služby je zpět tooits požadovaného počtu instancí vytváření nahrazení. Například Řekněme došlo bezstavové služby s `InstanceCount`-1, což znamená, běží na všechny platné uzly v clusteru hello. Řekněme, že některé z těchto instancí byly toofail. V takovém případě Service Fabric oznámení, že není v jeho požadovaný stav hello služby a pokusí toocreate hello instance na uzlech hello tam, kde jsou chybějící. 

Pro stavové služby hello situaci závisí na jestli hello služby obsahuje trvalé stavu, nebo ne. Také závisí na tom, kolik služba hello repliky provedla a kolik se nezdařilo. Určení, zda došlo k havárii pro stavové služby a správu odpovídá tří fází:

1. Určení, zda existuje došlo ke ztrátě kvora nebo ne
 - Ztrátě kvora je vždy, když jsou většinou hello repliky stavové služby dolů na hello stejnou dobu, včetně hello primární.
2. Určení, zda ztrátě kvora hello je trvalé, či nikoliv
 - Většina hello čas selhání je přechodný. Procesy, jsou restartovány, uzly se restartují, virtuální počítače jsou relaunched, retušovat síťové oddíly. V některých případech ale selhání jsou trvalé. 
    - Pro služby bez trvalý stav, selhání kvorum nebo více replik výsledků _okamžitě_ ve ztrátě kvora trvalé. Zjistí-li Service Fabric ztrátě kvora v stavové služby dočasnou, pokračuje hned dataloss toostep 3 deklarováním (potenciální). Pokračováním toodataloss má smysl, protože Service Fabric ví, že neexistuje žádný bod v čeká na hello repliky toocome zpět, protože i v případě, že byly obnoveny by být prázdný.
    - Pro stavové služby trvalé způsobí selhání kvorum nebo více replik Service Fabric toostart čekání hello repliky toocome zpět a obnovit kvora. To vede k výpadku služeb pro libovolný _zapíše_ toohello vliv služby hello oddílu (nebo "sady replik"). Ale čtení může být stále možné s omezenou konzistence záruky. vzhledem k tomu, že budete pokračovat, je (potenciální) událost dataloss a představuje další rizika je nekonečno, Hello výchozí množství času, kterou Service Fabric čeká kvora toobe obnovit. Přepsání výchozího hello `QuorumLossWaitDuration` hodnotu je možné, ale není to doporučeno. Místo toho v tuto chvíli všechny třeba se toorestore hello dolů repliky. To vyžaduje přinesou hello uzlů, které jsou dolů zpět a zajistit, aby se znovu připojit hello jednotky, kde budou uloženy hello místní trvalý stav. Pokud ztrátě kvora hello je způsobena selhání procesu, Service Fabric automaticky pokusí toorecreate hello procesy a restartujte hello repliky v nich. Pokud se to nezdaří, Service Fabric hlásí stav chyby. Pokud tyto lze vyřešit pak hello repliky obvykle se vrátit. V některých případech ale hello repliky nelze převést zpět. Například jednotky hello všechny nezdařila nebo hello počítače fyzicky zničeno nějakým způsobem. V těchto případech máme teď ztráty trvalé kvora. toostop Service Fabric tootell čekání hello dolů zpět, repliky toocome Správce clusteru musíte určit, které oddíly, které služby jsou vliv a volání hello `Repair-ServiceFabricPartition -PartitionId` nebo ` System.Fabric.FabricClient.ClusterManagementClient.RecoverPartitionAsync(Guid partitionId)` rozhraní API.  Toto rozhraní API umožňuje zadání hello ID hello oddílu toomove mimo QuorumLoss a do potenciální dataloss.

> [!NOTE]
> Je _nikdy_ bezpečné toouse toto rozhraní API jinak než v cílové způsob, jak na konkrétní oddíly. 
>

3. Určení, zda došlo ke ztrátě skutečná data a obnovení ze zálohy
  - Když Service Fabric volá hello `OnDataLossAsync` metoda je vždy z důvodu _by mohly vzbuzovat podezření_ dataloss. Service Fabric zajistí, že toto volání se doručí toohello _nejlepší_ zbývající repliky. Toto je, podle toho, která repliky udělal hello většina průběh. Hello důvod vždy říkáme _by mohly vzbuzovat podezření_ dataloss je, že je možné, zbývající repliky hello ve skutečnosti má všechny stejného stavu jako v době hello primární snížila. Ale bez tohoto stavu toocompare ho, neexistuje žádný funkční způsob pro Service Fabric nebo operátory tooknow pro jistotu. V tomto okamžiku Service Fabric také zná hello další repliky nejsou vracející se zpět. Který byl hello rozhodnutí provedeny, když jsme zastavil čeká na hello tooresolve ztráty kvora, sám sebe. Hello nejlepším řešením pro službu hello je obvykle toofreeze a počkejte konkrétní zásahů správce. Jaké jsou proto Typická implementace hello `OnDataLossAsync` metoda udělat?
  - Nejprve protokolu, který `OnDataLossAsync` byla spuštěna a spusťte vypnout všechny potřebné pro správu výstrahy.
   - Obvykle v tomto okamžiku toopause a počkejte další rozhodnutí a toobe ručně prováděné akce provedena. Je to proto, že i když jsou k dispozici zálohování, které potřebují toobe připravený. Například pokud dva různé služby koordinaci informace, tyto zálohy může být nutné toobe upravit v pořadí tooensure, který je konzistentní po obnovení hello se stane, že informace hello tyto dvě služby zajímají. 
  - Často je zde také některé další telemetrií nebo výfukového ze služby hello. Tato metadata mohou být obsaženy v jiných služeb nebo v protokolech. Tato informace může být použité potřebné toodetermine, kdyby všechny volání přijme a zpracuje v hello primární, které se nacházejí na replice konkrétní zálohování nebo replikované toothis hello. To může být nutné toobe přehraná nebo je přidán toohello zálohování předtím, než je možné je obnovení.  
   - Porovnání hello zbývající toothat stavu repliky obsažené v všechny zálohy, které jsou k dispozici. Pokud pomocí hello Service Fabric spolehlivé kolekcí, pak jsou nástroje a zpracuje k dispozici pro to, které jsou popsané v [v tomto článku](service-fabric-reliable-services-backup-restore.md). cílem Hello je toosee Pokud stačí hello stavu v rámci hello repliky nebo jaké hello zálohování může být také chybí.
  - Jednou hello porovnání se provádí, a pokud potřebné hello obnovení dokončeno, hello kódu služby by měla vrátit hodnotu true, pokud nebyly provedeny žádné změny stavu. V případě repliky hello zjistíte, že byl hello nejlépe k dispozici kopii hello stavu a provedeny žádné změny, pak se vraťte hodnotu false. Pravda označuje, že jakékoli _jiných_ zbývající repliky může být nyní nekonzistentní tímto tématem. Bude být zrušen a znovu vytvořen z této repliky. NEPRAVDA označuje, že nebyly provedeny žádné změny stavu, takže hello další repliky můžete ponechat co mají. 

Je důležité, že autoři služby praxi potenciální dataloss a scénářích selhání, před služby jsou někdy nasazením v produkčním prostředí. tooprotect proti hello možnost dataloss, je důležité tooperiodically [zálohování stavu hello](service-fabric-reliable-services-backup-restore.md) všech geograficky redundantní úložiště tooa stavové služby. Musí také zkontrolujte, zda máte možnost toorestore hello ho. Vzhledem k tomu, že zálohy mnoho různých služeb se provádějí v různou dobu, musíte po obnovení mít vašim službám konzistentní zobrazení vzájemně tooensure. Představte si třeba situaci, kde jedna služba vygeneruje číslo a uloží ji pak odešle ji tooanother služba, která také ukládá ji. Po obnovení můžete zjistit hello druhý služba má hello číslo, ale hello nejprve neexistuje, protože jeho zálohování nezahrnuli tuto operaci.

Pokud zjistíte, že zbývající hello repliky jsou nedostatečné toocontinue z ve scénáři dataloss a nemůže rekonstrukci stav služby z telemetrie nebo výfukového, hello četnost záloh určí vaší nejlepší plánovaného bodu možné obnovení (RPO) . Service Fabric nabízí celou řadu nástrojů pro testování různých scénářích selhání, včetně trvalé kvora a dataloss, které vyžadují obnovení ze zálohy. Tyto scénáře jsou zahrnuty jako součást nástrojů testovatelnosti Service Fabric spravuje hello selhání Analysis Service. Další informace o těchto nástrojů a vzory je k dispozici [zde](service-fabric-testability-overview.md). 

> [!NOTE]
> Systémových služeb může také dojít k ztráty kvora, s hello vlivem konkrétní toohello služby nejistá. Pro instanci ztrátě kvora ve službě pojmenování hello ovlivňuje překlad IP adres, zatímco ztrátě kvora ve službě service manager převzetí služeb při selhání hello blokuje vytvoření nové služby a převzetí služeb při selhání. Při hello Service Fabric systémových služeb podle hello stejný vzor jako vaší služby pro správu stavu, není doporučeno toomove mají pokusit o je mimo ztrátě kvora a do potenciální dataloss. Hello doporučení se místo toho příliš[vyhledat podporu](service-fabric-support.md) toodetermine řešení, které je cílem tooyour konkrétní situaci.  Obvykle je vhodnější toosimply čekání do hello dolů návratový repliky.
>

## <a name="availability-of-hello-service-fabric-cluster"></a>Dostupnost clusteru Service Fabric hello
Obecně řečeno samotného clusteru Service Fabric hello je vysoce distribuovaném prostředí s žádný jediný bod selhání. Selhání jednoho libovolného uzlu nezpůsobí dostupnosti nebo problémy se spolehlivosti pro hello cluster primárně, protože hello Service Fabric systémových služeb podle dříve stejné pokyny uvedenými hello: budou vždy spustit s tři nebo více replik ve výchozím nastavení a ty služby systému, které jsou bezstavové spouštět na všech uzlech. Hello základní sítě Service Fabric a vrstvy detekce selhání jsou plně distribuován. Většina systémových služeb můžete znovu sestavit z metadat v clusteru hello nebo vědět, jak tooresynchronize jejich stavu z jiného místa. Hello dostupnost clusteru hello mohou způsobit ohrožení, pokud systémových služeb do kvora případů ztráty jako ty, které jsou popsané výše. V těchto případech nemusí být možné tooperform určité operace na clusteru hello jako spuštěním upgradu nebo nasazování nových služeb, ale samotný cluster hello je pořád zapnutý. Služeb v již byla spuštěna zůstane spuštěný v těchto podmínkách, pokud potřebují zápisy toohello systému služby toocontinue funguje. Například pokud hello Failover Manager je ve ztrátě kvora všechny služby bude pokračovat toorun, ale žádné služby, které nesplní nebude možné tooautomatically restartování, vzhledem k tomu, že to vyžaduje hello účast hello Správce převzetí služeb při selhání. 

### <a name="failures-of-a-datacenter-or-azure-region"></a>Selhání datacenter nebo oblast Azure
Ve výjimečných případech může stát fyzické datového centra není dočasně k dispozici z důvodu tooloss napájení nebo připojením k síti. V těchto případech clusterů Service Fabric a služeb na tomto datovém centru nebo v oblasti Azure k dispozici. Ale _vašich dat se zachová, i_. Pro clustery spuštěná v Azure, můžete zobrazit aktualizace na výpadky na hello [Azure stavové stránce][azure-status-dashboard]. V hello vysoce nepravděpodobné událost, která fyzické datového centra je částečně nebo zcela zničení všech clusterů Service Fabric hostované existuje nebo hello služby v nich by mohly být ztraceny. To zahrnuje všechny stavu nejsou zálohovány mimo dané datacenter nebo oblasti.

Není k dispozici dvě různé strategie pro zbývajících hello trvalé nebo dlouhodobě selhání jednoho datového centra nebo oblasti. 

1. Spusťte samostatných clusterů Service Fabric v několika takových oblastech a využívat některé mechanismus pro převzetí služeb při selhání a navrácení služeb mezi těchto prostředích. Tento druh více clusteru aktivní aktivní nebo aktivní – pasivní modelu vyžaduje další operace správy a kód. Také je vyžadována koordinaci záloh z hello služby v jednom datacenter nebo oblast, aby byly k dispozici v dalších datových center nebo oblastech, pokud jeden selže. 
2. Spusťte jeden cluster Service Fabric, která přesahuje více datových centrech nebo oblasti. minimální podporované konfigurace pro tento Hello je tři datových centrech nebo oblasti. Hello doporučený počet oblastí nebo datových centrech je pět. To vyžaduje složitější topologie clusteru. Hello výhodou tohoto modelu je však, že selhání jednoho datového centra nebo oblasti je převeden po havárii na normální selhání. Tyto chyby mohou být zpracována hello mechanismy, které fungují pro clustery v jedné oblasti. Domén selhání, domén upgradu a pravidla pro umístění Service Fabric Ujistěte se, že úlohy distribuují tak, aby se tolerovat selhání normální. Další informace o zásadách, které vám mohou pomoci provoz služby v tomto typu clusteru, najdete v tématu na [zásady umístění](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md)

### <a name="random-failures-leading-toocluster-failures"></a>Náhodné chyby úvodní toocluster selhání
Service Fabric má hello konceptu uzly počáteční hodnoty. Jedná se o uzly, které udržují hello dostupnost hello základní clusteru. Tyto uzly pomoct tooensure hello clusteru zůstane až tím zřízením zapůjčení s ostatními uzly a slouží jako tiebreakers během určité druhy chyb v síti. Pokud náhodné chyby odebrat Většina uzlů hello počáteční hodnoty v hello clusteru a jejich zpět nedostaly, hello cluster automaticky vypne. V Azure, jsou automaticky spravovány uzly počáteční hodnoty: jsou distribuovány prostřednictvím doménách selhání hello k dispozici a upgradu, pokud jeden počáteční uzel je odebrán z clusteru hello jiný bude vytvořena na příslušné místo. 

V samostatných clusterů Service Fabric a Azure je hello "Primární uzel typu" hello, ten, který spouští semen hello. Při definování typu primárního uzlu, Service Fabric se automaticky využít výhod hello počet uzlů zajištěna vytvořením uzly počáteční hodnoty too9 a 9 repliky jednotlivých hello systémových služeb. Pokud sadu náhodné chyby používá současně se většina těchto replik služby systému, zadá hello systémových služeb ztráty kvora, jsme popsaný výše. Pokud se Většina uzlů hello počáteční hodnoty jsou ztraceny, hello clusteru se zastaví krátce po.

## <a name="next-steps"></a>Další kroky
- Zjistěte, jak toosimulate různých selháních pomocí hello [testovatelnosti framework](service-fabric-testability-overview.md)
- Přečtěte si další prostředky pro zotavení po havárii a vysoká dostupnost. Microsoft publikuje velké množství pokyny v těchto tématech. Při některé z těchto dokumentů naleznete toospecific techniky pro použití v jiných produktech, obsahují mnoho obecné osvědčené postupy, které můžete použít v kontextu. hello Service Fabric také:
  - [Kontrolní seznam k dostupnosti](../best-practices-availability-checklist.md)
  - [Provádění postupu zotavení po havárii](../sql-database/sql-database-disaster-recovery-drills.md)
  - [Zotavení po havárii a vysoká dostupnost pro aplikace Azure][dr-ha-guide]
- Informace o [možnostech podpory pro Service Fabric](service-fabric-support.md)

<!-- External links -->

[repair-partition-ps]: https://msdn.microsoft.com/library/mt163522.aspx
[azure-status-dashboard]:https://azure.microsoft.com/status/
[azure-regions]: https://azure.microsoft.com/regions/
[dr-ha-guide]: https://msdn.microsoft.com/library/azure/dn251004.aspx


<!-- Images -->

[sfx-cluster-map]: ./media/service-fabric-disaster-recovery/sfx-clustermap.png
