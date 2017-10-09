---
title: "aaaOverview hello Service Fabric spolehlivé Service programovací model | Microsoft Docs"
description: "Další informace o programovací model spolehlivá služba Service Fabric a začněte psát vlastní služby."
services: Service-Fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: vturecek; mani-ramaswamy
ms.assetid: 0c88a533-73f8-4ae1-a939-67d17456ac06
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: masnider;
ms.openlocfilehash: 41d1826df902b1f1845c4702bf2567e6b9ca1f1f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="reliable-services-overview"></a>Přehled Reliable Services
Azure Service Fabric zjednodušuje zápis a správu bezstavové a stavové spolehlivé služby. Toto téma obsahuje:

* Spolehlivé služby programovací model Hello pro bezstavové a stavové služby.
* Volby Hello máte toomake při zápisu spolehlivě.
* Některé scénáře a příklady toouse spolehlivé služby a jak jsou napsány.

Spolehlivé služby je jedním z hello programovací modely, které jsou k dispozici v Service Fabric. Hello jiných je hello spolehlivé objektu Actor programovací model, který poskytuje virtuální programovací model objektu Actor hello spolehlivé služby model. Další informace o programovacího modelu Reliable Actors hello najdete v tématu [tooService Úvod Fabric Reliable Actors](service-fabric-reliable-actors-introduction.md).

Service Fabric spravuje hello životnost služeb od zřízení a nasazení prostřednictvím aktualizace a odstranění, prostřednictvím [Správa aplikací Service Fabric](service-fabric-deploy-remove-applications.md).

## <a name="what-are-reliable-services"></a>Jaké jsou spolehlivé služby?
Spolehlivé služby poskytuje jednoduché, výkonné, nejvyšší úrovně programovací model toohelp express co je důležité tooyour aplikace. Spolehlivé služby hello programovací model získáte:

* Ostatní toohello přístup hello Service Fabric programovací rozhraní API. Na rozdíl od služby Service Fabric modelován jako [spustitelné soubory hosta](service-fabric-deploy-existing-app.md), spolehlivé služby získat přímo toouse hello zbytek hello rozhraní API služby prostředků infrastruktury. To umožňuje službám:
  * Dotaz hello systému
  * Sestava stavu o entitách v clusteru hello
  * přijímání oznámení o změnách konfigurace a kódu
  * Najít a komunikovat s jinými službami
  * (volitelně) použít hello [spolehlivé kolekce](service-fabric-reliable-services-reliable-collections.md)
  * .. a poskytnete jim přístup k toomany další funkce, všechny z první třídy programovací model v několika programovací jazyky.
* Jednoduchý model pro spuštění vlastního kódu, který vypadá jako programovací modely, které se používají k. Váš kód má jasně definované vstupní bod a jednoduše spravované životního cyklu.
* Modulární komunikační model. Používají hello podle vaší volby, jako je například HTTP s [webového rozhraní API](service-fabric-reliable-services-communication-webapi.md), Websocket, vlastní protokoly TCP, nebo cokoliv jiného. Spolehlivé služby poskytují některé skvělé možnosti se na pole můžete použít, nebo můžete zadat vlastní.
* Pro stavové služby, hello Reliable Services programovací model, vám umožní tooconsistently a spolehlivě ukládat vašemu stavu právo uvnitř vaší služby pomocí [spolehlivé kolekce](service-fabric-reliable-services-reliable-collections.md). Spolehlivé kolekce jsou jednoduchou sadou vysoce dostupné a spolehlivého kolekce tříd, které bude známé tooanyone, který byl použit C# kolekce. Tradičně služby pro správu spolehlivé stavu potřeba externími systémy. Ke kolekcím, spolehlivé, můžete uložit stav další tooyour výpočetní s hello stejnou vysokou dostupnost a spolehlivost, vám přijde tooexpect z externí vysoce dostupné úložiště. Tento model taky zlepšuje latence, protože jsou společné umísťování hello výpočty a stavu, je nutné toofunction.

Podívejte se na toto video Microsoft Virtual Academy přehled spolehlivé služby:<center>
<a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=HhD9566yC_4106218965">
<img src="./media/service-fabric-reliable-services-introduction/ReliableServicesVid.png" WIDTH="360" HEIGHT="244" />
</a>
</center>

## <a name="what-makes-reliable-services-different"></a>Díky spolehlivé služby různých?
Spolehlivé služby v Service Fabric se liší od služby, které může jste napsali před. Service Fabric nabízí spolehlivost, dostupnost, konzistence a škálovatelnost.

* **Spolehlivost** – vaše služba zůstane nahoru i v nespolehlivé prostředích, kde vaše počítače selhat nebo stiskněte tlačítko problémy se sítí, nebo v případech, kde hello služeb sami dojde k chybám a havárií nebo selhání. Pro stavové služby vašemu stavu se zachová i hello přítomnost sítě nebo jiné chyby.
* **Dostupnost** – vaše služba je dostupná a dobře reagovaly. Service Fabric spravuje vaše požadovaný počet spuštěná kopie.
* **Škálovatelnost** – služby, které jsou odpojené od konkrétní hardware, a můžou růst nebo zmenšení podle potřeby prostřednictvím hello přidání nebo odebrání hardwaru nebo jiným prostředkům. Jsou tooensure snadno oddílů (zejména v případě stavová hello), který dokáže vyhovět hello služby a popisovač služby částečné selhání. Služby lze vytvořit a odstranit dynamicky prostřednictvím kódu, povolení více instancí toobe spuštěné podle potřeby můžete v odpovědi toocustomer požadavky. Nakonec Service Fabric podporuje lightweight toobe služby. Service Fabric umožňuje tisíce služby toobe zřídit v jednom procesu, místo nutnosti nebo vyhradit celý instancí operačního systému nebo procesy tooa jednu instanci služby.
* **Konzistence** -jakékoli informace uložené v rámci této služby může zaručit toobe konzistentní. To platí i napříč více spolehlivé kolekcí v rámci služby. Změny mezi kolekcemi v rámci služby mohou být provedeny transakčně atomic způsobem.

## <a name="service-lifecycle"></a>Životní cyklus služby
Jestli stateful nebo bezstavové služby se poskytují spolehlivé služby jednoduché životního cyklu, které vám umožní rychle zařaďte kódu a začít pracovat.  Existují jen jednu nebo dvě metody, je třeba tooimplement tooget služby fungovaly.

* **CreateServiceReplicaListeners/CreateServiceInstanceListeners** -tato metoda je, kde hello služby definuje stack(s) komunikace hello že požaduje toouse. Hello komunikačního balíku, jako například [webového rozhraní API](service-fabric-reliable-services-communication-webapi.md), je co definuje hello naslouchání koncového bodu nebo koncové body pro hello služby (jak klienti moci připojit hello služby). Také definuje, jak hello zprávy, které komunikují s hello zbytek kódu služby hello.
* **RunAsync** -tato metoda je, kde vaše služba spustí své obchodní logiky a kde by nové úlohy na pozadí, které měly být spuštěny dobu jeho existence hello hello služby. token zrušení Hello, který je k dispozici je signál pro Pokud by se měla zastavit svou práci. Například pokud hello služba potřebuje toopull zprávy z fronty spolehlivé a jejich zpracování, toto je, kde se stane svou práci.

Pokud seznamování o službách reliable services pro hello poprvé, přečtěte si na! Pokud hledáte podrobný návod životního cyklu hello spolehlivé služby, můžete přejděte přes příliš[v tomto článku](service-fabric-reliable-services-lifecycle.md).

## <a name="example-services"></a>Příklad služby
Znalost Tento programovací model, podívejme rychlý přehled toosee dva různé služby, jak tyto součásti zapadají.

### <a name="stateless-reliable-services"></a>Bezstavové spolehlivé služby
Bezstavové služby je jeden, kdy není žádný stav uchovávaných v rámci hello služby v rámci volání. Žádný stav, který je k dispozici je zcela na jedno použití a nevyžaduje synchronizace replikace, trvalost nebo vysokou dostupnost.

Představte si třeba kalkulačky, která má nedostatek paměti a přijímá všechny podmínky a operace tooperform najednou.

V takovém případě hello `RunAsync()` (C#) nebo `runAsync()` (Java) z hello služby nesmí být prázdné, protože neexistuje žádný pozadí úloh zpracování služby hello musí toodo. Při vytváření služby kalkulačky hello vrátí `ICommunicationListener` (C#) nebo `CommunicationListener` (Java) (například [webového rozhraní API](service-fabric-reliable-services-communication-webapi.md)), otevře se koncový bod naslouchání na některé portu. Tento koncový bod naslouchání zachytí až toohello různé metody výpočtu (Příklad: "Přidat (n1, n2)"), zadejte veřejné rozhraní API kalkulačky hello.

Při volání z klienta hello odpovídající metoda je volána a službu kalkulačky hello provádí operace hello na hello zadaná data a vrátí výsledek hello. Jakýkoli stav nejsou uložena.

Neukládejte žádný vnitřní stav je tento příklad kalkulačky snadné. Ale většina služeb nejsou skutečně bezstavové. Místo toho budou využívajícím jejich stavu toosome jiného úložiště. (Například webové aplikace, které jsou závislé na zachování stavu relace v úložišti zálohování nebo mezipaměti není bezstavové.)

Běžným příkladem jak bezstavové služby se používají v Service Fabric je jako front-end, že zpřístupňuje hello veřejné rozhraní API pro webovou aplikaci. front-endovou službu Hello potom komunikuje toostateful služby toocomplete na žádost uživatele. V takovém případě jsou volání od klientů směrovanou tooa známé port, jako třeba 80, kde bezstavové služby hello naslouchá. Tato bezstavové služby přijímá volání hello a určuje, zda text hello volání je z důvěryhodné strany a ji obsluhovat je určené pro.  Bezstavové služby hello pak předá hello volání toohello správné oddílu hello stavové služby a čeká na odpověď. Když bezstavové služby hello obdrží odpověď, reaguje toohello původní klienta. Příkladem takové služby je v naše ukázky [C#](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started) / [Java](https://github.com/Azure-Samples/service-fabric-java-getting-started/tree/master/Actors/VisualObjectActor/VisualObjectWebService). Toto je pouze jedním z příkladů tohoto vzoru v hello ukázky, existují i další v jiných ukázky také.

### <a name="stateful-reliable-services"></a>Stavová spolehlivé služby
Stavové služby je ten, který musí mít některé část stavu zachovány konzistentní a v pořadí pro službu toofunction hello. Zvažte službu, která neustále vypočítá kumulativní průměr některá z hodnot na základě aktualizací, které obdrží. toodo, musí mít hello aktuální sadu příchozí požadavky, je nutné tooprocess a hello aktuální průměr. Jakoukoli službu, která načte, procesy a ukládá informace v externím obchodu (například Azure blob nebo table úložišti dnes) je stavový. V úložišti externí stavu hello právě udržuje její stav.

Většina služeb dnes uložit externě, jejich stav, kdy hello externí úložiště je pro tento stav co nabízí spolehlivost, dostupnost, škálovatelnost a konzistence. V Service Fabric služeb nejsou požadované toostore jejich stavu externě. Tyto požadavky pro hello kódu služby a stav služby hello postará Service Fabric.

> [!NOTE]
> Podpora pro stavové služby Reliable Services není k dispozici v systému Linux ještě (pro C# nebo Java).
>

Řekněme, že chceme toowrite služba, která zpracovává bitové kopie. toodo to hello služby trvá v bitové kopie a hello řadu převody tooperform na této bitové kopie. Tato služba vrátí naslouchací proces komunikace (umožňuje Předpokládejme, že je WebAPI), jako například zpřístupňuje rozhraní API `ConvertImage(Image i, IList<Conversion> conversions)`. Pokud obdrží požadavek, služba hello uloží jej do `IReliableQueue`a vrátí některé id toohello klienta, takže ho můžete sledovat žádosti o hello.

V rámci této služby `RunAsync()` může být složitější. Služba Hello obsahuje smyčku v jeho `RunAsync()` , vrátí požadavky mimo `IReliableQueue` a provádí převody hello požadovaný. získat Hello výsledky uložené v `IReliableDictionary` tak, aby při hello klienta zpátky mohou získat jejich převedený bitové kopie. není to i v případě, že něco nepovede hello image tooensure ztraceny, tato spolehlivá služba by mimo hello fronty pro vyžádání obsahu, proveďte hello převody a uložit výsledek hello vše v rámci jedné transakce. V takovém případě odebrání hello zprávy z fronty hello a výsledky hello jsou uloženy ve slovníku výsledek hello pouze po dokončení převody hello. Alternativně může hello služba hello image mimo hello fronty pro vyžádání obsahu a okamžitě ji uložit do vzdáleného úložiště. Tím se snižuje hello množství stavová služba hello má toomanage, ale zvyšuje složitost vzhledem k tomu, že má služba hello tookeep hello nezbytné metadata toomanage hello vzdálené úložiště. S buď přístup Pokud něco se nepovedlo v zůstane hello střední hello žádosti v toobe čekací fronty hello zpracovat.

Jednu věc toonote o této službě je, že vypadá podobně jako normální služby rozhraní .NET! Hello jediným rozdílem je, že hello datových struktur používá (`IReliableQueue` a `IReliableDictionary`) jsou poskytovány Service Fabric a jsou vysoce spolehlivé, k dispozici a konzistentní.

## <a name="when-toouse-reliable-services-apis"></a>Když toouse spolehlivé rozhraní API služby
Pokud některé z následujících hello charakterizovat služby potřebám vaší aplikace, měli byste zvážit spolehlivé rozhraní API služby:

* Má vaše služba kód (a volitelně stavu) toobe vysoce dostupné a spolehlivé
* Je nutné transakční záruky přes víc jednotek služby stavu (například objednávek a položek řádku pořadí).
* Stav aplikace můžete přirozeně modelován jako slovník spolehlivé a fronty.
* Kód aplikace nebo stavu musí toobe vysokou dostupností s nízkou latencí čtení a zápisy.
* Aplikace musí toocontrol hello souběžnosti nebo členitost zpracovaných operací v jedné nebo více kolekcí spolehlivé.
* Chcete toomanage hello komunikaci nebo ovládací prvek hello dělení schéma pro vaši službu.
* Váš kód potřebuje podprocesy běhového prostředí.
* Aplikace musí toodynamically ani nezničí spolehlivé slovník nebo fronty nebo celý služby za běhu.
* Budete potřebovat tooprogrammatically řízení zadaný Service Fabric zálohování a obnovení funkce pro služby stavu.
* Aplikace musí toomaintain změnu historie pro jeho jednotky stavu.
* Chcete toodevelop nebo využívat zprostředkovatelé třetí strany vyvinuté, vlastní stavu.

## <a name="next-steps"></a>Další kroky
* [Spolehlivé služby rychlý start](service-fabric-reliable-services-quick-start.md)
* [Spolehlivé služby advanced využití](service-fabric-reliable-services-advanced-usage.md)
* [Hello programovacího modelu Reliable Actors](service-fabric-reliable-actors-introduction.md)
