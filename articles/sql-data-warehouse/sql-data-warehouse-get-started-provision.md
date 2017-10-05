---
title: "Vytvoření SQL Data Warehouse na portálu Azure Portal | Dokumentace Microsoftu"
description: "Naučte se vytvářet Azure SQL Data Warehouse na portálu Azure"
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
ms.openlocfilehash: 24ed2d8bad3090e378acf2a42fb909dee0a8517b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-azure-sql-data-warehouse"></a><span data-ttu-id="fa166-103">Vytvoření Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="fa166-103">Create an Azure SQL Data Warehouse</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="fa166-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="fa166-104">Azure portal</span></span>](sql-data-warehouse-get-started-provision.md)
> * [<span data-ttu-id="fa166-105">TSQL</span><span class="sxs-lookup"><span data-stu-id="fa166-105">TSQL</span></span>](sql-data-warehouse-get-started-create-database-tsql.md)
> * [<span data-ttu-id="fa166-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="fa166-106">PowerShell</span></span>](sql-data-warehouse-get-started-provision-powershell.md)
>
>

<span data-ttu-id="fa166-107">Tento kurz využívá portál Azure k vytvoření služby SQL Data Warehouse, která obsahuje ukázkovou databázi AdventureWorksDW.</span><span class="sxs-lookup"><span data-stu-id="fa166-107">This tutorial uses the Azure portal to create a SQL Data Warehouse that contains an AdventureWorksDW sample database.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fa166-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="fa166-108">Prerequisites</span></span>
<span data-ttu-id="fa166-109">Na začátek budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="fa166-109">To get started, you need:</span></span>

* <span data-ttu-id="fa166-110">**Účet Azure**: Pokud si chcete vytvořit účet, přejděte na stránku [Bezplatná zkušební verze Azure][Azure Free Trial] nebo [Kredity Azure pro předplatitele MSDN][MSDN Azure Credits].</span><span class="sxs-lookup"><span data-stu-id="fa166-110">**Azure account**: Visit [Azure Free Trial][Azure Free Trial] or [MSDN Azure Credits][MSDN Azure Credits] to create an account.</span></span>
* <span data-ttu-id="fa166-111">**Server SQL Azure:** Další podrobnosti najdete v části [Vytvoření Azure SQL Database pomocí webu Azure Portal][Create an Azure SQL database in the Azure portal].</span><span class="sxs-lookup"><span data-stu-id="fa166-111">**Azure SQL server**:  See [Create an Azure SQL database with the Azure portal][Create an Azure SQL database in the Azure portal] for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="fa166-112">Vytvoření služby SQL Data Warehouse může znamenat, že se vám začne fakturovat nová služba.</span><span class="sxs-lookup"><span data-stu-id="fa166-112">Creating a SQL Data Warehouse might result in a new billable service.</span></span>  <span data-ttu-id="fa166-113">Další podrobnosti najdete v tématu [SQL Data Warehouse – ceny][SQL Data Warehouse pricing].</span><span class="sxs-lookup"><span data-stu-id="fa166-113">See [SQL Data Warehouse pricing][SQL Data Warehouse pricing] for more details.</span></span>
>
>

## <a name="create-a-sql-data-warehouse"></a><span data-ttu-id="fa166-114">Vytvoření SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="fa166-114">Create a SQL Data Warehouse</span></span>
1. <span data-ttu-id="fa166-115">Přihlaste se k webu [Portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="fa166-115">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="fa166-116">Klikněte na **+ Nový** > **Databáze** > **SQL Data Warehouse**.</span><span class="sxs-lookup"><span data-stu-id="fa166-116">Click **+ New** > **Databases** > **SQL Data Warehouse**.</span></span>

    ![Vytvoření](./media/sql-data-warehouse-get-started-provision/create-sample.gif)
3. <span data-ttu-id="fa166-118">V okně **SQL Data Warehouse** vyplňte potřebné informace a kliknutím na Vytvořit proveďte vytvoření.</span><span class="sxs-lookup"><span data-stu-id="fa166-118">In the **SQL Data Warehouse** blade, fill in the information needed, then press 'Create' to create.</span></span>

    ![Vytvoření databáze](./media/sql-data-warehouse-get-started-provision/create-database.png)

   * <span data-ttu-id="fa166-120">**Server:** Doporučujeme nejdříve vybrat server.</span><span class="sxs-lookup"><span data-stu-id="fa166-120">**Server**: We recommend you select your server first.</span></span>  
   * <span data-ttu-id="fa166-121">**Název databáze:** Název, který se použije k odkazování na SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="fa166-121">**Database name**: The name that is used to reference the SQL Data Warehouse.</span></span>  <span data-ttu-id="fa166-122">Musí být pro server jedinečný.</span><span class="sxs-lookup"><span data-stu-id="fa166-122">It must be unique to the server.</span></span>
   * <span data-ttu-id="fa166-123">**Výkon:** Doporučujeme začít se 400 [DWU][DWU].</span><span class="sxs-lookup"><span data-stu-id="fa166-123">**Performance**: We recommend starting with 400 [DWUs][DWU].</span></span> <span data-ttu-id="fa166-124">Výkon datového skladu můžete upravit posunutím jezdce doleva nebo doprava, případně pak můžete vertikálně navýšit nebo snížit kapacitu i po jeho vytvoření.</span><span class="sxs-lookup"><span data-stu-id="fa166-124">You can move the slider to the left or right to adjust the performance of your data warehouse, or scale up or down after creation.</span></span>  <span data-ttu-id="fa166-125">Pokud vás zajímají další informace o DWU, projděte si naši dokumentaci ke [škálování](sql-data-warehouse-manage-compute-overview.md) nebo [stránku s cenami][SQL Data Warehouse pricing].</span><span class="sxs-lookup"><span data-stu-id="fa166-125">To learn more about DWUs, see our documentation on [scaling](sql-data-warehouse-manage-compute-overview.md) or our [pricing page][SQL Data Warehouse pricing].</span></span>
   * <span data-ttu-id="fa166-126">**Předplatné:** Vyberte [předplatné], na které se bude tuto službu SQL Data Warehouse fakturovat.</span><span class="sxs-lookup"><span data-stu-id="fa166-126">**Subscription**: Select the [subscription] that this SQL Data Warehouse will bill to.</span></span>
   * <span data-ttu-id="fa166-127">**Skupina prostředků:** [Skupiny prostředků][Resource group] jsou kontejnery, které jsou navržené tak, aby vám pomohly při správě kolekce prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="fa166-127">**Resource group**: [Resource groups][Resource group] are containers designed to help you manage a collection of Azure resources.</span></span> <span data-ttu-id="fa166-128">Další informace o [skupinách prostředků](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="fa166-128">Learn more about [resource groups](../azure-resource-manager/resource-group-overview.md).</span></span>
   * <span data-ttu-id="fa166-129">**Zvolit zdroj:** Klikněte na **Zvolit zdroj** > **Ukázka**.</span><span class="sxs-lookup"><span data-stu-id="fa166-129">**Select source**: Click **Select source** > **Sample**.</span></span> <span data-ttu-id="fa166-130">Azure automaticky vyplní možnost **Vyberte vzorek** s možností AdventureWorksDW.</span><span class="sxs-lookup"><span data-stu-id="fa166-130">Azure automatically populates the **Select sample** option with AdventureWorksDW.</span></span>

   > [!NOTE]
   > <span data-ttu-id="fa166-131">Výchozí kolace pro SQL Data Warehouse je SQL_Latin1_General_CP1_CI_AS.</span><span class="sxs-lookup"><span data-stu-id="fa166-131">The default collation for a SQL Data Warehouse is SQL_Latin1_General_CP1_CI_AS.</span></span> <span data-ttu-id="fa166-132">V případě potřeby jiné kolace je možné pomocí [T-SQL][T-SQL] vytvořit databázi s jinou kolací.</span><span class="sxs-lookup"><span data-stu-id="fa166-132">If a different collation is needed, [T-SQL][T-SQL] can be used to create the database with a different collation.</span></span>
   >
   >

1. <span data-ttu-id="fa166-133">Kliknutím na **Vytvořit** si vytvořte svoji službu SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="fa166-133">Click **Create** to create your SQL Data Warehouse.</span></span>
2. <span data-ttu-id="fa166-134">Počkejte několik minut.</span><span class="sxs-lookup"><span data-stu-id="fa166-134">Wait for a few minutes.</span></span> <span data-ttu-id="fa166-135">Když je váš datový sklad připraven, můžete se vrátit do [portálu Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="fa166-135">When your data warehouse is ready, you should be returned to the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="fa166-136">Svoji službu SQL Data Warehouse najdete na řídicím panelu v části s vašimi databázemi SQL nebo ve skupině prostředků, kterou jste použili k jejímu vytvoření.</span><span class="sxs-lookup"><span data-stu-id="fa166-136">You can find your SQL Data Warehouse on your dashboard, listed under your SQL Databases, or in the resource group that you used to create it.</span></span>

    ![zobrazení portálu](./media/sql-data-warehouse-get-started-provision/database-portal-view.png)

[!INCLUDE [SQL Database create server](../../includes/sql-database-create-new-server-firewall-portal.md)]

## <a name="next-steps"></a><span data-ttu-id="fa166-138">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fa166-138">Next steps</span></span>
<span data-ttu-id="fa166-139">Službu SQL Data Warehouse máte vytvořenou, takže se teď můžete [připojit](sql-data-warehouse-connect-overview.md) a můžete začít se zadáváním dotazů.</span><span class="sxs-lookup"><span data-stu-id="fa166-139">Now that you have created a SQL Data Warehouse, you are ready to [Connect](sql-data-warehouse-connect-overview.md) and begin querying.</span></span>

<span data-ttu-id="fa166-140">Pokud budete chtít načíst data do SQL Data Warehouse, najdete další informace v části [s přehledem načítání](sql-data-warehouse-overview-load.md).</span><span class="sxs-lookup"><span data-stu-id="fa166-140">To load data into SQL Data Warehouse, see the [loading overview](sql-data-warehouse-overview-load.md).</span></span>

<span data-ttu-id="fa166-141">Pokud se pokoušíte migrovat existující databázi do SQL Data Warehouse, podívejte na [přehled migrace](sql-data-warehouse-overview-migrate.md) nebo použijte [nástroj pro migraci](sql-data-warehouse-migrate-migration-utility.md).</span><span class="sxs-lookup"><span data-stu-id="fa166-141">If you are trying to migrate an existing database to SQL Data Warehouse, see the [Migration overview](sql-data-warehouse-overview-migrate.md) or use [Migration Utility](sql-data-warehouse-migrate-migration-utility.md).</span></span>

<span data-ttu-id="fa166-142">Pravidla brány firewall lze také konfigurovat pomocí jazyka Transact-SQL.</span><span class="sxs-lookup"><span data-stu-id="fa166-142">Firewall rules can also be configured using Transact-SQL.</span></span> <span data-ttu-id="fa166-143">Další informace najdete v tématech [sp_set_firewall_rule][sp_set_firewall_rule] a [sp_set_database_firewall_rule][sp_set_database_firewall_rule].</span><span class="sxs-lookup"><span data-stu-id="fa166-143">For more information, see [sp_set_firewall_rule][sp_set_firewall_rule] and [sp_set_database_firewall_rule][sp_set_database_firewall_rule].</span></span>

<span data-ttu-id="fa166-144">Také může být užitečné podívat se na [Osvědčené postupy][Best practices].</span><span class="sxs-lookup"><span data-stu-id="fa166-144">It's also a great idea to look at the [Best practices][Best practices].</span></span>

<!--Article references-->
[Create an Azure SQL database in the Azure portal]: ../sql-database/sql-database-get-started.md
[Create an Azure SQL database with PowerShell]: ../sql-database/sql-database-create-and-configure-database-powershell
[resource groups]: ../azure-resource-manager/resource-group-template-deploy-portal.md
[Best practices]: sql-data-warehouse-best-practices.md
[DWU]: sql-data-warehouse-overview-what-is.md
<span data-ttu-id="fa166-145">[předplatné]: ../azure-glossary-cloud-terminology.md#subscription</span><span class="sxs-lookup"><span data-stu-id="fa166-145">[subscription]: ../azure-glossary-cloud-terminology.md#subscription</span></span>
[resource group]: ../azure-glossary-cloud-terminology.md#resource-group
[T-SQL]: ./sql-data-warehouse-get-started-create-database-tsql.md

<!--MSDN references-->
[sp_set_firewall_rule]: https://msdn.microsoft.com/library/dn270017.aspx
[sp_set_database_firewall_rule]: https://msdn.microsoft.com/library/dn270010.aspx

<!--Other Web references-->
[SQL Data Warehouse pricing]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Azure Free Trial]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[MSDN Azure Credits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F
