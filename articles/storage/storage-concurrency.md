---
title: "aaaManaging souběžnosti v úložišti Microsoft Azure"
description: "Jak toomanage souběžnosti pro hello službám Blob, fronty, tabulky a souborů"
services: storage
documentationcenter: 
author: jasontang501
manager: tadb
editor: tysonn
ms.assetid: cc6429c4-23ee-46e3-b22d-50dd68bd4680
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/11/2017
ms.author: jasontang501
ms.openlocfilehash: 277fbbb880906da6be67b2267ed5c8e457455bd1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="managing-concurrency-in-microsoft-azure-storage"></a>Správa souběžnosti v Microsoft Azure Storage
## <a name="overview"></a>Přehled
Moderní aplikace na základě Internetu obvykle mají více uživatelů, zobrazování a aktualizace dat současně. To vyžaduje toothink vývojáři aplikace pečlivě o tom, jak tooprovide a předvídatelný prostředí tootheir koncovým uživatelům, zejména pro scénáře, kde můžete aktualizovat více uživatelů hello stejná data. Existují tři strategie souběžnosti hlavní data, které vývojáři obvykle zvažovat:  

1. Optimistickou metodu souběžného – provádění aplikace, aktualizace bude jako součást jeho aktualizace ověřte, jestli se od aplikace hello změnil hello data poslední přečíst data. Například pokud dva uživatelé zobrazení na stránce wikiwebu provádět aktualizaci toohello stejné stránku hello wiki platformy musí zajistěte, aby že daná aktualizace druhý hello nepřepisuje hello první aktualizace – a jak uživatelům pochopit, jestli jejich aktualizace proběhla úspěšně, či nikoli. Tato strategie se nejčastěji používá v webových aplikací.
2. Pesimistické souběžnosti – aplikace vyhledávání tooperform aktualizace bude trvat zámek objektu aktualizace hello dat, dokud nebude vydána hello uzamčení brání jiných uživatelů. Například ve scénáři hlavní nebo podřízený data replikace, kde pouze hlavní hello provede aktualizace hello hlavní bude obvykle obsahovat výhradní zámek pro delší dobu na tooensure hello dat, žádný jiný jeden jej aktualizovat.
3. Poslední zapisovače služby wins – postup, který umožňuje žádné tooproceed operace aktualizace bez ověření, pokud jiná aplikace aktualizoval hello dat, vzhledem k tomu, že aplikace hello nejdřív přečíst hello data. Tato strategie (nebo nedostatečná strategie formální) se obvykle používá kde dat rozdělena na oddíly tak, že neexistuje žádná pravděpodobnost, že více uživatelů, budou přistupovat k hello stejná data. Může také být užitečné, kde jsou zpracovávány krátkodobou datové proudy.  

Tento článek obsahuje přehled jak platformy Azure Storage hello usnadňuje vývoj díky prvotřídní podporu pro všechny tři strategie tyto souběžnosti.  

## <a name="azure-storage--simplifies-cloud-development"></a>Úložiště Azure – usnadňuje vývoj cloudu
Hello služba úložiště Azure podporuje všechny tři strategie, i když je kvůli navrženou tooembrace silnou konzistenci model, který zaručuje, že když rozlišovací v jeho možnost tooprovide plnou podporu pro pesimistické a optimistickou metodu souběžného zpracování Hello potvrzení služby úložiště dat vložit nebo aktualizovat operace všechny další získá přístup k datům toothat uvidí hello nejnovější aktualizace. Úložiště platformy, které používají model konzistence typu případné mají prodleva mezi když se provádí zápis jeden uživatel a při aktualizaci hello data lze prohlížet ostatní uživatelé proto komplikují vývoj klientských aplikací v pořadí tooprevent nekonzistence z dopad na koncové uživatele.  

Kromě toho tooselecting příslušné souběžnosti strategie vývojáři také měli vědět jak úložiště platformy izoluje změny – zvlášť změny toohello stejný objekt napříč transakce. Hello služba úložiště Azure používá tooallow izolace snímku číst operations toohappen souběžně operace zápisu v rámci jednoho oddílu. Na rozdíl od jiných úrovních izolace zaručuje izolaci snímku, všechny operace čtení najdete v části konzistentního snímku hello dat i během aktualizace jsou – v podstatě vrácením hello poslední potvrzené hodnoty během zpracování transakce aktualizace.  

## <a name="managing-concurrency-in-blob-storage"></a>Správa souběžnosti v úložišti objektů Blob
Můžete se taky rozhodnout toouse pesimistické nebo optimistickou metodu souběžného zpracování modelů tooblobs toomanage přístup a kontejnery v hello služby objektů blob. Pokud nezadáte explicitně strategie poslední zapíše wins je výchozí hello.  

### <a name="optimistic-concurrency-for-blobs-and-containers"></a>Optimistickou metodu souběžného pro objekty BLOB a kontejnery
Hello služba úložiště přiřadí identifikátor objektu tooevery uložené. Tento identifikátor se aktualizuje pokaždé, když se operace aktualizace se provádí na objekt. identifikátor Hello je vrácen toohello klienta v rámci pomocí hello hlavičku Etab (značky entity), který je definován v rámci protokolu hello HTTP odpovědi HTTP GET. Uživatel provádění, může poslat aktualizaci na takového objektu v hello původní ETag společně s tooensure podmíněného hlavičky, které aktualizace se vztahuje pouze pokud byla splněna určitá podmínka – v takovém případě je podmínka hello hlavičku "If-Match", který vyžaduje hello úložiště Služba tooensure hello hodnota hello značka ETag zadaná v žádosti o aktualizaci hello je hello stejný jako příkaz, který je uložen v hello služby úložiště.  

Osnova Hello tohoto procesu je následující:  

1. Načtení objektu blob ze služby úložiště hello, hello odpověď obsahuje hodnotu hlavičky protokolu HTTP ETag identifikující hello aktuální verze objektu hello v úložišti služby hello.
2. Při aktualizaci objektu blob hello zahrnují hodnota ETag hello získaný v kroku 1 v hello **If-Match** podmíněného hlavičky požadavku hello odeslat toohello služby.
3. Služba Hello porovná hello ETag hello žádost hodnotu s hello aktuální hodnota ETag objektu hello blob.
4. Pokud aktuální hodnota ETag hello objektu hello blob je odlišná verze, než hello ETag v hello **If-Match** podmíněného hlavičky v hello žádosti o služby hello vrátí klienta toohello 412 chyby. To znamená toohello klienta jiným procesem aktualizoval hello blob vzhledem k tomu, že jejím načtení hello klienta.
5. Pokud hello aktuální hodnota objektu hello blob je značka ETag hello stejnou verzi jako hello ETag v hello **If-Match** provede podmíněného hlavičky v hello žádosti o služby hello hello požadovanou operaci a aktualizace hello aktuální hodnota ETag objektu hello blob tooshow, že bylo vytvořeno nové verze.  

Hello následující fragment C# (pomocí hello Klientská knihovna pro úložiště 4.2.0) ukazuje jednoduchý příklad toho, jak tooconstruct **If-Match AccessCondition** podle hello ETag hodnotu, která je k němu přistupovat z hello vlastnosti objektu blob, který byl dříve buď načtena, nebo vložené. Poté používá hello **AccessCondition** objektu při jeho aktualizaci objektu blob hello: hello **AccessCondition** objekt přidá hello **If-Match** záhlaví toohello požadavku. Pokud objekt blob hello byl aktualizován jiným procesem, služby objektů blob hello vrátí zprávu stav HTTP 412 (nezdařil se předběžnou podmínku). Hello úplné ukázku můžete stáhnout tady: [Správa souběžnosti použití služby Azure Storage](http://code.msdn.microsoft.com/Managing-Concurrency-using-56018114).  

```csharp
// Retrieve hello ETag from hello newly created blob
// Etag is already populated as UploadText should cause a PUT Blob call
// toostorage blob service which returns hello etag in response.
string orignalETag = blockBlob.Properties.ETag;

// This code simulates an update by a third party.
string helloText = "Blob updated by a third party.";

// No etag, provided so orignal blob is overwritten (thus generating a new etag)
blockBlob.UploadText(helloText);
Console.WriteLine("Blob updated. Updated ETag = {0}",
blockBlob.Properties.ETag);

// Now try tooupdate hello blob using hello orignal ETag provided when hello blob was created
try
{
    Console.WriteLine("Trying tooupdate blob using orignal etag toogenerate if-match access condition");
    blockBlob.UploadText(helloText,accessCondition:
    AccessCondition.GenerateIfMatchCondition(orignalETag));
}
catch (StorageException ex)
{
    if (ex.RequestInformation.HttpStatusCode == (int)HttpStatusCode.PreconditionFailed)
    {
        Console.WriteLine("Precondition failure as expected. Blob's orignal etag no longer matches");
        // TODO: client can decide on how it wants toohandle hello 3rd party updated content.
    }
    else
        throw;
}  
```

Hello služba úložiště, jako také zahrnuje podporu pro další podmíněného záhlaví **If-upravit-Since**, **If-Unmodified-Since** a **If-None-Match** a také jejich kombinace. Další informace najdete v části [zadání podmíněného hlavičky pro operace služby objektů Blob](http://msdn.microsoft.com/library/azure/dd179371.aspx) na webu MSDN.  

Hello následující tabulka shrnuje hello kontejneru operace, které podmíněného hlavičky accept, jako **If-Match** v hello požadavek a vrátí hodnotu, značka ETag v odpovědi hello.  

| Operace | Vrátí hodnotu ETag kontejneru | Přijímá podmíněného hlavičky |
|:--- |:--- |:--- |
| Vytvoření kontejneru |Ano |Ne |
| Získat vlastnosti kontejneru |Ano |Ne |
| Získat Metadata kontejneru |Ano |Ne |
| Nastavit Metadata kontejneru |Ano |Ano |
| Získání kontejneru seznamu ACL |Ano |Ne |
| Nastavit kontejneru seznamu ACL |Ano |Ano (*) |
| Odstranit kontejner |Ne |Ano |
| Zapůjčení kontejneru |Ano |Ano |
| Seznam objektů BLOB |Ne |Ne |

hello oprávnění (*) je definované SetContainerACL jsou uložené v mezipaměti a oprávnění toothese aktualizace trvat 30 sekund, po které toopropagate během nichž jsou aktualizace není zaručena toobe konzistentní.  

Hello následující tabulka shrnuje operace hello objektů blob, které podmíněného hlavičky accept, jako **If-Match** v hello požadavek a vrátí hodnotu, značka ETag v odpovědi hello.

| Operace | Vrátí hodnotu ETag | Přijímá podmíněného hlavičky |
|:--- |:--- |:--- |
| Uveďte objektů Blob |Ano |Ano |
| Získání objektu Blob |Ano |Ano |
| Získat vlastnosti objektu Blob |Ano |Ano |
| Nastavit vlastnosti objektů Blob |Ano |Ano |
| Získat Metadata objektu Blob |Ano |Ano |
| Nastavit Metadata objektu Blob |Ano |Ano |
| Objekt Blob zapůjčení (*) |Ano |Ano |
| Objekt Blob snímku |Ano |Ano |
| Zkopírování objektu Blob |Ano |Ano (pro zdrojový a cílový objekt blob) |
| Abort – objekt Blob kopie |Ne |Ne |
| Odstranit objekt Blob |Ne |Ano |
| Uveďte bloku |Ne |Ne |
| Uveďte seznam blokovaných položek |Ano |Ano |
| Získat seznam blokovaných položek |Ano |Ne |
| Uveďte stránky |Ano |Ano |
| Get rozsahů stránek |Ano |Ano |

(*) Objekt Blob zapůjčení nezmění hello ETag na objekt blob.  

### <a name="pessimistic-concurrency-for-blobs"></a>Pesimistické souběžnosti pro objekty BLOB
toolock objekt blob pro výhradní použití, můžete získat [zapůjčení](http://msdn.microsoft.com/library/azure/ee691972.aspx) na něm. Když získáte zapůjčení, určíte, jak dlouho musí hello zapůjčení: to může být pro mezi 15 sekund too60 nebo nekonečné které objemy tooan výhradní zámek. Můžete obnovit konečné zapůjčení tooextend ho a vy můžete uvolnit všechny zapůjčení až skončíte s ním. služby objektů blob Hello automaticky uvolní konečné zapůjčení po vypršení jejich platnosti.  

Zapůjčení povolit různých synchronizace strategie toobe podporována, včetně výhradní zápis / sdílené zápisu pro čtení, výhradní / exkluzivní čtení a zápisu sdílené / číst vylučují. Tam, kde existuje zapůjčení služby úložiště hello vynucuje exkluzivní zápisy (put, nastavení a operace odstranění) ale zajistit výhradní právo pro operace čtení vyžaduje tooensure vývojáře hello všechny klientské aplikace používat ID zapůjčení a že pouze jeden klient vždy má platný zapůjčení ID. Operace čtení, které neobsahují výsledku ID zapůjčení sdílené čtení.  

Hello C# následující fragment kódu ukazuje příklad získávání výhradní zapůjčení pro 30 sekund na objekt blob, aktualizace hello obsahu objektu hello blob a uvolněním hello zapůjčení. Pokud již existuje platný zapůjčení u objektu blob hello při pokusu o tooacquire nové zapůjčení, služby objektů blob hello vrátí výsledek stav "Konflikt HTTP (409)". Hello fragment kódu níže používá **AccessCondition** objektu tooencapsulate informace o zapůjčení hello, pokud má objekt blob hello tooupdate žádosti v úložišti služby hello.  Hello úplné ukázku můžete stáhnout tady: [Správa souběžnosti použití služby Azure Storage](http://code.msdn.microsoft.com/Managing-Concurrency-using-56018114).

```csharp
// Acquire lease for 15 seconds
string lease = blockBlob.AcquireLease(TimeSpan.FromSeconds(15), null);
Console.WriteLine("Blob lease acquired. Lease = {0}", lease);

// Update blob using lease. This operation will succeed
const string helloText = "Blob updated";
var accessCondition = AccessCondition.GenerateLeaseCondition(lease);
blockBlob.UploadText(helloText, accessCondition: accessCondition);
Console.WriteLine("Blob updated using an exclusive lease");

//Simulate third party update tooblob without lease
try
{
    // Below operation will fail as no valid lease provided
    Console.WriteLine("Trying tooupdate blob without valid lease");
    blockBlob.UploadText("Update without lease, will fail");
}
catch (StorageException ex)
{
    if (ex.RequestInformation.HttpStatusCode == (int)HttpStatusCode.PreconditionFailed)
        Console.WriteLine("Precondition failure as expected. Blob's lease does not match");
    else
        throw;
}  
```

Když zkusíte bez předávání hello zapůjčení ID operace zápisu u zapůjčených objektu blob, hello požadavek selže s chybou 412. Všimněte si, že pokud hello zapůjčení vyprší před voláním hello **UploadText** metoda ale stále předat hello zapůjčení ID, hello požadavek také selže s **412** chyby. Další informace o správě identifikátory zapůjčení a časy vypršení platnosti zapůjčení najdete v tématu hello [zapůjčení Blob](http://msdn.microsoft.com/library/azure/ee691972.aspx) dokumentace k REST.  

Hello následující operace objektů blob můžete použít zapůjčení toomanage pesimistické souběžnosti:  

* Uveďte objektů Blob
* Získání objektu Blob
* Získat vlastnosti objektu Blob
* Nastavit vlastnosti objektů Blob
* Získat Metadata objektu Blob
* Nastavit Metadata objektu Blob
* Odstranit objekt Blob
* Uveďte bloku
* Uveďte seznam blokovaných položek
* Získat seznam blokovaných položek
* Uveďte stránky
* Get rozsahů stránek
* Objekt Blob snímku - ID zapůjčení nepovinná, pokud existuje zapůjčení
* Kopírování objektu Blob - ID zapůjčení vyžaduje, pokud existuje zapůjčení na hello cílový objekt blob
* Abort – objekt Blob kopírování – zapůjčení ID vyžaduje, pokud existuje nekonečné zapůjčení na cílový objekt blob hello
* Objekt Blob zapůjčení  

### <a name="pessimistic-concurrency-for-containers"></a>Pesimistické souběžnosti pro kontejnery
Zapůjčení kontejnerům povolit hello stejné strategie toobe synchronizace podporované jako na objekty BLOB (exkluzivní zápisu / sdílené zápisu pro čtení, výhradní / exkluzivní čtení a zápisu sdílené / číst exkluzivní) ale na rozdíl od objektů BLOB služby úložiště hello pouze vynucuje výhradního práva na operace delete. toodelete kontejner s aktivní zapůjčení, klient musí obsahovat ID aktivní zapůjčení hello se žádost o odstranění hello. Všechny ostatní operace kontejneru uspět na kontejner zapůjčených bez včetně hello zapůjčení ID v takovém případě jsou sdílené operace. Pokud se vyžaduje výhradní právo na aktualizace (put nebo sadu) nebo operací čtení pak vývojáři měli zajistěte, aby že všichni klienti používat ID zapůjčení a že jen jeden klientský současně má platný zapůjčení ID.  

Hello následující operace kontejneru můžete použít zapůjčení toomanage pesimistické souběžnosti:  

* Odstranit kontejner
* Získat vlastnosti kontejneru
* Získat Metadata kontejneru
* Nastavit Metadata kontejneru
* Získání kontejneru seznamu ACL
* Nastavit kontejneru seznamu ACL
* Zapůjčení kontejneru  

Další informace najdete v tématu:  

* [Určení podmíněného hlavičky pro operace služby objektů Blob](http://msdn.microsoft.com/library/azure/dd179371.aspx)
* [Zapůjčení kontejneru](http://msdn.microsoft.com/library/azure/jj159103.aspx)
* [Objekt Blob zapůjčení](http://msdn.microsoft.com/library/azure/ee691972.aspx)

## <a name="managing-concurrency-in-hello-table-service"></a>Správa souběžnost v hello služby Table
služby table Hello používá optimistickou metodu, souběžnosti kontroly jako hello výchozí chování při práci s entitami, na rozdíl od služby objektů blob hello, kde je nutné explicitně zvolit tooperform optimistickou metodu souběžného kontroly. Hello jiných rozdíl mezi hello tabulky a objektů blob služby je, že můžete spravovat pouze hello souběžnosti chování entit zatímco s hello služby objektů blob můžete spravovat hello souběžnosti kontejnery a objekty BLOB.  

optimistickou metodu souběžného toouse a toocheck Pokud jiný proces upravit entitu vzhledem k tomu, že jste získali od hello tabulka úložiště služby, můžete použít hello ETag hodnotu, kterou dostanete při služby table hello vrátí entity. Osnova Hello tohoto procesu je následující:  

1. Načtení entity ze služby úložiště table hello, hello odpověď obsahuje hodnotu značka ETag, který identifikuje hello aktuální identifikátor přidružený ke dané entity v úložišti služby hello.
2. Při aktualizaci hello entity zahrnují hodnota ETag hello získaný v kroku 1 v hello povinné **If-Match** hlavičky požadavku hello odeslat toohello služby.
3. Služba Hello porovná hello ETag hello žádost hodnotu s hello aktuální hodnota ETag hello entity.
4. Pokud je aktuální hodnota ETag hello hello entity se liší od hello ETag v hello povinné **If-Match** hlavičky v hello žádosti o služby hello vrátí klienta toohello 412 chyby. To znamená toohello klienta, že má jiným procesem od klienta hello jejím načtení aktualizace hello entity.
5. Pokud je hello hello aktuální hodnota ETag hello entity, stejně jako hello ETag v hello povinné **If-Match** hlavičky v požadavku hello nebo hello **If-Match** hlavička obsahuje hello zástupný znak (*), služba hello provede hello požadovanou operaci a aktualizace hello aktuální hodnota ETag hello entity tooshow, aby byla aktualizována.  

Všimněte si, že na rozdíl od služby objektů blob hello, vyžaduje služby table hello tooinclude klienta hello **If-Match** záhlaví v žádosti o aktualizaci. Je však možné tooforce nepodmíněné aktualizovat (poslední zapisovače wins strategie) a obejít kontrolách souběžnosti, pokud se klient hello nastaví hello **If-Match** záhlaví toohello zástupný znak (*) v žádosti o hello.  

Následující fragment kódu jazyka C# Hello ukazuje entitu zákazníka, která byla dříve buď vytvořit nebo načíst s e-mailové adresy aktualizovat. Hello počáteční vložit nebo načíst operace úložiště hello ETag hodnotu v objektu hello zákazníka a protože ukázka hello používá hello stejnou instanci objektu, když se provede hello operace nahrazování, automaticky odesílá hello ETag hodnota back toohello tabulky služby povolení hello služby toocheck pro porušení souběžnosti. Pokud jiný proces byl aktualizován hello entity ve službě table storage, vrátí se hello služby zpráva stav HTTP 412 (nezdařil se předběžnou podmínku).  Hello úplné ukázku můžete stáhnout tady: [Správa souběžnosti použití služby Azure Storage](http://code.msdn.microsoft.com/Managing-Concurrency-using-56018114).

```csharp
try
{
    customer.Email = "updatedEmail@contoso.org";
    TableOperation replaceCustomer = TableOperation.Replace(customer);
    customerTable.Execute(replaceCustomer);
    Console.WriteLine("Replace operation succeeded.");
}
catch (StorageException ex)
{
    if (ex.RequestInformation.HttpStatusCode == 412)
        Console.WriteLine("Optimistic concurrency violation – entity has changed since it was retrieved.");
    else
        throw;
}  
```

tooexplicitly zakázat kontrolu souběžnosti hello, měli byste nastavit hello **značka ETag** vlastnost hello **zaměstnanec** objektu příliš "*" před spuštěním operaci nahrazení hello.  

```csharp
customer.ETag = "*";  
```

Hello následující tabulka shrnuje jak hello tabulka entity operations použít ETag hodnoty:

| Operace | Vrátí hodnotu ETag | Vyžaduje hlavička If-Match požadavku |
|:--- |:--- |:--- |
| Dotaz entity |Ano |Ne |
| Vložení Entity |Ano |Ne |
| Aktualizace Entity |Ano |Ano |
| Sloučení Entity |Ano |Ano |
| Odstranění Entity |Ne |Ano |
| Vložení nebo nahrazení Entity |Ano |Ne |
| Vložení nebo sloučení Entity |Ano |Ne |

Všimněte si, že hello **vložení nebo nahrazení Entity** a **vložení nebo sloučení Entity** provést operace *není* provést jakékoli kontrolách souběžnosti, protože se neodesílají hodnotu toohello značka ETag služby Table.  

Obecně vývojáře, kteří používají tabulky se spoléhají na optimistickou metodu souběžného při vývoji škálovatelné aplikace. V případě potřeby pesimistické zamykání jeden ze způsobů vývojáři můžou při přístupu k tabulek je tooassign určený objekt blob pro každou tabulku a zkuste tootake zapůjčení u objektu blob hello před operační v tabulce hello. Tento přístup nevyžaduje tooensure aplikace hello všechny přístup k datům cesty získat hello zapůjčení předchozí toooperating v tabulce hello. Nezapomeňte také, že hello zapůjčení minimální doba je 15 sekund což vyžaduje, aby pečlivě zvážit pro škálovatelnost.  

Další informace najdete v tématu:  

* [Operace na entity](http://msdn.microsoft.com/library/azure/dd179375.aspx)  

## <a name="managing-concurrency-in-hello-queue-service"></a>Správa souběžnost v hello služby front
Jeden scénář, ve které souběžnosti je důležitou roli v hello služby Řízení front služby je, kde jsou více klientů načítání zpráv z fronty. Pokud zpráva se načítají z fronty hello, hello odpověď obsahuje uvítací zprávu a hodnotu pop přijetí, která je požadovaná toodelete uvítací zprávu. uvítací zprávu není automaticky odstraněn z hello fronty, ale po byl načteny, není viditelné tooother klientů pro hello časový interval určený parametrem visibilitytimeout hello. Hello klienta, který načte zprávu hello je očekávané toodelete uvítací zprávu po jeho zpracování a před hello času určeného hello TimeNextVisible element hello odpovědi, které se počítá na základě hodnoty hello hello visibilitytimeout parametr. Hodnota Hello visibilitytimeout je přidaná toohello čas, na které hello zprávu načíst hodnotu hello toodetermine TimeNextVisible.  

Služba front Hello nemá podpora souběžnosti optimistické nebo pesimistické a pro tento důvod klienti zpracování zpráv z fronty načíst by měl zajistěte, aby způsobem idempotent zpracování zpráv. Strategie poslední wins zapisovače se používá pro operace aktualizace, jako je například SetQueueServiceProperties, SetQueueMetaData, SetQueueACL a UpdateMessage.  

Další informace najdete v tématu:  

* [Rozhraní API REST služby fronty](http://msdn.microsoft.com/library/azure/dd179363.aspx)
* [Získávání zpráv](http://msdn.microsoft.com/library/azure/dd179474.aspx)  

## <a name="managing-concurrency-in-hello-file-service"></a>Správa souběžnost v hello služba souborů
Služba souborů Hello lze přistupovat pomocí dva koncové body jiný protokol – protokolu SMB a REST. Hello služby REST nemá podporu pro optimistické zamykání nebo pesimistické zamykání a všechny aktualizace bude postupovat podle strategie poslední zapisovače služby wins. Klienti SMB, které připojení sdílené složky můžete využít systémových uzamčení mechanismy toomanage přístup tooshared souborů – včetně hello možnost tooperform pesimistické zamykání. Když klienti SMB otevře soubor, určuje hello přístup k souborům a sdílenou složku režimu. Nastavení přístup k souborům možnost "Zápisu" nebo "Pro čtení a zápis" spolu s režimem sdílené složky "Žádný" bude mít za následek hello souboru je uzamčený klienti SMB, dokud hello soubor se zavřel. Pokud dojde k pokusu o souboru, kde klienti SMB má hello soubor uzamčen operace REST hello služby REST vrátí stavový kód 409 (konflikt) s kódem chyby SharingViolation.  

Když klienti SMB otevře soubor pro odstranění, označí hello soubor jako čekající na vyřízení odstranit, dokud všechny ostatní klienty protokolu SMB jsou uzavřeny otevřenými popisovači v souboru. Když soubor je označen jako čekající na odstranění, všechny operace REST u tohoto souboru vrátí stavový kód 409 (konflikt) s kódem chyby SMBDeletePending. Stavový kód 404 (není nalezena) nevrátí, protože je to možné, hello SMB klienta tooremove hello čeká na odstranění příznak předchozí tooclosing hello souboru. Jinými slovy stavový kód 404 (není nalezena) se očekávají jenom, když hello soubor byl odebrán. Všimněte si, že soubor je v protokolu SMB čeká na odstranění stavu, ho nebudou zahrnuty do hello soubory seznamu výsledků. Poznámka: operace REST odstranit soubor a odstraňte adresář REST hello potvrzeny atomicky a výsledkem není čekající také odstranit stavu.  

Další informace najdete v tématu:  

* [Správa souborů zamkne](http://msdn.microsoft.com/library/azure/dn194265.aspx)  

## <a name="summary-and-next-steps"></a>Souhrn a další kroky
Hello byla služba Microsoft Azure Storage určené toomeet hello potřebám hello nejsložitější online aplikace bez vynucení vývojáři toocompromise nebo přehodnotit klíče návrhu předpoklady například konzistence souběžnosti a data budou pocházet tootake pro oprávnění.  

Pro hello proveďte ukázkovou aplikaci, kterou se odkazuje v tomto blogu:  

* [Správa souběžnosti použití služby Azure Storage – ukázková aplikace](http://code.msdn.microsoft.com/Managing-Concurrency-using-56018114)  

Další informace o Azure Storage najdete v tématu:  

* [Microsoft Azure Storage domovské stránky](https://azure.microsoft.com/services/storage/)
* [Úvod tooAzure úložiště](storage-introduction.md)
* Začínáme se Storage [Blob](storage-dotnet-how-to-use-blobs.md), [tabulky](storage-dotnet-how-to-use-tables.md), [fronty](storage-dotnet-how-to-use-queues.md), a [soubory](storage-dotnet-how-to-use-files.md)
* Architektura úložiště – [Azure Storage: vysoce dostupný Cloudová služba úložiště se silnou konzistenci](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)

