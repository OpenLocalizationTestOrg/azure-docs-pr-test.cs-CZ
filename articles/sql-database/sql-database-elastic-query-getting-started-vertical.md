---
title: "aaaGet začít s dotazy mezidatabázové (vertikální dělení) | Microsoft Docs"
description: "jak toouse elastické databáze dotaz s svisle na oddíly databáze"
services: sql-database
documentationcenter: 
manager: jhubbard
author: torsteng
ms.assetid: e5b44b10-c432-4f96-b20e-08615ff4d5dd
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2016
ms.author: torsteng
ms.openlocfilehash: 9e6183268e8bf87e3ac28f502711fcc05a7a3f52
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-cross-database-queries-vertical-partitioning-preview"></a>Začínáme s dotazy mezidatabázové (vertikální dělení) (preview)
Dotaz elastické databáze (preview) pro databázi SQL Azure vám umožní toorun dotazů T-SQL, které jsou rozmístěny v několika databází pomocí jednoho připojení bodu. Toto téma se týká příliš[svisle na oddíly databáze](sql-database-elastic-query-vertical-partitioning.md).  

Po dokončení bude: Zjistěte, jak tooconfigure a používání Azure SQL Database tooperform dotazuje tohoto rozpětí více související databází. 

Další informace o funkci dotazu hello elastické databáze, najdete v tématu [přehled dotazu elastické databáze Azure SQL Database](sql-database-elastic-query-overview.md). 

## <a name="prerequisites"></a>Požadavky

Musíte mít oprávnění ALTER ANY EXTERNAL DATA SOURCE. Toto oprávnění je součástí hello oprávnění ALTER DATABASE. Oprávnění ALTER ANY externí zdroj dat se zdroji dat toohello potřebné toorefer.

## <a name="create-hello-sample-databases"></a>Vytvoření hello ukázkové databáze
toostart s potřebujeme toocreate dvě databáze, **zákazníci** a **objednávky**, buď v hello stejný nebo jiný logické servery.   

Provést následující dotazy na hello hello **objednávky** databáze toocreate hello **OrderInformation** tabulky a vstup hello ukázková data. 

    CREATE TABLE [dbo].[OrderInformation]( 
        [OrderID] [int] NOT NULL, 
        [CustomerID] [int] NOT NULL 
        ) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (123, 1) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (149, 2) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (857, 2) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (321, 1) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (564, 8) 

Nyní, spustit následující dotaz na hello **zákazníci** databáze toocreate hello **CustomerInformation** tabulky a vstup hello ukázková data. 

    CREATE TABLE [dbo].[CustomerInformation]( 
        [CustomerID] [int] NOT NULL, 
        [CustomerName] [varchar](50) NULL, 
        [Company] [varchar](50) NULL 
        CONSTRAINT [CustID] PRIMARY KEY CLUSTERED ([CustomerID] ASC) 
    ) 
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (1, 'Jack', 'ABC') 
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (2, 'Steve', 'XYZ') 
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (3, 'Lylla', 'MNO') 

## <a name="create-database-objects"></a>Vytvoření databázových objektů
### <a name="database-scoped-master-key-and-credentials"></a>Hlavní klíč a přihlašovací údaje na obor definovaný databází
1. Spusťte aplikaci SQL Server Management Studio nebo SQL Server Data Tools v sadě Visual Studio.
2. Připojení databáze objednávky toohello a spusťte následující příkazy T-SQL hello:
   
        CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<password>'; 
        CREATE DATABASE SCOPED CREDENTIAL ElasticDBQueryCred 
        WITH IDENTITY = '<username>', 
        SECRET = '<password>';  
   
    Hello "username" a "password" by se měly hello uživatelské jméno a heslo použít toologin do databáze hello zákazníků.
    Ověřování pomocí služby Azure Active Directory s elastické dotazy není aktuálně podporován.

### <a name="external-data-sources"></a>Externích zdrojů dat.
toocreate externího zdroje dat, spusťte následující příkaz v databázi objednávky hello hello: 

    CREATE EXTERNAL DATA SOURCE MyElasticDBQueryDataSrc WITH 
        (TYPE = RDBMS, 
        LOCATION = '<server_name>.database.windows.net', 
        DATABASE_NAME = 'Customers', 
        CREDENTIAL = ElasticDBQueryCred, 
    ) ;

### <a name="external-tables"></a>Externí tabulky
Vytvoření externí tabulky v databázi hello objednávky, který by odpovídal hello Definice tabulky CustomerInformation hello:

    CREATE EXTERNAL TABLE [dbo].[CustomerInformation] 
    ( [CustomerID] [int] NOT NULL, 
      [CustomerName] [varchar](50) NOT NULL, 
      [Company] [varchar](50) NOT NULL) 
    WITH 
    ( DATA_SOURCE = MyElasticDBQueryDataSrc) 

## <a name="execute-a-sample-elastic-database-t-sql-query"></a>Spuštění ukázkového dotazu T-SQL elastické databáze
Jakmile definujete zdroj externích dat a vaše externí tabulky můžete nyní používat T-SQL tooquery externí tabulky. Spusťte tento dotaz na databázi objednávky hello: 

    SELECT OrderInformation.CustomerID, OrderInformation.OrderId, CustomerInformation.CustomerName, CustomerInformation.Company 
    FROM OrderInformation 
    INNER JOIN CustomerInformation 
    ON CustomerInformation.CustomerID = OrderInformation.CustomerID 

## <a name="cost"></a>Náklady
V současné době funkce dotazu hello elastické databáze je zahrnut do hello náklady na vaší databázi SQL Azure.  

Informace o cenách najdete v části [SQL Database – ceny](https://azure.microsoft.com/pricing/details/sql-database). 

## <a name="next-steps"></a>Další kroky

* Přehled elastické dotazů najdete v tématu [elastické dotazu přehled](sql-database-elastic-query-overview.md).
* Syntaxe a ukázkové dotazy pro svisle oddílů data, najdete v části [dotazování svisle na oddíly dat)](sql-database-elastic-query-vertical-partitioning.md)
* Podívejte se kurz vodorovné rozdělení do oddílů (horizontálního dělení) [Začínáme s elastické dotazu pro vodorovné rozdělení do oddílů (horizontálního dělení)](sql-database-elastic-query-getting-started.md).
* Syntaxe a ukázkové dotazy pro horizontálně dělenou data, najdete v části [dotazování vodorovně rozdělena na oddíly dat)](sql-database-elastic-query-horizontal-partitioning.md)
* V tématu [sp\_provést \_vzdáleného](https://msdn.microsoft.com/library/mt703714) pro uloženou proceduru, který spouští příkaz jazyka Transact-SQL na jedné vzdálené databáze SQL Azure nebo sadu databází, které slouží jako horizontálních oddílů v vodorovné schéma rozdělení oddílů.