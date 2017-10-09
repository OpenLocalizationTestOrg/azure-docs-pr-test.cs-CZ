## <span data-ttu-id="e2656-101"><a name="create-client"></a>Vytvoření připojení klienta</span><span class="sxs-lookup"><span data-stu-id="e2656-101"><a name="create-client"></a>Create a client connection</span></span>
<span data-ttu-id="e2656-102">Vytvořte připojení klienta tak, že vytvoříte objekt `WindowsAzure.MobileServiceClient`.</span><span class="sxs-lookup"><span data-stu-id="e2656-102">Create a client connection by creating a `WindowsAzure.MobileServiceClient` object.</span></span>  <span data-ttu-id="e2656-103">Nahraďte `appUrl` s tooyour adresa URL mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="e2656-103">Replace `appUrl` with the URL tooyour Mobile App.</span></span>

```
var client = WindowsAzure.MobileServiceClient(appUrl);
```

## <span data-ttu-id="e2656-104"><a name="table-reference"></a>Práce s tabulkami</span><span class="sxs-lookup"><span data-stu-id="e2656-104"><a name="table-reference"></a>Work with tables</span></span>
<span data-ttu-id="e2656-105">tooaccess nebo aktualizace dat, vytvořit tabulku odkaz toohello back-end.</span><span class="sxs-lookup"><span data-stu-id="e2656-105">tooaccess or update data, create a reference toohello backend table.</span></span> <span data-ttu-id="e2656-106">Nahraďte `tableName` s názvem hello tabulky</span><span class="sxs-lookup"><span data-stu-id="e2656-106">Replace `tableName` with hello name of your table</span></span>

```
var table = client.getTable(tableName);
```

<span data-ttu-id="e2656-107">Jakmile budete mít odkaz na tabulku, můžete s ní dále pracovat:</span><span class="sxs-lookup"><span data-stu-id="e2656-107">Once you have a table reference, you can work further with your table:</span></span>

* [<span data-ttu-id="e2656-108">Dotazování na tabulku</span><span class="sxs-lookup"><span data-stu-id="e2656-108">Query a Table</span></span>](#querying)
  * [<span data-ttu-id="e2656-109">Filtrování dat</span><span class="sxs-lookup"><span data-stu-id="e2656-109">Filtering Data</span></span>](#table-filter)
  * [<span data-ttu-id="e2656-110">Procházení dat po stránkách</span><span class="sxs-lookup"><span data-stu-id="e2656-110">Paging through Data</span></span>](#table-paging)
  * [<span data-ttu-id="e2656-111">Řazení dat</span><span class="sxs-lookup"><span data-stu-id="e2656-111">Sorting Data</span></span>](#sorting-data)
* [<span data-ttu-id="e2656-112">Vkládání dat</span><span class="sxs-lookup"><span data-stu-id="e2656-112">Inserting Data</span></span>](#inserting)
* [<span data-ttu-id="e2656-113">Úprava dat</span><span class="sxs-lookup"><span data-stu-id="e2656-113">Modifying Data</span></span>](#modifying)
* [<span data-ttu-id="e2656-114">Odstranění dat</span><span class="sxs-lookup"><span data-stu-id="e2656-114">Deleting Data</span></span>](#deleting)

### <span data-ttu-id="e2656-115"><a name="querying"></a>Postup: Dotazování odkazu na tabulku</span><span class="sxs-lookup"><span data-stu-id="e2656-115"><a name="querying"></a>How to: Query a table reference</span></span>
<span data-ttu-id="e2656-116">Až budete mít odkaz na tabulky, můžete ho tooquery pro data na serveru hello.</span><span class="sxs-lookup"><span data-stu-id="e2656-116">Once you have a table reference, you can use it tooquery for data on hello server.</span></span>  <span data-ttu-id="e2656-117">Dotazy se sestavují v jazyce podobném jazyku LINQ.</span><span class="sxs-lookup"><span data-stu-id="e2656-117">Queries are made in a "LINQ-like" language.</span></span>
<span data-ttu-id="e2656-118">tooreturn všechna data z tabulky hello, hello použijte následující kód:</span><span class="sxs-lookup"><span data-stu-id="e2656-118">tooreturn all data from hello table, use hello following code:</span></span>

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

<span data-ttu-id="e2656-119">Hello úspěch funkce je volána s výsledky hello.</span><span class="sxs-lookup"><span data-stu-id="e2656-119">hello success function is called with hello results.</span></span>  <span data-ttu-id="e2656-120">Nepoužívejte `for (var i in results)` v hello úspěch fungovat jako bude iterace informace, které je součástí hello výsledky při dalších funkcí dotazu (například `.includeTotalCount()`) se používají.</span><span class="sxs-lookup"><span data-stu-id="e2656-120">Do not use `for (var i in results)` in hello success function as that will iterate over information that is included in hello results when other query functions (such as `.includeTotalCount()`) are used.</span></span>

<span data-ttu-id="e2656-121">Další informace o hello syntaxe dotazů najdete v tématu hello [dotaz na objekt dokumentace].</span><span class="sxs-lookup"><span data-stu-id="e2656-121">For more information on hello Query syntax, see hello [Query object documentation].</span></span>

#### <span data-ttu-id="e2656-122"><a name="table-filter"></a>Filtrování dat na hello server</span><span class="sxs-lookup"><span data-stu-id="e2656-122"><a name="table-filter"></a>Filtering data on hello server</span></span>
<span data-ttu-id="e2656-123">Můžete použít `where` klauzule na odkaz na tabulku hello:</span><span class="sxs-lookup"><span data-stu-id="e2656-123">You can use a `where` clause on hello table reference:</span></span>

```
table
    .where({ userId: user.userId, complete: false })
    .read()
    .then(success, failure);
```

<span data-ttu-id="e2656-124">Můžete také použít funkci, která filtruje hello objektu.</span><span class="sxs-lookup"><span data-stu-id="e2656-124">You can also use a function that filters hello object.</span></span>  <span data-ttu-id="e2656-125">V takovém případě hello `this` proměnné je přiřazena toothe filtrované aktuální objekt.</span><span class="sxs-lookup"><span data-stu-id="e2656-125">In this case, hello `this` variable is assigned toothe current object being filtered.</span></span>  <span data-ttu-id="e2656-126">Následující kód Hello je funkčně srovnatelný toohello předchozí příklad:</span><span class="sxs-lookup"><span data-stu-id="e2656-126">hello following code is functionally equivalent toohello prior example:</span></span>

```
function filterByUserId(currentUserId) {
    return this.userId === currentUserId && this.complete === false;
}

table
    .where(filterByUserId, user.userId)
    .read()
    .then(success, failure);
```

#### <span data-ttu-id="e2656-127"><a name="table-paging"></a>Procházení dat po stránkách</span><span class="sxs-lookup"><span data-stu-id="e2656-127"><a name="table-paging"></a>Paging through data</span></span>
<span data-ttu-id="e2656-128">Využívat hello `take()` a `skip()` metody.</span><span class="sxs-lookup"><span data-stu-id="e2656-128">Utilize hello `take()` and `skip()` methods.</span></span>  <span data-ttu-id="e2656-129">Pokud například chcete toosplit hello tabulky do řádek 100 záznamů:</span><span class="sxs-lookup"><span data-stu-id="e2656-129">For example, if you wish toosplit hello table into 100-row records:</span></span>

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

<span data-ttu-id="e2656-130">Hello `.includeTotalCount()` metoda je použité tooadd objekt totalCount pole toohello výsledky.</span><span class="sxs-lookup"><span data-stu-id="e2656-130">hello `.includeTotalCount()` method is used tooadd a totalCount field toohello results object.</span></span>  <span data-ttu-id="e2656-131">Pole totalCount je vyplněn hello celkový počet záznamů, které by byla vrácena, pokud žádné stránkování.</span><span class="sxs-lookup"><span data-stu-id="e2656-131">The totalCount field is filled with hello total number of records that would be returned if no paging is used.</span></span>

<span data-ttu-id="e2656-132">Potom můžete pomocí proměnné stránky hello a některé tooprovide tlačítka uživatelského rozhraní stránky seznamu; použít `loadPage()` načíst hello nové záznamy pro jednotlivé stránky.</span><span class="sxs-lookup"><span data-stu-id="e2656-132">You can then use hello pages variable and some UI buttons tooprovide a page list; use `loadPage()` to load hello new records for each page.</span></span>  <span data-ttu-id="e2656-133">Implementujte ukládání do mezipaměti toorecords toospeed přístupu, které už byly načteny.</span><span class="sxs-lookup"><span data-stu-id="e2656-133">Implement caching toospeed access toorecords that have already been loaded.</span></span>

#### <span data-ttu-id="e2656-134"><a name="sorting-data"></a>Postup: Vrácení seřazených dat</span><span class="sxs-lookup"><span data-stu-id="e2656-134"><a name="sorting-data"></a>How to: Return sorted data</span></span>
<span data-ttu-id="e2656-135">Použití hello `.orderBy()` nebo `.orderByDescending()` dotaz metody:</span><span class="sxs-lookup"><span data-stu-id="e2656-135">Use hello `.orderBy()` or `.orderByDescending()` query methods:</span></span>

```
table
    .orderBy('name')
    .read()
    .then(success, failure);
```

<span data-ttu-id="e2656-136">Další informace o objektu dotazu hello najdete v tématu hello [dotaz na objekt dokumentace].</span><span class="sxs-lookup"><span data-stu-id="e2656-136">For more information on hello Query object, see hello [Query object documentation].</span></span>

### <span data-ttu-id="e2656-137"><a name="inserting"></a>Postup: Vkládání dat</span><span class="sxs-lookup"><span data-stu-id="e2656-137"><a name="inserting"></a>How to: Insert data</span></span>
<span data-ttu-id="e2656-138">Vytvořte objekt jazyka JavaScript s hello příslušná data a volání `table.insert()` asynchronně:</span><span class="sxs-lookup"><span data-stu-id="e2656-138">Create a JavaScript object with hello appropriate date and call `table.insert()` asynchronously:</span></span>

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

<span data-ttu-id="e2656-139">V úspěšné vložení je vrácena hello vložit položky s hello další pole, které jsou požadovány pro synchronizační operace.</span><span class="sxs-lookup"><span data-stu-id="e2656-139">On successful insertion, hello inserted item is returned with hello additional fields that are required for sync operations.</span></span>  <span data-ttu-id="e2656-140">Aktualizujte tyto informace ve vlastní mezipaměti pro pozdější aktualizace.</span><span class="sxs-lookup"><span data-stu-id="e2656-140">Update your own cache with this information for later updates.</span></span>

<span data-ttu-id="e2656-141">Hello Azure Mobile Apps Node.js serveru SDK podporuje dynamické schématu pro účely vývoje.</span><span class="sxs-lookup"><span data-stu-id="e2656-141">hello Azure Mobile Apps Node.js Server SDK supports dynamic schema for development purposes.</span></span>  <span data-ttu-id="e2656-142">Dynamické schéma umožňuje tooadd sloupce toohello tabulku zadáním v operace insert nebo update.</span><span class="sxs-lookup"><span data-stu-id="e2656-142">Dynamic Schema allows you tooadd columns toohello table by specifying them in an insert or update operation.</span></span>  <span data-ttu-id="e2656-143">Doporučujeme, abyste před přesunutím vaší aplikace tooproduction dynamické schématu vypnout.</span><span class="sxs-lookup"><span data-stu-id="e2656-143">We recommend that you turn off dynamic schema before moving your application tooproduction.</span></span>

### <span data-ttu-id="e2656-144"><a name="modifying"></a>Postup: Úprava dat</span><span class="sxs-lookup"><span data-stu-id="e2656-144"><a name="modifying"></a>How to: Modify data</span></span>
<span data-ttu-id="e2656-145">Podobně jako toohello `.insert()` metodu, musí vytvořit objekt aktualizace a pak zavolají `.update()`.</span><span class="sxs-lookup"><span data-stu-id="e2656-145">Similar toohello `.insert()` method, you should create an Update object and then call `.update()`.</span></span>  <span data-ttu-id="e2656-146">Hello objekt aktualizací musí obsahovat hello ID záznamu toobe hello aktualizovat - při čtení záznamu hello nebo při volání metody je získat hello ID `.insert()`.</span><span class="sxs-lookup"><span data-stu-id="e2656-146">hello update object must contain hello ID of hello record toobe updated - hello ID is obtained when reading hello record or when calling `.insert()`.</span></span>

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

### <span data-ttu-id="e2656-147"><a name="deleting"></a>Postup: Odstranění dat</span><span class="sxs-lookup"><span data-stu-id="e2656-147"><a name="deleting"></a>How to: Delete data</span></span>
<span data-ttu-id="e2656-148">toodelete záznam, volání hello `.del()` metoda.</span><span class="sxs-lookup"><span data-stu-id="e2656-148">toodelete a record, call hello `.del()` method.</span></span>  <span data-ttu-id="e2656-149">Odkaz na objekt předejte hello ID:</span><span class="sxs-lookup"><span data-stu-id="e2656-149">Pass hello ID in an object reference:</span></span>

```
table
    .del({ id: '7163bc7a-70b2-4dde-98e9-8818969611bd' })
    .done(function () {
        // Record is now deleted - update your cache
    }, failure);
```
