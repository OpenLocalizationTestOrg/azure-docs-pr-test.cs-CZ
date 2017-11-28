---
title: "schémata aaaUser definované v SQL Data Warehouse | Microsoft Docs"
description: "Tipy pro používání schémata Transact-SQL v Azure SQL Data Warehouse pro vývoj řešení."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: 52af5bd5-d5d3-4f9b-8704-06829fb924e3
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: t-sql
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: c411d6fed68e67c444a5871eab06182eaeb6dbf5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="user-defined-schemas-in-sql-data-warehouse"></a><span data-ttu-id="6e753-103">Uživatelem definované schémata v SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="6e753-103">User-defined schemas in SQL Data Warehouse</span></span>
<span data-ttu-id="6e753-104">Tradiční datových skladů často použijte samostatné databáze toocreate aplikace hranice podle zatížení, domény nebo zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="6e753-104">Traditional data warehouses often use separate databases toocreate application boundaries based on either workload, domain or security.</span></span> <span data-ttu-id="6e753-105">Tradiční datového skladu SQL serveru může například obsahovat pracovní databáze, databáze datového skladu a některé databáze datového tržiště.</span><span class="sxs-lookup"><span data-stu-id="6e753-105">For example, a traditional SQL Server data warehouse might include a staging database, a data warehouse database, and some data mart databases.</span></span> <span data-ttu-id="6e753-106">V této topologii každou databázi funguje jako hranice zabezpečení v architektuře hello a zatížení.</span><span class="sxs-lookup"><span data-stu-id="6e753-106">In this topology each database operates as a workload and security boundary in hello architecture.</span></span>

<span data-ttu-id="6e753-107">Naopak spouští SQL Data Warehouse hello celého datového skladu zatížení v rámci jedné databáze.</span><span class="sxs-lookup"><span data-stu-id="6e753-107">By contrast, SQL Data Warehouse runs hello entire data warehouse workload within one database.</span></span> <span data-ttu-id="6e753-108">Mezi databáze nejsou povoleny spojení.</span><span class="sxs-lookup"><span data-stu-id="6e753-108">Cross database joins are not permitted.</span></span> <span data-ttu-id="6e753-109">Proto SQL Data Warehouse očekává, že všechny tabulky použité ve toobe skladu hello uložené v rámci jedné databáze hello.</span><span class="sxs-lookup"><span data-stu-id="6e753-109">Therefore SQL Data Warehouse expects all tables used by hello warehouse toobe stored within hello one database.</span></span>

> [!NOTE]
> <span data-ttu-id="6e753-110">SQL Data Warehouse nepodporuje křížové databázové dotazy jakéhokoli druhu.</span><span class="sxs-lookup"><span data-stu-id="6e753-110">SQL Data Warehouse does not support cross database queries of any kind.</span></span> <span data-ttu-id="6e753-111">V důsledku toho datového skladu implementace, které využívají tento vzor potřebovat toobe revize.</span><span class="sxs-lookup"><span data-stu-id="6e753-111">Consequently, data warehouse implementations that leverage this pattern will need toobe revised.</span></span>
> 
> 

## <a name="recommendations"></a><span data-ttu-id="6e753-112">Doporučení</span><span class="sxs-lookup"><span data-stu-id="6e753-112">Recommendations</span></span>
<span data-ttu-id="6e753-113">Toto jsou doporučení pro konsolidaci úlohy, zabezpečení, domény a funkční hranice pomocí uživatelsky definované schémata</span><span class="sxs-lookup"><span data-stu-id="6e753-113">These are recommendations for consolidating workloads, security, domain and functional boundaries by using user defined schemas</span></span>

1. <span data-ttu-id="6e753-114">Použijte jeden toorun databáze SQL Data Warehouse úlohy celého datového skladu</span><span class="sxs-lookup"><span data-stu-id="6e753-114">Use one SQL Data Warehouse database toorun your entire data warehouse workload</span></span>
2. <span data-ttu-id="6e753-115">Konsolidovat existující databázi datového skladu prostředí toouse jeden datový sklad SQL</span><span class="sxs-lookup"><span data-stu-id="6e753-115">Consolidate your existing data warehouse environment toouse one SQL Data Warehouse database</span></span>
3. <span data-ttu-id="6e753-116">Využívání **uživatelem definované schémata** hranic hello tooprovide dřív implementovaná pomocí databáze.</span><span class="sxs-lookup"><span data-stu-id="6e753-116">Leverage **user-defined schemas** tooprovide hello boundary previously implemented using databases.</span></span>

<span data-ttu-id="6e753-117">Pokud nebyly použity uživatelem definované schémata dříve máte čistou projektem.</span><span class="sxs-lookup"><span data-stu-id="6e753-117">If user-defined schemas have not been used previously then you have a clean slate.</span></span> <span data-ttu-id="6e753-118">Jednoduše použijte hello starý název databáze jako hello základ pro váš vlastní schémata v databázi SQL Data Warehouse hello.</span><span class="sxs-lookup"><span data-stu-id="6e753-118">Simply use hello old database name as hello basis for your user-defined schemas in hello SQL Data Warehouse database.</span></span>

<span data-ttu-id="6e753-119">Pokud již byl použit schémata máte několik možností:</span><span class="sxs-lookup"><span data-stu-id="6e753-119">If schemas have already been used then you have a few options:</span></span>

1. <span data-ttu-id="6e753-120">Odeberte hello starší verze schématu názvy a začít pracovat</span><span class="sxs-lookup"><span data-stu-id="6e753-120">Remove hello legacy schema names and start fresh</span></span>
2. <span data-ttu-id="6e753-121">Zachovat starší verze schématu názvy hello název tabulky toohello název starší verze schématu předem čekající hello</span><span class="sxs-lookup"><span data-stu-id="6e753-121">Retain hello legacy schema names by pre-pending hello legacy schema name toohello table name</span></span>
3. <span data-ttu-id="6e753-122">Zachovat starší verze schématu názvy hello implementací zobrazení hello tabulky v navíc schématu toore-vytvořit původní struktura schématu hello.</span><span class="sxs-lookup"><span data-stu-id="6e753-122">Retain hello legacy schema names by implementing views over hello table in an extra schema toore-create hello old schema structure.</span></span>

> [!NOTE]
> <span data-ttu-id="6e753-123">Na první kontroly zdánlivě možnost 3 jako možnost nejvíce přitažlivými hello.</span><span class="sxs-lookup"><span data-stu-id="6e753-123">On first inspection option 3 may seem like hello most appealing option.</span></span> <span data-ttu-id="6e753-124">Je však hello ďábla podrobně hello.</span><span class="sxs-lookup"><span data-stu-id="6e753-124">However, hello devil is in hello detail.</span></span> <span data-ttu-id="6e753-125">Zobrazení se čtou jenom v SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="6e753-125">Views are read only in SQL Data Warehouse.</span></span> <span data-ttu-id="6e753-126">Všechny změny dat nebo tabulka, potřebovat toobe adresovat hello základní tabulky.</span><span class="sxs-lookup"><span data-stu-id="6e753-126">Any data or table modification would need toobe performed against hello base table.</span></span> <span data-ttu-id="6e753-127">Možnost 3 také zavádí vrstvu zobrazení do systému.</span><span class="sxs-lookup"><span data-stu-id="6e753-127">Option 3 also introduces a layer of views into your system.</span></span> <span data-ttu-id="6e753-128">Můžete chtít toogive tento některé další myšlenku Pokud používáte zobrazení ve vaší architektury již.</span><span class="sxs-lookup"><span data-stu-id="6e753-128">You might want toogive this some additional thought if you are using views in your architecture already.</span></span>
> 
> 

### <a name="examples"></a><span data-ttu-id="6e753-129">Příklady:</span><span class="sxs-lookup"><span data-stu-id="6e753-129">Examples:</span></span>
<span data-ttu-id="6e753-130">Implementovat vlastní schémata podle názvy databází</span><span class="sxs-lookup"><span data-stu-id="6e753-130">Implement user-defined schemas based on database names</span></span>

```sql
CREATE SCHEMA [stg]; -- stg previously database name for staging database
GO
CREATE SCHEMA [edw]; -- edw previously database name for hello data warehouse
GO
CREATE TABLE [stg].[customer] -- create staging tables in hello stg schema
(       CustKey BIGINT NOT NULL
,       ...
);
GO
CREATE TABLE [edw].[customer] -- create data warehouse tables in hello edw schema
(       CustKey BIGINT NOT NULL
,       ...
);
```

<span data-ttu-id="6e753-131">Zachovat starší verze schématu názvy podle předem čekající je toohello název tabulky.</span><span class="sxs-lookup"><span data-stu-id="6e753-131">Retain legacy schema names by pre-pending them toohello table name.</span></span> <span data-ttu-id="6e753-132">Použijte schémata hello zatížení hranice.</span><span class="sxs-lookup"><span data-stu-id="6e753-132">Use schemas for hello workload boundary.</span></span>

```sql
CREATE SCHEMA [stg]; -- stg defines hello staging boundary
GO
CREATE SCHEMA [edw]; -- edw defines hello data warehouse boundary
GO
CREATE TABLE [stg].[dim_customer] --pre-pend hello old schema name toohello table and create in hello staging boundary
(       CustKey BIGINT NOT NULL
,       ...
);
GO
CREATE TABLE [edw].[dim_customer] --pre-pend hello old schema name toohello table and create in hello data warehouse boundary
(       CustKey BIGINT NOT NULL
,       ...
);
```

<span data-ttu-id="6e753-133">Zachovat starší verze schématu názvy pomocí zobrazení</span><span class="sxs-lookup"><span data-stu-id="6e753-133">Retain legacy schema names using views</span></span>

```sql
CREATE SCHEMA [stg]; -- stg defines hello staging boundary
GO
CREATE SCHEMA [edw]; -- stg defines hello data warehouse boundary
GO
CREATE SCHEMA [dim]; -- edw defines hello legacy schema name boundary
GO
CREATE TABLE [stg].[customer] -- create hello base staging tables in hello staging boundary
(       CustKey    BIGINT NOT NULL
,       ...
)
GO
CREATE TABLE [edw].[customer] -- create hello base data warehouse tables in hello data warehouse boundary
(       CustKey    BIGINT NOT NULL
,       ...
)
GO
CREATE VIEW [dim].[customer] -- create a view in hello legacy schema name boundary for presentation consistency purposes only
AS
SELECT  CustKey
,       ...
FROM    [edw].customer
;
```

> [!NOTE]
> <span data-ttu-id="6e753-134">Všechny změny ve schématu strategii potřebuje kontrolu hello model zabezpečení pro databázi hello.</span><span class="sxs-lookup"><span data-stu-id="6e753-134">Any change in schema strategy needs a review of hello security model for hello database.</span></span> <span data-ttu-id="6e753-135">V mnoha případech je možné toosimplify model zabezpečení hello přiřazením oprávnění na úrovni schématu hello.</span><span class="sxs-lookup"><span data-stu-id="6e753-135">In many cases you might be able toosimplify hello security model by assigning permissions at hello schema level.</span></span> <span data-ttu-id="6e753-136">Pokud jsou požadována oprávnění podrobnější můžete použít role databáze.</span><span class="sxs-lookup"><span data-stu-id="6e753-136">If more granular permissions are required then you can use database roles.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="6e753-137">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6e753-137">Next steps</span></span>
<span data-ttu-id="6e753-138">Další tipy pro vývoj, najdete v části [přehled vývoje][development overview].</span><span class="sxs-lookup"><span data-stu-id="6e753-138">For more development tips, see [development overview][development overview].</span></span>

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->
