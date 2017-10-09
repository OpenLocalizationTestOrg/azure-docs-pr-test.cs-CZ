---
title: "aaaImplement víceklientské aplikace SaaS s Azure SQL Database | Microsoft Docs"
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
ms.openlocfilehash: b87b8f296e2c20a8f674b56375f43fdc92df76d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="implement-a-multi-tenant-saas-application-using-azure-sql-database"></a><span data-ttu-id="b394e-103">Implementace víceklientské aplikace SaaS používání Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="b394e-103">Implement a multi-tenant SaaS application using Azure SQL Database</span></span>

<span data-ttu-id="b394e-104">Hello stejné nastavení služby toohundreds nebo tisíce klienty, kteří sdílet nebo vidět uživatele toho druhého data, která poskytuje víceklientské aplikace je aplikace hostované v cloudovém prostředí.</span><span class="sxs-lookup"><span data-stu-id="b394e-104">A multi-tenant application is an application hosted in a cloud environment and that provides hello same set of services toohundreds or thousands of tenants who do not share or see each other’s data.</span></span> <span data-ttu-id="b394e-105">Příkladem je aplikace SaaS, která poskytuje služby tootenants v prostředí hostovaných v cloudu.</span><span class="sxs-lookup"><span data-stu-id="b394e-105">An example is an SaaS application that provides services tootenants in a cloud-hosted environment.</span></span> <span data-ttu-id="b394e-106">Tento model izoluje hello dat pro každého klienta a optimalizuje distribučního hello prostředků pro náklady.</span><span class="sxs-lookup"><span data-stu-id="b394e-106">This model isolates hello data for each tenant and optimizes hello distribution of resources for cost.</span></span> 

<span data-ttu-id="b394e-107">Tento kurz ukazuje, jak toocreate aplikaci SaaS víceklientské používání Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="b394e-107">This tutorial demonstrates how toocreate a multi-tenant SaaS application using Azure SQL Database.</span></span>

<span data-ttu-id="b394e-108">V tomto kurzu se naučíte:</span><span class="sxs-lookup"><span data-stu-id="b394e-108">In this tutorial, you will learn to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="b394e-109">Nastavení databáze prostředí toosupport víceklientské aplikace SaaS, pomocí vzoru databáze za klienta hello</span><span class="sxs-lookup"><span data-stu-id="b394e-109">Set up a database environment toosupport a multi-tenant SaaS application, using hello Database-per-tenant pattern</span></span>
> * <span data-ttu-id="b394e-110">Vytvoření klienta katalogu</span><span class="sxs-lookup"><span data-stu-id="b394e-110">Create a tenant catalog</span></span>
> * <span data-ttu-id="b394e-111">Zřídit klienta databázi a její registrace v katalogu klienta hello</span><span class="sxs-lookup"><span data-stu-id="b394e-111">Provision a tenant database and register it in hello tenant catalog</span></span>
> * <span data-ttu-id="b394e-112">Nastavení ukázkové aplikace Java</span><span class="sxs-lookup"><span data-stu-id="b394e-112">Set up a sample Java application</span></span> 
> * <span data-ttu-id="b394e-113">Přístup k klienta databází jednoduchou konzolovou aplikaci Java</span><span class="sxs-lookup"><span data-stu-id="b394e-113">Access tenant databases simple a Java console application</span></span>
> * <span data-ttu-id="b394e-114">Odstranění klienta</span><span class="sxs-lookup"><span data-stu-id="b394e-114">Delete a tenant</span></span>

<span data-ttu-id="b394e-115">Pokud nemáte předplatné Azure, [vytvořit bezplatný účet](https://azure.microsoft.com/free/) před zahájením.</span><span class="sxs-lookup"><span data-stu-id="b394e-115">If you don't have an Azure subscription, [create a free account](https://azure.microsoft.com/free/) before you begin.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b394e-116">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b394e-116">Prerequisites</span></span>

<span data-ttu-id="b394e-117">toocomplete tento kurz, ověřte zda máte:</span><span class="sxs-lookup"><span data-stu-id="b394e-117">toocomplete this tutorial, make sure you have:</span></span>

* <span data-ttu-id="b394e-118">Hello nainstalovanou nejnovější verzi prostředí PowerShell a hello [nejnovější sadu Azure PowerShell SDK](http://azure.microsoft.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="b394e-118">Installed hello newest version of PowerShell and hello [latest Azure PowerShell SDK](http://azure.microsoft.com/downloads/)</span></span>

* <span data-ttu-id="b394e-119">Nejnovější verze nainstalované hello [SQL Server Management Studio](http://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).</span><span class="sxs-lookup"><span data-stu-id="b394e-119">Installed hello latest version of [SQL Server Management Studio](http://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).</span></span> <span data-ttu-id="b394e-120">Instalaci SQL Server Management Studio se nainstaluje také hello nejnovější verzi SQLPackage, nástroje příkazového řádku, které můžou být použité tooautomate řadu úloh vývoj databáze.</span><span class="sxs-lookup"><span data-stu-id="b394e-120">Installing SQL Server Management Studio also installs hello latest version of SQLPackage, a command-line utility that can be used tooautomate a range of database development tasks.</span></span>

* <span data-ttu-id="b394e-121">Nainstalované hello [Java Runtime Environment (JRE) 8](http://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html) a hello [nejnovější JAVA Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) nainstalovaný na počítači.</span><span class="sxs-lookup"><span data-stu-id="b394e-121">Installed hello [Java Runtime Environment (JRE) 8](http://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html) and hello [latest JAVA Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) installed on your machine.</span></span> 

* <span data-ttu-id="b394e-122">Nainstalovat [Apache Maven](https://maven.apache.org/download.cgi).</span><span class="sxs-lookup"><span data-stu-id="b394e-122">Installed [Apache Maven](https://maven.apache.org/download.cgi).</span></span> <span data-ttu-id="b394e-123">Použije se maven toohelp Správa závislostí, vytváření, testování a spuštění projektu Java ukázka hello</span><span class="sxs-lookup"><span data-stu-id="b394e-123">Maven will be used toohelp manage dependencies, build, test and run hello sample Java project</span></span>

## <a name="set-up-data-environment"></a><span data-ttu-id="b394e-124">Nastavení prostředí pro data</span><span class="sxs-lookup"><span data-stu-id="b394e-124">Set up data environment</span></span>

<span data-ttu-id="b394e-125">Bude zřizování databáze za klienta.</span><span class="sxs-lookup"><span data-stu-id="b394e-125">You will be provisioning a database per tenant.</span></span> <span data-ttu-id="b394e-126">model databáze za klienta Hello poskytuje nejvyšší úroveň izolace mezi klienty, s malé náklady na DevOps hello.</span><span class="sxs-lookup"><span data-stu-id="b394e-126">hello database-per-tenant model provides hello highest degree of isolation between tenants, with little DevOps cost.</span></span> <span data-ttu-id="b394e-127">toooptimize hello nákladů prostředků cloudu, můžete se také zřizovat hello klienta databází do pružného fondu, což vám umožní toooptimize hello ceny výkonu pro skupinu databází.</span><span class="sxs-lookup"><span data-stu-id="b394e-127">toooptimize hello cost of cloud resources, you will also be provisioning hello tenant databases into an elastic pool which allows you toooptimize hello price performance for a group of databases.</span></span> <span data-ttu-id="b394e-128">toolearn o jiné databázi zřizování modely [zde](sql-database-design-patterns-multi-tenancy-saas-applications.md#multi-tenant-data-models).</span><span class="sxs-lookup"><span data-stu-id="b394e-128">toolearn about other database provisioning models [see here](sql-database-design-patterns-multi-tenancy-saas-applications.md#multi-tenant-data-models).</span></span>

<span data-ttu-id="b394e-129">Postupujte podle těchto kroků toocreate systému SQL server a fondu elastické databáze, který bude hostitelem všechny databáze klienta.</span><span class="sxs-lookup"><span data-stu-id="b394e-129">Follow these steps toocreate a SQL server and an elastic pool that will host all your tenant databases.</span></span> 

1. <span data-ttu-id="b394e-130">Vytváření proměnných toostore hodnoty, které se použijí v hello zbytek hello kurzu.</span><span class="sxs-lookup"><span data-stu-id="b394e-130">Create variables toostore values that will be used in hello rest of hello tutorial.</span></span> <span data-ttu-id="b394e-131">Ujistěte se, že toomodify hello IP adresu proměnné tooinclude IP adresa</span><span class="sxs-lookup"><span data-stu-id="b394e-131">Make sure toomodify hello IP address variable tooinclude your IP address</span></span> 
   
   ```PowerShell 
   # Set an admin login and password for your database
   $adminlogin = "ServerAdmin"
   $password = "ChangeYourAdminPassword1"
   
   # Create random unique names for logical server and tenants
   $servername = "server-$(Get-Random)"
   $tenant1 = "geolamice"
   $tenant2 = "ranplex"
   
   # Store current client IP address (modify tooinclude your IP address)
   $startIpAddress = 0.0.0.0 
   $endIpAddress = 0.0.0.0
   ```
   
2. <span data-ttu-id="b394e-132">Přihlášení tooAzure a vytvořte SQL server a elastického fondu</span><span class="sxs-lookup"><span data-stu-id="b394e-132">Login tooAzure and create a SQL server and elastic pool</span></span> 
   
   ```PowerShell
   # Login tooAzure 
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
   
## <a name="create-tenant-catalog"></a><span data-ttu-id="b394e-133">Vytvořit katalog klienta</span><span class="sxs-lookup"><span data-stu-id="b394e-133">Create tenant catalog</span></span> 

<span data-ttu-id="b394e-134">V aplikaci SaaS více klientů je důležité tooknow se uloží informace pro klienta.</span><span class="sxs-lookup"><span data-stu-id="b394e-134">In a multi-tenant SaaS application, it’s important tooknow where information for a tenant is stored.</span></span> <span data-ttu-id="b394e-135">Běžně je uložen v databázi katalogu.</span><span class="sxs-lookup"><span data-stu-id="b394e-135">This is commonly stored in a catalog database.</span></span> <span data-ttu-id="b394e-136">databáze katalogu Hello je použité toohold mapování mezi klientem a databáze, ve kterém je uložený dat daného klienta.</span><span class="sxs-lookup"><span data-stu-id="b394e-136">hello catalog database is used toohold a mapping between a tenant and a database in which that tenant’s data is stored.</span></span>  <span data-ttu-id="b394e-137">Základní vzor Hello platí jak pro více klientů nebo databázi jednoho klienta se používá.</span><span class="sxs-lookup"><span data-stu-id="b394e-137">hello basic pattern applies whether a multi-tenant or a single-tenant database is used.</span></span>

<span data-ttu-id="b394e-138">Postupujte podle těchto kroků toocreate databáze katalogu pro aplikace SaaS ukázka hello.</span><span class="sxs-lookup"><span data-stu-id="b394e-138">Follow these steps toocreate a catalog database for hello sample SaaS application.</span></span>

```PowerShell
# Create empty database in pool
New-AzureRmSqlDatabase  -ResourceGroupName "myResourceGroup" `
    -ServerName $servername `
    -DatabaseName "tenantCatalog" `
    -ElasticPoolName "myElasticPool"

# Create table tootrack mapping between tenants and their databases
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

## <a name="provision-database-for-tenant1-and-register-in-tenant-catalog"></a><span data-ttu-id="b394e-139">Zřídit databázi pro 'tenant1' a zaregistrovat v katalogu klienta</span><span class="sxs-lookup"><span data-stu-id="b394e-139">Provision database for 'tenant1' and register in tenant catalog</span></span> 
<span data-ttu-id="b394e-140">Pomocí prostředí Powershell tooprovision databáze pro nového klienta, tenant1' a registraci tohoto klienta v katalogu hello.</span><span class="sxs-lookup"><span data-stu-id="b394e-140">Use Powershell tooprovision a database for a new tenant 'tenant1' and register this tenant in hello catalog.</span></span> 

```PowerShell
# Create empty database in pool for 'tenant1'
New-AzureRmSqlDatabase  -ResourceGroupName "myResourceGroup" `
    -ServerName $servername `
    -DatabaseName $tenant1 `
    -ElasticPoolName "myElasticPool"

# Create table WhoAmI and insert tenant name into hello table 
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

# Register 'tenant1' in hello tenant catalog 
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

## <a name="provision-database-for-tenant2-and-register-in-tenant-catalog"></a><span data-ttu-id="b394e-141">Zřídit databázi pro 'tenant2' a zaregistrovat v katalogu klienta</span><span class="sxs-lookup"><span data-stu-id="b394e-141">Provision database for 'tenant2' and register in tenant catalog</span></span>
<span data-ttu-id="b394e-142">Pomocí prostředí Powershell tooprovision databáze pro nového klienta, tenant2' a registraci tohoto klienta v katalogu hello.</span><span class="sxs-lookup"><span data-stu-id="b394e-142">Use Powershell tooprovision a database for a new tenant 'tenant2' and register this tenant in hello catalog.</span></span> 

```PowerShell
# Create empty database in pool for 'tenant2'
New-AzureRmSqlDatabase  -ResourceGroupName "myResourceGroup" `
    -ServerName $servername `
    -DatabaseName $tenant2 `
    -ElasticPoolName "myElasticPool"

# Create table WhoAmI and insert tenant name into hello table 
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

# Register tenant 'tenant2' in hello tenant catalog 
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

## <a name="set-up-sample-java-application"></a><span data-ttu-id="b394e-143">Nastavte ukázkovou aplikaci Java</span><span class="sxs-lookup"><span data-stu-id="b394e-143">Set up sample Java application</span></span> 

1. <span data-ttu-id="b394e-144">Vytvořte projekt maven.</span><span class="sxs-lookup"><span data-stu-id="b394e-144">Create a maven project.</span></span> <span data-ttu-id="b394e-145">V okně příkazového řádku zadejte následující hello:</span><span class="sxs-lookup"><span data-stu-id="b394e-145">Type hello following in a command prompt window:</span></span>
   
   ```
   mvn archetype:generate -DgroupId=com.microsoft.sqlserver -DartifactId=mssql-jdbc -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
   ```
   
2. <span data-ttu-id="b394e-146">Přidejte tuto závislost, úroveň jazyka a sestavení možnost toosupport manifestu souborů v souboru pom.xml toohello JAR:</span><span class="sxs-lookup"><span data-stu-id="b394e-146">Add this dependency, language level, and build option toosupport manifest files in jars toohello pom.xml file:</span></span>
   
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

3. <span data-ttu-id="b394e-147">Přidejte následující hello do soubor App.java hello:</span><span class="sxs-lookup"><span data-stu-id="b394e-147">Add hello following into hello App.java file:</span></span>

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
   
   System.out.println(" " + CMD_QUIT + " - quit hello application\n");
   
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
   
   // List all tenants that currently exist in hello system
   
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
   
   // Query hello data that was previously inserted into hello primary database from hello geo replicated database
   
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

4. <span data-ttu-id="b394e-148">Uložte soubor hello.</span><span class="sxs-lookup"><span data-stu-id="b394e-148">Save hello file.</span></span>

5. <span data-ttu-id="b394e-149">Přejděte toocommand konzoly a provést</span><span class="sxs-lookup"><span data-stu-id="b394e-149">Go toocommand console and execute</span></span>

   ```bash
   mvn package
   ```

6. <span data-ttu-id="b394e-150">Po dokončení spuštění následující aplikace hello toorun hello</span><span class="sxs-lookup"><span data-stu-id="b394e-150">When finished, execute hello following toorun hello application</span></span> 
   
   ```
   mvn -q -e exec:java "-Dexec.mainClass=com.sqldbsamples.App"
   ```
   
<span data-ttu-id="b394e-151">Pokud úspěšně proběhne, bude výstup Hello vypadat například takto:</span><span class="sxs-lookup"><span data-stu-id="b394e-151">hello output will look like this if it runs successfully:</span></span>

```
############################

## SAAS DATABASE TUTORIAL ##

############################

OPTIONS

LIST - list tenants

QUERY <NAME> - connect and tenant query tenant <NAME>

QUIT - quit hello application

* List hello tenants

* Query tenants you created
```

## <a name="delete-first-tenant"></a><span data-ttu-id="b394e-152">Odstranit první klienta</span><span class="sxs-lookup"><span data-stu-id="b394e-152">Delete first tenant</span></span> 
<span data-ttu-id="b394e-153">Pomocí prostředí PowerShell toodelete hello klienta databáze a katalog položky pro prvního klienta hello.</span><span class="sxs-lookup"><span data-stu-id="b394e-153">Use PowerShell toodelete hello tenant database and catalog entry for hello first tenant.</span></span>

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

<span data-ttu-id="b394e-154">Pokuste se připojit pomocí 'tenant1' příliš hello aplikace v jazyce Java.</span><span class="sxs-lookup"><span data-stu-id="b394e-154">Try connecting too'tenant1' using hello Java application.</span></span> <span data-ttu-id="b394e-155">Zobrazí se chyba oznamující, že tento klient hello neexistuje.</span><span class="sxs-lookup"><span data-stu-id="b394e-155">You will get an error stating that hello tenant does not exist.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b394e-156">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b394e-156">Next steps</span></span> 

<span data-ttu-id="b394e-157">V tomto kurzu jste se dozvěděli na:</span><span class="sxs-lookup"><span data-stu-id="b394e-157">In this tutorial, you learned to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="b394e-158">Nastavení databáze prostředí toosupport víceklientské aplikace SaaS, pomocí vzoru databáze za klienta hello</span><span class="sxs-lookup"><span data-stu-id="b394e-158">Set up a database environment toosupport a multi-tenant SaaS application, using hello Database-per-tenant pattern</span></span>
> * <span data-ttu-id="b394e-159">Vytvoření klienta katalogu</span><span class="sxs-lookup"><span data-stu-id="b394e-159">Create a tenant catalog</span></span>
> * <span data-ttu-id="b394e-160">Zřídit klienta databázi a její registrace v katalogu klienta hello</span><span class="sxs-lookup"><span data-stu-id="b394e-160">Provision a tenant database and register it in hello tenant catalog</span></span>
> * <span data-ttu-id="b394e-161">Nastavení ukázkové aplikace Java</span><span class="sxs-lookup"><span data-stu-id="b394e-161">Set up a sample Java application</span></span> 
> * <span data-ttu-id="b394e-162">Přístup k klienta databází jednoduchou konzolovou aplikaci Java</span><span class="sxs-lookup"><span data-stu-id="b394e-162">Access tenant databases simple a Java console application</span></span>
> * <span data-ttu-id="b394e-163">Odstranění klienta</span><span class="sxs-lookup"><span data-stu-id="b394e-163">Delete a tenant</span></span>

* <span data-ttu-id="b394e-164">Ukázky pro běžné úkoly, PowerShell najdete v části [ukázky PowerShell databáze SQL](https://docs.microsoft.com/azure/sql-database/sql-database-powershell-samples)</span><span class="sxs-lookup"><span data-stu-id="b394e-164">PowerShell samples for common tasks, see [SQL Database PowerShell samples](https://docs.microsoft.com/azure/sql-database/sql-database-powershell-samples)</span></span>

* <span data-ttu-id="b394e-165">Vzory víceklientské SaaS aplikací najdete v tématu návrhu [vzory návrhu](https://docs.microsoft.com/azure/sql-database/sql-database-design-patterns-multi-tenancy-saas-applications)</span><span class="sxs-lookup"><span data-stu-id="b394e-165">Design patterns for multi-tenant SaaS applications see [Design patterns](https://docs.microsoft.com/azure/sql-database/sql-database-design-patterns-multi-tenancy-saas-applications)</span></span>

* <span data-ttu-id="b394e-166">Ukázky jazyka Java pro běžné úlohy, Azure, najdete v části [středisko pro vývojáře Java](https://azure.microsoft.com/develop/java/)</span><span class="sxs-lookup"><span data-stu-id="b394e-166">Java samples for common Azure tasks, see [Java Developer Center](https://azure.microsoft.com/develop/java/)</span></span>



