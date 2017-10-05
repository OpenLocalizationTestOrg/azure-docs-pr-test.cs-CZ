---
title: "Monitorování úložiště v paměti XTP | Microsoft Docs"
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
ms.openlocfilehash: 5afb2209f18b1ba2aa0a916a439509b01afd80da
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="monitor-in-memory-oltp-storage"></a><span data-ttu-id="746f1-103">Monitorování úložiště OLTP v paměti</span><span class="sxs-lookup"><span data-stu-id="746f1-103">Monitor In-Memory OLTP Storage</span></span>
<span data-ttu-id="746f1-104">Při použití [OLTP v paměti](sql-database-in-memory.md), data v paměťově optimalizované tabulky a proměnných tabulek, které se nachází v úložišti OLTP v paměti.</span><span class="sxs-lookup"><span data-stu-id="746f1-104">When using [In-Memory OLTP](sql-database-in-memory.md), data in memory-optimized tables and table variables resides in In-Memory OLTP storage.</span></span> <span data-ttu-id="746f1-105">Má maximální velikost úložiště OLTP v paměti, která je popsána v jednotlivých úrovních služby Premium [úrovně služeb SQL Database článku](sql-database-service-tiers.md#single-database-service-tiers-and-performance-levels).</span><span class="sxs-lookup"><span data-stu-id="746f1-105">Each Premium service tier has a maximum In-Memory OLTP storage size, which is documented in the [SQL Database Service Tiers article](sql-database-service-tiers.md#single-database-service-tiers-and-performance-levels).</span></span> <span data-ttu-id="746f1-106">Po překročení tohoto limitu vložení a aktualizace operace může spustit (došlo k chybě 41823).</span><span class="sxs-lookup"><span data-stu-id="746f1-106">Once this limit is exceeded, insert and update operations may start failing (with error 41823).</span></span> <span data-ttu-id="746f1-107">V tomto okamžiku budete potřebovat buď odstranit data získat paměť, nebo upgradujte úroveň výkonu databáze.</span><span class="sxs-lookup"><span data-stu-id="746f1-107">At that point you will need to either delete data to reclaim memory, or upgrade the performance tier of your database.</span></span>

## <a name="determine-whether-data-will-fit-within-the-in-memory-storage-cap"></a><span data-ttu-id="746f1-108">Určení, zda budou data nevejdou do limitu úložiště v paměti</span><span class="sxs-lookup"><span data-stu-id="746f1-108">Determine whether data will fit within the in-memory storage cap</span></span>
<span data-ttu-id="746f1-109">Určit úložiště cap: obrátit [článku úrovních služby databáze SQL](sql-database-service-tiers.md#single-database-service-tiers-and-performance-levels) pro úložiště CAP o různých úrovních služeb Premium.</span><span class="sxs-lookup"><span data-stu-id="746f1-109">Determine the storage cap: consult the [SQL Database Service Tiers article](sql-database-service-tiers.md#single-database-service-tiers-and-performance-levels) for the storage caps of the different Premium service tiers.</span></span>

<span data-ttu-id="746f1-110">Odhadnout požadavky na paměť pro paměťově optimalizované tabulky funguje pro SQL Server stejným způsobem jako se nepodporuje v Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="746f1-110">Estimating memory requirements for a memory-optimized table works the same way for SQL Server as it does in Azure SQL Database.</span></span> <span data-ttu-id="746f1-111">Trvat několik minut, přečtěte si toto téma na [MSDN](https://msdn.microsoft.com/library/dn282389.aspx).</span><span class="sxs-lookup"><span data-stu-id="746f1-111">Take a few minutes to review that topic on [MSDN](https://msdn.microsoft.com/library/dn282389.aspx).</span></span>

<span data-ttu-id="746f1-112">Všimněte si, že tabulky a proměnné řádky tabulky, protože jako indexy, započítávat velikost dat max uživatele.</span><span class="sxs-lookup"><span data-stu-id="746f1-112">Note that the table and table variable rows, as well as indexes, count toward the max user data size.</span></span> <span data-ttu-id="746f1-113">Příkaz ALTER TABLE navíc vyžaduje dostatek místa k vytvoření nové verze celou tabulku a jeho indexů.</span><span class="sxs-lookup"><span data-stu-id="746f1-113">In addition, ALTER TABLE needs enough room to create a new version of the entire table and its indexes.</span></span>

## <a name="monitoring-and-alerting"></a><span data-ttu-id="746f1-114">Monitorování a upozorňování</span><span class="sxs-lookup"><span data-stu-id="746f1-114">Monitoring and alerting</span></span>
<span data-ttu-id="746f1-115">Můžete sledovat využití úložiště v paměti jako procentní podíl [limitu úložiště pro vaše úroveň výkonu](sql-database-service-tiers.md#single-database-service-tiers-and-performance-levels) ve službě Azure [portál](https://portal.azure.com/):</span><span class="sxs-lookup"><span data-stu-id="746f1-115">You can monitor in-memory storage use as a percentage of the [storage cap for your performance tier](sql-database-service-tiers.md#single-database-service-tiers-and-performance-levels) in the Azure [portal](https://portal.azure.com/):</span></span> 

* <span data-ttu-id="746f1-116">V okně databáze vyhledejte pole využití prostředků a klikněte na Upravit.</span><span class="sxs-lookup"><span data-stu-id="746f1-116">On the Database blade, locate the Resource utilization box and click on Edit.</span></span>
* <span data-ttu-id="746f1-117">Vyberte metriku `In-Memory OLTP Storage percentage`.</span><span class="sxs-lookup"><span data-stu-id="746f1-117">Then select the metric `In-Memory OLTP Storage percentage`.</span></span>
* <span data-ttu-id="746f1-118">Přidání oznámení, klikněte na pole využití prostředků a otevřete okno metriky a potom klikněte na Přidat upozornění.</span><span class="sxs-lookup"><span data-stu-id="746f1-118">To add an alert, click on the Resource Utilization box to open the Metric blade, then click on Add alert.</span></span>

<span data-ttu-id="746f1-119">Nebo použijte následující dotaz pro zobrazení využití úložiště v paměti:</span><span class="sxs-lookup"><span data-stu-id="746f1-119">Or use the following query to show the in-memory storage utilization:</span></span>

    SELECT xtp_storage_percent FROM sys.dm_db_resource_stats


## <a name="correct-out-of-memory-situations---error-41823"></a><span data-ttu-id="746f1-120">Opravte z důvodu nedostatku paměti situacích - chyba 41823</span><span class="sxs-lookup"><span data-stu-id="746f1-120">Correct out-of-memory situations - Error 41823</span></span>
<span data-ttu-id="746f1-121">V operacích INSERT, UPDATE a vytvořit selhání s chybovou zprávou 41823 spuštěna výsledky z důvodu nedostatku paměti.</span><span class="sxs-lookup"><span data-stu-id="746f1-121">Running out-of-memory results in INSERT, UPDATE, and CREATE operations failing with error message 41823.</span></span>

<span data-ttu-id="746f1-122">Chybová zpráva 41823 označuje, že paměťově optimalizované tabulky a proměnných tabulek, které překročily maximální velikost.</span><span class="sxs-lookup"><span data-stu-id="746f1-122">Error message 41823 indicates that the memory-optimized tables and table variables have exceeded the maximum size.</span></span>

<span data-ttu-id="746f1-123">Chcete tuto chybu vyřešit buď:</span><span class="sxs-lookup"><span data-stu-id="746f1-123">To resolve this error, either:</span></span>

* <span data-ttu-id="746f1-124">Odstranění dat z paměťově optimalizované tabulky potenciálně snižování zátěže dat do tabulky tradiční, založené na disku; nebo,</span><span class="sxs-lookup"><span data-stu-id="746f1-124">Delete data from the memory-optimized tables, potentially offloading the data to traditional, disk-based tables; or,</span></span>
* <span data-ttu-id="746f1-125">Upgradujte úroveň služby s dostatečně velké úložiště v paměti pro data, která je potřeba udržovat v paměťově optimalizovaných tabulkách.</span><span class="sxs-lookup"><span data-stu-id="746f1-125">Upgrade the service tier to one with enough in-memory storage for the data you need to keep in memory-optimized tables.</span></span>

## <a name="next-steps"></a><span data-ttu-id="746f1-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="746f1-126">Next steps</span></span>
<span data-ttu-id="746f1-127">Monitorování na pokyny najdete v části [monitorování databáze Azure SQL pomocí zobrazení dynamické správy](sql-database-monitoring-with-dmvs.md).</span><span class="sxs-lookup"><span data-stu-id="746f1-127">For monitoring guidance, see [Monitoring Azure SQL Database using dynamic management views](sql-database-monitoring-with-dmvs.md).</span></span>
