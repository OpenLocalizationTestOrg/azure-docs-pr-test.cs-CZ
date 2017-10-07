---
title: "aaaService prostředků infrastruktury zálohování a obnovení | Microsoft Docs"
description: "Rámcová dokumentace pro Service Fabric zálohování a obnovení"
services: service-fabric
documentationcenter: .net
author: mcoskun
manager: timlt
editor: subramar,jessebenson
ms.assetid: 91ea6ca4-cc2a-4155-9823-dcbd0b996349
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2017
ms.author: mcoskun
ms.openlocfilehash: e502b59c84999c3fe825167383f00a5ebd70c9b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-and-restore-reliable-services-and-reliable-actors"></a>Zálohování a obnovení Reliable Services a Reliable Actors
Azure Service Fabric je vysoká dostupnost platforma, která replikuje hello stavu napříč několika uzly toomaintain tento vysokou dostupnost.  Proto i v případě, že jeden uzel v clusteru hello selže, pokračovat hello služby toobe k dispozici. Při této redundance v vytvořená poskytované hello platformy pravděpodobně dostatečná pro některé, v některých případech je žádoucí, aby služba tooback hello dat (tooan externí úložiště).

> [!NOTE]
> Je důležité toobackup a obnovení dat (a test, který funguje podle očekávání), můžete obnovit z ztrátě dat..
> 
> 

Služba může například, tooback data v pořadí tooprotect z hello následující scénáře:

- V případě hello hello trvalé dojít ke ztrátě celého clusteru Service Fabric.
- Trvalé ztrátě většiny hello repliky oddílu služby
- Správce chyby, které hello stavu omylem získá odstranit nebo poškozený. Například to může dojít, pokud správce s dostatečná oprávnění omylem odstraní hello služby.
- Chyby v hello služby, které dojít k poškození dat. Například tomu může dojít při aktualizace služby kód spustí zápis vadný data tooa spolehlivé kolekce. V takovém případě obě hello kódu a hello dat může mít toobe vrátit tooan dříve stavu.
- Zpracování dat je offline. Může být vhodné toohave offline zpracování dat pro business intelligence, který se stane samostatně z hello služby, která generuje hello data.

Funkce zálohování a obnovení Hello umožňuje službám založený na hello spolehlivé rozhraní API služby toocreate a obnovení zálohy. Hello zálohování rozhraní API poskytované hello platformy umožňují zálohy oddílu služby stavu, bez blokování čtení nebo operace zápisu. obnovení Hello rozhraní API umožňují toobe stavu oddílu služba obnovena ze zálohy, vybrané.

## <a name="types-of-backup"></a>Typy zálohování
Existují dvě možnosti zálohování: úplné a přírůstkové.
Úplné zálohování je zálohy, která obsahuje všechny hello data požadovaná toorecreate hello stavu repliky hello: kontrolních bodů a všechny záznamy protokolu.
Vzhledem k tomu, že má hello kontrolních bodů a hello protokolu, úplné zálohování lze obnovit samostatně.

Hello problém s úplnými zálohami mohou nastat, pokud jsou velké hello kontrolní body.
Například repliky, která má 16 GB paměti stavu bude mít kontrolní body, které dohromady přibližně too16 GB.
Pokud budeme mít plánovaného bodu obnovení pět minut, je potřeba hello repliky toobe zálohovat každých pět minut.
Pokaždé, když se zálohuje, je nutné toocopy 16 GB paměti kontrolní body kromě too50 MB (konfigurovat pomocí `CheckpointThresholdInMB`) za protokolů.

![Příklad úplného zálohování.](media/service-fabric-reliable-services-backup-restore/FullBackupExample.PNG)

Hello řešení toothis problém je přírůstkové zálohování, kde záloha pouze obsahuje záznamy protokolu hello změnit od poslední zálohy hello.

![Příklad přírůstkové zálohování.](media/service-fabric-reliable-services-backup-restore/IncrementalBackupExample.PNG)

Vzhledem k tomu, že přírůstkové zálohy jsou pouze změny od poslední zálohy hello (nezahrnuje kontrolní body hello), se zpravidla toobe rychleji, ale je nebylo možné obnovit samostatně.
toorestore přírůstkové zálohy, hello celý řetěz záloh je povinný.
Řetěz záloh je řetěz záloh počínaje úplnou zálohu a následuje číslo souvislý přírůstkových záloh.

## <a name="backup-reliable-services"></a>Zálohování spolehlivé služby
Hello služby Autor má plnou kontrolu nad tím, kdy toomake zálohování a uložení zálohy.

toostart zálohu, služba hello musí tooinvoke hello zděděná – členská funkce `BackupAsync`.  
Zálohování může být vytvořen pouze ze primární repliky a vyžadují toobe stav zápisu udělena.

Jak je uvedeno níže, `BackupAsync` přebírá `BackupDescription` objektu, kde jeden určit úplné nebo přírůstkové zálohování, jakož i funkce zpětného volání, `Func<< BackupInfo, CancellationToken, Task<bool>>>` která je volána, když místně byla vytvořena hello zálohovací složky a je připravený toobe přesunout na toosome externí úložiště.

```csharp

BackupDescription myBackupDescription = new BackupDescription(backupOption.Incremental,this.BackupCallbackAsync);

await this.BackupAsync(myBackupDescription);

```

Žádost o tootake přírůstkové zálohování může selhat s `FabricMissingFullBackupException`. Tato výjimka označuje, že jeden z následujících věcí hello probíhá:

- Hello repliky nikdy trvá úplné zálohování, protože již není primární,
- Některé hello protokolování záznamů, protože hello poslední zálohy byl zkrácen nebo
- repliky předán hello `MaxAccumulatedBackupLogSizeInMB` limit.

Uživatele můžete zvýšit pravděpodobnost hello je možné toodo přírůstkové zálohování nakonfigurováním `MinLogSizeInMB` nebo `TruncationThresholdFactor`.
Všimněte si, že tyto hodnoty zvýšení zvyšuje hello za využití disku repliky.
Další informace najdete v tématu [spolehlivé konfigurace služeb](service-fabric-reliable-services-configuration.md)

`BackupInfo`poskytuje informace o zálohování hello, včetně umístění hello hello složky pro uložení zálohy hello hello runtime (`BackupInfo.Directory`). Funkce zpětného volání Hello můžete přesunout hello `BackupInfo.Directory` tooan externím obchodu nebo v jiném umístění.  Tato funkce také vrací logickou hodnotu, která určuje, zda bylo možné toosuccessfully přesunutí hello zálohovací složky tooits cílové umístění.

Hello následující kód ukazuje, jak hello `BackupCallbackAsync` metoda může být použité tooupload hello zálohování tooAzure úložiště:

```csharp
private async Task<bool> BackupCallbackAsync(BackupInfo backupInfo, CancellationToken cancellationToken)
{
    var backupId = Guid.NewGuid();

    await externalBackupStore.UploadBackupFolderAsync(backupInfo.Directory, backupId, cancellationToken);

    return true;
}
```

V příkladu předcházející hello `ExternalBackupStore` je hello ukázka třída, která je použité toointerface s Azure Blob storage a `UploadBackupFolderAsync` je metoda hello komprimaci hello složky a umístí jej do úložiště objektů Blob v Azure hello.

Poznámky:

  - Může existovat jenom jedna operace zálohování během letu za repliky v daném okamžiku. Více než jeden `BackupAsync` volání současně vyvolá výjimku `FabricBackupInProgressException` toolimit aktivních pořadových zálohování tooone.
  - Pokud repliku převezme v průběhu zálohování, zálohování hello nemusí byly dokončeny. Po dokončení převzetí služeb při selhání hello z toho důvodu je služba hello odpovědnost toorestart hello zálohování vyvoláním `BackupAsync` podle potřeby.

## <a name="restore-reliable-services"></a>Obnovení spolehlivé služby
Obecně platí hello případech, kdy může být nutné tooperform operace obnovení spadat do jednoho z těchto kategorií:

  - Služba Hello oddílu ke ztrátě dat.. Například získá hello disku pro dvě ze tří repliky pro oddíl (včetně primární repliky hello) poškozený nebo vymazat. nový primární Hello může být nutné toorestore data ze zálohy.
  - dojde ke ztrátě Hello celé služby. Například správce odebere hello celou službou, a proto hello služby a hello data potřebovat toobe obnovit.
  - Služba Hello replikovaných dat poškozené aplikace (např. z důvodu chyb aplikaci). V takovém případě hello služby má toobe upgradovat nebo vrácený tooremove hello příčinu hello poškození a -poškození dat toobe obnovit.

Zatímco řada přístupů je možné, nabízíme některé příklady použití `RestoreAsync` toorecover z hello výše scénáře.

## <a name="partition-data-loss-in-reliable-services"></a>Oddíl ztrátě dat v spolehlivé služby
V takovém případě by hello runtime automaticky rozpoznat hello ztrátě dat a vyvolání hello `OnDataLossAsync` rozhraní API.

Autor služby Hello musí tooperform hello toorecover následující:

  - Potlačí metodu virtuální třídy base hello `OnDataLossAsync`.
  - Najde hello nejnovější zálohy hello externích umístění, které obsahuje hello služby zálohování.
  - Stáhněte nejnovější zálohu hello (a dekomprimovat hello zálohování do zálohovací složky hello, pokud byla komprimována).
  - Hello `OnDataLossAsync` poskytuje metody `RestoreContext`. Volání hello `RestoreAsync` rozhraní API hello poskytuje `RestoreContext`.
  - Vrátí hodnotu PRAVDA, pokud hello obnovení bylo úspěšné.

Následuje příklad implementace hello `OnDataLossAsync` metoda:

```csharp
protected override async Task<bool> OnDataLossAsync(RestoreContext restoreCtx, CancellationToken cancellationToken)
{
    var backupFolder = await this.externalBackupStore.DownloadLastBackupAsync(cancellationToken);

    var restoreDescription = new RestoreDescription(backupFolder);

    await restoreCtx.RestoreAsync(restoreDescription);

    return true;
}
```

`RestoreDescription`Předaná toohello `RestoreContext.RestoreAsync` volání obsahuje člena s názvem `BackupFolderPath`.
Při obnovení jedné úplné zálohování, to `BackupFolderPath` by mělo být nastavené toohello místní cesty hello složku, která obsahuje vaše úplné zálohování.
Při obnovování úplnou zálohu a počet přírůstkové zálohování, `BackupFolderPath` by mělo být nastavené toohello místní cesty hello složku, ne jenom obsahující hello úplné zálohování, ale také všechny hello přírůstkové zálohy.
`RestoreAsync`můžete vyvolat volání `FabricMissingFullBackupException` Pokud hello `BackupFolderPath` zadané neobsahuje úplnou zálohu.
Můžete také vyvolat `ArgumentException` Pokud `BackupFolderPath` má porušený řetězec přírůstkových záloh.
Například pokud obsahuje hello úplného zálohování, první hello přírůstkové a hello třetí přírůstkové zálohování, ale žádné hello druhý přírůstkové zálohování.

> [!NOTE]
> Hello RestorePolicy ve výchozím nastavení tooSafe.  To znamená, že hello `RestoreAsync` rozhraní API se nezdaří s ArgumentException – pokud zjistí, že hello zálohování složka obsahuje stavu, který je starší než nebo rovna toohello stavu obsažené v této replice.  `RestorePolicy.Force`lze použít tooskip této kontroly zabezpečení. To je zadaný jako součást `RestoreDescription`.
> 

## <a name="deleted-or-lost-service"></a>Odstraněné nebo ke ztrátě služby
Pokud je odebrán služby, musíte nejdřív znovu vytvořit hello služby před hello je možné obnovit.  Je důležité toocreate hello služby s hello stejnou konfiguraci, například vytváření oddílů schéma, které hello dat může být obnovena bezproblémově.  Jakmile je služba hello, hello data toorestore rozhraní API (`OnDataLossAsync` výše) má toobe vyvolat každý oddíl této služby. Jeden ze způsobů dosažení jde pomocí `[FabricClient.TestManagementClient.StartPartitionDataLossAsync](https://msdn.microsoft.com/library/mt693569.aspx)` každý oddíl.  

Z tohoto bodu je hello implementace stejné jako hello výše scénář. Každý oddíl musí toorestore hello nejnovější relevantní zálohování z externího úložiště hello. Jeden přímý přístup paměti je ID může mít nyní změnit, protože hello runtime dynamicky vytvoří oddílu ID oddílu hello. Proto služba hello musí toostore hello oddílu příslušné informace a služby název tooidentify hello správné nejnovější zálohy toorestore z pro každý oddíl.

> [!NOTE]
> Není doporučeno toouse `FabricClient.ServiceManager.InvokeDataLossAsync` na každý oddíl toorestore hello celou službou, vzhledem k tomu, které by mohlo poškodit váš stav clusteru.
> 

## <a name="replication-of-corrupt-application-data"></a>Replikace dat poškozený aplikací
Pokud upgradu hello nově nasazené aplikace obsahuje chyby, které může způsobit poškození dat. Například upgradu aplikace může spustit tooupdate každý záznam telefonní číslo v slovník spolehlivé s neplatný kód oblasti.  V takovém případě hello neplatnými telefonními čísly bude replikován vzhledem k tomu, že Service Fabric nemá informace o povaze hello hello data, která ukládají.

Hello nejprve thing toodo po zjištění takové mimořádně závažných chyb, která způsobuje poškození dat je toofreeze hello služby na úrovni aplikace hello a, pokud je to možné, upgradujte verzi toohello hello aplikační kód, který nemá hello chyb.  I po opravení kódu služby hello hello dat může být poškozena a proto dat může být nutné toobe obnovit.  V takových případech nemusí být dostatečná toorestore hello nejnovější zálohu, protože nejnovější zálohy hello také může být poškozený.  Proto, že máte hello toofind poslední se zálohy, která byla vytvořená před hello data získali poškozená.

Pokud si nejste jisti, který zálohy jsou poškozené, můžete nasadit do nového clusteru Service Fabric a obnovit zálohy hello ovlivněných oddílů stejně jako hello výše "Odstraněno nebo ke ztrátě služby" scénář.  Pro každý oddíl spustíte obnovení záloh hello z nejnovější toohello hello alespoň. Po nalezení zálohy, která nemá hello poškození přesunutí nebo odstranění tohoto oddílu všechny zálohy, které byly novější (než zálohování). Tento postup opakujte pro každý oddíl. Teď, když `OnDataLossAsync` je volána v oddílu hello v hello provozní cluster, poslední zálohy hello nalezen v hello externí úložiště bude hello jeden zachyceny hello výše procesu.

Nyní hello kroky hello "Odstraněno nebo ke ztrátě služby" části lze využít toorestore hello stavu hello služby toohello stavu, než buggy kód hello poškozen hello stavu.

Poznámky:

  - Při obnovování, se pravděpodobnost, že hello zálohování obnovit není starší než hello stavu hello oddílu před hello data byla ztracena. Z toho důvodu obnovíte pouze jako poslední možnost toorecover jako množství dat nejdříve.
  - řetězec, který představuje cestu ke složce zálohování hello Hello a hello cest souborů do zálohovací složky hello může být větší než 255 znaků, v závislosti na hello proměnná FabricDataRoot cestu a název typu aplikace délka. To může způsobit některé metody rozhraní .NET, jako je třeba `Directory.Move`, toothrow hello `PathTooLongException` výjimka. Jeden řešení je toodirectly volání rozhraní API kernel32, jako je `CopyFile`.

## <a name="backup-and-restore-reliable-actors"></a>Zálohování a obnovení Reliable Actors


Spolehlivé Framework aktéři je postavená na spolehlivé služby. Hello ActorService, který hostuje hello actor(s) je stavová služba spolehlivé. Proto všechny hello zálohování a obnovení funkce je k dispozici v spolehlivé služby je také k dispozici tooReliable aktéři (s výjimkou chování, které jsou specifické pro zprostředkovatele stavu). Vzhledem k tomu, že zálohování budete přesměrováni na bázi oddílů, se stavy pro všechny účastníky v, že se bude zálohovat oddílu (a obnovení je podobný a proběhne na bázi oddílů). tooperform zálohování a obnovení, vlastník služby hello měli vytvořit vlastní objektu actor služby třídu odvozenou od třídy ActorService a pak zálohování nebo obnovení podobné tooReliable služby, jak je popsáno výše v předchozích částech.

```csharp
class MyCustomActorService : ActorService
{
     public MyCustomActorService(StatefulServiceContext context, ActorTypeInformation actorTypeInfo)
            : base(context, actorTypeInfo)
     {                  
     }
    
    //
   // Method overrides and other code.
    //
}
```

Když vytváříte třídu služby objektu actor vlastní, musíte to také tooregister při registraci objektu actor hello.

```csharp
ActorRuntime.RegisterActorAsync<MyActor>(
   (context, typeInfo) => new MyCustomActorService(context, typeInfo)).GetAwaiter().GetResult();
```

poskytovatel stavu výchozí Hello Reliable actors je `KvsActorStateProvider`. Přírůstkové zálohování není povoleno ve výchozím nastavení pro `KvsActorStateProvider`. Přírůstkové zálohování můžete povolit vytvořením `KvsActorStateProvider` s hello příslušným nastavení v jeho konstruktoru a předejte jí tooActorService konstruktor, jak je znázorněno v následující fragment kódu:

```csharp
class MyCustomActorService : ActorService
{
     public MyCustomActorService(StatefulServiceContext context, ActorTypeInformation actorTypeInfo)
            : base(context, actorTypeInfo, null, null, new KvsActorStateProvider(true)) // Enable incremental backup
     {                  
     }
    
    //
   // Method overrides and other code.
    //
}
```

Po přírůstkové zálohování, trvá přírůstkové zálohování může selhat s FabricMissingFullBackupException pro jednu z následujících důvodů a budete potřebovat tootake úplné zálohy před provedením přírůstkové zálohy:

  - repliky Hello nikdy trvá úplné zálohování, vzhledem k tomu, že jsou primární.
  - Některé záznamy protokolu hello bylo zkráceno od poslední zálohy.

Pokud je povoleno přírůstkové zálohování, `KvsActorStateProvider` nepoužívá cyklické vyrovnávací paměti toomanage protokol zaznamenává a pravidelně se zkrátí. Pokud nedojde k žádné zálohování uživatelem po dobu 45 minut, zkrátí hello systému automaticky hello záznamy protokolu. Tento interval můžete nakonfigurovat tak, že zadáte `logTrunctationIntervalInMinutes` v `KvsActorStateProvider` – konstruktor (podobně jako toowhen povolíte přírůstkové zálohování). záznamy protokolu Hello může získat také zkrácen, pokud primární repliky potřebovat toobuild jiné repliky odesláním všechna jeho data.

Při provádění obnovení ze řetěz záloh, podobně jako tooReliable služby, by měly obsahovat hello BackupFolderPath podadresářů pomocí jednoho podadresáři obsahující úplnou zálohu a ostatní podadresáře obsahující přírůstkové zálohy. rozhraní API obnovení Hello vyvolá výjimku FabricException se příslušná chybová zpráva, pokud selže ověření řetěz záloh hello. 

> [!NOTE]
> `KvsActorStateProvider`aktuálně ignoruje hello možnost RestorePolicy.Safe. Podpora pro tuto funkci je plánované v nadcházející verzi.
> 

## <a name="testing-backup-and-restore"></a>Testování zálohování a obnovení
Je důležité tooensure, který je zálohovaných důležitých dat a lze obnovit z. To lze provést vyvoláním hello `Start-ServiceFabricPartitionDataLoss` rutiny v prostředí PowerShell, které může způsobit ztrátě dat v konkrétní oddíl tootest, zda text hello data zálohování a obnovení funkce pro vaše služba funguje podle očekávání.  Je také možné tooprogrammatically vyvolání ztrátě dat a obnovit ze také tuto událost.

> [!NOTE]
> Můžete najít ukázkové implementace zálohování a obnovení funkce v hello webové aplikace odkaz na Githubu. Podívejte se prosím na hello `Inventory.Service` služby další podrobnosti.
> 
> 

## <a name="under-hello-hood-more-details-on-backup-and-restore"></a>Pod pokličkou hello: Další informace o zálohování a obnovení
Zde je některé další informace o zálohování a obnovení.

### <a name="backup"></a>Zálohování
Hello spolehlivé správce stavu poskytuje hello možnost toocreate konzistentní zálohování bez blokování všechny operace čtení nebo zápisu. toodo, takže ho využívá mechanismus trvalosti kontrolního bodu a protokolu.  Hello spolehlivé správce stavu trvá přibližné (lightweight) kontrolní body v určité zatížení toorelieve bodů z hello transakčního protokolu a vylepšovat dobu obnovení.  Když `BackupAsync` nazývá hello spolehlivé správce stavu dá pokyn všechny spolehlivé objekty toocopy jejich poslední kontrolní bod tooa místní zálohování složce se soubory.  Potom hello spolehlivé správce stavu zkopíruje všechny záznamy protokolu od hello "start ukazatel" toohello nejnovější záznam protokolu do zálohovací složky hello.  Vzhledem k tomu, že všechny záznamy protokolu hello až toohello nejnovější záznam protokolu jsou součástí zálohy hello a hello spolehlivé správce stavu zachovává předběžné protokolování, hello spolehlivé správce stavu zaručuje, že jsou všechny transakce, potvrzeny (`CommitAsync` vrátila úspěšně) jsou zahrnuté v záloze hello.

Jakékoli transakce, která provádí po `BackupAsync` byla volána může nebo nemusí být hello zálohování.  Jakmile hello místní zálohování složky naplněné platformou hello (tj, místní bylo zálohování dokončeno modulem hello runtime), je volána hello služby zálohování zpětného volání.  Tato zpětné volání je zodpovědná za přesunutí hello zálohovací složky tooan externí umístění například Azure Storage.

### <a name="restore"></a>Obnovení
Hello spolehlivé správce stavu poskytuje možnost toorestore hello ze zálohy pomocí hello `RestoreAsync` rozhraní API.  
Hello `RestoreAsync` metodu `RestoreContext` lze volat pouze uvnitř hello `OnDataLossAsync` metoda.
Hello bool vrácený `OnDataLossAsync` označuje, zda služba hello obnovit stav z externího zdroje.
Pokud hello `OnDataLossAsync` vrátí hodnotu true, Service Fabric se znovu vytvořit všechny ostatní repliky z tomuto primárnímu. Service Fabric zajišťuje, že repliky, které obdrží `OnDataLossAsync` volání první přechod toohello primární roli, ale nejsou uděleno číst stav nebo zapisovat stav.
To vyplývá, že pro implementátory StatefulService `RunAsync` dokud nebude volána `OnDataLossAsync` úspěšně dokončí.
Potom `OnDataLossAsync` bude volána pro nový primární hello.
Dokud služby dokončí toto rozhraní API úspěšně (vrací hodnotu true nebo false) a dokončí příslušné Rekonfigurace hello, hello rozhraní API bude udržovat volána po jednom.

`RestoreAsync`nejprve zahodí všechny existující stavy hello primární repliku, která byla volána na.  
Pak hello spolehlivé správce stavu vytvoří všechny objekty spolehlivé hello, které existují v zálohovací složce hello.  
V dalším kroku hello spolehlivé objekty jsou pokyn toorestore z jejich kontrolní body ve složce backup hello.  
Nakonec hello spolehlivé správce stavu obnovuje vlastní stavu ze záznamů protokolu hello hello zálohovací složky a provede obnovení.  
Jako součást procesu obnovení hello jsou operace od hello "výchozí bod", které mají záznamy protokolu potvrzení v zálohovací složce hello přehraná toohello spolehlivé objekty.  
Tento krok zajistí, že hello obnovené stav je konzistentní.

## <a name="next-steps"></a>Další kroky
  - [Reliable Collections](service-fabric-work-with-reliable-collections.md)
  - [Spolehlivé služby rychlý start](service-fabric-reliable-services-quick-start.md)
  - [Spolehlivé služby oznámení](service-fabric-reliable-services-notifications.md)
  - [Spolehlivé služby konfigurace](service-fabric-reliable-services-configuration.md)
  - [Referenční informace pro vývojáře pro spolehlivé kolekce](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)

