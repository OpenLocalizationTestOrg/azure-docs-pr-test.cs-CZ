---
title: "aaaHow toouse úložiště Azure Table z Node.js | Microsoft Docs"
description: "Ukládejte si strukturovaná data v cloudu hello pomocí Azure Table storage, úložiště dat typu NoSQL."
services: storage
documentationcenter: nodejs
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: fc2e33d2-c5da-4861-8503-53fdc25750de
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 12/08/2016
ms.author: marsma
ms.openlocfilehash: 990a71337b806d759d0277a7691712346db7b355
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-table-storage-from-nodejs"></a>Jak toouse úložiště Azure Table z Node.js
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-langsoon-tip-include](../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

## <a name="overview"></a>Přehled
Toto téma ukazuje, jak služba tooperform běžné scénáře s využitím hello Azure Table v aplikaci Node.js.

Hello příklady kódu v tomto tématu se předpokládá, že již máte aplikace Node.js. Informace o toocreate aplikace Node.js v Azure, najdete v některé z těchto témat:

* [Vytvoření webové aplikace Node.js ve službě Azure App Service](../app-service-web/app-service-web-get-started-nodejs.md)
* [Vytváření a nasazování tooAzure Node.js webové aplikace pomocí služby WebMatrix](../app-service-web/web-sites-nodejs-use-webmatrix.md)
* [Sestavení a nasazení tooan aplikace Node.js Azure Cloud Service](../cloud-services/cloud-services-nodejs-develop-deploy-app.md) (pomocí prostředí Windows PowerShell)

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="configure-your-application-tooaccess-azure-storage"></a>Konfigurace tooaccess vaše aplikace Azure Storage
toouse Azure Storage, musíte hello sada SDK úložiště Azure pro platformu Node.js, která obsahuje sadu knihoven pohodlí, které komunikují s služby REST úložiště hello.

### <a name="use-node-package-manager-npm-tooinstall-hello-package"></a>Použijte uzel balíček správce (NPM) tooinstall hello balíček
1. Pomocí rozhraní příkazového řádku, jako například **prostředí PowerShell** (Windows), **Terminálové** (Mac), nebo **Bash** (Unix) a přejděte toohello složku, kde jste vytvořili vaší aplikace.
2. Typ **npm nainstalujte azure-storage** v příkazovém okně hello. Výstup z příkazu hello je podobné toohello následující ukázka.

       azure-storage@0.5.0 node_modules\azure-storage
       +-- extend@1.2.1
       +-- xmlbuilder@0.4.3
       +-- mime@1.2.11
       +-- node-uuid@1.4.3
       +-- validator@3.22.2
       +-- underscore@1.4.4
       +-- readable-stream@1.0.33 (string_decoder@0.10.31, isarray@0.0.1, inherits@2.0.1, core-util-is@1.0.1)
       +-- xml2js@0.2.7 (sax@0.5.2)
       +-- request@2.57.0 (caseless@0.10.0, aws-sign2@0.5.0, forever-agent@0.6.1, stringstream@0.0.4, oauth-sign@0.8.0, tunnel-agent@0.4.1, isstream@0.1.2, json-stringify-safe@5.0.1, bl@0.9.4, combined-stream@1.0.5, qs@3.1.0, mime-types@2.0.14, form-data@0.2.0, http-signature@0.11.0, tough-cookie@2.0.0, hawk@2.3.1, har-validator@1.8.0)
3. Můžete ručně spustit hello **ls** tooverify příkaz, **uzlu\_moduly** složka byla vytvořena. V této složce budou najde hello **azure-storage** balíček, který obsahuje hello knihovny potřebujete tooaccess úložiště.

### <a name="import-hello-package"></a>Importovat balíček hello
Přidejte následující kód toohello začátek hello hello **server.js** souborů ve vaší aplikaci:

```nodejs
var azure = require('azure-storage');
```

## <a name="set-up-an-azure-storage-connection"></a>Nastavit připojení k Azure Storage
modul Hello azure, bude číst hello proměnné prostředí AZURE\_úložiště\_účet a AZURE\_úložiště\_přístup\_klíč nebo AZURE\_úložiště\_připojení \_Řetězec pro požadované informace tooconnect tooyour účtu úložiště Azure. Pokud nejsou nastavené těchto proměnných prostředí, musíte zadat informace o účtu hello při volání metody **TableService**.

Příklad nastavení proměnných prostředí hello v hello [portál Azure](https://portal.azure.com) web Azure, najdete v části [Node.js webové aplikace pomocí služby Azure Table hello](../app-service-web/storage-nodejs-use-table-storage-web-site.md).

## <a name="create-a-table"></a>Vytvoření tabulky
Hello následující kód vytvoří **TableService** objektu a používá je toocreate novou tabulku. Přidejte následující hello v hello horní části **server.js**.

```nodejs
var tableSvc = azure.createTableService();
```

Hello volání příliš**createTableIfNotExists** vytvoří novou tabulku se zadaným názvem hello, pokud ještě neexistuje. Hello následující příklad vytvoří novou tabulku s názvem "mytable", pokud ještě neexistuje:

```nodejs
tableSvc.createTableIfNotExists('mytable', function(error, result, response){
  if(!error){
    // Table exists or created
  }
});
```

Hello `result.created` bude `true` Pokud se vytvoří nové tabulky, a `false` Pokud hello tabulka již existuje. Hello `response` bude obsahovat informace o požadavku hello.

### <a name="filters"></a>Filtry
Volitelné filtrování operace může být použité toooperations provádí pomocí **TableService**. Filtrování operací může zahrnovat protokolování, automatickým opakovaným pokusem o atd. Filtry jsou objekty, které implementovat metodu podpisem hello:

```nodejs
function handle (requestOptions, next)
```

Až to předzpracování na možnosti hello požadavku, hello musí metoda toocall tlačítko Další"předání zpětné volání s hello následující podpis:

```nodejs
function (returnObject, finalCallback, next)
```

V této zpětného volání a po zpracování hello returnObject (hello odpověď ze serveru toohello požadavek hello), zpětné volání hello musí tooeither vyvolat další, pokud existuje toocontinue zpracování ostatní filtry nebo jednoduše vyvolat finalCallback jinak tooend hello volání služby.

Dva filtry, které implementují logiku opakovaných pokusů, které jsou součástí hello Azure SDK pro Node.js, **ExponentialRetryPolicyFilter** a **LinearRetryPolicyFilter**. vytvoří následující Hello **TableService** objekt, který používá hello **ExponentialRetryPolicyFilter**:

```nodejs
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var tableSvc = azure.createTableService().withFilter(retryOperations);
```

## <a name="add-an-entity-tooa-table"></a>Přidat tooa tabulka entity
tooadd entity, nejprve vytvořit objekt, který definuje vlastnosti vaší entity. Musí obsahovat všechny entity **PartitionKey** a **RowKey**, které jsou jedinečné identifikátory pro entitu hello.

* **PartitionKey** -určuje hello oddílu hello entity uložená v
* **RowKey** – jednoznačně identifikuje hello entity v rámci oddílu hello

Obě **PartitionKey** a **RowKey** musí být řetězcové hodnoty. Další informace najdete v tématu [hello Principy datového modelu služby Table](http://msdn.microsoft.com/library/azure/dd179338.aspx).

Hello následuje příklad definování entity. Všimněte si, že **dueDate** je definována jako typ **Edm.DateTime**. Zadání typu hello je volitelný a typy bude možné odvodit, pokud není zadán.

```nodejs
var task = {
  PartitionKey: {'_':'hometasks'},
  RowKey: {'_': '1'},
  description: {'_':'take out hello trash'},
  dueDate: {'_':new Date(2015, 6, 20), '$':'Edm.DateTime'}
};
```

> [!NOTE]
> K dispozici je také **časové razítko** pole pro každý záznam, který je nastavena jako Azure po vložení nebo aktualizaci entity.
>
>

Můžete taky hello **entityGenerator** toocreate entity. Hello následující příklad vytvoří hello stejné entity úloh pomocí hello **entityGenerator**.

```nodejs
var entGen = azure.TableUtilities.entityGenerator;
var task = {
  PartitionKey: entGen.String('hometasks'),
  RowKey: entGen.String('1'),
  description: entGen.String('take out hello trash'),
  dueDate: entGen.DateTime(new Date(Date.UTC(2015, 6, 20))),
};
```

tooadd tabulku tooyour entity předat hello entity objektu toohello **insertEntity** metoda.

```nodejs
tableSvc.insertEntity('mytable',task, function (error, result, response) {
  if(!error){
    // Entity inserted
  }
});
```

Pokud hello operace úspěšná, `result` bude obsahovat hello [značka ETag](http://en.wikipedia.org/wiki/HTTP_ETag) z hello vložit záznam a `response` bude obsahovat informace o operaci hello.

Příklad odpovědi:

```nodejs
{ '.metadata': { etag: 'W/"datetime\'2015-02-25T01%3A22%3A22.5Z\'"' } }
```

> [!NOTE]
> Ve výchozím nastavení **insertEntity** nevrací entity hello vložit jako součást hello `response` informace. Pokud plánujete provádí jiné operace na tuto entitu, nebo chcete toocache hello informace, může být užitečné toohave vrátil jako součást hello `result`. Můžete k tomu povolením **echoContent** následujícím způsobem:
>
> `tableSvc.insertEntity('mytable', task, {echoContent: true}, function (error, result, response) {...}`
>
>

## <a name="update-an-entity"></a>Aktualizace entity
Existuje několik metod k dispozici tooupdate stávající entity:

* **replaceEntity** -aktualizace stávající entity podle jeho nahrazení
* **mergeEntity** -aktualizace stávající entity sloučením nové hodnoty vlastností do stávající entity hello
* **insertOrReplaceEntity** -aktualizace stávající entity podle jeho nahrazení. Pokud žádná entita existuje, bude možné vložit nový
* **insertOrMergeEntity** -aktualizace stávající entity sloučením nové hodnoty vlastností do stávající hello. Pokud žádná entita existuje, bude možné vložit nový

Hello následující příklad ukazuje, aktualizuje entitu s využitím **replaceEntity**:

```nodejs
tableSvc.replaceEntity('mytable', updatedTask, function(error, result, response){
  if(!error) {
    // Entity updated
  }
});
```

> [!NOTE]
> Ve výchozím nastavení aktualizaci entity nekontroluje toosee Pokud aktualizovaných dat hello dříve bylo změněno jiným procesem. Souběžná aktualizace toosupport:
>
> 1. Získáte hello značka ETag objektu hello aktualizované. K této chybě dochází v rámci hello `response` pro všechny operace související entity a mohou být načteny prostřednictvím `response['.metadata'].etag`.
> 2. Při provádění operace aktualizace na entitu, přidejte načíst hello ETag informace dříve toohello novou entitu. Například:
>
>       entity2 [.metadata] .etag = currentEtag;
> 3. Proveďte operaci aktualizace hello. Pokud byl upraven hello entity vzhledem k tomu, že jste získali hello hodnota ETag, jako je například jiná instance aplikace, `error` bude vrácen s oznámením, že nebyla splněna podmínka aktualizace hello určený v požadavku hello.
>
>

S **replaceEntity** a **mergeEntity**, pokud neexistuje hello entity, který se právě aktualizuje, pak hello operace aktualizace se nezdaří. Proto pokud chcete toostore entity bez ohledu na to, jestli už existuje, použijte **insertOrReplaceEntity** nebo **insertOrMergeEntity**.

Hello `result` operace úspěšná aktualizace bude obsahovat hello **Značka Etag** hello aktualizovat entity.

## <a name="work-with-groups-of-entities"></a>Práce se skupinami entit
Někdy má smysl toosubmit více operací společně v batch tooensure atomic zpracování serverem hello. tooaccomplish, který použít hello **TableBatch** třídy toocreate dávky a pak použijte hello **executeBatch** metodu **TableService** tooperform hello zpracovat v dávce operace.

 Hello následující příklad ukazuje, odesílání dvě entity v dávce:

```nodejs
var task1 = {
  PartitionKey: {'_':'hometasks'},
  RowKey: {'_': '1'},
  description: {'_':'Take out hello trash'},
  dueDate: {'_':new Date(2015, 6, 20)}
};
var task2 = {
  PartitionKey: {'_':'hometasks'},
  RowKey: {'_': '2'},
  description: {'_':'Wash hello dishes'},
  dueDate: {'_':new Date(2015, 6, 20)}
};

var batch = new azure.TableBatch();

batch.insertEntity(task1, {echoContent: true});
batch.insertEntity(task2, {echoContent: true});

tableSvc.executeBatch('mytable', batch, function (error, result, response) {
  if(!error) {
    // Batch completed
  }
});
```

Pro úspěšné dávkových operací `result` bude obsahovat informace o jednotlivých operací ve službě hello batch.

### <a name="work-with-batched-operations"></a>Práce s dávkové operace
Operace přidány tooa batch může být prověřovány zobrazením hello `operations` vlastnost. Můžete také použít následující metody toowork s operacemi hello:

* **Vymazat** -vymaže všechny operace z dávky
* **getOperations** -získá operace z hello batch
* **hasOperations** -vrátí hodnotu true, pokud hello batch obsahuje operace
* **removeOperations** – odebere operace
* **velikost** – vrátí text hello počet operací v dávce hello

## <a name="retrieve-an-entity-by-key"></a>Načtení entity pomocí klíče
tooreturn konkrétní entitu podle hello **PartitionKey** a **RowKey**, použijte hello **retrieveEntity** metoda.

```nodejs
tableSvc.retrieveEntity('mytable', 'hometasks', '1', function(error, result, response){
  if(!error){
    // result contains hello entity
  }
});
```

Po dokončení této operace `result` bude obsahovat hello entity.

## <a name="query-a-set-of-entities"></a>Dotaz na sadu entit
tooquery tabulky, použijte hello **TableQuery** objektu toobuild do výrazu dotazu pomocí hello následující klauzulí:

* **Vyberte** -hello pole toobe vrácená z dotazu hello
* **kde** – hello kde – klauzule

  * **a** – `and` kde podmínky
  * **nebo** – `or` kde podmínky
* **horní** -hello počet položek toofetch

Hello následující příklad vytvoří dotaz, který vrátí hello nejvyšší pěti položek s PartitionKey 'hometasks'.

```nodejs
var query = new azure.TableQuery()
  .top(5)
  .where('PartitionKey eq ?', 'hometasks');
```

Vzhledem k tomu **vyberte** se nepoužívá, bude vrácen všechna pole. tooperform hello dotaz na tabulku, použijte **queryEntities**. Hello následující příklad používá tento query tooreturn entity z "mytable".

```nodejs
tableSvc.queryEntities('mytable',query, null, function(error, result, response) {
  if(!error) {
    // query was successful
  }
});
```

V případě úspěšného `result.entries` bude obsahovat pole entit, které odpovídají dotazu hello. Pokud byl hello dotaz nelze tooreturn všechny entity, `result.continuationToken` bude jinou hodnotu než*null* , můžete použít jako hello třetí parametr **queryEntities** tooretrieve více výsledků. Hello počáteční dotaz, použít *null* pro třetí parametr hello.

### <a name="query-a-subset-of-entity-properties"></a>Dotaz na podmnožinu vlastností entity
Tabulku tooa dotaz můžete načíst několika pole z entity.
To zmenšuje šířku pásma a může zlepšit výkon dotazů, hlavně pro velké entity. Použití hello **vyberte** vrátil klauzule a předejte jí hello názvy polí toobe hello. Například hello následující dotaz vrátí jenom hello **popis** a **dueDate** pole.

```nodejs
var query = new azure.TableQuery()
  .select(['description', 'dueDate'])
  .top(5)
  .where('PartitionKey eq ?', 'hometasks');
```

## <a name="delete-an-entity"></a>Odstranění entity
Můžete odstranit pomocí jeho klíče oddílu a řádku entity. V tomto příkladu hello **task1** objekt obsahuje hello **RowKey** a **PartitionKey** hodnoty toobe entity hello odstranit. Pak je předán hello objekt toohello **deleteEntity** metoda.

```nodejs
var task = {
  PartitionKey: {'_':'hometasks'},
  RowKey: {'_': '1'}
};

tableSvc.deleteEntity('mytable', task, function(error, response){
  if(!error) {
    // Entity deleted
  }
});
```

> [!NOTE]
> Zvažte použití značky etag binárním rozsáhlým při odstraňování položek, tooensure, který hello položky nebyl změněn jiným procesem. V tématu [aktualizovat entitu,](#update-an-entity) informace o použití značky etag binárním rozsáhlým.
>
>

## <a name="delete-a-table"></a>Odstranění tabulky
Následující kód Hello odstraní tabulku z účtu úložiště.

```nodejs
tableSvc.deleteTable('mytable', function(error, response){
    if(!error){
        // Table deleted
    }
});
```

Pokud si nejste jisti, zda text hello tabulka existuje, použijte **deleteTableIfExists**.

## <a name="use-continuation-tokens"></a>Použít pokračování tokeny
Když dotazujete tabulky pro velké objemy výsledky, vyhledejte pokračování tokeny. K dispozici pro dotaz, možná zjistíte, pokud jste nevytvářejte toorecognize při token pokračování je k dispozici může být velké objemy dat.

objekt Hello výsledky vrácené během dotazování sady entit `continuationToken` vlastnost, pokud takový token je k dispozici. Pak můžete toto při provádění dotaz toocontinue toomove napříč hello oddílu a tabulka entity.

Při dotazování, může být zadán parametr continuationToken mezi instance objektu hello dotazu a funkce zpětného volání hello:

```nodejs
var nextContinuationToken = null;
dc.table.queryEntities(tableName,
    query,
    nextContinuationToken,
    function (error, results) {
        if (error) throw error;

        // iterate through results.entries with results

        if (results.continuationToken) {
            nextContinuationToken = results.continuationToken;
        }

    });
```

Je-li si prohlédnout hello `continuationToken` objekt, zjistíte, vlastnosti, jako `nextPartitionKey`, `nextRowKey` a `targetLocation`, což může být použité tooiterate prostřednictvím všechny výsledky hello.

Je také ukázku pokračování v úložišti Azure Storage Node.js hello na Githubu. Vyhledejte `examples/samples/continuationsample.js`.

## <a name="work-with-shared-access-signatures"></a>Práce s podpisy sdíleného přístupu
Sdílené přístupové podpisy (SAS) jsou bezpečný tooprovide granulární přístup tootables bez zadání názvu účtu úložiště nebo klíče. SAS jsou často používané tooprovide omezený přístup tooyour data, například povolení mobilní aplikace tooquery záznamy.

Důvěryhodné aplikace, jako je Cloudová služba vygeneruje SAS pomocí hello **generateSharedAccessSignature** z hello **TableService**a poskytuje ji tooan nedůvěryhodné nebo částečně důvěryhodné aplikace například mobilní aplikace. Hello SAS je generována pomocí zásad, která popisuje hello počáteční a koncové datum během které hello SAS je platný, a také hello držitel SAS úrovně toohello udělí přístup.

Hello následující příklad vytvoří nové zásady sdíleného přístupu, který vám umožní hello SAS držitel tooquery (r) hello tabulky a vyprší platnost 100 minut po času hello, je vytvořena.

```nodejs
var startDate = new Date();
var expiryDate = new Date(startDate);
expiryDate.setMinutes(startDate.getMinutes() + 100);
startDate.setMinutes(startDate.getMinutes() - 100);

var sharedAccessPolicy = {
  AccessPolicy: {
    Permissions: azure.TableUtilities.SharedAccessPermissions.QUERY,
    Start: startDate,
    Expiry: expiryDate
  },
};

var tableSAS = tableSvc.generateSharedAccessSignature('mytable', sharedAccessPolicy);
var host = tableSvc.host;
```

Všimněte si, že informace o hostiteli hello je nutno zadat také, jako je povinný při hello SAS držitel pokusí tooaccess hello tabulky.

Hello klientskou aplikaci, pak používá hello SAS s **TableServiceWithSAS** tooperform operace s tabulkou hello. Následující ukázka Hello připojí toohello tabulky a provádí dotazu.

```nodejs
var sharedTableService = azure.createTableServiceWithSas(host, tableSAS);
var query = azure.TableQuery()
  .where('PartitionKey eq ?', 'hometasks');

sharedTableService.queryEntities(query, null, function(error, result, response) {
  if(!error) {
    // result contains hello entities
  }
});
```

Vzhledem k tomu, že hello SAS se vygeneroval s dotazu přístup jenom, pokud byly proveden pokus o tooinsert, aktualizace nebo odstranění entity, bude vrácena chyba.

### <a name="access-control-lists"></a>Seznamy řízení přístupu
Můžete taky zásady přístupu hello tooset seznamu řízení přístupu (ACL) pro SAS. To je užitečné, pokud chcete tooallow více klientů tooaccess hello tabulky, ale poskytnutí zásad, jiný přístup pro každého klienta.

Seznam ACL je implementovaná pomocí pole zásady přístupu s ID spojené s každou zásadu. Následující ukázka Hello definuje dvě zásady, jeden pro "uživatel1" a jeden pro, uživatel2":

```nodejs
var sharedAccessPolicy = {
  user1: {
    Permissions: azure.TableUtilities.SharedAccessPermissions.QUERY,
    Start: startDate,
    Expiry: expiryDate
  },
  user2: {
    Permissions: azure.TableUtilities.SharedAccessPermissions.ADD,
    Start: startDate,
    Expiry: expiryDate
  }
};
```

Následující příklad získá Hello hello aktuální seznam ACL pro hello **hometasks** tabulky a potom se přidají nové zásady hello pomocí **setTableAcl**. Tento přístup umožňuje:

```nodejs
var extend = require('extend');
tableSvc.getTableAcl('hometasks', function(error, result, response) {
if(!error){
    var newSignedIdentifiers = extend(true, result.signedIdentifiers, sharedAccessPolicy);
    tableSvc.setTableAcl('hometasks', newSignedIdentifiers, function(error, result, response){
      if(!error){
        // ACL set
      }
    });
  }
});
```

Jednou hello byla nastavena seznamu ACL, potom můžete vytvořit na základě hello ID pro zásadu SAS. Hello následující příklad vytvoří nový SAS pro, uživatel2":

```nodejs
tableSAS = tableSvc.generateSharedAccessSignature('hometasks', { Id: 'user2' });
```

## <a name="next-steps"></a>Další kroky
Další informace najdete v tématu hello následující prostředky.

* [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) je samostatná aplikace, volná, od společnosti Microsoft, která vám umožní toowork vizuálně s daty Azure Storage ve Windows, systému macOS a Linux.
* [Azure SDK úložiště pro uzel](https://github.com/Azure/azure-storage-node) úložišti na Githubu.
* [Středisko pro vývojáře Node.js](/develop/nodejs/)
* [Vytvoření a nasazení tooan aplikace Node.js webu Azure](../app-service-web/app-service-web-get-started-nodejs.md)
