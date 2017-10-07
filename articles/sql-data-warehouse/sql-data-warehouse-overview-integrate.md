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
# <a name="leverage-other-services-with-sql-data-warehouse"></a>Využít dalším službám pomocí SQL Data Warehouse
Kromě toho tooits základní funkce, SQL Data Warehouse umožňuje uživatelům tooleverage řadu hello jiných služeb společně se ho v Azure.  Konkrétně jsme provedli aktuálně toodeeply integrovat hello následující kroky:

* Power BI
* Azure Data Factory
* Azure Machine Learning
* Azure Stream Analytics

Mezi hello Azure ekosystém pracujeme tooconnect s více službami.

## <a name="power-bi"></a>Power BI
Integrace Power BI vám umožní tooleverage hello výpočetní výkon služby SQL Data Warehouse s hello dynamické generování sestav a vizualizaci Power BI. Integrace Power BI aktuálně zahrnuje:

* **Přímé připojení**: více rozšířených připojení s logické přenos směrem dolů vůči SQL Data Warehouse.  To poskytuje rychlejší analysis ve větším měřítku.
* **Otevřít v Power BI**: tlačítko Otevřít v Power BI hello předá instance informace tooPower BI, povolení pro více snadné připojení.

V tématu [integrací s Power BI](sql-data-warehouse-integrate-power-bi.md) nebo hello [Power BI dokumentaci](http://blogs.msdn.com/b/powerbi/archive/2015/06/24/exploring-azure-sql-data-warehouse-with-power-bi.aspx) Další informace.

## <a name="azure-data-factory"></a>Azure Data Factory
Azure Data Factory poskytuje uživatelům toocreate spravovaná platforma, které komplexní extrakce zatížení kanálů.  SQL Data Warehouse integraci s Azure Data Factory obsahuje hello následující:

* **Uložené procedury**: orchestraci hello provádění uložené procedury v SQL Data Warehouse.
* **Kopírování**: použití ADF toomove dat do SQL Data Warehouse.  Tato operace můžete použít mechanismus přesun ADF standardní dat nebo popisuje PolyBase pod hello. 

V tématu [integrací s Azure Data Factory](sql-data-warehouse-integrate-azure-data-factory.md) nebo hello [dokumentace Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) Další informace.

## <a name="azure-machine-learning"></a>Azure Machine Learning
Azure Machine Learning je plně spravovaná analytics službu, která umožňuje uživatelům toocreate komplikované modely využití velké sady prediktivní nástroje.  SQL Data Warehouse je podporovaný jako zdrojovém i cílovém pro tyto modely s hello následující funkce:

* **Čtení dat:** jednotky modely škálované pomocí T-SQL s SQL Data Warehouse.
* **Zapisovat Data:** potvrzení změn z kteréhokoli modelu zpět tooSQL datového skladu.

V tématu [integrací se službami Azure Machine Learning](sql-data-warehouse-integrate-azure-machine-learning.md) nebo hello [dokumentace Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) Další informace.

## <a name="azure-stream-analytics"></a>Azure Stream Analytics
Azure Stream Analytics je komplexní, plně spravovaná infrastruktury pro zpracování a využívají data události vygenerovaná z centra událostí Azure.  Integrace s SQL Data Warehouse umožňuje streamování dat toobe účinně zpracovat a uložená spolu s relačních dat povolení hlubší, další pokročilé analýzy.  

* **Výstup úlohy:** odeslání výstupu ze služby Stream Analytics úlohy přímo tooSQL datového skladu.

V tématu [integrací se službami Azure Stream Analytics](sql-data-warehouse-integrate-azure-stream-analytics.md) nebo hello [dokumentace Azure Stream Analytics](https://azure.microsoft.com/documentation/services/stream-analytics/) Další informace.

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
