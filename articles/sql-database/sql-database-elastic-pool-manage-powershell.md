---
title: "Prostředí PowerShell: Vytvoření a správa fondu elastické databáze Azure SQL | Microsoft Docs"
description: "Další informace o použití Powershellu ke správě fondu elastické databáze."
services: sql-database
documentationcenter: 
author: srinia
manager: jhubbard
editor: 
ms.assetid: 61289770-69b9-4ae3-9252-d0e94d709331
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: powershell
ms.workload: data-management
ms.date: 06/06/2017
ms.author: srinia
ms.openlocfilehash: 5e76397c62e5a6ff7fb356bd81218c307f3fda31
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-and-manage-an-elastic-pool-with-powershell"></a>Vytvoření a správa fondu elastické databáze pomocí prostředí PowerShell
Toto téma ukazuje, jak vytvořit a spravovat škálovatelné [elastické fondy](sql-database-elastic-pool.md) pomocí prostředí PowerShell.  Můžete také vytvořit a spravovat pomocí služby Azure elastického fondu [portál Azure](https://portal.azure.com/), REST API nebo [C#](sql-database-elastic-pool-manage-csharp.md). Můžete také vytvořit a přesun databáze do a z elastické fondy pomocí [Transact-SQL](sql-database-elastic-pool-manage-tsql.md).

[!INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## <a name="create-an-elastic-pool"></a>Vytvoření elastického fondu
[New-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/new-azurermsqlelasticpool) rutina vytvoří fondu elastické databáze. Hodnoty eDTU na fond a minimální a maximální Dtu jsou omezeny hodnota vrstvy služby (Basic, Standard, Premium nebo Premium RS). V tématu [omezení eDTU a úložiště pro elastické fondy a databází ve fondu](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools).

```PowerShell
New-AzureRmSqlElasticPool -ResourceGroupName "resourcegroup1" -ServerName "server1" -ElasticPoolName "elasticpool1" -Edition "Standard" -Dtu 400 -DatabaseDtuMin 10 -DatabaseDtuMax 100
```

## <a name="create-a-pooled-database-in-an-elastic-pool"></a>Vytvoření databáze ve fondu v elastickém fondu
Použijte rutinu [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase) a nastavte parametr **ElasticPoolName** na cílový fond. Chcete-li do pružného fondu přesunout existující databázi, přečtěte si téma [přesun databáze do pružného fondu](sql-database-elastic-pool-manage-powershell.md#move-a-database-into-an-elastic-pool).

```PowerShell
New-AzureRmSqlDatabase -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"
```

### <a name="complete-script"></a>Celý skript
Tento skript vytvoří skupinu prostředků Azure a serverem. Po výzvě zadejte jméno a heslo správce pro nový server (nikoli vaše přihlašovací údaje Azure).

```PowerShell
$subscriptionId = '<your Azure subscription id>'
$resourceGroupName = '<resource group name>'
$location = '<datacenter location>'
$serverName = '<server name>'
$poolName = '<pool name>'
$databaseName = '<database name>'

Login-AzureRmAccount
Set-AzureRmContext -SubscriptionId $subscriptionId

New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
New-AzureRmSqlServer -ResourceGroupName $resourceGroupName -ServerName $serverName -Location $location -ServerVersion "12.0"
New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $serverName -FirewallRuleName "rule1" -StartIpAddress "192.168.0.198" -EndIpAddress "192.168.0.199"

New-AzureRmSqlElasticPool -ResourceGroupName $resourceGroupName -ServerName $serverName -ElasticPoolName $poolName -Edition "Standard" -Dtu 400 -DatabaseDtuMin 10 -DatabaseDtuMax 100

New-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName -ElasticPoolName $poolName -MaxSizeBytes 10GB
```

## <a name="create-an-elastic-pool-and-add-multiple-pooled-databases"></a>Vytvoření fondu elastické databáze a přidání více databází ve fondu
Vytváření mnoho databází v elastickém fondu může trvat dobu, pokud se provádí pomocí portálu nebo rutiny prostředí PowerShell, který současně vytvořit pouze jednu databázi. K automatizaci vytváření fondu elastické databáze, najdete v části [CreateOrUpdateElasticPoolAndPopulate ](https://gist.github.com/billgib/d80c7687b17355d3c2ec8042323819ae).

## <a name="move-a-database-into-an-elastic-pool"></a>Přesun databáze do pružného fondu
Přesouváte databázi do nebo z fondu elastické databáze s [Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqlelasticpool).

```PowerShell
Set-AzureRmSqlDatabase -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"
```

## <a name="change-performance-settings-of-an-elastic-pool"></a>Změňte nastavení výkonu elastického fondu
Když ke snížení výkonu, můžete změnit nastavení fondu pro přizpůsobení růstu. Použití [Set-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/set-azurermsqlelasticpool) rutiny. Nastavte parametr - Dtu na Edtu na fond. V tématu [omezení eDTU a úložiště](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools) pro možné hodnoty.

```PowerShell
Set-AzureRmSqlElasticPool -ResourceGroupName “resourcegroup1” -ServerName “server1” -ElasticPoolName “elasticpool1” -Dtu 1200 -DatabaseDtuMax 100 -DatabaseDtuMin 50
```

## <a name="change-the-storage-limit-for-an-elastic-pool"></a>Změnit limit úložiště fondu elastické databáze

Použití [Set-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/set-azurermsqlelasticpool) rutiny nastavit _- StorageMB_ parametr. Zadejte limit úložiště v MB (například 2097152 sady úložiště omezit na 2 TB). V tématu [omezení eDTU a úložiště](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools) pro možné hodnoty.

> [!IMPORTANT]
> Výchozí maximální úložiště na každý fond pro fondy Premium s 1 500 Edtu nebo více je 750 GB. Získat vyšší _maximální velikost úložiště dat na každý fond_, musí být explicitně nastaveny limit úložiště. Premium fondy s více než 750 GB úložiště je aktuálně ve verzi public preview v následujících oblastech: východní USA 2, západ USA, Virginia nám verze pro státní správu, západní Evropa, Německo centrální, jihovýchodní Asie, Japonsko – východ, Austrálie – východ, Kanada centrální a Východní Kanada.

```PowerShell
Set-AzureRmSqlElasticPool -ServerName "server1" -ElasticPoolName “elasticpool1” -StorageMB 2097152
```

## <a name="get-the-status-of-pool-operations"></a>Načíst stav operace fondu
Vytvoření fondu elastické databáze může trvat dobu. Chcete-li sledovat stav fondu operacím, včetně vytváření a aktualizace, použijte [Get-AzureRmSqlElasticPoolActivity](/powershell/module/azurerm.sql/get-azurermsqlelasticpoolactivity) rutiny.

```PowerShell
Get-AzureRmSqlElasticPoolActivity -ResourceGroupName “resourcegroup1” -ServerName “server1” -ElasticPoolName “elasticpool1”
```

## <a name="get-the-status-of-moving-a-database-into-and-out-of-an-elastic-pool"></a>Získat stav přesun databáze do a z fondu elastické databáze
Přesunutí databáze může trvat dobu. Sleduje stav přesunutí pomocí [Get-AzureRmSqlDatabaseActivity](/powershell/module/azurerm.sql/get-azurermsqldatabaseactivity) rutiny.

```PowerShell
Get-AzureRmSqlDatabaseActivity -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"
```

## <a name="get-resource-usage-data-for-an-elastic-pool"></a>Získat data použití prostředků fondu elastické databáze
Metriky, které mohou být načteny jako procento limit fondu prostředků:

| Název metriky | Popis |
|:--- |:--- |
| Procesor\_procent |Průměr výpočetní využití v procentech limit fondu. |
| fyzické\_data\_číst\_procent |Průměrné využití vstupně-výstupních operací v procentech na základě limitu fondu. |
| protokol\_zápisu\_procent |Průměr zápisu využití prostředků v procentech limit fondu. |
| DTU\_spotřeba\_procent |Průměrná eDTU využití v procentech omezení eDTU pro fond |
| úložiště\_procent |Průměrné využití úložiště v procentech limit úložiště fondu. |
| pracovníci\_procent |Maximální souběžných pracovních procesů (počet požadavků) v procentech na základě limitu fondu. |
| relace\_procent |Maximální počet souběžných relací v procentech na základě limitu fondu. |
| eDTU_limit |Aktuální maximální elastický fond DTU nastavení pro tento elastický fond během tohoto intervalu. |
| úložiště\_limit |Aktuální maximální elastický fond úložiště limit nastavení pro toto elastického fondu v megabajtech během tohoto intervalu. |
| eDTU\_použít |Průměrná Edtu používané fondu v tomto intervalu. |
| úložiště\_použít |Průměrná úložiště používané fondu v tomto intervalu v bajtech |

**Metriky členitosti nebo období:**

* Data jsou vrácena v členitosti 5 minut.  
* Doba uchování dat je 35 dnů.  

Tato rutina a rozhraní API omezí počet řádků, které mohou být načteny jedno volání na 1000 řádky (o 3 dny dat ve členitosti 5 minut). Tento příkaz lze volat vícekrát s jinou počáteční nebo koncové časové intervaly pro načtení další data, ale

Chcete-li získat metriky:

```PowerShell
$metrics = (Get-AzureRmMetric -ResourceId /subscriptions/<subscriptionId>/resourceGroups/FabrikamData01/providers/Microsoft.Sql/servers/fabrikamsqldb02/elasticPools/franchisepool -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime "4/18/2015" -EndTime "4/21/2015")
```

## <a name="get-resource-usage-data-for-a-database-in-an-elastic-pool"></a>Získat data použití prostředků pro databáze v elastickém fondu
Tato rozhraní API jsou stejné jako rozhraní API používaná pro sledování využití prostředků služby jedné databáze, s výjimkou následujících sémantického rozdíl: metriky načíst jsou vyjádřené jako procentní podíl každou databázi max Edtu (nebo ekvivalentní zakončení pro základní metriku jako procesoru nebo vstupně-výstupní operace) nastavte pro tento fond. Například 50 % využití každého tyto metriky označuje, že spotřeba konkrétní prostředků je při 50 % za limitu cap databáze pro tento prostředek ve fondu nadřazené.

Chcete-li získat metriky:

```PowerShell
$metrics = (Get-AzureRmMetric -ResourceId /subscriptions/<subscriptionId>/resourceGroups/FabrikamData01/providers/Microsoft.Sql/servers/fabrikamsqldb02/databases/myDB -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime "4/18/2015" -EndTime "4/21/2015")
```

## <a name="add-an-alert-to-an-elastic-pool-resource"></a>Přidání oznámení na prostředek elastického fondu
Pravidla výstrah můžete přidat do fondu elastické databáze k odeslání e-mailových oznámení nebo výstrah řetězce [adresy URL koncových bodů](https://msdn.microsoft.com/library/mt718036.aspx) při elastický fond dotkne prahovou hodnotu využití, které jste nastavili. Použijte rutinu Add-AzureRmMetricAlertRule.

> [!IMPORTANT]
> Monitorování pro elastické fondy využití prostředků má prodlení alespoň 5 minut. Nastavení výstrah menší než 10 minut pro elastické fondy není aktuálně podporován. Nastavit všechny výstrahy pro elastické fondy s dobou (parametr s názvem "-velikost_okna" v rozhraní API prostředí PowerShell) menší než 10 minut nemusí aktivovat. Ujistěte se, že všechny výstrahy definujete pro elastické fondy použít dobu 10 minut (WindowSize) nebo víc.
>
>

Tento příklad přidá výstrahy pro získávání upozornění při spotřebě eDTU fondu elastické databáze překročí určitou mez.

```PowerShell
# Set up your resource ID configurations
$subscriptionId = '<Azure subscription id>'      # Azure subscription ID
$location =  '<location'                         # Azure region
$resourceGroupName = '<resource group name>'     # Resource Group
$serverName = '<server name>'                    # server name
$poolName = '<elastic pool name>'                # pool name

#$Target Resource ID
$ResourceID = '/subscriptions/' + $subscriptionId + '/resourceGroups/' +$resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/elasticpools/' + $poolName

# Create an email action
$actionEmail = New-AzureRmAlertRuleEmail -SendToServiceOwners -CustomEmail JohnDoe@contoso.com

# create a unique rule name
$alertName = $poolName + "- DTU consumption rule"

# Create an alert rule for DTU_consumption_percent
Add-AzureRMMetricAlertRule -Name $alertName -Location $location -ResourceGroup $resourceGroupName -TargetResourceId $ResourceID -MetricName "DTU_consumption_percent"  -Operator GreaterThan -Threshold 80 -TimeAggregationOperator Average -WindowSize 00:60:00 -Actions $actionEmail
```

Další informace najdete v tématu [vytvářet výstrahy, SQL Database na portálu Azure](sql-database-insights-alerts-portal.md).

## <a name="add-alerts-to-all-databases-in-an-elastic-pool"></a>Přidat výstrahy pro všechny databáze v elastickém fondu
Můžete přidat pravidla výstrah pro všechny databáze v elastickém fondu k odeslání e-mailových oznámení nebo výstrah řetězce [adresy URL koncových bodů](https://msdn.microsoft.com/library/mt718036.aspx) když prostředek dotkne prahovou hodnotu využití nastavit tak, že výstrahy.

> [!IMPORTANT]
> Monitorování pro elastické fondy využití prostředků má prodlení alespoň 5 minut. Nastavení výstrah menší než 10 minut pro elastické fondy není aktuálně podporován. Nastavit všechny výstrahy pro elastické fondy s dobou (parametr s názvem "-velikost_okna" v rozhraní API prostředí PowerShell) menší než 10 minut nemusí aktivovat. Ujistěte se, že všechny výstrahy definujete pro elastické fondy použít dobu 10 minut (WindowSize) nebo víc.
>

Tento příklad přidá výstrahu do všech databází v elastickém fondu pro získávání oznámeno, že databáze spotřeba DTU překročí určitou mez.

```PowerShell
# Set up your resource ID configurations
$subscriptionId = '<Azure subscription id>'      # Azure subscription ID
$location = '<location'                          # Azure region
$resourceGroupName = '<resource group name>'     # Resource Group
$serverName = '<server name>'                    # server name
$poolName = '<elastic pool name>'                # pool name

# Get the list of databases in this pool.
$dbList = Get-AzureRmSqlElasticPoolDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -ElasticPoolName $poolName

# Create an email action
$actionEmail = New-AzureRmAlertRuleEmail -SendToServiceOwners -CustomEmail JohnDoe@contoso.com

# Get resource usage metrics for a database in an elastic pool for the specified time interval.
foreach ($db in $dbList)
{
    $dbResourceId = '/subscriptions/' + $subscriptionId + '/resourceGroups/' + $resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/databases/' + $db.DatabaseName

    # create a unique rule name
    $alertName = $db.DatabaseName + "- DTU consumption rule"

    # Create an alert rule for DTU_consumption_percent
    Add-AzureRMMetricAlertRule -Name $alertName  -Location $location -ResourceGroup $resourceGroupName -TargetResourceId $dbResourceId -MetricName "dtu_consumption_percent"  -Operator GreaterThan -Threshold 80 -TimeAggregationOperator Average -WindowSize 00:60:00 -Actions $actionEmail

    # drop the alert rule
    #Remove-AzureRmAlertRule -ResourceGroup $resourceGroupName -Name $alertName
}
```

## <a name="collect-and-monitor-resource-usage-data-across-multiple-pools-in-a-subscription"></a>Shromažďování a sledovat data použití prostředků v rámci více fondů v předplatném.
Pokud máte mnoho databází v odběru, je náročná samostatně monitorování jednotlivých elastického fondu. Místo toho rutiny prostředí PowerShell databáze SQL a dotazů T-SQL můžete kombinovat shromažďovat data použití prostředků z více fondů a jejich databází pro sledování a analýza využití prostředků. A [Ukázka implementace](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools) sady prostředí PowerShell skripty lze nalézt v úložišti GitHub SQL Server – ukázky společně s dokumentací na co znamená a způsobu jeho použití.

Tato ukázka implementace, postupujte podle těchto kroků.

1. Stažení [skripty a dokumentaci](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools):
2. Změňte skripty pro vaše prostředí. Zadejte jeden nebo více serverů, na kterých jsou hostované elastické fondy.
3. Zadejte databázi telemetrie, kam mají být uloženy shromažďovat metriky.
4. Upravte skript tak, aby určete dobu trvání provádění tyto skripty.

Na vysoké úrovni skripty, postupujte takto:

* Zobrazí všechny servery v konkrétního předplatného Azure (nebo zadaný seznam serverů).
* Spustí úlohu na pozadí pro každý server. Úloha běží ve smyčce v pravidelných intervalech a shromažďuje telemetrická data pro všechny fondy na serveru. Potom načte shromážděná data do databáze zadaný telemetrie.
* Zobrazí seznam databází v každém fondu ke shromažďování dat o využití prostředků databáze. Potom načte shromážděná data do databáze telemetrie.

Shromažďovat metriky v databázi telemetrická data mohou být analyzovány tak, aby monitorovala elastické fondy a databází v ní. Skript se nainstaluje taky předem definovaná funkce hodnota tabulku (TVF) v databázi telemetrie pomohou agregace metriky pro zadané časové okno. Například funkci TVF výsledky lze zobrazit "top N elastické fondy s maximální počet jednotek eDTU využití v daném časovém období." Při dotazování a analýze shromážděných dat, použijte analytické nástroje, například aplikace Excel nebo produktu Power BI.

### <a name="example-retrieve-resource-consumption-metrics-for-an-elastic-pool-and-its-databases"></a>Příklad: načtení metriky spotřeby prostředků pro elastický fond a její databáze
Tento příklad načte spotřeba metriky pro daný elastický fond a všechny její databáze. Shromážděná data je naformátován a zapisují do formátovaného souboru .csv. Soubor můžete procházet pomocí aplikace Excel.

```PowerShell
$subscriptionId = '<Azure subscription id>'          # Azure subscription ID
$resourceGroupName = '<resource group name>'             # Resource Group
$serverName = <server name>                              # server name
$poolName = <elastic pool name>                          # pool name

# Login to Azure account and select the subscription.
Login-AzureRmAccount
Set-AzureRmContext -SubscriptionId $subscriptionId

# Get resource usage metrics for an elastic pool for the specified time interval.
$startTime = '4/27/2016 00:00:00'  # start time in UTC
$endTime = '4/27/2016 01:00:00'    # end time in UTC

# Construct the pool resource ID and retrive pool metrics at 5-minute granularity.
$poolResourceId = '/subscriptions/' + $subscriptionId + '/resourceGroups/' + $resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/elasticPools/' + $poolName
$poolMetrics = (Get-AzureRmMetric -ResourceId $poolResourceId -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime $startTime -EndTime $endTime)

# Get the list of databases in this pool.
$dbList = Get-AzureRmSqlElasticPoolDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -ElasticPoolName $poolName

# Get resource usage metrics for a database in an elastic pool for the specified time interval.
$dbMetrics = @()
foreach ($db in $dbList)
{
     $dbResourceId = '/subscriptions/' + $subscriptionId + '/resourceGroups/' + $resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/databases/' + $db.DatabaseName
     $dbMetrics = $dbMetrics + (Get-AzureRmMetric -ResourceId $dbResourceId -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime $startTime -EndTime $endTime)
}

#Optionally you can format the metrics and output as .csv file using the following script block.
$command = {
param($metricList, $outputFile)

# Format metrics into a table.
$table = @()
foreach($metric in $metricList) {
   foreach($metricValue in $metric.MetricValues) {
      $sx = New-Object PSObject -Property @{
      Timestamp = $metricValue.Timestamp.ToString()
      MetricName = $metric.Name;
      Average = $metricValue.Average;
      ResourceID = $metric.ResourceId
   }$table = $table += $sx
   }
}

# Output the metrics into a .csv file.
write-output $table | Export-csv -Path $outputFile -Append -NoTypeInformation
}

# Format and output pool metrics
Invoke-Command -ScriptBlock $command -ArgumentList $poolMetrics,c:\temp\poolmetrics.csv

# Format and output database metrics
Invoke-Command -ScriptBlock $command -ArgumentList $dbMetrics,c:\temp\dbmetrics.csv
```

## <a name="latency-of-elastic-pool-operations"></a>Latence operací elastického fondu
* Změna minimální počet jednotek Edtu na databázi nebo max Edtu na databázi obvykle dokončení během 5 minut nebo méně.
* Změna Edtu na fond, závisí na celkovém množství místo využité všechny databáze ve fondu. Změny průměrně trvají méně než 90 minut na každých 100 GB. Například pokud celkové místo používá všechny databáze ve fondu je 200 GB, pak očekávané latence pro změnu fondu eDTU na fond je 3 hodiny nebo méně.



## <a name="next-steps"></a>Další kroky
* [Vytvoření elastických úloh](sql-database-elastic-jobs-overview.md): Elastické úlohy umožňují spouštění skriptů T-SQL pro libovolný počet databází ve fondu.
* V tématu [horizontální navýšení kapacity s Azure SQL Database](sql-database-elastic-scale-introduction.md): pomocí nástrojů elastické škálování, přejděte dat, dotazů, nebo vytvořit transakce.
