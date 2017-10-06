---
title: "aaaCreate v hello portál Azure SQL Data Warehouse | Microsoft Docs"
description: "Zjistěte, jak hello toocreate na Azure SQL Data Warehouse v portálu Azure"
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: jhubbard
editor: 
tags: azure-sql-data-warehouse
ms.assetid: 552e496e-d560-419c-9996-6bbc80c521cb
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: create
ms.date: 10/31/2016
ms.author: elbutter;barbkess
ms.openlocfilehash: f5be6e3f2936e3af9d099854a468f8ce66fd8fbe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-sql-data-warehouse"></a><span data-ttu-id="90a08-103">Vytvoření Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="90a08-103">Create an Azure SQL Data Warehouse</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="90a08-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="90a08-104">Azure portal</span></span>](sql-data-warehouse-get-started-provision.md)
> * [<span data-ttu-id="90a08-105">TSQL</span><span class="sxs-lookup"><span data-stu-id="90a08-105">TSQL</span></span>](sql-data-warehouse-get-started-create-database-tsql.md)
> * [<span data-ttu-id="90a08-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="90a08-106">PowerShell</span></span>](sql-data-warehouse-get-started-provision-powershell.md)
>
>

<span data-ttu-id="90a08-107">Tento kurz používá hello portálu toocreate Azure SQL Data Warehouse, která obsahuje ukázkovou databázi AdventureWorksDW.</span><span class="sxs-lookup"><span data-stu-id="90a08-107">This tutorial uses hello Azure portal toocreate a SQL Data Warehouse that contains an AdventureWorksDW sample database.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="90a08-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="90a08-108">Prerequisites</span></span>
<span data-ttu-id="90a08-109">tooget spuštění, budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="90a08-109">tooget started, you need:</span></span>

* <span data-ttu-id="90a08-110">**Účet Azure**: navštivte [bezplatná zkušební verze Azure] [ Azure Free Trial] nebo [kredity Azure MSDN] [ MSDN Azure Credits] toocreate účet.</span><span class="sxs-lookup"><span data-stu-id="90a08-110">**Azure account**: Visit [Azure Free Trial][Azure Free Trial] or [MSDN Azure Credits][MSDN Azure Credits] toocreate an account.</span></span>
* <span data-ttu-id="90a08-111">**Azure SQL server**: najdete v části [vytvoření Azure SQL database pomocí portálu Azure hello] [ Create an Azure SQL database in hello Azure portal] další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="90a08-111">**Azure SQL server**:  See [Create an Azure SQL database with hello Azure portal][Create an Azure SQL database in hello Azure portal] for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="90a08-112">Vytvoření služby SQL Data Warehouse může znamenat, že se vám začne fakturovat nová služba.</span><span class="sxs-lookup"><span data-stu-id="90a08-112">Creating a SQL Data Warehouse might result in a new billable service.</span></span>  <span data-ttu-id="90a08-113">Další podrobnosti najdete v tématu [SQL Data Warehouse – ceny][SQL Data Warehouse pricing].</span><span class="sxs-lookup"><span data-stu-id="90a08-113">See [SQL Data Warehouse pricing][SQL Data Warehouse pricing] for more details.</span></span>
>
>

## <a name="create-a-sql-data-warehouse"></a><span data-ttu-id="90a08-114">Vytvoření SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="90a08-114">Create a SQL Data Warehouse</span></span>
1. <span data-ttu-id="90a08-115">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="90a08-115">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="90a08-116">Klikněte na **+ Nový** > **Databáze** > **SQL Data Warehouse**.</span><span class="sxs-lookup"><span data-stu-id="90a08-116">Click **+ New** > **Databases** > **SQL Data Warehouse**.</span></span>

    ![Vytvořit](./media/sql-data-warehouse-get-started-provision/create-sample.gif)
3. <span data-ttu-id="90a08-118">V hello **SQL Data Warehouse** okno vyplňte hello informace potřebné, stiskněte klávesu toocreate 'Vytvořit'.</span><span class="sxs-lookup"><span data-stu-id="90a08-118">In hello **SQL Data Warehouse** blade, fill in hello information needed, then press 'Create' toocreate.</span></span>

    ![Vytvoření databáze](./media/sql-data-warehouse-get-started-provision/create-database.png)

   * <span data-ttu-id="90a08-120">**Server:** Doporučujeme nejdříve vybrat server.</span><span class="sxs-lookup"><span data-stu-id="90a08-120">**Server**: We recommend you select your server first.</span></span>  
   * <span data-ttu-id="90a08-121">**Název databáze**: hello název, který je použité tooreference hello SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="90a08-121">**Database name**: hello name that is used tooreference hello SQL Data Warehouse.</span></span>  <span data-ttu-id="90a08-122">Musí být jedinečný toohello serveru.</span><span class="sxs-lookup"><span data-stu-id="90a08-122">It must be unique toohello server.</span></span>
   * <span data-ttu-id="90a08-123">**Výkon:** Doporučujeme začít se 400 [DWU][DWU].</span><span class="sxs-lookup"><span data-stu-id="90a08-123">**Performance**: We recommend starting with 400 [DWUs][DWU].</span></span> <span data-ttu-id="90a08-124">Můžete přesunout toohello hello posuvník doleva nebo pravým tooadjust hello výkonu datového skladu, nebo škálování nahoru nebo dolů po vytvoření.</span><span class="sxs-lookup"><span data-stu-id="90a08-124">You can move hello slider toohello left or right tooadjust hello performance of your data warehouse, or scale up or down after creation.</span></span>  <span data-ttu-id="90a08-125">toolearn Další informace o Dwu, najdete v dokumentaci na [škálování](sql-data-warehouse-manage-compute-overview.md) nebo [stránce s cenami][SQL Data Warehouse pricing].</span><span class="sxs-lookup"><span data-stu-id="90a08-125">toolearn more about DWUs, see our documentation on [scaling](sql-data-warehouse-manage-compute-overview.md) or our [pricing page][SQL Data Warehouse pricing].</span></span>
   * <span data-ttu-id="90a08-126">**Předplatné**: Vyberte hello [předplatné] , bude tato SQL Data Warehouse fakturovat.</span><span class="sxs-lookup"><span data-stu-id="90a08-126">**Subscription**: Select hello [subscription] that this SQL Data Warehouse will bill to.</span></span>
   * <span data-ttu-id="90a08-127">**Skupina prostředků**: [skupiny prostředků] [ Resource group] jsou kontejnery, které jsou určené toohelp správu kolekcí prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="90a08-127">**Resource group**: [Resource groups][Resource group] are containers designed toohelp you manage a collection of Azure resources.</span></span> <span data-ttu-id="90a08-128">Další informace o [skupinách prostředků](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="90a08-128">Learn more about [resource groups](../azure-resource-manager/resource-group-overview.md).</span></span>
   * <span data-ttu-id="90a08-129">**Zvolit zdroj:** Klikněte na **Zvolit zdroj** > **Ukázka**.</span><span class="sxs-lookup"><span data-stu-id="90a08-129">**Select source**: Click **Select source** > **Sample**.</span></span> <span data-ttu-id="90a08-130">Azure automaticky naplní hello **zvolit vzorek** možnost AdventureWorksDW.</span><span class="sxs-lookup"><span data-stu-id="90a08-130">Azure automatically populates hello **Select sample** option with AdventureWorksDW.</span></span>

   > [!NOTE]
   > <span data-ttu-id="90a08-131">Hello výchozí kolaci pro SQL Data Warehouse je SQL_Latin1_General_CP1_CI_AS.</span><span class="sxs-lookup"><span data-stu-id="90a08-131">hello default collation for a SQL Data Warehouse is SQL_Latin1_General_CP1_CI_AS.</span></span> <span data-ttu-id="90a08-132">V případě potřeby jiné kolace [T-SQL] [ T-SQL] lze použít toocreate hello databáze pomocí jiné kolace.</span><span class="sxs-lookup"><span data-stu-id="90a08-132">If a different collation is needed, [T-SQL][T-SQL] can be used toocreate hello database with a different collation.</span></span>
   >
   >

1. <span data-ttu-id="90a08-133">Klikněte na tlačítko **vytvořit** toocreate vaše SQL datového skladu.</span><span class="sxs-lookup"><span data-stu-id="90a08-133">Click **Create** toocreate your SQL Data Warehouse.</span></span>
2. <span data-ttu-id="90a08-134">Počkejte několik minut.</span><span class="sxs-lookup"><span data-stu-id="90a08-134">Wait for a few minutes.</span></span> <span data-ttu-id="90a08-135">Pokud váš datový sklad je připraven, byste se měli vrátit toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="90a08-135">When your data warehouse is ready, you should be returned toohello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="90a08-136">Můžete na řídicím panelu v rámci vaší databáze SQL, najít SQL Data Warehouse nebo hello prostředku skupiny této toocreate jste použili ji.</span><span class="sxs-lookup"><span data-stu-id="90a08-136">You can find your SQL Data Warehouse on your dashboard, listed under your SQL Databases, or in hello resource group that you used toocreate it.</span></span>

    ![zobrazení portálu](./media/sql-data-warehouse-get-started-provision/database-portal-view.png)

[!INCLUDE [SQL Database create server](../../includes/sql-database-create-new-server-firewall-portal.md)]

## <a name="next-steps"></a><span data-ttu-id="90a08-138">Další kroky</span><span class="sxs-lookup"><span data-stu-id="90a08-138">Next steps</span></span>
<span data-ttu-id="90a08-139">Nyní jste vytvořili SQL Data Warehouse, jste připraveni příliš[Connect](sql-data-warehouse-connect-overview.md) a začít se zadáváním dotazů.</span><span class="sxs-lookup"><span data-stu-id="90a08-139">Now that you have created a SQL Data Warehouse, you are ready too[Connect](sql-data-warehouse-connect-overview.md) and begin querying.</span></span>

<span data-ttu-id="90a08-140">tooload dat do SQL Data Warehouse, najdete v části hello [přehledem načítání](sql-data-warehouse-overview-load.md).</span><span class="sxs-lookup"><span data-stu-id="90a08-140">tooload data into SQL Data Warehouse, see hello [loading overview](sql-data-warehouse-overview-load.md).</span></span>

<span data-ttu-id="90a08-141">Pokud se pokoušíte toomigrate existující databáze tooSQL datového skladu, přečtěte si téma hello [Přehled migrace](sql-data-warehouse-overview-migrate.md) nebo použijte [nástroj pro migraci](sql-data-warehouse-migrate-migration-utility.md).</span><span class="sxs-lookup"><span data-stu-id="90a08-141">If you are trying toomigrate an existing database tooSQL Data Warehouse, see hello [Migration overview](sql-data-warehouse-overview-migrate.md) or use [Migration Utility](sql-data-warehouse-migrate-migration-utility.md).</span></span>

<span data-ttu-id="90a08-142">Pravidla brány firewall lze také konfigurovat pomocí jazyka Transact-SQL.</span><span class="sxs-lookup"><span data-stu-id="90a08-142">Firewall rules can also be configured using Transact-SQL.</span></span> <span data-ttu-id="90a08-143">Další informace najdete v tématech [sp_set_firewall_rule][sp_set_firewall_rule] a [sp_set_database_firewall_rule][sp_set_database_firewall_rule].</span><span class="sxs-lookup"><span data-stu-id="90a08-143">For more information, see [sp_set_firewall_rule][sp_set_firewall_rule] and [sp_set_database_firewall_rule][sp_set_database_firewall_rule].</span></span>

<span data-ttu-id="90a08-144">Je také toolook kvalitních nápad na hello [osvědčené postupy][Best practices].</span><span class="sxs-lookup"><span data-stu-id="90a08-144">It's also a great idea toolook at hello [Best practices][Best practices].</span></span>

<!--Article references-->
[Create an Azure SQL database in hello Azure portal]: ../sql-database/sql-database-get-started.md
[Create an Azure SQL database with PowerShell]: ../sql-database/sql-database-create-and-configure-database-powershell
[resource groups]: ../azure-resource-manager/resource-group-template-deploy-portal.md
[Best practices]: sql-data-warehouse-best-practices.md
[DWU]: sql-data-warehouse-overview-what-is.md
[předplatné]: ../azure-glossary-cloud-terminology.md#subscription
[resource group]: ../azure-glossary-cloud-terminology.md#resource-group
[T-SQL]: ./sql-data-warehouse-get-started-create-database-tsql.md

<!--MSDN references-->
[sp_set_firewall_rule]: https://msdn.microsoft.com/library/dn270017.aspx
[sp_set_database_firewall_rule]: https://msdn.microsoft.com/library/dn270010.aspx

<!--Other Web references-->
[SQL Data Warehouse pricing]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Azure Free Trial]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[MSDN Azure Credits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F
