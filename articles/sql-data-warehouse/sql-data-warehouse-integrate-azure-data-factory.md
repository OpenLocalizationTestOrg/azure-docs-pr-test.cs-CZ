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
# <a name="use-azure-data-factory-with-sql-data-warehouse"></a><span data-ttu-id="db540-103">Použít pro vytváření dat Azure s datovým skladem SQL</span><span class="sxs-lookup"><span data-stu-id="db540-103">Use Azure Data Factory with SQL Data Warehouse</span></span>
<span data-ttu-id="db540-104">Azure Data Factory poskytuje plně spravovaná metodu pro Orchestrace hello přenosu dat a provádění uložené procedury v SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="db540-104">Azure Data Factory provides a fully managed method for orchestrating hello transfer of data and execution of stored procedures on SQL Data Warehouse.</span></span>  <span data-ttu-id="db540-105">To vám umožní toomore snadno nastavení a plán komplexní extrahovat transformace a načítání (ETL) procedury s SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="db540-105">This will allow you toomore easily set-up and schedule complex Extract Transform and Load (ETL) procedures with SQL Data Warehouse.</span></span> <span data-ttu-id="db540-106">Úplnější přehled Azure Data Factory najdete v tématu hello [dokumentace Azure Data Factory][Azure Data Factory documentation].</span><span class="sxs-lookup"><span data-stu-id="db540-106">For a more complete overview of Azure Data Factory, see hello [Azure Data Factory documentation][Azure Data Factory documentation].</span></span>

## <a name="data-movement"></a><span data-ttu-id="db540-107">Pohyb dat</span><span class="sxs-lookup"><span data-stu-id="db540-107">Data Movement</span></span>
<span data-ttu-id="db540-108">Azure Data Factory umožňuje přesun dat mezi místním zdrojům i jiné služby Azure.</span><span class="sxs-lookup"><span data-stu-id="db540-108">Azure Data Factory enables data movement between both on-premises sources and different Azure services.</span></span>  <span data-ttu-id="db540-109">Celkové a aktuální integraci s Azure Data Factory podporuje tooand přesun dat z hello následující umístění:</span><span class="sxs-lookup"><span data-stu-id="db540-109">Overall, current integration with Azure Data Factory supports data movement tooand from hello following locations:</span></span>

* <span data-ttu-id="db540-110">Úložiště objektů blob v Azure</span><span class="sxs-lookup"><span data-stu-id="db540-110">Azure blob storage</span></span>
* <span data-ttu-id="db540-111">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="db540-111">Azure SQL Database</span></span>
* <span data-ttu-id="db540-112">Místní SQL Server</span><span class="sxs-lookup"><span data-stu-id="db540-112">On-premises SQL Server</span></span>
* <span data-ttu-id="db540-113">SQL Server na IaaS</span><span class="sxs-lookup"><span data-stu-id="db540-113">SQL Server on IaaS</span></span>

<span data-ttu-id="db540-114">Informace o jak aktivity kopírování tooset data v tématu [kopírování dat pomocí Azure Data Factory][Copy data with Azure Data Factory]</span><span class="sxs-lookup"><span data-stu-id="db540-114">For information on how tooset up a data copy activity see [Copy data with Azure Data Factory][Copy data with Azure Data Factory]</span></span>

## <a name="stored-procedures"></a><span data-ttu-id="db540-115">Uložené procedury</span><span class="sxs-lookup"><span data-stu-id="db540-115">Stored Procedures</span></span>
 <span data-ttu-id="db540-116">V hello stejný způsobem ji lze také použít tooschedule přenos dat, Azure Data Factory může být použité tooorchestrate hello provádění uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="db540-116">In hello same way it can be used tooschedule data transfer, Azure Data Factory can also be used tooorchestrate hello execution of stored procedures.</span></span>  <span data-ttu-id="db540-117">To umožňuje vytvořit složitější kanály toobe a rozšiřuje Azure Data Factory možnost tooleverage hello výpočetní výkon služby SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="db540-117">This allows more complex pipelines toobe created and extends Azure Data Factory's ability tooleverage hello computational power of SQL Data Warehouse.</span></span>

## <a name="next-steps"></a><span data-ttu-id="db540-118">Další kroky</span><span class="sxs-lookup"><span data-stu-id="db540-118">Next steps</span></span>
<span data-ttu-id="db540-119">Přehled integrace najdete v tématu [Přehled integrace SQL Data Warehouse][SQL Data Warehouse integration overview].</span><span class="sxs-lookup"><span data-stu-id="db540-119">For an overview of integration, see [SQL Data Warehouse integration overview][SQL Data Warehouse integration overview].</span></span>
<span data-ttu-id="db540-120">Další tipy pro vývoj najdete v části [Přehled vývoje SQL Data Warehouse][SQL Data Warehouse development overview].</span><span class="sxs-lookup"><span data-stu-id="db540-120">For more development tips, see [SQL Data Warehouse development overview][SQL Data Warehouse development overview].</span></span>

<!--Image references-->

<!--Article references-->

[Copy data with Azure Data Factory]: ../data-factory/data-factory-data-movement-activities.md
[SQL Data Warehouse development overview]: ./sql-data-warehouse-overview-develop.md
[SQL Data Warehouse integration overview]: ./sql-data-warehouse-overview-integrate.md

<!--MSDN references-->

<!--Other Web references-->
[Azure Data Factory documentation]:https://azure.microsoft.com/documentation/services/data-factory/

