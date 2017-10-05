---
title: "Používat pro vytváření dat Azure s SQL Data Warehouse | Microsoft Docs"
description: "Tipy pro používání Azure Data Factory (ADF) s datovým skladem SQL Azure pro vývoj řešení."
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: jhubbard
editor: 
ms.assetid: 492de762-c7a2-4cdb-943f-3135230e94f1
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: integrate
ms.date: 10/31/2016
ms.author: elbutter;barbkess
ms.openlocfilehash: 7cd113f4a92635bc68253c2beb165ad1f0c96569
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-azure-data-factory-with-sql-data-warehouse"></a>Použít pro vytváření dat Azure s datovým skladem SQL
Azure Data Factory poskytuje plně spravovaná metodu pro Orchestrace přenos dat a provádění uložené procedury v SQL Data Warehouse.  To vám umožní snadno nastavení a plán komplexní extrahovat transformace a načítání (ETL) postupy s SQL Data Warehouse. Podrobnější přehled Azure Data Factory najdete [dokumentace Azure Data Factory][Azure Data Factory documentation].

## <a name="data-movement"></a>Pohyb dat
Azure Data Factory umožňuje přesun dat mezi místním zdrojům i jiné služby Azure.  Celkové a aktuální integraci s Azure Data Factory podporuje přesun dat do a z těchto umístění:

* Úložiště objektů blob v Azure
* Azure SQL Database
* Místní SQL Server
* SQL Server na IaaS

Informace o tom, jak nastavit dat aktivitě kopírování najdete v části [kopírování dat pomocí Azure Data Factory][Copy data with Azure Data Factory]

## <a name="stored-procedures"></a>Uložené procedury
 Stejným způsobem, který lze naplánovat přenos dat Azure Data Factory lze také k orchestraci provádění uložené procedury.  To umožňuje složitější kanály, který se má vytvořit a rozšiřuje možnosti Azure Data Factory můžete využít výpočetní výkon služby SQL Data Warehouse.

## <a name="next-steps"></a>Další kroky
Přehled integrace najdete v tématu [Přehled integrace SQL Data Warehouse][SQL Data Warehouse integration overview].
Další tipy pro vývoj najdete v části [Přehled vývoje SQL Data Warehouse][SQL Data Warehouse development overview].

<!--Image references-->

<!--Article references-->

[Copy data with Azure Data Factory]: ../data-factory/data-factory-data-movement-activities.md
[SQL Data Warehouse development overview]: ./sql-data-warehouse-overview-develop.md
[SQL Data Warehouse integration overview]: ./sql-data-warehouse-overview-integrate.md

<!--MSDN references-->

<!--Other Web references-->
[Azure Data Factory documentation]:https://azure.microsoft.com/documentation/services/data-factory/

