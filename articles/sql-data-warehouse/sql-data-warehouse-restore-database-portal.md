---
title: "Obnovení Azure SQL Data Warehouse (portál Azure) | Microsoft Docs"
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
ms.openlocfilehash: f6bc8671410dc7015a8d2a4bea1ba11f9ae526c3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="restore-azure-sql-data-warehouse-portal"></a><span data-ttu-id="149b0-103">Obnovení Azure SQL Data Warehouse (portál)</span><span class="sxs-lookup"><span data-stu-id="149b0-103">Restore Azure SQL Data Warehouse (portal)</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="149b0-104">[Přehled][Overview]</span><span class="sxs-lookup"><span data-stu-id="149b0-104">[Overview][Overview]</span></span>
> * <span data-ttu-id="149b0-105">[Portál][Portal]</span><span class="sxs-lookup"><span data-stu-id="149b0-105">[Portal][Portal]</span></span>
> * <span data-ttu-id="149b0-106">[Prostředí PowerShell][PowerShell]</span><span class="sxs-lookup"><span data-stu-id="149b0-106">[PowerShell][PowerShell]</span></span>
> * <span data-ttu-id="149b0-107">[REST][REST]</span><span class="sxs-lookup"><span data-stu-id="149b0-107">[REST][REST]</span></span>
>
>
<span data-ttu-id="149b0-108">V tomto článku se dozvíte, jak obnovit Azure SQL Data Warehouse pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="149b0-108">In this article, you will learn how to restore Azure SQL Data Warehouse by using the Azure portal.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="149b0-109">Než začnete</span><span class="sxs-lookup"><span data-stu-id="149b0-109">Before you begin</span></span>
<span data-ttu-id="149b0-110">**Ověření vaší DTU kapacity.**</span><span class="sxs-lookup"><span data-stu-id="149b0-110">**Verify your DTU capacity.**</span></span> <span data-ttu-id="149b0-111">Každá instance služby SQL Data Warehouse je hostitelem SQL serveru (například myserver.database.windows.net), který má výchozí kvótu jednotek (DTU) propustnost data.</span><span class="sxs-lookup"><span data-stu-id="149b0-111">Each instance of SQL Data Warehouse is hosted by a SQL server (for example, myserver.database.windows.net) which has a default data throughput unit (DTU) quota.</span></span> <span data-ttu-id="149b0-112">Před obnovením SQL Data Warehouse, ověřte, zda SQL server má dostatek zbývající kvóty DTU pro databázi, kterou jste obnovení.</span><span class="sxs-lookup"><span data-stu-id="149b0-112">Before you can restore SQL Data Warehouse, verify that your SQL server has enough remaining DTU quota for the database that you're restoring.</span></span> <span data-ttu-id="149b0-113">Informace o výpočtu kvóty DTU nebo požádejte o další Dtu najdete v tématu [žádosti o změnu kvóty DTU][Request a DTU quota change].</span><span class="sxs-lookup"><span data-stu-id="149b0-113">To learn how to calculate DTU quota or to request more DTUs, see [Request a DTU quota change][Request a DTU quota change].</span></span>

## <a name="restore-an-active-or-paused-database"></a><span data-ttu-id="149b0-114">Obnovit databázi active nebo pozastavena</span><span class="sxs-lookup"><span data-stu-id="149b0-114">Restore an active or paused database</span></span>
<span data-ttu-id="149b0-115">Chcete-li obnovit databázi:</span><span class="sxs-lookup"><span data-stu-id="149b0-115">To restore a database:</span></span>

1. <span data-ttu-id="149b0-116">Přihlaste se na web [Azure Portal][Azure portal].</span><span class="sxs-lookup"><span data-stu-id="149b0-116">Sign in to the [Azure portal][Azure portal].</span></span>
2. <span data-ttu-id="149b0-117">V levém podokně vyberte **Procházet**a potom vyberte **servery SQL**.</span><span class="sxs-lookup"><span data-stu-id="149b0-117">In the left pane, select **Browse**, and then select **SQL servers**.</span></span>

    ![Vyberte Procházet > servery SQL Server](./media/sql-data-warehouse-restore-database-portal/01-browse-for-sql-server.png)
3. <span data-ttu-id="149b0-119">Najít váš server a pak ho vyberte.</span><span class="sxs-lookup"><span data-stu-id="149b0-119">Find your server, and then select it.</span></span>

    ![Vyberte svůj server](./media/sql-data-warehouse-restore-database-portal/01-select-server.png)
4. <span data-ttu-id="149b0-121">Najít instanci služby SQL Data Warehouse, kterou chcete obnovit z a pak ho vyberte.</span><span class="sxs-lookup"><span data-stu-id="149b0-121">Find the instance of SQL Data Warehouse that you want to restore from, and then select it.</span></span>

    ![Vyberte instanci služby SQL Data Warehouse k obnovení](./media/sql-data-warehouse-restore-database-portal/01-select-active-dw.png)
5. <span data-ttu-id="149b0-123">V horní části okna datového skladu, vyberte **obnovení**.</span><span class="sxs-lookup"><span data-stu-id="149b0-123">At the top of the Data Warehouse blade, select **Restore**.</span></span>

    ![Vyberte možnost obnovení](./media/sql-data-warehouse-restore-database-portal/01-select-restore-from-active.png)
6. <span data-ttu-id="149b0-125">Zadejte novou **název databáze**.</span><span class="sxs-lookup"><span data-stu-id="149b0-125">Specify a new **Database name**.</span></span>
7. <span data-ttu-id="149b0-126">Vyberte nejnovější **bod obnovení**.</span><span class="sxs-lookup"><span data-stu-id="149b0-126">Select the latest **Restore point**.</span></span>

   <span data-ttu-id="149b0-127">Ujistěte se, že vyberete nejnovější bod obnovení.</span><span class="sxs-lookup"><span data-stu-id="149b0-127">Make sure you choose the latest restore point.</span></span> <span data-ttu-id="149b0-128">Protože body obnovení jsou uvedeny v koordinovaný světový čas (UTC), nemusí být výchozí možnost nejnovější bod obnovení.</span><span class="sxs-lookup"><span data-stu-id="149b0-128">Because restore points are shown in Coordinated Universal Time (UTC), the default option might not be the latest restore point.</span></span>

      ![Vyberte bod obnovení](./media/sql-data-warehouse-restore-database-portal/01-restore-blade-from-active.png)
8. <span data-ttu-id="149b0-130">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="149b0-130">Select **OK**.</span></span>
9. <span data-ttu-id="149b0-131">Bude zahájena v procesu obnovení databáze a můžete použít **oznámení** k monitorování procesu.</span><span class="sxs-lookup"><span data-stu-id="149b0-131">The database restore process will begin, and you can use **NOTIFICATIONS** to monitor the process.</span></span>

> [!NOTE]
> <span data-ttu-id="149b0-132">Po dokončení obnovení můžete nakonfigurovat obnovené databáze pomocí následujících [nakonfigurovat databázi po obnovení][Configure your database after recovery].</span><span class="sxs-lookup"><span data-stu-id="149b0-132">After the restore has finished, you can configure your recovered database by following [Configure your database after recovery][Configure your database after recovery].</span></span>
>
>

## <a name="restore-a-deleted-database"></a><span data-ttu-id="149b0-133">Obnovení odstraněné databáze</span><span class="sxs-lookup"><span data-stu-id="149b0-133">Restore a deleted database</span></span>
<span data-ttu-id="149b0-134">Chcete-li obnovit odstraněnou databázi:</span><span class="sxs-lookup"><span data-stu-id="149b0-134">To restore a deleted database:</span></span>

1. <span data-ttu-id="149b0-135">Přihlaste se na web [Azure Portal][Azure portal].</span><span class="sxs-lookup"><span data-stu-id="149b0-135">Sign in to the [Azure portal][Azure portal].</span></span>
2. <span data-ttu-id="149b0-136">V levém podokně vyberte **Procházet**a potom vyberte **servery SQL**.</span><span class="sxs-lookup"><span data-stu-id="149b0-136">In the left pane, select **Browse**, and then select **SQL servers**.</span></span>

    ![Vyberte Procházet > servery SQL Server](./media/sql-data-warehouse-restore-database-portal/01-browse-for-sql-server.png)
3. <span data-ttu-id="149b0-138">Najít váš server a pak ho vyberte.</span><span class="sxs-lookup"><span data-stu-id="149b0-138">Find your server, and then select it.</span></span>

    ![Vyberte svůj server](./media/sql-data-warehouse-restore-database-portal/02-select-server.png)
4. <span data-ttu-id="149b0-140">Přejděte dolů k položce **Operations** část v okně vašeho serveru.</span><span class="sxs-lookup"><span data-stu-id="149b0-140">Scroll down to the **Operations** section on your server's blade.</span></span>
5. <span data-ttu-id="149b0-141">Vyberte **odstranila databáze** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="149b0-141">Select the **Deleted databases** tile.</span></span>

    ![Vyberte dlaždici odstraněné databáze](./media/sql-data-warehouse-restore-database-portal/02-select-deleted-dws.png)
6. <span data-ttu-id="149b0-143">Vyberte, kterou chcete obnovit odstraněnou databázi.</span><span class="sxs-lookup"><span data-stu-id="149b0-143">Select the deleted database that you want to restore.</span></span>

    ![Vyberte databázi pro obnovení](./media/sql-data-warehouse-restore-database-portal/02-select-deleted-dw.png)
7. <span data-ttu-id="149b0-145">Zadejte novou **název databáze**.</span><span class="sxs-lookup"><span data-stu-id="149b0-145">Specify a new **Database name**.</span></span>

    ![Přidat název databáze](./media/sql-data-warehouse-restore-database-portal/02-restore-blade-from-deleted.png)
8. <span data-ttu-id="149b0-147">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="149b0-147">Select **OK**.</span></span>
9. <span data-ttu-id="149b0-148">Bude zahájena v procesu obnovení databáze a můžete použít **oznámení** k monitorování procesu.</span><span class="sxs-lookup"><span data-stu-id="149b0-148">The database restore process will begin, and you can use **NOTIFICATIONS** to monitor the process.</span></span>

> [!NOTE]
> <span data-ttu-id="149b0-149">Konfigurace databáze po dokončení obnovení najdete v tématu [nakonfigurovat databázi po obnovení][Configure your database after recovery].</span><span class="sxs-lookup"><span data-stu-id="149b0-149">To configure your database after the restore has finished, see [Configure your database after recovery][Configure your database after recovery].</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="149b0-150">Další kroky</span><span class="sxs-lookup"><span data-stu-id="149b0-150">Next steps</span></span>
<span data-ttu-id="149b0-151">Další informace o funkcích kontinuity obchodních edice Azure SQL Database, přečtěte si [Azure SQL Database obchodní kontinuity přehled][Azure SQL Database business continuity overview].</span><span class="sxs-lookup"><span data-stu-id="149b0-151">To learn about the business continuity features of Azure SQL Database editions, read the [Azure SQL Database business continuity overview][Azure SQL Database business continuity overview].</span></span>

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
