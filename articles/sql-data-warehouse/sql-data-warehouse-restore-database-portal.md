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
# <a name="restore-azure-sql-data-warehouse-portal"></a>Obnovení Azure SQL Data Warehouse (portál)
> [!div class="op_single_selector"]
> * [Přehled][Overview]
> * [Portál][Portal]
> * [Prostředí PowerShell][PowerShell]
> * [REST][REST]
>
>
V tomto článku se dozvíte, jak hello toorestore Azure SQL Data Warehouse pomocí portálu Azure.

## <a name="before-you-begin"></a>Než začnete
**Ověření vaší DTU kapacity.** Každá instance služby SQL Data Warehouse je hostitelem SQL serveru (například myserver.database.windows.net), který má výchozí kvótu jednotek (DTU) propustnost data. Před obnovením SQL Data Warehouse, ověřte, zda má váš server SQL dostatečně zbývající kvóty DTU pro hello databázi, která jste obnovení. toolearn jak kvóty DTU toocalculate nebo toorequest více Dtu, najdete v části [žádosti o změnu kvóty DTU][Request a DTU quota change].

## <a name="restore-an-active-or-paused-database"></a>Obnovit databázi active nebo pozastavena
toorestore databáze:

1. Přihlaste se toohello [portál Azure][Azure portal].
2. V levém podokně hello vyberte **Procházet**a potom vyberte **servery SQL**.

    ![Vyberte Procházet > servery SQL Server](./media/sql-data-warehouse-restore-database-portal/01-browse-for-sql-server.png)
3. Najít váš server a pak ho vyberte.

    ![Vyberte svůj server](./media/sql-data-warehouse-restore-database-portal/01-select-server.png)
4. Najdete hello instanci služby SQL Data Warehouse má toorestore z a pak ho vyberte.

    ![Vyberte hello instanci služby SQL Data Warehouse toorestore](./media/sql-data-warehouse-restore-database-portal/01-select-active-dw.png)
5. Hello horní části okna hello datového skladu, vyberte **obnovení**.

    ![Vyberte možnost obnovení](./media/sql-data-warehouse-restore-database-portal/01-select-restore-from-active.png)
6. Zadejte novou **název databáze**.
7. Vyberte hello nejnovější **bod obnovení**.

   Ujistěte se, že si vyberete hello nejnovější bod obnovení. Protože body obnovení jsou uvedeny v koordinovaný světový čas (UTC), nemusí být hello výchozí možnost hello nejnovější bod obnovení.

      ![Vyberte bod obnovení](./media/sql-data-warehouse-restore-database-portal/01-restore-blade-from-active.png)
8. Vyberte **OK**.
9. proces obnovení databáze Hello zahájíte, a můžete použít **oznámení** toomonitor hello procesu.

> [!NOTE]
> Po dokončení obnovení hello obnovené databáze můžete nakonfigurovat pomocí následujících [nakonfigurovat databázi po obnovení][Configure your database after recovery].
>
>

## <a name="restore-a-deleted-database"></a>Obnovení odstraněné databáze
toorestore odstraněné databáze:

1. Přihlaste se toohello [portál Azure][Azure portal].
2. V levém podokně hello vyberte **Procházet**a potom vyberte **servery SQL**.

    ![Vyberte Procházet > servery SQL Server](./media/sql-data-warehouse-restore-database-portal/01-browse-for-sql-server.png)
3. Najít váš server a pak ho vyberte.

    ![Vyberte svůj server](./media/sql-data-warehouse-restore-database-portal/02-select-server.png)
4. Projděte dolů toohello **Operations** část v okně vašeho serveru.
5. Vyberte hello **odstranila databáze** dlaždici.

    ![Vyberte dlaždici databáze odstraněné hello](./media/sql-data-warehouse-restore-database-portal/02-select-deleted-dws.png)
6. Vyberte hello odstranit databázi, které chcete toorestore.

    ![Vyberte databázi toorestore](./media/sql-data-warehouse-restore-database-portal/02-select-deleted-dw.png)
7. Zadejte novou **název databáze**.

    ![Přidejte název databáze hello](./media/sql-data-warehouse-restore-database-portal/02-restore-blade-from-deleted.png)
8. Vyberte **OK**.
9. proces obnovení databáze Hello zahájíte, a můžete použít **oznámení** toomonitor hello procesu.

> [!NOTE]
> v tématu vaše databáze po dokončení obnovení hello tooconfigure [nakonfigurovat databázi po obnovení][Configure your database after recovery].
>
>

## <a name="next-steps"></a>Další kroky
toolearn o hello firmy kontinuitu podnikových procesů jednotlivých edice Azure SQL Database, přečtěte si hello [Azure SQL Database obchodní kontinuity přehled][Azure SQL Database business continuity overview].

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
