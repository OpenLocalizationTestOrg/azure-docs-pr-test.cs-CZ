---
title: "aaaBuild integrované řešení s SQL Data Warehouse | Microsoft Docs"
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
ms.openlocfilehash: c8a4202dd84305bea4e4c2faf0e4791d026e794f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="leverage-other-services-with-sql-data-warehouse"></a><span data-ttu-id="cc6c8-103">Využít dalším službám pomocí SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="cc6c8-103">Leverage other services with SQL Data Warehouse</span></span>
<span data-ttu-id="cc6c8-104">Kromě toho tooits základní funkce, SQL Data Warehouse umožňuje uživatelům tooleverage řadu hello jiných služeb společně se ho v Azure.</span><span class="sxs-lookup"><span data-stu-id="cc6c8-104">In addition tooits core functionality, SQL Data Warehouse enables users tooleverage many of hello other services in Azure alongside it.</span></span>  <span data-ttu-id="cc6c8-105">Konkrétně jsme provedli aktuálně toodeeply integrovat hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="cc6c8-105">Specifically, we have currently taken steps toodeeply integrate with hello following:</span></span>

* <span data-ttu-id="cc6c8-106">Power BI</span><span class="sxs-lookup"><span data-stu-id="cc6c8-106">Power BI</span></span>
* <span data-ttu-id="cc6c8-107">Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="cc6c8-107">Azure Data Factory</span></span>
* <span data-ttu-id="cc6c8-108">Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="cc6c8-108">Azure Machine Learning</span></span>
* <span data-ttu-id="cc6c8-109">Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="cc6c8-109">Azure Stream Analytics</span></span>

<span data-ttu-id="cc6c8-110">Mezi hello Azure ekosystém pracujeme tooconnect s více službami.</span><span class="sxs-lookup"><span data-stu-id="cc6c8-110">We are working tooconnect with more services across hello Azure ecosystem.</span></span>

## <a name="power-bi"></a><span data-ttu-id="cc6c8-111">Power BI</span><span class="sxs-lookup"><span data-stu-id="cc6c8-111">Power BI</span></span>
<span data-ttu-id="cc6c8-112">Integrace Power BI vám umožní tooleverage hello výpočetní výkon služby SQL Data Warehouse s hello dynamické generování sestav a vizualizaci Power BI.</span><span class="sxs-lookup"><span data-stu-id="cc6c8-112">Power BI integration allows you tooleverage hello compute power of SQL Data Warehouse with hello dynamic reporting and visualization of Power BI.</span></span> <span data-ttu-id="cc6c8-113">Integrace Power BI aktuálně zahrnuje:</span><span class="sxs-lookup"><span data-stu-id="cc6c8-113">Power BI integration currently includes:</span></span>

* <span data-ttu-id="cc6c8-114">**Přímé připojení**: více rozšířených připojení s logické přenos směrem dolů vůči SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="cc6c8-114">**Direct Connect**: A more advanced connection with logical pushdown against SQL Data Warehouse.</span></span>  <span data-ttu-id="cc6c8-115">To poskytuje rychlejší analysis ve větším měřítku.</span><span class="sxs-lookup"><span data-stu-id="cc6c8-115">This provides faster analysis on a larger scale.</span></span>
* <span data-ttu-id="cc6c8-116">**Otevřít v Power BI**: tlačítko Otevřít v Power BI hello předá instance informace tooPower BI, povolení pro více snadné připojení.</span><span class="sxs-lookup"><span data-stu-id="cc6c8-116">**Open in Power BI**: hello 'Open in Power BI' button passes instance information tooPower BI, allowing for a more seamless connection.</span></span>

<span data-ttu-id="cc6c8-117">V tématu [integrací s Power BI](sql-data-warehouse-integrate-power-bi.md) nebo hello [Power BI dokumentaci](http://blogs.msdn.com/b/powerbi/archive/2015/06/24/exploring-azure-sql-data-warehouse-with-power-bi.aspx) Další informace.</span><span class="sxs-lookup"><span data-stu-id="cc6c8-117">See [Integrate with Power BI](sql-data-warehouse-integrate-power-bi.md) or hello [Power BI documentation](http://blogs.msdn.com/b/powerbi/archive/2015/06/24/exploring-azure-sql-data-warehouse-with-power-bi.aspx) for more information.</span></span>

## <a name="azure-data-factory"></a><span data-ttu-id="cc6c8-118">Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="cc6c8-118">Azure Data Factory</span></span>
<span data-ttu-id="cc6c8-119">Azure Data Factory poskytuje uživatelům toocreate spravovaná platforma, které komplexní extrakce zatížení kanálů.</span><span class="sxs-lookup"><span data-stu-id="cc6c8-119">Azure Data Factory gives users a managed platform toocreate complex Extract-Load pipelines.</span></span>  <span data-ttu-id="cc6c8-120">SQL Data Warehouse integraci s Azure Data Factory obsahuje hello následující:</span><span class="sxs-lookup"><span data-stu-id="cc6c8-120">SQL Data Warehouse's integration with Azure Data Factory includes hello following:</span></span>

* <span data-ttu-id="cc6c8-121">**Uložené procedury**: orchestraci hello provádění uložené procedury v SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="cc6c8-121">**Stored Procedures**: Orchestrate hello execution of stored procedures on SQL Data Warehouse.</span></span>
* <span data-ttu-id="cc6c8-122">**Kopírování**: použití ADF toomove dat do SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="cc6c8-122">**Copy**: Use ADF toomove data into SQL Data Warehouse.</span></span>  <span data-ttu-id="cc6c8-123">Tato operace můžete použít mechanismus přesun ADF standardní dat nebo popisuje PolyBase pod hello.</span><span class="sxs-lookup"><span data-stu-id="cc6c8-123">This operation can use ADF's standard data movement mechanism or PolyBase under hello covers.</span></span> 

<span data-ttu-id="cc6c8-124">V tématu [integrací s Azure Data Factory](sql-data-warehouse-integrate-azure-data-factory.md) nebo hello [dokumentace Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) Další informace.</span><span class="sxs-lookup"><span data-stu-id="cc6c8-124">See [Integrate with Azure Data Factory](sql-data-warehouse-integrate-azure-data-factory.md) or hello [Azure Data Factory documentation](https://azure.microsoft.com/documentation/services/data-factory/) for more information.</span></span>

## <a name="azure-machine-learning"></a><span data-ttu-id="cc6c8-125">Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="cc6c8-125">Azure Machine Learning</span></span>
<span data-ttu-id="cc6c8-126">Azure Machine Learning je plně spravovaná analytics službu, která umožňuje uživatelům toocreate komplikované modely využití velké sady prediktivní nástroje.</span><span class="sxs-lookup"><span data-stu-id="cc6c8-126">Azure Machine Learning is a fully managed analytics service which allows users toocreate intricate models leveraging a large set of predictive tools.</span></span>  <span data-ttu-id="cc6c8-127">SQL Data Warehouse je podporovaný jako zdrojovém i cílovém pro tyto modely s hello následující funkce:</span><span class="sxs-lookup"><span data-stu-id="cc6c8-127">SQL Data Warehouse is supported as both a source and destination for these models with hello following functionality:</span></span>

* <span data-ttu-id="cc6c8-128">**Čtení dat:** jednotky modely škálované pomocí T-SQL s SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="cc6c8-128">**Read Data:** Drive models at scale using T-SQL against SQL Data Warehouse.</span></span>
* <span data-ttu-id="cc6c8-129">**Zapisovat Data:** potvrzení změn z kteréhokoli modelu zpět tooSQL datového skladu.</span><span class="sxs-lookup"><span data-stu-id="cc6c8-129">**Write Data:** Commit changes from any model back tooSQL Data Warehouse.</span></span>

<span data-ttu-id="cc6c8-130">V tématu [integrací se službami Azure Machine Learning](sql-data-warehouse-integrate-azure-machine-learning.md) nebo hello [dokumentace Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) Další informace.</span><span class="sxs-lookup"><span data-stu-id="cc6c8-130">See [Integrate with Azure Machine Learning](sql-data-warehouse-integrate-azure-machine-learning.md) or hello [Azure Machine Learning documentation](https://azure.microsoft.com/services/machine-learning/) for more information.</span></span>

## <a name="azure-stream-analytics"></a><span data-ttu-id="cc6c8-131">Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="cc6c8-131">Azure Stream Analytics</span></span>
<span data-ttu-id="cc6c8-132">Azure Stream Analytics je komplexní, plně spravovaná infrastruktury pro zpracování a využívají data události vygenerovaná z centra událostí Azure.</span><span class="sxs-lookup"><span data-stu-id="cc6c8-132">Azure Stream Analytics is a complex, fully managed infrastructure for processing and consuming event data generated from Azure Event Hub.</span></span>  <span data-ttu-id="cc6c8-133">Integrace s SQL Data Warehouse umožňuje streamování dat toobe účinně zpracovat a uložená spolu s relačních dat povolení hlubší, další pokročilé analýzy.</span><span class="sxs-lookup"><span data-stu-id="cc6c8-133">Integration with SQL Data Warehouse allows for streaming data toobe effectively processed and stored alongside relational data enabling deeper, more advanced analysis.</span></span>  

* <span data-ttu-id="cc6c8-134">**Výstup úlohy:** odeslání výstupu ze služby Stream Analytics úlohy přímo tooSQL datového skladu.</span><span class="sxs-lookup"><span data-stu-id="cc6c8-134">**Job Output:** Send output from Stream Analytics jobs directly tooSQL Data Warehouse.</span></span>

<span data-ttu-id="cc6c8-135">V tématu [integrací se službami Azure Stream Analytics](sql-data-warehouse-integrate-azure-stream-analytics.md) nebo hello [dokumentace Azure Stream Analytics](https://azure.microsoft.com/documentation/services/stream-analytics/) Další informace.</span><span class="sxs-lookup"><span data-stu-id="cc6c8-135">See [Integrate with Azure Stream Analytics](sql-data-warehouse-integrate-azure-stream-analytics.md) or hello [Azure Stream Analytics documentation](https://azure.microsoft.com/documentation/services/stream-analytics/) for more information.</span></span>

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
