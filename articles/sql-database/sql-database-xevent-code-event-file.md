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
# <a name="event-file-target-code-for-extended-events-in-sql-database"></a>Kód cílový soubor události pro rozšířené události v databázi SQL

[!INCLUDE [sql-database-xevents-selectors-1-include](../../includes/sql-database-xevents-selectors-1-include.md)]

Chcete úplného příkladu kódu informace robustní způsob toocapture a sestav pro rozšířené události.

V systému Microsoft SQL Server hello [cílový soubor událostí](http://msdn.microsoft.com/library/ff878115.aspx) je použité toostore výstupy události do souboru pevného disku. Ale tyto soubory nejsou k dispozici tooAzure databáze SQL. Místo toho jsme použití hello Azure Storage service toosupport hello soubor událostí cíle.

Toto téma představuje ukázka dvoufázového kódu:

* Prostředí PowerShell, toocreate kontejner Azure Storage v cloudu hello.
* Příkaz Transact-SQL:
  
  * tooassign hello Azure Storage kontejneru tooan soubor událostí cíl.
  * toocreate a spusťte relaci události hello a tak dále.

## <a name="prerequisites"></a>Požadavky

* Účet a předplatné Azure. Můžete si zaregistrovat i [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).
* Všechny databáze, které je možné vytvořit tabulku v.
  
  * Volitelně můžete [vytvořit **AdventureWorksLT** ukázkovou databázi](sql-database-get-started.md) v minutách.
* SQL Server Management Studio (ssms.exe), v ideálním případě její nejnovější měsíční aktualizace verzi. 
  Si můžete stáhnout nejnovější ssms.exe hello z:
  
  * Téma s názvem [stáhnout SQL Server Management Studio](http://msdn.microsoft.com/library/mt238290.aspx).
  * [Stahování toohello přímý odkaz.](http://go.microsoft.com/fwlink/?linkid=616025)
* Musíte mít hello [modulů prostředí Azure PowerShell](http://go.microsoft.com/?linkid=9811175) nainstalována.
  
  * moduly Hello zadejte příkazy, jako třeba - **New-AzureStorageAccount**.

## <a name="phase-1-powershell-code-for-azure-storage-container"></a>Fáze 1: Kód prostředí PowerShell pro Azure Storage kontejneru

Toto prostředí PowerShell je fáze 1 dvoufázového příklad hello.

skript Hello začíná příkazy tooclean po předchozí možného spustit a je rerunnable.

1. Vložte skript prostředí PowerShell hello do jednoduchého textového editoru, například Notepad.exe a uložíte skript hello jako soubor s příponou hello **.ps1**.
2. Spusťte prostředí PowerShell ISE jako správce.
3. Hello řádku zadejte<br/>`Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser`<br/>a stiskněte klávesu Enter.
4. V prostředí PowerShell ISE otevřete váš **.ps1** souboru. Spusťte skript hello.
5. skript Hello prvním spuštění nové okno, ve kterém přihlášení tooAzure.
   
   * Pokud hello skript bez nutnosti přerušení vaší relace, máte hello vhodnou možnost komentování out hello **Add-AzureAccount** příkaz.

![Prostředí PowerShell ISE s Azure modul nainstalovaný, připravené toorun skriptu.][30_powershell_ise]


### <a name="powershell-code"></a>Prostředí PowerShell kódu

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


Poznamenejte si hello několik pojmenovaných hodnot, které skript prostředí PowerShell hello vytiskne při jeho ukončení. Je nutné upravit tyto hodnoty do hello skript Transact-SQL, který následuje fáze 2.

## <a name="phase-2-transact-sql-code-that-uses-azure-storage-container"></a>Fáze 2: Kód Transact-SQL, který používá kontejner úložiště Azure

* V této ukázce kódu fáze 1 jste spustili toocreate skript prostředí PowerShell kontejner Azure Storage.
* Dále v fáze 2 hello následující skript jazyka Transact-SQL musíte použít hello kontejneru.

skript Hello začíná příkazy tooclean po předchozí možného spustit a je rerunnable.

Hello skript prostředí PowerShell vytištěny několik pojmenovaných hodnot, kdy se dokončila. Hello Transact-SQL skriptu toouse je nutné upravit tyto hodnoty. Najít **TODO** ve hello Transact-SQL skriptu toolocate hello upravit body.

1. Spusťte aplikaci SQL Server Management Studio (ssms.exe).
2. Připojte databáze Azure SQL Database tooyour.
3. Klikněte na tlačítko tooopen nové podokno dotazu.
4. Vložte následující skript jazyka Transact-SQL do podokna dotazu hello hello.
5. Najít každých **TODO** v hello skript a proveďte příslušné úpravy hello.
6. Uložte a pak spusťte skript hello.


> [!WARNING]
> Hello hodnotu klíče SAS generované hello předcházející skript prostředí PowerShell může začínat '?' (otazník). Pokud použijete klíč SAS hello v hello následující skriptu T-SQL, musíte *odebrat úvodní hello '?'* . V opačném případě mohou vaše úsilí blokovány zabezpečení.


### <a name="transact-sql-code"></a>Kód jazyka Transact-SQL

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


Pokud cílový hello selže tooattach při spuštění, musíte zastavit a restartovat relaci události hello:

```sql
ALTER EVENT SESSION ... STATE = STOP;
GO
ALTER EVENT SESSION ... STATE = START;
GO
```


## <a name="output"></a>Výstup

Po dokončení hello skript Transact-SQL, klikněte na buňku pod hello **event_data_XML** záhlaví sloupce. Jeden  **<event>**  element se zobrazuje, který zobrazuje jeden příkaz aktualizace.

Tady je jeden  **<event>**  element, který byl vygenerován při testování:


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


Hello předchozí hello používá skript Transact-SQL systému funkce tooread hello event_file následující:

* [Sys.fn_xe_file_target_read_file (Transact-SQL)](http://msdn.microsoft.com/library/cc280743.aspx)

Vysvětlení rozšířené možnosti pro hello zobrazení dat z rozšířených událostí je k dispozici na:

* [Rozšířené zobrazení cílová Data z rozšířených událostí](http://msdn.microsoft.com/library/mt752502.aspx)


## <a name="converting-hello-code-sample-toorun-on-sql-server"></a>Převádění toorun ukázkový kód hello na serveru SQL Server

Předpokládejme, že jste chtěli toorun hello předchozích ukázkových Transact-SQL na serveru Microsoft SQL Server.

* Pro jednoduchost, kterou by chcete toocompletely nahradit použít hello kontejneru úložiště Azure s jednoduchého souboru, jako **C:\myeventdata.xel**. soubor Hello by byla zapsána místní pevný disk toohello hello počítače, který je hostitelem systému SQL Server.
* Nebude potřeba jakýkoli druh příkazy jazyka Transact-SQL pro **CREATE MASTER KEY** a **vytvoření pověření**.
* V hello **vytvořit událost relace** příkaz v jeho **přidat cíl** klauzule, měli byste nahradit hello Http hodnotu přiřazenou provedené příliš**filename =** řetězcem úplnou cestu jako  **C:\MyFile.xel**.
  
  * Žádný účet úložiště Azure musí být zahrnut.

## <a name="more-information"></a>Další informace

Další informace o účtech a kontejnery v hello služby Azure Storage najdete v tématu:

* [Jak toouse úložiště Blob z rozhraní .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md)
* [Pojmenování a odkazování na kontejnerů, objektů BLOB a metadat](http://msdn.microsoft.com/library/azure/dd135715.aspx)
* [Práce s hello Kořenový kontejner](http://msdn.microsoft.com/library/azure/ee395424.aspx)
* [Lekce 1: Vytvoření zásady uložené přístup a sdílený přístupový podpis na kontejner Azure](http://msdn.microsoft.com/library/dn466430.aspx)
  * [Lekce 2: Vytvoření přihlašovacích údajů systému SQL Server pomocí sdíleného přístupového podpisu](http://msdn.microsoft.com/library/dn466435.aspx)

<!--
Image references.
-->

[30_powershell_ise]: ./media/sql-database-xevent-code-event-file/event-file-powershell-ise-b30.png

