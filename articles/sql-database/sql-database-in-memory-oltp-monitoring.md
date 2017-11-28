---
title: "úložiště v paměti XTP aaaMonitor | Microsoft Docs"
description: "Odhad a monitorování úložiště v paměti XTP používat, kapacity; Vyřešte chyby kapacity 41823"
services: sql-database
documentationcenter: 
author: jodebrui
manager: jhubbard
editor: 
ms.assetid: b617308e-692c-4938-8fa2-070034a3ecef
ms.service: sql-database
ms.custom: monitor & tune
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/19/2016
ms.author: jodebrui
ms.openlocfilehash: fcb17bd8e9ebef4862d4b55bf5a79b45b9419fca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-in-memory-oltp-storage"></a><span data-ttu-id="fb7ca-103">Monitorování úložiště OLTP v paměti</span><span class="sxs-lookup"><span data-stu-id="fb7ca-103">Monitor In-Memory OLTP Storage</span></span>
<span data-ttu-id="fb7ca-104">Při použití [OLTP v paměti](sql-database-in-memory.md), data v paměťově optimalizované tabulky a proměnných tabulek, které se nachází v úložišti OLTP v paměti.</span><span class="sxs-lookup"><span data-stu-id="fb7ca-104">When using [In-Memory OLTP](sql-database-in-memory.md), data in memory-optimized tables and table variables resides in In-Memory OLTP storage.</span></span> <span data-ttu-id="fb7ca-105">Každou úroveň služeb Premium je maximální velikost úložiště OLTP v paměti, která je popsána v hello [úrovně služeb SQL Database článku](sql-database-service-tiers.md#single-database-service-tiers-and-performance-levels).</span><span class="sxs-lookup"><span data-stu-id="fb7ca-105">Each Premium service tier has a maximum In-Memory OLTP storage size, which is documented in hello [SQL Database Service Tiers article](sql-database-service-tiers.md#single-database-service-tiers-and-performance-levels).</span></span> <span data-ttu-id="fb7ca-106">Po překročení tohoto limitu vložení a aktualizace operace může spustit (došlo k chybě 41823).</span><span class="sxs-lookup"><span data-stu-id="fb7ca-106">Once this limit is exceeded, insert and update operations may start failing (with error 41823).</span></span> <span data-ttu-id="fb7ca-107">V tomto okamžiku budete potřebovat tooeither odstranit data tooreclaim paměti, nebo upgradovat úroveň výkonu hello vaší databáze.</span><span class="sxs-lookup"><span data-stu-id="fb7ca-107">At that point you will need tooeither delete data tooreclaim memory, or upgrade hello performance tier of your database.</span></span>

## <a name="determine-whether-data-will-fit-within-hello-in-memory-storage-cap"></a><span data-ttu-id="fb7ca-108">Určení, zda data se vejde do cap hello úložiště v paměti</span><span class="sxs-lookup"><span data-stu-id="fb7ca-108">Determine whether data will fit within hello in-memory storage cap</span></span>
<span data-ttu-id="fb7ca-109">Určit úložiště cap hello: najdete hello [úrovně služeb SQL Database článku](sql-database-service-tiers.md#single-database-service-tiers-and-performance-levels) pro hello úložiště CAP o hello různých úrovních služeb Premium.</span><span class="sxs-lookup"><span data-stu-id="fb7ca-109">Determine hello storage cap: consult hello [SQL Database Service Tiers article](sql-database-service-tiers.md#single-database-service-tiers-and-performance-levels) for hello storage caps of hello different Premium service tiers.</span></span>

<span data-ttu-id="fb7ca-110">Odhad paměti, že požadavky pro paměťově optimalizované tabulky funguje hello stejný způsob systému SQL Server, protože nemá ve službě Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="fb7ca-110">Estimating memory requirements for a memory-optimized table works hello same way for SQL Server as it does in Azure SQL Database.</span></span> <span data-ttu-id="fb7ca-111">Trvat několik minut tooreview tohoto tématu [MSDN](https://msdn.microsoft.com/library/dn282389.aspx).</span><span class="sxs-lookup"><span data-stu-id="fb7ca-111">Take a few minutes tooreview that topic on [MSDN](https://msdn.microsoft.com/library/dn282389.aspx).</span></span>

<span data-ttu-id="fb7ca-112">Všimněte si, že tabulka hello a proměnné řádky tabulky, jakož i indexy, započítávat velikost dat max uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="fb7ca-112">Note that hello table and table variable rows, as well as indexes, count toward hello max user data size.</span></span> <span data-ttu-id="fb7ca-113">Příkaz ALTER TABLE navíc musí dostatek místnosti toocreate novou verzi hello celou tabulku a jeho indexů.</span><span class="sxs-lookup"><span data-stu-id="fb7ca-113">In addition, ALTER TABLE needs enough room toocreate a new version of hello entire table and its indexes.</span></span>

## <a name="monitoring-and-alerting"></a><span data-ttu-id="fb7ca-114">Monitorování a upozorňování</span><span class="sxs-lookup"><span data-stu-id="fb7ca-114">Monitoring and alerting</span></span>
<span data-ttu-id="fb7ca-115">Můžete sledovat využití úložiště v paměti jako procentní podíl hello [limitu úložiště pro vaše úroveň výkonu](sql-database-service-tiers.md#single-database-service-tiers-and-performance-levels) v hello Azure [portál](https://portal.azure.com/):</span><span class="sxs-lookup"><span data-stu-id="fb7ca-115">You can monitor in-memory storage use as a percentage of hello [storage cap for your performance tier](sql-database-service-tiers.md#single-database-service-tiers-and-performance-levels) in hello Azure [portal](https://portal.azure.com/):</span></span> 

* <span data-ttu-id="fb7ca-116">V okně databáze hello vyhledejte hello prostředků využití políčko a klikněte na Upravit.</span><span class="sxs-lookup"><span data-stu-id="fb7ca-116">On hello Database blade, locate hello Resource utilization box and click on Edit.</span></span>
* <span data-ttu-id="fb7ca-117">Vyberte metriku hello `In-Memory OLTP Storage percentage`.</span><span class="sxs-lookup"><span data-stu-id="fb7ca-117">Then select hello metric `In-Memory OLTP Storage percentage`.</span></span>
* <span data-ttu-id="fb7ca-118">tooadd výstrahu, klikněte na hello využití prostředků pole tooopen hello metriky okna a potom klikněte na Přidat upozornění.</span><span class="sxs-lookup"><span data-stu-id="fb7ca-118">tooadd an alert, click on hello Resource Utilization box tooopen hello Metric blade, then click on Add alert.</span></span>

<span data-ttu-id="fb7ca-119">Nebo použijte hello následující dotaz využití úložiště v paměti tooshow hello:</span><span class="sxs-lookup"><span data-stu-id="fb7ca-119">Or use hello following query tooshow hello in-memory storage utilization:</span></span>

    SELECT xtp_storage_percent FROM sys.dm_db_resource_stats


## <a name="correct-out-of-memory-situations---error-41823"></a><span data-ttu-id="fb7ca-120">Opravte z důvodu nedostatku paměti situacích - chyba 41823</span><span class="sxs-lookup"><span data-stu-id="fb7ca-120">Correct out-of-memory situations - Error 41823</span></span>
<span data-ttu-id="fb7ca-121">V operacích INSERT, UPDATE a vytvořit selhání s chybovou zprávou 41823 spuštěna výsledky z důvodu nedostatku paměti.</span><span class="sxs-lookup"><span data-stu-id="fb7ca-121">Running out-of-memory results in INSERT, UPDATE, and CREATE operations failing with error message 41823.</span></span>

<span data-ttu-id="fb7ca-122">Chybová zpráva 41823 označuje, že hello paměťově optimalizované tabulky a proměnných tabulek, které překročily maximální velikost hello.</span><span class="sxs-lookup"><span data-stu-id="fb7ca-122">Error message 41823 indicates that hello memory-optimized tables and table variables have exceeded hello maximum size.</span></span>

<span data-ttu-id="fb7ca-123">tooresolve tato chyba, buď:</span><span class="sxs-lookup"><span data-stu-id="fb7ca-123">tooresolve this error, either:</span></span>

* <span data-ttu-id="fb7ca-124">Odstranění dat z hello paměťově optimalizované tabulky, potenciálně přesměrovává hello data tootraditional, založené na disku tabulky; nebo,</span><span class="sxs-lookup"><span data-stu-id="fb7ca-124">Delete data from hello memory-optimized tables, potentially offloading hello data tootraditional, disk-based tables; or,</span></span>
* <span data-ttu-id="fb7ca-125">Upgrade tooone vrstvy služby hello s dostatečným místem úložiště v paměti pro hello data potřebujete tookeep v paměťově optimalizovaných tabulkách.</span><span class="sxs-lookup"><span data-stu-id="fb7ca-125">Upgrade hello service tier tooone with enough in-memory storage for hello data you need tookeep in memory-optimized tables.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fb7ca-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fb7ca-126">Next steps</span></span>
<span data-ttu-id="fb7ca-127">Monitorování na pokyny najdete v části [monitorování databáze Azure SQL pomocí zobrazení dynamické správy](sql-database-monitoring-with-dmvs.md).</span><span class="sxs-lookup"><span data-stu-id="fb7ca-127">For monitoring guidance, see [Monitoring Azure SQL Database using dynamic management views](sql-database-monitoring-with-dmvs.md).</span></span>
