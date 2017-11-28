---
title: aaaRestore Azure SQL Data Warehouse (REST API) | Microsoft Docs
description: "Úlohy REST API pro obnovení Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: Lakshmi1812
manager: jhubbard
editor: 
ms.assetid: fca922c6-b675-49c7-907e-5dcf26d451dd
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: backup-restore
ms.date: 10/31/2016
ms.author: lakshmir;barbkess
ms.openlocfilehash: cf6678d71aafff71b1ea715f447e41e25f20d1b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="restore-an-azure-sql-data-warehouse-rest-api"></a><span data-ttu-id="f5e7b-103">Obnovit Azure SQL Data Warehouse (REST API)</span><span class="sxs-lookup"><span data-stu-id="f5e7b-103">Restore an Azure SQL Data Warehouse (REST API)</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="f5e7b-104">[Přehled][Overview]</span><span class="sxs-lookup"><span data-stu-id="f5e7b-104">[Overview][Overview]</span></span>
> * <span data-ttu-id="f5e7b-105">[Portál][Portal]</span><span class="sxs-lookup"><span data-stu-id="f5e7b-105">[Portal][Portal]</span></span>
> * <span data-ttu-id="f5e7b-106">[Prostředí PowerShell][PowerShell]</span><span class="sxs-lookup"><span data-stu-id="f5e7b-106">[PowerShell][PowerShell]</span></span>
> * <span data-ttu-id="f5e7b-107">[REST][REST]</span><span class="sxs-lookup"><span data-stu-id="f5e7b-107">[REST][REST]</span></span>
> 
> 

<span data-ttu-id="f5e7b-108">V tomto článku se dozvíte, jak hello toorestore k Azure SQL Data Warehouse pomocí rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="f5e7b-108">In this article you will learn how toorestore an Azure SQL Data Warehouse using hello REST API.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="f5e7b-109">Než začnete</span><span class="sxs-lookup"><span data-stu-id="f5e7b-109">Before you begin</span></span>
<span data-ttu-id="f5e7b-110">**Ověření vaší DTU kapacity.**</span><span class="sxs-lookup"><span data-stu-id="f5e7b-110">**Verify your DTU capacity.**</span></span> <span data-ttu-id="f5e7b-111">Každý datový sklad SQL je hostitelem serveru SQL (např. myserver.database.windows.net), který má kvóty DTU.</span><span class="sxs-lookup"><span data-stu-id="f5e7b-111">Each SQL Data Warehouse is hosted by a SQL server (e.g. myserver.database.windows.net) which has a default DTU quota.</span></span>  <span data-ttu-id="f5e7b-112">Před obnovením SQL Data Warehouse, ověřte, že hello systému SQL server má dostatek zbývající kvóty DTU pro hello databáze obnovena.</span><span class="sxs-lookup"><span data-stu-id="f5e7b-112">Before you can restore a SQL Data Warehouse, verify that hello your SQL server has enough remaining DTU quota for hello database being restored.</span></span> <span data-ttu-id="f5e7b-113">toolearn jak toocalculate DTU potřebná nebo toorequest více DTU, najdete v části [žádosti o změnu kvóty DTU][Request a DTU quota change].</span><span class="sxs-lookup"><span data-stu-id="f5e7b-113">toolearn how toocalculate DTU needed or toorequest more DTU, see [Request a DTU quota change][Request a DTU quota change].</span></span>

## <a name="restore-an-active-or-paused-database"></a><span data-ttu-id="f5e7b-114">Obnovit databázi active nebo pozastavena</span><span class="sxs-lookup"><span data-stu-id="f5e7b-114">Restore an active or paused database</span></span>
<span data-ttu-id="f5e7b-115">toorestore databáze:</span><span class="sxs-lookup"><span data-stu-id="f5e7b-115">toorestore a database:</span></span>

1. <span data-ttu-id="f5e7b-116">Získání seznamu hello bodů obnovení databáze pomocí operace Get body obnovení databáze hello.</span><span class="sxs-lookup"><span data-stu-id="f5e7b-116">Get hello list of database restore points using hello Get Database Restore Points operation.</span></span>
2. <span data-ttu-id="f5e7b-117">Zahájit obnovení pomocí hello [požadavek na obnovení databáze vytvořit] [ Create database restore request] operaci.</span><span class="sxs-lookup"><span data-stu-id="f5e7b-117">Begin your restore by using hello [Create database restore request][Create database restore request] operation.</span></span>
3. <span data-ttu-id="f5e7b-118">Sledovat stav hello vaše obnovení pomocí hello [databáze stav operace] [ Database operation status] operaci.</span><span class="sxs-lookup"><span data-stu-id="f5e7b-118">Track hello status of your restore by using hello [Database operation status][Database operation status] operation.</span></span>

> [!NOTE]
> <span data-ttu-id="f5e7b-119">Po dokončení obnovení hello obnovené databáze můžete nakonfigurovat pomocí následujících [nakonfigurovat databázi po obnovení][Configure your database after recovery].</span><span class="sxs-lookup"><span data-stu-id="f5e7b-119">After hello restore has completed, you can configure your recovered database by following [Configure your database after recovery][Configure your database after recovery].</span></span>
> 
> 

## <a name="restore-a-deleted-database"></a><span data-ttu-id="f5e7b-120">Obnovení odstraněné databáze</span><span class="sxs-lookup"><span data-stu-id="f5e7b-120">Restore a deleted database</span></span>
<span data-ttu-id="f5e7b-121">toorestore odstraněné databáze:</span><span class="sxs-lookup"><span data-stu-id="f5e7b-121">toorestore a deleted database:</span></span>

1. <span data-ttu-id="f5e7b-122">Seznam všech možností obnovení odstraněné databáze pomocí hello [seznamu obnovitelné vyřadit databáze] [ List restorable dropped databases] operaci.</span><span class="sxs-lookup"><span data-stu-id="f5e7b-122">List all of your restorable deleted databases by using hello [List restorable dropped databases][List restorable dropped databases] operation.</span></span>
2. <span data-ttu-id="f5e7b-123">Získat hello podrobnosti pro hello odstranit databázi chcete toorestore pomocí hello [Get obnovitelné vyřadit databázi] [ Get restorable dropped database] operaci.</span><span class="sxs-lookup"><span data-stu-id="f5e7b-123">Get hello details for hello deleted database you want toorestore by using hello [Get restorable dropped database][Get restorable dropped database] operation.</span></span>
3. <span data-ttu-id="f5e7b-124">Zahájit obnovení pomocí hello [požadavek na obnovení databáze vytvořit] [ Create database restore request] operaci.</span><span class="sxs-lookup"><span data-stu-id="f5e7b-124">Begin your restore by using hello [Create database restore request][Create database restore request] operation.</span></span>
4. <span data-ttu-id="f5e7b-125">Sledovat stav hello vaše obnovení pomocí hello [databáze stav operace] [ Database operation status] operaci.</span><span class="sxs-lookup"><span data-stu-id="f5e7b-125">Track hello status of your restore by using hello [Database operation status][Database operation status] operation.</span></span>

> [!NOTE]
> <span data-ttu-id="f5e7b-126">v tématu vaše databáze po dokončení obnovení hello tooconfigure [nakonfigurovat databázi po obnovení][Configure your database after recovery].</span><span class="sxs-lookup"><span data-stu-id="f5e7b-126">tooconfigure your database after hello restore has completed, see [Configure your database after recovery][Configure your database after recovery].</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="f5e7b-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f5e7b-127">Next steps</span></span>
<span data-ttu-id="f5e7b-128">toolearn o hello firmy kontinuitu podnikových procesů jednotlivých edice Azure SQL Database, přečtěte si prosím hello [Azure SQL Database obchodní kontinuity přehled][Azure SQL Database business continuity overview].</span><span class="sxs-lookup"><span data-stu-id="f5e7b-128">toolearn about hello business continuity features of Azure SQL Database editions, please read hello [Azure SQL Database business continuity overview][Azure SQL Database business continuity overview].</span></span>

<!--Image references-->

<!--Article references-->
[Azure SQL Database business continuity overview]: ../sql-database/sql-database-business-continuity.md
[Request a DTU quota change]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change
[Configure your database after recovery]: ../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery
[How tooinstall and configure Azure PowerShell]: /powershell/azureps-cmdlets-docs
[Overview]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[REST]: ./sql-data-warehouse-restore-database-rest-api.md

<!--MSDN references-->
[Create database restore request]: https://msdn.microsoft.com/library/azure/dn509571.aspx
[Database operation status]: https://msdn.microsoft.com/library/azure/dn720371.aspx
[Get restorable dropped database]: https://msdn.microsoft.com/library/azure/dn509574.aspx
[List restorable dropped databases]: https://msdn.microsoft.com/library/azure/dn509562.aspx
[Restore-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt693390.aspx

<!--Other Web references-->
[Azure Portal]: https://portal.azure.com/
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
