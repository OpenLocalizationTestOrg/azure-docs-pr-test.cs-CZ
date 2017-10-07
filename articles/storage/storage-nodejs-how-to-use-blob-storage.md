---
title: "aaaHow toouse úložiště objektů Blob z Node.js | Microsoft Docs"
description: "Ukládání nestrukturovaných dat v cloudu hello s Azure Blob storage (úložiště objektů)."
services: storage
documentationcenter: nodejs
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 8b0df222-1ca8-4967-8248-6d6d720947b8
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 12/08/2016
ms.author: marsma
ms.openlocfilehash: e405eecdc60cd1eaa77510e7b29b41269372b65e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-blob-storage-from-nodejs"></a>Jak toouse úložiště objektů Blob z Node.js
[!INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-check-out-samples-all](../../includes/storage-check-out-samples-all.md)]

## <a name="overview"></a>Přehled
Tento článek ukazuje, jak tooperform běžné scénáře s využitím úložiště objektů Blob. Ukázky Hello jsou zapsány pomocí rozhraní API Node.js hello. pokryté scénáře Hello zahrnují jak tooupload, seznam, stahování a odstraňování objektů BLOB.

[!INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-nodejs-application"></a>Vytvoření aplikace Node.js
Pokyny najdete v části toocreate aplikace Node.js, [vytvoření webové aplikace Node.js ve službě Azure App Service], [sestavení a nasazení tooan aplikace Node.js Azure Cloud Service] – pomocí prostředí Windows PowerShell , nebo [sestavení a nasazení tooAzure webové aplikace Node.js, pomocí Web Matrix].

## <a name="configure-your-application-tooaccess-storage"></a>Konfigurace úložiště tooaccess vaší aplikace
toouse úložiště Azure, musíte hello sada SDK úložiště Azure pro platformu Node.js, která obsahuje sadu knihoven pohodlí, které komunikují s služby REST úložiště hello.

### <a name="use-node-package-manager-npm-tooobtain-hello-package"></a>Použijte uzel balíček správce (NPM) tooobtain hello balíček
1. Pomocí rozhraní příkazového řádku, jako například **prostředí PowerShell** (Windows), **Terminálové** (Mac), nebo **Bash** (Unix), toonavigate toohello složku, kde můžete vytvořit ukázky aplikace.
2. Typ **npm nainstalujte azure-storage** v příkazovém okně hello. Výstup z příkazu hello je podobné toohello následující ukázka kódu.

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
3. Můžete ručně spustit hello **ls** tooverify příkaz, **uzlu\_moduly** složka byla vytvořena. V této složce najít hello **azure-storage** balíček, který obsahuje hello knihovny, je nutné, aby tooaccess úložiště.

### <a name="import-hello-package"></a>Importovat balíček hello
Pomocí programu Poznámkový blok nebo jiném textovém editoru, přidejte následující toohello horní části hello hello **server.js** souboru hello aplikace, kde chcete toouse úložiště:

```nodejs
var azure = require('azure-storage');
```

## <a name="set-up-an-azure-storage-connection"></a>Nastavit připojení k Azure Storage
Hello modulu Azure, bude číst proměnné prostředí hello `AZURE_STORAGE_ACCOUNT` a `AZURE_STORAGE_ACCESS_KEY`, nebo `AZURE_STORAGE_CONNECTION_STRING`, informace o požadovaných účtu úložiště Azure tooyour tooconnect. Pokud nejsou nastavené těchto proměnných prostředí, musíte zadat informace o účtu hello při volání metody **createBlobService**.

Příklad nastavení proměnných prostředí hello v hello [portál Azure](https://portal.azure.com) webové aplikace Azure, najdete v části [Node.js webové aplikace pomocí služby Azure Table hello].

## <a name="create-a-container"></a>Vytvoření kontejneru
Hello **BlobService** objekt vám umožňuje spolupracovat s kontejnery a objekty BLOB. Hello následující kód vytvoří **BlobService** objektu. Přidejte následující hello v hello horní části **server.js**:

```nodejs
var blobSvc = azure.createBlobService();
```

> [!NOTE]
> Objekt blob může anonymně přístup pomocí **createBlobServiceAnonymous** a poskytnutím adresa hostitele hello. Například použít `var blobSvc = azure.createBlobServiceAnonymous('https://myblob.blob.core.windows.net/');`.
>
>

[!INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

toocreate nový kontejner, použijte **createContainerIfNotExists**. Hello následující příklad kódu vytvoří nový kontejner s názvem 'můj_kontejner':

```nodejs
blobSvc.createContainerIfNotExists('mycontainer', function(error, result, response){
    if(!error){
      // Container exists and is private
    }
});
```

Pokud je nově vytvořený kontejner hello, `result.created` hodnotu true. Pokud hello kontejneru již existuje, `result.created` je false. `response`obsahuje informace o operaci hello, včetně informací o ETag hello hello kontejneru.

### <a name="container-security"></a>Kontejner zabezpečení
Ve výchozím nastavení nové kontejnery jsou soukromá a nemohou být získat anonymní přístup. kontejner hello toomake veřejné, tak, aby ho může anonymně přístup, můžete nastavit úroveň přístupu kontejneru hello také**blob** nebo **kontejneru**.

* **objekt BLOB** -umožňuje anonymní přístup pro čtení tooblob obsahu a metadat v tomto kontejneru, ale není toocontainer metadata například výpis všech objektů BLOB do kontejneru
* **kontejner** -umožňuje anonymní přístup pro čtení tooblob obsahu a metadat, jakož i metadata kontejneru

Hello následující příklad kódu ukazuje nastavení úrovně přístupu hello příliš**blob**:

```nodejs
blobSvc.createContainerIfNotExists('mycontainer', {publicAccessLevel : 'blob'}, function(error, result, response){
    if(!error){
      // Container exists and allows
      // anonymous read access tooblob
      // content and metadata within this container
    }
});
```

Alternativně můžete upravit úroveň přístupu hello kontejneru pomocí **setContainerAcl** úroveň přístupu toospecify hello. Hello následující kód například změny hello přístup úrovně toocontainer:

```nodejs
blobSvc.setContainerAcl('mycontainer', null /* signedIdentifiers */, {publicAccessLevel : 'container'} /* publicAccessLevel*/, function(error, result, response){
  if(!error){
    // Container access level set too'container'
  }
});
```

Hello výsledek obsahuje informace o operaci hello, včetně aktuální hello **značka ETag** pro kontejner hello.

### <a name="filters"></a>Filtry
Můžete provést volitelný filtrování operací toooperations provést pomocí **BlobService**. Filtrování operací může zahrnovat protokolování, automatickým opakovaným pokusem o atd. Filtry jsou objekty, které implementovat metodu podpisem hello:

```nodejs
function handle (requestOptions, next)
```

Až to předzpracování na možnosti hello požadavku, hello musí metoda toocall tlačítko Další"předání zpětné volání s hello následující podpis:

```nodejs
function (returnObject, finalCallback, next)
```

V této zpětného volání a po zpracování hello returnObject (hello odpověď ze serveru toohello požadavek hello), zpětné volání hello musí tooeither vyvolat další, pokud existuje toocontinue zpracování ostatní filtry nebo jednoduše vyvolat finalCallback tooend hello služby volání.

Dva filtry, které implementují logiku opakovaných pokusů, které jsou součástí hello Azure SDK pro Node.js, **ExponentialRetryPolicyFilter** a **LinearRetryPolicyFilter**. vytvoří následující Hello **BlobService** objekt, který používá hello **ExponentialRetryPolicyFilter**:

```nodejs
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var blobSvc = azure.createBlobService().withFilter(retryOperations);
```

## <a name="upload-a-blob-into-a-container"></a>Nahrání objektu blob do kontejneru
Existují tři typy objektů blob: objekty BLOB bloků, objekty BLOB stránky a doplňovacích objektů BLOB. Objekty BLOB bloku umožňují toomore efektivně odeslání velkého množství dat. Append – objekty BLOB jsou optimalizované pro doplňovací operace. Objekty BLOB stránky jsou optimalizované pro operace čtení a zápisu. Další informace najdete v tématu [Principy objekty BLOB bloku a doplňovacích objektů BLOB, objekty BLOB stránky](http://msdn.microsoft.com/library/azure/ee691964.aspx).

### <a name="block-blobs"></a>Objekty blob bloku
tooupload data tooa objekt blob bloku, použijte hello následující:

* **createBlockBlobFromLocalFile** – vytvoří nový objekt blob bloku a odesílá hello obsah souboru
* **createBlockBlobFromStream** – vytvoří nový objekt blob bloku a odesílá hello obsah datového proudu
* **createBlockBlobFromText** – vytvoří nový objekt blob bloku a odesílá hello obsah řetězce
* **createWriteStreamToBlockBlob** – obsahuje objekt blob bloku tooa zápisu datového proudu

Hello následující příklad kódu odešle hello obsah hello **test.txt** soubor do **můj_objekt_blob**.

```nodejs
blobSvc.createBlockBlobFromLocalFile('mycontainer', 'myblob', 'test.txt', function(error, result, response){
  if(!error){
    // file uploaded
  }
});
```

Hello `result` vrácený tyto metody obsahuje informace o operaci hello, jako je například hello **značka ETag** objektu hello blob.

### <a name="append-blobs"></a>Doplňovací objekty BLOB
tooupload data tooa nové připojení objektů blob, použijte hello:

* **createAppendBlobFromLocalFile** – vytvoří nový doplňovací objekt blob a odesílá hello obsah souboru
* **createAppendBlobFromStream** – vytvoří nový doplňovací objekt blob a odesílá hello obsah datového proudu
* **createAppendBlobFromText** – vytvoří nový doplňovací objekt blob a odesílá hello obsah řetězce
* **createWriteStreamToNewAppendBlob** – vytvoří nový doplňovací objekt blob a pak poskytuje tooit toowrite datového proudu

Hello následující příklad kódu odešle hello obsah hello **test.txt** soubor do **myappendblob**.

```nodejs
blobSvc.createAppendBlobFromLocalFile('mycontainer', 'myappendblob', 'test.txt', function(error, result, response){
  if(!error){
    // file uploaded
  }
});
```

tooappend tooan bloku existující připojení objektů blob, použijte hello následující:

* **appendFromLocalFile** -připojit hello obsah ze souboru tooan existující připojení objektů blob
* **appendFromStream** -připojit hello obsah z datového proudu tooan existující připojení objektů blob
* **appendFromText** -připojit hello obsah z řetězce tooan existující připojení objektů blob
* **appendBlockFromStream** -připojit hello obsah z datového proudu tooan existující připojení objektů blob
* **appendBlockFromText** -připojit hello obsah z řetězce tooan existující připojení objektů blob

> [!NOTE]
> appendFromXXX rozhraní API provede volání nepotřebné serveru rychlé tooavoid toofail některé ověřování na straně klienta. appendBlockFromXXX nebude.
>
>

Hello následující příklad kódu odešle hello obsah hello **test.txt** soubor do **myappendblob**.

```nodejs
blobSvc.appendFromText('mycontainer', 'myappendblob', 'text toobe appended', function(error, result, response){
  if(!error){
    // text appended
  }
});
```

### <a name="page-blobs"></a>Objekty blob stránky
tooupload data tooa objekt blob stránky, použijte hello následující:

* **createPageBlob** -vytvoří nový objekt blob stránky konkrétní délky
* **createPageBlobFromLocalFile** – vytvoří nový objekt blob stránky a odesílá hello obsah souboru
* **createPageBlobFromStream** – vytvoří nový objekt blob stránky a odesílá hello obsah datového proudu
* **createWriteStreamToExistingPageBlob** -poskytuje zápisu datového proudu tooan existující objekt blob stránky
* **createWriteStreamToNewPageBlob** – vytvoří nový objekt blob stránky a pak poskytuje tooit toowrite datového proudu

Hello následující příklad kódu odešle hello obsah hello **test.txt** soubor do **mypageblob**.

```nodejs
blobSvc.createPageBlobFromLocalFile('mycontainer', 'mypageblob', 'test.txt', function(error, result, response){
  if(!error){
    // file uploaded
  }
});
```

> [!NOTE]
> Objekty BLOB stránky obsahovat 512 bajtů stránky. Obdržíte chybu při odesílání dat s velikostí, která není násobkem 512.
>
>

## <a name="list-hello-blobs-in-a-container"></a>Seznam hello objekty BLOB v kontejneru
toolist hello objekty BLOB v kontejneru, použijte hello **listBlobsSegmented** metoda. Pokud chcete objekty BLOB tooreturn s konkrétní předponou, použijte **listBlobsSegmentedWithPrefix**.

```nodejs
blobSvc.listBlobsSegmented('mycontainer', null, function(error, result, response){
  if(!error){
      // result.entries contains hello entries
      // If not all blobs were returned, result.continuationToken has hello continuation token.
  }
});
```

Hello `result` obsahuje `entries` kolekce, která je pole objektů, které popisují jednotlivých objektů blob. Pokud nemůže být vrácen všech objektů BLOB, hello `result` také poskytuje `continuationToken`, které můžete použít jako hello druhý parametr tooretrieve další záznamy.

## <a name="download-blobs"></a>Stáhnout objekty blob
toodownload data z objektu blob, použijte následující hello:

* **getBlobToLocalFile** -zapíše toofile obsah objektu blob hello
* **getBlobToStream** -zapíše hello datového proudu tooa obsah objektu blob
* **getBlobToText** -zapíše obsahu objektu blob hello tooa řetězec
* **createReadStream** -poskytuje tooread datového proudu z objektu blob hello

Hello následující příklad kódu ukazuje, jak pomocí **getBlobToStream** toodownload hello obsah hello **můj_objekt_blob** objektů blob a uložte ho toohello **výstup.txt** souboru pomocí datový proud:

```nodejs
var fs = require('fs');
blobSvc.getBlobToStream('mycontainer', 'myblob', fs.createWriteStream('output.txt'), function(error, result, response){
  if(!error){
    // blob retrieved
  }
});
```

Hello `result` obsahuje informace o objektu blob hello, včetně **značka ETag** informace.

## <a name="delete-a-blob"></a>Odstranění objektu blob
Nakonec toodelete objekt blob, zavolejte **deleteBlob**. Následující ukázka kódu hello odstranění objektů blob s názvem Hello **můj_objekt_blob**.

```nodejs
blobSvc.deleteBlob(containerName, 'myblob', function(error, response){
  if(!error){
    // Blob has been deleted
  }
});
```

## <a name="concurrent-access"></a>Souběžný přístup
blob tooa toosupport souběžný přístup z více klientů nebo více instancí procesu, můžete použít **značky etag binárním rozsáhlým** nebo **zapůjčení**.

* **Značka Etag** -poskytuje toodetect způsobem, který hello objektů blob nebo kontejner, byla změněna jiným procesem
* **Zapůjčení** – poskytuje způsob tooobtain výhradní, obnovitelné, zápis nebo odstranit objekt blob tooa přístup pro v časovém intervalu

### <a name="etag"></a>Značka ETag
Značky etag binárním rozsáhlým použijte, pokud potřebujete tooallow více klientů nebo instancí toowrite toohello blok objektů Blob nebo stránky objektu Blob současně. Hello ETag vám umožní toodetermine Pokud hello kontejner nebo objekt blob byla změněna od začátku čtení nebo vytvořili, což vám umožní tooavoid přepsal změny potvrzeny jiný klienta nebo proces.

Značka ETag podmínky lze nastavit pomocí hello volitelné `options.accessConditions` parametr. Hello následující příklad kódu pouze odešle hello **test.txt** soubor, pokud objekt blob hello již existuje a má hodnotu ETag hello obsahoval podle `etagToMatch`.

```nodejs
blobSvc.createBlockBlobFromLocalFile('mycontainer', 'myblob', 'test.txt', { accessConditions: { EtagMatch: etagToMatch} }, function(error, result, response){
    if(!error){
    // file uploaded
  }
});
```

Při použití značky etag binárním rozsáhlým, je obecný vzor hello:

1. Získáte hello ETag jako výsledek hello create, seznamu nebo operaci get.
2. Provedení akce, kontrola této hello hodnota ETag nebyl změněn.

Pokud byla změněna hello hodnotu, znamená to, že jiný klienta nebo instance upraveny hello blob nebo kontejneru vzhledem k tomu, že jste získali hodnota ETag hello.

### <a name="lease"></a>Zapůjčení
Nové zapůjčení můžete získat pomocí hello **acquireLease** metoda, zadání hello blob nebo kontejner, u nějž chcete zapůjčení pro tooobtain na. Například následující kód hello získá zapůjčení na **můj_objekt_blob**.

```nodejs
blobSvc.acquireLease('mycontainer', 'myblob', function(error, result, response){
  if(!error) {
    console.log('leaseId: ' + result.id);
  }
});
```

Dalších operacích na **můj_objekt_blob** musíte zadat hello `options.leaseId` parametr. Hello zapůjčení ID se vrátí jako `result.id` z **acquireLease**.

> [!NOTE]
> Ve výchozím nastavení je neomezenou dobu trvání zápůjčky hello. Můžete zadat dobu trvání bez nekonečné (mezi 15 a 60 sekund) tím, že poskytuje hello `options.leaseDuration` parametr.
>
>

použít tooremove zapůjčení, **releaseLease**. toobreak zapůjčení, ale zabránit jiným získat nové zapůjčení, dokud nevyprší doba trvání původní text hello, použijte **breakLease**.

## <a name="work-with-shared-access-signatures"></a>Práce s podpisy sdíleného přístupu
Sdílené přístupové podpisy (SAS) jsou bezpečný tooprovide granulární přístup tooblobs a kontejnery bez zadání názvu účtu úložiště nebo klíče. Sdílené přístupové podpisy jsou často používané tooprovide omezený přístup tooyour data, například povolení mobilní aplikace tooaccess objekty BLOB.

> [!NOTE]
> Při můžete povolit také tooblobs anonymní přístup, sdílené přístupové podpisy umožní tooprovide více řízený přístup, protože musíte vygenerovat hello SAS.
>
>

Důvěryhodné aplikace, jako je Cloudová služba generuje sdílené přístupové podpisy pomocí hello **generateSharedAccessSignature** z hello **BlobService**a poskytuje ji tooan nedůvěryhodná nebo částečně důvěryhodné aplikace například mobilní aplikace. Sdílený přístupový podpisy se generují pomocí zásad, která popisuje hello počáteční a koncové datum, během které hello sdílené přístupové podpisy jsou platné, jakož i hello přístup úrovně oprávnění vlastníka pro podpisy sdíleného přístupu toohello.

Hello následující příklad kódu vytvoří nová zásada sdíleného přístupu, která umožňuje čtení hello sdíleného přístupu podpisy držitel tooperform na hello **můj_objekt_blob** objektů blob a jeho platnost vyprší 100 minut po času hello je vytvořen.

```nodejs
var startDate = new Date();
var expiryDate = new Date(startDate);
expiryDate.setMinutes(startDate.getMinutes() + 100);
startDate.setMinutes(startDate.getMinutes() - 100);

var sharedAccessPolicy = {
  AccessPolicy: {
    Permissions: azure.BlobUtilities.SharedAccessPermissions.READ,
    Start: startDate,
    Expiry: expiryDate
  },
};

var blobSAS = blobSvc.generateSharedAccessSignature('mycontainer', 'myblob', sharedAccessPolicy);
var host = blobSvc.host;
```

Všimněte si, že informace o hostiteli hello je nutno zadat také, jako je povinný při hello sdílené přístupové podpisy držitel pokusí tooaccess hello kontejneru.

Hello klientská aplikace pak používá sdílené přístupové podpisy s **BlobServiceWithSAS** tooperform operace u objektu blob hello. Hello následující získá informace **můj_objekt_blob**.

```nodejs
var sharedBlobSvc = azure.createBlobServiceWithSas(host, blobSAS);
sharedBlobSvc.getBlobProperties('mycontainer', 'myblob', function (error, result, response) {
  if(!error) {
    // retrieved info
  }
});
```

Vzhledem k tomu, že hello sdílené přístupové podpisy byly vygenerovány s přístupem jen pro čtení, pokud je proveden pokus o toomodify hello blob, bude vrácena chyba.

### <a name="access-control-lists"></a>Seznamy řízení přístupu
Můžete taky zásad řízení přístupu (ACL) seznamu tooset hello přístup pro SAS. To je užitečné, pokud chcete tooallow více klientů tooaccess kontejner, ale poskytnutí zásad, jiný přístup pro každého klienta.

Seznam ACL je implementovaná pomocí pole zásady přístupu s ID spojené s každou zásadu. Následující ukázka kódu Hello definuje dvě zásady, jeden pro "uživatel1" a jeden pro, uživatel2":

```nodejs
var sharedAccessPolicy = {
  user1: {
    Permissions: azure.BlobUtilities.SharedAccessPermissions.READ,
    Start: startDate,
    Expiry: expiryDate
  },
  user2: {
    Permissions: azure.BlobUtilities.SharedAccessPermissions.WRITE,
    Start: startDate,
    Expiry: expiryDate
  }
};
```

Následující kód například získá Hello hello aktuální seznam ACL pro **můj_kontejner**a potom se přidají nové zásady hello pomocí **setBlobAcl**. Tento přístup umožňuje:

```nodejs
var extend = require('extend');
blobSvc.getBlobAcl('mycontainer', function(error, result, response) {
  if(!error){
    var newSignedIdentifiers = extend(true, result.signedIdentifiers, sharedAccessPolicy);
    blobSvc.setBlobAcl('mycontainer', newSignedIdentifiers, function(error, result, response){
      if(!error){
        // ACL set
      }
    });
  }
});
```

Jednou hello seznamu ACL je nastavena, potom můžete vytvořit sdílené přístupové podpisy, na základě hello ID zásad. Následující ukázka kódu Hello vytvoří nové sdílené přístupové podpisy pro, uživatel2":

```nodejs
blobSAS = blobSvc.generateSharedAccessSignature('mycontainer', { Id: 'user2' });
```

## <a name="next-steps"></a>Další kroky
Další informace najdete v tématu hello následující prostředky.

* [Úložiště Azure SDK pro uzel referenční dokumentace rozhraní API][Azure Storage SDK for Node API Reference]
* [Blog týmu Azure Storage][Azure Storage Team Blog]
* [Azure SDK úložiště pro uzel] [ Azure Storage SDK for Node] úložišti na Githubu
* [Středisko pro vývojáře Node.js](https://azure.microsoft.com/develop/nodejs/)
* [Přenos dat pomocí hello příkazového řádku azcopy](storage-use-azcopy.md)

[Azure Storage SDK for Node]: https://github.com/Azure/azure-storage-node

[vytvoření webové aplikace Node.js ve službě Azure App Service]: ../app-service-web/app-service-web-get-started-nodejs.md
[Node.js webové aplikace pomocí služby Azure Table hello]: ../app-service-web/storage-nodejs-use-table-storage-web-site.md
[sestavení a nasazení tooAzure webové aplikace Node.js, pomocí Web Matrix]: ../app-service-web/web-sites-nodejs-use-webmatrix.md
[Using hello REST API]: http://msdn.microsoft.com/library/azure/hh264518.aspx
[Azure portal]: https://portal.azure.com
[sestavení a nasazení tooan aplikace Node.js Azure Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
[Azure Storage SDK for Node API Reference]: http://dl.windowsazure.com/nodestoragedocs/index.html
