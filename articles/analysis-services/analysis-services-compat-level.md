---
title: "Datový model úroveň kompatibility v Azure Analysis Services | Microsoft Docs"
description: "Seznámení s úrovní kompatibility tabulková data modelu."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/16/2017
ms.author: owend
ms.openlocfilehash: b11ba54c2cdc2675ec535368e7076613a5290212
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="compatibility-level-for-analysis-services-tabular-models"></a><span data-ttu-id="c2ddd-103">Úroveň kompatibility pro tabulkové modely služby Analysis Services</span><span class="sxs-lookup"><span data-stu-id="c2ddd-103">Compatibility level for Analysis Services tabular models</span></span>

<span data-ttu-id="c2ddd-104">*Úroveň kompatibility* odkazuje na konkrétní verzi chování v rámci stroje služby Analysis Services.</span><span class="sxs-lookup"><span data-stu-id="c2ddd-104">*Compatibility level* refers to release-specific behaviors in the Analysis Services engine.</span></span> <span data-ttu-id="c2ddd-105">Změny úrovně kompatibility se obvykle shodují s hlavní verze systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="c2ddd-105">Changes to the compatibility level typically coincide with major releases of SQL Server.</span></span> <span data-ttu-id="c2ddd-106">Tyto změny jsou také implementované v Azure Analysis Services k udržování rozdíly mezi obě platformy.</span><span class="sxs-lookup"><span data-stu-id="c2ddd-106">These changes are also implemented in Azure Analysis Services to maintain parity between both platforms.</span></span> <span data-ttu-id="c2ddd-107">Kompatibilita úrovně ovlivní také funkce dostupné v tabulkové modely.</span><span class="sxs-lookup"><span data-stu-id="c2ddd-107">Compatibility level changes also affect features available in your tabular models.</span></span> <span data-ttu-id="c2ddd-108">Například DirectQuery a metadat pro tabulkový objektový mají různé implementace v závislosti na úrovni kompatibility.</span><span class="sxs-lookup"><span data-stu-id="c2ddd-108">For example, DirectQuery and tabular object metadata have different implementations depending on the compatibility level.</span></span> 

<span data-ttu-id="c2ddd-109">Azure Analysis Services podporuje tabulkové modely na úrovni kompatibility 1200 nebo 1400.</span><span class="sxs-lookup"><span data-stu-id="c2ddd-109">Azure Analysis Services supports tabular models at the 1200 and 1400 compatibility levels.</span></span>

<span data-ttu-id="c2ddd-110">Nejnovější úroveň kompatibility je 1400.</span><span class="sxs-lookup"><span data-stu-id="c2ddd-110">The latest compatibility level is 1400.</span></span> <span data-ttu-id="c2ddd-111">Tato úroveň se shoduje s SQL Server 2017 Analysis Services.</span><span class="sxs-lookup"><span data-stu-id="c2ddd-111">This level coincides with SQL Server 2017 Analysis Services.</span></span> <span data-ttu-id="c2ddd-112">Mezi hlavní funkce na úrovni kompatibility 1400 patří:</span><span class="sxs-lookup"><span data-stu-id="c2ddd-112">Major features in the 1400 compatibility level include:</span></span>

*  <span data-ttu-id="c2ddd-113">Novou infrastrukturu pro datové připojení a import do tabulkové modely s podporou pro rozhraní API TNÍ a TMSL skriptování.</span><span class="sxs-lookup"><span data-stu-id="c2ddd-113">New infrastructure for data connectivity and import into tabular models with support for TOM APIs and TMSL scripting.</span></span> <span data-ttu-id="c2ddd-114">Tato nová funkce umožňuje podporu pro další datové zdroje, jako je například úložiště objektů Blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="c2ddd-114">This new feature enables support for additional data sources such as Azure Blob storage.</span></span>
*  <span data-ttu-id="c2ddd-115">Transformace dat a možnosti data hybridní webové aplikace pomocí výrazy načíst Data a M.</span><span class="sxs-lookup"><span data-stu-id="c2ddd-115">Data transformation and data mashup capabilities by using Get Data and M expressions.</span></span>
*  <span data-ttu-id="c2ddd-116">Míry podporovat vlastnost řádky s podrobnostmi s výraz DAX.</span><span class="sxs-lookup"><span data-stu-id="c2ddd-116">Measures support a Detail Rows property with a DAX expression.</span></span> <span data-ttu-id="c2ddd-117">Tato vlastnost umožňuje nástroje klienta, například aplikace Microsoft Excel k podrobnostem podrobná data ze souhrnné sestavy.</span><span class="sxs-lookup"><span data-stu-id="c2ddd-117">This property enables client tools like Microsoft Excel to drill down to detailed data from an aggregated report.</span></span> <span data-ttu-id="c2ddd-118">Když uživatelé zobrazit celkový prodej oblasti a měsíc, mohou prohlížet podrobnosti související pořadí.</span><span class="sxs-lookup"><span data-stu-id="c2ddd-118">For example, when users view total sales for a region and month, they can view the associated order details.</span></span> 
*  <span data-ttu-id="c2ddd-119">Zabezpečení na úrovni objektu pro názvy tabulek a sloupců, kromě dat mezi nimi.</span><span class="sxs-lookup"><span data-stu-id="c2ddd-119">Object-level security for table and column names, in addition to the data within them.</span></span>
*  <span data-ttu-id="c2ddd-120">Vylepšená podpora pro nepravidelné hierarchie.</span><span class="sxs-lookup"><span data-stu-id="c2ddd-120">Enhanced support for ragged hierarchies.</span></span>
*  <span data-ttu-id="c2ddd-121">Výkon a vylepšení sledování.</span><span class="sxs-lookup"><span data-stu-id="c2ddd-121">Performance and monitoring improvements.</span></span>
  
## <a name="set-compatibility-level"></a><span data-ttu-id="c2ddd-122">Úroveň kompatibility sady</span><span class="sxs-lookup"><span data-stu-id="c2ddd-122">Set compatibility level</span></span> 
 <span data-ttu-id="c2ddd-123">Při vytváření nového projektu tabulkový model v sadě SSDT, můžete zadat úroveň kompatibility na **tabulkový model designer** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c2ddd-123">When creating a new tabular model project in SSDT, you can specify the compatibility level on the **Tabular model designer** dialog.</span></span> 
  
 ![ssas_tabularproject_compat1200](./media/analysis-services-compat-level/aas-tabularproject-compat.png)  
  
 <span data-ttu-id="c2ddd-125">Pokud jste vybrali **tuto zprávu již příště nezobrazovat** možnost, všechny následné projekty pomocí úroveň kompatibility, který jste zadali jako výchozí.</span><span class="sxs-lookup"><span data-stu-id="c2ddd-125">If you select the **Do not show this message again** option, all subsequent projects use the compatibility level you specified as the default.</span></span> <span data-ttu-id="c2ddd-126">Můžete změnit výchozí úroveň kompatibility v sadě SSDT v **nástroje** > **možnosti**.</span><span class="sxs-lookup"><span data-stu-id="c2ddd-126">You can change the default compatibility level in SSDT in **Tools** > **Options**.</span></span>  
  
 <span data-ttu-id="c2ddd-127">Pokud chcete upgradovat existující tabulkový model projekt v sadě SSDT, nastavte **úroveň kompatibility** vlastnost v modelu **vlastnosti** okno.</span><span class="sxs-lookup"><span data-stu-id="c2ddd-127">To upgrade an existing tabular model project in SSDT, set  the **Compatibility Level** property in the model **Properties** window.</span></span> <span data-ttu-id="c2ddd-128">Zachovat v paměti, upgrade úroveň kompatibility je nevratné.</span><span class="sxs-lookup"><span data-stu-id="c2ddd-128">Keep in-mind, upgrading the compatibility level is irreversible.</span></span>
  
## <a name="check-compatibility-level-for-a-tabular-model-database-in-sql-server-management-studio"></a><span data-ttu-id="c2ddd-129">Zkontrolujte úroveň kompatibility pro databázi tabulkového modelu v aplikaci SQL Server Management Studio</span><span class="sxs-lookup"><span data-stu-id="c2ddd-129">Check compatibility level for a tabular model database in SQL Server Management Studio</span></span> 
 <span data-ttu-id="c2ddd-130">V aplikaci SSMS, klikněte pravým tlačítkem na název databáze > **vlastnosti** > **úroveň kompatibility**.</span><span class="sxs-lookup"><span data-stu-id="c2ddd-130">In SSMS, right-click the database name > **Properties** > **Compatibility Level**.</span></span>  
  
## <a name="check-supported-compatibility-level-for-a-server-in-ssms"></a><span data-ttu-id="c2ddd-131">Zkontrolujte úroveň kompatibility podporované pro server v aplikaci SSMS</span><span class="sxs-lookup"><span data-stu-id="c2ddd-131">Check supported compatibility level for a server in SSMS</span></span>  
 <span data-ttu-id="c2ddd-132">V aplikaci SSMS, klikněte pravým tlačítkem na název serveru > **vlastnosti** > **podporované úroveň kompatibility**.</span><span class="sxs-lookup"><span data-stu-id="c2ddd-132">In SSMS, right-click the server name>  **Properties** > **Supported Compatibility Level**.</span></span>  
  
 <span data-ttu-id="c2ddd-133">Tato vlastnost určuje nejvyšší úroveň kompatibility databáze, která se spustí na serveru (s výjimkou preview).</span><span class="sxs-lookup"><span data-stu-id="c2ddd-133">This property specifies the highest compatibility level of a database that will run on the server (excluding preview).</span></span> <span data-ttu-id="c2ddd-134">Nelze změnit úroveň kompatibility podporované.</span><span class="sxs-lookup"><span data-stu-id="c2ddd-134">The supported compatibility level cannot be changed.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="c2ddd-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c2ddd-135">Next steps</span></span>
  <span data-ttu-id="c2ddd-136">[Vytvoření modelu na portálu Azure](analysis-services-create-model-portal.md) </span><span class="sxs-lookup"><span data-stu-id="c2ddd-136">[Create a model in Azure portal](analysis-services-create-model-portal.md) </span></span>  
  [<span data-ttu-id="c2ddd-137">Správa služby Analysis Services</span><span class="sxs-lookup"><span data-stu-id="c2ddd-137">Manage Analysis Services</span></span>](analysis-services-manage.md)  
