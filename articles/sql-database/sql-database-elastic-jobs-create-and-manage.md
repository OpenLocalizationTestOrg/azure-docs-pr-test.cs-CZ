---
title: "aaaManage skupiny databází Azure SQL | Microsoft Docs"
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
ms.openlocfilehash: a73c4fb24c332fae0e917c18272724cccd56f29a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-scaled-out-azure-sql-databases-using-elastic-jobs-preview"></a><span data-ttu-id="953b1-103">Vytvoření a správa škálovaný databází Azure SQL pomocí elastické úlohy (preview)</span><span class="sxs-lookup"><span data-stu-id="953b1-103">Create and manage scaled out Azure SQL Databases using elastic jobs (preview)</span></span>


<span data-ttu-id="953b1-104">**Elastické databáze úlohy** zjednodušení správy skupin databáze spuštěním operace správy, například změny schématu, Správa přihlašovacích údajů, aktualizace dat odkaz, shromažďování dat výkonu nebo telemetrie klienta (zákazníka) kolekce.</span><span class="sxs-lookup"><span data-stu-id="953b1-104">**Elastic Database jobs** simplify management of groups of databases by executing administrative operations such as schema changes, credentials management, reference data updates, performance data collection or tenant (customer) telemetry collection.</span></span> <span data-ttu-id="953b1-105">Úlohy elastické databáze je aktuálně k dispozici prostřednictvím hello portál Azure a rutin prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="953b1-105">Elastic Database jobs is currently available through hello Azure portal and PowerShell cmdlets.</span></span> <span data-ttu-id="953b1-106">Však hello Azure portálu povrchy omezené funkčnosti omezené tooexecution napříč všechny databáze [elastického fondu (preview)](sql-database-elastic-pool.md).</span><span class="sxs-lookup"><span data-stu-id="953b1-106">However, hello Azure portal surfaces reduced functionality limited tooexecution across all databases in an [elastic pool (preview)](sql-database-elastic-pool.md).</span></span> <span data-ttu-id="953b1-107">nastavit další funkce tooaccess a spouštění skriptů napříč skupinou databází, včetně vlastní definovanou kolekci nebo horizontálního oddílu (vytvořený [klientské knihovny pro elastické databáze](sql-database-elastic-scale-introduction.md)), najdete v části [vytváření a Správa úlohy pomocí prostředí PowerShell](sql-database-elastic-jobs-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="953b1-107">tooaccess additional features and execution of scripts across a group of databases including a custom-defined collection or a shard set (created using [Elastic Database client library](sql-database-elastic-scale-introduction.md)), see [Creating and managing jobs using PowerShell](sql-database-elastic-jobs-powershell.md).</span></span> <span data-ttu-id="953b1-108">Další informace o úlohách najdete v tématu [přehled úlohy elastické databáze](sql-database-elastic-jobs-overview.md).</span><span class="sxs-lookup"><span data-stu-id="953b1-108">For more information about jobs, see [Elastic Database jobs overview](sql-database-elastic-jobs-overview.md).</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="953b1-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="953b1-109">Prerequisites</span></span>
* <span data-ttu-id="953b1-110">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="953b1-110">An Azure subscription.</span></span> <span data-ttu-id="953b1-111">Bezplatná zkušební verze, najdete v části [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="953b1-111">For a free trial, see [Free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="953b1-112">Elastický fond.</span><span class="sxs-lookup"><span data-stu-id="953b1-112">An elastic pool.</span></span> <span data-ttu-id="953b1-113">V tématu [o elastické fondy](sql-database-elastic-pool.md).</span><span class="sxs-lookup"><span data-stu-id="953b1-113">See [About elastic pools](sql-database-elastic-pool.md).</span></span>
* <span data-ttu-id="953b1-114">Instalace součásti služby úlohy elastické databáze.</span><span class="sxs-lookup"><span data-stu-id="953b1-114">Installation of elastic database job service components.</span></span> <span data-ttu-id="953b1-115">V tématu [instalací služby úlohy elastické databáze hello](sql-database-elastic-jobs-service-installation.md).</span><span class="sxs-lookup"><span data-stu-id="953b1-115">See [Installing hello elastic database job service](sql-database-elastic-jobs-service-installation.md).</span></span>

## <a name="creating-jobs"></a><span data-ttu-id="953b1-116">Vytváření úloh</span><span class="sxs-lookup"><span data-stu-id="953b1-116">Creating jobs</span></span>
1. <span data-ttu-id="953b1-117">Pomocí hello [portál Azure](https://portal.azure.com), z existující fond elastické databáze úlohy, klikněte na tlačítko **vytvořit úlohu**.</span><span class="sxs-lookup"><span data-stu-id="953b1-117">Using hello [Azure portal](https://portal.azure.com), from an existing elastic database job pool, click **Create job**.</span></span>
2. <span data-ttu-id="953b1-118">Zadejte hello uživatelské jméno a heslo správce databáze hello (vytvořeny při instalaci úloh) pro databázi řízení úloh hello (metadata úložiště pro úlohy).</span><span class="sxs-lookup"><span data-stu-id="953b1-118">Type in hello username and password of hello database administrator (created at installation of Jobs) for hello jobs control database (metadata storage for jobs).</span></span>
   
    ![Název úlohy hello, zadejte nebo vložte kód a klikněte na tlačítko Spustit][1]
3. <span data-ttu-id="953b1-120">V hello **vytvoření úlohy** okno, zadejte název pro úlohu hello.</span><span class="sxs-lookup"><span data-stu-id="953b1-120">In hello **Create Job** blade, type a name for hello job.</span></span>
4. <span data-ttu-id="953b1-121">Zadejte hello uživatelské jméno a heslo tooconnect toohello cílovým databázím s dostatečnými oprávněními toosucceed provádění skriptu.</span><span class="sxs-lookup"><span data-stu-id="953b1-121">Type hello user name and password tooconnect toohello target databases with sufficient permissions for script execution toosucceed.</span></span>
5. <span data-ttu-id="953b1-122">Vložit nebo zadejte ve skriptu hello T-SQL.</span><span class="sxs-lookup"><span data-stu-id="953b1-122">Paste or type in hello T-SQL script.</span></span>
6. <span data-ttu-id="953b1-123">Klikněte na tlačítko **Uložit** a pak klikněte na **spustit**.</span><span class="sxs-lookup"><span data-stu-id="953b1-123">Click **Save** and then click **Run**.</span></span>
   
    ![Vytvoření úlohy a spuštění][5]

## <a name="run-idempotent-jobs"></a><span data-ttu-id="953b1-125">Spuštění úloh idempotent</span><span class="sxs-lookup"><span data-stu-id="953b1-125">Run idempotent jobs</span></span>
<span data-ttu-id="953b1-126">Při spuštění skriptu pro sadu databází, musí být jisti, že skript hello je idempotent.</span><span class="sxs-lookup"><span data-stu-id="953b1-126">When you run a script against a set of databases, you must be sure that hello script is idempotent.</span></span> <span data-ttu-id="953b1-127">To znamená, hello skriptu musí být schopný toorun vícekrát, i v případě, že se nezdařil před nekompletnímu stavu.</span><span class="sxs-lookup"><span data-stu-id="953b1-127">That is, hello script must be able toorun multiple times, even if it has failed before in an incomplete state.</span></span> <span data-ttu-id="953b1-128">Například pokud skript selže, hello úlohy bude automaticky opakovat, dokud neproběhne úspěšně (v rámci omezení, jako hello opakování logiku nakonec přestanou hello opakování).</span><span class="sxs-lookup"><span data-stu-id="953b1-128">For example, when a script fails, hello job will be automatically retried until it succeeds (within limits, as hello retry logic will eventually cease hello retrying).</span></span> <span data-ttu-id="953b1-129">toodo Hello způsob, jak je to toouse hello klauzuli "Pokud existuje" a odstranit všechny nalezen instanci před vytvořením nového objektu.</span><span class="sxs-lookup"><span data-stu-id="953b1-129">hello way toodo this is toouse hello an "IF EXISTS" clause and delete any found instance before creating a new object.</span></span> <span data-ttu-id="953b1-130">Zde je uveden příklad:</span><span class="sxs-lookup"><span data-stu-id="953b1-130">An example is shown here:</span></span>

    IF EXISTS (SELECT name FROM sys.indexes
            WHERE name = N'IX_ProductVendor_VendorID')
    DROP INDEX IX_ProductVendor_VendorID ON Purchasing.ProductVendor;
    GO
    CREATE INDEX IX_ProductVendor_VendorID
    ON Purchasing.ProductVendor (VendorID);

<span data-ttu-id="953b1-131">Můžete taky použijte klauzuli "Pokud není existuje" před vytvořením nové instance:</span><span class="sxs-lookup"><span data-stu-id="953b1-131">Alternatively, use an "IF NOT EXISTS" clause before creating a new instance:</span></span>

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

<span data-ttu-id="953b1-132">Tento skript potom aktualizuje hello tabulky, které jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="953b1-132">This script then updates hello table created previously.</span></span>

    IF NOT EXISTS (SELECT columns.name FROM sys.columns INNER JOIN sys.tables on columns.object_id = tables.object_id WHERE tables.name = 'TestTable' AND columns.name = 'AdditionalInformation')
    BEGIN

    ALTER TABLE TestTable

    ADD AdditionalInformation NVARCHAR(400);
    END
    GO

    INSERT INTO TestTable(InsertionTime, AdditionalInformation) VALUES (sysutcdatetime(), 'test');
    GO


## <a name="checking-job-status"></a><span data-ttu-id="953b1-133">Kontrola stavu úloh</span><span class="sxs-lookup"><span data-stu-id="953b1-133">Checking job status</span></span>
<span data-ttu-id="953b1-134">Po zahájení úlohy, můžete zkontrolovat v jeho průběhu.</span><span class="sxs-lookup"><span data-stu-id="953b1-134">After a job has begun, you can check on its progress.</span></span>

1. <span data-ttu-id="953b1-135">Na stránce hello elastického fondu, klikněte na tlačítko **spravovat úlohy**.</span><span class="sxs-lookup"><span data-stu-id="953b1-135">From hello elastic pool page, click **Manage jobs**.</span></span>
   
    ![Klikněte na tlačítko "Spravovat úlohy"][2]
2. <span data-ttu-id="953b1-137">Klikněte na hello název (a) úlohy.</span><span class="sxs-lookup"><span data-stu-id="953b1-137">Click on hello name (a) of a job.</span></span> <span data-ttu-id="953b1-138">Hello **stav** může být "Dokončeno" nebo "Se nezdařilo."</span><span class="sxs-lookup"><span data-stu-id="953b1-138">hello **STATUS** can be "Completed" or "Failed."</span></span> <span data-ttu-id="953b1-139">s jeho datum a čas vytvoření a spuštění zobrazují podrobnosti úlohy Hello (b).</span><span class="sxs-lookup"><span data-stu-id="953b1-139">hello job's details appear (b) with its date and time of creation and running.</span></span> <span data-ttu-id="953b1-140">Hello seznamu hello zobrazující průběh hello hello skriptu na každou databázi ve fondu hello poskytnutí podrobnosti datum a čas (c).</span><span class="sxs-lookup"><span data-stu-id="953b1-140">hello list (c) below hello that shows hello progress of hello script against each database in hello pool, giving its date and time details.</span></span>
   
    ![Kontrola dokončení úlohy][3]

## <a name="checking-failed-jobs"></a><span data-ttu-id="953b1-142">Kontrola neúspěšné úlohy</span><span class="sxs-lookup"><span data-stu-id="953b1-142">Checking failed jobs</span></span>
<span data-ttu-id="953b1-143">Pokud úloha selže, můžete najít protokolu jeho spuštění.</span><span class="sxs-lookup"><span data-stu-id="953b1-143">If a job fails, a log of its execution can found.</span></span> <span data-ttu-id="953b1-144">Klikněte na tlačítko hello název úlohy toosee hello se nezdařilo její podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="953b1-144">Click hello name of hello failed job toosee its details.</span></span>

![Podívejte se neúspěšnou úlohu][4]

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-create-and-manage/screen-1.png
[2]: ./media/sql-database-elastic-jobs-create-and-manage/click-manage-jobs.png
[3]: ./media/sql-database-elastic-jobs-create-and-manage/running-jobs.png
[4]: ./media/sql-database-elastic-jobs-create-and-manage/failed.png
[5]: ./media/sql-database-elastic-jobs-create-and-manage/screen-2.png


