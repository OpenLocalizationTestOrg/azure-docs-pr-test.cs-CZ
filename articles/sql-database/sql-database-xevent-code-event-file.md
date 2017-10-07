---
title: "aaaXEvent soubor událostí kód pro databázi SQL. | Microsoft Docs"
description: "Příklad dvoufázového kód, který ukazuje soubor událostí cíl hello v rozšířené události v databázi SQL Azure poskytuje prostředí PowerShell a Transact-SQL. Azure Storage je požadovaných součástí tohoto scénáře."
services: sql-database
documentationcenter: 
author: MightyPen
manager: jhubbard
editor: 
tags: 
ms.assetid: bbb10ecc-739f-4159-b844-12b4be161231
ms.service: sql-database
ms.custom: monitor & tune
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/06/2017
ms.author: genemi
ms.openlocfilehash: 4457bd3250f4644b54da2f7daddb9da12070e93a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="event-file-target-code-for-extended-events-in-sql-database"></a><span data-ttu-id="fc50a-104">Kód cílový soubor události pro rozšířené události v databázi SQL</span><span class="sxs-lookup"><span data-stu-id="fc50a-104">Event File target code for extended events in SQL Database</span></span>

[!INCLUDE [sql-database-xevents-selectors-1-include](../../includes/sql-database-xevents-selectors-1-include.md)]

<span data-ttu-id="fc50a-105">Chcete úplného příkladu kódu informace robustní způsob toocapture a sestav pro rozšířené události.</span><span class="sxs-lookup"><span data-stu-id="fc50a-105">You want a complete code sample for a robust way toocapture and report information for an extended event.</span></span>

<span data-ttu-id="fc50a-106">V systému Microsoft SQL Server hello [cílový soubor událostí](http://msdn.microsoft.com/library/ff878115.aspx) je použité toostore výstupy události do souboru pevného disku.</span><span class="sxs-lookup"><span data-stu-id="fc50a-106">In Microsoft SQL Server, hello [Event File target](http://msdn.microsoft.com/library/ff878115.aspx) is used toostore event outputs into a local hard drive file.</span></span> <span data-ttu-id="fc50a-107">Ale tyto soubory nejsou k dispozici tooAzure databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="fc50a-107">But such files are not available tooAzure SQL Database.</span></span> <span data-ttu-id="fc50a-108">Místo toho jsme použití hello Azure Storage service toosupport hello soubor událostí cíle.</span><span class="sxs-lookup"><span data-stu-id="fc50a-108">Instead we use hello Azure Storage service toosupport hello Event File target.</span></span>

<span data-ttu-id="fc50a-109">Toto téma představuje ukázka dvoufázového kódu:</span><span class="sxs-lookup"><span data-stu-id="fc50a-109">This topic presents a two-phase code sample:</span></span>

* <span data-ttu-id="fc50a-110">Prostředí PowerShell, toocreate kontejner Azure Storage v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="fc50a-110">PowerShell, toocreate an Azure Storage container in hello cloud.</span></span>
* <span data-ttu-id="fc50a-111">Příkaz Transact-SQL:</span><span class="sxs-lookup"><span data-stu-id="fc50a-111">Transact-SQL:</span></span>
  
  * <span data-ttu-id="fc50a-112">tooassign hello Azure Storage kontejneru tooan soubor událostí cíl.</span><span class="sxs-lookup"><span data-stu-id="fc50a-112">tooassign hello Azure Storage container tooan Event File target.</span></span>
  * <span data-ttu-id="fc50a-113">toocreate a spusťte relaci události hello a tak dále.</span><span class="sxs-lookup"><span data-stu-id="fc50a-113">toocreate and start hello event session, and so on.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fc50a-114">Požadavky</span><span class="sxs-lookup"><span data-stu-id="fc50a-114">Prerequisites</span></span>

* <span data-ttu-id="fc50a-115">Účet a předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="fc50a-115">An Azure account and subscription.</span></span> <span data-ttu-id="fc50a-116">Můžete si zaregistrovat i [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="fc50a-116">You can sign up for a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="fc50a-117">Všechny databáze, které je možné vytvořit tabulku v.</span><span class="sxs-lookup"><span data-stu-id="fc50a-117">Any database you can create a table in.</span></span>
  
  * <span data-ttu-id="fc50a-118">Volitelně můžete [vytvořit **AdventureWorksLT** ukázkovou databázi](sql-database-get-started.md) v minutách.</span><span class="sxs-lookup"><span data-stu-id="fc50a-118">Optionally you can [create an **AdventureWorksLT** demonstration database](sql-database-get-started.md) in minutes.</span></span>
* <span data-ttu-id="fc50a-119">SQL Server Management Studio (ssms.exe), v ideálním případě její nejnovější měsíční aktualizace verzi.</span><span class="sxs-lookup"><span data-stu-id="fc50a-119">SQL Server Management Studio (ssms.exe), ideally its latest monthly update version.</span></span> 
  <span data-ttu-id="fc50a-120">Si můžete stáhnout nejnovější ssms.exe hello z:</span><span class="sxs-lookup"><span data-stu-id="fc50a-120">You can download hello latest ssms.exe from:</span></span>
  
  * <span data-ttu-id="fc50a-121">Téma s názvem [stáhnout SQL Server Management Studio](http://msdn.microsoft.com/library/mt238290.aspx).</span><span class="sxs-lookup"><span data-stu-id="fc50a-121">Topic titled [Download SQL Server Management Studio](http://msdn.microsoft.com/library/mt238290.aspx).</span></span>
  * [<span data-ttu-id="fc50a-122">Stahování toohello přímý odkaz.</span><span class="sxs-lookup"><span data-stu-id="fc50a-122">A direct link toohello download.</span></span>](http://go.microsoft.com/fwlink/?linkid=616025)
* <span data-ttu-id="fc50a-123">Musíte mít hello [modulů prostředí Azure PowerShell](http://go.microsoft.com/?linkid=9811175) nainstalována.</span><span class="sxs-lookup"><span data-stu-id="fc50a-123">You must have hello [Azure PowerShell modules](http://go.microsoft.com/?linkid=9811175) installed.</span></span>
  
  * <span data-ttu-id="fc50a-124">moduly Hello zadejte příkazy, jako třeba - **New-AzureStorageAccount**.</span><span class="sxs-lookup"><span data-stu-id="fc50a-124">hello modules provide commands such as - **New-AzureStorageAccount**.</span></span>

## <a name="phase-1-powershell-code-for-azure-storage-container"></a><span data-ttu-id="fc50a-125">Fáze 1: Kód prostředí PowerShell pro Azure Storage kontejneru</span><span class="sxs-lookup"><span data-stu-id="fc50a-125">Phase 1: PowerShell code for Azure Storage container</span></span>

<span data-ttu-id="fc50a-126">Toto prostředí PowerShell je fáze 1 dvoufázového příklad hello.</span><span class="sxs-lookup"><span data-stu-id="fc50a-126">This PowerShell is phase 1 of hello two-phase code sample.</span></span>

<span data-ttu-id="fc50a-127">skript Hello začíná příkazy tooclean po předchozí možného spustit a je rerunnable.</span><span class="sxs-lookup"><span data-stu-id="fc50a-127">hello script starts with commands tooclean up after a possible previous run, and is rerunnable.</span></span>

1. <span data-ttu-id="fc50a-128">Vložte skript prostředí PowerShell hello do jednoduchého textového editoru, například Notepad.exe a uložíte skript hello jako soubor s příponou hello **.ps1**.</span><span class="sxs-lookup"><span data-stu-id="fc50a-128">Paste hello PowerShell script into a simple text editor such as Notepad.exe, and save hello script as a file with hello extension **.ps1**.</span></span>
2. <span data-ttu-id="fc50a-129">Spusťte prostředí PowerShell ISE jako správce.</span><span class="sxs-lookup"><span data-stu-id="fc50a-129">Start PowerShell ISE as an Administrator.</span></span>
3. <span data-ttu-id="fc50a-130">Hello řádku zadejte</span><span class="sxs-lookup"><span data-stu-id="fc50a-130">At hello prompt, type</span></span><br/>`Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser`<br/><span data-ttu-id="fc50a-131">a stiskněte klávesu Enter.</span><span class="sxs-lookup"><span data-stu-id="fc50a-131">and then press Enter.</span></span>
4. <span data-ttu-id="fc50a-132">V prostředí PowerShell ISE otevřete váš **.ps1** souboru.</span><span class="sxs-lookup"><span data-stu-id="fc50a-132">In PowerShell ISE, open your **.ps1** file.</span></span> <span data-ttu-id="fc50a-133">Spusťte skript hello.</span><span class="sxs-lookup"><span data-stu-id="fc50a-133">Run hello script.</span></span>
5. <span data-ttu-id="fc50a-134">skript Hello prvním spuštění nové okno, ve kterém přihlášení tooAzure.</span><span class="sxs-lookup"><span data-stu-id="fc50a-134">hello script first starts a new window in which you log in tooAzure.</span></span>
   
   * <span data-ttu-id="fc50a-135">Pokud hello skript bez nutnosti přerušení vaší relace, máte hello vhodnou možnost komentování out hello **Add-AzureAccount** příkaz.</span><span class="sxs-lookup"><span data-stu-id="fc50a-135">If you rerun hello script without disrupting your session, you have hello convenient option of commenting out hello **Add-AzureAccount** command.</span></span>

![Prostředí PowerShell ISE s Azure modul nainstalovaný, připravené toorun skriptu.][30_powershell_ise]


### <a name="powershell-code"></a><span data-ttu-id="fc50a-137">Prostředí PowerShell kódu</span><span class="sxs-lookup"><span data-stu-id="fc50a-137">PowerShell code</span></span>

```powershell
## TODO: Before running, find all 'TODO' and make each edit!!

#--------------- 1 -----------------------


# You can comment out or skip this Add-AzureAccount
# command after hello first run.
# Current PowerShell environment retains hello successful outcome.

'Expect a pop-up window in which you log in tooAzure.'


Add-AzureAccount

#-------------- 2 ------------------------


'
TODO: Edit hello values assigned toothese variables, especially hello first few!
'

# Ensure hello current date is between
# hello Expiry and Start time values that you edit here.

$subscriptionName    = 'YOUR_SUBSCRIPTION_NAME'
$policySasExpiryTime = '2016-01-28T23:44:56Z'
$policySasStartTime  = '2015-08-01'


$storageAccountName     = 'gmstorageaccountxevent'
$storageAccountLocation = 'West US'
$contextName            = 'gmcontext'
$containerName          = 'gmcontainerxevent'
$policySasToken         = 'gmpolicysastoken'


# Leave this value alone, as 'rwl'.
$policySasPermission = 'rwl'

#--------------- 3 -----------------------


# hello ending display lists your Azure subscriptions.
# One should match hello $subscriptionName value you assigned
#   earlier in this PowerShell script. 

'Choose an existing subscription for hello current PowerShell environment.'


Select-AzureSubscription -SubscriptionName $subscriptionName


#-------------- 4 ------------------------


'
Clean up hello old Azure Storage Account after any previous run, 
before continuing this new run.'


If ($storageAccountName)
{
    Remove-AzureStorageAccount -StorageAccountName $storageAccountName
}

#--------------- 5 -----------------------

[System.DateTime]::Now.ToString()

'
Create a storage account. 
This might take several minutes, will beep when ready.
  ...PLEASE WAIT...'

New-AzureStorageAccount `
    -StorageAccountName $storageAccountName `
    -Location           $storageAccountLocation

[System.DateTime]::Now.ToString()

[System.Media.SystemSounds]::Beep.Play()


'
Get hello primary access key for your storage account.
'


$primaryAccessKey_ForStorageAccount = `
    (Get-AzureStorageKey `
        -StorageAccountName $storageAccountName).Primary

"`$primaryAccessKey_ForStorageAccount = $primaryAccessKey_ForStorageAccount"

'Azure Storage Account cmdlet completed.
Remainder of PowerShell .ps1 script continues.
'

#--------------- 6 -----------------------


# hello context will be needed toocreate a container within hello storage account.

'Create a context object from hello storage account and its primary access key.
'

$context = New-AzureStorageContext `
    -StorageAccountName $storageAccountName `
    -StorageAccountKey  $primaryAccessKey_ForStorageAccount


'Create a container within hello storage account.
'


$containerObjectInStorageAccount = New-AzureStorageContainer `
    -Name    $containerName `
    -Context $context


'Create a security policy toobe applied toohello SAS token.
'

New-AzureStorageContainerStoredAccessPolicy `
    -Container  $containerName `
    -Context    $context `
    -Policy     $policySasToken `
    -Permission $policySasPermission `
    -ExpiryTime $policySasExpiryTime `
    -StartTime  $policySasStartTime 

'
Generate a SAS token for hello container.
'
Try
{
    $sasTokenWithPolicy = New-AzureStorageContainerSASToken `
        -Name    $containerName `
        -Context $context `
        -Policy  $policySasToken
}
Catch 
{
    $Error[0].Exception.ToString()
}

#-------------- 7 ------------------------


'Display hello values that YOU must edit into hello Transact-SQL script next!:
'

"storageAccountName: $storageAccountName"
"containerName:      $containerName"
"sasTokenWithPolicy: $sasTokenWithPolicy"

'
REMINDER: sasTokenWithPolicy here might start with "?" character, which you must exclude from Transact-SQL.
'

'
(Later, return here toodelete your Azure Storage account. See hello preceding - Remove-AzureStorageAccount -StorageAccountName $storageAccountName)'

'
Now shift toohello Transact-SQL portion of hello two-part code sample!'

# EOFile
```


<span data-ttu-id="fc50a-138">Poznamenejte si hello několik pojmenovaných hodnot, které skript prostředí PowerShell hello vytiskne při jeho ukončení.</span><span class="sxs-lookup"><span data-stu-id="fc50a-138">Take note of hello few named values that hello PowerShell script prints when it ends.</span></span> <span data-ttu-id="fc50a-139">Je nutné upravit tyto hodnoty do hello skript Transact-SQL, který následuje fáze 2.</span><span class="sxs-lookup"><span data-stu-id="fc50a-139">You must edit those values into hello Transact-SQL script that follows as phase 2.</span></span>

## <a name="phase-2-transact-sql-code-that-uses-azure-storage-container"></a><span data-ttu-id="fc50a-140">Fáze 2: Kód Transact-SQL, který používá kontejner úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="fc50a-140">Phase 2: Transact-SQL code that uses Azure Storage container</span></span>

* <span data-ttu-id="fc50a-141">V této ukázce kódu fáze 1 jste spustili toocreate skript prostředí PowerShell kontejner Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="fc50a-141">In phase 1 of this code sample, you ran a PowerShell script toocreate an Azure Storage container.</span></span>
* <span data-ttu-id="fc50a-142">Dále v fáze 2 hello následující skript jazyka Transact-SQL musíte použít hello kontejneru.</span><span class="sxs-lookup"><span data-stu-id="fc50a-142">Next in phase 2, hello following Transact-SQL script must use hello container.</span></span>

<span data-ttu-id="fc50a-143">skript Hello začíná příkazy tooclean po předchozí možného spustit a je rerunnable.</span><span class="sxs-lookup"><span data-stu-id="fc50a-143">hello script starts with commands tooclean up after a possible previous run, and is rerunnable.</span></span>

<span data-ttu-id="fc50a-144">Hello skript prostředí PowerShell vytištěny několik pojmenovaných hodnot, kdy se dokončila.</span><span class="sxs-lookup"><span data-stu-id="fc50a-144">hello PowerShell script printed a few named values when it ended.</span></span> <span data-ttu-id="fc50a-145">Hello Transact-SQL skriptu toouse je nutné upravit tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="fc50a-145">You must edit hello Transact-SQL script toouse those values.</span></span> <span data-ttu-id="fc50a-146">Najít **TODO** ve hello Transact-SQL skriptu toolocate hello upravit body.</span><span class="sxs-lookup"><span data-stu-id="fc50a-146">Find **TODO** in hello Transact-SQL script toolocate hello edit points.</span></span>

1. <span data-ttu-id="fc50a-147">Spusťte aplikaci SQL Server Management Studio (ssms.exe).</span><span class="sxs-lookup"><span data-stu-id="fc50a-147">Open SQL Server Management Studio (ssms.exe).</span></span>
2. <span data-ttu-id="fc50a-148">Připojte databáze Azure SQL Database tooyour.</span><span class="sxs-lookup"><span data-stu-id="fc50a-148">Connect tooyour Azure SQL Database database.</span></span>
3. <span data-ttu-id="fc50a-149">Klikněte na tlačítko tooopen nové podokno dotazu.</span><span class="sxs-lookup"><span data-stu-id="fc50a-149">Click tooopen a new query pane.</span></span>
4. <span data-ttu-id="fc50a-150">Vložte následující skript jazyka Transact-SQL do podokna dotazu hello hello.</span><span class="sxs-lookup"><span data-stu-id="fc50a-150">Paste hello following Transact-SQL script into hello query pane.</span></span>
5. <span data-ttu-id="fc50a-151">Najít každých **TODO** v hello skript a proveďte příslušné úpravy hello.</span><span class="sxs-lookup"><span data-stu-id="fc50a-151">Find every **TODO** in hello script and make hello appropriate edits.</span></span>
6. <span data-ttu-id="fc50a-152">Uložte a pak spusťte skript hello.</span><span class="sxs-lookup"><span data-stu-id="fc50a-152">Save, and then run hello script.</span></span>


> [!WARNING]
> <span data-ttu-id="fc50a-153">Hello hodnotu klíče SAS generované hello předcházející skript prostředí PowerShell může začínat '?' (otazník).</span><span class="sxs-lookup"><span data-stu-id="fc50a-153">hello SAS key value generated by hello preceding PowerShell script might begin with a '?' (question mark).</span></span> <span data-ttu-id="fc50a-154">Pokud použijete klíč SAS hello v hello následující skriptu T-SQL, musíte *odebrat úvodní hello '?'* .</span><span class="sxs-lookup"><span data-stu-id="fc50a-154">When you use hello SAS key in hello following T-SQL script, you must *remove hello leading '?'*.</span></span> <span data-ttu-id="fc50a-155">V opačném případě mohou vaše úsilí blokovány zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="fc50a-155">Otherwise your efforts might be blocked by security.</span></span>


### <a name="transact-sql-code"></a><span data-ttu-id="fc50a-156">Kód jazyka Transact-SQL</span><span class="sxs-lookup"><span data-stu-id="fc50a-156">Transact-SQL code</span></span>

```sql
---- TODO: First, run hello PowerShell portion of this two-part code sample.
---- TODO: Second, find every 'TODO' in this Transact-SQL file, and edit each.

---- Transact-SQL code for Event File target on Azure SQL Database.


SET NOCOUNT ON;
GO


----  Step 1.  Establish one little table, and  ---------
----  insert one row of data.


IF EXISTS
    (SELECT * FROM sys.objects
        WHERE type = 'U' and name = 'gmTabEmployee')
BEGIN
    DROP TABLE gmTabEmployee;
END
GO


CREATE TABLE gmTabEmployee
(
    EmployeeGuid         uniqueIdentifier   not null  default newid()  primary key,
    EmployeeId           int                not null  identity(1,1),
    EmployeeKudosCount   int                not null  default 0,
    EmployeeDescr        nvarchar(256)          null
);
GO


INSERT INTO gmTabEmployee ( EmployeeDescr )
    VALUES ( 'Jane Doe' );
GO


------  Step 2.  Create key, and  ------------
------  Create credential (your Azure Storage container must already exist).


IF NOT EXISTS
    (SELECT * FROM sys.symmetric_keys
        WHERE symmetric_key_id = 101)
BEGIN
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '0C34C960-6621-4682-A123-C7EA08E3FC46' -- Or any newid().
END
GO


IF EXISTS
    (SELECT * FROM sys.database_scoped_credentials
        -- TODO: Assign AzureStorageAccount name, and hello associated Container name.
        WHERE name = 'https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent')
BEGIN
    DROP DATABASE SCOPED CREDENTIAL
        -- TODO: Assign AzureStorageAccount name, and hello associated Container name.
        [https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent] ;
END
GO


CREATE
    DATABASE SCOPED
    CREDENTIAL
        -- use '.blob.',   and not '.queue.' or '.table.' etc.
        -- TODO: Assign AzureStorageAccount name, and hello associated Container name.
        [https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent]
    WITH
        IDENTITY = 'SHARED ACCESS SIGNATURE',  -- "SAS" token.
        -- TODO: Paste in hello long SasToken string here for Secret, but exclude any leading '?'.
        SECRET = 'sv=2014-02-14&sr=c&si=gmpolicysastoken&sig=EjAqjo6Nu5xMLEZEkMkLbeF7TD9v1J8DNB2t8gOKTts%3D'
    ;
GO


------  Step 3.  Create (define) an event session.  --------
------  hello event session has an event with an action,
------  and a has a target.

IF EXISTS
    (SELECT * from sys.database_event_sessions
        WHERE name = 'gmeventsessionname240b')
BEGIN
    DROP
        EVENT SESSION
            gmeventsessionname240b
        ON DATABASE;
END
GO


CREATE
    EVENT SESSION
        gmeventsessionname240b
    ON DATABASE

    ADD EVENT
        sqlserver.sql_statement_starting
            (
            ACTION (sqlserver.sql_text)
            WHERE statement LIKE 'UPDATE gmTabEmployee%'
            )
    ADD TARGET
        package0.event_file
            (
            -- TODO: Assign AzureStorageAccount name, and hello associated Container name.
            -- Also, tweak hello .xel file name at end, if you like.
            SET filename =
                'https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent/anyfilenamexel242b.xel'
            )
    WITH
        (MAX_MEMORY = 10 MB,
        MAX_DISPATCH_LATENCY = 3 SECONDS)
    ;
GO


------  Step 4.  Start hello event session.  ----------------
------  Issue hello SQL Update statements that will be traced.
------  Then stop hello session.

------  Note: If hello target fails tooattach,
------  hello session must be stopped and restarted.

ALTER
    EVENT SESSION
        gmeventsessionname240b
    ON DATABASE
    STATE = START;
GO


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM gmTabEmployee;

UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 2
    WHERE EmployeeDescr = 'Jane Doe';

UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 13
    WHERE EmployeeDescr = 'Jane Doe';

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM gmTabEmployee;
GO


ALTER
    EVENT SESSION
        gmeventsessionname240b
    ON DATABASE
    STATE = STOP;
GO


-------------- Step 5.  Select hello results. ----------

SELECT
        *, 'CLICK_NEXT_CELL_TO_BROWSE_ITS_RESULTS!' as [CLICK_NEXT_CELL_TO_BROWSE_ITS_RESULTS],
        CAST(event_data AS XML) AS [event_data_XML]  -- TODO: In ssms.exe results grid, double-click this cell!
    FROM
        sys.fn_xe_file_target_read_file
            (
                -- TODO: Fill in Storage Account name, and hello associated Container name.
                'https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent/anyfilenamexel242b',
                null, null, null
            );
GO


-------------- Step 6.  Clean up. ----------

DROP
    EVENT SESSION
        gmeventsessionname240b
    ON DATABASE;
GO

DROP DATABASE SCOPED CREDENTIAL
    -- TODO: Assign AzureStorageAccount name, and hello associated Container name.
    [https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent]
    ;
GO

DROP TABLE gmTabEmployee;
GO

PRINT 'Use PowerShell Remove-AzureStorageAccount toodelete your Azure Storage account!';
GO
```


<span data-ttu-id="fc50a-157">Pokud cílový hello selže tooattach při spuštění, musíte zastavit a restartovat relaci události hello:</span><span class="sxs-lookup"><span data-stu-id="fc50a-157">If hello target fails tooattach when you run, you must stop and restart hello event session:</span></span>

```sql
ALTER EVENT SESSION ... STATE = STOP;
GO
ALTER EVENT SESSION ... STATE = START;
GO
```


## <a name="output"></a><span data-ttu-id="fc50a-158">Výstup</span><span class="sxs-lookup"><span data-stu-id="fc50a-158">Output</span></span>

<span data-ttu-id="fc50a-159">Po dokončení hello skript Transact-SQL, klikněte na buňku pod hello **event_data_XML** záhlaví sloupce.</span><span class="sxs-lookup"><span data-stu-id="fc50a-159">When hello Transact-SQL script completes, click a cell under hello **event_data_XML** column header.</span></span> <span data-ttu-id="fc50a-160">Jeden  **<event>**  element se zobrazuje, který zobrazuje jeden příkaz aktualizace.</span><span class="sxs-lookup"><span data-stu-id="fc50a-160">One **<event>** element is displayed which shows one UPDATE statement.</span></span>

<span data-ttu-id="fc50a-161">Tady je jeden  **<event>**  element, který byl vygenerován při testování:</span><span class="sxs-lookup"><span data-stu-id="fc50a-161">Here is one **<event>** element that was generated during testing:</span></span>


```xml
<event name="sql_statement_starting" package="sqlserver" timestamp="2015-09-22T19:18:45.420Z">
  <data name="state">
    <value>0</value>
    <text>Normal</text>
  </data>
  <data name="line_number">
    <value>5</value>
  </data>
  <data name="offset">
    <value>148</value>
  </data>
  <data name="offset_end">
    <value>368</value>
  </data>
  <data name="statement">
    <value>UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 2
    WHERE EmployeeDescr = 'Jane Doe'</value>
  </data>
  <action name="sql_text" package="sqlserver">
    <value>

SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM gmTabEmployee;

UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 2
    WHERE EmployeeDescr = 'Jane Doe';

UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 13
    WHERE EmployeeDescr = 'Jane Doe';

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM gmTabEmployee;
</value>
  </action>
</event>
```


<span data-ttu-id="fc50a-162">Hello předchozí hello používá skript Transact-SQL systému funkce tooread hello event_file následující:</span><span class="sxs-lookup"><span data-stu-id="fc50a-162">hello preceding Transact-SQL script used hello following system function tooread hello event_file:</span></span>

* [<span data-ttu-id="fc50a-163">Sys.fn_xe_file_target_read_file (Transact-SQL)</span><span class="sxs-lookup"><span data-stu-id="fc50a-163">sys.fn_xe_file_target_read_file (Transact-SQL)</span></span>](http://msdn.microsoft.com/library/cc280743.aspx)

<span data-ttu-id="fc50a-164">Vysvětlení rozšířené možnosti pro hello zobrazení dat z rozšířených událostí je k dispozici na:</span><span class="sxs-lookup"><span data-stu-id="fc50a-164">An explanation of advanced options for hello viewing of data from extended events is available at:</span></span>

* [<span data-ttu-id="fc50a-165">Rozšířené zobrazení cílová Data z rozšířených událostí</span><span class="sxs-lookup"><span data-stu-id="fc50a-165">Advanced Viewing of Target Data from Extended Events</span></span>](http://msdn.microsoft.com/library/mt752502.aspx)


## <a name="converting-hello-code-sample-toorun-on-sql-server"></a><span data-ttu-id="fc50a-166">Převádění toorun ukázkový kód hello na serveru SQL Server</span><span class="sxs-lookup"><span data-stu-id="fc50a-166">Converting hello code sample toorun on SQL Server</span></span>

<span data-ttu-id="fc50a-167">Předpokládejme, že jste chtěli toorun hello předchozích ukázkových Transact-SQL na serveru Microsoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="fc50a-167">Suppose you wanted toorun hello preceding Transact-SQL sample on Microsoft SQL Server.</span></span>

* <span data-ttu-id="fc50a-168">Pro jednoduchost, kterou by chcete toocompletely nahradit použít hello kontejneru úložiště Azure s jednoduchého souboru, jako **C:\myeventdata.xel**.</span><span class="sxs-lookup"><span data-stu-id="fc50a-168">For simplicity, you would want toocompletely replace use of hello Azure Storage container with a simple file such as **C:\myeventdata.xel**.</span></span> <span data-ttu-id="fc50a-169">soubor Hello by byla zapsána místní pevný disk toohello hello počítače, který je hostitelem systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="fc50a-169">hello file would be written toohello local hard drive of hello computer that hosts SQL Server.</span></span>
* <span data-ttu-id="fc50a-170">Nebude potřeba jakýkoli druh příkazy jazyka Transact-SQL pro **CREATE MASTER KEY** a **vytvoření pověření**.</span><span class="sxs-lookup"><span data-stu-id="fc50a-170">You would not need any kind of Transact-SQL statements for **CREATE MASTER KEY** and **CREATE CREDENTIAL**.</span></span>
* <span data-ttu-id="fc50a-171">V hello **vytvořit událost relace** příkaz v jeho **přidat cíl** klauzule, měli byste nahradit hello Http hodnotu přiřazenou provedené příliš**filename =** řetězcem úplnou cestu jako  **C:\MyFile.xel**.</span><span class="sxs-lookup"><span data-stu-id="fc50a-171">In hello **CREATE EVENT SESSION** statement, in its **ADD TARGET** clause, you would replace hello Http value assigned made too**filename=** with a full path string like **C:\myfile.xel**.</span></span>
  
  * <span data-ttu-id="fc50a-172">Žádný účet úložiště Azure musí být zahrnut.</span><span class="sxs-lookup"><span data-stu-id="fc50a-172">No Azure Storage account need be involved.</span></span>

## <a name="more-information"></a><span data-ttu-id="fc50a-173">Další informace</span><span class="sxs-lookup"><span data-stu-id="fc50a-173">More information</span></span>

<span data-ttu-id="fc50a-174">Další informace o účtech a kontejnery v hello služby Azure Storage najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="fc50a-174">For more info about accounts and containers in hello Azure Storage service, see:</span></span>

* [<span data-ttu-id="fc50a-175">Jak toouse úložiště Blob z rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="fc50a-175">How toouse Blob storage from .NET</span></span>](../storage/blobs/storage-dotnet-how-to-use-blobs.md)
* [<span data-ttu-id="fc50a-176">Pojmenování a odkazování na kontejnerů, objektů BLOB a metadat</span><span class="sxs-lookup"><span data-stu-id="fc50a-176">Naming and Referencing Containers, Blobs, and Metadata</span></span>](http://msdn.microsoft.com/library/azure/dd135715.aspx)
* [<span data-ttu-id="fc50a-177">Práce s hello Kořenový kontejner</span><span class="sxs-lookup"><span data-stu-id="fc50a-177">Working with hello Root Container</span></span>](http://msdn.microsoft.com/library/azure/ee395424.aspx)
* [<span data-ttu-id="fc50a-178">Lekce 1: Vytvoření zásady uložené přístup a sdílený přístupový podpis na kontejner Azure</span><span class="sxs-lookup"><span data-stu-id="fc50a-178">Lesson 1: Create a stored access policy and a shared access signature on an Azure container</span></span>](http://msdn.microsoft.com/library/dn466430.aspx)
  * [<span data-ttu-id="fc50a-179">Lekce 2: Vytvoření přihlašovacích údajů systému SQL Server pomocí sdíleného přístupového podpisu</span><span class="sxs-lookup"><span data-stu-id="fc50a-179">Lesson 2: Create a SQL Server credential using a shared access signature</span></span>](http://msdn.microsoft.com/library/dn466435.aspx)

<!--
Image references.
-->

[30_powershell_ise]: ./media/sql-database-xevent-code-event-file/event-file-powershell-ise-b30.png

