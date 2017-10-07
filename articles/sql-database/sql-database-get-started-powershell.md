---
title: "Azure PowerShell: Vytvoření databáze SQL | Dokumentace Microsoftu"
description: "Zjistěte, jak hello toocreate logického serveru SQL Database, pravidla brány firewall na úrovni serveru a databází v portálu Azure."
keywords: "kurz k sql database, vytvoření databáze sql"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,DBs & servers
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: hero-article
ms.date: 04/17/2017
ms.author: carlrab
ms.openlocfilehash: e89f68b44083a3b64e61f95117dbbedfa6647ccb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-single-azure-sql-database-using-powershell"></a>Vytvoření izolované databáze SQL Azure pomocí PowerShellu

Prostředí PowerShell je použité toocreate a spravovat prostředky Azure z hello příkazového řádku nebo ve skriptech. Tato příručka podrobnosti o pomocí prostředí PowerShell toodeploy Azure SQL database v [skupina prostředků Azure](../azure-resource-manager/resource-group-overview.md) v [logického serveru Azure SQL Database](sql-database-features.md).

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.

Tento kurz vyžaduje hello prostředí Azure PowerShell verze modulu 4.0 nebo novější. Spustit ` Get-Module -ListAvailable AzureRM` toofind hello verze. Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [modul nainstalovat Azure PowerShell](/powershell/azure/install-azurerm-ps). 

## <a name="log-in-tooazure"></a>Přihlaste se tooAzure

Přihlaste se tooyour předplatného Azure, pomocí hello [Add-AzureRmAccount](/powershell/module/azurerm.profile/add-azurermaccount) příkazů a postupujte podle hello na obrazovce pokynů.

```powershell
Add-AzureRmAccount
```

## <a name="create-variables"></a>Vytvoření proměnných

Definování proměnné pro použití ve skriptech hello tento rychlý start.

```powershell
# hello data center and resource name for your resources
$resourcegroupname = "myResourceGroup"
$location = "WestEurope"
# hello logical server name: Use a random value or replace with your own value (do not capitalize)
$servername = "server-$(Get-Random)"
# Set an admin login and password for your database
# hello login information for hello server
$adminlogin = "ServerAdmin"
$password = "ChangeYourAdminPassword1"
# hello ip address range that you want tooallow tooaccess your server - change as appropriate
$startip = "0.0.0.0"
$endip = "0.0.0.0"
# hello database name
$databasename = "mySampleDatabase"
```

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Vytvoření [skupina prostředků Azure](../azure-resource-manager/resource-group-overview.md) pomocí hello [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) příkaz. Skupina prostředků je logický kontejner, ve kterém se nasazují a spravují prostředky jako skupina. Hello následující příklad vytvoří skupinu prostředků s názvem `myResourceGroup` v hello `westeurope` umístění.

```powershell
New-AzureRmResourceGroup -Name $resourcegroupname -Location $location
```
## <a name="create-a-logical-server"></a>Vytvoření logického serveru

Vytvoření [logického serveru Azure SQL Database](sql-database-features.md) pomocí hello [New-AzureRmSqlServer](/powershell/module/azurerm.sql/new-azurermsqlserver) příkaz. Logický server obsahuje soubor databází spravovaných jako skupina. Hello následující příklad vytvoří náhodným názvem serveru ve vaší skupině prostředků se k přihlášení správce s názvem `ServerAdmin` a heslo `ChangeYourAdminPassword1`. Podle potřeby tyto předdefinované hodnoty nahraďte.

```powershell
New-AzureRmSqlServer -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -Location $location `
    -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $adminlogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))
```

## <a name="configure-a-server-firewall-rule"></a>Konfigurace pravidla brány firewall serveru

Vytvoření [pravidlo brány firewall na úrovni serveru Azure SQL Database](sql-database-firewall-configure.md) pomocí hello [New-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule) příkaz. Pravidlo brány firewall na úrovni serveru umožňuje externí aplikací, jako je například SQL Server Management Studio nebo hello SQLCMD nástroj tooconnect tooa SQL database přes bránu firewall služby SQL Database hello. V následujícím příkladu hello je otevřené brány firewall hello pouze ostatní prostředky služby Azure. externí připojení tooenable změnu hello IP adres tooan příslušnou adresu pro vaše prostředí. tooopen všechny IP adresy, použijte 0.0.0.0 jako hello počáteční IP adresa a 255.255.255.255 jako hello koncová adresa.

```powershell
New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -FirewallRuleName "AllowSome" -StartIpAddress $startip -EndIpAddress $endip
```

> [!NOTE]
> SQL Database komunikuje přes port 1433. Pokud se pokoušíte tooconnect z podnikové sítě, odchozí provoz přes port 1433 nemusí mít povolený bránou firewall vaší sítě. Pokud ano, nebudou se moct tooconnect serveru Azure SQL Database tooyour, dokud vaše IT oddělení otevře port 1433.
>

## <a name="create-a-database-in-hello-server-with-sample-data"></a>Vytvořit databázi na serveru hello s ukázkovými daty

Vytvořit databázi s [úroveň výkonu S0](sql-database-service-tiers.md) v hello serveru pomocí hello [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase) příkaz. Hello následující příklad vytvoří databázi názvem `mySampleDatabase` a zatížením hello AdventureWorksLT ukázková data do této databáze. Nahraďte tyto předdefinované hodnoty podle potřeby (jiné rychlé spuštění v této kolekci sestavení při hello hodnoty v této úvodní).

```powershell
New-AzureRmSqlDatabase  -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -DatabaseName $databasename `
    -SampleName "AdventureWorksLT" `
    -RequestedServiceObjectiveName "S0"
```

## <a name="clean-up-resources"></a>Vyčištění prostředků

Další rychlé starty v této kolekci jsou postavené na tomto rychlém startu. 

> [!TIP]
> Pokud máte v plánu toocontinue na toowork s několik rychlé spuštění, vyčistěte hello prostředky vytvořené v této rychlé nespouštějte. Pokud neplánujete toocontinue, použijte následující kroky toodelete všechny prostředky vytvořeny pomocí této úvodní v hello portál Azure hello.
>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName $resourcegroupname
```

## <a name="next-steps"></a>Další kroky

Teď, když máte databázi, můžete se k ní připojit a provádět dotazování pomocí vašich oblíbených nástrojů. Další informace získáte výběrem vašeho nástroje níže:

- [SQL Server Management Studio](sql-database-connect-query-ssms.md)
- [Visual Studio Code](sql-database-connect-query-vscode.md)
- [.NET](sql-database-connect-query-dotnet.md)
- [PHP](sql-database-connect-query-php.md)
- [Node.js](sql-database-connect-query-nodejs.md)
- [Java](sql-database-connect-query-java.md)
- [Python](sql-database-connect-query-python.md)
- [Ruby](sql-database-connect-query-ruby.md)

