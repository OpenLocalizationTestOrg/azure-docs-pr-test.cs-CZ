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
# <a name="implement-a-multi-tenant-saas-application-using-azure-sql-database"></a>Implementace víceklientské aplikace SaaS používání Azure SQL Database

Hello stejné nastavení služby toohundreds nebo tisíce klienty, kteří sdílet nebo vidět uživatele toho druhého data, která poskytuje víceklientské aplikace je aplikace hostované v cloudovém prostředí. Příkladem je aplikace SaaS, která poskytuje služby tootenants v prostředí hostovaných v cloudu. Tento model izoluje hello dat pro každého klienta a optimalizuje distribučního hello prostředků pro náklady. 

Tento kurz ukazuje, jak toocreate aplikaci SaaS víceklientské používání Azure SQL Database.

V tomto kurzu se naučíte:
> [!div class="checklist"]
> * Nastavení databáze prostředí toosupport víceklientské aplikace SaaS, pomocí vzoru databáze za klienta hello
> * Vytvoření klienta katalogu
> * Zřídit klienta databázi a její registrace v katalogu klienta hello
> * Nastavení ukázkové aplikace Java 
> * Přístup k klienta databází jednoduchou konzolovou aplikaci Java
> * Odstranění klienta

Pokud nemáte předplatné Azure, [vytvořit bezplatný účet](https://azure.microsoft.com/free/) před zahájením.

## <a name="prerequisites"></a>Požadavky

toocomplete tento kurz, ověřte zda máte:

* Hello nainstalovanou nejnovější verzi prostředí PowerShell a hello [nejnovější sadu Azure PowerShell SDK](http://azure.microsoft.com/downloads/)

* Nejnovější verze nainstalované hello [SQL Server Management Studio](http://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms). Instalaci SQL Server Management Studio se nainstaluje také hello nejnovější verzi SQLPackage, nástroje příkazového řádku, které můžou být použité tooautomate řadu úloh vývoj databáze.

* Nainstalované hello [Java Runtime Environment (JRE) 8](http://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html) a hello [nejnovější JAVA Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) nainstalovaný na počítači. 

* Nainstalovat [Apache Maven](https://maven.apache.org/download.cgi). Použije se maven toohelp Správa závislostí, vytváření, testování a spuštění projektu Java ukázka hello

## <a name="set-up-data-environment"></a>Nastavení prostředí pro data

Bude zřizování databáze za klienta. model databáze za klienta Hello poskytuje nejvyšší úroveň izolace mezi klienty, s malé náklady na DevOps hello. toooptimize hello nákladů prostředků cloudu, můžete se také zřizovat hello klienta databází do pružného fondu, což vám umožní toooptimize hello ceny výkonu pro skupinu databází. toolearn o jiné databázi zřizování modely [zde](sql-database-design-patterns-multi-tenancy-saas-applications.md#multi-tenant-data-models).

Postupujte podle těchto kroků toocreate systému SQL server a fondu elastické databáze, který bude hostitelem všechny databáze klienta. 

1. Vytváření proměnných toostore hodnoty, které se použijí v hello zbytek hello kurzu. Ujistěte se, že toomodify hello IP adresu proměnné tooinclude IP adresa 
   
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
   
2. Přihlášení tooAzure a vytvořte SQL server a elastického fondu 
   
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
   
## <a name="create-tenant-catalog"></a>Vytvořit katalog klienta 

V aplikaci SaaS více klientů je důležité tooknow se uloží informace pro klienta. Běžně je uložen v databázi katalogu. databáze katalogu Hello je použité toohold mapování mezi klientem a databáze, ve kterém je uložený dat daného klienta.  Základní vzor Hello platí jak pro více klientů nebo databázi jednoho klienta se používá.

Postupujte podle těchto kroků toocreate databáze katalogu pro aplikace SaaS ukázka hello.

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

## <a name="provision-database-for-tenant1-and-register-in-tenant-catalog"></a>Zřídit databázi pro 'tenant1' a zaregistrovat v katalogu klienta 
Pomocí prostředí Powershell tooprovision databáze pro nového klienta, tenant1' a registraci tohoto klienta v katalogu hello. 

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

## <a name="provision-database-for-tenant2-and-register-in-tenant-catalog"></a>Zřídit databázi pro 'tenant2' a zaregistrovat v katalogu klienta
Pomocí prostředí Powershell tooprovision databáze pro nového klienta, tenant2' a registraci tohoto klienta v katalogu hello. 

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

## <a name="set-up-sample-java-application"></a>Nastavte ukázkovou aplikaci Java 

1. Vytvořte projekt maven. V okně příkazového řádku zadejte následující hello:
   
   ```
   mvn archetype:generate -DgroupId=com.microsoft.sqlserver -DartifactId=mssql-jdbc -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
   ```
   
2. Přidejte tuto závislost, úroveň jazyka a sestavení možnost toosupport manifestu souborů v souboru pom.xml toohello JAR:
   
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

3. Přidejte následující hello do soubor App.java hello:

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

4. Uložte soubor hello.

5. Přejděte toocommand konzoly a provést

   ```bash
   mvn package
   ```

6. Po dokončení spuštění následující aplikace hello toorun hello 
   
   ```
   mvn -q -e exec:java "-Dexec.mainClass=com.sqldbsamples.App"
   ```
   
Pokud úspěšně proběhne, bude výstup Hello vypadat například takto:

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

## <a name="delete-first-tenant"></a>Odstranit první klienta 
Pomocí prostředí PowerShell toodelete hello klienta databáze a katalog položky pro prvního klienta hello.

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

Pokuste se připojit pomocí 'tenant1' příliš hello aplikace v jazyce Java. Zobrazí se chyba oznamující, že tento klient hello neexistuje.

## <a name="next-steps"></a>Další kroky 

V tomto kurzu jste se dozvěděli na:
> [!div class="checklist"]
> * Nastavení databáze prostředí toosupport víceklientské aplikace SaaS, pomocí vzoru databáze za klienta hello
> * Vytvoření klienta katalogu
> * Zřídit klienta databázi a její registrace v katalogu klienta hello
> * Nastavení ukázkové aplikace Java 
> * Přístup k klienta databází jednoduchou konzolovou aplikaci Java
> * Odstranění klienta

* Ukázky pro běžné úkoly, PowerShell najdete v části [ukázky PowerShell databáze SQL](https://docs.microsoft.com/azure/sql-database/sql-database-powershell-samples)

* Vzory víceklientské SaaS aplikací najdete v tématu návrhu [vzory návrhu](https://docs.microsoft.com/azure/sql-database/sql-database-design-patterns-multi-tenancy-saas-applications)

* Ukázky jazyka Java pro běžné úlohy, Azure, najdete v části [středisko pro vývojáře Java](https://azure.microsoft.com/develop/java/)



