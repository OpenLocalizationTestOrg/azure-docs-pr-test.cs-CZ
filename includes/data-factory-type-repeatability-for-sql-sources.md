## <a name="repeatability-during-copy"></a><span data-ttu-id="2d389-101">Opakovatelnosti během kopírování</span><span class="sxs-lookup"><span data-stu-id="2d389-101">Repeatability during Copy</span></span>
<span data-ttu-id="2d389-102">Při kopírování dat do Azure SQL nebo systému SQL Server z jiných dat ukládá musí jeden opakovatelnosti mějte na paměti, aby se zabránilo neúmyslnému výstupy.</span><span class="sxs-lookup"><span data-stu-id="2d389-102">When copying data to Azure SQL/SQL Server from other data stores one needs to keep repeatability in mind to avoid unintended outcomes.</span></span> 

<span data-ttu-id="2d389-103">Při kopírování dat do databáze serveru SQL/SQL Azure, aktivitě kopírování se ve výchozím nastavení připojení v datové sadě do tabulky jímky ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="2d389-103">When copying data to Azure SQL/SQL Server Database, copy activity will by default APPEND the data set to the sink table by default.</span></span> <span data-ttu-id="2d389-104">Například při kopírování dat ze zdroje souboru CSV (data hodnot oddělených čárkami) obsahující dva záznamy k databázi Azure SQL nebo SQL Server, to je, jak vypadá v tabulce:</span><span class="sxs-lookup"><span data-stu-id="2d389-104">For example, when copying data from a CSV (comma separated values data) file source containing two records to Azure SQL/SQL Server Database, this is what the table looks like:</span></span>

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    2            2015-05-01 00:00:00
```

<span data-ttu-id="2d389-105">Předpokládejme, že byly nalezeny chyby ve zdrojovém souboru a aktualizované množství dolů trubice od 2 do 4 ve zdrojovém souboru.</span><span class="sxs-lookup"><span data-stu-id="2d389-105">Suppose you found errors in source file and updated the quantity of Down Tube from 2 to 4 in the source file.</span></span> <span data-ttu-id="2d389-106">Pokud znovu spustíte datový řez pro toto období, zjistíte dva nové záznamy připojenou k databázi Azure SQL nebo SQL Server.</span><span class="sxs-lookup"><span data-stu-id="2d389-106">If you re-run the data slice for that period, you’ll find two new records appended to Azure SQL/SQL Server Database.</span></span> <span data-ttu-id="2d389-107">Níže předpokládá žádné sloupce v tabulce nejsou omezení primárního klíče.</span><span class="sxs-lookup"><span data-stu-id="2d389-107">The below assumes none of the columns in the table have the primary key constraint.</span></span>

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    2            2015-05-01 00:00:00
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    4            2015-05-01 00:00:00
```

<span data-ttu-id="2d389-108">Abyste tomu předešli, budete muset zadat UPSERT sémantiku s využitím jeden z nižší než 2 mechanismy, které jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="2d389-108">To avoid this, you will need to specify UPSERT semantics by leveraging one of the below 2 mechanisms stated below.</span></span>

> [!NOTE]
> <span data-ttu-id="2d389-109">Řez opětovně lze spustit automaticky v Azure Data Factory podle určené zásady opakování.</span><span class="sxs-lookup"><span data-stu-id="2d389-109">A slice can be re-run automatically in Azure Data Factory as per the retry policy specified.</span></span>
> 
> 

### <a name="mechanism-1"></a><span data-ttu-id="2d389-110">Mechanismus 1</span><span class="sxs-lookup"><span data-stu-id="2d389-110">Mechanism 1</span></span>
<span data-ttu-id="2d389-111">Můžete využít **sqlWriterCleanupScript** vlastnosti tak, aby nejprve provést čištění akce při spuštění řez.</span><span class="sxs-lookup"><span data-stu-id="2d389-111">You can leverage **sqlWriterCleanupScript** property to first perform cleanup action when a slice is run.</span></span> 

```json
"sink":  
{ 
  "type": "SqlSink", 
  "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM table WHERE ModifiedDate >= \\'{0:yyyy-MM-dd HH:mm}\\' AND ModifiedDate < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
}
```

<span data-ttu-id="2d389-112">Skript pro vyčištění by byl proveden první během kopírování pro danou řez, který by odstranit data z tabulek SQL odpovídající této řez.</span><span class="sxs-lookup"><span data-stu-id="2d389-112">The cleanup script would be executed first during copy for a given slice which would delete the data from the SQL Table corresponding to that slice.</span></span> <span data-ttu-id="2d389-113">Aktivity budou následně vložení dat do tabulky SQL.</span><span class="sxs-lookup"><span data-stu-id="2d389-113">The activity will subsequently insert the data into the SQL Table.</span></span> 

<span data-ttu-id="2d389-114">Pokud je řez je nyní spusťte znovu a potom jste zjistí, že množství je aktualizovat, protože požadovaný.</span><span class="sxs-lookup"><span data-stu-id="2d389-114">If the slice is now re-run, then you will find the quantity is updated as desired.</span></span>

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    4            2015-05-01 00:00:00
```

<span data-ttu-id="2d389-115">Předpokládejme, že odebrání záznamu ploché podložku z původní sdílený svazek clusteru.</span><span class="sxs-lookup"><span data-stu-id="2d389-115">Suppose the Flat Washer record is removed from the original csv.</span></span> <span data-ttu-id="2d389-116">Opětovné spuštění řezu vznikna následující výsledek:</span><span class="sxs-lookup"><span data-stu-id="2d389-116">Then re-running the slice would produce the following result:</span></span> 

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
7     Down Tube    4            2015-05-01 00:00:00
```
<span data-ttu-id="2d389-117">Nic nového museli provést.</span><span class="sxs-lookup"><span data-stu-id="2d389-117">Nothing new had to be done.</span></span> <span data-ttu-id="2d389-118">Aktivita kopírování spustili čištění skript, který chcete odstranit data odpovídající této řez.</span><span class="sxs-lookup"><span data-stu-id="2d389-118">The copy activity ran the cleanup script to delete the corresponding data for that slice.</span></span> <span data-ttu-id="2d389-119">Pak jej číst vstupní ze souboru csv (což je obsaženy pouze 1 záznam) a vložit do tabulky.</span><span class="sxs-lookup"><span data-stu-id="2d389-119">Then it read the input from the csv (which then contained only 1 record) and inserted it into the Table.</span></span> 

### <a name="mechanism-2"></a><span data-ttu-id="2d389-120">Mechanismus 2</span><span class="sxs-lookup"><span data-stu-id="2d389-120">Mechanism 2</span></span>
> [!IMPORTANT]
> <span data-ttu-id="2d389-121">sliceIdentifierColumnName není v současné době podporovaný pro Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="2d389-121">sliceIdentifierColumnName is not supported for Azure SQL Data Warehouse at this time.</span></span> 

<span data-ttu-id="2d389-122">Jiný mechanismus k dosažení opakovatelnosti spočívá v použití vyhrazených sloupec (**sliceIdentifierColumnName**) v cílové tabulce.</span><span class="sxs-lookup"><span data-stu-id="2d389-122">Another mechanism to achieve repeatability is by having a dedicated column (**sliceIdentifierColumnName**) in the target Table.</span></span> <span data-ttu-id="2d389-123">V tomto sloupci se použije službou Azure Data Factory pro Ujistěte se, že zdrojový a cílový zůstane synchronizovaná.</span><span class="sxs-lookup"><span data-stu-id="2d389-123">This column would be used by Azure Data Factory to ensure the source and destination stay synchronized.</span></span> <span data-ttu-id="2d389-124">Tento přístup funguje, když je flexibilitu při změně nebo definování schématu cílové tabulky SQL.</span><span class="sxs-lookup"><span data-stu-id="2d389-124">This approach works when there is flexibility in changing or defining the destination SQL Table schema.</span></span> 

<span data-ttu-id="2d389-125">V tomto sloupci se použije službou Azure Data Factory pro účely opakovatelnost a v procesu Azure Data Factory nebude žádné změny schématu tabulky.</span><span class="sxs-lookup"><span data-stu-id="2d389-125">This column would be used by Azure Data Factory for repeatability purposes and in the process Azure Data Factory will not make any schema changes to the Table.</span></span> <span data-ttu-id="2d389-126">Způsob použití tohoto přístupu:</span><span class="sxs-lookup"><span data-stu-id="2d389-126">Way to use this approach:</span></span>

1. <span data-ttu-id="2d389-127">Definujte sloupce binárního typu (32) v cílové tabulce SQL.</span><span class="sxs-lookup"><span data-stu-id="2d389-127">Define a column of type binary (32) in the destination SQL Table.</span></span> <span data-ttu-id="2d389-128">Měla by existovat žádné omezení na tomto sloupci.</span><span class="sxs-lookup"><span data-stu-id="2d389-128">There should be no constraints on this column.</span></span> <span data-ttu-id="2d389-129">V tomto příkladu budeme název v tomto sloupci jako 'ColumnForADFuseOnly'.</span><span class="sxs-lookup"><span data-stu-id="2d389-129">Let's name this column as ‘ColumnForADFuseOnly’ for this example.</span></span>
2. <span data-ttu-id="2d389-130">Ho použijte k aktivitě kopírování následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="2d389-130">Use it in the copy activity as follows:</span></span>
   
    ```json
    "sink":  
    { 
   
        "type": "SqlSink", 
        "sliceIdentifierColumnName": "ColumnForADFuseOnly"
    }
    ```

<span data-ttu-id="2d389-131">Azure Data Factory bude naplnit tento sloupec podle jeho potřeba zajistit, že zdrojový a cílový zůstane synchronizovaná.</span><span class="sxs-lookup"><span data-stu-id="2d389-131">Azure Data Factory will populate this column as per its need to ensure the source and destination stay synchronized.</span></span> <span data-ttu-id="2d389-132">Hodnoty v tomto sloupci nepoužívejte mimo tento kontext uživatele.</span><span class="sxs-lookup"><span data-stu-id="2d389-132">The values of this column should not be used outside of this context by the user.</span></span> 

<span data-ttu-id="2d389-133">Podobně jako mechanismus 1, aktivity kopírování automaticky použije první vyčistit data pro danou řez z cílového tabulky SQL a poté spusťte aktivitě kopírování normálně k vložení dat ze zdroje do cíle pro tento řez.</span><span class="sxs-lookup"><span data-stu-id="2d389-133">Similar to mechanism 1, Copy Activity will automatically first clean up the data for the given slice from the destination SQL Table and then run the copy activity normally to insert the data from source to destination for that slice.</span></span> 

