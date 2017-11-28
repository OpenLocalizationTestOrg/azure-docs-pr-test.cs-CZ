---
title: "Implementovat víceklientské aplikace SaaS s Azure SQL Database | Microsoft Docs"
description: "Implementujte víceklientské aplikace SaaS s Azure SQL Database."
services: sql-database
documentationcenter: 
author: AyoOlubeko
manager: jhubbard
editor: monicar
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,scale out apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 05/08/2017
ms.author: AyoOlubek
ms.openlocfilehash: 0aea69d86a51c38c99a72f46737de1eea27bef83
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="implement-a-multi-tenant-saas-application-using-azure-sql-database"></a><span data-ttu-id="28580-103">Implementace víceklientské aplikace SaaS používání Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="28580-103">Implement a multi-tenant SaaS application using Azure SQL Database</span></span>

<span data-ttu-id="28580-104">Víceklientské aplikace je aplikace hostované v cloudovém prostředí a který poskytuje stejnou sadu služeb stovkami nebo tisíci klienty, kteří sdílet nebo vidět uživatele toho druhého data.</span><span class="sxs-lookup"><span data-stu-id="28580-104">A multi-tenant application is an application hosted in a cloud environment and that provides the same set of services to hundreds or thousands of tenants who do not share or see each other’s data.</span></span> <span data-ttu-id="28580-105">Příkladem je aplikace SaaS, která poskytuje služby klientům v prostředí hostovaných v cloudu.</span><span class="sxs-lookup"><span data-stu-id="28580-105">An example is an SaaS application that provides services to tenants in a cloud-hosted environment.</span></span> <span data-ttu-id="28580-106">Tento model izoluje data pro každého klienta a optimalizuje rozdělení prostředků pro náklady.</span><span class="sxs-lookup"><span data-stu-id="28580-106">This model isolates the data for each tenant and optimizes the distribution of resources for cost.</span></span> 

<span data-ttu-id="28580-107">Tento kurz ukazuje, jak vytvořit aplikaci SaaS víceklientské používání Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="28580-107">This tutorial demonstrates how to create a multi-tenant SaaS application using Azure SQL Database.</span></span>

<span data-ttu-id="28580-108">V tomto kurzu se naučíte:</span><span class="sxs-lookup"><span data-stu-id="28580-108">In this tutorial, you will learn to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="28580-109">Nastavení prostředí pro databázi pro podporu víceklientské aplikace SaaS, pomocí vzoru databáze za klienta</span><span class="sxs-lookup"><span data-stu-id="28580-109">Set up a database environment to support a multi-tenant SaaS application, using the Database-per-tenant pattern</span></span>
> * <span data-ttu-id="28580-110">Vytvoření klienta katalogu</span><span class="sxs-lookup"><span data-stu-id="28580-110">Create a tenant catalog</span></span>
> * <span data-ttu-id="28580-111">Zřídit klienta databázi a její registrace v katalogu klienta</span><span class="sxs-lookup"><span data-stu-id="28580-111">Provision a tenant database and register it in the tenant catalog</span></span>
> * <span data-ttu-id="28580-112">Nastavení ukázkové aplikace Java</span><span class="sxs-lookup"><span data-stu-id="28580-112">Set up a sample Java application</span></span> 
> * <span data-ttu-id="28580-113">Přístup k klienta databází jednoduchou konzolovou aplikaci Java</span><span class="sxs-lookup"><span data-stu-id="28580-113">Access tenant databases simple a Java console application</span></span>
> * <span data-ttu-id="28580-114">Odstranění klienta</span><span class="sxs-lookup"><span data-stu-id="28580-114">Delete a tenant</span></span>

<span data-ttu-id="28580-115">Pokud nemáte předplatné Azure, [vytvořit bezplatný účet](https://azure.microsoft.com/free/) před zahájením.</span><span class="sxs-lookup"><span data-stu-id="28580-115">If you don't have an Azure subscription, [create a free account](https://azure.microsoft.com/free/) before you begin.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="28580-116">Požadavky</span><span class="sxs-lookup"><span data-stu-id="28580-116">Prerequisites</span></span>

<span data-ttu-id="28580-117">K dokončení tohoto kurzu, zkontrolujte, zda že máte:</span><span class="sxs-lookup"><span data-stu-id="28580-117">To complete this tutorial, make sure you have:</span></span>

* <span data-ttu-id="28580-118">Nainstalovaná nejnovější verze prostředí PowerShell a [nejnovější sadu Azure PowerShell SDK](http://azure.microsoft.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="28580-118">Installed the newest version of PowerShell and the [latest Azure PowerShell SDK](http://azure.microsoft.com/downloads/)</span></span>

* <span data-ttu-id="28580-119">Nainstalovat nejnovější verzi [SQL Server Management Studio](http://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).</span><span class="sxs-lookup"><span data-stu-id="28580-119">Installed the latest version of [SQL Server Management Studio](http://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).</span></span> <span data-ttu-id="28580-120">Instalace SQL Server Management Studio také nainstaluje nejnovější verzi SQLPackage, nástroje příkazového řádku, které je možné automatizovat řadu úloh vývoj databáze.</span><span class="sxs-lookup"><span data-stu-id="28580-120">Installing SQL Server Management Studio also installs the latest version of SQLPackage, a command-line utility that can be used to automate a range of database development tasks.</span></span>

* <span data-ttu-id="28580-121">Nainstalována [Java Runtime Environment (JRE) 8](http://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html) a [nejnovější JAVA Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) nainstalovaný na počítači.</span><span class="sxs-lookup"><span data-stu-id="28580-121">Installed the [Java Runtime Environment (JRE) 8](http://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html) and the [latest JAVA Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) installed on your machine.</span></span> 

* <span data-ttu-id="28580-122">Nainstalovat [Apache Maven](https://maven.apache.org/download.cgi).</span><span class="sxs-lookup"><span data-stu-id="28580-122">Installed [Apache Maven](https://maven.apache.org/download.cgi).</span></span> <span data-ttu-id="28580-123">Maven se použije ke správě závislosti, vytváření, testování a spuštění ukázkového projektu Java</span><span class="sxs-lookup"><span data-stu-id="28580-123">Maven will be used to help manage dependencies, build, test and run the sample Java project</span></span>

## <a name="set-up-data-environment"></a><span data-ttu-id="28580-124">Nastavení prostředí pro data</span><span class="sxs-lookup"><span data-stu-id="28580-124">Set up data environment</span></span>

<span data-ttu-id="28580-125">Bude zřizování databáze za klienta.</span><span class="sxs-lookup"><span data-stu-id="28580-125">You will be provisioning a database per tenant.</span></span> <span data-ttu-id="28580-126">Model databáze za klienta poskytuje izolaci mezi klienty, s minimálními DevOps náklady na nejvyšší úrovni.</span><span class="sxs-lookup"><span data-stu-id="28580-126">The database-per-tenant model provides the highest degree of isolation between tenants, with little DevOps cost.</span></span> <span data-ttu-id="28580-127">K optimalizaci nákladů prostředků cloudu, budete se také zřizovat klienta databází do pružného fondu, která umožňuje optimalizovat výkon ceny pro skupinu databází.</span><span class="sxs-lookup"><span data-stu-id="28580-127">To optimize the cost of cloud resources, you will also be provisioning the tenant databases into an elastic pool which allows you to optimize the price performance for a group of databases.</span></span> <span data-ttu-id="28580-128">Další informace o jiné databázi zřizování modely [zde](sql-database-design-patterns-multi-tenancy-saas-applications.md#multi-tenant-data-models).</span><span class="sxs-lookup"><span data-stu-id="28580-128">To learn about other database provisioning models [see here](sql-database-design-patterns-multi-tenancy-saas-applications.md#multi-tenant-data-models).</span></span>

<span data-ttu-id="28580-129">Postupujte podle těchto kroků můžete vytvořit SQL server a fondu elastické databáze, který bude hostitelem všechny databáze klienta.</span><span class="sxs-lookup"><span data-stu-id="28580-129">Follow these steps to create a SQL server and an elastic pool that will host all your tenant databases.</span></span> 

1. <span data-ttu-id="28580-130">Vytvoření proměnné, které chcete ukládat hodnoty, které se použijí ve zbývající části tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="28580-130">Create variables to store values that will be used in the rest of the tutorial.</span></span> <span data-ttu-id="28580-131">Nezapomeňte upravit proměnnou IP adres, aby obsahovala IP adresu</span><span class="sxs-lookup"><span data-stu-id="28580-131">Make sure to modify the IP address variable to include your IP address</span></span> 
   
   ```PowerShell 
   # Set an admin login and password for your database
   $adminlogin = "ServerAdmin"
   $password = "ChangeYourAdminPassword1"
   
   # Create random unique names for logical server and tenants
   $servername = "server-$(Get-Random)"
   $tenant1 = "geolamice"
   $tenant2 = "ranplex"
   
   # Store current client IP address (modify to include your IP address)
   $startIpAddress = 0.0.0.0 
   $endIpAddress = 0.0.0.0
   ```
   
2. <span data-ttu-id="28580-132">Přihlášení k Azure a vytvoření SQL server a elastického fondu</span><span class="sxs-lookup"><span data-stu-id="28580-132">Login to Azure and create a SQL server and elastic pool</span></span> 
   
   ```PowerShell
   # Login to Azure 
   Login-AzureRmAccount
   
   # Create resource group 
   New-AzureRmResourceGroup -Name "myResourceGroup" -Location "northcentralus"
   
   # Create logical SQL Server with firewall rules 
   New-AzureRmSqlServer -ResourceGroupName "myResourceGroup" `
       -ServerName $servername `
       -Location "northcentralus" `
       -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $adminlogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))
   
   New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourcegroupname `
       -ServerName $servername `
       -FirewallRuleName "singleAddress" -StartIpAddress $startIpAddress -EndIpAddress $endIpAddress
   
   # Create elastic pool 
   New-AzureRmSqlElasticPool -ResourceGroupName "myResourceGroup"
       -ServerName $servername `
       -ElasticPoolName "myElasticPool" `
       -Edition "Standard" `
       -Dtu 50 `
       -DatabaseDtuMin 10 `
       -DatabaseDtuMax 20
   ```
   
## <a name="create-tenant-catalog"></a><span data-ttu-id="28580-133">Vytvořit katalog klienta</span><span class="sxs-lookup"><span data-stu-id="28580-133">Create tenant catalog</span></span> 

<span data-ttu-id="28580-134">V aplikaci SaaS více klientů je důležité vědět, se uloží informace pro klienta.</span><span class="sxs-lookup"><span data-stu-id="28580-134">In a multi-tenant SaaS application, it’s important to know where information for a tenant is stored.</span></span> <span data-ttu-id="28580-135">Běžně je uložen v databázi katalogu.</span><span class="sxs-lookup"><span data-stu-id="28580-135">This is commonly stored in a catalog database.</span></span> <span data-ttu-id="28580-136">Databáze katalogu se používá k ukládání mapování mezi klientem a databáze, ve kterém je uložený dat daného klienta.</span><span class="sxs-lookup"><span data-stu-id="28580-136">The catalog database is used to hold a mapping between a tenant and a database in which that tenant’s data is stored.</span></span>  <span data-ttu-id="28580-137">Základní vzor platí jak pro více klientů nebo databázi jednoho klienta se používá.</span><span class="sxs-lookup"><span data-stu-id="28580-137">The basic pattern applies whether a multi-tenant or a single-tenant database is used.</span></span>

<span data-ttu-id="28580-138">Postupujte podle těchto kroků k vytvoření databáze katalogu pro ukázkovou aplikaci SaaS.</span><span class="sxs-lookup"><span data-stu-id="28580-138">Follow these steps to create a catalog database for the sample SaaS application.</span></span>

```PowerShell
# Create empty database in pool
New-AzureRmSqlDatabase  -ResourceGroupName "myResourceGroup" `
    -ServerName $servername `
    -DatabaseName "tenantCatalog" `
    -ElasticPoolName "myElasticPool"

# Create table to track mapping between tenants and their databases
$commandText = "
CREATE TABLE Tenants
(
   TenantId         INT IDENTITY PRIMARY KEY,
   TenantName       NVARCHAR(128) NOT NULL,
   TenantDatabase   NVARCHAR(128) NOT NULL
);

CREATE INDEX IX_TenantName ON Tenants (TenantName);"

Invoke-SqlCmd `
    -Username $adminlogin `
    -Password $password `
    -ServerInstance ($servername + ".database.windows.net") `
    -Database "tenantCatalog" `
    -ConnectionTimeout 30 `
    -Query $commandText `
    -EncryptConnection
```

## <a name="provision-database-for-tenant1-and-register-in-tenant-catalog"></a><span data-ttu-id="28580-139">Zřídit databázi pro 'tenant1' a zaregistrovat v katalogu klienta</span><span class="sxs-lookup"><span data-stu-id="28580-139">Provision database for 'tenant1' and register in tenant catalog</span></span> 
<span data-ttu-id="28580-140">Zřídit databázi pro nového klienta, tenant1' a registraci tohoto klienta v katalogu pomocí prostředí Powershell.</span><span class="sxs-lookup"><span data-stu-id="28580-140">Use Powershell to provision a database for a new tenant 'tenant1' and register this tenant in the catalog.</span></span> 

```PowerShell
# Create empty database in pool for 'tenant1'
New-AzureRmSqlDatabase  -ResourceGroupName "myResourceGroup" `
    -ServerName $servername `
    -DatabaseName $tenant1 `
    -ElasticPoolName "myElasticPool"

# Create table WhoAmI and insert tenant name into the table 
$commandText = "
CREATE TABLE WhoAmI (TenantName NVARCHAR(128) NOT NULL);
INSERT INTO WhoAmI VALUES ('Tenant $tenant1');"

Invoke-SqlCmd `
    -Username $adminlogin `
    -Password $password `
    -ServerInstance ($servername + ".database.windows.net") `
    -Database $tenant1 `
    -ConnectionTimeout 30 `
    -Query $commandText `
    -EncryptConnection

# Register 'tenant1' in the tenant catalog 
$commandText = "
INSERT INTO Tenants VALUES ('$tenant1', '$tenant1');"
Invoke-SqlCmd `
    -Username $adminlogin `
    -Password $password `
    -ServerInstance ($servername + ".database.windows.net") `
    -Database "tenantCatalog" `
    -ConnectionTimeout 30 `
    -Query $commandText `
    -EncryptConnection
```

## <a name="provision-database-for-tenant2-and-register-in-tenant-catalog"></a><span data-ttu-id="28580-141">Zřídit databázi pro 'tenant2' a zaregistrovat v katalogu klienta</span><span class="sxs-lookup"><span data-stu-id="28580-141">Provision database for 'tenant2' and register in tenant catalog</span></span>
<span data-ttu-id="28580-142">Zřídit databázi pro nového klienta, tenant2' a registraci tohoto klienta v katalogu pomocí prostředí Powershell.</span><span class="sxs-lookup"><span data-stu-id="28580-142">Use Powershell to provision a database for a new tenant 'tenant2' and register this tenant in the catalog.</span></span> 

```PowerShell
# Create empty database in pool for 'tenant2'
New-AzureRmSqlDatabase  -ResourceGroupName "myResourceGroup" `
    -ServerName $servername `
    -DatabaseName $tenant2 `
    -ElasticPoolName "myElasticPool"

# Create table WhoAmI and insert tenant name into the table 
$commandText = "
CREATE TABLE WhoAmI (TenantName NVARCHAR(128) NOT NULL);
INSERT INTO WhoAmI VALUES ('Tenant $tenant2');"

Invoke-SqlCmd `
    -Username $adminlogin `
    -Password $password `
    -ServerInstance ($servername + ".database.windows.net") `
    -Database $tenant2 `
    -ConnectionTimeout 30 `
    -Query $commandText `
    -EncryptConnection

# Register tenant 'tenant2' in the tenant catalog 
$commandText = "
INSERT INTO Tenants VALUES ('$tenant2', '$tenant2');"
Invoke-SqlCmd `
    -Username $adminlogin `
    -Password $password `
    -ServerInstance ($servername + ".database.windows.net") `
    -Database "tenantCatalog" `
    -ConnectionTimeout 30 `
    -Query $commandText `
    -EncryptConnection
```

## <a name="set-up-sample-java-application"></a><span data-ttu-id="28580-143">Nastavte ukázkovou aplikaci Java</span><span class="sxs-lookup"><span data-stu-id="28580-143">Set up sample Java application</span></span> 

1. <span data-ttu-id="28580-144">Vytvořte projekt maven.</span><span class="sxs-lookup"><span data-stu-id="28580-144">Create a maven project.</span></span> <span data-ttu-id="28580-145">V okně příkazového řádku zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="28580-145">Type the following in a command prompt window:</span></span>
   
   ```
   mvn archetype:generate -DgroupId=com.microsoft.sqlserver -DartifactId=mssql-jdbc -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
   ```
   
2. <span data-ttu-id="28580-146">Přidejte tuto závislost, úroveň jazyka a sestavení možnost pro podporu manifestu souborů v JAR do souboru pom.xml:</span><span class="sxs-lookup"><span data-stu-id="28580-146">Add this dependency, language level, and build option to support manifest files in jars to the pom.xml file:</span></span>
   
   ```XML
   <dependency>
         <groupId>com.microsoft.sqlserver</groupId>
         <artifactId>mssql-jdbc</artifactId>
         <version>6.1.0.jre8</version>
   </dependency>
   
   <properties>
         <maven.compiler.source>1.8</maven.compiler.source>
         <maven.compiler.target>1.8</maven.compiler.target>
   </properties>
   
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

3. <span data-ttu-id="28580-147">Přidejte následující kód do soubor App.java:</span><span class="sxs-lookup"><span data-stu-id="28580-147">Add the following into the App.java file:</span></span>

   ```java 
   package com.sqldbsamples;
   
   import java.util.Map;
   
   import java.util.HashMap;
   
   import java.io.BufferedReader;
   
   import java.io.InputStreamReader;
   
   import java.sql.Connection;
   
   import java.sql.Statement;
   
   import java.sql.PreparedStatement;
   
   import java.sql.ResultSet;
   
   import java.sql.DriverManager;
   
   public class App {
   
   private static final String SERVER_NAME = "your-server-name";
   
   private static final String CATALOG_DB_NAME = "tenantCatalog";
   
   private static final String USER = "ServerAdmin";
   
   private static final String PASSWORD = "ChangeYourAdminPassword1";
   
   private static final String CATALOG_DB_URL = String.format("jdbc:sqlserver://%s.database.windows.net:1433;database=%s;user=%s;password=%s;encrypt=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;", SERVER_NAME, CATALOG_DB_NAME, USER, PASSWORD);
   
   private static final String CMD_LIST = "LIST";

   private static final String CMD_QUERY = "QUERY";

   private static final String CMD_QUIT = "QUIT";
   
   public static void main(String[] args) {
   
   System.out.println("\n############################");
   
   System.out.println("## SAAS DATABASE TUTORIAL ##");
   
   System.out.println("############################\n");
   
   System.out.println("OPTIONS");
   
   System.out.println(" " + CMD_LIST + " - list tenants");
   
   System.out.println(" " + CMD_QUERY + " <NAME> - connect and tenant query tenant <NAME>");
   
   System.out.println(" " + CMD_QUIT + " - quit the application\n");
   
   try (BufferedReader br = new BufferedReader(new InputStreamReader(System.in))) {
   
   while(true) {
   
   String[] input = br.readLine().split("\\s");
   
   if (null != input && input.length > 0) {
   
   if (input[0].equalsIgnoreCase(CMD_LIST)) {
   
   listTenants();
   
   } else if (input[0].toLowerCase().startsWith(CMD_QUERY.toLowerCase()) && input.length == 2) {
   
   queryTenant(input[1].trim());
   
   } else if (input[0].equalsIgnoreCase(CMD_QUIT)) {
   
   break;
   
   } else {
   
   System.out.println(" -> Command not supported");
   
   }
   
   }
   
   System.out.println("");
   
   }
   
   } catch (Exception e) {
   
   System.out.println(e.getMessage());
   
   e.printStackTrace();
   
   }
   
   }
   
   private static void listTenants() {
   
   // List all tenants that currently exist in the system
   
   String sql = "SELECT TenantName FROM Tenants";
   
   try (Connection connection = DriverManager.getConnection(CATALOG_DB_URL);
   
   Statement stmt = connection.createStatement();
   
   ResultSet resultSet = stmt.executeQuery(sql)) {
   
   while (resultSet.next()) {
   
   System.out.println(" -> " + resultSet.getString(1));
   
   }
   
   } catch (Exception e) {
   
   System.out.println(e.getMessage());
   
   e.printStackTrace();
   
   }
   
   }
   
   private static void queryTenant(String name) {
   
   // Query the data that was previously inserted into the primary database from the geo replicated database
   
   String url = null;
   
   String sql = "SELECT TenantDatabase FROM Tenants WHERE TenantName = ?";
   
   try (Connection connection = DriverManager.getConnection(CATALOG_DB_URL);
   
   PreparedStatement pstmt = connection.prepareStatement(sql)) {
   
   pstmt.setString(1, name);
   
   try (ResultSet resultSet = pstmt.executeQuery()) {
   
   if (resultSet.next()) {
   
   url = String.format("jdbc:sqlserver://%s.database.windows.net:1433;database=%s;user=%s;password=%s;encrypt=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;", SERVER_NAME, resultSet.getString(1), USER, PASSWORD);
   
   }
   
   }
   
   } catch (Exception e) {
   
   System.out.println(e.getMessage());
   
   e.printStackTrace();
   
   }
   
   if (null != url) {
   
   String tenantSql = "SELECT TenantName FROM WhoAmI";
   
   try (Connection connection = DriverManager.getConnection(url);
   
   Statement stmt = connection.createStatement();

   ResultSet resultSet = stmt.executeQuery(tenantSql)) {
   
   while (resultSet.next()) {
   
   System.out.println(" -> Entry in table WhoAmI in tenant " + name + " is: " + resultSet.getString(1));
   
   }
   
   } catch (Exception e) {
   
   System.out.println(e.getMessage());
   
   e.printStackTrace();
   
   }
   
   } else {
   
   System.out.println(" -> Tenant " + name + " not found");
   
   }
   
   }
   
   }
   ```

4. <span data-ttu-id="28580-148">Uložte soubor.</span><span class="sxs-lookup"><span data-stu-id="28580-148">Save the file.</span></span>

5. <span data-ttu-id="28580-149">Přejděte do konzoly příkazu a provést</span><span class="sxs-lookup"><span data-stu-id="28580-149">Go to command console and execute</span></span>

   ```bash
   mvn package
   ```

6. <span data-ttu-id="28580-150">Po dokončení spuštěním následujících příkazů spusťte aplikaci</span><span class="sxs-lookup"><span data-stu-id="28580-150">When finished, execute the following to run the application</span></span> 
   
   ```
   mvn -q -e exec:java "-Dexec.mainClass=com.sqldbsamples.App"
   ```
   
<span data-ttu-id="28580-151">Pokud úspěšně proběhne, výstup bude vypadat například takto:</span><span class="sxs-lookup"><span data-stu-id="28580-151">The output will look like this if it runs successfully:</span></span>

```
############################

## SAAS DATABASE TUTORIAL ##

############################

OPTIONS

LIST - list tenants

QUERY <NAME> - connect and tenant query tenant <NAME>

QUIT - quit the application

* List the tenants

* Query tenants you created
```

## <a name="delete-first-tenant"></a><span data-ttu-id="28580-152">Odstranit první klienta</span><span class="sxs-lookup"><span data-stu-id="28580-152">Delete first tenant</span></span> 
<span data-ttu-id="28580-153">Pomocí prostředí PowerShell můžete odstranit položku databáze a katalog klienta pro prvního klienta.</span><span class="sxs-lookup"><span data-stu-id="28580-153">Use PowerShell to delete the tenant database and catalog entry for the first tenant.</span></span>

```PowerShell
# Remove 'tenant1' from catalog 
$commandText = "DELETE FROM Tenants WHERE TenantName = '$tenant1';"
Invoke-SqlCmd `
    -Username $adminlogin `
    -Password $password `
    -ServerInstance ($servername + ".database.windows.net") `
    -Database "tenantCatalog" `
    -ConnectionTimeout 30 `
    -Query $commandText `
    -EncryptConnection

# Delete database 
Remove-AzureRmSqlDatabase -ResourceGroupName "myResourceGroup" `
    -ServerName $servername `
    -DatabaseName $tenant1
```

<span data-ttu-id="28580-154">Zkuste se připojit k 'tenant1' pomocí aplikace Java.</span><span class="sxs-lookup"><span data-stu-id="28580-154">Try connecting to 'tenant1' using the Java application.</span></span> <span data-ttu-id="28580-155">Zobrazí se chyba oznamující, že klient neexistuje.</span><span class="sxs-lookup"><span data-stu-id="28580-155">You will get an error stating that the tenant does not exist.</span></span>

## <a name="next-steps"></a><span data-ttu-id="28580-156">Další kroky</span><span class="sxs-lookup"><span data-stu-id="28580-156">Next steps</span></span> 

<span data-ttu-id="28580-157">V tomto kurzu jste se dozvěděli na:</span><span class="sxs-lookup"><span data-stu-id="28580-157">In this tutorial, you learned to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="28580-158">Nastavení prostředí pro databázi pro podporu víceklientské aplikace SaaS, pomocí vzoru databáze za klienta</span><span class="sxs-lookup"><span data-stu-id="28580-158">Set up a database environment to support a multi-tenant SaaS application, using the Database-per-tenant pattern</span></span>
> * <span data-ttu-id="28580-159">Vytvoření klienta katalogu</span><span class="sxs-lookup"><span data-stu-id="28580-159">Create a tenant catalog</span></span>
> * <span data-ttu-id="28580-160">Zřídit klienta databázi a její registrace v katalogu klienta</span><span class="sxs-lookup"><span data-stu-id="28580-160">Provision a tenant database and register it in the tenant catalog</span></span>
> * <span data-ttu-id="28580-161">Nastavení ukázkové aplikace Java</span><span class="sxs-lookup"><span data-stu-id="28580-161">Set up a sample Java application</span></span> 
> * <span data-ttu-id="28580-162">Přístup k klienta databází jednoduchou konzolovou aplikaci Java</span><span class="sxs-lookup"><span data-stu-id="28580-162">Access tenant databases simple a Java console application</span></span>
> * <span data-ttu-id="28580-163">Odstranění klienta</span><span class="sxs-lookup"><span data-stu-id="28580-163">Delete a tenant</span></span>

* <span data-ttu-id="28580-164">Ukázky pro běžné úkoly, PowerShell najdete v části [ukázky PowerShell databáze SQL](https://docs.microsoft.com/azure/sql-database/sql-database-powershell-samples)</span><span class="sxs-lookup"><span data-stu-id="28580-164">PowerShell samples for common tasks, see [SQL Database PowerShell samples](https://docs.microsoft.com/azure/sql-database/sql-database-powershell-samples)</span></span>

* <span data-ttu-id="28580-165">Vzory víceklientské SaaS aplikací najdete v tématu návrhu [vzory návrhu](https://docs.microsoft.com/azure/sql-database/sql-database-design-patterns-multi-tenancy-saas-applications)</span><span class="sxs-lookup"><span data-stu-id="28580-165">Design patterns for multi-tenant SaaS applications see [Design patterns](https://docs.microsoft.com/azure/sql-database/sql-database-design-patterns-multi-tenancy-saas-applications)</span></span>

* <span data-ttu-id="28580-166">Ukázky jazyka Java pro běžné úlohy, Azure, najdete v části [středisko pro vývojáře Java](https://azure.microsoft.com/develop/java/)</span><span class="sxs-lookup"><span data-stu-id="28580-166">Java samples for common Azure tasks, see [Java Developer Center](https://azure.microsoft.com/develop/java/)</span></span>



