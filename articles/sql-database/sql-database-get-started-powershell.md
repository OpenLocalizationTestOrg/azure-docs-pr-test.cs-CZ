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
# <a name="create-a-single-azure-sql-database-using-powershell"></a><span data-ttu-id="b0748-104">Vytvoření izolované databáze SQL Azure pomocí PowerShellu</span><span class="sxs-lookup"><span data-stu-id="b0748-104">Create a single Azure SQL database using PowerShell</span></span>

<span data-ttu-id="b0748-105">Prostředí PowerShell je použité toocreate a spravovat prostředky Azure z hello příkazového řádku nebo ve skriptech.</span><span class="sxs-lookup"><span data-stu-id="b0748-105">PowerShell is used toocreate and manage Azure resources from hello command line or in scripts.</span></span> <span data-ttu-id="b0748-106">Tato příručka podrobnosti o pomocí prostředí PowerShell toodeploy Azure SQL database v [skupina prostředků Azure](../azure-resource-manager/resource-group-overview.md) v [logického serveru Azure SQL Database](sql-database-features.md).</span><span class="sxs-lookup"><span data-stu-id="b0748-106">This guide details using PowerShell toodeploy an Azure SQL database in an [Azure resource group](../azure-resource-manager/resource-group-overview.md) in an [Azure SQL Database logical server](sql-database-features.md).</span></span>

<span data-ttu-id="b0748-107">Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.</span><span class="sxs-lookup"><span data-stu-id="b0748-107">If you don't have an Azure subscription, create a [free](https://azure.microsoft.com/free/) account before you begin.</span></span>

<span data-ttu-id="b0748-108">Tento kurz vyžaduje hello prostředí Azure PowerShell verze modulu 4.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="b0748-108">This tutorial requires hello Azure PowerShell module version 4.0 or later.</span></span> <span data-ttu-id="b0748-109">Spustit ` Get-Module -ListAvailable AzureRM` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="b0748-109">Run ` Get-Module -ListAvailable AzureRM` toofind hello version.</span></span> <span data-ttu-id="b0748-110">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [modul nainstalovat Azure PowerShell](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="b0748-110">If you need tooinstall or upgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span> 

## <a name="log-in-tooazure"></a><span data-ttu-id="b0748-111">Přihlaste se tooAzure</span><span class="sxs-lookup"><span data-stu-id="b0748-111">Log in tooAzure</span></span>

<span data-ttu-id="b0748-112">Přihlaste se tooyour předplatného Azure, pomocí hello [Add-AzureRmAccount](/powershell/module/azurerm.profile/add-azurermaccount) příkazů a postupujte podle hello na obrazovce pokynů.</span><span class="sxs-lookup"><span data-stu-id="b0748-112">Log in tooyour Azure subscription using hello [Add-AzureRmAccount](/powershell/module/azurerm.profile/add-azurermaccount) command and follow hello on-screen directions.</span></span>

```powershell
Add-AzureRmAccount
```

## <a name="create-variables"></a><span data-ttu-id="b0748-113">Vytvoření proměnných</span><span class="sxs-lookup"><span data-stu-id="b0748-113">Create variables</span></span>

<span data-ttu-id="b0748-114">Definování proměnné pro použití ve skriptech hello tento rychlý start.</span><span class="sxs-lookup"><span data-stu-id="b0748-114">Define variables for use in hello scripts in this quick start.</span></span>

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

## <a name="create-a-resource-group"></a><span data-ttu-id="b0748-115">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="b0748-115">Create a resource group</span></span>

<span data-ttu-id="b0748-116">Vytvoření [skupina prostředků Azure](../azure-resource-manager/resource-group-overview.md) pomocí hello [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) příkaz.</span><span class="sxs-lookup"><span data-stu-id="b0748-116">Create an [Azure resource group](../azure-resource-manager/resource-group-overview.md) using hello [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) command.</span></span> <span data-ttu-id="b0748-117">Skupina prostředků je logický kontejner, ve kterém se nasazují a spravují prostředky jako skupina.</span><span class="sxs-lookup"><span data-stu-id="b0748-117">A resource group is a logical container into which Azure resources are deployed and managed as a group.</span></span> <span data-ttu-id="b0748-118">Hello následující příklad vytvoří skupinu prostředků s názvem `myResourceGroup` v hello `westeurope` umístění.</span><span class="sxs-lookup"><span data-stu-id="b0748-118">hello following example creates a resource group named `myResourceGroup` in hello `westeurope` location.</span></span>

```powershell
New-AzureRmResourceGroup -Name $resourcegroupname -Location $location
```
## <a name="create-a-logical-server"></a><span data-ttu-id="b0748-119">Vytvoření logického serveru</span><span class="sxs-lookup"><span data-stu-id="b0748-119">Create a logical server</span></span>

<span data-ttu-id="b0748-120">Vytvoření [logického serveru Azure SQL Database](sql-database-features.md) pomocí hello [New-AzureRmSqlServer](/powershell/module/azurerm.sql/new-azurermsqlserver) příkaz.</span><span class="sxs-lookup"><span data-stu-id="b0748-120">Create an [Azure SQL Database logical server](sql-database-features.md) using hello [New-AzureRmSqlServer](/powershell/module/azurerm.sql/new-azurermsqlserver) command.</span></span> <span data-ttu-id="b0748-121">Logický server obsahuje soubor databází spravovaných jako skupina.</span><span class="sxs-lookup"><span data-stu-id="b0748-121">A logical server contains a group of databases managed as a group.</span></span> <span data-ttu-id="b0748-122">Hello následující příklad vytvoří náhodným názvem serveru ve vaší skupině prostředků se k přihlášení správce s názvem `ServerAdmin` a heslo `ChangeYourAdminPassword1`.</span><span class="sxs-lookup"><span data-stu-id="b0748-122">hello following example creates a randomly named server in your resource group with an admin login named `ServerAdmin` and a password of `ChangeYourAdminPassword1`.</span></span> <span data-ttu-id="b0748-123">Podle potřeby tyto předdefinované hodnoty nahraďte.</span><span class="sxs-lookup"><span data-stu-id="b0748-123">Replace these pre-defined values as desired.</span></span>

```powershell
New-AzureRmSqlServer -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -Location $location `
    -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $adminlogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))
```

## <a name="configure-a-server-firewall-rule"></a><span data-ttu-id="b0748-124">Konfigurace pravidla brány firewall serveru</span><span class="sxs-lookup"><span data-stu-id="b0748-124">Configure a server firewall rule</span></span>

<span data-ttu-id="b0748-125">Vytvoření [pravidlo brány firewall na úrovni serveru Azure SQL Database](sql-database-firewall-configure.md) pomocí hello [New-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule) příkaz.</span><span class="sxs-lookup"><span data-stu-id="b0748-125">Create an [Azure SQL Database server-level firewall rule](sql-database-firewall-configure.md) using hello [New-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule) command.</span></span> <span data-ttu-id="b0748-126">Pravidlo brány firewall na úrovni serveru umožňuje externí aplikací, jako je například SQL Server Management Studio nebo hello SQLCMD nástroj tooconnect tooa SQL database přes bránu firewall služby SQL Database hello.</span><span class="sxs-lookup"><span data-stu-id="b0748-126">A server-level firewall rule allows an external application, such as SQL Server Management Studio or hello SQLCMD utility tooconnect tooa SQL database through hello SQL Database service firewall.</span></span> <span data-ttu-id="b0748-127">V následujícím příkladu hello je otevřené brány firewall hello pouze ostatní prostředky služby Azure.</span><span class="sxs-lookup"><span data-stu-id="b0748-127">In hello following example, hello firewall is only opened for other Azure resources.</span></span> <span data-ttu-id="b0748-128">externí připojení tooenable změnu hello IP adres tooan příslušnou adresu pro vaše prostředí.</span><span class="sxs-lookup"><span data-stu-id="b0748-128">tooenable external connectivity, change hello IP address tooan appropriate address for your environment.</span></span> <span data-ttu-id="b0748-129">tooopen všechny IP adresy, použijte 0.0.0.0 jako hello počáteční IP adresa a 255.255.255.255 jako hello koncová adresa.</span><span class="sxs-lookup"><span data-stu-id="b0748-129">tooopen all IP addresses, use 0.0.0.0 as hello starting IP address and 255.255.255.255 as hello ending address.</span></span>

```powershell
New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -FirewallRuleName "AllowSome" -StartIpAddress $startip -EndIpAddress $endip
```

> [!NOTE]
> <span data-ttu-id="b0748-130">SQL Database komunikuje přes port 1433.</span><span class="sxs-lookup"><span data-stu-id="b0748-130">SQL Database communicates over port 1433.</span></span> <span data-ttu-id="b0748-131">Pokud se pokoušíte tooconnect z podnikové sítě, odchozí provoz přes port 1433 nemusí mít povolený bránou firewall vaší sítě.</span><span class="sxs-lookup"><span data-stu-id="b0748-131">If you are trying tooconnect from within a corporate network, outbound traffic over port 1433 may not be allowed by your network's firewall.</span></span> <span data-ttu-id="b0748-132">Pokud ano, nebudou se moct tooconnect serveru Azure SQL Database tooyour, dokud vaše IT oddělení otevře port 1433.</span><span class="sxs-lookup"><span data-stu-id="b0748-132">If so, you will not be able tooconnect tooyour Azure SQL Database server unless your IT department opens port 1433.</span></span>
>

## <a name="create-a-database-in-hello-server-with-sample-data"></a><span data-ttu-id="b0748-133">Vytvořit databázi na serveru hello s ukázkovými daty</span><span class="sxs-lookup"><span data-stu-id="b0748-133">Create a database in hello server with sample data</span></span>

<span data-ttu-id="b0748-134">Vytvořit databázi s [úroveň výkonu S0](sql-database-service-tiers.md) v hello serveru pomocí hello [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase) příkaz.</span><span class="sxs-lookup"><span data-stu-id="b0748-134">Create a database with an [S0 performance level](sql-database-service-tiers.md) in hello server using hello [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase) command.</span></span> <span data-ttu-id="b0748-135">Hello následující příklad vytvoří databázi názvem `mySampleDatabase` a zatížením hello AdventureWorksLT ukázková data do této databáze.</span><span class="sxs-lookup"><span data-stu-id="b0748-135">hello following example creates a database called `mySampleDatabase` and loads hello AdventureWorksLT sample data into this database.</span></span> <span data-ttu-id="b0748-136">Nahraďte tyto předdefinované hodnoty podle potřeby (jiné rychlé spuštění v této kolekci sestavení při hello hodnoty v této úvodní).</span><span class="sxs-lookup"><span data-stu-id="b0748-136">Replace these predefined values as desired (other quick starts in this collection build upon hello values in this quick start).</span></span>

```powershell
New-AzureRmSqlDatabase  -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -DatabaseName $databasename `
    -SampleName "AdventureWorksLT" `
    -RequestedServiceObjectiveName "S0"
```

## <a name="clean-up-resources"></a><span data-ttu-id="b0748-137">Vyčištění prostředků</span><span class="sxs-lookup"><span data-stu-id="b0748-137">Clean up resources</span></span>

<span data-ttu-id="b0748-138">Další rychlé starty v této kolekci jsou postavené na tomto rychlém startu.</span><span class="sxs-lookup"><span data-stu-id="b0748-138">Other quick starts in this collection build upon this quick start.</span></span> 

> [!TIP]
> <span data-ttu-id="b0748-139">Pokud máte v plánu toocontinue na toowork s několik rychlé spuštění, vyčistěte hello prostředky vytvořené v této rychlé nespouštějte.</span><span class="sxs-lookup"><span data-stu-id="b0748-139">If you plan toocontinue on toowork with subsequent quick starts, do not clean up hello resources created in this quick start.</span></span> <span data-ttu-id="b0748-140">Pokud neplánujete toocontinue, použijte následující kroky toodelete všechny prostředky vytvořeny pomocí této úvodní v hello portál Azure hello.</span><span class="sxs-lookup"><span data-stu-id="b0748-140">If you do not plan toocontinue, use hello following steps toodelete all resources created by this quick start in hello Azure portal.</span></span>
>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName $resourcegroupname
```

## <a name="next-steps"></a><span data-ttu-id="b0748-141">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b0748-141">Next steps</span></span>

<span data-ttu-id="b0748-142">Teď, když máte databázi, můžete se k ní připojit a provádět dotazování pomocí vašich oblíbených nástrojů.</span><span class="sxs-lookup"><span data-stu-id="b0748-142">Now that you have a database, you can connect and query using your favorite tools.</span></span> <span data-ttu-id="b0748-143">Další informace získáte výběrem vašeho nástroje níže:</span><span class="sxs-lookup"><span data-stu-id="b0748-143">Learn more by choosing your tool below:</span></span>

- [<span data-ttu-id="b0748-144">SQL Server Management Studio</span><span class="sxs-lookup"><span data-stu-id="b0748-144">SQL Server Management Studio</span></span>](sql-database-connect-query-ssms.md)
- [<span data-ttu-id="b0748-145">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b0748-145">Visual Studio Code</span></span>](sql-database-connect-query-vscode.md)
- [<span data-ttu-id="b0748-146">.NET</span><span class="sxs-lookup"><span data-stu-id="b0748-146">.NET</span></span>](sql-database-connect-query-dotnet.md)
- [<span data-ttu-id="b0748-147">PHP</span><span class="sxs-lookup"><span data-stu-id="b0748-147">PHP</span></span>](sql-database-connect-query-php.md)
- [<span data-ttu-id="b0748-148">Node.js</span><span class="sxs-lookup"><span data-stu-id="b0748-148">Node.js</span></span>](sql-database-connect-query-nodejs.md)
- [<span data-ttu-id="b0748-149">Java</span><span class="sxs-lookup"><span data-stu-id="b0748-149">Java</span></span>](sql-database-connect-query-java.md)
- [<span data-ttu-id="b0748-150">Python</span><span class="sxs-lookup"><span data-stu-id="b0748-150">Python</span></span>](sql-database-connect-query-python.md)
- [<span data-ttu-id="b0748-151">Ruby</span><span class="sxs-lookup"><span data-stu-id="b0748-151">Ruby</span></span>](sql-database-connect-query-ruby.md)

