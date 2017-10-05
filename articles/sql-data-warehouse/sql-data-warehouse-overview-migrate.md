---
title: "Migrace vašeho řešení do SQL Data Warehouse | Microsoft Docs"
description: "Migrace pokyny k uvedení řešení pro platformu Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: sqlmojo
manager: jhubbard
editor: 
ms.assetid: 198365eb-7451-4222-b99c-d1d9ef687f1b
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: migrate
ms.date: 06/27/2017
ms.author: joeyong;barbkess
ms.openlocfilehash: 771b9456e66b8a1e41f72340b695b19e2adaf793
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="migrate-your-solution-to-azure-sql-data-warehouse"></a><span data-ttu-id="c7bc6-103">Migrace vašeho řešení do Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="c7bc6-103">Migrate your solution to Azure SQL Data Warehouse</span></span>
<span data-ttu-id="c7bc6-104">Najdete v části Postup při migraci do stávajícího řešení pro databázi Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="c7bc6-104">See what's involved in migrating an existing database solution to Azure SQL Data Warehouse.</span></span> 

## <a name="profile-your-workload"></a><span data-ttu-id="c7bc6-105">Profil velikosti pracovní zátěže</span><span class="sxs-lookup"><span data-stu-id="c7bc6-105">Profile your workload</span></span>
<span data-ttu-id="c7bc6-106">Před migrací, mají být, že určité SQL Data Warehouse je to správné řešení pro úlohy.</span><span class="sxs-lookup"><span data-stu-id="c7bc6-106">Before migrating, you want to be certain SQL Data Warehouse is the right solution for your workload.</span></span> <span data-ttu-id="c7bc6-107">Datový sklad SQL je navržený tak, aby provádět analýzy na velkých objemů dat distribuovaného systému.</span><span class="sxs-lookup"><span data-stu-id="c7bc6-107">SQL Data Warehouse is a distributed system designed to perform analytics on large data.</span></span>  <span data-ttu-id="c7bc6-108">Migrace do SQL Data Warehouse vyžaduje některé změny návrhu, které nejsou příliš pevného, abyste pochopili, ale může trvat nějakou dobu implementace.</span><span class="sxs-lookup"><span data-stu-id="c7bc6-108">Migrating to SQL Data Warehouse requires some design changes that are not too hard to understand but might take some time to implement.</span></span> <span data-ttu-id="c7bc6-109">Jestli podnik potřebuje podnikové třídy datového skladu, výhody to stojí.</span><span class="sxs-lookup"><span data-stu-id="c7bc6-109">If your business requires an enterprise-class data warehouse, the benefits are worth the effort.</span></span> <span data-ttu-id="c7bc6-110">Pokud nepotřebujete power služby SQL Data Warehouse, je však cenově výhodnější používat SQL Server nebo Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="c7bc6-110">However, if you don't need the power of SQL Data Warehouse, it is more cost-effective to use SQL Server or Azure SQL Database.</span></span>

<span data-ttu-id="c7bc6-111">Zvažte použití SQL Data Warehouse při můžete:</span><span class="sxs-lookup"><span data-stu-id="c7bc6-111">Consider using SQL Data Warehouse when you:</span></span>
- <span data-ttu-id="c7bc6-112">Mít jeden nebo více terabajtů dat</span><span class="sxs-lookup"><span data-stu-id="c7bc6-112">Have one or more Terabytes of data</span></span>
- <span data-ttu-id="c7bc6-113">Chcete spustit analýzu na velkých objemů dat.</span><span class="sxs-lookup"><span data-stu-id="c7bc6-113">Plan to run analytics on large amounts of data</span></span>
- <span data-ttu-id="c7bc6-114">Potřebují možnost škálovat výpočetní kapacity a úložiště</span><span class="sxs-lookup"><span data-stu-id="c7bc6-114">Need the ability to scale compute and storage</span></span> 
- <span data-ttu-id="c7bc6-115">Chcete uložit náklady pozastavení výpočetní prostředky, když je nepotřebujete.</span><span class="sxs-lookup"><span data-stu-id="c7bc6-115">Want to save costs by pausing compute resources when you don't need them.</span></span>

<span data-ttu-id="c7bc6-116">Nepoužívejte SQL Data Warehouse pro provozní úlohy (OLTP), které mají:</span><span class="sxs-lookup"><span data-stu-id="c7bc6-116">Don't use SQL Data Warehouse for operational (OLTP) workloads that have:</span></span>
- <span data-ttu-id="c7bc6-117">Vysoká frekvence čte a zapisuje</span><span class="sxs-lookup"><span data-stu-id="c7bc6-117">High frequency reads and writes</span></span>
- <span data-ttu-id="c7bc6-118">Vybere velkého počtu singleton</span><span class="sxs-lookup"><span data-stu-id="c7bc6-118">Large numbers of singleton selects</span></span>
- <span data-ttu-id="c7bc6-119">Velký objem jednoho řádku vložení</span><span class="sxs-lookup"><span data-stu-id="c7bc6-119">High volumes of single row inserts</span></span>
- <span data-ttu-id="c7bc6-120">Řádek po řádku zpracování potřeb</span><span class="sxs-lookup"><span data-stu-id="c7bc6-120">Row by row processing needs</span></span>
- <span data-ttu-id="c7bc6-121">Nekompatibilních formátech (JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="c7bc6-121">Incompatible formats (JSON, XML)</span></span>


## <a name="plan-the-migration"></a><span data-ttu-id="c7bc6-122">Plánování migrace</span><span class="sxs-lookup"><span data-stu-id="c7bc6-122">Plan the migration</span></span>

<span data-ttu-id="c7bc6-123">Až se rozhodnete k migraci existujícího řešení do SQL Data Warehouse, je důležité plánovat migraci před Začínáme.</span><span class="sxs-lookup"><span data-stu-id="c7bc6-123">Once you have decided to migrate an existing solution to SQL Data Warehouse, it is important to plan the migration before getting started.</span></span> 

<span data-ttu-id="c7bc6-124">Plánování jeden cílem je zajistit, že vaše data, vaše schémata tabulek a kódu jsou kompatibilní s SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="c7bc6-124">One goal of planning is to ensure your data, your table schemas, and your code are compatible with SQL Data Warehouse.</span></span> <span data-ttu-id="c7bc6-125">Existují určité rozdíly kompatibility obejít mezi aktuálním systému a SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="c7bc6-125">There are some compatibility differences to work around between your current system and SQL Data Warehouse.</span></span> <span data-ttu-id="c7bc6-126">Plus migrace velké objemy dat do Azure trvá určitou dobu.</span><span class="sxs-lookup"><span data-stu-id="c7bc6-126">Plus, migrating large amounts of data to Azure takes time.</span></span> <span data-ttu-id="c7bc6-127">Pečlivé plánování urychluje získávání dat do Azure.</span><span class="sxs-lookup"><span data-stu-id="c7bc6-127">Careful planning expedites getting your data to Azure.</span></span> 

<span data-ttu-id="c7bc6-128">Jiné cílem plánování je provádět úpravy návrhu Ujistěte se, že vaše řešení využívá výhod vysokého výkonu dotazu, který SQL Data Warehouse je určená k poskytnutí.</span><span class="sxs-lookup"><span data-stu-id="c7bc6-128">Another goal of planning is to make design adjustments to ensure your solution takes advantage of the high query performance SQL Data Warehouse is designed to provide.</span></span> <span data-ttu-id="c7bc6-129">Návrh datových skladů pro škálování představuje různé návrhu vzory a proto tradiční přístupy nejsou vždy nejvhodnější.</span><span class="sxs-lookup"><span data-stu-id="c7bc6-129">Designing data warehouses for scale introduces different design patterns and so traditional approaches aren't always the best.</span></span> <span data-ttu-id="c7bc6-130">I když můžete provést některé úpravy návrhu po migraci, provedení změn dříve v procesu uloží později.</span><span class="sxs-lookup"><span data-stu-id="c7bc6-130">Although you can make some design adjustments after migration, making changes sooner in the process will save time later.</span></span>

<span data-ttu-id="c7bc6-131">K provedení úspěšné migrace, musíte migrovat vaší schémata tabulek, kód a data.</span><span class="sxs-lookup"><span data-stu-id="c7bc6-131">To perform a successful migration, you need to migrate your table schemas, your code, and your data.</span></span> <span data-ttu-id="c7bc6-132">Pokyny v těchto tématech migrace najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="c7bc6-132">For guidance on these migration topics, see:</span></span>

-  [<span data-ttu-id="c7bc6-133">Migrace vaší schémata</span><span class="sxs-lookup"><span data-stu-id="c7bc6-133">Migrate your schemas</span></span>](sql-data-warehouse-migrate-schema.md)
-  [<span data-ttu-id="c7bc6-134">Migrace vašeho kódu</span><span class="sxs-lookup"><span data-stu-id="c7bc6-134">Migrate your code</span></span>](sql-data-warehouse-migrate-code.md)
-  <span data-ttu-id="c7bc6-135">[Migrace dat](sql-data-warehouse-migrate-data.md).</span><span class="sxs-lookup"><span data-stu-id="c7bc6-135">[Migrate your data](sql-data-warehouse-migrate-data.md).</span></span> 

<!--
## Perform the migration


## Deploy the solution


## Validate the migration

-->

## <a name="next-steps"></a><span data-ttu-id="c7bc6-136">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c7bc6-136">Next steps</span></span>
<span data-ttu-id="c7bc6-137">CAT (poradní tým) má také některé skvělé SQL Data Warehouse pokyny, které publikují prostřednictvím blogy.</span><span class="sxs-lookup"><span data-stu-id="c7bc6-137">The CAT (Customer Advisory Team) also has some great SQL Data Warehouse guidance, which they publish through blogs.</span></span>  <span data-ttu-id="c7bc6-138">Podívejte se na jejich článku [migrace dat do Azure SQL Data Warehouse v praxi] [ Migrating data to Azure SQL Data Warehouse in practice] o další pokyny k migraci.</span><span class="sxs-lookup"><span data-stu-id="c7bc6-138">Take a look at their article, [Migrating data to Azure SQL Data Warehouse in practice][Migrating data to Azure SQL Data Warehouse in practice] for additional guidance on migration.</span></span>

<!--Image references-->

<!--Article references-->

<!--MSDN references-->

<!--Other Web references-->
[Migrating data to Azure SQL Data Warehouse in practice]: https://blogs.msdn.microsoft.com/sqlcat/2016/08/18/migrating-data-to-azure-sql-data-warehouse-in-practice/
