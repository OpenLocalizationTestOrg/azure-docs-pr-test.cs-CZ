---
title: "aaaRestore Azure SQL Data Warehouse (portál Azure) | Microsoft Docs"
description: "Azure portálu úlohy pro obnovení Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: Lakshmi1812
manager: barbkess
editor: 
ms.assetid: b0aef539-7657-4b0e-9899-74098f5c21bc
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: backup-restore
ms.date: 09/21/2016
ms.author: lakshmir;barbkess
ms.openlocfilehash: cb225d2a21b61acab70a51b69c266f8d3ffacc9a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="restore-azure-sql-data-warehouse-portal"></a><span data-ttu-id="11153-103">Obnovení Azure SQL Data Warehouse (portál)</span><span class="sxs-lookup"><span data-stu-id="11153-103">Restore Azure SQL Data Warehouse (portal)</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="11153-104">[Přehled][Overview]</span><span class="sxs-lookup"><span data-stu-id="11153-104">[Overview][Overview]</span></span>
> * <span data-ttu-id="11153-105">[Portál][Portal]</span><span class="sxs-lookup"><span data-stu-id="11153-105">[Portal][Portal]</span></span>
> * <span data-ttu-id="11153-106">[Prostředí PowerShell][PowerShell]</span><span class="sxs-lookup"><span data-stu-id="11153-106">[PowerShell][PowerShell]</span></span>
> * <span data-ttu-id="11153-107">[REST][REST]</span><span class="sxs-lookup"><span data-stu-id="11153-107">[REST][REST]</span></span>
>
>
<span data-ttu-id="11153-108">V tomto článku se dozvíte, jak hello toorestore Azure SQL Data Warehouse pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="11153-108">In this article, you will learn how toorestore Azure SQL Data Warehouse by using hello Azure portal.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="11153-109">Než začnete</span><span class="sxs-lookup"><span data-stu-id="11153-109">Before you begin</span></span>
<span data-ttu-id="11153-110">**Ověření vaší DTU kapacity.**</span><span class="sxs-lookup"><span data-stu-id="11153-110">**Verify your DTU capacity.**</span></span> <span data-ttu-id="11153-111">Každá instance služby SQL Data Warehouse je hostitelem SQL serveru (například myserver.database.windows.net), který má výchozí kvótu jednotek (DTU) propustnost data.</span><span class="sxs-lookup"><span data-stu-id="11153-111">Each instance of SQL Data Warehouse is hosted by a SQL server (for example, myserver.database.windows.net) which has a default data throughput unit (DTU) quota.</span></span> <span data-ttu-id="11153-112">Před obnovením SQL Data Warehouse, ověřte, zda má váš server SQL dostatečně zbývající kvóty DTU pro hello databázi, která jste obnovení.</span><span class="sxs-lookup"><span data-stu-id="11153-112">Before you can restore SQL Data Warehouse, verify that your SQL server has enough remaining DTU quota for hello database that you're restoring.</span></span> <span data-ttu-id="11153-113">toolearn jak kvóty DTU toocalculate nebo toorequest více Dtu, najdete v části [žádosti o změnu kvóty DTU][Request a DTU quota change].</span><span class="sxs-lookup"><span data-stu-id="11153-113">toolearn how toocalculate DTU quota or toorequest more DTUs, see [Request a DTU quota change][Request a DTU quota change].</span></span>

## <a name="restore-an-active-or-paused-database"></a><span data-ttu-id="11153-114">Obnovit databázi active nebo pozastavena</span><span class="sxs-lookup"><span data-stu-id="11153-114">Restore an active or paused database</span></span>
<span data-ttu-id="11153-115">toorestore databáze:</span><span class="sxs-lookup"><span data-stu-id="11153-115">toorestore a database:</span></span>

1. <span data-ttu-id="11153-116">Přihlaste se toohello [portál Azure][Azure portal].</span><span class="sxs-lookup"><span data-stu-id="11153-116">Sign in toohello [Azure portal][Azure portal].</span></span>
2. <span data-ttu-id="11153-117">V levém podokně hello vyberte **Procházet**a potom vyberte **servery SQL**.</span><span class="sxs-lookup"><span data-stu-id="11153-117">In hello left pane, select **Browse**, and then select **SQL servers**.</span></span>

    ![Vyberte Procházet > servery SQL Server](./media/sql-data-warehouse-restore-database-portal/01-browse-for-sql-server.png)
3. <span data-ttu-id="11153-119">Najít váš server a pak ho vyberte.</span><span class="sxs-lookup"><span data-stu-id="11153-119">Find your server, and then select it.</span></span>

    ![Vyberte svůj server](./media/sql-data-warehouse-restore-database-portal/01-select-server.png)
4. <span data-ttu-id="11153-121">Najdete hello instanci služby SQL Data Warehouse má toorestore z a pak ho vyberte.</span><span class="sxs-lookup"><span data-stu-id="11153-121">Find hello instance of SQL Data Warehouse that you want toorestore from, and then select it.</span></span>

    ![Vyberte hello instanci služby SQL Data Warehouse toorestore](./media/sql-data-warehouse-restore-database-portal/01-select-active-dw.png)
5. <span data-ttu-id="11153-123">Hello horní části okna hello datového skladu, vyberte **obnovení**.</span><span class="sxs-lookup"><span data-stu-id="11153-123">At hello top of hello Data Warehouse blade, select **Restore**.</span></span>

    ![Vyberte možnost obnovení](./media/sql-data-warehouse-restore-database-portal/01-select-restore-from-active.png)
6. <span data-ttu-id="11153-125">Zadejte novou **název databáze**.</span><span class="sxs-lookup"><span data-stu-id="11153-125">Specify a new **Database name**.</span></span>
7. <span data-ttu-id="11153-126">Vyberte hello nejnovější **bod obnovení**.</span><span class="sxs-lookup"><span data-stu-id="11153-126">Select hello latest **Restore point**.</span></span>

   <span data-ttu-id="11153-127">Ujistěte se, že si vyberete hello nejnovější bod obnovení.</span><span class="sxs-lookup"><span data-stu-id="11153-127">Make sure you choose hello latest restore point.</span></span> <span data-ttu-id="11153-128">Protože body obnovení jsou uvedeny v koordinovaný světový čas (UTC), nemusí být hello výchozí možnost hello nejnovější bod obnovení.</span><span class="sxs-lookup"><span data-stu-id="11153-128">Because restore points are shown in Coordinated Universal Time (UTC), hello default option might not be hello latest restore point.</span></span>

      ![Vyberte bod obnovení](./media/sql-data-warehouse-restore-database-portal/01-restore-blade-from-active.png)
8. <span data-ttu-id="11153-130">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="11153-130">Select **OK**.</span></span>
9. <span data-ttu-id="11153-131">proces obnovení databáze Hello zahájíte, a můžete použít **oznámení** toomonitor hello procesu.</span><span class="sxs-lookup"><span data-stu-id="11153-131">hello database restore process will begin, and you can use **NOTIFICATIONS** toomonitor hello process.</span></span>

> [!NOTE]
> <span data-ttu-id="11153-132">Po dokončení obnovení hello obnovené databáze můžete nakonfigurovat pomocí následujících [nakonfigurovat databázi po obnovení][Configure your database after recovery].</span><span class="sxs-lookup"><span data-stu-id="11153-132">After hello restore has finished, you can configure your recovered database by following [Configure your database after recovery][Configure your database after recovery].</span></span>
>
>

## <a name="restore-a-deleted-database"></a><span data-ttu-id="11153-133">Obnovení odstraněné databáze</span><span class="sxs-lookup"><span data-stu-id="11153-133">Restore a deleted database</span></span>
<span data-ttu-id="11153-134">toorestore odstraněné databáze:</span><span class="sxs-lookup"><span data-stu-id="11153-134">toorestore a deleted database:</span></span>

1. <span data-ttu-id="11153-135">Přihlaste se toohello [portál Azure][Azure portal].</span><span class="sxs-lookup"><span data-stu-id="11153-135">Sign in toohello [Azure portal][Azure portal].</span></span>
2. <span data-ttu-id="11153-136">V levém podokně hello vyberte **Procházet**a potom vyberte **servery SQL**.</span><span class="sxs-lookup"><span data-stu-id="11153-136">In hello left pane, select **Browse**, and then select **SQL servers**.</span></span>

    ![Vyberte Procházet > servery SQL Server](./media/sql-data-warehouse-restore-database-portal/01-browse-for-sql-server.png)
3. <span data-ttu-id="11153-138">Najít váš server a pak ho vyberte.</span><span class="sxs-lookup"><span data-stu-id="11153-138">Find your server, and then select it.</span></span>

    ![Vyberte svůj server](./media/sql-data-warehouse-restore-database-portal/02-select-server.png)
4. <span data-ttu-id="11153-140">Projděte dolů toohello **Operations** část v okně vašeho serveru.</span><span class="sxs-lookup"><span data-stu-id="11153-140">Scroll down toohello **Operations** section on your server's blade.</span></span>
5. <span data-ttu-id="11153-141">Vyberte hello **odstranila databáze** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="11153-141">Select hello **Deleted databases** tile.</span></span>

    ![Vyberte dlaždici databáze odstraněné hello](./media/sql-data-warehouse-restore-database-portal/02-select-deleted-dws.png)
6. <span data-ttu-id="11153-143">Vyberte hello odstranit databázi, které chcete toorestore.</span><span class="sxs-lookup"><span data-stu-id="11153-143">Select hello deleted database that you want toorestore.</span></span>

    ![Vyberte databázi toorestore](./media/sql-data-warehouse-restore-database-portal/02-select-deleted-dw.png)
7. <span data-ttu-id="11153-145">Zadejte novou **název databáze**.</span><span class="sxs-lookup"><span data-stu-id="11153-145">Specify a new **Database name**.</span></span>

    ![Přidejte název databáze hello](./media/sql-data-warehouse-restore-database-portal/02-restore-blade-from-deleted.png)
8. <span data-ttu-id="11153-147">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="11153-147">Select **OK**.</span></span>
9. <span data-ttu-id="11153-148">proces obnovení databáze Hello zahájíte, a můžete použít **oznámení** toomonitor hello procesu.</span><span class="sxs-lookup"><span data-stu-id="11153-148">hello database restore process will begin, and you can use **NOTIFICATIONS** toomonitor hello process.</span></span>

> [!NOTE]
> <span data-ttu-id="11153-149">v tématu vaše databáze po dokončení obnovení hello tooconfigure [nakonfigurovat databázi po obnovení][Configure your database after recovery].</span><span class="sxs-lookup"><span data-stu-id="11153-149">tooconfigure your database after hello restore has finished, see [Configure your database after recovery][Configure your database after recovery].</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="11153-150">Další kroky</span><span class="sxs-lookup"><span data-stu-id="11153-150">Next steps</span></span>
<span data-ttu-id="11153-151">toolearn o hello firmy kontinuitu podnikových procesů jednotlivých edice Azure SQL Database, přečtěte si hello [Azure SQL Database obchodní kontinuity přehled][Azure SQL Database business continuity overview].</span><span class="sxs-lookup"><span data-stu-id="11153-151">toolearn about hello business continuity features of Azure SQL Database editions, read hello [Azure SQL Database business continuity overview][Azure SQL Database business continuity overview].</span></span>

<!--Image references-->

<!--Article references-->
[Azure SQL Database business continuity overview]: ../sql-database/sql-database-business-continuity.md
[Overview]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[REST]: ./sql-data-warehouse-restore-database-rest-api.md
[Configure your database after recovery]: ../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery
[Request a DTU quota change]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change

<!--MSDN references-->

<!--Blog references-->

<!--Other Web references-->
[Azure portal]: https://portal.azure.com/
