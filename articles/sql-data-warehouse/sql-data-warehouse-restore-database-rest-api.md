---
title: Obnovit Azure SQL Data Warehouse (REST API) | Microsoft Docs
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
ms.openlocfilehash: 8656607611e7518e42b51b91774f55abec15c228
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="restore-an-azure-sql-data-warehouse-rest-api"></a><span data-ttu-id="44ba1-103">Obnovit Azure SQL Data Warehouse (REST API)</span><span class="sxs-lookup"><span data-stu-id="44ba1-103">Restore an Azure SQL Data Warehouse (REST API)</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="44ba1-104">[Přehled][Overview]</span><span class="sxs-lookup"><span data-stu-id="44ba1-104">[Overview][Overview]</span></span>
> * <span data-ttu-id="44ba1-105">[Portál][Portal]</span><span class="sxs-lookup"><span data-stu-id="44ba1-105">[Portal][Portal]</span></span>
> * <span data-ttu-id="44ba1-106">[Prostředí PowerShell][PowerShell]</span><span class="sxs-lookup"><span data-stu-id="44ba1-106">[PowerShell][PowerShell]</span></span>
> * <span data-ttu-id="44ba1-107">[REST][REST]</span><span class="sxs-lookup"><span data-stu-id="44ba1-107">[REST][REST]</span></span>
> 
> 

<span data-ttu-id="44ba1-108">V tomto článku se dozvíte, jak obnovit Azure SQL Data Warehouse pomocí rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="44ba1-108">In this article you will learn how to restore an Azure SQL Data Warehouse using the REST API.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="44ba1-109">Než začnete</span><span class="sxs-lookup"><span data-stu-id="44ba1-109">Before you begin</span></span>
<span data-ttu-id="44ba1-110">**Ověření vaší DTU kapacity.**</span><span class="sxs-lookup"><span data-stu-id="44ba1-110">**Verify your DTU capacity.**</span></span> <span data-ttu-id="44ba1-111">Každý datový sklad SQL je hostitelem serveru SQL (např. myserver.database.windows.net), který má kvóty DTU.</span><span class="sxs-lookup"><span data-stu-id="44ba1-111">Each SQL Data Warehouse is hosted by a SQL server (e.g. myserver.database.windows.net) which has a default DTU quota.</span></span>  <span data-ttu-id="44ba1-112">Před obnovením SQL Data Warehouse, ověřte, zda serveru SQL server má dostatek zbývající kvóty DTU pro databáze obnovena.</span><span class="sxs-lookup"><span data-stu-id="44ba1-112">Before you can restore a SQL Data Warehouse, verify that the your SQL server has enough remaining DTU quota for the database being restored.</span></span> <span data-ttu-id="44ba1-113">Informace o výpočtu DTU potřeby nebo požádejte o další DTU najdete v tématu [žádosti o změnu kvóty DTU][Request a DTU quota change].</span><span class="sxs-lookup"><span data-stu-id="44ba1-113">To learn how to calculate DTU needed or to request more DTU, see [Request a DTU quota change][Request a DTU quota change].</span></span>

## <a name="restore-an-active-or-paused-database"></a><span data-ttu-id="44ba1-114">Obnovit databázi active nebo pozastavena</span><span class="sxs-lookup"><span data-stu-id="44ba1-114">Restore an active or paused database</span></span>
<span data-ttu-id="44ba1-115">Chcete-li obnovit databázi:</span><span class="sxs-lookup"><span data-stu-id="44ba1-115">To restore a database:</span></span>

1. <span data-ttu-id="44ba1-116">Získejte seznam bodů obnovení databáze pomocí operace Get body obnovení databáze.</span><span class="sxs-lookup"><span data-stu-id="44ba1-116">Get the list of database restore points using the Get Database Restore Points operation.</span></span>
2. <span data-ttu-id="44ba1-117">Zahájit obnovení pomocí [požadavek na obnovení databáze vytvořit] [ Create database restore request] operaci.</span><span class="sxs-lookup"><span data-stu-id="44ba1-117">Begin your restore by using the [Create database restore request][Create database restore request] operation.</span></span>
3. <span data-ttu-id="44ba1-118">Sledování stavu obnovení pomocí [databáze stav operace] [ Database operation status] operaci.</span><span class="sxs-lookup"><span data-stu-id="44ba1-118">Track the status of your restore by using the [Database operation status][Database operation status] operation.</span></span>

> [!NOTE]
> <span data-ttu-id="44ba1-119">Po dokončení obnovení můžete nakonfigurovat obnovené databáze pomocí následujících [nakonfigurovat databázi po obnovení][Configure your database after recovery].</span><span class="sxs-lookup"><span data-stu-id="44ba1-119">After the restore has completed, you can configure your recovered database by following [Configure your database after recovery][Configure your database after recovery].</span></span>
> 
> 

## <a name="restore-a-deleted-database"></a><span data-ttu-id="44ba1-120">Obnovení odstraněné databáze</span><span class="sxs-lookup"><span data-stu-id="44ba1-120">Restore a deleted database</span></span>
<span data-ttu-id="44ba1-121">Chcete-li obnovit odstraněnou databázi:</span><span class="sxs-lookup"><span data-stu-id="44ba1-121">To restore a deleted database:</span></span>

1. <span data-ttu-id="44ba1-122">Seznam všech možností obnovení odstraněné databáze pomocí [seznamu obnovitelné vyřadit databáze] [ List restorable dropped databases] operaci.</span><span class="sxs-lookup"><span data-stu-id="44ba1-122">List all of your restorable deleted databases by using the [List restorable dropped databases][List restorable dropped databases] operation.</span></span>
2. <span data-ttu-id="44ba1-123">Získání podrobností o pro odstraněné databáze, kterou chcete obnovit pomocí [Get obnovitelné vyřadit databázi] [ Get restorable dropped database] operaci.</span><span class="sxs-lookup"><span data-stu-id="44ba1-123">Get the details for the deleted database you want to restore by using the [Get restorable dropped database][Get restorable dropped database] operation.</span></span>
3. <span data-ttu-id="44ba1-124">Zahájit obnovení pomocí [požadavek na obnovení databáze vytvořit] [ Create database restore request] operaci.</span><span class="sxs-lookup"><span data-stu-id="44ba1-124">Begin your restore by using the [Create database restore request][Create database restore request] operation.</span></span>
4. <span data-ttu-id="44ba1-125">Sledování stavu obnovení pomocí [databáze stav operace] [ Database operation status] operaci.</span><span class="sxs-lookup"><span data-stu-id="44ba1-125">Track the status of your restore by using the [Database operation status][Database operation status] operation.</span></span>

> [!NOTE]
> <span data-ttu-id="44ba1-126">Konfigurace databáze po dokončení obnovení najdete v tématu [nakonfigurovat databázi po obnovení][Configure your database after recovery].</span><span class="sxs-lookup"><span data-stu-id="44ba1-126">To configure your database after the restore has completed, see [Configure your database after recovery][Configure your database after recovery].</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="44ba1-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="44ba1-127">Next steps</span></span>
<span data-ttu-id="44ba1-128">Další informace o funkcích kontinuity obchodních edice Azure SQL Database, přečtěte si [Azure SQL Database obchodní kontinuity přehled][Azure SQL Database business continuity overview].</span><span class="sxs-lookup"><span data-stu-id="44ba1-128">To learn about the business continuity features of Azure SQL Database editions, please read the [Azure SQL Database business continuity overview][Azure SQL Database business continuity overview].</span></span>

<!--Image references-->

<!--Article references-->
[Azure SQL Database business continuity overview]: ../sql-database/sql-database-business-continuity.md
[Request a DTU quota change]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change
[Configure your database after recovery]: ../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery
[How to install and configure Azure PowerShell]: /powershell/azureps-cmdlets-docs
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
