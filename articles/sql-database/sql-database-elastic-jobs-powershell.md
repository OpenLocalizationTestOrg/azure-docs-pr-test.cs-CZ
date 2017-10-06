---
title: "aaaCreate a spravovat elastické úlohy pomocí prostředí PowerShell | Microsoft Docs"
description: "Fondy Azure SQL Database toomanage používá prostředí PowerShell"
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
ms.assetid: 737d8d13-5632-4e18-9cb0-4d3b8a19e495
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: f6c18aecfa7e8c0b102a3b7cd2f266f5542ae400
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-sql-database-elastic-jobs-using-powershell-preview"></a>Vytvářet a spravovat úlohy elastické databáze SQL pomocí prostředí PowerShell (preview)

Hello rozhraní API prostředí PowerShell pro **úlohy elastické databáze** (ve verzi preview), umožňují definovat skupiny databází, na které budou spuštěny skripty. Tento článek ukazuje, jak toocreate a spravovat **úlohy elastické databáze** pomocí rutin prostředí PowerShell. V tématu [elastické úlohy přehled](sql-database-elastic-jobs-overview.md). 

## <a name="prerequisites"></a>Požadavky
* Předplatné Azure. Bezplatná zkušební verze, najdete v části [bezplatnou zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).
* Sadu databází, které jsou vytvořené pomocí nástrojů pro elastické databáze hello. V tématu [začít pracovat s nástroji elastické databáze](sql-database-elastic-scale-get-started.md).
* Azure Powershell Podrobné informace najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).
* **Elastické databáze úlohy** balíček prostředí PowerShell: najdete v části [úlohy instalace elastické databáze](sql-database-elastic-jobs-service-installation.md)

### <a name="select-your-azure-subscription"></a>Vyberte předplatné Azure
tooselect hello předplatné, je třeba Id předplatného (**- SubscriptionId**) nebo název odběru (**- Název_předplatného**). Pokud máte více předplatných můžete spustit hello **Get-AzureRmSubscription** rutiny a zkopírujte hello potřeby informace o předplatném ze sady výsledků hello. Až budete mít informace o vašem předplatném, spusťte následující příkaz tooset hello toto předplatné jako výchozí hello, konkrétně hello cíl pro vytváření a Správa úloh:

    Select-AzureRmSubscription -SubscriptionId {SubscriptionID}

Hello [prostředí PowerShell ISE](https://technet.microsoft.com/library/dd315244.aspx) se doporučuje pro využití toodevelop a spustit skripty prostředí PowerShell proti hello úlohy elastické databáze.

## <a name="elastic-database-jobs-objects"></a>Objekty úlohy elastické databáze.
Hello následující tabulce jsou uvedeny na všech typech objekt hello **úlohy elastické databáze** spolu s jeho popis a příslušná rozhraní API prostředí PowerShell.

<table style="width:100%">
  <tr>
    <th>Typ objektu</th>
    <th>Popis</th>
    <th>Související rozhraní API prostředí PowerShell</th>
  </tr>
  <tr>
    <td>Přihlašovací údaj</td>
    <td>Uživatelské jméno a heslo toouse při připojování toodatabases pro spouštění skriptů nebo aplikace DACPACs. <p>Hello heslo je zašifrováno před odesláním tooand ukládání do databáze elastické databáze úlohy hello.  hello service úlohy elastické databáze pomocí přihlašovacích údajů hello vytvořen a odesláno z hello instalační skript se dešifrovat heslo Hello.</td>
    <td><p>Get-AzureSqlJobCredential</p>
    <p>Nové AzureSqlJobCredential</p><p>Set-AzureSqlJobCredential</p></td></td>
  </tr>

  <tr>
    <td>Skript</td>
    <td>Příkaz Transact-SQL skriptu toobe používané pro spuštění v rámci celé databáze.  Hello skriptu by měl být vytvořené toobe idempotent, protože hello služby bude opakovat akci během spuštění skriptu hello při selhání.
    </td>
    <td>
    <p>Get-AzureSqlJobContent</p>
    <p>Get-AzureSqlJobContentDefinition</p>
    <p>Nové AzureSqlJobContent</p>
    <p>Set-AzureSqlJobContentDefinition</p>
    </td>
  </tr>

  <tr>
    <td>DACPAC</td>
    <td><a href="https://msdn.microsoft.com/library/ee210546.aspx">Aplikace na datové vrstvě </a> balíček toobe použít mezi databázemi.

    </td>
    <td>
    <p>Get-AzureSqlJobContent</p>
    <p>Nové AzureSqlJobContent</p>
    <p>Set-AzureSqlJobContentDefinition</p>
    </td>
  </tr>
  <tr>
    <td>Cílové databáze</td>
    <td>Databáze a serveru název polohovací tooan Azure SQL Database.

    </td>
    <td>
    <p>Get-AzureSqlJobTarget</p>
    <p>Nové AzureSqlJobTarget</p>
    </td>
  </tr>
  <tr>
    <td>Cíl horizontálního oddílu mapy</td>
    <td>Použít kombinaci cíl databáze a přihlašovacích údajů toobe toodetermine informace uložené v rámci mapování horizontálních elastické databáze.
    </td>
    <td>
    <p>Get-AzureSqlJobTarget</p>
    <p>Nové AzureSqlJobTarget</p>
    <p>Set-AzureSqlJobTarget</p>
    </td>
  </tr>
<tr>
    <td>Cíl vlastní kolekce</td>
    <td>Definované skupiny databází toocollectively použijte pro provedení.</td>
    <td>
    <p>Get-AzureSqlJobTarget</p>
    <p>Nové AzureSqlJobTarget</p>
    </td>
  </tr>
<tr>
    <td>Cíl podřízené vlastní kolekce</td>
    <td>Cílové databáze, který se odkazuje z vlastní kolekce.</td>
    <td>
    <p>Přidat AzureSqlJobChildTarget</p>
    <p>Odebrat AzureSqlJobChildTarget</p>
    </td>
  </tr>

<tr>
    <td>Úloha</td>
    <td>
    <p>Definice parametrů pro úlohu, která se dá použít tootrigger provádění nebo toofulfill plánu.</p>
    </td>
    <td>
    <p>Get-AzureSqlJob</p>
    <p>Nové AzureSqlJob</p>
    <p>Set-AzureSqlJob</p>
    </td>
  </tr>

<tr>
    <td>Provádění úlohy</td>
    <td>
    <p>Kontejner úlohy nezbytné toofulfill provádění skriptu nebo použití cíl tooa DACPAC pomocí přihlašovacích údajů pro připojení databáze s chybami zpracování v souladu zásady spouštění tooan.</p>
    </td>
    <td>
    <p>Get-AzureSqlJobExecution</p>
    <p>Počáteční AzureSqlJobExecution</p>
    <p>Stop-AzureSqlJobExecution</p>
    <p>Počkejte AzureSqlJobExecution</p>
  </tr>

<tr>
    <td>Provádění úloh úkolu</td>
    <td>
    <p>Jedna jednotka toofulfill pracovní úlohy.</p>
    <p>Pokud úkol není možné toosuccessfully spuštění, bude do protokolu hello výsledné zpráva o výjimce a nové odpovídající úkol bude vytvořen a spustit v souladu toohello zadané zásady spouštění.</p></p>
    </td>
    <td>
    <p>Get-AzureSqlJobExecution</p>
    <p>Počáteční AzureSqlJobExecution</p>
    <p>Stop-AzureSqlJobExecution</p>
    <p>Počkejte AzureSqlJobExecution</p>
  </tr>

<tr>
    <td>Zásady spouštění úlohy</td>
    <td>
    <p>Ovládací prvky úlohy vypršení časových limitů provádění, limity opakování a intervaly mezi opakovanými pokusy.</p>
    <p>Elastické databáze úlohy obsahuje výchozí zásady spouštění úlohy, což způsobí, že v podstatě nekonečné opakování selhání úkolů úloh s exponenciálního omezení rychlosti intervalů mezi každou opakování.</p>
    </td>
    <td>
    <p>Get-AzureSqlJobExecutionPolicy</p>
    <p>Nové AzureSqlJobExecutionPolicy</p>
    <p>Set-AzureSqlJobExecutionPolicy</p>
    </td>
  </tr>

<tr>
    <td>Plán</td>
    <td>
    <p>Čas na základě specifikace pro provádění tootake místo v opakovaném intervalu nebo najednou.</p>
    </td>
    <td>
    <p>Get-AzureSqlJobSchedule</p>
    <p>Nové AzureSqlJobSchedule</p>
    <p>Set-AzureSqlJobSchedule</p>
    </td>
  </tr>

<tr>
    <td>Aktivační události úlohy</td>
    <td>
    <p>Mapování mezi úlohu a provádění tootrigger úlohy plán, podle plánu toohello.</p>
    </td>
    <td>
    <p>Nové AzureSqlJobTrigger</p>
    <p>Odebrat AzureSqlJobTrigger</p>
    </td>
  </tr>
</table>

## <a name="supported-elastic-database-jobs-group-types"></a>Podporované úlohy elastické databáze skupiny typy
Hello úloha spustí skripty jazyka Transact-SQL (T-SQL) nebo aplikace DACPACs napříč skupinou databází. Úlohy po odeslaná toobe provést napříč požadovanou skupinu databází hello úlohy "rozšíří" hello na podřízené úlohy, kde každá má hello vůči jedné databáze ve skupině hello. 

Existují dva typy skupin, které můžete vytvořit: 

* [Mapování horizontálních](sql-database-elastic-scale-shard-map-management.md) skupiny: odeslaná tootarget mapu horizontálního oddílu po úlohu dotazuje hello horizontálního oddílu mapy toodetermine jeho aktuální sadu horizontálních oddílů hello úlohy a potom vytvoří podřízené úlohy pro každý horizontálního oddílu v mapě hello horizontálního oddílu.
* Vlastní skupiny kolekce: definované vlastní sadu databází. Když úloha cílem vlastní kolekce, vytvoří podřízené úlohy pro každou databázi aktuálně v hello vlastní kolekce.

## <a name="tooset-hello-elastic-database-jobs-connection"></a>tooset hello připojení úlohy elastické databáze
Připojení musí toobe sady toohello úlohy *řízení databáze* předchozí toousing hello úlohy rozhraní API. Tuto rutinu spustíte aktivuje toopop okno přihlašovacích údajů se požaduje hello uživatelské jméno a heslo, které vytvořili při instalaci úlohy elastické databáze. Všechny příklady uvedené v tomto tématu předpokládají, že už jsou hotové tento první krok.

Otevřete úloh připojení toohello elastické databáze:

    Use-AzureSqlJobConnection -CurrentAzureSubscription 

## <a name="encrypted-credentials-within-hello-elastic-database-jobs"></a>Zašifrované přihlašovací údaje v rámci úlohy elastické databáze hello
Přihlašovací údaje databáze lze vložit do úlohy hello *řízení databáze* s jeho heslo šifrované. Je nutné toostore pověření tooenable úlohy toobe provést později, (pomocí plány úloh).

Šifrování funguje prostřednictvím certifikát vytvořen jako součást hello instalační skript. Vytvoří Hello instalační skript a nahrávání hello certifikát do hello Azure Cloud Service k dešifrování hello uložený šifrovaná hesla. Cloudová služba Azure Hello později ukládá hello veřejný klíč v rámci úlohy hello *řízení databáze* což umožňuje hello rozhraní API prostředí PowerShell nebo portálu Azure Classic rozhraní tooencrypt poskytnuté heslo bez nutnosti hello certifikátu toobe místně nainstalován.

Hello pověření hesla jsou zašifrované a zabezpečené od uživatelů s objekty úloh databáze tooElastic oprávnění jen pro čtení. Ale je možné, uživatel se zlými úmysly s přístup pro čtení a zápis tooElastic databáze úlohy objekty tooextract heslo. Přihlašovací údaje jsou navrženou toobe opětovně použít napříč spuštění úlohy. Přihlašovací údaje jsou předány tootarget databáze, při navazování připojení. Aktuálně neexistují žádná omezení hello cílové databáze používané pro každý přihlašovací údaje, uživatel se zlými úmysly může přidat cíl databáze pro databázi v rámci hello uživatelem se zlými úmysly. Hello uživatel může následně spustit úlohu cílení na této databázi toogain hello pověření heslo.

Osvědčené postupy zabezpečení pro úlohy elastické databáze patří:

* Omezit využití hello rozhraní API tootrusted jednotlivce.
* Přihlašovací údaje by měl mít hello minimálně úkol hello tooperform nezbytná oprávnění.  Další informace si můžete prohlédnout v rámci to [autorizace a oprávnění](https://msdn.microsoft.com/library/bb669084.aspx) článku na webu MSDN SQL Server.

### <a name="toocreate-an-encrypted-credential-for-job-execution-across-databases"></a>toocreate šifrované pověření pro provádění úlohy mezi databázemi
toocreate nový šifrovat přihlašovací údaje, hello [ **rutiny Get-Credential** ](https://technet.microsoft.com/library/hh849815.aspx) vyzve k zadání uživatelského jména a hesla, které lze předat toohello [ **New-AzureSqlJobCredential rutiny**](/powershell/module/elasticdatabasejobs/new-azuresqljobcredential).

    $credentialName = "{Credential Name}"
    $databaseCredential = Get-Credential
    $credential = New-AzureSqlJobCredential -Credential $databaseCredential -CredentialName $credentialName
    Write-Output $credential

### <a name="tooupdate-credentials"></a>přihlašovací údaje tooupdate
Při změně hesla, použijte hello [ **rutiny Set-AzureSqlJobCredential** ](/powershell/module/elasticdatabasejobs/set-azuresqljobcredential) a sadu hello **CredentialName** parametr.

    $credentialName = "{Credential Name}"
    Set-AzureSqlJobCredential -CredentialName $credentialName -Credential $credential 

## <a name="toodefine-an-elastic-database-shard-map-target"></a>toodefine cíl elastické databáze horizontálního oddílu mapy
tooexecute úlohu pro všechny databáze v sadě horizontálního oddílu (vytvořený [klientské knihovny pro elastické databáze](sql-database-elastic-database-client-library.md)), použít mapování horizontálních jako cíl hello databáze. Tento příklad vyžaduje horizontálně dělené aplikace vytvořené pomocí klientské knihovny pro elastické databáze hello. V tématu [Začínáme s ukázkou nástroje elastické databáze](sql-database-elastic-scale-get-started.md).

Hello horizontálního oddílu mapa správce databáze musí být nastavena jako databáze cíl a poté mapy hello konkrétní horizontálního oddílu musí být zadány jako cíl.

    $shardMapCredentialName = "{Credential Name}"
    $shardMapDatabaseName = "{ShardMapDatabaseName}" #example: ElasticScaleStarterKit_ShardMapManagerDb
    $shardMapDatabaseServerName = "{ShardMapServerName}"
    $shardMapName = "{MyShardMap}" #example: CustomerIDShardMap
    $shardMapDatabaseTarget = New-AzureSqlJobTarget -DatabaseName $shardMapDatabaseName -ServerName $shardMapDatabaseServerName
    $shardMapTarget = New-AzureSqlJobTarget -ShardMapManagerCredentialName $shardMapCredentialName -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapDatabaseServerName -ShardMapName $shardMapName
    Write-Output $shardMapTarget

## <a name="create-a-t-sql-script-for-execution-across-databases"></a>Vytvoření skriptu T-SQL pro provedení mezi databází
Při vytváření skriptů T-SQL pro spuštění, důrazně doporučujeme toobuild je toobe [idempotent](https://en.wikipedia.org/wiki/Idempotence) a odolné proti selhání. Vždy, když dojde k selhání, bez ohledu na to hello klasifikace hello selhání spuštění, bude opakovat úlohy elastické databáze provádění skriptu.

Použití hello [ **rutiny New-AzureSqlJobContent** ](/powershell/module/elasticdatabasejobs/new-azuresqljobcontent) toocreate a uložíte skript pro spuštění a nastavte hello **- ContentName** a **- CommandText**parametry.

    $scriptName = "Create a TestTable"

    $scriptCommandText = "
    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'TestTable')
    BEGIN
        CREATE TABLE TestTable(
            TestTableId INT PRIMARY KEY IDENTITY,
            InsertionTime DATETIME2
        );
    END
    GO
    INSERT INTO TestTable(InsertionTime) VALUES (sysutcdatetime());
    GO"

    $script = New-AzureSqlJobContent -ContentName $scriptName -CommandText $scriptCommandText
    Write-Output $script

### <a name="create-a-new-script-from-a-file"></a>Vytvořit nový skript ze souboru
Pokud hello skriptu T-SQL je definována v souboru, použijte tento skript tooimport hello:

    $scriptName = "My Script Imported from a File"
    $scriptPath = "{Path tooSQL File}"
    $scriptCommandText = Get-Content -Path $scriptPath
    $script = New-AzureSqlJobContent -ContentName $scriptName -CommandText $scriptCommandText
    Write-Output $script

### <a name="tooupdate-a-t-sql-script-for-execution-across-databases"></a>skript tooupdate T-SQL pro provádění mezi databázemi
Tato aktualizace skript prostředí PowerShell text hello text příkazů T-SQL pro existující skript.

Sada hello následující proměnné tooreflect hello potřeby sadu toobe definice skriptu:

    $scriptName = "Create a TestTable"
    $scriptUpdateComment = "Adding AdditionalInformation column tooTestTable"
    $scriptCommandText = "
    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'TestTable')
    BEGIN
    CREATE TABLE TestTable(
        TestTableId INT PRIMARY KEY IDENTITY,
        InsertionTime DATETIME2
    );
    END
    GO

    IF NOT EXISTS (SELECT columns.name FROM sys.columns INNER JOIN sys.tables on columns.object_id = tables.object_id WHERE tables.name = 'TestTable' AND columns.name = 'AdditionalInformation')
    BEGIN
    ALTER TABLE TestTable
    ADD AdditionalInformation NVARCHAR(400);
    END
    GO

    INSERT INTO TestTable(InsertionTime, AdditionalInformation) VALUES (sysutcdatetime(), 'test');
    GO"

### <a name="tooupdate-hello-definition-tooan-existing-script"></a>tooupdate hello Definice tooan stávajícího skriptu
    Set-AzureSqlJobContentDefinition -ContentName $scriptName -CommandText $scriptCommandText -Comment $scriptUpdateComment 

## <a name="toocreate-a-job-tooexecute-a-script-across-a-shard-map"></a>toocreate tooexecute úlohy skriptu napříč horizontálního oddílu mapy
Tento skript prostředí PowerShell spustí úlohu pro spuštění skriptu mezi každou horizontálního oddílu v mapě horizontálního oddílu služby elastické škálování.

Sada hello následující proměnné tooreflect hello potřeby skriptu a cíle:

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $shardMapServerName = "{Shard Map Server Name}"
    $shardMapDatabaseName = "{Shard Map Database Name}"
    $shardMapName = "{Shard Map Name}"
    $credentialName = "{Credential Name}"
    $shardMapTarget = Get-AzureSqlJobTarget -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapServerName -ShardMapName $shardMapName 
    $job = New-AzureSqlJob -ContentName $scriptName -CredentialName $credentialName -JobName $jobName -TargetId $shardMapTarget.TargetId
    Write-Output $job

## <a name="tooexecute-a-job"></a>tooexecute úlohy
Tento skript PowerShell spouští stávající úloze:

Aktualizace hello proměnné tooreflect hello potřeby úlohy název toohave provést následující:

    $jobName = "{Job Name}"
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName 
    Write-Output $jobExecution

## <a name="tooretrieve-hello-state-of-a-single-job-execution"></a>Stav hello tooretrieve provádění jedné úlohy
Použití hello [ **rutiny Get-AzureSqlJobExecution** ](/powershell/module/elasticdatabasejobs/get-azuresqljobexecution) a sadu hello **JobExecutionId** parametr tooview hello stav provádění úlohy.

    $jobExecutionId = "{Job Execution Id}"
    $jobExecution = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId
    Write-Output $jobExecution

Použití hello stejné **Get-AzureSqlJobExecution** rutiny s hello **metoda IncludeChildren** parametr tooview hello stav spuštěních podřízené úlohy, a to hello určitý stav pro každé spuštění úlohy proti jednotlivým databáze cílové úlohou hello.

    $jobExecutionId = "{Job Execution Id}"
    $jobExecutions = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId -IncludeChildren
    Write-Output $jobExecutions 

## <a name="tooview-hello-state-across-multiple-job-executions"></a>Stav hello tooview mezi jednotlivými spuštěními více úloh
Hello [ **rutiny Get-AzureSqlJobExecution** ](/powershell/module/elasticdatabasejobs/new-azuresqljob) má více volitelné parametry, které se dají použít toodisplay více spuštěních úloh filtrované prostřednictvím hello zadané parametry. Následující Hello ukazuje některé možné způsoby, jak toouse hello Get-AzureSqlJobExecution:

Načtěte všechny aktivní nejvyšší úrovně úloha spuštění:

    Get-AzureSqlJobExecution

Načtení všech spuštěních nejvyšší úrovně úlohy, včetně spuštěních neaktivní úlohy:

    Get-AzureSqlJobExecution -IncludeInactive

Načtěte všechny podřízené úlohy spuštěních zadaná úloha spuštění ID, včetně spuštěních neaktivní úlohy:

    $parentJobExecutionId = "{Job Execution Id}"
    Get-AzureSqlJobExecution -AzureSqlJobExecution -JobExecutionId $parentJobExecutionId -IncludeInactive -IncludeChildren

Načíst všechny úlohy spuštěních vytvořený plán / úlohy kombinaci, včetně neaktivní úlohy:

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Get-AzureSqlJobExecution -JobName $jobName -ScheduleName $scheduleName -IncludeInactive

Načtěte všechny úlohy cílení na mapě zadaný horizontálního oddílu, včetně neaktivní úlohy:

    $shardMapServerName = "{Shard Map Server Name}"
    $shardMapDatabaseName = "{Shard Map Database Name}"
    $shardMapName = "{Shard Map Name}"
    $target = Get-AzureSqlJobTarget -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapServerName -ShardMapName $shardMapName
    Get-AzureSqlJobExecution -TargetId $target.TargetId -IncludeInactive

Načtěte všechny úlohy cílení na vlastní kolekce, včetně neaktivní úlohy:

    $customCollectionName = "{Custom Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    Get-AzureSqlJobExecution -TargetId $target.TargetId -IncludeInactive

Načtení seznamu hello spuštěních úloh úlohy v rámci provedení určité úlohy:

    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Write-Output $jobTaskExecutions 

Načtěte podrobnosti o provádění úkolů úlohy:

Následující skript prostředí PowerShell Hello lze použít tooview hello podrobnosti o provádění úloh úkolu, který je zvláště užitečná při ladění selhání spuštění.

    $jobTaskExecutionId = "{Job Task Execution Id}"
    $jobTaskExecution = Get-AzureSqlJobTaskExecution -JobTaskExecutionId $jobTaskExecutionId
    Write-Output $jobTaskExecution

## <a name="tooretrieve-failures-within-job-task-executions"></a>tooretrieve selhání v rámci úlohy spuštěních úloh
Hello **JobTaskExecution objekt** zahrnuje vlastnost hello životní cyklus úlohy hello společně s vlastností zpráv. Pokud se nezdařilo provádění úloh úkolu, vlastnost hello životního cyklu bude nastavena příliš*se nezdařilo* a vlastnost zprávy hello se nastaví toohello výsledné zpráva o výjimce a jeho zásobníku. Pokud úloha nebyla úspěšná, je důležité tooview hello podrobnosti úlohy, které se nezdařilo pro danou úlohu.

    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Foreach($jobTaskExecution in $jobTaskExecutions) 
        {
        if($jobTaskExecution.Lifecycle -ne 'Succeeded')
            {
            Write-Output $jobTaskExecution
            }
        }

## <a name="toowait-for-a-job-execution-toocomplete"></a>toowait pro provádění toocomplete úlohy
Hello následující skript prostředí PowerShell může být použité toowait pro toocomplete úlohy úlohy:

    $jobExecutionId = "{Job Execution Id}"
    Wait-AzureSqlJobExecution -JobExecutionId $jobExecutionId 

## <a name="create-a-custom-execution-policy"></a>Vytvořit zásadu vlastní spuštění
Elastické databáze úlohy podporuje vytváření vlastní provádění zásad, které mohou být použity při spouštění úloh.

Zásady spouštění aktuálně povolit pro definování:

* Název: Identifikátor pro zásady spouštění hello.
* Časový limit úlohy: Celkový čas před úlohy budou zrušeny úlohami elastické databáze.
* Počáteční Interval opakování: Interval toowait před první opakování.
* Maximální Interval opakování: Cap z toouse intervalech zkuste to znovu.
* Koeficient omezení rychlosti Interval opakování: Koeficient použít toocalculate hello další interval mezi opakovanými pokusy.  Hello použije následující vzorec: (počáteční opakujte Interval) * Math.pow ((Interval omezení rychlosti koeficient), (počet pokusů o) - 2). 
* Maximální počet pokusů: hello maximální počet opakování pokusů o tooperform v rámci úlohy.

Zásady spouštění výchozí Hello používá hello následující hodnoty:

* Název: Zásady spouštění výchozí
* Časový limit úlohy: 1 týden
* Počáteční Interval opakování: 100 milisekund
* Maximální Interval opakování: 30 minut
* Opakujte koeficient Interval: 2
* Maximální počet pokusů: 2 147 483 647

Vytvoření zásady spouštění hello potřeby:

    $executionPolicyName = "{Execution Policy Name}"
    $initialRetryInterval = New-TimeSpan -Seconds 10
    $jobTimeout = New-TimeSpan -Minutes 30
    $maximumAttempts = 999999
    $maximumRetryInterval = New-TimeSpan -Minutes 1
    $retryIntervalBackoffCoefficient = 1.5
    $executionPolicy = New-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval 
    -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
    Write-Output $executionPolicy

### <a name="update-a-custom-execution-policy"></a>Aktualizovat zásady vlastní spuštění
Aktualizace hello potřeby tooupdate zásad spouštění:

    $executionPolicyName = "{Execution Policy Name}"
    $initialRetryInterval = New-TimeSpan -Seconds 15
    $jobTimeout = New-TimeSpan -Minutes 30
    $maximumAttempts = 999999
    $maximumRetryInterval = New-TimeSpan -Minutes 1
    $retryIntervalBackoffCoefficient = 1.5
    $updatedExecutionPolicy = Set-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
    Write-Output $updatedExecutionPolicy

## <a name="cancel-a-job"></a>Zrušení úlohy
Elastické databáze úlohy podporuje požadavků na zrušení úloh.  Pokud úlohy elastické databáze zjistí žádost o zrušení úlohy se spouští, se pokusí toostop hello úlohy.

Že úlohy elastické databáze můžete provést zrušení dvěma různými způsoby:

1. Zrušit aktuálně spuštěných úloh: Pokud zrušení se zjistí, zatímco úloha je aktuálně spuštěna, zrušení se pokusí v rámci hello aktuálně spuštěných aspekt úlohy hello.  Například: Pokud je aktuálně provést při pokusu o zrušení dlouho spuštěných dotazu, bude dotaz pokus o toocancel hello.
2. Zrušení úloh opakování: v případě zrušení zjištění hello řízení vlákno předtím, než se spustí úloha pro spuštění, bude hello řízení vláken vyhnout, spouštění úloh hello a deklarovat hello žádost jako zrušená.

Pokud zrušení úlohy je požadováno pro nadřazené úloze, bude požadavek na zrušení hello dodržet pro hello nadřazené úloze a všechny jeho podřízené úlohy.

toosubmit žádost o zrušení použít hello [ **rutinu Stop-AzureSqlJobExecution** ](/powershell/module/elasticdatabasejobs/stop-azuresqljobexecution) a sadu hello **JobExecutionId** parametr.

    $jobExecutionId = "{Job Execution Id}"
    Stop-AzureSqlJobExecution -JobExecutionId $jobExecutionId

## <a name="toodelete-a-job-and-job-history-asynchronously"></a>toodelete úlohy a úlohy historie asynchronně
Elastické databáze úlohy podporuje asynchronní odstranění úloh. Úloha může být označený k odstranění a hello systému odstraní hello úlohy a všechny jeho historie úlohy po dokončení všech spuštěních úloh pro úlohu hello. Hello systému nebude automaticky zrušit spuštěních aktivní úlohy.  

Vyvolání [ **Stop-AzureSqlJobExecution** ](/powershell/module/elasticdatabasejobs/stop-azuresqljobexecution) spuštěních toocancel aktivní úlohy.

Odstranění úlohy tootrigger, použijte hello [ **rutinu Remove-AzureSqlJob** ](/powershell/module/elasticdatabasejobs/remove-azuresqljob) a sadu hello **JobName** parametr.

    $jobName = "{Job Name}"
    Remove-AzureSqlJob -JobName $jobName

## <a name="toocreate-a-custom-database-target"></a>toocreate cíl vlastní databázi
Můžete definovat vlastní databázi cíle pro přímé spouštění nebo pro zahrnutí do skupiny vlastní databázi. Například protože **elastické fondy** není dosud podporován přímo pomocí rozhraní API prostředí PowerShell, můžete vytvořit vlastní databázi cíle a cílové kolekce vlastní databázi, který zahrnuje všechny hello databáze ve fondu hello.

Nastavte následující informace o databázi proměnné tooreflect hello potřeby hello:

    $databaseName = "{Database Name}"
    $databaseServerName = "{Server Name}"
    New-AzureSqlJobDatabaseTarget -DatabaseName $databaseName -ServerName $databaseServerName 

## <a name="toocreate-a-custom-database-collection-target"></a>toocreate cíl kolekce vlastní databázi
Použití hello [ **New-AzureSqlJobTarget** ](/powershell/module/elasticdatabasejobs/new-azuresqljobtarget) toodefine rutiny vlastní databázi kolekce tooenable provádění cílů napříč více definovaných databázových cílů. Po vytvoření databáze skupiny, může být přidružen hello vlastní kolekce cílové databáze.

Nastavte hello následující konfigurace cílového proměnné tooreflect hello požadovanou vlastní kolekce:

    $customCollectionName = "{Custom Database Collection Name}"
    New-AzureSqlJobTarget -CustomCollectionName $customCollectionName 

### <a name="tooadd-databases-tooa-custom-database-collection-target"></a>tooadd databáze tooa vlastní databázi kolekce cíl
tooadd databáze tooa určité vlastní kolekci pomocí hello [ **přidat AzureSqlJobChildTarget** ](/powershell/module/elasticdatabasejobs/add-azuresqljobchildtarget) rutiny.

    $databaseServerName = "{Database Server Name}"
    $databaseName = "{Database Name}"
    $customCollectionName = "{Custom Database Collection Name}"
    Add-AzureSqlJobChildTarget -CustomCollectionName $customCollectionName -DatabaseName $databaseName -ServerName $databaseServerName 

#### <a name="review-hello-databases-within-a-custom-database-collection-target"></a>Zkontrolujte hello databází v rámci kolekce cíl vlastní databázi
Použití hello [ **Get-AzureSqlJobTarget** ](/powershell/module/elasticdatabasejobs/new-azuresqljobtarget) rutiny tooretrieve hello podřízené databází v rámci kolekce cíl vlastní databázi. 

    $customCollectionName = "{Custom Database Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $childTargets = Get-AzureSqlJobTarget -ParentTargetId $target.TargetId
    Write-Output $childTargets

### <a name="create-a-job-tooexecute-a-script-across-a-custom-database-collection-target"></a>Vytvořit úlohu tooexecute skript pro cílovou kolekci vlastní databázi
Použití hello [ **New-AzureSqlJob** ](/powershell/module/elasticdatabasejobs/new-azuresqljob) rutiny toocreate úloh pro skupinu databází definované cílovou kolekci vlastní databázi. Úlohy elastické databáze bude rozšiřovat hello úlohy do více podřízených úloh každé příslušné databáze s tooa přidružený cílové kolekce hello vlastní databázi a zkontrolujte, zda je pro každou databázi hello skriptu. Znovu je důležité, aby skripty jsou odolné tooretries idempotent toobe.

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $customCollectionName = "{Custom Collection Name}"
    $credentialName = "{Credential Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job

## <a name="data-collection-across-databases"></a>Shromažďování dat mezi databázemi
Můžete použít tooexecute úlohu dotazu v rámci skupiny databází a poslat hello výsledky tooa určité tabulky. Tabulka Hello můžete položit dotaz na po hello fakt toosee hello výsledků dotazu z každé databáze. To poskytuje asynchronní metody tooexecute dotazu mezi mnoha databázemi. Neúspěšných pokusů o přihlášení se zpracovávají automaticky prostřednictvím opakování.

Hello zadané cílové tabulky se automaticky vytvoří, pokud ještě neexistuje. Nová tabulka Hello odpovídá hello schéma hello vrátila sadu výsledků. Pokud skript vrátí více sad výsledků dotazu, úlohy elastické databáze odešle hello první toohello cílové tabulky.

Hello následující skript PowerShell spouští skript a shromažďuje své výsledky do zadané tabulky. Tento skript předpokládá, že skriptu T-SQL byl vytvořen který výstupy jedné sadě výsledků a vytvořený cíl kolekce vlastní databázi.

Tento skript používá hello [ **Get-AzureSqlJobTarget** ](/powershell/module/elasticdatabasejobs/new-azuresqljobtarget) rutiny. Nastavte hello parametry skriptu, přihlašovací údaje a provádění cíle:

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $executionCredentialName = "{Execution Credential Name}"
    $customCollectionName = "{Custom Collection Name}"
    $destinationCredentialName = "{Destination Credential Name}"
    $destinationServerName = "{Destination Server Name}"
    $destinationDatabaseName = "{Destination Database Name}"
    $destinationSchemaName = "{Destination Schema Name}"
    $destinationTableName = "{Destination Table Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName

### <a name="toocreate-and-start-a-job-for-data-collection-scenarios"></a>toocreate a spusťte úlohu pro scénáře kolekce dat
Tento skript používá hello [ **Start-AzureSqlJobExecution** ](/powershell/module/elasticdatabasejobs/start-azuresqljobexecution) rutiny.

    $job = New-AzureSqlJob -JobName $jobName 
    -CredentialName $executionCredentialName 
    -ContentName $scriptName 
    -ResultSetDestinationServerName $destinationServerName 
    -ResultSetDestinationDatabaseName $destinationDatabaseName 
    -ResultSetDestinationSchemaName $destinationSchemaName 
    -ResultSetDestinationTableName $destinationTableName 
    -ResultSetDestinationCredentialName $destinationCredentialName 
    -TargetId $target.TargetId
    Write-Output $job
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName
    Write-Output $jobExecution

## <a name="tooschedule-a-job-execution-trigger"></a>tooschedule provádění aktivační události úlohy
Hello následující skript prostředí PowerShell se dá použít toocreate plánu opakování. Tento skript používá intervalu minutu, ale [ **New-AzureSqlJobSchedule** ](/powershell/module/elasticdatabasejobs/new-azuresqljobschedule) také podporuje – DayInterval, - HourInterval, - MonthInterval a - WeekInterval parametry. Plány, které jsou spouštěny pouze jednou lze vytvořit pomocí předávání - jednorázově.

Vytvoření nového plánu:

    $scheduleName = "Every one minute"
    $minuteInterval = 1
    $startTime = (Get-Date).ToUniversalTime()
    $schedule = New-AzureSqlJobSchedule 
    -MinuteInterval $minuteInterval 
    -ScheduleName $scheduleName 
    -StartTime $startTime 
    Write-Output $schedule

### <a name="tootrigger-a-job-executed-on-a-time-schedule"></a>tootrigger úlohu provést podle časového plánu
Aktivační události úlohy může být definovaná toohave časového plánu podle tooa úlohu provést. Následující skript prostředí PowerShell Hello lze použít toocreate aktivační události úlohy.

Použití [New-AzureSqlJobTrigger](/powershell/module/elasticdatabasejobs/new-azuresqljobtrigger) a sadu hello následující proměnné toocorrespond toohello požadované úlohy a plán:

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    $jobTrigger = New-AzureSqlJobTrigger
    -ScheduleName $scheduleName
    -JobName $jobName
    Write-Output $jobTrigger

### <a name="tooremove-a-scheduled-association-toostop-job-from-executing-on-schedule"></a>tooremove úlohu naplánované přidružení toostop ve spouštění podle plánu.
toodiscontinue nadále provádění úlohy prostřednictvím aktivační události úlohy, aktivační události úlohy hello lze odebrat. Odebrání toostop aktivační události úlohy úloha spouštěna podle plánu tooa pomocí hello [ **rutinu Remove-AzureSqlJobTrigger**](/powershell/module/elasticdatabasejobs/remove-azuresqljobtrigger).

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Remove-AzureSqlJobTrigger 
    -ScheduleName $scheduleName 
    -JobName $jobName

### <a name="retrieve-job-triggers-bound-tooa-time-schedule"></a>Načtení úlohy aktivační události vázané tooa časového plánu
Hello následující skript prostředí PowerShell může být použité tooobtain a zobrazit hello úlohy aktivační události registrované tooa konkrétního časového plánu.

    $scheduleName = "{Schedule Name}"
    $jobTriggers = Get-AzureSqlJobTrigger -ScheduleName $scheduleName
    Write-Output $jobTriggers

### <a name="tooretrieve-job-triggers-bound-tooa-job"></a>aktivační události úlohy tooretrieve vázaný tooa úlohy
Použití [Get-AzureSqlJobTrigger](/powershell/module/elasticdatabasejobs/get-azuresqljobtrigger) plány tooobtain a zobrazení obsahující úlohu registrované.

    $jobName = "{Job Name}"
    $jobTriggers = Get-AzureSqlJobTrigger -JobName $jobName
    Write-Output $jobTriggers

## <a name="toocreate-a-data-tier-application-dacpac-for-execution-across-databases"></a>toocreate aplikace na datové vrstvě (DACPAC) pro provedení mezi databázemi
toocreate DACPAC, najdete v části [aplikace datové vrstvy](https://msdn.microsoft.com/library/ee210546.aspx). toodeploy DACPAC, použijte hello [rutiny New-AzureSqlJobContent](/powershell/module/elasticdatabasejobs/new-azuresqljobcontent). Hello DACPAC musí být přístupné toohello služba. Je doporučeno tooupload vytvořený tooAzure DACPAC úložiště a vytvořte [sdíleného přístupového podpisu](../storage/common/storage-dotnet-shared-access-signature-part-1.md) pro hello DACPAC.

    $dacpacUri = "{Uri}"
    $dacpacName = "{Dacpac Name}"
    $dacpac = New-AzureSqlJobContent -DacpacUri $dacpacUri -ContentName $dacpacName 
    Write-Output $dacpac

### <a name="tooupdate-a-data-tier-application-dacpac-for-execution-across-databases"></a>tooupdate aplikace na datové vrstvě (DACPAC) pro provedení mezi databázemi
Existující DACPACs zaregistrován v rámci úlohy elastické databáze může být aktualizované toopoint toonew identifikátory URI. Použití hello [ **rutiny Set-AzureSqlJobContentDefinition** ](/powershell/module/elasticdatabasejobs/set-azuresqljobcontentdefinition) tooupdate hello DACPAC URI na existujícím zaregistrován DACPAC:

    $dacpacName = "{Dacpac Name}"
    $newDacpacUri = "{Uri}"
    $updatedDacpac = Set-AzureSqlJobDacpacDefinition -ContentName $dacpacName -DacpacUri $newDacpacUri
    Write-Output $updatedDacpac

## <a name="toocreate-a-job-tooapply-a-data-tier-application-dacpac-across-databases"></a>toocreate tooapply úlohy aplikace na datové vrstvě (DACPAC) mezi databázemi
Po vytvoření DACPAC v rámci úlohy elastické databáze, můžete úlohu vytvořit tooapply hello DACPAC napříč skupinou databází. Hello následující skript prostředí PowerShell může být použité toocreate úlohu DACPAC v kolekci vlastních databází:

    $jobName = "{Job Name}"
    $dacpacName = "{Dacpac Name}"
    $customCollectionName = "{Custom Collection Name}"
    $credentialName = "{Credential Name}"
    $target = Get-AzureSqlJobTarget 
    -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob 
    -JobName $jobName 
    -CredentialName $credentialName 
    -ContentName $dacpacName -TargetId $target.TargetId
    Write-Output $job 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-powershell/cmd-prompt.png
[2]: ./media/sql-database-elastic-jobs-powershell/portal.png
<!--anchors-->
