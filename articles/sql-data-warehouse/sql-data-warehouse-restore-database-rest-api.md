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
# <a name="restore-an-azure-sql-data-warehouse-rest-api"></a>Obnovit Azure SQL Data Warehouse (REST API)
> [!div class="op_single_selector"]
> * [Přehled][Overview]
> * [Portál][Portal]
> * [Prostředí PowerShell][PowerShell]
> * [REST][REST]
> 
> 

V tomto článku se dozvíte, jak hello toorestore k Azure SQL Data Warehouse pomocí rozhraní REST API.

## <a name="before-you-begin"></a>Než začnete
**Ověření vaší DTU kapacity.** Každý datový sklad SQL je hostitelem serveru SQL (např. myserver.database.windows.net), který má kvóty DTU.  Před obnovením SQL Data Warehouse, ověřte, že hello systému SQL server má dostatek zbývající kvóty DTU pro hello databáze obnovena. toolearn jak toocalculate DTU potřebná nebo toorequest více DTU, najdete v části [žádosti o změnu kvóty DTU][Request a DTU quota change].

## <a name="restore-an-active-or-paused-database"></a>Obnovit databázi active nebo pozastavena
toorestore databáze:

1. Získání seznamu hello bodů obnovení databáze pomocí operace Get body obnovení databáze hello.
2. Zahájit obnovení pomocí hello [požadavek na obnovení databáze vytvořit] [ Create database restore request] operaci.
3. Sledovat stav hello vaše obnovení pomocí hello [databáze stav operace] [ Database operation status] operaci.

> [!NOTE]
> Po dokončení obnovení hello obnovené databáze můžete nakonfigurovat pomocí následujících [nakonfigurovat databázi po obnovení][Configure your database after recovery].
> 
> 

## <a name="restore-a-deleted-database"></a>Obnovení odstraněné databáze
toorestore odstraněné databáze:

1. Seznam všech možností obnovení odstraněné databáze pomocí hello [seznamu obnovitelné vyřadit databáze] [ List restorable dropped databases] operaci.
2. Získat hello podrobnosti pro hello odstranit databázi chcete toorestore pomocí hello [Get obnovitelné vyřadit databázi] [ Get restorable dropped database] operaci.
3. Zahájit obnovení pomocí hello [požadavek na obnovení databáze vytvořit] [ Create database restore request] operaci.
4. Sledovat stav hello vaše obnovení pomocí hello [databáze stav operace] [ Database operation status] operaci.

> [!NOTE]
> v tématu vaše databáze po dokončení obnovení hello tooconfigure [nakonfigurovat databázi po obnovení][Configure your database after recovery].
> 
> 

## <a name="next-steps"></a>Další kroky
toolearn o hello firmy kontinuitu podnikových procesů jednotlivých edice Azure SQL Database, přečtěte si prosím hello [Azure SQL Database obchodní kontinuity přehled][Azure SQL Database business continuity overview].

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
