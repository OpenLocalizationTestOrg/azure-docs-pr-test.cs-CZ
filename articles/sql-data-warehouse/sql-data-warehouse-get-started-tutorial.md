---
title: "Začínáme aaaAzure SQL Data Warehouse – kurz | Microsoft Docs"
description: "V tomto kurzu se naučíte, jak tooprovision a načtení dat do Azure SQL Data Warehouse. Taky poznáte hello základní informace o škálování, pozastavení a ladění."
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: johnmac
editor: barbkess
ms.assetid: 52DFC191-E094-4B04-893F-B64D5828A900
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: quickstart
ms.date: 01/26/2017
ms.author: elbutter;barbkess
ms.openlocfilehash: edd2a21b0fe49ca8e9792c7c512310339a822c55
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-sql-data-warehouse"></a>Začínáme s SQL Data Warehouse

Tento kurz ukazuje, jak tooprovision a načtení dat do Azure SQL Data Warehouse. Taky poznáte hello základní informace o škálování, pozastavení a ladění. Jakmile budete hotovi, budete mít připravené tooquery a prozkoumat datového skladu.

**Odhadovaný čas toocomplete:** Toto je začátku do konce kurz s ukázkový kód, který trvá asi 30 minut toocomplete po jste splnili požadavky hello. 

## <a name="prerequisites"></a>Požadavky

Hello kurz předpokládá, že se seznámíte se základními koncepty SQL Data Warehouse. Pokud potřebujete úvodní informace, přečtěte si téma [Co je SQL Data Warehouse?](sql-data-warehouse-overview-what-is.md) 

### <a name="sign-up-for-microsoft-azure"></a>Přihlášení k Microsoft Azure
Pokud nemáte účet Microsoft Azure, musíte toosign pro jeden toouse této služby. Pokud již máte účet, tento krok přeskočte. 

1. Procházení stránek účet toohello [https://azure.microsoft.com/account/](https://azure.microsoft.com/account/)
2. Vytvořte si bezplatný účet Azure, nebo si účet zakupte.
3. Postupujte podle pokynů hello

### <a name="install-appropriate-sql-client-drivers-and-tools"></a>Instalace odpovídajících ovladačů a nástrojů klienta SQL

Většina nástroje SQL client tools připojit tooSQL datového skladu pomocí JDBC, ODBC nebo ADO.NET. Z důvodu toohello velký počet funkcí T-SQL, které podporuje SQL Data Warehouse nejsou některé klientské aplikace plně kompatibilní s datovým skladem SQL.

Pokud používáte operační systém Windows, doporučujeme použít buď sadu [Visual Studio], nebo aplikaci [SQL Server Management Studio].

[!INCLUDE [Create a new logical server](../../includes/sql-data-warehouse-create-logical-server.md)] 

[!INCLUDE [SQL Database create server](../../includes/sql-database-create-new-server-firewall-portal.md)]

## <a name="create-a-sql-data-warehouse"></a>Vytvoření SQL Data Warehouse

SQL Data Warehouse je zvláštním typem databáze, která je navržena pro výkonné paralelní zpracování. Hello databáze je rozdělené mezi více uzly a zpracovává dotazy paralelně. SQL Data Warehouse má řídicí uzel, které orchestrují hello aktivity všech uzlů hello. Hello v samotných uzlech použít SQL Database toomanage data.  

> [!NOTE]
> Vytvoření služby SQL Data Warehouse může znamenat, že se vám začne fakturovat nová služba.  Další informace najdete v tématu [SQL Data Warehouse – ceny](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).
>

### <a name="create-a-data-warehouse"></a>Vytvoření datového skladu

1. Přihlaste se k hello [portál Azure](https://portal.azure.com).
2. Klikněte na **Nový** > **Databáze** > **SQL Data Warehouse**.

    ![NewBlade](../../includes/media/sql-data-warehouse-create-dw/blade-click-new.png)![SelectDW](../../includes/media/sql-data-warehouse-create-dw/blade-select-dw.png)

3. Zadání podrobností o nasazení

    **Název databáze:** Výběr je zcela na vás. Pokud máte více datových skladů, doporučujeme, aby názvy zahrne podrobnosti, jako je například hello oblast, prostředí, například *mydw-westus-1-test*.

    **Předplatné:** Vaše předplatné Azure

    **Skupina prostředků:** Vytvořte skupinu prostředků nebo vyberte existující.
    > [!NOTE]
    > Skupiny prostředků jsou užitečné pro správu prostředků, jako je například určování rozsahu řízení přístupu a nasazení podle šablony. Další informace o skupinách prostředků Azure a osvědčených postupech si přečtete [zde](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups).

    **Zdroj:** Prázdná databáze

    **Server**: Vyberte hello serveru, které jste vytvořili v [požadavky].

    **Kolace**: ponechte hello výchozí kolace SQL_Latin1_General_CP1_CI_AS.

    **Vyberte výkonu**: doporučujeme začít s standardní 400DWU hello.

4. Zvolte **Pin toodashboard** ![tooDashboard PIN kód](./media/sql-data-warehouse-get-started-tutorial/pin-to-dashboard.png)

5. Sledujte a počkat na vaše data warehouse toodeploy! Je běžné, tento proces tootake několik minut. portál Hello vás upozorní, když váš datový sklad je připraven toouse. 

## <a name="connect-toosql-data-warehouse"></a>Připojit tooSQL datového skladu

Tento kurz používá SQL Server Management Studio (SSMS) tooconnect toohello datového skladu. TooSQL datového skladu můžete připojit přes tyto podporované konektory: ADO.NET, JDBC, rozhraní ODBC a PHP. Mějte na paměti, že funkce nástrojů, které nejsou podporované Microsoftem, můžou být omezené.


### <a name="get-connection-information"></a>Získání informací o připojení

tooconnect tooyour datového skladu, je nutné tooconnect prostřednictvím hello logický SQL server vytvoříte v [požadavky].

1. Vyberte datový sklad z hello řídicího panelu nebo vyhledejte ji ve vašich prostředků.

    ![Řídicí panel SQL Data Warehouse](./media/sql-data-warehouse-get-started-tutorial/sql-dw-dashboard.png)

2. Najít hello úplný název pro hello logický SQL server.

    ![Výběr názvu serveru](./media/sql-data-warehouse-get-started-tutorial/select-server.png)

3. Otevřete aplikaci SSMS a použití objektu explorer tooconnect toothis serveru pomocí pověření správce serveru hello jste vytvořili v [požadavky]

    ![Připojení přes SSMS](./media/sql-data-warehouse-get-started-tutorial/ssms-connect.png)

Pokud všechno proběhne správně, můžete nyní měli být připojené tooyour logický SQL server. Vzhledem k tomu, že jste se přihlásili jako hello správce serveru, se můžete připojit databázi tooany hostovaný hello serverem, včetně hello hlavní databázi. 

Je účet správce pouze jeden server a většina oprávnění uživatele, který má hello. Dávejte pozor, není tooallow příliš mnoho uživatelů v organizaci tooknow hello správce heslo. 

Můžete mít také účet správce Azure Active Directory. Poskytujeme není zde podrobnosti hello. Pokud chcete, aby toolearn Další informace o používání ověřování Azure Active Directory, přečtěte si téma [ověřování Azure AD](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication).

Dále se budeme věnovat vytváření dalších přihlašovacích údajů a uživatelů.


## <a name="create-a-database-user"></a>Vytvoření uživatele databáze

V tomto kroku vytvoříte tooaccess účet uživatele datového skladu. Můžeme také ukazují, jak toogive tohoto uživatele hello možnost toorun dotazy s velké množství prostředky paměti a procesoru.

### <a name="notes-about-resource-classes-for-allocating-resources-tooqueries"></a>Poznámky k třídy prostředků pro přidělování prostředků tooqueries

- tookeep data bezpečné, nepoužívejte hello serveru správce toorun dotazy na provozní databáze. Většina oprávnění uživatele, který má hello a použití tooperform operací na uživatelských datech vloží vaše data v ohrožení. Navíc vzhledem k tomu, že Dobrý den, správce serveru je určen tooperform operace správy, je spuštěna operace s jenom malé přidělení prostředky paměti a procesoru. 

- SQL Data Warehouse používá předem definovaných databázových rolí, jako třídy prostředků, tooallocate různých množství paměti, prostředky procesoru a toousers sloty souběžnosti. Každý uživatel může patřit třída tooa malé, střední, velký nebo velmi velká prostředků. Hello Třída prostředků uživatele určuje hello prostředky hello uživatel má toorun dotazů a načte operace.

- Pro optimální data kompresi pravděpodobně bude třeba hello uživatele tooload s velké nebo přidělení velmi velké zdroje. Další informace o třídách prostředků najdete [zde](./sql-data-warehouse-develop-concurrency.md#resource-classes):

### <a name="create-an-account-that-can-control-a-database"></a>Vytvoření účtu, který může řídit databázi

Vzhledem k tomu, že jste aktuálně přihlášeni v hello správce serveru nemáte oprávnění toocreate přihlášení a uživatele.

1. Pomocí SSMS nebo jiného klienta dotazů otevřete nový dotaz pro **master**.

    ![Nový dotaz na Master](./media/sql-data-warehouse-get-started-tutorial/query-on-server.png)

    ![Nový dotaz na Master1](./media/sql-data-warehouse-get-started-tutorial/query-on-master.png)

2. V okně dotazu hello spusťte tento příkaz toocreate T-SQL s názvem MedRCLogin přihlášení a uživatele s názvem LoadingUser. Toto přihlášení můžete připojit toohello logický SQL server.

    ```sql
    CREATE LOGIN MedRCLogin WITH PASSWORD = 'a123reallySTRONGpassword!';
    CREATE USER LoadingUser FOR LOGIN MedRCLogin;
    ```

3. Nyní dotazování hello *databázi SQL Data Warehouse*, vytvořte uživatele databáze založené na hello přihlášení, které jste vytvořili tooaccess a provádění operací v databázi hello.

    ```sql
    CREATE USER LoadingUser FOR LOGIN MedRCLogin;
    ```

4. Dejte hello uživatele řízení oprávnění toohello databáze názvem NYT. 

    ```sql
    GRANT CONTROL ON DATABASE::[NYT] tooLoadingUser;
    ```
    > [!NOTE]
    > Název databáze je pomlčky v něm, nebude se toowrap ji do hranatých závorek! 
    >

### <a name="give-hello-user-medium-resource-allocations"></a>Poskytnout přidělení střední zdroje hello uživatele

1. Spusťte tento příkaz toomake T-SQL a členem třídy hello střední prostředků, která se nazývá mediumrc it. 

    ```sql
    EXEC sp_addrolemember 'mediumrc', 'LoadingUser';
    ```
    > [!NOTE]
    > Klikněte na tlačítko [sem](sql-data-warehouse-develop-concurrency.md#resource-classes) toolearn více informací o souběžnosti a prostředků třídy! 
    >

2. Připojit toohello logického serveru s novými pověřeními hello

    ![Přihlášení pomocí nových přihlašovacích údajů](./media/sql-data-warehouse-get-started-tutorial/new-login.png)


## <a name="load-data-from-azure-blob-storage"></a>Načtení dat z Azure Blob Storage

Nyní je připraven tooload data do datového skladu. Tento krok ukazuje, jak tooload New Yorku taxíkem souboru cab dat z veřejného úložiště Azure blob. 

- Běžný způsob, jakým tooload dat do SQL Data Warehouse je toofirst přesunout úložiště objektů blob tooAzure hello data a pak můžete načíst do datového skladu. toomake je snazší toounderstand jak tooload, máme New Yorku taxíkem souboru cab dat již hostované ve veřejné úložiště Azure blob. 

- Pro budoucí použití, toolearn jak tooget tooAzure vaše data objektu blob úložiště nebo tooload ji přímo ze zdroje do SQL Data Warehouse, najdete v části hello [přehledem načítání](sql-data-warehouse-overview-load.md).


### <a name="define-external-data"></a>Definování externích dat

1. Vytvořte hlavní klíč. Potřebujete jenom toocreate hlavní klíč jednou za databáze. 

    ```sql
    CREATE MASTER KEY;
    ```

2. Zadejte umístění hello hello Azure blob, který obsahuje data souboru cab taxíkem hello.  

    ```sql
    CREATE EXTERNAL DATA SOURCE NYTPublic
    WITH
    (
        TYPE = Hadoop,
        LOCATION = 'wasbs://2013@nytpublic.blob.core.windows.net/'
    );
    ```

3. Zadejte externí formátů hello

    Hello ```CREATE EXTERNAL FILE FORMAT``` příkaz je použité toospecify formát soubory, které obsahují externích dat hello. Obsahují text oddělený jedním nebo několika znaky, kterým se říká oddělovače. Pro demonstrační účely hello taxíkem souboru cab data uložena jako nekomprimovaných dat a jako gzip komprimována data.

    Spuštění těchto příkazů T-SQL toodefine ve dvou různých formátech: nekomprimovaným a komprimované.

    ```sql
    CREATE EXTERNAL FILE FORMAT uncompressedcsv
    WITH (
        FORMAT_TYPE = DELIMITEDTEXT,
        FORMAT_OPTIONS ( 
            FIELD_TERMINATOR = ',',
            STRING_DELIMITER = '',
            DATE_FORMAT = '',
            USE_TYPE_DEFAULT = False
        )
    );

    CREATE EXTERNAL FILE FORMAT compressedcsv
    WITH ( 
        FORMAT_TYPE = DELIMITEDTEXT,
        FORMAT_OPTIONS ( FIELD_TERMINATOR = '|',
            STRING_DELIMITER = '',
        DATE_FORMAT = '',
            USE_TYPE_DEFAULT = False
        ),
        DATA_COMPRESSION = 'org.apache.hadoop.io.compress.GzipCodec'
    );
    ```

4.  Vytvořte schéma pro váš formát externích souborů. 

    ```sql
    CREATE SCHEMA ext;
    ```
5. Vytvořte hello externí tabulky. Tyto tabulky odkazují na data uložená v Azure Blob Storage. Několik externí tabulky, že všechny bodu toohello objektů blob v Azure definovaného dříve v našem externí zdroj dat spuštění hello následující toocreate příkazů T-SQL.

```sql
    CREATE EXTERNAL TABLE [ext].[Date] 
    (
        [DateID] int NOT NULL,
        [Date] datetime NULL,
        [DateBKey] char(10) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayOfMonth] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DaySuffix] varchar(4) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayName] varchar(9) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayOfWeek] char(1) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayOfWeekInMonth] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayOfWeekInYear] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayOfQuarter] varchar(3) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayOfYear] varchar(3) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [WeekOfMonth] varchar(1) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [WeekOfQuarter] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [WeekOfYear] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [Month] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [MonthName] varchar(9) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [MonthOfQuarter] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [Quarter] char(1) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [QuarterName] varchar(9) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [Year] char(4) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [YearName] char(7) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [MonthYear] char(10) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [MMYYYY] char(6) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [FirstDayOfMonth] date NULL,
        [LastDayOfMonth] date NULL,
        [FirstDayOfQuarter] date NULL,
        [LastDayOfQuarter] date NULL,
        [FirstDayOfYear] date NULL,
        [LastDayOfYear] date NULL,
        [IsHolidayUSA] bit NULL,
        [IsWeekday] bit NULL,
        [HolidayUSA] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL
    )
    WITH
    (
        LOCATION = 'Date',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = uncompressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0
    );
    
    CREATE EXTERNAL TABLE [ext].[Geography]
    (
        [GeographyID] int NOT NULL,
        [ZipCodeBKey] varchar(10) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL,
        [County] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [City] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [State] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [Country] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [ZipCode] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL
    )
    WITH
    (
        LOCATION = 'Geography',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = uncompressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0 
    );
        
    
    CREATE EXTERNAL TABLE [ext].[HackneyLicense]
    (
        [HackneyLicenseID] int NOT NULL,
        [HackneyLicenseBKey] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL,
        [HackneyLicenseCode] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL
    )
    WITH
    (
        LOCATION = 'HackneyLicense',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = uncompressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0
    )
    ;
        
    
    CREATE EXTERNAL TABLE [ext].[Medallion]
    (
        [MedallionID] int NOT NULL,
        [MedallionBKey] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL,
        [MedallionCode] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL
    )
    WITH
    (
        LOCATION = 'Medallion',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = uncompressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0
    )
    ;
        
    CREATE EXTERNAL TABLE [ext].[Time]
    (
        [TimeID] int NOT NULL,
        [TimeBKey] varchar(8) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL,
        [HourNumber] tinyint NOT NULL,
        [MinuteNumber] tinyint NOT NULL,
        [SecondNumber] tinyint NOT NULL,
        [TimeInSecond] int NOT NULL,
        [HourlyBucket] varchar(15) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL,
        [DayTimeBucketGroupKey] int NOT NULL,
        [DayTimeBucket] varchar(100) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL
    )
    WITH
    (
        LOCATION = 'Time',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = uncompressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0
    )
    ;
    
    
    CREATE EXTERNAL TABLE [ext].[Trip]
    (
        [DateID] int NOT NULL,
        [MedallionID] int NOT NULL,
        [HackneyLicenseID] int NOT NULL,
        [PickupTimeID] int NOT NULL,
        [DropoffTimeID] int NOT NULL,
        [PickupGeographyID] int NULL,
        [DropoffGeographyID] int NULL,
        [PickupLatitude] float NULL,
        [PickupLongitude] float NULL,
        [PickupLatLong] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DropoffLatitude] float NULL,
        [DropoffLongitude] float NULL,
        [DropoffLatLong] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [PassengerCount] int NULL,
        [TripDurationSeconds] int NULL,
        [TripDistanceMiles] float NULL,
        [PaymentType] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [FareAmount] money NULL,
        [SurchargeAmount] money NULL,
        [TaxAmount] money NULL,
        [TipAmount] money NULL,
        [TollsAmount] money NULL,
        [TotalAmount] money NULL
    )
    WITH
    (
        LOCATION = 'Trip2013',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = compressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0
    )
    ;
    
    CREATE EXTERNAL TABLE [ext].[Weather]
    (
        [DateID] int NOT NULL,
        [GeographyID] int NOT NULL,
        [PrecipitationInches] float NOT NULL,
        [AvgTemperatureFahrenheit] float NOT NULL
    )
    WITH
    (
        LOCATION = 'Weather2013',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = uncompressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0
    )
    ;
```

### <a name="import-hello-data-from-azure-blob-storage"></a>Umožňuje importovat hello data z Azure blob storage.

SQL Data Warehouse podporuje klíčový příkaz CREATE TABLE AS SELECT (CTAS). Tento příkaz vytvoří novou tabulku na základě výsledků hello příkazu select. Hello nová tabulka obsahuje hello stejnou sloupce a datové typy jako hello výsledky hello vyberte příkaz.  Jedná se způsob tooimport data z Azure blob storage do SQL Data Warehouse.

1. Spusťte tento skript tooimport vaše data.

    ```sql
    CREATE TABLE [dbo].[Date]
    WITH
    ( 
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS SELECT * FROM [ext].[Date]
    OPTION (LABEL = 'CTAS : Load [dbo].[Date]')
    ;
    
    CREATE TABLE [dbo].[Geography]
    WITH
    ( 
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS
    SELECT * FROM [ext].[Geography]
    OPTION (LABEL = 'CTAS : Load [dbo].[Geography]')
    ;
    
    CREATE TABLE [dbo].[HackneyLicense]
    WITH
    ( 
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS SELECT * FROM [ext].[HackneyLicense]
    OPTION (LABEL = 'CTAS : Load [dbo].[HackneyLicense]')
    ;
    
    CREATE TABLE [dbo].[Medallion]
    WITH
    (
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS SELECT * FROM [ext].[Medallion]
    OPTION (LABEL = 'CTAS : Load [dbo].[Medallion]')
    ;
    
    CREATE TABLE [dbo].[Time]
    WITH
    (
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS SELECT * FROM [ext].[Time]
    OPTION (LABEL = 'CTAS : Load [dbo].[Time]')
    ;
    
    CREATE TABLE [dbo].[Weather]
    WITH
    ( 
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS SELECT * FROM [ext].[Weather]
    OPTION (LABEL = 'CTAS : Load [dbo].[Weather]')
    ;
    
    CREATE TABLE [dbo].[Trip]
    WITH
    (
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS SELECT * FROM [ext].[Trip]
    OPTION (LABEL = 'CTAS : Load [dbo].[Trip]')
    ;
    ```

2. Zobrazte data během načítání.

   Provedete načtení několika GB dat a jejich kompresi do vysoce výkonných clusterovaných indexů columnstore. Spusťte následující dotaz, který používá zobrazení (zobrazení dynamické správy) dynamické správy tooshow hello stav zatížení hello hello. Po spuštění dotazu hello, získat, kávy a piknik při SQL Data Warehouse nepodporuje některé lifting náročné.
    
    ```sql
    SELECT
        r.command,
        s.request_id,
        r.status,
        count(distinct input_name) as nbr_files,
        sum(s.bytes_processed)/1024/1024/1024 as gb_processed
    FROM 
        sys.dm_pdw_exec_requests r
        INNER JOIN sys.dm_pdw_dms_external_work s
        ON r.request_id = s.request_id
    WHERE
        r.[label] = 'CTAS : Load [dbo].[Date]' OR
        r.[label] = 'CTAS : Load [dbo].[Geography]' OR
        r.[label] = 'CTAS : Load [dbo].[HackneyLicense]' OR
        r.[label] = 'CTAS : Load [dbo].[Medallion]' OR
        r.[label] = 'CTAS : Load [dbo].[Time]' OR
        r.[label] = 'CTAS : Load [dbo].[Weather]' OR
        r.[label] = 'CTAS : Load [dbo].[Trip]'
    GROUP BY
        r.command,
        s.request_id,
        r.status
    ORDER BY
        nbr_files desc, 
        gb_processed desc;
    ```

3. Zobrazte všechny systémové dotazy.

    ```sql
    SELECT * FROM sys.dm_pdw_exec_requests;
    ```

4. Užívejte si pohled na to, jak se data do Azure SQL Data Warehouse krásně načítají.

    ![Zobrazení načtených data](./media/sql-data-warehouse-get-started-tutorial/see-data-loaded.png)


## <a name="improve-query-performance"></a>Vylepšení výkonu dotazů

Existuje několik způsobů tooimprove dotazu výkonu a hello tooachieve vysokorychlostní se výkonu, který datový sklad SQL je určená tooprovide.  

### <a name="see-hello-effect-of-scaling-on-query-performance"></a>V tématu účinku hello škálování na výkon dotazů 

Jedním ze způsobů tooimprove výkon dotazů je tooscale prostředky tak, že změníte úroveň služby hello DWU pro datový sklad. Každá úroveň služeb je nákladnější, kdykoli však můžete škálovat směrem dolů nebo pozastavit prostředky. 

V tomto kroku porovnáte výkon pro dvě různá nastavení DWU.

První, nyní škálování hello velikosti dolů too100 DWU, takže jsme můžete získat představu o tom, jak jednom výpočetním uzlu může provádět svoje vlastní.

1. Přejděte toohello portál a vyberte svoji službu SQL Data Warehouse.

2. V okně SQL Data Warehouse hello vyberte škálování. 

    ![Škálování DW z portálu](./media/sql-data-warehouse-get-started-tutorial/scale-dw.png)

3. Snižovat výkon hello panelu too100 DWU a stiskněte tlačítko Uložit.

    ![Škálování a uložení](./media/sql-data-warehouse-get-started-tutorial/scale-and-save.png)

4. Počkejte, než pro vaše toofinish operace škálování.

    > [!NOTE]
    > Dotazy nelze spustit při změně měřítka hello. Škálování **ukončí** aktuálně spuštěné dotazy. Můžete je restartovat po dokončení operace hello.
    >
    
5. Proveďte operaci prohledávání na služební cestě data hello, výběr hello nejvyšší mil. položky pro všechny sloupce hello. Pokud jste přes toomove na rychle, zaregistrované volné tooselect méně řádků. Poznamenejte si dobu hello trvá toorun tuto operaci.

    ```sql
    SELECT TOP(1000000) * FROM dbo.[Trip]
    ```
6. Škálovat datový sklad zpět too400 DWU. Pamatujte si, že se každý 100 DWU je přidání jiného výpočetní uzel tooyour Azure SQL Data Warehouse.

7. Spusťte znovu dotaz hello! Měli byste zaznamenat značný rozdíl. 

    > [!NOTE]
    > Protože hello dotaz vrací velké množství dat, může být dostupnosti šířky pásma hello hello počítače spuštěného SSMS kritických bodů výkonu. Důsledkem může být, že neuvidíte žádná zlepšení výkonu!

> [!NOTE]
> Služba SQL Data Warehouse je postavena na architektuře MPP (Massively Parallel Processing). Dotazy, které kontroly ani provést u miliony řádků analytické funkce prostředí hello true power služby Azure SQL Data Warehouse.
>

### <a name="see-hello-effect-of-statistics-on-query-performance"></a>Na výkon dotazů najdete v části hello účinku statistiky

1. Spusťte dotaz, zda spojení hello datum tabulku s tabulkou cestě hello

    ```sql
    SELECT TOP (1000000) 
        dt.[DayOfWeek],
        tr.[MedallionID],
        tr.[HackneyLicenseID],
        tr.[PickupTimeID],
        tr.[DropoffTimeID],
        tr.[PickupGeographyID],
        tr.[DropoffGeographyID],
        tr.[PickupLatitude],
        tr.[PickupLongitude],
        tr.[PickupLatLong],
        tr.[DropoffLatitude],
        tr.[DropoffLongitude],
        tr.[DropoffLatLong],
        tr.[PassengerCount],
        tr.[TripDurationSeconds],
        tr.[TripDistanceMiles],
        tr.[PaymentType],
        tr.[FareAmount],
        tr.[SurchargeAmount],
        tr.[TaxAmount],
        tr.[TipAmount],
        tr.[TollsAmount],
        tr.[TotalAmount]
    FROM [dbo].[Trip] as tr
        JOIN dbo.[Date] as dt
        ON  tr.DateID = dt.DateID
    ```

    Tento dotaz zpracování chvíli trvá, protože SQL Data Warehouse má tooshuffle dat, než je možné provést hello spojení. Spojení nemáte tooshuffle data, pokud jsou data navrženou toojoin v hello stejným způsobem, jako je distribuován. To je na hlubší diskuzi. 

2. Na statistikách záleží. 
3. Spusťte tento příkaz toocreate statistiky na sloupce spojení hello.

    ```sql
    CREATE STATISTICS [dbo.Date DateID stats] ON dbo.Date (DateID);
    CREATE STATISTICS [dbo.Trip DateID stats] ON dbo.Trip (DateID);
    ```

    > [!NOTE]
    > SQL DW nespravuje statistiky automaticky. Statistiky jsou důležité pro výkon dotazů a důrazně se doporučuje statistiky vytvářet a aktualizovat.
    > 
    > **Hello získáte tak, že statistiky na sloupce použité ve spojení, sloupce použité v hello, kde najít klauzule a sloupce v GROUP BY využívat výhod.**
    >

3. Znovu spustit dotaz hello z požadovaných součástí a sledovat případné rozdíly výkonu. Při hello rozdíly ve výkonnosti dotazu nebude jako závažný jako vertikálním navýšení kapacity, měli byste zaznamenat zrychlit. 

## <a name="next-steps"></a>Další kroky

Teď už připravený tooquery a prozkoumat. Vyzkoušejte si naše osvědčené postupy a tipy.

Pokud jste hotovi zkoumat hello den, ujistěte se, že toopause instanci! V produkčním prostředí, můžete zaznamenat značné úspory pozastavení a škálování toomeet vašim obchodním potřebám.

![Pozastavení](./media/sql-data-warehouse-get-started-tutorial/pause.png)

## <a name="useful-readings"></a>Užitečné informace k přečtení

[Souběžnost a správa úloh][]

[Osvědčené postupy pro službu Azure SQL Data Warehouse][]

[Monitorování dotazů][]

[10 nejlepších osvědčených postupů pro sestavení rozsáhlého relačního datového skladu][]

[Migrace dat tooAzure SQL Data Warehouse][]

[Souběžnost a správa úloh]: sql-data-warehouse-develop-concurrency.md#changing-user-resource-class-example
[Osvědčené postupy pro službu Azure SQL Data Warehouse]: sql-data-warehouse-best-practices.md#hash-distribute-large-tables
[Monitorování dotazů]: sql-data-warehouse-manage-monitor.md
[10 nejlepších osvědčených postupů pro sestavení rozsáhlého relačního datového skladu]: https://blogs.msdn.microsoft.com/sqlcat/2013/09/16/top-10-best-practices-for-building-a-large-scale-relational-data-warehouse/
[Migrace dat tooAzure SQL Data Warehouse]: https://blogs.msdn.microsoft.com/sqlcat/2016/08/18/migrating-data-to-azure-sql-data-warehouse-in-practice/



[!INCLUDE [Additional Resources](../../includes/sql-data-warehouse-article-footer.md)]

<!-- Internal Links -->
[požadavky]: sql-data-warehouse-get-started-tutorial.md#prerequisites

<!--Other Web references-->
[Visual Studio]: https://www.visualstudio.com/
[SQL Server Management Studio]: https://msdn.microsoft.com/en-us/library/mt238290.aspx
