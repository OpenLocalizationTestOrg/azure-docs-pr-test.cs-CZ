---
title: "Prostředí PowerShell: Vytvoření a správa fondu elastické databáze Azure SQL | Microsoft Docs"
description: "Zjistěte, jak toouse prostředí PowerShell toomanage fondu elastické databáze."
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
ms.openlocfilehash: 92de2a4b243dcc74502064e9d2c31682691753d5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-an-elastic-pool-with-powershell"></a>Vytvoření a správa fondu elastické databáze pomocí prostředí PowerShell
Toto téma ukazuje, jak toocreate a spravovat škálovatelné [elastické fondy](sql-database-elastic-pool.md) pomocí prostředí PowerShell.  Můžete také vytvořit a spravovat Azure elastickém fondu pomocí hello [portál Azure](https://portal.azure.com/), REST API nebo [C#](sql-database-elastic-pool-manage-csharp.md). Můžete také vytvořit a přesun databáze do a z elastické fondy pomocí [Transact-SQL](sql-database-elastic-pool-manage-tsql.md).

[!INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## <a name="create-an-elastic-pool"></a>Vytvoření elastického fondu
Hello [New-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/new-azurermsqlelasticpool) rutina vytvoří fondu elastické databáze. Hello hodnoty eDTU na fond a minimální a maximální Dtu jsou omezeny hodnota vrstvy služby hello (Basic, Standard, Premium nebo Premium RS). V tématu [omezení eDTU a úložiště pro elastické fondy a databází ve fondu](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools).

```PowerShell
New-AzureRmSqlElasticPool -ResourceGroupName "resourcegroup1" -ServerName "server1" -ElasticPoolName "elasticpool1" -Edition "Standard" -Dtu 400 -DatabaseDtuMin 10 -DatabaseDtuMax 100
```

## <a name="create-a-pooled-database-in-an-elastic-pool"></a>Vytvoření databáze ve fondu v elastickém fondu
Použití hello [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase) rutiny a sadu hello **ElasticPoolName** parametr toohello cílový fond. toomove existující databáze do pružného fondu, najdete v části [přesun databáze do pružného fondu](sql-database-elastic-pool-manage-powershell.md#move-a-database-into-an-elastic-pool).

```PowerShell
New-AzureRmSqlDatabase -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"
```

### <a name="complete-script"></a>Celý skript
Tento skript vytvoří skupinu prostředků Azure a serverem. Po zobrazení výzvy zadejte jméno a heslo správce pro nový server hello (nikoli vaše přihlašovací údaje Azure).

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
Vytváření mnoho databází v elastickém fondu může trvat dobu, pokud se provádí pomocí hello portálu nebo rutiny prostředí PowerShell, který současně vytvořit pouze jednu databázi. Vytvoření tooautomate do pružného fondu, najdete v části [CreateOrUpdateElasticPoolAndPopulate ](https://gist.github.com/billgib/d80c7687b17355d3c2ec8042323819ae).

## <a name="move-a-database-into-an-elastic-pool"></a>Přesun databáze do pružného fondu
Přesouváte databázi do nebo z fondu elastické databáze s hello [Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqlelasticpool).

```PowerShell
Set-AzureRmSqlDatabase -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"
```

## <a name="change-performance-settings-of-an-elastic-pool"></a>Změňte nastavení výkonu elastického fondu
Když ke snížení výkonu, můžete změnit nastavení hello hello fondu tooaccommodate růstu. Použití hello [Set-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/set-azurermsqlelasticpool) rutiny. Nastavit hello - Dtu parametr toohello Edtu na fond. V tématu [omezení eDTU a úložiště](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools) pro možné hodnoty.

```PowerShell
Set-AzureRmSqlElasticPool -ResourceGroupName “resourcegroup1” -ServerName “server1” -ElasticPoolName “elasticpool1” -Dtu 1200 -DatabaseDtuMax 100 -DatabaseDtuMin 50
```

## <a name="change-hello-storage-limit-for-an-elastic-pool"></a>Změnit hello limit úložiště fondu elastické databáze

Použití hello [Set-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/set-azurermsqlelasticpool) rutiny tooset hello _- StorageMB_ parametr. Zadejte hello limit úložiště v MB (například 2097152 nastaví hello úložiště limit too2 TB). V tématu [omezení eDTU a úložiště](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools) pro možné hodnoty.

> [!IMPORTANT]
> Hello výchozí maximální dat úložiště na každý fond pro fondy Premium s 1 500 Edtu nebo více je 750 GB. tooobtain hello vyšší _maximální velikost úložiště dat na každý fond_, musí být explicitně nastaveny hello limit úložiště. Premium fondy s více než 750 GB úložiště je aktuálně ve verzi public preview v hello následující oblasti: východní USA 2, západ USA, Virginia nám verze pro státní správu, západní Evropa, Německo centrální, jihovýchodní Asie, Japonsko – východ, Austrálie – východ, Kanada centrální a Východní Kanada.

```PowerShell
Set-AzureRmSqlElasticPool -ServerName "server1" -ElasticPoolName “elasticpool1” -StorageMB 2097152
```

## <a name="get-hello-status-of-pool-operations"></a>Získat stav hello operací fondu
Vytvoření fondu elastické databáze může trvat dobu. Stav hello tootrack operací fondu, včetně vytváření a aktualizace, použijte hello [Get-AzureRmSqlElasticPoolActivity](/powershell/module/azurerm.sql/get-azurermsqlelasticpoolactivity) rutiny.

```PowerShell
Get-AzureRmSqlElasticPoolActivity -ResourceGroupName “resourcegroup1” -ServerName “server1” -ElasticPoolName “elasticpool1”
```

## <a name="get-hello-status-of-moving-a-database-into-and-out-of-an-elastic-pool"></a>Získat stav hello přesun databáze do a z fondu elastické databáze
Přesunutí databáze může trvat dobu. Sledování stavu přesunutí pomocí hello [Get-AzureRmSqlDatabaseActivity](/powershell/module/azurerm.sql/get-azurermsqldatabaseactivity) rutiny.

```PowerShell
Get-AzureRmSqlDatabaseActivity -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"
```

## <a name="get-resource-usage-data-for-an-elastic-pool"></a>Získat data použití prostředků fondu elastické databáze
Metriky, které mohou být načteny jako procento hello limit fondu prostředků:

| Název metriky | Popis |
|:--- |:--- |
| Procesor\_procent |Průměr výpočetní využití v procentech hello limit hello fondu. |
| fyzické\_data\_číst\_procent |Průměrné využití vstupně-výstupních operací v procentech na hello limitu hello fondu. |
| protokol\_zápisu\_procent |Průměr zápisu využití prostředků v procentech hello limit hello fondu. |
| DTU\_spotřeba\_procent |Průměrná eDTU využití v procentech omezení eDTU pro fond hello |
| úložiště\_procent |Průměrné využití úložiště v procentech hello limit úložiště fondu hello. |
| pracovníci\_procent |Maximální souběžných pracovních procesů (počet požadavků) v procentech na hello limitu hello fondu. |
| relace\_procent |Maximální počet souběžných relací v procentech na hello limitu hello fondu. |
| eDTU_limit |Aktuální maximální elastický fond DTU nastavení pro tento elastický fond během tohoto intervalu. |
| úložiště\_limit |Aktuální maximální elastický fond úložiště limit nastavení pro toto elastického fondu v megabajtech během tohoto intervalu. |
| eDTU\_použít |Průměrná Edtu používané hello fondu v tomto intervalu. |
| úložiště\_použít |Průměrná úložiště používané hello fondu v tomto intervalu v bajtech |

**Metriky členitosti nebo období:**

* Data jsou vrácena v členitosti 5 minut.  
* Doba uchování dat je 35 dnů.  

Tato rutina a rozhraní API omezí hello počet řádků, které mohou být načteny v jedno volání too1000 řádků (o 3 dny dat ve členitosti 5 minut). Tento příkaz lze volat vícekrát s jinou počáteční nebo koncové časové intervaly tooretrieve více dat, ale

Metrika tooretrieve hello:

```PowerShell
$metrics = (Get-AzureRmMetric -ResourceId /subscriptions/<subscriptionId>/resourceGroups/FabrikamData01/providers/Microsoft.Sql/servers/fabrikamsqldb02/elasticPools/franchisepool -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime "4/18/2015" -EndTime "4/21/2015")
```

## <a name="get-resource-usage-data-for-a-database-in-an-elastic-pool"></a>Získat data použití prostředků pro databáze v elastickém fondu
Tato rozhraní API jsou hello stejná hodnota jako hello rozhraní API používaná pro sledování využití prostředků hello služby jedné databáze, s výjimkou hello následující sémantického rozdíl: metriky načíst jsou vyjádřené jako procentní podíl hello za Edtu max databáze (nebo ekvivalentní zakončení pro hello základní metrika jako procesoru nebo vstupně-výstupní operace) nastavte pro tento fond. Například 50 % využití každého tyto metriky označuje, že spotřeba konkrétní prostředků hello je na 50 % hello za limitu cap databáze pro tento prostředek ve fondu nadřazené hello.

Metrika tooretrieve hello:

```PowerShell
$metrics = (Get-AzureRmMetric -ResourceId /subscriptions/<subscriptionId>/resourceGroups/FabrikamData01/providers/Microsoft.Sql/servers/fabrikamsqldb02/databases/myDB -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime "4/18/2015" -EndTime "4/21/2015")
```

## <a name="add-an-alert-tooan-elastic-pool-resource"></a>Přidat prostředek výstrahy tooan elastického fondu
Můžete přidat pravidla výstrah tooan elastický fond toosend e-mailová oznámení nebo upozornit řetězce příliš[adresy URL koncových bodů](https://msdn.microsoft.com/library/mt718036.aspx) při elastického fondu hello dotkne prahovou hodnotu využití, které jste nastavili. Pomocí rutiny Add-AzureRmMetricAlertRule hello.

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

## <a name="add-alerts-tooall-databases-in-an-elastic-pool"></a>Přidat výstrahy tooall databází v elastickém fondu
Můžete přidat pravidla výstrah e-mailová oznámení tooall databáze v toosend elastického fondu nebo výstrahy řetězce příliš[adresy URL koncových bodů](https://msdn.microsoft.com/library/mt718036.aspx) když prostředek dotkne prahovou hodnotu využití nastavit tak, že výstraha hello.

> [!IMPORTANT]
> Monitorování pro elastické fondy využití prostředků má prodlení alespoň 5 minut. Nastavení výstrah menší než 10 minut pro elastické fondy není aktuálně podporován. Nastavit všechny výstrahy pro elastické fondy s dobou (parametr s názvem "-velikost_okna" v rozhraní API prostředí PowerShell) menší než 10 minut nemusí aktivovat. Ujistěte se, že všechny výstrahy definujete pro elastické fondy použít dobu 10 minut (WindowSize) nebo víc.
>

Tento příklad přidá výstrahy tooeach hello databází v elastickém fondu pro získávání oznámeno, že databáze spotřeba DTU překročí určitou mez.

```PowerShell
# Set up your resource ID configurations
$subscriptionId = '<Azure subscription id>'      # Azure subscription ID
$location = '<location'                          # Azure region
$resourceGroupName = '<resource group name>'     # Resource Group
$serverName = '<server name>'                    # server name
$poolName = '<elastic pool name>'                # pool name

# Get hello list of databases in this pool.
$dbList = Get-AzureRmSqlElasticPoolDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -ElasticPoolName $poolName

# Create an email action
$actionEmail = New-AzureRmAlertRuleEmail -SendToServiceOwners -CustomEmail JohnDoe@contoso.com

# Get resource usage metrics for a database in an elastic pool for hello specified time interval.
foreach ($db in $dbList)
{
    $dbResourceId = '/subscriptions/' + $subscriptionId + '/resourceGroups/' + $resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/databases/' + $db.DatabaseName

    # create a unique rule name
    $alertName = $db.DatabaseName + "- DTU consumption rule"

    # Create an alert rule for DTU_consumption_percent
    Add-AzureRMMetricAlertRule -Name $alertName  -Location $location -ResourceGroup $resourceGroupName -TargetResourceId $dbResourceId -MetricName "dtu_consumption_percent"  -Operator GreaterThan -Threshold 80 -TimeAggregationOperator Average -WindowSize 00:60:00 -Actions $actionEmail

    # drop hello alert rule
    #Remove-AzureRmAlertRule -ResourceGroup $resourceGroupName -Name $alertName
}
```

## <a name="collect-and-monitor-resource-usage-data-across-multiple-pools-in-a-subscription"></a>Shromažďování a sledovat data použití prostředků v rámci více fondů v předplatném.
Pokud máte mnoho databází v odběru, je náročná toomonitor každý elastického fondu samostatně. Místo toho rutiny prostředí PowerShell databáze SQL a dotazů T-SQL může být kombinované toocollect data použití prostředků z více fondů a jejich databází pro sledování a analýza využití prostředků. A [Ukázka implementace](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools) sady prostředí PowerShell skripty lze nalézt v hello Githubu SQL Server – ukázky úložiště společně s dokumentací na co dělá a jak toouse ho.

toouse Tato ukázka implementace, postupujte podle těchto kroků.

1. Stáhnout hello [skripty a dokumentaci](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools):
2. Upravte hello skripty pro vaše prostředí. Zadejte jeden nebo více serverů, na kterých jsou hostované elastické fondy.
3. Zadejte databázi telemetrie, kde hello shromažďovat metriky jsou toobe uložené.
4. Přizpůsobte hello skriptu toospecify hello trvání provádění hello skriptů.

Na vysoké úrovni skripty hello hello následující:

* Zobrazí všechny servery v konkrétního předplatného Azure (nebo zadaný seznam serverů).
* Spustí úlohu na pozadí pro každý server. Hello úloha běží ve smyčce v pravidelných intervalech a shromažďuje telemetrická data pro všechny fondy hello hello serveru. Potom načte hello shromažďovaných dat do databáze zadaný telemetrie hello.
* Zobrazí seznam databází v každém fondu toocollect hello databáze data použití prostředků. Potom načte hello shromažďovaných dat do databáze telemetrie hello.

Hello shromažďovat metriky v hello telemetrie databáze může být analyzovaných toomonitor hello stavu elastické fondy a hello databází v ní. skript Hello nainstaluje taky předem definovaná funkce hodnota tabulku (TVF) v hello telemetrie databáze toohelp agregační hello metriky pro zadané časové okno. Například může být výsledky hello TVF používá tooshow "hlavních elastické fondy s hello maximální počet jednotek eDTU využití v daném časovém období." Volitelně můžete použít analytické nástroje, například aplikace Excel nebo produktu Power BI tooquery a analyzovat hello shromažďovat data.

### <a name="example-retrieve-resource-consumption-metrics-for-an-elastic-pool-and-its-databases"></a>Příklad: načtení metriky spotřeby prostředků pro elastický fond a její databáze
Tento příklad načte hello spotřeba metriky pro daný elastický fond a všechny její databáze. Shromážděná data je naformátován a zapisovat formátovaného souboru .csv tooa. soubor Hello lze procházet pomocí aplikace Excel.

```PowerShell
$subscriptionId = '<Azure subscription id>'          # Azure subscription ID
$resourceGroupName = '<resource group name>'             # Resource Group
$serverName = <server name>                              # server name
$poolName = <elastic pool name>                          # pool name

# Login tooAzure account and select hello subscription.
Login-AzureRmAccount
Set-AzureRmContext -SubscriptionId $subscriptionId

# Get resource usage metrics for an elastic pool for hello specified time interval.
$startTime = '4/27/2016 00:00:00'  # start time in UTC
$endTime = '4/27/2016 01:00:00'    # end time in UTC

# Construct hello pool resource ID and retrive pool metrics at 5-minute granularity.
$poolResourceId = '/subscriptions/' + $subscriptionId + '/resourceGroups/' + $resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/elasticPools/' + $poolName
$poolMetrics = (Get-AzureRmMetric -ResourceId $poolResourceId -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime $startTime -EndTime $endTime)

# Get hello list of databases in this pool.
$dbList = Get-AzureRmSqlElasticPoolDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -ElasticPoolName $poolName

# Get resource usage metrics for a database in an elastic pool for hello specified time interval.
$dbMetrics = @()
foreach ($db in $dbList)
{
     $dbResourceId = '/subscriptions/' + $subscriptionId + '/resourceGroups/' + $resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/databases/' + $db.DatabaseName
     $dbMetrics = $dbMetrics + (Get-AzureRmMetric -ResourceId $dbResourceId -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime $startTime -EndTime $endTime)
}

#Optionally you can format hello metrics and output as .csv file using hello following script block.
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

# Output hello metrics into a .csv file.
write-output $table | Export-csv -Path $outputFile -Append -NoTypeInformation
}

# Format and output pool metrics
Invoke-Command -ScriptBlock $command -ArgumentList $poolMetrics,c:\temp\poolmetrics.csv

# Format and output database metrics
Invoke-Command -ScriptBlock $command -ArgumentList $dbMetrics,c:\temp\dbmetrics.csv
```

## <a name="latency-of-elastic-pool-operations"></a>Latence operací elastického fondu
* Změna hello minimální počet jednotek Edtu na databázi nebo max Edtu na databázi obvykle dokončení během 5 minut nebo méně.
* Změna hello Edtu na fond závisí na celkovém množství místo využité všechny databáze ve fondu hello hello. Změny průměrně trvají méně než 90 minut na každých 100 GB. Například pokud celkové místo hello používá všechny databáze ve fondu hello je 200 GB, pak hello očekává, že latence pro změnu eDTU fondu hello na fondu je 3 hodiny nebo méně.



## <a name="next-steps"></a>Další kroky
* [Vytvoření elastických úloh](sql-database-elastic-jobs-overview.md) elastické úlohy umožňují spouštění skriptů T-SQL pro libovolný počet databází ve fondu hello.
* Najdete v části [horizontální navýšení kapacity s Azure SQL Database](sql-database-elastic-scale-introduction.md): pomocí nástroje elastické tooscale out, přesun dat, dotazů, nebo vytvořit transakce.
