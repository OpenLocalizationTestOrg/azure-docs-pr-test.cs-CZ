## <a name="repeatability-during-copy"></a><span data-ttu-id="b47b1-101">Opakovatelnosti během kopírování</span><span class="sxs-lookup"><span data-stu-id="b47b1-101">Repeatability during Copy</span></span>
<span data-ttu-id="b47b1-102">Při kopírování dat tooAzure SQL nebo SQL Server z jiných dat ukládá jeden opakovatelnosti tookeep potřebám v paměti tooavoid nezamýšleným výstupy.</span><span class="sxs-lookup"><span data-stu-id="b47b1-102">When copying data tooAzure SQL/SQL Server from other data stores one needs tookeep repeatability in mind tooavoid unintended outcomes.</span></span> 

<span data-ttu-id="b47b1-103">Při kopírování dat tooAzure databázi SQL nebo SQL Server, bude aktivitou kopírování pomocí výchozí připojení hello datové sady toohello podřízený tabulka ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="b47b1-103">When copying data tooAzure SQL/SQL Server Database, copy activity will by default APPEND hello data set toohello sink table by default.</span></span> <span data-ttu-id="b47b1-104">Například při kopírování dat ze zdroje souboru CSV (data hodnot oddělených čárkami) obsahující dva zaznamenává tooAzure databázi SQL nebo SQL Server, to je co hello tabulka vypadá jako:</span><span class="sxs-lookup"><span data-stu-id="b47b1-104">For example, when copying data from a CSV (comma separated values data) file source containing two records tooAzure SQL/SQL Server Database, this is what hello table looks like:</span></span>

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    2            2015-05-01 00:00:00
```

<span data-ttu-id="b47b1-105">Předpokládejme, že nalezlo chyby v zdrojový soubor a množství aktualizované hello zkumavky dolů z 2 too4 hello zdrojový soubor.</span><span class="sxs-lookup"><span data-stu-id="b47b1-105">Suppose you found errors in source file and updated hello quantity of Down Tube from 2 too4 in hello source file.</span></span> <span data-ttu-id="b47b1-106">Pokud znovu spustíte hello datový řez pro toto období, zjistíte, že dva nové záznamy připojí tooAzure databázi SQL nebo SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b47b1-106">If you re-run hello data slice for that period, you’ll find two new records appended tooAzure SQL/SQL Server Database.</span></span> <span data-ttu-id="b47b1-107">Hello níže předpokládá, že žádná z hello sloupců v tabulce hello nemá hello omezení primárního klíče.</span><span class="sxs-lookup"><span data-stu-id="b47b1-107">hello below assumes none of hello columns in hello table have hello primary key constraint.</span></span>

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    2            2015-05-01 00:00:00
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    4            2015-05-01 00:00:00
```

<span data-ttu-id="b47b1-108">tooavoid, budete potřebovat toospecify UPSERT sémantiku s využitím mezi hello pod 2 mechanismy, které jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="b47b1-108">tooavoid this, you will need toospecify UPSERT semantics by leveraging one of hello below 2 mechanisms stated below.</span></span>

> [!NOTE]
> <span data-ttu-id="b47b1-109">Řez opětovně lze spustit automaticky v Azure Data Factory podle určené zásady opakování hello.</span><span class="sxs-lookup"><span data-stu-id="b47b1-109">A slice can be re-run automatically in Azure Data Factory as per hello retry policy specified.</span></span>
> 
> 

### <a name="mechanism-1"></a><span data-ttu-id="b47b1-110">Mechanismus 1</span><span class="sxs-lookup"><span data-stu-id="b47b1-110">Mechanism 1</span></span>
<span data-ttu-id="b47b1-111">Můžete využít **sqlWriterCleanupScript** toofirst vlastnost provedení čištění akce při spuštění řez.</span><span class="sxs-lookup"><span data-stu-id="b47b1-111">You can leverage **sqlWriterCleanupScript** property toofirst perform cleanup action when a slice is run.</span></span> 

```json
"sink":  
{ 
  "type": "SqlSink", 
  "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM table WHERE ModifiedDate >= \\'{0:yyyy-MM-dd HH:mm}\\' AND ModifiedDate < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
}
```

<span data-ttu-id="b47b1-112">Hello skript pro vyčištění by byl proveden první během kopírování pro danou řez, který by odstranit hello data z hello tabulky SQL odpovídající toothat řez.</span><span class="sxs-lookup"><span data-stu-id="b47b1-112">hello cleanup script would be executed first during copy for a given slice which would delete hello data from hello SQL Table corresponding toothat slice.</span></span> <span data-ttu-id="b47b1-113">Hello aktivity budou následně insert hello tabulky SQL hello data.</span><span class="sxs-lookup"><span data-stu-id="b47b1-113">hello activity will subsequently insert hello data into hello SQL Table.</span></span> 

<span data-ttu-id="b47b1-114">Pokud hello řez je nyní spusťte znovu a potom můžete zjistí, že množství hello je aktualizovat, protože požadovaný.</span><span class="sxs-lookup"><span data-stu-id="b47b1-114">If hello slice is now re-run, then you will find hello quantity is updated as desired.</span></span>

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    4            2015-05-01 00:00:00
```

<span data-ttu-id="b47b1-115">Předpokládejme, že hello ploché podložku záznam se odebere z původní csv hello.</span><span class="sxs-lookup"><span data-stu-id="b47b1-115">Suppose hello Flat Washer record is removed from hello original csv.</span></span> <span data-ttu-id="b47b1-116">Opětovným spuštěním řez hello by pak vytváří hello následující výsledek:</span><span class="sxs-lookup"><span data-stu-id="b47b1-116">Then re-running hello slice would produce hello following result:</span></span> 

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
7     Down Tube    4            2015-05-01 00:00:00
```
<span data-ttu-id="b47b1-117">Nic nového měl toobe Hotovo.</span><span class="sxs-lookup"><span data-stu-id="b47b1-117">Nothing new had toobe done.</span></span> <span data-ttu-id="b47b1-118">Aktivita kopírování Hello spustili hello čištění toodelete hello odpovídající data skriptu pro tento řez.</span><span class="sxs-lookup"><span data-stu-id="b47b1-118">hello copy activity ran hello cleanup script toodelete hello corresponding data for that slice.</span></span> <span data-ttu-id="b47b1-119">Pak jej číst hello vstup ze souboru csv hello (což je obsaženy pouze 1 záznam) a vložit do hello tabulky.</span><span class="sxs-lookup"><span data-stu-id="b47b1-119">Then it read hello input from hello csv (which then contained only 1 record) and inserted it into hello Table.</span></span> 

### <a name="mechanism-2"></a><span data-ttu-id="b47b1-120">Mechanismus 2</span><span class="sxs-lookup"><span data-stu-id="b47b1-120">Mechanism 2</span></span>
> [!IMPORTANT]
> <span data-ttu-id="b47b1-121">sliceIdentifierColumnName není v současné době podporovaný pro Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="b47b1-121">sliceIdentifierColumnName is not supported for Azure SQL Data Warehouse at this time.</span></span> 

<span data-ttu-id="b47b1-122">Jiný mechanismus tooachieve opakovatelnosti spočívá v použití vyhrazených sloupec (**sliceIdentifierColumnName**) v hello cílové tabulky.</span><span class="sxs-lookup"><span data-stu-id="b47b1-122">Another mechanism tooachieve repeatability is by having a dedicated column (**sliceIdentifierColumnName**) in hello target Table.</span></span> <span data-ttu-id="b47b1-123">V tomto sloupci se použije pomocí Azure Data Factory tooensure hello zdrojové a cílové synchronní.</span><span class="sxs-lookup"><span data-stu-id="b47b1-123">This column would be used by Azure Data Factory tooensure hello source and destination stay synchronized.</span></span> <span data-ttu-id="b47b1-124">Tento přístup funguje, když je flexibilitu při změně nebo definice schématu tabulky SQL cílové hello.</span><span class="sxs-lookup"><span data-stu-id="b47b1-124">This approach works when there is flexibility in changing or defining hello destination SQL Table schema.</span></span> 

<span data-ttu-id="b47b1-125">V tomto sloupci se použije službou Azure Data Factory pro účely opakovatelnost a v procesu hello Azure Data Factory nebude mít žádné schéma změny toohello tabulky.</span><span class="sxs-lookup"><span data-stu-id="b47b1-125">This column would be used by Azure Data Factory for repeatability purposes and in hello process Azure Data Factory will not make any schema changes toohello Table.</span></span> <span data-ttu-id="b47b1-126">Způsob toouse tohoto přístupu:</span><span class="sxs-lookup"><span data-stu-id="b47b1-126">Way toouse this approach:</span></span>

1. <span data-ttu-id="b47b1-127">Definujte sloupce binárního typu (32) v cílovém hello tabulky SQL.</span><span class="sxs-lookup"><span data-stu-id="b47b1-127">Define a column of type binary (32) in hello destination SQL Table.</span></span> <span data-ttu-id="b47b1-128">Měla by existovat žádné omezení na tomto sloupci.</span><span class="sxs-lookup"><span data-stu-id="b47b1-128">There should be no constraints on this column.</span></span> <span data-ttu-id="b47b1-129">V tomto příkladu budeme název v tomto sloupci jako 'ColumnForADFuseOnly'.</span><span class="sxs-lookup"><span data-stu-id="b47b1-129">Let's name this column as ‘ColumnForADFuseOnly’ for this example.</span></span>
2. <span data-ttu-id="b47b1-130">Ho použijte k aktivitě kopírování hello následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="b47b1-130">Use it in hello copy activity as follows:</span></span>
   
    ```json
    "sink":  
    { 
   
        "type": "SqlSink", 
        "sliceIdentifierColumnName": "ColumnForADFuseOnly"
    }
    ```

<span data-ttu-id="b47b1-131">Azure Data Factory bude naplnit tento sloupec podle jeho nutné tooensure hello zdrojové a cílové zůstane synchronizovaná.</span><span class="sxs-lookup"><span data-stu-id="b47b1-131">Azure Data Factory will populate this column as per its need tooensure hello source and destination stay synchronized.</span></span> <span data-ttu-id="b47b1-132">mimo tento kontext hello uživatele by neměl používat Hello hodnoty daného sloupce.</span><span class="sxs-lookup"><span data-stu-id="b47b1-132">hello values of this column should not be used outside of this context by hello user.</span></span> 

<span data-ttu-id="b47b1-133">Podobně jako toomechanism 1, aktivitě kopírování se automaticky první vyčištění hello dat pro hello zadané řez z cílového hello tabulky SQL a poté spusťte aktivitu kopírování hello normálně tooinsert hello data ze zdroje toodestination pro tento řez.</span><span class="sxs-lookup"><span data-stu-id="b47b1-133">Similar toomechanism 1, Copy Activity will automatically first clean up hello data for hello given slice from hello destination SQL Table and then run hello copy activity normally tooinsert hello data from source toodestination for that slice.</span></span> 

