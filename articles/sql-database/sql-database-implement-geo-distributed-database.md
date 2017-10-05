---
title: "Implementovat řešení zeměpisné polohy Azure SQL Database | Microsoft Docs"
description: "Další informace ke konfiguraci Azure SQL Database a aplikace pro převzetí služeb při selhání na replikované databáze a testovací převzetí služeb při selhání."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,business continuity
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 05/26/2017
ms.author: carlrab
ms.openlocfilehash: 9f53f318e20dac9248906bdbe898ba4dacb286ac
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="implement-a-geo-distributed-database"></a><span data-ttu-id="5f99d-103">Implementace geograficky distribuovaná databáze</span><span class="sxs-lookup"><span data-stu-id="5f99d-103">Implement a geo-distributed database</span></span>

<span data-ttu-id="5f99d-104">V tomto kurzu konfigurace Azure SQL database a aplikace pro převzetí služeb při selhání do vzdáleného oblasti a poté otestujte plánu převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="5f99d-104">In this tutorial, you configure an Azure SQL database and application for failover to a remote region, and then test your failover plan.</span></span> <span data-ttu-id="5f99d-105">Získáte informace o těchto tématech:</span><span class="sxs-lookup"><span data-stu-id="5f99d-105">You learn how to:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="5f99d-106">Vytvoření databáze uživatelů a udělit mu oprávnění</span><span class="sxs-lookup"><span data-stu-id="5f99d-106">Create database users and grant them permissions</span></span>
> * <span data-ttu-id="5f99d-107">Nastavit pravidlo brány firewall na úrovni databáze</span><span class="sxs-lookup"><span data-stu-id="5f99d-107">Set up a database-level firewall rule</span></span>
> * <span data-ttu-id="5f99d-108">Vytvoření [geografická replikace převzetí služeb při selhání skupiny](sql-database-geo-replication-overview.md)</span><span class="sxs-lookup"><span data-stu-id="5f99d-108">Create a [geo-replication failover group](sql-database-geo-replication-overview.md)</span></span>
> * <span data-ttu-id="5f99d-109">Vytvoření a kompilace aplikace Java k dotazování databáze Azure SQL</span><span class="sxs-lookup"><span data-stu-id="5f99d-109">Create and compile a Java application to query an Azure SQL database</span></span>
> * <span data-ttu-id="5f99d-110">Provedení postupu zotavení po havárii</span><span class="sxs-lookup"><span data-stu-id="5f99d-110">Perform a disaster recovery drill</span></span>

<span data-ttu-id="5f99d-111">Pokud nemáte předplatné Azure, [vytvořit bezplatný účet](https://azure.microsoft.com/free/) před zahájením.</span><span class="sxs-lookup"><span data-stu-id="5f99d-111">If you don't have an Azure subscription, [create a free account](https://azure.microsoft.com/free/) before you begin.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="5f99d-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5f99d-112">Prerequisites</span></span>

<span data-ttu-id="5f99d-113">Předpokladem dokončení tohoto kurzu je splnění následujících požadavků:</span><span class="sxs-lookup"><span data-stu-id="5f99d-113">To complete this tutorial, make sure the following prerequisites are completed:</span></span>

- <span data-ttu-id="5f99d-114">Nainstalován nejnovější [prostředí Azure PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs).</span><span class="sxs-lookup"><span data-stu-id="5f99d-114">Installed the latest [Azure PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs).</span></span> 
- <span data-ttu-id="5f99d-115">Nainstalovat Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="5f99d-115">Installed an Azure SQL database.</span></span> <span data-ttu-id="5f99d-116">Tento kurz používá ukázkové databáze AdventureWorksLT s názvem **mySampleDatabase** z jednoho z těchto rychlé spuštění:</span><span class="sxs-lookup"><span data-stu-id="5f99d-116">This tutorial uses the AdventureWorksLT sample database with a name of **mySampleDatabase** from one of these quick starts:</span></span>

   - [<span data-ttu-id="5f99d-117">Vytvoření databáze – portál</span><span class="sxs-lookup"><span data-stu-id="5f99d-117">Create DB - Portal</span></span>](sql-database-get-started-portal.md)
   - [<span data-ttu-id="5f99d-118">Vytvoření databáze – rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="5f99d-118">Create DB - CLI</span></span>](sql-database-get-started-cli.md)
   - [<span data-ttu-id="5f99d-119">Vytvoření databáze – PowerShell</span><span class="sxs-lookup"><span data-stu-id="5f99d-119">Create DB - PowerShell</span></span>](sql-database-get-started-powershell.md)

- <span data-ttu-id="5f99d-120">Našli metodu pro spuštění skriptů SQL na databázi, můžete použít jednu z následujících nástrojů dotazu:</span><span class="sxs-lookup"><span data-stu-id="5f99d-120">Have identified a method to execute SQL scripts against your database, you can use one of the following query tools:</span></span>
   - <span data-ttu-id="5f99d-121">V editoru dotazů [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="5f99d-121">The query editor in the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="5f99d-122">Další informace o používání editoru dotazů na portálu Azure najdete v tématu [připojit a zadávat dotazy pomocí editoru dotazů](sql-database-get-started-portal.md#query-the-sql-database).</span><span class="sxs-lookup"><span data-stu-id="5f99d-122">For more information on using the query editor in the Azure portal, see [Connect and query using Query Editor](sql-database-get-started-portal.md#query-the-sql-database).</span></span>
   - <span data-ttu-id="5f99d-123">Nejnovější verzi [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms), což je integrované prostředí pro správu jakékoliv infrastruktury, SQL, z SQL serveru do databáze SQL pro Microsoft Windows.</span><span class="sxs-lookup"><span data-stu-id="5f99d-123">The newest version of [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms), which is an integrated environment for managing any SQL infrastructure, from SQL Server to SQL Database for Microsoft Windows.</span></span>
   - <span data-ttu-id="5f99d-124">Nejnovější verzi [Visual Studio Code](https://code.visualstudio.com/docs), což je editor grafické kódu pro Linux, systému macOS, a systém Windows, který podporuje rozšíření, včetně [mssql rozšíření](https://aka.ms/mssql-marketplace) k dotazování systému Microsoft SQL Server, Azure SQL Database a SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="5f99d-124">The newest version of [Visual Studio Code](https://code.visualstudio.com/docs), which is a graphical code editor for Linux, macOS, and Windows that supports extensions, including the [mssql extension](https://aka.ms/mssql-marketplace) for querying Microsoft SQL Server, Azure SQL Database, and SQL Data Warehouse.</span></span> <span data-ttu-id="5f99d-125">Další informace o použití tohoto nástroje s Azure SQL Database, najdete v části [připojit a zadávat dotazy s VS Code](sql-database-connect-query-vscode.md).</span><span class="sxs-lookup"><span data-stu-id="5f99d-125">For more information on using this tool with Azure SQL Database, see [Connect and query with VS Code](sql-database-connect-query-vscode.md).</span></span> 

## <a name="create-database-users-and-grant-permissions"></a><span data-ttu-id="5f99d-126">Vytvoření databáze uživatelů a udělit oprávnění</span><span class="sxs-lookup"><span data-stu-id="5f99d-126">Create database users and grant permissions</span></span>

<span data-ttu-id="5f99d-127">Připojení k vaší databázi a vytvořte uživatelské účty pomocí jedné z následujících nástrojů dotazu:</span><span class="sxs-lookup"><span data-stu-id="5f99d-127">Connect to your database and create user accounts using one of the following query tools:</span></span>

- <span data-ttu-id="5f99d-128">Editor dotazů na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="5f99d-128">The Query editor in the Azure portal</span></span>
- <span data-ttu-id="5f99d-129">SQL Server Management Studio</span><span class="sxs-lookup"><span data-stu-id="5f99d-129">SQL Server Management Studio</span></span>
- <span data-ttu-id="5f99d-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5f99d-130">Visual Studio Code</span></span>

<span data-ttu-id="5f99d-131">Tyto uživatelské účty automaticky replikovat do sekundárního serveru (a sesynchronizovávat).</span><span class="sxs-lookup"><span data-stu-id="5f99d-131">These user accounts replicate automatically to your secondary server (and be kept in sync).</span></span> <span data-ttu-id="5f99d-132">Pokud chcete použít SQL Server Management Studio nebo Visual Studio Code, musíte nakonfigurovat pravidlo brány firewall, pokud se připojujete z klienta na adresu IP, pro kterou jste zatím nenakonfigurovali bránou firewall.</span><span class="sxs-lookup"><span data-stu-id="5f99d-132">To use SQL Server Management Studio or Visual Studio Code, you may need to configure a firewall rule if you are connecting from a client at an IP address for which you have not yet configured a firewall.</span></span> <span data-ttu-id="5f99d-133">Podrobné pokyny najdete v tématu [vytvoření pravidla brány firewall na úrovni serveru](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).</span><span class="sxs-lookup"><span data-stu-id="5f99d-133">For detailed steps, see [Create a server-level firewall rule](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).</span></span>

- <span data-ttu-id="5f99d-134">V okně dotazu spustíte následující dotaz, který vytvoříte dva uživatelské účty ve vaší databázi.</span><span class="sxs-lookup"><span data-stu-id="5f99d-134">In a query window, execute the following query to create two user accounts in your database.</span></span> <span data-ttu-id="5f99d-135">Tento skript uděluje **db_owner** oprávnění k **app_admin** účet a uděluje **vyberte** a **aktualizace** oprávnění k **app_user** účtu.</span><span class="sxs-lookup"><span data-stu-id="5f99d-135">This script grants **db_owner** permissions to the **app_admin** account and grants **SELECT** and **UPDATE** permissions to the **app_user** account.</span></span> 

   ```sql
   CREATE USER app_admin WITH PASSWORD = 'ChangeYourPassword1';
   --Add SQL user to db_owner role
   ALTER ROLE db_owner ADD MEMBER app_admin; 
   --Create additional SQL user
   CREATE USER app_user WITH PASSWORD = 'ChangeYourPassword1';
   --grant permission to SalesLT schema
   GRANT SELECT, INSERT, DELETE, UPDATE ON SalesLT.Product TO app_user;
   ```

## <a name="create-database-level-firewall"></a><span data-ttu-id="5f99d-136">Vytvoření brány firewall na úrovni databáze</span><span class="sxs-lookup"><span data-stu-id="5f99d-136">Create database-level firewall</span></span>

<span data-ttu-id="5f99d-137">Vytvoření [pravidlo brány firewall na úrovni databáze](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database) pro vaši databázi SQL.</span><span class="sxs-lookup"><span data-stu-id="5f99d-137">Create a [database-level firewall rule](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database) for your SQL database.</span></span> <span data-ttu-id="5f99d-138">Toto pravidlo brány firewall na úrovni databáze automaticky replikuje na sekundární server, který vytvoříte v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="5f99d-138">This database-level firewall rule replicates automatically to the secondary server that you create in this tutorial.</span></span> <span data-ttu-id="5f99d-139">Pro zjednodušení (v tomto kurzu) použijte veřejnou IP adresu počítače, na kterém provádíte kroky v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="5f99d-139">For simplicity (in this tutorial), use the public IP address of the computer on which you are performing the steps in this tutorial.</span></span> <span data-ttu-id="5f99d-140">Adresa IP použitá pro pravidlo brány firewall na úrovni serveru pro váš aktuální počítač, zjistíte v [vytvoření brány firewall na úrovni serveru](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).</span><span class="sxs-lookup"><span data-stu-id="5f99d-140">To determine the IP address used for the server-level firewall rule for your current computer, see [Create a server-level firewall](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).</span></span>  

- <span data-ttu-id="5f99d-141">V okně otevřeného dotazu nahraďte tento dotaz, nahraďte IP adresy na příslušné IP adresy pro vaše prostředí předchozího dotazu.</span><span class="sxs-lookup"><span data-stu-id="5f99d-141">In your open query window, replace the previous query with the following query, replacing the IP addresses with the appropriate IP addresses for your environment.</span></span>  

   ```sql
   -- Create database-level firewall setting for your public IP address
   EXECUTE sp_set_database_firewall_rule @name = N'myGeoReplicationFirewallRule',@start_ip_address = '0.0.0.0', @end_ip_address = '0.0.0.0';
   ```

## <a name="create-an-active-geo-replication-auto-failover-group"></a><span data-ttu-id="5f99d-142">Vytvořit skupinu aktivní geografickou replikací automatické převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="5f99d-142">Create an active geo-replication auto failover group</span></span> 

<span data-ttu-id="5f99d-143">Pomocí Azure PowerShell, vytvořte [aktivní geografickou replikací automatické převzetí služeb při selhání skupiny](sql-database-geo-replication-overview.md) mezi existující server Azure SQL a nový prázdný server Azure SQL v oblasti Azure a poté přidejte ukázkové databáze ke skupině převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="5f99d-143">Using Azure PowerShell, create an [active geo-replication auto failover group](sql-database-geo-replication-overview.md) between your existing Azure SQL server and the new empty Azure SQL server in an Azure region, and then add your sample database to the failover group.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5f99d-144">Tyto rutiny vyžadují Azure PowerShell 4.0.</span><span class="sxs-lookup"><span data-stu-id="5f99d-144">These cmdlets require Azure PowerShell 4.0.</span></span> [!INCLUDE [sample-powershell-install](../../includes/sample-powershell-install-no-ssh.md)]
>

1. <span data-ttu-id="5f99d-145">Naplnění proměnných pro skripty prostředí PowerShell pomocí hodnot pro existující server a ukázkovou databázi a zadejte globálně jedinečná hodnota pro název skupiny pro převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="5f99d-145">Populate variables for your PowerShell scripts using the values for your existing server and sample database, and provide a globally unique value for failover group name.</span></span>

   ```powershell
   $adminlogin = "ServerAdmin"
   $password = "ChangeYourAdminPassword1"
   $myresourcegroupname = "<your resource group name>"
   $mylocation = "<your resource group location>"
   $myservername = "<your existing server name>"
   $mydatabasename = "mySampleDatabase"
   $mydrlocation = "<your disaster recovery location>"
   $mydrservername = "<your disaster recovery server name>"
   $myfailovergroupname = "<your unique failover group name>"
   ```

2. <span data-ttu-id="5f99d-146">Vytvoření prázdného zálohování serveru ve vaší oblasti převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="5f99d-146">Create an empty backup server in your failover region.</span></span>

   ```powershell
   $mydrserver = New-AzureRmSqlServer -ResourceGroupName $myresourcegroupname `
      -ServerName $mydrservername `
      -Location $mydrlocation `
      -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $adminlogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))
   $mydrserver   
   ```

3. <span data-ttu-id="5f99d-147">Vytvořte skupinu převzetí služeb při selhání mezi dvěma servery.</span><span class="sxs-lookup"><span data-stu-id="5f99d-147">Create a failover group between the two servers.</span></span>

   ```powershell
   $myfailovergroup = New-AzureRMSqlDatabaseFailoverGroup `
      –ResourceGroupName $myresourcegroupname `
      -ServerName $myservername `
      -PartnerServerName $mydrservername  `
      –FailoverGroupName $myfailovergroupname `
      –FailoverPolicy Automatic `
      -GracePeriodWithDataLossHours 2
   $myfailovergroup   
   ```

4. <span data-ttu-id="5f99d-148">Přidejte databázi ke skupině převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="5f99d-148">Add your database to the failover group.</span></span>

   ```powershell
   $myfailovergroup = Get-AzureRmSqlDatabase `
      -ResourceGroupName $myresourcegroupname `
      -ServerName $myservername `
      -DatabaseName $mydatabasename | `
    Add-AzureRmSqlDatabaseToFailoverGroup `
      -ResourceGroupName $myresourcegroupname ` `
      -ServerName $myservername `
      -FailoverGroupName $myfailovergroupname
   $myfailovergroup   
   ```

## <a name="install-java-software"></a><span data-ttu-id="5f99d-149">Instalace softwaru Java</span><span class="sxs-lookup"><span data-stu-id="5f99d-149">Install Java software</span></span>

<span data-ttu-id="5f99d-150">Kroky v této části předpokládají, že máte zkušenosti s vývojem pomocí Javy a teprve začínáte pracovat se službou Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="5f99d-150">The steps in this section assume that you are familiar with developing using Java and are new to working with Azure SQL Database.</span></span> 

### <a name="mac-os"></a><span data-ttu-id="5f99d-151">**Mac OS**</span><span class="sxs-lookup"><span data-stu-id="5f99d-151">**Mac OS**</span></span>
<span data-ttu-id="5f99d-152">Otevřete terminál a přejděte do adresáře, kde plánujete vytvoření projektu v Javě.</span><span class="sxs-lookup"><span data-stu-id="5f99d-152">Open your terminal and navigate to a directory where you plan on creating your Java project.</span></span> <span data-ttu-id="5f99d-153">Zadáním následujících příkazů nainstalujte **brew** a **Maven**:</span><span class="sxs-lookup"><span data-stu-id="5f99d-153">Install **brew** and **Maven** by entering the following commands:</span></span> 

```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew update
brew install maven
```

<span data-ttu-id="5f99d-154">Obsahuje podrobné pokyny k instalaci a konfiguraci prostředí Java a Maven, přejděte [sestavení aplikace pomocí systému SQL Server](https://www.microsoft.com/sql-server/developer-get-started/), vyberte **Java**, vyberte **systému MacOS**a pak postupujte podle podrobné pokyny ke konfiguraci Java a Maven v krok 1.2 a 1.3.</span><span class="sxs-lookup"><span data-stu-id="5f99d-154">For detailed guidance on installing and configuring Java and Maven environment, go the [Build an app using SQL Server](https://www.microsoft.com/sql-server/developer-get-started/), select **Java**, select **MacOS**, and then follow the detailed instructions for configuring Java and Maven in step 1.2 and 1.3.</span></span>

### <a name="linux-ubuntu"></a><span data-ttu-id="5f99d-155">**Linux (Ubuntu)**</span><span class="sxs-lookup"><span data-stu-id="5f99d-155">**Linux (Ubuntu)**</span></span>
<span data-ttu-id="5f99d-156">Otevřete terminál a přejděte do adresáře, kde plánujete vytvoření projektu v Javě.</span><span class="sxs-lookup"><span data-stu-id="5f99d-156">Open your terminal and navigate to a directory where you plan on creating your Java project.</span></span> <span data-ttu-id="5f99d-157">Zadáním následujících příkazů nainstalujte **Maven**:</span><span class="sxs-lookup"><span data-stu-id="5f99d-157">Install **Maven** by entering the following commands:</span></span>

```bash
sudo apt-get install maven
```

<span data-ttu-id="5f99d-158">Obsahuje podrobné pokyny k instalaci a konfiguraci prostředí Java a Maven, přejděte [sestavení aplikace pomocí systému SQL Server](https://www.microsoft.com/sql-server/developer-get-started/), vyberte **Java**, vyberte **Ubuntu**a pak postupujte podle podrobné pokyny ke konfiguraci Java a Maven v kroku 1.4, 1.2 a 1.3.</span><span class="sxs-lookup"><span data-stu-id="5f99d-158">For detailed guidance on installing and configuring Java and Maven environment, go the [Build an app using SQL Server](https://www.microsoft.com/sql-server/developer-get-started/), select **Java**, select **Ubuntu**, and then follow the detailed instructions for configuring Java and Maven in step 1.2, 1.3, and 1.4.</span></span>

### <a name="windows"></a><span data-ttu-id="5f99d-159">**Windows**</span><span class="sxs-lookup"><span data-stu-id="5f99d-159">**Windows**</span></span>
<span data-ttu-id="5f99d-160">Nainstalujte [Maven](https://maven.apache.org/download.cgi) pomocí oficiální instalační služby.</span><span class="sxs-lookup"><span data-stu-id="5f99d-160">Install [Maven](https://maven.apache.org/download.cgi) using the official installer.</span></span> <span data-ttu-id="5f99d-161">Používání Maven k správě závislosti, sestavení, otestovat a spustit projekt Java.</span><span class="sxs-lookup"><span data-stu-id="5f99d-161">Use Maven to help manage dependencies, build, test, and run your Java project.</span></span> <span data-ttu-id="5f99d-162">Obsahuje podrobné pokyny k instalaci a konfiguraci prostředí Java a Maven, přejděte [sestavení aplikace pomocí systému SQL Server](https://www.microsoft.com/sql-server/developer-get-started/), vyberte **Java**, vyberte Windows a pak postupujte podle podrobné pokyny ke konfiguraci Java a Maven v krok 1.2 a 1.3.</span><span class="sxs-lookup"><span data-stu-id="5f99d-162">For detailed guidance on installing and configuring Java and Maven environment, go the [Build an app using SQL Server](https://www.microsoft.com/sql-server/developer-get-started/), select **Java**, select Windows, and then follow the detailed instructions for configuring Java and Maven in step 1.2 and 1.3.</span></span>

## <a name="create-sqldbsample-project"></a><span data-ttu-id="5f99d-163">Vytvoření projektu SqlDbSample</span><span class="sxs-lookup"><span data-stu-id="5f99d-163">Create SqlDbSample project</span></span>

1. <span data-ttu-id="5f99d-164">V konzole příkazu (například Bash) vytvořte projekt Maven.</span><span class="sxs-lookup"><span data-stu-id="5f99d-164">In the command console (such as Bash), create a Maven project.</span></span> 
   ```bash
   mvn archetype:generate "-DgroupId=com.sqldbsamples" "-DartifactId=SqlDbSample" "-DarchetypeArtifactId=maven-archetype-quickstart" "-Dversion=1.0.0"
   ```
2. <span data-ttu-id="5f99d-165">Typ **Y** a klikněte na tlačítko **Enter**.</span><span class="sxs-lookup"><span data-stu-id="5f99d-165">Type **Y** and click **Enter**.</span></span>
3. <span data-ttu-id="5f99d-166">Změňte adresáře na nově vytvořený projekt.</span><span class="sxs-lookup"><span data-stu-id="5f99d-166">Change directories into your newly created project.</span></span>

   ```bash
   cd SqlDbSamples
   ```

4. <span data-ttu-id="5f99d-167">Ve složce projektu pomocí vašeho oblíbeného editoru otevřete soubor pom.xml.</span><span class="sxs-lookup"><span data-stu-id="5f99d-167">Using your favorite editor, open the pom.xml file in your project folder.</span></span> 

5. <span data-ttu-id="5f99d-168">Přidáte ovladač JDBC Microsoft pro systém SQL Server závislost na projekt Maven otevřením svém oblíbeném textovém editoru a kopírování a vkládání následující řádky do souboru pom.xml.</span><span class="sxs-lookup"><span data-stu-id="5f99d-168">Add the Microsoft JDBC Driver for SQL Server dependency to your Maven project by opening your favorite text editor and copying and pasting the following lines into your pom.xml file.</span></span> <span data-ttu-id="5f99d-169">Nepřepisovat existující hodnoty naplněnými v souboru.</span><span class="sxs-lookup"><span data-stu-id="5f99d-169">Do not overwrite the existing values prepopulated in the file.</span></span> <span data-ttu-id="5f99d-170">Závislost JDBC musí vložení v rámci větší (část) pro "závislosti".</span><span class="sxs-lookup"><span data-stu-id="5f99d-170">The JDBC dependency must be pasted within the larger “dependencies” section ( ).</span></span>   

   ```xml
   <dependency>
     <groupId>com.microsoft.sqlserver</groupId>
     <artifactId>mssql-jdbc</artifactId>
    <version>6.1.0.jre8</version>
   </dependency>
   ```

6. <span data-ttu-id="5f99d-171">Zadejte verzi jazyka Java kompilace projektu před přidáním v následující části "vlastnosti" do souboru pom.xml po v části "závislosti".</span><span class="sxs-lookup"><span data-stu-id="5f99d-171">Specify the version of Java to compile the project against by adding the following “properties” section into the pom.xml file after the "dependencies" section.</span></span> 

   ```xml
   <properties>
     <maven.compiler.source>1.8</maven.compiler.source>
     <maven.compiler.target>1.8</maven.compiler.target>
   </properties>
   ```
7. <span data-ttu-id="5f99d-172">Přidejte následující části "vytvoření" po v části "vlastnosti" pro podporu manifestu souborů v JAR do souboru pom.xml.</span><span class="sxs-lookup"><span data-stu-id="5f99d-172">Add the following "build" section into the pom.xml file after the "properties" section to support manifest files in jars.</span></span>       

   ```xml
   <build>
     <plugins>
       <plugin>
         <groupId>org.apache.maven.plugins</groupId>
         <artifactId>maven-jar-plugin</artifactId>
         <version>3.0.0</version>
         <configuration>
           <archive>
             <manifest>
               <mainClass>com.sqldbsamples.App</mainClass>
             </manifest>
           </archive>
        </configuration>
       </plugin>
     </plugins>
   </build>
   ```
8. <span data-ttu-id="5f99d-173">Soubor pom.xml uložte a zavřete.</span><span class="sxs-lookup"><span data-stu-id="5f99d-173">Save and close the pom.xml file.</span></span>
9. <span data-ttu-id="5f99d-174">Otevřete soubor App.java (C:\apache-maven-3.5.0\SqlDbSample\src\main\java\com\sqldbsamples\App.java) a nahraďte jeho obsah s tímto obsahem.</span><span class="sxs-lookup"><span data-stu-id="5f99d-174">Open the App.java file (C:\apache-maven-3.5.0\SqlDbSample\src\main\java\com\sqldbsamples\App.java) and replace the contents with the following contents.</span></span> <span data-ttu-id="5f99d-175">Název skupiny pro převzetí služeb při selhání nahraďte název pro skupinu pro převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="5f99d-175">Replace the failover group name with the name for your failover group.</span></span> <span data-ttu-id="5f99d-176">Pokud jste změnili hodnoty pro název databáze, uživatele nebo heslo, změna také tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="5f99d-176">If you have changed the values for the database name, user, or password, change those values as well.</span></span>

   ```java
   package com.sqldbsamples;

   import java.sql.Connection;
   import java.sql.Statement;
   import java.sql.PreparedStatement;
   import java.sql.ResultSet;
   import java.sql.Timestamp;
   import java.sql.DriverManager;
   import java.util.Date;
   import java.util.concurrent.TimeUnit;

   public class App {

      private static final String FAILOVER_GROUP_NAME = "myfailovergroupname";
  
      private static final String DB_NAME = "mySampleDatabase";
      private static final String USER = "app_user";
      private static final String PASSWORD = "ChangeYourPassword1";

      private static final String READ_WRITE_URL = String.format("jdbc:sqlserver://%s.database.windows.net:1433;database=%s;user=%s;password=%s;encrypt=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;", FAILOVER_GROUP_NAME, DB_NAME, USER, PASSWORD);
      private static final String READ_ONLY_URL = String.format("jdbc:sqlserver://%s.secondary.database.windows.net:1433;database=%s;user=%s;password=%s;encrypt=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;", FAILOVER_GROUP_NAME, DB_NAME, USER, PASSWORD);

      public static void main(String[] args) {
         System.out.println("#######################################");
         System.out.println("## GEO DISTRIBUTED DATABASE TUTORIAL ##");
         System.out.println("#######################################");
         System.out.println(""); 

         int highWaterMark = getHighWaterMarkId();

         try {
            for(int i = 1; i < 1000; i++) {
                //  loop will run for about 1 hour
                System.out.print(i + ": insert on primary " + (insertData((highWaterMark + i))?"successful":"failed"));
                TimeUnit.SECONDS.sleep(1);
                System.out.print(", read from secondary " + (selectData((highWaterMark + i))?"successful":"failed") + "\n");
                TimeUnit.SECONDS.sleep(3);
            }
         } catch(Exception e) {
            e.printStackTrace();
      }
   }

   private static boolean insertData(int id) {
      // Insert data into the product table with a unique product name that we can use to find the product again later
      String sql = "INSERT INTO SalesLT.Product (Name, ProductNumber, Color, StandardCost, ListPrice, SellStartDate) VALUES (?,?,?,?,?,?);";

      try (Connection connection = DriverManager.getConnection(READ_WRITE_URL); 
              PreparedStatement pstmt = connection.prepareStatement(sql)) {
         pstmt.setString(1, "BrandNewProduct" + id);
         pstmt.setInt(2, 200989 + id + 10000);
         pstmt.setString(3, "Blue");
         pstmt.setDouble(4, 75.00);
         pstmt.setDouble(5, 89.99);
         pstmt.setTimestamp(6, new Timestamp(new Date().getTime()));
         return (1 == pstmt.executeUpdate());
      } catch (Exception e) {
         return false;
      }
   }

   private static boolean selectData(int id) {
      // Query the data that was previously inserted into the primary database from the geo replicated database
      String sql = "SELECT Name, Color, ListPrice FROM SalesLT.Product WHERE Name = ?";

      try (Connection connection = DriverManager.getConnection(READ_ONLY_URL); 
              PreparedStatement pstmt = connection.prepareStatement(sql)) {
         pstmt.setString(1, "BrandNewProduct" + id);
         try (ResultSet resultSet = pstmt.executeQuery()) {
            return resultSet.next();
         }
      } catch (Exception e) {
         return false;
      }
   }

   private static int getHighWaterMarkId() {
      // Query the high water mark id that is stored in the table to be able to make unique inserts 
      String sql = "SELECT MAX(ProductId) FROM SalesLT.Product";
      int result = 1;
        
      try (Connection connection = DriverManager.getConnection(READ_WRITE_URL); 
              Statement stmt = connection.createStatement();
              ResultSet resultSet = stmt.executeQuery(sql)) {
         if (resultSet.next()) {
             result =  resultSet.getInt(1);
            }
         } catch (Exception e) {
          e.printStackTrace();
         }
         return result;
      }
   }
   ```
6. <span data-ttu-id="5f99d-177">Soubor App.java uložte a zavřete.</span><span class="sxs-lookup"><span data-stu-id="5f99d-177">Save and close the App.java file.</span></span>

## <a name="compile-and-run-the-sqldbsample-project"></a><span data-ttu-id="5f99d-178">Zkompilování a spuštění projektu SqlDbSample</span><span class="sxs-lookup"><span data-stu-id="5f99d-178">Compile and run the SqlDbSample project</span></span>

1. <span data-ttu-id="5f99d-179">V konzole pro příkaz spusťte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="5f99d-179">In the command console, execute to following command.</span></span>

   ```bash
   mvn package
   ```
2. <span data-ttu-id="5f99d-180">Po dokončení, spusťte následující příkaz ke spuštění aplikace (spuštění o jednu hodinu Pokud zastavíte ručně):</span><span class="sxs-lookup"><span data-stu-id="5f99d-180">When finished, execute the following command to run the application (it runs for about 1 hour unless you stop it manually):</span></span>

   ```bash
   mvn -q -e exec:java "-Dexec.mainClass=com.sqldbsamples.App"
   
   #######################################
   ## GEO DISTRIBUTED DATABASE TUTORIAL ##
   #######################################

   1. insert on primary successful, read from secondary successful
   2. insert on primary successful, read from secondary successful
   3. insert on primary successful, read from secondary successful
   ```

## <a name="perform-disaster-recovery-drill"></a><span data-ttu-id="5f99d-181">Provedení postupu zotavení po havárii</span><span class="sxs-lookup"><span data-stu-id="5f99d-181">Perform disaster recovery drill</span></span>

1. <span data-ttu-id="5f99d-182">Volání ruční převzetí služeb při selhání skupiny převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="5f99d-182">Call manual failover of failover group.</span></span> 

   ```powershell
   Switch-AzureRMSqlDatabaseFailoverGroup `
   -ResourceGroupName $myresourcegroupname  `
   -ServerName $mydrservername `
   -FailoverGroupName $myfailovergroupname
   ```

2. <span data-ttu-id="5f99d-183">Pozorovat výsledky aplikace během převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="5f99d-183">Observe the application results during failover.</span></span> <span data-ttu-id="5f99d-184">Některé vloží nezdaří, při obnovení mezipaměti DNS.</span><span class="sxs-lookup"><span data-stu-id="5f99d-184">Some inserts fail while the DNS cache refreshes.</span></span>     

3. <span data-ttu-id="5f99d-185">Zjistěte, jaké role provádí vašeho serveru pro obnovení po havárii.</span><span class="sxs-lookup"><span data-stu-id="5f99d-185">Find out which role your disaster recovery server is performing.</span></span>

   ```powershell
   $mydrserver.ReplicationRole
   ```

4. <span data-ttu-id="5f99d-186">Navrácení služeb po obnovení.</span><span class="sxs-lookup"><span data-stu-id="5f99d-186">Failback.</span></span>

   ```powershell
   Switch-AzureRMSqlDatabaseFailoverGroup `
   -ResourceGroupName $myresourcegroupname  `
   -ServerName $myservername `
   -FailoverGroupName $myfailovergroupname
   ```

5. <span data-ttu-id="5f99d-187">Pozorovat výsledky aplikace během navrácení služeb po obnovení.</span><span class="sxs-lookup"><span data-stu-id="5f99d-187">Observe the application results during failback.</span></span> <span data-ttu-id="5f99d-188">Některé vloží nezdaří, při obnovení mezipaměti DNS.</span><span class="sxs-lookup"><span data-stu-id="5f99d-188">Some inserts fail while the DNS cache refreshes.</span></span>     

6. <span data-ttu-id="5f99d-189">Zjistěte, jaké role provádí vašeho serveru pro obnovení po havárii.</span><span class="sxs-lookup"><span data-stu-id="5f99d-189">Find out which role your disaster recovery server is performing.</span></span>

   ```powershell
   $fileovergroup = Get-AzureRMSqlDatabaseFailoverGroup `
      -FailoverGroupName $myfailovergroupname `
      -ResourceGroupName $myresourcegroupname `
      -ServerName $mydrservername
   $fileovergroup.ReplicationRole
   ```
## <a name="next-steps"></a><span data-ttu-id="5f99d-190">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5f99d-190">Next steps</span></span> 

<span data-ttu-id="5f99d-191">Další informace najdete v tématu [aktivní geografickou replikaci a převzetí služeb při selhání skupiny](sql-database-geo-replication-overview.md).</span><span class="sxs-lookup"><span data-stu-id="5f99d-191">For more information, see [Active geo-replication and failover groups](sql-database-geo-replication-overview.md).</span></span>
