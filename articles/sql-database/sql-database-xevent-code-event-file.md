---
title: "Kód soubor události XEvent pro SQL Database | Microsoft Docs"
description: "Příklad dvoufázového kód, který ukazuje soubor událostí cíl v rozšířené události v databázi SQL Azure poskytuje prostředí PowerShell a Transact-SQL. Azure Storage je požadovaných součástí tohoto scénáře."
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
ms.openlocfilehash: e8c7a9af11ac4c22be00426337ab7c8b8ff0860f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="event-file-target-code-for-extended-events-in-sql-database"></a><span data-ttu-id="5430a-104">Kód cílový soubor události pro rozšířené události v databázi SQL</span><span class="sxs-lookup"><span data-stu-id="5430a-104">Event File target code for extended events in SQL Database</span></span>

[!INCLUDE [sql-database-xevents-selectors-1-include](../../includes/sql-database-xevents-selectors-1-include.md)]

<span data-ttu-id="5430a-105">Chcete úplného příkladu kódu pro robustní způsob, jak informace o zachycení a sestav pro rozšířené události.</span><span class="sxs-lookup"><span data-stu-id="5430a-105">You want a complete code sample for a robust way to capture and report information for an extended event.</span></span>

<span data-ttu-id="5430a-106">V systému Microsoft SQL Server [cílový soubor událostí](http://msdn.microsoft.com/library/ff878115.aspx) se používá k ukládání výstupy události do souboru pevného disku.</span><span class="sxs-lookup"><span data-stu-id="5430a-106">In Microsoft SQL Server, the [Event File target](http://msdn.microsoft.com/library/ff878115.aspx) is used to store event outputs into a local hard drive file.</span></span> <span data-ttu-id="5430a-107">Ale tyto soubory nejsou k dispozici pro Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="5430a-107">But such files are not available to Azure SQL Database.</span></span> <span data-ttu-id="5430a-108">Místo toho jsme používat službu Azure Storage pro podporu cílový soubor událostí.</span><span class="sxs-lookup"><span data-stu-id="5430a-108">Instead we use the Azure Storage service to support the Event File target.</span></span>

<span data-ttu-id="5430a-109">Toto téma představuje ukázka dvoufázového kódu:</span><span class="sxs-lookup"><span data-stu-id="5430a-109">This topic presents a two-phase code sample:</span></span>

* <span data-ttu-id="5430a-110">Prostředí PowerShell, chcete-li vytvořit kontejner Azure Storage v cloudu.</span><span class="sxs-lookup"><span data-stu-id="5430a-110">PowerShell, to create an Azure Storage container in the cloud.</span></span>
* <span data-ttu-id="5430a-111">Příkaz Transact-SQL:</span><span class="sxs-lookup"><span data-stu-id="5430a-111">Transact-SQL:</span></span>
  
  * <span data-ttu-id="5430a-112">Kontejner úložiště Azure přiřadit cíl soubor událostí.</span><span class="sxs-lookup"><span data-stu-id="5430a-112">To assign the Azure Storage container to an Event File target.</span></span>
  * <span data-ttu-id="5430a-113">Chcete-li vytvořit a zahájit relaci události a tak dále.</span><span class="sxs-lookup"><span data-stu-id="5430a-113">To create and start the event session, and so on.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5430a-114">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5430a-114">Prerequisites</span></span>

* <span data-ttu-id="5430a-115">Účet a předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="5430a-115">An Azure account and subscription.</span></span> <span data-ttu-id="5430a-116">Můžete si zaregistrovat i [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5430a-116">You can sign up for a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="5430a-117">Všechny databáze, které je možné vytvořit tabulku v.</span><span class="sxs-lookup"><span data-stu-id="5430a-117">Any database you can create a table in.</span></span>
  
  * <span data-ttu-id="5430a-118">Volitelně můžete [vytvořit **AdventureWorksLT** ukázkovou databázi](sql-database-get-started.md) v minutách.</span><span class="sxs-lookup"><span data-stu-id="5430a-118">Optionally you can [create an **AdventureWorksLT** demonstration database](sql-database-get-started.md) in minutes.</span></span>
* <span data-ttu-id="5430a-119">SQL Server Management Studio (ssms.exe), v ideálním případě její nejnovější měsíční aktualizace verzi.</span><span class="sxs-lookup"><span data-stu-id="5430a-119">SQL Server Management Studio (ssms.exe), ideally its latest monthly update version.</span></span> 
  <span data-ttu-id="5430a-120">Si můžete stáhnout nejnovější ssms.exe z:</span><span class="sxs-lookup"><span data-stu-id="5430a-120">You can download the latest ssms.exe from:</span></span>
  
  * <span data-ttu-id="5430a-121">Téma s názvem [stáhnout SQL Server Management Studio](http://msdn.microsoft.com/library/mt238290.aspx).</span><span class="sxs-lookup"><span data-stu-id="5430a-121">Topic titled [Download SQL Server Management Studio](http://msdn.microsoft.com/library/mt238290.aspx).</span></span>
  * [<span data-ttu-id="5430a-122">Přímý odkaz na stažení.</span><span class="sxs-lookup"><span data-stu-id="5430a-122">A direct link to the download.</span></span>](http://go.microsoft.com/fwlink/?linkid=616025)
* <span data-ttu-id="5430a-123">Musíte mít [modulů prostředí Azure PowerShell](http://go.microsoft.com/?linkid=9811175) nainstalována.</span><span class="sxs-lookup"><span data-stu-id="5430a-123">You must have the [Azure PowerShell modules](http://go.microsoft.com/?linkid=9811175) installed.</span></span>
  
  * <span data-ttu-id="5430a-124">Moduly zadejte příkazy, jako třeba - **New-AzureStorageAccount**.</span><span class="sxs-lookup"><span data-stu-id="5430a-124">The modules provide commands such as - **New-AzureStorageAccount**.</span></span>

## <a name="phase-1-powershell-code-for-azure-storage-container"></a><span data-ttu-id="5430a-125">Fáze 1: Kód prostředí PowerShell pro Azure Storage kontejneru</span><span class="sxs-lookup"><span data-stu-id="5430a-125">Phase 1: PowerShell code for Azure Storage container</span></span>

<span data-ttu-id="5430a-126">Toto prostředí PowerShell je fáze 1 dvoufázového příklad.</span><span class="sxs-lookup"><span data-stu-id="5430a-126">This PowerShell is phase 1 of the two-phase code sample.</span></span>

<span data-ttu-id="5430a-127">Skript se spustí pomocí příkazů odstraňování až po předchozí možného spustit a je rerunnable.</span><span class="sxs-lookup"><span data-stu-id="5430a-127">The script starts with commands to clean up after a possible previous run, and is rerunnable.</span></span>

1. <span data-ttu-id="5430a-128">Vložte skript prostředí PowerShell do jednoduchého textového editoru, například Notepad.exe a uložíte skript jako soubor s příponou **.ps1**.</span><span class="sxs-lookup"><span data-stu-id="5430a-128">Paste the PowerShell script into a simple text editor such as Notepad.exe, and save the script as a file with the extension **.ps1**.</span></span>
2. <span data-ttu-id="5430a-129">Spusťte prostředí PowerShell ISE jako správce.</span><span class="sxs-lookup"><span data-stu-id="5430a-129">Start PowerShell ISE as an Administrator.</span></span>
3. <span data-ttu-id="5430a-130">Na příkazovém řádku zadejte</span><span class="sxs-lookup"><span data-stu-id="5430a-130">At the prompt, type</span></span><br/>`Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser`<br/><span data-ttu-id="5430a-131">a stiskněte klávesu Enter.</span><span class="sxs-lookup"><span data-stu-id="5430a-131">and then press Enter.</span></span>
4. <span data-ttu-id="5430a-132">V prostředí PowerShell ISE otevřete váš **.ps1** souboru.</span><span class="sxs-lookup"><span data-stu-id="5430a-132">In PowerShell ISE, open your **.ps1** file.</span></span> <span data-ttu-id="5430a-133">Spusťte skript.</span><span class="sxs-lookup"><span data-stu-id="5430a-133">Run the script.</span></span>
5. <span data-ttu-id="5430a-134">Skript spustí nejprve nové okno, ve kterém se přihlásit k Azure.</span><span class="sxs-lookup"><span data-stu-id="5430a-134">The script first starts a new window in which you log in to Azure.</span></span>
   
   * <span data-ttu-id="5430a-135">Pokud skript bez nutnosti přerušení vaší relace, máte vhodnou možnost komentování se **Add-AzureAccount** příkaz.</span><span class="sxs-lookup"><span data-stu-id="5430a-135">If you rerun the script without disrupting your session, you have the convenient option of commenting out the **Add-AzureAccount** command.</span></span>

![Prostředí PowerShell ISE s Azure modul nainstalovaný, připraven ke spuštění skriptu.][30_powershell_ise]


### <a name="powershell-code"></a><span data-ttu-id="5430a-137">Prostředí PowerShell kódu</span><span class="sxs-lookup"><span data-stu-id="5430a-137">PowerShell code</span></span>

```powershell
## TODO: Before running, find all 'TODO' and make each edit!!

#--------------- 1 -----------------------


# You can comment out or skip this Add-AzureAccount
# command after the first run.
# Current PowerShell environment retains the successful outcome.

'Expect a pop-up window in which you log in to Azure.'


Add-AzureAccount

#-------------- 2 ------------------------


'
TODO: Edit the values assigned to these variables, especially the first few!
'

# Ensure the current date is between
# the Expiry and Start time values that you edit here.

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


# The ending display lists your Azure subscriptions.
# One should match the $subscriptionName value you assigned
#   earlier in this PowerShell script. 

'Choose an existing subscription for the current PowerShell environment.'


Select-AzureSubscription -SubscriptionName $subscriptionName


#-------------- 4 ------------------------


'
Clean up the old Azure Storage Account after any previous run, 
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
Get the primary access key for your storage account.
'


$primaryAccessKey_ForStorageAccount = `
    (Get-AzureStorageKey `
        -StorageAccountName $storageAccountName).Primary

"`$primaryAccessKey_ForStorageAccount = $primaryAccessKey_ForStorageAccount"

'Azure Storage Account cmdlet completed.
Remainder of PowerShell .ps1 script continues.
'

#--------------- 6 -----------------------


# The context will be needed to create a container within the storage account.

'Create a context object from the storage account and its primary access key.
'

$context = New-AzureStorageContext `
    -StorageAccountName $storageAccountName `
    -StorageAccountKey  $primaryAccessKey_ForStorageAccount


'Create a container within the storage account.
'


$containerObjectInStorageAccount = New-AzureStorageContainer `
    -Name    $containerName `
    -Context $context


'Create a security policy to be applied to the SAS token.
'

New-AzureStorageContainerStoredAccessPolicy `
    -Container  $containerName `
    -Context    $context `
    -Policy     $policySasToken `
    -Permission $policySasPermission `
    -ExpiryTime $policySasExpiryTime `
    -StartTime  $policySasStartTime 

'
Generate a SAS token for the container.
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


'Display the values that YOU must edit into the Transact-SQL script next!:
'

"storageAccountName: $storageAccountName"
"containerName:      $containerName"
"sasTokenWithPolicy: $sasTokenWithPolicy"

'
REMINDER: sasTokenWithPolicy here might start with "?" character, which you must exclude from Transact-SQL.
'

'
(Later, return here to delete your Azure Storage account. See the preceding - Remove-AzureStorageAccount -StorageAccountName $storageAccountName)'

'
Now shift to the Transact-SQL portion of the two-part code sample!'

# EOFile
```


<span data-ttu-id="5430a-138">Poznamenejte si několik pojmenovaných hodnot, které skript prostředí PowerShell vytiskne při jeho ukončení.</span><span class="sxs-lookup"><span data-stu-id="5430a-138">Take note of the few named values that the PowerShell script prints when it ends.</span></span> <span data-ttu-id="5430a-139">Je nutné upravit tyto hodnoty do skriptu jazyka Transact-SQL, který následuje fáze 2.</span><span class="sxs-lookup"><span data-stu-id="5430a-139">You must edit those values into the Transact-SQL script that follows as phase 2.</span></span>

## <a name="phase-2-transact-sql-code-that-uses-azure-storage-container"></a><span data-ttu-id="5430a-140">Fáze 2: Kód Transact-SQL, který používá kontejner úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="5430a-140">Phase 2: Transact-SQL code that uses Azure Storage container</span></span>

* <span data-ttu-id="5430a-141">V této ukázce kódu fáze 1 jste spustili skript prostředí PowerShell vytvořit kontejner úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="5430a-141">In phase 1 of this code sample, you ran a PowerShell script to create an Azure Storage container.</span></span>
* <span data-ttu-id="5430a-142">Další ve fázi 2, musíte použít následující skript jazyka Transact-SQL kontejneru.</span><span class="sxs-lookup"><span data-stu-id="5430a-142">Next in phase 2, the following Transact-SQL script must use the container.</span></span>

<span data-ttu-id="5430a-143">Skript se spustí pomocí příkazů odstraňování až po předchozí možného spustit a je rerunnable.</span><span class="sxs-lookup"><span data-stu-id="5430a-143">The script starts with commands to clean up after a possible previous run, and is rerunnable.</span></span>

<span data-ttu-id="5430a-144">Skript prostředí PowerShell vytištěny několik pojmenovaných hodnot, kdy se dokončila.</span><span class="sxs-lookup"><span data-stu-id="5430a-144">The PowerShell script printed a few named values when it ended.</span></span> <span data-ttu-id="5430a-145">Je nutné upravit skript Transact-SQL pro použití těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="5430a-145">You must edit the Transact-SQL script to use those values.</span></span> <span data-ttu-id="5430a-146">Najít **TODO** ve skriptu jazyka Transact-SQL pro vyhledání bodů upravit.</span><span class="sxs-lookup"><span data-stu-id="5430a-146">Find **TODO** in the Transact-SQL script to locate the edit points.</span></span>

1. <span data-ttu-id="5430a-147">Spusťte aplikaci SQL Server Management Studio (ssms.exe).</span><span class="sxs-lookup"><span data-stu-id="5430a-147">Open SQL Server Management Studio (ssms.exe).</span></span>
2. <span data-ttu-id="5430a-148">Připojení k vaší databázi Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="5430a-148">Connect to your Azure SQL Database database.</span></span>
3. <span data-ttu-id="5430a-149">Kliknutím otevřete nové podokno dotazu.</span><span class="sxs-lookup"><span data-stu-id="5430a-149">Click to open a new query pane.</span></span>
4. <span data-ttu-id="5430a-150">Vložte následující skript jazyka Transact-SQL do podokna dotazu.</span><span class="sxs-lookup"><span data-stu-id="5430a-150">Paste the following Transact-SQL script into the query pane.</span></span>
5. <span data-ttu-id="5430a-151">Najít každých **TODO** ve skriptu a proveďte příslušné úpravy.</span><span class="sxs-lookup"><span data-stu-id="5430a-151">Find every **TODO** in the script and make the appropriate edits.</span></span>
6. <span data-ttu-id="5430a-152">Uložte a pak spusťte skript.</span><span class="sxs-lookup"><span data-stu-id="5430a-152">Save, and then run the script.</span></span>


> [!WARNING]
> <span data-ttu-id="5430a-153">Hodnotu klíče SAS generované předchozí skript prostředí PowerShell může začínat '?' (otazník).</span><span class="sxs-lookup"><span data-stu-id="5430a-153">The SAS key value generated by the preceding PowerShell script might begin with a '?' (question mark).</span></span> <span data-ttu-id="5430a-154">Pokud použijete klíč SAS v následujícím skriptu T-SQL, musíte *odebrat úvodního '?'* .</span><span class="sxs-lookup"><span data-stu-id="5430a-154">When you use the SAS key in the following T-SQL script, you must *remove the leading '?'*.</span></span> <span data-ttu-id="5430a-155">V opačném případě mohou vaše úsilí blokovány zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="5430a-155">Otherwise your efforts might be blocked by security.</span></span>


### <a name="transact-sql-code"></a><span data-ttu-id="5430a-156">Kód jazyka Transact-SQL</span><span class="sxs-lookup"><span data-stu-id="5430a-156">Transact-SQL code</span></span>

```sql
---- TODO: First, run the PowerShell portion of this two-part code sample.
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
        -- TODO: Assign AzureStorageAccount name, and the associated Container name.
        WHERE name = 'https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent')
BEGIN
    DROP DATABASE SCOPED CREDENTIAL
        -- TODO: Assign AzureStorageAccount name, and the associated Container name.
        [https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent] ;
END
GO


CREATE
    DATABASE SCOPED
    CREDENTIAL
        -- use '.blob.',   and not '.queue.' or '.table.' etc.
        -- TODO: Assign AzureStorageAccount name, and the associated Container name.
        [https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent]
    WITH
        IDENTITY = 'SHARED ACCESS SIGNATURE',  -- "SAS" token.
        -- TODO: Paste in the long SasToken string here for Secret, but exclude any leading '?'.
        SECRET = 'sv=2014-02-14&sr=c&si=gmpolicysastoken&sig=EjAqjo6Nu5xMLEZEkMkLbeF7TD9v1J8DNB2t8gOKTts%3D'
    ;
GO


------  Step 3.  Create (define) an event session.  --------
------  The event session has an event with an action,
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
            -- TODO: Assign AzureStorageAccount name, and the associated Container name.
            -- Also, tweak the .xel file name at end, if you like.
            SET filename =
                'https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent/anyfilenamexel242b.xel'
            )
    WITH
        (MAX_MEMORY = 10 MB,
        MAX_DISPATCH_LATENCY = 3 SECONDS)
    ;
GO


------  Step 4.  Start the event session.  ----------------
------  Issue the SQL Update statements that will be traced.
------  Then stop the session.

------  Note: If the target fails to attach,
------  the session must be stopped and restarted.

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


-------------- Step 5.  Select the results. ----------

SELECT
        *, 'CLICK_NEXT_CELL_TO_BROWSE_ITS_RESULTS!' as [CLICK_NEXT_CELL_TO_BROWSE_ITS_RESULTS],
        CAST(event_data AS XML) AS [event_data_XML]  -- TODO: In ssms.exe results grid, double-click this cell!
    FROM
        sys.fn_xe_file_target_read_file
            (
                -- TODO: Fill in Storage Account name, and the associated Container name.
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
    -- TODO: Assign AzureStorageAccount name, and the associated Container name.
    [https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent]
    ;
GO

DROP TABLE gmTabEmployee;
GO

PRINT 'Use PowerShell Remove-AzureStorageAccount to delete your Azure Storage account!';
GO
```


<span data-ttu-id="5430a-157">Pokud cílový se nepodaří připojit při spuštění, musíte zastavit a restartovat relaci události:</span><span class="sxs-lookup"><span data-stu-id="5430a-157">If the target fails to attach when you run, you must stop and restart the event session:</span></span>

```sql
ALTER EVENT SESSION ... STATE = STOP;
GO
ALTER EVENT SESSION ... STATE = START;
GO
```


## <a name="output"></a><span data-ttu-id="5430a-158">Výstup</span><span class="sxs-lookup"><span data-stu-id="5430a-158">Output</span></span>

<span data-ttu-id="5430a-159">Po dokončení skriptu jazyka Transact-SQL, klikněte na buňku v části **event_data_XML** záhlaví sloupce.</span><span class="sxs-lookup"><span data-stu-id="5430a-159">When the Transact-SQL script completes, click a cell under the **event_data_XML** column header.</span></span> <span data-ttu-id="5430a-160">Jeden  **<event>**  element se zobrazuje, který zobrazuje jeden příkaz aktualizace.</span><span class="sxs-lookup"><span data-stu-id="5430a-160">One **<event>** element is displayed which shows one UPDATE statement.</span></span>

<span data-ttu-id="5430a-161">Tady je jeden  **<event>**  element, který byl vygenerován při testování:</span><span class="sxs-lookup"><span data-stu-id="5430a-161">Here is one **<event>** element that was generated during testing:</span></span>


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


<span data-ttu-id="5430a-162">Uvedený skript Transact-SQL použít následující funkce systému ke čtení event_file:</span><span class="sxs-lookup"><span data-stu-id="5430a-162">The preceding Transact-SQL script used the following system function to read the event_file:</span></span>

* [<span data-ttu-id="5430a-163">Sys.fn_xe_file_target_read_file (Transact-SQL)</span><span class="sxs-lookup"><span data-stu-id="5430a-163">sys.fn_xe_file_target_read_file (Transact-SQL)</span></span>](http://msdn.microsoft.com/library/cc280743.aspx)

<span data-ttu-id="5430a-164">Vysvětlení rozšířené možnosti pro zobrazení dat z rozšířených událostí je k dispozici na:</span><span class="sxs-lookup"><span data-stu-id="5430a-164">An explanation of advanced options for the viewing of data from extended events is available at:</span></span>

* [<span data-ttu-id="5430a-165">Rozšířené zobrazení cílová Data z rozšířených událostí</span><span class="sxs-lookup"><span data-stu-id="5430a-165">Advanced Viewing of Target Data from Extended Events</span></span>](http://msdn.microsoft.com/library/mt752502.aspx)


## <a name="converting-the-code-sample-to-run-on-sql-server"></a><span data-ttu-id="5430a-166">Převádění ukázkový kód pro spuštění na serveru SQL</span><span class="sxs-lookup"><span data-stu-id="5430a-166">Converting the code sample to run on SQL Server</span></span>

<span data-ttu-id="5430a-167">Předpokládejme, že chcete spustit v předchozím příkladu Transact-SQL na serveru Microsoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="5430a-167">Suppose you wanted to run the preceding Transact-SQL sample on Microsoft SQL Server.</span></span>

* <span data-ttu-id="5430a-168">Pro jednoduchost, by chcete úplně nahradit použití kontejner úložiště Azure jednoduchého souboru, jako **C:\myeventdata.xel**.</span><span class="sxs-lookup"><span data-stu-id="5430a-168">For simplicity, you would want to completely replace use of the Azure Storage container with a simple file such as **C:\myeventdata.xel**.</span></span> <span data-ttu-id="5430a-169">Soubor by byla zapsána na místní pevný disk počítače, který je hostitelem systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="5430a-169">The file would be written to the local hard drive of the computer that hosts SQL Server.</span></span>
* <span data-ttu-id="5430a-170">Nebude potřeba jakýkoli druh příkazy jazyka Transact-SQL pro **CREATE MASTER KEY** a **vytvoření pověření**.</span><span class="sxs-lookup"><span data-stu-id="5430a-170">You would not need any kind of Transact-SQL statements for **CREATE MASTER KEY** and **CREATE CREDENTIAL**.</span></span>
* <span data-ttu-id="5430a-171">V **vytvořit událost relace** příkaz v jeho **přidat cíl** klauzule, měli byste nahradit Http hodnotu přiřazenou provedené **filename =** řetězcem úplnou cestu jako **C:\myfile.xel**.</span><span class="sxs-lookup"><span data-stu-id="5430a-171">In the **CREATE EVENT SESSION** statement, in its **ADD TARGET** clause, you would replace the Http value assigned made to **filename=** with a full path string like **C:\myfile.xel**.</span></span>
  
  * <span data-ttu-id="5430a-172">Žádný účet úložiště Azure musí být zahrnut.</span><span class="sxs-lookup"><span data-stu-id="5430a-172">No Azure Storage account need be involved.</span></span>

## <a name="more-information"></a><span data-ttu-id="5430a-173">Další informace</span><span class="sxs-lookup"><span data-stu-id="5430a-173">More information</span></span>

<span data-ttu-id="5430a-174">Další informace o účtech a kontejnerů ve službě Azure Storage najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="5430a-174">For more info about accounts and containers in the Azure Storage service, see:</span></span>

* [<span data-ttu-id="5430a-175">Používání úložiště Blob z rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="5430a-175">How to use Blob storage from .NET</span></span>](../storage/blobs/storage-dotnet-how-to-use-blobs.md)
* [<span data-ttu-id="5430a-176">Pojmenování a odkazování na kontejnerů, objektů BLOB a metadat</span><span class="sxs-lookup"><span data-stu-id="5430a-176">Naming and Referencing Containers, Blobs, and Metadata</span></span>](http://msdn.microsoft.com/library/azure/dd135715.aspx)
* [<span data-ttu-id="5430a-177">Práce s Kořenový kontejner</span><span class="sxs-lookup"><span data-stu-id="5430a-177">Working with the Root Container</span></span>](http://msdn.microsoft.com/library/azure/ee395424.aspx)
* [<span data-ttu-id="5430a-178">Lekce 1: Vytvoření zásady uložené přístup a sdílený přístupový podpis na kontejner Azure</span><span class="sxs-lookup"><span data-stu-id="5430a-178">Lesson 1: Create a stored access policy and a shared access signature on an Azure container</span></span>](http://msdn.microsoft.com/library/dn466430.aspx)
  * [<span data-ttu-id="5430a-179">Lekce 2: Vytvoření přihlašovacích údajů systému SQL Server pomocí sdíleného přístupového podpisu</span><span class="sxs-lookup"><span data-stu-id="5430a-179">Lesson 2: Create a SQL Server credential using a shared access signature</span></span>](http://msdn.microsoft.com/library/dn466435.aspx)

<!--
Image references.
-->

[30_powershell_ise]: ./media/sql-database-xevent-code-event-file/event-file-powershell-ise-b30.png

