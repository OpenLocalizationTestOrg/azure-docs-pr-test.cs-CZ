---
title: "aaaHow tooview související datových prostředcích v Azure Data Catalog | Microsoft Docs"
description: "Tento článek vysvětluje, jak tooview související s datovým prostředkům vybrané datové prostředky v Azure Data Catalog."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/17/2017
ms.author: maroche
ms.openlocfilehash: b69686737070ac563a0318f48e693215c605f90b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooview-related-data-assets-in-azure-data-catalog"></a><span data-ttu-id="02eb8-103">Jak tooview související datových prostředcích v Azure Data Catalog?</span><span class="sxs-lookup"><span data-stu-id="02eb8-103">How tooview related data assets in Azure Data Catalog?</span></span>
<span data-ttu-id="02eb8-104">Azure Data Catalog umožňuje tooview data prostředky související tooa vybraná data asset a zobrazení vztahy mezi nimi.</span><span class="sxs-lookup"><span data-stu-id="02eb8-104">Azure Data Catalog allows you tooview data assets related tooa selected data asset and view relationships between them.</span></span> 

## <a name="supported-data-sources"></a><span data-ttu-id="02eb8-105">Podporované zdroje dat</span><span class="sxs-lookup"><span data-stu-id="02eb8-105">Supported data sources</span></span> 
<span data-ttu-id="02eb8-106">Při registraci datových prostředků z hello následující zdroje dat Azure Data Catalog automaticky registruje metadata o relace typu join mezi hello vybrané datové prostředky.</span><span class="sxs-lookup"><span data-stu-id="02eb8-106">When you register data assets from hello following data sources, Azure Data Catalog automatically registers metadata about join relationships between hello selected data assets.</span></span> 

- <span data-ttu-id="02eb8-107">SQL Server</span><span class="sxs-lookup"><span data-stu-id="02eb8-107">SQL Server</span></span>
- <span data-ttu-id="02eb8-108">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="02eb8-108">Azure SQL Database</span></span>
- <span data-ttu-id="02eb8-109">MySQL</span><span class="sxs-lookup"><span data-stu-id="02eb8-109">MySQL</span></span>
- <span data-ttu-id="02eb8-110">Oracle</span><span class="sxs-lookup"><span data-stu-id="02eb8-110">Oracle</span></span>

## <a name="view-related-data-assets"></a><span data-ttu-id="02eb8-111">Zobrazit související datových prostředků</span><span class="sxs-lookup"><span data-stu-id="02eb8-111">View related data assets</span></span>
<span data-ttu-id="02eb8-112">tooview datové prostředky, které jsou související tooa vybrané datové sady, použijte hello **vztahy** kartě, jak ukazuje následující obrázek hello:</span><span class="sxs-lookup"><span data-stu-id="02eb8-112">tooview data assets that are related tooa selected dataset, use hello **Relationships** tab as shown in hello following image:</span></span> 

![Azure Data Catalog – zobrazení související s datovým prostředkům](media\data-catalog-how-to-view-related-data-assets\relationships-tab.png)

<span data-ttu-id="02eb8-114">V tomto příkladu jsou dva vztahy hello vybrané **ProductSubcategory** datový prostředek:</span><span class="sxs-lookup"><span data-stu-id="02eb8-114">In this example, there are two relationships for hello selected **ProductSubcategory** data asset:</span></span> 

- <span data-ttu-id="02eb8-115">ProductSubcategoryID sloupec tabulky produktu hello má relace cizího klíče s ProductSubcategoryID sloupec tabulky ProductSubcategory hello vybrané.</span><span class="sxs-lookup"><span data-stu-id="02eb8-115">ProductSubcategoryID column of hello Product table has a foreign key relationship with ProductSubcategoryID column of hello selected ProductSubcategory table.</span></span> 
- <span data-ttu-id="02eb8-116">ProductCategoryID sloupec tabulky ProductSubCategory hello má relace cizího klíče se sloupcem ProductCategoryID hello vybrané ProductCategory tabulky.</span><span class="sxs-lookup"><span data-stu-id="02eb8-116">ProductCategoryID column of hello ProductSubCategory table has a foreign key relationship with ProductCategoryID column of hello selected ProductCategory table.</span></span>

> [!NOTE]
> <span data-ttu-id="02eb8-117">Všimněte si, směr hello hello šipky v zobrazení stromu vztahů hello.</span><span class="sxs-lookup"><span data-stu-id="02eb8-117">Notice hello direction of hello arrow in hello relationships tree view.</span></span>  

<span data-ttu-id="02eb8-118">toosee další podrobnosti, jako je plně kvalifikovaný název hello hello sloupce, přesuňte myš hello přes a zobrazí místní okno podobné toohello následující bitové kopie:</span><span class="sxs-lookup"><span data-stu-id="02eb8-118">toosee more details such as hello fully qualified name of hello column, move hello mouse over and you see a popup similar toohello following image:</span></span> 

![Azure Data Catalog – místní relace](media\data-catalog-how-to-view-related-data-assets\relationship-popup.png)

<span data-ttu-id="02eb8-120">tooinclude vztahy mezi prostředky, které jsou již zaregistrovány, zaregistrujte znovu tyto prostředky.</span><span class="sxs-lookup"><span data-stu-id="02eb8-120">tooinclude relationships between assets that have already been registered, re-register those assets.</span></span>

## <a name="next-steps"></a><span data-ttu-id="02eb8-121">Další kroky</span><span class="sxs-lookup"><span data-stu-id="02eb8-121">Next steps</span></span>
- [<span data-ttu-id="02eb8-122">Jak toomanage datových prostředků</span><span class="sxs-lookup"><span data-stu-id="02eb8-122">How toomanage data assets</span></span>](data-catalog-how-to-manage.md)
