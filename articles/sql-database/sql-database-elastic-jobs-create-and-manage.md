---
title: "Spravovat skupiny databází Azure SQL | Microsoft Docs"
description: "Provede procesem vytváření a správa elastické úlohy."
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: f858344d-085b-4022-935e-1b5fa20adbac
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: d30cc74778e0b36dd632c2f040ce80d1ca8af5a5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-and-manage-scaled-out-azure-sql-databases-using-elastic-jobs-preview"></a><span data-ttu-id="e4b55-103">Vytvoření a správa škálovaný databází Azure SQL pomocí elastické úlohy (preview)</span><span class="sxs-lookup"><span data-stu-id="e4b55-103">Create and manage scaled out Azure SQL Databases using elastic jobs (preview)</span></span>


<span data-ttu-id="e4b55-104">**Elastické databáze úlohy** zjednodušení správy skupin databáze spuštěním operace správy, například změny schématu, Správa přihlašovacích údajů, aktualizace dat odkaz, shromažďování dat výkonu nebo telemetrie klienta (zákazníka) kolekce.</span><span class="sxs-lookup"><span data-stu-id="e4b55-104">**Elastic Database jobs** simplify management of groups of databases by executing administrative operations such as schema changes, credentials management, reference data updates, performance data collection or tenant (customer) telemetry collection.</span></span> <span data-ttu-id="e4b55-105">Úlohy elastické databáze je aktuálně k dispozici prostřednictvím portálu Azure a rutin prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e4b55-105">Elastic Database jobs is currently available through the Azure portal and PowerShell cmdlets.</span></span> <span data-ttu-id="e4b55-106">Však Azure portálu povrchy snížit funkce omezené na provádění mezi všechny databáze [elastického fondu (preview)](sql-database-elastic-pool.md).</span><span class="sxs-lookup"><span data-stu-id="e4b55-106">However, the Azure portal surfaces reduced functionality limited to execution across all databases in an [elastic pool (preview)](sql-database-elastic-pool.md).</span></span> <span data-ttu-id="e4b55-107">Pro přístup k další funkce a spouštění skriptů napříč skupinou databází, včetně vlastní definovanou kolekci nebo horizontálního oddílu nastavit (vytvořený [klientské knihovny pro elastické databáze](sql-database-elastic-scale-introduction.md)), najdete v části [vytváření a Správa úlohy pomocí prostředí PowerShell](sql-database-elastic-jobs-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="e4b55-107">To access additional features and execution of scripts across a group of databases including a custom-defined collection or a shard set (created using [Elastic Database client library](sql-database-elastic-scale-introduction.md)), see [Creating and managing jobs using PowerShell](sql-database-elastic-jobs-powershell.md).</span></span> <span data-ttu-id="e4b55-108">Další informace o úlohách najdete v tématu [přehled úlohy elastické databáze](sql-database-elastic-jobs-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e4b55-108">For more information about jobs, see [Elastic Database jobs overview](sql-database-elastic-jobs-overview.md).</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="e4b55-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e4b55-109">Prerequisites</span></span>
* <span data-ttu-id="e4b55-110">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="e4b55-110">An Azure subscription.</span></span> <span data-ttu-id="e4b55-111">Bezplatná zkušební verze, najdete v části [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e4b55-111">For a free trial, see [Free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="e4b55-112">Elastický fond.</span><span class="sxs-lookup"><span data-stu-id="e4b55-112">An elastic pool.</span></span> <span data-ttu-id="e4b55-113">V tématu [o elastické fondy](sql-database-elastic-pool.md).</span><span class="sxs-lookup"><span data-stu-id="e4b55-113">See [About elastic pools](sql-database-elastic-pool.md).</span></span>
* <span data-ttu-id="e4b55-114">Instalace součásti služby úlohy elastické databáze.</span><span class="sxs-lookup"><span data-stu-id="e4b55-114">Installation of elastic database job service components.</span></span> <span data-ttu-id="e4b55-115">V tématu [nainstalovat službu úlohy elastické databáze](sql-database-elastic-jobs-service-installation.md).</span><span class="sxs-lookup"><span data-stu-id="e4b55-115">See [Installing the elastic database job service](sql-database-elastic-jobs-service-installation.md).</span></span>

## <a name="creating-jobs"></a><span data-ttu-id="e4b55-116">Vytváření úloh</span><span class="sxs-lookup"><span data-stu-id="e4b55-116">Creating jobs</span></span>
1. <span data-ttu-id="e4b55-117">Pomocí [portál Azure](https://portal.azure.com), z existující fond elastické databáze úlohy, klikněte na tlačítko **vytvořit úlohu**.</span><span class="sxs-lookup"><span data-stu-id="e4b55-117">Using the [Azure portal](https://portal.azure.com), from an existing elastic database job pool, click **Create job**.</span></span>
2. <span data-ttu-id="e4b55-118">Zadejte uživatelské jméno a heslo správce databáze (vytvořeny při instalaci úloh) pro databázi řízení úloh (metadata úložiště pro úlohy).</span><span class="sxs-lookup"><span data-stu-id="e4b55-118">Type in the username and password of the database administrator (created at installation of Jobs) for the jobs control database (metadata storage for jobs).</span></span>
   
    ![Název úlohy, zadejte nebo vložte kód a klikněte na tlačítko Spustit][1]
3. <span data-ttu-id="e4b55-120">V **vytvoření úlohy** okno, zadejte název pro úlohu.</span><span class="sxs-lookup"><span data-stu-id="e4b55-120">In the **Create Job** blade, type a name for the job.</span></span>
4. <span data-ttu-id="e4b55-121">Zadejte uživatelské jméno a heslo pro připojení k cílovým databázím s dostatečnými oprávněními pro spuštění skriptu proběhla úspěšně.</span><span class="sxs-lookup"><span data-stu-id="e4b55-121">Type the user name and password to connect to the target databases with sufficient permissions for script execution to succeed.</span></span>
5. <span data-ttu-id="e4b55-122">Vložení nebo typu ve skriptu T-SQL.</span><span class="sxs-lookup"><span data-stu-id="e4b55-122">Paste or type in the T-SQL script.</span></span>
6. <span data-ttu-id="e4b55-123">Klikněte na tlačítko **Uložit** a pak klikněte na **spustit**.</span><span class="sxs-lookup"><span data-stu-id="e4b55-123">Click **Save** and then click **Run**.</span></span>
   
    ![Vytvoření úlohy a spuštění][5]

## <a name="run-idempotent-jobs"></a><span data-ttu-id="e4b55-125">Spuštění úloh idempotent</span><span class="sxs-lookup"><span data-stu-id="e4b55-125">Run idempotent jobs</span></span>
<span data-ttu-id="e4b55-126">Při spuštění skriptu pro sadu databází, musí být jisti, že skript idempotent.</span><span class="sxs-lookup"><span data-stu-id="e4b55-126">When you run a script against a set of databases, you must be sure that the script is idempotent.</span></span> <span data-ttu-id="e4b55-127">Skript tedy musí být možné spustit vícekrát, i v případě, že se nezdařil před nekompletnímu stavu.</span><span class="sxs-lookup"><span data-stu-id="e4b55-127">That is, the script must be able to run multiple times, even if it has failed before in an incomplete state.</span></span> <span data-ttu-id="e4b55-128">Například pokud skript selže, úloha bude automaticky opakovat, dokud neproběhne úspěšně (omezení, jako opakovat logiku nakonec přestanou opakování).</span><span class="sxs-lookup"><span data-stu-id="e4b55-128">For example, when a script fails, the job will be automatically retried until it succeeds (within limits, as the retry logic will eventually cease the retrying).</span></span> <span data-ttu-id="e4b55-129">Způsob k tomu je použít klauzuli "Pokud existuje" a odstranit všechny nalezen instanci před vytvořením nového objektu.</span><span class="sxs-lookup"><span data-stu-id="e4b55-129">The way to do this is to use the an "IF EXISTS" clause and delete any found instance before creating a new object.</span></span> <span data-ttu-id="e4b55-130">Zde je uveden příklad:</span><span class="sxs-lookup"><span data-stu-id="e4b55-130">An example is shown here:</span></span>

    IF EXISTS (SELECT name FROM sys.indexes
            WHERE name = N'IX_ProductVendor_VendorID')
    DROP INDEX IX_ProductVendor_VendorID ON Purchasing.ProductVendor;
    GO
    CREATE INDEX IX_ProductVendor_VendorID
    ON Purchasing.ProductVendor (VendorID);

<span data-ttu-id="e4b55-131">Můžete taky použijte klauzuli "Pokud není existuje" před vytvořením nové instance:</span><span class="sxs-lookup"><span data-stu-id="e4b55-131">Alternatively, use an "IF NOT EXISTS" clause before creating a new instance:</span></span>

    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'TestTable')
    BEGIN
     CREATE TABLE TestTable(
      TestTableId INT PRIMARY KEY IDENTITY,
      InsertionTime DATETIME2
     );
    END
    GO

    INSERT INTO TestTable(InsertionTime) VALUES (sysutcdatetime());
    GO

<span data-ttu-id="e4b55-132">Tento skript potom aktualizuje v tabulce, které jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="e4b55-132">This script then updates the table created previously.</span></span>

    IF NOT EXISTS (SELECT columns.name FROM sys.columns INNER JOIN sys.tables on columns.object_id = tables.object_id WHERE tables.name = 'TestTable' AND columns.name = 'AdditionalInformation')
    BEGIN

    ALTER TABLE TestTable

    ADD AdditionalInformation NVARCHAR(400);
    END
    GO

    INSERT INTO TestTable(InsertionTime, AdditionalInformation) VALUES (sysutcdatetime(), 'test');
    GO


## <a name="checking-job-status"></a><span data-ttu-id="e4b55-133">Kontrola stavu úloh</span><span class="sxs-lookup"><span data-stu-id="e4b55-133">Checking job status</span></span>
<span data-ttu-id="e4b55-134">Po zahájení úlohy, můžete zkontrolovat v jeho průběhu.</span><span class="sxs-lookup"><span data-stu-id="e4b55-134">After a job has begun, you can check on its progress.</span></span>

1. <span data-ttu-id="e4b55-135">Na stránce elastického fondu, klikněte na tlačítko **spravovat úlohy**.</span><span class="sxs-lookup"><span data-stu-id="e4b55-135">From the elastic pool page, click **Manage jobs**.</span></span>
   
    ![Klikněte na tlačítko "Spravovat úlohy"][2]
2. <span data-ttu-id="e4b55-137">Klikněte na název (a) úlohy.</span><span class="sxs-lookup"><span data-stu-id="e4b55-137">Click on the name (a) of a job.</span></span> <span data-ttu-id="e4b55-138">**Stav** může být "Dokončeno" nebo "Se nezdařilo."</span><span class="sxs-lookup"><span data-stu-id="e4b55-138">The **STATUS** can be "Completed" or "Failed."</span></span> <span data-ttu-id="e4b55-139">S jeho datum a čas vytvoření a spuštění zobrazují podrobnosti úlohy (b).</span><span class="sxs-lookup"><span data-stu-id="e4b55-139">The job's details appear (b) with its date and time of creation and running.</span></span> <span data-ttu-id="e4b55-140">V seznamu (c) níže se, že zobrazí průběh skriptu na každou databázi ve fondu, poskytne jeho podrobnosti datum a čas.</span><span class="sxs-lookup"><span data-stu-id="e4b55-140">The list (c) below the that shows the progress of the script against each database in the pool, giving its date and time details.</span></span>
   
    ![Kontrola dokončení úlohy][3]

## <a name="checking-failed-jobs"></a><span data-ttu-id="e4b55-142">Kontrola neúspěšné úlohy</span><span class="sxs-lookup"><span data-stu-id="e4b55-142">Checking failed jobs</span></span>
<span data-ttu-id="e4b55-143">Pokud úloha selže, můžete najít protokolu jeho spuštění.</span><span class="sxs-lookup"><span data-stu-id="e4b55-143">If a job fails, a log of its execution can found.</span></span> <span data-ttu-id="e4b55-144">Klikněte na název zobrazíte její podrobnosti o neúspěšné úloze.</span><span class="sxs-lookup"><span data-stu-id="e4b55-144">Click the name of the failed job to see its details.</span></span>

![Podívejte se neúspěšnou úlohu][4]

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-create-and-manage/screen-1.png
[2]: ./media/sql-database-elastic-jobs-create-and-manage/click-manage-jobs.png
[3]: ./media/sql-database-elastic-jobs-create-and-manage/running-jobs.png
[4]: ./media/sql-database-elastic-jobs-create-and-manage/failed.png
[5]: ./media/sql-database-elastic-jobs-create-and-manage/screen-2.png


