---
title: aaaUse Azure Data Factory s SQL Data Warehouse | Microsoft Docs
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
ms.openlocfilehash: d40a547830f9681504253d39ae3066800a955c04
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-data-factory-with-sql-data-warehouse"></a>Použít pro vytváření dat Azure s datovým skladem SQL
Azure Data Factory poskytuje plně spravovaná metodu pro Orchestrace hello přenosu dat a provádění uložené procedury v SQL Data Warehouse.  To vám umožní toomore snadno nastavení a plán komplexní extrahovat transformace a načítání (ETL) procedury s SQL Data Warehouse. Úplnější přehled Azure Data Factory najdete v tématu hello [dokumentace Azure Data Factory][Azure Data Factory documentation].

## <a name="data-movement"></a>Pohyb dat
Azure Data Factory umožňuje přesun dat mezi místním zdrojům i jiné služby Azure.  Celkové a aktuální integraci s Azure Data Factory podporuje tooand přesun dat z hello následující umístění:

* Úložiště objektů blob v Azure
* Azure SQL Database
* Místní SQL Server
* SQL Server na IaaS

Informace o jak aktivity kopírování tooset data v tématu [kopírování dat pomocí Azure Data Factory][Copy data with Azure Data Factory]

## <a name="stored-procedures"></a>Uložené procedury
 V hello stejný způsobem ji lze také použít tooschedule přenos dat, Azure Data Factory může být použité tooorchestrate hello provádění uložené procedury.  To umožňuje vytvořit složitější kanály toobe a rozšiřuje Azure Data Factory možnost tooleverage hello výpočetní výkon služby SQL Data Warehouse.

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

