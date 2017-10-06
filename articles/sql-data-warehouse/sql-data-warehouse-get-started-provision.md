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
# <a name="create-an-azure-sql-data-warehouse"></a>Vytvoření Azure SQL Data Warehouse
> [!div class="op_single_selector"]
> * [Azure Portal](sql-data-warehouse-get-started-provision.md)
> * [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
> * [PowerShell](sql-data-warehouse-get-started-provision-powershell.md)
>
>

Tento kurz používá hello portálu toocreate Azure SQL Data Warehouse, která obsahuje ukázkovou databázi AdventureWorksDW.

## <a name="prerequisites"></a>Požadavky
tooget spuštění, budete potřebovat:

* **Účet Azure**: navštivte [bezplatná zkušební verze Azure] [ Azure Free Trial] nebo [kredity Azure MSDN] [ MSDN Azure Credits] toocreate účet.
* **Azure SQL server**: najdete v části [vytvoření Azure SQL database pomocí portálu Azure hello] [ Create an Azure SQL database in hello Azure portal] další podrobnosti.

> [!NOTE]
> Vytvoření služby SQL Data Warehouse může znamenat, že se vám začne fakturovat nová služba.  Další podrobnosti najdete v tématu [SQL Data Warehouse – ceny][SQL Data Warehouse pricing].
>
>

## <a name="create-a-sql-data-warehouse"></a>Vytvoření SQL Data Warehouse
1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Klikněte na **+ Nový** > **Databáze** > **SQL Data Warehouse**.

    ![Vytvořit](./media/sql-data-warehouse-get-started-provision/create-sample.gif)
3. V hello **SQL Data Warehouse** okno vyplňte hello informace potřebné, stiskněte klávesu toocreate 'Vytvořit'.

    ![Vytvoření databáze](./media/sql-data-warehouse-get-started-provision/create-database.png)

   * **Server:** Doporučujeme nejdříve vybrat server.  
   * **Název databáze**: hello název, který je použité tooreference hello SQL Data Warehouse.  Musí být jedinečný toohello serveru.
   * **Výkon:** Doporučujeme začít se 400 [DWU][DWU]. Můžete přesunout toohello hello posuvník doleva nebo pravým tooadjust hello výkonu datového skladu, nebo škálování nahoru nebo dolů po vytvoření.  toolearn Další informace o Dwu, najdete v dokumentaci na [škálování](sql-data-warehouse-manage-compute-overview.md) nebo [stránce s cenami][SQL Data Warehouse pricing].
   * **Předplatné**: Vyberte hello [předplatné] , bude tato SQL Data Warehouse fakturovat.
   * **Skupina prostředků**: [skupiny prostředků] [ Resource group] jsou kontejnery, které jsou určené toohelp správu kolekcí prostředků Azure. Další informace o [skupinách prostředků](../azure-resource-manager/resource-group-overview.md).
   * **Zvolit zdroj:** Klikněte na **Zvolit zdroj** > **Ukázka**. Azure automaticky naplní hello **zvolit vzorek** možnost AdventureWorksDW.

   > [!NOTE]
   > Hello výchozí kolaci pro SQL Data Warehouse je SQL_Latin1_General_CP1_CI_AS. V případě potřeby jiné kolace [T-SQL] [ T-SQL] lze použít toocreate hello databáze pomocí jiné kolace.
   >
   >

1. Klikněte na tlačítko **vytvořit** toocreate vaše SQL datového skladu.
2. Počkejte několik minut. Pokud váš datový sklad je připraven, byste se měli vrátit toohello [portál Azure](https://portal.azure.com). Můžete na řídicím panelu v rámci vaší databáze SQL, najít SQL Data Warehouse nebo hello prostředku skupiny této toocreate jste použili ji.

    ![zobrazení portálu](./media/sql-data-warehouse-get-started-provision/database-portal-view.png)

[!INCLUDE [SQL Database create server](../../includes/sql-database-create-new-server-firewall-portal.md)]

## <a name="next-steps"></a>Další kroky
Nyní jste vytvořili SQL Data Warehouse, jste připraveni příliš[Connect](sql-data-warehouse-connect-overview.md) a začít se zadáváním dotazů.

tooload dat do SQL Data Warehouse, najdete v části hello [přehledem načítání](sql-data-warehouse-overview-load.md).

Pokud se pokoušíte toomigrate existující databáze tooSQL datového skladu, přečtěte si téma hello [Přehled migrace](sql-data-warehouse-overview-migrate.md) nebo použijte [nástroj pro migraci](sql-data-warehouse-migrate-migration-utility.md).

Pravidla brány firewall lze také konfigurovat pomocí jazyka Transact-SQL. Další informace najdete v tématech [sp_set_firewall_rule][sp_set_firewall_rule] a [sp_set_database_firewall_rule][sp_set_database_firewall_rule].

Je také toolook kvalitních nápad na hello [osvědčené postupy][Best practices].

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
