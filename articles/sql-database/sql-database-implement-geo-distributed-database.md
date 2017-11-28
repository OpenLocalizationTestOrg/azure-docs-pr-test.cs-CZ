---
title: "aaaImplement zeměpisné polohy řešení Azure SQL Database | Microsoft Docs"
description: "Přečtěte si tooconfigure Azure SQL Database a aplikace pro převzetí služeb při selhání tooa replikované databáze a testovací převzetí služeb při selhání."
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
ms.openlocfilehash: 9295d33c669405108a1a64ef1e7cb77f582773a1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="implement-a-geo-distributed-database"></a><span data-ttu-id="d6942-103">Implementace geograficky distribuovaná databáze</span><span class="sxs-lookup"><span data-stu-id="d6942-103">Implement a geo-distributed database</span></span>

<span data-ttu-id="d6942-104">V tomto kurzu konfigurace Azure SQL database a aplikace pro vzdálené oblast tooa převzetí služeb při selhání a proveďte test převzetí služeb při selhání plánu.</span><span class="sxs-lookup"><span data-stu-id="d6942-104">In this tutorial, you configure an Azure SQL database and application for failover tooa remote region, and then test your failover plan.</span></span> <span data-ttu-id="d6942-105">Získáte informace o těchto tématech:</span><span class="sxs-lookup"><span data-stu-id="d6942-105">You learn how to:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="d6942-106">Vytvoření databáze uživatelů a udělit mu oprávnění</span><span class="sxs-lookup"><span data-stu-id="d6942-106">Create database users and grant them permissions</span></span>
> * <span data-ttu-id="d6942-107">Nastavit pravidlo brány firewall na úrovni databáze</span><span class="sxs-lookup"><span data-stu-id="d6942-107">Set up a database-level firewall rule</span></span>
> * <span data-ttu-id="d6942-108">Vytvoření [geografická replikace převzetí služeb při selhání skupiny](sql-database-geo-replication-overview.md)</span><span class="sxs-lookup"><span data-stu-id="d6942-108">Create a [geo-replication failover group](sql-database-geo-replication-overview.md)</span></span>
> * <span data-ttu-id="d6942-109">Vytvoření a kompilace tooquery aplikace Java Azure SQL database</span><span class="sxs-lookup"><span data-stu-id="d6942-109">Create and compile a Java application tooquery an Azure SQL database</span></span>
> * <span data-ttu-id="d6942-110">Provedení postupu zotavení po havárii</span><span class="sxs-lookup"><span data-stu-id="d6942-110">Perform a disaster recovery drill</span></span>

<span data-ttu-id="d6942-111">Pokud nemáte předplatné Azure, [vytvořit bezplatný účet](https://azure.microsoft.com/free/) před zahájením.</span><span class="sxs-lookup"><span data-stu-id="d6942-111">If you don't have an Azure subscription, [create a free account](https://azure.microsoft.com/free/) before you begin.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="d6942-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d6942-112">Prerequisites</span></span>

<span data-ttu-id="d6942-113">toocomplete dokončení tohoto kurzu, ujistěte se, hello následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="d6942-113">toocomplete this tutorial, make sure hello following prerequisites are completed:</span></span>

- <span data-ttu-id="d6942-114">Nejnovější nainstalované hello [prostředí Azure PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs).</span><span class="sxs-lookup"><span data-stu-id="d6942-114">Installed hello latest [Azure PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs).</span></span> 
- <span data-ttu-id="d6942-115">Nainstalovat Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="d6942-115">Installed an Azure SQL database.</span></span> <span data-ttu-id="d6942-116">Tento kurz používá ukázkové databáze AdventureWorksLT hello s názvem **mySampleDatabase** z jednoho z těchto rychlé spuštění:</span><span class="sxs-lookup"><span data-stu-id="d6942-116">This tutorial uses hello AdventureWorksLT sample database with a name of **mySampleDatabase** from one of these quick starts:</span></span>

   - [<span data-ttu-id="d6942-117">Vytvoření databáze – portál</span><span class="sxs-lookup"><span data-stu-id="d6942-117">Create DB - Portal</span></span>](sql-database-get-started-portal.md)
   - [<span data-ttu-id="d6942-118">Vytvoření databáze – rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="d6942-118">Create DB - CLI</span></span>](sql-database-get-started-cli.md)
   - [<span data-ttu-id="d6942-119">Vytvoření databáze – PowerShell</span><span class="sxs-lookup"><span data-stu-id="d6942-119">Create DB - PowerShell</span></span>](sql-database-get-started-powershell.md)

- <span data-ttu-id="d6942-120">Našli metoda tooexecute SQL skripty proti databázi, můžete použít jednu z následujících nástrojů dotazů hello:</span><span class="sxs-lookup"><span data-stu-id="d6942-120">Have identified a method tooexecute SQL scripts against your database, you can use one of hello following query tools:</span></span>
   - <span data-ttu-id="d6942-121">editor dotazů Hello v hello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="d6942-121">hello query editor in hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="d6942-122">Další informace o použití editoru dotazů hello v hello portálu Azure najdete v tématu [připojit a zadávat dotazy pomocí editoru dotazů](sql-database-get-started-portal.md#query-the-sql-database).</span><span class="sxs-lookup"><span data-stu-id="d6942-122">For more information on using hello query editor in hello Azure portal, see [Connect and query using Query Editor](sql-database-get-started-portal.md#query-the-sql-database).</span></span>
   - <span data-ttu-id="d6942-123">nejnovější verze Hello [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms), což je integrované prostředí pro správu jakékoliv infrastruktury, SQL, z tooSQL systému SQL Server databáze pro Microsoft Windows.</span><span class="sxs-lookup"><span data-stu-id="d6942-123">hello newest version of [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms), which is an integrated environment for managing any SQL infrastructure, from SQL Server tooSQL Database for Microsoft Windows.</span></span>
   - <span data-ttu-id="d6942-124">nejnovější verze Hello [Visual Studio Code](https://code.visualstudio.com/docs), což je editor grafické kódu pro Linux, systému macOS, a Windows, které podporuje rozšíření, včetně hello [mssql rozšíření](https://aka.ms/mssql-marketplace) k dotazování systému Microsoft SQL Server , Azure SQL Database a SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="d6942-124">hello newest version of [Visual Studio Code](https://code.visualstudio.com/docs), which is a graphical code editor for Linux, macOS, and Windows that supports extensions, including hello [mssql extension](https://aka.ms/mssql-marketplace) for querying Microsoft SQL Server, Azure SQL Database, and SQL Data Warehouse.</span></span> <span data-ttu-id="d6942-125">Další informace o použití tohoto nástroje s Azure SQL Database, najdete v části [připojit a zadávat dotazy s VS Code](sql-database-connect-query-vscode.md).</span><span class="sxs-lookup"><span data-stu-id="d6942-125">For more information on using this tool with Azure SQL Database, see [Connect and query with VS Code](sql-database-connect-query-vscode.md).</span></span> 

## <a name="create-database-users-and-grant-permissions"></a><span data-ttu-id="d6942-126">Vytvoření databáze uživatelů a udělit oprávnění</span><span class="sxs-lookup"><span data-stu-id="d6942-126">Create database users and grant permissions</span></span>

<span data-ttu-id="d6942-127">Připojit tooyour databáze a vytvořit uživatelské účty pomocí jedné z následujících nástrojů dotazů hello:</span><span class="sxs-lookup"><span data-stu-id="d6942-127">Connect tooyour database and create user accounts using one of hello following query tools:</span></span>

- <span data-ttu-id="d6942-128">editor dotazů Hello v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="d6942-128">hello Query editor in hello Azure portal</span></span>
- <span data-ttu-id="d6942-129">SQL Server Management Studio</span><span class="sxs-lookup"><span data-stu-id="d6942-129">SQL Server Management Studio</span></span>
- <span data-ttu-id="d6942-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d6942-130">Visual Studio Code</span></span>

<span data-ttu-id="d6942-131">Tyto uživatelské účty automaticky replikovat tooyour sekundární server (a sesynchronizovávat).</span><span class="sxs-lookup"><span data-stu-id="d6942-131">These user accounts replicate automatically tooyour secondary server (and be kept in sync).</span></span> <span data-ttu-id="d6942-132">toouse SQL Server Management Studio nebo Visual Studio Code, může být nutné tooconfigure pravidlo brány firewall při připojení z klienta na adresu IP, pro kterou jste zatím nenakonfigurovali bránou firewall.</span><span class="sxs-lookup"><span data-stu-id="d6942-132">toouse SQL Server Management Studio or Visual Studio Code, you may need tooconfigure a firewall rule if you are connecting from a client at an IP address for which you have not yet configured a firewall.</span></span> <span data-ttu-id="d6942-133">Podrobné pokyny najdete v tématu [vytvoření pravidla brány firewall na úrovni serveru](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).</span><span class="sxs-lookup"><span data-stu-id="d6942-133">For detailed steps, see [Create a server-level firewall rule](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).</span></span>

- <span data-ttu-id="d6942-134">V okně dotazu spusťte následující dotaz toocreate dva uživatelské účty ve vaší databázi hello.</span><span class="sxs-lookup"><span data-stu-id="d6942-134">In a query window, execute hello following query toocreate two user accounts in your database.</span></span> <span data-ttu-id="d6942-135">Tento skript uděluje **db_owner** oprávnění toohello **app_admin** účet a uděluje **vyberte** a **aktualizace** toohello oprávnění **app_user** účtu.</span><span class="sxs-lookup"><span data-stu-id="d6942-135">This script grants **db_owner** permissions toohello **app_admin** account and grants **SELECT** and **UPDATE** permissions toohello **app_user** account.</span></span> 

   ```sql
   CREATE USER app_admin WITH PASSWORD = 'ChangeYourPassword1';
   --Add SQL user toodb_owner role
   ALTER ROLE db_owner ADD MEMBER app_admin; 
   --Create additional SQL user
   CREATE USER app_user WITH PASSWORD = 'ChangeYourPassword1';
   --grant permission tooSalesLT schema
   GRANT SELECT, INSERT, DELETE, UPDATE ON SalesLT.Product tooapp_user;
   ```

## <a name="create-database-level-firewall"></a><span data-ttu-id="d6942-136">Vytvoření brány firewall na úrovni databáze</span><span class="sxs-lookup"><span data-stu-id="d6942-136">Create database-level firewall</span></span>

<span data-ttu-id="d6942-137">Vytvoření [pravidlo brány firewall na úrovni databáze](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database) pro vaši databázi SQL.</span><span class="sxs-lookup"><span data-stu-id="d6942-137">Create a [database-level firewall rule](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database) for your SQL database.</span></span> <span data-ttu-id="d6942-138">Toto pravidlo brány firewall na úrovni databáze replikuje automaticky toohello sekundární server, který vytvoříte v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="d6942-138">This database-level firewall rule replicates automatically toohello secondary server that you create in this tutorial.</span></span> <span data-ttu-id="d6942-139">Pro zjednodušení (v tomto kurzu) použijte hello veřejnou IP adresu hello počítače, na kterém provádíte hello kroky v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="d6942-139">For simplicity (in this tutorial), use hello public IP address of hello computer on which you are performing hello steps in this tutorial.</span></span> <span data-ttu-id="d6942-140">toodetermine hello IP adresa použitá pro hello pravidlo brány firewall na úrovni serveru pro váš aktuální počítač, najdete v části [vytvoření brány firewall na úrovni serveru](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).</span><span class="sxs-lookup"><span data-stu-id="d6942-140">toodetermine hello IP address used for hello server-level firewall rule for your current computer, see [Create a server-level firewall](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).</span></span>  

- <span data-ttu-id="d6942-141">V okně otevřeného dotazu nahraďte předchozího dotazu hello hello následující dotaz, nahraďte hello IP adresy hello odpovídající IP adresy pro vaše prostředí.</span><span class="sxs-lookup"><span data-stu-id="d6942-141">In your open query window, replace hello previous query with hello following query, replacing hello IP addresses with hello appropriate IP addresses for your environment.</span></span>  

   ```sql
   -- Create database-level firewall setting for your public IP address
   EXECUTE sp_set_database_firewall_rule @name = N'myGeoReplicationFirewallRule',@start_ip_address = '0.0.0.0', @end_ip_address = '0.0.0.0';
   ```

## <a name="create-an-active-geo-replication-auto-failover-group"></a><span data-ttu-id="d6942-142">Vytvořit skupinu aktivní geografickou replikací automatické převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="d6942-142">Create an active geo-replication auto failover group</span></span> 

<span data-ttu-id="d6942-143">Pomocí Azure PowerShell, vytvořte [aktivní geografickou replikací automatické převzetí služeb při selhání skupiny](sql-database-geo-replication-overview.md) mezi existující server Azure SQL a hello nový prázdný server Azure SQL v oblasti Azure, a poté přidejte skupině ukázkové databáze toohello převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="d6942-143">Using Azure PowerShell, create an [active geo-replication auto failover group](sql-database-geo-replication-overview.md) between your existing Azure SQL server and hello new empty Azure SQL server in an Azure region, and then add your sample database toohello failover group.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d6942-144">Tyto rutiny vyžadují Azure PowerShell 4.0.</span><span class="sxs-lookup"><span data-stu-id="d6942-144">These cmdlets require Azure PowerShell 4.0.</span></span> [!INCLUDE [sample-powershell-install](../../includes/sample-powershell-install-no-ssh.md)]
>

1. <span data-ttu-id="d6942-145">Naplnění proměnných pro skripty prostředí PowerShell pomocí hello hodnot pro existující server a ukázkovou databázi a zadejte globálně jedinečná hodnota pro název skupiny pro převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="d6942-145">Populate variables for your PowerShell scripts using hello values for your existing server and sample database, and provide a globally unique value for failover group name.</span></span>

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

2. <span data-ttu-id="d6942-146">Vytvoření prázdného zálohování serveru ve vaší oblasti převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="d6942-146">Create an empty backup server in your failover region.</span></span>

   ```powershell
   $mydrserver = New-AzureRmSqlServer -ResourceGroupName $myresourcegroupname `
      -ServerName $mydrservername `
      -Location $mydrlocation `
      -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $adminlogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))
   $mydrserver   
   ```

3. <span data-ttu-id="d6942-147">Vytvořte skupinu převzetí služeb při selhání mezi dvěma servery hello.</span><span class="sxs-lookup"><span data-stu-id="d6942-147">Create a failover group between hello two servers.</span></span>

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

4. <span data-ttu-id="d6942-148">Přidáte skupinu převzetí služeb při selhání toohello vaší databáze.</span><span class="sxs-lookup"><span data-stu-id="d6942-148">Add your database toohello failover group.</span></span>

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

## <a name="install-java-software"></a><span data-ttu-id="d6942-149">Instalace softwaru Java</span><span class="sxs-lookup"><span data-stu-id="d6942-149">Install Java software</span></span>

<span data-ttu-id="d6942-150">Hello kroky v této části Předpokládejme, že jsou obeznámeni s vývojem pomocí Java a jsou nové tooworking s Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="d6942-150">hello steps in this section assume that you are familiar with developing using Java and are new tooworking with Azure SQL Database.</span></span> 

### <a name="mac-os"></a><span data-ttu-id="d6942-151">**Mac OS**</span><span class="sxs-lookup"><span data-stu-id="d6942-151">**Mac OS**</span></span>
<span data-ttu-id="d6942-152">Otevřete terminálu a přejděte tooa adresáře, kde plánujete vytvoření projektu Java.</span><span class="sxs-lookup"><span data-stu-id="d6942-152">Open your terminal and navigate tooa directory where you plan on creating your Java project.</span></span> <span data-ttu-id="d6942-153">Nainstalujte **brew** a **Maven** zadáním hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="d6942-153">Install **brew** and **Maven** by entering hello following commands:</span></span> 

```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew update
brew install maven
```

<span data-ttu-id="d6942-154">Obsahuje podrobné pokyny k instalaci a konfiguraci prostředí Java a Maven, přejděte hello [sestavení aplikace pomocí systému SQL Server](https://www.microsoft.com/sql-server/developer-get-started/), vyberte **Java**, vyberte **systému MacOS**a pak postupujte podle Hello podrobné pokyny ke konfiguraci Java a Maven v krok 1.2 a 1.3.</span><span class="sxs-lookup"><span data-stu-id="d6942-154">For detailed guidance on installing and configuring Java and Maven environment, go hello [Build an app using SQL Server](https://www.microsoft.com/sql-server/developer-get-started/), select **Java**, select **MacOS**, and then follow hello detailed instructions for configuring Java and Maven in step 1.2 and 1.3.</span></span>

### <a name="linux-ubuntu"></a><span data-ttu-id="d6942-155">**Linux (Ubuntu)**</span><span class="sxs-lookup"><span data-stu-id="d6942-155">**Linux (Ubuntu)**</span></span>
<span data-ttu-id="d6942-156">Otevřete terminálu a přejděte tooa adresáře, kde plánujete vytvoření projektu Java.</span><span class="sxs-lookup"><span data-stu-id="d6942-156">Open your terminal and navigate tooa directory where you plan on creating your Java project.</span></span> <span data-ttu-id="d6942-157">Nainstalujte **Maven** zadáním hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="d6942-157">Install **Maven** by entering hello following commands:</span></span>

```bash
sudo apt-get install maven
```

<span data-ttu-id="d6942-158">Obsahuje podrobné pokyny k instalaci a konfiguraci prostředí Java a Maven, přejděte hello [sestavení aplikace pomocí systému SQL Server](https://www.microsoft.com/sql-server/developer-get-started/), vyberte **Java**, vyberte **Ubuntu**a pak postupujte podle Hello podrobné pokyny ke konfiguraci Java a Maven v kroku 1.4, 1.2 a 1.3.</span><span class="sxs-lookup"><span data-stu-id="d6942-158">For detailed guidance on installing and configuring Java and Maven environment, go hello [Build an app using SQL Server](https://www.microsoft.com/sql-server/developer-get-started/), select **Java**, select **Ubuntu**, and then follow hello detailed instructions for configuring Java and Maven in step 1.2, 1.3, and 1.4.</span></span>

### <a name="windows"></a><span data-ttu-id="d6942-159">**Windows**</span><span class="sxs-lookup"><span data-stu-id="d6942-159">**Windows**</span></span>
<span data-ttu-id="d6942-160">Nainstalujte [Maven](https://maven.apache.org/download.cgi) pomocí instalačního programu oficiální hello.</span><span class="sxs-lookup"><span data-stu-id="d6942-160">Install [Maven](https://maven.apache.org/download.cgi) using hello official installer.</span></span> <span data-ttu-id="d6942-161">Používání Maven toohelp Správa závislostí, vytvoření, testování a spuštění projektu jazyka Java.</span><span class="sxs-lookup"><span data-stu-id="d6942-161">Use Maven toohelp manage dependencies, build, test, and run your Java project.</span></span> <span data-ttu-id="d6942-162">Obsahuje podrobné pokyny k instalaci a konfiguraci prostředí Java a Maven, přejděte hello [sestavení aplikace pomocí systému SQL Server](https://www.microsoft.com/sql-server/developer-get-started/), vyberte **Java**vyberte Windows a potom postupujte podle hello podrobné pokyny pro Konfigurace Java a Maven na krok 1.2 a 1.3.</span><span class="sxs-lookup"><span data-stu-id="d6942-162">For detailed guidance on installing and configuring Java and Maven environment, go hello [Build an app using SQL Server](https://www.microsoft.com/sql-server/developer-get-started/), select **Java**, select Windows, and then follow hello detailed instructions for configuring Java and Maven in step 1.2 and 1.3.</span></span>

## <a name="create-sqldbsample-project"></a><span data-ttu-id="d6942-163">Vytvoření projektu SqlDbSample</span><span class="sxs-lookup"><span data-stu-id="d6942-163">Create SqlDbSample project</span></span>

1. <span data-ttu-id="d6942-164">V konzole příkaz hello (například Bash) vytvořte projekt Maven.</span><span class="sxs-lookup"><span data-stu-id="d6942-164">In hello command console (such as Bash), create a Maven project.</span></span> 
   ```bash
   mvn archetype:generate "-DgroupId=com.sqldbsamples" "-DartifactId=SqlDbSample" "-DarchetypeArtifactId=maven-archetype-quickstart" "-Dversion=1.0.0"
   ```
2. <span data-ttu-id="d6942-165">Typ **Y** a klikněte na tlačítko **Enter**.</span><span class="sxs-lookup"><span data-stu-id="d6942-165">Type **Y** and click **Enter**.</span></span>
3. <span data-ttu-id="d6942-166">Změňte adresáře na nově vytvořený projekt.</span><span class="sxs-lookup"><span data-stu-id="d6942-166">Change directories into your newly created project.</span></span>

   ```bash
   cd SqlDbSamples
   ```

4. <span data-ttu-id="d6942-167">Ve složce projektu pomocí vašeho oblíbeného editoru otevřete soubor pom.xml hello.</span><span class="sxs-lookup"><span data-stu-id="d6942-167">Using your favorite editor, open hello pom.xml file in your project folder.</span></span> 

5. <span data-ttu-id="d6942-168">Přidejte hello ovladač JDBC Microsoft pro projekt Maven tooyour závislost SQL Server tak, že otevřete svém oblíbeném textovém editoru a kopírování a vkládání hello následující řádky do souboru pom.xml.</span><span class="sxs-lookup"><span data-stu-id="d6942-168">Add hello Microsoft JDBC Driver for SQL Server dependency tooyour Maven project by opening your favorite text editor and copying and pasting hello following lines into your pom.xml file.</span></span> <span data-ttu-id="d6942-169">Nepřepisovat existující hodnoty hello naplněnými v souboru hello.</span><span class="sxs-lookup"><span data-stu-id="d6942-169">Do not overwrite hello existing values prepopulated in hello file.</span></span> <span data-ttu-id="d6942-170">Hello JDBC závislostí musí vložení v rámci (hello větší "závislosti" části).</span><span class="sxs-lookup"><span data-stu-id="d6942-170">hello JDBC dependency must be pasted within hello larger “dependencies” section ( ).</span></span>   

   ```xml
   <dependency>
     <groupId>com.microsoft.sqlserver</groupId>
     <artifactId>mssql-jdbc</artifactId>
    <version>6.1.0.jre8</version>
   </dependency>
   ```

6. <span data-ttu-id="d6942-171">Zadejte verzi hello Java toocompile hello projektu před přidáním hello následující části "vlastnosti" do souboru pom.xml hello po části "závislosti" hello.</span><span class="sxs-lookup"><span data-stu-id="d6942-171">Specify hello version of Java toocompile hello project against by adding hello following “properties” section into hello pom.xml file after hello "dependencies" section.</span></span> 

   ```xml
   <properties>
     <maven.compiler.source>1.8</maven.compiler.source>
     <maven.compiler.target>1.8</maven.compiler.target>
   </properties>
   ```
7. <span data-ttu-id="d6942-172">Přidejte následující hello "sestavení" části do souboru pom.xml hello po hello "vlastnosti" části toosupport souborů manifestu v JAR.</span><span class="sxs-lookup"><span data-stu-id="d6942-172">Add hello following "build" section into hello pom.xml file after hello "properties" section toosupport manifest files in jars.</span></span>       

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
8. <span data-ttu-id="d6942-173">Uložte a zavřete soubor pom.xml hello.</span><span class="sxs-lookup"><span data-stu-id="d6942-173">Save and close hello pom.xml file.</span></span>
9. <span data-ttu-id="d6942-174">Otevřete soubor App.java hello (C:\apache-maven-3.5.0\SqlDbSample\src\main\java\com\sqldbsamples\App.java) a nahraďte obsah hello hello následující obsah.</span><span class="sxs-lookup"><span data-stu-id="d6942-174">Open hello App.java file (C:\apache-maven-3.5.0\SqlDbSample\src\main\java\com\sqldbsamples\App.java) and replace hello contents with hello following contents.</span></span> <span data-ttu-id="d6942-175">Nahraďte název skupiny pro převzetí služeb při selhání hello hello název pro skupinu pro převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="d6942-175">Replace hello failover group name with hello name for your failover group.</span></span> <span data-ttu-id="d6942-176">Pokud jste změnili hello hodnoty pro název databáze hello, uživatele nebo heslo, změna také tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="d6942-176">If you have changed hello values for hello database name, user, or password, change those values as well.</span></span>

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
      // Insert data into hello product table with a unique product name that we can use toofind hello product again later
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
      // Query hello data that was previously inserted into hello primary database from hello geo replicated database
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
      // Query hello high water mark id that is stored in hello table toobe able toomake unique inserts 
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
6. <span data-ttu-id="d6942-177">Uložte a zavřete soubor App.java hello.</span><span class="sxs-lookup"><span data-stu-id="d6942-177">Save and close hello App.java file.</span></span>

## <a name="compile-and-run-hello-sqldbsample-project"></a><span data-ttu-id="d6942-178">Zkompilování a spuštění projektu SqlDbSample hello</span><span class="sxs-lookup"><span data-stu-id="d6942-178">Compile and run hello SqlDbSample project</span></span>

1. <span data-ttu-id="d6942-179">V konzole příkaz hello spusťte příkaz toofollowing.</span><span class="sxs-lookup"><span data-stu-id="d6942-179">In hello command console, execute toofollowing command.</span></span>

   ```bash
   mvn package
   ```
2. <span data-ttu-id="d6942-180">Po dokončení, spusťte následující příkaz toorun hello aplikace (spuštění o jednu hodinu Pokud zastavíte ručně) hello:</span><span class="sxs-lookup"><span data-stu-id="d6942-180">When finished, execute hello following command toorun hello application (it runs for about 1 hour unless you stop it manually):</span></span>

   ```bash
   mvn -q -e exec:java "-Dexec.mainClass=com.sqldbsamples.App"
   
   #######################################
   ## GEO DISTRIBUTED DATABASE TUTORIAL ##
   #######################################

   1. insert on primary successful, read from secondary successful
   2. insert on primary successful, read from secondary successful
   3. insert on primary successful, read from secondary successful
   ```

## <a name="perform-disaster-recovery-drill"></a><span data-ttu-id="d6942-181">Provedení postupu zotavení po havárii</span><span class="sxs-lookup"><span data-stu-id="d6942-181">Perform disaster recovery drill</span></span>

1. <span data-ttu-id="d6942-182">Volání ruční převzetí služeb při selhání skupiny převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="d6942-182">Call manual failover of failover group.</span></span> 

   ```powershell
   Switch-AzureRMSqlDatabaseFailoverGroup `
   -ResourceGroupName $myresourcegroupname  `
   -ServerName $mydrservername `
   -FailoverGroupName $myfailovergroupname
   ```

2. <span data-ttu-id="d6942-183">Pozorovat výsledky. aplikace hello během převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="d6942-183">Observe hello application results during failover.</span></span> <span data-ttu-id="d6942-184">Některé vloží selžou aktualizuje hello mezipaměť DNS.</span><span class="sxs-lookup"><span data-stu-id="d6942-184">Some inserts fail while hello DNS cache refreshes.</span></span>     

3. <span data-ttu-id="d6942-185">Zjistěte, jaké role provádí vašeho serveru pro obnovení po havárii.</span><span class="sxs-lookup"><span data-stu-id="d6942-185">Find out which role your disaster recovery server is performing.</span></span>

   ```powershell
   $mydrserver.ReplicationRole
   ```

4. <span data-ttu-id="d6942-186">Navrácení služeb po obnovení.</span><span class="sxs-lookup"><span data-stu-id="d6942-186">Failback.</span></span>

   ```powershell
   Switch-AzureRMSqlDatabaseFailoverGroup `
   -ResourceGroupName $myresourcegroupname  `
   -ServerName $myservername `
   -FailoverGroupName $myfailovergroupname
   ```

5. <span data-ttu-id="d6942-187">Pozorovat výsledky. aplikace hello během navrácení služeb po obnovení.</span><span class="sxs-lookup"><span data-stu-id="d6942-187">Observe hello application results during failback.</span></span> <span data-ttu-id="d6942-188">Některé vloží selžou aktualizuje hello mezipaměť DNS.</span><span class="sxs-lookup"><span data-stu-id="d6942-188">Some inserts fail while hello DNS cache refreshes.</span></span>     

6. <span data-ttu-id="d6942-189">Zjistěte, jaké role provádí vašeho serveru pro obnovení po havárii.</span><span class="sxs-lookup"><span data-stu-id="d6942-189">Find out which role your disaster recovery server is performing.</span></span>

   ```powershell
   $fileovergroup = Get-AzureRMSqlDatabaseFailoverGroup `
      -FailoverGroupName $myfailovergroupname `
      -ResourceGroupName $myresourcegroupname `
      -ServerName $mydrservername
   $fileovergroup.ReplicationRole
   ```
## <a name="next-steps"></a><span data-ttu-id="d6942-190">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d6942-190">Next steps</span></span> 

<span data-ttu-id="d6942-191">Další informace najdete v tématu [aktivní geografickou replikaci a převzetí služeb při selhání skupiny](sql-database-geo-replication-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d6942-191">For more information, see [Active geo-replication and failover groups](sql-database-geo-replication-overview.md).</span></span>
