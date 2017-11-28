---
title: "aaaMigrate tooSQL vaše řešení datového skladu | Microsoft Docs"
description: "Migrace pokyny k uvedení platforma SQL Data Warehouse tooAzure vaše řešení."
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
ms.openlocfilehash: 27b51f15247603f054070f360ede7f24541c0288
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-your-solution-tooazure-sql-data-warehouse"></a><span data-ttu-id="f45d1-103">Migrace vašeho řešení tooAzure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="f45d1-103">Migrate your solution tooAzure SQL Data Warehouse</span></span>
<span data-ttu-id="f45d1-104">Najdete v části Postup při migraci stávající tooAzure řešení databáze SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="f45d1-104">See what's involved in migrating an existing database solution tooAzure SQL Data Warehouse.</span></span> 

## <a name="profile-your-workload"></a><span data-ttu-id="f45d1-105">Profil velikosti pracovní zátěže</span><span class="sxs-lookup"><span data-stu-id="f45d1-105">Profile your workload</span></span>
<span data-ttu-id="f45d1-106">Před migrací, budete chtít toobe určité že datový sklad SQL je hello správným řešením pro úlohy.</span><span class="sxs-lookup"><span data-stu-id="f45d1-106">Before migrating, you want toobe certain SQL Data Warehouse is hello right solution for your workload.</span></span> <span data-ttu-id="f45d1-107">Datový sklad SQL je tooperform analýzy velkých objemů dat distribuovaný systém určený.</span><span class="sxs-lookup"><span data-stu-id="f45d1-107">SQL Data Warehouse is a distributed system designed tooperform analytics on large data.</span></span>  <span data-ttu-id="f45d1-108">Migrace tooSQL datového skladu vyžaduje některé změny návrhu, které nejsou příliš pevný toounderstand, ale může trvat některé tooimplement čas.</span><span class="sxs-lookup"><span data-stu-id="f45d1-108">Migrating tooSQL Data Warehouse requires some design changes that are not too hard toounderstand but might take some time tooimplement.</span></span> <span data-ttu-id="f45d1-109">Jestli podnik potřebuje podnikové třídy datového skladu, hello výhody jsou vhodné hello úsilí.</span><span class="sxs-lookup"><span data-stu-id="f45d1-109">If your business requires an enterprise-class data warehouse, hello benefits are worth hello effort.</span></span> <span data-ttu-id="f45d1-110">Pokud nepotřebujete hello power služby SQL Data Warehouse, je však další nákladově efektivní toouse systému SQL Server nebo Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="f45d1-110">However, if you don't need hello power of SQL Data Warehouse, it is more cost-effective toouse SQL Server or Azure SQL Database.</span></span>

<span data-ttu-id="f45d1-111">Zvažte použití SQL Data Warehouse při můžete:</span><span class="sxs-lookup"><span data-stu-id="f45d1-111">Consider using SQL Data Warehouse when you:</span></span>
- <span data-ttu-id="f45d1-112">Mít jeden nebo více terabajtů dat</span><span class="sxs-lookup"><span data-stu-id="f45d1-112">Have one or more Terabytes of data</span></span>
- <span data-ttu-id="f45d1-113">Plán toorun analýzy velkých objemů dat.</span><span class="sxs-lookup"><span data-stu-id="f45d1-113">Plan toorun analytics on large amounts of data</span></span>
- <span data-ttu-id="f45d1-114">Třeba hello možnost tooscale výpočetního prostředí a úložiště</span><span class="sxs-lookup"><span data-stu-id="f45d1-114">Need hello ability tooscale compute and storage</span></span> 
- <span data-ttu-id="f45d1-115">Chtít toosave náklady ponecháte-výpočetní prostředky, když je nepotřebujete.</span><span class="sxs-lookup"><span data-stu-id="f45d1-115">Want toosave costs by pausing compute resources when you don't need them.</span></span>

<span data-ttu-id="f45d1-116">Nepoužívejte SQL Data Warehouse pro provozní úlohy (OLTP), které mají:</span><span class="sxs-lookup"><span data-stu-id="f45d1-116">Don't use SQL Data Warehouse for operational (OLTP) workloads that have:</span></span>
- <span data-ttu-id="f45d1-117">Vysoká frekvence čte a zapisuje</span><span class="sxs-lookup"><span data-stu-id="f45d1-117">High frequency reads and writes</span></span>
- <span data-ttu-id="f45d1-118">Vybere velkého počtu singleton</span><span class="sxs-lookup"><span data-stu-id="f45d1-118">Large numbers of singleton selects</span></span>
- <span data-ttu-id="f45d1-119">Velký objem jednoho řádku vložení</span><span class="sxs-lookup"><span data-stu-id="f45d1-119">High volumes of single row inserts</span></span>
- <span data-ttu-id="f45d1-120">Řádek po řádku zpracování potřeb</span><span class="sxs-lookup"><span data-stu-id="f45d1-120">Row by row processing needs</span></span>
- <span data-ttu-id="f45d1-121">Nekompatibilních formátech (JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="f45d1-121">Incompatible formats (JSON, XML)</span></span>


## <a name="plan-hello-migration"></a><span data-ttu-id="f45d1-122">Plánování migrace hello</span><span class="sxs-lookup"><span data-stu-id="f45d1-122">Plan hello migration</span></span>

<span data-ttu-id="f45d1-123">Jakmile jste se rozhodli toomigrate existující řešení tooSQL datového skladu, je důležité tooplan hello migrace před Začínáme.</span><span class="sxs-lookup"><span data-stu-id="f45d1-123">Once you have decided toomigrate an existing solution tooSQL Data Warehouse, it is important tooplan hello migration before getting started.</span></span> 

<span data-ttu-id="f45d1-124">Jeden cíl plánování tooensure data, vaše schémata tabulek a kódu jsou kompatibilní s SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="f45d1-124">One goal of planning is tooensure your data, your table schemas, and your code are compatible with SQL Data Warehouse.</span></span> <span data-ttu-id="f45d1-125">Existují toowork některé kompatibility rozdíly kolem mezi aktuálním systému a SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="f45d1-125">There are some compatibility differences toowork around between your current system and SQL Data Warehouse.</span></span> <span data-ttu-id="f45d1-126">Plus migraci velkých objemů dat tooAzure trvá určitou dobu.</span><span class="sxs-lookup"><span data-stu-id="f45d1-126">Plus, migrating large amounts of data tooAzure takes time.</span></span> <span data-ttu-id="f45d1-127">Pečlivé plánování urychluje získávání tooAzure vaše data.</span><span class="sxs-lookup"><span data-stu-id="f45d1-127">Careful planning expedites getting your data tooAzure.</span></span> 

<span data-ttu-id="f45d1-128">Jiné cílem plánování je toomake návrhu tooensure úpravy, které řešení využívá výhod hello vysokého výkonu dotazu, který datový sklad SQL je určený tooprovide.</span><span class="sxs-lookup"><span data-stu-id="f45d1-128">Another goal of planning is toomake design adjustments tooensure your solution takes advantage of hello high query performance SQL Data Warehouse is designed tooprovide.</span></span> <span data-ttu-id="f45d1-129">Návrh datových skladů pro škálování zavádí vzory návrhu různých a proto tradiční přístupy nejsou vždy hello nejlépe.</span><span class="sxs-lookup"><span data-stu-id="f45d1-129">Designing data warehouses for scale introduces different design patterns and so traditional approaches aren't always hello best.</span></span> <span data-ttu-id="f45d1-130">I když můžete provést některé úpravy návrhu po migraci, uloží dříve provádění změn v procesu hello později.</span><span class="sxs-lookup"><span data-stu-id="f45d1-130">Although you can make some design adjustments after migration, making changes sooner in hello process will save time later.</span></span>

<span data-ttu-id="f45d1-131">tooperform úspěšné migrace, je nutné toomigrate vaše schémata tabulek, kód a data.</span><span class="sxs-lookup"><span data-stu-id="f45d1-131">tooperform a successful migration, you need toomigrate your table schemas, your code, and your data.</span></span> <span data-ttu-id="f45d1-132">Pokyny v těchto tématech migrace najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="f45d1-132">For guidance on these migration topics, see:</span></span>

-  [<span data-ttu-id="f45d1-133">Migrace vaší schémata</span><span class="sxs-lookup"><span data-stu-id="f45d1-133">Migrate your schemas</span></span>](sql-data-warehouse-migrate-schema.md)
-  [<span data-ttu-id="f45d1-134">Migrace vašeho kódu</span><span class="sxs-lookup"><span data-stu-id="f45d1-134">Migrate your code</span></span>](sql-data-warehouse-migrate-code.md)
-  <span data-ttu-id="f45d1-135">[Migrace dat](sql-data-warehouse-migrate-data.md).</span><span class="sxs-lookup"><span data-stu-id="f45d1-135">[Migrate your data](sql-data-warehouse-migrate-data.md).</span></span> 

<!--
## Perform hello migration


## Deploy hello solution


## Validate hello migration

-->

## <a name="next-steps"></a><span data-ttu-id="f45d1-136">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f45d1-136">Next steps</span></span>
<span data-ttu-id="f45d1-137">Hello CAT (poradní tým) má také některé skvělé SQL Data Warehouse pokyny, které publikují prostřednictvím blogy.</span><span class="sxs-lookup"><span data-stu-id="f45d1-137">hello CAT (Customer Advisory Team) also has some great SQL Data Warehouse guidance, which they publish through blogs.</span></span>  <span data-ttu-id="f45d1-138">Podívejte se na jejich článku [migrace dat tooAzure SQL Data Warehouse v praxi] [ Migrating data tooAzure SQL Data Warehouse in practice] o další pokyny k migraci.</span><span class="sxs-lookup"><span data-stu-id="f45d1-138">Take a look at their article, [Migrating data tooAzure SQL Data Warehouse in practice][Migrating data tooAzure SQL Data Warehouse in practice] for additional guidance on migration.</span></span>

<!--Image references-->

<!--Article references-->

<!--MSDN references-->

<!--Other Web references-->
[Migrating data tooAzure SQL Data Warehouse in practice]: https://blogs.msdn.microsoft.com/sqlcat/2016/08/18/migrating-data-to-azure-sql-data-warehouse-in-practice/
