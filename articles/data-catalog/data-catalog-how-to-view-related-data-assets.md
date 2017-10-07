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
# <a name="how-tooview-related-data-assets-in-azure-data-catalog"></a>Jak tooview související datových prostředcích v Azure Data Catalog?
Azure Data Catalog umožňuje tooview data prostředky související tooa vybraná data asset a zobrazení vztahy mezi nimi. 

## <a name="supported-data-sources"></a>Podporované zdroje dat 
Při registraci datových prostředků z hello následující zdroje dat Azure Data Catalog automaticky registruje metadata o relace typu join mezi hello vybrané datové prostředky. 

- SQL Server
- Azure SQL Database
- MySQL
- Oracle

## <a name="view-related-data-assets"></a>Zobrazit související datových prostředků
tooview datové prostředky, které jsou související tooa vybrané datové sady, použijte hello **vztahy** kartě, jak ukazuje následující obrázek hello: 

![Azure Data Catalog – zobrazení související s datovým prostředkům](media\data-catalog-how-to-view-related-data-assets\relationships-tab.png)

V tomto příkladu jsou dva vztahy hello vybrané **ProductSubcategory** datový prostředek: 

- ProductSubcategoryID sloupec tabulky produktu hello má relace cizího klíče s ProductSubcategoryID sloupec tabulky ProductSubcategory hello vybrané. 
- ProductCategoryID sloupec tabulky ProductSubCategory hello má relace cizího klíče se sloupcem ProductCategoryID hello vybrané ProductCategory tabulky.

> [!NOTE]
> Všimněte si, směr hello hello šipky v zobrazení stromu vztahů hello.  

toosee další podrobnosti, jako je plně kvalifikovaný název hello hello sloupce, přesuňte myš hello přes a zobrazí místní okno podobné toohello následující bitové kopie: 

![Azure Data Catalog – místní relace](media\data-catalog-how-to-view-related-data-assets\relationship-popup.png)

tooinclude vztahy mezi prostředky, které jsou již zaregistrovány, zaregistrujte znovu tyto prostředky.

## <a name="next-steps"></a>Další kroky
- [Jak toomanage datových prostředků](data-catalog-how-to-manage.md)
