---
title: "aaaService Fabric spolehlivé aktéři přehled | Microsoft Docs"
description: "Úvod toohello Service Fabric programovacího modelu Reliable Actors."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 7fdad07f-f2d6-4c74-804d-e0d56131f060
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: vturecek
ms.openlocfilehash: ab010cbf936c6cf723b3d453ef95a9bf51f76c95
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooservice-fabric-reliable-actors"></a>Úvod tooService Reliable Actors prostředků infrastruktury
Reliable Actors je architektura aplikace Service Fabric podle hello [virtuální objektu Actor](http://research.microsoft.com/en-us/projects/orleans/) vzor. Hello spolehlivé aktéři API poskytuje programovací model jednovláknové založený na škálovatelnost a spolehlivost záruky hello poskytované Service Fabric.

## <a name="what-are-actors"></a>Jaké jsou aktéři?
Objekt actor je s jednotkou izolované, nezávislé výpočetních operací a stavu s jedním podprocesem provádění. Hello [vzor objektu actor](https://en.wikipedia.org/wiki/Actor_model) je výpočetní model souběžných nebo distribuovaných systémů, ve které velké množství těchto aktéři mohou být prováděny současně a nezávisle na sobě navzájem. Aktéři mohou komunikovat navzájem a mohou vytvářet další aktéři.

### <a name="when-toouse-reliable-actors"></a>Když toouse Reliable Actors
Služba Fabric Reliable Actors je implementace tohoto vzoru návrhu objektu actor hello. Stejně jako u jakékoli vzoru návrhu softwaru hello rozhodnutí, jestli se provádí specifického vzoru toouse podle zda softwaru návrh problém vyhovuje vzoru hello.

I když vzoru návrhu objektu actor hello může být dobrým shody tooa počet distribuovaných systémů problémy a scénáře, pečlivě zvážit omezení hello vzor hello a hello framework implementace, které musí být provedeny. Jako obecné pokyny vezměte v úvahu hello objektu actor vzor toomodel problém nebo scénář pokud:

* Váš prostor problém zahrnuje velký počet (tisíc nebo více) malé, nezávislé a izolované jednotek stavu a logiku.
* Chcete toowork s jedním podprocesem objekty, které nevyžadují významné interakce z externí součásti, včetně dotaz na stav mezi sadu aktéři.
* Vaše instance objektu actor neblokuje volající s nepředvídatelným zpoždění vydáním vstupně-výstupních operací.

## <a name="actors-in-service-fabric"></a>Aktéři v Service Fabric
V Service Fabric, kteří se implementují ve hello Reliable Actors framework: na základě objektu actor vzor aplikační rozhraní založené na [spolehlivé služby Service Fabric](service-fabric-reliable-services-introduction.md). Každý spolehlivé objektu Actor službu, kterou píšete je ve skutečnosti oddílů, stavová spolehlivá služba.

Každý objektu actor je definován jako instance typu objektu actor, způsob identické toohello objekt .NET je instance typu .NET. Například může být typ objektu actor, který implementuje hello funkce kalkulačky a může být mnoho aktéři daného typu, které jsou rozmístěny v různých uzlech v clusteru. Každé takové objektu actor je jedinečně identifikovaný identifikátor objektu actor.

### <a name="actor-lifetime"></a>Doba života objektu actor
Service Fabric aktéři jsou virtuální, což znamená, že své životnosti není vázanou tootheir reprezentací v paměti. V důsledku toho nepotřebují toobe explicitně vytvořen nebo zničeno. modul runtime Reliable Actors Hello automaticky aktivuje objektu actor hello poprvé obdrží žádost pro ID tohoto objektu actor. Pokud objekt actor se nepoužívá pro určitou dobu, hello Reliable Actors runtime uvolňování paměti – shromažďuje hello objektů v paměti. Také zachová znalostní báze hello objektu actor existence měli toobe později znovu aktivovat. Další podrobnosti najdete v tématu [kolekce paměti a životního cyklu objektu Actor](service-fabric-reliable-actors-lifecycle.md).

Tato abstrakce doba života objektu actor virtuální představuje některé upozornění v důsledku hello virtuální objektu actor modelu a ve skutečnosti odchylují hello Reliable Actors implementace v některých případech z tohoto modelu.

* Objekt actor se automaticky aktivuje (což objektu actor objekt toobe sestavený) hello poprvé, je odeslána zpráva ID tooits objektu actor. Po nějaké časové období je objekt actor hello uvolnění z paměti. V hello budoucí, znovu pomocí ID objektu actor hello způsobí, že nového objektu actor toobe objekt vytvořený. Stav objektu actor outlives doba života objektu hello při uložené v hello správce stavu.
* Voláním jakékoli metody objektu actor pro ID objektu actor aktivuje tohoto objektu actor. Z tohoto důvodu objektu actor typy mají jejich Konstruktor volá implicitně hello runtime. Kód klienta proto nelze předat parametry toohello objektu actor konstruktoru typu, i když parametry mohou být předaná toohello objektu actor konstruktor samotnou službu hello. Hello výsledkem je, že aktéři může zkonstruovat ve stavu částečně inicializovat pomocí hello, když se nazývají jiné metody, pokud objektu actor hello vyžaduje inicializační parametry z klienta hello. Neexistuje jeden vstupní bod pro aktivaci hello objektu actor z klienta hello.
* I když Reliable Actors implicitně vytvořit objekty objektu actor; Máte tooexplicitly hello možnost odstranit objekt actor a její stav.

### <a name="distribution-and-failover"></a>Distribuce a převzetí služeb při selhání
tooprovide škálovatelnost a spolehlivost, Service Fabric distribuuje aktéři v rámci clusteru hello a automaticky migruje je z selhání uzlů toohealthy ty, které jsou podle potřeby. Toto je abstrakci přes [oddílů, stavová služba spolehlivé](service-fabric-concepts-partitioning.md). Distribuce, škálovatelnost, spolehlivost a automatické převzetí služeb při selhání jsou všechny poskytované na základě hello skutečnost, že aktéři běží ve stavové spolehlivá služba s názvem hello *služby objektu Actor*.

Aktéři jsou rozmístěny v hello oddíly hello služby objektu Actor a tyto oddíly jsou rozmístěny v hello uzlů v clusteru Service Fabric. Každý oddíl služby obsahuje sadu aktéři. Service Fabric spravuje distribuce a převzetí služeb při selhání oddílů služby hello.

Například služby objektu actor s devět oddíly nasadit toothree, které by thusly distribuována uzlů pomocí hello výchozí objektu actor oddílu umístění:

![Spolehlivé aktéři distribuce][2]

oddíl schématu a klíč nastavení rozsahu pro vás spravuje Hello objektu Actor Framework. Zjednodušuje některé možnosti, ale také představuje některé pozornost:

* Spolehlivé služby vám umožní toochoose schéma rozdělení oddílů, klíče rozsah (při použití rozsah dělení schéma) a počet oddílů. Reliable Actors je omezená toohello rozsah schéma rozdělení oddílů (hello uniform Int64 schéma) a vyžaduje, že používáte hello plný Int64 klíče rozsah.
* Ve výchozím nastavení se umístí do oddílů, které jsou výsledkem uniform distribuční náhodně aktéři.
* Protože aktéři jsou náhodně umístěn, je třeba očekávat, objektu actor operace budou vždy vyžadovat komunikaci sítě, včetně serializace a deserializace dat volání metody, by docházelo k latenci a zatížení.
* V pokročilých scénářích je možné toocontrol objektu actor oddílu umístění pomocí objektu actor Int64 ID, které mapují toospecific oddíly. Ale to tak může způsobit jako nevyváženou distribučního aktéři napříč oddíly.

Další informace o tom, jak jsou služby objektu actor do několika oddílů, najdete v části příliš[dělení koncepty pro aktéři](service-fabric-reliable-actors-platform.md#service-fabric-partition-concepts-for-actors).

### <a name="actor-communication"></a>Komunikace objektu actor
Interakce objektu actor jsou definovány v rozhraní, které sdílí objektu actor hello, který implementuje rozhraní hello a hello klienta, který získá proxy objektu actor tooan prostřednictvím hello stejné rozhraní. Protože toto rozhraní je tooinvoke použitých actor metody asynchronně, musí být každou metodu na rozhraní hello vrácení úloh.

Volání metod a jejich odpovědi nakonec za následek síťové požadavky napříč hello clusteru, takže hello argumentů a typy výsledků hello hello úloh, že vrací musejí být serializovatelná platformou hello. Konkrétně musí být [kontraktů dat serializovatelný](service-fabric-reliable-actors-notes-on-actor-type-serialization.md).

#### <a name="hello-actor-proxy"></a>Proxy Server Hello objektu actor
Hello Reliable Actors klientského rozhraní API poskytuje komunikaci mezi instanci objektu actor a objektu actor klienta. toocommunicate s objektu actor, klient vytvoří objekt objektu actor proxy serveru, který implementuje rozhraní objektu actor hello. Hello klient komunikuje s objektu actor hello volajícím metody u objektu proxy hello. proxy objektu actor Hello lze použít pro komunikaci klienta do objektu actor a objektu actor actor.

```csharp
// Create a randomly distributed actor ID
ActorId actorId = ActorId.CreateRandom();

// This only creates a proxy object, it does not activate an actor or invoke any methods yet.
IMyActor myActor = ActorProxy.Create<IMyActor>(actorId, new Uri("fabric:/MyApp/MyActorService"));

// This will invoke a method on hello actor. If an actor with hello given ID does not exist, it will be activated by this method call.
await myActor.DoWorkAsync();
```

```java
// Create actor ID with some name
ActorId actorId = new ActorId("Actor1");

// This only creates a proxy object, it does not activate an actor or invoke any methods yet.
MyActor myActor = ActorProxyBase.create(actorId, new URI("fabric:/MyApp/MyActorService"), MyActor.class);

// This will invoke a method on hello actor. If an actor with hello given ID does not exist, it will be activated by this method call.
myActor.DoWorkAsync().get();
```


Hello dva údaje použity toocreate hello objektu actor proxy – objekt jsou hello objekt actor s ID a název aplikace hello. ID objektu actor Hello jednoznačně identifikuje objektu actor hello, zatímco název aplikace hello identifikuje hello [aplikace Service Fabric](service-fabric-reliable-actors-platform.md#application-model) kde je nasazená objektu actor hello.

Hello `ActorProxy`(C#) nebo `ActorProxyBase`– třída (Java) na straně klienta hello provede hello nezbytné řešení toolocate objektu actor hello podle ID a otevřete komunikační kanál s ním. Opakuje také toolocate hello objektu actor v případech hello selhání komunikace a převzetí služeb při selhání. V důsledku toho doručování zpráv má hello následující vlastnosti:

* Doručení zpráv je nejlepší úsilí.
* Aktéři mohou se zobrazit duplicitní zprávy z hello stejného klienta.

### <a name="concurrency"></a>Souběžnost
Hello Reliable Actors runtime poskytuje jednoduché přístupu na základě zapněte model pro přístup k objektu actor metody. To znamená, že kdykoli může být aktivní uvnitř objekt actor kód více než jedno vlákno. Není nutné pro synchronizaci mechanismy pro přístup k datům přístupu na základě zapnout výrazně zjednodušuje souběžných systémy. Taky to znamená, že systémy musí být vytvořeny s zvláštní upozornění pro hello jednovláknové přístup povaze každá instance objektu actor.

* Více než jeden požadavek nelze zpracovat instance jednoho objektu actor v čase. Instance objektu actor může způsobit úzkým místem propustnost, pokud je očekávaný toohandle souběžných požadavků.
* Aktéři můžete zablokování na sobě navzájem, pokud je požadavek cyklické mezi dvěma aktéři při Přišla žádost o externí tooone hello aktéři současně. Hello objektu actor runtime bude automaticky času odhlašování v objektu actor volá a k vyvolání k výjimce toohello volající toointerrupt situacích možné zablokování.

![Spolehlivé aktéři komunikace][3]

#### <a name="turn-based-access"></a>Přístupu na základě zapnout
Zapněte se skládá z hello dokončení provádění metody objektu actor v žádosti o tooa odpovědi jiných klientů nebo aktéři nebo hello dokončení provádění [časovače nebo připomenutí](service-fabric-reliable-actors-timers-reminders.md) zpětného volání. I když jsou tyto metody a zpětná volání asynchronní, hello aktéři runtime není prokládání dat je. Předtím, než je povolený nový zapnout, musí být plně dokončení zapnout. Jinými slovy objektu actor metoda nebo časovače nebo připomenutí zpětné volání, které je aktuálně spuštěných musí být plně dokončení před nové metody tooa volání nebo zpětné volání je povolen. Metoda nebo zpětného volání považuje za toohave dokončení Pokud hello provádění vrátila z metody hello nebo dokončení zpětného volání a hello úlohy vrácený metodou hello nebo zpětného volání. Je vhodné zdůraznění, že na základě zapnout concurrency je dodržena i přes různé metody, časovače a zpětná volání.

Hello aktéři runtime vynucuje na základě zapnout souběžnosti získávání zámku na objektu actor na začátku hello zapnout a uvolněním hello zámku na konci hello hello vypnout. Na základě zapnout souběžnosti je proto vynucují na základě za objekt actor a není mezi aktéři. Metody objektu actor a zpětná volání časovače nebo připomenutí můžete spustit současně jménem různých aktéři.

Hello následující příklad znázorňuje hello výše koncepty. Vezměte v úvahu typ objektu actor, který implementuje dvě asynchronní metody (Řekněme, *Method1* a *Method2*), a časovač a připomenutí. Hello následující diagram ukazuje příklad časovou osu pro hello provádění těchto metod a zpětná volání jménem dvě aktéři (*ActorId1* a *ActorId2*), patří toothis objektu actor typu.

![Spolehlivé aktéři modul runtime na základě zapnout souběžnosti a přístup][1]

Tento diagram dodržovat tyto konvence:

* Každé svislé čáry zobrazuje hello logický tok provádění metody nebo zpětné volání jménem konkrétního objektu actor.
* Hello na každé svislé čáry označit k událostem v chronologickém pořadí s novější událostí pod starší.
* Různé barvy se používají pro odpovídající aktéři toodifferent časové osy.
* Zvýraznění je použité tooindicate hello duration, pro které hello je blokován zámek na objektu actor jménem metoda nebo zpětného volání.

Některé důležité body tooconsider:

* Při *Method1* provádí jménem *ActorId2* v odpovědi tooclient požadavku *xyz789*, další požadavek klienta (*abc123*) dorazí, který taky vyžaduje *Method1* toobe provedený *ActorId2*. Ale hello druhý provádění *Method1* nemá na začátku až do dokončení předchozí provádění hello. Podobně, zobrazí se připomenutí registrovaných *ActorId2* aktivuje se při *Method1* je spouštěna v odpovědi tooclient požadavku *xyz789*. Hello zpětného volání připomenutí se spustí až po obou spuštěních z *Method1* jsou dokončeny. Všechny tyto je z důvodu souběžnosti na základě tooturn vynucení pro *ActorId2*.
* Podobně se na základě zapnout souběžnosti také vynucuje pro *ActorId1*, jak je znázorněno pomocí hello provádění *Method1*, *Method2*, a hello zpětné volání časovače jménem *ActorId1* děje sériové způsobem.
* Provádění *Method1* jménem *ActorId1* se překrývá s jeho spuštění jménem *ActorId2*. Je to proto, že na základě zapnout concurrency se vynucuje jenom v rámci objektu actor a není napříč aktéři.
* V některých hello metoda/zpětného volání spuštěních hello `Task`(C#) nebo `CompletableFuture`(Java) vrácený hello metoda/zpětného volání dokončení po návratu metody hello. V některých jiných hello asynchronní operace již byla dokončena hello doby, vrátí hodnotu hello metoda/zpětného volání. V obou případech zámku na objektu actor hello vydání až po obě metody nebo zpětné hello volání vrátí, dokončení asynchronní operace hello.

#### <a name="reentrancy"></a>Vícenásobný přístup
Hello aktéři runtime umožňuje vícenásobný přístup ve výchozím nastavení. To znamená, že pokud metoda objektu actor *objektu Actor A* volá metodu na *objektu Actor B*, která volá jinou metodu na *objektu Actor A*, že metoda může toorun. Důvodem je, že je součástí hello stejné logické řetězce volání kontextu. Všechna volání časovače a připomenutí začínat hello nové logické volání kontextu. V tématu hello [vícenásobný přístup Reliable Actors](service-fabric-reliable-actors-reentrancy.md) další podrobnosti.

#### <a name="scope-of-concurrency-guarantees"></a>Rozsah záruky souběžnosti
modul runtime aktéři Hello poskytuje tyto záruky souběžnosti v situacích, kde se řídí hello volání z těchto metod. Například poskytuje tyto záruky pro hello volání metod, které se provádějí v požadavku klienta tooa odpověď, a také pro zpětné volání časovače a připomenutí. Ale pokud hello objektu actor kód přímo vyvolá tyto metody mimo hello mechanismus hello aktéři runtime, hello runtime nelze zadejte žádné záruky souběžnosti. Například pokud hello metoda je volána v kontextu hello některé úlohy, který není spojen s hello úloh vrácených metody objektu actor hello, hello runtime nelze zadejte souběžnosti záruky. Pokud hello metoda je volána z vlákna tohoto objektu actor hello vytvoří sama o sobě, pak hello runtime také neposkytuje souběžnosti záruky. Proto měli používat aktéři tooperform operace na pozadí, [objektu actor časovače a upomínek objektu actor](service-fabric-reliable-actors-timers-reminders.md) , respektují na základě zapnout souběžnosti.

## <a name="next-steps"></a>Další kroky
* Začínáme se ve vaší první službě Reliable Actors:
   * [Začínáme s Reliable Actors na rozhraní .NET](service-fabric-reliable-actors-get-started.md)
   * [Začínáme s Reliable Actors v jazyce Java](service-fabric-reliable-actors-get-started-java.md)

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-introduction/concurrency.png
[2]: ./media/service-fabric-reliable-actors-introduction/distribution.png
[3]: ./media/service-fabric-reliable-actors-introduction/actor-communication.png
