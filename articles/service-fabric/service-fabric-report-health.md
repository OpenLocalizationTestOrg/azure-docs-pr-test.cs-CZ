---
title: "sestavy stavu Service Fabric vlastní aaaAdd | Microsoft Docs"
description: "Popisuje, jak toosend vlastní stavu sestavy tooAzure entity stavu Service Fabric. Poskytuje doporučení pro navrhování a implementace sestav stavu kvality."
services: service-fabric
documentationcenter: .net
author: oanapl
manager: timlt
editor: 
ms.assetid: 0a00a7d2-510e-47d0-8aa8-24c851ea847f
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/19/2017
ms.author: oanapl
ms.openlocfilehash: 12c9f664e2a457b4e1e8f340873ca60ebcefb097
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-custom-service-fabric-health-reports"></a>Přidání vlastních stavových sestav Service Fabric
Představuje Azure Service Fabric [stavu modelu](service-fabric-health-introduction.md) určené tooflag není v pořádku, cluster a aplikace podmínek na konkrétní entity. model stavu Hello používá **stavu reporters** (součásti systému a watchdogs). cílem Hello je rychlé a snadné diagnostiky a opravy. Služba zapisovače potřebovat toothink předem o stavu. Všechny podmínku, která může mít vliv na stav by měl být zaznamenány na, zejména v případě, že může pomoci zavřete toohello kořenové problémy příznak. informace o stavu Hello můžete ušetřit čas a úsilí na ladění a šetření. Hello užitečnost je obzvláště zrušte po hello služba je spuštěná ve velkém měřítku v cloudu hello (privátní nebo Azure).

monitorování reporters Service Fabric Hello identifikovat podmínky, které vás zajímají. Sestavy budou tyto podmínky založené na jejich místní zobrazení. Hello [úložiště stavu](service-fabric-health-introduction.md#health-store) agreguje data o stavu odeslaných všechny toodetermine reporters jestli entity jsou globální v pořádku. Hello model je určený toobe bohaté, flexibilní a snadno toouse. Hello kvalitu sestav stavu hello Určuje přesnost hello zobrazení stavu hello hello clusteru. Falešně pozitivních zjištění, které se nesprávně zobrazí není v pořádku problémy může mít negativní vliv na upgrady nebo jiné služby, které používají data o stavu. Příkladem takové služby jsou opravy služeb a mechanismy pro výstrahy. Proto některé myšlenku je potřebné tooprovide sestavy, které zaznamenat podmínky zájem o hello co nejlepší možný způsob.

musí být toodesign a implementujte komponenty vytváření sestav, watchdogs a systému stavu:

* Definujte hello podmínku, kterou budou zajímat, hello způsobem, který je monitorován a hello vliv na funkčnost clusteru nebo aplikace hello. Na základě těchto informací, rozhodněte o hello sestavy stavu a vlastnost stavu.
* Určení hello [entity](service-fabric-health-introduction.md#health-entities-and-hierarchy) hello sestavy, které se vztahují k.
* Určení, kde se provádí hello reporting, uvnitř hello sledovací služba nebo z interní nebo externí zařízení.
* Definování zdroj použitý tooidentify hello ohlašování.
* Volba strategie vytváření sestav, pravidelně nebo na přechody. Hello doporučuje způsob, jak je pravidelně jako se vyžaduje kód jednodušší a méně náchylná tooerrors.
* Určení jak dlouho hello sestavy není v pořádku podmínky by měla zůstat v hello health store a způsob, jakým má být vymazána. Na základě těchto informací rozhodněte, sestava hello čas toolive a odebrat na vypršení platnosti chování.

Jak je uvedeno, vytváření sestav můžete provést z:

* Hello sledovat repliky služby Service Fabric.
* Interní watchdogs nasazené jako služba Service Fabric (například Service Fabric bezstavové služba, která monitoruje podmínky a vydá sestavy). Hello watchdogs může být nasazen všechny uzly nebo může být spřaženém toohello monitorované služby.
* Interní watchdogs, které spouštět na uzlech hello Service Fabric, ale jsou *není* implementovaný jako služby Service Fabric.
* Externí watchdogs prostředku hello testu z *mimo* hello cluster Service Fabric (například monitorování služby jako Gomez).

> [!NOTE]
> Předinstalované hello hello clusteru naplněný sestavy o stavu odeslaných hello komponent systému. Další informace v [stavu systému pomocí sestav pro řešení potíží s](service-fabric-understand-and-troubleshoot-with-system-health-reports.md). Hello uživatel sestavy, musí se poslat [entity stavu](service-fabric-health-introduction.md#health-entities-and-hierarchy) , již byly vytvořeny systémem hello.
> 
> 

Jednou hello návrh sestav stavu je zřejmé, sestavy stavu lze snadno odeslat. Můžete použít [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) tooreport stavu Pokud hello clusteru není [zabezpečené](service-fabric-cluster-security.md) nebo klienta fabric hello má oprávnění správce. Vytváření sestav můžete provést prostřednictvím hello rozhraní API pomocí pomocí [FabricClient.HealthManager.ReportHealth](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.reporthealth), pomocí prostředí PowerShell nebo prostřednictvím REST. Konfigurace knoflíky dávky sestavy pro zlepšení výkonu.

> [!NOTE]
> Sestava stavu je synchronní a představuje pouze pracovní hello ověření na straně klienta hello. Hello skutečnost, že hello sestavy je přijat hello stavu klienta nebo hello `Partition` nebo `CodePackageActivationContext` objekty neznamená, že se použije v úložišti hello. Je odeslán asynchronně a které by mohly mít zpracovat v dávce s dalšími sestavami. stále může selhat Hello zpracování na serveru hello: hello pořadové číslo může být zastaralé, hello entity, na které hello se musí použít sestava byla odstraněna, atd.
> 
> 

## <a name="health-client"></a>Stav klienta
sestavy stavu Hello odešlou toohello health store prostřednictvím stavu klienta, které je umístěn uvnitř klienta fabric hello. Hello stavu klienta se dá nakonfigurovat s hello následující nastavení:

* **HealthReportSendInterval**: hello zpoždění mezi hello čas hello sestavy je čas klienta a hello přidané toohello odesláním toohello health store. Použít toobatch sestavy do jedné zprávy, místo odeslání jednu zprávu pro každý sestavu. dávkování Hello zvyšuje výkon. Výchozí hodnota: 30 sekund.
* **HealthReportRetrySendInterval**: hello interval, ve které hello stavu klient znovu odešle Akumulovaná stavu sestavy toohello health store. Výchozí hodnota: 30 sekund.
* **HealthOperationTimeout**: hello časový limit pro sestavu zprávy odeslané toohello health store. Pokud zprávu časového limitu, hello stavu klienta se opakuje, dokud hello health store potvrdí, že byla zpracována hello sestavy. Výchozí hodnota: dvě minuty.

> [!NOTE]
> Pokud jsou sestavy hello zpracovat v dávce, hello klienta fabric musí být udržovány zachování připojení pro alespoň hello tooensure HealthReportSendInterval, která se odešlou. Pokud dojde ke ztrátě uvítací zprávu nebo hello stav úložiště nelze použít z důvodu chyby tootransient, hello klienta fabric musí být udržovány zachování delší toogive ho tooretry příležitosti.
> 
> 

Hello ukládání do vyrovnávací paměti na klientovi hello trvá hello jedinečnosti hello sestav v úvahu. Například pokud konkrétní chybný ohlašování vykazování 100 sestav za sekundu na hello stejné vlastnosti hello stejné entity, hello sestavy jsou nahrazeny hello poslední verze. Maximálně jeden takový sestavy existuje ve frontě hello klienta. Pokud dávkování nastaveno, hello počet sestav odeslané toohello stavu úložiště je pouze jeden intervalu odeslání. Tato sestava je hello poslední přidané zprávu, která odráží hello aktuálního stavu entity hello.
Zadejte parametry konfigurace při `FabricClient` se vytvoří pomocí předání [FabricClientSettings](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclientsettings) s hello požadované hodnoty pro stav související položky.

Hello následující příklad vytvoří klienta fabric a určuje, že by měly být odeslány hello sestav při přidání. V časových limitů a chyb, které můžete zkusit znovu opakování dojít každých 40 sekund.

```csharp
var clientSettings = new FabricClientSettings()
{
    HealthOperationTimeout = TimeSpan.FromSeconds(120),
    HealthReportSendInterval = TimeSpan.FromSeconds(0),
    HealthReportRetrySendInterval = TimeSpan.FromSeconds(40),
};
var fabricClient = new FabricClient(clientSettings);
```

Doporučujeme zachovat fabric hello výchozí nastavení klienta, která nastavit `HealthReportSendInterval` too30 sekund. Toto nastavení zajistí optimální výkon kvůli toobatching. Pro kritické sestavy, které musí být nejdříve poslat, použijte `HealthReportSendOptions` s Immediate `true` v [FabricClient.HealthClient.ReportHealth](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.reporthealth) rozhraní API. Okamžitou sestavy nepoužívat hello dávkování intervalu. Pomocí tohoto příznaku dát pozor; Chceme tootake výhod hello stavu klienta dávkování kdykoli je to možné. Okamžité odeslání je také užitečné při zavírání hello klienta fabric (například hello proces bylo zjištěno neplatné stavu a je třeba tooshut dolů tooprevent vedlejší účinky). Zajišťuje best effort odesílání hello nahromadění sestav. Při přidání jednu sestavu s příznakem okamžitou hello stavu klienta dávek všechny sestavy hello shromážděných od posledního odeslání.

Stejnými parametry lze zadat, pokud cluster tooa připojení je vytvořený pomocí prostředí PowerShell. Hello následující ukázka spuštění místní cluster tooa připojení:

```powershell
PS C:\> Connect-ServiceFabricCluster -HealthOperationTimeoutInSec 120 -HealthReportSendIntervalInSec 0 -HealthReportRetrySendIntervalInSec 40
True

ConnectionEndpoint   :
FabricClientSettings : {
                       ClientFriendlyName                   : PowerShell-1944858a-4c6d-465f-89c7-9021c12ac0bb
                       PartitionLocationCacheLimit          : 100000
                       PartitionLocationCacheBucketCount    : 1024
                       ServiceChangePollInterval            : 00:02:00
                       ConnectionInitializationTimeout      : 00:00:02
                       KeepAliveInterval                    : 00:00:20
                       HealthOperationTimeout               : 00:02:00
                       HealthReportSendInterval             : 00:00:00
                       HealthReportRetrySendInterval        : 00:00:40
                       NotificationGatewayConnectionTimeout : 00:00:00
                       NotificationCacheUpdateTimeout       : 00:00:00
                       }
GatewayInformation   : {
                       NodeAddress                          : localhost:19000
                       NodeId                               : 1880ec88a3187766a6da323399721f53
                       NodeInstanceId                       : 130729063464981219
                       NodeName                             : Node.1
                       }
```

Podobně tooAPI, sestavy lze zaslat pomocí `-Immediate` přepínač toobe odesílány okamžitě, bez ohledu na to hello `HealthReportSendInterval` hodnotu.

REST se odesílají sestavy hello brána toohello Service Fabric, která má klientem interní prostředků infrastruktury. Tohoto klienta je ve výchozím nastavení nakonfigurované toosend sestavy zpracovat v dávce každých 30 sekund. Hello batch interval, můžete změnit pomocí nastavení konfigurace clusteru hello `HttpGatewayHealthReportSendInterval` na `HttpGateway`. Jak je uvedeno, je lepší volbou toosend hello sestavy s `Immediate` hodnotu true. 

> [!NOTE]
> tooensure, který Neautorizováno služby nelze sestavy stavu proti hello entity v hello clusteru, nakonfigurovat hello serveru tooaccept požadavky jenom z zabezpečené klientů. Hello `FabricClient` použité pro generování sestav, musí mít povolené zabezpečení toobe možné toocommunicate s clusterem hello (například pomocí protokolu Kerberos nebo ověření certifikátu). Další informace o [clusteru zabezpečení](service-fabric-cluster-security.md).
> 
> 

## <a name="report-from-within-low-privilege-services"></a>Sestava z v rámci služby nízkou úrovní oprávnění
Pokud služby Service Fabric nemají clusteru toohello přístupu správce, můžete sestavu stavu na entity z aktuálního kontextu hello prostřednictvím `Partition` nebo `CodePackageActivationContext`.

* Pro bezstavové služby použijte [IStatelessServicePartition.ReportInstanceHealth](https://docs.microsoft.com/dotnet/api/system.fabric.istatelessservicepartition.reportinstancehealth) tooreport na aktuální instanci služby hello.
* Pro stavové služby, použijte [IStatefulServicePartition.ReportReplicaHealth](https://docs.microsoft.com/dotnet/api/system.fabric.istatefulservicepartition.reportreplicahealth) tooreport na aktuální repliky.
* Použití [IServicePartition.ReportPartitionHealth](https://docs.microsoft.com/dotnet/api/system.fabric.iservicepartition.reportpartitionhealth) tooreport u hello aktuální oddíl entity.
* Použití [CodePackageActivationContext.ReportApplicationHealth](https://docs.microsoft.com/dotnet/api/system.fabric.codepackageactivationcontext.reportapplicationhealth) tooreport v aktuální aplikaci.
* Použití [CodePackageActivationContext.ReportDeployedApplicationHealth](https://docs.microsoft.com/dotnet/api/system.fabric.codepackageactivationcontext.reportdeployedapplicationhealth) tooreport na hello aktuální aplikace nasazené na aktuálním uzlu hello.
* Použití [CodePackageActivationContext.ReportDeployedServicePackageHealth](https://docs.microsoft.com/dotnet/api/system.fabric.codepackageactivationcontext.reportdeployedservicepackagehealth) tooreport na balíček služby hello aplikace nasazené na aktuálním uzlu hello.

> [!NOTE]
> Interně hello `Partition` a hello `CodePackageActivationContext` uložení stavu klienta konfiguruje s výchozím nastavením. Jak je popsáno pro hello [stavu klienta](service-fabric-report-health.md#health-client), sestavy se dávkují a posílají časovače. objekty Hello by měl být zachovány zachování toohave sestavy hello toosend příležitosti.
> 
> 

Můžete zadat `HealthReportSendOptions` při odesílání sestavy prostřednictvím `Partition` a `CodePackageActivationContext` stavu rozhraní API. Pokud máte kritická sestavy, které musí být nejdříve poslat, použijte `HealthReportSendOptions` s Immediate `true`. Okamžitou sestavy nepoužívat hello dávkování interval hello interní stavu klienta. Jak je uvedeno nahoře, použijte tento příznak dát pozor; Chceme tootake výhod hello stavu klienta dávkování kdykoli je to možné.

## <a name="design-health-reporting"></a>Návrh, vytváření sestav stavu
Hello prvním krokem při generování sestav vysoce kvalitní je identifikace hello podmínky, které může mít vliv na stav hello hello služby. Všechny podmínku, která může pomoci příznak problémy v clusteru nebo hello služby při jeho spuštění--nebo i lepší, než se stane problém – můžete potenciálně uložit až miliardy dolarů. Hello výhody patří menší výpadek, méně noci čas strávený příčin a opravu problémů a vyšší spokojenost zákazníků.

Jakmile hello podmínky jsou určeny, zapisovače sledovací zařízení nutné toofigure out hello nejlepší způsob, jak toomonitor je rovnováha mezi režii a použitelnosti. Představte si třeba služba, která nemá komplexní výpočty, které používají některé dočasné soubory ve sdílené složce. Sledovací zařízení může sledovat tooensure hello sdílenou složku, že je k dispozici dostatek místa. Ho může čekat na oznámení změn v souboru nebo adresáře. Upozornění ho může sestavu, pokud předem prahová hodnota je dosaženo a zobrazovat chyby, pokud sdílenou složku hello je plný. Oprava systému na upozornění, může spustit čištění starší soubory ve sdílené složce hello. Při chybě může oprava systému přesunout hello služby repliky tooanother uzel. Všimněte si, jak jsou popsané podmínky stavy hello z hlediska stavu: hello stav hello podmínku, která lze považovat za v pořádku (ok) nebo není v pořádku (upozornění nebo chyby).

Jakmile hello monitorování podrobnosti jsou nastaveny, zapisovač sledovací zařízení musí toofigure na tom, jak tooimplement hello sledovací zařízení. Pokud podmínky hello lze určit v rámci služby hello, hello sledovací zařízení můžou být součástí samotné hello monitorované služby. Například můžete kódu služby hello zkontrolujte využití hello sdílenou složku a potom nahlásit pokaždé, když se pokusí o toowrite soubor. Hello výhodou tohoto přístupu je, že vytváření sestav je jednoduché. Musí být pozor tooprevent sledovací zařízení chyby z hello funkcí služby, které mají vliv.

Generování sestav z v rámci hello monitorované služby není vždy možnost. Sledovací zařízení v rámci služby hello nemusí být možné toodetect hello podmínky. Něj nemusí být hello logiku nebo data toomake hello rozhodnutí. Hello režii při sledování hello podmínek může být vysoká. podmínky Hello také může nesmí být konkrétní tooa služby, ale místo toho ovlivnit interakce mezi službami. Další možností je toohave watchdogs v clusteru hello jako samostatné procesy. Hello watchdogs monitorování hello podmínky a sestavy, aniž by to ovlivnilo hello hlavní služby žádným způsobem. Například může být tyto watchdogs implementovaný jako bezstavové služby v hello stejnou aplikaci, nasazené na všech uzlech nebo hello stejným uzlům jako služba hello.

V některých případech sledovací zařízení spuštěna v clusteru hello není možnost buď. Pokud hello monitorovat stav dostupnosti hello nebo funkce služby hello jako uživatele nevidíte, je nejlepší watchdogs hello toohave v hello stejné umístění jako klienti hello uživatele. Zde můžete otestovat hello operace v hello stejným uživatelům způsob, jak volat. Například můžete mít sledovací zařízení, který je umístěn mimo hello cluster, vydává žádosti o služby toohello a ověří hello latence a správnost hello výsledku. (Pro službu kalkulačky, například 2 + 2 nevrátí 4 v přiměřené době?)

Jakmile udrželi podrobnosti hello sledovací zařízení byste měli rozhodnout na ID zdroje, který jednoznačně identifikuje. Pokud více watchdogs hello stejného typu žijí v hello clusteru, se musí hlásit různých entit nebo, pokud budou hlásit hello stejné entity, použijte jiný zdroj ID nebo vlastnost. Tímto způsobem jejich sestavy mohou existovat vedle sebe. Vlastnost Hello sestava stavu hello měli zaznamenat hello monitorovat stav. (Například hello výše, může být vlastnost hello **ShareSize**.) Pokud více sestav použít toohello stejné podmínky hello vlastnost by měla obsahovat některé dynamické informace, které umožňuje toocoexist sestavy. Například pokud více sdílených složek potřebovat toobe monitorován, název vlastnosti hello může být **ShareSize sharename**.

> [!NOTE]
> Proveďte *není* použít hello úložiště tookeep informace o stavu. Pouze informace týkající se stavu musí uvádět jako stavu, jako tento informace ovlivňuje hello vyhodnocování stavu entity. úložiště stavu Hello nebyl navržen jako úložiště pro obecné účely. Používá tooaggregate logiku vyhodnocení stavu všechna data do stavu hello. Odesílání informací, že nesouvisejícími toohealth (jako je vytváření sestav stavu s stav OK) nebude mít vliv na hello Agregovat stav, ale může negativně ovlivnit výkon hello hello health store.
> 
> 

Dalším kritériem při rozhodování Hello je které tooreport entity na. Většina hello času, podmínku hello jasně idetifies hello entity. Zvolte hello entity s rozlišením nejlepší možný. Pokud stav má dopad na všechny repliky v oddílu, zprávu o hello oddílu, nikoli na hello služby. Existují případy rohu, kde další myšlenku je potřeba, když. Pokud podmínky hello ovlivňuje entity, jako je například repliku, ale hello desire je podmínka hello toohave příznakem déle než hello doba života repliky, pak by měl být ohlášeny hello oddílu. Jinak při odstranění repliky hello úložiště stavu hello vyčistí její sestavy. Sledovací zařízení zapisovače musí vzít v úvahu hello životnosti hello sestavy a hello entity. Musí být zrušte, pokud by měla být vyčištěna sestavy z úložiště (například při chybě oznámené entitu již nepoužívá.).

Podívejme se na příklad, který převádí společně hello body, které I popsané. Vezměte v úvahu, že aplikace Service Fabric se skládá z hlavní stavové služby trvalé a sekundární bezstavové služby nasazeni na všechny uzly (jeden typ sekundární služby pro každý typ úlohy). hlavní server Hello má zpracování fronty, který obsahuje příkazy toobe provedený sekundární repliky. sekundární databáze Hello hello příchozích žádostí a odesílání signálů back potvrzení. Jednu podmínku, která může sledovat je hello délka fronty hlavní zpracování hello. Pokud délka fronty hlavní hello dosáhne prahové hodnoty, je uvedená upozornění. upozornění Hello označuje, že sekundární repliky hello nemůže zpracovat hello zatížení. Pokud hello fronty dosáhne hello maximální délku a příkazy jsou vyřadit, zobrazí se chybová zpráva, jako hello služby nelze obnovit. Hello sestav lze u vlastnosti hello **QueueStatus**. sledovací zařízení Hello je umístěn uvnitř hello služby a se pravidelně odesílají na hello hlavní primární repliky. čas Hello toolive dvě minuty, a odesláním pravidelně každých 30 sekund. Pokud primární hello přestane fungovat, sestava hello je automaticky vyčistí z úložiště. Pokud replika služby hello je zapnutý, ale je zablokována nebo jiné problémy s hello sestavy vyprší v hello health store. V takovém případě je hello entity vyhodnocovány v chybě.

Jiná podmínka, která se dá sledovat je čas spuštění úlohy. Hello hlavní distribuuje úkoly toohello sekundárních založený na typu úloh hello. V závislosti na hello návrhu může dotazovat hello hlavní hello sekundární databáze pro stav úlohy. Ho může také počkejte sekundárních toosend back potvrzení signály Pokud se provádí. V druhém případě hello musí dát pozor toodetect situacích, kdy jsou ztraceny die sekundární repliky nebo zprávy. Jednou z možností je pro hlavní toosend hello toohello požadavku ping stejné sekundární, který odesílá zpět jeho stav. Pokud není žádná stav, hlavní hello bude považovat za selhání a provádí změny plánu úlohy hello. Toto chování předpokládá, že se úlohy hello idempotent.

Hello monitorovat stav lze přeložit jako varování, pokud úloha hello neprobíhá v určitou dobu (**t1**, například 10 minut). Pokud hello úloha není dokončena v čase (**t2**, například 20 minut), podmínka hello monitorovat lze přeložit jako chyba. Vytváření těchto sestav lze provést několika způsoby:

* hlavní primární repliky Hello hlásí pravidelně sám na sobě. Může mít jednu vlastnost pro všechny úlohy čekající na vyřízení ve frontě hello. Pokud alespoň jedna úloha trvá déle, hello zprávy o stavu u vlastnosti hello **PendingTasks** je upozornění nebo chyby, podle potřeby. Pokud neexistují žádné čekající úlohy nebo všech úloh spustil provádění, je hello zprávy o stavu v pořádku. Hello úlohy jsou trvalé. Pokud primární hello přestane fungovat, hello nově povýší primární můžete dál tooreport správně.
* Jiný proces sledovací zařízení (v cloudu hello nebo externí) kontroluje hello úlohy (z mimo, podle potřeby hello výsledek úlohy) toosee, pokud jejich dokončení. Pokud nerespektují hello prahové hodnoty, sestavy se odesílají na hlavní služby hello. Sestavy se také odesílají na každý úkol, který obsahuje identifikátor hello úlohy, jako je třeba **PendingTask + taskId**. Sestavy by měly být odeslány pouze na stavy není v pořádku. Nastavte čas toolive tooa několik minut a označit toobe hello sestavy odebrány při vypršení jejich platnosti tooensure čištění.
* Hello sekundární, který spouští úlohu nahlásí, když ho trvá déle než očekávané toorun ho. Oznámí na instanci služby hello u vlastnosti hello **PendingTasks**. Sestava Hello zjišťuje hello instance služby, který má potíže, ale neukládá hello situaci, kdy kterou hello instance. Hello sestavy jsou pak vyčistit. Ho může zprávu o sekundární službu hello. Pokud hello sekundární dokončí úlohu hello, sekundární instance hello vymaže hello sestavy z úložiště hello. Hello sestavu neukládá hello situaci, kdy dojde ke ztrátě uvítací zprávu potvrzení a hello úkol není dokončen z hlediska hlavního hello.

Ale hello reporting se provádí v případech hello popsané výše, hello sestavy při vyhodnocování stavu zaznamenání ve stavu aplikace.

## <a name="report-periodically-vs-on-transition"></a>Sestava pravidelně oproti na přechod
Pomocí vykazování modelu stavu hello watchdogs sestavy můžete odesílat pravidelně nebo na přechody. Hello doporučoval způsob pro sledovací zařízení vytváření sestav je pravidelně, protože hello kód je mnohem jednodušší a méně náchylná tooerrors. Hello watchdogs musí zajistit toobe stejně jednoduché jako možné tooavoid chyby, které aktivují nesprávné sestavy. Nesprávný *není v pořádku* sestavy vliv vyhodnocení stavu a scénáře podle stavu, včetně upgrady. Nesprávný *pořádku* sestavy skrýt problémy v hello clusteru, což není žádoucí.

Pro pravidelné vytváření sestav, se dá implementovat s časovačem hello sledovací zařízení. Na zpětné volání časovače můžete hello sledovací zařízení zkontrolujte stav hello a odesílat sestavy na základě aktuálního stavu hello. Neexistuje žádné toosee třeba která sestava byla odeslána dříve nebo se přesvědčte, žádné optimalizace z hlediska zasílání zpráv. Hello stavu klienta má dávkování logiku toohelp s výkonem. Při hello stavu klienta je udržováno zachování připojení, se pokusí interně dokud hello sestavy je potvrzena hello stavu úložiště nebo sledovací zařízení hello vytváří novější sestavu s hello stejné entity, vlastnosti a zdroj.

Vytváření sestav na přechody vyžaduje pečlivě zpracování stavu. sledovací zařízení Hello monitoruje některé podmínky a sestavy pouze v případě, že se změní stav hello. Hello vzhůru tohoto přístupu je, že jsou potřeba méně sestavy. Nevýhodou Hello je komplexní logiku hello hello sledovací zařízení. sledovací zařízení Hello musí zachovat hello podmínky nebo hello sestav tak, aby bylo možné je zkontrolovanou toodetermine změny stavu. Na převzetí služeb při selhání se musí dát pozor se sestavami, přidat, ale dosud nebyl odeslán toohello health store. Hello pořadové číslo musí být stále rostoucí. Pokud ne, hello sestavy jsou odmítnuty jako zastaralé. Ve výjimečných případech hello kde při ztrátě dat může být potřeba synchronizace mezi hello stav hello ohlašování a stav hello hello health store.

Zprávy o přechody smysl pro služby vytváření sestav na sami, prostřednictvím `Partition` nebo `CodePackageActivationContext`. Když hello místní objektu (repliky nebo nasazený balíček služby / nasazené aplikace) je odebrána, všechny její sestavy jsou také odebrány. Toto automatické čištění zmírní hello potřebu synchronizace mezi ohlašování a health store. Pokud sestava hello se nadřazený oddíl nebo nadřazená aplikace, musí dát pozor na převzetí služeb při selhání tooavoid zastaralé sestavy v hello health store. Logika je nutné přidat toomaintain hello správný stav a sestavy zrušte hello z úložiště, když už není potřeba.

## <a name="implement-health-reporting"></a>Implementovat, vytváření sestav stavu
Jakmile entity a sestavu Podrobnosti jsou hello jasné, odesílání sestav stavu lze provést prostřednictvím rozhraní API hello, prostředí PowerShell nebo REST.

### <a name="api"></a>Rozhraní API
tooreport prostřednictvím hello rozhraní API, musíte toocreate typ entity konkrétní toohello sestavy stavu, který chtějí tooreport na. Dejte hello sestavy tooa stavu klienta. Alternativně vytvořte informace o stavu a předejte jej toocorrect reporting metody na `Partition` nebo `CodePackageActivationContext` tooreport na aktuální entity.

Následující příklad ukazuje Hello pravidelné generování sestav z sledovací zařízení v rámci clusteru hello. sledovací zařízení Hello kontroluje, zda externí prostředek je přístupná z v rámci uzlu. Hello prostředků je potřeba manifest služby v rámci aplikace hello. Pokud není k dispozici prostředek hello, hello jiných služeb v rámci aplikace hello můžete i nadále fungovat správně. Proto hello sestavy se odesílají na hello nasazené služby balíček entity každých 30 sekund.

```csharp
private static Uri ApplicationName = new Uri("fabric:/WordCount");
private static string ServiceManifestName = "WordCount.Service";
private static string NodeName = FabricRuntime.GetNodeContext().NodeName;
private static Timer ReportTimer = new Timer(new TimerCallback(SendReport), null, 30 * 1000, 30 * 1000);
private static FabricClient Client = new FabricClient(new FabricClientSettings() { HealthReportSendInterval = TimeSpan.FromSeconds(0) });

public static void SendReport(object obj)
{
    // Test whether hello resource can be accessed from hello node
    HealthState healthState = this.TestConnectivityToExternalResource();

    // Send report on deployed service package, as hello connectivity is needed by hello specific service manifest
    // and can be different on different nodes
    var deployedServicePackageHealthReport = new DeployedServicePackageHealthReport(
        ApplicationName,
        ServiceManifestName,
        NodeName,
        new HealthInformation("ExternalSourceWatcher", "Connectivity", healthState));

    // TODO: handle exception. Code omitted for snippet brevity.
    // Possible exceptions: FabricException with error codes
    // FabricHealthStaleReport (non-retryable, hello report is already queued on hello health client),
    // FabricHealthMaxReportsReached (retryable; user should retry with exponential delay until hello report is accepted).
    Client.HealthManager.ReportHealth(deployedServicePackageHealthReport);
}
```

### <a name="powershell"></a>PowerShell
Odesílat zprávy o stavu s  **odeslání ServiceFabric*EntityType*HealthReport **.

Následující příklad ukazuje Hello pravidelné vytváření sestav na hodnoty využití procesoru na uzlu. sestavy Hello by měly být odeslány každých 30 sekund a mají čas toolive dvě minuty. Pokud vypršení jejich platnosti, hello zpravodaje, která má problémy, tak uzel hello je vyhodnocován v chybě. Když hello procesoru je nad prahovou hodnotou, hello sestava má stav varování. Pokud hello procesoru zůstává nad prahovou hodnotu pro více než čas hello nakonfigurované, uvede se jako chyba. V opačném hello zpravodaje, která odešle stav OK.

```powershell
PS C:\> Send-ServiceFabricNodeHealthReport -NodeName Node.1 -HealthState Warning -SourceId PowershellWatcher -HealthProperty CPU -Description "CPU is above 80% threshold" -TimeToLiveSec 120

PS C:\> Get-ServiceFabricNodeHealth -NodeName Node.1
NodeName              : Node.1
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='PowershellWatcher', Property='CPU', HealthState='Warning', ConsiderWarningAsError=false.

HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 5
                        SentAt                : 4/21/2015 8:01:17 AM
                        ReceivedAt            : 4/21/2015 8:02:12 AM
                        TTL                   : Infinite
                        Description           : Fabric node is up.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/21/2015 8:02:12 AM

                        SourceId              : PowershellWatcher
                        Property              : CPU
                        HealthState           : Warning
                        SequenceNumber        : 130741236814913394
                        SentAt                : 4/21/2015 9:01:21 PM
                        ReceivedAt            : 4/21/2015 9:01:21 PM
                        TTL                   : 00:02:00
                        Description           : CPU is above 80% threshold
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Warning = 4/21/2015 9:01:21 PM
```

Hello následující příklad hlásí přechodný upozornění v replice. Nejdřív získá ID oddílu hello a pak hello ID repliky služby hello, kterou je zajímá. Pak odešle zprávu z **PowershellWatcher** u vlastnosti hello **ResourceDependency**. Hello zprávy týkající se pouze dvě minuty, a automaticky odebrán z úložiště hello.

```powershell
PS C:\> $partitionId = (Get-ServiceFabricPartition -ServiceName fabric:/WordCount/WordCount.Service).PartitionId

PS C:\> $replicaId = (Get-ServiceFabricReplica -PartitionId $partitionId | where {$_.ReplicaRole -eq "Primary"}).ReplicaId

PS C:\> Send-ServiceFabricReplicaHealthReport -PartitionId $partitionId -ReplicaId $replicaId -HealthState Warning -SourceId PowershellWatcher -HealthProperty ResourceDependency -Description "hello external resource that hello primary is using has been rebooted at 4/21/2015 9:01:21 PM. Expect processing delays for a few minutes." -TimeToLiveSec 120 -RemoveWhenExpired

PS C:\> Get-ServiceFabricReplicaHealth  -PartitionId $partitionId -ReplicaOrInstanceId $replicaId


PartitionId           : 8f82daff-eb68-4fd9-b631-7a37629e08c0
ReplicaId             : 130740415594605869
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='PowershellWatcher', Property='ResourceDependency', HealthState='Warning', ConsiderWarningAsError=false.

HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 130740768777734943
                        SentAt                : 4/21/2015 8:01:17 AM
                        ReceivedAt            : 4/21/2015 8:02:12 AM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/21/2015 8:02:12 AM

                        SourceId              : PowershellWatcher
                        Property              : ResourceDependency
                        HealthState           : Warning
                        SequenceNumber        : 130741243777723555
                        SentAt                : 4/21/2015 9:12:57 PM
                        ReceivedAt            : 4/21/2015 9:12:57 PM
                        TTL                   : 00:02:00
                        Description           : hello external resource that hello primary is using has been rebooted at 4/21/2015 9:01:21 PM. Expect processing delays for a few minutes.
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : ->Warning = 4/21/2015 9:12:32 PM
```

### <a name="rest"></a>REST
Odesílat zprávy o stavu pomocí požadavků POST, které přejděte toohello potřeby entity a mají popis sestavy stavu hello textu hello REST. Například v tématu Jak toosend REST [clusteru sestav stavu](https://docs.microsoft.com/rest/api/servicefabric/report-the-health-of-a-cluster) nebo [sestav stavu služby](https://docs.microsoft.com/rest/api/servicefabric/report-the-health-of-a-service). Jsou podporovány všechny entity.

## <a name="next-steps"></a>Další kroky
Na základě hello stavu dat, služba zapisovače a Správce clusteru nebo aplikace si můžete představit způsoby tooconsume hello informace. Například můžete nastavit tak výstrahy podle stavu stav toocatch závažné problémy před jejich vyvolat výpadků. Správci můžete také nastavit tak oprava systémy toofix problémy automaticky.

[Úvod tooService stav infrastruktury monitorování](service-fabric-health-introduction.md)

[Zobrazit sestavy stavu Service Fabric](service-fabric-view-entities-aggregated-health.md)

[Jak tooreport a zkontrolujte stav služby](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[Pomocí sestav o stavu systému pro řešení potíží](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)

[Monitorování a Diagnostika služby místně](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Upgrade aplikace Service Fabric](service-fabric-application-upgrade.md)

