---
title: "úroveň kompatibility modelu aaaData ve službě Azure Analysis Services | Microsoft Docs"
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
ms.openlocfilehash: bfaf0c60666729d1e6e0baf082c046ea9faa4e86
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="compatibility-level-for-analysis-services-tabular-models"></a><span data-ttu-id="29f5d-103">Úroveň kompatibility pro tabulkové modely služby Analysis Services</span><span class="sxs-lookup"><span data-stu-id="29f5d-103">Compatibility level for Analysis Services tabular models</span></span>

<span data-ttu-id="29f5d-104">*Úroveň kompatibility* odkazuje toorelease konkrétní chování v hello stroje služby Analysis Services.</span><span class="sxs-lookup"><span data-stu-id="29f5d-104">*Compatibility level* refers toorelease-specific behaviors in hello Analysis Services engine.</span></span> <span data-ttu-id="29f5d-105">Úroveň kompatibility toohello změny obvykle shodovat se hlavní verze systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="29f5d-105">Changes toohello compatibility level typically coincide with major releases of SQL Server.</span></span> <span data-ttu-id="29f5d-106">Tyto změny jsou také implementované v Azure Analysis Services toomaintain rozdíly mezi obě platformy.</span><span class="sxs-lookup"><span data-stu-id="29f5d-106">These changes are also implemented in Azure Analysis Services toomaintain parity between both platforms.</span></span> <span data-ttu-id="29f5d-107">Kompatibilita úrovně ovlivní také funkce dostupné v tabulkové modely.</span><span class="sxs-lookup"><span data-stu-id="29f5d-107">Compatibility level changes also affect features available in your tabular models.</span></span> <span data-ttu-id="29f5d-108">Například DirectQuery a metadat pro tabulkový objektový mají různé implementace v závislosti na úrovni kompatibility hello.</span><span class="sxs-lookup"><span data-stu-id="29f5d-108">For example, DirectQuery and tabular object metadata have different implementations depending on hello compatibility level.</span></span> 

<span data-ttu-id="29f5d-109">Azure Analysis Services podporuje tabulkové modely na úrovní kompatibility 1200 a 1400 hello.</span><span class="sxs-lookup"><span data-stu-id="29f5d-109">Azure Analysis Services supports tabular models at hello 1200 and 1400 compatibility levels.</span></span>

<span data-ttu-id="29f5d-110">nejnovější úroveň kompatibility Hello je 1400.</span><span class="sxs-lookup"><span data-stu-id="29f5d-110">hello latest compatibility level is 1400.</span></span> <span data-ttu-id="29f5d-111">Tato úroveň se shoduje s SQL Server 2017 Analysis Services.</span><span class="sxs-lookup"><span data-stu-id="29f5d-111">This level coincides with SQL Server 2017 Analysis Services.</span></span> <span data-ttu-id="29f5d-112">Hlavní funkce v úroveň kompatibility 1400 hello:</span><span class="sxs-lookup"><span data-stu-id="29f5d-112">Major features in hello 1400 compatibility level include:</span></span>

*  <span data-ttu-id="29f5d-113">Novou infrastrukturu pro datové připojení a import do tabulkové modely s podporou pro rozhraní API TNÍ a TMSL skriptování.</span><span class="sxs-lookup"><span data-stu-id="29f5d-113">New infrastructure for data connectivity and import into tabular models with support for TOM APIs and TMSL scripting.</span></span> <span data-ttu-id="29f5d-114">Tato nová funkce umožňuje podporu pro další datové zdroje, jako je například úložiště objektů Blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="29f5d-114">This new feature enables support for additional data sources such as Azure Blob storage.</span></span>
*  <span data-ttu-id="29f5d-115">Transformace dat a možnosti data hybridní webové aplikace pomocí výrazy načíst Data a M.</span><span class="sxs-lookup"><span data-stu-id="29f5d-115">Data transformation and data mashup capabilities by using Get Data and M expressions.</span></span>
*  <span data-ttu-id="29f5d-116">Míry podporovat vlastnost řádky s podrobnostmi s výraz DAX.</span><span class="sxs-lookup"><span data-stu-id="29f5d-116">Measures support a Detail Rows property with a DAX expression.</span></span> <span data-ttu-id="29f5d-117">Tato vlastnost umožňuje nástroje klienta, například aplikace Microsoft Excel toodrill dolů toodetailed data ze souhrnné sestavy.</span><span class="sxs-lookup"><span data-stu-id="29f5d-117">This property enables client tools like Microsoft Excel toodrill down toodetailed data from an aggregated report.</span></span> <span data-ttu-id="29f5d-118">Například pokud uživatelé zobrazit celkový prodej oblasti a měsíc, mohou prohlížet hello související podrobnosti pořadí.</span><span class="sxs-lookup"><span data-stu-id="29f5d-118">For example, when users view total sales for a region and month, they can view hello associated order details.</span></span> 
*  <span data-ttu-id="29f5d-119">Zabezpečení na úrovni objektu pro tabulky a sloupce názvy kromě toohello dat mezi nimi.</span><span class="sxs-lookup"><span data-stu-id="29f5d-119">Object-level security for table and column names, in addition toohello data within them.</span></span>
*  <span data-ttu-id="29f5d-120">Vylepšená podpora pro nepravidelné hierarchie.</span><span class="sxs-lookup"><span data-stu-id="29f5d-120">Enhanced support for ragged hierarchies.</span></span>
*  <span data-ttu-id="29f5d-121">Výkon a vylepšení sledování.</span><span class="sxs-lookup"><span data-stu-id="29f5d-121">Performance and monitoring improvements.</span></span>
  
## <a name="set-compatibility-level"></a><span data-ttu-id="29f5d-122">Úroveň kompatibility sady</span><span class="sxs-lookup"><span data-stu-id="29f5d-122">Set compatibility level</span></span> 
 <span data-ttu-id="29f5d-123">Při vytváření nového projektu tabulkový model v sadě SSDT, můžete zadat úroveň kompatibility hello na hello **tabulkový model designer** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="29f5d-123">When creating a new tabular model project in SSDT, you can specify hello compatibility level on hello **Tabular model designer** dialog.</span></span> 
  
 ![ssas_tabularproject_compat1200](./media/analysis-services-compat-level/aas-tabularproject-compat.png)  
  
 <span data-ttu-id="29f5d-125">Pokud vyberete hello **tuto zprávu již příště nezobrazovat** možnost, všechny následné projekty pomocí úroveň kompatibility hello jste zadali jako výchozí hello.</span><span class="sxs-lookup"><span data-stu-id="29f5d-125">If you select hello **Do not show this message again** option, all subsequent projects use hello compatibility level you specified as hello default.</span></span> <span data-ttu-id="29f5d-126">Můžete změnit úroveň kompatibility výchozí hello v sadě SSDT v **nástroje** > **možnosti**.</span><span class="sxs-lookup"><span data-stu-id="29f5d-126">You can change hello default compatibility level in SSDT in **Tools** > **Options**.</span></span>  
  
 <span data-ttu-id="29f5d-127">tooupgrade existujícího projektu tabulkový model v sadě SSDT, sada hello **úroveň kompatibility** vlastnost v modelu hello **vlastnosti** okno.</span><span class="sxs-lookup"><span data-stu-id="29f5d-127">tooupgrade an existing tabular model project in SSDT, set  hello **Compatibility Level** property in hello model **Properties** window.</span></span> <span data-ttu-id="29f5d-128">Zachovat v paměti, upgrade úroveň kompatibility hello je nevratné.</span><span class="sxs-lookup"><span data-stu-id="29f5d-128">Keep in-mind, upgrading hello compatibility level is irreversible.</span></span>
  
## <a name="check-compatibility-level-for-a-tabular-model-database-in-sql-server-management-studio"></a><span data-ttu-id="29f5d-129">Zkontrolujte úroveň kompatibility pro databázi tabulkového modelu v aplikaci SQL Server Management Studio</span><span class="sxs-lookup"><span data-stu-id="29f5d-129">Check compatibility level for a tabular model database in SQL Server Management Studio</span></span> 
 <span data-ttu-id="29f5d-130">V aplikaci SSMS, klikněte pravým tlačítkem na název databáze hello > **vlastnosti** > **úroveň kompatibility**.</span><span class="sxs-lookup"><span data-stu-id="29f5d-130">In SSMS, right-click hello database name > **Properties** > **Compatibility Level**.</span></span>  
  
## <a name="check-supported-compatibility-level-for-a-server-in-ssms"></a><span data-ttu-id="29f5d-131">Zkontrolujte úroveň kompatibility podporované pro server v aplikaci SSMS</span><span class="sxs-lookup"><span data-stu-id="29f5d-131">Check supported compatibility level for a server in SSMS</span></span>  
 <span data-ttu-id="29f5d-132">V aplikaci SSMS, klikněte pravým tlačítkem na název serveru hello > **vlastnosti** > **podporované úroveň kompatibility**.</span><span class="sxs-lookup"><span data-stu-id="29f5d-132">In SSMS, right-click hello server name>  **Properties** > **Supported Compatibility Level**.</span></span>  
  
 <span data-ttu-id="29f5d-133">Tato vlastnost určuje hello nejvyšší úroveň kompatibility databáze, která se spustí na serveru hello (s výjimkou preview).</span><span class="sxs-lookup"><span data-stu-id="29f5d-133">This property specifies hello highest compatibility level of a database that will run on hello server (excluding preview).</span></span> <span data-ttu-id="29f5d-134">nelze změnit úroveň kompatibility Hello podporována.</span><span class="sxs-lookup"><span data-stu-id="29f5d-134">hello supported compatibility level cannot be changed.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="29f5d-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="29f5d-135">Next steps</span></span>
  <span data-ttu-id="29f5d-136">[Vytvoření modelu na portálu Azure](analysis-services-create-model-portal.md) </span><span class="sxs-lookup"><span data-stu-id="29f5d-136">[Create a model in Azure portal](analysis-services-create-model-portal.md) </span></span>  
  [<span data-ttu-id="29f5d-137">Správa služby Analysis Services</span><span class="sxs-lookup"><span data-stu-id="29f5d-137">Manage Analysis Services</span></span>](analysis-services-manage.md)  
