---
title: "Připojení ke službě Azure SQL Data Warehouse pomocí sqlcmd | Dokumentace Microsoftu"
description: "Pomocí nástroje příkazového řádku [sqlcmd][sqlcmd] se můžete připojit a dotazovat službu Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: antvgski
manager: jhubbard
editor: 
ms.assetid: 6e2b69e5-4806-4e91-9ea1-e2b63bf28c46
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: connect
ms.date: 10/31/2016
ms.author: anvang;barbkess
ms.openlocfilehash: 5a3fe1046c3417070ba8ff5bd18a0485e2152eff
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="connect-to-sql-data-warehouse-with-sqlcmd"></a><span data-ttu-id="bea80-103">Připojení k SQL Data Warehouse pomocí sqlcmd</span><span class="sxs-lookup"><span data-stu-id="bea80-103">Connect to SQL Data Warehouse with sqlcmd</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="bea80-104">Power BI</span><span class="sxs-lookup"><span data-stu-id="bea80-104">Power BI</span></span>](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [<span data-ttu-id="bea80-105">Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="bea80-105">Azure Machine Learning</span></span>](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [<span data-ttu-id="bea80-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bea80-106">Visual Studio</span></span>](sql-data-warehouse-query-visual-studio.md)
> * [<span data-ttu-id="bea80-107">sqlcmd</span><span class="sxs-lookup"><span data-stu-id="bea80-107">sqlcmd</span></span>](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> * [<span data-ttu-id="bea80-108">SSMS</span><span class="sxs-lookup"><span data-stu-id="bea80-108">SSMS</span></span>](sql-data-warehouse-query-ssms.md)
> 
> 

<span data-ttu-id="bea80-109">Pomocí nástroje příkazového řádku [sqlcmd][sqlcmd] se můžete připojit a dotazovat službu Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="bea80-109">Use [sqlcmd][sqlcmd] command-line utility to connect to and query an Azure SQL Data Warehouse.</span></span>  

## <a name="1-connect"></a><span data-ttu-id="bea80-110">1. Připojení</span><span class="sxs-lookup"><span data-stu-id="bea80-110">1. Connect</span></span>
<span data-ttu-id="bea80-111">Chcete-li začít s nástrojem [sqlcmd][sqlcmd], otevřete příkazový řádek a zadejte příkaz **sqlcmd** následovaný připojovacím řetězcem pro vaši databázi SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="bea80-111">To get started with [sqlcmd][sqlcmd], open the command prompt and enter **sqlcmd** followed by the connection string for your SQL Data Warehouse database.</span></span> <span data-ttu-id="bea80-112">Připojovací řetězec bude muset mít následující parametry:</span><span class="sxs-lookup"><span data-stu-id="bea80-112">The connection string requires the following parameters:</span></span>

* <span data-ttu-id="bea80-113">**Server (-S):** Server v následující podobě: `<`název serveru`>`.database.windows.net</span><span class="sxs-lookup"><span data-stu-id="bea80-113">**Server (-S):** Server in the form `<`Server Name`>`.database.windows.net</span></span>
* <span data-ttu-id="bea80-114">**Database (-d):** Název databáze</span><span class="sxs-lookup"><span data-stu-id="bea80-114">**Database (-d):** Database name.</span></span>
* <span data-ttu-id="bea80-115">**Enable Quoted Identifiers (-I):** Aby bylo možné se připojit k instanci služby SQL Data Warehouse, musí být povolené identifikátory v uvozovkách.</span><span class="sxs-lookup"><span data-stu-id="bea80-115">**Enable Quoted Identifiers (-I):** Quoted identifiers must be enabled to connect to a SQL Data Warehouse instance.</span></span>

<span data-ttu-id="bea80-116">Chcete-li používat ověřování systému SQL Server, je třeba přidat parametry uživatelského jména a hesla:</span><span class="sxs-lookup"><span data-stu-id="bea80-116">To use SQL Server Authentication, you need to add the username/password parameters:</span></span>

* <span data-ttu-id="bea80-117">**User (-U):** Uživatel serveru v následující podobě: `<`Uživatel`>`</span><span class="sxs-lookup"><span data-stu-id="bea80-117">**User (-U):** Server user in the form `<`User`>`</span></span>
* <span data-ttu-id="bea80-118">**Password (-P):** Heslo přidružené k uživateli</span><span class="sxs-lookup"><span data-stu-id="bea80-118">**Password (-P):** Password associated with the user.</span></span>

<span data-ttu-id="bea80-119">Připojovací řetězec může například vypadat následovně:</span><span class="sxs-lookup"><span data-stu-id="bea80-119">For example, your connection string might look like the following:</span></span>

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
```

<span data-ttu-id="bea80-120">Chcete-li používat integrované ověřování v Azure Active Directory, je třeba přidat parametry Azure Active Directory:</span><span class="sxs-lookup"><span data-stu-id="bea80-120">To use Azure Active Directory Integrated authentication, you need to add the Azure Active Directory parameters:</span></span>

* <span data-ttu-id="bea80-121">**Azure Active Directory Authentication (-G):** Pro ověřování používat Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="bea80-121">**Azure Active Directory Authentication (-G):** use Azure Active Directory for authentication</span></span>

<span data-ttu-id="bea80-122">Připojovací řetězec může například vypadat následovně:</span><span class="sxs-lookup"><span data-stu-id="bea80-122">For example, your connection string might look like the following:</span></span>

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -G -I
```

> [!NOTE]
> <span data-ttu-id="bea80-123">Abyste mohli provádět ověřování pomocí Active Directory, je třeba [povolit ověřování Azure Active Directory](sql-data-warehouse-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="bea80-123">You need to [enable Azure Active Directory Authentication](sql-data-warehouse-authentication.md) to authenticate using Active Directory.</span></span>
> 
> 

## <a name="2-query"></a><span data-ttu-id="bea80-124">2. Dotaz</span><span class="sxs-lookup"><span data-stu-id="bea80-124">2. Query</span></span>
<span data-ttu-id="bea80-125">Po připojení můžete pro instanci zadávat všechny podporované příkazy jazyka Transact-SQL.</span><span class="sxs-lookup"><span data-stu-id="bea80-125">After connection, you can issue any supported Transact-SQL statements against the instance.</span></span>  <span data-ttu-id="bea80-126">V tomto příkladu jsou dotazy zadávány v interaktivním režimu.</span><span class="sxs-lookup"><span data-stu-id="bea80-126">In this example, queries are submitted in interactive mode.</span></span>

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
1> SELECT name FROM sys.tables;
2> GO
3> QUIT
```

<span data-ttu-id="bea80-127">Tyto další příklady ukazují, jak lze vaše dotazy spouštět v dávkovém režimu pomocí parametru -Q nebo vedení serveru SQL k příkazu sqlcmd.</span><span class="sxs-lookup"><span data-stu-id="bea80-127">These next examples show how you can run your queries in batch mode using the -Q option or piping your SQL to sqlcmd.</span></span>

```sql
sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I -Q "SELECT name FROM sys.tables;"
```

```sql
"SELECT name FROM sys.tables;" | sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I > .\tables.out
```

## <a name="next-steps"></a><span data-ttu-id="bea80-128">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bea80-128">Next steps</span></span>
<span data-ttu-id="bea80-129">Viz část [Dokumentace sqlcmd][sqlcmd], kde najdete další informace o možnostech dostupných v sqlcmd.</span><span class="sxs-lookup"><span data-stu-id="bea80-129">See [sqlcmd documentation][sqlcmd] for more about details about the options available in sqlcmd.</span></span>

<!--Image references-->

<!--Article references-->

<!--MSDN references--> 
[sqlcmd]: https://msdn.microsoft.com/library/ms162773.aspx
[Azure portal]: https://portal.azure.com

<!--Other Web references-->
