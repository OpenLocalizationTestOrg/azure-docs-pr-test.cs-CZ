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
# <a name="implement-a-geo-distributed-database"></a>Implementace geograficky distribuovaná databáze

V tomto kurzu konfigurace Azure SQL database a aplikace pro vzdálené oblast tooa převzetí služeb při selhání a proveďte test převzetí služeb při selhání plánu. Získáte informace o těchto tématech: 

> [!div class="checklist"]
> * Vytvoření databáze uživatelů a udělit mu oprávnění
> * Nastavit pravidlo brány firewall na úrovni databáze
> * Vytvoření [geografická replikace převzetí služeb při selhání skupiny](sql-database-geo-replication-overview.md)
> * Vytvoření a kompilace tooquery aplikace Java Azure SQL database
> * Provedení postupu zotavení po havárii

Pokud nemáte předplatné Azure, [vytvořit bezplatný účet](https://azure.microsoft.com/free/) před zahájením.


## <a name="prerequisites"></a>Požadavky

toocomplete dokončení tohoto kurzu, ujistěte se, hello následující požadavky:

- Nejnovější nainstalované hello [prostředí Azure PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs). 
- Nainstalovat Azure SQL database. Tento kurz používá ukázkové databáze AdventureWorksLT hello s názvem **mySampleDatabase** z jednoho z těchto rychlé spuštění:

   - [Vytvoření databáze – portál](sql-database-get-started-portal.md)
   - [Vytvoření databáze – rozhraní příkazového řádku](sql-database-get-started-cli.md)
   - [Vytvoření databáze – PowerShell](sql-database-get-started-powershell.md)

- Našli metoda tooexecute SQL skripty proti databázi, můžete použít jednu z následujících nástrojů dotazů hello:
   - editor dotazů Hello v hello [portál Azure](https://portal.azure.com). Další informace o použití editoru dotazů hello v hello portálu Azure najdete v tématu [připojit a zadávat dotazy pomocí editoru dotazů](sql-database-get-started-portal.md#query-the-sql-database).
   - nejnovější verze Hello [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms), což je integrované prostředí pro správu jakékoliv infrastruktury, SQL, z tooSQL systému SQL Server databáze pro Microsoft Windows.
   - nejnovější verze Hello [Visual Studio Code](https://code.visualstudio.com/docs), což je editor grafické kódu pro Linux, systému macOS, a Windows, které podporuje rozšíření, včetně hello [mssql rozšíření](https://aka.ms/mssql-marketplace) k dotazování systému Microsoft SQL Server , Azure SQL Database a SQL Data Warehouse. Další informace o použití tohoto nástroje s Azure SQL Database, najdete v části [připojit a zadávat dotazy s VS Code](sql-database-connect-query-vscode.md). 

## <a name="create-database-users-and-grant-permissions"></a>Vytvoření databáze uživatelů a udělit oprávnění

Připojit tooyour databáze a vytvořit uživatelské účty pomocí jedné z následujících nástrojů dotazů hello:

- editor dotazů Hello v hello portálu Azure
- SQL Server Management Studio
- Visual Studio Code

Tyto uživatelské účty automaticky replikovat tooyour sekundární server (a sesynchronizovávat). toouse SQL Server Management Studio nebo Visual Studio Code, může být nutné tooconfigure pravidlo brány firewall při připojení z klienta na adresu IP, pro kterou jste zatím nenakonfigurovali bránou firewall. Podrobné pokyny najdete v tématu [vytvoření pravidla brány firewall na úrovni serveru](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).

- V okně dotazu spusťte následující dotaz toocreate dva uživatelské účty ve vaší databázi hello. Tento skript uděluje **db_owner** oprávnění toohello **app_admin** účet a uděluje **vyberte** a **aktualizace** toohello oprávnění **app_user** účtu. 

   ```sql
   CREATE USER app_admin WITH PASSWORD = 'ChangeYourPassword1';
   --Add SQL user toodb_owner role
   ALTER ROLE db_owner ADD MEMBER app_admin; 
   --Create additional SQL user
   CREATE USER app_user WITH PASSWORD = 'ChangeYourPassword1';
   --grant permission tooSalesLT schema
   GRANT SELECT, INSERT, DELETE, UPDATE ON SalesLT.Product tooapp_user;
   ```

## <a name="create-database-level-firewall"></a>Vytvoření brány firewall na úrovni databáze

Vytvoření [pravidlo brány firewall na úrovni databáze](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database) pro vaši databázi SQL. Toto pravidlo brány firewall na úrovni databáze replikuje automaticky toohello sekundární server, který vytvoříte v tomto kurzu. Pro zjednodušení (v tomto kurzu) použijte hello veřejnou IP adresu hello počítače, na kterém provádíte hello kroky v tomto kurzu. toodetermine hello IP adresa použitá pro hello pravidlo brány firewall na úrovni serveru pro váš aktuální počítač, najdete v části [vytvoření brány firewall na úrovni serveru](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).  

- V okně otevřeného dotazu nahraďte předchozího dotazu hello hello následující dotaz, nahraďte hello IP adresy hello odpovídající IP adresy pro vaše prostředí.  

   ```sql
   -- Create database-level firewall setting for your public IP address
   EXECUTE sp_set_database_firewall_rule @name = N'myGeoReplicationFirewallRule',@start_ip_address = '0.0.0.0', @end_ip_address = '0.0.0.0';
   ```

## <a name="create-an-active-geo-replication-auto-failover-group"></a>Vytvořit skupinu aktivní geografickou replikací automatické převzetí služeb při selhání 

Pomocí Azure PowerShell, vytvořte [aktivní geografickou replikací automatické převzetí služeb při selhání skupiny](sql-database-geo-replication-overview.md) mezi existující server Azure SQL a hello nový prázdný server Azure SQL v oblasti Azure, a poté přidejte skupině ukázkové databáze toohello převzetí služeb při selhání.

> [!IMPORTANT]
> Tyto rutiny vyžadují Azure PowerShell 4.0. [!INCLUDE [sample-powershell-install](../../includes/sample-powershell-install-no-ssh.md)]
>

1. Naplnění proměnných pro skripty prostředí PowerShell pomocí hello hodnot pro existující server a ukázkovou databázi a zadejte globálně jedinečná hodnota pro název skupiny pro převzetí služeb při selhání.

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

2. Vytvoření prázdného zálohování serveru ve vaší oblasti převzetí služeb při selhání.

   ```powershell
   $mydrserver = New-AzureRmSqlServer -ResourceGroupName $myresourcegroupname `
      -ServerName $mydrservername `
      -Location $mydrlocation `
      -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $adminlogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))
   $mydrserver   
   ```

3. Vytvořte skupinu převzetí služeb při selhání mezi dvěma servery hello.

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

4. Přidáte skupinu převzetí služeb při selhání toohello vaší databáze.

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

## <a name="install-java-software"></a>Instalace softwaru Java

Hello kroky v této části Předpokládejme, že jsou obeznámeni s vývojem pomocí Java a jsou nové tooworking s Azure SQL Database. 

### <a name="mac-os"></a>**Mac OS**
Otevřete terminálu a přejděte tooa adresáře, kde plánujete vytvoření projektu Java. Nainstalujte **brew** a **Maven** zadáním hello následující příkazy: 

```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew update
brew install maven
```

Obsahuje podrobné pokyny k instalaci a konfiguraci prostředí Java a Maven, přejděte hello [sestavení aplikace pomocí systému SQL Server](https://www.microsoft.com/sql-server/developer-get-started/), vyberte **Java**, vyberte **systému MacOS**a pak postupujte podle Hello podrobné pokyny ke konfiguraci Java a Maven v krok 1.2 a 1.3.

### <a name="linux-ubuntu"></a>**Linux (Ubuntu)**
Otevřete terminálu a přejděte tooa adresáře, kde plánujete vytvoření projektu Java. Nainstalujte **Maven** zadáním hello následující příkazy:

```bash
sudo apt-get install maven
```

Obsahuje podrobné pokyny k instalaci a konfiguraci prostředí Java a Maven, přejděte hello [sestavení aplikace pomocí systému SQL Server](https://www.microsoft.com/sql-server/developer-get-started/), vyberte **Java**, vyberte **Ubuntu**a pak postupujte podle Hello podrobné pokyny ke konfiguraci Java a Maven v kroku 1.4, 1.2 a 1.3.

### <a name="windows"></a>**Windows**
Nainstalujte [Maven](https://maven.apache.org/download.cgi) pomocí instalačního programu oficiální hello. Používání Maven toohelp Správa závislostí, vytvoření, testování a spuštění projektu jazyka Java. Obsahuje podrobné pokyny k instalaci a konfiguraci prostředí Java a Maven, přejděte hello [sestavení aplikace pomocí systému SQL Server](https://www.microsoft.com/sql-server/developer-get-started/), vyberte **Java**vyberte Windows a potom postupujte podle hello podrobné pokyny pro Konfigurace Java a Maven na krok 1.2 a 1.3.

## <a name="create-sqldbsample-project"></a>Vytvoření projektu SqlDbSample

1. V konzole příkaz hello (například Bash) vytvořte projekt Maven. 
   ```bash
   mvn archetype:generate "-DgroupId=com.sqldbsamples" "-DartifactId=SqlDbSample" "-DarchetypeArtifactId=maven-archetype-quickstart" "-Dversion=1.0.0"
   ```
2. Typ **Y** a klikněte na tlačítko **Enter**.
3. Změňte adresáře na nově vytvořený projekt.

   ```bash
   cd SqlDbSamples
   ```

4. Ve složce projektu pomocí vašeho oblíbeného editoru otevřete soubor pom.xml hello. 

5. Přidejte hello ovladač JDBC Microsoft pro projekt Maven tooyour závislost SQL Server tak, že otevřete svém oblíbeném textovém editoru a kopírování a vkládání hello následující řádky do souboru pom.xml. Nepřepisovat existující hodnoty hello naplněnými v souboru hello. Hello JDBC závislostí musí vložení v rámci (hello větší "závislosti" části).   

   ```xml
   <dependency>
     <groupId>com.microsoft.sqlserver</groupId>
     <artifactId>mssql-jdbc</artifactId>
    <version>6.1.0.jre8</version>
   </dependency>
   ```

6. Zadejte verzi hello Java toocompile hello projektu před přidáním hello následující části "vlastnosti" do souboru pom.xml hello po části "závislosti" hello. 

   ```xml
   <properties>
     <maven.compiler.source>1.8</maven.compiler.source>
     <maven.compiler.target>1.8</maven.compiler.target>
   </properties>
   ```
7. Přidejte následující hello "sestavení" části do souboru pom.xml hello po hello "vlastnosti" části toosupport souborů manifestu v JAR.       

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
8. Uložte a zavřete soubor pom.xml hello.
9. Otevřete soubor App.java hello (C:\apache-maven-3.5.0\SqlDbSample\src\main\java\com\sqldbsamples\App.java) a nahraďte obsah hello hello následující obsah. Nahraďte název skupiny pro převzetí služeb při selhání hello hello název pro skupinu pro převzetí služeb při selhání. Pokud jste změnili hello hodnoty pro název databáze hello, uživatele nebo heslo, změna také tyto hodnoty.

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
6. Uložte a zavřete soubor App.java hello.

## <a name="compile-and-run-hello-sqldbsample-project"></a>Zkompilování a spuštění projektu SqlDbSample hello

1. V konzole příkaz hello spusťte příkaz toofollowing.

   ```bash
   mvn package
   ```
2. Po dokončení, spusťte následující příkaz toorun hello aplikace (spuštění o jednu hodinu Pokud zastavíte ručně) hello:

   ```bash
   mvn -q -e exec:java "-Dexec.mainClass=com.sqldbsamples.App"
   
   #######################################
   ## GEO DISTRIBUTED DATABASE TUTORIAL ##
   #######################################

   1. insert on primary successful, read from secondary successful
   2. insert on primary successful, read from secondary successful
   3. insert on primary successful, read from secondary successful
   ```

## <a name="perform-disaster-recovery-drill"></a>Provedení postupu zotavení po havárii

1. Volání ruční převzetí služeb při selhání skupiny převzetí služeb při selhání. 

   ```powershell
   Switch-AzureRMSqlDatabaseFailoverGroup `
   -ResourceGroupName $myresourcegroupname  `
   -ServerName $mydrservername `
   -FailoverGroupName $myfailovergroupname
   ```

2. Pozorovat výsledky. aplikace hello během převzetí služeb při selhání. Některé vloží selžou aktualizuje hello mezipaměť DNS.     

3. Zjistěte, jaké role provádí vašeho serveru pro obnovení po havárii.

   ```powershell
   $mydrserver.ReplicationRole
   ```

4. Navrácení služeb po obnovení.

   ```powershell
   Switch-AzureRMSqlDatabaseFailoverGroup `
   -ResourceGroupName $myresourcegroupname  `
   -ServerName $myservername `
   -FailoverGroupName $myfailovergroupname
   ```

5. Pozorovat výsledky. aplikace hello během navrácení služeb po obnovení. Některé vloží selžou aktualizuje hello mezipaměť DNS.     

6. Zjistěte, jaké role provádí vašeho serveru pro obnovení po havárii.

   ```powershell
   $fileovergroup = Get-AzureRMSqlDatabaseFailoverGroup `
      -FailoverGroupName $myfailovergroupname `
      -ResourceGroupName $myresourcegroupname `
      -ServerName $mydrservername
   $fileovergroup.ReplicationRole
   ```
## <a name="next-steps"></a>Další kroky 

Další informace najdete v tématu [aktivní geografickou replikaci a převzetí služeb při selhání skupiny](sql-database-geo-replication-overview.md).
