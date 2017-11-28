---
title: aaaConnect tooAzure SQL Data Warehouse sqlcmd | Microsoft Docs
description: "Použijte [sqlcmd] [sqlcmd] nástroj příkazového řádku tooconnect tooand dotaz Azure SQL Data Warehouse."
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
ms.openlocfilehash: 0334df7b969da1966ba29c97f835a2dc9e383e29
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-toosql-data-warehouse-with-sqlcmd"></a><span data-ttu-id="7b7c0-103">Připojit tooSQL datového skladu pomocí sqlcmd</span><span class="sxs-lookup"><span data-stu-id="7b7c0-103">Connect tooSQL Data Warehouse with sqlcmd</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="7b7c0-104">Power BI</span><span class="sxs-lookup"><span data-stu-id="7b7c0-104">Power BI</span></span>](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [<span data-ttu-id="7b7c0-105">Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="7b7c0-105">Azure Machine Learning</span></span>](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [<span data-ttu-id="7b7c0-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7b7c0-106">Visual Studio</span></span>](sql-data-warehouse-query-visual-studio.md)
> * [<span data-ttu-id="7b7c0-107">sqlcmd</span><span class="sxs-lookup"><span data-stu-id="7b7c0-107">sqlcmd</span></span>](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> * [<span data-ttu-id="7b7c0-108">SSMS</span><span class="sxs-lookup"><span data-stu-id="7b7c0-108">SSMS</span></span>](sql-data-warehouse-query-ssms.md)
> 
> 

<span data-ttu-id="7b7c0-109">Použití [sqlcmd] [ sqlcmd] tooand tooconnect nástroj příkazového řádku dotaz Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="7b7c0-109">Use [sqlcmd][sqlcmd] command-line utility tooconnect tooand query an Azure SQL Data Warehouse.</span></span>  

## <a name="1-connect"></a><span data-ttu-id="7b7c0-110">1. Připojení</span><span class="sxs-lookup"><span data-stu-id="7b7c0-110">1. Connect</span></span>
<span data-ttu-id="7b7c0-111">tooget začít s [sqlcmd][sqlcmd], otevřete hello příkazový řádek a zadejte **sqlcmd** následuje hello připojovací řetězec pro vaši databázi SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="7b7c0-111">tooget started with [sqlcmd][sqlcmd], open hello command prompt and enter **sqlcmd** followed by hello connection string for your SQL Data Warehouse database.</span></span> <span data-ttu-id="7b7c0-112">Hello připojovací řetězec potřebuje hello následující parametry:</span><span class="sxs-lookup"><span data-stu-id="7b7c0-112">hello connection string requires hello following parameters:</span></span>

* <span data-ttu-id="7b7c0-113">**Server (-S):** serveru v podobě hello `<`název serveru`>`. database.windows.net</span><span class="sxs-lookup"><span data-stu-id="7b7c0-113">**Server (-S):** Server in hello form `<`Server Name`>`.database.windows.net</span></span>
* <span data-ttu-id="7b7c0-114">**Database (-d):** Název databáze</span><span class="sxs-lookup"><span data-stu-id="7b7c0-114">**Database (-d):** Database name.</span></span>
* <span data-ttu-id="7b7c0-115">**Enable Quoted Identifiers (-I):** identifikátory v uvozovkách, musí být povoleno tooconnect instanci SQL Data Warehouse tooa.</span><span class="sxs-lookup"><span data-stu-id="7b7c0-115">**Enable Quoted Identifiers (-I):** Quoted identifiers must be enabled tooconnect tooa SQL Data Warehouse instance.</span></span>

<span data-ttu-id="7b7c0-116">toouse ověřování systému SQL Server, je třeba tooadd hello uživatelského jména a hesla parametry:</span><span class="sxs-lookup"><span data-stu-id="7b7c0-116">toouse SQL Server Authentication, you need tooadd hello username/password parameters:</span></span>

* <span data-ttu-id="7b7c0-117">**Uživatel (-U):** uživatel serveru v podobě hello `<`uživatele`>`</span><span class="sxs-lookup"><span data-stu-id="7b7c0-117">**User (-U):** Server user in hello form `<`User`>`</span></span>
* <span data-ttu-id="7b7c0-118">**Heslo (-P):** heslo, které jsou přidružené uživateli hello.</span><span class="sxs-lookup"><span data-stu-id="7b7c0-118">**Password (-P):** Password associated with hello user.</span></span>

<span data-ttu-id="7b7c0-119">Připojovací řetězec může například vypadat hello následující:</span><span class="sxs-lookup"><span data-stu-id="7b7c0-119">For example, your connection string might look like hello following:</span></span>

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
```

<span data-ttu-id="7b7c0-120">Azure Active Directory integrované ověřování toouse, je třeba tooadd hello Azure Active Directory parametry:</span><span class="sxs-lookup"><span data-stu-id="7b7c0-120">toouse Azure Active Directory Integrated authentication, you need tooadd hello Azure Active Directory parameters:</span></span>

* <span data-ttu-id="7b7c0-121">**Azure Active Directory Authentication (-G):** Pro ověřování používat Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7b7c0-121">**Azure Active Directory Authentication (-G):** use Azure Active Directory for authentication</span></span>

<span data-ttu-id="7b7c0-122">Připojovací řetězec může například vypadat hello následující:</span><span class="sxs-lookup"><span data-stu-id="7b7c0-122">For example, your connection string might look like hello following:</span></span>

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -G -I
```

> [!NOTE]
> <span data-ttu-id="7b7c0-123">Je třeba příliš[povolení ověřování Azure Active Directory](sql-data-warehouse-authentication.md) tooauthenticate pomocí služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="7b7c0-123">You need too[enable Azure Active Directory Authentication](sql-data-warehouse-authentication.md) tooauthenticate using Active Directory.</span></span>
> 
> 

## <a name="2-query"></a><span data-ttu-id="7b7c0-124">2. Dotaz</span><span class="sxs-lookup"><span data-stu-id="7b7c0-124">2. Query</span></span>
<span data-ttu-id="7b7c0-125">Po připojení můžete použít všechny podporované příkazy jazyka Transact-SQL proti hello.</span><span class="sxs-lookup"><span data-stu-id="7b7c0-125">After connection, you can issue any supported Transact-SQL statements against hello instance.</span></span>  <span data-ttu-id="7b7c0-126">V tomto příkladu jsou dotazy zadávány v interaktivním režimu.</span><span class="sxs-lookup"><span data-stu-id="7b7c0-126">In this example, queries are submitted in interactive mode.</span></span>

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
1> SELECT name FROM sys.tables;
2> GO
3> QUIT
```

<span data-ttu-id="7b7c0-127">Tyto další příklady ukazují, jak můžete své dotazy spustit v dávkovém režimu pomocí možnosti -Q hello nebo potrubí vaší toosqlcmd SQL.</span><span class="sxs-lookup"><span data-stu-id="7b7c0-127">These next examples show how you can run your queries in batch mode using hello -Q option or piping your SQL toosqlcmd.</span></span>

```sql
sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I -Q "SELECT name FROM sys.tables;"
```

```sql
"SELECT name FROM sys.tables;" | sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I > .\tables.out
```

## <a name="next-steps"></a><span data-ttu-id="7b7c0-128">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7b7c0-128">Next steps</span></span>
<span data-ttu-id="7b7c0-129">V tématu [dokumentaci k sqlcmd] [ sqlcmd] Další informace o podrobnosti o možnostech hello k dispozici v sqlcmd.</span><span class="sxs-lookup"><span data-stu-id="7b7c0-129">See [sqlcmd documentation][sqlcmd] for more about details about hello options available in sqlcmd.</span></span>

<!--Image references-->

<!--Article references-->

<!--MSDN references--> 
[sqlcmd]: https://msdn.microsoft.com/library/ms162773.aspx
[Azure portal]: https://portal.azure.com

<!--Other Web references-->
