---
title: "aaaManage Azure Data Lake Analytics pomocí Azure PowerShell | Microsoft Docs"
description: "Zjistěte, jak toomanage účtů Data Lake Analytics, datových zdrojů, úloh a položek katalogu. "
services: data-lake-analytics
documentationcenter: 
author: matt1883
manager: jhubbard
editor: cgronlun
ms.assetid: ad14d53c-fed4-478d-ab4b-6d2e14ff2097
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/23/2017
ms.author: mahi
ms.openlocfilehash: 5954f0efb7d5a9778727edfccae83aec046343bd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-data-lake-analytics-using-azure-powershell"></a>Správa Azure Data Lake Analytics pomocí Azure PowerShell
[!INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Zjistěte, jak toomanage účtů Azure Data Lake Analytics, datových zdrojů, úloh a položky katalogu pomocí prostředí Azure PowerShell. 

## <a name="prerequisites"></a>Požadavky

Při vytváření účtu Data Lake Analytics, je třeba tooknow:

* **ID předplatného**: hello ID předplatného Azure, ve kterém se nachází váš účet Data Lake Analytics.
* **Skupina prostředků**: hello název skupiny prostředků Azure hello, která obsahuje váš účet Data Lake Analytics.
* **Název účtu data Lake Analytics**: hello účet název musí obsahovat jenom malá písmena a číslice.
* **Výchozí účet Data Lake Store:** Každý účet Data Lake Analytics má výchozí účet Data Lake Store. Tyto účty musí být v hello stejné umístění.
* **Umístění**: hello umístění vašeho účtu Data Lake Analytics, jako je například "East US 2" nebo jiné podporované umístění. Podporovaná umístění můžete zobrazit na našem [stránce s cenami](https://azure.microsoft.com/pricing/details/data-lake-analytics/).

fragmenty kódu Hello prostředí PowerShell v tomto kurzu tyto informace použít tyto proměnné toostore

```powershell
$subId = "<SubscriptionId>"
$rg = "<ResourceGroupName>"
$adla = "<DataLakeAnalyticsAccountName>"
$adls = "<DataLakeStoreAccountName>"
$location = "<Location>"
```

## <a name="log-in"></a>Přihlásit se

Přihlaste se pomocí id předplatného.

```powershell
Login-AzureRmAccount -SubscriptionId $subId
```

Přihlaste se pomocí název odběru.

```
Login-AzureRmAccount -SubscriptionName $subname 
```

Hello `Login-AzureRmAccount` rutiny vždycky zobrazí výzvu k zadání pověření. Dotaz, zda pomocí následující rutiny hello se můžete vyhnout:

```powershell
# Save login session information
Save-AzureRmProfile -Path D:\profile.json  

# Load login session information
Select-AzureRmProfile -Path D:\profile.json 
```

## <a name="managing-accounts"></a>Správa účtů

### <a name="create-a-data-lake-analytics-account"></a>Vytvoření účtu Data Lake Analytics

Pokud ještě nemáte [skupiny prostředků](../azure-resource-manager/resource-group-overview.md#resource-groups) toouse, vytvořte ho. 

```powershell
New-AzureRmResourceGroup -Name  $rg -Location $location
```

Každý účet Data Lake Analytics vyžaduje výchozí účet Data Lake Store, který slouží k ukládání protokolů. Můžete znovu použít existující účet nebo si vytvořit účet. 

```powershell
New-AdlStore -ResourceGroupName $rg -Name $adls -Location $location
```

Jakmile budete mít k dispozici skupinu prostředků a účet Data Lake Store, vytvořte účet Data Lake Analytics.

```powershell
New-AdlAnalyticsAccount -ResourceGroupName $rg -Name $adla -Location $location -DefaultDataLake $adls
```

### <a name="get-information-about-an-account"></a>Získání informací o účtu

Získejte podrobnosti o účtu.

```powershell
Get-AdlAnalyticsAccount -Name $adla
```

Zkontrolujte existenci hello určitého účtu Data Lake Analytics. Hello rutina vrací buď `True` nebo `False`.

```powershell
Test-AdlAnalyticsAccount -Name $adla
```

Zkontrolujte existenci hello určitého účtu Data Lake Store. Hello rutina vrací buď `True` nebo `False`.

```powershell
Test-AdlStoreAccount -Name $adls
```

### <a name="listing-accounts"></a>Výpis účtů

Seznam Data Lake Analytics účtů v rámci aktuálního předplatného hello.

```powershell
Get-AdlAnalyticsAccount
```

Seznam Data Lake Analytics účtů v rámci určité skupiny zdrojů.

```powershell
Get-AdlAnalyticsAccount -ResourceGroupName $rg
```

## <a name="managing-firewall-rules"></a>Správa pravidel brány firewall

Zobrazí seznam pravidel brány firewall.

```powershell
Get-AdlAnalyticsFirewallRule -Account $adla
```

Přidejte pravidlo brány firewall.

```powershell
$ruleName = "Allow access from on-prem server"
$startIpAddress = "<start IP address>"
$endIpAddress = "<end IP address>"

Add-AdlAnalyticsFirewallRule -Account $adla -Name $ruleName -StartIpAddress $startIpAddress -EndIpAddress $endIpAddress
```

Změňte pravidlo brány firewall.

```powershell
Set-AdlAnalyticsFirewallRule -Account $adla -Name $ruleName -StartIpAddress $startIpAddress -EndIpAddress $endIpAddress
```

Odeberte pravidlo brány firewall.

```powershell
Remove-AdlAnalyticsFirewallRule -Account $adla -Name $ruleName
```



Povolit Azure IP adresy.

```powershell
Set-AdlAnalyticsAccount -Name $adla -AllowAzureIpState Enabled
```

```powershell
Set-AdlAnalyticsAccount -Name $adla -FirewallState Enabled
Set-AdlAnalyticsAccount -Name $adla -FirewallState Disabled
```

## <a name="managing-data-sources"></a>Správa zdrojů dat
Azure Data Lake Analytics teď podporuje hello následující zdroje dat:

* [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md)
* [Azure Storage](../storage/common/storage-introduction.md)

Když vytvoříte účet Analytics, je třeba určit Data Lake Store účtu toobe hello výchozí datový zdroj. Hello výchozího účtu Data Lake Store se používá protokoly auditu metadata toostore úlohy a úlohy. Po vytvoření účtu Data Lake Analytics, můžete přidat další účty Data Lake Store a účty úložiště. 

### <a name="find-hello-default-data-lake-store-account"></a>Najít hello výchozího účtu Data Lake Store

```powershell
$adla_acct = Get-AdlAnalyticsAccount -Name $adla
$dataLakeStoreName = $adla_acct.DefaultDataLakeAccount
```

Hello výchozího účtu Data Lake Store můžete najít pomocí filtrování hello seznam zdrojů dat pomocí hello `IsDefault` vlastnost:

```powershell
Get-AdlAnalyticsDataSource -Account $adla  | ? { $_.IsDefault } 
```

### <a name="add-a-data-source"></a>Přidání zdroje dat

```powershell

# Add an additional Storage (Blob) account.
$AzureStorageAccountName = "<AzureStorageAccountName>"
$AzureStorageAccountKey = "<AzureStorageAccountKey>"
Add-AdlAnalyticsDataSource -Account $adla -Blob $AzureStorageAccountName -AccessKey $AzureStorageAccountKey

# Add an additional Data Lake Store account.
$AzureDataLakeStoreName = "<AzureDataLakeStoreAccountName"
Add-AdlAnalyticsDataSource -Account $adla -DataLakeStore $AzureDataLakeStoreName 
```

### <a name="list-data-sources"></a>Seznam zdrojů dat

```powershell
# List all hello data sources
Get-AdlAnalyticsDataSource -Name $adla

# List attached Data Lake Store accounts
Get-AdlAnalyticsDataSource -Name $adla | where -Property Type -EQ "DataLakeStore"

# List attached Storage accounts
Get-AdlAnalyticsDataSource -Name $adla | where -Property Type -EQ "Blob"
```

## <a name="submit-u-sql-jobs"></a>Odesílání úloh U-SQL

### <a name="submit-a-string-as-a-u-sql-script"></a>Odeslat řetězec jako skript U-SQL

```powershell
$script = @"
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso", 1500.0),
            ("Woodgrove", 2700.0)
        ) AS D( customer, amount );
OUTPUT @a
    too"/data.csv"
    USING Outputters.Csv();
"@

$scriptpath = "d:\test.usql"
$script | Out-File $scriptpath 

Submit-AdlJob -AccountName $adla -Script $script -Name "Demo"
```


### <a name="submit-a-file-as-a-u-sql-script"></a>Odeslat soubor jako skript U-SQL

```powershell
$scriptpath = "d:\test.usql"
$script | Out-File $scriptpath 
Submit-AdlJob -AccountName $adla –ScriptPath $scriptpath -Name "Demo"
```

## <a name="list-jobs-in-an-account"></a>Seznam úloh v účtu

### <a name="list-all-hello-jobs-in-hello-account"></a>Zobrazí seznam všech úloh hello v účtu hello. 

výstup Hello zahrnuje hello aktuálně spuštěných úloh a tyto úlohy, které byly nedávno dokončeny.

```powershell
Get-AdlJob -Account $adla
```


### <a name="list-a-specific-number-of-jobs"></a>Seznam určitý počet úloh

Ve výchozím nastavení je hello seznam úloh seřadit na odeslání čas. Tak hello naposledy odeslání úlohy se zobrazí na prvním. Ve výchozím nastavení hello ADLA účet pamatuje úlohy na 180 dní, ale hello Ge AdlJob rutina ve výchozím nastavení vrátí hello pouze první 500. Pomocí - nejvyšší parametr toolist určitý počet úloh.

```powershell
$jobs = Get-AdlJob -Account $adla -Top 10
```


### <a name="list-jobs-based-on-hello-value-of-job-property"></a>Seznam úloh na základě hello hodnoty vlastnosti úlohy

Pomocí hello `-State` parametr. Zkombinováním některou z těchto hodnot:

* `Accepted`
* `Compiling`
* `Ended`
* `New`
* `Paused`
* `Queued`
* `Running`
* `Scheduling`
* `Start`

```powershell
# List hello running jobs
Get-AdlJob -Account $adla -State Running

# List hello jobs that have completed
Get-AdlJob -Account $adla -State Ended

# List hello jobs that have not started yet
Get-AdlJob -Account $adla -State Accepted,Compiling,New,Paused,Scheduling,Start
```

Použití hello `-Result` parametr toodetect zda zakončeno úlohy byl úspěšně dokončen. Obsahuje tyto hodnoty:

* Zrušena
* Se nezdařilo
* Žádný
* Úspěch

``` powershell
# List Successful jobs.
Get-AdlJob -Account $adla -State Ended -Result Succeeded

# List Failed jobs.
Get-AdlJob -Account $adla -State Ended -Result Failed
```


Hello `-Submitter` parametr pomáhá identifikovat, kdo odeslání úlohy.

```powershell
Get-AdlJob -Account $adla -Submitter "joe@contoso.com"
```

Hello `-SubmittedAfter` je užitečný při filtrování tooa časový rozsah.


```powershell
# List  jobs submitted in hello last day.
$d = [DateTime]::Now.AddDays(-1)
Get-AdlJob -Account $adla -SubmittedAfter $d

# List  jobs submitted in hello last seven day.
$d = [DateTime]::Now.AddDays(-7)
Get-AdlJob -Account $adla -SubmittedAfter $d
```

### <a name="common-scenarios-for-listing-jobs"></a>Časté scénáře pro seznam úloh


```
# List jobs submitted in hello last five days and that successfully completed.
$d = (Get-Date).AddDays(-5)
Get-AdlJob -Account $adla -SubmittedAfter $d -State Ended -Result Succeeded

# List all failed jobs submitted by "joe@contoso.com" within hello past seven days.
Get-AdlJob -Account $adla `
    -Submitter "joe@contoso.com" `
    -SubmittedAfter (Get-Date).AddDays(-7) `
    -Result Failed
```

## <a name="filtering-a-list-of-jobs"></a>Filtrování seznamu úloh

Jakmile máte seznam úloh v aktuální relaci prostředí PowerShell. Můžete použít normální prostředí PowerShell rutiny toofilter hello seznamu.

Filtr seznam úloh toohello úlohy odeslání v hello posledních 24 hodin

```
$upperdate = Get-Date
$lowerdate = $upperdate.AddHours(-24)
$jobs | Where-Object { $_.EndTime -ge $lowerdate }
```

Filtrovat seznam úloh toohello úlohy, které skončila v hello posledních 24 hodin

```
$upperdate = Get-Date
$lowerdate = $upperdate.AddHours(-24)
$jobs | Where-Object { $_.SubmitTime -ge $lowerdate }
```

Filtrovat seznam úloh toohello úlohy, které spuštění. Úloha může selhat v době kompilace - a proto se nespustí. Podívejme se na úlohy hello se nezdařilo, které ve skutečnosti spuštění a pak se nezdařilo.

```powershell
$jobs | Where-Object { $_.StartTime -ne $null }
```

### <a name="analyzing-a-list-of-jobs"></a>Analýza seznam úloh

Použití hello `Group-Object` rutiny tooanalyze seznam úloh.

```
# Count hello number of jobs by Submitter
$jobs | Group-Object Submitter | Select -Property Count,Name

# Count hello number of jobs by Result
$jobs | Group-Object Result | Select -Property Count,Name

# Count hello number of jobs by State
$jobs | Group-Object State | Select -Property Count,Name

#  Count hello number of jobs by DegreeOfParallelism
$jobs | Group-Object DegreeOfParallelism | Select -Property Count,Name
```
Při provádění analýzu, může být užitečné tooadd vlastnosti toohello úlohy objekty toomake filtrování a seskupení jednodušší. Hello následující fragment kódu ukazuje, jak tooannotate a informací o úloze s vypočítat vlastnosti.

```
function annotate_job( $j )
{
    $dic1 = @{
        Label='AUHours';
        Expression={ ($_.DegreeOfParallelism * ($_.EndTime-$_.StartTime).TotalHours)}}
    $dic2 = @{
        Label='DurationSeconds';
        Expression={ ($_.EndTime-$_.StartTime).TotalSeconds}}
    $dic3 = @{
        Label='DidRun';
        Expression={ ($_.StartTime -ne $null)}}

    $j2 = $j | select *, $dic1, $dic2, $dic3
    $j2
}

$jobs = Get-AdlJob -Account $adla -Top 10
$jobs = $jobs | %{ annotate_job( $_ ) }
```

## <a name="get-information-about-pipelines-and-recurrences"></a>Získání informací o kanálů a opakování

Použití hello `Get-AdlJobPipeline` rutiny toosee hello kanálu informace dříve odeslání úlohy.

```powershell
$pipelines = Get-AdlJobPipeline -Account $adla

$pipeline = Get-AdlJobPipeline -Account $adla -PipelineId "<pipeline ID>"
```

Použití hello `Get-AdlJobRecurrence` rutiny toosee hello opakování informace pro dříve odeslaný úlohy.

```powershell
$recurrences = Get-AdlJobRecurrence -Account $adla

$recurrence = Get-AdlJobRecurrence -Account $adla -RecurrenceId "<recurrence ID>"
```

## <a name="get-information-about-a-job"></a>Získání informací o úlohy

### <a name="get-job-status"></a>Získat stav úlohy

Získáte stav hello určité úlohy.

```powershell
Get-AdlJob -AccountName $adla -JobId $job.JobId
```

### <a name="examine-hello-job-outputs"></a>Zkontrolujte výstupy úlohy hello

Po ukončení úlohy hello, zkontrolujte, jestli hello výstupní soubor existuje tak, že uvedete hello souborů ve složce.

```powershell
Get-AdlStoreChildItem -Account $adls -Path "/"
```

## <a name="manage-running-jobs"></a>Spravovat spuštěné úlohy

### <a name="cancel-a-job"></a>Zrušení úlohy

```powershell
Stop-AdlJob -Account $adls -JobID $jobID
```

### <a name="wait-for-a-job-toofinish"></a>Počkejte toofinish úlohy

Místo opakující se `Get-AdlAnalyticsJob` , dokud nebude dokončeno úlohu, můžete použít hello `Wait-AdlJob` toowait rutiny pro úlohy tooend hello.

```powershell
Wait-AdlJob -Account $adla -JobId $job.JobId
```

## <a name="manage-compute-policies"></a>Správa zásad výpočetní

### <a name="list-existing-compute-policies"></a>Seznam existujících zásad výpočetní

Hello `Get-AdlAnalyticsComputePolicy` rutina načte informace o výpočetní zásady pro účet Data Lake Analytics.

```powershell
$policies = Get-AdlAnalyticsComputePolicy -Account $adla
```

### <a name="create-a-compute-policy"></a>Vytvořit zásadu výpočetní

Hello `New-AdlAnalyticsComputePolicy` rutina vytvoří novou zásadu výpočetní pro účet Data Lake Analytics. Tento příklad, že nastaví hello maximální dostupné toohello Austrálie zadané uživatele too50 a too250 s prioritou hello minimální úlohy.

```powershell
$userObjectId = (Get-AzureRmAdUser -SearchString "garymcdaniel@contoso.com").Id

New-AdlAnalyticsComputePolicy -Account $adla -Name "GaryMcDaniel" -ObjectId $objectId -ObjectType User -MaxDegreeOfParallelismPerJob 50 -MinPriorityPerJob 250
```

## <a name="check-for-hello-existence-of-a-file"></a>Kontrola hello existenci souboru.

```powershell
Test-AdlStoreItem -Account $adls -Path "/data.csv"
```

## <a name="uploading-and-downloading"></a>Odesílání a stahování

Nahrajte soubor.

```powershell
Import-AdlStoreItem -AccountName $adls -Path "c:\data.tsv" -Destination "/data_copy.csv" 
```

Nahrajte rekurzivnímu celou složku.

```powershell
Import-AdlStoreItem -AccountName $adls -Path "c:\myData\" -Destination "/myData/" -Recurse
```

Stažení souboru.

```powershell
Export-AdlStoreItem -AccountName $adls -Path "/data.csv" -Destination "c:\data.csv"
```

Stáhněte si rekurzivnímu celou složku.

```powershell
Export-AdlStoreItem -AccountName $adls -Path "/" -Destination "c:\myData\" -Recurse
```

> [!NOTE]
> Pokud hello nahrát nebo dojde k přerušení procesu stahování, můžete se pokusit tooresume hello proces spuštěný rutiny hello znovu s hello ``-Resume`` příznak.

## <a name="manage-catalog-items"></a>Spravovat položky katalogu

katalog Hello U-SQL je použité toostructure dat a kódu, takže může být sdílen skriptů U-SQL. katalog Hello umožňuje hello nejvyšší možný výkon s daty v Azure Data Lake. Další informace najdete v tématu [Použití katalogu U-SQL](data-lake-analytics-use-u-sql-catalog.md).

### <a name="list-items-in-hello-u-sql-catalog"></a>Položky seznamu v katalogu hello U-SQL

```powershell
# List U-SQL databases
Get-AdlCatalogItem -Account $adla -ItemType Database 

# List tables within a database
Get-AdlCatalogItem -Account $adla -ItemType Table -Path "database"

# List tables within a schema.
Get-AdlCatalogItem -Account $adla -ItemType Table -Path "database.schema"
```

Zobrazí seznam všech hello sestavení v všechny databáze hello ADLA účtu.

```powershell
$dbs = Get-AdlCatalogItem -Account $adla -ItemType Database

foreach ($db in $dbs)
{
    $asms = Get-AdlCatalogItem -Account $adla -ItemType Assembly -Path $db.Name

    foreach ($asm in $asms)
    {
        $asmname = "[" + $db.Name + "].[" + $asm.Name + "]"
        Write-Host $asmname
    }
}
```

### <a name="get-details-about-a-catalog-item"></a>Získání podrobností o položek katalogu

```powershell
# Get details of a table
Get-AdlCatalogItem  -Account $adla -ItemType Table -Path "master.dbo.mytable"

# Test existence of a U-SQL database.
Test-AdlCatalogItem  -Account $adla -ItemType Database -Path "master"
```

### <a name="create-credentials-in-a-catalog"></a>Vytvořit přihlašovací údaje v katalogu

V databázi U-SQL vytvořte objekt přihlašovacích údajů pro databázi hostované v Azure. V současné době jsou přihlašovací údaje U-SQL hello jediným typem položek katalogu, které můžete vytvořit pomocí prostředí PowerShell.

```powershell
$dbName = "master"
$credentialName = "ContosoDbCreds"
$dbUri = "https://contoso.database.windows.net:8080"

New-AdlCatalogCredential -AccountName $adla `
          -DatabaseName $db `
          -CredentialName $credentialName `
          -Credential (Get-Credential) `
          -Uri $dbUri
```

### <a name="get-basic-information-about-an-adla-account"></a>Získejte základní informace o účtu ADLA

Zadaný název účtu vyhledává hello následující kód základní informace o účtu hello

```
$adla_acct = Get-AdlAnalyticsAccount -Name "saveenrdemoadla"
$adla_name = $adla_acct.Name
$adla_subid = $adla_acct.Id.Split("/")[2]
$adla_sub = Get-AzureRmSubscription -SubscriptionId $adla_subid
$adla_subname = $adla_sub.Name
$adla_defadls_datasource = Get-AdlAnalyticsDataSource -Account $adla_name  | ? { $_.IsDefault } 
$adla_defadlsname = $adla_defadls_datasource.Name

Write-Host "ADLA Account Name" $adla_name
Write-Host "Subscription Id" $adla_subid
Write-Host "Subscription Name" $adla_subname
Write-Host "Defautl ADLS Store" $adla_defadlsname
Write-Host 

Write-Host '$subname' " = ""$adla_subname"" "
Write-Host '$subid' " = ""$adla_subid"" "
Write-Host '$adla' " = ""$adla_name"" "
Write-Host '$adls' " = ""$adla_defadlsname"" "
```

## <a name="working-with-azure"></a>Práce s Azure

### <a name="get-details-of-azurerm-errors"></a>Získat podrobnosti o chybách AzureRm

```powershell
Resolve-AzureRmError -Last
```

### <a name="verify-if-you-are-running-as-an-administrator"></a>Ověřte, zda používáte jako správce

```powershell
function Test-Administrator  
{  
    $user = [Security.Principal.WindowsIdentity]::GetCurrent();
    $p = New-Object Security.Principal.WindowsPrincipal $user
    $p.IsInRole([Security.Principal.WindowsBuiltinRole]::Administrator)  
}
```

### <a name="find-a-tenantid"></a>Najít TenantID

Z názvu předplatného:

```powershell
function Get-TenantIdFromSubcriptionName( [string] $subname )
{
    $sub = (Get-AzureRmSubscription -SubscriptionName $subname)
    $sub.TenantId
}

Get-TenantIdFromSubcriptionName "ADLTrainingMS"
```

Z id předplatného:

```powershell
function Get-TenantIdFromSubcriptionId( [string] $subid )
{
    $sub = (Get-AzureRmSubscription -SubscriptionId $subid)
    $sub.TenantId
}

$subid = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
Get-TenantIdFromSubcriptionId $subid
```

Z adresy domény jako je například "contoso.com"


```powershell
function Get-TenantIdFromDomain( $domain )
{
    $url = "https://login.windows.net/" + $domain + "/.well-known/openid-configuration"
    return (Invoke-WebRequest $url|ConvertFrom-Json).token_endpoint.Split('/')[3]
}

$domain = "contoso.com"
Get-TenantIdFromDomain $domain
```

### <a name="list-all-your-subscriptions-and-tenant-ids"></a>Zobrazí seznam všech odběrů a ID klienta

```powershell
$subs = Get-AzureRmSubscription
foreach ($sub in $subs)
{
    Write-Host $sub.Name "("  $sub.Id ")"
    Write-Host "`tTenant Id" $sub.TenantId
}
```

## <a name="create-a-data-lake-analytics-account-using-a-template"></a>Vytvoření účtu Data Lake Analytics pomocí šablony

Můžete také použít šablonu skupiny prostředků Azure pomocí hello následující skript prostředí PowerShell:

```powershell
$subId = "<Your Azure Subscription ID>"

$rg = "<New Azure Resource Group Name>"
$location = "<Location (such as East US 2)>"
$adls = "<New Data Lake Store Account Name>"
$adla = "<New Data Lake Analytics Account Name>"

$deploymentName = "MyDataLakeAnalyticsDeployment"
$armTemplateFile = "<LocalFolderPath>\azuredeploy.json"  # update hello JSON template path 

# Log in tooAzure
Login-AzureRmAccount -SubscriptionId $subId

# Create hello resource group
New-AzureRmResourceGroup -Name $rg -Location $location

# Create hello Data Lake Analytics account with hello default Data Lake Store account.
$parameters = @{"adlAnalyticsName"=$adla; "adlStoreName"=$adls}
New-AzureRmResourceGroupDeployment -Name $deploymentName -ResourceGroupName $rg -TemplateFile $armTemplateFile -TemplateParameterObject $parameters 
```

Další informace najdete v tématu [nasazení aplikace pomocí šablony Azure Resource Manageru](../azure-resource-manager/resource-group-template-deploy.md) a [šablon pro tvorbu Azure Resource Manageru](../azure-resource-manager/resource-group-authoring-templates.md).

**Příklad šablony**

Uložte následující text jako hello `.json` souboru a pak použijte hello předcházející šablony hello toouse skript prostředí PowerShell. 

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "adlAnalyticsName": {
      "type": "string",
      "metadata": {
        "description": "hello name of hello Data Lake Analytics account toocreate."
      }
    },
    "adlStoreName": {
      "type": "string",
      "metadata": {
        "description": "hello name of hello Data Lake Store account toocreate."
      }
    }
  },
  "resources": [
    {
      "name": "[parameters('adlStoreName')]",
      "type": "Microsoft.DataLakeStore/accounts",
      "location": "East US 2",
      "apiVersion": "2015-10-01-preview",
      "dependsOn": [ ],
      "tags": { }
    },
    {
      "name": "[parameters('adlAnalyticsName')]",
      "type": "Microsoft.DataLakeAnalytics/accounts",
      "location": "East US 2",
      "apiVersion": "2015-10-01-preview",
      "dependsOn": [ "[concat('Microsoft.DataLakeStore/accounts/',parameters('adlStoreName'))]" ],
      "tags": { },
      "properties": {
        "defaultDataLakeStoreAccount": "[parameters('adlStoreName')]",
        "dataLakeStoreAccounts": [
          { "name": "[parameters('adlStoreName')]" }
        ]
      }
    }
  ],
  "outputs": {
    "adlAnalyticsAccount": {
      "type": "object",
      "value": "[reference(resourceId('Microsoft.DataLakeAnalytics/accounts',parameters('adlAnalyticsName')))]"
    },
    "adlStoreAccount": {
      "type": "object",
      "value": "[reference(resourceId('Microsoft.DataLakeStore/accounts',parameters('adlStoreName')))]"
    }
  }
}
```

## <a name="next-steps"></a>Další kroky
* [Přehled služby Microsoft Azure Data Lake Analytics](data-lake-analytics-overview.md)
* Začínáme s Data Lake Analytics pomocí [portál Azure](data-lake-analytics-get-started-portal.md) | [prostředí Azure PowerShell](data-lake-analytics-get-started-powershell.md) | [2.0 rozhraní příkazového řádku](data-lake-analytics-get-started-cli2.md)
* Správa Azure Data Lake Analytics pomocí [portál Azure](data-lake-analytics-manage-use-portal.md) | [prostředí Azure PowerShell](data-lake-analytics-manage-use-powershell.md) | [rozhraní příkazového řádku](data-lake-analytics-manage-use-cli.md) 
