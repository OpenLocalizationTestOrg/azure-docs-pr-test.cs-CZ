---
title: "Sestavení integrované řešení s SQL Data Warehouse | Microsoft Docs"
description: "Nástroje a partnery, se řešení, které se integrují se službou SQL Data Warehouse. "
services: sql-data-warehouse
documentationcenter: NA
author: mlee3gsd
manager: jhubbard
editor: 
ms.assetid: e2dc8f3f-10e3-4589-a4e2-50c67dfcf67f
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: integrate
ms.date: 10/31/2016
ms.author: martinle;barbkess
ms.openlocfilehash: d407c29f99fd7537590ec787febd84a9e3f4f353
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="leverage-other-services-with-sql-data-warehouse"></a><span data-ttu-id="40c90-103">Využít dalším službám pomocí SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="40c90-103">Leverage other services with SQL Data Warehouse</span></span>
<span data-ttu-id="40c90-104">Kromě jeho základní funkce SQL Data Warehouse umožňuje uživatelům využívat mnoho dalších služeb v Azure společně se ho.</span><span class="sxs-lookup"><span data-stu-id="40c90-104">In addition to its core functionality, SQL Data Warehouse enables users to leverage many of the other services in Azure alongside it.</span></span>  <span data-ttu-id="40c90-105">Konkrétně aktuálně přesměrovali jsme kroky pro řešení integraci s následujícími službami:</span><span class="sxs-lookup"><span data-stu-id="40c90-105">Specifically, we have currently taken steps to deeply integrate with the following:</span></span>

* <span data-ttu-id="40c90-106">Power BI</span><span class="sxs-lookup"><span data-stu-id="40c90-106">Power BI</span></span>
* <span data-ttu-id="40c90-107">Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="40c90-107">Azure Data Factory</span></span>
* <span data-ttu-id="40c90-108">Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="40c90-108">Azure Machine Learning</span></span>
* <span data-ttu-id="40c90-109">Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="40c90-109">Azure Stream Analytics</span></span>

<span data-ttu-id="40c90-110">Pracujeme na připojení s více službami napříč ekosystému Azure.</span><span class="sxs-lookup"><span data-stu-id="40c90-110">We are working to connect with more services across the Azure ecosystem.</span></span>

## <a name="power-bi"></a><span data-ttu-id="40c90-111">Power BI</span><span class="sxs-lookup"><span data-stu-id="40c90-111">Power BI</span></span>
<span data-ttu-id="40c90-112">Integrace Power BI umožňuje využít výpočetní výkon služby SQL Data Warehouse s dynamické generování sestav a vizualizaci Power BI.</span><span class="sxs-lookup"><span data-stu-id="40c90-112">Power BI integration allows you to leverage the compute power of SQL Data Warehouse with the dynamic reporting and visualization of Power BI.</span></span> <span data-ttu-id="40c90-113">Integrace Power BI aktuálně zahrnuje:</span><span class="sxs-lookup"><span data-stu-id="40c90-113">Power BI integration currently includes:</span></span>

* <span data-ttu-id="40c90-114">**Přímé připojení**: více rozšířených připojení s logické přenos směrem dolů vůči SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="40c90-114">**Direct Connect**: A more advanced connection with logical pushdown against SQL Data Warehouse.</span></span>  <span data-ttu-id="40c90-115">To poskytuje rychlejší analysis ve větším měřítku.</span><span class="sxs-lookup"><span data-stu-id="40c90-115">This provides faster analysis on a larger scale.</span></span>
* <span data-ttu-id="40c90-116">**Otevřít v Power BI**: tlačítko Otevřít v Power BI předává informace o instanci Power BI umožňuje pro bezproblémové připojení.</span><span class="sxs-lookup"><span data-stu-id="40c90-116">**Open in Power BI**: The 'Open in Power BI' button passes instance information to Power BI, allowing for a more seamless connection.</span></span>

<span data-ttu-id="40c90-117">V tématu [integrací s Power BI](sql-data-warehouse-integrate-power-bi.md) nebo [Power BI dokumentaci](http://blogs.msdn.com/b/powerbi/archive/2015/06/24/exploring-azure-sql-data-warehouse-with-power-bi.aspx) Další informace.</span><span class="sxs-lookup"><span data-stu-id="40c90-117">See [Integrate with Power BI](sql-data-warehouse-integrate-power-bi.md) or the [Power BI documentation](http://blogs.msdn.com/b/powerbi/archive/2015/06/24/exploring-azure-sql-data-warehouse-with-power-bi.aspx) for more information.</span></span>

## <a name="azure-data-factory"></a><span data-ttu-id="40c90-118">Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="40c90-118">Azure Data Factory</span></span>
<span data-ttu-id="40c90-119">Azure Data Factory poskytuje uživatelům spravovaná platforma vytvořit komplexní extrakce zatížení kanály.</span><span class="sxs-lookup"><span data-stu-id="40c90-119">Azure Data Factory gives users a managed platform to create complex Extract-Load pipelines.</span></span>  <span data-ttu-id="40c90-120">SQL Data Warehouse integraci s Azure Data Factory zahrnuje následující položky:</span><span class="sxs-lookup"><span data-stu-id="40c90-120">SQL Data Warehouse's integration with Azure Data Factory includes the following:</span></span>

* <span data-ttu-id="40c90-121">**Uložené procedury**: orchestraci provádění uložené procedury v SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="40c90-121">**Stored Procedures**: Orchestrate the execution of stored procedures on SQL Data Warehouse.</span></span>
* <span data-ttu-id="40c90-122">**Kopírování**: použití ADF pro přesun dat do SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="40c90-122">**Copy**: Use ADF to move data into SQL Data Warehouse.</span></span>  <span data-ttu-id="40c90-123">Tato operace můžete v pozadí využívají mechanismus přesun ADF standardní dat nebo PolyBase.</span><span class="sxs-lookup"><span data-stu-id="40c90-123">This operation can use ADF's standard data movement mechanism or PolyBase under the covers.</span></span> 

<span data-ttu-id="40c90-124">V tématu [integrací s Azure Data Factory](sql-data-warehouse-integrate-azure-data-factory.md) nebo [dokumentace Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) Další informace.</span><span class="sxs-lookup"><span data-stu-id="40c90-124">See [Integrate with Azure Data Factory](sql-data-warehouse-integrate-azure-data-factory.md) or the [Azure Data Factory documentation](https://azure.microsoft.com/documentation/services/data-factory/) for more information.</span></span>

## <a name="azure-machine-learning"></a><span data-ttu-id="40c90-125">Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="40c90-125">Azure Machine Learning</span></span>
<span data-ttu-id="40c90-126">Azure Machine Learning je plně spravovaná analytics službu, která umožňuje uživatelům vytvářet složité modely využití velké sady prediktivní nástroje.</span><span class="sxs-lookup"><span data-stu-id="40c90-126">Azure Machine Learning is a fully managed analytics service which allows users to create intricate models leveraging a large set of predictive tools.</span></span>  <span data-ttu-id="40c90-127">SQL Data Warehouse je podporovaný jako zdrojovém i cílovém pro tyto modely následující funkce:</span><span class="sxs-lookup"><span data-stu-id="40c90-127">SQL Data Warehouse is supported as both a source and destination for these models with the following functionality:</span></span>

* <span data-ttu-id="40c90-128">**Čtení dat:** jednotky modely škálované pomocí T-SQL s SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="40c90-128">**Read Data:** Drive models at scale using T-SQL against SQL Data Warehouse.</span></span>
* <span data-ttu-id="40c90-129">**Zapisovat Data:** potvrzení změny z kteréhokoli modelu zpět do SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="40c90-129">**Write Data:** Commit changes from any model back to SQL Data Warehouse.</span></span>

<span data-ttu-id="40c90-130">V tématu [integrací se službami Azure Machine Learning](sql-data-warehouse-integrate-azure-machine-learning.md) nebo [dokumentace Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) Další informace.</span><span class="sxs-lookup"><span data-stu-id="40c90-130">See [Integrate with Azure Machine Learning](sql-data-warehouse-integrate-azure-machine-learning.md) or the [Azure Machine Learning documentation](https://azure.microsoft.com/services/machine-learning/) for more information.</span></span>

## <a name="azure-stream-analytics"></a><span data-ttu-id="40c90-131">Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="40c90-131">Azure Stream Analytics</span></span>
<span data-ttu-id="40c90-132">Azure Stream Analytics je komplexní, plně spravovaná infrastruktury pro zpracování a využívají data události vygenerovaná z centra událostí Azure.</span><span class="sxs-lookup"><span data-stu-id="40c90-132">Azure Stream Analytics is a complex, fully managed infrastructure for processing and consuming event data generated from Azure Event Hub.</span></span>  <span data-ttu-id="40c90-133">Integrace s SQL Data Warehouse umožňuje dat účinně zpracovat a uložená spolu s relačních dat povolení hlubší, pokročilejší analýzy datových proudů.</span><span class="sxs-lookup"><span data-stu-id="40c90-133">Integration with SQL Data Warehouse allows for streaming data to be effectively processed and stored alongside relational data enabling deeper, more advanced analysis.</span></span>  

* <span data-ttu-id="40c90-134">**Výstup úlohy:** odeslání výstupu z úlohy Stream Analytics přímo do SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="40c90-134">**Job Output:** Send output from Stream Analytics jobs directly to SQL Data Warehouse.</span></span>

<span data-ttu-id="40c90-135">V tématu [integrací se službami Azure Stream Analytics](sql-data-warehouse-integrate-azure-stream-analytics.md) nebo [dokumentace Azure Stream Analytics](https://azure.microsoft.com/documentation/services/stream-analytics/) Další informace.</span><span class="sxs-lookup"><span data-stu-id="40c90-135">See [Integrate with Azure Stream Analytics](sql-data-warehouse-integrate-azure-stream-analytics.md) or the [Azure Stream Analytics documentation](https://azure.microsoft.com/documentation/services/stream-analytics/) for more information.</span></span>

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop/

[Azure Data Factory]: sql-data-warehouse-integrate-azure-data-factory.md
[Azure Machine Learning]: sql-data-warehouse-integrate-azure-machine-learning.md
[Azure Stream Analytics]: sql-data-warehouse-integrate-azure-stream-analytics.md
[Power BI]: sql-data-warehouse-integrate-power-bi.md
[Partners]: sql-data-warehouse-partner-business-intelligence.md

<!--MSDN references-->

<!--Other Web references-->
