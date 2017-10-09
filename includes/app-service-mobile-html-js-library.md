## <a name="create-client"></a>Vytvoření připojení klienta
Vytvořte připojení klienta tak, že vytvoříte objekt `WindowsAzure.MobileServiceClient`.  Nahraďte `appUrl` s tooyour adresa URL mobilní aplikace.

```
var client = WindowsAzure.MobileServiceClient(appUrl);
```

## <a name="table-reference"></a>Práce s tabulkami
tooaccess nebo aktualizace dat, vytvořit tabulku odkaz toohello back-end. Nahraďte `tableName` s názvem hello tabulky

```
var table = client.getTable(tableName);
```

Jakmile budete mít odkaz na tabulku, můžete s ní dále pracovat:

* [Dotazování na tabulku](#querying)
  * [Filtrování dat](#table-filter)
  * [Procházení dat po stránkách](#table-paging)
  * [Řazení dat](#sorting-data)
* [Vkládání dat](#inserting)
* [Úprava dat](#modifying)
* [Odstranění dat](#deleting)

### <a name="querying"></a>Postup: Dotazování odkazu na tabulku
Až budete mít odkaz na tabulky, můžete ho tooquery pro data na serveru hello.  Dotazy se sestavují v jazyce podobném jazyku LINQ.
tooreturn všechna data z tabulky hello, hello použijte následující kód:

```
/**
 * Process hello results that are received by a call tootable.read()
 *
 * @param {Object} results hello results as a pseudo-array
 * @param {int} results.length hello length of hello results array
 * @param {Object} results[] hello individual results
 */
function success(results) {
   var numItemsRead = results.length;

   for (var i = 0 ; i < results.length ; i++) {
       var row = results[i];
       // Each row is an object - hello properties are hello columns
   }
}

function failure(error) {
    throw new Error('Error loading data: ', error);
}

table
    .read()
    .then(success, failure);
```

Hello úspěch funkce je volána s výsledky hello.  Nepoužívejte `for (var i in results)` v hello úspěch fungovat jako bude iterace informace, které je součástí hello výsledky při dalších funkcí dotazu (například `.includeTotalCount()`) se používají.

Další informace o hello syntaxe dotazů najdete v tématu hello [dotaz na objekt dokumentace].

#### <a name="table-filter"></a>Filtrování dat na hello server
Můžete použít `where` klauzule na odkaz na tabulku hello:

```
table
    .where({ userId: user.userId, complete: false })
    .read()
    .then(success, failure);
```

Můžete také použít funkci, která filtruje hello objektu.  V takovém případě hello `this` proměnné je přiřazena toothe filtrované aktuální objekt.  Následující kód Hello je funkčně srovnatelný toohello předchozí příklad:

```
function filterByUserId(currentUserId) {
    return this.userId === currentUserId && this.complete === false;
}

table
    .where(filterByUserId, user.userId)
    .read()
    .then(success, failure);
```

#### <a name="table-paging"></a>Procházení dat po stránkách
Využívat hello `take()` a `skip()` metody.  Pokud například chcete toosplit hello tabulky do řádek 100 záznamů:

```
var totalCount = 0, pages = 0;

// Step 1 - get hello total number of records
table.includeTotalCount().take(0).read(function (results) {
    totalCount = results.totalCount;
    pages = Math.floor(totalCount/100) + 1;
    loadPage(0);
}, failure);

function loadPage(pageNum) {
    let skip = pageNum * 100;
    table.skip(skip).take(100).read(function (results) {
        for (var i = 0 ; i < results.length ; i++) {
            var row = results[i];
            // Process each row
        }
    }
}
```

Hello `.includeTotalCount()` metoda je použité tooadd objekt totalCount pole toohello výsledky.  Pole totalCount je vyplněn hello celkový počet záznamů, které by byla vrácena, pokud žádné stránkování.

Potom můžete pomocí proměnné stránky hello a některé tooprovide tlačítka uživatelského rozhraní stránky seznamu; použít `loadPage()` načíst hello nové záznamy pro jednotlivé stránky.  Implementujte ukládání do mezipaměti toorecords toospeed přístupu, které už byly načteny.

#### <a name="sorting-data"></a>Postup: Vrácení seřazených dat
Použití hello `.orderBy()` nebo `.orderByDescending()` dotaz metody:

```
table
    .orderBy('name')
    .read()
    .then(success, failure);
```

Další informace o objektu dotazu hello najdete v tématu hello [dotaz na objekt dokumentace].

### <a name="inserting"></a>Postup: Vkládání dat
Vytvořte objekt jazyka JavaScript s hello příslušná data a volání `table.insert()` asynchronně:

```javascript
var newItem = {
    name: 'My Name',
    signupDate: new Date()
};

table
    .insert(newItem)
    .done(function (insertedItem) {
        var id = insertedItem.id;
    }, failure);
```

V úspěšné vložení je vrácena hello vložit položky s hello další pole, které jsou požadovány pro synchronizační operace.  Aktualizujte tyto informace ve vlastní mezipaměti pro pozdější aktualizace.

Hello Azure Mobile Apps Node.js serveru SDK podporuje dynamické schématu pro účely vývoje.  Dynamické schéma umožňuje tooadd sloupce toohello tabulku zadáním v operace insert nebo update.  Doporučujeme, abyste před přesunutím vaší aplikace tooproduction dynamické schématu vypnout.

### <a name="modifying"></a>Postup: Úprava dat
Podobně jako toohello `.insert()` metodu, musí vytvořit objekt aktualizace a pak zavolají `.update()`.  Hello objekt aktualizací musí obsahovat hello ID záznamu toobe hello aktualizovat - při čtení záznamu hello nebo při volání metody je získat hello ID `.insert()`.

```javascript
var updateItem = {
    id: '7163bc7a-70b2-4dde-98e9-8818969611bd',
    name: 'My New Name'
};

table
    .update(updateItem)
    .done(function (updatedItem) {
        // You can now update your cached copy
    }, failure);
```

### <a name="deleting"></a>Postup: Odstranění dat
toodelete záznam, volání hello `.del()` metoda.  Odkaz na objekt předejte hello ID:

```
table
    .del({ id: '7163bc7a-70b2-4dde-98e9-8818969611bd' })
    .done(function () {
        // Record is now deleted - update your cache
    }, failure);
```
