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
# <a name="use-azure-data-factory-with-sql-data-warehouse"></a><span data-ttu-id="0d940-103">Použít pro vytváření dat Azure s datovým skladem SQL</span><span class="sxs-lookup"><span data-stu-id="0d940-103">Use Azure Data Factory with SQL Data Warehouse</span></span>
<span data-ttu-id="0d940-104">Azure Data Factory poskytuje plně spravovaná metodu pro Orchestrace přenos dat a provádění uložené procedury v SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="0d940-104">Azure Data Factory provides a fully managed method for orchestrating the transfer of data and execution of stored procedures on SQL Data Warehouse.</span></span>  <span data-ttu-id="0d940-105">To vám umožní snadno nastavení a plán komplexní extrahovat transformace a načítání (ETL) postupy s SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="0d940-105">This will allow you to more easily set-up and schedule complex Extract Transform and Load (ETL) procedures with SQL Data Warehouse.</span></span> <span data-ttu-id="0d940-106">Podrobnější přehled Azure Data Factory najdete [dokumentace Azure Data Factory][Azure Data Factory documentation].</span><span class="sxs-lookup"><span data-stu-id="0d940-106">For a more complete overview of Azure Data Factory, see the [Azure Data Factory documentation][Azure Data Factory documentation].</span></span>

## <a name="data-movement"></a><span data-ttu-id="0d940-107">Pohyb dat</span><span class="sxs-lookup"><span data-stu-id="0d940-107">Data Movement</span></span>
<span data-ttu-id="0d940-108">Azure Data Factory umožňuje přesun dat mezi místním zdrojům i jiné služby Azure.</span><span class="sxs-lookup"><span data-stu-id="0d940-108">Azure Data Factory enables data movement between both on-premises sources and different Azure services.</span></span>  <span data-ttu-id="0d940-109">Celkové a aktuální integraci s Azure Data Factory podporuje přesun dat do a z těchto umístění:</span><span class="sxs-lookup"><span data-stu-id="0d940-109">Overall, current integration with Azure Data Factory supports data movement to and from the following locations:</span></span>

* <span data-ttu-id="0d940-110">Úložiště objektů blob v Azure</span><span class="sxs-lookup"><span data-stu-id="0d940-110">Azure blob storage</span></span>
* <span data-ttu-id="0d940-111">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="0d940-111">Azure SQL Database</span></span>
* <span data-ttu-id="0d940-112">Místní SQL Server</span><span class="sxs-lookup"><span data-stu-id="0d940-112">On-premises SQL Server</span></span>
* <span data-ttu-id="0d940-113">SQL Server na IaaS</span><span class="sxs-lookup"><span data-stu-id="0d940-113">SQL Server on IaaS</span></span>

<span data-ttu-id="0d940-114">Informace o tom, jak nastavit dat aktivitě kopírování najdete v části [kopírování dat pomocí Azure Data Factory][Copy data with Azure Data Factory]</span><span class="sxs-lookup"><span data-stu-id="0d940-114">For information on how to set up a data copy activity see [Copy data with Azure Data Factory][Copy data with Azure Data Factory]</span></span>

## <a name="stored-procedures"></a><span data-ttu-id="0d940-115">Uložené procedury</span><span class="sxs-lookup"><span data-stu-id="0d940-115">Stored Procedures</span></span>
 <span data-ttu-id="0d940-116">Stejným způsobem, který lze naplánovat přenos dat Azure Data Factory lze také k orchestraci provádění uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="0d940-116">In the same way it can be used to schedule data transfer, Azure Data Factory can also be used to orchestrate the execution of stored procedures.</span></span>  <span data-ttu-id="0d940-117">To umožňuje složitější kanály, který se má vytvořit a rozšiřuje možnosti Azure Data Factory můžete využít výpočetní výkon služby SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="0d940-117">This allows more complex pipelines to be created and extends Azure Data Factory's ability to leverage the computational power of SQL Data Warehouse.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0d940-118">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0d940-118">Next steps</span></span>
<span data-ttu-id="0d940-119">Přehled integrace najdete v tématu [Přehled integrace SQL Data Warehouse][SQL Data Warehouse integration overview].</span><span class="sxs-lookup"><span data-stu-id="0d940-119">For an overview of integration, see [SQL Data Warehouse integration overview][SQL Data Warehouse integration overview].</span></span>
<span data-ttu-id="0d940-120">Další tipy pro vývoj najdete v části [Přehled vývoje SQL Data Warehouse][SQL Data Warehouse development overview].</span><span class="sxs-lookup"><span data-stu-id="0d940-120">For more development tips, see [SQL Data Warehouse development overview][SQL Data Warehouse development overview].</span></span>

<!--Image references-->

<!--Article references-->

[Copy data with Azure Data Factory]: ../data-factory/data-factory-data-movement-activities.md
[SQL Data Warehouse development overview]: ./sql-data-warehouse-overview-develop.md
[SQL Data Warehouse integration overview]: ./sql-data-warehouse-overview-integrate.md

<!--MSDN references-->

<!--Other Web references-->
[Azure Data Factory documentation]:https://azure.microsoft.com/documentation/services/data-factory/

